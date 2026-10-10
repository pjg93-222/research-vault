---
type: system
name: Terrier canvass targeting and walk planning
slug: terrier-canvass
components: [terrier-canvass.candidate-support, terrier-canvass.turnout-calibration, terrier-canvass.untapped-potential, terrier-canvass.street-presets-and-turf]
brain_notes: [terrier/terrier, terrier/2026-10-06-terrier-rulings, terrier/2026-10-06-terrier-blueprint]
paper_count: 11
tags: [system, type/system-hub]
---

# Terrier canvass targeting and walk planning

Door-level targeting for candidate districts. Covers candidate support s, calibrated chance of voting t, the untapped-potential formula, street-plan presets, the one-click turf planner and walk routing over TIGER streets.

Brain notes: [terrier/terrier](obsidian://open?vault=brain&file=terrier%2Fterrier); [terrier/2026-10-06-terrier-rulings](obsidian://open?vault=brain&file=terrier%2F2026-10-06-terrier-rulings); [terrier/2026-10-06-terrier-blueprint](obsidian://open?vault=brain&file=terrier%2F2026-10-06-terrier-blueprint)

## Terrier canvass targeting and walk planning: evidence assessment

**Scope.** This covers four components: candidate support s, calibrated chance of voting t, the untapped-potential formula, and street presets with the turf planner and routing. The pool holds 14 evidence rows from 11 papers. By status: **6 VERIFIED-SOFTENED**, **5 UNVERIFIED**, **3 VERIFIED**. Under DE-062 the UNVERIFIED rows are single-agent leads, not findings.

**Bottom line.** None of the four components has direct, VERIFIED, transferable evidence for its core assumption. Each component's own empirical questions are unanswered by the pool:
- **Persuasion heterogeneity.** No row reports how canvass persuasion effects vary with prior support.
- **GOTV by propensity.** No row reports how get-out-the-vote effects vary with turnout propensity.
- **Turnout scores.** No row tests whether vendor turnout scores are calibrated.
- **Turf construction.** No row compares contacts-per-hour across ways of building turf.

**What the evidence does say.**
- **Partisan midpoint.** The best-grounded bearing is VERIFIED but survey-based (Broockman & Kalla 2026). It shows that the partisan midpoint holds cross-pressured voters who respond to a given issue in *opposite* directions. That qualifies the s = 0.5 persuasion peak without refuting it.
- **Contact value is not a pure target property.** Several VERIFIED-SOFTENED rows agree that a contact's value depends on message, context and the person's own response. They are Tappin 2023, Cagala 2021, Blackwell 2026 and the grf tutorial (sverdrup-nd). The persuasion and reminder weights (wp, wi) are therefore unanchored.
- **How to test the score.** The only rigorous way to check the formula is a randomized knock/no-knock holdout. It would be scored with a rank-the-effect metric (RATE/Qini), as described in the grf tutorial. The 60-door turf default assumes a constant time per contact, which the UNVERIFIED Kalla & Broockman 2020 row puts in question.

**Code-side findings from reading untapped.py.** These were found while checking the evidence against the code, and they change the reading of two components:
1. **Calibration changes who ranks first.** Calibrated t feeds both potential terms (untapped.py:118-119, 254). The persuasion term rises with t and the reminder term falls with it, so a monotone recalibration of t changes the order of *potential*, not just of t. For example, take person A (s 0.5, t 0.5) and person B (s 1.0, t 0.2) at wp 0.5. Under the raw scores B ranks first. After a curve that maps 0.5 to 0.75 and 0.2 to 0.4, A ranks first. "Ranking unchanged" is true only of t alone.
2. **Unknown support is treated as undecided.** A person with no support row defaults to s = 0.5 (untapped.py:236), which is exactly the persuasion peak. Unknown support is therefore scored as maximally undecided.

The 1-2 pp moderation magnitudes in Broockman & Kalla are pooled across opposite-signed groups. They are labeled UNSTRATIFIED and are not a canvass effect size.

## Candidate support score s

**What it computes.** Equal-weight average of oriented partisan score, presidential support, registration and recent primary ballot (0-100).

**Method.** Missing signals drop out of the denominator. Primary mode awaits a spec.

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/tools/support.py`

**Assumptions.**
- Vendor scores and registration point toward party

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[broockman-2026-should-moving-middle]] | 2026 | qualifies | Vote choice responds to a candidate's specific issue positions, and the response depends on where the voter stands on each issue. Effects range from -4.2 pp (transgender sports) to +7.3 pp (gay and lesbian rights) by issue for Republican candidates and from -1.7 pp (Social Security) to +4.5 pp (affirmative action) for Democratic candidates. On majority-supported issues, moderating hurts Republicans (-0.0237, SE 0.0048); the Democratic estimate is negative but not significant (-0.0042, SE 0.0041). | The paper partly supports a party-only baseline: issue effects are modest next to partisanship, and the authors themselves say not to over-read the point estimates. It also shows that a moderate or out-of-step candidate's support is not well proxied by party lean among cross-pressured voters. The evidence comes from the presidential level and a national sample, and candidates were shown without records. Carrying it over to a specific House candidate requires knowing that candidate's issue profile, which Terrier does not hold. | moderate | VERIFIED |

**Assessment.**

**Evidence weight: thin. One VERIFIED bearing qualifies the component, and nothing validates it.**

**What the evidence shows.** The only bearing is Broockman & Kalla 2026 (VERIFIED, moderate, *qualifies*). Vote choice responds to a candidate's specific issue positions, and the sign of the response depends on where each voter stands on each issue:
- For Republican candidates, issue effects run from -4.2 pp to +7.3 pp.
- For Democratic candidates, they run from -1.7 pp to +4.5 pp.
- On majority-supported issues, moderating hurts Republicans (-0.0237, SE 0.0048).

**What s computes.** s is an equal-weight average of party-oriented signals: partisan score, Harris/Trump support, registration and primary ballot. It has no input for the candidate's own positions, so every same-party candidate in a district receives the same s.

**How far the result transfers.** Transfer is limited:
- The design is a national-sample survey conjoint for a hypothetical 2028 presidential race.
- Candidates were shown without records.
- It is not a House race and not OR-05 or CT-01.

The paper partly *supports* a party-only baseline, because the issue effects are modest next to partisanship. It also warns that among cross-pressured voters, a moderate or out-of-step candidate's support is poorly proxied by party lean.

**What the pool does not test.** No row checks the system's own empirical question: whether the composite predicts candidate vote choice against survey or precinct returns.

**Code note, not literature.** A person with no support row is set to s = 0.5 (untapped.py:236). Downstream, that is the persuasion peak, so missing data is treated as undecided. Because missing signals drop out of the denominator, a person with only a registration value gets s equal to that one signal. No evidence speaks to whether those mixed-coverage scores are comparable.

**Contradictions.**
- The assumption that vendor scores and registration point toward the party holds for party-level choice. Broockman & Kalla 2026 (VERIFIED) shows that the sign of response is issue-specific and depends on the voter's position. A party-oriented s therefore cannot tell a moderate candidate's cross-pressured supporters from cross-pressured opponents.
- This is code versus intent rather than evidence. Unscored people default to s = 0.5, which reads 'unknown' as 'undecided' and gives them maximal persuasion weight in untapped potential.

**Gaps.**
- No validation of the s composite against vote choice. To fill it: match the voter file to a validated-vote survey, or run an ecological check of mean s against candidate two-party share by precinct in OR-05 and CT-01, reporting a calibration slope by district (stratified, not pooled).
- No evidence on equal weighting versus learned weights for the four signals, or on whether dropping missing signals biases s for thin-file voters. To fill it: a held-out comparison of equal-weight and fitted-weight composites, stratified by how many signals each person has.
- Primary mode has no specification and no evidence.

**Paul's position vs the evidence.**

Paul highlighted that returns to moderation are issue-specific, and the VERIFIED Broockman & Kalla 2026 bearing supports that reading directly. He left no annotation, and the system does not act on it: s has no issue or candidate-position dimension. His reading is correct, but it is not encoded. The paper also partly justifies the party-only baseline he built, so the honest summary is 'adequate baseline, blind to candidate positioning', not 'contradicted'.

**Open questions.**
- Should unscored voters keep s = 0.5, which places them at the persuasion peak, or be flagged and excluded from the persuasion term?
- Does Terrier hold, or plan to hold, any candidate issue profile? Without one, the Broockman & Kalla finding cannot be acted on.
- Is precinct-level validation of s against 2024 House returns feasible for OR-05 and CT-01 with the current geography build?

**Read first.** [[broockman-2026-should-moving-middle]], [[tappin-2023-quantifying-potential-persuasive]], [[cagala-2021-optimal-targeting-fundraising]]

## Chance of voting t with PAV calibration

**What it computes.** Raw t = mean of five turnout scores / 100, then a weighted PAV monotone curve per district fitted to own general-election records. OR mean 0.58 -> 0.83; CT 0.48 -> 0.74.

**Method.** Ranking unchanged. Refused below 1000 people.

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/untapped.py:109-277`

**Assumptions.**
- The past general record is the target
- Turnout is monotone in score

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment.**

**Evidence weight: none. This is a GAP.** No evidence row bears on this component. Its only literature tie is the pool-adjacent-violators (PAV) algorithm, which is a method citation, not evidence that the target or the transfer is valid.

**What the code does** (untapped.py:248-270):
- **Raw t.** Raw t is the mean of five vendor turnout scores divided by 100.
- **Fit.** A weighted monotone curve is fitted per district to the same people's own general-election records (voted / eligible), then applied to those same people. The fit is in-sample with no holdout.
- **No vendor score.** People without vendor scores receive their record rate directly. With no record either, they get EVEN_ODDS.
- **Size of the shift.** Calibration moves the mean a long way: OR from 0.58 to 0.83, and CT from 0.48 to 0.74.

That shift means one of three things: the vendor scores are badly miscalibrated to this target, the target is not the quantity the vendors score, or both. A record dominated by presidential-year generals will overstate midterm turnout.

**Cross-component consequence (derived arithmetic, not literature).** Calibrated t feeds both potential terms. The persuasion term (1-|2s-1|)·t rises with t and the reminder term s·(1-t) falls with it. A monotone transform of t therefore changes the *order of untapped potential* and shifts the effective balance toward persuasion without any change to wp. For example, with A at (s 0.5, t 0.5) and B at (s 1, t 0.2), B ranks above A on raw t. Once a curve maps 0.5 to 0.75 and 0.2 to 0.4, A ranks above B. The note 'Ranking unchanged' is true only of t itself.

**Contradictions.**
- This is internal, not from the literature. 'Ranking unchanged' holds for t, but calibrated t feeds untapped potential non-monotonically: persuasion multiplies by t, and the reminder term by (1-t). Calibration therefore re-ranks doors and shifts the effective weight between persuasion and reminder.

**Gaps.**
- Vendor turnout scores are not validated against observed turnout. To fill it: a prospective out-of-sample check. Fit the curve on the record up to cycle k, then score Brier, expected calibration error and a reliability diagram against validated turnout in cycle k+1. Report each district separately.
- Cross-cycle transfer is untested. To fill it: fit on presidential generals and test on a midterm general (and the reverse), because the 2026 target is a midterm and the 0.58 to 0.83 shift suggests the record is presidential-weighted.
- The isotonic fit is not checked against simpler alternatives. To fill it: compare it on held-out data with Platt or logistic recalibration and with no calibration. Check stability near the 1,000-person refusal floor.
- There is no literature in the pool on the accuracy of voter-file turnout scores or on turnout validation. Work on voter-file validation (for example Ansolabehere & Hersh on validated turnout) is a lead to source; it is not evidence yet.

**Paul's position vs the evidence.**

Paul's position is 'No literature', and the pool agrees: there is none. The component rests on a method (PAV) and an untested choice of target, namely that past general-election turnout is the right target for a future election. Neither is supported or contradicted by the evidence pool. The size of the mean shift is itself a warning sign that the target choice deserves a test before the calibrated t is trusted in potential.

**Open questions.**
- Which general elections make up the 'record' in each district: presidential only, midterm only, or both pooled?
- Should calibration feed untapped potential at all, given that it re-weights persuasion against reminder? Or should potential use raw t, or a cycle-matched calibrated t?
- When raw scores exist but are known to be miscalibrated, is the 1,000-person refusal floor the right fallback?

**Read first.** [[sverdrup-nd-introduction-grf-causal-forests]], [[cagala-2021-optimal-targeting-fundraising]], [[blackwell-2026-effect-political-advertising]]

## Untapped potential

**What it computes.** potential = (wp (1 - |2s-1|) t + (1-wp) s (1-t)) x (1 + wi x i), where i = sphere percentile / 100. Knocked doors are excluded.

**Method.** VERIFIED in untapped.py:9-10: the supporter weight is tied to 1 - wp, not a separate wR as the brain note describes. Defaults wp 0.5, wi 0.5.

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/untapped.py:1-455`

**Assumptions.**
- Persuasion value peaks at s = 0.5 among likely voters
- Mobilization value is in supporters unlikely to vote

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[broockman-2026-should-moving-middle]] | 2026 | qualifies | Voters who are moderate on average reward a candidate's move to the elite middle only on issues where they disagree with the party. Among general moderates holding the Republican position, the Republican-candidate effect is -0.0524 (SE 0.0112), against +0.0381 (SE 0.0072) among moderates holding the Democratic position (Table A9). Fewer than 10% of respondents gave consistently Democratic, Republican or elite-middle answers across five issues. Persuadability is therefore issue-specific and two-signed, not a property of sitting at the partisan midpoint. | The paper measures response to candidate issue positions in a survey conjoint: a hypothetical 2028 presidential race, a national sample of 2024 voters, and partial equilibrium. It does not measure the effect of a canvass contact, and it does not cover the OR-05 or CT-01 House electorates. It shows that a mid-scale partisan score mixes voters who move in opposite directions on any given issue. That implies a midpoint score ranks people by how cross-pressured they are, not by how persuadable they are by an arbitrary message. It does not show that the midpoint is the wrong target overall. The 1-2 pp pooled magnitudes are labeled UNSTRATIFIED and are not a canvass effect size. | moderate | VERIFIED |
| [[kalla-2023-which-narrative-strategies]] | 2020 | qualifies | Durable persuasion at the door depends on the conversation content. Perspective-getting narratives drive the effect, while analogic and vicarious steps add nothing reliable. Study 1 dichotomized health-policy items rose 5-9 pp. Study 2 overall index: Full minus PG-only 95% CI -0.05 to 0.09 SD; scholarships +12.7 pp, in-state tuition +7.4 pp. Study 3 DiD about 0.12 SD (p<.01, non-randomized). Effects held at least 11 weeks (Study 2) and 4.5 months (Study 1). Estimates are ITT among door-openers, about 81-82% of whom engaged (multiplier about 1.22-1.23). The main text reports no heterogeneity by baseline attitude. | The evidence is weak for this formula. The outcomes are explicit issue attitudes toward outgroups (immigrants, transgender people) in 2019-2020, delivered by trained advocacy-org canvassers. They are not candidate vote choice or turnout, which are what s and t encode. The paper neither supports nor refutes the peak at s=0.5 because it does not report effects by prior position. What carries over is narrower: if Terrier canvasses run a persuasion script, the expected per-contact lift depends on the script, and effects of a few tenths of an SD can last for months. The value of the persuade term is therefore conditional on the protocol, not fixed by target selection. The DiD and within-subject results are observational (UNVERIFIED-grade), so only the randomized Study 1 and Study 2 contrasts should be quoted. | weak | UNVERIFIED |
| [[blackwell-2026-effect-political-advertising]] | 2026 | qualifies | Democratic interest-group TV ads raise the Democratic two-party share mainly by raising Democratic turnout, with a weaker negative effect on Republican turnout. Republican interest-group ads have no estimated effect on any outcome. The Democratic effect is concentrated after Trump's candidacy; the pre/post-Trump effect differences are significant at alpha 0.1 (no level is stated in the text for the pooled effects). No point estimates are given in the text. | Weak. The treatment is broadcast TV ads aggregated to the media market in Senate and gubernatorial races, 2010-2020. It is not door-to-door contact, and the outcome is not individual. The finding suggests that mobilization effects can dominate and can differ by party and era, so a fixed, party-symmetric wp is a judgment call that this paper neither validates nor sets. It does not transfer to US House districts (OR-05, CT-01) or to canvassing without field-experiment evidence. | weak | VERIFIED-SOFTENED |
| [[cagala-2021-optimal-targeting-fundraising]] | 2021 | suggests-extension | Responsiveness to a costly contact is heterogeneous. Warm-list gift effects (2014, first-year net donations) run from -11.59 euro at the 5th percentile to +17.99 euro at the 95th, and 45 percent of warm-list individuals have estimated effects above the 1.16-euro cost (Section 5.2, Figure 2). A rule built on 300-m geospatial counts, distances and elevation (17.40 euro expected net donation) does not significantly differ from the full-covariate rule (17.61 euro; Table 6, column 6 difference -0.22, not significant). | Applies to individual-level targeting of a costly contact, which is structurally closest to Terrier. The numbers do not transfer: they come from charitable gifts, not canvass persuasion or GOTV. A learned rule would need randomized contact assignment, such as held-out control households per turf, because saved walk results may be selected on who was knocked (plausible, not checked against walk data). | moderate | VERIFIED-SOFTENED |
| [[simchon-2024-persuasive-effects-political]] | 2024 | context | Personality-matched messaging produces small attitudinal increments (b = -0.03 to -0.08 per SD of mismatch on a 1-5 self-report), which the authors translate to 2,490 to 11,405 extra persuaded per 100,000 under a midpoint cutoff, with no behavioural outcome. | Only relevant if Terrier ever tailors canvass scripts by inferred voter traits; the paper's effects are on perceived persuasion of ads in a UK online panel, not doorstep vote choice, so it does not argue for changing the potential formula. | weak | UNVERIFIED |
| [[tappin-2023-quantifying-potential-persuasive]] | 2023 | suggests-extension | Study 1 (Lucid, Feb 2022; 5,284 analyzed across both modules). Citizenship Act: microtargeting 5.17 pp vs single-best-message 2.41 vs naive random message -0.96 (P=0.016 and P<0.001). Precision-weighted across both modules: 5.96 vs 3.48 vs 1.79. UBI module: microtargeting 7.16 vs single-best-message 5.03 vs naive 5.70, not significant. Profiles were ranked with generalized random forests (grf R package) over 250 Monte Carlo 75/25 splits. The advantage was qualitatively largest for one-covariate targeting, though differences were slight. | Tappin's effects are online survey estimates from Feb 2022 U.S. respondents with forced exposure on low-salience policy attitudes. They are not field or door-canvass effects. The paper says candidate-centered and non-U.S. settings are untested, and it says survey estimates may overstate the relative advantage of microtargeting in the field. A party or ideology split is the coarse covariate the paper found sufficient in-sample, and Terrier holds registration and partisan scores. Any gain stays UNVERIFIED until Terrier runs its own pretest. | moderate | VERIFIED-SOFTENED |
| [[tappin-2023-quantifying-potential-persuasive]] | 2023 | qualifies | The authors say persuasion returns may be highly context dependent. Study 1 Citizenship Act significant (5.17 vs 2.41, P=0.016); UBI not significant (7.16 vs 5.03, P=0.12). Study 2 across 36 issues: per-person issue targeting was directionally below one best-issue ad (12.30 vs 15.42, P=0.11). The authors say survey estimates may overstate the relative persuasive advantage of microtargeting. Candidate-centered and non-U.S. campaigns are untested. | The paper does not estimate effects by distance from s=0.5, so it neither supports nor refutes the peak assumption. It says effect sizes vary by issue and setting and are likely smaller in high-salience contexts with countermessaging. Treat wp as unanchored until Terrier has local contact results. | weak | VERIFIED-SOFTENED |
| [[madsen-2017-strategic-advantages-micro]] | 2017 | qualifies | Persuasion contact with voters who give the messenger credibility at or below 0.5 is inefficient or harmful. Targeting only receptive voters (signal factor above 0.5) beats random contact at equal reach; random contact needs about 2x the reach to break even. Simulation only: N=10,000 agents, 50 ticks, targeted reach 20 per tick. | Partial transfer. Both concern voter-level persuasion targeting in a two-candidate race. The paper, however, uses unconnected simulated agents, a single proposition and no turnout, and its credibility values are drawn from distributions, not measured. It supports adding a messenger-credibility or backfire guard as a hypothesis to test against canvass results. It does not support any particular weight. Strength is weak because these are uncalibrated ABM outputs with no replications reported. | weak | UNVERIFIED |
| [[sverdrup-nd-introduction-grf-causal-forests]] |  | suggests-extension | RATE/TOC tests whether a ranking sorts units by treatment effect. In the Brazil RCT (n about 17,000 students, 850 schools), ranking by low financial autonomy gave AUTOC 0.69 (SE 0.22, t about 3.2). Ranking by out-of-bag forest CATE gave one-sided p = 0.65 (the text says this rejects, which the printed p-value does not). Qini curves for budgeted targeting and depth-1 policy trees from doubly robust scores are described (pp.7-10). | Needs randomized or known-propensity knock vs no-knock assignment with logged outcomes (support ID or validated turnout), scored as RATE on held-out doors. Without randomized contacts, any test is an observational CATE that assumes unconfoundedness. Not transferable from this tutorial as evidence about the formula. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: many bearings, little that is direct. The formula's two core assumptions are untested by any VERIFIED row.**

**Assumption 1: persuasion value peaks at s = 0.5 among likely voters.**
- Broockman & Kalla 2026 (VERIFIED, moderate, *qualifies*) shows that general moderates move in opposite directions by issue position. Among moderates who hold the Republican position, the Republican-candidate effect is -0.0524 (SE 0.0112). Among moderates who hold the Democratic position, it is +0.0381 (SE 0.0072).
- So a midpoint score ranks people by how *cross-pressured* they are, not by how persuadable they are by an arbitrary message. This qualifies the peak; it does not refute it.
- Tappin 2023 (VERIFIED-SOFTENED) and Kalla & Broockman 2020 (UNVERIFIED) both say explicitly that they do not estimate effects by distance from the midpoint. The pool therefore neither confirms nor refutes the peak.
- Madsen 2017 (UNVERIFIED; a simulation with no calibration) suggests that contacting voters who find the messenger low-credibility can backfire. That is a hypothesis to test, not a weight to adopt.

**Assumption 2: mobilization value lies in supporters unlikely to vote.**
- No row tests get-out-the-vote (GOTV) effects by turnout propensity.
- Blackwell 2026 (VERIFIED-SOFTENED, weak) finds that ad effects work mainly through Democratic turnout and differ by party and era. This is TV ads aggregated to the media market in Senate and gubernatorial races, 2010-2020, not canvassing. It weakens the case for a fixed, party-symmetric wp; it does not set a value.
- Note that the term s·(1-t) peaks at t near 0. Whether the lowest-propensity supporters are the most mobilizable is exactly the open empirical question.

**Weights wp and wi.**
- The defaults are 0.5 and 0.5. They are module defaults, not rulings.
- Tappin 2023 says returns are context-dependent and likely smaller under high salience and counter-messaging. Cagala 2021 (VERIFIED-SOFTENED, moderate) shows that responsiveness to a costly contact is heterogeneous: 45% of warm-list individuals have estimated effects above cost. That is fundraising, not vote choice.
- Nothing anchors either weight. The sphere multiplier (1 + wi·i) has **no evidence at all**. Under D-025, i is a heuristic model output, not a measure of reach or spillover.

**Path to a test.**
- The grf tutorial (sverdrup-nd; VERIFIED-SOFTENED, moderate) and Cagala 2021 point to the same design: randomized knock/no-knock holdouts per turf, with logged support ID or validated turnout, and the potential ranking scored with RATE/AUTOC or Qini curves on held-out doors.
- Saved walk results cannot stand in for this test, because which doors got knocked was chosen, not randomized.

**Protocol.**
- Kalla & Broockman 2020 (UNVERIFIED, weak) is a lead that per-contact lift depends on the script. Its outcomes are outgroup attitudes, not vote choice.
- Only its randomized Study 1 and Study 2 contrasts should be quoted. The Study 3 difference-in-differences is non-randomized.

**Code facts.**
- The supporter weight is 1 - wp (untapped.py:9-10). The brain note describes a separate wR, so the documentation and the code disagree.
- Calibrated t feeds both terms (untapped.py:118-119, 254), so the turnout calibration silently re-weights persuasion against reminder.

**Contradictions.**
- Broockman & Kalla 2026 (VERIFIED) qualifies the assumption that 'persuasion value peaks at s = 0.5'. The midpoint holds voters who respond with opposite signs on any given issue, so (1-|2s-1|) measures cross-pressure, not persuadability to a particular message. This is a qualification, not a refutation.
- This is a contradiction across Paul's own systems. Melange's flip and money-response models cite Kalla & Broockman's finding that general-election persuasion is about zero (citations_in_system: flip_model.py:21, money_response.py). Terrier, by default, puts half its weight (wp = 0.5) on general-election persuasion of likely voters. Both cannot be the working assumption without an explicit ruling on why the doorstep differs.
- The brain note describes a separate supporter weight wR, but the code ties it to 1 - wp (untapped.py:9-10). The documentation contradicts the implementation.
- Turnout calibration is described as ranking-neutral, but it changes potential's ranking and the effective balance between persuasion and reminder. This is derived from the formula plus untapped.py:254.

**Gaps.**
- No field evidence on how canvass persuasion effects vary with prior support, which is the component's first empirical question. To fill it: a door-canvass RCT that reports conditional effects by baseline support band, or a re-analysis of published canvass RCT replication data stratified by pre-treatment partisanship.
- No evidence on whether GOTV contact works best among low-to-middle propensity supporters, which is the second empirical question. To fill it: GOTV field-experiment heterogeneity by propensity score, cycle-matched (midterm versus presidential). The literature on propensity-dependent GOTV effects (for example Arceneaux & Nickerson 2009) is a lead to source and verify, not yet in the pool.
- No evidence for the sphere multiplier (1 + wi·i). To fill it: a household- or network-spillover field design that randomizes contact and measures turnout or support of non-contacted household or sphere members. Until then, i stays a heuristic rank.
- No local effect-size evidence for wp. To fill it: randomized knock/no-knock holdouts per turf, scored with RATE/Qini on the potential ranking, and a separate check of the persuasion and reminder components.
- No party or era asymmetry term. Blackwell 2026 hints that one matters, but only for TV ads in Senate and gubernatorial markets; canvass-specific evidence is needed.

**Paul's position vs the evidence.**

Paul cites Kalla & Broockman 2020 and Broockman & Kalla 2026 and highlighted the door protocol at length: nonjudgmental exchange, perspective-getting, and the 7-minute version. Only the 2026 citation is VERIFIED in the pool, and it qualifies his formula rather than supporting it. The 2020 bearing is UNVERIFIED, weak, and measured on outgroup attitudes, not vote choice. His own observation that the protocol 'is not encoded in the persuasion term' is the right reading of the evidence. The literature he highlighted implies the persuasion term's value is conditional on the script, while the formula treats value as a property of the target alone. So his highlights point to a gap he has not yet closed, and the wp default has no evidential basis either way.

**Open questions.**
- Which Melange assumption about persuasion applies to the doorstep: about zero in general elections, or worth half the weight? Is there a stated reason the doorstep differs?
- Will Terrier canvasses run a persuasion script, and which one? If not, should wp default toward reminder?
- Is a randomized holdout (for example 5-10% of doors per turf left un-knocked) acceptable to the campaigns, so that a RATE/Qini test of the ranking is possible?
- Should the brain note's wR be corrected to 1 - wp, or should the code gain a separate wR?
- What does the sphere percentile i measure in Terrier, and is there any outcome it has been checked against?

**Read first.** [[broockman-2026-should-moving-middle]], [[tappin-2023-quantifying-potential-persuasive]], [[sverdrup-nd-introduction-grf-causal-forests]], [[cagala-2021-optimal-targeting-fundraising]], [[kalla-2023-which-narrative-strategies]], [[blackwell-2026-effect-political-advertising]]

## Street-plan presets, turf planner and walk routing

**What it computes.** Block value per 100 m (mixed-support, supporter turnout or identification). The turf grows along connected streets to 60 doors (400 m jump). Dijkstra routing on the noded TIGER network.

**Method.** Blocks need at least 5 doors; length floored at 50 m.

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/walk_turf.py`; `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/walk_views.py:636-774`; `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/tools/districts.py:57-64`

**Assumptions.**
- Value per walking distance is the efficiency criterion

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kalla-2023-which-narrative-strategies]] | 2020 | suggests-extension | A perspective-getting-only conversation averaged 7.3 minutes, against 17.1 minutes for the full intervention, with statistically indistinguishable effects (Study 2). In Study 1, removing the analogic step gave 15.5 vs 18.5 minutes with no loss of effect. | This holds only if Terrier volunteers deliver a persuasion conversation, not a reminder or ID knock. The timings come from trained canvassers on an issue-attitude script, so they bound door throughput only roughly. A fixed 60-door target implicitly assumes a constant time per contact. The paper suggests that a conversation-minutes parameter, or a mode switch between remind and persuade, would change feasible list size by about 2x. | weak | UNVERIFIED |
| [[lawande-2022-systematic-review-analysis]] | 2022 | supports | With obstacles (Table 4, single run), A* expands fewer blocks than Dijkstra at 16x16 (140 vs 177) and 32x32 (587 vs 754) with the same path length (31, 75); at 8x8 A* expands more (32 vs 30). Execution times at 32x32 are 3975.34 vs 5678.25 ms. A* uses F(n)=G(n)+H(n). | Closest match in the system (spatial graph, metric estimate). Road networks are not uniform grids; paper figures are single runs with unreported hardware; gains appear only from 16x16 up. Supports the design choice, gives no measured speedup for street networks. | weak | VERIFIED |
| [[luo-2020-path-planning-algorithm]] | 2020 | context | Multiple targets are visited greedily, each time taking the nearest remaining target by Euclidean distance (Eq. 3). Multiple static targets took 0.23 to 0.91 s on 394.8 to 752.6 m paths (Table 2). No comparison to an ordering optimizer is reported. | Nothing to adopt directly. The paper's single greedy pass is weaker than the ordering already built. A geometric heuristic is valid for street routing, and the system already uses one. | weak | VERIFIED-SOFTENED |
| [[esmalian-2022-characterizing-equitable-access]] | 2022 | context | "Approaches focused on physical distance ... is not a reliable measure of accessibility during disasters". Trip time and trip distance diverged by income group during preparation: lower-income zones with long baseline trips saw the largest increase in trip time, and high-income zones with short baseline trips saw the largest increase in distance. | Carries over only if canvassing happens under disrupted conditions, such as flooding or road closures, when walk time stops tracking distance. For normal door-to-door canvassing on foot, distance is an adequate cost, and the paper offers no evidence against it. This is recorded as context, not a proposed change. | weak | UNVERIFIED |

