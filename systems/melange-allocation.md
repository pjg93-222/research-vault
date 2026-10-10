---
type: system
name: Melange budget allocation
slug: melange-allocation
components: [melange-allocation.joint-optimizer, melange-allocation.stance-money-gate, melange-allocation.per-topic-allocators, melange-allocation.budget-frontier, melange-allocation.workbook-allocator, melange-allocation.break-even-and-roi]
brain_notes: [actors/quant-model-evaluation-skill, actors/melange-dashboard-june-capstone, concepts/influence-vs-alignment-axes]
paper_count: 23
tags: [system]
---

# Melange budget allocation

Spending decisions made on top of the influence engine: the joint CU pot optimizer, per-topic flip and coalition allocators, the budget frontier (including electoral contest and defend modes), the workbook top-N allocator, the stance money gate, break-even CU spend and the ROI floor.

**Brain notes:** [actors/quant-model-evaluation-skill](obsidian://open?vault=brain&file=actors/quant-model-evaluation-skill); [actors/melange-dashboard-june-capstone](obsidian://open?vault=brain&file=actors/melange-dashboard-june-capstone); [concepts/influence-vs-alignment-axes](obsidian://open?vault=brain&file=concepts/influence-vs-alignment-axes)

## Melange budget allocation: evidence assessment

**Bottom line.** The evidence on this system is almost entirely about **the math of the allocators**: which approximation guarantees apply, how noise in the simulations affects greedy selection, and how competing spenders change things. It says almost nothing about **whether the allocations work in Congress**. None of the 40 bearings tests a Melange allocation, or any comparable PAC allocation, against what legislators actually did. All of the outcome evidence comes from other settings:

- church donors (Cagala 2014-15)
- firm market share (Bickmore 1998-2016)
- a one-time tax provision (Alexander FY2004-2006)
- FARA contact logs
- synthetic or non-legislative graphs

So every allocation "gain" the system reports is a model output. It should be described that way until the outcomes are scored against a benchmark allocation.

**Status of the 40 bearings.**
- 8 are VERIFIED.
- 30 are VERIFIED-SOFTENED.
- 2 are UNVERIFIED: zeng (a drone-swarm reinforcement-learning study) and madsen-2017 (a voter micro-targeting agent model). Both are analogies only. Under DE-062 they are leads, not findings.
- No bearing argues for either of the two rejected SNA proposals (dropping regular cosponsorship edges; per-edge volume normalization). Nothing needs flagging on that front.

**How well each component holds up.**
- **Joint CU optimizer:** best supported. In a fixed stance state the objective is separable and concave, so greedy is the exact optimum on the $500 grid. The code correctly withholds that certificate once modeled conversions fire.
- **Budget frontier and coalition water-fill:** heavily qualified. As built, no (1-1/e) guarantee transfers. The reasons are the two-colour competitive kernel, the per-candidate `fac` weights, the bystander exclusion that changes with the seed set, Monte Carlo noise at MC=20, and the fact that the water-fill is not greedy. The honest label for both is **heuristic**.
- **Stance money gate:** weakly supported as a policy. Lobbyists contact friendly legislators, and donors give on ideology. But no bearing tests whether money given to opponents converts them, which is the question the gate actually rests on.
- **Workbook allocator:** essentially unevidenced. Its own system-side claims could not be reproduced: column U had no cached values, and `fec/apply_alloc_cap.py` does not exist.
- **Break-even and ROI:** the algebra is internal and consistent. The literature warns that ROI figures read off contribution-to-outcome ratios are often implausibly large and point to misspecification. `roi_verify.py` was not found in the repo.

**Where Paul's positions stand.**
- The view that greedy plus CELF is the operational layer is supported, and at this scale it is stronger than Paul states.
- The expectation that a learned policy (GCOMB) will replace greedy has no supporting bearing. At 533 members with a separable objective, greedy is exact and cheap. The GCOMB paper is not among the evidence rows, and the one RL-beats-rules row (zeng) is an UNVERIFIED drone analogy.
- The orienteering / political-capital formulation is unbuilt and unevidenced.
- The "cultivate champions" reading behind the stance gate agrees with the evidence but is not tested by it.
- Calling the Alexander figure an outlier is only partly right. The review literature says very high naive lobbying returns are common, not exceptional, so the whole class of figures is suspect, not just Alexander's.

## Joint CU budget optimizer

**What it computes.** Allocation of a $2.5M pot in $500 steps maximizing total winnability, with a per-candidate cap PMAX $10k.

**Method.** CELF lazy greedy on per-member concave chunks. Modeled conversions rebuild the queue. Exact only in a fixed stance state. Golovin-Krause bound as a diagnostic. Knee at 20% of peak marginal.

**Code refs.** `fec/engine/optimize.py:1-177`

**Assumptions.**
- The objective is separable and concave in CU dollars (checked numerically)
- The pot is one PAC's budget (F2 ruling)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[pritoni-2025-lobbying-coalitions-determinants]] | 2025 | qualifies | The literature finds both positive (Junk 2019; Nelson and Yackee 2012) and negative (Bunea 2013; Haider-Markel 2006; Heinz et al. 1993; Junk 2020a) links between forming a coalition and policy influence, and calls the results contradictory. The review leaves coalitions as an explanatory variable for influence to future research. | The weapon-of-the-weak evidence might tempt someone to pool allied industries' dollars into the CU money leg. The review does not support that, because the effect of coalitions on outcomes is unresolved. The single-actor pot remains defensible on this evidence. | weak | VERIFIED: The pot is one PAC's budget (Paul's ruling of 2026-09-30). PMAX defaults to $10k per member above existing CU money, which is that PAC's per-candidate per-cycle limit. Allied industries' money is not represented, and CU money is treated as a single actor. |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | method-source | Lazy greedy (Algorithm 2) loses no solution quality because stale marginals upper-bound current ones. Speedups were about 3.5-7x fewer evaluations on 46 sensors (temperature) and 30-38x on 357 sensors (traffic) at 50 percent failure. The Lemma 6 data-dependent bound was tighter than the worst-case bound in the sensor runs. | Carries over in the deterministic fixed-state case where per-member chunk marginals are non-increasing, which the code's concavity check tests. It does not carry over once modeled conversions fire; the code then withholds the certificate. The code citation Golovin-Krause JAIR 11 Lemma 5.3 / A.9 does not match this text copy, which has Lemma 6 Eq.13 and Lemma 37 for costs. | strong | VERIFIED: Heap-based lazy greedy over $500 chunks in optimize.py re-queues a member only when its marginal is stale. dd_bound sums each member's current next-chunk marginal times the chunks left in its PMAX headroom, within BUDGET, and is computed only under the fixed-state certificate. The code comment calls exact greedy optimality stronger than the 1-1/e floor. |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | Theorem 5's (1-1/e) bound compares with the best adaptive policy and needs observed outcomes, an adaptive monotone submodular objective and a known prior (Def 2-3 p.8; Thm 5 p.11). Treasure-hunting instances with strong synergies fall outside the class (Sec 3.4 p.9). Theorem 26 shows no O(/E/^(1-eps/beta)) approximation for a constructed treasure-hunting family of pointwise-modular instances unless PH = Sigma_2^P (Sec 12 p.33). | The paper supports the refusal to claim a certificate in conversion mode, since a threshold jump can make a later chunk's marginal rise. Softened scope: Theorem 26 is a hardness result for one constructed family, not for every objective lacking adaptive submodularity. Sec 12 frames it more broadly, but the proof covers only that family. | strong | VERIFIED-SOFTENED: Nothing is observed (observations is an empty list). When cumulative CU dollars cross the deterministic DTHRESH threshold, the member is banked as won, the stance matrix shifts and the queue is rebuilt. The run is labelled modeled_threshold_replanning with guarantee kind none. |
| [[cagala-2021-optimal-targeting-fundraising]] | 2021 | qualifies | Warm-list targeting rule (2014 field experiment, first-year net donations; warm list N=2,354 with 1,180 gift and 1,174 control; 20-fold cross-validated out-of-sample): gift to 33 percent of warm-list individuals; expected net donation 17.61 euro, which is +2.14 (13.8 percent) vs all-gift, +2.20 (14.3 percent) vs no-gift and +2.17 (14.1 percent) vs random-half, all significant at 1 percent (Table 2, Panel B). On the full 2015 follow-up sample (N=3,616): expected 17.16 euro, -0.88 vs all-gift and +0.45 vs no-gift, neither significant (Table 8, Panel B2). | The transferable part is the evaluation design: benchmark-relative, net-of-cost gains on held-out units under randomized treatment. Effect sizes do not transfer (1.16-euro gift to German church donors in 2014-15, vs PAC dollars to members of Congress). FEC limits and the absence of randomization mean the system cannot replicate the identification. Any optimizer gain stays model-internal and UNVERIFIED (DE-062) until outcomes are scored against a benchmark allocation. | moderate | VERIFIED: fec/engine/optimize.py: lazy greedy allocation of BUDGET (default 2,500,000) in INC (default 500) chunks across members, maximizing modeled winnability in a fixed stance state. The docstring calls the greedy an exact discrete optimum within that state and withholds the fixed-state certificate when a modeled money threshold changes stance. The observations list is reserved and empty; no realized-outcome comparison against uniform, random or no-spend allocation was found. |
| [[arya-2025-relink-edge-activation]] | 2025 | qualifies | RELINK's learned edge policy is slower than PSNA on small and medium graphs and becomes competitive or often faster on large graphs, which the paper attributes to GPU inference and PSNA's complexity (Sec 6.2 Q3, Fig 5). Training took 6,257.329 s for 2,000 epochs because exact marginal gains are computed (Sec 6.4). The objective is non-submodular (Introduction). | The paper gives no graph-size threshold, so the tens-of-thousands figure in the rel is not from the paper. RELINK's case for a learned policy rests on scale and on non-submodular edge choice. The separable allocation in optimize.py is neither, so this paper neither supports nor refutes replacing greedy there. All benchmarks are synthetic-trained IC simulations. | moderate | VERIFIED-SOFTENED: optimize.py is a lazy greedy over per-member chunks. Its docstring says that in a fixed stance state the objective is separable by member with diminishing returns, so greedy is the exact discrete allocation optimum. A Golovin-Krause bound is kept as a diagnostic. The dashboard budgetRecruit comment says no ML needed at our scale (tpl line 776). |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | Thm 4: an alpha-approximate adaptive greedy gets at least (1 - e^{-l/(alpha k)}) of the best policy; alpha=1 and l=k give 1-1/e. Thm 9: for general instances no polynomial algorithm with budget beta*k approximates the optimum within O(/E/^{1-eps/beta}) unless PH = Sigma2P, even for pointwise linear f. | Thm 4 needs an adaptive monotone and adaptive submodular objective. The fixed-state case is stronger than the paper because it is separable and concave, so the paper adds nothing there. Lemma 5.3 (Adaptive Data Dependent Bound) is in the JAIR text (file 6ba342dad525.txt, line 710) but not in the 13-page CoRR text. A.9 was not located. Thm 9 is worst-case over general instances and does not by itself show that greedy fails on the actual cosponsor instance. | strong | VERIFIED-SOFTENED: optimize.py runs a heap-based lazy greedy over INC increments (default INC=500). In a fixed stance state the objective is separable with verified concave chunks, giving an exact grid optimum. If a modeled money conversion is reachable the guarantee kind is 'none'. The Golovin-Krause data-dependent bound is labelled 'JAIR'11 Lemma 5.3 / A.9' at line 108. |
| [[borodin-nd-threshold-models-competitive]] |  | qualifies | In the weight-proportional competitive LT model (Eq. 1) the A-spread is not monotone (Theorem 1: root A-probability 11/40 before and 103/400 after adding one A seed, Appendix A Theorem 12) and not submodular (Theorem 2). The Nemhauser-Wolsey-Fisher greedy bound does not transfer to that model. | Structural analogy only. Borodin non-monotonicity runs through multi-step propagation under random thresholds, which pflip does not have. The conversion-path concern applies only when conversion_reachable is true. Whether the mAt renormalisation lowers total winnability after a conversion is not measured in this text. | moderate | VERIFIED-SOFTENED: pflip = a/(a+b) (flip_model.py 92-97) has the form of Eq. 1, but a and b each add a money term to WP*nA/mAt, so it is not the same model. Stance is fixed inside a run, so the objective is separable by member. optimize.py checks concavity at run time (concavity_violations) and labels only the fixed-state run as fixed_state_lazy_greedy. A modeled conversion re-runs topic_setup, where mAt = max(nA) (flip_model.py 138-140), so one conversion can lower another member's WP*nA/mAt term. Such a run is labeled modeled_threshold_replanning (optimize.py 190) and carries no certificate. |
| [[cai-2025-competitive-influence-maximization]] | 2025 | suggests-extension | With the budget fixed, choosing a start time raises final vote share over spending from t=0 (Table 2, constant-opponent setting). For N=100 networks, per-node start times and budgets raise vote share further: the relative gain is large at small budget ratios but small in absolute terms (Fig 6a relative axis 1.00 to 1.25). A resource-limited controller starts later and a resource-rich one starts earlier (abstract; Sec 4.3.1). Under the uniform-start formula (Eq 3-5) the controller spends its budget over a short late window; the paper does not prescribe this as a rule. | Only as a hypothesis. PAC dollars cannot be withdrawn, FEC per-cycle limits bind, and the paper's continuous gain a_i(t) has no clear counterpart in lump-sum contributions. The paper's large relative gain at small budgets means the per-node result should not be read as uniformly small. Untested on legislators. | weak | VERIFIED-SOFTENED: optimize.py docstring describes a lazy greedy allocator with a Golovin-Krause bound kept as a diagnostic. Defaults are a 2.5M pot in 500 increments with a 10k per-candidate cap (PMAX). It allocates amounts per member with no start time or spending schedule; the objective is time-free winnability. |
| [[moreno-2021-shadowing-shielding-effective]] | 2021 | qualifies | With a passive opponent the continuous optimum spreads widely: 111 of 1,133 nodes (H plus A) get above-average allocation and 1,013 nodes (V) are near flat with std/mean 0.11. Continuous optima beat the best discrete plan by 5-10 percent for passive K below 100 (Fig 1c, 15 samples). With two active controllers the unique pure-strategy Nash equilibrium is uniform targeting w_ai = Ba/N, reported as independent of network type and budgets (numerical result; the uniqueness proof is in S1 Appendix Section 12, not in the text file). | The paper's spread and shielding come from money propagating through the network's steady state. Melange's dollar term is node-local, so the paper's spread cannot arise from dollars alone; shielding-like behaviour can arise only through the b term of a/(a+b) and through conversion-driven stance moves. The Nash result implies the plan is a best response to a frozen opponent and is exploitable if industry re-optimises; qualitative only. Equal-budget and per-candidate FEC limits have no analogue in the paper. | moderate | VERIFIED-SOFTENED: optimize.py greedy spends a 2.5M pot in 500-dollar chunks with a 10k per-member cap (lines 24-27). Industry money ind = fm.ind0 is set once at line 59 and never rewritten, so industry is a frozen passive opponent. marg_at (lines 64-72) changes only the funded member's pflip. Conversion events call shift_pro (lines 93-105, 136-142), which rewrites that member's stance in MAT; the nA and nB camp totals in flip_model.topic_setup are rebuilt from MAT, so a conversion does move neighbours' pull. |
| [[zeng-nd-multi-agent-reinforcement]] |  | context | An RL controller (Q-learning, linear function approximation, alpha 0.1, gamma 0.75) beats a rule-based Boid baseline on one simulated drone scenario: near-collisions 45 vs 108 (-58%), ideal-range steps 3,176 vs 2,764 (+15%), heading deviation 14.3 vs 21.1, all over 3,500 steps with N = 17 UAVs. No seeds, replicates, variance or baseline specification are given. | Carries over only as an analogy: a learned policy versus hand-set rules in a multi-agent setting. Nothing transfers to political allocation. The agents are physical drones, the reward is hand-specified geometry, and there is no budget, submodularity, money or legislator response. The evidence is one unreplicated simulation, so it cannot show that RL would outperform greedy anywhere, let alone on the cosponsor kernel. | weak | UNVERIFIED: The built optimizer uses deterministic CELF lazy greedy over per-member concave chunks ($500 steps, PMAX $10k cap, stance-gated members excluded). It has no learned policy. Paul's notes expect a learned policy (GCOMB) to replace greedy at scale. |
| [[zareie-2023-influence-maximization-social]] | 2023 | qualifies | Cost-aware (budgeted) IM assumes every influenced user brings the same benefit. Profit-aware IM models per-user benefit, but data on users' monetary concerns and valuations may not always be readily available, so cost-aware methods may be more relevant in practice. Nguyen and Zheng (2013) first introduced budgeted IM. | Partial analogy. The optimizer allocates money that shifts per-member flip odds and does not pick cascade seeds, so the survey's spread objective does not apply. The survey's point carries over: results depend on the PERS and IMP tables, which the code labels calibrated constants. A uniform-benefit variant would be a sensitivity check, not a replacement. | weak | VERIFIED-SOFTENED: optimize.py runs lazy greedy over a $2.5M pot in $500 increments with a $10k per-member cap (BUDGET, INC, PMAX defaults). Member targets are weighted by the PERS and IMP tables and by engagement, direction and delivery terms. The objective is Linear-Threshold winnability on per-member flip odds, not spread from cascade seeds, so the analogy is structural only. |
| [[almazaydeh-2026-heuristic-centrality-methods]] | 2026 | qualifies | Theoretical guarantees such as the (1-1/e) bound can diverge sharply from realized performance when marginal gains are estimated by noisy simulation. CELF++ reached 12.9 percent of IMM at 1000 nodes. | The paper's gap comes from MC estimation error, which does not arise in a deterministic objective, so the paper does not undercut the optimizer's fixed-state certificate. It would become relevant only if the objective were replaced by a simulated cascade. | moderate | VERIFIED: optimize.py runs a fixed-state lazy greedy over the INC grid (INC default 500 and BUDGET default 2500000 at line 26). Its docstring says the objective is separable and concave in a fixed stance state, and that highest-next-marginal selection is the exact discrete optimum. certificate_valid = fixed_state_concave and not conversions (lines 48 and 166); a modelled conversion switches the mode to modeled_threshold_replanning (line 190). No Monte Carlo call appears in the sections read. |

