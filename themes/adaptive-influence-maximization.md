---
type: theme
name: "Adaptive Influence Maximization"
paper_count: 16
tags: [theme, type/theme]
---

# Adaptive Influence Maximization

This theme covers 16 works that take influence maximization (IM) past the one-shot static problem. They add sequential and adaptive seeding, learned (RL/GNN) seeding policies, and competitive or temporal settings where the classic greedy guarantee may or may not survive. Paul's two addendum notes organize it in three tiers that he maps onto Melange's architecture:

1. **One-shot greedy.** The problem is static and submodular, and greedy gets (1-1/e), about 63% of optimal ([[kempe-2015-maximizing-spread-influence]]).
2. **Adaptive greedy.** This tier replans within a campaign as outcomes are observed, and adaptive submodularity carries the same factor against the best *policy* ([[golovin-2011-adaptive-submodularity-theory]]).
3. **The RL envelope.** A learned policy handles cross-campaign, non-submodular compounding, where no greedy guarantee reaches ([[chen-2021-contingency-aware-influence]], [[chen-2023-complex-contagion-influence]], [[kamarthi-2020-influence-maximization-unknown]], [[arya-2025-relink-edge-activation]], grounded in [[sutton-2018-reinforcement-learning-introduction]]).

The works agree more than the three tiers suggest, and the synthesis is mostly about where each guarantee stops. Classic greedy is provably good only under the following conditions:
- monotone submodular spread: IC or uniform-threshold LT ([[kempe-2015-maximizing-spread-influence]]), a concave threshold CDF ([[ma-2022-adversarial-attack-graph]]), or the last mover against a fixed rival in competitive IC ([[bharathi-nd-competitive-influence-maximization]]);
- marginals estimated well enough;
- in the adaptive case, a known prior and the right feedback model.

The guarantee breaks under complex contagion ([[chen-2023-complex-contagion-influence]]), most competitive threshold variants ([[borodin-nd-threshold-models-competitive]]), edge-activation routing ([[arya-2025-relink-edge-activation]]) and noisy, under-sampled Monte Carlo ([[almazaydeh-2026-heuristic-centrality-methods]]).

RL earns its keep in two ways:
- **Speed:** it runs fast on unseen graphs. In the submodular regime it only *matches* greedy ([[chen-2021-contingency-aware-influence]]).
- **Quality:** a real quality advantage is shown only in non-submodular regimes ([[chen-2023-complex-contagion-influence]]).

Voter-model work ([[moreno-2021-shadowing-shielding-effective]], [[cai-2025-competitive-influence-maximization]]) adds two further points. Continuous allocation is spread out rather than hub-concentrated, and timing and reversibility change who wins under a fixed budget.

**No work in the theme uses observed legislative, lobbying or persuasion outcomes.** Every spread figure is a simulated model output, under assumed edge probabilities, on co-authorship, social, village or synthetic graphs. For Melange the theme mostly sets the scope of guarantees the code already claims. It does not calibrate any constant.

**Online scan (2026-10-10).** A light web check turned up three things:
- **Retracted myopic-feedback result has since been settled.** The myopic-feedback IM result in Golovin-Krause, which they retracted, was settled by Peng and Chen (NeurIPS 2019, arXiv 1905.11663). Under myopic feedback, IC is *not* adaptive submodular. Adaptive greedy is guaranteed only at least (1-1/e)/4 of the optimal adaptive policy, and the adaptivity gap is between e/(e-1) and 4.
- **Batch-adaptive algorithms with guarantees exist.** Han et al., "Efficient Algorithms for Adaptive Influence Maximization" (PVLDB 11(9), 2018), give practical batch-adaptive algorithms with guarantees (AdaptGreedy/EPIC).
- **MaDGNN could not be found.** No Scientific Reports paper by that name turned up, so the frontier citation remains unanchored.

## Strands

### Static greedy and its cost-quality trade-off

**The foundation.** [[kempe-2015-maximizing-spread-influence]] proves that expected spread is monotone submodular for every triggering-set model, including IC and LT with uniform thresholds. Greedy therefore reaches (1-1/e-eps) of the optimal k-seed set. The guarantee has hard edges:
- General threshold models are inapproximable within n^(1-eps).
- Competing cascades are left open.
- Edge parameters are set by rule, not estimated.

On one 10,748-node hep-th graph (10,000 simulations per evaluation, k up to 30), greedy beats high-degree seeds by about 18% and distance centrality by over 40% under LT.

**The cost question.** The other three works ask whether cheap rankings can stand in for greedy.
- [[trivedi-2020-efficient-influence-maximization]] (degree-discount DHICM) finishes 12.5% below CELF at k=10 and 7.9% below at k=30 on NetHept. The number of Monte Carlo (MC) runs is not reported and there are no error bars.
- [[almazaydeh-2026-heuristic-centrality-methods]] puts PageRank and Betweenness at about 96.5% of IMM spread on a 1,000-node Facebook subgraph (IC p=0.1, k=10, 10 runs × 50 MC). CELF++ collapses to 12.9% of IMM, which the authors untestedly blame on 50-MC marginals and a Python implementation.
- [[kumar-2022-influence-maximization-social]] trains a GNN regressor on struc2vec embeddings to predict each node's standalone spread, then takes the top-k. All of its performance claims are from a preview only and are UNVERIFIED.

