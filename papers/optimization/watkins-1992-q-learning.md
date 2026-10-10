---
type: paper
title: "Q-Learning"
authors: ["Christopher J. C. H. Watkins", "Peter Dayan"]
year: "1992"
venue: "Machine Learning, 8, 279-292"
doi: ""
category: "Optimization"
systems: ["melange-allocation", "melange-influence-engine"]
components: ["melange-allocation.joint-optimizer", "melange-influence-engine.seed-gate"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/watkins-1992-q-learning.pdf]]"
original_paths: []
sha12: ["f99ce7c07b39"]
tags: [paper, optimization, type/paper, theme/optimization, evidence/verified]
---

# Q-Learning

**Christopher J. C. H. Watkins, Peter Dayan (1992).** *Machine Learning, 8, 279-292.* Theme: [[themes/optimization|Optimization]].

> [!abstract] Summary
> Watkins and Dayan (Machine Learning 8, 1992, pp. 279-292) give the convergence proof for one-step Q-learning, a model-free way to learn optimal action values in a finite controlled Markov process. The theorem (p.282) says Q_n(x,a) converges to Q*(x,a) with probability 1 if rewards are bounded, learning rates sit in (0,1) with divergent sums and finite sums of squares, values are kept in a lookup table, and every state-action pair is tried infinitely often. The proof builds an artificial action-replay process from the episode history. Lemma A shows Q_n is optimal for that process. Lemma B shows the process converges to the real one. Section 4 sketches two extensions: undiscounted problems with absorbing goal states, and updating many Q values per step. It also notes that replaying stored episodes moves the method toward certainty-equivalence planning. The proof gives no convergence rate and no finite-sample bound. It does not cover function approximation, which the authors say may fail to converge (p.282), or multi-step TD(lambda) updates. For Paul's systems this is background rather than a method in use. The repo has no reinforcement learning. The joint optimizer is a certainty-equivalence planner. It takes a modeled stance state and runs exact greedy allocation over it, and its list for observed outcomes is empty. The paper matters for one point: Paul expects a learned policy such as GCOMB to replace greedy allocation at scale. GCOMB and S2V-DQN use Q-learning with function approximation, which is outside this theorem. A lobbying cycle also cannot try each member-action pair the many times the theorem requires. An online scan found no applications of RL to lobbying or to allocating contributions across legislators.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Under what conditions does Q-learning, an incremental model-free method for learning optimal action-values in controlled Markov processes, converge to the optimal Q values?

**Theory:** Dynamic programming and Bellman optimality; Sutton's temporal differences (TD); stochastic approximation (Kushner and Clark 1978).

**Data:** None. Theoretical paper. Setting is a finite discrete controlled Markov process with finite state and action sets and bounded rewards; no sample, unit, N or period.

**Method:** Formal convergence proof. An artificial action-replay process (ARP) is built from the episode sequence. Lemma A (p.282, proof p.288) shows the Q_n values are optimal ARP action values, by backward induction. Lemma B (p.282-284, appendix p.289-291) has four parts: B.1 discounting of infinite sequences, B.2 probability of straying below a level, B.3 stochastic convergence of ARP transition and reward estimates via Kushner and Clark (1978), B.4 close rewards and transitions imply close values. Section 3.2 (p.284-285) combines these with epsilon bounds on s-step values.

**Identification:** Not statistical identification. Convergence holds with probability 1 under: every state and action tried infinitely often (p.281); bounded rewards; discount 0 < gamma < 1; learning rates 0 < alpha_n < 1 satisfying conditions on their sums and sums of squares (equation 3, formula garbled in extraction, p.282-283).

**Scope:** Finite discrete Markov decision processes with a look-up table representation of Q values; bounded rewards; discounting. Section 4 sketches (does not prove) two extensions: undiscounted processes with absorbing goal states (p.285-286), and updating many Q values per iteration (p.286). Excluded: function approximation and multi-step updates such as TD(lambda) (p.286). Era: 1992, following Watkins (1989).

## Key findings

