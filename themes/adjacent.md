---
type: theme
name: "Adjacent"
paper_count: 12
tags: [theme]
---

# Adjacent

**Adjacent** holds 12 works from outside lobbying and congressional-network research. Paul has left no theme-level notes for it (`paul_notes` is empty), so this synthesis is built from the per-work summaries, findings, links and bearings in the theme file.

The category is a grab-bag, but it splits cleanly. Each work matters to Paul's systems for one of two reasons:

- **It is cited or implemented in the system.** These are the Melange constants and mechanisms that rest on it:
  - **Receptivity layer:** [[byrne-2014-chokes-under-pressure]] and [[linde-2016-politicians-take-risks]] are cited directly. [[palm-2025-influence-personality-traits]] and [[amsalem-2018-effect-politicians-personality]] bear on the same layer.
  - **Salience-shock exec cue:** [[noble-2024-presidential-leadership-legislative]].
  - **Committee pillar:** [[thakur-2025-matching-theory-perspective]].
  - **Competitiveness multiplier:** [[best-2022-redistricting-commissions-avoid]].
- **It is a method from another field that could, by analogy, test a design choice.**
  - [[zeller-2016-step-path-selection]]: converting probabilities into path-finding costs.
  - [[zhan-2025-cost-minimized-stage]]: imputing missing values from network neighbours.
  - [[esmalian-2022-characterizing-equitable-access]]: facility counts as a stand-in for access.
  - [[zeng-nd-multi-agent-reinforcement]]: a learned policy versus a hand-set rule.
  - [[anon-2022-simulating-role-knowledge]]: no usable text, so no claims can be drawn from it.

The main lesson is about how far each source reaches. Almost every cited paper supports a narrower claim than the constant built on it:

- Linde supports a reflection effect. It does not support loss aversion or the constants 1.15 / 0.92 / 1.10.
- Byrne supports neuroticism (N) hurting delayed-reward choices under pressure among students. It does not show N making a member more open to persuasion.
- Noble's predictions are theory only, and the live advisory may reverse its central prediction. That reversal is UNVERIFIED and comes from a single analysis path (DE-062).

The institutional papers make a second point. Things the system treats as fixed attributes of a member or district are partly products of procedure: who holds a committee seat depends on each party's assignment mechanism and the seniority queue, and district lean depends on how the map was drawn. None of the 12 works validates any constant in Paul's systems.

## Strands

### Elite psychology behind the receptivity layer (traits, pressure, risk)

Four works address the psychology the receptivity layer assumes. The layer's susceptibility composite is `.40A + .30N - .20C - .10E`; it also has a loss-domain multiplier `pt_mult` (1.15 live race / 0.92 safe) and a visibility disposition that feeds `seed_prob`.

**Where they agree.** Personality effects on elite outcomes are small and depend on context:
- Amsalem: trait betas are about 0.1, against 0.369 for being a minister. The sample is n = 306 legislators in Belgium, Canada and Israel, from a 2015 survey matched to media counts over 2011-2015. It is pooled with country dummies, so it is UNSTRATIFIED by legislature.
- Linde: elite effects are weaker than in the lottery tasks, and the political scenarios reach significance only when pooled (n = 46 Dutch MPs, 2014-2015).
- Byrne: only neuroticism survives both experiments (Exp 1 N = 127 undergraduates; Exp 2 about 61-62, with no control arm).
- Palm: the evidence base is 80 studies (1982 to Sep 2024), mostly the general public (62.5%) and students (31.25%), with no pooled effect sizes.

None of the four samples is the US Congress.

**Where they disagree.** They disagree on the sign of each trait slot, because each measures a different construct:
- **N.** [[byrne-2014-chokes-under-pressure]] supports the +.30 N weight only in the sense that N degrades delayed-reward choices under pressure. [[palm-2025-influence-personality-traits]] ranks N *least* susceptible to persuasion. [[amsalem-2018-effect-politicians-personality]] finds emotionally stable MPs get *more* coverage.
- **A.** Palm ranks A most susceptible, which supports the +.40 slot. Byrne's A effect is only a trend in Exp 1 (p = .08), and Amsalem finds *low*-A MPs more visible. Because the visibility disposition raises `seed_prob`, the persona path and the lexical susceptibility weights pull in opposite directions on A and N.
- **E.** Palm ranks E above average in susceptibility, which runs against the -.10 slot.

