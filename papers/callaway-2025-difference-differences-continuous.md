---
type: paper
title: "Difference-in-Differences with a Continuous Treatment"
authors: ["Brantly Callaway", "Andrew Goodman-Bacon", "Pedro H. C. Sant'Anna"]
year: 2025
venue: "arXiv 2107.02637v8 (econ.EM); working paper, peer-review status not stated in text"
doi: ""
category: "Causal & Methodological"
systems: [melange-influence-engine, mentat-sector-growth-study]
components: ["mentat-sector-growth-study.growth-spending-regressions", "melange-influence-engine.flip-model"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/callaway-2025-difference-differences-continuous.pdf]]"
original_paths: []
sha12: ["22e43da4fc06"]
tags: [paper, causal-methodological]
---

# Difference-in-Differences with a Continuous Treatment

**Brantly Callaway, Andrew Goodman-Bacon, Pedro H. C. Sant'Anna (2025).** *arXiv 2107.02637v8 (econ.EM); working paper, peer-review status not stated in text.* Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> Callaway, Goodman-Bacon and Sant'Anna (arXiv 2107.02637v8, 2025) work out what difference-in-differences identifies when treatment comes as a continuous dose rather than an on/off switch. Under ordinary parallel trends, only effects for the units that actually got dose d are identified (ATT(d|d), and ATT_loc pooled over treated units). Comparing outcomes across dose levels mixes the causal response with a selection-bias term (Thm 3.2). Causal responses such as ACRT(d), the derivative in dose, need strong parallel trends: the response to each dose must be the same across dose groups. Pre-period data cannot test that assumption. The common two-way fixed effects (TWFE) dose-by-post slope is a weighted average with non-obvious weights. In the levels form those weights integrate to zero and are negative below the mean dose (Thm 3.4). The slope also depends on the dose units and on whether untreated units are included. In the Medicare PPS hospital application (5,881 hospitals, 1980-1986), TWFE gives 1.14. That sits near ATT_loc = 0.80 only because two biases offset. ACRT_glob = -0.08 (s.e. 0.19), and dropping untreated units shrinks TWFE by 78 percent while the causal responses do not move. Estimation uses binarized DiD plus B-spline sieve regressions with honest uniform bands. The paper bears most on the Mentat sector-growth regressions, which put a continuous growth measure into sector-plus-year fixed-effects models with no zero-dose group. It also bears, as a design template, on the open question of whether money's effect is concave, which the Melange money terms assume but never estimate. Purely econometric. No lobbying, network or legislator content.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** How should difference-in-differences designs with a continuous or multi-valued dose be identified, interpreted and estimated? Which causal parameters are identified under standard parallel trends versus stronger assumptions, and what does the common two-way fixed effects (TWFE) regression with a dose-by-post interaction actually estimate?

**Theory:** Potential outcomes indexed by dose (Robins 1986). Distinguishes level treatment effects Y(d)-Y(0) from causal responses, the marginal derivative in dose (Angrist and Imbens 1995). Generalizes Fricke (2017) from two doses to continuous and multi-valued doses. Uses Yitzhaki (1996) linear-projection decomposition of regression slopes. The empirical interpretation of the derivative as an elasticity of substitution comes from a homothetic two-factor hospital production model following Acemoglu and Finkelstein (2008).

**Data:** Application: Acemoglu and Finkelstein (2008) AHA Annual Survey Database hospital panel. Balanced panel of 5881 hospitals; pre period averaged over 1980-1983 and post period over 1984-1986 (Sec 6, p.25). Dose M is the 1983 Medicare inpatient share; among treated hospitals mean 0.45, SD 0.15 (Sec 2, p.5). 4987 hospitals have a positive dose; 57 above 0.75, 20 above 0.80, 3 above 0.90 (Sec 6.2, p.28). Outcome is the depreciation share of total operating expenses as a capital-labor proxy; 1983 mean 4.5; untreated mean change 0.37 (Fig 4 note, p.27). Figure 1 uses 1980-1986 annual data and drops 860 of 6741 hospitals with missing outcomes. Policy: 1983 PPS set the labor subsidy to zero and left the capital subsidy unchanged.

