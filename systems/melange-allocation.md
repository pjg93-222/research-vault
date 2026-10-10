---
type: system
name: "Melange budget allocation"
slug: melange-allocation
components:
  - melange-allocation.joint-optimizer
  - melange-allocation.stance-money-gate
  - melange-allocation.per-topic-allocators
  - melange-allocation.budget-frontier
  - melange-allocation.workbook-allocator
  - melange-allocation.break-even-and-roi
brain_notes:
  - C:/Users/pgoff/Documents/brain/actors/quant-model-evaluation-skill.md
  - C:/Users/pgoff/Documents/brain/actors/melange-dashboard-june-capstone.md
  - C:/Users/pgoff/Documents/brain/concepts/influence-vs-alignment-axes.md
paper_count: 30
tags: [system]
---

# Melange budget allocation

Spending decisions made on top of the influence engine: the joint CU pot optimizer, per-topic flip and coalition allocators, the budget frontier (including electoral contest and defend modes), the workbook top-N allocator, the stance money gate, break-even CU spend and the ROI floor.

**Brain notes:** [actors/quant-model-evaluation-skill](obsidian://open?vault=brain&file=actors%2Fquant-model-evaluation-skill); [actors/melange-dashboard-june-capstone](obsidian://open?vault=brain&file=actors%2Fmelange-dashboard-june-capstone); [concepts/influence-vs-alignment-axes](obsidian://open?vault=brain&file=concepts%2Finfluence-vs-alignment-axes)

## Melange budget allocation: evidence assessment

**What the corpus covers.** There are 49 evidence rows across 6 components: 12 VERIFIED, 35 VERIFIED-SOFTENED, 2 UNVERIFIED. Most of them are algorithm theory: when greedy, lazy greedy and influence-maximization guarantees hold, and when they do not. Very few measure what allocated money actually does to legislators. Only one row speaks directly to legislator response to PAC money: Ansolabehere et al. 2002 (member fixed effects, business and labor PACs, CCUS scores, 1978-1994). The other money-outcome rows are firm-level returns (Alexander 2009, Bickmore 2025, Fremeth 2018) or a literature review (Bombardini 2020). None of them can calibrate WM, HALFF, PMAX or the saturating money term.

**Headline judgements.**
1. **The joint optimizer's fixed-state exactness is well supported, and its scope is stated correctly.** Golovin-Krause lazy greedy (VERIFIED) and Kempe 2015 Thm 7.1 (VERIFIED) cover a separable, concave, deterministic objective in a fixed stance state. The code withholds the certificate once a modeled conversion fires, and the evidence agrees with that choice. The weak point is the claim's reach: it is exact for the model, not for the world (Sutton 2018). No row compares predicted gain with realized CU-aligned outcomes.
2. **The budget frontier and the coalition allocator carry no transferable guarantee.** The reasons, each from VERIFIED or VERIFIED-SOFTENED rows (Kempe 2015, Borodin, Bharathi, Golovin 2011, Manchanda 2020):
   - the kernel is a two-colour competitive race with bystander mobilization;
   - marginals are fac-weighted and clamped at zero;
   - MC is only 20;
   - the opponent set changes with S;
   - the coalition water-fill is not greedy at all.

   Calling these the "submodular tier" overstates what is known.
3. **The stance money gate is the best-aligned component.** Every row points the same way: do not pay to convert opponents. The rows are Ansolabehere 2002, Bonica 2013, Bombardini 2020 on Hall-Deardorff, and Madsen 2017 as an UNVERIFIED lead. Each is indirect for CU PAC money in the current Congress.
4. **Break-even and ROI rest on an unvalidated functional form.** The share-type ratio a/(a+b) has no evidence row testing ratio against level. The ROI literature in the corpus warns that naive return figures run implausibly high (Bombardini 2020). `roi_verify.py` was not found in the repo.
5. **The workbook allocator is effectively a GAP.** There is one weak conceptual row. The system side could not be reproduced: column U has no cached values and `fec/apply_alloc_cap.py` does not exist.

**Paul's citations missing from the corpus.** Paul's positions cite five works that have no evidence row: Huneeus & Kim 2018, Furnas et al. 2022, Leskovec et al. 2007 (CELF), Kim J. et al. 2025 and Kempe et al. 2003. Until they are ingested, the positions that rest on them (ROI framing, fundraising goals, "cultivate champions") cannot be checked against their own sources.

## Joint CU budget optimizer

**Computes.** Allocation of a $2.5M pot in $500 steps maximizing total winnability, with a per-candidate cap PMAX $10k.

**Method.** CELF lazy greedy on per-member concave chunks. Modeled conversions rebuild the queue. Exact only in a fixed stance state. Golovin-Krause bound as a diagnostic. Knee at 20% of peak marginal.

**Code refs.** `fec/engine/optimize.py:1-177`

**Assumptions.**
- The objective is separable and concave in CU dollars (checked numerically)
- The pot is one PAC's budget (F2 ruling)

**Evidence table**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[pritoni-2025-lobbying-coalitions-determinants]] | 2025 | qualifies | The literature finds both positive (Junk 2019; Nelson and Yackee 2012) and negative (Bunea 2013; Haider-Markel 2006; Heinz et al. 1993; Junk 2020a) links between forming a coalition and policy influence, and calls the results contradictory. The review leaves coalitions as an explanatory variable for influence to future research. | The weapon-of-the-weak evidence might tempt someone to pool allied industries' dollars into the CU money leg. The review does not support that, because the effect of coalitions on outcomes is unresolved. The single-actor pot remains defensible on this evidence. | weak | VERIFIED |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | method-source | Lazy greedy (Algorithm 2) loses no solution quality because stale marginals upper-bound current ones. Speedups were about 3.5-7x fewer evaluations on 46 sensors (temperature) and 30-38x on 357 sensors (traffic) at 50 percent failure. The Lemma 6 data-dependent bound was tighter than the worst-case bound in the sensor runs. | Carries over in the deterministic fixed-state case where per-member chunk marginals are non-increasing, which the code's concavity check tests. It does not carry over once modeled conversions fire; the code then withholds the certificate. The code citation Golovin-Krause JAIR 11 Lemma 5.3 / A.9 does not match this text copy, which has Lemma 6 Eq.13 and Lemma 37 for costs. | strong | VERIFIED |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | Theorem 5's (1-1/e) bound compares with the best adaptive policy and needs observed outcomes, an adaptive monotone submodular objective and a known prior (Def 2-3 p.8; Thm 5 p.11). Treasure-hunting instances with strong synergies fall outside the class (Sec 3.4 p.9). Theorem 26 shows no O(/E/^(1-eps/beta)) approximation for a constructed treasure-hunting family of pointwise-modular instances unless PH = Sigma_2^P (Sec 12 p.33). | The paper supports the refusal to claim a certificate in conversion mode, since a threshold jump can make a later chunk's marginal rise. Softened scope: Theorem 26 is a hardness result for one constructed family, not for every objective lacking adaptive submodularity. Sec 12 frames it more broadly, but the proof covers only that family. | strong | VERIFIED-SOFTENED |
| [[cagala-2021-optimal-targeting-fundraising]] | 2021 | qualifies | Warm-list targeting rule (2014 field experiment, first-year net donations; warm list N=2,354 with 1,180 gift and 1,174 control; 20-fold cross-validated out-of-sample): gift to 33 percent of warm-list individuals; expected net donation 17.61 euro, which is +2.14 (13.8 percent) vs all-gift, +2.20 (14.3 percent) vs no-gift and +2.17 (14.1 percent) vs random-half, all significant at 1 percent (Table 2, Panel B). On the full 2015 follow-up sample (N=3,616): expected 17.16 euro, -0.88 vs all-gift and +0.45 vs no-gift, neither significant (Table 8, Panel B2). | The transferable part is the evaluation design: benchmark-relative, net-of-cost gains on held-out units under randomized treatment. Effect sizes do not transfer (1.16-euro gift to German church donors in 2014-15, vs PAC dollars to members of Congress). FEC limits and the absence of randomization mean the system cannot replicate the identification. Any optimizer gain stays model-internal and UNVERIFIED (DE-062) until outcomes are scored against a benchmark allocation. | moderate | VERIFIED |
| [[arya-2025-relink-edge-activation]] | 2025 | qualifies | RELINK's learned edge policy is slower than PSNA on small and medium graphs and becomes competitive or often faster on large graphs, which the paper attributes to GPU inference and PSNA's complexity (Sec 6.2 Q3, Fig 5). Training took 6,257.329 s for 2,000 epochs because exact marginal gains are computed (Sec 6.4). The objective is non-submodular (Introduction). | The paper gives no graph-size threshold, so the tens-of-thousands figure in the rel is not from the paper. RELINK's case for a learned policy rests on scale and on non-submodular edge choice. The separable allocation in optimize.py is neither, so this paper neither supports nor refutes replacing greedy there. All benchmarks are synthetic-trained IC simulations. | moderate | VERIFIED-SOFTENED |
| [[borodin-nd-threshold-models-competitive]] |  | qualifies | In the weight-proportional competitive LT model (Eq. 1) the A-spread is not monotone (Theorem 1: root A-probability 11/40 before and 103/400 after adding one A seed, Appendix A Theorem 12) and not submodular (Theorem 2). The Nemhauser-Wolsey-Fisher greedy bound does not transfer to that model. | Structural analogy only. Borodin non-monotonicity runs through multi-step propagation under random thresholds, which pflip does not have. The conversion-path concern applies only when conversion_reachable is true. Whether the mAt renormalisation lowers total winnability after a conversion is not measured in this text. | moderate | VERIFIED-SOFTENED |
| [[cai-2025-competitive-influence-maximization]] | 2025 | suggests-extension | With the budget fixed, choosing a start time raises final vote share over spending from t=0 (Table 2, constant-opponent setting). For N=100 networks, per-node start times and budgets raise vote share further: the relative gain is large at small budget ratios but small in absolute terms (Fig 6a relative axis 1.00 to 1.25). A resource-limited controller starts later and a resource-rich one starts earlier (abstract; Sec 4.3.1). Under the uniform-start formula (Eq 3-5) the controller spends its budget over a short late window; the paper does not prescribe this as a rule. | Only as a hypothesis. PAC dollars cannot be withdrawn, FEC per-cycle limits bind, and the paper's continuous gain a_i(t) has no clear counterpart in lump-sum contributions. The paper's large relative gain at small budgets means the per-node result should not be read as uniformly small. Untested on legislators. | weak | VERIFIED-SOFTENED |
| [[moreno-2021-shadowing-shielding-effective]] | 2021 | qualifies | With a passive opponent the continuous optimum spreads widely: 111 of 1,133 nodes (H plus A) get above-average allocation and 1,013 nodes (V) are near flat with std/mean 0.11. Continuous optima beat the best discrete plan by 5-10 percent for passive K below 100 (Fig 1c, 15 samples). With two active controllers the unique pure-strategy Nash equilibrium is uniform targeting w_ai = Ba/N, reported as independent of network type and budgets (numerical result; the uniqueness proof is in S1 Appendix Section 12, not in the text file). | The paper's spread and shielding come from money propagating through the network's steady state. Melange's dollar term is node-local, so the paper's spread cannot arise from dollars alone; shielding-like behaviour can arise only through the b term of a/(a+b) and through conversion-driven stance moves. The Nash result implies the plan is a best response to a frozen opponent and is exploitable if industry re-optimises; qualitative only. Equal-budget and per-candidate FEC limits have no analogue in the paper. | moderate | VERIFIED-SOFTENED |
| [[zeng-nd-multi-agent-reinforcement]] |  | context | An RL controller (Q-learning, linear function approximation, alpha 0.1, gamma 0.75) beats a rule-based Boid baseline on one simulated drone scenario: near-collisions 45 vs 108 (-58%), ideal-range steps 3,176 vs 2,764 (+15%), heading deviation 14.3 vs 21.1, all over 3,500 steps with N = 17 UAVs. No seeds, replicates, variance or baseline specification are given. | Carries over only as an analogy: a learned policy versus hand-set rules in a multi-agent setting. Nothing transfers to political allocation. The agents are physical drones, the reward is hand-specified geometry, and there is no budget, submodularity, money or legislator response. The evidence is one unreplicated simulation, so it cannot show that RL would outperform greedy anywhere, let alone on the cosponsor kernel. | weak | UNVERIFIED |
| [[zareie-2023-influence-maximization-social]] | 2023 | qualifies | Cost-aware (budgeted) IM assumes every influenced user brings the same benefit. Profit-aware IM models per-user benefit, but data on users' monetary concerns and valuations may not always be readily available, so cost-aware methods may be more relevant in practice. Nguyen and Zheng (2013) first introduced budgeted IM. | Partial analogy. The optimizer allocates money that shifts per-member flip odds and does not pick cascade seeds, so the survey's spread objective does not apply. The survey's point carries over: results depend on the PERS and IMP tables, which the code labels calibrated constants. A uniform-benefit variant would be a sensitivity check, not a replacement. | weak | VERIFIED-SOFTENED |
| [[almazaydeh-2026-heuristic-centrality-methods]] | 2026 | qualifies | Theoretical guarantees such as the (1-1/e) bound can diverge sharply from realized performance when marginal gains are estimated by noisy simulation. CELF++ reached 12.9 percent of IMM at 1000 nodes. | The paper's gap comes from MC estimation error, which does not arise in a deterministic objective, so the paper does not undercut the optimizer's fixed-state certificate. It would become relevant only if the objective were replaced by a simulated cascade. | moderate | VERIFIED |
| [[sutton-2018-reinforcement-learning-introduction]] | 2018 | qualifies | Acting greedily on current value estimates without exploration can lock onto suboptimal actions. On the 10-armed testbed (2000 independent problems, 1000 steps each), the greedy method averages about 1 reward per step against a best possible of about 1.54, and epsilon-greedy does better in the long run. Planning on an incorrect model is likely to compute a suboptimal policy (Sec. 8.3); planning uses a model, learning from experience uses observed interactions (Sec. 8.1). | Exploration pays only if the same allocation repeats and outcomes are observed and fed back. Per-cycle CU outcomes are sparse and confounded, so a bandit layer would learn slowly. The transferable point holds: the optimum is exact only for the model, not for the world. | moderate | VERIFIED-SOFTENED |
| [[sutton-2018-reinforcement-learning-introduction]] | 2018 | suggests-extension | Greedy contextual-bandit policies maximize immediate clicks and ignore long-term effects; an MDP lifetime-value policy (Theocharous et al.) beat greedy on the LTV metric, while greedy won on CTR. In the illustrative Figure 16.8, CTR is 0.35 and LTV is 1.5. | Holds only if repeated CU giving to the same member changes later responsiveness. That is untested here. Per D-025 it must not be phrased as access or closeness. Any multi-cycle objective needs owner authorization and a second-path reproduction (DE-062). | weak | VERIFIED-SOFTENED |
| [[watkins-1992-q-learning]] | 1992 | qualifies | The convergence theorem needs a look-up table and an infinite number of episodes for every starting state and action (p.281). The look-up table caveat and the citation of Watkins 1989 (that Q-learning may not converge correctly for other representations) are on p.281. No rate or finite-sample bound is given. Section 4 (p.286) says reusing stored episodes moves Q-learning toward certainty-equivalence planning, which is the limit of re-presenting old episodes infinitely often. Certainty equivalence costs dearly early in learning, citing Barto and Singh 1990 (p.280). | The theorem covers only a finite MDP with look-up-table values and unbounded sampling, and this optimizer has neither. The item's claim that one cycle over 533 members yields at most a few observed outcomes per member-action pair is not in the code or the paper and is unverified here. GCOMB and S2V-DQN use n-step or fitted Q-learning with function approximation, which this paper does not cover (checked in their item files). The claim that exact greedy is optimal in a fixed state rests on the docstring's separability and diminishing-returns assertion, which I did not independently check. | moderate | VERIFIED-SOFTENED |
| [[dai-2017-learning-combinatorial-optimization-graphs]] | 2017 | qualifies | Dai et al. report that S2V-DQN beats hand heuristics by wide margins on MemeTracker MVC (ratio 1.0021 vs 1.2220 for MVCApprox-Greedy and 1.4080 for MVCApprox) and on Ising MaxCut (1.0223 vs 1.2825 for MaxcutApprox), on one MemeTracker graph and ten Ising instances. The paper does not study budget allocation or separable resource objectives. | The paper's gains are on synthetic graphs and benchmark graph instances, scored against CPLEX best-found values with a one-hour cutoff. The paper says nothing about separable concave allocation, so it gives no support for the optimizer's exactness; that rests only on the code's own docstring and concavity check. A learned replacement would need many training instances, which the system does not have. | weak | VERIFIED-SOFTENED |
| [[manchanda-2020-gcomb-learning-budget-constrained]] | 2020 | qualifies | GCOMB's advantage is wall-clock speed on large graphs: about 10 times faster than greedy on MCP; speed-up over CELF on YT falls from 4.0 at b=20 to 2.01 at b=200 (Table 2b). Training saturates within 10 minutes (Fig 2a, synthetic BP 5k). Sequential and capacity-constrained problems are named as future work (Conclusion p.9). | The 10x and 2.01 to 4.0 speed-ups are for MCP on real graphs of 1M+ nodes, not for a small capacity-constrained allocation. A GCOMB-style learned policy would fit only pure set selection without per-member caps at much larger scale. The paper reports no quality gain over greedy. | moderate | VERIFIED-SOFTENED |
| [[kempe-2015-maximizing-spread-influence]] | 2015 | supports | Thm 7.1 (p.128): hill climbing with total budget k, taken in pieces of size delta, on a non-negative monotone non-decreasing g with diminishing returns, ends with g(x) >= (1 - exp(-k gamma/(k + delta m))) times g of the optimum under sum <= k. Each step picks a direction with gamma-approximately largest gradient. | Holds only in the fixed stance state where concavity is checked. Once money crosses a defection threshold, returns are non-concave steps and neither Thm 7.1 nor the exact certificate applies. The code states this scope. | moderate | VERIFIED |
| [[fremeth-2018-spillovers-regulating-corporate]] | 2018 | qualifies | Per-candidate PAC limits bind for 14.4 percent of candidates receiving any PAC money (text Section 3, p.8; not in Table 1). Firm-cycles at the limit show +549,166 dollars of lobbying per cycle (Table 3 col 1, SE 225,013; 2000-2008, 787 firms, 2,783 firm-cycles, pooled across the 2002 BCRA break). Firms at the limit show CEO giving 0.066 more likely (Table 2, 1992-2008, 950 firms). Authors say PAC limits are effectively non-binding and all estimates are partial correlations. | Cap mechanics transfer if the 10,000 per-cycle limit is the same. The paper does not say the limit is unindexed; that is an assumption not stated in the paper. Spillover magnitudes come from large S&P 500 for-profit firms, not CU PACs, and are partial correlations. Supports a caveat that PMAX bounds PAC dollars only; does not justify reweighting. | moderate | VERIFIED-SOFTENED |

