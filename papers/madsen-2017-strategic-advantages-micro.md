---
type: paper
title: "The strategic advantages of micro-targeted campaigning: A proof of principle Bayesian Agent-Based Model"
authors: ["Jens Koed Madsen (University of Oxford)", "Toby D. Pilditch (University College London)"]
year: "2017 per the metadata hint; the text itself shows no year (its latest cited work is Madsen 2016)"
venue: "Not stated in the text. Page numbers run 2651 to 2656, which suggests a proceedings or journal volume, but no venue name appears."
doi: ""
category: "Campaign Politics"
systems: ["melange-allocation", "melange-influence-engine", "terrier-canvass"]
components: ["terrier-canvass.untapped-potential", "melange-influence-engine.seed-gate", "melange-allocation.stance-money-gate"]
paul_annotated: false
paul_markup_count: 0
evidence_status: unverified
source: "[[_sources/madsen-2017-strategic-advantages-micro.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\Strategic advantages of micro-targeted campaigns - Madsen & Pilditch 2017.pdf"]
sha12: ["ef4355b0bd91"]
tags: [paper, campaign-politics]
---

# The strategic advantages of micro-targeted campaigning: A proof of principle Bayesian Agent-Based Model

**Jens Koed Madsen (University of Oxford), Toby D. Pilditch (University College London) (2017 per the metadata hint; the text itself shows no year (its latest cited work is Madsen 2016)).** *Not stated in the text. Page numbers run 2651 to 2656, which suggests a proceedings or journal volume, but no venue name appears..* Theme: [[themes/campaign-politics|Campaign Politics]].

> [!abstract] Summary
> Madsen and Pilditch build a small agent-based model as a proof of concept. Two candidates compete over 50 ticks for 10,000 simulated voters who are not connected to each other. Each contacted voter replaces its belief with a Bayesian source-credibility posterior. Credibility is split into expertise and trustworthiness, and the conditional probabilities come from Madsen (2016) with no free parameters. The micro-targeted campaign contacts only voters whose mean credibility score for it is above 0.5, 20 per tick. The stochastic campaign contacts voters at random, with reach set to 1 to 10 times the targeted campaign's.
> 
> Results:
> - At equal reach, targeting wins.
> - When mean credibility is 0.5, random contact is wasted, because contacts land on favourable and unfavourable voters in equal numbers.
> - A random campaign with about twice the reach cancels the targeting advantage.
> - Above a 2x reach ratio, and with credibility above 0.5, the random campaign wins.
> 
> All results come from one figure. The paper reports no replications, no intervals and no sensitivity analysis, and it does not model turnout, multiple issues, or voter-to-voter networks.
> 
> For Paul's systems this is weak evidence about mechanism, not a calibration source. It agrees with two design choices: contacts go only to receptive targets, and money is never spent trying to convert opponents. Both match the paper's finding that blind contact with low-credibility audiences is inefficient or harmful. It also says the advantage of targeting depends on reach, which no part of the system models. The closest analogue is Terrier's untapped-potential score for door canvassing. That score gives persuasion weight to undecided likely voters but carries no credibility or receptivity term for the messenger. That missing term is the variable that decides the paper's result. The paper does not touch the cosponsorship network, LDA, reciprocity, or the rank pillars. It gives no reason to change a constant. Every number it reports is a simulation output, not an estimate.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Does a micro-targeted campaign (MTC), which contacts only voters with a favourable view of the candidate's credibility, persuade the electorate more efficiently than a stochastic campaign that contacts voters at random? Can an agent-based model with a Bayesian source-credibility belief-updating rule simulate persuasion campaign strategies? The paper frames itself as an exploratory proof of concept.

**Theory:** Bayesian source-credibility model (Harris et al., 2015; Bovens & Hartmann, 2003; Hahn et al., 2009/2012). Credibility is split into expertise (E, capability to give accurate information) and trustworthiness (T, willingness to give accurate information). The two are treated as orthogonal. Beliefs are revised with an expanded Bayes rule, p(h|rep), using conditional probabilities from Table 1 taken from Madsen (2016), so belief revision has no free parameters. Political campaign theory: the persuasion phase of a campaign is modelled separately from get-out-the-vote (Green & Gerber, 2008). Micro-targeting background draws on Issenberg (2012) and O'Neil (2016).

