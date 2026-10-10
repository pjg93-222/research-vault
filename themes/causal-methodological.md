---
type: theme
name: "Causal & Methodological"
paper_count: 14
tags: [theme]
---
# Causal & Methodological

**Causal & Methodological** is a methods toolkit, not a body of substantive findings. The 14 works fall into three clusters. The first asks when a strategically chosen treatment can be read causally: [[weimer-2023-causal-approach-detecting]], [[blackwell-2026-effect-political-advertising]] and [[imai-2021-fixed-effects-regression]]. The second supplies the variance and simulation machinery behind such estimates: the three Zeileis sandwich papers and [[harrison-2010-introduction-monte-carlo]]. The third asks whether a measurement or targeting choice quietly decides the answer: [[hargittai-2015-bigger-always-better]], [[deluca-2022-validating-applicability-bayesian]], [[becker-nd-gameability-redistricting-criteria]], [[poole-1999-nominate-after-years]], [[cagala-2021-optimal-targeting-fundraising]] and [[sverdrup-nd-introduction-grf-causal-forests]]. [[anon-nd-house-committee-requests]] (identified by Paul as Frisch & Kelly 2007) was not read: extraction returned 0 characters.

**Paul's own notes are thin for this theme.** The theme's annotated bibliography is five bare citations: Imai & Kim, Keele, Blackwell & Yamauchi, Poole & Rosenthal, and Fouirnaies & Hall. It has no takeaways, and the annotation prose was probably lost in the docx-to-markdown conversion. The richer material is Paul's project documents. The sector-growth proposal sets out hypotheses H1-H4: a pooled sector-FE panel, about 9 elections over 2008-2024, 5 clusters, and a wild cluster bootstrap. The contact-inference evaluation plan and the contact-identification state record frame their claims explicitly as ranking and calibration, not causal effects, and use the phrase "inferred contact plausibility". The approach-brief handoff says setting differences are "descriptive, never causal".

**Paul's documents and the literature agree on the central lesson.** Without a designed source of variation (randomization, a rule-fixed natural experiment, or a falsification check), the system's outputs are forecasts and heuristic rankings, not effects. The theme bears most directly on the Mentat sector-growth regressions and on the Melange prospective validation contract.

## Strands

### Identifying effects of strategically chosen treatments

**The shared problem is a treatment chosen in response to the outcome.** In [[weimer-2023-causal-approach-detecting]], a coach calls a timeout because of momentum. In [[blackwell-2026-effect-political-advertising]], an interest group targets ads at persuadable markets. In [[imai-2021-fixed-effects-regression]], panel units switch treatment on and off. The three works answer the problem differently.

- **Rule-fixed natural experiment (Weimer).** TV timeouts fall at points the rulebook fixes, so they act as near-random pauses. Runs interrupted by one are matched 1:1 on propensity scores to runs with none. Over NBA seasons 2004-05 to 2016-17 (5,148 matched runs), a TV timeout cuts the running team's points in the next 3 minutes by 11.2% (b = -0.119). A Monte Carlo placebo with zero true momentum returns b = 0.002 (n = 1,336).
- **Weighting with unit fixed effects (Blackwell & Yamauchi).** Unit fixed effects go into the propensity model, estimated by conditional MLE, which then supplies weights for a marginal structural model (IPTW-FE). Data are Senate and gubernatorial general elections, 2010-2020: 467 market-races for Democratic-group ads and 623 for Republican. Placebo outcomes come out near zero. The effects hold only at alpha 0.1, and the text gives no point estimates.
- **Proof that the default fails (Imai & Kim).** Linear two-way fixed effects (2FE) is a matching estimator with mismatches. It equals difference-in-differences only in the 2-group, 2-period case. Beyond that, some control observations get negative weights.

**All three agree on the core point.** Linear regression on observational panels does not identify a design-based effect without extra structure, and a falsification check (simulated null or pre-treatment placebo) is cheap and informative.

[[anon-nd-house-committee-requests]] would add the selection version of the problem: did members ask for the committee seat that is later credited as leverage? It is unread, so it carries no weight.

