---
type: system
name: "Moneyball four-pillar influence rank"
slug: moneyball-rank-pillars
components: [moneyball-rank-pillars.score-blend, moneyball-rank-pillars.committee-pillar, moneyball-rank-pillars.legislative-pillar, moneyball-rank-pillars.press-pillar, moneyball-rank-pillars.demographic-pillar, moneyball-rank-pillars.sublayer-efficiency-repair, moneyball-rank-pillars.network-centrality-influencescore, moneyball-rank-pillars.undervaluation-lens, moneyball-rank-pillars.legacy-workbook-cascade]
brain_notes: [concepts/four-pillar-percentile-blend, concepts/influence-vs-alignment-axes, actors/moneyball-workbook, actors/moneyball-workbook-rules-scoring-and-sublayer-surgery, actors/moneyball-workbook-lineage-and-june-audit, topics/2026-07-rfc-melange-architecture-system-of-record-model-config-and-committee-formula]
paper_count: 24
tags: [system]
---

# Moneyball four-pillar influence rank

The official 533-member CU-influence rank. It blends Legislative, Press, Influence (committee) and Demographic pillars into a score; ranks_python.json (fec/engine/compute_pillars.py) is the rank authority and the workbook is a view of it. Includes each pillar's construction, the legacy workbook cascade and the underdog lens. Code paths are relative to C:/Users/pgoff/OneDrive/Desktop/Melange Systems.

Brain notes: [concepts/four-pillar-percentile-blend](obsidian://open?vault=brain&file=concepts/four-pillar-percentile-blend); [concepts/influence-vs-alignment-axes](obsidian://open?vault=brain&file=concepts/influence-vs-alignment-axes); [actors/moneyball-workbook](obsidian://open?vault=brain&file=actors/moneyball-workbook); [actors/moneyball-workbook-rules-scoring-and-sublayer-surgery](obsidian://open?vault=brain&file=actors/moneyball-workbook-rules-scoring-and-sublayer-surgery); [actors/moneyball-workbook-lineage-and-june-audit](obsidian://open?vault=brain&file=actors/moneyball-workbook-lineage-and-june-audit); [topics/2026-07-rfc-melange-architecture-system-of-record-model-config-and-committee-formula](obsidian://open?vault=brain&file=topics/2026-07-rfc-melange-architecture-system-of-record-model-config-and-committee-formula)

Evidence census (computed from the hub file): 30 rows, 24 distinct papers; UNVERIFIED 2, VERIFIED 8, VERIFIED-SOFTENED 20.

**System:** Moneyball four-pillar influence rank. Score = w·Leg% + w·Press% + w·Influence% + w·Demo% over 533 members. `ranks_python.json` is the rank authority.

**How much evidence there is.** The hub has 30 bearing rows. 28 are VERIFIED or VERIFIED-SOFTENED. Two are UNVERIFIED leads: van der Does 2022 on press, and the House Committee Requests codebook on committee. Almost no verified row tests a pillar against an outcome. The papers back premises, such as "lobbying attention is organised by committee jurisdiction" and "direction-blind centrality does not separate passed from failed bills." They do not back the numbers, such as the pillar weights, the tier weights, the 1/6 subcommittee factor, the leadership ladder, the 4:2:1 role weights or the sign×LSD product. Most of the strong-looking support comes from settings that do not transfer directly:
- state legislatures (Bucchianeri on North Carolina, Fouirnaies & Hall);
- firms (Kim et al., Huneeus & Kim, Azaiez);
- other countries' parliaments (Amsalem, Haselmayer);
- simulated cascades (Kempe, Trivedi, Kumar).

**Bottom line.** The rank is defensible as owner policy built on well-motivated premises. It has **no external validity evidence** at the composite level. The single federal convergence figure is ρ = +0.10 against CEL LES. It is recorded only in the brain and is unverified in this pass. If real, it falls in the null band of the system's own bar B (lep_criterion.py:17-19).

**Errors to fix (not literature questions):**
1. The pillar weights disagree across three sources:
   - `pillar_policy.py:11-16` has 45/10/40/5 (live, unratified, S-5 open).
   - `MATHEMATICAL_SPECIFICATION.md:12` has 25 Leg / 30 Press / 40 / 5.
   - The ratified record says 30/25/40/5.
2. The press pillar is described as "signed raw/max". The verified code path (`normalize_signed_extrema`) divides positive values by the largest positive and negative values by the absolute largest negative. Those are two different scales, not one raw/max.
3. In the legacy cascade, Primary Layer H2 = 0.80, which is probably meant to be 0.08. The weight row sums to 1.72, and the cascade is orphaned but not retired.
4. Two verified rows (Sotoudeh, Battaglini) searched `fec/` and could not find the AB13 = 0 centrality retirement or the underdog-lens use of centrality. Both exist only in the workbook, and the lens has no code file.

**Online scan.** The scan was light. Its leads are marked as leads in the components and the agenda; none is verified. They are:
- the composite-indicator sensitivity literature (Saisana, Saltelli & Tarantola 2005; Paruolo, Saisana & Saltelli 2013; OECD-JRC Handbook 2008);
- Kessler & Krehbiel 1996 on cosponsorship timing;
- CEL's congressional LES benchmarks;
- Grier & Munger 1991 and Kroszner & Stratmann 1998 on committee self-selection;
- district financial-sector employment and TARP votes, where the evidence is mixed and comes from a student journal and a working paper.

## Four-pillar score blend and rank

**What it computes:** Score = w2*Leg% + w3*Press% + w4*Influence% + w5*Demo%; Rank = descending competition rank over 533 members. ranks_python.json is the rank authority.

**Method:** Linear weighted sum of normalized pillars. Normalization differs by pillar: legislative uses Excel PERCENTRANK.INC truncated to 5 decimals, committee uses raw/max, press uses signed raw/max, and demographic uses PERCENTRANK. Weight history: .40/.30/.20/.10 (2026-06-29, historical); 30/25/40/5 (ratified 2026-07-07); 45/10/40/5 (Leg/Press/Inf/Demo), live in fec/engine/pillar_policy.py since 2026-09-03 and unratified (code-review S-5 open). VERIFIED: pillar_policy.py:11-16 reads leg .45, press .10, inf .40, demo .05. Its docstring says 10 of the original 25 press points remain and 15 moved to legislative, which implies the ratified set was Leg .30 / Press .25. MATHEMATICAL_SPECIFICATION.md:12 instead writes 0.25*Leg + 0.30*Press, and brain notes disagree on the order. Unresolved.

**Code refs:** `fec/engine/compute_pillars.py:11-14,36-59,195-216`, `fec/engine/pillar_policy.py:11-16`, `fec/engine/apply_weights.py:11-13`, `MATHEMATICAL_SPECIFICATION.md:12`, `ranks_python.json`

**Assumptions:**
- Pillar values after normalization are commensurable, so fixed weights are meaningful
- Weights are owner policy, not estimated from outcomes
- Rank measures influence; stance (alignment) is a separate axis
- Min-max was rejected because one outlier set each pillar's scale (effective variance shares at 06-25: Press 40 / Leg 6 / Committee 52 / Demo 2)

**Evidence (3 rows, 3 distinct papers: VERIFIED-SOFTENED 3)**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[christopher-2016-mathematical-modeling-analysis]] | 2016 | method-source | Criterion weights come from AHP pairwise comparison on Saaty's nine-point scale, accepted at CR below 0.10 (here 0.062). Robustness follows Alinezhad and Amini Equation 1: change one weight in 0.1 steps, rescale the others proportionally, and track rank. Raising the out-degree weight keeps Freedom Partners Chamber of Commerce first. Raising the weight the paper labels lowest (closeness, Figure 5) lets Center to Protect Patients Rights overtake it. | The one-weight proportional-rescale sweep can be used directly for the open top-133 sensitivity question. AHP would only document owner judgment and check its internal coherence; it gives no outcome validity, and the paper's own weights are unvalidated. Any AHP-derived set must be labelled owner policy with its CR, never an estimate. | weak | VERIFIED-SOFTENED |
| [[bucchianeri-2025-legislative-effectiveness-american]] | 2025 | qualifies | Stage-based effectiveness is a valid construct: SLES explains about half the variation in NC expert rankings, and lagged SLES persists (rho 0.38 when majority entered, 0.40 when minority entered, 0.66 with no change in control). Effectiveness reflects innate or cultivated lawmaking ability as well as position. | The persistence and NC validity figures are from the state paper and do not validate the federal blend. The orthogonality reading depends on CEL LES being the federal analogue. Under bar B, +0.10 falls in the null band, which matches the rel's wording, but the +0.10 value itself was not verified. | moderate | VERIFIED-SOFTENED |
| [[becker-nd-gameability-redistricting-criteria]] |  | context | When a criterion admits several defensible metrics, each metric favors a different plan, so stakeholders can cherry-pick the metric their plan wins on. The paper recommends that states clarify metric definitions rather than set hard numeric thresholds. | Analogy only. The paper studies redistricting metrics, not legislator rankings, and provides no evidence about these weights. It supports pinning one documented weight set and reporting rank sensitivity across the competing sets; it does not justify any particular weights. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: thin and indirect.** Three verified rows bear on this component, and none tests the blend against an outcome.
- **Bucchianeri 2025** (VERIFIED-SOFTENED, moderate) shows that stage-based effectiveness is a valid, persistent construct in North Carolina. In 2005-2012, SLES explains about half the variation in expert rankings, with lagged ρ of 0.38 to 0.66. That is a state result and does not validate the federal blend. The federal analogue is ρ = +0.10 between the blend and CEL LES. It is recorded only in the brain and was not verified here. Under bar B (lep_criterion.py:17-19) it would fall in the null band, below 0.15.
- **Christopher 2016** (VERIFIED-SOFTENED, method-source) supplies a usable robustness recipe: change one weight in 0.1 steps, rescale the others proportionally and track rank. Its AHP weights are themselves unvalidated, so AHP could only document owner judgment, reported with its consistency ratio (CR).
- **Becker** (context) is an analogy from redistricting metrics. When metrics are contestable, the advice is to pin one documented definition and report sensitivity.

