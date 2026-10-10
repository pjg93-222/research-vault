---
type: theme
name: "Path-Finding Algorithms"
paper_count: 8
tags: [theme]
---

# Path-Finding Algorithms

**Path-Finding Algorithms: literature synthesis (8 works, 2016-2025)**

This theme holds eight works. All come from computer science, operations research or robotics. None studies legislators, lobbying or elections. The only social-network work is [[ahmad-nd-shortest-path-algorithms]], which uses a DBLP co-authorship subset from 2001-2010 and a 15-day Twitter retweet graph. The works fall into five groups:

- **Exact shortest-path theory.** [[duan-2025-breaking-sorting-barrier]] gives a deterministic O(m log^(2/3) n) single-source shortest-path algorithm for directed graphs with real non-negative weights. It is the first to beat Dijkstra's O(m + n log n) on sparse graphs. It is purely theoretical: there is no implementation and no constant.
- **Probabilistic and "strongest" path objectives.** [[chen-2016-most-reliable-path]] studies deadline reliability, P(T <= b), which is non-additive, so Dijkstra cannot solve it. [[ahmad-nd-shortest-path-algorithms]] studies the classical max-product or -log "strongest path", which is additive, so Dijkstra can solve it.
- **Heuristic and goal-directed search benchmarks.** [[lawande-2022-systematic-review-analysis]] runs BFS, Dijkstra, A* and Greedy Best First once each on Unity grids from 4x4 to 32x32.
- **Incremental replanning (the D* Lite family).** [[luo-2020-path-planning-algorithm]] (UAV, 3D grids), [[xu-2024-improved-lite-based]] (Arctic ship routing, a 448x304 grid of 25 km cells), [[ren-2022-multi-objective-path]] (multi-objective, MAPF benchmark grids) and [[abdelrahman-2025-conflict-based-search]] (multi-agent CBS on 3D voxel grids).
- **Multi-objective and Pareto search.** [[ren-2022-multi-objective-path]] is the core work, with [[chen-2016-most-reliable-path]]'s dominance label-setting and [[xu-2024-improved-lite-based]]'s weighted-sum risk/distance trade-off alongside it.

**Paul's own notes for this category.** His two path-finding reading lists (sha12 6f4e1c72fa73 and d963fd8719df) carry annotation text on only 2 of their 9 works. Both are A*-family papers that are *not* in this theme's work set:

- Oropeza-Martinez 2024 adds OCEAN traits to A*. Paul tags it "Priority - Structure - Basis".
- Rongkuan 2024 is an improved A* for unstable tactical terrain. Paul tags it "Priority - Structure - Augmentation".

His argument is that routing in social networks should reflect how people actually react, not only distance. His stated gaps are:
- no congressional application;
- traits belong on edges, but the methods put them on nodes;
- the OCEAN reduction from 10 to 4 observable traits loses information;
- robustness under node loss has only been shown on physical, not social, networks.

The theme's eight works do not answer these questions. They answer different ones: how fast, how exact, how to replan, and which objective is being optimized.

**Main finding for Paul's systems.** The literature mostly audits citations and design choices already in Melange's routers. It does not supply new evidence for them:
- The dashboard recruitment chain cites [[chen-2016-most-reliable-path]], but it implements the classical max-product path of [[ahmad-nd-shortest-path-algorithms]].
- cosponsor_route.py names MOPBD* ([[ren-2022-multi-objective-path]]) but implements a weighted-sum sweep over greedy set selection.
- scenario.py cites D* Lite reuse that the code does not perform ([[xu-2024-improved-lite-based]]).

Two possible correctness problems in the "PROVABLY-OPTIMAL" recruitment chain came out of reading these papers against the code: edge direction, and stale cost-to-target values after a member is dropped. Both are UNVERIFIED single-agent leads (DE-062), not findings.

## Strands

### Exact shortest-path theory and the cost of speed

**What it covers.** [[duan-2025-breaking-sorting-barrier]] is the only worst-case complexity result in the theme.

- It combines Dijkstra and Bellman-Ford through a recursive bounded multi-source routine (BMSSP) and a FindPivots step. The parameters are k = floor(log^(1/3) n) and t = floor(log^(2/3) n).
- This reaches O(m log^(2/3) n) on directed graphs with real non-negative weights.
- It is also the first *deterministic* algorithm to beat O(m + n log n) on undirected graphs. The earlier DMSY23 result, O(m sqrt(log n log log n)), was randomized.
- The gain covers distances only. The paper cites Haeupler et al. (2024), who show Dijkstra is optimal when the output must include the full ordering of vertices by distance.

**Agreement.** The three works agree that additive objectives, sums of non-negative weights or sums of -log p, are the setting where Dijkstra-class exactness holds.
- [[ahmad-nd-shortest-path-algorithms]]'s strongest path, log(1/D) + sum of log(1/influence) with D = 0.95, is such an objective. Ahmad nonetheless solves it by enumerating paths. An exact Dijkstra-on-(-log) search, or Duan's algorithm, would do the same job without enumeration.
- [[chen-2016-most-reliable-path]] marks the boundary. Once the objective is a deadline probability with summed variances under a square root, it is non-additive and Dijkstra-type methods fail.

