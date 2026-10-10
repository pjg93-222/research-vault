---
type: paper
title: "Wild Bootstrap Inference for Wildly Different Cluster Sizes"
authors: ["James G. MacKinnon", "Matthew D. Webb"]
year: 2017
venue: "Queen's Economics Department Working Paper 1314 (reissued and corrected 2019-08); published as Journal of Applied Econometrics 32 (2017) 233-254"
doi: ""
category: "Causal & Methodological"
systems: ["mentat-sector-growth-study"]
components: ["mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.growth-spending-regressions"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/mackinnon-2017-wild-bootstrap-inference.pdf]]"
original_paths: []
sha12: ["90c86220a8fd"]
tags: [paper, causal-methodological]
---

# Wild Bootstrap Inference for Wildly Different Cluster Sizes

**James G. MacKinnon, Matthew D. Webb (2017).** *Queen's Economics Department Working Paper 1314 (reissued and corrected 2019-08); published as Journal of Applied Econometrics 32 (2017) 233-254.* Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> MacKinnon and Webb (Queen's WP 1314, corrected 2019; JAE 2017) study when cluster-robust (CRVE) t tests with t(G-1) critical values give correct size, and when the wild cluster bootstrap does better. Using Monte Carlo designs with 50 or 100 clusters sized like U.S. states (4 to 242 observations), CPS placebo laws (547,518 obs, 51 states) and a Colombian replication, they show the rule of 42 fails once cluster sizes are unequal: t(G-1) rejects up to 0.107 at nominal .05 with 50 state-sized clusters and 0.084 even with 100. The restricted wild cluster bootstrap (WCR) stays near .05 in those designs (0.049 to 0.053). The bootstrap breaks when few clusters are treated: with one treated cluster of 50 the t(G-1) test rejects 77.75 percent of the time while the WCR never rejects; WCR is safe roughly for 7 <= G1 <= G-7 with equal clusters and 6 <= G1 <= G-5 with unbalanced clusters at G of 20 to 32. The effective-clusters G* correction is erratic. Rademacher weights are not advised below about 12 clusters. For Paul's systems the paper is the method source for one component: the Mentat sector growth study already runs a WCR bootstrap-t by sector with Webb six-point weights, enumerating all 6^G draws when small, and labels per-sector terms resting on one cluster DESCRIPTIVE, which matches the paper's G1 = 1 warning. The open point is scale: the study runs with 3 to 9 sector clusters, below the 12-cluster floor of every design the paper tests, so the paper supports the procedure choice but does not certify its size at G = 9 or 5. No network, lobbying-edge or influence claim follows.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** When is inference with cluster robust variance estimator t statistics (CRVE, with t(G-1) critical values) reliable, and which alternatives (effective number of clusters G* critical values, restricted or unrestricted wild cluster bootstrap) work when cluster sizes are very unequal or the number of treated clusters is small?

**Theory:** Asymptotic validity of the CRVE under White (1984) assumptions A1 (number of clusters grows), A2 (same within-cluster error correlation) and A3 (equal cluster sizes). Wild cluster bootstrap with Rademacher weights (Cameron, Gelbach and Miller 2008; Webb 2014 for G below about 12). Effective number of clusters G* of Carter, Schnepel and Steigerwald (2017). Section 6 analytic argument: with G1 fixed, the treatment coefficient is inconsistent, and the variance of the residual sum for a treated cluster shrinks by the factor (M1 minus N1) over M1 under IID errors (eq. A.2).

**Data:** Monte Carlo (Sections 4-6, Appendix A.2, A.6): simulated samples of N = 2000 observations (G = 50 or 100 clusters, equal sizes or sizes proportional to U.S. state populations without DC; smallest state cluster 4 obs and largest 242 for G = 50; 400,000 replications; 399 bootstrap draws). Appendix A.6 uses N = 50G with G = 12, 16, 20, 25, 32. Placebo laws (Section 7): U.S. Current Population Survey, women aged 25-50, 1979-1999, log weekly earnings, earnings under 20 dollars dropped, 547,518 micro observations, 51 states including DC, year of treatment start 1985-1995; 100,000 or 10,000 replications; G* computed on a 1/100 subsample; rho-hat about 0.031-0.032. Aggregate version: 1071 state-year cells of 21 obs each. Empirical example (Section 8): Angrist and Kugler (2008) Colombia, men 181,882 obs in 32 departments (aggregate 288 dept-year cells); teenage boys 22,141 obs in 38 departments (aggregate 340 cells); 99,999 bootstrap samples.

