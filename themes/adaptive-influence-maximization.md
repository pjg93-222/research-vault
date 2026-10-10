---
type: theme
name: "Adaptive Influence Maximization"
paper_count: 15
tags: [theme]
---

# Adaptive Influence Maximization

**Adaptive Influence Maximization: synthesis of 15 works**

Paul's addendum (two notes, sha12 d09295bf5960 and 46c7c53ceb7f) gives this theme a three-tier spine that mirrors Melange:

- **Tier 1, one-shot greedy.** The problem is static and submodular, and greedy carries the 1-1/e bound (about 63%).
- **Tier 2, adaptive greedy.** This is the bridge tier: replanning within a campaign as intelligence arrives, with a guarantee from adaptive submodularity and lazy (CELF-style) evaluation.
- **Tier 3, a learned RL envelope.** It handles compounding across campaigns, where no greedy guarantee reaches.

His unifying claim is: static inner problem submodular, sequential inner problem adaptive submodular, full cross-campaign problem non-submodular. He says RL4CCIM confirms the last point for complex contagion, and he places the durable moat in that non-submodular regime.

**What the 15 works support.** The two Golovin-Krause papers ([[golovin-2011-adaptive-submodularity-theory]], [[golovin-2011-adaptive-submodularity-theory]]) supply the tier-2 guarantee. A cluster of papers maps where the guarantee ends:
- competitive linear-threshold models ([[borodin-nd-threshold-models-competitive]]);
- complex contagion ([[chen-2023-complex-contagion-influence]]);
- edge activation in closed networks ([[arya-2025-relink-edge-activation]]);
- threshold CDFs that are not concave ([[ma-2022-adversarial-attack-graph]], Proposition 2).

[[bharathi-nd-competitive-influence-maximization]] shows the guarantee survives competition, but only for the last mover. A learned-policy lineage from the Tambe group ([[chen-2021-contingency-aware-influence]], [[kamarthi-2020-influence-maximization-unknown]], [[chen-2023-complex-contagion-influence]]) plus [[arya-2025-relink-edge-activation]] and [[kumar-2022-influence-maximization-social]] shows where RL earns its keep: a non-submodular objective, a graph that is not observed, or a runtime budget on new graphs.

Two strands sit outside the spine but bear on the system:
- **Cheap heuristics against greedy:** [[trivedi-2020-efficient-influence-maximization]] and [[almazaydeh-2026-heuristic-centrality-methods]].
- **Competition over time in voter models:** [[cai-2025-competitive-influence-maximization]] and [[moreno-2021-shadowing-shielding-effective]]. These are the only works where timing and reversible opinions change who wins.

[[zareie-2023-influence-maximization-social]] is a taxonomy that exposes design choices the system already made.

**Where the evidence falls short of Paul's claim.** No work in this set studies compounding across campaigns. RL4CCIM ([[chen-2023-complex-contagion-influence]]) proves non-submodularity for a *single* seeding under its K-complex model. It does not prove it for repeated campaigns, so "confirms" overstates it. The cross-campaign claim stays an open hypothesis. The tier-2 bridge also has a practical hole. Adaptive greedy needs observed outcomes, a known prior and near-exact marginals. No Melange engine observes real take-up today, so the adaptive half of tier 2 is unbuilt (bearing from [[golovin-2011-adaptive-submodularity-theory]]).

No paper in the set uses observed legislative, lobbying or persuasion outcomes. Every quantitative result is a simulated or proven property of a model kernel on synthetic or public non-political graphs. The one apparent exception is Cai's "Congress votes" mentions graph, which is still pure simulation.

The set covers only four of the ten works Paul's addendum anchors: Golovin-Krause, Kamarthi, Chen 2021 and Chen 2023. The other six are missing (Leskovec/CELF, Sutton-Barto, Ng 1999, Watkins, Khalil, GCOMB/Manchanda); see Gaps.

## Strands

### Guarantee theory: submodularity and adaptive submodularity (Tier 1 to Tier 2)

This strand supplies the formal backbone of Paul's tiers 1 and 2.

**The Golovin-Krause theorem and its costs.** [[golovin-2011-adaptive-submodularity-theory]] (COLT extended abstract) and [[golovin-2011-adaptive-submodularity-theory]] (JAIR version) prove that under adaptive monotonicity and adaptive submodularity, alpha-approximate adaptive greedy run for l steps gets at least (1 - e^{-l/(alpha k)}) of the best k-item policy. That is 1-1/e when alpha = 1 and l = k. The guarantee has a price:
- The prior must be known.
- Marginals estimated with additive error epsilon cost l*epsilon (COLT text after Thm 4).
- Without adaptive submodularity there is no polynomial approximation within O(|E|^(1-eps)) (JAIR Thm 26 / COLT Thm 9).

**What the JAIR version corrects.** It replaces the earlier logarithmic average-cost cover bounds with squared-log bounds (flaw identified by Nan and Saligrama 2017). It also retracts the myopic-feedback viral-marketing model that the COLT version's Thm 6 covered. That second point matters for Paul: a design that seeds, observes take-up and replans most naturally uses myopic feedback, and that is exactly the case no longer certified. The lazy greedy (Algorithm 2) is the CELF analogue Paul cites as what makes tier 2 practical. It ran about 3.5-7x faster on 46 sensors and 30-38x faster on 357 sensors (sensor-placement experiments only).

