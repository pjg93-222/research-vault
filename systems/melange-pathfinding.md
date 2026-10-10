---
type: system
name: Melange path-finding (recruitment and introduction routes)
slug: melange-pathfinding
components:
  - melange-pathfinding.member-recruitment-path
  - melange-pathfinding.multi-target-and-budget-modes
  - melange-pathfinding.cosponsor-route-pareto
  - melange-pathfinding.staffer-closeness-routes
  - melange-pathfinding.staffer-overlap-routes
brain_notes:
  - actors/melange-dashboard-june-capstone
  - actors/staffer-live-sidecar
paper_count: 26
tags: [system]
---

# Melange path-finding (recruitment and introduction routes)

Route search over two graphs: member-to-member recruitment chains on the cosponsor kernel (dashboard pathfinder modes and cosponsor_route.py), and staffer introduction routes on the people/office graph (closeness routes, ordered overlap routes and the employment-routing overlay). Route objectives, tie rules and coverage rules live here; the graphs themselves are built in melange-sna and melange-staffer-network.

**Brain notes:** [melange-dashboard-june-capstone](obsidian://open?vault=brain&file=actors%2Fmelange-dashboard-june-capstone) · [staffer-live-sidecar](obsidian://open?vault=brain&file=actors%2Fstaffer-live-sidecar)

## Melange path-finding: evidence assessment

**Scope.** This covers 49 evidence rows across 5 components, taken from the hub file `_work/state/hub/melange-pathfinding.json`. There are 18 VERIFIED rows, 24 VERIFIED-SOFTENED rows and 7 UNVERIFIED rows. UNVERIFIED rows are treated as leads, not findings (DE-062).

| Component | Rows | VERIFIED | SOFTENED | UNVERIFIED |
|---|---|---|---|---|
| member-recruitment-path | 14 | 4 | 7 | 3 |
| multi-target-and-budget-modes | 9 | 3 | 6 | 0 |
| cosponsor-route-pareto | 8 | 2 | 5 | 1 |
| staffer-closeness-routes | 14 | 2 | 9 | 3 |
| staffer-overlap-routes | 4 | 2 | 1 | 1 |

**Bottom line.** Every paper bearing on this system is about algorithms, analogies or wording. None tests the system's outcome. No row shows that any of these things predicts real-world success:

- a cosponsor-kernel path,
- the ASKM ask lift,
- a recruiter's whip cap,
- a staffer route cost,
- a shared-tenure strength.

The two empirical questions each component asks are therefore **GAPs in every case**.

What the evidence does settle:

1. **Several citations ground the system in name only.** These are Chen et al. 2016, Sotoudeh et al. 2024, Furnas et al. 2019, Porter et al. 2005, Kim & Kunisky, MOPBD* and RELINK. Each paper solves a different problem from the one the code cites it for.
2. **One optimality claim is overstated, and it is a VERIFIED contradiction.** The budget mode claims the (1-1/e) guarantee even when it applies the target weight. The guarantee for the weighted rule falls to 1-e^(-1/3) = 0.283.
3. **Exact Dijkstra is the right tool at the system's sizes.** The 533-member House graph and an additive staffer cost are both small and simple enough for it. The D* Lite, A* and Duan-2025 papers do not justify a change.
4. **Code-reading defects are leads only.** Two single-path readings flag problems in the "PROVABLY-OPTIMAL" member chain: PE asymmetry against the reverse-direction field, and the dropped set. Under DE-062 they stay UNVERIFIED until a second path reproduces them.

**Wording.** All path scores, chain holds and route costs are heuristic model outputs. They are never closeness, access, introduction or ask-success probabilities (D-025). Influence wording is barred on revolving-door edges.

## Sponsor-to-target max-product path and chain hold

**What it computes.** The most reliable recruitment chain from a sponsor to a target, with per-hop $ and probability; chainHold = product of activeP per hop.

**Method.** Dijkstra on -log p. activeP(p) = p/(p + (1-p) x ASKM) with ASKM 0.07 (about a 14x odds shift, unestimated). recruitCost(p) = 1000 + 9000(1-p), clamped to $1k-$10k.

**Code refs.**
- `fec/templates/make_dashboard_html_00.tpl (buildPath, chainHold, recruitCost)`

**Assumptions.**
- Cosponsor-kernel edge probabilities proxy ask success
- Active asks raise odds by a constant factor (no study pins it)
- Money is capped by FEC limits; difficulty shows up in reliability, not dollars

**Evidence.**

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

**Assessment.** **Evidence weight: moderate on algorithm semantics, nil on the empirical premise.** There are 14 rows: 4 VERIFIED, 7 VERIFIED-SOFTENED and 3 UNVERIFIED.

**What the VERIFIED rows establish**

- **Chen et al. 2016 (strong, qualifies) is the wrong grounding.** Chen et al. solve a non-additive on-time-arrival objective, Phi((b-mean)/SD). They say outright that ordinary shortest-path methods cannot solve it, which is why they use label-setting (MRP-TS) rather than Dijkstra. The dashboard chain instead maximizes a product of edge probabilities, which is additive in -log p. Dijkstra is the right tool for that, and the right citation is the classical most-reliable-path or -log p result (Frank / Bellman), not Chen.
- **Chen et al. 2016 (suggests-extension) gives a usable validation design.** The paper checks its exact algorithm against an independent exact solver on the same inputs. That design transfers directly as a software test.
- **Sotoudeh, Porter & Krishnagopal 2024 (strong, qualifies) does not transfer.** The paper measures bill passage from aggregated pairwise pass-rate scores. It does not study recruitment, paths or ask success. Its score is not a probability, and it never compares its measure against raw cosponsor counts. The template comment cites it for a 'PROVABLY-OPTIMAL' chain with 'mathematical certainty', which overstates the paper and conflicts with D-025.
- **Duan et al. 2025 is irrelevant at n = 533.** Its gain over Dijkstra is asymptotic only.

**What the VERIFIED-SOFTENED rows add**

- **Independence (Chen 2016).** The source paper flags independent links as its own main limitation. Asks within one chain share the bill, its salience, the party context and the ASKM lever, so their outcomes are plausibly positively correlated. Under positive correlation, the product of per-hop terms is *not* the 'conservative floor' the code comment claims.
- **Misattributed citation (Furnas 2019, contradicts).** Furnas is cited for 'money is capped, info is the lever'. The paper never tests dollar amounts. The $1k-$10k clamp rests on FEC contribution limits, which are law, so the cost function stands without the citation.
- **Direction matters (Ahmad).** The paper's point that influence is asymmetric supports treating edge direction as relevant to the search.
- **Greedy extraction can fail after the graph changes (Lawande, Luo, Xu).** A greedy walk over a goal-rooted field that ignores dropped nodes can stall or return a suboptimal chain. At 533 nodes the remedy is a full recompute that honours the dropped set. Incremental D* Lite is not needed.
- **Multi-route robustness (Baptista RWR).** An RWR score could add multi-route robustness beside the single best chain, but only as heuristic proximity.

**UNVERIFIED leads (Cao, Zeller, Abdel-Rahman).** These suggest checking hop independence against observed join sequences and adding reroute-aware repair. None is evidence.

**Contradictions.**
- Sotoudeh et al. 2024 is cited for a PROVABLY-OPTIMAL chain and 'mathematical certainty', but the paper measures bill passage, not recruitment or ask success, and its score is not a probability (VERIFIED, strong). The probability wording conflicts with D-025.
- Chen et al. 2016 is cited as grounding for maximizing the probability that the whole chain holds, but the paper solves a non-additive deadline-reliability objective that Dijkstra cannot solve. The built max-product chain is a different object (VERIFIED, strong).
- Furnas et al. 2019 is cited for 'money is capped', but the paper never tests dollar amounts. The clamp rests on FEC law, so this is a misattribution (VERIFIED-SOFTENED, contradicts).
- The code calls chainHold a 'conservative floor', but an independent product is not a floor on joint success when outcomes are positively correlated. Chen 2016 itself flags independence as its main limitation (VERIFIED-SOFTENED).
- The code labels the chain 'PROVABLY-OPTIMAL', but two code readings disagree with that. dT is computed from the target in the reverse direction over an asymmetric PE (EMU range 0.80-1.20, plus role priors), and distToTarget ignores the dropped set while tracePath skips it. These are single-path readings, UNVERIFIED under DE-062, and need a second-path reproduction such as Dijkstra on -log of the transposed PE with a bridging member dropped.

**Gaps.**
- **ASKM = 0.07 (about a 14x odds shift) has no evidence.** No row estimates how much a direct ask from an existing cosponsor lifts the chance a colleague joins, compared with passive diffusion. Two studies could fill this. One is an observational study of dated cosponsor joins that links Dear Colleague letters or recorded member-to-member asks to later joins. It should be stratified by party and same- versus cross-party pairs, and should state the Congress window and bill denominator. The other is a staff survey experiment in the Furnas or Kalla style that varies the requester (a member colleague versus an outside group).
- **Nothing shows that path reliability predicts time-to-join.** A prospective time-to-event analysis would fill this: compute most-reliable path length or reliability at bill introduction, then model the time until each target cosponsors. It needs a single-look holdout on a later Congress, stated as N bills and N members, with a stratified or UNSTRATIFIED label across the party fault line.
- **Nothing tests hop independence.** A test could compare observed joint join rates along multi-hop chains with the product of the per-hop rates. Zeller (UNVERIFIED) is only an analogy for this.

**Paul's position vs the evidence.** **Paul's view.** Paul's central path-finding gap is turning lobbying and bipartite data into a graph that path-finding can run on, and he wants A* heuristics that cope with unstable information.

**What the evidence says about search.** None of it supports A* or a learned heuristic here. At 533 nodes, exact Dijkstra on -log p is cheap and correct. The D* Lite papers (Luo, Xu; VERIFIED-SOFTENED) say a full recompute is enough at this size. The real defects the evidence points to are correctness issues: edge direction and the dropped set. They are not speed issues.

**What the evidence says about lobbying data.** Building edges from LDA would breach D-021, under which LDA is an identity source, not an edge or weight source. The bipartite lobbying communities in Kim & Kunisky belong to an organisation x member graph and would need a non-LDA tie source before any route could run on them.

**What the evidence says about unstable information.** It is better served by keeping uncertainty additive, so that Dijkstra stays exact. Repair-on-event (Abdel-Rahman, UNVERIFIED) is a design pointer only.

**Open questions.**
- Should the template comments drop the 'PROVABLY-OPTIMAL' and 'mathematical certainty' wording, and replace the Sotoudeh, Chen 2016 and Furnas citations with the classical -log p reliable-path grounding and the FEC limit?
- Can a second path reproduce the direction and dropped-set defect before anyone acts on it (DE-062)? For example, compare the traced chain with Dijkstra on -log of the transposed PE, with and without a bridging member dropped.
- Is there any observable record of member-to-member asks, such as Dear Colleague letters, that could put a real estimate on ASKM?
- Should chainHold be relabelled as a scenario figure under independence, rather than a floor?

**Read first:** [[chen-2016-most-reliable-path]], [[sotoudeh-2024-network-based-measure]], [[ahmad-nd-shortest-path-algorithms]], [[xu-2024-improved-lite-based]], [[furnas-2019-moneyed-interests-information]], [[baptista-2022-universal-multilayer-network]]

## Best-sponsor, multi-target tree and budget max-coverage modes

**What it computes.** Recruitment trees and budget-bounded recruit sets with coverage cov[v] = 1 - prod(1 - p).

**Method.** Greedy max coverage (1-1/e) with locality and degree pruning (GCOMB), plus grow() spread with recruitability x log(deg) x proximity.

**Code refs.**
- `fec/templates/make_dashboard_html_00.tpl (budgetRecruit, multiTarget, grow)`

**Assumptions.**
- Approximate submodularity on the cosponsor graph
- Community structure is exploitable (Porter-Mucha-Newman; Kim-Kunisky)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2020-mapping-political-communities]] |  | qualifies | Communities are latent memberships in the bipartite interest-group x politician lobbying network (k = 8, 113th Congress). Legislators with the most community memberships are often senior politicians or party leaders; legislators with entropy at least 2 (roughly 4+ communities) sit on power committees. The dc-biSBM, being a hard partition, assigns mixed actors (CHAMBER OF COMMERCE, BRITISH PETROLEUM, MCAFEE) to only one community, and the LSM places high-entropy actors in an undifferentiated centre region. | Does not carry over as cited: the paper's communities belong to an organisation x member lobbying graph, not the member-member cosponsor graph, and the code uses no communities. The brain note's citation should be re-scoped. If a community term is ever added, the paper argues for mixed membership over a hard partition because hub actors are the ones a partition misassigns. | strong | VERIFIED-SOFTENED |
| [[porter-2005-network-analysis-committees]] | 2005 | qualifies | Porter et al. (2005) find hierarchical clusters in a committee-to-committee co-membership network built from House assignment data (single-linkage on normalized interlocks, about four levels). They do not study cosponsorship or recruitment communities. | The citation mismatch stands: the Porter 2005 paper does not ground the cosponsor recruitment code. The Sotoudeh citation (line 738) grounds passage influence, a different claim. Community detection exists elsewhere in fec/network/press_hcoi.py (Louvain, line 64) but is not used in pathfinding. | moderate | VERIFIED-SOFTENED |
| [[trivedi-2020-efficient-influence-maximization]] | 2020 | qualifies | Under MICM on NetPhy the paper reports percent gains over ICM: DHICM +23.7% at k=30 and CELF +55.55% at k=10. These are different k values and gains over ICM, so they do not measure a head-to-head DHICM vs CELF gap under MICM. The paper does not say its discount assumes small p, and it does not test one-hop versus multi-hop spread. | The paper's MICM is a different edge-probability form from the kernel, so it cannot say how one-hop coverage fares at the kernel's p-range. The claim that the kernel reaches p of about 0.52 was not checked: the cited sector-networks note is not in the repo. Any approximation-quality claim stays UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED-SOFTENED |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | The (1-1/e) greedy guarantee holds for the monotone submodular objective greedy maximizes (Sec 3.1-3.2 pp.6-8). Under IC with full-adoption feedback, observing which asks succeed keeps adaptive submodularity (Thm 19 p.21). | Without a target the classical guarantee holds for reach. With a target the selection score is not the marginal of any fixed submodular function, so the (1-1/e) comment overstates the claim. An adaptive version would need recruit outcomes fed back, which is not built. | moderate | VERIFIED |
| [[ma-2022-adversarial-attack-graph]] | 2022 | suggests-extension | Corollary 1 Eq. 6 with a = 0.01 over B = M^L (L = 4) is a saturated coverage function and is submodular. The paper says this objective outperforms one-step and centrality heuristics. The paper's experiments compare against Degree, Betweenness, PageRank, RWCS and GC-RWCS; no one-step coverage baseline is tested. | The 1-1/e guarantee applies to plain greedy on the monotone coverage objective. It does not transfer cleanly to the target-weighted variant, where argmax of (1 + 2 dT) times marginal is not greedy on the reach objective. A multi-hop extension would replace p with an L-step walk weight on the kernel; on the cosponsor graph that is a heuristic reach score (D-025), not an introduction probability, and must not reintroduce the rejected per-edge volume normalization. Weak. | weak | VERIFIED-SOFTENED |
| [[arya-2025-relink-edge-activation]] | 2025 | qualifies | The IM-CSN objective is non-submodular (Introduction, citing Huang et al. 2022). The paper gives no optimality guarantee for its own policy and lists optimality under submodular objectives as future work (Sec 7). In Fig 4 (k=20, uniform weights) Random and PSNA generally perform best among baselines, followed by FoF and Degree. On dense graphs (email-Eu-core, wiki-Vote, sx-mathoverflow) PSNA beats RELINK at smaller seed sets (Sec 6.3, Fig 6). | The non-submodularity point and the warning that degree- or hub-favouring heuristics are weak under edge budgets carry over only as structural context. The paper's comparisons use public networks, mostly under IC with random weights, and its dense-graph losses to PSNA say nothing about the 533-member House graph. Hub convergence cannot be judged from the paper. | moderate | VERIFIED-SOFTENED |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | contradicts | An alpha-approximate greedy picks e' with Delta(e') >= (1/alpha) max_e Delta(e). Thm 4 then gives 1 - e^{-1/alpha} at l=k. The full 1-1/e needs alpha=1. | The weighted rule as written gives alpha=3: if w_c in [1,3] and the chosen c maximises w_c*g_c, then g_chosen >= g_c/3 for every c. The unweighted coverage objective is monotone submodular, so Thm 4 with alpha=3 gives 1 - e^{-1/3} = 0.283. Caveat the rel missed: the candidate pool is pruned (DEG below 0.1 times mean degree, the dropped set, the stanceSafe filter) and the sponsor is forced, so even the untargeted 1-1/e is relative to the pruned pool, not the global optimum. | strong | VERIFIED |
| [[chen-2023-complex-contagion-influence]] |  | qualifies | The (1-1/e) greedy guarantee of Kempe et al. 2003 applies to submodular IC and LT. Under complex contagion no such guarantee holds, and CCIM is NP-hard to approximate even under simplified network models (citing Schoenebeck and Tao 2019). | The guarantee is true of the coverage proxy only, not of real recruitment. Dashboard wording should say so if threshold joining is plausible. The target-weighted branch multiplies marginals by (1 + 2 dT), which lies outside the classic argument, so the guarantee should not be cited for targeted calls. | weak | VERIFIED |
| [[kamarthi-2020-influence-maximization-unknown]] | 2019 | supports | A learned query policy is needed only because the graph is unknown. OPT is greedy influence maximization on the full graph G* and is the ceiling; rewards are scaled as (influence minus CHANGE) over (OPT minus CHANGE), Eq. 1. With the graph known, greedy is the reference standard. | The paper premise (learning pays only when edges are hidden) applies only if the cosponsor graph is a partial sample of a latent network. The 1-1/e guarantee holds for greedy on the graph as observed, not for the true network if ties are missing. | moderate | VERIFIED-SOFTENED |

**Assessment.** **Evidence weight: strong on the approximation guarantee, which is contradicted; weak on the community-structure grounding.** There are 9 rows: 3 VERIFIED and 6 VERIFIED-SOFTENED.

**What the VERIFIED rows establish**

- **Golovin & Krause 2011, 'approach' paper (strong, contradicts).** Untargeted, budgetRecruit maximizes probabilistic coverage 1 - prod(1-p), which is monotone submodular, so (1-1/e) holds *relative to the pruned candidate pool*. With a target set, each marginal is multiplied by (1 + 2 dT), which lies in [1, 3]. That makes the rule an alpha = 3 approximate greedy, and Theorem 4 then gives 1 - e^(-1/3) = 0.283, not 0.63. The code comment claims the full (1-1/e) in both cases.
- **Golovin & Krause 2011, 'theory' paper.** It confirms the classical bound for the untargeted case. It also shows an adaptive version would need recruit outcomes fed back, and that is not built.
- **Chen, complex contagion (n.d.).** Under threshold-style joining, which is complex contagion, no such guarantee exists and the problem is NP-hard to approximate. The guarantee therefore holds for the coverage proxy, not for real recruitment. The one live call passes target = -1, so the weighted branch is not exercised there.

**What the VERIFIED-SOFTENED rows add**

- **Kim & Kunisky (strong, qualifies).** Its communities are latent memberships in an interest-group x member *lobbying* graph (k = 8, 113th Congress), not the member-member cosponsor graph. The code uses no community term at all.
- **Porter et al. 2005.** Its clusters come from committee co-membership. The template does not cite it; the strings 'Porter-Mucha-Newman' and 'Kim-Kunisky' are not in the repo.
- **Kamarthi 2019 (supports).** Greedy is the reference standard when the graph is known, which favours the current greedy design. Caveat: the bound covers the observed graph, not the true network if ties are missing.
- **Code facts from Ma 2022 and Trivedi 2020.** Coverage is one hop only. The 'cost-bounded' comment is wrong, because the code bounds by cardinality (nSeeds).
- **Arya 2025 (RELINK).** It confirms that closedCascadeRoute correctly avoids claiming (1-1/e).

**Flag.** Ma 2022 suggests a multi-hop L-step walk weight. Any such extension must not reintroduce per-edge volume normalization, which was measured and REJECTED in this project.

**Contradictions.**
- The code comment claims the (1-1/e) guarantee, of about 0.63, for budgetRecruit. With a target set, the (1 + 2 dT) multiplier makes it an alpha = 3 greedy, with a bound of 1 - e^(-1/3) = 0.283 (Golovin & Krause 2011, VERIFIED, strong). Even untargeted, the bound is relative to the pruned pool (degree below 0.1 x mean, the dropped set, stanceSafe, a forced sponsor), not the global optimum.
- Kim & Kunisky is cited (in the brain note) for community structure in the cosponsor network that the pathfinder exploits. The paper's communities are in an organisation x member lobbying graph, and the code uses no communities (VERIFIED-SOFTENED, strong).
- Porter et al. 2005 is cited for community structure in the pathfinder. The paper studies committee co-membership, not cosponsorship or recruitment, and the template does not cite it (VERIFIED-SOFTENED).
- The budgetRecruit comment says the selection is cost-bounded. The code bounds it by cardinality (nSeeds) (VERIFIED-SOFTENED).

**Gaps.**
- **Nothing shows that community-local recruits join at higher rates than cross-community ones.** A study could detect communities on the cosponsor graph alone (not on LDA, per D-021), using mixed-membership models per Kim & Kunisky's warning about hub actors. It would then compare join hazards for within-community and cross-community targets on later bills. It should be stratified by party, name the Congress window and the bill and pair denominators, and use a single-look holdout.
- **Nothing shows how one-hop coverage approximates multi-hop spread at the kernel's actual p range.** The claim that the kernel reaches p of about 0.52 was not checked. A simulation could compare one-hop coverage against Monte Carlo IC reach on the real kernel.
- **GCOMB (Manchanda 2020), the cited grounding, has no evidence row.**

**Paul's position vs the evidence.** **Paul's view.** Paul's position is that GCOMB pruning and community structure are the cited grounding, which matches his addendum.

**What the evidence shows.** The evidence does not support that grounding as built:

- No evidence row covers GCOMB.
- The two community-structure citations do not transfer to the cosponsor graph, and the code has no community term.
- The approximation guarantee the mode advertises holds only untargeted, only relative to a pruned pool, and only for a coverage proxy. Under a targeted call the VERIFIED bound is 0.283, and under complex contagion there is no guarantee at all.

**What still holds.** Paul's broader design choice, greedy on a known graph with no ML, is consistent with Kamarthi 2019.

**Constraint on future work.** Adding a community term would need a cosponsor-native partition. Kim & Kunisky's lobbying-graph communities would breach D-021 if they became routing weights.

**Open questions.**
- Should the budgetRecruit comment be restated? One option: (1-1/e) relative to the pruned pool when untargeted, 1-e^(-1/3) when targeted, and a coverage proxy rather than recruitment in both cases.
- Should the brain note's Kim & Kunisky and Porter citations be re-scoped or removed from the pathfinder grounding?
- Is a community term actually wanted? If so, from which non-LDA graph?
- Does anyone need the targeted branch, given that the one live call passes target = -1?

**Read first:** [[golovin-2011-adaptive-submodularity-theory]], [[golovin-2011-adaptive-submodularity-theory]], [[chen-2023-complex-contagion-influence]], [[kim-2020-mapping-political-communities]], [[kamarthi-2020-influence-maximization-unknown]], [[porter-2005-network-analysis-committees]]

## Cosponsor recruitment route finder (Pareto front)

**What it computes.** Greedy routes from champion seeds with at most K = 5 recruits per recruiter, giving a Pareto front over cost, expected cosponsors and risk.

**Method.** Edge cost = CEDGE/max(0.1, p); value = (gain - lambda(1-p))/cost for lambda in {0, 0.5, 1, 2, 4}. Optional MC reach. MOPBD* (Ren et al.) is cited as concept only.

**Code refs.**
- `fec/engine/cosponsor_route.py:1-89`

**Assumptions.**
- Recruiter whip capacity is bounded
- Edge activations are independent

**Evidence.**

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

**Assessment.** **Evidence weight: moderate to strong that the cited methods do not describe what was built; nil on the empirical premise.** There are 8 rows: 2 VERIFIED, 5 VERIFIED-SOFTENED and 1 UNVERIFIED.

**Ren et al. 2022 (MOPBD*), VERIFIED-SOFTENED, two rows**

- **Strong row: the method is not what the docstring says.** MOPBD* returns an exact Pareto set of source-to-goal paths with additive vector costs. The route finder is a lambda-parameterised greedy over recruitment sets, with product-form reach and a mean-p risk. The docstring's 'RELINK + MOPBD*' therefore overstates the method. The front is a heuristic approximation over five lambda runs, and points in non-convex regions cannot be reached (also Xu 2024).
- **Moderate row: incremental reuse helps only for a few local cost changes.** Its advantage shrinks as changes grow, and from-scratch NAMOA* wins at M = 4. A whole-network batch refresh, or a change to lambda or K, is outside what the paper covers.

**Other rows**

- **Luo 2020.** Its own scale result argues against adding D* Lite to a 533-node graph.
- **Lawande 2022 (VERIFIED).** It has no D* Lite measurements, so it cannot justify building a replanning layer.
- **Arya 2025 (RELINK, VERIFIED-SOFTENED).** Only the framing transfers: edge actions under a per-node budget. Its 15% gain and 456-of-480 win counts come from a trained DDQN policy on simulated IC spread, so quoting them for Melange routes would be a misattribution.
- **Chen 2016 (VERIFIED).** An exact front would need a dominance relation that respects the K cap.
- **Abdel-Rahman 2025 (UNVERIFIED).** It is a lead for repair-on-event: repair the route when a member joins, declines or changes stance.

**Unestimated defaults.** K = 5 and CEDGE = 10000 are unestimated environment defaults. MC reach is off by default (MCMODE = 0). The independence of edge activations is assumed, and no row tests it.

**Contradictions.**
- The docstring credits 'RELINK + MOPBD*', but the code is a lambda-parameterised greedy with a dominance filter. It is not an exact Pareto path search, and it is not a trained RELINK policy (Ren 2022, VERIFIED-SOFTENED strong; Arya 2025, VERIFIED-SOFTENED).
- Paul's operational layer is D* Lite and LPA*. The evidence says incremental reuse pays only for a few local changes on large graphs. At 533 nodes, with batch refreshes, a full recompute is cheap, and from-scratch search wins as changes multiply (Ren 2022 moderate, Luo 2020, Xu 2024; all VERIFIED-SOFTENED). The one paper reporting a large D* Lite speedup (Abdel-Rahman) is UNVERIFIED and measured on 3D voxel grids.

**Gaps.**
- **Nothing shows that whip capacity per recruiter is bounded, or that K = 5 is right.** A study could use dated cosponsor joins on a recruiter's own bills, or recorded Dear Colleague recruitment, to test whether prolific recruiters show diminishing marginal joins per additional ask. It should be stratified by party and leadership status, name the Congress window and the recruiter denominator, and use a single-look holdout.
- **Nothing tests the independence of edge activations.** A test could compare joint versus product join rates within recruiter cohorts.
- **No study compares the approximate front against an exact or larger-lambda-grid front** on the real kernel. That comparison is a software check, not an empirical one.

**Paul's position vs the evidence.** **Paul's view.** Paul marked D* dynamic replanning 'Very interesting' and made D* Lite and LPA* the operational layer in his addendum.

**What the evidence says.** It runs against making replanning a priority at the current scale:

- Luo 2020 and Ren 2022 (VERIFIED-SOFTENED) both show incremental search helps only when a few local changes hit a large graph.
- Lawande 2022 (VERIFIED) has no D* Lite measurements.
- The supportive speedup (Abdel-Rahman, 99.2%) is UNVERIFIED and measured on voxel grids.

**What does transfer.** The *concept* of repairing a route after an observed event, such as a member joining, declining or changing stance, is reasonable. At 533 nodes it can be done by a plain recompute.

**The more pressing finding.** The built front is heuristic, and the docstring overstates it as MOPBD*.

**Open questions.**
- Should the docstring be changed from 'RELINK + MOPBD*' to 'lambda-scalarised greedy, heuristic front; MOPBD* and RELINK named as concepts only'?
- Does Paul want the front to be exact, which would need a capacity-aware dominance relation, or is an illustrative front acceptable if it is labelled as such?
- Is there a live event stream (joins, declines, stance moves) that would make replanning meaningful at all?
- What empirical basis, if any, should K = 5 and CEDGE = 10000 rest on?

**Read first:** [[ren-2022-multi-objective-path]], [[arya-2025-relink-edge-activation]], [[luo-2020-path-planning-algorithm]], [[chen-2016-most-reliable-path]], [[xu-2024-improved-lite-based]], [[abdelrahman-2025-conflict-based-search]]

## Staffer closeness route search

**What it computes.** The least-cost introduction route to staff anchored at a member office. Hop cost = -ln(0.8 x strength) + w x uncertainty; terminal cost includes a member-context modifier (beta 0.15).

**Method.** Bidirectional Dijkstra with heuristic 0 (labelled A*) and Yen loopless alternatives. The default route search is one-directional to a virtual sink (staffer_closeness_routes.py:110-138). Ties within 1e-12 relative: fewer steps wins, then lower cost. The office search excludes uncovered staffers and reports the count.

**Code refs.**
- `fec/surfaces/staffer_closeness_routes.py:1-30,78-138,911-999`
- `fec/surfaces/staffer_routing.py:381`

**Assumptions.**
- The minimum-cost path approximates the most usable introduction
- The prestige modifier is a bonus, not access evidence

**Evidence.**

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

**Assessment.** **Evidence weight: moderate that the search is correctly solved; weak on cost semantics; nil on outcome validity.** There are 14 rows: 2 VERIFIED, 9 VERIFIED-SOFTENED and 3 UNVERIFIED.

**What the VERIFIED rows establish**

- **Chen 2016 is a boundary condition that the design satisfies.** The uncertainty penalty is a per-hop sum, so bidirectional Dijkstra with h = 0 stays exact. If uncertainty were ever restated as a route-level SD, or as the probability of an introduction by a date, Dijkstra would lose exactness and label-setting would be needed.
- **Xu 2024: incremental D* Lite does not transfer.** Requests use different origins and targets, and Xu's approximate local update conflicts with the exact-cost contract.

**What the VERIFIED-SOFTENED rows add**

- **Ahmad (moderate, method-source).** It supports only the log-inverse-product part of the hop cost. The per-hop ln(1/0.8) penalty is the system's own design. The paper never tests whether strongest paths succeed more often.
- **Lawande and Luo.** With no spatial embedding there is no admissible geometric heuristic, so they support keeping h = 0 rather than adding A* by name.
- **Duan (two rows).** No change to the router follows. Its lexicographic tie order is an analogy, not validation of the 1e-12 tolerance band or the steps-first rule (D-082).
- **Furnas 2019.** Staff are gatekeepers, but a meeting is the lowest-commitment step. Routes reach a contact point, not an outcome.
- **Kalla & Broockman (moderate).** The tier of the person reached varies with the requester: chief of staff or above 12.5% versus 2.4% under the donor label, in the paper's randomized district-office experiment. The denominator is not recorded in the row. This supports coding the seniority reached as a validation target, not turning costs into access probabilities.
- **Bombardini 2020 (moderate).** Former Senate staffers turned lobbyists lose about 24% of lobbying revenue when their connected senator leaves office (Blanes i Vidal et al. 2012). The sample years are not stated in the review, and the exogeneity assumption is flagged as strong. This suggests a term for whether the patron still holds office. It may be worded only as dependence on the patron's current office, never as access or influence.
- **Ren 2022.** A strength-uncertainty front is possible, at a latency cost.

**UNVERIFIED leads (Zeller, two rows).** These offer the most concrete validation design. Pair each used route with its Yen alternatives as the choice set, then fit the hop-cost weights by conditional logistic regression (PathSF). This needs an outreach log. Zeller also warns that the probability-to-cost transform is an untested assumption that route choice is sensitive to.

**Contradictions.**
- The component description and the evidence rows disagree on which search is the default. The component text says the default route search is one-directional to a virtual sink (staffer_closeness_routes.py:110-138). The Lawande and Duan code readings say the default is bidirectional Dijkstra and that one-directional _search_reference is a test oracle only. The memory note for the latency lane says one-way Dijkstra was kept. This needs a direct read of the live file to settle.
- The method is 'labelled A*' but runs h = 0, which makes it Dijkstra. Lawande and Luo (VERIFIED-SOFTENED) show there is no admissible geometric heuristic on a person-tie graph, which cuts against Paul's wish for learned heuristics unless they are provably admissible.
- Ahmad is the method source for the strongest-path objective, but its edge influence is directed and normalized by investment. That is a per-edge volume normalization, a proposal this project REJECTED. Only the log-inverse-product form should be borrowed, not the normalization (FLAG).

**Gaps.**
- **Nothing shows that cheaper or shorter routes correspond to successful introductions.** This needs an outreach log of attempted introductions, each with its route, its Yen alternatives, the outcome, and the seniority reached (Kalla's six-rank coding). Hop-cost weights would then be fitted by PathSF conditional logistic regression (Zeller design, a lead), with a single-look holdout. Outputs stay heuristic scores, never access probabilities (D-025).
- **Nothing tests robustness to node loss.** Paul's centerless design has no supporting evidence row. A study could remove nodes, such as staff departures, and measure how far the route set and costs degrade. That is a software and simulation check on the real graph.
- **Nothing supports HOP_FACTOR 0.8, MEMBER_BETA 0.15, the uncertainty weight w, or the five-year staleness half-life.** All are unfitted.
- **There is no term for the patron's current office or committee.** Bombardini (via Blanes i Vidal) motivates one. Its absence was checked only in two files, so whether this is a system-wide gap is UNVERIFIED.

**Paul's position vs the evidence.** **Paul's view.** Paul wants a centerless design that stays intact when a node is lost, plus learned heuristics. The router uses h = 0.

**On learned heuristics.** The evidence supports h = 0 as correct, not as a shortfall. A person-tie graph has no coordinates (Lawande, Luo), and any learned heuristic would need to be admissible to keep the exact-optimal contract. ALT landmarks, which are admissible, were deferred in the latency lane. The measured bottleneck there was memory (pair_cache), not heap cost, so a heuristic would not address it.

**On centerless robustness.** No evidence row addresses it. Yen loopless alternatives give partial redundancy, but nothing measures degradation when a node is lost.

**Where the evidence does point.** It points toward outcome validation: an outreach log plus PathSF. It does not point toward a faster search.

**Open questions.**
- Which is the live default: the bidirectional search or the one-way search to a virtual sink? The hub text and the code readings disagree.
- Does Paul keep or plan an outreach log that records attempted and completed introductions and the seniority reached?
- Should a term for the patron's current office or committee be added? It could only be worded as dependence on the patron's current office, never access or influence.
- Is the 'labelled A*' wording in metadata and docs worth correcting to 'Dijkstra (h = 0)'?

**Read first:** [[zeller-2016-step-path-selection]], [[kalla-nd-campaign-contributions-facilitate]], [[bombardini-2020-empirical-models-lobbying]], [[ahmad-nd-shortest-path-algorithms]], [[chen-2016-most-reliable-path]], [[furnas-2019-moneyed-interests-information]]

## Ordered person-overlap routes and employment-routing overlay

**What it computes.** Routes through dated Politico-reconciled co-work. distance = ceil(1e6 x 128 / strength), with strength = proximity x log2(1 + days/30). Overlay costs: current co-worker 1, historical 3, reported activity 2, filing 5, policy 8.

**Method.** Bidirectional Dijkstra on (cost, steps). User-asserted current co-worker hop at cost 1.

**Code refs.**
- `fec/surfaces/staffer_overlap_routes.py:1-280`
- `fec/surfaces/staffer_routing.py:153-640`
- `fec/surfaces/staffer_routing_evidence.py:157-162`

**Assumptions.**
- Log-scaled shared work days proxy relationship strength
- Only Politico career-history evidence creates work ties

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | supports | Covered-position disclosures are made once and have no dates, so recency cannot be established. Co-listing on filings shows registrant-lobbyist contracts, not dated co-work. | The paper describes federal LDA lobbyists, and the routes cover congressional staff. The paper's point that filing data cannot date relationships applies directly to the filing-year hops. It says nothing about the dated payroll-overlap hops. Routes are heuristic introduction paths, not access probabilities (D-025). | moderate | VERIFIED |
| [[holyoke-2015-learnable-skills-unteachable]] | 2015 | qualifies | Relationships with lawmakers are built on mutual need and each side's ability to supply something, and are targeted, not personal friendships (p.17-18, introduced as 'arguably'). A lobbyist's market value is their portfolio of relationships (citing Blanes i Vidal et al. 2012). About 52% of roughly 30,000 DC lobbyists (2012, Rehr) came through government, and 48% of those held two or more posts (LaPira and Thomas 2014, as cited p.5). These figures are secondary and the authors did not re-derive them. | Conceptual only. The paper never measures whether shared tenure produces usable relationships. It implies that co-work measures the opportunity for contact, not the mutual need that keeps a relationship going. Its revolving-door share is a federal DC figure from about 2012-2014 and fits the router's staff-to-lobbyist population. It does not support any cost constant or the log-days form. Route cost must stay a heuristic, not an introduction probability (D-025). No influence wording on revolving-door edges. | weak | UNVERIFIED |
| [[duan-2025-breaking-sorting-barrier]] | 2025 | qualifies | The result is for real weights in the comparison-addition model. For integer weights in the word RAM model, the related-work line culminates in Thorup's O(m + n log log min{n, C}) for directed graphs [Tho04], where C is the maximum edge weight. | With integer weights the tighter asymptotic line is integer/word-RAM SSSP (Thorup), not this paper. Matters only if overlap-route latency becomes heap-bound, which has not been shown. | weak | VERIFIED |
| [[ahmad-nd-shortest-path-algorithms]] |  | context | Hop-count shortest paths ignore tie strength and asymmetry (Abstract). Tie strength matters for social search (Granovetter motivation, Sec. 1.1). The strongest path uses the product of normalized influences (Sec. 4). | Context only. The paper does not compare inverse-strength costs with log-product costs, and the overlap router is a deliberate design choice. Shared work days and shared background are heuristic tie scores, not measures of closeness, access or introduction probability (D-025). Influence language is barred on revolving-door edges. | weak | VERIFIED-SOFTENED |

**Assessment.** **Evidence weight: thin.** There are 4 rows: 2 VERIFIED, 1 VERIFIED-SOFTENED and 1 UNVERIFIED. Only one row speaks to tie semantics with any strength.

**What the VERIFIED rows establish**

- **Bacik et al. 2025 (supports, moderate).** LDA covered-position disclosures are undated, and co-listing on a filing shows a registrant-lobbyist contract, not dated co-work. This directly supports the existing design: filing-year episodes go into a weak 'co_reported_firm_association' hop that is excluded by default (include_weak = False, filing cost 5, which is at least the historical cost of 3). Dated Politico co-work stays the main tie source.
- **Duan 2025.** Overlap distances are integers, so the relevant asymptotic line is integer SSSP (Thorup), not Duan. This matters only if latency becomes heap-bound, which has not been shown.

**VERIFIED-SOFTENED: Ahmad (context).** The overlap costs are additive inverse strengths, ceil(1e6 x 128 / strength), not -log of a bounded weight. They cannot be read as a product of per-hop reliabilities, and no paper compares the two forms.

**UNVERIFIED: Holyoke 2015 (lead).** It argues that lobbying relationships are targeted and rest on mutual need. That implies co-work measures the opportunity for contact, not a usable relationship. Its revolving-door figures (about 52% of roughly 30,000 DC lobbyists came through government, around 2012) are secondary citations of Rehr and of LaPira & Thomas.

**Unsupported constants.** No row supports the log2(1 + days/30) form, the proximity weights, or the overlay costs (current co-worker 1, reported activity 2, historical 3, filing 5, policy 8).

**Contradictions.**
- Paul's position leans on Holyoke's highlights, but the Holyoke row is UNVERIFIED. Read as a lead, it implies that shared tenure measures the opportunity for contact rather than the mutual need that sustains a relationship, which cuts against using log-days as a proxy for relationship strength.
- Possible D-021 tension (FLAG, not a finding): when include_weak is set, the filing overlay creates route hops from LDA co-listing. D-021 makes LDA an identity source, not an edge or weight source. The hop is weak and off by default, but whether it should exist at all needs a ruling.
- There is a citation-year mismatch: the position cites 'Bacik et al. 2026', while the evidence row is Bacik et al. 2025 (arXiv 2503.11745).

**Gaps.**
- **Nothing shows that shared tenure length (log days) predicts a successful warm introduction.** This needs an outreach log with route features (overlap days, proximity, current versus historical) and outcomes, fitted by PathSF conditional logistic regression with Yen alternatives as choice sets and a single-look holdout. The results must be reported as heuristic tie scores, never closeness or introduction probabilities (D-025), and with no influence wording on revolving-door edges.
- **Nothing supports the overlay cost ladder (1/2/3/5/8) or the constant 128.** At minimum, a sensitivity analysis should show how route sets change under alternative constants.
- **Nothing compares inverse-strength costs with log-product costs** for social routing.

**Paul's position vs the evidence.** **Paul's view.** Paul treats professional and revolving-door ties as central to lobbying, and Holyoke's highlights stress that relationships are targeted and rest on mutual need.

**Partial support.** The evidence supports him only in part:

- Bacik 2025 (VERIFIED) supports keeping dated co-work primary and filing ties weak.
- Bombardini (VERIFIED-SOFTENED, closeness component) shows a revolving-door staffer's lobbying revenue depends on the patron's tenure. That is a revenue measure, not introduction success.

**Unsupported in its specifics.** Nothing validates shared tenure as a proxy for a usable tie. The only paper on relationship formation (Holyoke) is UNVERIFIED and, read as a lead, says co-work is opportunity, not relationship.

**Wording.** Revolving-door edges must carry no influence language.

**Open questions.**
- Should LDA filing co-listing be allowed to create any route hop, even a weak default-off one, given D-021?
- Is the log2(1 + days/30) form a deliberate choice, or a placeholder awaiting outcome data?
- Should a hop for the patron's current office or tenure (the Bombardini / Blanes i Vidal mechanism) also apply here, worded only as dependence on the patron's current office?

**Read first:** [[bacik-2025-measuring-dynamical-evolution]], [[holyoke-2015-learnable-skills-unteachable]], [[bombardini-2020-empirical-models-lobbying]], [[ahmad-nd-shortest-path-algorithms]], [[zeller-2016-step-path-selection]]

## Across components

## Patterns across components

**1. Citations ground the code in name only.** Seven or more citations ground code in papers that solve a different problem:

- **Chen 2016:** deadline reliability, not max-product.
- **Sotoudeh 2024:** bill passage, not recruitment.
- **Furnas 2019:** requester identity, not a cap on money.
- **Porter 2005:** committee co-membership.
- **Kim & Kunisky:** a lobbying bipartite graph.
- **MOPBD\*:** exact multi-objective paths.
- **RELINK:** a trained DDQN policy.

The fix is mostly wording: re-scope comments and docstrings, and use the classical -log p reliable-path and FEC-limit groundings. The code itself usually stands.

**2. No component has outcome evidence.** All ten empirical questions are GAPs. Two data sources would close most of them:

- **Dated cosponsor joins (public), plus any record of member-to-member asks.** These serve the three member-side components: ASKM, path-to-join timing, whip cap and community locality.
- **An outreach log of attempted and completed introductions with the seniority reached.** This serves the two staffer components. A PathSF conditional-logistic fit against Yen alternatives would fit the costs (Zeller, a lead).

Every study needs a named window and denominator, party strata or an UNSTRATIFIED label, and a single-look holdout.

**3. The algorithm evidence agrees across components.** Exact Dijkstra is correct and cheap at 533 members and with additive staffer costs. D* Lite, A*, Duan-2025 and Thorup-style speedups do not transfer at the current scale. Paul's interest in D* Lite, LPA*, A* and learned heuristics is not supported by any VERIFIED row for this system. The only strong speedup claim is UNVERIFIED and measured on voxel grids.

**4. Independence is assumed everywhere and tested nowhere.** chainHold, the Pareto reach and the Noisy-OR coverage all multiply independent per-edge terms. Chen 2016 names independence as its own main limitation. Asks on the same bill are plausibly positively correlated.

**5. Overstated certainty.** Phrases such as 'PROVABLY-OPTIMAL', 'mathematical certainty', (1-1/e) under a targeted call, and a 'conservative floor' each overstate what the code guarantees. Under D-025, every path score, chain hold and route cost is a heuristic model output.

**6. Project-rule flags**

- **D-021 (LDA is not an edge or weight source).** Two places touch it: the weak LDA filing hop in the overlap routes, and Paul's aim of turning lobbying and bipartite data into a routable graph.
- **Per-edge volume normalization (REJECTED in this project).** Two places risk reintroducing it: Ahmad's investment-normalized influence, and Ma 2022's multi-hop extension. Neither should be adopted.
- **Reciprocity (D-104..D-108).** No row here validates it, and nothing in the route logic should assume reciprocal asks.

**7. Code-reading defects need confirmation.** The direction mismatch and the dropped-set issue in the member chain rest on single-path code readings. They need a second-path reproduction before any claim is made (DE-062).

## Research agenda

1. Reproduce the member-chain defects by a second path (DE-062). Compare tracePath output with Dijkstra on -log of the transposed PE, with and without dropping a bridging member, and with non-unit EMU and role priors. Then decide whether distToTarget should honour the dropped set and the query direction.
2. Correct the grounding wording, with Paul's approval. In the template, drop 'PROVABLY-OPTIMAL' and 'mathematical certainty' and replace the Sotoudeh, Chen 2016 and Furnas citations with the classical reliable-path result and the FEC limit. Restate budgetRecruit's guarantee as (1-1/e) relative to the pruned pool when untargeted and 1-e^(-1/3) when targeted, for a coverage proxy. Change the cosponsor_route docstring to 'lambda-scalarised greedy, heuristic front'. Re-scope the Kim & Kunisky and Porter citations in the brain note.
3. Build a cosponsor join-sequence dataset from dated cosponsorships across several Congresses, with recorded Dear Colleague or member-to-member asks where available. From it, estimate the ask lift (ASKM), test hop independence, test whether path reliability predicts time-to-join, and test whether marginal recruits fall off per recruiter (K). Stratify by party, name the window and the bill and member denominators, and use a single-look holdout on a later Congress.
4. Test whether recruits join at higher rates within a community: detect mixed-membership communities on the cosponsor graph only (never LDA, per D-021) and compare join hazards, stratified by party.
5. Design an outreach log for staffer introductions, recording origin, route used, Yen alternatives, outcome and seniority reached (Kalla six-rank). Pre-register a PathSF conditional-logistic fit of the hop-cost components (HOP_FACTOR, w, staleness, log-days, overlay ladder), report outputs only as heuristic scores, and use a single-look holdout.
6. Settle which staffer closeness search is the live default (bidirectional, or one-way to a virtual sink) by reading the live file, and fix the 'labelled A*' wording if h = 0 remains.
7. Ask Paul for a ruling under D-021 on whether LDA filing co-listing may create any route hop, including the weak default-off 'co_reported_firm_association' hop.
8. Run a node-loss robustness simulation on the staffer graph: remove departing staff and measure degradation in the route set and costs. This would test Paul's centerless-design goal before any heuristic or replanning work.
9. Defer D* Lite, LPA*, A*, learned heuristics and Duan or Thorup SSSP until there is a live event stream that changes edges between queries, or a measured heap-bound latency problem. At current scale, re-evaluate only on evidence of either.
10. Verify the UNVERIFIED leads before citing them: Holyoke 2015 (relationships rest on mutual need), Zeller 2016 (PathSF design), Abdel-Rahman 2025 (D* Lite inside CBS) and Cao 2025 (multi-path trust aggregation).
