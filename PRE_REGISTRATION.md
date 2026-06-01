# HLE-30 Moderate Ablation — Pre-Registration (Round 3)

Date to commit: TBD (before any solver runs)
Model under test: Claude Opus 4.8
Reps: 1 per (task, condition) pair, 90 solve agents total
Protocol: each solve agent calls `/harness/` itself when it sees its task (agentic-tool pattern)
Source: `cais/hle` on HuggingFace (gated; HF_TOKEN required)
Evaluation: exact-match free-text answers, graded by a blind LLM judge with X/Y/Z anonymization per question. Aggregates computed downstream from per-item judgments (not from agent-emitted aggregate fields, per the methodological lesson from Round 2 correction).

## Background

Round 1 (MHPP-10, code mode) saturated at 9/9/9 (10/10/10 corrected). Blind expert review converged on A > D > B in 8 of 9 ballots, showing code-character effect.

Round 2 (HLE-15 hardest, reasoning mode) showed B=4, D=5, A=5. Both harness arms produced a +1 question lift over baseline (H2 outcome from the pre-registration). The predicted A > D ladder did not emerge at n=15 with extreme-tail question selection. Five of eight categories returned 0/0/0 across all conditions because the questions were beyond Opus 4.8's capability ceiling regardless of scaffolding.

Round 3 tests whether the extreme-tail selection in Round 2 compressed visible separation. The same harness, same conditions, same model, same benchmark — one variable changed: question difficulty selection.

## Subset selection (the key methodological change)

30 questions sampled from the text-only exactMatch subset of HLE, stratified across 8 categories, picked from the MEDIAN of the difficulty distribution within each category (NOT the extreme tail like Round 2).

Stratification quota:
- Math: 6 (double Round 2's 3)
- Biology/Medicine: 4
- Computer Science/AI: 4
- Physics: 4
- Humanities/Social Science: 4
- Chemistry: 4
- Engineering: 2
- Other: 2
- Total: 30 (vs 15 in Round 2)

Within each category, candidates are sorted by difficulty score (rationale length + 0.3 × question length) and the middle window of size N is picked. This gives the median-difficulty question rather than the hardest-tail question.

**Difficulty comparison:**
- Round 2 (hardest): max difficulty score 32,630; rationales 30+ KB; problems requiring multi-day PhD-mathematician work
- Round 3 (moderate): difficulty range 561-1,308; rationales ~1 KB; problems where Opus 4.8 has fighting chance

Pinned HF test-split indices (deterministic): `[1797, 716, 922, 758, 1286, 2278, 2032, 2317, 1421, 510, 1916, 116, 2143, 2083, 558, 786, 423, 1193, 342, 2121, 2229, 2304, 1493, 1496, 780, 1497, 2198, 336, 709, 307]`.

The 15 Round 2 indices are EXCLUDED from the candidate pool so Round 3 uses entirely fresh questions.

## Conditions (identical to Round 2)

- B (raw baseline): no harness call
- D (dynamic reasoning): agent calls `/harness/` with `mode=reasoning`, top-1 retrieval injected
- A (adaptive reasoning): agent calls `/harness/` with `mode=adaptive-reasoning`, top-5 + adapter-rewritten scaffold

## Judging methodology

Single Opus 4.8 judge agent grades all 90 free-text submissions for semantic equivalence to canonicals, with per-question X/Y/Z anonymization rotated deterministically. Per-item judgments are the source of truth.

**Methodological fix from Round 2:** the judge's emitted `pass_rate_*` aggregate strings are explicitly NOT trusted. Aggregates are computed in the workflow's JavaScript runtime by counting `passed=true` entries in the judgments array. This is enforced in the workflow script itself rather than left to the judge agent.

## Predicted pass rates

On the moderate-difficulty subset, Opus 4.8 baseline is expected to be substantially higher than Round 2's hardest-tail subset (4/15 = 27%). Top published Claude HLE scores are around 46-47%, and moderate-difficulty stratification should approach that baseline:

- A: 16-22 out of 30 (53-73%)
- D: 14-19 out of 30 (47-63%)
- B: 11-15 out of 30 (37-50%)

The prediction is calibrated such that A > D > B with visible spread of at least 3-5 percentage points between adjacent conditions. This is the prediction that would falsify Round 2's null pass-rate result as a selection artifact rather than a genuine product limitation.

## Hypotheses

- **H1**: A > D > B with visible pass-rate spread (3+ pp between adjacent conditions). Round 2's null is a selection-artifact and the harness lifts reasoning at this benchmark + model combination.
- **H2**: A ≈ D, both > B with visible spread. Retrieval matters more than the adapter at this benchmark; consistent with Round 2's H2 outcome on a different subset.
- **H3**: A ≈ D ≈ B with no spread. The harness does not lift reasoning on HLE at Opus 4.8 capability level, regardless of difficulty surface. Round 2's mild lift was sampling noise.
- **H4**: Inverted (B > D > A). Would require investigation. Very unlikely given Round 2's per-item data showed harness arms tied with baseline at worst.

## Power analysis

n=30 with single replicate per cell gives 95% binomial CIs of approximately ±15-18 percentage points on the pass rates. A +3 pp difference between conditions (1 question swing) is well within sampling noise; a +6-10 pp difference (2-3 question swing) starts to be visible if not statistically significant. A +15-20 pp difference (5-6 question swing) would be a robust signal.

This pilot can detect medium-to-large effect sizes but cannot rule out small-or-moderate effects. A multi-replicate follow-up would be needed to bound the noise floor.

## Comparison to Rounds 1 and 2

| Round | Benchmark | Surface | Pass rate B | Pass rate D | Pass rate A | Outcome |
|---|---|---|---|---|---|---|
| 1 | MHPP-10 | Saturated coding | 10/10 | 10/10 | 10/10 | Code-character signal via blind review |
| 2 | HLE-15 hardest | Extreme-tail reasoning | 4/15 (27%) | 5/15 (33%) | 5/15 (33%) | Mild lift, no A > D separation |
| 3 | HLE-30 moderate | Median reasoning | predicted 37-50% | predicted 47-63% | predicted 53-73% | TBD |

If Round 3 shows clean spread, the series argument is complete: "harness shifts code character on saturated coding; produces visible pass-rate lift on moderate-difficulty reasoning where the solver has headroom."

If Round 3 also shows null/mild, the harness's reasoning-mode lift on Opus 4.8 is not robust enough to detect at n=30 single-rep. Next experiment would be multi-replicate or cross-model.

## Commitment

Results published regardless of which hypothesis obtains. The same correction-trail discipline applied to Round 2 will apply here: per-item judge data is authoritative, aggregates are computed downstream and verified before publication.

## Methodological notes

- HLE is gated. HF_TOKEN required at workflow runtime.
- Solver model: Claude Opus 4.8 (session main-loop model). Subagents inherit.
- Judge model: Claude Opus 4.8. Same-family judge introduces a known evaluator bias acknowledged here.
- Aggregate pass rates are computed in the workflow script's JavaScript runtime from the judge's `passed` boolean per item, not from agent-emitted aggregate strings.
- Public repo for Round 3: `ejentum/ablation-hle-30-moderate` (to be created at workflow dispatch).
- Cross-link to Round 1: `ejentum/ablation-mhpp-10`, Round 2: `ejentum/ablation-hle-15`.
- After Round 3 lands, Index phase mirrors into `ejentum/benchmarks/hle-30-moderate/` and updates the root benchmarks README + CHANGELOG.
