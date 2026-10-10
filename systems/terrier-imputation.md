---
type: system
name: Terrier Luna imputation of blank consumer-file fields
slug: terrier-imputation
components:
  - terrier-imputation.luna-rf-knn
  - terrier-imputation.prior-correction
  - terrier-imputation.household-model
  - terrier-imputation.gpu-xgboost-and-status
brain_notes:
  - C:/Users/pgoff/Documents/brain/projects/cu-geography.md
  - C:/Users/pgoff/Documents/brain/topics/2026-09-feature-cu-geography-luna-and-row-framework.md
  - C:/Users/pgoff/Documents/brain/concepts/single-look-holdout-discipline-cases-4-5-luna-and-pac.md
  - C:/Users/pgoff/Documents/brain/terrier/terrier.md
paper_count: 1
tags: [system, type/system-hub]
---

# Terrier Luna imputation of blank consumer-file fields

Completion of blank sci_* fields: stratified RF/KNN with logistic calibration, the prior-corrected rare-class repair, the household model, the GPU XGBoost round, majority-default and household-copy statuses, and the per-district refits for OR-05 and CT-01.

Brain notes: [cu-geography.md](obsidian://open?vault=brain&file=projects/cu-geography); [2026-09-feature-cu-geography-luna-and-row-framework.md](obsidian://open?vault=brain&file=topics/2026-09-feature-cu-geography-luna-and-row-framework); [single-look-holdout-discipline-cases-4-5-luna-and-pac.md](obsidian://open?vault=brain&file=concepts/single-look-holdout-discipline-cases-4-5-luna-and-pac); [terrier.md](obsidian://open?vault=brain&file=terrier/terrier)

## Terrier Luna imputation: evidence assessment (2026-10-09)

**Status in brief:** this system is almost unsupported by outside research. The vault holds **one** evidence row for it: DeLuca & Curiel (2022, *Political Analysis*). It is graded **VERIFIED-SOFTENED, weak, "qualifies"**, and it bears on the Luna RF/KNN component only. The other three components (prior-corrected repair, household model, GPU XGBoost and fill-status rules) have **no evidence rows**, so each is a **GAP**. A keyword sweep of all 127 paper notes for imputation and missingness terms found nothing else that applies. The only other imputation paper, Zhan & Cai 2025, is KNN imputation of trust-network opinions in group decision making. It is mapped to melange-stance and does not transfer to consumer-file fields.

**Paul's position on all four components is "No literature."** That is accurate for three of them. For the Luna component it is now slightly out of date. One weak outside result applies, and it cuts against how blanks are filled (most probable class). It does not cut against the model itself.

**What the system rests on instead is internal validation, and that is fairly strong:**
- **Holdouts.** Household-grouped CV and a frozen split-2 holdout. Colorado CD-05 scores 86.673% to 86.760% on that holdout (pooled over fields, UNSTRATIFIED). OR-05 scores 86.64% against a 77.89% majority baseline, and CT-01 scores 87.39% against 77.91%. Each per-district holdout was scored once, and these figures are also pooled over fields (UNSTRATIFIED).
- **Household control.** A shuffled-household negative control removes the household gain.
- **Independent re-scoring checks.**
- **Limits of that validation.** Internal validation answers "does it predict held-out *observed* labels". It cannot answer the questions the components actually depend on:
  - whether blanks are missing at random (MAR) given the predictors;
  - whether the rare-class probabilities are calibrated;
  - whether area shares built from the filled codes are biased.

**The one hard limit:** the Colorado split-2 holdout has been used twice, and no untouched labelled data remains. The +0.087 pp GPU gain comes from the second, disclosed use. Under the project's single-look discipline it is weaker evidence than the household model's +1.009 pp, which was a first use.

## Luna stratified RF-KNN completion

**What it computes.** Per-field calibrated class probabilities for blank sci_* values; most probable class fills the blank.

**Method.** StratifiedGroupKFold by household; RF (128 trees) vs KNN grid; logistic calibration; promotion gate on A2. Colorado holdout 86.67-86.76%. Per-district refits: OR-05 86.64% vs 77.89% majority; CT-01 87.39% vs 77.91%.

**Code refs.**
- `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/stratified-scaling-20260929/revised.py`
- `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/stratified-scaling-20260929/train_revised.py`
- `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/tools/luna_district/models.py`

**Assumptions.**
- Missing at random given predictors
- No untouched labels remain in Colorado

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[deluca-2022-validating-applicability-bayesian]] | 2022 | qualifies | Modal assignment gives a longer right tail of precinct error for White voters and higher district error than summed probabilities in nearly all simulated districts. For Black voters, PSM vastly outperforms PM at precinct level. | Applies only if modal-filled codes reach an area-level share. Whether the v3 ingest reads the Luna-completed file remains unconfirmed. The rare-class under-count is a hypothesis; the paper measures bias for race only, not for vendor consumer fields. | weak | VERIFIED-SOFTENED. System side: models.py lines 386-393 score Luna candidates by argmax accuracy against a modal majority baseline (confirmed). measures.py lines 197-220 count literal codes with raw.eq(code) over valid-code denominators (confirmed). The step that fills each blank with the most probable class was not located in the lines read. The 78 percent majority baseline was not found in the code. |

