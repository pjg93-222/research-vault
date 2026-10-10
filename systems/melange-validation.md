---
type: system
name: "Melange validation protocol and prospective tests"
slug: melange-validation
components:
  - melange-validation.single-look-discipline
  - melange-validation.gold-set-and-rank-gate
  - melange-validation.prospective-contract
  - melange-validation.sensitivity-and-mc
brain_notes:
  - C:/Users/pgoff/Documents/brain/concepts/single-look-holdout-discipline.md
  - C:/Users/pgoff/Documents/brain/concepts/single-look-holdout-discipline-look-ledger.md
  - C:/Users/pgoff/Documents/brain/concepts/gold-set-validation.md
  - C:/Users/pgoff/Documents/brain/concepts/zero-rank-delta-verification.md
  - C:/Users/pgoff/Documents/brain/concepts/trend-over-significance.md
paper_count: 15
tags: [system]
---

# Melange validation protocol and prospective tests

How claims in the Melange models are tested. Covers single-look holdout discipline, gold-set validation, the zero-rank-delta gate, the frozen forecast ledger, the prospective receptivity outcome contract (H1/H2), the receptivity sensitivity harness, Monte Carlo stability and method audits, and the F5 power gate.

Brain notes: [concepts/single-look-holdout-discipline](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline); [concepts/single-look-holdout-discipline-look-ledger](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline-look-ledger); [concepts/gold-set-validation](obsidian://open?vault=brain&file=concepts%2Fgold-set-validation); [concepts/zero-rank-delta-verification](obsidian://open?vault=brain&file=concepts%2Fzero-rank-delta-verification); [concepts/trend-over-significance](obsidian://open?vault=brain&file=concepts%2Ftrend-over-significance)

## Melange validation protocol and prospective tests: evidence assessment

**What this covers.** There are four components: (1) single-look holdout discipline, (2) the gold-set and zero-rank-delta gates, (3) the prospective receptivity contract (H1/H2) with its forecast ledger, and (4) the receptivity sensitivity harness, the MC stability check and the F5 power gate. The hub file has 17 evidence rows (by status: 8 VERIFIED, 9 VERIFIED-SOFTENED, 0 UNVERIFIED). Eight of them land on the prospective contract.

**Overall judgement.** On the methods it uses, the protocol is stricter than the papers it cites. Of the other papers, Marrero 2022 reports no permutation null, Cagala 2021 validates with 20-fold CV rather than a single look, and b-2025 reports no CI. The evidence supports that stance but does not *validate* the protocol. No row measures the protocol's real false-discovery rate. Several verified rows also show that the tests measure something narrower than their labels suggest:

- **H1 is not a test of seed_prob as a take-up probability.** Chen 2021 defines q as take-up given an invitation. H1 correlates seed_prob with cosponsorship that is not conditioned on any contact.
- **H2 may be partly mechanical.** Fouirnaies & Hall 2022 found that effort falls while positions stay flat. That is a state-legislature result, not a Congress result.
- **The stance benchmark measures agreement, not accuracy.** Its reference labels are 541 Kim GNN predictions plus 335 GPT-4 labels, 876 pairs in all.
- **The pass bar has already been failed once.** By the system's own ROBUST bar (top-20 overlap of 18 or more), the hand sweep of PERS_NONE fails at 2 of 5 settings.

**Errors to fix before any look is spent.** The rows flag these:

1. **The 150-mover threshold differs between code and protocol text.** The code counts members with a *positive* cosponsorship delta. PROTOCOL_TEXT says members with *any* delta. The Kalla and Sutton rows disagree on which one matches.
2. **The F5 figure "72 of 511 new-event members" could not be found.** The Kalla row searched the engine code and docs and did not find it.
3. **The component description puts the ROBUST bar in the wrong place.** The bar lives in `receptivity_sensitivity.py:113`. `mc_stability_check.py` sets no numeric pass bar.

These are single-agent findings (DE-062) until someone re-checks them in the code.

**Online scan.** These are leads only, not added to the evidence base:

- **Scheel, Schijen & Lakens 2021 (AMPPS).** About 44% of first hypotheses in Registered Reports are positive, against 96% in the standard psychology literature. This is a field-level analogue for the single-look gap.
- **Morris/Sobol global sensitivity screening.** The literature treats one-at-a-time sweeps as blind to interactions between parameters.
- **Validation-set design.** Sources say curated or oversampled gold sets cannot estimate prevalence-dependent metrics unless the scores are reweighted.

None of these was read in full, so none counts as a VERIFIED bearing.

## Single-look holdout discipline and look ledger

**What it computes.** Pass or miss of pre-registered confirmatory looks.

**Method.** The gate is pre-registered with hashes; a look is consumed once and never re-run; look fragments are void. A joint-null simulation gives about 1.5-2% false pass.

**Code refs.** (none listed)

**Assumptions.**
- Pre-registration prevents basis-shopping

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[weimer-2023-causal-approach-detecting]] | 2023 | supports | A Monte Carlo with zero true momentum (130,000 independent 50/50 scoring plays, a TV timeout after one play in every 50, substitutions not simulated) yields a null treatment coefficient (b = 0.002, SE 0.022, n = 1,336). The Fig. 6 caption calls it a negative control showing the matched design does not find an effect where none exists. | Supports the general practice of simulating under a known null to check that a selection or gating rule cannot manufacture a pass. Says nothing about the 1.5-2% figure or pre-registration; the brain simulation concerns look false-pass rates, not engine outcome estimators. | weak | VERIFIED |
| [[marrero-2022-evaluating-voice-samples]] | 2022 | qualifies | No standard test fits the repeated model and feature selection; uncertainty is handled by assuming a generalizable model improves with 100-fold CV (about 9 percent more training data per fold). No permutation null is reported. Post-hoc reliability-performance correlation t(4)=2.65, r=0.80, p=.057, falls to r=0.309 under leave-one-out. | The paper's effect sizes come from N=545 self-report students and have no null distribution, so they should not set a pass bar for an N=90, 88-feature legislator design without a permutation floor. This is consistent with the system's stricter rule. | moderate | VERIFIED |

