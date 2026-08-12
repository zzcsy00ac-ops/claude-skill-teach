# Mastery Confidence Score (掌握信心分)

> 解决评估报告 `[三方确认]` 核心瓶颈：二元 pass/fail 信息损失大，无法区分"Relational但信心不足"vs"Relational且信心高"。
> 基于 PFA 模型 (Pavlik, Cen & Koedinger 2009, AIED)、BKT (Corbett & Anderson 1995, UMUAI, 被引1351+)、
> 不对称更新 (Nature Scientific Reports 2025)。

## 为什么需要概率化

当前 Coverage Gate 和 Feynman Check 用二元判断（addressed/not, pass/fail）。BKT 文献证明：
- 同一个"pass"可能来自真懂（P(mastered)=0.95）或运气猜对（P(mastered)=0.45）
- 二元判断丢失这个关键信息 → 学习者模型 → 教学策略 → 课程序列全部降级
- 对标 BKT：每个知识点维护 P(mastered) ∈ [0,1]，每次交互后贝叶斯更新

## 简化方案：不对称启发式更新

不实现完整 BKT 4参数模型，用确定性公式在 prompt 中描述为规则（LLM 负责信号提取，公式负责概率计算）：

```
每个知识点维护: p_mastery ∈ [0, 1]（初始 0.15）

答对（含 Feynman pass）:  p_mastery += 0.35 × (1 - p_mastery)   # 上升快
答错（含 Recovery 触发）:  p_mastery -= 0.25 × p_mastery           # 下降慢
Recovery L1-L5 加权失败:  L1=0.5次, L2=0.7次, L3=1次, L4=1.5次, L5=2次
```

**不对称设计依据**: α⁺(0.35) > α⁻(0.25)——对未掌握内容高响应，对已掌握内容稳定 [Nature Scientific Reports 2025]。

**为什么不用 LLM 直接推理概率**: LLM 存在过度自信风险 (Xiong 2024)，确定性公式更可靠。LLM 只负责判断"答对了没有""用了哪级 Recovery"。

## 通过阈值

| Gate | 旧规则 | 新规则（v3） |
|------|--------|-------------|
| Coverage Gate（每 item） | addressed = 被触及 → pass | p_mastery > 0.65 → item pass |
| Feynman Check | correct + own words → pass | p_mastery > 0.70 + correct + own words |

阈值选择理由：Bloom 标准化测验 80%→对话场景 0.65（考虑 LLM 评估噪声）。

## 更新流程（每轮交互后）

```
1. 判断学习者回应类型:
   - 答对（独立推理正确）→ p_mastery += 0.35×(1-p_mastery)
   - 答对但用了 Recovery L1-L2 → p_mastery += 0.15×(1-p_mastery)（弱上升）
   - 答对但用了 Recovery L3-L5 → 不更新（提示太多不算掌握）
   - 答错但有合理推理 → 不更新，标记 partial
   - 盲猜/答错 → p_mastery -= 0.25×p_mastery

2. 写入 Learner Profile 的知识状态索引（p_mastery + successes + failures + last_practice）
3. Coverage Gate 检查时：所有 item 的 p_mastery > 0.65 才通过
```

## 在 Learner Profile 中的存储

```markdown
## 知识状态索引
| 概念 | p_mastery | 成功 | 失败 | 上次练习 | 前置依赖 | 复习到期 |
|------|-----------|------|------|----------|----------|----------|
| goroutine | 0.72 | 3 | 1 | 2026-08-10 | — | 2026-08-18 |
| channel | 0.85 | 4 | 0 | 2026-08-10 | goroutine | 2026-08-26 |
| context | 0.38 | 1 | 2 | 2026-08-10 | goroutine | 2026-08-12 |
```

## 长期方向

v3 使用确定性启发式公式。未来可升级为：
- BKT 贝叶斯更新（引入 P(T)/P(G)/P(S) 参数估计）
- LLM-native KT（NTKT-style，Norris et al. 2025 证明 LLM 可直接做知识追踪）