Works: [[weimer-2023-causal-approach-detecting]], [[blackwell-2026-effect-political-advertising]], [[imai-2021-fixed-effects-regression]], [[anon-nd-house-committee-requests]]

### Robust variance and few-cluster inference

**The three papers form one software lineage:** [[zeileis-nd-econometric-computing-with]] (2004), then [[zeileis-nd-object-oriented-computation]] (2006), then [[zeileis-nd-various-versatile-variances]] (2020).

- **2004: HC and HAC errors.** These become one sandwich form. Worked examples show that the choice of estimator can flip significance. On 50 US states (1979 school spending), one high-leverage state (Alaska) moves a p-value from 0.056 under HC0 to 0.77 under HC4. On the Greene investment series, HAC lag and prewhitening settings move a standard error by about 40%.
- **2006: the bread-and-meat abstraction.** One computation now serves any model with estimating functions. It also carries the key caveat: sandwich errors help only if the point estimates stay consistent, and they cost power when the model is correct.
- **2020: clustered and panel covariances, with Monte Carlo coverage evidence.** In 10,000 replications, CL-0 coverage is near 0.95 once G is about 50 to 100. At G = 10, standard errors are underestimated (best to worst: HC3, HC2, HC1, HC0). Panel Newey-West and PCSE fail with short panels.

**The strand corrects variances, not estimands.** Its own caveat lines up with [[imai-2021-fixed-effects-regression]]: a robust SE on a model-dependent 2FE coefficient is a robust SE on the wrong target.

Works: [[zeileis-nd-econometric-computing-with]], [[zeileis-nd-object-oriented-computation]], [[zeileis-nd-various-versatile-variances]]

### Heterogeneous effects and targeting rules

**The pair share a method:** AIPW doubly robust scores, causal-forest heterogeneity tests, and depth-limited policy trees in the Athey-Wager line. [[sverdrup-nd-introduction-grf-causal-forests]] is the tutorial, and [[cagala-2021-optimal-targeting-fundraising]] is a field application with costs netted out.

**Cagala et al. are the 2014 German church-fundraiser experiment (N = 19,779).** Sending the gift to everyone loses 0.97 euro per person on the cold list (N = 17,425). A learned rule gives the gift to 33% of the warm list (N = 2,354) and raises net donations by about 2.1-2.2 euro (about 14%) over the all-gift, no-gift and random benchmarks under 20-fold cross-validation. Applied to the full 2015 sample, the rule's gain was not significant. It helped only where 2015 donors closely matched 2014 donors.

**The grf tutorial supplies the test a ranking must pass.** RATE (AUTOC) asks whether a prioritization rule concentrates the treatment effect. In the payday example (about 2,500 people), held-out AUTOC is 0.0061 (SE 0.0092), meaning no heterogeneity. The authors call out-of-bag TOC tests heuristics without finite-sample guarantees.

**Both works agree on three conditions.** Targeting is justified only by a held-out comparison against uniform and random benchmarks. It requires randomized or known-propensity treatment assignment. It may not transport to a new population.

Works: [[cagala-2021-optimal-targeting-fundraising]], [[sverdrup-nd-introduction-grf-causal-forests]]

### Measurement, sampling frame and metric choice

**Four works make one argument from different directions:** an upstream measurement choice the analyst barely notices can decide the answer.

- **Sampling frame ([[hargittai-2015-bigger-always-better]]).** Platform adoption is non-random by age, education, income, gender and race. In the 2013 Pew survey (N = 1,801), the LinkedIn coefficient on logged income is 0.72. A one-site frame can reverse a finding's direction, not just add noise.
- **Aggregation rule ([[deluca-2022-validating-applicability-bayesian]]).** Summing posterior probabilities (PSM) beats modal assignment (PM). Across 10,000 simulated plans per state (North Carolina and Georgia, Black/White only), PSM keeps the Black-share error at or below about 5 points.
- **Metric definition ([[becker-nd-gameability-redistricting-criteria]]).** County-split and least-change measures crown different maps. In Minnesota (11 maps) and Wisconsin (6 maps), no map is best on both. The analysis is descriptive and hand-built, and the study was funded by Jenner & Block.
- **Scaling baseline ([[poole-1999-nominate-after-years]]).** Over 1789-1998, legislator positions are near-fixed: the linear trend adds about 0.3 classification points over a constant position. The second dimension now adds under 1%, and party overlap on dimension 1 is under 1.5% over the last five Congresses. Any member-level signal is therefore heavily party-loaded.

