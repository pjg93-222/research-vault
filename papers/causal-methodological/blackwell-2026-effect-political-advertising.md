---
type: paper
title: "Political Advertising after Citizens United: IPTW-FE for MSMs"
authors: ["Matthew Blackwell", "Soichiro Yamauchi"]
year: "2026"
venue: "Metadata hint said \"Working paper\". The text is a manuscript dated February 9, 2026 with \"Comments welcome\"; no venue, journal or series is stated."
doi: ""
category: "Causal & Methodological"
systems: ["melange-influence-engine", "melange-validation", "mentat-sector-growth-study", "terrier-canvass"]
components: ["mentat-sector-growth-study.growth-spending-regressions", "melange-validation.prospective-contract", "terrier-canvass.untapped-potential", "melange-influence-engine.competitiveness-multiplier"]
paul_annotated: true
paul_markup_count: 90
evidence_status: "verified"
source: "[[_sources/blackwell-2026-effect-political-advertising.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Causal & Methodological\\psfe.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Causal & Methodological\\psfe.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Causal & Methodological\\psfe.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Causal & Methodological\\psfe.pdf"]
sha12: ["559bf8cc8dc4"]
tags: [paper, causal-methodological, type/paper, theme/causal-methodological, evidence/verified]
---

# Political Advertising after Citizens United: IPTW-FE for MSMs

**Matthew Blackwell, Soichiro Yamauchi (2026).** *Metadata hint said "Working paper". The text is a manuscript dated February 9, 2026 with "Comments welcome"; no venue, journal or series is stated..* Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> Blackwell and Yamauchi (manuscript, 9 Feb 2026) ask whether TV ads run by independent interest groups after Citizens United changed vote shares and turnout. The sample is US Senate and gubernatorial general elections, 2010-2020, measured as weekly media-market by race panels (N = 467 market-races for Democratic-group ads, 623 for Republican). The methods problem is feedback: ads and campaign conditions shape each other, and groups may target persuadable markets on information the researcher cannot see. The authors fit a propensity model with a fixed effect for each unit (estimated by conditional MLE) and use it to build inverse-probability weights for a truncated marginal structural model. They prove root-N consistency when N and T grow together. In simulations (500 replications) the estimator beats IPTW without fixed effects, although some bias remains when heterogeneity is high.
> 
> Empirically, Democratic-group ads raise the Democratic two-party share, mostly by raising Democratic turnout, and the effect is concentrated after Trump's candidacy. Republican-group ads show no effect. Placebo tests on lagged outcomes come out near zero. The pre/post-Trump era differences are significant at alpha 0.1 (the text states no level for the main effects), the text gives no point estimates, and the Trump split is dated 2015 on p.30 but 2016 elsewhere.
> 
> For Paul's systems this is mainly a methods paper. It sharpens the caution on Mentat's fixed-effects regressions of sector spending on growth, and it shows a placebo-outcome check that the prospective receptivity contract lacks. It also gives one weak, party-asymmetric data point on Terrier's split between persuasion and mobilization. It says nothing about legislators, so it cannot support the competitiveness multiplier or any part of the flip model's money terms.

## Paul's annotation

From [[annotations/Causal & Methodological - Causal & Methodological - Annotated Bibliography|Causal & Methodological - Causal & Methodological - Annotated Bibliography]]:

> Blackwell, Matthew, and Soichiro Yamauchi. *The Effect of Political Advertising after Citizens United: Adjusting for Unmeasured Confounding  in Marginal Structural Models∗*, 9 Feb. 2026.

## Paul's markup in the PDF

