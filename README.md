# Teach — Socratic Tutor Skill

> **Never answers. Always asks.** A modular Socratic tutoring skill for Claude Code, Hermes Agent, Codex, Cursor, and any agent that supports `SKILL.md`.

**Version 2.1.0** · MIT License · 2026-07-27

---

## What It Does

`/teach <topic>` triggers a Socratic tutor that guides you to **construct every insight yourself** — through one-question-at-a-time questioning, Feynman checks, and adaptive pacing that responds to how fast you learn.

It doesn't give you the answer. It makes you derive it.

```
你: /teach Go goroutine和channel

导师: 你好！先问一下——你之前写过并发程序吗？Python threading 之类的？
你: 写过，理解 GIL 和锁。
导师: 好的，基础很扎实。你更喜欢先理解理论再动手，还是边做边学？
你: 边做边学，快点来。
导师: 收到，直接上。给你一个已知：Go 里 `go f()` 启动 goroutine，
      是用户态轻量协程。那你在 Python 里要让两个线程安全传数据，一般怎么做？
你: queue.Queue 或加锁共享变量。
导师: 对。Go 不靠共享内存+锁，而是靠 channel...
```

## Key Features

| Feature | Description |
|---|---|
| **以已知推未知** | Every question has exactly one unknown. All premises established first. Never force a guess. |
| **Adaptive Pacing** | 3 correct answers → compress. 2 wrong → escalate. "我懂了" → trust & quick-check. |
| **Compressed Feynman** | Full teach-back when needed; "一句话总结" when understanding is already obvious. |
| **Scaffold Fading** | Within each stage: specific anchors (Early) → direction (Mid) → open challenges (Late). |
| **Motivation Engine** | Progress feedback, escape hatch (every 2 stages), fatigue detection, curiosity hooks. |
| **6-Level Stuck Recovery** | Reframe → Analogy → Simplify → Decompose → Worked Example → Hint. Auto-escalates. |
| **高手点拨 (Enrichment)** | Post-mastery craft upgrade: derivable improvements are guided, conventions are shown. |
| **Metacognitive Tags** | Names learning techniques (analogy, decomposition, etc.) at end of stage — builds "how to learn" awareness. |
| **Learner Style Aware** | Detects fast-paced hands-on vs deep-understanding in Stage 0, adapts the whole session. |
| **Session Persistence** | Plan file (`*-learning-plan.md`) + notes file (`*-学习笔记.md`) + spaced review on resume. |
| **Anti-Leak Defense** | Rationalization table, red flags, pre-question check — givens are told, construction is yours. |

## Install

### Claude Code

```bash
cd ~/.claude/skills
git clone https://github.com/<your-username>/teach-skill.git teach
```

Or manually copy:

```bash
mkdir -p ~/.claude/skills/teach/references
cp skills/teach/SKILL.md ~/.claude/skills/teach/
cp skills/teach/references/*.md ~/.claude/skills/teach/references/
```

### Hermes Agent

```bash
cp -r skills/teach ~/.hermes/skills/productivity/teach
```

### Codex CLI

```bash
mkdir -p ~/.codex/skills/teach/references
cp skills/teach/SKILL.md ~/.codex/skills/teach/
cp skills/teach/references/*.md ~/.codex/skills/teach/references/
```

### Cursor

```bash
mkdir -p ~/.cursor/skills/teach/references
cp skills/teach/SKILL.md ~/.cursor/skills/teach/
cp skills/teach/references/*.md ~/.cursor/skills/teach/references/
```

## Usage

Just type `/teach` followed by your topic:

```
/teach Python 装饰器
/teach MILP optimization
/teach Go goroutine和channel
/teach 量子叠加
教我 React useEffect
我想学 Docker
讲解一下 Fourier transform
```

### Session Commands

| You say | What happens |
|---|---|
| `/teach X` (first time) | Frame goal → diagnose → write plan → start teaching |
| `/teach X` (plan exists) | Resume from next unchecked stage + spaced review |
| "今天先到这 / 下课" | Session wrap-up → generates `*-学习笔记.md` |
| "直接给代码 / 给我答案" | Recognized as **stuck signal** → Recovery Kit (NOT an exit) |

## How It Works

```
0. Frame goal + detect style  →  1. Diagnose baseline  →  2. Write learning plan (file)
                                                                    ↓
3. Teach one stage (one question per turn, premises first)
                                                                    ↓
                                                              Feynman check
                                                              ↙        ↘
                                                        fail: Recovery   pass: 高手点拨 → next stage
                                                                    ↓
                                                          All stages done → 生成学习笔记
```

## File Structure

```
teach-skill-repo/
├── README.md                          ← You are here
├── LICENSE                            ← MIT
├── CHANGELOG.md                       ← Version history
├── skills/
│   └── teach/
│       ├── SKILL.md                   ← Core rules (7.4KB, always loaded)
│       └── references/                ← Detail files (loaded on demand)
│           ├── anti-leak.md           ← Rationalization table + red flags + pre-question check
│           ├── stuck-recovery.md      ← 6-level recovery + 5 questioning types + signal table
│           ├── enrichment.md          ← 高手点拨: derivable vs conventional classifier
│           └── session-mgmt.md        ← Plan file format + notes + resume + spaced review
└── docs/
    ├── 2026-07-26-socratic-tutor-sota-benchmark.md   ← 30-paper literature review
    └── 2026-07-27-teach-skill-test-report.md         ← 6-scenario test report (28/28 passed)
```

