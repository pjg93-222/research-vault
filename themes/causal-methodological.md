---
type: theme
name: "Causal & Methodological"
paper_count: 17
tags: [theme, type/theme]
---

# Causal & Methodological

## Causal & Methodological: synthesis

This theme holds **17 works**. Most are toolkits or critiques, not substantive findings about politics. As a group they answer one question for Paul: **what can his observational, small-sample, hand-weighted systems honestly claim, and with what inference?** Five clusters come out of the reading:

1. **Fixed-effects panels are not design-based causal estimators.** [[imai-2021-fixed-effects-regression]] shows this for binary on/off treatments. [[callaway-2025-difference-differences-continuous]] shows it for continuous doses. [[blackwell-2026-effect-political-advertising]] offers a weighting alternative when strategic treatment feeds back on outcomes.
2. **Inference with few clusters is fragile.** The sandwich lineage ([[zeileis-nd-econometric-computing-with]], then [[zeileis-nd-object-oriented-computation]], then [[zeileis-nd-various-versatile-variances]]) and [[mackinnon-2017-wild-bootstrap-inference]] agree on two points. CR1 / t(G-1) tests are anti-conservative at small G. The restricted wild cluster bootstrap is the best available tool, but only inside a band of treated-cluster counts. None of these works tests G = 5 to 9, which is the range Mentat's sector-growth study works in.
3. **Falsification and simulated nulls.** [[weimer-2023-causal-approach-detecting]] uses an exogenous interruption plus a Monte Carlo placebo. Blackwell & Yamauchi use placebo outcomes from before treatment. [[harrison-2010-introduction-monte-carlo]] is background: a simulation is only as good as its inputs.
4. **Heterogeneous effects and targeting.** [[cagala-2021-optimal-targeting-fundraising]] and [[sverdrup-nd-introduction-grf-causal-forests]] set the standard. A targeting rule must beat uniform and random assignment out of sample, using randomized or known-propensity treatment. It may still fail to carry over to a new population.
5. **Measurement validity and legislator baselines.** [[deluca-2022-validating-applicability-bayesian]] shows that summing probabilities beats argmax aggregation. [[hargittai-2015-bigger-always-better]] shows that a biased sampling frame can reverse a finding. [[becker-nd-gameability-redistricting-criteria]] shows that the choice of metric picks the winner. [[poole-1999-nominate-after-years]] documents a one-dimensional, sorted Congress. [[harbridge-yong-2023-bipartisan-path-effective]] finds that the share of cross-party cosponsors a member attracts predicts effectiveness, peaking near a 50/50 mix. [[anon-nd-house-committee-requests]] (Frisch & Kelly request data) was **not read**: extraction returned 0 characters.

**Paul's own notes for this folder.** His annotated bibliography lists five bare citations with no prose: Imai & Kim, Keele, Blackwell & Yamauchi, Fouirnaies & Hall, and Poole & Rosenthal. It also has one to-do line: "Look at Washington University at St. Louis and UCSD". Two of the five, **Keele** and **Fouirnaies & Hall**, have no work record in this theme. Several design documents carry his operative views: the sector-growth design (H1-H4, about 9 elections, 5 clusters, wild cluster bootstrap, Driscoll-Kraay), the contact-inference evaluation plan and its consolidated state, and the approach-brief handoff.

- **Sector-growth design.** Paul chooses majority-control probability over seat change. He separates the direction of giving (RShare) from total dollars, uses prior-year growth as the main test, and flags 5 clusters and about 9 elections as fragile in his own words.
- **Contact-inference documents.** These set his claim discipline. The contact layer makes a **ranking-and-calibration claim, not a causal one**. LDA does not record contacts, so every accuracy figure is a lower bound. Contacts are long-relationship members, identified by bill authorship, geography and specific-bill engagement, not by money or event timing.

The literature here mostly confirms caution Paul has already built in. It sharpens three open exposures:
- the Mentat sector-growth FE and cluster inference;
- the lack of any negative control or Monte Carlo SE in the Melange engine;
- hand-set heterogeneity (allocators, PERS, untapped potential) with no logged treatment to test it against.

## Strands

### Panel identification: what fixed-effects and DiD estimates mean

**Shared claim.** The workhorse linear fixed-effects regression has a causal reading only through its linear-additive model, not through the research design.

**[[imai-2021-fixed-effects-regression]].** The two-way FE (2FE) estimator is a matching estimator. Its adjustment set contains same-status "mismatches", so it cannot adjust for unit and time confounders nonparametrically at the same time. Multi-period DiD equals only a *weighted* 2FE, with some control observations getting negative weights. Unweighted 2FE equals DiD only in the 2-group x 2-period case.

**[[callaway-2025-difference-differences-continuous]].** This is the continuous-dose analogue:
- Under ordinary parallel trends, only ATT(d|d) and ATT_loc are identified.
- Causal-response slopes (ACRT) need *strong* parallel trends, which pre-period data cannot test.
- The TWFE dose-by-post slope is a weighted integral whose levels weights integrate to zero and are negative below the mean dose.
- In the Medicare PPS application (5,881 hospitals, 1980-1986), TWFE = 1.14 lands near ATT_loc = 0.80 only because two biases offset. ACRT_glob = -0.08 (s.e. 0.19). Dropping the untreated units shrinks TWFE by 78%.

**[[blackwell-2026-effect-political-advertising]].** Ads are placed strategically and feed back on campaign conditions, so the paper puts a unit FE inside the *propensity* model (IPTW-FE) rather than the outcome model. It proves root-N consistency as N and T grow together. It cites Imai & Kim's earlier version on linear-FE limits.
- Empirical result, stratified by sponsoring party (Senate and governor races 2010-2020; 467 Democratic and 623 Republican market-races): Democratic interest-group ads raise Democratic share, mainly through Democratic turnout. Republican interest-group ads show no effect.
- Caveats: significant only at alpha 0.1, no point estimates in the text, and the Trump split is dated inconsistently (2015 vs 2016).

