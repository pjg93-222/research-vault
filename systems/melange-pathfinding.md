---
type: system
name: "Melange path-finding (recruitment and introduction routes)"
slug: melange-pathfinding
components: [melange-pathfinding.member-recruitment-path, melange-pathfinding.multi-target-and-budget-modes, melange-pathfinding.cosponsor-route-pareto, melange-pathfinding.staffer-closeness-routes, melange-pathfinding.staffer-overlap-routes]
brain_notes: [actors/melange-dashboard-june-capstone.md, actors/staffer-live-sidecar.md]
paper_count: 28
tags: [system]
---

# Melange path-finding (recruitment and introduction routes)

Route search over two graphs: member-to-member recruitment chains on the cosponsor kernel (dashboard pathfinder modes and cosponsor_route.py), and staffer introduction routes on the people/office graph (closeness routes, ordered overlap routes and the employment-routing overlay). Route objectives, tie rules and coverage rules live here; the graphs themselves are built in melange-sna and melange-staffer-network.

Brain notes: [actors/melange-dashboard-june-capstone.md](obsidian://open?vault=brain&file=actors%2Fmelange-dashboard-june-capstone); [actors/staffer-live-sidecar.md](obsidian://open?vault=brain&file=actors%2Fstaffer-live-sidecar)

## Melange path-finding: evidence assessment (2026-10-10)

**Scope.** This covers 54 evidence rows across 5 components: 12 VERIFIED, 34 VERIFIED-SOFTENED and 8 UNVERIFIED, from 28 distinct papers. I also ran a short online scan for the empirical gaps. Scan papers are leads only: I read the abstract and first pages myself, with no adversarial pass (DE-062). Open-access copies are in `C:\Users\pgoff\Documents\research\_work\downloads\scan_2026-10-10\`:
- Box-Steffensmeier, Christenson & Craig 2019 (AJPS)
- Montgomery & Nyhan (JOP)
- Battaglini, Leone Sciabolazza & Patacchini (NBER w24442)
- the Zelizer 2019 APSR supplement

Fowler 2006 failed on a DNS error. No open copy was found for Zelizer's main article or for McCrain 2018.

**Main finding.** The literature supports the **algorithms** far better than the **quantities** they run on.
- **Algorithms.** Dijkstra on -log p, h = 0 with additive costs, greedy coverage under (1-1/e), and exact tie rules are all standard and correctly chosen for graphs of this size.
- **Quantities.** None of the following is estimated from data, and no paper in the corpus estimates them:
  - ASKM 0.07 (the active-ask lift)
  - K = 5 (whip capacity)
  - CEDGE
  - HOP_FACTOR 0.8
  - MEMBER_BETA 0.15
  - the overlay costs 1 / 2 / 3 / 5 / 8
  - the log2(1 + days/30) tenure form

  Every route score is therefore a heuristic model output. None is a closeness, access, ask-success or introduction probability (D-025).

**Real errors found** (code-level, not annotation issues):
1. **Dropped members break the recruitment chain.** In the recruitment path, `distToTarget` ignores the dropped set but `tracePath` skips dropped members. With any member dropped, the "PROVABLY-OPTIMAL" chain can be suboptimal or wrongly unreachable.
   - The logic was reproduced in a JS replica: chain product 0.009 against a best drop-avoiding chain of 0.36.
   - Not yet confirmed in the browser.
2. **Edge direction may be reversed (UNVERIFIED).** `dT` appears to be computed in the reverse edge direction from the one `tracePath` needs while PE is asymmetric. The effect is bounded by EMU 0.80–1.20 and only one agent has read the code.
3. **chainHold may not be a floor.** The code calls chainHold a "conservative floor", but that holds only if asks are independent. Asks on the same bill and in the same party context are likely positively correlated, and then the product is not a floor.
4. **Code comments claim more than the code does:**
   - `budgetRecruit` is called "cost-bounded", but the loop stops at nSeeds.
   - (1-1/e) is claimed for the target-weighted branch. The live caller passes target -1, so the live call is unaffected.
   - MOPBD* is named as if it produced an exact Pareto front. The code is a five-lambda greedy whose front is approximate and misses non-convex points.

**Misattributed citations.** Several citations do not show what they are cited for:
- Sotoudeh 2024 measures passage, not recruitment probability.
- Chen 2016 solves a different reliability problem.
- Furnas 2019 does not ground the $1k–$10k cost.
- Kim & Kunisky 2020 and Porter 2005 describe a lobbying bipartite graph and a committee graph, not the cosponsor graph, and the code contains no community term.
- GCOMB is a label for plain greedy.

**Paul's stated directions:**
- **A\*, learned heuristics, D\* Lite / LPA\*.** The evidence weighs against building these now. At 533 members a full recompute is cheap. The staffer-router bottleneck measured on 2026-10-02 was memory (7.0 → 17.5 GB from pair_cache), not search time. The replanning papers show the incremental advantage shrinking as changes grow, and they need a live event stream that does not exist.
- **Turning lobbying and bipartite data into a graph.** This must respect D-021: LDA is an identity source, not an edge or weight source.

**Largest gap.** Neither graph has any outcome data: no record of asks made and answered, and no outreach or introduction log. Without one, no route parameter can be fitted or validated.

## Sponsor-to-target max-product path and chain hold

**Computes.** The most reliable recruitment chain from a sponsor to a target, with per-hop $ and probability; chainHold = product of activeP per hop.

**Method.** Dijkstra on -log p. activeP(p) = p/(p + (1-p) x ASKM) with ASKM 0.07 (about a 14x odds shift, unestimated). recruitCost(p) = 1000 + 9000(1-p), clamped to $1k-$10k.

**Code refs.** `fec/templates/make_dashboard_html_00.tpl (buildPath, chainHold, recruitCost)`

**Assumptions.**
- Cosponsor-kernel edge probabilities proxy ask success
- Active asks raise odds by a constant factor (no study pins it)
- Money is capped by FEC limits; difficulty shows up in reliability, not dollars

**Evidence weight:** 15 rows (4 VERIFIED, 8 VERIFIED-SOFTENED, 3 UNVERIFIED); 13 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[furnas-2019-moneyed-interests-information]] | 2019 | contradicts | The paper tests requester identity (donor vs constituent vs lobbyists) and information source, never dollar amounts; it makes no claim that money is capped. It does say staff want information and analysis from ideologically aligned experts and defer to requesters who bring think-tank research (pp.3, 18), so 'info is the lever' is a loose paraphrase, not a fabrication. | The $1k-$10k bound rests on FEC contribution limits, which are law, not on this paper. The citation is a misattribution, and the cost function stands or falls without it. The paper's own result (aligned evidence earns staff side-taking) concerns what an outside requester brings to a staffer. It does not cover member-to-member asks along cosponsor paths. | moderate | VERIFIED-SOFTENED |
| [[duan-2025-breaking-sorting-barrier]] | 2025 | context | Dijkstra's O(m + n log n) is beaten only asymptotically on sparse graphs, for a directed graph with non-negative weights; the paper assumes every vertex is reachable from s. | At n = 533 the asymptotic gain is irrelevant. The paper adds nothing to reliability semantics (-log p) or the unestimated ASKM lift. Relevant only as a reminder that directed SSSP must relax edges in the query direction (see the distToTarget flag). | weak | VERIFIED |
| [[lawande-2022-systematic-review-analysis]] | 2022 | qualifies | Table 4 (with obstacles, single run): GBF is fastest from 8x8 up but its reported path is longer than the other three at 16x16 (37 vs 31 blocks) and 32x32 (81 vs 75). At 8x8 the table shows the reverse (GBF 13 vs 15 for the other three), an unexplained anomaly. Without obstacles all four report the same length (5/13/29/61, Table 3). The paper calls GBF 'not optimal'; it does not state the general rule that a greedy walk is optimal only when its estimate is exact (that is the analyst's inference). | Only the algorithmic point carries over (greedy choice on an inexact estimate loses optimality); grid timings and lengths do not. Cheap remedy is to exclude dropped members inside distToTarget. Needs a second-path reproduction (e.g. a test that drops a bridging member) before any claim. | moderate | VERIFIED-SOFTENED |
| [[chen-2016-most-reliable-path]] | 2016 | qualifies | The paper's 'most reliable path' maximizes on-time arrival probability Phi((b - mean)/SD) for a travel-time budget b. It states that this objective 'is non-additive and cannot be solved by existing' shortest-path methods, so it uses M-B dominance plus multi-criteria label-setting (MRP-TS) rather than Dijkstra. | The paper supports the system's method only in name. Its deadline-reliability algorithm would carry over only if the system modeled a stochastic time-to-join per hop and asked for P(target joins before a deadline such as markup), which it does not. As built, the right grounding is the classical max-product or -log p shortest path, not this paper. Road data, 2009 Wuhan; no legislative content. | strong | VERIFIED |
| [[chen-2016-most-reliable-path]] | 2016 | qualifies | The method assumes independent link travel times. Sec. 2 notes the assumption can be relaxed with a two-level hierarchical network model (Chen et al. 2012); the Conclusions list correlation first among future directions, saying it is omitted "for simplicity" although urban link times are strongly correlated. Normal approximation of path travel time, checked against Monte Carlo ground truth on 100 randomly selected Wuhan paths, averages 98.3% accuracy at the 10th percentile and 94.9% at the 90th. | Only the structural point transfers: path reliability built as a product or sum of independent per-link terms inherits an independence assumption that the source paper itself flags as its main limitation. The direction and size of correlation among legislator asks is untested. The UI already labels the figure as scenario output, not an empirical probability (tpl:1119, 1155). | moderate | VERIFIED-SOFTENED |
| [[chen-2016-most-reliable-path]] | 2016 | suggests-extension | MRP-TS's optimality is proven (Props. 4-5, Lemmas 1-3) and also checked empirically: it returns the same optimal paths as an independent multi-criteria A* on the same budgets, across 100 O-D pairs. | Chen et al.'s validation design (exact algorithm vs an independent exact solver on the same inputs) transfers directly, since it is a software check, not an empirical claim. The direction mismatch matters only when PE is asymmetric. That happens when the bundle carries emult or the issue's role priors are non-unit; with neutral multipliers the walk is exact. The size of the effect is bounded by the EMU range [0.80, 1.20] and has not been measured. | moderate | VERIFIED |
| [[ahmad-nd-shortest-path-algorithms]] |  | qualifies | Influence is asymmetric: if Y is highly influenced by X, the reverse is not guaranteed, even for undirected interactions such as co-authorship (Sec. 3.2; Sec. 1.1). Asymmetry 'has to be considered' in social search; the paper does not say in so many words that edges must be directed. Average node influence is 0.67 (Twitter) and 0.60 (DBLP), and most node influence values lie below one (Sec. 5.3, Table 2). | The paper's asymmetry point is enough to say edge direction matters for path search. The defect comes from reading the code, not from the paper. Its size depends on how far EMU (0.80-1.20) and the role priors push PE[u][v] away from PE[v][u], and on whether dropped nodes lie on the best chain. It may be small. A second path should confirm it (DE-062), for example by comparing Dijkstra on -log of the transposed PE with the traced chain. | moderate | VERIFIED-SOFTENED |
| [[baptista-2022-universal-multilayer-network]] | 2022 | suggests-extension | RWR scores proximity by summing over walks rather than taking a single path, and RWR has been shown to outperform local distance measures for gene-disease prioritization (Kohler et al. 2008, ref 21). The paper defines RWR as steady-state proximity from all nodes to the seed but does not use the phrase summing over all walks. | An RWR score seeded at the target, or at the pro-sponsor set, would add a multi-route robustness measure beside the single best chain. The paper's support comes from biological prioritization, not recruitment, and gives no congressional estimate. Any such score is a heuristic proximity, never an ask-success or access probability (D-025). | weak | VERIFIED-SOFTENED |
| [[sotoudeh-2024-network-based-measure]] | 2024 | qualifies | The paper measures whether bill-level aggregates of cosponsors' pairwise pass-rate scores separate passed from failed bills. It does not study recruitment, paths, ask success or per-edge probabilities. It never compares its measure against raw cosponsor count; it only cites prior work that cosponsor count is not a clear signal of success (pp. 3-4, refs 29 and 23). | None. The paper's outcome is bill passage, not whether a target joins as cosponsor, and its score is not a probability. The citation overstates what the paper shows, and the probability wording conflicts with D-025. | strong | VERIFIED |
| [[cao-2025-social-network-group]] | 2025 | suggests-extension | Trust propagation is done either with propagation operators along one path (six operator families) or by aggregating trust over multiple paths. Propagation has four properties: symmetry, follow-through, directness and divergence. Sun et al. (2023) and Tan et al. (2022) study optimal paths and long-chain effects. | Taking a product along one path is the multiplicative operator family the review lists. Aggregating over alternative paths would turn the floor into a fuller estimate. This only matters if the edge probabilities mean something empirically, and the cosponsor-kernel p and ASKM are uncalibrated. The paper offers a concept, not evidence that multi-path aggregation predicts real recruitment. | weak | UNVERIFIED |
| [[luo-2020-path-planning-algorithm]] | 2020 | suggests-extension | Step 6 replans when the next node is more than r = 20 m away or the target has moved. The search runs backward from the goal (step 2). Plain D* Lite is suited to short-range or local planning with little environmental change. | Only the idea of refreshing a goal-rooted field after the graph changes transfers. The paper's speed numbers do not (3D grids of about 1M cells vs a 533-node graph). At this size a recompute that skips dropped nodes is enough; incremental D* Lite is not needed. | moderate | VERIFIED-SOFTENED |
| [[xu-2024-improved-lite-based]] | 2024 | qualifies | Paper says greedy path extraction after map changes can deadlock where a predecessor cost value exceeds its successor value (Fig. 3b). Its fix (Eq. 6, Algorithm 2) picks the unexplored successor minimizing passage cost plus g and excludes explored nodes. The 18,468 vs 28,351 freezing node counts compare the whole improved method (local update plus extraction plus path optimization) with the original D* Lite, not Eq. 6 alone. The paper does not say the g values are out of date or made consistent with the change. | The mechanism is general graph search and does not depend on ice data, so it applies to the 533-node cosponsor graph whenever the member-out scenario is active. The paper's timings (single voyage, no hardware stated) do not transfer. Fix is a full recompute that honors dropped, which is cheap at this size. | moderate | VERIFIED-SOFTENED |
| [[zeller-2016-step-path-selection]] | 2016 | qualifies | Whole-path selection (PathSF) gives coefficients of larger magnitude than step-wise selection (SSF), and different characteristic scales for every cover type except chaparral. The road-crossing sites also differ between step-based and path-based models. Composing steps does not reproduce whole-path behaviour. | Loose analogy. The paper shows that animal movement along a whole path is not the composition of independent steps. Legislators recruited through a chain may likewise not act as independent hops. The evidence concerns habitat selection, not social asks, so it only motivates checking hop independence against observed cosponsor join sequences. It does not show that the product rule is wrong here. | weak | UNVERIFIED |
| [[abdelrahman-2025-conflict-based-search]] | 2025 | suggests-extension | D* Lite reuses earlier search results when costs change, so a route can be repaired instead of recomputed. The authors credit this for faster conflict resolution in dynamic settings, but they report no replanning counts. | The extension would need an observed event: a failed ask or a stance move that changes edge probabilities while a campaign is under way. Only then would incremental repair, or simply recomputing the route, give a reroute-aware chain hold. The paper gives no evidence about recruitment success. Edge p values are heuristic kernel outputs, not ask-success probabilities (D-025 wording applies). | weak | UNVERIFIED |
| [[ng-1999-policy-invariance-reward-transformations]] | 1999 | qualifies | Corollary: shaped Q' = Q - Phi and V' = V - Phi, with Phi(absorbing) = 0 in the undiscounted case, so the shaped and original problems share the same argmax over actions. Remark (Robustness and learning): the identities hold for arbitrary policies, so an epsilon-accurate potential keeps policies epsilon-near-optimal. Choosing Phi = V* makes V' identically zero, which the authors call an easy value function to learn. | Deterministic max-product over per-ask probabilities, equivalent to a -log p shortest path (comment at line 735). Applies only when the dropped set is non-empty. Drops are set only from the drop text box in applyShocks (line 1165); the URL parameter read at line 1537 is coal, not drops, so the earlier URL claim is removed. The browser-level failure stays UNVERIFIED under DE-062; the logic failure is reproduced in a replica. | strong | VERIFIED-SOFTENED |

**Assessment.** Strong rows, Chen 2016 and Sotoudeh 2024, *qualify* the system rather than support it.

**What holds.** A max-product path over independent per-hop probabilities is additive in -log p, so exact Dijkstra is the right tool (Chen 2016; Baptista 2022 confirms the ordering). At N = 533 an O(N^2) array scan is fine; Duan 2025 adds nothing. With an empty dropped set and a symmetric PE, the greedy trace is exact (Ng 1999: dT satisfies the Bellman equation).

**What does not hold.**
- *Dropped-member defect (actual error).* `distToTarget` (tpl ~line 734) does not exclude `dropped`; `tracePath` (~740) skips dropped members, so the greedy descent follows dT values that route through excluded members. A logic replica gave chain 0.009 vs a best drop-avoiding chain of 0.36, and reached=false while a path existed (Ng 1999 row; Luo 2020 and Xu 2024 rows independently describe the same mechanism). Browser-level reproduction is still UNVERIFIED. Fix: recompute dT with dropped members excluded; that is cheap at this size, and no D* Lite is needed.
- *Direction mismatch (UNVERIFIED lead).* PE is directed (receiver EMU[v], source_shock/target_resistance), but dT[v] is computed along t -> v rather than v -> t (Ahmad, Chen-2016 suggests-extension rows). Bounded by EMU 0.80-1.20; vanishes with neutral multipliers. Needs a second-path test: Dijkstra on -log of transposed PE vs the traced chain.
- *'Conservative floor' wording.* chainHold multiplies activeP as if asks were independent. Chen 2016 names independence as its own main limitation; asks in one chain share the bill, salience, party context and the ASKM lever, so they are likely positively correlated, and under positive correlation the product is not a floor. Zeller 2016 (UNVERIFIED, analogy) points the same way. Paths are also ranked on passive reliability while chainHold is shown on the active-ask scale.
- *ASKM = 0.07 (about a 14x odds shift) has no source.* No corpus paper estimates the ask lift.

**Online scan (leads, not findings):** Zelizer 2019 APSR, two randomized field experiments, finds cue-taking spreads through a self-selected legislator communication network. It is the closest causal design for an ask lift, but it is **state-legislature** evidence with an information treatment, not a member-to-member ask in Congress. Rogowski & Sinclair 2012 (Political Analysis), using the House office lottery for the 105th-112th Houses, finds **no** effect of proximity on cosponsorship. That cautions against reading kernel edges as ask channels. Box-Steffensmeier, Christenson & Craig 2019 (AJPS) archive about 94k House Dear Colleague letters (106th-111th; 44% solicit cosponsors). It is observational, but it is the data that could measure ask lift in Congress.

**Contradictions.**
- The code comment says 'PROVABLY-OPTIMAL'. That is false whenever the dropped set is non-empty (reproduced in the logic replica), and possibly false when PE is asymmetric (UNVERIFIED).
- The code comment calls chainHold a 'conservative floor', but a product of per-hop probabilities is a floor only under independence or negative correlation. Correlated asks in one chain contradict it (Chen 2016 limitation).
- Sotoudeh 2024 is cited for the chain and for 'mathematical certainty' probability wording. The paper measures bill passage, not recruitment or per-edge probability, and the wording conflicts with D-025.
- Chen 2016 is cited as grounding for the most-reliable chain. It solves a deadline on-time-arrival problem that is non-additive and cannot be solved by Dijkstra, which is a different object.
- recruitCost cites Furnas 2019 for 'money is capped, info is the lever'. The paper never tests dollar amounts; the $1k-$10k bound rests on FEC law.

**Gaps.**
- ASKM (the active-ask lift): no study estimates it. Fill with a design that records asks: (a) observational, Dear Colleague cosponsor solicitations (Box-Steffensmeier et al. archive) linked to BILLSTATUS cosponsor dates, estimating join hazard for solicited vs unsolicited members with party, ideology and committee controls; (b) causal, a Zelizer-style randomized information or ask treatment, which exists only for a state legislature and cannot be borrowed for Congress without a transfer caveat.
- Hop independence: untested. Fill by checking observed cosponsor join sequences for correlated joining along kernel paths, for example whether a second-hop member's join probability conditional on the first hop exceeds the product.
- Path length vs time-to-join (the component's second empirical question): no evidence. Fill with a survival analysis of cosponsor join dates on kernel path length from the sponsor, reported with its window and denominator.
- Kernel edge p as an ask-success proxy: no evidence. Rogowski & Sinclair 2012 (scan lead) is a null for proximity effects on cosponsorship, so homophily vs contagion must be separated before any causal reading.

**Paul's position vs the evidence.** Paul's central gap is turning lobbying and bipartite data into a graph where path-finding can run. The corpus does not support this as an edge source, and D-021 forbids it: LDA is an identity source, not an edge or weight source. Kim & Kunisky's communities live in an organisation x member bipartite graph and cannot become member-member edges. Paul's interest in A* heuristics that cope with unstable information is not supported at this scale: exact Dijkstra on 533 nodes is cheap, and no paper shows a learned heuristic helping a graph this size. Ng 1999 only shows that potential-based shaping keeps optimal routes. The defect that matters is the dropped-set and direction bug, which needs a correct exact recompute, not a heuristic. Paul's note that the built chain is most-reliable-path Dijkstra matches the code.

**Open questions.**
- Is the direction mismatch real in the browser build? Second-path test: compare the traced chain against Dijkstra on -log(PE transposed) with emult and role priors on.
- Should chainHold be relabelled from 'conservative floor' to 'scenario product under independence'? The UI already says scenario output (tpl:1119, 1155).
- Does Paul want ASKM estimated, which needs ask data, or kept as an explicit scenario dial with a sensitivity band?
- Should the Sotoudeh and Chen 2016 citations be re-scoped in the template comments to classical max-product / -log p shortest path?

**Read first.** [[ng-1999-policy-invariance-reward-transformations]], [[chen-2016-most-reliable-path]], [[sotoudeh-2024-network-based-measure]], [[ahmad-nd-shortest-path-algorithms]], [[furnas-2019-moneyed-interests-information]], [[xu-2024-improved-lite-based]]

## Best-sponsor, multi-target tree and budget max-coverage modes

**Computes.** Recruitment trees and budget-bounded recruit sets with coverage cov[v] = 1 - prod(1 - p).

**Method.** Greedy max coverage (1-1/e) with locality and degree pruning (GCOMB), plus grow() spread with recruitability x log(deg) x proximity.

**Code refs.** `fec/templates/make_dashboard_html_00.tpl (budgetRecruit, multiTarget, grow)`

**Assumptions.**
- Approximate submodularity on the cosponsor graph
- Community structure is exploitable (Porter-Mucha-Newman; Kim-Kunisky)

**Evidence weight:** 10 rows (2 VERIFIED, 8 VERIFIED-SOFTENED, 0 UNVERIFIED); 10 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2020-mapping-political-communities]] | 2020 | qualifies | Communities are latent memberships in the bipartite interest-group x politician lobbying network (k = 8, 113th Congress). Legislators with the most community memberships are often senior politicians or party leaders; legislators with entropy at least 2 (roughly 4+ communities) sit on power committees. The dc-biSBM, being a hard partition, assigns mixed actors (CHAMBER OF COMMERCE, BRITISH PETROLEUM, MCAFEE) to only one community, and the LSM places high-entropy actors in an undifferentiated centre region. | Does not carry over as cited: the paper's communities belong to an organisation x member lobbying graph, not the member-member cosponsor graph, and the code uses no communities. The brain note's citation should be re-scoped. If a community term is ever added, the paper argues for mixed membership over a hard partition because hub actors are the ones a partition misassigns. | strong | VERIFIED-SOFTENED |
| [[porter-2005-network-analysis-committees]] | 2005 | qualifies | Porter et al. (2005) find hierarchical clusters in a committee-to-committee co-membership network built from House assignment data (single-linkage on normalized interlocks, about four levels). They do not study cosponsorship or recruitment communities. | The citation mismatch stands: the Porter 2005 paper does not ground the cosponsor recruitment code. The Sotoudeh citation (line 738) grounds passage influence, a different claim. Community detection exists elsewhere in fec/network/press_hcoi.py (Louvain, line 64) but is not used in pathfinding. | moderate | VERIFIED-SOFTENED |
| [[trivedi-2020-efficient-influence-maximization]] | 2020 | qualifies | Under MICM on NetPhy the paper reports percent gains over ICM: DHICM +23.7% at k=30 and CELF +55.55% at k=10. These are different k values and gains over ICM, so they do not measure a head-to-head DHICM vs CELF gap under MICM. The paper does not say its discount assumes small p, and it does not test one-hop versus multi-hop spread. | The paper's MICM is a different edge-probability form from the kernel, so it cannot say how one-hop coverage fares at the kernel's p-range. The claim that the kernel reaches p of about 0.52 was not checked: the cited sector-networks note is not in the repo. Any approximation-quality claim stays UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED-SOFTENED |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | The (1-1/e) greedy guarantee holds for the monotone submodular objective greedy maximizes (Sec 3.1-3.2 pp.6-8). Under IC with full-adoption feedback, observing which asks succeed keeps adaptive submodularity (Thm 19 p.21). | Without a target the classical guarantee holds for reach. With a target the selection score is not the marginal of any fixed submodular function, so the (1-1/e) comment overstates the claim. An adaptive version would need recruit outcomes fed back, which is not built. | moderate | VERIFIED |
| [[ma-2022-adversarial-attack-graph]] | 2022 | suggests-extension | Corollary 1 Eq. 6 with a = 0.01 over B = M^L (L = 4) is a saturated coverage function and is submodular. The paper says this objective outperforms one-step and centrality heuristics. The paper's experiments compare against Degree, Betweenness, PageRank, RWCS and GC-RWCS; no one-step coverage baseline is tested. | The 1-1/e guarantee applies to plain greedy on the monotone coverage objective. It does not transfer cleanly to the target-weighted variant, where argmax of (1 + 2 dT) times marginal is not greedy on the reach objective. A multi-hop extension would replace p with an L-step walk weight on the kernel; on the cosponsor graph that is a heuristic reach score (D-025), not an introduction probability, and must not reintroduce the rejected per-edge volume normalization. Weak. | weak | VERIFIED-SOFTENED |
| [[arya-2025-relink-edge-activation]] | 2025 | qualifies | The IM-CSN objective is non-submodular (Introduction, citing Huang et al. 2022). The paper gives no optimality guarantee for its own policy and lists optimality under submodular objectives as future work (Sec 7). In Fig 4 (k=20, uniform weights) Random and PSNA generally perform best among baselines, followed by FoF and Degree. On dense graphs (email-Eu-core, wiki-Vote, sx-mathoverflow) PSNA beats RELINK at smaller seed sets (Sec 6.3, Fig 6). | The non-submodularity point and the warning that degree- or hub-favouring heuristics are weak under edge budgets carry over only as structural context. The paper's comparisons use public networks, mostly under IC with random weights, and its dense-graph losses to PSNA say nothing about the 533-member House graph. Hub convergence cannot be judged from the paper. | moderate | VERIFIED-SOFTENED |
| [[chen-2023-complex-contagion-influence]] | 2023 | qualifies | The (1-1/e) greedy guarantee of Kempe et al. 2003 applies to submodular IC and LT. Under complex contagion no such guarantee holds, and CCIM is NP-hard to approximate even under simplified network models (citing Schoenebeck and Tao 2019). | The guarantee is true of the coverage proxy only, not of real recruitment. Dashboard wording should say so if threshold joining is plausible. The target-weighted branch multiplies marginals by (1 + 2 dT), which lies outside the classic argument, so the guarantee should not be cited for targeted calls. | weak | VERIFIED |
| [[kamarthi-2020-influence-maximization-unknown]] | 2020 | supports | A learned query policy is needed only because the graph is unknown. OPT is greedy influence maximization on the full graph G* and is the ceiling; rewards are scaled as (influence minus CHANGE) over (OPT minus CHANGE), Eq. 1. With the graph known, greedy is the reference standard. | The paper premise (learning pays only when edges are hidden) applies only if the cosponsor graph is a partial sample of a latent network. The 1-1/e guarantee holds for greedy on the graph as observed, not for the true network if ties are missing. | moderate | VERIFIED-SOFTENED |
| [[dai-2017-learning-combinatorial-optimization-graphs]] | 2017 | qualifies | On set covering built from the MemeTracker-derived graph, averaged over 1000 test graphs, Dai et al. report ratios of S2V-DQN 1.001, LP 1.0009 and Greedy 1.03. This is minimum set cover, not budgeted maximum coverage, and the paper makes no general claim about coverage objectives. | Dai's set cover is minimum cover; budgetRecruit is budgeted maximum coverage with a target reweighting and degree pruning, so the 3 percent greedy gap does not transfer directly. 533 nodes is inside Dai's tested range of up to 1200, but there is no training distribution. The guarantee does not hold strictly under the reweighting and pruning. | weak | VERIFIED-SOFTENED |
| [[manchanda-2020-gcomb-learning-budget-constrained]] | 2020 | method-source | Locality is neighbours not yet covered by the partial solution (Eq 3). Greedy reaches at least 99 percent of optimal on small graphs (Table 1b: BP-500 b=20 GCOMB 0.998, greedy 0.99, optimal 1). GCOMB matches greedy on real graphs and wins mainly on time at scale. | The 1-1/e bound holds for cardinality-constrained monotone submodular coverage, which this objective is. Paper evidence for greedy near-optimality is only on 500 to 900 node graphs (Table 1b); applying it to a 533-member graph is an extrapolation. The GCOMB Eq 3 locality is a Q-learning state feature. The code uses the plain coverage marginal, which greedy already contains, so the GCOMB label overstates the mapping. The degree prune is a fixed rule, not the learned noise predictor. | strong | VERIFIED-SOFTENED |

**Assessment.** The rows agree with each other, and the evidence is fairly firm on the algorithm.

**What holds.** Noisy-OR coverage cov = 1 - prod(1-p) is monotone submodular, so plain cardinality-bounded greedy carries the (1-1/e) bound **on that coverage proxy** (Golovin 2011; Manchanda 2020). With the graph known, greedy is the reference standard and no learned policy is needed (Kamarthi 2020). The one live call passes target -1, so the live path is plain greedy.

**What does not hold.**
- The guarantee is about the proxy, not about real recruitment. Under complex or threshold joining no such guarantee exists (Chen 2023).
- When a target is set, marginals are multiplied by (1 + 2 dT). That score is not the marginal of a fixed submodular function, so the (1-1/e) comment overstates the targeted branch (Golovin, Ma 2022, Chen 2023, Dai 2017).
- The comment says 'cost-bounded', but the loop stops at nSeeds; the $k budget becomes a seed count via CEDGE (Ma 2022, Dai 2017).
- The GCOMB label overstates the mapping. The code uses the plain coverage marginal plus a fixed degree prune (max(1, 0.1 x mean degree)), not GCOMB's learned locality or noise predictor, and greedy's near-optimality is shown only on 500-900 node graphs (Manchanda Table 1b).
- Coverage is one hop; there is no multi-hop reach (Trivedi 2020, Ma 2022).
- **Community grounding has no code counterpart.** Kim & Kunisky's communities are latent memberships in an interest-group x politician lobbying graph, and Porter 2005 is committee co-membership. Neither is the cosponsor graph, and `budgetRecruit` and `multiTarget` contain no community term. Louvain exists only in press_hcoi.py.

**Flags.** Ma 2022's multi-hop extension (an L-step walk weight) must not reintroduce per-edge volume normalization, which was measured and REJECTED in this project. A community term built from the Kim-Kunisky lobbying graph would make LDA an edge or weight source, which D-021 bars.

**Contradictions.**
- The comment claims (1-1/e) for the target-weighted branch. Golovin 2011 and Chen 2023 show the guarantee applies only to unweighted monotone submodular greedy.
- The comment calls budgetRecruit cost-bounded, but the code is cardinality-bounded at nSeeds.
- Kim & Kunisky 2020 and Porter 2005 are cited (brain note, component assumption) for exploitable cosponsor community structure. They describe a lobbying bipartite graph and a committee co-membership graph, and the code uses no communities.

**Gaps.**
- Community-local vs cross-community recruitment (the component's empirical question): no evidence. Fill with an observational join-rate study on BILLSTATUS cosponsor dates. Partition the cosponsor graph itself (not the LDA graph), compare join rates of within- vs cross-community targets of a sponsor's existing cosponsors, and control for party, ideology and committee overlap, because homophily is the main confound (Rogowski & Sinclair 2012 null, scan lead). Report by party stratum or label UNSTRATIFIED.
- Whether real joining is threshold-like (complex contagion), which would void the guarantee for real outcomes: untested. Fill by testing whether a member's join hazard rises nonlinearly with the number of already-joined neighbours.
- Greedy approximation quality on the 533-member kernel at its actual p-range: not measured. Fill with an exact or ILP comparison on this graph; the cited 0.52 p-range figure was not verified.

**Paul's position vs the evidence.** Paul's position is that GCOMB pruning and community structure are the grounding and match his addendum. The evidence partly disagrees. GCOMB maps only loosely: the built code is plain greedy max coverage with a fixed degree prune, which is fine and well grounded by Nemhauser and Golovin, but it is not GCOMB. Community structure is not used by the code at all. Its two cited sources describe different graphs, and building it from the lobbying bipartite graph would breach D-021. The stronger, accurate grounding is the classical submodular coverage result, scoped to the proxy and the untargeted call.

**Open questions.**
- Re-scope the brain note's Kim-Kunisky and Porter citations, or add a community term built from the cosponsor graph itself?
- Drop the (1-1/e) claim from the targeted branch's comment, or restrict the comment to target = -1?
- Rename 'cost-bounded' to 'seed-count-bounded (budget / CEDGE)'?

**Read first.** [[golovin-2011-adaptive-submodularity-theory]], [[manchanda-2020-gcomb-learning-budget-constrained]], [[chen-2023-complex-contagion-influence]], [[kim-2020-mapping-political-communities]], [[porter-2005-network-analysis-committees]], [[kamarthi-2020-influence-maximization-unknown]]

## Cosponsor recruitment route finder (Pareto front)

**Computes.** Greedy routes from champion seeds with at most K = 5 recruits per recruiter, giving a Pareto front over cost, expected cosponsors and risk.

**Method.** Edge cost = CEDGE/max(0.1, p); value = (gain - lambda(1-p))/cost for lambda in {0, 0.5, 1, 2, 4}. Optional MC reach. MOPBD* (Ren et al.) is cited as concept only.

**Code refs.** `fec/engine/cosponsor_route.py:1-89`

**Assumptions.**
- Recruiter whip capacity is bounded
- Edge activations are independent

**Evidence weight:** 9 rows (2 VERIFIED, 6 VERIFIED-SOFTENED, 1 UNVERIFIED); 8 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[lawande-2022-systematic-review-analysis]] | 2022 | context | GBF 'will always try to pick the best track at that particular moment' (F(n)=H(n)); it is fastest of the four implemented but not optimal. D*/D* Lite is described with pseudocode in Section 3.2.3 but is not among the four algorithms implemented or benchmarked. | Analogy only: the route finder solves a budgeted coverage problem, not point-to-point shortest path. The paper has no D* Lite measurements, so it cannot justify building the replanning layer. | weak | VERIFIED |
| [[chen-2016-most-reliable-path]] | 2016 | suggests-extension | Mean-Budget dominance with multi-criteria label-setting finds exact non-dominated path sets fast. 11,087-18,115 non-dominated paths are handled in 162-276 ms on Wuhan. The FSD label-correcting benchmark generates 225,878 paths, of which 95.09% are M-B dominated. | This is a weak, conceptual transfer. Chen's dominance rule is specific to normal path times and a path objective. The cosponsor router grows a recruitment tree with a recruiter capacity cap K, so exact label-setting would need a different dominance relation. It is relevant only if Paul wants the front to be exact rather than illustrative. | weak | VERIFIED |
| [[arya-2025-relink-edge-activation]] | 2025 | method-source | IM-CSN as edge activation: fixed seed set, each node may activate at most k of its out-edges (k in 5, 10, 20), reward is marginal IC spread. A trained Double DQN edge policy beats Random, Degree and FoF in 462, 464 and 462 of 480 configurations and PSNA in 456 of 480. Abstract says up to 15% higher spread; Sec 6.3 says improvements frequently exceed 15%, so the two statements differ. | Only the framing transfers: edge actions under a per-node budget. The paper's numbers are for a trained DDQN policy on simulated IC spread over public online graphs with random or uniform 0.5 weights. The built routes are greedy heuristics, so no RELINK result transfers to them. Any citation of 15% or 456/480 for Melange routes would be a misattribution. | moderate | VERIFIED-SOFTENED |
| [[luo-2020-path-planning-algorithm]] | 2020 | qualifies | Plain D* Lite is adequate for short-range planning where the map changes little. The paper's speed gains come from shrinking 100x100x100 m and 120x120x100 m grids, with path lengths differing by up to about 8 percent (Env4, 337 vs 311 m). | The paper's own scale result argues against adding D* Lite to a 533-node graph where full recomputation is cheap. Map zooming trades optimality for speed, which the cosponsor objective does not want. Revisit only if routes are recomputed live on a much larger graph. | weak | VERIFIED-SOFTENED |
| [[xu-2024-improved-lite-based]] | 2024 | context | Paper uses cost = delta*r + d (Eq. 12). Raising delta from 1 to 3 at Icemax 1.2 m (freezing) lowers average risk from 0.076 to 0.039 and lengthens the route from 2,558.7 to 2,645.5 n mi (+3.4%) (Table 5). Paper calls delta = 3 sufficient based on simulation experiments it does not show. The text does not call them unpublished. The paper shows no Pareto front. | The two works share only the weighted-sum risk cost. The system reports a front while the paper fixes one delta. Weighted-sum scalarization can miss non-convex front points in both. | weak | VERIFIED-SOFTENED |
| [[ren-2022-multi-objective-path]] | 2022 | qualifies | MOPBD* returns the exact Pareto-optimal set of source-to-goal paths (Theorem 1: C = C*). Multi-objective shortest path is NP-hard even with two objectives, and the front can grow exponentially with the number of nodes. | The paper claim concerns source-target paths with additive vector costs. The route finder uses a product-form reach and a mean-p risk over a recruitment set, so MOPBD* cannot be dropped in. The docstring overstates the method; the front is a heuristic approximation, not an exact Pareto set. | strong | VERIFIED-SOFTENED |
| [[ren-2022-multi-objective-path]] | 2022 | qualifies | Incremental reuse helps when cost changes are few and local. On the maze map with random obstacle add and delete (M = 2, 3, 4; 5 minute limit), MOPBD* beats from-scratch NAMOA* in general on median and average runtime, but its advantage shrinks as M grows and from-scratch NAMOA* wins at M = 4. The worst case can exceed from-scratch search when many states are invalidated. | The paper evidence covers single-robot grid planning with local edge changes at 1 to 5 minute limits on simulated grids in Python. It does not cover whole-network batch refresh or changes to global weights such as lambda or K. | moderate | VERIFIED-SOFTENED |
| [[abdelrahman-2025-conflict-based-search]] | 2025 | suggests-extension | Replacing A* with incremental D* Lite inside CBS cuts mean execution time from 5.575 s to 0.04 s (99.2%) and nodes explored from 2142 to 250.5 (88.3%), at about 0.8% longer paths (15.73 vs 15.6). However, the 8-point case explores more nodes with D* Lite (198 vs 178). The number of runs is not reported. | The speedup does not transfer. It was measured on 3D voxel grids with moving obstacles, while this graph has 533 nodes and recomputing it costs very little. What carries over is the idea of repairing a route when a member joins, declines or changes stance, rather than rebuilding it. The K=5 whip cap loosely resembles a CBS conflict over a shared resource, but CBS resolves conflicts between agents competing for space and time, not recruiter bandwidth. Treat the paper as a design pointer only (weak, single-source, unreplicated, UNVERIFIED). | weak | UNVERIFIED |
| [[dai-2017-learning-combinatorial-optimization-graphs]] | 2017 | suggests-extension | A Q-function over a graph embedding, trained by n-step Q-learning with fitted Q-iteration, can serve as a greedy evaluation function and generalizes from 50-100 node training graphs to 1000-1200 node test graphs, with MaxCut BA ratio 1.0038 and MVC BA ratio 1.0062 (Table 2, 1000 test instances). | Requires a simulated instance distribution, for example cascades on perturbed or topic-specific kernels, as training data, which the system lacks. A learned Q-value is a heuristic score, never an ask-success or access probability (D-025). Any result stays UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED-SOFTENED |

**Assessment.** Every row qualifies the component or is context. None supports the method as built.

**What is built.** A greedy recruitment-tree builder runs once for each lambda in {0, 0.5, 1, 2, 4}, scoring (gain - lambda(1-p))/cost under a K = 5 per-recruiter cap. Non-dominated (cost, expected cosponsors, risk) points are kept. MC reach is off by default (MCMODE=1). K and CEDGE = 10000 are unestimated env defaults.

**Against the cited sources.**
- Ren 2022 (MOPBD*) returns the **exact** Pareto set of source-to-goal paths with additive vector costs. The route finder works on recruitment *sets* with a product-form reach and a mean-p risk, so MOPBD* cannot be dropped in. The docstring's 'RELINK + MOPBD*' overstates the method: the front is a heuristic approximation.
- Scalarizing over five lambdas cannot reach points in non-convex parts of the front (Chen 2016, Xu 2024).
- RELINK (Arya 2025) transfers as framing only: edge activation under a per-node budget. Its numbers come from a trained DDQN on simulated IC over public graphs. Quoting '15%' or '456/480' for Melange routes would be a misattribution, and the paper's abstract and Sec 6.3 disagree with each other about the 15% figure.
- Incremental replanning: Ren 2022 shows the MOPBD* advantage shrinks as changes grow, and from-scratch search wins at M = 4. Luo 2020's own result says plain D* Lite suits small, local changes. At 533 nodes with batch refresh, full recompute is cheap, so the evidence argues **against** building a D* Lite layer now. Abdel-Rahman 2025 (UNVERIFIED; run count not reported) is a design pointer only: repair a route when a member joins, declines or moves stance.
- Dai 2017 (learned greedy) needs a training distribution of instances that the system lacks.

**Assumptions with no evidence:** bounded whip capacity (K = 5) and independent edge activations.

**Contradictions.**
- The docstring credits 'RELINK + MOPBD*', but the code is a lambda-parameterised greedy over recruitment sets with no path search and no replanning. Ren 2022 returns an exact Pareto set, which this code does not produce.
- The output is presented as a Pareto front, but weighted-sum scalarization over five lambdas cannot reach non-convex front points. The front is illustrative, not complete.

**Gaps.**
- Whip capacity (K = 5): no study. Fill with an observational yield curve: per sponsor and Congress, count cosponsor solicitations (Dear Colleague archive, scan lead) against cosponsors gained, and test whether marginal recruits fall off past some count. Report window and denominator.
- Edge-activation independence: untested. This is the same gap as the recruitment path; correlated joining on a shared bill would bias the MC reach.
- CEDGE = 10000 and the lambda grid: no grounding. They are scenario dials; at minimum, run a sensitivity sweep showing how the front moves.

**Paul's position vs the evidence.** Paul marked D* dynamic replanning 'Very interesting' and made D* Lite and LPA* the operational layer in his addendum. The evidence does not support building it at the current scale. Every replanning paper is grid robotics; the incremental advantage shrinks with many changes; and a 533-node recompute is trivial. The valid kernel of Paul's idea is reroute-on-event: update the plan when an ask fails or a stance moves. That needs a stream of observed ask outcomes, which does not exist. Once it does, a plain recompute delivers the same answer as D* Lite at this size. The multi-objective concept is reasonable, but it should be described as an approximate front.

**Open questions.**
- Should the docstring drop 'MOPBD*' and describe the method as a 'lambda-scalarized greedy front (approximate)'?
- Does Paul want an exact front (which needs a recruitment-set dominance relation, not Chen or Ren path labels), or is illustrative enough?
- Is there any planned live in-campaign event feed? Without one, the D* Lite layer has nothing to replan on.

**Read first.** [[ren-2022-multi-objective-path]], [[arya-2025-relink-edge-activation]], [[chen-2016-most-reliable-path]], [[luo-2020-path-planning-algorithm]], [[abdelrahman-2025-conflict-based-search]]

## Staffer closeness route search

**Computes.** The least-cost introduction route to staff anchored at a member office. Hop cost = -ln(0.8 x strength) + w x uncertainty; terminal cost includes a member-context modifier (beta 0.15).

**Method.** Bidirectional Dijkstra with heuristic 0 (labelled A*) and Yen loopless alternatives. The default route search is one-directional to a virtual sink (staffer_closeness_routes.py:110-138). Ties within 1e-12 relative: fewer steps wins, then lower cost. The office search excludes uncovered staffers and reports the count.

**Code refs.** `fec/surfaces/staffer_closeness_routes.py:1-30,78-138,911-999`, `fec/surfaces/staffer_routing.py:381`

**Assumptions.**
- The minimum-cost path approximates the most usable introduction
- The prestige modifier is a bonus, not access evidence

**Evidence weight:** 16 rows (2 VERIFIED, 11 VERIFIED-SOFTENED, 3 UNVERIFIED); 13 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | suggests-extension | Former Senate staffers turned lobbyists experience a persistent 24% drop in lobbying revenues when their connected senator exits office (Blanes i Vidal et al. 2012); drops are larger for connections on Finance, Appropriations or Ways and Means and statistically insignificant otherwise. Bertrand et al. (2014) find lobbyists take up new issues as the politicians they contacted change committee assignments. The review flags B-i-V's exogeneity of senator exit as a strong assumption. | The evidence measures lobbyists' revenue, not introduction success, closeness or influence. Under D-025 and the revolving-door wording rule, an added term may be described only as dependence on the patron's current office, never as access or influence. The review text does not state Blanes i Vidal's sample years. Absence of an office/committee term was checked only in these two files, not repo-wide (UNVERIFIED as a system-wide gap). | moderate | VERIFIED-SOFTENED |
| [[furnas-2019-moneyed-interests-information]] | 2019 | qualifies | Staff are gatekeepers for Members' time and recommend bills and cosponsorships. Taking a meeting, though, is the lowest-commitment step: it barely varies with requester identity, and whether staff side with a requester depends on the evidence's ideological alignment, not on access. | This supports treating staffer routes as routes to a contact point only. Under D-025, route cost is a heuristic and not an access or introduction probability. The paper adds that even a secured meeting predicts little about action, so route outputs should never be labelled as influence or outcome. It is limited to DC staff in 2017 and to stated responses. | weak | VERIFIED-SOFTENED |
| [[duan-2025-breaking-sorting-barrier]] | 2025 | qualifies | O(m log^(2/3) n) beats Dijkstra's O(m + n log n) only on sparse graphs, for single-source distances to ALL vertices on directed graphs with real non-negative weights, in the comparison-addition model. The analysis runs on a constant-degree transform with O(m) vertices and edges (m = O(n)). It returns distances without a full ordering; Dijkstra is optimal when the ordering is required (HHR+24). The constant C in Lemma 3.12 is not quantified and nothing is implemented. | Carries over only if the system needs full single-source distance fields over the staffer graph (one origin to every office). The graph size is not established here: the ~4.66M figure traces to the co-tenure GUARANTEED pair count (4,660,388, staffer-sorting memory), which is an edge count, so the analyst's n ~ 10^6-10^7 is unsupported. The asymptotic ratio vs Dijkstra is about log^(1/3) n (roughly 2.5-3x for n in 10^6-10^7) before an unknown constant. Does not carry over to early-stopping point-to-sink, bidirectional, Yen or exact-tie passes, and does not touch the measured memory bottleneck. No recommendation to change the router. | weak | VERIFIED-SOFTENED |
| [[duan-2025-breaking-sorting-barrier]] | 2025 | context | Assumption 2.1 (all paths have distinct lengths) is made WLOG by ordering each path as the tuple <length l, vertex count alpha, vertices read from the end v_alpha..v_1> lexicographically, comparable in O(1) using Pred[] and alpha stored per bd[v]. The paper notes this is done like in many shortest-path papers. | Analogous, not identical: the paper compares exact length first and uses vertex count only on exact ties; the system lets a 1e-12 relative cost band count as a tie and then puts steps BEFORE exact cost. Both read the vertex sequence from the end. The paper's order is a presentation device for its own analysis and does not validate the system's tolerance band or step-first rule. | weak | VERIFIED-SOFTENED |
| [[lawande-2022-systematic-review-analysis]] | 2022 | qualifies | Informed (A*) search cuts expansions via an estimate H(n); the paper's estimates are grid/geometric. Dijkstra is described as using no heuristic (H(n)=0, F(n)=G(n)) and as extended to A* by adding a heuristic. | No spatial embedding, so the paper's Euclidean estimates do not apply. A* gains would need an admissible non-metric estimate (e.g. ALT landmarks, deferred), which the paper does not test. Supports keeping h=0, not adding A* by name. | weak | VERIFIED-SOFTENED |
| [[chen-2016-most-reliable-path]] | 2016 | context | Penalizing path variability through SD makes the path objective non-additive, so Dijkstra loses exactness. Exact solutions need multi-criteria labels such as mean and variance. | The paper serves as a boundary condition, not a criticism. The current additive design is correctly solved. If uncertainty were ever restated as an SD aggregated over the route, or as a probability of a usable introduction by a date, Dijkstra would no longer be exact and Chen-style label-setting would apply. Route costs remain heuristic outputs, never introduction or access probabilities (D-025). | weak | VERIFIED |
| [[ahmad-nd-shortest-path-algorithms]] |  | method-source | Algorithm 1 (from [5], Hangal et al.) enumerates all paths, multiplies edge influences along each, and returns the path minimizing Total_influence = log(1/D) + log(1/product of edge influences), with D = 0.95. As printed, log(1/D) is added once per path, not per hop, so it is a constant that cannot change which path wins. Table 2: average strongest vs shortest path length 6.8 vs 4.9 hops (Twitter) and 4.7 vs 3.3 (DBLP), over 500 random pairs per dataset. No path-quality metric is reported. | Only the log-inverse-product part of the objective carries over. The router's per-hop decay is its own design (possibly closer to Hangal's original paper, not checked here), not something Ahmad et al. print. The paper does not validate the objective: it never measures whether strongest paths succeed more often. Its influence is directed and normalized by investment; staffer strength is symmetric exposure. Route cost is a heuristic model output, not an introduction probability (D-025). | moderate | VERIFIED-SOFTENED |
| [[kalla-nd-campaign-contributions-facilitate]] |  | suggests-extension | Who is asked changes which tier takes the meeting. The donor label raised chief-of-staff-or-above access (12.5% vs 2.4%, p=.006) but not access at the district-staff tier (48.4% vs 43.3%, p=.26). | The paper supports recording the seniority of the person reached as a validation target, matching its six-rank coding. It does not justify converting route costs or ranks into access probabilities, which would break D-025. The paper measured a requester attribute in district-office meetings, not a warm introduction through a co-worker tie. | moderate | VERIFIED-SOFTENED |
| [[luo-2020-path-planning-algorithm]] | 2020 | qualifies | Speed comes from a geometric heuristic (max absolute coordinate difference) and from coarsening the map to a quarter of its size. Path lengths differ by up to about 8 percent between the two methods in Table 1. | Neither speed lever transfers. A person-tie graph has no coordinates, so there is no admissible geometric heuristic, and map coarsening would break the proven-optimal contract. The only analog is hierarchical search, which would need its own correctness proof. Route costs remain heuristic outputs, not closeness or introduction probabilities (D-025). | weak | VERIFIED-SOFTENED |
| [[xu-2024-improved-lite-based]] | 2024 | qualifies | Paper's local-update rule (Eq. 5) cuts node updates by 34.9% (freezing) and 79.5% (melting) against the original D* Lite and planning time by 46.2% and 85.7%. Route length differs from full re-planning by about 0.1% (2,558.747 vs 2,561.408 n mi freezing; 2,419.087 vs 2,418.146 n mi melting). The paper gives no optimality guarantee; it reports error below 0.1% against the global re-planning routes. | Incremental replanning helps when the same origin and goal are re-queried after small local edge changes on a large graph. Staffer requests use different origins and targets, so the paper's gains do not transfer directly. The approximate update would conflict with an exact-cost requirement. | weak | VERIFIED |
| [[ren-2022-multi-objective-path]] | 2022 | suggests-extension | Vector costs with a non-dominated set expose trade-offs that a single scalar sum hides. Epsilon-dominance (0.01 to 0.05) gives smaller stored sets and faster runs, at a cost in solution quality; the paper states no set-size or runtime bound. The advantage over from-scratch search shrinks as the number of objectives grows. | The extension applies only if the strength-uncertainty trade-off should be shown as a front rather than fixed by w. It would add latency where route speed is already a concern. Any front members remain heuristic model outputs, never closeness or introduction probabilities (D-025). | weak | VERIFIED-SOFTENED |
| [[zeller-2016-step-path-selection]] | 2016 | qualifies | Resistance is set to the inverse of the predicted relative probability, exp(sum b_i x_i), and the authors state that no empirical evidence supports equating that inverse with resistance. The routes and crossing locations it produces (top-20 current-flow pixels on I-15) shift with method (SSF vs PathSF), with scale (532-7,390 m) and with the paired vs unpaired framework. | Analogy only. In the paper, a fitted selection probability on a 30 m landscape becomes a cost. Here, hand-set exposure strengths between people become a cost. What carries over is the methodological warning: the probability-to-cost transform is an untested assumption, and route choice is sensitive to it. That supports the system's existing non-probability wording (D-025). It gives no evidence about which transform is right for staffer ties. | weak | UNVERIFIED |
| [[zeller-2016-step-path-selection]] | 2016 | suggests-extension | PathSF compares each observed path with its own set of available alternatives in a conditional-logistic case-control design, and so estimates cost coefficients from observed route choices. It outperformed step-wise SSF at every scale except the finest (10-fold CCC 0.992 vs 0.976), and the authors recommend PathSF when there is a choice. | This only applies if an outreach log of attempted and completed introductions is ever recorded. Each used route could then be paired with Yen-alternative routes as the available set, and conditional-logistic weights could be fitted for the hop-cost components. Until then it is a design template, not evidence. Any fitted output would stay a heuristic score, never an access or introduction probability (D-025), and would need a single-look holdout. | weak | UNVERIFIED |
| [[abdelrahman-2025-conflict-based-search]] | 2025 | qualifies | Incremental planning is not always cheaper. In the 8-point case, D* Lite expanded 198 nodes against A*'s 178. The paper also does not report node counts for single-shot queries on unchanged graphs. | D* Lite pays off only when costs change between successive queries on the same graph. Each staffer request starts on a fixed snapshot, so incremental search buys little unless the sidecar starts updating evidence in place between queries. The current design, exact and stateless, has no gap that this paper fills. | weak | UNVERIFIED |
| [[ng-1999-policy-invariance-reward-transformations]] | 1999 | method-source | Theorem (sufficiency and necessity): adding gamma*Phi(s') - Phi(s) for any real Phi leaves every optimal policy unchanged, and without prior knowledge of T and R no other additive shaping term guarantees this. The paper does not discuss A*, heuristics used in search, or bidirectional search. | Holds for deterministic additive costs. Bidirectional search, the 10^12 tie band and Yen ranking are outside the paper and need their own checks in original cost units. The premise that Paul wants learned or landmark heuristics is not in the item (paul_entries is empty) and is unverified here. | moderate | VERIFIED-SOFTENED |
| [[ng-1999-policy-invariance-reward-transformations]] | 1999 | qualifies | Necessity: for any non-potential additive shaping term there exist dynamics and rewards under which the shaped optimum is not optimal in the original problem (Appendix A proves the two-action case). A constant added to every terminal arc telescopes to a potential term Phi(goal) and leaves route ranking unchanged. A term that varies by staffer or per step is not a potential and can change the winner. | Deterministic costs. The code calls the modifier member_context_max_bonus (line 1014) and comments prestige bonus (line 917). D-025 is not cited in this file, so the D-025 wording is not verified here. | moderate | VERIFIED-SOFTENED |

**Assessment.** Most rows are weak or moderate and qualify the component. On the algorithm the evidence is consistent and favourable; on route *meaning* there is nothing.

**Algorithm: sound.**
- Hop cost = -ln(0.8 x strength) + w x uncertainty is a per-hop sum, so exact bidirectional Dijkstra with h = 0 is correct (Chen 2016, as a boundary condition). That stops being true only if uncertainty were restated as a route-level SD or a dated probability.
- A person graph has no geometry, so the Euclidean heuristics in Lawande 2022 and Luo 2020 do not apply. An admissible heuristic would have to be something like ALT landmarks, which are deferred.
- Duan 2025 does not touch early-stopping point-to-sink, bidirectional, Yen or exact-tie passes. The measured bottleneck was memory (pair_cache, 7.0 -> 17.5 GB), not heap cost.
- Incremental replanning (Xu 2024; Abdel-Rahman 2025, UNVERIFIED) buys nothing for stateless queries with differing origins on a fixed snapshot, and Xu's approximate update would break the exact-cost contract.
- The routing cache is FIFO, not LRU (Luo row).

**Objective: partly borrowed, mostly Paul's design.**
- Only the log-inverse-product term matches Ahmad et al. The per-hop ln(1/0.8) decay has no counterpart in Ahmad's printed algorithm, where log(1/D) is a per-path constant.
- Ahmad never measures whether strongest paths succeed more often.
- On Ng 1999: dividing all terminal arcs by (1 + MEMBER_BETA) is a constant shift, so the line-916 comment is correct for that step. But line 919 adds a target-varying anchor uncertainty term, the member-context modifier is per-staffer, and staffer_routing.py:424-425 adds a balance step penalty in the ranking key. All three are non-potential terms that **can change the winning route**. They are legitimate design choices, but they are not ranking-neutral.
- Tie rule (D-082): the system puts steps *before* exact cost inside a 1e-12 band. Duan's lexicographic ordering is a proof device and does not validate this.

**Meaning of a route: no evidence.**
- Zeller 2016 (UNVERIFIED, analogy): the probability-to-cost inversion is an untested assumption, and route choice is sensitive to it.
- Furnas 2019: reaching a staffer gives access to a contact point at most; a meeting predicts little about action.
- Kalla & Broockman: which tier takes a meeting varies with who asks. That supports recording the seniority of the person reached as a validation target, not converting cost to a probability.
- Bombardini 2020 (Blanes i Vidal): former staffers' lobbying revenue drops 24% when their senator leaves office. The router does not read whether the anchoring member still serves (checked in two files only). Any added term may be described only as dependence on the patron's current office, never as access or influence, because these are revolving-door edges.

**Scan leads:**
- McCrain 2018 JOP: shared-office staff connections still on the Hill predict lobbyist first-year revenue (+18% per SD). The outcome is revenue, not introductions.
- Montgomery & Nyhan (JOP): offices that exchange senior staff are more alike in effectiveness and voting.

Both are leads only. Neither measures introductions, and both concern revolving-door or staff-exchange ties, where influence wording is barred.

**Contradictions.**
- The member-context comment says the modifier leaves ranking unchanged. That holds for the constant (1 + MEMBER_BETA) shift, but the per-staffer modifier, the target-varying anchor uncertainty (line 919) and the balance step penalty (staffer_routing.py:424-425) are non-potential terms that can change the winner (Ng 1999).
- The documentation says the router is 'labelled A*', but it runs heuristic 0, so it is Dijkstra. The label is harmless but inaccurate.

**Gaps.**
- Do cheaper or shorter routes correspond to successful introductions (the component's empirical question)? There is zero evidence. Fill with an outreach log of attempted and completed introductions, and fit hop-cost weights with a path-selection conditional logit (Zeller PathSF design): each used route against its Yen alternatives as the choice set, scored on a single-look holdout. Outputs stay heuristic scores (D-025).
- HOP_FACTOR 0.8, the uncertainty weight w, the 5-year staleness half-life and MEMBER_BETA 0.15: none is fitted, and no paper supplies them. They are filled by the same outreach-log study; until then, run sensitivity sweeps.
- Patron-still-in-office dependence: there is evidence for lobbyist revenue (Blanes i Vidal via Bombardini) but none for introductions. Whether the router reads member tenure was checked only in two files; it needs a repo-wide check.
- Robustness to node loss (Paul's 'centerless' goal): no paper in the corpus evaluates it. Fill with a node-removal simulation on the staffer graph that reports how often top-k routes survive, which Yen alternatives partly provide already.

**Paul's position vs the evidence.** Paul wants a centerless design that survives losing a node, and learned heuristics. On robustness there is no evidence in the corpus, which makes it a gap rather than a contradiction. Yen loopless alternatives are the existing mechanism, and a node-removal simulation would measure it. On learned heuristics the evidence weighs against: the router's contract is exact optimal routes; a learned heuristic keeps that only if admissible; no paper tests learned heuristics on person graphs; and the measured bottleneck is memory, not search. ALT landmarks (deferred) are the one admissible option. Note that the premise that Paul wants learned heuristics on this specific router was flagged in one row as not traceable to his own entries.

**Open questions.**
- Should the router read whether the anchoring member is still in office, worded as patron-office dependence only?
- Are the non-potential terms (per-staffer modifier, anchor uncertainty, balance penalty) intended to change winners? If so, the line-916 comment should say that ranking changes beyond the constant shift.
- Is any outreach or introduction log being kept? It is the only route to validating this component.

**Read first.** [[chen-2016-most-reliable-path]], [[ng-1999-policy-invariance-reward-transformations]], [[ahmad-nd-shortest-path-algorithms]], [[zeller-2016-step-path-selection]], [[bombardini-2020-empirical-models-lobbying]], [[kalla-nd-campaign-contributions-facilitate]]

## Ordered person-overlap routes and employment-routing overlay

**Computes.** Routes through dated Politico-reconciled co-work. distance = ceil(1e6 x 128 / strength), with strength = proximity x log2(1 + days/30). Overlay costs: current co-worker 1, historical 3, reported activity 2, filing 5, policy 8.

**Method.** Bidirectional Dijkstra on (cost, steps). User-asserted current co-worker hop at cost 1.

**Code refs.** `fec/surfaces/staffer_overlap_routes.py:1-280`, `fec/surfaces/staffer_routing.py:153-640`, `fec/surfaces/staffer_routing_evidence.py:157-162`

**Assumptions.**
- Log-scaled shared work days proxy relationship strength
- Only Politico career-history evidence creates work ties

**Evidence weight:** 4 rows (2 VERIFIED, 1 VERIFIED-SOFTENED, 1 UNVERIFIED); 4 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | supports | Covered-position disclosures are made once and have no dates, so recency cannot be established. Co-listing on filings shows registrant-lobbyist contracts, not dated co-work. | The paper describes federal LDA lobbyists, and the routes cover congressional staff. The paper's point that filing data cannot date relationships applies directly to the filing-year hops. It says nothing about the dated payroll-overlap hops. Routes are heuristic introduction paths, not access probabilities (D-025). | moderate | VERIFIED |
| [[holyoke-2015-learnable-skills-unteachable]] | 2015 | qualifies | Relationships with lawmakers are built on mutual need and each side's ability to supply something, and are targeted, not personal friendships (p.17-18, introduced as 'arguably'). A lobbyist's market value is their portfolio of relationships (citing Blanes i Vidal et al. 2012). About 52% of roughly 30,000 DC lobbyists (2012, Rehr) came through government, and 48% of those held two or more posts (LaPira and Thomas 2014, as cited p.5). These figures are secondary and the authors did not re-derive them. | Conceptual only. The paper never measures whether shared tenure produces usable relationships. It implies that co-work measures the opportunity for contact, not the mutual need that keeps a relationship going. Its revolving-door share is a federal DC figure from about 2012-2014 and fits the router's staff-to-lobbyist population. It does not support any cost constant or the log-days form. Route cost must stay a heuristic, not an introduction probability (D-025). No influence wording on revolving-door edges. | weak | UNVERIFIED |
| [[duan-2025-breaking-sorting-barrier]] | 2025 | qualifies | The result is for real weights in the comparison-addition model. For integer weights in the word RAM model, the related-work line culminates in Thorup's O(m + n log log min{n, C}) for directed graphs [Tho04], where C is the maximum edge weight. | With integer weights the tighter asymptotic line is integer/word-RAM SSSP (Thorup), not this paper. Matters only if overlap-route latency becomes heap-bound, which has not been shown. | weak | VERIFIED |
| [[ahmad-nd-shortest-path-algorithms]] |  | context | Hop-count shortest paths ignore tie strength and asymmetry (Abstract). Tie strength matters for social search (Granovetter motivation, Sec. 1.1). The strongest path uses the product of normalized influences (Sec. 4). | Context only. The paper does not compare inverse-strength costs with log-product costs, and the overlap router is a deliberate design choice. Shared work days and shared background are heuristic tie scores, not measures of closeness, access or introduction probability (D-025). Influence language is barred on revolving-door edges. | weak | VERIFIED-SOFTENED |

**Assessment.** This is the thinnest component.

**What holds.**
- Bacik 2025 (supports): LDA covered-position disclosures are made once and are undated, so filing co-listing shows contracts, not dated co-work. The system's handling matches: filing-year episodes are routed separately as weak `co_reported_firm_association`, are off by default (include_weak False), and carry cost 5, above historical 3. This also matches D-021 (LDA is an identity source, not an edge or weight source).
- Duan 2025 (weak): costs are integers, so the relevant asymptotic line is integer-weight SSSP (Thorup), not Duan. It matters only if latency becomes heap-bound, which has not been shown.
- Ahmad (context): costs are additive inverse strengths, ceil(1e6 x 128 / strength), not -log of a bounded weight. They cannot be read as products of per-hop reliabilities, so no probability reading exists.

**What is unsupported.**
- Holyoke 2015 (UNVERIFIED, secondary figures): lobbying relationships rest on mutual need and are targeted. Co-work therefore measures the *opportunity* for contact, not a working relationship. Nothing supports the log2(1 + days/30) tenure form or the overlay costs (current 1, reported activity 2, historical 3, filing 5, policy 8).
- The assumption that only Politico career history creates work ties is a sourcing choice. No paper tests it, but Bacik supports excluding filing co-listing as dated co-work.

**Scan leads:** McCrain 2018 counts connections as shared-office staff still on the Hill, a presence measure, and links them to revolving-door revenue. It does not test overlap *duration*, so it neither supports nor refutes the log-days form. No influence wording is allowed on these edges.

**Contradictions.**
- (none)

**Gaps.**
- Does shared tenure length (log days) predict a successful warm introduction (the component's empirical question)? There is no evidence. Fill with the outreach-log design used for closeness routes, testing tenure length against presence-only and linear-days forms on a held-out look.
- Overlay cost constants (1 / 2 / 3 / 5 / 8) and DISTANCE_SCALE: unestimated. Until outcome data exist, the only fill is a sensitivity sweep showing how often top routes change.
- Proximity weights inside strength: no source. Fill alongside the tenure-form test.
- As a proxy outcome before any introduction log exists, a McCrain-style design could test whether overlap duration, not just presence, predicts a downstream observable. Any result would describe the value of a tie, never access or influence.

**Paul's position vs the evidence.** Paul treats professional and revolving-door ties as central to lobbying. The evidence agrees that these ties carry market value: Blanes i Vidal via Bombardini, Holyoke's secondary figures on government-to-lobbying flows, and McCrain 2018 as a scan lead. But it measures lobbyist revenue, not introduction success. Holyoke's own framing (targeted, mutual-need relationships) implies that shared tenure is only a proxy for the opportunity for contact. So Paul's premise is supported, but the specific route mechanics (log-days strength, overlay constants) are not. Revolving-door edges must carry no influence language.

**Open questions.**
- Is the log2(1 + days/30) form a deliberate prior Paul wants to keep, or should it be tested against presence-only, which is what McCrain uses?
- Should the 'reported activity' (cost 2) and 'policy' (cost 8) overlay hops be checked against D-021, to confirm neither derives an edge from LDA records?

**Read first.** [[bacik-2025-measuring-dynamical-evolution]], [[holyoke-2015-learnable-skills-unteachable]], [[ahmad-nd-shortest-path-algorithms]], [[duan-2025-breaking-sorting-barrier]]

## Across components

## Cross-component patterns

1. **The algorithms are right; the numbers have no grounding.** All five components use textbook-correct search or greedy procedures for their graph sizes. None has a single empirically estimated parameter: ASKM, K, CEDGE, HOP_FACTOR, MEMBER_BETA, the staleness half-life, the overlay costs and the log-days form are all unestimated. Every output is a heuristic model output (D-025). Evidence on algorithm choice cannot substitute for evidence on parameters.

2. **One missing data asset blocks validation of everything.** The member graph needs observed asks and their outcomes. The staffer graph needs an outreach or introduction log. Both are the same kind of asset. Without them:
   - no hop-independence test
   - no ask-lift estimate
   - no route-to-success check
   - no fitted costs (a Zeller PathSF-style conditional logit would need them)
   - nothing for incremental replanning to replan on

   The Dear Colleague archive (scan lead) is the only existing congressional record of asks, and it is observational.

3. **Code comments claim more than the code does,** and this repeats across components:
   - 'PROVABLY-OPTIMAL' fails with drops.
   - 'conservative floor' fails under correlation.
   - '(1-1/e)' fails on the targeted branch.
   - 'cost-bounded' is actually a seed count.
   - 'MOPBD*' implies an exact front the code does not produce.
   - 'GCOMB' labels plain greedy.
   - 'A*' labels an h = 0 search.

   Citations overstate in the same way: Sotoudeh, Chen 2016, Furnas, Kim & Kunisky, Porter. These are wording fixes, apart from the dropped-set defect, which is a real logic error.

4. **The speed literature does not apply.** D* Lite, Duan's SSSP bound, A* and learned greedy all target scales or dynamics this system lacks: 533 members, and a staffer router whose measured bottleneck is memory. This consistently weighs against Paul's interest in replanning layers and learned heuristics as next steps. Correctness fixes and outcome data matter more.

5. **Independence is assumed in two places.** chainHold and the cosponsor-route MC reach both assume independent per-edge events. Chen 2016 flags independence as its own main limitation, and correlated asks on a shared bill are the plausible default. Both components share the same untested gap.

6. **Transfer conditions.**
   - Zelizer's causal cue-taking evidence comes from a state legislature. The state-level staff-sharing non-replication noted in the scan shows that congressional and state results can diverge.
   - Kim & Kunisky's and Porter's graphs are not the cosponsor graph.
   - The revolving-door value evidence (Blanes i Vidal, McCrain) measures revenue, not introductions.

7. **Rule checks.**
   - D-021: LDA must stay identity-only. This constrains Paul's 'lobbying/bipartite data into a graph' aim and any community term built from Kim & Kunisky.
   - REJECTED proposals: Ma 2022's multi-hop walk weights must not reintroduce per-edge volume normalization. No paper here argues for dropping regular cosponsorship edges.
   - Revolving-door wording: no influence language on the staffer edges.

## Research agenda

1. Fix and test, not literature: exclude dropped members inside distToTarget, then run a second-path test (Dijkstra on -log of transposed PE, with emult and role priors on) to confirm or kill the direction-mismatch lead. Add a regression test that drops a bridging member.
2. Ask-lift study (fills ASKM and whip capacity K): link the Box-Steffensmeier/Christenson/Craig Dear Colleague archive (106th-111th, ~94k letters, 44% cosponsor solicitations; copy in scan_2026-10-10) to BILLSTATUS cosponsor dates. Estimate join hazard for solicited vs unsolicited members and a per-sponsor yield curve, controlling for party, ideology and committee. Report window and denominator, and stratify by party or label UNSTRATIFIED.
3. Hop-independence test: from observed cosponsor join sequences, test whether joins along kernel paths are positively correlated. If they are, relabel chainHold and the MC reach as products under independence, not floors.
4. Start an outreach/introduction log (attempted, completed, seniority of contact reached) as the outcome asset for both staffer components. Pre-register a PathSF-style conditional logit (used route vs Yen alternatives) with a single-look holdout to fit HOP_FACTOR, w, MEMBER_BETA, the overlay costs and the log-days form.
5. Get Zelizer 2019 APSR main text (only the supplement is downloaded) and McCrain 2018 JOP (no open copy found; try the Emory ETD dissertation chapter). Retry Fowler 2006 'Connecting the Congress', which failed on DNS. Put Rogowski & Sinclair 2012 (office-lottery null on cosponsorship) through adversarial verification before citing.
6. Community-local recruitment study: partition the cosponsor graph itself (not the LDA graph; D-021) and compare within- vs cross-community join rates against homophily controls, before any community term enters budgetRecruit or multiTarget.
7. Node-removal robustness simulation on the staffer graph (Paul's centerless goal): how often top-k routes survive removal of high-betweenness people. This is reporting only, with no ranking claims.
8. Wording pass on code comments and citations: PROVABLY-OPTIMAL, conservative floor, (1-1/e) on the targeted branch, cost-bounded, MOPBD*, GCOMB, A*, and the Sotoudeh / Chen 2016 / Furnas / Kim-Kunisky / Porter citations.