None of the four is causal. They are validity checks on the inputs that causal or predictive claims rest on.

Works: [[hargittai-2015-bigger-always-better]], [[deluca-2022-validating-applicability-bayesian]], [[becker-nd-gameability-redistricting-criteria]], [[poole-1999-nominate-after-years]]

### Simulation as method and as falsification

**[[harrison-2010-introduction-monte-carlo]] is a tutorial and history, not evidence.** It gives the recipe (model as probability densities, then sample, then tally), covers generator choice (Mersenne Twister over linear congruential generators), and covers inversion and acceptance-rejection. Its warnings: simulation gives no exact answers, is only as good as its inputs, is prone to bugs, and analytic alternatives should be tried first.

**Two applied uses show what the recipe is for.** [[weimer-2023-causal-approach-detecting]] uses Monte Carlo as a negative control on its own selection rule. [[zeileis-nd-various-versatile-variances]] uses 10,000 replications to measure estimator coverage.

**The applied works treat simulation as a test of an estimator, not a source of truth about the world.** That is the distinction that matters for the Melange cascade engines, which use Monte Carlo as the estimator itself.

Works: [[harrison-2010-introduction-monte-carlo]], [[weimer-2023-causal-approach-detecting]], [[zeileis-nd-various-versatile-variances]]

## Agreements and debates

- **Correcting the estimand versus correcting the variance.** [[imai-2021-fixed-effects-regression]] says the problem with 2FE is the estimand: weights are model-dependent and some are negative. The Zeileis line ([[zeileis-nd-object-oriented-computation]], [[zeileis-nd-various-versatile-variances]]) fixes standard errors and warns that this helps only if the point estimates are consistent. They do not contradict each other, but they answer different failures. A study that adds clustered or HAC errors to a 2FE fit has addressed only the second.
- **How to keep unit fixed effects.** [[imai-2021-fixed-effects-regression]] points toward matching and weighted estimators that drop mismatches. [[blackwell-2026-effect-political-advertising]] cites Imai & Kim (2019) and keeps unit fixed effects, but moves them into a propensity model for inverse-probability weights. Its own 500-replication simulations show residual bias when heterogeneity is high, and it is an unpublished manuscript (9 Feb 2026). The two agree that linear 2FE is not design-based but disagree on the repair.
- **Natural experiment, randomization, or adjusted observation.** [[weimer-2023-causal-approach-detecting]] relies on a rule-fixed pause. [[cagala-2021-optimal-targeting-fundraising]] and [[sverdrup-nd-introduction-grf-causal-forests]] require randomized assignment. [[blackwell-2026-effect-political-advertising]] adjusts observational data for time-constant unmeasured confounding. The ordering of credibility is implicit and shared, but only Blackwell attempts the observational case, and its significance is at alpha 0.1.
- **Whether a learned targeting rule generalizes.** [[cagala-2021-optimal-targeting-fundraising]] shows a rule that wins out of sample in 2014 and shows no significant gain on the full 2015 sample. [[sverdrup-nd-introduction-grf-causal-forests]] flags out-of-bag TOC tests as heuristics. Both say held-out evaluation is necessary, and Cagala shows it is not sufficient across years.
- **How many clusters are enough.** [[zeileis-nd-various-versatile-variances]] finds CR-type errors anti-conservative at G = 10, its smallest tested size. Paul's sector-growth proposal has 5 clusters (5 to 9 sectors in the live study) and chooses a wild cluster restricted bootstrap with Webb weights. The paper tests only a variance bootstrap (vcovBS), which does only somewhat better than HC0/HC1 at G = 10. So the theme neither validates nor refutes Paul's choice. It is an open question, not a settled one.
- **Measurement choice as researcher degrees of freedom.** [[hargittai-2015-bigger-always-better]] (frame), [[deluca-2022-validating-applicability-bayesian]] (aggregation rule) and [[becker-nd-gameability-redistricting-criteria]] (metric definition) agree that the choice can reverse or reorder results. Becker goes further and recommends explicit metric definitions without hard numeric caps.
- **Internal inconsistencies flagged in the sources:**
  - [[weimer-2023-causal-approach-detecting]]: the mean-reversion coefficient in the text (.001) disagrees with Table 3 (-0.002, -0.003, -0.018).
  - [[blackwell-2026-effect-political-advertising]]: the Trump-era split is dated 2015 on p.30 and 2016 elsewhere.
  - [[becker-nd-gameability-redistricting-criteria]]: the text says Plan 9 moves 0.03% more people than Plan 8, while Table 3 shows 1.90% against 1.93%.