The project's own test of Byrne's mechanism came back null: H5, N x loss domain, partial r = +0.036, n = 441 (the record does not state the window). The N slot should therefore stay UNVERIFIED.

**Risk and loss.** [[linde-2016-politicians-take-risks]] supports a reflection effect: risky choice in the incentivized lottery is 39.1% under a gain frame vs 76.1% under a loss frame, n = 46. It found no loss aversion. Its framing was randomly manipulated, and its MPs are elected from list seats. So it does not justify reading seat insecurity as a loss frame, equating risk-seeking with being easier to move with money, or any of the constants 1.15, 0.92 or `LOSSFRAME` 1.10.

**Missing data.** Amsalem adds a selection point. If airtime selects on temperament, then interview-derived traits (including affective N), which exist only for members who get airtime, are not missing at random.

Works:
- [[byrne-2014-chokes-under-pressure]]
- [[linde-2016-politicians-take-risks]]
- [[palm-2025-influence-personality-traits]]
- [[amsalem-2018-effect-politicians-personality]]

### Formal and institutional models: how procedure manufactures system inputs

These three works model or measure the political institutions behind inputs the engine consumes.

**[[noble-2024-presidential-leadership-legislative]]** is a pure game-theoretic model with no data. Its two central results:
- Going public always widens gridlock (Prop 1).
- Presidents go public over a wider range of costs when the status quo already favours them, the "defensive appeal" (c(-1) > c(1), Prop 2).

Melange implements it almost line for line in `salience_shock.py` and the dashboard's exec cue. The party asymmetry carries over faithfully. Several mappings do not:
- Rally and retreat odds are tied to the elite stance gap.
- psi is built from missing stance data.
- The presidential-reelection channel is moved onto legislators and onto a CU actor.

The live advisory credits "defections prevented" on every issue. As a result, all four GO PUBLIC verdicts (2026-10-08) fall on *offense* issues, which would reverse Prop 2. That finding is UNVERIFIED until a second path reproduces it (DE-062).

**[[thakur-2025-matching-theory-perspective]]** argues that Senate Republicans and Democrats fill committee seats through structurally different mechanisms: a strategyproof seniority serial dictatorship versus an approximate Boston mechanism with committee-on-committees tie-breaks. The evidence is explicitly suggestive:
- Freshman Democrats list Prestige committees as first choice 8% less often (p = 0.15, 1947-1995 request data).
- Republicans show no such gap (p = 0.80).
- A Republican-only structural model covers 426 new assignments (104th-114th Congresses).

**[[best-2022-redistricting-commissions-avoid]]** compares enacted 2010-round maps in 34 states with 10,000 partisan-blind plans per state. Three of the seven commission states show 1.5-4 points of partisan asymmetry (2008-2016 statewide elections).

The three agree on one structural point: values the system reads as properties of a member or district are partly artifacts of procedure. Committee seat holdings reflect party mechanism and seniority queues. Cook-style district lean shifts by about the same size as the competitiveness multiplier's 5-point scale constant, depending on how the lines are drawn. Noble adds that whether a salience shock helps depends on the status quo, which a single "defections prevented" score ignores.

None of the three provides causal identification, and none validates a system weight.

Works:
- [[noble-2024-presidential-leadership-legislative]]
- [[thakur-2025-matching-theory-perspective]]
- [[best-2022-redistricting-commissions-avoid]]

### Cost, access and movement measured from behaviour

Both papers come from outside political science, and both reach the same methodological conclusion: costs or access estimated from what agents actually do differ from those assumed from geometry or supply counts.

**[[zeller-2016-step-path-selection]]** fits step and path selection functions to GPS tracks from 10 pumas (75,716 fixes, 2011-2014). It finds:
- Coefficients vary sharply with scale; some cover types flip sign across 19 scales from 532 to 7,390 m.
- Coarsening fixes from 5 to 60 minutes shifts coefficients 3-4 fold.
- The authors say no empirical evidence supports treating resistance as the inverse of probability.

This bears by analogy on the path-finding layer's probability-to-cost inversion (`-log p`, `-ln(0.8*strength)`). It also supplies a calibration template, used versus available routes, which would need an outreach log that does not exist yet.

**[[esmalian-2022-characterizing-equitable-access]]** studies Harris County during Hurricane Harvey (1,804 traffic zones, 920 stores, Aug-Sep 2017). The number of stores in a zone barely tracks the change in its access (r = -0.06 / -0.01 / -0.03 across the three indicators). Its "degree" counts distinct stores visited; it is not a centrality measure.

