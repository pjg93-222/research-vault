---
type: system
name: Melange budget allocation
slug: melange-allocation
components: [melange-allocation.joint-optimizer, melange-allocation.stance-money-gate, melange-allocation.per-topic-allocators, melange-allocation.budget-frontier, melange-allocation.workbook-allocator, melange-allocation.break-even-and-roi]
brain_notes: ["actors/quant-model-evaluation-skill", "actors/melange-dashboard-june-capstone", "concepts/influence-vs-alignment-axes", "actors/moneyball-workbook-rules-scoring-and-sublayer-surgery"]
paper_count: 27
tags: [system]
---

# Melange budget allocation

Spending decisions made on top of the influence engine: the joint CU pot optimizer, per-topic flip and coalition allocators, the budget frontier (including electoral contest and defend modes), the workbook top-N allocator, the stance money gate, break-even CU spend and the ROI floor.

**Brain notes:** [actors/quant-model-evaluation-skill](obsidian://open?vault=brain&file=actors/quant-model-evaluation-skill); [actors/melange-dashboard-june-capstone](obsidian://open?vault=brain&file=actors/melange-dashboard-june-capstone); [concepts/influence-vs-alignment-axes](obsidian://open?vault=brain&file=concepts/influence-vs-alignment-axes); [actors/moneyball-workbook-rules-scoring-and-sublayer-surgery](obsidian://open?vault=brain&file=actors/moneyball-workbook-rules-scoring-and-sublayer-surgery)

**Melange budget allocation: evidence assessment (2026-10-10)**

The evidence file has 46 bearings across six components: Joint CU budget optimizer 17, Stance money gate (F1) 3, Per-topic flip and coalition allocators 9, Budget frontier with knee and electoral modes 11, Workbook allocator (top-133 from fixed pot) 1, Break-even CU spend and ROI floor 5. Status split: 2 UNVERIFIED, 11 VERIFIED, 33 VERIFIED-SOFTENED. The 2 UNVERIFIED are Zeng (drones), which is irrelevant, and Madsen (micro-targeting), which is an analogy only. A short online scan filled the stance-gate and ROI gaps. Those hits are listed separately as **online leads**. Only abstracts and secondary summaries were read, so they count as leads, not findings (DE-062).

**What the evidence says.** The *method* layer is well supported where the code is careful and unsupported where it is not.

- **Joint optimizer (`optimize.py`).** Lazy greedy over separable, concave chunks. It checks concavity and withholds its certificate once a modeled conversion fires. That is exactly the scope the theory allows (Golovin & Krause; Kempe Thm 7.1).
- **Budget frontier.** It inherits *no* guarantee. Five independent sources agree: Golovin, Kempe 2015, Borodin, Bharathi and Manchanda. The reasons are a two-colour competitive race, fac-weighted clamped marginals, an opponent set that changes as seeds are added, and MC=20 sampling. The frontier is a heuristic and has to be labelled as one.
- **Coalition allocator.** A proportional water-fill on singleton marginals is *not* greedy, so no submodular bound applies to it. On hep-th, Kempe shows singleton or centrality ranking losing to greedy by roughly 18-40%.
- **Missing layer.** No bearing validates any allocator against realized outcomes. The `observations` list is empty. Every gain, spread or flip figure is a model output, not influence, access or closeness (D-025).

**Contradictions with Paul's stated positions**

- **GCOMB replacing greedy at scale.** GCOMB's advantage is wall-clock speed on graphs with 1M+ nodes. It shows no quality gain over greedy and names capacity-constrained problems as future work (Manchanda 2020). The joint optimizer is a capacity-capped allocation over about 533 members, where greedy is already exact.
- **Submodular tier label.** Paul places the per-topic allocators in the static/submodular tier. The coalition allocator does not use greedy and its competitive kernel is not shown to be submodular.
- **ROI.** Paul's scepticism about the Alexander 220x figure is confirmed. Bombardini 2020 says excessive returns are common in this literature, and adding an electoral-vote channel cuts the naive Farm Bill figure to $13.7 per dollar.

**Actual errors found**

1. **Lemma citation.** The Golovin-Krause reference in the code (JAIR Lemma 5.3 / A.9) does not match the text copy, where the bound is Lemma 6 Eq. 13 and the cost bound is Lemma 37.
2. **Missing producer for the workbook column.** `fec/apply_alloc_cap.py`, cited as the producer of the workbook Allocation column, does not exist. The column holds no cached values, so the top-133 of 533 from a $2.0M pot claim was not reproduced.
3. **Missing ROI checker.** `roi_verify.py`, said to check the ROI floors, was not found in the repo.
4. **Lenient frontier audit.** `audit_frontier.py` passes up to 35% marginal inversions as MC noise, wide enough to hide genuine non-submodularity.
5. **Unsourced crypto figure.** The crypto outspends allies 100x figure in Goff n.d. has no source, window or denominator, and its graph is a missing placeholder.
6. **Citations with no bearing.** Huneeus & Kim 2018 is cited for three components, and Kim J. et al. 2025 for break-even. Neither has a bearing in the evidence file.

## Joint CU budget optimizer

**What it computes.** Allocation of a $2.5M pot in $500 steps maximizing total winnability, with a per-candidate cap PMAX $10k.

**Method.** CELF lazy greedy on per-member concave chunks. Modeled conversions rebuild the queue. Exact only in a fixed stance state. Golovin-Krause bound as a diagnostic. Knee at 20% of peak marginal.

**Code refs.** `fec/engine/optimize.py:1-177`

**Assumptions**
- The objective is separable and concave in CU dollars (checked numerically)
- The pot is one PAC's budget (F2 ruling)

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[pritoni-2025-lobbying-coalitions-determinants]] | 2025 | qualifies | The literature finds both positive (Junk 2019; Nelson and Yackee 2012) and negative (Bunea 2013; Haider-Markel 2006; Heinz et al. 1993; Junk 2020a) links between forming a coalition and policy influence, and calls the results contradictory. The review leaves coalitions as an explanatory variable for influence to future research. | The weapon-of-the-weak evidence might tempt someone to pool allied industries' dollars into the CU money leg. The review does not support that, because the effect of coalitions on outcomes is unresolved. The single-actor pot remains defensible on this evidence. | weak | VERIFIED |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | method-source | Lazy greedy (Algorithm 2) loses no solution quality because stale marginals upper-bound current ones. Speedups were about 3.5-7x fewer evaluations on 46 sensors (temperature) and 30-38x on 357 sensors (traffic) at 50 percent failure. The Lemma 6 data-dependent bound was tighter than the worst-case bound in the sensor runs. | Carries over in the deterministic fixed-state case where per-member chunk marginals are non-increasing, which the code's concavity check tests. It does not carry over once modeled conversions fire; the code then withholds the certificate. The code citation Golovin-Krause JAIR 11 Lemma 5.3 / A.9 does not match this text copy, which has Lemma 6 Eq.13 and Lemma 37 for costs. | strong | VERIFIED |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | Theorem 5's (1-1/e) bound compares with the best adaptive policy and needs observed outcomes, an adaptive monotone submodular objective and a known prior (Def 2-3 p.8; Thm 5 p.11). Treasure-hunting instances with strong synergies fall outside the class (Sec 3.4 p.9). Theorem 26 shows no O(\|E\|^(1-eps/beta)) approximation for a constructed treasure-hunting family of pointwise-modular instances unless PH = Sigma_2^P (Sec 12 p.33). | The paper supports the refusal to claim a certificate in conversion mode, since a threshold jump can make a later chunk's marginal rise. Softened scope: Theorem 26 is a hardness result for one constructed family, not for every objective lacking adaptive submodularity. Sec 12 frames it more broadly, but the proof covers only that family. | strong | VERIFIED-SOFTENED |
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

