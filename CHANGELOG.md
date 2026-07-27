# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.1.0] - 2026-07-27

### Added
- **Motivation & Engagement** — 4 automatic mechanisms: progress feedback ("X/Y 完成"), escape hatch (every 2 stages), fatigue detection (short answers → slow down → propose break), curiosity hooks (counterintuitive / mystery / boundary, ~1/4 questions)
- **Within-stage Scaffold Fading** — Early (specific anchors) → Mid (direction) → Late (open challenges). Fading adjusts question setup, not answer.
- **Metacognitive Tags** — names learning techniques at end of stage (analogy, decomposition, productive failure, etc.). Max 1 per stage, in notes file.
- `docs/2026-07-27-teach-skill-v2.1-test-report.md` — 8-scenario test (38/38 checks passed, 0 regressions)

### Fixed
- Escape hatch no longer fires after stage 1 (too early, breaks flow)
- Curiosity hooks frequency linked to fatigue state (more hooks when fatigued, 1/4 when normal)
- Metacognitive tags now required even in fast-paced mode if a named technique was used

## [2.0.0] - 2026-07-27

### Breaking Change
- Complete rewrite: monolithic 24KB SKILL.md → modular 7.4KB core + 4 reference files

### Added
- **Adaptive Pacing** — 6 signal-based triggers (3 correct → compress, 2 wrong → escalate, "我懂了" → trust & check, etc.)
- **Learner Style Detection** — Stage 0 auto-detects fast-paced hands-on vs deep-understanding
- **Compressed Feynman Check** — "一句话总结" when understanding is obvious from answers
- **5 Questioning Types** — Clarifying / Probing / Connecting / Counter / Hypothetical strategy table
- **Spaced Review on Resume** — 1→2→4→8→16 day interval check for prior stages
- **No-Repeat Rule** — once a conclusion is confirmed, build on it; never re-establish known premises
- `references/anti-leak.md` — rationalization table, red flags, pre-question check (extracted from monolith)
- `references/stuck-recovery.md` — 6-level recovery + adaptive escalation table + end vs stuck signal discriminator (extracted + expanded)
- `references/enrichment.md` — derivable vs conventional classifier (extracted)
- `references/session-mgmt.md` — plan file format, notes generation, resume, spaced review (extracted + expanded)
- `docs/2026-07-26-socratic-tutor-sota-benchmark.md` — 30-paper literature review + 10 design dimensions
- `docs/2026-07-27-teach-skill-test-report.md` — 6-scenario role-play test (28/28 checks passed)

### Changed
- SKILL.md reduced from 24,338 bytes → 7,434 bytes (−69%)
- Graphviz `dot` flowchart → plain-text diagram (model-universal)
- Learner Style moved to Stage 0 (was a separate section)

### Removed
- 11-row rationalization table from SKILL.md (moved to `references/anti-leak.md`)
- 12 red-flag items from SKILL.md (moved to `references/anti-leak.md`)
- Full session-mgmt format templates from SKILL.md (moved to `references/session-mgmt.md`)

## [1.0.0] - 2026-07-26

### Initial Release
- Monolithic SKILL.md with: 6 Hard Rules, Feynman stage gates, Stuck-Recovery Kit, 高手点拨 enrichment, session wrap-up
- Core principle: 以已知推未知 (single-unknown derivation with premises-first)
- Givens-vs-construction anti-leak line
- Plan file + 学习笔记 persistence
