---
type: system
name: Moneyball four-pillar influence rank
slug: moneyball-rank-pillars
components: [moneyball-rank-pillars.score-blend, moneyball-rank-pillars.committee-pillar, moneyball-rank-pillars.legislative-pillar, moneyball-rank-pillars.press-pillar, moneyball-rank-pillars.demographic-pillar, moneyball-rank-pillars.sublayer-efficiency-repair, moneyball-rank-pillars.network-centrality-influencescore, moneyball-rank-pillars.undervaluation-lens, moneyball-rank-pillars.legacy-workbook-cascade]
brain_notes:
  - C:/Users/pgoff/Documents/brain/concepts/four-pillar-percentile-blend.md
  - C:/Users/pgoff/Documents/brain/concepts/influence-vs-alignment-axes.md
  - C:/Users/pgoff/Documents/brain/actors/moneyball-workbook.md
  - C:/Users/pgoff/Documents/brain/actors/moneyball-workbook-rules-scoring-and-sublayer-surgery.md
  - C:/Users/pgoff/Documents/brain/actors/moneyball-workbook-lineage-and-june-audit.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-rfc-melange-architecture-system-of-record-model-config-and-committee-formula.md
paper_count: 23
tags: [system]
---

# Moneyball four-pillar influence rank

The official 533-member CU-influence rank. It blends Legislative, Press, Influence (committee) and Demographic pillars into a score; ranks_python.json (fec/engine/compute_pillars.py) is the rank authority and the workbook is a view of it. Includes each pillar's construction, the legacy workbook cascade and the underdog lens. Code paths are relative to C:/Users/pgoff/OneDrive/Desktop/Melange Systems.