**Evidence tally.** 2 rows, 2 VERIFIED; 2 distinct papers.

**What the evidence supports.** Two VERIFIED rows, both bearing on method rather than outcome.

**Weimer 2023 (VERIFIED, weak).** The paper runs a zero-momentum Monte Carlo: 130,000 simulated 50/50 plays, with n = 1,336 matched runs, giving b = 0.002 (SE 0.022). It works as a negative control.

- **What it supports:** the general practice of simulating under a known null to show that a gating rule cannot manufacture a pass.
- **What it does not support:** the 1.5-2% joint-null false-pass figure, pre-registration, or look-consumption rules. It is an observational study of NBA momentum.

**Marrero 2022 (VERIFIED, moderate).** The paper reports no permutation null. Its post-hoc reliability-performance correlation (r = 0.80, t(4) = 2.65, p = .057) falls to r = 0.309 under leave-one-out.

- **What it supports:** the system's decision not to borrow published effect sizes as pass bars without a permutation floor.
- **How the system already acts on it:** the audio-E result, r = +0.228 against a permutation floor of +0.234, correctly did not pass.
- **Transfer:** Marrero used N = 545 self-reporting students. The system's design is N of about 90 legislators with 88 features.

**Where the evidence stops.** No row measures the protocol's empirical false-discovery rate, which is the component's own empirical question. No row tests the assumption that pre-registration prevents basis-shopping either.

The 1.5-2% figure comes from the system's own joint-null simulation. It is a model-based rate under assumed independence, not an observed rate across the look ledger.

Weight of evidence: **supportive of practice, silent on performance.**

**Contradictions.**
- Paul cites Weimer et al. 2023 as a 'Monte Carlo negative control'. The paper does contain one, but its subject is NBA momentum. It supports only the generic idea of simulating under a known null, not the 1.5-2% false-pass figure or anything about look discipline.

**Gaps.**
- Empirical FDR of the protocol. Fill it with an internal audit: count every consumed look in the ledger as PASS, MISS or RETIRED, then compare the observed pass rate with the simulated joint-null rate. Add a planted-signal battery: inject known effects of graded size and record the detection rate. Report the window (ledger dates) and the denominator (number of looks).
- No external study tests the pre-registration-prevents-basis-shopping assumption in this setting. A meta-research comparison is the right kind of study. Online-scan lead: Scheel, Schijen & Lakens 2021 found 44% positive first hypotheses in Registered Reports against 96% in the standard literature. That is psychology, observational and confounded by topic selection, so it is a lead only.
- Imai & Kim 2021 and Keele 2015 are cited for this component but have no evidence rows. Their bearing is unassessed.

**Paul's position vs the evidence.**

Paul says the Causal & Methodological folder is unannotated and that the discipline is stricter than anything in the bibliography. The evidence agrees on strictness. Marrero has no null distribution, Cagala uses 20-fold CV rather than a single look, and b-2025 has no CI. Strictness is not the same as demonstrated calibration, though. Nothing in the evidence base, or in the system's outputs as described, shows the protocol's realised false-pass or false-miss rate. 'Stricter than the bibliography' is a fair description; 'validated' would not be.

**Open questions.**
- What is the observed PASS/MISS/RETIRED count across the look ledger, and how does the observed pass rate compare with the simulated 1.5-2%?
- Does the joint-null simulation assume independent panels? If the gold panels are correlated, how far does the false-pass rate move?
- What do Imai & Kim 2021 and Keele 2015 contribute? They are cited but unassessed.

**Read first.** [[weimer-2023-causal-approach-detecting]], [[marrero-2022-evaluating-voice-samples]], [[cagala-2021-optimal-targeting-fundraising]]

## Gold-set validation and zero-rank-delta gate

**What it computes.** Trust scores against owner-ruled gold sets, and a byte-identical ranks_python.json check for behaviour-preserving changes.

