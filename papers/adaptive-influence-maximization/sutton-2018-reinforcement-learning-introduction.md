---
type: paper
title: "Reinforcement Learning: An Introduction (2nd ed.)"
authors: ["Richard S. Sutton", "Andrew G. Barto"]
year: "2018"
venue: "MIT Press (Adaptive Computation and Machine Learning series; copyright 2018, 2020 printing; ISBN 9780262039246)"
doi: ""
category: "Adaptive Influence Maximization"
systems: ["melange-allocation", "melange-validation"]
components: ["melange-allocation.joint-optimizer", "melange-validation.prospective-contract"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/sutton-2018-reinforcement-learning-introduction.pdf]]"
original_paths: []
sha12: ["05def740ce1f"]
tags: [paper, adaptive-influence-maximization, type/paper, theme/adaptive-influence-maximization, evidence/verified]
---

# Reinforcement Learning: An Introduction (2nd ed.)

**Richard S. Sutton, Andrew G. Barto (2018).** *MIT Press (Adaptive Computation and Machine Learning series; copyright 2018, 2020 printing; ISBN 9780262039246).* Theme: [[themes/adaptive-influence-maximization|Adaptive Influence Maximization]].

> [!abstract] Summary
> Sutton and Barto's second edition is the standard textbook on reinforcement learning (RL): an agent learns a policy by trial and error so as to maximize cumulative reward. It moves from k-armed bandits and the exploration-exploitation trade-off (Ch. 2), through finite MDPs, dynamic programming, Monte Carlo and temporal-difference learning (Ch. 3-7), to model-based planning (Ch. 8), function approximation and the deadly triad (Ch. 9-11), eligibility traces, policy gradients, psychology and neuroscience, and case studies (Ch. 16). It contains no social-science data. Its evidence is simulation (on the 10-armed testbed, greedy averages about 1 reward per step against a best of about 1.54) plus reported applications: TD-Gammon after about 300,000 self-play games, AlphaGo Zero after 4.9 million self-play games, and Adobe ad policies scored by high-confidence off-policy evaluation on bank-website logs. For Paul's systems the book is mainly vocabulary and foundation, not evidence. It underlies the RL influence-maximization papers in the vault (Chen 2021 and 2023, RELINK) that Paul maps to the seed gate and the allocator. Measured against the code, the joint CU optimizer is a one-shot, model-based plan. It runs greedy against modeled flip probabilities, never explores, and ingests no observed outcomes (observations=[]). It replans only after modeled threshold crossings. In the book's terms it is pure exploitation of a fixed model, with a myopic single-period objective. The book also sharpens a feasibility limit on Paul's expectation that a learned policy will replace greedy. Its successes needed hundreds of thousands to millions of episodes, but a real CU allocation gives roughly one episode per two-year cycle. Any learned policy would therefore be trained inside the same unvalidated simulator. Section 16.7 points to the more transferable idea: evaluate a candidate allocation policy off-policy from logged contacts and funding, with propensities. The prospective contract does not do this.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** How can an agent learn by trial-and-error interaction with an environment to maximize cumulative reward, and what are the key ideas and algorithms for doing so (stated aim: a clear and simple account of the key ideas and algorithms of reinforcement learning, per the Preface to the Second Edition)?

**Theory:** Markov decision process framework with Bellman equations for value functions (Chapters 3-4); temporal-difference learning (Chapter 6); eligibility traces (Chapter 12); policy gradient methods (Chapter 13); links to animal psychology and neuroscience reward prediction error hypotheses (Chapters 14-15).

**Data:** No empirical dataset. Simulated 10-armed bandit testbed (2000 randomly generated bandit problems, 1000 time steps each, averaged over 2000 runs) and simulated random-walk and gridworld tasks. Chapter 16 case studies report results from prior work, mainly games (backgammon, checkers, Go, Jeopardy! wagering) and computer systems (memory controller, video games).

**Method:** Expository textbook with worked examples, pseudocode, exercises and mathematical derivations (optional material in shaded boxes). Simulation experiments compare algorithms, for example greedy versus epsilon-greedy action-value methods on the 10-armed testbed.

**Scope:** Single-agent sequential decision problems modeled as Markov decision processes or bandits, mainly tabular methods first and then function approximation. Field is artificial intelligence and machine learning, not social science. Relevance to lobbying, elections, persuasion, turnout, networks or text measurement: none of these topics appears in the text. Closest overlaps are bandit and contextual-bandit allocation (Sections 2.9 and 16.7).

## Key findings