**Competition.** [[bharathi-nd-competitive-influence-maximization]] extends the independent cascade to several players. With the opponent's seeds fixed, a player's reach is monotone and submodular in its own seeds (Lemma 1). Greedy is therefore a (1-1/e) best response, but only for the last player to commit. The price of competition is at most 2. Pure Nash equilibria need not exist, and first-mover strategy is solved only on disjoint lines.

**Threshold models.** [[ma-2022-adversarial-attack-graph]] reaches the guarantee from an unrelated domain, a black-box attack on a GNN recast as linear-threshold influence maximization. Its Proposition 2 gives the cleanest condition in the set: the objective is submodular when every threshold CDF is concave on [0, inf).

**Where the four agree.** The guarantee is a property of the objective's structure, not of greedy itself. It survives randomness, adaptivity and a fixed opponent. It does not survive an opponent who responds, sampled marginals with unbounded error, or weights layered on top of marginal gain.

Works: [[golovin-2011-adaptive-submodularity-theory]], [[golovin-2011-adaptive-submodularity-theory]], [[bharathi-nd-competitive-influence-maximization]], [[ma-2022-adversarial-attack-graph]]

### Where submodularity breaks (the boundary of Tier 3)

Four works map the edge of the guarantee. Each uses a different mechanism.

**Competitive linear-threshold models.** [[borodin-nd-threshold-models-competitive]] shows that most competitive extensions lose the guarantee:
- The weight-proportional adoption split is not even monotone. Adding an A seed lowers the root's A-probability from 11/40 to 103/400 on a small tree counterexample.
- The separated-threshold model is monotone but not submodular, and no forcing or tie-breaking rule fixes that.
- With technology-specific weights, the follower problem is NP-hard to approximate within N^(1/2-eps).
- Only the OR model, where each technology spreads independently and a submodular rule picks the winner at the end, keeps 1-1/e.

**Complex contagion.** [[chen-2023-complex-contagion-influence]] proves non-submodularity (Thm 1) and NP-hardness (Thm 2) under K-complex adoption, where a node joins once at least K neighbours are active. Paul's addendum leans on this paper.

**Closed-network edge activation.** [[arya-2025-relink-edge-activation]] states that its problem is non-submodular and offers no guarantee.

**Threshold shape.** [[ma-2022-adversarial-attack-graph]] states the converse of its own condition: S-shaped adoption, a threshold CDF that is not concave, falls outside Proposition 2.

**What these papers do not dispute.** They mark the scope of [[golovin-2011-adaptive-submodularity-theory]]; they do not contradict it. The verification notes on every record in the file downgrade the earlier 'contradicts' labels to 'complements' for this reason.

**Bearing on Paul's claim.** Non-submodularity arrives *within* a single campaign once adoption is threshold-like or competition is linear-threshold. It does not need cross-campaign compounding. That makes the tier-3 boundary closer than the addendum implies. It also means RL4CCIM shows complex contagion is non-submodular, not that the cross-campaign problem is.

Works: [[borodin-nd-threshold-models-competitive]], [[chen-2023-complex-contagion-influence]], [[arya-2025-relink-edge-activation]], [[ma-2022-adversarial-attack-graph]], [[golovin-2011-adaptive-submodularity-theory]]

### Learned policies: the RL and GNN envelope (Tier 3)

**The Tambe-group lineage.** Paul's addendum names a progression: Khalil -> GCOMB -> Kamarthi -> Chen 2021 -> Chen 2023. Three of its links are in this set.

[[kamarthi-2020-influence-maximization-unknown]] (arXiv 2019, AAMAS 2020) learns which nodes to query when the graph is hidden. The setup is 5 queries from 5 random starts, then greedy picks 10 seeds on the discovered subgraph. Geometric-DQN closes 7.91-26.6% of the gap between CHANGE and greedy-optimal, depending on network family (Table 4, best variant per family). The paper's own text quotes this range inconsistently, as 7-23% in some places and 7-26% in others.

[[chen-2021-contingency-aware-influence]] (RL4IM) casts seeding with probabilistic take-up as an MDP. Each invited node accepts with a known, uniform q. Training is made tractable by a state abstraction and a surrogate reward (O(2^b) -> O(1); the Thm 3 gap bound grows arbitrarily loose as q nears 0 or 1). On 200-node synthetic graphs (IC p=0.1, q=0.6, budget 8, 200 runs per setting), it only *matches or slightly beats* CHANGE greedy. Its case rests on runtime on unseen graphs: CHANGE takes about 10 hours at 20 seeds on 1,000 nodes.

[[chen-2023-complex-contagion-influence]] (RL4CCIM, IJCAI-23) builds on RL4IM and gives RL a real quality win. Under deterministic K-complex cascades on nine small networks, it posts the best normalised influence on every network (some ties). On India it scores 0.5347 against greedy's 0.0396. On DC and DOSIM it matches the MIP optimum, and it sits about 6-8% below the optimum on CHANGE, MFP and SPY.

