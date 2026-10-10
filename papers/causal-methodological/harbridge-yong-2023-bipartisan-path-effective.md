---
type: paper
title: "The Bipartisan Path to Effective Lawmaking"
authors: ["Laurel Harbridge-Yong", "Craig Volden", "Alan E. Wiseman"]
year: 2023
venue: "Journal of Politics (per item metadata; the text itself is an August 2022 manuscript and names no journal)"
doi: ""
category: "Causal & Methodological"
systems: ["melange-allocation", "melange-influence-engine", "melange-legislative-data", "melange-psychometrics", "melange-sna"]
components: ["melange-sna.bipartisanship-bpf", "melange-influence-engine.flip-model", "melange-legislative-data.effectiveness-shadow", "melange-psychometrics.cooperation-v2", "melange-allocation.per-topic-allocators", "melange-influence-engine.cascade-kernel"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/harbridge-yong-2023-bipartisan-path-effective.pdf]]"
original_paths: []
sha12: ["b269422428eb"]
tags: [paper, causal-methodological, type/paper, theme/causal-methodological, evidence/verified]
---

# The Bipartisan Path to Effective Lawmaking

**Laurel Harbridge-Yong, Craig Volden, Alan E. Wiseman (2023).** *Journal of Politics (per item metadata; the text itself is an August 2022 manuscript and names no journal).*  Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> Harbridge-Yong, Volden and Wiseman ask whether bipartisanship pays for individual lawmakers. They use all House and Senate members in the 93rd-114th Congresses (1973-2016; about 9,000 House and 2,170 Senate legislator-Congress rows) and regress the Volden-Wiseman Legislative Effectiveness Score on the share of a member's cosponsors who come from the other party. The design uses legislator fixed effects. That attracted share predicts effectiveness: within member, the coefficient is 0.433 in the House and 0.330 in the Senate, so one SD is worth about 6-8 percent of mean LES. The share members offer to the other party's bills does not predict effectiveness once controls are in. The effect holds across eras, majority margins and polarization levels, and in majority and minority strata. It is an inverted U that peaks near a 50/50 cosponsor mix (0.496 House, 0.512 Senate). In the House the attracted share also predicts each later stage, from committee action to law. Offering cross-party cosponsorships predicts attracting them, at 0.317 within member in the House and 0.547 in the Senate. The evidence is observational, with one-tailed tests and no design-based identification. For Paul's system, the sharpest point concerns direction. bpf is an undirected, symmetrized cross-party tie share. Cooperation v2's directional component measures offered support. Nothing in the system measures attracted support, which is the side the paper ties to effectiveness. The flip model's (1+0.3 bpf) passage bonus and the coalition seed boost (1+0.8 bpf) are monotone, but the paper finds a peak and then a decline. The paper's reciprocity is cosponsorship exchange. It does not validate the money reciprocity in D-104..D-108. Any transfer is UNVERIFIED until it is reproduced on Paul's data.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Why does bipartisanship persist in Congress despite polarization and primary threats? The paper asks whether legislators who attract a larger share of opposite-party cosponsors on their bills are more effective at advancing them, whether that holds across eras and party conditions, whether the effect has an optimum, and whether offering bipartisan cosponsorships in turn drives attracting them (reciprocity).

**Theory.** Two competing hypotheses. Bipartisanship and Effectiveness: spatial models (Krehbiel 1998) imply successful bills need pivotal veto-player support; bipartisan support signals quality to gatekeepers (Koger 2003) and refines proposals (p.4-6). Partisanship and Effectiveness: leaders reward one-sided party support (Cox and McCubbins 2005; Hastert Rule), and policy specialists achieve more (Bernhard and Sulkin 2018) (p.6-7). Conditional hypotheses cover majority status, era and polarization (p.7-8). Reciprocity: offering cross-party cosponsorships generates reciprocal ones (p.25-26).

**Data.** Sample: all Representatives and Senators serving in the 93rd-114th Congresses (1973-2016); independents excluded. Unit: legislator by two-year Congress. Bills: all public bills introduced in the period. Cosponsorship data for 93rd-110th Congresses from Fowler (2006), updated by the authors for 111th-114th. N: House 8,997 to 9,202 legislator-Congress observations; Senate 2,167 to 2,192 (Table 1). Hitchhiker law sample (Table A11): 4,494 House observations, 1993-2014.