**Disagreement.** The works do not disagree directly. The tension is about relevance. Duan's bound is asymptotic and covers full single-source runs on sparse graphs. Paul's routers are small (the 533-member dashboard uses an O(N^2) array scan), point-to-sink with early stopping, or integer-weight.

**Lesson for the staffer lane.** The staffer-network latency lane found the bottleneck was memory (pair_cache), not the heap's log n. Speed results like Duan's therefore matter less than the exactness contract.

**Transferable practices.**
- Duan breaks ties lexicographically on (length, hop count, predecessor). This matches the staffer router's (cost, steps) rule.
- Chen checks that its algorithm is optimal by matching an independent multi-criteria A* on the same budgets.

Works: [[duan-2025-breaking-sorting-barrier]], [[chen-2016-most-reliable-path]], [[ahmad-nd-shortest-path-algorithms]]

### Probabilistic, reliability and tie-strength path objectives

This strand asks which quantity a path should optimize when edges carry probabilities or tie strengths.

**[[chen-2016-most-reliable-path]]** maximizes on-time arrival probability under a travel-time budget b, with stochastic link times. It uses a two-stage exact algorithm, MRP-TS:
- Stage 1: a least-expected-time search sets the risk regime.
- Stage 2: Mean-Budget dominance label-setting finds the optimum.

Results (Wuhan taxi network, 19,306 nodes, 2009 morning peaks):
- The chosen path changes with the budget. At b = 2301.4 s the best path is the high-variance one. At b = 2630.9 s (the least expected time) it is the mean path, with 50% reliability. At b = 2875.5 s it is the low-variance path.
- Lognormal is the best-fitting link distribution for 50.29% of links, against 19.46% for normal.
- The normal approximation of path times was accurate on 98.3% of 100 Monte Carlo-checked paths at the 10th percentile, and 94.9% at the 90th.
- Links are assumed independent, and the authors name link correlation as the main open limitation.

**[[ahmad-nd-shortest-path-algorithms]]** applies Hangal et al.'s (2010) directed influence weight: each edge's share of the node's total interaction volume, chained as a strongest path. It compares this with hop-count Dijkstra on 500 random source-target pairs per network.
- On Twitter (15-day retweet graph, about 23.5k users), the strongest path was longer in 323 of 500 pairs (64.6%). Mean lengths were 6.8 vs 4.9 hops.
- On DBLP (2001-2010, about 38.9k authors), it was longer in 225 of 500 pairs (45.0%). Mean lengths were 4.7 vs 3.3 hops.
- The paper calls the weighted search "better" but reports no path-quality outcome and no significance test. It shows only that the weighted route is different and longer.

**Agreement.** Both papers hold that edge strength and direction change the optimal route, and that the hop-shortest path is the wrong target when ties differ.

**Disagreement.** They optimize different objectives, and this is where the dashboard's citation goes wrong. The recruitment chain implements Ahmad and Hangal's additive max-product path, which Dijkstra solves exactly, but it cites Chen, whose problem Dijkstra cannot solve. The citation supports the word "reliable", not the method.

**Cautions.**
- Chen's independence caveat applies to any product of per-hop probabilities: it overstates certainty when hops are correlated.
- Wording (D-025): Ahmad's tie strengths, and any route cost built on them, are heuristic model inputs. They are not access, closeness or introduction probabilities.
- Ahmad's asymmetry argument is not evidence that reciprocity measures are valid. Reciprocity is NOT validated in this project (D-104..D-108).

Works: [[chen-2016-most-reliable-path]], [[ahmad-nd-shortest-path-algorithms]]

### Heuristic and goal-directed search: when greed loses optimality

**[[lawande-2022-systematic-review-analysis]]** is labelled a systematic review but is really one single-run benchmark of BFS, Dijkstra, A* and Greedy Best First (GBF) on Unity grids.

Open grids (4x4 to 32x32):
- GBF is fastest at every size. Dijkstra is 2.8x (4x4) to 21.6x (32x32) slower, a ratio computed from Table 3.
- All four return the same path length: 5, 13, 29 and 61 blocks.

Grids with obstacles:
- GBF stays fastest from 8x8 upward, but its paths get longer: 37 vs 31 blocks at 16x16, and 81 vs 75 at 32x32.
- A* expands fewer nodes than Dijkstra and still returns the optimal length. At 32x32 it uses 587 vs 754 iterations.
- The Dijkstra vs BFS timing gaps (0.7%-15%) cannot be separated from noise, because no trials were repeated.
- Memory was never measured, and several Table 5 citations do not match their claims.

