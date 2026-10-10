---
type: system
name: Melange validation protocol and prospective tests
slug: melange-validation
components: [melange-validation.single-look-discipline, melange-validation.gold-set-and-rank-gate, melange-validation.prospective-contract, melange-validation.sensitivity-and-mc]
brain_notes:
  - C:/Users/pgoff/Documents/brain/concepts/single-look-holdout-discipline.md
  - C:/Users/pgoff/Documents/brain/concepts/single-look-holdout-discipline-look-ledger.md
  - C:/Users/pgoff/Documents/brain/concepts/gold-set-validation.md
  - C:/Users/pgoff/Documents/brain/concepts/zero-rank-delta-verification.md
  - C:/Users/pgoff/Documents/brain/concepts/trend-over-significance.md
paper_count: 14
tags: [system]
---

# Melange validation protocol and prospective tests

How claims in the Melange models are tested. Covers single-look holdout discipline, gold-set validation, the zero-rank-delta gate, the frozen forecast ledger, the prospective receptivity outcome contract (H1/H2), the receptivity sensitivity harness, Monte Carlo stability and method audits, and the F5 power gate.

Brain notes: [concepts/single-look-holdout-discipline](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline); [concepts/single-look-holdout-discipline-look-ledger](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline-look-ledger); [concepts/gold-set-validation](obsidian://open?vault=brain&file=concepts%2Fgold-set-validation); [concepts/zero-rank-delta-verification](obsidian://open?vault=brain&file=concepts%2Fzero-rank-delta-verification); [concepts/trend-over-significance](obsidian://open?vault=brain&file=concepts%2Ftrend-over-significance)

## Melange validation protocol and prospective tests: evidence assessment

**Scope.** The hub file has 4 components and 16 evidence rows from 14 distinct papers. By status: VERIFIED 8, VERIFIED-SOFTENED 8. No row is labelled UNVERIFIED. Several rows still contain transfer claims that their own text marks as leads under DE-062:
- the Fouirnaies & Hall "mechanical H2" mechanism
- the Kim-benchmark-as-accuracy reading
- any seniority-of-attendee outcome

These are treated below as leads, not findings.

**Headline.** Most of the literature in this file **qualifies** the validation machinery rather than supporting it. The one supporting row (Weimer 2023's negative control) is weak in relevance. None of the papers studied a validation protocol like Melange's, and only one (Kalla & Broockman) studied the U.S. Congress. The others come from other settings: NBA games (Weimer), state legislatures (Fouirnaies & Hall), a parliamentary MP sample (Linde & Vis), a climate-lobbying ODE model (Jacoby), charitable fundraising (Cagala), lobbying-report text (Kim), LLM political-text guidance (Ornstein). So the evidence works as **design critique**, not as external validation.

**Per-component weight:**
- **Single-look discipline.** Thin but consistent. Weimer is weak support for simulating under a known null. Marrero is a cited source with weaker inference, which matches Paul's claim that Melange is stricter. The empirical false-discovery question has no evidence.
- **Gold-set and rank gate.** Two qualifying rows (Kim 2025, Ornstein 2024) put pressure on the assumption that "gold labels are independent of the pipeline". The zero-rank-delta gate and the 7/14-day freshness thresholds have no literature at all.
- **Prospective contract (H1/H2).** The best-covered component. Every row qualifies it or suggests an extension, and none supports it. The rows converge on one point: H1 and H2 are predictive-validity associations. A PASS would not validate seed_prob as a contingency probability, or pressure_mult as responsiveness.
- **Sensitivity, MC and F5.** Jacoby and Linde & Vis show that the harness's swept ranges leave out the null arms and the flip-model channel weights. So "ROBUST" holds only for the range swept. The F5 figure "72 of 511 new-event members" could not be found in the engine code or docs (Kalla row). It has no stated window and should not be quoted until it is found.

**Contradictions to name plainly:**
1. **Source vs. use.** Fouirnaies & Hall, which the system cites for the retiring multiplier, find that effort falls but positions do not move. H2's outcome is stance-tier movement, the channel where that paper predicts nothing. The paper is also a state-legislature result, not a Congress result.
2. **Gold independence.** build_tweet_gold_queue.py writes model_stance into the same rows as the human gold columns. If labellers can see it, the assumption "gold labels are independent of the pipeline" fails by design (Ornstein row; a design implication, not a measured anchoring effect).
3. **Agreement reported as accuracy.** The Kim benchmark compares against labels that are mostly model outputs: 541 of 876 pairs are GNN predictions and 335 are GPT-4 labels, with no year window stated. It measures agreement, not accuracy.
4. **Component description vs. code.** The description puts the ROBUST bar (top-20 overlap of 18 or more) next to MC stability. In the code that bar lives only in receptivity_sensitivity.py, and mc_stability_check.py sets no numeric bar (Harrison row).

## Single-look holdout discipline and look ledger

**What it computes.** Pass or miss of pre-registered confirmatory looks.

**Method.** The gate is pre-registered with hashes; a look is consumed once and never re-run; look fragments are void. A joint-null simulation gives about 1.5-2% false pass.

**Code refs.** (none listed)

**Assumptions.**
- Pre-registration prevents basis-shopping

Brain notes: [concepts/single-look-holdout-discipline](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline); [concepts/single-look-holdout-discipline-look-ledger](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline-look-ledger)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[weimer-2023-causal-approach-detecting]] | 2023 | supports | A Monte Carlo with zero true momentum (130,000 independent 50/50 scoring plays, a TV timeout after one play in every 50, substitutions not simulated) yields a null treatment coefficient (b = 0.002, SE 0.022, n = 1,336). The Fig. 6 caption calls it a negative control showing the matched design does not find an effect where none exists. | Supports the general practice of simulating under a known null to check that a selection or gating rule cannot manufacture a pass. Says nothing about the 1.5-2% figure or pre-registration; the brain simulation concerns look false-pass rates, not engine outcome estimators. | weak | VERIFIED |
| [[marrero-2022-evaluating-voice-samples]] | 2022 | qualifies | No standard test fits the repeated model and feature selection; uncertainty is handled by assuming a generalizable model improves with 100-fold CV (about 9 percent more training data per fold). No permutation null is reported. Post-hoc reliability-performance correlation t(4)=2.65, r=0.80, p=.057, falls to r=0.309 under leave-one-out. | The paper's effect sizes come from N=545 self-report students and have no null distribution, so they should not set a pass bar for an N=90, 88-feature legislator design without a permutation floor. This is consistent with the system's stricter rule. | moderate | VERIFIED |

Rows: 2 (2 distinct papers); VERIFIED 2.

**Assessment.**

**Evidence weight: thin, directionally consistent, and silent on the core empirical question.**

- **Weimer 2023 (supports, weak).** A Monte Carlo with zero true momentum returns a null coefficient: b = 0.002, SE 0.022, n = 1,336 matched runs in the simulated set (130,000 simulated 50/50 plays). This supports the practice of simulating under a known null to check that a gate cannot manufacture a pass, as Melange did with its joint-null false-pass simulation (pass both panels about 1.5-2%, dual dominance under 1%). It says nothing about the 1.5-2% figure itself, about pre-registration, or about look-consumption rules.
- **Marrero 2022 (qualifies, moderate).** Cited in the audio prereg, yet it has no permutation null and uses 100-fold CV. Its post-hoc reliability-performance correlation (r = 0.80, t(4) = 2.65, p = .057) falls to r = 0.309 under leave-one-out. Effect sizes come from N = 545 self-reporting students. The system was right not to let those effect sizes set the pass bar: audio-E r = +0.228 against a floor of +0.234 did not pass. This shows one cited source is less disciplined than Melange, not that Melange's discipline is well calibrated.

**Assumption: "Pre-registration prevents basis-shopping."** No row tests this. Weimer's window-sweep appendix (filed under sensitivity-and-mc) indirectly supports locking the outcome window. The contract currently locks a minimum (60 days), not a window set.

**Empirical question: the empirical false-discovery rate across the look ledger.** No evidence. GAP.

**Contradictions.**
- None between evidence and the system. The only tension is internal: a strict single-look rule combined with an underpowered prospective contract (F5: '72 of 511' unlocated) means strictness may show up mostly as unspent or MISS looks rather than as a low false-discovery rate. Strictness and calibration are different properties.

**Gaps.**
- Empirical false-discovery and false-miss rates of the protocol across the look ledger. To fill it: tally every consumed look (PASS / MISS / RETIRED / VOID), then run a plant battery that injects known-true and known-null signals through the same gates. Report it as a diagnostic, not a look.
- External corroboration of the 1.5-2% joint-null false-pass figure. To fill it: rerun the simulation with a second implementation (two-path reproduction, DE-062).
- Imai & Kim 2021 and Keele 2015 are in Paul's citation list but have no evidence rows.

**Paul's position vs the evidence.**

Paul's position is that the Causal & Methodological folder is unannotated and that the validation discipline is stricter than anything in the bibliography. The rows are consistent with the second half: Marrero (no permutation null) and Cagala (20-fold CV rather than a single look) are both looser. But 'stricter' is a claim about procedure, not about error rates. No row shows that the stricter procedure gives a lower false-discovery rate or acceptable power. The evidence does not contradict Paul's position, and does not support any claim beyond 'more procedurally conservative than the sampled sources'.

**Open questions.**
- Does the look ledger's realised PASS rate fit the simulated 1.5-2% joint-null rate, or does it look like a protocol that mostly returns MISS or UNDERPOWERED because power is low?
- Should the contract lock a small set of outcome windows instead of a 60-day minimum? This is a ruling for Paul.
- Where are Imai & Kim 2021 and Keele 2015 used, and what do they contribute?

**Read first.** [[weimer-2023-causal-approach-detecting]], [[marrero-2022-evaluating-voice-samples]], [[sverdrup-nd-introduction-grf-causal-forests]], [[cagala-2021-optimal-targeting-fundraising]]

## Gold-set validation and zero-rank-delta gate

**What it computes.** Trust scores against owner-ruled gold sets, and a byte-identical ranks_python.json check for behaviour-preserving changes.

**Method.** Gold sets: 400 tweets, 760 press rows, Senate panels. Freshness: warn after 7 days, fail after 14.

**Code refs.** `fec/tests/run_tests.py`, `fec/audit/audit_full.py`

**Assumptions.**
- Gold labels are independent of the pipeline

Brain notes: [concepts/gold-set-validation](obsidian://open?vault=brain&file=concepts%2Fgold-set-validation); [concepts/zero-rank-delta-verification](obsidian://open?vault=brain&file=concepts%2Fzero-rank-delta-verification)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2025-measuring-interest-group]] | 2025 | qualifies | LLM labels agree with human coders at 96.93% accuracy and F1 97.19, but only on the 391 samples where two coders agreed. MapLight concordance is 91.03% (1,076 of 1,182 pairs) and covers Support/Oppose only; MapLight labels were also part of the GNN training labels. GNN best configuration: accuracy 78.51, F1 74.65; per-class F1 Support 79.19, Oppose 64.06, Engage 80.70. | The paper's human validation uses only coder-agreed lines, so its 96.93% does not carry over to ambiguous, Amend or Mention lines. The system benchmark measures agreement with Kim's labels, not accuracy: 541 of its 876 reference pairs are Kim GNN predictions (a stage with 78.51% test accuracy) and 335 are GPT-4 labels. Denominator: the 876-pair overlap (the benchmark file states no year window). UNVERIFIED as accuracy until a human-gold path reproduces it (DE-062). | strong | VERIFIED-SOFTENED |
| [[b-2025-comparing-fine-tuned]] | 2025 | method-source | Stance is a three-way task in which unrelated is a first-class label alongside agree and disagree. Per-class precision, recall and F1 are reported, with macro averages. | The labelling and metric conventions transfer across language and domain. The paper gives no cross-validation, no agreement statistic and no confidence interval, so it is a source for the convention only, not for validation rigour. | moderate | VERIFIED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | qualifies | The recommended validation has three steps: hand-label a small set to design the prompt, validate on a second hand-labelled set (by the team or crowd coders) for strong correlation with ground truth, then apply. Crowd workers increasingly use LLMs (Veselovsky et al. 2023), which threatens crowd-code independence. | The paper does not test anchoring on pre-labels, so this is a design implication, not a measured effect. If a labeller sees model_stance, agreement with gold partly measures agreement with the model. Oversampled hard slices make these accuracies incomparable with the paper random-sample 88.4 percent. | moderate | VERIFIED-SOFTENED |

Rows: 3 (3 distinct papers); VERIFIED 1, VERIFIED-SOFTENED 2.

**Assessment.**

**Evidence weight: moderate, and it mostly qualifies the component. The independence assumption is under direct design pressure. The rank gate has no evidence.**

- **Ornstein 2024 (qualifies).** The recommended validation is to hand-label a design set, then validate on a second hand-labelled set; crowd coders increasingly use LLMs, which threatens independence. build_tweet_gold_queue.py writes model_stance and model_margin into the same rows as the human gold_* columns (lines 92-96), and oversamples low-margin and anti-bank slices (lines 25-27). If labellers see model_stance, gold agreement partly measures agreement with the model. The paper does not measure anchoring, so this is a design implication. The oversampling also makes Melange's gold accuracies incomparable with Ornstein's random-sample 88.4%.
- **Kim 2025 (qualifies, strong).** The 96.93% LLM-human accuracy is computed only on the 391 samples where the two coders agreed. MapLight concordance (91.03%, 1,076 of 1,182 pairs) covers Support/Oppose only, and MapLight labels were among the GNN training labels. The system's benchmark uses 876 (organisation, bill) pairs (no year window stated): 541 are Kim GNN predictions (78.51% test accuracy) and 335 are GPT-4 labels. Recorded Support/Oppose recall 0.49 and precision 0.42, with 0 Amend found against Kim's 23. These are **agreement with model labels, not accuracy**, until a human-gold path reproduces them (DE-062).
- **B. 2025 (method source).** Supports the convention stance_eval.py follows: unrelated as a first-class class, per-class and macro-F1 reporting. It supports the metric convention only, not validation rigour.

**Zero-rank-delta gate and freshness thresholds (warn 7 days, fail 14).** No evidence rows. The byte-identity gate is an engineering regression check; the 7/14-day thresholds are owner choices with no stated empirical basis.

**Empirical question: gold-set accuracy vs accuracy on randomly sampled live rows.** Unanswered. Oversampled hard slices bias gold accuracy downward; anchoring, if present, biases it upward. Net direction unknown.

**Contradictions.**
- 'Gold labels are independent of the pipeline' conflicts with the gold queue design: model_stance and model_margin are written into the same rows as the gold_* columns (build_tweet_gold_queue.py:92-96). Whether labellers see them is not recorded.
- The Kim-format benchmark is described in places as accuracy, but its reference labels are mostly model outputs: 541 of 876 pairs are GNN predictions and 335 are GPT-4. It is agreement, not accuracy.
- The DEBATE 0.571 adversarial-frame figure comes from audit documents, not the benchmark files. Its denominator and window are not stated here; do not quote it without them.

**Gaps.**
- Gold accuracy vs live-row accuracy. To fill it: a blind simple random sample of live rows (state window and N), compare macro-F1 against the oversampled gold set, raw and reweighted to live base rates.
- Anchoring effect of visible pre-labels. To fill it: a small within-labeller experiment, same rows with and without model_stance visible, randomised order.
- Basis for the 7/14-day freshness thresholds. To fill it: measure gold-set accuracy decay on successive monthly random samples.
- Birkenmaier & Lechner 2025 is in Paul's citations with no evidence row.

**Paul's position vs the evidence.**

Paul highlighted human-labelled validation in Birkenmaier & Lechner 2025 and Kim 2025. The evidence on Kim cuts against reading it as a strong precedent: its 96.93% covers only the 391 coder-agreed lines, not the ambiguous, Amend or Mention lines that matter most. Ornstein adds that the human set must be independent of the model, which the current gold queue layout may not guarantee. Paul's emphasis is supported in principle, but the two highlighted papers are weaker precedents than they look, and the system's own gold set has an independence risk. Birkenmaier & Lechner has no evidence row, so that half is unassessed.

**Open questions.**
- Do labellers see model_stance and model_margin during labelling?
- Is there a human-gold subset of the 876 Kim-overlap pairs that could turn agreement figures into accuracy figures?
- Were the 400-tweet and 760-press-row gold sets drawn with the same oversampling? If so, what reweighting gives live-base-rate accuracy?

**Read first.** [[ornstein-2024-train-your-stochastic]], [[kim-2025-measuring-interest-group]], [[b-2025-comparing-fine-tuned]]

## Prospective receptivity contract (H1-H2) and forecast ledger

**What it computes.** H1: Spearman(seed_prob, new cosponsorship). H2: Spearman(pressure_mult, |tier movement|). Frozen cosponsor-join and dyad-persistence (p = 0.82) forecasts with Brier scores.

**Method.** Bars: >= .20 PASS, .10-.20 DIRECTIONAL, <= -.10 INDICTED. Needs at least 60 days and 150 movers. One look per contract.

**Code refs.** `fec/engine/outcome_contract.py`, `fec/engine/forecast_ledger.py`

**Assumptions.**
- Cross-sectional Spearman is a valid test

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[weimer-2023-causal-approach-detecting]] | 2023 | qualifies | The authors argue that comparing points before and after a coach-called timeout is fallacious because coaches often call timeouts to stop momentum. They use TV timeouts, which fall at predetermined times, and match runs interrupted by a TV timeout to uninterrupted runs on propensity scores (pairs; unmatched runs dropped). Result: -11.2% points over the next 3 minutes (b = -0.119, SE 0.012), n = 5,148 matched runs. Caveat from the same paper: Appendix E (as described in the main text) finds coaches' timeouts give largely similar, slightly larger effects, and the TV-vs-coach coefficient difference was not significant; Sec. 5.3 concedes TV timeouts may not be fully exogenous. | Design logic only, not effect size. Lobbyist-chosen contact is endogenous like a coach's timeout, so an engaged-only rho would confound pressure_mult with targeting unless contact timing is exogenous or matched on targeting covariates. The engaged-only arm is a subgroup correlation, not a before/after-contact comparison, so the analogy is to the selection problem, not an identical design. The paper's own Appendix E finding (coach vs TV timeouts not significantly different) means it does not empirically show the endogenous design is badly biased in its setting. Single observational study with matching. | moderate | VERIFIED-SOFTENED |
| [[fouirnaies-2022-electoral-incentives-affect]] | 2022 | qualifies | In the final term, effort falls (sponsorship -6%, committee activity -8%, participation -2.7 points) while positions stay flat (W-NOMINATE and interest-group nulls). | Moderate as a threat to interpretation. It applies only if tier movement is driven by evidence volume. If the contract is still unfrozen, a reported retiring/other-office stratum could be added. If it is frozen, any H2 result should carry this caveat. This is analysis, not a look, and stays UNVERIFIED until reproduced (DE-062). | moderate | VERIFIED |
| [[blackwell-2026-effect-political-advertising]] | 2026 | suggests-extension | Placebo tests check the identifying assumptions by applying the estimator to outcomes measured before treatment: the previous election's vote share and turnout, and the pre-election poll share. IPTW-FE estimates are near zero, while the naive approach shows residual confounding for some Democratic-ad effects. | The placebo logic carries over directly. A pre-registered negative control would be Spearman(seed_prob, cosponsorship delta over a window that ends before the freeze). A non-zero value would show that seed_prob tracks a stable member trait (a time-constant confounder) rather than responsiveness. Adding it changes the contract, so it needs Paul's ruling before the single look is spent, and it needs a second pre-freeze snapshot pair. Neither H1 nor H2 measures a lobbying treatment, so even a PASS remains an association. | moderate | VERIFIED |
| [[kalla-nd-campaign-contributions-facilitate]] |  | suggests-extension | The seniority of the official who attends responded to a randomized donor label across 191 offices over about three months. Cosponsorship as an outcome tells us little: nine legislators cosponsored in total, with the same share in both arms, and the authors say the estimate is too uncertain to bear on access effects. | The transfer concerns outcome choice, not effect size. A seniority-of-attendee outcome needs a real outreach log, which the system does not have, plus its own pre-registered single look. Any result stays UNVERIFIED until a second path reproduces it (DE-062). Do not use the 72 of 511 figure until it is located. | moderate | VERIFIED-SOFTENED |
| [[cagala-2021-optimal-targeting-fundraising]] | 2021 | suggests-extension | Policy gains are identified from randomized assignment through the AIPW score; identifying the causal effect of each covariate is not needed (Section 4.3, Appendix B). The 2014 rule's gains held in a caliper-matched 2015 sample but not in the full 2015 sample (Section 5.6, Table 8). The paper validates with 20-fold cross-validation rather than a single-look holdout (Section 5.3). | The paper shows the missing design: randomized treatment within strata, which is the precondition for upgrading from forecasting to spending-effect evidence. This is feasible for Terrier contacts or CU outreach logs and not for statutory PAC contributions to legislators. The paper's gains are not evidence for the system. | weak | VERIFIED |
| [[chen-2021-contingency-aware-influence]] | 2021 | qualifies | q is the probability that a selected node is willing to act as a seed, conditional on being selected or invited. It is assumed known a priori, so it is a take-up rate, not an unconditional outcome. | Unconditional new cosponsorship mixes take-up with contact choice and base activity. A pass would not validate seed_prob as a contingency probability, and a miss would not refute it. The construct match needs an engaged-only arm built on a contact log. | moderate | VERIFIED-SOFTENED |
| [[sverdrup-nd-introduction-grf-causal-forests]] |  | qualifies | Effect heterogeneity is tested by ranking on a score and computing the gain over the ATE in the top-ranked fraction (TOC/AUTOC), which requires treated and untreated units. The authors warn that out-of-bag TOC is a heuristic without finite-sample guarantees and that policy trees need a held-out set (pp.8-11). | Effect-modification reading applies only if CU outreach or money exposure is logged per member with plausibly known assignment. Otherwise H1 is a predictive-validity check. The held-out, single-look design is consistent with the authors' advice, but this is a judgment, not something the paper tests. | moderate | VERIFIED |

Rows: 7 (7 distinct papers); VERIFIED 4, VERIFIED-SOFTENED 3.

**Assessment.**

**Evidence weight: the most-studied component. It is uniformly qualified, with no supporting rows. The rows converge: H1/H2 can at best establish predictive association, not the constructs their predictors claim to represent.**

**Construct mismatch.**
- **Chen 2021.** In the cited source, q is a take-up probability *conditional on being invited*. H1 correlates seed_prob with *unconditional* new cosponsorships, which mixes take-up with contact choice and base activity. A PASS would not validate seed_prob as a contingency probability, and a MISS would not refute it. Only an engaged-only arm built on a contact log tests the construct, and that arm is NOT_IMPLEMENTED_NO_CONTACT_CONTRACT.
- **Sverdrup / grf.** Effect-modification tests (TOC/AUTOC) need treated and untreated units. Without logged outreach, H1 is a predictive-validity check only.

**Mechanical-confound threat to H2.**
- **Fouirnaies & Hall 2022.** A **U.S. state-legislature** result, not Congress. In the final term, effort falls (sponsorship -6%, committee activity -8%, participation -2.7 points) while positions stay flat. pressure_mult carries a 0.60 retiring factor. If tier movement is driven by evidence-event volume, retiring members show both low pressure_mult and low movement, so a positive H2 could arise mechanically. This stays a lead (DE-062) until reproduced, and applies only if movement is volume-driven.

**Missing negative control.**
- **Blackwell 2026.** Placebo outcomes measured before treatment caught residual confounding. The direct port is Spearman(seed_prob, cosponsorship delta over a window ending before the freeze). A non-zero value would show seed_prob tracks a stable member trait. This changes the contract, so it needs Paul's ruling **before** the single look is spent, plus a second pre-freeze snapshot pair.

**Selection into contact.**
- **Weimer 2023.** Coach-called timeouts are endogenous, like lobbyist-chosen contact. An engaged-only rho would confound pressure_mult with targeting unless contact is exogenous or matched. Caveat: in its setting the endogenous design was not significantly different from the TV-timeout design.

**Outcome sparsity and design ceiling.**
- **Kalla & Broockman (Congress, randomised, 191 offices, about 3 months).** Cosponsorship was nearly useless as an outcome: 9 legislators cosponsored in total, the same share in both arms. Short-window cosponsorship deltas are sparse, which bears on the 60-day / 150-mover threshold.
- **Cagala 2021.** Randomised assignment within strata is the precondition for moving from forecasting to spending-effect evidence. Feasible for Terrier or CU outreach logs, not for statutory PAC contributions. CLAIM_SCOPE (receptivity_forecast_only_not_spending_effects) already concedes this.

**Assumption: "Cross-sectional Spearman is a valid test."** Valid as a test of rank association between a frozen score and a later outcome; not valid for the causal or contingency constructs the predictors are named for.

**Bars (PASS at .20 or above, DIRECTIONAL .10-.20, INDICTED at -.10 or below).** No literature basis in the evidence. My own arithmetic, not evidence: at n = 150 the sampling SE of a correlation near .20 is about 0.08, so a true rho of exactly .20 would be classed PASS only about half the time.

**Forecast ledger (frozen cosponsor-join and dyad-persistence p = 0.82, Brier).** No evidence rows. D-104..D-108 say reciprocity is NOT validated; any dyad-persistence figure is a model output, not evidence of tie strength or access (D-025).

**Contradictions.**
- Fouirnaies & Hall, cited for the retiring multiplier, finds that final-term legislators cut effort but do not change positions. H2's outcome is stance-tier movement, the channel where the paper predicts no change. It is also a state-legislature result.
- 'seed_prob' (a contingency take-up probability in Chen 2021) and the H1 outcome (unconditional new cosponsorship) do not measure the same construct.
- The F5 replay figure '72 of 511 new-event members' could not be found in the engine code or docs searched (Kalla row). No window is stated. Do not quote it until found.

**Gaps.**
- Engaged-only arm: needs a receipted contact log, ideally with randomised or stratified-random assignment (Cagala design).
- Pre-freeze placebo for H1 and H2 (Blackwell design): needs a second pre-freeze snapshot pair and Paul's ruling before the look is spent.
- Retiring / other-office stratum for H2, or a pre-stated caveat; plus a check of whether |tier movement| tracks the count of new evidence events over the same window.
- Operating characteristics of the .20 / .10 / -.10 bars at n = 150: simulate P(PASS), P(DIRECTIONAL), P(INDICTED) across true rho. Diagnostic, not a look.
- Forecast ledger calibration: a reliability diagram on resolved forecasts, with window and N stated.

**Paul's position vs the evidence.**

Paul has recorded no annotation on this component. His two cited sources both **qualify** rather than support the contract: Chen 2021 defines q as conditional take-up, which H1 does not measure; Fouirnaies & Hall 2022 is a state-legislature study that finds no position change in the final term, while H2 scores position movement and could be mechanically inflated by the documented effort drop. CLAIM_SCOPE (forecasting only, not spending effects) is the one design choice the evidence clearly endorses (Cagala, Sverdrup, Blackwell). Any H1 or H2 PASS should be reported as an association between a frozen model score and a later outcome, not as validation of receptivity, take-up or responsiveness.

**Open questions.**
- Is the H1/H2 contract already frozen (hashed)? If not, Paul could rule on adding a pre-freeze placebo and a retiring stratum before the look. If frozen, both become mandatory caveats.
- Is tier movement driven mainly by the volume of new evidence events?
- Will any contact log (Terrier, CU outreach) ever have plausibly exogenous or randomised assignment?
- Where does the '72 of 511 new-event members' F5 figure come from, and over what window?

**Read first.** [[chen-2021-contingency-aware-influence]], [[fouirnaies-2022-electoral-incentives-affect]], [[blackwell-2026-effect-political-advertising]], [[kalla-nd-campaign-contributions-facilitate]], [[weimer-2023-causal-approach-detecting]], [[cagala-2021-optimal-targeting-fundraising]]

## Receptivity sensitivity harness, MC stability and F5 power gate

**What it computes.** Top-20/50 target overlap under constant sweeps; seed-set stability across MC budgets; H1/H2 power (requires at least 150 new-event members).

**Method.** ROBUST at top-20 overlap >= 18. The F5 replay found 72 of 511 new-event members (UNDERPOWERED; look unspent).

**Code refs.** `fec/members/receptivity_sensitivity.py`, `fec/engine/mc_stability_check.py`, `fec/engine/method_audit.py`

**Assumptions.**
- A robust target set means the constant cannot change a decision

Brain notes: [actors/quant-model-evaluation-skill](obsidian://open?vault=brain&file=actors%2Fquant-model-evaluation-skill)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[weimer-2023-causal-approach-detecting]] | 2023 | suggests-extension | Appendix D (described in the main text) re-tallies post-treatment points over windows from 60 s to 4 minutes in 10 s steps; the negative effect persists but the point estimate steadily decreases as the window lengthens, and there is no significant effect for points scored between minutes 3 and 4. The first-minute effect is about four times the 3-minute effect. | Suggests pre-registering a small set of outcome windows inside the contract lock rather than choosing one afterward (basis-shopping). Minute-scale decay says nothing about congressional time scales. | weak | VERIFIED-SOFTENED |
| [[harrison-2010-introduction-monte-carlo]] | 2010 | context | The paper reports no error analysis, convergence diagnostic or comparison of samplers. On randomness quality it says LCGs do not do well on many Diehard-type tests and that the Mersenne Twister is becoming the generator of choice, passing most tests except some for encryption RNGs. | The system goes further than the source. The paper can endorse the generator choice but supplies no criterion for MC budget or stability. The ROBUST bar (top-20 overlap >= 18) lives in fec/members/receptivity_sensitivity.py:113, a constant-sensitivity check, not in mc_stability_check.py; any MC-stability bar would have to come from elsewhere (the script's docstring cites Almazaydeh et al. 2026 as motivation only). | weak | VERIFIED-SOFTENED |
| [[linde-2016-politicians-take-risks]] | 2016 | suggests-extension | Outside the Asian disease problem, the political-scenario reflection effect is weak: only the votes+economic scenario shows it (85% vs 69.23%, Fisher p=0.302; significant only as a scenario interaction in the pooled regression, OR 8.684, p=0.033), and the paper reports no loss aversion. Effect direction for an electoral pt domain is therefore uncertain, and the size of LOSSFRAME is unsupported. | Applies directly. Adding a pt = 1.0/1.0 arm and a LOSSFRAME = 1.0 arm to the existing top-20/50 overlap harness would show whether either constant changes targeting decisions. Diagnostic sweep, not a confirmatory look. | moderate | VERIFIED-SOFTENED |
| [[jacoby-2025-mathematical-modeling-analysis]] | 2025 | suggests-extension | Varying the centrist-flow parameters by 10% produces a wide spread of final C, Y and N counts. In the parameter-impact ranking, centrist-lobbying parameters (M_LF, M_LA) and tau rank high and initial compartment sizes rank low. Linear SVMs on 1000-sample sweeps find no clean pass/fail boundary in multi-parameter sweeps. | The transfer is a method idea, not evidence. Jacoby's sensitivity comes from a well-mixed ODE model with literature-fixed parameters, and the system's flip model is a network-based ratio of saturating terms. What carries over is the diagnostic: the parameters that decide who is movable and how fast they move should be swept first. The pass/fail SVM is not needed, because top-k overlap already measures decision change. Any such sweep is a bound on the model, not validation (DE-062). | moderate | VERIFIED |

Rows: 4 (4 distinct papers); VERIFIED 1, VERIFIED-SOFTENED 3.

**Assessment.**

**Evidence weight: moderate on what to sweep, nil on pass bars. The ROBUST label is conditional on ranges that leave out the null arms and the highest-leverage flip-model parameters.**

**Receptivity sensitivity harness.**
- **Jacoby 2025 (method idea, not evidence).** In a well-mixed climate-lobbying ODE, 10% changes to the centrist-flow parameters give a wide spread of final counts. That model is structurally unlike Melange's network flip model, so only the diagnostic transfers: sweep first the parameters that decide who is movable. The harness does not sweep WM/WP (0.35/0.65) or the PERS persuadability table.
- The one comparable knob, PERS_NONE, was swept by hand across 0.30-0.95. Top-20 overlap was 16/18/20/18/15 (denominator 20), so the two ends fall below the ROBUST bar of 18. Total winnability ran from 149.5 to 245.8 (model units; window and denominator not stated). A sweep bounds the model; it does not validate it (DE-062).
- **Linde & Vis 2016 (moderate).** In a parliamentary MP sample (not Congress), the reflection effect outside the Asian-disease frame is weak. Only the votes+economic scenario shows it (85% vs 69.23%, Fisher p = 0.302, significant only as a pooled interaction, OR 8.684, p = 0.033), and no loss aversion is reported. The harness sweeps PT only between mild (1.10/0.95) and strong (1.25/0.85): no PT-off arm (1.0/1.0), no reversed arm, no LOSSFRAME sweep. The constants with the weakest literature support are never swept to their null.

**MC stability.**
- **Harrison 2010 (context, weak).** Endorses the Mersenne Twister (bystander rolls in scenario.py) and says nothing about splitmix64 (edge draws), MC budget or a stability criterion. mc_stability_check.py reports Jaccard overlap, first-k identical picks and final-spread delta at MC = 100 (SEED 7) versus MC = 20 (SEED 1, 13), but **sets no numeric pass bar**. The ROBUST 18-of-20 bar lives only in receptivity_sensitivity.py:113.
- **Weimer 2023 (weak).** Its window sweep (60 s to 4 min) showed the estimate decaying as the window lengthened. The transferable idea is to pre-register a small set of outcome windows inside the contract lock.

**F5 power gate.** The figure '72 of 511 new-event members (UNDERPOWERED; look unspent)' could not be found in the engine code or docs searched (Kalla row). The 150-member threshold has no literature basis in the evidence.

**Assumption: "A robust target set means the constant cannot change a decision."** Holds only for the swept range.

**Contradictions.**
- The component description puts the ROBUST bar next to MC stability, but mc_stability_check.py has no numeric bar; the 18-of-20 bar exists only in receptivity_sensitivity.py:113 (Harrison row).
- The F5 figure '72 of 511 new-event members' is stated as fact in the component description, but could not be found in the engine code or docs, and its window is not stated.
- The PERS_NONE hand sweep gave top-20 overlap of 16 and 15 (of 20) at the range ends, below the system's own ROBUST bar of 18, yet PERS is not in the harness's VARIANTS.

**Gaps.**
- Null and reversed arms for weakly supported constants: PT 1.0/1.0, PT reversed, LOSSFRAME 1.0. Diagnostic, not confirmatory.
- WM/WP and the PERS table are missing from VARIANTS; fold the PERS_NONE hand sweep into the harness.
- A pre-registered numeric stability bar for mc_stability_check.py, with its basis stated.
- A power study for H1/H2: Spearman power at n = 150 for plausible true rho, and expected new-event members per 60-day window from historical cosponsorship rates (state window and denominator).

**Paul's position vs the evidence.**

Paul highlighted Jacoby's finding that outcomes are highly sensitive to the centrist-flow parameters. The evidence supports a method lesson, not a substantive one: Jacoby's model is a well-mixed ODE with literature-fixed parameters, and its sensitivity numbers do not transfer. The lesson (sweep the parameters that govern movability first) is the one the harness does not yet follow: WM/WP and PERS are unswept, and the PERS_NONE hand sweep already falls below the ROBUST bar at its range ends. The highlight argues for extending the harness; current ROBUST labels should be read as conditional on the swept range.

**Open questions.**
- Does adding PT 1.0/1.0 and LOSSFRAME 1.0 arms drop top-20 overlap below 18?
- Should the PERS_NONE hand sweep result (16/18/20/18/15 of 20) be recorded as a sensitivity finding and brought into the harness?
- What numeric bar, if any, should mc_stability_check.py enforce, and on what basis?
- Where is the 72-of-511 F5 replay recorded, and what window and denominator does it use?

**Read first.** [[jacoby-2025-mathematical-modeling-analysis]], [[linde-2016-politicians-take-risks]], [[harrison-2010-introduction-monte-carlo]], [[weimer-2023-causal-approach-detecting]]

## Across components

**1. Several checks may partly measure the thing they check.** Gold set: model_stance is written beside the human gold columns (Ornstein row). Kim benchmark: scored against labels that are mostly model outputs (541 GNN and 335 GPT-4 of 876 pairs). H2: may correlate pressure_mult with an outcome whose volume the retiring factor already predicts (Fouirnaies & Hall lead, state-legislature transfer). Each is a design risk, not a measured bias, and each can be fixed cheaply: hide the pre-labels, get a human-gold subset, add a retiring stratum or volume check.

**2. Everything is forecasting, nothing is treatment.** Cagala, Sverdrup, Blackwell, Kalla and Chen all point the same way. Without logged and preferably randomised contact, the system can establish rank association between frozen scores and later behaviour, and nothing more. Dashboards and briefs must not upgrade a future H1/H2 PASS to 'receptivity validated'. Graph-derived scores remain heuristic model outputs, not closeness, access or introduction probabilities (D-025).

**3. Strictness and power pull against each other.** The single-look discipline is procedurally stricter than the sampled literature (Marrero, Cagala), but the prospective contract appears underpowered: the F5 figure is unlocated, cosponsorship events are sparse (Kalla: 9 cosponsors among 191 offices over about 3 months), and point-estimate bars at n = 150 classify a true rho of .20 as PASS only about half the time (my arithmetic, not evidence). A strict protocol with low power produces mostly UNDERPOWERED or MISS outcomes, which is not a low false-discovery rate. The look ledger's empirical error rates are the missing measurement.

**4. Robustness depends on the range swept.** The harness labels constants ROBUST without testing their null values. The weakest-supported parameters (PT and LOSSFRAME per Linde & Vis) and the highest likely leverage (WM/WP and PERS, per Jacoby's method lesson) are left out.

**5. Transfer conditions.** Only Kalla & Broockman is a U.S. Congress study. Fouirnaies & Hall is state legislatures, Linde & Vis is MPs outside the U.S., and Weimer, Jacoby, Cagala and Harrison come from unrelated domains. Every row is design guidance, not an importable effect size.

**Project wording constraints.** Dyad-persistence and reciprocity-based forecasts are not validated (D-104..D-108). Nothing in these rows argues for dropping regular cosponsorship edges or per-edge volume normalization (both REJECTED proposals). LDA appears only as an identity and position source (Kim 2025), consistent with D-021.

## Research agenda

1. Rule (Paul) before the H1/H2 look is spent: whether to add a pre-freeze placebo, Spearman(seed_prob, cosponsorship delta over a window ending before the freeze), following Blackwell 2026. Needs a second pre-freeze snapshot pair.
2. H2 retiring confound check (diagnostic): correlate |tier movement| with the count of new evidence events over the scoring window. If volume-driven, add a retiring/other-office stratum, or a mandatory caveat if the contract is frozen. A lead (DE-062) until a second path reproduces it.
3. Simulate operating characteristics of the .20 / .10 / -.10 bars at n = 150 (P(PASS), P(DIRECTIONAL), P(INDICTED) across true rho), with historical new-cosponsorship rates per 60-day window.
4. Locate or retract the F5 '72 of 511 new-event members' figure, and state its window and denominator before anyone quotes it.
5. Extend receptivity_sensitivity.py VARIANTS: PT 1.0/1.0, PT reversed, LOSSFRAME 1.0, WM/WP sweep, and the PERS table (bring the PERS_NONE hand sweep, 16/18/20/18/15 of 20, into the harness). Diagnostic sweep, not a confirmatory look.
6. Pre-register a numeric stability bar for mc_stability_check.py with its basis stated. Correct the component description, which implies the 18-of-20 ROBUST bar applies to MC stability.
7. Gold-set independence: confirm whether labellers see model_stance and model_margin. Hide them going forward, and run a small with/without-pre-label anchoring check.
8. Gold vs. live accuracy: label a blind simple random sample of live rows (state window and N), then compare macro-F1 against the oversampled gold set, raw and reweighted to live base rates.
9. Kim benchmark: obtain or build a human-gold subset of the 876 overlap pairs so agreement figures (recall 0.49 / precision 0.42) can be restated as accuracy, or stay labelled as agreement.
10. Look-ledger audit: tally every consumed look's outcome and run a plant battery with known-true and known-null signals to estimate empirical false-pass and false-miss rates. Independently reproduce the 1.5-2% joint-null simulation.
11. Long-horizon design (Paul's call): a receipted contact log with stratified-random assignment (Terrier or CU outreach), the only route to testing seed_prob as take-up and pressure_mult as responsiveness (Chen, Cagala, Sverdrup).
12. Add evidence rows for the cited but unassessed sources: Imai & Kim 2021, Keele 2015, and Birkenmaier & Lechner 2025.