**Assessment**

**Overall: method well supported within its stated scope. Real-world performance is unvalidated.** 17 bearings: 1 UNVERIFIED, 5 VERIFIED, 11 VERIFIED-SOFTENED.

**Supports the build**
- **Golovin & Krause 2011 (VERIFIED, method-source, strong).** Lazy greedy loses no quality, because a stale marginal is always an upper bound on the current one. The data-dependent bound is tighter than the worst case. This matches `optimize.py`: a heap of $500 chunks under a $10k PMAX, with `dd_bound` computed only under the fixed-state certificate.
- **Kempe 2015, Thm 7.1 (VERIFIED, supports).** Budgeted hill-climbing on a monotone function with diminishing returns has a bound. It holds only in the fixed stance state, and the code says so.
- **Golovin, Thm 5 / Sec 3.4 / Thm 26.** These support the code's refusal to claim a certificate in `modeled_threshold_replanning` mode. A threshold jump can raise a later chunk's marginal, so the conditions for the bound fail.

**Qualifies it**
- **Moreno 2021.** Industry money is frozen (`ind = fm.ind0`, never rewritten). The plan is a best response to a passive opponent. With two active controllers, the paper's numerical Nash result is uniform targeting, so the plan could be exploited if industry re-optimises.
- **Sutton 2018 / Watkins 1992.** The optimum is exact for the model, not for the world. The optimizer has no exploration and no outcome feedback, because `observations=[]` is never appended to.
- **Cagala 2021.** The transferable part is the evaluation design: net-of-cost gains against benchmark allocations on held-out units. The donor effect sizes do not transfer.
- **Borodin.** The `a/(a+b)` flip form looks like a competitive LT model. After a conversion, `mAt = max(nA)` renormalises, and that can lower other members' terms. Structural analogy only; whether total winnability drops is not measured.
- **Pritoni 2025.** The literature on whether coalitions affect influence is contradictory. Treating CU as a single-actor pot is defensible, and pooling allied industries' dollars is not supported.
- **Almazaydeh 2026.** Monte Carlo noise is irrelevant here because this objective is deterministic. It would matter only if the objective were swapped for a simulated cascade.

**Contradicts Paul's GCOMB expectation:** Manchanda 2020, Arya 2025 (RELINK) and Dai 2017 (S2V-DQN). Their gains are speed on very large graphs or quality on non-separable set-selection problems. None applies to separable concave allocation over about 533 members with per-member caps.

**Ignore:** Zeng (UNVERIFIED, drones).

**Actual error:** the Golovin-Krause lemma cited in the code (5.3 / A.9) does not match the text, which has Lemma 6 Eq. 13 and Lemma 37 for costs.

**Contradictions**
- Paul expects GCOMB to replace greedy at scale. Manchanda 2020 reports only speed gains, 2-4x over CELF and about 10x over greedy, on graphs with 1M+ nodes. It reports no quality gain and names capacity-constrained problems as future work. The optimizer is a capacity-capped, separable allocation over about 533 members, where greedy is already the exact discrete optimum in a fixed state.
- Paul's citations treat Golovin & Krause as the guarantee source. The code's lemma reference (JAIR Lemma 5.3 / A.9) does not match the text copy (Lemma 6 Eq. 13; Lemma 37 for costs). The citation itself is wrong.
- Moreno 2021: with two active controllers the equilibrium is uniform targeting. The optimizer freezes industry money (`ind0`), so its optimal plan holds only against a passive opponent.