**Online scan, UNVERIFIED leads.** Saisana, Saltelli & Tarantola (2005, JRSS A) propagate normalisation, weighting and aggregation choices by Monte Carlo and decompose rank variance with Sobol indices. Paruolo, Saisana & Saltelli (2013, JRSS A) find that nominal weights rarely match effective importance (main effects) when the inputs are correlated and heteroscedastic. This is the published form of the system's own 06-25 finding that min-max gave effective variance shares of Press 40 / Leg 6 / Committee 52 / Demo 2 against nominal 40/30/20/10.

**Why it matters here.** Several sources point to majority status and chair position as drivers of legislative effectiveness:
- Bucchianeri reports +0.370 for majority status (pooled, UNSTRATIFIED; strata 0.362 lower and 0.387 upper chamber) and +0.507 for chairs (pooled; strata 0.609 and 0.307).
- Box-Steffensmeier reports a majority-sponsor logit coefficient of 1.353 (House, 106th-111th Congress, N = 6,597 bills).

The same positional advantage therefore probably loads on both Leg% (through advancement) and Influence% (through chairs). The *effective* weight of position is then likely above the nominal 40%. That is an inference to test, not a finding.

**Actual error.** The weight set is inconsistent across code, spec and ratification record (see the contradictions).

**Contradictions**
- Weight set: pillar_policy.py:11-16 = Leg .45 / Press .10 / Inf .40 / Demo .05 (live, unratified, S-5 open); MATHEMATICAL_SPECIFICATION.md:12 = .25 Leg / .30 Press / .40 / .05; ratified 2026-07-07 record = 30/25/40/5 (and the pillar_policy docstring arithmetic implies Leg .30 / Press .25). At most one of these can be right; the spec line is wrong against both the code and the docstring.
- Normalisation is not uniform across pillars (PERCENTRANK for Leg and Demo, raw/max for committee, signed extrema for press), so the 'commensurable pillars' assumption is unmet by construction: a percentile pillar is uniform on [0,1] while a raw/max pillar is skewed toward 0 (245/533 members have committee score 0). Nominal weights therefore cannot equal effective shares (Paruolo et al. 2013 lead).
- The only federal convergence figure (rho +0.10 vs CEL LES, brain-only, unverified) sits in the null band of the system's own bar B, while the state-level analogue (Bucchianeri, NC) is strong; the system's 'rank measures influence' assumption is not supported at the federal level by anything verified.

**Gaps**
- No study tests the blended rank, or any pillar, against an external federal influence criterion (CEL LES for the 119th Congress, CU-bill advancement, CU-aligned final-passage votes). Fill with a preregistered single-look criterion study: Spearman between rank and LES, reported by chamber and by majority/minority (UNSTRATIFIED if pooled), with each pillar alone as a comparator.
- No sensitivity analysis exists for top-133 membership across the 30/25/40/5 and 45/10/40/5 sets. Fill with Christopher's one-weight proportional-rescale sweep plus a Saisana-style Monte Carlo over weights and normalisation; report Kendall tau-b and top-133 overlap (window: current roster, denominator 533).
- No measurement of effective vs nominal pillar importance under the current mixed normalisation (Paruolo main-effect / correlation-ratio check).

**Paul's position vs the evidence**

Paul's position is that no literature underwrites the weights and that they are owner policy. The evidence agrees, and that is the honest framing. His reading favours measuring effectiveness, citing Aref & Neal and Sotoudeh against LES. The only federal figure points the other way: the blend is roughly orthogonal to CEL LES (+0.10, unverified). So either the rank measures something other than effectiveness, or the pillars need re-weighting. Paul should choose which claim the rank makes before the weights are ratified, and S-5 should be closed against one documented weight set.