- Theorem (p.282): with bounded rewards, learning rates in (0,1) meeting the stated conditions, and all state-action pairs sampled infinitely often, Q_n(x,a) converges to Q*(x,a) for all x and a, with probability 1. Section 3.2, p.284-285.
- Lemma A (p.282, appendix p.288): the Q_n values are the optimal action values of the ARP at every level n, proved by induction on n.
- Lemma B.3 (p.283-284, appendix p.290): ARP transition probabilities and expected rewards converge with probability 1 to those of the real process as the level grows.
- B.4 (p.283, appendix p.291): if transitions and rewards of s chained steps are within eta of the true values, the value of the s actions differs by at most a term growing quadratically in s, so close models give close values.
- B.1 (p.283, appendix p.289): with discount gamma below 1 and bounded rewards, the effect of truncating an infinite sequence after s actions tends to 0 as s grows.
- Section 4 (p.285-286): the theorem extends to undiscounted processes with absorbing goal states, because a bound on steps to trap and a minimum trap probability replace discounting. Sketched, not fully proved.
- Section 4 (p.286): updating many Q values per iteration preserves convergence with no non-trivial change to the proof, since the ARP still estimates the same quantities from more episodes.
- Section 4 (p.286): reusing stored episodes interpolates between Q-learning and certainty equivalence; in the limit of re-presenting old episodes infinitely often, reuse equals the certainty-equivalence step on the observed sample.
- Section 4 (p.286): the proof covers only a restricted form of Watkins (1989) because it does not allow updates from rewards of more than one iteration; the TD(lambda) extension does not follow trivially and may need other proof methods.

## Limitations

- Look-up table representation only; Watkins (1989) shows convergence may fail for other representations (p.282).
- Requires an infinite number of episodes for each starting state and action (p.281-282). The authors call this strong but argue no method could do better under the stochastic conditions.
- Convergence is asymptotic (n tends to infinity) with probability 1; no rates or finite-sample bounds are given.
- Undiscounted and multi-update cases are sketched only (p.285-286).
- Multi-step (TD(lambda)-style) updates are not covered by the proof (p.286).
- Several appendix formulas are garbled in the extracted text, so exact statements of equations 3 and the ARP definitions could not be confirmed from the text.
- Assumes the set of available actions is the same in every state; the authors note in footnote 1 that the general case extends straightforwardly.

## Short quotes

