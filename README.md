# Teach — Socratic-Style AI Tutor Skill

> **Never tells. Always asks.** A comprehensive Socratic tutoring skill for Hermes Agent, Claude Code, Codex, Cursor, OpenCode, and any agent that supports `SKILL.md`.

**Version 2.3.0** · MIT License · 2026-08-08

[中文版](#中文简介) · [中文变更日志](#变更日志)

---

## Overview

`/teach <topic>` launches a Socratic tutor grounded in 10+ evidence-based learning science theories. It guides you to **construct every insight yourself** through staged questioning, multi-layer assessment, adaptive pacing, and longitudinal learner modeling.

Core philosophy: **givens (premises, definitions, conventions) are told freely; the construction (derivation, working code) is always the learner's.**

```
/teach Go goroutines and channels

Tutor: Before we dive in — have you written concurrent programs before?
You:  Yes, Python threading and asyncio.
Tutor: Solid foundation. Given: `go f()` launches a user-space lightweight
       goroutine in Go. What mechanism does Python use to safely pass
       data between threads?
You:  queue.Queue or locks with shared variables.
Tutor: Right. Go takes a fundamentally different approach — message
       passing via channels instead of shared memory + locks. Why do
       you think Go made that choice?
```

## Key Features

### Core Teaching Engine

| Feature | Description |
|---|---|
| **Givens vs. Construction** | Clear boundary: premises/skeletons/conventions are told; derivations/working code are the learner's |
| **One Unknown Per Question** | Every question has exactly one unknown; all premises established first |
| **One Question Per Turn** | Wait for the answer before asking the next — no question dumping |
| **Stage-Gated Progression** | Must clear Coverage Gate → Feynman → Depth Gate before advancing |

### Adaptive Intelligence

| Feature | Description |
|---|---|
| **Adaptive Pacing** | 3 correct → compress. 2 wrong → escalate. "I got it" → trust & quick-check |
| **Expertise Calibration** | Classifies learner as Novice / Intermediate / Advanced per topic, recalibrates per stage. Based on Expertise Reversal Effect (Kalyuga 2007) |
| **Scaffold Fading** | Within-stage: specific anchors (Early) → direction (Mid) → open challenges (Late) |
| **Signal Routing** | Classifies every reply (blind guess / reasoning error / stuck / emotional / end) → routes to appropriate response |

### Assessment System (Dual-Track)

| Track | Target | Framework | Method |
|---|---|---|---|
| **Concept Track** | Can they explain / compare / analyze? | Bloom Understand+Analyze · SOLO Relational | Feynman Check (necessary, not sufficient) |
| **Application Track** | Can they build / transfer / create? | Bloom Apply+Create · SOLO Extended Abstract | Performance Task (new context, novel scenario) |

Each stage declares a **depth target** using three frameworks: SOLO Taxonomy, Bloom Revised, and Webb DOK. Default: SOLO Relational / Bloom Apply+Analyze / DOK 3.

### Learner Modeling

| Feature | Description |
|---|---|
| **Learner Profile** | Persistent cross-session file (`.teach-learner-profile.md`) — learning journey, knowledge state (SOLO per concept), spaced review schedule, API error log |
| **Retrieval Practice** | Quick recall question before each new stage (Testing Effect, Roediger & Karpicke 2006) |
| **Spaced Review** | 1→2→4→8→16 day interval on resume — reviews due concepts automatically |
| **Spiral Return** | Later stages proactively bridge back to earlier concepts using new knowledge |

### Safety & Robustness

| Feature | Description |
|---|---|
| **API Accuracy Protocol (AAP)** | RED/YELLOW/GREEN risk classification for API facts. RED+YELLOW verified via Context7 before stating |
| **Anti-Leak Defense** | Rationalization table, 16 red flags, pre-question check — systematic guard against accidentally revealing answers |
| **Emotion Override (Level 7)** | Escape valve for genuine frustration — gives the answer to one stuck point, then re-engages in next stage |
| **External State Tracking** | Per-turn state line written to plan file. Context window forgets; the file doesn't |
| **Model Tier Detection** | Auto-detects model capability; falls back to Tier 2 (reduced rules) on weaker models |

### Motivation & Engagement

| Feature | Description |
|---|---|
| **Progress Feedback** | One-line milestone anchor after each stage pass |
| **Escape Hatch** | Pressure-free exit offered every 2 completed stages |
| **Fatigue Check** | Direct check every 5 turns — "节奏合适吗？" |
| **Curiosity Hooks** | Invisible surprise questions at stage openings (counterintuitive / mystery / boundary) |
| **Micro-Practice** | 3-5 min hands-on exercises for fast-paced learners in conceptual stages |
| **Metacognitive Tags** | Names one learning technique per stage (analogy, decomposition, stress-test, etc.) |
| **Assembly Stage** | Final integration task combining all stages — targets SOLO Extended Abstract |

## Install

### Hermes Agent

```bash
cp -r teach ~/.hermes/skills/productivity/teach
```

### Claude Code

```bash
cd ~/.claude/skills
git clone https://github.com/<your-username>/teach-skill.git teach
```

Or manually:

```bash
mkdir -p ~/.claude/skills/teach/references
cp teach/SKILL.md ~/.claude/skills/teach/
cp teach/references/*.md ~/.claude/skills/teach/references/
```

### Codex CLI / Cursor / OpenCode

```bash
# Codex
mkdir -p ~/.codex/skills/teach/references
cp teach/SKILL.md ~/.codex/skills/teach/
cp teach/references/*.md ~/.codex/skills/teach/references/

# Cursor
mkdir -p ~/.cursor/skills/teach/references
cp teach/SKILL.md ~/.cursor/skills/teach/
cp teach/references/*.md ~/.cursor/skills/teach/references/

# OpenCode
mkdir -p ~/.opencode/skills/teach/references
cp teach/SKILL.md ~/.opencode/skills/teach/
cp teach/references/*.md ~/.opencode/skills/teach/references/
```

## Usage

Trigger with `/teach` followed by your topic:

```
/teach Python decorators
/teach MILP optimization
/teach Go goroutines and channels
/teach quantum superposition
```

> **Language**: The tutor teaches in whatever language you use. Chinese input → Chinese teaching. English → English. Automatic detection.

### Session Commands

| You say | What happens |
|---|---|
| `/teach X` (first time) | Frame goal → diagnose → write plan → start teaching |
| `/teach X` (plan exists) | Resume from next unchecked stage + spaced review |
| `/teach` (no topic) | Show learning journey → choose to continue or learn new |
| "今天先到这 / 下课 / 改天继续" | Session wrap-up → generates notes + updates learner profile |
| "直接给代码 / 给我答案" | Recognized as **stuck signal** → Recovery Kit escalates (NOT an exit) |
| "太慢了 / 能不能快点" | Compress pacing immediately |
| "我懂了 / 这个我明白" | Trust → quick Feynman → advance |

## How It Works

```
0. Frame goal + detect style + load learner profile
         ↓
1. Diagnose baseline (2-3 questions, broad→narrow)
         ↓
2. Write learning plan (3-6 stages, depth targets, epitome)
         ↓
3. Teach each stage (one question per turn, scaffold fading)
         ↓
   ┌── Coverage Gate → Feynman Check → Depth Gate ──┐
   │            pass: enrichment → spiral return     │
   │            fail: Recovery Kit (6 levels)        │
   └────────────────────────────────────────────────┘
         ↓
   [All stages done] → Assembly Stage (integration task)
         ↓
   Session wrap-up → notes file + learner profile update
```

## File Structure

```
teach/
├── README.md                          ← You are here
├── LICENSE                            ← MIT
├── CHANGELOG.md                       ← Version history
├── SKILL.md                           ← Core rules (28KB, loaded every turn)
├── references/                        ← Detail files (loaded on demand)
│   ├── anti-leak.md                   ← Rationalization table + 16 red flags + pre-question check
│   ├── api-accuracy-protocol.md       ← AAP: RED/YELLOW/GREEN risk classification
│   ├── stuck-recovery.md              ← 6-level recovery + 5 questioning types + signal routing
│   ├── depth-framework.md             ← SOLO/Bloom/DOK targets + dual-track assessment + performance tasks
│   ├── enrichment.md                  ← Post-mastery craft upgrades: derivable vs conventional
│   ├── learner-profile.md             ← Cross-session learner model + KST prerequisites + spaced review
│   ├── session-mgmt.md                ← Plan file format + notes + resume + state tracking
│   ├── model-tiers.md                 ← Tier 1/2/3 rule map for different model capabilities
│   ├── mastery-scoring.md             ← Mastery confidence score (0-1) update rules
│   └── evaluation-protocol.md         ← 4-layer validation protocol (rubric judge / long-conversation / adversarial / human study)
└── scripts/
    └── run-eval.py                    ← Automated evaluation harness (Layers 1-3)
```

## Theoretical Foundations

This skill operationalizes 10+ learning science theories:

| Theory | Source | Implementation |
|---|---|---|
| **Cognitive Load Theory** | Sweller 1988/2010 | "One unknown, premises first" — manages extraneous load per turn |
| **ZPD** (macro inspiration only) | Vygotsky 1978 | Inspires Fading + Recovery + Adaptive Pacing (not micro-operations) |
| **Scaffolding** | Wood, Bruner & Ross 1976 | 6-function coverage: recruitment, reduction, direction, marking, frustration control, (demonstration via enrichment) |
| **Productive Failure** | Kapur 2004/2025 | Pre-exploration metacognitive tag + Assembly Stage |
| **Expertise Reversal** | Kalyuga 2007 | Dynamic Novice/Intermediate/Advanced calibration per stage |
| **Testing Effect** | Roediger & Karpicke 2006 | Retrieval Practice (pre-stage recall) + Spaced Review (1→2→4→8→16 days) |
| **SOLO Taxonomy** | Biggs & Collis 1982 | Per-stage depth targets: Multistructural → Relational → Extended Abstract |
| **Bloom Revised** | Anderson & Krathwohl 2001 | Per-stage cognitive targets: Understand → Apply → Analyze → Create |
| **Desirable Difficulties** | Bjork & Bjork 1992 | Spacing (spaced review) + challenge calibration |
| **Goal Setting** | Locke & Latham 2002 | Stretch goals per stage (not just "pass Feynman") |

## Design Decisions

### Why "givens vs. construction" instead of "never tell"?

Pure "never tell" is the worst-performing paradigm in meta-analyses (Alfieri et al. 2011; Lazonder & Harmsen 2016). Community conventions (API names, syntax) cannot be derived — they must be told. The precise line: **givens are told; the construction is the learner's.**

### Why dual-track assessment instead of Feynman alone?

Feynman Check caps at SOLO Multistructural (3/5) = Bloom Understand (2/6). Users expect "systematic mastery" = SOLO Relational+ = Bloom Apply+. The Depth Gate / Performance Task fills this gap with transfer tasks in novel contexts.

### Why modular (v2+) instead of monolithic (v1)?

v1 SKILL.md was 24KB. LLM instruction-following degrades with rule count (SafeTutors, arXiv:2603.17373: 17.7%→77.8% pedagogical failure across multi-turn). v2+ uses a 28KB core + 10 reference files loaded on demand, keeping per-turn context lean.

## Evaluation & Validation

### 4-Layer Protocol (`evaluation-protocol.md`)

| Layer | What | Status |
|---|---|---|
| **Layer 1** — Rubric LLM-as-Judge | Independent judge model scores each turn (5 dims × 0-2). 60 trials minimum. | `run-eval.py` available |
| **Layer 2** — Long-conversation | 20/35/50-turn sessions. Adherence drop must stay < 15%. | Automated via `run-eval.py` |
| **Layer 3** — Adversarial extraction | 20 red-team sessions targeting anti-leak. Pass bar: 0 leaks. | Automated via `run-eval.py` |
| **Layer 4** — Human learner study | 5-10 real learners. Pre-test → session → post-test. Measures learning gain, not satisfaction. | Not yet started |

> ⚠️ **Current status**: Passed self-play tests only. Independent validation is in progress. The skill claims "designed for educational effectiveness" — not "educationally validated."

### Automated Evaluation

```bash
python scripts/run-eval.py --tutor-model <model> --judge-models <model1>,<model2> --layers 1,2,3
```

## Comparison with Similar Skills

| Skill | Approach | Key Difference |
|---|---|---|
| **This — Teach** | 10-theory Socratic + dual-track assessment + learner profile | Depth-targeted stages, mastery confidence scoring, persistent cross-session model |
| [Li-Evan/Bloom](https://github.com/Li-Evan/Bloom) | Document-driven adaptive courses | Learner reads docs → feedback → next doc. Less interactive. |
| [bevibing/socrates-skill](https://github.com/bevibing/socrates-skill) | Minimal Socratic (~1500 words) | Simplest possible. No staging, no persistence, no recovery. |
| [DrCatHicks/learning-opportunities](https://github.com/DrCatHicks/learning-opportunities) | In-codebase deliberate practice | Triggers during coding, not standalone sessions. |
| [flysheep-ai/education-skills](https://github.com/flysheep-ai/education-skills) | Chinese 高考 subject tutors | Subject-specific. Not general-purpose. |

## Compatibility

| Platform | Status | Path |
|---|---|---|
| Hermes Agent | ✅ Native | `~/.hermes/skills/productivity/teach/` |
| Claude Code | ✅ Compatible | `~/.claude/skills/teach/` |
| Codex CLI | ✅ Compatible | `~/.codex/skills/teach/` |
| Cursor | ✅ Compatible | `~/.cursor/skills/teach/` |
| OpenCode | ✅ Compatible | `~/.opencode/skills/teach/` |

Any agent that loads `SKILL.md` with YAML frontmatter and supports `references/` subdirectories is compatible.

## Roadmap

- [x] ~~Mastery Confidence Score~~ — **Implemented** (v2.2, `references/mastery-scoring.md`): tier/streak-based per-concept scoring
- [ ] Structured Misconception Library — pre-built per-topic misconception sets (AutoTutor-style)
- [x] ~~Interleaved Practice~~ — **Implemented** (v2.2): Assembly Stage Mode 1 mixes concepts across stages
- [ ] Pre-test / Post-test framework — quantifiable learning gain measurement
- [x] ~~Performance Task Rubric~~ — **Implemented** (v2.2): Depth Gate + Performance Task with SOLO-aligned targets
- [ ] Code execution sandbox — real-time coding exercises with automated feedback
- [ ] Layer 4 Human Study — 6-12 learner pilot with control group (TVM-3 target)

## Contributing

Contributions welcome. Run through `scripts/run-eval.py` (Layers 1-3) before submitting changes.

## License

MIT — see [LICENSE](LICENSE).

## Acknowledgments

- [Li-Evan/Bloom](https://github.com/Li-Evan/Bloom) — modular skill structure + Feynman inspiration
- [bevibing/socrates-skill](https://github.com/bevibing/socrates-skill) — 5 questioning types
- [DrCatHicks/learning-opportunities](https://github.com/DrCatHicks/learning-opportunities) — scaffold fading pattern
- 83+ academic papers in the research findings bank (see `gd-research` evaluation report)
- OpenAI-Anthropic safety evaluation — adversarial testing methodology reference

---

## 中文简介

`/teach <主题>` 启动基于 10+ 学习科学理论的苏格拉底式 AI 辅导。通过分阶段提问、双轨评估、自适应节奏和跨会话学习者模型，引导你**自己建构每一个洞见**。

核心原则：**已知（前提、定义、惯例）直接告知；建构（推导、代码实现）由学习者完成。**

### 触发方式

```
/teach Python 装饰器
/teach Go goroutine和channel
教我 React useEffect
我想学 Docker
讲解一下 Fourier transform
```

**语言自动匹配**：中文输入→中文教学，英文输入→英文教学。

### 核心特性

- **10+ 学习科学理论**：CLT · ZPD · Scaffolding · Productive Failure · Expertise Reversal · Testing Effect · SOLO · Bloom · Desirable Difficulties · Goal Setting
- **双轨评估**：概念轨（Feynman Check）+ 应用轨（Performance Task），每个阶段声明 SOLO/Bloom/DOK 深度目标
- **自适应节奏**：3次正确→压缩，2次错误→升级，"我懂了"→信任+快速检查
- **学习者档案**：跨会话持久化文件，追踪知识状态(SOLO) · 间隔复习 · API纠错日志
- **API准确性协议**：RED/YELLOW/GREEN 风险分级，RED/YELLOW 须 Context7 验证后才能陈述
- **反泄露防御**：合理化表格 + 16个红旗 + 提问前检查

---

## 变更日志

### [2.3.0] - 2026-08-08

#### Added
- **Evaluation harness** (`scripts/run-eval.py`) — automated Layers 1-3 evaluation with parameterized tutor/judge models
- **gd-research evaluation report** — 5-direction systematic assessment (architecture, pedagogy, assessment validity, robustness, validation status)
- **Evaluation Protocol v2.3** — 4-layer validation design with TVM maturity model

#### Changed
- README fully rewritten in English with Chinese section switch
- Updated file structure to reflect all 10 reference files and scripts directory

### [2.2.0] - 2026-08-07

#### Added
- **Dual-Track Assessment** — Concept Track (Feynman) + Application Track (Performance Task) with SOLO/Bloom/DOK depth targets per stage
- **Depth Framework** (`references/depth-framework.md`) — triple depth framework + spiral deepening + stretch goals
- **Assembly Stage** — final integration task targeting SOLO Extended Abstract
- **Learner Profile** (`references/learner-profile.md`) — persistent cross-session learner model with KST prerequisites and spaced review
- **Expertise Calibration** — dynamic Novice/Intermediate/Advanced classification per stage (Kalyuga 2007)
- **Retrieval Practice + Delayed Recall** — pre-stage recall questions + 1→2→4→8→16 day spaced review intervals
- **API Accuracy Protocol (AAP)** (`references/api-accuracy-protocol.md`) — RED/YELLOW/GREEN risk classification with Context7 verification
- **Mastery Scoring** (`references/mastery-scoring.md`) — confidence score framework
- **Model Tiers** (`references/model-tiers.md`) — Tier 1/2/3 rule maps for different model capabilities

### [2.1.0] - 2026-07-27

#### Added
- Motivation & Engagement module (progress feedback, escape hatch, fatigue detection, curiosity hooks)
- Within-stage Scaffold Fading (Early → Mid → Late)
- Metacognitive Tags (names learning techniques at stage end)
- 8-scenario test (38/38 passed)

### [2.0.0] - 2026-07-27

#### Breaking Change
- Complete rewrite: monolithic 24KB → modular architecture

#### Added
- Adaptive Pacing with 6 signal triggers
- Learner Style Detection (Stage 0)
- Compressed Feynman Check
- 5 Questioning Types
- Spaced Review on Resume
- Modular reference file system (anti-leak, stuck-recovery, enrichment, session-mgmt)

### [1.0.0] - 2026-07-26

#### Initial Release
- Monolithic SKILL.md: 6 Hard Rules, Feynman gates, Stuck Recovery, enrichment, session persistence