### Assessment

**Evidence weight: moderate-to-strong on the algorithm; none on outcomes.**

**What is verified.** golovin-2011-adaptive-submodularity-theory (VERIFIED, method-source, strong) shows that lazy greedy (Algorithm 2) loses no solution quality, because stale marginals are upper bounds on current ones. That transfers directly to the heap-based CELF over $500 chunks.

Two VERIFIED-SOFTENED bearings (golovin-2011 theory and approach versions) add that Theorems 4 and 5 need an adaptive, monotone, submodular objective with observed outcomes. They also say a threshold jump can make a later chunk's marginal *rise*.

The code already handles both cases correctly:
- **Fixed stance state:** the objective is separable and concave, so greedy is the exact grid optimum. That is a stronger result than 1-1/e, and the papers add nothing to it.
- **Modeled conversions:** the run is labelled `modeled_threshold_replanning`, its guarantee kind is `none`, and the certificate is withheld. The evidence supports withholding it.

almazaydeh-2026 (VERIFIED) shows that noisy Monte Carlo marginals wreck CELF: CELF++ reached 12.9% of IMM at 1,000 nodes. That does not apply here, because this objective is deterministic. It would apply only if a simulated cascade replaced the objective.

**What qualifies it.**
- **Frozen opponent.** moreno-2021 (VERIFIED-SOFTENED) shows that with two active controllers the unique Nash equilibrium is uniform targeting. The optimizer freezes industry money (`ind = fm.ind0`, set once), so its plan is a best response to a passive opponent and could be exploited if industry re-optimizes. This is qualitative only.
- **Conversions.** borodin-nd (VERIFIED-SOFTENED) shows that weight-proportional competitive LT is neither monotone nor submodular. `pflip = a/(a+b)` is only a structural analogy to that model. However, a conversion re-runs `topic_setup`, where `mAt = max(nA)`, so one conversion can lower another member's pull. Whether that reduces total winnability has not been measured.
- **Timing.** cai-2025 (suggests-extension, weak) says start timing matters in voter dynamics. The optimizer has no schedule, and FEC per-cycle limits make that transfer doubtful.
- **Evaluation design.** cagala-2021 (VERIFIED) is the only bearing with real outcome evaluation. Its targeting rule beat all-gift, no-gift and random-half by about 14% on the warm list (N=2,354, 2014 experiment, 20-fold out-of-sample). On the full 2015 follow-up (N=3,616) the advantage was not significant. Its *design* transfers (benchmark-relative, net of cost, held-out units); its effect sizes do not.

