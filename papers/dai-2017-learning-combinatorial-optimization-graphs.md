---
type: paper
title: "Learning Combinatorial Optimization Algorithms over Graphs"
authors: ["Hanjun Dai", "Elias B. Khalil", "Yuyu Zhang", "Bistra Dilkina", "Le Song"]
year: "2017"
venue: "NeurIPS 2017 (NIPS 30); arXiv:1704.01665 v4, Feb 2018"
doi: ""
category: "Optimization"
systems: ["melange-allocation", "melange-pathfinding"]
components: ["melange-allocation.joint-optimizer", "melange-pathfinding.cosponsor-route-pareto", "melange-pathfinding.multi-target-and-budget-modes"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/dai-2017-learning-combinatorial-optimization-graphs.pdf]]"
original_paths: []
sha12: ["f306b1a973d9"]
tags: [paper, optimization]
---

# Learning Combinatorial Optimization Algorithms over Graphs

**Hanjun Dai, Elias B. Khalil, Yuyu Zhang, Bistra Dilkina, Le Song (2017).** *NeurIPS 2017 (NIPS 30); arXiv:1704.01665 v4, Feb 2018.* Theme: [[themes/optimization|Optimization]].

> [!abstract] Summary
> Dai, Khalil, Zhang, Dilkina and Song (NeurIPS 2017) learn greedy heuristics for NP-hard graph problems instead of hand-writing them. A structure2vec embedding (about 4-5 message-passing rounds, 64 dimensions) scores candidate nodes, and n-step Q-learning with fitted Q-iteration trains that score so a greedy loop adds one node at a time. Tested on minimum vertex cover, MaxCut, TSP and set cover, the learned policy (S2V-DQN) is near-optimal on synthetic graphs, holds up when trained on 50-100 nodes and run on 1000-1200 nodes (MVC ratio 1.0062), and beats hand heuristics on a 960-node MemeTracker diffusion graph (MVC ratio 1.0021 vs 1.2220 for greedy). It also wins on Ising MaxCut (1.0223) and TSPLIB (1.0475). Its edge is biggest where hand heuristics are weak (MVC, MaxCut). On set cover it nearly ties LP (1.001 vs 1.0009), and simple greedy trails by only 3 percent (1.03). It assumes many instances drawn from one distribution, offers no guarantees, and compares GPU runs against CPU baselines. For Melange this is the origin of the learned-greedy line (GCOMB, RL4IM, RELINK) that Paul cites. Every hand-built greedy in the system already fits its meta-algorithm with a hand-written score: the joint budget optimizer, budgetRecruit, the cosponsor route finder and closedCascadeRoute. The paper's own evidence suggests a learned score adds little where greedy is already exact or near-optimal (the fixed-state joint optimizer, coverage recruitment). The only plausible headroom is in the non-submodular route heuristics, and even there the training data does not exist: Melange has one 533-member graph per Congress, not a distribution of instances.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Can heuristics for NP-hard graph combinatorial optimization problems be learned automatically from a distribution of instances that share structure, rather than hand-designed with trial and error? (Abstract; Introduction p.2)

**Theory:** Greedy meta-algorithm: a feasible solution is built by successively adding nodes chosen by an evaluation function Q, with the problem supplied through a helper function, cost function and termination criterion (Section 2). Reinforcement learning view: states are partial solutions, actions add a node, rewards are changes in the cost (Section 4.1, Table 1). No economic or political theory; the framing is algorithm design and ML.

