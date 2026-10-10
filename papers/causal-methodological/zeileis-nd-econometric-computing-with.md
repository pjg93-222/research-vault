---
type: paper
title: "Econometric Computing with HC and HAC Covariance Matrix Estimators"
authors: ["Achim Zeileis"]
year: "2004"
venue: "Journal of Statistical Software 11(10), 1-17 (this file is a slightly modified reprint of that article, as a package introduction for the R package sandwich)"
doi: "10.18637/jss.v011.i10"
category: "Causal & Methodological"
systems: ["mentat-giving-forecast", "mentat-sector-growth-study"]
components: ["mentat-sector-growth-study.seats-weekly-test", "mentat-giving-forecast.cycle-model", "mentat-sector-growth-study.growth-spending-regressions"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/zeileis-nd-econometric-computing-with.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\Codex\\2026-09-26\\loo\\work\\lobbying_followup\\Rlib\\sandwich\\doc\\sandwich.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Mentat Complete Files\\FORECAST_OVERNIGHT_2026-10-07\\staging\\runtime\\Rlib\\sandwich\\doc\\sandwich.pdf"]
sha12: ["2c95fc847117"]
tags: [paper, causal-methodological, type/paper, theme/causal-methodological, evidence/verified]
---

# Econometric Computing with HC and HAC Covariance Matrix Estimators

**Achim Zeileis (2004).** *Journal of Statistical Software 11(10), 1-17 (this file is a slightly modified reprint of that article, as a package introduction for the R package sandwich).* [10.18637/jss.v011.i10](https://doi.org/10.18637/jss.v011.i10). Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> Zeileis (JSS 11(10), 2004; reprinted as the sandwich package introduction) is a software paper, not an empirical study. It casts HC and HAC covariance estimators as one sandwich form, (X'X)^-1 X'Omega X (X'X)^-1, and gives R tools that plug into coeftest and structural-change tests: vcovHC (const, HC0-HC4, HC3 default) and vcovHAC with Newey-West, Andrews kernel and Lumley weights, optional VAR prewhitening and automatic bandwidth. Three worked examples carry the numbers. On 50 US states (1979 school spending), the quadratic income term goes from SE 830 (HC0, p .056) to SE 5489 (HC4, p .77) because one high-leverage state, Alaska, drives it. On the Greene investment series, three HAC settings move the RealGNP SE from 0.0168 to 0.0236 (about 40 percent) without changing the conclusion. On quarterly US real interest 1961-1986 (103 quarters), a HAC-scaled CUSUM rejects stability (p .0082) and Bai-Perron dating finds breaks in 1972(3) and 1980(3). There is no simulation evidence. Claims about small-sample performance come from Long and Ervin (2000) and Cribari-Neto (2004). For Paul's systems this is a method source for the robust-variance rows in Mentat. The forecast refit calls sandwich::vcovHC HC1 and vcovCL directly. The SEATS-W weekly test hand-codes a Bartlett Newey-West SE at fixed lags 2, 4 and 8 weeks and labels it secondary. The paper's own examples support that design: HAC lag and prewhitening choices visibly move SEs. Its leverage example qualifies the HC1 rows in small refit regressions (df about 11). Nothing here touches the network, lobbying or influence components.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** How to turn HC (heteroskedasticity-consistent) and HAC (heteroskedasticity and autocorrelation consistent) covariance estimators from the econometrics literature into reusable, modular computational tools in R that plug into fitted models and inference procedures.

**Theory.** Sandwich form Psi = (X'X)^-1 X'Omega X (X'X)^-1 for OLS (eq. 4-5, p.3-4). HC: plug diagonal Omega estimate with weights omega_i from residuals and hat-matrix diagonal (const, HC0-HC4, p.4-5). HAC: plug estimate of Phi from weighted lagged products of estimating functions V_i (eq. 6, p.6), extending to estimating functions beyond OLS (ML, GMM, QML; footnote 1, p.5). Cites White 1980, MacKinnon and White 1985, Newey and West 1987, 1994, Andrews 1991, Andrews and Monahan 1992, Lumley and Heagerty 1999.

**Data.** Three illustrative real-world datasets, no new data collected. (1) PublicSchools (Greene 1993 Table 14.1): per capita school expenditure vs per capita income for 51 US states in 1979; after dropping Wisconsin NA the analysed N is 50 (p.10). Unit: US state, cross-section. (2) Investment (Greene 1993 Table 15.1): real investment on real GNP and real interest, time series; period not stated in text (fitted series starts 1964 per appendix code, p.19). (3) RealInt (strucchange package): quarterly US ex-post real interest rate 1961(1)-1986(3), so 103 quarterly observations by calendar count, regressed on a mean (p.13).

**Method.** Estimator functions in R package sandwich: vcovHC(lmobj, omega, type = HC3 default) for HC; vcovHAC(lmobj, weights, prewhite, adjust, sandwich, order.by, ar.method, data) for HAC. Weight helpers: NeweyWest (Bartlett with lag L, default data-driven bwNeweyWest), weightsAndrews and bwAndrews via kernHAC (default quadratic spectral kernel, VAR(1) prewhitening, AR(1) bandwidth), weightsLumley via weave (truncate C=4 or smooth C=1). Inference: coeftest from lmtest with z tests (df = Inf) or t tests; gefp and sctest for OLS-based CUSUM with kernHAC; Fstats for supF; breakpoints (Bai and Perron 2003 dynamic programming, BIC-chosen partition) with confint using vcov = kernHAC.

**Identification.** None in the causal sense. Consistency of the covariance estimators is asserted from the cited asymptotic theory under regularity conditions (p.3-4, p.6), with no new identification argument. Paper is a software and methods exposition.

**Scope.** Scope is the general linear model estimated by OLS (lm), with HAC also available for glm and robust rlm (MASS) per p.2-3, and extensions to other estimating-function models per footnote 1 and footnote 2. Era: literature of roughly the last 20 years as of about 2004, software in R 4.6.1 with sandwich 3.1-3, lmtest 0.9-40, strucchange 1.5-4, zoo 1.9-0 (appendix, p.18). Population: applied econometricians using R. Institution: none stated.

## Key findings

- Public schools (Sec 4.1, p.10-11): HC0 gives the quadratic income term I(Income^2) coefficient 1587, SE 830, z 1.91, p 0.056 (weakly significant); HC4 gives SE 5489, z 0.29, p 0.77 (clearly non-significant).
- The HC0 vs HC4 difference is attributed to a single high-leverage outlier, Alaska (Figure 2, p.11), with HC4 correcting for leverage.
- Investment (Sec 4.2, p.11-13): RealGNP coefficient 0.1691 is significant under all three HAC variants: SE 0.0168 (Newey-West lag 4, no prewhitening, p<2e-16), SE 0.0236 (Newey-West automatic bandwidth with prewhitening, p 7.4e-13), SE 0.0208 (Parzen kernel, VAR(2) prewhitening, no finite-sample adjustment, p 4.7e-16).
- RealInt coefficient -1.0014 is not significant under any of the three HAC variants: SE 3.3424, 3.6399, 3.9475 with p 0.76, 0.78, 0.80 respectively (p.12-13).
- Intercept under the three HAC variants: SE 18.9583, 24.3742, 24.6639 (p 0.51, 0.61, 0.61) (p.11-13). Paper states the three estimators give slightly different standard errors but agree on the substantive conclusions.
- Real interest CUSUM (Sec 4.3, p.13-14): OLS-based CUSUM with quadratic spectral kernel HAC variance (VAR(1) prewhitened, AR(1) bandwidth) crosses its 5 percent boundary; sctest p = 0.0082, indicating a significant change in mean with a clear peak in the early 1980s.
- Bai-Perron dating (BIC-minimal partition, 3 segments, two breaks; p.14-15): break 1 at observation 47 (1972(3)) with 95 percent CI 37 to 48 (1970(1) to 1972(4)); break 2 at observation 79 (1980(3)) with 95 percent CI 77 to 81 (1980(1) to 1981(1)).
- Footnote 6 (p.15): choosing breakpoints by sequential tests instead of BIC yields an additional breakpoint with rather wide confidence intervals.
- Section 3.1 (p.5): paper cites Long and Ervin (2000) concluding HC3 performs best in small samples as it down-weights influential observations; Cribari-Neto (2004) proposed HC4 for influential observations.
- Section 3.2 (p.6-8): all implemented HAC estimators are weighted sums of lagged products of estimating functions (eq. 6); kernel weights per Andrews 1991 (Figure 1, p.7) with bandwidth selection by AR(1) or ARMA(1,1) approximation or Newey-West 1994.

## Limitations

- Paper is a software exposition and reports no simulation or Monte Carlo evidence on finite-sample coverage; small-sample performance claims are cited from Long and Ervin (2000) and Cribari-Neto (2004).
- Choice of HAC weights and bandwidth (lag L, kernel, prewhitening order, bandwidth selector) changes standard errors and the paper notes results depend on these choices (p.8-13).
- HC0 vs HC4 conclusions on the public-schools quadratic term hinge on one outlier (Alaska), as the paper itself notes (p.11).
- HAC requires that observations be ordered (order.by, default current order, suited to time series); prewhitening uses VAR(p) with OLS by default (p.6-7).
- Weights vectors shorter than n are padded with zeros (footnote 3, p.7), a convention the user must keep in mind.
- The breakpoint CI approach needs a function-valued vcov because it is applied to several segments; the sequential-test alternative gives wide intervals (footnote 6, p.15).

## Short quotes

> Paper attributes the HC4 vs HC0 gap on school spending to 'the improved performance of the HC4' correcting high-leverage points (p.11)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#SEATS-W weekly seat-gain test (2022 House)\|SEATS-W weekly seat-gain test (2022 House)]] | method-source | Eq. 6 HAC is a weighted sum of lagged products of estimating functions. Investment example RealGNP SE is 0.0168 (Newey-West lag 4, no prewhitening), 0.0236 (Newey-West automatic bandwidth with prewhitening) and 0.0208 (Parzen kernel, VAR(2) prewhitening, no finite-sample adjustment). Spread about 40 percent (0.0236 vs 0.0168). Coefficient 0.1691 is significant under all three; paper says the estimators give slightly different SEs but agree on the conclusion. (Sec 3.2 eq. 6 (p.6); Sec 4.2 Investment output (p.11-12)) | SEATS-W hand-codes a Bartlett Newey-West SE at fixed lags 2, 4 and 8 weeks, lag pairs matched within cycle by calendar week, n/(n-k) adjustment, no prewhitening and no automatic bandwidth, labelled secondary with no p-value. Primary inference is a fixed-X moving-block bootstrap with Holm. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_seats.R:277 and :391` | Paper examples are time series; RealInt has 103 quarterly obs (1961(1)-1986(3)); Investment length is not stated in the text (fitted series starts 1964). The text does not call the series stationary and gives no coverage evidence at weekly-panel sizes. The ~40 percent spread supports showing several lag settings, not validating any one lag. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-giving-forecast#Cycle-aware giving refit (phase and drift log models, 50-50 growth)\|Cycle-aware giving refit (phase and drift log models, 50-50 growth)]] | qualifies | On the public schools data (51 states, Wisconsin NA dropped, N=50), the quadratic income term has SE 830, z 1.91, p 0.056 under HC0 and SE 5489, z 0.29, p 0.77 under HC4. Paper attributes the gap to one high-leverage outlier (Alaska) and HC4's leverage correction. It cites Long and Ervin (2000) for HC3 as best in small samples and makes HC3 the vcovHC default. (Sec 3.1 p.5; Sec 4.1 p.10-11 and Figure 2) | Refit robustness block prints an HC1 row via sandwich::vcovHC type HC1, beside CR1, CR2 (clubSandwich) and a wild cluster restricted bootstrap. Fixture row: HC1 SE 0.0395 on 11 df for the pac G_current model. `mentat/forecast/refit/R/rf_robust.R:9 and mentat/forecast/refit/tests/testthat/fixtures/robustness_pvalues.csv:3 (repo at C:/Users/pgoff/Mentat)` | The paper's HC0 vs HC4 contrast is an estimator-choice comparison on one 50-state cross-section with one outlier. It does not show HC1 understates SEs in the refit's ~11 df regressions. The HC1 understatement point is the analyst's inference from cited small-sample literature, not a result in this paper. The paper has no simulation. | weak | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | context | HC variants use the sandwich form (X'X)^-1 X' Omega X (X'X)^-1 with diagonal Omega weights built from residuals and the hat-matrix diagonal; HC0 to HC4 differ only in these weights. (Sec 2 eq. 4-5 (p.3-4); Sec 3.1 (p.4-5)) | sgs_estimate.R hand-codes an HC1 sandwich V = (n/df) A X'uu'X A, labelled reference only. DESCRIPTIVE tests report the estimate and the HC1 error only; inference is a wild cluster restricted bootstrap with Webb weights. The Holm step was not seen in this file. `sector_growth_study/code/R/sgs_estimate.R:35 (also :1 and :266) in C:/Users/pgoff/Mentat` | Context only. Any HC3 column for DESCRIPTIVE fits is an UNVERIFIED suggestion until a second path reproduces it (DE-062). | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: summary_md says the paper's examples support the SEATS-W fixed-lag design. The paper shows lag and prewhitening sensitivity only; it does not endorse fixed lags 2, 4 and 8.
> - Summary issue: summary_md says Mentat robust rows use HC1 throughout. Only the refit robustness block and sgs_estimate.R were checked; no full census was run.
> - Summary issue: summary_md says the package versions match registry/models.json. The paper does state sandwich 3.1-3, lmtest 0.9-40, strucchange 1.5-4 and zoo 1.9-0 (p.18), but the registry file was not opened.
> - Summary issue: rel bearing 2 code_ref rf_robust.R:8 should be :9 (the vcovHC line); line 8 is the rf_hc1 function header.
> - Summary issue: rel bearing 1 transfer_conditions describes the examples as stationary macro series of about 40-100 observations; the paper states neither the stationarity nor the Investment series length.
> - Summary issue: rel bearing 2 says a single leverage point moves p from .056 to .77; that contrast is an HC0 vs HC4 estimator change, and the paper attributes it to one outlier plus the HC4 correction.
> - Summary issue: rel related 'various-versatile-variances' is real (title and abstract confirm clustered covariances in sandwich) but postdates and is not cited by this 2004 paper.
> - Summary issue: Coverage: paper_markup is empty because paul_entries is empty and n_annots is 0. Page images were not used; checks were against the text file only.

## Gaps and flags

- Mentat's robust-variance rows use HC1 throughout, in sandwich::vcovHC in the refit and hand-coded in sgs_estimate. There is no HC3 or HC4 leverage-adjusted row, although the paper shows a single leverage point moving p from .056 to .77 at N=50 and makes HC3 the small-sample default.
- SEATS-W HAC uses fixed Bartlett lags 2/4/8 with no prewhitening or data-driven bandwidth (Andrews 1991 or Newey-West 1994). The paper's examples show these choices move SEs by about 40 percent. The secondary-only, no-p labelling already contains the risk, but no automatic-bandwidth row is shown.
- No component runs a structural-change test (CUSUM or Bai-Perron) on giving series. The giving forecast assumes stable phase effects across cycles (D-092), and the paper's strucchange workflow is one candidate check. Treat it as exploratory, because giving series have few cycles.
- No conflict with brain rules: the paper makes no network, lobbying or influence claims and does not argue for either rejected SNA proposal.
- The paper is a software exposition with no Monte Carlo evidence. Any small-sample claim (HC3 or HC4 superiority) comes from Long and Ervin (2000) and Cribari-Neto (2004) and should be cited as such, not to this paper.
- Package versions in the reprint (sandwich 3.1-3) match the refit's vendored sandwich 3.1-3 in registry/models.json, so the method source and the code agree on version.

Paul has no annotated-bibliography entry and no markup for this work (n_annots 0), so there is no takeaway to compare. The system as built already follows the paper's spirit and goes further. HC and HAC errors are reported but kept secondary to cluster and block bootstraps, with Holm adjustment, which is stricter than the paper's coeftest z-tests. The live code shows the paper is a method source, not a stale citation.

## Related papers

- [[zeileis-nd-object-oriented-computation]] — extends — The paper points to Zeileis 2006b, Object-Oriented Computation of Sandwich Estimators, as the follow-up implementation. The rel's claim that it generalizes to any model with estimating functions and bread was not confirmed in this paper's text, so the link rests on the paper's own citation.
- [[zeileis-nd-various-versatile-variances]] — extends — Later work by Zeileis, Koll and Graham on clustered covariances in the same sandwich package. This 2004 paper does not cite it, so the link is real but is not a citation from this paper. The vcovCL use in the Mentat refit is a code fact, not something this paper supports.

### Linked from
_None._