**Gaps**
- No outcome validation. Nothing compares the optimizer's allocation with uniform, random, no-spend or actual CU PAC allocations on realized outcomes. The study that would fill this is a retrospective benchmark-relative backtest over past cycles in the Cagala design, scored on member-level cosponsor joins or votes. Members are the denominator, and the window must be named.
- Huneeus & Kim 2018 is cited for this component, but the evidence file has no bearing from it.
- Whether the post-conversion `mAt` renormalisation lowers total winnability is unmeasured. It needs a read-only before/after diff on a conversion-reachable run.
- There is no adversarial or re-optimising opponent model. A best-response check against an industry allocator would show how exploitable the plan is.

**Paul's position vs the evidence**

Paul's first claim is that adaptive greedy plus CELF is the operational layer. The evidence supports this, with the qualification that the build is a fixed-state lazy greedy, not adaptive. The adaptive label overstates it, because nothing is observed. His second claim is that a learned policy (GCOMB) will replace greedy at scale. The evidence does not support this at Melange's scale or for this problem shape. Every learned-policy paper in the set (GCOMB, RELINK, S2V-DQN) wins on speed on very large graphs or on non-separable objectives, and none on capped separable allocation. A learned policy would also need many training instances, which the system does not have.

**Open questions**
- Should industry money stay frozen (passive opponent), or does Paul want a best-response or Nash check run as a diagnostic?
- Does Paul want the GCOMB expectation formally retired for the joint optimizer and kept only for any future large-graph seed selection?
- Is a multi-cycle objective, in which repeated giving changes later responsiveness, in scope? It is untested, and it needs owner authorization.

**Read first:** [[golovin-2011-adaptive-submodularity-theory]], [[kempe-2015-maximizing-spread-influence]], [[manchanda-2020-gcomb-learning-budget-constrained]], [[moreno-2021-shadowing-shielding-effective]], [[cagala-2021-optimal-targeting-fundraising]], [[sutton-2018-reinforcement-learning-introduction]]

## Stance money gate (F1)

**What it computes.** Members with overall tier Skeptic, Highly Skeptical or Opponent get zero money in every allocator.

**Method.** Applied before the top-45 shortlist. Fails open for members with no matrix row. Before the gate 10 opposed members were funded $54,500; after it, 19 were gated and none funded.

**Code refs.** `fec/moneyball_core.py:713-729`, `fec/engine/optimize.py:44-52`

**Assumptions**
- Overall stance should gate money; one topic cell should not

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | supports | Hall and Deardorff (2006) treat information as a subsidy to resource-strapped legislators, which explains why lobbyists lobby friendly legislators and questions the persuasion role. Hirsch et al. (2019; FARA contacts, 219 foreign-interest lobbyists, 110th-111th Congress) find any lobbying contact is more likely for lobbyists ideologically aligned with the politician. | The contact evidence concerns foreign-principal lobbyists under FARA, not domestic PAC dollars. It is consistent with funding allies but does not test whether contributions to opposed members change their positions. | weak | VERIFIED |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | supports | Text PDF p.18 (printed p.17): of 49,418 individuals with 25+ federal contributions (2004-2012), only 167 (0.3 percent) have money-weighted SD above the 0.577 random-giving baseline. Mean money-weighted SD is 0.20 for the sample. Descriptive evidence that active individual donors give on ideology. | Descriptive only: donors are ideological, but the paper does not test whether funding opponents converts them. Window is individual donors 2004-2012; PAC behavior differs (Table 2). | weak | VERIFIED-SOFTENED |
| [[madsen-2017-strategic-advantages-micro]] | 2017 | supports | Contact spent on audiences with an unfavourable view of the source is wasted or counterproductive. The targeted campaign's advantage comes entirely from never contacting those voters. | An analogy only. Campaign contacts with voters are not PAC dollars to legislators, and the paper has no money mechanism. It agrees with Paul's Furnas reading ('cultivate champions') but adds no new evidence about legislator behaviour. | weak | UNVERIFIED |

**Assessment**

**Overall: thin but consistent support, with the causal question untested.** 3 bearings: 1 UNVERIFIED, 1 VERIFIED, 1 VERIFIED-SOFTENED.

**From the evidence file**
- **Bombardini 2020** summarises Hall & Deardorff's legislative-subsidy theory and Hirsch et al.'s FARA contact data. Lobbyists contact ideologically aligned members. This is consistent with funding allies, but it concerns foreign-principal lobbyists, not domestic PAC dollars.
- **Bonica 2013** shows that individual donors give on ideology. In 2004-2012, only 167 of 49,418 donors with 25+ federal contributions (0.3%) exceed the random-giving baseline for dispersion. Descriptive only, and PACs behave differently.
- **Madsen 2017** is a voter-contact analogy and adds nothing on legislator behaviour.

**Online leads (abstracts and summaries only, not findings)**

In favour of the gate:
- **Hall & Wayman 1990 (APSR).** Money buys committee participation by allies, not floor votes.
- **Ansolabehere, de Figueiredo & Snyder 2003 (JEP).** Contributions have small effects on roll calls next to party and district.

Cutting the other way:
- **Stratmann 2002 (J. Law & Econ.).** Money shifted House votes on financial-services legislation, holding positions fixed across two near-identical Glass-Steagall repeal bills (1992, 1998). This is the closest domain match to credit unions.
- **Grier, Grier & Mkrtchian 2023 (APSR).** With member fixed effects, rising sugar money raised the probability of voting against reform.

These two suggest money can move marginal members on low-salience issues. That does not contradict the gate, which blocks only Skeptic and worse and leaves neutrals eligible. It does mean the case for the gate rests on the claim that opponents specifically cannot be moved, and nothing in the set tests that.

**Design risk (system side):** the gate fails open. A member with no matrix row gets tier '-' and stays eligible.

