---
type: paper
title: "Policy invariance under reward transformations: theory and application to reward shaping"
authors: ["Andrew Y. Ng", "Daishi Harada", "Stuart Russell"]
year: "1999"
venue: "ICML (International Conference on Machine Learning); venue line not printed in the extracted text, hint kept from item"
doi: ""
category: "Optimization"
systems: ["melange-pathfinding"]
components: ["melange-pathfinding.member-recruitment-path", "melange-pathfinding.staffer-closeness-routes"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/ng-1999-policy-invariance-reward-transformations.pdf]]"
original_paths: []
sha12: ["94066dc12fe3"]
tags: [paper, optimization, type/paper, theme/optimization, evidence/verified]
---

# Policy invariance under reward transformations: theory and application to reward shaping

**Andrew Y. Ng, Daishi Harada, Stuart Russell (1999).** *ICML (International Conference on Machine Learning); venue line not printed in the extracted text, hint kept from item.* Theme: [[themes/optimization|Optimization]].

> [!abstract] Summary
> Ng, Harada and Russell (ICML 1999) ask which additions to an MDP reward leave the optimal policy unchanged. Their answer is potential-based shaping, F(s,a,s') = gamma*Phi(s') - Phi(s). It is sufficient for any real Phi on states. It is also necessary when nothing is known about the transitions or the reward: for any other F there is some MDP in which the shaped optimum is wrong. The corollary is that shaped Q and V equal the originals minus Phi(s), so the argmax over actions is the same. When Phi = V*, the shaped value is zero and greedy one-step choice is already optimal. A robustness remark adds that near-optimal policies carry over too. The experiments are small Sarsa grid worlds (10x10, 50x50, and a 5x5 grid with five subgoals). The paper reports no statistical tests, and this extraction lost the run counts. None of Paul's systems is a reinforcement learner. The bearing is on path-finding, where the deterministic case with gamma = 1 is the same thing as A* reduced costs, c'(u,v) = c(u,v) - h(u) + h(v). The theorem gives the rule for the learned or landmark heuristic Paul wants (his staffer and recruitment routers use heuristic 0 today). The heuristic must enter as a potential and be consistent; any other additive bonus changes which route wins. The staffer router's terminal normalization is a constant shift and is safe. Its member-context modifier and balanced step penalty change routes, which is correct as long as they are described as objective changes. One finding: the cosponsor recruitment path follows a greedy rule over a distance-to-target table that ignores dropped members. The template still calls this path provably optimal. UNVERIFIED until reproduced.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Under what conditions can the reward function of a finite Markov decision process be modified (reward shaping) without changing which policies are optimal, especially when the learner does not know the transition function or the original reward?

**Theory:** Bellman optimality for finite MDPs (discounted, and undiscounted with an absorbing goal state). A shaping reward F(s,a,s prime) is added to the original reward R. The paper defines potential-based shaping F(s,a,s prime) = gamma*Phi(s prime) - Phi(s) for an arbitrary real function Phi on states.

**Data:** No empirical dataset. Simulated grid-world MDPs: a shortest-path goal task on a 10x10 grid, the same task on a 50x50 grid, and a 5x5 grid in which five subgoals (flags) must be visited in order before the final goal. Start and goal are in opposite corners for the goal-only grids. Unit of analysis: learning trial and steps taken to reach the goal. Number of independent runs used for averaging is not recoverable from the extraction.

**Method:** Formal analysis: definitions, a sufficiency proof (potential-based F leaves optimal policies unchanged, with Q and V shifted by Phi), a necessity proof for the case of no prior knowledge of T and R (Appendix A, fully worked for finite states and two actions), and robustness remarks. Simulation: Sarsa and Sarsa(lambda) with epsilon-greedy exploration on the grid worlds, comparing no shaping against shaping with a Manhattan-distance potential and, in the subgoal task, a subgoal-count potential and a fine-tuned estimate of V*.

**Scope:** Finite-state, finite-action MDPs with deterministic, bounded rewards. The paper argues infinite-state results follow under suitable regularity conditions but defers those proofs to a full version. Era: 1999 reinforcement-learning practice. Population: learning agents in small simulated domains; no real-world system.

## Key findings

