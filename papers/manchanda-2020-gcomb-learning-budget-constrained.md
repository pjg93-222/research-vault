---
type: paper
title: "GCOMB: Learning Budget-constrained Combinatorial Algorithms over Billion-sized Graphs"
authors: ["Sahil Manchanda", "Akash Mittal", "Anuj Dhawan", "Sourav Medya", "Sayan Ranu", "Ambuj Singh"]
year: "2020"
venue: "NeurIPS 2020 (34th Conference on Neural Information Processing Systems, Vancouver); arXiv 1903.03332v4, 2 Dec 2020"
doi: ""
category: "Optimization"
systems: ["melange-allocation", "melange-pathfinding"]
components: ["melange-allocation.budget-frontier", "melange-allocation.joint-optimizer", "melange-pathfinding.multi-target-and-budget-modes"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/manchanda-2020-gcomb-learning-budget-constrained.pdf]]"
original_paths: []
sha12: ["92b0b21bca2f"]
tags: [paper, optimization]
---

# GCOMB: Learning Budget-constrained Combinatorial Algorithms over Billion-sized Graphs

**Sahil Manchanda, Akash Mittal, Anuj Dhawan, Sourav Medya, Sayan Ranu, Ambuj Singh (2020).** *NeurIPS 2020 (34th Conference on Neural Information Processing Systems, Vancouver); arXiv 1903.03332v4, 2 Dec 2020.* Theme: [[themes/optimization|Optimization]].

> [!abstract] Summary
> GCOMB (Manchanda et al., NeurIPS 2020) learns a heuristic for budget-constrained set problems on graphs: max coverage, max vertex cover and influence maximization. A GCN regresses probabilistic-greedy marginal-gain scores; a noise predictor prunes low-value nodes; then n-step Q-learning picks seeds using score plus locality, the count of neighbours not already covered. The payoff is speed, not quality. On small graphs greedy already reaches at least 99 percent of the CPLEX optimum (BP-500 at b=20: GCOMB 0.998, greedy 0.99, optimal 1). On real graphs GCOMB matches greedy and runs about 10 times faster, 2 to 4 times faster than CELF on YouTube, and 30 to 160 times faster than IMM. It still finishes on billion-edge graphs where IMM runs out of 256 GB RAM. The paper explicitly excludes sequential and capacity-constrained problems (Sec 5). For Paul's systems, the dashboard's budget max-coverage recruiter is the direct descendant. It keeps GCOMB's locality idea (count only newly covered members) and a degree prune, but it uses exact greedy, which this paper's own evidence supports on a 533-member graph. The paper does not support replacing the CELF joint optimizer or the lazy-greedy frontier with a learned policy. Its gains appear only at million-edge scale. The optimizer's per-member PMAX cap is the capacity constraint GCOMB excludes. The IM objective is single-colour IC, evaluated with 10,000 Monte Carlo runs, not the system's two-colour competitive cascade at MC 20. An online scan found a 2024 VLDB benchmark (Liang et al., arXiv 2406.14697) reporting that lazy greedy consistently beats five deep-RL methods, GCOMB included, on max coverage. IMM and OPIM beat them on IM in most settings. That benchmark is not in the vault.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Can a learned heuristic for budget-constrained set combinatorial problems on graphs (max coverage, max vertex cover, influence maximization) scale to billion-edge graphs while matching greedy and prior learned solvers in solution quality? (Sec 1, p.2-3)

**Theory:** No formal economic or causal theory. Framework mixes supervised learning (GCN regresses probabilistic-greedy marginal-gain scores) with n-step fitted Q-learning over a state built from node score and locality (Eq 3: neighbors not already covered by the partial solution). Greedy 1-1/e approximation is cited from prior work (Sec 2, App B), not proved here.

**Data:** SNAP graphs (Table 1a, p.6): Brightkite 58.2K nodes/214K edges; Gowalla 196.5K/950.3K; YouTube 1.13M/2.99M; StackOverflow 2.69M/5.9M; Orkut 3.07M/117.1M; Twitter 41.6M/1.5B; Friendster 65.6M/1.8B. Synthetic bipartite BP graphs (edge probability 0.1) and Barabasi-Albert graphs (density 4). Unit is the graph at budget b. IM edge weights: constant 0.1, tri-valency, or learned from Stack logs 8/2008-3/2016 (Sec 4.3, p.8).