**Outside the lineage.** [[arya-2025-relink-edge-activation]] (RELINK) trains a Double DQN on 40 small synthetic graphs and transfers it to 20 real ones. It beats Random, Degree and FoF in about 462-464 of 480 configurations and PSNA in 456 of 480. The margin narrows as the edge budget grows, and PSNA can win on dense graphs at small seed sets. [[kumar-2022-influence-maximization-social]] (SGNN) regresses each node's standalone influence from struc2vec embeddings and takes the top-k. Only a preview of this paper was available, so all of its claims are UNVERIFIED.

**What the strand shows.** RL beats greedy on quality only where greedy has no guarantee (complex contagion) or no graph (Kamarthi). Where the objective is submodular, RL reaches parity and its advantage is runtime. None of these papers models compounding across campaigns or temporal credit assignment across multiple votes, which is Paul's research question (d).

Works: [[chen-2021-contingency-aware-influence]], [[chen-2023-complex-contagion-influence]], [[kamarthi-2020-influence-maximization-unknown]], [[arya-2025-relink-edge-activation]], [[kumar-2022-influence-maximization-social]]

### Cheap heuristics against greedy

This strand asks how much spread a cheap rule gives up against greedy.

**Trivedi.** [[trivedi-2020-efficient-influence-maximization]] has a degree-discount rule (DHICM) that lands 12.5% below CELF at k=10 and 7.9% below at k=30 on NetHept (15,233 nodes, IC p=0.01). It beats plain degree by 10-16% on the same graph. The Monte Carlo run count is not reported. The paper's MICM edge-probability formula raises every algorithm's spread, which follows mechanically from leaving a deliberately low baseline p.

**Almazaydeh.** [[almazaydeh-2026-heuristic-centrality-methods]] tests Facebook ego subgraphs at 1,000 nodes (IC p=0.1, k=10, 10 runs x 50 MC simulations). PageRank reaches 921.78/954.90 = 96.5% of IMM in 0.4 s; the paper itself says 96.3%. CELF++ collapses to 12.9% of IMM, with CV up to 71.5% at 500 nodes. The authors blame 50 MC draws per marginal plus Python but never test that explanation. The paper also has several internal inconsistencies: percentages, the 794x speed-up and Table 4 against its text.

**Ma.** [[ma-2022-adversarial-attack-graph]] points the other way on a saturated-coverage objective. Degree and betweenness are the weakest selectors there, and greedy coverage beats PageRank.

**Kumar.** [[kumar-2022-influence-maximization-social]] shows the main failure mode of learned and heuristic rankers. Ranking by standalone spread ignores overlap between seeds, which is the problem degree-discount exists to fix.

**Reading across the four.** Centrality comes close to greedy when the objective is plain single-colour IC spread and greedy's marginals are noisy. It falls behind when the objective saturates or seeds overlap. The surviving lesson for Melange is about Monte Carlo noise more than about centrality. A greedy loop is only as good as its marginal estimates, and comparing marginals under common random numbers is what protects it.

Works: [[trivedi-2020-efficient-influence-maximization]], [[almazaydeh-2026-heuristic-centrality-methods]], [[kumar-2022-influence-maximization-social]], [[ma-2022-adversarial-attack-graph]]

### Competition, timing and continuous allocation

**Two formalisms.**
- *Progressive cascades:* [[bharathi-nd-competitive-influence-maximization]] and [[borodin-nd-threshold-models-competitive]]. A node, once coloured, stays coloured.
- *Non-progressive voter models:* [[moreno-2021-shadowing-shielding-effective]] and [[cai-2025-competitive-influence-maximization]] (same Brede group). Opinions can flip back, and controllers allocate a continuous budget against an opponent.