- p1 Highlight (yellow): "Paraphrase: title fragment, 'Marginal Structural Model' heading (covered text truncated in the annotation)." — 
- p1 Highlight (yellow): "Paraphrase: abstract on interest groups becoming major sponsors of TV ads after Citizens United." — 
- p1 Highlight (green): "Paraphrase: abstract on estimating the partisan effects of group-sponsored ads for a fuller picture of voter behaviour." — 
- p1 Highlight (yellow): "Paraphrase: MSMs are a natural tool for estimating effects of ads whose strategy changes over the campaign." — 
- p1 Highlight (yellow): "Paraphrase: standard MSM approach requires no unobserved confounders, which may not hold in electoral data." — 
- p1 Highlight (green): "Paraphrase: proposed IPTW estimator with propensity-score fixed effects for time-constant unmeasured confounding in fixed-length treatment-history MSMs." — 
- p1 Highlight (yellow): "Paraphrase: the approach works even when the outcome is measured only once, as in this setting." — 
- p1 Highlight (yellow): "Paraphrase: the method needs some treatment switching within units." — 
- p1 Highlight (green): "Paraphrase: findings that Democratic-group ads are effective and mostly after 2016, against conventional wisdom." — 
- p2 Highlight (yellow): "Paraphrase: TV ads have been central to US politics since the 1950s." — 
- p2 Highlight (yellow): "Paraphrase: 2019-2020 cycle count of Senate ad airings (2.33 million)." — 
- p2 Highlight (yellow): "Paraphrase: 2019-2020 cycle count of presidential-race ad airings (2.35 million)." — 
- p2 Highlight (yellow): "Paraphrase: 2019-2020 cycle count of House ad airings (1.36 million)." — 
- p2 Highlight (yellow): "Paraphrase: each of those airing counts set a record." — 
- p2 Highlight (green): "Paraphrase: prior studies find ad airings persuade voters, with larger effects at lower office levels." — 
- p2 Highlight (yellow): "Paraphrase: early research found Citizens United raised Republican vote share in state legislative races." — 
- p3 Highlight (yellow): "Paraphrase: the dynamic deployment of ads is a major challenge to assessing effects." — 
- p3 Highlight (yellow): "Paraphrase: groups and campaigns respond to each other's ad decisions (feedback)." — 
- p3 Highlight (green): "Paraphrase: the feedback cycle implies time-varying confounding that can bias advertising-effect estimates." — 
- p3 Highlight (yellow): "Paraphrase: definition of sequential ignorability (no unmeasured confounders given covariate and treatment history)." — 
- p3 Highlight (pink): "Paraphrase: proposed modification to IPTW: unit-specific fixed effects in the propensity-score model used for inverse-probability weights." — 
- p4 Highlight (yellow): "Paraphrase: the result holds with a single outcome measurement after the final treatment." — 
- p4 Highlight (yellow): "Paraphrase: MSM parameters are estimated at the slower root-N rate, so asymptotic bias from fixed-effect estimation vanishes." — 
- p5 Highlight (yellow): "Paraphrase: pro-Democratic group ad effectiveness is driven mostly by the post-2016 era after Trump's candidacy." — 
- p5 Highlight (yellow): "Paraphrase: the method has null effects on several placebo tests, supporting that results are not driven by unmeasured confounding." — 
- p5 Highlight (yellow): "Paraphrase: pointer to the fixed-effect approach section." — 
- p6 Highlight (yellow): "Paraphrase: data period 2010 to 2020." — 
- p6 Highlight (yellow): "Paraphrase: sample is Senate and gubernatorial general-election campaigns." — 
- p6 Highlight (yellow): "Paraphrase: analysis at the media-market level, the lowest level at which ad data are available." — 
- p6 Highlight (yellow): "Paraphrase: definition of the treatment history as binary weekly values in {0,1}^T." — 
- p7 Highlight (yellow): "Paraphrase: contribution is showing how MSM methods apply when time-constant unmeasured confounding is suspected." — 
- p8 Highlight (yellow): "Paraphrase: groups may target markets with more persuadable voters by a metric unknown to the researcher." — 
- p8 Highlight (green): "Paraphrase: the sequential ignorability limitation motivates the fixed-effects approach." — 
- p8 Highlight (yellow): "Paraphrase: a marginal structural model is used to limit the number of treatment histories to estimate." — 
- p9 Highlight (yellow): "Paraphrase: estimated propensity scores come from a parametric model fit by MLE." — 
- p9 Highlight (yellow): "Paraphrase: h(.) is a researcher-specified vector of fixed functions of the treatment history." — 
- p9 Highlight (green): "Paraphrase: the estimator simplifies to standard techniques in many cases." — 
- p10 Highlight (yellow): "Paraphrase: Robins (1998) procedure gives a consistent, asymptotically normal MSM estimator." — 
- p10 Highlight (yellow): "Paraphrase: IPTW weights can be unstable and give highly variable estimates." — 
- p10 Highlight (yellow): "Paraphrase: stabilised weights use the marginal treatment probability as numerator." — 
- p10 Highlight (yellow): "Paraphrase: trimming the weights guards against unstable causal estimates." — 
- p10 Highlight (yellow): "Paraphrase: truncated treatment history with fixed k lags and a parameter vector of fixed length J." — 
- p10 Highlight (yellow): "Paraphrase: a bias source from the incidental parameters problem." — 
- p11 Highlight (yellow): "Paraphrase: label for the no-anticipation assumption." — 
- p11 Highlight (yellow): "Paraphrase: Assumption 1: treatment is independent of future potential outcomes and covariates given the unit-specific effect." — 
- p12 Highlight (yellow): "Paraphrase: the assumption allows time-varying measured confounding and time-constant measured and unmeasured confounding." — 
- p13 Highlight (yellow): "Paraphrase: large-N, large-T asymptotic framework used for nonlinear panel models." — 
- p13 Highlight (green): "Paraphrase: strong mixing lets the authors use laws of large numbers and central limit theorems over time." — 
- p13 Highlight (yellow): "Paraphrase: weak dependence over time, so information becomes uncorrelated as time distance grows." — 
- p13 Highlight (yellow): "Paraphrase: new information arrives as time goes on within units." — 
- p14 Highlight (yellow): "Paraphrase: propensity parameters are the solution to a population conditional maximum-likelihood condition." — 
- p14 Highlight (yellow): "Paraphrase: theory requires a correctly specified parametric covariate model in the propensity score." — 
- p15 Highlight (yellow): "Paraphrase: a fixed-length parameter vector is assumed; growing it with N and T is left to future work." — 
- p16 Highlight (yellow): "Paraphrase: the setting where N and T grow at the same rate." — 
- p17 Highlight (yellow): "Paraphrase: weighted least squares with an identity link and continuous outcomes is the straightforward estimator." — 
- p18 Highlight (yellow): "Paraphrase: the weighting-model parameters matter only insofar as they give consistent weights." — 
- p19 Highlight (green): "Paraphrase: future work: analytic or jackknife corrections for second-order terms (Hahn and Newey, 2004)." — 
- p19 Highlight (yellow): "Paraphrase: a doubly robust outcome model needs unobserved heterogeneity and repeated outcomes, which this setting lacks." — 
- p19 Highlight (yellow): "Paraphrase: propensity fixed effects are not identified for units that are always or never treated." — 
- p19 Highlight (yellow): "Paraphrase: in-sample positivity violations mean some units get undefined weights." — 
- p19 Highlight (green): "Paraphrase: option one: drop no-treatment-variance units and estimate on the rest." — 
- p19 Highlight (green): "Paraphrase: option two: bound the unit-specific effects and set them to the bounds for always or never treated units." — 
- p20 Highlight (yellow): "Paraphrase: alternative: change the quantity of interest to incremental propensity-score interventions (Kennedy, 2019)." — 
- p20 Highlight (yellow): "Paraphrase: simulation design mirrors the asymptotic approach; rho = N/T implies few time points at large rho." — 
- p21 Highlight (yellow): "Paraphrase: covariates are exogenous multivariate normal draws with a fixed correlation structure." — 
- p22 Highlight (yellow): "Paraphrase: simulation estimands are the final-period effect and the cumulative lagged effect." — 
- p23 Highlight (yellow): "Paraphrase: the simulations focus on the correctly specified MSM." — 
- p23 Highlight (yellow): "Paraphrase: the design isolates the effect of unmeasured heterogeneity on estimator performance." — 
- p23 Highlight (green): "Paraphrase: bias, standard errors and coverage come from 500 Monte Carlo simulations." — 
- p24 Highlight (yellow): "Paraphrase: performance claim that the method works well even at n=200 and T=4." — 
- p24 Highlight (yellow): "Paraphrase: IPTW without fixed effects may do better in finite samples when heterogeneity is very low." — 
- p24 Highlight (yellow): "Paraphrase: the fixed-effects estimator trades efficiency for lower bias versus naive IPTW." — 
- p25 Highlight (green): "Paraphrase: the proposed approach can outperform the naive approach despite its issues." — 
- p25 Highlight (yellow): "Paraphrase: campaign windows run from primary to general election, 8 to 40 weeks (median 20)." — 
- p27 Highlight (yellow): "Paraphrase: the fixed-effects specification adds a market-race fixed effect; IPTW includes only race-level fixed effects." — 
- p28 Highlight (green): "Paraphrase: all methods agree Democratic IG ads positively affect Democratic electoral performance." — 
- p28 Highlight (green): "Paraphrase: the effect runs through higher Democratic turnout and lower Republican turnout." — 
- p28 Highlight (yellow): "Paraphrase: Democratic groups' ads may mobilise Democrats and perhaps demobilise Republicans." — 
- p28 Highlight (yellow): "Paraphrase: Republican IG ads have no estimated effect on any measure and seem not very effective." — 
- p30 Highlight (yellow): "Paraphrase: the Democratic-group effect runs counter to the conventional wisdom on which party Citizens United would help." — 
- p30 Highlight (yellow): "Paraphrase: Democratic ads more effective at raising Democratic votes and cutting Republican votes, all significant at alpha=0.1." — 
- p31 Highlight (yellow): "Paraphrase: the positive Democratic-ad effect is stronger in the final weeks than earlier." — 
- p31 Highlight (yellow): "Paraphrase: key differences between the authors' approach and Sides et al." — 
- p32 Highlight (yellow): "Paraphrase: example of confounding if ads target markets with many opponent negative ads." — 
- p32 Highlight (yellow): "Paraphrase: the magnitudes are within the broad range of values in the literature." — 
- p32 Highlight (green): "Paraphrase: residual unmeasured confounding may remain that the approach misses." — 
- p33 Highlight (yellow): "Paraphrase: both IPTW approaches give placebo estimates near zero, consistent with the identifying assumptions." — 
- p34 Highlight (pink): "Paraphrase: conclusion: a fixed-effects propensity-score method controls for time-constant unmeasured confounding in MSMs." — 
- p34 Highlight (pink): "Paraphrase: asymptotic setup with the number of time periods and units growing together." — 
- p34 Highlight (pink): "Paraphrase: simulations show the proposed method beats the naive no-fixed-effects approach and performs well overall." — 