**Method:** Monte Carlo rejection frequencies at nominal .05 (and .10 and .01 in places) for: CRVE t with t(N-2), t(G-1), t(G*-1) and t(G*) critical values (G* at rho = 0.99 and rho-hat); restricted wild cluster bootstrap (WCR) with symmetric P values; unrestricted bootstrap (WCU, Appendix A.3); HCCME t with normal critical values. Rho-hat from the cluster-dummy regression. Power comparison of micro versus aggregate estimators (Appendix A.8) with bootstrap tests.

**Identification:** Not a causal identification paper. Test size is the object: the null beta equal to zero holds in the simulations and placebo laws, and the question is whether CRVE t tests and bootstrap P values have correct size when cluster sizes are unequal and the number of treated clusters is small.

**Scope:** Inference in linear models with clustered errors, with a focus on U.S. state-level clusters. Simulation designs are synthetic; placebo-law and empirical examples use CPS 1979-1999 earnings and Colombian household data from 1995-2000. Results are for G from 12 to 200 clusters and at most 2000 observations in the main Monte Carlo designs.

## Key findings

- Section 4, Table 1 (p.6): with 50 equal-sized clusters, t(G-1) rejection rises with the regressor's within-cluster correlation and exceeds 0.065 when rho_x is 1 (value 0.06568 at rho_eps 0). Wild bootstrap rejections in Tables 1 and 3 all lie close to .05 (Table 1 range 0.04924 to 0.05084, p.8).
- Section 4, Table 2 (p.7): with 50 state-sized clusters (sizes 4 to 242), t(G-1) rejection is 0.05784 with both correlations zero and reaches 0.10718 at rho_eps 0.9 and rho_x 1 (text says 0.1073, a small mismatch with the table). Bootstrap rejections range 0.04905 to 0.05277 (p.10).
- Section 4 (p.10): with state-sized clusters, t(G*-1) critical values underreject severely when rho_x is large; worst case rejection 0.0166 at rho_eps 0 and rho_x 0.6, where average G* is 9.43 using rho-hat and 5.97 using rho 0.99.
- Section 4, Table 4 (p.9): with 100 state-sized clusters (each state appearing twice), t(G-1) rejection reaches 0.08442 (rho_eps 0.9, rho_x 1); the wild cluster bootstrap never exceeds 0.05124. The rule of 42 fails under unequal clusters even at 100 clusters.
- Section 5, Figure 1 (p.11): for DiD with t(G-1) critical values, severe overrejection when G1 (treated clusters) or G minus G1 is near 0; with equal-sized clusters rejection is near .05 for G1 from 17 to 34. Overrejection is worse when the smallest clusters are treated first.
- Section 5, Figure 2 (p.12): t(G*-1) critical values overreject except when the number of treated clusters is very small or very large; extreme underrejection at G1 = 1 and G1 = 49. Figure 3 (p.13): restricted wild bootstrap underrejects severely when G1 or G minus G1 is very small but works extremely well for G1 from about 7 to 43 (equal-sized clusters).
- Section 6 (pp.16-17, Figure 4): with G1 = 1 and 50 equal clusters, the 0.975 quantile of the CRVE t statistic is 14.226 versus 2.010 for t(49); the t(G-1) test rejects 77.75 percent of the time at .05; restricted wild bootstrap tests never reject at conventional levels when G1 = 1 in the experiments.
- Section 6 (p.19): for DiD with equal-sized clusters, the wild cluster bootstrap (restricted or unrestricted) can probably be used safely for 7 <= G1 <= G-7; likely misleading below 4 or above G-4. Appendix A.6 (p.39): with unbalanced clusters and G of 20 or 32, the restricted bootstrap is acceptable for 6 <= G1 <= G-5. Exact cutoffs are data dependent.
- Section 7 (pp.21-23): placebo laws with micro CPS data; HCCME t tests reject more than 50 percent of the time at .05 for all G1; t(G-1) CRVE rejects at least 0.063 whenever many states are treated and about 0.07 half the time, with severe overrejection when few states are treated; restricted bootstrap underrejects severely when G1 is very small but performs extremely well for larger G1 (Figure 9).
- Section 7 (p.24, Figure 10): with aggregated state-year data (balanced cells), CRVE t(G-1) performs very well for most G1 and slightly underrejects; restricted wild bootstrap performs extremely well for G1 at least 5. Appendix A.7 reports unrestricted bootstrap rejection 0.7184 at .05 for G1 = 1 with aggregate data.
- Section 8, Table 5 Panel A (p.27): adult men (32 departments, N = 181,882), rural 1998-2000 coefficient 0.1219 with s.e. 0.0359 (t = 3.395); t(G-1) p = 0.002, bootstrap p = 0.028, t(G*-1) p = 0.059 with G* = 3.402. Rural 1995-1997 has t(G-1) p = 0.045 but bootstrap p = 0.186. Only the rural 1998-2000 term is significant at .05 under the bootstrap.
- Section 8, Table 5 Panel B (p.27): teenage boys (38 departments, N = 22,141), rho-hat 0.155, G* between 8.9 and 12.9; both rural coefficients significant under t(G-1) (p 0.028 and 0.048) and bootstrap (p 0.030 and 0.050), but t(G*-1) p-values are 0.051 and 0.067. Urban coefficients have p of at least 0.6.