**Assessment**

**What it is.** A CELF-style lazy greedy over $500 chunks of a $2.5M pot, with a PMAX cap of $10k per member above existing CU money. The objective is modeled total winnability (expected weighted flips).

**Evidence: 18 rows (5 VERIFIED, 12 VERIFIED-SOFTENED, 1 UNVERIFIED).**

- **Mechanics (strong support).**
  - Golovin & Krause 2011 (VERIFIED, method-source, strong): lazy greedy loses no quality, because stale marginals upper-bound the current ones. Their speedups were 3.5-7x fewer evaluations at 46 sensors and 30-38x at 357 sensors.
  - Kempe 2015 Thm 7.1 (VERIFIED, supports): hill climbing in budget pieces on a monotone function with diminishing returns gets a (1 - exp(-k*gamma/(k + delta*m))) fraction of the optimum.
  - Together these cover the fixed-state case, where the code's concavity check tests whether per-member chunk marginals are non-increasing. In that case the separable greedy is the exact grid optimum *of the model*.
  - Golovin's Theorem 5 and Sec 3.4 (VERIFIED-SOFTENED, strong) support withholding the certificate in `modeled_threshold_replanning` mode. A threshold jump can make a later chunk's marginal rise. That mode also observes nothing (observations = []), so the adaptive theory does not apply.