**Agreement.** All three agree that a pooled FE slope is a weighted contrast whose weights the analyst did not choose. All three point to matching, weighting or binarized DiD as the design-based alternatives.

**Bearing (VERIFIED-SOFTENED).** This strand speaks most directly to the **Mentat sector-growth study**:
- The study fits sector-baseline OLS with sector+year FE, log-FE and first-difference variants as robustness checks.
- On the L-LV lobbying test the growth slope flips sign under year FE: L3-LV goes from +0.352 to -0.477, pooled across sectors (**UNSTRATIFIED**). Imai & Kim explain why this is expected: one-way and two-way FE weight sector-years differently. Sign agreement across those variants is therefore not agreement on one causal estimand.
- The DECEL dummy switches on and off within sector, which is exactly the Imai & Kim setting.
- Growth is a continuous dose with no zero-dose group, which is exactly the Callaway setting. The TWFE slope there is not ACRT.

Paul already treats the design as correlational ("correlational design only" is in his stated limits). These papers say the robustness variants should be described as **different estimands**, not as checks on one estimate.

Works: [[imai-2021-fixed-effects-regression]], [[callaway-2025-difference-differences-continuous]], [[blackwell-2026-effect-political-advertising]]

### Robust and few-cluster inference: the sandwich lineage and the wild bootstrap

**The software chain.** These form one citation chain plus one size study.
- [[zeileis-nd-econometric-computing-with]] (2004) casts HC and HAC estimators as one sandwich, with working examples:
  - one high-leverage state (Alaska) moves a quadratic-income SE from 830 under HC0 to 5,489 under HC4, and p from .056 to .77;
  - three HAC settings move a SE by about 40% without changing the conclusion.
- [[zeileis-nd-object-oriented-computation]] (2006) generalizes this to any model with estfun() and bread() (B M B). It carries the author's key caveat: sandwich SEs help only if the point estimates stay consistent, and they cost power when the model is right.
- [[zeileis-nd-various-versatile-variances]] (2020) adds clustered, panel and bootstrap covariances, with a 10,000-replication Monte Carlo:
  - CL-0 coverage is near 0.95 only at G of about 50 to 100;
  - at G = 10, SEs are underestimated (ordering HC3 > HC2 > HC1 > HC0);
  - the variance bootstrap (vcovBS) is only somewhat better than HC0/HC1;
  - panel Newey-West and PCSE fail when panels are short or the dependence is exchangeable.

**The size study.** [[mackinnon-2017-wild-bootstrap-inference]] shows that the "rule of 42" fails under unequal cluster sizes:
- With 50 state-sized clusters, t(G-1) rejects up to 0.107 at nominal .05. With 100 clusters it still rejects up to 0.084.
- The restricted wild cluster bootstrap (WCR) stays at 0.049 to 0.053 in those designs.
- WCR breaks when few clusters are treated. With one treated cluster of 50, t(G-1) rejects 77.75% of the time and WCR never rejects. WCR is safe roughly for 7 <= G1 <= G-7 with equal clusters.
- Rademacher weights are not advised below about 12 clusters.

**Agreement.** CR1-type t tests are anti-conservative at small G, and a bootstrap-t with restricted residuals is the best available choice.

**Where they do not overlap.** Zeileis's vcovBS is a *variance* bootstrap, not a bootstrap-t. The two "bootstrap does / does not help much" results are therefore about different procedures and do not contradict each other.

**Bearing (VERIFIED-SOFTENED).** These are method sources for the Mentat sector-growth study:
- **What the study already does:**
  - two-way sector x year CR1 sensitivity in `sgs_review.R`, which is Zeileis 2020 Eq. 20 coded by hand;
  - WCR bootstrap-t with Webb six-point weights, enumerating all 6^G draws when G is small;
  - per-sector terms resting on one cluster labelled DESCRIPTIVE, which matches MacKinnon's G1 = 1 warning;
  - SEATS-W Newey-West kept secondary with no p-value, which matches Zeileis 2020 on panel NW.
