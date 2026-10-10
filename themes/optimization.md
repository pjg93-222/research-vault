---
type: theme
name: "Optimization"
paper_count: 5
tags: [theme, type/theme]
---

# Optimization

**Optimization: 5 items, 4 of them substantive.** The four papers are two theory papers ([[watkins-1992-q-learning]], [[ng-1999-policy-invariance-reward-transformations]]) and two learned-heuristic papers ([[dai-2017-learning-combinatorial-optimization-graphs]], [[manchanda-2020-gcomb-learning-budget-constrained]]). The fifth item, [[anon-nd-untitled-document-single]], is a link stub with no text.

The theme asks one question: **when does a cheap step-by-step rule (greedy, or greedy guided by a heuristic or a learned score) give the same answer as the true optimum, and what does learning that rule add?** All four substantive papers point to the same practical answer for Melange. The exact greedy and CELF lazy-greedy allocators are the right tools at the system's scale (one graph of about 533 members per Congress). The learned-policy line (S2V-DQN, then GCOMB) has no guarantees. It assumes many training graphs drawn from one distribution, which Melange does not have, and its claimed benefit is speed on graphs with millions of edges, not better answers. The theory papers set the limits: Watkins' convergence proof covers lookup tables only and needs every member-action pair tried infinitely often, and Ng's invariance result says which route-score additions are safe.

**Online scan (2026-10-10):**
- **The stub's link is now resolved (UNVERIFIED as the intended paper).** The link points to FAIA vol. 397, *Social Robots with AI: Prospects, Risks, and Responsible Methods* (Robophilosophy 2024, eds. Seibt, Fazekas, Quick; IOS Press 2025; about 740 pages including about 18 pages of front matter). Allowing for that offset, PDF page 583 falls on printed page 565. That is inside Jakob Stenseke, "Optipolitics: Is Politics an Optimization Problem?" (pp. 563-575). It asks whether politics can be framed as a mathematical optimization problem and tests a liberal-democratic version against eight challenges. That fits the folder name. The chapter itself needs an IOS Press login; the direct PDF link returned the login page, so no copy was saved.
- **Benchmark downloaded:** Liang, Yang, Ke, Xiao and Gao, "A Benchmark Study of Deep-RL Methods for Maximum Coverage Problems over Graphs" (PVLDB 17(11), 2024; arXiv 2406.14697). Saved to `C:\Users\pgoff\Documents\research\_raw\dl_liang2024\liang-2024-benchmark-deep-rl-maximum-coverage.pdf`. It is not yet extracted into the vault.

## Strands

### Optimality guarantees for sequential decision rules (MDP theory)

Both papers prove that something is preserved, and both hold only under narrow conditions.