Brain notes: [four-pillar-percentile-blend](obsidian://open?vault=brain&file=concepts/four-pillar-percentile-blend), [influence-vs-alignment-axes](obsidian://open?vault=brain&file=concepts/influence-vs-alignment-axes), [moneyball-workbook](obsidian://open?vault=brain&file=actors/moneyball-workbook), [moneyball-workbook-rules-scoring-and-sublayer-surgery](obsidian://open?vault=brain&file=actors/moneyball-workbook-rules-scoring-and-sublayer-surgery), [moneyball-workbook-lineage-and-june-audit](obsidian://open?vault=brain&file=actors/moneyball-workbook-lineage-and-june-audit), [2026-07-rfc-melange-architecture-system-of-record-model-config-and-committee-formula](obsidian://open?vault=brain&file=topics/2026-07-rfc-melange-architecture-system-of-record-model-config-and-committee-formula)

## Moneyball four-pillar influence rank: evidence assessment

**System.** This is the official CU-influence rank of 533 members. Score = w·Leg% + w·Press% + w·Influence% + w·Demo%. The rank authority is `ranks_python.json`, produced by `fec/engine/compute_pillars.py`. The ledger has 29 paper-bearing rows (23 distinct papers) across 9 components, 6 of which have rows: 27 VERIFIED or VERIFIED-SOFTENED (8 VERIFIED, 19 VERIFIED-SOFTENED) and 2 UNVERIFIED.

**Headline.** No paper in the ledger validates any number the system uses. That covers the pillar weights, committee tier weights, the 1/6 subcommittee factor, the leadership ladder, the 4:2:1 role weights, the press category tiers and the 0.60/0.25/0.15 centrality weights. The verified literature supports a few qualitative premises:
- Money and lobbying concentrate on members who hold committee jurisdiction (Kim 2020/2023, manufacturing firms, federal, 2008-2018).
- Decayed success ratios carry outcome information (Sotoudeh 2024, House 2009-2019, UNSTRATIFIED).
- Stage-based effectiveness is a valid construct (Bucchianeri 2025, but in North Carolina's legislature, not Congress).
- Plain centrality is a weak influence signal (Sotoudeh 2024; Battaglini 2016; Azaiez 2025). This backs the 2026-06-25 retirement of centrality from Influence, though loosely.

**Contradictions to name.**
1. **Three weight sets are in play.** The code (`pillar_policy.py:11-16`) runs 45/10/40/5, which is unratified (S-5 open). The spec (`MATHEMATICAL_SPECIFICATION.md:12`) says 25/30/40/5. The ratified-set record says 30/25/40/5.
2. **The Bucchianeri bearing questions the pooled roster.** The paper says effectiveness scores are relative to chamber-term and should not be compared across chambers. The system pools House and Senate in one percentile (Leg) and one raw/max (Influence), with the same chair multipliers in both chambers and no majority-status term.
3. **The underdog lens contradicts the retirement.** It counts cosponsor centrality as "intrinsic" influence, yet the system retired that same centrality from Influence as direction-blind.
4. **The press normalization is described two ways.** The component says signed raw/max. The code (`normalize_signed_extrema`) divides positives by the largest positive and negatives by the largest absolute negative, so the two sides have separate scales.

**Gaps.** Three components have no paper evidence:
- Demographic pillar
- Sub Layer efficiency repair (engineering)
- Legacy workbook cascade (lineage only)

The press pillar's two most distinctive choices also have no evidence: own-domain filtering and 1/n per-event normalization.

**Paul's positions vs the evidence.** His positions are mostly honest about this state of affairs. He says the weights are owner policy, the committee-selection problem is unresolved, and direction-blind centrality should stay retired. The evidence agrees on all three. Where the evidence pushes back:
- **Committee pillar:** Bucchianeri's chamber and majority strata, and Thakur's finding that Small Business has low common value while all 74 of its seats are tiered Highly.
- **Underdog lens:** Paul reads Aref & Neal's hidden bloc as the conceptual twin. The paper's bloc is defined by signed-tie structure and its effectiveness tracks majority alignment, which is closer to what the lens subtracts than to what it rewards.

## Four-pillar score blend and rank

**Component id:** `moneyball-rank-pillars.score-blend`

**What it computes.** Score = w2*Leg% + w3*Press% + w4*Influence% + w5*Demo%; Rank = descending competition rank over 533 members. ranks_python.json is the rank authority.

**Method.** Linear weighted sum of normalized pillars. Normalization differs by pillar: legislative uses Excel PERCENTRANK.INC truncated to 5 decimals, committee uses raw/max, press uses signed raw/max, and demographic uses PERCENTRANK. Weight history: .40/.30/.20/.10 (2026-06-29, historical); 30/25/40/5 (ratified 2026-07-07); 45/10/40/5 (Leg/Press/Inf/Demo), live in fec/engine/pillar_policy.py since 2026-09-03 and unratified (code-review S-5 open). VERIFIED: pillar_policy.py:11-16 reads leg .45, press .10, inf .40, demo .05. Its docstring says 10 of the original 25 press points remain and 15 moved to legislative, which implies the ratified set was Leg .30 / Press .25. MATHEMATICAL_SPECIFICATION.md:12 instead writes 0.25*Leg + 0.30*Press, and brain notes disagree on the order. Unresolved.

**Code refs.** `fec/engine/compute_pillars.py:11-14,36-59,195-216`, `fec/engine/pillar_policy.py:11-16`, `fec/engine/apply_weights.py:11-13`, `MATHEMATICAL_SPECIFICATION.md:12`, `ranks_python.json`

**Assumptions**
- Pillar values after normalization are commensurable, so fixed weights are meaningful
- Weights are owner policy, not estimated from outcomes
- Rank measures influence; stance (alignment) is a separate axis
- Min-max was rejected because one outlier set each pillar's scale (effective variance shares at 06-25: Press 40 / Leg 6 / Committee 52 / Demo 2)

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[christopher-2016-mathematical-modeling-analysis]] | 2016 | method-source | Criterion weights come from AHP pairwise comparison on Saaty's nine-point scale, accepted at CR below 0.10 (here 0.062). Robustness follows Alinezhad and Amini Equation 1: change one weight in 0.1 steps, rescale the others proportionally, and track rank. Raising the out-degree weight keeps Freedom Partners Chamber of Commerce first. Raising the weight the paper labels lowest (closeness, Figure 5) lets Center to Protect Patients Rights overtake it. | The one-weight proportional-rescale sweep can be used directly for the open top-133 sensitivity question. AHP would only document owner judgment and check its internal coherence; it gives no outcome validity, and the paper's own weights are unvalidated. Any AHP-derived set must be labelled owner policy with its CR, never an estimate. | weak | VERIFIED-SOFTENED |
| [[bucchianeri-2025-legislative-effectiveness-american]] | 2025 | qualifies | Stage-based effectiveness is a valid construct: SLES explains about half the variation in NC expert rankings, and lagged SLES persists (rho 0.38 when majority entered, 0.40 when minority entered, 0.66 with no change in control). Effectiveness reflects innate or cultivated lawmaking ability as well as position. | The persistence and NC validity figures are from the state paper and do not validate the federal blend. The orthogonality reading depends on CEL LES being the federal analogue. Under bar B, +0.10 falls in the null band, which matches the rel's wording, but the +0.10 value itself was not verified. | moderate | VERIFIED-SOFTENED |
| [[becker-nd-gameability-redistricting-criteria]] |  | context | When a criterion admits several defensible metrics, each metric favors a different plan, so stakeholders can cherry-pick the metric their plan wins on. The paper recommends that states clarify metric definitions rather than set hard numeric thresholds. | Analogy only. The paper studies redistricting metrics, not legislator rankings, and provides no evidence about these weights. It supports pinning one documented weight set and reporting rank sensitivity across the competing sets; it does not justify any particular weights. | weak | VERIFIED-SOFTENED |

Rows: 3 (3 verified, 0 unverified).

**Assessment**

**Evidence weight: weak, with no validation of the weights.** Three verified-softened rows bear on this component, and none of them tests the blend.

- **Bucchianeri 2025 (moderate, qualifies).** Stage-based effectiveness is a valid construct. In North Carolina 2005-2012, SLES explains about half the variance in expert rankings, and lagged SLES persists (rho 0.38 to 0.66, depending on change in majority control). This supports Paul's interest in measuring effectiveness. It is a state-legislature result, so it does not validate the federal blend.
  - The brain records a blended-rank vs CEL LES correlation of +0.10. Under bar B in `lep_criterion.py:17-19` that falls in the null band (below 0.15). The +0.10 value and its n were not re-verified in this pass.
  - If the figure holds, the blended rank is close to orthogonal to the Volden-Wiseman effectiveness construct. That is a real tension with the assumption that "Rank measures influence".
- **Christopher 2016 (weak, method source).** Supplies a usable one-weight proportional-rescale sweep for the open question of top-133 sensitivity. Its AHP procedure could document owner judgment with a consistency ratio, but it gives no outcome validity.
- **Becker (n.d.; weak, context).** An analogy from redistricting: when several defensible metrics exist, each favours a different answer. It supports pinning one documented weight set and reporting sensitivity across the competing sets.

**What is still open.** No row addresses the mixed normalization: PERCENTRANK for Leg and Demo, raw/max for Influence, and signed split extrema for Press. The question is whether that mix makes the effective variance shares diverge from the nominal weights. That divergence is the defect that drove the earlier rejection of min-max: at 06-25 the effective shares were Press 40 / Leg 6 / Committee 52 / Demo 2 against the nominal weights then in force.

**Contradictions**
- Three weight sets coexist: code pillar_policy.py:11-16 runs Leg .45 / Press .10 / Inf .40 / Demo .05 (live since 2026-09-03, unratified, S-5 open); MATHEMATICAL_SPECIFICATION.md:12 says .25 Leg / .30 Press; the pillar_policy docstring implies the ratified set was Leg .30 / Press .25. The order of the ratified set is unresolved.
- If the brain-recorded +0.10 correlation between blended rank and CEL LES holds (unverified here), it falls in the null band of bar B and sits in tension with the assumption that 'Rank measures influence': the rank is near-orthogonal to the leading published effectiveness construct.

**Gaps**
- No study tests whether the blend predicts an external criterion better than any single pillar. Needed: a pre-registered single-look comparison of blended rank vs each pillar against CEL LES (119th Congress) and CU-aligned final-passage votes, stratified by chamber and majority status, with window and denominator stated.
- Effective variance shares under the current mixed normalization are unmeasured. Needed: decompose Var(Score) into each pillar's contribution under 45/10/40/5 and 30/25/40/5 on the 533-member roster.
- Rank sensitivity is unmeasured. Needed: Christopher-style one-weight sweeps plus Kendall tau and top-133 overlap between the competing weight sets.

**Paul's position vs the evidence**

Consistent. Paul says no literature underwrites the weights and that they are owner policy, and the evidence confirms it: no row estimates or tests them. His reading of Sotoudeh and Aref & Neal as support for measuring effectiveness is fair at the construct level. But the only verified validity evidence is from a state legislature (Bucchianeri, NC), and the recorded LES correlation, if it holds, suggests the current blend does not measure that construct.

**Open questions**
- Which weight set is ratified: 30/25/40/5 or 25/30/40/5? Is 45/10/40/5 to be ratified (S-5)?
- Is the +0.10 blended-rank vs CEL LES correlation reproducible, and what are its window, n and chamber strata?
- Should all four pillars share one normalization, so that the nominal weights equal the effective shares?

**Read first** [[bucchianeri-2025-legislative-effectiveness-american]], [[christopher-2016-mathematical-modeling-analysis]], [[becker-nd-gameability-redistricting-criteria]], [[sotoudeh-2024-network-based-measure]]

## Committee - Influence pillar

**Component id:** `moneyball-rank-pillars.committee-pillar`

**What it computes.** Influence% = committee_raw / MAX(committee_raw), where committee_raw = (Membership + Leadership)/2 from seats held.

**Method.** Seat weight by CU-relevance tier: Highly 3.0, Moderately 1.8, Slightly 0.9; subcommittee seats (names containing ' - ') weigh 1/6. Leadership = seat weight x multiplier: Chair 3.0 full / 7.5 sub, Ranking 2.0 / 4.5, Vice 1.0 / 2.0 (Paul applied chair/ranking x1.5 by hand on 2026-06-30). Replaced a double-PERCENTRANK version (CMTEFIX). Centrality was retired from Influence on 2026-06-25 (AB13 = 0) as direction-blind. The formal-office party-elite bonus is display-only (set to 0 at compute_pillars.py:163). Judiciary was added at tier Slightly.

**Code refs.** `fec/engine/committee_scores.py:1-185`, `fec/engine/apply_committee_fix.py:3-18`, `fec/engine/apply_committee_reweight.py:15-37`, `fec/engine/add_judiciary_seats.py:3-27`, `fec/engine/influence_roles.py:11-30`

**Assumptions**
- Committee seat relevance and leadership measure structural CU power
- Subcommittee seats are worth 1/6 of a full seat
- Tier labels and multipliers are owner judgment, not estimated

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2020-strategic-sequential-links]] | 2020 | supports | Donation-linked lobbying effects are particularly stronger for the committee-membership channel than for co-sponsorship or sponsorship; the authors read this as firms targeting politicians with legislative power over their interests (committee jurisdiction over the firm's industry). | Only the qualitative point carries: money and lobbying converge on members holding jurisdiction. Nothing about tier weights, the 1/6 factor or the leadership ladder. Kim's 'powerful committee' covariate is Groseclose-Stewart (Appropriations, Armed Services, Finance, Rules and Administration), not Financial Services or Banking. Committee outcome also has a higher base rate than sponsorship (fn 13), so 'stronger' partly tracks channel breadth. Sample is manufacturing firms. | moderate | VERIFIED |
| [[kim-2020-mapping-political-communities]] |  | supports | Legislators who interact with probability at least 40% in the 'Finance & Insurance' community have committee memberships concentrated in relevant committees such as House Financial Services; high-entropy legislators sit on power committees (House Ways and Means, Senate Appropriations), and broad groups such as the CHAMBER OF COMMERCE target them. Window: 113th Congress; lobbying-attention counts, not outcomes. | Supports the premise that lobbying attention is organised by jurisdiction. It does not show seats produce CU outcomes. One Congress, descriptive. The power-committee finding suggests checking how tax-writing and appropriations seats are tiered; not verified here. | moderate | VERIFIED-SOFTENED |
| [[huneeus-2018-effects-firms-lobbying]] | 2018 | suggests-extension | Committee relevance is measured from data. Weight w_ict-k is the firm's lagged share of lobbied bills referred to committee c, with t-1 preferred and t-2 and t-3 in robustness (Appendix F). Interacted with committee seats held by politicians from the firm's headquarters state, the first-stage F is 14.30 (all years) and 13.10 (post-2007) in Table 2. Deeper-lag panels in Table F.1 give F of 6.0 to 9.9. About 30 percent of politicians change committee between Congresses (Section 4.1; Figure 6). | Extension as stated: compute each committee's lagged share of CU-category bills referred, from congress.gov billstatus rather than LDA lobbied-bill lists (D-021). H&K's weights are firm-specific and validated only as an instrument for firm revenue, with no evidence that referral shares predict a member's CU effectiveness, so any re-weighting needs its own look. | moderate | VERIFIED |
| [[porter-2005-network-analysis-committees]] | 2005 | qualifies | In the 107th House the four Intelligence subcommittees each include at least half of the 20-member parent committee and at least one third of each other subcommittee, with normalized interlocks of 14.4 to 26.6 among the five nodes. Single-linkage clustering places subcommittees at the bottom of a roughly four-level hierarchy (subcommittees, committees, groups, House). | The nesting evidence is one parent committee in one Congress, so it cannot support a general statement that subcommittee seats are nested in the parent. The paper calls these overlaps expected and measures membership redundancy, not influence, so it says nothing on whether additive counting is wrong for CU outcomes. The 1/6 factor is a code constant; its origin as owner judgment is not shown in the lines read. | weak | VERIFIED-SOFTENED |
| [[porter-2005-network-analysis-committees]] | 2005 | suggests-extension | Committees differ in mean partisan extremism of their members. Select Homeland Security ranks 4th of 113 committees and above all 19 standing committees. Rules is the second most extreme standing committee (after Judiciary) and 18th of 113. Intelligence and its four subcommittees rank among the 10 least extreme of 113. | Extremism is relative to the 107th House mean, tracks party, and is descriptive with no test. It is evidence of heterogeneity across committees, not a weight to adopt. Any use would mix party loyalty with CU leverage, the chicken-or-egg problem Paul raised. | weak | VERIFIED |
| [[kim-2023-systemic-sequential-links]] | 2023 | supports | Committee-linked lobbying after a donation is the largest channel and grows over time (PAC 0.103 to 0.147 over t+0 to t+3). Hypothesis H3 predicts the committee channel is strongest (pp.6-8). Committee chairmanship enters the model only as a control. | The paper supports only the claim that committee position is where interest-group money and lobbying concentrate. It does not estimate chair, ranking, or subcommittee weights, and it does not estimate committee effects on bill outcomes. Sample is manufacturing firms, federal level, 2008-2018. | weak | VERIFIED |
| [[thakur-2025-matching-theory-perspective]] | 2025 | qualifies | Senate seats are filled by two party mechanisms. Republicans use a seniority-ordered Serial Dictatorship that the paper says seems to be run as TTC/YRMH-IGYT in practice. Democrats use a Boston approximation with COC tie-breaking. Cross-party seniority-at-appointment gaps on some committees are read as possible party discipline: Environment and Public Works R mean 3.80 (N=30) vs D 1.00 (N=14), diff 2.80, t=3.05; Commerce, Science and Transport diff -1.74, t=-2.09; 104th-114th. | Senate rows only (about 100 senators; the 533 total is not from the paper). Paper calls the evidence suggestive (p. 18). Only Environment and Commerce have reported t-stats; Budget and Rules differences rest on SD gaps with no test. Supports only that seat selection differs by party, not the tier weights or any power claim. | moderate | VERIFIED-SOFTENED |
| [[thakur-2025-matching-theory-perspective]] | 2025 | qualifies | Committees differ in common value. Rules, Finance, Governmental Affairs and Appropriations are the most sought after even after excluding the most senior members. In the Republican reduced-form model, Finance and Appropriations have large common value and small idiosyncratic spread, while Constituency committees such as Small Business have lower common value. Sample: 426 assignments, 25 committees, 97 senators, 104th-114th. | Tables A7 and A8 numeric cells are garbled in extraction, so the lower-common-value claim for Small Business is verified only as text, not numerically. Republican Senate 1995-2017 only. Use as a validity caveat, not a re-tiering input. | weak | VERIFIED-SOFTENED |
| [[bucchianeri-2025-legislative-effectiveness-american]] | 2025 | qualifies | Committee chairs score 0.507 above rank-and-file in the pooled full-sample estimate (strata: 0.609 lower chamber, 0.307 upper). The chair gap widens with log chamber size (0.205, p<0.05) and narrows where chambers vote on committee appointments (-0.180, p<0.01). Majority status adds 0.370 (strata 0.362 and 0.387). | The paper's estimates are associational with endogeneity unaddressed (p. 34) and do not estimate multipliers. The chamber-size transfer to a 435-seat House and 100-seat Senate is the analyst's extrapolation, not the paper's claim. The Ranking-to-Chair ratio of two-thirds holds only for full weights; for sub weights it is 4.5 over 7.5, or 0.60. | moderate | VERIFIED-SOFTENED |
| [[anon-nd-house-committee-requests]] |  | suggests-extension | UNVERIFIED and unread: the codebook (0 chars extracted) could not be opened. Paul's annotation names it 'Frisch & Kelly 2007, House Committee Request Data'. From general knowledge only, it records House members' committee-assignment requests next to the assignments they received. No numbers can be quoted. | This applies only if the codebook confirms request-vs-assignment fields. Even then the data are historical (not the 119th Congress) and House-only, and they cover new assignments and transfers rather than continuing seats. They could support a design check: whether high committee scores reflect pre-existing interest (selection). They could not be used as a live input or an edge or weight source. Strength is weak until the document is read and a second path reproduces any result (DE-062). | weak | UNVERIFIED |

Rows: 10 (9 verified, 1 unverified).

**Assessment**

**Evidence weight: moderate for the premise, none for the numbers.** This component has the most evidence of any: 10 rows, 9 of them verified.

**Support for the premise that committee position is where interest money concentrates:**
- **Kim 2020 and Kim 2023 (VERIFIED).** Donation-linked lobbying is strongest through the committee channel. Committee-linked lobbying after a PAC donation rises from 0.103 at t+0 to 0.147 at t+3.
  - The sample is manufacturing firms, 2008-2018.
  - Kim's "powerful committees" are the Groseclose-Stewart set (Appropriations, Armed Services, Finance, Rules and Administration), not Financial Services or Banking.
  - The committee channel has a higher base rate than the sponsorship channel, so "stronger" partly reflects how broad the channel is.
- **Kim & Kunisky (n.d.; VERIFIED-SOFTENED).** In the 113th Congress, lobbying attention to Finance & Insurance concentrates on House Financial Services members. This is descriptive and is about attention, not outcomes.

**No row estimates the tier weights (3.0 / 1.8 / 0.9), the 1/6 subcommittee factor or the chair/ranking/vice ladder.**

**Qualifying evidence:**
- **Bucchianeri 2025 (moderate).** Chairs score 0.507 above rank-and-file. This is the pooled estimate; by stratum it is 0.609 in lower chambers and 0.307 in upper chambers. The chair gap widens with chamber size and narrows where chambers vote on appointments.
  - The system applies one ladder to both chambers and pools House and Senate in a single raw/max.
  - Majority status adds 0.370 (strata 0.362 / 0.387), and the system has no majority term.
  - All of this is from state legislatures and is associational.
- **Thakur 2025 (Senate, 104th-114th Congress, suggestive).**
  - Seat selection differs by party mechanism.
  - Small Business has low common value, while all 74 Small Business seat tokens are tiered Highly. Thakur's numeric tables are garbled in extraction, so this point is verified as text only.
- **Porter 2005 (weak).**
  - The subcommittee-nesting evidence covers one parent committee in one Congress. It cannot establish that additive counting double-counts.
  - Committees differ in partisan extremism. Using that as a weight would fold party loyalty into CU leverage, which is Paul's chicken-or-egg problem.

**Extension: derive relevance weights from data.** Huneeus & Kim 2018 (VERIFIED) uses lagged bill-referral shares to set committee relevance. The extension would use the share of CU-category bills referred to each committee, from congress.gov billstatus, not LDA lobbied-bill lists (D-021). Huneeus & Kim validated those weights only as an instrument for firm revenue, so any re-weighting needs its own single look.

**UNVERIFIED lead.** The Frisch & Kelly House committee-request codebook (0 characters extracted) could support a test of selection versus leverage. It is a lead, not a finding (DE-062).

**Contradictions**
- Bucchianeri 2025 reports chair effects that differ by chamber (0.609 lower vs 0.307 upper) and scores that should not be compared across chambers. The system applies identical Chair/Ranking/Vice multipliers in both chambers and pools House and Senate in one committee_raw/MAX (compute_pillars.py:181). The bearing is state-level, so this is a tension, not a refutation.
- Thakur 2025 places Small Business among low-common-value constituency committees (Senate, verified as text only). The live sheet tiers all 74 of 74 Small Business seat tokens Highly.
- Kim's 'powerful committee' covariate excludes Financial Services and Banking, so the 'committee channel is strongest' result does not establish that the system's CU tiers identify the right committees.

**Gaps**
- No study validates the 3.0/1.8/0.9 tiers, the 1/6 factor or the leadership ladder against CU outcomes. Needed: for CU-category bills in the 117th-119th Congress, regress markup or advancement on the committee score of the members of the committee of referral, stratified by chamber and majority status.
- Selection vs leverage is untested. Needed: compare members' CU activity before and after assignment (event study on new assignments), or use committee-request data once a second path verifies it.
- The 245 of 533 members with no committee row get a zero score, and this is untested. Needed: compare their CU outcomes with those of low-scoring seated members.

**Paul's position vs the evidence**

Partly supported. Paul's claim that money and lobbying are strongest in committee-related activity matches Kim 2020/2023 (VERIFIED), but only qualitatively, for manufacturing firms and for a different set of power committees. Porter supports committees as structure, not as a source of influence weights. The evidence supports Paul's chicken-or-egg concern (Thakur; Porter extremism; the Frisch & Kelly lead), and the pillar does nothing to address it. Gaynor's leadership insulation has no row in the ledger. Bucchianeri adds an issue Paul has not raised: majority status and chamber strata.

**Open questions**
- Should seat relevance come from CU-bill referral shares (billstatus) rather than owner tiers? That would need its own look.
- Should the leadership ladder or the normalization be chamber-specific?
- Should a majority-status term exist, or should majority status stay excluded as an alignment confound?
- Is the 74/74 Highly tier for Small Business intended?

**Read first** [[bucchianeri-2025-legislative-effectiveness-american]], [[huneeus-2018-effects-firms-lobbying]], [[kim-2020-strategic-sequential-links]], [[thakur-2025-matching-theory-perspective]], [[kim-2020-mapping-political-communities]], [[anon-nd-house-committee-requests]]

## Legislative pillar (stance-gated agenda plus efficiency)

**Component id:** `moneyball-rank-pillars.legislative-pillar`

**What it computes.** Leg% = percentile of a blend of agenda% and efficiency% (0.7/0.3 at 2026-06-25; live blend read from workbook AB10/AB11). Agenda = sum over topics of IMP[topic] x activity x stanceMult, so opponents go negative.

**Method.** Activity weights by role: sponsor 0.286, original cosponsor 0.143, cosponsor 0.0714 (letters count at original-cosponsor weight). stanceMult is signed from category_stance.json. Efficiency per category = decay-weighted sign of advanced sponsored or original-cosponsored bills over total decay weight; advancement is tri-state and unknown outcomes are excluded. The pillar stays PERCENTRANK because raw/max (tried 2026-07-01) squashed the mid-board. The live routing implementation is fec/engine/live_legislative_pillar.py.

**Code refs.** `fec/apply_leg_stance.py`, `fec/apply_legstance_undo.py`, `fec/engine/live_legislative_pillar.py:17-124`

**Assumptions**
- Sponsorship and cosponsorship in a CU topic reveal the direction of advocacy once signed by stance
- Advancement past introduction measures legislative efficacy
- category_stance.json is current when the step runs (known staleness trap)

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[fouirnaies-2022-electoral-incentives-affect]] | 2022 | qualifies | Bill sponsorship volume falls about 6% when reelection incentives end (-1.63, SE 0.77, mean 27.32; N=16,727) with no ideological change. Sponsorship volume partly reflects electoral advertising (legislators' webpages list bills introduced) rather than position. | Weak. The 6% effect at the state level is small, and in Congress it would touch only members in their final Congress or those running for other office. The paper supports the influence-vs-alignment split. It does not address the 4:2:1 role weights or cosponsorship. | weak | VERIFIED-SOFTENED |
| [[sotoudeh-2024-network-based-measure]] | 2024 | supports | A decayed pass/total success ratio is informative about outcomes. Passed bills carry higher cosponsor pass-ratio influence than failed bills in every 4-month window (relative difference 0.139 mean / 0.248 max at a 6-month half-life). The authors contrast their approach with LES, which has no decay and splits the process into finer stages. | Indirect support only. The paper's ratio is pairwise, aggregated to bills, and includes each bill's own outcome. The pillar's ratio is member-level and describes CU-topic advancement; the paper does not show that a member's success rate measures CU influence or predicts future behaviour. The paper pools across parties and 2009-2019 with no strata, so label UNSTRATIFIED. The authors report a decline from about 2015 and hypothesize it is correlated with polarization. | weak | VERIFIED |
| [[boxsteffensmeier-2018-taking-congress-interest]] | 2018 | qualifies | Passed House logit (Table 4, N = 6,597): hot bill 2.428 (SE .161), commemorative 1.386 (SE .150), majority-party sponsor 1.353 (SE .220), endorser count 0.013 (SE .004), cosponsors 0.004 (SE .001). Endorser social power is -0.035 (SE .032, ns) for passage and 0.020 (SE .031, ns) for reporting. | The paper estimates a covariate association over letter-bearing House bills pooled across 106th-111th Congresses. It is not a per-member efficiency model and says nothing about efficiency metrics. Reading the majority coefficient as showing that a raw advancement rate rewards majority status is an inference, not the paper's claim, and any adjustment is Paul's call. | moderate | VERIFIED-SOFTENED |
| [[bucchianeri-2025-legislative-effectiveness-american]] | 2025 | qualifies | In North Carolina 2005-2012 SLES outperforms hit-rate metrics against subjective rankings. Scores are relative to each chamber-term and should not be compared directly across chambers or eras. Majority status adds 0.370 in the pooled full-sample estimate (strata: 0.362 lower chamber, 0.387 upper), 58 percent above the minority mean of 0.636. | The NC criterion covers one state chamber pair over eight years; transfer to Congress is an assumption. The 0.370 figure is an unstratified pooled estimate and must carry strata or an UNSTRATIFIED label, which the rel does. | moderate | VERIFIED |

Rows: 4 (4 verified, 0 unverified).

**Assessment**

**Evidence weight: weak to moderate, and only for the efficiency term.**

- **Sotoudeh 2024 (VERIFIED, weak).** A decayed pass/total ratio separates passed from failed bills: relative difference 0.139 (mean) and 0.248 (max) at a 6-month half-life. The setting is House 2009-2019, pooled across parties, so it is UNSTRATIFIED.
  - This indirectly supports the decay-weighted efficiency term.
  - The paper's ratio is pairwise and includes each bill's own outcome. It does not show that a member's success rate measures CU influence. The authors also report the signal declining from about 2015.
- **Bucchianeri 2025 (VERIFIED, moderate).** Stage-based scores outperform hit rates against expert rankings (North Carolina, 2005-2012). The pillar's efficiency is a hit-rate-style advancement share, so this qualifies it.
  - Bucchianeri also says scores are relative to chamber-term. The pillar uses one percentile across House and Senate.
  - Majority status adds 0.370 (strata 0.362 / 0.387).
- **Box-Steffensmeier 2018 (VERIFIED-SOFTENED).** In the House logit for passage over the 106th-111th Congress (N = 6,597 letter-bearing bills), majority-party sponsor has coefficient 1.353 and hot-bill status 2.428. The inference that a raw advancement rate rewards majority status is the analyst's, not the paper's. The efficiency block has no majority or salience term.
- **Fouirnaies & Hall 2022 (weak, state legislatures).** Sponsorship volume falls about 6% when reelection incentives end, with no change in ideology. This supports the split between influence and alignment: volume and direction separate. In Congress it would affect only members in their final term.

**Nothing tests the 4:2:1 sponsor / original-cosponsor / cosponsor weights, the stance-signing, or whether efficiency adds validity beyond agenda volume.**

No row argues for dropping regular cosponsorship. The 0.0714 cosponsor weight stays as it is. Dropping it would revive a rejected proposal.

**Contradictions**
- Bucchianeri 2025: effectiveness scores are chamber-term relative and should not be compared across chambers. The pillar percent-ranks House and Senate members together on one roster.
- Box-Steffensmeier 2018 and Bucchianeri 2025 both show a large majority-party advantage in advancement (1.353 logit; 0.370 pooled with strata). The efficiency term is a raw advancement share with no majority adjustment, so it may partly measure majority status rather than efficacy. Whether to adjust is Paul's call.

**Gaps**
- The 4:2:1 role weights have no evidence. Needed: estimate role-specific marginal effects on CU-bill advancement, or on CU-aligned votes, in the 117th-119th Congress, with chamber strata.
- Stance-signed vs unsigned activity is untested. Needed: a single-look comparison of each against CU-aligned final-passage votes, using an as-of design so that no outcome leaks into the predictor.
- The incremental validity of the efficiency term over agenda volume is untested.
- The Ramey (conscientiousness and sponsorship) citation has no evidence row in this ledger.

**Paul's position vs the evidence**

Consistent but under-supported. Paul favours observable effort as an incentive-sensitive signal, and Fouirnaies & Hall support that effort and position separate, though only from state data and with a small effect. Paul admits no annotation covers the 4:2:1 weights, and the evidence confirms the gap. The evidence adds two cautions Paul has not addressed: the majority-status confound in advancement-based efficiency, and the chamber pooling.

**Open questions**
- Should efficiency be stratified or percent-ranked within chamber?
- Should advancement be adjusted for majority status, or is that an alignment signal the pillar should keep?
- Is the live 0.7/0.3 agenda/efficiency blend in workbook AB10/AB11 still what runs, given that the routing lives in live_legislative_pillar.py?

**Read first** [[bucchianeri-2025-legislative-effectiveness-american]], [[boxsteffensmeier-2018-taking-congress-interest]], [[sotoudeh-2024-network-based-measure]], [[fouirnaies-2022-electoral-incentives-affect]]

## Press pillar (own-domain, signed raw-max)

**Component id:** `moneyball-rank-pillars.press-pillar`

**What it computes.** Press% = signed press_value / MAX, where press_value = sum over categories of (stance sign x LSD tone intensity) x tiered category weight, for the member's own-domain statements only.

**Method.** Only *.senate.gov and *.house.gov rows count; 819 third-party rows were dropped, 816 of them americascreditunions.org. 1,310 synthetic billstatus rows were de-duplicated out. Category weights tiered 2026-07-02: Interchange, CU Tax Status, Credit-Card Mandates 0.30; Charter/FOM 0.15; Exam/NCUA 0.10; medium 0.08; low 0.02-0.05. Speech channel normalized at 1/n per event (a3174bc). Press is the only directional signal in the original design. Display-only overrides do not feed the pillar.

**Code refs.** `fec/apply_press_dedup.py`, `fec/apply_raw_max_norm.py`, `fec/moneyball_core.py:214-241`, `fec/engine/pillar_policy.py:2-55`

**Assumptions**
- A member's own-domain statements represent the member's stance; third-party mentions do not
- Stance labels are the owner's domain (never relabelled)
- Tone intensity x stance sign is a valid single press measure

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[haselmayer-2017-sentiment-analysis-political]] | 2017 | qualifies | Dictionary-based tonality runs in the same direction as expert coding but compresses effects. In App. 3 Table 6 the government-pair coefficient is -0.13 with the dictionary vs -0.40 with expert coding, and electoral losses are 0.12 vs 0.23. Authors call dictionary estimates more conservative than manual coding. Pooled regression with party fixed effects, standard errors clustered by party pair; adjusted R2 0.03 with n = 619 (Table 3 and Table 6 dictionary columns). | The evidence is one Austrian campaign, in German, on negativity only. It suggests dictionary intensity carries a weaker, noisier version of the human-coded signal, so multiplying stance sign by that intensity may add noise. The paper does not test a stance x intensity product, so this is a caution, not a refutation. The paper's pooled coefficients should be labelled as pooled across parties with party fixed effects if quoted. | weak | VERIFIED-SOFTENED |
| [[vanderdoes-2022-strategic-identity-signaling]] |  | qualifies | As cross-partisan outgroup share rose from 1 to 9 of 10, participants shared fewer tweets overall (AME -5.38, 95 percent CI -6.04 to -4.72) and shifted toward covert ones (AME 0.73). Covert tweets drew fewer outgroup dislikes but also fewer ingroup likes, so the message is lower-affect for the ingroup. | Requires that legislators in politically mixed districts or states reduce or code their CU statements the way the experiment participants did. The experiment paid at most 20 cents of cost and fixed audiences with no feedback, which is far from electoral stakes. Test by correlating own-domain press volume and LSD charge with district lean before acting; any pooled correlation across chambers is quoted with chamber strata or labelled UNSTRATIFIED. | weak | UNVERIFIED |

