---
type: system
name: "Moneyball four-pillar influence rank"
slug: moneyball-rank-pillars
components:
  - moneyball-rank-pillars.score-blend
  - moneyball-rank-pillars.committee-pillar
  - moneyball-rank-pillars.legislative-pillar
  - moneyball-rank-pillars.press-pillar
  - moneyball-rank-pillars.demographic-pillar
  - moneyball-rank-pillars.sublayer-efficiency-repair
  - moneyball-rank-pillars.network-centrality-influencescore
  - moneyball-rank-pillars.undervaluation-lens
  - moneyball-rank-pillars.legacy-workbook-cascade
brain_notes:
  - C:/Users/pgoff/Documents/brain/concepts/four-pillar-percentile-blend.md
  - C:/Users/pgoff/Documents/brain/concepts/influence-vs-alignment-axes.md
  - C:/Users/pgoff/Documents/brain/actors/moneyball-workbook.md
  - C:/Users/pgoff/Documents/brain/actors/moneyball-workbook-rules-scoring-and-sublayer-surgery.md
  - C:/Users/pgoff/Documents/brain/actors/moneyball-workbook-lineage-and-june-audit.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-rfc-melange-architecture-system-of-record-model-config-and-committee-formula.md
paper_count: 27
tags: [system]
---

# Moneyball four-pillar influence rank

The official 533-member CU-influence rank. It blends Legislative, Press, Influence (committee) and Demographic pillars into a score; ranks_python.json (fec/engine/compute_pillars.py) is the rank authority and the workbook is a view of it. Includes each pillar's construction, the legacy workbook cascade and the underdog lens. Code paths are relative to C:/Users/pgoff/OneDrive/Desktop/Melange Systems.

**Brain notes:** [concepts/four-pillar-percentile-blend](obsidian://open?vault=brain&file=concepts/four-pillar-percentile-blend); [concepts/influence-vs-alignment-axes](obsidian://open?vault=brain&file=concepts/influence-vs-alignment-axes); [actors/moneyball-workbook](obsidian://open?vault=brain&file=actors/moneyball-workbook); [actors/moneyball-workbook-rules-scoring-and-sublayer-surgery](obsidian://open?vault=brain&file=actors/moneyball-workbook-rules-scoring-and-sublayer-surgery); [actors/moneyball-workbook-lineage-and-june-audit](obsidian://open?vault=brain&file=actors/moneyball-workbook-lineage-and-june-audit); [topics/2026-07-rfc-melange-architecture-system-of-record-model-config-and-committee-formula](obsidian://open?vault=brain&file=topics/2026-07-rfc-melange-architecture-system-of-record-model-config-and-committee-formula)

**Moneyball four-pillar influence rank: evidence assessment (35 rows, 27 distinct works; 2 UNVERIFIED, 8 VERIFIED, 25 VERIFIED-SOFTENED)**

**Overall: the literature supports the system's direction but none of its numbers.** Every weight in the system is owner policy: the pillar weights, the committee tier map (3.0 / 1.8 / 0.9), the 1/6 subcommittee factor, the leadership ladder, the 4:2:1 sponsor-role ratio, the 0.7/0.3 agenda/efficiency blend, the press category tiers and the 0.60/0.25/0.15 centrality mix. No paper in the evidence set estimates any of them, and none of them has been checked against an outcome. That matches Paul's own position on the blend ("owner policy that no literature tests"). It holds equally for every pillar.

**Where the evidence is strongest**
- **Retiring direction-blind cosponsor centrality from Influence.** Battaglini et al. 2018 is the only federal study in the set with a legislative-effectiveness outcome (House, 109th-113th, N = 2,176, pooled, UNSTRATIFIED by party). After correcting for endogenous tie formation, betweenness and eigenvector centrality lose all significance against LES. Sotoudeh et al. 2024 (House 2009-2019, UNSTRATIFIED) finds that eigenvector, closeness and strength barely separate passed from failed bills.
- **Committee position as the place where money and lobbying concentrate.** Kim et al. 2020/2023, de Figueiredo 2013 and Kim & Kunisky 2020 support this qualitatively.
- **A chair premium in effectiveness.** It survives Battaglini's correction: OLS 1.97 falls to 1.61 LES points, about 18% smaller (House 109th-113th).

**Where the evidence cuts against the build**
1. **House and Senate are pooled into one scale** (one percentile for Leg%, one raw/max for Influence%). Bucchianeri et al. 2025 says effectiveness scores are relative to each chamber-term and should not be compared across chambers. Its chair premium also differs sharply by chamber: pooled 0.507, strata 0.609 lower chamber and 0.307 upper (NC 2005-2012).
2. **Majority status is ignored.** It is a large effectiveness covariate: Bucchianeri reports +0.370 pooled, strata 0.362 / 0.387 (NC 2005-2012). Box-Steffensmeier et al. 2018 reports a majority-sponsor logit of 1.353 on House passage (106th-111th, N = 6,597 letter-bearing bills, UNSTRATIFIED). Neither the efficiency term nor the committee score has a majority term. That raw advancement rates therefore reward majority status is an analyst inference, not a paper finding.
3. **The underdog lens keeps the measure that was retired.** It still uses centrality as "intrinsic" influence, which is the same centrality the evidence says carries no corrected signal. Its conceptual anchor (Aref & Neal 2021) defines the hidden bloc by majority alignment, which is closer to what the lens subtracts than to what it rewards.
4. **Hit-rate efficiency underperforms in the one test available.** In NC, hit-rate metrics did worse than stage-based effectiveness scores against expert rankings (Bucchianeri). The efficiency term is a hit-rate-style metric.

**Gaps (no evidence rows)**
- **Demographic pillar:** no evidence.
- **Sub Layer repair and legacy cascade:** engineering only; no literature applies.
- **Press pillar:** no paper tests own-domain filtering, or whether press stance predicts later CU behaviour. The two verified press rows (Haselmayer 2017; Osnabrugge et al. 2021) only qualify the LSD intensity multiplier.
- **Weighted out-degree (60% of InfluenceScore):** no paper tests it against any outcome.

**Unresolved internal contradiction.** Three pillar weight sets disagree:
- code (pillar_policy.py:11-16): 45/10/40/5;
- MATHEMATICAL_SPECIFICATION.md:12: 25 Leg / 30 Press / 40 / 5;
- ratified set as implied by the code docstring: 30 Leg / 25 Press / 40 / 5.

This is Paul's ruling to make (code-review S-5). No literature resolves it.

**Verification note.** Several evidence agents could not find the AB13 = 0 retirement or the underdog-lens wiring anywhere under fec/. Both live in the workbook (rows on Sotoudeh, Battaglini 2016 and Christopher). The lens has no code file. These facts rest on brain notes, not code reads.

**Flags.** No paper in this set argues for either rejected SNA proposal (dropping regular cosponsorship edges; per-edge volume normalization), so there is nothing to flag. Kim & Kunisky 2020 and Huneeus & Kim 2018 use LDA lobbying data. Under D-021 they may inform reading only, never edges or weights.

## Four-pillar score blend and rank

**Computes.** Score = w2*Leg% + w3*Press% + w4*Influence% + w5*Demo%; Rank = descending competition rank over 533 members. ranks_python.json is the rank authority.

**Method.** Linear weighted sum of normalized pillars. Normalization differs by pillar: legislative uses Excel PERCENTRANK.INC truncated to 5 decimals, committee uses raw/max, press uses signed raw/max, and demographic uses PERCENTRANK. Weight history: .40/.30/.20/.10 (2026-06-29, historical); 30/25/40/5 (ratified 2026-07-07); 45/10/40/5 (Leg/Press/Inf/Demo), live in fec/engine/pillar_policy.py since 2026-09-03 and unratified (code-review S-5 open). VERIFIED: pillar_policy.py:11-16 reads leg .45, press .10, inf .40, demo .05. Its docstring says 10 of the original 25 press points remain and 15 moved to legislative, which implies the ratified set was Leg .30 / Press .25. MATHEMATICAL_SPECIFICATION.md:12 instead writes 0.25*Leg + 0.30*Press, and brain notes disagree on the order. Unresolved.

**Code refs.** `fec/engine/compute_pillars.py:11-14,36-59,195-216`, `fec/engine/pillar_policy.py:11-16`, `fec/engine/apply_weights.py:11-13`, `MATHEMATICAL_SPECIFICATION.md:12`, `ranks_python.json`

**Assumptions**
- Pillar values after normalization are commensurable, so fixed weights are meaningful
- Weights are owner policy, not estimated from outcomes
- Rank measures influence; stance (alignment) is a separate axis
- Min-max was rejected because one outlier set each pillar's scale (effective variance shares at 06-25: Press 40 / Leg 6 / Committee 52 / Demo 2)

**Evidence** (3 rows, 3 distinct works; 3 VERIFIED-SOFTENED)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[christopher-2016-mathematical-modeling-analysis]] | 2016 | method-source | Criterion weights come from AHP pairwise comparison on Saaty's nine-point scale, accepted at CR below 0.10 (here 0.062). Robustness follows Alinezhad and Amini Equation 1: change one weight in 0.1 steps, rescale the others proportionally, and track rank. Raising the out-degree weight keeps Freedom Partners Chamber of Commerce first. Raising the weight the paper labels lowest (closeness, Figure 5) lets Center to Protect Patients Rights overtake it. | The one-weight proportional-rescale sweep can be used directly for the open top-133 sensitivity question. AHP would only document owner judgment and check its internal coherence; it gives no outcome validity, and the paper's own weights are unvalidated. Any AHP-derived set must be labelled owner policy with its CR, never an estimate. | weak | VERIFIED-SOFTENED |
| [[bucchianeri-2025-legislative-effectiveness-american]] | 2025 | qualifies | Stage-based effectiveness is a valid construct: SLES explains about half the variation in NC expert rankings, and lagged SLES persists (rho 0.38 when majority entered, 0.40 when minority entered, 0.66 with no change in control). Effectiveness reflects innate or cultivated lawmaking ability as well as position. | The persistence and NC validity figures are from the state paper and do not validate the federal blend. The orthogonality reading depends on CEL LES being the federal analogue. Under bar B, +0.10 falls in the null band, which matches the rel's wording, but the +0.10 value itself was not verified. | moderate | VERIFIED-SOFTENED |
| [[becker-nd-gameability-redistricting-criteria]] |  | context | When a criterion admits several defensible metrics, each metric favors a different plan, so stakeholders can cherry-pick the metric their plan wins on. The paper recommends that states clarify metric definitions rather than set hard numeric thresholds. | Analogy only. The paper studies redistricting metrics, not legislator rankings, and provides no evidence about these weights. It supports pinning one documented weight set and reporting rank sensitivity across the competing sets; it does not justify any particular weights. | weak | VERIFIED-SOFTENED |