**Method.** Gold sets: 400 tweets, 760 press rows, Senate panels. Freshness: warn after 7 days, fail after 14.

**Code refs.** `fec/tests/run_tests.py`, `fec/audit/audit_full.py`

**Assumptions.**
- Gold labels are independent of the pipeline

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2025-measuring-interest-group]] | 2025 | qualifies | LLM labels agree with human coders at 96.93% accuracy and F1 97.19, but only on the 391 samples where two coders agreed. MapLight concordance is 91.03% (1,076 of 1,182 pairs) and covers Support/Oppose only; MapLight labels were also part of the GNN training labels. GNN best configuration: accuracy 78.51, F1 74.65; per-class F1 Support 79.19, Oppose 64.06, Engage 80.70. | The paper's human validation uses only coder-agreed lines, so its 96.93% does not carry over to ambiguous, Amend or Mention lines. The system benchmark measures agreement with Kim's labels, not accuracy: 541 of its 876 reference pairs are Kim GNN predictions (a stage with 78.51% test accuracy) and 335 are GPT-4 labels. Denominator: the 876-pair overlap (the benchmark file states no year window). UNVERIFIED as accuracy until a human-gold path reproduces it (DE-062). | strong | VERIFIED-SOFTENED |
| [[b-2025-comparing-fine-tuned]] | 2025 | method-source | Stance is a three-way task in which unrelated is a first-class label alongside agree and disagree. Per-class precision, recall and F1 are reported, with macro averages. | The labelling and metric conventions transfer across language and domain. The paper gives no cross-validation, no agreement statistic and no confidence interval, so it is a source for the convention only, not for validation rigour. | moderate | VERIFIED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | qualifies | The recommended validation has three steps: hand-label a small set to design the prompt, validate on a second hand-labelled set (by the team or crowd coders) for strong correlation with ground truth, then apply. Crowd workers increasingly use LLMs (Veselovsky et al. 2023), which threatens crowd-code independence. | The paper does not test anchoring on pre-labels, so this is a design implication, not a measured effect. If a labeller sees model_stance, agreement with gold partly measures agreement with the model. Oversampled hard slices make these accuracies incomparable with the paper random-sample 88.4 percent. | moderate | VERIFIED-SOFTENED |

**Evidence tally.** 3 rows, 1 VERIFIED, 2 VERIFIED-SOFTENED; 3 distinct papers.

**What the evidence says.** Three rows bear on gold-set design; none bears on the zero-rank-delta gate or the freshness rules. Those are engineering checks and need no literature.

**Kim 2025 (VERIFIED-SOFTENED, strong). The paper's headline accuracy does not cover the hard cases.**

