# Session Management

## Plan File Format

**Filename rule:** lowercase topic, join words with single hyphens. `Python 装饰器` → `./python-装饰器-learning-plan.md`. CJK topics with no spaces stay unchanged: `量子叠加` → `./量子叠加-learning-plan.md`.

```markdown
# 学习计划：<topic>
- 目标：<this session's goal>
- 诊断基础：<what they already know, one line>
- 学习风格：<快节奏实操型 / 深度理解型>
- 专长等级：<Novice / Intermediate / Advanced>
- 创建：<date> · 最近更新：<date>

## 全景（Epitome）
<one line: 学完这个主题，你将能够……>

## 阶段
- [ ] A. <sub-goal>
  - 完成判定：<Feynman criterion>
  - 深度目标：SOLO <level> / Bloom <levels> / DOK <n>
  - stretch goal：<不参考资料独立完成 X>
- [ ] B. ...

## 状态
- T<n> | stage:<X> | turn_in_stage:<m> | turn_in_session:<k> | wrong:<q> | flags:<fatigue? hook?>

## 进度日志
- <date>：诊断完成，进入阶段 A
- <date>：阶段 A 通过 ✅。观察：<一句话：顺 / 卡在哪 / 误区>
```

## Notes File Format (结课收尾)

**Trigger:** either (a) last stage passes Feynman, or (b) learner says "今天先到这 / 下课 / 改天继续".

**Filename:** same derivation as plan, suffix `-学习笔记.md`.

```markdown
# 学习笔记：<topic>
- 目标：<goal> · 本节：<date>
- 覆盖：<passed stages>　·　剩余：<unchecked stages>

## 要点（本节你已掌握的核心）
- <earned conclusion — only Feynman-passed stages>

## 行家做法（本节引入的最佳实践）
- <idiom / best-practice shared via 高手点拨 + one-line why>
- (omit this section if no enrichment happened)

## 学习方法（本节用到的技巧）
- <technique name + one-line how you used it, e.g. "分解法：把MILP拆成目标函数和约束分别推导">
- (omit this section if no metacognitive tags were given)

## 复习建议（针对你）
- <from observation: where you stalled — re-derive it yourself>

## 进阶建议（下一步）
- 剩余阶段：<named, not solved>
- <follow-up topics, named + one-line why-relevant>
```

**In chat:** print 要点 + one-line pointer to the notes file. Don't paste the whole file.

**Append to 进度日志:** `- <date>：本次结束，已生成学习笔记（覆盖 <stages>；剩余 <stages>）。`

## Red Lines for Wrap-up

- Write exactly ONE notes file. All sections go in it.
- **Partial wrap-up must not leak.** 要点 states conclusions ONLY for Feynman-passed stages. For un-reached stages, 进阶建议 may NAME the next topic + hook, but must NOT give the method.
- **Observations go to the file, never chat.** Chat stays Socratic until wrap-up moment.

## Resume Procedure

If `/teach <topic>` runs and plan file exists:
1. Derive filename (same rule), read the plan file. **Read the last `## 状态` line first to restore counters** (turn_in_stage, turn_in_session, wrong count).
2. Recap where they left off (last checked stage + recent 观察).
3. Resume from next unchecked stage. No re-diagnosis unless they ask.
4. If they want to start over, reset on request.

## Spaced Review on Resume (NEW)

Before resuming new content, check prior stages:
1. Read 进度日志 for stages passed in previous sessions.
2. Apply 1→2→4→8→16 day interval from pass date.
3. If a stage is due for review (interval elapsed), run an **interleaved quick recall** (v3: mixing 2-3 concepts, not just one): "还记得 X 和 Y 吗？它们之间有什么联系？一句话说说你的理解。" Interleaving at review time compounds spacing + mixing effects (Brunmair & Richter 2019).
4. If quick recall fails → insert a brief review of that stage before new content.
5. If it passes → note in log, continue to new content.

This prevents the "learned and forgot" gap across sessions.

## Learner Profile Integration (NEW)

The Learner Profile (`./.teach-learner-profile.md`) is a cross-topic persistent layer that sits above plan files. See `references/learner-profile.md` for full format and flow.

**At session start:**
1. Read `./.teach-learner-profile.md` (create if missing).
2. If `/teach` with no topic → show learning journey, ask continue or new.
3. If `/teach <topic>` → run readiness check (prerequisites + due-for-review).

**At session end (wrap-up):**
1. Add/update topic row in 学习旅程 table.
2. Update 知识状态索引: for each concept taught, record SOLO mastery level + date.
3. Record API error corrections or notable observations.
4. Compute next review dates (1→2→4→8→16 day intervals from mastery date).

**During session:**
- When a stage passes, update the concept's mastery level in the profile (not just the plan file).
- If teaching a new concept that relates to a prior topic, note the cross-topic link.