**Assessment.**

**Evidence weight: weak and indirect. The efficiency criterion is untested.**

**What the evidence covers.** The only VERIFIED row is Lawande 2022 (weak, *supports*): A* expands fewer nodes than Dijkstra on 16x16 and 32x32 video-game grids, finding the same path length.
- It supports the routing design choice. The evidence row reports that walk_route._astar uses a straight-line heuristic and falls back to Dijkstra when a node or goal lacks coordinates.
- It gives no measured speedup on street networks: these are single runs on grids, with unreported hardware.
- Luo 2020 (VERIFIED-SOFTENED, context) uses a greedy nearest-next ordering. That is weaker than the exact solve plus 2-opt and or-opt ordering already built, so there is nothing to adopt.
- Esmalian 2022 (UNVERIFIED, context) says physical distance stops tracking access time under disruption. For normal canvassing on foot it offers no argument against using distance as the cost.

**Throughput.** The relevant bearing is Kalla & Broockman 2020 (UNVERIFIED, weak):
- A perspective-getting-only conversation averaged 7.3 minutes.
- The full intervention averaged 17.1 minutes.
- Effects were statistically indistinguishable (Study 2).

A fixed 60-door turf assumes a constant time per contact. If Terrier mixes persuasion and reminder knocks, feasible list size could vary by about 2x. That is a lead for adding a conversation-minutes parameter or a mode switch, not a finding.