- The 96.93% LLM-human accuracy is computed only on the 391 samples where both coders agreed. It says nothing about ambiguous, Amend or Mention lines.
- The system's benchmark uses the 876 overlapping (organisation, bill) pairs, with no year window stated. Of these, 541 are Kim GNN predictions (78.51% test accuracy) and 335 are GPT-4 labels.
- So the system scores (Support/Oppose recall 0.49, precision 0.42; Amend found 0 against Kim's 23) measure **agreement with another model's labels, not accuracy**. They stay UNVERIFIED as accuracy until a human-gold path reproduces them (DE-062).

**Ornstein 2024 (VERIFIED-SOFTENED, moderate). Three-step validation, and a threat to independence.**

- The paper recommends three steps: hand-label to design the prompt, validate on a second hand-labelled set, then apply.
- It also warns that crowd coders increasingly use LLMs, which threatens their independence.
- On the system side, `build_tweet_gold_queue.py` (lines 92-96) writes `model_stance` and `model_margin` into the same rows as the human `gold_*` columns.
- If labellers can see those columns, gold agreement partly measures agreement with the model.
- The queue also oversamples the low-margin and anti-bank slices (lines 25-27). Accuracies on that queue are therefore not comparable with random-sample figures such as Ornstein's 88.4%, and they cannot estimate live-row accuracy unless reweighted.

**b-2025 (VERIFIED, moderate).** It is a source for the three-way stance convention (with 'unrelated' as a class) and for macro-F1 only. It has no CV, agreement statistic or CI, so it adds nothing on rigour.

Weight of evidence: **these rows qualify the gold-set results more than they support them.**

**Contradictions.**
- The stated assumption is 'Gold labels are independent of the pipeline'. But the tweet gold queue writes model_stance and model_margin into the labelling rows (`build_tweet_gold_queue.py:92-96`). Unless the labelling UI provably hides those columns, the assumption is not established. This is a design risk; the code rows were read by a single agent.
- Paul highlights human-labelled validation in Kim J. et al. 2025. That human validation covers only the 391 coder-agreed samples. The system's benchmark against Kim then uses mostly Kim's machine labels (541 GNN predictions plus 335 GPT-4 labels), not Kim's human labels.

**Gaps.**
- The component's own empirical question has no evidence: how far gold-set accuracy differs from accuracy on randomly sampled live rows. Study needed: draw a simple or stratified random sample of live rows with recorded inclusion probabilities, have them labelled blind to model output, and compare with the curated-gold accuracy. Report the window and the denominator for each.
- Birkenmaier & Lechner 2025 is cited but has no evidence row, so its human-validation design is unassessed against the Senate-panel gold.
- The 400-tweet and 760-press-row gold sets have no inter-coder agreement figure among the evidence rows.

**Paul's position vs the evidence.**

Paul highlighted human-labelled validation in Birkenmaier & Lechner 2025 and Kim J. et al. 2025. The Kim evidence qualifies that endorsement. Its human check is restricted to easy cases, where coders agreed, and the system's comparison against Kim uses Kim's model labels, not its human ones. Ornstein adds that human gold is only as independent as the labelling interface. The highlighted principle is sound. The system's current gold sets and the Kim benchmark do not yet meet it in full.

**Open questions.**
- Does the tweet-labelling interface show `model_stance` or `model_margin` to the human labeller?
- Can the 876-pair Kim benchmark be restricted to Kim's human-coded subset? How many pairs survive?
- Are the oversampling weights for the Q_LOWMARGIN and Q_ANTIBANK slices recorded, so gold accuracy can be reweighted to the live distribution?

**Read first.** [[kim-2025-measuring-interest-group]], [[ornstein-2024-train-your-stochastic]], [[b-2025-comparing-fine-tuned]]

## Prospective receptivity contract (H1-H2) and forecast ledger

**What it computes.** H1: Spearman(seed_prob, new cosponsorship). H2: Spearman(pressure_mult, |tier movement|). Frozen cosponsor-join and dyad-persistence (p = 0.82) forecasts with Brier scores.

**Method.** Bars: >= .20 PASS, .10-.20 DIRECTIONAL, <= -.10 INDICTED. Needs at least 60 days and 150 movers. One look per contract.

**Code refs.** `fec/engine/outcome_contract.py`, `fec/engine/forecast_ledger.py`

**Assumptions.**
- Cross-sectional Spearman is a valid test

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[weimer-2023-causal-approach-detecting]] | 2023 | qualifies | The authors argue that comparing points before and after a coach-called timeout is fallacious because coaches often call timeouts to stop momentum. They use TV timeouts, which fall at predetermined times, and match runs interrupted by a TV timeout to uninterrupted runs on propensity scores (pairs; unmatched runs dropped). Result: -11.2% points over the next 3 minutes (b = -0.119, SE 0.012), n = 5,148 matched runs. Caveat from the same paper: Appendix E (as described in the main text) finds coaches' timeouts give largely similar, slightly larger effects, and the TV-vs-coach coefficient difference was not significant; Sec. 5.3 concedes TV timeouts may not be fully exogenous. | Design logic only, not effect size. Lobbyist-chosen contact is endogenous like a coach's timeout, so an engaged-only rho would confound pressure_mult with targeting unless contact timing is exogenous or matched on targeting covariates. The engaged-only arm is a subgroup correlation, not a before/after-contact comparison, so the analogy is to the selection problem, not an identical design. The paper's own Appendix E finding (coach vs TV timeouts not significantly different) means it does not empirically show the endogenous design is badly biased in its setting. Single observational study with matching. | moderate | VERIFIED-SOFTENED |
| [[fouirnaies-2022-electoral-incentives-affect]] | 2022 | qualifies | In the final term, effort falls (sponsorship -6%, committee activity -8%, participation -2.7 points) while positions stay flat (W-NOMINATE and interest-group nulls). | Moderate as a threat to interpretation. It applies only if tier movement is driven by evidence volume. If the contract is still unfrozen, a reported retiring/other-office stratum could be added. If it is frozen, any H2 result should carry this caveat. This is analysis, not a look, and stays UNVERIFIED until reproduced (DE-062). | moderate | VERIFIED |
| [[blackwell-2026-effect-political-advertising]] | 2026 | suggests-extension | Placebo tests check the identifying assumptions by applying the estimator to outcomes measured before treatment: the previous election's vote share and turnout, and the pre-election poll share. IPTW-FE estimates are near zero, while the naive approach shows residual confounding for some Democratic-ad effects. | The placebo logic carries over directly. A pre-registered negative control would be Spearman(seed_prob, cosponsorship delta over a window that ends before the freeze). A non-zero value would show that seed_prob tracks a stable member trait (a time-constant confounder) rather than responsiveness. Adding it changes the contract, so it needs Paul's ruling before the single look is spent, and it needs a second pre-freeze snapshot pair. Neither H1 nor H2 measures a lobbying treatment, so even a PASS remains an association. | moderate | VERIFIED |
| [[kalla-nd-campaign-contributions-facilitate]] |  | suggests-extension | The seniority of the official who attends responded to a randomized donor label across 191 offices over about three months. Cosponsorship as an outcome tells us little: nine legislators cosponsored in total, with the same share in both arms, and the authors say the estimate is too uncertain to bear on access effects. | The transfer concerns outcome choice, not effect size. A seniority-of-attendee outcome needs a real outreach log, which the system does not have, plus its own pre-registered single look. Any result stays UNVERIFIED until a second path reproduces it (DE-062). Do not use the 72 of 511 figure until it is located. | moderate | VERIFIED-SOFTENED |
| [[cagala-2021-optimal-targeting-fundraising]] | 2021 | suggests-extension | Policy gains are identified from randomized assignment through the AIPW score; identifying the causal effect of each covariate is not needed (Section 4.3, Appendix B). The 2014 rule's gains held in a caliper-matched 2015 sample but not in the full 2015 sample (Section 5.6, Table 8). The paper validates with 20-fold cross-validation rather than a single-look holdout (Section 5.3). | The paper shows the missing design: randomized treatment within strata, which is the precondition for upgrading from forecasting to spending-effect evidence. This is feasible for Terrier contacts or CU outreach logs and not for statutory PAC contributions to legislators. The paper's gains are not evidence for the system. | weak | VERIFIED |
| [[chen-2021-contingency-aware-influence]] | 2021 | qualifies | q is the probability that a selected node is willing to act as a seed, conditional on being selected or invited. It is assumed known a priori, so it is a take-up rate, not an unconditional outcome. | Unconditional new cosponsorship mixes take-up with contact choice and base activity. A pass would not validate seed_prob as a contingency probability, and a miss would not refute it. The construct match needs an engaged-only arm built on a contact log. | moderate | VERIFIED-SOFTENED |
| [[sverdrup-nd-introduction-grf-causal-forests]] |  | qualifies | Effect heterogeneity is tested by ranking on a score and computing the gain over the ATE in the top-ranked fraction (TOC/AUTOC), which requires treated and untreated units. The authors warn that out-of-bag TOC is a heuristic without finite-sample guarantees and that policy trees need a held-out set (pp.8-11). | Effect-modification reading applies only if CU outreach or money exposure is logged per member with plausibly known assignment. Otherwise H1 is a predictive-validity check. The held-out, single-look design is consistent with the authors' advice, but this is a judgment, not something the paper tests. | moderate | VERIFIED |
| [[sutton-2018-reinforcement-learning-introduction]] | 2018 | suggests-extension | Before deploying a new policy, Adobe (Theocharous et al.) estimated its value from data logged under other policies, using high-confidence off-policy evaluation with probabilistic improvement guarantees. Logs: about 200,000 interactions from a campaign that randomly offered one of 7 offers, and about 4,000,000 interactions over 12 offers. The test set was real interactions served by a random policy. | Off-policy estimates for alternative allocations need logged actions with known or estimable propensities and overlap. I did not confirm that funding and contact are logged without propensities beyond the missing contact contract. The claim that real assignment is strategic and near-deterministic is the analyst's assertion and has no support in the code I read. Any such estimate stays UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED-SOFTENED |

