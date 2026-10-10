---
type: paper
title: "Object-Oriented Computation of Sandwich Estimators"
authors: ["Achim Zeileis"]
year: "2006"
venue: "Journal of Statistical Software 16(9), 1-16 (text is a slightly modified reprint, in sandwich package vignette form, of Zeileis 2006)"
doi: "10.18637/jss.v016.i09"
category: "Causal & Methodological"
systems: ["mentat-sector-growth-study"]
components: ["mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.seats-weekly-test"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/zeileis-nd-object-oriented-computation.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\Codex\\2026-09-26\\loo\\work\\lobbying_followup\\Rlib\\sandwich\\doc\\sandwich-OOP.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Mentat Complete Files\\FORECAST_OVERNIGHT_2026-10-07\\staging\\runtime\\Rlib\\sandwich\\doc\\sandwich-OOP.pdf"]
sha12: ["2c2eeae381ad"]
tags: [paper, causal-methodological, type/paper, theme/causal-methodological, evidence/verified]
---

# Object-Oriented Computation of Sandwich Estimators

**Achim Zeileis (2006).** *Journal of Statistical Software 16(9), 1-16 (text is a slightly modified reprint, in sandwich package vignette form, of Zeileis 2006).* [10.18637/jss.v016.i09](https://doi.org/10.18637/jss.v016.i09). Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> Zeileis (JSS 16(9), 2006; read as the sandwich package vignette) recasts heteroskedasticity-consistent (HC) and autocorrelation-consistent (HAC) covariance estimation as a modular sandwich S = (1/n) B M B, where the bread B is the inverse Hessian and the meat M is the variance of the empirical estimating functions. R generics bread(), estfun(), meat(), meatHC() and meatHAC() let one computation serve lm, glm, survreg and other models. Two illustrations show what the correction does. In a Poisson fit to simulated negative-binomial data (n = 250), the naive z for I(x^2) is -2.12 (p = .034); with the sandwich it is -1.35 (p = .18). In a probit on Fair's affairs data (n = 601), age moves from p = .033 to p = .051. The author's own caveat carries the most weight: sandwich errors help only if the point estimates stay consistent, they cost power when the model is right, and the package does no inference itself. For Paul's systems the paper is a method source, not evidence. The Mentat sector-growth study hand-codes the same estimators in base R: HC1 (meat as an outer product with the n/(n-k) factor) as a reference-only error, CR1 and two-way cluster errors, and, in SEATS-W, a Bartlett-weighted Newey-West HAC at 2, 4 and 8 week lags kept secondary with no p-value. The way the system is built already follows the paper's caveat, since primary inference comes from wild-cluster and block bootstraps. The paper's HC meat also ignores cluster dependence, which the study handles separately with CR1. One real gap: HC1 and CR1 are cross-checked against statsmodels in a test, but no comparable second-path check was found for the hand-coded HAC (for example against sandwich::NeweyWest), so under DE-062 those HAC numbers stay UNVERIFIED.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** How can sandwich (robust) covariance matrix estimators be computed in general parametric models fitted by estimating functions, not only in lm(), so that inference such as partial z or t tests is robust to some model misspecification.

**Theory.** M-estimation and estimating-function theory (White 1994; Stefanski and Boos 2002). Sandwich form S = B M B with B the inverse expected negative derivative of the estimating function and M the variance of the estimating function (Eq. 4-6, p.2-3). For models where the objective depends on x and theta only through the linear predictor, the estimating function is r(y, eta) times x (Eq. 7-8, p.4), which justifies HC-type meat estimators. Consistency of theta-hat still requires correctly specified estimating functions (p.3, p.13).

**Data.** No empirical sample in the theory sections. Illustration 1 (Sec 5.1, p.9-11): simulated count data, n = 250, one regressor x ~ N(0,1), y negative binomial with log-mean 1 + x and size 1, seed 123, R 3.5.0 RNG. Illustration 2 (Sec 5.2, p.11-13): Fair's affairs data from the AER package, n = 601 individuals, response number of affairs (tobit) and binary affairs > 0 (probit); covariates age, yearsmarried, religiousness, occupation, rating. Benchmarks are Greene (2003) Table 22.3 values.

**Method.** Object-oriented R infrastructure in the sandwich package: generics bread() (default Hessian-based bread, Eq. 9, p.5), estfun() (n x k matrix of empirical estimating functions, p.6), meat() outer-product estimator (Eq. 10, with optional n/(n-k) adjustment, p.6), meatHAC() with weights over lags (Eq. 11, p.7), meatHC() with diagonal omega(r, hat, df) (Eq. 12, p.8), and sandwich() = (1/n) B M B (p.8-9). Wrappers vcovHAC(), vcovHC(), kernHAC(), NeweyWest(), weave(). Inference via lmtest coeftest() and waldtest(), car linear.hypothesis(). Methods are demonstrated for glm (poisson, quasipoisson, glm.nb), survreg via tobit(), and glm probit.

**Identification.** Robustness comes from the sandwich variance, not from a new estimator. Validity needs consistency of theta-hat under the assumed estimating function (p.3, p.13). HC extension beyond HC0/HC1 additionally needs a linear-predictor structure, a model.matrix method, and hatvalues method for HC2-HC4 (p.8). No formal asymptotic theorem is proven in this paper; it refers to White (1994) Theorem 6.10 and Lumley and Heagerty (1999).

**Scope.** Parametric regression models fitted by estimating functions in R: linear models, GLMs (Poisson, negative binomial, quasi-Poisson, probit), nls, survreg (tobit via AER), coxph, rlm. Cross-section (HC) and time-series (HAC) settings. Era: 2006 software paper; R 2.x-era package sandwich. Not an inference package itself.

## Key findings

- Sandwich form S = B M B with M = VAR[psi] and B = inverse expected Hessian (Eq. 4-6, p.2-3); estimator is S-hat = (1/n) B-hat M-hat B-hat (p.8-9).
- Poisson fit on overdispersed NB data: naive z-test of I(x^2) is -2.12, p = 0.034; sandwich SE 0.0363 gives z = -1.35, p = 0.18 (Sec 5.1, p.10-11).
- Quasi-Poisson gives the same conclusion: I(x^2) SE 0.0506, z = -0.97, p = 0.33; glm.nb gives coefficient -0.0527, SE 0.0649, z = -0.81, p = 0.42 (p.11).
- Poisson coefficients track the true conditional mean (intercept 1.0633, x 0.9961, true values 1 and 1); only dispersion is misspecified (p.10).
- Tobit on Fair's affairs (n = 601): HC SEs only slightly differ from model SEs; religiousness SE 0.4038 vs sandwich 0.3999; rating -2.2850 with SE 0.4078 vs sandwich 0.3935; age SE 0.0791 vs sandwich 0.0889 (p.12).
- Tobit: age p-value moves from 0.0234 to 0.0437 with sandwich, still significant at 5 percent; occupation remains insignificant (0.200 vs 0.185) (p.11-12).
- Probit on Fair's affairs: age z = -2.13, p = 0.0328 (model) becomes z = -1.95, p = 0.0508 (sandwich), a change in significance at 5 percent; rating z = -5.19 to -5.12 (p.12-13).
- Author's caveat: sandwich errors lose power when the model is correctly specified, and are useful only if the estimates remain consistent, i.e. misspecification causes no bias (p.14).
- The paper claims the same qualitative interpretations for tobit and probit under HC and model-based SEs, so HC is not a power-free default (p.12-13).

## Limitations

- A
- u
- t
- h
- o
- r
-  
- s
- t
- a
- t
- e
- s
-  
- t
- h
- e
-  
- s
- a
- n
- d
- w
- i
- c
- h
-  
- p
- a
- c
- k
- a
- g
- e
-  
- c
- o
- n
- t
- a
- i
- n
- s
-  
- n
- o
-  
- i
- n
- f
- e
- r
- e
- n
- c
- e
-  
- f
- u
- n
- c
- t
- i
- o
- n
- s
-  
- (
- p
- .
- 1
- 4
- )
- .
-  
- C
- o
- n
- s
- i
- s
- t
- e
- n
- c
- y
-  
- u
- n
- d
- e
- r
-  
- m
- i
- s
- s
- p
- e
- c
- i
- f
- i
- c
- a
- t
- i
- o
- n
-  
- i
- s
-  
- o
- n
- l
- y
-  
- p
- a
- r
- t
- l
- y
-  
- u
- n
- d
- e
- r
- s
- t
- o
- o
- d
-  
- o
- u
- t
- s
- i
- d
- e
-  
- l
- i
- n
- e
- a
- r
-  
- m
- o
- d
- e
- l
- s
-  
- (
- p
- .
- 1
- 4
- )
- .
-  
- H
- C
- 2
- -
- H
- C
- 4
-  
- a
- n
- d
-  
- H
- A
- C
-  
- w
- e
- i
- g
- h
- t
- s
-  
- a
- r
- e
-  
- d
- e
- s
- c
- r
- i
- b
- e
- d
-  
- o
- n
- l
- y
-  
- b
- y
-  
- r
- e
- f
- e
- r
- e
- n
- c
- e
-  
- t
- o
-  
- Z
- e
- i
- l
- e
- i
- s
-  
- (
- 2
- 0
- 0
- 4
- )
-  
- a
- n
- d
-  
- A
- n
- d
- r
- e
- w
- s
-  
- (
- 1
- 9
- 9
- 1
- )
-  
- (
- p
- .
- 7
- -
- 8
- )
- .
-  
- E
- x
- a
- m
- p
- l
- e
- s
-  
- u
- s
- e
-  
- o
- n
- e
-  
- s
- i
- m
- u
- l
- a
- t
- e
- d
-  
- d
- a
- t
- a
- s
- e
- t
-  
- a
- n
- d
-  
- o
- n
- e
-  
- r
- e
- a
- l
-  
- d
- a
- t
- a
- s
- e
- t
- ;
-  
- n
- o
-  
- s
- i
- m
- u
- l
- a
- t
- i
- o
- n
-  
- c
- o
- v
- e
- r
- a
- g
- e
- ,
-  
- n
- o
-  
- c
- o
- v
- e
- r
- a
- g
- e
- -
- r
- a
- t
- e
-  
- s
- t
- u
- d
- y
- .
-  
- F
- i
- n
- i
- t
- e
- -
- s
- a
- m
- p
- l
- e
-  
- n
- /
- (
- n
- -
- k
- )
-  
- a
- d
- j
- u
- s
- t
- m
- e
- n
- t
-  
- h
- a
- s
-  
- n
- o
-  
- t
- h
- e
- o
- r
- e
- t
- i
- c
- a
- l
-  
- s
- u
- p
- p
- o
- r
- t
-  
- f
- o
- r
-  
- n
- o
- n
- -
- l
- i
- n
- e
- a
- r
-  
- m
- o
- d
- e
- l
- s
-  
- (
- p
- .
- 6
- )
- .
-  
- R
- e
- s
- t
- r
- i
- c
- t
- i
- o
- n
-  
- t
- e
- s
- t
- i
- n
- g
-  
- v
- i
- a
-  
- s
- a
- n
- d
- w
- i
- c
- h
-  
- i
- s
-  
- o
- n
- e
-  
- o
- f
-  
- s
- e
- v
- e
- r
- a
- l
-  
- s
- t
- r
- a
- t
- e
- g
- i
- e
- s
- ;
-  
- G
- o
- d
- f
- r
- e
- y
-  
- (
- 2
- 0
- 0
- 6
- )
-  
- a
- n
- d
-  
- K
- i
- e
- f
- e
- r
-  
- a
- n
- d
-  
- V
- o
- g
- e
- l
- s
- a
- n
- g
-  
- (
- 2
- 0
- 0
- 2
- )
-  
- a
- r
- e
-  
- m
- e
- n
- t
- i
- o
- n
- e
- d
-  
- a
- s
-  
- a
- l
- t
- e
- r
- n
- a
- t
- i
- v
- e
- s
-  
- (
- p
- .
- 1
- 4
- )
- .

## Short quotes

> Sandwich covariance matrix estimators are a popular tool in applied regression modeling
> the sandwich estimators are only useful if the parameters estimates are still consistent
> Object orientation can be achieved by providing a few extractor functions

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | method-source | Meat is the outer product of estimating functions with optional n/(n-k) adjustment (Eq. 10, p.6); sandwich errors lose power under correct specification and are useful only if estimates stay consistent; probit age p moves from .033 to .051 (n = 601, Sec 5.2). (Eq. 10 p.6; Sec 5.2 p.12-13; Sec 6 p.14) | OLS with sector baselines reports HC1 = (n/df) A X'uu'X A as a reference-only error (sgs_estimate.R lines 35-36); CR1 at sgs_estimate.R lines 44-45; primary inference is a wild cluster restricted bootstrap-t with a t_cr1 statistic (header lines 1-12, sgs_estimate.R line 264). `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:35` | HC1 and CR1 formulas match Eq. 10 with cluster sums added. The paper's meat assumes independent rows, so within-sector dependence needs CR1, which the paper does not treat. Statsmodels cross-check is in code/tests/test_estimate.py lines 44-45, not tests/ at repo root. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#SEATS-W weekly seat-gain test (2022 House)\|SEATS-W weekly seat-gain test (2022 House)]] | method-source | HAC meat is a weighted sum of lagged estimating-function cross-products (Eq. 11, p.7) behind NeweyWest-type wrappers; no coverage study is reported, and Kiefer-Vogelsang fixed-b testing is named as an alternative (Sec 6, p.14). (Eq. 11 p.7; Sec 3.2 p.7-8; Sec 6 p.14) | SEATS-W computes Bartlett Newey-West HAC by hand at lags 2, 4 and 8 weeks within cycle with weights 1 - l/(L+1) and n/(n-p) (sgs_seats.R line 277 and lines 389-400); reported as secondary with no p-value. Primary inference is a residual moving-block bootstrap with fixed regressors, plus a recursive block bootstrap for dynamic models. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_seats.R:391` | Lags, Bartlett weights and n/(n-p) factor match the paper's Eq. 11 setup. The paper gives no finite-sample guarantee, so the caveat that HAC is secondary applies. The 17-week figure appears in the code comment at sgs_seats.R lines 71-77 as an example of an interval, not as a stated cycle length. No second-path check against sandwich::NeweyWest was found in code/tests. | moderate | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary says the paper's HC meat ignores cluster dependence and the study handles it with CR1. The meat assumes independent rows, which is verified, but the paper never discusses clustering, so this is the reviewer's inference, not a paper claim.
> - Summary issue: Summary's gap about the missing HAC second-path check is verified: no NeweyWest or HAC check exists in code/tests. The HAC figures stay UNVERIFIED under DE-062.
> - Summary issue: Summary's numbers are all verified: Poisson z -2.12 (p .034) to -1.35 (p .18); probit age .033 to .051; n = 250 and n = 601; statsmodels cross-check of HC1 and CR1. The path tests/test_estimate.py should read code/tests/test_estimate.py.
> - Summary issue: Extraction scope says R 2.x-era package. The paper's simulation calls RNGversion 3.5.0 (Sec 5.1, p.9), so the text is an R 3.5-era reprint. Correct the scope line.
> - Summary issue: Extraction limitations say the n/(n-k) adjustment has no theoretical support for non-linear models (p.6). The paper says only that the adjustment lacks theoretical justification, with no linear or non-linear qualifier. Soften.
> - Summary issue: Extraction key finding says the paper claims the same qualitative interpretations for tobit and probit. The paper says results are qualitatively identical for tobit but the picture is only similar for probit, and the probit age term does cross 0.05 (0.0328 to 0.0508). Restrict the claim to tobit.
> - Summary issue: Extraction citation of the 2006 paper as year 2006 with JSS 16(9) and DOI 10.18637/jss.v016.i09 is verified against the paper's own self-citation (text line 885). Coverage: all numeric claims in ext and rel were checked against the text; theory sections 2-3 were spot-checked only.

## Gaps and flags

- The hand-coded Newey-West HAC in sgs_seats.R:391-400 has no second-path reproduction such as sandwich::NeweyWest(lag = L, prewhite = FALSE, adjust = TRUE). HC1 and CR1 do have statsmodels checks at tests/test_estimate.py:44-45. Under DE-062 the HAC figures stay UNVERIFIED.
- No coverage or size simulation exists for HC1 or HAC at the study's sample sizes (sector-year panels; about 17 weeks per cycle). The paper offers none either, so the reference errors carry no finite-sample guarantee.
- The paper's HC meat assumes independent rows. Any Mentat or Melange regression quoting HC1 on panel data without clustering would understate uncertainty. The growth study avoids this, but the rule is not stated system-wide.
- Paper is a software and method paper with no substantive political finding; it does not bear on SNA, LDA or reciprocity rules.
- Paper figures (z -2.12 to -1.35; p .033 to .051) come from one simulated dataset (n = 250) and one cross-section (n = 601). They illustrate the estimator and do not validate it.

Paul has no annotated-bibliography entry and no markup for this work (n_annots 0), so there is no takeaway to compare against. The built system already follows the paper's central caveat: HC1 and HAC are kept as reference or secondary errors, and bootstraps carry inference. This fits Paul's directives to present regressions as R summary() output with a bootstrap and to put trend over significance.

## Related papers

- [[zeileis-nd-econometric-computing-with]] — builds-on — Real item. The 2006 text cites Zeileis 2004 JSS 11(10) as the linear-model predecessor, and the 2004 text covers HC4 and Andrews-type HAC weights, matching the claim that this paper only cites those details.
- [[zeileis-nd-various-versatile-variances]] — extends — Real item. Its title is an object-oriented implementation of clustered covariances in R, which is the CR extension of the same bread and meat framework that the growth study uses via CR1.

### Linked from
_None._