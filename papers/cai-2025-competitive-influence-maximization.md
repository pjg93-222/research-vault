---
type: paper
title: "Competitive Influence Maximization in Voter Dynamics"
authors: ["Zhongqi Cai", "Enrico Gerding", "Markus Brede"]
year: "Not printed in the text (metadata hint says 2025; latest cited reference is 2024, Palermo et al.)"
venue: "Preprint submitted to Elsevier (29 pages). No journal named in the text. Affiliations: Xiamen University of Technology (Cai); University of Southampton (Gerding, Brede)."
doi: "\"\""
category: "Adaptive Influence Maximization"
systems: ["melange-allocation", "melange-influence-engine"]
components: ["melange-influence-engine.flip-model", "melange-allocation.joint-optimizer", "melange-allocation.break-even-and-roi", "melange-influence-engine.competitive-cascade-crn", "melange-influence-engine.persuadability-table", "melange-allocation.per-topic-allocators"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/cai-2025-competitive-influence-maximization.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\Inter_temporal_Influence_Maximization_ZC_2025.pdf"]
sha12: ["5630d0d41fa7"]
tags: [paper, adaptive-influence-maximization]
---

# Competitive Influence Maximization in Voter Dynamics

**Zhongqi Cai, Enrico Gerding, Markus Brede (Not printed in the text (metadata hint says 2025; latest cited reference is 2024, Palermo et al.)).** *Preprint submitted to Elsevier (29 pages). No journal named in the text. Affiliations: Xiamen University of Technology (Cai); University of Southampton (Gerding, Brede)..* [DOI ""](https://doi.org/""). Theme: [[themes/adaptive-influence-maximization|Adaptive Influence Maximization]].

> [!abstract] Summary
> Cai, Gerding and Brede study a time-limited, budget-constrained influence campaign in a non-progressive voter model, where opinions can flip back. They use heterogeneous mean-field theory, relaxation-time analysis, hill climbing and an iterative minimax search for Nash start times. Every result is a simulation on configuration-model scale-free graphs or on five public graphs. The paper uses no observed votes, lobbying or money data.
> 
> Findings. Against a fixed opponent (Table 2, constant-opponent setting), a later start raises final share over starting at t=0; the spend-near-deadline pattern is a reading of Eq 3-5, not a paper statement. Across all datasets, horizons and budget ratios in Table 2, an optimized start beat starting at t=0. One example is the 'Congress votes' mentions graph at T=16 with equal budgets, constant opponent, in model time units: vote share 0.705 against 0.500. A richer controller starts earlier: on that graph (T=16, constant opponent) the optimal start falls from 14.73 at budget ratio 0.1 to 10.48 at ratio 10. High-degree nodes take longer to settle, so on a short horizon the outcome stays far from equilibrium (degree of equilibrium never above 0.82 at T=16). In the two-controller game the side with more budget moves first, and the second mover can only offset its lead, not overtake it. Small budgets go to low-degree nodes. With 20 percent partial zealots (N=100 setting), zealots get fewer resources than normal agents.
> 
> Why it matters here. Melange's money engines have no time dimension at all. The flip model's pflip = a/(a+b) is a stationary share ratio with no time term, the optimizer allocates dollars with no timing, and the competitive cascades let a member who has been reached stay that colour. The paper is a formal case that timing and reversibility change who wins under a fixed budget. Its evidence is abstract: model time units, unit edge weights, full knowledge of the network, and no calibration to any real campaign. Read it as a design critique and a source of hypotheses. It does not validate or refute any Melange constant.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** How do the timing and distribution of a time-limited, budget-constrained influence campaign shape the final vote share in a voter model, and how does network degree heterogeneity change the optimal campaign timing? Two settings: (1) one controller (A) against a constant, fixed opponent (B); (2) two controllers choosing start times simultaneously in a zero-sum game (paper Sections 1, 2, abstract p.1).

**Theory.** Non-progressive voter model (reversible opinion changes; Masuda 2015) with heterogeneous mean-field theory and relaxation-time analysis (Son et al. 2008). Degree heterogeneity drives timescales. Competitive timing and budget allocation are analysed with interior-point optimization, stochastic hill climbing, and minimax-based iterative search for Nash equilibria in a zero-sum game. Partial zealotry (Masuda et al. 2010; Moreno et al. 2020) adds agent heterogeneity. Authors contrast their smooth zealotry with latency models (Palermo et al. 2024).

**Data.** Simulated and real networks, no empirical opinion or vote-outcome data.
(a) Synthetic configuration-model scale-free networks. Main experiments: N = 10^4 nodes, target mean degree 10 (realized 10.5), degree exponent lambda in [1.6, 5], k_max = sqrt(N) (p.11, p.25). Individual-optimization experiments: N = 10^2, mean degree 6, k_max = 30, 10 realizations (p.17, p.25). Complete-network zealotry experiment: N = 10^2 (p.26). Each figure reports 100 or 10 realizations (see captions).
(b) Real networks, Table 1 (p.11): Congress votes N=219, 764 edges, mean degree 7.0, lambda about 1.9 (text: mentions between US politicians with supportive or opposing edges). Facebook N=3862, 87324 edges, mean degree 45.2, lambda about 1.6 (users affiliated with one corporation). Advogato N=6541, 51127 edges, mean degree 15.6, lambda about 1.7 (trust network). Wikipedia Elections N=7118, 103689 edges, mean degree 29.1, lambda about 1.5 (positive/negative votes between users; these are Wikipedia admin-style votes, not political elections). Twitter N=22322, 31823 edges, mean degree 2.9, lambda about 4.3 (following network).
Unit of analysis: individual node (agent). Time: simulated horizons T=16 (short) and T=256 (long), dimensionless model time, not calendar periods. Initial opinion share 0.5 for all nodes (or 0 in some experiments, p.18, p.26); one illustrative example uses S_A(0)=0.55 with N=10^4, T=30 (p.5).

**Method.** Analytical plus numerical simulation.
- Model (Eq. 1, p.4): each node's opinion-A probability x_i(t) evolves under neighbour influence and two controllers with time-varying gains a_i(t), b_i(t). Budget constraints sum of integrals <= b_A, b_B (p.3-4). Vote share S_A(T) = mean of x_i(T) (Eq. 2).
- Constant-opponent, uniform start (Sec 2.1, Eq. 3-5): A's gain is 0 until t_a, then b_A/((T-t_a)N); B's gain is constant b_B/(TN). Optimal t_a solved by interior-point method (p.5).
- Individual optimization (Sec 2.2, Eq. 6-7): each node has its own start time and budget; stochastic hill climbing (Algorithm 1, p.7), with L_max = N^2 iterations (p.26). Budget moves between nodes keep the total fixed.
- Heterogeneity extension (Sec 2.2.1, Eq. 8): partial zealots with probability q_i of resisting adoption of A when holding B.
- Game (Sec 2.3, Eq. 9; Algorithm 2, p.8): simultaneous choice of start times t_a, t_b; iterative alternating best responses (A maximizes, B minimizes S_A(T)) with step size mu = 1/i, stopping threshold 1e-9 (p.8).
- Mean-field (Sec 3, Eq. 10-11, derived in App. B): nodes grouped by degree k; closed-form x_k(t) and S_A(t).
- Relaxation times (Eq. 12-14, 19): normalized order parameter r_k(t); tau_relax,k and tau_relax as degree-weighted averages; l-percentage relaxation time tau^l.
- Taylor expansion (Eq. 15-18) to second order in 1/k, assuming (a_k+b_k)/k << 1.
- Numerical integration: 4th-order Runge-Kutta, time step 0.01 (p.11).
- Real-network experiments: Table 2 compares optimized t_a with t_a=0 at T in {16, 256} and b_A/b_B in {0.1, 1, 10}.
- Appendix A: single-node case with fully flexible allocation via calculus of variations (Eq. 20-30), yielding a transcendental equation for start time.

**Identification.** None in a causal sense; model-based simulation. Mean-field and Taylor predictions are validated against Runge-Kutta integration (Fig. 2, 3, 4c-f; p.11-16). Optimality of the start time is checked by a convex vote-share peak (Fig. 4c-d, p.15-16). Within-model comparison is optimized start versus t_a=0 (Table 2, p.18).

**Scope.** Population: abstract simulated networks (configuration model) and five public network datasets; no sampled human population. Era: none stated. Opinions: binary (A or B), simple contagion, unit link weights, complete knowledge of network topology assumed (p.22-23). Applications named as motivation only (marketing, politics, public health; p.1-2, p.22); no application-specific calibration or validation is reported.

## Key findings

- Constant-opponent, uniform start: optimal strategy is to delay. The controller lets the opponent dominate early and reserves budget; end-loaded spending is a reading of Eq 3-5, not a paper statement. (Sec 4.3.1, p.16-17; Fig. 5a; Sec 1 p.3 (main finding i))
- Network heterogeneity shifts optimal timing with horizon. For short horizons, highly heterogeneous networks (low lambda) should start later; for long horizons they should start earlier. (Sec 4.3.1, p.16-17; Sec 1 p.3 (main finding i))
- Relaxation time increases with node degree; higher-degree nodes equilibrate more slowly. Mean-field and Taylor estimates track Runge-Kutta. (Sec 4.2, Fig. 2(a), p.11-12; Eq. 14, 17 p.10)
- More heterogeneous networks (lower lambda) have longer overall equilibration times, driven by high-degree nodes. (Sec 4.2, Fig. 2(b), p.12; Conclusion p.22)
- Degree of equilibrium l reverses the ranking of relaxation times. At low l, less heterogeneous networks (high lambda) take longer; near equilibrium, more heterogeneous networks take longer. Early changes are driven by low-degree nodes; late changes by high-degree nodes. (Sec 4.2, Fig. 3(a)-(b), p.13-14)
- Real networks: optimized start time beats t_a=0 in every dataset, both horizons, and all budget ratios tested. (Sec 4.3.1, Table 2, p.18)
- Budget advantage means earlier start. A controller with more budget relative to its rival starts influence earlier; a disadvantaged controller starts later. (Sec 4.3.1, Fig. 5b, p.17-18; Table 2; Appendix A Eq. 30, p.23-24)
- Individual (node-specific) optimization improves vote share over a single uniform start, but absolute gains are small. Relative gains are largest for small budget ratios. (Sec 4.3.2, Fig. 6(a)-(c), p.17-18; Appendix D p.26; Fig. 11(c) p.27)
- Under node-specific optimization, small budgets target only low-degree nodes. Control gains fall with degree and start times rise with degree when the budget ratio is small; the pattern reverses for large ratios. (Sec 4.3.2, Fig. 6(b)-(c), p.18; Appendix D Fig. 10(b)-(c), p.26)
- Zealotry: 20% of nodes are partial zealots (N=100 per Sec 4.3.2). Normal agents receive more resources than zealots at every q tested, and zealot allocations depend on q. (Sec 4.3.3, Fig. 7(a)-(d), p.19-20)
- Complete-network zealotry (no degree structure, zealotry uniform on [0,1]): at large budget ratio, controller A starts earlier as zealotry rises up to a threshold near q=0.7, then reverses. (Appendix E, Fig. 11(a)-(c), p.26-27)
- Game with two controllers: the controller with the larger budget starts first, and the second mover starts later and can only buffer the first mover's gain. (Sec 4.3.4, Fig. 8(a)-(b), p.19-20)
- Nash starting times depend on degree exponent. For budget advantage, the advantaged controller starts at time 0 when lambda > 2.2; below 2.2 it starts later. Advantaged controller's per-node allocation is also larger, so the second mover can only offset, not overtake. (Sec 4.3.4, Fig. 9(a)-(b), p.21-22)
- Analytical single-node result: with fully flexible control against a constant opponent, the optimal start time satisfies a transcendental equation; a larger budget ratio pushes the start earlier. (Appendix A, Eq. 29-30, p.23-24)

## Limitations

- Binary opinions and simple contagion only; complex contagion and multi-state opinion models are not studied (p.22).
- Assumes complete knowledge of network topology and of the opponent's strategy; partial visibility and uncertain opponent behaviour are flagged as future work (p.22-23).
- Mean-field analysis assumes degree-uncorrelated networks (p.9); its accuracy is validated only on configuration-model networks and not on the five real datasets (no mean-field check reported for Table 2).
- Taylor expansion valid only when (a_k+b_k)/k is small (p.10).
- Individual optimization restricted to N=10^2 for computational reasons; absolute improvements described as very small (p.17-18).
- Stochastic hill climbing and the iterative Nash algorithm are numerical heuristics; the text reports no optimality guarantee (p.7-8). Nash equilibria are approximated, not solved in closed form (p.8).
- Individual optimization is not analysed in the game-theoretic setting because gains are small (p.22).
- Integer k_min forces realized mean degree to deviate from target (target 10 gives 10.5) (p.25).
- Degree exponent lower bound 1.6 chosen because lower values fail the k_min constraint (p.25).
- Real datasets are not election outcome or lobbying data: Congress votes are mentions/support-oppose edges, Wikipedia Elections are user-to-user votes, Facebook and Twitter are social graphs. No validation against observed vote shares is reported.
- Time horizons are abstract model units; no calendar mapping is given beyond the illustrative 30-day example (p.5).

## Short quotes

- "the best strategy for the active controller is to delay its influence" (Sec 1 Introduction, p.3 (main finding i))
- "resource-rich controllers tend to start campaigns early, while resource-limited controllers strategically delay to counter their opponent's advantage" (Abstract, p.1)
- "networks with more heterogeneity have longer timescales to reach equilibrium" (Sec 4.2, p.12)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | qualifies | Vote share at a finite horizon depends on when each controller starts and on degree heterogeneity. At T=16 (dimensionless model time, unit-weight graphs) the degree of equilibrium l never exceeds 0.82 and high-degree nodes relax more slowly. An optimized start beats t_a=0 in all 30 rows of Table 2 (5 real networks, T in {16, 256}, budget ratio in {0.1, 1, 10}). Congress votes network, T=16, budget ratio 1: S_A 0.705 vs 0.500 (model output, not observed votes). (Sec 4.2 p.11-16 (text around Fig 4e); Sec 4.3.1 Table 2 p.18) | pflip = a/(a+b), where a and b are each a saturating money pull plus a party-weighted proximity term. It is a static, time-free share with no horizon, no start time and no relaxation lag. `fec/engine/flip_model.py:3-8,92-97` | Structural parallel only: both are ratios of competing pulls. Magnitudes do not transfer, since paper time is dimensionless, edges have unit weight and opinions are binary simple contagion. The paper shows that an equilibrium-share form omits timing, not that pflip is miscalibrated. | moderate | VERIFIED |
| [[systems/melange-allocation#Joint CU budget optimizer\|Joint CU budget optimizer]] | suggests-extension | With the budget fixed, choosing a start time raises final vote share over spending from t=0 (Table 2, constant-opponent setting). For N=100 networks, per-node start times and budgets raise vote share further: the relative gain is large at small budget ratios but small in absolute terms (Fig 6a relative axis 1.00 to 1.25). A resource-limited controller starts later and a resource-rich one starts earlier (abstract; Sec 4.3.1). Under the uniform-start formula (Eq 3-5) the controller spends its budget over a short late window; the paper does not prescribe this as a rule. (Abstract p.1; Sec 2.1 Eq 3-5 p.4-5; Sec 4.3.1 Table 2 p.18; Sec 4.3.2 Fig 6 p.17-18) | optimize.py docstring describes a lazy greedy allocator with a Golovin-Krause bound kept as a diagnostic. Defaults are a 2.5M pot in 500 increments with a 10k per-candidate cap (PMAX). It allocates amounts per member with no start time or spending schedule; the objective is time-free winnability. `fec/engine/optimize.py:3-12,26-27` | Only as a hypothesis. PAC dollars cannot be withdrawn, FEC per-cycle limits bind, and the paper's continuous gain a_i(t) has no clear counterpart in lump-sum contributions. The paper's large relative gain at small budgets means the per-node result should not be read as uniformly small. Untested on legislators. | weak | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Break-even CU spend and ROI floor\|Break-even CU spend and ROI floor]] | qualifies | Two settings are mixed here. The optimal start for Congress votes at T=16 falls from 14.730 at budget ratio 0.1 to 10.476 at ratio 10 (Table 2, constant-opponent setting, fixed opponent starting at 0). In the two-controller Nash game the controller with the larger budget moves first, and the second mover can only buffer the first mover's gain, never exceed its vote share (Sec 4.3.4, stage 3, p.21-22). Fig 9b shows per-node resource per unit time rising with the budget advantage. (Sec 4.3.1 Table 2 p.18; Sec 4.3.4 Fig 8-9 p.19-22) | The head-to-head panel solves CU share against competitor share in closed form for the baseline odds. It reports that CU money alone cannot fully offset when the required money ratio q is at least 1. The solve uses the CU-to-competitor money ratio at one instant, with no sequencing. `fec/surfaces/templates/make_dashboard_html_00.tpl:1096-1104` | The paper's statement that a disadvantaged controller can offset but not overtake agrees in spirit with the cannot-offset message. The claim that a disadvantaged side does better by delaying has no counterpart in a static break-even, and it rests on the paper's abstract and simulations only. Transfer would need a model of within-cycle spending order, which the system lacks. | weak | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Competitive independent cascade with common random numbers\|Competitive independent cascade with common random numbers]] | qualifies | Competitive influence is analysed in a non-progressive voter model with reversible opinions, where the opponent is a strategic controller choosing its own start time at a Nash equilibrium (Eq 9, Algorithm 2 p.8; Fig 8 p.19-20). The paper does not state that the second mover concentrates its budget in the final stage. Its text says the second mover starts later and can only buffer the first mover's gain (p.21). End-loaded budgets appear for the budget-advantaged controller when the degree exponent is below about 2.2 (Sec 4.3.4, p.21-22). (Sec 2.3 Eq 9 Algorithm 2 p.8; Sec 4.3.4 Fig 8-9 p.19-22) | A two-colour first-to-reach independent cascade: a reached member keeps its colour for the rest of the run. Industry money enters only as non-strategic bystander activation, min(0.9, ind/400000). Nothing represents an opponent that responds in time. `fec/engine/budget_frontier.py:79-94,133; fec/engine/per_topic_coalition.py:40-53,101-108` | This is a difference in model class, not evidence that the independent cascade choice is wrong. The original claims that cosponsorship is close to progressive with rare withdrawals, and that stance and votes are not, have no source in the paper or the code. Those claims are UNVERIFIED here and should not be relied on. | weak | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Persuadability table PERS\|Persuadability table PERS]] | supports | In the individual-optimization runs with 20 percent of agents as partial zealots (Sec 4.3.3, N=100 per the Sec 4.3.2 reduction; N not restated in 4.3.3), normal agents receive more budget than zealots at every zealotry level q tested. At q=0.1 the maximum zealot allocation is under 20 and the normal-agent maximum is near 25 (allocation units as plotted in Fig 7). At medium and large q zealots get fewer resources and start later as degree rises. (Sec 4.3.3 Fig 7 p.19-20; Appendix E Fig 11 p.26-27) | PERS in moneyball_core.py gives Opponent 0.08, Highly Skeptical 0.15 and Champion 0.10, with middle tiers between 0.50 and 0.65. The F1 gate in optimize.py gives zero allocation to members whose overall tier is Skeptic, Highly Skeptical or Opponent. `fec/moneyball_core.py:564-568,717; fec/engine/optimize.py:44-52` | The direction matches for resistant tiers (Opponent and Highly Skeptical). Champion is a committed pro tier, so its low multiplier reflects being already won, not zealot resistance, and it does not support the paper's result. The paper never sets zealot allocation to zero, so it does not support the hard zero gate on its own. Simulated agents only. | weak | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Per-topic flip and coalition allocators\|Per-topic flip and coalition allocators]] | qualifies | Under node-specific optimization on N=100 networks with mean degree 6 (Sec 4.3.2), the budget ratio sets the degree pattern. With small budget ratios, control gains fall and start times rise with degree, so low-degree nodes are targeted. With large ratios the pattern reverses (Fig 6b-c p.18; Appendix D Fig 10 p.26). (Sec 4.3.2 Fig 6(b)-(c) p.18; Appendix D Fig 10 p.26) | The coalition water-fill boosts each candidate's marginal spread by (1 + 0.8 bpf), where bpf is a bipartisan share read from bipartisan.json divided by 100, so it favours bipartisan bridges. The ranking does not depend on budget size, although the CAP-clipped water-fill decides how far down the list money goes. `fec/engine/per_topic_coalition.py:30,68,117; fec/moneyball_core.py:68-72` | bpf is a cross-party share, not degree, so the paper's degree result maps only indirectly. The paper's result rests on N=100 graphs with mean degree 6. Measuring the Melange cosponsor graph's degree exponent and testing whether greedy seeds move toward low-degree members at small budgets would be new analysis and stays UNVERIFIED. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Several related links are not in the paper and were dropped: Borodin (no citation, and the paper has no submodularity or greedy-guarantee discussion, so the claim that Cai's timing game shows competition breaks greedy guarantees is false for this paper), Golovin-Krause, Chen 2021 contingency-aware IM and Noble 2024. These are analyst analogies, not paper claims.
> - Summary issue: Brede, Restocchi and Stein 2019 (ref 10, Effects of time horizons on influence maximization in the voter dynamics) is described by the paper as the most directly relevant prior work, but no item for it exists in the items folder. It should be added as a related link once an item exists.
> - Summary issue: paul_alignment cites a Kim J. 2025 margin note and Paul's positions on joint-optimizer and competitive-cascade-crn, yet the same paragraph says Paul has not annotated the paper, and the item file has paul_entries empty with no markup supplied. Those statements are unsourced and should be removed or sourced.
> - Summary issue: Summary findings give 14.73 to 10.48 and 0.705 vs 0.500 without naming the setting; they are constant-opponent Table 2 values, not Nash-game values. The bearing on break-even had this error.
> - Summary issue: The 0.705 vs 0.500 example and the zealot allocation figures (under 20, near 25) are given without N or time window in the summary, which breaks the rule that every figure names its window and denominator.
> - Summary issue: The summary's claim that this is the clearest formal case in the vault for timing and reversibility is a vault-level comparison that was not verified here.
> - Summary issue: The gap about the optimizer uses the name CELF in the bearing, but optimize.py describes lazy greedy; the name should match the code.
> - Summary issue: Figure-axis values (Fig 6a 1.00 to 1.25; Fig 9b 1 to 3.5) were checked only against text extraction of axis labels, not against the figure images, which cannot be rendered on this machine.

## Gaps and flags

- No time dimension in any money or influence engine. optimize.py, flip_model.py, budget_frontier.py and scenario.py allocate amounts with no start time, horizon or spending schedule, although the paper shows timing alone shifts final share under a fixed budget.
- No model of a strategic opponent. Competitor money is a fixed input or bystander noise (min(0.9, ind/400000)), never a best-responding player, so the system cannot represent a strategic opponent's response.
- No reversibility. Competitive cascades are first-to-reach and absorbing, while stance in the paper (and in reality) can revert. stance_dynamics measures trajectories, but the allocators do not consume them.
- The degree heterogeneity (degree exponent) of the cosponsor graph is unmeasured, though the paper makes optimal timing and degree targeting depend on it, with a regime change near lambda 2.2.
- The contested-bills registry has a schema for a counterparty but no consumer, so the paper's two-controller game framing has nowhere to land (fec/lobbying/contested_bills.py:1-20 says no consumer exists).
- Flag: The paper's 'Congress votes' dataset is a network of mentions between politicians with support/oppose edges. It is not roll calls or cosponsorship, and Table 2 'vote share' is model output, not observed votes. Never quote 0.705 vs 0.500 as an electoral or legislative effect.
- Flag: 'Influence' in the paper is a model-internal controller gain. Under D-025, no bearing here may be read as closeness, access or a probability of moving a member.
- Flag: Every number in the paper is in dimensionless model time on unit-weight graphs. Any figure carried over must name that window and denominator (N, mean degree, lambda, T, realizations).
- Flag: The paper does not argue for dropping regular cosponsorship edges or for per-edge volume normalization; it uses unit weights as a simplification. Neither rejected SNA proposal is implicated.
- Flag: A degree-exponent measurement or timing test on Melange graphs would be new agent analysis.
- Flag: The component list says contested-bills cites Romero Moreno 2021, but fec/lobbying/contested_bills.py contains no such citation. The link lives only in notes.

Paul has not annotated this paper.

## Related papers

- [[moreno-2021-shadowing-shielding-effective]] — complements — Same Brede group and voter-model continuous influence-maximization line. The paper cites a different work for zealotry, Moreno, Manino, Tran-Thanh and Brede 2020 (ref 41, Zealotry and influence maximization in the voter model), not this 2021 item. The original builds-on relation was not supported by the text.
- [[bharathi-nd-competitive-influence-maximization]] — complements — Bharathi, Kempe and Salek 2007 is cited by Cai only for budget constraints (ref 6, with ref 28). The progressive-model claim cites refs 11, 13 and 24, not Bharathi. Bharathi's progressive competitive IC is an alternative formalization, not a contradiction, so contradicts was wrong.

### Linked from
- [[borodin-nd-threshold-models-competitive]] — complements — Competitive influence maximization in voter dynamics with timing and network heterogeneity. Its text does not cite Borodin or discuss submodularity, so it is a neighbouring competitive-IM study on a different diffusion model, not a follow-up to Borodin.
- [[madsen-2017-strategic-advantages-micro]] — extended-by — A two-candidate competitive voter-dynamics model on networks. It adds the voter-to-voter interaction this paper leaves out and says targeting would benefit from.
- [[kempe-2015-maximizing-spread-influence]] — complements — Competitive IM under voter dynamics, where timing matters; Kempe lists competition and timing as open (Conclusions pp.137-138). Item exists; not re-read here.