Rows: 2 (1 verified, 1 unverified).

**Assessment**

**Evidence weight: thin.** There is one verified-softened caution and one unverified lead.

- **Haselmayer & Jenny 2017 (VERIFIED-SOFTENED, weak).** Dictionary tone runs in the same direction as expert coding but compresses the effects (for example -0.13 vs -0.40). The setting is one Austrian campaign, in German, negativity only, with party-pair fixed effects (n = 619, adjusted R² 0.03).
  - Multiplying stance sign by LSD intensity may therefore add noise relative to sign alone. The paper does not test that product, so this is a caution, not a refutation.
- **Strategic identity signaling (UNVERIFIED lead).** In an experiment, people facing mixed audiences spoke less and more covertly. That would make a member hedging for a mixed district score like a lukewarm one.
  - The stakes were trivial (at most 20 cents) and audiences were fixed.
  - Before acting, test it on system data: correlate own-domain press volume and tone with district lean, with chamber strata.

**Nothing in the ledger bears on the pillar's two distinctive choices:**
- the own-domain filter, which keeps only *.house.gov and *.senate.gov and drops 819 third-party rows, 816 of them from americascreditunions.org;
- the 1/n per-event speech normalization.

The 1/n speech-channel normalization is a pillar-level scaling, not an SNA edge weight. It must not be reframed as per-edge volume normalization, which was measured and REJECTED. Fink 2023's author-volume normalized edges, cited elsewhere in the system, are that rejected proposal. FLAG Fink 2023 and do not recommend it for this pillar.

