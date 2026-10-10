---
type: system
name: "Melange influence engine (persuasion, money and cascade physics)"
slug: melange-influence-engine
components: [melange-influence-engine.cascade-kernel, melange-influence-engine.competitive-cascade-crn, melange-influence-engine.seed-gate, melange-influence-engine.flip-model, melange-influence-engine.persuadability-table, melange-influence-engine.money-response-defection, melange-influence-engine.lt-priming, melange-influence-engine.salience-shock, melange-influence-engine.receptivity-layer, melange-influence-engine.competitiveness-multiplier, melange-influence-engine.electoral-plays, melange-influence-engine.scenario-runner, melange-influence-engine.priority-engagement]
brain_notes: ["topics/2026-07-feature-influence-engine-internals", "topics/2026-07-feature-influence-engine-internals-receptivity-layer", "topics/2026-07-feature-influence-engine-internals-salience-sna-wave", "topics/2026-06-feature-moneyball-excel-model-build-scenario-tools-and-calibration"]
paper_count: 71
tags: [system]
---

# Melange influence engine (persuasion, money and cascade physics)

Behavioural simulation layer that sits downstream of the rank. It covers the shared cascade kernel and partisan edge factors, competitive independent cascade with common random numbers, contingent seeding, the flip/winnability model, money-to-lean response and defection, linear-threshold priming, salience shocks, the receptivity (susceptibility) layer, the competitiveness multiplier, electoral plays and the scenario runner. Core code: fec/moneyball_core.py and fec/engine/*.py.

Brain notes: [topics/2026-07-feature-influence-engine-internals](obsidian://open?vault=brain&file=topics%2F2026-07-feature-influence-engine-internals); [topics/2026-07-feature-influence-engine-internals-receptivity-layer](obsidian://open?vault=brain&file=topics%2F2026-07-feature-influence-engine-internals-receptivity-layer); [topics/2026-07-feature-influence-engine-internals-salience-sna-wave](obsidian://open?vault=brain&file=topics%2F2026-07-feature-influence-engine-internals-salience-sna-wave); [topics/2026-06-feature-moneyball-excel-model-build-scenario-tools-and-calibration](obsidian://open?vault=brain&file=topics%2F2026-06-feature-moneyball-excel-model-build-scenario-tools-and-calibration)

### Melange influence engine: evidence assessment

**Source of this assessment:** hub file `melange-influence-engine.json` (13 components, 113 evidence rows, 66 system citations). Status mix: 34 VERIFIED, 66 VERIFIED-SOFTENED and 13 UNVERIFIED (71 distinct papers). The UNVERIFIED rows are single-agent leads (DE-062) and are not counted as findings.

**Overall verdict.** No evidence row estimates any engine constant from congressional data. That covers the saturation constant 6, cpf 1.7/0.5, the 0.4 diffusion floor, the bystander rule min(0.9, $/400k), SGATE, PERS, WM/WP, HALFF, MAXSHIFT, LOSSFRAME, the THETA ladder, the VULN multiplier of 1.35, the receptivity weights, c's AMPLITUDE/SCALE and LELEV. The literature mostly gives **direction-of-effect priors**, and most of it was measured outside the U.S. Congress:
- mass-public survey experiments (Bakker; Merkley);
- stated intentions of DC staff (Furnas 2019, N=441 staff in 2017, 5.2% response);
- one field experiment on access (Kalla & Broockman, 191 House offices of one party, June-Aug 2013);
- 46 Dutch MPs (Linde & Vis, 2014-15);
- state chambers under binding term limits (Fouirnaies & Hall);
- a European Parliament committee (ringe-2013-pinpointing-powerful-voting, 1,319 dyads, 2004-08);
- simulations (Jacoby, Cai, Moreno, Chen).

**What holds up:**
- Edge-level common random numbers with first-colour-wins match the deterministic equivalence in Bharathi-Kempe-Salek.
- The shared two-colour kernel stays outside Borodin's hardness scope.
- The direction "money is a minor lever on positions" is supported (Bombardini review; Poole stability; Jacoby simulation). The Kalla cosponsorship estimate is uninformative, not a null.
- The lexical-to-behavioural switch in the receptivity basis is consistent with Koutsoumpis (52 lexical categories explain 5.07% of self-report variance).
- The zero clamp on non-bridge out-party cues in lt-priming fits every cue study in the set.

**Plain contradictions:**
1. **Salience shock.** The defensive-appeal credit is added on offense issues. In the live advisory (`salience_advisory.json`, 2026-10-08, denominator 18 issues), all four GO PUBLIC verdicts are offense issues. Technology & Emerging ranks first even though it sits near the case where Noble-Turner says never to go public. This is an internal-consistency failure against the model the code claims to implement.
2. **LOSSFRAME 1.10.** It cites Linde & Vis, but that paper found no loss aversion in MPs (n=46).
3. **Out-partisan sign.** cpf_pressure gives an out-partisan source +0.5 pull. Bakker (out-party cue about -.06 to -.07 on a 0-1 scale) and Merkley (backlash) point to zero or negative. Furnas 2019 also puts the non-aligned penalty at about twice the aligned gain, while the system's gain (+0.7) exceeds its penalty (-0.5). Furnas is null when pooled, so its figures must be quoted by party stratum.
4. **Access premise.** Furnas 2019 (stated staff intentions, 2017) finds no donor meeting premium, which contradicts the access half of the WM/WP comment that Kalla & Broockman support.
5. **Retiree list.** Fouirnaies & Hall argue that members running for other office keep their electoral incentives, yet the retiring list applies incent 0.60 to them (for example, Barr and Stefanik). The paper also finds no position change, only an effort drop of -0.24 SD.

**Mislabelled or misattributed constants:**
- WM/WP and MAXSHIFT are labelled CALIBRATED, but nothing calibrates them.
- The competitiveness term 1+.30(comp-0.5) is attributed to Fouirnaies & Hall, which never tests seat competitiveness.
- VULN 1.35 is attributed to Noble A.4, but A.4 concerns the president's reelection.
- The b-psi backfire link is attributed to Prop 5, which does not link psi to backfire.
- A Volden-Wiseman passage bonus appears in a comment but not in the code.
- The scenario reuse claim cites D* Lite, but isolation actually comes from CRN.

**Gaps:**
- priority-engagement has a single weak row and is effectively unevidenced.
- No component has a congressional test of its core behavioural assumption: threshold adoption of CU bills, take-up after contact by stance tier, contribution effects by competitiveness, or cross-issue defection.

**Guardrails applied throughout:**
- pflip, seed_prob, gate and leverage are heuristic model outputs, never access or introduction probabilities (D-025).
- Reciprocity is not validated (D-104..D-108), and no paper here validates it. Coleman's "reciprocated ties" and Cialdini's "reciprocity" are unrelated constructs.
- LDA stays an identity source only (D-021), so no row is read as a call to add a lobbying-dollar channel.
- Trivedi's degree-based edge probability is **FLAGGED** and must not be read as support for the REJECTED per-edge volume normalization.

## Cascade activation kernel build_P and partisan edge factors

**Computes:** p(u->v) = x/(x+6) with x = w x cpf x edge_mult(v). cpf_diffusion = 1 for same party and 0.4 + 0.6 sqrt(bpf_u bpf_v) across D/R. cpf_pressure = 1.7 co-partisan, 0.5 out-partisan.

**Method:** One shared kernel for cascade, coalition, frontier, scenario, priming and route engines. bpf is the bipartisan share (default 0.24). The two physics factors are deliberately never merged.

**Code refs:** `fec/moneyball_core.py:1-12,68-72,543-562`

**Assumptions:**
- Cross-party contagion flows through bridge-builders
- Co-partisan sources persuade more and out-partisan sources less (Furnas/Bakker source alignment)
- Saturation constant 6 is a design choice, not estimated
- Receiver susceptibility, not sender loudness, governs activation

**Evidence (11 rows; 11 distinct papers; 2 VERIFIED, 9 VERIFIED-SOFTENED):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[furnas-2019-moneyed-interests-information]] | 2019 | qualifies | Pooled over all staff, minimal evidence that think-tank evidence raises siding with the requester (if anything, less likely than 'own analysis'). Split by party, aligned think-tank evidence raises siding and non-aligned lowers it, and for both parties the non-aligned penalty is roughly twice the aligned benefit (Fig. 4). The reference category is stated inconsistently: 'own analysis' in the p.12 prose, 'supportive polling' in the Fig. 4 note. Stated intentions; N=441 staff (5.2% response), 549 ratings per factor; party subgroup Ns not reported. | The paper randomises the ideology of an outside requester's evidence and records DC staffers' stated likelihood of siding with it (2017, single hypothetical). The system applies the factor to legislator-to-legislator cosponsor edges, where the partisan relation is between two Members, not an evidence source. That supports the direction of a source-alignment term, but not its 3.4 ratio or the gain-over-penalty asymmetry. If anything, the paper points to a penalty larger than the gain. The pooled estimate is null over the party fault line, so any figure taken from the paper must be quoted by party stratum or labelled UNSTRATIFIED. | moderate | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | supports | Citing Coleman, Katz and Menzel (1966): some sources inform (mass media, drug salesmen) while others legitimate adoption; the legitimating sources, other doctors, were most influential, and close reciprocated ties carried legitimacy. Citing Granovetter (1973): over 80 percent of job information came from occasional or rare contacts, under 17 percent from frequent contacts. | Conceptual parallel only. The cited evidence concerns physicians and job seekers, not legislators, and the article gives no Coleman sample size or data-collection dates. It supports separating an information channel from a legitimation channel. It does not support the 1.7 and 0.5 ratios, the 0.4 floor or the constant 6. Coleman's reciprocated ties must not be imported as a reciprocity weight, since reciprocity is not validated (D-104 to D-108). | weak | VERIFIED |
| [[gaynor-2022-financial-ties-bind]] | 2022 | suggests-extension | Within a party, Model A (both members in the same caucus) is positive for Freedom +1.208*** and Tea Party +0.686*** (R), and Progressive +0.381* (D, p<0.1). It is negative for Tuesday Group -1.033*** and Liberty -0.389*. Model D (recipient in caucus, donor not) is -1.020*** for Freedom and -0.545*** for Tea Party. Model C (donor in caucus, recipient not) is -1.780*** for Freedom and -0.314*** for Tea Party. Mean transitivity R 0.497, D 0.2768. | Evidence is binary LPAC donation ties with amounts ignored, not cosponsorship diffusion or persuasion, so it cannot set a faction multiplier. It shows party is not a homogeneous block for money ties, and the fault line is party-specific: any pooled faction coefficient needs party strata. 115th House primary; 116th replication is partial (Tables A1-A4). | weak | VERIFIED-SOFTENED |
| [[trivedi-2020-efficient-influence-maximization]] | 2020 | qualifies | Structure-derived edge probabilities p_ij = 0.01 + (d_i+d_j)/n + CN(i,j)/n raise spread for every algorithm on the co-authorship graphs, e.g. Degree +32.60% (NetPhy, k=20) and CELF +55.55% (NetPhy, k=10). The form is asserted to reflect real propagation dynamics and is not fitted to or validated against observed diffusion. | The paper gives no calibration or validation, so it cannot support tuning or replacing the constant 6. Its gains are percent increases over a 0.01 baseline that it says gives very little influence and does not favour real-world dynamics; calling the gains mechanical is this analysis's reading, not the paper's. The common-neighbour term is a candidate covariate only. The degree term raises probability for hubs and must not be presented as support for the REJECTED per-edge volume normalization. | weak | VERIFIED-SOFTENED |
| [[bakker-nd-understanding-partisan-receptivity]] |  | contradicts | Out-party cues lower policy support by about .06 (study 1, p < .01, N = 747), about .07 (study 2, no p-value stated in the text) and about .07 in the pooled studies 1-4 meta-analysis (no p-value stated). In-party cues add about .03, non-significant in studies 1 and 2 and reported without a test in the pooled model. The out-party sign is negative, so the paper supports no positive out-partisan persuasion weight. | Direction only. The paper measures mass-public 0-1 policy support after one embedded cue, on low-salience issues, in 2016-2018 non-probability online panels. It does not measure legislator-to-legislator pressure, cosponsorship or lobbying. The sign result supports zeroing or reversing the out-partisan weight, not the 1.7 amplification, since the paper's in-party effect is small and often null. Legislator-side constants need elite cue evidence such as Box-Steffensmeier et al. | moderate | VERIFIED-SOFTENED |
| [[ringe-2013-pinpointing-powerful-voting]] |  | qualifies | Among opposing-bloc MEP dyads, higher point connectivity predicts less co-voting. Table 3 model 3 log-odds multilevel coefficient is -0.1020 (SE 0.0307), about 10 percent lower odds per unit (95 percent CI 7-13 percent). Predicted co-voting falls from 52 to 44 percent between connectivity 0.6 and 3.8 (about -1 to +1 SD). Window: 40 Environment Committee roll calls, July 2004 to July 2008. Denominator: 1,319 non-unanimous dyads (perfect co-voters excluded in model 3). | Paper tie is staff-reported contact, not cosponsorship. Setting is a multi-party EP committee, not two-party US Congress. Result is cross-sectional association with acknowledged simultaneity and specification sensitivity (fn. 81). It does not show cross-party diffusion is zero or negative in US cosponsorship. Use it to motivate a test stratified by party pair, not to change a constant. | weak | VERIFIED-SOFTENED |
| [[ringe-nd-pinpointing-powerful-voting]] |  | qualifies | Converting cueing networks into voting data loses network structure: simple networks are recovered best, moderate and complex networks less well, and betweenness recovery is only about 0.25 correlation. | The paper's result concerns recovering node centralities from simulated voting records; it says nothing about cascade edge probabilities or the saturation constant 6. The analyst's claim that kernel edges come from cosponsorship weights was not checked, because load_network was not opened in this pass. Relevance is only as a caution against calibrating kernel edges from roll-call agreement. The evidence is simulation, not US data. | weak | VERIFIED-SOFTENED |
| [[merkley-nd-party-cues-news]] |  | qualifies | Democratic cues move Republicans toward skepticism, away from the cued position, in both the experiment and the time series (backlash rather than attenuated persuasion). Republican cues have no aggregate effect (chi2 0.00, p=0.94) and only a weak effect on Republican skepticism (chi2 2.94, p~0.09). The authors say co-partisan sources dominate. | The paper supports co-partisan sources dominating, which fits the 1.7 versus 0.5 ordering. It reports backlash, meaning movement away from the cued position, but estimates no multiplier relative to co-partisan influence, so it cannot show the out-partisan sign should be at or below zero rather than 0.5. Cosponsorship ties are observed behaviour rather than cues, so transfer is weak. The paper gives no support for the 1.7/0.5 ratio itself. | weak | VERIFIED-SOFTENED |
| [[zareie-2023-influence-maximization-social]] | 2023 | suggests-extension | Trust-agnostic opinion-aware methods consider that all network relationships are positive (friend), whereas in reality networks may also include negative (foe) relationships. Trust-aware work uses signed graphs on the premise that opinion change depends on trust between users. The survey also warns that numeric friend-or-foe encodings may not capture attitude. | A design taxonomy only, not evidence that signed edges improve prediction of Congressional cosponsorship. Adopting foe edges would need a second path showing that negative references predict lower future cosponsorship (DE-062). The survey's warning on numeric attitude encodings applies to the LLM valence scores. Signed edges would also change the two-physics architecture, so the owner decides. | weak | VERIFIED-SOFTENED |
| [[chen-2023-complex-contagion-influence]] |  | context | IC and LT assume activation probability independent of how many neighbours are active, which keeps them submodular. Complex contagion breaks this (Sec. 3, lines 254-258 and footnote 2). | Context only. The paper gives no legislative estimate. Any count-dependent kernel would be a new model and must keep the two cpf physics separate. | weak | VERIFIED |
| [[fink-2023-congressional-social-network-dataset]] | 2023 | method-source | Fink offers the directed pairwise probabilities as empirical transmission probabilities that can vary across edges. The paper argues that Monte Carlo SIR-type benchmarks usually assume one homogeneous, arbitrarily chosen rate. | The mapping holds only for untied pairs and only when physics and edge_mult equal 1. Fink rates are directed and normalized per source tweet, while the system symmetrizes them and reads them as per-campaign activation odds, which changes unit and meaning. The 2.0 cap gives p_max of 0.25. Fink covers one 2022 window and says the rates may not be stationary. | moderate | VERIFIED-SOFTENED |

**Assessment:** **Evidence weight: weak to moderate on direction, nil on magnitudes.** 11 rows: 2 VERIFIED, 9 VERIFIED-SOFTENED.

**What supports the design.**
- Co-partisan sources dominate. Supported by Furnas 2019 (aligned evidence helps and non-aligned evidence hurts, by party stratum; N=441 DC staff, 2017, stated intentions), Merkley (co-partisan cues dominate) and Bakker.
- Haythornthwaite (citing Coleman) gives a conceptual basis for separating an information channel (diffusion) from a legitimation channel (pressure). That fits the never-merge two-physics rule, but it is about physicians and job seekers, not legislators.
- Fink 2023 supports heterogeneous, empirically derived edge probabilities over one homogeneous rate. The mapping w = 6p/(1-p) recovers Fink's p only for untied pairs when physics and edge_mult equal 1. Fink rates are directed and per source tweet; the system symmetrizes them. The data cover one 2022 window and may not be stationary. The 2.0 cap limits p to 0.25.

**What contradicts it.**
- The out-partisan term is **positive** in both factors (cpf_pressure 0.5; cpf_diffusion floor 0.4).
- Bakker: out-party cues lower mass-public policy support by about .06 (study 1, N=747, p<.01) to .07 (pooled studies 1-4) on a 0-1 scale, measured in 2016-18 online panels. In-party cues add only about .03 and are often null.
- Merkley: out-party (Democratic) cues produce backlash among Republicans.
- ringe-2013-pinpointing-powerful-voting: among opposing-bloc MEP dyads, higher connectivity predicts **less** co-voting (log-odds -0.102 per unit; 1,319 dyads; 40 Environment Committee roll calls, 2004-08).
- Furnas 2019: the non-aligned penalty is about twice the aligned gain, but the system's asymmetry runs the other way (+0.7 gain, -0.5 penalty).
- No row measures legislator-to-legislator cosponsorship influence, so the contradiction is on **sign and asymmetry**, transferred from mass, staff or EP settings. It is a strong reason to test, not grounds to change a constant on this evidence alone.

**What has no basis.**
- The saturation constant 6 is unsupported. Trivedi's structure-derived probabilities are asserted, not fitted.
- The 1.7/0.5 = 3.4 ratio is unsupported.
- The 0.4 floor and the sqrt(bpf_u bpf_v) bridge form are unsupported. No paper measures a bipartisan-bridge attribute.
- Gaynor shows party is not homogeneous for leadership-PAC money ties (Freedom +1.208, Tuesday Group -1.033; 115th House). Any faction term would be party-specific, and a pooled coefficient would need an UNSTRATIFIED label.

**Flags.**
- Trivedi's degree term raises probability for hubs. It is **not** support for the REJECTED per-edge volume normalization.
- Coleman's reciprocated ties must not become a reciprocity weight (D-104..D-108).
- Zareie's signed (friend/foe) edges would change the two-physics architecture. That is an owner decision and needs a second-path test first (DE-062).

**Contradictions:**
- Out-partisan sign: cpf_pressure 0.5 and cpf_diffusion floor 0.4 give positive out-party pull. Bakker (out-party cue -.06/-.07 on a 0-1 scale, mass public, 2016-18), Merkley (backlash) and ringe-2013-pinpointing-powerful-voting (cross-bloc connectivity predicts less co-voting, EP 2004-08, 1,319 dyads) all point to zero or negative.
- Asymmetry: Furnas 2019 finds the non-aligned penalty about twice the aligned gain, by party stratum (the pooled estimate is null). The system's gain (+0.7) exceeds its penalty (-0.5).
- Bakker's in-party cue effect is small and often non-significant (about +.03), which does not support a 1.7x co-partisan amplification.

**Gaps:**
- Saturation constant 6. No estimate exists. Needed: a hazard model of cosponsorship join timing on CU bills as a function of the summed weight of already-joined ties, fitted to recover the half-saturation point.
- Cross-party diffusion form. Needed: a cosponsorship-diffusion test stratified by party pair (D-R, R-D) and by bpf tercile, checking whether cross-party join hazards rise with sqrt(bpf_u bpf_v) or are flat or negative. Quote by stratum.
- Elite cue magnitudes. Needed: a legislator-side or legislative-staff cue experiment (for example Box-Steffensmeier-type elite cue designs) to replace the mass-public transfer for the 1.7/0.5 constants.

**Paul's position vs the evidence:** Paul treats directed edge probabilities as foundational (Fink) and reads source party as a filter on receptiveness (Furnas). The evidence supports the filter's direction: co-partisan sources dominate. It does not support the system's out-partisan sign. Furnas, Bakker and Merkley, the same sources Paul relies on, point to a penalty or backlash, not a dampened positive pull. Paul's own note that the saturation constant has no literature basis is confirmed. Fink supports heterogeneous edge rates in principle, but the current mapping is valid only for untied pairs at neutral physics. Paul highlighted Bakker and Merkley; read closely, both argue against the 0.5 out-partisan weight more than for it.

**Open questions:**
- Should cpf_pressure's out-partisan value be tested at 0 (matching the lt-priming clamp) against 0.5 in a preregistered one-look comparison? That is Paul's call.
- Is the bpf bridge exemption (sqrt form) empirically distinguishable from a flat cross-party floor in cosponsorship timing?
- Does the receptivity edge_mult, which loads cooperation_v2 (rho +0.888 with bpf), double-count bpf on the same edge alongside cpf_diffusion?

**Read first:** [[bakker-nd-understanding-partisan-receptivity]], [[furnas-2019-moneyed-interests-information]], [[merkley-nd-party-cues-news]], [[ringe-2013-pinpointing-powerful-voting]], [[fink-2023-congressional-social-network-dataset]], [[gaynor-2022-financial-ties-bind]]

## Competitive independent cascade with common random numbers

**Computes:** Expected pro-CU activations in a two-colour first-to-reach race; bystanders turn anti with probability min(0.9, industry$/400000).

**Method:** Keyed splitmix64 draws per (iteration, edge), reseeded with 1000003*SEED+it. SEED 7, MC 20 (scenario 40). Seed activation uses a separate 2^62 key range.

**Code refs:** `fec/moneyball_core.py:549-554`, `fec/engine/budget_frontier.py:40-94`, `fec/engine/per_topic_coalition.py:27-57`, `fec/engine/scenario.py:40-53`, `fec/engine/salience_shock.py:71-81`

**Assumptions:**
- Independent edge activations given the draws (Bharathi-Kempe-Salek competitive IC)
- Industry money mobilizes neutral bystanders
- MC = 20 is adequate

**Evidence (10 rows; 9 distinct papers; 2 VERIFIED, 8 VERIFIED-SOFTENED):**

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
| [[kamarthi-2020-influence-maximization-unknown]] | 2019 | context | Every comparison uses a single-colour independent cascade with uniform p = 0.1, budget 10, and the mean of 100 simulations, with no variance or confidence intervals. Tables 2 and 4 report the best Geometric-DQN variant per test network or per test set. | Gains measured under uniform p = 0.1 single-colour IC do not carry over to the system heterogeneous two-colour kernel. Any reuse needs a rerun on the system own P and stays UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED |

**Assessment:** **Evidence weight: moderate on algorithmic structure; nil on empirical fit, bystander rule and MC adequacy.** 10 rows: 2 VERIFIED, 8 VERIFIED-SOFTENED.

**Supported structure.**
- Bharathi-Kempe-Salek (strong, method-source): edge outcomes fixed in advance plus first-colour-wins is exactly what keyed splitmix64 draws with no colour in the key deliver. One draw per edge serves whichever colour arrives first. This is the soundest piece of the engine.
- Borodin: hardness of approximation applies to models with **separate per-technology edge weights**. The shared kernel P sits outside that scope, but only while it stays shared. Giving the anti side its own physics (for example cpf_pressure on B only) would move the engine into the NP-hard scope.

**Departures from the source.**
- The code drops Bharathi's continuous exponential delays and runs hop-synchronous spread.
- The frontier is built A-first, so same-hop ties resolve **deterministically in A's (pro-CU) favour**. Bharathi avoids ties entirely through continuous delays. Reading the code confirms the departure; its effect on CU reach is unmeasured and stays UNVERIFIED. A B-first or randomized-order rerun under the same draws would measure it cheaply.

**No basis.**
- The bystander rule min(0.9, industry$/400000) has no support in any row.
- MC = 20 (40 in the scenario runner) is the system's own claim. Arya's 100,000 draws are that paper's protocol and say nothing about adequacy here. Harrison warns that simulation is only as good as its inputs.
- No row tests whether competitive IC reproduces observed cosponsorship accumulation on CU bills.
- Kumar and Kamarthi surrogates are single-colour, uniform-p models and do not transfer.
- Cai and Moreno use reversible voter models, a different model class. Moreno's regime point carries over: discrete optimisers drift toward hubs.
- Cai's row explicitly marks the claim that cosponsorship is near-progressive with rare withdrawals as UNVERIFIED. That is the key assumption that makes progressive IC appropriate.

**Contradictions:**
- Tie-breaking: Bharathi avoids simultaneous-arrival ties through continuous delays. The code resolves same-hop ties deterministically for A (pro-CU) through A-first frontier order. The departure is confirmed in code; its effect on reach is unmeasured.

**Gaps:**
- Empirical fit. Needed: a comparison of simulated competitive-IC accumulation curves with observed cosponsor accumulation on CU bills (timing and final counts), alongside an LT alternative.
- Bystander mobilization. Needed: an observational test of whether opposing-industry PAC dollars predict neutral members later taking anti positions, and with what functional form. Currently min(0.9, $/400k) is unestimated.
- MC adequacy. Needed: the system's own mc_stability_check run at MC 20 and 40 on the rank or allocation decisions actually reported. No literature row addresses it.
- Progressivity. Needed: a census of cosponsor withdrawals on CU bills to test whether progressive IC is a fair approximation.

**Paul's position vs the evidence:** Paul wanted rival industries modelled and favoured cheap analytic scenario computation (Viral Centrality). The system chose Monte Carlo with CRN. The evidence supports the CRN and first-colour-wins structure (Bharathi), and Harrison's advice to prefer analytic routes is partly honoured: the flip channel is closed-form while the cascade is MC. Nothing in the set compares Viral Centrality or Katz-Bonacich with the cascade MC, so Paul's preference is neither vindicated nor refuted. Rival industries are represented only as non-strategic bystander activation with an unsupported mapping, which is thinner than Paul's 'conflicting industries' note implies.

**Open questions:**
- How much CU reach does A-first tie resolution add, measured by a B-first or random-order rerun under identical draws?
- Is cosponsorship on CU bills close enough to progressive (rare withdrawals) to justify IC over a reversible model?
- Would a Viral Centrality analytic proxy reproduce the MC frontier ranks closely enough to replace it for scenario use?

**Read first:** [[bharathi-nd-competitive-influence-maximization]], [[borodin-nd-threshold-models-competitive]], [[harrison-2010-introduction-monte-carlo]], [[arya-2025-relink-edge-activation]], [[cai-2025-competitive-influence-maximization]]

## Contingent seed activation gate

**Computes:** gate(n) = max(0.15, seed_prob(n) x SGATE[tier]); committed allies are never gated.

**Method:** SGATE ranges from Highly Persuadable 1.00 down to Opponent 0.35, with unknown 0.85. seed_prob comes from the receptivity layer.

**Code refs:** `fec/moneyball_core.py:626-630`

**Assumptions:**
- A lobbied legislator acts only with a stance-graded take-up probability (Chen et al. UAI 2021 contingency-aware seeding)

**Evidence (5 rows; 5 distinct papers; 1 VERIFIED, 3 VERIFIED-SOFTENED, 1 UNVERIFIED):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | supports | Stochastic submodular maximization with independent item states (failure types) and a monotone submodular objective: greedy obtains (1-1/e) of the best adaptive policy (Thm 16, generalizing Asadpour et al. 2008). Robustness to a misspecified prior is shown for a multiplicative error condition with alpha = d/c (Sec 4.3). | The structure matches: independent per-seed Bernoulli take-up over a spread function. The guarantee also needs spread given activated seeds to be monotone submodular, which is not shown for this competitive kernel, and needs known take-up probabilities. SGATE and seed_prob are unestimated, so the known-prior assumption fails. | moderate | VERIFIED |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | suggests-extension | Thm 5: with independent location states P[Phi] = product over e of P[Phi(e)] and a monotone submodular base f-hat, the adaptive greedy gets (1-1/e) of the best adaptive policy. Thm 6 covers IC cascades with feedback and does not mention take-up failures. | An adaptive guarantee would need observed take-up between picks, independence across members, and a reward on the realized seed set that is monotone submodular and adaptive submodular. The rel's claim that whip and coalition timing correlate take-up is plausible but not shown by the paper. The SGATE values are unestimated, per the rel. The mapping from sensor failure to take-up is an analogy, not a theorem. | moderate | VERIFIED-SOFTENED |
| [[chen-2021-contingency-aware-influence]] | 2021 | method-source | A selected seed is willing with a probability q that is known a priori. Footnote 2 says q is the same for all nodes because per-node values are impractical to know. Default q = 0.6; Q3 varies q over 0.2, 0.4, 0.6, 0.8. The paper estimates no real-world q. | The modeling idea transfers: invitation is not activation. The paper's q is homogeneous and its analysis is multi-round. SGATE is member-specific and used single-shot, so Theorems 1-3 do not cover it. The 0.60 baseline is the paper's simulation default, not an estimate. The source population is HIV peer leaders, not legislators. SGATE values remain design priors until measured. | moderate | VERIFIED-SOFTENED |
| [[zareie-2023-influence-maximization-social]] | 2023 | qualifies | Behaviour-aware IM adds user features (BV), relationship features (BE) and message features (AM) to the structural IM problem (Eq. 1). The survey reports no take-up rate, no effect size and no benchmark, and it classifies methods without comparing their accuracy. It warns that numeric encodings of attitude may not capture personality and that many users are silent. | Supports only the framing that member attributes may enter seed activation. The survey does not discuss contingency-aware seeding, does not cover legislators, and does not justify the 0.60 baseline, the coefficients or the 0.30-0.90 clip, which remain design priors. Evidence route is H1: outcome_contract.py requires at least 150 members with cosponsorship deltas. The F5 count of 72 was not checked. | moderate | VERIFIED-SOFTENED |
| [[madsen-2017-strategic-advantages-micro]] | 2017 | supports | Under Bayesian source-credibility updating, contacts restricted to receptive targets (credibility signal above 0.5) convert better than untargeted contacts. Contacting audiences who view the source unfavourably moves them away from the source's position. | Directional agreement only. The paper's agents are voters, not legislators. Its unit is a source-credibility belief update, not a take-up decision about cosponsoring, and it estimates no take-up rates. The SGATE values remain unestimated design constants; this paper does not calibrate them. | weak | UNVERIFIED |

**Assessment:** **Evidence weight: moderate for the modelling idea, nil for values.** 5 rows: 1 VERIFIED, 3 VERIFIED-SOFTENED, 1 UNVERIFIED.

**What carries over.**
- Chen et al. 2021: an invited seed is not an activated seed.
- Chen's q is **homogeneous** across nodes (footnote 2 says per-node values are impractical), is used multi-round, and is a simulation default (0.6, varied 0.2-0.8). It is not estimated, and the population is HIV peer leaders.
- SGATE is member- and stance-specific and used single-shot, so Chen's theorems do not cover it. The 0.60 baseline in seed_prob is Chen's simulation default, not a measurement.

**Guarantees.**
- Golovin & Krause (both rows) give a (1-1/e) adaptive guarantee only with independent take-up, **known** take-up probabilities, and monotone submodular spread given activated seeds. None is shown here: SGATE and seed_prob are unestimated, and submodularity of the competitive kernel is unproven.
- The allocator also does not condition later picks on observed take-up, so it is non-adaptive.
- Zareie supports only the framing that member attributes may enter seed activation.

**Lead, not finding.**
- Madsen (UNVERIFIED): contacting unreceptive targets can move them **away**. The gate has no negative contact outcome, only lower take-up (floor 0.15).

**Label.** gate(n) and seed_prob are heuristic model weights, never access or meeting probabilities (D-025).

**Contradictions:**
- (none)

**Gaps:**
- Take-up rates by prior stance. Needed: a contact log linked to subsequent cosponsorship (H1 contract: at least 150 members with cosponsorship deltas), estimating conversion by prior stance tier. LD-2 filings never name the member contacted and LDA is an identity source only (D-021), so the contact data must come from non-LDA logs such as CU advocacy fly-in records.
- Backfire on contact. Needed: the same data, testing whether contacts with Opponent-tier members precede movement away (Madsen lead).

**Paul's position vs the evidence:** Paul maps contingency-aware seeding directly onto legislators who may or may not move when approached. The concept transfers. The implementation goes beyond the source: Chen assumes one known homogeneous q, while SGATE grades take-up by stance with unestimated constants and reuses Chen's simulation default as a baseline. The direct mapping is an analogy, and the SGATE values are design priors until measured.

**Open questions:**
- Does any CU advocacy contact log exist with dates and member identities sufficient for an H1-sized test?
- Should the Opponent-tier gate allow a negative (backfire) outcome, or is the 0.15 floor the intended conservative reading?

**Read first:** [[chen-2021-contingency-aware-influence]], [[golovin-2011-adaptive-submodularity-theory]], [[golovin-2011-adaptive-submodularity-theory]], [[madsen-2017-strategic-advantages-micro]]

## Flip - winnability model

**Computes:** pflip = a/(a+b), where a = WM pm (c cu+1)/((c cu+1)+HALFF R) + WP nA/mAt and b is the same with industry money and anti neighbours. flip_value = sum tgt x pflip, with tgt = engagement x PERS x (1+0.3 bpf) x delivery_mult.

**Method:** Saturating money term on receipts R (floored at $200k; zero imputed to median). nA = pro-neighbour weight x cpf_pressure. WM 0.35, WP 0.65, HALFF 0.10. A comment cites a Volden-Wiseman passage bonus but the code line has none.

**Code refs:** `fec/engine/flip_model.py:1-147`, `fec/engine/money_observation.py:4-13`

**Assumptions:**
- Money buys access more than votes (Ansolabehere; Hall & Wayman; Kalla & Broockman)
- Peer proximity outweighs money
- Money saturates relative to receipts
- Flip-side competitive LT has no submodularity guarantee (Borodin et al.)

**Evidence (15 rows; 15 distinct papers; 5 VERIFIED, 9 VERIFIED-SOFTENED, 1 UNVERIFIED):**

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
| [[alexander-2009-lobbying-analysis-under]] |  | context | Lobbying firms repatriated about 70% of dollars: 209.1bn of 298.36bn (Table 6; the introduction says 208bn). Table 5 has no lobbying regressor, so the only lobbying test is the group comparison in Table 6. The introduction promises a regression with lobbying, and no such regression appears in the text. | Does not bear on WM, WP or HALFF. The paper measures a firm benefit, not member movement, and measures lobbying rather than PAC giving. Furnas (item furnas-nd-conflating-lobbying-pacs) finds only a small minority of organizations do both. The paper gives no basis for raising the money weight. The clause about Paul's ROI enthusiasm has no support in the item file and is dropped. | weak | VERIFIED-SOFTENED |
| [[cai-2025-competitive-influence-maximization]] | 2025 | qualifies | Vote share at a finite horizon depends on when each controller starts and on degree heterogeneity. At T=16 (dimensionless model time, unit-weight graphs) the degree of equilibrium l never exceeds 0.82 and high-degree nodes relax more slowly. An optimized start beats t_a=0 in all 30 rows of Table 2 (5 real networks, T in {16, 256}, budget ratio in {0.1, 1, 10}). Congress votes network, T=16, budget ratio 1: S_A 0.705 vs 0.500 (model output, not observed votes). | Structural parallel only: both are ratios of competing pulls. Magnitudes do not transfer, since paper time is dimensionless, edges have unit weight and opinions are binary simple contagion. The paper shows that an equilibrium-share form omits timing, not that pflip is miscalibrated. | moderate | VERIFIED |
| [[moreno-2021-shadowing-shielding-effective]] | 2021 | context | Steady-state opinions solve (L + Wa + Wb) x = wa for the voter model with zealots (Eq 1 rate equation on p.3; Eq 2 vote share on p.4). In the high-allocation limit (w much larger than d) the optimal allocation has no degree dependence (Eqs 7-8, pp.7-8). | The functional form is analogous (a share-of-pull ratio), but the paper's neighbour states are endogenous while Melange fixes them from stance, so the paper's HMF results do not transfer. Read pflip only as a vote-share proxy, never as a vote or access probability (D-025). The analogy itself is an inference (DE-062). | weak | VERIFIED |
| [[anon-2025-cross-industry-political]] | 2025 | qualifies | Bank CEOs give little directly to candidates; several, including Moynihan and Masrani, gave nothing across recent cycles. Bank money flows mainly through company PACs and the ABA (cyclical average $2,402,499, memo wording). Mutual-fund Super PAC spending (about $261M annual average to the four biggest Super PACs; industry PACs about $13M cumulative; memo figures, no source or window) is described as channeled by the industry. CU giving runs through ACU coordination, with $0 dark money to the four biggest Super PACs (memo wording). | Structural caution only. The memo's dollar figures have no source, window, N or denominator, so they cannot calibrate WM, HALFF or the b-term. Before any engine change, Mentat's trade-association and Super PAC columns would need mapping to members. | weak | VERIFIED-SOFTENED |

**Assessment:** **Evidence weight: moderate on the direction 'money is the minority channel', contested on 'money buys access', nil on WM/WP/HALFF.** 15 rows: 5 VERIFIED, 9 VERIFIED-SOFTENED, 1 UNVERIFIED.

**Supported direction.**
- Bombardini & Trebbi review: Kang's structural estimate puts lobbying's marginal enactment effect at 0.021%-0.415% against an 8% baseline (538 provisions in 445 bills, 110th Congress, energy). This is lobbying spend against enactment, not PAC money against member flips.
- Jacoby: donations move 1 of 435 House members in a simulation of one bill (ACESA, 2009). This is a model output from literature parameters, not an estimate.
- Holyoke (UNVERIFIED) agrees but gives no figure.

**Access premise is contested.**
- Kalla & Broockman (strong): a donor label raised senior-staff access, for example chief of staff or above at 12.5% vs 2.4% (191 offices of one party, June-Aug 2013). There is no estimate on votes or positions, so the paper supports the access step only.
- Furnas 2019 (VERIFIED): staff report **no** meeting premium for donors (N=441, 2017, stated intentions; social desirability possible).
- The code comment 'money buys access/effort > votes' therefore rests on one field experiment on one side and a staff survey contradicting it on the other.

**Labelling problem.** WM 0.35 / WP 0.65 is labelled CALIBRATED. No row calibrates it, and Bonica, Kalla and Bombardini each explicitly say they cannot. Bonica also finds corporate and trade PAC money mixed-motive (investor and ideological forms nearly tied), so these dollars cannot be treated as purely expressive. A code comment cites a Volden-Wiseman passage bonus that the code does not implement.

**Omissions the literature points to.**
1. Timing. Kim et al. 2020/2023 find lobbying links follow first donations immediately and then decay, while pflip uses static cycle-level money. Kim 2023's amount effect is small, about 0.03 pp per 10%, and non-causal.
2. Network spillover. Battaglini's phi of 0.21 (committee) and 0.08 (alumni) is autocorrelation in contributions, not a demonstrated vote spillover. Any spillover term needs its own prereg look.
3. Timing of competition. Cai and Moreno: an equilibrium-share form omits start times.

**Out of bounds.** Huneeus & Kim show lobbying spend is more than six times PAC and party giving (FEC cycles 1999-2016). Under D-021 the absence of LDA dollars is deliberate. The same applies to Jacoby's lobbying result.

**Contradictions:**
- The access half of the 'money buys access/effort > votes' comment: Kalla & Broockman (field experiment, 191 offices, 2013) find a donor access premium, while Furnas 2019 (staff survey, N=441, 2017) finds none and finds Democratic staff less likely to meet donors.
- WM 0.35 / WP 0.65 is labelled CALIBRATED in code, but every cited source says it provides no calibration.
- A code comment cites a Volden-Wiseman passage bonus that the formula does not contain.

**Gaps:**
- Money-to-alignment. Needed: a regression of later CU roll-call or cosponsorship alignment on relative CU vs industry money as a share of receipts, controlling for party and peer exposure. It must handle strategic targeting (Blackwell) and report party strata.
- Peer vs money weight ratio. Needed: a joint hazard model of cosponsorship with peer exposure and recent contributions entered together.
- Concavity. Needed: a test of whether contribution effects saturate in the contribution-to-receipts ratio, compared with linear and log forms.
- Timing. Needed: an onset or recency term tested against static levels (Kim lead).

**Paul's position vs the evidence:** Paul believes money buys access and returns, but he highlighted Jacoby's result that lobbying sways undecided members more than donations. On votes, the conservative model agrees with his sources. On access, his position is weaker than he may assume: Furnas 2019 contradicts the access premium Kalla & Broockman found. The verifier could not find support in Paul's item notes for the hub's characterisation of his 'ROI enthusiasm' (Alexander row), so that framing should not be leaned on. Paul's Bonica margin note asking for short-term engagement and sunk costs lines up with the Kim timing evidence that the static money term ignores.

**Open questions:**
- Should the CALIBRATED label on WM/WP be relabelled as owner judgment until an alignment regression exists?
- Should the uncoded Volden-Wiseman comment be removed or implemented? That is Paul's call.
- Is the access premise needed at all, given that pflip is a vote-share proxy and not an access probability (D-025)?

**Read first:** [[kalla-nd-campaign-contributions-facilitate]], [[furnas-2019-moneyed-interests-information]], [[bombardini-2020-empirical-models-lobbying]], [[bonica-2013-mapping-ideological-marketplace]], [[kim-2020-strategic-sequential-links]], [[battaglini-2016-influencing-connected-legislators]]

## Persuadability table PERS

**Computes:** Topic target multiplier by stance tier: Champion 0.10 ... Highly Persuadable 0.65 ... Opponent 0.08; no position PERS_NONE 0.45.

**Method:** Fixed design table. PERS_NONE was ratified 2026-07-02 at Ally parity. The no-position default drives about 60% of cells (open M5).

**Code refs:** `fec/moneyball_core.py:564-568`

**Assumptions:**
- Persuadability is inverted-U along the stance ladder
- Silence equals uncertainty, not enthusiasm

**Evidence (6 rows; 6 distinct papers; 2 VERIFIED, 4 VERIFIED-SOFTENED):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[broockman-2026-should-moving-middle]] | 2026 | qualifies | Moving on an issue is rewarded only where the audience disagrees with the current position, and it is penalized where the audience holds that position (Republican candidate, own=R -0.0364 vs own=D +0.0404, Table A8). | This is an analogy only. The paper studies voters choosing between candidates, not legislators being lobbied. It gives no evidence on legislator persuadability or on the shape of the PERS curve, and it should not be read as calibrating PERS values. Its one structural point, that the target of an appeal is the issue-level disagreement and not the aggregate midpoint, is consistent with PERS being applied per topic. | weak | VERIFIED |
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | qualifies | The third coalition's median ideology shifts across sessions: very liberal (98th-102nd), center-left (103rd, 111th), center-right (105th-110th). Its members are effective. The paper does not measure persuadability, vote change or electoral vulnerability. | The paper gives no support for the PERS values or for treating third-coalition or Fitzpatrick-type members as more movable. Coalition-level ideological fluidity describes session-to-session composition, not the responsiveness of any individual member. | weak | VERIFIED |
| [[jacoby-2025-mathematical-modeling-analysis]] | 2025 | qualifies | Only undecided centrists (C) move. Committed Y and N legislators are absorbing states, and the equilibrium with all members undecided is unstable. Widening the centrist band on a -1 to 1 ideology scale from ±0.25 to ±0.35 reverses the House result so that no votes exceed yes votes. | The paper supports the extremes being near-fixed only because its model assumes it; Y and N are absorbing by construction. It does not test an inverted-U. Its centrist group is defined by an unnamed ideology scale, while PERS is defined by CU stance tier, so the band result carries over only as a warning: how the movable set is defined can decide the outcome. The system's PERS_NONE sweep shows the same thing in the system itself. Evidence is a single bill (ACESA, 2009) at the federal level. | weak | VERIFIED-SOFTENED |
| [[cai-2025-competitive-influence-maximization]] | 2025 | supports | In the individual-optimization runs with 20 percent of agents as partial zealots (Sec 4.3.3, N=100 per the Sec 4.3.2 reduction; N not restated in 4.3.3), normal agents receive more budget than zealots at every zealotry level q tested. At q=0.1 the maximum zealot allocation is under 20 and the normal-agent maximum is near 25 (allocation units as plotted in Fig 7). At medium and large q zealots get fewer resources and start later as degree rises. | The direction matches for resistant tiers (Opponent and Highly Skeptical). Champion is a committed pro tier, so its low multiplier reflects being already won, not zealot resistance, and it does not support the paper's result. The paper never sets zealot allocation to zero, so it does not support the hard zero gate on its own. Simulated agents only. | weak | VERIFIED-SOFTENED |
| [[tappin-2023-quantifying-potential-persuasive]] | 2023 | context | In the Study 2 calibration (n=3,990 respondents, 36 issues, 48 progressive video ads), the messages predicted most persuasive were often on obscure, low-information policies such as net neutrality. The authors offer this as one possible explanation for the Study 2 divergence, alongside a mixed pretest design and a nearly one-year gap before follow-up. They expect smaller effects in high-salience settings with countermessaging. | Weak. Tappin measures attitude change among public respondents after one video, while PERS models legislators under lobbying. The low-information point might carry over to members on low-visibility CU topics, but nothing here measures that. | weak | VERIFIED-SOFTENED |
| [[sverdrup-nd-introduction-grf-causal-forests]] |  | qualifies | Best linear projection tests whether effects vary along a chosen covariate: financial.autonomy.index coefficient -0.0329 (SE 0.0128, p = 0.0099). Other terms: is.female p = 0.17, family cash transfer p = 0.066, is.unemployed p = 0.61. Heterogeneity can be hard to detect at moderate n (payday AUTOC t = 0.67, about 2,500 individuals; text says possibly underpowered). | A BLP of contact effect on stance-tier ordinals needs logged contact and tier movement; the payday example suggests a sample of 533 members may be too small for tier-level heterogeneity. A null would not refute the table. | weak | VERIFIED-SOFTENED |

**Assessment:** **Evidence weight: weak. The inverted-U is untested anywhere in the set.** 6 rows: 2 VERIFIED, 4 VERIFIED-SOFTENED.

- **Jacoby:** only centrists move, but that is **assumed** (Y and N are absorbing states by construction), not found. Its useful lesson is a warning: widening the centrist band from ±0.25 to ±0.35 reverses the House outcome (simulation of one bill, ACESA 2009). How the movable set is defined can decide results. The system's own PERS_NONE sweep shows the same sensitivity, and PERS_NONE drives about 60% of cells (59.8% per the flip_model comment).
- **Cai:** simulated optimisers give zealots less budget. That matches the low Opponent and Highly Skeptical values. It does not justify Champion's 0.10, which reflects being already won rather than resistance, and it never zeroes zealots, so it does not support the hard F1 zero gate.
- **Aref & Neal:** gives **no** support for treating third-coalition or Fitzpatrick-type members as more movable. Coalition fluidity is session-level composition, not individual responsiveness.
- **Broockman 2026 and Tappin:** voter-level analogies only.
- **Sverdrup (GRF):** power warning. Heterogeneity in contact effects across tiers may be undetectable with 533 members.

**Contradictions:**
- (none)

**Gaps:**
- Inverted-U shape. Needed: a panel of stance-tier transitions on CU issues after a dated contact or salience event, estimating movement probability by prior tier (best linear projection or causal forest), with power analysis first.
- PERS_NONE (about 60% of cells). Needed: a test of whether silent members later move toward a position more or less often than tiered members, to decide whether silence means uncertainty or indifference.

**Paul's position vs the evidence:** Paul reads ideologically fluid, competitive-seat members and centrists as the most movable (for example Fitzpatrick). The table's inverted-U is consistent with that, but the evidence does not test it. Aref & Neal, the source behind the Fitzpatrick reading, explicitly does not measure individual persuadability, and Jacoby's centrist result is an assumption of the model. Paul's position is untested, which the hub already notes; the literature adds no confirmation.

**Open questions:**
- Is a sample of 533 members powered to detect tier-level heterogeneity in movement at all, or should tiers be pooled into three bands for any test?
- Is the F1 hard zero gate for Skeptic, Highly Skeptical and Opponent members intended as policy rather than as a persuadability claim?

**Read first:** [[jacoby-2025-mathematical-modeling-analysis]], [[sverdrup-nd-introduction-grf-causal-forests]], [[cai-2025-competitive-influence-maximization]], [[aref-2021-identifying-hidden-coalitions]]

## Money response (lean shift) and money-triggered defection

**Computes:** shift = MAXSHIFT x (r/(r+HALF)) x pressure_mult, with r = c x amount/receipts; competitor money is multiplied by LOSSFRAME 1.10. Defection moves floor(|shift|/DTHRESH) rungs; Champion and Opponent are immune.

**Method:** MAXSHIFT 18 (cut from 40), HALF 0.10, DTHRESH 11.25. Crossing one rung takes roughly a sixth of receipts.

**Code refs:** `fec/engine/money_response.py:15-90`

**Assumptions:**
- Money moves positions little (null vote-buying literature)
- Threats weigh more than gifts (loss aversion, elite-attenuated)

**Evidence (5 rows; 5 distinct papers; 3 VERIFIED, 2 VERIFIED-SOFTENED):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[linde-2016-politicians-take-risks]] | 2016 | contradicts | Loss aversion was NOT found in the MP sample (n=46) nor in the student sample; the authors suspect the loss-aversion tasks lack validity (participants may treat the endowment as house money, fn 4). The abstract's claim that MPs' deviations are 'somewhat smaller than those of other people' is not a formal statistical test in the main text: it rests on the authors' student comparison sample (n=176, Appendix E, not in the file; e.g. students showed the common ratio effect, MPs did not, p.106) plus prior literature. | The paper is no basis for LOSSFRAME. (1) It reports no loss aversion in politicians, so the citation runs against the dial. (2) Its loss frame concerns outcomes for the decision-maker relative to a reference point; a competitor PAC donation to a member is not a loss to that member, so the 'threat frame' is a separate untested assumption. (3) 1.10 is not derived from any number in the paper. LOSSFRAME stands as owner judgment and should be cited as such. | strong | VERIFIED-SOFTENED |
| [[poole-1999-nominate-after-years]] | 1999 | supports | Across Congresses 1-105, a constant ideal point per legislator classifies almost as well as a linear one (about 0.3 percent difference); linear change captured most of the relatively small changes in positions. | Indirect: the paper measures general-ideology stability with no money treatment, so it supports only a low prior on position change, not any MAXSHIFT or DTHRESH value. | weak | VERIFIED |
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | supports | Structural work finds a very small marginal enactment effect of lobbying (Kang 0.021%-0.415%) despite large stakes, and campaign contributions are low with federal donation caps very seldom binding (Ansolabehere et al. 2003, as reported). | The review addresses enactment and bill outcomes, not shifts in individual member stance. It supports rare money-triggered defection but not the LOSSFRAME 1.10 asymmetry or the specific constants. | weak | VERIFIED |
| [[jacoby-2025-mathematical-modeling-analysis]] | 2025 | supports | Campaign donations alone move one House member out of 435 (from 9 undecided to 8, 230 to 231 yes). The Senate is reported only for the combined lobbying-plus-donations run, which changes it by one senator (52/47/1 to 52/48/0). The authors note that donations may reflect ideology rather than buy influence (citing Bonica [5]). | This is a consistency check, not a calibration. The donation effect in the paper is a model output from literature parameters (I_F, I_A taken from Meng and Rode), not an observed shift. It is weaker than the experimental and null-result literature already cited at money_response.py:15. | weak | VERIFIED-SOFTENED |
| [[kalla-nd-campaign-contributions-facilitate]] |  | qualifies | The measured outcome was access: the rank of the official who attended. Positions and votes were not measured. Cosponsorship was nine legislators in total, with equal shares in both arms, and the authors say it tells little about effects. | The paper can be cited for the access measure, but not for a null on positions: its cosponsorship estimate is uninformative rather than null. It says nothing about MAXSHIFT, HALF, DTHRESH or LOSSFRAME. The not-votes half of the premise lies outside the paper (p.555). | weak | VERIFIED |

**Assessment:** **Evidence weight: moderate support for a low ceiling on money-driven position change; a strong contradiction on LOSSFRAME.** 5 rows: 3 VERIFIED, 2 VERIFIED-SOFTENED.

**Supported.** The MAXSHIFT cut from 40 to 18 has a direction-only basis:
- Poole: constant ideal points classify almost as well as linear ones, about 0.3 percentage points worse across Congresses 1-105.
- Bombardini: small marginal lobbying effects; federal donation caps rarely bind.
- Jacoby: 1 of 435 members moves in a simulation.
- None sets 18, HALF 0.10 or DTHRESH 11.25. Kalla & Broockman's cosponsorship result is uninformative (nine cosponsors in total), not a null, so it cannot be cited for 'not positions'.

**Contradicted.** LOSSFRAME 1.10 cites 'K&T loss aversion, elite-attenuated per Linde-Vis', and audit_full locks it in as by design. But:
1. Linde & Vis found **no** loss aversion in MPs (n=46, Dutch, 2014-15) or in students, and suspect their loss-aversion task lacked validity.
2. A competitor PAC donation to a member is not a loss to that member, so the 'threat frame' is a separate, untested assumption.
3. 1.10 is not derived from any number in the paper.

The defensible label is owner judgment.

**Contradictions:**
- LOSSFRAME 1.10 is justified as elite-attenuated loss aversion per Linde & Vis, but Linde & Vis report no loss aversion in politicians (n=46). The citation runs against the dial.
- MAXSHIFT is labelled CALIBRATED, but no source sets its value. The cut from 40 to 18 rests on direction only.

**Gaps:**
- Competitor-spend response. Needed: an event study around large competitor PAC or Super PAC spends (FEC-dated), testing for subsequent press-stance or roll-call shifts on CU issues against matched non-targeted members.
- Loss/gain asymmetry for legislators. No elite study estimates it. Needed: a comparison of response to competitor spending against equal CU spending in the same event-study frame.
- Defection mechanics. Needed: a census of observed rung-crossing stance changes and what preceded them.

**Paul's position vs the evidence:** Paul's highlights support the reflection effect and loss framing in MPs, weaker than in the public. The evidence separates the two. Linde & Vis do find a reflection effect (paid lottery: risky choice 39.13% under gain vs 76.09% under loss, n=46), but they find no loss aversion, and LOSSFRAME encodes loss aversion. Paul's reading has merged the two constructs. His notes say nothing on defection, and neither does the literature.

**Open questions:**
- Should LOSSFRAME's citation be replaced by an explicit owner-judgment label, and should the audit's 'by design' lock reference that label?
- Is a competitor donation to a member meant to model a threat (a challenger-funding signal) rather than a loss? If so, which literature backs a threat effect?

**Read first:** [[linde-2016-politicians-take-risks]], [[kalla-nd-campaign-contributions-facilitate]], [[bombardini-2020-empirical-models-lobbying]], [[poole-1999-nominate-after-years]]

## Linear-threshold priming (cheapest tip)

**Computes:** For each target, the cheapest mix of recruiting pro neighbours and money that closes theta - pressure.

**Method:** theta = tier threshold (0.6 ... 3.0) + max(0, -lean/120) + 0.25 per Opponent cell. Cross-party cue factor sqrt(bpf_u bpf_v) (Bakker-Lelkes-Malka). Recruit cost = CEDGE x 0.5 / PERS[tier]. Money closes the gap only up to 0.97 pm units.

**Code refs:** `fec/engine/lt_priming.py:11-145`

**Assumptions:**
- Linear-threshold adoption over summed neighbour pressure
- Out-party cues do not persuade

**Evidence (9 rows; 9 distinct papers; 4 VERIFIED, 5 VERIFIED-SOFTENED):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[furnas-2019-moneyed-interests-information]] | 2019 | supports | Evidence from the other party's think tank lowers the chance that staff side with the requester (a penalty about twice the aligned benefit). Republican staff also rate presenters of center-left evidence as less representative of their district. | Directionally, this backs a non-positive out-party cue, and it hints that a negative (backfire) term could be defended. The evidence is about outside information sources judged by staff, not Member-to-Member cosponsor cues, and it is stated intention from an underpowered design. It does not justify inventing a backfire magnitude. Keeping the clamp at 0 remains the conservative reading. | weak | VERIFIED |
| [[ma-2022-adversarial-attack-graph]] | 2022 | qualifies | Proposition 2: if each threshold CDF F_j is concave on [0, +inf), the expected spread h(S) is submodular, so greedy selection applies. Corollary 1 (uniform thresholds, Eq. 6) gives a saturated coverage objective that is submodular. Lemma 1: influence maximization on the directed bipartite linear threshold graph is NP-hard, by reduction from Set Cover with thresholds alpha/2. | The concavity result transfers as mathematics only. Lemma 1 is an NP-hardness result for deterministic thresholds; the paper does not state a converse that missing concavity implies hardness. A point-mass (step) threshold CDF is not concave on [0, +inf), so the paper's submodularity guarantee does not cover lt_priming's deterministic thresholds if they were aggregated into a seed-set objective. Moderate for the mathematics, weak for empirical relevance. | moderate | VERIFIED-SOFTENED |
| [[bakker-nd-understanding-partisan-receptivity]] |  | qualifies | Out-party cues lower policy support by about .07 on a 0-1 scale in the pooled studies 1-4 analysis, relative to the no-cue control. The paper does not use the word backfire; the decrease is the reported effect. Heterogeneity runs through partisan identity strength interacted with cognitive resources (NfC, latent resources). The paper measures no bipartisan-bridge or cross-party attribute. The authors say real messages often carry both in-party and out-party cues. | Sign only. The paper supports shrinking out-party cross-cues toward zero, which the bpf scaling does. The paper does not support the bridge exemption, under which sqrt(bpf) restores full cross-party influence for perfect bridge pairs, because bipartisanship is never measured. Mass-public one-shot survey effects do not transfer to elite cosponsorship recruitment without elite-level evidence. | moderate | VERIFIED-SOFTENED |
| [[golovin-2011-adaptive-submodularity-theory]] | 2011 | context | Thm 9 is a worst-case hardness statement for general adaptive stochastic maximization: no polynomial algorithm within O(\|E\|^{1-eps/beta}) unless PH = Sigma2P, even for pointwise linear f. | Thm 9 says hard instances exist in the general class. It does not establish hardness for the cosponsor graph. The rel's claim that threshold adoption is not submodular in general is not from this paper and was not checked here. | weak | VERIFIED-SOFTENED |
| [[ringe-2013-pinpointing-powerful-voting]] |  | supports | Social connection between political adversaries goes with less voting agreement, not more. Theory predicts this when the pair expects to disagree (H1, Fig. 1). Opposed-bloc baseline co-voting is about 50 percent (p. 626). | Direction agrees: out-party ties should not count as pro-pressure. The association is not causal and is EP staff contact, so it does not justify adding a negative backfire term. Keeping the floor at 0 remains the default. | moderate | VERIFIED |
| [[borodin-nd-threshold-models-competitive]] |  | suggests-extension | In the separated-threshold competitive LT model a node is activated by whichever technology weighted active neighbours first exceed that technology own threshold. Adding A seeds never raises the probability that B propagates (Lemma 3 part 1, used in Theorem 3), but the process is not submodular in general (Theorem 4, counterexample only in an unincluded full version). | Thresholds here are not normalised to Borodin uniform [0,1] draws (base ladder 0.0 to 3.0 plus additive terms). Adding an anti-side threshold would be a model change for Paul to decide. Borodin monotonicity result is for the separated-threshold model and says nothing about this solver recruit lists, for which the paper gives no guarantee. | weak | VERIFIED-SOFTENED |
| [[merkley-nd-party-cues-news]] |  | supports | Out-group (Democratic) elite cues move Republicans away from the cued position rather than toward it. Experiment: strong Republicans +0.06 toward skepticism (about 0.2 SD, p~0.03), with a party-ID interaction p~0.05. Time series: Democratic cues Granger-cause Republican skepticism (chi2 7.35, p=0.03, 2 lags); the reverse has p=0.86. | The paper supports that out-party cues do not persuade and reads as backlash, so a negative sign is consistent with it. But the +0.06 effect is measured on mass Republicans and one climate item, not on legislators reading cosponsorship signals, so it cannot set a backfire magnitude for the priming term. Keeping the zero clamp remains defensible. | moderate | VERIFIED |
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | context | Table 7, Model 7.1 gives 0.015 (SE 0.002) and Model 7.2 gives 0.012 (SE 0.002) for female speeches x female pitch on the share of male votes cast with female members. At +2 SD pitch (1.41 on the paper's scale), that share moves from 0.23 SD below to 0.53 SD above as female speeches go from 0 to 43 (Figure 3, p. 957). The authors say the data are observational and do not claim causation (p. 956-957). Vote timing is unknown (fn 31). | Weak and gender-specific: women's issues, 2009-2014, floor votes with no timestamps, not cosponsorship. The paper does not test a threshold mechanism, so it cannot set any engine constant. | weak | VERIFIED |
| [[chen-2023-complex-contagion-influence]] |  | qualifies | K-complex model (Eq. 1): activation probability p0 when at least K neighbours are active and p1 much smaller otherwise. It is motivated by Centola and Macy 2007 and Centola 2010 but not validated on field adoption data in this paper; the paper cites succeeding studies as support. | The paper supports only the modelling option that adoption can be threshold-like. It gives no K or threshold values for legislators. Joint multi-target priming would be a new model needing owner authorisation and a holdout look. | weak | VERIFIED-SOFTENED |

**Assessment:** **Evidence weight: moderate on the out-party clamp, weak on threshold adoption itself.** 9 rows: 4 VERIFIED, 5 VERIFIED-SOFTENED.

**Out-party cues.** This is the best-evidenced choice in the engine, and every cue study in the set agrees that out-party cues do not persuade:
- Bakker: about -.07 on a 0-1 scale, pooled.
- Merkley: strong Republicans move +0.06 toward skepticism after a Democratic cue (about 0.2 SD, p≈0.03), with Granger evidence from the time series.
- Furnas 2019: the non-aligned penalty is about twice the aligned gain.
- ringe-2013-pinpointing-powerful-voting: connectivity among adversaries goes with less co-voting.

Clamping non-bridge cross-party cues to about 0, and **not** inventing a backfire magnitude, is the conservative reading every row endorses. None of these papers measures member-to-member cosponsorship cues.

**Bridge exemption.** sqrt(bpf_u bpf_v) restores full cross-party influence for perfect bridge pairs. No paper measures a bipartisanship attribute, so the exemption is unsupported.

**Threshold mechanism.**
- Chen (complex contagion) supports threshold-like adoption as a modelling option, with no K for legislators.
- Ma: deterministic step thresholds have non-concave CDFs, so there is no submodularity guarantee if the per-target solves were aggregated into a seed-set objective.
- Borodin: separated-threshold competitive LT is not submodular in general.
- The solver does not invoke submodularity (cheapest recruits per target, one hop), so these are cautions, not defects.
- No anti-side threshold pressure exists. Adding one would be a model change for Paul to decide.
- Dietrich (vocal pitch) is context only.

**Contradictions:**
- (none)

**Gaps:**
- Threshold adoption on CU bills. Needed: a hazard model of a member joining a CU bill as a function of the count and share of close cosponsor partners already joined, compared with a linear-exposure (IC-like) specification. Homophily must be controlled (Brandenberger/Russo-style), because shared preferences mimic thresholds.
- Out-party endorsements. Needed: the same model with exposure split by party of the joined partner, testing whether out-party joins add zero, positive or negative hazard. This is the elite-level evidence every row says is missing.
- THETA ladder values (0.6-3.0) and the lean/120 term: no source. They could be estimated from the same hazard model.

**Paul's position vs the evidence:** Paul hypothesizes that coalition formation is complex contagion, and his Chen margin note says pressure from high-ranking members is 'not diminishing', which challenges submodularity. The theory rows agree that submodularity is not guaranteed under thresholds (Borodin, Ma, Chen), so his scepticism is well placed. The current solver sidesteps it by not optimizing a seed set. No row tests complex contagion or seniority-weighted pressure on legislators, so his hypothesis remains a hypothesis. The kernel has no seniority or leadership term (Chen row), so his 'high-ranking' point is not represented anywhere.

**Open questions:**
- Should the out-party treatment here (clamp to about 0 for non-bridges) be the reference for the kernel's cpf_pressure and the salience rally step, which currently keep positive out-party pull?
- Is a seniority or leadership term warranted for priming pressure, per Paul's Chen margin note? It would be a new model needing owner authorisation.

**Read first:** [[bakker-nd-understanding-partisan-receptivity]], [[merkley-nd-party-cues-news]], [[ringe-2013-pinpointing-powerful-voting]], [[chen-2023-complex-contagion-influence]], [[ma-2022-adversarial-attack-graph]], [[borodin-nd-threshold-models-competitive]]

## Salience shock (Noble-Turner going public)

**Computes:** Rally, retreat and backfire probabilities per issue, member steps, and a go-public advisory.

**Method:** z = min(1, |D-R tier gap|/3); psi = uncertainty; b = min(0.35, 0.05 + 0.10 psi); p = (1-b)(1-z), q = (1-b)z. Rally: co-partisan +1, out-partisan +0.5 if bpf >= 0.30. Retreat: co +1, out -1. Vulnerable members x1.35 x susc_mult. Defensive appeal raises the threshold by (1 + DELTA/2).

**Code refs:** `fec/engine/salience_shock.py:1-246`, `fec/moneyball_core.py:731-751`

**Assumptions:**
- Co-partisans move more than out-partisans under salience (Noble & Turner 2024)
- Backfire grows with policy uncertainty
- Electorally vulnerable members respond more

**Evidence (11 rows; 6 distinct papers; 4 VERIFIED, 5 VERIFIED-SOFTENED, 2 UNVERIFIED):**

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
| [[vanderdoes-2022-strategic-identity-signaling]] |  | suggests-extension | The calibrated payoff model makes outgroup share of the audience the main driver of whether covert beats overt signaling, with cost a weaker driver, and covert messaging only reduces, not removes, outgroup dislikes. | The paper's audience is a tweet reader pool, not the Noble-Turner public-campaign setting. An outgroup-share term would need its own calibration on congressional data, and the paper's r_o = 0.5 cannot be ported as a parameter. | weak | UNVERIFIED |

**Assessment:** **Evidence weight: strong as an internal-consistency check against Noble & Turner, and that check fails in several places. Weak as empirical support.** 11 rows: 4 VERIFIED, 5 VERIFIED-SOFTENED, 2 UNVERIFIED.

**Headline contradiction (strong).** Noble & Turner say going public pays more when **defending** a favourable status quo. Under Partisan Retreat it never pays against an unfavourable one. The code instead adds defensive-appeal credit (held counts) on offense issues too. In the live `salience_advisory.json` (2026-10-08 15:37, denominator 18 issues):
- all four GO PUBLIC verdicts (top quartile, cut 68.27) are sq=offense and are driven by held counts;
- Technology & Emerging, close to the paper's never-go-public case, ranks first;
- the three defend issues rank 13th-15th.

These are heuristic advisory scores, not measured effects. The dashboard surfaces them as 'GO PUBLIC · holds N'. Gating the held credit to sq=='defend' would restore the paper's ordering.

**Misattributions.**
- psi is coded as stance-data **missingness**, not preference uncertainty. Coverage differs by party and chamber.
- Backfire b = min(0.35, 0.05+0.10 psi) is labelled 'Prop 5 caution', but the paper never links psi to backfire. The real Prop 5 damping sits behind SAL_PSI_DAMP, which defaults to 0.
- VULN 1.35 cites Noble A.4, but A.4's gambling-for-reelection logic concerns the **president**, not rank-and-file members. Prop 6 is also internally inconsistent on the direction of rho.
- gridlock_width (occupancy of a five-rung mid-band) is a different quantity from the paper's gridlock interval, and Prop 1 predicts nothing about its sign.
- z (the elite D-R tier gap) sets the mass-base reaction odds. That is a design choice the paper's own infrastructure example warns against.

**Regime steps.**
- Retreat (out-partisan -1) is directionally supported by mass-level repulsion (Bakker; Merkley's polarized cue, +0.09 for strong Republicans).
- Rally's out-partisan +0.5 when bpf ≥ 0.30 is **not** supported. Merkley's consensus cue shows no effect and Bakker shows no positive out-party movement.
- The regime structure transfers best to the dashboard's executive cue (a presidential actor). It transfers worse to a CU or industry campaign actor, which has no shared electoral fate (delta).

**Other rows.** Joo: auto intensity (0.5 + alignment) has no estimate behind it and does not reach the chain's advisory, because the chain runs with intensity 1.0. Zhang (burst detector) and anon-nd (outgroup audience share) are UNVERIFIED extension leads.

**Contradictions:**
- Defensive-appeal credit on offense issues contradicts Noble & Turner's status-quo asymmetry (Prop 2; Partisan Retreat never goes public against an unfavourable status quo). In the live 2026-10-08 advisory (18 issues), all four GO PUBLIC verdicts are offense issues, led by Technology & Emerging.
- The b-psi backfire link is labelled 'Prop 5 caution', but Prop 5 concerns damping the value of going public and never links psi to backfire. The actual Prop 5 damping is off by default (SAL_PSI_DAMP=0).
- VULN 1.35 cites Noble A.4, which concerns the president gambling for reelection, not rank-and-file legislators' responsiveness.
- Rally out-partisan +0.5 (bpf ≥ 0.30) conflicts with Merkley's consensus-cue null and Bakker's absence of positive out-party movement.

**Gaps:**
- Empirical regime test. Needed: an event study after dated public CU campaigns, comparing co-partisan and out-partisan stance shifts of the campaign's champions, and recording whether the status quo was favourable.
- Base-side opinion per issue. Needed: a partisan gap in public or CU-member opinion by issue, to replace the elite-gap proxy for base reactions.
- Vulnerable-member amplification. Needed: evidence from the electoral-incentives literature (not Noble-Turner) on whether vulnerable members respond more to salience.

**Paul's position vs the evidence:** Paul highlighted Noble & Turner heavily (rally, retreat, backfire, defensive appeals) without annotating, and the hub says the component implements the model 'almost directly'. The evidence says otherwise. The component departs from the paper at its central prediction (the defend/offense asymmetry) and attributes three constructs to the paper that the paper does not contain (psi as missingness driving backfire, VULN, gridlock-width reading). Paul's reading of the paper is sound; the code's reading is not.

**Open questions:**
- Should held-count credit be gated to sq=='defend' issues? That restores the paper's ordering and would change the live advisory. Paul's call.
- Should the advisory rely on stance-data missingness as psi at all, given that coverage differs by party and chamber?
- Is going public by a CU-side actor, with no shared electoral fate, a valid application of the delta channel?

**Read first:** [[noble-2024-presidential-leadership-legislative]], [[merkley-nd-party-cues-news]], [[bakker-nd-understanding-partisan-receptivity]], [[joo-2022-image-data-automated]]

## Receptivity layer (susceptibility, incentive, prospect domain, seed probability)

**Computes:** Per member: susc, susc_mult [0.70, 1.30], edge_mult [0.80, 1.20], pressure_mult = clip(susc x incent x pt, 0.50, 1.60), and seed_prob = clip(0.60 + 0.10 coop + 0.06 breadth + 0.04 visibility, 0.30, 0.90). The rank is not affected.

**Method:** Behavioural basis (default): susc = .40 cooperation_v2 + .30 N(affective) - .20 diligence - .10 dominance, blended 0.70/0.30 with persona when n >= 15 cues. Lexical basis: .40A + .30N - .20C - .10E. incent: retiring 0.60, else clip(1 + .30(comp - 0.5), 0.85, 1.15), neutral when COMPET_MULT is on. pt: 1.15 loss domain, 0.92 safe. PERSONA_W 0.30 is kept by owner ruling. Recorded double-count: cooperation_v2 vs bpf rho +0.888.

**Code refs:** `fec/members/receptivity_layer.py:1-216`, `fec/moneyball_core.py:185-213 (rc)`

**Assumptions:**
- Neuroticism degrades decisions under pressure (Byrne et al. 2015); the A, C and E signs are design priors, not Byrne findings
- Final-term members are less responsive (Fouirnaies & Hall)
- Members in a live race are in the loss domain and take risks (Linde & Vis 2016)
- No N susceptibility signal has been demonstrated (Nokken-Poole drift MISS; H5 null)

**Evidence (22 rows; 19 distinct papers; 4 VERIFIED, 11 VERIFIED-SOFTENED, 7 UNVERIFIED):**

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

**Assessment:** **Evidence weight: weak. Direction is supported for two of roughly a dozen terms; no magnitudes; several citations overreach.** 22 rows: 4 VERIFIED, 11 VERIFIED-SOFTENED, 7 UNVERIFIED.

**Susceptibility composite.**
- Byrne (undergraduates, lab, delayed-reward task) supports only the **direction** of an N-under-pressure term. Exp 1 condition x N interaction ΔR² = .06.
- The docstring overstates the agreeableness result. Exp 2 has no low-pressure arm, so it cannot show an interaction, and the authors call the A result exploratory. 'A ranked above N' has no support.
- On C, Byrne's only signal (one uncontrolled correlation, not replicated) runs opposite to the -.20 dampener. The honest wording is 'no support', not contradiction.
- The default behavioural basis fills the A slot with cooperation_v2, which correlates +0.888 with bpf. That is a recorded double count with bpf in the kernel.
- Palm (UNVERIFIED) ranks N as the **least** susceptible trait in consumer persuasion. That cuts against the +.30 N weight, possibly differently for peer channels (edge_mult) and direct-persuasion channels (susc_mult), which share one scalar.
- The project's own record says no N susceptibility signal has been demonstrated (Nokken-Poole drift MISS, n=441; H5 null). Poole's low drift variance is consistent with the MISS and does not reopen it.

**Lexical vs behavioural basis.** Koutsoumpis (lexical categories explain 5.07% of self-report variance), Azucar (A is the least recoverable trait) and Celli review (Perception validity rho 0.30) support keeping lexical out of the default. None supports the sqrt shrinkage form. Birkenmaier and Lechner support measuring a projected persona only; the communion-to-susceptibility signs and the .60/.40 weights are design priors. PERSONA_W stays as ruled.

**Incentive (retiring 0.60).**
- Fouirnaies & Hall: final-term members cut effort by -0.24 SD (SE 0.03; range -0.09 to -0.41) with **no** ideological shift, in state chambers under binding term limits.
- The 0.60 size and the 'less responsive to pressure' mechanism are unsupported.
- The paper argues members seeking other office keep their electoral incentives, yet the list includes them.
- Gaynor: peers give retirees less LPAC money (115th Congress), which supports low target value, not low responsiveness.
- The competitiveness term 1+.30(comp-0.5) is attributed to Fouirnaies & Hall, which never tests seat competitiveness.

**Prospect domain.** Linde & Vis support a reflection effect in politicians, with votes as the likely reference point. That fits keying the domain on a live race. But framing was randomly assigned in hypothetical scenarios, the sample is 46 Dutch MPs, and risk-seeking is not the same thing as responsiveness. The 1.15/0.92 magnitudes and the docstring's 'loss-averse overall' are unsupported (no loss aversion found). The votes+economic OR 8.684 is a scenario-specific term in a pooled regression; quote it with the non-significant Fisher p=0.302.

**Lost signal.** With COMPET_MULT on (the default), the candidate-specific Ballotpedia comp term is neutralised and PVI-only c carries competitiveness. Cook's 2024 table (435 districts, as of 2024-09-12) shows PVI and rating diverge: Fitzpatrick is EVEN and Likely R, Salazar EVEN and Solid R.

**seed_prob.** Amsalem: visible MPs are less agreeable (β -.116) and less neurotic. The +.04 visibility and +.10 cooperation terms may pull against each other. The check corr(visibility_z, coop_v2) on the roster has not been run.

**Contradictions:**
- Retiring list: Fouirnaies & Hall argue members seeking other office retain electoral incentives (lower-bound argument), but the list applies incent 0.60 to them (for example Barr, Stefanik, Golden).
- Fouirnaies & Hall find no ideological or position shift in the final term, only less effort. The code reads the paper as reduced 'responsiveness to pressure'.
- The docstring says politicians are 'loss-averse overall' per Linde & Vis, which found no loss aversion.
- The docstring claims a Byrne Exp-2 A interaction, but Exp 2 has no low-pressure arm, so no interaction can be shown.
- The competitiveness incentive term 1+.30(comp-0.5) is attributed to Fouirnaies & Hall, who do not study seat competitiveness.

**Gaps:**
- Prospective H2. Needed: a test of whether higher-susceptibility members change cosponsorship or votes more after a dated contact or salience event. This is the only test that could validate the composite.
- Retiree responsiveness. Needed: a comparison of PAC-money-to-alignment slopes for retiring members against continuing members, splitting those leaving Congress entirely from those seeking other office.
- Domain and risk. Needed: a test of whether members in live races take more extreme or riskier CU positions than safe-seat members.
- Channel split for N. Needed: a test of whether one susceptibility scalar should serve both peer (edge_mult) and direct-pressure (susc_mult) channels (Palm lead).

**Paul's position vs the evidence:** Paul wants psychological traits to adjust response to pressure, placed as edge parameters (citing Oropeza, for which there is no evidence row), and notes that 10 personalities collapse to 4 observable ones. The evidence and the system's own validity record undercut this more than his notes acknowledge. Only N-under-pressure has directional lab support, and N has no demonstrated signal in the project's data. The lexical traits carry little true variance. Every weight is a design prior. The incentive and prospect terms rest on real findings (effort drop; reflection effect), but those findings are about different outcomes (effort, risky choice) than the one the layer models (movability under pressure). PERSONA_W stays per Paul's ruling; nothing here re-argues it.

**Open questions:**
- Should members seeking other office be removed from the retiring list, per Fouirnaies & Hall's own lower-bound argument?
- Should COMPET_MULT neutralise the candidate-specific Ballotpedia comp term, given Cook ratings show PVI misses incumbent strength?
- What are corr(visibility_z, coop_v2) and corr(visibility_z, affective N) on the 533-member roster? This is a quick check not yet run.
- Should the docstring's Byrne Exp-2 interaction claim and 'loss-averse overall' be corrected?

**Read first:** [[byrne-2014-chokes-under-pressure]], [[fouirnaies-2022-electoral-incentives-affect]], [[linde-2016-politicians-take-risks]], [[report-2024-2024-house-glance]], [[koutsoumpis-2024-kernel-truth-text]], [[amsalem-2018-effect-politicians-personality]]

## Competitiveness multiplier c (COMPET_MULT)

**Computes:** c = 1 + 1.0 x exp(-max(lean, 0)/5.0), range (1, 2], where lean = Cook PVI oriented to the member's party. It scales effective dollars on both money legs.

**Method:** Matched by Cook incumbent or district, with overrides; unmatched members get 1.0. Default ON since 2026-10-06 (D-104/D-105/D-107/D-108). Real dollars are unchanged.

**Code refs:** `fec/moneyball_core.py:74-172`, `fec/engine/build_competitiveness.py:1-304`, `data/money/competitiveness_c.json`

**Assumptions:**
- Dollars buy more in competitive seats (reciprocity NOT validated)
- AMPLITUDE and SCALE are uncalibrated owner judgment

**Evidence (9 rows; 9 distinct papers; 4 VERIFIED, 3 VERIFIED-SOFTENED, 2 UNVERIFIED):**

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

**Assessment:** **Evidence weight: nil on the premise; moderate on measurement cautions.** 9 rows: 4 VERIFIED, 3 VERIFIED-SOFTENED, 2 UNVERIFIED. **No row tests whether a dollar buys more in a competitive seat.** Reciprocity remains not validated (D-104..D-108), and AMPLITUDE 1.0 / SCALE 5.0 remain uncalibrated owner judgment.

**Where money goes, not what it buys.**
- Gaynor: vulnerable members (Cook PVI 'likely', Oct 2018, dichotomous) are about 70% (R) and 75% (D) more likely to receive member-to-member LPAC ties. 115th Congress, by party; denominators 238 R and 168 D connected nodes.
- Bonica models individual donors' allocation (2004-2012). The partisan electoral model fits worse than the ideological one.
- Both support Cook PVI as a signal donors act on. Neither supports the continuous exp(-lean/5) decay or any effectiveness premium.

**What a calibration would require.**
- Kalla & Broockman blocked on partisanship but report no heterogeneity by competitiveness. With 64 treated offices, it could not have estimated it.
- Cagala: establishing heterogeneous dollar effectiveness needs randomized variation, a heterogeneity test and a transfer check. Their warm-list loading was 0.959 (SE 0.710).
- Blackwell: givers target competitive seats on information the analyst lacks, so naive observational calibration of c is confounded.

**Measurement cautions.**
- PVI is not a race rating. In the Cook 2024 table (435 districts, as of 2024-09-12), six EVEN seats get five different ratings (Solid R to Toss Up). c maps all of them to 2.0, and maps D+3 Solid D and D+3 Lean D both to about 1.55.
- PVI is partly a product of the map. Best (UNVERIFIED): enacted maps shift asymmetry by 1.5-4 points vs 10,000 neutral plans (2010 round). Near lean 0 a 3-4 point shift moves c by about 0.45-0.55. This matters most in the ten flagged mid-decade redistricted states.
- McCartan (UNVERIFIED) adds that partisan metrics are not comparable across states.
- Becker: incumbent-based constituency change can diverge sharply from population change (Minnesota 1.93% vs 13.90%), which would be a sharper diagnostic than the state flag.

**Contradictions:**
- (none)

**Gaps:**
- Reciprocity premise. Needed: a design that varies competitiveness exogenously for sitting members. Mid-decade or post-census redistricting that changes a member's PVI is a candidate natural experiment, estimating within-member change in the contribution-to-alignment or contribution-to-access slope. It must address strategic targeting (Blackwell) and report party strata.
- Decay scale. If the premise holds, needed: a fit of heterogeneity in contribution effects across PVI bins to choose between the exponential form and step or linear alternatives.
- Map sensitivity. Needed: a per-district constituency-turnover measure (block assignments for the 2025 and 2026 maps) to inform Paul's override review.

**Paul's position vs the evidence:** Paul endorses Cook PVI as a vulnerability measure (Gaynor) and reads competitive seats as responsive. The evidence supports the first only as 'donors act on it' and is silent on the second. The hub already notes that no source Paul annotated says dollars buy more in competitive seats, and this assessment finds none either. His position and the evidence agree on the facts; c is an owner-judgment dial and should be described that way everywhere it is quoted.

**Open questions:**
- Should c use a forward-looking race rating (current-cycle Cook ratings, dated before the decision) instead of, or alongside, PVI?
- How should c be read for members in the ten flagged redistricted states, where the PVI behind c is partly an artifact of the map?
- Is an uncertainty band on c warranted for seats with large 2025-to-2026 PVI shifts?

**Read first:** [[gaynor-2022-financial-ties-bind]], [[report-2024-2024-house-glance]], [[blackwell-2026-effect-political-advertising]], [[cagala-2021-optimal-targeting-fundraising]], [[best-2022-redistricting-commissions-avoid]]

## Electoral security, leverage and contest-defend plays

**Computes:** Security percentile from Ballotpedia Election_Average; leverage = 1 + 0.8(1 - security); play routes contest-primary, contest-general, defend or none.

**Method:** Defend value = comp x positive gap vs the cross-party average. Contest route = the larger of the primary and general gaps. Contest plays convert opponents in spread().

**Code refs:** `fec/engine/build_electoral.py:7-31`, `fec/engine/build_electoral_plays.py:20-36`

**Assumptions:**
- Ballotpedia electoral average measures security

**Evidence (5 rows; 5 distinct papers; 1 VERIFIED, 4 VERIFIED-SOFTENED):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[report-2024-2024-house-glance]] | 2024 | suggests-extension | The table gives a forward-looking expert risk category (Solid, Likely, Lean, Toss Up) for each seat, along with OPEN and VACANT flags: 41 OPEN lines and 4 VACANT seats as of 2024-09-12. | Useful only as the design pattern for a dated, forward-looking risk input. Using it would require the current-cycle Cook ratings, dated before the decision, not this 2024 table. Open-seat retirees would also need the same retiring treatment the receptivity layer already gives them. | weak | VERIFIED-SOFTENED |
| [[gaynor-2022-financial-ties-bind]] | 2022 | qualifies | Vulnerability is associated with higher inward LPAC degree, for both raw and weighted degree, from a regression reported in the text. Leadership is associated with higher outward degree. Givers target electorally exposed co-partisans. | Backs vulnerability as a magnet for co-partisan money, consistent with defend plays. Gives no evidence that competitive seats are cheaper to move. The paper uses Cook PVI while this component uses Ballotpedia Election_Average, so two vulnerability measures feed the engine. Applies within party only. | weak | VERIFIED |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | qualifies | Text PDF p.20: the partisan electoral model's main implication is that donors fund competitive races to maximize party seat share. Table 2 (PDF p.21): for individuals partisan -1405855 vs ideological -1334200, so the partisan electoral model fits worse. Paper also says the ideological model significantly improves fit over a partisan model. | The paper states a hypothesis it rejects for individual donors; it neither supports nor refutes the comp weighting. It describes donor allocation, not the payoff of a play. | weak | VERIFIED-SOFTENED |
| [[chohan-2025-crypto-oligarchy-cryptocurrency]] | 2025 | context | Fairshake campaigned against Katie Porter in California's Senate primary and opposed Jamaal Bowman's re-election bid. The paper calls this influence 'decisive' and offers no counterfactual or estimate. | Shows that federal primary-contest plays by an industry actor occurred in 2024. It is not evidence that such plays work, because outcomes are confounded and there is no comparison group. Do not use it to calibrate conversion. | weak | VERIFIED-SOFTENED |
| [[porter-2025-campaignview-campaign-platforms]] | 2025 | suggests-extension | Platform text for 5,228 of 6,016 (86.9%) major-party ballot-eligible House primary candidates 2018-2022, with 43,465 hand-topic-coded platform points. Identifiers are FECCandID (not universal) and BioGuideID (incumbents only). | House only, 2018-2022 cycles; the 14 topic codes are not CU categories. BioGuideID is incumbent-only and the paper advises against aggregating by either ID. Challenger coverage gaps (low fundraising, uncompetitive races) are where the proxy matters most. New analysis stays UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED |

**Assessment:** **Evidence weight: weak.** 5 rows: 1 VERIFIED, 4 VERIFIED-SOFTENED. Nothing tests whether contest or defend plays convert anyone, or whether leverage = 1 + 0.8(1-security) tracks anything.

- **Gaynor (VERIFIED):** vulnerability attracts co-partisan LPAC money (115th Congress, within party). That is consistent with defend plays as a description of where allies spend, not evidence that competitive members are cheaper to move. Gaynor uses Cook PVI while this component uses Ballotpedia Election_Average, so the engine runs two vulnerability measures (three counting Cook ratings, which it does not use).
- **Bonica:** the partisan electoral model (donors fund competitive races) fits **worse** than the ideological model for individual donors (2004-2012). It describes donor allocation, not the payoff of a play.
- **Cook 2024 table:** offers the design pattern of a dated, forward-looking risk input. Ballotpedia's average looks backward. Open-seat retirees would need the retiring treatment.
- **Chohan:** Fairshake ran federal primary-contest plays in 2024 (Porter, Bowman). No counterfactual, so the row shows only that such plays happen.
- **Porter CampaignView:** challenger platform text (5,228 of 6,016 House primary candidates, 2018-2022, 86.9%) could replace the same-party-average proxy in the contest-primary route. It is House only, uses non-CU topic codes, and BioGuideIDs exist for incumbents only.

**Contradictions:**
- (none)

**Gaps:**
- Effectiveness of plays. Needed: a test of whether CU PAC dollars to members with lower electoral security precede greater later alignment than equal dollars to safe members (same strategic-targeting caveat as c).
- Contest outcomes. Needed: a comparison-group study of industry primary-contest plays (for example matched contested and uncontested primaries) on later incumbent positions.
- Security measure. Needed: a test of whether Ballotpedia Election_Average, Cook PVI or Cook rating best predicts later electoral loss, to choose one vulnerability input across components.

**Paul's position vs the evidence:** Paul highlighted Bonica's statement that, under the partisan electoral model, donors direct funds to competitive races. In Bonica that model is a hypothesis the paper tests and rejects in favour of the ideological model for individual donors. Paul's highlight should be read as a stated model implication, not a finding. Gaynor gives better support for the narrower claim that co-partisan money flows to vulnerable members.

**Open questions:**
- Should electoral-plays, the receptivity domain and the salience VULN multiplier share one vulnerability measure instead of mixing Ballotpedia and Cook PVI?
- Is LELEV 0.8 intended as owner judgment? No source bears on it.

**Read first:** [[gaynor-2022-financial-ties-bind]], [[bonica-2013-mapping-ideological-marketplace]], [[report-2024-2024-house-glance]], [[porter-2025-campaignview-campaign-platforms]], [[chohan-2025-crypto-oligarchy-cryptocurrency]]

## Scenario runner (what-if)

**Computes:** Change in coalition spread and flip value under user money, defection or stance moves, using CRN at MC 40.

**Method:** A defection shifts all movable positions by at most 1 fine tier.

**Code refs:** `fec/engine/scenario.py:1-120`

**Assumptions:**
- A defection is a general disposition shift across issues

**Evidence (4 rows; 4 distinct papers; 2 VERIFIED, 2 VERIFIED-SOFTENED):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[weimer-2023-causal-approach-detecting]] | 2023 | qualifies | The paper's Monte Carlo is a negative control for an estimator applied to observed data: simulate data with no effect, re-run the matching estimator, expect a null. | A Weimer-style placebo applies to empirical estimators that read observed outcomes (forecasters, the contract), not to the forward cascade's deltas, which are model outputs. | weak | VERIFIED |
| [[harrison-2010-introduction-monte-carlo]] | 2010 | context | Monte Carlo is a key tool for analytically intractable problems. Some feel it is overused as a lazy way out, and analytic and experimental alternatives should be considered before committing to simulation. | The split between closed-form flip math and MC spread fits the paper's advice. The paper offers nothing on which analytic proxy (Viral Centrality, Katz-Bonacich) might replace the cascade MC; that comparison would need its own test. | weak | VERIFIED-SOFTENED |
| [[xu-2024-improved-lite-based]] | 2024 | qualifies | Paper reports node updates of 18,468 vs 72,835 (freezing) and 16,864 vs 114,929 (melting) for its improved D* Lite against full global re-planning. Numbers are correct. The paper frames reuse of earlier search data as the motivation (Sec. 2.1) but does not say D* Lite family planners save work only by reuse. | Documentation point only. At 533 members and MC=40 a full recompute is cheap, so incremental reuse is not warranted. The Xu numbers say nothing about the cascade. The cited work is Ren et al., not Xu. | weak | VERIFIED-SOFTENED |
| [[bharathi-nd-competitive-influence-maximization]] |  | qualifies | The best-response guarantee holds only for the last committer. Pure Nash equilibria need not exist and mixed ones do (lines 83-85). Theorem 2 bounds the price of competition at 2 on total activations, not on any one player's share. First-mover results cover only a duopoly on disjoint lines (Lemma 2, lines 149-153). | The fixed-opponent framing holds only if industry commits first and never reacts. FEC filings are public, so if industry reacts, CU becomes a first mover on a general graph, where the paper gives no algorithm or guarantee. The price-of-competition bound says nothing about CU's own share. | weak | VERIFIED |

**Assessment:** **Evidence weight: weak, and mostly about framing.** 4 rows: 2 VERIFIED, 2 VERIFIED-SOFTENED. Nothing tests the component's one behavioural assumption, that a defection is a general disposition shift across issues.

- **Weimer:** its Monte Carlo is a negative control for an **estimator** run on observed data. The scenario runner's zero delta for an unperturbed scenario is an identity property of CRN, not a placebo test. Weimer-style placebos belong on the empirical forecasters and the outcome contract, not on forward-model deltas.
- **Harrison:** keeping the flip channel closed-form (pflip) while simulating only cascade spread fits its advice. No row compares analytic proxies (Viral Centrality, Katz-Bonacich) with the cascade MC.
- **Bharathi:** the best-response guarantee holds only for the last committer facing a fixed opponent. Scenarios hold industry fixed. FEC filings are public, so if industry reacts, CU becomes a first mover on a general graph, where no guarantee exists. The price-of-competition bound says nothing about CU's own share.
- **Xu (D* Lite):** documentation point only. The docstring cites Ren et al. for incremental recompute, but isolation actually comes from CRN, and at 533 members and MC=40 full recompute is cheap.

**Contradictions:**
- The docstring attributes 'recompute only what the scenario touches' to D* Lite-style reuse, but the code recomputes every category and gets isolation from common random numbers.

**Gaps:**
- Cross-issue defection. Needed: a panel of members' stance changes across CU categories, testing whether a switch on one issue predicts same-direction switches on others within the same window, by party. Poole's stable ideal points imply general stability but do not address correlated switching.
- Reactive opponent. Needed: an observational check of whether industry PAC giving responds to CU giving within a cycle, using FEC dates, to test the fixed-opponent assumption.

**Paul's position vs the evidence:** Paul treats Monte Carlo as a tool and highlighted Weimer's use of Monte Carlo as a negative control. The evidence clarifies that the transfer applies to Melange's empirical estimators, not to the scenario runner's forward simulation. Paul's highlight is valuable, but it is aimed at the wrong component if read as validating scenario deltas.

**Open questions:**
- Should Weimer-style placebo runs be added to the forecasters and outcome contract instead?
- Should the D* Lite reference be removed from the scenario docstring?

**Read first:** [[bharathi-nd-competitive-influence-maximization]], [[weimer-2023-causal-approach-detecting]], [[harrison-2010-introduction-monte-carlo]], [[xu-2024-improved-lite-based]]

## Priority anchor, leave-one-out orientation and engagement signal

**Computes:** Topic priority = tier anchor (1, 2/3, 1/3, 0) + alpha x IMP-weighted stance on other topics (alpha 0.15 flip / 0.25 coalition); engagement = block-weighted legislative activity; base = max(0, eng + 0.15 x priority).

**Method:** Leave-one-topic-out orientation. Fails closed on an incomplete roster.

**Code refs:** `fec/moneyball_core.py:255-317,573-625`

**Assumptions:**
- Stance on other CU issues predicts priority on this issue

**Evidence (1 rows; 1 distinct papers; 1 VERIFIED-SOFTENED):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | qualifies | The second dimension now adds less than 1 percent improvement over the first dimension in both chambers. | engine_orientation reads category_stance scores, not votes. The Path V vote shares (UNVERIFIED) show opposite party direction across some categories (CFPB, Technology, Exam, CU Capital Requirements lean R; Charter, NCUA Budget & Insurance lean D) and none in others. If stance scores share that pattern, leave-one-out borrowing could import party sign; that has not been checked on the stance scores. Alpha is small. | weak | VERIFIED-SOFTENED |

**Assessment:** **GAP. Effectively unevidenced.** There are 1 row(s) (1 VERIFIED-SOFTENED), and the system cites no literature for this component.

- Poole's finding that a second NOMINATE dimension adds under 1% supports, in principle, borrowing a member's orientation from other topics, since positions are low-dimensional.
- engine_orientation reads category stance scores, not votes. Path V vote shares (UNVERIFIED) suggest party direction differs across CU categories: CFPB, Technology, Exam and CU Capital Requirements lean R; Charter and NCUA Budget & Insurance lean D. If stance scores follow the same pattern, leave-one-topic-out borrowing could import a party sign into topics where it does not belong. This has not been checked.
- Alpha is small (0.15 flip / 0.25 coalition), which limits the damage.
- The engagement signal (block-weighted legislative activity) and the tier anchors (1, 2/3, 1/3, 0) have no evidence at all.

**Contradictions:**
- (none)

**Gaps:**
- Leave-one-topic-out validity. Needed: a held-out prediction test of whether IMP-weighted stance on other CU topics predicts stance on the held-out topic, reported by party and by topic. It would show whether categories with opposite party direction are mis-predicted.
- Engagement as a priority signal. Needed: a test of whether block-weighted legislative activity on a topic predicts later action (cosponsorship, statements) on that topic, against a volume-only baseline.
- The tier anchor spacing (1, 2/3, 1/3, 0) has no source. A sensitivity sweep against held-out outcomes would show whether it matters.

**Paul's position vs the evidence:** No literature is cited and Paul has stated no position. The one bearing (Poole) gives conditional support for borrowing orientation and raises a party-sign risk that has not been checked on the stance data.

**Open questions:**
- Do category stance scores show the same cross-category party-direction pattern as the UNVERIFIED Path V vote shares?
- Should alpha be set per topic, or zeroed for topics whose party direction opposes the member's other topics?

**Read first:** [[poole-1999-nominate-after-years]]

## Across components

**1. Out-party influence is treated four different ways on the same evidence.**

| Component | Treatment of out-party influence |
|---|---|
| Kernel cpf_pressure | +0.5 |
| Kernel cpf_diffusion | floor 0.4 |
| lt-priming | clamps non-bridge cross-party cues to about 0 |
| Salience rally | gives out-partisans +0.5 when bpf ≥ 0.30 |
| Salience retreat | gives out-partisans -1 |

Every cue row (Bakker, Merkley, Furnas 2019 by party stratum, ringe-2013-pinpointing-powerful-voting) supports a non-positive out-party cue. None supports positive out-party persuasion, and none supports the sqrt(bpf) bridge exemption, because no paper measures a bridge attribute. Only the lt-priming clamp matches the evidence. One preregistered, party-pair-stratified cosponsorship test could settle the sign for all of these. Whether to harmonise is Paul's call.

**2. bpf is loaded repeatedly.** bpf enters:
- cpf_diffusion;
- the lt-priming cross-cue;
- the flip target (1 + 0.3 bpf);
- the salience rally gate;
- the receptivity layer, through cooperation_v2 (recorded rho +0.888 with bpf), which feeds edge_mult on the same edges as cpf_diffusion.

The same signal therefore scales one edge more than once. No evidence row bears on bpf directly.

**3. Three vulnerability measures, and the switch removes the candidate-specific one.**
- Cook PVI drives c.
- Ballotpedia Election_Average drives electoral security and leverage, the prospect domain, VULN and the retired incentive comp term.
- Cook ratings are used nowhere.

With COMPET_MULT on, the only candidate-specific measure (the Ballotpedia comp term in incent) is neutralised. Cook's 2024 table shows PVI and rating diverge for exactly the incumbents Paul cares about (Fitzpatrick EVEN and Likely R).

**4. Citation hygiene is a recurring problem.** These constants carry a citation the cited paper does not support:
- LOSSFRAME ← Linde & Vis (no loss aversion found);
- 'loss-averse overall' ← Linde & Vis;
- competitiveness incent term ← Fouirnaies & Hall (does not study competitiveness);
- retiree 'responsiveness' ← Fouirnaies & Hall (effort, not positions);
- VULN 1.35 ← Noble A.4 (concerns the president);
- backfire-psi ← Noble Prop 5;
- Byrne Exp-2 A interaction (no low-pressure arm);
- Volden-Wiseman passage bonus (not in the code);
- D* Lite reuse (isolation comes from CRN);
- WM/WP and MAXSHIFT labelled CALIBRATED (no calibration exists).

The honest label for each is **owner judgment / design prior**. Relabelling is a documentation change; any value change goes through change control.

**5. Transfer conditions.** No row estimates an engine quantity on U.S. congressional data:
- elite evidence comes from Dutch MPs, state chambers under term limits, EP committee dyads, DC staff stated intentions (2017) and one 2013 field experiment;
- the rest is mass-public or simulation.

Direction-of-effect claims transfer modestly; magnitudes do not transfer at all.

**6. Guardrails held.**
- pflip, seed_prob, gate, leverage and advisory scores are heuristic model outputs, not access, introduction or vote probabilities (D-025).
- Reciprocity is not validated by any row (D-104..D-108).
- LDA stays identity-only (D-021). Huneeus & Kim and Jacoby are not grounds for a lobbying-dollar channel.
- Trivedi's degree-based edge probability is **FLAGGED**: it must not be read as support for the REJECTED per-edge volume normalization. No paper here argues for dropping regular cosponsorship edges.
- Coleman's 'reciprocated ties' and Cialdini's 'reciprocity' are unrelated to the system's reciprocity premise.
- UNVERIFIED rows (13) are leads only (DE-062).

**7. Strongest single item.** The salience defensive-credit contradiction. It is internal (no transfer question), it is live in the 2026-10-08 advisory (18 issues), and it is surfaced on the dashboard.

## Research agenda

1. Salience defensive credit: decide whether to gate held-count credit to sq=='defend' issues, restoring Noble-Turner's status-quo asymmetry. Live in the 2026-10-08 advisory (18 issues), where all four GO PUBLIC verdicts are offense issues. Paul's ruling; no new data needed.
2. Out-party sign: run one preregistered cosponsorship-diffusion test on CU bills, stratified by party pair and bpf tercile, measuring whether out-party joins add positive, zero or negative join hazard. Use the result to settle cpf_pressure 0.5, the cpf_diffusion floor 0.4, the lt-priming clamp and the salience rally +0.5 together.
3. Tie-order sensitivity: rerun the competitive cascade with B-first and randomized frontier order under identical CRN draws to measure how much CU reach the A-first tie rule adds. Cheap and internal.
4. Threshold vs IC: fit a hazard model of joining CU bills on count and share of already-joined close partners against linear exposure, with homophily controls. This also yields estimates for the saturation constant 6 and the THETA ladder.
5. Citation and label hygiene: relabel LOSSFRAME, WM/WP, MAXSHIFT, VULN, backfire-psi, the competitiveness incent term and retiree 'responsiveness' as owner judgment; remove or implement the Volden-Wiseman comment and the D* Lite docstring. Documentation only; values unchanged.
6. Retiree list: separate members seeking other office from those leaving politics, per Fouirnaies & Hall's lower-bound argument. Then test PAC-to-alignment slopes for each group against continuing members.
7. Reciprocity behind c: use redistricting-induced PVI changes for sitting members as a natural experiment for within-member change in contribution-alignment slopes, by party and with a strategic-targeting design (Blackwell, Cagala). Until then c stays owner judgment (D-104..D-108).
8. Vulnerability measure: test whether Ballotpedia Election_Average, Cook PVI or Cook race rating best predicts later electoral loss, then decide whether COMPET_MULT should keep neutralising the candidate-specific comp term.
9. Contact take-up (SGATE, PERS): locate a non-LDA CU contact log (fly-ins, meetings) large enough for the H1 contract (at least 150 members with cosponsorship deltas) and estimate conversion and backfire by prior stance tier, with a power check per Sverdrup.
10. Competitor-spend event study: FEC-dated large competitor spends against later press-stance or roll-call shifts, versus matched members, for MAXSHIFT/DTHRESH and any threat asymmetry.
11. Leave-one-topic-out validity: held-out topic stance prediction by party and topic, checking whether categories with opposite party direction are mis-predicted by priority orientation.
12. Quick roster checks, not yet run: corr(visibility_z, coop_v2) and corr(visibility_z, affective N) for seed_prob tension; the system's mc_stability_check at MC 20 and 40 on reported rank decisions; a census of cosponsor withdrawals to test IC progressivity.