**Where they agree.** Exact lazy greedy is the quality reference, and its practical weak point is Monte Carlo noise in the marginals, not the algorithm.

**Where they disagree.** They differ on how close heuristics come: 18–40% behind (Kempe, LT), 8–12% behind (Trivedi, IC p=0.01), or within 3.5% (Almazaydeh, IC p=0.1 on dense ego subgraphs).

**The lesson Paul's notes draw.** Rankings by standalone spread (Kumar's top-k, singleton marginals) ignore overlap between seeds. That is exactly the failure Kempe's experiments penalize and degree-discount exists to fix. At Melange's scale (533 nodes, a 45-member stance-gated shortlist, a 60-seed cap), the cost argument for heuristics or learned rankers mostly does not apply.

Works: [[kempe-2015-maximizing-spread-influence]], [[trivedi-2020-efficient-influence-maximization]], [[almazaydeh-2026-heuristic-centrality-methods]], [[kumar-2022-influence-maximization-social]]

### Adaptive submodularity: replanning under observation

**The bridge.** [[golovin-2011-adaptive-submodularity-theory]] is Paul's Tier-2 bridge. If the conditional expected marginal gain never rises as more outcomes are observed, adaptive greedy gets (1-1/e) of the best adaptive policy. That requires a known prior, and sampled marginals carry only additive error. The paper also gives the following:
- squared-log min-cost cover bounds (corrected from an earlier, flawed log claim);
- a 4α min-sum bound;
- data-dependent upper bounds;
- a lazy variant, 30–38x faster on 357 sensors;
- Theorem 26: without adaptive submodularity, no polynomial approximation within O(|E|^(1-eps)) exists.

**Correction to Paul's note.** The IM application under myopic feedback was retracted. Peng and Chen (NeurIPS 2019) later showed myopic-feedback IC is not adaptive submodular and adaptive greedy gets only at least (1-1/e)/4. Paul's Tier-2 claim, "adaptive greedy gives 63% of the optimal policy", holds for IM only under full-adoption feedback. That is a real scope error in the addendum, not a style point.

**Two extensions of the observation model.**
- [[chen-2021-contingency-aware-influence]] is about willingness, not spread. An invited seed accepts with a known, uniform q, revealed at the end of each of T rounds. The paper explicitly contrasts itself with adaptive IM, which it says does not handle willingness uncertainty.
- [[kamarthi-2020-influence-maximization-unknown]] is about the graph. Edges are hidden and must be discovered by querying 5 nodes before greedy seeds 10.

**Shared assumption.** All three need the stochastic model to be known or learnable from many episodes. None supplies a real-world prior.