One description issue: the code divides positives by the largest positive value and negatives by the largest absolute negative value (`normalize_signed_extrema`). That is not a single signed raw/max, so the scales of the two sides differ.

**Contradictions**
- The component says Press% = signed press_value / MAX. The code (fec/stance/media_rules.py normalize_signed_extrema, via moneyball_core.py:214-237) normalizes positives and negatives by separate extrema. These are not the same transform.
- Haselmayer & Jenny suggest dictionary intensity is a compressed, noisier version of human-coded tone. The pillar multiplies stance sign by LSD intensity as if intensity adds signal. This is untested, so it is a caution, not a refutation.

**Gaps**
- Own-domain filtering has no evidence. Needed: a single look comparing the validity of press with and without third-party CU-lobby rows against later CU-aligned cosponsorship or votes, using an as-of design.
- The 1/n per-event normalization has no evidence. Needed: test whether high-volume speakers are proportionally more aligned (volume vs vote alignment, chamber strata).
- Sign x intensity vs sign alone is untested. Needed: a head-to-head validity comparison against votes.
- Paul's cited press sources (Bacik 2026, Birkenmaier & Lechner 2025, Ramey, Merkley & Stecula) have no evidence rows here.

**Paul's position vs the evidence**

Not yet engaged by the evidence. Paul wants to add press, news cycles and social feeds as determinants of pathway change (the Bacik augmentation) and treats public statements as the observable persona. Nothing in the ledger tests either. The only verified row cautions against the tone-intensity part of the current measure. The unverified signaling lead, if it held, would cut against treating public statements as a faithful readout of the persona in mixed districts. Paul notes that nothing he annotated covers own-domain filtering, and that remains true.