**Agreement with Chen and Luo.** All three agree that goal-directed search keeps exactness only with an admissible heuristic:
- Chen's risk-averse procedure, MRP-RA, needs an admissible lower bound h(i) for its optimality proof (Prop. 5).
- Luo uses a Chebyshev-type heuristic on 3D grids.
- Lawande's GBF-with-obstacles result shows what happens without one: greedy expansion on an imperfect estimate gives longer, non-optimal paths.

**Gap.** None of the three offers an admissible heuristic for a *non-spatial* social graph. That is the open problem behind Paul's note on Rongkuan 2024 (heuristic augmentation) and behind the staffer and dashboard routers both running with heuristic 0.

**Bearing on Melange.** The sharpest bearing is a lead, not a finding (DE-062). The dashboard recruitment walk is greedy over precomputed reliabilities to the target. That is exact only while those reliabilities are current. Once a member is dropped, the walk becomes a greedy best-first search on an out-of-date estimate, which is Lawande's failure case.

**Bearing on Terrier.** Terrier's walk router is textbook A* with an admissible straight-line estimate, where Lawande's A* result applies most directly (VERIFIED support).

Works: [[lawande-2022-systematic-review-analysis]], [[chen-2016-most-reliable-path]], [[luo-2020-path-planning-algorithm]]

### Incremental replanning: the D* Lite family

Four works modify D* Lite, which searches backward from the goal and reuses g and rhs values when edge costs change. [[lawande-2022-systematic-review-analysis]] describes D* and D* Lite but tests neither.

**[[luo-2020-path-planning-algorithm]]** adds map zooming to a quarter of the size, a 40 m local refinement window, spline smoothing and greedy nearest-first ordering of targets.
- Path lengths are within about 8% of plain D* Lite across six simulated environments (largest gap Env4, 337 vs 311 m).
- The one large speed gain is Env3: 12.6 s for plain D* Lite vs 0.25 s, on a grid of about a million cells.
- Table 1's time cells contradict the text for Env1 and Env2.
- The authors themselves conclude that plain D* Lite is fine for short-range planning where the map changes little.

**[[xu-2024-improved-lite-based]]** adds a local-update rule, a deadlock-avoiding path-extraction rule and shortcut smoothing. Cost is delta x risk + distance. Each period is one simulated voyage.
- Against original D* Lite, nodes updated fall 34.9% in the freezing period (18,468 vs 28,351) and 79.5% in the melting period (16,864 vs 82,210).
- Planning time falls 46.2% (924 vs 1,720 s) and 85.7% (632 vs 4,409 s).
- Route distance stays within about 0.1%.
- The local update is *approximate*.

**[[ren-2022-multi-objective-path]]** (MOPBD*) carries D* Lite reuse into Pareto-set search (Python, 4-connected grids, 2-4 objectives).
- It is up to 10x faster than the node-based MOD* with two objectives.
- Against from-scratch NAMOA* it needs fewer expansions but is not always faster. It is slower on the 16x16 empty map, and from-scratch NAMOA* wins at four objectives.
- Its worst case can exceed from-scratch search when many states are invalidated.

**[[abdelrahman-2025-conflict-based-search]]** reports, from Table 2 averages, execution time falling from 5.575 s to 0.04 s and nodes explored falling from 2142 to 250.5.
- The 5.575 s figure does not match Table 1, whose values average to 4.575 s.
- The node reduction comes entirely from the 4-neighbour case. With 8 neighbours, D* Lite explores more nodes than A* (198 vs 178).
- Paths come out about 0.8% longer (15.73 vs 15.6), yet the paper says optimality is kept.
- The run count is not reported.

**Agreement.** All four agree that reuse pays when changes are *local* and the graph is *large*.

**Disagreement.** They disagree on how general the gain is:
- Abdelrahman claims near-universal 99% speedups.
- Ren and Luo show the gain depends on scale and change pattern, with from-scratch search sometimes winning.

**Bearing on Melange.** No Melange router reuses earlier search state. Each rebuilds from scratch. On a 533-member graph a full recompute is already cheap. Together these works do not, on their own, support Paul's addendum naming D* Lite and LPA* as the operational replanning layer.

What does transfer is conceptual:
- Repair a route when an ask fails, instead of abandoning it. The dashboard's chain-hold figure currently assumes no rerouting at all.
- Xu warns that extracting a path from stale cost-to-goal values can deadlock or go wrong.

Works: [[luo-2020-path-planning-algorithm]], [[xu-2024-improved-lite-based]], [[ren-2022-multi-objective-path]], [[abdelrahman-2025-conflict-based-search]], [[lawande-2022-systematic-review-analysis]]

### Multi-objective and Pareto path search

This strand asks how to keep more than one objective, such as length, risk and reliability, without folding them into one number in advance.

**[[ren-2022-multi-objective-path]]** keeps the full Pareto-optimal path set between a moving start and a fixed goal. Each node stores a set of non-dominated cost vectors.
- Theorem 1 claims exactness on termination. It is backed by a proof but no numerical check.
- An epsilon-dominance variant (epsilon in {0.01, 0.02, 0.05}) trades accuracy for speed.