**Data:** Synthetic: Erdos-Renyi (edge probability 0.15) and Barabasi-Albert (average degree 4) graphs for MVC and MaxCut, with node counts sampled from ranges such as 15-20 up to 1000-1200 (Sections 5 and D.1); 2-D Euclidean TSP instances (random and clustered) from the DIMACS generator, unit is one graph or tour instance, training sets of 100 held-out validation graphs and 1000 test graphs (Section 5). Set covering built from the MemeTracker diffusion graph (Appendix B and C.4). Realistic: MemeTracker network of 960 nodes and 5000 edges for MVC (Appendix C.1, diffusion probabilities from average transmission times with alpha 0.1); ten Ising spin-glass MaxCut instances, 125 nodes and 375 edges each, from the optsicom MaxCut library (Appendix C.2); 38 TSPLIB instances from 51 to 318 cities (Appendix C.3). Period: not applicable; no calendar data.

**Method:** Structure2vec graph embedding (T about 4 to 5 message-passing rounds, embedding size p = 64) feeds a Q-function approximation for a greedy policy (Sections 3.1-3.2, Table D.9). Training uses n-step Q-learning combined with fitted Q-iteration over an experience replay buffer, with epsilon annealed from 1.0 to 0.05 (Section 4.2, Algorithm 1, Appendix D.5). Testing uses the deterministic greedy policy, not Active Search, except for TSPLIB where Active Search is applied on each instance (Sections 5 and C.3). Baselines: MVCApprox and MVCApprox-Greedy, MaxcutApprox and SDP, TSP heuristics (Farthest, Cheapest, Closest, Christofides, 2-opt, Nearest, MST), pointer networks with actor-critic (PN-AC, reimplemented), and CPLEX for optimal or best-known solutions with a 1-hour cutoff.

**Identification:** None in the econometric sense. Generalization is tested by training on one size range (50-100 nodes for the headline generalization table) and testing on larger graphs up to 1200 nodes (Section 5.2, Table 2). Performance is judged by approximation ratio against CPLEX or Concorde best-known values, averaged over test instances (Section 5).

**Scope:** Graph combinatorial optimization on synthetic and a few public benchmark graphs. Population: NP-hard graph problems MVC, MaxCut, TSP and set covering. Era: NeurIPS 2017 (arXiv 2017, revised 2018). Not an institutional or political study.

## Key findings

- On 1000 synthetic test graphs the approximation ratio of S2V-DQN is very close to 1 for MVC and beats MVCApprox, MVCApprox-Greedy, PN-AC and all TSP heuristics on most settings (Section 5.1, Figure 2).
- Generalization: trained on 50-100 node graphs, S2V-DQN gives MVC (BA) ratio 1.0062 and MaxCut (BA) ratio 1.0038 on 1000-1200 node graphs (Section 5.2, Table 2); TSP clustered ratio rises to 1.1065 at that size.
- Speed: on 1200-node graphs MVC is solved in about 11 seconds on one GPU at ratio 1.0062 (Section 5.3); MVC is slightly slower than the approximation algorithms but much more accurate.
- MemeTracker MVC (Appendix C.1): S2V-DQN covers 474 nodes against an optimum of 473, ratio 1.0021; MVCApprox-Greedy 1.2220 and MVCApprox 1.4080 (Table 3, Section 5.4).
- Ising MaxCut physics instances (Appendix C.2, Table C.1): ratio 1.0223 for S2V-DQN, optimal on 3 of 10 instances, versus 1.2825 for MaxcutApprox and 1.8996 for SDP (Table 3).
- TSPLIB (Appendix C.3, Table C.2): S2V-DQN ratio 1.0475 across 38 instances with 51-318 cities, best tour on all but 6; next best Farthest 1.0800 and 2-opt 1.0947 (Table 3).
- Set covering on MemeTracker-derived graphs (Appendix C.4): S2V-DQN ratio 1.001 versus LP 1.0009 and Greedy 1.03.
- Discovered heuristics (Section 5.5, Appendix D.9-D.10): MVC picks nodes balancing degree against the connectivity of the remaining graph; MaxCut avoids adding nodes that cancel existing cut edges. Authors offer these as interpretable findings, not proofs.
- PN-AC (pointer network, actor-critic) reproduces the Bello et al. TSP ratios only approximately: 1.03 vs 1.02 at TSP20, 1.07 vs 1.05 at TSP50, 1.09 vs 1.07 at TSP100 (Appendix D.11). Comparison is therefore sensitive to reimplementation.
- Training convergence: MVC, MaxCut and SCP converge smoothly on validation; TSP is harder to learn from graph structure because the graph is fully connected (Appendix D.6, Figure D.2).
- Sign of the TSP reward matters: the negative of the insertion reward worked better, which the authors explain as bias toward recent rewards (Appendix D.5).

