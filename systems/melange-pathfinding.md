---
type: system
name: "Melange path-finding (recruitment and introduction routes)"
slug: melange-pathfinding
components:
  - melange-pathfinding.member-recruitment-path
  - melange-pathfinding.multi-target-and-budget-modes
  - melange-pathfinding.cosponsor-route-pareto
  - melange-pathfinding.staffer-closeness-routes
  - melange-pathfinding.staffer-overlap-routes
brain_notes:
  - actors/melange-dashboard-june-capstone.md
  - actors/staffer-live-sidecar.md
paper_count: 29
tags: [system]
---

# Melange path-finding (recruitment and introduction routes)

Route search over two graphs: member-to-member recruitment chains on the cosponsor kernel (dashboard pathfinder modes and cosponsor_route.py), and staffer introduction routes on the people/office graph (closeness routes, ordered overlap routes and the employment-routing overlay). Route objectives, tie rules and coverage rules live here; the graphs themselves are built in melange-sna and melange-staffer-network.

Brain notes: [actors/melange-dashboard-june-capstone](obsidian://open?vault=brain&file=actors%2Fmelange-dashboard-june-capstone); [actors/staffer-live-sidecar](obsidian://open?vault=brain&file=actors%2Fstaffer-live-sidecar)

## Melange path-finding: evidence assessment

**Scope.** This covers 5 components and 55 evidence rows (29 distinct papers): 13 VERIFIED, 34 VERIFIED-SOFTENED and 8 UNVERIFIED. UNVERIFIED rows are single-agent leads, not findings (DE-062).

**What the evidence is.** No row gives an empirical estimate of anything the router assumes about Congress or congressional staff. That includes the direct-ask lift ASKM, per-hop ask success, hop independence, recruiter whip capacity, and whether route cost or shared tenure predicts a real introduction. The rows fall into three groups:
- **Algorithm papers** from roads, grids, UAVs, ships and online graphs (Chen 2016, Ren 2022, Luo 2020, Xu 2024, Duan 2025, GCOMB, RELINK, Golovin 2011, Ng 1999). These are good for checking whether a search is correct and whether its guarantees hold.
- **Lobbying and access studies** (Furnas 2019, Kalla & Broockman, Blanes i Vidal through Bombardini 2020 and de Figueiredo 2013, Holyoke 2015, Bacik 2025). These concern access, revenue or how disclosure works, not routes.
- **One congressional cosponsorship paper** (Sotoudeh 2024). Its outcome is whether a bill passes, not whether a member joins.

So every edge probability, cost constant and dial in this system remains an unestimated heuristic. Its outputs are heuristic scores, never closeness, access, introduction or ask-success probabilities (D-025).

**The main results are about correctness and citations, not empirical support.**
1. **The member chain can return a wrong path.** It is labelled PROVABLY-OPTIMAL, but it is not optimal once any member is dropped. A toy replica of buildPath gave a chain product of 0.009 where the best chain avoiding the dropped member is 0.36. In a second case it reported "unreachable" when a path existed. The replica reproduces the logic only; the failure in the live browser page is UNVERIFIED.
2. **The chain may also be traced against the edge direction.** This is a second, unconfirmed defect, from single-path code readings. The PE matrix is asymmetric, and the size of the effect is bounded by EMU 0.80-1.20.
3. **Several citations claim more than the papers show.** Furnas 2019 is cited for recruitCost. Sotoudeh 2024 is cited for "mathematical certainty". Chen 2016's deadline-reliability method is not the max-product object the chain uses. Kim & Kunisky and Porter 2005 are cited for cosponsor communities that the code does not use. The GCOMB label, the MOPBD* and RELINK docstrings, and the (1-1/e) claim on the target-weighted call also overstate.
4. **Paul's D* Lite and LPA* replanning layer is not supported at this scale.** Every replanning paper's own results say a full recompute is enough on a 533-node graph.
5. **One rule conflict found in the code.** The overlay's filing hops are built from LDA source filings. That makes LDA an edge source, which D-021 forbids. The hops are off by default and review-only, but this needs Paul's ruling.

**What the rows do support.**
- The staffer router uses additive costs, heuristic 0 and exact Dijkstra, and the papers back that as correct.
- The overlay treats undated filing years as weak, opt-in ties, which Bacik 2025 (VERIFIED) supports.
- The untargeted budget greedy (the only live call) carries the classical (1-1/e) guarantee on its coverage proxy, but not on real recruitment.

## Sponsor-to-target max-product path and chain hold

**What it computes.** The most reliable recruitment chain from a sponsor to a target, with per-hop $ and probability; chainHold = product of activeP per hop.

**Method.** Dijkstra on -log p. activeP(p) = p/(p + (1-p) x ASKM) with ASKM 0.07 (about a 14x odds shift, unestimated). recruitCost(p) = 1000 + 9000(1-p), clamped to $1k-$10k.

**Code refs:** `fec/templates/make_dashboard_html_00.tpl (buildPath, chainHold, recruitCost)`

**Assumptions**
- Cosponsor-kernel edge probabilities proxy ask success
- Active asks raise odds by a constant factor (no study pins it)
- Money is capped by FEC limits; difficulty shows up in reliability, not dollars

**Evidence** (15 rows, 13 distinct papers; UNVERIFIED 3, VERIFIED 4, VERIFIED-SOFTENED 8)

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

**Assessment**

**Evidence weight.** 15 rows: 4 VERIFIED, 8 VERIFIED-SOFTENED, 3 UNVERIFIED (Cao 2025, Zeller 2016, Abdel-Rahman 2025). None of them tests the assumptions empirically. The only congressional paper is Sotoudeh 2024 (VERIFIED, strong), and it does not transfer: its outcome is bill passage, and its score is not a probability.

**Correctness.** The main result is a defect.
- Lawande 2022, Xu 2024, Luo 2020 and Ng 1999 agree on the mechanism. distToTarget ignores the dropped set while tracePath skips dropped members, so the greedy trace reads a field that still routes through excluded members.
- The Ng 1999 row reproduced this in a replica of the functions: a chain product of 0.009 against a best drop-avoiding chain of 0.36, and reached=false while a path existed.
- The defect is real at the logic level. The browser-level failure stays UNVERIFIED. It is live only when the drop text box (applyShocks, line 1165) is used.
- The second defect, a direction mismatch, rests on single-path readings (Ahmad, Chen 2016 extension row). distToTarget relaxes PE[u][v] outward from the target, but the trace needs the reverse direction, and PE is asymmetric through EMU[v] and the role priors. The EMU range [0.80, 1.20] bounds the effect, and its size is unmeasured.
- Duan 2025 (VERIFIED) is relevant only as the reminder that directed SSSP must relax edges in the query direction.
- Chen 2016's validation design (an exact solver checked against an independent exact solver) transfers directly as a software check.

**Citations.**
- Chen 2016 solves deadline reliability, a non-additive objective, with label-setting. The chain's product of independent p is the classical -log p shortest path. The cited paper supports the method in name only.
- Furnas 2019 never discusses dollar amounts. The $1k-$10k clamp rests on FEC limits, which are law.
- Sotoudeh 2024 is cited for 'mathematical certainty' about chain probability, which also breaks the D-025 wording.

**chainHold semantics.** Chen 2016 (VERIFIED-SOFTENED) flags independence as its own main limitation. The asks in one chain share a bill, its salience, the party context and the ASKM lever, so they are plausibly positively correlated. If so, the product is not the 'conservative floor' the comment claims.

There is also a scale mismatch. Paths are ranked on passive reliability, but chainHold is shown on the active-ask scale.

**Extensions (leads only).**
- RWR over multiple walks (Baptista, weak).
- Multi-path trust aggregation (Cao, UNVERIFIED).
- Path-level versus step-level selection (Zeller, UNVERIFIED). This motivates testing hop independence against observed join sequences.

**Gap.** Nothing estimates ASKM 0.07 (about a 14x odds shift). Nothing tests whether path length predicts how fast a member joins. Filling this needs an observational House study, within one Congress: link targeted asks (Dear Colleague letters or sponsor outreach records) to cosponsor join timing in an event-history model. Report it by majority/minority and co-/out-party strata, or label it UNSTRATIFIED.

**Contradictions**
- The code comment labels the chain 'PROVABLY-OPTIMAL'. A logic replica shows it is not optimal when the dropped set is non-empty (0.009 vs 0.36; reached=false while a path exists). The browser-level failure is UNVERIFIED (DE-062).
- The rows disagree about the no-drop case. The Ng 1999 row says the greedy step is exact when nothing is dropped. The Ahmad and Chen 2016 rows say the field is computed against the edge direction when PE is asymmetric. Both readings are single-path, so the question is unresolved.
- Chen 2016 is cited as grounding, but it solves a non-additive deadline-reliability problem that Dijkstra cannot solve. The chain is a -log p additive path.
- Furnas 2019 is cited for 'money is capped, info is the lever' in recruitCost. The paper tests requester identity, never dollar amounts.
- Sotoudeh 2024 is cited for a 'mathematical certainty' chain probability. The paper's outcome is bill passage and its score is not a probability. The wording conflicts with D-025.
- chainHold is called a 'conservative floor', but it assumes independent asks. Under positive correlation the product is not a floor (Chen 2016's own stated limitation).
- Ranking and display use different scales: paths are ranked by passive pathReliability, while the displayed chainHold uses active-ask activeP.

**Gaps**
- No estimate of the direct-ask lift ASKM. Needs an observational study linking documented member-to-member asks (Dear Colleague letters, sponsor outreach) to cosponsor join timing, House, within one Congress, stratified by party and majority status.
- No test of whether most-reliable path length or chainHold predicts whether or how fast a target joins. Needs a prospective holdout on bills with observed cosponsor accrual.
- No test of hop independence. Needs to check whether joins along observed recruitment sequences are correlated beyond what the product rule implies.
- No second-path reproduction of the direction-asymmetry defect. Needs a test that compares Dijkstra on -log of the transposed PE with the traced chain under non-unit EMU and role priors.

**Paul's position vs the evidence**

Paul's central gap is turning lobbying and bipartite data into a graph that path-finding can run on. No row addresses it directly. D-021 also limits the route: LDA can serve as an identity source, not an edge or weight source. Kim & Kunisky's communities live on exactly that organisation-by-member lobbying graph, so building path edges from it would conflict with D-021.

Paul wants A* heuristics that cope with unstable information. The evidence gives no reason to add one to a 533-node exact search, where a full recompute is cheap (Luo 2020, Xu 2024). Ng 1999 shows that only a potential-based reweighting keeps the optimum. A learned or inadmissible heuristic would break the 'provably optimal' contract that the code already fails to meet under drops.

The evidence points first to fixing the existing exact search: honour the dropped set and the edge direction inside distToTarget. Learned heuristics come after that.

**Open questions**
- Under the default bundle, is PE actually asymmetric, through emult or non-unit role priors? If not, the direction concern is moot in practice.
- How often is the dropped set non-empty in real use?
- Should chainHold be reported on the same passive scale used for ranking, or should ranking switch to the active scale?
- Is there any source of attributed member-to-member asks that could estimate ASKM without breaking D-021?

**Read first:** [[ng-1999-policy-invariance-reward-transformations]], [[chen-2016-most-reliable-path]], [[ahmad-nd-shortest-path-algorithms]], [[sotoudeh-2024-network-based-measure]], [[furnas-2019-moneyed-interests-information]], [[xu-2024-improved-lite-based]]

## Best-sponsor, multi-target tree and budget max-coverage modes

**What it computes.** Recruitment trees and budget-bounded recruit sets with coverage cov[v] = 1 - prod(1 - p).

**Method.** Greedy max coverage (1-1/e) with locality and degree pruning (GCOMB), plus grow() spread with recruitability x log(deg) x proximity.

**Code refs:** `fec/templates/make_dashboard_html_00.tpl (budgetRecruit, multiTarget, grow)`

**Assumptions**
- Approximate submodularity on the cosponsor graph
- Community structure is exploitable (Porter-Mucha-Newman; Kim-Kunisky)

**Evidence** (10 rows, 10 distinct papers; VERIFIED 2, VERIFIED-SOFTENED 8)

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

**Assessment**

**Evidence weight.** 10 rows: 2 VERIFIED (Golovin 2011, Chen 2023), 8 VERIFIED-SOFTENED, none UNVERIFIED. All of it is theory or method. None of it concerns Congress.

**What holds.** budgetRecruit maximises Noisy-OR coverage 1 - prod(1-p), which is monotone submodular. Plain greedy therefore carries (1-1/e) on that coverage proxy (Golovin 2011, VERIFIED). The one live call passes target -1, so the live path is the classical case (Chen 2023, VERIFIED).

GCOMB Table 1b shows greedy reaching at least 99% of optimal, but only on 500-900 node synthetic graphs. Applying that to the 533-member graph is an extrapolation. Kamarthi 2020 adds that greedy is the reference standard only when the graph is known. The guarantee holds for the observed cosponsor graph, not for any latent network.

**What is overstated.**
- With a target set, marginals are multiplied by (1 + 2 dT). That is not greedy on any fixed submodular function, so (1-1/e) should not be claimed for targeted calls (Golovin, Ma 2022, Chen 2023, Dai 2017).
- The comment says 'cost-bounded', but the loop is cardinality-bounded (nSeeds).
- The objective is one-hop (Trivedi 2020, VERIFIED-SOFTENED). Trivedi's claim that the kernel reaches p of about 0.52 traces to a note not found in the repo, so it is unchecked.
- The 'GCOMB' label overstates the mapping. GCOMB's Eq. 3 locality is a Q-learning state feature, and the code's degree prune is a fixed rule, not GCOMB's learned noise predictor.
- (1-1/e) applies to the coverage proxy, not to real recruitment. If joining follows a threshold (complex contagion), no guarantee holds (Chen 2023).

**Community grounding does not transfer.**
- Kim & Kunisky 2020 find communities on the bipartite interest-group-by-politician lobbying graph (k = 8, 113th Congress), not on the member-member cosponsor graph.
- Porter 2005 studies committee co-membership.
- Neither citation appears in the template, and the code has no community term. Louvain exists only in press_hcoi.py.
- If a community term is ever added, Kim & Kunisky argue for mixed membership over a hard partition, because hub actors are the ones a partition misassigns. Building it from LDA lobbying edges would conflict with D-021.

**Flag.** Ma 2022's multi-hop extension, which uses L-step walk weights, must not reintroduce per-edge volume normalization. That proposal was measured and REJECTED in this project.

**Gap.** Nothing tests whether members recruited through community-local paths join at higher rates than cross-community targets. Filling this needs a community partition built on the cosponsor graph itself, plus observed cosponsor accrual per bill. Join rates would be compared within one Congress and stratified by party.

**Contradictions**
- Paul's addendum and the brain note (melange-dashboard-june-capstone.md line 83) say the pathfinder exploits community structure (Porter-Mucha-Newman; Kim-Kunisky). The code has no community term, and both papers study different graphs: committee co-membership, and organisation-by-politician lobbying.
- The budgetRecruit comment claims a (1-1/e) guarantee and 'cost-bounded' selection. With a target set, the (1 + 2 dT) reweighting voids the guarantee, and the loop is bounded by cardinality, not cost.
- The 'GCOMB' label implies GCOMB's learned locality and noise pruning. The code uses the plain coverage marginal and a fixed degree prune.

**Gaps**
- No study of community-local versus cross-community recruitment success. Needs a cosponsor-graph partition (mixed membership preferred) and per-bill join outcomes, within one Congress, stratified by party.
- No measure of how one-hop coverage relates to realised multi-hop recruitment on the kernel's actual p range. Needs simulated cascades compared with observed join sets.
- No evidence on whether joining is simple or complex contagion. That decides whether any (1-1/e) language applies to real recruitment.

**Paul's position vs the evidence**

The hub says GCOMB pruning and community structure match Paul's addendum. The evidence agrees only on paper. GCOMB is a fair loose source for greedy-with-pruning, but the label overstates the mapping. The community claim is not implemented, and the two cited papers do not show community structure on the cosponsor graph. The position is sound as a design intent. As a description of the built system it is inaccurate, and its grounding should be re-scoped.

**Open questions**
- Should the brain note's Kim-Kunisky and Porter community citations be withdrawn, or should a cosponsor-graph community term be built and tested?
- Should the targeted budgetRecruit call keep the (1 + 2 dT) weighting and drop the (1-1/e) wording, or switch to a constrained submodular objective that keeps a guarantee?
- Where does the p of about 0.52 kernel figure come from? The cited sector-networks note is not in the repo.

**Read first:** [[golovin-2011-adaptive-submodularity-theory]], [[manchanda-2020-gcomb-learning-budget-constrained]], [[kim-2020-mapping-political-communities]], [[chen-2023-complex-contagion-influence]], [[kamarthi-2020-influence-maximization-unknown]], [[porter-2005-network-analysis-committees]]

## Cosponsor recruitment route finder (Pareto front)

**What it computes.** Greedy routes from champion seeds with at most K = 5 recruits per recruiter, giving a Pareto front over cost, expected cosponsors and risk.

**Method.** Edge cost = CEDGE/max(0.1, p); value = (gain - lambda(1-p))/cost for lambda in {0, 0.5, 1, 2, 4}. Optional MC reach. MOPBD* (Ren et al.) is cited as concept only.

**Code refs:** `fec/engine/cosponsor_route.py:1-89`

**Assumptions**
- Recruiter whip capacity is bounded
- Edge activations are independent

**Evidence** (9 rows, 8 distinct papers; UNVERIFIED 1, VERIFIED 2, VERIFIED-SOFTENED 6)

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

**Assessment**

**Evidence weight.** 9 rows: 2 VERIFIED (Lawande 2022 context, Chen 2016 extension), 6 VERIFIED-SOFTENED, 1 UNVERIFIED (Abdel-Rahman 2025). All of it is algorithmic. Nothing bears on recruiter capacity or edge independence in Congress.

**Method versus citation.**
- The docstring credits 'RELINK + MOPBD*'. Ren 2022 (VERIFIED-SOFTENED, strong) proves that MOPBD* returns the exact Pareto set of source-to-goal paths with additive vector costs.
- cosponsor_route.py instead runs a greedy recruitment-set builder five times (lambda in {0, 0.5, 1, 2, 4}) and keeps the non-dominated (cost, spread, risk) points. That is a heuristic approximate front over recruitment sets, not a path search. Product-form reach and mean-p risk do not fit MOPBD*.
- Scalarised runs cannot reach points in non-convex regions of the front (Chen 2016, Xu 2024).
- RELINK (Arya 2025) contributes only the framing: edge actions under a per-node budget. Its results are a trained DDQN beating baselines in 456-464 of 480 configurations, and 'up to 15%' more spread, on simulated IC over public online graphs. Neither figure may be cited for Melange routes. The paper's abstract and Sec. 6.3 also state the 15% figure differently.

**Replanning.**
- Ren 2022 itself shows MOPBD*'s edge over from-scratch NAMOA* shrinking as more obstacles change, with from-scratch winning at M = 4. Its worst case can exceed from-scratch search. This was on grid mazes with a 5-minute limit.
- Luo 2020 says plain D* Lite suits small, local changes, and its speed comes from coarsening million-cell grids.
- Abdel-Rahman (UNVERIFIED) reports a 99.2% time cut on 3D voxel grids, with run counts not reported.
- None of these transfers to a 533-node graph where each run already rebuilds cheaply.
- The idea that does carry over is repairing a route when a member joins, declines or changes stance. A plain recompute triggered by that observed event would do it.

**Gap.** K = 5 recruits per recruiter and CEDGE = 10000 are unestimated environment defaults. Edge independence is assumed. No row tests either.

**Contradictions**
- The docstring names 'RELINK + MOPBD*'. The code is a greedy builder swept over lambda with a dominance filter, which gives a heuristic, approximate front, not MOPBD*'s exact Pareto set (Ren 2022, Theorem 1).
- Paul made D* Lite and LPA* the operational layer and called D* 'Very interesting'. Every replanning paper in the evidence (Ren 2022, Luo 2020, Xu 2024) indicates that incremental reuse does not pay at this graph size or under whole-network batch refresh.

**Gaps**
- Whip capacity is untested: do prolific recruiters' marginal recruits drop off? Needs recruiter attribution, such as Dear Colleague signatories or sponsor outreach records, plus cosponsor join data, within one Congress. Raw cosponsor lists do not identify who recruited whom.
- Edge-activation independence is untested. Needs evidence on whether joins cluster beyond what independent activation predicts.
- No exact or epsilon-dominance front exists to measure how much of the true front the five-lambda sweep misses. This is a software benchmark, not an empirical study.

**Paul's position vs the evidence**

Paul's interest in D* dynamic replanning is sound as a concept for a live campaign, where asks fail and stances move. The evidence does not support building incremental D* Lite or LPA* here. On a 533-node graph a recompute triggered by an observed event is cheaper and stays exact, and MOPBD*'s own results lose to from-scratch search once enough changes pile up. The multi-objective idea is the right one to keep. If Paul wants the front to be exact rather than illustrative, the next step is an exact or epsilon-dominance front over the recruitment-set objective, not incremental replanning.

**Open questions**
- Is the Pareto front meant to be illustrative or exact? That decides whether a dominance-based exact method is worth building.
- Is there any observable event stream (stance changes, declined asks) that would make reroute-on-failure meaningful?
- Should the RELINK citation be dropped from the docstring, given that no RELINK result applies?

**Read first:** [[ren-2022-multi-objective-path]], [[arya-2025-relink-edge-activation]], [[chen-2016-most-reliable-path]], [[luo-2020-path-planning-algorithm]], [[abdelrahman-2025-conflict-based-search]]

## Staffer closeness route search

**What it computes.** The least-cost introduction route to staff anchored at a member office. Hop cost = -ln(0.8 x strength) + w x uncertainty; terminal cost includes a member-context modifier (beta 0.15).

**Method.** Bidirectional Dijkstra with heuristic 0 (labelled A*) and Yen loopless alternatives. The default route search is one-directional to a virtual sink (staffer_closeness_routes.py:110-138). Ties within 1e-12 relative: fewer steps wins, then lower cost. The office search excludes uncovered staffers and reports the count.

**Code refs:** `fec/surfaces/staffer_closeness_routes.py:1-30,78-138,911-999`, `fec/surfaces/staffer_routing.py:381`

**Assumptions**
- The minimum-cost path approximates the most usable introduction
- The prestige modifier is a bonus, not access evidence

**Evidence** (16 rows, 13 distinct papers; UNVERIFIED 3, VERIFIED 2, VERIFIED-SOFTENED 11)

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

**Assessment**

**Evidence weight.** 16 rows: 2 VERIFIED (Chen 2016, Xu 2024), 11 VERIFIED-SOFTENED, 3 UNVERIFIED (both Zeller rows, Abdel-Rahman). No row shows that a cheaper route corresponds to a successful introduction.

**The search design holds up.**
- Chen 2016 (VERIFIED) is a boundary condition, not a criticism. Because the uncertainty penalty is a per-hop sum, bidirectional Dijkstra stays exact. If uncertainty were restated as a route-level SD, or as a probability of introduction by a date, Dijkstra would stop being exact.
- Lawande 2022 and Luo 2020: a person-tie graph has no coordinates, so geometric A* heuristics do not apply. ALT landmarks would be the only admissible candidate, and they are deferred.
- Xu 2024 and Abdel-Rahman: incremental replanning pays only when the same origin and goal are re-queried after small edge changes. Staffer requests vary in origin and run on a fixed snapshot.
- Duan 2025 does not reach the measured bottleneck, which is memory (pair_cache 7.0 to 17.5 GB), not heap cost.
- Ahmad (nd) supports only the log-inverse-product part of the hop cost. The per-hop ln(1/0.8) decay is the system's own design, and Ahmad never shows that strongest paths succeed more often.

**Ranking terms.** Ng 1999 (VERIFIED-SOFTENED) separates the terms:
- Dividing by (1 + MEMBER_BETA) is a constant terminal shift and keeps the ranking.
- The per-staffer member-context modifier, the per-target anchor uncertainty term (line 919) and the balance step penalty in staffer_routing.py:424-425 are not potentials, and each can change the winner.

That is acceptable if it is intended, since the prestige modifier is meant to act as a bonus. It does mean the documented rule (ties broken by fewer steps, then cost) is not the only ranking rule in force. Duan 2025's lexicographic tie order is analogous but does not validate the 1e-12 band or the step-first rule (decision D-082, Paul's).

**Substantive bearings.**
- Furnas 2019: route outputs are routes to a contact point only. A meeting barely varies with requester identity and predicts little about action (DC staff, 2017, stated responses).
- Kalla & Broockman: who asks changes which tier of staff takes the meeting. The donor label raised meetings with chief of staff or above from 2.4% to 12.5% (p = .006) but not district-staff meetings (43.3% vs 48.4%, p = .26), in the paper's field sample of meeting requests. The sample size is not recorded in the row. This supports recording the seniority of the person reached as a validation target.
- Bombardini 2020 reports Blanes i Vidal: former Senate staffers turned lobbyists lose about 24% of lobbying revenue when their connected senator leaves office. The effect is significant only for Finance, Appropriations and Ways & Means connections. de Figueiredo 2013 reports the same study at about 23%, so this is one study, not two.
- Neither router file reads whether the anchoring member is still serving. That was checked in two files only, not repo-wide (UNVERIFIED system-wide).
- If a patron-in-office term is added, it may be described only as dependence on the patron's current office, never as access or influence.

**Description drift (checked in the live file).** The component text says the default search is one-directional to a virtual sink (staffer_closeness_routes.py:110-138). The live module docstring and ALGORITHM constant (line 79) say bidirectional Dijkstra, heuristic 0. The rows describe _search_reference as a one-way test oracle only, and lines 110-138 hold the _Meter class.

**Gap.** The open question (do shorter or cheaper routes match successful introductions?) has no evidence. Zeller 2016 (UNVERIFIED) supplies a design: record each attempted introduction, pair the route used with its Yen alternatives as the choice set, and fit hop-cost weights by conditional logit (path selection function), with a single-look holdout. The fitted scores would remain heuristic, never introduction probabilities (D-025).

**Contradictions**
- The component description says the default route search is one-directional to a virtual sink (staffer_closeness_routes.py:110-138). The live module docstring and the ALGORITHM constant (line 79) say bidirectional Dijkstra with heuristic 0. The rows describe the one-way search as a test oracle only. The memory note on the latency lane ('one-way Dijkstra') conflicts as well.
- The documented tie rule is fewest steps, then least cost, within 1e-12. Ng 1999 shows that non-potential terms (the per-staffer member-context modifier, the anchor uncertainty term at line 919, and the balance step penalty at staffer_routing.py:424-425) can also change the winner. So the documented rule is not the only ranking rule in force.
- The Ng 1999 row says the premise that Paul wants learned heuristics is not evidenced in the item (paul_entries empty). The hub's position text asserts it.

**Gaps**
- No outreach log links route cost or rank to completed introductions. Needs recorded attempts and outcomes, with the seniority tier reached (Kalla's coding), and a path-selection fit against Yen alternatives on a single-look holdout.
- No test of the probability-to-cost transform (-ln(0.8 x strength)). Zeller warns that route choice is sensitive to it.
- No test of whether the anchoring member still holding office should enter the cost. Needs a repo-wide check first, then a study using member exits as events.
- No evidence on robustness to node loss (Paul's centerless goal). Needs a simulation of route availability and cost inflation when top-k hub staffers or offices are removed.

**Paul's position vs the evidence**

Paul wants a centerless design that stays intact when a node is lost. No row tests node-loss robustness on the staffer graph, so this is an open gap. It could be measured directly by removal simulations on the existing graph.

Paul also wants learned heuristics. The evidence cuts against them for this router. There is no admissible geometric heuristic. ALT landmarks are the only exact-preserving option, and they are deferred. Ng 1999 shows that only potential-form terms leave the optimum unchanged, and a learned inadmissible heuristic would break the router's 'route proven or shown absent' contract. The memory bottleneck, not search effort, is the measured latency problem. Heuristic 0 with exact search is the better-supported choice today.

**Open questions**
- Is the balance step penalty in staffer_routing.py:424-425 meant to override the documented tie rule? If so, should the docstring say so?
- Does any file outside the two checked ones read whether the anchoring member is still serving?
- Is an outreach log being recorded, or could one be, so that a route-validation study becomes possible?
- Should the component description be corrected to say bidirectional Dijkstra as the default?

**Read first:** [[furnas-2019-moneyed-interests-information]], [[kalla-nd-campaign-contributions-facilitate]], [[ng-1999-policy-invariance-reward-transformations]], [[zeller-2016-step-path-selection]], [[bombardini-2020-empirical-models-lobbying]], [[chen-2016-most-reliable-path]]

## Ordered person-overlap routes and employment-routing overlay

**What it computes.** Routes through dated Politico-reconciled co-work. distance = ceil(1e6 x 128 / strength), with strength = proximity x log2(1 + days/30). Overlay costs: current co-worker 1, historical 3, reported activity 2, filing 5, policy 8.

**Method.** Bidirectional Dijkstra on (cost, steps). User-asserted current co-worker hop at cost 1.

**Code refs:** `fec/surfaces/staffer_overlap_routes.py:1-280`, `fec/surfaces/staffer_routing.py:153-640`, `fec/surfaces/staffer_routing_evidence.py:157-162`

**Assumptions**
- Log-scaled shared work days proxy relationship strength
- Only Politico career-history evidence creates work ties

**Evidence** (5 rows, 5 distinct papers; UNVERIFIED 1, VERIFIED 3, VERIFIED-SOFTENED 1)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | supports | Covered-position disclosures are made once and have no dates, so recency cannot be established. Co-listing on filings shows registrant-lobbyist contracts, not dated co-work. | The paper describes federal LDA lobbyists, and the routes cover congressional staff. The paper's point that filing data cannot date relationships applies directly to the filing-year hops. It says nothing about the dated payroll-overlap hops. Routes are heuristic introduction paths, not access probabilities (D-025). | moderate | VERIFIED |
| [[holyoke-2015-learnable-skills-unteachable]] | 2015 | qualifies | Relationships with lawmakers are built on mutual need and each side's ability to supply something, and are targeted, not personal friendships (p.17-18, introduced as 'arguably'). A lobbyist's market value is their portfolio of relationships (citing Blanes i Vidal et al. 2012). About 52% of roughly 30,000 DC lobbyists (2012, Rehr) came through government, and 48% of those held two or more posts (LaPira and Thomas 2014, as cited p.5). These figures are secondary and the authors did not re-derive them. | Conceptual only. The paper never measures whether shared tenure produces usable relationships. It implies that co-work measures the opportunity for contact, not the mutual need that keeps a relationship going. Its revolving-door share is a federal DC figure from about 2012-2014 and fits the router's staff-to-lobbyist population. It does not support any cost constant or the log-days form. Route cost must stay a heuristic, not an introduction probability (D-025). No influence wording on revolving-door edges. | weak | UNVERIFIED |
| [[duan-2025-breaking-sorting-barrier]] | 2025 | qualifies | The result is for real weights in the comparison-addition model. For integer weights in the word RAM model, the related-work line culminates in Thorup's O(m + n log log min{n, C}) for directed graphs [Tho04], where C is the maximum edge weight. | With integer weights the tighter asymptotic line is integer/word-RAM SSSP (Thorup), not this paper. Matters only if overlap-route latency becomes heap-bound, which has not been shown. | weak | VERIFIED |
| [[ahmad-nd-shortest-path-algorithms]] |  | context | Hop-count shortest paths ignore tie strength and asymmetry (Abstract). Tie strength matters for social search (Granovetter motivation, Sec. 1.1). The strongest path uses the product of normalized influences (Sec. 4). | Context only. The paper does not compare inverse-strength costs with log-product costs, and the overlap router is a deliberate design choice. Shared work days and shared background are heuristic tie scores, not measures of closeness, access or introduction probability (D-025). Influence language is barred on revolving-door edges. | weak | VERIFIED-SOFTENED |
| [[defigueiredo-2013-advancing-empirical-research]] | 2013 | qualifies | Blanes i Vidal, Draca and Fons-Rosen (2012): lobbyist revenue of former Senate staffers falls about 23 percent when the senator they served is defeated or retires, in a 22-period panel with lobbyist and party-chamber-time fixed effects (pp.18-20). The essay reads this as the value of who you know (p.20). | The essay's evidence is a revenue effect for lobbyists, not a route-cost or access measure. Influence language stays barred on revolving-door edges. | weak | VERIFIED |

**Assessment**

**Evidence weight.** 5 rows: 3 VERIFIED (Bacik 2025, Duan 2025, de Figueiredo 2013), 1 VERIFIED-SOFTENED (Ahmad), 1 UNVERIFIED (Holyoke 2015). This is the thinnest component, and nothing tests its central assumption.

**What is supported.** Bacik 2025 (VERIFIED, supports) shows that covered-position disclosures have no dates and that co-listing on filings records registrant-lobbyist contracts, not dated co-work. That directly supports routing filing-year episodes as weak and opt-in: include_weak defaults to False, filing_cost is 5 and at least historical_cost (3), and the episodes are labelled 'filing_year_association_only'. Bacik says nothing about the dated payroll-overlap hops.

**Rule conflict (checked in live code).** staffer_routing_evidence.py builds those filing episodes from LDA source filings: lda_source_person_id, confidence 'SOURCE_NATIVE_FILING_ASSOCIATION', review_only True. staffer_routing.py:346-367 then turns shared employer-year filings into route hops at filing_cost. That makes LDA an edge source, which D-021 forbids, even though it is off by default and marked review-only. The ruling is Paul's.

**What is not supported.**
- Holyoke 2015 (UNVERIFIED, conceptual) says lawmaker relationships are targeted and rest on mutual need, not personal friendship. On that reading, co-work measures opportunity for contact, not the need that keeps a relationship going. That undercuts treating log-scaled shared days as relationship strength.
- Holyoke's revolving-door share (about 52% of roughly 30,000 DC lobbyists came through government, 2012, from secondary sources) is population context only.
- Ahmad (nd): the inverse-strength integer costs are additive, so they cannot be read as a product of per-hop reliabilities.
- Duan 2025: integer weights make Thorup-style integer SSSP the relevant asymptotic line, which matters only if overlap routing becomes heap-bound. It has not been shown to be.
- de Figueiredo 2013 and Bombardini (through Blanes i Vidal, one study) measure lobbyist revenue that depends on the patron's office. That is not route cost, closeness or access. Influence language stays barred on revolving-door edges.

**Gap.** No row supports the log2(1 + days/30) form, the 128 scale, the overlay costs (current co-worker 1, reported activity 2, historical 3, filing 5, policy 8), or the assumption that only Politico career history creates work ties.
- Coverage could be measured: compare Politico-reconciled co-work against an independent payroll source for one window and report recall by chamber.
- Validity needs the same outreach-log design as the closeness routes, testing whether shared-tenure length predicts warm introductions, with a single-look holdout.

**Contradictions**
- The overlay's filing hops come from LDA source filings (staffer_routing_evidence.py lda_source_person_id; staffer_routing.py:346-367 filing_cost). That uses LDA as an edge source, contrary to D-021. Default-off and review-only mitigate it but do not resolve it.
- Holyoke 2015 (UNVERIFIED) says relationships rest on mutual need and are targeted, which cuts against the assumption that log-scaled shared work days proxy relationship strength.
- The hub's citation list gives 'Bacik et al. 2026'. The evidence slug and the arXiv id (2503.11745) date the paper 2025.

**Gaps**
- No test of whether shared tenure length (log days) predicts successful warm introductions. Needs an outreach log with attempted and completed introductions, the route used and its alternatives, and a single-look holdout.
- No coverage audit of 'Politico career history as the only work-tie source'. Needs a comparison with an independent staff payroll or disbursement source over a named window, reporting recall of co-work ties.
- No basis for the overlay cost constants (1/2/3/5/8) or the 128 scale. Needs a path-selection fit or a sensitivity study of how route rankings move under plausible constant ranges.

**Paul's position vs the evidence**

Paul treats professional and revolving-door ties as central to lobbying. The VERIFIED evidence (Blanes i Vidal, through two reviews) supports one narrower point: former staff turned lobbyists earn revenue that depends on their patron's current office. It does not show that co-work ties carry access or influence, and wording rules bar influence language on these edges.

The Holyoke reading Paul highlights (relationships are targeted and rest on mutual need) is UNVERIFIED. If it holds, it argues that shared tenure measures opportunity, not relationship strength. Paul's position and the router's log-days proxy pull in different directions.

Bacik 2025 supports the conservative handling of undated filing ties, but the source of those ties (LDA) needs a D-021 ruling.

**Open questions**
- Paul's ruling: should the LDA-derived filing-year hops be removed, kept as review-only, or exempted from D-021?
- What evidence produces the 'policy 8' overlay hop, and is it subject to the same LDA rule?
- Is there an independent payroll source for a coverage audit of the Politico co-work ties?

**Read first:** [[bacik-2025-measuring-dynamical-evolution]], [[holyoke-2015-learnable-skills-unteachable]], [[defigueiredo-2013-advancing-empirical-research]], [[ahmad-nd-shortest-path-algorithms]], [[bombardini-2020-empirical-models-lobbying]]

## Across components

**1. No empirical anchor anywhere.** All five components rest on hand-set constants: ASKM 0.07, recruitCost, K = 5, CEDGE, HOP_FACTOR 0.8, MEMBER_BETA 0.15, the overlay costs 1/2/3/5/8, and log2(1 + days/30). None has an estimate in the evidence base. One outreach log (attempted and completed introductions and asks, the route used, its alternatives, and the seniority reached) would supply validation data for the closeness routes, the overlap routes and, through member asks, ASKM. Until that exists, every route score is a heuristic model output, never a closeness, access, introduction or ask-success probability (D-025).

**2. The replanning question gets the same answer in every component.** Ren 2022, Luo 2020, Xu 2024 and Abdel-Rahman 2025 all show that incremental D* Lite / MOPBD* pays only when the same query is repeated after small local changes on large graphs. The member graph has 533 nodes and the staffer router is stateless on snapshots. What carries over is a recompute triggered by an observed event, not an incremental algorithm. That also fixes the one real correctness defect: the member chain's dropped set.

**3. Node-loss handling is where the system currently fails, and where Paul's goal sits.** Paul wants routing that stays intact when a node is lost. In the member chain, the member-out scenario is exactly where the replica shows PROVABLY-OPTIMAL failing. For staffers, no robustness test exists. One work item covers both: honour removed nodes in the field computation, and measure route survival under hub removal.

**4. Citations drift between the brain note, the code comments and the papers.** Chen 2016, Furnas 2019, Sotoudeh 2024, Kim & Kunisky, Porter 2005, GCOMB, MOPBD* and RELINK are each cited for more than the paper shows. Several mismatches sit in the brain vault rather than the code (Kim-Kunisky, Porter-Mucha-Newman). Re-scoping them is a documentation change for Paul to apply.

**5. Some sources appear twice.** Blanes i Vidal et al. 2012 reaches the hub twice, through Bombardini 2020 (about 24%) and de Figueiredo 2013 (about 23%). It is one study and should be counted once.

**6. Project-rule flags.**
- **D-021.** The overlap overlay builds route hops from LDA filing co-listings. Separately, any future community term built from Kim & Kunisky's lobbying graph would make LDA a weight source.
- **Rejected SNA proposals.** No paper here argues for dropping regular cosponsorship edges. Ma 2022's multi-hop reach extension must not reintroduce per-edge volume normalization, which was measured and rejected.
- **Reciprocity (D-104..D-108).** No row bears on it.
- **UNVERIFIED rows.** All 8 are leads only: Cao 2025, Zeller 2016 x3, Abdel-Rahman 2025 x2, Holyoke 2015.

## Research agenda

1. Second-path verification of buildPath (software check, no data). Write a test that drops a bridging member, and a second test that compares Dijkstra on -log of the transposed PE with the traced chain under non-unit EMU and role priors. If both confirm, the fix is to exclude dropped members and relax edges in the query direction inside distToTarget, then retire or re-scope the PROVABLY-OPTIMAL comment. The fix is Paul's to apply.
2. Re-scope citations. Drop Furnas 2019 from recruitCost (FEC limits are the basis). Remove Sotoudeh's 'mathematical certainty' and probability wording (D-025). Ground the chain in the classical -log p reliable path rather than Chen 2016. Withdraw or re-scope the Kim-Kunisky and Porter community claims in the brain note. Soften the GCOMB, MOPBD* and RELINK labels. Limit the (1-1/e) claim to the untargeted coverage call on the coverage proxy.
3. Paul's D-021 ruling on the LDA-derived filing-year hops in the staffer overlay (staffer_routing_evidence.py and staffer_routing.py:346-367).
4. Estimate ASKM with an observational House study within one Congress: link documented member-to-member asks (Dear Colleague letters, sponsor outreach) to cosponsor join timing in an event-history model, stratified by majority and party, or labelled UNSTRATIFIED. Run the same data to test hop independence along join sequences.
5. Test recruiter whip capacity: do prolific recruiters' marginal recruits drop off? This needs recruiter attribution, which raw cosponsor lists do not provide.
6. Start an outreach log: record attempted and completed introductions, the route used, its Yen alternatives, and the seniority tier reached (Kalla & Broockman coding). Fit hop-cost weights by path-selection conditional logit (Zeller design, UNVERIFIED lead) with a single-look holdout. Outputs remain heuristic scores (D-025).
7. Coverage audit of Politico-reconciled co-work against an independent staff payroll source over a named window, reporting recall of work ties by chamber.
8. Node-loss robustness simulation on the staffer graph (Paul's centerless goal): route availability and cost inflation under removal of the top-k hub staffers or offices.
9. Repo-wide check of whether the anchoring member still serving enters staffer route cost. If a term is added, describe it only as dependence on the patron's current office (Blanes i Vidal, one study), never as access or influence.
10. Correct the component description: the live closeness router defaults to bidirectional Dijkstra with heuristic 0, not one-directional search. Document that the non-potential member-context, anchor-uncertainty and balance-step terms can change the winner beyond the documented tie rule.