- **Scope cautions.**
  - Borodin (VERIFIED-SOFTENED): after a conversion, the mAt = max(nA) renormalisation in `topic_setup` can lower other members' WP*nA/mAt terms. Whether that lowers total winnability is not measured.
  - Moreno 2021 (VERIFIED-SOFTENED): industry money (`ind0`) is a frozen passive opponent. In their two-controller game the unique Nash equilibrium is uniform targeting (a numerical result), so the plan is a best response to a fixed opponent and could be exploited if industry re-optimises. Qualitative only.
  - Sutton 2018 (VERIFIED-SOFTENED): greedy exploitation of a modeled response, with no exploration and no outcome feedback, is optimal for the model only.
  - Watkins 1992 (VERIFIED-SOFTENED): tabular Q-learning guarantees need infinite sampling. Sparse per-cycle outcomes would not support a learned policy.
  - Fremeth 2018 (VERIFIED-SOFTENED): per-candidate PAC limits bound 14.4% of PAC-receiving candidates (2000-2008, 787 S&P 500 firms, partial correlations, pooled across the 2002 BCRA break, so UNSTRATIFIED). Firms at the limit show +$549k of lobbying per cycle. So PMAX bounds PAC dollars only, not total political spending.
  - Pritoni 2025 (VERIFIED): the coalition-to-influence link is contradictory in the literature. Treating the pot as one actor is defensible.
- **Evaluation design.** Cagala 2021 (VERIFIED) is the only row that shows how to score an allocator against reality: net-of-cost gains on held-out units under random assignment, compared with all/none/random baselines. On their warm list (N = 2,354, 2014) targeting gave +13.8%. On the 2015 follow-up (N = 3,616) the gain was no longer significant. That fragility is itself the lesson. The effect sizes do not transfer.
- **Learned-policy evidence.**
  - Manchanda 2020 (VERIFIED-SOFTENED): GCOMB is about 10x faster than greedy on MCP, and 2.01-4.0x faster than CELF on YT, on graphs of 1M+ nodes. It reports no quality gain, and it names capacity-constrained problems as future work.
  - Dai 2017 and Arya 2025 (VERIFIED-SOFTENED, weak to moderate): their wins are on non-separable graph problems at scale.
  - Zeng (UNVERIFIED): a drone simulation. It is a lead with no transfer.
- **Citation mismatch.** The code cites Golovin-Krause "Lemma 5.3/A.9". The text copy has the data-dependent bound at Lemma 6, Eq. 13, with costs in Lemma 37.

**Bottom line.** The exactness claim is sound within its own scope. Whether the optimizer helps in the real world is untested; no row bears on the empirical question.

**Contradictions**
- Paul expects a learned policy (GCOMB) to replace greedy at scale. Manchanda 2020 (VERIFIED-SOFTENED) shows GCOMB's gain is wall-clock speed on 1M+ node graphs with no quality gain over greedy, and lists capacity-constrained problems as future work. Dai 2017 and Arya 2025 qualify it the same way. The built problem is a separable, capped allocation over 533 members, where greedy is already exact in a fixed state. The evidence contradicts the expectation that a learned policy would add value here.
- The code's citation of Golovin-Krause Lemma 5.3/A.9 does not match the text copy, which has Lemma 6 Eq. 13 and Lemma 37 (VERIFIED row). The proof the code relies on is cited at the wrong location.
- "Exact optimum" in the docstring holds for the model state only. Sutton 2018 and Moreno 2021 show the plan is a best response to a frozen industry opponent and has no outcome feedback. That is not a contradiction of the code. It does contradict any reading of the output as a real-world optimum.

**Gaps**
- No row tests the empirical question: does predicted greedy gain track realized CU-aligned outcomes in funded vs comparable unfunded members? The study that would fill it is a prospective, preregistered comparison. Score the optimizer's allocation and a uniform, random or proportional benchmark against realized cosponsorship or votes on CU bills in the next cycle, with matched unfunded members as the comparison group. Name the window and denominator. Use the Cagala-style benchmark-relative, net-of-cost design. Randomization is not available under FEC rules, so this stays observational and labelled as such.
- Not measured: whether the mAt renormalisation after a modeled conversion lowers total winnability. A read-only diagnostic on conversion-mode runs would settle it.
- No evidence on an opponent that responds, such as industry re-allocating. A sensitivity run with a reactive opponent would bound how exploitable the plan is.
- Paul's own citations for this component, Leskovec 2007 (CELF) and Huneeus & Kim 2018, have no evidence rows.

**Paul's position vs the evidence**

**First half (adaptive greedy plus CELF as the operational layer): matches the build and is well supported**, with one correction. The built optimizer is lazy greedy in a fixed stance state. It is not adaptive in the Golovin-Krause sense, because nothing is observed. The adaptive guarantee is correctly withheld in conversion mode.

**Second half (GCOMB replacing greedy at scale): not supported for this component.** GCOMB's evidence is speed on graphs with millions of nodes and no quality gain. The problem here is small, separable and capped, and greedy is already exact for it. A learned policy would also need many training instances, which the system does not have.

**Pritoni 2025** supports Paul's single-PAC pot ruling (F2). **Fremeth 2018** adds the caveat that PMAX caps PAC dollars only.

**Open questions**
- Should the code's Golovin-Krause lemma reference be corrected to the text's Lemma 6 / Eq. 13? This is an owner change under change-control.
- Is a reactive-opponent sensitivity check wanted, given Moreno's Nash result? It is qualitative evidence only.
- Would Paul accept an observational funded-vs-unfunded backtest as the scoring route, given that FEC limits rule out randomization?

**Read first:** [[golovin-2011-adaptive-submodularity-theory]], [[kempe-2015-maximizing-spread-influence]], [[manchanda-2020-gcomb-learning-budget-constrained]], [[cagala-2021-optimal-targeting-fundraising]], [[moreno-2021-shadowing-shielding-effective]], [[fremeth-2018-spillovers-regulating-corporate]]

## Stance money gate (F1)

**Computes.** Members with overall tier Skeptic, Highly Skeptical or Opponent get zero money in every allocator.

**Method.** Applied before the top-45 shortlist. Fails open for members with no matrix row. Before the gate 10 opposed members were funded $54,500; after it, 19 were gated and none funded.

**Code refs.** `fec/moneyball_core.py:713-729`, `fec/engine/optimize.py:44-52`

**Assumptions.**
- Overall stance should gate money; one topic cell should not

**Evidence table**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | supports | Hall and Deardorff (2006) treat information as a subsidy to resource-strapped legislators, which explains why lobbyists lobby friendly legislators and questions the persuasion role. Hirsch et al. (2019; FARA contacts, 219 foreign-interest lobbyists, 110th-111th Congress) find any lobbying contact is more likely for lobbyists ideologically aligned with the politician. | The contact evidence concerns foreign-principal lobbyists under FARA, not domestic PAC dollars. It is consistent with funding allies but does not test whether contributions to opposed members change their positions. | weak | VERIFIED |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | supports | Text PDF p.18 (printed p.17): of 49,418 individuals with 25+ federal contributions (2004-2012), only 167 (0.3 percent) have money-weighted SD above the 0.577 random-giving baseline. Mean money-weighted SD is 0.20 for the sample. Descriptive evidence that active individual donors give on ideology. | Descriptive only: donors are ideological, but the paper does not test whether funding opponents converts them. Window is individual donors 2004-2012; PAC behavior differs (Table 2). | weak | VERIFIED-SOFTENED |
| [[madsen-2017-strategic-advantages-micro]] | 2017 | supports | Contact spent on audiences with an unfavourable view of the source is wasted or counterproductive. The targeted campaign's advantage comes entirely from never contacting those voters. | An analogy only. Campaign contacts with voters are not PAC dollars to legislators, and the paper has no money mechanism. It agrees with Paul's Furnas reading ('cultivate champions') but adds no new evidence about legislator behaviour. | weak | UNVERIFIED |
| [[ansolabehere-2002-little-money-politics]] | 2002 | supports | With member fixed effects, changes in donations to an individual legislator do not translate into changes in that legislator's roll-call voting. A third explanation for group giving is to elect sympathetic legislators rather than to change the views of sitting ones. | The evidence is general business and labor PAC money against CCUS scores (1978-1994), not CU-specific bills or the current Congress. It supports not paying to convert opposed members. It does not measure electoral help to allies. | moderate | VERIFIED-SOFTENED |

**Assessment**

**What it is.** Members whose overall tier is Skeptic, Highly Skeptical or Opponent get zero money in the allocators. The gate fails open when a member has no matrix row.

**Evidence: 4 rows (1 VERIFIED, 2 VERIFIED-SOFTENED, 1 UNVERIFIED).** All of them support the direction. None tests it directly on CU money.

