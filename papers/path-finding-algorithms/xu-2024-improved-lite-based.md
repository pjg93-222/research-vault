---
type: paper
title: "An Improved D* Lite-Based Dynamic Route Planning Algorithm for Ships in Arctic Waters"
authors: ["Tie Xu", "Haoran Yang", "Jun Ma", "Kun Xiong", "Qinyou Hu"]
year: "2024"
venue: "Journal of Marine Science and Engineering, 12, 2323 (MDPI, Basel); published 18 Dec 2024"
doi: "10.3390/jmse12122323"
category: "Path-Finding Algorithms"
systems: ["melange-influence-engine", "melange-pathfinding"]
components: ["melange-pathfinding.member-recruitment-path", "melange-pathfinding.staffer-closeness-routes", "melange-influence-engine.scenario-runner", "melange-pathfinding.cosponsor-route-pareto"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/xu-2024-improved-lite-based.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\jmse-12-02323-v2.pdf"]
sha12: ["3a84bc118f9c"]
tags: [paper, path-finding-algorithms, type/paper, theme/path-finding-algorithms, evidence/verified]
---

# An Improved D* Lite-Based Dynamic Route Planning Algorithm for Ships in Arctic Waters

**Tie Xu, Haoran Yang, Jun Ma, Kun Xiong, Qinyou Hu (2024).** *Journal of Marine Science and Engineering, 12, 2323 (MDPI, Basel); published 18 Dec 2024.* [DOI 10.3390/jmse12122323](https://doi.org/10.3390/jmse12122323). Theme: [[themes/path-finding-algorithms|Path-Finding Algorithms]].

> [!abstract] Summary
> Xu et al. (2024) change D* Lite so that a ship's route can be re-planned cheaply as Arctic ice changes. The test grid is 448 x 304 cells at 25 km. They make three changes: (1) a local-update rule, where a cell that becomes blocked keeps its old cost and a cell that opens takes its new cost, so changes don't spread across the whole map; (2) a path-extraction rule that picks the unexplored successor with the lowest c+g, which avoids deadlocks at the edge of updated regions; (3) shortcut smoothing. Cost is a weighted sum, delta*risk + distance.
> 
> Compared with the original D* Lite, nodes updated fell 34.9% in the freezing period (18,468 vs 28,351) and 79.5% in the melting period. Planning time fell 46% and 86%. Route length stayed within about 0.1%, and average risk was unchanged. Raising delta trades length for risk (freezing period: R 0.076 to 0.039, distance +3.4%). Each period is one simulated voyage, with no variance and no hardware stated. Some numbers in the text don't match the tables: 8 cells x 25 km is about 108 n mi, not 432, and the 0.07% error figure can't be reproduced.
> 
> For Paul's systems the paper is narrow. None of the router code (fec/) has D* Lite or LPA*. The gains need large graphs where repeated queries see small, local changes. The 533-node cosponsor graph is too small for incremental replanning to matter, and the staffer router serves many different origin/target pairs. Its approximate local update would also conflict with an exact-cost requirement for staffer routes (the 2026-10-03 exact-tie ruling is unverified from the inputs). The paper's sharpest use is diagnostic. Its warning that extracting a path from stale cost-to-goal values can deadlock describes the dashboard's buildPath "member dropped" mode: distToTarget ignores dropped members, but tracePath skips them, so the chain labelled "PROVABLY-OPTIMAL" is not optimal once someone is dropped (UNVERIFIED, read from the code but not run). Separately, scenario.py's D* Lite citation reads as a reuse claim that the code doesn't implement; that is a Melange-side docstring point, not a finding of the paper.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** How can D* Lite dynamic replanning be made cheap enough for real-time ship routing in Arctic waters, where ice conditions change over large areas, while keeping route distance and average risk close to full re-planning? The authors set the goal as cutting node updates and planning time without changing the heuristic.

**Theory.** No social-science theory. The basis is graph-search path planning: incremental replanning from LPA* and D* (Koenig and Likhachev 2002, ref. 26), a risk-weighted movement cost (cost = delta*r + d, Eq. 12), and a ship-specific icebreaking limit (maximum continuous ice-breaking thickness) used in place of ice-class rules.

**Data.** Sea ice thickness and related ice data from CryoSat-2 (European Space Agency), stated in Sec. 4 (data availability: NSIDC, accessed 29 April 2024). Study area 55-90 degrees N, grid 448 x 304 cells at 25 km x 25 km. Unit: grid cell (node) and route update. Two simulated voyages, one per period: freezing (October 2023 dataset, 26 route updates) and melting (mid-April to mid-May 2023, 24 route updates). No ship sample. Ship parameters are a single hypothetical vessel with icebreaking thickness 1.2 m (freezing) or 2.8 m (melting), risk threshold 1, and a fixed update distance of 8 grid units per update. Text says this is "approximately 432 nautical miles", which does not match the stated grid resolution (see limitations).

**Method.** Improved D* Lite with three modified rules. (1) Local update (Sec. 2.2, Eq. 5, Algorithm 1): a node that becomes an obstacle takes its cost from the old data, and a node that becomes navigable takes its cost from the new data, so updates do not spread through the whole map. (2) Path extraction (Sec. 2.3, Eq. 6, Algorithm 2): the next node is the unexplored successor minimizing c + g, which avoids deadlocks at the boundary of updated regions. (3) Path optimization (Sec. 2.4, Eqs. 7-9): shortcut any segment where the direct connection costs less and has no obstacles. Risk r = h x C / Icemax (Eq. 10); cost = delta*r + d (Eq. 12); average route risk R = sum(r_i d_i)/sum(d_i) (Eq. 13). Non-navigable cells are land, cells with average ice thickness above Icemax, and cells with r above the threshold (Sec. 3.1). Benchmarks: global re-planning every update, and the original D* Lite. All three run on identical ice snapshots and ship positions.

**Identification.** 

**Scope.** Population: one Arctic region, two ice periods (freezing October 2023; melting April-May 2023), one ship type parameterized by icebreaking thickness and safety coefficient. Institution: Shanghai Maritime University (Merchant Marine College). Era: 2023 ice data, paper published Dec 2024. Speed differences by ice condition are excluded (Sec. 4, p. 13).

## Key findings

- Freezing period, improved vs original D* Lite: nodes updated fell by 34.9% (18,468 vs 28,351); planning time fell by 46.2% (924 s vs 1,720 s). Voyage distance 2,558.7 vs 2,561.4 n mi; average risk identical at 0.076. (Sec. 4.1, Table 3, p. 15)
- Freezing period, improved vs global re-planning: nodes updated fell 74.6% and planning time fell 72.7%. Route quality matched global re-planning on risk, with distance within about 0.1%. (Sec. 4.1, Table 3, p. 15)
- Melting period, improved vs original D* Lite: nodes updated fell 79.5% and planning time fell 85.7%. Voyage distance 2,419.1 vs 2,418.1 n mi; risk identical at 0.615. (Sec. 4.2, Table 4, p. 17)
- Melting period, improved vs global re-planning: nodes updated fell 85.3% and planning time fell 86.7%. (Sec. 4.2, Table 4, p. 17)
- Sensitivity, freezing period: raising Icemax from 1.0 to 1.4 m shortens the route (2,654.5 to 2,459.9 n mi) and raises average risk (0.002 to 0.113). Raising delta from 1 to 3 at 1.2 m lengthens the route (2,558.7 to 2,645.5 n mi) and lowers risk (0.076 to 0.039). (Sec. 4.3, Table 5, p. 18-19)
- Sensitivity, melting period: delta 1 to 3 at 2.8 m lengthens the route (2,419.1 to 2,644.1 n mi) and lowers risk (0.615 to 0.468). Risk does not fall monotonically with Icemax: at delta 1 it is 0.625, 0.631, 0.615 for 2.4, 2.6, 2.8 m. (Sec. 4.3, Table 5, p. 18-19)
- Safety coefficient delta = 3 is adopted as sufficient on the basis of simulation experiments the paper does not show: higher values did not change route outcomes much. (Sec. 3.2, p. 12)
- Discussion, single illustrative map (Icemax 0.8 m, PC-6-equivalent): the POLARIS-based route crosses non-navigable cells, while the proposed risk formula routes around them. Qualitative only, no numbers. (Sec. 5, Fig. 16, p. 19)
- Conclusion claims planning-efficiency gains are directly proportional to ice-data completeness. Only two periods were tested, so this is an untested claim. (Sec. 6, p. 20)

## Limitations

- Authors: ship speed varies with ice and is not modelled; the update distance is fixed at 8 grid units per update (Sec. 4, p. 13).
- Authors: forecast uncertainty and floating-ice drift can make a route invalid, so local obstacle avoidance is future work (Sec. 5, p. 20).
- Authors: POLARIS was not used because it works from ice type and ice class, not thickness and maximum icebreaking thickness (Sec. 5, p. 19).
- Authors: ice thickness data are unreliable in rapidly changing months, and no average-thickness data were released June-September, so only two windows were studied (Sec. 4, p. 13).
- Mine: one simulated voyage per period with no repetition, variance, or confidence intervals; timing claims rest on single runs.
- Mine: no hardware or software specification is given for the timings in Tables 3-4.
- Mine: the text's 0.07% voyage/risk error figures do not reproduce from Tables 3-4 (voyage deviation is 0.10% freezing vs original and 0.04% melting vs original; risk is identical).
- Mine: the text says 8 grid units equal about 432 n mi, but 8 x 25 km = 200 km, about 108 n mi.
- Mine: the text says risk rises with Icemax, but in the melting sweep at delta 1 risk goes 0.625, 0.631, 0.615, which is not monotone.
- Mine: the conclusion's proportionality claim about ice-data completeness rests on two periods only.
- Mine: the abstract's 'gradual and convergent' ice-change premise is asserted, not tested.

## Short quotes

- "achieves planning results very similar to those of the original D* Lite algorithm" (Abstract, p. 1)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-pathfinding#Sponsor-to-target max-product path and chain hold\|Sponsor-to-target max-product path and chain hold]] | qualifies | Paper says greedy path extraction after map changes can deadlock where a predecessor cost value exceeds its successor value (Fig. 3b). Its fix (Eq. 6, Algorithm 2) picks the unexplored successor minimizing passage cost plus g and excludes explored nodes. The 18,468 vs 28,351 freezing node counts compare the whole improved method (local update plus extraction plus path optimization) with the original D* Lite, not Eq. 6 alone. The paper does not say the g values are out of date or made consistent with the change. (Sec. 2.3, Eq. 6, Algorithm 2, Fig. 3, pp. 6-8; Table 3, p. 15) | Dashboard buildPath runs one max-product Dijkstra from the target (distToTarget). distToTarget does not check the dropped set, while tracePath skips dropped members. When members are dropped, dT still counts routes through them, so the greedy trace can be suboptimal or return reached=false. The code comment labels the chain PROVABLY-OPTIMAL. With nothing dropped the extraction is exact. `fec/surfaces/templates/make_dashboard_html_00.tpl:734 (distToTarget, no dropped check); :735 (PROVABLY-OPTIMAL comment); :740 (tracePath skips dropped); buildPath near :753-757` | The mechanism is general graph search and does not depend on ice data, so it applies to the 533-node cosponsor graph whenever the member-out scenario is active. The paper's timings (single voyage, no hardware stated) do not transfer. Fix is a full recompute that honors dropped, which is cheap at this size. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | qualifies | Paper's local-update rule (Eq. 5) cuts node updates by 34.9% (freezing) and 79.5% (melting) against the original D* Lite and planning time by 46.2% and 85.7%. Route length differs from full re-planning by about 0.1% (2,558.747 vs 2,561.408 n mi freezing; 2,419.087 vs 2,418.146 n mi melting). The paper gives no optimality guarantee; it reports error below 0.1% against the global re-planning routes. (Sec. 2.2, Eq. 5, Algorithm 1; Sec. 4.1-4.2; Tables 3-4, pp. 15-17) | Staffer router is an exact bidirectional Dijkstra with heuristic 0. Ties are costs within 1e-12 relative, broken by fewer steps. Results are cached under a key that includes the graph revision, so a revision change forces a fresh search. No incremental reuse exists. `fec/surfaces/staffer_closeness_routes.py:1-21 (exact search and tie docstring); :79 (ALGORITHM string); fec/surfaces/staffer_routing.py:386 (heuristic 0); :567-572 (revision-keyed cache)` | Incremental replanning helps when the same origin and goal are re-queried after small local edge changes on a large graph. Staffer requests use different origins and targets, so the paper's gains do not transfer directly. The approximate update would conflict with an exact-cost requirement. | weak | VERIFIED |
| [[systems/melange-influence-engine#Scenario runner (what-if)\|Scenario runner (what-if)]] | qualifies | Paper reports node updates of 18,468 vs 72,835 (freezing) and 16,864 vs 114,929 (melting) for its improved D* Lite against full global re-planning. Numbers are correct. The paper frames reuse of earlier search data as the motivation (Sec. 2.1) but does not say D* Lite family planners save work only by reuse. (Sec. 2.1 (p. 5); Tables 3-4, pp. 15-17) | scenario.py docstring (line 13) cites Ren et al. Multi-Objective Path-Based D* Lite for recompute only what the scenario touches. The code uses common random numbers (cascade, around :42-52) and recomputes spread and flip value for every category (:103-115). Isolation comes from CRN, not from reusing earlier search state. `fec/engine/scenario.py:13 (citation); :42-52 (CRN cascade); :103-104 and :113-115 (per-category recompute)` | Documentation point only. At 533 members and MC=40 a full recompute is cheap, so incremental reuse is not warranted. The Xu numbers say nothing about the cascade. The cited work is Ren et al., not Xu. | weak | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Cosponsor recruitment route finder (Pareto front)\|Cosponsor recruitment route finder (Pareto front)]] | context | Paper uses cost = delta*r + d (Eq. 12). Raising delta from 1 to 3 at Icemax 1.2 m (freezing) lowers average risk from 0.076 to 0.039 and lengthens the route from 2,558.7 to 2,645.5 n mi (+3.4%) (Table 5). Paper calls delta = 3 sufficient based on simulation experiments it does not show. The text does not call them unpublished. The paper shows no Pareto front. (Sec. 3.2, Eq. 12, p. 12; Table 5, pp. 18-19) | cosponsor_route.py builds greedy routes with a lambda risk penalty (value = (gain - lambda(1-p))/cost) over lambda in {0, 0.5, 1, 2, 4}, then keeps non-dominated (cost, spread, risk) points. It is not a shortest-path search. The file header cites MOPBD* (conceptual). `fec/engine/cosponsor_route.py:40-63 (greedy lambda route); :64-76 (non-dominated filter over lambda sweep); file header (MOPBD* citation)` | The two works share only the weighted-sum risk cost. The system reports a front while the paper fixes one delta. Weighted-sum scalarization can miss non-convex front points in both. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: summary_md says the paper shows scenario.py's D* Lite citation overclaims reuse. The paper never mentions scenario.py. That is a Melange-side finding, not a paper finding.
> - Summary issue: paul_alignment says Paul marked D* replanning Very interesting and that his addendum made D* Lite the operational layer. Neither is in the inputs (paul_entries and paul_markup are empty). Unverified; remove or source it.
> - Summary issue: Related link lawande-2022 is wrong. That paper compares Dijkstra, A*, BFS and Greedy Best First on video-game grids; it is not a D*-family survey. Dropped.
> - Summary issue: Related link luo-2020 why-text said its local-replanning changes parallel Xu's. Its changes are map zooming and spline smoothing. Corrected in related.
> - Summary issue: Bearing 4 says unpublished simulations. The paper says only that simulation experiments showed delta 3 near-peak (Sec. 3.2, p. 12). Corrected.
> - Summary issue: The paper's 0.07 percent error figures do not reproduce. Tables 3-4 give distance gaps of 0.10 percent (freezing, same for original and global) and 0.04 percent (melting). Quote the table-derived values only.
> - Summary issue: Paper says 8 grid units is about 432 n mi. At 25 km per cell 8 units is about 200 km, or about 108 n mi. Internal inconsistency in the paper; not resolved.
> - Summary issue: Paper timing reductions round differently from its tables: 924 vs 1,720 s is 46.3 percent and 924 vs 3,392 s is 72.8 percent; the text says 46.2 and 72.7. Rounding only.
> - Summary issue: Paper's conclusion says planning efficiency rose 35 percent, but 34.9 is the node-update reduction and 46.2 the planning-time reduction. Do not repeat it as an efficiency figure.
> - Summary issue: Paper text says risk rises with Icemax (Sec. 4.3), but its own Table 5 melting sweep at delta 1 gives 0.625, 0.631, 0.615. Rel flags this correctly.
> - Summary issue: Paper's proportionality claim (gains proportional to ice-data completeness) rests on two periods. Rel flags it as untested, which is correct.
> - Summary issue: Not verifiable from inputs: the 2026-10-02 ALT deferral, the 2026-10-03 exact-tie ruling as Paul's rule, and the 4.66M overlap-pair count (not found in staffer_routing.py). Treat as unverified.
> - Summary issue: Repo search for D* Lite or LPA* implementations in fec/ found none. Output was capped at 40 lines, but no D* implementation names appeared. The no-incremental-replanning claim holds on this evidence.
> - Summary issue: Dashboard finding (distToTarget ignores dropped) is code-confirmed but not run. UNVERIFIED under DE-062 until reproduced.
> - Summary issue: Paper has no hardware specification and reports one voyage per period with no variance. Rel's limitation list is correct on both points.

## Gaps and flags

- Dashboard recruitment-path 'member dropped' mode: distToTarget (make_dashboard_html_00.tpl:734) ignores the dropped set, while tracePath (:740) skips dropped members. The chain labelled 'PROVABLY-OPTIMAL' (:735) can then be suboptimal or falsely unreachable. The simple fix is to honor dropped in distToTarget (a full recompute is cheap at N=533). UNVERIFIED until reproduced by running the dashboard.
- No incremental replanning (D* Lite or LPA*) exists anywhere in fec/ routing code; a search of fec/*.py, *.tpl and *.js found only citations. The only place it could plausibly pay off is a standing staffer route re-checked after a graph revision (the ALT deferral of 2026-10-02 could not be verified from the inputs).
- Melange-side, not a paper finding: scenario.py credits D* Lite-style 'recompute only what the scenario touches', but recomputes every category. The mechanism is CRN isolation, and the docstring could say so.
- None of the system's route objectives has a sensitivity table like the paper's Table 5 (route outcome as the risk weight changes). The cosponsor Pareto sweep comes closest, but ASKM (0.07) and HOP_FACTOR are not swept.
- Flag: Do not port the paper's local-update rule (Eq. 5) into the staffer router. It is approximate (about 0.1% length deviation, no optimality proof) and would conflict with the exact-cost, 1e-12 relative tie rule in the router (the 2026-10-03 ruling itself is unverified from the inputs).
- Flag: The dashboard optimality finding (dropped members ignored in distToTarget) is new agent analysis from reading the code.
- Flag: scenario.py:13 cites D* Lite reuse that the code does not perform (Melange-side docstring issue; the paper does not mention scenario.py).
- Flag: Some of the paper's numbers are unreliable: '8 grid units = 432 n mi' (should be about 108 n mi), the 0.07% error figure can't be reproduced from Tables 3-4, timings are single runs with no hardware stated, and the claim that risk is monotone in Icemax fails in the melting sweep. Quote its percentages only as single-voyage simulation results.
- Flag: The paper says nothing on SNA, LDA, reciprocity or influence wording, so no brain wording rules are triggered.


## Related papers

- [[ren-2022-multi-objective-path]] — same-method — Verified: Multi-Objective Path-Based D* Lite (IEEE RA-L 2022). Its use in scenario.py and cosponsor_route.py is at docstring level only.
- [[luo-2020-path-planning-algorithm]] — same-method — Verified: improved D* Lite for forest-firefighting UAVs. Its changes are map zooming and spline smoothing, not local replanning, so the parallel to Xu is only that both modify D* Lite for a specific domain.
- [[abdelrahman-2025-conflict-based-search]] — same-method — Verified: D* Lite as the low-level planner inside CBS for multi-agent dynamic replanning. Shares the incremental-replanning base, with a multi-agent setting.
- [[chen-2016-most-reliable-path]] — complements — Verified: most reliable path maximizes on-time arrival probability. The dashboard comment at make_dashboard_html_00.tpl:735-739 cites it for the max-product chain.
- [[duan-2025-breaking-sorting-barrier]] — complements — Verified: from-scratch directed single-source shortest paths. It is the opposite strategy to Xu's reuse of earlier search state.

### Linked from
- [[lawande-2022-systematic-review-analysis]] — complements — A D* Lite variant with a dynamic route-planning simulation benchmark, filling the replanning-evidence gap that Lawande only discusses.