## Study

**Question:** Do independent interest-group TV ads affect Senate and gubernatorial vote shares and turnout after Citizens United, and can an MSM estimator be adjusted for time-constant unmeasured confounding (unit-level heterogeneity) when only a single outcome is observed?

**Theory:** Time-varying treatment with feedback: campaigns and groups respond to each other's ad decisions, so treatment and covariates co-evolve, causing time-varying confounding. Sequential ignorability is a poor fit when groups target markets with persuadable voters by a metric the researcher does not observe (p.3, p.8). The paper proposes a unit-specific sequential ignorability (Assumption 1, p.11): conditional on an unmeasured time-constant alpha_i, treatment is independent of future potential outcomes and covariates. The MSM is a g-computation-style marginal model for treatment histories (pp.8-9, 15).

**Data:** Unit: media market by race (market-race), weekly panel over the campaign window from primary to general election (8 to 40 weeks, median 20; p.25). Races: US Senate and gubernatorial general elections, 2010-2020 (pp.6, 25). Sample after dropping no-treatment-variation races: N = 467 market-races for Democratic IG ads and N = 623 for Republican IG ads (p.25). Sources: Wesleyan Media Project ad data covering all broadcast stations (Fowler et al., 2019); CQ Voting and Elections Collection outcomes; CVAP from US Census; RealClearPolitics polls; county-to-media-market mapping from Sides et al. (2022) (p.6). Simulations (Section 5, p.21): n in {200, 500, 1000, 3000} units, rho = n/T in {5, 10, 50}, 500 Monte Carlo replications (p.23).