**Open questions**
- Should Press% use a single signed max or the current split extrema?
- Should the sign-only variant be tested against sign x LSD intensity?
- Does the own-domain filter improve or reduce validity against votes?

**Read first** [[haselmayer-2017-sentiment-analysis-political]], [[vanderdoes-2022-strategic-identity-signaling]], [[amsalem-2018-effect-politicians-personality]]

## Demographic pillar input (Demo%)

**Component id:** `moneyball-rank-pillars.demographic-pillar`

**What it computes.** Demo% = PERCENTRANK of Blended Model column F (5% weight).

**Method.** Column F is formula-backed and edited by hand; no Python producer writes it. Sub Layer V:AF holds hard-coded values. The DEMO_POTENTIAL dial does not move this input. A 06-21 bias audit found a mild Democratic tilt (bucket mean D 0.52 vs R 0.34), accepted at 5% weight.

**Code refs.** `Moneyball Main Version - Blended.xlsx Blended Model col F; Sub Layer V:AF`

**Assumptions**
- District demographics carry a small legitimate influence signal
- Demographic Potential is a market score, not a support prediction

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

Rows: 0 (0 verified, 0 unverified).

**Assessment**

**GAP: no paper evidence.**

The pillar is Demo% = PERCENTRANK of Blended Model column F, at 5% weight. Column F is hand-edited, and no Python producer writes it. Sub Layer V:AF holds hard-coded values.