**Method:** Two-period DiD with continuous dose, identified under parallel trends (PT, Assumption PT) and strong parallel trends (SPT, Assumption SPT). Estimators: (i) binarized DiD regression of the outcome change on an indicator for positive dose (eq 4.6), which recovers ATT_glob under SPT; (ii) saturated regression with dose dummies for discrete doses (eq 4.1); (iii) quadratic parametric regression among treated units (eq 4.2); (iv) nonparametric sieve regression with cubic B-splines and a data-driven Lepski-type sieve dimension (Chen, Christensen and Kankanala 2025), with honest uniform confidence bands (Appendix B). ACRT_glob is the sample mean of estimated ACRT(D_i) over treated units. TWFE is decomposed (Theorem 3.4) into causal response, levels, scaled levels and scaled 2x2 representations with explicit weights (Table 1). Extensions: staggered timing (Appendix C), relaxing SPT through bounds, sub-regions and covariate-conditional SPT (Sec 5). Pre-trend checks use event-study versions ATT_loc(e) and ACRT_loc(e). Inference clusters at the hospital level.

**Identification:** Under PT, ATT(d|d) = E[dY|D=d] - E[dY|D=0] and ATT_loc = E[dY|D>0] - E[dY|D=0] are identified, but comparisons across dose groups mix causal responses with selection bias (Thm 3.1, 3.2). Under SPT, ATT(d) and ACRT(d) are identified, and ATT_glob = E[dY|D>0] - E[dY|D=0] and ACRT_glob = E of the dose-density-weighted derivative of E[dY|D=d] (Thm 3.3, Cor 3.1). Without untreated units, the dose-group comparison to the lowest dose recovers differences only, and interpreting them causally requires strengthening PT (Remark 3.1). Pre-period data cannot test SPT (p.13-14).

**Scope:** Population: US hospitals in the Acemoglu and Finkelstein balanced panel subject to the 1983 Medicare PPS labor-subsidy change. Era: 1980-1986. The identification and estimation results apply to any two-period DiD with a continuous or multi-valued dose, and to staggered designs in Appendix C. The empirical content is a single application, not a general estimate.

## Key findings

- Under PT, ATT(d|d) and ATT_loc = E[dY|D>0] - E[dY|D=0] are identified (Thm 3.1, p.10). The main text makes clear that ATT(d) is not identified under PT alone.
- Under PT alone, comparing outcome paths across dose groups mixes a causal response with a selection bias term ATT(l|h) - ATT(l|l); causal responses are not identified (Thm 3.2, p.11).
- Under SPT, ATT(d), ACRT(d), ATT_glob and ACRT_glob are identified; ACRT_glob equals the average derivative weighted by the dose density among treated (Thm 3.3, Cor 3.1, p.12-14).
- The TWFE coefficient is a weighted integral of ATT(d|d) whose weights integrate to zero, with negative weights below the mean dose and positive above (Thm 3.4(b), p.16-17).
- Under PT, the TWFE causal-response decomposition is a convex average of ACRT(d|d) plus selection bias, with positive weights integrating to one (Thm 3.4(a), p.16). Positive weights do not make TWFE a natural target.
- Equation 3.1 writes beta_twfe as a Wald-type ratio with distance weights; in the application the numerator is 0.60 and the denominator 0.53 (p.17, p.28).
- Nonparametric ATT_loc = 0.80 (s.e. 0.05), about 18 percent of the 1983 mean outcome of 4.5; reject zero for doses 0.05 to 0.78, covering 96 percent of treated hospitals (Sec 6.2, p.27).
- Static TWFE coefficient is 1.14 (s.e. 0.099, Fig 1); it is close to ATT_loc only because two biases offset. Dropping untreated units shrinks TWFE by 78 percent while causal responses are unchanged (p.3, p.28, p.30).
- ACRT_glob = -0.08 (s.e. 0.19), not significantly different from zero; estimated ACRT(m) is negative for m above 0.41, covering 71 percent of treated hospitals (Sec 6.3, p.30).
- Post-period average of the ATT event study is 0.755 (s.e. 0.048) (Fig 8, p.30); post-period average of the ACRT event study is 0.220 (s.e. 0.170) (Fig 10, p.32).
- Pre-treatment ACRT event-study estimates reject the zero implied by no-anticipation plus parallel trends in 1981, though the violation is small relative to post-period effects (p.32).
- Implied average elasticity of substitution point estimates do not fit the positive-elasticity prediction, although the uniform bands do not reject zero; the authors treat this as evidence against SPT or the two-factor model (p.31-32).

## Limitations