**Data:** No empirical data. The study is a simulation. Units are simulated voters (agents). N = 10,000 voters per simulation, 2 candidates, 1 binary proposition (H = 0 or 1), 50 ticks (campaign days). Voter expertise and trustworthiness scores are drawn from normal distributions with mean 0.5 or 0.6 and SD 0.25. Voter prior belief is drawn from normal (mean 0.5, SD 0.25), bounded 0.01 to 1. The MTC candidate's reach is fixed at 20 contacts per tick. The non-MTC reach ratio runs from 1 to 10. The text does not report the number of replications or random seeds. The conditional probability table (Table 1) is garbled in the extraction, with values 80.38, 58.21, 34.63, 18.04, 22.59, 42.3, 59.90, 71.26 and no clear units.

**Method:** Agent-based model (ABM) with two agent types, voters and persuaders. Persuaders are either MTC or stochastic (non-MTC). Voters are randomly distributed, with no spatial role, and have no links to each other. Persuaders make direct contacts. The non-MTC candidate contacts voters at random and states p(candidate) = 1. The MTC candidate contacts only voters whose signal factor (mean of T and E) exceeds 0.5, sampling randomly among eligible voters up to its reach, and states p(candidate) = 0. Each contacted voter replaces its belief with the posterior P(candidate|rep). A voter is contacted at most once per tick. At the end, each voter votes for the candidate it favours (p > 0.5). Design: 2 x 2 manipulation of mean credibility (0.5 or 0.6) crossed with non-MTC reach ratio 1 to 10. Outcome is the share of voters supporting the non-MTC candidate. Results are presented as a single figure (Fig. 2). No statistical tests, confidence intervals or sensitivity analysis are reported.

**Identification:** None in the causal-inference sense. The mechanism is built in by design: the MTC selection rule and the Bayesian update are fixed, and the two manipulated inputs are credibility mean and reach ratio. The authors frame the study as a proof of concept and do not claim identification of real-world effects.

**Scope:** Population: a simulated electorate of 10,000 homogeneous-process voters. Institution: a stylised two-candidate, single-proposition election with a persuasion phase only. Turnout is not modelled, since all voters vote with probability 1 at the end. Era: not stated. The model is motivated by contemporary micro-targeting in US-style campaigns, citing Issenberg (2012) and O'Neil (2016). The model does not include negative campaigning, multiple issues, voter networks or personality differences.

## Key findings

- Under the Bayesian source-credibility rule, an MTC with the same reach consistently beats a stochastic campaign. (Abstract, p. 2651; Discussion, p. 2655) — MTC reach fixed at 20 per tick; non-MTC reach ratio 1 to 10 (ratio 1 = equal reach). Exact vote shares are only in Fig. 2 and are not given in the text.
- With mean credibility 0.5, a stochastic (non-MTC) campaign is inefficient. Opinions are equally split, so blind contacts reach supporters and opponents in equal numbers. Blind campaigns only work when the candidate's average perceived credibility is above 0.5. Below that, they can be ineffective or harmful. (Results, p. 2654 (first conclusion)) — Mean credibility 0.5 versus 0.6 (SD 0.25).
- A non-MTC campaign with roughly double the reach cancels out the MTC advantage. (Results, p. 2654 (second conclusion)) — Break-even at non-MTC reach about 2x the MTC reach (MTC reach = 20).
- If the non-MTC reach ratio exceeds 2 and the non-MTC candidate's perceived credibility is above 0.5, the stochastic campaign beats the strategic one. (Results, p. 2654) — Reach ratio > 2; credibility mean > 0.5.
- Discussion restates that the MTC advantage is not general: a positively perceived stochastic candidate with greater reach can beat a strategic one. (Discussion, p. 2655) — Non-MTC needs double reach to cancel the advantage.
- The authors predict, without testing, that MTC efficiency rises with available voter data and with electorate complexity (belief systems, individual differences, voter-to-voter interaction). MTC costs are also predicted to rise with complexity. (Discussion, p. 2655) — None reported; this is a prediction only.

