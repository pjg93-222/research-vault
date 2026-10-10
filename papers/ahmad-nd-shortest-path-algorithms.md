---
type: paper
title: "Shortest Path Algorithms for Social Network Strengths"
authors: ["Amreen Ahmad", "Tanvir Ahmad", "Harsh Vijay"]
year: "Not stated in extracted text (data runs to Feb 2010; references include a 2010 item)"
venue: "Not visible in extracted text (LNCS-style layout per metadata hint; not verified)"
doi: ""
category: "Path-Finding Algorithms"
systems: ["melange-pathfinding", "melange-sna"]
components: ["melange-pathfinding.staffer-closeness-routes", "melange-pathfinding.member-recruitment-path", "melange-sna.multilayer-social-press-edges", "melange-pathfinding.staffer-overlap-routes"]
paul_annotated: true
paul_markup_count: 42
evidence_status: "verified"
source: "[[_sources/ahmad-nd-shortest-path-algorithms.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Path-Finding Algorithums\\paper.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Path-Finding Algorithums\\paper.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Path-Finding Algorithums\\paper.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Path-Finding Algorithums\\paper.pdf"]
sha12: ["fa6c2ee00be5"]
tags: [paper, path-finding-algorithms]
---

# Shortest Path Algorithms for Social Network Strengths

**Amreen Ahmad, Tanvir Ahmad, Harsh Vijay (Not stated in extracted text (data runs to Feb 2010; references include a 2010 item)).** *Not visible in extracted text (LNCS-style layout per metadata hint; not verified).* Theme: [[themes/path-finding-algorithms|Path-Finding Algorithms]].

