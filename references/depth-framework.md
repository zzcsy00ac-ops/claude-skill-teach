# Depth Framework (深度层级框架)

> 解决"只学皮毛"——当前 Feynman Check 只测 Bloom Understand(2/6)，SOLO Multistructural。
> 用户期望"系统掌握"= SOLO Relational/Extended Abstract，Bloom Apply→Create。
> 差 2 个 SOLO 层级，4 个 Bloom 层级。

## 三重深度框架

### SOLO Taxonomy (Biggs & Collis 1982)

| 层级 | 含义 | 学习者表现 |
|------|------|------------|
| Prestructural | 不懂 | 答非所问 |
| Unistructural | 只懂一点 | 只能说出一个要点 |
| **Multistructural** | 能罗列要点但无法整合 | ← **Feynman Check 天花板** |
| **Relational** | 能整合、比较、分析关系 | ← **用户期望下限** |
| **Extended Abstract** | 能迁移、泛化、创造 | ← **用户期望上限** |

### Bloom 修订版 (Anderson & Krathwohl 2001)

| 层级 | 含义 | 对应 SOLO |
|------|------|-----------|
| Remember | 记忆事实 | Unistructural |
| **Understand** | 理解、解释 | Multistructural |
| **Apply** | 应用到新情境 | Relational |
| **Analyze** | 分解、比较、组织 | Relational |
| Evaluate | 判断、批判 | Extended Abstract |
| **Create** | 设计、构造新事物 | Extended Abstract |

### Webb DOK (Depth of Knowledge)

| 层级 | 含义 |
|------|------|
| DOK 1 | 回忆与复述 |
| DOK 2 | 技能与概念 |
| **DOK 3** | 战略性思考 |
| **DOK 4** | 扩展性思考 |

## 深度目标声明（每个阶段）

在 Plan 的每个阶段声明三重目标：

```yaml
stage:
  solo_target: "Relational"            # 目标 SOLO 层级
  bloom_target: ["Apply", "Analyze"]   # 目标 Bloom 层级
  dok_target: 3                        # 目标 DOK 层级
```

**默认目标线：**
- 概念性主题：SOLO Relational / Bloom Apply+Analyze / DOK 3
- 实操性主题：SOLO Extended Abstract / Bloom Apply+Create / DOK 3-4

**Feynman Check 降级：** 从唯一评估标准降级为概念轨的**必要非充分条件**。通过 Feynman 只证明到达 Multistructural——还需要通过 transfer task 或 analysis check 才算到达声明目标。

## 双轨评估

| 轨道 | 测什么 | 框架 | 方法 |
|------|--------|------|------|
| **概念轨** | 能否解释/比较/分析 | Bloom Understand/Analyze + SOLO Relational | Feynman Check（必要非充分） |
| **应用轨** | 能否做出来/迁移/创造 | Bloom Apply/Create + SOLO EA + DOK 3-4 | Performance Task（新增） |

### Performance Task 设计原则（UbD Stage 2）

1. **开放性**——不是填空或选择题，是解决真实问题
2. **必须迁移**——用新情境，不是课上讲过的同一例子
3. **产出工件**——代码/方案/分析报告，可评判
4. **SOLO Extended Abstract 标准**——不只是"做对了"，而是"能在新情境中泛化"

**Performance Task 示例：**
- 学完 Go goroutine → "写一个 worker pool：3 个 worker 并发处理一个 channel 里的 10 个任务，收集结果。不参考教程。"
- 学完 DCF 估值 → "给三家公司的财务数据，判断哪家被低估。说明你的推理过程。"
- 学完 SQL JOIN → "给你两个表和查询需求，写出 JOIN 语句并解释为什么选 LEFT 而非 INNER。"

**Performance Task 通过标准：**
- ✅ 能独立完成（不参考教学时的例子）= SOLO Extended Abstract
- ✅ 能完成但需要提示 = SOLO Relational
- ❌ 无法完成 = 需要回到教学阶段补强

