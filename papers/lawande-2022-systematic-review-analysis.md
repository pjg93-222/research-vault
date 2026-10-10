---
type: paper
title: "A Systematic Review and Analysis of Intelligence-Based Pathfinding Algorithms in the Field of Video Games"
authors:
  - "Sharmad Rajnish Lawande"
  - "Graceline Jasmine"
  - "Jani Anbarasi"
  - "Lila Iznita Izhar"
year: "2022"
venue: "Applied Sciences 12(11), 5499 (MDPI). Received 25 Mar 2022; accepted 24 May 2022; published 28 May 2022."
doi: "10.3390/app12115499"
category: "Path-Finding Algorithms"
systems:
  - melange-pathfinding
  - terrier-canvass
components:
  - melange-pathfinding.cosponsor-route-pareto
  - melange-pathfinding.member-recruitment-path
  - melange-pathfinding.staffer-closeness-routes
  - terrier-canvass.street-presets-and-turf
paul_annotated: true
paul_markup_count: 68
evidence_status: verified
source: "[[_sources/lawande-2022-systematic-review-analysis.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Path-Finding Algorithums\\applsci-12-05499-v3.pdf"
sha12:
  - f585bac36b56
tags: [paper, path-finding-algorithms]
---

# A Systematic Review and Analysis of Intelligence-Based Pathfinding Algorithms in the Field of Video Games

