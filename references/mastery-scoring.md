# Mastery Confidence Score (掌握信心分)

> v3 版本：LLM 不做浮点心算。用离散 streak + 三档 tier 替代连续 p_mastery。
> 理论依据不变：PFA 模型 (Pavlik, Cen & Koedinger 2009, AIED)、BKT (Corbett & Anderson 1995, UMUAI)、
> 不对称更新（设计假设，未经独立验证）。

## 为什么需要概率化

Coverage Gate 和 Feynman Check 不能仅用二元 pass/fail——同一个"pass"可能来自真懂或运气猜对。
用 streak（连续答对次数）+ tier（档位）替代浮点概率，LLM 只需计数，不做算术。

## 三档方案

每个知识点维护两个值：
- **streak**：连续独立答对次数（整数 + 半整数，初始 0）
- **tier**：掌握档位（未掌握 / 掌握 / 巩固）

### 档位与 streak 的映射

| 档位 | streak 范围 | 语义 | 对应旧 p_mastery |
|------|------------|------|-----------------|
| 未掌握 | 0 - 2.5 | 仍在学习 | < 0.65 |
| 掌握 | 3 - 4.5 | 能独立解释 | 0.65 - 0.85 |
| 巩固 | ≥ 5 | 稳固、可迁移 | ≥ 0.85 |

### 迁移规则

| 当前档位 | 事件 | 迁移 | streak 变化 |
|----------|------|------|------------|
| 未掌握 | streak ≥ 3 | → 掌握 | 不重置 |
| 掌握 | streak ≥ 5 | → 巩固 | 不重置 |
| 巩固 | 答错 1 次 | → 掌握（降一级） | 归零 |
| 掌握 | 答错 1 次 | → 未掌握（降一级） | 归零 |
| 未掌握 | 答错 1 次 | 留在未掌握 | 归零 |

**降档永远只降一级，不跳级。**

**不对称设计说明：** 升档需 3 次连续答对，降档只需 1 次答错——但降档只降一级。
保留原公式 α⁺(0.35) > α⁻(0.25) 的精神：上升快、下降慢。

### Recovery 辅助折算

| 辅助级别 | streak 更新 |
|---------|------------|
| 无辅助（独立正确） | +1 |
| Recovery L1-L2 辅助后正确 | +0.5（两次辅助答对 = 一次独立答对） |
| Recovery L3-L5 辅助后正确 | +0（提示太多不算掌握证据） |
| 答错 / 盲猜 | 归零 + 降档 |

### compress 模式例外（详见 SKILL.md Adaptive Pacing）

当 compress 信号已触发（Advanced 学习者 / 连续答对 3+ stages / 学习者要求加速）：
- item 掌握门槛降为：**1 次独立答对 = 当场 tier = 掌握**（无需 streak ≥ 3）
- 理由：compress 信号本身是强掌握证据，单次正确的先验概率远高于普通学习者
- 限制：仍不可跳过 Feynman（可 compressed 形式）；不可直接升到巩固

## 通过阈值

| Gate | 通过条件 |
|------|---------|
| Coverage Gate（每 item） | tier ≥ 掌握 |
| Feynman Check | Coverage Gate 已通过 + Feynman 回答正确且用自己的话 |

> Feynman teach-back 本身计为 +1 streak（判定前更新）。
> 旧版 p_mastery > 0.65 / > 0.70 双阈值已废弃。

## 更新流程（每轮交互后）

```
1. 判断回应类型，更新 streak（见 Recovery 辅助折算表）
2. 如答错：streak 归零，触发降档（只降一级）
3. 如答对：streak 增加，检查是否触发升档
4. 写入 Learner Profile（streak + tier + successes + failures + last_practice）
5. Coverage Gate 检查：所有 item tier ≥ 掌握？
```

## 在 Learner Profile 中的存储

```markdown
## 知识状态索引
| 概念 | tier | streak | 成功 | 失败 | 上次练习 | 前置依赖 | 复习到期 |
|------|------|--------|------|------|----------|----------|----------|
| goroutine | 掌握 | 3.5 | 4 | 1 | 2026-08-10 | — | 2026-08-18 |
| channel | 巩固 | 6 | 6 | 0 | 2026-08-10 | goroutine | 2026-08-26 |
| context | 未掌握 | 1 | 1 | 2 | 2026-08-10 | goroutine | 2026-08-12 |
```

## 长期方向

未来可升级为 BKT 贝叶斯更新或 LLM-native KT。
当前离散化方案是 LLM 可靠执行的最大精度。