## Limitations

- A
- u
- t
- h
- o
- r
- s
-  
- s
- t
- a
- t
- e
- :
-  
- p
- l
- a
- c
- e
- b
- o
- -
- l
- a
- w
-  
- G
- *
-  
- u
- s
- e
- s
-  
- a
-  
- 1
- /
- 1
- 0
- 0
-  
- s
- u
- b
- s
- a
- m
- p
- l
- e
- ;
-  
- B
- e
- l
- l
- -
- M
- c
- C
- a
- f
- f
- r
- e
- y
-  
- a
- n
- d
-  
- I
- m
- b
- e
- n
- s
- -
- K
- o
- l
- e
- s
- a
- r
-  
- c
- o
- r
- r
- e
- c
- t
- i
- o
- n
- s
-  
- n
- o
- t
-  
- s
- t
- u
- d
- i
- e
- d
- ;
-  
- I
- b
- r
- a
- g
- i
- m
- o
- v
- -
- M
- u
- l
- l
- e
- r
-  
- d
- o
- e
- s
-  
- n
- o
- t
-  
- a
- p
- p
- l
- y
-  
- t
- o
-  
- m
- o
- s
- t
-  
- d
- e
- s
- i
- g
- n
- s
- ;
-  
- R
- a
- d
- e
- m
- a
- c
- h
- e
- r
-  
- w
- e
- i
- g
- h
- t
- s
-  
- n
- o
- t
-  
- r
- e
- c
- o
- m
- m
- e
- n
- d
- e
- d
-  
- b
- e
- l
- o
- w
-  
- a
- b
- o
- u
- t
-  
- 1
- 2
-  
- c
- l
- u
- s
- t
- e
- r
- s
- .
-  
- B
- o
- o
- t
- s
- t
- r
- a
- p
-  
- r
- e
- l
- i
- a
- b
- i
- l
- i
- t
- y
-  
- d
- e
- p
- e
- n
- d
- s
-  
- o
- n
-  
- G
- 1
-  
- w
- i
- t
- h
-  
- n
- o
-  
- c
- l
- e
- a
- n
-  
- c
- u
- t
- o
- f
- f
- ,
-  
- a
- n
- d
-  
- h
- e
- t
- e
- r
- o
- s
- k
- e
- d
- a
- s
- t
- i
- c
- i
- t
- y
-  
- b
- y
-  
- t
- r
- e
- a
- t
- m
- e
- n
- t
-  
- s
- t
- a
- t
- u
- s
-  
- c
- o
- u
- l
- d
-  
- c
- h
- a
- n
- g
- e
-  
- r
- e
- s
- u
- l
- t
- s
- .
-  
- G
- *
-  
- d
- e
- p
- e
- n
- d
- s
-  
- s
- h
- a
- r
- p
- l
- y
-  
- o
- n
-  
- r
- h
- o
- .
-  
- N
- o
-  
- G
- *
-  
- f
- o
- r
-  
- j
- o
- i
- n
- t
-  
- t
- e
- s
- t
- s
- .
-  
- P
- u
- r
- e
- -
- t
- r
- e
- a
- t
- m
- e
- n
- t
-  
- r
- e
- s
- u
- l
- t
- s
-  
- c
- h
- a
- n
- g
- e
- d
-  
- a
- f
- t
- e
- r
-  
- a
-  
- r
- h
- o
-  
- c
- o
- r
- r
- e
- c
- t
- i
- o
- n
-  
- (
- A
- p
- p
- e
- n
- d
- i
- x
-  
- A
- .
- 2
- )
- ,
-  
- u
- n
- e
- x
- p
- l
- a
- i
- n
- e
- d
- .