- Potential-based shaping F(s,a,s prime) = gamma*Phi(s prime) - Phi(s) is sufficient: every optimal policy of the shaped MDP is optimal in the original MDP, and vice versa. Location: Main results, Theorem, sufficiency part and its proof.
- Necessity: absent prior knowledge of T and R, potential-based F is the only shaping form that guarantees consistency with the optimal policy; for any non-potential F there exist transitions and rewards such that no optimal policy of the shaped MDP is optimal in the original. Location: Main results, Theorem, necessity part; proof in Appendix A (finite states, two actions).
- Corollary: Q prime*(s,a) = Q*(s,a) - Phi(s) and V prime*(s) = V*(s) - Phi(s), so the shaped problem has the same argmax over actions. With Phi zero at the absorbing state the identities hold exactly in the undiscounted case. Location: Main results, Corollary.
- Robustness remark: the identities hold for arbitrary policies, not only optimal ones, so near-optimal policies are also preserved and an epsilon-optimal policy learned on the shaped MDP is epsilon-optimal in the original. Location: Main results, Remark (Robustness and learning).
- Remark: if the original reward is itself potential-based, every policy is optimal in the original MDP, so potential-based shaping gives no reason to prefer one policy over another. Location: Main results, Remark (All policies optimal under potential).
- Choosing Phi = V*(s) makes the shaped optimal value function identically zero, which the authors describe as an easy function to learn, leaving only the non-zero Q-values to learn. Location: Main results, paragraph after the Corollary.
- On the 10x10 goal grid, a Manhattan-distance potential speeds learning substantially; on the 50x50 grid, unshaped learning is far slower, with the authors saying it is losing hopelessly to the potential-based version. Location: Experiments, first two grid-world figures; the step axes run to about 600 (10x10) and about 2.5 x 10^4 (50x50).
- Even a crude potential (zero, or a distance-based estimate) still helps the early phase of learning, while the finely tuned V*-based potential does better still. Location: Experiments, first grid-world discussion.
- Subgoal task (5x5 grid, five flags visited in order): a potential that counts achieved subgoals, scaled by an expected time per subgoal, gives a reward jump at each subgoal and speeds learning clearly over no shaping; the fine-tuned V*-estimate potential performs better still. Location: Experiments, subgoal figure and text; the trial axis runs to about 450 and the step axis to about 800.
- Non-potential shaping can create positive-return cycles: in the bicycle example (Randlov and Alstrom), an agent rewarded for progress toward the goal but not penalized for moving away learned to ride in tiny circles; a soccer robot rewarded for touching the ball learned to stay next to it and vibrate. Location: Introduction and Shaping rewards section.
- Discussion: the authors suggest that non-potential shaping may still help in practice when used with care, but carries no guarantee; learning a rough shaping function as a multiscale value-function approximation is named as promising future work. Location: Discussion and Conclusions.
- Extensions stated without full proof: for semi-MDPs with variable action durations tau the potential form generalizes to F = gamma^tau*Phi(s prime) - Phi(s); policy-class-restricted learning also preserves the guarantee; positive rescaling of rewards is allowed. Location: Discussion and Conclusions; Shaping rewards section.

## Limitations

- Fully rigorous proofs only for finite state spaces; infinite-state extensions are asserted, with bounded shaping rewards required, and the full proofs are deferred to a longer paper.
- Necessity proof is given in full only for two actions; the general case is described as obvious but tedious.
- The guarantee covers optimal policies under the stated assumptions; for undiscounted problems with non-potential shaping the authors concede no general guarantee.
- Empirics are limited to small simulated grid worlds and a subgoal task; there is no real-world or high-dimensional domain, no statistical tests, and the number of runs is not recoverable from the extraction.
- Only Sarsa and Sarsa(lambda) are tested; there is no comparison with other shaping schemes or with learned shaping functions.
- Non-potential shaping is discussed qualitatively; the paper does not quantify its harm or benefit.
- Text extraction garbles several numeric parameters, so exact learning rates, exploration rates and slip probabilities are unknown from this copy.

## Short quotes