- "converges to the optimum action-values with probability 1"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-allocation#Joint CU budget optimizer\|Joint CU budget optimizer]] | qualifies | The convergence theorem needs a look-up table and an infinite number of episodes for every starting state and action (p.281). The look-up table caveat and the citation of Watkins 1989 (that Q-learning may not converge correctly for other representations) are on p.281. No rate or finite-sample bound is given. Section 4 (p.286) says reusing stored episodes moves Q-learning toward certainty-equivalence planning, which is the limit of re-presenting old episodes infinitely often. Certainty equivalence costs dearly early in learning, citing Barto and Singh 1990 (p.280). (Section 2 pp.281-282 (look-up table, infinite episodes, theorem); Section 4 p.286 (continuum to certainty equivalence); p.280 (certainty-equivalence cost)) | optimize.py docstring (lines 3-12): lazy greedy allocates a 2.5M pot in 500-dollar steps over a fixed modeled stance state and calls the result exact there. No learning step. observations=[] at line 58 is never appended to; it is only echoed in the output dict at line 220. Modeled threshold crossings (conversions, lines 133-144) trigger replanning and are not observed outcomes. `fec/engine/optimize.py:3-12; fec/engine/optimize.py:58; fec/engine/optimize.py:133-144; fec/engine/optimize.py:220` | The theorem covers only a finite MDP with look-up-table values and unbounded sampling, and this optimizer has neither. The item's claim that one cycle over 533 members yields at most a few observed outcomes per member-action pair is not in the code or the paper and is unverified here. GCOMB and S2V-DQN use n-step or fitted Q-learning with function approximation, which this paper does not cover (checked in their item files). The claim that exact greedy is optimal in a fixed state rests on the docstring's separability and diminishing-returns assertion, which I did not independently check. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Contingent seed activation gate\|Contingent seed activation gate]] | context | Q-learning needs each state-action pair sampled an infinite number of times (p.281). Lemma B.3 (p.284; proof p.290) says action-replay estimates of rewards and transitions converge with probability 1 as the level grows, under learning-rate sum conditions. This concerns estimating model quantities from unbounded samples, not learning a gate parameter. No finite-sample rate is given. (Section 2 p.281 (infinite episodes); Section 3.1 Lemma B.3 p.284; appendix p.290) | moneyball_core.py lines 626-630: SGATE is a literal hand-set take-up dict (Highly Persuadable 1.00 down to Opponent 0.35, default 0.85). gate() multiplies it by seed_prob with a floor of 0.15. These lines contain no update path from observed outcomes. No Chen 2021 reference appears in moneyball_core.py or optimize.py. `fec/moneyball_core.py:626-630` | Learning take-up rates would need repeated observed contact-to-action outcomes per stance tier, which the system does not log. The paper's result is asymptotic and gives no sample size. The link is conceptual: Lemma B.3 concerns action-replay transition and reward estimates, not gate probabilities. Weak strength is right. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Theorem conditions are garbled in the text extraction. The wording 'divergent sums and finite sums of squares' is inferred from the Kushner and Clark use in B.3, not read from the theorem itself.
> - Summary issue: Several page cites are off. The look-up table caveat and infinite-episode condition are on p.281, not p.282. Lemma A is stated on p.283 (proof p.288). B.3 and B.4 are on p.284, not p.283. The quadratic growth bound is on p.284.
> - Summary issue: The summary says the authors warn Q-learning may fail with function approximation. The paper attributes that warning to Watkins 1989 (p.281), citing it.
> - Summary issue: The claim that an online scan found no RL applications to lobbying or allocating contributions has no supporting record in the item and is unverified.
> - Summary issue: Paul's expectation that GCOMB replaces greedy allocation at scale, and his tie of SGATE to Chen et al. 2021, have no support in the item files or the code reviewed. Paul's position could not be checked because paul_entries is empty and no markup exists.
> - Summary issue: The claim that the repo has no reinforcement learning holds for fec/*.py: a search for Q-learning, qlearn, reinforcement learning and GCOMB found no matches.
> - Summary issue: The claim that the built allocator's exact-greedy optimality makes a learned policy useless at this size rests on the code docstring's own assertion, which was not independently checked.
> - Summary issue: The statement that Paul's position is not stale cannot be checked from these files.
> - Summary issue: Coverage: all 533 member-scale and optimizer assertions were checked only where code lines exist; the 533 count and the few-observations inference are unsupported here. No paul_markup items exist to drop, so none were dropped.

## Gaps and flags

- No observed-outcome feedback loop: optimize.py keeps an empty observations list, so every allocation is certainty-equivalence planning on modeled stance states. No sampled rewards ever update the model.
- SGATE take-up probabilities and PERS tiers are fixed priors. Nothing estimates them from logged contact outcomes, so no learning method (RL or otherwise) has data to work from.
- The allocator is single-cycle and one-shot. No multi-cycle sequential policy is modeled, which would be the setting where an MDP framing such as Q-learning applies.
- Paul's expectation that a learned policy (GCOMB) replaces greedy at scale has no stated convergence or sample-size basis. The tabular guarantee in this paper does not carry over to function-approximation Q-learning.

Paul has no entry or markup for this work. His nearest position is that a learned policy (GCOMB) should replace greedy allocation at scale. This paper qualifies that: the guarantee covers only lookup-table Q-learning with unbounded sampling. GCOMB uses function approximation, which may not converge (p.282). The built optimizer is a greedy planner that is exact in a fixed modeled state (optimize.py:3-12). So the replacement is a scaling preference, not a convergence-backed upgrade. The position is not stale, since no learned policy exists, but this source does not support it.

## Related papers

- [[sutton-2018-reinforcement-learning-introduction]] — complements — Real item (Sutton and Barto, 2018). Its content on the deadly triad and tabular versus function-approximation limits was not checked against the item text in this pass.
- [[manchanda-2020-gcomb-learning-budget-constrained]] — builds-on — Verified in the item: GCOMB trains seed selection with n-step fitted Q-learning over GCN-derived node scores. That is function-approximation Q-learning, outside this paper's theorem.
- [[dai-2017-learning-combinatorial-optimization-graphs]] — builds-on — Verified in the item: S2V-DQN uses n-step Q-learning with fitted Q-iteration over graph embeddings. That extends this paper's update to multi-step returns and function approximation, both outside the proof.
- [[chen-2021-contingency-aware-influence]] — builds-on — Real item: contingency-aware influence maximization as a reinforcement learning approach (title and authors confirmed). Whether its learning step is Q-learning was not verified in the item text.
- [[chen-2023-complex-contagion-influence]] — complements — Real item: complex contagion influence maximization as a reinforcement learning approach (title confirmed). The deep RL and Q-learning lineage claimed by the analyst was not confirmed, so the link is weakened to complements.
- [[arya-2025-relink-edge-activation]] — complements — Real item: RELINK, deep reinforcement learning for edge activation (title confirmed). Its Q-learning lineage was not verified, so the link is weakened to complements.
- [[golovin-2011-adaptive-submodularity-theory]] — complements — Real item (Golovin and Krause, 2011, adaptive submodularity). Its guarantee and sample-free claims were not checked against the item text; the link is conceptual.

### Linked from
- [[ng-1999-policy-invariance-reward-transformations]] — complements — Watkins and Dayan 1992 introduce Q-learning, the learner whose optimal Q is shifted by the corollary.