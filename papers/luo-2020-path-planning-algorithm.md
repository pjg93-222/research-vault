---
type: paper
title: "A UAV Path Planning Algorithm Based on an Improved D* Lite Algorithm for Forest Firefighting"
authors: ["Zhaohui Luo", "Youmin Zhang", "Lingxia Mu", "Jing Huang", "Jing Xin", "Han Liu", "Shangbin Jiao", "Guo Xie", "Yingmin Yi"]
year: "2020"
venue: "2020 Chinese Automation Congress (CAC), IEEE. Metadata hint said \"IEEE\" only; the venue is the CAC conference (corrected)."
doi: "10.1109/CAC51589.2020.9327111"
category: "Path-Finding Algorithms"
systems: ["melange-pathfinding", "terrier-canvass"]
components: ["melange-pathfinding.member-recruitment-path", "melange-pathfinding.cosponsor-route-pareto", "melange-pathfinding.staffer-closeness-routes", "terrier-canvass.street-presets-and-turf"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/luo-2020-path-planning-algorithm.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\A_UAV_Path_Planning_Algorithm_Based_on_an_Improved_D_Lite_Algorithm_for_Forest_Firefighting.pdf"]
sha12: ["690a2c02e3be"]
tags: ["paper", "path-finding-algorithms"]
---

# A UAV Path Planning Algorithm Based on an Improved D* Lite Algorithm for Forest Firefighting

**Zhaohui Luo, Youmin Zhang, Lingxia Mu, Jing Huang, Jing Xin, Han Liu, Shangbin Jiao, Guo Xie, Yingmin Yi (2020).** *2020 Chinese Automation Congress (CAC), IEEE. Metadata hint said "IEEE" only; the venue is the CAC conference (corrected)..* [10.1109/CAC51589.2020.9327111](https://doi.org/10.1109/CAC51589.2020.9327111) Theme: [[themes/path-finding-algorithms|Path-Finding Algorithms]].

> [!abstract] Summary
> Luo et al. (2020, Chinese Automation Congress) change D* Lite for 3D firefighting-drone path planning in maps that are partly unknown. They shrink the map to a quarter of its size and search backward from the goal on that coarse map. They then refine a 40 m local window around the drone's 20 m radar range, smooth the path with sight-line pruning and a Catmull-Rom spline, and replan whenever the path is blocked or the target moves. Several targets are visited greedily, nearest first. All evidence is simulated: one run per synthetic grid, plain D* Lite as the only comparison, no variance, and a Table 1 whose time cells contradict the text for Env1 and Env2. The usable results are narrow. Path lengths are within about 8% of each other. Env3 shows a large speed gain (12.6 s vs 0.25 s), but only in one simulated 100x100x100 m map (the paper gives no cell resolution). The authors themselves conclude that plain D* Lite is fine for short-range planning where the map changes little.
> 
> For Paul's systems the paper matters for two reasons. First, his addendum names D* Lite and LPA* as the operational replanning layer. This paper suggests the benefit depends on scale: incremental and hierarchical search pays off on very large graphs. Paul's 533-member cosponsor graph is far smaller, and full recomputation there is cheap. Second, the dashboard's recruitment pathfinder already searches backward from the target, which is D* Lite's structure. Reading the code, I found that it does not update that search when a member is dropped in a what-if. This is exactly the replanning case the paper addresses.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Can an improved D* Lite incremental search, combined with map zooming and path smoothing, plan safe, short, real-time 3D paths for firefighting UAVs that reach suspected fire spots and UGVs (refuelling/material stations) when the 3D workspace is partly or wholly unknown?

**Theory.** No social or economic theory. The framing is algorithmic: D* Lite incremental/heuristic search (Koenig-style, cited as [4], [5]), a hierarchical/partitioned A* idea (HPA*, described in Sec. II intro, not referenced), and Catmull-Rom spline smoothing ([8]).

**Data.** Simulated 3D grid environments only, with no real forest data. Table 1: six environments (Env1-2 120x120x100 m; Env3-6 100x100x100 m), obstacle densities 0%, 1%, 5%, cuboid obstacles plus random 5x5x5 m^3 obstacles; single static target. Table 2: four improved-only environments (Env1-2 100x100x100 m; Env3-4 120x120x100 m). Fig. 6: four environments with multiple static targets. Fig. 7: one scenario with a dynamic target (UGV). Unit = one simulated planning run per scenario; no run count or repetitions reported. No time period (2020 paper). Hardware: Intel Core i7-9700, Ubuntu 18.04, Python 2.

**Method.** Improved D* Lite (Sec. II.B, steps 1-7, p.4234-4235): (1) zoom the map out to a quarter of its size; (2) if Euclidean start-to-goal distance is below 28, run plain D* Lite, otherwise plan on the shrunken map with reverse search from the target; (3) scan obstacles with radar range r = 20 m and refine a local window d = 2r = 40 m; (4) smooth the path with two methods: sight-line node removal (method 1) and centripetal Catmull-Rom spline insertion (method 2); (5) fill the trajectory where node coordinates differ by more than 1 on any axis; (6) re-plan when the path is invalidated (next node more than r away, or target moved). Heuristic h is the maximum absolute coordinate difference (Chebyshev-type). Multiple targets are visited greedily by nearest remaining Euclidean distance (Eq. 3, Sec. III.B). The UAV is treated as a point particle with dynamics neglected (Sec. III, p.4235).

**Identification.** Not applicable: simulation study with no causal identification strategy; performance is compared between two planners on the same simulated scenarios.

**Scope.** Population: simulated forest firefighting UAVs (plus UGVs as moving targets), not real flights. Institution: Xi'an University of Technology and Concordia University. Era: 2020. Results are for synthetic grids with one static target per run in Table 1, and UGV tracking in one dynamic scenario.

## Key findings

- Path length is nearly equal between the two algorithms: the gap is at most 10% in all six Table 1 environments (largest is Env4, 337 m vs 311 m, about 7.7%; others 0.4% to 6.4%). (Env1 715.9 vs 719 m; Env2 711.5 vs 739 m; Env3 302 vs 316 m; Env4 337 vs 311 m; Env5 292.2 vs 311 m; Env6 297.6 vs 306 m) [Sec. III.A, Table 1, p.4236]
- Text claims D* Lite is much slower than the improved algorithm in Env1, Env2 and Env4, and that D* Lite is unsuitable in changing environments. Env4 agrees with this claim (D* Lite about 0.36 s vs improved about 0.14 s). Env1 and Env2 do not agree once the planning-time column is read as the paper's own 0.6 s / 2061 km/h example implies (time = small value); see caveat. (Env4: D* Lite 0.36 s vs improved 0.14 s; Env1 as extracted: 0.2 s vs 0.68 s; Env2 implied about 0.11 s vs 0.6 s) [Sec. III.A, p.4236, Table 1]
- Env3 (1% obstacles) shows the improved algorithm far faster than D* Lite: about 12.6 s for D* Lite vs 0.25 s for the improved algorithm. (D* Lite 12.6 s, 302 m, 86 km/h; improved 0.25 s, 316 m, 4550 km/h) [Sec. III.A, Table 1, p.4236]
- Under changing environments, the improved algorithm's time rises by 13.3% (Env5 to Env6, reproduced from Table 1). The text's 27% rise for D* Lite does not reconcile with Table 1. (Improved: Env5 0.15 s, Env6 0.17 s (+13.3%); D* Lite about 17 s (Env5), not traceable to a Table 1 cell) [Sec. III.A, p.4236 (text, not fully reconciled to Table 1)]
- Conclusion stated by the authors: plain D* Lite is suitable for short-range or local planning with little environmental change; the improved method is claimed for long-distance and changing environments. [Sec. III.A, p.4236 (final paragraph)]
- Improved algorithm on a single moving (UGV) target: total planning time 0.6 s, path length 343.5 m, computed planning speed 2061 km/h. This is the only dynamic-target number reported. (0.6 s; 343.5 m; 2061 km/h) [Sec. III.C, Fig. 7, p.4237]
- Improved algorithm on four further environments (multiple static targets / Table 2): times 0.23 to 0.91 s, lengths 394.8 to 752.6 m. (Env1 0.34 s, 418.8 m, 4434.4 km/h; Env2 0.23 s, 394.8 m, 5685.1 km/h; Env3 0.91 s, 752.6 m, 2977.3 km/h; Env4 0.72 s, 734 m, 3667 km/h) [Sec. III.B, Fig. 6 and Table 2, p.4237]
- Authors' headline claim: the method plans safe paths for UAVs effectively and quickly in real time across a variety of test scenarios. This rests on simulation only. [Sec. IV, p.4237]

## Limitations

- Simulation only: no field flights, no real forest, no hardware-in-the-loop; UAV modelled as a particle with dynamics neglected (p.4235).
- UGV path planning explicitly excluded: 'Due to space limit' (Sec. I); 'Path planning of the UGVs will be considered as one of our future works' (Sec. III.C).
- Single run per scenario; no repetitions, variance, or seeds reported; no statistical testing.
- Only one comparator (plain D* Lite); no comparison with A*, HPA*, RRT, or other planners.
- Table 1 text layer is scrambled; the time/speed/length cells do not reconcile cleanly for every row (Env2 trad speed 0.03 vs implied about 0.11; Env5-6 D* Lite cells ambiguous). The Env1-2 direction contradicts the text; the table should be checked against the rendered PDF.
- The text's 27% D* Lite increase and 17 s figure cannot be traced to Table 1 cells; Table 1 implies about 17 s for Env5 and about 13.6 s for Env6 on the D* Lite side, a decrease. The improved method's 13.3% growth reproduces (0.15 to 0.17 s).
- Environment and map-scale parameters (cell size, unit for the 28 threshold, obstacle generator) are not specified well enough to reproduce.
- Python 2 implementation; no code or data release mentioned.
- Smoothing method 2 is said to 'slightly increase path length' but no length cost is quantified.

## Short quotes

- "the improved D* Lite algorithm proposed in this paper can plan paths quickly and safely in both long-distance and complex changing environments" (Sec. III.A, p.4236)
- "D* Lite algorithm is suitable for short-range or local path planning with little environmental changes." (Sec. III.A, p.4236)
- "The algorithm can quickly plan the safe paths to multiple static and dynamic targets by zooming out the maps." (Sec. IV, p.4237)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-pathfinding#Sponsor-to-target max-product path and chain hold\|Sponsor-to-target max-product path and chain hold]] | suggests-extension | Step 6 replans when the next node is more than r = 20 m away or the target has moved. The search runs backward from the goal (step 2). Plain D* Lite is suited to short-range or local planning with little environmental change. (Sec. II.B steps 2 and 6 (p.4235); Sec. III.A final paragraph (p.4236)) | buildPath calls distToTarget, a goal-rooted max-product field, then tracePath. distToTarget does not check the dropped set. tracePath skips dropped nodes only when choosing the next step, so its greedy descent reads field values that may route through dropped members. `fec/surfaces/templates/make_dashboard_html_00.tpl:734 (distToTarget, no dropped check); :740 (tracePath skips dropped at the step choice); :753-757 (buildPath); :677 (dropped set)` | Only the idea of refreshing a goal-rooted field after the graph changes transfers. The paper's speed numbers do not (3D grids of about 1M cells vs a 533-node graph). At this size a recompute that skips dropped nodes is enough; incremental D* Lite is not needed. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Cosponsor recruitment route finder (Pareto front)\|Cosponsor recruitment route finder (Pareto front)]] | qualifies | Plain D* Lite is adequate for short-range planning where the map changes little. The paper's speed gains come from shrinking 100x100x100 m and 120x120x100 m grids, with path lengths differing by up to about 8 percent (Env4, 337 vs 311 m). (Sec. III.A, Table 1 and text, p.4236) | cosponsor_route.py builds a recruitment route greedily with out-degree budget K=5, once for each risk weight lambda in 0, 0.5, 1, 2, 4, then filters a Pareto front. MOPBD* appears in the docstring as a concept only. Nothing is replanned incrementally; each run rebuilds. `fec/engine/cosponsor_route.py:3-5 (docstring, MOPBD* concept); :9 (K default 5); :41 (build_route greedy heap); :70 (lambda sweep); :77 (Pareto filter)` | The paper's own scale result argues against adding D* Lite to a 533-node graph where full recomputation is cheap. Map zooming trades optimality for speed, which the cosponsor objective does not want. Revisit only if routes are recomputed live on a much larger graph. | weak | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | qualifies | The paper combines a geometric heuristic (max absolute coordinate difference) with coarsening the map to a quarter of its size; attributing the speed gain to either is the analyst's inference, since the paper has no ablation. Path lengths differ by up to about 8 percent between the two methods in Table 1. (Sec. II.B steps 1-3 and heuristic definition (p.4234-4235); Table 1 (p.4236)) | staffer_closeness_routes.py runs bidirectional Dijkstra with heuristic 0 and exact tie-breaking at 1e-12, with Yen loopless alternatives. The routing layer caches up to 64 results in an OrderedDict and evicts the oldest entry with no recency update on a hit, so it is FIFO, not LRU. The 4.66M overlap-pair figure was not checked. `fec/surfaces/staffer_closeness_routes.py:14-21 (docstring); :79 (ALGORITHM label); :861 (heuristic 0 in output). fec/surfaces/staffer_routing.py:65 (OrderedDict); :380-390 (_base, heuristic 0); :569-570 (hit returns without move_to_end); :670-672 (popitem oldest while over 64)` | Neither speed lever transfers. A person-tie graph has no coordinates, so there is no admissible geometric heuristic, and map coarsening would break the proven-optimal contract. The only analog is hierarchical search, which would need its own correctness proof. Route costs remain heuristic outputs, not closeness or introduction probabilities (D-025). | weak | VERIFIED-SOFTENED |
| [[systems/terrier-canvass#Street-plan presets, turf planner and walk routing\|Street-plan presets, turf planner and walk routing]] | context | Multiple targets are visited greedily, each time taking the nearest remaining target by Euclidean distance (Eq. 3). Multiple static targets took 0.23 to 0.91 s on 394.8 to 752.6 m paths (Table 2). No comparison to an ordering optimizer is reported. (Sec. III.B, Eq. 3, Fig. 6, Table 2 (p.4237)) | walk_route.py solves up to 8 runs exactly. Larger groups use 20 nearest-next starts, then 2-opt with direction flips and or-opt, with double-bridge kicks under a deterministic evaluation cap. Groups are joined by A* on the walk road network with a straight-line heuristic. `Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/walk_route.py:1-13 (docstring); :31 (ARTERIAL_FACTOR); :34 (EXACT_TASKS=8); :35 (STARTS=20); :37 (EVALS_PER_MS); :176 (dijkstra); :200 (_astar straight-line estimate)` | Nothing to adopt directly. The paper's single greedy pass is weaker than the ordering already built. A geometric heuristic is valid for street routing, and the system already uses one. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Env3 is said to be a grid of about a million cells. Table 1 gives only the 100x100x100 m map size and no cell resolution, so the cell count is not stated in the paper.
> - Summary issue: The paul_alignment text says Paul marked D* replanning 'Very interesting'. The annotation (_annots f585bac36b56, p.12) sits on the Lawande 2022 review, on a generic re-planning passage, not on the Luo paper. The attribution is wrong.
> - Summary issue: The 'Env5 to Env6 +27 percent for D* Lite' claim does not reconcile. Table 1 speed and length columns imply about 17 s for Env5 and about 13.6 s for Env6 on the D* Lite side, which is a decrease. The improved method's 13.3 percent growth (0.15 to 0.17 s) reproduces, though the text says 0.2 s.
> - Summary issue: The staffer routing cache is described as an LRU. staffer_routing.py evicts the oldest insertion (popitem with last=False) and does not move entries on hit, so it is FIFO.
> - Summary issue: The 'about 4.66M guaranteed overlap pairs after v6' figure in the staffer-closeness bearing was not verified in any file read here.
> - Summary issue: The claim that coarsening returns non-optimal paths is not in the paper. The paper reports length differences, which go both ways in Table 1: the improved path is shorter in Env4 (311 vs 337 m) and longer elsewhere. Attribution of the speed gains to map shrinking is the analyst's inference, since the paper has no ablation.
> - Summary issue: The Terrier bearing names a TIGER network. walk_route.py loads road tables from walk.sqlite3 and does not name TIGER. Corrected in the ver bearing.
> - Summary issue: The dropped-member replanning defect is supported by static reading of make_dashboard_html_00.tpl (lines 734, 740, 753-757). The consequence in the summary (worse chain or false not-reached) is not tested and must be marked UNVERIFIED under DE-062.
> - Summary issue: Claims that the staffer router is already stronger than the paper's methods, and that no incremental replanning exists because Paul's addendum anticipated it, are evaluative. No benchmark supports the first, and the addendum (line 65, Adaptative addendum) names D Lite and LPA* as the operational layer without saying absence was anticipated.
> - Summary issue: The Chen 2016 related link is described as the source of the max-product objective. Its title concerns on-time arrival probability; the product form is the dashboard's assumption, cited to Chen only in code comments.

## Gaps and flags

- The dashboard's recruitment pathfinder does not replan when a member is dropped: distToTarget (make_dashboard_html_00.tpl:734) builds the target-rooted field without honouring the dropped set, while tracePath (:740) filters it. This is a static code reading; a what-if drop on the best chain may yield a degraded chain or a false 'not reached', which was not tested. The minimal fix is to skip dropped nodes in distToTarget; incremental D* Lite is not needed at N = 533.
- No incremental or replanning search exists anywhere in the pathfinding family (cosponsor routes, recruitment paths, staffer routes). On the evidence of this paper, though, that layer would pay off only on the large staffer graph, not the 533-node member graph.
- No timing benchmark exists for staffer route search at full graph scale. Such a benchmark is needed before any decision on a hierarchical or incremental search, and it must come from logs or existing runs, never a load test.
- The paper's own Table 1 is internally inconsistent: the Env1 and Env2 time cells contradict the text's claim that D* Lite is slower, and the 27% D* Lite increase and 17 s figure do not trace to table cells. None of its speedup figures should be quoted as evidence for adopting D* Lite in Paul's systems. Each run is one simulation (window: 2020 synthetic grids; denominator: one run per scenario, no repetitions).
- No conflict with the SNA rejections (regular cosponsorship edges and per-edge volume normalization are untouched) or with D-021 (LDA is not used). Any route cost or chain-hold value discussed alongside this paper stays a modeled or heuristic output, not an introduction or closeness probability (D-025).

Paul's addendum names D* Lite and LPA* as the operational layer. This paper gives that view only qualified support. Its own conclusion is that plain D* Lite is enough for short-range planning where the map changes little, and its gains come from coarsening very large 3D grids. That fits the staffer graph, with millions of pairs, much better than the 533-member cosponsor graph, where full recomputation is cheap. His note is not stale: no incremental replanning has been built. It also points to a concrete, cheap fix that the D* Lite framing makes visible. The dashboard's goal-rooted search is not refreshed when a member is dropped, on a static code reading. The fix is a recompute that respects the dropped set, not a D* Lite implementation.

## Related papers

- [[xu-2024-improved-lite-based]] — same-method — Title is an improved D* Lite for dynamic route planning of ships in Arctic waters. The relation is title-level only; the body was not re-read here.
- [[ren-2022-multi-objective-path]] — same-method — Title is Multi-Objective Path-Based D* Lite, matching the MOPBD* concept that cosponsor_route.py cites in its docstring.
- [[abdelrahman-2025-conflict-based-search]] — same-method — Title shows D*Lite used for dynamic re-planning in multi-agent path planning. Relation is title-level only.
- [[chen-2016-most-reliable-path]] — complements — Title is most-reliable path-finding for maximizing on-time arrival probability. The product-of-probabilities form that the dashboard uses is cited to Chen in code comments, not stated in the title.
- [[lawande-2022-systematic-review-analysis]] — complements — Systematic review of intelligence-based pathfinding in video games. Paul's 'Very interesting' annotation sits on its p.12 re-planning passage, not on the Luo paper.
- [[duan-2025-breaking-sorting-barrier]] — complements — Title is breaking the sorting barrier for directed single-source shortest paths, an exact algorithm. It bears on the exact-optimality contract of the staffer router, not on map coarsening.

### Linked from
- [[zeng-nd-multi-agent-reinforcement]] — complements — Same UAV application domain. Luo plans paths for single drones with D* Lite, while Zeng controls a swarm with RL. Both are engineering simulations relevant to Paul's pathfinding reading only by analogy.