## Limitations

- The authors call the study exploratory. The model is 'necessarily simplified' and a 'proof of concept' (p. 2651, p. 2655).
- Voters are cognitively homogeneous, with the same belief-revision process. There are no moral foundations, personality or reasoning-strategy differences (p. 2655).
- Voters consider a single proposition, while real elections involve many issues, preferences and candidates (p. 2655).
- The MTC segments only on candidate favourability, not on swing/secure status, policy preference or turnout likelihood (p. 2655).
- Voters cannot communicate or influence one another. Social networks, community leaders and social nodes are absent, though the authors suspect MTCs would do better with them (p. 2653, p. 2655).
- Real campaigners do not get a perfect signal from each voter. Noise was added to the voter signal, but the amount and form of that noise are not described in the text (p. 2655).
- Turnout is not modelled. All voters vote with probability 1 at the end (p. 2652).
- The conditional probabilities are taken from Madsen (2016) and not calibrated to this electorate (p. 2653).
- No replications, confidence intervals, statistical tests or sensitivity analysis are reported. Results rest on one figure (Fig. 2).
- Negative campaigning is not implemented (footnote 3, p. 2654), and the model is not tested against real campaign data.
- Footnote 2 and the Table 1 layout are garbled in the extracted text, so the conditional-probability values cannot be checked against the printed table with confidence.

## Short quotes