**Bottom line.** The fixed-state exactness claim is sound. Any claim that the optimizer *improves CU outcomes* is UNVERIFIED (DE-062): the `observations` list is empty and no realized-outcome comparison exists.

### Contradictions

- The code cites the bound as 'Golovin-Krause JAIR'11 Lemma 5.3 / A.9'. One bearing says this does not match the text copy it read, which has Lemma 6 Eq.13 and Lemma 37 instead. A second bearing found Lemma 5.3 in the JAIR text (file 6ba342dad525.txt, line 710) but not in the 13-page CoRR version, and could not locate A.9 in either. Net: Lemma 5.3 is correct for the JAIR version; A.9 is unconfirmed.
- Paul expects a learned policy (GCOMB) to replace greedy at scale. arya-2025 (RELINK) argues for learned policies only on large graphs with non-submodular edge choice. This optimizer is neither: it is separable and concave on 533 members. The dashboard's own comment says 'no ML needed at our scale'. The only row favoring RL over rules (zeng) is UNVERIFIED and about drones.
- The joint optimizer's knee is at 20% of peak marginal. The budget frontier's knee is where 4 x gain >= peak, which is 25%. These are two different knee rules in one system, with no stated reason for the difference.

### Gaps

- Nothing tests whether greedy-predicted gains track realized CU-aligned outcomes. The component's own empirical question is unanswered. What would fill it: a benchmark-relative backtest in the style of Cagala. Compare members funded under the optimizer's allocation with members who were comparable but unfunded, or compare against uniform, random and proportional allocations. Score on a held-out cycle with the window and denominator stated, and report results by party (or label them UNSTRATIFIED).
- The effect of the frozen industry opponent is unmeasured. What would fill it: a model-internal sensitivity run in which industry money best-responds (alternating best responses), measuring how much CU winnability the frozen-opponent plan loses.
- The effect of `mAt` renormalisation after a conversion is unmeasured. What would fill it: a read-only check of total winnability before and after each modeled conversion.
- No GCOMB bearing exists, even though Paul cites GCOMB.

### Paul's position vs the evidence

Partly confirmed, partly unsupported.

- **Confirmed:** adaptive greedy plus CELF as the operational layer. In the fixed stance state the built optimizer is better than Paul's framing: it is exact, not 1-1/e.
- **Unsupported:** the expectation that GCOMB or another learned policy will replace greedy at scale. No verified bearing supports it here, and the scale argument does not apply to a separable objective over 533 members. If a learned policy is ever wanted, it would first need to be shown that greedy is infeasible, and the GCOMB paper itself would need to be read.
- **Defensible:** Huneeus & Kim and Pritoni do not argue for pooling allied money. Treating the pot as one PAC's budget (the F2 ruling) holds up on pritoni-2025, which finds the effects of coalitions on influence contradictory.

### Open questions

- Should the code's bound citation be corrected to name the JAIR version (Lemma 5.3) and drop 'A.9' until it is located?
- Is the 20% knee here meant to differ from the frontier's 25% knee?
- Does Paul still want the GCOMB replacement on the roadmap, given that greedy is exact at this scale in the fixed state?
- Should the frozen-opponent assumption be shown on the allocation output, for example as 'best response to fixed industry money'?

### Read first

- [[golovin-2011-adaptive-submodularity-theory]]
- [[cagala-2021-optimal-targeting-fundraising]]
- [[moreno-2021-shadowing-shielding-effective]]
- [[borodin-nd-threshold-models-competitive]]
- [[arya-2025-relink-edge-activation]]

## Stance money gate (F1)

**What it computes.** Members with overall tier Skeptic, Highly Skeptical or Opponent get zero money in every allocator.

**Method.** Applied before the top-45 shortlist. Fails open for members with no matrix row. Before the gate 10 opposed members were funded $54,500; after it, 19 were gated and none funded.

**Code refs.** `fec/moneyball_core.py:713-729`, `fec/engine/optimize.py:44-52`

**Assumptions.**
- Overall stance should gate money; one topic cell should not

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | supports | Hall and Deardorff (2006) treat information as a subsidy to resource-strapped legislators, which explains why lobbyists lobby friendly legislators and questions the persuasion role. Hirsch et al. (2019; FARA contacts, 219 foreign-interest lobbyists, 110th-111th Congress) find any lobbying contact is more likely for lobbyists ideologically aligned with the politician. | The contact evidence concerns foreign-principal lobbyists under FARA, not domestic PAC dollars. It is consistent with funding allies but does not test whether contributions to opposed members change their positions. | weak | VERIFIED: Members whose overall tier is Skeptic, Highly Skeptical or Opponent are blocked as money targets in the allocators (per_topic_flip, optimize; lt_priming and dashboard hold copies). The gate fails open for members with no matrix row. |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | supports | Text PDF p.18 (printed p.17): of 49,418 individuals with 25+ federal contributions (2004-2012), only 167 (0.3 percent) have money-weighted SD above the 0.577 random-giving baseline. Mean money-weighted SD is 0.20 for the sample. Descriptive evidence that active individual donors give on ideology. | Descriptive only: donors are ideological, but the paper does not test whether funding opponents converts them. Window is individual donors 2004-2012; PAC behavior differs (Table 2). | weak | VERIFIED-SOFTENED: STANCE_GATE_BLOCKED = Skeptic, Highly Skeptical, Opponent, tested on overall tier. Members in this set are excluded as money targets. The gate fails open: a member with no matrix row gets tier dash and stays eligible. |
| [[madsen-2017-strategic-advantages-micro]] | 2017 | supports | Contact spent on audiences with an unfavourable view of the source is wasted or counterproductive. The targeted campaign's advantage comes entirely from never contacting those voters. | An analogy only. Campaign contacts with voters are not PAC dollars to legislators, and the paper has no money mechanism. It agrees with Paul's Furnas reading ('cultivate champions') but adds no new evidence about legislator behaviour. | weak | UNVERIFIED: Members whose overall tier is Skeptic, Highly Skeptical or Opponent receive zero money in every allocator, which funds allies instead of trying to convert opponents. The gate fails open for members with no matrix row. |

### Assessment

**Evidence weight: weak. It is consistent with the gate but does not test it.**

There are three bearings, and none tests the component's own question: do contributions to opposed members change their positions?

- **bombardini-2020 (VERIFIED, supports, weak).** It reports the Hall & Deardorff (2006) view of lobbying as a subsidy to allies, which explains why lobbyists lobby friends. It also reports Hirsch et al. (2019): of 219 foreign-interest lobbyists in the 110th-111th Congress, contact was more likely with ideologically aligned legislators. That is FARA contact data, not domestic PAC dollars, so it describes where lobbyists go, not whether money converts anyone.
- **bonica-2013 (VERIFIED-SOFTENED, weak).** Of 49,418 individuals with 25 or more federal contributions (2004-2012), only 167 (0.3%) gave more widely across ideology than random giving would produce. This is descriptive evidence that *individual* donors give on ideology. PACs behave differently (Table 2 of the paper), and it says nothing about conversion.
- **madsen-2017 (UNVERIFIED, analogy).** In an agent model of voters, contact with an unfavorable audience was wasted. This is a lead only.

**Model-internal facts from the component record (no window given).** Before the gate, 10 opposed members were funded $54,500. After it, 19 members were gated and none was funded. The gate **fails open**: a member with no matrix row gets tier '-' and stays eligible. No evidence speaks to whether failing open is safe. It is a design choice that should be visible to users.

**Bottom line.** The gate is a defensible policy prior that agrees with the access and subsidy literature. It is not an empirically validated rule.

### Contradictions

- No bearing contradicts the gate.
- The supporting evidence measures a different thing from the gate. Its strongest support is about lobbyist *contact* (FARA) and *individual-donor* ideology, while the gate allocates *PAC dollars*. Bonica's own Table 2 shows PACs behave differently from individuals.

### Gaps

- Whether money given to opposed members precedes position change, which is the counter-strategy question. What would fill it: a within-member panel of PAC receipts from CU-aligned sources against later CU-relevant positions (cosponsorship or roll calls), with member fixed effects, reported by party. The access-not-votes literature the system already cites (Kalla & Broockman; Fowler, Garro & Spenkuch) should be the prior. Identification is weak without randomization, so the result would be a lead, not a finding.
- The safety of failing open is unmeasured. What would fill it: a census of members with no matrix row, showing how much money the allocators currently send them.