The only internal evidence is the 06-21 bias audit. It found a mild Democratic tilt (bucket mean D 0.52 vs R 0.34), which was accepted at 5% weight. The denominator and bucket definitions are in the brain note, not re-verified here.

Paul's cited source, Huneeus & Kim 2018, uses home-state committee seats as an instrument for firm revenue. It does not show that district demographics predict member CU behaviour, so it is not evidence for this pillar.

The larger problems are provenance and construct:
- An input that is edited by hand and has no producer cannot be reproduced.
- Demographic Potential v3.0 is the documented method, but it does not feed Demo%.

**Contradictions**
- The documented Demographic Potential method (v3.0) is not what Demo% reads: column F is hand-edited and the DEMO_POTENTIAL dial does not move it.

**Gaps**
- Validity is untested. Needed: partial correlation of Demo% with CU-aligned final-passage votes and cosponsorship controlling for party, stratified by chamber, with window (Congress) and denominator (members with non-missing column F) stated.
- Swap sensitivity is untested. Needed: rank-delta, Kendall tau and top-133 overlap from replacing column F with Demographic Potential v3.0.
- Provenance is missing. Needed: a reproducible producer, or at least a hash-locked snapshot, for column F and Sub Layer V:AF (engineering, not literature).

**Paul's position vs the evidence**

No evidence either way. Paul's proposed state layer, which became Demographic Potential, is not what the pillar uses, so his position and the live pillar have diverged. No literature in the ledger links district or state industry presence to member CU behaviour in Congress.

**Open questions**
- Should Demo% be rewired to Demographic Potential v3.0, or kept as hand-maintained column F?
- Is a party-tilted input acceptable at 5% if the pillar's validity is never tested?

**Read first** [[huneeus-2018-effects-firms-lobbying]], [[kim-2020-mapping-political-communities]], [[vanderdoes-2022-strategic-identity-signaling]]

## Sub Layer legislative-efficiency formulas and off-by-one repair

**Component id:** `moneyball-rank-pillars.sublayer-efficiency-repair`

**What it computes.** Per-member legislative efficiency from Sub Layer - Raw AG:AW, normalized by Table1 min-max.

**Method.** A 2026-07-15 row shift made every member use the neighbour's record. Repair c6dc6b2 (2026-07-17) repointed 9,061 formulas, fixed AH (Q->P) and AI (AI->X), and anchored on each formula's own Table1 basis column. Independent recompute matches 533/533; 520/533 ranks moved, all legislative-only.

**Code refs.** `fec/repair_sublayer_references.py (--plan/--apply)`

**Assumptions**
- Each formula's Table1 basis names the intended column (the majority pattern was the bug)
- A stale pillar snapshot can shield downstream consumers from an unrepaired reference plane

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

Rows: 0 (0 verified, 0 unverified).

**Assessment**

**GAP by design. This is engineering, not a literature question.**

The 2026-07-15 row shift made every member read a neighbour's record. Repair c6dc6b2 (2026-07-17) repointed 9,061 formulas. An independent recompute matched 533 of 533 members, and 520 of 533 ranks moved, all legislative-only.

The evidence that matters is reproduction, not papers. The internal recompute shows the repaired formulas match their own Table1 basis. It does not show that the efficiency values reproduce from raw congress.gov data.

Three papers bear on what efficiency should mean, though not on the repair itself:
- Sotoudeh 2024: the decayed-ratio definition.
- Bucchianeri 2025: hit rate vs stage-based measures.
- Box-Steffensmeier 2018: the majority confound in advancement.

The repair's own assumption is unverified: that each formula's Table1 basis names the intended column, even though the majority pattern was the bug.

**Contradictions**
- (none)

**Gaps**
- Independent reproduction is missing. Needed: rebuild per-member efficiency from congress.gov billstatus in a separate pipeline (a second path, not the repair script) and compare it member by member with the post-repair Sub Layer AG:AW, reporting mismatches out of 533.
- There is no regression guard. Needed: a check that fails when a formula row offset reappears.

**Paul's position vs the evidence**

Consistent. Paul treats this as an engineering repair with no literature, and that is correct. The open item is an independent reproduction, not a reading.

**Open questions**
- Has the post-repair efficiency been reproduced from raw billstatus by a second path?
- Do the Sub Layer efficiency values still feed the live Leg%, or does live_legislative_pillar.py now supersede them?

**Read first** [[sotoudeh-2024-network-based-measure]], [[bucchianeri-2025-legislative-effectiveness-american]], [[boxsteffensmeier-2018-taking-congress-interest]]

## Cosponsor-network InfluenceScore (computed, weight 0)

**Component id:** `moneyball-rank-pillars.network-centrality-influencescore`

**What it computes.** InfluenceScore = 0.60 x weighted out-degree + 0.25 x betweenness + 0.15 x eigenvector, joined to 494/533 members; the 39 absent fall back to committee%.

**Method.** Computed from the congress_centrality output. It was retired from Influence on 2026-06-25 (AB13 = 0) because it is direction-blind. It still feeds the underdog lens.

**Code refs.** `congress_centrality_*.xlsx; Blended Model AB12/AB13`

**Assumptions**
- Cosponsor centrality reflects influence regardless of stance direction

**Evidence**

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

Rows: 8 (8 verified, 0 unverified).

**Assessment**

