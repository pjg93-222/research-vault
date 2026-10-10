---
type: system
name: "Melange influence engine (persuasion, money and cascade physics)"
slug: melange-influence-engine
components: [melange-influence-engine.cascade-kernel, melange-influence-engine.competitive-cascade-crn, melange-influence-engine.seed-gate, melange-influence-engine.flip-model, melange-influence-engine.persuadability-table, melange-influence-engine.money-response-defection, melange-influence-engine.lt-priming, melange-influence-engine.salience-shock, melange-influence-engine.receptivity-layer, melange-influence-engine.competitiveness-multiplier, melange-influence-engine.electoral-plays, melange-influence-engine.scenario-runner, melange-influence-engine.priority-engagement]
brain_notes: [topics/2026-07-feature-influence-engine-internals, topics/2026-07-feature-influence-engine-internals-receptivity-layer, topics/2026-07-feature-influence-engine-internals-salience-sna-wave, topics/2026-06-feature-moneyball-excel-model-build-scenario-tools-and-calibration]
paper_count: 79
tags: [system, type/system-hub]
---

# Melange influence engine (persuasion, money and cascade physics)

Behavioural simulation layer that sits downstream of the rank. It covers the shared cascade kernel and partisan edge factors, competitive independent cascade with common random numbers, contingent seeding, the flip/winnability model, money-to-lean response and defection, linear-threshold priming, salience shocks, the receptivity (susceptibility) layer, the competitiveness multiplier, electoral plays and the scenario runner. Core code: fec/moneyball_core.py and fec/engine/*.py.

Brain notes: [topics/2026-07-feature-influence-engine-internals](obsidian://open?vault=brain&file=topics/2026-07-feature-influence-engine-internals); [topics/2026-07-feature-influence-engine-internals-receptivity-layer](obsidian://open?vault=brain&file=topics/2026-07-feature-influence-engine-internals-receptivity-layer); [topics/2026-07-feature-influence-engine-internals-salience-sna-wave](obsidian://open?vault=brain&file=topics/2026-07-feature-influence-engine-internals-salience-sna-wave); [topics/2026-06-feature-moneyball-excel-model-build-scenario-tools-and-calibration](obsidian://open?vault=brain&file=topics/2026-06-feature-moneyball-excel-model-build-scenario-tools-and-calibration)

## Melange influence engine: evidence assessment

**Scope.** This covers 13 components and 124 paper bearings (35 VERIFIED, 76 VERIFIED-SOFTENED, 13 UNVERIFIED) across 79 distinct papers. UNVERIFIED rows are single-agent leads (DE-062) and do not count as findings here.

**Overall verdict.** The engine's architecture has sound *method* sources:
- Bharathi-Kempe-Salek competitive IC
- Chen 2021 contingency-aware seeding
- Noble & Turner 2024 going public
- Kempe-Kleinberg-Tardos threshold models

Almost none of its *constants* has an empirical source. Constants with no estimate behind them:
- the saturation constant 6, the bystander rule min(0.9, $/400k) and MC=20
- the SGATE and PERS tables
- WM/WP 0.35/0.65 (labelled CALIBRATED in code, but no paper calibrates it), HALFF, MAXSHIFT 18, DTHRESH 11.25 and LOSSFRAME 1.10
- the pt multipliers 1.15/0.92, incent 0.60 and VULN 1.35
- the c amplitude and scale, and the backfire map b(psi)

The literature mostly tells us the *direction* of an effect and the *class of model*. It almost never gives a legislator-level magnitude.

**Where the evidence is strongest:**
1. **Money moves votes and positions little.** Ansolabehere et al. 2002 is the anchor: House 1978-1994, N=3,400; member fixed effects remove the contribution effect. Bombardini & Trebbi 2020 and Poole 1999 agree. This supports the small MAXSHIFT and money as the minority term, but not any specific value.
2. **Out-party cues do not persuade.** Bakker (mass public, 2016-2018 panels), Merkley & Stecula (mass Republicans, climate), Furnas et al. 2019 (N=441 DC staff, 2017, stated intentions) and Ringe et al. 2013 (EP Environment Committee 2004-2008, 1,319 dyads) agree. All of this is mass-public, staff or EP evidence. None of it is US member-to-member.

**Contradictions found:**
- **Salience shock.** The defensive-appeal credit is applied regardless of the status quo. All four GO PUBLIC verdicts in the 2026-10-08 advisory (18 issues) are offense issues. The top-ranked one (Technology & Emerging) is close to the case where Noble & Turner say never go public.
- **LOSSFRAME 1.10.** It cites Linde & Vis 2016 for elite-attenuated loss aversion, but that paper found no loss aversion in its MPs (n=46 Dutch MPs, 2014-2015).
- **Out-party sign.** The kernel and the flip model give out-partisan pro-neighbours a positive 0.5 weight. The cue evidence is zero-or-negative, and Furnas 2019 suggests the out-party penalty is about twice the in-party gain, the opposite of the kernel's asymmetry (+0.7 vs -0.5).
- **Same-hop ties.** The cascade breaks same-hop ties deterministically in CU's favour, where Bharathi's model has no ties. The size of this effect has not been measured.
- **Retiring members.** Fouirnaies & Hall 2022 is cited for incent 0.60 on final-term members. It finds an effort drop (-0.24 SD) in term-limited *state* legislatures with no position change, and argues that members seeking other office keep their electoral incentives. The retiring list includes such members.

**Gaps:**
- No study validates the cascade against observed cosponsorship accumulation.
- No contact-to-take-up rates exist by stance tier (SGATE, PERS).
- No evidence shows dollars buy more in competitive seats. The supply-side papers show only that money *flows* to close seats, which is a selection confound for any future estimate of c (reciprocity NOT validated, D-104..D-108).
- No evidence supports the scenario runner's cross-issue defection premise.
- No literature at all supports the leave-one-out priority orientation.

**Paul's positions vs the evidence:**
- He reads source party as a filter on receptiveness (Furnas). The evidence supports the direction, but not the 3.4 ratio and not a positive out-party weight.
- His access-and-ROI view of money is more generous than the evidence. The evidence on access is itself split: Kalla & Broockman find a donor-label access premium (191 House offices, Jun-Aug 2013); Furnas 2019 finds no donor meeting premium.
- His complex-contagion hypothesis is a legitimate modelling option (Chen 2023, Kempe 2015), but it has no legislative estimate.
- His trait-as-edge-parameter view of receptivity is undercut more than his notes admit. No bearing links any trait to a legislator's response to lobbying.

## Cascade activation kernel build_P and partisan edge factors

**What it computes.** p(u->v) = x/(x+6) with x = w x cpf x edge_mult(v). cpf_diffusion = 1 for same party and 0.4 + 0.6 sqrt(bpf_u bpf_v) across D/R. cpf_pressure = 1.7 co-partisan, 0.5 out-partisan.

**Method.** One shared kernel for cascade, coalition, frontier, scenario, priming and route engines. bpf is the bipartisan share (default 0.24). The two physics factors are deliberately never merged.

**Code refs:** `fec/moneyball_core.py:1-12,68-72,543-562`

**Assumptions:**
- Cross-party contagion flows through bridge-builders
- Co-partisan sources persuade more and out-partisan sources less (Furnas/Bakker source alignment)
- Saturation constant 6 is a design choice, not estimated
- Receiver susceptibility, not sender loudness, governs activation

**Evidence (14 bearings: 3 VERIFIED, 11 VERIFIED-SOFTENED, 0 UNVERIFIED; 14 distinct papers).**

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
| [[battaglini-2018-effectiveness-connected-legislators]] | 2018 | qualifies | In the corrected column, between-party spillover is 0.8668 (SE 0.2136) and within-party 0.8346 (SE 0.0584); the gap is small relative to the SEs. The authors nonetheless conclude that cross-party ties matter more. House, 109th-113th. | The kernel models stance and activation diffusion, while the paper measures productivity spillovers, so cross-party damping may still be right for persuasion. Table 6 is not the only stratification: Table 5a and 5b heterogeneity, the Table 7 stage split and the Table 8 state-experience subsample (N = 1,775) also stratify or subset. | weak | VERIFIED-SOFTENED |
| [[harbridge-yong-2023-bipartisan-path-effective]] | 2023 | context | Cross-party cosponsorship is exchanged: members who offer cross-party support attract it back, within member 0.317 House (Model 4.2) and 0.547 Senate (Model 4.4), p<0.001. The paper tests member-level aggregates only, with no dyadic diffusion or join timing. | The paper cannot test the sqrt-product form or the 0.4 floor. Context only. | weak | VERIFIED |

**Assessment.** Counts computed from the evidence rows: 14 bearings: 3 VERIFIED, 11 VERIFIED-SOFTENED, 0 UNVERIFIED. **Evidence weight: moderate on direction, nil on constants.** 

**Direction of the source-alignment term.** Co-partisan sources weigh more than out-partisan ones. This is supported in direction by:
- Furnas et al. 2019: 2017 staff vignette, N=441 staff (5.2% response). This is *outside requester evidence*, not Member-to-Member ties.
- Merkley & Stecula: mass Republicans, climate.

**What the same papers undercut:**
- **The asymmetry.** Furnas finds the non-aligned penalty roughly twice the aligned benefit. The kernel's gain (+0.7 over 1.0) exceeds its penalty (-0.5). Furnas's pooled estimate is null over the party fault line, so any figure must be quoted by party stratum or as UNSTRATIFIED.
- **The sign.** Bakker (mass public, 2016-2018 online panels, study 1 N=747) finds out-party cues *lower* support by about .06-.07 on a 0-1 scale. In-party cues add about .03, mostly non-significant. Merkley reads out-party cues as backlash. A positive out-partisan pressure weight of 0.5, which enters flip_model nA, has no support.

**Cross-party diffusion damping.** cpf_diffusion is 0.544 at the default bpf 0.24.
- Battaglini et al. 2018 (House 109th-113th) report between-party productivity spillover 0.867 (SE 0.214) vs within-party 0.835 (SE 0.058), so they find no damping. But their outcome is productivity spillover, not persuasion, so this qualifies the term rather than refuting it.
- Ringe et al. 2013 (EP, 2004-2008, 1,319 non-unanimous opposing-bloc dyads) find connectivity between adversaries goes with *less* co-voting. This is a multi-party, staff-contact setting and is cross-sectional.

**Saturation constant 6.** It has no basis. Kempe et al. 2015 show seed rankings are sensitive to the activation level, so a sweep (3/6/12) should accompany any claim that frontier seed order is stable. Trivedi 2020 gives no calibration. **FLAG:** its degree-based edge probability must not be read as support for the REJECTED per-edge volume normalization.

**Fink 2023.** It supports heterogeneous directed edge probabilities in concept. The system symmetrizes them and reads them as per-campaign odds (cap 2.0, so p_max 0.25), and the mapping holds only for untied pairs.

**Extensions the papers suggest (not adopted):**
- Gaynor 2022: party is not homogeneous; there are faction effects in LPAC ties in the 115th House, which need party strata.
- Zareie 2023: signed edges. This would change the two-physics architecture, so the owner decides.

**Reciprocity.** Haythornthwaite's reciprocated ties must not be imported as a reciprocity weight (D-104..D-108).

**Contradictions:**
- Bakker (mass public, 2016-2018, study 1 N=747): out-party cues lower policy support by about .06-.07. The kernel and flip model give out-partisan pro-neighbours a positive 0.5 weight.
- Furnas et al. 2019 (N=441 staff, 2017; party subgroup Ns not reported): the out-party penalty is about 2x the aligned gain. cpf_pressure has the gain (+0.7) larger than the penalty (-0.5).
- Battaglini et al. 2018 (House 109th-113th): between-party spillover 0.867 is not below within-party 0.835. cpf_diffusion damps cross-party edges to 0.544 at bpf 0.24. The outcome differs (productivity, not persuasion), so this is a soft contradiction.

**Gaps:**
- No estimate exists for the saturation constant 6. Fill it with a likelihood fit of x/(x+c) to observed CU-bill cosponsorship join timing on the member network, or at minimum a sensitivity sweep (3/6/12) of frontier seed order.
- No US Member-to-Member evidence exists for the 1.7/0.5 ratio. Fill it with an elite cue study, such as a cosponsorship-recruitment hazard model by party pair, stratified D-D, R-R and D-R, with bpf as a moderator.
- The sqrt(bpf_u bpf_v) bridge form and its 0.4 floor are untested. Harbridge-Yong 2023 is member-level only and has no dyadic diffusion.

**Paul's position vs the evidence.** Paul treats directed edge probabilities as foundational (Fink) and party or source identity as a filter on receptiveness (Furnas). The evidence supports the filter's direction, but not its magnitude, its gain-over-penalty asymmetry, or a positive out-party weight. The mass-public and staff evidence points to an out-party weight at or below zero. Paul's directed-edge view is also not what the code does: it symmetrizes Fink rates. He correctly notes that the constant 6 has no literature basis.

**Open questions:**
- Should cpf_pressure's out-partisan weight stay positive (0.5) when every cue bearing points to zero or negative? This is a rank-moving change and Paul's call.
- Does frontier seed order survive a 3/6/12 sweep of the saturation constant?
- Is the 1.7 co-partisan amplification, as opposed to the out-party penalty, supported by anything? Bakker's in-party effect is small and often null.

**Read first:** [[furnas-2019-moneyed-interests-information]], [[bakker-nd-understanding-partisan-receptivity]], [[battaglini-2018-effectiveness-connected-legislators]], [[kempe-2015-maximizing-spread-influence]], [[fink-2023-congressional-social-network-dataset]], [[merkley-nd-party-cues-news]]

## Competitive independent cascade with common random numbers

**What it computes.** Expected pro-CU activations in a two-colour first-to-reach race; bystanders turn anti with probability min(0.9, industry$/400000).

**Method.** Keyed splitmix64 draws per (iteration, edge), reseeded with 1000003*SEED+it. SEED 7, MC 20 (scenario 40). Seed activation uses a separate 2^62 key range.

**Code refs:** `fec/moneyball_core.py:549-554`, `fec/engine/budget_frontier.py:40-94`, `fec/engine/per_topic_coalition.py:27-57`, `fec/engine/scenario.py:40-53`, `fec/engine/salience_shock.py:71-81`

**Assumptions:**
- Independent edge activations given the draws (Bharathi-Kempe-Salek competitive IC)
- Industry money mobilizes neutral bystanders
- MC = 20 is adequate

**Evidence (10 bearings: 2 VERIFIED, 8 VERIFIED-SOFTENED, 0 UNVERIFIED; 9 distinct papers).**

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

**Assessment.** Counts computed from the evidence rows: 10 bearings: 2 VERIFIED, 8 VERIFIED-SOFTENED, 0 UNVERIFIED. **Evidence weight: strong as a method source, nil as empirical validation.** 

**What carries over from Bharathi-Kempe-Salek:**
- Edge-level common random numbers.
- First-colour-wins.
- One draw per edge serving whichever colour arrives. This matches the paper's fixed-in-advance edge equivalence (Lemma 1).

**What departs from it:**
- **Timing.** Hop-synchronous unit steps replace the paper's continuous exponential delays.
- **Ties.** The paper has no ties between simultaneous arrivals. The code's frontier is built A-first, so a node reached by both colours in the same hop goes to CU. This is confirmed by code reading. It biases CU reach upward by an unmeasured amount, and the size stays UNVERIFIED until a B-first or randomized-order rerun under the same draws measures it.

**Complexity.** Borodin et al. show that the shared kernel keeps the engine outside the separate-weights hardness result. Giving the anti side its own physics (for example cpf_pressure on B only) would move it inside.

**Constants with no source:**
- The bystander rule min(0.9, $/400,000) has no basis in any paper.
- MC=20 adequacy is the system's own claim (mc_stability_check). Arya 2025 used 100,000 draws with no rationale, which says nothing about 20.

**Model-class alternatives, not refutations:**
- Cai 2025: a non-progressive voter model with a strategic opponent.
- Moreno 2021: hub targeting in the discrete regime.
- Kumar 2022 and Kamarthi 2020: surrogates trained on single-colour labels. These would need retraining on competitive labels before standing in for this engine.

**Harrison 2010** is a generic caution: MC is only as good as its inputs, and analytic alternatives should be considered first.

**Contradictions:**
- Bharathi et al. avoid simultaneous-arrival ties through continuous delays. The code resolves same-hop ties deterministically in CU's favour. The departure is confirmed in code; its effect on CU reach is unmeasured (UNVERIFIED, DE-062).

**Gaps:**
- No study shows the competitive IC reproduces observed cosponsorship accumulation curves on CU bills. Fill it with a back-test of simulated vs observed cosponsor counts over time on past CU bills, with a pre-registered fit metric.
- The bystander mobilization rule min(0.9, industry$/400k) has no evidence. Fill it with an observational test of whether industry PAC money to a member predicts anti-side stance among previously neutral members, stratified by party.
- MC=20 adequacy is unsupported. Fill it with a variance study of rank and frontier decisions across MC 20/40/200 under CRN, reported as rank-flip rates.
- Order bias is unmeasured. Fill it with a rerun using B-first and randomized frontier order under identical draws.

**Paul's position vs the evidence.** Paul wants rival industries modelled; the two-colour race does that, and Bharathi supports the structure. He also wanted a cheap analytic proxy (Viral Centrality). The system chose MC with CRN, and no bearing compares the two head-to-head. Harrison's advice to consider analytic alternatives first is in Paul's favour, but it is not evidence that the proxy is accurate.

**Open questions:**
- How much CU reach does the A-first same-hop tie rule add?
- Does industry react to CU seeding (filings are public)? If so, the fixed-opponent framing fails (Bharathi).
- Would Viral Centrality or Katz-Bonacich reproduce the MC frontier order within the CRN noise band?

**Read first:** [[bharathi-nd-competitive-influence-maximization]], [[borodin-nd-threshold-models-competitive]], [[harrison-2010-introduction-monte-carlo]], [[moreno-2021-shadowing-shielding-effective]], [[cai-2025-competitive-influence-maximization]]

## Contingent seed activation gate

**What it computes.** gate(n) = max(0.15, seed_prob(n) x SGATE[tier]); committed allies are never gated.

**Method.** SGATE ranges from Highly Persuadable 1.00 down to Opponent 0.35, with unknown 0.85. seed_prob comes from the receptivity layer.

**Code refs:** `fec/moneyball_core.py:626-630`

**Assumptions:**
- A lobbied legislator acts only with a stance-graded take-up probability (Chen et al. UAI 2021 contingency-aware seeding)

**Evidence (5 bearings: 1 VERIFIED, 3 VERIFIED-SOFTENED, 1 UNVERIFIED; 5 distinct papers).**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | supports | Stochastic submodular maximization with independent item states (failure types) and a monotone submodular objective: greedy obtains (1-1/e) of the best adaptive policy (Thm 16, generalizing Asadpour et al. 2008). Robustness to a misspecified prior is shown for a multiplicative error condition with alpha = d/c (Sec 4.3). | The structure matches: independent per-seed Bernoulli take-up over a spread function. The guarantee also needs spread given activated seeds to be monotone submodular, which is not shown for this competitive kernel, and needs known take-up probabilities. SGATE and seed_prob are unestimated, so the known-prior assumption fails. | moderate | VERIFIED |
| [[chen-2021-contingency-aware-influence]] | 2021 | method-source | A selected seed is willing with a probability q that is known a priori. Footnote 2 says q is the same for all nodes because per-node values are impractical to know. Default q = 0.6; Q3 varies q over 0.2, 0.4, 0.6, 0.8. The paper estimates no real-world q. | The modeling idea transfers: invitation is not activation. The paper's q is homogeneous and its analysis is multi-round. SGATE is member-specific and used single-shot, so Theorems 1-3 do not cover it. The 0.60 baseline is the paper's simulation default, not an estimate. The source population is HIV peer leaders, not legislators. SGATE values remain design priors until measured. | moderate | VERIFIED-SOFTENED |
| [[zareie-2023-influence-maximization-social]] | 2023 | qualifies | Behaviour-aware IM adds user features (BV), relationship features (BE) and message features (AM) to the structural IM problem (Eq. 1). The survey reports no take-up rate, no effect size and no benchmark, and it classifies methods without comparing their accuracy. It warns that numeric encodings of attitude may not capture personality and that many users are silent. | Supports only the framing that member attributes may enter seed activation. The survey does not discuss contingency-aware seeding, does not cover legislators, and does not justify the 0.60 baseline, the coefficients or the 0.30-0.90 clip, which remain design priors. Evidence route is H1: outcome_contract.py requires at least 150 members with cosponsorship deltas. The F5 count of 72 was not checked. | moderate | VERIFIED-SOFTENED |
| [[madsen-2017-strategic-advantages-micro]] | 2017 | supports | Under Bayesian source-credibility updating, contacts restricted to receptive targets (credibility signal above 0.5) convert better than untargeted contacts. Contacting audiences who view the source unfavourably moves them away from the source's position. | Directional agreement only. The paper's agents are voters, not legislators. Its unit is a source-credibility belief update, not a take-up decision about cosponsoring, and it estimates no take-up rates. The SGATE values remain unestimated design constants; this paper does not calibrate them. | weak | UNVERIFIED |
| [[watkins-1992-q-learning]] | 1992 | context | Q-learning needs each state-action pair sampled an infinite number of times (p.281). Lemma B.3 (p.284; proof p.290) says action-replay estimates of rewards and transitions converge with probability 1 as the level grows, under learning-rate sum conditions. This concerns estimating model quantities from unbounded samples, not learning a gate parameter. No finite-sample rate is given. | Learning take-up rates would need repeated observed contact-to-action outcomes per stance tier, which the system does not log. The paper's result is asymptotic and gives no sample size. The link is conceptual: Lemma B.3 concerns action-replay transition and reward estimates, not gate probabilities. Weak strength is right. | weak | VERIFIED-SOFTENED |

**Assessment.** Counts computed from the evidence rows: 5 bearings: 1 VERIFIED, 3 VERIFIED-SOFTENED, 1 UNVERIFIED. **Evidence weight: method source only, plus one lead.** 

**Chen et al. 2021** supplies the idea that invitation is not activation. But in that paper:
- the take-up probability q is homogeneous across nodes and known a priori;
- the default q is 0.6, a simulation default (the paper estimates no real-world q);
- the setting is multi-round, with HIV peer leaders.

The system's seed_prob baseline of 0.60 matches that simulation default. It is not a legislator estimate. SGATE is member-specific and single-shot, so Chen's theorems do not cover it.

**Golovin & Krause 2011** give (1-1/e) for independent take-up. That guarantee needs a known prior and adaptive submodular spread; SGATE is unestimated and the competitive kernel's submodularity is not shown.

**Zareie 2023** supports only the framing that member attributes can enter activation. It gives no rates.

**Madsen 2017 (UNVERIFIED lead):** contacting unreceptive audiences can move them *away*. The gate has no backfire outcome.

**Watkins 1992:** learning the gate would need repeated logged contact-to-action outcomes per tier, which the system does not log.

**Bottom line:** SGATE (1.00 down to 0.35, unknown 0.85) and the seed_prob coefficients are design priors.

**Contradictions:**
- none

**Gaps:**
- No take-up rates exist by stance tier. Fill it with logged lobbying-contact events (from CU-side meeting logs, not LDA, which is identity-only per D-021) joined to subsequent cosponsorship deltas, tested on the prospective H1 contract (at least 150 members with cosponsorship deltas).
- There is no test of whether contact with Opponents backfires. Fill it with the same panel, testing for negative stance movement after contact in low tiers.

**Paul's position vs the evidence.** Paul maps contingency-aware seeding directly onto legislators who may or may not move when approached. The concept transfers. Two caveats: Chen's q is homogeneous, known, and a simulation default; and the system's 0.60 baseline is that default, not a measurement. Paul's mapping is a reasonable design prior that has not been calibrated.

**Open questions:**
- Can CU-side contact logs be obtained at member level to estimate SGATE?
- Should low tiers carry a negative-outcome branch (Madsen lead), or is a floor of 0.15 enough?

**Read first:** [[chen-2021-contingency-aware-influence]], [[golovin-2011-adaptive-submodularity-theory]], [[zareie-2023-influence-maximization-social]], [[madsen-2017-strategic-advantages-micro]]

## Flip - winnability model

**What it computes.** pflip = a/(a+b), where a = WM pm (c cu+1)/((c cu+1)+HALFF R) + WP nA/mAt and b is the same with industry money and anti neighbours. flip_value = sum tgt x pflip, with tgt = engagement x PERS x (1+0.3 bpf) x delivery_mult.

**Method.** Saturating money term on receipts R (floored at $200k; zero imputed to median). nA = pro-neighbour weight x cpf_pressure. WM 0.35, WP 0.65, HALFF 0.10. A comment cites a Volden-Wiseman passage bonus but the code line has none.

**Code refs:** `fec/engine/flip_model.py:1-147`, `fec/engine/money_observation.py:4-13`

**Assumptions:**
- Money buys access more than votes (Ansolabehere; Hall & Wayman; Kalla & Broockman)
- Peer proximity outweighs money
- Money saturates relative to receipts
- Flip-side competitive LT has no submodularity guarantee (Borodin et al.)

**Evidence (20 bearings: 5 VERIFIED, 14 VERIFIED-SOFTENED, 1 UNVERIFIED; 20 distinct papers).**

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
| [[ansolabehere-2002-little-money-politics]] | 2002 | qualifies | Money is a small part of a reelection-minded legislator's calculation: party moves the score by more than 30 points against about 2 for money. The paper lists buying access as one of several group motives and does not test it. It cites 3 billion dollars of lobbying against 300 million of PAC contributions in 1997-98, and Langbein's estimate of about 10000 dollars per legislator hour. Its main thesis is consumption rather than access. | The paper supports money as the minority partner on votes, not on access. The access attribution rests on Kalla and Broockman, which is outside this paper. Peer proximity (cpf_pressure) is not party; the 30-point party effect is only the nearest analogue. With peer terms zero, pflip approaches 0 or 1 only at extreme money ratios because of the +1 smoothing. | moderate | VERIFIED-SOFTENED |
| [[callaway-2025-difference-differences-continuous]] | 2025 | suggests-extension | A dose-response curve ACRT(d) is identified only under strong parallel trends; cross-dose comparisons otherwise carry selection bias ATT(l\|h) minus ATT(l\|l) (Thm 3.2). In the application the nonparametric ACRT(m) point estimates are negative for m above 0.41, covering 71 percent of treated hospitals, but the 95 percent uniform band covers zero everywhere, so only pointwise sign changes are reported. ATT_loc stays positive. A quadratic functional form is precise only if it is correctly specified. | A test would need a member panel: votes or stance before and after giving, an unfunded zero-dose comparison group, and dose as share of receipts. PAC dose is plausibly selected on expected response, so strong parallel trends is doubtful and any curve beyond ATT_loc would carry selection bias. This is a design template for a future test, not evidence on the current HALF or MAXSHIFT values. Any result stays UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED |
| [[defigueiredo-2013-advancing-empirical-research]] | 2013 | suggests-extension | Lobbying spending was about 3.5 billion dollars in 2012, roughly five times the approximately 750 million a year in interest-group campaign contributions (pp.5-6). Kang (2012) finds expenditure effects on enactment are very small but the marginal policy return is about 140 percent (p.28), and the value of lobbying depends on timing and targets (p.28). Selection into lobbying (about 10 percent of firms) biases naive return estimates upward only if non-lobbying interests expect no return (p.17). | Lobbying cannot enter a per-member leg directly, because LD-2 names no member and LDA is an identity source, not an edge or weight source (D-021). Pooled return figures must carry an UNSTRATIFIED label. Kang's 140 percent is a policy return in a structural model, not a dollar ROI. | moderate | VERIFIED-SOFTENED |
| [[fremeth-2018-spillovers-regulating-corporate]] | 2018 | qualifies | Firm-cycles whose PAC reaches the limit are associated with 549,000 to 1.6M dollars more lobbying per cycle (abstract; Table 3 col 1; Table 5) and with about 110 dollars more CEO giving per candidate at the limit (Table 2). CEO giving is candidate money outside the PAC channel. Authors describe these as partial correlations. | Hypothesis only: if competitor bank and finserv firms hit the cap as S&P 500 PAC firms do, ind0 undercounts competitor pressure on the members they max out. The paper does not test this. Does not license LDA dollars as a per-member weight (D-021). | moderate | VERIFIED-SOFTENED |
| [[harbridge-yong-2023-bipartisan-path-effective]] | 2023 | qualifies | Within-member quadratic in the attracted share: House linear 1.828 and squared -1.844 peak at 0.496 (Model 3.1); Senate 1.666 and -1.626 peak at 0.512 (Model 3.4). Effectiveness falls on both sides of the peak. A one-SD decline from the mean costs about 22 percent of LES in the House and 16 percent in the Senate; a one-SD rise adds about 8 and 4 percent. Mean LES about 1.0. | A linear bonus approximates the paper only below a share of about 0.5, where most members sit (paper mean 0.29 House, 0.35 Senate). The paper outcome is LES on a member's own sponsored bills, not the value of flipping a vote, and its share is the attracted share, not bpf. Any change to the functional form would move ranks and is not proposed here. | moderate | VERIFIED-SOFTENED |

**Assessment.** Counts computed from the evidence rows: 20 bearings: 5 VERIFIED, 14 VERIFIED-SOFTENED, 1 UNVERIFIED. **Evidence weight: moderate for "money is the minority term on votes", contested on access, nil on the constants.** 

**Money is small on votes.** Ansolabehere et al. 2002 is the anchor (House 1978-1994, N=3,400). About $60k of corporate PAC money moves the CCUS score by at most 2 points, against more than 30 for party; that contrast is conditional on party and must be labelled UNSTRATIFIED. Member fixed effects eliminate the effect. Bombardini & Trebbi 2020, citing Kang, report a tiny enactment effect for energy policy in the 110th Congress. Jacoby 2025 is a simulation from literature parameters: one House member out of 435 moves on one 2009 bill. Holyoke 2015 (UNVERIFIED) agrees.

**Access is contested:**
- Kalla & Broockman find a donor-label access premium: 191 House offices of one party, Jun-Aug 2013, chief-of-staff-or-above meetings 12.5% vs 2.4%. The study measures no votes.
- Furnas 2019 finds *no* donor meeting premium in 2017 stated staff intentions.

The code comment cites "money buys access not votes", yet the model puts money directly into a flip probability at weight 0.35. That is a modelling step beyond what the access literature shows. pflip must be read as a heuristic share, not as a vote or access probability (D-025).

**Constants.** No paper calibrates WM/WP, HALFF or the saturation. The code labels WM/WP as CALIBRATED, and Bonica 2013 explicitly gives that label no support. Callaway et al. 2025 supply a dose-response design template; PAC dose is plausibly selected on expected response.

**Passage bonus.** The target term (1+0.3 bpf) carries an attributed "Volden-Wiseman +8-14%" that is not in the paper. Harbridge-Yong 2023 show an inverted-U in attracted bipartisan share, peaking at about 0.50 (House) and 0.51 (Senate). The monotone linear bonus approximates that only below 0.5.

The component's own method note says the code line has no bonus, but the bearing and the computes field both show (1+0.3 bpf). This needs checking.

**Timing.** Kim et al. 2020 and 2023 find the money-to-lobbying link is onset-driven and transient; the static cycle-level term ignores this. Cai 2025 shows equilibrium shares depend on timing.

**Coverage.**
- Furnas (nd) shows that 64-70% of politically active organizations per year (1998-2018) lobby without a PAC.
- Fremeth 2018 suggests maxed-out PACs shift money to lobbying.
- So the b-leg may undercount competitors. Under D-021, LDA dollars still cannot become a per-member weight.

**Lead.** Battaglini 2016 suggests network spillover of money pricing. This would need its own pre-registered look.

**Contradictions:**
- The code comment says 'money buys access' and cites Kalla & Broockman. Furnas et al. 2019 (2017 staff vignette, N=441) find no donor meeting premium, and Democratic staff are less likely to meet donors.
- The target bonus (1+0.3 bpf) is attributed to Volden-Wiseman at +8-14%. That figure is absent from the paper. Harbridge-Yong 2023 show an inverted-U (peak attracted share about 0.50), not a monotone rise.
- WM/WP are labelled CALIBRATED in code. No bearing, including the cited ones, estimates them.

**Gaps:**
- The relative effect of CU vs industry money as a share of receipts on CU roll-call or cosponsorship alignment is unestimated. Fill it with a member panel using continuous-dose difference-in-differences (Callaway), with an unfunded comparison group, member fixed effects, and party strata.
- The peer-vs-money weight ratio is unestimated. Fill it with a joint hazard model of cosponsorship timing on neighbour adoption and money dose.
- Concavity in the contribution-to-receipts ratio is untested. The same panel can check it with a nonparametric dose curve.

**Paul's position vs the evidence.** Paul believes money buys access and returns. On *votes*, the evidence sides with the model's conservative stance, not with his ROI enthusiasm. On *access*, the evidence is split (Kalla & Broockman yes; Furnas 2019 no).

His Jacoby highlight, that lobbying sways undecided members more than donations do, comes from a simulation with literature parameters, one bill (ACESA 2009), and 7-8 members moving. It is not an estimate. Under D-021 it cannot license an LDA weight in the model.

His Bonica margin note asking for a short-term theory of engagement is answered in part by Kim et al. (onset-driven, transient links). The current static money term ignores that.

**Open questions:**
- Does the code at flip_model.py:142 carry the (1+0.3 bpf) bonus? The component note and the bearing disagree.
- Should the CALIBRATED label on WM/WP be changed to owner judgment?
- Should money enter pflip at all if the evidence shows access but not votes, or should it enter only through an access or engagement term?

**Read first:** [[ansolabehere-2002-little-money-politics]], [[kalla-nd-campaign-contributions-facilitate]], [[furnas-2019-moneyed-interests-information]], [[harbridge-yong-2023-bipartisan-path-effective]], [[callaway-2025-difference-differences-continuous]], [[kim-2020-strategic-sequential-links]]

## Persuadability table PERS

**What it computes.** Topic target multiplier by stance tier: Champion 0.10 ... Highly Persuadable 0.65 ... Opponent 0.08; no position PERS_NONE 0.45.

**Method.** Fixed design table. PERS_NONE was ratified 2026-07-02 at Ally parity. The no-position default drives about 60% of cells (open M5).

**Code refs:** `fec/moneyball_core.py:564-568`

**Assumptions:**
- Persuadability is inverted-U along the stance ladder
- Silence equals uncertainty, not enthusiasm

**Evidence (6 bearings: 2 VERIFIED, 4 VERIFIED-SOFTENED, 0 UNVERIFIED; 6 distinct papers).**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[broockman-2026-should-moving-middle]] | 2026 | qualifies | Moving on an issue is rewarded only where the audience disagrees with the current position, and it is penalized where the audience holds that position (Republican candidate, own=R -0.0364 vs own=D +0.0404, Table A8). | This is an analogy only. The paper studies voters choosing between candidates, not legislators being lobbied. It gives no evidence on legislator persuadability or on the shape of the PERS curve, and it should not be read as calibrating PERS values. Its one structural point, that the target of an appeal is the issue-level disagreement and not the aggregate midpoint, is consistent with PERS being applied per topic. | weak | VERIFIED |
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | qualifies | The third coalition's median ideology shifts across sessions: very liberal (98th-102nd), center-left (103rd, 111th), center-right (105th-110th). Its members are effective. The paper does not measure persuadability, vote change or electoral vulnerability. | The paper gives no support for the PERS values or for treating third-coalition or Fitzpatrick-type members as more movable. Coalition-level ideological fluidity describes session-to-session composition, not the responsiveness of any individual member. | weak | VERIFIED |
| [[jacoby-2025-mathematical-modeling-analysis]] | 2025 | qualifies | Only undecided centrists (C) move. Committed Y and N legislators are absorbing states, and the equilibrium with all members undecided is unstable. Widening the centrist band on a -1 to 1 ideology scale from ±0.25 to ±0.35 reverses the House result so that no votes exceed yes votes. | The paper supports the extremes being near-fixed only because its model assumes it; Y and N are absorbing by construction. It does not test an inverted-U. Its centrist group is defined by an unnamed ideology scale, while PERS is defined by CU stance tier, so the band result carries over only as a warning: how the movable set is defined can decide the outcome. The system's PERS_NONE sweep shows the same thing in the system itself. Evidence is a single bill (ACESA, 2009) at the federal level. | weak | VERIFIED-SOFTENED |
| [[cai-2025-competitive-influence-maximization]] | 2025 | supports | In the individual-optimization runs with 20 percent of agents as partial zealots (Sec 4.3.3, N=100 per the Sec 4.3.2 reduction; N not restated in 4.3.3), normal agents receive more budget than zealots at every zealotry level q tested. At q=0.1 the maximum zealot allocation is under 20 and the normal-agent maximum is near 25 (allocation units as plotted in Fig 7). At medium and large q zealots get fewer resources and start later as degree rises. | The direction matches for resistant tiers (Opponent and Highly Skeptical). Champion is a committed pro tier, so its low multiplier reflects being already won, not zealot resistance, and it does not support the paper's result. The paper never sets zealot allocation to zero, so it does not support the hard zero gate on its own. Simulated agents only. | weak | VERIFIED-SOFTENED |
| [[tappin-2023-quantifying-potential-persuasive]] | 2023 | context | In the Study 2 calibration (n=3,990 respondents, 36 issues, 48 progressive video ads), the messages predicted most persuasive were often on obscure, low-information policies such as net neutrality. The authors offer this as one possible explanation for the Study 2 divergence, alongside a mixed pretest design and a nearly one-year gap before follow-up. They expect smaller effects in high-salience settings with countermessaging. | Weak. Tappin measures attitude change among public respondents after one video, while PERS models legislators under lobbying. The low-information point might carry over to members on low-visibility CU topics, but nothing here measures that. | weak | VERIFIED-SOFTENED |
| [[sverdrup-nd-introduction-grf-causal-forests]] |  | qualifies | Best linear projection tests whether effects vary along a chosen covariate: financial.autonomy.index coefficient -0.0329 (SE 0.0128, p = 0.0099). Other terms: is.female p = 0.17, family cash transfer p = 0.066, is.unemployed p = 0.61. Heterogeneity can be hard to detect at moderate n (payday AUTOC t = 0.67, about 2,500 individuals; text says possibly underpowered). | A BLP of contact effect on stance-tier ordinals needs logged contact and tier movement; the payday example suggests a sample of 533 members may be too small for tier-level heterogeneity. A null would not refute the table. | weak | VERIFIED-SOFTENED |

**Assessment.** Counts computed from the evidence rows: 6 bearings: 2 VERIFIED, 4 VERIFIED-SOFTENED, 0 UNVERIFIED. **Evidence weight: weak. No bearing tests the inverted-U in legislators.** 

**Jacoby 2025.** Committed members are fixed only *by assumption* (absorbing states). Widening the centrist band from ±0.25 to ±0.35 reverses the House outcome. How the movable set is defined can decide the result, and the system's own PERS_NONE sweep shows the same thing. PERS_NONE 0.45 drives about 60% of cells (59.8% per the flip_model comment).

**Cai 2025** (simulated agents): resistant agents get less budget. This matches the low Opponent multiplier in direction. It does not support the hard zero F1 gate, and Champion's low value means "already won", not resistance.

**Broockman & Kalla 2026** (voters, not legislators): moving pays only where the audience disagrees. This is consistent with applying PERS per topic. It is an analogy only.

**Aref & Neal 2021** describe coalition-level ideological drift across sessions, not individual movability. They give no support for Fitzpatrick-type members being more movable.

**Tappin 2023:** persuasion was largest on obscure issues, which may carry over to low-visibility CU topics. Nothing has measured that.

**Sverdrup:** tier-level heterogeneity may be undetectable at n=533.

**Contradictions:**
- none

**Gaps:**
- The inverted-U is untested. Fill it with observed stance-tier transitions after CU contact or campaign exposure, by starting tier (an ordinal transition model). Sverdrup's power caution applies at 533 members, so a null would not refute the table.
- PERS_NONE (silence = uncertainty) is unvalidated, yet it drives about 60% of cells. Fill it with a test of whether silent members move at rates between Ally and Persuadable.

**Paul's position vs the evidence.** Paul reads ideologically fluid, competitive-seat members and centrists (his example is Fitzpatrick) as the most movable. This is consistent with the table's shape, but no bearing tests it. Aref & Neal, which he cites, give it no individual-level support. The Cook table shows Fitzpatrick at EVEN PVI but rated Likely R, which reflects personal strength rather than fluidity. Jacoby warns that the movable-set definition can decide outcomes.

**Open questions:**
- How sensitive are ranks to PERS_NONE across 0.30-0.60, given that it drives about 60% of cells?
- Should the F1 zero-allocation gate for resistant tiers be softened? Cai never sets zealot allocation to zero.

**Read first:** [[jacoby-2025-mathematical-modeling-analysis]], [[broockman-2026-should-moving-middle]], [[aref-2021-identifying-hidden-coalitions]], [[sverdrup-nd-introduction-grf-causal-forests]], [[tappin-2023-quantifying-potential-persuasive]]

## Money response (lean shift) and money-triggered defection

**What it computes.** shift = MAXSHIFT x (r/(r+HALF)) x pressure_mult, with r = c x amount/receipts; competitor money is multiplied by LOSSFRAME 1.10. Defection moves floor(|shift|/DTHRESH) rungs; Champion and Opponent are immune.

**Method.** MAXSHIFT 18 (cut from 40), HALF 0.10, DTHRESH 11.25. Crossing one rung takes roughly a sixth of receipts.

**Code refs:** `fec/engine/money_response.py:15-90`

**Assumptions:**
- Money moves positions little (null vote-buying literature)
- Threats weigh more than gifts (loss aversion, elite-attenuated)

**Evidence (6 bearings: 3 VERIFIED, 3 VERIFIED-SOFTENED, 0 UNVERIFIED; 6 distinct papers).**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[linde-2016-politicians-take-risks]] | 2016 | contradicts | Loss aversion was NOT found in the MP sample (n=46) nor in the student sample; the authors suspect the loss-aversion tasks lack validity (participants may treat the endowment as house money, fn 4). The abstract's claim that MPs' deviations are 'somewhat smaller than those of other people' is not a formal statistical test in the main text: it rests on the authors' student comparison sample (n=176, Appendix E, not in the file; e.g. students showed the common ratio effect, MPs did not, p.106) plus prior literature. | The paper is no basis for LOSSFRAME. (1) It reports no loss aversion in politicians, so the citation runs against the dial. (2) Its loss frame concerns outcomes for the decision-maker relative to a reference point; a competitor PAC donation to a member is not a loss to that member, so the 'threat frame' is a separate untested assumption. (3) 1.10 is not derived from any number in the paper. LOSSFRAME stands as owner judgment and should be cited as such. | strong | VERIFIED-SOFTENED |
| [[poole-1999-nominate-after-years]] | 1999 | supports | Across Congresses 1-105, a constant ideal point per legislator classifies almost as well as a linear one (about 0.3 percent difference); linear change captured most of the relatively small changes in positions. | Indirect: the paper measures general-ideology stability with no money treatment, so it supports only a low prior on position change, not any MAXSHIFT or DTHRESH value. | weak | VERIFIED |
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | supports | Structural work finds a very small marginal enactment effect of lobbying (Kang 0.021%-0.415%) despite large stakes, and campaign contributions are low with federal donation caps very seldom binding (Ansolabehere et al. 2003, as reported). | The review addresses enactment and bill outcomes, not shifts in individual member stance. It supports rare money-triggered defection but not the LOSSFRAME 1.10 asymmetry or the specific constants. | weak | VERIFIED |
| [[jacoby-2025-mathematical-modeling-analysis]] | 2025 | supports | Campaign donations alone move one House member out of 435 (from 9 undecided to 8, 230 to 231 yes). The Senate is reported only for the combined lobbying-plus-donations run, which changes it by one senator (52/47/1 to 52/48/0). The authors note that donations may reflect ideology rather than buy influence (citing Bonica [5]). | This is a consistency check, not a calibration. The donation effect in the paper is a model output from literature parameters (I_F, I_A taken from Meng and Rode), not an observed shift. It is weaker than the experimental and null-result literature already cited at money_response.py:15. | weak | VERIFIED-SOFTENED |
| [[kalla-nd-campaign-contributions-facilitate]] |  | qualifies | The measured outcome was access: the rank of the official who attended. Positions and votes were not measured. Cosponsorship was nine legislators in total, with equal shares in both arms, and the authors say it tells little about effects. | The paper can be cited for the access measure, but not for a null on positions: its cosponsorship estimate is uninformative rather than null. It says nothing about MAXSHIFT, HALF, DTHRESH or LOSSFRAME. The not-votes half of the premise lies outside the paper (p.555). | weak | VERIFIED |
| [[ansolabehere-2002-little-money-politics]] | 2002 | supports | In the most common House specification (Column 1, 1978-1994, N = 3400, CCUS 0-100 score) an extra 60000 dollars of corporate PAC money (about 1 SD) changes the score by at most 2 points; 50000 dollars of labor PAC money changes it by 6; a change of party changes it by more than 30 points (40 with district FE). Member fixed effects eliminate the contribution effects, and IV reverses the sign on corporate money. Changes in donations to an individual legislator do not translate into changes in that legislator's roll-call votes. | The paper estimates business-wide PAC money against a Chamber score at ordinary contribution sizes, pooled across the House. The defection channel fires only on scenario shocks near 1/6 of receipts, outside the paper's support. The member-FE null puts the within-member slope at zero, so any nonzero MAXSHIFT is owner judgment, not an estimate. The 2-vs-30 contrast is conditional on party and is not stratified; quote it with an UNSTRATIFIED label. | strong | VERIFIED-SOFTENED |

**Assessment.** Counts computed from the evidence rows: 6 bearings: 3 VERIFIED, 3 VERIFIED-SOFTENED, 0 UNVERIFIED. **Evidence weight: strong support for a small shift, and a strong contradiction on LOSSFRAME.** 

**Small shift.** Ansolabehere et al. 2002 (House 1978-1994, N=3,400; UNSTRATIFIED pooled contrast): member fixed effects eliminate contribution effects and IV reverses the sign. The within-member slope is about zero, so any nonzero MAXSHIFT is owner judgment, not an estimate. Defection fires at roughly 1/6 of receipts (about $290k against median receipts of $1,743,281, n=517), which is outside the paper's data support. Poole & Rosenthal 1999 (Congresses 1-105) show near-constant ideal points. Bombardini & Trebbi 2020 and Jacoby 2025 are consistent with rare defection. Jacoby's is a model output, not an observation.

**Kalla & Broockman.** Their cosponsorship estimate covers only nine legislators in total. It is *uninformative*, not a null on positions.

**LOSSFRAME.** The 1.10 dial cites Linde & Vis for elite-attenuated loss aversion. Linde & Vis found **no** loss aversion in their MPs (n=46 Dutch MPs, 2014-2015) or students, and suspect the task lacked validity. A competitor's donation is also not a loss to the member, so the "threat frame" is a separate untested assumption. LOSSFRAME should be cited as owner judgment.

**Contradictions:**
- LOSSFRAME 1.10 cites Linde & Vis 2016 for elite-attenuated loss aversion. Linde & Vis found no loss aversion among MPs (n=46, 2014-2015), and their 'smaller than other people' claim is not a formal test.
- Treating competitor money as a loss to the member is unsupported. The paper's loss frame concerns the decision-maker's own outcomes against a reference point.

**Gaps:**
- There is no evidence on whether large competitor spends precede stance shifts in press or votes. Fill it with an event study of large industry contributions or IEs against subsequent CU stance tier changes, with member fixed effects and party strata.
- The loss/gain asymmetry is unmeasured. Fill it with the same panel, comparing CU-side and competitor-side dose coefficients.

**Paul's position vs the evidence.** Paul's highlights support the reflection effect and loss framing in MPs, weaker than in the public. The evidence separates these. Reflection (risk-seeking under losses) is found in the paid lottery task and in 2 of 4 scenarios. Loss aversion is not found, and the 'weaker than the public' comparison is informal. The LOSSFRAME dial rests on the half that failed. Paul's notes do not discuss defection, and the null vote-buying literature supports keeping it rare.

**Open questions:**
- Should LOSSFRAME be relabelled as owner judgment, or reset to 1.0? Either is Paul's call.
- Should defection be disabled outside the range of observed contribution shares (well below 1/6 of receipts)?

**Read first:** [[linde-2016-politicians-take-risks]], [[ansolabehere-2002-little-money-politics]], [[poole-1999-nominate-after-years]], [[bombardini-2020-empirical-models-lobbying]], [[kalla-nd-campaign-contributions-facilitate]]

## Linear-threshold priming (cheapest tip)

**What it computes.** For each target, the cheapest mix of recruiting pro neighbours and money that closes theta - pressure.

**Method.** theta = tier threshold (0.6 ... 3.0) + max(0, -lean/120) + 0.25 per Opponent cell. Cross-party cue factor sqrt(bpf_u bpf_v) (Bakker-Lelkes-Malka). Recruit cost = CEDGE x 0.5 / PERS[tier]. Money closes the gap only up to 0.97 pm units.

**Code refs:** `fec/engine/lt_priming.py:11-145`

**Assumptions:**
- Linear-threshold adoption over summed neighbour pressure
- Out-party cues do not persuade

**Evidence (9 bearings: 4 VERIFIED, 5 VERIFIED-SOFTENED, 0 UNVERIFIED; 9 distinct papers).**

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

**Assessment.** Counts computed from the evidence rows: 9 bearings: 4 VERIFIED, 5 VERIFIED-SOFTENED, 0 UNVERIFIED. **Evidence weight: moderate on direction (out-party cues non-positive), sound on mathematics, nil on thresholds.** 

**Out-party cue clamp.** Furnas 2019, Ringe 2013 and Merkley (all VERIFIED) agree that out-party cues do not persuade; Merkley reads them as backlash. Clamping at zero rather than inventing a backfire size is the conservative reading. None of these is US member-to-member evidence.

**Bridge exemption.** Bakker does not support it. sqrt(bpf_u bpf_v) restores full cross-party influence between perfect bridge pairs, and bipartisanship is never measured in that paper.

**Theory.** Kempe 2015 and Ma 2022 show that deterministic thresholds (the 0.6-3.0 ladder) sit outside the greedy and submodularity guarantees. That is fine as long as per-target cheapest-tip stays a labelled heuristic. A joint multi-target optimizer would inherit the hardness results.

**Competition.** Borodin: anti neighbours exert no competing threshold pressure in the solver.

**Complex contagion.** Chen 2023 supports threshold-like adoption as a modelling option only. It has no legislative K.

**Dietrich 2019** (women's issues, 2009-2014 floor votes) cannot set constants.

**Contradictions:**
- Bakker measures no bipartisanship attribute, so the sqrt(bpf) bridge exemption has no basis in the source it cites. This conflicts with the source, not with the data.

**Gaps:**
- Threshold adoption is untested. Fill it with a test of whether members join CU bills after a threshold share of close cosponsorship partners has joined: a hazard model with a fractional-exposure term vs a linear count, by party.
- Out-party endorsement effect is unmeasured. Fill it with the same model, splitting exposure into co-party and out-party joiners, stratified by bpf.

**Paul's position vs the evidence.** Paul hypothesizes that coalition formation is a complex contagion. Chen 2023 and Kempe 2015 make that a legitimate model class, and the deterministic LT ladder already departs from submodularity. His margin note that pressure from high-ranking members is 'not diminishing' fits non-submodular threshold models, but nothing tests it. The kernel has no seniority or leadership term, so the note's leadership effect is not modelled anywhere.

**Open questions:**
- Should bridge pairs keep full cross-party cue weight without elite evidence?
- Should the out-party clamp stay at zero, or go negative? Evidence leans negative, but has no legislator magnitude.

**Read first:** [[bakker-nd-understanding-partisan-receptivity]], [[merkley-nd-party-cues-news]], [[ringe-2013-pinpointing-powerful-voting]], [[kempe-2015-maximizing-spread-influence]], [[chen-2023-complex-contagion-influence]], [[borodin-nd-threshold-models-competitive]]

## Salience shock (Noble-Turner going public)

**What it computes.** Rally, retreat and backfire probabilities per issue, member steps, and a go-public advisory.

**Method.** z = min(1, |D-R tier gap|/3); psi = uncertainty; b = min(0.35, 0.05 + 0.10 psi); p = (1-b)(1-z), q = (1-b)z. Rally: co-partisan +1, out-partisan +0.5 if bpf >= 0.30. Retreat: co +1, out -1. Vulnerable members x1.35 x susc_mult. Defensive appeal raises the threshold by (1 + DELTA/2).

**Code refs:** `fec/engine/salience_shock.py:1-246`, `fec/moneyball_core.py:731-751`

**Assumptions:**
- Co-partisans move more than out-partisans under salience (Noble & Turner 2024)
- Backfire grows with policy uncertainty
- Electorally vulnerable members respond more

**Evidence (11 bearings: 4 VERIFIED, 5 VERIFIED-SOFTENED, 2 UNVERIFIED; 6 distinct papers).**

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

**Assessment.** Counts computed from the evidence rows: 11 bearings: 4 VERIFIED, 5 VERIFIED-SOFTENED, 2 UNVERIFIED. **Evidence weight: one strong method source with several verified departures from it.** 

The regime structure follows Noble & Turner 2024: rally, partisan retreat and backfire, with co-partisans above out-partisans.

**Verified departures:**
1. **Status-quo gate missing.** defensive_appeal_value is credited on every issue, but in the paper the defensive logic holds only for a favourable status quo. In salience_advisory.json (2026-10-08, 18 issues), all four GO PUBLIC verdicts are offense issues, driven by held counts. Technology & Emerging ranks first while sitting near the paper's never-go-public case. The three defend issues rank 13th-15th. These are heuristic advisory scores, not measured effects.
2. **z conflates elite and mass.** z is the elite stance gap, used to set the mass-base reaction odds. The paper treats these as separate primitives, and its own infrastructure example shows they diverge.
3. **psi and backfire.** psi measures missing posture data, not preference uncertainty. The b(psi) link is not in the paper. The paper's actual Prop 5 damping is off by default (SAL_PSI_DAMP=0).
4. **VULN 1.35.** The paper's 'gambling for reelection' concerns the president, not rank-and-file members. Prop 6 is internally inconsistent on direction.
5. **gridlock_width** (mid-band occupancy) is not Prop 1's gridlock interval.

**Other bearings:**
- Bakker and Merkley support the negative out-partisan step in retreat and backfire. Neither supports rally out-partisan +0.5; Merkley's consensus cue shows a null.
- Joo 2022: auto intensity is not wired into the chain, and its 65% input is inflated by ACU syndication.
- Leads (UNVERIFIED): Zhang 2022 suggests a burst-detector salience onset signal. van der Does 2022 suggests the audience's outgroup share should drive backfire.

**Contradictions:**
- Noble & Turner: defensive appeals pay only under a favourable status quo, and under partisan retreat the actor never goes public against an unfavourable one. The live advisory credits defensive holds on offense issues, and all 4 GO PUBLIC verdicts (of 18 issues, 2026-10-08) are offense issues.
- The paper keeps elite polarization z and base reactions separate. The code derives base-reaction odds from the elite stance gap.
- The paper's psi damps go-public value (Prop 5) and is never linked to backfire. The code maps psi (a data-missingness measure) to backfire probability and leaves Prop 5 damping off by default.
- VULN 1.35 is attributed to Noble A.4. In the paper, gambling for reelection applies to the president, not legislators.
- Rally out-partisan +0.5: Merkley's consensus cue shows no effect, and Bakker shows no positive out-party movement.

**Gaps:**
- There is no evidence that co-partisans of a campaign's champions shift more than out-partisans after a public CU campaign. Fill it with an event study around dated CU public campaigns, using stance-tier change by party relative to the champions.
- Whether issue polarization predicts the retreat regime is untested. The same event panel can test it with z as a moderator, plus a mass-opinion partisan gap per issue, which the system lacks.
- Vulnerable-member amplification is unsupported here. Fill it from the electoral-incentives literature or the event panel, stratified by seat security.

**Paul's position vs the evidence.** Paul highlighted Noble & Turner heavily and the component is described as implementing it 'almost directly'. The verified bearings show at least five departures: the status-quo gate, the z mapping, psi and backfire, VULN, and the gridlock metric. The first changes which issues get a GO PUBLIC verdict. The regime skeleton is faithful; the parameterisation is not the paper's.

**Open questions:**
- Should held-count credit be gated to sq=='defend' as the paper implies? It would reorder verdicts, so it is Paul's call.
- Does the paper's presidential-actor logic transfer to an ACU or industry actor with no shared electoral fate (delta)?
- Should psi be rebuilt as preference uncertainty, separate from press-collector coverage gaps that differ by party and chamber?

**Read first:** [[noble-2024-presidential-leadership-legislative]], [[merkley-nd-party-cues-news]], [[bakker-nd-understanding-partisan-receptivity]], [[joo-2022-image-data-automated]], [[vanderdoes-2022-strategic-identity-signaling]]

## Receptivity layer (susceptibility, incentive, prospect domain, seed probability)

**What it computes.** Per member: susc, susc_mult [0.70, 1.30], edge_mult [0.80, 1.20], pressure_mult = clip(susc x incent x pt, 0.50, 1.60), and seed_prob = clip(0.60 + 0.10 coop + 0.06 breadth + 0.04 visibility, 0.30, 0.90). The rank is not affected.

**Method.** Behavioural basis (default): susc = .40 cooperation_v2 + .30 N(affective) - .20 diligence - .10 dominance, blended 0.70/0.30 with persona when n >= 15 cues. Lexical basis: .40A + .30N - .20C - .10E. incent: retiring 0.60, else clip(1 + .30(comp - 0.5), 0.85, 1.15), neutral when COMPET_MULT is on. pt: 1.15 loss domain, 0.92 safe. PERSONA_W 0.30 is kept by owner ruling. Recorded double-count: cooperation_v2 vs bpf rho +0.888.

**Code refs:** `fec/members/receptivity_layer.py:1-216`, `fec/moneyball_core.py:185-213 (rc)`

**Assumptions:**
- Neuroticism degrades decisions under pressure (Byrne et al. 2015); the A, C and E signs are design priors, not Byrne findings
- Final-term members are less responsive (Fouirnaies & Hall)
- Members in a live race are in the loss domain and take risks (Linde & Vis 2016)
- No N susceptibility signal has been demonstrated (Nokken-Poole drift MISS; H5 null)

**Evidence (22 bearings: 4 VERIFIED, 11 VERIFIED-SOFTENED, 7 UNVERIFIED; 19 distinct papers).**

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

**Assessment.** Counts computed from the evidence rows: 22 bearings: 4 VERIFIED, 11 VERIFIED-SOFTENED, 7 UNVERIFIED. **Evidence weight: weak.** No bearing links any trait or persona measure to a legislator's response to lobbying or money. 

**Susceptibility composite:**
- **N.** Byrne et al. (undergraduates, self-report BFI, lab delay task) support only the direction of an N-under-pressure term.
- **A.** The A-above-N weighting is not supported. The claimed A interaction comes from Exp 2, which has no low-pressure arm. The default basis fills the A slot with cooperation_v2, which correlates +0.888 with bpf, a recorded double count.
- **C and E.** Byrne gives no support to the C or E dampeners; C's one correlation runs the other way and was not replicated. Palm 2025 (UNVERIFIED) ranks N least susceptible to persuasion, and suggests N may act differently on the peer and direct channels.

**Validity record.** Koutsoumpis 2024 (closed-vocabulary categories explain 5.07% of self-report variance) supports keeping the lexical basis out of the default. Poole 1999 is consistent with the Nokken-Poole drift MISS (2026-07-08, n=441) and argues against another drift look.

**Incentive term:**
- Fouirnaies & Hall 2022 is about term-limited *state* legislatures: effort -0.24 SD, no position change.
- It does not support the 0.60 size or a 'pressure response' mechanism.
- Its lower-bound argument says members seeking other office keep their incentives, which contradicts their inclusion on the retiring list (e.g. Barr).
- It does not test seat competitiveness, though the code attributes the comp term to it.

**Prospect domain.** Linde & Vis show the reflection effect is real: paid lottery, risky choice 39% under gain vs 76% under loss, n=46. Its domain was randomly assigned, not inferred from seat security. The 1.15/0.92 sizes and 'loss-averse overall' are unsupported.

**COMPET_MULT side effect.** With COMPET_MULT on, the candidate-specific Ballotpedia comp term is neutralized. The Cook 2024 table shows candidate-specific ratings carry information PVI lacks: Fitzpatrick EVEN/Likely R, Salazar EVEN/Solid R.

**Persona and visibility:**
- Birkenmaier 2025 measures the projected persona only, with no behavioural outcome.
- Lechner 2025 offers nothing on behaviour under pressure.
- Amsalem 2018: more visible MPs are less agreeable and less neurotic, so seed_prob's visibility and cooperation terms may pull against each other. The correlation check has not been run.

**Leads (UNVERIFIED):** Celli 2012 (posting-volume confound in text N), Selfhout 2010 (dyadic similarity), Simchon 2024 (trait-message fit). The system's own persona-homophily test was null.

**Contradictions:**
- Fouirnaies & Hall 2022 argue that members running for other office retain electoral incentives (estimates are a lower bound). The retiring list applies incent 0.60 to members seeking other office.
- Fouirnaies & Hall find no ideological position change (|W-NOMINATE| +0.02, SE 0.01) for final-term state legislators. The 0.60 factor encodes reduced responsiveness to pressure.
- The docstring cites Byrne Exp 2 for an A x pressure interaction, but Exp 2 has no low-pressure arm.
- Linde & Vis found no loss aversion. The docstring calls politicians 'loss-averse overall'.
- Palm 2025 (UNVERIFIED lead): N is least susceptible to persuasion messages, while the composite gives N +.30.

**Gaps:**
- There is no evidence that higher-susceptibility members change cosponsorship or votes more after lobbying contact. The prospective H2 contract is the designated test; it needs logged contact and a pre-registered single look.
- Retiree responsiveness to PAC money is untested. Fill it with a roll-call alignment panel comparing final-term to continuing members, separating those seeking other office, with party strata.
- Risk-taking by competitive-seat members on CU issues is untested. Fill it by comparing stance variance or position extremity by seat security, stratified by party.

**Paul's position vs the evidence.** Paul wants psychological traits to adjust the response to pressure as edge parameters (Oropeza), and notes 10 personalities collapsing into 4. The evidence and the system's own validity record undercut this more than his notes acknowledge: drift MISS, H5 null, lexical traits judged unfit. No bearing connects a trait to legislator response to lobbying; the strongest trait evidence is lab undergraduates. PERSONA_W 0.30 is kept by owner ruling and is not revisited here.

**Open questions:**
- Should members seeking other office be removed from the 0.60 retiring factor, as Fouirnaies & Hall's own argument implies?
- With COMPET_MULT on, should the candidate-specific Ballotpedia comp term be restored alongside PVI-based c?
- What are corr(visibility_z, coop_v2) and corr(visibility_z, affective N) on the roster? These are cheap read-only checks.
- Is the cooperation_v2/bpf double count (rho +0.888) acceptable, given bpf also enters the kernel, LT priming, rally and the target bonus?

**Read first:** [[byrne-2014-chokes-under-pressure]], [[fouirnaies-2022-electoral-incentives-affect]], [[linde-2016-politicians-take-risks]], [[koutsoumpis-2024-kernel-truth-text]], [[birkenmaier-2025-measuring-politicians-public]], [[amsalem-2018-effect-politicians-personality]]

## Competitiveness multiplier c (COMPET_MULT)

**What it computes.** c = 1 + 1.0 x exp(-max(lean, 0)/5.0), range (1, 2], where lean = Cook PVI oriented to the member's party. It scales effective dollars on both money legs.

**Method.** Matched by Cook incumbent or district, with overrides; unmatched members get 1.0. Default ON since 2026-10-06 (D-104/D-105/D-107/D-108). Real dollars are unchanged.

**Code refs:** `fec/moneyball_core.py:74-172`, `fec/engine/build_competitiveness.py:1-304`, `data/money/competitiveness_c.json`

**Assumptions:**
- Dollars buy more in competitive seats (reciprocity NOT validated)
- AMPLITUDE and SCALE are uncalibrated owner judgment

**Evidence (11 bearings: 4 VERIFIED, 5 VERIFIED-SOFTENED, 2 UNVERIFIED; 11 distinct papers).**

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
| [[ansolabehere-2002-little-money-politics]] | 2002 | qualifies | Closeness raises the quantity of money. In the gubernatorial panel (1976-2000, N = 326, state FE) the closeness coefficient on log per-capita spending is 2.93. In House races of 1992-2000, close seats (under 5 points) draw 60 percent of funds from individuals and 31 percent from PACs, while safe seats (30 points or more) draw 48 and 46 percent. Most of the gap in the individual share comes from gifts under 500 dollars. | The paper shows the supply side only: more money, mostly from individuals, flows to close races. It says nothing about per-dollar influence. The PAC evidence is in shares, not dollar levels. Higher receipts in close seats shrink r and partly offset c. Reciprocity is not validated (D-104..D-108), and this paper does not validate it. | moderate | VERIFIED-SOFTENED |

**Assessment.** Counts computed from the evidence rows: 11 bearings: 4 VERIFIED, 5 VERIFIED-SOFTENED, 2 UNVERIFIED. **Evidence weight: no support for the premise.**  None shows a dollar buys more access or alignment in a competitive seat. Reciprocity is NOT validated (D-104..D-108), and nothing here changes that.

**Supply side only.** Several papers show money *flows* to competitive seats:
- Gaynor 2022: in the 115th House, vulnerable members are about 70% (R) and 75% (D) more likely to receive member LPAC ties; denominators 238 R and 168 D connected nodes.
- Ansolabehere 2002: close House seats 1992-2000 draw a larger individual share. In the governor panel (1976-2000, N=326), closeness raises spending.
- Bouton 2022: Democratic candidates only, 2012-2020. UNSTRATIFIED by party.
- Bonica 2013: donor allocation.

Two consequences follow:
1. This is exactly the strategic-targeting confound that any future estimate of c must handle (Blackwell 2026; Cagala 2021 shows the randomized-variation standard).
2. Higher receipts in close seats shrink r = amount/receipts and partly offset c.

**Heterogeneity never measured.** Kalla & Broockman blocked on Obama 2012 share and reported no heterogeneity by competitiveness.

**PVI is not a rating.** In the Cook 2024 table (435 districts, as of 2024-09-12), EVEN seats get five different ratings, and an EVEN Solid R incumbent still gets the maximum c = 2.0. The table supports PVI as a partisan baseline, not as vulnerability.

**Redistricting leads (UNVERIFIED).** Best 2022 and McCartan 2022: map choice can move lean by 3-4 points, which moves c by about 0.45-0.55 near lean 0. This matters most in the ten flagged mid-decade-redistricted states. Becker suggests a constituency-turnover diagnostic.

**Contradictions:**
- none

**Gaps:**
- There is no evidence that the marginal effect of PAC dollars on access or alignment is larger in competitive seats. Fill it with a heterogeneous-effect design: randomized or quasi-random variation in contributions, or a continuous-dose DiD with an interaction by PVI. It must handle strategic targeting (Blackwell) and pass a transfer check (Cagala). Report by party stratum.
- The decay scale (5 PVI points) and amplitude (1.0) are unfitted. The same design could fit them with a flexible lean interaction.
- Map sensitivity of c is unmeasured in redistricted states. Fill it by comparing c under 2025 and 2026 PVIs, and against Cook ratings, for the ten flagged states.

**Paul's position vs the evidence.** Paul endorses Cook PVI as a vulnerability measure (Gaynor) and reads competitive seats as responsive. Gaynor supports PVI as a signal donors act on. No source Paul annotated, and no bearing here, says dollars buy more in competitive seats. The multiplier rests entirely on owner judgment, which the code header already states.

**Open questions:**
- Should c use Cook ratings or candidate-specific security rather than raw PVI, given the rating/PVI divergence (e.g. EVEN Solid R seats get c=2.0)?
- How much does the receipts denominator offset c in close seats? Is the net effect on r near 1?
- Should redistricted-state members carry a c uncertainty band?

**Read first:** [[gaynor-2022-financial-ties-bind]], [[ansolabehere-2002-little-money-politics]], [[report-2024-2024-house-glance]], [[kalla-nd-campaign-contributions-facilitate]], [[blackwell-2026-effect-political-advertising]], [[cagala-2021-optimal-targeting-fundraising]]

## Electoral security, leverage and contest-defend plays

**What it computes.** Security percentile from Ballotpedia Election_Average; leverage = 1 + 0.8(1 - security); play routes contest-primary, contest-general, defend or none.

**Method.** Defend value = comp x positive gap vs the cross-party average. Contest route = the larger of the primary and general gaps. Contest plays convert opponents in spread().

**Code refs:** `fec/engine/build_electoral.py:7-31`, `fec/engine/build_electoral_plays.py:20-36`

**Assumptions:**
- Ballotpedia electoral average measures security

**Evidence (5 bearings: 1 VERIFIED, 4 VERIFIED-SOFTENED, 0 UNVERIFIED; 5 distinct papers).**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[report-2024-2024-house-glance]] | 2024 | suggests-extension | The table gives a forward-looking expert risk category (Solid, Likely, Lean, Toss Up) for each seat, along with OPEN and VACANT flags: 41 OPEN lines and 4 VACANT seats as of 2024-09-12. | Useful only as the design pattern for a dated, forward-looking risk input. Using it would require the current-cycle Cook ratings, dated before the decision, not this 2024 table. Open-seat retirees would also need the same retiring treatment the receptivity layer already gives them. | weak | VERIFIED-SOFTENED |
| [[gaynor-2022-financial-ties-bind]] | 2022 | qualifies | Vulnerability is associated with higher inward LPAC degree, for both raw and weighted degree, from a regression reported in the text. Leadership is associated with higher outward degree. Givers target electorally exposed co-partisans. | Backs vulnerability as a magnet for co-partisan money, consistent with defend plays. Gives no evidence that competitive seats are cheaper to move. The paper uses Cook PVI while this component uses Ballotpedia Election_Average, so two vulnerability measures feed the engine. Applies within party only. | weak | VERIFIED |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | qualifies | Text PDF p.20: the partisan electoral model's main implication is that donors fund competitive races to maximize party seat share. Table 2 (PDF p.21): for individuals partisan -1405855 vs ideological -1334200, so the partisan electoral model fits worse. Paper also says the ideological model significantly improves fit over a partisan model. | The paper states a hypothesis it rejects for individual donors; it neither supports nor refutes the comp weighting. It describes donor allocation, not the payoff of a play. | weak | VERIFIED-SOFTENED |
| [[chohan-2025-crypto-oligarchy-cryptocurrency]] | 2025 | context | Fairshake campaigned against Katie Porter in California's Senate primary and opposed Jamaal Bowman's re-election bid. The paper calls this influence 'decisive' and offers no counterfactual or estimate. | Shows that federal primary-contest plays by an industry actor occurred in 2024. It is not evidence that such plays work, because outcomes are confounded and there is no comparison group. Do not use it to calibrate conversion. | weak | VERIFIED-SOFTENED |
| [[porter-2025-campaignview-campaign-platforms]] | 2025 | suggests-extension | Platform text for 5,228 of 6,016 (86.9%) major-party ballot-eligible House primary candidates 2018-2022, with 43,465 hand-topic-coded platform points. Identifiers are FECCandID (not universal) and BioGuideID (incumbents only). | House only, 2018-2022 cycles; the 14 topic codes are not CU categories. BioGuideID is incumbent-only and the paper advises against aggregating by either ID. Challenger coverage gaps (low fundraising, uncompetitive races) are where the proxy matters most. New analysis stays UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED |

**Assessment.** Counts computed from the evidence rows: 5 bearings: 1 VERIFIED, 4 VERIFIED-SOFTENED, 0 UNVERIFIED. **Evidence weight: weak.** 

**Defend plays.** Gaynor 2022 (115th House, within party) shows vulnerability attracts co-partisan LPAC money, which is consistent with defend plays. It gives no evidence that low-security members are cheaper to move.

**Two vulnerability measures.** The engine now uses Cook PVI in c and Ballotpedia Election_Average here. Ballotpedia is backward-looking; the Cook rating table is forward-looking, with OPEN and VACANT flags (41 OPEN lines and 4 VACANT seats as of 2024-09-12).

**Bonica 2013** states, then rejects for individual donors, the partisan-electoral model behind Paul's highlight. It neither supports nor refutes the comp weighting.

**Contest plays.** Chohan 2025 shows industry primary plays happen (Fairshake 2024) but offers no counterfactual. Porter 2025 (CampaignView) could supply challenger platforms for the contest-primary proxy: 5,228 of 6,016 House primary candidates, 2018-2022. Its topics are not CU categories and BioGuideID covers incumbents only.

**Contradictions:**
- Two vulnerability measures feed the engine: Cook PVI in c and Ballotpedia Election_Average in security and leverage. They need not agree, and the receptivity comp term (Ballotpedia) is neutralized when COMPET_MULT is on.

**Gaps:**
- There is no evidence that CU PAC dollars or plays are more effective in later alignment for low-security members. Fill it with an alignment panel interacting contributions with security, stratified by party, with selection handled as for c.
- Contest-play conversion is unmeasured. Fill it with outcomes of past industry primary interventions against a matched comparison set of untargeted incumbents.

**Paul's position vs the evidence.** Paul highlighted Bonica's statement that donors fund competitive races under the partisan electoral model. Bonica's own table shows that model fits individual donors worse than the ideological model. Either way it describes donor allocation, not the payoff of a play. Paul left no annotation.

**Open questions:**
- Should security be harmonized with the PVI or rating source used by c?
- Should forward-looking dated ratings replace the backward-looking Ballotpedia average?

**Read first:** [[gaynor-2022-financial-ties-bind]], [[bonica-2013-mapping-ideological-marketplace]], [[report-2024-2024-house-glance]], [[porter-2025-campaignview-campaign-platforms]], [[chohan-2025-crypto-oligarchy-cryptocurrency]]

## Scenario runner (what-if)

**What it computes.** Change in coalition spread and flip value under user money, defection or stance moves, using CRN at MC 40.

**Method.** A defection shifts all movable positions by at most 1 fine tier.

**Code refs:** `fec/engine/scenario.py:1-120`

**Assumptions:**
- A defection is a general disposition shift across issues

**Evidence (4 bearings: 2 VERIFIED, 2 VERIFIED-SOFTENED, 0 UNVERIFIED; 4 distinct papers).**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[weimer-2023-causal-approach-detecting]] | 2023 | qualifies | The paper's Monte Carlo is a negative control for an estimator applied to observed data: simulate data with no effect, re-run the matching estimator, expect a null. | A Weimer-style placebo applies to empirical estimators that read observed outcomes (forecasters, the contract), not to the forward cascade's deltas, which are model outputs. | weak | VERIFIED |
| [[harrison-2010-introduction-monte-carlo]] | 2010 | context | Monte Carlo is a key tool for analytically intractable problems. Some feel it is overused as a lazy way out, and analytic and experimental alternatives should be considered before committing to simulation. | The split between closed-form flip math and MC spread fits the paper's advice. The paper offers nothing on which analytic proxy (Viral Centrality, Katz-Bonacich) might replace the cascade MC; that comparison would need its own test. | weak | VERIFIED-SOFTENED |
| [[xu-2024-improved-lite-based]] | 2024 | qualifies | Paper reports node updates of 18,468 vs 72,835 (freezing) and 16,864 vs 114,929 (melting) for its improved D* Lite against full global re-planning. Numbers are correct. The paper frames reuse of earlier search data as the motivation (Sec. 2.1) but does not say D* Lite family planners save work only by reuse. | Documentation point only. At 533 members and MC=40 a full recompute is cheap, so incremental reuse is not warranted. The Xu numbers say nothing about the cascade. The cited work is Ren et al., not Xu. | weak | VERIFIED-SOFTENED |
| [[bharathi-nd-competitive-influence-maximization]] |  | qualifies | The best-response guarantee holds only for the last committer. Pure Nash equilibria need not exist and mixed ones do (lines 83-85). Theorem 2 bounds the price of competition at 2 on total activations, not on any one player's share. First-mover results cover only a duopoly on disjoint lines (Lemma 2, lines 149-153). | The fixed-opponent framing holds only if industry commits first and never reacts. FEC filings are public, so if industry reacts, CU becomes a first mover on a general graph, where the paper gives no algorithm or guarantee. The price-of-competition bound says nothing about CU's own share. | weak | VERIFIED |

**Assessment.** Counts computed from the evidence rows: 4 bearings: 2 VERIFIED, 2 VERIFIED-SOFTENED, 0 UNVERIFIED. **Evidence weight: weak; mostly framing.** 

**Weimer 2023.** In that paper, Monte Carlo is a negative control for an estimator applied to observed data. The scenario runner's zero delta for an unperturbed scenario is an identity property of CRN, not a placebo. Weimer-style placebos belong on the empirical estimators (the forecasters and the outcome contract).

**Harrison 2010.** The split between closed-form flip maths and MC spread fits the paper's advice.

**Xu 2024.** The docstring's D* Lite citation (Ren et al.) is a documentation mismatch. Isolation comes from CRN, not from reusing earlier search state, and full recompute is cheap at 533 members and MC=40.

**Bharathi.** Scenarios hold industry fixed, so the follower guarantee holds only if industry commits first and never reacts. FEC filings are public.

**Cross-issue defection.** The core premise, that a defection is a general disposition shift across issues, has no bearing.

**Contradictions:**
- none

**Gaps:**
- The cross-issue defection premise is untested. Fill it with a test of whether members who switch stance tier on one CU issue shift on other CU issues in the same window: within-member co-movement of tier changes across categories, by party.

**Paul's position vs the evidence.** Paul treats Monte Carlo as a tool and highlighted Weimer's use of it as a negative control. That use applies to estimators reading observed outcomes, not to this forward simulator, so the highlight points at the forecasting and contract layer rather than at the scenario runner.

**Open questions:**
- Should the scenario docstring drop the D* Lite citation in favour of a CRN description?
- Should scenarios offer a reacting-industry variant, given that the fixed-opponent assumption is untested?

**Read first:** [[bharathi-nd-competitive-influence-maximization]], [[weimer-2023-causal-approach-detecting]], [[harrison-2010-introduction-monte-carlo]], [[xu-2024-improved-lite-based]]

## Priority anchor, leave-one-out orientation and engagement signal

**What it computes.** Topic priority = tier anchor (1, 2/3, 1/3, 0) + alpha x IMP-weighted stance on other topics (alpha 0.15 flip / 0.25 coalition); engagement = block-weighted legislative activity; base = max(0, eng + 0.15 x priority).

**Method.** Leave-one-topic-out orientation. Fails closed on an incomplete roster.

**Code refs:** `fec/moneyball_core.py:255-317,573-625`

**Assumptions:**
- Stance on other CU issues predicts priority on this issue

**Evidence (1 bearings: 0 VERIFIED, 1 VERIFIED-SOFTENED, 0 UNVERIFIED; 1 distinct papers).**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | qualifies | The second dimension now adds less than 1 percent improvement over the first dimension in both chambers. | engine_orientation reads category_stance scores, not votes. The Path V vote shares (UNVERIFIED) show opposite party direction across some categories (CFPB, Technology, Exam, CU Capital Requirements lean R; Charter, NCUA Budget & Insurance lean D) and none in others. If stance scores share that pattern, leave-one-out borrowing could import party sign; that has not been checked on the stance scores. Alpha is small. | weak | VERIFIED-SOFTENED |

**Assessment.** Counts computed from the evidence rows: 1 bearings: 0 VERIFIED, 1 VERIFIED-SOFTENED, 0 UNVERIFIED. **Evidence weight: effectively a GAP.** 

Poole & Rosenthal 1999 find that the second dimension adds under 1% over the first. That suggests the leave-one-topic-out orientation may mostly carry the party or first-dimension signal.

The Path V vote shares (UNVERIFIED) show party direction reversing across CU categories:
- CFPB, Technology, Exam and CU Capital Requirements lean R.
- Charter and NCUA Budget & Insurance lean D.

If stance scores share that pattern, borrowing from other topics could import the wrong party sign. Nobody has checked this on the stance scores. Alpha is small (0.15 flip, 0.25 coalition), which limits the damage. The engagement block weights have no literature at all.

**Contradictions:**
- none

**Gaps:**
- There is no test that leave-one-topic-out stance predicts held-out-topic stance. Fill it with a predictive check over all CU categories, reported by party stratum and by category, including the categories where party direction reverses.
- Engagement block weights are unvalidated. Fill it with a test of whether block-weighted activity predicts later cosponsorship or stance movement on CU bills.

**Paul's position vs the evidence.** Paul has no literature or stated position here. The one bearing raises a sign-importing risk that he has not addressed.

**Open questions:**
- Does orientation borrowing flip sign for categories where party direction reverses?

**Read first:** [[poole-1999-nominate-after-years]], [[harbridge-yong-2023-bipartisan-path-effective]], [[aref-2021-identifying-hidden-coalitions]]

## Across components

**1. The out-party sign differs across engines.** Different parts of the engine encode four different out-party responses:

| Where | Out-party value |
|---|---|
| Kernel cpf_pressure, entering flip-model nA | **+0.5** (positive pull) |
| cpf_diffusion | at least 0.4 |
| LT priming | sqrt(bpf_u bpf_v), clamped at 0 (about 0 except for bridge pairs) |
| Salience retreat and backfire | **-1** |
| Salience rally | **+0.5** if bpf is at least 0.30 |

The cue evidence (Bakker, Merkley, Furnas 2019, Ringe 2013) consistently points to zero or negative. All of it is mass-public, DC-staff or EP evidence, so it sets a sign prior, not a magnitude. A single, documented out-party convention would remove the internal inconsistency. Choosing it is Paul's call because it moves ranks.

**2. bpf is counted many times.** The bipartisan share enters:
- cpf_diffusion;
- the LT cross-cue factor;
- the rally gate (bpf of at least 0.30);
- the flip target bonus (1+0.3 bpf);
- the receptivity layer, through cooperation_v2 (rho +0.888 with bpf, a recorded double count).

Harbridge-Yong 2023 shows the payoff from bipartisanship is inverted-U, not monotone. No bearing licenses rewarding bridge-builders five times over.

**3. Unvalidated factors stack on a channel the literature says is small.** On the competitor leg, effective dollars can be scaled by:
- c, up to 2.0 (reciprocity NOT validated, D-104..D-108);
- pressure_mult, up to 1.60 (which includes pt 1.15 and susc);
- LOSSFRAME 1.10 (contradicted by its own citation).

That is up to about 3.5x on money whose within-member vote effect is near zero (Ansolabehere 2002, member fixed effects; House 1978-1994, N=3,400). Every factor in the chain is owner judgment.

**4. Two vulnerability measures disagree.** c uses Cook PVI. Electoral plays and the receptivity comp term use Ballotpedia security, and COMPET_MULT neutralizes the receptivity comp term. The Cook 2024 rating table (435 districts) shows PVI and candidate-specific risk diverge: EVEN seats get five different ratings.

**5. Transfer conditions are poor across the board.** Fouirnaies & Hall (term-limited state legislatures) is not a Congress result. Other sources are equally far from US members:
- Linde & Vis: 46 Dutch MPs.
- Byrne: undergraduates.
- Bakker and Merkley: mass public.
- Furnas 2019: staff stated intentions.
- Ringe: European Parliament.
- Cai, Moreno and Jacoby: simulations.

The only US-Congress causal evidence on money is Kalla & Broockman (access only, 2013) and the Ansolabehere panel (votes, observational with member fixed effects).

**6. Project-rule flags:**
- Trivedi 2020's degree-based edge probability must not be read as support for the REJECTED per-edge volume normalization.
- No bearing argues for dropping regular cosponsorship edges.
- Several papers measure LDA-style lobbying dollars (Huneeus & Kim, Jacoby, de Figueiredo, Fremeth, Furnas nd). None licenses LDA as an edge or weight (D-021).
- 'Reciprocity' appears three ways, and none validates the reciprocity premise behind c: Haythornthwaite's reciprocated ties, Palm's Cialdini strategy, and Harbridge-Yong's cross-party exchange.
- All engine outputs (pflip, seed_prob, spread, GO PUBLIC scores) are heuristic model outputs, not access, vote or introduction probabilities (D-025).

**7. Citation-hygiene defects found in passing:**
- the Volden-Wiseman +8-14% bonus figure is absent from the paper;
- WM/WP are labelled CALIBRATED with no calibration behind them;
- LOSSFRAME is attributed to Linde & Vis;
- VULN is attributed to Noble A.4;
- the comp term is attributed to Fouirnaies & Hall;
- the D* Lite citation in the scenario docstring does not match what the code does;
- the flip-model component note says there is no passage-bonus code line, while the bearing shows flip_model.py:142 applies it.

## Research agenda

1. Cheap read-only robustness runs, no new data needed: (a) frontier seed order under saturation constant 3/6/12; (b) cascade CU reach with A-first vs B-first vs randomized same-hop tie order under identical CRN draws; (c) rank-flip rates at MC 20/40/200; (d) corr(visibility_z, coop_v2) and corr(visibility_z, affective N) on the roster.
2. Salience status-quo audit: recompute the 18-issue advisory with defensive held-credit gated to sq=='defend' (Noble & Turner Prop 2 logic) and report how the verdict order changes. This is presented to Paul; it is not applied.
3. Out-party cue test on US data: a cosponsorship-join hazard model on CU bills with exposure split into co-party and out-party prior joiners, stratified D-D, R-R and D-R and by bpf. It covers the kernel 1.7/0.5, the LT clamp, the bridge exemption and the rally +0.5 step together.
4. Money dose-response panel: member-level continuous-dose DiD (Callaway et al. 2025) of CU and industry PAC money as a share of receipts on CU roll-call and cosponsorship alignment. Use member fixed effects, an unfunded comparison group, party strata, and an interaction with PVI and with security. Handle strategic targeting (Blackwell 2026). This is the only route to evidence on WM/WP, HALFF, MAXSHIFT, LOSSFRAME and c.
5. Contact-to-take-up study: obtain CU-side member-level contact logs (not LDA, D-021), join them to cosponsorship deltas, and estimate take-up and stance-tier transitions by starting tier for SGATE and PERS, including possible backfire in low tiers. Run it through the prospective H1/H2 contract (at least 150 members), with a single pre-registered look.
6. Retiree split: compare final-term members who are leaving office with those seeking other office on post-announcement alignment and responsiveness, per Fouirnaies & Hall's lower-bound argument, stratified by party.
7. Cross-issue co-movement: test whether a stance-tier switch on one CU category predicts switches on others in the same window (scenario-runner premise). Run the leave-one-topic-out predictive check by party and category, including the categories where party direction reverses.
8. Cascade back-test: compare simulated vs observed cosponsor accumulation curves on past CU bills under a pre-registered fit metric, with the bystander rule min(0.9, $/400k) tested as a separate component.
9. Vulnerability harmonization study: compare Cook PVI, Cook ratings and Ballotpedia security as predictors of the dose-response heterogeneity above. In the ten mid-decade-redistricted states, check c sensitivity to 2025 vs 2026 maps (Best/McCartan leads, UNVERIFIED).