**Evidence tally.** 8 rows, 4 VERIFIED, 4 VERIFIED-SOFTENED; 8 distinct papers.

**What the evidence says.** This component has the most evidence: 8 rows. Almost all of them qualify the contract or suggest extensions; none supports it as written.

**H1: construct mismatch.** Chen 2021 (VERIFIED-SOFTENED, moderate) defines q as the probability that a node takes up the seed role *given that it was invited*.

- H1 instead correlates seed_prob with new cosponsorship that is not conditioned on any contact.
- So a PASS would not validate seed_prob as a take-up probability, and a MISS would not refute it.
- A real construct test needs the engaged-only arm. That arm is currently `NOT_IMPLEMENTED_NO_CONTACT_CONTRACT`.
- sverdrup/grf (VERIFIED) agrees: testing effect modification needs treated and untreated units. Without them, H1 is a predictive-validity check only.

**H2: could pass for the wrong reason.** Fouirnaies & Hall 2022 (VERIFIED, moderate) studies **U.S. state legislatures, not Congress**. It finds that effort falls in the final term (sponsorship -6%, committee activity -8%), while W-NOMINATE and interest-group positions stay flat.

- pressure_mult carries a 0.60 retiring factor, and tier movement depends on new evidence events.
- A positive H2 could therefore arise from differences in activity volume rather than from responsiveness.
- The paper's own prediction is that no real position change comes through this channel.
- Whether this is a mechanical artefact here is an analysis lead (DE-062), not a finding.

**Design extensions the evidence suggests.**

- **Pre-freeze placebo** (Blackwell 2026, VERIFIED, moderate): correlate seed_prob with cosponsorship change over a window that ends before the freeze. A non-zero placebo would show seed_prob tracking a stable member trait. Adding it changes the contract, so it needs Paul's ruling before the look is spent.
- **Pre-registered outcome windows** (Weimer 2023, softened): the effect estimate decayed as the post-treatment window lengthened, from 60 s out to 4 minutes. The contract's window is just whatever elapses after 60 days; a small pre-locked set of windows would block basis-shopping. The NBA time scale does not transfer.
- **Endogenous contact** (Weimer 2023): lobbyist-chosen contact works like a coach's timeout, so a future engaged-only arm would confound pressure_mult with targeting unless contact is matched or exogenous.
- **Randomization or logged propensities** (Cagala 2021, Sutton 2018, Kalla n.d.): randomized or propensity-logged treatment is the precondition for any spending-effect or policy-value claim. `CLAIM_SCOPE = receptivity_forecast_only_not_spending_effects` already respects this.
- **Cosponsorship is a thin outcome** (Kalla n.d.): nine cosponsors in total across 191 offices.