**Contradictions**
- Online leads, not verified: Stratmann 2002 and Grier et al. 2023 report within-member vote shifts from contributions on low-salience economic legislation. They qualify a strong reading of never fund to convert. They do not contradict blocking Skeptic and worse, because neither paper isolates members who were opposed at baseline.

**Gaps**
- No study tests whether PAC money to members opposed at baseline changes their positions. The study that would fill this is a within-member fixed-effects design (Stratmann / Grier style) on CU-relevant financial-services votes, with results split by prior stance tier. Window and member denominator must be named, and the result labelled UNSTRATIFIED if pooled.
- Furnas et al. 2022 is Paul's cited basis, but the evidence file has no bearing from it.
- There is no check on how many members the fail-open path admits, i.e. members with no matrix row.

**Paul's position vs the evidence**

Paul's reading is that ideological money cultivates champions rather than recruiting them. The VERIFIED evidence agrees in direction: aligned contact (Hall-Deardorff via Bombardini) and ideological donors (Bonica). Both are indirect, since one is FARA contacts and the other individual donors, not PAC-to-member conversion. The online leads add support (Hall & Wayman) and a qualification (Stratmann, Grier). Money appears to move marginal members on low-salience bills, which fits funding neutrals but has not been tested for opponents. The position is plausible but rests on analogy, not on a direct test.

**Open questions**
- Should the gate fail closed for members with no matrix row?
- Are Hall & Wayman 1990 and Stratmann 2002 worth adding to the vault? Stratmann 2002 is financial services, the nearest domain match.

**Read first:** [[bombardini-2020-empirical-models-lobbying]], [[bonica-2013-mapping-ideological-marketplace]], [[madsen-2017-strategic-advantages-micro]]

## Per-topic flip and coalition allocators

**What it computes.** Per-topic diagnostic allocations: greedy $1k steps on flip gain; coalition water-fill over the top-45 neutral recruits weighted by marginal spread x (1 + 0.8 bpf).

**Method.** Budget $2M per topic, cap $50k. Joint vs naive additive spread is reported (non-additivity).

**Code refs.** `fec/engine/per_topic_flip.py:1-100`, `fec/engine/per_topic_coalition.py:28-130`, `fec/engine/build_im_allocation.py:31-47`

**Assumptions**
- Bridge-builders are better seeds
- Topic budgets are diagnostic and not summable

**Evidence**

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

**Assessment**

**Overall: the flip allocator is sound. The coalition allocator is a heuristic with a known loss mode.** 9 bearings: 3 VERIFIED, 6 VERIFIED-SOFTENED.

**Flip allocator.** An exact heap-based greedy over fixed-state separable `pflip` chunks. Golovin adds nothing here, because greedy is already exact.

**Coalition allocator (`per_topic_coalition.py`).** Its steps are:
1. Shortlist the top 45 neutral members by engagement priority, not by marginal value.
2. Score each candidate by singleton `spread({n}) - base`.
3. Boost the score by `(1 + 0.8*bpf)`.
4. Water-fill $2M with a $50k cap.
5. Report the joint spread afterwards.

The evidence agrees across sources:
- **Kempe 2015** and **Bharathi**: singleton or centrality ranking ignores overlap. On hep-th, greedy beats high-degree by about 18% and distance centrality by over 40% under LT. Summed singletons overstate the joint gain.
- **Golovin**: a proportional split is not greedy, so no submodular guarantee applies, even if spread were submodular.
- **Chen 2023**: complex contagion means a seed may be valuable only alongside others. Pre-cutting by engagement can drop such a seed before any marginal is computed.
- **Ma 2022 and Trivedi 2020**: discounting for already-chosen neighbours helps. This is an extension argument; there is no measured gain on this graph.

Reporting the joint spread is the right diagnostic. Its meaning is limited: under pure IC the joint should be at or below the naive sum, so the comparison tests for complementarity only if threshold effects exist.

**Cai 2025.** Budget size changes which nodes are optimal to target. The `bpf` boost does not depend on budget, so this maps only indirectly.

**Zareie 2023.** Topic-aware IM is suggested, but the survey reports no accuracy evidence. The kernel is topic-independent.

**Contradictions**
- Paul places these allocators in the static and submodular tier. The coalition allocator runs no greedy, uses a proportional water-fill on singleton marginals, and runs on a bystander-filtered competitive kernel whose submodularity is unverified. No submodular guarantee applies to it as built. Golovin, Bharathi and Kempe all agree on this.

**Gaps**
- Nobody has measured what singleton ranking loses on the 533-member cosponsor kernel. The study that would fill this is a read-only comparison under the same CRN and equal seed count. It would set the water-fill funded set against lazy greedy, and against degree, PageRank and Katz-Bonacich seed sets as an ablation. Results should be labelled UNSTRATIFIED, with topic, window and the 533 denominator named. Spread is modeled cascade reach, not influence (D-025).
- There is no check of whether a seed outside the top-45 engagement shortlist would have a high marginal.
- Topic-conditioned kernels are untested. That would require dense per-topic cosponsorship history, and any change would fall under change control.

**Paul's position vs the evidence**

For the flip allocator, the submodular tier label is accurate and even understated, since greedy there is exact. For the coalition allocator, the label is not earned: it uses proportional allocation on singleton marginals, which the cited theory (Kempe 2003/2015, Golovin) does not cover. The evidence would support relabelling it as a diagnostic heuristic, which the code partly does already, and comparing it against lazy greedy before calling it submodular.

**Open questions**
- Should the coalition allocator be switched to lazy greedy, or kept as a diagnostic with a greedy comparison printed beside it?
- Should the top-45 pre-cut rank by singleton marginal rather than by engagement priority?