- **No paper in this theme argues for either rejected SNA proposal** (dropping regular cosponsorship edges, or per-edge volume normalization). If a later reading draws that inference from [[hargittai-2015-bigger-always-better]]'s frame argument, flag it: both proposals were measured and REJECTED.

## Methods

**Dominant methods and their weak points**

| Method | Works | Main weakness |
|---|---|---|
| Propensity matching or weighting on a natural experiment or observational panel | [[weimer-2023-causal-approach-detecting]], [[blackwell-2026-effect-political-advertising]] | Unmeasured confounding remains. Weimer concedes strategy changes during timeouts. Blackwell removes only time-constant confounding, keeps bias at high heterogeneity, reports effects significant only at alpha 0.1, and gives no point estimates in the text. |
| Algebraic proof, no data | [[imai-2021-fixed-effects-regression]] | Binary treatment only. No simulation or application, so it shows how large the problem can be without measuring it in any real panel. |
| Randomized field experiment plus causal ML (AIPW, causal forests, policy trees, RATE) | [[cagala-2021-optimal-targeting-fundraising]], [[sverdrup-nd-introduction-grf-causal-forests]] | Needs logged randomized or known-propensity assignment. Cagala is one city and one charity, and the rule does not transport to the full 2015 sample. The grf out-of-bag tests are heuristics. The SUTVA assumption rules out the spillovers a network system is built on. |
| Validation against ground truth with simulated ensembles | [[deluca-2022-validating-applicability-bayesian]] (10,000 plans per state) | Two highly segregated states, two racial groups, ZIP priors. Generality is unknown. |
| Descriptive metric comparison | [[becker-nd-gameability-redistricting-criteria]] | No inference. Grid plans are hand-built and the optimizer runs are not representative samples. Study funded by a litigating firm. |
| Logistic regressions on survey and panel data | [[hargittai-2015-bigger-always-better]] | 2013 and 2009-2012 general-population and student data with R-squared .03-.16. The coefficients do not carry to elite accounts; only the frame logic does. |
| Spatial scaling by maximum likelihood | [[poole-1999-nominate-after-years]] | The authors concede the standard errors are not econometrically correct. The donor and primary claim rests on an uncited working paper. |
| Robust-covariance software and Monte Carlo coverage studies | [[zeileis-nd-econometric-computing-with]], [[zeileis-nd-object-oriented-computation]], [[zeileis-nd-various-versatile-variances]] | Smallest tested G is 10. The wild cluster restricted bootstrap-t is not evaluated. The 2004 and 2006 small-sample claims are borrowed from other studies. |
| Tutorial | [[harrison-2010-introduction-monte-carlo]] | No evidence. Nothing on common random numbers, Monte Carlo standard errors or convergence. |

**Recurring good practice:**
- falsification by simulated null or pre-treatment placebo (Weimer, Blackwell);
- held-out comparison against uniform and random policies (Cagala, grf);
- checking against a second implementation (Zeileis reproduces multiwayvcov, plm and pcse to six decimals).

That last one is the literature's analogue of the project's second-path rule (DE-062).

**Recurring gap:** no work here reports a sensitivity analysis for unmeasured confounding beyond placebos.

## How the works relate

