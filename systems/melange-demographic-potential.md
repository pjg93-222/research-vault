---
type: system
name: Melange Demographic Potential and district metrics
slug: melange-demographic-potential
components: [melange-demographic-potential.growth-room-v21, melange-demographic-potential.footprint-v30, melange-demographic-potential.census-cd-metrics]
brain_notes: [concepts/demographic-potential-method, actors/draft-systems-r-pipeline-architecture]
paper_count: 6
tags: [system, type/system-hub]
---

# Melange Demographic Potential and district metrics

District and state CU market-opportunity scores: v2.1 growth room (location quotients and shift-share) and v3.0 CU footprint (the default). Includes the Census congressional-district metric build. Not wired to the Demo% pillar input.

**Brain notes:** [concepts/demographic-potential-method](obsidian://open?vault=brain&file=concepts/demographic-potential-method); [actors/draft-systems-r-pipeline-architecture](obsidian://open?vault=brain&file=actors/draft-systems-r-pipeline-architecture)

## Melange Demographic Potential and district metrics: evidence assessment

**Short version.** None of the three components has literature evidence that tests what it computes. Across all three there are 6 paper bearings (6 distinct papers). By status: 4 UNVERIFIED, 1 VERIFIED, 1 VERIFIED-SOFTENED. By strength: 5 weak, 1 moderate. Huneeus 2018 (VERIFIED, weak) *qualifies* the claim. Becker n.d. (VERIFIED-SOFTENED, moderate) *suggests an extension*. The other four are UNVERIFIED single-agent leads, each weak. No bearing contradicts any component outright. All are about transfer: they come from a different unit (states, precincts, one county), a different outcome (votes, grocery trips) or a different mechanism (a headquarters-state identifying assumption) from what Melange computes.

**By component:**
- **v2.1 growth room (non-default): a GAP.** No bearing is filed against it. The one citation Paul attaches to it (Huneeus & Kim 2018, "state layer") has a VERIFIED reading that does not support growth room.
- **v3.0 CU footprint (default, D-035): thin.** It rests on an owner ruling ("the footprint should be the potential"). The ruling is a definitional choice, and the evidence does not overturn it. But no paper here supports the 0.30/0.20/0.25/0.25 weights, and none addresses the component's own empirical question: does footprint track members' CU-aligned behaviour?
- **Census CD metrics: unanswered.** The empirical question is how much error the mixed boundaries introduce (CBP on 116th-Congress boundaries, ACS on 119th). No paper answers it. Becker (VERIFIED-SOFTENED) supplies a measurement design that could size the error. McCartan (UNVERIFIED) supplies a crosswalk recipe that could remove it, but the recipe adds an allocation error of its own.

**Most important tension.** Paul cites Huneeus & Kim 2018 for both v2.1 and v3.0, yet the two versions score in opposite directions: v2.1 rewards whitespace (CU absence), v3.0 rewards footprint (CU presence). One paper cannot back both. The VERIFIED reading says it backs neither: it shows only that the home-state link works as an identifying assumption for firm headquarters, at state level.

**What is at stake.** Demographic Potential is not wired to the Demo% pillar input. That input is a hand-edited, formula-backed workbook column (D-035). So these evidence gaps affect the member desk's Demographic Potential label, score, tier and rank, not the rank blend. The score's own method doc already says the weights "cannot be called empirically optimal" without a preregistered out-of-sample outcome. The evidence base agrees.

## Demographic Potential v2.1 (growth room)

**What it computes.** 0-100 score = 100 x sum(weight x percentile) over LQ strength 20, competitive shift 20, aligned sectors 15, branch whitespace 15, deposit whitespace 5, LMI 10, large employer 8, small business 7.

**Method.** LQ = local share / US share; shift-share identity. Qualification LQ >= 1.25. Non-default.

**Code refs:** `fec/district/demographic_potential.py`, `DEMOGRAPHIC_POTENTIAL_METHOD.md`

**Assumptions:**
- Judgmental weights, not estimated
- Whitespace is opportunity

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

### Demographic Potential v2.1 (growth room): GAP

**What it computes.** A score from 0 to 100: 100 x the sum of (weight x tie-aware percentile) over eight judgmental components. They are LQ strength (20), competitive shift (20), aligned sectors (15), CU branch whitespace (15), deposit whitespace (5), LMI (10), large employer (8) and small business (7). A market qualifies only if at least one CU-relevant constituency sector has a two-digit LODES LQ of at least 1.25, a positive competitive shift, and more than 500 jobs in both the current and base years. The version is non-default; `DEMO_POTENTIAL=growth_room` restores it.

**Evidence weight: none direct.** No bearing in the hub is filed against this component. What exists is indirect:
- **Huneeus 2018 (VERIFIED, weak, filed on v3.0).** Paul cites this paper for v2.1's "state layer". The verified reading is narrower than that. The firm-politician link is *defined* as the politician representing the firm's headquarters state. It is an identifying assumption, with first-stage F of 14.30 and 13.10 (Table 2; the bearing gives no window or N). It is not an estimated effect of sector concentration, shift-share growth or whitespace. It therefore gives v2.1 no support for its LQ and shift-share terms, its 1.25 gate or its weights.
- **Esmalian 2022 (UNVERIFIED lead, weak, filed on v3.0).** The bearing extends its caution to v2.1's branch-whitespace term (`demographic_potential.py:152`). The caution is that facility counts track realized use poorly: stores per zone against % change in stores visited, r = -0.06. That figure covers Harris County on 24 Aug 2017 only, with the number of zones not reported. Growth room reads the *absence* of CU branches as opportunity. Whether a CU-branch gap marks unmet demand or simply no demand is exactly the question this lead raises, and it is untested in this domain.
- **Holyoke 2015 (UNVERIFIED lead, weak, filed on v3.0).** Lobbyists target lawmakers with a constituency stake. That is a conceptual match for targeting by industry presence. The paper does not test it, and it says nothing about growth room in particular.

**Transfer conditions.** None of these papers studies credit-union market growth, LQ or shift-share as a predictor, or House-district geography.

**Study that would fill the gap.** The component's own empirical question, and the method doc's stated requirement, call for a **preregistered out-of-sample backtest**:
- Compute v2.1 for a base year, using LODES, CBP, NCUA and FDIC SOD vintages from that year, on a fixed district crosswalk.
- Score it against later CU outcomes over a stated window, e.g. 3-5 years. Outcomes: NCUA membership, shares, loans, and growth in branch counts per district.
- Report Spearman and top-decile hit rates against three comparisons: an equal-weight version, each leave-one-component-out variant (already computed on every rerun) and a size-only baseline.
- Stratify by urban/rural and by Census region. Otherwise label the pooled result UNSTRATIFIED.

A secondary option is a targeted literature search for empirical work on bank and CU **branch-entry determinants**: does market whitespace or concentration predict entry? No such paper is in the corpus.

**Contradictions**
- Paul cites Huneeus & Kim 2018 as support for both v2.1 (rewards whitespace, i.e. CU absence) and v3.0 (rewards CU presence). One citation cannot back opposite scoring directions. The VERIFIED reading of the paper backs neither: the home-state link there is an identifying assumption for firm headquarters at state level, not an estimated effect of sector concentration or whitespace.

**Gaps**
- No bearing tests whether the growth-room score predicts later CU membership, shares, loans or branch growth in a district. This is the component's own empirical question, and the method doc names it as the precondition for calling the weights anything but judgmental.
- No evidence supports the 1.25 LQ qualification threshold, the 500-job floor or any of the eight weights. All are policy choices.
- No evidence tests whether "whitespace is opportunity" (the branch and deposit whitespace terms) means unmet demand rather than an absence of demand. The only bearing that touches it (Esmalian 2022) is an UNVERIFIED lead from a different domain.

**Paul's position vs the evidence.** Paul's position is that v2.1 realizes his targeting-by-industry-presence idea through LQ and shift-share, citing Huneeus & Kim 2018 (state layer). As a description of what the code does, that is accurate. As a claim with evidential support, it is unbacked. The only verified reading of the cited paper says the paper treats the headquarters-state link as an identifying assumption. It does not show that industry presence, growth or whitespace predicts anything, and it works at state level, not district level. The citation should be relabelled as design inspiration, not support. The idea itself is testable (see the backtest design), and that test has not been run.

**Open questions**
- Should Huneeus & Kim 2018 stay attached to v2.1 at all, given that its verified use is a headquarters-state identifying assumption and v3.0 cites the same paper for the opposite direction?
- Paul should decide whether the preregistered backtest, the method doc's own precondition for empirical weights, is worth running for a non-default mode. If v2.1 stays retired as default, the test may matter only if D-035 is reversed.
- Does the branch-whitespace term measure unmet demand or an absence of demand? Without an outcome test, this sign is assumed.

**Read first:** [[huneeus-2018-effects-firms-lobbying]], [[esmalian-2022-characterizing-equitable-access]], [[holyoke-2015-learnable-skills-unteachable]]

## Demographic Potential v3.0 (CU footprint, default)

**What it computes.** Score from CU employment LQ .30, establishment LQ .20, branch share .25 and deposit share .25; qualifies at employment LQ >= 1.0.

**Method.** Default since 2026-09-16 (D-035). Opposite direction to v2.1.

**Code refs:** `fec/district/demographic_potential.py:29-42`

**Assumptions:**
- Current footprint is the potential (owner ruling)

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[holyoke-2015-learnable-skills-unteachable]] | 2015 | context | Relationship targeting means finding lawmakers who respond to information and who have an electoral interest in the group's constituency (p.18). Lobbyists should pick out lawmakers with a relevant interest (p.17). | This matches the targeting logic only in concept: aim at members whose constituents hold a stake. The paper does not test whether that stake predicts responsiveness, and Demographic Potential does not feed Demo%. No weight support. | weak | UNVERIFIED |
| [[huneeus-2018-effects-firms-lobbying]] | 2018 | qualifies | Firm-politician connection is defined as politicians representing the firm's headquarters state. The paper reports no headquarters moves in its data, and the likelihood that a politician changes district is less than 1 percent. The instrument built on the home-state link has first-stage F 14.30 and 13.10 (Table 2). | In H&K the home-state link is an identifying assumption for headquarters only, not an estimated effect of employment or branch footprint, and it is at state level, not district level. It gives weak support to the state-layer idea. It does not validate the footprint weights or any claim that footprint predicts CU-aligned behaviour. | weak | VERIFIED |
| [[esmalian-2022-characterizing-equitable-access]] | 2022 | qualifies | How many facilities a zone has is a poor indicator of realized access. Correlation of stores per zone with the preparation-phase % change: stores visited r = -0.06 (p = 0.007), trip time r = -0.01 (p = 0.448), trip distance r = -0.03 (p = 0.100). Window: 24 Aug 2017. Denominator: Harris County zones in the analysis, N not reported. Food-desert status alone did not separate high- from low-income access. | Weak analogy only. The paper covers grocery trips in a disaster in one county, with access measured from mobility traces. Demographic Potential is a national, district-level market score under normal conditions. It is explicitly not a measure of support or access, and the footprint choice is an owner ruling. The paper's lesson (supply counts are not realized use) would matter only if footprint were read as members' or constituents' access to CUs. It does not challenge the owner's choice to treat footprint as potential. The same caution applies to the growth_room branch-whitespace term (demographic_potential.py:152). | weak | UNVERIFIED |

