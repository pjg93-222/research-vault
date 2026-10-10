---
type: paper
title: "Various Versatile Variances: An Object-Oriented Implementation of Clustered Covariances in R"
authors: ["Achim Zeileis", "Susanne Köll", "Nathaniel Graham"]
year: "2020"
venue: "Journal of Statistical Software 95(1), 1-36 (this text is a slightly modified reprint; it reports R 4.6.1 and sandwich 3.1-3)"
doi: "10.18637/jss.v095.i01"
category: "Causal & Methodological"
systems: ["mentat-sector-growth-study"]
components: ["mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.seats-weekly-test"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/zeileis-nd-various-versatile-variances.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\Codex\\2026-09-26\\loo\\work\\lobbying_followup\\Rlib\\sandwich\\doc\\sandwich-CL.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Mentat Complete Files\\FORECAST_OVERNIGHT_2026-10-07\\staging\\runtime\\Rlib\\sandwich\\doc\\sandwich-CL.pdf"]
sha12: ["02a5be949f84"]
tags: [paper, causal-methodological]
---

# Various Versatile Variances: An Object-Oriented Implementation of Clustered Covariances in R

**Achim Zeileis, Susanne Köll, Nathaniel Graham (2020).** *Journal of Statistical Software 95(1), 1-36 (this text is a slightly modified reprint; it reports R 4.6.1 and sandwich 3.1-3).* [10.18637/jss.v095.i01](https://doi.org/10.18637/jss.v095.i01). Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> Zeileis, Koell and Graham (JSS 95(1), 2020; reprint run on sandwich 3.1-3) describe the sandwich package's object-oriented clustered and panel covariances: vcovCL (one-way and multi-way, HC0-HC3, G/(G-1) adjustment, odd/even inclusion-exclusion with an optional multi0 swap), vcovPL (panel Newey-West and Driscoll-Kraay), vcovPC (Beck-Katz PCSE) and vcovBS (clustered bootstrap). Worked examples reproduce multiwayvcov, plm and pcse to six decimals on the Petersen benchmark. On the Aghion et al. hurdle model, clustered SEs come out roughly 1.5 to 2 times the basic sandwich SEs. The Monte Carlo (10,000 replications, clusters of 5) finds CL-0 coverage near 0.95 across lm, GLMs, beta and count models once G is around 50 to 100. At G = 10, standard errors are underestimated, and the order from best to worst is HC3, HC2, HC1, HC0. The bootstrap is only somewhat better than HC0 and HC1 at that size. Panel NW and PCSE fail when panels are short or the dependence is exchangeable rather than decaying. The main relevance to Paul's systems is the Mentat sector growth study. Its two-way sector-by-year CR1 sensitivity in sgs_review.R is Equation 20 of this paper, implemented by hand. Its primary inference clusters by 5 to 9 sectors. That is below the paper's smallest tested G of 10, a size the paper already shows to be anti-conservative for CR1-type errors. The study's choice of a wild cluster restricted bootstrap with Webb weights, rather than CR1 t-tests, is therefore the right instinct. However, this paper does not test WCR bootstrap-t (its vcovBS is a variance bootstrap), so it neither validates nor refutes that choice. The SEATS-W test treats Newey-West HAC as secondary with no p-value, which matches the paper's finding that panel NW is unreliable. The paper has no bearing on networks, lobbying identity or allocation.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** How can clustered and panel sandwich covariances be provided as one object-oriented R implementation that works for any model with estfun() and bread() methods, and how well do these covariances perform in finite samples beyond lm() and glm()?

**Theory.** Quasi-maximum likelihood / M-estimation with misspecified likelihood (White 1994); sandwich covariance B M B with bread and meat; cluster-robust meat with G approaching infinity (Cameron and Miller 2015); independence working model of GEE (Liang and Zeger 1986).

**Data.** Sec 5.1: InstInnovation data from the sandwich package, following Aghion, Van Reenen and Zingales (2013): 803 firms, firm-level panel 1991-1999, citation-weighted patent counts; clustered by company (803 clusters) and by industry. Sec 5.2: PetersenCL simulated benchmark data from Petersen (2009): 500 firms over 10 years, linear model y on x. Sec 6 and Appendix A: Monte Carlo simulations with balanced clusters of 5 observations (up to 50 in Appendix A), G from 10 to 250 clusters, 100 clusters in the main Experiments I-III, cluster correlation rho from 0 to 0.9 via a Gaussian copula, regressor correlation rho_x = 0.25; unit of analysis is the observation within cluster; period is cross-sectional simulation (no calendar period).

**Method.** Object-oriented sandwich estimators in R package sandwich: vcovCL (one-way and multi-way clustered covariances with HC0-HC3 bias corrections, cluster factor G/(G-1), multi-way inclusion-exclusion sum with odd/even signs, optional multi0 variant), vcovPL (panel Newey-West and Driscoll-Kraay with Bartlett or other kernels, lag NW1987 or NW1994), vcovPC (Beck and Katz panel-corrected SEs, balanced and unbalanced via pairwise or casewise), vcovBS (clustered bootstrap with xy, residual, wild, Mammen, Webb, norm types, R default 250). Benchmarks against lmtest, multiwayvcov, plm, geepack, pcse and lme4 random-intercept models, and GEE with exchangeable correlation.

**Identification.** None in the causal sense. Inference is via Wald tests and 95 percent confidence intervals using the estimated covariance; the paper notes that clustering in treatment assignment matters more than clustering in sampling (Abadie, Athey, Imbens and Wooldridge 2023) when fixed cluster effects are included.

**Scope.** Population: regression models with estfun() and bread() methods (lm, glm, mlm, nls, betareg, clm, coxph, survreg, crch, hurdle, zeroinfl, mlogit, polr, rlm); simulated cross-section and panel data; benchmark data from finance and political-economy style applications. Software era: sandwich 2.4.0 onward, paper computations with sandwich 3.1-3 and R 4.6.1. Not an institutional or historical study.

## Key findings

- Sec 5.1 (p.17): hurdle negbin on Aghion et al. data with CL clustering by company (803 clusters): institutions in zero part 0.00658, clustered SE 0.00209, p = 0.0016; in count part 0.00397, SE 0.00430, p = 0.357. Only the zero-part coefficient is significant.
- Sec 5.1 (p.17-18): usual standard errors greatly overstate precision. Standard and basic sandwich SEs for zero institutions are both about 0.00135 versus 0.00209 under clustering; clustered SEs are about 1.5 to 2 times the basic sandwich SEs.
- Sec 5.1 (p.18): the clustered bootstrap (vcovBS, R = 250) agrees closely with CL-1, e.g. count institutions 0.00411 versus 0.00430, at much higher computational cost.
- Sec 5.2 (p.18-19): on the Petersen benchmark (500 firms, 10 years), one-way firm-clustered slope SE 0.050596 from vcovCL matches multiwayvcov; plm and geepack give 0.050540 with HC0 and no cluster adjustment.
- Sec 5.2 (p.20): two-way firm and year clustering gives intercept SE 0.065066 and slope SE 0.053561 (multi0 = TRUE); the authors caution that year has only 10 clusters, below what theory requires.
- Sec 5.2 (p.20-21): Driscoll-Kraay vcovPL with adjust = FALSE gives slope SE 0.028163, identical to plm vcovSCC; PCSE slope SE 0.025276 agrees with pcse, including unbalanced panels (pairwise 0.025338, casewise 0.025241).
- Sec 6.2 Experiment I (p.24): with G = 100 clusters of 5 observations and cluster correlation rho up to 0.9, standard, basic, PL and PC covariances lose coverage for the correlated and clustered regressors; CL-0 stays near the nominal 0.95. PL also fails for the uncorrelated regressor x3.
- Sec 6.2 Experiment II (p.25): binomial (logit) and Poisson GLMs give results similar to the Gaussian case, so clustered covariances work well with GLMs; random-intercept mixed models differ because they are conditional rather than marginal in nonlinear models.
- Sec 6.2 Experiment III (p.26-27): beta, zero-truncated Poisson and zero-inflated Poisson: CL-0 performs well and is similar to bootstrap (R = 250); coverage for zerotrunc and zeroinfl sits slightly below 0.95 regardless of rho, which the authors attribute to the normal approximation rather than to clustering.
- Sec 6.2 Experiment IV (p.27-28): at rho = 0.25 and G = 10, standard errors are underestimated in most cases; about 50 to 100 clusters suffice for CL-0. For small G the ordering is HC3 best, then HC2, HC1, HC0; bootstrap is somewhat better than HC0 and HC1 at small G.
- Sec 6.2 Experiment IV (p.28): the authors do not find HC3 overly aggressive for small G, contrary to Pustejovsky and Tipton (2018), though HC3 slightly over-corrects for binomial models at G = 10.
- Appendix A (p.35-36): with 5 to 50 observations per cluster, PL and PC approach 0.95 only under AR(1) correlation; under exchangeable correlation with correlated or clustered regressors their coverage falls as clusters get longer. Clustered HC0 performs satisfactorily throughout.

## Limitations

- HC2 and HC3 cluster corrections need blocks of the full hat matrix, which no object-oriented extractor provides, so they are available only for lm and glm (Sec 4.1, p.9 and p.13).
- Panel estimators PL and PC are unreliable for short panels and with non-decaying autocorrelation (Sec 7, p.28; Appendix A).
- Small numbers of clusters bias standard errors downward (G = 10 in Exp IV); the year dimension in the Petersen example has only 10 clusters, so its two-way results are cautioned (p.20).
- Residual cluster bootstrap is not well defined for unbalanced clusters and only issues a warning (p.16).
- The fixed-G asymptotic framework is not pursued (p.4). Main simulations use balanced clusters of size 5 and at most three regressors with fixed coefficient vectors, fixed beta precision 10 and zero-inflation probability 0.3.
- The authors explain the below-0.95 coverage for zero-truncated and zero-inflated Poisson by the normal approximation but do not test this explanation (p.26-27).
- The xy bootstrap makes the same assumptions as the asymptotic estimator, so its results converge to it and may be unnecessary when the asymptotic estimator is available (p.15).
- Multi-way clustering default follows Ma (2014) and differs from multiwayvcov unless multi0 is set (p.13, p.20).
- Clustered estimators are unavailable when a model package lacks estfun() (p.15).

## Short quotes

> the usual standard errors greatly overstate the precision of the estimators
> clustered covariances work reasonably well in the models investigated but some care is needed when applying panel estimators
> the larger the number of clusters G, the better the coverage and the less standard errors are underestimated

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | method-source | Eq 20 gives the two-way meat as G(id)/(G(id)-1) M(id) + G(time)/(G(time)-1) M(time) - G(id x time)/(G(id x time)-1) M(id x time). For Petersen firm+year the slope SE 0.053561 matches multiwayvcov only in the printed run with multi0 = TRUE; the text says the default Ma (2014) correction differs from multiwayvcov. (Sec 3.2 Eq 20 (text ~lines 545-575); Sec 4.1 multi0 (text ~lines 769-772); Sec 5.2 Petersen (text ~lines 1150-1168, p.19-20)) | sgs_review.R crv() applies (G/(G-1))*((n-1)/(n-p)) to cluster score sums; two-way = crv(sector) + crv(year) - crv(sector x year); df = min(G_sector, G_year) - 1; a non-positive variance is reported as a note only, not repaired. No multi0 option exists in the code. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_review.R:21 and :74-86 (crv and two-way block) and :98-106 (df, note)` | Formula is for OLS on the sector-year design. Equivalence with sandwich::vcovCL on the study design is not run, so it stays UNVERIFIED (DE-062). The paper's multi0 advice applies when each sector-year cell holds one observation; whether the study's cells are singletons was not checked. | strong | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | qualifies | Exp IV (Fig 4, rho = 0.25, G = 10 to 250): at G = 10 clustered SEs are underestimated in most cases, with HC3 the exception for binomial and Poisson; about 50 to 100 clusters suffice for CL-0. Separately, Sec 5.2 cautions that the Petersen year dimension has only 10 clusters. (Sec 6.2 Exp IV and Fig 4 caption (p.27-28); Sec 5.2 (p.19-20)) | sgs_review.R computes the two-way CR1 only when both dimensions have at least 3 clusters (SGS_TWOWAY_MIN_CLUSTERS = 3), with t on min(G_sector, G_year) - 1 df, labelled a sensitivity. DEFAULT_WINDOW is 2020-2025 (about 6 years; 2026 partial and excluded by default). Nine study sectors are listed in sgs_config.R. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_review.R:21 and :98-106; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_config.R:28-29 and :34-36` | The paper's simulations are balanced clusters of 5 with Gaussian-copula correlation, not two-way designs. The direction (SEs too small below G = 10) transfers qualitatively; the size of the bias for the study's sector x year design is not measured. The analyst's 9 x 4 design is not traced in code. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | suggests-extension | Exp IV: for small G the coverage order is HC3 best, then HC2, HC1, HC0. The authors say their results are not consistent with HC3 being overly aggressive, though HC3 slightly over-corrects at G = 10 for binomial. The clustered bootstrap (R = 250) is somewhat better than HC0 and HC1 at small G. Pustejovsky and Tipton (2018) is cited as recommending HC2 with Satterthwaite t. (Sec 6.2 Exp IV (p.27-28; text ~lines 1600-1615)) | sgs_estimate.R: WCR bootstrap-t with Webb six-point weights, clusters = sectors, CR1 factor G/(G-1)(n-1)/(n-p) in the t statistic (sgs_cr1_factor). A grep of sgs_review.R and sgs_estimate.R finds no CR2 or CR3 error. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:4-5 and :20` | The paper's bootstrap is a clustered bootstrap with R = 250; its resampling type is not stated for the simulations, and wild or Webb bootstrap-t is not tested. So the paper cannot rank WCR against CR2 or CR3. The HC3 ranking is for linear and GLM fits with balanced clusters of 5, not two-way designs with about 9 sector clusters. | weak | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#SEATS-W weekly seat-gain test (2022 House)\|SEATS-W weekly seat-gain test (2022 House)]] | supports | Panel PL and PC coverage is poor with short panels (5 observations per cluster) and with exchangeable correlation, where coverage falls as panels lengthen. With decaying AR(1) correlation, coverage rises toward 0.95 as panels lengthen. The paper says some care is needed with panel estimators in short panels or with non-decaying autocorrelation. (Sec 6 discussion (p.20, text ~lines 1412-1421); Fig 6 and Appendix A (p.35-36); Sec 7 (p.28)) | sgs_seats.R: primary inference is a fixed-X residual moving-block bootstrap within cycle (3 and 5 weeks, Holm within family). Newey-West HAC with Bartlett lags 2, 4 and 8 is a secondary check and carries no p-value. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_seats.R:271-277 (HAC and bootstrap design comments); :588-589 (Holm within family)` | The paper's panel estimators apply to many short clusters. SEATS-W is one weekly series inside each cycle with no cluster dimension, so only the general point transfers: HAC is fragile when dependence does not decay. The paper gives no evidence on a single series. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Window error: summary says the study has 9 sectors over the 2020-2026 cycles. sgs_config.R DEFAULT_WINDOW is 2020-2025 (about 6 years), 2026 is partial and excluded by default, and the fallback is the latest run of at least 10 years. The 9 study sectors are correct.
> - Summary issue: The 5-to-9 sector cluster range in the summary is not traced. Config lists 9 study sectors and MIN_SECTORS = 2; the lower bound of 5 needs a per-test count from the study outputs.
> - Summary issue: Overstatement: summary says panel NW and PCSE fail when panels are short or dependence is exchangeable. The paper says coverage deteriorates and needs care, and that AR(1) improves coverage toward 0.95.
> - Summary issue: Threshold misattribution: the 50 to 100 cluster threshold for CL-0 comes from Exp IV (Gaussian, binomial, Poisson). Exp III (beta, zero-truncated and zero-inflated Poisson) is run at G = 100 only, so near-0.95 coverage across beta and count models at G = 50 to 100 is not shown.
> - Summary issue: At G = 10 the paper says standard errors are underestimated in most cases, not all; HC3 is the exception for binomial and Poisson. The summary drops this qualifier.
> - Summary issue: The summary's judgment that WCR bootstrap-t is the right instinct is analyst opinion, not supported by this paper. vcovBS offers wild, Mammen, Webb and Rademacher types and the paper says Webb may help with few clusters, but these are described, not evaluated.
> - Summary issue: Missing point: the paper suggests multi0 (basic sandwich in place of the intersection term) when each id-time cell holds one observation. sgs_review.R has no multi0 option. Check whether sector-year cells are singletons before treating Eq 20 without multi0 as the paper's preferred form.
> - Summary issue: Removed: the code has no sandwich fix = TRUE option, so the claim that a non-positive two-way diagonal is eigen-fixed is wrong. The code only records the non-positive case as a note.
> - Summary issue: Dropped related link imai-2021-fixed-effects-regression: this paper does not discuss two-way fixed-effects causal inference, and the stated complement is not in the text.
> - Summary issue: The 1.5 to 2 times SE inflation is from the InstInnovation negative binomial hurdle model (803 company clusters, text ~lines 943-1063). It must be quoted with that window and denominator, not as a general factor.
> - Summary issue: Verification scope: all text claims were checked by search in the text file; figure contents were not read (text and captions only). No paul_markup exists (n_annots 0). No bearing is refuted: each is softened to what the paper and code support.

## Gaps and flags

- No CR2 or CR3 small-G cluster-robust error is reported alongside CR1 in the sector growth study, although the paper finds HC3 best at G = 10 and the study has 5 to 9 sector clusters.
- The two-way CR1 floor of 3 clusters per dimension is far below the paper's 50 to 100. Its df = min(G) - 1 t-test is untested at the study's 9 x 4 size.
- There is no cross-check of the hand-rolled two-way CR1 against sandwich::vcovCL on the same design (a second path for DE-062).
- This entry is a methods and software paper. No conflict with D-021, D-025, D-104..D-108 or the rejected SNA proposals.
- The 1.5 to 2 times SE inflation figure belongs to the Aghion et al. firm panel (803 company clusters). Quote it with that window and denominator, never as a general Mentat factor.
- Bearings are agent analysis and stay UNVERIFIED until a second path reproduces them (DE-062).

There are no annotated-bibliography entries or annotations for this work (n_annots 0), so there is no takeaway to check or flag as stale. The built sector growth study already follows the paper's main lessons: cluster at the dependence level, distrust few clusters, and demote HAC.

## Related papers

- [[zeileis-nd-object-oriented-computation]] — builds-on — The paper cites Zeileis 2006b for the object-oriented sandwich framework that vcovCL and vcovPL extend. Verified in the reference list and Sec 3.
- [[zeileis-nd-econometric-computing-with]] — builds-on — The paper cites Zeileis 2004 for the HC and HAC estimators reused in vcovPL and the HC0-HC3 corrections (text ~line 336 and ~line 773). Verified.

### Linked from
- [[mackinnon-2017-wild-bootstrap-inference]] — complements — Checked: the Zeileis text documents an R implementation of clustered covariances including multiway clustering and CR1-type corrections, so it covers how the study's CR1 and two-way SEs are computed. Kept.