- [[weimer-2023-causal-approach-detecting]] — complements — [[imai-2021-fixed-effects-regression]]: Imai & Kim recast 2FE as a matching estimator and show its causal limits; Weimer uses propensity matching on a rule-fixed natural experiment. Both govern when an observational contrast can be read causally.
- [[weimer-2023-causal-approach-detecting]] — complements — [[blackwell-2026-effect-political-advertising]]: same strategic-treatment confounding problem (coach timeouts vs targeted interest-group ads). Both add falsification checks: a Monte Carlo zero-effect placebo vs placebo outcomes from before treatment.
- [[weimer-2023-causal-approach-detecting]] — applies — [[harrison-2010-introduction-monte-carlo]]: Weimer runs the sample-and-tally recipe Harrison describes, but as a negative control on its own selection rule rather than as the estimator.
- [[blackwell-2026-effect-political-advertising]] — builds-on (citation chain) — [[imai-2021-fixed-effects-regression]]: Blackwell & Yamauchi cite Imai & Kim (2019) on the limits of linear FE and move the unit fixed effects into a propensity model for inverse-probability weights, one of the weighting-based alternatives Imai & Kim's conclusion calls for.
- [[cagala-2021-optimal-targeting-fundraising]] — same-method — [[sverdrup-nd-introduction-grf-causal-forests]]: both use AIPW doubly robust scores, causal-forest heterogeneity tests and depth-limited policy trees in the Athey-Wager line; Cagala is an applied field case, grf the tutorial.
- [[cagala-2021-optimal-targeting-fundraising]] — contrasts-design — [[blackwell-2026-effect-political-advertising]]: both measure returns to costly, targeted political or fundraising spending; Cagala identifies them by randomization and learns who to target, Blackwell adjusts observational data for strategic placement.
- [[sverdrup-nd-introduction-grf-causal-forests]] — complements — [[imai-2021-fixed-effects-regression]]: grf estimates heterogeneous effects under randomization or unconfoundedness; Imai & Kim set out what panel FE regressions can and cannot identify when that assumption is replaced by a linear model.
- [[zeileis-nd-object-oriented-computation]] — builds-on — [[zeileis-nd-econometric-computing-with]]: the 2006 paper cites the 2004 HC/HAC paper as its linear-model predecessor and generalizes it to any model with estimating functions and a bread.
- [[zeileis-nd-various-versatile-variances]] — builds-on — [[zeileis-nd-object-oriented-computation]]: vcovCL and vcovPL extend the 2006 bread-and-meat framework to clustered and panel covariances; verified in the reference list and Sec 3.
- [[zeileis-nd-various-versatile-variances]] — builds-on — [[zeileis-nd-econometric-computing-with]]: reuses the 2004 HC0-HC3 corrections and HAC estimators inside vcovPL; cited in the text.
- [[zeileis-nd-various-versatile-variances]] — complements (same downstream target) — [[imai-2021-fixed-effects-regression]]: both bear on the Mentat sector-growth FE regressions, Imai & Kim on what the coefficient means, Zeileis et al. on whether its standard error is trustworthy at 5 to 9 clusters.
- [[zeileis-nd-various-versatile-variances]] — applies — [[harrison-2010-introduction-monte-carlo]]: the 10,000-replication coverage study is an applied use of Monte Carlo to evaluate an estimator, the use Harrison's tutorial describes generically.
- [[deluca-2022-validating-applicability-bayesian]] — complements / contrasts-design — [[becker-nd-gameability-redistricting-criteria]]: both work at precinct and district geography in redistricting. DeLuca samples 10,000 plans as a reference distribution; Becker's ReCom runs optimize one metric each and are explicitly not representative sampling.
- [[hargittai-2015-bigger-always-better]] — parallel-argument — [[deluca-2022-validating-applicability-bayesian]]: both show an upstream measurement choice (sampling frame; PSM vs modal aggregation) changes conclusions, possibly in direction, not just precision.
- [[hargittai-2015-bigger-always-better]] — parallel-argument — [[becker-nd-gameability-redistricting-criteria]]: frame choice (Hargittai) and metric definition (Becker) each let an analyst reorder or reverse results; both ask that the choice be stated explicitly.
- [[anon-nd-house-committee-requests]] — complements (unread; inference only) — [[weimer-2023-causal-approach-detecting]]: committee request data is the standard tool for separating self-selection into a seat from the effect of holding it, the same endogenous-treatment logic as Weimer's coach timeouts. The codebook was not read, so this link carries no weight.

