---
type: paper
title: "Contingency-Aware Influence Maximization: A Reinforcement Learning Approach"
authors: ["Haipeng Chen", "Wei Qiu", "Han-Ching Ou", "Bo An", "Milind Tambe"]
year: "2021"
venue: "Accepted to UAI 2021 (37th Conference on Uncertainty in Artificial Intelligence); arXiv:2106.07039v1 [cs.SI], 13 Jun 2021. The metadata hint said \"arXiv preprint (v1)\"; the PDF states UAI 2021 acceptance, so the venue is corrected."
doi: "arXiv:2106.07039"
category: "Adaptive Influence Maximization"
systems: ["melange-allocation", "melange-influence-engine", "melange-validation"]
components: ["melange-influence-engine.seed-gate", "melange-allocation.budget-frontier", "melange-allocation.per-topic-allocators", "melange-validation.prospective-contract"]
paul_annotated: true
paul_markup_count: 35
evidence_status: verified
source: "[[_sources/chen-2021-contingency-aware-influence.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\chen21b.pdf"]
sha12: ["ef034dca27c3", "2116479414e8"]
tags: [paper, adaptive-influence-maximization]
---

# Contingency-Aware Influence Maximization: A Reinforcement Learning Approach

**Haipeng Chen, Wei Qiu, Han-Ching Ou, Bo An, Milind Tambe (2021).** *Accepted to UAI 2021 (37th Conference on Uncertainty in Artificial Intelligence); arXiv:2106.07039v1 [cs.SI], 13 Jun 2021. The metadata hint said "arXiv preprint (v1)"; the PDF states UAI 2021 acceptance, so the venue is corrected..* [arXiv:2106.07039](https://doi.org/arXiv:2106.07039) Theme: [[themes/adaptive-influence-maximization|Adaptive Influence Maximization]].

> [!abstract] Summary
> Chen, Qiu, Ou, An and Tambe (UAI 2021) study contingency-aware influence maximization. In this setting an invited seed accepts only with a probability q, which is known in advance and the same for every node. Seeds are chosen over T rounds of B seeds, and willingness is revealed only at the end of each round. The authors cast the problem as an MDP and train a DQN with structure2vec embeddings (RL4IM). Two devices make training tractable: a state abstraction (X1 + q*X3) and a surrogate reward, (1-q)*dI0 + q*dI_{b-1}. The surrogate cuts reward computation from O(2^b) to O(1), and Theorem 3 bounds its gap from the exact reward, though the bound becomes arbitrarily loose as q approaches 0 or 1. All experiments use synthetic Barabasi-Albert-type graphs: 200 nodes, Independent Cascade with p=0.1, q=0.6, budget 8, and 200 runs per setting. On these graphs RL4IM matches or slightly beats the CHANGE greedy baseline; in the q sweep (Q3) the results are roughly twice Random. The paper's case for RL is runtime on new, unseen networks: CHANGE takes about 10 hours at 20 seeds on a 1,000-node graph. It gives no empirical estimate of q for any real population.
> 
> For Melange the paper is the cited source of the SGATE contingency gate (fec/moneyball_core.py:626-630). The gate is used non-adaptively in the coalition and frontier spread estimators. The system goes beyond the paper's model in three ways: q varies by member (seed_prob x stance gate), there is a single round with no observed willingness, and committed allies are never gated. The paper's theorems do not cover that model, and the system does not need them, because it estimates expected spread directly by Monte Carlo. The paper does not validate any take-up rate. The 0.60 baseline in seed_prob equals the paper's simulation default. The planned H1 test correlates seed_prob with unconditional new cosponsorships, which is not the conditional-on-invitation quantity the paper models. The runtime argument for RL does not apply to a fixed 533-member chamber graph.

## Paul's annotation

From [[annotations/Adaptative - Influence Maximization - Melange Annotated Bibliography — Addendum|Adaptative - Influence Maximization - Melange Annotated Bibliography — Addendum]]:

> **Chen, H., Qiu, W., Ou, H.-C., An, B., & Tambe, M. (2021).** *Contingency-Aware Influence Maximization: A Reinforcement Learning Approach.* UAI '21 (PMLR). arXiv:2106.07039. https://arxiv.org/abs/2106.07039
>
> RL for IM when chosen seeds may decline to participate — sequential, observe-then-act seeding under uncertainty about whether any given activation actually "takes." The contingency framing maps onto legislators who may or may not move when approached, and onto lobbying outreach where commitment is probabilistic rather than guaranteed.

## Paul's markup in the PDF

- p1 Highlight: "problem aims at finding a subset of seed nodes in a social net work that maximize the spread of influence." — 
- p1 Highlight: "Such contingency aware IM is critical for applications for non-profit organi zations in low resource communities (e.g., spreading awareness of disease prevention)." — 
- p1 Highlight: "whenever there is a new social network, the non-profits usually do not have the HPCs to recalculate the solutions." — 
- p1 Highlight: "Markov Decision Process (MDP), and use RL to learn an IM policy over historically seen networks, and generalize to unseen networks with negligible runtime at test phase." — 
- p1 Highlight: "including state-abstraction and theoretically grounded reward shaping." — 
- p1 Highlight: "Originally derived from the viral marketing domain, the majority of IM algorithms" — 
- p1 Highlight: "focus on settings where nodes are always willing to be the seeds" — 
- p1 Highlight: "In this sub-class of IM problems called contingency-aware influence maximization [Yadav et al., 2018], when a node is invited to become a seed node, there is uncertainty in whether it is willing to accept the invitation." — 
- p1 Highlight: "whenever the underlying social network changes, the solution to the IM problem needs to be recomputed, whereas the stakeholders usually do not have the HPCs to perform the computation on their own" — 
- p1 Highlight: "on a network with 1000 nodes and 4974 edges when the number of seeds increases (run on a single Intel(R) Core(TM) i9 9820X CPU @ 3.30GHz core, with the sampling frequency of CHANGE set to 50, the influence propagation probability being set to 0.2). CHANGE runs 10 hours even with just 20 seed nodes," — 
- p2 Highlight: "The key idea is to decompose the selection of nodes into a sequence, and learn a heuristic policy that selects nodes sequentially." — 
- p2 Highlight: ", which is deterministic." — 
- p2 Highlight: ", the willingness status of nodes selected before the current step are unknown" — 
- p2 Highlight: "Though this formulation preserves the Markovian property, its state is highly sparse and thus makes it hard for RL to learn efficiently." — 
- p2 Highlight: "2) The influence function is submodular." — 
- p2 Highlight: ", we then design a surrogate reward function in place of the exact expected reward, with provable worst case guarantee compared to the exact expected reward." — 
- p2 Highlight: ", RL can perform as good as state-of-the-art greedy IM algorithms from the HIV prevention domain" — 
- p2 Highlight: "Our code can be found via https://github. com/Haipeng-Chen/RL4IM-Contingency" — 
- p2 Highlight: "which has a guarantee of 1 -1/e." — 
- p2 Highlight: "Adaptive IM differs from our setting in that our focus is to address the uncertainty in a node's willingness to participate, which they do not address." — 
- p3 Highlight: ". These approaches do not generalize well among unseen graphs, or are data inefficient." — 
- p3 Highlight: "use RL to do influence maximizaiton in a competitive setting." — 
- p3 Highlight: "It uses supervised learning as a preliminary step to predict the individual quality of a node," — 
- p3 Highlight: "We will show empirically that directly applying their methods leads to sub-optimal performance." — 
- p3 Highlight: "The trained youth will then act as peer-leaders who further spread the awareness of HIV prevention among the youth social network. A" — 
- p3 Highlight: "(V, E) where V and E are respectively the nodes and edges" — 
- p3 Highlight: "s either activated, meaning the node is influenced, or inactivated otherwise" — 
- p3 Highlight: "nitially inactivated unless chosen as the seed node" — 
- p3 Highlight: "for each node v in V that is activated at a certain time, it has a single chance of activating its neighbors at the next time" — 
- p3 Highlight: "I(G, S)" — 
- p3 Highlight: "at finding an seed nodes S* optimal set subset G, such that the influence spread is maximized." — 
- p3 Highlight: "As discussed before, when being selected, each node v in St may not necessarily be willing to act as a seed node" — 
- p4 Highlight: "node willing to be seed (when selected as We assume that this probability is known a priori," — 
- p4 Highlight: "Naturally, the set of nodes Ot who are willing to be seeds at round t is a subset of St: Ot subset St" — 
- p4 Highlight: "we use a binary matrix Xt,b b in a current su {0, 1}3x|V|" — 

## Study

**Question.** How to choose seed nodes for influence maximization when each invited node is only probabilistically willing to accept (contingency-aware IM), at low computational cost on new social networks, using reinforcement learning that generalizes to unseen networks without re-running an expensive greedy algorithm (Abstract p.1; Introduction p.1-2).

**Theory.** Influence spreads under the Independent Cascade (IC) model (Goldenberg et al. 2001; Kempe et al. 2003), Sec 3.1 p.3. Seed willingness has a known probability q, Sec 3.1 p.3. The influence function is treated as submodular (stated as "usually submodular", Sec 4.3 p.4). The sequential decision is a discrete-time MDP with sub-steps (t,b), Sec 3.2 p.3-4. The learner is DQN with structure2vec-style graph embeddings (Khalil et al. 2017), Sec 4.1 p.4. The key derived result is a surrogate reward (Eq. 11, Theorem 1, p.4) under assumptions Eqs. 9-10 (marginal contribution depends only on the count of willing predecessors; arithmetic-sequence form).

**Data.** Synthetic graphs only. Power-law graphs from a Barabasi-Albert growth model with an extra triangle step (Onnela et al. 2007): average degree 3, triangle probability 0.05, Sec 5.1 p.5. Unit of observation: graph and seed node. Default |V|=200 nodes, T=2 rounds, B=4 per round (budget 8), q=0.6, 200 training graphs, 5 validation graphs, 10 unseen test graphs, each run 20 times (200 runs per setting), Sec 5.1-5.2 p.5-6. Influence propagation probability 0.1 in experiments (text reads "belief propagation probability", Sec 5.1 p.5). One real-scale runtime reference: CHANGE on a 1000-node, 4974-edge network, propagation probability 0.2, sampling frequency 50, one Intel i9-9820X core; runtime reaches about 10 hours at 20 seeds (Figure 1, p.1-2). Experiment hardware: 2 Xeon Gold 6148 cores, 5 GB RAM, one Tesla V100 (Sec 5.1 p.5). Time period: not stated. No human sample or N of people. The HIV-prevention, homeless-youth setting (Yadav et al. 2016, 2018; Wilder et al. 2018) is the motivating application only.

**Method.** Reinforcement learning for influence maximization (RL4IM), Sec 4, p.4-5. (1) Each sub-step selects one node; the state is a 3 x |V| binary matrix (unselected; selected-willing; selected-unwilling; selected-unknown) plus fixed graph G, Sec 3.2 p.3-4. (2) State abstraction: X_bar = X1 + q*X3 (Eq. 5, Sec 4.2 p.4), with a feasible-action set maintained separately for unwilling nodes. (3) Reward shaping: the exact expected marginal reward (Eq. 6) is replaced by r~ = (1-q)*dI0 + q*dI_{b-1} (Eq. 11, Theorem 1), where dI0 and dI_{b-1} bound the marginal contribution (Lemma 1, Eq. 8). Theorem 2: reward computation falls from O(2^b) to O(1). Theorem 3 gives the gap bound max{(q-(1-q)^{b-1})(dI0-dI_{b-1}), (1-q-q^{b-1})(dI0-dI_{b-1})}. (4) Training: DQN with experience replay (memory 4096, batch 32, max 2000 training steps, discount 0.99, S2V embedding + 128-unit MLP, Adam), Sec 5.1 p.5. Baselines: CHANGE greedy (Wilder et al. 2018), S2V-DQN-IM (state abstraction added; reward uses dI_{b-1}), and Random. Validation uses 20 episodes x 5 graphs per checkpoint; IM spread is simulated 100 times per estimate.

**Identification.** None in the causal sense. This is an algorithm and simulation study with no quasi-experimental or observational identification. Performance is measured on held-out synthetic graphs: validation graphs select hyperparameters and checkpoints, and unseen test graphs measure generalization. Theorem 3 bounds the approximation gap of the surrogate reward under Eqs. 9-10, which are stated assumptions, not identified facts.

**Scope.** Population: synthetic power-law social networks, with a real-scale runtime illustration only (Figure 1). Institutions: Harvard Center for Research on Computation and Society; Nanyang Technological University (p.1). Era: arXiv June 2021, UAI 2021. Application framing: low-resource non-profits running HIV-prevention peer-leader interventions among homeless youth (Introduction p.1-2; Section 3 p.3). Generalization is claimed only across synthetic graphs with similar characteristics (Introduction p.2).

## Key findings

- Formulates contingency-aware IM as a discrete-time MDP in which each sub-step selects one node; the state is the willingness matrix plus the fixed graph. The time horizon is T x B. (T x B sub-steps; state X in {0,1}^(3 x |V|); Sec 3.2, p.3-4)
- Surrogate reward (Eq. 11) replaces the exact expected marginal contribution with (1-q)*dI0 + q*dI_{b-1}, under assumptions Eqs. 9-10. (r~ = (1-q)dI0 + q dI_{b-1}; Theorem 1, Sec 4.3, p.4)
- The surrogate cuts reward computation from O(2^b) to O(1) per sub-step, using 4 influence evaluations. (O(2^b) reduced to O(1); 4 influence values; Theorem 2, p.4)
- The gap between surrogate and exact reward is bounded, but the bound can be arbitrarily bad as q approaches 0 or 1. Authors say these cases are rare and that RL4IM still works at q=0.2 and q=0.8. (gap <= max{(q-(1-q)^{b-1})(dI0-dI_{b-1}), (1-q-q^{b-1})(dI0-dI_{b-1})}; q=0.2 and 0.8 tested; Theorem 3 and following paragraph, p.4-5)
- Ablation Q1: removing state abstraction or reward shaping makes training unstable or converges to a sub-optimal point. Figure 3 shows validation curves; numeric values are not in the extracted text. (Figure values not extractable; Q1, Sec 5.2, p.5; Figure 3, p.5)
- Q2: more training graphs give a more stable validation curve. The paper uses 200 training graphs for all later experiments. (training pool sizes 10, 50, 100, 200, 500; 200 chosen; Q2, Sec 5.2, p.5-6; Figure 4)
- Q3: across q, RL4IM and CHANGE both reach roughly twice Random and are more stable; S2V-DQN-IM is unstable. Expected influence rises with q. (roughly 2x Random; figure axis labels -0.05 to 0.2; plotted values not in text; Q3, Sec 5.2, p.6; Figure 5(a))
- Q4: with T x B fixed at 8, RL4IM works for single-round IM (T=1); CHANGE and RL4IM are the two best methods. (T in {8,4,2,1}; T x B = 8; Q4, Sec 5.2, p.6; Figure 5(b))
- Q5: normalized influence falls as graph size grows from 50 to 1000 nodes with budget fixed at 8; CHANGE and RL4IM are best. (|V| in {50,100,200,500,1000}; budget 8; axis 0 to 0.35; Q5, Sec 5.2, p.6; Figure 5(c))
- Overall, RL4IM matches or slightly exceeds CHANGE. Authors attribute the edge to proactively modeling unwillingness in the reward, versus CHANGE reacting to realized willingness. Authors claim near-zero test runtime (policy in seconds on a laptop). ('slightly better than CHANGE'; runtime 'within seconds' (no measured figure given); Sec 5.2 p.6 (paragraph after Q5); Conclusion p.6)
- Runtime motivation: CHANGE needs about 10 hours even with 20 seeds on a 1000-node, 4974-edge network. (10 hours at 20 seeds; 1000 nodes; 4974 edges; Introduction and Figure 1, p.1-2)

## Limitations

- The surrogate reward rests on assumptions (Eqs. 9-10: marginal contribution depends only on the count of willing nodes; arithmetic-sequence form) and on submodularity of influence (Eq. 7; stated as 'usually submodular', p.4). The paper does not test these on real data.
- The Theorem 3 gap bound can be arbitrarily bad as q goes to 0 or 1; the authors call these cases rare and extreme (p.5).
- Willingness probability q is assumed known a priori and identical across nodes (footnote 2, p.3; Sec 3.1 p.3).
- Experiments use synthetic power-law graphs only, with IC propagation probability 0.1 (p.5). The only real-scale measurement is the CHANGE runtime in Figure 1.
- Only the IC model is evaluated.
- No training cost (wall-clock or GPU hours) is reported. The 'negligible runtime at test phase' claim is qualitative ('within seconds', p.6).
- Baselines are one greedy algorithm (CHANGE) and one RL baseline (S2V-DQN-IM with state abstraction added). Other contingency-aware methods, such as the Yadav et al. POMDP, are not benchmarked.
- Key figure results (Figures 3-5) are described only qualitatively in the text; plotted values are not stated numerically.
- The single-round setting (T=1) is evaluated only on synthetic graphs (p.6).

## Short quotes

- "the non-profits usually do not have the HPCs to recalculate the solutions" (Abstract and Introduction, p.1 (12-word fragment; the sentence is longer, so only this fragment is quoted))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Contingent seed activation gate|Contingent seed activation gate]] | method-source | A selected seed is willing with a probability q that is known a priori. Footnote 2 says q is the same for all nodes because per-node values are impractical to know. Default q = 0.6; Q3 varies q over 0.2, 0.4, 0.6, 0.8. The paper estimates no real-world q. (Sec 3.1 p.3 (text 'known a priori', marker 2); footnote 2 p.3-4; Sec 5.1-5.2 p.5-6; Fig 5(a) p.6) | gate(n,cs) = max(0.15, seed_prob(n) x SGATE[stance]) at moneyball_core.py:630. SGATE values at 626-629 match the description (1.00 down to 0.35; unknown 0.85; Ally, Champion, Defender 1.0). seed_prob has a behavioral branch (0.60 + .10 coop + .06 breadth + .04 visibility, clipped 0.30-0.90) and a non-behavioral branch (0.60 + .10A + .06O + .04E, same clip). (`fec/moneyball_core.py:626-630; fec/members/receptivity_layer.py:210-216`) | The modeling idea transfers: invitation is not activation. The paper's q is homogeneous and its analysis is multi-round. SGATE is member-specific and used single-shot, so Theorems 1-3 do not cover it. The 0.60 baseline is the paper's simulation default, not an estimate. The source population is HIV peer leaders, not legislators. SGATE values remain design priors until measured. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Budget frontier with knee and electoral modes|Budget frontier with knee and electoral modes]] | supports | The authors attribute RL4IM's slight edge over greedy CHANGE to proactively modeling unwillingness in the reward, versus CHANGE reacting to realized willingness. The edge is called slight and gets no numbers. At T=1 (single round, T x B = 8), RL4IM and CHANGE are the two best methods. The roughly-twice-Random result is stated for the q sweep (Q3), not for T=1. All results are synthetic graphs under IC with p=0.1. (Q3 p.6 (Fig 5a); Q4 p.6 (Fig 5b); paragraph after Q5 p.6; Sec 5.1 p.5) | spread() in budget_frontier.py draws each lobbied seed's activation against gate(n,cs) on a dedicated CRN stream (key offset 2^62, lines 127-128). A failed activation stays an opponent (lines 129-131). MAXSEEDS defaults to 60 (line 7). Lazy-greedy mechanics were not checked. (`fec/engine/budget_frontier.py:7,126-131`) | Supports only the design choice of putting take-up uncertainty inside the expected-spread objective. The paper models one colour; the system has a two-colour competitive cascade. The paper's synthetic graphs (about 200 nodes) are comparable in scale to the 533-member graph, but there are no political data. The slight edge is qualitative. | weak | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Per-topic flip and coalition allocators|Per-topic flip and coalition allocators]] | context | Within a round, a seed's exact expected marginal contribution depends on which earlier seeds turned out willing, giving O(2^b) cases. The paper assumes the influence function is submodular, stated as usually submodular and citing Kempe et al. 2003 (Eq. 7). (Sec 4.3 Eqs. 6-8, Theorems 1-2, p.4 (Eq. 7 text at line 674)) | per_topic_coalition.py gates each lobbied seed inside each spread() call (lines 102-110). Individual marginals spread({n}) minus base (line 115) feed a water-fill (line 119). The funded set's joint spread is reported (lines 124-125) because summing marginals overstates the joint gain. The allocator is one-shot, with no rounds and no observed willingness. (`fec/engine/per_topic_coalition.py:102-125`) | The paper's O(2^b) dependence arises from realized willingness within sequential sub-steps. The coalition allocator averages willingness inside each spread() call, so there is no conditional dependence to ignore at allocation time. The shared point is only that seed non-additivity is real and the joint report exposes it. The surrogate reward needs homogeneous q and Eqs. 9-10, so it does not port to heterogeneous gates. | weak | VERIFIED-SOFTENED |
| [[systems/melange-validation#Prospective receptivity contract (H1-H2) and forecast ledger|Prospective receptivity contract (H1-H2) and forecast ledger]] | qualifies | q is the probability that a selected node is willing to act as a seed, conditional on being selected or invited. It is assumed known a priori, so it is a take-up rate, not an unconditional outcome. (Sec 3.1 p.3 (lines 314-316, probability of a node willing to be seed when selected as seed node)) | H1 protocol: Spearman correlation of frozen seed_prob with NEW cosponsorships adopted after the freeze, over members with panel coverage in both snapshots. Engaged-only scoring is unavailable until receipted contact data exists. Minimum data is 60 days and 150 members with any delta; otherwise status is UNDERPOWERED (outcome_contract.py:17-21 and 262-263). (`fec/engine/outcome_contract.py:17-21,262-263`) | Unconditional new cosponsorship mixes take-up with contact choice and base activity. A pass would not validate seed_prob as a contingency probability, and a miss would not refute it. The construct match needs an engaged-only arm built on a contact log. | moderate | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The summary calls the 0.60 baseline arbitrary. The paper calls q=0.6 its simulation default and gives no basis, so arbitrary goes beyond the text.
> - Summary issue: The summary and bearing 1 call willingness Bernoulli. The paper states only a known probability q; the word is not in the paper.
> - Summary issue: The summary describes seed_prob as one formula. The code also has a non-behavioral branch (A, O, E terms, same clip) that the summary omits.
> - Summary issue: Bearing 2 attaches roughly-twice-Random to the T=1 result. In the paper it belongs to the q sweep (Q3).
> - Summary issue: Paul's addendum tier filing is checked: the paper assumes submodularity (Eq. 7), so it does not support a non-submodular tier. Its observe-then-act framing is accurate only between rounds; the system is single-shot.
> - Summary issue: Not verified: the 72 of 511 new-event count and the look-unspent status in H1. No matching file was found in the audits folder within the search limit.
> - Summary issue: The citation outcome_contract.py:277 was not confirmed; the minimum-data check is at lines 262-263.
> - Summary issue: Lazy-greedy mechanics in budget_frontier.py were not checked; only MAXSEEDS=60 (line 7) was verified.
> - Summary issue: Coverage: the output schema accepts bearings, related and summary_issues only, so the task's key_findings and paul_markup lists were not returned. Verification covered the paper's Sec 3.1, 3.2, 4.2-4.3, 5.1-5.2 and conclusion, footnote 2, and the three Melange code references plus H1 protocol.

## Gaps and flags

- The system never observes or updates willingness. The paper's setting is multi-round: willingness is revealed at the end of each round and later seeds are chosen knowing it. Melange seeding is a one-shot plan. Replanning after observed take-up would need an outreach or contact log, which does not exist.
- No empirical take-up rate backs either SGATE or seed_prob. The paper supplies none for any population, and the 0.60 baseline equals its simulation default.
- The H1 receptivity contract measures unconditional cosponsorship, not take-up conditional on contact. The construct Chen defines cannot be tested until receipted contact data exists.
- The paper shows that a seed's marginal value depends on earlier seeds' realized willingness. The coalition allocator funds on individual gated marginals and only reports the joint shortfall afterwards. It does not fund on conditional marginals.
- Language: seed_prob x SGATE is a heuristic design prior. Do not describe it as a measured take-up, access or commitment probability for a legislator. The paper validates no q for any real population, and H1 is underpowered against its 150-member floor.
- Paul's addendum files Chen 2021 under 'C. RL influence maximization - the non-submodular envelope'. The paper explicitly assumes submodularity (Eq. 7) and IC. The non-submodular, complex-contagion work is the same group's RL4CCIM (chen-2023-complex-contagion-influence). Chen 2021 should not be cited as evidence for the non-submodular tier.
- Paul's takeaway describes the method as 'observe-then-act'. That is accurate only between rounds; within a round willingness is unknown and is replaced by its expectation (X1 + q*X3). The system as built observes nothing (single-shot), so the 'observe-then-act' framing describes an unbuilt capability.
- The paper's motivation for RL (repeated recomputation on new graphs by low-compute NGOs) does not apply to Melange's fixed 533-member chamber graphs. Its quality result is parity with greedy. Neither supports replacing the CELF or lazy-greedy layer with a learned policy.
- No rule conflicts. The paper does not argue for dropping regular cosponsorship edges or for per-edge volume normalization, and it uses no LDA or reciprocity claims.

Paul's mapping is right in substance and is already built as SGATE. His mapping: a lobbied legislator may or may not take up an invitation, so contact is not activation. The receptivity docstring echoes it ('legislators know they are being lobbied'). The gate is applied inside the frontier and coalition spread estimators. Two parts of his note are stale or loose relative to the system and the paper. First, 'sequential, observe-then-act seeding' describes the paper's multi-round setting, but Melange seeds single-shot and never observes willingness, so that half of his takeaway is an extension, not current behaviour. Second, he files the paper in the non-submodular RL tier, but Chen 2021 assumes submodularity; the complex-contagion argument belongs to RL4CCIM. His broader expectation that a learned policy (GCOMB, RL) will replace greedy is not supported here. RL4IM only matches greedy, and its advantage is runtime on new graphs, which Melange does not face.

## Related papers

- [[chen-2023-complex-contagion-influence]] — extends — Item exists. Same group and RL-for-IM lineage. Paul's addendum attributes the non-submodular argument to this paper, but this paper assumes submodularity; the complex-contagion work is the one that carries that framing.
- [[golovin-2011-adaptive-submodularity-theory]] — complements — Item exists. The paper contrasts itself with adaptive IM, which it says does not address willingness uncertainty. That is a contrast, not a contradiction, so the relation is changed from contradicts.
- [[kamarthi-2020-influence-maximization-unknown]] — complements — Item exists. RL for IM under graph-exploration uncertainty, cited in Paul's addendum as the other learned-policy entry. The paper's own reference list dates it 2020, so the slug year may differ.
- [[zareie-2023-influence-maximization-social]] — complements — Verified title: Influence maximization in social networks: a survey of behaviour-aware methods. Code comments in budget_frontier.py tie it to Chen as the SNAM source for seed_prob.
- [[bharathi-nd-competitive-influence-maximization]] — complements — Verified title: Competitive Influence Maximization in Social Networks. It is the likely source of the competitive cascade the gate is inserted into; the gate link itself was not checked.
- [[kumar-2022-influence-maximization-social]] — same-method — Verified title: influence maximization using graph embedding and graph neural networks. Shares the graph-embedding approach of the paper's structure2vec backbone.
- [[arya-2025-relink-edge-activation]] — same-method — Verified title: RELINK, edge activation for closed-network IM via deep RL. Shares the deep-RL approach; the claim that it sits in the same S2V/DQN lineage was not checked.
- [[almazaydeh-2026-heuristic-centrality-methods]] — complements — Verified title: heuristic centrality methods challenge greedy optimization in IM. Fits the paper's parity-with-greedy result, which suggests RL's advantage is runtime, not quality.

### Linked from
- [[trivedi-2020-efficient-influence-maximization]] — extended-by — Title matches contingency-aware seeding, where invited nodes accept only probabilistically, which the plain DHICM and CELF comparison does not model.
- [[zeng-nd-multi-agent-reinforcement]] — complements — Both use reinforcement learning to replace a hand-set rule policy. Chen et al. do it for seeding legislators-as-nodes under take-up uncertainty, which is the literature actually behind the SGATE seed gate. Zeng applies RL only to physical swarm control.
- [[madsen-2017-strategic-advantages-micro]] — complements — Both make contact conditional on the target's receptivity. Chen does it as contingency-aware seeding, which the system implements as SGATE; this paper does it as credibility-gated contact.
- [[chen-2023-complex-contagion-influence]] — built-on-by — RL4IM from that paper is the base architecture and a baseline in this paper (Sec. 5.2 and Sec. 6.1). receptivity_layer.py line 29 and budget_frontier.py line 73 cite the contingency-aware idea from the same group.
- [[kamarthi-2020-influence-maximization-unknown]] — complements — Tambe group RL for contingency-aware IM, where invited seeds may refuse (uncertain seed take-up). Wilder is cited, not a co-author. Kamarthi handles an uncertain graph instead. The SGATE mapping is Paul's and was not checked here.
- [[sutton-2018-reinforcement-learning-introduction]] — complements — Paper exists in the research store (papers/chen-2021-contingency-aware-influence.md) and its components list the seed gate. Its stub does not cite Sutton and Barto, so a builds-on link to this book is not confirmed.
- [[ng-1999-policy-invariance-reward-transformations]] — complements — Chen et al. 2021 have a reward-shaping component (REWARD SHAPING section) based on marginal contribution, not a potential-based form, so the Ng condition bears directly on it. The earlier needs-the-potential-condition wording is an inference, not a finding in Chen.
- [[watkins-1992-q-learning]] — built-on-by — Real item: contingency-aware influence maximization as a reinforcement learning approach (title and authors confirmed). Whether its learning step is Q-learning was not verified in the item text.
- [[dai-2017-learning-combinatorial-optimization-graphs]] — built-on-by — Verified: RL4IM uses S2V-based Q-networks (text line 994) and S2V-DQN-IM is a baseline (line 968).
- [[manchanda-2020-gcomb-learning-budget-constrained]] — same-method — Title confirms RL for IM with contingent seed acceptance. Checked by title and note only; item file exists.
- [[kempe-2015-maximizing-spread-influence]] — extended-by — Adds seed take-up probability to Kempe-style IM. Kempe has no take-up step. Item exists; not re-read here.