**Method:** Stage 1 (Sec 3.1-3.2, p.4-5): probabilistic greedy run m=30 times per graph, stopping at marginal gain 0.01, gives node scores (Eq 1). A noise predictor on the first GCN layer prunes low-rank nodes; a GCN (K=2, embedding 60, 1000 epochs) regresses scores by MSE on good nodes (Eq 2). Stage 2 (Sec 3.3, p.5): n-step fitted Q-learning (n=2, gamma 0.8) with max-pool state over good nodes; locality from importance-sampled neighbors (Theorem 1, App F). Inference: one GCN forward pass plus budget-many Q-steps. Baselines: S2V-DQN, GCN-TreeSearch, Greedy, CELF, CPLEX, IMM, OPIM.

**Identification:** Not applicable: algorithmic benchmark, no causal identification. Theorem 1 gives a Hoeffding-type concentration bound for the importance-sampled locality estimate, under the paper's i.i.d. assumption (App F).

**Scope:** Set-valued, budget-constrained combinatorial problems on graphs only (MCP, MVC, IM); sequential and capacity-constrained problems explicitly excluded (Sec 5, p.9). Hardware: 64-core Xeon E5-2698v4, one GTX 1080 Ti (12 GB), 256 GB RAM, Ubuntu 16.04 (Sec 4.1, p.6). Era: 2020 NeurIPS benchmark on SNAP-scale graphs; no populations, institutions or time periods beyond the graph datasets.

## Key findings

- MCP at b=15 on synthetic bipartite graphs: BP-2k GCOMB 0.89, Greedy 0.89, S2V-DQN 0.87, GCN-TreeSearch 0.86; BP-20k GCOMB 0.83, GCN-TreeSearch 0.82, S2V-DQN not run (out of memory above 20,000 nodes). (Table 2a, Sec 4.2, p.7)
- Table 1b on small graphs: Greedy reaches at least 99 percent of the CPLEX optimum; on BP-500 at b=20 GCOMB 0.998, Greedy 0.99, Optimal 1; GCOMB sometimes beats Greedy. (Table 1b and Sec 4.2, p.6-7)
- Real-graph MCP quality: GCOMB similar to Greedy across budgets on Gowalla and YouTube; GCN-TreeSearch marginally inferior. (Figs 2b-2c, Sec 4.2, p.7; Fig 6a App K, p.18)
- Speed: GCOMB up to 2 orders of magnitude faster than GCN-TreeSearch and 10 times faster than Greedy on MCP (Twitter-ego, YouTube); speed-up over CELF on YouTube falls from 4.0 at b=20 to 2.01 at b=200. (Figs 2d-2e and Table 2b, Sec 4.2, p.7)
- Training time: GCOMB performance saturates within 10 minutes; S2V-DQN and GCN-TreeSearch need about 9 and 5 hours to reach best performance. (Fig 2a, Sec 4.2, p.7)
- IM on YouTube subgraphs at b=20: GCN-TreeSearch takes about 3 hours on the 70 percent subgraph; GCOMB finishes in 5 seconds. (Fig 3a, Sec 4.3, p.8)
- IM vs IMM on YouTube and Stack: GCOMB 30 to 160 times faster; spread difference entries in Table 3a are small (largest magnitude about 3e-3 at YT-TV, b=50). Units of the table entries are unclear given the x100 definition. (Table 3a and Fig 3b-3c, Sec 4.3, p.8)
- IMM crashes on Orkut, Twitter and Friendster (sample exceeds 256 GB RAM); GCOMB finishes within minutes for b<30 and within 100 minutes at b=100 and 200 on the billion-edge graphs. (Sec 4.3, p.8; Figs 3g-3h)
- Ablation: Q-learning adds up to 10 percent coverage over choosing top-b GCN scores on MCP; the noise predictor cuts running time by close to two orders of magnitude with no quality loss and slightly better quality. (Sec 4.4, Figs 3i-3l, p.9)
- Versus Stochastic Greedy: GCOMB up to 20 percent better quality at eps=0.2 and 2 times faster; Stochastic Greedy does not match GCOMB quality even at eps=0.05. (App L, Table 4, p.18)
- MVC (Table 5): GCOMB equals Greedy at BA-10k (0.11) and BA-100k (0.0346); GCN-TreeSearch is 0.0328 at BA-100k; S2V-DQN ran out of memory above BA-20k. (Table 5, App M, p.18-19)
- Theorem 1: importance-sampled locality estimate is unbiased (Lemma 1) and needs sample size O(log|N(Vg)|/eps^2) for error eps with probability above 1 - 1/|N|^2 under a Hoeffding argument (Lemma 2). (Sec 3.3.1 p.5; App F p.15-16)