### Demographic Potential v3.0 (CU footprint, default since 2026-09-16, D-035)

**What it computes.** Four percentile components, all read from code (`demographic_potential.py`, `COMPONENT_DEFINITIONS_FOOTPRINT`):
- CU (NAICS 522130) employment LQ, weight 0.30, source CBP
- CU establishment LQ, weight 0.20, source CBP
- CU branch share, CU / (CU + FDIC bank branches), weight 0.25
- CU deposit share, weight 0.25. This is a cross-system proxy: each CU's NCUA shares are split equally across its branches and set against FDIC branch deposits.

A market qualifies at an unrounded employment LQ of at least 1.0. The score runs in the opposite direction to v2.1. The definition rests on Paul's ruling that "the footprint should be the potential".

**Evidence weight: thin, one weak VERIFIED bearing plus two leads.**
- **Huneeus 2018 (VERIFIED, weak, qualifies).** The home-state firm-politician link serves as an identifying assumption for *headquarters*: no HQ moves in the data, under 1% chance a politician changes district, first-stage F 14.30 and 13.10 (Table 2; the bearing gives no window or N). The bearing judges this weak support for the *state-layer* idea that representatives answer to home-state firms. It does not support district-level transfer, employment or branch footprint in place of headquarters, the four weights, or any link from footprint to CU-aligned behaviour.
- **Holyoke 2015 (UNVERIFIED lead, weak, context).** Lobbyists should target lawmakers who respond to information and have an electoral interest in the group's constituency (pp. 17-18). This matches the logic of footprint-as-potential in concept only. The paper offers advice on practice and does not test whether stake predicts responsiveness.
- **Esmalian 2022 (UNVERIFIED lead, weak, qualifies).** Supply counts are a poor proxy for realized use: stores per zone against % change in stores visited, r = -0.06 (p = 0.007). The window is Harris County, 24 Aug 2017, with the number of zones not reported. The paper is about grocery access in one county during a disaster. It matters only if the footprint score is read as *constituents' access to or use of* CUs, which the system explicitly does not claim. It does not challenge the ruling.

