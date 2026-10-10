---
type: paper
title: "Validating the Applicability of Bayesian Inference with Surname and Geocoding to Congressional Redistricting"
authors: ["Kevin DeLuca", "John A. Curiel"]
year: 2022
venue: "Political Analysis, vol. 31 (2023), pp. 465-471; published online 20 May 2022"
doi: "10.1017/pan.2022.14"
category: "Causal & Methodological"
systems: ["terrier-imputation", "terrier-row-scoring"]
components: ["terrier-row-scoring.cua-index", "terrier-row-scoring.area-eb-aggregation", "terrier-imputation.luna-rf-knn"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/deluca-2022-validating-applicability-bayesian.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\DeLuca_Curiel_2022.pdf"]
sha12: ["ea669c91615e"]
tags: [paper, causal-methodological]
---

# Validating the Applicability of Bayesian Inference with Surname and Geocoding to Congressional Redistricting

**Kevin DeLuca, John A. Curiel (2022).** *Political Analysis, vol. 31 (2023), pp. 465-471; published online 20 May 2022.* [DOI](https://doi.org/10.1017/pan.2022.14). Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> DeLuca and Curiel test Bayesian Improved Surname Geocoding (BISG) against the race people reported on their own voter-file records. They use the North Carolina and Georgia voter files, Black versus White only, with ZIP-level priors from zipWRUext. They compare two ways of turning each voter's posterior into precinct and district totals. One sums the probabilities (PSM). The other gives each voter the most probable race and counts those (plurality or modal assignment, PM). Across 10,000 simulated congressional plans per state, PSM had lower absolute error in nearly every district. Under PSM the Black share was off by at most about 5 points in both states. The White share was off by about 0 points in North Carolina and up to about 10 in Georgia. At the precinct level, PM had a longer right tail of errors for White voters, and PSM vastly outperformed PM for Black voters. The authors expect errors of 5 to 10 points at the district level. They expect errors to be largest where surnames carry little information and areas are mixed. Their results come from two highly segregated states with two dominant groups.
> 
> None of the Melange federal components estimate race, and none aggregate individual probabilities to areas, so most of the system has no bearing here. The paper matters for Terrier. Terrier's row framework already follows the PSM rule. It takes expected values over imputed class probabilities and never the most probable class (argmax). Area top-decile shares are built from summed probabilities, not flags. This paper is outside evidence for that aggregation rule. A possible exposed spot is the Luna completion step. Whether it fills each blank with the most probable class was not confirmed, and if the area measures read modal-filled codes, they are plurality aggregates, the PM rule the paper argues against. The transfer is about how to aggregate, not about race. It holds best when the class probabilities are reasonably calibrated, and it is UNVERIFIED for vendor consumer fields until an area-level holdout check exists.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Can Bayesian Improved Surname Geocoding (BISG) estimate the racial composition of precincts and congressional districts accurately enough for redistricting, and which posterior allocation rule is better: summing probabilities (PSM) or plurality/modal assignment (PM)? (Abstract p.465; Intro pp.465-466.)

**Theory:** BISG applies Bayes' rule, using a surname-based prior and Census geography conditional probabilities, to give each voter a posterior over racial groups (p.466, lines 93-98). The authors argue that plurality assignment invites extreme, clustered errors and goes against population-level best practice (King 1997), so summing posteriors to a geographic unit should be preferred (p.466, lines 124-125). Their stated expectation is that BISG errors are largest where surnames are uninformative and geographic units are racially heterogeneous (p.466).

**Data:** Benchmark data are self-reported race from the voter files of North Carolina and Georgia, the two states with race in their voter files (p.466). The outcome is the Black share of registered voters, with White as the comparison group (p.467, fn.3). BISG inputs are surname dictionary priors and ZIP-code demographics via the R package zipWRUext (Clark et al. 2021) (p.466). Unit of analysis: individual voters aggregated to precincts (precinct-level error) and to simulated congressional districts (district-level error). Geography: precinct maps (NC from Curiel and Steelman 2018; GA from its precinct shapefile for 2010 congressional districts, with small hole and overlap fixes) (p.467, fn.4). Sample sizes: 10,000 simulated congressional plans per state, 20,000 total (p.466). Precinct count and total voter N for the validation are not stated in the main text. Only illustrative N is given: about 7 million voters in the Georgia voter file, processed in about 10 minutes on a 3.1-GHz MacBook Pro with 8 GB RAM (p.467, fn.2). Period: not stated. Framed for the 2020 redistricting cycle, with GA 2010 districts as the base map (p.467, p.469). Replication data are on Harvard Dataverse with voter-file PII redacted (p.469).

**Method:** Descriptive validation against ground truth. (1) Run BISG with zipWRUext and form two allocations: PSM (sum posteriors within a precinct) and PM (assign each voter to the modal race). (2) Precinct check: compute absolute percentage difference between BISG-estimated and reported counts for White and Black voters, shown as density plots (Fig. 1, p.468). (3) District check: draw 10,000 congressional plans per state with the Redist package v2 using rook contiguity on the precinct base map. For each plan, compute absolute percentage-point difference between BISG-estimated and voter-file share for each race, then take the distribution and a 95% interval (p.467). Results are plotted against district White or Black population share (Fig. 2, p.469).

**Identification:** No causal identification strategy. The design is criterion validation: BISG estimates are benchmarked against self-reported voter-file race at precinct and district level. Uncertainty about district error comes from the spread across 10,000 simulated plans, not from sampling inference. The authors compare PSM and PM on the same data, so the comparison between allocation rules is within-sample.

**Scope:** Population: registered voters in North Carolina and Georgia. Institution: congressional redistricting and VRA-style majority-minority district construction (NC and GA require Black majority or plurality districts, p.466, fn.3). Era: 2010-cycle base maps, applied to the 2020 cycle and to the post-preclearance context (p.469). Race is limited to Black vs White; Hispanic and other groups are excluded (p.467, fn.3; p.469). The authors say results should be more accurate in states with heavy residential segregation and two dominant groups, so they may not transfer to more diverse states (p.469).

## Key findings

- PSM significantly reduces error rates at the precinct and district level relative to plurality assignment; recommended as the preferred method for BISG in redistricting (abstract). (not reported as a single number; Abstract, p.465 (lines 17-20))
- Precinct level: for White voters the modal error approaches zero under both methods, but PM has a longer right tail. For Black voters PSM vastly outperforms PM. (qualitative; no point estimates in main text; Results, p.466 (lines 149-157); Fig. 1 p.468)
- District level: in nearly all district estimates, PSM gives significantly lower absolute error than PM across 10,000 simulated plans per state. (10,000 plans per state; 95% CIs plotted; p.467 (lines 184-190); Fig. 2 p.469)
- North Carolina, PSM: White-share error close to zero; Black-share error never above 5 percentage points. (White about 0 pp; Black max 5 pp; p.467 (lines 191-196))
- Georgia, PSM: White-share error reaches about 10 percentage points; Black-share error is lower and peaks around 5 percentage points. (White max about 10 pp; Black peak about 5 pp; p.467 (lines 194-196))
- Headline district-level accuracy: the paper's figures are for the Black share and the White share separately, in NC and GA. Under PSM, Black-share error is at most about 5 points in NC and peaks about 5 in GA, while White-share error is near 0 in NC and reaches about 10 in GA. Do not generalize to other minority shares. (Intro, p.466 (lines 86-88))
- Practical guidance: aggregate by summing probabilities; assigning race to single voters makes errors drastically higher; expect roughly 5-10 percentage-point district-level error; accuracy is high for White vs non-White coalition districts but lower for specific minority groups. (5-10 pp expected district error; Discussion, p.469 (lines 220-230))
- Cost: ZIP-level BISG gave race estimates for about 7 million Georgia voters in about 10 minutes on a laptop, with no geocoding. (7 million voters; about 10 minutes; 3.1 GHz, 8 GB RAM; p.467 fn.2 (lines 160-163))
- BISG is less accurate for non-Black minorities at ZIP level than with block-level geocoding; errors are greatest where surnames are uninformative and geography is heterogeneous. (not quantified in text; p.466 (lines 96-98, 140-142))

## Limitations

- Only Black and White race categories are analyzed; Hispanic and other minorities are excluded, and Hispanic-as-ethnicity would need a different package (p.467 fn.3; p.469 fn.5).
- Results come from two states with high residential segregation and two dominant groups; the authors expect larger BISG error in more heterogeneous states (p.469).
- BISG uses ZIP-code geography rather than block-level geocoding; the authors concede this is less accurate for non-Black minorities (p.466).
- The paper does not incorporate differential turnout from voter history; this is flagged as future work (p.469).
- Error magnitude varies across states and racial groups, and district errors of 5-10 percentage points should be expected (p.469).
- Precinct-level and district-level claims rest on two states; the main text gives no precinct count, voter N for validation, or time period; these are in Supplementary Material (not in the text file).
- Voter-file self-reported race is treated as ground truth, so any voter-file measurement error passes into the validation (implicit; p.466).
- Replication data have voter PII redacted (p.469).
- Reference-list inconsistency: the text cites Lu et al. (2019, 465) for statistical packages that default to plurality assignment (p.466), but the reference list gives Lu et al. 2019 as a paper on scientific writing styles, which does not match the claim.

## Short quotes

- "probabilistic summing of the BISG posteriors significantly reduces error rates at the precinct and district level relative to plurality racial assignment" (Abstract, p.465)
- "researchers using BISG should aggregate up to some polygonal unit of interest by summing the estimated probabilities" (Discussion, p.469)
- "researchers should be prepared to deal with around a 5-10 percentage point error rate in estimating race at the district level" (Discussion, p.469)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/terrier-row-scoring#CUA (Credit Union Alignment) index\|CUA (Credit Union Alignment) index]] | supports | Summed BISG posteriors (PSM) give lower absolute error than modal assignment (PM) in nearly all districts. In NC, PSM Black-share error never exceeds about 5 pp; in GA it peaks about 5 pp. PSM White-share error is near 0 in NC and reaches about 10 pp in GA. (Lines 184-196 (district results); Abstract lines 17-20; Discussion lines 220-224. Item page labels are unreliable; cite lines.) | engine.py lines 264-270 (categorical_map branch) compute mu = P @ m as the expected value and variance P @ (m*m) - mu^2 for estimated categorical fields. The draw spec in the same branch samples from the probability columns, not argmax. `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/rowfw/engine.py:264-270` | The paper validates area-level aggregates of voter-level race posteriors for Black vs White in two segregated states. CUA is a person-level expected value for vendor consumer fields. Support for the person-level index is indirect and holds only where the class probabilities are roughly calibrated; calibration is the open ECE question. | weak | VERIFIED-SOFTENED |
| [[systems/terrier-row-scoring#Area aggregation with empirical-Bayes shrinkage\|Area aggregation with empirical-Bayes shrinkage]] | supports | Researchers should aggregate to a polygonal unit by summing estimated probabilities of racial membership. Assigning race to single voters makes errors drastically higher. (Lines 220-223 (Discussion). Figure 1 caption at lines 172-176.) | areas.py eb_top10_params (lines 435-449): area top-decile share is np.bincount of summed P(top10) contributions divided by row count (line 438), followed by beta-binomial EB shrinkage with a design-effect variance (lines 444-448). `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/rowfw/areas.py:436-440` | The summed-probability step matches the paper's recommended aggregation. The paper does not test EB shrinkage or top-decile shares, so it says nothing about the shrinkage layer. Its expectation of larger error in heterogeneous areas is where shrinkage matters most. This is an analogy for the aggregation choice, not a validation of Terrier area values. | moderate | VERIFIED |
| [[systems/terrier-imputation#Luna stratified RF-KNN completion\|Luna stratified RF-KNN completion]] | qualifies | Modal assignment gives a longer right tail of precinct error for White voters and higher district error than summed probabilities in nearly all simulated districts. For Black voters, PSM vastly outperforms PM at precinct level. (Lines 152-157 (precinct results); lines 188-190 (district results)) | models.py lines 386-393 score Luna candidates by argmax accuracy against a modal majority baseline (confirmed). measures.py lines 197-220 count literal codes with raw.eq(code) over valid-code denominators (confirmed). The step that fills each blank with the most probable class was not located in the lines read. The 78 percent majority baseline was not found in the code. `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/tools/luna_district/models.py:386-393; C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/tools/measures.py:197-220` | Applies only if modal-filled codes reach an area-level share. Whether the v3 ingest reads the Luna-completed file remains unconfirmed. The rare-class under-count is a hypothesis; the paper measures bias for race only, not for vendor consumer fields. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary says PM had a longer right tail especially for Black voters. Line 153 gives the longer right tail for White voters; for Black voters, line 154 says PSM vastly outperforms PM.
> - Summary issue: Page labels appear off by one under the running-footer layout: Intro lines 86-88 fall on p.466 not p.465; Discussion lines 212-230 fall on p.469 not p.468; Fig. 1 caption (line 176) falls on p.468; Fig. 2 caption (line 210) falls on p.469. Fn 2 and fn 3 (lines 160-166) fall on p.467. Line numbers are reliable.
> - Summary issue: Summary's Luna claim (fills each blank with the most probable class) was not confirmed in the lines read; the claim that Luna's majority baseline is about 78 percent was not found in the code. Both remain UNVERIFIED.
> - Summary issue: Ext headline says PSM typically within 5 pp for minority share; the paper's figures are for the Black share and the White share separately, in NC and GA. Do not generalize to all minority shares.
> - Summary issue: Paul alignment claims (recorded position No literature; notes not stale; first outside evidence in the vault) were not checked; this item has no paul_entries or markup, and the vault was not searched.
> - Summary issue: Lu et al. 2019 citation mismatch is confirmed: line 128 cites Lu for packages that default to plurality, but the reference at line 317 is a paper on scientific writing styles.
> - Summary issue: Codex-path code references resolved, but a grep of the luna_district folder returned hits under C:/Users/pgoff/Documents/Terrier/outputs, so the Codex path may be a link to the Terrier tree. Confirm which is canonical before citing.

## Gaps and flags

- Terrier has no area-level criterion validation of aggregated imputations. The paper's design is to compare summed estimates against ground truth by area. Its Terrier analogue would mask observed sci_* values within blocks or tracts and compare the summed-probability share with the modal-fill share. That would give an error distribution like the paper's Fig. 1.
- Nothing records whether the cu-geography-v3 area measures (measures.py) read observed-only codes or Luna's modal fills. The methodology text says 'literal valid codes' but does not say whether imputed values are included.
- Terrier does not report area-level error by area heterogeneity. The paper expects the largest errors in mixed areas, which are also where EB shrinkage toward the parent has the most effect.
- There is no check of how rare classes are counted in aggregates. The paper's largest PM penalty falls on the minority class, so rare-code shares are where Terrier aggregates would be most biased.
- No conflict with the brain rules: the paper does not argue for dropping regular cosponsorship edges or for per-edge volume normalization, and it makes no claims about graph rank, LDA or reciprocity.
- Scope flag: the validation covers Black vs White only, in NC and GA (segregated, two dominant groups). The authors say it may not transfer to more diverse states. Do not cite its 5-10 pp error band as an expected error for Terrier districts (CO, OR-05, CT-01) or for non-race attributes.
- Sensitive-attribute caution: no Terrier or Melange component infers race. If anyone proposes BISG for Terrier, it would add inferred race to voter rows, which is outside the current design. Any such proposal is for Paul to decide, not something this paper recommends.
- Citation defect in the paper: the reference list gives Lu et al. 2019 (cited for packages that default to plurality assignment) as a paper on scientific writing styles. Do not rely on that citation.
- The claim that modal fills reach the v3 area shares is unconfirmed.

Paul has no annotations on this paper. The summed-probability aggregation rule already built into Terrier agrees with the paper's recommendation. The one possible tension is Luna's fill step: the paper argues against modal assignment for aggregates, and whether those fills feed area shares was not confirmed.

## Related papers

- [[best-2022-redistricting-commissions-avoid]] — complements — Real item (American Politics Research 2022 on commissions and partisan gerrymanders). Link is thin: DeLuca supplies a racial-composition measure, and no direct analytic dependency was checked. Weak link.
- [[becker-nd-gameability-redistricting-criteria]] — complements — Real item on the gameability of redistricting criteria. Both papers are in redistricting. The shared ensemble-of-plans method was not verified in Becker and so the same-method label is withdrawn.

### Linked from
- [[mccartan-2022-simulated-redistricting-plans]] — complements — DeLuca validates BISG race estimation for redistricting. McCartan's VRA constraint relies on district minority VAP shares, the measurement DeLuca's method addresses.