**[[chen-2016-most-reliable-path]]** uses the same mechanism, pruning dominated partial paths with multi-criteria labels, in Mean-Budget form. It verifies optimality against a second algorithm.

**[[xu-2024-improved-lite-based]]** takes the other route: a weighted sum, delta x risk + distance.
- Raising delta from 1 to 3 in the freezing period (Icemax 1.2 m) lowers average risk from 0.076 to 0.039 and lengthens the route by 3.4% (2,558.7 to 2,645.5 n mi).
- Risk is not monotone in Icemax in the melting period (0.625, 0.631, 0.615 at 2.4, 2.6, 2.8 m).

**Agreement.** The works agree that risk-length trade-offs are real and depend on the budget. Chen's budget-dependent path switch and Xu's delta sweep show the same shape.

**Disagreement.** They differ on method. Ren and Chen use exact dominance search. Xu sweeps a scalar weight. A weighted-sum sweep can only reach the convex-hull (supported) points of a Pareto front.

**Bearing on cosponsor_route.py.** This difference is what Ren measures cosponsor_route.py against. The file names MOPBD* in its docstring but does something else:
- It builds greedy recruitment *sets* under five fixed risk weights.
- It then filters those points by dominance.

That is a weighted-sum sweep over a set-selection problem. It is not a Pareto search over source-target paths, and it never replans. The bearing is VERIFIED-SOFTENED.

**Possible extension.** Ren also suggests a two-objective view of the staffer route cost, which now folds strength and uncertainty into one weighted sum. This is an UNVERIFIED lead (DE-062), and Paul's exactness rule would rule out Ren's epsilon variant.

Works: [[ren-2022-multi-objective-path]], [[chen-2016-most-reliable-path]], [[xu-2024-improved-lite-based]]

## Agreements and debates

**Where the works agree**
- **Additive objectives are Dijkstra's home ground.** Sums of non-negative weights or of -log p are solved exactly by Dijkstra-class algorithms ([[duan-2025-breaking-sorting-barrier]], [[ahmad-nd-shortest-path-algorithms]]). Non-additive objectives such as deadline reliability need label-setting ([[chen-2016-most-reliable-path]]).
- **Goal-directed search stays exact only with an admissible heuristic.** Chen's Prop. 5, Lawande's A* vs GBF-with-obstacles results, and Luo's Chebyshev heuristic all point this way.
- **Incremental reuse helps when changes are local and graphs are large.** All four D* Lite papers ([[luo-2020-path-planning-algorithm]], [[xu-2024-improved-lite-based]], [[ren-2022-multi-objective-path]], [[abdelrahman-2025-conflict-based-search]]) agree on this.
- **Risk and length trade off, and the best path depends on the budget or weight.** Chen's budget sweep (b = 2301.4 / 2630.9 / 2875.5 s on Wuhan) and Xu's delta sweep (freezing period, delta 1 to 3) both show it.

**Where they disagree or contradict**
1. **Is incremental replanning generally faster?**
   - [[abdelrahman-2025-conflict-based-search]] reports a 99.2% time cut (Table 2 averages, run count not reported).
   - [[ren-2022-multi-objective-path]] finds from-scratch NAMOA* faster on the 16x16 empty map and at four objectives, and says the worst case can exceed from-scratch search.
   - [[luo-2020-path-planning-algorithm]]'s authors concede that plain D* Lite suffices for short range and little change. Their one large gain is on a grid of about a million cells.
   - Read together, the gain is conditional on scale and change pattern.
2. **What "optimal" means.**
   - Abdelrahman says optimality is "maintained" while its paths are about 0.8% longer (15.73 vs 15.6).
   - [[lawande-2022-systematic-review-analysis]] calls GBF "not optimal" although on open grids all four algorithms return identical lengths (5/13/29/61 blocks). GBF's penalty appears only with obstacles.
   - Ren proves exactness (Theorem 1) without a numerical check.
   - Only Chen checks optimality empirically against a second algorithm.
3. **Exact vs approximate.**
   - Duan and Chen are exact.
   - Xu's local-update rule and Ren's epsilon-dominance variant are deliberate approximations.
   - This matters because Paul's 2026-10-03 rule requires staffer routes to be exactly optimal.
4. **Is weighted social search "better"?** Ahmad asserts it but measures only that strongest paths are longer: 64.6% of 500 Twitter pairs and 45.0% of 500 DBLP pairs. No outcome is measured. The claim is unsupported, not refuted.

**Debates inside Paul's systems that these works expose**
- **The Chen citation in the dashboard** (make_dashboard_html_00.tpl:735-757). The code solves Ahmad and Hangal's max-product problem but cites Chen's deadline-reliability problem. Chen supports the word "reliable", not the method (VERIFIED qualifies). [[ahmad-nd-shortest-path-algorithms]] is the better-fitting method source.
- **Direction in the "PROVABLY-OPTIMAL" chain.** The path matrix PE is directed: receiver multipliers and role priors make it so. The backward Dijkstra (distToTarget) appears to multiply edges in the forward direction. If so, the chain is exactly max-product only when PE is symmetric. UNVERIFIED (DE-062): a single-agent code reading, raised in [[duan-2025-breaking-sorting-barrier]] and [[ahmad-nd-shortest-path-algorithms]].
- **Stale cost-to-target values after a member is dropped.** distToTarget is computed through dropped members, while tracePath skips them. The forward walk then becomes greedy best-first on an out-of-date estimate, which is Lawande's GBF failure and the stale-extraction hazard Xu warns about. UNVERIFIED: read from the code, not run.

