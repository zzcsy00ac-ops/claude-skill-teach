# Gamification & Daily Quest (RPG 技能树)

> 可选中枢。学习者明确表示喜欢游戏化（"RPG 技能树""XP 升级""BOSS 战"）时启用。
> 不启用时完全无感——不增加教学规则密度，不影响 Socratic 核心流程。
> XP 结算只在阶段边界（Feynman pass / Performance Task pass），不在每轮交互中结算。

## 启用判断

Stage 0 Frame 时检测信号：
- 学习者主动提及"升级""经验值""技能树""成就""打怪"等游戏化词汇 → 自动启用
- 学习者档案 `gamification: true` → 启用
- 否则不启用。可在任意时刻说"开启游戏化"激活。

启用时告知一句："已开启 RPG 模式 🎮 ——你将通过通关阶段获得 XP，升级解锁技能树。"

## XP 规则

| 事件 | XP | 说明 |
|------|-----|------|
| 阶段通过 Feynman Check | +50 XP | 概念轨通关 |
| 阶段通过 Performance Task (Depth Gate) | +100 XP | 应用轨通关 |
| 阶段双轨全通过 (Feynman + Performance) | +150 XP 合计 | 不重复加，上两行之和 |
| Coverage Gate 全部 item 达到 tier ≥ 掌握 | +20 XP | 补全覆盖完整性奖励 |
| Assembly Stage 通过 (BOSS 战) | +200 XP | 终局整合 |
| 连续 3 个阶段首试通过 (streak bonus) | +30 XP | 奖励持续表现 |
| Daily Quest 完成并提交 | +15 XP/题 | 课后练习 |

**XP 结算时机：** 仅在阶段通过、Coverage Gate 清空、Assembly 完成时结算。每轮交互不结算。

**XP 不扣减。** 答错不罚——学习中的错误是学习本身，不是惩罚理由。

## 等级曲线

| 等级 | 累计 XP | 称号 |
|------|---------|------|
| Lv 1 | 0 | 学徒 (Novice) |
| Lv 2 | 200 | 探索者 (Explorer) |
| Lv 3 | 400 | 实践者 (Practitioner) |
| Lv 4 | 600 | 分析师 (Analyst) |
| Lv 5 | 800 | 创造者 (Creator) |
| Lv 6 | 1000 | 专家 (Expert) |
| Lv 7 | 1200 | 大师 (Master) |
| Lv 8 | 1400 | 宗师 (Grandmaster) |
| Lv 9 | 1600 | 贤者 (Sage) |
| Lv 10 | 1800 | 传奇 (Legend) |

每级 200 XP，线性增长。升级时一句播报："⭐ Lv X 达成！称号：XX。下一个目标：Lv X+1（还需 Y XP）。"

## BOSS 战 = Assembly Stage

Assembly Stage 本身就是 BOSS 战。启用游戏化时：
- 在进入 Assembly 时播报："🐉 **BOSS 战开始** —— 组装你学过的所有技能，解决以下挑战。"
- 使用 Assembly 三模式之一（见 SKILL.md Assembly Stage），但在开场和结算时使用 BOSS 框架语言。
- BOSS 战通过 = Assembly 通过 = +200 XP。
- 失败可重试，不扣 XP。重试时降低难度或给 Recovery 提示。

## 进度条

Telegram 不渲染 LaTeX，使用纯文本进度条：

```
阶段进度: ▰▰▰▱▱ 3/5
当前 XP: 520 | Lv 3 实践者
下一级: 600 (还差 80 XP)
```

- 每完成一个阶段时更新
- 使用 Unicode 块字符：▰ (已通过) / ▱ (未通过)
- 总阶段数 = Plan 中的阶段数

### XP 结算播报格式（阶段通过时）

```
⚔️ 阶段 B 通过！
+50 XP (Feynman) + 100 XP (Performance Task) = +150 XP
当前: 520 XP | Lv 3 实践者
进度: ▰▰▱▱▱ 2/5
```

## Daily Quest (课后任务)

### 触发

Session wrap-up 时自动生成（仅游戏化模式下）。生成到 `~/.teach/<topic>-daily-quest.md`。

### 格式

```markdown
# Daily Quest: <topic>
日期: <date>
难度: ⭐⭐⭐ (基于当前等级 Lv X)
预计时长: 10-15 分钟

## 任务

### Q1 [基础] [阶段 A 回顾] (5 min)
<一道针对已学阶段的回顾题，要求学习者独立回答>
- 预期：能用一句话解释 X 的核心原理

### Q2 [应用] [阶段 B 迁移] (5 min)
<一道迁移应用题，需要在新情境中使用已学知识>
- 预期：能在新场景中正确使用 Y

### Q3 [挑战] [跨阶段整合] (5 min)
<一道需要整合多个阶段知识的综合题>
- 预期：能联系 A 和 B 的关系，给出结构化回答

## 提交方式
完成后在下一次 /teach 时展示你的答案，或在消息中直接回复。
每题完成 = +15 XP，全通 = 额外 +15 XP bonus。
```

### Daily Quest 设计原则

1. **3 道题**：基础回顾 / 应用迁移 / 跨阶段整合
2. **标注难度**：⭐ 基础 / ⭐⭐ 应用 / ⭐⭐⭐ 挑战
3. **标注对应阶段**：指明这道题考的是哪个阶段的知识
4. **标注预计时长**：总时长控制在 10-15 分钟
5. **不超纲**：只用已通过阶段的知识，不涉及未学内容
6. **可验证**：下次 `/teach` 时检查答案，结算 XP

### 与 Retrieval Practice 的关系

Daily Quest 是 Retrieval Practice 的课后延伸——间隔一天后回忆 + 应用。与 SKILL.md 的 Retrieval Practice 机制互补：
- 课内 Retrieval Practice：阶段转换时的 Free Recall / Cued Recall
- 课后 Daily Quest：跨阶段整合的迁移应用

## 持久化到 Learner Profile

游戏化数据存储在 learner-profile.md 中：

```markdown
## 游戏化状态
- 模式: 启用
- 总 XP: 520
- 等级: Lv 3 (实践者)
- BOSS 战记录: 0/1 通过
- Daily Quest 统计: 发出 3 次, 完成 2 次
- 连续完成 streak: 2
- 上次 Daily Quest: 2026-08-10
```

在 session wrap-up 时更新此节。

## 设计约束

1. **游戏化是增强层，不是教学层** — XP/等级只在阶段边界结算，不在每轮交互中增加认知负担
2. **不增加教学规则密度** — 游戏化播报最多 1-2 句，不喧宾夺主
3. **可随时关闭** — 学习者说"关掉游戏化"即停用，不再播报 XP/等级
4. **不影响评估严谨性** — Feynman/Coverage Gate/Depth Gate 的通过标准不变，XP 只是奖励层
5. **不惩罚错误** — 答错不扣 XP，学习中的错误是过程不是过失