**The criterion itself.** No row tests the component's own empirical question: whether turfs built on value per 100 m produce more contacts per hour, or more value per hour, than precinct-order lists. The thresholds have no evidential basis either way:
- the 5-door block minimum
- the 50 m length floor
- the 400 m jump allowance
- the 60-door target

**Description mismatch.** The component description says 'Dijkstra routing', but the evidence row's code reading says A* with a Dijkstra fallback. The description should be reconciled with walk_route.py.

**Contradictions.**
- The component description says 'Dijkstra routing on the noded TIGER network'. The evidence row's reading of walk_route.py says the router is A* with a straight-line heuristic, degrading to Dijkstra only when coordinates are missing; a plain dijkstra() helper sits at lines 176-197. This is a documentation mismatch, not a fault in the evidence.

**Gaps.**
- No evidence that turf built on value per 100 m raises contacts per hour or value per hour over precinct-order or walk-list baselines. To fill it: a turf-level field A/B test that randomizes turf-construction method across shifts or volunteers and measures doors attempted, contacts and conversations per hour, and summed potential reached.
- No evidence on time per door by contact mode (reminder, ID, persuasion) for Terrier volunteers. To fill it: log timestamps per door in walk results and estimate minutes per contact by mode, then size turfs by time rather than by a fixed door count.
- No sensitivity analysis of the 5-door, 50 m, 400 m and 60-door thresholds. To fill it: an offline sweep on OR-05 and CT-01 street networks, reporting how turf value and walk length change at each threshold.
- No street-network benchmark of A* against Dijkstra. Lawande's grid results do not transfer; an offline timing run on the TIGER graph would settle it, if latency matters at all.