- "one can add a reward for transitions between states"
- "any other transformation may yield suboptimal policies unless further assumptions are made about the underlying MDP"
- "Often, a very simple pattern of extra rewards suffices to render straightforward an otherwise completely intractable problem"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-pathfinding#Sponsor-to-target max-product path and chain hold\|Sponsor-to-target max-product path and chain hold]] | qualifies | Corollary: shaped Q' = Q - Phi and V' = V - Phi, with Phi(absorbing) = 0 in the undiscounted case, so the shaped and original problems share the same argmax over actions. Remark (Robustness and learning): the identities hold for arbitrary policies, so an epsilon-accurate potential keeps policies epsilon-near-optimal. Choosing Phi = V* makes V' identically zero, which the authors call an easy value function to learn. (Main results: Corollary; Remark (Robustness and learning); paragraph after the Corollary on Phi = V*) | buildPath (line 753) calls distToTarget(target) (line 734), a max-product proximity over the full PE graph that does not exclude dropped members. tracePath (line 740) takes the argmax of PE[cur][v] times dT[v] over unseen, non-dropped neighbours. With no drops this greedy step is exact because dT satisfies the Bellman equation. With a drop, dT still counts paths through the dropped member, so the greedy step can follow a dead end or a weaker chain. The comment at line 735 says PROVABLY-OPTIMAL and is wrong once a member is dropped. A toy replica of these functions (scratchpad folder ng1999, file trap2.js) gave chain product 0.009 where the best drop-avoiding chain is 0.36 (case 1), and reached=false while a path exists (case 2). This replicates the logic only, not the browser page. `fec/surfaces/templates/make_dashboard_html_00.tpl:734, :735, :740, :753-756; drop input at :1165` | Deterministic max-product over per-ask probabilities, equivalent to a -log p shortest path (comment at line 735). Applies only when the dropped set is non-empty. Drops are set only from the drop text box in applyShocks (line 1165); the URL parameter read at line 1537 is coal, not drops, so the earlier URL claim is removed. The browser-level failure stays UNVERIFIED under DE-062; the logic failure is reproduced in a replica. | strong | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | method-source | Theorem (sufficiency and necessity): adding gamma*Phi(s') - Phi(s) for any real Phi leaves every optimal policy unchanged, and without prior knowledge of T and R no other additive shaping term guarantees this. The paper does not discuss A*, heuristics used in search, or bidirectional search. (Main results: Theorem (sufficiency and necessity); Appendix A) | staffer_routing.py:386 and staffer_closeness_routes.py:79 both state Bidirectional Dijkstra with heuristic 0, with terminal costs and a virtual sink (search code from line 511). No heuristic is in use. The theorem covers changing the objective (rewards or arc costs). A search heuristic in A* changes search effort, not the objective, so it lies outside the theorem. Its correctness depends on admissibility or consistency, which is standard A* theory and not in this paper. The potential condition applies only if a heuristic is folded into arc costs as a reweighting, where telescoping keeps route ranking. `fec/surfaces/staffer_closeness_routes.py:79, :511; fec/surfaces/staffer_routing.py:386` | Holds for deterministic additive costs. Bidirectional search, the 10^12 tie band and Yen ranking are outside the paper and need their own checks in original cost units. The premise that Paul wants learned or landmark heuristics is not in the item (paul_entries is empty) and is unverified here. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | qualifies | Necessity: for any non-potential additive shaping term there exist dynamics and rewards under which the shaped optimum is not optimal in the original problem (Appendix A proves the two-action case). A constant added to every terminal arc telescopes to a potential term Phi(goal) and leaves route ranking unchanged. A term that varies by staffer or per step is not a potential and can change the winner. (Main results: Theorem (necessity part); Appendix A) | Lines 913-918: modifier = 1 + MEMBER_BETA*value, with MEMBER_BETA = 0.15 (closeness_math.py:21). Dividing by (1 + MEMBER_BETA) is a constant shift on every terminal arc when an office is set, so the code comment at line 916 about ranking is correct for that step. Line 919 also adds uncertainty_weight times the anchor uncertainty cost, which varies by target and is not a constant shift; the earlier draft omitted it, and it can change the winner. The modifier is the per-staffer term that is not a potential. In staffer_routing.py:424-425 the balance step penalty is added in the ranking key among candidate routes, a per-step term that is not a potential and changes the winner among candidates. `fec/surfaces/staffer_closeness_routes.py:913-919; fec/surfaces/staffer_routing.py:424-425; fec/staffer_network/closeness_math.py:21` | Deterministic costs. The code calls the modifier member_context_max_bonus (line 1014) and comments prestige bonus (line 917). D-025 is not cited in this file, so the D-025 wording is not verified here. | moderate | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: summary_md: the statement that the greedy one-step choice is already optimal when Phi = V* is an inference (V' = 0 implies Q' equals expected immediate reward); the paper does not state it.
> - Summary issue: summary_md: the claim that the theorem gives the rule for the learned or landmark heuristic Paul wants overreaches; the theorem covers objective changes, not search heuristics (see bearing 2).
> - Summary issue: summary_md: the staffer normalization is called a constant shift and safe, but the terminal cost also adds a target-specific uncertainty term (closeness routes line 919), so the summary is incomplete.
> - Summary issue: summary_md: the Ng, Harada and Russell venue (ICML 1999) comes from item metadata; the extracted text does not print a venue line.
> - Summary issue: summary_md: the statements about what Paul wants, the note that the paper bears on his centerless or node-loss concern, and the heuristic-0 status of recruitment routers rest on Paul intent not in the item (paul_entries is empty); unverified here.
> - Summary issue: summary_md: for any other F should read non-potential F; the paper's necessity claim is for proper transitions and bounded rewards.
> - Summary issue: ext key_findings 8: the fine-tuned V* result is placed in the grid-world experiments, but the paper tests a V*-based potential only in the subgoal task (Figure b). The zero potential is in fact a scaled Manhattan potential (0.0 times Phi0, scale unreadable), not a zero potential.
> - Summary issue: ext limitations: the authors' concession of no general guarantee refers to using an undiscounted-form potential in a discounted problem (Discussion), not to undiscounted problems with non-potential shaping.
> - Summary issue: rel related: the Duan 2025 link is dropped; its text has no reweighting or potential content, so the stated why is unsupported.
> - Summary issue: Extraction caveat: digits and some symbols (gamma, learning rates, run counts, slip probabilities) are garbled in the text file; the number of averaging runs cannot be checked.