### 评分量规 Rubric (v3 新增)

为 Feynman Check 和 Performance Task 各设 3 级评分量规，减少评分者主观性（Messick 1995 构念无关方差；UbD Stage 2 要求"公平一致"）。

**Feynman Check Rubric:**

| 评分 | SOLO 层级 | 标准 |
|------|---------|------|
| 3 (EA) | Extended Abstract | 能解释 + 能迁移到新情境 + 能批判性分析边界条件 |
| 2 (Relational) | Relational | 能解释 + 能比较多种方法/视角的关系 |
| 1 (Multistructural) | Multistructural | 能列出要点但无法整合（通过门槛 = 1） |
| 0 | Prestructural | 无法解释 → 回教学阶段 |

**Performance Task Rubric:**

| 评分 | 标准 | 提示依赖 |
|------|------|---------|
| 3 (EA) | 独立完成 + 产出优于基本要求 + 有原创洞察 | 0 次提示 |
| 2 (Relational) | 独立完成 + 产出满足要求 | 0-1 次提示 |
| 1 (Multistructural) | 按 step 完成，缺乏整合 | 2-3 次提示 |
| 0 | 无法完成 | 3+ 次提示仍无法完成 |

**通过门槛:** Feynman ≥ 1 + Performance Task ≥ 2 才算 stage pass。

## 螺旋式深化（Bruner 1960 + Reigeluth Elaboration Theory）

不是线性走完一遍就结束，而是**三轮螺旋**：

### 第一轮：Epitome（概览）
- 每个阶段开头用一个问题建立"全景"："这个概念在整个主题版图中的位置是什么？"
- 学习者看到一个粗粒度但完整的心智模型
- SOLO 目标：Unistructural → Multistructural（知道有哪些部分）

### 第二轮：Elaboration（深入关系）
- 阶段内的 Socratic 教学 + Feynman Check + Coverage Gate
- SOLO 目标：Relational（理解部分之间的关系）
- Bloom 目标：Understand + Apply

### 第三轮：Extended Abstract（迁移创造）
- Performance Task + 跨阶段关联
- SOLO 目标：Extended Abstract
- Bloom 目标：Analyze + Create

### 螺旋回归（Spiral Return）

在主题学习的后期阶段（通常是最后 1-2 个阶段），主动回溯早期阶段的概念：

- "你还记得阶段 A 学的 X 吗？现在你学了 B 和 C，有没有发现 X 其实和它们有这种关系……"
- 这不是简单的复习——而是用新学知识**重新照亮**旧知识，形成更高层级的整合
- SOLO 目标从 Relational 提升到 Extended Abstract

**触发条件：** 当第二个阶段通过 Feynman 后，如果后续阶段与已通过阶段存在概念关联，在进入新阶段前进行一次 1-2 问的螺旋回归。

## 目标气候：从 Performance Goal 到 Mastery Goal

**Locke & Latham 目标设定理论（2002/2019）：**
- 具体+挑战性目标 > 模糊目标（"尽力而为"）
- Mastery Goal（掌握目标）> Performance Goal（表现目标）
- 掌握目标与学业成绩显著正相关（Locke & Latham 目标设定理论；具体元分析数据为设计假设，未经独立验证）

**操作性改变：**
- Plan 中每个阶段的 done-when 不再只是 "通过 Feynman"
- 增加 **stretch goal**："不参考资料独立完成 X"
- Feynman 是 checkpoint，不是终点——stretch goal 才是目标

## 全景先行（Reigeluth Epitome）

在 Plan 的第一个阶段开始前，用 1-2 个问题让学习者看到整个主题的全景：

- "在开始之前——你觉得学习 X 最终能让你做到什么？"
- 给学习者一个"终局画面"：学完这个主题，你将能够……

这不是教学——这是建立**学习方向感**，让后续每个阶段都知道自己在拼图的哪个位置。