- **Ansolabehere et al. 2002** (VERIFIED-SOFTENED, moderate) is the strongest row.
  - With member fixed effects, changes in donations to a legislator did not translate into changes in that legislator's roll-call voting.
  - Window and data: general business and labor PAC money against CCUS scores, 1978-1994.
  - This is the closest available test of "do contributions convert members", and it says no. It is old, it uses a business-wide score rather than CU bills, and it is pooled across parties (UNSTRATIFIED).
- **Bombardini 2020 on Hall-Deardorff** (VERIFIED, weak): lobbying works as a subsidy to friendly legislators. Its contact data are FARA, foreign-principal lobbyists, 110th-111th Congress, not domestic PAC dollars.
- **Bonica 2013** (VERIFIED-SOFTENED, weak, descriptive): of 49,418 individuals with 25 or more federal contributions in 2004-2012, only 167 (0.3%) give across ideology above the random baseline. This is individual-donor behaviour, and PACs differ.
- **Madsen 2017** (UNVERIFIED): contacting audiences with an unfavourable view is wasted. It is a voter-contact analogy and a lead, not a finding.

The code side was partly confirmed. `STANCE_GATE_BLOCKED` is defined at moneyball_core.py:717 and applied in optimize.py:52, per_topic_flip.py:37 and per_topic_coalition.py:26. The claim that gated members get zero money *in every allocator* and still count in the objective was not fully confirmed in the evidence pass. The workbook allocator is not shown to apply the gate.

**Contradictions**
- No evidence contradicts the gate's direction.
- Scope tension: the gate tests the overall tier. The evidence (Ansolabehere's member FE on a general score) is about overall voting behaviour, which fits overall gating. No row addresses the assumption that one topic cell should not gate. That assumption is unevidenced, not contradicted.

**Gaps**
- The empirical question is untested: does funding opposed members change their positions or produce counter-strategy effects? The study that would fill it is a member fixed-effects panel of CU and allied PAC contributions against positions on CU bills (cosponsorship, votes) in recent Congresses. It should be stratified by party and chamber, with window and denominator stated, in the Ansolabehere design and updated to current data.
- Fail-open behaviour is unevidenced. Nobody has counted how many members with no matrix row received money, or how much. A read-only audit would fill this.
- Whether the workbook allocator applies the gate is unconfirmed.
- Furnas et al. 2022, the citation Paul's position rests on, has no evidence row in the corpus.

**Paul's position vs the evidence**

Paul reads ideological money as cultivating champions rather than recruiting them. **This agrees with every row.** Ansolabehere's member-FE null on conversion, Bonica's ideological donors and the Hall-Deardorff subsidy-to-friends view all point the same way. Madsen agrees too, but it is UNVERIFIED and an analogy.

The support is indirect: old windows, non-CU money, and FARA or individual donors rather than CU PACs. **Paul's own source (Furnas 2022) is not in the corpus to check.**

**Open questions**
- Should the fail-open default stand? A member with no matrix row is currently fundable.
- Should Furnas et al. 2022 be ingested so the 'cultivate champions' reading can be checked against its text?

**Read first:** [[ansolabehere-2002-little-money-politics]], [[bombardini-2020-empirical-models-lobbying]], [[bonica-2013-mapping-ideological-marketplace]], [[madsen-2017-strategic-advantages-micro]]

## Per-topic flip and coalition allocators

**Computes.** Per-topic diagnostic allocations: greedy $1k steps on flip gain; coalition water-fill over the top-45 neutral recruits weighted by marginal spread x (1 + 0.8 bpf).

**Method.** Budget $2M per topic, cap $50k. Joint vs naive additive spread is reported (non-additivity).

**Code refs.** `fec/engine/per_topic_flip.py:1-100`, `fec/engine/per_topic_coalition.py:28-130`, `fec/engine/build_im_allocation.py:31-47`

**Assumptions.**
- Bridge-builders are better seeds
- Topic budgets are diagnostic and not summable

**Evidence table**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[trivedi-2020-efficient-influence-maximization]] | 2020 | suggests-extension | Degree-discount (DHICM) discounts neighbours of already chosen seeds (Eq 3.2). The paper reports DHICM above plain degree by 16% at k=30 and 10.4% at k=80 on NetHept, and by 10% at k=50 on NetPhy. Plain degree therefore trails DHICM by less than those percentages, not by them. | The paper's direction is DHICM over degree, so the gap is an extension argument, not a measured gain. The percentages are simulated, depend on k, come from one benchmark, and have no error bars. A sequential or discounted pass is a candidate for owner review only. | moderate | VERIFIED-SOFTENED |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | Near-optimality comes from greedy selection that conditions each pick on what has been selected or observed. For deterministic priors the notion reduces to classical submodularity, so Nemhauser et al. (1-1/e) applies (Sec 3.1-3.2 pp.6-8; Thm 5 p.11; Conclusions p.36). | Flip allocator: with fixed-state separable concave pflip, greedy is exact, so the paper adds nothing. Coalition allocator: the singleton-marginal proportional split is not greedy, so no submodular guarantee applies even if spread were submodular; reporting joint spread is the right diagnostic. Submodularity of the bystander-filtered competitive kernel is unverified. | moderate | VERIFIED |
| [[ma-2022-adversarial-attack-graph]] | 2022 | qualifies | Table 1: both InfMax methods beat Degree, Betweenness and PageRank in all 18 setups. The paper says both proposed methods beat all baselines in all but 3 of 18 setups. Pubmed GAT at 30 percent threshold: GC-RWCS 69.1, InfMax-Unif 64.1, no attack 85.2. In a 4-community SBM the better selectors spread seeds more evenly (qualitative). | The paper's objective is GNN misclassification on citation graphs at a 1 percent node budget. The CU objective is a Monte Carlo competitive cascade on the cosponsor kernel. The structural point transfers: weighting by singleton marginals ignores overlap, so funded seeds can cluster. The code already measures the joint-vs-naive gap. The community-spread finding is qualitative. Weak to moderate. | weak | VERIFIED-SOFTENED |
| [[chen-2021-contingency-aware-influence]] | 2021 | context | Within a round, a seed's exact expected marginal contribution depends on which earlier seeds turned out willing, giving O(2^b) cases. The paper assumes the influence function is submodular, stated as usually submodular and citing Kempe et al. 2003 (Eq. 7). | The paper's O(2^b) dependence arises from realized willingness within sequential sub-steps. The coalition allocator averages willingness inside each spread() call, so there is no conditional dependence to ignore at allocation time. The shared point is only that seed non-additivity is real and the joint report exposes it. The surrogate reward needs homogeneous q and Eqs. 9-10, so it does not port to heterogeneous gates. | weak | VERIFIED-SOFTENED |
| [[cai-2025-competitive-influence-maximization]] | 2025 | qualifies | Under node-specific optimization on N=100 networks with mean degree 6 (Sec 4.3.2), the budget ratio sets the degree pattern. With small budget ratios, control gains fall and start times rise with degree, so low-degree nodes are targeted. With large ratios the pattern reverses (Fig 6b-c p.18; Appendix D Fig 10 p.26). | bpf is a cross-party share, not degree, so the paper's degree result maps only indirectly. The paper's result rests on N=100 graphs with mean degree 6. Measuring the Melange cosponsor graph's degree exponent and testing whether greedy seeds move toward low-degree members at small budgets would be new analysis and stays UNVERIFIED. | weak | VERIFIED |
| [[zareie-2023-influence-maximization-social]] | 2023 | suggests-extension | Topic-aware IM (Barbieri, Bonchi and Manco 2013) represents each user's interest as a continuous topic vector. The survey says this captures preferences more realistically than binary target or label methods, but determining topic vectors needs a good record of historical activity, which may not always be available. The survey reports no accuracy comparison of any kind. | Transfers only if per-category cosponsorship history is dense enough to estimate topic-specific edge weights. A topic-conditioned kernel would change ranks and engine outputs, so change-control applies. The survey gives no evidence that topic conditioning beats topic-blind IM, so any benefit is conjectural. The recorded crypto p_max of 0.23 was not checked. | weak | VERIFIED-SOFTENED |
| [[bharathi-nd-competitive-influence-maximization]] |  | qualifies | Reach is submodular in a player's own seeds, so summed single-seed marginals overstate the joint gain; the (1-1/e) guarantee belongs to greedy, not to individual marginals (Lemma 1, Theorem 1). | Submodularity implies the joint report is at most the naive additive sum, so the joint report is supported as inference. The paper gives no guarantee for a proportional water-fill. The bystander set excludes funded seeds, so the spread is not Lemma 1's fixed-opponent function. The figures are one allocation run per topic at MC 20. | moderate | VERIFIED-SOFTENED |
| [[chen-2023-complex-contagion-influence]] | 2023 | suggests-extension | Credit sparseness in an 8-node toy (K=3, T=4): only 17 of 70 four-seed sets (24.3 percent) are effective, and the seed sequence 1,2,3,5 earns per-step rewards 1,1,1,2, so a seed's value shows only alongside specific other seeds. | A joint gain above naive-additive on a topic would show complementarity that solo ranking cannot see. Under pure IC the joint should stay at or below naive-additive, so the printout is a real check only if threshold effects exist. The 45-candidate cut uses engagement rather than marginal value, so a seed valuable only in combination can be dropped before any marginal is computed. | moderate | VERIFIED-SOFTENED |
| [[kempe-2015-maximizing-spread-influence]] | 2015 | qualifies | Seeds picked by individual degree or distance centrality ignore clustering and overlap. On the hep-th graph under LT, greedy beats high-degree by about 18% and distance centrality by over 40%. Under IC with p=10%, the centrality heuristics level off near 2415 activations and random seeds beat them beyond about 12 targets. Greedy keeps rising because it prices overlap. | Evidence is one 10748-node co-authorship graph under uniform p, window winter 2002. Whether singleton ranking loses comparably on the 533-member cosponsor kernel is unverified; a check would compare the joint spread of the water-fill seed set against lazy greedy at equal seed count and the same CRN. The allocator is labelled diagnostic, which limits the harm. | moderate | VERIFIED |
| [[harbridge-yong-2023-bipartisan-path-effective]] | 2023 | qualifies | Returns to bipartisan composition are nonlinear. One SD below the mean costs about 22 percent of LES in the House and 16 percent in the Senate; one SD above adds about 8 and 4 percent. Gains flatten and reverse past a 50/50 mix. | This holds only if the seed boost is meant to proxy a recruit's legislative effectiveness. The paper says nothing about diffusion seeding, so the link is an analogy and the allocators are diagnostic. | weak | VERIFIED |

