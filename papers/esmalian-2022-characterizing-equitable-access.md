---
type: paper
title: "Characterizing equitable access to grocery stores during disasters using location-based data"
authors:
  - "Amir Esmalian"
  - "Natalie Coleman"
  - "Faxi Yuan"
  - "Xin Xiao"
  - "Ali Mostafavi"
year: "2022"
venue: "Scientific Reports 12:20203 (Nature Portfolio, open access CC BY 4.0). Received 27 April 2022, accepted 31 October 2022."
doi: "10.1038/s41598-022-23532-y"
category: "Adjacent"
systems:
  - melange-demographic-potential
  - terrier-canvass
components:
  - melange-demographic-potential.footprint-v30
  - terrier-canvass.street-presets-and-turf
paul_annotated: false
paul_markup_count: 0
evidence_status: unverified
source: "[[_sources/esmalian-2022-characterizing-equitable-access.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Downloads\\s41598-022-23532-y.pdf"
sha12:
  - c82f74711258
tags: [paper, adjacent]
---

# Characterizing equitable access to grocery stores during disasters using location-based data

**Amir Esmalian, Natalie Coleman, Faxi Yuan, Xin Xiao, Ali Mostafavi (2022).** *Scientific Reports 12:20203 (Nature Portfolio, open access CC BY 4.0). Received 27 April 2022, accepted 31 October 2022..* [10.1038/s41598-022-23532-y](https://doi.org/10.1038/s41598-022-23532-y). Theme: [[themes/adjacent|Adjacent]].

> [!abstract] Summary
> Esmalian et al. use StreetLight origin-destination trips and SafeGraph store locations to build bipartite networks linking traffic zones to grocery stores in Harris County during Hurricane Harvey (Aug-Sep 2017). The sample is 1,804 zones, 920 stores and 6,052 links. Each zone gets three access measures: redundancy (how many distinct stores its residents visit), rapidity (mean trip time) and proximity (mean trip distance). Each measure is expressed as percentage change from a weekday baseline and clustered with dynamic time warping (DTW).
> 
> In the preparation phase, high-income zones added more distinct stores than low-income zones. During recovery, the cluster that held up better had higher incomes and a lower minority share. The number of stores in a zone barely tracks the change in access: r = -0.06 for stores visited, -0.01 for trip time and -0.03 for trip distance. Food-desert status alone does not separate the groups. The paper's own test finds no flood-extent difference between the recovery clusters (p = 0.33), which undercuts the abstract's flooding claim.
> 
> The design is descriptive: one storm, one county, about six weeks. It reports no effect sizes. As extracted, Table 3 contradicts the text about which cluster is lower-income.
> 
> For Paul's systems this is an Adjacent paper with almost no transfer. It says nothing about lobbying, legislators, persuasion, turnout or influence networks. Its "network degree" counts distinct stores visited; it is not a centrality or influence measure. The only real point of contact: counting facilities on the supply side, or measuring plain distance, is a weak stand-in for the access people actually get. That bears weakly on Demographic Potential v3.0, which scores CU branch share and deposit share as "potential". It bears more weakly still on the Terrier turf planner, which ranks blocks by walking distance. Both transfers are by analogy across domains (grocery stores in a disaster vs CU branches and canvass walks in normal conditions). They are UNVERIFIED as evidence for either component.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** (1) What are the characteristics of access to grocery stores for different sub-populations at different stages of disasters? (2) What is the extent of disparity in access among different sociodemographic groups in different disaster phases? (3) What are the contributing factors to unequal access to grocery stores, and to what extent are facility distribution inequities, such as food deserts, indicative of access disparities? (p.2, lines 106-112)

**Theory:** Access to facilities is treated as three complementary dimensions: redundancy (number of distinct stores visited), rapidity (trip duration, reflecting traffic and road disruption and restoration of system function), and proximity (trip distance). The paper argues that physical-distance and food-desert measures are inadequate in disasters because disruption changes store availability and trip time, and because household capabilities and protective behavior shape access. Framing draws on accessibility and resilience literature (refs 6, 22-28, 33-35). It is not a formal behavioral or economic theory.

**Data:** Unit of analysis: census-tract-aggregated traffic analysis zones (TAZs) as population nodes, linked to grocery points-of-interest (POIs) as facility nodes. Sample: 1,804 TAZs, 920 grocery stores, 6,052 TAZ-POI links in Harris County, Texas (Fig. 2a caption, p.4). Period: StreetLight O-D trips for 1 Aug to 30 Sep 2017. Analysis window 1 Aug to 15 Sep 2017 (p.6). Baseline 1-20 Aug 2017 (weekday-matched). Normal-period clustering window is stated as 5-16 Aug in the text and 7-16 Aug in the Fig. 3 caption, so it is inconsistent. Sources: StreetLight Data (anonymized cell and GPS trips, not individual-level counts reported; the paper cites more than 40 billion records nationally per month); SafeGraph POIs (grocery, specialty food and general merchandise categories, manually filtered); ACS 2015-2019 5-year estimates; FEMA Harvey flood depth grid; USDA Food Access Research Atlas. The StreetLight penetration rate is given as "approximately 23 penetration rate" in the extracted text, which appears garbled and should be verified in the PDF. Data access is restricted by license and code is available on request.

**Method:** Descriptive population-facility network analysis plus unsupervised time-series clustering. (1) Build directed bipartite TAZ-to-POI networks (unweighted; weighted by trip duration; weighted by trip distance) from daily aggregated trips (Table 1, Eqs. 1-3). (2) Compute three TAZ-level indicators: unweighted degree (unique POIs visited), average weighted degree for duration (seconds), and average weighted degree for distance (miles). (3) Express each indicator as percentage change from the weekday baseline, Pc = (M - B)/B (Eq. 4, p.5). (4) Impute gaps with a Kalman filter via R imputeTS after dropping TAZs with 5+ consecutive missing days. (5) Smooth with a 3-day moving average, then run partitional multivariate DTW clustering (dtwclust, R). Cluster number is chosen with COP and modified Davies-Bouldin indices, and the chosen k and the cluster sizes are not reported in the text. (6) Compare sociodemographic and facility characteristics across clusters and income groups (ANOVA, Tukey, chi-square, a two-sample proportion test, and a correlation between number of POIs in a TAZ and each percentage change). Phases: normal (pre-event), preparation (single focal day 24 Aug 2017, the largest percentage change, before landfall on 25 Aug), and impact/short-term recovery (flood-affected TAZs). Flooded TAZ = flood extent above 6.5% of TAZ area (the 75th percentile in Harris County).</method>
<parameter name="identification">None. The study is descriptive and associational. Group comparisons (income tertiles, food-desert status, clusters) and phase comparisons are not causal. The paper uses causal language in places ("disproportionately exacerbated", "flooding disproportionally affects"), which the design does not support. The text mentions a "multivariate analysis" (p.13) but no model, coefficients or specification are reported.

**Identification:** 

**Scope:** Population: residents of Harris County, Texas (Houston metro), identified by home TAZ, who visited grocery stores in August-September 2017. Institutions: grocery stores and supermarkets (including warehouse clubs, supercenters and eating places in the category filter). Era: a single event, Hurricane Harvey (landfall 25 Aug 2017, over 60 inches of rain). Single county, single storm, single month window. The paper's own claim about generalizing beyond this event is not present.

## Key findings

- Normal-period clusters on the three access indicators show spatial clustering, and the three sets of clusters barely overlap. Areas with low redundancy are concentrated in the east and south of the county, and large northern TAZs have poor proximity and rapidity. Numbers: Three clusters per indicator; Fig. 3d shows group counts in parentheses, which are not extracted in the text. (Results, pre-disaster normal period, pp.6-8 (Fig. 3))
- The normal-period cluster with the highest redundancy is described as lower socioeconomic status. This conflicts with the Table 3 values as extracted (see limitations). Numbers: Table 3 extracted: per-capita income for the highest-redundancy cluster C1 = $36,008.19 (starred, significant) vs $28,305.49 and $28,305.94; food-desert share 0.45* vs 0.38 and 0.34; stores per TAZ 0.67 vs 1.03 and 1.87*. (Results p.7 (text) vs Table 3 p.8-9)
- Preparation phase (24 Aug 2017): high-income TAZs had a significantly larger increase in unique stores visited than low-income TAZs (ANOVA and Tukey, alpha 0.05). Lower-income TAZs with long normal-period trips had a disproportionately larger increase in trip duration. High-income TAZs with short normal-period distances had the largest increase in distance. Numbers: No effect sizes, means or test statistics for these comparisons appear in the text, only boxplots. (Results, Preparation, pp.8-9 (lines 479-491; Figs. 4-5))
- Correlation between number of POIs in a TAZ and each percentage change in access indicators during preparation is very weak. The text reads it as a slight association for redundancy only, and no association for duration or distance. Numbers: Visited-POI % change r = -0.06, p = 0.007; duration % change r = -0.01, p = 0.448; distance % change r = -0.03, p = 0.100. (Results, Table 4, p.10)
- Within food deserts, high-income TAZs visited more POIs during preparation without significantly longer trips or distances than low-income TAZs in food deserts. The authors conclude food-desert status alone does not capture preparation access. Numbers: No counts or test statistics for the four income x food-desert groups in the text. (Results, income x food desert, pp.9-10 (lines 630-657; Fig. 5))
- Among flood-affected TAZs during impact and short-term recovery, two access clusters emerge. Cluster 2 shows a larger increase in unique POIs visited and a smaller loss of proximity and rapidity. Cluster 2 has higher income, a lower minority share, a higher chance of food-desert status, and fewer POIs per TAZ. Numbers: Two clusters; cluster sizes not reported in the text. (Results, Impact/short-term recovery, pp.10-11 (lines 658-714; Figs. 6-7))
- Income and food-desert status interact in the recovery clusters. Among cluster 2 TAZs, high-income TAZs are a larger share of food-desert TAZs than low-income ones. This pattern is absent in cluster 1. Numbers: Cluster 2: high-income share in food deserts 0.53 vs low-income 0.25 (p < 0.001). Cluster 1: 0.57 vs 0.47 (p = 0.76). (Results p.11 (lines 715-721; proportion test))
- Flood extent does not differ significantly between the two recovery clusters. The authors argue that diminished access extends beyond flooding and is explained by sociodemographic and facility-distribution characteristics. This works against the abstract's claim that flooding disproportionally affects socially vulnerable populations. Numbers: p = 0.33 for flooding extent, cluster 1 vs cluster 2; flood threshold 6.5% of TAZ area. (Results p.11 (lines 724-728); Abstract p.1 (lines 22-23))
- Discussion conclusion: during recovery, higher-income, lower-minority areas kept higher redundancy with smaller losses in proximity and rapidity. Lower-income, higher-minority areas had longer commutes and fewer store visits. Numbers: None beyond the results above. (Discussion pp.12-13 (lines 752-768))

## Limitations

- Authors' stated: household-level metrics that drive food inaccessibility are complex and are not captured. Future work should add financial ability and household purchasing power (p.13, lines 793-797).
- Authors' stated: the data are licensed from SafeGraph and StreetLight, access is by request only, and code is available from the corresponding author on request (Data and Code availability, p.14).
- Observed, not stated by authors: one storm, one county and one month. The study does not test generalization beyond Harvey or Harris County.
- Observed: the design is descriptive and associational. No causal identification, no multivariate model reported, and no effect sizes or test statistics for the preparation and recovery comparisons (only boxplots and p-values).
- Observed: Table 3 as extracted contradicts the text. The text says the highest-redundancy cluster has lower socioeconomic status and more stores, but the table values show higher per-capita income ($36,008 starred) and fewer stores (0.67). Column alignment may be lost in extraction; verify against the PDF before citing.
- Observed: the abstract says flooding disproportionally affects socially vulnerable populations. The paper's own test finds no significant flood-extent difference between recovery clusters (p = 0.33), so the flooding claim rests on a weaker basis than the abstract states.
- Observed: date inconsistency. The normal-period window is 5-16 Aug in the text and 7-16 Aug in the Fig. 3 caption. The analysis window is 1 Aug-15 Sep in the text but the data run to 30 Sep.
- Observed: the mobility sample is undescribed. StreetLight's penetration rate is garbled in the text, home-TAZ inference is a black box, and the number of TAZs dropped for 5+ missing days is not reported.
- Observed: clustering choices are not reported (chosen k, validity index values, cluster sizes, and sensitivity to the 3-day smoothing and DTW settings).
- Observed: the 6.5% flood threshold (75th percentile) is a single arbitrary cut and is not tested for sensitivity.
- Observed: sociodemographic data are 2015-2019 ACS estimates, which post-date parts of the 2017 event's baseline. Their timing relative to the storm is not discussed.
- Observed: the food-desert and income-tertile comparisons rest on small and unreported subgroup sizes, and the text does not report N per group.

## Short quotes

- "Approaches focused on physical distance, as done in the majority of current studies, is not a reliable measure of accessibility during disasters" (p.2, lines 70-72 (Introduction))
- "in the aftermath of disasters, the socially vulnerable population (low-income and minority groups) has lesser access to grocery stores" (p.10, lines 679-681 (Impact/short-term recovery, Fig. 7))
- "The insignificant difference in flooding shows that the diminishment of access to grocery stores goes beyond the extent of flooding" (p.11, lines 724-727)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-demographic-potential#Demographic Potential v3.0 (CU footprint, default)\|Demographic Potential v3.0 (CU footprint, default)]] | qualifies | How many facilities a zone has is a poor indicator of realized access. Correlation of stores per zone with the preparation-phase % change: stores visited r = -0.06 (p = 0.007), trip time r = -0.01 (p = 0.448), trip distance r = -0.03 (p = 0.100). Window: 24 Aug 2017. Denominator: Harris County zones in the analysis, N not reported. Food-desert status alone did not separate high- from low-income access. (Results, Table 4, p.10; income x food desert, pp.9-10) | The default Demographic Potential (footprint, ruling R7, D-035) scores a district on supply-side presence. Weights: CU employment LQ .30, establishment LQ .20, CU branch share .25, CU deposit share .25. It treats current footprint as the potential and makes no use-based or access-based measure. `fec/district/demographic_potential.py:160-165` | Weak analogy only. The paper covers grocery trips in a disaster in one county, with access measured from mobility traces. Demographic Potential is a national, district-level market score under normal conditions. It is explicitly not a measure of support or access, and the footprint choice is an owner ruling. The paper's lesson (supply counts are not realized use) would matter only if footprint were read as members' or constituents' access to CUs. It does not challenge the owner's choice to treat footprint as potential. The same caution applies to the growth_room branch-whitespace term (demographic_potential.py:152). | weak | UNVERIFIED |
| [[systems/terrier-canvass#Street-plan presets, turf planner and walk routing\|Street-plan presets, turf planner and walk routing]] | context | "Approaches focused on physical distance ... is not a reliable measure of accessibility during disasters". Trip time and trip distance diverged by income group during preparation: lower-income zones with long baseline trips saw the largest increase in trip time, and high-income zones with short baseline trips saw the largest increase in distance. (Introduction p.2 lines 70-72; Results, Preparation pp.8-9) | The turf planner ranks blocks by preset value per 100 m of street length (length floored at 50 m). It grows a turf along connected streets with a 400 m jump allowance and routes on the TIGER street network, so cost is physical distance only. `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/walk_views.py:701-718; walk_turf.py:73` | Carries over only if canvassing happens under disrupted conditions, such as flooding or road closures, when walk time stops tracking distance. For normal door-to-door canvassing on foot, distance is an adequate cost, and the paper offers no evidence against it. This is recorded as context, not a proposed change. | weak | UNVERIFIED |

## Gaps and flags

- Demographic Potential (footprint v3.0 and growth_room v2.1) uses only supply-side measures: branch and deposit share, LQ and whitespace. It has no measure of realized use, such as CU visits or membership per resident, to test whether presence equals access. The paper shows that facility counts track use changes only weakly (|r| <= 0.06). This matters only if footprint is ever read as access.
- No component checks whether its scores differ across sociodemographic strata such as income or minority share. The paper's main finding is heterogeneity by income tertile. Any future use of a pooled district score over income strata should be quoted with those strata or labelled UNSTRATIFIED.
- The paper does not argue for either rejected SNA proposal (dropping regular cosponsorship edges; per-edge volume normalization). Its 'networks' are bipartite zone-to-store trip graphs, and 'degree' means distinct stores visited, not influence, closeness or access probability. Do not port its 'weighted degree' language into Melange SNA wording (D-025).
- The causal language in the paper ('disproportionately exacerbated', 'flooding disproportionally affects') is not supported by its descriptive design, and its own test finds no flood-extent difference between clusters (p = 0.33). Do not cite it for causal claims.
- As extracted, Table 3 contradicts the text: the highest-redundancy cluster shows a higher per-capita income ($36,008, starred) and fewer stores (0.67). Check against the PDF before quoting.
- The normal-period window is inconsistent: 5-16 Aug in the text vs 7-16 Aug in the Fig. 3 caption. Any figure quoted from it must name which window it uses.
- Both bearings are this agent's analogy across domains. They stay UNVERIFIED until a second path reproduces them (DE-062).

Paul has not annotated this paper, and no component lists it among his citations. Nothing in his positions to agree or disagree with, and no stale note to update. His one related idea is the state or industry-presence targeting layer, which became Demographic Potential (Huneeus & Kim). The paper gently cautions that presence differs from realized access. It does not touch the owner ruling that footprint is the potential.

## Related papers

- [[hargittai-2015-bigger-always-better]] — complements — Hargittai documents sampling biases in big data drawn from digital traces. Esmalian rests on StreetLight cell/GPS traces with an undescribed penetration rate and black-box home-zone inference, which is exactly that kind of bias. Read together, this limits how far its findings by income can be trusted.
- [[zeller-2016-step-path-selection]] — complements — Both papers infer movement cost or access from observed movement rather than straight-line distance: Zeller estimates resistance to movement with step and path selection functions, and Esmalian uses trip time vs trip distance. Both are relevant to any travel-cost weighting in the Terrier turf planner.

### Linked from
_None._