**Assessment**

**Four-pillar score blend and rank: 3 rows, 3 distinct works; 3 VERIFIED-SOFTENED. None tests the blend.**

**What the evidence offers**
- **Bucchianeri et al. 2025 (moderate).** Shows that stage-based effectiveness is a valid and fairly persistent construct in one US state. SLES explains about half the variation in NC expert rankings (2005-2012). Lagged persistence is rho 0.38 to 0.66 depending on changes in control. This supports using a federal effectiveness score (CEL LES) as the external criterion the blend's first empirical question asks for. It does not validate the federal blend.
- **The project's own convergence result is unverified.** Per the brain, the blend correlates with CEL LES at about +0.10. That falls in the null band of lep_criterion bar B (below 0.15). The value and its n were not verified in this check. If it holds, the blend is close to orthogonal to the main published effectiveness measure.
- **Christopher 2016 and Becker (method and context only).**
  - Christopher's one-weight proportional-rescale sweep is a ready method for the open top-133 sensitivity question. AHP would only document owner judgment with a consistency ratio; it is never an estimate.
  - Becker's gameability argument supports pinning one documented weight set and reporting rank sensitivity across the competing sets.

**Contradictions inside the system, not with the literature**
- **Three weight sets are in play.** Code is 45/10/40/5; MATHEMATICAL_SPECIFICATION.md:12 is 25/30/40/5; the docstring implies a ratified 30/25/40/5. The live set is unratified (S-5).
- **The commensurability assumption fails by construction.** The pillars are normalized four different ways: PERCENTRANK truncated (Leg), raw/max (Committee), signed raw/max (Press) and PERCENTRANK (Demo). Raw/max pillars keep their skew while percentile pillars are uniform, so the effective variance shares will not equal the nominal weights. The 06-25 min-max episode (Press 40 / Leg 6 / Committee 52 / Demo 2) shows how far they can drift. This is an internal computation, not a literature question.
- **House and Senate share one rank.** Bucchianeri says effectiveness scores are relative to each chamber-term and not comparable across chambers.

**Contradictions**
- Code pillar weights 45/10/40/5 (pillar_policy.py:11-16) vs MATHEMATICAL_SPECIFICATION.md:12 at 25 Leg / 30 Press / 40 / 5 vs the ratified 30 Leg / 25 Press / 40 / 5 implied by the docstring; unresolved, Paul's ruling (S-5).
- The assumption that normalized pillars are commensurable conflicts with the mixed normalizations (percentile vs raw/max vs signed raw/max), so nominal weights are not effective weights.
- Bucchianeri 2025 says effectiveness scores are chamber-term relative and not comparable across chambers; the blend ranks House and Senate members on one 533-member scale.

**Gaps**
- No study, in the literature or in the project, validates the blended rank against an external federal criterion (CEL LES, CU-bill advancement, CU-aligned final-passage votes), chamber-stratified. Fill with a preregistered single look: blend and each single pillar vs criterion, Spearman with bootstrap CI, House and Senate separately.
- No measurement of effective variance shares under the current normalizations. Fill by internal computation (share of score variance by pillar), not literature.
- No sensitivity analysis of top-133 membership across the three candidate weight sets. Fill with Kendall tau-b and overlap counts plus a Christopher-style one-weight proportional-rescale sweep.

**Paul's position vs the evidence.** Consistent. Paul says no annotated work underwrites the weights, and the evidence confirms it: no row estimates or tests pillar weights. His wish to measure effectiveness (Aref & Neal; Sotoudeh vs LES) is supported in principle by Bucchianeri (NC 2005-2012) as a construct-validity argument for using an effectiveness criterion. The evidence adds one thing Paul has not stated: if the brain's roughly +0.10 rho with CEL LES holds (unverified here), the blend sits in bar B's null band. The blend would then rank something other than published legislative effectiveness. That should be stated, not hidden.