**Assessment.** **Evidence weight: one weak VERIFIED-SOFTENED bearing that qualifies the method. It neither supports nor contradicts the model's accuracy.**

**What the paper shows.** DeLuca & Curiel (2022) score race estimates from surname and geography (BISG) against self-reported race in the North Carolina and Georgia voter files, Black versus White only, over 10,000 simulated congressional plans per state. They compare two ways of turning each voter's probabilities into area totals. Summing the probabilities (PSM) beat giving each voter the single most probable race (modal assignment, PM) in nearly every simulated district. At precinct level, modal assignment had a longer right tail of error for White voters and was far worse for Black voters. The recommended rule is to aggregate summed probabilities and never argmax labels.

**Why it bears on Luna.** Luna fills each blank `sci_*` value with its most probable class. That is the PM rule. If any area-level measure (area layers, top-decile shares, Sphere or CUA area roll-ups) counts those filled codes, it is a modal-assignment aggregate. Under the paper's result it will under-count classes that are rarely anyone's most probable class.

**Transfer conditions are poor, so the bearing stays weak:**
- The paper studies race, not vendor consumer fields.
- Its probabilities come from Bayesian surname and geography models, not calibrated RF/KNN.
- It covers two segregated states with two dominant groups, and precincts and districts rather than Terrier's areas.
- The system side is only partly confirmed. `models.py` scores candidates by argmax accuracy against a modal baseline, and `measures.py` counts literal codes. The fill-with-argmax step and the 78% baseline were not located in code. Whether the v3 ingest reads the Luna-completed file is **unconfirmed**.

**Agreement with Paul's own rule.** The row framework already follows the paper's rule: Paul's ruling "Expected value + variant" takes expected values over class probabilities. The exposure is any path that reads the stored argmax code instead.

**Internal evidence on the stated assumptions (not literature):**
- **MAR given predictors: untested.** The per-district "expected on the real blanks" figures, 82.85% for OR-05 and 83.14% for CT-01, fall about 4 points below the holdout scores. So the blank rows differ from labelled rows on observed covariates. The reweighting that produces those figures itself *assumes* MAR, so it cannot test MAR. Nothing in the hub tests missing-not-at-random (MNAR) behaviour.
- **Cross-district transfer: designed out.** Paul ruled for per-district refits, with no Colorado-trained model used for Oregon or Connecticut. The empirical question "does a model trained on one district transfer?" is therefore moot for deployment. Per-district holdouts were each scored once.
- **Pooled accuracy is UNSTRATIFIED.** Every accuracy figure is pooled across fields with very different majority rates. Per-field lift and the AUC range of 0.57 to 0.95 are the stratified view, and those per-field denominators are not in the hub.

**Contradictions.**
- DeLuca & Curiel (2022, VERIFIED-SOFTENED, weak): summed probabilities give lower area-level error than modal (argmax) assignment. Luna fills each blank with the argmax class, so any area share built from filled codes is a modal aggregate. This is direct tension with the paper, conditional on the unconfirmed point that area layers read the filled codes.
- Internal tension: the row-framework ruling "Expected value + variant" aggregates class probabilities, which matches the paper. The completion export stores argmax codes. The two paths would disagree on rare-class area shares.