## Limitations

- Scope is limited to set combinatorial problems on graphs; sequential and capacity-constrained problems are named as future work (Sec 5, p.9).
- Abstract claims GCOMB is 150 times faster than IMM on IM, while Sec 4.3 reports 30 to 160 times on YouTube and Stack; no single measured 150x figure appears in the body.
- Intro cites GCN-TreeSearch at about one hour on a million-edge graph (Sec 1.1, Fig 3a) while Sec 4.3 reports about 3 hours on a 70 percent YouTube subgraph; different workloads, not reconciled.
- Greedy is near-optimal only shown on small graphs (Table 1b); for large graphs the paper assumes the same ratio holds (Sec 4.2, p.7).
- Main results average 5 runs but report no variance or confidence intervals in tables.
- Noise-predictor interpolators are fitted on separate 10 percent subgraphs of Gowalla, Twitter-ew and YouTube; test on the edge-disjoint 90 percent (App H, p.17).
- Fixed 12-hour training budget for all learned models, chosen for fairness rather than tuned per method (Sec 4.1).
- IMM and OPIM crash on the largest graphs, so there is no direct quality comparison there; GCOMB quality on billion-edge graphs is not reported against Greedy.
- Theorem 1 proof asserts the importance samples are i.i.d. to apply Hoeffding (App F, p.16); the text does not justify this for sampling from a fixed neighbor set.
- Table 3a unit is unclear given the spread difference is defined as a percentage (x100), but entries are of order 1e-3 or smaller.

## Short quotes