**Transfer conditions.** All three papers are far from "House-district CU footprint, normal conditions, Congress". None measures CU presence or credit-union policy behaviour.

**Measurement notes from code** (not literature, but they bear on how much any future validation can show):
- 50% of the weight (both LQs) comes from CBP NAICS 522130 at district level. That makes it subject to CBP cell suppression and to whatever boundary vintage the district CBP file uses (see census-cd-metrics).
- The deposit share is an equal-split proxy across systems, as the code's own caveat says.

**Study that would fill the gap.** The empirical question is whether a district's CU footprint is associated with its member's CU-aligned behaviour.
- **Cross-sectional design.** Regress member CU-aligned behaviour on v3.0 footprint at the start of each Congress. Outcomes: cosponsorship of CU-position bills, and roll calls on bills where CU trade associations took public positions. Controls: party, DW-NOMINATE, Financial Services Committee seat, bank footprint as the counter-constituency, and district urbanity and size. Stratify by party, a known fault line, or label the pooled coefficient UNSTRATIFIED. Report the Congresses (window) and the member-Congress N (denominator).
- **Stronger design.** Use within-member change in footprint caused by redistricting (2022 maps, mid-decade redraws) as a quasi-experiment. Melange's `redistricting_density.py` already computes cu_share_old, cu_share_new and delta per member, so the exposure variable exists. Any such result is association. It is not evidence of access or influence.

