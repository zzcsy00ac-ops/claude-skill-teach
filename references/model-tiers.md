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

## Detection
If you notice yourself: dropping hooks, miscounting stages, collapsing Recovery to one level, or forgetting to track state — you are likely Tier 2/3. Announce "我切换到精简教学模式" and run the Tier-2 set. Do NOT silently degrade — silent degradation is worse than honest downgrade.