**Read first:** [[kempe-2015-maximizing-spread-influence]], [[bharathi-nd-competitive-influence-maximization]], [[golovin-2011-adaptive-submodularity-theory]], [[chen-2023-complex-contagion-influence]], [[trivedi-2020-efficient-influence-maximization]]

## Budget frontier with knee and electoral modes

**What it computes.** Spread vs cumulative seeds per topic; knee where 4 x gain >= peak; contest factor REPFAC 0.6 x lev.

**Method.** Lazy greedy up to 60 seeds under CRN.

**Code refs.** `fec/engine/budget_frontier.py:1-188`, `fec/engine/mc_stability_check.py:1-44`

**Assumptions**
- Diminishing marginal returns to seeds

**Evidence**

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

**Overall: a non-adaptive heuristic. No approximation guarantee carries over, and every bearing agrees.** 11 bearings: 2 VERIFIED, 9 VERIFIED-SOFTENED.

**How it is built.** `budget_frontier.py` runs a CELF-style lazy greedy, up to MAXSEEDS=60, over a stance-gated top-45 shortlist. Each marginal is a 20-draw Monte Carlo estimate on a two-colour competitive cascade with common random numbers (CRN). Marginals are clamped at zero and multiplied by `fac` (REPFAC x lev for contest seeds, 1.0 for defend seeds, lev otherwise). The knee is taken at a quarter of the peak marginal.

**Why no guarantee transfers**
- **Kempe 2015 and Manchanda 2020.** The (1-1/e) bound needs single-cascade submodular spread. A competitive race breaks that.
- **Bharathi.** The guarantee holds only with a fixed opponent set and `fac = 1`. Here contest removal and the bystander exclusion both change with the seed set.
- **Borodin.** Competitive threshold models lose submodularity and sometimes monotonicity. This hits the electoral contest and defend modes directly.
- **Golovin.** Sampled marginals give only additive error. The zero clamp is itself a sign that sampled spread is not monotone.

**Monte Carlo noise**
- **Almazaydeh 2026 (strong).** CELF++ at 50 MC draws reached only 12.9% of IMM at 1000 nodes, because each candidate's estimate had independent noise. Melange's edge-keyed CRN removes that failure mode for pairwise comparisons, and the brain records MEA-7 PASS (100% Jaccard at MC=100). The design is supported in principle.
- **The audit is too lenient.** `audit_frontier.py` tolerates inversions in up to 35% of marginals as noise, which can hide real non-submodularity.

**Knee and stop rule.** Both presume diminishing returns. Chen 2023 shows greedy can be arbitrarily bad under complex contagion. That bites only if real cosponsor joining is threshold-like; under the per-edge IC kernel the rule is internally coherent.

**Smaller points**
- **Chen 2021.** Supports putting seed take-up uncertainty inside the expected-spread objective.
- **Trivedi 2020.** Exact greedy is affordable because the pool is about 45 candidates.
- **Almazaydeh, ablation finding.** PageRank reaches about 96.5% of IMM. Usable only as an UNSTRATIFIED ablation diagnostic, never as a reason to replace greedy.

**Online lead for the electoral modes:** Stromberg 2008 (AER), allocation weighted by pivot probability. A candidate benchmark for contest/defend targeting, not read in full.

**Contradictions**
- The frontier is described with Golovin-Krause and GCOMB citations, which imply greedy guarantees. Kempe 2015, Manchanda 2020, Bharathi, Borodin and Golovin all say no guarantee carries over to a fac-weighted, clamped, MC=20, two-colour competitive cascade with an opponent set that changes as seeds are added. The citations are context, not certificates.
- `audit_frontier.py` passes up to 35% marginal inversions as MC noise. Under CRN, inversions of that size are more likely to signal structural non-submodularity than noise. The audit threshold contradicts the claim that the objective behaves submodularly.

**Gaps**
- Raw marginals under CRN have never been checked to separate noise from structure. The study: re-run the frontier read-only at MC=20 against MC=200 with the same CRN keys, and count inversions on unclamped marginals.
- The electoral contest and defend modes have no outcome validation. The study: a retrospective comparison of contest/defend picks with realized race margins, or with a pivot-probability benchmark in the style of Stromberg 2008.
- The orienteering (political-capital budget) formulation Paul describes is not built and has no evidence rows. GCOMB lists capacity-constrained and sequential problems as future work, so it does not cover this.

**Paul's position vs the evidence**

Paul says GCOMB's budget framing maps onto a political-capital budget in an orienteering formulation. Manchanda 2020 evaluates only single-colour IC set selection and lists capacity-constrained and sequential variants as future work. GCOMB therefore supports neither the orienteering mapping nor the competitive frontier as built. The position is an unbuilt design idea with no evidential backing in this set.

**Open questions**
- Should the frontier and the knee be labelled heuristic model output, no approximation guarantee on every surface?
- Should the 0.35 inversion tolerance in `audit_frontier.py` be tightened, or replaced with an unclamped CRN inversion count?
- Is the orienteering formulation still on Paul's roadmap?

**Read first:** [[kempe-2015-maximizing-spread-influence]], [[bharathi-nd-competitive-influence-maximization]], [[borodin-nd-threshold-models-competitive]], [[almazaydeh-2026-heuristic-centrality-methods]], [[golovin-2011-adaptive-submodularity-theory]], [[chen-2023-complex-contagion-influence]]

## Workbook allocator (top-133 from fixed pot)

**What it computes.** Funds the top 133 of 533 members from a flat $2.0M in proportion to score (uncapped; max about $25.8k).

**Method.** A capped variant ($10k cap, top 250) exists only in a backup and was not applied.

**Code refs.** `Blended Model col U`, `fec/apply_alloc_cap.py (not applied)`