## Limitations

- Authors state the CPLEX 'optimal' values are often best-found solutions under a 1-hour cutoff, so ratios can fall below 1 or be inflated on large instances (Section 5.2, Appendix D.1).
- TSP training is capped at 200-300 nodes by single-GPU memory; TSPLIB instances above 318 cities are not tackled (Appendix C.3).
- Hyperparameters tuned only on graphs under 50 nodes and then fixed for larger graphs (Appendix D.4); MVC pre-training on smaller graphs is a heuristic trick (Section 5).
- Reward design is described as still challenging; the sign-flip for TSP is ad hoc (Appendix D.5).
- Speed comparison with GPU-based S2V-DQN against CPU-based baselines is acknowledged as not exactly fair (Section 5.3).
- Dense graphs may need edge sampling for embedding computation; left as future work (Section 5.3).
- No theoretical guarantees; the learned policy is empirical only.

## Short quotes

- "The learned greedy policy behaves like a meta-algorithm that incrementally constructs a solution, and the action is determined by a graph embedding network."
- "instances of the same type of problem are solved again and again on a regular basis, maintaining the same combinatorial structure, but differing mainly in their data."
- "an optimal cover has 473 nodes, whereas S2V-DQN finds a cover with 474 nodes, only one more than the optimum"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-allocation#Joint CU budget optimizer\|Joint CU budget optimizer]] | qualifies | Dai et al. report that S2V-DQN beats hand heuristics by wide margins on MemeTracker MVC (ratio 1.0021 vs 1.2220 for MVCApprox-Greedy and 1.4080 for MVCApprox) and on Ising MaxCut (1.0223 vs 1.2825 for MaxcutApprox), on one MemeTracker graph and ten Ising instances. The paper does not study budget allocation or separable resource objectives. (Section 5.4, Table 3; Appendix C.1-C.2 (text lines 710-722, 904, 912-918)) | fec/engine/optimize.py is a lazy greedy (docstring lines 3-11) over per-member chunks of INC, default 500 dollars (line 26), with per-member cap PMAX, default 10000 (line 27). The docstring says the fixed-state objective is separable with diminishing chunk marginals, so the largest marginal is the exact discrete optimum. concavity_violations at line 73 checks this. Replanning after modeled money thresholds does not carry the certificate. The code says lazy greedy; CELF is not named. `fec/engine/optimize.py:3-11, 26-27, 73-85` | The paper's gains are on synthetic graphs and benchmark graph instances, scored against CPLEX best-found values with a one-hour cutoff. The paper says nothing about separable concave allocation, so it gives no support for the optimizer's exactness; that rests only on the code's own docstring and concavity check. A learned replacement would need many training instances, which the system does not have. | weak | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Best-sponsor, multi-target tree and budget max-coverage modes\|Best-sponsor, multi-target tree and budget max-coverage modes]] | qualifies | On set covering built from the MemeTracker-derived graph, averaged over 1000 test graphs, Dai et al. report ratios of S2V-DQN 1.001, LP 1.0009 and Greedy 1.03. This is minimum set cover, not budgeted maximum coverage, and the paper makes no general claim about coverage objectives. (Appendix C.4 (set covering; text lines 1405-1420), with Appendix B for the SCP setup) | budgetRecruit in fec/surfaces/templates/make_dashboard_html_00.tpl lines 773-793 scores a candidate by its marginal noisy-OR coverage gain, 1 minus cov[c] plus the sum of PE times (1 minus cov[v]), multiplied by (1 plus 2 dT[c]) when a target is set. Candidates below 0.1 times mean degree are pruned. The loop stops at nSeeds, not at a cost bound. The comment at lines 773-776 calls it cost-bounded, claims the (1-1/e) guarantee, and says no ML is needed at this scale. `fec/surfaces/templates/make_dashboard_html_00.tpl:773-793` | Dai's set cover is minimum cover; budgetRecruit is budgeted maximum coverage with a target reweighting and degree pruning, so the 3 percent greedy gap does not transfer directly. 533 nodes is inside Dai's tested range of up to 1200, but there is no training distribution. The guarantee does not hold strictly under the reweighting and pruning. | weak | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Cosponsor recruitment route finder (Pareto front)\|Cosponsor recruitment route finder (Pareto front)]] | suggests-extension | A Q-function over a graph embedding, trained by n-step Q-learning with fitted Q-iteration, can serve as a greedy evaluation function and generalizes from 50-100 node training graphs to 1000-1200 node test graphs, with MaxCut BA ratio 1.0038 and MVC BA ratio 1.0062 (Table 2, 1000 test instances). (Sections 3-4, Algorithm 1; Section 5.2, Table 2 (text lines 556-600)) | build_route in fec/engine/cosponsor_route.py lines 41-56 is a greedy over candidate edges, with a heap keyed by (gain minus lam times (1 minus p)) divided by edge cost CEDGE over max(0.1, p). K defaults to 5 recruits per recruiter (line 9), and the loop stops when the best value is at most zero. The non-submodular sibling closedCascadeRoute in the template at lines 824-835 is explicitly labeled a sequential heuristic with no (1-1/e) claim. `fec/engine/cosponsor_route.py:41-56, 9; fec/surfaces/templates/make_dashboard_html_00.tpl:824-835` | Requires a simulated instance distribution, for example cascades on perturbed or topic-specific kernels, as training data, which the system lacks. A learned Q-value is a heuristic score, never an ask-success or access probability (D-025). Any result stays UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary says the joint optimizer is a CELF lazy greedy. optimize.py says lazy greedy only; CELF does not appear in the file.
> - Summary issue: Summary says the paper's evidence suggests a learned score adds little where greedy is near-optimal. Dai et al. make no such claim; it is analyst inference from a 3 percent set cover gap.
> - Summary issue: Summary says RELINK belongs to the learned-greedy line that Dai starts. RELINK's text does not cite Dai, S2V or GCOMB, so it is a parallel line, not a descendant.
> - Summary issue: Summary gives GCOMB's benchmark as S2V-DQN 0.87 vs greedy 0.89 at b=15. The values appear in GCOMB's BP-2k row; the b=15 setting was not verified.
> - Summary issue: Summary says Melange has one 533-member graph per Congress. Not verified in this pass; the repo and vault were not checked for per-Congress structure.
> - Summary issue: Summary implies the set cover gap generalizes to coverage-type objectives. The paper reports only minimum set cover on one MemeTracker-derived graph.
> - Summary issue: paul_alignment says Paul expects a learned replacement via GCOMB. Supported by Paul's addendum and melange-allocation.md, so kept. This item has no Paul entries, so the Paul link comes from other vault notes.
> - Summary issue: The budgetRecruit comment overclaim in gaps_exposed is verified. The comment cites GCOMB (Manchanda) as grounding, not Dai; the bearing calling it Dai's meta-algorithm is only loosely true.

