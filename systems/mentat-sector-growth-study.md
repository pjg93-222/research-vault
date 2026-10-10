---
type: system
name: Mentat sector growth vs political spending study
slug: mentat-sector-growth-study
components:
  - mentat-sector-growth-study.growth-spending-regressions
  - mentat-sector-growth-study.seats-weekly-test
brain_notes:
  - topics/2026-09-feature-sector-growth-study.md
paper_count: 17
tags: [system]
---

# Mentat sector growth vs political spending study

An exploratory R regression family in C:/Users/pgoff/Mentat/sector_growth_study. It relates industry growth, election timing and expected party control to giving, partisan share and lobbying, and adds the 2022 weekly seat-gain test (SEATS-W). Results are reported trend-first.

Brain notes: [topics/2026-09-feature-sector-growth-study.md](obsidian://open?vault=brain&file=topics%2F2026-09-feature-sector-growth-study)

## Mentat sector growth vs political spending study: evidence assessment

**System.** An exploratory R regression family in `C:/Users/pgoff/Mentat/sector_growth_study`, with two components:
- **Growth-spending regressions.** The C1-C3 contribution tests, the L1-L3 lobbying tests and the SHIFT, ASYM, CONTRACT and DECEL extensions.
- **SEATS-W.** A weekly test of whether seat gains move giving, run on the 2022 House only.

**Evidence base.** There are 37 bearings (9 VERIFIED, 28 VERIFIED-SOFTENED). The plain VERIFIED rows are the project's own design memos and the Codex review, which describe the study rather than test it; one HC formula context row; rows on the 2024 Cook table; and the Furnas partisan-ties paper. The external paper bearings are mostly VERIFIED-SOFTENED, and each one transfers only partly.

**What the literature does and does not support.**
- The returns-to-lobbying papers (Huneeus & Kim 2018, Bickmore et al. 2025, Bombardini & Trebbi 2020, Alexander et al.) are firm-level and cross-sectional (between firms). They say nothing direct about whether a sector's growth predicts its own spending over time within that sector.
- The panel-method papers (Imai & Kim 2021, Blackwell et al. 2026) argue against reading any of the fixed-effects slopes causally.
- The variance papers (the Zeileis set) describe formulas the code matches. They also warn that clustered standard errors are too small at roughly 10 clusters. The study has 9 listed sectors, and fewer in some fits.

**Headline numbers, read with care.**
- **L3-LV slope.** The main recorded result is +0.352 (n=77, sector baselines plus national controls). It flips to -0.477 under sector + year FE, is -0.006 in the log-FE fit, and is -0.537 in first differences (FDY, 71 adjacent pairs). Adding growth changes adjusted R2 only from 0.884690 to 0.884717. These are hard-coded reference values from the Codex review of run analysis-R-20260926T070630Z, a single analysis path, so UNVERIFIED under DE-062. The pooled slope is UNSTRATIFIED unless the per-sector SLOPES are quoted beside it.
- **SEATS-W.** The recorded result is -3.47 pp in the Democratic share of giving per +1 expected Democratic seat (2022 House, 17 weeks, 538 Lite). That figure is the equal-sector variant. The pooled-dollar primary row is -3.36 pp with block-bootstrap p 0.11 / 0.10, not significant. The project's own report calls it effectively a one-election test.

**Two open contradictions.**
1. The build departs from both design memos: separate growth timings rather than one distributed lag, dollar levels rather than logs, and national controls rather than year FE. Results cannot be reported as tests of the memo hypotheses.
2. The brain note reports the SEATS-W House result without saying which weighting was used. That hides the fact that the pooled-dollar version is null.

**Paul's positions.**
- His caution on the growth regressions is better supported than his annotations' ROI reading. The evidence backs "no stable growth effect" and the 2FE caution.
- His SEATS-W citations rest on two papers that are not on disk (Cox & Magar 1999, Snowberg, Wolfers & Zitzewitz 2007). The Furnas $40k figure is verified, but it measures a different outcome (contract-lobbying revenue after control actually changed), not giving shares responding to forecasts.

## Sector growth vs giving, partisan share and lobbying regressions

**What it computes.** log giving ~ sector FE + growth(t-1, t) + phase + tossup + trend; RShare ~ growth + Shift (P - RMajNow); log lobbying with an AR term.

**Method.** Wild sector-cluster bootstrap with Holm adjustment; two-way cluster SE as sensitivity; FD sensitivity. Results: no stable partisan shift (-0.04, p .76); the lobbying lead flips sign under year FE; growth adds no adjusted R2.

**Code refs.** `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R`, `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_review.R`

**Assumptions.**
- Lagged growth predicts giving
- Expected control is known at the time (retrospective for 2023/2025)

**Evidence.** 27 rows (6 VERIFIED, 21 VERIFIED-SOFTENED); 16 distinct papers/notes.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[report-2024-2024-house-glance]] | 2024 | measurement-source | Dated Cook ratings for all 435 2024 House races on a four-step scale (Solid, Likely, Lean, Toss Up): per-row totals Solid D 174, Solid R 192, Likely D 16, Likely R 11, Lean D 13, Lean R 5, Toss Up 24 (sum 435). One source quirk: the MA-01 challenger slot (line 1120) holds a broken #REF! cell. | Valid as the Cook input for the 2024 cycle only, and only for analysis windows after 2024-09-12. The #REF! cell should be handled before ingestion. Whether this table is the source the study actually used is not established. | moderate | VERIFIED-SOFTENED |
| [[imai-2021-fixed-effects-regression]] | 2021 | qualifies | The 2FE estimator equals a matching estimator whose adjustment set has \|A_it\| = \|M_it\| x \|N_it\| and can contain same-status mismatches, so its simultaneous adjustment for unit and time confounders 'critically relies upon the linearity assumption'. Unweighted 2FE is not in general equivalent to the multi-period DiD estimator; the equivalence is the commonly cited justification from the basic two-group, two-period case. | Transfers in part. The paper covers a binary treatment only (its conclusion says so); the growth regressors here are continuous, so the mismatch and negative-weight algebra does not carry over directly. The general point does: a 2FE slope is a model-based weighted contrast whose causal reading depends on linear additivity, and one-way FE, two-way FE and first-difference fits target differently weighted contrasts. Extension to continuous treatments rests on the TWFE literature the paper cites, not on this paper. The panel is small (77 rows on the L3 base fit). The paper supports the study's existing caution (year FE as a check, no causal language); it does not make the year-FE coefficient the 'true' effect. | strong | VERIFIED-SOFTENED |
| [[imai-2021-fixed-effects-regression]] | 2021 | suggests-extension | The multi-period DiD estimator, where units switch in and out of treatment, equals a weighted 2FE estimator with weights W_it; some control observations receive negative weights when (2X_it - 1)(2X_i't' - 1) = -1, so unweighted 2FE is not the DiD estimator. The wfe R package implements the weighted estimators. | DECEL has the paper's binary, reversible treatment shape, but it is fit with sector baselines only, not two-way FE, so Theorem 1's 2FE/DiD negative-weight result does not apply as written; the one-way unit-FE matching result is in Imai and Kim (2019), which this paper extends. It would apply directly only if a sector + year FE DECEL variant were added. DECEL is also built from the growth regressor itself, not an assigned treatment. Any signed implicit-weight diagnostic or wfe/PanelMatch comparison would be new analysis: UNVERIFIED until a second path reproduces it (DE-062), EXPLORATORY under the original-model contract. | weak | VERIFIED-SOFTENED |
| [[blackwell-2026-effect-political-advertising]] | 2026 | qualifies | Unit fixed-effects regressions are biased when treatment and time-varying covariates feed back on each other (citing Imai & Kim 2019 and Sobel 2012). The paper's remedy moves the fixed effects into a propensity model and requires a binary treatment that switches within units, a correctly specified parametric propensity model, and N and T growing together. Simulations use n of 200-3000 with T of at least 4; in the empirical panel, campaign windows run 8-40 weeks. | Only the diagnosis transfers, not the estimator. The study's regressor is continuous sector growth with few sector clusters and annual T, so IPTW-FE cannot run: no binary within-unit-switching treatment and no feasible propensity model. Read the paper as support for not interpreting the L-LV coefficients causally. A sign flip under added year FE is consistent with feedback or time-varying confounding, so neither sign is causal. Applies to any sector-year panel where lobbying or giving is the outcome of a growth regressor that may itself respond to past spending. | moderate | VERIFIED-SOFTENED |
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | qualifies | Huneeus and Kim (2019; LobbyView-Compustat): 766 of 7,646 US public firms lobbied in 2017; lobbying firms have almost four times the revenue of non-lobbying firms; conditional on lobbying, the elasticity of lobbying to sales is around one. Using committee-assignment shocks for identification, a 10% increase in lobbying produces a 3% increase in revenue. | The review's figures are firm-level cross-sectional size elasticities and a firm-level revenue effect. They do not predict a within-sector growth-to-spending effect once year shocks are absorbed, so a sector-year null does not conflict with them. They should not be cited as support for a deep-pockets hypothesis at sector-year grain. | moderate | VERIFIED-SOFTENED |
| [[furnas-2019-partisan-ties-lobbying]] | 2019 | context | Firm ties to the House majority are worth approximately $5,000 to $6,000 per lobbyist per quarter in the six levels models (firm random effects, year fixed effects, 2008-2016, N = 33,243 firm-quarters, 1,603 firms); first-differences models give $1,800 to $5,000. Senate effects appear only with the time-fixed measure in the levels models and not at all in first differences. | The outcomes differ: the paper measures who earns lobbying fees, not where giving goes, and the separate Furnas et al. paper on lobbying/PAC overlap reports minimal overlap between lobbying and campaign-spending organizations. A null SHIFT preview is not contradicted by a lobbying-firm revenue premium. If SHIFT is ever extended to lobbying, key it to realized House control with chambers stratified. | weak | VERIFIED-SOFTENED |
| [[huneeus-2018-effects-firms-lobbying]] | 2018 | qualifies | Lobbying raises firm revenue. Table 2 (printed p.26): IV log-sales elasticity 0.319 for all years (SE 0.0754, first-stage F 14.30) and 0.227 for post-2007 (first-stage F 13.10). OLS 0.0436 (all years) and 0.0500 (post-2007). Firm, year and state-year fixed effects. N = 15,332 (all) and 8,796 (post-2007). The paper names column 4 (IV post-2007, 0.227) as its preferred estimate. The reverse effect of size on lobbying is listed as an open question in the Conclusion (printed p.33). | Design warning only. If lobbying feeds revenue, a growth-to-lobbying regression is simultaneous, so OLS with fixed effects cannot sign the lead. The paper's IV is 4.5 to 7.3 times its OLS (0.227/0.0500 = 4.5; 0.319/0.0436 = 7.3), not 4.5 to 7. The paper uses Compustat public firms with federal LDA filings 1999-2015 and no trade associations, while the sector panel includes associations, so the effect size does not transfer. The paper does not say how it handles self-filer and outside-firm double counting; the text mentions only that about 85 percent of lobbying is outsourced. | moderate | VERIFIED-SOFTENED |
| [[bickmore-2025-corporate-lobbying-political]] | 2025 | qualifies | Lobby coefficient on ShareAssets is 0.0743 (SE 0.0094) with full controls and 0.0907 with none; ShareMcap 0.0804 and ShareSales 0.0759 at full controls. Industry and year FE, no firm FE. Full-control sample N = 116,242 firm-years (1998-2016). ShareAssets mean 0.0727, median 0.0010. Coefficients are level shifts in a 0-1 share. | The paper estimates a between-firm cross-sectional association within NAICS industries, with lobbying on the right-hand side. The sector study asks a within-sector, over-time question with lobbying as the outcome. The paper has no within-firm or within-sector-over-time evidence, so it cannot support the growth-to-lobbying lead. Pooled across industries, so label UNSTRATIFIED. Imai and Kim caution is the analysts own point, not the paper's. | moderate | VERIFIED-SOFTENED |
| [[alexander-2009-lobbying-analysis-under]] |  | qualifies | Logit (Table 3, N not stated): ln assets .415 (p<.001), pre-tax income indicator .729 (p=.001), OCF/sales 1.918 (p<.001), cash/assets .225 (p=.640); no industry indicator significant. OLS (Table 5, N=434, adj R2 .48): ln assets .915 (p<.001), cash/assets 3.702 (p<.001), OCF/sales 1.679 (p=.023), pre-tax income indicator -.314 (p=.330), SIC 3800 2.196 (p=.085). | These are firm-level predictors of capturing a 2004-2006 benefit in a cross-sectional design with no causal identification. They are weak support for a firm-size covariate and say nothing about sector-year giving or growth. Cite as single-era and cross-sectional. Any pooled coefficient over the industry fault line needs strata or an UNSTRATIFIED label. | weak | VERIFIED-SOFTENED |
| [[anon-2025-cross-industry-political]] | 2025 | qualifies | The memo says industries adapt giving to executive-branch pressure regardless of party. Examples: Bank of America PAC spending rose and its partisan split became more balanced after the debanking dispute and DOJ probe; JPMorgan streamlined PAC giving after internal memos. The causal link is hedged ('most likely', lines 22 and 28). No counterfactual or test is given. | The memo concerns executive-branch pressure, while SHIFT concerns congressional control, so the two do not test the same thing. The memo offers firm-level anecdote with no counterfactual and cannot be cited as a finding. At most it motivates a pre-registered event study around dated enforcement actions. | weak | VERIFIED-SOFTENED |
| [[zeileis-nd-various-versatile-variances]] |  | method-source | Eq 20 gives the two-way meat as G(id)/(G(id)-1) M(id) + G(time)/(G(time)-1) M(time) - G(id x time)/(G(id x time)-1) M(id x time). For Petersen firm+year the slope SE 0.053561 matches multiwayvcov only in the printed run with multi0 = TRUE; the text says the default Ma (2014) correction differs from multiwayvcov. | Formula is for OLS on the sector-year design. Equivalence with sandwich::vcovCL on the study design is not run, so it stays UNVERIFIED (DE-062). The paper's multi0 advice applies when each sector-year cell holds one observation; whether the study's cells are singletons was not checked. | strong | VERIFIED-SOFTENED |
| [[zeileis-nd-various-versatile-variances]] |  | qualifies | Exp IV (Fig 4, rho = 0.25, G = 10 to 250): at G = 10 clustered SEs are underestimated in most cases, with HC3 the exception for binomial and Poisson; about 50 to 100 clusters suffice for CL-0. Separately, Sec 5.2 cautions that the Petersen year dimension has only 10 clusters. | The paper's simulations are balanced clusters of 5 with Gaussian-copula correlation, not two-way designs. The direction (SEs too small below G = 10) transfers qualitatively; the size of the bias for the study's sector x year design is not measured. The analyst's 9 x 4 design is not traced in code. | moderate | VERIFIED-SOFTENED |
| [[zeileis-nd-various-versatile-variances]] |  | suggests-extension | Exp IV: for small G the coverage order is HC3 best, then HC2, HC1, HC0. The authors say their results are not consistent with HC3 being overly aggressive, though HC3 slightly over-corrects at G = 10 for binomial. The clustered bootstrap (R = 250) is somewhat better than HC0 and HC1 at small G. Pustejovsky and Tipton (2018) is cited as recommending HC2 with Satterthwaite t. | The paper's bootstrap is a clustered bootstrap with R = 250; its resampling type is not stated for the simulations, and wild or Webb bootstrap-t is not tested. So the paper cannot rank WCR against CR2 or CR3. The HC3 ranking is for linear and GLM fits with balanced clusters of 5, not two-way designs with about 9 sector clusters. | weak | VERIFIED-SOFTENED |
| [[zeileis-nd-object-oriented-computation]] |  | method-source | Meat is the outer product of estimating functions with optional n/(n-k) adjustment (Eq. 10, p.6); sandwich errors lose power under correct specification and are useful only if estimates stay consistent; probit age p moves from .033 to .051 (n = 601, Sec 5.2). | HC1 and CR1 formulas match Eq. 10 with cluster sums added. The paper's meat assumes independent rows, so within-sector dependence needs CR1, which the paper does not treat. Statsmodels cross-check is in code/tests/test_estimate.py lines 44-45, not tests/ at repo root. | moderate | VERIFIED-SOFTENED |
| [[zeileis-nd-econometric-computing-with]] |  | context | HC variants use the sandwich form (X'X)^-1 X' Omega X (X'X)^-1 with diagonal Omega weights built from residuals and the hat-matrix diagonal; HC0 to HC4 differ only in these weights. | Context only. Any HC3 column for DESCRIPTIVE fits is an UNVERIFIED suggestion until a second path reproduces it (DE-062). | weak | VERIFIED |
| [[note-request-for-review-industry-economic-performance]] | 2026 | method-source | Pooled sector-FE models: (1) log Contrib on G(t-1), G(t), Neg(t-1), Midterm, Pres, Tossup, Trend; (2) RShare by chamber on growth, Shift, RMajNow, Midterm, Pres; (3) log Lobby on lagged log Lobby, growth, Neg, Unified. Growth z-scored within sector; CPI-deflated dollars; wild cluster bootstrap by sector; proposed sample 2008-2025 with 5 initial sectors (expansion to about 70 BEA industries allowed). | Holds only as a statement of the memo design. The build departs from it (next bearing), so build results must not be read as memo tests. | internal (design doc) | VERIFIED |
| [[note-request-for-review-industry-economic-performance]] | 2026 | contradicts | Both growth timings belong in one regression (distributed lag) because separate models would each pick up part of the other effect. Totals get Tossup = 1 - abs(2P - 1), not a signed Republican-gains term. A Neg dummy, a Trend and lagged log lobbying are included. Lobbying uses Unified control. | Applies to every CORE C and L result. Rulings R4-R8 and the 2026-09-26 rulings may supersede parts of the memo, but results from separate-timing, signed-control models should not be reported as tests of memo H1 or H3. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-industry-growth-and-political-spending-research]] | 2026 | method-source | Six core tests C1-C3 (contributions) and L1-L3 (lobbying), each with prior actual, same-year actual or predicted growth as the regressor; Y in millions of 2025 USD; X in percentage points; keep all six results including nulls; nine planned sectors give few independent groups so ordinary independent-row OLS errors are not sufficient for strong significance claims. | Direct for the six-test design and the inference rules. The plan asks for dollar levels as the main outcome, and the build keeps levels as the main outcome too, so the log departure is a robustness addition, not the main model. Label any log-based figure as robustness. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-industry-growth-and-political-spending-research]] | 2026 | qualifies | Readiness check: verified contribution years overlap the IEM congressional forecasts only in 2021-2022; IEM covers 2008-2022 with 2023-2025 missing; a later election result must never substitute for a missing forecast. | Holds for the main series only as the labelled RETROSPECTIVE / NOWCAST measure that Paul ruled. Any quoted main-series result using 2023-2026 expectations must carry that label. The STRICT variant is the timing-clean check that the plan's rule supports, and it should be quoted beside any main-series result. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-industry-growth-and-political-spending-research]] | 2026 | qualifies | Do not add a full set of year indicators while keeping national annual controls because year effects absorb them; a separate check may swap the controls for year indicators to see whether the growth relationship survives common yearly shocks. | The recorded figures are hard-coded reference values from the Codex methodology review of run analysis-R-20260926T070630Z, which the pipeline must reproduce; they are not a fresh re-estimation in this check. The pooled slope is UNSTRATIFIED unless the per-sector SLOPES strata are quoted beside it. Single analysis path, so UNVERIFIED under DE-062. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-review-industry-growth-and-political-spending]] | 2026 | contradicts | Main model should be log S_it = alpha_i + lambda_t + beta g_(i,t-1) with sector effects and full year effects, raw percent growth and log dollars; dynamic and other terms only as sensitivities. | Same study and panel. The review predates the 2026-09-25 rulings; promoting the year-FE log model to primary is Paul's decision, as the review itself says. | internal (design doc) | VERIFIED |
| [[note-review-industry-growth-and-political-spending]] | 2026 | supports | A contraction indicator estimates a level jump at zero, not an asymmetric slope; define contraction on raw growth below zero and use g+ = max(g,0) and g- = max(-g,0) with separate coefficients. | Direct. The sign must be flipped when comparing the built _neg slope with the review's g- coefficient. Both ASYM and CONTRACT are EXTENSION labels, not primary. | internal (design doc) | VERIFIED |
| [[note-review-industry-growth-and-political-spending]] | 2026 | qualifies | f(P-M)+gM equals fP+(g-f)M, so Shift adds no information beyond P and current control M; a test of current versus expected power needs both terms in the model. | Holds whenever a year-level probability enters with or without current control; within-chamber year effects would absorb both terms anyway. H4shift is labelled EXPLORATORY in code. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-review-industry-growth-and-political-spending]] | 2026 | contradicts | The main campaign outcome should be corporate and trade-association PAC disbursements to congressional candidates, with individual, party, super PAC and independent-expenditure channels separate; never combine donor-to-PAC receipts with PAC-to-candidate payments. | Paul's rulings govern scope. The review's point bears on how C1-C3 are read as a capacity mechanism and on the mixed receipt types, not on whether the series is collected. | internal (design doc) | VERIFIED |
| [[note-review-industry-growth-and-political-spending]] | 2026 | supports | With five to ten sectors, report small-sample cluster corrections, restricted wild-cluster inference, cluster leverage and leave-one-sector-out checks; bootstrap p-values are not a guarantee; Driscoll-Kraay only as sensitivity with about 18 years. | Direct. The review's point that no method repairs few independent units applies to every pooled growth coefficient. | internal (design doc) | VERIFIED |
| [[note-sector-growth-and-political-spending-source]] | 2026 | method-source | Six core tests: Spending(sector, year) = sector baseline + b x Growth(sector, year-k) + c PresidentialYear + d MidtermYear + e ExpectedDemHouse + f ExpectedDemSenate, with k = 0 or k = 1, plus two predicted-growth tests; each run for contributions and lobbying; 72 sector-years (4 sectors x 2008-2025); no regressions run yet. | The built study is not the note's scope. The primary sector-year window is 2020-2025 with at least 6 distinct years (SGS_WINDOW_RULE, sgs_registry.R lines 17-18; ruling R8 per sgs_config.R line 34), with a fallback of 10 consecutive years inside 2008-2025 only if the primary fails. The LobbyView tests use the 2008-2025 fallback. The claim that same-year growth is outside a real forecast's information set is not in the note and could not be verified, so it is removed. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-sector-growth-and-political-spending-source]] | 2026 | measurement-source | Real growth = 100 x (rev_t / rev_t-1 - 1) on IBISWorld constant-price revenue with no further CPI adjustment. Spending in 2025 dollars = nominal x 321.9 / CPI_year. Democratic share = D / (D + R) by chamber, missing when the denominator is zero. Blanks stay missing, never zero. | IBISWorld covers whole industries while Mentat contributions cover a roster of companies, a population mismatch the note does not state. The attribution of this mismatch to ruling 6 is not in the note and was not verified. IBISWorld historical values may include estimates and later revisions (text line 38). | internal (design doc) | VERIFIED-SOFTENED |