**Paul's position vs the evidence.**

Paul's position is 'Indirect only', and that is accurate. His one citation, Lawande et al. 2022, is VERIFIED but concerns video-game grid pathfinding: it supports choosing A* and says nothing about turf value or canvass efficiency. The pool adds no direct evidence on his efficiency criterion. The most decision-relevant lead is the UNVERIFIED conversation-length result from Kalla & Broockman 2020, which he highlighted under the protocol but which bears here on turf size.

**Open questions.**
- Should turf size be set in minutes rather than doors once a contact mode is chosen?
- Are walk-result timestamps captured, so that minutes per contact can be measured locally?
- Is a turf-construction A/B test feasible with the volunteer pool in OR-05 or CT-01?
- Should the component description be corrected from 'Dijkstra' to 'A* with Dijkstra fallback'?

**Read first.** [[kalla-2023-which-narrative-strategies]], [[lawande-2022-systematic-review-analysis]], [[luo-2020-path-planning-algorithm]], [[esmalian-2022-characterizing-equitable-access]]

## Across components

**1. Turnout calibration feeds untapped potential non-neutrally (t into potential).** This is the most consequential coupling, and it comes from the code, not the literature. untapped.py:254 replaces raw t with the calibrated curve before the persuasion and reminder terms are built (lines 118-119). Persuasion multiplies by t and reminder by (1 - t), so the sizeable upward shift (OR mean 0.58 to 0.83; CT 0.48 to 0.74) does three things:
- It inflates the persuasion term relative to the reminder term.
- It changes which doors rank highest, which a worked two-person example confirms.
- It shifts the effective persuasion/reminder balance away from the wp weight Paul set.