**FLAGS (project rules)**
- **Per-edge volume normalization (rejected in this project).** Ahmad's influence weight (Eqs. 1-3) divides each tie by the node's total interaction volume. If anyone reads [[ahmad-nd-shortest-path-algorithms]] as an argument to extend per-edge volume normalization to other Melange layers, FLAG it. That proposal was measured and REJECTED in this project and is not recommended here. No work in this theme argues for dropping regular cosponsorship edges.
- **Wording (D-025).** Tie strengths and route costs in these papers are heuristic model inputs and outputs, never closeness, access or introduction probabilities. The component name "staffer-closeness-routes" is an identifier, not a claim of closeness.

## Methods

**Which methods dominate**
1. **Simulated grid benchmarks.** Five of the eight works use them: [[lawande-2022-systematic-review-analysis]] (Unity 2D grids), [[luo-2020-path-planning-algorithm]] (synthetic 3D grids), [[xu-2024-improved-lite-based]] (a 448x304 ice grid), [[ren-2022-multi-objective-path]] (MAPF benchmark grids) and [[abdelrahman-2025-conflict-based-search]] (3D voxel grids). The comparator is usually the parent algorithm: A*, plain D* Lite, MOD* or NAMOA*.
2. **Proof-based algorithm design.** [[duan-2025-breaking-sorting-barrier]] is proof-only (Lemmas 3.2-3.12). [[ren-2022-multi-objective-path]] (Theorem 1) and [[chen-2016-most-reliable-path]] (Props. 4-5, Lemmas 1-3) pair proofs with experiments.
3. **Real-network computation.** Only two works use real networks:
   - [[chen-2016-most-reliable-path]]: the Wuhan taxi network, 19,306 nodes, 2009 morning peaks, plus a Chicago benchmark where it runs about 750x faster than label-correcting.
   - [[ahmad-nd-shortest-path-algorithms]]: DBLP 2001-2010 and a 15-day Twitter window, 500 pairs each, descriptive only.

**Weaknesses that recur**
- **No repeated trials, variance or significance tests.** This holds for Lawande, Luo, Xu and Abdelrahman. Every speed figure in the D* Lite strand is a single run or an unreported number of runs, so timing gaps such as Lawande's 0.7%-15% Dijkstra vs BFS spread cannot be separated from noise.
- **Internal numerical inconsistencies.**
  - Luo: Table 1 time cells contradict the text for Env1 and Env2.
  - Abdelrahman: the 5.575 s average does not match Table 1, which averages to 4.575 s. Its 3-agent and 6-agent numbers appear only in the conclusion.
  - Xu: 8 cells x 25 km is about 108 n mi, not the 432 stated, and the 0.07% error figure cannot be reproduced.
  - Lawande: several Table 5 citations do not match their claims.
- **Hardware, language or seeds not stated** in Lawande, Xu and Abdelrahman, so absolute timings cannot be compared across papers.
- **No outcome validation of routes.** Ahmad calls weighted paths "better" without any path-quality metric. No work tests whether a chosen path achieves anything downstream.
- **Domain transfer is untested.** Every replanning result is on spatial grids with spatial heuristics. Nothing shows an admissible heuristic or a replanning payoff on an abstract social graph.
- **Distributional assumptions.** Chen assumes independent links and normal path times. Its own data show normal fits only 19.46% of links best, and lognormal 50.29%.
- **Weighted sums stand in for Pareto search** (Xu, and the Melange cosponsor route). A sweep over scalar weights cannot recover non-supported Pareto points.

**Strongest practice in the set.** [[chen-2016-most-reliable-path]] cross-checks its exact algorithm against an independent multi-criteria A* on the same budgets. This is the one methodological habit that transfers directly to Melange's "PROVABLY-OPTIMAL" labels.

## How the works relate

