---
type: paper
title: "On the Use of Two-Way Fixed Effects Regression Models for Causal Inference with Panel Data"
authors: ["Kosuke Imai", "In Song Kim"]
year: "2021"
venue: "Political Analysis 29(3):405-415 (running header reads \"Political Analysis (2020)\"; published online 2020)"
doi: "10.1017/pan.2020.33"
category: "Causal & Methodological"
systems: ["mentat-sector-growth-study"]
components: ["mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.growth-spending-regressions"]
paul_annotated: true
paul_markup_count: 18
evidence_status: "verified"
source: "[[_sources/imai-2021-fixed-effects-regression.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Causal & Methodological\\FEmatch-twoway.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Causal & Methodological\\FEmatch-twoway.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Causal & Methodological\\FEmatch-twoway.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Causal & Methodological\\FEmatch-twoway.pdf"]
sha12: ["619251216983"]
tags: [paper, causal-methodological]
---

# On the Use of Two-Way Fixed Effects Regression Models for Causal Inference with Panel Data

**Kosuke Imai, In Song Kim (2021).** *Political Analysis 29(3):405-415 (running header reads "Political Analysis (2020)"; published online 2020).* [10.1017/pan.2020.33](https://doi.org/10.1017/pan.2020.33) Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> Imai and Kim (Political Analysis 29(3), 2021) ask whether the standard linear two-way fixed effects regression (unit plus time dummies) can be read as a design-based, nonparametric causal estimator. Their answer is no. Proposition 1 (PDF p.4) rewrites the 2FE estimator as a matching estimator that builds each counterfactual from three averages: the same unit, the same period, and an adjustment set of size |M_it| x |N_it|. Because that adjustment set includes observations with the same treatment status ("mismatches"), the estimator cannot adjust nonparametrically for unit and time confounders at the same time. Mismatches tend to cause attenuation bias. In the toy example for the adjusted matching estimator of Proposition 2 (Figure 1 panel b), 2 mismatches in an adjustment set of 4 give K = 3/2 (PDF p.7, article p.6). The multi-period difference-in-differences estimator, where treatment can switch on and off, equals a weighted 2FE estimator in which some control observations get negative weights (Theorem 1, PDF p.9). The unweighted 2FE estimator is not in general equivalent to multi-period DiD; the two-group, two-period case is the common justification. So the causal reading of 2FE depends on the linear additive model, not on the design. The paper is purely analytical: no data, no simulation, binary treatment only. Its software is the R package wfe.
> 
> For Paul's systems, this paper bears on one place: the Mentat sector-growth study. That study fits sector-baseline OLS and treats sector+year FE, log-FE and first-difference-with-year-effects variants as robustness checks. On the L-LV lobbying tests the growth slope flips sign under year FE (L3-LV +0.352 to -0.477, the Codex review reference values in sgs_review.R:33-35, also recorded in the sector-growth lane memory file). The paper explains why this is expected. One-way and two-way FE weight sector-years differently, and the 2FE coefficient is a model-dependent weighted contrast, not a DiD estimate. Sign agreement across those variants is therefore not agreement on a single causal estimand. The DECEL dummy switches on and off within each sector, the binary reversible shape the paper studies, but it is fit with sector baselines only (sgs_registry.R:358-364), not sector + year FE, so Theorem 1's 2FE/DiD result does not apply to it as written.

## Paul's annotation

From [[annotations/Causal & Methodological - Causal & Methodological - Annotated Bibliography|Causal & Methodological - Causal & Methodological - Annotated Bibliography]]:

> Imai, Kosuke, and In Song Kim. “On the use of two-way fixed effects regression models for causal inference with panel data.” *Political Analysis*, vol. 29, no. 3, 12 Nov. 2020, pp. 405–415, [https://doi.org/10.1017/pan.2020.33](https://doi.org/10.1017/pan.2020.33).

## Paul's markup in the PDF

- p2 Highlight: "default method for estimating causal effect" — 
- p2 Highlight: "from panel dat" — 
- p2 Highlight: "critically relies upon the assumption of linear additive effects." — 
- p2 Highlight: "We show that this equivalence does not hold under more general settings commonlyencounteredinappliedresearch" — 
- p2 Highlight: "emulti-perioddifference-in-differences estimatorisequivalenttotheweighted2FEestimatorwithsomeobservationshavingnegativeweight" — 
- p2 Highlight: "Instead, its validity fundamentally rests on the modeling assumptions." — 
- p2 Highlight: "Applied researchers ofen use the 2FE regression to adjust for unobserved unit-specific and time-specific confounders at the same time." — 
- p2 Highlight: "2FE estimator in order to understand the causal interpretation of these widely used estimators within the nonparametric framework" — 
- p3 Highlight: "our analysis extends to a more general case," — 
- p3 Highlight: "units can go in and out of the treatment condition at different points in time" — 
- p3 Highlight: "The least squares estimate of β can be computed efficiently by transforming the outcome and treatment variables and then regressing the former on the latter." — 
- p3 Highlight: "We show below that such a justification is unwarranted without critically relying on the functional-form assumption." — 
- p4 Highlight: "An intuitive explanation of this result is as follows." — 
- p4 Highlight: "Thus, the 2FE estimator critically relies upon the linearity assumption for its simultaneous adjustment for the two types of unobserved confounders." — 
- p4 Highlight: "t ′�t Yit ′/(T −1), , � i ′�i Yi ′t/(N −1), n-time matched set" — 
- p4 Highlight: "|Ait | = |Mit | · |Nit |" — 
- p5 Highlight: "Therefore, mismatches imply the (partial) comparison of observations with the same treatment status, which generally leads to an attenuation bias" — 
- p10 Highlight: "Given the problems of the standard two-way fixed effects regression estimator identified in this paper, future research should develop design-based estimators for causal inference with panel data." — 

## Study

**Question:** Can the linear two-way fixed effects (2FE) regression with unit and time fixed effects be justified as a design-based, nonparametric causal estimator for panel data, both (a) as an adjustment for unit-specific and time-specific unobserved confounders at once and (b) as equivalent to the difference-in-differences (DiD) estimator beyond the two-group, two-period case?

**Theory:** Matching representation of fixed effects estimators (extending Imai and Kim 2019 from unit FE to two-way FE) and the parallel trends assumption of the multi-period DiD design (Assumption 1, PDF p.7). The paper's goal is to clarify two common misunderstandings of 2FE, not to propose a new estimator (PDF p.3).

**Data:** None. This is an analytical paper with no empirical dataset, sample, time period or reported N. Setting: a panel of N units i=1..N and T periods t=1..T, binary treatment X_it, outcome Y_it, balanced for notational simplicity (unbalanced extension asserted as straightforward, PDF p.3). The only numbers are an illustrative treatment matrix with N=5 units and T=4 periods (Figure 1, PDF pp.4-5).

**Method:** Algebraic/proof-based analysis. Proposition 1 (PDF p.4): the 2FE estimator (eq. 2) equals a two-way matching estimator with adjustment factor K. Proposition 2 (PDF p.6): a matching estimator using only opposite-status matches equals a weighted 2FE estimator with weights K_it. Section 4: multi-period DiD (eq. 12, PDF p.8) defined via matched sets (eqs. 9-11, PDF p.8) and shown to equal a weighted 2FE estimator with weights W_it (Theorem 1, PDF p.9; proof Appendix A, PDF p.10). Proofs of Propositions 1-2 are in the Online Supplementary Information. No simulation or empirical application appears in the text.

**Identification:** No new identification strategy is proposed. The paper states that under the multi-period DiD design with parallel trends (Assumption 1, PDF p.7), the estimand is the ATT, tau = E(Y_it(1) - Y_it(0) | X_it=1, X_i,t-1=0) (eq. 8, PDF p.7). Footnote 2 (PDF p.9): if model (1) is correct, the 2FE estimator is consistent for tau under the multi-period DiD design (beta = tau). The paper argues that this validity rests on the linear model, not on design.

**Scope:** General panel data with a binary treatment in which units can switch into and out of treatment at different times (not only staggered adoption, PDF p.3). No population, institution or era is studied; the paper is methodological and makes no claims about a particular substantive field.

## Key findings

- The 2FE estimator can be written as a two-way matching estimator in which each counterfactual is built from the same-unit average, same-time average, minus a pooled adjustment-set average, with a factor K equal to the net proportion of proper (opposite-status) matches. (Proposition 1 and Section 3.1, PDF pp.3-4 (eqs. 3-4)). Adjustment set size |A_it| = |M_it| x |N_it| (eq. 4, PDF p.4)
- Simultaneous nonparametric adjustment for unit-specific and time-specific confounders is impossible under 2FE: mismatches (same-status observations) and an adjustment set containing observations sharing neither the unit nor the time period remain even after restricting to opposite-status matches (Proposition 2). (Section 3.2, PDF pp.5-6; Figure 1 caption PDF p.5). Illustrative example: a_it = 2 mismatches in adjustment set of size 4, so K_it = 1 + 2/4 = 3/2 (Proposition 2's adjusted matching, Figure 1 panel b, PDF p.7, article p.6)
- Under the multi-period DiD design, all three sets (within-unit matched, within-time matched, adjustment) contain only control observations, so mismatches are eliminated. (Section 4, Figure 2 and eqs. 9-11, PDF p.8). 
- The multi-period DiD estimator equals a weighted 2FE estimator; the unweighted 2FE estimator is not in general equivalent to it, contrary to common belief. (Theorem 1 and text after it, PDF p.9). 
- Some control observations receive negative regression weights, which occurs when (2X_it - 1)(2X_i't' - 1) = -1, i.e., when a control enters the adjustment set for multiple treated observations. (Theorem 1 weights and discussion, PDF p.9). Weight product equal to -1 (no empirical magnitude reported)
- The two-group, two-period setting is the common justification for equating 2FE with DiD; the paper says unweighted 2FE is not in general equivalent to multi-period DiD, and with more periods and units switching in and out the justification requires the linearity assumption. (Section 4.2 opening and discussion, PDF pp.8-9). Two periods, two groups (no numbers)
- Conclusion: 2FE is not a design-based, nonparametric causal estimator; its validity fundamentally rests on modeling assumptions. Future work should develop design-based panel estimators (e.g., synthetic control extensions). (Abstract PDF p.2; Section 5 PDF p.10). 

## Limitations

- Parallel trends (Assumption 1) may not be credible in some settings; the paper cites Bilinski and Hatfield 2018, Kahn-Lang and Lang 2019, and Rambachan and Roth 2019 on this point (PDF p.7).
- Only a binary treatment is analyzed; general treatment regimes are left for future work (PDF p.10).
- The panel is assumed balanced for notational simplicity; the unbalanced case is claimed to extend readily but is not worked out (PDF p.3).
- The DiD-2FE equivalence for beta = tau depends on model (1) being correct (footnote 2, PDF p.9).
- No simulation, empirical application or numerical check of the claims appears in the text; results are purely analytical.
- The paper does not propose a replacement estimator; the proposed matching/weighting alternatives are only referenced (Imai, Kim and Wang 2018, PDF p.10).

## Short quotes

- "does not represent a design-based, nonparametric estimation strategy for causal inference" (Introduction, PDF p.2 (journal p.1))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions|Sector growth vs giving, partisan share and lobbying regressions]] | qualifies | The 2FE estimator equals a matching estimator whose adjustment set has \|A_it\| = \|M_it\| x \|N_it\| and can contain same-status mismatches, so its simultaneous adjustment for unit and time confounders 'critically relies upon the linearity assumption'. Unweighted 2FE is not in general equivalent to the multi-period DiD estimator; the equivalence is the commonly cited justification from the basic two-group, two-period case. (Proposition 1 and Section 3.1 (eq. 3-4, \|A_it\| = \|M_it\| x \|N_it\|), PDF pp.3-4; Theorem 1 implications, PDF pp.9-10) | The study's base L-LV models use sector baselines plus controls; the robustness family adds R-yearFE (sector + year FE), R-logFE, R-FD and FDY (first differences + year effects, EXPLORATORY, descriptive). The L3-LV slope is +0.352 (n=77) in the original specification and -0.477 under sector + year FE (log-FE -0.006; FDY -0.537); these are the Codex review reference values the pipeline checks its run against (sgs_review.R:33-35), and the sign flip is recorded in the sector-growth lane memory file (not the project brain), whose lesson reads 'Never call a trend a finding before the year-FE and incremental-fit checks'. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_review.R:33-35,305-307,340-342; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_summaries.R:83-90; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:386` | Transfers in part. The paper covers a binary treatment only (its conclusion says so); the growth regressors here are continuous, so the mismatch and negative-weight algebra does not carry over directly. The general point does: a 2FE slope is a model-based weighted contrast whose causal reading depends on linear additivity, and one-way FE, two-way FE and first-difference fits target differently weighted contrasts. Extension to continuous treatments rests on the TWFE literature the paper cites, not on this paper. The panel is small (77 rows on the L3 base fit). The paper supports the study's existing caution (year FE as a check, no causal language); it does not make the year-FE coefficient the 'true' effect. | strong | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions|Sector growth vs giving, partisan share and lobbying regressions]] | suggests-extension | The multi-period DiD estimator, where units switch in and out of treatment, equals a weighted 2FE estimator with weights W_it; some control observations receive negative weights when (2X_it - 1)(2X_i't' - 1) = -1, so unweighted 2FE is not the DiD estimator. The wfe R package implements the weighted estimators. (Theorem 1 and its implications, PDF pp.9-10; Acknowledgments, PDF p.11) | DECEL is a within-sector binary indicator (1 when growth is below its prior-year value in the same sector), which switches on and off across years. The DECEL-<base> tests add g_decel and g x g_decel to the base specification, i.e. sector baselines (one-way unit FE) plus controls; no sector + year FE DECEL variant was found in the registry. The pipeline's 'temporal support' diagnostic reports each year's share of the absolute OLS weight on an estimate, not signed per-observation implicit weights. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R:358-364; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_panel.R:1007-1021; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:149-155; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_report.R:247; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_review.R:252-257` | DECEL has the paper's binary, reversible treatment shape, but it is fit with sector baselines only, not two-way FE, so Theorem 1's 2FE/DiD negative-weight result does not apply as written; the one-way unit-FE matching result is in Imai and Kim (2019), which this paper extends. It would apply directly only if a sector + year FE DECEL variant were added. DECEL is also built from the growth regressor itself, not an assigned treatment. Any signed implicit-weight diagnostic or wfe/PanelMatch comparison would be new analysis: UNVERIFIED until a second path reproduces it (DE-062), EXPLORATORY under the original-model contract. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The toy-example adjustment factor K = 3/2 (2 mismatches in an adjustment set of 4) belongs to Proposition 2's adjusted matching / weighted 2FE estimator (Figure 1 panel b), on PDF p.7 (article p.6), not to Proposition 1's standard 2FE at PDF pp.5-6 as the summary's flow implies.
> - Summary issue: 'The DECEL dummy ... is the paper's exact setting' overstates: DECEL is binary and reversible, but DECEL tests are fit with sector baselines only (sgs_registry.R:358-364), not sector + year FE, so Theorem 1's 2FE/DiD result does not apply as written; the one-way case is Imai and Kim 2019.
> - Summary issue: The L3-LV +0.352 to -0.477 figures are the Codex review reference values in sgs_review.R:33-35 and the sector-growth lane memory file; the bearing's 'the brain records' attribution is wrong.
> - Summary issue: 'No other component estimates a causal effect from a panel with unit and time fixed effects' is an unverified repo-wide existence claim; no census was shown. Mark UNVERIFIED.
> - Summary issue: 'The unweighted 2FE estimator equals DiD only in the 2-group, 2-period case' is stronger than the text: the paper says unweighted 2FE is not in general equivalent to multi-period DiD and cites the 2x2 case as the common justification.

## Gaps and flags

- The sector-growth study has no signed per-observation weight diagnostic for its FE regressions. Temporal support reports only absolute weight shares by year (sgs_review.R:252-257), so it cannot show negative-weight sector-years, which Theorem 1 says arise under 2FE with switching binary treatments. DECEL is fit with sector baselines only, so Theorem 1 would apply to it only if a sector + year FE variant were added.
- The study has no design-based panel comparison (matched multi-period DiD via wfe/PanelMatch, or synthetic-control-style estimators) for the binary DECEL/ASYM terms. Every variant in the robustness family (base, R-yearFE, R-logFE, R-FD, FDY) is a linear additive model, so they share the same modelling assumption the paper identifies.
- No parallel-trends check (pre-period placebo or event-study leads) is recorded for any sector-FE growth or DECEL test. The paper names parallel trends (Assumption 1) as the identifying condition for the DiD reading.
- Paul's 'trend over significance' directive (2026-09-26) treats sign consistency across variants as evidence. The paper implies that the one-way FE, two-way FE and first-difference variants target differently weighted contrasts, so a sign flip between them (L3-LV +0.352 vs -0.477) means different estimands rather than noise. Sign agreement across those specs should not be cited as a consistent trend in one causal effect.
- Year-effect variants are reported without strata (sgs_estimate.R:398 sets no_strata for year-effect fits other than the year_fe variant). Under the brain wording rule, any pooled FE coefficient quoted across the sector fault line must carry strata or the UNSTRATIFIED label.
- No conflict with the rejected SNA proposals, D-021, D-025 or D-104..D-108. The paper does not address networks, LDA edges or reciprocity.

Paul's highlights focus on the paper's central claims: 2FE 'critically relies upon the assumption of linear additive effects', its validity 'fundamentally rests on the modeling assumptions', and DiD equals weighted 2FE with negative weights. His positions table cites this paper for the sector-growth study ('Imai & Kim's 2FE caution bears on the design') and for single-look discipline. That reading matches the evidence. The sector-growth lane learned the same lesson the hard way: L3-LV was called 'supported' before the year-FE check, then flipped sign. His annotation record for this paper is empty (no takeaways, no system links), so his note is not stale; it simply was never written. One tension: his 'trend over significance' directive accepts sign stability as the bar, while this paper implies that sign stability across one-way FE, two-way FE and first-difference specs is not stability of one causal quantity. For single-look discipline the paper bears only indirectly: a pre-registered look on a 2FE spec tests a linear model, not a design. Nothing sharper holds there, so no bearing was assigned.

## Related papers

- [[blackwell-2026-effect-political-advertising]] — complements — Both address causal inference with panel data and time-varying treatments. Blackwell and Yamauchi's IPTW with unit fixed effects in the propensity model for marginal structural models (citing Imai and Kim 2019 on linear FE limits) is a weighting-based alternative of the kind Imai and Kim's conclusion calls for (PDF p.10).
- [[kim-2020-strategic-sequential-links]] — complements — Shares co-author In Song Kim. Kim, Stuckatz and Wolters fit linear probability models with firm, year, politician and firm-politician fixed effects and then a matched DiD (Imai, Kim and Wang), so the FE-panel caveat applies and the matched-DiD step is the design-based route this paper points to.

### Linked from
- [[weimer-2023-causal-approach-detecting]] — complements — Imai & Kim recast two-way fixed effects as a matching estimator and show its causal limits; Weimer uses matching on a natural experiment. Both bear on reading observational looks causally.
- [[bonica-2025-money-campaign-finance]] — complements — The donor-level models are two-way fixed-effects regressions. Imai and Kim's cautions about reading 2FE as causal apply. Not cited.
- [[fouirnaies-2022-electoral-incentives-affect]] — complements — Fouirnaies & Hall rely on individual-level two-way fixed-effects DiD with a staggered final-term treatment and admit they cannot run pretrend tests (p.669). Imai & Kim's critique of TWFE causal assumptions applies directly when judging how far the -0.24 SD estimate should transfer.
- [[huneeus-2018-effects-firms-lobbying]] — complements — Its two-way fixed-effects caution applies to H&K's OLS columns (firm, year and state-year FE) and to the system's sector-growth regressions.
- [[bickmore-2025-corporate-lobbying-political]] — complements — Imai is not cited in this paper. The paper's industry and year FE without firm FE makes the two-way FE caution topically relevant. Keep as analyst context.
- [[sverdrup-nd-introduction-grf-causal-forests]] — complements — Vault item exists. Rel uses it for observational panel identification assumptions; kept on existence, content not re-read here.
- [[callaway-2025-difference-differences-continuous]] — complements — Imai and Kim (Political Analysis 2021) critique two-way fixed effects with panel treatments; this paper gives the continuous-dose analogue of that caution. Item exists and is linked to the same sector-growth component.
- [[defigueiredo-2013-advancing-empirical-research]] — complements — Imai and Kim (2021) on two-way fixed effects for causal inference with panels. Formalizes the essay's warning about fixed effects on persistent panels.
- [[mackinnon-2017-wild-bootstrap-inference]] — complements — Checked: the item is Imai and Kim on two-way fixed-effects regression for causal inference with panel data, matching the identification side of the year-FE sensitivity. Kept.
- [[snowberg-2006-partisan-impacts-economy]] — complements — Imai and Kim show that two-way fixed-effects estimates rely on linearity for a causal reading, which bounds what the SHIFT and SEATS-W panel regressions can claim.
- [[note-request-for-review-industry-economic-performance]] — complements — Imai and Kim (2021) on two-way fixed effects for causal inference supports caution about the memo year FE plan. The text has no lagged-dependent-variable discussion, so the rel attribution about a lagged DV with few units is not carried over.
- [[note-industry-growth-and-political-spending-research]] — complements — Item record exists (Imai and Kim 2021, two-way fixed effects for causal inference with panel data); fits the plan's sector-FE and year-FE check. Title checked only.
- [[note-review-industry-growth-and-political-spending]] — complements — Imai and Kim (Political Analysis 2021) caution what two-way fixed effects identify for causal claims; the review makes sector and year FE its primary design, so the caution applies.