- SPT is described as strong and perhaps implausible in many applications; the authors do not propose it as a default (p.13).
- Pre-trend tests cannot distinguish PT from SPT, because pre-period outcomes are untreated potential outcomes (p.13-14).
- Event-study pre-trend tests may miss PT violations; aggregate PT allows violations that average to zero across dose groups, and whether that should count as evidence against the design is debated (p.24-25).
- Quadratic parametric estimates are precise only if the functional form is correct; pretesting parametric against nonparametric fits can mislead, so the authors use honest nonparametric bands (p.28-29).
- Nonparametric precision is limited where doses are sparse, with only 57 treated hospitals above dose 0.75 (p.28).
- Footnote 12: the sieve dimension used for ACRT_glob is treated as non-random; the randomness of the Lepski-type selection is left for future research (p.30).
- A Neyman-orthogonal representation for ACRT_glob suggests double machine learning, which the paper leaves for future research (p.22).
- Without untreated units, ATT(d|d) and the levels decompositions are not identified; only dose-group differences are identified (Remark 3.1, Remark 3.2, p.14-15).
- The 1983 pre-PPS event-study coefficient may reflect partial 1983 PPS effects and fiscal-year reporting (footnote 11, p.29).
- The elasticity interpretation assumes no anticipation and a homothetic two-factor production model; the authors note alternatives such as a three-factor or non-homothetic model could rationalize the negative ACRT (p.31-32).
- Formal estimation for the staggered case is not provided; the authors say disaggregated results apply directly and that binarized event-study procedures can be used when ATT_loc(e) is the target (Remark C.1, p.42).
- TWFE beta depends on the unit of dose: recoding the dose from 0 to 100 shrinks beta from 1.14 to 0.0114 while ATT_loc is unchanged (p.28).

## Short quotes

- "Strong parallel trends may not be plausible in many applications."
- "comparing these parameters across treatments is challenging because parallel trends does not rule out selection bias."
- "a researcher confusing a small ACRT for an ATT would mistakenly conclude that a policy was ineffective."

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | qualifies | The TWFE slope on a continuous dose is a weighted integral of ATT(d\|d). In the levels form the weights integrate to zero and are negative for doses below the mean dose (Thm 3.4b). Under PT the causal-response form is a convex average of ACRT(d\|d) plus a selection-bias term (Thm 3.4a). With no untreated units only differences relative to the lowest dose are identified (Remarks 3.1, 3.2). Application: TWFE 1.14 (s.e. 0.099), ATT_loc 0.80 (s.e. 0.05), ACRT_glob -0.08 (s.e. 0.19); dropping untreated units shrinks TWFE by 78 percent; recoding dose from 0-1 to 0-100 shrinks it to 0.0114. (Sec 3.3 Thm 3.4 and Eq 3.1 (p.15-17); Remarks 3.1-3.2 (p.14-15); Sec 6.2-6.3 (p.27-31); Appendix C staggered extension) | sgs_registry.R:31 defines the growth regressors (growth_previous_year_pct, growth_same_year_pct, predicted_growth_pct). Robustness tests R-yearFE-* and R-logFE-* replace the national controls with sector plus year fixed effects (sgs_registry.R:664-671). Sector-only fits with year effects are labelled UNSTRATIFIED because no sector-only estimate exists (sgs_estimate.R:254-255, reason at 385-398). The primary specifications were not fully traced here, so sector-plus-year is confirmed only for the robustness variants. In the latest run (summaries_analysis-R-20260926T221444Z.txt) the L1-LV year-FE growth coefficient is -0.0014 (s.e. 0.123) with adjusted R2 0.8863 with the term and 0.8874 without it. The claim that the lobbying lead flips sign under year FE was not checked. (`C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R:31; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R:664-671; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:254-255; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:385-398; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_report.R:593`) | The paper's formal results cover two-period and staggered absorbing doses. Sector growth is non-absorbing and changes every year, so the decomposition does not carry over exactly; the de Chaisemartin and D'Haultfoeuille estimators the paper cites fit that case. The qualitative lesson transfers: a null or sign-changing pooled growth slope under FE can reflect implicit weighting across sector-years and heterogeneous sector responses, not only zero response. Window and denominator: sector-years in each test's gated sample. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | suggests-extension | A dose-response curve ACRT(d) is identified only under strong parallel trends; cross-dose comparisons otherwise carry selection bias ATT(l\|h) minus ATT(l\|l) (Thm 3.2). In the application the nonparametric ACRT(m) point estimates are negative for m above 0.41, covering 71 percent of treated hospitals, but the 95 percent uniform band covers zero everywhere, so only pointwise sign changes are reported. ATT_loc stays positive. A quadratic functional form is precise only if it is correctly specified. (Thm 3.2-3.3 and Cor 3.1 (p.11-14); Sec 4 (eq 4.1, 4.2, 4.6); Sec 6.3 (p.30-31); Appendix B) | flip_model.py:33-34 sets HALFF default 0.10. pflip (flip_model.py:92-96) uses (cm*cu+1)/((cm*cu+1)+HALFF*R). money_response.py:15-16 sets MAXSHIFT 18 and HALF 0.10. shift_lean (money_response.py:68-74) uses MAXSHIFT*(r/(r+HALF)) with r = amount/receipts. Code comments describe these as judgment dials calibrated against Ansolabehere and Fowler-Garro-Spenkuch, so 'literature priors' is a fair description. Neither file estimates a dose-response from panel data. The statement that the component's own open question asks about concavity was not checked. (`C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/engine/flip_model.py:33-34; C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/engine/flip_model.py:92-96; C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/engine/money_response.py:15-16; C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/engine/money_response.py:68-74`) | A test would need a member panel: votes or stance before and after giving, an unfunded zero-dose comparison group, and dose as share of receipts. PAC dose is plausibly selected on expected response, so strong parallel trends is doubtful and any curve beyond ATT_loc would carry selection bias. This is a design template for a future test, not evidence on the current HALF or MAXSHIFT values. Any result stays UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The summary says the Mentat growth regressions use sector-plus-year fixed effects. Year FE appear only in the robustness variants R-yearFE and R-logFE (sgs_registry.R:664-671); the primary specifications were not fully traced here.
> - Summary issue: All other summary numbers check against the text: 5881 hospitals, 1980-1986, TWFE 1.14, ATT_loc 0.80, ACRT_glob -0.08, 78 percent shrinkage, 0.0114 recoding, and the Thm 3.4b sign pattern. Purely econometric scope is correct, with no lobbying, network or legislator content.