**Gaps.**
- Missingness mechanism of vendor consumer fields: no study in the vault. Two kinds of study would fill it. One is a validation that links vendor-modeled consumer fields to an external truth, such as survey self-report or administrative records, and checks whether blank status predicts the true value given the observed fields. The other is an MNAR sensitivity (delta-adjustment) analysis on the blanks.
- Area-level accuracy of filled codes: no vault study and no internal check. What would fill it is an area-level holdout: hide known values, fill them, aggregate to precincts or areas both ways (argmax counts versus summed probabilities), and compare error, stratified by field and class prevalence.
- Literature on RF/KNN imputation accuracy for categorical survey or consumer data: none in the vault. Methods comparisons such as missForest-style benchmarks would give external calibration of the 86-87% (pooled, UNSTRATIFIED) figures.

**Paul's position vs the evidence.** Paul's position is "No literature." That is now slightly stale. One weak VERIFIED-SOFTENED bearing (DeLuca & Curiel 2022) applies, and it qualifies the fill rule (argmax), not the model. It agrees with Paul's own expected-value ruling for the row framework. It is a warning only if area measures read the argmax-filled codes, which is unconfirmed. Nothing in the vault supports or refutes the MAR assumption the component lists, so on that point the honest status is still no literature.

**Open questions.**
- Do the v3 area layers, Sphere roll-ups or row framework read the argmax-filled `sci_*` codes, or the stored class probabilities? This is a cheap code check, and it decides whether the DeLuca bearing bites.
- Where is the step that fills each blank with the most probable class, and where is the 78% majority baseline computed? The bearing could not locate either in code.
- Is there any external truth for even one `sci_*` field, such as party affiliation against registration, that could test MAR versus MNAR on blanks?
- Per-field accuracy and lift for OR-05 and CT-01 (the stratified view behind the pooled 86.64% and 87.39%).

**Read first.** [[deluca-2022-validating-applicability-bayesian]]

## Prior-corrected rare-class repair

**What it computes.** Rare-class probability under balanced weights x training prevalence, renormalized.

**Method.** Applied to two fields.

**Code refs.**
- `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/prior-corrected-repair-20260929/prior_repair.py`

**Assumptions.**
- Balanced weights inflate rare classes by about inverse prevalence

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment.** **Evidence weight: none. This component is a GAP.** No paper in the vault bears on prior correction of class-balanced classifiers.

**What the system does.** For two fields (children D and dwelling G), the rare-class probability from a balanced-weight RF is multiplied by training prevalence and renormalized (Paul decision 1, 2026-09-29).

**Internal evidence, which is thin on the stated question:**
- Deployed accuracy is 80.941%, 81.001% and 81.075% on the B, test and stress checks. These checks were reused, not the holdout, and the figures are pooled over all fields (UNSTRATIFIED).
- The fills equal the earlier accuracy-correction export, with 1,154 fills reverted. So the repair changed which class wins in only a small number of cells.
- The empirical question is whether prior correction improves rare-class calibration on holdout. The hub reports **no calibration metric** for it: no per-class Brier score, reliability curve or expected-vs-observed rare-class share.

**Structural point (my reasoning, not a bearing).** The brain notes record that the rare-code probability is exactly 0 on about 90% of children blank rows and 83% of dwelling blank rows. A multiplicative prior correction cannot move a zero. On those rows the repair is inert, and any calibration gain is confined to the remaining 10-17%. For summed-probability aggregation this matters more than for argmax fills. A zero-mass RF vote also under-states the rare class in any expected-value area share.

**On the assumption "balanced weights inflate rare classes by about inverse prevalence".** This is the standard prior-shift argument from the classification literature, but no source for it is in the vault. For RF it holds only approximately. Class weights act on split criteria and leaf votes, not on a clean likelihood ratio, so the exact inverse-prevalence factor is an assumption that should be measured.

**Contradictions.** None.

**Gaps.**
- No literature in the vault on prior-shift or prior correction of classifier outputs. Standard candidates not yet in the vault are Saerens, Latinne & Decaestecker (2002, Neural Computation), Elkan (2001, IJCAI) and Dal Pozzolo et al. (2015, IEEE SSCI) on calibrating after undersampling. These are acquisition leads cited from memory and not yet checked.
- No holdout calibration study. What would fill it is a per-class reliability analysis on labelled rows for the two fields: compare balanced RF, balanced RF with prior correction, and unweighted RF with Platt or isotonic calibration, using Brier score, log loss and the predicted-versus-observed rare-class share. Under the single-look discipline this needs new labels or a disclosed reuse.
- No check of how much rare-class mass is lost to exact-zero RF probabilities (about 90% and 83% of blank rows) and whether smoothing, such as more trees or a Laplace correction on leaf votes, would change area shares.