- "GCOMB is 100 times faster and marginally better in quality than state-of-the-art algorithms"
- "it runs out of memory on graphs containing more than 20, 000 nodes"
- "Thus, pruning the search space is as important as prediction of the solution set."

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-pathfinding#Best-sponsor, multi-target tree and budget max-coverage modes\|Best-sponsor, multi-target tree and budget max-coverage modes]] | method-source | Locality is neighbours not yet covered by the partial solution (Eq 3). Greedy reaches at least 99 percent of optimal on small graphs (Table 1b: BP-500 b=20 GCOMB 0.998, greedy 0.99, optimal 1). GCOMB matches greedy on real graphs and wins mainly on time at scale. (Sec 3.3 Eq 3 p.5; Sec 4.2 Table 1b and Figs 2b-2e p.6-7) | budgetRecruit is greedy max coverage. Gain = (1 - cov[c]) + sum over v of PE[c][v] times (1 - cov[v]), with cov = 1 - prod(1-p) over selected recruiters. Candidates below max(1, 0.1 x mean degree) are skipped. The budget in $k becomes a seed count via K.CEDGE. The live caller passes target -1, so the target multiplier is off. `fec/surfaces/templates/make_dashboard_html_00.tpl:773-793; fec/surfaces/templates/make_dashboard_html_00.tpl:1126` | The 1-1/e bound holds for cardinality-constrained monotone submodular coverage, which this objective is. Paper evidence for greedy near-optimality is only on 500 to 900 node graphs (Table 1b); applying it to a 533-member graph is an extrapolation. The GCOMB Eq 3 locality is a Q-learning state feature. The code uses the plain coverage marginal, which greedy already contains, so the GCOMB label overstates the mapping. The degree prune is a fixed rule, not the learned noise predictor. | strong | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Joint CU budget optimizer\|Joint CU budget optimizer]] | qualifies | GCOMB's advantage is wall-clock speed on large graphs: about 10 times faster than greedy on MCP; speed-up over CELF on YT falls from 4.0 at b=20 to 2.01 at b=200 (Table 2b). Training saturates within 10 minutes (Fig 2a, synthetic BP 5k). Sequential and capacity-constrained problems are named as future work (Conclusion p.9). (Sec 4.2 p.7 (Table 2b, Figs 2d-2e, Fig 2a); Conclusion p.9) | Joint CU budget optimizer: heap-based lazy greedy over equal $500 chunks (INC) of a $2.5M pot (BUDGET), with a per-member cap PMAX of $10k. The queue is rebuilt after each conversion. Members behind the stance gate receive nothing. This is capacity-constrained allocation over about 533 members, not set selection on a large graph. `fec/engine/optimize.py:27 (BUDGET, INC, PMAX definitions); fec/engine/optimize.py:126-151 (heap loop, queue rebuild, re-push)` | The 10x and 2.01 to 4.0 speed-ups are for MCP on real graphs of 1M+ nodes, not for a small capacity-constrained allocation. A GCOMB-style learned policy would fit only pure set selection without per-member caps at much larger scale. The paper reports no quality gain over greedy. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Budget frontier with knee and electoral modes\|Budget frontier with knee and electoral modes]] | qualifies | IM is evaluated with single-colour Independent Cascade, with expected spread averaged over 10,000 Monte Carlo simulations. GCOMB spread is within small differences of IMM (Table 3a, largest magnitude about 3e-3 at YT-TV b=50) and 30 to 160 times faster. (App A Def 4 (IC); Sec 4.3 p.8 (text and Table 3a)) | budget_frontier.py runs a heap-based lazy greedy up to MAXSEEDS 60. Spread is a two-colour competitive cascade (pro versus anti seeds) at MC 20 with common random numbers. Knee = last seed whose marginal is at least a quarter of the peak marginal. `fec/engine/budget_frontier.py:7; fec/engine/budget_frontier.py:49-58; fec/engine/budget_frontier.py:83-94; fec/engine/budget_frontier.py:118-135; fec/engine/budget_frontier.py:159-172` | The paper's 1-1/e and IC comparisons assume submodular single-cascade spread. The two-colour race with bystander mobilization need not be submodular, so those guarantees do not transfer. MC 20 has no convergence check by the paper's standard of 10,000 runs. The 30 to 160 times speed figure is versus IMM, not versus this frontier. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: summary_md says greedy reaches at least 99 percent of optimal on small graphs. The table minimum is about 98.8 percent (Gowalla-900 b=6: greedy 0.93 vs optimal 0.941). Reword as about 99 percent.
> - Summary issue: summary_md says the paper explicitly excludes sequential and capacity-constrained problems (Sec 5). The Conclusion only names them as future work and says the method is limited to set problems. Reword.
> - Summary issue: summary_md and the joint-optimizer bearing attach training saturation in 10 minutes to large-graph speed. Fig 2a is a synthetic BP 5k graph.
> - Summary issue: summary_md says the budget recruiter keeps GCOMB's locality idea. The template gain is the plain coverage marginal. Eq 3 locality is a Q-learning state feature the code does not implement. Reword as an analogy.
> - Summary issue: summary_md places the recruiter on a 533-member graph. That size was not verified in this pass.
> - Summary issue: The Liang et al. 2024 VLDB benchmark (arXiv 2406.14697) was checked online. Title and VLDB 2024 venue confirmed. Abstract: lazy greedy consistently beats all deep-RL methods for MCP, and IMM and OPIM beat them on IM in most scenarios. GCOMB is one of five methods, but the abstract gives no GCOMB-specific number. This is external to the paper text.
> - Summary issue: paul_alignment and flags describe Paul's positions taken from component_literature. Those positions are not in this paper's text and were not verified here.
> - Summary issue: Abstract says 150 times faster than IMM. Body (Sec 4.3) reports 30 to 160 times on YT and Stack; the abstract figure is the top of that range, not a single measured result. Body figure confirmed.
> - Summary issue: Coverage: all 3 rel bearings and all 7 rel related links were checked. paul_entries is empty, so no Paul markup was checked.

