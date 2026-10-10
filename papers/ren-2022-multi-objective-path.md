---
type: paper
title: "Multi-Objective Path-Based D* Lite"
authors:
  - "Zhongqiang Ren"
  - "Sivakumar Rathinam"
  - "Maxim Likhachev"
  - "Howie Choset"
year: "2022"
venue: "IEEE Robotics and Automation Letters, vol. 7, no. 2, April 2022"
doi: "10.1109/LRA.2022.3146918"
category: "Path-Finding Algorithms"
systems:
  - "melange-pathfinding"
components:
  - "melange-pathfinding.cosponsor-route-pareto"
  - "melange-pathfinding.staffer-closeness-routes"
paul_annotated: true
paul_markup_count: 40
evidence_status: "verified"
source: "[[_sources/ren-2022-multi-objective-path.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Downloads\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\adoring-yalow-ad763f\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\agent-a2168cba4e2fdc5a2\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\agent-a5628f8fee4a9c869\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\amazing-lewin-409893\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\codex-websocket-error-468fa9\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\ecc-benchmark-methodology-c4f259\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\evidence-csv-fetch-streaming-8bedff\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\forecasting-model-audit-5603f2\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\hungry-montalcini-08aad4\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\melange-coalition-builder-design-7babbe\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\melange-math-eval-skill-18e907\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\melange-members-section-fixes-6613db\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\melange-ui-redesign-372c43\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\mentat-forecast-redesign-0fb0be\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\mentat-github-migration-b06195\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\mentat-systems-production-file-53552c\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\pac-map-downstream-viz-c2bca4\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\sector-growth-political-spending-9cc0c8\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\silly-einstein-57ef31\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\unfinished-work-handoff-a3bbad\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\upbeat-golick-968cdd\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\vibrant-khayyam-65f943\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\vibrant-sammet-a549a5\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
  - "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\research\\Multi-Objective_Path-Based_D_Lite.pdf"
sha12:
  - "1006f6d4f610"
tags: [paper, path-finding-algorithms]
---

# Multi-Objective Path-Based D* Lite