Both transfers are analogical and weak, and every bearing they carry is UNVERIFIED.

The staffer-route search already labels its cost "not a probability of access", which is consistent with D-025. These papers reinforce that label; they do not license reading route cost as closeness.

Works:
- [[zeller-2016-step-path-selection]]
- [[esmalian-2022-characterizing-equitable-access]]

### Simulation-only designs from other fields

The three works share an evidentiary weakness. Their support is synthetic or comes from a single scenario, with no holdout, no replication and no causal identification.

**[[zhan-2025-cost-minimized-stage]]** uses one worked case: 20 decision-makers, 5 alternatives, random adjustment costs, and a trust network taken from an earlier paper. Group consensus rises from 0.9117 to 0.9368 over 11 rounds. Its imputation error (MSE 0.09-0.13) is measured only on randomly generated matrices.

**[[zeng-nd-multi-agent-reinforcement]]** reports one Pygame scenario (17 UAVs, 3,500 steps) against a baseline the paper never describes. Near-collision events are 45 vs 108, with no variance and no perturbation experiment, although the abstract claims robustness.

**[[anon-2022-simulating-role-knowledge]]** has no recoverable text. Its only local copy is Wiley licence headers (5,082 chars), and Paul's 82 highlights carry no text.

Only Zhan touches a system component. Its trust- and similarity-weighted neighbour imputation is a possible alternative to the stance-dynamics proposer's plain mean over cosponsor neighbours. But that component already found co-movement on cosponsor ties to be null, which undercuts the homophily premise this whole imputation family needs. Zhan's "trust" is an elicited score; reading cosponsor weights as trust would break D-025. Zhan does not propose per-edge volume normalization (a REJECTED SNA proposal), and any adaptation should not drift into it.

Zeng is only an example of the "learned policy beats hand-set rule" type of claim that Paul's notes anticipate for GCOMB or contingency-aware RL. It is not evidence for that claim, and it would fail the project's single-look discipline.

Works:
- [[zhan-2025-cost-minimized-stage]]
- [[zeng-nd-multi-agent-reinforcement]]
- [[anon-2022-simulating-role-knowledge]]

## Agreements and debates

**1. The sign of N, A and E in the susceptibility composite.**

| Trait | Byrne | Palm | Amsalem |
|---|---|---|---|
| N | Hurts performance under pressure (supports +.30, read as "chokes") | Least susceptible to persuasion | More stable MPs get more coverage |
| A | Exp 1 trend only, p = .08; significant only in the uncontrolled Exp 2 | Most susceptible (supports +.40) | Low-A MPs more visible |
| E | Null | Above-average susceptibility (against -.10) | More visible |

Sources: [[byrne-2014-chokes-under-pressure]], [[palm-2025-influence-personality-traits]], [[amsalem-2018-effect-politicians-personality]].

The disagreement is partly a construct mismatch, not a straight contradiction. The three measure decisions breaking down under pressure, response to persuasion messages, and media coverage respectively. Inside Melange, though, it becomes a real tension: the visibility disposition raises `seed_prob` while the lexical susceptibility weights point the other way on A and N. The project's own H5 null (partial r = +0.036, n = 441, window not stated in the record) sides against relying on Byrne's mechanism.

**2. Reflection effect vs loss aversion.** [[linde-2016-politicians-take-risks]] finds reflection, both in the incentivized lottery and in the Asian-disease task (odds ratio 14.6). It finds no loss aversion. Its claim that "votes set the reference point" rests on a pooled odds ratio of 8.68 (p = 0.033, n = 164 responses across four scenarios; three of the four show no effect). That pooled figure should be quoted as UNSTRATIFIED by scenario. Melange's comment that `LOSSFRAME` is "elite-attenuated per Linde-Vis" overstates what the paper found.

**3. Offense vs defense in going public.** [[noble-2024-presidential-leadership-legislative]] predicts that appeals are mainly defensive (Prop 2, and the retreat case). The live advisory's four GO PUBLIC verdicts all fall on offense issues. This is a contradiction between the paper and the implementation, not between papers, and it is UNVERIFIED (single path, DE-062).