**Method:** Proposed IPTW-FE: a two-step procedure. Step 1 fits a parametric logit propensity model with unit-specific fixed effects alpha_i by conditional MLE (incidental-parameters setting). Step 2 builds stabilized inverse-probability weights over the last k+1 periods and solves the weighted MSM estimating equation (weighted least squares for the identity link) (pp.15-17). Theorem 1 gives sqrt(N)-consistency and asymptotic normality with a sandwich variance, with N and T growing at the same rate (p.17); appendix proofs in Appendix A (pp.41-53). Comparators: IPTW with no fixed effects (race-level FE only), and a naive approach that ignores time-varying covariates. Empirical MSM (p.26): E[Y_i(d) | R_i] = gamma_R + gamma_1 * sum_{k=0}^{4} d_{T-k}, race-specific intercepts, effect of one extra week of IG ads in the last five weeks. Weighting model covariates (p.27): lagged Dem poll share and square, undecided/third-party share, Republican negativity over six weeks, cumulative ads by each party and squares, market-race FE. Simulations report bias, SE and 90% CI coverage (pp.22-24).

**Identification:** Unit-specific sequential ignorability (Assumption 1, p.11): conditional on the past treatment history, the covariate history and an unmeasured time-constant alpha_i, treatment is independent of future potential outcomes and future covariates. This allows time-constant measured and unmeasured confounding plus time-varying measured confounding, but rules out time-varying unmeasured confounding (p.12). Positivity is needed. Within-unit treatment switching is required (p.1). A no-anticipation assumption is built into the covariate process (p.11). Truncated MSMs (fixed k lags) are used so the number of propensity parameters, and hence incidental-parameter bias, stays limited (pp.10-11). Identification is not from a pre-period or a difference-in-differences design, because there is one endpoint outcome per unit (p.5, p.4).