### Paul's position vs the evidence

Paul reads ideological money as cultivating champions rather than recruiting them, citing Furnas et al. 2022, and so funds allies rather than converting opponents. The verified evidence (Hall-Deardorff via Bombardini; Bonica) agrees in direction. But it is about contact and individual donors, and none of it tests conversion. Paul's position is a reasonable prior that the evidence has not yet confirmed or refuted. Madsen adds agreement but no evidence about legislators.

### Open questions

- Should the fail-open behavior (no matrix row means eligible) be flagged on the allocation outputs?
- Does Paul want the counter-strategy question (does money convert opponents?) put on the research agenda, or treated as settled policy?

### Read first

- [[bombardini-2020-empirical-models-lobbying]]
- [[bonica-2013-mapping-ideological-marketplace]]
- [[madsen-2017-strategic-advantages-micro]]

## Per-topic flip and coalition allocators

**What it computes.** Per-topic diagnostic allocations: greedy $1k steps on flip gain; coalition water-fill over the top-45 neutral recruits weighted by marginal spread x (1 + 0.8 bpf).

**Method.** Budget $2M per topic, cap $50k. Joint vs naive additive spread is reported (non-additivity).

**Code refs.** `fec/engine/per_topic_flip.py:1-100`, `fec/engine/per_topic_coalition.py:28-130`, `fec/engine/build_im_allocation.py:31-47`

**Assumptions.**
- Bridge-builders are better seeds
- Topic budgets are diagnostic and not summable

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[trivedi-2020-efficient-influence-maximization]] | 2020 | suggests-extension | Degree-discount (DHICM) discounts neighbours of already chosen seeds (Eq 3.2). The paper reports DHICM above plain degree by 16% at k=30 and 10.4% at k=80 on NetHept, and by 10% at k=50 on NetPhy. Plain degree therefore trails DHICM by less than those percentages, not by them. | The paper's direction is DHICM over degree, so the gap is an extension argument, not a measured gain. The percentages are simulated, depend on k, come from one benchmark, and have no error bars. A sequential or discounted pass is a candidate for owner review only. | moderate | VERIFIED-SOFTENED: per_topic_coalition scores each candidate by isolated marginal spread({n}) minus base (line 115), boosts by a bridge factor with BETA_C=0.8 (lines 30, 117), water-fills the budget (line 119), and reports the joint spread only afterwards (lines 124-125). Verified: no discount for adjacency to other funded seeds. |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | Near-optimality comes from greedy selection that conditions each pick on what has been selected or observed. For deterministic priors the notion reduces to classical submodularity, so Nemhauser et al. (1-1/e) applies (Sec 3.1-3.2 pp.6-8; Thm 5 p.11; Conclusions p.36). | Flip allocator: with fixed-state separable concave pflip, greedy is exact, so the paper adds nothing. Coalition allocator: the singleton-marginal proportional split is not greedy, so no submodular guarantee applies even if spread were submodular; reporting joint spread is the right diagnostic. Submodularity of the bystander-filtered competitive kernel is unverified. | moderate | VERIFIED: Coalition allocator ranks the top-45 neutral candidates by singleton marginals spread({n}) minus base, splits the budget by water-fill with a per-member cap and no sequential conditioning, then reports the joint spread of the funded set. A comment asserts spread is submodular. The flip allocator is an exact heap-based greedy over pflip chunks. |
| [[ma-2022-adversarial-attack-graph]] | 2022 | qualifies | Table 1: both InfMax methods beat Degree, Betweenness and PageRank in all 18 setups. The paper says both proposed methods beat all baselines in all but 3 of 18 setups. Pubmed GAT at 30 percent threshold: GC-RWCS 69.1, InfMax-Unif 64.1, no attack 85.2. In a 4-community SBM the better selectors spread seeds more evenly (qualitative). | The paper's objective is GNN misclassification on citation graphs at a 1 percent node budget. The CU objective is a Monte Carlo competitive cascade on the cosponsor kernel. The structural point transfers: weighting by singleton marginals ignores overlap, so funded seeds can cluster. The code already measures the joint-vs-naive gap. The community-spread finding is qualitative. Weak to moderate. | weak | VERIFIED-SOFTENED: per_topic_coalition ranks neutral candidates by ev (engagement priority times delivery) and keeps the top 45, not by singleton marginal. Singleton marginal spread({n}) minus base is then computed over those 45 and used as the allocation weight, boosted by (1 + 0.8 times bpf). The 2M budget is water-filled with a 50k per-seed cap. The joint spread of the funded set is reported afterwards and does not shape the allocation. |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | The expected-spread function of Kempe et al. is submodular (cited in §1 and §6). Thm 4 extends the Nemhauser greedy guarantee to adaptive greedy on true conditional marginals. | The allocation uses singleton marginals at the base state, not conditional marginals, so no greedy guarantee transfers to the funded set. Reporting joint spread is consistent with submodularity. The Kempe submodularity claim is verified only as cited background; the Kempe result is not re-derived here. | moderate | VERIFIED: per_topic_coalition.py computes singleton marginals mg[n] = spread({n}) - base over the top-45 gated candidates, boosts them by (1 + BETA_C*bpf) with BETA_C=0.8, water-fills the dollar budget, then reports the joint spread of the funded set beside the naive-additive sum, noting that spread is submodular. |
| [[chen-2021-contingency-aware-influence]] | 2021 | context | Within a round, a seed's exact expected marginal contribution depends on which earlier seeds turned out willing, giving O(2^b) cases. The paper assumes the influence function is submodular, stated as usually submodular and citing Kempe et al. 2003 (Eq. 7). | The paper's O(2^b) dependence arises from realized willingness within sequential sub-steps. The coalition allocator averages willingness inside each spread() call, so there is no conditional dependence to ignore at allocation time. The shared point is only that seed non-additivity is real and the joint report exposes it. The surrogate reward needs homogeneous q and Eqs. 9-10, so it does not port to heterogeneous gates. | weak | VERIFIED-SOFTENED: per_topic_coalition.py gates each lobbied seed inside each spread() call (lines 102-110). Individual marginals spread({n}) minus base (line 115) feed a water-fill (line 119). The funded set's joint spread is reported (lines 124-125) because summing marginals overstates the joint gain. The allocator is one-shot, with no rounds and no observed willingness. |
| [[cai-2025-competitive-influence-maximization]] | 2025 | qualifies | Under node-specific optimization on N=100 networks with mean degree 6 (Sec 4.3.2), the budget ratio sets the degree pattern. With small budget ratios, control gains fall and start times rise with degree, so low-degree nodes are targeted. With large ratios the pattern reverses (Fig 6b-c p.18; Appendix D Fig 10 p.26). | bpf is a cross-party share, not degree, so the paper's degree result maps only indirectly. The paper's result rests on N=100 graphs with mean degree 6. Measuring the Melange cosponsor graph's degree exponent and testing whether greedy seeds move toward low-degree members at small budgets would be new analysis and stays UNVERIFIED. | weak | VERIFIED: The coalition water-fill boosts each candidate's marginal spread by (1 + 0.8 bpf), where bpf is a bipartisan share read from bipartisan.json divided by 100, so it favours bipartisan bridges. The ranking does not depend on budget size, although the CAP-clipped water-fill decides how far down the list money goes. |
| [[zareie-2023-influence-maximization-social]] | 2023 | suggests-extension | Topic-aware IM (Barbieri, Bonchi and Manco 2013) represents each user's interest as a continuous topic vector. The survey says this captures preferences more realistically than binary target or label methods, but determining topic vectors needs a good record of historical activity, which may not always be available. The survey reports no accuracy comparison of any kind. | Transfers only if per-category cosponsorship history is dense enough to estimate topic-specific edge weights. A topic-conditioned kernel would change ranks and engine outputs, so change-control applies. The survey gives no evidence that topic conditioning beats topic-blind IM, so any benefit is conjectural. The recorded crypto p_max of 0.23 was not checked. | weak | VERIFIED-SOFTENED: per_topic_coalition.py:37 builds one topic-independent kernel with MBC.build_P from the CU cosponsor graph. Topics differ in target weights and industry money, not in edge probabilities. Kernel sharing was verified for per_topic_coalition only; the per_topic_flip and flip_model paths were not checked. |
| [[bharathi-nd-competitive-influence-maximization]] |  | qualifies | Reach is submodular in a player's own seeds, so summed single-seed marginals overstate the joint gain; the (1-1/e) guarantee belongs to greedy, not to individual marginals (Lemma 1, Theorem 1). | Submodularity implies the joint report is at most the naive additive sum, so the joint report is supported as inference. The paper gives no guarantee for a proportional water-fill. The bystander set excludes funded seeds, so the spread is not Lemma 1's fixed-opponent function. The figures are one allocation run per topic at MC 20. | moderate | VERIFIED-SOFTENED: The allocator computes single-seed marginals against the base, multiplies each by (1 + 0.8 times bpf), water-fills 2,000,000 dollars with a 50,000 per-member cap in proportion to those boosted values, runs no greedy, and reports the joint spread of the funded set. The code comment cites submodularity for that report. |
| [[chen-2023-complex-contagion-influence]] |  | suggests-extension | Credit sparseness in an 8-node toy (K=3, T=4): only 17 of 70 four-seed sets (24.3 percent) are effective, and the seed sequence 1,2,3,5 earns per-step rewards 1,1,1,2, so a seed's value shows only alongside specific other seeds. | A joint gain above naive-additive on a topic would show complementarity that solo ranking cannot see. Under pure IC the joint should stay at or below naive-additive, so the printout is a real check only if threshold effects exist. The 45-candidate cut uses engagement rather than marginal value, so a seed valuable only in combination can be dropped before any marginal is computed. | moderate | VERIFIED-SOFTENED: per_topic_coalition.py takes the top 45 neutral members ranked first by a priority engagement score ev (line 114), and only then computes each candidate's solo marginal over the base spread (line 115). It keeps candidates with positive marginal (line 116), boosts them by (1 + 0.8 bpf) (line 117) and water-fills the budget. The funded set's joint spread is one extra MC evaluation, printed beside the naive-additive sum. The code comment asserts spread is submodular. |

