---
type: paper
title: "The Effects of Congressional Staff Networks in the U.S. House of Representatives"
authors: ["Jacob M. Montgomery", "Brendan Nyhan"]
year: 2017
venue: "Journal of Politics (text reads: Forthcoming, Journal of Politics)"
doi: ""
category: "Social Network Analysis"
systems: ["melange-legislative-data", "melange-staffer-network"]
components: ["melange-staffer-network.overlap-edge-rule", "melange-staffer-network.overlap-edge-rule", "melange-staffer-network.access-uncertainty", "melange-legislative-data.effectiveness-shadow"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/montgomery-2017-effects-congressional-staff.pdf]]"
original_paths: []
sha12: ["a67b202a3c29"]
tags: [paper, social-network-analysis, type/paper, theme/social-network-analysis, evidence/verified]
---

# The Effects of Congressional Staff Networks in the U.S. House of Representatives

**Jacob M. Montgomery, Brendan Nyhan (2017).** *Journal of Politics (text reads: Forthcoming, Journal of Politics).* Theme: [[themes/social-network-analysis|Social Network Analysis]].

> [!abstract] Summary
> Montgomery and Nyhan ask whether House members linked by shared personal staff become alike in legislative effectiveness (Volden-Wiseman LES) and roll-call ideology (CJR ideal points). A tie between two members is the count of senior or policy staffers who worked for both offices in the current or previous two Congresses, row-standardized, over the 105th-111th Congresses (435-446 members per Congress). They fit a Bayesian heteroskedastic spatial autoregressive model per Congress and pool the seven estimates. Senior-staff ties show a small positive LES lag (pooled rho 0.043, 95% CI 0.0095 to 0.096; positive in 5 of 7 Congresses). Adding policy staff weakens it (0.013, CI -0.015 to 0.037). For ideology, senior ties show nothing (-0.003). Senior plus policy ties show a small positive lag (0.028, CI 0.013 to 0.047), but covariates already explain R2 above 0.88. Placebo tests help only partly. Lower-level staff ties give a null LES lag. Future senior ties (CI -0.033 to 0.071) and the lower-level ideology placebo overlap the headline estimates, so homophily is not ruled out. The authors conclude that staff may act as extensions of their members. For Paul's systems the paper is most useful as a comparison for the staffer network. Montgomery's tie is an office-to-office sequence link, which the system already emits as directed office moves and shared_with counts. The system deliberately does not treat that link as a person-to-person edge. In this paper the link carries only a weak signal of similarity, and the signal is partly confounded by homophily. It supports showing these counts as descriptive data. It does not support treating them as transmission channels. All findings are observational and House-only. The window is 1997-2010 and excludes committee staff.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Do members of Congress who exchange senior or policy staff across offices become more similar in legislative effectiveness and in roll-call ideology than chance or observed covariates would predict? Secondary: do staff ties transmit expertise and positions across legislators, as opposed to homophily in staff hiring?

**Theory:** Staff are not mere extensions of members (p.3-4). Two mechanisms (p.7-8): (1) a staffer carries knowledge, relationships with party leaders and expertise from a prior boss to a new boss; (2) staff who move between offices facilitate coordination between the two offices. Hypothesis 1: linked offices are more similar in legislative effectiveness. Hypothesis 2: linked offices vote more similarly. Senior staff are expected to matter for effectiveness; policy staff for roll-call voting (p.9).

**Data:** CQ First Street personal-staff records for House members, digitized from CQ Press directories 1993-2009 and updated after (p.10). Subset to personal staff, 103rd-111th Congresses; staff ties built for the 105th-111th Congresses. Unit: member-Congress. 3,973 member-Congress observations in the period; staff data missing for 112 (2.8%) (p.11). Regression N per Congress 435-446 (Tables 1-4). Outcomes: Volden and Wiseman (2014) legislative effectiveness scores (LES, range 0-18.7, mean 0.996, SD 1.526 in the 105th-111th); Bayesian one-dimensional ideal points from Clinton, Jackman and Rivers (2004), estimated with pscl on Voteview roll calls (Online Appendix). Committee staff excluded because majority/minority status and member links are unreliable (p.31; Online Appendix). Descriptives: 12 percent of senior/policy staff held such a post for two or more House members, 44 percent among those serving five or more Congresses (p.13); fewer than 50 of 12,938 senior/policy staffers crossed party lines (p.14).