**Open questions**
- Which weight set is ratified: 30/25/40/5 or 45/10/40/5? (S-5, Paul's ruling.)
- Is the +0.10 rho vs CEL LES real? Which window and n, and is it stratified by chamber?
- Should the rank claim to measure 'effectiveness' (testable against LES) or 'CU-specific leverage' (needs a CU outcome criterion)?
- How much of Leg% and Influence% is shared majority/chair variance?

**Read first:** [[bucchianeri-2025-legislative-effectiveness-american]], [[christopher-2016-mathematical-modeling-analysis]], [[becker-nd-gameability-redistricting-criteria]], [[boxsteffensmeier-2018-taking-congress-interest]]

## Committee - Influence pillar

**What it computes:** Influence% = committee_raw / MAX(committee_raw), where committee_raw = (Membership + Leadership)/2 from seats held.

**Method:** Seat weight by CU-relevance tier: Highly 3.0, Moderately 1.8, Slightly 0.9; subcommittee seats (names containing ' - ') weigh 1/6. Leadership = seat weight x multiplier: Chair 3.0 full / 7.5 sub, Ranking 2.0 / 4.5, Vice 1.0 / 2.0 (Paul applied chair/ranking x1.5 by hand on 2026-06-30). Replaced a double-PERCENTRANK version (CMTEFIX). Centrality was retired from Influence on 2026-06-25 (AB13 = 0) as direction-blind. The formal-office party-elite bonus is display-only (set to 0 at compute_pillars.py:163). Judiciary was added at tier Slightly.

**Code refs:** `fec/engine/committee_scores.py:1-185`, `fec/engine/apply_committee_fix.py:3-18`, `fec/engine/apply_committee_reweight.py:15-37`, `fec/engine/add_judiciary_seats.py:3-27`, `fec/engine/influence_roles.py:11-30`

**Assumptions:**
- Committee seat relevance and leadership measure structural CU power
- Subcommittee seats are worth 1/6 of a full seat
- Tier labels and multipliers are owner judgment, not estimated

**Evidence (10 rows, 8 distinct papers: UNVERIFIED 1, VERIFIED 4, VERIFIED-SOFTENED 5)**

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

**Assessment**

**Evidence weight: moderate for the premise, none for the parameters.**

**Support for the premise that committee jurisdiction is where interest-group attention and money concentrate:**
- Kim 2020 'Strategic' (VERIFIED, moderate);
- Kim 2023 'Systemic' (VERIFIED, weak; manufacturing firms, federal, 2008-2018);
- Kim 2020 'Mapping' (VERIFIED-SOFTENED; 113th Congress, descriptive; Finance & Insurance community concentrated in House Financial Services).

All three are about *where lobbying goes*. None shows that seats *produce* CU outcomes. Kim's power-committee covariate is Groseclose-Stewart (Appropriations, Armed Services, Finance, Rules), not Financial Services or Banking. Kim's committee channel also has a higher base rate, so 'stronger' partly tracks breadth.

**Bearing on the parameters:**
- **Bucchianeri 2025** (VERIFIED-SOFTENED, moderate; NC state legislature) puts chairs 0.507 above rank-and-file. That figure is pooled; the strata are 0.609 lower chamber and 0.307 upper. The chair gap varies with chamber size and with appointment rules. This undercuts using identical multipliers for the House and Senate pooled under one max. It does not estimate the multipliers.
- **Online scan, UNVERIFIED.** CEL's 117th Congress House averages are 0.58 for the minority, 1.40 for the majority and 2.55 for chairs. Senate averages are 0.77, 1.23 and 1.51. The direction matches a chair premium, and the size differs by chamber.
- **Porter 2005** (two rows, weak). Subcommittee nesting is shown for one parent committee (Intelligence, 107th Congress), so it cannot justify or refute additive 1/6 counting. Committees vary in partisan extremism, which is descriptive.
- **Thakur 2025** (two rows; Senate 104th-114th, suggestive). Seat assignment differs by party mechanism. Finance and Appropriations carry high common value; constituency committees such as Small Business carry lower common value. Small Business seats are tiered Highly (74 of 74). That is a validity caveat, not a re-tiering input; the Thakur A7/A8 numbers are garbled.
- **Huneeus & Kim 2018** (VERIFIED, extension). Committee relevance can be *measured* as the lagged share of relevant bills referred to each committee (first-stage F 14.30, all years). Ported here, that means CU-category referral shares from congress.gov billstatus, not LDA (D-021), as a sensitivity check that needs its own look.
- **House Committee Requests codebook** (UNVERIFIED lead; unread, 0 characters extracted). This would be the selection test: do members request CU-relevant seats because of prior interest?

**Contradictions**
- Pooled cross-chamber normalisation (committee_raw / one max across House and Senate, identical multipliers) vs Bucchianeri's strata (chair premium 0.609 lower vs 0.307 upper chamber, NC) and CEL's federal averages (unverified lead: chairs 2.55 House vs 1.51 Senate). The evidence says the chair premium differs by chamber; the system assumes it does not.
- Small Business seats tiered Highly (74/74) vs Thakur's reading of Small Business as a low-common-value constituency committee (Senate, Republican, 1995-2017; text-verified only). Not a refutation of CU relevance, but a prestige mismatch.

**Gaps**
- No study links committee score to CU-bill markup or advancement for bills referred to those committees. Fill with an as-of panel: CU-category bills referred, 119th Congress, outcome = reported/advanced, predictor = committee score of the referred committee's members, stratified by chamber and majority status.
- Leadership ladder (Chair 3.0/7.5, Ranking 2.0/4.5, Vice 1.0/2.0) and the 1/6 subcommittee factor have no empirical source. Fill by estimating chair/ranking/subcommittee-chair premiums on federal LES or CU advancement, by chamber.
- Selection vs conferred leverage is untested. Fill with House committee request data (once the codebook is actually read) or a Grier & Munger 1991 / Kroszner & Stratmann 1998-style test (online-scan leads, UNVERIFIED) of whether financial-sector money and interest precede assignment.
- 245/533 members have no committee row (score 0). No study checks whether they differ in CU outcomes beyond the zero.

**Paul's position vs the evidence**

Paul's position is that committees are the structural layer and that money and lobbying effects concentrate in committee activity. The verified Kim rows and Kim's mapping paper support this as a statement about *attention*. They do not support it as a statement about *outcomes*. His chicken-or-egg concern about partisanship versus committee selection is well placed. Porter's extremism heterogeneity and Thakur's party-specific assignment mechanisms both bear on it, and no evidence resolves it. Paul cited Huneeus & Kim for the committee-assignment instrument. That paper actually offers a data-derived relevance weight (referral shares), which could replace or check the hand tiers.

**Open questions**
- Should the House and Senate be normalised separately, or should multipliers differ by chamber?
- Should tiers be checked against CU-category referral shares from billstatus? Is that Paul's call, and does it need its own look?
- Is Small Business correctly tiered Highly for CU relevance, given that the override forcing it was removed?
- Should Judiciary at Slightly and tax-writing or appropriations seats be re-examined, given Kim's high-entropy power-committee finding?

**Read first:** [[kim-2020-strategic-sequential-links]], [[huneeus-2018-effects-firms-lobbying]], [[bucchianeri-2025-legislative-effectiveness-american]], [[kim-2020-mapping-political-communities]], [[thakur-2025-matching-theory-perspective]], [[porter-2005-network-analysis-committees]]

## Legislative pillar (stance-gated agenda plus efficiency)

**What it computes:** Leg% = percentile of a blend of agenda% and efficiency% (0.7/0.3 at 2026-06-25; live blend read from workbook AB10/AB11). Agenda = sum over topics of IMP[topic] x activity x stanceMult, so opponents go negative.

**Method:** Activity weights by role: sponsor 0.286, original cosponsor 0.143, cosponsor 0.0714 (letters count at original-cosponsor weight). stanceMult is signed from category_stance.json. Efficiency per category = decay-weighted sign of advanced sponsored or original-cosponsored bills over total decay weight; advancement is tri-state and unknown outcomes are excluded. The pillar stays PERCENTRANK because raw/max (tried 2026-07-01) squashed the mid-board. The live routing implementation is fec/engine/live_legislative_pillar.py.

**Code refs:** `fec/apply_leg_stance.py`, `fec/apply_legstance_undo.py`, `fec/engine/live_legislative_pillar.py:17-124`

**Assumptions:**
- Sponsorship and cosponsorship in a CU topic reveal the direction of advocacy once signed by stance
- Advancement past introduction measures legislative efficacy
- category_stance.json is current when the step runs (known staleness trap)

**Evidence (4 rows, 4 distinct papers: VERIFIED 2, VERIFIED-SOFTENED 2)**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[fouirnaies-2022-electoral-incentives-affect]] | 2022 | qualifies | Bill sponsorship volume falls about 6% when reelection incentives end (-1.63, SE 0.77, mean 27.32; N=16,727) with no ideological change. Sponsorship volume partly reflects electoral advertising (legislators' webpages list bills introduced) rather than position. | Weak. The 6% effect at the state level is small, and in Congress it would touch only members in their final Congress or those running for other office. The paper supports the influence-vs-alignment split. It does not address the 4:2:1 role weights or cosponsorship. | weak | VERIFIED-SOFTENED |
| [[sotoudeh-2024-network-based-measure]] | 2024 | supports | A decayed pass/total success ratio is informative about outcomes. Passed bills carry higher cosponsor pass-ratio influence than failed bills in every 4-month window (relative difference 0.139 mean / 0.248 max at a 6-month half-life). The authors contrast their approach with LES, which has no decay and splits the process into finer stages. | Indirect support only. The paper's ratio is pairwise, aggregated to bills, and includes each bill's own outcome. The pillar's ratio is member-level and describes CU-topic advancement; the paper does not show that a member's success rate measures CU influence or predicts future behaviour. The paper pools across parties and 2009-2019 with no strata, so label UNSTRATIFIED. The authors report a decline from about 2015 and hypothesize it is correlated with polarization. | weak | VERIFIED |
| [[boxsteffensmeier-2018-taking-congress-interest]] | 2018 | qualifies | Passed House logit (Table 4, N = 6,597): hot bill 2.428 (SE .161), commemorative 1.386 (SE .150), majority-party sponsor 1.353 (SE .220), endorser count 0.013 (SE .004), cosponsors 0.004 (SE .001). Endorser social power is -0.035 (SE .032, ns) for passage and 0.020 (SE .031, ns) for reporting. | The paper estimates a covariate association over letter-bearing House bills pooled across 106th-111th Congresses. It is not a per-member efficiency model and says nothing about efficiency metrics. Reading the majority coefficient as showing that a raw advancement rate rewards majority status is an inference, not the paper's claim, and any adjustment is Paul's call. | moderate | VERIFIED-SOFTENED |
| [[bucchianeri-2025-legislative-effectiveness-american]] | 2025 | qualifies | In North Carolina 2005-2012 SLES outperforms hit-rate metrics against subjective rankings. Scores are relative to each chamber-term and should not be compared directly across chambers or eras. Majority status adds 0.370 in the pooled full-sample estimate (strata: 0.362 lower chamber, 0.387 upper), 58 percent above the minority mean of 0.636. | The NC criterion covers one state chamber pair over eight years; transfer to Congress is an assumption. The 0.370 figure is an unstratified pooled estimate and must carry strata or an UNSTRATIFIED label, which the rel does. | moderate | VERIFIED |

**Assessment**

**Evidence weight: weak to moderate, with every row qualifying the pillar rather than supporting it.**

- **Bucchianeri 2025** (VERIFIED, moderate) is the most relevant row and it is a caution. In North Carolina 2005-2012, the stage-based SLES *outperforms hit-rate metrics* against expert rankings. The pillar's efficiency term is a decay-weighted hit rate. Scores are relative to each chamber-term, but the system puts the House and Senate in one percentile. Transfer to Congress is assumed.
- **Box-Steffensmeier 2018** (VERIFIED-SOFTENED, moderate; House, 106th-111th Congress, N = 6,597 letter-bearing bills) finds that passage is driven by hot-bill status (2.428), commemorative bills (1.386) and majority-party sponsor (1.353). The efficiency term has no majority, salience or commemorative adjustment, so it likely rewards majority status. That is an inference. Any adjustment is Paul's call.
- **Sotoudeh 2024** (VERIFIED, weak) supports the idea that a decayed success ratio carries outcome information: the cosponsor pass-ratio separates passed from failed bills in every window. Its ratio is pairwise and includes each bill's own outcome. It is pooled across parties for 2009-2019 (UNSTRATIFIED), and the authors report a decline after about 2015.
- **Fouirnaies & Hall 2022** (VERIFIED-SOFTENED, weak; US state legislatures, N = 16,727) finds sponsorship falls about 6% when reelection incentives end, with no ideological change. Volume partly reflects electoral advertising. This supports keeping volume and direction separate, which signing by stance does. Because agenda volume is not normalised per member, Leg% also carries some electoral-incentive variance.

**4:2:1 role weights: no verified evidence.**
- *Online-scan leads (UNVERIFIED).* Kessler & Krehbiel (1996, APSR) find that early cosponsorship fits intralegislative signalling, which bears on original vs later cosponsors. Russo et al. (2022/ACL 2023, already in the system's cosponsor-refinement citations) separate active from passive cosponsorship.
- **FLAG:** if either paper is read as arguing for dropping regular cosponsorship, that proposal was measured and REJECTED in this project. Use these papers only to inform the weight *ratio*, never to drop the edge or row class.

**Contradictions**
- Bucchianeri (NC): stage-based effectiveness beats hit-rate metrics vs the system's efficiency term, which is a hit rate (advanced/total, decay-weighted). State-level, so a caution rather than a refutation.
- Bucchianeri: scores are only comparable within chamber-term vs the system's single cross-chamber percentile over 533 members.
- Box-Steffensmeier: majority-party sponsorship is a large passage predictor (1.353, House 106th-111th) vs no majority-status term in live_legislative_pillar.py:60-81, so efficiency% partly encodes majority status, which the committee pillar also rewards (double loading).

**Gaps**
- No test of whether stance-signed activity predicts CU-aligned final-passage votes better than unsigned activity. Fill with a holdout look on 119th CU roll calls, stratified by party.
- No evidence for the 0.286 : 0.143 : 0.0714 role weights. Fill by estimating role-specific effects (sponsor / original / later cosponsor) on subsequent CU votes, or by a cosponsorship-timing signal model (Kessler & Krehbiel lead), without dropping any role class.
- No test that efficiency adds validity beyond agenda volume. Fill with nested-model comparison against an external criterion, within chamber and majority strata.
- No federal check of hit-rate vs stage-based efficiency. CEL's five-stage LES is the available comparator.

**Paul's position vs the evidence**

Paul favours observable effort (sponsorship, committee work) as a signal that responds to incentives. Fouirnaies & Hall support that framing and also show volume moves with electoral incentives, not position. Signing by stance protects direction, but the agenda magnitude still moves with those incentives. Paul notes that nothing addresses the 4:2:1 weights, and the evidence confirms it. The efficiency design runs against Bucchianeri's state-level finding that stage-based scoring beats hit rates. Paul's own reading of Sotoudeh contrasts it with LES, but the verified evidence favours the LES-style approach for validity.

**Open questions**
- Should the efficiency term be computed within chamber and majority strata, or benchmarked as CEL does (on majority status, seniority and chair)?
- Should efficiency move from a hit rate to stage counts (introduced, action in committee, passed committee, passed chamber, law)?
- Is category_stance.json fresh at every run? This is the known staleness trap.
- What is the live agenda/efficiency blend? It is read from AB10/AB11 and is not in code.

**Read first:** [[bucchianeri-2025-legislative-effectiveness-american]], [[boxsteffensmeier-2018-taking-congress-interest]], [[sotoudeh-2024-network-based-measure]], [[fouirnaies-2022-electoral-incentives-affect]]

## Press pillar (own-domain, signed raw-max)

**What it computes:** Press% = signed press_value / MAX, where press_value = sum over categories of (stance sign x LSD tone intensity) x tiered category weight, for the member's own-domain statements only.

**Method:** Only *.senate.gov and *.house.gov rows count; 819 third-party rows were dropped, 816 of them americascreditunions.org. 1,310 synthetic billstatus rows were de-duplicated out. Category weights tiered 2026-07-02: Interchange, CU Tax Status, Credit-Card Mandates 0.30; Charter/FOM 0.15; Exam/NCUA 0.10; medium 0.08; low 0.02-0.05. Speech channel normalized at 1/n per event (a3174bc). Press is the only directional signal in the original design. Display-only overrides do not feed the pillar.

**Code refs:** `fec/apply_press_dedup.py`, `fec/apply_raw_max_norm.py`, `fec/moneyball_core.py:214-241`, `fec/engine/pillar_policy.py:2-55`

**Assumptions:**
- A member's own-domain statements represent the member's stance; third-party mentions do not
- Stance labels are the owner's domain (never relabelled)
- Tone intensity x stance sign is a valid single press measure

**Evidence (2 rows, 2 distinct papers: UNVERIFIED 1, VERIFIED-SOFTENED 1)**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[haselmayer-2017-sentiment-analysis-political]] | 2017 | qualifies | Dictionary-based tonality runs in the same direction as expert coding but compresses effects. In App. 3 Table 6 the government-pair coefficient is -0.13 with the dictionary vs -0.40 with expert coding, and electoral losses are 0.12 vs 0.23. Authors call dictionary estimates more conservative than manual coding. Pooled regression with party fixed effects, standard errors clustered by party pair; adjusted R2 0.03 with n = 619 (Table 3 and Table 6 dictionary columns). | The evidence is one Austrian campaign, in German, on negativity only. It suggests dictionary intensity carries a weaker, noisier version of the human-coded signal, so multiplying stance sign by that intensity may add noise. The paper does not test a stance x intensity product, so this is a caution, not a refutation. The paper's pooled coefficients should be labelled as pooled across parties with party fixed effects if quoted. | weak | VERIFIED-SOFTENED |
| [[vanderdoes-2022-strategic-identity-signaling]] | 2022 | qualifies | As cross-partisan outgroup share rose from 1 to 9 of 10, participants shared fewer tweets overall (AME -5.38, 95 percent CI -6.04 to -4.72) and shifted toward covert ones (AME 0.73). Covert tweets drew fewer outgroup dislikes but also fewer ingroup likes, so the message is lower-affect for the ingroup. | Requires that legislators in politically mixed districts or states reduce or code their CU statements the way the experiment participants did. The experiment paid at most 20 cents of cost and fixed audiences with no feedback, which is far from electoral stakes. Test by correlating own-domain press volume and LSD charge with district lean before acting; any pooled correlation across chambers is quoted with chamber strata or labelled UNSTRATIFIED. | weak | UNVERIFIED |

**Assessment**

**Evidence weight: weak. One verified caution and one unverified lead.**

- **Haselmayer & Jenny 2017** (VERIFIED-SOFTENED, weak; one Austrian campaign, German, negativity only, n = 619, adjusted R² 0.03, pooled with party fixed effects) finds that dictionary tone runs in the same direction as expert coding but compresses effects. On the government pair, the coefficient is −0.13 with the dictionary vs −0.40 with expert coding. Multiplying stance sign by LSD intensity may therefore add noise relative to sign alone. The paper does not test a sign×intensity product, so this is a caution, not a refutation.
- **van der Does 2022** (UNVERIFIED lead) is a low-stakes online experiment. It suggests members facing mixed audiences post less and more covertly, which would lower Press% for hedging members. Its transfer to electoral stakes is unshown. Test this by correlating own-domain press volume and LSD charge with district lean, by chamber.

**Actual description/code mismatch.** The component calls the method 'signed raw/max'. The verified code path is `normalize_signed_extrema` (fec/stance/media_rules.py, via moneyball_core.py:214-237), and it scales positive and negative values by separate extrema. A +x and a −x therefore get different magnitudes whenever the two extrema differ. The R rollup (run_stage5.R:171-182) is a decay-weighted *mean* per category, not a sum. The description should say so, because a mean removes volume while the 1/n per-event speech normalisation also removes volume.

**No verified row on own-domain filtering.** Paul notes the same. Dropping 819 third-party rows (816 from americascreditunions.org) is a validity choice that no study has tested.

**Online scan lead (UNVERIFIED).** Grimmer's work on Senate press releases and representational style shows press releases are a distinct representational channel. Whether they predict votes was not established in this scan.

**Contradictions**
- Component text 'signed raw/max' vs code normalize_signed_extrema (asymmetric: positives / max positive, negatives / |max negative|). A member's negative and positive press are on different scales.
- Component text 'sum over categories of (sign x tone) x weight' vs run_stage5.R per-category rollup as a decay-weighted mean; the volume semantics differ from the description.

**Gaps**
- No test of whether own-domain press stance predicts later CU-aligned cosponsorship or votes. Fill with a time-ordered holdout: press in window t, CU cosponsorship/votes in t+1, stratified by party and chamber.
- No test of sign alone vs sign×LSD intensity against an external criterion (Haselmayer suggests intensity may be noise). Fill with a nested comparison on the same holdout.
- No test of whether excluding third-party CU-lobby rows raises or lowers validity. Fill with an ablation using the same outcome.
- No evidence that high-volume speakers are not proportionally more aligned, which the 1/n per-event normalisation assumes. Fill by regressing alignment on statement volume, by chamber.

**Paul's position vs the evidence**

Paul wants press, news cycles and social feeds, scored by sentiment, added as determinants of pathway change (the Bacik augmentation). He treats public statements as the observable persona. The verified evidence counsels caution on the sentiment-intensity part: dictionary tone is a compressed, noisier proxy. The unverified van der Does lead implies the persona shown in press is strategically shaped by audience. Neither refutes using press, but both argue for testing sign alone before adding more sentiment channels. Stance labels are Paul's domain and were not evaluated here.

**Open questions**
- Is the asymmetric signed-extrema scaling intended? If not, should the pillar use a single |max| divisor?
- Should the description be corrected to say the R rollup is a mean, not a sum?
- Does sign alone predict CU behaviour as well as sign×LSD intensity?

**Read first:** [[haselmayer-2017-sentiment-analysis-political]], [[vanderdoes-2022-strategic-identity-signaling]]

## Demographic pillar input (Demo%)

**What it computes:** Demo% = PERCENTRANK of Blended Model column F (5% weight).

**Method:** Column F is formula-backed and edited by hand; no Python producer writes it. Sub Layer V:AF holds hard-coded values. The DEMO_POTENTIAL dial does not move this input. A 06-21 bias audit found a mild Democratic tilt (bucket mean D 0.52 vs R 0.34), accepted at 5% weight.

**Code refs:** `Moneyball Main Version - Blended.xlsx Blended Model col F; Sub Layer V:AF`

**Assumptions:**
- District demographics carry a small legitimate influence signal
- Demographic Potential is a market score, not a support prediction

**Evidence (0 rows, 0 distinct papers)**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment**

**GAP. There is no evidence row for this component.** Paul's only citation is Huneeus & Kim's state layer, and it is the conceptual origin of Demographic Potential. Demographic Potential does not feed Demo%.

Demo% comes from a hand-edited column F with no Python producer. Sub Layer V:AF is hard-coded. The DEMO_POTENTIAL dial does not move it. The 06-21 bias audit found bucket means of D 0.52 vs R 0.34 (party buckets over the 533-member roster; 06-21 snapshot) and accepted the result at 5% weight. At that weight it is small, but it is an uncontrolled party loading.

**Online scan, UNVERIFIED leads.**
- District financial-sector employment predicts TARP votes in a student-journal paper and a working paper. The evidence is mixed: it holds for the Senate but not the House in one probit.
- A state-legislature study finds that an industry's employment share predicts legislators' ties to that industry.

None measures credit-union presence. These leads justify a test, not the current input.

**Contradictions**
- Demographic Potential, the method Paul's state-layer idea produced, does not feed Demo%; the pillar runs on an unproduced hand-edited column. That breaks the lineage between Paul's stated position and the live input.

**Gaps**
- No study shows district demographics or CU market presence predict CU-aligned behaviour. Fill with a partial correlation or logit: CU-aligned votes/cosponsorship on Demo% (or district CU membership share or branch density from NCUA Call Reports), controlling for party, stratified by chamber.
- No reproduction path: column F has no producer, so the input cannot be audited or refreshed. This is an engineering gap, not a literature gap.
- No rank-impact check of swapping column F for Demographic Potential v3.0. Fill with a Kendall tau and top-133 overlap comparison (denominator 533).

**Paul's position vs the evidence**

Paul proposed a state or industry-presence layer to show whom to target. The idea is reasonable and has some indirect support in the literature (leads only). The live Demo% is not that layer: it is a hand-maintained column with a known mild Democratic tilt. Neither the position nor the input has verified evidence.

**Open questions**
- Should Demo% be produced by code, from Demographic Potential or an NCUA-based CU-presence measure? This is Paul's call.
- After controlling for party, does the D 0.52 / R 0.34 tilt leave any residual signal?

**Read first:** [[huneeus-2018-effects-firms-lobbying]]

## Sub Layer legislative-efficiency formulas and off-by-one repair

**What it computes:** Per-member legislative efficiency from Sub Layer - Raw AG:AW, normalized by Table1 min-max.

**Method:** A 2026-07-15 row shift made every member use the neighbour's record. Repair c6dc6b2 (2026-07-17) repointed 9,061 formulas, fixed AH (Q->P) and AI (AI->X), and anchored on each formula's own Table1 basis column. Independent recompute matches 533/533; 520/533 ranks moved, all legislative-only.

**Code refs:** `fec/repair_sublayer_references.py (--plan/--apply)`

**Assumptions:**
- Each formula's Table1 basis names the intended column (the majority pattern was the bug)
- A stale pillar snapshot can shield downstream consumers from an unrepaired reference plane

**Evidence (0 rows, 0 distinct papers)**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment**

**No literature is needed. This is an engineering repair**, and Paul's position matches.

The evidence that matters is internal. The 2026-07-17 repair (c6dc6b2) repointed 9,061 formulas. An independent recompute matches 533/533. Ranks moved for 520/533 members, all legislative-only. That shows internal consistency after the repair. It does not show the efficiency values match raw congress.gov outcomes.

The underlying efficiency construct is assessed under the legislative pillar, where Bucchianeri's hit-rate caution applies.

**Contradictions**
- (none)

**Gaps**
- No reproduction of post-repair efficiency values from raw congress.gov billstatus in an independent pipeline (the component's own open question). Fill with a second-path recompute (Python from billstatus) and a byte or rank match against the Sub Layer values.

**Paul's position vs the evidence**

Paul says there is no literature and this is an engineering repair. That is correct, and the internal recompute supports the repair. External reproduction is still open.

**Open questions**
- Do any downstream consumers still read a stale pre-repair pillar snapshot?

**Read first:** (none)

## Cosponsor-network InfluenceScore (computed, weight 0)

**What it computes:** InfluenceScore = 0.60 x weighted out-degree + 0.25 x betweenness + 0.15 x eigenvector, joined to 494/533 members; the 39 absent fall back to committee%.

**Method:** Computed from the congress_centrality output. It was retired from Influence on 2026-06-25 (AB13 = 0) because it is direction-blind. It still feeds the underdog lens.

**Code refs:** `congress_centrality_*.xlsx; Blended Model AB12/AB13`

**Assumptions:**
- Cosponsor centrality reflects influence regardless of stance direction

**Evidence (9 rows, 9 distinct papers: VERIFIED 2, VERIFIED-SOFTENED 7)**

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

**Assessment**

**Evidence weight: moderate and consistent with retiring centrality from Influence. None of it validates keeping centrality in the underdog lens.**

**Rows that bear on the retirement:**
- **Sotoudeh 2024** (VERIFIED-SOFTENED, moderate; House 2009-2019). Eigenvector, closeness and strength barely separate passed from failed bills: relative difference about 0.004 vs 0.139 for the paper's own influence score. The paper never tests betweenness or weighted out-degree, and it uses a different, undirected ratio matrix.
- **Battaglini & Patacchini 2016** (VERIFIED-SOFTENED, weak; House committee and alumni networks, 2004-2012). Unweighted degree, closeness and betweenness do not explain PAC money. That is neutral on influence.
- **Azaiez 2025** (VERIFIED-SOFTENED, moderate). Centrality tracks resources (lobbying FTE, assets) rather than influence, on EC company nodes (N = 1,178). This matches the project's finding that the raw Katz ranking measured activity (Spearman with tie volume +0.845).
- **Christopher 2016** (VERIFIED). An out-degree-led judgment composite like this one has no outcome test.

**Context rows** (Haythornthwaite, Ringe appendix, Kumar, Trivedi, Kempe 2015) are taxonomy or simulated-cascade evidence. Kempe adds that, under IC at p = 10%, random seeds beat centrality heuristics beyond about 12 targets. That evidence comes from one physics co-authorship graph (winter 2002; denominator 10,748 researchers; k = 1-30).

**Circularity warning (Sotoudeh row).** A passage test on the system graph would be circular, because weighted out-degree already triples ties on passed bills. Any test needs an as-of network built only from bills resolved before the outcome window.

**Wording.** Any revived centrality score is a heuristic model output, never a closeness, access or influence probability (D-025).

**Record vs code.** Two verified rows searched `fec/` and found the weights at build_legislation_base_network.py:818-840. They did not find the AB13 = 0 retirement or the lens feed, which exist only in the workbook.

**Contradictions**
- The system retires centrality from Influence as direction-blind, yet the same InfluenceScore enters the underdog lens's Intrinsic composite at 0.5 weight as a measure of intrinsic influence. The evidence supporting the retirement (Sotoudeh, Azaiez) argues equally against the lens use.
- The AB13 = 0 retirement and lens feed are recorded in brain notes but not found in fec/ code (Sotoudeh and Battaglini rows); they live only in the workbook, so the rank authority's (ranks_python.json) treatment of centrality cannot be confirmed from code.

**Gaps**
- No federal test of whether cosponsor centrality predicts passage of a member's sponsored bills beyond committee position. Fill with an as-of network built only from bills resolved before the outcome window, testing weighted out-degree and betweenness (untested anywhere in the verified set), stratified by chamber and majority.
- Paul's requested Viral Centrality test (Fink) and a stance-gated (signed) centrality have no evidence either way. Any signed variant must keep regular cosponsorship edges and not apply per-edge volume normalisation: both proposals were measured and REJECTED here.

**Paul's position vs the evidence**

Paul says more research on forms of centrality is needed. He values directionality and highlighted Sotoudeh's line that eigenvector centrality is not a strong indicator of passage. The verified evidence agrees and supports the retirement. The underdog lens still uses the retired measure, which conflicts with the reasoning Paul endorsed.

**Open questions**
- Should centrality be removed from the underdog lens for the same reason it was retired from Influence?
- Should the retirement be moved from the workbook into code (compute_pillars.py or pillar_policy.py) so the rank authority records it?
- Is a signed or stance-gated centrality worth a prereg, and what outcome would it be tested on?

**Read first:** [[sotoudeh-2024-network-based-measure]], [[azaiez-2025-hypergraph-analysis-european]], [[battaglini-2016-influencing-connected-legislators]], [[christopher-2016-mathematical-modeling-analysis]], [[kempe-2015-maximizing-spread-influence]]

## Undervaluation (underdog) lens

**What it computes:** Underdog rank = Intrinsic - Visibility, where Intrinsic = 0.5 Leg% + 0.5 Centrality% and Visibility = 0.5 Press% + 0.5 Committee%.

**Method:** Difference of two equal-weight composites, ranked. The brain note gives no code file.

**Code refs:** (none)

**Assumptions:**
- Legislative activity and centrality measure intrinsic influence; press and committee measure visibility

**Evidence (2 rows, 2 distinct papers: VERIFIED-SOFTENED 2)**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | qualifies | Third-coalition members are usually ideologically aligned with the majority party. The paper attributes the majority party's effectiveness to agenda-setting power, and says a substantial portion of the apparent majority advantage comes from the third coalition. Third-coalition effectiveness drops temporarily during majority transitions and rests on a few ultra-effective members. | The paper defines its 'hidden' bloc by signed-tie structure, not by low press or committee visibility, and its effectiveness tracks majority alignment. That is closer to what the lens subtracts (committee position) than to what it rewards. The link is an analogy and is not evidence for the lens's formula. | weak | VERIFIED-SOFTENED |
| [[amsalem-2018-effect-politicians-personality]] | 2018 | qualifies | Holding office is the strongest predictor of visibility: minister beta .369 (b 3.122), about three times the trait betas. Party leader beta .195 is only slightly stronger than the personality effects. Office here means minister, party leader and house speaker; committee position is not tested. | The paper's evidence is MPs in three parliamentary systems, not US House members. Its visibility construct is press mentions, which Press% does not capture. Committee% is a visibility input the paper never tests. Any reading of personality as part of the underdog residual is an inference, not a finding. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: weak. Both rows are analogies, and both cut against the formula.**

- **Aref & Neal 2021** (VERIFIED-SOFTENED) define their 'hidden' third coalition by signed-tie structure, not by low press or committee visibility. Its effectiveness tracks *majority alignment and agenda-setting*, which is close to what the lens subtracts (committee position), not what it rewards. The bloc also rests on a few ultra-effective members.
- **Amsalem 2018** (VERIFIED-SOFTENED; MPs in Belgium, Canada and Israel) finds office holding is the strongest driver of media visibility (minister β = .369). Committee position is not tested, and the paper's visibility construct is press *mentions*, which Press% does not measure. Press% is own-domain statements.

So neither paper supports Intrinsic = 0.5 Leg% + 0.5 Centrality% minus Visibility = 0.5 Press% + 0.5 Committee%.

The lens also has internal problems:
- it has no code file;
- it rewards centrality the system retired as direction-blind;
- it classifies own-domain press output as 'visibility', when that output is the member's own advocacy, not media attention.

**Contradictions**
- The lens treats Press% (own-domain statements by the member) as 'visibility', while the visibility literature (Amsalem) measures third-party media mentions, which the press pillar explicitly excludes.
- Aref & Neal's hidden effective bloc derives effectiveness from majority alignment, the same positional advantage the lens subtracts via Committee%.
- Centrality, retired from Influence as direction-blind, is half of the lens's Intrinsic.

**Gaps**
- No test that high-underdog members show higher realised effectiveness per dollar of PAC attention than their visibility predicts (the component's own question). Fill with: underdog score in Congress t vs CEL LES (or CU advancement) in t+1, divided by PAC receipts (FEC, same window), stratified by chamber and majority.
- No code producer, so the lens cannot be reproduced or audited.

**Paul's position vs the evidence**

Paul reads Aref & Neal's hidden effective coalition as the conceptual twin of the lens and as the responsive or vulnerable seats. The verified reading does not support the twin claim. The paper's bloc is structurally defined and majority-aligned. It is not defined by low headlines and high intrinsic activity. The analogy is a motivation, not evidence for the formula.

**Open questions**
- Should the lens be rebuilt in code, with centrality removed or replaced?
- Should 'visibility' be measured by third-party media mentions rather than own-domain Press%?
- Is the target construct 'undervalued by the press' or 'undervalued by PAC money'? The two need different denominators.

**Read first:** [[aref-2021-identifying-hidden-coalitions]], [[amsalem-2018-effect-politicians-personality]]

## Legacy Sub Layer -> Primary Layer -> Rank & Allocation cascade

**What it computes:** Legacy 11-dimension SUMPRODUCT score and rank (Rank & Allocation cols K/L/M).

**Method:** This cascade is orphaned in the Blended version but not retired. Its rankings disagree with Blended. Primary Layer - Normalized H2 = 0.80 (likely meant to be 0.08), so the weight row sums to 1.72.

**Code refs:** `Rank & Allocation cols C-M; Primary Layer - Normalized row 2`

**Assumptions:**
- Superseded design; recorded for lineage only

**Evidence (0 rows, 0 distinct papers)**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment**

**No literature is needed. It is recorded for lineage**, and Paul's position matches.

**There is one actual error.** Primary Layer - Normalized H2 = 0.80, which is probably meant to be 0.08, so the weight row sums to 1.72. The cascade is orphaned but not retired, and its rankings disagree with the Blended version.

A reader of Rank & Allocation columns K/L/M could take a mis-weighted rank as current. The component's open question about which rank better tracks CU-aligned votes is answerable only after the H2 error is fixed. Otherwise the comparison is against a broken baseline.

**Contradictions**
- Weight row sums to 1.72 (H2 = 0.80, likely 0.08), so the legacy rank is mis-weighted by construction; legacy and Blended ranks disagree partly because of this error, not only design.

**Gaps**
- No comparison of the legacy (with H2 corrected) vs the blended rank against CU-aligned final-passage votes in the 119th Congress. Fill once with a single-look Spearman per rank, by chamber, or retire the cascade and skip it.

**Paul's position vs the evidence**

Paul says there is no literature, which is correct. The open issue is housekeeping, not evidence. The cascade should be either corrected (H2) and labelled legacy, or formally retired. That is Paul's call under change control.

**Open questions**
- Retire the cascade or correct H2 to 0.08? This is Paul's ruling.

**Read first:** (none)

## Across components

**1. Positional advantage is counted twice.**
- Majority status and chair position raise advancement:
  - Box-Steffensmeier: majority sponsor 1.353 (House, 106th-111th Congress);
  - Bucchianeri: majority +0.370 (pooled, UNSTRATIFIED) and chair +0.507 (pooled; strata 0.609 and 0.307; North Carolina).
- Advancement feeds Leg% efficiency, and chair and ranking seats feed Influence% directly.
- The composite has no term that separates the two, so position likely carries more than its nominal 40%. This is an inference, testable with a Paruolo-style main-effect check.
- CEL benchmarks on majority status, seniority and chair precisely so that effectiveness is not just position. The Moneyball rank deliberately rewards position as 'structural power', which is a defensible owner choice. It should be stated as such and not compared to LES as if the two were the same construct.

**2. House and Senate are pooled everywhere.**
- Leg% is one percentile over 533 members, committee% uses one max, and Press% uses one set of signed extrema.
- Every verified effectiveness source reports chamber-specific magnitudes (Bucchianeri strata; CEL averages from the scan, unverified).
- Any figure quoted from the rank across both chambers should carry an UNSTRATIFIED label until per-chamber normalisation is tested.

**3. Normalisation is mixed, so nominal weights are not effective weights.**
- The pillars use PERCENTRANK, raw/max (skewed, with 245/533 committee zeros) and asymmetric signed extrema.
- The 06-25 min-max finding already showed effective shares can depart sharply from nominal ones.
- No variance-share or main-effect audit has been run on the current mix.

**4. Centrality is treated inconsistently.**
- It is retired from Influence as direction-blind, and the verified evidence (Sotoudeh, Azaiez, Battaglini) supports that.
- It is still half of the underdog lens's Intrinsic.
- Both the retirement and the lens feed exist only in the workbook, not in code.

**5. There is no outcome criterion anywhere.**
- Every empirical question in the hub needs the same missing object: a preregistered, time-ordered CU outcome set for the 119th Congress (CU-bill advancement, CU-aligned final-passage votes), plus CEL LES as a general comparator.
- Building that once would answer the blend, legislative, press, committee, demo, lens and legacy questions in one look per question.

**Guardrails for the follow-up work:**
- Any centrality or graph rank is a heuristic model output, never closeness, access or introduction probability (D-025).
- Any proposal to drop regular cosponsorship rows or edges, or to apply per-edge volume normalisation, was measured and REJECTED. That includes readings of Russo et al. or Kessler & Krehbiel on active vs passive cosponsorship; flag such a proposal, never adopt it.
- LDA lobbied-bill lists may not supply committee relevance weights (D-021); use billstatus referrals.
- UNVERIFIED rows (van der Does, the Committee Requests codebook) and all online-scan items are leads only (DE-062).

## Research agenda

1. Fix the pillar-weight record. Rule on S-5 (30/25/40/5 vs 45/10/40/5), then make pillar_policy.py:11-16, MATHEMATICAL_SPECIFICATION.md:12 and the brain agree. Paul's ruling; this is the error-level item.
2. Weight sensitivity. Run a one-weight proportional-rescale sweep (Christopher 2016 recipe) plus a Monte Carlo over weights and normalisation (Saisana et al. 2005, web-scan lead) on the 533-member roster. Report Kendall tau-b and top-133 overlap between the two candidate weight sets.
3. Effective-vs-nominal weight audit. Compute each pillar's main effect or variance share in the composite under the current mixed normalisation (Paruolo, Saisana & Saltelli 2013, web-scan lead). Report by chamber.
4. Build a preregistered outcome criterion set for the 119th Congress: CU-category bill advancement by stage, CU-aligned final-passage votes, and CEL LES. Then run a single look of the blend and of each pillar against it, stratified by chamber and majority/minority. Verify or retire the brain-only +0.10 rho vs LES.
5. Committee: estimate chair, ranking and subcommittee premiums by chamber on the outcome set. As a sensitivity check (needs its own look), compare the hand tiers with CU-category referral shares from billstatus, following Huneeus & Kim; never use LDA for this (D-021).
6. Committee selection: obtain and actually read the House Committee Request data (Frisch & Kelly codebook; UNVERIFIED and unread). Pull Grier & Munger 1991 and Kroszner & Stratmann 1998 (web-scan leads) to design a selection-vs-conferred-leverage check.
7. Legislative: test stage counts against the hit-rate efficiency term, and the efficiency term against agenda volume alone, within chamber and majority strata. Estimate role-specific effects to check the 4:2:1 ratio, using Kessler & Krehbiel 1996 as a lead, and never drop regular cosponsorship.
8. Press: correct the description to match the code (asymmetric signed extrema; per-category mean rollup). Run a time-ordered test of sign alone vs sign x LSD intensity on later CU cosponsorship and votes. Ablate the third-party-row exclusion. Correlate own-domain press volume and LSD charge with district lean by chamber (van der Does lead).
9. Demographic: give Demo% a code producer. Test Demo% or an NCUA-based CU-presence measure against CU-aligned behaviour, controlling for party and stratified by chamber. Report the rank impact of swapping in Demographic Potential v3.0.
10. Centrality and underdog lens: move the AB13 retirement into code; write the lens in code. Decide whether centrality stays in Intrinsic. If any centrality test is run, build an as-of network from bills resolved before the outcome window, to avoid passage circularity. Wording per D-025.
11. Sub Layer: run an independent recompute of post-repair efficiency from raw congress.gov billstatus and match it against the Sub Layer values.
12. Legacy cascade: Paul rules either to correct Primary Layer H2 (0.80 to 0.08; the weight row now sums to 1.72) and label the cascade legacy, or to retire it formally.
13. Copies to fetch (web-scan leads, not yet downloaded): Saisana, Saltelli & Tarantola 2005 (JRSS A 168(2)); Paruolo, Saisana & Saltelli 2013 (JRSS A 176(3)); OECD-JRC Handbook on Constructing Composite Indicators 2008; Kessler & Krehbiel 1996 (APSR, Dynamics of Cosponsorship); Russo et al. arXiv 2205.09674; CEL LES 119th Congress data and methodology (thelawmakers.org); Grier & Munger 1991; Kroszner & Stratmann 1998.
