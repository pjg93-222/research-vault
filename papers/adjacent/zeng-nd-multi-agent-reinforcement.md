---
type: paper
title: "Multi-Agent Reinforcement Learning-Based Extended Boid Modeling for Drone Swarms"
authors:
  - "Qingli Zeng"
  - "Farid Nait-Abdesselam"
year: "2024"
venue: "IEEE International Conference on Communications (ICC) 2024, IoT and Sensor Networks Symposium, pp. 1551-1556"
doi: "10.1109/ICC51166.2024.10622479"
category: "Adjacent"
systems:
  - "melange-allocation"
components:
  - "melange-allocation.joint-optimizer"
paul_annotated: false
paul_markup_count: 0
evidence_status: "unverified"
source: "[[_sources/zeng-nd-multi-agent-reinforcement.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Downloads\\Multi-Agent_Reinforcement_Learning-Based_Extended_Boid_Modeling_for_Drone_Swarms.pdf"
sha12:
  - "995e0e0b27e3"
tags: [paper, adjacent, type/paper, theme/adjacent, evidence/unverified]
---

# Multi-Agent Reinforcement Learning-Based Extended Boid Modeling for Drone Swarms

**Qingli Zeng, Farid Nait-Abdesselam (2024).** *IEEE International Conference on Communications (ICC) 2024, IoT and Sensor Networks Symposium, pp. 1551-1556.* [10.1109/ICC51166.2024.10622479](https://doi.org/10.1109/ICC51166.2024.10622479) Theme: [[themes/adjacent|Adjacent]].

> [!abstract] Summary
> Zeng and Nait-Abdesselam (IEEE ICC 2024) train simulated drones with Q-learning and linear function approximation to reproduce Reynolds' Boid flocking rules: cohesion, alignment and separation, plus a leader-follow rule and a connectivity band. They compare this controller with an unspecified rule-based Boid baseline. The setup is one Pygame scenario with 17 UAVs and 3,500 steps. Training is sequential: one leader, one follower and one learner. Reward weights are fixed by hand (collision -20, alignment +5/-1, cohesion +2, connectivity +1).
> 
> Table II favours the RL controller on every count:
> - proximity advances: 2,097 vs 1,976
> - steps within the 20-40 unit ideal range: 3,176 vs 2,764
> - near-collision events: 45 vs 108
> - mean heading deviation: 14.3 vs 21.1
> 
> The evidence is thin. There is one scenario, with no seeds, replicates, variance or test. The baseline is never described. The counts are not normalised for swarm size. The abstract claims robustness to perturbations, but no perturbation experiment is reported.
> 
> For Paul's systems the paper is essentially out of scope. It contains no legislators, lobbying, money, text, elections or social-network structure. Its only network-like element is distance-based neighbour interaction among physical agents. The one thin link is methodological. It is another case of a learned policy beating a hand-set rule baseline, which is the kind of claim Paul's notes make when they expect GCOMB or contingency-aware RL to replace greedy allocation. On its own evidence the paper cannot support that expectation. A single uncontrolled, unreplicated comparison would not pass the project's single-look discipline, so it should be read only as an example of the claim type. It is not evidence for it. No component's assumptions, constants or code paths are tested, supported or contradicted by this work.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Can drones trained with multi-agent reinforcement learning (Q-learning with function approximation) reproduce bird-flock behaviour (cohesion, alignment, separation, following) more adaptively than rule-based Boid models? The text states the aim as this comparison rather than as a formal question.

**Theory.** Reynolds' Boid model (cohesion, separation, alignment) extended with a leader-follow rule and connectivity band, with reinforcement learning as the adaptive mechanism. Framework is an MDP tuple (S, A, P, R) solved by Q-learning, with linear Q-value approximation Q(s,a)=w·phi(s,a) and Bellman updates (Eqs. 2-6). Rewards are the sum of four terms (Eqs. 7-14).

**Data.** No empirical data. Simulated drone swarm built in Python with Pygame. Unit: UAV (drone). Sample: N = 17 UAVs in the simulation parameters (Table I); training uses three designated UAVs (V1 leader on a predefined trajectory, V2 follower, V3 learner). Period: 3,500 simulation steps per method (Table II). Working area 600 x 800 units, maximum speed 4, distance thresholds d1=20, d2=40, d3=80 (Table I). Number of independent runs and random seeds are not reported.

**Method.** Simulation comparison of two controllers: (a) Q-learning/MARL-trained drones with linear function approximation (learning rate initialised at alpha=0.1 and said to adapt across iterations; discount gamma=0.75) and (b) a rule-based Boid baseline. Reward weights from Table I: collision -20, connectivity +1, alignment with leader +5 (closer) and -1 (farther), cohesion +2. Sequential training: follower V2 learns to trail the leader at about 30 units; learner V3 learns to trail V1 while staying near V2 and avoiding collisions. Performance is reported as four counts or averages over the run: proximity advances, ideal-range tracking (20-40 units), near-collision events (within 20 units), and heading consistency (mean heading-angle difference, Eqs. 15-16). The rule-based baseline is not specified in the text beyond being a Boid-type rule model.

**Identification.** None. The comparison is a single descriptive contrast of two controllers on one simulated scenario. No statistical test, confidence interval, replication, or ablation is reported.

**Scope.** Population: simulated drones in a 2D Pygame environment (dimensionality is implied by the working-area dimensions but not stated). Institution: University of Missouri-Kansas City. Era: 2024. Results apply only to the single simulated scenario in Table I.

## Key findings

- Abstract claims the MARL Boid surpasses rule-based models in cohesion, alignment and separation, and shows robustness to perturbations. Section IV reports only the Table II comparison; no perturbation experiment is reported in the text. (Not applicable (qualitative claim); Abstract, p.1551; contributions, p.1552)
- Both methods completed the task over the same horizon. (Steps 3,500 for RL and rule-based; Table II, Section IV-B, p.1556)
- RL drones recorded more proximity advances than rule-based drones. (2,097 vs 1,976 (+121, about +6.1%); Table II, Section IV-B, p.1556)
- RL drones spent more instances within the ideal 20-40 unit range. (3,176 vs 2,764 (+412, about +14.9%); Table II, Section IV-B, p.1556)
- RL drones had far fewer near-collision events, which the authors read as improved safety. (45 vs 108 (-63, about -58.3%); Table II and text, Section IV-B, p.1556)
- RL heading consistency is reported as better (lower average heading deviation, assuming the metric is a deviation, as defined on p.1556). (14.3 vs 21.1 (-6.8, about -32.2%); Table II, Section IV-B, p.1556)
- Conclusion: RL can minimise collisions, keep consistent headings and keep proximity, and may suit dynamic scenarios that hard-coded rules handle poorly. These are the authors' interpretations of the single comparison, not tested in the text. (Not applicable; Conclusion, p.1556)

## Limitations

- Stated by the authors (Conclusion, p.1556): future research could explore more intricate behaviours, enhanced real-world applicability, and other advanced learning paradigms.
- Not stated in the text but visible in it: a single simulated scenario; no reported seeds, replicates or variance; no description of the rule-based baseline's parameters; no perturbation experiment despite the robustness claim; the Table II counts are not normalised for the number of drones or area.

## Short quotes

- "Empirical evaluations showcase that our MARL Boid not only surpass traditional rule-based models in cohesion, alignment, and separation" — Abstract, p.1551
- "Our foundational belief was rooted in the notion that the essence of flocking could be imbibed by training a singular UAV agent." — Section III-D, p.1555
- "the RL approach considerably enhanced safety, evidenced by a reduction in near-collision events to 45, in contrast to the Rule-based strategy's 108." — Section IV-B, p.1556

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-allocation#Joint CU budget optimizer\|Joint CU budget optimizer]] | context | An RL controller (Q-learning, linear function approximation, alpha 0.1, gamma 0.75) beats a rule-based Boid baseline on one simulated drone scenario: near-collisions 45 vs 108 (-58%), ideal-range steps 3,176 vs 2,764 (+15%), heading deviation 14.3 vs 21.1, all over 3,500 steps with N = 17 UAVs. No seeds, replicates, variance or baseline specification are given. (Table I (p.1555) and Table II with Section IV-B (p.1556)) | The built optimizer uses deterministic CELF lazy greedy over per-member concave chunks ($500 steps, PMAX $10k cap, stance-gated members excluded). It has no learned policy. Paul's notes expect a learned policy (GCOMB) to replace greedy at scale. `fec/engine/optimize.py:3-7,27,71,126-151` | Carries over only as an analogy: a learned policy versus hand-set rules in a multi-agent setting. Nothing transfers to political allocation. The agents are physical drones, the reward is hand-specified geometry, and there is no budget, submodularity, money or legislator response. The evidence is one unreplicated simulation, so it cannot show that RL would outperform greedy anywhere, let alone on the cosponsor kernel. | weak | UNVERIFIED |

## Gaps and flags

- None specific to the system. The paper's topic (drone swarm control) is absent from every component. The only general point is a reminder: if a learned allocation or seeding policy (GCOMB, contingency-aware RL) is ever built, its comparison with CELF greedy needs replicates, seeds and a fully specified baseline. This paper provides none of the three.
- Out of scope: the paper contains no lobbying, legislators, elections, persuasion, money, text-measurement or social-network material. Do not cite it as support for any Melange component.
- Its RL-beats-rules claim rests on a single unreplicated simulation with no reported variance, and the robustness claim in the abstract has no experiment behind it. If it is ever cited for 'learned policies beat greedy/rule baselines', label the claim UNVERIFIED in line with the project's single-look and DE-062 standards.
- Its distance-based neighbour interaction is not a social tie. It should not be read as support for neighbour-based stance imputation or cascade edges, whose co-movement test was null in this project.

Paul has no annotation of this paper, and none of his component positions cite it. Its one point of contact is his expectation that a learned policy (GCOMB, Chen et al. 2021 contingency-aware RL) will eventually replace greedy allocation and seeding. The paper repeats the general claim that RL beats hand-set rules, but on evidence too thin to strengthen that expectation. It neither confirms nor dates any of his notes. The system as built (CELF greedy, the SGATE table) is unaffected.

## Related papers

- [[chen-2021-contingency-aware-influence]] — complements — Both use reinforcement learning to replace a hand-set rule policy. Chen et al. do it for seeding legislators-as-nodes under take-up uncertainty, which is the literature actually behind the SGATE seed gate. Zeng applies RL only to physical swarm control.
- [[luo-2020-path-planning-algorithm]] — complements — Same UAV application domain. Luo plans paths for single drones with D* Lite, while Zeng controls a swarm with RL. Both are engineering simulations relevant to Paul's pathfinding reading only by analogy.

### Linked from
- [[sutton-2018-reinforcement-learning-introduction]] — complements — Paper exists. Adjacent multi-agent RL with no direct bearing on Melange; the builds-on relation is not confirmed.