## Gaps and flags

- No learned or sampled-greedy solver is needed at the 533-member scale, and none exists. If Melange ever runs IM on the staffer network or a larger person graph, the scaling evidence here (IMM out of memory at 256 GB on billion-edge graphs) becomes relevant.
- The budget frontier's MC 20 spread estimate has no convergence check in this paper's terms (10,000 runs). CRN reduces variance in marginals, but the paper offers nothing on whether 20 runs rank seeds stably.
- The template comment calls the budget mode cost-bounded. It is actually cardinality-bounded at uniform cost K.CEDGE. A heterogeneous per-member cost would need the cost-benefit greedy variant and its weaker bound.
- The online scan found Liang et al. 2024 (VLDB, arXiv 2406.14697), A Benchmark Study of Deep-RL Methods for Maximum Coverage Problems over Graphs. Lazy greedy beats GCOMB on max coverage there, and IMM and OPIM beat it on IM in most settings. It is not in the vault and is a candidate to add.
- Paul's position on the joint optimizer expects a learned policy (GCOMB) to replace greedy at scale. The paper's own results show no quality gain over greedy, only speed on million-to-billion-edge graphs. An independent 2024 VLDB benchmark finds lazy greedy beats GCOMB on max coverage. This is not a brain-rule conflict, but the expectation is not supported at Melange's scale.
- Paul's position on the budget frontier maps GCOMB budgets onto a political-capital orienteering formulation. GCOMB explicitly excludes sequential problems (Sec 5), so the paper does not ground that mapping.
- No rejected SNA proposal is argued here: no cosponsorship-edge dropping and no per-edge volume normalization. No closeness or access-probability language transfers.

There are no annotations on this file; Paul's positions come from component_literature. The pathfinding position, GCOMB pruning plus locality as grounding, agrees with the evidence and matches the code (make_dashboard_html_00.tpl:773-793). The joint-optimizer expectation that GCOMB should replace greedy at scale is stale for the system as built. At 533 members, CELF is already the right tool, the paper's own gains are speed-only and appear at 1M+ nodes, and the optimizer's PMAX cap is a capacity constraint GCOMB excludes. The budget-frontier orienteering reading goes beyond what the paper claims.

## Related papers

- [[dai-2017-learning-combinatorial-optimization-graphs]] — builds-on — S2V-DQN is GCOMB's main learned baseline. Table 2a BP-2k: GCOMB 0.89, S2V-DQN 0.87. S2V-DQN runs out of memory above 20,000 nodes (Sec 4.2). Checked against paper text; item file exists.
- [[kempe-2015-maximizing-spread-influence]] — cites — The IC model in GCOMB Def 4 is cited to Kempe et al. (ref 14); the item file exists. Checked against paper text, not against the Kempe item itself.
- [[almazaydeh-2026-heuristic-centrality-methods]] — complements — Title argues heuristic centrality challenges greedy IM. Checked by title and note only; item file exists.
- [[arya-2025-relink-edge-activation]] — extends — Title confirms deep-RL edge activation for closed-network IM. Checked by title and note only; item file exists.
- [[kamarthi-2020-influence-maximization-unknown]] — same-method — Title confirms RL policy for IM under unknown structure. Checked by title and note only; item file exists.
- [[chen-2021-contingency-aware-influence]] — same-method — Title confirms RL for IM with contingent seed acceptance. Checked by title and note only; item file exists.
- [[bharathi-nd-competitive-influence-maximization]] — complements — Title confirms competitive IM, the basis for the system's two-colour cascade. Checked by title and note only; item file exists.

### Linked from
- [[watkins-1992-q-learning]] — built-on-by — Verified in the item: GCOMB trains seed selection with n-step fitted Q-learning over GCN-derived node scores. That is function-approximation Q-learning, outside this paper's theorem.