**Moreno.** On a single 1,133-node email network with zealots, the continuous optimum spreads out. About 111 of 1,133 nodes get above-average allocation, and the rest is near uniform (std/mean 0.11). This beats the best discrete K-node plan by 5-10% when the passive opponent targets fewer than 100 nodes. Hub targeting explains discrete optima, but continuous optima follow *shadowing* (spend where the opponent spends) and *shielding* (spend on the neighbours of the opponent's targets). In the low-allocation limit the best response is linear in the opponent's spend with coefficient (Ba/Bb - 1): a richer controller shadows the opponent, and a poorer one avoids the opponent's nodes. With two active controllers, the unique pure Nash equilibrium is uniform allocation.

**Cai.** Cai adds time. Against a fixed opponent, the best strategy is to hold budget and spend late. An optimized start beat t=0 in every row of Table 2: five public graphs, T in {16, 256}, budget ratio in {0.1, 1, 10}. On the 'Congress votes' mentions graph at T=16 with equal budgets, the vote share is 0.705 against 0.500. High-degree nodes settle slowly, so short horizons stay far from equilibrium (degree of equilibrium at most 0.82 at T=16). In the two-controller game, the side with more budget moves first, and the second mover can only offset its lead, not overtake it.

**Where they disagree with the cascade papers.** The cascade papers make competition a question of who reaches a node first, and they give guarantees only for a fixed opponent. The voter papers show that once opinions can flip back, timing and whether a budget is concentrated or spread matter as much as seed choice. Both camps agree on one point: an opponent that responds removes the clean guarantees. Bharathi solves only lines; Moreno and Cai find equilibria only numerically.

Works: [[bharathi-nd-competitive-influence-maximization]], [[borodin-nd-threshold-models-competitive]], [[cai-2025-competitive-influence-maximization]], [[moreno-2021-shadowing-shielding-effective]]

### Behaviour-aware framing and seed take-up

[[zareie-2023-influence-maximization-social]] is a narrative survey with no data. It splits influence maximization into behaviour-agnostic and behaviour-aware methods. The behaviour-aware side has four families: interest-aware, opinion-aware (trust-agnostic vs signed friend/foe), money-aware (cost-aware vs profit-aware) and physical-world-aware. It judges three things:
- Topic-aware edge probabilities are the most realistic but need rich history.
- Trust-agnostic models, which treat every tie as positive, are unrealistic.
- Profit-aware IM needs per-user valuations that are rarely available.

The Melange code pairs this survey with [[chen-2021-contingency-aware-influence]] as 'Chen/SNAM' to back contingency-aware seeding. Neither paper supplies an empirical take-up rate. Chen's q=0.6 is an arbitrary simulation default, and the survey gives no magnitudes.

[[kamarthi-2020-influence-maximization-unknown]] is the other form of behavioural uncertainty: an unknown graph instead of unknown willingness. Paul's research question (b) asks whether unknown relationship or persuasion costs can be learned by probing. Kamarthi probes edges, not costs, so it is at best an analogue.

The strand's message is that member features belong in influence maximization, and that the hardest inputs (take-up, signed ties, valuations) are exactly the ones the literature leaves unestimated.

Works: [[zareie-2023-influence-maximization-social]], [[chen-2021-contingency-aware-influence]], [[kamarthi-2020-influence-maximization-unknown]]

## Agreements and debates

**1. Is the full problem non-submodular, and does RL4CCIM show it?**

*Paul's position:* the cross-campaign problem is non-submodular, RL4CCIM confirms this, and the moat lives there.

*The evidence:* [[chen-2023-complex-contagion-influence]] proves non-submodularity for one seeding under deterministic or probabilistic K-complex adoption. It says nothing about compounding across campaigns. [[borodin-nd-threshold-models-competitive]] reaches non-submodularity by a different route, competitive linear threshold, also within a single campaign. Against both, [[bharathi-nd-competitive-influence-maximization]] keeps last-mover submodularity under competitive IC.

*Resolution:* which regime applies depends on the adoption kernel (IC vs threshold) and on whether the opponent responds. Repetition is not the deciding factor. The cross-campaign claim is untested by anything in this set. The record for [[chen-2021-contingency-aware-influence]] adds a further note: that paper *assumes* submodularity, so any attribution of the non-submodular argument to the 2021 paper rather than the 2023 one is a misattribution.

**2. Does RL beat greedy?**

- [[chen-2021-contingency-aware-influence]]: parity with CHANGE greedy on 200-node synthetic graphs. The advantage is runtime.
- [[chen-2023-complex-contagion-influence]]: large quality wins where greedy finds no effective seed set (India: 0.5347 vs 0.0396 normalised influence).
- [[arya-2025-relink-edge-activation]]: beats hand heuristics in about 96% of 480 configurations. The margin is smallest at large edge budgets and on dense graphs.
- [[kamarthi-2020-influence-maximization-unknown]]: wins only because the graph is hidden.

*Consensus:* RL wins on quality only outside the submodular, fully observed regime.

**3. Do centrality heuristics nearly match greedy?**

- [[almazaydeh-2026-heuristic-centrality-methods]]: yes. PageRank reaches 96.5% of IMM (1,000-node Facebook subgraphs, IC p=0.1, k=10).
- [[trivedi-2020-efficient-influence-maximization]]: mostly. DHICM is 7.9-12.5% below CELF on NetHept (k=30 and k=10).
- [[ma-2022-adversarial-attack-graph]]: no. Degree and betweenness are weakest on a saturated-coverage objective.

The disagreement is mostly about objectives: plain spread vs saturating coverage. Almazaydeh's CELF++ collapse (12.9% of IMM) is very likely an artifact of its Monte Carlo budget, a cause the paper names but never tests. It should not be read as a verdict on greedy.

**4. Concentrate or spread?**

- [[moreno-2021-shadowing-shielding-effective]]: continuous optima spread widely, and hub targeting explains only discrete optima.
- [[cai-2025-competitive-influence-maximization]]: small budgets go to low-degree nodes.
- [[ma-2022-adversarial-attack-graph]]: good selectors spread seeds across communities.
- [[kumar-2022-influence-maximization-social]]: its top-k-by-standalone-spread rule pulls the other way, and the cluster reads it as a caution against that rule.

**5. Progressive or reversible adoption.**

Bharathi and Borodin assume a reached node stays reached. Cai and Moreno let opinions flip back, and Cai finds timing (spend late) dominant. No work in the set tests which assumption fits cosponsorship.

**6. Reliability of the source texts.**

Several works contradict themselves internally:
- [[almazaydeh-2026-heuristic-centrality-methods]]: 96.3% vs 96.5%, 11.2% vs 12.9%, a stated 794x speed-up vs the 809x its own times give, and Table 4 vs its text.
- [[kamarthi-2020-influence-maximization-unknown]]: 7-23% vs 7-26%, plus the multi-network training claim against Table 3.
- [[chen-2023-complex-contagion-influence]]: 24.3% vs 24.2%.

The Golovin-Krause pair corrects itself between versions: squared-log bounds replace log bounds, and the myopic-feedback result is retracted.

## Methods

**Dominant methods.**

1. **Formal proofs and reductions.** The Golovin-Krause pair, [[bharathi-nd-competitive-influence-maximization]], [[borodin-nd-threshold-models-competitive]], and the theory sections of [[ma-2022-adversarial-attack-graph]] and [[chen-2023-complex-contagion-influence]]. They use Nemhauser-style telescoping, counterexamples on small trees, Vertex Cover and Set Cover reductions, and Vetta's utility-system theorem.
   - *Weakness:* they establish properties of a model kernel and say nothing about whether real adoption follows that kernel.

2. **Monte Carlo simulation of IC, SIR or LT on synthetic and public graphs.** Used by every empirical paper.
   - *Weaknesses:*
     - Edge probabilities are fixed by convention (p=0.01 in Trivedi; p=0.1 in Almazaydeh, Kamarthi and Chen 2021; uniform 0.5 or random in RELINK).
     - Take-up rates are arbitrary (q=0.6).
     - Run counts are sometimes unreported ([[trivedi-2020-efficient-influence-maximization]]) or too small for greedy marginals ([[almazaydeh-2026-heuristic-centrality-methods]], 50 per marginal).
     - Error bars are often missing.
     - No paper calibrates its kernel to an observed diffusion.

3. **Deep RL (DQN or Double-DQN with structure2vec, GAT, GCN+DiffPool, or Node2Vec embeddings).** Used by [[chen-2021-contingency-aware-influence]], [[chen-2023-complex-contagion-influence]], [[kamarthi-2020-influence-maximization-unknown]] and [[arya-2025-relink-edge-activation]]; the GNN regressor of [[kumar-2022-influence-maximization-social]] is a relative.
   - *Weaknesses:*
     - Small training graphs: 200 nodes, and 25-150 nodes for RELINK.
     - Reward shaping and solution filtering that bake in assumptions.
     - Best variant reported per test set ([[kamarthi-2020-influence-maximization-unknown]]).
     - No guarantees.
     - Expensive training: RELINK took 6,257 s for 2,000 epochs.
     - Evaluation against greedy baselines that are themselves approximate.

4. **Continuous optimisation and mean-field analysis in voter models.** Used by [[cai-2025-competitive-influence-maximization]] and [[moreno-2021-shadowing-shielding-effective]]: gradient ascent with simplex projection, hill climbing, heterogeneous mean-field, relaxation times, and iterated best response for Nash start times.
   - *Weaknesses:* full knowledge of the network, unit edge weights, abstract time units, no noise, and in Moreno's case a single network.

5. **Narrative taxonomy.** [[zareie-2023-influence-maximization-social]].
   - *Weakness:* no data and no magnitudes.

**Cross-cutting weakness.** No study uses observed outcomes in any political, legislative or lobbying setting. Every "spread", "vote share" or "influence" figure in this theme is a simulated output of the authors' model on the authors' graphs. It is never measured influence. It is never a closeness, access or introduction probability (D-025).

## How the works relate

- [[golovin-2011-adaptive-submodularity-theory]] — extends-and-corrects — [[golovin-2011-adaptive-submodularity-theory]]: Same authors. The JAIR version extends the COLT abstract, replaces its logarithmic average-cost cover factors with squared-log factors (flaw found by Nan and Saligrama 2017), and retracts the myopic-feedback viral-marketing model that COLT Thm 6 covered.
- [[chen-2023-complex-contagion-influence]] — builds-on — [[chen-2021-contingency-aware-influence]]: RL4CCIM takes RL4IM as its base architecture and uses it as a baseline (Sec. 5.2, 6.1). Same Tambe group.
- [[chen-2023-complex-contagion-influence]] — cites — [[kamarthi-2020-influence-maximization-unknown]]: RL4CCIM cites Kamarthi et al. (AAMAS 2020) on learned graph sampling. Same lineage; Wilder and Tambe co-author both.
- [[borodin-nd-threshold-models-competitive]] — cites / bounds-scope — [[bharathi-nd-competitive-influence-maximization]]: Borodin cites Bharathi-Kempe-Salek among greedy approaches to competition. Bharathi keeps last-mover submodularity under competitive IC; Borodin shows most competitive linear-threshold variants lose it.
- [[cai-2025-competitive-influence-maximization]] — extends — [[moreno-2021-shadowing-shielding-effective]]: Same Brede group and voter-model line of continuous influence maximisation. Cai adds timing, which Moreno names as its time-invariance limitation (p.14). Cai cites a different Moreno 2020 zealotry paper, not this one.
- [[cai-2025-competitive-influence-maximization]] — cites / contrasts-formalism — [[bharathi-nd-competitive-influence-maximization]]: Cai cites Bharathi only for budget constraints. Cai's non-progressive voter model is an alternative to Bharathi's progressive competitive IC, not a contradiction of it.
- [[chen-2023-complex-contagion-influence]] — marks-boundary-of — [[golovin-2011-adaptive-submodularity-theory]]: K-complex contagion is non-submodular (Thm 1), so it falls outside the adaptive-submodular class. This sets the scope of the theorem and does not contradict it.
- [[arya-2025-relink-edge-activation]] — marks-boundary-of — [[golovin-2011-adaptive-submodularity-theory]]: RELINK states that edge activation in closed networks is non-submodular, so the Thm 4 bound does not carry over (consistent with Thm 9 hardness).
- [[ma-2022-adversarial-attack-graph]] — complements — [[chen-2023-complex-contagion-influence]]: Ma's Proposition 2 (a concave threshold CDF gives submodularity) is the converse of Chen's S-shaped complex-contagion case, where submodularity fails.
- [[almazaydeh-2026-heuristic-centrality-methods]] — tension — [[ma-2022-adversarial-attack-graph]]: Almazaydeh finds PageRank within 96.5% of IMM; Ma finds degree, betweenness and PageRank weaker than greedy coverage. The objectives differ (IC spread vs saturated coverage), so this is a tension, not a direct contradiction.
- [[trivedi-2020-efficient-influence-maximization]] — agrees (same question) — [[almazaydeh-2026-heuristic-centrality-methods]]: Both find cheap heuristics close to greedy under IC: DHICM 7.9-12.5% below CELF on NetHept; PageRank 96.5% of IMM on Facebook subgraphs.
- [[kumar-2022-influence-maximization-social]] — contrasts — [[trivedi-2020-efficient-influence-maximization]]: Trivedi's degree-discount explicitly corrects for overlap between seeds; Kumar's top-k-by-standalone-influence ignores it.
- [[chen-2021-contingency-aware-influence]] — contrasts — [[golovin-2011-adaptive-submodularity-theory]]: Chen positions RL4IM against adaptive IM, which it says does not handle willingness uncertainty. This is a contrast of approach, not a refutation.
- [[arya-2025-relink-edge-activation]] — same-method — [[chen-2021-contingency-aware-influence]]: Both are deep-RL (DQN-family) influence maximization with learned graph embeddings. Topical link; RELINK does not cite Chen.
- [[kumar-2022-influence-maximization-social]] — same-method — [[arya-2025-relink-edge-activation]]: Both use graph embeddings and GNN-style encoders to rank nodes or edges for influence maximization.
- [[kamarthi-2020-influence-maximization-unknown]] — same-lineage / complements — [[chen-2021-contingency-aware-influence]]: Both are Tambe-group RL for influence maximization under uncertainty: Kamarthi over an unknown graph, Chen over uncertain take-up.
- [[zareie-2023-influence-maximization-social]] — co-cited-in-code — [[chen-2021-contingency-aware-influence]]: Melange code cites the pair as 'Chen/SNAM' for contingency-aware seeding. The survey itself does not cite Chen.
- [[moreno-2021-shadowing-shielding-effective]] — complements — [[bharathi-nd-competitive-influence-maximization]]: Continuous competitive allocation in a voter model against discrete competitive seeding in a progressive cascade; two formalisms for the same rival-campaign problem.
- [[chen-2021-contingency-aware-influence]] — agrees — [[almazaydeh-2026-heuristic-centrality-methods]]: Both point to the learned or cheap method's advantage being runtime, not spread: RL4IM reaches parity with greedy, and PageRank comes near IMM at a fraction of the cost.
- [[borodin-nd-threshold-models-competitive]] — agrees — [[chen-2023-complex-contagion-influence]]: Both show that threshold-type adoption, competitive or complex, breaks submodularity within a single campaign.

## What it means for the systems

**Status of every point below.** The bearing tags in the file are VERIFIED or VERIFIED-SOFTENED. However, every summary describes its code mapping as one agent's reading, and several say outright that it is UNVERIFIED (DE-062). Treat each point as a lead to confirm, not a finding.

**Joint optimizer (`fec/engine/optimize.py`)** — see [[systems/melange-allocation]].
- [[golovin-2011-adaptive-submodularity-theory]] is its method source: lazy greedy plus the G-K data-dependent bound as a diagnostic.
- The code already scopes its claims correctly. In a fixed stance state the objective is separable and concave, so the grid optimum is exact, which is stronger than 1-1/e. Once a modeled conversion can occur, it reports no guarantee, consistent with Thm 26/Thm 9.
- `observations` is empty, so it is not an adaptive policy.
- [[borodin-nd-threshold-models-competitive]] bites only on modeled-conversion replanning. There, the max-normalized neighbour term can make adding pro mass *lower* other members' pflip.

**Dashboard budgetRecruit (strongest single lead)** — [[systems/melange-allocation]].
- It claims the full 1-1/e, but multiplies each candidate's gain by a target-proximity weight in [1,3].
- By Thm 4 of [[golovin-2011-adaptive-submodularity-theory]], the supportable bound with a target set is about 0.28.
- This bearing is tagged 'contradicts: VERIFIED'. The fix is a wording or scope correction in the dashboard claim.

**Budget frontier** — [[systems/melange-allocation]], [[systems/melange-validation]].
- It runs lazy greedy at MC=20 with common random numbers. Ranking by marginal spread times a leverage or competitiveness factor voids the clean guarantee ([[bharathi-nd-competitive-influence-maximization]], [[golovin-2011-adaptive-submodularity-theory]]).
- [[almazaydeh-2026-heuristic-centrality-methods]] motivates `mc_stability_check.py`. The recorded MEA-7 pass predates QME fix 3 (2026-10-01): before that fix, the splitmix keys ignored SEED, so the "different RNG seed" leg reused the same draws. **The cross-seed stability check should be re-run.** The MC=100 leg still stands.
- Suggested ablation only: run PageRank, degree and Katz-Bonacich seed baselines through the same CRN cascade. Any centrality rank used there is a heuristic model output, never closeness or access (D-025).

**Per-topic coalition allocator** — [[systems/melange-allocation]].
- It water-fills money in proportion to single-seed marginals and ignores overlap between seeds. This is the failure [[trivedi-2020-efficient-influence-maximization]] (degree-discount) and [[kumar-2022-influence-maximization-social]] warn about.
- No greedy runs, so no guarantee applies.

**Competitive cascade and scenario runner (`scenario.py`)** — [[systems/melange-influence-engine]].
- It cites [[bharathi-nd-competitive-influence-maximization]]. Three departures from the paper:
  1. Synchronous hops process A first, so pro-CU wins every same-hop tie. The paper breaks ties randomly.
  2. Electoral mode removes opponent seeds, which Lemma 1 does not cover.
  3. Industry is a fixed opponent that never responds. Once it does respond, only line graphs are solved.

**Seed gate (SGATE)** — [[systems/melange-influence-engine]].
- [[chen-2021-contingency-aware-influence]] is its method source, used non-adaptively.
- The 0.60 baseline in seed_prob equals the paper's arbitrary simulation default; it is not validated.
- The planned H1 test correlates seed_prob with *unconditional* new cosponsorships. That is not the conditional-on-invitation quantity the paper models, so H1 as designed cannot validate q.
- The adaptive step (seed, observe take-up, replan) is the unbuilt part of Paul's tier 2. If built, note that the myopic-feedback case is the one [[golovin-2011-adaptive-submodularity-theory]] retracted.

**Flip model (pflip = a/(a+b))** — [[systems/melange-influence-engine]].
- Its functional form matches Borodin's weight-proportional split, but it has no random thresholds or propagation.
- It has no time term. [[cai-2025-competitive-influence-maximization]] is the clearest case that timing and reversibility change outcomes under a fixed budget. Use it as a design critique and hypothesis source, not as calibration.

**Contested bills and the shadowing layer** — [[systems/melange-lobbying]], [[systems/mentat-fec-attribution]].
- [[moreno-2021-shadowing-shielding-effective]] is cited for a counterparty term. The w_b opponent-share field exists, but no engine reads it.
- Money acts only on the funded member's own pflip, so shielding cannot emerge.
- If built, opponent spend must come from money data. LDA may identify the opponent but supplies no edge or weight (D-021).

**Cosponsor route and closedCascadeRoute** — [[systems/melange-pathfinding]].
- [[arya-2025-relink-edge-activation]] supplies only the framing: edges as actions, with a per-node out-degree budget.
- The builds are hand-tuned greedy heuristics on a dense 533-node graph, the regime where RELINK's edge is smallest.
- The paper supports the non-submodular disclaimer, but gives no support for K=5, fan-out 2 or the kernel probabilities.

**LT priming** — [[systems/melange-influence-engine]].
- [[ma-2022-adversarial-attack-graph]] Prop 2: deterministic tier thresholds are not concave CDFs, so greedy claims built on them are uncertified.
- [[chen-2023-complex-contagion-influence]] supplies the test to run before any method swap: do CU-bill join hazards jump with the count of already-joined close partners? This is Paul's research question (a). Until it is tested, read greedy guarantees as properties of the model kernel only.

**Staffer graph** — [[systems/melange-staffer-network]].
- [[kamarthi-2020-influence-maximization-unknown]] fits only as a way to order which directories or people to research next.
- It is never a reading of access or influence. Influence language is barred on revolving-door edges.

**Flags under the two rejected SNA proposals** — [[systems/melange-sna]].
- *Per-edge volume normalization (rejected):* [[trivedi-2020-efficient-influence-maximization]]'s MICM replaces observed edge probabilities with a structural formula, (d_i+d_j)/n + CN/n. **FLAG:** this sits next to that rejected proposal, and the paper gives no evidence MICM is realistic. Do not adopt it for the kernel, which uses weighted cosponsorship with saturation.
- *Dropping regular cosponsorship edges (rejected):* RELINK's edge *activation* is a per-campaign decision about which existing edges carry an ask. It is not a graph-construction rule and gives no licence to prune cosponsorship edges.

**Overall.** For a fixed, fully observed 533-member chamber, the RL runtime argument ([[chen-2021-contingency-aware-influence]], [[kumar-2022-influence-maximization-social]]) does not apply. RL is justified only if coalition formation proves threshold-like, or once real outcomes are observed across campaigns.

## Gaps

**Anchors named in the addendum but missing from this theme set.** Leskovec et al. 2007 (CELF), Khalil et al. 2017 (S2V-DQN), Manchanda et al. (GCOMB, the budget-constraint analogue Paul maps to the political-capital budget), Ng-Harada-Russell 1999 (reward shaping, which licenses marginal-influence shaping into the outer MDP), Sutton-Barto, Watkins (Q-learning), Kempe-Kleinberg-Tardos 2003 and Nemhauser-Wolsey-Fisher 1978. That leaves the Khalil -> GCOMB -> Kamarthi -> Chen 2021 -> Chen 2023 progression with two of its five links absent. The frontier work MaDGNN (Scientific Reports 2026) is named without authors or DOI and is unanchored.

**Unsupported or loose claims in the addendum.**
- "RL4CCIM confirms" cross-campaign non-submodularity: it shows single-campaign non-submodularity under complex contagion only.
- The "four anchored entries are sufficient" sentence is inconsistent with the seven RL-IM entries actually anchored.
- "Golovin-Krause was first to study adaptive IM" is an unverified priority claim.
- Kamarthi's slug year (2019) differs from its published venue (AAMAS 2020).

**Research questions the set does not answer.**
- (a) Whether congressional coalition formation is a complex contagion. No paper uses legislative join data, and the join-hazard test against the count of already-joined partners has not been run.
- (b) Learning persuasion or relationship costs by probing. [[kamarthi-2020-influence-maximization-unknown]] probes topology, not costs.
- (c) Real take-up rates. [[chen-2021-contingency-aware-influence]] uses an arbitrary q; no work estimates it empirically.
- (d) Temporal credit assignment across campaigns, meaning which early investments deserve credit when a vote flips. No paper touches it.

**Missing method families.**
- Adaptive influence maximization with a responding opponent: adaptive and competitive at once.
- Robust influence maximization under uncertainty in kernel parameters.
- RIS/IMM theory papers (they appear only as benchmarks in [[almazaydeh-2026-heuristic-centrality-methods]]).
- Signed (trust-aware) influence maximization beyond the taxonomy in [[zareie-2023-influence-maximization-social]].
- Topic-specific kernels: the system uses one topic-independent kernel across 18 CU categories.
- Non-progressive cascade models that tie timing to legislative calendars.

**Empirical gap.** No work validates a diffusion kernel against observed political adoption ([[systems/melange-validation]]). All numbers are simulated outputs on synthetic or public non-political graphs, so none of them calibrates or refutes a Melange constant (K, fan-out, seed_prob 0.60, kernel saturation, leverage factor).

**Internal follow-ups this synthesis surfaces.**
- Re-run the cross-seed MC stability leg after QME fix 3.
- Correct the budgetRecruit 1-1/e claim (about 0.28 with a target set).
- Make tie-breaking in the competitive cascade random rather than A-first, or document it.
- Redesign the H1 seed_prob test around conditional-on-invitation take-up.

## All works

- [[kumar-2022-influence-maximization-social]] (2022) — Influence maximization in social networks using graph embedding and graph neural network
- [[trivedi-2020-efficient-influence-maximization]] (2020) — Efficient Influence Maximization in Social-Networks Under Independent Cascade Model
- [[golovin-2011-adaptive-submodularity-theory]] (2011) — Adaptive Submodularity: Theory and Applications in Active Learning and Stochastic Optimization
- [[ma-2022-adversarial-attack-graph]] (2022) — Adversarial Attack on Graph Neural Networks as An Influence Maximization Problem
- [[arya-2025-relink-edge-activation]] (2025) — RELINK: Edge Activation for Closed Network Influence Maximization via Deep Reinforcement Learning
- [[golovin-2011-adaptive-submodularity-theory]] (2011) — Adaptive Submodularity: A New Approach to Active Learning and Stochastic Optimization
- [[chen-2021-contingency-aware-influence]] (2021) — Contingency-Aware Influence Maximization: A Reinforcement Learning Approach
- [[borodin-nd-threshold-models-competitive]] (n.d.) — Threshold Models for Competitive Influence in Social Networks
- [[cai-2025-competitive-influence-maximization]] (2025) — Competitive Influence Maximization in Voter Dynamics: The Role of Timing and Network Heterogeneity
- [[moreno-2021-shadowing-shielding-effective]] (2021) — Shadowing and shielding: Effective heuristics for continuous influence maximisation in the voting dynamics
- [[zareie-2023-influence-maximization-social]] (2023) — Influence maximization in social networks: a survey of behaviour-aware methods
- [[almazaydeh-2026-heuristic-centrality-methods]] (2026) — Heuristic centrality methods challenge greedy optimization in influence maximization
- [[bharathi-nd-competitive-influence-maximization]] (n.d.) — Competitive Influence Maximization in Social Networks
- [[chen-2023-complex-contagion-influence]] (n.d.) — Complex Contagion Influence Maximization: A Reinforcement Learning Approach
- [[kamarthi-2020-influence-maximization-unknown]] (2019) — Influence maximization in unknown social networks: Learning Policies for Effective Graph Sampling