**Sharmad Rajnish Lawande, Graceline Jasmine, Jani Anbarasi, Lila Iznita Izhar (2022).** *Applied Sciences 12(11), 5499 (MDPI). Received 25 Mar 2022; accepted 24 May 2022; published 28 May 2022.* [10.3390/app12115499](https://doi.org/10.3390/app12115499). Theme: [[themes/path-finding-algorithms|Path-Finding Algorithms]].

> [!abstract] Summary
> Lawande et al. (2022) call the paper a systematic review, but it is really a benchmark of four algorithms on one setup: BFS, Dijkstra, A* and Greedy Best First (GBF), each run once on Unity square grids from 4x4 to 32x32, with and without obstacles. It reports no search protocol, no repeated trials, no hardware details and no measured memory, so its memory rankings and the Table 5 citations (several of which do not match the claims) cannot be relied on. The usable results are narrow. With no obstacles, GBF is fastest at every size; Dijkstra is 2.8x to 21.6x slower than GBF, and all four find the same path length. With obstacles, GBF stays fastest from 8x8 up, but its paths are longer at 16x16 and 32x32 (37 vs 31, 81 vs 75), while at 8x8 the table shows the reverse (13 vs 15), unexplained. A* expands fewer nodes than Dijkstra on the larger grids (587 vs 754 iterations at 32x32) and returns the same path length as Dijkstra and BFS in this single run (the paper never verifies optimality independently). D* and D* Lite are only described, never tested. The proposed GRIN_PF is just a loop that runs the four existing algorithms.
> 
> What this means for the system as built is mostly a check on design choices, not new evidence. The one sharp bearing comes from the paper's GBF-with-obstacles result. The dashboard's recruitment path first runs an exact max-product Dijkstra backward from the target (`distToTarget`), then walks forward greedily, choosing at each hop the neighbour with the highest edge probability times its precomputed reliability to the target. That walk is optimal only while the precomputed reliabilities are exact. Once a user drops members, the walk skips them but the reliabilities are still computed through them. The walk may then become a greedy best-first search on an out-of-date estimate (single-path code reading, UNVERIFIED per DE-062), the kind of case where the paper's GBF paths were longer at 16x16 and 32x32. Terrier's walk router already does textbook A* with an admissible straight-line estimate, which is the setting where the paper's A* result applies best. For the non-spatial social graphs, the paper offers no usable heuristic.

## Paul's annotation

From [[annotations/Path-Finding Algorithums - Path-Finding Algorithums - Annotated Bibliography|Path-Finding Algorithums - Path-Finding Algorithums - Annotated Bibliography]]:

> Lawande, Sharmad Rajnish, et al. “A systematic review and analysis of intelligence-based pathfinding algorithms in the field of video games.” *Applied Sciences*, vol. 12, no. 11, 28 May 2022, p. 5499, [https://doi.org/10.3390/app12115499](https://doi.org/10.3390/app12115499).

## Paul's markup in the PDF

- p1 Highlight: "informed, uninformed, and metaheuristic." — 
- p1 Highlight: "s Dijkstra’s, A-star, Breadth First Search, and Greedy Best First." — 
- p1 Highlight: "pathfinding; Breadth First Search; A-star; Dijkstra’s; Greedy Best First; Unity" — 
- p1 Highlight: "Enemy game-objects may use AI for developing a mechanism to outsmart the player and win the game." — 
- p1 Highlight: "Pathfinding refers to the concept of finding the optimal path from source node to destination node in the least time." — 
- p1 Highlight: "They may also use AI for pathfinding." — 
- p1 Highlight: "Moreover, since often they need to compute paths for multiple components and the resources allocated to these Algorithms are limited, there is a demand for Algorithms with high performance in less execution time." — 
- p1 Highlight: "two groups: static and dynamic" — 
- p2 Highlight: "Some of these pathfinding Algorithms may take various obstacles into consideration and some may not." — 
- p2 Highlight: "Another example of uninformed Algorithms is the Dijkstra’s Algorithm which can be also used in graphs to find the shortest path but only for positive costs" — 
- p2 Highlight: "Informed search Algorithms are better optimized as compared to uninformed search Algorithms." — 
- p2 Highlight: "Heuristic function tries to search for a solution that is most near to the best solution in the shortest time [" — 
- p2 Highlight: "hereby saving significant amount of time. Some examples include A*, D*, HPA*, etc." — 
- p2 Highlight: "Behavior Tree, Decision Tree," — 
- p2 Highlight: "static and dynamic pathfindin" — 
- p4 Highlight: "0 form a graph A grid i form a graph" — 
- p4 Highlight: "A grid is a connection or a network of vertices or points through edges in order to A grid is a connection or a network of vertices or points through edges in order to" — 
- p4 Highlight: "Waypoints, mesh navigation, and visibility graph come under irregular system. In this paper, pathfinding Algorithms in gaming have been patterns can be of triangular, hexagonal, square, or cubic falling under ries." — 
- p4 Highlight: "patterns of grid system. experimented and analyz 2D or 3D categories. Way ular patterns of grid syst experimented and analyz" — 
- p5 Highlight: "These grids are the least popular when it comes to pathfinding in games." — 
- p5 Highlight: "Here, the smallest unit is an equilateral triangle connected using one side and vertices." — 
- p5 Highlight: "These are the most widely used grids to represent graphs" — 
- p5 Highlight: "The graphs formed using this grid system requires less search time and memory complexities as compared to those formed from the octile system." — 
- p6 Highlight: "nt types and its categories of grid along with its smallest unit such as equi- : Waypoints have a number of applications mainly in robotics as well as The different al triangle, sq Waypoints: W pathfinding." — 
- p6 Highlight: "qui- dge that The different types and its categories of grid along with its smallest unit such as lateral triangle, square, regular hexagon, cube, navigation marker, graph node, and" — 
- p6 Highlight: "have been applied to Euclidean distance calculations for shortest paths when obstacles are present" — 
- p6 Highlight: "A mesh can be formed from different shapes, commonly triangles and other polygons." — 
- p6 Highlight: "Most of the applications of this graph are present in games" — 
- p7 Highlight: "The main objective of these Algorithms was to only get the least distance within a graph without taking into consideration the performance optimization to reduce execution time and avoid obstacles." — 
- p7 Highlight: "All the Algorithms present in this category work on the concept of blind search, i.e., they try to reach the goal using brute force by not knowing anything about the search space." — 
- p9 Highlight: "sed in a variety of applications such as digital mapping services such phone networks, IP routing for finding Open Shortest Path First, ro- Then, vertex 5 will be visited which is the remaining node of vertex in a graph [26]." — 
- p9 Highlight: "drawbacks of this Algorithm is that it cannot be ding to acyclic graphs and therefore the correct t was also used in traffic information systems in" — 
- p9 Highlight: "used for negative value costs, thereby leading to acyclic graphs and therefore the corr hortest path cannot be found [32,33]. For each node in a graph, a label is assigned wh order to track source and destinations from another specific source" — 
- p9 Highlight: "other nodes of the ecreases; that is, the There is no heuristic" — 
- p9 Highlight: "Algorithm runs sequentia of the heuristic function" — 
- p9 Highlight: "graph [34]. After each step, the value of the label of the graph nodes decreases; that is, the Algorithm runs sequentially [35]. function used in this Algorithm, but it is extended to the A-star Algorithm by making use" — 
- p10 Highlight: "Therefore, the shortest path will be A to D to E to C with minimum cost of 7 n’, and heuristic value H(n), which is the acceptable cost to move from pre- arget node, are assigned to 0 for Dijkstra’s Algorithm 2." — 
- p10 Highlight: "Informed Search or Heuristic Approach Algorithms Heuristic Algorithms are basically optimized techniques that are used to find the" — 
- p10 Highlight: "vertices S_DJK[] Heuristic Algorithms are basically optimized techniques that are used to find the solution to a particular problem more speedily and efficiently as compared to the basic" — 
- p10 Highlight: "vertices S_DJK[] [] = ϕØ, where s is the sou conventional Algorithms" — 
- p11 Highlight: "of the most famous Algorithms used in the gaming industry due to its Published in 1986 by Hart, Nilsson, and Raphael, the main objective of this search for the most efficient solution from start node to the end node [44]." — 
- p11 Highlight: "generally make use of this Algorithm due to its accuracy and high ng to various categories, such as racing, real-time strategy, RPG (role- enerally make use of this Algorithm due to its accuracy and high" — 
- p11 Highlight: "F(n) = H(n) + G(n) F(n) = H(n) + G(n)" — 
- p12 Highlight: ". As the name suggests, the Algorithm 4 will always try to pick the best track at that particular moment" — 
- p12 Highlight: "where the environment maybe unknown or partially unknown." — 
- p12 Highlight: "here the environment model was considered to be accurate and complet" — 
- p12 FreeText: "(no covered text; free-text box placed in the D* node-state passage)" — Very interesting
- p12 Highlight: "environment would change suddenly without any warnings or there exists information which may be incomplete or even non-existent" — 
- p12 Highlight: "require a method that has the capability of fast and efficient re-planning helping them to move more intelligently in a time of critical situations or difficult terrains" — 
- p12 Highlight: "There are several different states of nodes such as NEW, RAISE, LOWER, OPEN, and CLOSED" — 
- p12 Highlight: "D* or “Dynamic A*”" — 
- p13 Highlight: "The RAISE state refers to the fact that the cost of the node was higher than the previous time it was kept on the open list" — 
- p13 Highlight: "f (q) = minq′∈Pred(q)(g(q′) + c(q′, q)) if q = s otherwise" — 
- p13 Highlight: "0" — 
- p13 Highlight: "The Algorithm makes use of the concept of divide and conquer approach in order to reduce the memory usage and complexity" — 
- p13 Highlight: "The technique used by JPS is based on avoiding useless searches of symmetrical paths that A* searches, saving lots of time with the same memory load." — 
- p13 Highlight: "To keep going with JPS, it is important to understand the concept of “pruning”, which is the elimination of nodes to expand" — 
- p13 Highlight: "“natural neighbors”, which are the only ones that we want to conside" — 
- p14 Highlight: "consider one or two nodes that are not natural. Those are called In Jump Point Search, our objective is to avoid symmetric paths by" — 
- p14 Highlight: "A-star" — 
- p14 Highlight: "F(n) = H(n) + G(n)" — 
- p14 Highlight: "T(n) = O(bd)" — 
- p14 Highlight: "S(n) = O(bd)" — 
- p14 Highlight: "meta and heuristic." — 
- p15 Highlight: "Unlike the heuristic Algorithms em. In general, they are not gree In general, they are not gree n of the solution (example, the ore more thoroughly the solu" — 
- p15 Highlight: "Their main objective a nearly good soluti ey are much better" — 
- p15 Highlight: "solution (that sometimes will coincide with the global optimum) [73]. Their main objective is to make use of the search space as much as possible to discover a nearly good solution to" — 
- p15 Highlight: "is to make use of the searc an optimization problem. o an optimization prob erms of performance and" — 

## Study

**Question:** Which of four pathfinding algorithms (Dijkstra, A*, Breadth First Search, Greedy Best First) performs best on 2D square grids of size 4x4 to 32x32, with and without obstacles, in terms of execution time, total iteration blocks, computed blocks and shortest path length, so that game developers can choose one? Secondary: how do the families compare in the literature on memory and time (Table 5).

**Theory:** Taxonomy only. Pathfinding is split into uninformed (BFS, Dijkstra), informed/heuristic (A*, Greedy Best First, D* Lite, HPA*, JPS) and metaheuristic (GA, ACO) families, and static vs dynamic. The paper argues heuristic methods save time by pruning the search space (Sections 2, 3.2). No formal model is stated beyond the cost functions F(n)=G(n)+H(n) and the Table 2 complexity expressions.

**Data:** Synthetic game-style grids generated in the Unity engine: 2D square (octile) grids of 4x4, 8x8, 16x16 and 32x32 cells, each run with and without obstacles. Unit = one algorithm run on one grid configuration. Reported cells: 4 algorithms x 4 sizes x 2 conditions = 32 algorithm-by-grid results (16 rows in each of Tables 3 and 4). Not reported: number of repetitions, start and goal placement, obstacle layout or density, the iteration count k, hardware, and run dates. Literature input for Section 3 and Table 5: 95 references, span 1959 to 2022 (count of papers actually screened not stated). No real-world dataset.

**Method:** Implemented BFS, Dijkstra, A* and Greedy Best First in Unity (Algorithms 1-4; the "GRIN_PF" Algorithm 6 is a harness that loops over these four). Per run recorded: total iteration blocks, total computed blocks excluding the shortest path, execution time in ms, and shortest path length in blocks (Section 5, p.19-20). Results compared as within-size rankings and narrative (Section 6, p.23-26). Theoretical side: asymptotic time/space table (Table 2, p.14) and a pairwise literature comparison table with category, time, memory, implemented yes/no, and cited source (Table 5, p.26-27). No statistical tests, confidence intervals, or repeated timings. Memory was not measured.

**Identification:** None. Descriptive benchmark: the same grids are fed to each algorithm and the outputs are ranked. There is no causal design, no controls for hardware or run-to-run variance, and no repeated trials. Theory section is a taxonomy (uninformed / informed-heuristic / metaheuristic), not a tested hypothesis.

**Scope:** Population: single-agent pathfinding on regular 2D square grids in a game-style environment. Dynamic obstacles and 3D, hexagonal, triangular or irregular (waypoint, visibility graph, navmesh) grids are discussed but not tested. Institutions: VIT Chennai (India) and Universiti Teknologi PETRONAS (Malaysia); funded by YUTP grant 015LC0-134. Era: article 2022; cited literature 1959-2022. Not related to lobbying, elections, legislators, persuasion/turnout or text measurement.

## Key findings

- Without obstacles, Greedy Best First is fastest at every grid size; Dijkstra is slowest at every size. Numbers: GBF / Dijkstra execution time (ms): 4x4 14.38 / 39.70; 8x8 36.01 / 177.08; 16x16 97.90 / 899.27; 32x32 346.60 / 7500.14. (Section 6, p.23-24; Table 3 p.20-21; Figure 15 p.24)
- The Dijkstra-to-GBF time gap widens with grid size (computed from Table 3 values, not stated by the authors). Numbers: Dijkstra/GBF ratio: 2.8x (4x4), 4.9x (8x8), 9.2x (16x16), 21.6x (32x32). (Table 3 p.20-21 (reviewer ratio))
- Without obstacles, GBF returns the same shortest path length as the other three algorithms at every size, yet the text calls GBF 'not optimal'. The open-grid table shows no length penalty. Numbers: Shortest path length (blocks) for all four algorithms: 5, 13, 29, 61 for 4x4, 8x8, 16x16, 32x32. (Table 3 p.20-21; text p.24 and p.25-26)
- GBF expands almost no nodes without obstacles; BFS and Dijkstra have identical iteration counts at every open-grid size. Numbers: Computed blocks without SPL, GBF: 1, 1, 1, 1. Dijkstra: 9, 49, 225, 961. BFS: 9, 49, 225, 961. Total iterations for Dijkstra and BFS both 14, 62, 254, 1022. (Table 3 p.20-21)
- Dijkstra and BFS execution times differ by only 0.7% to 15% despite identical iteration counts; no repeated trials are reported, so these gaps cannot be separated from timing noise. Numbers: Dijkstra vs BFS (ms): 4x4 39.70 vs 33.76 (15%); 8x8 177.08 vs 164.49 (7.1%); 16x16 899.27 vs 868.56 (3.4%); 32x32 7500.14 vs 7449.17 (0.7%). (Table 3 p.20-21 (reviewer computation); text p.23-24 says 'almost similar')
- With obstacles, GBF is fastest at 8x8, 16x16 and 32x32; A* is fastest at 4x4, where all four are within about 2 ms. Numbers: 4x4: A* 12.92, BFS 13.90, GBF 14.25, Dijkstra 15.17 ms. 8x8: GBF 40.19, A* 91.35, Dijkstra 102.60, BFS 120.54 ms. 16x16: GBF 298.61, A* 512.45, BFS 663.76, Dijkstra 743.47 ms. 32x32: GBF 1396.73, A* 3975.34, BFS 5316.65, Dijkstra 5678.25 ms. (Table 4 p.22-23; Section 6 p.24-26)
- With obstacles, GBF path length is longer at 16x16 and 32x32, but shorter than the optimal-family result at 8x8; the 8x8 result is not discussed and is inconsistent with Dijkstra and A* returning optimal lengths on unit-cost grids. Numbers: Shortest path length: 8x8 GBF 13 vs Dijkstra/A*/BFS 15; 16x16 GBF 37 vs 31; 32x32 GBF 81 vs 75. (Table 4 p.22-23)
- With obstacles, A* expands fewer nodes than Dijkstra at 16x16 and 32x32 but more at 8x8. Numbers: Total iterations A* vs Dijkstra: 8x8 32 vs 30; 16x16 140 vs 177; 32x32 587 vs 754. (Table 4 p.22-23)
- The memory conclusions in Section 6 and Table 5 are not measured. Memory is inferred from computed-block counts or taken from cited literature. Section 5 lists the measured parameters and memory is not among them. Numbers: Measured: iterations, computed blocks, time (ms), path length. Memory: none. (Section 5 p.19-20; Section 6 p.24-26; Table 5 p.26-27)
- The headline ranking (GBF fastest and least memory; A* second; BFS third; Dijkstra worst, with 'on average' used without weighting) rests on one run per cell. Numbers: Single run per algorithm-grid cell reported in Tables 3-4. (Section 6 p.24-26; Conclusion p.27)
- Table 5 cites sources whose reference-list entries do not match the claims. Examples: the D* Lite vs A* memory claim cites [93] (a 2017 ambulance-routing paper) but gives 2009; BFS vs DFS cites [90] (a 2022 healthcare Q-learning paper); Dijkstra vs GA cites [91] (a 2021 Mars-landing paper). The text dates A* to 1986 but cites [38], a 1968 IEEE paper. Numbers: Refs [90]-[95] are not about pathfinding (healthcare, Mars landing, UAV, ambulance routing, metabolic pathfinding, vehicle routing). (Table 5 p.26-27; Section 3.2.1 p.11; reference list p.28-30)
- The 'GRIN_PF outperforms existing approaches' claim is not tested. Algorithm 6 is a loop running the four existing algorithms; no GRIN_PF-specific result appears in Sections 5-6. Numbers: No GRIN_PF timing or path numbers in any table or figure. (Section 4 p.18-19; Conclusion p.27)
- Authors' remark on the open problem: dynamic obstacles and realistic movement remain unsolved; they suggest sensor-equipped agents and runtime algorithm switching, without testing either. Numbers: None. (Section 6 p.26 (pre-processing and splines); Conclusion p.27)

## Limitations

- Stated by authors: dynamic obstacles would need 'much effort' to handle reactively (Conclusion, p.27); the study uses a 2D square grid only (Abstract; Section 2.1, p.4); the authors say GBF is 'not optimal' (p.24, p.26); realism of movement and pre-processing issues are discussed (p.26) but not tested.
- Observed: no repetitions, variance, or timing protocol; hardware and Unity version unreported; start/goal placement, obstacle layout and density, and iteration count k are not specified.
- Observed: memory is never measured, though Section 6 and Table 5 rank algorithms by memory.
- Observed: the 'systematic review' has no reported search protocol, databases, inclusion or exclusion criteria, or paper counts.
- Observed: Table 5 mixes implemented comparisons with literature-derived comparisons and cites sources that do not match the claims (see key findings).
- Observed: GRIN_PF is presented as the proposed method, but it is only a harness; the conclusion's claim that it outperforms existing approaches is untested.
- Observed: the 8x8 with-obstacle path-length anomaly (GBF 13 vs optimal-family 15) is unexplained, which raises a question about how path length is counted.

## Short quotes

- "The Algorithms are implemented on 4 × 4, 8 × 8, 16 × 16, and 32 × 32 grid systems." (Section 5, p.19)
- "However, this Algorithm is not optimal." (Section 6, p.24)
- "The worst Algorithm for without obstacles requiring most memory usage was Dijkstra’s Algorithm." (Section 6, p.24)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-pathfinding#Sponsor-to-target max-product path and chain hold\|Sponsor-to-target max-product path and chain hold]] | qualifies | Table 4 (with obstacles, single run): GBF is fastest from 8x8 up but its reported path is longer than the other three at 16x16 (37 vs 31 blocks) and 32x32 (81 vs 75). At 8x8 the table shows the reverse (GBF 13 vs 15 for the other three), an unexplained anomaly. Without obstacles all four report the same length (5/13/29/61, Table 3). The paper calls GBF 'not optimal'; it does not state the general rule that a greedy walk is optimal only when its estimate is exact (that is the analyst's inference). (Table 3 p.19-21; Table 4 p.21-23; Section 3.2.2 p.12; Section 6 p.23-26) | distToTarget (line 734) runs an exact max-product Dijkstra from the target over the full PE graph and does not consult the dropped set. tracePath (line 740) walks forward greedily by argmax PE[cur][v]*dT[v], skipping seen and dropped nodes. With drops, dT can still reflect routes through excluded members, so the walk can return a less reliable chain or stop short ('unreachable') when a route exists. buildPath (753-757) and bestSponsors (760-764) reuse the same dT. The comment at line 735 says 'PROVABLY-OPTIMAL'; dropped is set at line 1165. Single-path code reading, UNVERIFIED per DE-062. `fec/surfaces/templates/make_dashboard_html_00.tpl:734,735,740,753-757,760-764,1165` | Only the algorithmic point carries over (greedy choice on an inexact estimate loses optimality); grid timings and lengths do not. Cheap remedy is to exclude dropped members inside distToTarget. | moderate | VERIFIED-SOFTENED |
| [[systems/terrier-canvass#Street-plan presets, turf planner and walk routing\|Street-plan presets, turf planner and walk routing]] | supports | With obstacles (Table 4, single run), A* expands fewer blocks than Dijkstra at 16x16 (140 vs 177) and 32x32 (587 vs 754) with the same path length (31, 75); at 8x8 A* expands more (32 vs 30). Execution times at 32x32 are 3975.34 vs 5678.25 ms. A* uses F(n)=G(n)+H(n). (Table 4 p.21-23; Section 3.2.1 p.11) | walk_route._astar searches the street graph with estimate max(0, straight-line distance to nearest goal - NODE_SLACK_M); when any goal lacks coordinates (or a node lacks a position) the estimate is 0, i.e. Dijkstra, so it stays admissible. A plain dijkstra() helper sits at lines 176-197. `C:/Users/pgoff/Documents/Terrier/outputs/cu-geography-v3/server/research/walk_route.py:45,176-197,200-234` | Closest match in the system (spatial graph, metric estimate). Road networks are not uniform grids; paper figures are single runs with unreported hardware; gains appear only from 16x16 up. Supports the design choice, gives no measured speedup for street networks. | weak | VERIFIED |
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | qualifies | Informed (A*) search cuts expansions via an estimate H(n); the paper's estimates are grid/geometric. Dijkstra is described as using no heuristic (H(n)=0, F(n)=G(n)) and as extended to A* by adding a heuristic. (Section 3.1.3 p.9-10 (Dijkstra, H(n)=0); Table 2 p.14; Section 3.2.1 p.11; Table 4 p.21-23) | The staffer router is bidirectional Dijkstra with terminal costs and heuristic 0, plus Yen loopless alternatives and exact-arithmetic tie resolution. A one-directional Dijkstra to a virtual sink (_search_reference) is kept only as a test oracle that no server path can select. Route metadata records 'heuristic': 0. `fec/surfaces/staffer_closeness_routes.py:12-21,79,264-298,511,861` | No spatial embedding, so the paper's Euclidean estimates do not apply. A* gains would need an admissible non-metric estimate (e.g. ALT landmarks, deferred), which the paper does not test. Supports keeping h=0, not adding A* by name. | weak | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Cosponsor recruitment route finder (Pareto front)\|Cosponsor recruitment route finder (Pareto front)]] | context | GBF 'will always try to pick the best track at that particular moment' (F(n)=H(n)); it is fastest of the four implemented but not optimal. D*/D* Lite is described with pseudocode in Section 3.2.3 but is not among the four algorithms implemented or benchmarked. (Section 3.2.2 p.12; Section 3.2.3 p.12-13; Tables 3-4 p.19-23) | build_route is a greedy construction: a heap orders candidate edges by (gain - lam*(1-p))/cost with an out-degree cap K (env default 5); the script keeps non-dominated points across lam in (0,0.5,1,2,4). The header names MOPBD* as a roadmap concept; there is no incremental replanning code. `fec/engine/cosponsor_route.py:3,9,40-63,70-76` | Analogy only: the route finder solves a budgeted coverage problem, not point-to-point shortest path. The paper has no D* Lite measurements, so it cannot justify building the replanning layer. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The summary says that with obstacles GBF 'paths get longer', but Table 4 at 8x8 shows GBF reporting a SHORTER path than Dijkstra/A*/BFS (13 vs 15). This anomaly is unmentioned and undercuts the implicit claim that the other three always return the optimal length; the obstacle results are internally inconsistent at 8x8.
> - Summary issue: 'A* ... still returns the optimal length' holds only relative to Dijkstra/BFS within this single run (equal lengths at 16x16 and 32x32); the paper never verifies optimality independently, and the 8x8 row casts doubt on it.
> - Summary issue: The recruitment-path drop finding is stated in the summary as fact ('The walk then becomes a greedy best-first search on an out-of-date estimate'); it is a single-path code reading and must carry the UNVERIFIED (DE-062) label in the summary too.
> - Summary issue: Bearing (not summary) error carried into the record: the staffer router's default search is bidirectional, not one-directional; the one-directional search is a test oracle only.
> - Summary issue: Other summary figures verified: Dijkstra/GBF time ratio without obstacles 39.7/14.38=2.76x to 7500.14/346.60=21.6x; 37 vs 31 and 81 vs 75; 587 vs 754; no protocol, no repeated trials, no hardware details (the paper itself says hardware performance is unknown), memory never measured.

## Gaps and flags

- The dashboard recruitment path (distToTarget at make_dashboard_html_00.tpl:734) computes target reliabilities without excluding dropped members, while tracePath (line 740) skips them. With drops, the 'provably optimal' chain becomes a greedy walk on an out-of-date estimate. No test covers the case where a dropped member was on the route. Single-path observation, UNVERIFIED (DE-062).
- There is no admissible estimate for the non-spatial social graphs (cosponsor, staffer), so A*-family speedups are not available there. ALT landmarks remain deferred. The paper's grid heuristics do not fill this gap.
- No component replans incrementally (D*/D* Lite/LPA*) when the graph changes. For N=533 a full recompute is cheap enough. It would matter only for the multi-million-pair staffer graph, and the paper gives no evidence on that.
- The paper's memory rankings (GBF 'least memory', Dijkstra 'most memory usage') were never measured. Section 5 lists no memory metric. Do not cite this paper for memory claims.
- The paper is not a systematic review (no protocol, databases or counts), and Table 5 cites references [90]-[95] that do not support the claims attached to them. Cite only the Table 3-4 counts, and label them single-run.
- The GRIN_PF 'outperforms existing approaches' claim is untested. Do not cite it.
- Wording: the dashboard comment calls the chain 'PROVABLY-OPTIMAL ... mathematical certainty ... exact, maximal probability' (make_dashboard_html_00.tpl:735-737). That holds only for the modeled kernel with no drops, and chainHold is a modeled output that depends on ASKM, not an empirical probability (D-025 spirit). The UI tooltip at line 1119 is already correctly hedged.
- The paper does not argue for dropping regular cosponsorship edges or for per-edge volume normalization, and says nothing about LDA or reciprocity. No conflict with the rejected SNA proposals.

Paul files this paper in his Path-Finding folder as a citation only, with no takeaways. His only mark is the free-text note 'Very interesting' beside the D* node-state passage. The component positions also cite it as grounding for D*/D* Lite replanning (cosponsor-route-pareto) and as indirect support for Terrier's street routing. The evidence only partly supports that reading. The paper describes D* but never runs it, so it cannot ground the replanning layer Paul names in his addendum. Its tested contribution is narrower: A* beats Dijkstra on node expansions on larger grids when it has an admissible geometric estimate, and greedy search's reported paths were longer at 16x16 and 32x32 with obstacles (shorter at 8x8, unexplained). Paul's note is not stale, since the system still has no incremental replanning. But the paper points somewhere more practical than D* Lite: the 533-node dashboard path should simply recompute its exact backward pass once members are dropped. His 'A* heuristics that cope with unstable information' goal (Rongkuan, Oropeza-Martinez) gets nothing from this paper for non-spatial graphs. The only place its A* evidence applies cleanly is Terrier's street router, which already uses an admissible straight-line estimate.

## Related papers

- [[chen-2016-most-reliable-path]] — complements — Chen 2016 is the most-reliable-path work cited in the dashboard comment (tpl:739) for the max-product objective; Lawande supplies the greedy-vs-exact search contrast relevant to where that walk can lose optimality.
- [[duan-2025-breaking-sorting-barrier]] — complements — Both concern single-source shortest-path cost: Duan gives worst-case asymptotic improvements over Dijkstra on directed graphs; Lawande gives only single-run grid timings.
- [[ren-2022-multi-objective-path]] — complements — Multi-Objective Path-Based D* Lite (MOPBD*), which cosponsor_route.py names as a roadmap concept; Lawande describes the D* family but tests none of it.
- [[luo-2020-path-planning-algorithm]] — complements — An improved D* Lite evaluated in simulated 3D grid environments (UAV forest firefighting), supplying D* Lite results that Lawande leaves untested.
- [[xu-2024-improved-lite-based]] — complements — A D* Lite variant with a dynamic route-planning simulation benchmark, filling the replanning-evidence gap that Lawande only discusses.
- [[abdelrahman-2025-conflict-based-search]] — complements — Multi-agent CBS with D* Lite replanning (advantages largely qualitative), extending past Lawande's single-agent static-grid scope.
- [[ahmad-nd-shortest-path-algorithms]] — complements — Applies shortest-path algorithms to social-network tie strengths, the non-spatial setting where Lawande's grid heuristics do not carry over.

### Linked from
- [[zeller-2016-step-path-selection]] — complements — Lawande covers grid and waypoint path-finding algorithms on a given cost surface. Zeller covers how such a cost surface is estimated and how sensitive the resulting routes are to it.