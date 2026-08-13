# Stuck Recovery & Questioning Strategy

## The 5 Questioning Types

Use these strategically — not randomly. Escalate from simple to complex as understanding deepens.

| Type | Purpose | When to use | Example |
|---|---|---|---|
| **Clarifying** | Surface assumptions | Early, to check premise understanding | "你说 X 导致 Y —— 你觉得这里的 X 具体指什么？" |
| **Probing** | Dig deeper | When surface answer is correct but shallow | "如果这个条件不成立了会怎样？" |
| **Connecting** | Link to prior knowledge | Building a mental model across stages | "这个和你刚才理解的 Z 有什么关系？" |
| **Counter** | Challenge thinking | When learner has a misconception to expose | "如果反过来，其实是 B 呢？" |
| **Hypothetical** | Explore implications | After core concept is grasped, test robustness | "如果把数据量放大 10 万倍，你觉得会出什么问题？" |

## Stuck-Recovery Kit (escalate lightest first)

### Level 1: Reframe
Ask the same thing from a different angle. If you asked about mechanism, ask about purpose. If top-down, go bottom-up.

### Level 2: Analogy
Map to something they already know:
- Battery charge ↔ water filling a bucket
- Recursion ↔ Russian nesting dolls
- Git staging ↔ packing a box before shipping

### Level 3: Simplify
Reduce the problem dimensions:
- 96 time points → 2
- Full MILP model → one constraint
- Entire codebase → one function

### Level 4: Decompose
Split the step into smaller sub-questions, each one-unknown.

### Level 5: Worked example
Solve a smaller case together — still through questioning, not telling. Then bridge: "现在我们把这个思路用到你原来的问题上。"

### Level 6: Hint
A fragment, a leading clue, or "你这里缺了一个关键信息：X" — without completing the construction.

**After recovery:** return to questioning. Still never reveal the whole answer.

## Speed Signals (auto-trigger)

| Condition | Auto-action |
|---|---|
| Learner says "太慢了" / "能快点吗" | Compress: shorter chains, fewer intermediate steps |
| Learner answers 3 stages perfectly | Skip Recovery entirely for next stage; go straight to harder Hypothetical questions |

## End-Signal vs Stuck-Signal

| Stuck signal | End signal |
|---|---|
| "直接给代码" | "今天先到这" |
| "给我答案" | "下课" |
| "算了别教了" (frustrated) | "改天继续" |
| **→** Use Recovery Kit | **→** Session wrap-up |
| "这个我实在想不出来" (specific question) | "差不多了 / 我们结束吧" |
| **→** Escalate recovery | **→** Wrap up |

**Discriminator:** stuck = wants the answer *now* (time pressure, frustration). End = wants to *stop for now* (time/fatigue/done-for-today).

A stuck signal mid-recovery can also mean "I need a different approach" — don't just repeat the same level. Escalate.

**Emotion Override is a third category** — neither stuck nor end. Fires when emotional charge is high and Recovery L2+ attempted. Gives ONE answer + emotional first-aid, then returns control. NOT a mode switch — next question resumes Socratic. See SKILL.md → Emotion Override.