**Method.** Cross-sectional time-series least squares regressions with legislator fixed effects and robust standard errors (Tables 1-3, A2-A13); pooled OLS and fixed-effects OLS for reciprocity regressions (Table 4, clustered SEs in pooled models). Nonlinear models add a squared term. Robustness: commemorative-bill exclusion, normalization by Congress, cosponsorship-level controls, low-cosponsorship eras, binary alternative measures, Lugar index, no member FE, electoral safety interactions, later legislative stages, hitchhiker-inclusive law counts, and era splits (pre/post 1994).

**Identification.** No instrument, natural experiment or design-based strategy. Identification relies on legislator fixed effects (within-member variation), a standard covariate set from the effectiveness literature, and robustness checks. Authors interpret coefficients as associations and separately test the offered-vs-attracted distinction (p.15-18). Reverse causation and time-varying confounds are not formally addressed.

**Scope.** US Congress, House and Senate, 1973-2016, public bills only. Population is all members in that window. The argument concerns individual lawmaking effectiveness, not chamber-level outcomes; the 60-vote cloture environment is offered to explain higher Senate levels.

## Key findings

- Table 1 Model 1.2 (House, p.17): proportion bipartisan cosponsors attracted = 0.433 (SE 0.097), p<0.001; Model 1.5 (Senate, p.17) = 0.330 (SE 0.147), p<0.05. One SD increase = +0.08 LES in House and +0.06 in Senate, about 6-8 percent of mean LES of 1.0 (p.16).
- Uncontrolled Models 1.1 (House 0.986) and 1.4 (Senate 0.538) are larger; the coefficient on proportion bipartisan cosponsorships offered is not significant with controls (Model 1.3 = -0.234; Model 1.6 = -0.420) (p.17-18).
- Robust to excluding commemorative bills, normalizing by Congress, controlling for cosponsorship levels, binary alternative measures (Table A5 House 0.213, p<0.001) and the Lugar index (p.18-19).
- Without member fixed effects coefficients are larger (Table A6 with controls: House 0.552, p<0.001; Senate 0.440, p<0.001) (p.18-19, Appendix p.10).
- Table 2 (p.20-21): effect stays positive and significant (House 0.432 to 0.629; Senate 0.358 to 0.442) when interacted with a Congress counter, majority margin or party polarization; all interactions small and not significant.
- Table 3 nonlinearity (p.22-23): House linear 1.828 and squared -1.844 peak at 0.496; Senate 1.666 and -1.626 peak at 0.512. A one-SD decline costs about 22 percent of LES in House and 16 percent in Senate; a one-SD rise adds about 8 and 4 percent (p.24).
- Majority and minority subsamples show similar nonlinear shapes (Table 3 Models 3.2-3.3, 3.5-3.6). Authors read the larger majority coefficients as a scale artifact, since majority LES is about three times minority LES (p.24-25).
- Reciprocity (Table 4, p.27): offering bipartisan cosponsorships predicts attracting them. House pooled 0.626 (SE 0.039); House with member FE 0.317; Senate pooled 0.739; Senate FE 0.547; all p<0.001.
- Distance from median predicts fewer bipartisan cosponsors in pooled models (House -0.154, Senate -0.114) but the House effect disappears with member FE (0.005) (p.27).
- Later stages (Table A8, House, Appendix p.12): bipartisan share predicts action in committee 0.894, beyond committee 0.853, passing chamber 0.800, and becoming law 0.485 (all p<0.001). Senate committee-stage model A9.2 is not significant (1.767); Senate law model A9.5 = 0.589 (p<0.05) (Appendix p.13).
- Law production (Table A10, Appendix p.14): nonlinear House laws 1.685 with squared -1.586; Senate 2.342 with squared -2.135; output rises to about 50 percent bipartisan cosponsors and falls after. Hitchhiker-inclusive laws 1993-2014 (Table A11): linear 0.691, p<0.001.
- Eras and electoral safety: House effect 0.470 (1973-94) and 0.380 (1995-2016); Senate 0.332 and 0.307 (Tables A12-A13, Appendix p.16-17). Electoral-safety interaction not significant (Table A7). Descriptives (Table A1): mean share 0.290 House (SD 0.194), 0.354 Senate (SD 0.190); median 30-40 percent in the 1970s-80s (Figure 1, p.12-13).

## Limitations