- **The open points:**
  - The study runs on **3 to 9 sector clusters**, below every design these papers test (G >= 10; MacKinnon's floor is about 12). The literature supports the *procedure*, but it cannot certify its *size* at G = 5 to 9.
  - Paul's design note specifies **Driscoll-Kraay SEs where year-level terms appear**, with only about 9 elections. Zeileis 2020 finds panel NW-type estimators unreliable in short panels, so Driscoll-Kraay should also be labelled secondary.
  - The hand-coded HAC has no second-path check (for example against `sandwich::NeweyWest`). Under DE-062 those HAC numbers stay **UNVERIFIED**.

Works: [[zeileis-nd-econometric-computing-with]], [[zeileis-nd-object-oriented-computation]], [[zeileis-nd-various-versatile-variances]], [[mackinnon-2017-wild-bootstrap-inference]]

### Falsification, natural experiments and simulated nulls

**[[weimer-2023-causal-approach-detecting]]** supplies the transferable identification idea:
- A treatment someone chooses (a coach's timeout) responds to the outcome process, so before/after comparisons are biased.
- A schedule-fixed interruption (the TV timeout) is close to exogenous.
- Result: in 5,148 propensity-matched NBA scoring runs (seasons 2004-05 to 2016-17), a TV timeout cuts the running team's next-3-minute points by 11.2% (b = -0.119, SE 0.012).
- A Monte Carlo placebo with zero true effect returns b = 0.002 (n = 1,336). This shows the run-selection rule does not manufacture the effect.
- One internal discrepancy: the mean-reversion coefficient differs between text and table.

**[[blackwell-2026-effect-political-advertising]]** adds placebo outcomes from before treatment: lagged vote and pre-election polls, both near zero under IPTW-FE.

**[[harrison-2010-introduction-monte-carlo]]** is a textbook tutorial, not evidence. Its warnings carry the weight: simulation gives no exact answers, it is only as good as its model and inputs, it is bug-prone, and analytic alternatives should be tried first.

**Agreement.** All three treat a simulated or pre-period null as cheap insurance that an estimator or selection rule is not producing its own result.

**Bearing.**
- **Contact-conditioned arm (VERIFIED, qualifies).** The prospective receptivity contract already limits claims to forecasting, which is consistent with Weimer. The *contact-conditioned* arm it plans is Weimer's coach-timeout design. Lobbyist contact is chosen in response to expected receptivity. This matches Paul's own contact-inference rule that scores are "inferred contact plausibility," never contact records (H4), and that the lane makes no causal claim.
- **No negative control (UNVERIFIED).** A repo search of `fec/engine` for placebo, negative-control or null-simulation code found none. That is a single-agent search.
- **MC cascade.** The Melange cascade engines follow Harrison's sample-and-tally recipe: a Bernoulli edge draw on the build_P kernel. They go beyond it with common random numbers, splitmix64/PCG64 generators and seed-stability checks. Harrison's "only as good as its inputs" applies directly to the unestimated kernel saturation constant 6 and the $400k bystander scale.
- **Missing MC standard errors (UNVERIFIED).** Scenario and frontier spreads are reported as point means with no Monte Carlo standard error, based on a four-file search.
- **Analytic proxy.** Harrison's "analytic first" supports Paul's wish to test an analytic proxy (Viral Centrality) against the MC cascade. Any such proxy is a heuristic model output, not a closeness or access probability (D-025).

Works: [[weimer-2023-causal-approach-detecting]], [[harrison-2010-introduction-monte-carlo]], [[blackwell-2026-effect-political-advertising]]

### Heterogeneous effects and targeting rules

**[[cagala-2021-optimal-targeting-fundraising]]** is a natural field experiment: N = 19,779 church members, one German city, 2014, gift randomized within warm and cold lists.
- Gifting everyone barely broke even on the warm list (net +0.06 euro, not significant) and lost 0.97 euro per person on the cold list.
- Warm-list effects ranged from -11.59 to +17.99 euro between the 5th and 95th percentiles.
- A depth-2 exact policy tree on AIPW scores gifted 33% of the warm list. Under 20-fold cross-validation it raised net donations by about 2.1-2.2 euro (about 14%) over all-gift, no-gift and random assignment.
- On the cold list the rule gifted 1.4%.
- **The rule did not carry over to a new year.** On the full 2015 sample there was no significant gain. It helped only where 2015 donors closely resembled 2014 donors (caliper 0.1).

**[[sverdrup-nd-introduction-grf-causal-forests]]** is the grf tutorial (causal forests, AIPW, BLP, RATE/TOC/AUTOC, Qini, policy trees). Its own examples show how often heterogeneity is absent or weak:
- school RCT: whole-sample AUTOC one-sided p = 0.65;
- payday experiment: held-out AUTOC 0.0061 (SE 0.0092);
- only a pre-specified prioritization (low financial autonomy) gives AUTOC 0.69 (SE 0.22).
- The authors flag that out-of-bag TOC tests are heuristics with no finite-sample guarantees, and that policy trees need held-out evaluation.

**Agreement.** A ranking of who is most movable must be tested against uniform and random benchmarks, out of sample, using randomized or known-propensity treatment. Heterogeneity should be presumed absent until a RATE-type test shows it.

**Bearing (VERIFIED / VERIFIED-SOFTENED).** Every targeting score Paul ships embeds heterogeneity that is assumed, not learned: Melange's joint and workbook allocators, the PERS persuadability table, receptivity seed_prob, the competitiveness multiplier, and Terrier's untapped-potential score. All are hand-set response curves, multipliers and weights, and all are heuristic model outputs.
- None of the systems logs treatment assignment (canvass contacts, outreach, money) with outcomes, so neither method can be run yet.
- The papers describe the experiment and benchmark test the systems lack. They validate **no** constant.
- The populations also differ: individual donors and students here, legislators (Melange) and voters (Terrier) in the systems.

Works: [[cagala-2021-optimal-targeting-fundraising]], [[sverdrup-nd-introduction-grf-causal-forests]]

### Measurement validity: aggregation rules, sampling frames and metric choice

All three show that a measurement decision made before estimation can decide the answer.

**[[deluca-2022-validating-applicability-bayesian]].** BISG was checked against self-reported race in the North Carolina and Georgia voter files (Black vs White only) across 10,000 simulated plans per state.
- **Summing posteriors (PSM) beats plurality/argmax (PM)** in nearly all districts.
- Under PSM, Black-share error stayed within about 5 points in both states. White-share error was about 0 in North Carolina and up to about 10 points in Georgia.
- PM has a long right tail of errors at precinct level.
- Caveat: two highly segregated states with two dominant groups.

**[[hargittai-2015-bigger-always-better]].** Platform choice is not random: Pew 2013 (N = 1,801) and a UIC panel 2009-2012 (about 517 students). A biased frame can **reverse the direction** of a finding, not just add noise, so studies should state their frame in the title and abstract.

**[[becker-nd-gameability-redistricting-criteria]].** County-split and least-change criteria each have many defensible measures that rank maps differently.
- On Minnesota (11 maps) and Wisconsin (6 maps), no map wins on both a split measure and a change measure.
- Two Minnesota plans move 1.90% vs 1.93% of people, yet differ 13.90% vs 1.93% on incumbent-people change.
- The analysis is descriptive only. The study was funded by Jenner & Block, and text and Table 3 disagree on one comparison.

**Bearing.**
- **Terrier.** The row framework already follows the PSM rule (expected values over imputed class probabilities, area shares from summed probabilities). DeLuca is the first outside evidence for that owner policy (VERIFIED). The exposed spot is the **Luna completion step**, which fills blanks with the argmax class, which is the PM rule (VERIFIED-SOFTENED). The transfer is about aggregation, not race. It is UNVERIFIED for vendor consumer fields until an area-level holdout exists.
- **Melange social layer (Hargittai).** These counts are one agent's reading of live files and stay **UNVERIFIED** (DE-062).
  - X coverage is near-complete: 502 of 515 members in social_media.db have an X handle.
  - Bluesky (posts 2023-07-01 to 2026-09-19): 135 of 533 roster members posted (117 D, 16 R, 2 I).
  - 392 of 411 member-to-member Bluesky mentions are D-to-D, and none cross party. These mentions enter the social interaction layer at weight 0.4.
  - The media blend gives a member who is silent on social media half their press signal, so silence counts as zero stance.
  - Hargittai's coefficients do not transfer; only her sampling-frame logic does.
- **Becker.** The district plumbing uses single, implicit change measures: CD116 vs CD119 in `redistricting_density.py` (seat-label change, block-count weighting) and the PVI orientation flags across mid-decade redistricting. The weaker lesson, that metric choice reorders winners, also applies to the Moneyball score blend's three unratified weight sets. All of this is UNVERIFIED.

Works: [[deluca-2022-validating-applicability-bayesian]], [[hargittai-2015-bigger-always-better]], [[becker-nd-gameability-redistricting-criteria]]

### Legislator baselines: ideal points, bipartisan effectiveness and committee selection

**[[poole-1999-nominate-after-years]]** (Congresses 1-105, 1789-1998) is a measurement baseline:
- Members barely move over a career. A linear time trend adds about 0.3 points of classification over constant positions (House 85.65 to 85.96).
- The second dimension now adds under 1%.
- Party overlap on dimension 1 is under 1.5% in both chambers over the last five Congresses studied.
- Polarization rose smoothly, with no break at 1994.
- The donor-and-primary claim Paul highlighted rests on a cited working paper this text does not present. Paul's bibliography also flags that the Poole & Rosenthal title is run together with its subtitle and should be checked against the source.

**[[harbridge-yong-2023-bipartisan-path-effective]]** (93rd-114th Congresses, 1973-2016; about 9,000 House and 2,170 Senate legislator-Congress rows):
- With legislator FE, the share of cosponsors a member **attracts** from the other party predicts LES: 0.433 House, 0.330 Senate (one SD is about 6-8% of mean LES). The share a member **offers** does not.
- The effect is an **inverted U**, peaking near 50/50 (0.496 House, 0.512 Senate).
- The effect holds within era, majority/minority and polarization strata. Pre/post-1994 splits are reported.
- Offering predicts attracting: within-member 0.317 House, 0.547 Senate.
- Design limits: observational, one-tailed tests, no design-based identification. Coefficients are larger without member FE (0.552 House), which is a reminder from the first strand that FE choice changes the estimand.

**[[anon-nd-house-committee-requests]]**: **no text was read** (0 characters extracted). Paul's annotation identifies it as Frisch & Kelly 2007, House committee request data. Its relevance rests on general knowledge only (UNVERIFIED). Request data is the standard tool for separating self-selection into a committee from the effect of holding the seat.

**Bearing.**
- **Say-do gate.** Poole qualifies the say-do gate's Path V. In a sorted, one-dimensional chamber, one CU-aligned final-passage vote mostly records party (UNVERIFIED read-only check). It also argues for an ideal-point control over a bare party dummy wherever party-confounded tiers are read, as in the CFPB isolation.
- **bpf and cooperation v2.** Harbridge-Yong qualifies Melange **bpf**. bpf is an undirected, symmetrized cross-party tie share, a heuristic model output. Cooperation v2 measures *offered* support. Nothing in the system measures *attracted* support. The flip model's (1+0.3 bpf) passage bonus and the coalition seed boost (1+0.8 bpf) are monotone, against the paper's peak-then-decline (VERIFIED-SOFTENED).
- **Reciprocity.** The paper's reciprocity is **cosponsorship exchange** and does **not** validate the money reciprocity of D-104..D-108, which stays unvalidated.
- **Flags.** Its robustness rows (normalizing by Congress, controlling for cosponsorship levels) are member-level regression controls. They are not an argument for per-edge volume normalization or for dropping regular cosponsorship edges, both of which this project measured and REJECTED.
- **Committee pillar.** The committee-request data would bear on the Moneyball Committee/Influence pillar (40% of the live blend; owner-set tiers 3.0/1.8/0.9), which treats a seat as structural power and never models selection. It is House-only and historical, so it cannot be joined to the 119th roster.

Works: [[poole-1999-nominate-after-years]], [[harbridge-yong-2023-bipartisan-path-effective]], [[anon-nd-house-committee-requests]]

## Agreements and debates

**1. Is a fixed-effects slope a causal effect?** Here the works agree with each other and disagree with common practice.
- [[imai-2021-fixed-effects-regression]], [[callaway-2025-difference-differences-continuous]] and [[blackwell-2026-effect-political-advertising]] all say no, except under linear-additive or strong-parallel-trends assumptions that cannot be tested.
- [[harbridge-yong-2023-bipartisan-path-effective]] reads its within-legislator FE coefficient as the payoff to bipartisanship, using one-tailed tests. Its coefficients move from 0.433 to 0.552 (House) when member FE are dropped, which is the estimand sensitivity Imai & Kim predict.
- Paul's sector-growth design sits on the cautious side. It is labelled correlational, though its FE variants are still framed as robustness *checks* rather than as different estimands.

**2. Does the bootstrap rescue few-cluster inference?**
- [[mackinnon-2017-wild-bootstrap-inference]]: yes, the restricted wild cluster bootstrap holds size, but only for roughly 7 <= G1 <= G-7. It fails completely when one cluster is treated.
- [[zeileis-nd-various-versatile-variances]]: at G = 10 the bootstrap is only somewhat better than HC0/HC1.
- These look like opposite answers but are not. Zeileis tests a *variance* bootstrap (vcovBS), and MacKinnon tests a restricted *bootstrap-t*.
- Neither paper goes below G = 10. At Mentat's 5 to 9 sector clusters, the literature is silent, not reassuring.

**3. Panel HAC with short panels.**
- [[zeileis-nd-econometric-computing-with]]: HAC lag and prewhitening choices move SEs by about 40% without changing conclusions, in a long time series.
- [[zeileis-nd-various-versatile-variances]]: panel Newey-West and PCSE fail when panels are short.
- Paul's design note calls for Driscoll-Kraay SEs where year-level terms appear, with about 9 elections. This is a direct tension. SEATS-W already keeps its Newey-West estimate secondary, but the Driscoll-Kraay rows have no equivalent label.

**4. Do learned targeting rules generalize?**
- [[cagala-2021-optimal-targeting-fundraising]] finds an out-of-sample gain of about 14% within the 2014 sample, but **no significant gain on the full 2015 sample**.
- [[sverdrup-nd-introduction-grf-causal-forests]] shows whole-sample heterogeneity tests that are null (AUTOC p = 0.65) or near zero.
- Both work against the systems' assumption that hand-set heterogeneity (PERS, multipliers, untapped potential) sorts units by treatment effect.

**5. Monotone vs non-monotone response.**
- [[harbridge-yong-2023-bipartisan-path-effective]] finds an inverted U with a peak near a 50/50 cosponsor mix.
- [[callaway-2025-difference-differences-continuous]] shows that estimating any dose-response *shape* (for example concavity of money effects) requires strong parallel trends.
- The system assumes monotone bpf bonuses and concave money terms, and estimates neither.

**6. Aggregation and metric choice.**
- [[deluca-2022-validating-applicability-bayesian]] favours summing probabilities. Terrier's Luna completion step does argmax.
- [[becker-nd-gameability-redistricting-criteria]] shows there is no neutral single change or split metric. The system uses single implicit ones.
- [[hargittai-2015-bigger-always-better]] warns that frame bias can flip the sign of a result. The Bluesky layer is nearly all Democratic (UNVERIFIED counts).

**7. Simulation vs analysis.**
- [[harrison-2010-introduction-monte-carlo]] advises trying analytic methods first and treats a simulation as only as good as its inputs.
- [[weimer-2023-causal-approach-detecting]] uses simulation as a falsification tool.
- The two are compatible. Together they argue that the Melange cascade needs both an analytic benchmark and a null-simulation check.

**SNA flags.** No work in this theme argues for dropping regular cosponsorship edges or for per-edge volume normalization, the two SNA proposals this project measured and REJECTED. Harbridge-Yong's Congress-normalization and cosponsorship-level controls are member-level regression controls and must not be cited as support for either.

## Methods

| Method family | Works | Typical weakness |
|---|---|---|
| Analytic and econometric proofs | [[imai-2021-fixed-effects-regression]], [[callaway-2025-difference-differences-continuous]] | No data or binary treatment only (Imai & Kim); the identifying assumptions they show are needed (linearity, strong parallel trends) are untestable |
| Monte Carlo size and coverage studies | [[mackinnon-2017-wild-bootstrap-inference]], [[zeileis-nd-various-versatile-variances]] | Designs start at G >= 10 (MacKinnon's tests are mostly G of 20 to 100); results do not reach G = 5-9 |
| Software and method papers | the three Zeileis papers, [[sverdrup-nd-introduction-grf-causal-forests]], [[harrison-2010-introduction-monte-carlo]] | Tools, not evidence; the worked examples are illustrations, and small-sample claims are borrowed from other studies (Zeileis 2004) |
| Observational causal designs with matching or weighting | [[weimer-2023-causal-approach-detecting]], [[blackwell-2026-effect-political-advertising]] | Depend on no unmeasured time-varying confounding; Blackwell's effects reach only alpha 0.1, with no point estimates in the text |
| Randomized field experiment + policy learning | [[cagala-2021-optimal-targeting-fundraising]] | One city and one charity; the learned rule fails to carry over to the next year's full sample |
| Observational FE regression | [[harbridge-yong-2023-bipartisan-path-effective]] | No design-based identification; one-tailed tests; outcome (LES) and predictor both come from bill records |
| Descriptive validation against ground truth | [[deluca-2022-validating-applicability-bayesian]], [[becker-nd-gameability-redistricting-criteria]] | Two segregated states and two races (DeLuca); hand-built grids plus funded litigation maps, no inference (Becker) |
| Survey and panel logistic regression | [[hargittai-2015-bigger-always-better]] | 2013 platform mix and a single-campus student panel |
| Roll-call spatial scaling | [[poole-1999-nominate-after-years]] | Standard errors acknowledged as not econometrically correct; ends at the 105th Congress |
| None (unread) | [[anon-nd-house-committee-requests]] | 0 characters extracted |

**What the theme does well.** It gives Paul the vocabulary for estimands (ATT_loc vs ACRT; weighted vs unweighted 2FE) and for inference size (G, G1, WCR, Webb weights). It also supplies two falsification templates: a simulated null and placebo outcomes from before treatment.

**What is weak across the theme.**
- No work studies inference at Paul's actual cluster counts.
- None of the substantive causal results concerns legislators, lobbying or money-to-legislator effects. Blackwell is about voters, Cagala about donors, Weimer about basketball.
- Several bearings rest on single-agent repo reads:
  - no negative control in `fec/engine`;
  - no Monte Carlo SE on scenario spreads;
  - the Bluesky party counts;
  - the district-change measures.

  These are **UNVERIFIED leads, not findings** (DE-062).

**Figures quoted from these works name their window and denominator.**
- Pooled coefficients over a known fault line are labelled UNSTRATIFIED: the sector-growth L3-LV sign flip, pooled across sectors.
- Party-stratified results stay stratified: Blackwell's Democratic vs Republican ad effects.

## How the works relate

- [[zeileis-nd-object-oriented-computation]] — builds-on — [[zeileis-nd-econometric-computing-with]]: The 2006 paper cites the 2004 paper as its linear-model predecessor and generalizes the HC and HAC sandwich to any model with estfun() and bread().
- [[zeileis-nd-various-versatile-variances]] — builds-on — [[zeileis-nd-object-oriented-computation]]: vcovCL, vcovPL and vcovBS extend the 2006 object-oriented bread and meat framework; the citation is verified in the 2020 paper's reference list and Sec. 3.
- [[zeileis-nd-various-versatile-variances]] — builds-on — [[zeileis-nd-econometric-computing-with]]: It reuses the 2004 HC0-HC3 corrections and HAC estimators inside the panel and cluster covariances; the citation is verified in the text.
- [[mackinnon-2017-wild-bootstrap-inference]] — complements — [[zeileis-nd-various-versatile-variances]]: Same few-cluster problem. Both find CR1 / t(G-1) anti-conservative at small G. MacKinnon tests a restricted wild bootstrap-t and Zeileis a variance bootstrap, so their bootstrap verdicts concern different procedures. Neither tests G below 10.
- [[callaway-2025-difference-differences-continuous]] — extends — [[imai-2021-fixed-effects-regression]]: Continuous-dose analogue of the TWFE critique: in both, the TWFE coefficient carries implicit and partly negative weights and is not the DiD or causal estimand.
- [[blackwell-2026-effect-political-advertising]] — builds-on — [[imai-2021-fixed-effects-regression]]: Blackwell & Yamauchi cite Imai & Kim (2019 version) on the limits of linear FE and move the unit FE into the propensity model (IPTW-FE), a weighting route of the kind Imai & Kim call for.
- [[weimer-2023-causal-approach-detecting]] — complements — [[blackwell-2026-effect-political-advertising]]: Both face a strategically chosen treatment (coach timeouts; ad placement) and both run falsification checks: a Monte Carlo zero-effect placebo versus placebo outcomes from before treatment.
- [[weimer-2023-causal-approach-detecting]] — complements — [[imai-2021-fixed-effects-regression]]: Imai & Kim recast 2FE as matching and show its limits; Weimer uses explicit propensity matching on a natural experiment, the design-based route.
- [[weimer-2023-causal-approach-detecting]] — applies — [[harrison-2010-introduction-monte-carlo]]: Weimer's zero-effect placebo simulation is an applied use of the sample-and-tally Monte Carlo recipe that Harrison describes. Harrison runs no simulation itself.
- [[harrison-2010-introduction-monte-carlo]] — complements — [[mackinnon-2017-wild-bootstrap-inference]]: MacKinnon & Webb's rejection-frequency study is Monte Carlo used to check inference procedures; Harrison supplies the generic method and its caveat that a simulation is only as good as its inputs.
- [[cagala-2021-optimal-targeting-fundraising]] — same-method — [[sverdrup-nd-introduction-grf-causal-forests]]: Both use AIPW / doubly robust scores with Athey-Wager policy trees and causal-forest heterogeneity tests. Cagala applies to a field experiment the toolkit the grf vignette teaches.
- [[callaway-2025-difference-differences-continuous]] — complements — [[sverdrup-nd-introduction-grf-causal-forests]]: Callaway et al. name Neyman-orthogonal / double-ML routes for ACRT_glob as future work; grf supplies that doubly robust machinery.
- [[callaway-2025-difference-differences-continuous]] — complements — [[mackinnon-2017-wild-bootstrap-inference]]: For the Mentat sector-growth regressions, Callaway covers what the continuous-growth point estimate means and MacKinnon covers how to do inference on it with few clusters.
- [[mackinnon-2017-wild-bootstrap-inference]] — complements — [[imai-2021-fixed-effects-regression]]: Inference side (size with few clusters) and identification side (meaning of the 2FE coefficient) of the same sector-by-year FE design.
- [[harbridge-yong-2023-bipartisan-path-effective]] — qualified-by — [[imai-2021-fixed-effects-regression]]: Harbridge-Yong et al. read a within-legislator FE coefficient causally. Imai & Kim's result that FE estimates are model-dependent weighted contrasts qualifies that reading; the coefficient moves from 0.433 to 0.552 (House) without member FE.
- [[poole-1999-nominate-after-years]] — complements — [[harbridge-yong-2023-bipartisan-path-effective]]: Poole & Rosenthal document a rising, one-dimensional polarization with near-zero party overlap. Harbridge-Yong et al. find the attracted-bipartisanship effect holds across polarization levels, so the two give context for each other on cross-party behaviour in a sorted Congress.
- [[anon-nd-house-committee-requests]] — complements — [[harbridge-yong-2023-bipartisan-path-effective]]: Weak link (codebook unread). Both bear on separating member self-selection from the effect of a legislative position or behaviour. Harbridge-Yong lacks a selection design, and request data is the standard selection tool for committees.
- [[deluca-2022-validating-applicability-bayesian]] — complements — [[becker-nd-gameability-redistricting-criteria]]: Both evaluate measurement choices over congressional district geography (racial-composition aggregation; split and change metrics). Both run plan-level comparisons, DeLuca over 10,000 simulated plans; the shared ensemble method was not verified in Becker.
- [[hargittai-2015-bigger-always-better]] — complements — [[deluca-2022-validating-applicability-bayesian]]: Both show that a measurement step taken before analysis (sampling frame; probability-sum vs argmax aggregation) can bias or reverse population-level conclusions.
- [[becker-nd-gameability-redistricting-criteria]] — complements — [[hargittai-2015-bigger-always-better]]: Same lesson in different domains: an analyst's choice of metric or frame decides which unit wins or which direction a finding takes, so the choice must be stated explicitly.

## What it means for the systems

Statuses follow the work records: VERIFIED, VERIFIED-SOFTENED, or UNVERIFIED (single-agent lead, DE-062).

### Mentat: sector-growth study (heaviest exposure) — [[systems/mentat-sector-growth-study]]
- **Identification (VERIFIED-SOFTENED).**
  - From [[imai-2021-fixed-effects-regression]] and [[callaway-2025-difference-differences-continuous]]: sector-FE, year-FE, log-FE and first-difference variants are **different estimands**, not robustness checks on one estimate. The L3-LV slope flip (+0.352 to -0.477 under year FE, pooled across sectors: **UNSTRATIFIED**) is what those papers predict.
  - The continuous-growth TWFE slope is not a causal-response (ACRT) estimate. There is no zero-dose group.
  - DECEL switches on and off within sector, the exact Imai & Kim setting.
  - **Suggested extension:** report binarized DiD or per-sector strata next to the pooled slope.
- **Inference (VERIFIED-SOFTENED).**
  - WCR bootstrap-t with Webb weights, enumeration of 6^G draws, and DESCRIPTIVE labels on one-cluster terms match [[mackinnon-2017-wild-bootstrap-inference]].
  - The two-way CR1 is Eq. 20 of [[zeileis-nd-various-versatile-variances]].
  - Gap: at **3 to 9 clusters** no paper certifies size. A study-specific Monte Carlo of size at G = 5 and 9 (the simulated-null idea from [[weimer-2023-causal-approach-detecting]]) is the missing evidence.
  - Paul's Driscoll-Kraay rows for year-level terms (about 9 elections) should be labelled secondary, as SEATS-W Newey-West already is. Zeileis 2020 finds panel NW-type estimators unreliable in short panels.
  - The hand-coded HAC lacks a second-path check against `sandwich::NeweyWest`, so it is **UNVERIFIED**.
- **[[systems/mentat-giving-forecast]] cycle model:** HC1 rows in small refits (df about 11) are qualified by the leverage example in [[zeileis-nd-econometric-computing-with]].

### Melange: validation and influence engine — [[systems/melange-influence-engine]]
- **Prospective contract (VERIFIED, suggests extension).**
  - Add a negative control: a placebo outcome ([[blackwell-2026-effect-political-advertising]]) or a simulated null ([[weimer-2023-causal-approach-detecting]]).
  - "No negative control in `fec/engine`" is an **UNVERIFIED** single-agent search.
  - The planned contact-conditioned arm is the coach-timeout design Weimer warns against. This agrees with Paul's contact-inference rules: ranking and calibration only, "inferred contact plausibility," never "lobbied/contacted" ([[systems/melange-lobbying]]).
  - LDA remains an identity source, not an edge or weight source (D-021).
- **MC cascade (VERIFIED-SOFTENED).**
  - [[harrison-2010-introduction-monte-carlo]] is context. The engine exceeds the recipe (CRN, PCG64, seed stability).
  - Unestimated constants (saturation 6, $400k bystander scale) fall under "only as good as its inputs".
  - The missing MC standard error on scenario and frontier spreads is **UNVERIFIED**.
  - Graph ranks and cascade outputs remain heuristic model outputs, never closeness, access or introduction probabilities (D-025) ([[systems/melange-sna]], [[systems/melange-pathfinding]]).
- **Allocators, PERS, receptivity seed_prob, competitiveness multiplier (VERIFIED / VERIFIED-SOFTENED, qualifies)** ([[systems/melange-allocation]]). These rest on assumed heterogeneity. [[cagala-2021-optimal-targeting-fundraising]] and [[sverdrup-nd-introduction-grf-causal-forests]] give the test (RATE/AUTOC, policy value vs uniform and random), but it needs logged, randomized or known-propensity treatment. No system records that today.
- **Flip model / bpf / coalition seed (VERIFIED-SOFTENED).**
  - [[harbridge-yong-2023-bipartisan-path-effective]]: the effectiveness signal sits in **attracted** cross-party support and is an inverted U peaking near 0.5. bpf is undirected and the bonuses are monotone. Cooperation v2 measures offered support.
  - **Reciprocity:** the cosponsorship reciprocity in this paper does **not** validate money reciprocity. D-104..D-108 remain unvalidated.
  - [[callaway-2025-difference-differences-continuous]] is a design template for testing whether money terms are concave, but it needs strong parallel trends.
  - [[blackwell-2026-effect-political-advertising]] concerns voters, not legislators, so it cannot support the competitiveness multiplier (VERIFIED).
- **Say-do gate Path V and CFPB isolation (VERIFIED-SOFTENED)** ([[systems/melange-stance]], [[systems/melange-psychometrics]]). [[poole-1999-nominate-after-years]]: in a sorted, one-dimensional chamber, one aligned final-passage vote mostly records party. Prefer an ideal-point control (`fec/members/dw_control.py`, Nokken-Poole) over a party dummy. The party-sorting check is **UNVERIFIED**.
- **SNA social layer and media blend (VERIFIED-SOFTENED qualifies; counts UNVERIFIED)** ([[systems/melange-sna]], [[systems/melange-staffer-network]]). [[hargittai-2015-bigger-always-better]]:
  - Bluesky mentions (392 of 411 D-to-D, 2023-07-01 to 2026-09-19) enter the cascade network at weight 0.4.
  - Social silence halves the press signal in the media blend.
  - Neither should be read as a party-neutral frame.

### Terrier
- **Row scoring (VERIFIED):** [[deluca-2022-validating-applicability-bayesian]] is the first outside support for summed-probability (PSM) area aggregation.
- **Luna completion (VERIFIED-SOFTENED):** its argmax fill is the PM rule the paper argues against.
- **Untapped potential:** qualified by the targeting strand. The persuasion vs mobilization split gets one weak, party-asymmetric data point from Blackwell (alpha 0.1).

### Moneyball — [[systems/moneyball-rank-pillars]]
- **Committee pillar (UNVERIFIED; codebook unread):** [[anon-nd-house-committee-requests]] would let the pillar test self-selection vs seat effect, but it is House-only and historical.
- **Score blend (context):** [[becker-nd-gameability-redistricting-criteria]] warns that unratified weight sets let metric choice pick the winners.
- **District geography** (CD116 vs CD119, PVI orientation): single implicit change measures; suggested extension, **UNVERIFIED** ([[systems/melange-legislative-data]]).
- Money-to-vote attribution context: [[systems/mentat-fec-attribution]].

## Gaps

**Works Paul named but this theme does not hold**
- **Keele** (causal inference in political methodology) and **Fouirnaies & Hall** are listed in Paul's folder bibliography but have no work record here. The Fouirnaies spelling is also flagged as possibly a typo.
- Paul's to-do line, "Look at Washington University at St. Louis and UCSD", is unresolved. The file does not say which works or programmes it refers to.
- **[[anon-nd-house-committee-requests]]** is unread (0 characters extracted). Paul needs to supply the Frisch & Kelly codebook PDF before it carries any weight.
- Works Paul's sector-growth design relies on, for which this theme holds no work record: Cox & Magar 1999 (majority-status value), Kerr, Lincoln & Mishra 2014 (lobbying persistence), Grier, Munger & Roberts (sign of the performance-spending relation), and the prediction-market measure of projected control. Paul's own "verify before relying" list (the $36k Cox & Magar figure, Cook accuracy figures, the Senate flip dated 2020 vs January 2021) stays open.

**Method gaps the theme exposes but does not fill**
1. **Inference at G = 3 to 9 clusters.** No work tests this range. A study-specific Monte Carlo of WCR size under Paul's actual cluster sizes and treated-cluster counts is the missing evidence. Randomization inference or sector-level aggregate tests are alternatives to consider.
2. **Modern staggered and heterogeneous DiD.** Callaway & Sant'Anna 2021, Goodman-Bacon 2021, de Chaisemartin & D'Haultfoeuille, Sun & Abraham, and Rambachan & Roth honest pre-trends are absent. Paul's open question 7 (event study vs regression with about 9 elections) needs them.
3. **Sensitivity to unmeasured confounding.** For example Cinelli & Hazlett, or E-values. No work gives a way to bound how much hidden confounding would overturn an observational estimate.
4. **Few-period inference.** Driscoll-Kraay with about 9 time points has no supporting evidence here.
5. **Causal evidence on legislators and money.** No work in this theme identifies an effect of money or lobbying on legislators. The closest is [[kalla-nd-campaign-contributions-facilitate]], held in another theme. The flip model's money terms and the competitiveness multiplier have no causal anchor here.
6. **Logged treatment data.** The targeting toolkit ([[cagala-2021-optimal-targeting-fundraising]], [[sverdrup-nd-introduction-grf-causal-forests]]) cannot be applied until canvass, outreach or money assignments are recorded with known propensities. No system records them.
7. **Calibration of class probabilities for vendor fields.** DeLuca's PSM advantage assumes reasonably calibrated posteriors. Terrier needs an area-level holdout before the transfer counts.
8. **Dynamic ideal points after 1998.** Poole & Rosenthal stops at the 105th Congress. Current Nokken-Poole and DW-NOMINATE work is not in the vault.
9. **Multiple-testing and one-look discipline.** No work treats how many variants (FE sets, lags, HAC bandwidths) may be reported before inflation sets in. This gap matters given the project's one-look rule.

**Verification debt inside the theme.** Several system bearings are single-agent repo reads and remain **UNVERIFIED** until a second path reproduces them (DE-062):
- no negative control in `fec/engine`;
- no MC standard error on scenario spreads;
- the Bluesky party counts;
- party sorting in the say-do categories;
- the hand-coded HAC;
- the district-change measures.

**Internal inconsistencies to check.**
- Weimer: text vs Table 3 on the mean-reversion coefficient.
- Blackwell & Yamauchi: Trump split dated 2015 vs 2016; no venue or DOI for the 9 Feb 2026 manuscript.
- Becker & Gold: Plan 8 vs Plan 9 direction, text vs table.
- MacKinnon & Webb: 0.1073 in the text vs 0.10718 in the table.
- Poole & Rosenthal: title conflated with its subtitle.

**Scope note.** The request mentions 24 new works. This theme file holds 17, so the remainder presumably belongs to other themes.

## All works

- [[weimer-2023-causal-approach-detecting]] (2023) — A causal approach for detecting team-level momentum in NBA games
- [[harrison-2010-introduction-monte-carlo]] (2010) — Introduction To Monte Carlo Simulation
- [[imai-2021-fixed-effects-regression]] (2021) — On the Use of Two-Way Fixed Effects Regression Models for Causal Inference with Panel Data
- [[blackwell-2026-effect-political-advertising]] (2026) — The Effect of Political Advertising after Citizens United: Adjusting for Unmeasured Confounding in Marginal Structural Models
- [[poole-1999-nominate-after-years]] (1999) — D-NOMINATE After 10 Years: A Comparative Update to Congress: A Political Economic History of Roll Call Voting
- [[cagala-2021-optimal-targeting-fundraising]] (2021) — Optimal Targeting in Fundraising: A Causal Machine-Learning Approach
- [[deluca-2022-validating-applicability-bayesian]] (2022) — Validating the Applicability of Bayesian Inference with Surname and Geocoding to Congressional Redistricting
- [[hargittai-2015-bigger-always-better]] (2015) — Is Bigger Always Better? Potential Biases of Big Data Derived from Social Network Sites
- [[becker-nd-gameability-redistricting-criteria]] () — The Gameability of Redistricting Criteria
- [[anon-nd-house-committee-requests]] () — House Committee Requests Codebook
- [[sverdrup-nd-introduction-grf-causal-forests]] () — An introduction to grf
- [[zeileis-nd-various-versatile-variances]] () — Various Versatile Variances: An Object-Oriented Implementation of Clustered Covariances in R
- [[zeileis-nd-object-oriented-computation]] () — Object-Oriented Computation of Sandwich Estimators
- [[zeileis-nd-econometric-computing-with]] () — Econometric Computing with HC and HAC Covariance Matrix Estimators
- [[callaway-2025-difference-differences-continuous]] (2025) — Difference-in-Differences with a Continuous Treatment
- [[harbridge-yong-2023-bipartisan-path-effective]] (2023) — The Bipartisan Path to Effective Lawmaking
- [[mackinnon-2017-wild-bootstrap-inference]] (2017) — Wild Bootstrap Inference for Wildly Different Cluster Sizes