**Evidence weight: moderate in favour of the retirement, and none for the composite weights.**

**Against plain centrality as an influence measure:**
- **Sotoudeh 2024 (VERIFIED-SOFTENED, moderate).** On the decayed pass/total matrix (House 2009-2019, largest connected component), eigenvector, closeness and strength barely separate passed from failed bills: relative difference 0.002 to 0.004, against 0.139 / 0.248 for the paper's influence score.
- **Battaglini 2016 (weak).** Unweighted degree and betweenness do not predict PAC money (House committee and alumni networks, 2004-2012).
- **Azaiez 2025 (moderate).** Centrality tracks resources and embeddedness: lobbying FTE predicts hypercoreness with beta 0.464 (European Commission network, N = 1,178 companies). This matches the project finding that the raw all-bills Katz ranking measured activity (Spearman +0.845 with tie volume).

**Context only (Haythornthwaite 1996, Trivedi 2020, Christopher 2016, Ringe n.d.).** These offer taxonomies, simulated orderings or judgment-weighted composites with no outcome test. Christopher's composite is also led by out-degree and is never validated, so it cannot justify 0.60/0.25/0.15.

**No paper tests weighted out-degree or the composite on a cosponsorship graph against passage.**

**FLAG: Sotoudeh's pass/total ratio matrix divides each pair's passed bills by its total.** Adopting it as the system's edge weight would be per-edge volume normalization, which was measured and REJECTED. Use the paper as an outcome-test design only.

Sotoudeh's row also flags a circularity. Weighted out-degree sums ties on a member's own sponsored bills, which are multiplied by 3 when the bill passed. Any passage test therefore needs an as-of network built only from bills resolved before the outcome window.

**Code state.** The Python rank authority defines Influence as committee/max (`compute_pillars.py:181`), so the retirement holds in effect. Two rows searched `fec/` and did not find the AB13 cell or the underdog-lens feed. Those live in the workbook only and remain unconfirmed in code.

Any revived centrality rank is a heuristic model output, never a closeness, access or introduction probability (D-025).

**Contradictions**
- The component record says InfluenceScore still feeds the underdog lens and was retired via AB13 = 0. Code searches in two evidence rows found no AB13 reference and no lens feed under fec/. The retirement is real in compute_pillars.py (Influence = committee/max), but the lens feed exists only in the workbook record.
- A direction-blind score that was retired from Influence is still used as an 'intrinsic influence' input to the underdog lens (see undervaluation-lens).

**Gaps**
- No as-of passage test of the system's own composite exists. Needed: build the cosponsor network from bills resolved before window t, then test whether weighted out-degree, betweenness and eigenvector predict passage of each member's sponsored bills in window t+1 beyond committee position, stratified by chamber and majority status. This must not use the passage-tripled weights.
- Paul's requested Viral Centrality test (Fink) has no evidence row. Fink's author-volume normalization is the REJECTED per-edge volume normalization and must not come with it.

**Paul's position vs the evidence**

Supported. Paul's note from Sotoudeh that eigenvector centrality is not a strong indicator of passage matches the verified numbers, and his emphasis on directionality (Haythornthwaite) is consistent with the retirement. His call for more research on forms of centrality is fair: no row tests the composite. The caution is that two of his cited sources carry proposals this project rejected: Sotoudeh's ratio edges and Fink's author-volume normalization, both per-edge volume normalization. FLAG both; neither should be imported.

**Open questions**
- Should the underdog lens stop using centrality, given the retirement rationale?
- Can the AB13 retirement and the lens feed be located in code, or are they workbook-only?
- Is an as-of passage test worth a single look, or is the retirement final?

**Read first** [[sotoudeh-2024-network-based-measure]], [[battaglini-2016-influencing-connected-legislators]], [[azaiez-2025-hypergraph-analysis-european]], [[ringe-nd-pinpointing-powerful-voting]], [[haythornthwaite-1996-social-network-analysis]], [[christopher-2016-mathematical-modeling-analysis]]

## Undervaluation (underdog) lens

**Component id:** `moneyball-rank-pillars.undervaluation-lens`

**What it computes.** Underdog rank = Intrinsic - Visibility, where Intrinsic = 0.5 Leg% + 0.5 Centrality% and Visibility = 0.5 Press% + 0.5 Committee%.

**Method.** Difference of two equal-weight composites, ranked. The brain note gives no code file.

**Code refs.** (none)

**Assumptions**
- Legislative activity and centrality measure intrinsic influence; press and committee measure visibility

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | qualifies | Third-coalition members are usually ideologically aligned with the majority party. The paper attributes the majority party's effectiveness to agenda-setting power, and says a substantial portion of the apparent majority advantage comes from the third coalition. Third-coalition effectiveness drops temporarily during majority transitions and rests on a few ultra-effective members. | The paper defines its 'hidden' bloc by signed-tie structure, not by low press or committee visibility, and its effectiveness tracks majority alignment. That is closer to what the lens subtracts (committee position) than to what it rewards. The link is an analogy and is not evidence for the lens's formula. | weak | VERIFIED-SOFTENED |
| [[amsalem-2018-effect-politicians-personality]] | 2018 | qualifies | Holding office is the strongest predictor of visibility: minister beta .369 (b 3.122), about three times the trait betas. Party leader beta .195 is only slightly stronger than the personality effects. Office here means minister, party leader and house speaker; committee position is not tested. | The paper's evidence is MPs in three parliamentary systems, not US House members. Its visibility construct is press mentions, which Press% does not capture. Committee% is a visibility input the paper never tests. Any reading of personality as part of the underdog residual is an inference, not a finding. | weak | VERIFIED-SOFTENED |

Rows: 2 (2 verified, 0 unverified).

**Assessment**

**Evidence weight: weak. The one conceptual anchor points the other way.**

- **Aref & Neal 2021 (VERIFIED-SOFTENED, weak).** The paper defines its hidden third coalition by signed-tie structure, not by low press or committee visibility.
  - The bloc's effectiveness tracks alignment with the majority party and agenda-setting power.
  - It drops during majority transitions and rests on a few ultra-effective members.
  - That is closer to what the lens subtracts (committee position, a majority-linked asset) than to what it rewards.
- **Amsalem 2018 (weak).** Office is the strongest predictor of media visibility (minister beta .369). The study covers MPs in three parliamentary systems and never tests committee position. It does not support treating Committee% as visibility, and its visibility construct (press mentions) is not what Press% measures.

**Problems in the lens's internal construction:**
- Intrinsic includes Centrality%, which the system retired from Influence as direction-blind.
- Visibility includes Committee%, which is the system's main structural-power pillar at 40%.
- No code file exists.

The underdog rank is a heuristic model output, not evidence that a member is under-priced or accessible (D-025).

**Contradictions**
- Aref & Neal's hidden bloc is effective through majority alignment and agenda-setting. The lens treats committee position, a majority-linked structural asset, as mere visibility and subtracts it.
- The lens's Intrinsic composite uses cosponsor centrality, the same direction-blind measure the system retired from Influence on 2026-06-25.
- Committee% counts as structural power in the main rank (40% weight) but as visibility in the lens. One input cannot be both without a stated rationale.

**Gaps**
- The lens has never been tested. Needed: test whether high-underdog members show higher realized CU legislative effectiveness (LES or CU-bill advancement) in the following Congress than members with similar visibility. Use an as-of design, chamber strata, and name the denominator (members with non-missing inputs).
- No code exists. Needed: a producer script so that the lens can be reproduced at all.

**Paul's position vs the evidence**

Not supported as stated. Paul reads Aref & Neal's bloc as the conceptual twin of the lens and as the responsive or vulnerable seats. The verified reading of the paper says the bloc is defined by signed ties and gains effectiveness from majority alignment, so it is an analogy at best and partly inverted. No evidence row supports reading the bloc as vulnerable seats.

**Open questions**
- Should Centrality% be removed from Intrinsic, or replaced with a stance-gated measure?
- Should Committee% count as visibility or as intrinsic power?
- Should the lens be retired or rebuilt with a code producer?

