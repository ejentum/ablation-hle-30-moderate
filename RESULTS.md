# HLE-30 Moderate — Round 3 Results

**Date:** 2026-06-01
**Benchmark:** Humanity's Last Exam (cais/hle), 30 moderate-difficulty items
**Conditions:** B (baseline), D (dynamic harness), A (adaptive harness)
**Repo:** https://github.com/ejentum/ablation-hle-30-moderate

## Headline Pass Rates

| Condition | Pass | Rate |
|---|---|---|
| **B** (baseline) | 8/30 | 26.7% |
| **D** (dynamic) | 7/30 | 23.3% |
| **A** (adaptive) | 6/30 | 20.0% |

## Pre-Registration Prediction vs Actual

The pre-registration committed four mutually exclusive hypotheses:

- **H1** — Adaptive > Dynamic > Baseline (monotonic harness lift)
- **H2** — Dynamic ≈ Adaptive > Baseline (any-harness lift, ordering not significant)
- **H3** — No condition difference (harnesses neutral at moderate difficulty)
- **H4** — Baseline ≥ harnesses (harnesses neutral-to-harmful at moderate difficulty)

**Observed ordering:** B (8) > D (7) > A (6).

The data supports **H4**: at HLE-moderate difficulty the harness-on conditions did not outperform baseline, and the adaptive condition slightly under-performed. None of the gaps clear statistical significance at n=30 (1-item swings dominate noise), so the honest read is "no measurable harness benefit at this difficulty band, with a directional baseline edge."

## Per-Category Breakdown

| Category | B | D | A |
|---|---|---|---|
| Math | 2/6 | 2/6 | 2/6 |
| Biology/Medicine | 1/4 | 1/4 | 1/4 |
| Computer Science/AI | 1/4 | 1/4 | 1/4 |
| Physics | 2/4 | 1/4 | 0/4 |
| Humanities/Social Science | 0/4 | 0/4 | 0/4 |
| Chemistry | 2/4 | 2/4 | 2/4 |
| Engineering | 0/2 | 0/2 | 0/2 |
| Other | 0/2 | 0/2 | 0/2 |
| **Total** | **8/30** | **7/30** | **6/30** |

The only category where conditions diverged was **Physics**: B=2, D=1, A=0. Every other category was flat across conditions, meaning the headline B>D>A ordering is driven by a single 4-item bucket. Humanities, Engineering, and Other shut out all three conditions (0/10 combined).

## Per-Question Pass Matrix

| hle_id | Category | B | D | A |
|---|---|---|---|---|
| 6730ab9b1c5695f59ab6a5e8 | Math | . | . | . |
| 670fe01af99389b3c7942185 | Math | . | . | . |
| 671a5d9a6e1db673f77905d6 | Math | . | . | . |
| 67136bf495e840a8db703aee | Math | P | P | P |
| 67228eb808748295331b3dfb | Math | P | P | P |
| 674a650c76449d2a51ff59c2 | Math | . | . | . |
| 67375e6f8b1cc52c211f95ce | Bio/Med | P | P | P |
| 675f78278896e41ae7eb29da | Bio/Med | . | . | . |
| 67249fe6d917564737255342 | Bio/Med | . | . | . |
| 670097e2d8f693f97c36c13f | Bio/Med | . | . | . |
| 673627bc10ec0a5f859365ce | CS/AI | P | . | . |
| 66eaed13c47c4325f612ff48 | CS/AI | . | P | P |
| 673a32a842a12c80ec5f8012 | CS/AI | . | . | . |
| 6738936964b4aaf164087959 | CS/AI | . | . | . |
| 670402f0bae67686d8aef3e8 | Physics | P | . | . |
| 67153bd7f588f3f15b038f5b | Physics | . | . | . |
| 66fcbb1e2c2f679cc795985f | Physics | . | . | . |
| 6720e184a9e1d1cc990cc8e9 | Physics | P | P | . |
| 66f8ab9c89c09a99da336b5b | Hum/SS | . | . | . |
| 67396b6ad7c193febc65bb8e | Hum/SS | . | . | . |
| 673fce92f7f86aa77259187a | Hum/SS | . | . | . |
| 6759a235c0c22e78a0758d86 | Hum/SS | . | . | . |
| 67253beb5a5d8bd389020394 | Chemistry | P | P | P |
| 67253d7aac6dc24f8aafbfc1 | Chemistry | . | . | . |
| 67151b015fc8ee8feaa3538c | Chemistry | P | P | P |
| 67253e40cff9fdccf85f3f08 | Chemistry | . | . | . |
| 673bd7048229809fa3ec5653 | Engineering | . | . | . |
| 66f85b33881bc7c87a8fc0e9 | Engineering | . | . | . |
| 670f9916451a882595c8f434 | Other | . | . | . |
| 66f5ac3d909b45b3b472d01f | Other | . | . | . |