## Gaps and flags

- No estimated money dose-response anywhere: the saturating shapes in flip_model.py and money_response.py are priors, not fitted from a before/after panel with an unfunded comparison group.
- Sector-growth fits report a pooled growth slope but not the implicit weights each sector-year gets (the Eq 3.1 Wald-type numerator and denominator). So a null cannot be told apart from offsetting heterogeneous sector responses.
- Sector growth has no zero-dose group. Under the paper's Remark 3.1, any growth contrast is identified only relative to other growth levels, not relative to no growth.
- Growth is non-absorbing and time-varying. The study has no estimator built for switching continuous treatments (e.g. de Chaisemartin-D'Haultfoeuille style), only TWFE and first-difference variants.
- No conflict with brain rules. The sector-growth code already labels year-FE pooled slopes UNSTRATIFIED (sgs_estimate.R:250-256) and frames results as associations only (sgs_report.R:593), as the pooled-coefficient rule requires.
- The paper's application is hospitals under Medicare PPS, not political spending. Any transfer to giving or legislator behaviour is a design lesson, not an effect size.
- No Paul annotations exist for this work (annot_sha12s empty; paul_entries empty), so there is no stated takeaway to check against.

Paul has no entry or markup for this paper, so there is no takeaway to compare. His sector-growth position cites the Imai and Kim two-way fixed effects caution. This paper extends that caution from binary to continuous treatment and agrees with it: TWFE slopes on a dose are weighted mixtures that need strong parallel trends for a causal-response reading. The system already treats these slopes as non-causal associations and reports a null growth effect. No current note is made stale; the paper gives a sharper reason not to read the null as no effect.

## Related papers

- [[imai-2021-fixed-effects-regression]] — complements — Imai and Kim (Political Analysis 2021) critique two-way fixed effects with panel treatments; this paper gives the continuous-dose analogue of that caution. Item exists and is linked to the same sector-growth component.
- [[mackinnon-2017-wild-bootstrap-inference]] — complements — Covers the wild cluster bootstrap inference layer the sector-growth study uses with few clusters; this paper covers what the point estimate means, not inference. Item exists; MacKinnon text not re-read here.
- [[kalla-nd-campaign-contributions-facilitate]] — complements — Kalla and Broockman (AJPS 2016) is a randomized field experiment on whether contributions buy access, the experimental counterpart of the observational money dose-response design discussed here. Confirmed from the item's paper record.
- [[sverdrup-nd-introduction-grf-causal-forests]] — complements — The grf vignette is an introduction to generalized random forests for heterogeneous effects; this paper points to Neyman-orthogonal and double machine learning routes for ACRT_glob as future work (p.22). Item exists; grf text not re-read here.
- [[ansolabehere-2002-little-money-politics]] — complements — The money_response.py calibration comment cites Ansolabehere for the small MAXSHIFT, so the link is real in the code. The Ansolabehere paper text was not re-read here.

### Linked from
_None._