- Section 2.3 (pp. 28-30): On the 10-armed testbed (2000 random bandit problems, 1000 steps, 2000 runs), the greedy method reaches about 1 reward per step against a best possible of about 1.54, and performs significantly worse in the long run because it gets stuck on suboptimal actions.
- Section 11.3 (p. 264): The deadly triad of function approximation, bootstrapping and off-policy training, combined, can cause instability and divergence in the prediction case.
- Section 16.1 (pp. 421-429): TD-Gammon combined TD(lambda) with a multilayer neural network trained on self-play. TD-Gammon 0.0 with essentially zero backgammon knowledge learned to play approximately as well as the best previous backgammon computer after about 300,000 self-play games.
- Section 16.1: Adding specialized backgammon features while keeping self-play TD learning produced TD-Gammon 1.0, described as clearly substantially better than all previous backgammon programs.
- Section 16.1: The backgammon game tree has an effective branching factor of about 400, too large for conventional heuristic search methods used in chess and checkers.
- Section 16.3 (pp. 429-432): Watson Daily-Double wagering used a TD-Gammon-style value function learned over millions of simulated games and chose the bet with the maximum action value. The cited authors report its effectiveness went well beyond what human players do in live play.
- Section 16.4 (pp. 429-432): A reinforcement learning memory controller improved on the FR-FCFS controller by 7 percent to 33 percent across nine applications, averaging 19 percent, and closed 27 percent of the gap to an unrealizable Optimistic controller, per the text.
- Section 16.6.2 (p. 447): AlphaGo Zero trained over 4.9 million self-play games in about 3 days. In a 100-game match it defeated the AlphaGo version that beat Lee Sedol in all 100 games. Reported Elo ratings: 4308 for AlphaGo Zero, 3739 for the Lee Sedol version, 3144 for the Fan Hui version.
- Section 16.7 (p. 450): Personalized delivery of news or advertisements is framed as a contextual bandit problem, because a non-associative bandit does not personalize content delivery.
- Chapter 1 (pp. 8-13): The tic-tac-toe example learns position value estimates from self-play using a temporal-difference style update; this is the book framing example for the core RL idea.

## Limitations

- P
- r
- e
- f
- a
- c
- e
- :
-  
- t
- h
- e
-  
- b
- o
- o
- k
-  
- i
- s
-  
- a
- n
-  
- i
- n
- t
- r
- o
- d
- u
- c
- t
- i
- o
- n
-  
- a
- n
- d
-  
- m
- a
- k
- e
- s
-  
- n
- o
-  
- a
- t
- t
- e
- m
- p
- t
-  
- a
- t
-  
- c
- o
- m
- p
- r
- e
- h
- e
- n
- s
- i
- v
- e
-  
- c
- o
- v
- e
- r
- a
- g
- e
-  
- o
- f
-  
- a
-  
- f
- i
- e
- l
- d
-  
- t
- h
- a
- t
-  
- h
- a
- s
-  
- e
- x
- p
- l
- o
- d
- e
- d
-  
- i
- n
-  
- m
- a
- n
- y
-  
- d
- i
- r
- e
- c
- t
- i
- o
- n
- s
- .
-  
- I
- t
-  
- a
- v
- o
- i
- d
- s
-  
- a
-  
- r
- i
- g
- o
- r
- o
- u
- s
-  
- f
- o
- r
- m
- a
- l
-  
- t
- r
- e
- a
- t
- m
- e
- n
- t
- ,
-  
- k
- e
- e
- p
- i
- n
- g
-  
- t
- h
- e
-  
- m
- o
- r
- e
-  
- m
- a
- t
- h
- e
- m
- a
- t
- i
- c
- a
- l
-  
- p
- a
- r
- t
- s
-  
- i
- n
-  
- s
- h
- a
- d
- e
- d
-  
- b
- o
- x
- e
- s
- .
-  
- F
- o
- c
- u
- s
-  
- i
- s
-  
- o
- n
-  
- c
- o
- r
- e
-  
- o
- n
- l
- i
- n
- e
-  
- l
- e
- a
- r
- n
- i
- n
- g
-  
- a
- l
- g
- o
- r
- i
- t
- h
- m
- s
- .
-  
- C
- h
- a
- p
- t
- e
- r
-  
- 1
- 6
-  
- i
- n
- c
- l
- u
- d
- e
- s
-  
- o
- n
- l
- y
-  
- a
-  
- h
- a
- n
- d
- f
- u
- l
-  
- o
- f
-  
- a
- p
- p
- l
- i
- c
- a
- t
- i
- o
- n
- s
- ,
-  
- o
- u
- t
-  
- o
- f
-  
- n
- e
- c
- e
- s
- s
- i
- t
- y
- .