### Assessment

**Evidence weight: moderate on the theory, and mostly qualifying. No empirical evidence on sub-additivity.**

**Flip allocator.** This is an exact heap-based greedy over `pflip` chunks. Under the fixed-state separable, concave objective, greedy is exact, and golovin-2011 (VERIFIED) adds nothing to that.

**Coalition allocator.** This is *not* greedy. It ranks the top-45 neutral candidates by solo marginal, `spread({n}) - base`, boosts each by `(1 + 0.8 bpf)`, and water-fills the $2M per topic in proportion to the boosted values, with a $50k cap. Four bearings agree that no guarantee transfers:
- golovin-2011 approach and theory versions (both VERIFIED)
- bharathi-nd (VERIFIED-SOFTENED): Lemma 1 covers a fixed opponent set, but here the bystander set excludes funded seeds
- chen-2021 (VERIFIED-SOFTENED)

The same bearings support the **joint-vs-naive-additive report** as the right diagnostic. If spread is submodular, the joint value should be at or below the naive sum.

**Extensions and cautions.**
- **Clustering.** trivedi-2020 (degree-discount) and ma-2022 both point to the same risk: weighting by solo marginals ignores overlap, so funded seeds can cluster together. These are extension arguments, not measured gains on this kernel.
- **Complementarity.** chen-nd (complex contagion, VERIFIED-SOFTENED) says that if real cosponsor joining is threshold-like, a joint value *above* the naive sum would show complementarity. It also warns that the engagement-ranked top-45 cut, which happens before any marginal is computed, can drop seeds that are valuable only in combination.
- **Degree.** cai-2025 (VERIFIED, weak) finds that low-degree nodes are the better targets at small budget ratios. But `bpf` is a cross-party share, not degree, so this maps only indirectly.
- **Topic kernels.** zareie-2023 floats topic-conditioned kernels without any accuracy evidence. The kernel is currently topic-independent (verified for `per_topic_coalition` only).

**Bottom line.** Topic budgets are correctly treated as diagnostic and not summable. The code comment saying spread is submodular is unverified for this competitive, bystander-filtered kernel.

### Contradictions

- Paul places these allocators in the 'static and submodular tier', and the code comment says spread is submodular. The bearings find that submodularity is not shown for the competitive kernel with bystander exclusion (bharathi-nd, golovin-2011). They also find that the coalition water-fill is not greedy, so even true submodularity would give no guarantee for it.
- The assumption that 'bridge-builders are better seeds' (BETA_C = 0.8) has no supporting bearing. cai-2025's degree result, the nearest evidence, depends on budget size, whereas the `bpf` boost is the same at every budget.

### Gaps

- Whether coalition spread from multiple seeds is sub-additive in *observed* cosponsorship data. No bearing measures this. What would fill it: an event-history study of when members join CU-relevant bills, as a function of how many of their cosponsor partners have already joined. Compare single-exposure (cascade-like) and threshold (complex-contagion) specifications, control for homophily (the system already cites Shalizi & Thomas on homophily vs contagion confounding), and report by party.
- There is no basis for BETA_C = 0.8. What would fill it: a model-internal sensitivity sweep of BETA_C, reporting rank movement, labelled as model output.
- Water-fill and conditional greedy have not been compared on the same kernel. What would fill it: a read-only run of both under common random numbers per topic, with the joint spread reported for each.

### Paul's position vs the evidence

Paul places these in the static and submodular tier of his three-tier architecture. For the flip allocator, the evidence agrees (it is static and exact). For the coalition allocator, it does not: the water-fill is not a submodular-greedy method, and submodularity of the competitive kernel is unverified. 'Static, heuristic' is the accurate label for the coalition allocator. Kempe 2003 is cited only as background and is not re-derived.

### Open questions

- Should the coalition allocator be relabelled 'heuristic', and the code's submodularity comment softened?
- Does any topic's joint spread exceed its naive-additive sum in existing runs? That would indicate threshold effects in the kernel.
- Should the top-45 cut rank by marginal value instead of engagement? That would change ranks, so it falls under change-control.

### Read first

- [[bharathi-nd-competitive-influence-maximization]]
- [[golovin-2011-adaptive-submodularity-theory]]
- [[chen-2023-complex-contagion-influence]]
- [[trivedi-2020-efficient-influence-maximization]]
- [[ma-2022-adversarial-attack-graph]]

## Budget frontier with knee and electoral modes

**What it computes.** Spread vs cumulative seeds per topic; knee where 4 x gain >= peak; contest factor REPFAC 0.6 x lev.

**Method.** Lazy greedy up to 60 seeds under CRN.

**Code refs.** `fec/engine/budget_frontier.py:1-188`, `fec/engine/mc_stability_check.py:1-44`

