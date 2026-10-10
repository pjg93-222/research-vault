---
type: paper
title: "Breaking the Sorting Barrier for Directed SSSP"
authors:
  - "Ran Duan"
  - "Jiayi Mao"
  - "Xiao Mao"
  - "Xinkai Shu"
  - "Longhui Yin"
year: "2025"
venue: "arXiv preprint, arXiv:2504.17033v2 [cs.DS] (v2 stamped 30 Jul 2025; title page dated July 31, 2025). The text does not state a venue. The later STOC 2025 listing in Paul's bib docx cannot be verified from this file."
doi: ""
category: "Path-Finding Algorithms"
systems:
  - melange-pathfinding
components:
  - melange-pathfinding.member-recruitment-path
  - melange-pathfinding.staffer-closeness-routes
  - melange-pathfinding.staffer-overlap-routes
paul_annotated: true
paul_markup_count: 26
evidence_status: verified
source: "[[_sources/duan-2025-breaking-sorting-barrier.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Path-Finding Algorithums\\2504.17033v2.pdf"
sha12:
  - 203c3d9d4ab6
tags: [paper, path-finding-algorithms]
---

# Breaking the Sorting Barrier for Directed SSSP

**Ran Duan, Jiayi Mao, Xiao Mao, Xinkai Shu, Longhui Yin (2025).** *arXiv preprint, arXiv:2504.17033v2 [cs.DS] (v2 stamped 30 Jul 2025; title page dated July 31, 2025). The text does not state a venue. The later STOC 2025 listing in Paul's bib docx cannot be verified from this file.* Theme: [[themes/path-finding-algorithms|Path-Finding Algorithms]].