**Assumptions**
- Score ordering maps to funding priority

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[cagala-2021-optimal-targeting-fundraising]] | 2021 | qualifies | Treating everyone as a gift recipient is net-negative or near zero: all-gift minus no-gift is +0.05 to +0.08 euro for the warm list (not significant) and -0.97 euro per donor for the cold list (significant) (Table 1). Targeted warm-list donors differ from non-targeted donors only modestly in prior giving (8-year total 130.98 vs 123.39 euro; 787 targeted vs 1,567 non-targeted; Table 4, Panel A). The paper cautions that observed characteristics are correlates of response, not the channels through which the gift works (Section 5.4). | Conceptual only. The magnitudes come from donors, not legislators. The transferable point is to compare any proportional allocator against uniform and random benchmarks. The paper does not show that the allocator is wrong, and the system side is unverified. | weak | VERIFIED-SOFTENED |

**Assessment**

**Overall: a GAP on both the evidence side and the system side.** 1 bearing: Cagala 2021 (VERIFIED-SOFTENED, weak, conceptual). Its point is that blanket treatment is net-negative or near zero: the all-gift minus no-gift difference is +0.05 to +0.08 euro on the warm list (not significant) and -0.97 euro per donor on the cold list (significant). Targeting rules must be evaluated against uniform and random benchmarks. The donor magnitudes do not transfer to legislators.

**System side, an actual error.** The cited producer `fec/apply_alloc_cap.py` does not exist. The Blended Model sheet's Allocation column (U) holds no cached values in the copy read. The top-133 of 533 from a flat $2.0M pot claim was not reproduced.

Proportional allocation down a ranked list is the same structural move as the coalition allocator's water-fill. Kempe 2015 and Golovin 2011 therefore apply by extension: proportional or rank-based allocation carries no optimality guarantee, and greedy on marginals is the benchmark.

Huneeus & Kim 2018 is Paul's cited basis, but it has no bearing in the file.

**Contradictions**
- The system description says the top 133 of 533 members are funded from a $2.0M pot. The producer script it cites (`fec/apply_alloc_cap.py`) does not exist, and the workbook column has no cached values. The claim is unsupported by the repo as read.

**Gaps**
- No evidence addresses proportional top-N allocation for PAC money. The study that would fill this is a read-only comparison of the workbook allocation against uniform, random-N and joint-optimizer greedy allocations, scored on the same modeled objective and, if outcomes exist, on realized outcomes, following the Cagala benchmark design.
- There is no Huneeus & Kim 2018 bearing to support Paul's fundraising goals reading.
- The producer of the allocation column needs locating or rebuilding before any assessment is possible.

**Paul's position vs the evidence**

Paul sees Huneeus & Kim as the basis for setting fundraising goals, and no annotation addresses proportional allocation. The evidence file contains nothing from Huneeus & Kim, so the position is untested here. The only bearing, Cagala, warns that rank-based or blanket allocation must beat simple benchmarks to justify itself. Nothing shows the workbook allocator does.

**Open questions**
- Where is the workbook Allocation column actually produced now that `fec/apply_alloc_cap.py` is gone?
- Should the workbook allocator be retired in favour of the joint optimizer's output, or kept as a simple communication device?

**Read first:** [[cagala-2021-optimal-targeting-fundraising]], [[kempe-2015-maximizing-spread-influence]], [[golovin-2011-adaptive-submodularity-theory]]

## Break-even CU spend and ROI floor

**What it computes.** The CU dollars that restore baseline flip odds against competitor spend (closed form, 'cannot offset' when q >= 1), and member ROI floor values.

**Method.** Solves a(CU)/(a(CU) + b(comp)) = pfbase with the saturating money term; ROI floor parity is checked by roi_verify.py.

**Code refs.** `fec/templates/make_dashboard_html_00.tpl (break-even)`, `verify_finish/roi_verify.py`