**Assumptions.**
- Diminishing marginal returns to seeds

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kumar-2022-influence-maximization-social]] | 2022 | qualifies | Introduction says greedy IM methods are generally more accurate with a theoretical performance guarantee but computationally intensive, while centrality methods are cheap but network-specific. SGNN replaces the greedy step with a learned per-node influence regressor and top-k selection. No SGNN numbers appear in the accessible preview. | The submodular guarantee is not shown for this two-colour engine with leverage factors, so the claim that the greedy carries a guarantee here should not be made. A learned surrogate would trade away the marginal accounting for speed. Speed matters only if per-candidate MC greedy is infeasible, and the 533-member, 60-seed setting is not shown to be infeasible. | moderate | VERIFIED-SOFTENED: budget_frontier.py runs a lazy greedy: a heap of stale marginals, where each candidate marginal is spread(S+c) minus spread(S), clipped at zero and multiplied by a per-candidate factor fac. MC defaults to 20 edge-keyed CRN cascades. It stops at MAXSEEDS (default 60) or at zero marginal and reports a knee. |
| [[trivedi-2020-efficient-influence-maximization]] | 2020 | supports | Under ICM with p=0.01 on two co-authorship graphs, DHICM is 12.5% (NetHept k=10) and 7.9% (NetHept k=30) below CELF, and 2.5% below CELF at k=30 on NetPhy. The paper says greedy is slow on medium-sized graphs (more than 1500 nodes) because of its high time complexity. | The gap was measured at p=0.01 on two co-authorship graphs of 15k to 37k nodes, with no MC sample size or variance reported. It ranks algorithms under ICM only and does not validate the frontier's spread values, which are model outputs. The cost argument holds because the candidate pool is about 45, not because of a node-count threshold in the paper. | moderate | VERIFIED-SOFTENED: budget_frontier runs exact lazy greedy: candidates are the stance-gated top-45 neutral shortlist (line 137), marginals are re-evaluated when popped from a heap (lines 159-170), and MAXSEEDS defaults to 60 and MC to 20 (line 7). Verified in code. |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | Greedy guarantees assume exact or alpha-approximate marginal evaluation; sampled marginals give only additive error (Sec 5 p.11, bound with epsilon). The adaptive IC result (Thm 19 p.21) covers a single cascade with full-adoption feedback; the myopic-feedback version was retracted with a 3-node counterexample (Sec 8.2.1 p.23). | No Golovin-Krause guarantee carries over as built. Marginals are sampled, selection uses fac-weighted marginals rather than the reported spread, and the kernel is competitive. The zero clamp signals that sampled spread is not monotone. The frontier is a non-adaptive heuristic. | moderate | VERIFIED-SOFTENED: Lazy greedy picks up to 60 seeds (MAXSEEDS default 60). Each marginal is a 20-draw CRN Monte Carlo estimate (MC=20) on a two-colour first-to-reach cascade with money-mobilized bystanders. Marginals are clamped at zero and multiplied by a per-candidate factor fac: REPFAC times lev for contest candidates, 1.0 for defend candidates, and lev for all others. The knee is read from the raw path. |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | qualifies | With additive marginal error eps, greedy gets at least (1 - e^{-l/k}) OPT - l*eps. Thm 6: adaptive seeding under the single-colour independent cascade with any monotone submodular reward is adaptive submodular, giving 1-1/e under myopic or full-adoption feedback. | Thm 6 covers only the single-colour IC. The two-colour race with anti bystanders and the per-candidate fac weights place the objective outside Thm 6, so no 1-1/e transfers. The eps term is real: Monte Carlo noise over up to 60 seeds enters as -l*eps, and the clamp exists because of observed dips. eps is not bounded anywhere in the code. | moderate | VERIFIED: budget_frontier.py runs lazy greedy up to MAXSEEDS=60 with MC=20 replicates and common random numbers. The marginal is max(0, sp - cur)*fac[c]. The seed gate is a CRN draw applied before the cascade. The displayed curve is clamped non-decreasing (prev=max(prev,cur)) and the raw spread is stored alongside it. |
| [[chen-2021-contingency-aware-influence]] | 2021 | supports | The authors attribute RL4IM's slight edge over greedy CHANGE to proactively modeling unwillingness in the reward, versus CHANGE reacting to realized willingness. The edge is called slight and gets no numbers. At T=1 (single round, T x B = 8), RL4IM and CHANGE are the two best methods. The roughly-twice-Random result is stated for the q sweep (Q3), not for T=1. All results are synthetic graphs under IC with p=0.1. | Supports only the design choice of putting take-up uncertainty inside the expected-spread objective. The paper models one colour; the system has a two-colour competitive cascade. The paper's synthetic graphs (about 200 nodes) are comparable in scale to the 533-member graph, but there are no political data. The slight edge is qualitative. | weak | VERIFIED-SOFTENED: spread() in budget_frontier.py draws each lobbied seed's activation against gate(n,cs) on a dedicated CRN stream (key offset 2^62, lines 127-128). A failed activation stays an opponent (lines 129-131). MAXSEEDS defaults to 60 (line 7). Lazy-greedy mechanics were not checked. |
| [[borodin-nd-threshold-models-competitive]] |  | qualifies | Most natural competitive extensions lose submodularity. Monotonicity can survive while submodularity fails (Theorems 3-6). The greedy (1-1/e) guarantee is shown only for the OR model with monotone submodular decision functions (Theorems 9-11). Section 7 leaves other models open. | Applies to electoral mode (contest and defend seeds). This paper gives no submodularity result for the electoral objective. Any coalition-mode greedy guarantee comes from the competitive IC literature cited to Bharathi et al. (Borodin ref 1), not from this paper, so coalition-only mode is not covered here. Separating MC noise from structure needs a read-only check of raw marginals under common random numbers. | moderate | VERIFIED-SOFTENED: budget_frontier.py runs a CELF-style lazy greedy on a heap. spread() applies the seed gate (line 128) and removes activated contest seeds from the anti set (line 131). marg() multiplies the raw gain by fac, which is REPFAC times leverage for contest seeds (line 143, applied at 156), and floors at zero. audit_frontier.py 27-35 counts marginal inversions on clamped spread and passes up to 35 percent of marginals as MC noise (tolerance 0.35 times the marginal count). |
| [[almazaydeh-2026-heuristic-centrality-methods]] | 2026 | qualifies | At 1000 nodes CELF++ with 50 MC sims per marginal estimate reached 122.95 +/- 24.37 vs IMM 954.90 +/- 1.27, i.e. 12.9 percent of IMM (t = -107.78, p < 0.001). Its CV rose to 71.5 percent at 500 nodes. The authors attribute the gap to MC budget and Python implementation but do not test that. The abstract gives 11.2 percent, which conflicts with the body. | The paper's failure mode is independent noise across candidate estimates. CRN removes that for pairwise marginal comparison, so the CRN design is supported in principle. The paper uses undirected uniform-p IC; Melange runs competitive two-colour IC with seed gating on a 533-node kernel. The paper's numbers show the mechanism only. | strong | VERIFIED-SOFTENED: budget_frontier.py runs lazy greedy at MC=20 (default at line 7, MAXSEEDS=60 at line 7) with edge-keyed splitmix CRN in cascade() (lines 83-94), so competing candidates share draws. The brain records MEA-7 PASS (100 percent Jaccard overlap at MC=100; 98 percent across two RNG seeds) at STATE.md lines 748-749, in a block dated 2026-08-18. |
| [[almazaydeh-2026-heuristic-centrality-methods]] | 2026 | suggests-extension | At 1000 nodes PageRank reaches 921.78 against IMM 954.90 (96.5 percent by Table 2 arithmetic; the paper states 96.3 percent in the abstract and discussion) in 0.4 s. Betweenness at 921.80 is indistinguishable (t = -0.01, p = 0.99). Degree reaches about 85 percent. | Usable only as an ablation diagnostic, labelled UNSTRATIFIED, with window, denominator (533 members) and topic named. The paper's hub-favouring importance sampling tilts results toward centrality, and its graphs are undirected uniform-p, so a near-match on spread would not justify replacing greedy. Spread is modelled cascade reach, not access or influence (D-025). | moderate | VERIFIED-SOFTENED: The frontier and the coalition allocator pre-cut the candidate pool to the top 45 neutral members ranked by priority-engagement times delivery multiplier: budget_frontier.py line 137 and per_topic_coalition.py line 114. The code does not score a degree, PageRank or Katz-Bonacich seed set under the same CRN cascade. The claim that no such comparison exists in the brain was not verified by a repo-wide search in this pass. |
| [[bharathi-nd-competitive-influence-maximization]] |  | qualifies | Lemma 1 gives monotone submodularity of a player's expected reach in its own seeds, holding the other players' seed sets fixed. Greedy over that function gives a (1-1/e) approximation to the best response (Theorem 1). | The raw-spread guarantee applies only with a fixed opponent set and A-seed additions. Independent per-seed gating is an expectation over independent masks and keeps submodularity. The fac-weighted ranking is a heuristic, not greedy on spread. Contest removal and the S-dependent bystander exclusion fall outside Lemma 1. MC of 20 adds noise. The bound holds only with fac equal to 1, which is the LEV-off case. | moderate | VERIFIED-SOFTENED: Lazy greedy with MAXSEEDS 60 ranks each candidate by max(0, spread gain) times fac. fac is leverage, or the competitiveness multiplier if the LEV source is set to it, and REPFAC times leverage for contest plays. Seeds are gated at random. Contest plays remove B seeds. The bystander set excludes any node already in the A seed set, so the opponent set changes with S, which Lemma 1 does not cover. |
| [[chen-2023-complex-contagion-influence]] |  | qualifies | Complex-contagion IM is non-submodular (Thm 1) and NP-hard (Thm 2). Greedy can be arbitrarily bad: normalised influence 0.0396 for Greedy vs 0.5347 for RL4CCIM on India (202 nodes), and 0.0293 vs 0.5585 on Exhibition. Deterministic setting p0=1, p1=0, budget T=8 (T=12 on Exhibition). | The zero-marginal failure bites only if real cosponsor joining is threshold-like, so that single-seed marginals are zero until K partners are joined. Under the system's per-edge IC kernel the stop rule and knee are internally coherent. The paper's numbers are simulated cascades on non-legislative networks, so they do not transfer as estimates. | moderate | VERIFIED: budget_frontier.py runs a lazy-greedy heap over a candidate pool with MAXSEEDS default 60 (line 7). Spread comes from an edge-keyed IC cascade over the P kernel (lines 83-94). The loop stops when the best marginal is zero (inside lines 161-172). The knee is the last seed whose gain is at least a quarter of the peak gain on the spread path (frontier_knee, lines 49-58, called at line 172). Both the stop rule and the knee presume diminishing returns. |

### Assessment

**Evidence weight: moderate-to-strong, and mostly qualifying. The frontier is a non-adaptive heuristic with no transferable guarantee.**

**No guarantee transfers as built.** Five bearings converge on this:
- **golovin-2011-adaptive-submodularity-theory (VERIFIED).** Theorem 6 covers only single-colour independent cascades. The marginal-error term `-l*eps` is real, and `eps` is not bounded anywhere in the code.
- **golovin-2011-adaptive-submodularity-theory (VERIFIED-SOFTENED).** The myopic-feedback version of the adaptive IC result was retracted.
- **bharathi-nd.** The bound holds only when `fac = 1` (the LEV-off case) and the opponent set is fixed. Contest removals and the bystander exclusion, which changes with S, fall outside it.
- **borodin-nd.** It gives no submodularity result for the electoral contest and defend mode.
- **kumar-2022.** Speed is not a reason to replace greedy here: the candidate pool is about 45.

**Noise.** almazaydeh-2026 (VERIFIED-SOFTENED, strong) shows that independent Monte Carlo noise corrupts CELF: CELF++ reached 12.9% of IMM at 1,000 nodes, with 50 Monte Carlo simulations per marginal. The frontier's edge-keyed common random numbers remove that failure in principle for pairwise comparisons. The brain records MEA-7 PASS: 100% Jaccard overlap at MC=100 and 98% across two RNG seeds (block dated 2026-08-18). The frontier still runs at MC=20, though. `audit_frontier.py` treats up to 35% of marginals inverting as noise, which is a loose tolerance. And the zero clamp exists because dips were actually observed.

**Shape.** chen-nd (VERIFIED) shows that complex-contagion influence maximization is non-submodular, and that greedy can be arbitrarily bad: 0.0396 vs 0.5347 on a 202-node graph. The zero-marginal stop rule and the knee both assume diminishing returns. That holds under the system's per-edge cascade kernel, but it is unknown for real cosponsor joining.

**Lesser bearings.** chen-2021 supports putting take-up uncertainty (the seed gate) inside the objective, though only in single-colour synthetic tests. trivedi-2020 supports exact CELF over cheaper heuristics on cost grounds.

**Suggested extension.** almazaydeh-2026 suggests a centrality ablation (PageRank reached 96.5% of IMM). It should be used only as a diagnostic labelled UNSTRATIFIED, with the window, the denominator (533 members) and the topic stated. Spread is modeled cascade reach, not access or influence (D-025).

### Contradictions

- The assumption 'diminishing marginal returns to seeds' is built into the stop rule and the knee. Yet the code has to clamp the curve to non-decreasing and tolerate up to 35% marginal inversions. Inside the model, sampled spread is not monotone at MC=20. Whether that is noise or structure has not been separated.
- The frontier's knee (4 x gain >= peak, i.e. 25%) differs from the joint optimizer's (20% of peak marginal).
- The code cites Bharathi et al. for coalition submodularity. That result needs a fixed opponent and `fac = 1`, neither of which holds by default.

### Gaps

- Where the real-world marginal return to additional lobbied cosponsors flattens for CU bills. No bearing addresses this. What would fill it: an observational study of cosponsor accumulation on CU bills against the number of early or recruited cosponsors. Note that LDA cannot identify which members were lobbied: LD-2 filings do not name members, and LDA is an identity source, not an edge or weight source (D-021). 'Lobbied' therefore needs a different source, such as staff-contact or meeting records.
- `eps` is not bounded. What would fill it: a read-only check of raw (unclamped) marginals at MC=20 against MC=100 under common random numbers, per topic, giving an empirical noise bound and a principled inversion tolerance to replace 0.35.
- Electoral-mode (contest and defend) objectives have no theory. Only a simulation study of monotonicity and submodularity violations in this specific kernel would fill this.