## Gaps and flags

- Recruitment-path greedy trace uses a distance-to-target table computed without the dropped-member set (make_dashboard_html_00.tpl:734 vs 740). After a drop the chain may be suboptimal or falsely unreachable, yet it is labelled provably optimal. The lazy fix is to skip dropped nodes inside distToTarget. UNVERIFIED (DE-062).
- No router in the system uses a heuristic yet (staffer routes heuristic 0, recruitment path plain Dijkstra). Paul's wanted learned or landmark heuristic has no spec stating that it must enter as a consistent potential, with ties and the stop rule checked in original cost units.
- The staffer router's member-context modifier and balanced step penalty are objective changes. No test pins how often they change the winning route compared with the pure -log strength objective.
- The recruitment-path comment 'PROVABLY-OPTIMAL' (make_dashboard_html_00.tpl:735) does not hold when members are dropped. This is new agent analysis, UNVERIFIED until a second path reproduces it (DE-062).
- Route costs and chain hold are heuristic model outputs, not introduction or access probabilities (D-025). Nothing in this paper licenses reading them as such.

Paul has no annotations on this work. It fits his stated wish for A* or learned heuristics on the recruitment and staffer routers. The paper gives the correctness rule for that wish: a heuristic must act as a potential to leave route choice unchanged. His component positions are current, since both routers still run with heuristic 0. The paper also bears on his centerless or node-loss concern: when a node is removed, a precomputed V* potential is stale. That is exactly the dropped-member gap found in the recruitment path.

## Related papers

- [[sutton-2018-reinforcement-learning-introduction]] — complements — The 2018 text discusses potential-based shaping and cites Wiewiora 2003 alongside it, so it complements this paper's shaping theory.
- [[watkins-1992-q-learning]] — complements — Watkins and Dayan 1992 introduce Q-learning, the learner whose optimal Q is shifted by the corollary.
- [[chen-2021-contingency-aware-influence]] — complements — Chen et al. 2021 have a reward-shaping component (REWARD SHAPING section) based on marginal contribution, not a potential-based form, so the Ng condition bears directly on it. The earlier needs-the-potential-condition wording is an inference, not a finding in Chen.

### Linked from
_None._