**Paul's position vs the evidence.** Paul's position is "No literature," and that is accurate: the vault holds nothing on this component. The repair is an owner decision (decision 1, 2026-09-29) that rests on a textbook argument with no citation and no reported calibration test. Its observed effect is mostly on argmax fills (1,154 reverted). The calibration claim it implies is untested.

**Open questions.**
- Did any stage compute rare-class calibration for children D or dwelling G before and after the repair? If so, where is the receipt?
- Is the near-universal exact-zero rare-class probability a property of RF leaf votes at 128 trees, and do the GPU-round models (RF500 or XGBoost) produce non-zero mass on those rows?
- Do downstream expected-value measures use the prior-corrected probabilities or the earlier ones?

**Read first.** None in the vault.

## Household model

**What it computes.** RF with leave-one-out household co-member labels: +1.009 pp holdout (95% +0.959 to +1.060). The shuffled-household control removes the gain.

**Method.** Published as section 29.

**Code refs.**
- `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/household-model-20260930/household_model.py`

**Assumptions.**
- Household members share characteristics

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment.** **Evidence weight: none from literature. This component is a GAP. It has the best internal validation of the four components.**

**Internal evidence (Colorado CD-05, split-2 holdout, first and only use for this model, scored 2026-09-30, pooled over fields, UNSTRATIFIED):**
- The holdout score rose from 85.664% to 86.673%, a gain of +1.009 pp (95% interval +0.959 to +1.060).
- The reused checks moved by similar amounts: B +0.98, test +1.02, stress +1.14 pp.
- The reweighted gain on the actual blanks is +0.88 pp (+1.01 unweighted).
- A leakage audit passed. CV was grouped by household.
- **The shuffled-household negative control stayed within ±0.6 pp of legacy, so the gain disappears.** That is real evidence that the gain comes from true household membership, not from adding features or capacity.
- Brain notes put household information at about +3 pp overall, counting the earlier household-copy stage, against about +0.1 pp from bigger models.

**What is missing is external grounding** for the size and limits of within-household similarity on consumer attributes. Four questions are open:
- **Leave-one-out co-member labels at deploy time.** A blank row's co-members may also be blank or filled. If filled values were ever fed back as "labels", errors would propagate. The hub does not say whether only observed labels are used.
- **Mismatch between holdout and deployment.** The holdout scores labelled rows whose co-members are mostly labelled. Blank rows sit in households with different coverage, and 20.9% of blanks sit in groups of 200+, which suggests address clustering that may not be true households.
- **Variation by field.** Household similarity should be strong for household-level fields such as dwelling and owner/renter, and weaker for person-level fields. The 17-field pooled figure hides this.
- **No vault literature** on household clustering in voter or consumer files to say whether a gain of about 1 pp is typical.

**Contradictions.** None.

**Gaps.**
- No literature on intra-household concordance of consumer or political attributes in voter or commercial files. A study that would fill it reports concordance by attribute type (household-level versus person-level) in a linked voter-file or survey panel, ideally with household-size strata.
- No stratified internal report: household-model gain by field, by household size, and by co-member label coverage. Groups of 200+ in particular need a separate figure.
- No check that co-member features at deployment use only observed labels, never earlier fills.

**Paul's position vs the evidence.** Paul's position is "No literature," and that is accurate: the vault holds nothing on this component. The component does not need literature to justify its headline gain, because the shuffled-household control is a stronger test than most published work would give. Literature would help with the size of the gain and the conditions it depends on, especially large co-resident groups.

**Open questions.**
- When a blank row's co-members are themselves blank, what does the model see? Is the feature missing, or is it built from earlier fills?
- Gain by field and by household size, especially for the 20.9% of blanks in groups of 200+.
- Was the household model refit per district for OR-05 and CT-01, and was a shuffled-household control run there?

**Read first.** None in the vault.

## GPU XGBoost round and fill-status rules

**What it computes.** XGBoost or RF500 fills in 8 fields (+0.087 pp on second holdout use). Majority-default status when lift < 0.5 pp. Historical household copy.

**Method.** Section 30 is current. Bigger models gained only about +0.1 pp, so the ceiling is the information in the data.

**Code refs.**
- `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/gpu-publish-20260930/gpu_publish.py`