**Assessment**

**Two different allocators sit under this heading.**

**1. Flip allocator.** A greedy in $1k steps on pflip chunks. In a fixed state it is separable and concave, so greedy is exact. Golovin 2011 (VERIFIED) adds nothing beyond that.

**2. Coalition allocator.** The steps are:
- shortlist the top 45 neutral members by engagement priority (ev);
- score each by its singleton marginal, spread({n}) minus base;
- boost by (1 + 0.8*bpf);
- water-fill $2M with a $50k cap.

It is **not greedy**, so no submodular guarantee applies even if spread were submodular (Golovin 2011 VERIFIED; Bharathi VERIFIED-SOFTENED).

**Evidence: 10 rows (4 VERIFIED, 6 VERIFIED-SOFTENED).**

- **Overlap blindness.**
  - Kempe 2015 (VERIFIED, moderate): on one co-authorship graph (10,748 nodes, winter 2002), greedy beats high-degree seeds by about 18% (LT) and distance centrality by over 40%. Under IC at p = 10%, centrality picks plateau and random seeds beat them beyond about 12 targets. Singleton-marginal ranking has the same overlap blindness.
  - Trivedi 2020 and Ma 2022 (VERIFIED-SOFTENED) point the same way.
  - The code already reports joint spread against the naive additive sum. That is the right diagnostic, but it does not shape the allocation.
- **Joint vs naive.**
  - Bharathi: if spread is submodular in own seeds, joint spread is at most the naive sum.
  - Chen 2023 (VERIFIED-SOFTENED): under threshold-like joining, joint can exceed naive. A seed valuable only in combination can then be dropped by the engagement-based top-45 cut before any marginal is computed.
- **Bridge boost.** Harbridge-Yong 2023 (VERIFIED, weak): returns to bipartisan composition are nonlinear. One SD below the mean costs about 22% of LES (House) and 16% (Senate). One SD above adds about 8% and 4%. Gains flatten and reverse past a 50/50 mix. The boost (1 + 0.8*bpf) is monotone. The transfer is analogical (LES, not diffusion seeding).
- **Budget-dependent targeting.** Cai 2025 (VERIFIED, weak): degree targeting reverses with the budget ratio (N = 100 graphs, mean degree 6). bpf is not degree, so this maps only indirectly.
- **Single kernel for all topics.** Zareie 2023 (VERIFIED-SOFTENED, weak) proposes topic-aware kernels but offers no accuracy evidence. Kernel sharing was verified for per_topic_coalition only.
- **Gating inside spread().** Chen 2021 (VERIFIED-SOFTENED, context) confirms that seed non-additivity is real. Averaging willingness inside spread() removes the O(2^b) conditional dependence at allocation time.

**Contradictions**
- The 'submodular tier' label vs the build: the coalition allocator is a proportional water-fill on singleton marginals, not greedy. Its kernel is competitive, with bystander exclusion that depends on the funded set. The code comment asserts spread is submodular, but no row shows that for this kernel (Bharathi, Golovin, Kempe 2015 all VERIFIED or VERIFIED-SOFTENED).
- Assumption 'bridge-builders are better seeds' with a monotone (1 + 0.8*bpf) boost, vs Harbridge-Yong 2023 (VERIFIED): bipartisan returns flatten and reverse past a 50/50 mix. Analogical transfer, but the shapes disagree.
- The top-45 pre-cut uses engagement priority, not marginal value. Under complex contagion (Chen 2023) this can drop complementary seeds, and the assumption of diminishing returns would then fail.

**Gaps**
- The empirical question is untested: is multi-seed coalition spread sub-additive in *observed* cosponsorship data? One study: for CU bills, compare realized cosponsor joining after multiple sponsors from a member's neighbourhood joined with the sum of single-sponsor effects. Name the Congress, the bill set and the denominator. LDA may identify lobbied bills but must not supply edges or weights (D-021).
- No same-CRN comparison exists of the water-fill seed set against lazy greedy at equal seed count on the 533-member kernel. It would be a read-only model-internal check.
- Submodularity of the bystander-filtered competitive kernel is unverified in any row.
- No evidence on whether topic-specific kernels outperform the shared kernel.
- Kumar 2022 and Kempe 2003 (Paul's citations) appear only through Kempe 2015 and a Kumar row under the frontier.

**Paul's position vs the evidence**

Paul places these allocators in the static and submodular tier of his three-tier architecture.

- **The flip allocator** fits 'static'. Greedy is exact there in a fixed state.
- **The coalition allocator** fits 'static' but not 'submodular' as built. It does not use greedy, and no row establishes submodularity for its competitive kernel.

The evidence supports keeping both allocators diagnostic-only and non-summable, which matches the component's own assumption. Kempe 2015's centrality-vs-greedy gap is a reason to doubt that singleton ranking is a good proxy.

**Open questions**
- Should the water-fill be replaced or compared with a sequential or degree-discount pass (DHICM-style)? This is an owner change under change-control, a candidate only.
- Is the bpf boost meant to proxy legislative effectiveness? If so, Harbridge-Yong's nonlinearity suggests capping or bending it. That would be Paul's ruling.
- Should the top-45 cut be re-ranked by marginal value rather than engagement?

**Read first:** [[kempe-2015-maximizing-spread-influence]], [[bharathi-nd-competitive-influence-maximization]], [[golovin-2011-adaptive-submodularity-theory]], [[chen-2023-complex-contagion-influence]], [[harbridge-yong-2023-bipartisan-path-effective]], [[trivedi-2020-efficient-influence-maximization]]

## Budget frontier with knee and electoral modes

**Computes.** Spread vs cumulative seeds per topic; knee where 4 x gain >= peak; contest factor REPFAC 0.6 x lev.

**Method.** Lazy greedy up to 60 seeds under CRN.

**Code refs.** `fec/engine/budget_frontier.py:1-188`, `fec/engine/mc_stability_check.py:1-44`

**Assumptions.**
- Diminishing marginal returns to seeds

**Evidence table**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kumar-2022-influence-maximization-social]] | 2022 | qualifies | Introduction says greedy IM methods are generally more accurate with a theoretical performance guarantee but computationally intensive, while centrality methods are cheap but network-specific. SGNN replaces the greedy step with a learned per-node influence regressor and top-k selection. No SGNN numbers appear in the accessible preview. | The submodular guarantee is not shown for this two-colour engine with leverage factors, so the claim that the greedy carries a guarantee here should not be made. A learned surrogate would trade away the marginal accounting for speed. Speed matters only if per-candidate MC greedy is infeasible, and the 533-member, 60-seed setting is not shown to be infeasible. | moderate | VERIFIED-SOFTENED |
| [[trivedi-2020-efficient-influence-maximization]] | 2020 | supports | Under ICM with p=0.01 on two co-authorship graphs, DHICM is 12.5% (NetHept k=10) and 7.9% (NetHept k=30) below CELF, and 2.5% below CELF at k=30 on NetPhy. The paper says greedy is slow on medium-sized graphs (more than 1500 nodes) because of its high time complexity. | The gap was measured at p=0.01 on two co-authorship graphs of 15k to 37k nodes, with no MC sample size or variance reported. It ranks algorithms under ICM only and does not validate the frontier's spread values, which are model outputs. The cost argument holds because the candidate pool is about 45, not because of a node-count threshold in the paper. | moderate | VERIFIED-SOFTENED |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | Greedy guarantees assume exact or alpha-approximate marginal evaluation; sampled marginals give only additive error (Sec 5 p.11, bound with epsilon). The adaptive IC result (Thm 19 p.21) covers a single cascade with full-adoption feedback; the myopic-feedback version was retracted with a 3-node counterexample (Sec 8.2.1 p.23). | No Golovin-Krause guarantee carries over as built. Marginals are sampled, selection uses fac-weighted marginals rather than the reported spread, and the kernel is competitive. The zero clamp signals that sampled spread is not monotone. The frontier is a non-adaptive heuristic. | moderate | VERIFIED-SOFTENED |
| [[chen-2021-contingency-aware-influence]] | 2021 | supports | The authors attribute RL4IM's slight edge over greedy CHANGE to proactively modeling unwillingness in the reward, versus CHANGE reacting to realized willingness. The edge is called slight and gets no numbers. At T=1 (single round, T x B = 8), RL4IM and CHANGE are the two best methods. The roughly-twice-Random result is stated for the q sweep (Q3), not for T=1. All results are synthetic graphs under IC with p=0.1. | Supports only the design choice of putting take-up uncertainty inside the expected-spread objective. The paper models one colour; the system has a two-colour competitive cascade. The paper's synthetic graphs (about 200 nodes) are comparable in scale to the 533-member graph, but there are no political data. The slight edge is qualitative. | weak | VERIFIED-SOFTENED |
| [[borodin-nd-threshold-models-competitive]] |  | qualifies | Most natural competitive extensions lose submodularity. Monotonicity can survive while submodularity fails (Theorems 3-6). The greedy (1-1/e) guarantee is shown only for the OR model with monotone submodular decision functions (Theorems 9-11). Section 7 leaves other models open. | Applies to electoral mode (contest and defend seeds). This paper gives no submodularity result for the electoral objective. Any coalition-mode greedy guarantee comes from the competitive IC literature cited to Bharathi et al. (Borodin ref 1), not from this paper, so coalition-only mode is not covered here. Separating MC noise from structure needs a read-only check of raw marginals under common random numbers. | moderate | VERIFIED-SOFTENED |
| [[almazaydeh-2026-heuristic-centrality-methods]] | 2026 | qualifies | At 1000 nodes CELF++ with 50 MC sims per marginal estimate reached 122.95 +/- 24.37 vs IMM 954.90 +/- 1.27, i.e. 12.9 percent of IMM (t = -107.78, p < 0.001). Its CV rose to 71.5 percent at 500 nodes. The authors attribute the gap to MC budget and Python implementation but do not test that. The abstract gives 11.2 percent, which conflicts with the body. | The paper's failure mode is independent noise across candidate estimates. CRN removes that for pairwise marginal comparison, so the CRN design is supported in principle. The paper uses undirected uniform-p IC; Melange runs competitive two-colour IC with seed gating on a 533-node kernel. The paper's numbers show the mechanism only. | strong | VERIFIED-SOFTENED |
| [[almazaydeh-2026-heuristic-centrality-methods]] | 2026 | suggests-extension | At 1000 nodes PageRank reaches 921.78 against IMM 954.90 (96.5 percent by Table 2 arithmetic; the paper states 96.3 percent in the abstract and discussion) in 0.4 s. Betweenness at 921.80 is indistinguishable (t = -0.01, p = 0.99). Degree reaches about 85 percent. | Usable only as an ablation diagnostic, labelled UNSTRATIFIED, with window, denominator (533 members) and topic named. The paper's hub-favouring importance sampling tilts results toward centrality, and its graphs are undirected uniform-p, so a near-match on spread would not justify replacing greedy. Spread is modelled cascade reach, not access or influence (D-025). | moderate | VERIFIED-SOFTENED |
| [[bharathi-nd-competitive-influence-maximization]] |  | qualifies | Lemma 1 gives monotone submodularity of a player's expected reach in its own seeds, holding the other players' seed sets fixed. Greedy over that function gives a (1-1/e) approximation to the best response (Theorem 1). | The raw-spread guarantee applies only with a fixed opponent set and A-seed additions. Independent per-seed gating is an expectation over independent masks and keeps submodularity. The fac-weighted ranking is a heuristic, not greedy on spread. Contest removal and the S-dependent bystander exclusion fall outside Lemma 1. MC of 20 adds noise. The bound holds only with fac equal to 1, which is the LEV-off case. | moderate | VERIFIED-SOFTENED |
| [[chen-2023-complex-contagion-influence]] | 2023 | qualifies | Complex-contagion IM is non-submodular (Thm 1) and NP-hard (Thm 2). Greedy can be arbitrarily bad: normalised influence 0.0396 for Greedy vs 0.5347 for RL4CCIM on India (202 nodes), and 0.0293 vs 0.5585 on Exhibition. Deterministic setting p0=1, p1=0, budget T=8 (T=12 on Exhibition). | The zero-marginal failure bites only if real cosponsor joining is threshold-like, so that single-seed marginals are zero until K partners are joined. Under the system's per-edge IC kernel the stop rule and knee are internally coherent. The paper's numbers are simulated cascades on non-legislative networks, so they do not transfer as estimates. | moderate | VERIFIED |
| [[manchanda-2020-gcomb-learning-budget-constrained]] | 2020 | qualifies | IM is evaluated with single-colour Independent Cascade, with expected spread averaged over 10,000 Monte Carlo simulations. GCOMB spread is within small differences of IMM (Table 3a, largest magnitude about 3e-3 at YT-TV b=50) and 30 to 160 times faster. | The paper's 1-1/e and IC comparisons assume submodular single-cascade spread. The two-colour race with bystander mobilization need not be submodular, so those guarantees do not transfer. MC 20 has no convergence check by the paper's standard of 10,000 runs. The 30 to 160 times speed figure is versus IMM, not versus this frontier. | weak | VERIFIED |
| [[kempe-2015-maximizing-spread-influence]] | 2015 | qualifies | Greedy on a monotone submodular sigma reaches 1-1/e-eps of optimum (Thm 1.1). Submodularity follows from triggering-set (live-edge) worlds (Lemma 4.4; Thm 4.5 for IC; Thm 4.6 for LT). Each estimate uses 10000 simulations (Sec 8.3). Competing cascades are left open (Conclusions). | With CRN, greedy is exact greedy on the 20-world sample objective, which is submodular only for a single cascade. The two-colour race needs a separate argument whose tie order differs from this BFS (A queued first). fac-weighted, clamped marginals optimize no stated set function. The (1-1/e) bound is context, not a certificate, unless fac = 1 and race submodularity is shown. | strong | VERIFIED-SOFTENED |