## Gaps and flags

- The budgetRecruit comment (tpl:773-776) calls the routine cost-bounded and claims the classic (1-1/e) guarantee. The loop is actually bounded by seed count, and when a target is set each candidate's coverage marginal is multiplied by (1+2 dT[c]). That is greedy on a reweighted score, not on the coverage function, and degree pruning also restricts the candidates, so the stated guarantee does not strictly hold. This is a comment overclaim, UNVERIFIED by a second path.
- No component has a distribution of training instances, such as per-topic or per-Congress kernels or perturbed cascades. A learned greedy policy (S2V-DQN, GCOMB, RL4IM) would need one.
- Neither the system nor the paper measures how far the non-submodular route heuristics (cosponsor_route, closedCascadeRoute) are from optimal. Dai uses CPLEX optima for this; the system has no equivalent exact or ILP baseline at 533 nodes.
- No conflict with the brain rules: the paper does not argue for dropping cosponsorship edges or for per-edge volume normalization. If adopted, learned Q-values must be worded as heuristic model scores, not closeness, access or introduction probabilities (D-025).
- The paper's approximation ratios are averaged over its own synthetic test sets (typically 1000 graphs) or small benchmark sets (10 Ising instances, 38 TSPLIB instances), against CPLEX best-found values under a 1-hour cutoff. Quote them with that denominator.
- Actual error, our code: the budgetRecruit comment's (1-1/e) and cost-bounded claims do not match the loop (see gaps).