**Zhongqiang Ren, Sivakumar Rathinam, Maxim Likhachev, Howie Choset (2022).** *IEEE Robotics and Automation Letters, vol. 7, no. 2, April 2022.* [10.1109/LRA.2022.3146918](https://doi.org/10.1109/LRA.2022.3146918) Theme: [[themes/path-finding-algorithms|Path-Finding Algorithms]].

> [!abstract] Summary
> Ren, Rathinam, Likhachev and Choset (RA-L 2022) present MOPBD*, an incremental planner that keeps the full Pareto-optimal path set between a moving start and a fixed goal when edge cost vectors change. It merges D* Lite's reuse of earlier search (g and v values, local consistency) with NAMOA*'s path-based expansion, so each node stores a set of non-dominated cost vectors rather than one value. Theorem 1 claims the stored set equals the true Pareto front on termination. An epsilon-dominance variant trims those sets and trades accuracy for speed. The tests use simulated 4-connected grids from the Stern et al. MAPF benchmark, with random integer costs, 2 to 4 objectives, Python code, obstacles inserted every 7 moves, and 1 to 5 minute limits. Against the node-based incremental MOD*, MOPBD* is faster by up to an order of magnitude with two objectives. Against NAMOA* from scratch it needs fewer expansions but is not always faster: it is slower on the 16x16 empty map, and at four objectives from-scratch NAMOA* wins. The worst case can exceed from-scratch search when many states are invalidated. In Melange, cosponsor_route.py names MOPBD* in its docstring but does not implement it. It runs a greedy build for each risk penalty lambda in 0, 0.5, 1, 2, 4, scoring edges by (gain - lambda(1-p))/cost, and then filters the points by dominance. That is a lambda-parameterised greedy heuristic over recruitment sets, not a Pareto search over source-target paths, and it never replans incrementally. The paper's main use for Melange is as a yardstick that shows this gap. A second use is a possible two-objective view of the staffer route cost, which currently folds strength and uncertainty into one weighted sum.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Paul's markup in the PDF

- pPDF p.1 (journal p.3318) Highlight: "quent path planning tasks" — 
- pPDF p.1 (journal p.3318) Highlight: "reuse previous, and perhaps partial, searches to expedite subsequent" — 
- pPDF p.1 (journal p.3318) Highlight: "We numerically evaluate the performance of MOPBD* and its variants in various maps with two and three objectives." — 
- pPDF p.1 (journal p.3318) Highlight: "is a problem of fundamental importance with numerous applications in robotics and logistics" — 
- pPDF p.1 (journal p.3318) Highlight: "When edge costs change, incremental search algorithms aim to reuse previous searches to speed up subsequent planning tasks." — 
- pPDF p.1 (journal p.3318) Highlight: "objectives such as travel risk, fuel usage, arrival time, to name a few." — 
- pPDF p.1 (journal p.3318) Highlight: "MO-SPP generalizes the conventional SPP by associating each edge in the given graph with a cost vector where each component of the vector corresponds to an objective to be minimized." — 
- pPDF p.1 (journal p.3318) Highlight: "we consider the dynamic version of the MO-SPP where the costs of the edges can change" — 
- pPDF p.2 (journal p.3319) Highlight: "This work aims to leverage both D* Lite and the path-based expansion in NAMOA* to create a novel incremental multi-objective planner to plan paths in dynamic environments." — 
- pPDF p.2 (journal p.3319) Highlight: "However, all these algorithms optimize a single-objective: minimizing the sum of edge cost values along the planned path." — 
- pPDF p.2 (journal p.3319) Highlight: "which leverages both path-based expansion in NAMOA* and incremental search in D* Lite." — 
- pPDF p.2 (journal p.3319) Highlight: "The numerical results show that MOPBD* outperforms both baselines." — 
- pPDF p.2 (journal p.3319) Highlight: "The set of all the non-dominated paths between uc and ud is called the Pareto-optimal set." — 
- pPDF p.3 (journal p.3320) Highlight: "Let OPEN denote a priority queue containing all inconsistent nodes to be expanded, where the nodes are prioritized by comparing their keys in the lexicographic order." — 
- pPDF p.3 (journal p.3320) Highlight: "Among these nodes, inconsistent ones are inserted into OPEN with updated keys." — 
- pPDF p.3 (journal p.3320) Highlight: "The basic difference between MO-SPP and SPP is that there are multiple non-dominated partial solution paths between any pair of nodes in the graph in general" — 
- pPDF p.3 (journal p.3320) Highlight: "Similarly, H(u) is a set of heuristic vectors, each of which underestimates the cost of a non-dominated path between u and uc." — 
- pPDF p.3 (journal p.3320) Highlight: "These two features show that MOA* takes a node-based expansion strategy." — 
- pPDF p.3 (journal p.3320) Highlight: "In addition, node expansion can be computationally demanding as all partial solutions at this node need to be extended." — 
- pPDF p.4 (journal p.3321) Highlight: "Then MOD* searches in the same manner by expanding inconsistent nodes until all Pareto-optimal paths are found." — 
- pPDF p.4 (journal p.3321) Highlight: "NAMOA* employs a path-based expansion to mitigate the drawbacks of the node-based expansion." — 
- pPDF p.5 (journal p.3322) Highlight: "we introduce the Delete procedure, which is invoked by UpdateVset" — 
- pPDF p.5 (journal p.3322) Highlight: "By doing so, V(u) for all u in U always contains the cost vectors of non-dominated paths between ud and u (Invariant-1)." — 
- pPDF p.5 (journal p.3322) Highlight: "dominated by g_k becomes non-dominated after the removal of g_k, and may lead to Pareto-optimal solutions." — 
- pPDF p.6 (journal p.3323) Highlight: "then s represents a (partial solution) path that goes through" — 
- pPDF p.6 (journal p.3323) Highlight: "ComputePath is invoked either at line 3 or line 9 in Algorithm 1." — 
- pPDF p.6 (journal p.3323) Highlight: "Theorem 1: When ComputePath terminates, C = C*." — 
- pPDF p.6 (journal p.3323) Highlight: "It is also known that multi-objective search requires exponential space and time with respect to the size of the graph in the worst case [6]." — 
- pPDF p.6 (journal p.3323) Highlight: "MOPBD* is thus less efficient in comparison with naively searching from scratch, when there are lots of states to be deleted after the edge cost changes." — 
- pPDF p.7 (journal p.3324) Highlight: "We assigned every edge in G a random integer vector of length M with components randomly sampled from [1, 10], where M varies in the following sections." — 
- pPDF p.7 (journal p.3324) Highlight: "After every k (k = 7 in our tests) moves of the robot, the simulator adds an obstacle node in front of the robot along the selected path." — 
- pPDF p.7 (journal p.3324) Highlight: "All algorithms use the same heuristic" — 
- pPDF p.7 (journal p.3324) Highlight: "in terms of the number of expansions, MOPBD* outperforms NAMOA* over all maps." — 
- pPDF p.7 (journal p.3324) Highlight: "When k = 0, l0 denote the average length of the Pareto-optimal paths between the initial start and ud." — 
- pPDF p.7 (journal p.3324) Highlight: "The reason is that when epsilon increases, only a few non-dominated partial solution paths are stored at each node, and the number of vectors in the V-sets" — 
- pPDF p.8 (journal p.3325) Highlight: "the G-sets) at nodes become smaller, which makes the procedure ProcessEdge computationally less demanding when edge costs change." — 
- pPDF p.8 (journal p.3325) Highlight: "MOPBD* outperforms running NAMOA* from scratch for each planning task in general, based on the median and the average over succeeded cases" — 
- pPDF p.8 (journal p.3325) Highlight: "However, as the number of objectives increases, the advantage of MOPBD* becomes less obvious and when M = 4, NAMOA* outperforms MOPBD*." — 
- pPDF p.8 (journal p.3325) Highlight: "The numerical results verify the efficiency of MOPBD* and its variant MOPBD*-epsilon with two, three and four objectives in comparison with baseline methods." — 
- pPDF p.8 (journal p.3325) Highlight: "One can also leverage MOPBD* to improve multi-objective multi-agent planners" — 

## Study

**Question.** How to incrementally recompute the full Pareto-optimal path set in a graph whose multi-objective edge costs change during robot navigation, by fusing D* Lite reuse with NAMOA* path-based expansion.

**Theory.** Incremental search theory from D* Lite (local consistency via g and v values) extended to sets of non-dominated cost vectors: a set-based consistency notion (a state is consistent if its cost vector is in V(u)), Pareto dominance, and epsilon-dominance for approximate fronts.

**Data.** Synthetic 4-connected grid maps from a public multi-agent pathfinding benchmark (Stern et al. 2019): empty, maze, random and game categories. Edge cost vectors are random integers in [1,10] (main tests) or [1,5] (obstacle test). Two to four objectives. Number of maps and test instances not stated in the extracted text. Time limit 1 minute per planning task (Sec VII-A) or 5 minutes (Sec VII-D). Unit: grid node. No real-world data.

**Method.** Algorithm design plus simulation benchmark, implemented in Python. Simulator: planner computes the Pareto set; robot follows a random path from it; every k=7 moves an obstacle node (incident edges set to infinite cost) is added ahead; planner replans. Baselines: MOD* (node-based incremental) and NAMOA* from scratch. MOPBD*-epsilon compared with NAMOA*-epsilon, epsilon in {0.01, 0.02, 0.05}. Heuristic: unit vector scaled by Manhattan distance. Correctness argued by Theorem 1 (proof, no numerical check); worst-case runtime argued analytically. No statistical testing.

**Identification.** Not applicable: controlled algorithmic benchmark on randomly generated inputs; no causal identification strategy.

**Scope.** Single robot on grid-graph path planning with changing edge costs; 2 to 4 objectives; 2022 publication. Results apply to simulated grids in Python, not to physical robots, continuous space or large real networks.

## Key findings

- MOPBD* is faster than the node-based incremental baseline MOD* by up to an order of magnitude on two-objective grid maps. (up to 10x runtime speedup; Table I cell values not recoverable from extraction; Abstract p.1; Sec VII-B p.7 (Table I))
- Against NAMOA* from scratch, MOPBD* needs fewer expansions on all maps but is not uniformly faster; it is slower on average on the 16 x 16 empty map because ProcessEdge is expensive. (fewer expansions on all maps; runtime exception: 16x16 empty map; Sec VII-B p.7 (Table I))
- MOD* timed out in some planning tasks on the 65 x 81 game map, so its average solution count is lower than the other two planners. (1 minute limit per planning task; 65 x 81 game map; Sec VII-B p.7)
- With three objectives, as epsilon rises from 0.01 to 0.05, MOPBD*-epsilon beats NAMOA*-epsilon on more planning tasks; epsilon 0.01 has few data points because many tasks time out. (epsilon in {0.01, 0.02, 0.05}; x-axis l_k/l_0 in [0,1]; Fig. 3 values not in text; Sec VII-C p.7, Fig. 3)
- Epsilon-dominance shrinks the stored G- and V-sets, making ProcessEdge and ComputePath faster; the trade-off is approximation quality. (not quantified in text; Sec VI-C p.6-7)
- With random add/delete of obstacles in a 5 x 5 window on the maze map (epsilon 0), MOPBD* beats from-scratch NAMOA* in median and average runtime in general. (M = 2, 3, 4; costs in [1,5]; 5 minute limit; Table II values not recoverable; Sec VII-D p.8 (Table II))
- The advantage of MOPBD* shrinks as objectives increase; at M = 4 from-scratch NAMOA* outperforms MOPBD*. (M = 4 is the crossover reported; Sec VII-D p.8; Conclusion p.8)
- Theorem 1 states that when ComputePath terminates, the set V(uc) equals the true Pareto-optimal front C*. (none (proof claim); Sec VI-A p.6, Theorem 1)
- Worst-case runtime can exceed from-scratch search, because all previous search results are deleted before searching again when many states are invalidated. (none; Sec VI-B p.6)
- Multi-objective shortest path is NP-hard even with two objectives, and the Pareto front can grow exponentially with the number of nodes. (none; Sec I p.1; Sec VI-B p.6)

## Limitations

- Worst case is exponential in graph size; MOPBD* is less efficient than from-scratch search when many states are deleted after cost changes (Sec VI-B).
- Advantage shrinks with more objectives; from-scratch NAMOA* beats MOPBD* at M = 4 (Sec VII-D).
- Slower than NAMOA* on average on the 16 x 16 empty map (Sec VII-B).
- Evaluation uses simulated 4-connected grids with random integer costs and synthetic obstacles; no physical robot or real cost data (Sec VII-A, VII-D).
- Timeouts censor some runs, e.g. MOD* on the game map and epsilon 0.01 runs (Sec VII-B, VII-C).
- The epsilon variant trades solution quality for speed; the quality ceiling depends on epsilon (Sec VI-C).
- Baseline labelling is inconsistent: the introduction calls MOD* baseline 1 and NAMOA* baseline 2, while Sec VII calls both baselines without numbering (Sec I, VII).

## Short quotes

- "MOPBD* is thus less efficient in comparison with naively searching from scratch" — Sec VI-B p.6
- "as the number of objectives increases, the advantage of MOPBD* becomes less obvious" — Sec VII-D p.8
- "MOPBD* outperforms NAMOA* over all maps." — Sec VII-B p.7 (number of expansions)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-pathfinding#Cosponsor recruitment route finder (Pareto front)\|Cosponsor recruitment route finder (Pareto front)]] | qualifies | MOPBD* returns the exact Pareto-optimal set of source-to-goal paths (Theorem 1: C = C*). Multi-objective shortest path is NP-hard even with two objectives, and the front can grow exponentially with the number of nodes. (Theorem 1 at PDF p.6 (text line 672); NP-hard and exponential statements at PDF p.1 and p.6 (text lines 88-90 and 675-676)) | Docstring line 3 says RELINK + MOPBD*. The code runs a greedy build_route for each lambda in 0, 0.5, 1, 2, 4 (line 70), scoring candidate edges as (gain - lam(1-p))/cost, then filters (cost, spread, risk) points by dominance (lines 64-76). It is a lambda-parameterised greedy heuristic over recruitment sets, not a weighted sum over the three objectives and not a path search. `fec/engine/cosponsor_route.py:3, 41-62, 64-76` | The paper claim concerns source-target paths with additive vector costs. The route finder uses a product-form reach and a mean-p risk over a recruitment set, so MOPBD* cannot be dropped in. The docstring overstates the method; the front is a heuristic approximation, not an exact Pareto set. | strong | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Cosponsor recruitment route finder (Pareto front)\|Cosponsor recruitment route finder (Pareto front)]] | qualifies | Incremental reuse helps when cost changes are few and local. On the maze map with random obstacle add and delete (M = 2, 3, 4; 5 minute limit), MOPBD* beats from-scratch NAMOA* in general on median and average runtime, but its advantage shrinks as M grows and from-scratch NAMOA* wins at M = 4. The worst case can exceed from-scratch search when many states are invalidated. (Sec VI-B at PDF p.6 (highlight on deleted states); Sec VII-D at PDF p.8, Table II (text lines 808-833)) | cosponsor_route.py keeps no saved search state: each run rebuilds routes from scratch (build_route called fresh for each lambda, lines 41-62 and 70-72). Edge probabilities come from MBC.build_P(adj) over the whole network (around line 22). The statement that these probabilities change in batch at pipeline refresh was not verified from code read here. `fec/engine/cosponsor_route.py:22, 41-62, 70-76` | The paper evidence covers single-robot grid planning with local edge changes at 1 to 5 minute limits on simulated grids in Python. It does not cover whole-network batch refresh or changes to global weights such as lambda or K. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | suggests-extension | Vector costs with a non-dominated set expose trade-offs that a single scalar sum hides. Epsilon-dominance (0.01 to 0.05) gives smaller stored sets and faster runs, at a cost in solution quality; the paper states no set-size or runtime bound. The advantage over from-scratch search shrinks as the number of objectives grows. (Sec VI-C at PDF p.6-7; Sec VII-B and VII-C at PDF p.7, Fig 3; Sec VII-D at PDF p.8) | Hop cost is minus log of HOP_FACTOR times strength plus a fixed weight times uncertainty (_hop_cost, lines 115-116). HOP_FACTOR is 0.8 in fec/staffer_network/closeness_math.py:20. Search is bidirectional Dijkstra with heuristic 0 (ALGORITHM, line 79). Yen loopless alternatives follow the same scalar (module docstring, line 23). Ties go to the fewest steps, then the least cost (around line 519). `fec/surfaces/staffer_closeness_routes.py:23, 79, 115-116, 519; fec/staffer_network/closeness_math.py:20` | The extension applies only if the strength-uncertainty trade-off should be shown as a front rather than fixed by w. It would add latency where route speed is already a concern. Any front members remain heuristic model outputs, never closeness or introduction probabilities (D-025). | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Annotation file shows 48 Highlight entries and 1 Ink entry (49 total), not 49 highlights; there are no text sticky notes. Checked against _annots/1006f6d4f610.json.
> - Summary issue: paul_entries (7 items: Bouton 2022 campaign donors and Grand View crypto forecasts) are misfiled and contain nothing about this paper. Confirmed.
> - Summary issue: paul_markup: 8 of 40 quotes did not match the annotation covered_text by 40-character prefix, possibly due to ligature or whitespace normalisation. Unresolved; no verdict depends on them.
> - Summary issue: summary_md describes a weighted-sum sweep over five fixed risk weights. The code is a lambda-penalised greedy with a dominance filter, not a weighted sum over objectives. Corrected in bearing 1.
> - Summary issue: paul_alignment says the outperforms claim omits the 16x16 and M = 4 exceptions. The paper states both itself (text lines 757 and 829); the omission is only in Paul's highlight selection.
> - Summary issue: The chen-2016 link's code basis (buildPath and chainHold) was not found by rg in fec/ for *.py and *.js. Link kept on the paper-level relation only.
> - Summary issue: The D* Lite and LPA* operational-layer claim traces to the Influence Maximization addendum (annotations folder, line 65), a general IM note rather than a cosponsor-routing document. The paraphrase is accurate but the scope is broader.
> - Summary issue: Extraction coverage states 47,178 bytes; the item n_chars is 46,067 characters. Byte count not checked; not material to any verdict.

## Gaps and flags

- cosponsor_route.py's docstring claims MOPBD*, but the code is a lambda-penalised greedy sweep over five lambda values with post-hoc dominance filtering, and no code in fec implements MOPBD*, NAMOA*, D* Lite or LPA*.
- The route-finder front is unmeasured against an exact or epsilon-approximate multi-objective front, so how many non-supported trade-off points it misses is unknown.
- No pathfinding component reuses search state across edge changes. Paul's addendum names D* Lite or LPA* as the operational layer, but nothing is built, and no use case (an interactive single-edge what-if) has been defined in which incremental reuse would pay.
- The staffer router fixes the strength-uncertainty trade-off through w, with no Pareto view and no sensitivity report over w.
- Item paul_entries are misfiled. They are Bouton et al. 2022 Small campaign donors (x2) and five Grand View Research crypto forecast entries, which belong to other documents and say nothing about this paper. Paul's view here comes only from his 48 highlights and 1 ink entry, with no text sticky notes.
- The docstring overstates the method: fec/engine/cosponsor_route.py:3 labels the module RELINK + MOPBD*, but the algorithm is a lambda-penalised greedy sweep. Melange-facing text should call it a lambda-sweep approximate front.
- The paper's evidence argues against adopting incremental replanning by default: its advantage disappears at M=4 and under widespread state invalidation. This qualifies the addendum's D* Lite/LPA* operational-layer plan.
- No conflict with the rejected SNA proposals, D-021 (LDA) or the reciprocity rule. Any multi-objective staffer front must keep the D-025 wording.

Paul's highlights, with no notes, cover incremental reuse, the Pareto set, the D* Lite plus NAMOA* fusion, and the claim that it outperforms both baselines. His addendum makes D* Lite and LPA* the operational layer. The paper backs the Pareto concept but qualifies replanning: the gains come on small grids with local changes and vanish at M=4. The paper itself states the 16x16 and M=4 exceptions; the omission is only in the selection of his highlights. His takeaway is aspirational rather than stale: no incremental planner exists, and the module that cites MOPBD* does not implement it.

## Related papers

- [[abdelrahman-2025-conflict-based-search]] — same-method — Second D* Lite citation, used as the low-level planner inside multi-agent Conflict-Based Search for replanning. Item text confirmed.
- [[luo-2020-path-planning-algorithm]] — same-method — Improved single-objective D* Lite for UAV replanning; a contrast with MOPBD*'s set-valued multi-objective extension of the same incremental core.
- [[xu-2024-improved-lite-based]] — same-method — Improved single-objective D* Lite for ship routing in a changing cost field; the setting in which incremental replanning is used.
- [[lawande-2022-systematic-review-analysis]] — complements — Survey placing D* Lite among pathfinding families. Background context only, not evidence for the paper's claims.
- [[chen-2016-most-reliable-path]] — complements — Most-reliable-path search with a -log p style objective, a conceptual pairing with vector-cost route search. The code reference to buildPath and chainHold was not found in fec/ and is dropped.
- [[duan-2025-breaking-sorting-barrier]] — complements — Faster single-source shortest-path algorithm, a separate lever for staffer route latency. The link is conceptual; the paper does not discuss it.

### Linked from
- [[arya-2025-relink-edge-activation]] — complements — cosponsor_route.py cites RELINK together with MOPBD*. The linked item is titled Multi-Objective Path-Based D* Lite, so the link is via the code docstring, not via RELINK's own text.