**Assessment**

**What it is.** A lazy greedy up to 60 seeds (MAXSEEDS), at MC = 20, on edge-keyed CRN cascades over a two-colour, first-to-reach competitive kernel with money-mobilized bystanders.
- Marginals are max(0, delta) times fac. fac is REPFAC*lev for contest seeds, 1.0 for defend seeds, and lev otherwise.
- The knee is the last seed whose gain is at least a quarter of the peak gain.

**Evidence: 11 rows (2 VERIFIED, 9 VERIFIED-SOFTENED).** The evidence agrees that **no greedy guarantee carries over**:
- Kempe 2015 (VERIFIED-SOFTENED, strong): the (1-1/e) bound needs monotone submodular single-cascade spread. Competing cascades are left open.
- Borodin (VERIFIED-SOFTENED): most competitive extensions lose submodularity. Nothing is shown for the electoral contest and defend objective.
- Bharathi (VERIFIED-SOFTENED): its competitive bound holds only for a fixed opponent set with fac = 1, the LEV-off case. Contest removal and the S-dependent bystander set fall outside Lemma 1.
- Golovin 2011 (VERIFIED-SOFTENED): sampled marginals give only additive error.
- Manchanda 2020 (VERIFIED, weak): its benchmarks use 10,000 MC simulations against this system's 20.

So the frontier is a **non-adaptive heuristic**, and the fac-weighted ranking optimizes no stated set function.

**Noise.**
- Almazaydeh 2026 (VERIFIED-SOFTENED, strong): CELF++ at 50 MC reached 12.9% of IMM at 1,000 nodes, a failure from independent noise.
- The edge-keyed CRN design addresses that mechanism in principle.
- The brain records MEA-7 PASS (100% Jaccard overlap at MC = 100; 98% across two RNG seeds), per the evidence row. That was at MC = 100, while the frontier runs at MC = 20.
- `audit_frontier.py` tolerates up to 35% of marginals inverted as MC noise. That is a loose bar, and it was never separated from structural non-monotonicity.

**Knee and stop rule.** Chen 2023 (VERIFIED): under complex contagion, greedy can be arbitrarily bad. On the India graph (202 nodes) greedy reaches 0.0396 normalized influence against 0.5347 for RL4CCIM. Zero-marginal stops and knees that presume diminishing returns would mislead if real joining is threshold-like. Under the system's per-edge IC kernel they are internally coherent.

**Cost.**
- Trivedi 2020 (VERIFIED-SOFTENED, supports) and Kumar 2022: exact greedy stays affordable because the candidate pool is about 45. The node-count thresholds in those papers are not what makes it affordable.
- Almazaydeh's PageRank, at 96.5% of IMM, is usable only as an UNSTRATIFIED ablation, not as a replacement.

Spread is **modeled cascade reach**. It is not access, closeness or influence (D-025).

**Contradictions**
- The code and system framing cite Golovin-Krause as backing for greedy seed selection. Kempe 2015, Borodin, Bharathi and Golovin's own scope rows say the guarantee does not hold for a sampled, fac-weighted, clamped, competitive objective. The zero clamp itself signals that sampled spread is not monotone.
- The MEA-7 stability PASS (MC = 100) is cited where the frontier runs at MC = 20. The evidence row does not show stability at the operating MC.
- The assumption 'diminishing marginal returns to seeds' is presumed by both the knee and the stop rule. Chen 2023 (VERIFIED) shows it can fail badly under threshold-like adoption. Whether CU cosponsor joining is threshold-like is untested.

**Gaps**
- The empirical question is untested: where does the realized marginal return to additional cosponsors flatten for CU bills? One study: a per-bill curve of cosponsor joining over time against the number of early recruited sponsors, on CU bills across recent Congresses, with window and denominator named. LDA serves only to identify which bills were lobbied (D-021), not to weight edges.
- There is no read-only check of raw marginals at MC = 20 against MC = 100 or higher under CRN that would separate noise from structural non-monotonicity. The 35% inversion tolerance is unexamined.
- No same-CRN ablation of greedy against degree, PageRank or Katz-Bonacich seed sets on the 533-member kernel exists. The evidence row could not confirm the absence by a repo-wide search.
- Paul's orienteering (political-capital budget) formulation is not built, and no row speaks to it.

**Paul's position vs the evidence**

Paul says GCOMB's budget framing maps onto a political-capital budget in an orienteering formulation.

**No evidence row bears on the orienteering mapping, and it is not built.** The GCOMB rows (Manchanda 2020, VERIFIED and VERIFIED-SOFTENED) qualify rather than support it. GCOMB assumes single-colour submodular IC, benchmarks against 10,000 MC runs, and its speed gain is measured against IMM on large graphs. None of that holds for a 45-candidate, two-colour, MC = 20 frontier.

The position stays a design idea with no evidential base yet.

**Open questions**
- Should the frontier's documentation drop any implied (1-1/e) guarantee and call it a heuristic? This is a docs and claims change.
- Is an MC = 20 vs MC = 100+ stability check at the frontier's operating point authorized? It is read-only.
- Does Paul still want the orienteering formulation, given that GCOMB's assumptions do not hold here?