Paul has no entry for this paper. Through GCOMB, he expects a learned policy to replace greedy plus CELF at scale. The evidence here, with GCOMB's own benchmark (S2V-DQN 0.87 vs greedy 0.89 on maximum coverage), says learning pays off where hand heuristics are weak, not on 533-node coverage problems. The system already agrees: the template says no ML is needed at this scale, and the joint optimizer is exact in the fixed state. On learned replacement his note is stale; his GCOMB grounding for budget recruitment holds, apart from the guarantee overclaim.

## Related papers

- [[manchanda-2020-gcomb-learning-budget-constrained]] — extends — Verified: GCOMB (text lines 510-539) benchmarks a budget-constrained learned policy against S2V-DQN, Greedy and CELF on maximum coverage. The BP-2k row reads S2V-DQN 0.87, GCOMB 0.89, Greedy 0.89. The b=15 setting was not verified.
- [[chen-2021-contingency-aware-influence]] — builds-on — Verified: RL4IM uses S2V-based Q-networks (text line 994) and S2V-DQN-IM is a baseline (line 968).
- [[chen-2023-complex-contagion-influence]] — builds-on — Verified: RL4CCIM says it builds on the Khalil 2017 and RL4IM line (text lines 395-413) and uses a graph attention network encoder (line 484).
- [[kamarthi-2020-influence-maximization-unknown]] — builds-on — Partly verified: Kamarthi says previous graph RL work [13], which is Khalil et al. 2017 in its reference list, did not use a distinct global representation (text around line 215). The closest prior work phrasing is not in the text. Internal item slug is kamarthi-2019 (arXiv 1907.11625) while the filename says 2020.
- [[arya-2025-relink-edge-activation]] — complements — RELINK (CIKM 2025) does not cite Dai 2017, S2V, GCOMB or Khalil 2017; its only Khalil reference is the 2014 KDD paper. It is a parallel closed-network RL line, not a descendant. The RELINK design attribution in the closedCascadeRoute code comment is verified.
- [[watkins-1992-q-learning]] — builds-on — Dai's n-step Q-learning cites Sutton and Barto 1998 [36], not Watkins; fitted Q-iteration cites Riedmiller [33]. The Watkins link is conceptual, not a citation.
- [[golovin-2011-adaptive-submodularity-theory]] — complements — Dai does not cite Golovin. The link runs through Melange: the optimize.py docstring keeps a Golovin-Krause upper bound as a diagnostic (verified). Conceptual, not from the paper.
- [[kempe-2015-maximizing-spread-influence]] — complements — Dai cites the classical Kempe et al. influence maximization paper as reference [20] (text line 796). The item year 2015 may differ from the 2003 KDD original; link kept as a conceptual complement.

### Linked from
_None._