- [[chen-2016-most-reliable-path]] — contrasting-objective — [[ahmad-nd-shortest-path-algorithms]]: Chen solves non-additive deadline reliability, P(T<=b), which Dijkstra cannot solve. Ahmad and Hangal's strongest path is additive in -log influence, which Dijkstra can solve. The dashboard cites Chen but implements Ahmad's objective, so Ahmad is the better-fitting method source.
- [[ahmad-nd-shortest-path-algorithms]] — same-problem-class — [[duan-2025-breaking-sorting-barrier]]: The strongest path is a directed, non-negative-weight shortest path on -log influence. An exact Dijkstra or Duan search could replace Ahmad's exhaustive path enumeration.
- [[duan-2025-breaking-sorting-barrier]] — complements — [[chen-2016-most-reliable-path]]: Duan speeds up additive single-source shortest paths. Chen's objective lies outside that class and needs label-setting.
- [[chen-2016-most-reliable-path]] — same-method — [[ren-2022-multi-objective-path]]: Both prune dominated partial paths with multi-criteria labels to get non-dominated sets: Mean-Budget dominance label-setting in Chen, path-based expansion in MOPBD*.
- [[luo-2020-path-planning-algorithm]] — same-method — [[xu-2024-improved-lite-based]]: Both are domain-specific modifications of single-objective D* Lite. Luo changes map zoom and smoothing; Xu changes local update and path extraction.
- [[ren-2022-multi-objective-path]] — same-method — [[abdelrahman-2025-conflict-based-search]]: Both build on D* Lite's incremental core: MOPBD* for multi-objective Pareto sets, Abdelrahman as the low-level planner inside multi-agent CBS. Together they are the D* Lite pair behind Paul's operational-layer addendum.
- [[ren-2022-multi-objective-path]] — qualifies — [[abdelrahman-2025-conflict-based-search]]: Ren shows from-scratch NAMOA* can beat incremental search (16x16 empty map, four objectives), and that the worst case can exceed from-scratch search. This undercuts Abdelrahman's near-universal 99.2% speedup claim.
- [[luo-2020-path-planning-algorithm]] — qualifies — [[abdelrahman-2025-conflict-based-search]]: Luo's authors concede that plain D* Lite suffices for short-range planning with little change, and their large gain appears only on a grid of about a million cells. Replanning gains are scale-dependent.
- [[xu-2024-improved-lite-based]] — same-method — [[ren-2022-multi-objective-path]]: Both extend D* Lite. Xu approximates locally for speed; Ren keeps the exact Pareto set, with an optional epsilon approximation.
- [[xu-2024-improved-lite-based]] — opposite-strategy — [[duan-2025-breaking-sorting-barrier]]: Xu reuses earlier search state when costs change. Duan computes single-source distances from scratch faster on a static graph.
- [[abdelrahman-2025-conflict-based-search]] — opposite-strategy — [[duan-2025-breaking-sorting-barrier]]: These are the two speed-oriented sources: work reuse on a changing graph vs a faster from-scratch algorithm on a static one.
- [[lawande-2022-systematic-review-analysis]] — complements — [[luo-2020-path-planning-algorithm]]: Lawande describes D* and D* Lite but tests neither. Luo supplies simulated D* Lite results.
- [[lawande-2022-systematic-review-analysis]] — complements — [[xu-2024-improved-lite-based]]: Xu's dynamic-route benchmark fills the replanning-evidence gap that Lawande only discusses.
- [[lawande-2022-systematic-review-analysis]] — complements — [[abdelrahman-2025-conflict-based-search]]: Abdelrahman extends past Lawande's single-agent, static-grid scope to multi-agent replanning.
- [[lawande-2022-systematic-review-analysis]] — complements — [[chen-2016-most-reliable-path]]: Lawande's GBF-with-obstacles result (longer paths) shows what happens without an admissible heuristic. Chen's MRP-RA needs an admissible heuristic for its optimality proof.
- [[lawande-2022-systematic-review-analysis]] — complements — [[duan-2025-breaking-sorting-barrier]]: Both concern the cost of single-source search. Duan gives worst-case asymptotics; Lawande gives single-run grid timings.
- [[lawande-2022-systematic-review-analysis]] — complements — [[ahmad-nd-shortest-path-algorithms]]: Both use a Dijkstra baseline. Ahmad's non-spatial social graphs are where Lawande's grid heuristics do not carry over.
- [[xu-2024-improved-lite-based]] — parallel-tradeoff — [[chen-2016-most-reliable-path]]: Both show the best route depending on a risk weight or budget: Xu's delta x risk + distance sweep, and Chen's budget-dependent switch between high-variance and low-variance paths. Xu folds the objectives into a weighted sum; Chen keeps the full objective exactly.
- [[ren-2022-multi-objective-path]] — complements — [[duan-2025-breaking-sorting-barrier]]: Ren covers dynamic, multi-objective cases that Duan's static, single-objective result leaves out.

## What it means for the systems

Each bearing below carries its status from the theme file. VERIFIED-SOFTENED means verified with softened wording. UNVERIFIED items are single-agent leads, not findings (DE-062).