**Read first** [[aref-2021-identifying-hidden-coalitions]], [[amsalem-2018-effect-politicians-personality]], [[sotoudeh-2024-network-based-measure]], [[azaiez-2025-hypergraph-analysis-european]]

## Legacy Sub Layer -> Primary Layer -> Rank & Allocation cascade

**Component id:** `moneyball-rank-pillars.legacy-workbook-cascade`

**What it computes.** Legacy 11-dimension SUMPRODUCT score and rank (Rank & Allocation cols K/L/M).

**Method.** This cascade is orphaned in the Blended version but not retired. Its rankings disagree with Blended. Primary Layer - Normalized H2 = 0.80 (likely meant to be 0.08), so the weight row sums to 1.72.

**Code refs.** `Rank & Allocation cols C-M; Primary Layer - Normalized row 2`

**Assumptions**
- Superseded design; recorded for lineage only

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

Rows: 0 (0 verified, 0 unverified).

**Assessment**

**GAP: no evidence. The component is kept for lineage.**

The legacy 11-dimension SUMPRODUCT cascade is orphaned in the Blended workbook but not retired, and its ranks disagree with the Blended ranks.

It also has a known defect: Primary Layer - Normalized H2 = 0.80, likely meant to be 0.08, so the weight row sums to 1.72. Any comparison against it inherits that defect.

No paper bears on it. Its only use would be a horse-race against the blended rank, and that is worth running only if Paul wants to retire it on evidence rather than by fiat.

**Contradictions**
- The weight row sums to 1.72 because H2 = 0.80 (likely 0.08), so the legacy ranks are computed on a malformed weight vector.
- The legacy and Blended ranks disagree, and the legacy cascade is not marked retired.

**Gaps**
- Needed only if retirement on evidence is wanted: a single-look comparison of legacy vs blended rank against CU-aligned final-passage votes in the 119th Congress, chamber-stratified, with H2 corrected first and the correction labelled.

**Paul's position vs the evidence**

Consistent. Paul cites no literature and the cascade is superseded. The open question is administrative: whether to retire it formally.

**Open questions**
- Should the legacy cascade be formally retired?
- Is H2 = 0.80 a typo for 0.08?

**Read first** [[christopher-2016-mathematical-modeling-analysis]], [[becker-nd-gameability-redistricting-criteria]], [[bucchianeri-2025-legislative-effectiveness-american]]

## Across components

## Patterns across the components

**1. No number in the system is literature-validated.** That covers the pillar weights, committee tiers, the leadership ladder, the 1/6 factor, the 4:2:1 role weights, the press tiers and the centrality weights. They are all owner policy. The verified evidence supports only qualitative premises:
- jurisdiction attracts money (Kim);
- decayed success ratios carry outcome information (Sotoudeh, UNSTRATIFIED);
- stage-based effectiveness is a construct (Bucchianeri, North Carolina);
- plain centrality is weak (Sotoudeh, Battaglini, Azaiez).

Every pillar should be labelled owner policy until an outcome test exists.

**2. Chamber pooling and majority status recur across components.** Bucchianeri 2025 bears on three components: score-blend, committee and legislative. It says effectiveness is chamber-term relative, the chair effect differs by chamber (0.609 lower vs 0.307 upper) and majority status adds 0.370 (strata 0.362 / 0.387). Box-Steffensmeier adds a majority-sponsor passage coefficient of 1.353 for the House over the 106th-111th Congress.

The system pools House and Senate in one percentile (Leg) and one raw/max (Influence), and has no majority term. All of this evidence is state-level or pooled, so it is a design caution, not a refutation. But it is the single most repeated pushback in the ledger.

**3. The normalization differs by pillar.** Leg and Demo use PERCENTRANK, Influence uses raw/max and Press uses split signed extrema. No row addresses whether this makes the effective variance shares diverge from the nominal weights. That is the same failure that killed min-max at 06-25 (Press 40 / Leg 6 / Committee 52 / Demo 2).

**4. Retired measures are still used in the lens.** Centrality is retired from Influence as direction-blind but is still treated as intrinsic influence in the underdog lens. Committee% is structural power in the rank but visibility in the lens. Neither has code-confirmed wiring.

**5. FLAGS on rejected proposals.** Sotoudeh's pass/total edge ratio and Fink 2023's author-volume normalized edges are both per-edge volume normalization, which was measured and REJECTED. They are cited in the system and by Paul for the centrality component. Use them as outcome-test designs only, never as edge weights.

No row argues for dropping regular cosponsorship edges, and the legislative pillar's 0.0714 cosponsor weight should stay.

Kim & Kunisky's lobbying communities are built from LDA. Per D-021 they may inform identity and jurisdiction context, never edges or weights.

**6. Transfer discipline.** The evidence leans on non-congressional settings:
- Bucchianeri and Fouirnaies & Hall: state legislatures.
- Amsalem: parliamentary MPs.
- Azaiez: the European Commission lobby network.
- Thakur: Senate only, with garbled tables.
- Kim: manufacturing firms only.

Two leads are UNVERIFIED and must not be cited as findings (DE-062): the Frisch & Kelly committee-request codebook and the strategic identity-signaling experiment.

**7. Paul's positions are mostly well calibrated.** The exceptions are the underdog lens, where the evidence inverts his reading of Aref & Neal, and the press pillar, where his augmentation agenda has no evidence and the one verified row cautions against the tone-intensity product.

## Research agenda

1. Resolve the weight-set record: 45/10/40/5 in code vs 25/30/40/5 in the spec vs the ratified 30/25/40/5. Then run Christopher-style one-weight sweeps reporting Kendall tau and top-133 overlap between the sets, with the window (the snapshot date of the 533-member roster) named.
2. Measure the effective variance shares of the four pillars under the current mixed normalization (PERCENTRANK / raw-max / split signed extrema) and compare them with the nominal weights.
3. Run a pre-registered single-look validity test of the blended rank vs each single pillar against CEL LES (119th Congress) and CU-aligned final-passage votes, stratified by chamber and majority status. Re-verify the brain's +0.10 rank-LES figure first.
4. Chamber pooling: test whether percent-ranking or max-normalizing within chamber changes ranks materially (Bucchianeri bearing). Report rank deltas out of 533.
5. Committee relevance from data: compute each committee's lagged share of CU-category bills referred, from congress.gov billstatus (not LDA, D-021), and compare it with the owner tiers. Check the 74/74 Highly tier on Small Business. Any re-weighting needs its own look.
6. Committee selection vs leverage: an event study of members' CU activity before and after new assignments. Verify the Frisch & Kelly codebook by a second path before any use.
7. Legislative role weights and majority confound: estimate role-specific effects (sponsor, original cosponsor, cosponsor) on CU-bill advancement, and test whether efficiency adds validity beyond agenda once majority status is stratified.
8. Press: compare sign-only vs sign x LSD intensity, and with vs without the own-domain filter, against later CU-aligned votes (as-of design). Test the 1/n normalization by correlating speaker volume with alignment. Do not reframe it as per-edge volume normalization.
9. Centrality: if revisited, run an as-of passage test with a network built only from bills resolved before the outcome window, without the passage-tripled weights, beyond committee position. Do not adopt Sotoudeh ratio edges or Fink author-volume edges (REJECTED per-edge volume normalization).
10. Underdog lens: give it a code producer, remove or replace direction-blind Centrality% in Intrinsic, decide Committee%'s role, then test it against next-Congress CU effectiveness. Report it as a heuristic model output (D-025).
11. Demographic pillar: give column F a reproducible producer or a hash-locked snapshot, test the partial correlation of Demo% with CU-aligned votes controlling party (chamber strata), and measure the rank effect of swapping in Demographic Potential v3.0.
12. Sub Layer repair: reproduce post-repair efficiency independently from raw billstatus by a second pipeline, and add a row-offset regression guard.
13. Legacy cascade: decide whether to retire it formally. If evidence is wanted, correct H2 (0.80 to 0.08, labelled) and run one horse-race against the blended rank on 119th-Congress votes.
