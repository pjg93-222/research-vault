---
type: "annotation-source"
source_zip: "Annotated Bibliographies & Literature Synthesis-20260705T013300Z-3-001.zip"
source_path: "Annotated Bibliographies & Literature Synthesis/Adaptative - Influence Maximization/Melange Annotated Bibliography — Addendum.docx"
sha12: "46c7c53ceb7f"
---

# Melange Annotated Bibliography — Addendum

## Sequential & Adaptive Influence and the Reinforcement-Learning Envelope

*This section extends the existing Influence-Maximization and RL-for-Graph-Combinatorial-Optimization entries into the dynamic regime. The organizing spine matches the architecture: one-shot greedy (already covered: Kempe et al. 2003; Nemhauser–Wolsey–Fisher 1978) →* ***adaptive greedy*** *(the bridge, with a survival guarantee for within-campaign replanning) → the* ***RL envelope*** *(for cross-campaign compounding, where no greedy guarantee reaches).*

### A. The adaptive bridge — subordinate greedy that replans

**Golovin, D., & Krause, A. (2011).** *Adaptive Submodularity: Theory and Applications in Active Learning and Stochastic Optimization.* Journal of Artificial Intelligence Research, 42, 427–486. arXiv:1003.3967. https://arxiv.org/abs/1003.3967

The single most load-bearing reference for the operational layer. Generalizes submodular set functions to *adaptive policies* — choose, observe the outcome, choose again — and proves that under adaptive submodularity a simple adaptive-greedy policy is competitive with the optimal policy, inheriting the classic (1 − 1/e) guarantee. Critically, the authors were the first to study influence maximization under the adaptive model, selecting seeds sequentially with each choice made after observing the spread of prior ones. This is the formal IM analog of D* Lite's incremental replanning: a greedy routine that re-optimizes as intelligence arrives, with a provable bound. The practical caveat — and the reason one still drifts toward learned methods — is that it presumes exact expected spread, which is NP-hard to compute in general.

**Leskovec, J., Krause, A., Guestrin, C., Faloutsos, C., VanBriesen, J., & Glance, N. (2007).** *Cost-effective Outbreak Detection in Networks.* KDD '07, 420–429.

Introduces CELF (Cost-Effective Lazy Forward selection), the practical engine that makes greedy IM tractable. It exploits submodularity directly: because marginal gains only decrease as the seed set grows, gains are recomputed lazily via a priority queue of stale upper bounds — only the top candidate is re-evaluated, and if it stays on top it is provably still optimal. Roughly two orders of magnitude faster than naive greedy with no loss of the (1 − 1/e) bound. The lazy-evaluation trick carries directly into the adaptive setting above.

### B. MDP / delayed-reward foundations

**Sutton, R. S., & Barto, A. G. (2018).** *Reinforcement Learning: An Introduction* (2nd ed.). MIT Press.

The canonical reference for the delayed-reward machinery underneath the envelope layer: temporal-difference learning, n-step returns, eligibility traces, and the temporal credit-assignment problem — i.e., which early relationship investments deserve credit when a downstream vote finally flips. Also the standard source for the MDP formalism ⟨S, A, P, R, γ⟩ and the Bellman optimality equation that captures compounding through the discounted value function.

**Ng, A. Y., Harada, D., & Russell, S. (1999).** *Policy Invariance Under Reward Transformations: Theory and Application to Reward Shaping.* ICML '99, 278–287.

The potential-based reward-shaping theorem. Establishes how to densify an otherwise-sparse terminal reward ("did you reach the target / flip the vote?") with intermediate signal *without* changing the optimal policy. This is the formal license for using marginal influence gain at each step as a shaping reward — the mechanism by which the subordinate greedy routine feeds the outer MDP.

**Watkins, C. J. C. H., & Dayan, P. (1992).** *Q-learning.* Machine Learning, 8(3–4), 279–292.

The value-learning method underneath every DQN-based IM paper in Section C. Included as the algorithmic root of the learned-policy approaches.

### C. Reinforcement-learning influence maximization — the non-submodular envelope

**Khalil, E., Dai, H., Zhang, Y., Dilkina, B., & Song, L. (2017).** *Learning Combinatorial Optimization Algorithms over Graphs.* NeurIPS '17. (S2V-DQN)

*Already listed under RL-for-Graph-Combinatorial-Optimization; cross-referenced here as the parent of the IM-specific cluster.* Learns a greedy-style construction policy over graph embeddings (structure2vec) via Q-learning, with influence maximization as one of its test problems. The template for learning navigation policies from one's own data.