**Read first:** [[kempe-2015-maximizing-spread-influence]], [[borodin-nd-threshold-models-competitive]], [[bharathi-nd-competitive-influence-maximization]], [[almazaydeh-2026-heuristic-centrality-methods]], [[chen-2023-complex-contagion-influence]], [[manchanda-2020-gcomb-learning-budget-constrained]]

## Workbook allocator (top-133 from fixed pot)

**Computes.** Funds the top 133 of 533 members from a flat $2.0M in proportion to score (uncapped; max about $25.8k).

**Method.** A capped variant ($10k cap, top 250) exists only in a backup and was not applied.

**Code refs.** `Blended Model col U`, `fec/apply_alloc_cap.py (not applied)`

**Assumptions.**
- Score ordering maps to funding priority

**Evidence table**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[cagala-2021-optimal-targeting-fundraising]] | 2021 | qualifies | Treating everyone as a gift recipient is net-negative or near zero: all-gift minus no-gift is +0.05 to +0.08 euro for the warm list (not significant) and -0.97 euro per donor for the cold list (significant) (Table 1). Targeted warm-list donors differ from non-targeted donors only modestly in prior giving (8-year total 130.98 vs 123.39 euro; 787 targeted vs 1,567 non-targeted; Table 4, Panel A). The paper cautions that observed characteristics are correlates of response, not the channels through which the gift works (Section 5.4). | Conceptual only. The magnitudes come from donors, not legislators. The transferable point is to compare any proportional allocator against uniform and random benchmarks. The paper does not show that the allocator is wrong, and the system side is unverified. | weak | VERIFIED-SOFTENED |

**Assessment**

**What it is.** The workbook allocator reportedly funds the top 133 of 533 members from a flat $2.0M in proportion to score: uncapped, with a maximum of about $25.8k.

**Evidence: 1 row (1 VERIFIED-SOFTENED).**

Cagala 2021 shows that blanket treatment can be net-negative. On their cold list, all-gift minus no-gift was -0.97 euro per donor, significant. Targeted units differed only modestly on observables. The transferable point is methodological: **benchmark any proportional allocator against uniform and random allocations.** The donor magnitudes do not transfer.

**The system side did not reproduce.**
- The Blended Model sheet's Allocation column (column U) has no cached values in the copy read.
- The cited producer, `fec/apply_alloc_cap.py`, does not exist in the repo.
- The capped variant ($10k cap, top 250) exists only in a backup and was not applied.

The component's own figures (133, $2.0M, about $25.8k maximum) are therefore **unverified**.

This is effectively a GAP on two fronts: there is no evidence on proportional-to-score allocation, and the artifact itself is unconfirmed.

One model-internal point needs no new paper. The proportional rule ignores PMAX ($10k), which the joint optimizer treats as the CU PAC per-candidate per-cycle limit (the F2 / 2026-09-30 ruling). A $25.8k maximum allocation would exceed that limit if both describe one PAC's money.

**Contradictions**
- The workbook allocator is uncapped (maximum about $25.8k per member). The joint optimizer caps at PMAX = $10k, read as the CU PAC's per-candidate per-cycle limit. If both allocate the same PAC's money, the workbook figure breaches the limit the optimizer enforces. This is an internal inconsistency, not a literature contradiction.
- The component's description (top 133, $2.0M) is not reproducible from the artifact: column U is empty, and `apply_alloc_cap.py` is missing.