## Design Principles

This skill is grounded in evidence-based learning science:

- **Vygotsky's ZPD** — questions calibrated to the edge of what you can do *with* support (the "one unknown" rule)
- **Feynman Technique** — explain in plain words to expose gaps (stage gate)
- **Scaffold Fading** (Wood, Bruner & Ross) — supports withdrawn as mastery grows
- **Productive Failure** (Kapur) — struggle before instruction builds schema
- **Cognitive Load Theory** (Sweller) — novices get worked examples, experts get problems
- **Bloom's 2-Sigma** — one-to-one Socratic tutoring targets the +2σ effect

See [`docs/2026-07-26-socratic-tutor-sota-benchmark.md`](docs/2026-07-26-socratic-tutor-sota-benchmark.md) for a full 30-paper literature review mapping these principles to the skill's design.

## Design Decisions

### Why modular (v2) instead of monolithic (v1)?

The v1 SKILL.md was 24KB — too large for reliable instruction-following on mid-tier models. Research shows LLM pedagogical failures rise from 17.7% to 77.8% across multi-turn dialogue (SafeTutors, arXiv:2603.17373). More rules ≠ better execution; it means more attention dilution.

**v2 solution:** 7.4KB core file (always loaded) + 4 reference files (loaded only when the relevant situation arises). Same rules, 69% smaller footprint per turn.

### Why "givens vs construction" instead of "never give answers"?

Pure "never tell" fails for rote knowledge (a date, a spelling, a function name) and for community conventions (`csv.DictReader` exists — you can't derive it). The line is: **givens (premises, skeletons, conventions) are told; the construction (derivation, working code) is the learner's.** See [`references/anti-leak.md`](skills/teach/references/anti-leak.md).

### Why adaptive pacing instead of pure Socratic?

Cognitive Load Theory shows pure discovery learning hurts novices (overload), while experts benefit from struggle. The skill auto-detects which mode to use via 6 signal triggers. See the Adaptive Pacing table in [`SKILL.md`](skills/teach/SKILL.md).

## Testing

6 role-play scenarios were simulated covering diverse learner types. All 28 checks passed:

| Scenario | Learner Type | Checks |
|---|---|---|
| Python 变量 | Complete beginner | 5/5 ✅ |
| Go goroutine | Experienced, fast-paced | 5/5 ✅ |
| 递归 | Gets stuck, "我不确定" | 4/4 ✅ |
| MILP 优化 | Demands "直接给代码" | 4/4 ✅ |
| 列表推导式 | Fast-paced hands-on | 5/5 ✅ |
| Session end | "今天先到这" | 5/5 ✅ |

Full report: [`docs/2026-07-27-teach-skill-test-report.md`](docs/2026-07-27-teach-skill-test-report.md)

## Comparison with Similar Skills

| Skill | Approach | Key Difference |
|---|---|---|
| **This — Teach** | Socratic questioning + adaptive pacing + modular | Adaptive to learner speed; compressed Feynman; givens-vs-construction line |
| [Li-Evan/Bloom](https://github.com/Li-Evan/Bloom) | Document-driven adaptive courses | Learner reads docs → feedback → next doc. Less interactive, more self-paced. |
| [bevibing/socrates-skill](https://github.com/bevibing/socrates-skill) | Minimal Socratic (~1500 words) | Simplest possible. No staging, no persistence, no recovery. |
| [DrCatHicks/learning-opportunities](https://github.com/DrCatHicks/learning-opportunities) | In-codebase deliberate practice | Triggers during coding, not standalone learning sessions. |
| [flysheep-ai/education-skills](https://github.com/flysheep-ai/education-skills) | Chinese 高考 subject tutors | Subject-specific (math/physics/chem). Not general-purpose. |

## Compatibility

| Platform | Status | Path |
|---|---|---|
| Claude Code | ✅ | `~/.claude/skills/teach/` |
| Hermes Agent | ✅ | `~/.hermes/skills/productivity/teach/` |
| Codex CLI | ✅ | `~/.codex/skills/teach/` |
| Cursor | ✅ | `~/.cursor/skills/teach/` |
| OpenCode | ✅ | `~/.opencode/skills/teach/` |

Any agent that loads `SKILL.md` with YAML frontmatter and supports `references/` subdirectories is compatible.

## Roadmap

- [ ] Multi-session progress dashboard (aggregate across topics)
- [ ] Vocabulary-specific extension (integrate with SRS pipelines)
- [ ] Code execution sandbox for real-time coding exercises
- [ ] Multi-language UI (English / 中文)

## Contributing

Contributions welcome. Please run through the 6 test scenarios in `docs/` before submitting changes.

## License

MIT — see [LICENSE](LICENSE).

## Acknowledgments

- [Li-Evan/Bloom](https://github.com/Li-Evan/Bloom) — modular skill structure + Feynman skill inspiration
- [bevibing/socrates-skill](https://github.com/bevibing/socrates-skill) — 5 questioning types
- [DrCatHicks/learning-opportunities](https://github.com/DrCatHicks/learning-opportunities) — scaffold fading pattern
- 30 academic papers in `docs/2026-07-26-socratic-tutor-sota-benchmark.md`