Weight of evidence: **the contract is honestly scoped as association-only. As specified, its tests cannot discriminate the constructs their labels name.**

**Contradictions.**
- Threshold inconsistency, to fix before the look. The Sutton row reports that the code counts 'movers' as members with a *positive* cosponsorship delta, while PROTOCOL_TEXT says members with *any* delta. The Kalla row says the thresholds match PROTOCOL_TEXT at `outcome_contract.py:17-20`. The two rows disagree, so the code and the text need reconciling. Single-agent reads (DE-062).
- Fouirnaies & Hall 2022 is cited for the retiring multiplier and labels H2 ('Byrne/F&H'). But it is a state-legislature study, and it finds positions do *not* change in the final term. That is the opposite of a stance-movement channel, so it cannot support H2 as a test of responsiveness.
- Paul cites Chen et al. 2021 for seed take-up. Chen's q is a probability conditional on being invited, and H1 tests unconditional adoption. The citation and the test measure different quantities.

**Gaps.**
- No evidence on whether cross-sectional Spearman is a valid test of the receptivity constructs, which is this component's stated assumption. Study type: a pre-registered negative control (the pre-freeze placebo), plus a construct test on a receipted contact log with known assignment.
- No evidence on dyad-persistence (p = 0.82) or on the cosponsor-join forecasts scored by Brier. A study would compare their calibration against a naive baseline, such as last-period persistence, over a named window and denominator.
- H2's predictor uses Byrne et al. 2015 for susceptibility, and that link has no evidence row here.

**Paul's position vs the evidence.**

Paul has no annotation on this component; he cites Chen et al. 2021 and Fouirnaies & Hall 2022. The evidence turns both citations into caveats rather than support:

- **Chen 2021:** its quantity is a conditional take-up rate, not what H1 measures.
- **Fouirnaies & Hall 2022:** a state-legislature result predicting effort change without position change.

The contract's CLAIM_SCOPE (forecast only, not spending effects) matches what the evidence would allow.

**Open questions.**
- Is the contract frozen? If not, should the pre-freeze placebo and a retiring/other-office stratum be added? Both change the contract and need Paul's ruling before the look.
- Which mover definition is authoritative, positive delta or any delta?
- Does tier movement scale with evidence-event volume? If it does, H2 needs an activity covariate or stratum.
- Is a receipted contact log feasible, for Terrier or CU outreach, to enable the engaged-only arm?

**Read first.** [[chen-2021-contingency-aware-influence]], [[fouirnaies-2022-electoral-incentives-affect]], [[blackwell-2026-effect-political-advertising]], [[weimer-2023-causal-approach-detecting]], [[sverdrup-nd-introduction-grf-causal-forests]], [[kalla-nd-campaign-contributions-facilitate]]

## Receptivity sensitivity harness, MC stability and F5 power gate

**What it computes.** Top-20/50 target overlap under constant sweeps; seed-set stability across MC budgets; H1/H2 power (requires at least 150 new-event members).

**Method.** ROBUST at top-20 overlap >= 18. The F5 replay found 72 of 511 new-event members (UNDERPOWERED; look unspent).

**Code refs.** `fec/members/receptivity_sensitivity.py`, `fec/engine/mc_stability_check.py`, `fec/engine/method_audit.py`