- "the micro-targeted campaign consistently beat stochastic campaigns with the same reach" (Abstract, p. 2651 (Madsen & Pilditch 2017). Capped at one quote under 15 words per the copyright rule; the other candidate quotes were dropped.)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/terrier-canvass#Untapped potential\|Untapped potential]] | qualifies | Persuasion contact with voters who give the messenger credibility at or below 0.5 is inefficient or harmful. Targeting only receptive voters (signal factor above 0.5) beats random contact at equal reach; random contact needs about 2x the reach to break even. Simulation only: N=10,000 agents, 50 ticks, targeted reach 20 per tick. (Results p. 2654; Discussion p. 2655; Fig. 2) | potential = (wp x (1-\|2s-1\|) x t + (1-wp) x s x (1-t)) x (1 + wi x i). The persuasion weight peaks for undecided likely voters (s = 0.5). The formula has no term for how credible the canvasser or candidate is to the person being contacted, and no term for backfire. `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/tools/field_labels.py:105-118; C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/untapped.py:1-12` | Partial transfer. Both concern voter-level persuasion targeting in a two-candidate race. The paper, however, uses unconnected simulated agents, a single proposition and no turnout, and its credibility values are drawn from distributions, not measured. It supports adding a messenger-credibility or backfire guard as a hypothesis to test against canvass results. It does not support any particular weight. Strength is weak because these are uncalibrated ABM outputs with no replications reported. | weak | UNVERIFIED |
| [[systems/melange-influence-engine#Contingent seed activation gate\|Contingent seed activation gate]] | supports | Under Bayesian source-credibility updating, contacts restricted to receptive targets (credibility signal above 0.5) convert better than untargeted contacts. Contacting audiences who view the source unfavourably moves them away from the source's position. (Model p. 2653; Results p. 2654) | gate(n) = max(0.15, seed_prob x SGATE[tier]). SGATE falls from 1.00 for Highly Persuadable to 0.35 for Opponent, so the system already expects contacts with unreceptive targets to convert at lower rates. The model has no negative (backfire) outcome for a contact. `fec/moneyball_core.py:626-630; fec/members/receptivity_layer.py:212-216` | Directional agreement only. The paper's agents are voters, not legislators. Its unit is a source-credibility belief update, not a take-up decision about cosponsoring, and it estimates no take-up rates. The SGATE values remain unestimated design constants; this paper does not calibrate them. | weak | UNVERIFIED |
| [[systems/melange-allocation#Stance money gate (F1)\|Stance money gate (F1)]] | supports | Contact spent on audiences with an unfavourable view of the source is wasted or counterproductive. The targeted campaign's advantage comes entirely from never contacting those voters. (Results p. 2654 (first conclusion); Discussion p. 2655) | Members whose overall tier is Skeptic, Highly Skeptical or Opponent receive zero money in every allocator, which funds allies instead of trying to convert opponents. The gate fails open for members with no matrix row. `fec/moneyball_core.py:713-729; fec/engine/optimize.py:44-52` | An analogy only. Campaign contacts with voters are not PAC dollars to legislators, and the paper has no money mechanism. It agrees with Paul's Furnas reading ('cultivate champions') but adds no new evidence about legislator behaviour. | weak | UNVERIFIED |

## Gaps and flags

- Terrier's untapped potential has no messenger-credibility or backfire term. In the paper, the share of the audience that views the source favourably is the variable that decides whether contact helps or hurts.
- No component models the trade-off between reach and targeting: how much more random reach makes up for targeting precision (the paper's break-even is about 2x). The allocators and turf planner treat contact capacity as a fixed budget, not a strategy choice.
- None of the influence-engine contact models (SGATE, PERS) has a negative outcome for a contact. A contact with an Opponent only converts with low probability; it never pushes the member further away.
- The paper's results are simulation outputs from a single figure, with no replications, intervals or sensitivity analysis reported. None of its numbers (the 0.5 credibility threshold, the 2x break-even reach) should be quoted as empirical estimates or used to set SGATE, PERS or Terrier weights. If cited, it should be labelled as a theoretical proof of concept.
- Per DE-062, this agent analysis is UNVERIFIED until a second path reproduces it.
- The paper does not argue for either SNA proposal this project has rejected (dropping regular cosponsorship edges; per-edge volume normalization). It has no network at all, so there is nothing to flag there.

Paul has not annotated this paper. It agrees with his Furnas-based position that ideological money cultivates champions rather than recruiting them, and with his seeding view taken from Chen et al. (contingency-aware seeding: legislators may or may not move when approached). Both are already built as the stance money gate and SGATE. His Broockman & Kalla and Kalla & Broockman highlights on the door canvass are about how the message is delivered. This paper adds a different dimension: who the messenger is to the voter. Terrier's potential formula encodes neither. Nothing in his notes is out of date relative to this paper.

## Related papers

- [[tappin-2023-quantifying-potential-persuasive]] — complements — Tappin measures persuasive returns to microtargeting with experiments; Madsen and Pilditch only simulate them. Tappin is the empirical check on this paper's claim that targeting has an advantage.
- [[simchon-2024-persuasive-effects-political]] — complements — Simchon also studies the persuasive effects of political microtargeting, with generative-AI messages. It offers the empirical evidence this paper does not have.
- [[kalla-2023-which-narrative-strategies]] — complements — Field evidence on persuasion from door canvassing, the real-world setting behind Terrier's untapped-potential score, which this paper treats only abstractly.
- [[chen-2021-contingency-aware-influence]] — complements — Both make contact conditional on the target's receptivity. Chen does it as contingency-aware seeding, which the system implements as SGATE; this paper does it as credibility-gated contact.
- [[cai-2025-competitive-influence-maximization]] — extends — A two-candidate competitive voter-dynamics model on networks. It adds the voter-to-voter interaction this paper leaves out and says targeting would benefit from.

### Linked from
- [[broockman-2026-should-moving-middle]] — complements — An agent-based model of micro-targeted campaigning. Issue-specific, voter-position-dependent responses of the kind measured here are the empirical input such a model would need.
- [[anon-2022-simulating-role-knowledge]] — same-method — Both build agent-based simulation models of a political process (micro-targeted campaigning in Madsen; knowledge brokering in state agencies here), so they are the vault's two ABM references for any future agent-level extension of the influence engine.
- [[cagala-2021-optimal-targeting-fundraising]] — complements — Item file title matches Madsen and Pilditch 2017, an agent-based proof of principle for micro-targeted campaigning; this paper supplies the field-experimental counterpart with net-of-cost numbers.