**Manchanda, S., Mittal, A., Dhawan, A., Medya, S., Ranu, S., & Singh, A. (2020).** *GCOMB: Learning Budget-constrained Combinatorial Algorithms over Billion-sized Graphs.* NeurIPS '20. arXiv:1903.03332. https://arxiv.org/abs/1903.03332

The most direct realization of "a learned policy replacing the subordinate greedy at scale." GCOMB explicitly mimics the greedy algorithm and constructs solutions incrementally under a budget constraint — a GCN prunes to promising nodes, then a Q-learning component selects the seed set. Budget-aware and applicable to dynamic networks; on the IM case study it runs roughly 150× faster than the specialized IMM algorithm at comparable quality. The budget-constraint framing maps cleanly onto the political-capital budget in the orienteering formulation.

**Kamarthi, H., Vijayan, P., Wilder, B., Ravindran, B., & Tambe, M. (2020).** *Influence Maximization in Unknown Social Networks: Learning Policies for Effective Graph Sampling.* AAMAS '20, 575–583. arXiv:1907.11625. https://arxiv.org/abs/1907.11625 (Geometric-DQN)

The unknown-network case, formulated as an MDP and solved with a Geometric-DQN architecture (DiffPool for global state representation, DeepWalk for local action embeddings) trained by deep Q-learning. Directly analogous to the Melange situation where the true relationship/persuasion costs are not known a priori and must be revealed by probing — the agent learns *which nodes to query* to discover the network efficiently before seeding. Reports a 7–13% improvement over the best hand-crafted sampling heuristic.

**Chen, H., Qiu, W., Ou, H.-C., An, B., & Tambe, M. (2021).** *Contingency-Aware Influence Maximization: A Reinforcement Learning Approach.* UAI '21 (PMLR). arXiv:2106.07039. https://arxiv.org/abs/2106.07039

RL for IM when chosen seeds may decline to participate — sequential, observe-then-act seeding under uncertainty about whether any given activation actually "takes." The contingency framing maps onto legislators who may or may not move when approached, and onto lobbying outreach where commitment is probabilistic rather than guaranteed.

**Chen, H., Wilder, B., Qiu, W., An, B., Rice, E., & Tambe, M. (2023).** *Complex Contagion Influence Maximization: A Reinforcement Learning Approach.* IJCAI '23, 5531–5540. https://www.ijcai.org/proceedings/2023/614 (RL4CCIM)

The clearest published validation of the master/subordinate split, and the most important paper in this section for Melange's theory. Complex contagion — where a node's adoption probability rises sharply with the *number* of already-activated neighbors — is **non-submodular**, which breaks the diminishing-returns property that the greedy (1 − 1/e) guarantee depends on. The authors give the first RL approach to the problem and identify reward sparseness as the core obstacle, addressing it with a reward-shaping component (marginal-contribution rewards at episode end) and return-based prioritized experience replay. This is precisely why the outer MDP layer cannot be replaced by iterated greedy. Note the substantive implication: coalition formation in Congress is plausibly a complex contagion (a fence-sitter commits only once enough allies already have), which would place Melange's core problem *outside* the submodular regime and squarely in the RL envelope.

**Frontier (2024–2026).** Active work continues to push DRL-IM on scale and stability — e.g., cascade-aware GNN encoders paired with entropy-regularized (Munchausen) RL for large-scale networks (MaDGNN, *Scientific Reports*, 2026), and graph-embedding-plus-RL formulations addressing dynamic-network generalization. Worth tracking as the methods mature, but the four anchored entries above are sufficient to ground the architecture.

### How this slots into the existing Melange bibliography

- **Extends the Influence-Maximization section** from the static, one-shot Kempe et al. result into the adaptive (Golovin–Krause) and learned (Section C) regimes.
- *Connects to the operational layer (D Lite / LPA*):** adaptive submodularity is the influence-side analog of incremental replanning — both formalize "re-optimize cheaply as the world changes."
- **Connects to the strategic layer (MCTS / orienteering):** the RL-IM papers operationalize the same sequential, long-horizon decision problem that MCTS approximates by search, and that the orienteering frame describes as reach-while-collecting.
- **The unifying theoretical claim:** the inner static problem is submodular (greedy → 63%); the inner sequential problem is adaptive submodular (adaptive greedy → 63% of optimal policy); the full cross-campaign problem is non-submodular (RL4CCIM confirms this for complex contagion), which is where the durable competitive moat lives.