**Assumptions.**
- A robust target set means the constant cannot change a decision

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[weimer-2023-causal-approach-detecting]] | 2023 | suggests-extension | Appendix D (described in the main text) re-tallies post-treatment points over windows from 60 s to 4 minutes in 10 s steps; the negative effect persists but the point estimate steadily decreases as the window lengthens, and there is no significant effect for points scored between minutes 3 and 4. The first-minute effect is about four times the 3-minute effect. | Suggests pre-registering a small set of outcome windows inside the contract lock rather than choosing one afterward (basis-shopping). Minute-scale decay says nothing about congressional time scales. | weak | VERIFIED-SOFTENED |
| [[harrison-2010-introduction-monte-carlo]] | 2010 | context | The paper reports no error analysis, convergence diagnostic or comparison of samplers. On randomness quality it says LCGs do not do well on many Diehard-type tests and that the Mersenne Twister is becoming the generator of choice, passing most tests except some for encryption RNGs. | The system goes further than the source. The paper can endorse the generator choice but supplies no criterion for MC budget or stability. The ROBUST bar (top-20 overlap >= 18) lives in fec/members/receptivity_sensitivity.py:113, a constant-sensitivity check, not in mc_stability_check.py; any MC-stability bar would have to come from elsewhere (the script's docstring cites Almazaydeh et al. 2026 as motivation only). | weak | VERIFIED-SOFTENED |
| [[linde-2016-politicians-take-risks]] | 2016 | suggests-extension | Outside the Asian disease problem, the political-scenario reflection effect is weak: only the votes+economic scenario shows it (85% vs 69.23%, Fisher p=0.302; significant only as a scenario interaction in the pooled regression, OR 8.684, p=0.033), and the paper reports no loss aversion. Effect direction for an electoral pt domain is therefore uncertain, and the size of LOSSFRAME is unsupported. | Applies directly. Adding a pt = 1.0/1.0 arm and a LOSSFRAME = 1.0 arm to the existing top-20/50 overlap harness would show whether either constant changes targeting decisions. Diagnostic sweep, not a confirmatory look. | moderate | VERIFIED-SOFTENED |
| [[jacoby-2025-mathematical-modeling-analysis]] | 2025 | suggests-extension | Varying the centrist-flow parameters by 10% produces a wide spread of final C, Y and N counts. In the parameter-impact ranking, centrist-lobbying parameters (M_LF, M_LA) and tau rank high and initial compartment sizes rank low. Linear SVMs on 1000-sample sweeps find no clean pass/fail boundary in multi-parameter sweeps. | The transfer is a method idea, not evidence. Jacoby's sensitivity comes from a well-mixed ODE model with literature-fixed parameters, and the system's flip model is a network-based ratio of saturating terms. What carries over is the diagnostic: the parameters that decide who is movable and how fast they move should be swept first. The pass/fail SVM is not needed, because top-k overlap already measures decision change. Any such sweep is a bound on the model, not validation (DE-062). | moderate | VERIFIED |

**Evidence tally.** 4 rows, 1 VERIFIED, 3 VERIFIED-SOFTENED; 4 distinct papers.

**What the evidence says.** Four rows, all suggestions or context. No row supports the harness as sufficient.

**Jacoby 2025 (VERIFIED, moderate).** In Jacoby's model, the centrist-flow and lobbying parameters (M_LF, M_LA) and tau rank high in impact, and outcomes spread widely under +-10% perturbations.

- The transfer is a method idea only. Jacoby's model is a well-mixed ODE; Melange's flip model is network-based.
- The method idea is that the parameters deciding who is movable should be swept first.
- The harness does not sweep the flip-model channel weights WM/WP (0.35/0.65) or the PERS persuadability table.
- The one centrist-like knob swept by hand, PERS_NONE over 0.30-0.95, gave top-20 overlap of 16/18/20/18/15 and total winnability of 149.5 to 245.8.
- **By the system's own bar (overlap of 18 or more is ROBUST), 2 of 5 settings fail.**

**Linde & Vis 2016 (VERIFIED-SOFTENED, moderate). Dutch MPs, not Congress.**

- The reflection effect in political scenarios is weak. In the votes+economic scenario it was 85% vs 69.23%, Fisher p = 0.302. It reached significance only as a pooled interaction (OR 8.684, p = 0.033).
- The paper reports no loss aversion.
- The sweep varies PT only between mild (1.10/0.95) and strong (1.25/0.85). It has no PT-off (1.0/1.0) arm, no reversed arm, and never sweeps LOSSFRAME (1.10).
- Those are the constants with the weakest literature support, which answers this component's empirical question in part. Adding those arms is a diagnostic sweep, not a look.

**Harrison 2010 (softened, weak).** The paper backs Mersenne Twister as a generator but gives no MC-budget or stability criterion. The edge draws use splitmix64, which the paper does not cover. `mc_stability_check.py` reports Jaccard overlap at MC = 20 and 100 across seeds 7/1/13 **with no numeric pass bar**.

**Weimer 2023 (softened, weak).** Suggests adding an outcome-window sweep. Its time scale does not transfer.

**F5 power gate.** The figure of 72 of 511 new-event members (UNDERPOWERED; look unspent) could not be found in the engine code or docs (Kalla row). Treat it as unlocated until a receipt is found.

Weight of evidence: **the harness asks the right question (does a constant change a decision?) but sweeps the wrong or too few constants, one at a time.**

**Contradictions.**
- The assumption is 'A robust target set means the constant cannot change a decision'. The only recorded sweep of a persuadability constant (PERS_NONE) fails the ROBUST bar at 2 of 5 settings (overlap 16 and 15), and winnability varies by about 64%. That constant can change decisions.
- The component description says 'ROBUST at top-20 overlap >= 18'. That bar belongs to `receptivity_sensitivity.py:113`, not to the MC stability check, which has no pass bar. The description conflates the two.
- The F5 replay figure (72 of 511 new-event members) is stated as fact in the component description, but the Kalla row did not find it in the engine code or docs searched.

**Gaps.**
- No numeric MC-stability criterion. The kind of study needed is a convergence study: plot seed-set Jaccard and spread delta against MC budget (e.g. 20, 50, 100, 200, 500) under common random numbers, then set the bar where marginal change falls below a pre-stated tolerance.
- No interaction-aware sweep. Online-scan lead: Morris elementary-effects screening, followed by Sobol total-effect indices on top-20 overlap. Morris costs roughly 10 model evaluations per parameter, so it is affordable for the receptivity and flip constants.
- Even a full sweep only bounds the model; it does not validate it (DE-062). External validity still rests on the prospective contract.

**Paul's position vs the evidence.**

Paul highlighted Jacoby's finding that outcomes are highly sensitive to centrist-flow parameters. The system's own hand sweep of PERS_NONE reproduces that sensitivity pattern, not in Jacoby's model but in Melange's. Yet the harness still omits WM/WP, the PERS table, a PT-off arm and LOSSFRAME. So the highlight is not yet acted on where it matters most. Jacoby's SVM boundary method is unnecessary here, because top-k overlap already measures decision change.

**Open questions.**
- Should WM/WP, the PERS table, PT = 1.0/1.0 and LOSSFRAME = 1.0 be added to the VARIANTS in `receptivity_sensitivity.py`? That is a diagnostic only, but it changes the harness.
- Where is the 72 of 511 F5 receipt, and what window and denominator does it use?
- What numeric bar should `mc_stability_check.py` enforce, and at what MC budget?

**Read first.** [[jacoby-2025-mathematical-modeling-analysis]], [[linde-2016-politicians-take-risks]], [[harrison-2010-introduction-monte-carlo]], [[weimer-2023-causal-approach-detecting]]

## Across components

## Cross-component patterns

**1. The protocol is honest about scope but thin on construct validity.** Each test is narrower than its label:

- **H1:** correlates with cosponsorship that is not conditioned on contact, so it is not a test of take-up.
- **H2:** may move with activity volume rather than responsiveness.
- **Stance benchmark:** agreement with Kim's model labels, not accuracy.
- **ROBUST:** decided by one-at-a-time sweeps of the wrong constants.

The fixes share one ingredient: an **independent reference with known provenance**. That means a receipted contact log with known assignment, a blind random-sample gold set, and an interaction-aware sweep.

**2. A receipted contact log would unblock several items at once.**

- The H1 engaged-only arm (Chen 2021, grf).
- The H2 targeting confound (Weimer 2023).
- Any policy-value estimate (Sutton 2018, Cagala 2021).
- A seniority-of-attendee outcome (Kalla n.d.).

**3. Transfer conditions recur.**

- Fouirnaies & Hall: U.S. state legislatures.
- Linde & Vis: Dutch MPs.
- Weimer: NBA, on a minute time scale.
- Jacoby: a well-mixed ODE model.
- Marrero: student self-reports.

None of these is a Congress result. Every receptivity constant whose citation comes from a non-Congress source should be read as a design prior.

**4. Housekeeping errors.** These are single-agent reads (DE-062); reconcile them before any look:

- The mover threshold (positive vs any delta) differs between code and PROTOCOL_TEXT.
- The F5 72 of 511 figure is unlocated.
- The component text attributes the ROBUST bar to the MC check, which has no bar.

**5. Out of scope here.** No paper in this evidence set argues for dropping regular cosponsorship edges or for per-edge volume normalization, and none bears on reciprocity. The D-104..D-108 status (reciprocity not validated) is unaffected.

## Research agenda

1. Reconcile the H1 mover definition (positive delta in code vs any delta in PROTOCOL_TEXT) before the look is spent.
2. Find the receipt for the F5 replay (72 of 511 new-event members), with its window and denominator, or strike the figure.
3. Get Paul's ruling on adding a pre-freeze placebo (Spearman of seed_prob with pre-freeze cosponsorship change) and a retiring/other-office stratum for H2. Both change the contract, so they need the ruling before the single look.
4. Sweep WM/WP, the PERS table, PT = 1.0/1.0 and LOSSFRAME = 1.0 in the receptivity sensitivity harness, scored by top-20/50 overlap. This is diagnostic only.
5. Run a Morris screen, then Sobol total-effect indices, on top-20 overlap so parameter interactions are captured. This is a lead from the online scan.
6. Set a numeric pass bar for mc_stability_check.py from a convergence study over MC budgets under common random numbers.
7. Audit gold-set independence: confirm whether labellers see model_stance; label a blind random sample of live rows with inclusion weights and compare with curated-gold accuracy.
8. Restrict the Kim benchmark to Kim's human-coded pairs, then report agreement and accuracy separately, each with its denominator.
9. Tally the look ledger (PASS/MISS/RETIRED with dates and counts) and compare it with the 1.5-2% simulated joint-null rate. Add a planted-signal battery.
10. Design a receipted contact log (Terrier or CU outreach) with randomized or logged assignment. It is the shared precondition for the engaged-only arm, any policy-value estimate and the construct test of seed_prob.
11. Assess the unannotated citations Imai & Kim 2021, Keele 2015 and Birkenmaier & Lechner 2025 against these components. Read Scheel, Schijen & Lakens 2021 (AMPPS) as a field-level lead on what pre-registration does to positive-result rates.