## What it means for the systems

**Mentat sector-growth study ([[systems/mentat-sector-growth-study]]; strongest bearing, also Paul's own proposal).**
- **The year-FE sign flip has a known explanation.** On the L3-LV lobbying test, the growth slope moves from +0.352 to -0.477 under year FE. [[imai-2021-fixed-effects-regression]] explains why this is expected: one-way and two-way FE weight sector-years differently, so the variants are not robustness checks on a single estimand. Sign agreement across them is not agreement on a causal effect. [[blackwell-2026-effect-political-advertising]] reinforces this. The DECEL dummy, which switches on and off within a sector, is exactly Imai & Kim's setting.
- **Wording rule for the pooled coefficient.** The pooled sector-FE coefficient spans a known fault line (banks, credit unions, life insurance, auto, with crypto separate). It should be quoted per sector or labelled UNSTRATIFIED, with its window (2008-2024, about 9 elections) and its denominator (sector-years).
- **Inference at 5 to 9 clusters is outside the evidence.** It is below [[zeileis-nd-various-versatile-variances]]'s smallest tested G of 10, where CR1-type errors are already anti-conservative. Choosing a wild cluster restricted bootstrap with Webb weights over CR1 t-tests is the right instinct, but nothing in this theme validates it.
- **Implementation checks.** The two-way sector-by-year CR1 sensitivity hand-codes the paper's Eq. 20. The hand-coded Bartlett Newey-West HAC in SEATS-W (lags of 2, 4 and 8 weeks, secondary, no p-value) is consistent with [[zeileis-nd-econometric-computing-with]], whose examples show lag and prewhitening choices moving SEs by about 40%. That HAC has no second-path check (for example against sandwich::NeweyWest), so its numbers stay UNVERIFIED.
- **Small refit regressions ([[systems/mentat-giving-forecast]]).** In the forecast refit (df about 11), HC1 rows are exposed to the leverage problem in the Alaska example.

**Melange validation contract ([[systems/melange-validation]]).**
- **No falsification check.** The prospective receptivity contract limits its claims to forecasting, which is correct. But it has no negative control or placebo: a repo search of fec/engine found none. [[weimer-2023-causal-approach-detecting]] and [[blackwell-2026-effect-political-advertising]] show cheap ones: a simulated zero-effect run through the same selection rule, and pre-treatment placebo outcomes.
- **The planned contact-conditioned arm is biased by design.** It is the coach-timeout design Weimer warns against: contacts are chosen in response to the expected outcome.
- **This fits Paul's contact-inference documents.** They already frame the layer as a ranking-and-calibration claim and bar "lobbied/contacted" wording. LDA remains an identity source only, never an edge or weight source (D-021). See [[systems/melange-lobbying]].
- **Monte Carlo inputs and error bars ([[systems/melange-influence-engine]]).** [[harrison-2010-introduction-monte-carlo]] applies to the cascade engines as a caution only: the saturation constant 6 and the $400k bystander scale are unestimated inputs. Scenario and frontier spreads are reported as point means with no Monte Carlo standard error. That is UNVERIFIED and rests on a four-file search.

**Targeting scores ([[systems/terrier-row-scoring]], [[systems/melange-allocation]], [[systems/mentat-fec-attribution]]; Terrier untapped potential, Melange PERS persuadability, receptivity seed_prob, workbook and joint allocators).**
- **These rank on heterogeneity that is assumed, not learned.** The rankings come from hand-set curves, multipliers and weights. [[cagala-2021-optimal-targeting-fundraising]] and [[sverdrup-nd-introduction-grf-causal-forests]] give the test such a ranking must pass (RATE/AUTOC, or a held-out comparison against uniform and random policies). Running it requires logged treatment assignment, randomized or known-propensity, plus outcomes. No system records these today.
- **What the rankings may and may not be called.** Until the test is run, these rankings and graph ranks remain heuristic model outputs. They are not closeness, access, introduction or persuasion probabilities (D-025). Nothing here validates reciprocity (D-104..D-108).
- **Scope of the evidence.** Blackwell & Yamauchi study voters, not legislators, so they cannot support the competitiveness multiplier or the money terms in the flip model.

**Terrier aggregation ([[systems/terrier-imputation]], [[systems/terrier-canvass]]).** [[deluca-2022-validating-applicability-bayesian]] is the first outside support in the vault for the rule of summing class probabilities rather than taking the argmax. Bearings rated VERIFIED and VERIFIED-SOFTENED cover the CUA index and area EB aggregation. The exposed spot is the Luna RF/KNN completion step, which fills each blank with the modal class. If the area measures read those filled codes, they are plurality aggregates. The transfer holds only for calibrated probabilities and is UNVERIFIED for vendor consumer fields.

**Melange SNA and the media blend ([[systems/melange-sna]], [[systems/melange-staffer-network]], [[systems/moneyball-rank-pillars]]).** [[hargittai-2015-bigger-always-better]]'s frame logic applies to Bluesky, where X adoption does not raise the problem. Over 2023-07-01 to 2026-09-19, 135 of 533 roster members posted on Bluesky: 117 D, 16 R and 2 I. Of the 411 member-to-member mentions, 392 are D-to-D and none cross party. These mentions enter the social layer at weight 0.4. Separately, in the media blend a member silent on social media in a category gets half their press signal, so silence reads as zero stance ([[systems/melange-stance]]). These counts are a single-agent reading and are UNVERIFIED leads, not findings (DE-062). Two possible responses are a frame label or stratified reporting. Neither per-edge volume normalization nor dropping cosponsorship edges is a remedy: both were measured and REJECTED.

**Member measurement ([[systems/melange-psychometrics]], [[systems/melange-legislative-data]], [[systems/melange-labels-and-tiers]]).**
- **Say-do gate and drift criteria.** [[poole-1999-nominate-after-years]] (near-fixed, one-dimensional, polarized positions) qualifies say-do gate Path V: one aligned final-passage vote mostly records party. It also explains why drift-based susceptibility criteria have little true variance.
- **Ideal-point controls.** The paper argues for an ideal-point control rather than a bare party dummy where party-confounded tiers are read, as in the CFPB isolation. The supporting party-sorting check is UNVERIFIED.
- **Committee pillar (40% of the live blend).** [[anon-nd-house-committee-requests]] could separate self-selection from seat leverage for this pillar. The source is unread and its bearing is UNVERIFIED, so it carries no weight.

**District geography ([[systems/melange-demographic-potential]], [[systems/melange-pathfinding]]).** [[becker-nd-gameability-redistricting-criteria]] warns that the system's single, implicit change measures each choose an answer. These are the CD116-to-CD119 comparison in redistricting_density.py and the PVI carryover across mid-decade redistricting, which COMPET_MULT depends on. The same applies to the three unratified score-blend weight sets. All of this is UNVERIFIED (DE-062). Audio-derived features ([[systems/melange-audio-speech]]) carry no bearing from this theme.

## Gaps

**Missing from the theme itself**
- **Two bibliography entries have no work record.** Paul's bibliography lists Keele (causal inference in political methodology) and Fouirnaies & Hall. The source spells the first author "FOUIRNAIES", which needs checking.
- **Unresolved pointer.** "Look at Washington University at St. Louis and UCSD" does not say which works it refers to.
- **Annotation prose is likely lost.** It probably vanished in the docx-to-markdown conversion; the original .docx should be checked.
- **The committee-request codebook was not read.** [[anon-nd-house-committee-requests]] (Frisch & Kelly 2007) returned 0 characters. Paul needs to supply the PDF.
- **Metadata to verify:**
  - [[blackwell-2026-effect-political-advertising]] is a 9 Feb 2026 manuscript with no venue or DOI.
  - The [[poole-1999-nominate-after-years]] title appears run together with its subtitle.
  - Most Zeileis, Sverdrup and Becker entries carry no year.

**Literature Paul's own design calls for but this theme lacks**
- **Proposal anchors.** The sector-growth proposal's anchor works are not in this theme: Cox & Magar 1999, Snowberg et al., Kerr, Lincoln & Mishra 2014, de Figueiredo & Richter, Grier, Munger & Roberts, Ansolabehere et al., and Bonica. Grier et al. is the only theory anchor offered for the sign of the performance-spending relation (Q10).
- **Few-cluster inference (5 clusters).** Nothing evaluates the wild cluster restricted bootstrap-t below G = 10. [[zeileis-nd-various-versatile-variances]] stops at 10 and tests only a variance bootstrap.
- **Lagged dependent variable with fixed effects (Q13).** Nothing addresses the dynamic-panel bias of a lagged DV with unit FE over a short T of about 9.
- **Event study versus regression with about 9 elections (Q7).** There is no staggered-adoption or event-study DiD work. [[imai-2021-fixed-effects-regression]] covers only the binary on/off case.
- **Sensitivity to unmeasured confounding.** No work gives bounds beyond placebos.
- **Causal inference under network interference.** Melange's core is a cascade on a network, but [[cagala-2021-optimal-targeting-fundraising]] and [[sverdrup-nd-introduction-grf-causal-forests]] assume SUTVA. Nothing here covers spillovers, exposure mappings or peer effects.
- **Money and legislators.** No work in this theme identifies an effect of money on legislator behavior or access. The randomized-access literature is referenced from other themes but is absent here, so the flip model's money terms have no causal support in this theme.
- **Monte Carlo practice.** Nothing covers Monte Carlo standard errors, convergence or common-random-number variance reduction beyond the generic [[harrison-2010-introduction-monte-carlo]].

**Missing inside the systems (from the bearings, all leads)**
- **Falsification.** No placebo or negative-control run exists in the Melange validation contract.
- **Treatment logs.** No logged treatment assignment exists anywhere, so no targeting score can be tested for effect concentration.
- **Second paths.** No second-path check exists for the hand-coded HAC in SEATS-W.
- **Error bars.** No Monte Carlo standard error is reported on scenario or frontier spreads.
- **Aggregation.** The Luna completion step uses modal assignment, against the PSM evidence.
- **Unverified counts.** The Bluesky frame counts and the party-sorting check both await a second reproducing path (DE-062).

## All works

- [[weimer-2023-causal-approach-detecting]] (2023) — A causal approach for detecting team-level momentum in NBA games
- [[harrison-2010-introduction-monte-carlo]] (2010) — Introduction To Monte Carlo Simulation
- [[imai-2021-fixed-effects-regression]] (2021) — On the Use of Two-Way Fixed Effects Regression Models for Causal Inference with Panel Data
- [[blackwell-2026-effect-political-advertising]] (2026) — The Effect of Political Advertising after Citizens United: Adjusting for Unmeasured Confounding in Marginal Structural Models
- [[poole-1999-nominate-after-years]] (1999) — D-NOMINATE After 10 Years: A Comparative Update to Congress: A Political Economic History of Roll Call Voting
- [[cagala-2021-optimal-targeting-fundraising]] (2021) — Optimal Targeting in Fundraising: A Causal Machine-Learning Approach
- [[deluca-2022-validating-applicability-bayesian]] (2022) — Validating the Applicability of Bayesian Inference with Surname and Geocoding to Congressional Redistricting
- [[hargittai-2015-bigger-always-better]] (2015) — Is Bigger Always Better? Potential Biases of Big Data Derived from Social Network Sites
- [[becker-nd-gameability-redistricting-criteria]] (n.d.) — The Gameability of Redistricting Criteria
- [[anon-nd-house-committee-requests]] (n.d.) — House Committee Requests Codebook
- [[sverdrup-nd-introduction-grf-causal-forests]] (n.d.) — An introduction to grf
- [[zeileis-nd-various-versatile-variances]] (n.d.) — Various Versatile Variances: An Object-Oriented Implementation of Clustered Covariances in R
- [[zeileis-nd-object-oriented-computation]] (n.d.) — Object-Oriented Computation of Sandwich Estimators
- [[zeileis-nd-econometric-computing-with]] (n.d.) — Econometric Computing with HC and HAC Covariance Matrix Estimators