**Assumptions.**
- Lift over majority tests the information content of a field

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP; the DeLuca & Curiel 2022 bearing is filed under the Luna component and applies here by extension) | | | | | | |

**Assessment.** **Evidence weight: none from literature. This component is a GAP.** The DeLuca bearing is filed under luna-rf-knn, but its logic applies here **more strongly** (see the last point below).

**GPU round (Colorado, 8 fields, scored 2026-09-30).** XGBoost or RF500 fills gained +0.087 pp (95% +0.073 to +0.100) on the **second, disclosed use** of the split-2 holdout. The B, test and stress checks each showed about +0.08 pp, and the reweighted blank estimate is +0.098 pp. Because the holdout had already informed earlier selection, this is the weakest of the holdout figures under the single-look discipline. It is still consistent across checks. It is pooled over 8 fields (UNSTRATIFIED).

**"The ceiling is information, not model capacity."** Two higher-capacity families gaining about +0.1 pp against about +3 pp from household features supports that claim **for these feature sets**. It does not show that the fields are near their information limit in general. New predictors such as geography, precinct context or registration history could still move them. Treat it as a scoped internal finding, not a general one.

**Majority-default rule** (13 fields marked on 2026-09-29; 3 fields carrying 238,653 fills in the current export). A field is marked when its accuracy lift over the majority class is under 0.5 pp on all three checks. The stated assumption is that "lift over majority tests the information content of a field". **That assumption is weak (my reasoning, not a bearing).**
- Argmax accuracy can only beat the majority baseline when the predicted probability for some non-majority class becomes the highest.
- On imbalanced fields, a model can carry real discriminative information without ever flipping the argmax. The per-district AUC floor of 0.57 shows such fields exist.
- Accuracy lift is a test of whether the **argmax decision** changes. It is not a test of information content.
- A proper-scoring-rule lift, measured as log-loss or Brier improvement over the prevalence baseline, or per-field AUC, would test information.

**Link to DeLuca (my extension, not a separate bearing).** A majority-default fill is modal assignment in its most extreme form: every blank gets the population mode. Any area share built from those codes assigns zero of every minority class to blank rows. Summing the calibrated probabilities would keep the sub-threshold information. The paper's PSM-over-PM result is the closest outside evidence on this, with the same weak transfer caveats.

**Historical household copy.** This is section 28, superseded, and no copies remain. It is metadata only and needs no evidence.

**Contradictions.**
- Majority-default fills are maximal modal assignment. Under DeLuca & Curiel (2022, VERIFIED-SOFTENED, weak, transfer from race to consumer fields unverified), area aggregates built from modal codes carry higher error than summed probabilities. This is tension, conditional on area measures reading these codes.
- The stated assumption that lift over majority tests information content conflicts with the per-district AUC evidence (as low as 0.57). Fields can carry information without accuracy lift. This is an internal inconsistency in the rule's rationale, not a literature contradiction.

**Gaps.**
- No literature on model-capacity versus information ceilings in tabular imputation. A study that would fill it is a learning-curve and feature-ablation design: hold features fixed while scaling capacity, then hold capacity fixed while adding feature families, per field.
- No literature or internal test for the 0.5 pp majority-default threshold. A per-field comparison would fill it: for the 13 marked fields, compare accuracy lift with proper-scoring lift (log-loss or Brier against prevalence) and AUC, and report how many marked fields carry non-trivial information.
- No area-level check of majority-default fields: compare area shares built from majority codes with shares built from summed probabilities against held-out truth.

**Paul's position vs the evidence.** Paul's position is "No literature," and that is accurate for this component's own claims. The capacity-ceiling claim is a scoped internal result (8 fields, second holdout use). It is reasonable but should not be quoted as general. The majority-default rule rests on an accuracy-lift rationale that the system's own AUC figures undercut. The one weak outside bearing, DeLuca, applies to it more strongly than to the Luna fill itself.

**Open questions.**
- For the 3 live majority-default fields (79,551 fills each), what are the per-field AUC and log-loss gain over prevalence?
- Do any area measures or Sphere roll-ups read the majority-default codes, or the underlying probabilities?
- Were the OR-05 and CT-01 "six fields labelled as the file default" chosen by the same accuracy-lift rule, and what are their AUCs?

**Read first.** [[deluca-2022-validating-applicability-bayesian]]