**Assumptions**
- Competitor and CU money enter a share-type ratio

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | qualifies | Kang's model implies an average policy value of more than $500m, and the Farm Bill comparison gives a naive return of 600,000% ($22bn in 2000 subsidies vs under $3.7m in agribusiness PAC contributions that cycle). The review says excessive returns are common in this literature. Adding an electoral-vote channel (Bombardini and Trebbi 2011: $200-$400 per vote) lowers the Farm Bill figure to $13.7 per dollar, which the authors call still high but more reasonable; B&T's simulations give average returns of 10%-20% per year. The conclusion notes some structural models display symptoms of misspecification. | This is a caution about any ROI read off contribution-to-outcome ratios. It applies whenever dashboard ROI floors are presented as returns. It does not invalidate the break-even algebra, which is internal to the model. Any ROI shown should carry an UNVERIFIED or model-output label. | moderate | VERIFIED-SOFTENED |
| [[bickmore-2025-corporate-lobbying-political]] | 2025 | qualifies | Lobby on ShareAssets 0.0743 with full controls; PAC on ShareAssets 0.1100 (election years, N = 62,198). Abramoff DiD Treatment x Post on ShareAssets with full controls is -0.0265 (SE 0.0096); on ShareMcap it is -0.0336 (SE 0.0099). Post window 2006Q1-2009Q1; pre-period 2003Q1-2005Q4 per the Table 8 description. | The paper's return is a firm-level industry-share outcome from Compustat matched to CRP by company name, 1998-2016. It does not measure votes, cosponsorship or flips, so it cannot calibrate WM, HALFF or LOSSFRAME. It supports only a framing claim that political spending associates with firm share gains, reported in share points with the unit stated. | moderate | VERIFIED-SOFTENED |
| [[alexander-2009-lobbying-analysis-under]] | 2009 | qualifies | Overall return on lobbying 220 = (46,157.5 + 15,897.0) / 282.7, i.e. about 62.05bn estimated savings on 282.7M lobbying across 93 lobbying repatriators (FY2004-2006). Coalition 257, non-coalition 154, per-company mean 869, per-company median 172.2, lobbying over 1M subsample (48 firms) 243. | No numeric transfer. The paper numerator is a firm tax benefit from a one-time provision and the denominator is LDA spend (upper bound). Melange numerator is modeled member movement and denominator is FEC PAC money. Any quote needs the window FY2004-2006, the 93-firm denominator, and the median 172 next to the mean 869. The bearing describes the PAC side as capped at 10k; the code uses 10000 as a marginal increment, so describe it as a per-10k increment. | moderate | VERIFIED-SOFTENED |
| [[cai-2025-competitive-influence-maximization]] | 2025 | qualifies | Two settings are mixed here. The optimal start for Congress votes at T=16 falls from 14.730 at budget ratio 0.1 to 10.476 at ratio 10 (Table 2, constant-opponent setting, fixed opponent starting at 0). In the two-controller Nash game the controller with the larger budget moves first, and the second mover can only buffer the first mover's gain, never exceed its vote share (Sec 4.3.4, stage 3, p.21-22). Fig 9b shows per-node resource per unit time rising with the budget advantage. | The paper's statement that a disadvantaged controller can offset but not overtake agrees in spirit with the cannot-offset message. The claim that a disadvantaged side does better by delaying has no counterpart in a static break-even, and it rests on the paper's abstract and simulations only. Transfer would need a model of within-cycle spending order, which the system lacks. | weak | VERIFIED-SOFTENED |
| [[goff-nd-cross-industry-finance]] |  | qualifies | Crypto outspends key allies by factors of 100, so credit unions would have no chance in a monetary fight with the industry; the supporting graph is a missing placeholder. | The factor of 100 has no source, window or denominator, so the system cannot size the disadvantage. The bounded term shows only that the form caps the monetary pull, not that money decides outcomes. | weak | VERIFIED-SOFTENED |

**Assessment**

**Overall: the break-even algebra is internal to the model and needs no outside support. Any ROI figure shown as a real-world return is unsupported.** 5 bearings: 5 VERIFIED-SOFTENED.

**Bombardini 2020 (moderate).** Naive return figures in this literature are routinely absurd: the Farm Bill comparison gives about 600,000%. Adding an electoral-vote channel cuts that to $13.7 per dollar, and Bombardini & Trebbi's simulations give 10-20% per year. Some structural models show symptoms of misspecification. Any ROI floor shown on the dashboard needs a model-output or UNVERIFIED label.

**Alexander 2009.** The 220x figure covers FY2004-2006, is computed over 93 repatriating firms, and comes from a one-time tax provision measured against LDA spend. The per-company median is 172 against a mean of 869. Nothing transfers numerically. `desk_roi` measures modeled weighted flips per $10k increment, which is not a firm return.

**Bickmore 2025.** Firm industry-share coefficients (Compustat x CRP, 1998-2016, N=62,198, pooled across industries, so quote as UNSTRATIFIED). They cannot calibrate WM, HALFF or LOSSFRAME. Framing use only, in share points.

**Cai 2025 (weak).** A disadvantaged controller can buffer but not overtake. This loosely matches the cannot fully offset when q >= 1 message. The paper's timing result has no counterpart in a static break-even.

**Goff n.d.** The 100x crypto outspending claim has no source, window or denominator, and the graph is a placeholder.

**Online leads (unread in full):**
- **Ansolabehere, de Figueiredo & Snyder 2003.** Small vote effects; contributions behave more like consumption than investment.
- **Stratmann 2002 and Grier et al. 2023.** Measurable vote effects on low-salience bills.

Together they bracket the plausible range and argue against presenting large ROI multiples.

**Actual errors:**
- `roi_verify.py`, said to check the ROI floors, was not found.
- Kim J. et al. 2025 and Huneeus & Kim 2018 are cited but have no bearings.

**Contradictions**
- The ROI floors are said to be checked by `roi_verify.py`. No such file was found in the repo, so the claim is unconfirmed.
- The head-to-head panel's saturating functional form, `(x+1)/((x+1)+HALFF*rcp)`, is an assumption that has not been validated against outcomes. The cannot offset message therefore reflects the functional form, not evidence that money decides outcomes.

**Gaps**
- Competing-industry conflict, which Paul wants included, has only one weak bearing (Cai 2025: buffer, not overtake). The study that would fill it is a retrospective analysis of contested votes where CU and an opposing industry both gave. It would estimate whether relative money predicts outcomes, conditional on stance, with the window and vote denominator named.
- No bearing exists for Huneeus & Kim 2018 or Kim J. et al. 2025, although both are cited.
- The 100x crypto disadvantage needs a sourced figure: an FEC window, a denominator and the named allies.

**Paul's position vs the evidence**

Paul frames lobbying as ROI, calls the Alexander figure an outlier, and wants competing-industry conflict included. The evidence agrees that Alexander is an outlier: Bombardini says excessive returns are common and models are often misspecified, and Alexander's own median (172) is far below its mean (869). The evidence cautions against the ROI framing itself when figures are shown as real returns. Melange's ROI is modeled flips per dollar, not firm returns, and should be labelled that way. Competing-industry conflict is in the dashboard only as a static closed-form solve and has almost no evidential support.

**Open questions**
- Should dashboard ROI and break-even outputs carry a fixed model output, not a measured return label?
- What is the source and window for the 100x crypto outspending figure?
- Where did `roi_verify.py` go, or was it never built?

**Read first:** [[bombardini-2020-empirical-models-lobbying]], [[alexander-2009-lobbying-analysis-under]], [[bickmore-2025-corporate-lobbying-political]], [[cai-2025-competitive-influence-maximization]]