## Short quotes

- "the danger of instability and divergence arises whenever we combine all of the following three elements, making up what we call the deadly triad"
- "Tesauro obtained an unending sequence of games by playing his learning backgammon player against itself."
- "In a match of 100 games between AlphaGo Zero, trained as described, and the exact version of AlphaGo that defeated Lee Sedol, AlphaGo Zero defeated AlphaGo in all 100 games."

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-allocation#Joint CU budget optimizer\|Joint CU budget optimizer]] | qualifies | Acting greedily on current value estimates without exploration can lock onto suboptimal actions. On the 10-armed testbed (2000 independent problems, 1000 steps each), the greedy method averages about 1 reward per step against a best possible of about 1.54, and epsilon-greedy does better in the long run. Planning on an incorrect model is likely to compute a suboptimal policy (Sec. 8.3); planning uses a model, learning from experience uses observed interactions (Sec. 8.1). (Sec. 2.2-2.3, pp. 27-30; Sec. 8.1, pp. 159-161; Sec. 8.3 (When the Model Is Wrong)) | fec/engine/optimize.py allocates a CU budget (default $2.5M in $500 increments, with a per-member cap PMAX) by lazy greedy on modeled marginals of flip_value. The loop stops at the budget or when no positive marginal remains, so spend can fall short of the pot. It has no exploration term. observations is an empty list reserved for real outcomes. It replans after a modeled defection threshold is crossed (the conversions list), so it is not a one-shot plan. In RL terms it is greedy exploitation of a modeled response with no feedback from observed outcomes. `fec/engine/optimize.py:3-12 (docstring), 26 (BUDGET and INC defaults), 58 (observations), 127-150 (greedy loop and modeled conversion replanning)` | Exploration pays only if the same allocation repeats and outcomes are observed and fed back. Per-cycle CU outcomes are sparse and confounded, so a bandit layer would learn slowly. The transferable point holds: the optimum is exact only for the model, not for the world. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Joint CU budget optimizer\|Joint CU budget optimizer]] | suggests-extension | Greedy contextual-bandit policies maximize immediate clicks and ignore long-term effects; an MDP lifetime-value policy (Theocharous et al.) beat greedy on the LTV metric, while greedy won on CTR. In the illustrative Figure 16.8, CTR is 0.35 and LTV is 1.5. (Sec. 16.7, pp. 450-453 (Fig. 16.8); Sec. 2.9) | The objective at optimize.py line 86 is total_winnability, the sum of flip_value over issues for a single run. The printed objective is labelled expected weighted flips (line 209). The objective is single-run and does not model multi-cycle carry-over. I did not check flip_model or money_response for cross-cycle memory, so the claim that money has no modeled effect on next cycle's receptivity, relationship or stance is confirmed only for the optimizer itself. `fec/engine/optimize.py:86, 209` | Holds only if repeated CU giving to the same member changes later responsiveness. That is untested here. Per D-025 it must not be phrased as access or closeness. Any multi-cycle objective needs owner authorization and a second-path reproduction (DE-062). | weak | VERIFIED-SOFTENED |
| [[systems/melange-validation#Prospective receptivity contract (H1-H2) and forecast ledger\|Prospective receptivity contract (H1-H2) and forecast ledger]] | suggests-extension | Before deploying a new policy, Adobe (Theocharous et al.) estimated its value from data logged under other policies, using high-confidence off-policy evaluation with probabilistic improvement guarantees. Logs: about 200,000 interactions from a campaign that randomly offered one of 7 offers, and about 4,000,000 interactions over 12 offers. The test set was real interactions served by a random policy. (Sec. 16.7, pp. 451-453) | outcome_contract.py scores frozen predictors by Spearman: seed_prob against new cosponsorship gain (H1), and pressure_mult against the absolute tier movement (H2). Verdict bars: PASS at 0.20 or above, DIRECTIONAL from 0.10 to 0.20, INDICTED at -0.10 or below. Minimum: 60 days and at least 150 members with a positive cosponsorship delta (the movers count in code). The protocol text instead says 150 members with any delta. The contract ranks frozen predictors; it does not estimate the value of a counterfactual policy. The engaged-only contact arm is marked NOT_IMPLEMENTED_NO_CONTACT_CONTRACT. `fec/engine/outcome_contract.py:17-22 (protocol text), 216-217 (verdict), 243-263 (deltas, movers, minimum), 275-281 (scoring)` | Off-policy estimates for alternative allocations need logged actions with known or estimable propensities and overlap. I did not confirm that funding and contact are logged without propensities beyond the missing contact contract. The claim that real assignment is strategic and near-deterministic is the analyst's assertion and has no support in the code I read. Any such estimate stays UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The summary calls the joint CU optimizer a one-shot plan, but the same paragraph says it replans after modeled threshold crossings. It replans, so it is not one-shot.
> - Summary issue: The summary says Section 16.7 points to evaluating policies off-policy with propensities. The book describes high-confidence off-policy evaluation from logs of other policies; it does not mention propensities.
> - Summary issue: The summary says the book underlies the RL influence-maximization papers in the vault. The Chen 2021 stub does not cite Sutton and Barto, so this dependence is unconfirmed.
> - Summary issue: The statements about Paul's expectation that a learned policy will replace greedy, and about his Chen 2021 mapping, are not verifiable from the book or the code and need Paul's own source.
> - Summary issue: The gaps list says the per-topic allocators and budget frontier exploit fixed curves with no feedback loop. Only optimize.py was checked for this; the other two were not verified.
> - Summary issue: The metadata flag says kind academic-article should be book. The ext stage agrees; the title, authors, year and venue hints match the text.
> - Summary issue: Coverage: about 1.5M characters were not read in full. Checks were targeted at the cited passages and keyword searches, and page numbers are approximate running-header numbers.

## Gaps and flags

- No component learns from observed outcomes. The optimizer, budget frontier and per-topic allocators all exploit fixed modeled response curves, and none has an exploration or feedback loop. The observations field in optimize.py is reserved but always empty.
- The seed gate implements Chen 2021's contingency concept as a hand-set SGATE table (moneyball_core.py:626-630). None of the RL machinery that Paul's cited sources build on (Q-learning, policy learning) is present, and nothing estimates SGATE values from data.
- There is no logged-policy record (who was contacted or funded, when, and with what propensity), so the off-policy evaluation of alternative allocation policies described in Sec. 16.7 is not possible yet.
- The allocation objective is single-cycle. Effects that carry across cycles from repeated giving are not modeled, so a myopic-versus-long-horizon comparison (the CTR-versus-LTV contrast) cannot be run.
- Metadata: the item kind is academic-article, but the work is a textbook (2nd edition, 2020 printing). No DOI is needed.
- Feasibility caution, not a brain-rule conflict: the book's RL successes used 300,000 to 4.9 million training episodes. Any learned allocation policy for Melange would be trained in the simulator, not on real cycles. Its outputs would inherit simulator error and would stay UNVERIFIED (DE-062) until reproduced by a second path.
- Sec. 11.3 deadly triad: combining function approximation, bootstrapping and off-policy training risks divergence. A GCOMB-style learned replacement for greedy trained off-policy on sparse legislative data would combine all three.

Paul has no annotations or entries on this book. His position on the joint optimizer (adaptive greedy plus CELF now, a learned policy such as GCOMB later at scale) is consistent with the system as built, and that note is not stale: the code is still CELF greedy with no learned policy. The book qualifies the second half of his expectation. Learned policies earn their advantage through very large numbers of episodes, which real CU allocation cannot supply, so a learned policy would only learn the simulator. His Chen 2021 mapping is built as a fixed SGATE table.

## Related papers

- [[chen-2021-contingency-aware-influence]] — complements — Paper exists in the research store (papers/chen-2021-contingency-aware-influence.md) and its components list the seed gate. Its stub does not cite Sutton and Barto, so a builds-on link to this book is not confirmed.
- [[chen-2023-complex-contagion-influence]] — complements — Paper exists in the research store. Shares the RL foundations of the book, but no citation of this book was confirmed in the stub.
- [[arya-2025-relink-edge-activation]] — complements — Paper exists in the research store. The link to this book's function-approximation methods is the analyst's inference and was not checked against the paper text.
- [[golovin-2011-adaptive-submodularity-theory]] — complements — Paper exists. The optimize.py docstring does retain a Golovin-Krause bound as a diagnostic, so the complement link is real.
- [[zeng-nd-multi-agent-reinforcement]] — complements — Paper exists. Adjacent multi-agent RL with no direct bearing on Melange; the builds-on relation is not confirmed.

### Linked from
- [[ng-1999-policy-invariance-reward-transformations]] — complements — The 2018 text discusses potential-based shaping and cites Wiewiora 2003 alongside it, so it complements this paper's shaping theory.
- [[watkins-1992-q-learning]] — complements — Real item (Sutton and Barto, 2018). Its content on the deadly triad and tabular versus function-approximation limits was not checked against the item text in this pass.