## Across components

## Cross-component picture

**1. Where blanks become area numbers is the real exposure, not model accuracy.** All four components optimize and report **argmax accuracy against held-out observed labels**. The only outside evidence (DeLuca & Curiel 2022, weak, VERIFIED-SOFTENED, race rather than consumer fields) says modal assignment biases area aggregates. Two places in this system would be affected:
- every Luna fill;
- the majority-default fills, where the bias is worst because every blank gets the population mode.

Paul's own row-framework ruling, "Expected value + variant", already takes the paper's side. A single code check decides whether this is a real problem or a non-issue: do the v3 area layers read stored class probabilities or the filled codes?

**2. Calibration carries the load, and none of it is measured.** Summed-probability aggregation is only as good as the probabilities. The prior-corrected repair changes probabilities, and so would any switch from argmax codes to expected values. Yet the hub reports no per-class calibration anywhere in the system. The exact-zero rare-class mass on about 83-90% of blank rows (children and dwelling) would make expected-value shares under-count rare classes too. So switching the aggregation rule alone does not fully fix it.

**3. MAR is assumed everywhere and tested nowhere.** The gap between the holdouts (86.64% and 87.39%) and the blank-row expectations (82.85% and 83.14%) shows that labelled and blank rows differ on observed covariates. The reweighting that produces the blank expectations itself assumes MAR. No component has an MNAR sensitivity check.

**4. Pooled accuracy is the only figure quoted.** All accuracies pool fields with very different majority rates and are UNSTRATIFIED. The stratified view, per-field lift and AUC (0.57 to 0.95), is what decides the majority-default rule and the capacity-ceiling claim.

**5. Holdout budget.** Colorado has no untouched labels left. The OR-05 and CT-01 holdouts were each scored once. Any aggregation or calibration study on existing labels is a second look and must be disclosed; under the single-look discipline, whether to spend it is Paul's call. New labels are the clean route, either from an external truth source for one or two fields or from a fresh frozen split in a new district.

**6. Evidence base.** One paper in total, weak, for a four-component system. Three components are pure GAPs. The system's credibility rests on its internal controls, chiefly the shuffled-household control and the independent re-scoring. Those controls are good at what they test, but they do not address aggregation, calibration or missingness mechanism.

## Research agenda

1. Code check (cheap, decides the DeLuca transfer): trace whether the Terrier v3 area layers, Sphere and CUA roll-ups, and walk-list scores read the stored argmax or majority-default `sci_*` codes or the class probabilities. Locate the argmax fill step and the majority-baseline computation the bearing could not find.
2. Area-level aggregation study: on labelled rows with values hidden, compare precinct- and area-level class shares from argmax counts against summed probabilities, stratified by field and class prevalence. Use OR-05 or CT-01, disclosed as a second look, or new labels. Paul must approve the holdout use under the single-look discipline.
3. Rare-class calibration study for children D and dwelling G: per-class reliability, Brier score and log loss for balanced RF, balanced RF with prior correction, and unweighted RF with Platt or isotonic calibration. Also quantify the exact-zero RF mass on blank rows and whether the RF500 or XGBoost models remove it.
4. Replace or augment the 0.5 pp accuracy-lift majority-default rule with a proper-scoring-rule test (log-loss or Brier gain over prevalence) plus per-field AUC. Re-examine the 13 Colorado fields and the six OR-05/CT-01 file-default fields.
5. MNAR sensitivity analysis: delta-adjust the predicted class probabilities on blank rows and report how area shares and row rankings move. If any `sci_*` field has an external truth, such as registration against vendor party, test MAR directly there.
6. Stratified household-model report: gain by field, by household size (separate figure for the 20.9% of blanks in groups of 200+), and by co-member label coverage. Confirm that deploy-time co-member features use observed labels only, and run the shuffled-household control for OR-05 and CT-01 if the household model was refit there.
7. Literature acquisition (all are leads cited from memory, not yet in the vault or checked): Saerens, Latinne & Decaestecker 2002 and Elkan 2001 on prior-shift correction; Dal Pozzolo et al. 2015 on calibration after rebalancing; Stekhoven & Buhlmann 2012 (missForest) on RF imputation accuracy; Igielnik et al. 2018 (Pew, commercial voter files) and Ansolabehere & Hersh 2012 on vendor- and voter-file validation; and any study of intra-household concordance in voter or consumer files.