### Paul's position vs the evidence

Paul maps GCOMB's budget framing onto a political-capital budget in an orienteering formulation. No bearing addresses orienteering or political-capital budgets, and the formulation is not built, so this is an unevidenced design direction. The Golovin-Krause citation is used for framing, but the bearings show its guarantees do not carry over to this two-colour, fac-weighted, sampled objective. The frontier should be described as a heuristic curve of modeled spread.

### Open questions

- Should MC be raised from 20 to the 100 at which MEA-7 passed, or should the 35% inversion tolerance be justified with a measured noise bound?
- Should the frontier output and the code carry an explicit 'heuristic, no approximation guarantee' label?
- Does Paul still intend to build the orienteering / political-capital formulation, and with what budget unit?

### Read first

- [[golovin-2011-adaptive-submodularity-theory]]
- [[almazaydeh-2026-heuristic-centrality-methods]]
- [[bharathi-nd-competitive-influence-maximization]]
- [[borodin-nd-threshold-models-competitive]]
- [[chen-2023-complex-contagion-influence]]

## Workbook allocator (top-133 from fixed pot)

**What it computes.** Funds the top 133 of 533 members from a flat $2.0M in proportion to score (uncapped; max about $25.8k).

**Method.** A capped variant ($10k cap, top 250) exists only in a backup and was not applied.

**Code refs.** `Blended Model col U`, `fec/apply_alloc_cap.py (not applied)`

**Assumptions.**
- Score ordering maps to funding priority

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[cagala-2021-optimal-targeting-fundraising]] | 2021 | qualifies | Treating everyone as a gift recipient is net-negative or near zero: all-gift minus no-gift is +0.05 to +0.08 euro for the warm list (not significant) and -0.97 euro per donor for the cold list (significant) (Table 1). Targeted warm-list donors differ from non-targeted donors only modestly in prior giving (8-year total 130.98 vs 123.39 euro; 787 targeted vs 1,567 non-targeted; Table 4, Panel A). The paper cautions that observed characteristics are correlates of response, not the channels through which the gift works (Section 5.4). | Conceptual only. The magnitudes come from donors, not legislators. The transferable point is to compare any proportional allocator against uniform and random benchmarks. The paper does not show that the allocator is wrong, and the system side is unverified. | weak | VERIFIED-SOFTENED: The workbook sheet Blended Model has an Allocation column (column U) beside Rank (column T) and Score (column S). In the copy read here that column has no cached values, so the claim that the top 133 of 533 members are funded from a flat 2.0 million dollar pot was NOT reproduced. The cited producer fec/apply_alloc_cap.py does not exist in the repo. |

### Assessment

**Evidence weight: essentially none. Even the system-side description is unverified.**

There is one bearing: cagala-2021 (VERIFIED-SOFTENED, weak). It shows that untargeted allocation can be net-negative or near zero. For the cold list, treating everyone was -0.97 euro per donor and significant. For the warm list, all-gift minus no-gift was +0.05 to +0.08 euro and not significant (Table 1). Its transferable lesson is to compare any proportional allocator against uniform and random benchmarks. The magnitudes come from church donors and do not transfer.

**The component's own claims could not be reproduced.**
- Column U of Blended Model had no cached values in the copy read, so the claim that the top 133 of 533 members are funded from a flat $2.0M was not reproduced.
- The cited capped variant, `fec/apply_alloc_cap.py`, does not exist in the repo.

**Inside the model.** Greedy on a separable concave objective is the exact grid optimum (golovin-2011), so proportional-to-score allocation is weakly dominated *in predicted winnability* by construction. But this holds only if both allocations are scored under the same winnability objective. The workbook ranks by Blended Model score, which is a different quantity, so the comparison is not automatic.

### Contradictions

- The component record says the allocator is uncapped, with a maximum of about $25.8k per member. The joint optimizer's PMAX of $10k is described as the PAC's per-candidate, per-cycle limit. If the workbook amounts are read as one PAC's contributions, the maximum exceeds the limit the rest of the system enforces. The $25.8k figure itself was not reproduced.
- The capped variant is described as existing 'only in a backup', but no such file is in the repo.

### Gaps

- Whether proportional-to-score allocation is dominated by the greedy allocation in predicted and realized winnability. What would fill it, in two parts: (1) a model-internal comparison scoring both allocations under the same winnability function, with the window and the 533-member denominator stated; (2) a realized-outcome comparison against uniform and random benchmarks in the style of Cagala, on a held-out cycle.
- No literature addresses proportional-to-score allocation of PAC money at all.

### Paul's position vs the evidence

Paul treats Huneeus & Kim as a basis for setting fundraising goals. No bearing addresses Huneeus & Kim or proportional allocation, so his position has no evidence either way. The one bearing (Cagala) argues that this allocator should be benchmarked before its output is trusted.

### Open questions

- Where does the workbook allocation in column U come from now, given that the producer script is missing and the column has no cached values?
- Should the workbook allocator be capped at the $10k per-candidate limit for consistency with PMAX, or retired in favor of the joint optimizer's output? This is Paul's ruling.

### Read first

- [[cagala-2021-optimal-targeting-fundraising]]
- [[golovin-2011-adaptive-submodularity-theory]]
- [[moreno-2021-shadowing-shielding-effective]]

## Break-even CU spend and ROI floor

**What it computes.** The CU dollars that restore baseline flip odds against competitor spend (closed form, 'cannot offset' when q >= 1), and member ROI floor values.

**Method.** Solves a(CU)/(a(CU) + b(comp)) = pfbase with the saturating money term; ROI floor parity is checked by roi_verify.py.

**Code refs.** `fec/templates/make_dashboard_html_00.tpl (break-even)`, `verify_finish/roi_verify.py`

**Assumptions.**
- Competitor and CU money enter a share-type ratio

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | qualifies | Kang's model implies an average policy value of more than $500m, and the Farm Bill comparison gives a naive return of 600,000% ($22bn in 2000 subsidies vs under $3.7m in agribusiness PAC contributions that cycle). The review says excessive returns are common in this literature. Adding an electoral-vote channel (Bombardini and Trebbi 2011: $200-$400 per vote) lowers the Farm Bill figure to $13.7 per dollar, which the authors call still high but more reasonable; B&T's simulations give average returns of 10%-20% per year. The conclusion notes some structural models display symptoms of misspecification. | This is a caution about any ROI read off contribution-to-outcome ratios. It applies whenever dashboard ROI floors are presented as returns. It does not invalidate the break-even algebra, which is internal to the model. Any ROI shown should carry an UNVERIFIED or model-output label. | moderate | VERIFIED-SOFTENED: Saturating share-type money term in shift_lean (money_response.py) that break-even computations share. The dashboard template exists at fec/surfaces/templates/make_dashboard_html_00.tpl; no roi_verify.py was found in the repo, so the claim about ROI floors checked by roi_verify.py is unconfirmed. |
| [[bickmore-2025-corporate-lobbying-political]] | 2025 | qualifies | Lobby on ShareAssets 0.0743 with full controls; PAC on ShareAssets 0.1100 (election years, N = 62,198). Abramoff DiD Treatment x Post on ShareAssets with full controls is -0.0265 (SE 0.0096); on ShareMcap it is -0.0336 (SE 0.0099). Post window 2006Q1-2009Q1; pre-period 2003Q1-2005Q4 per the Table 8 description. | The paper's return is a firm-level industry-share outcome from Compustat matched to CRP by company name, 1998-2016. It does not measure votes, cosponsorship or flips, so it cannot calibrate WM, HALFF or LOSSFRAME. It supports only a framing claim that political spending associates with firm share gains, reported in share points with the unit stated. | moderate | VERIFIED-SOFTENED: The dashboard head-to-head block computes CU versus industry flip odds using WM, HALFF, the competitiveness multiplier and receipts, then solves for break-even CU dollars. When q is at least 1 the displayed text reads that CU money alone cannot fully offset the competitor spend. The rel's quoted message 'cannot offset' is not the literal wording. |
| [[alexander-2009-lobbying-analysis-under]] |  | qualifies | Overall return on lobbying 220 = (46,157.5 + 15,897.0) / 282.7, i.e. about 62.05bn estimated savings on 282.7M lobbying across 93 lobbying repatriators (FY2004-2006). Coalition 257, non-coalition 154, per-company mean 869, per-company median 172.2, lobbying over 1M subsample (48 firms) 243. | No numeric transfer. The paper numerator is a firm tax benefit from a one-time provision and the denominator is LDA spend (upper bound). Melange numerator is modeled member movement and denominator is FEC PAC money. Any quote needs the window FY2004-2006, the 93-firm denominator, and the median 172 next to the mean 869. The bearing describes the PAC side as capped at 10k; the code uses 10000 as a marginal increment, so describe it as a per-10k increment. | moderate | VERIFIED-SOFTENED: desk_roi in fec/surfaces/make_member_desk.py returns expected weighted flips gained from the next 10k increment (CHUNK) of CU money on a member. The dashboard head-to-head block solves for the CU money that offsets competitor spend. Neither is a firm return on lobbying dollars. |
| [[cai-2025-competitive-influence-maximization]] | 2025 | qualifies | Two settings are mixed here. The optimal start for Congress votes at T=16 falls from 14.730 at budget ratio 0.1 to 10.476 at ratio 10 (Table 2, constant-opponent setting, fixed opponent starting at 0). In the two-controller Nash game the controller with the larger budget moves first, and the second mover can only buffer the first mover's gain, never exceed its vote share (Sec 4.3.4, stage 3, p.21-22). Fig 9b shows per-node resource per unit time rising with the budget advantage. | The paper's statement that a disadvantaged controller can offset but not overtake agrees in spirit with the cannot-offset message. The claim that a disadvantaged side does better by delaying has no counterpart in a static break-even, and it rests on the paper's abstract and simulations only. Transfer would need a model of within-cycle spending order, which the system lacks. | weak | VERIFIED-SOFTENED: The head-to-head panel solves CU share against competitor share in closed form for the baseline odds. It reports that CU money alone cannot fully offset when the required money ratio q is at least 1. The solve uses the CU-to-competitor money ratio at one instant, with no sequencing. |
| [[goff-nd-cross-industry-finance]] |  | qualifies | Crypto outspends key allies by factors of 100, so credit unions would have no chance in a monetary fight with the industry; the supporting graph is a missing placeholder. | The factor of 100 has no source, window or denominator, so the system cannot size the disadvantage. The bounded term shows only that the form caps the monetary pull, not that money decides outcomes. | weak | VERIFIED-SOFTENED: The head-to-head pull uses a saturating term (x+1) over (x+1) plus HALFF times rcp, weighted by WM and added to a coalition term WP (make_dashboard_html_00.tpl lines 1096-1105). The dashboard prints the cannot fully offset message only when q is at least 1. The functional form is an assumption and is not validated against outcomes. |

