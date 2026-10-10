---
type: paper
title: "Conflict-Based Search for Multi-Agent Path Planning Using D*Lite for Dynamic Re-planning: Analysis and Verification"
authors:
  - "Ahmed S. Abdel-Rahman"
  - "S.F. Nafea"
  - "Basem E. Elnaghi"
  - "Ayman A. El-Badawy"
year: "2025"
venue: "Suez Canal Engineering, Energy and Environmental Science Journal, Vol. 3, No. 4, pp. 20-29 (Faculty of Engineering, Suez Canal University). Received 14/04/2025, accepted 05/08/2025, published 30/10/2025."
doi: "10.21608/sceee.2025.375386.1074"
category: "Path-Finding Algorithms"
systems:
  - melange-pathfinding
components:
  - melange-pathfinding.cosponsor-route-pareto
  - melange-pathfinding.member-recruitment-path
  - melange-pathfinding.staffer-closeness-routes
paul_annotated: true
paul_markup_count: 4
evidence_status: unverified
source: "[[_sources/abdelrahman-2025-conflict-based-search.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Downloads\\SCEEE_Volume3_Issue4_Pages20-29.pdf"
sha12:
  - b513fdf30f7a
tags: [paper, path-finding-algorithms, type/paper, theme/path-finding-algorithms, evidence/unverified]
---

# Conflict-Based Search for Multi-Agent Path Planning Using D*Lite for Dynamic Re-planning: Analysis and Verification

