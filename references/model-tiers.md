# Model Tier Rule Map

This skill contains 77+ explicit rules. Not all models can hold them all. This file defines which modules each tier enables/disables.

## Tier 1 (recommended — frontier-class: GPT-4 / Claude-3.5-Sonnet / flagship open ≥30B)
**Enabled:** Full skill — all 5 Core Rules + AAP, Adaptive Pacing, Fading ladder, Motivation & Engagement (all 5 mechanisms), Metacognitive Tags, Curiosity Hooks, Enrichment, Spaced Review, Signal Routing, Emotion Override, **Depth Framework (SOLO/Bloom/DOK targets + Depth Gate + Performance Task)**, **Expertise Calibration**, **Spiral Return**, **Retrieval Practice**, **Assembly Stage**, **Learner Profile**.

## Tier 2 (capable but smaller — reliable execution ~50 rules)
**Enabled:** 5 Core Rules + AAP + Adaptive Pacing (speed only) + Recovery Kit + Signal Routing + External State Tracking + **Depth targets (simplified: declare target but skip Performance Task, use Feynman as gate)** + **Learner Profile (read on entry, update on exit, skip mid-session updates)**.
**Disabled:** Motivation module (Progress Feedback, Escape Hatch, Fatigue Check, Curiosity Hooks, Micro-Practice), Metacognitive Tags, Enrichment, Spaced Review, **Spiral Return, Retrieval Practice, Assembly Stage, Expertise Calibration**.
**Rationale:** Soft-rule modules degrade silently after ~10 turns on Tier 2. Disabling them is safer than letting them misfire. Depth targets are kept (simplified) because they address the core "只学皮毛" problem.

## Tier 3 (last resort — reliable execution ~15-25 rules)
**Enabled ONLY:** 5 Core Rules (anti-leak, one-unknown, one-question, stage-gate, learner's language) + AAP (RED-level only) + one-question-per-turn + stage-gate.
**Disabled:** Everything above — Fading, Pacing, Motivation, Tags, Enrichment, Spaced Review, Signal Routing (simplified to: blind guess → retreat; wrong → reframe), **all v2 features (Depth Framework, Expertise Calibration, Spiral Return, Retrieval Practice, Assembly Stage, Learner Profile)**.
**Rationale:** Tier 3 models can reliably hold ~7-15 rules. The 5 Core Rules + AAP RED + 2 gating rules are the minimum viable invariant. AAP RED is kept because teaching wrong API facts is worse than not teaching.

## Known Model → Tier Mapping

At session start, look up the current model name in this table. If found, use the listed tier directly — do NOT rely on self-diagnosis. If the model is not listed, fall back to self-diagnosis below.

| Model (substring match, case-insensitive) | Suggested Tier | Notes |
|---|---|---|
| `gpt-4o` / `gpt-4.1` / `o3` / `o4` | Tier 1 | Frontier OpenAI |
| `claude-opus-4` / `claude-sonnet-4` / `claude-4.5-sonnet` | Tier 1 | Frontier Anthropic |
| `claude-3.5-sonnet` / `claude-3-7-sonnet` | Tier 1 | Previous-gen frontier, still sufficient |
| `glm-5` / `glm-4.6` | Tier 1 | Zhipu frontier |
| `gemini-2.5-pro` / `gemini-2.0-pro` | Tier 1 | Google frontier |
| `deepseek-v4` / `deepseek-r2` | Tier 1 | DeepSeek frontier |
| `llama-4-maverick` / `llama-4-scout` | Tier 2 | Capable but rule-density limits after ~10 turns |
| `qwen-3-72b` / `qwen3-72b` | Tier 2 | Capable but incomplete Tier-1 reliability |
| `mistral-large-3` | Tier 2 | Capable but smaller effective context for rules |
| `gpt-4o-mini` / `gpt-4.1-mini` | Tier 2 | Reduced rule capacity |
| `claude-3.5-haiku` / `claude-3-haiku` | Tier 2 | Fast tier, reduced rule capacity |
| `gemini-2.5-flash` / `gemini-2.0-flash` | Tier 2 | Fast tier |
| `glm-4` / `glm-4-flash` | Tier 2 | Previous-gen |
| `deepseek-v3` / `deepseek-chat` | Tier 2 | Capable but rule-density limits |
| Models < 30B params or not listed | Tier 1 (default) | Self-diagnose; downgrade if hard triggers fire |

> **These are suggested values.** A user can override by stating their tier explicitly (e.g., "I'm running on a Tier 1 model" or "use Tier 2"). User override takes precedence.

## Detection

### Primary: Table Lookup (preferred)

At session start, check the current model name against the table above. If matched, adopt that tier immediately. No self-diagnosis needed.

### Secondary: Self-Diagnosis (fallback for unlisted models)

If the model is NOT in the table: assume Tier 1, but watch for degradation signals.

### Runtime Degradation Hard Triggers (all tiers)

Regardless of how the tier was determined, these observable failures trigger an immediate downgrade check:

| Hard Trigger | Action |
|---|---|
| 3 consecutive turns forgetting to write state line (Recovery L3+ and Emotion Override turns are exempt) | Downgrade one tier (T1→T2, T2→T3) |
| 3+ rules violated in a single turn (leaked answer + multi-question + skipped state line) | Downgrade one tier immediately |
| Collapsing Recovery to one level (always L1 Reframe, never escalating) | Downgrade one tier |
| Miscounting stages or forgetting Coverage Gate items mid-stage | Downgrade one tier |

When a hard trigger fires: announce "我检测到规则密度超载，切换到精简教学模式" and apply the lower tier's rule set. Do NOT silently degrade — silent degradation is worse than honest downgrade.
