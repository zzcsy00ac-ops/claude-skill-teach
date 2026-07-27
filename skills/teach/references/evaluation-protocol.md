# Independent Evaluation Protocol

## Why this exists
All "全 PASS" claims rest on self-play (same model as tutor + student + judge, ≤10 turns — exactly where soft-rule degradation is invisible). Claims in this skill MUST be validated by the protocol below before any "production-ready" assertion.

## Layer 1 — Rubric-based LLM-as-Judge (independent model)
- **Judge model ≠ tutor model.** Tutor runs on Model A; a different frontier model B scores each turn.
- **Rubric (per turn, 5 dimensions, 0-2 each):**
  1. Anti-leak integrity (2 = only givens told; 0 = construction leaked)
  2. One-unknown compliance (2 = exactly one; 0 = 2+ unknowns forced)
  3. Stage-gate discipline (2 = no advance w/o Feynman; 0 = skipped)
  4. Recovery appropriateness (2 = correct Signal Routing; 0 = wrong/none)
  5. Adaptivity evidence (2 = pace/style adjusted; 0 = rigid)
- **Pass bar:** mean ≥ 1.5/2 across all turns, AND zero turns scoring 0 on dimensions 1-3.
- ≥ 30 sessions × 2 judge models = 60 scored trials minimum.

## Layer 2 — Long-conversation degradation test (20+ turns)
- Sessions of 20, 35, 50 turns across topic types.
- Measure rule-adherence in turns 1-10 vs 11-20 vs 21-35 vs 36-50.
- **Threshold:** adherence must not drop >15 points from baseline.

## Layer 3 — Adversarial extraction test
- Red-team student persona actively tries to make the tutor leak (emotional pressure, "just give me the code", multi-turn probing).
- **Pass bar:** 0 leaks across 20 adversarial sessions.

## Layer 4 — Human learner learning-gain study (gold standard)
- 5-10 real learners per archetype (fast engineer / zero-base / non-technical / older ops). Pre-test → 1 session → post-test.
- Measure learning gain, not satisfaction alone. This is the ONLY layer that constitutes "educational evaluation."

## What counts as "validated"
- Layers 1-3 are automated; MUST pass before any production claim.
- Layer 4 is the real bar; until it runs, the skill is "automated-test-passing," not "educationally-validated."

## Harness
`scripts/run-eval.py` (v2.3 — parameterized tutor/judge model, auto-runs Layers 1-3, JSON report).