**Gaps**
- No study compares proportional-to-score allocation with greedy, uniform or random allocation. Model-internal fill: on the same objective and stance state, compute predicted winnability for proportional vs greedy (`optimize.py`) vs uniform vs random. Real-world fill: the same allocations scored against next-cycle CU-aligned outcomes, as in the joint-optimizer backtest.
- Provenance gap: the producer of column U must be located, or the allocator documented as retired, before any claim about it is made.
- Whether the workbook allocator applies the stance money gate is unconfirmed.
- Huneeus & Kim 2018 (Paul's citation) is not in the evidence corpus.

**Paul's position vs the evidence**

Paul sees Huneeus & Kim as the basis for setting fundraising goals. **That paper is not in the evidence corpus**, and no annotation addresses proportional allocation.

The position is about the size of the pot, not how it is split, so it neither supports nor contradicts the proportional rule. The only row (Cagala 2021) argues for benchmarking the split, not for proportionality.

**Open questions**
- Is the workbook allocator still live? Where is its producer, given that `fec/apply_alloc_cap.py` does not exist and column U is empty?
- Should the workbook allocation respect PMAX = $10k, to match the F2 single-PAC ruling?
- Should Huneeus & Kim 2018 be ingested so the fundraising-goal position can be checked?

**Read first:** [[cagala-2021-optimal-targeting-fundraising]], [[kempe-2015-maximizing-spread-influence]], [[golovin-2011-adaptive-submodularity-theory]]

## Break-even CU spend and ROI floor

**Computes.** The CU dollars that restore baseline flip odds against competitor spend (closed form, 'cannot offset' when q >= 1), and member ROI floor values.

**Method.** Solves a(CU)/(a(CU) + b(comp)) = pfbase with the saturating money term; ROI floor parity is checked by roi_verify.py.

**Code refs.** `fec/templates/make_dashboard_html_00.tpl (break-even)`, `verify_finish/roi_verify.py`

**Assumptions.**
- Competitor and CU money enter a share-type ratio

**Evidence table**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | qualifies | Kang's model implies an average policy value of more than $500m, and the Farm Bill comparison gives a naive return of 600,000% ($22bn in 2000 subsidies vs under $3.7m in agribusiness PAC contributions that cycle). The review says excessive returns are common in this literature. Adding an electoral-vote channel (Bombardini and Trebbi 2011: $200-$400 per vote) lowers the Farm Bill figure to $13.7 per dollar, which the authors call still high but more reasonable; B&T's simulations give average returns of 10%-20% per year. The conclusion notes some structural models display symptoms of misspecification. | This is a caution about any ROI read off contribution-to-outcome ratios. It applies whenever dashboard ROI floors are presented as returns. It does not invalidate the break-even algebra, which is internal to the model. Any ROI shown should carry an UNVERIFIED or model-output label. | moderate | VERIFIED-SOFTENED |
| [[bickmore-2025-corporate-lobbying-political]] | 2025 | qualifies | Lobby on ShareAssets 0.0743 with full controls; PAC on ShareAssets 0.1100 (election years, N = 62,198). Abramoff DiD Treatment x Post on ShareAssets with full controls is -0.0265 (SE 0.0096); on ShareMcap it is -0.0336 (SE 0.0099). Post window 2006Q1-2009Q1; pre-period 2003Q1-2005Q4 per the Table 8 description. | The paper's return is a firm-level industry-share outcome from Compustat matched to CRP by company name, 1998-2016. It does not measure votes, cosponsorship or flips, so it cannot calibrate WM, HALFF or LOSSFRAME. It supports only a framing claim that political spending associates with firm share gains, reported in share points with the unit stated. | moderate | VERIFIED-SOFTENED |
| [[alexander-2009-lobbying-analysis-under]] | 2009 | qualifies | Overall return on lobbying 220 = (46,157.5 + 15,897.0) / 282.7, i.e. about 62.05bn estimated savings on 282.7M lobbying across 93 lobbying repatriators (FY2004-2006). Coalition 257, non-coalition 154, per-company mean 869, per-company median 172.2, lobbying over 1M subsample (48 firms) 243. | No numeric transfer. The paper numerator is a firm tax benefit from a one-time provision and the denominator is LDA spend (upper bound). Melange numerator is modeled member movement and denominator is FEC PAC money. Any quote needs the window FY2004-2006, the 93-firm denominator, and the median 172 next to the mean 869. The bearing describes the PAC side as capped at 10k; the code uses 10000 as a marginal increment, so describe it as a per-10k increment. | moderate | VERIFIED-SOFTENED |
| [[cai-2025-competitive-influence-maximization]] | 2025 | qualifies | Two settings are mixed here. The optimal start for Congress votes at T=16 falls from 14.730 at budget ratio 0.1 to 10.476 at ratio 10 (Table 2, constant-opponent setting, fixed opponent starting at 0). In the two-controller Nash game the controller with the larger budget moves first, and the second mover can only buffer the first mover's gain, never exceed its vote share (Sec 4.3.4, stage 3, p.21-22). Fig 9b shows per-node resource per unit time rising with the budget advantage. | The paper's statement that a disadvantaged controller can offset but not overtake agrees in spirit with the cannot-offset message. The claim that a disadvantaged side does better by delaying has no counterpart in a static break-even, and it rests on the paper's abstract and simulations only. Transfer would need a model of within-cycle spending order, which the system lacks. | weak | VERIFIED-SOFTENED |
| [[goff-nd-cross-industry-finance]] |  | qualifies | Crypto outspends key allies by factors of 100, so credit unions would have no chance in a monetary fight with the industry; the supporting graph is a missing placeholder. | The factor of 100 has no source, window or denominator, so the system cannot size the disadvantage. The bounded term shows only that the form caps the monetary pull, not that money decides outcomes. | weak | VERIFIED-SOFTENED |

**Assessment**

**What it is.** A closed-form solve for the CU dollars that restore baseline flip odds against competitor spend, using a(CU)/(a(CU) + b(comp)) = pfbase with a saturating money term. When q is at least 1, the display says CU money alone cannot fully offset the competitor spend. It also produces member ROI floor values.

**Evidence: 5 rows (5 VERIFIED-SOFTENED).** None tests the functional form.

- **Bombardini 2020** (moderate): naive lobbying-return figures are implausibly high across the literature.
  - The Farm Bill comparison gives about 600,000% ($22bn in 2000 subsidies against under $3.7m in agribusiness PAC money that cycle).
  - Adding an electoral-vote channel brings it to $13.7 per dollar. Bombardini-Trebbi simulations give 10%-20% per year.
  - Some structural models show signs of misspecification.
  - Any ROI floor shown as a return should carry a model-output label.
- **Alexander 2009** (moderate): an overall return of 220 ($62.05bn estimated tax savings on $282.7M of LDA lobbying, 93 repatriating firms, FY2004-2006). The per-company median is 172 against a mean of 869. Its numerator, a one-time tax provision, and its denominator, LDA spend, do not map onto Melange's modeled member movement per FEC PAC dollar.
- **Bickmore 2025** (moderate): a firm industry-share outcome (Compustat, 1998-2016, N = 62,198 election-year obs).
  - PAC on ShareAssets is 0.110. In the Abramoff DiD, Treatment x Post is -0.0265 (SE 0.0096).
  - It does not measure votes or flips, so it cannot calibrate WM, HALFF or LOSSFRAME.
- **Cai 2025** (weak): in a two-controller game, a disadvantaged side can buffer but not overtake. That loosely agrees with the cannot-offset message. Its timing result (delay when disadvantaged) has no counterpart in a static break-even.
- **goff-nd-cross-industry-finance** (Paul's own note, weak): "crypto outspends allies by about 100x" has no source, window or denominator, and its supporting graph is a placeholder.

**System-side caveats.**
- `roi_verify.py` was not found in the repo, so ROI floor parity is unconfirmed.
- `desk_roi` returns expected weighted flips per $10k increment. That is not a dollar return.

The break-even algebra is internally consistent. The share-type ratio assumption is **untested**.

**Contradictions**
- Paul calls the Alexander 2009 figure an outlier. Bombardini 2020 (VERIFIED-SOFTENED) says excessive returns are common across this literature and attributes some of them to model misspecification. On that reading, Alexander's 220 is typical of a class of naive return estimates, not an isolated outlier.
- The component says ROI floor parity is checked by `roi_verify.py`, but no such file was found in the repo (Bombardini row, system side).
- The 'about 100x outspending' premise behind the cannot-offset framing has no source, window or denominator (Goff note), so the system cannot size the disadvantage it reports.

**Gaps**
- The empirical question is untested: do member positions respond to the ratio of CU money to opposing-industry money rather than its level? One study: a member fixed-effects panel of position change on CU bills, regressed on both CU/(CU + opposing) and on CU and opposing levels separately. Stratify by party and chamber, name the Congresses and the industry set, and compare model fits. Given the Ansolabehere 2002 member-FE null on conversion, expect small effects and preregister the comparison.
- No row calibrates WM, HALFF or the saturation constant against outcomes. All money-return rows are firm-level.
- No source exists for the competitor-spend multiple. A dated FEC tabulation of CU vs crypto/fintech PAC receipts by cycle, with denominators, would fill it.
- Kim J. et al. 2025 and Huneeus & Kim 2018 (Paul's citations) are not in the corpus.

**Paul's position vs the evidence**

**Paul's ROI framing is qualified, not supported.**
- Every return figure in the corpus is a firm-level or policy-value ratio with a different numerator and denominator from Melange's.
- The review literature warns that such ratios run implausibly high.
- His wish to include competing-industry conflict is consistent with Cai 2025's competitive framing, but no row validates the share-type ratio used to do it.
- His view of Alexander as an outlier is contradicted in kind by Bombardini 2020.

Recommended labelling: break-even and ROI floor values are model outputs, never returns. Quote the window and denominator of any external figure: Alexander FY2004-2006 across 93 firms, with median 172 next to mean 869.

**Open questions**
- Where is `roi_verify.py`, or should the ROI-floor parity claim be retired?
- Should dashboard ROI floors be relabelled as model output (expected weighted flips per $10k), given Bombardini's caution?
- Can Paul source the about-100x crypto-vs-allies spending figure, with window and denominator?

**Read first:** [[bombardini-2020-empirical-models-lobbying]], [[alexander-2009-lobbying-analysis-under]], [[bickmore-2025-corporate-lobbying-political]], [[cai-2025-competitive-influence-maximization]], [[goff-nd-cross-industry-finance]]

## Across components

## Patterns across components

**1. Guarantees are stated for a cleaner problem than the one built.** The separable joint optimizer in a fixed state is the one place where a guarantee transfers: exact grid greedy, backed by Golovin 2011 and Kempe 2015 Thm 7.1, both VERIFIED. The frontier and the coalition allocator run on a two-colour competitive kernel with:
- fac-weighted, zero-clamped marginals;
- MC = 20;
- a bystander set that depends on the seed set.

Kempe 2015, Borodin, Bharathi and Golovin all agree that no (1-1/e) bound covers that. Where the code or framing cites Golovin-Krause or calls these allocators submodular, it borrows a guarantee that only the joint optimizer has earned. The code's Lemma 5.3/A.9 citation also does not match the text, which has Lemma 6, Eq. 13.

**2. Nothing is scored against reality.**
- `observations = []` is never appended to.
- No row compares any allocation with realized CU-aligned outcomes.
- The only legislator-response evidence (Ansolabehere 2002, member FE, 1978-1994, UNSTRATIFIED) finds donations do not change roll-call voting.

That null **supports** the stance gate. It **undercuts** any reading of allocator gains or ROI floors as real effects. All allocator outputs should carry model-output labels until a preregistered backtest exists (DE-062 discipline).

**3. Learned-policy enthusiasm is not supported here.** GCOMB, S2V-DQN, RELINK and Q-learning win on speed at large scale or on non-separable graph problems, and they need many training instances. Melange is small (533 members, a candidate pool of about 45), capped, and mostly separable, and per-cycle outcomes are sparse. Sutton 2018's multi-cycle (LTV) idea would require money to change later responsiveness. That is a reciprocity-style mechanism, and reciprocity is NOT validated in this project (D-104..D-108). Any multi-cycle objective is blocked behind that, plus owner authorization.

**4. Internal inconsistencies to resolve before any claim.**
- The workbook allocator is uncapped (max about $25.8k), while the optimizer enforces PMAX = $10k under the single-PAC ruling.
- The workbook producer (`apply_alloc_cap.py`) and `roi_verify.py` were not found.
- The stance gate's coverage of the workbook path is unconfirmed.
- The gate fails open for members with no matrix row.

**5. Wording.**
- Spread, frontier reach and joint-vs-naive figures are modeled cascade reach, not access, closeness or influence (D-025).
- Any centrality ablation (Almazaydeh's PageRank at 96.5% of IMM) is an UNSTRATIFIED diagnostic only.
- LDA may identify lobbied bills in any backtest but cannot weight edges (D-021).

**6. Flags on rejected SNA proposals.** No paper in this hub argues for dropping regular cosponsorship edges or for per-edge volume normalization. Zareie 2023's topic-aware kernel is a different proposal. It has no accuracy evidence and would be a change-controlled engine change, so it is listed as conjectural only.

**7. Corpus gaps against Paul's own citations.** Huneeus & Kim 2018, Furnas et al. 2022, Leskovec 2007 (CELF), Kim J. et al. 2025 and Kempe et al. 2003 underpin Paul's positions but have no evidence rows. Ingesting them is the cheapest way to test the positions on their own terms.

## Research agenda

1. Ingest and annotate Paul's cited-but-absent sources: Huneeus & Kim 2018, Furnas et al. 2022, Leskovec et al. 2007 (CELF), Kim J. et al. 2025, Kempe et al. 2003. Then re-run the position checks for the joint optimizer, stance gate, workbook allocator and ROI.
2. Preregistered observational backtest: score the joint optimizer's allocation and uniform, random and proportional benchmarks against realized CU-aligned cosponsorship and votes in the next cycle. Use matched unfunded members, net of cost, with window and denominator named, stratified by party and chamber. Results stay UNVERIFIED until reproduced by a second path (DE-062).
3. Member fixed-effects panel testing the share-type money ratio against levels (CU vs opposing-industry PAC money) on CU-bill positions in recent Congresses, stratified by party and chamber. This updates the Ansolabehere 2002 design and directly tests the break-even assumption and the stance-gate rationale.
4. Read-only frontier noise check: raw marginals at MC = 20 against MC = 100 and 200 under the same CRN keys. Report inversion rates against the 35% audit tolerance, and confirm MEA-7 stability at the operating MC.
5. Read-only coalition-allocator ablation: joint spread of the water-fill seed set against lazy greedy, and against degree, PageRank and Katz-Bonacich sets, at equal seed count and the same CRN, per topic. Label UNSTRATIFIED and report modeled reach only (D-025).
6. Observed sub-additivity and threshold test on CU bills: do multiple early sponsors in a member's neighbourhood produce less (IC-like) or more (complex-contagion) cosponsor joining than the sum of single-sponsor effects? This decides whether the knee and zero-marginal stop rule are trustworthy. LDA is used for bill identity only (D-021).
7. Model-internal comparison of proportional-to-score (workbook) against greedy (`optimize.py`) on the same objective and stance state, after the column-U producer is located or the allocator is declared retired. Reconcile the uncapped $25.8k maximum with PMAX = $10k.
8. Provenance and code audit, read-only: locate `roi_verify.py` and `fec/apply_alloc_cap.py`. Count members passed by the fail-open stance gate and the money they received. Confirm the gate's coverage of the workbook path. Correct the Golovin-Krause lemma citation (owner change).
9. Bridge-boost shape check: compare the monotone (1 + 0.8*bpf) boost with Harbridge-Yong 2023's nonlinear, reversing-past-50/50 pattern. Any change to BETA_C or its shape is Paul's ruling.
10. Source the competitor-spend multiple (CU vs crypto/fintech PAC receipts by cycle, from FEC, with denominators) to replace the unsourced about-100x figure in the cannot-offset framing.
