---
type: paper
title: "Simulated redistricting plans for the analysis and evaluation of redistricting in the United States"
authors: ["Cory McCartan", "Christopher T. Kenny", "Tyler Simko", "George Garcia III", "Kevin Wang", "Melissa Wu", "Shiro Kuriwaki", "Kosuke Imai"]
year: "2022"
venue: "Scientific Data 9:689 (2022)"
doi: "10.1038/s41597-022-01808-2"
category: "Campaign Politics"
systems: ["melange-demographic-potential", "melange-influence-engine"]
components: ["melange-demographic-potential.census-cd-metrics", "melange-influence-engine.competitiveness-multiplier"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "unverified"
source: "[[_sources/mccartan-2022-simulated-redistricting-plans.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\s41597-022-01808-2.pdf"]
sha12: ["b4df69e16e12"]
tags: [paper, campaign-politics]
---

# Simulated redistricting plans for the analysis and evaluation of redistricting in the United States

**Cory McCartan, Christopher T. Kenny, Tyler Simko, George Garcia III, Kevin Wang, Melissa Wu, Shiro Kuriwaki, Kosuke Imai (2022).** *Scientific Data 9:689 (2022).* [10.1038/s41597-022-01808-2](https://doi.org/10.1038/s41597-022-01808-2). Theme: [[themes/campaign-politics|Campaign Politics]].

> [!abstract] Summary
> McCartan, Kenny, Simko, Garcia, Wang, Wu, Kuriwaki and Imai (Scientific Data 9:689, 2022) is a data descriptor for the 50stateSimulations dataset. For each state's congressional map it gives 5,000 simulated 2020-cycle plans, drawn with the Sequential Monte Carlo sampler in the redist R package. The hard constraints are contiguity, at most 0.5 percent population deviation, and county splits capped at districts minus one. Soft constraints cover compactness and matching the enacted plan's count of minority-opportunity districts. Precinct-level VEST election returns are crosswalked onto 2020 Census blocks. The enacted plan is read as a possible gerrymander when it is an outlier against this ensemble. In the Georgia example (14 districts, 20,000 plans sampled and thinned to 5,000), the enacted 12th and 14th districts have 2020 presidential Democratic two-party shares above the simulated range, which suggests packing. The paper tests no substantive hypothesis. It warns that its partisan metrics depend on which statewide elections exist in each state and cannot be compared across states. Two authors disclose paid expert-witness work. For Paul's systems the bearing is narrow. Nothing here covers lobbying, PAC money, networks or persuasion. The work touches the system only where district geography enters. (1) The competitiveness multiplier c uses a single Cook PVI point per seat. It flags the 10 mid-decade-redistricted states but never adjusts them. The dataset could supply a map-dependence baseline, but only for 2020-cycle maps, not the 2025-26 redraws. (2) The census district metrics mix 119th-Congress ACS with 116th-Congress CBP boundaries. The paper's precinct-to-block crosswalk is a reusable template for re-aggregation. It does not measure the error that this mismatch causes. Both bearings are weak. Paul has no annotations on this work. The analysis here is UNVERIFIED (DE-062).

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** How can analysts run a complete, reproducible redistricting simulation analysis for the congressional districts of all 50 states, and what do simulated 2020-cycle plans show about enacted maps? The paper is a data descriptor: it documents the dataset, code and validation rather than testing a substantive hypothesis (p.1-2).
**Theory.** Gerrymandering is detected by treating an enacted plan as an outlier relative to a sample of alternative plans that satisfy the same statutory guidelines, on partisan bias metrics (p.2, citing Katz, King and Rosenblatt 2020).
**Data.** Precinct-level geography per state: 2020 Census PL 94-171 population and VAP by race, joined with VEST precinct election results (estimated to 2010 blocks, crosswalked to 2020 blocks), municipality boundaries, enacted 2010 and 2020 plans. Output: 5,000 simulated congressional plans per state, all 50 states, on Harvard Dataverse. Georgia example: 14 districts, 20,000 plans sampled and thinned to 5,000; lean uses 2016-2020 statewide results (p.3-5). Precinct count not stated.
**Method.** Sequential Monte Carlo (SMC) algorithm from the redist R package (McCartan and Imai 2021). Hard constraints: contiguity; population deviation at most 0.5 percent (about 3,826 people in Georgia); county or municipality splits capped at districts minus one. Soft constraints: default compactness, untuned in most states; minority opportunity districts matched to the enacted plan (Georgia: soft 52 percent BVAP target). Most states combine two samples of 2,500 (p.3-4). Diagnostics: resampling weights, variation of information, bottleneck checks, Gelman-Rubin R about 1.05 or less (p.7).
**Identification.** No causal identification strategy. The inference is an outlier comparison between the enacted plan and the simulated reference distribution. The authors state that a result from this comparison alone is not conclusive evidence of partisan gerrymandering (p.8-9).
**Scope.** Congressional districts of all 50 states, 2020 redistricting cycle, with plans intended for use in 2022 elections (p.2-4). Plans should not be compared across states, and partisan summary statistics are not directly comparable across states because statewide races differ (p.5, p.8). Legal criteria per state are in Supplementary Table 1 (not in the main text).

## Key findings

- Georgia 2020 enacted plan: two of its 14 ordered districts (12th and 14th) have 2020 presidential Democratic two-party vote shares above the range of the 5,000 simulated plans; the authors suggest packing may exceed what traditional criteria require (p.8, Fig. 2).
- Remaining Georgia enacted districts fall within the simulated range on 2020 presidential Democratic share (p.8, Fig. 2).
- Georgia: 20,000 plans sampled, then 5,000 kept by random sampling after convergence diagnostics passed (p.3, p.4).
- Population deviation limit of 0.5 percent corresponds to about 3,826 people in Georgia; precinct-level units are used rather than blocks (p.3).
- Compactness is not tuned to the enacted map in most states; Florida and North Carolina final simulations are less compact than the enacted maps, while Illinois simulations are more compact (p.6-7).
- Georgia sampler diagnostics: Gelman-Rubin R of about 1.05 or less is the convergence threshold for all summary statistics; no numeric variation-of-information value for Georgia is reported in the text (p.7-8).
- VRA check: every simulated plan must have at least as many minority opportunity districts as the enacted plan, using an effects-based definition based on Chen and Stephanopoulos (p.6-7).
- Dataset output: each state has documentation, a redist_map file, a redist_plans file and a summary-statistics CSV for 5,000 plans per state (p.4-5).
- Output size example: 1,000 precincts, 5 districts, 5,000 plans plus one reference plan gives 25,005 rows in redist_plans (p.5).
- Figure 1 shows 9 of 5,000 Georgia simulated plans next to the enacted plan, coloured by 2016-2020 average partisan lean (p.5).

## Limitations

- Simulations are not a legal determination; the enacted plan is assumed to comply with the VRA and the authors do not judge legality (p.6-7, p.9).
- Incumbency pairing was not implemented for Georgia, though courts may require it (p.2, p.9).
- Units are precincts, not Census blocks, so the 0.5 percent deviation tolerance is coarser than a block-level plan (p.3).
- Compactness defaults are not tuned to the enacted plan because compactness can disguise a gerrymander; this is a stated design choice (p.6-7).
- The VRA approach is effects-based and is one of several possible methods; it does not say how many VRA districts a state should have (p.6-7).
- Partisan metrics depend on which statewide elections exist in each state, so cross-state comparisons of partisan statistics are not valid (p.5, p.8).
- Some states need extra soft constraints, subsetting or redefined administrative units; the Georgia 52 percent BVAP target is a team choice, not a legal threshold (p.3-4).
- SMC can get stuck in a few plans that repeat many times; the diagnostics reduce but do not rule this out (p.6-7).
- Internal inconsistency: the abstract describes ensembles of simulated 2020 plans, while the Background refers to a sample of simulated 2022 plans (p.1-2).
- Competing interests: C.T.K. was a paid expert for the Maryland Redistricting Commission; K.I. was a paid expert witness in 2020-cycle cases in Alabama, Kentucky, Ohio and South Carolina (p.10).

## Short quotes

- "Alternative plans should not be compared across states (p.8)."

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Competitiveness multiplier c (COMPET_MULT)|Competitiveness multiplier c (COMPET_MULT)]] | suggests-extension | District partisanship is a function of the map. In Georgia the enacted 12th and 14th districts have 2020 presidential Democratic two-party shares above the range of 5,000 simulated plans (p.8, Fig. 2). Partisan metrics depend on the statewide elections available in each state, so cross-state comparisons are not valid (p.5, p.8). (p.5-8, Fig. 1-2; Measures section (ndshare, e_dvs, pr_dem)) | c = 1 + 1.0 x exp(-max(lean,0)/5.0), where lean is one Cook PVI point oriented to the member's party. AL CA FL LA MO NC OH TN TX UT are FLAGGED as mid-decade redistricted, never altered. A separate flag fires when the 2025-to-2026 PVI shift is 5 or more points. Retirees take their 2025 seat PVI through ballot overrides. c carries no uncertainty term. `fec/moneyball_core.py:83-93; fec/engine/build_competitiveness.py:50-52` | The ensembles cover 2020-cycle maps only. They do not include the 2025-26 mid-decade redraws that the system flags, and they describe counterfactual neutral maps, not the realized seat a member runs in. At most they could add a per-district map-sensitivity band or a cross-check of Cook lean against ndshare. Per the paper, ndshare is not comparable across states. Reciprocity and the c amplitude/scale stay unvalidated (D-104..D-108). This is not evidence for them. | weak | UNVERIFIED |
| [[systems/melange-demographic-potential#Census congressional-district metrics|Census congressional-district metrics]] | method-source | VEST precinct returns are estimated down to 2010 blocks and crosswalked to 2020 blocks, then aggregated to any district plan. Each state's output is a redist_map object plus 5,000 plans: 1,000 precincts x 5 districts x 5,001 plans gives 25,005 rows (p.3-5). (p.3-5, Methods and Data Records) | The scraper pulls ACS 2024 1-year on 119th-Congress districts and CBP 2020 on 116th-Congress districts and stacks them. Some new districts come back NA. The boundary mismatch is documented but never quantified. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/census_cd_scraper.R:47-48; census_cd_scraper.R:350` | The block-crosswalk recipe could re-aggregate county or ZIP CBP counts onto 119th boundaries, but only with population-weighted allocation. That allocation adds its own error, because establishments are not distributed like population. The paper reports no estimate of that error, so it gives a method only, not an answer to the component's question about measurement error. | weak | UNVERIFIED |

## Gaps and flags

- Competitiveness c treats Cook PVI as an exact point, with no band for uncertainty or map dependence. The 10 redistricted states are flagged but not adjusted, and no alternative lean source (for example ndshare from statewide elections) is cross-checked.
- No ensemble or baseline exists for the 2025-26 mid-decade maps. Public 50-state ensembles stop at the 2020 cycle.
- The census district metrics mix 119th-Congress ACS with 116th-Congress CBP boundaries. The resulting measurement error has never been estimated, and no crosswalk re-aggregation exists.
- Competing interests: C.T.K. was paid by the Maryland Redistricting Commission, and K.I. was a paid expert witness in Alabama, Kentucky, Ohio and South Carolina cases (p.10). Weigh the gerrymander-detection framing accordingly.
- The paper is internally inconsistent: the abstract says simulated 2020 plans, while the Background says 2022 plans (p.1-2).
- Do not quote any partisan statistic from this dataset across states. Per the paper, each state's partisan metrics rest on different statewide elections. Any figure must name its state, its election window and its 5,000-plan denominator.
- No conflict with D-021, D-025 or the two rejected SNA proposals. The paper does not touch networks or LDA.

Paul has no annotated-bibliography entries or highlights for this work (n_annots 0), so there is no takeaway to compare and nothing is stale. The closest live system choice is Paul's 2026-10-05 ruling to flag, never alter, the mid-decade-redistricted states in the competitiveness build. The paper neither contradicts nor validates that ruling. Its ensembles predate those redraws.

## Related papers

- [[becker-nd-gameability-redistricting-criteria]] — same-method — Both use ensembles of algorithmically sampled districting plans as the neutral baseline for judging maps and criteria.
- [[best-2022-redistricting-commissions-avoid]] — complements — Best asks whether commissions avoid partisan gerrymanders. McCartan supplies the outlier-versus-ensemble benchmark such an evaluation needs.
- [[deluca-2022-validating-applicability-bayesian]] — complements — DeLuca validates BISG race estimation for redistricting. McCartan's VRA constraint relies on district minority VAP shares, the measurement DeLuca's method addresses.
- [[harrison-2010-introduction-monte-carlo]] — builds-on — The SMC sampler and its convergence diagnostics (Gelman-Rubin R of about 1.05 or less, resampling weights) extend the basic Monte Carlo sampling that Harrison introduces.

### Linked from
_None._