### Sector growth vs giving, partisan share and lobbying regressions

**Evidence weight: thin on substance, solid on method.** There are 27 bearings (6 VERIFIED, 21 VERIFIED-SOFTENED). Plain VERIFIED rows are design or review documents and context rows:
- Codex review note: one row supports the inference rules (strong), one supports the ASYM split (moderate);
- the memo, a method-source describing the design;
- one HC-formula context row, and two rows on the 2024 Cook table.

No VERIFIED external paper supports a growth-to-spending effect within sectors over time.

**1. The returns literature does not transfer to this design (all VERIFIED-SOFTENED).**
- **Huneeus & Kim 2018.** IV log-sales elasticity 0.227, the post-2007 preferred estimate (N = 8,796 firm-years, Compustat public firms with LDA filings, 1999-2015; first-stage F 13.10). Its IV is 4.5 to 7.3 times its OLS.
- **Bombardini & Trebbi review.** A lobbying-to-sales elasticity of about 1 among 766 lobbying firms out of 7,646 public firms in 2017.
- **Bickmore et al. 2025.** Lobby coefficient on ShareAssets 0.0743 (N = 116,242 firm-years, 1998-2016, industry + year FE, no firm FE). It is pooled across industries, so the label is UNSTRATIFIED.
- **Alexander et al.** A single-era (2004-2006) cross-sectional logit and OLS (N = 434 in the OLS).
- **Why none of this transfers.** Every one of these is firm-level and between firms. Several put lobbying on the right-hand side. None tests whether a sector's own growth predicts its own spending over time once year shocks are absorbed. A sector-year null therefore does not contradict them.
- **What the literature does transfer is a warning.** Huneeus & Kim show lobbying raises revenue, so a growth-to-lobbying regression is simultaneous. Its sign cannot be identified by OLS with fixed effects.

