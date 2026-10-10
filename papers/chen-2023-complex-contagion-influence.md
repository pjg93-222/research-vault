---
type: paper
title: "Complex Contagion Influence Maximization (RL4CCIM)"
authors: ["Haipeng Chen", "Bryan Wilder", "Wei Qiu", "Bo An", "Eric Rice", "Milind Tambe"]
year: "2023"
venue: "IJCAI-23 (Proceedings of the Thirty-Second International Joint Conference on Artificial Intelligence), pp. 5531-5540. Metadata hint had blank venue, year and DOI; the venue and year are taken from the running header in the text."
doi: ""
category: "Adaptive Influence Maximization"
systems: ["melange-allocation", "melange-influence-engine", "melange-pathfinding"]
components: ["melange-allocation.budget-frontier", "melange-allocation.per-topic-allocators", "melange-influence-engine.lt-priming", "melange-pathfinding.multi-target-and-budget-modes", "melange-influence-engine.cascade-kernel"]
paul_annotated: true
paul_markup_count: 74
evidence_status: "verified"
source: "[[_sources/chen-2023-complex-contagion-influence.pdf]]"
original_paths: ["C:\\Users\\pgoff\\OneDrive\\Desktop\\0614.pdf"]
sha12: ["9cdf2b506cfb"]
tags: [paper, adaptive-influence-maximization]
---

# Complex Contagion Influence Maximization (RL4CCIM)

**Haipeng Chen, Bryan Wilder, Wei Qiu, Bo An, Eric Rice, Milind Tambe (2023).** *IJCAI-23 (Proceedings of the Thirty-Second International Joint Conference on Artificial Intelligence), pp. 5531-5540. Metadata hint had blank venue, year and DOI; the venue and year are taken from the running header in the text..* Theme: [[themes/adaptive-influence-maximization|Adaptive Influence Maximization]].

> [!abstract] Summary
> Chen, Wilder, Qiu, An, Rice and Tambe (IJCAI-23) study seed selection when adoption follows complex contagion: under their K-complex model a node activates with probability p0 once at least K neighbours are active and p1 otherwise. They prove the objective is non-submodular and NP-hard, then frame seeding as an MDP solved by Double-DQN (RL4CCIM). Three additions address sparse rewards (solution filtering to nodes near a degree-K hub, reward-prioritised replay, a global marginal-contribution reward), and a GAT encoder changes the graph representation. On nine networks (five small shelter networks and four larger public ones) with deterministic cascades (p0=1, p1=0, budget T=8 or 12), RL4CCIM has the best normalised influence everywhere, sometimes tied. Greedy collapses on India (0.0396 vs 0.5347). RL4CCIM matches the MIP optimum on DC and DOSIM and comes within about 6 to 8 percent elsewhere, although the SPY optimum is unverified because the MIP exceeded its 12-hour limit from SPY onward. In the India ablation, single-component drops are 4.6 to 21.3 percent, and removing all components gives 35.2 percent. There are no field data: cascades are simulated, and scale is the stated limit. For Paul's system the paper does not supply an estimate. It is a warning about an assumption. The budget frontier runs a simple-contagion independent-cascade kernel with lazy-greedy and knee logic justified by diminishing returns; per-topic coalition is a one-pass marginal ranking with water-fill, and the dashboard budget recruit uses one-hop coverage probabilities, not a cascade. If real cosponsor joining is threshold-like, as the LT priming module already assumes one target at a time, the budget frontier's greedy path can miss jointly valuable seed sets. They would also drop candidates whose solo marginal is zero. The useful takeaway is a test, not a method swap: measure whether CU-bill join hazards jump with the count of already-joined close partners. Until that is checked, read greedy guarantees as properties of the model kernel only.

## Paul's annotation

From [[annotations/Adaptative - Influence Maximization - Melange Annotated Bibliography — Addendum|Adaptative - Influence Maximization - Melange Annotated Bibliography — Addendum]]:

> **Chen, H., Wilder, B., Qiu, W., An, B., Rice, E., & Tambe, M. (2023).** *Complex Contagion Influence Maximization: A Reinforcement Learning Approach.* IJCAI '23, 5531–5540. https://www.ijcai.org/proceedings/2023/614 (RL4CCIM)
>
> The clearest published validation of the master/subordinate split, and the most important paper in this section for Melange's theory. Complex contagion — where a node's adoption probability rises sharply with the *number* of already-activated neighbors — is **non-submodular**, which breaks the diminishing-returns property that the greedy (1 − 1/e) guarantee depends on. The authors give the first RL approach to the problem and identify reward sparseness as the core obstacle, addressing it with a reward-shaping component (marginal-contribution rewards at episode end) and return-based prioritized experience replay. This is precisely why the outer MDP layer cannot be replaced by iterated greedy. Note the substantive implication: coalition formation in Congress is plausibly a complex contagion (a fence-sitter commits only once enough allies already have), which would place Melange's core problem *outside* the submodular regime and squarely in the RL envelope.