**Open questions**
- Which weight set is ratified: 30/25/40/5, 25/30/40/5 or the live 45/10/40/5? (Paul's ruling, S-5.)
- Is the brain's roughly +0.10 rho between the blend and CEL LES reproducible, and on what n and Congress window?
- Should House and Senate be percentile-ranked within chamber before blending?
- What are the effective variance shares of the four pillars under current normalizations?

**Read first:** [[bucchianeri-2025-legislative-effectiveness-american]], [[christopher-2016-mathematical-modeling-analysis]], [[becker-nd-gameability-redistricting-criteria]], [[battaglini-2018-effectiveness-connected-legislators]], [[sotoudeh-2024-network-based-measure]]

## Committee - Influence pillar

**Computes.** Influence% = committee_raw / MAX(committee_raw), where committee_raw = (Membership + Leadership)/2 from seats held.

**Method.** Seat weight by CU-relevance tier: Highly 3.0, Moderately 1.8, Slightly 0.9; subcommittee seats (names containing ' - ') weigh 1/6. Leadership = seat weight x multiplier: Chair 3.0 full / 7.5 sub, Ranking 2.0 / 4.5, Vice 1.0 / 2.0 (Paul applied chair/ranking x1.5 by hand on 2026-06-30). Replaced a double-PERCENTRANK version (CMTEFIX). Centrality was retired from Influence on 2026-06-25 (AB13 = 0) as direction-blind. The formal-office party-elite bonus is display-only (set to 0 at compute_pillars.py:163). Judiciary was added at tier Slightly.

**Code refs.** `fec/engine/committee_scores.py:1-185`, `fec/engine/apply_committee_fix.py:3-18`, `fec/engine/apply_committee_reweight.py:15-37`, `fec/engine/add_judiciary_seats.py:3-27`, `fec/engine/influence_roles.py:11-30`

**Assumptions**
- Committee seat relevance and leadership measure structural CU power
- Subcommittee seats are worth 1/6 of a full seat
- Tier labels and multipliers are owner judgment, not estimated

**Evidence** (12 rows, 10 distinct works; 1 UNVERIFIED, 4 VERIFIED, 7 VERIFIED-SOFTENED)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2020-strategic-sequential-links]] | 2020 | supports | Donation-linked lobbying effects are particularly stronger for the committee-membership channel than for co-sponsorship or sponsorship; the authors read this as firms targeting politicians with legislative power over their interests (committee jurisdiction over the firm's industry). | Only the qualitative point carries: money and lobbying converge on members holding jurisdiction. Nothing about tier weights, the 1/6 factor or the leadership ladder. Kim's 'powerful committee' covariate is Groseclose-Stewart (Appropriations, Armed Services, Finance, Rules and Administration), not Financial Services or Banking. Committee outcome also has a higher base rate than sponsorship (fn 13), so 'stronger' partly tracks channel breadth. Sample is manufacturing firms. | moderate | VERIFIED |
| [[kim-2020-mapping-political-communities]] | 2020 | supports | Legislators who interact with probability at least 40% in the 'Finance & Insurance' community have committee memberships concentrated in relevant committees such as House Financial Services; high-entropy legislators sit on power committees (House Ways and Means, Senate Appropriations), and broad groups such as the CHAMBER OF COMMERCE target them. Window: 113th Congress; lobbying-attention counts, not outcomes. | Supports the premise that lobbying attention is organised by jurisdiction. It does not show seats produce CU outcomes. One Congress, descriptive. The power-committee finding suggests checking how tax-writing and appropriations seats are tiered; not verified here. | moderate | VERIFIED-SOFTENED |
| [[huneeus-2018-effects-firms-lobbying]] | 2018 | suggests-extension | Committee relevance is measured from data. Weight w_ict-k is the firm's lagged share of lobbied bills referred to committee c, with t-1 preferred and t-2 and t-3 in robustness (Appendix F). Interacted with committee seats held by politicians from the firm's headquarters state, the first-stage F is 14.30 (all years) and 13.10 (post-2007) in Table 2. Deeper-lag panels in Table F.1 give F of 6.0 to 9.9. About 30 percent of politicians change committee between Congresses (Section 4.1; Figure 6). | Extension as stated: compute each committee's lagged share of CU-category bills referred, from congress.gov billstatus rather than LDA lobbied-bill lists (D-021). H&K's weights are firm-specific and validated only as an instrument for firm revenue, with no evidence that referral shares predict a member's CU effectiveness, so any re-weighting needs its own look. | moderate | VERIFIED |
| [[porter-2005-network-analysis-committees]] | 2005 | qualifies | In the 107th House the four Intelligence subcommittees each include at least half of the 20-member parent committee and at least one third of each other subcommittee, with normalized interlocks of 14.4 to 26.6 among the five nodes. Single-linkage clustering places subcommittees at the bottom of a roughly four-level hierarchy (subcommittees, committees, groups, House). | The nesting evidence is one parent committee in one Congress, so it cannot support a general statement that subcommittee seats are nested in the parent. The paper calls these overlaps expected and measures membership redundancy, not influence, so it says nothing on whether additive counting is wrong for CU outcomes. The 1/6 factor is a code constant; its origin as owner judgment is not shown in the lines read. | weak | VERIFIED-SOFTENED |
| [[porter-2005-network-analysis-committees]] | 2005 | suggests-extension | Committees differ in mean partisan extremism of their members. Select Homeland Security ranks 4th of 113 committees and above all 19 standing committees. Rules is the second most extreme standing committee (after Judiciary) and 18th of 113. Intelligence and its four subcommittees rank among the 10 least extreme of 113. | Extremism is relative to the 107th House mean, tracks party, and is descriptive with no test. It is evidence of heterogeneity across committees, not a weight to adopt. Any use would mix party loyalty with CU leverage, the chicken-or-egg problem Paul raised. | weak | VERIFIED |
| [[kim-2023-systemic-sequential-links]] | 2023 | supports | Committee-linked lobbying after a donation is the largest channel and grows over time (PAC 0.103 to 0.147 over t+0 to t+3). Hypothesis H3 predicts the committee channel is strongest (pp.6-8). Committee chairmanship enters the model only as a control. | The paper supports only the claim that committee position is where interest-group money and lobbying concentrate. It does not estimate chair, ranking, or subcommittee weights, and it does not estimate committee effects on bill outcomes. Sample is manufacturing firms, federal level, 2008-2018. | weak | VERIFIED |
| [[thakur-2025-matching-theory-perspective]] | 2025 | qualifies | Senate seats are filled by two party mechanisms. Republicans use a seniority-ordered Serial Dictatorship that the paper says seems to be run as TTC/YRMH-IGYT in practice. Democrats use a Boston approximation with COC tie-breaking. Cross-party seniority-at-appointment gaps on some committees are read as possible party discipline: Environment and Public Works R mean 3.80 (N=30) vs D 1.00 (N=14), diff 2.80, t=3.05; Commerce, Science and Transport diff -1.74, t=-2.09; 104th-114th. | Senate rows only (about 100 senators; the 533 total is not from the paper). Paper calls the evidence suggestive (p. 18). Only Environment and Commerce have reported t-stats; Budget and Rules differences rest on SD gaps with no test. Supports only that seat selection differs by party, not the tier weights or any power claim. | moderate | VERIFIED-SOFTENED |
| [[thakur-2025-matching-theory-perspective]] | 2025 | qualifies | Committees differ in common value. Rules, Finance, Governmental Affairs and Appropriations are the most sought after even after excluding the most senior members. In the Republican reduced-form model, Finance and Appropriations have large common value and small idiosyncratic spread, while Constituency committees such as Small Business have lower common value. Sample: 426 assignments, 25 committees, 97 senators, 104th-114th. | Tables A7 and A8 numeric cells are garbled in extraction, so the lower-common-value claim for Small Business is verified only as text, not numerically. Republican Senate 1995-2017 only. Use as a validity caveat, not a re-tiering input. | weak | VERIFIED-SOFTENED |
| [[bucchianeri-2025-legislative-effectiveness-american]] | 2025 | qualifies | Committee chairs score 0.507 above rank-and-file in the pooled full-sample estimate (strata: 0.609 lower chamber, 0.307 upper). The chair gap widens with log chamber size (0.205, p<0.05) and narrows where chambers vote on committee appointments (-0.180, p<0.01). Majority status adds 0.370 (strata 0.362 and 0.387). | The paper's estimates are associational with endogeneity unaddressed (p. 34) and do not estimate multipliers. The chamber-size transfer to a 435-seat House and 100-seat Senate is the analyst's extrapolation, not the paper's claim. The Ranking-to-Chair ratio of two-thirds holds only for full weights; for sub weights it is 4.5 over 7.5, or 0.60. | moderate | VERIFIED-SOFTENED |
| [[anon-nd-house-committee-requests]] |  | suggests-extension | UNVERIFIED and unread: the codebook (0 chars extracted) could not be opened. Paul's annotation names it 'Frisch & Kelly 2007, House Committee Request Data'. From general knowledge only, it records House members' committee-assignment requests next to the assignments they received. No numbers can be quoted. | This applies only if the codebook confirms request-vs-assignment fields. Even then the data are historical (not the 119th Congress) and House-only, and they cover new assignments and transfers rather than continuing seats. They could support a design check: whether high committee scores reflect pre-existing interest (selection). They could not be used as a live input or an edge or weight source. Strength is weak until the document is read and a second path reproduces any result (DE-062). | weak | UNVERIFIED |
| [[battaglini-2018-effectiveness-connected-legislators]] | 2018 | qualifies | Committee-chair coefficient on LES: OLS 1.9669 (SE 0.1147) falls to 1.6107 (SE 0.1545) in the corrected network model, about 18 percent smaller. Chair susceptibility cell in Table 5b is -0.5981 (SE 0.1588). Seniority susceptibility in Table 5a is -0.0339 (SE 0.0123). House, 109th-113th. | The paper uses LES as the outcome, while the Influence pillar is a prestige proxy, so the 18 percent figure is indicative only. House only. | weak | VERIFIED-SOFTENED |
| [[defigueiredo-2013-advancing-empirical-research]] | 2013 | supports | De Figueiredo and Silverman (2006), academic earmarks 1997-1999: lobbying raises earmarks when the district legislator sits on an appropriations committee and has no statistically significant independent effect when not (pp.22-24). Powerful, allied and marginal legislators are the ones lobbied (pp.10-11). | The evidence is from academic earmarks on appropriations and supports the conditioning logic only. It does not validate the tier weights or the 1/6 subcommittee ratio, which remain owner judgment. | weak | VERIFIED-SOFTENED |

**Assessment**

**Committee / Influence pillar: the most-evidenced pillar by rows. 12 rows, 10 distinct works; 1 UNVERIFIED, 4 VERIFIED, 7 VERIFIED-SOFTENED.** The evidence supports the premise and leaves every number untested.

**Supports the premise: money and lobbying concentrate on jurisdiction**
- **Kim et al. 2020 and 2023, best read as one line of evidence.** These are two versions of the Kim, Stuckatz and Wolters donation-lobbying work, so they are not independent confirmation. In both, the committee channel is the largest and grows over time (PAC 0.103 to 0.147 over t+0 to t+3; manufacturing firms, federal, 2008-2018). Caveat: the committee outcome has a higher base rate, so "stronger" partly tracks how broad the channel is.
- **de Figueiredo 2013.** Lobbying raises earmarks only when the district legislator sits on Appropriations (1997-1999).
- **Kim & Kunisky 2020.** Finance-community legislators concentrate on Financial Services (113th Congress, descriptive). This is LDA-derived, so it is reading only under D-021.

**The best federal outcome evidence: Battaglini et al. 2018**
The committee-chair premium on LES is 1.97 (OLS), falling to 1.61 after the network endogeneity correction, about 18% smaller (House, 109th-113th, N = 2,176, pooled, UNSTRATIFIED by party). A chair premium is therefore real at the federal level. Whether a 3.0 full / 7.5 sub multiplier is the right size is untested: the Influence pillar is a prestige proxy and LES is a different outcome.

**Qualifiers**
- **Bucchianeri 2025 (NC 2005-2012).**
  - The chair premium differs by chamber: pooled 0.507, strata 0.609 lower and 0.307 upper.
  - It narrows where chambers vote on appointments.
  - Majority status adds 0.370 (strata 0.362 / 0.387).
  - The system uses identical multipliers in both chambers, pools them under one raw/max, and has no majority term.
- **Thakur 2025** (Senate Republicans, 104th-114th, 426 assignments). Small Business has low common value while Finance and Appropriations have high value. The system tiers 74 of 74 Small Business seats as Highly. Treat this as a validity caveat, not a re-tiering input: the paper's tables were garbled in extraction.
- **Porter et al. 2005.** Subcommittees are heavily nested in their parents (Intelligence, 107th), so additive seat counting may double-count. This rests on one committee in one Congress and is weak.
- **Huneeus & Kim 2018.** Offers a data-driven alternative to the tier map: each committee's lagged share of referred bills. For CU bills that share must come from congress.gov billstatus, not LDA (D-021). It is validated only as an instrument for firm revenue.

**Lead, not a finding**
The Frisch & Kelly House committee-request codebook is UNVERIFIED and unread. If it confirms request-vs-assignment fields, it could support a selection check on whether high scores reflect prior interest rather than seat leverage.

**Contradictions**
- Bucchianeri 2025 reports a chamber-dependent chair premium (pooled 0.507; strata 0.609 lower vs 0.307 upper, NC 2005-2012). The system applies identical Chair/Ranking/Vice multipliers in House and Senate and normalizes both under one raw/max.
- Thakur 2025 finds Small Business is a low-common-value committee (Senate Republicans, 104th-114th). The system tiers all 74 Small Business seat tokens as Highly. Weak: Senate-only, tables garbled, and CU relevance is a different axis from prestige.
- Majority status is a large effectiveness covariate in both the NC data (+0.370, strata 0.362/0.387) and House passage data (Box-Steffensmeier, majority-sponsor logit 1.353, UNSTRATIFIED). The committee score has no majority term.

**Gaps**
- No study estimates the tier weights, the 1/6 subcommittee factor or the leadership ladder. Fill: compare the tier map against a billstatus referral-share map (Huneeus-style, lagged share of CU-category bills referred to each committee) on CU-bill markup and advancement for bills referred to that committee, chamber-stratified, as a single preregistered look.
- No evidence on the 245/533 members with no committee row. Fill: compare their CU outcomes with low-score members, by chamber and majority status.
- Selection vs leverage (Paul's chicken-or-egg point) is untested. Fill: read the Frisch & Kelly codebook first; if usable, a historical House check of whether request patterns predict committee score beyond the seat itself.
- How tax-writing and appropriations seats (Ways and Means, Finance, Appropriations) are tiered was not verified. CU Tax Status is a top press category, so their tier matters.

**Paul's position vs the evidence.** Largely consistent. Paul says committees are the structural layer (Porter) and that money and lobbying effects are strongest in committee activity (Kim et al.; Huneeus & Kim). The verified rows bear this out qualitatively, though Kim 2020 and 2023 are one study line, not two. His chicken-or-egg flag is well founded: Porter's extremism heterogeneity and Thakur's party-specific seat selection both show that seats are not randomly assigned, and no row resolves it. The Frisch & Kelly lead he annotated is the right kind of data but is unread. No evidence row speaks to his Gaynor leadership-insulation point. Where the evidence goes beyond his position: chamber pooling and the missing majority term are concrete, testable weaknesses he has not raised.

**Open questions**
- Should Chair/Ranking multipliers differ by chamber, given the NC strata (0.609 lower vs 0.307 upper)? Paul's call after a federal check.
- Does a billstatus referral-share weighting outperform the owner tier map on CU-bill advancement?
- Are Small Business seats correctly Highly for CU relevance even though Senate demand for them is low? Relevance and prestige may simply be different axes.
- Does additive counting of nested subcommittee seats over-reward members with many subcommittee seats on one parent?
- What does the Frisch & Kelly codebook actually contain?

**Read first:** [[battaglini-2018-effectiveness-connected-legislators]], [[bucchianeri-2025-legislative-effectiveness-american]], [[kim-2020-strategic-sequential-links]], [[huneeus-2018-effects-firms-lobbying]], [[thakur-2025-matching-theory-perspective]], [[defigueiredo-2013-advancing-empirical-research]]

## Legislative pillar (stance-gated agenda plus efficiency)

**Computes.** Leg% = percentile of a blend of agenda% and efficiency% (0.7/0.3 at 2026-06-25; live blend read from workbook AB10/AB11). Agenda = sum over topics of IMP[topic] x activity x stanceMult, so opponents go negative.

**Method.** Activity weights by role: sponsor 0.286, original cosponsor 0.143, cosponsor 0.0714 (letters count at original-cosponsor weight). stanceMult is signed from category_stance.json. Efficiency per category = decay-weighted sign of advanced sponsored or original-cosponsored bills over total decay weight; advancement is tri-state and unknown outcomes are excluded. The pillar stays PERCENTRANK because raw/max (tried 2026-07-01) squashed the mid-board. The live routing implementation is fec/engine/live_legislative_pillar.py.

**Code refs.** `fec/apply_leg_stance.py`, `fec/apply_legstance_undo.py`, `fec/engine/live_legislative_pillar.py:17-124`

**Assumptions**
- Sponsorship and cosponsorship in a CU topic reveal the direction of advocacy once signed by stance
- Advancement past introduction measures legislative efficacy
- category_stance.json is current when the step runs (known staleness trap)

**Evidence** (4 rows, 4 distinct works; 2 VERIFIED, 2 VERIFIED-SOFTENED)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[fouirnaies-2022-electoral-incentives-affect]] | 2022 | qualifies | Bill sponsorship volume falls about 6% when reelection incentives end (-1.63, SE 0.77, mean 27.32; N=16,727) with no ideological change. Sponsorship volume partly reflects electoral advertising (legislators' webpages list bills introduced) rather than position. | Weak. The 6% effect at the state level is small, and in Congress it would touch only members in their final Congress or those running for other office. The paper supports the influence-vs-alignment split. It does not address the 4:2:1 role weights or cosponsorship. | weak | VERIFIED-SOFTENED |
| [[sotoudeh-2024-network-based-measure]] | 2024 | supports | A decayed pass/total success ratio is informative about outcomes. Passed bills carry higher cosponsor pass-ratio influence than failed bills in every 4-month window (relative difference 0.139 mean / 0.248 max at a 6-month half-life). The authors contrast their approach with LES, which has no decay and splits the process into finer stages. | Indirect support only. The paper's ratio is pairwise, aggregated to bills, and includes each bill's own outcome. The pillar's ratio is member-level and describes CU-topic advancement; the paper does not show that a member's success rate measures CU influence or predicts future behaviour. The paper pools across parties and 2009-2019 with no strata, so label UNSTRATIFIED. The authors report a decline from about 2015 and hypothesize it is correlated with polarization. | weak | VERIFIED |
| [[boxsteffensmeier-2018-taking-congress-interest]] | 2018 | qualifies | Passed House logit (Table 4, N = 6,597): hot bill 2.428 (SE .161), commemorative 1.386 (SE .150), majority-party sponsor 1.353 (SE .220), endorser count 0.013 (SE .004), cosponsors 0.004 (SE .001). Endorser social power is -0.035 (SE .032, ns) for passage and 0.020 (SE .031, ns) for reporting. | The paper estimates a covariate association over letter-bearing House bills pooled across 106th-111th Congresses. It is not a per-member efficiency model and says nothing about efficiency metrics. Reading the majority coefficient as showing that a raw advancement rate rewards majority status is an inference, not the paper's claim, and any adjustment is Paul's call. | moderate | VERIFIED-SOFTENED |
| [[bucchianeri-2025-legislative-effectiveness-american]] | 2025 | qualifies | In North Carolina 2005-2012 SLES outperforms hit-rate metrics against subjective rankings. Scores are relative to each chamber-term and should not be compared directly across chambers or eras. Majority status adds 0.370 in the pooled full-sample estimate (strata: 0.362 lower chamber, 0.387 upper), 58 percent above the minority mean of 0.636. | The NC criterion covers one state chamber pair over eight years; transfer to Congress is an assumption. The 0.370 figure is an unstratified pooled estimate and must carry strata or an UNSTRATIFIED label, which the rel does. | moderate | VERIFIED |

**Assessment**

**Legislative pillar: 4 rows, 4 distinct works; 2 VERIFIED, 2 VERIFIED-SOFTENED. The evidence qualifies the pillar; none validates it.**

**Efficiency term**
- **Sotoudeh et al. 2024 (VERIFIED, weak).** A decayed pass/total ratio is informative about bill outcomes: relative difference 0.139 mean / 0.248 max at a 6-month half-life (House 2009-2019). The figure is pooled across parties and years, so label it UNSTRATIFIED. The authors note a decline from about 2015 that they tie to polarization. Their ratio is pairwise and bill-level and includes each bill's own outcome. It does not show that a member's CU-topic success rate measures CU influence.
- **Bucchianeri 2025 (VERIFIED, moderate) cuts the other way.** In NC (2005-2012), stage-based scores outperform hit-rate metrics against expert rankings, and the efficiency term is a hit-rate-style metric. Bucchianeri also shows majority status adds 0.370 (strata 0.362 / 0.387), and Box-Steffensmeier 2018 shows a majority-sponsor logit of 1.353 on House passage (106th-111th, N = 6,597 letter-bearing bills, UNSTRATIFIED). The efficiency block has no majority term, so it plausibly partly measures majority membership. That is an analyst inference.
- **Chamber pooling.** House and Senate share one percentile, against Bucchianeri's chamber-term caution.

**Agenda term**
- **Fouirnaies & Hall 2022 (weak).** Sponsorship volume falls about 6% when reelection incentives end, with no ideological change (US state legislatures, N = 16,727). Volume partly reflects electoral advertising. This qualifies the unnormalized agenda sum for members in their final Congress, and it supports keeping volume and direction separate, which the stance sign does.

**Not addressed by any row**
- The 4:2:1 sponsor / original / cosponsor ratio.
- Treating letters at original-cosponsor weight.
- The 0.7/0.3 agenda/efficiency blend.
- Whether stance-signed activity predicts CU-aligned votes better than unsigned activity.

**Contradictions**
- Bucchianeri 2025 (NC 2005-2012): hit-rate metrics underperform stage-based effectiveness against expert rankings. The efficiency term is a decay-weighted hit rate.
- Bucchianeri says scores are chamber-term relative; the pillar percentile-ranks House and Senate members together.
- Majority status is a large predictor of advancement (NC +0.370, strata 0.362/0.387; House passage logit 1.353, UNSTRATIFIED). The efficiency term has no majority adjustment, so part of it may index party control rather than skill.

**Gaps**
- No study estimates role weights (sponsor vs original vs cosponsor) by effort or effect. Fill: compare the advancement association of bills by the count of sponsors vs original cosponsors vs later cosponsors, or survey staff time per role.
- No test that stance-signed agenda predicts CU-aligned final-passage votes better than unsigned agenda. Fill: preregistered comparison on the 119th, chamber-stratified.
- No test that efficiency adds validity beyond agenda volume. Fill: incremental Spearman or partial correlation against CEL LES and CU votes, controlling majority status.
- Paul's Ramey (conscientiousness and sponsorship) citation has no evidence row in this set.

**Paul's position vs the evidence.** Partly consistent. Paul favours observable effort (sponsorship, committee work) as an incentive-sensitive signal. Fouirnaies & Hall support the incentive sensitivity (about -6% at the state level), but that cuts both ways: volume is partly electoral advertising, not just effort. Sotoudeh supports a decayed success ratio as informative at the bill level. Bucchianeri warns that hit-rate metrics underperform stage-based scores and that majority status dominates. Paul's note that no annotation addresses the 4:2:1 weights is confirmed. No row on Ramey was produced, so his conscientiousness link is uncorroborated in this set.

**Open questions**
- Should the efficiency term be majority-adjusted or computed within majority/minority strata? Paul's call.
- Should Leg% be percentile-ranked within chamber?
- Would a stage-based (LES-style) progression score outperform the binary advanced/not-advanced hit rate?
- Does the final-Congress volume drop (Fouirnaies) materially move any top-133 member?

**Read first:** [[bucchianeri-2025-legislative-effectiveness-american]], [[sotoudeh-2024-network-based-measure]], [[boxsteffensmeier-2018-taking-congress-interest]], [[fouirnaies-2022-electoral-incentives-affect]]

## Press pillar (own-domain, signed raw-max)

**Computes.** Press% = signed press_value / MAX, where press_value = sum over categories of (stance sign x LSD tone intensity) x tiered category weight, for the member's own-domain statements only.

**Method.** Only *.senate.gov and *.house.gov rows count; 819 third-party rows were dropped, 816 of them americascreditunions.org. 1,310 synthetic billstatus rows were de-duplicated out. Category weights tiered 2026-07-02: Interchange, CU Tax Status, Credit-Card Mandates 0.30; Charter/FOM 0.15; Exam/NCUA 0.10; medium 0.08; low 0.02-0.05. Speech channel normalized at 1/n per event (a3174bc). Press is the only directional signal in the original design. Display-only overrides do not feed the pillar.

**Code refs.** `fec/apply_press_dedup.py`, `fec/apply_raw_max_norm.py`, `fec/moneyball_core.py:214-241`, `fec/engine/pillar_policy.py:2-55`

**Assumptions**
- A member's own-domain statements represent the member's stance; third-party mentions do not
- Stance labels are the owner's domain (never relabelled)
- Tone intensity x stance sign is a valid single press measure

**Evidence** (3 rows, 3 distinct works; 1 UNVERIFIED, 2 VERIFIED-SOFTENED)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[haselmayer-2017-sentiment-analysis-political]] | 2017 | qualifies | Dictionary-based tonality runs in the same direction as expert coding but compresses effects. In App. 3 Table 6 the government-pair coefficient is -0.13 with the dictionary vs -0.40 with expert coding, and electoral losses are 0.12 vs 0.23. Authors call dictionary estimates more conservative than manual coding. Pooled regression with party fixed effects, standard errors clustered by party pair; adjusted R2 0.03 with n = 619 (Table 3 and Table 6 dictionary columns). | The evidence is one Austrian campaign, in German, on negativity only. It suggests dictionary intensity carries a weaker, noisier version of the human-coded signal, so multiplying stance sign by that intensity may add noise. The paper does not test a stance x intensity product, so this is a caution, not a refutation. The paper's pooled coefficients should be labelled as pooled across parties with party fixed effects if quoted. | weak | VERIFIED-SOFTENED |
| [[vanderdoes-2022-strategic-identity-signaling]] | 2022 | qualifies | As cross-partisan outgroup share rose from 1 to 9 of 10, participants shared fewer tweets overall (AME -5.38, 95 percent CI -6.04 to -4.72) and shifted toward covert ones (AME 0.73). Covert tweets drew fewer outgroup dislikes but also fewer ingroup likes, so the message is lower-affect for the ingroup. | Requires that legislators in politically mixed districts or states reduce or code their CU statements the way the experiment participants did. The experiment paid at most 20 cents of cost and fixed audiences with no feedback, which is far from electoral stakes. Test by correlating own-domain press volume and LSD charge with district lean before acting; any pooled correlation across chambers is quoted with chamber strata or labelled UNSTRATIFIED. | weak | UNVERIFIED |
| [[osnabrugge-2021-playing-gallery-emotive]] | 2021 | qualifies | Hypothesis H (p. 4): legislators use more emotive rhetoric in high-profile debates with a larger general audience. The theory section adds that overt emotion can signal weak credibility, so speakers balance ethos, logos and pathos by audience. The paper studies parliamentary speech only, not press releases. | Holds only to the extent that between-member intensity differences reflect style rather than commitment. The paper shows within-speaker venue shifts in parliament, not between-member style differences within one venue. | weak | VERIFIED-SOFTENED |

**Assessment**

**Press pillar: thin evidence. 3 rows, 3 distinct works; 1 UNVERIFIED, 2 VERIFIED-SOFTENED.** None is a US congressional study, and none tests the pillar's core claims.

**What the rows say about the intensity term (stance sign x LSD tone)**
- **Haselmayer & Jenny 2017.** Dictionary tonality points the same way as expert coding but compresses effects: government-pair coefficient -0.13 vs -0.40; electoral losses 0.12 vs 0.23. Adjusted R2 is 0.03 (n = 619, one Austrian German-language campaign, negativity only; pooled across parties with party fixed effects). If this transfers to LSD on English press releases, multiplying the stance sign by dictionary intensity adds a weaker, noisier copy of the signal. It is a caution, not a refutation: the paper never tests a sign x intensity product.
- **Osnabrugge et al. 2021.** Emotive rhetoric rises with audience size (parliamentary debate, within-speaker venue shifts). Between-member intensity differences in press releases may index messaging style rather than commitment. That is analyst inference; the paper does not study press releases or between-member style.

**Lead only**
- **van der Does 2022 (UNVERIFIED).** In an experiment, participants facing a mixed audience posted less and more covertly. If legislators in mixed districts hedge CU statements the same way, Press% would score a hedger like a lukewarm member. Stakes in the experiment were trivial (at most 20 cents), so this is a hypothesis to test against district lean, not a finding.

**Untouched by any row**
- Own-domain filtering (dropping 819 third-party rows, 816 of them from americascreditunions.org).
- The 1/n per-event speech normalization.
- The tiered category weights.
- Whether press stance predicts later CU cosponsorship or votes.

The 1/n per-event normalization is member-level, not per-edge SNA normalization, so the rejected per-edge proposal does not bear on it. Stance labels remain Paul's domain; nothing here argues for relabelling.

**Contradictions**
- (none)

**Gaps**
- No study tests whether own-domain press stance predicts subsequent CU-aligned cosponsorship or votes. Fill: preregistered single look, press stance at t vs CU cosponsorship and votes at t+1, chamber-stratified.
- No test of sign-only vs sign x LSD intensity. Fill: compare both versions against the same criterion; Haselmayer predicts intensity adds noise.
- No evidence on own-domain filtering. Fill: compare criterion validity with and without the third-party rows (the 816 americascreditunions.org rows are lobby-authored, so they are the obvious contamination case).
- No evidence on 1/n per-event normalization. Fill: test whether high-volume speakers are proportionally more aligned on the criterion.
- No US congressional press-release sentiment validation in the set. Fill: hand-code a stratified sample of own-domain CU releases and measure LSD agreement.

**Paul's position vs the evidence.** Paul wants press, news-cycle and social sentiment added as determinants of pathway change (the Bacik augmentation), and treats public statements as the observable persona. None of his press citations (Bacik et al. 2026; Birkenmaier & Lechner 2025; Ramey et al.; Merkley & Stecula) produced an evidence row here, so his position is uncorroborated in this set. The rows that do exist push toward caution, not expansion. Dictionary sentiment compresses the human-coded signal (Haselmayer), and intensity tracks audience and venue (Osnabrugge), so more sentiment channels would add style-driven noise unless validated against behaviour. His note that nothing he annotated addresses own-domain filtering is confirmed.

**Open questions**
- Does sign-only press stance validate as well as or better than sign x LSD intensity against CU votes?
- Does press volume or LSD charge correlate with district lean (van der Does hedging), with chamber strata?
- Should the press weight (now 0.10) wait for a validation look before any sentiment channels are added?

**Read first:** [[haselmayer-2017-sentiment-analysis-political]], [[osnabrugge-2021-playing-gallery-emotive]], [[vanderdoes-2022-strategic-identity-signaling]]

## Demographic pillar input (Demo%)

**Computes.** Demo% = PERCENTRANK of Blended Model column F (5% weight).

**Method.** Column F is formula-backed and edited by hand; no Python producer writes it. Sub Layer V:AF holds hard-coded values. The DEMO_POTENTIAL dial does not move this input. A 06-21 bias audit found a mild Democratic tilt (bucket mean D 0.52 vs R 0.34), accepted at 5% weight.

**Code refs.** `Moneyball Main Version - Blended.xlsx Blended Model col F; Sub Layer V:AF`

**Assumptions**
- District demographics carry a small legitimate influence signal
- Demographic Potential is a market score, not a support prediction

**Evidence** (no rows)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment**

**Demographic pillar: GAP, no rows.**

Nothing in the set speaks to whether district demographics carry a legitimate influence signal, to the 5% weight, or to the input itself. Column F is hand-edited, has no Python producer, and the Sub Layer V:AF values are hard-coded.

**Bias audit.** The 06-21 audit found a mild Democratic tilt: bucket mean D 0.52 vs R 0.34, over the 533-member roster. It was accepted at 5% weight. The audit is internal, and no literature tests whether that tilt is signal or partisan confound.

**Paul's state-layer idea.** Paul's linked citation (Huneeus & Kim 2018) appears in this set only as committee-pillar evidence. There, headquarters-state x committee-seat interactions instrument firm revenue. That points, as analyst inference, to geography mattering through an interaction with jurisdiction rather than as an additive pillar. No row tests it.

**Contradictions**
- (none)

**Gaps**
- No study tests whether Demo% correlates with CU-aligned behaviour after controlling for party. Fill: partial correlation (or ordinal regression shown as summary output) of Demo% against CU-aligned final-passage votes and cosponsorship with party and chamber controls, reported with strata.
- No comparison of column F with Demographic Potential v3.0. Fill: rank-delta and Kendall tau-b of the top 133 under each input. This is an internal computation.
- No evidence on whether district CU presence works additively or through committee jurisdiction (the Huneeus & Kim interaction logic). Fill: test Demo x CU-relevant committee seat against CU-bill outcomes.
- Provenance gap: no reproducible producer for column F. Fill by engineering, not literature.

**Paul's position vs the evidence.** Paul proposed a state layer showing whom to target by general industry presence (via Huneeus & Kim). It became Demographic Potential, which does not feed Demo%. The evidence set has nothing on Demo% itself, so his position is neither supported nor contradicted. The one related row uses Huneeus & Kim's geography only as an interaction with committee seats, which differs from the additive 5% pillar.

**Open questions**
- Should Demo% be replaced by Demographic Potential v3.0, left as is, or dropped pending validation? Paul's call.
- Does the D 0.52 vs R 0.34 tilt survive a party control, or is the pillar a party proxy at 5% weight?

**Read first:** [[huneeus-2018-effects-firms-lobbying]]

## Sub Layer legislative-efficiency formulas and off-by-one repair

**Computes.** Per-member legislative efficiency from Sub Layer - Raw AG:AW, normalized by Table1 min-max.

**Method.** A 2026-07-15 row shift made every member use the neighbour's record. Repair c6dc6b2 (2026-07-17) repointed 9,061 formulas, fixed AH (Q->P) and AI (AI->X), and anchored on each formula's own Table1 basis column. Independent recompute matches 533/533; 520/533 ranks moved, all legislative-only.

**Code refs.** `fec/repair_sublayer_references.py (--plan/--apply)`

**Assumptions**
- Each formula's Table1 basis names the intended column (the majority pattern was the bug)
- A stale pillar snapshot can shield downstream consumers from an unrepaired reference plane

**Evidence** (no rows)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment**

**Sub Layer efficiency repair: GAP by nature, no rows.**

This is an engineering repair, not a modelling claim, and no literature applies. Repair c6dc6b2 (2026-07-17) repointed 9,061 formulas. An independent recompute matched 533/533, and 520/533 ranks moved, all legislative-only.

The open question is reproducibility from raw congress.gov billstatus data in an independent pipeline. Papers cannot answer that; only a second-path reproduction can (DE-062 logic: one path is a lead, two agreeing paths are a finding).

The assumption that each formula's Table1 basis names the intended column is plausible. It rests on the majority pattern having been the bug, so it should be confirmed against raw data, not reasoning.

**Contradictions**
- (none)

**Gaps**
- No independent reproduction of post-repair efficiency values from raw congress.gov billstatus. Fill: a second pipeline that recomputes per-member efficiency from billstatus and compares member by member (exact match count over 533, max absolute deviation).
- No check that the live Python efficiency (live_legislative_pillar.py) and the Sub Layer AG:AW values agree. Fill: cross-pipeline diff.

**Paul's position vs the evidence.** Paul's position (no literature; engineering repair) is correct, and the evidence set confirms it: no rows.

**Open questions**
- Does the Sub Layer still feed anything live, or is live_legislative_pillar.py now the sole authority for efficiency?
- Do post-repair values reproduce from billstatus in an independent path?

**Read first:** (none)

## Cosponsor-network InfluenceScore (computed, weight 0)

**Computes.** InfluenceScore = 0.60 x weighted out-degree + 0.25 x betweenness + 0.15 x eigenvector, joined to 494/533 members; the 39 absent fall back to committee%.

**Method.** Computed from the congress_centrality output. It was retired from Influence on 2026-06-25 (AB13 = 0) because it is direction-blind. It still feeds the underdog lens.

**Code refs.** `congress_centrality_*.xlsx; Blended Model AB12/AB13`

**Assumptions**
- Cosponsor centrality reflects influence regardless of stance direction

**Evidence** (10 rows, 10 distinct works; 2 VERIFIED, 8 VERIFIED-SOFTENED)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | supports | Network centrality is a proxy for embeddedness, not a direct measure of influence. Company hypercoreness is predicted by lobbying FTE (beta 0.464) and assets (0.253), R2 = 0.398, N = 1,178, with reverse causation acknowledged. | The regression covers EC company nodes (N = 1,178 in H, 702 in Hstak), not legislators, so it supports the principle that centrality tracks resources and activity rather than influence, not any US estimate. It agrees with the project finding that the raw all-bills Katz ranking measured activity (Spearman with tie volume +0.845, STATE.md:886-887). | moderate | VERIFIED-SOFTENED |
| [[kumar-2022-influence-maximization-social]] | 2022 | qualifies | SGNN features come from struc2vec structural identity, and labels come from SIR and IC diffusion simulations. The regressor therefore predicts diffusion-based influence on structure, not purely structural spreading power. | Applies only if someone proposes learned structural embeddings as an influence signal. Any such signal would need stance gating and would remain a heuristic model output, never closeness or access (D-025). | weak | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | qualifies | Prominence is measured by degree (lines per actor) and by closeness (lowest sum of shortest-path distances). Brokerage is measured by betweenness. In Figure 3, after three arms are removed, actors B and C each keep degree 4 and actor A drops to degree 2, yet A still sits between the peripheral groups and keeps an intermediary role. | A taxonomy of measures, with no evidence on which predicts legislative influence. It does not bear on the stance-sign retirement. Whether centrality predicts passage belongs to Sotoudeh 2024 and the system data. | weak | VERIFIED |
| [[sotoudeh-2024-network-based-measure]] | 2024 | supports | Eigenvector centrality on the decayed pass/total matrix (largest connected component) barely separates passed from failed bills: relative difference 0.004 (mean) and 0.002 (max), against 0.139 and 0.248 for the influence score. Closeness (0.004 / 0.002) and strength (0.004 / 0.003) are equally weak. The authors suspect network density but do not test it. | This backs the retirement only loosely. The paper's centralities use a different matrix (ratio, undirected, House 2009-2019) and it never tests betweenness or weighted out-degree. Answering the component's open question on the system graph would be circular: weighted out-degree sums ties on the member's own sponsored bills, which are already multiplied by 3 when the bill passed. A passage test needs an as-of network built only from bills resolved before the outcome window. | moderate | VERIFIED-SOFTENED |
| [[battaglini-2016-influencing-connected-legislators]] | 2016 | supports | Standard centralities do not explain PAC money (Table 4): committee degree 0.0004 (SE 0.0003), alumni degree -0.0018 (SE 0.0053), closeness 0.5902 (SE 0.4711), betweenness committee -4.9e-05 (SE 2.9e-05, significant at 10 percent only). R-squared is 0.148 in most Table 4 columns (0.149 in one). Likelihood-ratio tests reject phi = 0 for the Bonacich-type SAR term (11.127 committee, 9.615 alumni). | The paper tests unweighted degree, closeness and betweenness on PAC money in House committee and alumni networks, 2004-2012. It does not test weighted out-degree on a cosponsorship graph, and it does not test legislative influence or bill passage. It supports only the narrow point that direction-blind degree and betweenness do not predict money; it is neutral on whether they measure influence. | weak | VERIFIED-SOFTENED |
| [[trivedi-2020-efficient-influence-maximization]] | 2020 | context | The paper says selecting maximum-degree vertices gives higher simulated influence than other centrality measures, and that degree-discount and greedy beat plain degree. Katz equals degree at small k and falls as k grows. The paper does not test betweenness, eigenvector or any weighted composite. | The paper measures simulated cascade spread on co-authorship graphs, not legislative influence or bill passage. It does not test the composite, so it cannot support or contradict the retirement or the underdog lens. Degree beating other centralities is a simulated ordering, not evidence about influence on outcomes. | weak | VERIFIED-SOFTENED |
| [[christopher-2016-mathematical-modeling-analysis]] | 2016 | qualifies | Expert-weighted centralities: out-degree 0.50199, total degree 0.14517, eigenvector 0.11045, betweenness 0.07489 (CR 0.062). TOPSIS on this composite matches the plain out-degree order only for the top two of eight. Corner Table LLC (out-degree 7) ranks 3rd and TC4 Trust (out-degree 38) ranks 7th. The authors state that no significant changes resulted, and they validate the composite against no outcome. | Both composites use judgment weights led by out-degree, so the structural comparison holds. The paper adds a consistency check and a sensitivity sweep but no outcome test, so it cannot justify the 0.60/0.25/0.15 weights or reverse the retirement. Its ties are dollar flows among nonprofits, not cosponsorship. The outcome test that matters is Sotoudeh 2024: does centrality predict bill passage. | weak | VERIFIED |
| [[ringe-nd-pinpointing-powerful-voting]] |  | qualifies | In a three-voter single-party legislature the leader's centrality is at least each follower's, with the condition reducing to q2 below 1 and q3 below 1; the result is extended to n voters by a term-by-term argument. In simulation, absolute-value centrality correlates highly with closeness and weighted degree (no coefficients reported) and about 0.25 with betweenness. | The proof is in signed phi terms (leader-follower correlation at least follower-follower), while simulations use the absolute-value measure; the appendix does not show the inequality holds for that version. The algebra is omitted (line 344). The result concerns co-voting correlations, not cosponsorship weights. The paper's 0.25 is a simulated correlation with betweenness, not a weight, so it says nothing about InfluenceScore's 0.25 betweenness weight. Any revived centrality rank is a heuristic model output, never a closeness, access or influence probability (D-025). | weak | VERIFIED-SOFTENED |
| [[kempe-2015-maximizing-spread-influence]] | 2015 | context | Under LT, greedy beats high-degree seeding by about 18 percent and distance-centrality seeding by over 40 percent (Sec 8.4, p.134). The base of these percentages is not stated, so high-degree is not 18 percent below greedy; it is about 15 percent below greedy if 18 is measured against greedy. Under IC with p=10%, random seeds beat both centrality heuristics beyond about 12 targets (p.135). | Paper evidence comes from one physics co-authorship graph, window winter 2002, denominator 10748 researchers, k = 1 to 30. Under D-025 centrality scores are heuristic outputs, not influence or access probabilities. | weak | VERIFIED-SOFTENED |
| [[battaglini-2018-effectiveness-connected-legislators]] | 2018 | supports | Naive OLS: betweenness 0.0079 (SE 0.0007), closeness 0.0011 (SE 0.0001) and eigenvector 0.1203 (SE 0.0073), all significant; coefficients are on centralities scaled by 100, 10,000 and 10,000 per the Table 4 note. After the endogeneity correction none is significant: betweenness -0.2599 (SE 0.1662), closeness 0.0085 (SE 0.0086), eigenvector -0.4122 (SE 0.2500). Weighted out-degree is not tested. House, 109th-113th, N = 2,176. | The paper tests these centralities against LES, not against CU-relevant influence. The 2026-06-25 retirement rested on edge direction, which the paper does not address, so this is independent corroboration of the null only, not the original reason. Weighted out-degree is untested here. | moderate | VERIFIED-SOFTENED |

**Assessment**

**Cosponsor-network InfluenceScore: the best-evidenced component on outcomes. 10 rows, 10 distinct works; 2 VERIFIED, 8 VERIFIED-SOFTENED.** The evidence supports retiring it from Influence (AB13 = 0), though for a different reason than the one recorded.

**Outcome evidence**
- **Battaglini et al. 2018 (moderate; the key paper).** Naive OLS shows betweenness, closeness and eigenvector all significant against LES. After correcting for endogenous tie formation none is: betweenness -0.26 (SE 0.17); eigenvector -0.41 (SE 0.25). House, 109th-113th, N = 2,176, pooled, UNSTRATIFIED by party. Only the model-weighted Katz-Bonacich term carries signal (cf. the system's kb_benchmark.py).
- **Sotoudeh et al. 2024.** Eigenvector, closeness and strength barely separate passed from failed bills: relative difference 0.004 mean vs 0.139 for their influence score (House 2009-2019, UNSTRATIFIED).
- **Battaglini et al. 2016.** Standard centralities do not explain PAC money (House 2004-2012). This is a different outcome.
- **Azaiez 2025.** Centrality tracks resources and embeddedness rather than influence (EC companies, N = 1,178). This agrees with the project's own finding that the all-bills Katz ranking measured activity: Spearman +0.845 with tie volume.

**Context only**
Kempe 2015, Trivedi 2020 and Kumar 2022 concern simulated diffusion on co-authorship or social graphs. Haythornthwaite 1996 is a taxonomy. Ringe concerns co-voting, not cosponsorship. Christopher 2016 uses hand-weighted composites like the system's 0.60/0.25/0.15, with no outcome test, so it cannot justify those weights.

**The untested 60%.** Weighted out-degree, 60% of the score, appears in no outcome test anywhere in the set. Testing it on the system graph would be circular: out-degree sums ties on the member's own sponsored bills, which are already weighted x3 when the bill passed. A valid test needs an as-of network built only from bills resolved before the outcome window.

**Reason mismatch.** The recorded retirement reason is direction-blindness. The evidence supports a different reason: a null after endogeneity correction. It is independent corroboration, not the original rationale.

**Verification.** Agents could not locate AB13 or the lens feed under fec/. Both live in the workbook only.

Per D-025, any revived centrality rank is a heuristic model output, never closeness, access or influence probability.

**Contradictions**
- The evidence supports retirement on endogeneity and null-outcome grounds (Battaglini 2018; Sotoudeh 2024), not the recorded direction-blindness reason. Same conclusion, different basis.
- The same centrality retired from Influence still feeds the underdog lens as half of 'Intrinsic' influence. The evidence that justifies retirement equally undercuts that use.

**Gaps**
- Weighted out-degree (60% of InfluenceScore) is tested against no outcome in any paper. Fill: build an as-of cosponsor network from bills resolved before the outcome window, then test out-degree against subsequent passage of sponsored bills beyond committee position (the component's own question), with a tie-formation endogeneity control in the Battaglini style, chamber-stratified.
- No test of a signed or stance-gated centrality. Fill: compare a stance-gated variant with the unsigned one on CU-bill outcomes. Paul's directionality interest (Haythornthwaite) and the Viral Centrality idea (Fink) both need such a test.
- Code location of the AB13 retirement and the lens feed is unverified. Fill: open the workbook cells and record them.

**Paul's position vs the evidence.** Consistent, and better supported than Paul states. He cites Sotoudeh's highlight that eigenvector is 'not a strong indicator of bill passage', which the verified row confirms (relative difference 0.004 vs 0.139, House 2009-2019, UNSTRATIFIED). Battaglini 2018, which he does not cite in this position, gives the stronger federal result: no centrality survives endogeneity correction against LES. His call for more research on forms of centrality (Ardekani) and his wish to test Viral Centrality (Fink) are reasonable but have no evidence rows. His directionality interest (Haythornthwaite) matches the retirement reason, though the paper itself offers no outcome evidence.

**Open questions**
- Does weighted out-degree on an as-of network predict passage of a member's sponsored bills beyond committee position?
- Should the Katz-Bonacich benchmark, the only term Battaglini found significant, be the comparison object for any revived network signal?
- Where exactly in the workbook are AB12/AB13 and the lens Centrality% column, and which centrality column feeds the lens?

**Read first:** [[battaglini-2018-effectiveness-connected-legislators]], [[sotoudeh-2024-network-based-measure]], [[battaglini-2016-influencing-connected-legislators]], [[azaiez-2025-hypergraph-analysis-european]], [[christopher-2016-mathematical-modeling-analysis]], [[ringe-nd-pinpointing-powerful-voting]]

## Undervaluation (underdog) lens

**Computes.** Underdog rank = Intrinsic - Visibility, where Intrinsic = 0.5 Leg% + 0.5 Centrality% and Visibility = 0.5 Press% + 0.5 Committee%.

**Method.** Difference of two equal-weight composites, ranked. The brain note gives no code file.

**Code refs.** 

**Assumptions**
- Legislative activity and centrality measure intrinsic influence; press and committee measure visibility

**Evidence** (3 rows, 3 distinct works; 3 VERIFIED-SOFTENED)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | qualifies | Third-coalition members are usually ideologically aligned with the majority party. The paper attributes the majority party's effectiveness to agenda-setting power, and says a substantial portion of the apparent majority advantage comes from the third coalition. Third-coalition effectiveness drops temporarily during majority transitions and rests on a few ultra-effective members. | The paper defines its 'hidden' bloc by signed-tie structure, not by low press or committee visibility, and its effectiveness tracks majority alignment. That is closer to what the lens subtracts (committee position) than to what it rewards. The link is an analogy and is not evidence for the lens's formula. | weak | VERIFIED-SOFTENED |
| [[amsalem-2018-effect-politicians-personality]] | 2018 | qualifies | Holding office is the strongest predictor of visibility: minister beta .369 (b 3.122), about three times the trait betas. Party leader beta .195 is only slightly stronger than the personality effects. Office here means minister, party leader and house speaker; committee position is not tested. | The paper's evidence is MPs in three parliamentary systems, not US House members. Its visibility construct is press mentions, which Press% does not capture. Committee% is a visibility input the paper never tests. Any reading of personality as part of the underdog residual is an inference, not a finding. | weak | VERIFIED-SOFTENED |
| [[battaglini-2018-effectiveness-connected-legislators]] | 2018 | qualifies | After correcting for endogenous tie formation, eigenvector and betweenness centralities lose significance for LES (Table 4). Only the model-weighted Katz-Bonacich term carries signal. House only, 109th-113th. | Applies only if the lens treats Centrality% as hidden effectiveness. If it is a descriptive visibility contrast, the endogeneity point weakens. The paper covers the House only. | weak | VERIFIED-SOFTENED |

**Assessment**

**Underdog lens: 3 rows, 3 distinct works; 3 VERIFIED-SOFTENED, all weak or moderate. The evidence weakens the lens rather than supporting it.**

**Formula and status.** Underdog = Intrinsic (0.5 Leg% + 0.5 Centrality%) minus Visibility (0.5 Press% + 0.5 Committee%). The lens has no code file. Which centrality column feeds it is unconfirmed.

**What the rows show**
- **Aref & Neal 2021, the conceptual anchor, is an analogy only.** Its hidden third coalition is defined by signed-tie structure, not low visibility. Its effectiveness tracks majority alignment and agenda-setting, and it rests on a few ultra-effective members. That is closer to what the lens subtracts (committee position) than to what it rewards.
- **Battaglini et al. 2018.** Centrality has no corrected association with LES (House, 109th-113th). Half of 'Intrinsic' therefore rests on a measure with no corrected signal.
- **Amsalem et al. 2018.** Office is the strongest driver of media visibility: minister beta .369, about three times the personality betas (MPs in Belgium, Canada and Israel). The paper does not test committee position and measures press mentions, which Press% (own-domain statements) does not capture.

**Conclusion.** Nothing validates Committee% as 'visibility' or Leg% + Centrality% as 'intrinsic'. The lens is an unvalidated contrast and should be labelled as such.

**Contradictions**
- Aref & Neal's hidden effective bloc owes its effectiveness to majority alignment and agenda-setting power. That is structurally closer to the Visibility side (committee position) than to the Intrinsic side the lens rewards.
- Centrality was retired from Influence because it lacks a valid influence reading, and Battaglini 2018 finds no corrected LES signal. Yet it is half of the lens's Intrinsic composite.

**Gaps**
- No test that underdog rank predicts realized effectiveness beyond visibility. Fill: prospective single look comparing high-underdog members' next-Congress CEL LES or CU-bill advancement with what their visibility predicts, chamber- and majority-stratified.
- No implementation record: no code file and an unconfirmed Centrality% column. Fill: document the producer before any test.
- Committee% as 'visibility' is untested. Fill: correlate Committee% with an actual media-mention count for US members.

**Paul's position vs the evidence.** Contradicted on the anchor. Paul reads Aref & Neal's hidden bloc, whose actions 'otherwise avoid typical headlines', as the conceptual twin of the underdog lens, and as the responsive or vulnerable seats. The verified row says the bloc is defined by signed ties, is usually aligned with the majority, and draws its effectiveness from agenda-setting. That is not a low-visibility, under-priced group in the lens's sense, and nothing in the paper links it to responsive or vulnerable seats. The link is an analogy and is not evidence for the formula.

**Open questions**
- Which centrality column feeds Centrality%: the retired InfluenceScore or another measure?
- Should Centrality% be dropped from Intrinsic given the retirement evidence, leaving Leg% vs visibility? Paul's call.
- Is the lens a ranking claim or a display-only descriptive contrast? The evidentiary bar differs.

**Read first:** [[battaglini-2018-effectiveness-connected-legislators]], [[aref-2021-identifying-hidden-coalitions]], [[amsalem-2018-effect-politicians-personality]]

## Legacy Sub Layer -> Primary Layer -> Rank & Allocation cascade

**Computes.** Legacy 11-dimension SUMPRODUCT score and rank (Rank & Allocation cols K/L/M).

**Method.** This cascade is orphaned in the Blended version but not retired. Its rankings disagree with Blended. Primary Layer - Normalized H2 = 0.80 (likely meant to be 0.08), so the weight row sums to 1.72.

**Code refs.** `Rank & Allocation cols C-M; Primary Layer - Normalized row 2`

**Assumptions**
- Superseded design; recorded for lineage only

**Evidence** (no rows)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment**

**Legacy cascade (Sub Layer to Primary Layer to Rank & Allocation): GAP, no rows.**

The cascade is orphaned but not retired. Its 11-dimension SUMPRODUCT ranking disagrees with the Blended rank. It also carries a known defect: Primary Layer - Normalized H2 = 0.80, likely meant to be 0.08, so the weight row sums to 1.72 instead of 1.

Any comparison of legacy vs Blended rank is distorted until that cell is fixed or the cascade is formally retired. No literature bears on the question. It is an internal head-to-head that needs a single preregistered look against an external criterion.

**Contradictions**
- The weight row sums to 1.72 because of the probable 0.80/0.08 typo in H2, so the legacy rank as it stands is not the intended design.

**Gaps**
- No comparison of legacy vs Blended rank against CU-aligned final-passage votes in the 119th Congress. Fill: fix or flag H2 first, then run one preregistered look with Spearman and bootstrap CIs, chamber-stratified.
- Retirement status is undecided. Fill: Paul's ruling on retire vs repair.

**Paul's position vs the evidence.** Paul's position (no literature) is correct: no rows. The evidence set adds nothing beyond confirming that this is an internal lineage question.

**Open questions**
- Retire the legacy cascade formally, or repair H2 and keep it as a comparison baseline? Paul's call.
- If kept, which external criterion should arbitrate legacy vs Blended?

**Read first:** (none)

## Across components

**Three problems cut across the pillars.**

**1. Chamber pooling is a fault line at every stage.**
- Leg% is one percentile over House and Senate.
- Influence% is one raw/max over both chambers, with identical leadership multipliers.
- The blend ranks 533 members on one scale.

The only evidence on cross-chamber comparability (Bucchianeri et al. 2025, NC 2005-2012) says effectiveness scores are chamber-term relative. Its own chair premium splits 0.609 lower vs 0.307 upper. Any validation look the project runs should report chamber strata, or label pooled results UNSTRATIFIED.

**2. Majority status is absent from every pillar.**
It is the most consistent covariate across the effectiveness papers: NC +0.370 (strata 0.362 / 0.387); House passage logit 1.353 for majority sponsors (106th-111th, UNSTRATIFIED); the chair LES premium in the House (Battaglini 2018). Two pillars probably carry it implicitly: the efficiency term (advancement rates) and the committee score (chairs are majority by construction). The blend may therefore partly rank majority membership. This is an analyst inference to test, not a finding.

**3. The centrality retirement is applied inconsistently.**
Battaglini 2018 and Sotoudeh 2024 justify removing direction-blind centrality from Influence. The same family of measures remains half of the underdog lens's 'Intrinsic' composite. Either the lens should drop Centrality%, or it should be explicitly labelled a descriptive contrast with no influence reading (D-025).

**Shared overlap and limits**
- **Overlap.** Bucchianeri 2025 and Battaglini 2018 bear on several components each and are the two load-bearing works for this system. Kim et al. 2020 and 2023 are one study line and should not be double-counted.
- **Transfer limits.** Most evidence transfers imperfectly: NC state legislature (Bucchianeri), US state legislatures (Fouirnaies), Senate Republicans only (Thakur), EC lobby network (Azaiez), Austria (Haselmayer), Belgium, Canada and Israel (Amsalem), simulated diffusion (Kempe, Trivedi, Kumar). The federal House rows (Battaglini 2016 and 2018, Sotoudeh, Box-Steffensmeier, Kim et al.) carry the most weight. None of them uses a CU-specific outcome.
- **No CU outcome anywhere.** No paper in the set tests anything against CU-specific outcomes (CU-bill advancement or CU-aligned votes). That is the single largest gap, and it applies to every component.
- **Owner policy.** Every weight is owner policy. The honest label for the rank today is "owner-weighted composite, unvalidated against an external criterion, with House and Senate pooled".

**Rule checks**
- No paper argues for either rejected SNA proposal.
- LDA-derived papers (Kim & Kunisky; Huneeus & Kim) are usable as reading only (D-021). A Huneeus-style referral-share weighting must use congress.gov billstatus.
- Reciprocity does not arise in this system's evidence.
- The UNVERIFIED rows are leads only (DE-062): van der Does on press hedging, and the Frisch & Kelly codebook on committee selection.

## Research agenda

1. Prerequisite (Paul's ruling, S-5): name the ratified pillar weight set (30/25/40/5, 25/30/40/5 or the live 45/10/40/5) and reconcile pillar_policy.py, MATHEMATICAL_SPECIFICATION.md:12 and the brain notes.
2. Weight sensitivity (internal, no look needed): Kendall tau-b and top-133 overlap across the three weight sets, plus a Christopher-style one-weight proportional-rescale sweep. Report effective variance shares per pillar under the current mixed normalizations.
3. Preregistered single look, the external criterion: blended rank and each single pillar vs CEL LES (federal) and CU-aligned final-passage votes in the 119th Congress. Spearman with bootstrap CIs, reported separately for House and Senate and by majority status. Verify or replace the brain's roughly +0.10 rho with CEL LES.
4. Chamber and majority structure: re-run Leg% and Influence% percentiles within chamber, and add or stratify by majority status. Measure the rank change and the change in criterion validity.
5. Committee weights: build a billstatus referral-share committee map (Huneeus-style, lagged share of CU-category bills referred; no LDA per D-021) and compare it with the owner tier map on CU-bill markup and advancement for referred bills, chamber-stratified. Also check the tier of Ways and Means, Finance and Appropriations seats, and test additive counting of nested subcommittees.
6. Committee selection: read the Frisch & Kelly House Committee Request codebook (currently unread, UNVERIFIED). If it has request-vs-assignment fields, test whether committee scores reflect prior interest rather than seat leverage (historical, House only).
7. Network: build an as-of cosponsor network from bills resolved before the outcome window, and test weighted out-degree (60% of InfluenceScore, untested anywhere) against subsequent passage of sponsored bills beyond committee position, with a Battaglini-style endogeneity control and Katz-Bonacich as the comparator. Outputs remain heuristic (D-025).
8. Underdog lens: locate its producer and the Centrality% column in the workbook, along with AB12/AB13. Decide whether to drop Centrality% from Intrinsic. Then run a prospective test of whether high-underdog members outperform their visibility on next-Congress LES or CU advancement.
9. Legislative pillar: test incremental validity of efficiency over agenda volume, and stage-based progression vs binary advancement, controlling majority status. Test sign-only vs stance-signed agenda against CU votes.
10. Press pillar: preregistered look of own-domain press stance at t vs CU cosponsorship and votes at t+1. Compare sign-only vs sign x LSD intensity, and with vs without the third-party (americascreditunions.org) rows. Correlate press volume and LSD charge with district lean to test the van der Does hedging lead, chamber-stratified.
11. Demographic pillar: partial correlation of Demo% with CU-aligned behaviour controlling party and chamber. Compute the rank delta from swapping column F for Demographic Potential v3.0, and give column F a reproducible producer.
12. Engineering: reproduce post-repair Sub Layer efficiency from raw congress.gov billstatus in an independent pipeline (533-member match count, max deviation). Fix or formally retire the legacy cascade (H2 0.80 vs 0.08; weights sum to 1.72) before any legacy-vs-Blended comparison.