**Bearing on Melange (Paul's notes).** Melange's joint optimizer correctly says it is not an adaptive policy (observations=[]). The adaptive half of this strand is unbuilt, because no Melange system observes real outcomes.

Works: [[golovin-2011-adaptive-submodularity-theory]], [[chen-2021-contingency-aware-influence]], [[kamarthi-2020-influence-maximization-unknown]]

### The RL envelope: learned seeding policies

**Foundation.** [[sutton-2018-reinforcement-learning-introduction]] supplies the vocabulary: MDPs, DQN-family function approximation, the deadly triad and off-policy evaluation. The IM papers built on it share one recipe: a graph encoder (S2V, GAT, GCN+DiffPool, Node2Vec features), then a DQN or Double-DQN, plus a reward-shaping trick to make training tractable.

**The lineage.**
- [[chen-2021-contingency-aware-influence]] (RL4IM) uses a surrogate reward to cut computation from O(2^b) to O(1). Its gap bound becomes arbitrarily loose as q approaches 0 or 1.
- [[chen-2023-complex-contagion-influence]] (RL4CCIM) builds directly on RL4IM, adding solution filtering, reward-prioritised replay, a global marginal reward and a GAT encoder.
- [[kamarthi-2020-influence-maximization-unknown]] learns a node-query policy.
- [[arya-2025-relink-edge-activation]] learns edge activation under a per-node out-degree budget.

**What each claims.**
- **RL4IM:** parity with the CHANGE greedy (200-node synthetic graphs, q=0.6, budget 8). The case for RL is runtime on unseen graphs, where CHANGE takes about 10 hours at 20 seeds on 1,000 nodes.
- **RL4CCIM:** best normalized influence on all nine small networks, deterministic cascades with p0=1, p1=0 (some ties). Greedy collapses on India (0.0396 vs 0.5347). RL4CCIM matches the MIP optimum on 2 networks and comes within about 6–8% on 3 others.
- **Kamarthi:** 7.91–26.6% of the CHANGE-to-optimal gap closed, best variant per family. The text alternates between 7–23 and 7–26.
- **RELINK:** beats PSNA in 456 of 480 configurations, with the smallest edge at large k and on dense graphs.

**The pattern.** Learned policies win on quality only where greedy's premise fails: non-submodular objectives, or hidden information. Elsewhere their advantage is test-time speed.

**Learned ranking without sequence.** [[kumar-2022-influence-maximization-social]] is the non-sequential cousin: a learned ranker, not a policy.

**Feasibility limit.** Sutton-Barto sharpens a constraint on Paul's Tier-3 'moat'. The book's successes needed about 3×10^5 to 5×10^6 episodes, while a real CU campaign yields roughly one episode per two-year cycle. Any learned policy would therefore be trained inside the same unvalidated simulator. Off-policy evaluation from logged contacts and funding (Sec. 16.7) is the more transferable idea.

Works: [[sutton-2018-reinforcement-learning-introduction]], [[chen-2021-contingency-aware-influence]], [[chen-2023-complex-contagion-influence]], [[kamarthi-2020-influence-maximization-unknown]], [[arya-2025-relink-edge-activation]], [[kumar-2022-influence-maximization-social]]

### Competition and the limits of submodularity

**Positive results.** This strand marks where greedy guarantees survive once there is a rival or a threshold.
- [[bharathi-nd-competitive-influence-maximization]]: under competitive IC with exponential delays and random tie-breaking, the *last* mover's reach is monotone submodular in its own seeds. Greedy gets (1-1/e) of the best response, and the price of competition is at most 2. Pure Nash equilibria need not exist, and the first-mover problem is solved only on disjoint lines.
- [[ma-2022-adversarial-attack-graph]] gives the precise single-campaign condition for threshold models: submodularity holds iff the threshold CDF is concave on the positive domain (Proposition 2). Saturating-coverage objectives beat raw centrality.

**Negative results.**
- [[borodin-nd-threshold-models-competitive]] shows the competitive LT extensions mostly lose the guarantee. The weight-proportional model is not even monotone: one seed lowers A's root probability from 11/40 to 103/400. The separated-threshold model is monotone but not submodular, and the follower problem is NP-hard to approximate within Ω(N^(1/2-eps)). Only the OR model keeps the guarantee.
- [[chen-2023-complex-contagion-influence]] proves K-complex contagion IM non-submodular and NP-hard.
- [[kempe-2015-maximizing-spread-influence]] had already flagged general thresholds as inapproximable.

**What the works agree on.** The diffusion model, not the algorithm, decides whether greedy is principled.

**Where Melange sits (Paul's notes, leads not findings).** Melange's two-colour cascade cites Bharathi but departs from it in three ways:
- it uses synchronous hops with A winning every tie;
- the frontier greedy scales marginals by leverage or competitiveness factors;
- the coalition allocator water-fills on singleton marginals.

Melange's flip term pflip = a/(a+b) has Borodin's weight-proportional form, but it has no propagation.

Works: [[bharathi-nd-competitive-influence-maximization]], [[borodin-nd-threshold-models-competitive]], [[ma-2022-adversarial-attack-graph]], [[chen-2023-complex-contagion-influence]], [[kempe-2015-maximizing-spread-influence]]

### Continuous and temporal competition in voter dynamics

Both papers come from the Brede group, use non-progressive voter models with a budgeted active controller, and use no political, lobbying or money data.

**Allocation in space.** [[moreno-2021-shadowing-shielding-effective]] studies one 1,133-node email network against a passive, known opponent.
- The continuous optimum is spread out: about 111 of 1,133 nodes get above-average allocation, with std/mean 0.11 elsewhere.
- It beats the best discrete plan by 5–10% when the opponent targets fewer than 100 nodes.
- Hub targeting explains discrete optima, while shadowing (spend where the opponent spends) and shielding (spend on the opponent's targets' neighbours) explain continuous ones.
- Continuous shadowing plus shielding gets within 1.89e-3 of optimal vote share, against 2.09e-2 for degree targeting.
- A richer controller shadows the opponent and a poorer one avoids its nodes. The two-sided Nash equilibrium is uniform allocation.

**Allocation in time.** [[cai-2025-competitive-influence-maximization]] adds the time dimension that Moreno explicitly lists as a limitation.
- Against a fixed opponent, the best strategy is to hold budget back and spend late.
- In Table 2, an optimized start beats t=0 in every dataset, horizon and budget ratio. On the 'Congress votes' mentions graph at T=16 with equal budgets, vote share is 0.705 vs 0.500.
- Richer controllers start earlier, and high-degree nodes relax more slowly.
- In the two-player game, the richer side moves first and the second mover can only offset its lead, not overtake it.

**Bearing on Melange.** This strand is the most direct design critique of Melange's money layer. It is a stationary, single-node a/(a+b) form against fixed industry money, with no spread to neighbours and no time term. Moreno supports the concept of an opponent-response ('shadowing') term. Cai shows timing and reversibility change outcomes. Neither calibrates anything.

Works: [[moreno-2021-shadowing-shielding-effective]], [[cai-2025-competitive-influence-maximization]]

### Behaviour-aware framing

[[zareie-2023-influence-maximization-social]] is a data-free narrative survey. It splits IM into behaviour-agnostic and behaviour-aware methods, and the behaviour-aware side into four families: interest-aware, opinion-aware, money-aware and physical-world-aware.

**Judgments useful against the rest of the theme:**
- Topic-aware edge probabilities are the most realistic but need rich history.
- Trust-agnostic (all-positive-tie) models are unrealistic.
- Profit-aware IM needs per-user valuations that are rarely available, so cost-aware IM is the more practical option.
- Changing the diffusion model can change accuracy substantially.

**Its place in the theme.** It is the only work that raises silent users and adaptive diffusion as open problems. It also places every other paper here: all of them are behaviour-agnostic structural IM except the contingency-aware work, which adds a uniform take-up q.

**Bearing on Melange (Paul's notes).** Melange's single topic-independent kernel across 18 CU categories is trust-agnostic: edge magnitude varies, never sign. Its optimizer is profit-aware in shape (owner-set PERS, IMP weights), which is exactly the data the survey says is usually missing.

Works: [[zareie-2023-influence-maximization-social]]

## Agreements and debates

**1. Greedy vs. cheap heuristics: how big is the gap?**
- [[kempe-2015-maximizing-spread-influence]] (greedy +18% over degree, +40% over distance under LT) and [[ma-2022-adversarial-attack-graph]] (degree and betweenness are the weakest selectors) find greedy-type selection clearly better.
- [[trivedi-2020-efficient-influence-maximization]] finds degree-discount 8–12% short of CELF.
- [[almazaydeh-2026-heuristic-centrality-methods]] finds PageRank at 96.5% of IMM and CELF++ at 12.9%.

These are not direct contradictions. The results differ by diffusion parameter (p=0.01 vs 0.1), graph density, objective, and especially Monte Carlo budget. Almazaydeh's CELF++ collapse is most plausibly under-sampled marginals; the authors offer that explanation but never test it. What they share is that the estimation regime governs whether a guarantee shows up in practice. Almazaydeh also has internal arithmetic errors: 96.3% stated vs 96.5% computed, 794x vs 809x, 11.2% vs 12.9%.

**2. Greedy vs. RL: speed or quality?**
- [[chen-2021-contingency-aware-influence]] reaches only parity with greedy. Its argument for RL is runtime.
- [[chen-2023-complex-contagion-influence]] shows greedy collapsing (India 0.0396 vs 0.5347) once spread is non-submodular.
- [[arya-2025-relink-edge-activation]] and [[kamarthi-2020-influence-maximization-unknown]] beat heuristic baselines, not greedy optima. RELINK's margin is narrowest at large k and on dense graphs.

Read together: RL beats greedy on quality only where greedy's premise fails. Paul's claim that RL4CCIM "confirms" the cross-campaign problem is non-submodular overstates it. RL4CCIM proves non-submodularity for a stylized K-complex model on 9 small simulated graphs. Whether congressional coalition formation follows that model is the untested hypothesis (a).

**3. Does competition preserve submodularity?**
- Yes for the last mover in competitive IC ([[bharathi-nd-competitive-influence-maximization]]).
- Mostly no for competitive LT ([[borodin-nd-threshold-models-competitive]]).

The results hinge on the diffusion model and on whether the rival is fixed. Both papers treat the opponent as fixed for the guarantee. Bharathi shows equilibrium reasoning is only tractable on lines.

**4. Concentrate or spread?** Kempe-style discrete seeding favours selected nodes. [[moreno-2021-shadowing-shielding-effective]] shows the continuous optimum is diffuse and opponent-shaped, and [[cai-2025-competitive-influence-maximization]] says to spend late. Discrete hub logic is an artefact of the fixed-gain, single-shot regime.

**5. Is adaptivity worth much?** [[golovin-2011-adaptive-submodularity-theory]] gives a policy-level guarantee. Peng and Chen (2019, outside the vault) bound the adaptive advantage in myopic IC to at most a factor of 4. They also show adaptive greedy's guarantee drops to at least (1-1/e)/4 there. Tier 2's value depends on what is observed, and the addendum does not specify that.

**6. Can Tier 3 be trained at all?** [[sutton-2018-reinforcement-learning-introduction]]'s case studies need 10^5–10^6 or more episodes. A real campaign yields about one per cycle, so a learned outer policy would learn the simulator, not Congress.

**Flagged errors (actual, not stylistic):**
- **Chen 2021 author list.** The hub citation for [[chen-2021-contingency-aware-influence]] gives the authors as Chen, Kamarthi, Wilder, Tambe. The paper is by Haipeng Chen, Wei Qiu, Han-Ching Ou, Bo An and Milind Tambe (UAI 2021, PMLR 161:1535-1545; arXiv 2106.07039).
- **Tier-2 63% claim.** It is stated without the full-adoption-feedback condition.
- **"Four entries sufficient" sentence.** It contradicts the seven RL-IM entries actually listed.
- **Kamarthi's range.** It is reported internally as both 7–23% and 7–26%.

## Methods

**Dominant methods:**
1. **Formal proofs.** Submodularity and monotonicity via live-edge or triggering-set coupling, hardness reductions, and approximation and price-of-competition bounds: [[kempe-2015-maximizing-spread-influence]], [[golovin-2011-adaptive-submodularity-theory]], [[bharathi-nd-competitive-influence-maximization]], [[borodin-nd-threshold-models-competitive]], and the theory sections of [[ma-2022-adversarial-attack-graph]] and [[chen-2023-complex-contagion-influence]].
2. **Monte Carlo IC/LT simulation benchmarks** on public or synthetic graphs: [[trivedi-2020-efficient-influence-maximization]], [[almazaydeh-2026-heuristic-centrality-methods]], [[kumar-2022-influence-maximization-social]].
3. **Deep RL (DQN or Double-DQN) with graph encoders**, trained on many small graphs and tested on held-out ones: [[chen-2021-contingency-aware-influence]], [[chen-2023-complex-contagion-influence]], [[kamarthi-2020-influence-maximization-unknown]], [[arya-2025-relink-edge-activation]].
4. **Mean-field, gradient and hill-climbing optimization in voter models**: [[moreno-2021-shadowing-shielding-effective]], [[cai-2025-competitive-influence-maximization]].

**Shared weaknesses:**
- **No ground truth.** No work observes real diffusion, adoption or persuasion. Edge probabilities are fixed by rule: p=0.01 or 0.1, uniform 0.5, structural formulas like MICM, or random-walk powers. Take-up q is an arbitrary simulation default (0.6 in Chen 2021, which Melange's seed_prob baseline mirrors).
- **Thin Monte Carlo reporting.** Trivedi does not report R. Almazaydeh uses 50 per marginal. Error bars are often absent.
- **Small or single graphs.** RL4IM uses 200-node synthetic graphs, RL4CCIM 9 small networks with deterministic cascades, Moreno one email network, Kempe one co-authorship graph.
- **Selective reporting.** Kamarthi reports the best variant per test set without confidence intervals.
- **Internal numeric inconsistencies** in Almazaydeh, Kamarthi and Chen 2023 (24.3% vs 24.2%).
- **RL baselines are often heuristics, not greedy optima.** RELINK and Kamarthi compare against heuristics, and Chen 2023 reimplements DPIM itself.
- **Opponents are fixed or passive** in every competitive paper except the Moreno and Cai equilibrium sections.
- **Static, fully known graphs** everywhere except Kamarthi.

**Strength:** the theory results are exact. They tell you precisely when a guarantee applies, which is the theme's main transferable value.

## How the works relate

- [[golovin-2011-adaptive-submodularity-theory]] — extends — [[kempe-2015-maximizing-spread-influence]]: Generalizes the submodular greedy guarantee for IM to adaptive policies; reduces to the classical notion for deterministic priors.
- [[trivedi-2020-efficient-influence-maximization]] — builds-on — [[kempe-2015-maximizing-spread-influence]]: Speeds up Kempe-style greedy under IC with a degree-discount heuristic benchmarked against CELF.
- [[bharathi-nd-competitive-influence-maximization]] — extends — [[kempe-2015-maximizing-spread-influence]]: Extends IC submodularity to a follower facing a fixed competing cascade, a case Kempe leaves open.
- [[borodin-nd-threshold-models-competitive]] — extends — [[kempe-2015-maximizing-spread-influence]]: Extends the Kempe-Kleinberg-Tardos LT model to two technologies and shows the greedy guarantee mostly fails.
- [[borodin-nd-threshold-models-competitive]] — cites — [[bharathi-nd-competitive-influence-maximization]]: Borodin cites Bharathi-Kempe-Salek among greedy approaches to competitive extensions; the IC follower result and the LT negative results bracket the guarantee.
- [[chen-2023-complex-contagion-influence]] — builds-on — [[chen-2021-contingency-aware-influence]]: RL4CCIM uses RL4IM as base architecture and baseline (same Tambe group).
- [[chen-2023-complex-contagion-influence]] — complements — [[kempe-2015-maximizing-spread-influence]]: Leaves the triggering-set regime Kempe proves submodular; a different model, not a contradiction of Kempe's theorems.
- [[chen-2023-complex-contagion-influence]] — complements — [[golovin-2011-adaptive-submodularity-theory]]: Shows the submodularity premise behind G-K bounds fails under complex contagion.
- [[chen-2021-contingency-aware-influence]] — complements — [[golovin-2011-adaptive-submodularity-theory]]: Explicitly contrasts with adaptive IM, which it says does not model seed willingness uncertainty.
- [[chen-2021-contingency-aware-influence]] — extends — [[kempe-2015-maximizing-spread-influence]]: Adds a probabilistic seed take-up step that Kempe's model lacks.
- [[kamarthi-2020-influence-maximization-unknown]] — builds-on — [[kempe-2015-maximizing-spread-influence]]: Relaxes the known-graph assumption; uses greedy IC IM as its influence oracle.
- [[kamarthi-2020-influence-maximization-unknown]] — complements — [[chen-2021-contingency-aware-influence]]: Same Tambe-group RL-for-IM program: one handles uncertain graph structure, the other uncertain seed willingness.
- [[arya-2025-relink-edge-activation]] — same-method — [[chen-2023-complex-contagion-influence]]: Deep Q-learning for a non-submodular IM objective with no optimality guarantee.
- [[arya-2025-relink-edge-activation]] — complements — [[golovin-2011-adaptive-submodularity-theory]]: RELINK's edge-activation objective is non-submodular, so G-K-style bounds do not carry over.
- [[kumar-2022-influence-maximization-social]] — complements — [[trivedi-2020-efficient-influence-maximization]]: Both trade greedy's cost for a cheaper ranker under IC; one learned (GNN), one degree-discount.
- [[almazaydeh-2026-heuristic-centrality-methods]] — contradicts — [[kempe-2015-maximizing-spread-influence]]: Finds centrality heuristics within ~3.5% of IMM and CELF++ collapsing, whereas Kempe finds greedy clearly beats centrality; tension driven by different graphs, p and MC budgets.
- [[almazaydeh-2026-heuristic-centrality-methods]] — same-method — [[trivedi-2020-efficient-influence-maximization]]: Both are IC Monte Carlo benchmarks of heuristics versus greedy on spread and runtime.
- [[ma-2022-adversarial-attack-graph]] — contradicts — [[almazaydeh-2026-heuristic-centrality-methods]]: Ma finds degree, betweenness and PageRank weaker than greedy coverage selection; Almazaydeh finds PageRank near-optimal. Different objectives, so a tension rather than a head-on contradiction.
- [[ma-2022-adversarial-attack-graph]] — complements — [[borodin-nd-threshold-models-competitive]]: Ma gives the single-campaign threshold condition for submodularity (concave CDF); Borodin shows competitive threshold variants lose it.
- [[ma-2022-adversarial-attack-graph]] — complements — [[kumar-2022-influence-maximization-social]]: Opposite directions at the GNN-IM junction: IM to attack a GNN versus a GNN to solve IM.
- [[cai-2025-competitive-influence-maximization]] — extends — [[moreno-2021-shadowing-shielding-effective]]: Same Brede-group voter-model line; adds the timing dimension Moreno lists as a limitation.
- [[cai-2025-competitive-influence-maximization]] — complements — [[bharathi-nd-competitive-influence-maximization]]: Non-progressive voter-model competition with timing versus progressive competitive IC; Cai cites Bharathi only for budget constraints.
- [[moreno-2021-shadowing-shielding-effective]] — complements — [[borodin-nd-threshold-models-competitive]]: Heuristic responses (shadowing, shielding) for competitive settings where Borodin-type results remove greedy guarantees.
- [[sutton-2018-reinforcement-learning-introduction]] — complements — [[chen-2021-contingency-aware-influence]]: Supplies the MDP/DQN foundations RL4IM applies; no confirmed citation.
- [[sutton-2018-reinforcement-learning-introduction]] — complements — [[chen-2023-complex-contagion-influence]]: Same RL foundations (Double-DQN, reward shaping, replay).
- [[zareie-2023-influence-maximization-social]] — cites — [[kempe-2015-maximizing-spread-influence]]: Survey of behaviour-aware methods built on the Kempe diffusion models.
- [[zareie-2023-influence-maximization-social]] — complements — [[chen-2021-contingency-aware-influence]]: Paired in Melange code as 'Chen/SNAM' backing for contingency-aware seeding; the survey does not cite Chen.

## What it means for the systems

All bearings below come from Paul's per-work notes. Most are VERIFIED-SOFTENED or UNVERIFIED single-agent readings of the code. Under DE-062 they are leads, not findings. Spread, activation and pflip figures are model outputs under assumed parameters. They are not closeness, access, introduction or persuasion probabilities (D-025).

**Joint optimizer** (`fec/engine/optimize.py`; [[systems/melange-allocation]])
- **Best-supported part.** Lazy greedy over a separable concave objective in a fixed stance state gives an exact grid optimum, which is stronger than (1-1/e) ([[kempe-2015-maximizing-spread-influence]] supports it; [[golovin-2011-adaptive-submodularity-theory]] is its method source for the data-dependent bound).
- **It is not adaptive.** It correctly says so (observations=[]). Modeled threshold conversions void the certificate, and [[borodin-nd-threshold-models-competitive]] says why: the max-normalized neighbour term can make adding pro mass lower others' pflip.
- **No case for a learned replacement.** [[arya-2025-relink-edge-activation]] and [[sutton-2018-reinforcement-learning-introduction]] weaken the case for swapping it for a learned policy, because the greedy is already exact.
- **Design gaps.** [[moreno-2021-shadowing-shielding-effective]] and [[cai-2025-competitive-influence-maximization]] point to a missing opponent-response term and a missing time dimension. The w_b 'shadowing' field exists but no engine reads it. Per D-021, LDA cannot supply that opponent-placement weight; it would need money or position data such as per-bill interest-group positions (relevant to [[systems/mentat-fec-attribution]]).

**Budget frontier and per-topic coalition allocators** ([[systems/melange-allocation]], [[systems/melange-validation]])
- **Guarantees claimed but not supplied.** These claim or imply submodular guarantees that the literature does not supply, for several reasons:
  - only 20 Monte Carlo worlds (MC=20);
  - a two-colour race with A winning every same-hop tie, unlike [[bharathi-nd-competitive-influence-maximization]]'s random tie-breaking;
  - marginals scaled by leverage or competitiveness;
  - removal of opponent seeds in electoral mode;
  - the coalition allocator water-filling on singleton marginals, which is the overlap-blind pattern [[kempe-2015-maximizing-spread-influence]] and [[trivedi-2020-efficient-influence-maximization]] penalize.
- **Stale MC-stability evidence.** [[almazaydeh-2026-heuristic-centrality-methods]] motivates `mc_stability_check.py`, but the recorded cross-seed pass (MEA-7) predates the 2026-10-01 SEED-keying fix. The cross-seed leg therefore did not test independent draws and should be re-run.
- **Cheap ablation the system lacks.** Run a PageRank, degree or Katz-Bonacich seed baseline through the same CRN cascade.
- **Complex-contagion risk.** If cosponsor joining is threshold-like, these greedy paths miss jointly valuable seed sets and drop zero-solo-marginal candidates ([[chen-2023-complex-contagion-influence]]). Read every greedy guarantee as a property of the model kernel only until hypothesis (a) is tested.

**Seed gate (SGATE)** ([[systems/melange-influence-engine]])
- [[chen-2021-contingency-aware-influence]] is the method source. The gate goes beyond the paper: q varies by member, there is one round, and committed allies are ungated. The paper's theorems do not cover that model, and the system does not need them because it estimates spread by Monte Carlo.
- The 0.60 baseline equals the paper's arbitrary default and is not a measured take-up rate.
- The planned H1 test measures unconditional new cosponsorships, not take-up conditional on invitation.
- [[golovin-2011-adaptive-submodularity-theory]] Theorem 16's independent per-seed structure matches the gate.

**LT priming** ([[systems/melange-influence-engine]])
- Its deterministic tier thresholds fall outside [[kempe-2015-maximizing-spread-influence]]'s uniform-threshold condition and [[ma-2022-adversarial-attack-graph]]'s concave-CDF condition. Any greedy claim over it carries no guarantee.

**Cosponsor route and closedCascadeRoute** ([[systems/melange-pathfinding]])
- These borrow [[arya-2025-relink-edge-activation]]'s framing (edge as action, per-node out-degree budget) but are hand-tuned greedy heuristics.
- They sit in the dense-graph, large-k regime where RELINK's edge is smallest.
- The paper backs the code's own disclaimer that no (1-1/e) guarantee applies. It gives no support for K=5, fan-out 2 or the kernel probabilities.

**Cascade kernel** ([[systems/melange-influence-engine]])
- It is one topic-independent, trust-agnostic kernel ([[zareie-2023-influence-maximization-social]]).
- [[trivedi-2020-efficient-influence-maximization]]'s MICM is a structural edge-probability rule, not a calibration. It is not a per-edge volume normalization and must not be read as support for that REJECTED proposal.
- No work in this theme argues for dropping regular cosponsorship edges.

**Staffer network** ([[systems/melange-pathfinding]])
- [[kamarthi-2020-influence-maximization-unknown]] applies only to deciding which directories or people to research next on the partially observed staffer graph. It licenses no access or influence reading. The cosponsor graph is fully observed, so its learned policy does not pay there.

**Prospective contract** ([[systems/melange-validation]])
- [[sutton-2018-reinforcement-learning-introduction]] Sec. 16.7 suggests off-policy evaluation of allocation policies from logged contacts and funding, with propensities. The contract does not do this.

**Tier mapping overall**
- Paul's mapping of D* Lite/LPA* to adaptive submodularity, MCTS/orienteering to RL-IM, and the political-capital budget to GCOMB is architectural analogy, not a result any work here establishes.

## Gaps

**1. Hypothesis (a) is untested.** Whether coalition formation in Congress is a complex contagion is unsettled. The online scan found no study that directly estimates a cosponsorship adoption threshold. The nearest designs are:
- a threshold 'toppling' model of Congress (arXiv 1001.3732);
- Canen-Trebbi's structural cosponsorship networks;
- 'Diffusion in Congress' on discharge petitions (PSRM);
- Traag's complex contagion of campaign donations (arXiv 1601.07679), the clearest transferable test design.

The decisive in-house test is whether CU-bill join hazards jump with the count of already-joined partners, stratified by party and chamber. This is the single most important missing piece, because it decides whether Tier 1/2 guarantees apply at all.

**2. No real take-up or edge-probability estimates** for q, the seed_prob baseline, or the kernel probabilities. Hypothesis (c), probabilistic commitment, has no empirical anchor.

**3. Adaptive feedback model unspecified.**
- Tier 2 needs a stated feedback model.
- The Golovin-Krause myopic IM result was retracted, and Peng and Chen, "Adaptive Influence Maximization with Myopic Feedback" (NeurIPS 2019, arXiv 1905.11663), settle it at (1-1/e)/4 with an adaptivity gap of at most 4. Not in the vault; a strong candidate addition.
- Also missing: Han et al., "Efficient Algorithms for Adaptive Influence Maximization" (PVLDB 11(9), 2018), batch-adaptive AdaptGreedy/EPIC with guarantees; and Chen, Peng, Schoenebeck and Tao, "Adaptive Greedy versus Non-adaptive Greedy for IM" (JAIR).

**4. Anchored entries absent from the theme.** Paul's addendum anchors these, but they are not works in this theme file: Leskovec et al. 2007 (CELF), Khalil et al. 2017 (S2V-DQN), Manchanda et al. 2020 (GCOMB, the political-capital budget mapping), Ng-Harada-Russell 1999 (reward shaping, which licenses greedy-as-shaping-reward), Watkins (Q-learning) and Nemhauser-Wolsey-Fisher 1978. Hypothesis (b), cost learning by probing, rests on Kamarthi, which probes graph structure, not costs. Nothing here learns persuasion costs.

**5. MaDGNN is unanchored and could not be located** in Scientific Reports by the scan. Treat it as unverified until a title and authors are supplied.

**6. Adaptive opponents.** No adaptive or RL work handles an opponent that best-responds. Bharathi solves only lines, and Moreno and Cai give equilibria only in voter models. Melange treats industry money as fixed.

**7. Temporal credit assignment.** Which early investment deserves credit when a vote flips is named in Paul's notes, but no work addresses it on real data.

**8. Sample efficiency for Tier 3.** No work shows an RL-IM policy learning from on the order of 10 episodes. Off-policy evaluation from logs is the realistic substitute and is absent.

**9. Behaviour-aware and signed diffusion.** Signed (friend/foe) and topic-aware diffusion is flagged as an open area by [[zareie-2023-influence-maximization-social]] and is untreated in the theme.

**10. Download note.** No files were downloaded in this pass. The open-access copies identified by the scan (arXiv 1905.11663, 2106.07039, 1601.07679, 1001.3732) are candidates if Paul wants them pulled into the vault.

## All works

- [[kumar-2022-influence-maximization-social]] (2022) — Influence maximization in social networks using graph embedding and graph neural network
- [[trivedi-2020-efficient-influence-maximization]] (2020) — Efficient Influence Maximization in Social-Networks Under Independent Cascade Model
- [[golovin-2011-adaptive-submodularity-theory]] (2011) — Adaptive Submodularity: Theory and Applications in Active Learning and Stochastic Optimization
- [[ma-2022-adversarial-attack-graph]] (2022) — Adversarial Attack on Graph Neural Networks as An Influence Maximization Problem
- [[arya-2025-relink-edge-activation]] (2025) — RELINK: Edge Activation for Closed Network Influence Maximization via Deep Reinforcement Learning
- [[chen-2021-contingency-aware-influence]] (2021) — Contingency-Aware Influence Maximization: A Reinforcement Learning Approach
- [[borodin-nd-threshold-models-competitive]] () — Threshold Models for Competitive Influence in Social Networks
- [[cai-2025-competitive-influence-maximization]] (2025) — Competitive Influence Maximization in Voter Dynamics: The Role of Timing and Network Heterogeneity
- [[moreno-2021-shadowing-shielding-effective]] (2021) — Shadowing and shielding: Effective heuristics for continuous influence maximisation in the voting dynamics
- [[zareie-2023-influence-maximization-social]] (2023) — Influence maximization in social networks: a survey of behaviour-aware methods
- [[almazaydeh-2026-heuristic-centrality-methods]] (2026) — Heuristic centrality methods challenge greedy optimization in influence maximization
- [[bharathi-nd-competitive-influence-maximization]] () — Competitive Influence Maximization in Social Networks
- [[chen-2023-complex-contagion-influence]] (2023) — Complex Contagion Influence Maximization: A Reinforcement Learning Approach
- [[kamarthi-2020-influence-maximization-unknown]] (2020) — Influence maximization in unknown social networks: Learning Policies for Effective Graph Sampling
- [[sutton-2018-reinforcement-learning-introduction]] (2018) — Reinforcement Learning: An Introduction (2nd ed.)
- [[kempe-2015-maximizing-spread-influence]] (2015) — Maximizing the Spread of Influence through a Social Network (Theory of Computing extended version)