**2. The panel-method papers back the study's caution (VERIFIED-SOFTENED, partial transfer).**
- **Imai & Kim 2021.** A 2FE slope is a weighted contrast that depends on linearity. One-way FE, two-way FE and first-difference fits target different weighted contrasts. The paper's negative-weight theorem covers binary treatments under two-way FE. Here growth is continuous, and DECEL (binary and reversible) is fit with sector FE only.
- **Blackwell et al. 2026.** Unit-FE regressions are biased under feedback between treatment and covariates. The diagnosis transfers. The IPTW-FE remedy does not, because it needs a binary treatment and a propensity model.
- **Reading of the L3-LV result.**
  - The fits: +0.352 (n=77) in the base fit, -0.477 with sector + year FE, -0.006 in log-FE, -0.537 in FDY (71 pairs).
  - Adding growth moves adjusted R2 only from 0.884690 to 0.884717 on the same rows.
  - This pattern is consistent with feedback or time-varying confounding, so neither sign is causal.
  - All of these are hard-coded reference values from sgs_review.R for run analysis-R-20260926T070630Z. That is a single path, so they are UNVERIFIED (DE-062).
  - The pooled slope must carry per-sector strata or the UNSTRATIFIED label.

**3. Inference (Zeileis set, VERIFIED-SOFTENED; Codex review, VERIFIED).**
- **Formulas match.** The hand-coded HC1, CR1 and two-way CR1 match the published sandwich formulas. Equivalence with `sandwich::vcovCL` on this design has not been run, so it is UNVERIFIED.
- **Small-cluster problem.** At G = 10 clusters, clustered SEs are underestimated. The study has 9 listed sectors; one source note describes a 4-sector, 2008-2025 panel, and fits with 2 sectors are marked DESCRIPTIVE.
- **What the code already does.** Wild cluster restricted bootstrap-t with Webb weights; a DESCRIPTIVE flag when p below 2/6^G is unreachable; LOSO and LOYO checks.
- **What is missing.** Driscoll-Kraay (none in sgs_estimate.R); cluster leverage (not located); CR2/CR3 (none).
- **Limit of the evidence.** No paper ranks the wild bootstrap against CR2/CR3 at about 9 clusters, so the inference choice is defensible but not validated.