**Scope:** Population: US Senate and gubernatorial general-election market-races, 2010-2020, using broadcast TV ads by independent interest groups (ads by candidates and parties are excluded; p.6). Unit of analysis is the media market (not county or state). Era: pre- and post-Citizens United (2010 decision), with a split at Trump's candidacy. Methodological scope: truncated MSMs with a fixed-length treatment history and a fixed-length parameter vector, with N and T growing at the same rate (pp.10, 15). Empirical findings are about down-ballot races and are not generalised to presidential races.

## Key findings

- Democratic IG ads raise the Democratic share of the two-party vote; the effect is driven by a positive effect on Democratic turnout and a weaker negative effect on Republican turnout (per the text; figure values not extractable). (Section 6.2, p.28 (Figure 3); Introduction p.4; Abstract p.1). Coefficients appear only in Figure 3; no point estimates in the text. Era differences significant at alpha=0.1 per p.30; no level stated for the pooled effects.
- Republican IG ads have no estimated effect on any measured outcome. (Section 6.2, p.28 (Figure 3)). No estimate in text; described as null.
- The effectiveness of Democratic IG ads is concentrated in the post-Trump era (after 2016 per Abstract and p.5; text on p.30 says 2015). Democratic ads more effective at increasing Democratic votes and reducing Republican votes; differences significant at alpha = 0.1. (Section 6.2, p.30 (Figure 4, pp.29-30); Abstract p.1; p.5). Significance at alpha=0.1; no point estimates in text. Years inconsistent: 2015 (p.30) vs 2016 (p.1, p.5, Figure 4 caption p.29).
- Democratic IG ads in the last three weeks show a somewhat stronger positive effect than those 4-5 weeks out; the text calls this consistent with prior experiments. (Section 6.2, p.31 (Figure 5)). Figure 5 only; no numeric values in text.
- Magnitudes compared with the literature: Sides et al. (2022) find 0.38-0.87 pp Democratic vote share per 1,000 Democratic-minus-Republican ads; Hewitt et al. (2024) find 1.2-2.3 pp for internal experiments; Klumpp et al. (2016) find 4.1-6.4 pp on House win probability from Citizens United. The authors say their magnitudes are within the broad range and their estimates are slightly higher than Sides et al. (Section 6.3, pp.31-32). Sides et al.: 0.38-0.87 pp per 1,000 ads; Hewitt et al.: 1.2-2.3 pp; Klumpp et al.: 4.1-6.4 pp House win probability.
- Placebo tests: IPTW and IPTW-FE estimates on lagged (previous-election) outcomes and pre-election Democratic poll share are close to zero; the naive approach shows residual confounding for some Democratic-ad effects. (Section 6.4, p.33 (Figure 6)). Figure 6 only; no numeric values in text.
- Balance: both IPTW and IPTW-FE sharply reduce conditional imbalance in measured time-varying covariates relative to naive; unweighted data have a few extremely unbalanced time-varying confounders. (Section 6.1, p.28 (Figure 2)). Standardized absolute coefficients in Figure 2 only.
- Positivity violations are common in the last five weeks: Table 1 counts weeks of Democratic IG ads for the final five weeks by year and by number of weeks; the paper's motivation for MSMs rests on this. (Section 6.1, p.26 (Table 1)). Table 1 reports counts by year (2010-2020); not summarised in text.
- Simulation, low heterogeneity (alpha range [-1,1], FE explain ~1/3 of treatment variance): IPTW-FE bias indistinguishable from IPTW-True; nominal 90% coverage across n and rho; works at n=200, T=4. Naive IPTW has higher bias but smaller SEs. (Section 5.2, pp.23-24 (Figure 1)). n in {200,500,1000,3000}; rho in {5,10,50}; 500 MC replications; true effects tau_F=1, tau_C=0.3.
- Simulation, high heterogeneity (alpha range [-2,2], FE explain ~2/3 of variance): IPTW-FE bias is larger than IPTW-True but shrinks with n and T; naive IPTW bias stays constant as n and T grow; the proposed method still beats naive. (Section 5.2, p.24 (Figure 1); Conclusion p.34). Figure 1 only; qualitative.
- Propensity-score fixed effects are not identified for always- or never-treated units; IPTW-FE can drop them, impute bounded alpha, or use imputed propensity scores (0.01/0.99). Including them with imputed FE (Supplemental D) gives similar results, but naive IPTW is no longer significant for Democratic ads. (Section 4.3, pp.19-20; Supplemental D, p.68 (Figure 10)). Imputed propensity scores 0.01 and 0.99 (p.19); no-variation races dropped in main results.
- Robustness to propensity-score link misspecification (probit fit to a logit truth) is good: bias and coverage close to correct specification, with slightly higher SEs. (Supplemental C.2, pp.65-66 (Figure 9)). Figure 9 only.
- Adding a linear election-year trend interacted with covariates in the weighting model leaves main results similar, with slightly wider uncertainty. (Supplemental D, p.68 (Figure 11)). Figure 11 only.