**Contradictions**
- There is no contradiction between papers. The tension is internal. Paul calls v3.0 "the same idea" as v2.1 and attaches the same citation (Huneeus & Kim 2018), but the two versions score in opposite directions. The verified reading of Huneeus supports neither direction at district level.
- Esmalian 2022 (UNVERIFIED) finds that facility counts poorly predict realized use. That is in tension only with a reading of footprint as access. It is not in tension with the owner ruling, which defines footprint as potential.

**Gaps**
- No bearing tests the component's empirical question: whether district CU footprint is associated with members' CU-aligned behaviour (constituency interest). This needs a member-Congress association study stratified by party, ideally using redistricting-induced footprint change.
- No evidence supports the weights 0.30/0.20/0.25/0.25 or the employment-LQ >= 1.0 gate. They are judgmental, and the bearings themselves call them unestimated.
- No evidence covers the reliability of district-level NAICS 522130 CBP cells (suppression, boundary vintage) or the equal-split NCUA-shares deposit proxy. The first is a data-quality study; the second needs a check against a branch-level share source if one exists.

**Paul's position vs the evidence.** Paul's position is "same idea; owner ruling chose footprint". The ruling stands. It is a definition of what the score means, and nothing in the evidence overturns it. The "same idea" framing and the Huneeus & Kim citation outrun the evidence. Huneeus (VERIFIED) supports, weakly, only the state-level premise that representatives are linked to home-state firms, and only as an identifying assumption. Holyoke (UNVERIFIED) matches the targeting logic in concept without testing it. Nothing in hand shows that a higher CU footprint goes with more CU-aligned member behaviour. The default score is therefore best described as a defined market measure under an owner ruling, not an evidence-backed predictor.

