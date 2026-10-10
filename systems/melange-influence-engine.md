---
type: system
name: "Melange influence engine (persuasion, money and cascade physics)"
slug: melange-influence-engine
components:
  - melange-influence-engine.cascade-kernel
  - melange-influence-engine.competitive-cascade-crn
  - melange-influence-engine.seed-gate
  - melange-influence-engine.flip-model
  - melange-influence-engine.persuadability-table
  - melange-influence-engine.money-response-defection
  - melange-influence-engine.lt-priming
  - melange-influence-engine.salience-shock
  - melange-influence-engine.receptivity-layer
  - melange-influence-engine.competitiveness-multiplier
  - melange-influence-engine.electoral-plays
  - melange-influence-engine.scenario-runner
  - melange-influence-engine.priority-engagement
brain_notes:
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-influence-engine-internals.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-influence-engine-internals-receptivity-layer.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-influence-engine-internals-salience-sna-wave.md
  - C:/Users/pgoff/Documents/brain/topics/2026-06-feature-moneyball-excel-model-build-scenario-tools-and-calibration.md
paper_count: 73
tags: [system]
---

# Melange influence engine (persuasion, money and cascade physics)

Behavioural simulation layer that sits downstream of the rank. It covers the shared cascade kernel and partisan edge factors, competitive independent cascade with common random numbers, contingent seeding, the flip/winnability model, money-to-lean response and defection, linear-threshold priming, salience shocks, the receptivity (susceptibility) layer, the competitiveness multiplier, electoral plays and the scenario runner. Core code: fec/moneyball_core.py and fec/engine/*.py.

Brain notes: [topics/2026-07-feature-influence-engine-internals](obsidian://open?vault=brain&file=topics/2026-07-feature-influence-engine-internals); [topics/2026-07-feature-influence-engine-internals-receptivity-layer](obsidian://open?vault=brain&file=topics/2026-07-feature-influence-engine-internals-receptivity-layer); [topics/2026-07-feature-influence-engine-internals-salience-sna-wave](obsidian://open?vault=brain&file=topics/2026-07-feature-influence-engine-internals-salience-sna-wave); [topics/2026-06-feature-moneyball-excel-model-build-scenario-tools-and-calibration](obsidian://open?vault=brain&file=topics/2026-06-feature-moneyball-excel-model-build-scenario-tools-and-calibration)

**Melange influence engine: evidence assessment** (hub file `melange-influence-engine.json`, read 2026-10-10)

The engine is the behavioural simulation layer that sits downstream of the rank. It has 13 components. The hub file holds 115 evidence rows: 34 VERIFIED, 68 VERIFIED-SOFTENED, 13 UNVERIFIED. Only VERIFIED and VERIFIED-SOFTENED rows count as evidence here. UNVERIFIED rows, and anything from the online scan, are single-agent leads (DE-062). The rows break down as 65 qualifies, 14 context, 14 supports, 13 suggests-extension, 4 contradicts, 4 method-source, 1 measurement-source. They cite 73 distinct papers.

**Bottom line.** The literature supports the engine's architecture and the direction of most of its terms:
- an independent-cascade or threshold diffusion backbone;
- co-partisan sources counting for more than out-partisan ones;
- money as a weak lever on positions;
- seeds that may not take up an invitation;
- Noble-Turner's three going-public regimes.

Almost none of the constants come from evidence. That covers the saturation constant 6, cpf 1.7/0.5, the 0.4 cross-party floor, SGATE, PERS, WM/WP, HALFF, MAXSHIFT, DTHRESH, LOSSFRAME, the receptivity weights, the 1.15/0.92 and 0.60 multipliers, VULN 1.35, and the AMPLITUDE and SCALE of c. Each one is either owner judgment or a simulation default borrowed from a non-legislative paper (for example, Chen 2021's q = 0.6 behind the 0.60 seed_prob base). That is acceptable if the code labels them as design priors. It is not acceptable where the code labels them CALIBRATED (WM/WP, MAXSHIFT) or where a citation runs against the dial.

**Four rows contradict the system outright:**
1. **LOSSFRAME 1.10 cites Linde & Vis 2016, which found *no* loss aversion in MPs.** n = 46 Dutch MPs, 2014-15.
2. **The salience advisory gives defensive credit on status-quo-unfavourable (sq = offense) issues.** Noble & Turner 2024 say the actor never goes public in that case. The live advisory (salience_advisory.json, 2026-10-08, 18 issues) ranks Technology & Emerging first under GO PUBLIC.
3. **Out-party cues persuade negatively in the evidence, but the engine gives them positive pull.** Bakker et al., mass public, four studies, pooled out-party effect about -.07 on a 0-1 scale. The engine gives out-partisans +0.5 in flip and kernel pressure.
4. **The competitive cascade breaks same-hop ties deterministically in favour of the pro side.** Bharathi et al. design ties away, so this is a departure from the cited model. It may inflate pro reach; the effect is unmeasured.

**Components with essentially no direct evidence:** the priority/leave-one-out orientation (one weak row), the scenario runner's 'defection is a general disposition shift', and the premise that a dollar buys more in a competitive seat. That premise sits under both the competitiveness multiplier and the electoral plays. Reciprocity stays NOT validated (D-104 to D-108). Every row filed under c is evidence about where donors send money, not about what a dollar buys.

**Transfer is the main limit throughout.** The closest causal evidence on lobbying and cue-taking comes from state legislatures, mass-public survey experiments, EU Parliament committees and simulations. It does not come from the US Congress on credit-union issues. The one congressional field experiment, Kalla & Broockman (191 House offices, one party, June-Aug 2013), measures access only.

**Online scan (leads only, UNVERIFIED, not yet in the hub file):**
- Camp, Schwam-Baird & Zelizer: null effects of lobbyist outreach on cosponsorship across four field experiments in two state legislatures.
- Zelizer: briefing-spillover field experiment in a state legislature. The size of the indirect effect is reported inconsistently in secondary sources, 60% versus 80% of the direct effect.
- Rogowski & Sinclair: House office lottery; no proximity effect on roll calls or cosponsorship, 105th-112th Houses.
- Coppock: null cue-taking among similar legislators.
- Fong (JOP): cue-taking from expert peers, reportedly crossing party lines.
- Box-Steffensmeier, Christenson & Craig (AJPS 2019): Dear Colleague endorsements grow cosponsor lists.
- Simkin & Roychowdhury (arXiv 1001.3732): threshold/sandpile model of cosponsor timing on H.R. 1207.
- Levitt 1998: PAC returns run through the election channel.
- A 2024 AEA conference paper: the money-vote relation is stronger under electoral pressure, identified from close incumbent defeats.

The scan found no study that reports take-up of lobbying contact by the target's prior position, which is the quantity SGATE and PERS encode.

No PDFs were downloaded in this pass. The scan surfaced titles and URLs only.

## Cascade activation kernel build_P and partisan edge factors

**What it computes.** p(u->v) = x/(x+6) with x = w x cpf x edge_mult(v). cpf_diffusion = 1 for same party and 0.4 + 0.6 sqrt(bpf_u bpf_v) across D/R. cpf_pressure = 1.7 co-partisan, 0.5 out-partisan.

**Method.** One shared kernel for cascade, coalition, frontier, scenario, priming and route engines. bpf is the bipartisan share (default 0.24). The two physics factors are deliberately never merged.

**Code refs.** `fec/moneyball_core.py:1-12,68-72,543-562`

**Assumptions**
- Cross-party contagion flows through bridge-builders
- Co-partisan sources persuade more and out-partisan sources less (Furnas/Bakker source alignment)
- Saturation constant 6 is a design choice, not estimated
- Receiver susceptibility, not sender loudness, governs activation

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[furnas-2019-moneyed-interests-information]] | 2019 | qualifies | Pooled over all staff, minimal evidence that think-tank evidence raises siding with the requester (if anything, less likely than 'own analysis'). Split by party, aligned think-tank evidence raises siding and non-aligned lowers it, and for both parties the non-aligned penalty is roughly twice the aligned benefit (Fig. 4). The reference category is stated inconsistently: 'own analysis' in the p.12 prose, 'supportive polling' in the Fig. 4 note. Stated intentions; N=441 staff (5.2% response), 549 ratings per factor; party subgroup Ns not reported. | The paper randomises the ideology of an outside requester's evidence and records DC staffers' stated likelihood of siding with it (2017, single hypothetical). The system applies the factor to legislator-to-legislator cosponsor edges, where the partisan relation is between two Members, not an evidence source. That supports the direction of a source-alignment term, but not its 3.4 ratio or the gain-over-penalty asymmetry. If anything, the paper points to a penalty larger than the gain. The pooled estimate is null over the party fault line, so any figure taken from the paper must be quoted by party stratum or labelled UNSTRATIFIED. | moderate | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | supports | Citing Coleman, Katz and Menzel (1966): some sources inform (mass media, drug salesmen) while others legitimate adoption; the legitimating sources, other doctors, were most influential, and close reciprocated ties carried legitimacy. Citing Granovetter (1973): over 80 percent of job information came from occasional or rare contacts, under 17 percent from frequent contacts. | Conceptual parallel only. The cited evidence concerns physicians and job seekers, not legislators, and the article gives no Coleman sample size or data-collection dates. It supports separating an information channel from a legitimation channel. It does not support the 1.7 and 0.5 ratios, the 0.4 floor or the constant 6. Coleman's reciprocated ties must not be imported as a reciprocity weight, since reciprocity is not validated (D-104 to D-108). | weak | VERIFIED |
| [[gaynor-2022-financial-ties-bind]] | 2022 | suggests-extension | Within a party, Model A (both members in the same caucus) is positive for Freedom +1.208*** and Tea Party +0.686*** (R), and Progressive +0.381* (D, p<0.1). It is negative for Tuesday Group -1.033*** and Liberty -0.389*. Model D (recipient in caucus, donor not) is -1.020*** for Freedom and -0.545*** for Tea Party. Model C (donor in caucus, recipient not) is -1.780*** for Freedom and -0.314*** for Tea Party. Mean transitivity R 0.497, D 0.2768. | Evidence is binary LPAC donation ties with amounts ignored, not cosponsorship diffusion or persuasion, so it cannot set a faction multiplier. It shows party is not a homogeneous block for money ties, and the fault line is party-specific: any pooled faction coefficient needs party strata. 115th House primary; 116th replication is partial (Tables A1-A4). | weak | VERIFIED-SOFTENED |
| [[trivedi-2020-efficient-influence-maximization]] | 2020 | qualifies | Structure-derived edge probabilities p_ij = 0.01 + (d_i+d_j)/n + CN(i,j)/n raise spread for every algorithm on the co-authorship graphs, e.g. Degree +32.60% (NetPhy, k=20) and CELF +55.55% (NetPhy, k=10). The form is asserted to reflect real propagation dynamics and is not fitted to or validated against observed diffusion. | The paper gives no calibration or validation, so it cannot support tuning or replacing the constant 6. Its gains are percent increases over a 0.01 baseline that it says gives very little influence and does not favour real-world dynamics; calling the gains mechanical is this analysis's reading, not the paper's. The common-neighbour term is a candidate covariate only. The degree term raises probability for hubs and must not be presented as support for the REJECTED per-edge volume normalization. | weak | VERIFIED-SOFTENED |
| [[bakker-nd-understanding-partisan-receptivity]] |  | contradicts | Out-party cues lower policy support by about .06 (study 1, p < .01, N = 747), about .07 (study 2, no p-value stated in the text) and about .07 in the pooled studies 1-4 meta-analysis (no p-value stated). In-party cues add about .03, non-significant in studies 1 and 2 and reported without a test in the pooled model. The out-party sign is negative, so the paper supports no positive out-partisan persuasion weight. | Direction only. The paper measures mass-public 0-1 policy support after one embedded cue, on low-salience issues, in 2016-2018 non-probability online panels. It does not measure legislator-to-legislator pressure, cosponsorship or lobbying. The sign result supports zeroing or reversing the out-partisan weight, not the 1.7 amplification, since the paper's in-party effect is small and often null. Legislator-side constants need elite cue evidence such as Box-Steffensmeier et al. | moderate | VERIFIED-SOFTENED |
| [[ringe-2013-pinpointing-powerful-voting]] | 2013 | qualifies | Among opposing-bloc MEP dyads, higher point connectivity predicts less co-voting. Table 3 model 3 log-odds multilevel coefficient is -0.1020 (SE 0.0307), about 10 percent lower odds per unit (95 percent CI 7-13 percent). Predicted co-voting falls from 52 to 44 percent between connectivity 0.6 and 3.8 (about -1 to +1 SD). Window: 40 Environment Committee roll calls, July 2004 to July 2008. Denominator: 1,319 non-unanimous dyads (perfect co-voters excluded in model 3). | Paper tie is staff-reported contact, not cosponsorship. Setting is a multi-party EP committee, not two-party US Congress. Result is cross-sectional association with acknowledged simultaneity and specification sensitivity (fn. 81). It does not show cross-party diffusion is zero or negative in US cosponsorship. Use it to motivate a test stratified by party pair, not to change a constant. | weak | VERIFIED-SOFTENED |
| [[ringe-nd-pinpointing-powerful-voting]] |  | qualifies | Converting cueing networks into voting data loses network structure: simple networks are recovered best, moderate and complex networks less well, and betweenness recovery is only about 0.25 correlation. | The paper's result concerns recovering node centralities from simulated voting records; it says nothing about cascade edge probabilities or the saturation constant 6. The analyst's claim that kernel edges come from cosponsorship weights was not checked, because load_network was not opened in this pass. Relevance is only as a caution against calibrating kernel edges from roll-call agreement. The evidence is simulation, not US data. | weak | VERIFIED-SOFTENED |
| [[merkley-nd-party-cues-news]] |  | qualifies | Democratic cues move Republicans toward skepticism, away from the cued position, in both the experiment and the time series (backlash rather than attenuated persuasion). Republican cues have no aggregate effect (chi2 0.00, p=0.94) and only a weak effect on Republican skepticism (chi2 2.94, p~0.09). The authors say co-partisan sources dominate. | The paper supports co-partisan sources dominating, which fits the 1.7 versus 0.5 ordering. It reports backlash, meaning movement away from the cued position, but estimates no multiplier relative to co-partisan influence, so it cannot show the out-partisan sign should be at or below zero rather than 0.5. Cosponsorship ties are observed behaviour rather than cues, so transfer is weak. The paper gives no support for the 1.7/0.5 ratio itself. | weak | VERIFIED-SOFTENED |
| [[zareie-2023-influence-maximization-social]] | 2023 | suggests-extension | Trust-agnostic opinion-aware methods consider that all network relationships are positive (friend), whereas in reality networks may also include negative (foe) relationships. Trust-aware work uses signed graphs on the premise that opinion change depends on trust between users. The survey also warns that numeric friend-or-foe encodings may not capture attitude. | A design taxonomy only, not evidence that signed edges improve prediction of Congressional cosponsorship. Adopting foe edges would need a second path showing that negative references predict lower future cosponsorship (DE-062). The survey's warning on numeric attitude encodings applies to the LLM valence scores. Signed edges would also change the two-physics architecture, so the owner decides. | weak | VERIFIED-SOFTENED |
| [[chen-2023-complex-contagion-influence]] | 2023 | context | IC and LT assume activation probability independent of how many neighbours are active, which keeps them submodular. Complex contagion breaks this (Sec. 3, lines 254-258 and footnote 2). | Context only. The paper gives no legislative estimate. Any count-dependent kernel would be a new model and must keep the two cpf physics separate. | weak | VERIFIED |
| [[fink-2023-congressional-social-network-dataset]] | 2023 | method-source | Fink offers the directed pairwise probabilities as empirical transmission probabilities that can vary across edges. The paper argues that Monte Carlo SIR-type benchmarks usually assume one homogeneous, arbitrarily chosen rate. | The mapping holds only for untied pairs and only when physics and edge_mult equal 1. Fink rates are directed and normalized per source tweet, while the system symmetrizes them and reads them as per-campaign activation odds, which changes unit and meaning. The 2.0 cap gives p_max of 0.25. Fink covers one 2022 window and says the rates may not be stationary. | moderate | VERIFIED-SOFTENED |
| [[kempe-2015-maximizing-spread-influence]] | 2015 | qualifies | Edge probabilities are set by rule, not estimated: LT weights from co-authorship multiplicity, IC uniform p of 1 or 10 percent, and weighted cascade p = 1/d_v with no multiplicity term (Sec 8.2, p.133). At p=1% each targeted node adds about three activations, so random seeds do relatively better (Sec 8.4, p.135). Estimating parameters from data is left open (Conclusions, p.138). | Seed rankings are sensitive to the activation level, so a sweep of the constant 6 (for example 3, 6, 12) should accompany any claim that frontier seed order is stable. This is not validation of the constant. | moderate | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: moderate on direction, nil on constants.** 12 rows: 2 VERIFIED, 10 VERIFIED-SOFTENED.

**What holds**
- Fink 2023 (method-source) supports directed, heterogeneous per-edge probabilities over one homogeneous rate. The x/(x+6) kernel returns Fink's p exactly, but only for untied pairs and only when the physics term and edge_mult both equal 1. The Fink data are one 2022 Twitter window and are per-tweet rates, which the system symmetrises.
- Haythornthwaite (VERIFIED, weak) gives a conceptual basis for keeping an information channel separate from a legitimation channel, which is what keeping cpf_diffusion and cpf_pressure apart does. It is not evidence for any ratio.
- Kempe 2015 confirms that edge probabilities in the canonical work are set by rule, not estimated, and that seed rankings are sensitive to the activation level. A sweep of the constant (3 / 6 / 12) should accompany any claim that seed order is stable. That is a robustness check, not validation.

**What does not hold**
- The 3.4 co-/out-partisan ratio has no source.
- Furnas 2019 (stated intentions of DC staff, N = 441 staff, 5.2% response, 2017) shows, by party stratum, an out-aligned *penalty* about twice the aligned *gain*. The system has the opposite asymmetry: +0.7 gain against -0.5 penalty, measured from 1.0. The pooled Furnas estimate is null across the party fault line, so any Furnas figure must be quoted by party stratum or labelled UNSTRATIFIED.
- Bakker (contradicts; mass public, 2016-18 online panels, N = 747 in study 1) finds out-party cues *lower* support by about .06-.07 on a 0-1 scale. In-party cues add about .03 and are mostly non-significant. Merkley finds backlash from out-party cues.
- Both papers measure publics, not legislator-to-legislator pressure, so they set sign, not size. On sign they run against cpf_pressure's positive 0.5 out-partisan pull in the flip model.
- Ringe 2013 (EP Environment Committee, 40 roll calls 2004-08, 1,319 non-unanimous opposing-bloc dyads) finds that more connection between opposing-bloc members goes with *less* co-voting. That cuts against a 0.4 cross-party diffusion floor, but the result is cross-sectional and not from the US.

**Flags**
- Trivedi 2020's degree term raises probability for hubs. It must not be read as support for the REJECTED per-edge volume normalization.
- Zareie's signed-edge taxonomy would change the two-physics architecture, so it is the owner's decision. It is not a recommendation.
- Chen 2023 is context only: the kernel ignores how many neighbours are already active, so it cannot represent complex contagion.

### Contradictions
- Bakker et al. (VERIFIED-SOFTENED, contradicts): out-party cues lower support (about -.07 pooled, mass public). The flip model gives an out-party pro neighbour positive pull at cpf_pressure 0.5.
- Furnas 2019, by party stratum: the out-aligned penalty is about twice the aligned gain. The kernel's asymmetry runs the other way (+0.7 gain, -0.5 penalty). The pooled Furnas estimate is null, so quote the party strata.
- Ringe 2013: more connection between opposing-bloc members goes with less co-voting (EP, cross-sectional), against a positive 0.4 cross-party diffusion floor for non-bridge pairs.

### Gaps
- Saturation constant 6: no study estimates it. Fill with an internal fit of cosponsor join timing on CU bills (hazard of joining as a function of already-joined weighted neighbours), held out by Congress.
- Out-partisan weight on legislator-to-legislator edges: needs elite-level evidence, not mass-public cue experiments. Candidates are a test stratified by party pair of whether cross-party cosponsor ties predict later joining, or the scan leads (Fong JOP, Box-Steffensmeier/Christenson/Craig AJPS 2019, Rogowski & Sinclair office lottery). These are UNVERIFIED.
- sqrt(bpf_u bpf_v) bridge form: no paper measures bipartisanship as a moderator of cross-party diffusion.

### Paul's position vs the evidence

Paul's two reads hold for direction only. Directed per-edge influence probabilities are foundational (Fink), and source party filters receptiveness (Furnas). His own Furnas source implies a larger out-party penalty than the 0.5 floor gives. Bakker and Merkley, which he highlighted, point to a negative out-party sign, not a weaker positive one. He is right that the saturation constant has no literature basis, and the evidence confirms nobody estimates it. It needs an internal fit, not more reading.

### Open questions
- Should cpf_pressure's out-partisan value go to 0, as lt_priming's xf already does for non-bridge pairs? That is a gated recalibration and Paul's call.
- Do frontier seed rankings stay stable when the constant 6 is swept over 3, 6 and 12?
- Does the Fink-derived weight change meaning (per-tweet rate versus per-campaign activation odds) enough to distort engagement edges?

### Read first

[[bakker-nd-understanding-partisan-receptivity]] [[furnas-2019-moneyed-interests-information]] [[merkley-nd-party-cues-news]] [[fink-2023-congressional-social-network-dataset]] [[kempe-2015-maximizing-spread-influence]] [[ringe-2013-pinpointing-powerful-voting]]

## Competitive independent cascade with common random numbers

**What it computes.** Expected pro-CU activations in a two-colour first-to-reach race; bystanders turn anti with probability min(0.9, industry$/400000).

**Method.** Keyed splitmix64 draws per (iteration, edge), reseeded with 1000003*SEED+it. SEED 7, MC 20 (scenario 40). Seed activation uses a separate 2^62 key range.

**Code refs.** `fec/moneyball_core.py:549-554`, `fec/engine/budget_frontier.py:40-94`, `fec/engine/per_topic_coalition.py:27-57`, `fec/engine/scenario.py:40-53`, `fec/engine/salience_shock.py:71-81`

**Assumptions**
- Independent edge activations given the draws (Bharathi-Kempe-Salek competitive IC)
- Industry money mobilizes neutral bystanders
- MC = 20 is adequate

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[harrison-2010-introduction-monte-carlo]] | 2010 | qualifies | Many simulations follow three steps: model the system as PDFs, sample from them repeatedly, and tally the statistics of interest. Drawbacks: simulation can need huge computing resources, gives no exact solutions, is only as good as the model and inputs, and its software is prone to bugs. Analytic and experimental alternatives should be considered before committing to simulation. | The recipe and caveats are generic, so they carry over fully. They say nothing about whether the cascade parameters are right, which IC model to use, or whether MC = 20 is enough. The paper does not cover common random numbers or variance reduction. Read it as a warning about inputs, not as support for the design. | weak | VERIFIED-SOFTENED |
| [[kumar-2022-influence-maximization-social]] | 2022 | qualifies | Training labels are each node's individual influence computed under SIR and IC diffusion, and the seed set is the top k nodes by predicted influence. The preview does not say the simulations are single-source, and the paper does not model competition between seed sets. | Single-colour standalone-spread labels do not encode competition or seed overlap. A surrogate would need retraining on competitive-race marginal labels before standing in for this engine, and the paper never tests that case. | moderate | VERIFIED-SOFTENED |
| [[arya-2025-relink-edge-activation]] | 2025 | measurement-source | Spread is estimated with 100,000 Monte Carlo IC simulations per selected subgraph, each experiment repeated 10 times, with averages reported (Sec 6.1). The paper gives no rationale for the number of draws. The 0.85 to 1.15 range is only the colour scale of the Fig 6 heatmap, not a claim about resolving method differences. | The paper's 100,000 draws are its own protocol and say nothing on whether 20 to 40 draws are adequate for Melange rank decisions. Adequacy remains the system's own claim (mc_stability_check), which this paper does not test. Any variance-reduction benefit of CRN is not supported by this paper. | weak | VERIFIED-SOFTENED |
| [[borodin-nd-threshold-models-competitive]] |  | supports | With technology-specific edge weights, the separated-threshold competitive problem (Theorem 7) and the Carnes wave-propagation variant with technology-specific probabilities (Theorem 8) are NP-hard to approximate within Omega(N^(1/2-eps)) by a Vertex Cover reduction (Appendix D, Theorem 16). This refutes the Carnes et al. conjecture for that case. | Holds only while the kernel stays shared. Giving anti-side diffusion its own edge weights or physics, such as cpf_pressure on the B side only (moneyball_core.py 546), would move the engine into the stated hardness scope. Tie-breaking by frontier order is not analysed in this paper. | moderate | VERIFIED-SOFTENED |
| [[cai-2025-competitive-influence-maximization]] | 2025 | qualifies | Competitive influence is analysed in a non-progressive voter model with reversible opinions, where the opponent is a strategic controller choosing its own start time at a Nash equilibrium (Eq 9, Algorithm 2 p.8; Fig 8 p.19-20). The paper does not state that the second mover concentrates its budget in the final stage. Its text says the second mover starts later and can only buffer the first mover's gain (p.21). End-loaded budgets appear for the budget-advantaged controller when the degree exponent is below about 2.2 (Sec 4.3.4, p.21-22). | This is a difference in model class, not evidence that the independent cascade choice is wrong. The original claims that cosponsorship is close to progressive with rare withdrawals, and that stance and votes are not, have no source in the paper or the code. Those claims are UNVERIFIED here and should not be relied on. | weak | VERIFIED-SOFTENED |
| [[moreno-2021-shadowing-shielding-effective]] | 2021 | context | In the discrete regime optima are driven mainly by node degree (hub targeting). Shadowing and shielding matter mostly in the continuous regime, and shielding weakens as K grows (K = 256 in Fig 3c). | IC is progressive (an activated node stays activated) and the voter model is reversible, so only the regime point transfers: seed-set optimisers will tend toward hubs, and opponent-aware placement matters more when allocation is continuous. One network and no noise in the paper. The lazy-greedy description of the frontier engine was not checked in this pass. | weak | VERIFIED |
| [[zareie-2023-influence-maximization-social]] | 2023 | qualifies | Influence detection by simulation is the most expensive family, ranked simulation, then MIA, then RIS, then centrality (Sec. 2.2). Almost all methods model spreading with a conventional diffusion model, and changing that model may change accuracy significantly (Sec. 4). The survey gives no numbers for either point. | The survey gives no numbers, so it supports only a caution that outputs are model-conditional. The claim that RIS for two-colour competitive IC is not standard does not come from the survey and was not checked. The scenario count of 40 was not verified. The binding issue is diffusion-model validation, not speed. | weak | VERIFIED-SOFTENED |
| [[bharathi-nd-competitive-influence-maximization]] |  | method-source | Independent cascade extended to two colours. Each edge gets its activation coin and an exponential delay fixed before the run. Lemma 1 builds a deterministic equivalent in which each edge outcome is fixed in advance and a node takes the colour of the path that reaches it. The paper's random tie-breaking applies only when several players select the same seed node, not to simultaneous arrivals. | Edge-level CRN and first-colour-wins carry over. Timing does not: unit hops replace the paper's continuous delays, so the paper's equivalence does not cover the system's timing. The bystander mapping (0.9 cap, 400000 scale) and the kernel constant 6 have no basis in the paper. | strong | VERIFIED-SOFTENED |
| [[bharathi-nd-competitive-influence-maximization]] |  | contradicts | The paper avoids ties between simultaneous activation attempts by giving each attempt a continuous exponential delay. It randomizes only between players who select the same seed node. The rel claim that simultaneous arrivals are tie-broken at random is not in the text. | The departure from the paper's no-tie design is confirmed by reading the code. Its effect on CU reach is unmeasured. A rerun with B-first or randomized frontier order under the same draws would measure it. Remains UNVERIFIED under DE-062. | moderate | VERIFIED-SOFTENED |
| [[kamarthi-2020-influence-maximization-unknown]] | 2020 | context | Every comparison uses a single-colour independent cascade with uniform p = 0.1, budget 10, and the mean of 100 simulations, with no variance or confidence intervals. Tables 2 and 4 report the best Geometric-DQN variant per test network or per test set. | Gains measured under uniform p = 0.1 single-colour IC do not carry over to the system heterogeneous two-colour kernel. Any reuse needs a rerun on the system own P and stays UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED |

### Assessment

**Evidence weight: strong on mechanism, nil on the bystander rule and MC sufficiency.** 10 rows: 2 VERIFIED, 8 VERIFIED-SOFTENED.

**What holds**
- Bharathi et al. (method-source, strong) supports edge-level common random numbers with first-colour-wins. Each edge has one draw with no colour in the key, which matches the paper's fixed-in-advance edge equivalence (Lemma 1).
- Borodin's inapproximability result covers models with separate per-technology edge weights. The shared kernel sits outside it. That holds only while both colours use the same P; giving the anti side its own physics would move the engine into the hardness scope.

**Departures from the cited model**
- **Timing.** Unit hops replace Bharathi's continuous exponential delays.
- **Ties (VERIFIED-SOFTENED, contradicts).** The A-first frontier resolves same-hop arrivals deterministically for the pro side, where the paper designs ties away. The direction of the bias favours CU reach. Its size is unmeasured.

**Unsupported inputs**
- The bystander rule min(0.9, industry$/400000) has no basis in any row.
- MC = 20 adequacy is the system's own claim. Arya 2025 used 100,000 draws but gives no rationale, and no row tests 20 versus more.
- Harrison 2010 and Zareie apply as generic cautions that outputs are model-conditional.

**Model class**
- Cai 2025 and Moreno 2021 use reversible voter models and a strategic opponent. They show a difference of model class, not an error. The claim that cosponsorship is near-progressive is UNVERIFIED.
- Kamarthi and Kumar gains come from uniform single-colour IC and do not transfer to this two-colour heterogeneous kernel.

### Contradictions
- Bharathi et al. (VERIFIED-SOFTENED, contradicts): the paper avoids simultaneous-arrival ties. The code's A-first frontier hands same-hop ties to the pro side deterministically. The effect on CU reach is unmeasured.

### Gaps
- Bystander mobilization min(0.9, $/400k): no evidence. Fill by regressing anti-side cosponsor or opposition accumulation on CU bills on opposing-industry PAC dollars per member, within a stated window and denominator.
- MC sufficiency: run an internal convergence check of seed-order stability against MC (20, 40, 200) under CRN, reporting rank deltas. This is not a literature question.
- Fit to reality: no row checks whether competitive IC reproduces observed cosponsor accumulation curves on CU bills.

### Paul's position vs the evidence

Paul wants rival industries modelled; the engine models them, but only as a non-strategic bystander probability. Bharathi's guarantees hold only for a last mover facing a fixed opponent, so industry reacting to public FEC filings is outside the design. Paul's preference for cheap analytic scenario estimates (Viral Centrality) has no head-to-head test against the MC cascade in any row. The choice of MC with CRN is defensible (Harrison) but unbenchmarked.

### Open questions
- How much CU spread is attributable to A-first tie-breaking? Rerun with B-first and randomized frontier order under the same draws.
- Is MC = 20 enough for frontier seed order, or only for aggregate spread?
- Should industry be modelled as a reacting player, given public FEC disclosure?

### Read first

[[bharathi-nd-competitive-influence-maximization]] [[borodin-nd-threshold-models-competitive]] [[harrison-2010-introduction-monte-carlo]] [[moreno-2021-shadowing-shielding-effective]] [[cai-2025-competitive-influence-maximization]]

## Contingent seed activation gate

**What it computes.** gate(n) = max(0.15, seed_prob(n) x SGATE[tier]); committed allies are never gated.

**Method.** SGATE ranges from Highly Persuadable 1.00 down to Opponent 0.35, with unknown 0.85. seed_prob comes from the receptivity layer.

**Code refs.** `fec/moneyball_core.py:626-630`

**Assumptions**
- A lobbied legislator acts only with a stance-graded take-up probability (Chen et al. UAI 2021 contingency-aware seeding)

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | supports | Stochastic submodular maximization with independent item states (failure types) and a monotone submodular objective: greedy obtains (1-1/e) of the best adaptive policy (Thm 16, generalizing Asadpour et al. 2008). Robustness to a misspecified prior is shown for a multiplicative error condition with alpha = d/c (Sec 4.3). | The structure matches: independent per-seed Bernoulli take-up over a spread function. The guarantee also needs spread given activated seeds to be monotone submodular, which is not shown for this competitive kernel, and needs known take-up probabilities. SGATE and seed_prob are unestimated, so the known-prior assumption fails. | moderate | VERIFIED |
| [[chen-2021-contingency-aware-influence]] | 2021 | method-source | A selected seed is willing with a probability q that is known a priori. Footnote 2 says q is the same for all nodes because per-node values are impractical to know. Default q = 0.6; Q3 varies q over 0.2, 0.4, 0.6, 0.8. The paper estimates no real-world q. | The modeling idea transfers: invitation is not activation. The paper's q is homogeneous and its analysis is multi-round. SGATE is member-specific and used single-shot, so Theorems 1-3 do not cover it. The 0.60 baseline is the paper's simulation default, not an estimate. The source population is HIV peer leaders, not legislators. SGATE values remain design priors until measured. | moderate | VERIFIED-SOFTENED |
| [[zareie-2023-influence-maximization-social]] | 2023 | qualifies | Behaviour-aware IM adds user features (BV), relationship features (BE) and message features (AM) to the structural IM problem (Eq. 1). The survey reports no take-up rate, no effect size and no benchmark, and it classifies methods without comparing their accuracy. It warns that numeric encodings of attitude may not capture personality and that many users are silent. | Supports only the framing that member attributes may enter seed activation. The survey does not discuss contingency-aware seeding, does not cover legislators, and does not justify the 0.60 baseline, the coefficients or the 0.30-0.90 clip, which remain design priors. Evidence route is H1: outcome_contract.py requires at least 150 members with cosponsorship deltas. The F5 count of 72 was not checked. | moderate | VERIFIED-SOFTENED |
| [[madsen-2017-strategic-advantages-micro]] | 2017 | supports | Under Bayesian source-credibility updating, contacts restricted to receptive targets (credibility signal above 0.5) convert better than untargeted contacts. Contacting audiences who view the source unfavourably moves them away from the source's position. | Directional agreement only. The paper's agents are voters, not legislators. Its unit is a source-credibility belief update, not a take-up decision about cosponsoring, and it estimates no take-up rates. The SGATE values remain unestimated design constants; this paper does not calibrate them. | weak | UNVERIFIED |
| [[watkins-1992-q-learning]] | 1992 | context | Q-learning needs each state-action pair sampled an infinite number of times (p.281). Lemma B.3 (p.284; proof p.290) says action-replay estimates of rewards and transitions converge with probability 1 as the level grows, under learning-rate sum conditions. This concerns estimating model quantities from unbounded samples, not learning a gate parameter. No finite-sample rate is given. | Learning take-up rates would need repeated observed contact-to-action outcomes per stance tier, which the system does not log. The paper's result is asymptotic and gives no sample size. The link is conceptual: Lemma B.3 concerns action-replay transition and reward estimates, not gate probabilities. Weak strength is right. | weak | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: the concept is supported, the values are unsupported.** 5 rows: 1 VERIFIED, 3 VERIFIED-SOFTENED, 1 UNVERIFIED.

**The concept**
- Chen 2021 (method-source) establishes that an invitation is not an activation.
- Chen's q is homogeneous across nodes, a known prior, and set to a simulation default of 0.6 (HIV peer leaders). The paper estimates no real-world take-up rate.
- SGATE is member-specific and single-shot, so Chen's theorems do not cover it. The 0.60 base of seed_prob is Chen's simulation default carried over, not an estimate.

**The guarantee**
- Golovin 2011 (VERIFIED) gives the (1-1/e) adaptive guarantee for independent take-up. It needs two things the system does not have: monotone submodular spread, which is unshown for this competitive kernel, and known take-up probabilities. SGATE is not estimated, so the known-prior condition fails.

**The values**
- Zareie supports only the framing that member attributes may enter seed activation.
- Madsen 2017 (UNVERIFIED, voter agent-based model) agrees that contacting unreceptive targets converts worse and can backfire. SGATE has no negative outcome.
- Watkins 1992 notes that learning take-up would need logged contact-to-action outcomes per tier, which the system does not collect.

seed_prob is a heuristic take-up weight, not an access or introduction probability (D-025).

### Contradictions
- none recorded

### Gaps
- SGATE values (1.00 down to 0.35, unknown 0.85) and the 0.15 floor are unestimated. The study that would fill this is a field or quasi-experiment logging lobbying contact and subsequent cosponsorship, with take-up broken down by prior stance tier.
- Scan lead (UNVERIFIED): Camp, Schwam-Baird & Zelizer report null lobbyist-outreach effects on cosponsorship across four field experiments in two state legislatures, with block randomization within legislator. If its replication files allow a split by prior position, it is the closest existing design. Transfer from state legislatures to Congress is not established.
- No backfire branch for contacts with Opponents.

### Paul's position vs the evidence

Paul's mapping of contingency-aware seeding to legislators who may or may not move holds as a concept. The evidence does not support SGATE as anything more than a design prior. Chen itself uses one homogeneous q, chosen by simulation default, because per-node values are impractical to know.

### Open questions
- Is there any internal record (ACU fly-in logs, contact notes) that could estimate take-up by tier, given that LDA filings are identity-only (D-021)?
- Does the outcome contract's H1 threshold (at least 150 members with cosponsorship deltas) make an SGATE test feasible this Congress?

### Read first

[[chen-2021-contingency-aware-influence]] [[golovin-2011-adaptive-submodularity-theory]] [[zareie-2023-influence-maximization-social]] [[madsen-2017-strategic-advantages-micro]]

## Flip - winnability model

**What it computes.** pflip = a/(a+b), where a = WM pm (c cu+1)/((c cu+1)+HALFF R) + WP nA/mAt and b is the same with industry money and anti neighbours. flip_value = sum tgt x pflip, with tgt = engagement x PERS x (1+0.3 bpf) x delivery_mult.

**Method.** Saturating money term on receipts R (floored at $200k; zero imputed to median). nA = pro-neighbour weight x cpf_pressure. WM 0.35, WP 0.65, HALFF 0.10. A comment cites a Volden-Wiseman passage bonus but the code line has none.

**Code refs.** `fec/engine/flip_model.py:1-147`, `fec/engine/money_observation.py:4-13`

**Assumptions**
- Money buys access more than votes (Ansolabehere; Hall & Wayman; Kalla & Broockman)
- Peer proximity outweighs money
- Money saturates relative to receipts
- Flip-side competitive LT has no submodularity guarantee (Borodin et al.)

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2020-strategic-sequential-links]] | 2020 | qualifies | Lobbying links follow first-time donations immediately and decline over the next three years; the paper measures lobbying-legislative links, not votes, and cites Fowler, Garro and Spenkuch (2020) for no quid-pro-quo returns to corporate PAC donations even if donations buy access. | Consistent with the access-not-votes stance but gives no vote-shift evidence, so it cannot calibrate WM/WP or HALFF. It implies the lobbying-link value of money is onset-driven and transient, which a static cycle-level money term ignores. Sample is public manufacturing firms; the system models CU/bank money. | weak | VERIFIED-SOFTENED |
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | supports | Kang (2016) structural Tullock-contest model of energy policy (538 policies in 445 bills, 110th Congress): lobbying raises enactment probability by only 0.021%-0.415% relative to an 8% baseline, while average returns to lobbying are estimated at 137% (renewables) to 152% (oil and gas). The review reads both as possible misspecification. The separate point that observable contributions miss other influence channels comes from the Farm Bill / Bombardini and Trebbi (2011) discussion, not from Kang. | Kang measures firm lobbying expenditure against provision-level enactment in the energy sector. Melange's money leg is PAC contributions against member-level winnability on CU topics. The review supports only the direction (small marginal effect of money), not the WM/WP split or the HALFF constant, which remain unestimated design choices. The evidence is federal and from 2007-08. | moderate | VERIFIED-SOFTENED |
| [[holyoke-2015-learnable-skills-unteachable]] | 2015 | supports | Evidence on campaign finance's impact is at best mixed (Baumgartner and Leech 1998). 'Very few' lobbyists take part in campaign finance, and those who do are partisan (Koger and Victor 2009). The authors conclude that campaign money matters less to lobbying than popular belief holds. No figure is given. | The paper agrees with the ordering (money below relationships and information) but gives no estimate. It relies on secondary sources from 1998-2009. It speaks about lobbyists' personal giving, not PAC money given to members, so it cannot calibrate the 0.35/0.65 split or HALFF. | weak | UNVERIFIED |
| [[furnas-nd-conflating-lobbying-pacs]] |  | qualifies | About 75% of 1998-2018 lobbying expenditures came from organizations that also gave through PACs. The top lobbying groups almost always have affiliated PACs. Organizations doing both average $403,435 a year in lobbying (IQR $118,825-$1,558,597) vs $64,000 for lobby-only organizations (IQR $30,000-$130,000). Even so, 64-70% of politically active organizations per year lobby without a PAC. | PAC money should flag the biggest lobbying spenders, since the paper finds those almost always have PACs; organizations that lobby without a PAC contribute nothing to b's money leg. Whether specific finance opponents (ABA, ICBA, large banks, fintech or crypto firms) fall in either group is the analyst's assumption, not in the paper. The paper tests neither access nor votes and gives no support for the WM/WP split. UNVERIFIED (DE-062). | moderate | VERIFIED-SOFTENED |
| [[furnas-2019-moneyed-interests-information]] | 2019 | qualifies | Donors are not granted meetings more readily than geographic constituents; Democratic staff are less likely to meet donors. This is contrary to Kalla & Broockman 2016 and consistent with Chin, Bond & Geva 2000. Staff say they are much less likely to use information from donors. They are relatively more likely to side with donors than to meet them or use their information, which the authors leave unresolved. The authors also find no preferential access for donors qua donors. | The evidence agrees that money is a minor lever on positions. It contradicts the access half of the code comment, at least for stated staff intentions in 2017: a request framed as coming from a donor gets no meeting premium. The vignette varies requester identity, not dollar amounts, so it says nothing about the shape of the saturating money term or about HALFF. Social desirability may push stated donor meetings down, as the authors warn, and that limits how far the evidence carries. | moderate | VERIFIED |
| [[huneeus-2018-effects-firms-lobbying]] | 2018 | qualifies | The return to lobbying is measured as firm revenue (IV elasticity 0.227 to 0.319), not as member votes or positions (Table 2, printed p.26). Lobbying spend is more than six times PAC, party and candidate contributions on average (Fact 2 and Figure 1, printed pp.8-9; FEC two-year cycles 1999-2016; individual contributions excluded). | The paper has no member-level outcome and cannot calibrate WM, HALFF or the saturation. Its only relevance is the scale fact that lobbying is the largest corporate political channel and is absent from the money terms. Under D-021 that absence is deliberate, so this is not a recommendation to add LDA dollars. The claim that Paul's positions cite H&K under the flip model is not supported by this item (paul_entries holds no takeaways or links), so it was not verified. | weak | VERIFIED |
| [[jacoby-2025-mathematical-modeling-analysis]] | 2025 | qualifies | With parameters taken from cited literature and databases (none fitted to the vote), adding donations to the House run moves 1 legislator (230 to 231 yes, 8 undecided). The lobbying run yields 231 yes, 203 no, 1 undecided; the authors describe this as undecided members falling from 9 to 1 and conclude that lobbying sways undecided members more than donations do. | Directionally consistent with the claim that money moves votes little, but this is a contrast between simulation runs under chosen inputs, not an estimated effect. The firm-value inputs (rho_Y, rho_N, Delta pi) are not reported in Table 2, the effect is 7-8 of 435 members on one bill (House, ACESA 2009), and the lobbying input is a ProPublica count of all ACESA-related lobbyists (regular plus a revolving-door subgroup, Fig 3 caption). The paper gives no basis for setting WM/WP or for adding a lobbying channel. Under D-021, LDA-derived lobbying data cannot become an edge or weight term. | weak | VERIFIED-SOFTENED |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | qualifies | Table 2 (PDF p.21): individuals investor -1504601 vs baseline -1520312 and ideological -1334200. Corporate and trade PACs (N=1312): unconstrained investor -1687949 vs ideological -1694458, so the investor form is ahead by about 6,500 log-lik units. Paper says corporate and trade PACs combine ideological and strategic motives and excludes them from scaling as a precaution. Paper does not measure vote-buying, access or any effect of money. | Both money legs are corporate or trade PAC money, the class the paper finds mixed-motive, so the individual-donor ideology result cannot be borrowed to call these dollars purely expressive. Paper provides no calibration of WM, HALFF or MAXSHIFT, so it gives no support for the CALIBRATED label on WM. | moderate | VERIFIED-SOFTENED |
| [[battaglini-2016-influencing-connected-legislators]] | 2016 | suggests-extension | Legislators value conformity with network peers, so a group pricing a legislator counts downstream effects through network walks (Bonacich multiplier), not only the legislator's own vote (Sec 2; Prop 1). The estimated externality is positive and significant: phi 0.2088 committee, 0.0837 alumni (Table 2). | The paper's phi measures network autocorrelation in contributions, which may reflect correlated effects rather than vote conformity; the paper does not show that money to one member moves a peer's vote. The paper's equilibrium is two-sided; the system optimizes for one PAC, so the link is conceptual. Any continuous spillover term needs its own prereg look, not a calibration from this paper. | weak | VERIFIED |
| [[kim-2023-systemic-sequential-links]] | 2023 | context | Corporate PAC donations show the strongest association with later lobbying of the recipient (committee 0.103 to 0.147 versus CEO 0.017 to 0.050 and employee 0.015 to 0.039). The authors say the estimates should not be read as causal (p.29). The amount effect is small: 0.003 on log donation amount in the firm-politician FE column (Table 1 col.8), which is about 0.03 pp per 10 percent (text p.20). | The paper says nothing about the WM to WP ratio or roll-call flips. Its outcome is the firm's lobbying behaviour, not the member's vote, in manufacturing firms 2008-2018. It supports at most the general premise that PAC money is the most-associated channel, so it is context and a weak prior, not evidence for the 0.35 weight. | weak | VERIFIED-SOFTENED |
| [[kalla-nd-campaign-contributions-facilitate]] |  | qualifies | Donor label raised senior access in 191 House offices of one party, June to August 2013: chief of staff or above 12.5% vs 2.4% (p=.006), legislative director or above 18.8% vs 5.5% (p=.005), Member 7.8% vs 2.4% (p=.07). Ratios implied by Table 1: about 5.2, 3.4 and 3.2. The paper reports no effect on votes or positions. | The paper supports only the access step: one 2013 experiment, one liberal group, one bill, one party's House offices, a randomized donor label rather than dollars. It gives no estimate of WM, WP, HALFF or saturation, and no effect on votes or positions (p.555). The citation supports access before votes only if narrowed to access; the not-votes half is outside the paper. | strong | VERIFIED-SOFTENED |
| [[alexander-2009-lobbying-analysis-under]] | 2009 | context | Lobbying firms repatriated about 70% of dollars: 209.1bn of 298.36bn (Table 6; the introduction says 208bn). Table 5 has no lobbying regressor, so the only lobbying test is the group comparison in Table 6. The introduction promises a regression with lobbying, and no such regression appears in the text. | Does not bear on WM, WP or HALFF. The paper measures a firm benefit, not member movement, and measures lobbying rather than PAC giving. Furnas (item furnas-nd-conflating-lobbying-pacs) finds only a small minority of organizations do both. The paper gives no basis for raising the money weight. The clause about Paul's ROI enthusiasm has no support in the item file and is dropped. | weak | VERIFIED-SOFTENED |
| [[cai-2025-competitive-influence-maximization]] | 2025 | qualifies | Vote share at a finite horizon depends on when each controller starts and on degree heterogeneity. At T=16 (dimensionless model time, unit-weight graphs) the degree of equilibrium l never exceeds 0.82 and high-degree nodes relax more slowly. An optimized start beats t_a=0 in all 30 rows of Table 2 (5 real networks, T in {16, 256}, budget ratio in {0.1, 1, 10}). Congress votes network, T=16, budget ratio 1: S_A 0.705 vs 0.500 (model output, not observed votes). | Structural parallel only: both are ratios of competing pulls. Magnitudes do not transfer, since paper time is dimensionless, edges have unit weight and opinions are binary simple contagion. The paper shows that an equilibrium-share form omits timing, not that pflip is miscalibrated. | moderate | VERIFIED |
| [[moreno-2021-shadowing-shielding-effective]] | 2021 | context | Steady-state opinions solve (L + Wa + Wb) x = wa for the voter model with zealots (Eq 1 rate equation on p.3; Eq 2 vote share on p.4). In the high-allocation limit (w much larger than d) the optimal allocation has no degree dependence (Eqs 7-8, pp.7-8). | The functional form is analogous (a share-of-pull ratio), but the paper's neighbour states are endogenous while Melange fixes them from stance, so the paper's HMF results do not transfer. Read pflip only as a vote-share proxy, never as a vote or access probability (D-025). The analogy itself is an inference (DE-062). | weak | VERIFIED |
| [[anon-2025-cross-industry-political]] | 2025 | qualifies | Bank CEOs give little directly to candidates; several, including Moynihan and Masrani, gave nothing across recent cycles. Bank money flows mainly through company PACs and the ABA (cyclical average $2,402,499, memo wording). Mutual-fund Super PAC spending (about $261M annual average to the four biggest Super PACs; industry PACs about $13M cumulative; memo figures, no source or window) is described as channeled by the industry. CU giving runs through ACU coordination, with $0 dark money to the four biggest Super PACs (memo wording). | Structural caution only. The memo's dollar figures have no source, window, N or denominator, so they cannot calibrate WM, HALFF or the b-term. Before any engine change, Mentat's trade-association and Super PAC columns would need mapping to members. | weak | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: direction moderately supported, weights unsupported, access premise contested.** 15 rows: 5 VERIFIED, 9 VERIFIED-SOFTENED, 1 UNVERIFIED.

**Money as the minority partner (WM 0.35 vs WP 0.65)**

The ordering is consistent with:
- Bombardini 2020's review: Kang's structural estimate puts lobbying's enactment effect at 0.021%-0.415% against an 8% baseline (energy, 110th Congress);
- Kim 2020/2023 (amount effect about 0.03 pp per 10% more donation; not causal; manufacturing firms 2008-18);
- Jacoby 2025, a simulation, not an estimate: donations move 1 of 435 House members on ACESA 2009;
- Holyoke 2015 (UNVERIFIED).

No row estimates WM, WP or HALFF. The CALIBRATED label on WM has no source (Bonica row).

**The access half of the code comment is contested**
- Kalla & Broockman (VERIFIED-SOFTENED, strong) found that a donor label raised senior access: chief of staff or above 12.5% vs 2.4%. That is 191 House offices of one party, June-Aug 2013, with no vote or position outcome.
- Furnas 2019 (VERIFIED) found no donor meeting premium in staff stated intentions in 2017.
- Neither paper supports the 'not votes' half; that rests on Ansolabehere and Fowler-Garro-Spenkuch, which no row checked.

**Structural points**
- Kim 2020 suggests the lobbying-link value of money is onset-driven and decays over about three years. A static cycle-level money term ignores this.
- Battaglini 2016 (VERIFIED) shows network spillover of influence (Bonacich multiplier), which pflip's one-hop, own-member-only money term omits. Its phi may reflect correlated effects rather than conformity, so a spillover term would need its own prereg look.
- Bonica 2013: both money legs are corporate or trade PAC money, which is mixed-motive. Individual-donor ideology results cannot be borrowed to call these dollars expressive.

**LDA (D-021)**
- Huneeus & Kim, Jacoby and Furnas (conflating) all point out that lobbying spend is the larger channel and is absent from the money terms. Under D-021 that absence is deliberate. This is not a recommendation to add LDA dollars or weights.

pflip is a heuristic winnability score, not a vote probability (D-025).

### Contradictions
- The code comment 'money buys access' (Kalla & Broockman: access premium, 2013 field experiment) conflicts with Furnas 2019 (no donor meeting premium in staff stated intentions, 2017). The evidence is divided. Neither paper bears on votes.
- WM/WP are labelled CALIBRATED in flip_model.py, but no source in the evidence calibrates them (Bonica 2013 row).

### Gaps
- Empirical peer-vs-money weight ratio in cosponsorship or roll-call decisions on CU bills: needs a member-level panel regressing alignment on CU and opposing PAC money as a share of receipts, with party and peer-exposure controls, stratified by party.
- Concavity in the contribution-to-receipts ratio: untested anywhere in the rows.
- Timing: no onset or recency term, although Kim 2020 implies one.

### Paul's position vs the evidence

Paul's 'money buys access and returns' and his reading of Jacoby (lobbying sways undecided members more than donations) both fit the model's conservative money weight. Jacoby is a simulation with literature-chosen inputs, not an estimate, so it corroborates rather than calibrates. Paul's own access source, Kalla & Broockman, measures access only. His ROI sources (Alexander, Huneeus & Kim) measure firm returns to lobbying spend, not member movement from PAC money, so they give no basis for raising WM.

### Open questions
- Should the CALIBRATED tag on WM/WP be relabelled as owner judgment? That is a docs change and Paul's call.
- Is there an onset effect, meaning first-time CU gifts move alignment more than repeat gifts?
- The method note says a code comment cites a Volden-Wiseman passage bonus that the code does not implement. Should the comment or the code be fixed?

### Read first

[[kalla-nd-campaign-contributions-facilitate]] [[furnas-2019-moneyed-interests-information]] [[bombardini-2020-empirical-models-lobbying]] [[kim-2020-strategic-sequential-links]] [[battaglini-2016-influencing-connected-legislators]] [[bonica-2013-mapping-ideological-marketplace]]

## Persuadability table PERS

**What it computes.** Topic target multiplier by stance tier: Champion 0.10 ... Highly Persuadable 0.65 ... Opponent 0.08; no position PERS_NONE 0.45.

**Method.** Fixed design table. PERS_NONE was ratified 2026-07-02 at Ally parity. The no-position default drives about 60% of cells (open M5).

**Code refs.** `fec/moneyball_core.py:564-568`

**Assumptions**
- Persuadability is inverted-U along the stance ladder
- Silence equals uncertainty, not enthusiasm

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[broockman-2026-should-moving-middle]] | 2026 | qualifies | Moving on an issue is rewarded only where the audience disagrees with the current position, and it is penalized where the audience holds that position (Republican candidate, own=R -0.0364 vs own=D +0.0404, Table A8). | This is an analogy only. The paper studies voters choosing between candidates, not legislators being lobbied. It gives no evidence on legislator persuadability or on the shape of the PERS curve, and it should not be read as calibrating PERS values. Its one structural point, that the target of an appeal is the issue-level disagreement and not the aggregate midpoint, is consistent with PERS being applied per topic. | weak | VERIFIED |
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | qualifies | The third coalition's median ideology shifts across sessions: very liberal (98th-102nd), center-left (103rd, 111th), center-right (105th-110th). Its members are effective. The paper does not measure persuadability, vote change or electoral vulnerability. | The paper gives no support for the PERS values or for treating third-coalition or Fitzpatrick-type members as more movable. Coalition-level ideological fluidity describes session-to-session composition, not the responsiveness of any individual member. | weak | VERIFIED |
| [[jacoby-2025-mathematical-modeling-analysis]] | 2025 | qualifies | Only undecided centrists (C) move. Committed Y and N legislators are absorbing states, and the equilibrium with all members undecided is unstable. Widening the centrist band on a -1 to 1 ideology scale from ±0.25 to ±0.35 reverses the House result so that no votes exceed yes votes. | The paper supports the extremes being near-fixed only because its model assumes it; Y and N are absorbing by construction. It does not test an inverted-U. Its centrist group is defined by an unnamed ideology scale, while PERS is defined by CU stance tier, so the band result carries over only as a warning: how the movable set is defined can decide the outcome. The system's PERS_NONE sweep shows the same thing in the system itself. Evidence is a single bill (ACESA, 2009) at the federal level. | weak | VERIFIED-SOFTENED |
| [[cai-2025-competitive-influence-maximization]] | 2025 | supports | In the individual-optimization runs with 20 percent of agents as partial zealots (Sec 4.3.3, N=100 per the Sec 4.3.2 reduction; N not restated in 4.3.3), normal agents receive more budget than zealots at every zealotry level q tested. At q=0.1 the maximum zealot allocation is under 20 and the normal-agent maximum is near 25 (allocation units as plotted in Fig 7). At medium and large q zealots get fewer resources and start later as degree rises. | The direction matches for resistant tiers (Opponent and Highly Skeptical). Champion is a committed pro tier, so its low multiplier reflects being already won, not zealot resistance, and it does not support the paper's result. The paper never sets zealot allocation to zero, so it does not support the hard zero gate on its own. Simulated agents only. | weak | VERIFIED-SOFTENED |
| [[tappin-2023-quantifying-potential-persuasive]] | 2023 | context | In the Study 2 calibration (n=3,990 respondents, 36 issues, 48 progressive video ads), the messages predicted most persuasive were often on obscure, low-information policies such as net neutrality. The authors offer this as one possible explanation for the Study 2 divergence, alongside a mixed pretest design and a nearly one-year gap before follow-up. They expect smaller effects in high-salience settings with countermessaging. | Weak. Tappin measures attitude change among public respondents after one video, while PERS models legislators under lobbying. The low-information point might carry over to members on low-visibility CU topics, but nothing here measures that. | weak | VERIFIED-SOFTENED |
| [[sverdrup-nd-introduction-grf-causal-forests]] |  | qualifies | Best linear projection tests whether effects vary along a chosen covariate: financial.autonomy.index coefficient -0.0329 (SE 0.0128, p = 0.0099). Other terms: is.female p = 0.17, family cash transfer p = 0.066, is.unemployed p = 0.61. Heterogeneity can be hard to detect at moderate n (payday AUTOC t = 0.67, about 2,500 individuals; text says possibly underpowered). | A BLP of contact effect on stance-tier ordinals needs logged contact and tier movement; the payday example suggests a sample of 533 members may be too small for tier-level heterogeneity. A null would not refute the table. | weak | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: weak. No legislator evidence on the inverted-U.** 6 rows: 2 VERIFIED, 4 VERIFIED-SOFTENED. All are weak.

- Jacoby 2025 makes the extremes immovable by construction: Y and N are absorbing states. It does not test an inverted-U. Its useful warning: widening the centrist band from ±0.25 to ±0.35 reverses the House result, so how the movable set is defined can decide the outcome. The system's own PERS_NONE sweep shows the same sensitivity, and PERS_NONE drives about 60% of cells (59.8% per the flip_model comment).
- Cai 2025 (simulated agents) allocates less to zealots. That matches the low PERS for Opponent and Highly Skeptical, but it never sets allocation to zero, so it does not back the hard F1 zero gate. Champion's low PERS reflects a member already won, not zealot resistance.
- Broockman 2026 (voters, not legislators) supports applying persuadability issue by issue.
- Aref 2021 shows coalition composition shifting across sessions, not individual movability.
- Tappin 2023 (public respondents) hints that low-information topics are more movable. No row measures this for members.
- Sverdrup/grf shows how a best-linear-projection test of contact effect by tier would work, and warns that 533 members may be too few for tier-level heterogeneity.

### Contradictions
- none recorded

### Gaps
- The inverted-U is untested on legislators. Fill with observed stance-tier movement after a dated lobbying or advocacy contact, by prior tier, with heterogeneity tested by BLP or causal forest. It needs logged contacts the system does not have.
- PERS_NONE 0.45 (silence equals uncertainty) has no evidence either way, yet it drives about 60% of cells.
- Scan lead (UNVERIFIED): Camp, Schwam-Baird & Zelizer report null average lobbying effects in state legislatures. If replicated in Congress, a null average would bound how much any tier can move.

### Paul's position vs the evidence

Paul's view that centrists and ideologically fluid competitive seats are most movable fits the table's shape. He has never tested it, and no row tests it on legislators. Aref gives no support for treating third-coalition or Fitzpatrick-type members as individually more movable. Jacoby's centrist result is assumed, not found.

### Open questions
- How sensitive are flip and frontier outputs to PERS_NONE across plausible values (0.25-0.65)? The value covers about 60% of cells.
- Should F1's hard zero for Skeptic and Opponent tiers be softened to a small positive weight, in line with Cai? That is Paul's call.

### Read first

[[jacoby-2025-mathematical-modeling-analysis]] [[broockman-2026-should-moving-middle]] [[sverdrup-nd-introduction-grf-causal-forests]] [[cai-2025-competitive-influence-maximization]] [[tappin-2023-quantifying-potential-persuasive]]

## Money response (lean shift) and money-triggered defection

**What it computes.** shift = MAXSHIFT x (r/(r+HALF)) x pressure_mult, with r = c x amount/receipts; competitor money is multiplied by LOSSFRAME 1.10. Defection moves floor(|shift|/DTHRESH) rungs; Champion and Opponent are immune.

**Method.** MAXSHIFT 18 (cut from 40), HALF 0.10, DTHRESH 11.25. Crossing one rung takes roughly a sixth of receipts.

**Code refs.** `fec/engine/money_response.py:15-90`

**Assumptions**
- Money moves positions little (null vote-buying literature)
- Threats weigh more than gifts (loss aversion, elite-attenuated)

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[linde-2016-politicians-take-risks]] | 2016 | contradicts | Loss aversion was NOT found in the MP sample (n=46) nor in the student sample; the authors suspect the loss-aversion tasks lack validity (participants may treat the endowment as house money, fn 4). The abstract's claim that MPs' deviations are 'somewhat smaller than those of other people' is not a formal statistical test in the main text: it rests on the authors' student comparison sample (n=176, Appendix E, not in the file; e.g. students showed the common ratio effect, MPs did not, p.106) plus prior literature. | The paper is no basis for LOSSFRAME. (1) It reports no loss aversion in politicians, so the citation runs against the dial. (2) Its loss frame concerns outcomes for the decision-maker relative to a reference point; a competitor PAC donation to a member is not a loss to that member, so the 'threat frame' is a separate untested assumption. (3) 1.10 is not derived from any number in the paper. LOSSFRAME stands as owner judgment and should be cited as such. | strong | VERIFIED-SOFTENED |
| [[poole-1999-nominate-after-years]] | 1999 | supports | Across Congresses 1-105, a constant ideal point per legislator classifies almost as well as a linear one (about 0.3 percent difference); linear change captured most of the relatively small changes in positions. | Indirect: the paper measures general-ideology stability with no money treatment, so it supports only a low prior on position change, not any MAXSHIFT or DTHRESH value. | weak | VERIFIED |
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | supports | Structural work finds a very small marginal enactment effect of lobbying (Kang 0.021%-0.415%) despite large stakes, and campaign contributions are low with federal donation caps very seldom binding (Ansolabehere et al. 2003, as reported). | The review addresses enactment and bill outcomes, not shifts in individual member stance. It supports rare money-triggered defection but not the LOSSFRAME 1.10 asymmetry or the specific constants. | weak | VERIFIED |
| [[jacoby-2025-mathematical-modeling-analysis]] | 2025 | supports | Campaign donations alone move one House member out of 435 (from 9 undecided to 8, 230 to 231 yes). The Senate is reported only for the combined lobbying-plus-donations run, which changes it by one senator (52/47/1 to 52/48/0). The authors note that donations may reflect ideology rather than buy influence (citing Bonica [5]). | This is a consistency check, not a calibration. The donation effect in the paper is a model output from literature parameters (I_F, I_A taken from Meng and Rode), not an observed shift. It is weaker than the experimental and null-result literature already cited at money_response.py:15. | weak | VERIFIED-SOFTENED |
| [[kalla-nd-campaign-contributions-facilitate]] |  | qualifies | The measured outcome was access: the rank of the official who attended. Positions and votes were not measured. Cosponsorship was nine legislators in total, with equal shares in both arms, and the authors say it tells little about effects. | The paper can be cited for the access measure, but not for a null on positions: its cosponsorship estimate is uninformative rather than null. It says nothing about MAXSHIFT, HALF, DTHRESH or LOSSFRAME. The not-votes half of the premise lies outside the paper (p.555). | weak | VERIFIED |

### Assessment

**Evidence weight: the low prior on position change is supported. LOSSFRAME is contradicted by its own citation.** 5 rows: 3 VERIFIED, 2 VERIFIED-SOFTENED.

**Supported: money moves positions little**
- Poole 1999 (VERIFIED): across Congresses 1-105, constant ideal points classify within about 0.3% of linear-drift ones.
- Bombardini 2020 (VERIFIED): tiny enactment effects.
- Jacoby 2025: a model output, not an observation.

Together these back a low MAXSHIFT and rare money-triggered defection: a rung needs about a sixth of receipts, and a $10k gift moves median receipts by about 0.9 lean against DTHRESH 11.25. None of them sets MAXSHIFT 18, HALF or DTHRESH; those are design values. MAXSHIFT is labelled CALIBRATED with no calibration in the rows.

**Kalla & Broockman**
- Its cosponsorship estimate (nine legislators in total) is *uninformative*, not null. It cannot be cited for 'money does not move positions'.

**Contradicted: LOSSFRAME 1.10**
- Linde & Vis 2016 (VERIFIED-SOFTENED, strong) found **no loss aversion** in MPs (n = 46) or in students. The code comment cites it as 'elite-attenuated' loss aversion, and audit_full locks the asymmetry in as by design.
- Separately, a competitor's PAC gift to a member is not a loss *to that member* in the prospect-theory sense, so the 'threat frame' is a second, untested assumption.

### Contradictions
- Linde & Vis 2016 (contradicts, strong): no loss aversion found in MPs (n = 46). LOSSFRAME 1.10 cites the paper as its basis, so the citation runs against the dial. Cite LOSSFRAME as owner judgment.

### Gaps
- Stance shifts after large competitor spends: no row. Fill with event studies of press or vote positions on CU issues around large opposing-industry PAC or Super PAC disbursements, with the window stated.
- Loss/gain asymmetry for legislators facing donor money: no evidence exists. The study would be a survey experiment on staff or members framing the same dollar amount as a rival's gift versus a forgone gift.
- Cross-issue defection mechanics: no row.

### Paul's position vs the evidence

Paul's highlights support a reflection effect in MPs, and Linde & Vis does find one: risky choice 39% under gain vs 76% under loss in the paid lottery, n = 46. Reflection is a different thing from loss aversion. The dial implements loss aversion, which the same paper did not find. The citation should move from LOSSFRAME to the receptivity layer's prospect-domain term, where it fits better, and LOSSFRAME should be relabelled as judgment.

### Open questions
- Should LOSSFRAME be set to 1.00 or relabelled? Either is a gated change and Paul's call.
- Should the CALIBRATED label on MAXSHIFT 18 be relabelled as owner judgment?

### Read first

[[linde-2016-politicians-take-risks]] [[poole-1999-nominate-after-years]] [[bombardini-2020-empirical-models-lobbying]] [[kalla-nd-campaign-contributions-facilitate]]

## Linear-threshold priming (cheapest tip)

**What it computes.** For each target, the cheapest mix of recruiting pro neighbours and money that closes theta - pressure.

**Method.** theta = tier threshold (0.6 ... 3.0) + max(0, -lean/120) + 0.25 per Opponent cell. Cross-party cue factor sqrt(bpf_u bpf_v) (Bakker-Lelkes-Malka). Recruit cost = CEDGE x 0.5 / PERS[tier]. Money closes the gap only up to 0.97 pm units.

**Code refs.** `fec/engine/lt_priming.py:11-145`

**Assumptions**
- Linear-threshold adoption over summed neighbour pressure
- Out-party cues do not persuade

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[furnas-2019-moneyed-interests-information]] | 2019 | supports | Evidence from the other party's think tank lowers the chance that staff side with the requester (a penalty about twice the aligned benefit). Republican staff also rate presenters of center-left evidence as less representative of their district. | Directionally, this backs a non-positive out-party cue, and it hints that a negative (backfire) term could be defended. The evidence is about outside information sources judged by staff, not Member-to-Member cosponsor cues, and it is stated intention from an underpowered design. It does not justify inventing a backfire magnitude. Keeping the clamp at 0 remains the conservative reading. | weak | VERIFIED |
| [[ma-2022-adversarial-attack-graph]] | 2022 | qualifies | Proposition 2: if each threshold CDF F_j is concave on [0, +inf), the expected spread h(S) is submodular, so greedy selection applies. Corollary 1 (uniform thresholds, Eq. 6) gives a saturated coverage objective that is submodular. Lemma 1: influence maximization on the directed bipartite linear threshold graph is NP-hard, by reduction from Set Cover with thresholds alpha/2. | The concavity result transfers as mathematics only. Lemma 1 is an NP-hardness result for deterministic thresholds; the paper does not state a converse that missing concavity implies hardness. A point-mass (step) threshold CDF is not concave on [0, +inf), so the paper's submodularity guarantee does not cover lt_priming's deterministic thresholds if they were aggregated into a seed-set objective. Moderate for the mathematics, weak for empirical relevance. | moderate | VERIFIED-SOFTENED |
| [[bakker-nd-understanding-partisan-receptivity]] |  | qualifies | Out-party cues lower policy support by about .07 on a 0-1 scale in the pooled studies 1-4 analysis, relative to the no-cue control. The paper does not use the word backfire; the decrease is the reported effect. Heterogeneity runs through partisan identity strength interacted with cognitive resources (NfC, latent resources). The paper measures no bipartisan-bridge or cross-party attribute. The authors say real messages often carry both in-party and out-party cues. | Sign only. The paper supports shrinking out-party cross-cues toward zero, which the bpf scaling does. The paper does not support the bridge exemption, under which sqrt(bpf) restores full cross-party influence for perfect bridge pairs, because bipartisanship is never measured. Mass-public one-shot survey effects do not transfer to elite cosponsorship recruitment without elite-level evidence. | moderate | VERIFIED-SOFTENED |
| [[ringe-2013-pinpointing-powerful-voting]] | 2013 | supports | Social connection between political adversaries goes with less voting agreement, not more. Theory predicts this when the pair expects to disagree (H1, Fig. 1). Opposed-bloc baseline co-voting is about 50 percent (p. 626). | Direction agrees: out-party ties should not count as pro-pressure. The association is not causal and is EP staff contact, so it does not justify adding a negative backfire term. Keeping the floor at 0 remains the default. | moderate | VERIFIED |
| [[borodin-nd-threshold-models-competitive]] |  | suggests-extension | In the separated-threshold competitive LT model a node is activated by whichever technology weighted active neighbours first exceed that technology own threshold. Adding A seeds never raises the probability that B propagates (Lemma 3 part 1, used in Theorem 3), but the process is not submodular in general (Theorem 4, counterexample only in an unincluded full version). | Thresholds here are not normalised to Borodin uniform [0,1] draws (base ladder 0.0 to 3.0 plus additive terms). Adding an anti-side threshold would be a model change for Paul to decide. Borodin monotonicity result is for the separated-threshold model and says nothing about this solver recruit lists, for which the paper gives no guarantee. | weak | VERIFIED-SOFTENED |
| [[merkley-nd-party-cues-news]] |  | supports | Out-group (Democratic) elite cues move Republicans away from the cued position rather than toward it. Experiment: strong Republicans +0.06 toward skepticism (about 0.2 SD, p~0.03), with a party-ID interaction p~0.05. Time series: Democratic cues Granger-cause Republican skepticism (chi2 7.35, p=0.03, 2 lags); the reverse has p=0.86. | The paper supports that out-party cues do not persuade and reads as backlash, so a negative sign is consistent with it. But the +0.06 effect is measured on mass Republicans and one climate item, not on legislators reading cosponsorship signals, so it cannot set a backfire magnitude for the priming term. Keeping the zero clamp remains defensible. | moderate | VERIFIED |
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | context | Table 7, Model 7.1 gives 0.015 (SE 0.002) and Model 7.2 gives 0.012 (SE 0.002) for female speeches x female pitch on the share of male votes cast with female members. At +2 SD pitch (1.41 on the paper's scale), that share moves from 0.23 SD below to 0.53 SD above as female speeches go from 0 to 43 (Figure 3, p. 957). The authors say the data are observational and do not claim causation (p. 956-957). Vote timing is unknown (fn 31). | Weak and gender-specific: women's issues, 2009-2014, floor votes with no timestamps, not cosponsorship. The paper does not test a threshold mechanism, so it cannot set any engine constant. | weak | VERIFIED |
| [[chen-2023-complex-contagion-influence]] | 2023 | qualifies | K-complex model (Eq. 1): activation probability p0 when at least K neighbours are active and p1 much smaller otherwise. It is motivated by Centola and Macy 2007 and Centola 2010 but not validated on field adoption data in this paper; the paper cites succeeding studies as support. | The paper supports only the modelling option that adoption can be threshold-like. It gives no K or threshold values for legislators. Joint multi-target priming would be a new model needing owner authorisation and a holdout look. | weak | VERIFIED-SOFTENED |
| [[kempe-2015-maximizing-spread-influence]] | 2015 | qualifies | LT is submodular when in-weights sum to at most 1 (Thm 4.6). The LT model draws each threshold uniformly on [0,1] (Sec 2). Thm 3.3, for the general threshold model, shows NP-hardness of approximation within n^(1-eps) for any eps > 0. Fixed-threshold models are only stated to admit no non-trivial approximation factor (p.117). | Deterministic thresholds are the regime the paper excludes, so the greedy guarantee does not transfer. A future joint multi-target priming optimizer would inherit the n^(1-eps) hardness for general thresholds. Per-target cheapest tip remains a heuristic. | strong | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: the sign of the cross-party cue is well supported. No threshold values exist.** 9 rows: 4 VERIFIED, 5 VERIFIED-SOFTENED.

**The zero clamp is well supported**

The BAKKER_XCUE treatment sends cross-party pro-cues through sqrt(bpf_u bpf_v), so non-bridge cues fall to about 0 and never go negative. Three VERIFIED rows agree on direction:
- Furnas 2019 (the out-party penalty);
- Ringe 2013 (opposing-bloc ties go with less co-voting);
- Merkley (out-party cues produce backlash: strong Republicans +0.06 toward skepticism, p about 0.03).

All of them measure publics, staff or EP members, so none can set a backfire magnitude. Keeping the clamp at 0 is the conservative reading. Bakker supports shrinking out-party cues but not the bridge exemption, because it never measures bipartisanship.

**Mathematics**
- Kempe 2015 and Ma 2022: deterministic per-tier thresholds (0.6-3.0, not normalised) are the regime with no approximation guarantee. Per-target cheapest tip is a heuristic.
- A future joint multi-target optimiser would inherit n^(1-eps) hardness under general thresholds (Kempe 2015).
- Borodin: the anti side exerts no competing threshold here; adding one would be a model change.
- Chen 2023 supports the modelling option of threshold-like (complex) adoption but gives no K for legislators.
- Dietrich 2019 is context only.

### Contradictions
- none recorded

### Gaps
- Threshold values: no legislative estimate. Fill by testing on own data whether CU-bill join hazard jumps after a threshold share of close cosponsorship partners has joined, against a smooth dose-response, held out by Congress.
- Scan leads (UNVERIFIED): Simkin & Roychowdhury (arXiv 1001.3732) model cosponsor accumulation on H.R. 1207 as a threshold/sandpile process with power-law join bursts. Rogowski & Sinclair (House office lottery) and Coppock find little peer cue-taking. Zelizer (state legislature) finds large briefing spillovers. Fong (JOP) reports expert-peer cues that cross party lines. If Fong holds, it would cut against the zero clamp for expert cross-party cues.

### Paul's position vs the evidence

Paul's hypothesis that coalition formation is a complex contagion is a modelling option the literature allows (Chen 2023). No row tests it on Congress. His view that pressure from high-ranking members is not diminishing would break submodularity. That is consistent with the component claiming no greedy guarantee, but there is no evidence for or against a seniority amplification. The kernel has no leadership term, so the idea is currently unimplemented, not refuted.

### Open questions
- Do expert cross-party cues (Fong) differ from generic out-party cues, and would that justify a bridge or expertise exemption from the clamp?
- Is cosponsor joining on CU bills bursty (threshold-like) or smooth?

### Read first

[[kempe-2015-maximizing-spread-influence]] [[merkley-nd-party-cues-news]] [[bakker-nd-understanding-partisan-receptivity]] [[furnas-2019-moneyed-interests-information]] [[chen-2023-complex-contagion-influence]] [[ma-2022-adversarial-attack-graph]]

## Salience shock (Noble-Turner going public)

**What it computes.** Rally, retreat and backfire probabilities per issue, member steps, and a go-public advisory.

**Method.** z = min(1, |D-R tier gap|/3); psi = uncertainty; b = min(0.35, 0.05 + 0.10 psi); p = (1-b)(1-z), q = (1-b)z. Rally: co-partisan +1, out-partisan +0.5 if bpf >= 0.30. Retreat: co +1, out -1. Vulnerable members x1.35 x susc_mult. Defensive appeal raises the threshold by (1 + DELTA/2).

**Code refs.** `fec/engine/salience_shock.py:1-246`, `fec/moneyball_core.py:731-751`

**Assumptions**
- Co-partisans move more than out-partisans under salience (Noble & Turner 2024)
- Backfire grows with policy uncertainty
- Electorally vulnerable members respond more

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[noble-2024-presidential-leadership-legislative]] | 2024 | contradicts | Going public is more attractive when the status quo is favorable: c(-1) > c(1) always (Prop 2). Under Partisan Retreat the president never goes public against an unfavorable status quo (c_q(1) < 0) but always has an incentive to defend a favorable one. The model predicts that going public occurs more often in defense of a favorable status quo than to win policy change (p.25). The defensive appeal logic depends on the co-partisan being the relevant pivot, which holds only when the status quo is favorable (p.15, p.22-23). | No empirical transfer is needed. This is an internal-consistency check against the model the code claims to implement. The contradiction holds whenever the defensive credit is added on sq='offense' issues. Gating held to sq=='defend' would restore the paper's ordering. Whether the paper's ordering is right for a CU advocacy campaign is a separate, untested question. | strong | VERIFIED-SOFTENED |
| [[noble-2024-presidential-leadership-legislative]] | 2024 | qualifies | Sincere legislative polarization z and base-pressure reactions (beta_L, beta_R, with probabilities p, q, 1-p-q) are separate primitives that the authors vary independently. The infrastructure illustration (Pew 2019 partisan gap of 7 points vs 46 on health care) is presented as a case where elite polarization exceeds mass polarization and a Rally is still possible. | The mapping is a design choice, not something the paper implies. It holds only if base reactions on CU issues track legislator stance gaps. That would need a base-side or mass-opinion measure, such as a partisan gap in public or credit-union-member opinion per issue, which the system does not have. The paper's own example warns that the two can diverge. | moderate | VERIFIED |
| [[noble-2024-presidential-leadership-legislative]] | 2024 | qualifies | psi bounds a common preference shock and stands for informational policy uncertainty. Higher psi makes the president weakly less likely to go public and shrinks the favorable-vs-unfavorable gap (Prop 5). The paper never links psi to the probability of Backfire. Under a favorable status quo, going public can pay even when backfire is certain (the threshold on beta_L falls as z and delta rise). | Data missingness stands in for psi only if a member's lack of a stance label reflects real preference uncertainty rather than collection gaps. Coverage is known to differ by party and chamber (press collector). The b-psi link has no support in this paper and should be cited as a design prior. | moderate | VERIFIED |
| [[noble-2024-presidential-leadership-legislative]] | 2024 | qualifies | Section 4 lets the president's silent-reelection probability rho(x_sq) vary. A higher rho lowers the go-public range. When rho(-1) > rho(1) and psi > psi*, the president may 'gamble for reelection' by going public on an unfavorable status quo (Prop 6, Eq. 15). Legislators' delta is their stake in their party winning the White House, not in their own seats. Caveat: Prop 6's first bullet (p.44) states the opposite direction for an electorally weak President, so the paper is internally inconsistent on the rho direction. | This paper does not support the amplifier. Its rho and the gambling logic concern the actor who goes public (the president), not the responsiveness of rank-and-file legislators. Any support for 'electorally vulnerable members respond more' has to come from elsewhere (for example, the electoral-incentives literature), and the 1.35 constant is uncalibrated. | moderate | VERIFIED-SOFTENED |
| [[noble-2024-presidential-leadership-legislative]] | 2024 | method-source | There are three base-reaction regimes: Rally (both toward), Partisan Retreat (co toward, out away) and Backfire (both away, out more). The single restriction is beta_L > beta_R: co-partisans always favor the president's proposal more than out-partisans do. Going public adds the electoral stake delta to the thresholds as z + beta_L + delta/2 and -z + beta_R - delta/2 (Eqs. 1-2). | The regime structure transfers most cleanly to the dashboard's executive cue, which matches the paper's presidential actor in a federal two-party Congress. It transfers less well to an ACU or industry 'going public' actor, which has no reelection tied to the outcome. The delta channel exists only because co-partisans share the president's electoral fate. The bpf >= 0.30 cutoff, the step sizes and the multiplicative delta are the code's own design, not the paper's. The paper's delta is additive in the vote threshold and has no money-defection analog. | moderate | VERIFIED-SOFTENED |
| [[noble-2024-presidential-leadership-legislative]] | 2024 | qualifies | 'Gridlock region' means the range of the common preference shock theta over which two pivots keep the status quo. Going public always widens it, even under a Rally, because asymmetric base pressure creates 'effective polarization'. | Occupancy of a middle band among 533 members is a different quantity from a shock interval between two pivots. Prop 1 makes no prediction about its sign, so the metric cannot be read as testing or reproducing Prop 1. | weak | VERIFIED |
| [[joo-2022-image-data-automated]] | 2022 | qualifies | The paper links consistent frames to policy response only by citing Wouters & Walgrave (2017) and Ketelaars et al. (2017), saying higher duplication may indicate greater frame alignment; Study 2 is descriptive and does not test whether higher duplication produces larger effects. | The 0.5 + alignment mapping has no estimate behind it in Joo, and the 65% input is inflated by ACU syndication. If auto intensity is ever enabled it needs the own-domain recompute first and should be treated as uncalibrated owner judgment. | moderate | VERIFIED |
| [[zhang-2022-data-assisted-social]] | 2022 | suggests-extension | Defines a time-varying event hotness index He(t) = sum over bursting events of sentiment x volume, with bursts flagged by z-scores of one-day variation over trailing 5- and 25-day windows (\|z\|>30). It is proposed as a brand-value measure, and no estimates or forecast errors are reported. | A burst detector on press and social volume per CU category could supply a dated salience-onset signal. Any such signal stays UNVERIFIED until it is tested against Noble-Turner style co-partisan shifts on a second path. The paper's 'sentiment x volume' must not be imported as an influence or importance score: it measures volume, not access or influence. It comes from consumer tweets about retailers, not congressional statements. | weak | UNVERIFIED |
| [[bakker-nd-understanding-partisan-receptivity]] |  | qualifies | Out-party cues lower support by about .06 to .07 and in-party cues raise it by about .03 (pooled and studies 1-2). The authors speculate, post hoc, that in-party patterns were stronger in the food irradiation (change) studies and out-party patterns in the farm subsidy (status quo) studies, and name other possible explanations. | The paper's mass-level out-party repulsion supports the negative out-partisan step in retreat and backfire. It shows no positive out-party movement, so it does not back the rally +0.5 step, which must rest on other sources. The issue-dependence finding is post hoc and should not be used to select a regime. | weak | VERIFIED-SOFTENED |
| [[merkley-nd-party-cues-news]] |  | qualifies | A polarized cue (Democrats for, Republicans against) moves strong Republicans +0.09 points on the 0-1 scale (about 0.3 SD, p~0.002). It is numerically the largest cue effect, but the authors do not report it as significantly different from the single Democratic cue or opposing-GOP cue effects. A single Democratic cue moves them +0.06 (p~0.03). Supporting-GOP and consensus cues show no polarizing effect and no de-polarizing effect. Observationally, +100 articles a quarter with cues held fixed goes with -0.4 SD in Republican skepticism (Model 3, PCCTI, N=54; p~0.03). | Carries over only as a direction-of-effect prior. The paper studies mass partisans reacting to elite cues in national newspapers on climate; the engine moves legislators' stance tiers on credit-union issues. The rally result rests on one thin MTurk treatment, so it weakens but does not refute out-partisan rally movement. Treating z as endogenous to going public is the analyst's inference, not the paper's; the Granger results concern cue shares and skepticism, not the partisan stance gap. | moderate | VERIFIED-SOFTENED |
| [[vanderdoes-2022-strategic-identity-signaling]] | 2022 | suggests-extension | The calibrated payoff model makes outgroup share of the audience the main driver of whether covert beats overt signaling, with cost a weaker driver, and covert messaging only reduces, not removes, outgroup dislikes. | The paper's audience is a tweet reader pool, not the Noble-Turner public-campaign setting. An outgroup-share term would need its own calibration on congressional data, and the paper's r_o = 0.5 cannot be ported as a parameter. | weak | UNVERIFIED |

### Assessment

**Evidence weight: the regime structure transfers. Three implementation choices depart from the source, one of them an internal contradiction.** 11 rows: 4 VERIFIED, 5 VERIFIED-SOFTENED, 2 UNVERIFIED. Five of the rows are on Noble & Turner 2024.

**What holds**
- The rally/retreat/backfire structure with beta_L > beta_R (co-partisans always favour the proposal more) is implemented in steps_for().
- It transfers most cleanly to the dashboard's executive cue, which matches the paper's presidential actor. It transfers less well to an ACU or industry actor, which has no shared reelection stake (delta).

**Contradiction (VERIFIED-SOFTENED, strong)**
- Noble & Turner predict going public mainly in *defense* of a favourable status quo; under partisan retreat the actor never goes public against an unfavourable one.
- defensive_appeal_value() gives held-count credit on every issue with no status-quo check.
- In the live salience_advisory.json (2026-10-08 15:37, denominator 18 issues), all four GO PUBLIC verdicts (top quartile, cut 68.27) are sq = offense. Technology & Emerging, close to the paper's pure-retreat case, ranks first. The three defend issues rank 13th-15th.
- These are heuristic advisory scores, not measured effects. Gating the held credit to sq == defend would restore the paper's ordering.

**Mappings that are design priors, not the paper's**
- **z from legislator tier gaps.** The paper treats elite polarization and base reactions as separate, and its own infrastructure example has them diverge.
- **psi from missing stance labels**, and backfire b = f(psi). The paper never links psi to backfire. Coverage gaps differ by party and chamber.
- **VULN 1.35.** The paper's rho and 'gambling for reelection' concern the president, not rank-and-file members. Prop 6 is also internally inconsistent on direction.
- **gridlock_width.** Midband occupancy across 533 members is a different quantity from the paper's gridlock interval and cannot test Prop 1.

**Rally +0.5 out-partisan step**
- Unsupported. Merkley's consensus cue is null and Bakker shows no positive out-party movement.
- Both papers support the negative out-partisan step in retreat and backfire, as a direction only.

**Intensity input**
- Joo 2022's alignment-to-intensity mapping has no estimate behind it.
- The 65% CU Tax Status input is inflated by ACU syndication, and it does not reach the chain's advisory anyway.

### Contradictions
- Noble & Turner 2024 (contradicts, strong): defensive credit is applied on sq = offense issues. All four live GO PUBLIC verdicts (18 issues, 2026-10-08) are offense issues, against the paper's prediction that going public is mainly a defense of a favourable status quo.
- Rally out-partisan +0.5 (bpf >= 0.30) is contradicted in direction by Merkley's null consensus cue and by Bakker's lack of any positive out-party movement (both mass public).

### Gaps
- Electoral vulnerability amplifier 1.35: Noble & Turner do not support it. It needs a separate electoral-incentives source or an internal test: do vulnerable members shift more after CU public campaigns?
- Base-side (mass or CU-member) opinion gap per issue: needed for z. The system has none.
- Salience onset timing: Zhang 2022 (UNVERIFIED) suggests a burst detector on press volume as a dated trigger. It must be tested against co-partisan shifts before any use, and volume must not be read as influence.

### Paul's position vs the evidence

Paul highlighted Noble & Turner heavily, and the component implements its regimes closely. The one place it departs is the place that drives the live verdicts: defensive credit on offense issues. Paul's reading and the paper agree on co-partisans moving more. They disagree with the code on when going public pays.

### Open questions
- Should held credit be gated to sq == defend? It would reorder the live GO PUBLIC verdicts. Paul's call.
- Should VULN 1.35 be relabelled as judgment, or sourced from the electoral-incentives literature?
- Is missing stance data a defensible proxy for preference uncertainty, given party and chamber coverage gaps?

### Read first

[[noble-2024-presidential-leadership-legislative]] [[merkley-nd-party-cues-news]] [[bakker-nd-understanding-partisan-receptivity]] [[joo-2022-image-data-automated]]

## Receptivity layer (susceptibility, incentive, prospect domain, seed probability)

**What it computes.** Per member: susc, susc_mult [0.70, 1.30], edge_mult [0.80, 1.20], pressure_mult = clip(susc x incent x pt, 0.50, 1.60), and seed_prob = clip(0.60 + 0.10 coop + 0.06 breadth + 0.04 visibility, 0.30, 0.90). The rank is not affected.

**Method.** Behavioural basis (default): susc = .40 cooperation_v2 + .30 N(affective) - .20 diligence - .10 dominance, blended 0.70/0.30 with persona when n >= 15 cues. Lexical basis: .40A + .30N - .20C - .10E. incent: retiring 0.60, else clip(1 + .30(comp - 0.5), 0.85, 1.15), neutral when COMPET_MULT is on. pt: 1.15 loss domain, 0.92 safe. PERSONA_W 0.30 is kept by owner ruling. Recorded double-count: cooperation_v2 vs bpf rho +0.888.

**Code refs.** `fec/members/receptivity_layer.py:1-216`, `fec/moneyball_core.py:185-213 (rc)`

**Assumptions**
- Neuroticism degrades decisions under pressure (Byrne et al. 2015); the A, C and E signs are design priors, not Byrne findings
- Final-term members are less responsive (Fouirnaies & Hall)
- Members in a live race are in the loss domain and take risks (Linde & Vis 2016)
- No N susceptibility signal has been demonstrated (Nokken-Poole drift MISS; H5 null)

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[report-2024-2024-house-glance]] | 2024 | qualifies | Cook's rating differs from PVI for incumbents with personal electoral strength. Fitzpatrick is EVEN and Likely R. Salazar is EVEN and Solid R. Cuellar is D+3 and Likely D, while Hayes is D+3 and Lean D. A candidate-specific vulnerability signal therefore carries information that PVI lacks. | This is the clearest example of the PVI-versus-rating gap affecting a live dial. It holds as long as COMPET_MULT=1 neutralizes the Ballotpedia comp term. A 2024 snapshot cannot calibrate 119th-Congress values; it only shows that the two measures diverge. | moderate | VERIFIED |
| [[linde-2016-politicians-take-risks]] | 2016 | qualifies | Strong reflection effect in the paid lottery task: risky choice 39.13% under gain vs 76.09% under loss (36.96 pp, binomial p=0.0002, n=46). In the four hypothetical scenarios (Table 4, Fisher tests): economic 45% vs 50% (p=0.774), votes 45.83% vs 40.91% (p=0.774), votes+economic 85% vs 69.23% (p=0.302), Asian disease 88.46% vs 30% (p<0.001). In a logistic regression over all four scenarios (n=164, covariates, clustered SEs) the scenario-specific loss-frame odds ratios are 0.860 (economic, p=0.827), 1.599 (votes, p=0.490), 8.684 (votes+economic, p=0.033) and 14.633 (Asian disease, p=0.001). Reflection thus appears in two of four scenarios; votes look like the reference point ('less strong but nevertheless suggestive'). Time in parliament does not predict risk-taking (OR 0.995, p=0.580). | Carries over: the reflection effect is real in politicians, and votes are the more plausible reference dimension, which fits keying the domain on a live race. Does not: (a) framing was randomly assigned in hypothetical scenarios, not inferred from seat security; (b) the sample is 46 Dutch MPs, 2014-2015, 76.2% right-wing; mapping onto US seat security is untested; (c) risk-seeking here means preferring higher-variance options, not greater responsiveness to money or pressure; (d) the 1.15/0.92 magnitudes and 'loss-averse overall' are unsupported (no loss aversion found). The votes+economic OR 8.684 is a scenario-specific interaction in a regression that pools scenarios; quote it with that label and alongside the non-significant Fisher test (p=0.302). | moderate | VERIFIED-SOFTENED |
| [[fouirnaies-2022-electoral-incentives-affect]] | 2022 | qualifies | Removing reelection incentives lowers visible effort: the productivity index falls -0.24 SD (SE 0.03), with a range of -0.09 to -0.41 across specifications. Ideological positioning does not shift: \|W-NOMINATE\| +0.02 (SE 0.01), the party-split estimates are about 0, and interest-group ratings show no systematic change. The authors argue that legislators who run for other offices keep their electoral incentives, so the estimates are likely a lower bound. | Weak for the 0.60 size and the 'responsiveness' reading. The paper estimates effort allocation in low-salience state chambers under exogenous, binding term limits. It does not estimate response to lobbying or money, and its null on positions points against extra movability or immovability. Congressional retirement is voluntary and self-selected, which is the confound the paper's within-person design is built to avoid. Its lower-bound argument (p.672) contradicts treating members who seek other office as having no electoral incentive. The direction (less effort in the final term) carries over; the magnitude and the mechanism of 'pressure response' do not. | moderate | VERIFIED-SOFTENED |
| [[poole-1999-nominate-after-years]] | 1999 | qualifies | The linear model is not a striking improvement over the constant model, about 0.3 percent in both chambers (Table 1: House 2D constant 85.65 vs linear 5-Congress 85.96; Senate 84.04 vs 84.40). The authors let legislators serving as few as 3 Congresses get linear terms, admitting random walkers will appear linear, a bias deliberately in favour of the polynomial models. | Interpretation only: low true-score variance in drift is consistent with the MISS but does not overturn or reopen it. Supports the brain's listed alternative (CU-specific vote-switch criterion), not another look. | moderate | VERIFIED-SOFTENED |
| [[birkenmaier-2025-measuring-politicians-public]] | 2025 | qualifies | The construct is the projected public persona under a lens model. The authors state that the relative weight of intrinsic personality and strategic self-presentation in any cue cannot be determined. The paper reports no behavioural outcome linking agency or communion to responsiveness or persuadability. | The paper supports measuring the projected persona only. The communion-to-susceptibility direction and the .60/.40 weights are design priors (Byrne-style), not findings of this paper. The PERSONA_W keep ruling belongs to the owner and is not revisited here. | moderate | VERIFIED |
| [[ramey-2016-measuring-elite-personality]] | 2016 | suggests-extension | Conscientiousness acts like a discount factor (fn 35), but fn 35 attributes that model to a separate Ramey, Klingler and Hollibaugh 2015 manuscript. Within this paper, more Conscientious minority members are less deterred by partisan obstacles because they weight future payoffs (pp.15-16); the C effect is smallest for the majority (p.18). | The paper concerns bill-type choice, not response to lobbying pressure, so transfer is analogical. Conditioning C on majority status would be a new proposal with no validation behind it. | weak | VERIFIED-SOFTENED |
| [[gaynor-2022-financial-ties-bind]] | 2022 | qualifies | Announced retirees are about 40% (R) and 45% (D) less likely to receive member LPAC ties. Republican leaders give to retirees at 39% (Table 4 coef -0.397***). The Table 5 retirement coefficient is -0.055 marked *** with SE 0.102, which is internally inconsistent. | Shows peers and leaders put less money into retirees, which fits treating retirees as low-value targets. Says nothing about whether retirees respond less to pressure, which is the mechanism the 0.60 factor encodes. The factor still rests on Fouirnaies and Hall, not this paper. | weak | VERIFIED |
| [[byrne-2014-chokes-under-pressure]] | 2014 | qualifies | Neuroticism was the only trait significant in the regression in both experiments. Exp 1: condition x N b = .45, p = .02 (minus sign lost in text extraction; authors describe a negative effect), interaction DR2 = .06, F(11,115) = 1.96, p = .04; under high pressure N b = .50, p < .01; under low pressure ns. Exp 2 (social plus time pressure, no low-pressure arm): N b = .31, p < .05; A b = .36, p < .01; model R2 = .19. Exp 1 agreeableness interaction only a trend, b = .33, p = .08. Authors call the agreeableness result exploratory. | Weak transfer. (1) Construct: the outcome is worse choices on a delayed-reward lab task, not being moved by a persuader. (2) Population: undergraduates earning course credit at one university, individual decisions, an $8 bonus; not legislators facing lobbyists. (3) Measurement: the paper uses self-report BFI; the system uses text or behavioural proxies (affective_n for N, cooperation_v2 for A). (4) Size: Exp 1 step-1 R2 = .10 (condition plus five traits); the interaction increment is DR2 = .06, so the paper gives no anchor for the plus or minus 30 percent multiplier range. The paper supports the direction of an N-under-pressure term only; it does not support A outweighing N or the size of the term. | moderate | VERIFIED-SOFTENED |
| [[byrne-2014-chokes-under-pressure]] | 2014 | qualifies | Under high pressure conscientiousness correlated negatively with performance, r = .27, p < .05 (Exp 1 correlation only). In the Exp 1 regression it was a trend, b = .16, p = .09. Experiment 2 found C not predictive: r = .16 ns, and Table 5 gives b = .16 with no significance mark. Authors say no clear conclusion about conscientiousness is possible. Extraversion and openness had no effect in either experiment. | The paper's C direction is opposite to the code's dampener sign, but the only evidence is one uncontrolled correlation that the authors decline to interpret and that did not replicate. That qualifies the term; it does not contradict it. For C and E, the honest wording is that this paper gives no support to the dampener. | weak | VERIFIED-SOFTENED |
| [[palm-2025-influence-personality-traits]] | 2025 | qualifies | Count-based susceptibility ranking across 80 studies: agreeableness High, extraversion Above average, conscientiousness Average, openness Below average, neuroticism Low (least susceptible). No pooled effect sizes. | Weak transfer. The review measures response to persuasion messages in consumers and students, using self-reported traits and perceived-persuasiveness outcomes in health and tech settings. The system's N slot is choking under pressure (Byrne), a different construct, and members' traits are read from text or behaviour, not self-report. The review supports the positive A or cooperation weight. It cuts against the positive N weight and gives no support to the negative E or dominance weight. It cannot set magnitudes because it reports no effect sizes. Neither sign change is recommended on this evidence alone: changing weights moves ranks and is a gated recalibration, which is Paul's call. | weak | UNVERIFIED |
| [[palm-2025-influence-personality-traits]] | 2025 | qualifies | Neuroticism is the least susceptible trait overall, yet Oyibo & Vassileva (2019) found it the trait most consistently correlated with responsiveness to social-influence strategies, and Gkika et al. (2016) found greater acceptance of social proof among high-neuroticism participants. | The paper suggests that the direction of N may differ by channel: positive for peer or social-proof influence, which the edge_mult cascade receiver models, and negative for direct persuasion appeals. That would argue against a single susceptibility dial for both channels. The paper's evidence is one or two persuasive-technology studies, so treat this as a hypothesis only. | weak | UNVERIFIED |
| [[palm-2025-influence-personality-traits]] | 2025 | suggests-extension | Trait-matched strategies are more effective than non-matching ones, and contra-matched (random, non-ideal) strategies are often counterproductive. Examples: agreeableness with reciprocity and liking; extraversion with liking, commitment, scarcity and reciprocity; neuroticism with commitment and consistency. | This only applies if lobbying asks were coded by appeal type, and the system has no such data. The matching evidence comes from health and persuasive-technology apps, with outcomes mostly measured as perceived persuasiveness rather than behaviour (Sec 5.3). Cialdini's 'reciprocity' here is a message strategy. It is unrelated to the system's unvalidated reciprocity assumption (D-104..D-108). | weak | UNVERIFIED |
| [[koutsoumpis-2024-kernel-truth-text]] | 2024 | supports | The strongest self-report category effects sit at \|rho\| .08-.14 (abstract p. 843), and the 52 categories explain 5.07 percent of self-report variance (Table 4 p. 854). Effects are typically stronger for longer texts (Table 6 continuous moderators, p. 854). | Closed-vocabulary rates carry little true trait variance, so a lexical composite is mostly noise. This supports keeping the lexical basis out of the default, but not the specific sqrt shrinkage form. | moderate | VERIFIED-SOFTENED |
| [[celli-nd-twenty-years-personality]] |  | qualifies | Computer Perception convergent validity is Rho 0.30 (Hinds and Joinson 2024). The paper names personality-aware marketing used to influence voting (cites [79]) as a threat and calls for explainable AI in Perception systems. | The 0.30 figure is a Perception convergent validity, not a Recognition figure; the bearing's earlier wording implied otherwise. The 9 percent shared variance is the reviewer's arithmetic, not the paper's. The voting threat concerns voters and consumers, so it is context for wording, not evidence against the layer. Owner ruling on PERSONA_W was not checked. | moderate | VERIFIED-SOFTENED |
| [[bakker-nd-understanding-partisan-receptivity]] |  | suggests-extension | Cue receptivity is not higher among low-resource people; the meta-analysis shows no larger cue effects for low resources. Among high-resource respondents the in-party cue effect rises with partisan identity strength: meta slopes CRT b=.09 (SE .04), NfC b=.15 (SE .05), latent b=.15 (SE .05). The roughly 15-percentage-point rise from lowest to highest identity strength is reported for latent resources only. | Weak. The paper studies the mass public; legislators are high-resource and strongly identified, so the moderator has little range in Congress. The paper concerns party cues, not lobbying pressure. The layer's design prior does not come from this paper. UNVERIFIED agent analysis (DE-062). | weak | VERIFIED-SOFTENED |
| [[azucar-2017-predicting-personality-traits]] | 2017 | qualifies | Agreeableness is the least recoverable trait from footprints (r .29 [.21,.36]). A gained no accuracy from multiple footprint types. Among the moderators tested, demographics was the only significant gain for A (beta .25, R2 .19). | The paper is about predicting self-report among lay users. It does not test susceptibility to pressure or any choking composite, so it cannot support the susceptibility weights. Its only bearing is that lexical A is the weakest footprint trait. The analyst's statement that the paper supports the system's move from lexical A to behavioral cooperation is the analyst's inference, not the paper's finding. | weak | VERIFIED |
| [[amsalem-2018-effect-politicians-personality]] | 2018 | qualifies | More visible MPs are less agreeable (beta -.116, p < .05) and more emotionally stable (neuroticism beta -.111, p < .05). The authors read this as media competition requiring psychological resilience (Discussion). | On the default path the tension is real but indirect. Visible MPs in the paper have lower A and lower N. Lower cooperation and lower affective N both lower susc_trait, while visibility_z raises seed_prob. Inside seed_prob, the visibility term (+.04) and the cooperation term (+.10) can pull in opposite directions if visible members are less cooperative. This is not a measured US result: the paper's betas are small, self-reported and cross-national, and seed_prob is not an access or introduction probability (D-025). Check corr(visibility_z, coop_v2) and corr(visibility_z, affective N) on the roster; not yet run (DE-062). | weak | VERIFIED-SOFTENED |
| [[selfhout-2010-emerging-late-adolescent]] | 2010 | qualifies | Effects of one person's own traits are small next to similarity effects. Extraversion predicts making more ties (ego .10), agreeableness predicts being chosen (alter .14), and openness has no ego or alter effect (.02, n.s.). | Transfer is weak. The paper measures friendship nomination, not whether a contacted official acts. Within that limit, it gives no support to the openness term in the lexical basis. The agreeableness result concerns being approached, not acting once approached. The paper's main point, that pairwise similarity outweighs individual levels, shows a dyadic term is missing, but there is no legislative evidence for adding one, and the system's own persona-homophily test came out null. | weak | UNVERIFIED |
| [[lechner-2025-mapping-political-personality]] | 2025 | qualifies | Observer correlations in Table 5 (Trump n=104, Merkel n=107, Study 4, Apr 2021): Empathy with likeability r=.70 (Trump) and .76 (Merkel); Integrity with ideological proximity r=.46 (Trump) and .36 (Merkel); Intellect with proximity r=.48 (Trump). All proximity correlations are positive as printed. PoliPers tests no link from any facet to behavior under pressure. | The correlations come from observers rating two foreign leaders on same-occasion ratings. They do not bear on how members of Congress respond to lobbying. Nothing here validates or refutes the persona signs, and the sign of the proximity association is unresolved (see verifier note). | weak | VERIFIED-SOFTENED |
| [[simchon-2024-persuasive-effects-political]] | 2024 | qualifies | Persuasion depends on the fit between message style and recipient openness, above and beyond the recipient's own openness: mismatch coefficient b = -0.07 (study 1a, n = 440) and -0.08 (study 1b, n = 804) per SD of \|z(O_person) - z(O_ad)\| on a 1-5 perceived-persuasion scale, b = -0.03 for ChatGPT-tailored ads (n = 804), with person and ad openness held as separate predictors. | Holds only if trait-message fit operates for legislators receiving lobbying asks as it does for UK Prolific adults rating Facebook ads; the outcome is self-reported perceived persuasion, not a vote or cosponsorship; effects are small (simulated 2,490 per 100,000 at a midpoint cutoff). Any matching term would need a message-side trait score Melange does not compute, and would stay UNVERIFIED until tested against the prospective H1/H2 contract. | weak | UNVERIFIED |
| [[kumari-nd-five-personality-predictions]] |  | context | Pitches text-derived personality as directly usable for business targeting and mental-health uses (Sec 4), with no validation of whether traits predict any behaviour. | This is only a caution. The paper's implicit claim that a text-derived trait is action-ready is exactly what the system's single-look record refuted for legislators. Nothing in the paper should move the system back to a lexical basis. | weak | UNVERIFIED |
| [[celli-2012-role-emotional-stability]] | 2012 | qualifies | Neurotic users post and retweet more, and calm users reply slightly more (Fig. 5, no values in the text). Neurotic networks form longer chains (average path 1.551 vs 1.334; diameter 6 vs 5), and calm networks have more mutual ties. | Celli measures communication style and ego-network shape, not response to pressure or persuasion. The data are anonymous 2011 public Twitter users with tagger-inferred labels, not legislators. The result gives no support for N as a susceptibility multiplier. The only thing that transfers is a caution: if text-N tracks posting volume, a volume confound could enter N. The system residualizes on E and party but has no stated volume control. That control is untested here. | weak | UNVERIFIED |

### Assessment

**Evidence weight: the most rows and the weakest support. Every multiplier's size is unsupported, and two citations are misapplied.** 22 rows: 4 VERIFIED, 11 VERIFIED-SOFTENED, 7 UNVERIFIED.

**Susceptibility traits**
- Byrne 2014 supports only the direction of an N-under-pressure term. The population is undergraduates on a delayed-reward task, with self-report BFI and an interaction increment ΔR² = .06.
- Byrne does not support A outweighing N. Experiment 2 has no low-pressure arm, so the docstring's 'Exp-2 interaction' cannot exist. The A slot is filled by cooperation_v2, which has a recorded rho of +0.888 with bpf (a double count).
- Byrne's only C evidence is one uncontrolled correlation whose sign is opposite to the code's dampener. The authors decline to interpret it, so it qualifies the dampener rather than contradicting it.
- Text-trait validity is low. Koutsoumpis 2024: LIWC categories explain 5.07% of self-report variance. Azucar 2017: A is the least recoverable trait. Celli: convergent validity rho 0.30. This supports keeping the lexical basis out of the default.
- The system's own record agrees: Nokken-Poole drift MISS (n = 441), H5 null, no demonstrated N signal. Poole 1999's low drift variance is consistent with that MISS; it does not reopen it.
- Leads (UNVERIFIED): Palm 2025 ranks N *least* susceptible and suggests N's sign may differ by channel. Celli 2012 flags a possible posting-volume confound in text-N.

**Final-term factor (0.60)**
- Fouirnaies & Hall 2022 is evidence from state legislatures under binding term limits. Productivity falls 0.24 SD and ideological positioning does not move.
- It supports less effort in the final term. It does not support less responsiveness to pressure, and it does not support the 0.60 size.
- Its lower-bound argument (members running for other office keep their electoral incentive) contradicts including members such as Barr and Stefanik in the retiring list.
- The docstring also credits Fouirnaies & Hall for the competitiveness term 1 + .30(comp - 0.5), which the paper does not test.
- Gaynor 2022: retirees receive fewer LPAC ties. That is about where peers send money, not how retirees respond.

**Prospect domain (pt 1.15 / 0.92)**
- Linde & Vis supports reflection: 39% vs 76% risky choice under gain vs loss, n = 46. Votes are a plausible reference dimension.
- It does not support the magnitudes, the mapping from seat security to framing, or 'risk-seeking = more movable'. The docstring's 'loss-averse overall' is unsupported, because no loss aversion was found.

**COMPET_MULT side effect (VERIFIED)**
- With COMPET_MULT on, the Ballotpedia candidate-specific comp term is set to neutral. Cook 2024 shows ratings and PVI diverge (Fitzpatrick EVEN/Likely R; Salazar EVEN/Solid R).
- So the engine path no longer sees candidate-specific vulnerability.

**Persona channel**
- Birkenmaier and Lechner validate measuring a *projected* persona only. Neither paper links persona to behaviour under pressure.
- PERSONA_W 0.30 is kept by owner ruling and is not revisited here.

pressure_mult and seed_prob are heuristic multipliers, not access probabilities (D-025).

### Contradictions
- Fouirnaies & Hall 2022: members running for other office keep electoral incentives (the lower-bound argument). The retiring list applies incent 0.60 to members seeking other office.
- Fouirnaies & Hall is cited for the competitiveness term 1 + .30(comp - 0.5), but the paper does not test seat competitiveness.
- Byrne 2014: the docstring cites an 'Exp-2 interaction' for A, but Experiment 2 has no low-pressure arm, so no interaction can be estimated.
- Linde & Vis 2016: the docstring says politicians are 'loss-averse overall', and the paper found no loss aversion.
- With COMPET_MULT on, the candidate-specific Ballotpedia vulnerability is set to neutral and only PVI-based c remains. Cook ratings show PVI and candidate risk diverge (VERIFIED).

### Gaps
- Does higher susceptibility predict more cosponsorship or vote change after contact? This is prospective H2, still untested, and no literature measures it for legislators.
- Are retirees less responsive to PAC money in roll-call alignment (Congress, voluntary retirement)? Fill with a within-member final-term versus prior-term comparison of money-alignment slopes, excluding members who run for other office.
- Do members in live races take riskier CU positions? There is no US legislator evidence.
- Channel-specific N (peer influence versus direct appeal): Palm is a lead only, and testing it needs edge-level versus pressure-level outcomes.

### Paul's position vs the evidence

Paul wants traits to adjust the response to pressure as edge parameters (Oropeza). The evidence supports measuring a projected persona and gives direction-only support for an N-under-pressure term. It does not support the composite weights, the A-over-N ranking, or any demonstrated susceptibility signal in members. The project's own validity record (drift MISS, H5 null, lexical traits rated unfit) weighs against the layer more than Paul's notes acknowledge. His observation that the 10 personalities collapse into 4 observable ones fits the low text-trait validity the meta-analyses report.

### Open questions
- Should members seeking other office be removed from the incent 0.60 list?
- Should the competitiveness attribution to Fouirnaies & Hall be removed from the docstring?
- With COMPET_MULT on, should candidate-specific vulnerability re-enter somewhere, since PVI misses it?
- Is there a posting-volume confound in affective N? Check corr(N, statement count) on the roster.

### Read first

[[fouirnaies-2022-electoral-incentives-affect]] [[linde-2016-politicians-take-risks]] [[byrne-2014-chokes-under-pressure]] [[report-2024-2024-house-glance]] [[koutsoumpis-2024-kernel-truth-text]] [[palm-2025-influence-personality-traits]]

## Competitiveness multiplier c (COMPET_MULT)

**What it computes.** c = 1 + 1.0 x exp(-max(lean, 0)/5.0), range (1, 2], where lean = Cook PVI oriented to the member's party. It scales effective dollars on both money legs.

**Method.** Matched by Cook incumbent or district, with overrides; unmatched members get 1.0. Default ON since 2026-10-06 (D-104/D-105/D-107/D-108). Real dollars are unchanged.

**Code refs.** `fec/moneyball_core.py:74-172`, `fec/engine/build_competitiveness.py:1-304`, `data/money/competitiveness_c.json`

**Assumptions**
- Dollars buy more in competitive seats (reciprocity NOT validated)
- AMPLITUDE and SCALE are uncalibrated owner judgment

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[report-2024-2024-house-glance]] | 2024 | qualifies | Cook's table separates PVI from race rating. Parsed per district row (435 rows), the 24 Toss Ups span R+8 (AK-AL) to D+5 (CA-22, NY-04). The six EVEN seats get five different ratings: FL-27 Solid R, PA-01 Likely R, NH-01 Likely D, PA-17 Lean D, CO-08 and NE-02 Toss Up. Seats at D+2 or D+3 are Solid D (AZ-04, CT-02, GA-02), Lean D (CT-05, NY-03) or Toss Up (OR-05, CA-45, NY-17; all three held by Republicans). Window: 2024 cycle as of 2024-09-12. Denominator: 435 districts. | The structural point carries over: PVI is a partisan baseline, while the rating adds incumbent and candidate factors. The numbers do not carry over: they use 2024 PVIs, which come from the 2016 and 2020 presidential results, and 2024 ratings, while the system reads Cook 2026 PVIs for the 119th Congress roster. It applies only to House members; senators use Paul's state PVI table. The table says nothing about whether money works better in competitive seats, so it neither validates nor refutes the reciprocity premise behind c. | moderate | VERIFIED-SOFTENED |
| [[blackwell-2026-effect-political-advertising]] | 2026 | qualifies | Groups may target markets with more persuadable voters using a metric the researcher cannot see, so a naive comparison of treated and untreated units is confounded. The paper handles only time-constant unmeasured confounding, through unit fixed effects in the propensity score. Time-varying unmeasured confounding still breaks identification. | No direct transfer. The paper estimates effects of ads on voters, not the responsiveness of legislators to PAC dollars, and it must not be cited as support for c (D-104..D-108). Its only bearing is on calibration: any future attempt to estimate c from observed giving and alignment must handle strategic targeting of competitive seats, because givers pick those seats on information the analyst lacks. | weak | VERIFIED |
| [[gaynor-2022-financial-ties-bind]] | 2022 | qualifies | Vulnerable members (Cook PVI likely, Oct 2018, dichotomous) are about 70% (R) and 75% (D) more likely to receive a member-to-member LPAC tie, per the text. Republican leaders give to vulnerable members at 73% (Table 4 coef 1.004***). 115th Congress, stratified by party; denominators 238 R and 168 D connected nodes (Table 2-3 notes). | Paper is evidence on where donor LPAC money goes (binary tie presence, Oct 2018 dichotomy, member-to-member only). It does not show a dollar buys more in a competitive seat, which c encodes. It supports Cook PVI as the signal donors act on, not the continuous exp(-lean/5) decay. Neither validates nor refutes D-104..D-108. | moderate | VERIFIED |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | qualifies | Table 2 (printed p.20, PDF p.21), individual donors (5,000 random donors with 25+ federal gifts, 2004-2012): LogLik ideological -1334200, partisan electoral -1405855, investor -1504601, baseline -1520312. Ideological fits best of these. Paper says partisanship is a coarsened ideology measure, so party vs ideology variance is hard to separate. Largest shifts in period-specific CFscores from candidate covariates are 0.03 to 0.05, about a twentieth of an SD (supplement Table 1 text). Paper models donor allocation and does not estimate a dollar's effect on the recipient. | Paper supports neither c nor its calibration: its dependent variable is where individual donors send money, not what a dollar buys in a competitive seat. Sample is individuals 2004-2012 only. Any use of the partisan-electoral result must note that party and ideology are collinear. | moderate | VERIFIED-SOFTENED |
| [[kalla-nd-campaign-contributions-facilitate]] |  | context | Offices were blocked on Obama's 2012 two-party vote share among six covariates, with omnibus balance p=.92 (chi-square 1.44, 5 df). The paper reports no heterogeneity of the donor-label effect by seat competitiveness. | Blocking on partisanship balances it across arms but does not estimate whether the donor effect differs in competitive seats; 64 treated offices could not have estimated that. The paper gives no support for c. The multiplier stays unvalidated (D-104..D-108). | weak | VERIFIED |
| [[cagala-2021-optimal-targeting-fundraising]] | 2021 | qualifies | Responsiveness to the gift is heterogeneous and has to be estimated. Causal-forest best linear predictor heterogeneity loading 0.959 (SE 0.710; p 0.088 one-sided) on the warm list and 0.511 (SE 1.069) on the cold list (Table E.3). The 2014 rule applied to a caliper-matched 2015 warm-list sample (radius 0.1, 35.6 percent treated) gave +2.64 vs all-gift and +2.30 vs no-gift. On the full 2015 sample it gave no significant gain. The paper reports significance against no-gift up to caliper radius 0.25 and says effects are not significant above radius 0.1. | The paper is about method, not legislators or PVI. It shows what establishing heterogeneous dollar effectiveness would require: randomized variation, a heterogeneity test and a transfer check to a new population. It gives no evidence for or against the PVI shape of c. Do not cite it as support for c. | moderate | VERIFIED-SOFTENED |
| [[best-2022-redistricting-commissions-avoid]] | 2022 | qualifies | District partisanship is partly a product of how lines are drawn. Compared with 10,000 neutral plans per state, enacted maps added about +1.5 points of pro-Democratic asymmetry (median minus mean) in Arizona, 1.7-3.7 points against Democrats in New Jersey and 2.7-4.0 points against Democrats in Virginia (10 of 10 elections; fewer than 1% of neutral plans as skewed). The Arizona commission's districts had larger SDs than expected, meaning packing in both directions and fewer competitive seats than neutral drawing would produce. | The paper's shifts are state-level skews in two-party vote share. They are not per-district PVI changes, so they show only the order of magnitude by which a map can move lean. Under the model formula, a 3-4 point lean change moves c by roughly 0.45-0.55 near lean 0. That makes c partly a function of map choice, which matters most in the flagged 2025-26 mid-decade redistricted states. The paper covers the 2010 round and congressional districts only, and it says nothing about whether contributions are worth more in competitive seats. It must not be cited as support for the reciprocity premise (D-104..D-108). | weak | UNVERIFIED |
| [[becker-nd-gameability-redistricting-criteria]] |  | suggests-extension | Population-based change and incumbent-based change can diverge sharply. In Minnesota, Plan 8 and Plan 9 have almost the same People Change (1.93 percent vs 1.90 percent), yet Incumbent-People Pair Change is 1.93 percent vs 13.90 percent, because one incumbent precinct was moved. The text says Plan 9 moves 0.03 percent more people than Plan 8, which Table 3 contradicts. | Holds where a member's seat was redrawn between the 2025 and 2026 maps. A constituency-turnover measure would be a sharper diagnostic than a state flag or PVI delta, but it needs block assignments for both maps and would only inform Paul's override review. c stays PVI-defined. Reciprocity is not validated. | weak | VERIFIED |
| [[mccartan-2022-simulated-redistricting-plans]] | 2022 | suggests-extension | District partisanship is a function of the map. In Georgia the enacted 12th and 14th districts have 2020 presidential Democratic two-party shares above the range of 5,000 simulated plans (p.8, Fig. 2). Partisan metrics depend on the statewide elections available in each state, so cross-state comparisons are not valid (p.5, p.8). | The ensembles cover 2020-cycle maps only. They do not include the 2025-26 mid-decade redraws that the system flags, and they describe counterfactual neutral maps, not the realized seat a member runs in. At most they could add a per-district map-sensitivity band or a cross-check of Cook lean against ndshare. Per the paper, ndshare is not comparable across states. Reciprocity and the c amplitude/scale stay unvalidated (D-104..D-108). This is not evidence for them. | weak | UNVERIFIED |
| [[bouton-2022-small-campaign-donors]] | 2022 | context | Relative to safe Democratic seats, close races (mean Democratic winning margin between -10 and 10 percent over the last two elections) receive 0.11 pp more of a small donor's giving (48 percent of mean, not significant) and 0.18 pp more of a large donor's (75 percent of mean, significant at the 5 percent level). Sample is Democratic congressional candidates 2012-2020 with donor fixed effects. | The paper measures where individual donors send money, not what a dollar buys from a legislator, so it neither supports nor refutes c. Quote the close-seat figures only as Democratic candidates only, UNSTRATIFIED by party. | weak | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: none for the premise. Every row is about donor allocation, measurement, or confounding.** 10 rows: 4 VERIFIED, 4 VERIFIED-SOFTENED, 2 UNVERIFIED. Reciprocity is NOT validated (D-104 to D-108), and nothing here changes that.

**Allocation evidence, not effectiveness**

These rows show givers *target* vulnerable seats:
- Gaynor 2022 (VERIFIED): vulnerable members are about 70% (R) and 75% (D) more likely to receive member LPAC ties. 115th Congress, Oct 2018 Cook dichotomy, 238 R and 168 D connected nodes, stratified by party.
- Bonica 2013: individual donors 2004-12. The partisan-electoral model fits *worse* than the ideological one.
- Bouton 2022: close races get +0.18 pp of a large donor's giving. Democratic candidates only, 2012-20, so UNSTRATIFIED by party.

None shows that a dollar buys more in a competitive seat, which is what c encodes.

**Confounding and measurement**
- Blackwell 2026: strategic targeting of competitive seats on information the analyst lacks confounds any future calibration of c.
- Cagala 2021: what establishing heterogeneous dollar effectiveness would require (randomized variation, a heterogeneity test, transfer to a new population).
- Kalla & Broockman blocked on partisanship and report no heterogeneity by competitiveness; 64 treated offices could not detect it.
- Cook 2024 (435 districts, as of 2024-09-12): PVI and race rating diverge. The six EVEN seats get five different ratings, and Toss Ups span R+8 to D+5. So c (PVI-only, maximum 2.0 at EVEN or against-party lean) gives Salazar (Solid R) and Bacon (Toss Up) the same c.
- Best 2022 and McCartan 2022 (UNVERIFIED): district lean is partly a product of the map. A 3-4 point shift moves c by about 0.45-0.55 near lean 0, which matters most in the ten flagged mid-decade redistricted states.
- Becker: a constituency-turnover measure would be a sharper redistricting diagnostic than the state flag.

### Contradictions
- none recorded

### Gaps
- Whether PAC dollars have a larger marginal effect on alignment or access in competitive seats is the core premise and has no evidence. Fill with an identified design: a randomized donor treatment stratified by seat competitiveness, or quasi-experimental variation such as close-election shocks.
- Scan leads (UNVERIFIED): a 2024 AEA conference paper reports that the link between carbon-firm money and climate-skeptic votes is stronger under electoral pressure, identified from narrow incumbent defeats. Levitt 1998 argues PAC returns run mainly through the election channel, which would favour marginal seats for a different reason than reciprocity. Neither has been read.
- Decay scale in PVI points (SCALE 5.0): no evidence. Only estimable once the premise itself has an identified effect.

### Paul's position vs the evidence

Paul endorses Cook PVI as a vulnerability measure (Gaynor uses it), and the evidence agrees that donors act on it. He reads competitive seats as responsive. No source he annotated, and no row in the hub file, says dollars buy more there. AMPLITUDE and SCALE stay owner judgment, and the dial runs ON by default on an unvalidated premise. The Cook table also shows PVI misses candidate-specific risk, which the Ballotpedia term used to carry before COMPET_MULT neutralised it.

### Open questions
- Should c use the Cook race rating, or blend it with PVI, rather than PVI alone?
- How many members sit in the ten flagged redistricted states with c near the steep part of the curve?
- Does the Levitt election-channel argument suggest c belongs on electoral plays rather than on persuasion money legs?

### Read first

[[gaynor-2022-financial-ties-bind]] [[blackwell-2026-effect-political-advertising]] [[cagala-2021-optimal-targeting-fundraising]] [[report-2024-2024-house-glance]] [[bouton-2022-small-campaign-donors]] [[best-2022-redistricting-commissions-avoid]]

## Electoral security, leverage and contest-defend plays

**What it computes.** Security percentile from Ballotpedia Election_Average; leverage = 1 + 0.8(1 - security); play routes contest-primary, contest-general, defend or none.

**Method.** Defend value = comp x positive gap vs the cross-party average. Contest route = the larger of the primary and general gaps. Contest plays convert opponents in spread().

**Code refs.** `fec/engine/build_electoral.py:7-31`, `fec/engine/build_electoral_plays.py:20-36`

**Assumptions**
- Ballotpedia electoral average measures security

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[report-2024-2024-house-glance]] | 2024 | suggests-extension | The table gives a forward-looking expert risk category (Solid, Likely, Lean, Toss Up) for each seat, along with OPEN and VACANT flags: 41 OPEN lines and 4 VACANT seats as of 2024-09-12. | Useful only as the design pattern for a dated, forward-looking risk input. Using it would require the current-cycle Cook ratings, dated before the decision, not this 2024 table. Open-seat retirees would also need the same retiring treatment the receptivity layer already gives them. | weak | VERIFIED-SOFTENED |
| [[gaynor-2022-financial-ties-bind]] | 2022 | qualifies | Vulnerability is associated with higher inward LPAC degree, for both raw and weighted degree, from a regression reported in the text. Leadership is associated with higher outward degree. Givers target electorally exposed co-partisans. | Backs vulnerability as a magnet for co-partisan money, consistent with defend plays. Gives no evidence that competitive seats are cheaper to move. The paper uses Cook PVI while this component uses Ballotpedia Election_Average, so two vulnerability measures feed the engine. Applies within party only. | weak | VERIFIED |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | qualifies | Text PDF p.20: the partisan electoral model's main implication is that donors fund competitive races to maximize party seat share. Table 2 (PDF p.21): for individuals partisan -1405855 vs ideological -1334200, so the partisan electoral model fits worse. Paper also says the ideological model significantly improves fit over a partisan model. | The paper states a hypothesis it rejects for individual donors; it neither supports nor refutes the comp weighting. It describes donor allocation, not the payoff of a play. | weak | VERIFIED-SOFTENED |
| [[chohan-2025-crypto-oligarchy-cryptocurrency]] | 2025 | context | Fairshake campaigned against Katie Porter in California's Senate primary and opposed Jamaal Bowman's re-election bid. The paper calls this influence 'decisive' and offers no counterfactual or estimate. | Shows that federal primary-contest plays by an industry actor occurred in 2024. It is not evidence that such plays work, because outcomes are confounded and there is no comparison group. Do not use it to calibrate conversion. | weak | VERIFIED-SOFTENED |
| [[porter-2025-campaignview-campaign-platforms]] | 2025 | suggests-extension | Platform text for 5,228 of 6,016 (86.9%) major-party ballot-eligible House primary candidates 2018-2022, with 43,465 hand-topic-coded platform points. Identifiers are FECCandID (not universal) and BioGuideID (incumbents only). | House only, 2018-2022 cycles; the 14 topic codes are not CU categories. BioGuideID is incumbent-only and the paper advises against aggregating by either ID. Challenger coverage gaps (low fundraising, uncompetitive races) are where the proxy matters most. New analysis stays UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: weak. No evidence that plays convert anyone.** 5 rows: 1 VERIFIED, 4 VERIFIED-SOFTENED.

- Gaynor 2022 (VERIFIED) backs vulnerability as a magnet for co-partisan money, within party, which fits defend plays. It gives no evidence that low-security members are cheaper to move.
- Bonica 2013 states the partisan-electoral hypothesis (donors fund competitive races) and rejects it for individual donors. It neither supports nor refutes the comp weighting.
- Chohan 2025 documents that industry primary plays occurred (Fairshake against Porter and Bowman, 2024). It has no counterfactual and must not be used to calibrate conversion.
- Cook 2024 suggests a forward-looking, dated risk input could replace the backward-looking Ballotpedia Election_Average.
- CampaignView (Porter 2025: 5,228 of 6,016 House primary candidates, 2018-22) could supply challenger platforms for the contest-primary route. It is House-only, its topics are not CU categories, and the IDs are imperfect.

**Inconsistency.** The engine uses two vulnerability measures: Ballotpedia security here, Cook PVI in c. With COMPET_MULT on, the two can disagree on the same member.

### Contradictions
- Internal: electoral plays use Ballotpedia Election_Average security, while the competitiveness multiplier uses Cook PVI. Gaynor and the Cook table show the two measure different things.

### Gaps
- Are CU PAC dollars more effective in lower-security members? This shares the unvalidated premise behind c and needs the same identified design.
- Contest plays converting opponents: no evidence. Fill with a comparison of positions held by successors versus defeated incumbents in seats targeted by advocacy primaries, against matched untargeted seats.
- Ballotpedia security as a measure of current-cycle risk: untested against dated ratings.

### Paul's position vs the evidence

Paul highlighted Bonica's partisan-electoral model (donors direct funds to competitive races). The paper fits that model *worse* than the ideological one for individual donors, so the highlight describes a hypothesis Bonica rejects. Nothing in the evidence addresses whether contest or defend plays work.

### Open questions
- Should security switch to a dated, forward-looking rating?
- Should one vulnerability measure feed both the plays and c?

### Read first

[[report-2024-2024-house-glance]] [[gaynor-2022-financial-ties-bind]] [[porter-2025-campaignview-campaign-platforms]] [[bonica-2013-mapping-ideological-marketplace]]

## Scenario runner (what-if)

**What it computes.** Change in coalition spread and flip value under user money, defection or stance moves, using CRN at MC 40.

**Method.** A defection shifts all movable positions by at most 1 fine tier.

**Code refs.** `fec/engine/scenario.py:1-120`

**Assumptions**
- A defection is a general disposition shift across issues

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[weimer-2023-causal-approach-detecting]] | 2023 | qualifies | The paper's Monte Carlo is a negative control for an estimator applied to observed data: simulate data with no effect, re-run the matching estimator, expect a null. | A Weimer-style placebo applies to empirical estimators that read observed outcomes (forecasters, the contract), not to the forward cascade's deltas, which are model outputs. | weak | VERIFIED |
| [[harrison-2010-introduction-monte-carlo]] | 2010 | context | Monte Carlo is a key tool for analytically intractable problems. Some feel it is overused as a lazy way out, and analytic and experimental alternatives should be considered before committing to simulation. | The split between closed-form flip math and MC spread fits the paper's advice. The paper offers nothing on which analytic proxy (Viral Centrality, Katz-Bonacich) might replace the cascade MC; that comparison would need its own test. | weak | VERIFIED-SOFTENED |
| [[xu-2024-improved-lite-based]] | 2024 | qualifies | Paper reports node updates of 18,468 vs 72,835 (freezing) and 16,864 vs 114,929 (melting) for its improved D* Lite against full global re-planning. Numbers are correct. The paper frames reuse of earlier search data as the motivation (Sec. 2.1) but does not say D* Lite family planners save work only by reuse. | Documentation point only. At 533 members and MC=40 a full recompute is cheap, so incremental reuse is not warranted. The Xu numbers say nothing about the cascade. The cited work is Ren et al., not Xu. | weak | VERIFIED-SOFTENED |
| [[bharathi-nd-competitive-influence-maximization]] |  | qualifies | The best-response guarantee holds only for the last committer. Pure Nash equilibria need not exist and mixed ones do (lines 83-85). Theorem 2 bounds the price of competition at 2 on total activations, not on any one player's share. First-mover results cover only a duopoly on disjoint lines (Lemma 2, lines 149-153). | The fixed-opponent framing holds only if industry commits first and never reacts. FEC filings are public, so if industry reacts, CU becomes a first mover on a general graph, where the paper gives no algorithm or guarantee. The price-of-competition bound says nothing about CU's own share. | weak | VERIFIED |

### Assessment

**Evidence weight: tooling choices supported. The behavioural assumption is a gap.** 4 rows: 2 VERIFIED, 2 VERIFIED-SOFTENED.

- Harrison 2010 fits the split the runner already uses: closed-form pflip for flips, MC only for cascade spread.
- Weimer 2023 (VERIFIED) clarifies that a CRN zero-delta on an unperturbed scenario is an identity property of the simulator, not a negative control. Placebo tests belong on empirical estimators.
- Bharathi (VERIFIED): the (1-1/e) best-response claim holds only for a follower facing a fixed opponent. If industry reacts to public FEC filings, CU becomes a first mover on a general graph, where no guarantee exists. The price-of-competition bound says nothing about CU's own share.
- Xu 2024 is a citation mismatch: the docstring cites Ren et al. D* Lite, but isolation comes from CRN, not from reusing search state. At 533 members and MC = 40 a full recompute is cheap anyway.

The core assumption, that one defection shifts all movable positions by up to one fine tier as a general disposition shift, has no evidence.

### Contradictions
- none recorded

### Gaps
- Defection as a cross-issue shift: when a member switches stance on one CU issue, do they move on others? Fill with an internal panel of stance-tier changes across CU categories within member, testing co-movement against an independence null, with the window and number of members stated.
- Opponent reaction: is industry money reactive to CU spending? Fill with a timing analysis of FEC filings.

### Paul's position vs the evidence

Paul treats Monte Carlo as a tool and highlighted Weimer's use of it as a negative control. The runner's CRN zero-delta is not that kind of control, because it holds by construction. A Weimer-style placebo belongs on the forecasters and the outcome contract, not here.

### Open questions
- Should the scenario docstring's D* Lite citation be corrected to describe CRN isolation?
- Should scenarios offer a reacting-industry variant?

### Read first

[[bharathi-nd-competitive-influence-maximization]] [[harrison-2010-introduction-monte-carlo]] [[weimer-2023-causal-approach-detecting]]

## Priority anchor, leave-one-out orientation and engagement signal

**What it computes.** Topic priority = tier anchor (1, 2/3, 1/3, 0) + alpha x IMP-weighted stance on other topics (alpha 0.15 flip / 0.25 coalition); engagement = block-weighted legislative activity; base = max(0, eng + 0.15 x priority).

**Method.** Leave-one-topic-out orientation. Fails closed on an incomplete roster.

**Code refs.** `fec/moneyball_core.py:255-317,573-625`

**Assumptions**
- Stance on other CU issues predicts priority on this issue

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | qualifies | The second dimension now adds less than 1 percent improvement over the first dimension in both chambers. | engine_orientation reads category_stance scores, not votes. The Path V vote shares (UNVERIFIED) show opposite party direction across some categories (CFPB, Technology, Exam, CU Capital Requirements lean R; Charter, NCUA Budget & Insurance lean D) and none in others. If stance scores share that pattern, leave-one-out borrowing could import party sign; that has not been checked on the stance scores. Alpha is small. | weak | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: effectively a GAP.** 1 rows: 1 VERIFIED-SOFTENED. Weak.

Poole 1999 notes that a second NOMINATE dimension adds under 1% of fit. Low dimensionality makes it plausible that stance on other issues carries information about the held-out one. That is about votes, not CU category stance scores.

The row also raises a risk. UNVERIFIED Path V vote shares show party direction reversing across CU categories (some lean R, some lean D). If the stance scores share that pattern, leave-one-topic-out borrowing could import party sign into priority. Alpha is small (0.15 flip, 0.25 coalition), which limits the damage.

This component needs an internal test more than literature.

### Contradictions
- none recorded

### Gaps
- Does leave-one-topic-out stance predict held-out-topic stance? Fill with an internal held-out validation on the 533 x category stance matrix: rank correlation of the LOO orientation with the held-out tier, stratified by party, reported per category with denominators.
- Engagement block weights: no source.

### Paul's position vs the evidence

No literature, and none is really needed. The question is empirical and answerable on the system's own stance matrix.

### Open questions
- Do CU category stance scores reverse party direction across categories, as the UNVERIFIED Path V vote shares suggest? If so, does LOO orientation import party sign?

### Read first

[[poole-1999-nominate-after-years]] [[aref-2021-identifying-hidden-coalitions]]

## Across components

**1. Out-party influence is treated three different ways in one engine.**

The evidence is consistent on sign, though all of it comes from mass publics, staff or EP members, not US legislator-to-legislator ties: out-party cues do not persuade and often backfire (Bakker, Merkley, Furnas by stratum, Ringe). The engine handles them three ways:

| Component | Treatment of out-party influence | Fit with the evidence |
|---|---|---|
| lt_priming | Clamps non-bridge out-party cues to about 0 | Fits the evidence |
| Kernel diffusion | Keeps a 0.4 floor | Positive pull the evidence does not support |
| Flip model (cpf_pressure) | Out-partisan weight 0.5 | Positive pull the evidence does not support |
| Salience rally | +0.5 out-partisan step | Positive pull the evidence does not support |

There should be one documented choice. Unifying the treatment is a gated recalibration and Paul's call. The UNVERIFIED Fong lead (expert cues crossing party lines) is the main thing that could justify a bridge or expertise exemption.

**2. Labels outrun the evidence.**
- WM/WP and MAXSHIFT are labelled CALIBRATED, and no source calibrates them.
- LOSSFRAME and the receptivity docstring cite Linde & Vis for loss aversion that the paper did not find.
- Fouirnaies & Hall is cited for a competitiveness term it never tested.
- Byrne is cited for an Experiment 2 interaction that its design cannot estimate.
- The scenario docstring cites a D* Lite reuse mechanism the code does not use.

These are documentation corrections. Engine values do not need to change.

**3. Two vulnerability measures, and one premise with no evidence.**
- Cook PVI drives c. Ballotpedia security drives the electoral plays and, before COMPET_MULT, the receptivity incentive term.
- With COMPET_MULT on by default, candidate-specific risk has dropped out of the engine path. The Cook 2024 table shows PVI and race ratings diverge.
- The premise under c, under leverage and under the VULN 1.35 amplifier is the same: dollars and pressure work better on vulnerable members. Its only support is allocation evidence (donors target these seats). Reciprocity stays NOT validated (D-104 to D-108).

**4. Transfer ceiling.**
- No row provides a Congress-level causal estimate of lobbying or peer effects on cosponsorship.
- The best-identified designs are state-legislature field experiments (scan leads: Zelizer; Camp et al.) or House access-only experiments (Kalla & Broockman).
- Engine outputs (pflip, spread, seed_prob, GO PUBLIC scores) are heuristic model outputs, never vote, access or introduction probabilities (D-025).

**5. Guardrails honoured.**
- No row argues for dropping regular cosponsorship edges.
- Trivedi's degree-weighted edge form is FLAGGED: it must not be read as support for the REJECTED per-edge volume normalization.
- Lobbying-spend gaps raised by Huneeus & Kim, Jacoby and Furnas are not recommendations to bring LDA dollars in as weights (D-021).

**6. Most gaps are answerable on the system's own data.**

These internal tests answer more than further literature would:
- cosponsor join-timing hazard (fits the constant 6 and tests thresholds);
- the held-out-topic test (priority and leave-one-out orientation);
- cross-issue co-movement (scenario defection);
- a tie-break rerun (competitive cascade);
- an MC convergence check;
- a PERS_NONE sweep.

Only the money-effect-by-competitiveness question and the take-up-by-tier question need external, identified designs.

## Research agenda

1. Fix citation and label errors (docs only, Paul applies): relabel LOSSFRAME, WM/WP and MAXSHIFT as owner judgment rather than CALIBRATED. Move Linde & Vis from LOSSFRAME to the prospect-domain term and drop 'loss-averse overall'. Remove the Fouirnaies & Hall attribution from the competitiveness term. Correct Byrne's 'Exp-2 interaction' wording. Correct the scenario D* Lite citation.
2. Salience advisory: decide whether to gate held/defensive credit to sq == defend, to match Noble & Turner. The live 2026-10-08 advisory (18 issues) has all four GO PUBLIC verdicts on offense issues. This is Paul's ruling.
3. Out-party influence: decide on one treatment across the kernel (0.4 floor), flip (cpf_pressure 0.5), salience rally (+0.5) and lt_priming (clamped about 0). Read the Fong (JOP) expert-cue lead before deciding on any bridge exemption.
4. Internal test: CU-bill cosponsor join hazard as a function of already-joined weighted neighbours, stratified by party pair and held out by Congress. It estimates the constant 6, tests threshold versus smooth adoption, and tests whether cross-party ties predict joining.
5. Internal test: rerun the competitive cascade with B-first and randomized same-hop tie-breaking under identical CRN draws, and report the change in CU spread. Separately, check seed-order stability at MC 20/40/200.
6. Internal test: leave-one-topic-out orientation versus held-out tier on the 533 x category stance matrix, stratified by party, with per-category denominators. Also test cross-category co-movement of stance changes for the scenario defection assumption.
7. Retiring list: remove members seeking other office from incent 0.60, per Fouirnaies & Hall's lower-bound argument. Paul's call.
8. COMPET_MULT: decide whether candidate-specific vulnerability (Cook race rating or Ballotpedia comp) should re-enter, since PVI-only c gives EVEN Solid R and EVEN Toss Up seats the same value. Count members in the ten flagged redistricted states near lean 0.
9. Read the online-scan leads (all UNVERIFIED, to be added to the hub file before use): Camp, Schwam-Baird & Zelizer, 'The limits of lobbying: null effects from four field experiments in two state legislatures' (cambridge.org PSRM); Zelizer's briefing-spillover experiment (resolve the 60% versus 80% indirect-effect figure); Rogowski & Sinclair, House office lottery; Box-Steffensmeier, Christenson & Craig, 'Cue-Taking in Congress' (AJPS 2019); Fong, cue-taking (fongsurvey.polisci.lsa.umich.edu/papers/cue-taking.pdf); Simkin & Roychowdhury, H.R. 1207 cosponsor dynamics (arxiv.org/abs/1001.3732); Levitt 1998, PAC election channel (ideas.repec.org/a/bla/ecopol/v10y1998i1p19-35.html); the 2024 AEA paper on carbon money and electoral pressure (aeaweb.org/conference/2024/program/paper/nn3G7ztR).
10. External design needed: the take-up of lobbying contact by prior stance tier (SGATE, PERS) and the money effect by seat competitiveness (c, leverage). Both need identified variation. Check whether the Camp et al. and Kalla & Broockman replication files allow a split by prior position or by competitiveness.