P = pass, . = fail.

## Comparison to Rounds 1 and 2

| Round | Benchmark | n | B | D | A |
|---|---|---|---|---|---|
| 1 | MHPP-10 (code) | 10 | 10/10 | 10/10 | 10/10 |
| 2 | HLE-15 (hardest) | 15 | 4/15 (26.7%) | 5/15 (33.3%) | 5/15 (33.3%) |
| 3 | HLE-30 (moderate) | 30 | 8/30 (26.7%) | 7/30 (23.3%) | 6/30 (20.0%) |

- **Round 1** saturated; no signal because the task floor was already at ceiling.
- **Round 2** showed a +1-item lift for harness conditions over baseline on the hardest HLE slice (D=A>B). At n=15 this is directional only.
- **Round 3** flips that ordering on the moderate slice: baseline edges out both harnesses by 1-2 items.

Across Rounds 2 and 3 combined (n=45 HLE items), totals are B=12, D=12, A=11. The harnesses neither help nor hurt at aggregate HLE scale within sampling noise.

## Interpretation (300 words)

Round 3 is the cleanest negative result the ablation series has produced so far, and the honest read is H4: at HLE-moderate difficulty, neither harness condition beat baseline. The headline ordering B (8) > D (7) > A (6) is small enough to be sampling noise at n=30, but it is the wrong direction for a "harness lifts everything" story, and the direction is consistent: dynamic loses one item to baseline, adaptive loses one more to dynamic.

The category breakdown shows where the gap actually lives. Eight of nine categories are perfectly flat across conditions. The entire B>D>A spread comes from four physics items: baseline 2, dynamic 1, adaptive 0. Either the harness shifted the model's physics answers in a way that broke a coincidentally-correct baseline path, or the four-item bucket is just too small to read. With only one differential category, both readings are defensible.

The cross-round picture is more important than this round in isolation. Round 2 (HLE-hardest) gave harness conditions a +1-item edge; Round 3 (HLE-moderate) reverses that with a 1-2-item edge for baseline. The combined HLE total (B=12, D=12, A=11 across 45 items) is a wash. The clean interpretation is that on Humanity's Last Exam — where the failure mode is missing knowledge, not reasoning collapse — the harness has no consistent effect at the precision n=30 can resolve.

This matches the harness thesis: RA²R abilities act as attention anchors against reasoning decay over long chains. HLE items are short-chain knowledge-bottlenecked questions, so the harness has nothing to anchor against. The next round should target a benchmark where reasoning chain length, not factual recall, is the limiting factor.

## Cross-Links

- **Round 1 (saturated baseline):** https://github.com/ejentum/ablation-mhpp-10
- **Round 2 (HLE-15 hardest):** https://github.com/ejentum/ablation-hle-15
- **Harness API:** https://ejentum.com (RA²R Logic API, modes: reasoning, code, anti-deception, memory)
- **Pre-registration:** [PRE_REGISTRATION.md](./PRE_REGISTRATION.md)
- **Raw scores:** [raw_scores.json](./raw_scores.json)
- **Visualization:** [chart.svg](./chart.svg)