- LES omits oversight, constituent service, obstruction and amendment success; authors note amendment-success research is a gap (p.8-9, footnote 5).
- Cosponsorship treated as public endorsement; the authors concede it is debatable whether it implies effort to pass the bill (p.9-10).
- No design-based identification. Legislator fixed effects and controls only; time-varying confounders not excluded. Authors call the coefficient a within-member marginal impact (p.15) and mostly use associational language.
- House cosponsor cap of 25 before 1979 and low early cosponsorship rates (footnote 9, p.11); addressed in robustness Table A4 only.
- Table notes report one-tailed significance tests. Supplemental appendix is stated as to be posted online; the appendix tables are included in the text file.
- The Table A9 note claims greater success starting with passage out of committee, but Model A9.2 (committee action) is not significant, so the note overstates the Senate committee-stage result.

## Short quotes

- "bipartisanship unambiguously helps individual legislators who seek to advance their policy goals (abstract, p.1)"
- "Holding the number of bill cosponsorships constant, as a legislator attracts more cosponsors from the opposite party to her bills, her value increases (p.11)"
- "the effect of bipartisanship rises until about half of all of a member's cosponsors are from each party, and then falls again (p.22-23)"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-sna#Bipartisanship (bpf)\|Bipartisanship (bpf)]] | qualifies | Within-member (legislator FE, one-tailed) coefficient on the attracted share, the per-bill mean of other-party cosponsor share: House 0.433 (SE 0.097) Model 1.2; Senate 0.330 (SE 0.147, p<0.05) Model 1.5. Offered share not significant with controls: House -0.234 (Model 1.3), Senate -0.420 (Model 1.6). Window 93rd-114th Congresses (1973-2016), about 9,000 House and 2,170 Senate legislator-Congress rows. (Table 1 Models 1.2, 1.3, 1.5, 1.6 (printed pp.16-17); Table A1 variable definitions) | bpf() in fec/moneyball_core.py reads bipartisan.json (default 0.24). The materialize script builds that file: each undirected pair takes the mean of its two directed rows when both directions exist, otherwise the single directed weight; member share = (cross + 6 x prior) / (same + cross + 6). Directed rows are kept only in audit fields, so bpf mixes offered and attracted ties. No attracted-direction share is computed in fec .py. `fec/legislation/materialize_legislation_base_and_bipartisan.py (pair symmetrization about 725-737; share about 786); fec/moneyball_core.py:68-72` | Paper variable is a mean over a member's own bills of other-party cosponsor share; bpf is a roster-snapshot weighted tie share with prior shrinkage, so window and denominator differ. Paper coefficients are by chamber, 1973-2016, one-tailed; quote by chamber only. Transfer to the current roster is UNVERIFIED until reproduced (DE-062). | strong | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | qualifies | Within-member quadratic in the attracted share: House linear 1.828 and squared -1.844 peak at 0.496 (Model 3.1); Senate 1.666 and -1.626 peak at 0.512 (Model 3.4). Effectiveness falls on both sides of the peak. A one-SD decline from the mean costs about 22 percent of LES in the House and 16 percent in the Senate; a one-SD rise adds about 8 and 4 percent. Mean LES about 1.0. (Table 3 Models 3.1 and 3.4; footnote 21; text p.24) | flip_model.py:142 multiplies the target by (1+0.3*bpf). The inline comment attributes a passage bonus of +8-14 percent to Volden-Wiseman. No 8-14 percent figure appears in the paper text; its figures are 6-8 percent of LES per SD and the 22/16 and 8/4 percent shape numbers. The bonus is +7.2 percent at bpf 0.24 and +30 percent at bpf 1.0, rising monotonically with no peak. `fec/engine/flip_model.py:142` | A linear bonus approximates the paper only below a share of about 0.5, where most members sit (paper mean 0.29 House, 0.35 Senate). The paper outcome is LES on a member's own sponsored bills, not the value of flipping a vote, and its share is the attracted share, not bpf. Any change to the functional form would move ranks and is not proposed here. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-legislative-data#Five-stage legislative effectiveness shadow and CEL criterion\|Five-stage legislative effectiveness shadow and CEL criterion]] | suggests-extension | Stage models use a member's count of bills reaching each stage as the DV, with legislator FE and one-tailed tests. House: committee action (A8.2) 0.894 SE 0.204; beyond committee (A8.3) 0.853; passing chamber (A8.4) 0.800; law (A8.5) 0.485 SE 0.088; all p<0.001. Senate: committee action (A9.2) 1.767, not significant; law (A9.5) 0.589, p<0.05. (Appendix Table A8 (House, p.12); Table A9 (Senate, p.13)) | legislative_effectiveness.py defines STAGES BILL AIC ABC PASS LAW (line 17), role weights sponsor 3, original 2, cosponsor 1 (lines 18-19), a cosponsor timing multiplier of 2 to the power of minus delay over 180 (line 132), and compute_effectiveness (line 152). There is no cross-party term. I found no code that compares the shadow score with the Volden-Wiseman LES; verify_effectiveness_shadow.py checks internal consistency only. `fec/engine/legislative_effectiveness.py:17-19,132,152; fec/legislation/verify_effectiveness_shadow.py` | The paper DVs are counts of a member's own bills at each stage, not stage rates and not LES. A shadow-vs-LES check would need a per-Congress attracted share (not built) and a comparable LES series. It stays a diagnostic. Quote the coefficients as count-DV associations, House and Senate separately. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Cooperation v2 (share-based bridging)\|Cooperation v2 (share-based bridging)]] | supports | Offering cross-party cosponsorships predicts attracting them (Table 4; DV attracted share, IV offered share). House pooled Model 4.1 0.626 SE 0.039; House member FE Model 4.2 0.317 SE 0.036; Senate pooled Model 4.3 0.739 SE 0.045; Senate FE Model 4.4 0.547 SE 0.047; all p<0.001. Distance from median: House pooled -0.154, FE 0.005 (n.s.); Senate pooled -0.114, FE -0.096. (Table 4 Models 4.1-4.4, pp.25-27) | coop_v2_candidate.py composite = mean of z(c1) and z(c2). c1 is the bipartisan.json undirected cross-party share. c2 is the directional offered cross-party share from bills the member chose to join. c3 reciprocity is reported and never composited. receptivity_layer.py:183 records coop v2 as a near-duplicate of bpf (rho +0.888), a system-internal overlap, not paper evidence. `fec/members/coop_v2_candidate.py:9-17 and composite block about 108-118; fec/members/receptivity_layer.py:183` | The paper is a member-level aggregate over 1973-2016, by chamber. Pooled and FE coefficients differ: the House roughly halves under FE, the Senate falls by about a quarter, so quote by chamber with strata. The paper shows offered-to-attracted reciprocity only, not that the composite tracks any effectiveness outcome. | weak | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Per-topic flip and coalition allocators\|Per-topic flip and coalition allocators]] | qualifies | Returns to bipartisan composition are nonlinear. One SD below the mean costs about 22 percent of LES in the House and 16 percent in the Senate; one SD above adds about 8 and 4 percent. Gains flatten and reverse past a 50/50 mix. (Table 3 and text p.24) | per_topic_coalition.py line 30 sets BETA_C = 0.8. Line 117 boosts each positive marginal spread by (1 + BETA_C*bpf) before the water-fill. The boost is monotone in bpf, and the code comment says bridges get a larger seed. `fec/engine/per_topic_coalition.py:30,117` | This holds only if the seed boost is meant to proxy a recruit's legislative effectiveness. The paper says nothing about diffusion seeding, so the link is an analogy and the allocators are diagnostic. | weak | VERIFIED |
| [[systems/melange-influence-engine#Cascade activation kernel build_P and partisan edge factors\|Cascade activation kernel build_P and partisan edge factors]] | context | Cross-party cosponsorship is exchanged: members who offer cross-party support attract it back, within member 0.317 House (Model 4.2) and 0.547 Senate (Model 4.4), p<0.001. The paper tests member-level aggregates only, with no dyadic diffusion or join timing. (Table 4 Models 4.2 and 4.4, p.27) | moneyball_core.py cpf_diffusion (line 543) returns 0.4 + 0.6*sqrt(bpf_n * bpf_u) for cross-party D/R pairs and 1.0 otherwise. The form is symmetric in the two members. `fec/moneyball_core.py:543-545` | The paper cannot test the sqrt-product form or the 0.4 floor. Context only. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: bpf symmetrization: the rel says every pair takes the mean of its two directed weights; the code uses the single directed weight when only one direction exists.
> - Summary issue: Offered and attracted ties cannot be recovered: overstated. The materialize output keeps source_directed_weights, so an attracted share is buildable from data on disk but is not built.
> - Summary issue: Later-stage effects: the Table A8-A9 DVs are counts of a member's bills at each stage, not stage rates, and not LES. The rel line that the LES outcome is the CEL score mislabels them.
> - Summary issue: Shadow checked against CEL LES: not found in the code read; treat as unsupported.
> - Summary issue: Majority/minority similar shapes: holds for the House. The Senate minority squared term (Model 3.6, -0.397) is not significant, so the Senate minority shape is not established.
> - Summary issue: Reciprocity halving: House pooled 0.626 to FE 0.317 roughly halves; Senate 0.739 to 0.547 does not. The rho .888 sentence is a non sequitur for the paper link.
> - Summary issue: Flip-model card: the claim that the component card says the code has no bonus was not checked, since the card text was not available. The 8-14 percent attribution to Volden-Wiseman is not in this paper.
> - Summary issue: Senate Table A9 note overstates the committee-stage result (A9.2 1.767 not significant). Confirmed as the rel flags.
> - Summary issue: Related links: aroyehun relation same-data changed to complements (shared LES outcome only). Dropped noble-2024-presidential-leadership-legislative: it models presidential going-public under polarization and does not study effectiveness, so the link is not genuine. Aref-2021 description reworded.
> - Summary issue: Paul alignment: the Bacik citation and the choice of LES for the shadow lane are not in this item (paul_entries is empty); unverified here.
> - Summary issue: Coverage: full text read for Tables 1, 3, 4, A1, A5, A6, A8-A13 and notes (Table 2 spot-checked); code read for all six code_refs plus the coop composite and shadow verifier. paul_markup empty, 0 dropped. key_findings not returned (schema has no field).

## Gaps and flags

- No ATTRACTED-direction bipartisan share exists anywhere in the system. bpf is undirected and symmetrized, and cooperation v2 c2 measures offered support. The paper ties only the attracted share to effectiveness.
- Every bpf consumer is monotone: the flip passage bonus (1+0.3 bpf) and the coalition seed boost (1+0.8 bpf). The paper finds an inverted U peaking near a 0.5 share in both chambers.
- The flip-model comment cites a Volden-Wiseman bonus of +8-14 percent, but this paper reports 6-8 percent of mean LES per SD. The component card says the code line has no bonus, yet flip_model.py:142 carries (1+0.3*bpf), so the card is stale.
- bpf is a single current-roster snapshot. The paper's effects are within member across Congresses, and the system has no per-Congress bipartisan series to test them.
- The effectiveness shadow has never been checked against bipartisan coalition composition, which is a ready convergent-validity test against CEL LES.

- The paper's reciprocity is cosponsorship exchange between members. It does not validate the money or competitiveness reciprocity in D-104..D-108, which stays NOT validated. Do not cite this paper for it.
- The paper controls for average cosponsors attracted in its regressions. That is not per-edge volume normalization and gives no support to the rejected per-edge normalization proposal.
- The evidence is observational with legislator FE and one-tailed tests, and it has no design-based identification, so it supports associational language only. The Table A9 note overstates the Senate committee-stage result (A9.2 is not significant).
- Coefficients are reported by chamber and by majority/minority strata. Quote them by chamber with the 1973-2016 window, never as one pooled number.
- bpf and any attracted-share variant are heuristic model outputs, not closeness or access probabilities (D-025).
- Transfer of any of these effects to Paul's current-roster data is UNVERIFIED until reproduced by a second path (DE-062).

Paul has no annotated entry for this work. The evidence agrees in direction with his bpf position, which cites Bacik on bipartisan teams earning more, and with his choice of LES as the effectiveness criterion for the shadow lane. It refines rather than contradicts both. The payoff is to bipartisan support attracted, not offered, and it is nonlinear. No Paul note is stale because of this paper. The flip-model comment and the component card describing it are out of step with the code and with the paper's magnitudes.

## Related papers

- [[sotoudeh-2024-network-based-measure]] — complements — Sotoudeh (2024) measures cosponsorship influence on House bill passage and contrasts it with the Volden-Wiseman LES. This paper uses LES as its outcome.
- [[battaglini-2018-effectiveness-connected-legislators]] — complements — Battaglini, Leone Sciabolazza and Patacchini model LES as a Katz-Bonacich centrality of the cosponsorship network. This paper uses the cross-party composition of cosponsors instead.
- [[bucchianeri-2025-legislative-effectiveness-american]] — same-method — Bucchianeri, Volden and Wiseman extend the Volden-Wiseman effectiveness framework to American state legislatures (SLES).
- [[aroyehun-2026-politics-of-feelings]] — complements — Aroyehun (2026) also regresses Congressional LES on a member-level predictor, emotional expression from floor speeches. The link is a shared outcome, not shared data.
- [[aref-2021-identifying-hidden-coalitions]] — complements — Aref and Neal (2021) partition House cosponsorship signed networks; their third coalition, marked by negative same-party ties, is the most effective coalition in most sessions. Their coalitions are signed-network partitions, not cross-party by definition.

### Linked from
_None._