The claim that calibration leaves the ranking unchanged is true of t alone, not of the ranking that drives turfs.

**2. Missing support data lands at the persuasion peak (s into potential).** Unscored people default to s = 0.5 (untapped.py:236), which maximizes (1 - |2s - 1|). Gaps in vendor and registration coverage therefore become the highest persuasion targets. Thin-file voters are not random, so this can tilt turfs toward under-documented areas for reasons the formula does not intend.

**3. Potential sets block value, which shapes the turf (potential into turf).** The turf planner ranks blocks by summed potential per 100 m. Every unanchored weight in potential (wp, wi, the s = 0.5 peak, the sphere multiplier i) passes straight into turf shape and walk order. Testing the turf planner's efficiency is meaningless until the value it maximizes has been tested.

**4. The protocol links two components.** The UNVERIFIED Kalla & Broockman 2020 lead bears on two components at once:
- Untapped potential: the per-contact lift depends on the script.
- Turf: conversation length differs by about 2x between versions with indistinguishable effects.

A single 'contact mode' setting (reminder, ID, persuasion) would, if adopted, have to set both the persuasion weight and the turf size together.

**5. One experiment answers most of the open questions.** The VERIFIED-SOFTENED grf and Cagala bearings converge on a single design. Randomize a held-out set of un-knocked doors per turf, log outcomes (support ID and validated turnout), and score the potential ranking with RATE or Qini. Run per district and stratified by cycle, it would address three things at once: the s = 0.5 peak, mobilization by propensity, and whether calibrated or raw t ranks better.