**4. Measurement and scope.**
- **The DV is broad.** Ruling R4 defines general giving across all recipient categories. Workbook 08, window 2019-2026 with 2019-2020 missing: 5.6% of general-giving dollars reach House or Senate candidate committees. Company-PAC gifts are 8.84% of 2021-25 nominal giving. The Codex review says the main outcome should be corporate and trade-association PAC money to candidates, with channels kept separate. Paul's ruling governs here, but a capacity reading of C1-C3 is diluted.
- **Expected-control series has a look-ahead risk.** 2023-2026 values are RETROSPECTIVE / NOWCAST. The 2023 value was measured in Nov 2024, after the year it describes. Any main-series figure needs the STRICT variant quoted beside it.
- **Population mismatch.** IBISWorld revenue covers whole industries, while the giving and lobbying side covers a roster of companies.
- **LobbyView DV double counts.** The undifferentiated LobbyView DV double counts self-filer fees (median 9-26% by sector).

**5. SHIFT.**
- The core SHIFT-House and SHIFT-Senate tests are NOT ESTIMABLE.
- The SHIFT-Chamber preview gives -0.0425, p 0.76: 80 rows, 4 residual df, 2021-25 window, which is below the 6-year minimum and post-result exploratory.
- The Codex review shows that Shift alone cannot separate expected from current control. The contribution core lacks the current-control terms (cur_dem_*), so that model cannot test current-versus-expected power at all.
- Furnas et al. 2019 (lobbying-firm revenue) and the cross-industry memo (executive-branch anecdote, no counterfactual) do not bear on this test.