**Ahmed S. Abdel-Rahman, S.F. Nafea, Basem E. Elnaghi, Ayman A. El-Badawy (2025).** *Suez Canal Engineering, Energy and Environmental Science Journal, Vol. 3, No. 4, pp. 20-29 (Faculty of Engineering, Suez Canal University). Received 14/04/2025, accepted 05/08/2025, published 30/10/2025..* [10.21608/sceee.2025.375386.1074](https://doi.org/10.21608/sceee.2025.375386.1074). Theme: [[themes/path-finding-algorithms|Path-Finding Algorithms]].

> [!abstract] Summary
> Abdel-Rahman et al. (2025) swap A* for D* Lite as the low-level single-agent planner inside Conflict-Based Search (CBS), a two-level multi-agent path-finding method. They test it on simulated 3D voxel grids with moving or appearing obstacles. In the paper's Table 2 averages, execution time falls from 5.575 s to 0.04 s and nodes explored fall from 2142 to 250.5, while D* Lite paths come out about 0.8% longer (15.73 vs 15.6). The paper labels that change -0.8% and says optimality is kept. The evidence is thin:
> - The number of runs is not reported, and there is no variance or significance test.
> - The 5.575 s average does not match its own Table 1, whose values average to 4.575 s.
> - The node reduction comes entirely from the 4-neighbour case. With 8 neighbours, D* Lite explores more nodes than A* (198 vs 178).
> - The scalability claim rests on one 15-agent case.
> - Nothing in the paper concerns legislators, lobbying or social networks.
> 
> For Paul's systems, the paper is relevant only to path-finding, as a possible design pattern rather than as evidence. D* Lite earns its speed by reusing earlier search work when edge costs change. None of the built routers do that: the cosponsor route finder, the dashboard recruitment chain and the staffer route search each rebuild every search from scratch on a fixed graph. On a 533-member graph a full recompute is already cheap, so the paper's compute savings do not transfer. Its useful idea is conceptual. When a planned recruitment ask fails or a member's stance changes, the route should be repaired, not abandoned. The dashboard's chain-hold figure currently assumes no rerouting at all. Paul's addendum makes D* Lite and LPA* the operational layer, and this paper is too weak to support that choice on its own.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Paul's markup in the PDF

- p1 Highlight: "Abstract sentence (p.20) on CBS weaknesses: path optimality and computational cost in dynamic environments. Paraphrased; yellow highlight." — 
- p1 Highlight: "Abstract sentence (p.20) stating that the study analyzes D* Lite as a replacement for A* in the low-level planner. The extracted highlight text is garbled (fragments interleaved). Paraphrased; yellow highlight." — 
- p1 Highlight: "Abstract sentence (p.20) on D* Lite's incremental replanning, which avoids recalculating whole paths, and the resulting gain in CBS adaptability. Paraphrased; yellow highlight." — 
- p1 Highlight: "Abstract sentence (p.20) on D* Lite reusing parts of existing paths to resolve conflicts faster while keeping path optimality, ending with the simulation speed claim. Paraphrased; green-tinted yellow highlight." — 

## Study

**Question:** Does replacing A* with D* Lite as the low-level single-agent planner inside Conflict-Based Search (CBS) reduce computation time and nodes explored, while keeping path length, for multi-agent path finding in dynamic environments (moving or appearing obstacles, moving agents)?

**Theory:** No formal theory is developed. The paper rests on the two-level CBS framework (high-level conflict tree plus low-level single-agent planner; Sharon et al. 2012) and on D* Lite's incremental replanning, which reuses earlier search results when costs change (Koenig and Likhachev 2002). The D* Lite priority key is written out in Eq. (1) and the one-step lookahead rhs(s) in Eq. (2); Eq. (3) gives each agent's path as a D* Lite call on graph G=(V,E) under CBS constraints. No hypotheses are stated.

**Data:** Simulated 3D voxel-grid multi-agent path-planning scenarios with random start and goal positions and randomized obstacle positions and movement, regenerated for each iteration (Sec. 4, p.24-25). Three environment types are described: Static Maze, Dynamic Open Field (sparse moving obstacles), and Crowded Warehouse (dense, randomly appearing and disappearing obstacles). Unit of analysis: a multi-agent instance (per-scenario run). Number of runs or instances (N) is not reported. The only instance with reported detail is 15 agents (Figs. 3-6, Table 1). The conclusion also cites a 3-agent case and a 6-agent case with no table or figure. No real-world data, no human sample, no time period beyond simulation.

**Method:** Simulation comparison of two low-level planners inside CBS: A* (baseline CBS-A*) versus D* Lite (CBS-D*Lite), each with 4-point (4 directions in 3D) and 8-point (4 plus diagonals) neighbour search. Pseudocode is in Fig. 1 (p.23). Metrics: execution time (s) to compute paths for all agents, nodes explored (expansions by the low-level planner), and average path length per agent (units). Results are reported as means across test scenarios (Table 2) with per-case values for 15 agents (Table 1). Qualitative claims on adaptability, conflict-resolution responsiveness and scalability are drawn from the same runs (p.28). No replication count, variance, confidence intervals or significance tests are reported. Implementation language, hardware and random seeds are not stated.

**Identification:** None in the causal or statistical sense. The design is an algorithm-versus-algorithm benchmark on simulated instances. The two planners are compared on the same scenario type, but the paper does not state that instances were paired across planners, nor how many instances went into the Table 2 averages.

**Scope:** Population: simulated 3D multi-agent path-planning instances (warehouse, swarm, search-and-rescue and autonomous-vehicle settings motivate the work). Institutions: Electrical Engineering and Mechatronics departments at Suez Canal University and German University in Cairo. Era: received April 2025, published October 2025. Not tested on physical robots or real facilities.

## Key findings

- Table 2 averages across test scenarios: execution time falls from 5.575 s (CBS-A*) to 0.04 s (CBS-D*Lite), reported as a 99.2% improvement. Numbers: 5.575 s vs 0.04 s; 99.2%. Check: the mean of the two A* values in Table 1 (8.64 and 0.51) is 4.575, not 5.575. The 99.2% figure holds either way (about 99.1-99.3%). (Section 5, Table 2, p.28)
- Average nodes explored falls from 2142 to 250.5 (reported 88.3% reduction). The reduction is driven by the 4-point case; in the 8-point case D* Lite explores more nodes than A*. Numbers: Table 2: 2142 vs 250.5, 88.3%. Table 1 (15 agents): 4-point 4106 (A*) vs 303 (D*Lite); 8-point 178 (A*) vs 198 (D*Lite). (Section 5, Tables 1 and 2, p.27-28)
- Average path length is slightly longer with D* Lite, although the paper labels this a -0.8% change and says path optimality is maintained. Numbers: 15.6 (A*) vs 15.73 (D*Lite), listed as -0.8%. The arithmetic gives D*Lite paths about 0.8% longer. Table 1: 4-point 19.60 vs 19.53; 8-point 11.60 vs 11.93. (Section 5, Table 2, p.28; Abstract p.20)
- Single 15-agent case: 4-point CBS-A* takes 8.64 s and CBS-D*Lite 0.03 s; 8-point CBS-A* takes 0.51 s and CBS-D*Lite 0.05 s. The 15-agent case is not tied to a named environment type. Numbers: 4-point: 8.64 s vs 0.03 s; 8-point: 0.51 s vs 0.05 s. (Section 5, p.27-28, Figs. 3-6 and Table 1)
- The conclusion cites a 3-agent case (15.24 s vs 0.22 s) and a 6-agent case (874 vs 39 nodes explored). These appear only in the conclusion; no table or figure reports them. Numbers: 3 agents: 15.24 s (A*) vs 0.22 s (D*Lite). 6 agents: 874 vs 39 nodes. (Section 6 Conclusion, p.29)
- Adaptability, conflict-resolution responsiveness and scalability are claimed as advantages, but these are qualitative. No replanning-count measurements, agent-count sweep, or per-environment breakdown is reported. The scalability claim rests on one 15-agent case. Numbers: None reported. (Section 5, p.28 (qualitative subsections); Section 4, p.24-25)

## Limitations


## Short quotes

- "CBS encounters challenges in terms of path optimality and computational cost" (Abstract, p.20 (PDF p.1))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-pathfinding#Cosponsor recruitment route finder (Pareto front)\|Cosponsor recruitment route finder (Pareto front)]] | suggests-extension | Replacing A* with incremental D* Lite inside CBS cuts mean execution time from 5.575 s to 0.04 s (99.2%) and nodes explored from 2142 to 250.5 (88.3%), at about 0.8% longer paths (15.73 vs 15.6). However, the 8-point case explores more nodes with D* Lite (198 vs 178). The number of runs is not reported. (Section 5, Tables 1-2, p.27-28; Abstract p.20) | The route finder is a static greedy builder on the shared build_P kernel. It reruns build_route from scratch for each of the five lambda values and keeps a Pareto front over cost, expected cosponsors and risk. A per-recruiter whip cap of K=5 is the only shared-resource constraint. MOPBD* is named in the docstring as a concept only. The code does no incremental replanning and no repair when an edge or stance changes. `fec/engine/cosponsor_route.py:3-5,23,41-63,70-71` | The speedup does not transfer. It was measured on 3D voxel grids with moving obstacles, while this graph has 533 nodes and recomputing it costs very little. What carries over is the idea of repairing a route when a member joins, declines or changes stance, rather than rebuilding it. The K=5 whip cap loosely resembles a CBS conflict over a shared resource, but CBS resolves conflicts between agents competing for space and time, not recruiter bandwidth. Treat the paper as a design pointer only (weak, single-source, unreplicated, UNVERIFIED). | weak | UNVERIFIED |
| [[systems/melange-pathfinding#Sponsor-to-target max-product path and chain hold\|Sponsor-to-target max-product path and chain hold]] | suggests-extension | D* Lite reuses earlier search results when costs change, so a route can be repaired instead of recomputed. The authors credit this for faster conflict resolution in dynamic settings, but they report no replanning counts. (Abstract p.20 (Paul's highlights); Section 3 Eqs. (1)-(3); Section 5 qualitative subsections p.28) | The dashboard chain uses Dijkstra on -log p. chainHold multiplies activeP over the hops and is documented as P(every ask lands first-try, no reroute), a conservative floor that rerouting could only improve. ASKM=0.07 is an unestimated dial. No reroute-on-failure logic exists. `fec/surfaces/templates/make_dashboard_html_00.tpl:748-753` | The extension would need an observed event: a failed ask or a stance move that changes edge probabilities while a campaign is under way. Only then would incremental repair, or simply recomputing the route, give a reroute-aware chain hold. The paper gives no evidence about recruitment success. Edge p values are heuristic kernel outputs, not ask-success probabilities (D-025 wording applies). | weak | UNVERIFIED |
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | qualifies | Incremental planning is not always cheaper. In the 8-point case, D* Lite expanded 198 nodes against A*'s 178. The paper also does not report node counts for single-shot queries on unchanged graphs. (Section 5, Table 1, p.27) | The router runs bidirectional Dijkstra with heuristic 0 to a virtual sink, plus Yen loopless alternatives. It runs one search per request on a fixed evidence snapshot, with no work caps (exact proof of the optimal route). The latency lane kept one-way Dijkstra without a heuristic and deferred ALT. `fec/surfaces/staffer_closeness_routes.py:12-21,79` | D* Lite pays off only when costs change between successive queries on the same graph. Each staffer request starts on a fixed snapshot, so incremental search buys little unless the sidecar starts updating evidence in place between queries. The current design, exact and stateless, has no gap that this paper fills. | weak | UNVERIFIED |

## Gaps and flags

- No incremental or repair-based replanning in any built router: cosponsor_route.py, the dashboard buildPath/chainHold, and the staffer routes all recompute from scratch. A route is never updated when a planned ask fails or a stance changes during a campaign.
- chainHold explicitly assumes no rerouting, so it understates chain success in any setting where failed hops are re-planned. Nothing models a reroute-aware chain probability.
- There is no multi-agent conflict layer. Several recruiters or lobbyists working the same targets at once (overlapping asks, the same member asked by two chains) are handled only by the per-recruiter K cap, not by a CBS-style search over conflicts.
- Internal numeric inconsistency: Table 2 reports a CBS-A* mean of 5.575 s, but Table 1's values (8.64 and 0.51) average to 4.575 s. The path-length change is labelled -0.8%, yet D* Lite paths are about 0.8% longer.
- The paper is weak as evidence: N is not reported, there is no variance or significance testing, the hardware and seeds are not stated, and scalability rests on a single 15-agent case. The 3-agent and 6-agent figures appear only in the conclusion. Do not quote the 99.2% figure as a general D* Lite speedup.
- The node-reduction result reverses in the 8-point neighbour case (198 vs 178), so the claim that D* Lite uniformly reduces nodes explored is false within the paper's own data.
- Paul's addendum makes D* Lite and LPA* the operational path-finding layer. This paper does not substantiate that choice for a small, static-per-query legislative graph, and any adoption argument built on it stays UNVERIFIED (DE-062) until it is reproduced independently.
- The paper does not argue for either rejected SNA proposal (dropping regular cosponsorship edges, per-edge volume normalization), and it raises no LDA or reciprocity issues.

Paul's four highlights, all on the abstract (CBS cost problems, D* Lite as the A* replacement, incremental replanning, faster conflict resolution), match what the paper claims. His component note is still accurate: only the multi-objective concept was built, and cosponsor_route.py does no incremental replanning (verified at fec/engine/cosponsor_route.py:41-71). His note does not record two things. First, the paper's own evidence is thin and partly inconsistent, including the Table 2 vs Table 1 mismatch and the 8-point reversal. Second, on a 533-node graph the compute savings that motivate D* Lite hardly matter. The part worth keeping is reroute-on-failure, which the dashboard chainHold explicitly leaves out. That supports Paul's interest in dynamic replanning as a modeling idea, but not as a performance requirement.

## Related papers

- [[ren-2022-multi-objective-path]] — same-method — MOPBD* is the multi-objective D* Lite variant cited as concept-only in cosponsor_route.py. Together with this paper it forms the D* Lite pair behind Paul's operational-layer addendum.
- [[luo-2020-path-planning-algorithm]] — same-method — An improved D* Lite for UAV firefighting routes. It is another robotics-only application of the same incremental replanner.
- [[xu-2024-improved-lite-based]] — same-method — An improved D* Lite for dynamic ship routing in Arctic waters. It shares the same method and the same dynamic-obstacle motivation.
- [[chen-2016-most-reliable-path]] — complements — The built recruitment chain runs most-reliable-path Dijkstra on -log p. That paper sets the reliability objective, while this one only addresses replanning compute.
- [[lawande-2022-systematic-review-analysis]] — complements — Its review of A*, D* and related pathfinding families gives the broader context for choosing D* Lite over A*.
- [[duan-2025-breaking-sorting-barrier]] — complements — It is the other speed-oriented path-finding source. It speeds up single-source shortest paths on a static graph, while this paper reuses work when the graph changes.

### Linked from
_None._