### Assessment

**Evidence weight: moderate as caution. None calibrates the functional form.**

All five bearings are VERIFIED-SOFTENED and all *qualify*.

- **bombardini-2020 (moderate).** This is the key one. Very high naive lobbying returns are *common* in the literature. Example: Farm Bill subsidies of $22bn in 2000 against under $3.7m in agribusiness PAC contributions that cycle, a naive return of about 600,000%. Adding an electoral-vote channel brings it down to $13.7 per dollar. The authors read such figures as symptoms of misspecification. Any ROI floor shown on the dashboard should be labelled a model output.
- **alexander-nd.** Overall return 220x on $282.7M of LDA spend across 93 repatriating firms, FY2004-2006. The per-company mean is 869x and the median 172x. The numerator is a one-time tax benefit, so nothing transfers numerically to member-level flips per $10k increment (`desk_roi`).
- **bickmore-2025.** A firm-level market-share outcome from Compustat matched to CRP, 1998-2016 (election-year N = 62,198). It cannot calibrate WM, HALFF or LOSSFRAME.
- **cai-2025 (weak).** A disadvantaged controller can offset but not overtake, which agrees in spirit with the 'cannot fully offset' message. Its timing result has no counterpart in a static break-even.
- **goff-nd (Paul's own cross-industry report).** It asserts that crypto outspends allies 'by factors of 100'. The figure has no source, window or denominator, and the supporting graph is a missing placeholder. It fails the rule that every figure names its window and denominator.

**System side.** The head-to-head block solves `a(CU)/(a(CU)+b(comp)) = pfbase` using the saturating term. When `q >= 1` it prints that CU money alone cannot fully offset the competitor spend (the actual wording is not literally 'cannot offset'). The share-ratio form is an assumption that has never been validated against outcomes. `roi_verify.py` was not found in the repo, so the claim that ROI-floor parity is checked is unconfirmed.

### Contradictions

- Paul calls the Alexander figure an outlier. Bombardini says excessive naive returns are common in this literature, so Alexander is typical of a suspect class rather than an outlier. The real issue is the one-time tax provision as numerator, and a mean of 869x against a median of 172x.
- The component says ROI-floor parity is checked by `verify_finish/roi_verify.py`, but no such file was found in the repo.
- Paul's own cross-industry report gives 'factors of 100' with no source, window or denominator, which conflicts with the project's figure-quoting rule.

### Gaps

- Whether member positions respond to the *ratio* of CU money to opposing-industry money rather than to its *level*. No bearing tests this. What would fill it: a member-by-cycle panel of CU and opposing-industry PAC receipts against CU-relevant positions, comparing share-ratio and level specifications with member fixed effects. Stratify by party (a pooled coefficient across that fault line is quoted only with strata or labelled UNSTRATIFIED), and treat the result as associational.
- No calibration exists for the saturating term's parameters (WM, HALFF) against member-level outcomes.
- Kim J. et al. 2025 and Huneeus & Kim, which Paul cites, have no bearing rows here.

### Paul's position vs the evidence

Paul frames lobbying as ROI and wants competing-industry conflict included. The head-to-head block already includes competitor spend, but its share-ratio form is unvalidated. The evidence warns that ROI framing is prone to inflated, misspecified figures (Bombardini), and that the outcome measures in Alexander and Bickmore are not legislative. Paul's instinct to discount Alexander is right, though the reason is that its numerator cannot be compared, not that the figure is unusual. His own 'factor of 100' claim needs a source before the system relies on it.

### Open questions

- Does `roi_verify.py` exist somewhere outside the repo, or should the parity claim be withdrawn?
- What is the source, window and denominator for the 'factors of 100' crypto-vs-allies figure?
- Should dashboard ROI floors and break-even dollars carry a 'model output, not a measured return' label?

### Read first

- [[bombardini-2020-empirical-models-lobbying]]
- [[alexander-2009-lobbying-analysis-under]]
- [[bickmore-2025-corporate-lobbying-political]]
- [[cai-2025-competitive-influence-maximization]]
- [[goff-nd-cross-industry-finance]]

## Across components

## Cross-component findings

1. **No outcome validation anywhere.** All 40 bearings concern algorithm properties or out-of-domain outcomes: donors, firms, tax provisions, synthetic graphs and drones. Every allocator's 'gain', 'spread', 'winnability' or 'ROI' is a model output and should be quoted as one. Spread and graph-based scores are heuristic outputs, never access or influence probabilities (D-025).

2. **Two tiers of guarantee.** The *joint optimizer* and the *flip allocator* are exact on the grid in a fixed stance state, which is stronger than 1-1/e. The *budget frontier* and the *coalition water-fill* carry no transferable guarantee. The causes are the competitive two-colour kernel, the `fac` weights, the bystander exclusion that depends on the seed set, sampled marginals at MC=20, and the fact that the water-fill is not greedy. Code comments asserting submodularity, and Paul's 'submodular tier' label, should be narrowed to match.

3. **Frozen, static opponent everywhere.** The optimizer, the frontier and the break-even all treat industry money as fixed and spending as instantaneous. moreno-2021 (Nash: uniform targeting) and cai-2025 (timing) show that both assumptions matter in competitive settings. The plans are best responses to a passive opponent.

4. **Inconsistent parameters across allocators:**
   - knee at 20% of peak (joint optimizer) vs 25% (frontier)
   - per-member cap of $10k (PMAX, described as the per-candidate legal limit) vs $50k (per-topic, diagnostic) vs uncapped at about $25.8k (workbook, not reproduced)

   If any of these numbers is presented as real PAC money, the $10k limit should bind.

5. **Unreproduced or missing artifacts:** `roi_verify.py` and `fec/apply_alloc_cap.py` are not in the repo; workbook column U has no cached values; the code's 'Lemma 5.3 / A.9' citation is only half located.

6. **Stance gate in front of every allocator.** The gate fails open (no matrix row means eligible), and its evidence base is weak and indirect. Every allocator inherits both.

7. **Rules checks.**
   - No bearing argues for the rejected SNA proposals (dropping regular cosponsorship edges; per-edge volume normalization).
   - zareie-2023's topic-specific edge weights would be a kernel change under change-control. It is not one of the rejected proposals, but it has no accuracy evidence.
   - Any 'lobbied cosponsor' study cannot use LDA to identify members (D-021; LD-2 does not name members).
   - The two UNVERIFIED rows (zeng, madsen) are analogies only and should not be cited as support.
   - Paul's own cross-industry report fails the window-and-denominator rule ('factors of 100').

## Research agenda

1. Run a benchmark-relative backtest in Cagala's design: score the joint optimizer's allocation against uniform, random and proportional-to-score (workbook) allocations on a held-out cycle of CU-aligned outcomes. State the window and denominator, report by party, and treat any result as a lead until it is preregistered. Needs Paul's go.
2. Run a read-only noise bound for the frontier: compare raw (unclamped) marginals at MC=20 and MC=100 under common random numbers, per topic. Use it to bound eps and to replace the 0.35 inversion tolerance in audit_frontier.py with a measured one.
3. Run a read-only competitive-response sensitivity check: let industry money best-respond to the CU plan in the joint optimizer, and report how much winnability the frozen-opponent plan loses.
4. For each topic, run conditional greedy and the singleton water-fill side by side on the coalition kernel under common random numbers. Report both joint spreads, and flag any topic where joint spread exceeds the naive-additive sum, which would indicate threshold effects.
5. Run an observational complex-contagion study: model when members join CU-relevant bills as a function of how many cosponsor partners have already joined. Compare cascade and threshold specifications, control for homophily, stratify by party, and do not use LDA for member identification (D-021).
6. Run a ratio-vs-level panel for the break-even form: member-by-cycle CU and opposing-industry PAC receipts against CU-relevant positions, with member fixed effects, stratified by party.
7. Investigate the stance-gate counter-strategy: do contributions to opposed members precede position change? Use a within-member panel, framed against the access-not-votes literature the system already cites, and expect weak identification.
8. Housekeeping: locate or retract roi_verify.py; locate the workbook column U producer and apply_alloc_cap.py; correct the 'JAIR Lemma 5.3 / A.9' citation; reconcile the 20% and 25% knee rules; reconcile the per-member caps ($10k / $50k / about $25.8k).
9. Source the 'factors of 100' crypto-vs-allies figure in Paul's cross-industry report, with window and denominator, before any system surface relies on it.
10. If the GCOMB or orienteering direction stays on the roadmap: read the GCOMB paper itself (no bearing yet), and first show that greedy is infeasible at the actual scale before proposing a learned policy.