### Contradictions
- The build contradicts the 2026 'Request for Review' memo (VERIFIED-SOFTENED, strong). The memo puts both growth timings in one distributed-lag model, with Tossup, Neg, Trend and a lagged log-lobbying DV. The build runs C1-C3 as separate single-growth models on dollar levels, with signed expected-control terms and none of those controls. Core C and L results are therefore not tests of memo H1 or H3.
- The build contradicts the Codex review (VERIFIED, strong). The review's primary model is log dollars with sector + full year FE on lagged raw growth. The build's primary model is dollar levels with national controls and no year FE. Year FE, log and log-FE fits exist only as robustness variants. The research plan (VERIFIED-SOFTENED, strong) supports the build's choice, since it says not to add year FE while keeping national controls. So the two design documents disagree with each other, and promoting either one is Paul's ruling.
- The L3-LV sign depends on the specification: +0.352 (n=77) in the base fit, -0.477 with sector + year FE, -0.537 in FDY over 71 pairs, -0.006 in log-FE. Adding growth changes adjusted R2 only from 0.884690 to 0.884717. These are reference values from a single path, so they are UNVERIFIED.
- The bearings disagree on whether the 'growth adds no adjusted R2' result is documented. Three bearings (Bickmore, Bombardini, Alexander) say it was not located. The research-plan bearing found the reference values in sgs_review.R. The values are in code, but the narrative claim has no write-up in the study's markdown.
- The SHIFT design contradicts the Codex review's algebra: Shift alone cannot separate P from M, and the contribution core has no current-control terms (cur_dem_*).