**Melange: staffer route search** (component `melange-pathfinding.staffer-closeness-routes`; the name is an identifier, and route costs are heuristic model outputs, never closeness or access probabilities, per D-025) — see [[systems/melange-pathfinding]] and [[systems/melange-staffer-network]]
- *Speed.* [[duan-2025-breaking-sorting-barrier]] does not apply (qualifies, VERIFIED-SOFTENED). The router is an early-stopping, point-to-sink search with Yen alternatives, and the staffer-network latency lane found the bottleneck was memory (pair_cache), not the heap's log n. ALT landmarks remain the deferred lever, and Duan does not replace them.
- *Tie rule.* Duan's lexicographic tie rule on (length, hops, predecessor) matches the router's (cost, steps) rule. This is context only.
- *Exactness.* Paul's 2026-10-03 rule that staffer routes must be exactly optimal rules out [[xu-2024-improved-lite-based]]'s approximate local update (qualifies, VERIFIED) and Ren's epsilon variant. Many different origin-target pairs also undercut incremental reuse.
- *Method source.* The hop cost -ln(0.8 x strength) has the same form as Hangal's log(1/D) + log(1/influence) in [[ahmad-nd-shortest-path-algorithms]], with D = 0.8 instead of 0.95 (method-source, VERIFIED-SOFTENED). The paper gives no validation of that cost.
- *Possible extension.* [[ren-2022-multi-objective-path]] suggests a two-objective view of strength vs uncertainty, which the router now folds into one weighted sum (suggests-extension, VERIFIED-SOFTENED; the analysis itself is UNVERIFIED).
- *Heuristic.* [[chen-2016-most-reliable-path]]'s admissible-heuristic requirement bears on Paul's wish for heuristic-augmented A*. The router currently uses heuristic 0, and no admissible heuristic for a social graph exists in this literature.
- *Replanning.* [[abdelrahman-2025-conflict-based-search]]'s bearing (qualifies) is UNVERIFIED.

**Melange: staffer overlap routes** — [[systems/melange-staffer-network]]
- Integer-weight search falls outside Duan's real-weight model (qualifies, VERIFIED).

**Melange: dashboard recruitment chain** (`member-recruitment-path`, make_dashboard_html_00.tpl:735-757) — [[systems/melange-pathfinding]]
- *Citation.* The citation to [[chen-2016-most-reliable-path]] supports the word "reliable", not the max-product method (qualifies, VERIFIED). [[ahmad-nd-shortest-path-algorithms]] is the better method source.
- *Chain-hold figure.* By Chen's own independence caveat, the figure overstates certainty when hops are correlated. By [[abdelrahman-2025-conflict-based-search]]'s logic, it assumes no rerouting after a failed ask (suggests-extension, UNVERIFIED).
- *Second-algorithm check.* Chen's practice suggests cross-checking "PROVABLY-OPTIMAL" against an independent search (suggests-extension, VERIFIED).
- *Two correctness leads, both UNVERIFIED (DE-062).*
  - (a) The backward distToTarget may multiply directed PE edges in the forward direction. If so, the chain is exact only when PE is symmetric.
  - (b) After a member is dropped, distToTarget is stale while tracePath skips the member. The walk then becomes greedy best-first on an out-of-date estimate ([[lawande-2022-systematic-review-analysis]], qualifies, VERIFIED-SOFTENED; [[xu-2024-improved-lite-based]]; [[luo-2020-path-planning-algorithm]], suggests-extension, VERIFIED-SOFTENED).
  - A second, independent check should confirm or close each lead before anyone calls it a defect.

**Melange: cosponsor route Pareto** (`cosponsor-route-pareto`) — [[systems/melange-pathfinding]]
- cosponsor_route.py names MOPBD* only at docstring level. It actually runs greedy set selection under five fixed risk weights, then filters by dominance. That is not a Pareto path search and it has no replanning ([[ren-2022-multi-objective-path]], qualifies, VERIFIED-SOFTENED; [[luo-2020-path-planning-algorithm]], qualifies, VERIFIED-SOFTENED).
- Chen's exact label-setting is the extension path, should Paul want one (suggests-extension, VERIFIED).

**Melange: influence-engine scenario runner** — [[systems/melange-influence-engine]]
- scenario.py cites D* Lite reuse that the code does not perform ([[xu-2024-improved-lite-based]], qualifies, VERIFIED-SOFTENED). This is a documentation and citation accuracy issue.

**Melange: multilayer social and press edges** (`melange-sna.multilayer-social-press-edges`) — [[systems/melange-sna]]
- The social layer's edge probability (events divided by the author's posting volume) is essentially Ahmad's Eq. 3. The layer then averages the two directions, which discards the asymmetry Ahmad argues for (qualifies, VERIFIED-SOFTENED).
- This is not a recommendation to restore direction as a validated signal. Reciprocity is NOT validated (D-104..D-108).
- FLAG: Ahmad's normalization by node volume is a per-edge volume normalization. That proposal was measured and REJECTED in this project, so this paper must not be cited to extend it.