**Method:** Bayesian heteroskedastic spatial autoregressive model with covariates, y = X beta + rho W y + e, with e having variance sigma^2 V (diagonal scalars, Student-t-like errors, r = 4), estimated in Matlab with sar_g (LeSage and Pace 2009) (p.17-18). W is a row-standardized, symmetric count of staff who worked for both members in the current or previous two Congresses (p.18). Separate cross-sectional model per Congress, pooled across Congresses with the Gelman et al. (2004) procedure because W changes over time (p.19). Direct, total and indirect effects from the (I - rho W)^-1 expansion (p.20). Two networks: senior-only ties and senior-plus-policy ties. Controls: seniority, majority party, female, African American, Hispanic, committee chair, subchair, majority and minority leadership, power committee (LES); Democrat, female, race and ethnicity, district presidential vote and its square (ideology). Falsification: placebo ties from lower-level staff and from future senior ties (p.29-30).

**Identification:** Observational. Identification rests on the spatial lag rho capturing outcome similarity among members linked by staff moves, net of covariates. No design isolates an exogenous staff shock. Falsification tests are used to probe homophily: lower-level staff ties and future senior ties, which should not causally matter (p.29-30). The authors state homophily cannot be fully ruled out (p.30). Causal language for race is acknowledged as problematic (p.28, footnote 34).

**Scope:** U.S. House only; personal senior and policy staff only; committee staff excluded; 105th-111th Congresses (roughly 1997-2010 by calendar arithmetic; text does not give years for these Congresses). Explicitly does not test the 1970s-era 'textbook Congress' (p.31).

## Key findings

- Senior-staff ties, LES: pooled rho = 0.043, 95% CI [0.0095, 0.096]; positive in 5 of 7 Congresses but not each reliably above zero (Table 1, p.22-23).
- Senior plus policy ties, LES: pooled rho = 0.013, 95% CI [-0.015, 0.037]; authors call it consistent with expectations but not conclusive (Table 2, p.24).
- Senior-staff ties, ideology: pooled rho = -0.003, 95% CI [-0.029, 0.020]; no reliable effect on roll-call ideal points (Table 3, p.26).
- Senior plus policy ties, ideology: pooled rho = 0.028, 95% CI [0.0129, 0.0465]; reliably positive in the 109th and 110th Congresses per text (Table 4, p.27).
- Ideology R2 above 0.88 in every Congress, so the tie effect is small against the covariates (p.27-28).
- Committee chair direct effect on LES 1.853, 90% CI [1.376, 2.352]; indirect effect via ties 0.442, 90% CI [0.131, 0.826] (p.24).
- Being African American, 109th Congress ideology: direct effect 0.176, 90% CI [0.042, 0.317]; indirect effect via ties 0.0093, 90% CI [0.0003, 0.0238] (p.28).
- Falsification, lower-level staff ties on LES: pooled rho about -0.016; text gives CI [-0.070, 0.027], which excludes the 0.043 headline point (Table 5 p.30; text p.29).
- Falsification, future senior ties on LES: pooled CI [-0.033, 0.071] overlaps the headline estimate, so homophily cannot be excluded (p.29).
- Falsification, lower-level ties on ideology: pooled 0.006, CI [-0.016, 0.067] overlaps the Table 4 estimate (p.29).
- Robustness (Online Appendix Table A1): no-covariate and committee-fixed-effects models keep the LES pattern for senior ties; two-Congress window leaves the senior LES interval barely including zero (-0.0006, 0.1088).
- Authors conclude staff may act as extensions of members (Kingdon 1973) and that the ideology link is partly homophily (p.30-31).

## Limitations

- Cannot test long-term changes in staff roles or the textbook Congress era (p.31).
- House only; Senate not studied (p.31).
- Committee staff excluded; their effects untested (p.31; Online Appendix).
- Imperfect outcome measures, LES and ideal points (p.31).
- No multiplex analysis with cosponsorship or other dyadic ties (p.31).
- Homophily between staff and members not fully ruled out; future designs needed (p.31).
- Staff missing for 2.8 percent of observations, and some members repeatedly fail to match (Online Appendix).
- Covariate choice affects inference; committee dummies removed power committee for collinearity (Online Appendix).

## Short quotes