## Paul's markup in the PDF

- p1 Highlight: "Paraphrase: defines the influence maximization goal, choosing seed nodes to maximize spread." — 
- p1 Highlight: "Paraphrase: sociology evidence that many cascades follow complex contagion, where probability rises with influenced neighbors." — 
- p1 Highlight: "Paraphrase: the same complex-contagion idea, restated as higher cascade probability with more influenced neighbors." — 
- p1 Highlight: "Paraphrase: main obstacle for applying existing RL to CCIM is reward sparseness from two sources." — 
- p1 Highlight: "Paraphrase: IC and LT are the standard cascade models, cited to Kempe et al. 2003." — 
- p1 Highlight: "Paraphrase: a Centola-style claim that individual behavior responds to neighbors as a whole." — 
- p1 Highlight: "Word-level highlight: a single fragment of the individual-behavior sentence." — 
- p1 Highlight: "Paraphrase: adoption more likely with reinforcement from multiple network neighbors (Centola 2010)." — 
- p1 Highlight: "Paraphrase: Figure 1 shows a sharp surge in complex-contagion adoption at three activated neighbors." — 
- p1 Highlight: "Paraphrase: designing interventions in these domains needs algorithms that seed complex cascades." — 
- p1 Highlight: "Paraphrase: diminishing returns to adding seeds, i.e., submodularity." — 
- p1 Highlight: "Paraphrase: under complex contagion, a single seed's marginal gain is small because its value appears only alongside specific other seeds." — 
- p1 FreeText: "Margin note on the introduction; no covered text." — What about the impact of high ranking members? Not diminishing. Susceptible to pressure as varia
- p2 Highlight: "Paraphrase: continuation of the complementarity point, that a seed's value depends on a specific set of other seeds." — 
- p2 Ink: "Freehand ink mark; no covered text." — 
- p2 Highlight: "Paraphrase: DPIM depends heavily on hierarchical network structure and is limited when no good decomposition exists." — 
- p2 Highlight: "Paraphrase: RL methods choose seeds one at a time, greedily by marginal score." — 
- p2 Highlight: "Fragment: the scores are estimated with deep function approximators." — 
- p2 Highlight: "Paraphrase: the CCIM reward signal is much sparser than in simple-contagion IM." — 
- p2 Highlight: "Paraphrase: first cause of sparseness, the effective solution space is much smaller." — 
- p2 Highlight: "Paraphrase: second cause, a node's marginal contribution is sparse because reward appears only after several seeds." — 
- p2 Ink: "Freehand ink mark; no covered text." — 
- p2 Highlight: "Paraphrase: an adapted return-based prioritized replay improves policy exploration." — 
- p2 Ink: "Freehand ink mark; no covered text." — 
- p2 Ink: "Freehand ink mark; no covered text." — 
- p2 Highlight: "Paraphrase: results on nine real networks show state-of-the-art performance." — 
- p2 Highlight: "Paraphrase: classical cascade models (IC, LT) are essentially submodular, and prior work with guarantees focused on them." — 
- p2 Highlight: "Paraphrase: complex contagion models collective behavior where probability is higher when several neighbors are active." — 
- p2 Highlight: "Paraphrase: RL methods for combinatorial problems break the combined action into sequential single actions." — 
- p2 Ink: "Freehand ink mark; no covered text." — 
- p3 Highlight: "Paraphrase: the network is a graph G=(V,E)." — 
- p3 Highlight: "Paraphrase: each node is either activated or inactive." — 
- p3 Highlight: "Paraphrase: in IC and LT, activation probability is a constant independent of neighbor count." — 
- p3 Highlight: "Equation 1: the K-complex piecewise activation probability (p0 when k is at least K, p1 otherwise)." — 
- p3 Highlight: "Equation 2: the seed-set objective, maximizing spread over sets of size T." — 
- p3 Highlight: "Paraphrase: a K=1 complex contagion equals an arbitrary IC instance, which gives the NP-hardness reduction." — 
- p3 Highlight: "Paraphrase: each time step selects one seed, and the MDP horizon equals budget T." — 
- p4 Highlight: "Paraphrase: the state records which nodes were already selected before step t." — 
- p4 Highlight: "Equation 3: deterministic state transition, the next state equals the current state plus the action." — 
- p4 Highlight: "Paraphrase: one-step reward is the marginal spread gain of the newly selected seed." — 
- p4 Highlight: "Paraphrase: RL is a paradigm where agents learn actions to reach a goal." — 
- p4 Highlight: "Paraphrase: Q-learning Bellman update, with the discount factor and the next-state maximum." — 
- p4 Highlight: "Paraphrase: the authors build RL4CCIM on this line of prior RL work." — 
- p4 Highlight: "Paraphrase: credit sparseness illustration, a seed sequence of {1, 2, 3, 5} in the toy network." — 
- p5 Highlight: "Figure 3 caption: RL4CCIM architecture." — 
- p5 Highlight: "Fragment: a stray word from the architecture description." — 
- p5 Highlight: "Paraphrase: training runs many episodes; shaped-reward transitions go into the replay buffer." — 
- p5 Highlight: "Paraphrase: stored transitions are reused to generate more actions and training data." — 
- p5 Highlight: "Fragment from the GAT paragraph (OCR-broken): 'od W'." — 
- p5 Highlight: "Paraphrase: the paper focuses on the first three improvements and defers GAT details to the original paper." — 
- p5 Highlight: "Paraphrase: GAT as the graph representation method, citing Velickovic et al. 2018." — 
- p5 Ink: "Freehand ink mark; no covered text." — 
- p5 Highlight: "Fragment: graph notation (V, E)." — 
- p5 Highlight: "Fragment: budget notation T." — 
- p5 Highlight: "Paraphrase: the sequence decomposition avoids enumerating the full combinatorial action space." — 
- p5 Highlight: "Paraphrase: effective solutions are rare in the CCIM problem." — 
- p5 Highlight: "Paraphrase: effective solutions become rarer as networks and threshold K grow." — 
- p5 Highlight: "Fragment: continuation of the 'gets rarer' sentence." — 
- p5 Highlight: "Paraphrase: solution filtering keeps nodes with at least K neighbors and their neighbors." — 
- p5 Highlight: "Paraphrase: the filtered set keeps potentially optimal solutions and drops the rest." — 
- p5 Highlight: "Paraphrase: filtering does not exclude valid options, and exploration is restricted during action sampling." — 
- p6 Highlight: "Paraphrase: prioritized experience replay (Schaul et al. 2016) to favor high-reward solutions." — 
- p6 Highlight: "Equation 5: priority formula combining TD error and weighted reward." — 
- p6 Highlight: "Paraphrase: under submodularity, later seeds make smaller marginal contributions, which justifies the standard reward." — 
- p6 Highlight: "Paraphrase: adds a regularization term to the reward." — 
- p6 Highlight: "Equation 6: global marginal contribution of an action, given the other seeds in the episode." — 
- p6 Highlight: "Paraphrase: DPIM reimplemented because code is not public; uses a Jaccard-similarity hierarchical decomposition." — 
- p6 Highlight: "Paraphrase: the 12-hour MIP limit was exceeded starting at SPY (67 nodes)." — 
- p7 Highlight: "Paraphrase: Greedy can be arbitrarily bad, for example on India, where it finds no effective solution." — 
- p7 Highlight: "Paraphrase: DPIM depends on the hierarchy assumption, which is hard to measure in practice." — 
- p8 Highlight: "Paraphrase: simulation time is the main bottleneck, since RL needs many training episodes." — 
- p8 Highlight: "Paraphrase: complex contagion is harder to simulate and less studied, so simulation is slower." — 
- p8 Highlight: "Paraphrase: large networks could be pruned to keep only important nodes." — 
- p8 Highlight: "Paraphrase: empirical results show new state-of-the-art performance in CCIM." — 