## Limitations

- Assumption 1 is violated by time-varying unmeasured confounding, a limitation the paper shares with linear fixed effects and difference-in-differences (p.12).
- With one endpoint outcome per unit, a doubly robust estimator with outcome-regression models is not feasible without strong assumptions (p.19).
- Truncated MSMs rule out cumulative full-treatment-history terms; long-term effects cannot be estimated while adjusting for unmeasured confounding (p.11).
- Consistency requires a correctly specified parametric propensity model for covariates, plus fixed-length parameter vectors (pp.14-15). The model-specific results are asymptotic.
- Finite-sample second-order bias scales with propensity extremity and short T; strong heterogeneity is worse. A jackknife correction is left to future work (pp.18-19).
- Positivity: fixed effects are not identified for units never or always treated; the three remedies (omit, impute, bound alpha) each trade bias for variance (pp.19-20).
- Residual unmeasured confounding remains a worry the authors acknowledge (p.32), though placebo tests are null (p.33).
- The paper assumes no interference across media markets (p.6) and treats ads as weeks of presence, not ad counts; the comparison with Sides et al. is therefore indirect (p.32).
- Only broadcast television ads are measured; the approach uses the market-race as the unit, and county outcomes are mapped to markets (p.6).
- Simulation evidence uses 500 MC replications and only the correctly specified MSM in the main text; the proposed estimator is slightly biased under high heterogeneity even with the true propensity score (p.24).
- Internal inconsistency: the Trump-candidacy split is dated 2015 on p.30 but 2016 in the Abstract (p.1), p.5 and the Figure 4 caption (p.29).
- Metadata inconsistency to note: the text does not state a venue; the title and authors match the hint.

## Short quotes