### Gaps
- No study, verified or not, tests whether an industry's own growth predicts its own spending over time once year shocks are absorbed. Every external returns paper is firm-level and between firms. Fill it with: an industry-year panel with many more units (for example about 70 BEA industries, as the memo allowed) and an exogenous growth shifter, such as a shift-share or Bartik demand shock, or trade or commodity-price shocks. That would break the lobbying-to-revenue feedback that Huneeus & Kim document.
- No evidence covers continuous-treatment two-way FE weighting for this design. Fill it with: a signed implicit-weight diagnostic, or a heterogeneity-robust continuous-treatment estimator (for example the de Chaisemartin-D'Haultfoeuille family), run as a second path. It stays EXPLORATORY and UNVERIFIED until reproduced.
- No evidence tests how the wild cluster restricted bootstrap performs at 4-9 sector clusters in a two-way sector-year design. Fill it with: a simulation calibrated to the study's panel, comparing WCR-Webb, CR2-Satterthwaite and CR3 coverage.
- No study checks whether IBISWorld industry revenue is a valid stand-in for the growth of the roster companies. Fill it with: a validation comparing roster-firm Compustat revenue growth against IBISWorld industry growth for each sector.
- The executive-branch pressure hypothesis has only a memo with anecdotes. Fill it with: a pre-registered event study around dated enforcement actions.

### Paul's position vs the evidence

Paul's position has three parts. The evidence bears on each differently.

- **"Returns literature motivates deep-pockets."** This holds only as motivation. The verified papers are firm-level and cross-sectional, and the bearings say explicitly that they should not be cited as support for deep-pockets at sector-year grain. Huneeus & Kim is cited in the system, and its strongest transferable content is a warning (lobbying raises revenue, so the regression is simultaneous), not support.
- **"Imai & Kim's 2FE caution bears on the design."** This is supported. Imai & Kim and Blackwell et al. both back reading none of the FE slopes causally. The binary-treatment theorem does not apply as written to continuous growth, or to DECEL fit with one-way FE.
- **"No stable growth effect cuts against the confident ROI reading in Paul's annotations."** This is consistent with the evidence. The L3-LV sign flip and the negligible adjusted-R2 gain support it. Two qualifications apply: the figures are single-path reference values (UNVERIFIED, DE-062); a sector-year null neither refutes nor confirms firm-level ROI, because they are different estimands.

On balance, the evidence agrees with Paul's caution more than with his annotations.

### Open questions
- Which design governs the primary model: the research plan (levels with national controls, year FE as a check) or the Codex review (log dollars with year FE)? This is Paul's ruling.
- Are the per-sector SLOPES strata available to quote beside the pooled L3-LV slope?
- Has any second path, such as sandwich::vcovCL or fwildclusterboot, reproduced the CR1 and WCR p-values on the study design?
- Are the sector-year cells singletons, which would bear on the two-way multi0 correction?
- Should the C1-C3 DV be split by channel (company PAC to candidates versus other receipts), given that only 5.6% of general giving in 2019-2026 (2019-2020 missing) reaches candidate committees?
- Should SHIFT for contributions add the current-control terms (cur_dem_*), so that expected and current control can be separated?

### Read first
- [[note-review-industry-growth-and-political-spending]]
- [[note-industry-growth-and-political-spending-research]]
- [[imai-2021-fixed-effects-regression]]
- [[blackwell-2026-effect-political-advertising]]
- [[huneeus-2018-effects-firms-lobbying]]
- [[zeileis-nd-various-versatile-variances]]

## SEATS-W weekly seat-gain test (2022 House)

**What it computes.** Weekly D share of giving vs expected D seats gained: -3.47 pp per seat (House), block-bootstrap p .0086-.0174; Senate null.

**Method.** Fixed-X block bootstrap (3 and 5 weeks), Holm; HAC as secondary. One election.

**Code refs.** `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_seats.R`

**Assumptions.**
- As-of seat forecasts precede spending

**Evidence.** 10 rows (3 VERIFIED, 7 VERIFIED-SOFTENED); 10 distinct papers/notes.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[report-2024-2024-house-glance]] | 2024 | measurement-source | A single dated (2024-09-12) Cook rating per House race, which could serve as one rating observation in a 2024 extension. | The live test covers the 2022 House only, so this 2024 snapshot applies only if the test is extended to 2024. Even then it gives one date per race, not the weekly series the test needs. | weak | VERIFIED |
| [[furnas-2019-partisan-ties-lobbying]] | 2019 | qualifies | House-majority ties raise contract-firm revenue in 12 of 13 tests; Senate ties in only 3 of 13. In the 2011 House-flip kernel-weighted difference-in-differences, lobbyists at Republican-aligned firms gained just over $10,000 for the year, and ousted Democratic-aligned firms lost more than $40,000 in annual revenue (stated per lobbyist for one year in the key findings). Outcome: revenue per lobbyist; 1,603 contract firms, 2008 Q1-2016 Q3. The 2015 Senate flip shows no significant effect. | The House-yes, Senate-null pattern in SEATS-W matches the paper's House-over-Senate pattern, which supports chamber-specific estimates. The paper does not predict SEATS-W's sign: it measures contract-lobbying revenue after control actually changed, by firm partisanship; SEATS-W measures the partisan share of giving as within-cycle seat forecasts move. It cannot be cited as evidence for access-seeking in SEATS-W. Both are single-episode designs. | moderate | VERIFIED |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | supports | Table 2 (PDF p.21) and text PDF p.22: labor PACs ideological -231022 vs investor -252936; corporate and trade PACs unconstrained investor -1687949 vs ideological -1694458. Text says the investor model stays on par with the ideological model for these PACs, which is why they are excluded from scaling. Individuals are best fit by the ideological model. | Bonica's tests are cross-sectional over 2004-2012 with static covariates, not within-cycle forecast movement. The paper is consistent with strategic re-allocation for corporate and trade PACs and not for individuals. The 2022 result is one election and stays UNVERIFIED until a second path reproduces it. If the giving series mixes individual employee gifts with PAC money, the paper predicts dilution. | weak | VERIFIED-SOFTENED |
| [[bickmore-2025-corporate-lobbying-political]] | 2025 | qualifies | WinPAC 0.0620 (SE 0.0267, significant at 5 percent) and LosePAC 0.0404 (SE 0.0356, not significant) in the Table 7 region. The authors conclude contributions to winning candidates drive the PAC effect. The text reports no equality test of WinPAC against LosePAC. | Bickmore classifies money by realised election outcome after the fact and does not observe donors' responses to forecasts, so it cannot show donors anticipate winners. WinPAC versus LosePAC is a significant versus non-significant comparison with overlapping intervals, so treat it as a hypothesis. The paper's outcome is firm share, not weekly Democratic share. | weak | VERIFIED-SOFTENED |
| [[zeileis-nd-various-versatile-variances]] |  | supports | Panel PL and PC coverage is poor with short panels (5 observations per cluster) and with exchangeable correlation, where coverage falls as panels lengthen. With decaying AR(1) correlation, coverage rises toward 0.95 as panels lengthen. The paper says some care is needed with panel estimators in short panels or with non-decaying autocorrelation. | The paper's panel estimators apply to many short clusters. SEATS-W is one weekly series inside each cycle with no cluster dimension, so only the general point transfers: HAC is fragile when dependence does not decay. The paper gives no evidence on a single series. | weak | VERIFIED-SOFTENED |
| [[zeileis-nd-object-oriented-computation]] |  | method-source | HAC meat is a weighted sum of lagged estimating-function cross-products (Eq. 11, p.7) behind NeweyWest-type wrappers; no coverage study is reported, and Kiefer-Vogelsang fixed-b testing is named as an alternative (Sec 6, p.14). | Lags, Bartlett weights and n/(n-p) factor match the paper's Eq. 11 setup. The paper gives no finite-sample guarantee, so the caveat that HAC is secondary applies. The 17-week figure appears in the code comment at sgs_seats.R lines 71-77 as an example of an interval, not as a stated cycle length. No second-path check against sandwich::NeweyWest was found in code/tests. | moderate | VERIFIED-SOFTENED |
| [[zeileis-nd-econometric-computing-with]] |  | method-source | Eq. 6 HAC is a weighted sum of lagged products of estimating functions. Investment example RealGNP SE is 0.0168 (Newey-West lag 4, no prewhitening), 0.0236 (Newey-West automatic bandwidth with prewhitening) and 0.0208 (Parzen kernel, VAR(2) prewhitening, no finite-sample adjustment). Spread about 40 percent (0.0236 vs 0.0168). Coefficient 0.1691 is significant under all three; paper says the estimators give slightly different SEs but agree on the conclusion. | Paper examples are time series; RealInt has 103 quarterly obs (1961(1)-1986(3)); Investment length is not stated in the text (fitted series starts 1964). The text does not call the series stationary and gives no coverage evidence at weekly-panel sizes. The ~40 percent spread supports showing several lag settings, not validating any one lag. | moderate | VERIFIED-SOFTENED |
| [[note-request-for-review-industry-economic-performance]] | 2026 | qualifies | Probability of majority control is the preferred projected-control measure because access comes with majority status (Cox and Magar 1999, about 36,000 USD per member, cited and not verified here). Expected seat change is kept only as a robustness check. Q4 asks how quickly PAC giving tracks changing expectations within a cycle; Q7 asks about an event study around expected flips. | Exploratory, one-cycle reading only, 2022 House. The report itself calls it EFFECTIVELY A ONE-ELECTION TEST. Any headline must state the weighting; the pooled-dollar result is not significant. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-industry-growth-and-political-spending-research]] | 2026 | context | Partisan direction is tested with a per-chamber Democratic share D over (D plus R), with zero denominators set missing; party, presidential and other committee flows are not assigned to a chamber without a defensible mapping. | The plan supplies only the outcome definition. SEATS-W and the SHIFT tests are extensions beyond the plan's annual design. The SHIFT preview is exploratory and below the 6-year minimum, so it must not be quoted as an annual test result. The 2022-only window for SEATS-W is not confirmed by the report. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-review-industry-growth-and-political-spending]] | 2026 | supports | Some 538 models use fundraising, so explaining contributions with them creates a feedback problem; prefer polls-only. More monthly rows improve timing resolution, not the number of independent elections (about 9 elections, 6 flips): exploratory. | Holds until 2018 and 2020 weekly money joins; even then each further cycle adds one independent election, per the code notes. | internal (design doc) | VERIFIED |