> staff may help serve as extensions of members

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-staffer-network#Concurrent-service OVERLAP edge rule\|Concurrent-service OVERLAP edge rule]] | supports | The tie is a member-member count of senior or policy staffers who worked for both offices in the current or previous two Congresses, undirected and row-standardized (p.11, p.18). Senior-tie LES pooled rho is 0.043, 95% CI 0.0095 to 0.096; senior-only ideology is -0.003, CI -0.029 to 0.020. The future-senior placebo CI of -0.033 to 0.071 overlaps the headline, so homophily is not excluded (p.29). (p.11 (W as undirected count, fn 12); p.18 (row-standardized, 3-Congress window); Table 1 p.23 (pooled 0.043); Table 3 p.26 (ideology -0.003); Table 5 p.30 (placebos)) | office_moves are directed consecutive-span pairs, emitted with SEQUENCE IS NOT A SOCIAL EDGE and canonical_eligible False. The paper's tie is not a sequence link: it is the undirected co-service count, whose closest analogue in the system is shared_with / pair_people, which has no window. The rule against treating sequence as a person-person edge is still consistent with the paper's caution about homophily. `audits/staffer_network/build_staffer_social_network.py:2544-2550 (SEQUENCE comment); :2630-2641 (note, canonical_eligible False). Verified. shared_with path: fec/surfaces/build_staffer_network_section.py:1239-1242, 1253-1254` | House personal offices, 105th-111th Congresses; observational; member-level outcome similarity, not transmission. The text gives Congress numbers only, so the 1997-2010 window is a calendar inference, not the paper's statement. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-staffer-network#Concurrent-service OVERLAP edge rule\|Concurrent-service OVERLAP edge rule]] | qualifies | Only senior and policy staff carry signal. Lower-level staff ties are a placebo with a null LES lag (pooled -0.016, text CI -0.070 to 0.027). Adding policy staff to senior ties weakens the LES lag (pooled 0.013, CI -0.015 to 0.037; Table 2 p.24). (p.4 fn 2 (senior and policy title definitions); p.24 Table 2; p.29 text (placebo CI); p.30 Table 5) | shared_staff counts people attached to two or more member offices, with no title filter at the count stage and no time window. SHARED EMPLOYEE and intern titles are dropped only from the top_staff eligible list. shared_with takes top partners from pair_people by count, with no window. `fec/surfaces/build_staffer_network_section.py:160-162 (SHARED_STAFF_NOTE); :297-298 (excluded titles); :1239-1242 (pair_people to shared_partners); :1251 (shared_staff); :1258-1266 (title exclusion applies to eligible top-staff list). Verified.` | Applies if shared_with or shared_staff is ever read as a member relationship signal. The paper's senior-only and 3-Congress specification is the comparison; shipped counts are descriptive only. | moderate | VERIFIED |
| [[systems/melange-staffer-network#Recency and access-uncertainty factor\|Recency and access-uncertainty factor]] | context | Ties built over the current plus two prior Congresses carry the LES signal. Narrowing to a two-Congress horizon leaves the senior LES interval barely including zero (-0.0006 to 0.1088; 94% interval 0.0016 to 0.1066). This is window sensitivity, not a decay estimate. (p.18 (window); Online Appendix Table A1, Models 3-6 and the note on the two-congress horizon) | Access-uncertainty cost uses a fixed 5-year staleness half-life on years since last joint overlap (staleness = ln2 * age / 5). The file labels its calibration as 'Exploratory log-cost scenarios; not fitted success or friendship probabilities', and the docstring calls the costs assumptions that must not be shown as probabilities. `fec/staffer_network/relationship_uncertainty.py:10-14 (constants); :36-38 (staleness term); :49-55 (specification; calibration label at :54). Verified.` | Weak. The paper's window concerns member-member outcome similarity, not whether staffer-staffer relationships remain usable. It cannot calibrate the half-life. | weak | VERIFIED |
| [[systems/melange-legislative-data#Five-stage legislative effectiveness shadow and CEL criterion\|Five-stage legislative effectiveness shadow and CEL criterion]] | suggests-extension | Members sharing senior staff have modestly correlated LES net of covariates (pooled rho 0.043). Committee chair direct effect is 1.853 (90% CI 1.376 to 2.352); indirect effect via ties is 0.442 (90% CI 0.131 to 0.826). (Table 1 p.23 (pooled 0.043); p.24 text (chair direct 1.853, indirect 0.442)) | The five-stage shadow score (BILL, AIC, ABC, PASS, LAW) in the cited engine file is a versioned illustrative candidate policy that requires calibration. The claim that it is validated against CEL LES for ceremonial share only is not in the cited engine file; the ceremonial-share and CEL work sits in fec/members/lep_criterion.py. I found no network-similarity check in the cited files. `fec/engine/legislative_effectiveness.py:1-31 (five stages; ScoringPolicy labelled illustrative, needs calibration). Validation support is in fec/members/lep_criterion.py:3-8, :31, :59, :83 (ceremonial share on CEL data), a different file from the one cited.` | Optional second criterion: shadow scores should show a similar small lag on senior-title ties if they track LES. This is UNVERIFIED, needs office_moves restricted to senior titles, and is not evidence that the shadow score is valid. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Calls Montgomery's tie an office-to-office sequence link that the system emits as directed moves. The paper's W is an undirected, row-standardized co-service count over the current and two prior Congresses (p.11, fn 12, p.18). Sequence is the theory, not the operational tie.
> - Summary issue: States the window as 1997-2010. The text gives only the 105th-111th Congresses and never states years; 1997-2010 is calendar arithmetic.
> - Summary issue: Says the senior ideology result shows nothing (-0.003). Strictly, the paper says only that it is not reliably distinguishable from zero (95% CI -0.029 to 0.020); Table 3 has 4 of 7 Congress estimates positive, so the summary's flat reading is a fair shorthand but not a finding of no effect.

## Gaps and flags

- No member-level staff-sharing measure matches the paper's specification (senior/policy titles only, current plus two prior Congresses, row-standardized). shared_with counts every shared person with no window, so a published-style replication on the system's payroll data is not possible as shipped.
- No check exists for homophily versus transmission on staff-move links (for example, a future-ties placebo). Any use of office_moves beyond description lacks a falsification design.
- Committee staff are excluded in the paper, but the system's co-tenure layer gives committee containers kappa 0.75/0.60/0.40. No literature here addresses committee-staff ties.
- The paper does not test multiplex combination with cosponsorship (its own limitation, p.31). Whether staff-move ties add information beyond the cosponsor kernel edges is untested in the system.
- Causal wording (effects, indirect effects transmitted through ties) outruns the design. Placebos do not exclude homophily (p.29-31). If cited in Melange, staff-sharing ties must not be described as influence, access or introduction channels (D-025). The paper's figures are similarity estimates on 1997-2010 House data.
- Pooled rho values pool seven per-Congress estimates spanning a majority flip (110th). Quote them with the per-Congress strata in Tables 1-4, not as a single pooled coefficient.
- The source text is internally inconsistent. p.19 says 105th to 112th Congress while the data end at the 111th. p.27 calls the 110th ideology rho reliably positive, but Table 4 gives 0.024 (SE 0.021). The lower-level ideology placebo point 0.006 sits oddly inside its interval [-0.016, 0.067].
- The row-standardized W is a SAR convention (per-row normalization), not the rejected per-edge volume normalization. The paper does not argue for either rejected SNA proposal.
- The paper cites Drutman 2012 and Bertrand et al. 2014 on staff becoming lobbyists but does not analyze revolving-door edges. Nothing here licenses influence language on revolving-door edges or the use of LDA as an edge source (D-021).

No Paul entry for this work. His staffer position (staffers as gatekeepers, professional ties as conduits) is tempered here. Shared senior staff predict only small LES similarity, ideology barely moves beyond covariates, and homophily is not excluded. The system as built agrees: sequence stays an office-move property, never a person-person edge, and shared-staff counts are descriptive. His overlap note is not stale; this paper adds the empirical reason sequence links should stay descriptive.

## Related papers

- [[haythornthwaite-1996-social-network-analysis]] — complements — Vault item exists; title confirms information-exchange framing of ties. Montgomery does not cite it, so the link is conceptual only.
- [[sotoudeh-2024-network-based-measure]] — complements — Vault item exists; text contrasts its cosponsorship influence measure with LES, so the contrast is real. Its outcome is bill passing, not LES.
- [[bucchianeri-2025-legislative-effectiveness-american]] — same-method — Vault item exists; applies the Volden-Wiseman LES framework to American states, the same outcome method with a different population.
- [[aroyehun-2026-politics-of-feelings]] — same-method — Vault item exists; uses Volden-Wiseman LES as outcome, but the same underlying data was not verified, so same-data is downgraded.
- [[bacik-2025-measuring-dynamical-evolution]] — complements — Vault item exists; title confirms a US lobbying network over time. Montgomery does not model lobbying, so the bracketing claim is conceptual.
- [[furnas-2019-moneyed-interests-information]] — complements — Vault item exists; the survey experiment includes junior and senior congressional staff, which fits the staff-as-information-processor framing.

### Linked from
- [[battaglini-2018-effectiveness-connected-legislators]] — complements — Item exists (title match, staff networks in the House). It is a network source less endogenous than cosponsorship, as the rel says.