## Study

**Question:** Can reinforcement learning be used to solve complex contagion influence maximization (CCIM): choosing a fixed budget of seed nodes when cascade probability rises with the number of already-activated neighbors, so the objective is non-submodular? (Sec. 1, PDF p2)

**Theory:** Influence cascades are modeled as independent cascade (IC) or linear threshold (LT), both submodular, so greedy seeding has guarantees (Kempe et al. 2003). Complex contagion (Centola and Macy 2007) makes adoption probability jump when enough neighbors are active (S-shaped diffusion, Backstrom et al. 2006), which breaks submodularity and creates complementarities among seeds. The K-complex model (Eq. 1) sets activation probability to p0 when at least K neighbors are active and p1 otherwise, with 0 <= p1 << p0 <= 1. The authors frame seed selection as an MDP where each time step adds one seed, and reward is the marginal spread gain; sparse rewards are the central obstacle.

**Data:** Nine networks; no observation period is stated in the extracted text, and the study simulates cascades rather than using observed adoption data. Unit: network nodes (individuals in the social networks). Five small social networks collected from homeless youth shelters in Los Angeles: CHANGE, DC, DOSIM, MFP, SPY. Four public networks: Football (Girvan and Newman 2002), Polbooks (Rossi and Ahmed 2015), India (Banerjee et al. 2013), Exhibition (Isella et al. 2011). Node counts appear in the text only for India (202 nodes) and SPY (67 nodes); other sizes are in Appendix C, which is not in the extract. Basic setting: p0=1, p1=0 (deterministic), seed budget T=8 (T=12 for Exhibition), threshold K=3 for CHANGE-SPY, K=4 for Football, Polbooks, India, K=6 for Exhibition.