### SEATS-W weekly seat-gain test (2022 House)

**Evidence weight: weak. One election, and the headline depends on how sectors are weighted.** There are 10 bearings (3 VERIFIED, 7 VERIFIED-SOFTENED). Plain VERIFIED rows: the Codex review (strong, on 538 feedback and the count of independent elections), the partisan-ties paper (moderate) and the Cook 2024 table (weak). The rest are VERIFIED-SOFTENED.

**1. What the result actually is.** Report 221444, 2022 House, 17 weeks, 538 Lite expected Democratic seats, Democratic share of gross positive receipts:
- **Pooled-dollar (first row of the primary table):** -3.36 pp per seat, block-bootstrap p 0.11 (3-week) / 0.10 (5-week). Not significant.
- **Equal-sector:** -3.47 pp per seat, p 0.0086 / 0.0174, Holm 0.034 / 0.070. The 5-week Holm value does not clear 0.05.
- **538 Classic:** -3.58 in the same report.
- **Senate:** null under both weightings (equal-sector +6.23, p 0.65; pooled-dollar +3.99, p 0.81).
- The report itself calls this effectively a one-election test.
- Any headline must name the weighting. The brain note's -3.47 is the equal-sector variant.

**2. Feedback and identification (Codex review, VERIFIED, strong).**
- Some 538 models use fundraising, so regressing contributions on them creates feedback.
- The code says Lite is not polls-only (it imputes unpolled races) but omits the fundraising fundamentals that Classic and Deluxe use.
- Rows are gated on available_at.
- More weeks improve timing resolution, not the count of independent elections (about 9 elections and 6 flips in the full span). Each added cycle adds one election.

**3. Substantive literature (all partial transfer).**
- **Furnas et al. 2019, partisan ties of lobbying firms (VERIFIED).** In the 2011 House-flip kernel-weighted DiD (1,603 contract firms, 2008 Q1-2016 Q3), firms aligned with the ousted Democratic majority lost more than $40,000 in annual revenue per lobbyist. House ties raise revenue in 12 of 13 tests; Senate ties do so in 3 of 13. This matches SEATS-W's House-yes / Senate-null pattern and supports estimating each chamber separately. It does not predict SEATS-W's sign or mechanism. The outcome is contract-lobbying revenue after control actually changed, not giving shares responding to weekly forecasts.
- **Bonica 2013 (VERIFIED-SOFTENED, weak).** In cross-sections over 2004-2012, corporate and trade PACs fit an investor model as well as an ideological one, while individuals fit the ideological model. This predicts dilution if the weekly giving series mixes employee individual gifts with PAC money. It is not evidence about movement within a cycle.
- **Bickmore et al. 2025 (VERIFIED-SOFTENED, weak).** WinPAC 0.0620 (significant) against LosePAC 0.0404 (not significant), with no equality test reported. The paper classifies money by the realised election outcome, so it cannot show that donors anticipate winners. Treat it as a hypothesis only.

**4. Inference.**
- The primary method is a fixed-X residual moving-block bootstrap (3- and 5-week blocks, Holm within family). Newey-West HAC at lags 2, 4 and 8 is secondary and carries no p-value.
- The Zeileis papers confirm that the HAC formula matches the published estimator but give no finite-sample guarantee. Their example shows about a 40% spread in SE across bandwidth choices, which supports reporting several lags.
- No second-path check against `sandwich::NeweyWest` exists.
- With 17 weekly points, block-bootstrap coverage itself is unvalidated.

**5. Measurement.**
- The 2024 Cook table (VERIFIED) gives one dated rating per race (2024-09-12), not a weekly series. It applies only if SEATS-W is extended to 2024.
- The plan supplies the per-chamber Democratic share outcome definition, D/(D+R), with zero denominators set missing. SEATS-W itself is an extension beyond the plan's annual design.

### Contradictions
- The brain note records the 2022 House result as -3.47 pp, p 0.0086 / 0.0174, without naming a weighting. Report 221444 shows that this is the equal-sector variant. The pooled-dollar primary row is -3.36 pp, p 0.11 / 0.10, which is not significant. The headline as recorded overstates the evidence.
- The sgs_seats.R header marks SEATS-W 'NOT BUILT until 09 exists', yet report 221444 carries SEATS-W estimates. The build state and the reported results disagree.
- The bearings disagree on whether the 2022-only window is confirmed. The 'Request for Review' bearing says report 221444 covers the 2022 House only, while the research-plan bearing says the report does not confirm the 2022-only window.
- Paul's plan cites Cox & Magar's majority-status value (about $36,000 per member, cited in the memo and not verified) to justify probability of majority as the preferred measure. SEATS-W's regressor is expected seat change, which the memo keeps only as a robustness check.