**4. Member choice vs mechanism.** [[thakur-2025-matching-theory-perspective]] and [[best-2022-redistricting-commissions-avoid]] both argue against reading committee seats or district lean as purely member- or voter-driven. Neither is contradicted within the theme. Both rely on descriptive or suggestive evidence (Thakur's key contrasts have p = 0.15 and 0.80; Best is within-state and makes no causal claim).

**5. Learned vs rule policies.** [[zeng-nd-multi-agent-reinforcement]] claims RL beats rules on one scenario. Nothing in the theme tests that claim, and nothing in the theme argues against it.

## Methods

**Methods that dominate:**
- **Small-sample experiments:**
  - Byrne: student lab experiments, N = 127 and about 61-62.
  - Linde: an online incentivized experiment, n = 46 MPs.
- **One cross-sectional elite survey regression:** Amsalem, OLS, n = 306, TIPI traits.
- **One narrative systematic review:** Palm, 80 studies, counts and directions only.
- **Formal theory:**
  - Noble: subgame-perfect equilibrium.
  - Thakur: mechanism design, plus descriptive moments and a reduced-form structural model.
- **Simulation:**
  - Best: a counterfactual ensemble of 10,000 plans per state (careful work).
  - Zhan and Zeng: single-case or synthetic demonstrations (weak work).
- **Two behavioural-trace designs from other fields:**
  - Zeller: conditional-logistic step and path selection with 10-fold cross-validation.
  - Esmalian: bipartite trip networks with DTW clustering.

**Weaknesses that recur:**
- **No causal identification anywhere.** Amsalem's "effect" is a covariate-adjusted association, and Esmalian uses causal language its design cannot support.
- **The population differs from the target.** Samples are students, Dutch list-seat MPs, Belgian, Canadian and Israeli legislators, consumers, pumas and drones. None is the US Congress, and only Thakur touches it (Senate only, about 100 of 533 members).
- **Construct mismatch.** Byrne's delayed-reward task is not persuadability. Palm's perceived persuasiveness is not legislative movement. Esmalian's degree is not centrality.
- **Weak measurement.** Amsalem's agreeableness has alpha = .15. Byrne had no manipulation check, and the paper itself flags that money and social pressure are confounded. Zeller's resistance = 1/probability step is acknowledged as unvalidated. Zeller also shows that fixes coarsened from 5 to 60 minutes shift coefficients 3-4 fold.
- **Pooling across fault lines.** Amsalem pools four legislatures with country dummies, and Linde pools across scenario types. Both pooled figures should carry an UNSTRATIFIED label when quoted.
- **Single-scenario, unreplicated simulations** (Zeng, Zhan), with no variance and no holdout.
- **Predictions left untested in the paper itself.** Noble estimates nothing, and Thakur says he cannot test Predictions 1, 2(a) and 3(b).

## How the works relate

- [[linde-2016-politicians-take-risks]] — complements — [[noble-2024-presidential-leadership-legislative]]: Noble borrows 'gambling for reelection' (A.4), which salience_shock.py encodes as the VULN x1.35 amplifier; Linde's suggestive finding that votes set politicians' reference point is an individual-level prospect-theory analogue, though for legislators, not the president.
- [[linde-2016-politicians-take-risks]] — co-cited in one product — [[byrne-2014-chokes-under-pressure]]: Linde's loss domain (pt_mult) and Byrne's susceptibility composite (susc_mult) are multiplied together in pressure_mult (receptivity_layer.py:227); neither paper estimates the combined product, and the project's H5 test crossed Byrne's N with the loss domain and came back null.
- [[palm-2025-influence-personality-traits]] — contradicts (construct-mediated) — [[byrne-2014-chokes-under-pressure]]: Byrne is the source for the positive N weight (N degrades decisions under pressure); Palm & Tims rank N the least susceptible to persuasion. Same trait, opposite practical direction, because the constructs differ.
- [[amsalem-2018-effect-politicians-personality]] — complements — [[byrne-2014-chokes-under-pressure]]: Both bear on the receptivity N slot: Amsalem's emotionally stable MPs get more coverage, which implies interview-derived N is missing not at random; Byrne supplies the pressure mechanism the slot is cited to.
- [[amsalem-2018-effect-politicians-personality]] — in tension within the system — [[palm-2025-influence-personality-traits]]: Palm ranks agreeableness most susceptible (supporting the +.40 slot), while Amsalem finds low-A MPs more visible; since visibility raises seed_prob, the two feed the receptivity layer with opposite A signs.
- [[thakur-2025-matching-theory-perspective]] — parallel argument — [[best-2022-redistricting-commissions-avoid]]: Both show that a system input treated as a member or district attribute (committee seat, district partisan lean) is partly produced by institutional procedure (party matching mechanism and seniority queue; map drawing shifting lean 1.5-4 points).
- [[noble-2024-presidential-leadership-legislative]] — same method family — [[thakur-2025-matching-theory-perspective]]: Both are formal or mechanism-design models of congressional institutions whose central predictions go untested in the paper (Noble estimates nothing; Thakur cannot test Predictions 1, 2(a) or 3(b)).
- [[esmalian-2022-characterizing-equitable-access]] — complements — [[zeller-2016-step-path-selection]]: Both infer movement cost or access from observed movement rather than straight-line distance or supply counts; together they are relevant to any travel-cost weighting in the Terrier turf planner.
- [[zeller-2016-step-path-selection]] — parallel caution — [[zhan-2025-cost-minimized-stage]]: Both turn tie or proximity weights into derived quantities (an additive path cost; neighbour imputation weights) through a transform that is assumed rather than validated on real data.
- [[zhan-2025-cost-minimized-stage]] — same evidentiary design — [[zeng-nd-multi-agent-reinforcement]]: Both rest on one worked or simulated scenario with no holdout, replicates or variance (Zhan: 20 decision-makers, synthetic MSE; Zeng: 17 UAVs, 3,500 steps, undescribed baseline).
- [[anon-2022-simulating-role-knowledge]] — same method (agent-level simulation) — [[zeng-nd-multi-agent-reinforcement]]: Both are agent-level simulations of multi-actor systems. Anon's design cannot be compared until its text is recovered.

## What it means for the systems

The verification status of each bearing comes from the theme file. UNVERIFIED bearings are single-agent leads, not findings (DE-062).

**Melange influence engine: receptivity layer** ([[systems/melange-influence-engine]], [[systems/melange-psychometrics]]).
- **Byrne** qualifies the susceptibility composite (VERIFIED-SOFTENED). It is cited for N = +.30, yet its strongest trait (N) carries less weight than the weaker A (+.40). Keep N labelled UNVERIFIED in light of H5 (partial r = +0.036, n = 441).
- **Linde** qualifies `pt_mult` 1.15 / 0.92 and contradicts the loss-aversion rationale for money-response `LOSSFRAME` = 1.10 (VERIFIED-SOFTENED).
- **Palm** offers weak evidence against the N and E signs, and shows the engine has no persuasion-strategy dimension (UNVERIFIED).
- **Amsalem** qualifies three things (VERIFIED / VERIFIED-SOFTENED):
  - the visibility disposition behind `seed_prob`;
  - the extraversion-sociability label;
  - the neutral fill for members with no airtime, which is missingness that is not random.

**Salience shock / exec cue** ([[systems/melange-influence-engine]], [[systems/melange-stance]]).
- **Noble** is the method source (VERIFIED-SOFTENED). Several mappings qualify it: rally and retreat odds come from the stance gap, psi from missing stance data, and the reelection channel is moved onto legislators and a CU actor.
- The claim that the advisory reverses the defensive-appeal prediction is UNVERIFIED and needs a second path before anyone acts on it.

**Committee / Influence pillar** ([[systems/moneyball-rank-pillars]], [[systems/melange-legislative-data]]).
- **Thakur** qualifies the pillar's party- and chamber-blind tier weights (VERIFIED-SOFTENED).
- It supports dropping committee gatekeeping (path J) from the say-do gate.
- Any test of the pillar against outcomes should be stratified by party and chamber, or labelled UNSTRATIFIED.

**Competitiveness multiplier (c = 1 + exp(-lean/5))** ([[systems/mentat-fec-attribution]], [[systems/melange-allocation]]).
- **Best** shows map-drawing moves district lean by about the size of the 5-point scale constant (UNVERIFIED).
- It does not test whether dollars buy more in competitive seats.
- It weakly echoes the Census scraper's mix of 116th- and 119th-Congress district boundaries.

**Path-finding (staffer routes, member recruitment)** ([[systems/melange-pathfinding]], [[systems/melange-staffer-network]]).
- **Zeller** qualifies the `-log p` / `-ln(0.8*strength)` cost inversion and suggests a used-vs-available calibration (UNVERIFIED).
- Route costs stay heuristic model outputs, never closeness, access or introduction probabilities (D-025).

**Stance dynamics** ([[systems/melange-stance]], [[systems/melange-sna]]).
- **Zhan** suggests trust- and similarity-weighted neighbour imputation as an extension (UNVERIFIED).
- The co-movement null on cosponsor ties undercuts the premise, and treating cosponsor weights as trust would break D-025.
- None of these papers argues for the two REJECTED SNA proposals (dropping regular cosponsorship edges; per-edge volume normalization). Nothing here should be read as reopening them.

**Demographic Potential v3.0 and the Terrier turf planner** ([[systems/melange-demographic-potential]], [[systems/terrier-canvass]], [[systems/terrier-imputation]]).
- **Esmalian** weakly qualifies branch- and deposit-share "potential" and walking-distance ranking: supply counts and plain distance are weak stand-ins for realized access (UNVERIFIED, analogical).

**Allocation joint optimizer** ([[systems/melange-allocation]]).
- **Zeng** is context only (UNVERIFIED). It is not evidence that RL would beat greedy allocation.

**Net effect.** No work in this theme validates a Melange constant. The actionable items are:
1. Relabel or soften the citations behind 1.15 / 0.92 / 1.10 and the N weight.
2. Re-verify the salience advisory's offense/defense logic through a second path.
3. Stratify any committee-pillar test.

## Gaps

- **No theme-level notes from Paul.** The category has no stated purpose, so it works as an overflow bin. The four receptivity-psychology papers could move to a psychometrics or receptivity theme; [[thakur-2025-matching-theory-perspective]] and [[best-2022-redistricting-commissions-avoid]] could move to an institutions theme.
- **[[anon-2022-simulating-role-knowledge]] is unreadable.** Re-extract its text (for example with Poppler `pdftotext` in a new session) and re-run the relation stage. Paul's 82 highlights, concentrated on pp. 4 and 10, suggest it matters to him.
- **No US Congress evidence on legislator personality and persuadability.** Nothing in the theme links Big Five traits, measured by text, experts or self-report, to members actually moving under lobbying. Only that would validate the susceptibility weights. Expert ratings and text-based measures exist among the linked items but are outside this theme.
- **No persuasion-strategy dimension.** Palm's main result (matching strategy to trait) has nothing in the engine to attach to, and no work tests strategy-matching among elites.
- **No empirical test of Noble's predictions.** Nothing tests the defensive-appeal asymmetry, or the effects of polarization (z) and uncertainty (psi) on going public, on real presidential appeals.
- **No House analysis of committee assignment.** Thakur is Senate only, uses 1947-2017 data and has no causal identification.
- **No link from district competitiveness to money efficacy.** Best addresses the lean measure, not the premise that dollars buy more in competitive seats.
- **No used-vs-available outreach or route data.** Zeller's calibration template cannot be applied to staffer or recruitment routes until such a log exists.
- **No real-data test of the homophily premise behind neighbour imputation.** Zhan uses only synthetic data, and the system's own co-movement test on cosponsor ties was null.
- **No replicated comparison of a learned allocation policy with a greedy baseline** on political data.

## All works

- [[noble-2024-presidential-leadership-legislative]] (2024) — Presidential Leadership and Legislative Polarization
- [[linde-2016-politicians-take-risks]] (2016) — Do Politicians Take Risks Like the Rest of Us? An Experimental Test of Prospect Theory Under MPs
- [[anon-2022-simulating-role-knowledge]] (2022) — Simulating the role of knowledge brokers in policy making in state agencies: An agent-based model
- [[byrne-2014-chokes-under-pressure]] (2014) — Who chokes under pressure? The Big Five personality traits and decision-making under pressure
- [[palm-2025-influence-personality-traits]] (2025) — The influence of personality traits on the effect of persuasion strategies: A systematic literature review
- [[zhan-2025-cost-minimized-stage]] (2025) — A cost-minimized two-stage three-way dynamic consensus mechanism for social network-large scale group decision-making: Utilizing K-nearest neighbors for incomplete fuzzy preference relations
- [[thakur-2025-matching-theory-perspective]] (2025) — A matching theory perspective on legislative organization: assignment of committees
- [[best-2022-redistricting-commissions-avoid]] (2022) — Do Redistricting Commissions Avoid Partisan Gerrymanders?
- [[amsalem-2018-effect-politicians-personality]] (2018) — The Effect of Politicians' Personality on Their Media Visibility
- [[zeng-nd-multi-agent-reinforcement]] () — Multi-Agent Reinforcement Learning-Based Extended Boid Modeling for Drone Swarms
- [[zeller-2016-step-path-selection]] (2016) — Using step and path selection functions for estimating resistance to movement: pumas as a case study
- [[esmalian-2022-characterizing-equitable-access]] (2022) — Characterizing equitable access to grocery stores during disasters using location-based data