- **[[watkins-1992-q-learning]]** proves that one-step Q-learning reaches Q* with probability 1. The conditions are bounded rewards, learning rates that meet the Robbins-Monro style conditions, values kept in a lookup table, and every state-action pair visited infinitely often. The proof gives no convergence rate and no finite-sample bound, and it excludes function approximation, which the authors say may fail to converge (p.282).
- **[[ng-1999-policy-invariance-reward-transformations]]** proves that potential-based shaping, F = gamma*Phi(s') - Phi(s), leaves the optimal policy unchanged (the argmax over actions is the same). If nothing is known about the transitions or the reward, it is the only shaping that guarantees this. With Phi = V*, a one-step greedy choice is already optimal.

**How they fit together:** Watkins says when the learned Q is the true Q*. Ng says which reward changes leave the argmax over Q* unchanged.

**Why it matters to Paul:** he runs no reinforcement learner. What carries over is the deterministic case with gamma = 1, where Ng's shaping is the same thing as A* reduced costs. A heuristic added to a route must enter as a consistent potential, or it changes which route wins.

**Weaknesses:**
- Ng's experiments are small Sarsa grid worlds with no statistical tests.
- Watkins' extension to undiscounted problems with absorbing goal states is sketched, not proved.

Works: [[watkins-1992-q-learning]], [[ng-1999-policy-invariance-reward-transformations]]

### Learned greedy heuristics for graph combinatorial optimization

**The approach.** [[dai-2017-learning-combinatorial-optimization-graphs]] (S2V-DQN) turns greedy into a template: a learned score picks the next node, trained with n-step fitted Q-learning over structure2vec graph embeddings. [[manchanda-2020-gcomb-learning-budget-constrained]] (GCOMB) adapts this to budget-constrained coverage and influence maximization (IM). A GCN learns probabilistic-greedy scores, a noise predictor prunes nodes, and Q-learning with a locality term picks seeds.

**Agreement.** Both papers' own evidence says learning helps least where greedy is already strong:
- Dai: on set cover, S2V-DQN scores 1.001 against LP's 1.0009, and plain greedy is only about 3% worse (1.03). The gains come on minimum vertex cover and MaxCut.
- GCOMB: on small graphs greedy reaches at least 99% of the CPLEX optimum (BP-500 at b=20: greedy 0.99, GCOMB 0.998, optimum 1). On real graphs GCOMB's quality only matches greedy. Its stated payoff is speed: about 10x faster than greedy, 2.0-4.0x faster than CELF on YouTube for budgets b=20 to 200, and it finishes on billion-edge graphs where IMM runs out of memory.

**Contested (external evidence, not yet in the vault).** The Liang et al. 2024 VLDB benchmark contradicts GCOMB's speed claim on max coverage. Lazy greedy ran one to two orders of magnitude faster than GCOMB at inference, before counting GCOMB's training time. It solved the billion-node Friendster graph in minutes, while GCOMB crashed. Its inference memory was at least 3x lower. Lazy greedy beat all five deep-RL methods on quality, speed and memory for max coverage. For IM, IMM and OPIM generally beat the deep-RL methods. Liang also reports that GCOMB's runtime does not grow steadily with the budget, which it attributes to an unstable node pruner.

**Shared weaknesses:**
- Both assume many training graphs drawn from one distribution.
- Neither gives guarantees.
- Dai compares GPU runs against CPU baselines.
- GCOMB's IM objective is single-colour independent cascade scored with 10,000 Monte Carlo runs.
- GCOMB explicitly excludes sequential and capacity-constrained problems (Sec 5).

Works: [[dai-2017-learning-combinatorial-optimization-graphs]], [[manchanda-2020-gcomb-learning-budget-constrained]]

### Politics as an optimization problem (unresolved stub)

[[anon-nd-untitled-document-single]] has no text, only a link. The online scan places that link inside Stenseke's "Optipolitics: Is Politics an Optimization Problem?" (FAIA 397, pp. 563-575). This is UNVERIFIED and rests on the front-matter offset. Judging only from the abstract, it is a normative and conceptual paper, not an algorithmic one. It asks whether governance can be posed as an optimization problem and tests a liberal-democratic version against eight challenges. Its conclusion is that none can be fully overcome today, but many are just as hard for representative democracy. It would be the only work in the theme that questions the framing that every Melange allocator takes for granted. Nothing beyond the abstract can be claimed until the chapter is in hand; it needs an IOS Press login.

Works: [[anon-nd-untitled-document-single]]

## Agreements and debates

**1. Does a learned policy beat greedy on budgeted coverage?**
- [[manchanda-2020-gcomb-learning-budget-constrained]] says it does on speed but not on quality: GCOMB matches greedy and runs about 10x faster than greedy and 2-4x faster than CELF on YouTube.
- Liang et al. 2024 (not in the vault) contradicts the speed result. Lazy greedy beats GCOMB on quality, speed and memory for max coverage.
- [[dai-2017-learning-combinatorial-optimization-graphs]] already showed a near-tie on set cover (S2V-DQN 1.001, LP 1.0009, greedy 1.03).
- So the only uncontested gain of the learned line is on non-submodular problems such as minimum vertex cover and MaxCut, where hand-written heuristics are weak.

**2. Proven guarantees versus results that only hold empirically.**
- [[watkins-1992-q-learning]] and [[ng-1999-policy-invariance-reward-transformations]] give exact results under narrow conditions: lookup tables, every pair visited, potential-form shaping.
- S2V-DQN and GCOMB step outside every one of those conditions (function approximation, multi-step returns, few visits) and offer no replacement guarantee.
- No paper in the theme bridges the two.

**3. Is the learned score a shaping term or an objective change?**
- Ng says any additive bonus that is not a potential can change which policy is optimal.
- Learned greedy scores (Dai, GCOMB) are not constrained to be potentials. They are approximations of Q, not shaping terms, so Ng's invariance does not protect them.
- In Melange the same distinction separates safe changes from objective-changing ones in the route finders. The staffer router's terminal normalization is a constant shift and is safe. Its member-context modifier and step penalty change which route wins.

**4. No rejected proposal is endorsed.** None of the five works argues for dropping regular cosponsorship edges or for per-edge volume normalization. GCOMB's locality term counts newly covered nodes; it does not normalize edge volume.

## Methods

**Formal proof** ([[watkins-1992-q-learning]], [[ng-1999-policy-invariance-reward-transformations]]).
- Watkins proves convergence through an artificial action-replay construction plus stochastic-approximation lemmas.
- Ng gives a sufficiency proof and a necessity proof, the latter fully worked only for finite states and two actions.
- Weaknesses: no rates or finite-sample bounds, and only token experiments. Ng's grid worlds have no statistical tests, and this extraction lost the run counts.

**Empirical benchmarking of learned heuristics** ([[dai-2017-learning-combinatorial-optimization-graphs]], [[manchanda-2020-gcomb-learning-budget-constrained]]).
- Both report approximation ratios against CPLEX or best-known solutions, plus speed and generalization from small training graphs to larger test graphs.
- Weaknesses:
  - Hardware is not matched (GPU against CPU).
  - Speed figures count inference only and leave out training. Liang et al. 2024 report that lazy greedy wins even with that advantage handed to the learned methods.
  - Instances come from one distribution.
  - Liang reports evaluation gaps in GCOMB's own baselines and its noise-predictor evaluation.
  - IM is scored as single-colour independent cascade with 10,000 Monte Carlo runs, a different objective from Melange's two-colour competitive cascade evaluated at 20 Monte Carlo runs.
  - Results are reported as point ratios with no spread across seeds.

**Fitted Q-learning over graph embeddings** is the shared method of the learned line. Its convergence is not covered by Watkins' theorem.

## How the works relate

- [[dai-2017-learning-combinatorial-optimization-graphs]] — builds-on — [[watkins-1992-q-learning]]: S2V-DQN trains its greedy score with n-step fitted Q-learning. That extends the Q-learning update to multi-step returns and function approximation, both outside Watkins' tabular proof. The link is conceptual: Dai cites Sutton and Barto and Riedmiller, not Watkins.
- [[manchanda-2020-gcomb-learning-budget-constrained]] — builds-on — [[dai-2017-learning-combinatorial-optimization-graphs]]: S2V-DQN is GCOMB's main learned baseline (Table 2a, BP-2k at b=15: GCOMB 0.89, S2V-DQN 0.87). GCOMB keeps the learned-score greedy template and adds budget constraints, pruning and locality. S2V-DQN runs out of memory above 20,000 nodes.
- [[manchanda-2020-gcomb-learning-budget-constrained]] — builds-on — [[watkins-1992-q-learning]]: GCOMB's stage 2 is n-step fitted Q-learning (n=2, gamma 0.8) over GCN scores. That is function-approximation Q-learning, which the convergence theorem does not cover.
- [[ng-1999-policy-invariance-reward-transformations]] — complements — [[watkins-1992-q-learning]]: Both are tabular MDP theory. Watkins guarantees that the learned Q reaches Q*. Ng shows that potential-based shaping shifts Q* by Phi(s) and leaves its argmax unchanged, so a shaped Q-learner reaches an equivalent optimum.
- [[ng-1999-policy-invariance-reward-transformations]] — contrasts — [[dai-2017-learning-combinatorial-optimization-graphs]]: Ng shows that greedy one-step choice is optimal when the potential equals V*, and that additions which are not potentials can change the optimum. Dai learns an approximate Q for greedy construction with no potential constraint, so its scores carry no invariance or optimality guarantee. This is a conceptual link; neither paper cites the other.
- [[ng-1999-policy-invariance-reward-transformations]] — contrasts — [[manchanda-2020-gcomb-learning-budget-constrained]]: GCOMB's locality term adds to the learned score and is not potential-based. By Ng's result such additions change the objective rather than reshape it, so they belong among objective changes, not heuristics that leave the answer unchanged. This is conceptual; there is no citation.

## What it means for the systems

**Joint optimizer (CELF lazy greedy, `fec/engine/optimize.py`)** — [[systems/melange-allocation]]
- *Recommendation: keep it.* [[dai-2017-learning-combinatorial-optimization-graphs]], [[manchanda-2020-gcomb-learning-budget-constrained]] and the external Liang 2024 benchmark all point to exact greedy or lazy greedy being at least as good at Melange's scale (bearing: qualifies, VERIFIED-SOFTENED). GCOMB's only reported gain is speed, it appears only on graphs with millions of edges, and Liang contradicts it.
- *A planned GCOMB-style learned policy has no support in this literature.* Three reasons:
  - Melange has one graph per Congress, not a distribution of training graphs.
  - The per-member PMAX cap is a capacity constraint, which GCOMB explicitly excludes (Sec 5).
  - The system's objective is a two-colour competitive cascade scored at MC 20, not single-colour independent cascade.
- *A planned move to learned Q-values runs into [[watkins-1992-q-learning]].* A lobbying cycle cannot try each member-action pair the many times the theorem needs. The optimizer is also a certainty-equivalence planner whose list of observed outcomes is empty, so there is nothing to train on yet.

**Budget frontier and budget max-coverage recruiter (budgetRecruit)** — [[systems/melange-allocation]]
- The recruiter descends directly from GCOMB (bearing: method-source, VERIFIED-SOFTENED). It keeps GCOMB's locality idea (count only newly covered members) and a degree prune, but uses exact greedy, which GCOMB's own small-graph results support (bearing on the budget frontier: qualifies, VERIFIED).
- Frontier and coverage figures should name their budget window and member denominator, for example "of 533 members, 119th Congress".

**Route finders: staffer routes, cosponsor recruitment, closedCascadeRoute** — [[systems/melange-pathfinding]]
- Today these use heuristic 0. [[ng-1999-policy-invariance-reward-transformations]] sets the rule for any landmark or learned heuristic that may come later: it must enter as a consistent potential, c'(u,v) = c(u,v) - h(u) + h(v). Anything else changes which route wins (bearings: method-source and qualifies, VERIFIED-SOFTENED).
- The staffer router's terminal normalization is a constant shift and is safe. Its member-context modifier and balanced step penalty change which route wins. They should be documented as changes to the objective, not as neutral heuristics.
- Route scores remain heuristic model outputs, never closeness, access or introduction probabilities.
- **UNVERIFIED lead (single agent):** the cosponsor recruitment path follows a greedy rule over a distance-to-target table that ignores dropped members, yet the template still calls the path "provably optimal". This should be reproduced before any wording or code change.
- [[dai-2017-learning-combinatorial-optimization-graphs]] marks the only plausible room for a learned score: the route heuristics that are not submodular (bearing on cosponsor-route-pareto: suggests-extension, VERIFIED-SOFTENED). Even there the training data does not exist.

**Seed gate (influence engine)** — [[systems/melange-influence-engine]]: context only. Watkins is background (VERIFIED-SOFTENED).

**Framing.** If the stub is Stenseke's "Optipolitics", it is the one work that questions treating legislative influence as an optimization target at all. It bears on how the allocation layer is described, not on how it is computed (UNVERIFIED).

## Gaps

**Missing anchor papers**
- **CELF itself (Leskovec et al. 2007) and the Kempe-Kleinberg-Tardos submodularity result.** These are the actual basis of the shipped optimizer. The Kempe item sits in other themes and has a possible year mismatch (2015 against the 2003 original).
- **Capacity-constrained and matroid-constrained submodular maximization.** This is the setting of the PMAX cap. GCOMB excludes it, and no work in the theme covers it.
- **Competitive or two-colour influence maximization.** It is cited only in passing, via a related link from GCOMB.
- **Adaptive submodularity (Golovin and Krause 2011).** It is the optimizer's diagnostic upper bound but is not in this theme.

**Benchmarks and IM algorithms**
- **Liang et al. 2024 is downloaded but not extracted.** It is the strongest single piece of evidence on the learned-versus-greedy question and should become a vault item with a `contradicts` link to [[manchanda-2020-gcomb-learning-budget-constrained]].
- **IMM and OPIM.** These are the algorithms the deep-RL methods lose to on IM, and they have no vault items.

**Path-finding**
- **ALT and landmark A* heuristics (Goldberg and Harrelson 2005)** would make Ng's potential rule concrete for the route finders. The ALT work is deferred, and no item covers it.

**Learning from outcomes**
- **Online or bandit allocation from observed lobbying outcomes.** This is what learning would actually need. The theme has only tabular RL theory and offline learned heuristics.
- **Small-sample guarantees.** No work gives finite-sample or regret bounds that would apply at one graph per Congress.

**Items to fix**
- [[anon-nd-untitled-document-single]] needs the Optipolitics chapter, which requires an IOS Press login. The identification also needs confirming: does PDF page 583 fall at printed page 563-575?
- [[ng-1999-policy-invariance-reward-transformations]]'s UNVERIFIED lead on the "provably optimal" claim for the recruitment path needs a reproduction.
- Most bearings are VERIFIED-SOFTENED. Only the budget-frontier bearing is fully VERIFIED.

## All works

- [[anon-nd-untitled-document-single]] (n.d.) — Untitled document (single link)
- [[dai-2017-learning-combinatorial-optimization-graphs]] (2017) — Learning Combinatorial Optimization Algorithms over Graphs
- [[manchanda-2020-gcomb-learning-budget-constrained]] (2020) — GCOMB: Learning Budget-constrained Combinatorial Algorithms over Billion-sized Graphs
- [[ng-1999-policy-invariance-reward-transformations]] (1999) — Policy Invariance Under Reward Transformations: Theory and Application to Reward Shaping
- [[watkins-1992-q-learning]] (1992) — Q-Learning
