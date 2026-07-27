# Post-Mastery Enrichment (高手点拨)

## When to Fire

**ALL three must hold:**
1. **Solid Feynman pass** (not a bare/struggling one)
2. **Correct artifact** produced by the learner
3. **Correct-but-suboptimal** — a real craftsmanship gap exists

If any condition fails → skip enrichment, check off the stage.

## Classifier

For each improvement you could offer, ask: *Could the learner reason their way to this given the right stress-test — or is it arbitrary knowledge no reasoning surfaces?*

### Derivable → Guide
Reuse single-unknown questioning. Only the target shifts: from "make it work" to "make it good."

- **Stress-test:** "你的版本 100 个元素没问题——10 万个会先在哪一步变慢？" → they derive the bottleneck → reach for a better structure themselves.
- **Generalize:** "你把阈值写死了——用户想换一个呢？" → they parameterize.
- Nothing shown. The elegant form is still earned.

### Conventional → Show (contrast walkthrough)

> **Note:** This "conventional → show" rule applies not only post-mastery, but ALSO mid-teaching when the learner asks an operation question ("怎么做") about conventional knowledge. See SKILL.md Rule 1 (Concept vs Operation). The mid-teaching version is simpler: tell + one-line why, then return to the concept flow.

The learner can't derive `csv.DictReader`, `functools.wraps`, `pathlib`, `@dataclass`, or "log it, don't print." Show them:

1. **Affirm** their version is correct — the construction was the point.
2. **Name** what exists: "有个标准库工具就是干这个的：`csv.DictReader`。"
3. **Show** the idiomatic form, side by side.
4. **Walk through why** practitioners reach for it — the footgun it avoids.
5. **Frame honestly:** "这是社区约定，你推不出来、只能被告知；原理你已经搭通了。"

Their original work is never "wrong, discard it" — it's "yours proves you understand; this is the pro version."

### Mixed improvements
(e.g. `enumerate()` — partly derivable "don't track index by hand", partly conventional "it's called `enumerate`"): handle each part by its nature — name the tool, let them apply, or show if trivial.

## Log it

When enrichment fires, the stage's 观察 records the delta:
- `阶段 B：自己写出嵌套循环去重，点拨后用 set 优化为 O(n)。`
- `阶段 A：手写 CSV 解析，点拨引入 csv.DictReader。`

## What NOT to do

- Enrich before Feynman passes (pre-construction leak)
- Enrich when artifact is already idiomatic (manufactured gap)
- Show a derivable improvement instead of guiding it (skipping second construction)
- Dump best practices onto a shaky pass (overwhelm)
- Show without the contrast walkthrough — bare "here's the pro version" teaches nothing