**6. Paul's systems disagree about persuasion.** Melange's flip and money-response models cite Kalla & Broockman's finding that general-election persuasion is about zero. Terrier puts default weight 0.5 on general-election persuasion. The two systems need an explicit ruling or a stated reason why doorstep contact differs.

**Wording guards.**
- The sphere percentile i is a heuristic rank (D-025), not a measure of reach.
- All pooled moderation magnitudes from Broockman & Kalla are UNSTRATIFIED.
- No figure in this assessment is a canvass effect size.

## Research agenda

1. Run a randomized knock/no-knock holdout in each Terrier district: leave 5-10% of doors per turf un-knocked, log support ID and validated turnout, and score the untapped-potential ranking with RATE/AUTOC and Qini on held-out doors. Report each district and cycle separately; do not pool them.
2. Validate turnout calibration prospectively: fit the PAV curve on records up to cycle k, then score Brier, expected calibration error and reliability on validated turnout in cycle k+1. Test presidential-to-midterm transfer explicitly, given the 0.58 to 0.83 mean shift in OR.
3. Decide whether untapped potential should use raw t, calibrated t, or cycle-matched calibrated t, since calibration re-ranks doors and shifts the persuasion/reminder balance (untapped.py:118-119, 254).
4. Validate the support composite s against precinct-level House returns, or validated-vote survey matches, in OR-05 and CT-01. Stratify by how many of the four signals each person has, and settle how unscored people (now s = 0.5) are treated.
5. Source and verify the heterogeneity literature on GOTV by turnout propensity (for example Arceneaux & Nickerson 2009) and on canvass persuasion by prior support. Both are leads and are not yet in the evidence pool.
6. Reconcile Terrier's wp = 0.5 general-election persuasion weight with Melange's cited finding that general-election persuasion is about zero (Kalla & Broockman), with an explicit ruling.
7. Instrument walk results with per-door timestamps and contact mode, estimate minutes per contact, and test sizing turfs by time instead of the fixed 60-door target, following the Kalla & Broockman 2020 lead on conversation length.
8. Run a turf-construction field A/B test (value-per-100 m turfs against precinct-order lists), measuring contacts per hour and summed potential reached per hour.
9. Fix two documentation mismatches: the brain note describes a separate wR, but the code ties the supporter weight to 1 - wp; and the component describes 'Dijkstra routing', but the code uses A* with a Dijkstra fallback.
10. Before the sphere multiplier (1 + wi·i) keeps nonzero default weight, test it with a household- or sphere-spillover design. Until then, keep i labeled as a heuristic rank.