**Melange: D* Lite and LPA* as the operational replanning layer** (Paul's addendum) — [[systems/melange-pathfinding]]
- Across [[luo-2020-path-planning-algorithm]], [[xu-2024-improved-lite-based]], [[ren-2022-multi-objective-path]] and [[abdelrahman-2025-conflict-based-search]], gains depend on large graphs with local changes. On the 533-member cosponsor graph, full recompute is cheap.
- These works alone do not justify the layer on compute grounds. The case for it would have to be "repair, don't abandon" route semantics, not speed.

**Terrier: walk router** (`terrier-canvass.street-presets-and-turf`) — [[systems/terrier-canvass]]
- A* with an admissible straight-line estimate is the setting where [[lawande-2022-systematic-review-analysis]]'s A* result applies best (supports, VERIFIED). [[luo-2020-path-planning-algorithm]] gives context only (VERIFIED-SOFTENED).

**Moneyball and Mentat**
- No bearing from this theme.
- LDA is an identity source, not an edge or weight source (D-021). None of these works changes that, and any future lobbying-to-graph path work must respect it. See [[systems/melange-lobbying]].

## Gaps

**What is missing from this theme**
1. **No political or legislative application.** None of the eight works studies legislators, staff, lobbying or elections. The only social-graph work, [[ahmad-nd-shortest-path-algorithms]], is descriptive: 500 pairs per network, DBLP 2001-2010 and a 15-day Twitter window, with no outcome measure. This is Paul's stated gap 1 (no congressional application).
2. **Paul's own priority works are absent from the work set.** His two annotated A* papers are not among this theme's slugs, so this synthesis cannot assess them:
   - Oropeza-Martinez 2024, trait-aware A* with OCEAN ("Priority - Structure - Basis");
   - Rongkuan 2024, improved A* under unstable conditions ("Priority - Structure - Augmentation").
   Seven of nine entries in each of his two path-finding reading lists are citation-only. Alkazzi & Okumura (multi-agent path-finding) also sits unannotated in his ML and sentiment list.
3. **Traits on edges.** Paul's stated gap 2: traits belong to agents (nodes), but routing needs edge parameters. No work here gives a method for that. His stated limit of 10 to 4 observable OCEAN traits is also unaddressed.
4. **No admissible heuristic for non-spatial graphs.** Every heuristic in the set is geometric: Manhattan, Chebyshev or straight-line. The staffer and dashboard routers run with heuristic 0. Nothing here offers an informed but admissible estimate for a social graph, and ALT landmarks, the deferred lever, are not covered by any work.
5. **Missing families the system already uses or plans to use.** There is no literature on k-shortest or Yen alternatives, bidirectional search, ALT or contraction hierarchies, or LPA*, even though Yen is built, ALT is deferred and LPA* is named in Paul's addendum.
6. **Correlated edges.** [[chen-2016-most-reliable-path]] names link correlation as its main open limitation. Melange's chain-hold product assumes independence. No work models correlated per-hop probabilities.
7. **Robustness under node loss and temporal change on social graphs.** These are Paul's gaps (j) and (c). The replanning works study moving obstacles on spatial grids, not loss of members or ties shifting over time in a social network.
8. **Outcome validation.** No work tests whether a chosen route predicts anything downstream. For Melange this means route rankings stay heuristic model outputs (D-025) with no external criterion.
9. **Replication-grade benchmarks.** The D* Lite strand needs repeated trials, stated hardware, variance, and internally consistent tables. Luo, Xu, Abdelrahman and Lawande all fall short.
10. **Converting bipartite or multilayer data into path-ready graphs.** These are Paul's gaps (a) and (b), and they recur across his notes. Path-finding algorithms here assume the graph already exists. Any lobbying-derived conversion must keep LDA as identity only, not edges or weights (D-021).

**Next checks this synthesis implies (leads, not findings)**
- Independently re-verify the two UNVERIFIED recruitment-chain leads: direction in distToTarget, and stale values after a member is dropped.
- Cross-check "PROVABLY-OPTIMAL" against a second exact search, in Chen's style.
- Correct the Chen, MOPBD* and D* Lite citations in the dashboard, cosponsor_route.py and scenario.py so they describe what the code actually does.

## All works

- [[duan-2025-breaking-sorting-barrier]] (2025) — Breaking the Sorting Barrier for Directed Single-Source Shortest Paths
- [[lawande-2022-systematic-review-analysis]] (2022) — A Systematic Review and Analysis of Intelligence-Based Pathfinding Algorithms in the Field of Video Games
- [[chen-2016-most-reliable-path]] (2016) — Most Reliable Path-finding Algorithm for Maximizing On-Time Arrival Probability
- [[ahmad-nd-shortest-path-algorithms]] (n.d.) — Shortest Path Algorithms for Social Network Strengths
- [[luo-2020-path-planning-algorithm]] (2020) — A UAV Path Planning Algorithm Based on an Improved D* Lite Algorithm for Forest Firefighting
- [[xu-2024-improved-lite-based]] (2024) — An Improved D* Lite-Based Dynamic Route Planning Algorithm for Ships in Arctic Waters
- [[ren-2022-multi-objective-path]] (2022) — Multi-Objective Path-Based D* Lite
- [[abdelrahman-2025-conflict-based-search]] (2025) — Conflict-Based Search for Multi-Agent Path Planning Using D*Lite for Dynamic Re-planning: Analysis and Verification