> [!abstract] Summary
> Duan, Mao, Mao, Shu and Yin (arXiv 2504.17033v2, 2025; Paul's bib lists it as STOC 2025) give a deterministic single-source shortest-path algorithm for **directed** graphs with real non-negative weights that runs in O(m log^(2/3) n) in the comparison-addition model. It is the first to beat Dijkstra's O(m + n log n) on sparse graphs. The method combines Dijkstra with Bellman-Ford through a recursive bounded multi-source routine (BMSSP). FindPivots shrinks the frontier to at most |U|/k pivots, and a block-list structure sorts only partially, with k = log^(1/3) n and t = log^(2/3) n. The result covers distances only, with no full distance ordering. It is purely theoretical: there is no implementation, and the constant C is never given.
> 
> For Paul's systems this is a speed result with very little to transfer. Every route engine as built is either tiny (533 members, an O(N^2) array scan in the dashboard), a point-to-sink query that stops early (staffer closeness routes, bidirectional, plus Yen alternatives), or an integer-weight search (overlap routes). The paper's bound covers full single-source distances on sparse graphs with real weights, so none of these uses fits its model. The one loose link is Assumption 2.1: the paper breaks ties by lexicographic order on (length, hop count, reversed vertex sequence), which is analogous to, not the same as, the staffer router's tie rule (a 1e-12 relative cost band, then fewest steps before exact cost). On latency, the 2026-10-02 latency-lane memory file records that the bottleneck was memory (pair_cache), not the heap's log n. ALT landmarks remain the deferred lever, and this paper does not replace them. While checking the dashboard's 'provably optimal' chain, I found a possible problem with edge direction (see flags). It was reproduced by code reading only, not by a run.

## Paul's annotation

From [[annotations/Path-Finding Algorithums - Path-Finding Algorithums - Annotated Bibliography|Path-Finding Algorithums - Path-Finding Algorithums - Annotated Bibliography]]:

> Duan, Ran, et al. “Breaking the sorting barrier for directed single-source shortest paths.” *Proceedings of the 57th Annual ACM Symposium on Theory of Computing*, 15 June 2025, pp. 36–44, [https://doi.org/10.1145/3717823.3718179](https://doi.org/10.1145/3717823.3718179).

## Paul's markup in the PDF

- p1 Highlight: "single-source shortest paths (SSSP)" — 
- p1 Highlight: "h real non-negative edge weights in the comparison-addition model." — 
- p1 Highlight: "he O(m + n log n) time bound of Dijkstra’s algorithm on sparse grap" — 
- p1 Highlight: "Dijkstra’s algorithm is not optimal for SSSP." — 
- p1 Highlight: "considers the lengths of the shortest paths from a source vertex s to all v ∈V" — 
- p1 Highlight: "It works in the comparison- addition model, natural for real-number inputs" — 
- p1 Highlight: "which runs in O(mα(m, n)+min{n log n, n log log r}) time in the comparison-addition mode" — 
- p2 Highlight: ", thus our result is also the first deterministic algo- rithm to break such O(m + n log n) time bound even in undirected graphs." — 
- p2 Highlight: "Dijkstra’s algorithm" — 
- p2 Highlight: "each time extracts a vertex u with the minimum distance from the source, and from u relaxes its outgoing edg" — 
- p2 Highlight: "ral step" — 
- p2 Highlight: "it relaxes all edges for seve" — 
- p2 Highlight: "" — 
- p2 Highlight: "approach merges these two methods through a recursive partitioning technique, similar to those used in" — 
- p2 Highlight: "maintains a “fron- tier” S of vertices such that if a vertex u is “incomplete”" — 
- p2 Highlight: "vertices in S are not guaranteed to be all complete.)" — 
- p2 Highlight: "the vertex in S closest to source" — 
- p2 Highlight: "fact that sometimes the frontier may contain Θ(n) vertices." — 
- p2 Highlight: "Our most essential idea is a way to reduce the size of the frontier." — 
- p2 Highlight: "ï¿½ each of which has a shortest path tree of size ≥k, and the number of such pivots is bounded by |ï¿½U|/k" — 
- p2 Highlight: "we use a divide-and-conquer procedure that consists of log n/t levels, each containing a set of frontier vertices and an upper bound B" — 
- p2 Highlight: "Thus the running time per vertex gets reduced to log n/ logΩ(1)(n), which is a significant speedup." — 
- p3 Highlight: "where C is the maximum edge weight." — 
- p3 Highlight: "it was generally believed that such sorting barrier exists for SSSP and many similar problems." — 
- p3 Highlight: "where β(m, n) = min{k ≥1 : log(k) n ≤ m n } √" — 
- p3 Highlight: "we assume there is a source vertex in the graph denoted by s" — 

## Study

**Question:** Can single-source shortest paths (SSSP) on directed graphs with real non-negative edge weights be solved faster than Dijkstra's O(m + n log n) bound on sparse graphs, in the comparison-addition model, and can the Omega(n log n) sorting barrier be broken for this problem?

**Theory:** Dijkstra's algorithm sorts vertices by distance through a priority queue, which costs Theta(n log n) on the frontier. Bellman-Ford relaxes all edges in k rounds with no sorting. The paper merges the two ideas through recursive partitioning, in the style of bottleneck-path algorithms (Gabow-Tarjan 1988; Chechik et al. 2016; Duan et al. 2018), so that the frontier is shrunk to pivots before any sorting (pp. 2-4). The sorting cost is reduced by a block-based partial-sorting structure (Lemma 3.3, pp. 6-7). The paper does not present a behavioural or social theory.

**Data:** No empirical data. The analysis is over worst-case inputs: directed graph G=(V,E) with n vertices, m edges, and non-negative real weights w: E to R>=0. The algorithm first transforms G into a constant-degree graph G' with O(m) vertices and O(m) edges via zero-weight cycles (Sec. 2, p. 3). The bound assumes m = O(n) (p. 4). No sample, N, or time period applies.

**Method:** Deterministic algorithm design plus worst-case running-time analysis. The core procedure is a recursive bounded multi-source shortest path routine, BMSSP(l, B, S) (Algorithm 3, p. 10). It uses FindPivots (Algorithm 1, p. 6), a base case that is a mini-Dijkstra (Algorithm 2, p. 9), and a partial-sort block-list data structure D (Lemma 3.3, pp. 6-8). Parameters are k = floor(log^(1/3) n) and t = floor(log^(2/3) n), with recursion depth ceil(log n / t) (Sec. 3.1, p. 4). Correctness is proved by induction on level l (Lemma 3.7, pp. 12-13). Time is bounded in Lemma 3.12 (p. 14).

**Identification:** Not applicable. This is a worst-case complexity result, not a statistical or causal identification strategy.

**Scope:** Population: directed graphs with real non-negative weights, sparse regime m = O(n) after a constant-degree transformation. Model: comparison-addition only (each addition and comparison takes unit time; no other operations on weights). Era: 2025 arXiv preprint. Institutions: Tsinghua University, Stanford University, and Max Planck Institute for Informatics (author affiliations, p. 2 footnotes). The paper does not address lobbying, networks as social objects, elections, persuasion, turnout, legislators, or text measurement.

## Key findings

- Main result: a deterministic SSSP algorithm for directed graphs with real non-negative weights in time O(m log^(2/3) n), the first to break the O(m + n log n) bound of Dijkstra on sparse graphs. Numbers: O(m log^(2/3) n). (Abstract, p. 1; Theorem 1.1, Sec. 1.1, p. 1)
- The result is also the first deterministic algorithm to beat O(m + n log n) for SSSP even on undirected graphs, since the prior O(m sqrt(log n log log n)) SSSP algorithm of Duan, Mao, Shu, Yin (DMSY23) is randomized. Numbers: DMSY23: O(m sqrt(log n log log n)) randomized, undirected. (Sec. 1, p. 2 (remark after Theorem 1.1); DMSY23 cited on p. 1)
- Dijkstra is optimal if the output must include the vertex ordering by distance (Haeupler et al. 2024), so the improvement applies to distances without a full ordering. (Sec. 1, p. 1 (citing HHR+24); Sec. 3.1 p. 4 ('without necessarily recovering a complete ordering'))
- Key idea: shrink the frontier. For a bound B, the frontier S is reduced to pivots of size at most |eU|/k, where k = log^(Omega(1)) n. Each pivot roots a shortest-path tree of at least k vertices. Numbers: |W| = O(k|S|); |P| <= |W|/k <= |eU|/k; time O(min{k^2|S|, k|eU|}). (Technical overview, Sec. 1.1, p. 2; FindPivots Lemma 3.2, p. 5)
- Per-vertex running time is reduced from Theta(log n) to log n / log^(Omega(1)) n through divide-and-conquer over log n / t levels. Numbers: log n / log^(Omega(1))(n) per vertex. (Sec. 1.1, p. 2)
- Parameter choice that yields the final bound: k = floor(log^(1/3) n), t = floor(log^(2/3) n), recursion level l = ceil(log n / t). Numbers: k = floor(log^(1/3) n); t = floor(log^(2/3) n); l = ceil(log n / t). (Sec. 3.1, p. 4; Lemma 3.12 final statement, p. 14)
- Output-size bound for each BMSSP call: the returned set U has size at most 4k^(2lt), and a partial run has |U| at least k^(2lt). Numbers: |U| <= 4k^(2lt); partial case |U| >= k^(2lt). (Lemma 3.9, p. 13)
- Running-time bound for a BMSSP call at level l. Numbers: C(k + 2t/k)(l+1)|U| + C(t + l log k)|N+_[min S d(x), B)(U)|. (Lemma 3.12, p. 14)
- Total running time over recursion depth: depth is O(log^(1/3) n) and FindPivots costs O(nk) per depth, so the sum is O(nk (log n)/t) = O(n log^(2/3) n). Batch-prepend overhead is O(n log^(1/3) n log log n). Numbers: depth O(log^(1/3) n); per-depth cost O(nk); overall O(n log^(2/3) n); batch-prepend O(n log^(1/3) n log log n). (Sec. 3.2 'Running time', pp. 10-11)
- Structure of recursion: children's output sets U_y are disjoint, and the sum of |U_x| over all nodes of the recursion tree is O(n log n / t). Numbers: sum |U_x| = O(n (log n)/t). (Sec. 3.2, p. 10)
- Data structure D (partial sorting with block lists) supports insert in amortized O(max{1, log(N/M)}), batch prepend in O(L max{1, log(L/M)}), and pull in O(|S'|). Numbers: insert O(max{1, log(N/M)}); batch prepend O(L max{1, log(L/M)}); pull O(|S'|). (Lemma 3.3, pp. 6-8)
- Correctness: BMSSP returns the full set of vertices whose shortest path visits S, with all returned vertices complete. The top-level call with S = {s}, B = infinity finds all shortest paths. (Lemma 3.7, pp. 12-13; top level, p. 5)

## Limitations

- Theoretical only. The text reports no implementation or experiments, so practical constants are unmeasured. The hidden constant C in the bounds is not quantified.
- Computation model is the comparison-addition model only (real weights; each addition and comparison costs unit time). Integer-weight or word-RAM improvements are discussed as related work (Thorup 1999/2004), not as this paper's result.
- Assumes constant in- and out-degree via a transformation (Sec. 2, p. 3). The transformation is stated to preserve shortest paths and yield O(m) vertices and edges. Assumes m >= n-1 and every vertex reachable from s (p. 3-4).
- Assumes all path lengths are distinct (Assumption 2.1, p. 4). The paper argues a lexicographic tuple order makes this WLOG, using Pred[] and hop count alpha stored per bd[v] (p. 4).
- Bound is O(m log^(2/3) n), which is superlinear on sparse graphs; the paper does not claim linear time. The result is for distances and does not produce the full distance ordering. The paper cites HHR+24 that Dijkstra is optimal if ordering is required (p. 1).
- Partial executions of BMSSP are noted as more complex to analyze, and the paper introduces batch-prepend and FindPivots to handle them (Sec. 3.1, p. 5; Lemma 3.3, p. 7).
- Paul's markup contains no notes. All 26 highlights have empty note text. Two are green, one is pink, and the rest are yellow. The meaning of the colours is not recorded in the file. One highlight has empty covered_text (page 2). Two covered_text fragments are partial ('ral step', 'it relaxes all edges for seve'), and one has garbled glyphs ('ï¿½' replacement characters before '|U|/k' on p. 2).

## Short quotes

- "Dijkstra’s algorithm is not optimal for SSSP." (Abstract, p. 1 (Duan, Mao, Mao, Shu, Yin, arXiv:2504.17033v2))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | qualifies | O(m log^(2/3) n) beats Dijkstra's O(m + n log n) only on sparse graphs, for single-source distances to ALL vertices on directed graphs with real non-negative weights, in the comparison-addition model. The analysis runs on a constant-degree transform with O(m) vertices and edges (m = O(n)). It returns distances without a full ordering; Dijkstra is optimal when the ordering is required (HHR+24). The constant C in Lemma 3.12 is not quantified and nothing is implemented. (Abstract and Theorem 1.1, p. 1; Sec. 1, p. 1 (HHR+24); Sec. 2-3.1, pp. 3-4; Lemma 3.12, p. 14) | The default office route is a point-to-sink search on an undirected hop graph: bidirectional Dijkstra with heuristic 0 that stops once the frontier lower bounds pass the best meeting (docstring :12-21; ALGORITHM :79). A second pass (_fewest_steps_exact, :436-480) solves the settled nodes in exact integers by scaling dyadic floats. Yen loopless alternatives need repeated searches. _search_reference (:266-298) is a one-way test oracle only. Not in the code: the 2026-10-02 latency lane (memory file, not code) found pair_cache grew the sidecar from 7.0 to 17.5 GB, i.e. memory, not heap cost; ALT landmarks deferred. `fec/surfaces/staffer_closeness_routes.py:12-21,79,266-298,436-480` | Carries over only if the system needs full single-source distance fields over the staffer graph (one origin to every office). The graph size is not established here: the ~4.66M figure traces to the co-tenure GUARANTEED pair count (4,660,388, staffer-sorting memory), which is an edge count, so the analyst's n ~ 10^6-10^7 is unsupported. The asymptotic ratio vs Dijkstra is about log^(1/3) n (roughly 2.5-3x for n in 10^6-10^7) before an unknown constant. Does not carry over to early-stopping point-to-sink, bidirectional, Yen or exact-tie passes, and does not touch the measured memory bottleneck. No recommendation to change the router. | weak | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | context | Assumption 2.1 (all paths have distinct lengths) is made WLOG by ordering each path as the tuple <length l, vertex count alpha, vertices read from the end v_alpha..v_1> lexicographically, comparable in O(1) using Pred[] and alpha stored per bd[v]. The paper notes this is done like in many shortest-path papers. (Sec. 2, Total order of Paths, Assumption 2.1, p. 4) | Staffer routes: costs within one part in 10^12 of the cheapest are ties; among them fewest steps wins, then least exact cost, then smallest node ids read from the sink end (D-082, Paul 2026-10-03). `fec/surfaces/staffer_closeness_routes.py:5-9,22-27` | Analogous, not identical: the paper compares exact length first and uses vertex count only on exact ties; the system lets a 1e-12 relative cost band count as a tie and then puts steps BEFORE exact cost. Both read the vertex sequence from the end. The paper's order is a presentation device for its own analysis and does not validate the system's tolerance band or step-first rule. | weak | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Ordered person-overlap routes and employment-routing overlay\|Ordered person-overlap routes and employment-routing overlay]] | qualifies | The result is for real weights in the comparison-addition model. For integer weights in the word RAM model, the related-work line culminates in Thorup's O(m + n log log min{n, C}) for directed graphs [Tho04], where C is the maximum edge weight. (Sec. 1.2 Related Works, pp. 2-3 (Tho04; 'C is the maximum edge weight', highlighted by Paul, p. 3)) | Overlap-route distances are integers: distance_units = max(1, ceil(1e6 x 128 / strength)) (:33-34), and the work_time variant max(1, ceil(1e6 / strength)) (:211). The search is bidirectional Dijkstra over heap keys (:140-165). `fec/surfaces/staffer_overlap_routes.py:27,33-34,140-165,211` | With integer weights the tighter asymptotic line is integer/word-RAM SSSP (Thorup), not this paper. Matters only if overlap-route latency becomes heap-bound, which has not been shown. | weak | VERIFIED |
| [[systems/melange-pathfinding#Sponsor-to-target max-product path and chain hold\|Sponsor-to-target max-product path and chain hold]] | context | Dijkstra's O(m + n log n) is beaten only asymptotically on sparse graphs, for a directed graph with non-negative weights; the paper assumes every vertex is reachable from s. (Abstract and Theorem 1.1, p. 1; Sec. 2, pp. 3-4) | distToTarget is a heap-free max-product Dijkstra scanning an array over N = 533 members, O(N^2); tracePath greedily follows PE[cur][v] x dT[v]. PE is directed: EMU[v] (receiver) enters PEBASE[u][v] (:586) and source_shock(u)/target_resistance(v) enter PE[u][v] (:595-597), so PE[u][v] != PE[v][u] in general. `fec/surfaces/templates/make_dashboard_html_00.tpl:586-597,734-757` | At n = 533 the asymptotic gain is irrelevant. The paper adds nothing to reliability semantics (-log p) or the unestimated ASKM lift. Relevant only as a reminder that directed SSSP must relax edges in the query direction (see the distToTarget flag). | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: 'The one real link is Assumption 2.1 ... which matches the staffer router's (cost, steps) tie rule' overstates: the paper orders exact length, then vertex count, then reversed vertex sequence; the router (D-082) uses a 1e-12 relative tie band and puts fewest steps before least exact cost. Analogous, not matching.
> - Summary issue: The staffer hop graph is undirected (staffer_closeness_routes.py docstring); the paper's directed result covers it only as a special case.
> - Summary issue: The 'memory not heap' latency finding comes from the 2026-10-02 latency-lane memory file, not from the cited code lines.
> - Summary issue: Bearing transfer text gives a '~4.66M-pair staffer graph' and n ~ 10^6-10^7; 4,660,388 is the co-tenure GUARANTEED pair (edge) count, so the vertex scale is unsupported.
> - Summary issue: The distToTarget direction flag is reproduced by this verifier's code reading (second path, reading only); it remains unrun and is unrelated to the paper.
> - Summary issue: Chen 2016 related link: Chen solves on-time-arrival probability under a time budget, not the dashboard's max-product per-edge reliability, so 'supplies the semantics' is inaccurate. Ren 2022 link: the 'Pareto cosponsor route finder' citation was not found in fec/surfaces.
> - Summary issue: Paul's bib STOC 2025 (pp. 36-44, doi 10.1145/3717823.3718179) cannot be verified from the arXiv v2 text; cite arXiv:2504.17033v2.

## Gaps and flags

- No route engine has a heuristic or landmark speed-up: heuristic 0 everywhere (staffer_routing.py:381), and ALT is deferred. If latency matters, the paper's asymptotic SSSP gain is a weaker lever than goal-directed search or preprocessing such as ALT or contraction hierarchies, which the paper does not cover.
- No measurement isolates how much staffer-route time is heap/priority-queue cost versus neighbor-evidence construction (pair_cache). Without that split, no faster-SSSP algorithm can be judged worthwhile.
- The dashboard's most-reliable chain has no test against a reference shortest-path on the directed PE matrix, such as a reverse-graph Dijkstra oracle like the one staffer_closeness_routes keeps in _search_reference.
- Reproduced by code reading only, not run (DE-062): distToTarget (make_dashboard_html_00.tpl:734) seeds d[target] = 1 and relaxes d[v] = d[u] x PE[u][v]. On a directed PE that computes reliability FROM the target outward, but tracePath (:740) multiplies a forward edge PE[cur][v] by dT[v] as if dT were reliability TO the target. Edges are asymmetric here (EMU[v] at :586; source_shock/target_resistance at :595-597), so the 'PROVABLY-OPTIMAL' comment (:735-737) holds only if PE is symmetric. The same class of reversed index was fixed at :633 on 2026-09-30. This finding does not come from the paper.
- Wording: the dashboard code comment calls chain reliability 'the exact, maximal probability' and 'mathematical certainty' (:735-737). The UI tooltip (:1119, :1155) correctly says it is a scenario output, not an empirical probability. The comment should not be quoted as a claim about access or introduction probability (D-025 spirit).
- Venue: the PDF read is arXiv v2 with no venue. Paul's bib gives STOC 2025, pp. 36-44, doi 10.1145/3717823.3718179. That is plausible but cannot be verified from the file, so cite the arXiv version unless the STOC version is checked.
- No conflict with the rejected SNA proposals, D-021 (LDA) or reciprocity rules: the paper takes no position on edge construction or weighting.

Paul's two bibliography entries for this paper carry citations only, with no takeaways. His 26 highlights mark the headline claim, the Dijkstra/Bellman-Ford contrast, the frontier-shrinking idea and the comparison-addition model, so he read it as algorithm background. His stated path-finding gaps are not addressed here: he wants to turn lobbying and bipartite data into a routable graph and to use A* heuristics that cope with unstable information. This paper speeds up exact SSSP. It says nothing about graph construction, heuristics, dynamic replanning or probabilistic reliability. His notes are not stale, but the system has since moved on. The 2026-10-02 staffer-latency lane showed the routers are bidirectional Dijkstra with heuristic 0, that the bottleneck was memory rather than sorting, and that ALT is deferred. A theoretical sorting-barrier result therefore has no current place in the build. Citing Duan under member-recruitment-path is accurate only as background.

## Related papers

- [[chen-2016-most-reliable-path]] — complements — The dashboard comment (make_dashboard_html_00.tpl:738) cites Chen, Shi, Zhang & Lam 2016 for most-reliable-path theory. Chen maximizes on-time arrival probability under a travel-time budget with stochastic link times (MRP-TS), not the dashboard's product of independent per-edge probabilities; Duan only bounds exact deterministic SSSP cost.
- [[lawande-2022-systematic-review-analysis]] — complements — Empirically compares Dijkstra, A*, BFS and Greedy Best First on small 2D grids; covers the heuristic/goal-directed family that Duan's exact worst-case result does not.
- [[ren-2022-multi-objective-path]] — complements — Multi-Objective Path-Based D* Lite (incremental multi-objective replanning) covers dynamic and multi-objective cases that Duan's static single-objective SSSP does not. The analyst's claim that it is cited for a Pareto cosponsor route finder was not found in fec/surfaces.
- [[ahmad-nd-shortest-path-algorithms]] — complements — Compares hop-count Dijkstra with an influence-based strongest path on directed weighted social ties, the modelling side Duan leaves out. Its tie strengths are heuristic model inputs, not access or introduction probabilities (D-025).

### Linked from
- [[luo-2020-path-planning-algorithm]] — complements — Title is breaking the sorting barrier for directed single-source shortest paths, an exact algorithm. It bears on the exact-optimality contract of the staffer router, not on map coarsening.
- [[xu-2024-improved-lite-based]] — complements — Verified: from-scratch directed single-source shortest paths. It is the opposite strategy to Xu's reuse of earlier search state.
- [[abdelrahman-2025-conflict-based-search]] — complements — It is the other speed-oriented path-finding source. It speeds up single-source shortest paths on a static graph, while this paper reuses work when the graph changes.