### Gaps
- No on-disk, verified study tests whether donors' partisan allocation responds to forecast movement within a cycle. Cox & Magar 1999 and Snowberg, Wolfers & Zitzewitz 2007 are cited but not on disk. Fill it with: a multi-cycle weekly panel (2018, 2020, 2022 and 2024 House) with polls-only forecasts, or an event study around discrete forecast shocks. Ideally it would be split by channel (corporate and trade PAC versus individual employee gifts).
- No evidence exists on block-bootstrap or HAC coverage for a single 17-week series. Fill it with: a simulation calibrated to the weekly series' autocorrelation, comparing moving-block, fixed-b (Kiefer-Vogelsang) and Newey-West coverage.
- Nothing tests the feedback between 538 Lite and giving. Fill it with: re-estimating on a strictly polls-only forecast series (or the generic ballot) and comparing the result.
- The majority-control framing has no verified source. Fill it with: obtaining and verifying Cox & Magar 1999 and the Snowberg, Wolfers & Zitzewitz 2007 market-reaction design, then re-specifying the regressor as the probability of a majority.

### Paul's position vs the evidence

Paul highlighted Furnas's finding that firms tied to a party lose about $40k per lobbyist when that party loses the House. That is VERIFIED: ousted Democratic-aligned firms lost more than $40,000 in annual revenue per lobbyist in the 2011 House flip (1,603 contract firms, 2008 Q1-2016 Q3).

- **It supports one thing:** the value of House majority status, and modelling the chambers separately.
- **It does not support the SEATS-W reading.** It measures who earns lobbying fees after control actually changed. It does not measure where giving goes as forecasts move. The bearing says explicitly that it cannot be cited as evidence of access-seeking in SEATS-W.
- **Cox & Magar is the plan's anchor for majority value, and it is not on disk.** Its roughly $36k-per-member figure is cited and unverified.

The SEATS-W result itself is weaker than the brain note implies:
- The pooled-dollar estimate is null.
- The significant -3.47 is the equal-sector variant, and its 5-week Holm p is 0.070.
- It is one election.

Paul's own EXPLORATORY label is right, but his citations do not yet carry the claim.

### Open questions
- Which weighting, pooled-dollar or equal-sector, is the pre-stated primary for SEATS-W? Report 221444 lists pooled-dollar first.
- Can weekly money for 2018 and 2020 be joined, to add independent elections?
- Should the regressor switch to the probability of House majority, which is the memo's preferred measure, rather than expected seat change?
- Does the weekly giving series mix individual employee gifts with PAC money? Bonica predicts dilution if it does.
- Is the 'NOT BUILT until 09 exists' header stale, or were the report 221444 estimates produced outside the current build?

### Read first
- [[note-request-for-review-industry-economic-performance]]
- [[note-review-industry-growth-and-political-spending]]
- [[furnas-2019-partisan-ties-lobbying]]
- [[bonica-2013-mapping-ideological-marketplace]]
- [[zeileis-nd-econometric-computing-with]]
- [[bickmore-2025-corporate-lobbying-political]]

## Across components

1. **Every result is single-path and exploratory.**
   - The growth-spending figures are hard-coded Codex-review reference values for run analysis-R-20260926T070630Z.
   - The SEATS-W figures come from one report (221444).
   - Under DE-062, neither is a finding until a second path reproduces it.

2. **Few independent units on both sides.**
   - The growth panel has 4-9 sector clusters, some fits have 2, and the primary window is 2020-2025.
   - SEATS-W has one election.
   - The Zeileis papers and the Codex review agree that no variance correction repairs this. The honest frame is trend-first, meaning sign, size and consistency across specifications.
   - By that standard both components are inconsistent: L3-LV flips sign under year FE, and SEATS-W turns null when weighted by pooled dollars.

3. **The supporting literature is about something else.**
   - Huneeus & Kim, Bickmore, Bombardini & Trebbi and Alexander are firm-level, between-firm studies.
   - Furnas et al. measures contract-lobbying revenue after realised control changes.
   - Bonica is cross-sectional.
   - None measures what this study measures: sector spending over time within a sector, or giving shares responding to forecasts within a cycle. They supply motivation and design warnings, not support. Pooled cross-industry coefficients from them (Bickmore) must carry the UNSTRATIFIED label.

4. **The DV mixes channels in both components.**
   - General giving across all recipient categories: only 5.6% of 2019-2026 general-giving dollars (2019-2020 missing) reach candidate committees.
   - Company PAC money is 8.84% of 2021-25 nominal giving.
   - Bonica's split (investor PACs versus ideological individuals) predicts that mixing dilutes any strategic signal. That makes a channel split the single most informative refinement for both components.

5. **Timing integrity for expected control.**
   - The annual series fills 2023-2026 with RETROSPECTIVE / NOWCAST values, and the 2023 value was measured in Nov 2024. The STRICT variants must accompany any quote.
   - SEATS-W gates rows on available_at but uses 538 Lite, which is not polls-only.
   - Both components share a risk that the measure of political expectation is contaminated by the outcome or by later information.

6. **Design-document drift.** The research plan, the Request-for-Review memo and the Codex review disagree on the primary specification, and the build follows none of them exactly. Until Paul rules on which document governs, results should be labelled by the specification that produced them, not by the memo hypothesis.

## Research agenda

1. Get and verify Cox & Magar 1999 and Snowberg, Wolfers & Zitzewitz 2007. Both are cited in SEATS-W planning and neither is on disk. Until they are verified, the majority-value framing rests on an unverified figure of about $36k per member.
2. Reproduce the L3-LV reference values (+0.352 in the base fit, -0.477 with year FE, -0.006 in log-FE, -0.537 in FDY) and the SEATS-W estimates through a second path, such as fwildclusterboot or sandwich::vcovCL / NeweyWest. That moves them from UNVERIFIED to verified under DE-062.
3. Fix the SEATS-W headline in the brain so that it names the weighting: equal-sector -3.47 pp (Holm 0.034 / 0.070) against pooled-dollar -3.36 pp (p 0.11 / 0.10, null), 2022 House, 17 weeks.
4. Extend SEATS-W to more cycles (2018, 2020, 2024 weekly money), and add a polls-only forecast series to break the 538-fundraising feedback.
5. Split both components' DV by channel (corporate and trade PAC to candidates, individual employee gifts, party and other receipts) and re-run, to test the dilution Bonica predicts.
6. Look for or build an exogenous sector-growth shifter (for example a shift-share demand shock) on a wider BEA-industry panel. That addresses the simultaneity Huneeus & Kim document.
7. Run a small-cluster coverage simulation calibrated to the sector-year panel (about 9 sectors x 6 years) comparing WCR-Webb, CR2-Satterthwaite and CR3, and an equivalent simulation for the 17-week block bootstrap.
8. Get Paul's ruling on the primary specification: the research plan (levels with national controls) or the Codex review (log dollars with year FE). Also decide whether the contribution SHIFT model gets the current-control terms (cur_dem_*).