**Open questions**
- What boundary vintage does Melange's district CBP file (`district_industry_4digit.json`, which feeds both v3.0 LQs and 50% of the weight) use? If it is the 116th Congress, the census-cd-metrics mismatch flows straight into the default score.
- How many districts have suppressed or zero NAICS 522130 CBP cells? How does tie-aware percentile scoring handle them under the LQ >= 1.0 gate?
- Should the Huneeus & Kim citation on v3.0 be relabelled as "state-level identifying assumption, not district evidence"?
- Would Paul want the redistricting-delta association study, using cu_share_old/new/delta, run as a preregistered single look? It would be the first direct test of footprint as constituency interest.

**Read first:** [[huneeus-2018-effects-firms-lobbying]], [[holyoke-2015-learnable-skills-unteachable]], [[esmalian-2022-characterizing-equitable-access]]

## Census congressional-district metrics

**What it computes.** 489 rows x 25 ACS and CBP metrics at state and district level.

**Method.** Census API pulls stacked across levels. Documented boundary mismatch: ACS on 119th-Congress districts, CBP on 116th.

**Code refs:** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/census_cd_scraper.R`

**Assumptions:**
- Mixed-vintage boundaries are acceptable

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[best-2022-redistricting-commissions-avoid]] | 2022 | qualifies | Redistricting materially reshuffles how voters are distributed across districts: the SD and skew of district vote shares differ from neutral by several points. The authors also flag boundary-vintage mismatch (VTD shapefiles out of date because states re-precinct more often than they redistrict) as a limitation on their own estimates. | The paper is about vote distributions, not business counts. It does not quantify measurement error from mixing boundary vintages. It only reinforces that boundary changes are not innocuous in states that redrew substantially, which includes the 2020-round redraws and the later mid-decade redraws. The paper offers no evidence about how large the CBP error is. | weak | UNVERIFIED |
| [[becker-nd-gameability-redistricting-criteria]] | n.d. | suggests-extension | On the 10x9 grid, Plan 2 has the best People Change (9.5 percent, tied with Plan 3) but the second-worst Area Change (13.3 percent; Plan 1 is best at 7.8 percent). People, Area and Precinct Change first match each new district to an old one; Fowlkes-Mallows and Variation of Information are matching-free. The county-split measures in Table 1 disagree on the best plan (fewest counties split is Plan 2 with 1; lowest Shannon entropy is Plan 3 at 1.265; lowest Even Splits is Plan 4 at 2.485). | Measurement design transfers. A per-state matching-based or matching-free change measure on CD116 vs CD119 block assignments could size the CBP-ACS vintage error and could replace the label-only flag. Limits: the paper works in precinct or ward units on population, the system weights blocks, and the paper does not say which basis is right. Descriptive only, tested only on Minnesota and Wisconsin. | moderate | VERIFIED-SOFTENED |
| [[mccartan-2022-simulated-redistricting-plans]] | 2022 | method-source | VEST precinct returns are estimated down to 2010 blocks and crosswalked to 2020 blocks, then aggregated to any district plan. Each state's output is a redist_map object plus 5,000 plans: 1,000 precincts x 5 districts x 5,001 plans gives 25,005 rows (p.3-5). | The block-crosswalk recipe could re-aggregate county or ZIP CBP counts onto 119th boundaries, but only with population-weighted allocation. That allocation adds its own error, because establishments are not distributed like population. The paper reports no estimate of that error, so it gives a method only, not an answer to the component's question about measurement error. | weak | UNVERIFIED |

### Census congressional-district metrics

**What it computes.** 489 rows x 25 ACS and CBP metrics at state and district level, built by `census_cd_scraper.R` in Draft Systems. The script sets ACS_YEAR 2024 on 119th-Congress districts and CBP_YEAR 2020 on 116th-Congress districts, then stacks the two. Some new districts come back NA. The boundary mismatch is documented and accepted as tolerable but has never been measured.

**Evidence weight: one moderate VERIFIED-SOFTENED method bearing plus two leads. Nothing answers the size-of-error question.**
- **Becker n.d. (VERIFIED-SOFTENED, moderate, suggests extension).** Supplies plan-change measures that could size the vintage error per state:
  - matching-based People, Area and Precinct Change
  - matching-free Fowlkes-Mallows and Variation of Information
  - county-split measures (count, Shannon entropy, even splits)

  The worked example shows these measures disagree on which plan is best. On the 10x9 toy grid, Plan 2 is best on People Change (9.5%, tied) but second-worst on Area Change (13.3% vs 7.8%). That is a warning against relying on a single change metric. Limits:
  - The units are precinct and ward, on population; Melange weights blocks.
  - It is descriptive and tested only on Minnesota and Wisconsin.
  - The paper does not say which basis is right.

  This transfers as **measurement design**. It could replace the label-only `seat_changed = int(seat_117 != seat_119)` flag in `redistricting_density.py`.
- **McCartan 2022 (UNVERIFIED lead, weak, method source).** Its block-crosswalk recipe (VEST precincts estimated to 2010 blocks, crosswalked to 2020 blocks, aggregated to any plan) could re-aggregate CBP onto 119th boundaries. Allocating establishments by population adds its own error, because establishments are not distributed like population, and the paper does not estimate that error.
- **Best 2022 (UNVERIFIED lead, weak, qualifies).** Redistricting reshuffles the distribution of votes by several points, and stale boundary vintages are a real limitation. The paper is about votes, not business counts. It weakly cuts against treating the mismatch as innocuous in heavily redrawn states, and gives no magnitude.

**Transfer conditions.** All three are redistricting-for-votes papers. None measures error in business or establishment counts across boundary vintages.

**Study that would fill the gap** (a measurement study, not a literature search):
1. Build a CD116-to-CD119 block-assignment overlap for each state.
2. Compute at least one matching-based and one matching-free change measure (Becker), weighted two ways: population, and workplace jobs from LODES WAC at block level. Employment weighting addresses McCartan's population-allocation problem for establishments.
3. Re-aggregate the CBP metrics onto 119th boundaries with the employment weights. Report the absolute and relative change in each metric against the as-shipped 116th values, by state, with the window (CBP 2020; ACS 2024 1-year) and the denominator (districts per state) named.
4. Flag states above a stated tolerance. If a later Census CBP release on newer CD boundaries is available, use it as a ground-truth check.

**Contradictions**
- No bearing contradicts the component. Best 2022 (UNVERIFIED) leans against the stated assumption that mixed-vintage boundaries are acceptable, but only as a qualitative point about vote distributions, with no figure for business counts.

**Gaps**
- No evidence quantifies the measurement error from stacking CBP on 116th-Congress boundaries with ACS on 119th-Congress boundaries. This is the component's own empirical question and remains open. It needs a per-state block-overlap and re-aggregation study weighted by both population and employment (LODES WAC).
- No evidence covers the error introduced by allocating establishment counts across boundaries by population (McCartan's recipe applied to CBP).
- The NA new districts are documented but uncounted in the bearings. The share of affected districts and states should be reported.

**Paul's position vs the evidence.** Paul's position is that this is a data-procedure memo only, with no citations. That fits its role, and nothing in the evidence calls for a literature citation. The working assumption that mixed-vintage boundaries are acceptable is unsupported rather than contradicted: no paper measures the error, and the one leaning bearing (Best 2022) is an UNVERIFIED lead about votes. Becker gives the most usable path, a measurement design that would turn "documented and accepted" into "documented and sized". That is a modest, local check, not a model change.

**Open questions**
- Does Melange's district CBP input (which feeds v3.0's two LQ components, 50% of the default score's weight) come from this 116th-boundary CBP pull or from a separately crosswalked file?
- Which basis should the change measure use: population, blocks or workplace jobs? Becker does not settle it. For CBP metrics, employment weighting looks more appropriate than population, but that needs checking.
- How many of the 489 rows are NA new districts, and in which states?

**Read first:** [[becker-nd-gameability-redistricting-criteria]], [[mccartan-2022-simulated-redistricting-plans]], [[best-2022-redistricting-commissions-avoid]]

## Across components

**1. One citation does triple duty.** Huneeus & Kim 2018 is the only literature Paul attaches to this system. It appears on v2.1 ("state layer") and on v3.0. It also appears elsewhere in Melange as context for small, capped lobbying returns. The only verified reading is narrow: the home-state firm-politician link is an identifying assumption for headquarters, at state level, with first-stage F 14.30 and 13.10 (Table 2; the bearing gives no window or N). It cannot support two score versions that point in opposite directions, and it supports neither at district level. Recommendation: relabel it on both components as design inspiration, not evidential support.

**2. Boundary error probably flows into the default score.** Read from code (`fec/district/demographic_potential.py`):
- v3.0's employment LQ (0.30) and establishment LQ (0.20) both draw on CBP NAICS 522130 at district level (`district_industry_4digit.json`). Half the default score's weight therefore sits on CBP district cells.
- v2.1's CBP contradiction screen and its reach counts are also CBP-based.

If Melange's district CBP file shares the 116th-Congress vintage documented in the Draft Systems scraper, the census-cd-metrics measurement question is also a question about the default score. Whether it does is not verified here; it is the first open question to settle. The Becker change-measure study would size the error for both components at once.

**3. All weights are unestimated, and the evidence keeps them that way.** No bearing supports any weight, threshold or gate in either score version. The pattern is consistent: every paper is about something adjacent (headquarters identification, lobbying practice, grocery access, vote distributions). None measures CU market outcomes or CU-aligned legislative behaviour.

**4. The stakes are bounded.** Demographic Potential does not feed the Demo% pillar input (D-035; that input is a hand-edited workbook column). The evidence gaps limit what the desk's Demographic Potential label, tier and rank can claim. They do not reach the rank blend. Any future validation result should still be worded as association between footprint and behaviour. It is never evidence of access or influence.

**5. Evidence quality.** 6 bearings in total (4 UNVERIFIED, 1 VERIFIED, 1 VERIFIED-SOFTENED; strength: 5 weak, 1 moderate). Nothing here rises above weak support for any component claim. Under DE-062, the four UNVERIFIED bearings (Holyoke, Esmalian, Best, McCartan) are leads to confirm against the source PDFs before anyone cites them.

## Research agenda

1. Settle the plumbing question first (no new research needed). Check which boundary vintage Melange's district CBP input (`district_industry_4digit.json`) uses. If it is the 116th Congress, the boundary mismatch touches 50% of the default v3.0 score's weight.
2. Run the boundary-error measurement study for census-cd-metrics. Build CD116-to-CD119 block overlaps per state; compute at least one matching-based and one matching-free change measure (Becker n.d.), weighted by both population and LODES WAC jobs; re-aggregate CBP onto 119th boundaries; report per-state error with the window (CBP 2020, ACS 2024 1-year) and the denominator (districts per state). Use the result to replace the label-only `seat_changed` flag.
3. Design a preregistered single-look association study for v3.0: district CU footprint at the start of each Congress against member CU-aligned behaviour (cosponsorship of, and roll calls on, bills with public CU trade-association positions). Control for party, DW-NOMINATE, Financial Services seat, bank footprint and urbanity; stratify by party. Add a redistricting-induced footprint-change variant using the existing cu_share_old/new/delta. Word any result as association only, never access or influence.
4. If v2.1 is ever reconsidered as default, run the method doc's own precondition first: a preregistered out-of-sample backtest of the growth-room score against later NCUA membership, shares, loans and branch growth over a stated 3-5 year window. Compare against equal-weight, leave-one-out and size-only baselines, stratified by urban/rural and region.
5. Search the literature for empirical studies of bank and credit-union branch-entry and market-share determinants (does whitespace or concentration predict entry or growth?). There is none in the current corpus, and it is the missing literature for both whitespace (v2.1) and footprint (v3.0).
6. Confirm the four UNVERIFIED leads against their source PDFs (Holyoke 2015, Esmalian 2022, Best 2022, McCartan 2022) before any is cited in a Melange doc. In particular, check that Holyoke's pp. 17-18 targeting language and McCartan's block-crosswalk description are quoted accurately.
7. Housekeeping for Paul to rule on: relabel the Huneeus & Kim 2018 citation on both v2.1 and v3.0 as a state-level identifying assumption (design inspiration), not district-level evidence.