> [!abstract] Summary
> Ahmad, Ahmad and Vijay compare two ways of finding a social-search path between a source and a target. The first is plain Dijkstra, measured in hops. The second is the "strongest path" of Hangal et al. (2010). In that method, each directed edge carries an influence weight: the share of one node's total interaction investment that goes to the other node. The method then picks the path that minimises log(1/D) plus the log of 1 over the product of edge influences, with D = 0.95. The test uses 500 random source-target pairs in each of two networks: a DBLP co-authorship subset (2001-2010, about 38.9k authors) and a 15-day Twitter retweet graph (about 23.5k users, 32k directed edges). The strongest path was longer than the shortest in 64.6% of Twitter pairs and 45.0% of DBLP pairs. Mean lengths were 6.8 vs 4.9 hops on Twitter and 4.7 vs 3.3 on DBLP. The paper calls the weighted search "better", but it reports no measure of path quality, no outcome and no significance test. What it actually shows is only that the weighted route differs from the hop route and is longer. It is a method note, not evidence about legislatures.
> 
> For Paul's systems, it matters in three places:
> - **Staffer closeness router.** Its hop cost, -ln(0.8 x strength), shares only the log(1/product of influences) part of Hangal's objective. The router's per-hop ln(1/0.8) is a length penalty the paper does not print, since Algorithm 1 adds log(1/D) once per path. So this paper is a partial method source for that router, but it gives no validation of it.
> - **Social interaction layer.** Its edge probability (events divided by the author's posting volume) is a similar per-author volume normalization to the paper's Eq. 3 retweet influence, though the denominator is the author's total posts rather than total retweets. The layer then averages the two directions, which throws away the asymmetry the paper argues for.
> - **Dashboard recruitment path.** The path matrix PE is directed, because receiver edge multipliers and role priors make it so. But the backward Dijkstra (distToTarget) multiplies edges in the forward direction away from the target. So the chain labelled "PROVABLY-OPTIMAL" is max-product only if PE is symmetric, and even then tracePath skips "dropped" nodes that distToTarget does not exclude (DE-062).

## Paul's annotation

From [[annotations/Path-Finding Algorithums - Path-Finding Algorithums - Annotated Bibliography|Path-Finding Algorithums - Path-Finding Algorithums - Annotated Bibliography]]:

> *(PDF) Shortest Path Algorithms for Social Network Strengths*, 2025, [www.researchgate.net/publication/315330410_Shortest_Path_Algorithms_for_Social_Network_Strengths](http://www.researchgate.net/publication/315330410_Shortest_Path_Algorithms_for_Social_Network_Strengths).

## Paul's markup in the PDF

- p1 Highlight: "The influence of a person X over person Y is defined as the ratio of Y's investment that Y makes on X." — 
- p1 Highlight: "In this paper, it is proved that in social graph result can prove to be more effective by incorporating the concept of directed and weighted influence edges taking into account both asymmetry and tie strength." — 
- p1 Highlight: "two algorithms-Dijkstra shortest path algorithm and Influence based strongest path algorithm." — 
- p1 Highlight: "where the aim is to search for a “sequence” of people or search a particular person who will prove beneficial for person X in finding a job or introduce him to some particular person." — 
- p1 Highlight: "Hence, in these types of social networks the available shortest path is returned between the source and the target as a result of social search." — 
- p2 Highlight: "A large number of relationships are maintained by people in real life with varying tie strength: family, work colleagues, friends, close friends etc." — 
- p2 Highlight: "For capturing ties of varying strength, online social networks have proved to be beneficial." — 
- p2 Highlight: "Here users and target person need not know each other. It was found that professional ties can vary from medium to weak in strength and successful social search depended a lot on it" — 
- p2 Highlight: "Consideration of tie strength with different information such as homophily, geographical proximity is the common concept that has been used in the above research." — 
- p3 Highlight: "To find correlation between users symmetric measure of affinity is used internally by this site [7]." — 
- p3 Highlight: "In signed networks, it is considered that edges can be either negative or positive or absent [9]. A lot of work has been done in social network domain for determining relationship strength [3]." — 
- p3 Highlight: "As discussed above, if there exists a path where each node has considerable influence over other succeeding node, then global search problem can be successful." — 
- p3 Highlight: "such as co-authorship network in DBLP" — 
- p3 Highlight: "Influence has asymmetric nature that is if Y is highly influenced by X, then it is not guaranteed that the reverse will be true." — 
- p4 Highlight: "Influence (X,Y) = Invest(Y,Z) In " — 
- p4 Highlight: "Invest(Y,X)" — 
- p4 Highlight: ". In Figure 1(a) an undirected weighted graph is shown where the edges represent the investments and the same graph is shown in Figure 1(b)where influence is represented by weighted edges." — 
- p4 Highlight: "non-directional interaction" — 
- p4 Highlight: "Node X is supervisor," — 
- p4 Highlight: "Number of co-authorships between pair of nodes is represented by edge weights." — 
- p4 Highlight: "Shortest path between two nodes is computed using Dijkstra algorithm" — 
- p5 Highlight: "Dijkstra(G,start,Dest)" — 
- p5 Highlight: "Performance of global search on two large network" — 
- p6 Highlight: "i j Influence(v , v ) = j k Papers(v , v )" — 
- p6 Highlight: "i j Papers(v , v )" — 
- p6 Highlight: "There are approximately 23,491 unique users and 32,132 retweet directed edges." — 
- p6 Highlight: "DBLP and Twitter lies below one, this is due to the fact that influence has asymmetric nature in real life." — 
- p6 Highlight: "S(Pshort)" — 
- p6 Highlight: "number of hops" — 
- p6 Highlight: "strength" — 
- p6 Highlight: "so randomly one of them is chosen." — 
- p6 Highlight: "S(Pstrong)" — 
- p6 Highlight: "influenced based strongest path algorithm" — 
- p7 Highlight: "strongest path in 64.6 % and 45 %" — 
- p7 Highlight: "It is also seen that average influence of a node in Twitter is 0.67 and it is almost nearly same in DBLP, that is 0.6" — 
- p7 Highlight: "|Pstrong| and |Pshort| denotes length of strongest and shortest path There are 500 total nodes in the network" — 
- p8 Highlight: "the result is that inclusion of edge weights in social graph for performing social search gives better result than search performed on unweighted graph." — 
- p8 Highlight: "average one or two hops more than" — 
- p8 Highlight: "proved that tie strength concept in weighted graph gave better result than unweighted graph." — 
- p8 Highlight: "The result from the experiment proved that shortest path was on an average one or two hops less than strongest path." — 
- p8 Highlight: "but the selection of an influential path based on strongest path would yield better result than a random path based on shortest path." — 
- p8 Highlight: "Influence based strongest path based on social interactions would boost the performance of social search algorithms in online social networks as compared to traditional Dijkstra algorithm." — 

## Study

**Question.** Does using directed, weighted influence edges (capturing asymmetry and tie strength) make social-search paths from source to target more effective than the hop-count shortest path given by Dijkstra? (Sec. 1, p.1-2; Abstract)

**Theory.** Influence of X on Y is the proportion of X's investment (interaction effort, time or count) directed at Y, normalized over all of X's investments; influence is asymmetric and lies between 0 and 1 (Sec. 3.1-3.3, p.3-4, citing Hangal et al. 2010 [5]). Tie strength from Granovetter's weak-ties argument motivates the weighting (Sec. 1.1, p.2). Global social search is treated as routing along the most influential path (Sec. 3, p.3).

**Data.** Two datasets. (1) DBLP subset: about 38,942 unique author names and 16 data-mining conferences, January 2001 to February 2010; nodes are authors; undirected co-authorship edges weighted by co-authored papers (Sec. 5.1, p.6). (2) Twitter: 15 days of crawled tweets, about 23,491 unique users and 32,132 directed retweet edges; undated (Sec. 5.2, p.6). Experimental unit: 500 randomly selected source-destination pairs per dataset (Sec. 5.1, p.6). The Table 1 caption says "500 total nodes in the network", which conflicts with the stated network sizes; this is read here as the pair count, since the Table 1 counts sum to 500 per dataset.

**Method.** Descriptive comparison of two path algorithms on the same source-destination pairs. Baseline: Dijkstra shortest path, measured by hop count, ties broken at random (Sec. 4, p.5; Sec. 5.1). Comparator: influence-based strongest path (Algorithm 1, from [5]), which enumerates candidate paths, multiplies edge influences along each path, and returns the path minimizing log(1/D) + log(1/product of influences) with D = 0.95 (p.5). Edge influence: DBLP = Papers(vi,vj)/sum_k Papers(vj,vk) (Eq. 2, p.6); Twitter = Retweets(Y,X)/sum_Z Retweets(Y,Z) (Eq. 3, p.6); general form Eq. 1 (p.4). Outcome measures: path length in hops and mean influence. No significance tests.

**Identification.** None. No causal design, no randomized or quasi-experimental variation. The two algorithms are compared descriptively on the same sampled pairs.

**Scope.** Population: DBLP authors in 16 selected data-mining conferences (2001-2010) and Twitter users in a 15-day undated window. Institutions: none studied. Era: 2001-2010 DBLP data; Twitter window undated. Generalization is claimed to "social networks" and "online social networks" (Abstract; Sec. 6) from two datasets only.

## Key findings

- Influence is asymmetric; most node-level influence values lie below one. DBLP influence is more evenly distributed, while highly influential Twitter nodes sit in the tail of the distribution. (Mean influence: Twitter 0.67, DBLP 0.60) — Sec. 5.3, p.6-7 and Fig. 2
- Twitter: the shortest path was smaller than the strongest path in most pairs; the two were equal in the rest. (Strongest > shortest: 323 of 500 (64.6%); equal: 177 of 500 (35.4%)) — Sec. 5.3 and Table 1, p.7
- DBLP: the strongest path was longer than the shortest in fewer pairs, and equal in a majority. (Strongest > shortest: 225 of 500 (45.0%); equal: 275 of 500 (55.0%)) — Sec. 5.3 and Table 1, p.7
- Average path lengths: strongest paths are longer by about 1.9 hops (Twitter) and 1.4 hops (DBLP) on average. The text says one or two hops (Twitter) and one hop (DBLP), which is looser than these means. (Twitter: Avg |Pstrong| 6.8, Avg |Pshort| 4.9; DBLP: Avg |Pstrong| 4.7, Avg |Pshort| 3.3) — Sec. 5.3 and Table 2, p.7
- The paper concludes that weighted influence search beats unweighted search and that influential paths beat random shortest paths. No path-quality or path-influence metric is reported, so 'better result' is asserted, not measured. (None reported for path quality) — Abstract p.1; Sec. 5.3 p.7; Sec. 6 Conclusion p.8

## Limitations

- No limitations section. The paper says only that DBLP and Twitter differ because of structural differences (Sec. 5.3, p.7).
- 'Better result' and 'it is proved' (Abstract; Sec. 6) rest on path-length comparisons over a 500-pair sample; no path-quality metric and no statistical tests.
- Two datasets only: a 2001-2010 data-mining subset of DBLP and a 15-day Twitter window; generalization is untested.
- Influence definitions are narrow: DBLP uses co-authored papers in 16 conferences, Twitter uses retweets only.
- Shortest-path ties are broken at random, so the equal/unequal shares depend on that choice (Sec. 5.1, p.6).
- The 500-pair sample size is not justified by a power or convergence analysis; the Table 1 caption conflicts with the network sizes.
- Algorithm 1 enumerates all simple paths (exponential; scale not discussed). The decay constant D = 0.95 is unjustified. Algorithm 2 (Dijkstra) pseudocode appears to push the unextended path and to use edge cost, while experiments count hops, so the baseline as coded is ambiguous.
- Figure 1 is an illustrative four-node example (supervisor X and students A, B, Y; seven co-authored papers for A-X), not evidence.

## Short quotes

- "tie strength concept in weighted graph gave better result than unweighted graph" (Sec. 6 Conclusion, p.8)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | method-source | Algorithm 1 (from [5], Hangal et al.) enumerates all paths, multiplies edge influences along each, and returns the path minimizing Total_influence = log(1/D) + log(1/product of edge influences), with D = 0.95. As printed, log(1/D) is added once per path, not per hop, so it is a constant that cannot change which path wins. Table 2: average strongest vs shortest path length 6.8 vs 4.9 hops (Twitter) and 4.7 vs 3.3 (DBLP), over 500 random pairs per dataset. No path-quality metric is reported. (Sec. 4, p.5 (Algorithm 1); Sec. 5.3 and Table 2, p.7) | Hop cost = -ln(HOP_FACTOR x strength) + w x uncertainty_cost, with HOP_FACTOR = 0.8, i.e. ln(1/0.8) + ln(1/strength) per hop. The ln(1/strength) sum matches the paper's log(1/product) term. The per-hop ln(1/0.8) is a hop-length penalty with no counterpart in Algorithm 1 as printed, where log(1/D) is a per-path constant. Solved by bidirectional Dijkstra with heuristic 0, not by enumerating paths. Edges are undirected. `fec/surfaces/staffer_closeness_routes.py:115-116; fec/staffer_network/closeness_math.py:20; fec/surfaces/staffer_closeness_routes.py:3-21` | Only the log-inverse-product part of the objective carries over. The router's per-hop decay is its own design (possibly closer to Hangal's original paper, not checked here), not something Ahmad et al. print. The paper does not validate the objective: it never measures whether strongest paths succeed more often. Its influence is directed and normalized by investment; staffer strength is symmetric exposure. Route cost is a heuristic model output, not an introduction probability (D-025). | moderate | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Sponsor-to-target max-product path and chain hold\|Sponsor-to-target max-product path and chain hold]] | qualifies | Influence is asymmetric: if Y is highly influenced by X, the reverse is not guaranteed, even for undirected interactions such as co-authorship (Sec. 3.2; Sec. 1.1). Asymmetry 'has to be considered' in social search; the paper does not say in so many words that edges must be directed. Average node influence is 0.67 (Twitter) and 0.60 (DBLP), and most node influence values lie below one (Sec. 5.3, Table 2). (Sec. 1.1, p.2; Sec. 3.2, p.3; Sec. 5.3 and Table 2, p.6-7) | ADJ is symmetric, but PEBASE[u][v] multiplies by the receiver's EMU[v], and configureIssueNetwork scales PE[u][v] by source_shock(u)/target_resistance(v), so PE is directed. distToTarget(t) relaxes d[v] = d[u] x PE[u][v] outward from t, so dT[v] is the best product along t -> ... -> v in the forward edge direction. tracePath scores sponsor -> target moves with PE[cur][v] x dT[v]. When PE is asymmetric, the chain the comment calls 'PROVABLY-OPTIMAL' is therefore not guaranteed to be the max-product sponsor-to-target chain. tracePath also skips 'dropped' nodes that distToTarget does not exclude, which is a second way the trace can depart from the optimum. (DE-062). `fec/surfaces/templates/make_dashboard_html_00.tpl:547,586,594-597,734,735,740` | The paper's asymmetry point is enough to say edge direction matters for path search. The defect comes from reading the code, not from the paper. Its size depends on how far EMU (0.80-1.20) and the role priors push PE[u][v] away from PE[v][u], and on whether dropped nodes lie on the best chain. It may be small. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-sna#Social and press-engagement layers\|Social and press-engagement layers]] | qualifies | Twitter influence(X,Y) = Retweets(Y,X) / sum_Z Retweets(Y,Z): the share of Y's retweets that go to X. This is a directed weight normalized by the retweeter's total retweet volume (Eq. 3). The paper's reason for directed weights is that influence is asymmetric (Sec. 3.2). (Sec. 5.2 Eq. 3, p.6; Sec. 3.2, p.3) | p(a->b) = weighted events(a->b) / max(25, posts(a)), capped at 0.25 per direction. Events are reposts (1.0) and mentions (0.4); posts(a) counts all of a's Quorum posts, not only reposts, and excludes Bluesky. This is a per-author volume normalization like Eq. 3, but the denominator is total posts rather than total retweets. The pair weight is then the mean of the two directions, and only a 'symmetric' edge list is written, so direction is discarded. `fec/network/social_interaction_layer.py:14-18,54-58,60-61,67-69` | The paper's evidence is descriptive: a 15-day undated Twitter crawl in which strongest paths are longer, with no outcome. It does not show that directed retweet edges predict anything legislative. At most it gives weak support for keeping a directed variant of this layer alongside the symmetric one before path or recruitment use. Per-edge volume normalization was measured and REJECTED in this project's SNA tests: do not read this as support for wider volume normalization. | weak | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Ordered person-overlap routes and employment-routing overlay\|Ordered person-overlap routes and employment-routing overlay]] | context | Hop-count shortest paths ignore tie strength and asymmetry (Abstract). Tie strength matters for social search (Granovetter motivation, Sec. 1.1). The strongest path uses the product of normalized influences (Sec. 4). (Abstract, p.1; Sec. 1.1, p.2; Sec. 4, p.5) | Overlap routes minimize summed integer distances plus one balanced unit (DISTANCE_SCALE) per introduction. There are two cost forms. Shared-background mode uses ceil(1e6 x 128 / strength), with strength = sum(weight x log2(1 + shared facts)). Work-time mode uses ceil(1e6 / strength), with strength = proximity x log2(1 + overlap days / 30). Both are additive inverse strengths, not -log of a bounded weight, so neither can be read as a product of per-hop reliabilities. `fec/surfaces/staffer_overlap_routes.py:27-34,37-39,211,303,331` | Context only. The paper does not compare inverse-strength costs with log-product costs, and the overlap router is a deliberate design choice. Shared work days and shared background are heuristic tie scores, not measures of closeness, access or introduction probability (D-025). Influence language is barred on revolving-door edges. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The summary says the closeness router's hop cost -ln(0.8 x strength) 'has the same form as Hangal's per-hop log(1/D) + log(1/influence)'. In the paper's Algorithm 1, log(1/D) is added once per path, not per hop, so it cannot change which path wins. Only the log(1/product of influences) part is shared. The router's per-hop ln(1/0.8) is a length penalty the paper does not print.
> - Summary issue: The summary says the social layer's edge probability is 'essentially the paper's Eq. 3'. Eq. 3 divides by the retweeter's total retweets. The layer divides by the author's total Quorum posts (floor 25), adds 0.4-weighted mentions, and caps each direction at 0.25. It is a similar per-author volume normalization, not the same one.
> - Summary issue: The summary says the recruitment chain is exactly max-product 'only when PE is symmetric'. Symmetry is necessary but not sufficient: tracePath also skips 'dropped' nodes that distToTarget does not exclude. The finding stays UNVERIFIED (DE-062).
> - Summary issue: The related link to steinertthrelkeld-2018-twitter-data was removed. That item's own extraction says the book never discusses reposts, mentions, edge probabilities or normalization, so it does not cover the retweet-influence definition used here.
> - Summary issue: The related link to chen-2016-most-reliable-path claimed the same objective. Chen maximizes on-time arrival probability under stochastic travel times, not a product of edge probabilities. The link holds only through the dashboard's citation.

## Gaps and flags

- The recruitment path's backward Dijkstra does not transpose a directed PE. Nothing checks that the 'provably optimal' chain is optimal when receiver and role multipliers make edges asymmetric, or when dropped nodes lie on the best chain.
- The social interaction layer computes directed probabilities but publishes only their mean. No directed social edge reaches path search or recruitment.
- No router (member recruitment, staffer closeness or staffer overlap) has an outcome log against which 'strongest' and 'shortest' routes could be compared. The paper never measured this either, so the strongest-vs-shortest question stays open in both.
- The paper's influence weight (an edge divided by the actor's total investment) is a volume normalization. Per-edge volume normalization was measured and REJECTED in this project's SNA tests: cite the paper as a method description only, never as grounds to extend volume normalization to the cosponsor or staffer graphs.
- The paper's claims that 'it is proved' and 'better result' (Abstract; Sec. 6) rest only on path-length differences over 500 pairs, with no outcome or test. Do not quote them as validation of any router.
- The Table 1 caption says '500 total nodes in the network', which conflicts with the stated network sizes; read it as 500 pairs. Any figure quoted from the paper must name the window (DBLP 2001-2010; Twitter 15 days, undated) and the denominator (500 pairs).
- Route costs and strengths are heuristic model outputs, not introduction or access probabilities (D-025).

Paul's highlights centre on the asymmetric definition of influence, the comparison between Dijkstra and the strongest path, and the conclusion that weighted search beats unweighted search. He left no notes, and his positions list Ahmad only under member-recruitment-path, as the strongest-path citation. The system partly matches that reading. The staffer closeness router shares the log-inverse-product part of the Hangal objective, but adds its own per-hop ln(1/0.8) length penalty, which the paper does not print. The recruitment path uses a most-reliable (max-product) Dijkstra, which follows Chen 2016 more closely than Ahmad. Where the system departs from the asymmetry Paul highlighted: the social layer averages away direction, and the recruitment path's backward pass does not respect PE's direction. Paul's highlighted line about "better result" goes further than the evidence, since the paper measures only path length. His takeaway should be held as a method preference, not as a validated finding.

## Related papers

- [[chen-2016-most-reliable-path]] — complements — The dashboard recruitment path cites Chen et al. 2016 for its max-product ('most reliable') chain, the same objective Ahmad et al. call the strongest path. Chen's own objective is different: it maximizes on-time arrival probability under stochastic link travel times, not a product of edge probabilities.
- [[haythornthwaite-1996-social-network-analysis]] — complements — Both treat tie strength (Granovetter) and direction as core tie properties. Paul cites Haythornthwaite for directionality, and in the staffer-closeness-routes and SNA components.
- [[duan-2025-breaking-sorting-barrier]] — same-method — Directed single-source shortest paths with non-negative weights. An exact strongest-path search (Dijkstra on -log influence) could use this in place of Ahmad's exhaustive path enumeration. Both are cited for member-recruitment-path.
- [[lawande-2022-systematic-review-analysis]] — complements — Compares Dijkstra with A*, BFS and greedy best-first on grids, which includes the Dijkstra baseline used here. Both are cited for member-recruitment-path.

### Linked from
- [[cao-2025-social-network-group]] — complements — Strongest-path search over social tie strength corresponds to the review's trust-propagation operators and path-length decay concern.
- [[zeller-2016-step-path-selection]] — complements — Both turn tie strength into path cost for shortest-path search. Zeller adds a way to fit that transform empirically (used vs available paths) rather than setting it by hand.