**Method:** Algorithmic benchmark, no causal design. Proposed RL4CCIM builds on the RL4IM/S2V line (Khalil et al. 2017; Chen et al. 2021) and uses Double-DQN (Van Hasselt et al. 2016) with four changes: (1) Solution filtering: candidate set A is the union of neighborhoods of every node with at least K neighbors; exploration samples from A with probability eps*eps', eps' annealed from 0.99 to 0 (Alg. 1 lines 3-5, 9; Thm 3). (2) Reward-based prioritized experience replay with priority (|delta| + beta*r + eps)^alpha (Eq. 5). (3) Reward shaping r' = r + omega*r_global, where r_global is the action's marginal contribution given all seeds chosen in the episode (Eqs. 6-7); this needs a full simulation before per-step rewards are assigned. (4) Graph attention network (GAT) replaces S2V as the graph encoder. Baselines: Random+ (seeds drawn from A, 50 runs averaged), Greedy (Kempe et al. 2003), DPIM (Angell and Schoenebeck 2017; authors' own reimplementation because code is not public), RL4IM (Chen et al. 2021), and OPT from a Gurobi mixed-integer program on small networks only (12-hour limit, exceeded from SPY onward per text). RL methods: 15 runs, best model chosen by validation every 20 training steps. Training finishes within 2 hours. Hyperparameters are in Appendix B (not in extract).

**Identification:** Not a causal design. The study is an algorithmic benchmark: a fixed set of networks and a simulated cascade model, with no treatment or outcome data. Comparisons use the deterministic basic setting (p1=0, p0=1) to remove simulation noise, then sensitivity sweeps over p1, T and K. RL model selection uses a separate validation process every 20 training steps.

**Scope:** Algorithmic influence maximization on nine small to medium social and contact networks, with simulated K-complex cascades. Population: no human subjects; the five shelter networks come from LA homeless youth shelters, cited to Wilder et al. 2021 for the HIV-prevention setting. Institutions: a multi-institution computer science and AI group (William and Mary, CMU, NTU, USC, Harvard). Era: IJCAI-23, 2023. The paper says training time and scalability are not the focus, so all experiments use small networks. Nothing here addresses lobbying, elections, legislators, text measurement, or turnout.

## Key findings

- Complex contagion influence maximization is non-submodular (Theorem 1) and K-complex contagion IM is NP-hard (Theorem 2), shown by reduction from IC when K=1. Proofs are in Appendix A, which is not in the extract. Numbers: None reported beyond the K=1 reduction. (Sec. 3, PDF p3 (printed 5533))
- Reward sparseness has two sources: effective-solution sparseness (few seed sets activate anyone) and credit sparseness (rewards stay flat until several seeds are chosen). Numbers: Toy network (Fig. 2, 8 nodes, p0=1, p1=0, K=3, T=4): 17 of C(8,4)=70 four-seed sets are effective, 24.3%. Seed sequence {1,2,3,5} gives per-step rewards 1,1,1,2.. (Sec. 1 PDF p2; Sec. 5.1 PDF p4 (printed 5534))
- Solution filtering keeps only nodes that could be part of an effective solution (neighborhoods of nodes with at least K neighbors). Theorem 3 says no effective solution is excluded. Numbers: Toy example: feasible sets shrink from 70 to 15, which is 21.4% of the original space. The text also says 24.2% of nodes (not sets) have non-zero reward, which conflicts with 24.3% in Sec. 5.1.. (Sec. 5.3 PDF p5 (printed 5535), Alg. 1 lines 3-5)
- In the deterministic basic setting, RL4CCIM has the highest normalized influence on all nine networks, by the authors' account, with some ties. Numbers: Football 1.0 (RL4IM 0.513, DPIM 0.3913, Greedy 0.3217); Polbooks 0.819 (RL4IM 0.7429, DPIM 0.6762); India 0.5347 (DPIM 0.3911, RL4IM 0.3465, Greedy 0.0396); Exhibition 0.5585 (DPIM 0.5537, RL4IM 0.5439, Greedy 0.0293); CHANGE 0.283, tied with DPIM and RL4IM (Greedy 0.1509).. (Table 1 and Sec. 6.1, PDF p7 (printed 5537))
- On small networks where exact MIP gives a value, RL4CCIM matches the optimum on DC and DOSIM and is slightly below it elsewhere. Numbers: DC 0.4634 = OPT; DOSIM 0.375 = OPT; CHANGE 0.283 vs OPT 0.3019 (about 6.3% below, derived); MFP 0.3472 vs 0.375 (about 7.4% below, derived); SPY 0.5075 vs 0.5522 (about 8.1% below, derived).. (Table 1 (OPT row), PDF p7)
- Greedy performs very poorly on some networks. The authors say it finds no effective solution on India (202 nodes). Numbers: Greedy 0.0396 on India vs RL4CCIM 0.5347; Random+ 0.0498 on India; Greedy and Random+ both 0.0293 on Exhibition.. (Sec. 6.1 PDF p7; seed comparison in Appendix E (not in extract))
- RL4IM and DPIM are unstable across networks. The authors attribute DPIM's gaps to its hierarchy assumption and RL4IM's to effective-solution sparseness that grows with solution-space size. Both explanations are stated as hypotheses. Numbers: RL4IM Football 0.513 vs 1.0; DPIM Football 0.3913 and India 0.3911 vs RL4CCIM 1.0 and 0.5347.. (Sec. 6.1 PDF p7 (printed 5537))
- RL4CCIM beats all baselines across a sensitivity sweep of propagation probability, budget and threshold on India, Football and Polbooks. Greedy jumps on India when T rises from 12 to 14 and K falls from 4 to 3, which the authors attribute to a larger share of effective solutions. Numbers: p1 in {0.2, 0.4, 0.6, 0.8, 1}; T in {6, 8, 10, 12, 14}; K in {2, 3, 4, 5, 6} (K=5 used for Football T-sweep). Fig. 4 plotted values are not in the extracted text.. (Sec. 6.2 and Fig. 4 PDF pp. 7-8 (printed 5537-5538))
- Ablation on India: removing each component lowers normalized influence, and removing all of them (which equals RL4IM) lowers it most. Numbers: Drops: w/o solution filtering 10.2%; w/o PER 4.6%; w/o reward shaping 9.3%; w/o GAT 21.3%; w/o all components 35.2%.. (Sec. 6.3 and Fig. 5, PDF p8 (printed 5538))
- Scalability is the main limitation: simulation time for complex contagion is the top bottleneck, followed by Q-network gradient cost on large graphs. Pruning is suggested as a remedy. Numbers: All experiments use small networks; RL training finishes within 2 hours (Sec. 6 PDF p6).. (Limitation section, PDF p8 (printed 5538))
- Solution filtering alone and PER alone each contribute a measurable share of the gain, and the authors judge the combined components necessary. Numbers: See ablation drops above (10.2% and 4.6% respectively).. (Sec. 6.3, PDF p8)

## Limitations

- Scalability: complex-contagion simulation cost and Q-network training cost limit the method to small networks; the authors present pruning as an unexplored fix (PDF p8).
- The paper says training time and scalability are not its focus; all experiments run on small networks (PDF p6).
- The DPIM baseline is the authors' own reimplementation (original code not public), using a Jaccard-similarity hierarchical decomposition (PDF p6). Results for this baseline therefore depend on that implementation.
- The MIP optimum was reported as exceeding the 12-hour limit starting at SPY (67 nodes), yet Table 1 gives an OPT value for SPY (0.5522). This conflict is not explained in the text and should be checked.
- Table 1 is limited to the deterministic setting (p1=0, p0=1). Stochastic propagation appears only in Fig. 4, and its plotted values are not in the extracted text.
- K and T were chosen by the authors per network to keep spread neither too sparse nor too dense (PDF p7), so the reported comparisons depend on those choices.
- Only nine networks are tested, and the paper does not test transfer to new networks, which it lists as future work (Conclusion, PDF p8).
- Node heterogeneity is absent: Eq. 1 applies one threshold K and one pair (p0, p1) to every node, with no node-level status or rank term. Paul's margin note asks about high-ranking members, and the text does not address that question.
- Internal inconsistencies: the 24.3% vs 24.2% figure for effective solutions (Sec. 5.1 vs 5.3, where 24.2% is described as a share of nodes, not solutions); and the ablation text says 'removing the 3 components altogether' while listing four components and an all-removed variant (Sec. 6.3).
- The definition of 'normalized influence' used as the outcome in Table 1 and Figs. 4-5 is not given in the extracted text.
- The explanations for DPIM and RL4IM instability are the authors' hypotheses and were not tested in the paper (PDF p7).
- Appendices A-E (proofs, hyperparameter ranges, network descriptions, MIP formulation, seed comparison) were not in the extract, so claims that depend on them cannot be checked here.
- The paper tests simulated cascades only; it does not validate the K-complex model against field adoption data, though it cites Centola 2010 and Romero et al. 2011 as empirical support.

## Short quotes

- "Can we use RL to address complex contagion influence maximization?" (Sec. 1 Introduction, PDF p2 (printed 5532); the paper's research question)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-allocation#Budget frontier with knee and electoral modes\|Budget frontier with knee and electoral modes]] | qualifies | Complex-contagion IM is non-submodular (Thm 1) and NP-hard (Thm 2). Greedy can be arbitrarily bad: normalised influence 0.0396 for Greedy vs 0.5347 for RL4CCIM on India (202 nodes), and 0.0293 vs 0.5585 on Exhibition. Deterministic setting p0=1, p1=0, budget T=8 (T=12 on Exhibition). (Sec. 3 PDF p3 (Thm 1 and 2); Table 1 and Sec. 6.1 PDF p7) | budget_frontier.py runs a lazy-greedy heap over a candidate pool with MAXSEEDS default 60 (line 7). Spread comes from an edge-keyed IC cascade over the P kernel (lines 83-94). The loop stops when the best marginal is zero (inside lines 161-172). The knee is the last seed whose gain is at least a quarter of the peak gain on the spread path (frontier_knee, lines 49-58, called at line 172). Both the stop rule and the knee presume diminishing returns. `fec/engine/budget_frontier.py:7,49-58,83-94,161-172` | The zero-marginal failure bites only if real cosponsor joining is threshold-like, so that single-seed marginals are zero until K partners are joined. Under the system's per-edge IC kernel the stop rule and knee are internally coherent. The paper's numbers are simulated cascades on non-legislative networks, so they do not transfer as estimates. | moderate | VERIFIED |
| [[systems/melange-allocation#Per-topic flip and coalition allocators\|Per-topic flip and coalition allocators]] | suggests-extension | Credit sparseness in an 8-node toy (K=3, T=4): only 17 of 70 four-seed sets (24.3 percent) are effective, and the seed sequence 1,2,3,5 earns per-step rewards 1,1,1,2, so a seed's value shows only alongside specific other seeds. (Sec. 5.1 PDF p4 for the numbers; Sec. 1 PDF p2 for the qualitative point) | per_topic_coalition.py takes the top 45 neutral members ranked first by a priority engagement score ev (line 114), and only then computes each candidate's solo marginal over the base spread (line 115). It keeps candidates with positive marginal (line 116), boosts them by (1 + 0.8 bpf) (line 117) and water-fills the budget. The funded set's joint spread is one extra MC evaluation, printed beside the naive-additive sum. The code comment asserts spread is submodular. `fec/engine/per_topic_coalition.py:114-127` | A joint gain above naive-additive on a topic would show complementarity that solo ranking cannot see. Under pure IC the joint should stay at or below naive-additive, so the printout is a real check only if threshold effects exist. The 45-candidate cut uses engagement rather than marginal value, so a seed valuable only in combination can be dropped before any marginal is computed. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Linear-threshold priming (cheapest tip)\|Linear-threshold priming (cheapest tip)]] | qualifies | K-complex model (Eq. 1): activation probability p0 when at least K neighbours are active and p1 much smaller otherwise. It is motivated by Centola and Macy 2007 and Centola 2010 but not validated on field adoption data in this paper; the paper cites succeeding studies as support. (Sec. 3 Eq. 1 PDF p3; Sec. 1 PDF p1 for the motivation; Sec. 2 PDF p2 for the cited validation) | lt_priming.py is a deterministic gap model. Its THETA ladder runs from 0.6 to 3.0 (lines 19-21), adjusted per member by net_lean/120 and a commitment term for Opponent cells. Pressure is the sum of P[u][v] times a cross-cue factor from pro neighbours. Money closes the rest, and recruits are cheaper pro neighbours (lines 96-145). Each target is solved alone and one hop deep. `fec/engine/lt_priming.py:19-21,96-145` | The paper supports only the modelling option that adoption can be threshold-like. It gives no K or threshold values for legislators. Joint multi-target priming would be a new model needing owner authorisation and a holdout look. | weak | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Best-sponsor, multi-target tree and budget max-coverage modes\|Best-sponsor, multi-target tree and budget max-coverage modes]] | qualifies | The (1-1/e) greedy guarantee of Kempe et al. 2003 applies to submodular IC and LT. Under complex contagion no such guarantee holds, and CCIM is NP-hard to approximate even under simplified network models (citing Schoenebeck and Tao 2019). (Sec. 1 PDF p2 (NP-hard approximation); Sec. 2 PDF p2 (Kempe guarantee); Sec. 3 PDF p3) | make_dashboard_html_00.tpl budgetRecruit (line 779) is greedy max coverage with coverage 1 - prod(1 - p) over recruiter edges. The comment at lines 773-776 claims the (1-1/e) guarantee. The closed-cascade route (line 832) is commented at line 831 as non-submodular with no (1-1/e) claim. The one live call (line 1126) passes target -1, so the target-distance multiplier (1 + 2 dT) is not applied there. `fec/surfaces/templates/make_dashboard_html_00.tpl:773-779,831,1126` | The guarantee is true of the coverage proxy only, not of real recruitment. Dashboard wording should say so if threshold joining is plausible. The target-weighted branch multiplies marginals by (1 + 2 dT), which lies outside the classic argument, so the guarantee should not be cited for targeted calls. | weak | VERIFIED |
| [[systems/melange-influence-engine#Cascade activation kernel build_P and partisan edge factors\|Cascade activation kernel build_P and partisan edge factors]] | context | IC and LT assume activation probability independent of how many neighbours are active, which keeps them submodular. Complex contagion breaks this (Sec. 3, lines 254-258 and footnote 2). (Sec. 3 PDF p3) | build_P (moneyball_core.py lines 555-562) sets each edge probability to x/(x+6), where x = w times cpf_diffusion times receiver edge_mult. None of these terms depends on how many in-neighbours are already active. cpf_diffusion (lines 543-545) uses a party term and a bridge factor bpf, which is network position, not a seniority or leadership term. `fec/moneyball_core.py:543-562` | Context only. The paper gives no legislative estimate. Any count-dependent kernel would be a new model and must keep the two cpf physics separate. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary says nine small networks. The paper's nine are five small shelter networks (CHANGE, DC, DOSIM, MFP, SPY) and four larger public networks (Football, Polbooks, India, Exhibition); India alone has 202 nodes (Sec. 6.1, PDF p7).
> - Summary issue: Summary says four additions address sparse rewards. The paper ties only the first three (solution filtering, reward-prioritised replay, global reward shaping) to the two sparseness causes. The GAT encoder is a representation change (Sec. 5.2, PDF p4).
> - Summary issue: Summary says the India ablation drops are 10.2 to 35.2 percent. Single-component drops are 4.6 (wo-PER), 9.3 (wo-RS), 10.2 (wo-SF) and 21.3 (wo-GAT) percent. Removing all components gives 35.2 percent (Sec. 6.3, PDF p7).
> - Summary issue: Summary says all coalition engines run an IC cascade with greedy or knee logic. The dashboard budget-recruit uses one-hop coverage probabilities, not a cascade. Per-topic coalition is a one-pass marginal ranking with water-fill, not greedy.
> - Summary issue: Summary and table use the SPY OPT value 0.5522 as a MIP optimum. The paper says the MIP exceeded its 12-hour limit from SPY onward, so the SPY OPT figure is unverified. The 6 to 8 percent gap claim is weaker for SPY than stated.
> - Summary issue: Gap item says greedy paths exist in per-topic coalition. That engine is not greedy (see bearing 2), so the gap wording should name only the budget frontier.
> - Summary issue: Bearing 2 said the top 45 candidates are ranked by solo marginal. The code ranks them by engagement score ev before computing marginals (per_topic_coalition.py line 114).
> - Summary issue: Bearing 2 paper_location cites Sec. 1 PDF p2 for the 17-of-70 and 24.3 percent figures. Those numbers appear only in Sec. 5.1 (PDF p4) and Sec. 5.3 (24.2 percent).
> - Summary issue: Coverage: the Thm 1 proof sits in Appendix A, which is absent from the extracted text. Thm 1 is verified only as a stated theorem, and Appendices B to E were not checked.
> - Summary issue: Paul note: the only annotation text is one FreeText item truncated mid-word; the paul_alignment reading is consistent with it but cannot be confirmed in full.

## Gaps and flags

- No test of simple vs complex contagion in cosponsorship: join hazard on CU bills as a function of the number of already-joined close partners (exposure curve) has not been measured.
- The greedy path in the budget frontier has no exact or exhaustive baseline on a small topic to bound their optimality gap if spread were non-submodular.
- Candidates with zero solo marginal are dropped before allocation (per_topic_coalition.py:116); seeds valuable only in combination are invisible by construction.
- The frontier knee is read as diminishing returns; a flat-then-jump curve, which threshold adoption produces, would be misread as an early knee or stop.
- No source-status (leadership or rank) term in the cascade kernel, the gap Paul's margin note raises; the paper's homogeneous K does not fill it.
- No conflict with D-025, D-021 or the rejected SNA proposals: the paper argues for neither dropping regular cosponsorship edges nor per-edge volume normalisation, and uses no LDA data.
- Any count-dependent cascade variant must keep the two cpf physics separate (architecture rule); this paper does not justify merging them.
- Paper-internal issues: SPY OPT 0.5522 is reported although the MIP is said to exceed 12 hours from SPY onward; 24.3 vs 24.2 percent effective-solution figures; ablation text says three components while listing four.
- Paul's only note is truncated mid-word (as varia), so its full intent cannot be confirmed.

Paul's entry has no takeaways or system links; his one margin note questions diminishing returns for high-ranking members and ties pressure susceptibility to some variable. That instinct agrees with the paper's central point that seed value need not diminish, though the paper models it through neighbour-count thresholds with homogeneous K, not node status. The note is not stale: the system as built still assumes diminishing returns in the budget frontier knee and coalition ranking, and its kernel has no leadership term.

## Related papers

- [[chen-2021-contingency-aware-influence]] — builds-on — RL4IM from that paper is the base architecture and a baseline in this paper (Sec. 5.2 and Sec. 6.1). receptivity_layer.py line 29 and budget_frontier.py line 73 cite the contingency-aware idea from the same group.
- [[kamarthi-2020-influence-maximization-unknown]] — cites — The paper cites Kamarthi et al. (AAMAS 2020) on learned graph sampling for IM in unknown networks. The item slug is dated 2019.
- [[arya-2025-relink-edge-activation]] — same-method — Deep RL for closed-network IM. fec/surfaces/templates/make_dashboard_html_00.tpl line 831 says the closed-cascade route follows RELINK and is non-submodular, the regime this paper treats.
- [[kumar-2022-influence-maximization-social]] — same-method — Graph embedding and GNN encoders for IM. This paper does not cite it, so the same-method link is an analyst judgment, parallel to the GAT encoder here.
- [[bharathi-nd-competitive-influence-maximization]] — complements — fec/engine/scenario.py lines 6 and 11 cite Bharathi, Kempe and Salek for the competitive IC cascade. This paper shows what greedy loses once cascades are threshold-based rather than IC.
- [[borodin-nd-threshold-models-competitive]] — complements — fec/engine/scenario.py lines 11-12 cite Borodin et al. for competitive LT, where submodularity can fail. This is the competitive threshold counterpart to the K-complex model here.
- [[golovin-2011-adaptive-submodularity-theory]] — complements — fec/engine/optimize.py line 108 keeps a Golovin-Krause data-dependent bound as a diagnostic. That bound presumes submodularity, the premise this paper shows fails under complex contagion.
- [[almazaydeh-2026-heuristic-centrality-methods]] — complements — Also challenges greedy influence maximization, from the heuristic-centrality side rather than RL.
- [[trivedi-2020-efficient-influence-maximization]] — complements — Efficient IM under the submodular IC model, the setting whose guarantees this paper shows do not carry to complex contagion.
- [[zareie-2023-influence-maximization-social]] — complements — Survey of behaviour-aware IM methods. receptivity_layer.py line 29 cites it alongside Chen UAI 2021 for contingency-aware seeding.

### Linked from
_None._