- "interest group ads are only effective when run by groups supporting Democratic candidates" (Abstract, p.1 (paraphrase-length quote; one only per copyright rule))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions|Sector growth vs giving, partisan share and lobbying regressions]] | qualifies | Unit fixed-effects regressions are biased when treatment and time-varying covariates feed back on each other (citing Imai & Kim 2019 and Sobel 2012). The paper's remedy moves the fixed effects into a propensity model and requires a binary treatment that switches within units, a correctly specified parametric propensity model, and N and T growing together. Simulations use n of 200-3000 with T of at least 4; in the empirical panel, campaign windows run 8-40 weeks. (pp.3, 8, 10-15 (Assumption 1, Theorem 1); Section 5 pp.21-24) | OLS of sector-year spending DVs (contributions_real_m; LobbyView lobbying for the L*-LV tests) on a growth term with sector FE plus controls (pres_year, midterm_year, exp_dem_house, exp_dem_senate), with year-FE variants (variant year_fe) and first-differences-with-year-effects (FDY) checks; inference is a wild cluster restricted bootstrap by sector, DESCRIPTIVE when only 2 sectors. Reference values in the Codex review file sgs_review.R: L3-LV coefficient +0.352 (n=77) becomes -0.477 with year FE, and the FDY coefficient is -0.537 over 71 adjacent pairs (window and sample as set in that test's spec). `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R:22,33,222-235; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:1-4,385-398; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_review.R:33-35` | Only the diagnosis transfers, not the estimator. The study's regressor is continuous sector growth with few sector clusters and annual T, so IPTW-FE cannot run: no binary within-unit-switching treatment and no feasible propensity model. Read the paper as support for not interpreting the L-LV coefficients causally. A sign flip under added year FE is consistent with feedback or time-varying confounding, so neither sign is causal. Applies to any sector-year panel where lobbying or giving is the outcome of a growth regressor that may itself respond to past spending. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-validation#Prospective receptivity contract (H1-H2) and forecast ledger|Prospective receptivity contract (H1-H2) and forecast ledger]] | suggests-extension | Placebo tests check the identifying assumptions by applying the estimator to outcomes measured before treatment: the previous election's vote share and turnout, and the pre-election poll share. IPTW-FE estimates are near zero, while the naive approach shows residual confounding for some Democratic-ad effects. (Section 6.4, p.33 (Figure 6); p.5) | H1 is a Spearman correlation between the frozen seed_prob and new cosponsorships after the freeze. H2 is a Spearman correlation between pressure_mult and absolute tier movement. Both are cross-sectional, with no pre-freeze placebo. The contract requires at least 60 days and 150 movers, and returns UNDERPOWERED without consuming the look. `fec/engine/outcome_contract.py:17-20,262-263,277-278` | The placebo logic carries over directly. A pre-registered negative control would be Spearman(seed_prob, cosponsorship delta over a window that ends before the freeze). A non-zero value would show that seed_prob tracks a stable member trait (a time-constant confounder) rather than responsiveness. Adding it changes the contract, so it needs Paul's ruling before the single look is spent, and it needs a second pre-freeze snapshot pair. Neither H1 nor H2 measures a lobbying treatment, so even a PASS remains an association. | moderate | VERIFIED |
| [[systems/terrier-canvass#Untapped potential|Untapped potential]] | qualifies | Democratic interest-group TV ads raise the Democratic two-party share mainly by raising Democratic turnout, with a weaker negative effect on Republican turnout. Republican interest-group ads have no estimated effect on any outcome. The Democratic effect is concentrated after Trump's candidacy; the pre/post-Trump effect differences are significant at alpha 0.1 (no level is stated in the text for the pooled effects). No point estimates are given in the text. (Section 6.2, pp.28-30 (Figures 3-4); Abstract p.1) | potential = (wp x (1-\|2s-1\|) x t + (1-wp) x s x (1-t)) x (1 + wi x i). The weights on persuasion of undecided likely voters and on mobilization of supporters unlikely to vote are symmetric across parties, with module default wp 0.5 used only when the district entry names none (DEFAULT_WEIGHTS, untapped.py:53,71-73); no party term enters the formula. `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/untapped.py:9-10` | Weak. The treatment is broadcast TV ads aggregated to the media market in Senate and gubernatorial races, 2010-2020. It is not door-to-door contact, and the outcome is not individual. The finding suggests that mobilization effects can dominate and can differ by party and era, so a fixed, party-symmetric wp is a judgment call that this paper neither validates nor sets. It does not transfer to US House districts (OR-05, CT-01) or to canvassing without field-experiment evidence. | weak | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Competitiveness multiplier c (COMPET_MULT)|Competitiveness multiplier c (COMPET_MULT)]] | qualifies | Groups may target markets with more persuadable voters using a metric the researcher cannot see, so a naive comparison of treated and untreated units is confounded. The paper handles only time-constant unmeasured confounding, through unit fixed effects in the propensity score. Time-varying unmeasured confounding still breaks identification. (pp.8, 12, 32) | c = 1 + 1.0 x exp(-max(lean,0)/5.0) scales effective dollars on both money legs. AMPLITUDE and SCALE are Paul's judgment constants, uncalibrated, and the reciprocity they encode is explicitly NOT validated. `fec/moneyball_core.py:74-93` | No direct transfer. The paper estimates effects of ads on voters, not the responsiveness of legislators to PAC dollars, and it must not be cited as support for c (D-104..D-108). Its only bearing is on calibration: any future attempt to estimate c from observed giving and alignment must handle strategic targeting of competitive seats, because givers pick those seats on information the analyst lacks. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: 'Effects are significant only at alpha 0.1' overstates the text: alpha 0.1 is stated only for the pre/post-Trump era DIFFERENCES (p.30, line ~1209). For the main Figure 3 effects the text says only that methods 'generally agree on the direction and significance', with no level given.
> - Summary issue: Bearing 1 system description (in rel, echoed by 'fixed-effects regressions of sector growth on spending') is reversed in direction: spending is the DV and growth the regressor; also log DVs, tossup/trend controls and a lagged/AR lobbying term were not found in the cited code, and the sign-flip values come from sgs_review.R reference values, not a brain record.
> - Summary issue: Gap item on Mentat's transfer graph (89% oppose IE split) was not verified here; treat as UNVERIFIED.

## Gaps and flags

- No component estimates a causal effect of money, ads or lobbying. Every money-response constant in the influence engine (WM/WP, HALFF, MAXSHIFT, COMPET_MULT amplitude and scale) is judgment. The paper shows the kind of design (weekly treatment panel, propensity weighting with unit fixed effects, placebo outcomes) that estimating them would need.
- The prospective receptivity contract (H1/H2) has no negative-control or placebo look on pre-freeze outcomes, so it cannot tell responsiveness apart from a stable member trait.
- The sector-growth regressions have no design that addresses feedback between lagged lobbying or giving and later covariates. The sign flips under year FE are reported but cannot be interpreted causally.
- Neither Mentat nor Melange measures voter-facing outside spending as a treatment with timing. Mentat's transfer graph splits independent-expenditure dollars into support and oppose but carries no week-level timing or outcome link.
- Terrier's persuasion/mobilization weight wp is symmetric and fixed. No evidence base, including this paper, sets it by party or era.
- Voter-level evidence only. It must not be quoted as support for the competitiveness multiplier's reciprocity or for any legislator money response; reciprocity is NOT validated (D-104..D-108).
- Pooled versus stratified: the Democratic-ad effect is concentrated after Trump's candidacy. Quote the pooled Democratic effect only with the era strata, or label it UNSTRATIFIED. Window: Senate and gubernatorial races 2010-2020; denominators N = 467 (Democratic-group ads) and 623 (Republican-group ads) market-races.
- Internal inconsistency: the Trump-era split is dated 2015 on p.30 but 2016 in the Abstract, on p.5 and in the Figure 4 caption.
- The text states alpha 0.1 only for the pre/post-Trump era differences (p.30) and gives no point estimates (figures only). Do not invent magnitudes.
- The paper does not argue for either rejected SNA proposal, and it does not use LDA.

Paul records no takeaways. Both bibliography entries (folder Causal & Methodological, and a5b9f5a95891) are bare citations, which fits his component note that the Causal & Methodological folder is unannotated. His 90 highlights fall almost entirely on method: fixed effects inside the propensity score, the single-outcome setting, the need for treatment switching, the positivity remedies (drop, bound or impute alpha), the simulation design, and three pink conclusion highlights. That suggests he values the paper as an estimation template, not for its findings about ads. The bearings above agree with that reading. The paper's contribution is the confounding diagnosis and the placebo practice, both of which the system's validation discipline could absorb. No note of his is stale relative to the system, because none links this paper to a component.

## Related papers

- [[imai-2021-fixed-effects-regression]] — complements — Both papers question unit and two-way fixed-effects designs under feedback. Blackwell & Yamauchi cite the 2019 Imai & Kim paper on the same problem. Together they underwrite reading the sector-growth year-FE sign flip as non-causal.
- [[weimer-2023-causal-approach-detecting]] — complements — Weimer uses Monte Carlo as a negative control. Blackwell & Yamauchi use placebo outcomes from before treatment. Both are falsification checks that the prospective contract lacks.
- [[broockman-2026-should-moving-middle]] — complements — Both papers bear on campaign persuasion and its returns, and Broockman co-authored the Hewitt et al. (2024) experiments used here as a magnitude benchmark (1.2-2.3 pp). Together they inform Terrier's split between persuasion and mobilization.
- [[kalla-nd-campaign-contributions-facilitate]] — complements — Kalla's experiment identifies the effect of money on legislators' access. This paper identifies the effect of outside spending on voters. The flip model's money assumptions need evidence of the first kind, which this paper does not supply.

### Linked from
- [[irvin-2023-dark-investigation-dark]] — complements — Blackwell item is about estimated effects of political advertising after Citizens United. Real link; the framing fits, though Blackwell does not measure C4 opacity.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — Estimates effects of post-Citizens United independent-group TV ads on Senate and governor vote shares and turnout, with a confounding adjustment. It is the kind of causal evidence the paper's decisive-influence claims lack. Item summary checked.