## Short quotes

> we show that the rule of 42 no longer holds when the assumption of equal-sized clusters is relaxed (Section 1, p.2)
> the wild cluster bootstrap procedure always performs extremely well (Section 1, p.3)
> this procedure fails when a small number of clusters is treated (Abstract, p.1)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | method-source | With unequal state-sized clusters (G = 50, Table 2) CRVE t(G-1) rejection at nominal .05 runs from 0.0567 to 0.1078 across the rho grid (0.10718 at rho_eps 0.9, rho_x 1); at G = 100 (Table 4) it reaches 0.08442; the restricted wild cluster bootstrap stays between 0.04905 and 0.05277 (Table 2). The conclusion recommends WCR over t(G-1) and says G* critical values often improve matters but do not always work as well. (Section 4, Tables 2 and 4 (pp.7-10); Section 9 Conclusion (pp.25-26)) | Focal coefficients are tested with a wild cluster restricted bootstrap-t, null imposed, CR1 SE in the statistic, symmetric two-sided p, test-inverted 95 percent CI, B = 9,999 or full enumeration. Clusters are sectors for sector-year tests but champions (bioguide) for the H5 tests, so the analyst's sector-only system_side is overbroad. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:1-12; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_config.R:58-63` | The paper's size evidence is for Rademacher weights. The study uses Webb six-point weights, which this paper does not evaluate; it only cites Webb (2014) for G below about 12. Paper designs are linear models with G from 12 to 200. Whether the study's sector cluster sizes are near-balanced was not checked against study data in this pass; H5 bioguide clusters are the likely unequal-size case. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | qualifies | The paper says Rademacher weights should not be used when G is below about 12 and points to Webb (2014) for that regime, but does not itself test Webb six-point weights. Its evidence starts at G = 12 (Appendix A.6 uses G = 12, 16, 20, 25, 32; Sections 6 and 7 use larger G). It says the exact G1 cutoffs for safe bootstrap inference are data dependent. (Section 3 step 4 (p.5, Webb 2014 note); Section 6 (p.19); Appendix A.6 (p.39)) | The study clusters by its nine sectors (config STUDY_SECTORS lists nine), uses Webb six-point weights with exact enumeration when 6^G is at most 9,999, requires at least 3 sector clusters for LobbyView bootstrap inference (DESCRIPTIVE below), and prints the 2/6^G p floor for DESCRIPTIVE tests. MIN_SECTORS is 2, so runs with 2 clusters are possible in code. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_config.R:27-28, 43, 59-63, 113; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:258-266; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_rng.R:231-235` | The paper gives no rejection rates at G of 3 to 9 and no evidence on six-point weights. Bootstrap p-values at 3 to 9 sectors are therefore unverified by this paper and need the study's own placebo or simulated-null check, or Webb (2014), before being read as correctly sized. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | supports | With G1 = 1 (one treated cluster among 50 equal-sized clusters), the CRVE t(G-1) test rejects 77.75 percent of the time at .05, and the restricted wild bootstrap never rejects at any conventional level in these experiments. For equal-sized DiD the paper says the bootstrap can probably be used safely for 7 <= G1 <= G-7 and is likely misleading below 4 or above G-4. (Section 6, Figure 4 and text (pp.16-17); Section 6 p.19 for cutoffs; Section 5 Figure 1 for t(G-1) overrejection as G1 nears 0) | Per-sector terms resting on one sector cluster and saturated SHIFT terms are labelled DESCRIPTIVE with bootstrap lines as reference only. The review adds each year's share of absolute OLS weight for each focal term. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_review.R:13-14, 161, 258; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:266, 282-314, 370` | The paper's G1 is a count of treated clusters for a binary DiD regressor. For continuous growth terms the analogue is how concentrated OLS weight is across sectors; the study reports this per year but not per sector. The 4-to-7 cutoffs are for the bootstrap in equal-sized DiD and do not transfer as numeric cutoffs to continuous terms. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | supports | Conclusion: cluster-robust t inference should not be trusted even when the sample is large and the rule of 42 is satisfied, because overrejection with wildly unequal cluster sizes is serious; the paper's results are for one-way clustering. (Section 4, Tables 2 and 4 (pp.7-10); Section 7 (pp.21-23); Conclusion (pp.25-26)) | The two-way sector-by-year CR1 SE with min(G_sector, G_year) - 1 df is computed only as a sensitivity when both dimensions have at least 3 clusters, and is explicitly not the study's inference. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_review.R:7-10` | The paper tests one-way clustering only, so the claim that the two-way CRVE inherits the t(G-1) warning a fortiori is the analyst's reasoning, not a paper result. The about 7 years figure was not checked against study data. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Upper bound: summary says t(G-1) rejects up to 0.107 with 50 state-sized clusters; the grid maximum in Table 2 is 0.10776 (about 0.108) at rho_eps 0.8, rho_x 1. The 0.10718 cell is at rho_eps 0.9, rho_x 1.
> - Summary issue: Summary says the study runs with 3 to 9 sector clusters. Config MIN_SECTORS is 2; the 3-cluster threshold applies only to LobbyView bootstrap inference.
> - Summary issue: Summary says the paper supports the procedure choice of Webb six-point weights at 3 to 9 clusters. The paper evaluates Rademacher weights only and cites Webb (2014) for G below 12 without testing it.
> - Summary issue: Summary calls the G* correction erratic. The paper describes a directional pattern: overrejection when rho_x is large, underrejection when rho_eps is large, and sensitivity to rho. Erratic overstates or misdescribes it.
> - Summary issue: Summary says WCR is safe roughly for 6 <= G1 <= G-5 with unbalanced clusters at G of 20 to 32. That range comes from Appendix A.6 under one extreme unbalance design (gamma = 3, smallest cluster 8, largest 146 to 167) and should be stated as design-specific.
> - Summary issue: Summary says the study clusters by sector for every focal coefficient. Config says champions (bioguide) cluster the H5 tests.
> - Summary issue: Summary says the paper recommends WCR over G* critical values. The conclusion supports WCR generally, and says G* often helps but does not always work as well; the recommendation is not unconditional, since both fail when only a few clusters are treated.
> - Summary issue: Not verified in this pass: the brain note cluster count of 5 for the lobbying and champion reading, the about 7 years figure, and the near-balanced sector cluster sizes of the study data. These need the study data or the brain record to check.

## Gaps and flags

- No size check of the WCR bootstrap at the study's actual cluster counts (3 to 9 sectors); the paper's evidence starts at G = 12, so a placebo or simulated-null rejection rate on the study's own design would be needed before bootstrap p-values are read as correctly sized.
- No per-sector OLS-weight (leverage) share is reported for focal growth terms, the continuous-regressor analogue of the paper's G1 problem; leave-one-sector-out refits exist but are EXPLORATORY with HC1 only.
- The H5 champion-clustered tests (bioguide clusters, about 21 members) likely have unequal cluster sizes, the regime where the paper shows t(G-1) overrejection; whether the study reports cluster-size spread for those clusters was not confirmed.
- Brain note records 5 clusters for the lobbying and champion reading and the reply that it is supportive, not confirmed; the paper reinforces that caution since it gives no validation for WCR below 12 clusters.
- No conflict with D-021, D-025, D-104..D-108 or the rejected SNA proposals; the paper has no network or lobbying content.

Paul has no annotation entry for this work and no markup. The system as built already follows the paper's main recommendation (restricted wild cluster bootstrap over t(G-1), Webb weights for few clusters, DESCRIPTIVE labels when one cluster carries the variation), so the paper agrees with the design choice. It does not settle whether the bootstrap is correctly sized at 5 to 9 sectors, consistent with the brain's supportive-not-confirmed reading.

## Related papers

- [[zeileis-nd-various-versatile-variances]] — complements — Checked: the Zeileis text documents an R implementation of clustered covariances including multiway clustering and CR1-type corrections, so it covers how the study's CR1 and two-way SEs are computed. Kept.
- [[imai-2021-fixed-effects-regression]] — complements — Checked: the item is Imai and Kim on two-way fixed-effects regression for causal inference with panel data, matching the identification side of the year-FE sensitivity. Kept.
- [[callaway-2025-difference-differences-continuous]] — complements — Checked: the item is Callaway, Goodman-Bacon and Sant'Anna on DiD with a continuous treatment, matching the identification side for continuous growth terms. Kept.

### Linked from
_None._