## Across components

**Patterns that run across components**

1. **Exact where separable, heuristic where competitive.** The joint optimizer and the flip allocator (separable, concave and deterministic in a fixed state) are the only places where a greedy guarantee or an exactness claim holds, and the code scopes both correctly. The budget frontier and the coalition allocator (a competitive two-colour cascade, Monte Carlo sampling, fac weighting, proportional splits) carry no guarantee. Every surface that shows frontier, knee or coalition output should say heuristic model output. The Golovin, Kempe and GCOMB citations are context there, not certificates.

2. **No outcome loop anywhere.** All six components optimize modeled quantities: winnability, spread, flips per $10k and break-even dollars. None is scored against realized outcomes or against simple benchmark allocations (uniform, random, actual CU giving). The single most valuable study for the whole system is one shared retrospective benchmark backtest in the Cagala design. Run read-only, it would score every allocator on the same held-out cycle with a named window and denominator.

3. **The frozen opponent.** Industry money is static in the optimizer and the frontier, and the head-to-head break-even is a static ratio. Moreno 2021 and Cai 2025 both show the answer changes when the opponent re-optimises or when spending is timed. Every optimal plan is a best response to a passive industry.

4. **GCOMB/learned-policy expectation.** Across the optimizer and the frontier, the learned-policy literature in the set (GCOMB, RELINK, S2V-DQN, Q-learning) offers speed on graphs of 1M+ nodes or quality on non-separable set selection. Melange's problem is about 533 nodes, capped and partly separable. The expectation has no evidential support at this scale.

5. **Ranking before marginals.** Both the frontier and the coalition allocator pre-cut to a top-45 shortlist by engagement priority before any marginal is computed. Chen 2023 and Kempe 2015 both identify this as the place where combination-dependent value is lost.

6. **Citation hygiene.** Huneeus & Kim 2018 is cited for three components, Furnas 2022 for the stance gate and Kim J. 2025 for ROI, and none of them has an extracted bearing. The code also has one wrong lemma reference (Golovin-Krause) and two missing scripts (`apply_alloc_cap.py`, `roi_verify.py`).

**SNA guardrails.** No bearing in this set argues for dropping regular cosponsorship edges or for per-edge volume normalization, so nothing needed flagging. No bearing uses LDA as an edge or weight source. The Almazaydeh centrality result (PageRank at about 96% of IMM) must be quoted only as an UNSTRATIFIED ablation of modeled spread, never as access or closeness (D-025).

## Research agenda

1. Benchmark backtest (all allocators, read-only): score the joint optimizer, workbook top-N, coalition water-fill and the actual CU PAC allocation against uniform and random allocations of the same pot. Score on realized member-level outcomes, such as cosponsor joins or votes on CU-relevant bills, for one named past cycle. Members are the denominator. Results stay model-internal and labelled UNVERIFIED until a second path reproduces them (DE-062).
2. Coalition allocator vs lazy greedy under identical CRN keys and equal seed count, plus degree, PageRank and Katz-Bonacich ablations. Report the joint modeled spread for each, labelled UNSTRATIFIED, with topic, window and the 533 denominator named.
3. Frontier noise vs structure: re-run read-only at MC=20 and MC=200 with the same CRN keys, and count inversions on unclamped marginals. Use the result to decide whether the 0.35 inversion tolerance in `audit_frontier.py` should be tightened.
4. Stance-gate causal test: a within-member fixed-effects design in the Stratmann 2002 / Grier et al. 2023 style on financial-services votes, split by prior stance tier, to test whether money to Skeptic/Opponent members moves votes. Separately, count how many members the gate admits via the fail-open (no matrix row) path.
5. Opponent response: a diagnostic best-response or Nash check that lets industry money re-optimise against the CU plan (Moreno 2021), to measure how exploitable the frozen-opponent optimum is.
6. Fix the actual errors: correct the Golovin-Krause lemma citation in `optimize.py`; locate or rebuild the producer of the workbook Allocation column (`fec/apply_alloc_cap.py` missing); locate or retire `roi_verify.py`; source or remove the 100x crypto figure.
7. Extract bearings for cited papers that have none: Huneeus & Kim 2018, Furnas et al. 2022 and Kim J. et al. 2025.
8. Online-scan leads to add to the vault and verify, since only abstracts and summaries were read. Stratmann 2002 (J. Law & Econ., financial-services votes): https://chicagounbound.uchicago.edu/jle/vol45/iss2/3. Grier, Grier & Mkrtchian 2023 (APSR, sugar contributions, open access): https://www.cambridge.org/core/journals/american-political-science-review/article/campaign-contributions-and-rollcall-voting-in-the-us-house-of-representatives-the-case-of-the-sugar-industry/DBB341572A3FC0A1D06705B5DDE5C497. Hall & Wayman 1990 (APSR, Buying Time): https://ideas.repec.org/a/cup/apsrev/v84y1990i03p797-820_19.html. Ansolabehere, de Figueiredo & Snyder 2003 (JEP; NBER w9409): https://www.nber.org/papers/w9409. Powell, Money in Exile (committee access): https://appliedstatsworkshopgov3009.hsites.harvard.edu/presentations/money-exile-campaign-contributions-and-committee-access. Kalla & Broockman 2016 (AJPS, donor access experiment; literature on access only, never applied to graph scores per D-025): https://ajps.org/2015/05/12/how-campaign-money-facilitates-access-to-policymakers. Stromberg 2008 (AER, pivot-probability allocation, a benchmark for frontier electoral modes): https://ideas.repec.org/a/aea/aecrev/v98y2008i3p769-807.html.
