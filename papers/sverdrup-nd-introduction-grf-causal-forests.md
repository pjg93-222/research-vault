---
type: paper
title: "An introduction to grf"
authors: ["Erik Sverdrup", "Vitor Hadad", "Susan Athey", "Julie Tibshirani", "Stefan Wager"]
year: "2026"
venue: "R package grf vignette (no journal)"
doi: ""
category: "Causal & Methodological"
systems: ["melange-influence-engine", "melange-validation", "terrier-canvass"]
components: ["melange-influence-engine.persuadability-table", "melange-validation.prospective-contract", "terrier-canvass.untapped-potential"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/sverdrup-nd-introduction-grf-causal-forests.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\Codex\\2026-09-27\\we-are-going-to-have-an\\outputs\\cu-geography-v3\\_build\\grf-test-replication-20261002T072208Z\\research_package\\.Rlib\\grf\\doc\\grf_guide.pdf"]
sha12: ["c356deb40dc5"]
tags: [paper, causal-methodological]
---

# An introduction to grf

**Erik Sverdrup, Vitor Hadad, Susan Athey, Julie Tibshirani, Stefan Wager (2026).** *R package grf vignette (no journal).* Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> A 2026 tutorial vignette for the grf R package (Sverdrup, Hadad, Athey, Tibshirani, Wager), not a research paper. It walks through causal forests for conditional average treatment effects, AIPW doubly robust average effects, best linear projection (BLP) of effect heterogeneity on covariates, rank-weighted average treatment effect (RATE) via TOC/AUTOC curves to test whether a prioritization rule actually concentrates the treatment effect, Qini curves for budgeted targeting, and depth-limited policy trees learned from doubly robust scores. Two randomized examples: a Brazilian school financial-education RCT (about 17,000 students, 850 schools; ATE 4.35 points, SE 0.52; whole-sample AUTOC p = 0.65, but prioritizing low financial autonomy gives AUTOC 0.69, SE 0.22) and a payday experiment (about 2,500 people; held-out AUTOC 0.0061, SE 0.0092, no heterogeneity). The authors flag that out-of-bag TOC tests are heuristics without finite-sample guarantees and that policy trees need held-out evaluation. For Paul's systems the value is methodological. Every targeting score he ships (Terrier untapped potential, the Melange PERS persuadability table, receptivity seed_prob) is a hand-built or correlational ranking of who is most movable. grf supplies the standard test of whether such a ranking sorts people by treatment effect rather than by outcome level. None of it transfers without logged treatment assignment (randomized or known-propensity canvass contacts, outreach or money) and outcomes, which none of the systems currently record. Observational use assumes unconfoundedness. Any analysis built on this stays UNVERIFIED until a second path reproduces it.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Not a research paper: a tutorial showing how to use the grf R package for heterogeneous treatment effect (CATE) estimation, doubly robust ATE, best linear projection, TOC/AUTOC evaluation, Qini policy evaluation and interpretable policy learning.
**Theory.** Robinson (1988) partially linear model and residual-on-residual orthogonality; generalized random forests (Athey, Tibshirani, Wager 2019) using forest weights as adaptive neighborhoods; AIPW/doubly robust scores (Robins, Rotnitzky, Zhao 1994); orthogonal estimating equations and pseudo-outcomes; honesty and subsampling for pointwise asymptotics (Wager and Athey 2018). Section pp.1-6.
**Data.** Example 1: Bruhn et al. (2016) Brazil school-level randomized financial education program; about 17,000 students at 850 high schools; outcome post-treatment financial literacy score; 13 pre-treatment covariates (4-10 percent missing); clustering by school (pp.6-9). Example 2: Carvalho et al. (2016) payday experiment on low-income individuals tested before versus after payday; about 2,500 individuals and 24 pre-treatment covariates; outcome number of correct answers (variable outcome.correct.ans.per.second); 60/40 train/test split and top 10 covariates by variable importance (pp.10-12).
**Method.** causal_forest with W.hat and clusters; average_treatment_effect (AIPW); best_linear_projection with HC3 cluster-robust SEs; out-of-bag CATE predictions; rank_average_treatment_effect for TOC/AUTOC; variable_importance; double_robust_scores and policy_tree depth-1 for interpretable policies; regression_forest for nuisance Y.hat; maq sister package for Qini and budget policies (described, not run). Tutorial only, no new estimator.
**Identification.** Unconfoundedness given covariates X is assumed, described as a domain-knowledge choice (p.2). Both applied examples use randomized treatment with known propensity W.hat = 0.5 (pp.6-7, 10-11). Missing covariates are assumed missing at random and handled by MIA splitting (p.6).
**Scope.** Software vignette for the grf R package, 2026. Examples concern Brazilian high school students (RCT data from 2016 study) and low-income individuals in the Carvalho et al. payday sample. Generalization beyond these samples is not argued.

## Key findings

- Financial education program ATE = 4.35 points (SE 0.52), about 0.3 SD of outcome (p.7); the text calls this roughly a quarter SD.
- Best linear projection on financial.autonomy.index: coefficient -0.0329 (SE 0.0128, p = 0.0099); students with higher autonomy benefit less (p.7).
- Other BLP terms: is.female -0.7268 (SE 0.5307, p = 0.17); family.receives.cash.transfer -1.1992 (SE 0.6526, p = 0.066); is.unemployed 0.3206 (SE 0.6277, p = 0.61) (p.7).
- Whole-sample AUTOC using out-of-bag predictions: one-sided p = 0.65, so no reliable heterogeneity signal overall; authors call the evidence suggestive only (p.8).
- TOC prioritizing low financial autonomy: AUTOC estimate 0.69, SE 0.22, t about 3.2; bottom 20 percent by autonomy have ATE about 1.1 points above average with CI excluding zero (p.9).
- Depth-1 policy tree: assign training if financial.autonomy.index is 41 or below (p.10). Double robust mean scores: control 58, treated 62 (p.9).
- Poverty experiment: CATE TOC AUTOC estimate 0.0061 (SE 0.0092), t = 0.67; no evidence of heterogeneity in the held-out test sample (p.11).
- Poverty example histogram: most individuals have effects near the overall ATE with some variation (p.8).
- The original Carvalho et al. study found no statistically significant average effect; the tutorial cites Farbmacher et al. (2021) for subgroup concerns (p.10).

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
-  
- t
- h
- e
-  
- o
- u
- t
- -
- o
- f
- -
- b
- a
- g
-  
- T
- O
- C
-  
- t
- e
- s
- t
-  
- i
- s
-  
- a
-  
- p
- r
- a
- g
- m
- a
- t
- i
- c
-  
- h
- e
- u
- r
- i
- s
- t
- i
- c
-  
- w
- i
- t
- h
- o
- u
- t
-  
- f
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
- g
- u
- a
- r
- a
- n
- t
- e
- e
- s
- ,
-  
- b
- e
- c
- a
- u
- s
- e
-  
- t
- h
- e
-  
- s
- a
- m
- e
-  
- d
- a
- t
- a
-  
- b
- u
- i
- l
- d
-  
- a
- n
- d
-  
- e
- v
- a
- l
- u
- a
- t
- e
-  
- t
- h
- e
-  
- C
- A
- T
- E
-  
- (
- p
- .
- 8
- )
- .
-  
- P
- o
- l
- i
- c
- y
-  
- t
- r
- e
- e
-  
- e
- x
- a
- m
- p
- l
- e
-  
- i
- s
-  
- i
- n
- -
- s
- a
- m
- p
- l
- e
-  
- a
- n
- d
-  
- t
- h
- e
-  
- t
- e
- x
- t
-  
- s
- a
- y
- s
-  
- a
-  
- h
- e
- l
- d
- -
- o
- u
- t
-  
- t
- e
- s
- t
-  
- s
- e
- t
-  
- w
- o
- u
- l
- d
-  
- b
- e
-  
- n
- e
- e
- d
- e
- d
-  
- i
- n
-  
- p
- r
- a
- c
- t
- i
- c
- e
-  
- (
- p
- .
- 1
- 0
- )
- .
-  
- P
- o
- v
- e
- r
- t
- y
-  
- e
- x
- a
- m
- p
- l
- e
-  
- i
- s
-  
- m
- o
- d
- e
- r
- a
- t
- e
-  
- i
- n
-  
- s
- i
- z
- e
-  
- r
- e
- l
- a
- t
- i
- v
- e
-  
- t
- o
-  
- p
- r
- e
- d
- i
- c
- t
- o
- r
- s
-  
- a
- n
- d
-  
- p
- o
- s
- s
- i
- b
- l
- y
-  
- u
- n
- d
- e
- r
- p
- o
- w
- e
- r
- e
- d
-  
- (
- p
- .
- 1
- 0
- )
- .
-  
- M
- i
- s
- s
- i
- n
- g
-  
- d
- a
- t
- a
-  
- a
- s
- s
- u
- m
- e
- d
-  
- m
- i
- s
- s
- i
- n
- g
-  
- a
- t
-  
- r
- a
- n
- d
- o
- m
-  
- (
- p
- .
- 6
- )
- .
-  
- I
- d
- e
- n
- t
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
- r
- e
- s
- t
- s
-  
- o
- n
-  
- u
- n
- c
- o
- n
- f
- o
- u
- n
- d
- e
- d
- n
- e
- s
- s
- ,
-  
- a
-  
- m
- o
- d
- e
- l
- i
- n
- g
-  
- c
- h
- o
- i
- c
- e
-  
- (
- p
- .
- 2
- )
- .
-  
- C
- i
- t
- e
- d
-  
- W
- a
- g
- e
- r
-  
- (
- 2
- 0
- 2
- 4
- )
-  
- b
- o
- o
- k
-  
- i
- s
-  
- i
- n
-  
- p
- r
- e
- p
- a
- r
- a
- t
- i
- o
- n
- .

## Short quotes

- "Assumptions 2) and 3) are modeling choices that we may wish to relax."
- "Robinson showed that this estimator is root-n consistent even if m(x) and e(x) are estimated more slowly."
- "This should be interpreted as suggestive evidence rather than a formally validated inference procedure."

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/terrier-canvass#Untapped potential|Untapped potential]] | suggests-extension | RATE/TOC tests whether a ranking sorts units by treatment effect. In the Brazil RCT (n about 17,000 students, 850 schools), ranking by low financial autonomy gave AUTOC 0.69 (SE 0.22, t about 3.2). Ranking by out-of-bag forest CATE gave one-sided p = 0.65 (the text says this rejects, which the printed p-value does not). Qini curves for budgeted targeting and depth-1 policy trees from doubly robust scores are described (pp.7-10). (pp.7-10 (BLP, OOB AUTOC p=0.65, autonomy TOC 0.69, policy_tree split at 41); Qini in guided tour via maq (not run)) | Untapped potential (untapped.py docstring): undecided = (1 - \|2s-1\|) x t; supporter = s x (1-t); potential = (wp x undecided + (1-wp) x supporter) x (1 + wi x i), with s candidate support, t turnout, i sphere percentile. wp and wi come from the district settings (module default 0.5, 0.5). The score is a fixed formula with no effect-heterogeneity estimate and no RATE or Qini check. `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/untapped.py:8-9 (formula in module docstring; cited 9-10 in rel)` | Needs randomized or known-propensity knock vs no-knock assignment with logged outcomes (support ID or validated turnout), scored as RATE on held-out doors. Without randomized contacts, any test is an observational CATE that assumes unconfoundedness. Not transferable from this tutorial as evidence about the formula. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-validation#Prospective receptivity contract (H1-H2) and forecast ledger|Prospective receptivity contract (H1-H2) and forecast ledger]] | qualifies | Effect heterogeneity is tested by ranking on a score and computing the gain over the ATE in the top-ranked fraction (TOC/AUTOC), which requires treated and untreated units. The authors warn that out-of-bag TOC is a heuristic without finite-sample guarantees and that policy trees need a held-out set (pp.8-11). (pp.8-11 (OOB heuristic p.8; policy tree held-out note p.10; poverty example evaluated on held-out test sample p.11)) | H1 in the prospective-contract protocol: frozen seed_prob correlates positively with new cosponsorships adopted after the freeze (Spearman over members with panel coverage in both snapshots). This is an association with the outcome level, not a test of whether contact moves high-seed_prob members more. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/engine/outcome_contract.py:17 (PROTOCOL_TEXT H1 line; read-only)` | Effect-modification reading applies only if CU outreach or money exposure is logged per member with plausibly known assignment. Otherwise H1 is a predictive-validity check. The held-out, single-look design is consistent with the authors' advice, but this is a judgment, not something the paper tests. | moderate | VERIFIED |
| [[systems/melange-influence-engine#Persuadability table PERS|Persuadability table PERS]] | qualifies | Best linear projection tests whether effects vary along a chosen covariate: financial.autonomy.index coefficient -0.0329 (SE 0.0128, p = 0.0099). Other terms: is.female p = 0.17, family cash transfer p = 0.066, is.unemployed p = 0.61. Heterogeneity can be hard to detect at moderate n (payday AUTOC t = 0.67, about 2,500 individuals; text says possibly underpowered). (p.7 (BLP table); pp.10-11 (payday sample size and AUTOC t = 0.67)) | PERS in moneyball_core.py: fixed table over stance tiers (Highly Persuadable 0.65, Persuadable 0.60, Ally 0.40, Champion 0.10, Opponent 0.08, no-position default 0.45 via PERS_NONE). Not estimated against observed stance movement after contact. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/moneyball_core.py:564-568 (PERS table at 565; PERS_NONE at 564; read-only)` | A BLP of contact effect on stance-tier ordinals needs logged contact and tier movement; the payday example suggests a sample of 533 members may be too small for tier-level heterogeneity. A null would not refute the table. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary's whole-sample AUTOC p = 0.65 is the right number, but the vignette text says the one-sided test rejects the null. A pnorm upper-tail p of 0.65 does not reject, so the vignette's wording is wrong and should not be repeated.
> - Summary issue: Flags call the 0.69 autonomy AUTOC a same-sample out-of-bag heuristic. The text does not say that. The autonomy covariate was chosen after the BLP result (p = 0.0099) on the same data, with no held-out set or selection adjustment. It is post-selected in-sample evidence, and the summary should say so.
> - Summary issue: The poverty AUTOC is described as held-out (correct: text evaluates on the 40 percent test sample, p.11). The 0.0061 SE 0.0092 t 0.67 figures match.
> - Summary issue: Summary calls the BLP terms null. Cash-transfer p = 0.066 is nominally significant at 10 percent; say 'not significant at 5 percent' instead.
> - Summary issue: Untapped code_ref cites lines 9-10; the formula sits in the docstring at lines 8-9. wp = 0.5 is only DEFAULT_WEIGHTS for districts without a setting, not the district value.
> - Summary issue: Summary says PERS is an inverted-U table; the values do not form a clean U. The 533-member count and the 'about 60 percent of cells' for the 0.45 default were not verified.
> - Summary issue: Paul-alignment cites Aref and Neal as persuasion support for PERS. The vault item aref-2020 is signed-network coalition detection, not persuasion. Kalla 2020 and Broockman 2026 exist as items; their support for PERS and untapped was not checked.
> - Summary issue: Component label Terrier is not confirmed from the code path, which sits in a Codex cu-geography-v3 outputs folder.
> - Summary issue: Quarter-SD wording: the text says roughly a quarter SD, while the computed ratio is 0.3 SD. Quote 0.3 SD with a note that the text says roughly a quarter.
> - Summary issue: Ext and rel metadata fixes (five authors, year 2026, venue grf vignette) are correct per the title page.

## Gaps and flags

- No system logs treatment assignment (canvass knocks, CU outreach, PAC dollars) with propensities, so none of the targeting scores can be tested for effect heterogeneity, only for outcome prediction.
- Terrier untapped potential assumes persuasion peaks at s = 0.5 and mobilization at low-t supporters. Neither has been checked with a RATE/Qini evaluation on held-out canvass outcomes.
- The prospective receptivity contract H1 tests predictive association (Spearman with outcome level), not effect modification. Its label as a receptivity test overstates what it can show.
- The PERS inverted-U table has no estimate of heterogeneity behind it, and the 533-member scale is likely too small to detect tier-level CATE differences.
- Tutorial, not peer-reviewed evidence: both examples are non-political RCTs, and the only positive heterogeneity finding (AUTOC 0.69) comes from a same-sample out-of-bag heuristic that the authors say lacks finite-sample guarantees.
- Any causal-forest or BLP analysis built from this on Paul's data stays UNVERIFIED until a second path reproduces it (DE-062) and must follow single-look discipline. BLP coefficients are pooled and must be quoted with strata (party, chamber) or labelled UNSTRATIFIED.
- No conflict with D-025, D-021 or the rejected SNA proposals. The paper does not touch networks, lobbying data or edge weighting.
- Metadata corrected by the extract stage: five authors, year 2026, venue is the grf package vignette.

Paul has no annotated-bibliography entry and no PDF markup for this work (n_annots 0), so there is no takeaway to check. His positions on the nearest components rely on substantive persuasion papers (Kalla and Broockman 2020, Broockman and Kalla 2026 for untapped potential; Aref and Neal, Jacoby, Broockman and Kalla for PERS) and never use an estimator for effect heterogeneity. This vignette supplies the missing evaluation tool and does not contradict his reading.

## Related papers

- [[cagala-2021-optimal-targeting-fundraising]] — same-method — Vault item exists. Rel describes it as applying causal forests and policy learning to target fundraising appeals by estimated individual effect. Content was not re-read in this pass; kept on existence and title fit.
- [[tappin-2023-quantifying-potential-persuasive]] — complements — Vault item exists with a tappin-2023 ext and rel file. Rel describes it as measuring how persuasion effects vary across audiences. Kept on existence; content not re-read here.
- [[kalla-2023-which-narrative-strategies]] — complements — Vault item exists and its title (Which Narrative Strategies Durably Reduce Prejudice? Field and Survey Experiments) confirms it is a randomized field-experiment paper, which fits the 'randomized canvass design' role.
- [[imai-2021-fixed-effects-regression]] — complements — Vault item exists. Rel uses it for observational panel identification assumptions; kept on existence, content not re-read here.

### Linked from
- [[callaway-2025-difference-differences-continuous]] — complements — The grf vignette is an introduction to generalized random forests for heterogeneous effects; this paper points to Neyman-orthogonal and double machine learning routes for ACRT_glob as future work (p.22). Item exists; grf text not re-read here.