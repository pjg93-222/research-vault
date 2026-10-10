---
type: system
name: "Melange member psychometrics (Big Five, dispositions, persona)"
slug: melange-psychometrics
components:
  - melange-psychometrics.lexical-big5
  - melange-psychometrics.gold-panel-validation
  - melange-psychometrics.llm-and-hybrid-readers
  - melange-psychometrics.behavioral-dispositions
  - melange-psychometrics.cooperation-v2
  - melange-psychometrics.affective-n
  - melange-psychometrics.persona-agency-communion
  - melange-psychometrics.shrinkage-and-weighting
brain_notes:
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-personality-psychometrics.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-personality-psychometrics-external-criteria-and-open-a-search.md
  - C:/Users/pgoff/Documents/brain/concepts/single-look-holdout-discipline-case-1-moneyball-gate2.md
  - C:/Users/pgoff/Documents/brain/concepts/single-look-holdout-discipline-cases-2-3-n-slot-and-gold-template.md
paper_count: 27
tags: [system]
---

# Melange member psychometrics (Big Five, dispositions, persona)

Measuring legislator personality and behavioural dispositions from text and behaviour. Covers the lexical Big Five pipeline (Ramey-style, with SMOreg and RKH calibration), the LLM item-level reader, the ridge hybrid, the CT-CM latent model, behavioural dispositions, cooperation_v2, affective N, the agency/communion persona channel, shrinkage rules, and the genre-transfer tests. These feed the receptivity layer.

Brain notes: [topics/2026-07-feature-personality-psychometrics](obsidian://open?vault=brain&file=topics%2F2026-07-feature-personality-psychometrics); [topics/2026-07-feature-personality-psychometrics-external-criteria-and-open-a-search](obsidian://open?vault=brain&file=topics%2F2026-07-feature-personality-psychometrics-external-criteria-and-open-a-search); [concepts/single-look-holdout-discipline-case-1-moneyball-gate2](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline-case-1-moneyball-gate2); [concepts/single-look-holdout-discipline-cases-2-3-n-slot-and-gold-template](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline-cases-2-3-n-slot-and-gold-template)

**Melange member psychometrics: evidence assessment (hub snapshot 2026-10-10)**

The hub holds 74 bearings on 7 of the 8 components: 11 VERIFIED, 51 VERIFIED-SOFTENED and 12 UNVERIFIED. Interview weighting and gated shrinkage (C_INT_W / P6) has none. On top of the hub I ran a short online scan (2026-10-10). Every result from that scan is a lead from a single read (DE-062) and none is a finding.

**What the evidence says overall**

1. **Text-to-trait signal is weak.** Against self-report, closed-vocabulary word use correlates at about |r| .10 to .16 (Pennebaker; Koutsoumpis meta-analysis: mean R² 5.07% across 52 LIWC categories). Observer ratings are easier to match than self-report (cue utilization R² about 38.5%). Matching observers shows that raters and text use the same cues; it does not show trait accuracy.
2. **The system's validation story rests on criteria that share its method.** RKH is itself built on a Mairesse-type SVR. Refitting to RKH therefore turns convergence into a fit statistic, not validation. Mairesse 2007 found that SMOreg trained on observer ratings did worse than the mean baseline on all five traits. Its relative absolute error ran 132% to 213%, against a baseline of 100%.
3. **Senate gold panels are the only independent criterion.** They are Senate-only and historic, skewed by party at the extremes, and include senators rated by a single coder (k=1), whom Mondak excludes.
4. **Several numbers in the system description could not be located in the repo:**
   - the held-out CV figures against RKH (E .510, A .406, C .363, ES .258, O .214; mean .350);
   - the claim that O was reversed on the party check;
   - the affective-N genre-transfer values (rho .050 and .161), which were not in the cited lines.

   What the repo does hold: `data/members/big5_v2.json` records held-out r against *behavioural* targets (A .059, C .281, E .563, O .135), and the brain records C +0.348 held out. The component descriptions look stale. The next action is to re-source them, not to re-run anything (single-look discipline).

**Actual errors to fix.** These are documentation or code defects, not modelling findings.
- The `behavioral_dispositions.py` docstring attributes roll-call participation as a C criterion to Ramey 2016. The paper's only C outcome is the share of ceremonial bills.
- The `big5_llm.py` docstring says "balanced keying". The items are keyed 3:1 for O, C, E and A, and 2:2 for ES.
- `build_big5_senate_gold.py` drops the malformed academic Feingold row without warning (the panel has 125 records against 126 in the appendix). Its comment says only headings reach that branch, which is wrong.
- The primary gold validators do not apply Mondak's rule excluding k=1 senators.
- The `emit_affective_n.py` docstring still says the score combines LIWC. LIWC is now only a convergent anchor.
- The brain ruling of 2026-07-04 misreads Birkenmaier. The paper does not call its task "easier", and .72 is the agreement on whether any cue is present, not the agency or communion alpha.

**Online scan.** These copies are downloaded to `C:\Users\pgoff\Documents\research\_work\downloads\scan_2026-10-10\` with one folder per paper. None has been ingested into `papers/` yet.
- `cao_kosinski_2024_scirep\` – GPT-3 name embeddings predict how 226 public figures' personalities are perceived (600 raters). Raw r is .78 to .88; after controlling for likability and demographics it falls to .53 to .70. This is direct evidence for the reputational-prior assumption behind de-identification, and it shows likability as a large share of perceived personality.
- `peters_matz_2024_arxiv\` – Zero-shot GPT inference from Facebook posts against self-report, mean r .29. Accuracy differs by sex and age.
- `osnabrugge_hobolt_rodon_2021_apsr\` – In about 2 million UK and Irish parliamentary speeches, emotive rhetoric rises in high-audience debates. Affect in legislative speech follows the audience.
- `rheault_2016_plosone\` – In a century of House of Commons debates, emotional polarity tracks the national economy.
- `aroyehun_2026_arxiv\` – A 2026 preprint on 1.7 million Congress speeches (1973 to 2024). Emotion varies with policy domain and ideology, and anger is negatively associated with legislative effectiveness.
- `decter_frain_frimer_2016_frontiers\` – An aggregate time series of agency and communion in House floor speech (1996 to 2014) against Congress approval. Only the "helping" form of communion is positive.
- `hargrave_blumenau_2022_ucl\` – In UK MPs' speech (1997 to 2019), gendered style shifts over time, with women adopting more agentic styles.
- `harbridge_volden_wiseman_2023_jop\` – Members of the 93rd to 114th Congresses who draw a larger share of their cosponsors from the other party are more effective. The paper proposes reciprocity as the path.

Cao & Kosinski 2024 (PNAS Nexus) could not be downloaded because PMC blocks scripted fetches. Park et al. 2015 (JPSP) is paywalled and no trusted open copy was found. Both remain in `missing.md`.

## Lexical Big Five (Ramey-style closed vocabulary)

**Computes.** Corpus-relative Big Five z (O, C, E, A, ES; N = -ES) from category rates of floor speech and interviews.

**Method.** Rates per 1000 words (LIWC-analog, Empath, MATTR, Brysbaert concreteness) are z-scored and combined with linear weights from published correlates or SMOreg (Mairesse) fit on essays, then refit to RKH targets per trait (hybrid). Shrinkage z x sqrt(tokens/1500). Held-out CV vs RKH: E .510, A .406, C .363, ES .258, O .214 (mean .350). O was reversed on the party check.

**Code refs.** `fec/members/big5_traits.py:1-286`, `fec/members/fit_smoreg_rkh.py`, `fec/members/fit_smoreg_essays.py`, `fec/liwc/mairesse_smoreg.json`

**Assumptions**
- Closed-vocabulary rates carry trait signal
- RKH estimates are a valid reference (method-shared, so convergence is inflated)
- Scores are meaningful only within this corpus

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[birkenmaier-2025-measuring-politicians-public]] | 2025 | qualifies | Ramey et al. used a pretrained Mairesse SVR, which the paper says lacks validation against human-annotated data and appears outdated. The paper's own bag-of-words SVM, modelled on Ramey's approach, scores 0.55 to 0.63 macro-F1 on its cue task and often fails to reach 50 percent of functional test units. | The SVM numbers are the authors' own cue-classification results, not member-level trait regression, so they do not measure this component's accuracy. The critique bites harder because RKH, the reference standard, is itself built on Mairesse-type SVR (paper Sec 2.5 p.5 on Ramey et al.). | moderate | VERIFIED-SOFTENED |
| [[ramey-2016-measuring-elite-personality]] | 2016 | method-source | SMOreg on standardized LIWC and MRCPD features per member-year; scores relative to the Congressional Record (fn 10); 100 sentence-level bootstraps per member-year; CI width about 0.3 or less above about 5,000 words a year (median member about 11,000 words); optional jackknife over other years. | Transfers as a procedure only. The paper's precision band assumes about 11,000 words per member-year; the 1,500-token floor is far below the paper's 5,000-word band, so the precision claim does not transfer. Refitting to RKH makes RKH convergence a fit statistic, not validation. | strong | VERIFIED-SOFTENED |
| [[marrero-2022-evaluating-voice-samples]] | 2022 | qualifies | LIWC 2015 features alone explained 3.13 percent of out-of-sample variance in self-reported Big Five (N=614 students, average sample about 92 words). The authors call the estimates conservative because samples were brief, constrained and drawn from students. Text names pronoun as top LIWC feature for C and negate for E; feature-level explanations are called highly speculative. | The paper's 3.13 percent is a self-report criterion in students with about 92 words per sample. It does not validate the system's fixed signed weights, which the paper did not test. Its small-sample caution supports a thin-corpus floor only by analogy, since Melange members have far more text than 92 words. | weak | VERIFIED-SOFTENED |
| [[rammstedt-2025-going-global-language]] | 2025 | context | The authors argue that a standardized, parallel adaptation makes the 39 versions more likely to be comparable than independent national translations. They report no psychometric evidence for this: multi-group CFA alignment was run on Field Trial data (about 450 to 1,800 respondents per country) but the results are confidential, and the versions are described as not yet fully validated. | The analogy maps languages and countries onto speech genres (interview, floor, hearing, social). It supports demanding empirical invariance evidence before pooling or comparing scores across genres, rather than relying on a uniform procedure. It provides no method or numbers the system could import. | weak | UNVERIFIED |
| [[koutsoumpis-2024-kernel-truth-text]] | 2024 | qualifies | Self-reported Big Five correlate with LIWC categories at most \|rho\| .08-.14 (abstract p. 843). The 52 categories explain a mean R2 of 5.07 percent of self-report variance (narrow categories, k = 25 self-report samples, n = 83,243-84,084). About 87 percent of that n is Schwartz et al. 2013 (n = 72,809). Heterogeneity significant in 96 percent of cue-validity effects. Table 7 lists LIWC best-category average r = .10 (self) against Moreno et al. .27, HTTP .31, IBM-WPI .13 and human judges .12, all uncorrected. | Paper samples are mostly informal English social media and student or general-population samples, LIWC 2001-2015. Congressional speech is formal and partly staff-written, and the paper shows formality attenuates markers. The system uses exemplar lists, not licensed LIWC dictionaries, so category equivalence is unverified. Table 7 values are post hoc best-category maxima and cannot rank methods. | strong | VERIFIED-SOFTENED |
| [[koutsoumpis-2024-kernel-truth-text]] | 2024 | qualifies | Bivariate cue-validity effects (self-report, Table 3): Openness words over six letters rho .11; Conscientiousness negative emotion -.12, swear -.11, anger -.10; Agreeableness anger -.14, swear -.10, negative emotion -.10, positive emotion .10. Emotional Stability has no first-person singular validity effect at \|rho\| .10 or more. Cue utilization (observer): Emotional Stability first-person singular -.29. | The paper estimates are bivariate correlations. SMOreg weights are multivariate, so suppression can flip signs without error. The ES first-person comparison sets an observer-based paper effect against a model fit to other data, so it is not like-for-like. The flagged signs remain UNVERIFIED (DE-062) and are an audit lead only. | moderate | VERIFIED-SOFTENED |
| [[celli-nd-twenty-years-personality]] |  | qualifies | Text Personality Recognition SotA is an average correlation of about 0.30 over a 0.133 baseline (WASSA2024 Rho 0.300; PANDORA talks 0.292). Among 45 text systems: SVM 20, neural networks 12, Naive Bayes 10; social network data 27, Essays 15; psycho-linguistic features 16 homogeneous, 20 broader. | Paper figures are against self-report in general-user corpora. The system's held-out CV figure against RKH is not a Recognition benchmark and must not be compared with 0.30 as if it were. The CV figure (.350, with E .510 to O .214) was not found anywhere in the Melange fec tree. fit_smoreg_rkh.py produces candidate refit weights promoted only on REFIT WINS, so the summary's claim that weights are refit to RKH should say candidate, not active. | moderate | VERIFIED-SOFTENED |
| [[pennebaker-2003-psychological-aspects-natural]] | 2003 | qualifies | Self-reported Big Five correlates with closed-vocabulary word use at r = 0.10 to 0.16 across several hundred college students with multiple writing samples (Pennebaker and King 1999). Neuroticism goes with negative emotion and I; extraversion with positive emotion and social words; agreeableness with positive and low negative emotion. | The paper ceiling applies to text matched to self-report in students. RKH targets are derived from text, so .35 against RKH is not comparable evidence of validity. Hand-weight signs carry over only if the fallback is reactivated. The review does not cite Yarkoni or Mairesse. | moderate | VERIFIED-SOFTENED |
| [[pennebaker-2003-psychological-aspects-natural]] | 2003 | qualifies | Word use is consistent across text types for 36 LIWC dimensions (Pennebaker and King 1999). Four-week retest of student everyday conversation (EAR, 52 college students, Mehl and Pennebaker 2002a) was r = 0.41 for standard linguistic variables and 0.24 for psychological-process variables. Successive 2-minute speech intervals correlated 0.51 across 21 categories, a result the review attributes to Gleser et al. 1959 on 5-minute talks. | The paper consistency evidence comes from diaries, college writing assignments, journal abstracts and recorded student conversation, not all self-authored. Floor, press and social text is outside its scope. The inference that 0.24 retest reliability predicts weak genre transfer is the analyst's, not the paper's. | moderate | VERIFIED-SOFTENED |
| [[azucar-2017-predicting-personality-traits]] | 2017 | qualifies | Pooled random-effects r per trait: O .39 [.30,.48], C .35 [.29,.42], E .40 [.33,.46], A .29 [.21,.36], N .33 [.27,.39], 16 effect sizes each. Heterogeneity I2 93.15-97.46. Each study contributed its best model or highest single-feature effect. Paper-side numbers reproduced exactly from Table 2 (Z, Q, I2, tau2). | Azucar's pooled r is not a benchmark for this system. Its criterion is self-report from lay users; the system's held-out targets are behavioral dispositions, and no RKH held-out figure exists in the repo. Heterogeneity is over 93 percent and best-model selection probably inflates the pooled values. Any citation must carry the label UNSTRATIFIED (16 studies, 4 platforms, 2012-2017; two included studies, Farnadi 2016 Study 1 and Kosinski 2013, used MyPersonality data). The ordering claim as first written is wrong: in Azucar E (.40) is highest and O (.39) second; A (.29) is lowest. In this system O is the weakest trait on the breadth target (.135), which is not an inversion of a best-predicted trait. | moderate | VERIFIED-SOFTENED |
| [[amsalem-2018-effect-politicians-personality]] | 2018 | qualifies | Extraversion appears in observable media presence (beta .117 pooled; b = 0.117, p < .05 on websites; p < .1 for newspapers and TV). TIPI Cronbach alphas are .75 extraversion, .39 openness, .53 neuroticism, .40 conscientiousness and .15 agreeableness. The agreeableness item pair correlates r = .09, p > .1. | The paper does not say an E-broadcast-volume link is partly expected for real extraverts. It says extraversion reflects need for interaction and public recognition (lines 884-887). Overlap between text-E and interview volume is possible but not shown. Lexical E also draws on posts and floor speech, so the circularity point applies only in part. The paper calls the TIPI sufficiently valid overall (line 538), so the claim that elite self-report is not usable for A, O or C is the analyst's inference. | weak | VERIFIED-SOFTENED |
| [[lechner-2025-mapping-political-personality]] | 2025 | suggests-extension | Global Big Five domains lack political specificity, and contextualized measures outperform non-contextualized ones. Facets diverge within a domain: Compromise with Agreeableness r=.33 versus Trust r=.50, and Intellect with Emotional Stability r=.41, which the authors call unexpected. | The argument concerns survey item design, not text features. A facet-level text reader would need its own validation, and this paper does not change the domain-level validity record. | weak | VERIFIED |
| [[mondak-nd-online-appendix-personality]] |  | measurement-source | Insider, APSA Fellow and academic raters supply Big Five ratings of senators' public persona, usable as an external criterion for text-derived traits. The appendix calls these Big Five measures built from insider and academic data. It does not call them independent or observer ratings. | Applies to senators who overlap the appendix and to text from the same public-persona era. Does not carry to House members or to genres (floor, interview, social) the raters never saw. Ratings describe perceived public persona, not self-report. | moderate | VERIFIED-SOFTENED |
| [[simchon-2024-persuasive-effects-political]] | 2024 | qualifies | The ad openness score comes from a language model trained to predict how attractive a text is to readers high in openness (Simchon et al. 2023, ref 13), not who wrote it; prompted ChatGPT rewrites shift that score by Cohen's d = 0.51 and GPT-3 rewrites by only d = 0.07, so the style can be produced on demand. | The paper shows openness-styled text is easy to generate on demand (d = 0.51 with a simple prompt), which supports the worry that legislators' public text reflects staff and communications strategy rather than author trait; it does not validate author-side lexical O in either direction. Applies to Melange only as a construct-validity caution for O on prepared speech. | weak | UNVERIFIED |
| [[naz-2025-machine-deep-learning]] | 2025 | qualifies | Deep and transformer readers beat TF-IDF/closed-vocabulary shallow ML at trait detection. MBTI experiment: Bi-LSTM + sentence embeddings Ext 92 / Cons 90 / Agr 91 / Open 89; DistilBERT Open 92 / Cons 91; SVM (TF-IDF) Open 89 / Cons 80; POS-DT Cons about 53. The paper reports accuracy only, with no split, cross-validation or variance. | Weak transfer. The paper's labels are MBTI types that ordinary social-media users reported about themselves, its unit is the post or user, and its metric is accuracy on dichotomized traits. The system's targets are expert and insider ratings or RKH estimates for about 147 to 533 legislators, scored by held-out correlation. Fine-tuning a deep reader would need thousands of labeled members, and the system has no such set. The paper therefore gives no usable effect size for replacing the lexical reader, and its accuracies must not be compared with the system's r. | weak | UNVERIFIED |
| [[salam-2022-learning-personalised-models]] | 2022 | suggests-extension | Within the paper's NAS talk-turn models, adding sentiment to duration, content and speech-rate features moves AMSE from 0.874 to 0.942 (generic), 0.883 to 1.074 (age split), and 0.844 to 0.784 (gender split). Single test split, no intervals. The authors themselves expected sentiment to hurt, because their Spanish-only sentiment tool was applied to Catalan and English turns. | The paper's sentiment features are a Spanish sentiment classifier applied to talk turns, not a lexical trait dictionary, and the paper's own language-mismatch caveat applies. The transferable item is a diagnostic only: report text-trait correlations against the target stratified by sex, or label them UNSTRATIFIED. This paper supports no per-sex models. | weak | VERIFIED-SOFTENED |
| [[soto-2017-next-five-inventory]] | 2017 | context | Self-peer domain agreement ranged .42 to .69 (mean .56) and facet agreement .27 to .73 (mean .49) in the student sample, with 184 peers (60 percent friends, 30 percent romantic partners). Cross-instrument convergence of the BFI-2 with other Big Five measures runs .72 to .92, from NEO PI-R to BFI. | Use the paper's figures only as a rough scale for convergence and self-peer agreement, not as a bar. The paper compares human self-report with human peer report, and its convergence figures come from the student sample. The shared-method claim (Mairesse and LIWC on both sides) is not stated in the script and is not confirmed here. | weak | VERIFIED-SOFTENED |
| [[kumari-nd-five-personality-predictions]] |  | context | Proposes predicting Big Five trait percentages from tweets with TF-IDF, LIWC/psycholinguistic categories, sentiment and profile features, using RF/SVM/NN (Sec 3.3) or NB/SVM/RNN/BERT (Sec 3.5) plus regression. No N, no label source and no accuracy, precision or recall values are reported. It says cross-validation and confusion matrices are used (Sec 3.4) but shows none. | It does not transfer as evidence. There are no labels, no sample, no results, and the genre (general Twitter users) differs from legislators' floor speech and interviews. At most it is a list of candidate features. Its absolute 'percentage' outputs conflict with the system's rule that scores are meaningful only within this corpus. Its accuracy claim stays unverified and should never be cited as a benchmark (DE-062 spirit). | weak | UNVERIFIED |
| [[celli-2012-role-emotional-stability]] | 2012 | method-source | An unsupervised ES tagger built from Mairesse et al. 2007 linguistic correlations (Table 2) scores 0.625 accuracy against a 0.5 majority baseline on Personage (8 users x 20 sentences). The authors call this in line with the state of the art. Prior text classifiers reached E.Stab. accuracy of 0.531-0.705 (Table 1). | Shared lineage only: both apply published Mairesse correlates to new text without local supervision. Celli's evidence covers 2011 English tweets from public users and 8 human-rated users. Congressional floor speech and interviews are a different genre and population. The paper does not show that the ES weights transfer, and its accuracy is barely above chance at that sample size. Use it as a historical benchmark for how weak unsupervised ES reading is, never as validation. | weak | UNVERIFIED |
| [[yang-2023-getting-personal-deep]] | 2023 | qualifies | On PANDORA DeepPerson reaches AUC 75.0 (+33.7% vs an IBM feature baseline of about 56.1) against best comparator AttRCNN 64.6. The authors say IBM moved away from LIWC toward open-vocabulary GloVe embeddings. In Table 7 the concept encoder adds about 2 pp AUC (68.0 with it vs 66.2 without). The paper does not rank closed-vocabulary scoring as a family. | The paper's evidence is binary median-split self-report classification on Reddit, Facebook and student essays. The system is judged against RKH and Senate gold panels on congressional speech, which the paper never tests. The held-out CV figure of .350 vs RKH and the refit-to-RKH step were not located in the cited file, so they stay unverified. | moderate | VERIFIED-SOFTENED |
| [[mairesse-2007-automatic-recognition-personality]] | 2007 | contradicts | SMOreg trained on EAR observer ratings (96 speakers, 10-fold CV) has relative absolute error above the mean baseline for all five traits: E 140.05, ES 162.05, A 173.97, C 131.75, O 213.20 percent (baseline 100). The authors say linear regression and SVMs perform poorly and suggest they need a bigger dataset. | Holds as a statement about the Mairesse observer model and Ramey fn 4. The paper never says what data the distributed observer model was fit on, so the claim that it was fit on all 96 speakers is an assumption. Ramey line 191 calls the training sample the essay sample, so Ramey is internally inconsistent on provenance. | strong | VERIFIED-SOFTENED |
| [[mairesse-2007-automatic-recognition-personality]] | 2007 | qualifies | On the essays corpus with self-reports, SMO binary accuracy is 54.9 to 62.1 percent (openness 62.1 vs 50.3 baseline). SMOreg relative error is E 100.65, ES 98.35, A 100.28, C 99.30 (none significant) and O 94.19 (significant). Best Pearson r is 0.33 for openness and 0.24 for emotional stability. | Paper figures verified. Table 12 SMO values are E 54.93, ES 57.35, A 55.78, C 55.29, O 62.11, so the 55 to 62 range is rounded. The grid search picks the best of 12 settings on the same folds, so its CV figure is optimistic, as stated. The paper's binary accuracies are not comparable to the system's refit since the features differ (Empath-augmented). | moderate | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: moderate as procedure, weak as validity.** The hub has 22 rows (19 distinct papers): 1 VERIFIED, 16 VERIFIED-SOFTENED and 5 UNVERIFIED.

**What the literature supports**
- Ramey 2016 (VERIFIED-SOFTENED, method source) documents the procedure the system follows: corpus-relative standardization, member-level features and N = -ES.
- Self-report convergence for closed-vocabulary features is low. Pennebaker 2003 reports r .10 to .16 in students. Koutsoumpis 2024 reports a mean R² of 5.07%, with maximum self-report cue validity of |rho| .08 to .14 and heterogeneity in 96% of effects. Marrero 2022 reports 3.13% out-of-sample R² on texts of about 92 words.
- The Text Personality Recognition state of the art is about r .30 against self-report (Celli review).

**Why the system's .350 figure is not comparable**
- The criterion differs: RKH is a text-derived target, while the benchmarks above are self-report.
- RKH shares the method. It is built on a Mairesse-type SVR, so refitting to RKH makes agreement a fit statistic (Ramey and Birkenmaier rows).
- Precision does not transfer. Ramey's CI band of about 0.3 assumes about 5,000 or more words per member-year, and the system's low-confidence floor is 1,500 tokens.

**Contradictions**
- Mairesse 2007 (strong, VERIFIED-SOFTENED). SMOreg trained on observer ratings had relative absolute error of 132% to 213%, against a mean baseline of 100%, on every trait. On self-report essays only O beat baseline. The model family behind the active weights did not beat the mean in its own source paper.
- The provenance of the shipped weights is unclear. One row says the RKH refit (`fit_smoreg_rkh.py`) is promoted only when it wins and is a candidate, not active. Another says the v2 builder (`big5_lexcal_v2.py`) learns against behavioural criteria and treats RKH and Mairesse weights as v1 priors.
- The held-out CV figures against RKH (E .510, mean .350) were not found by any of six verifiers. What `big5_v2.json` does hold is held-out r against behavioural targets: A .059, C .281, E .563, O .135, with ES missing.
- Koutsoumpis disagrees with some SMOreg weight signs: ES first-person singular +.0094 and E positive emotion -.0339. This is UNVERIFIED (DE-062). The comparison is multivariate weights against bivariate paper effects, so suppression could explain it. Treat it as an audit lead only.

**Transfer conditions**
- Nearly all evidence comes from student or social-media self-report in informal English. Congressional speech is formal and partly staff-written, and Koutsoumpis shows that formality weakens these markers.
- Simchon 2024 (UNVERIFIED) shows openness-styled text can be produced on demand (d .51). This is a construct caution for O on prepared speech.
- Yang 2023 and Naz 2025 favour open-vocabulary and transformer readers, but on self-report classification. They give no usable effect size for this system.

**Contradictions**
- Mairesse 2007: SMOreg on observer ratings has relative absolute error above the mean baseline for all five traits (E 140%, ES 162%, A 174%, C 132%, O 213%). The model family behind the active weights failed in its own source paper.
- The component's held-out CV figures against RKH (E .510, A .406, C .363, ES .258, O .214; mean .350) were not located by six independent checks. big5_v2.json instead holds held-out r against behavioural targets (A .059, C .281, E .563, O .135).
- There are conflicting statements about which weights are active. The RKH refit is a candidate promoted only on a win. The v2 builder learns against behavioural criteria. The component text says 'refit to RKH'.
- The claim that O was 'reversed on the party check' is not supported. The ONC candidate marks O NOT_LOOKED_CRITERION_STOP_BAR_MISS.
- Some SMOreg signs differ from Koutsoumpis cue-validity signs (ES first-person singular, E positive emotion). UNVERIFIED (DE-062), and suppression may explain them.

**Gaps**
- No convergence of text traits with an independent criterion for the House. The Senate panels cover only the overlap with current members.
- No genre-invariance evidence (floor vs interview vs social) for any trait other than N. Needed: a multi-trait multi-method (MTMM) design with genre as the method, plus a pre-registered single look at cross-genre rank stability.
- No measured staff-authorship effect on the system's corpus. Ramey asserts robustness to speechwriting, but only for floor speech.

**Paul's position vs the evidence.** Paul's reading, 'dated but foundational', together with Birkenmaier's critique, fits the evidence. One point should be added: Mairesse 2007 itself shows SMOreg underperforming a mean baseline, which is stronger than 'outdated'. No error in his notes.

**Open questions**
- Which weight file is active in the shipped build: the published-correlate weights, mairesse_smoreg.json, the RKH refit candidate, or the v2 behavioural fit? Where is the .350 CV figure stored?
- Does the 1,500-token floor give usable precision when Ramey's CI band assumes about 5,000 words per member-year?
- Do floor-speech traits survive removing staff-drafted prepared remarks?

**Read first**
- [[koutsoumpis-2024-kernel-truth-text]]
- [[mairesse-2007-automatic-recognition-personality]]
- [[ramey-2016-measuring-elite-personality]]
- [[birkenmaier-2025-measuring-politicians-public]]
- [[celli-nd-twenty-years-personality]]
- [[pennebaker-2003-psychological-aspects-natural]]

## Big Five validation against Senate gold panels and criteria

**Computes.** Pass or miss of trait validity looks. E and A pass both panels; O passes academics only; N passes insiders only; C has an insider association only.

**Method.** Pre-registered looks: interview C vs votes cast rho +0.155 PASS; C vs follow-through +0.112 PASS. Several improvement rounds MISSed. A-ES about .44 is population-real (RKH benchmark) plus about .21 text-tone halo. Status labels must accompany any external claim.

**Code refs.** `fec/big5_improvements_eval.py`, `fec/b5_residual_eval.py`, `fec/b5_stylometry_eval.py`

**Assumptions**
- Gold panels are independent ratings
- k=1 insider ratings are noisy

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[fouirnaies-2022-electoral-incentives-affect]] | 2022 | qualifies | Roll-call participation moves with electoral incentives inside the same person (-2.70 points in the final term, mean 90.48). As a criterion it therefore mixes trait and electoral situation. | Weak to moderate. The PASS is recorded and must not be re-run (single-look). The paper shows the criterion has a situational component, so any external claim from this look should carry an UNSTRATIFIED label on electoral status. This matters most if campaign-driven absences correlate with interview volume, for example among members running statewide. | weak | VERIFIED |
| [[birkenmaier-2025-measuring-politicians-public]] | 2025 | measurement-source | Three coders (the first author and two RAs) reached Krippendorff alpha of .70 to .78 for agency and communion, and .72 to .73 for detecting any cue. The paper calls this acceptable and sufficient in Sec 3.2.1.3 and 4.1 and moderate in Sec 5. No computational model exceeded macro-F1 0.80. | The paper's alpha is a multilabel agreement measure on sentence cue labels, from three trained coders with a codebook. The single-rater test used 1-9 holistic member ratings. The paper does not call its task easier, and .72 is the presence-detection alpha rather than the agency or communion alpha. | moderate | VERIFIED-SOFTENED |
| [[ramey-2016-measuring-elite-personality]] | 2016 | supports | The paper has no direct validation against ground-truth personality measures for legislators. Its validity rests on ideology correlations (Table 1 Model 1, R2 0.10, N 844), LIWC face validity, NPAT response and bill shares. For NPAT, Emotional Stability moves predicted response from about 30 to about 53 percent between -2 and +2 SD (Table 2 Model 4, N 598). The paper says this suggests selection bias; it does not show it. | Senate panels against a House paper is a cross-chamber assumption. The paper's NPAT argument concerns self-report surveys, not expert ratings, so it does not directly apply to the panels. | moderate | VERIFIED-SOFTENED |
| [[koutsoumpis-2024-kernel-truth-text]] | 2024 | qualifies | Observer-rated (cue utilization, k = 5, n = 1,637, mostly zero-acquaintance raters) explained variance averages 38.52 percent against 5.07 percent for self-report cue validity. Largest observer \|rho\| .39 (Agreeableness swear) and .31 (Extraversion word count). Emotional Stability has the lowest observer R2 at 31.48 percent. The authors say valid or utilized cues are not the same as true personality. | The paper observer samples are k = 5 with n = 1,637, nearly all zero-acquaintance. The Senate panels are informed insiders and experts, which the paper could not test. SOKA predicts the overt traits (E, A) would pass and ES would be weakest, which fits the brain topic. Panel convergence shows raters and text use the same cues, not trait accuracy. | moderate | VERIFIED-SOFTENED |
| [[celli-nd-twenty-years-personality]] |  | measurement-source | Brunswik lens separates Recognition (self-report target) from Perception (observer-attributed traits). Computer Perception convergent validity is Rho 0.30 against 0.38 for human perception (Hinds and Joinson 2024), with a caveat that settings are not fully comparable. The paper recommends correlation coefficients over median splits. | Expert and insider panels are observer judgments, so the Perception figures (0.30 machine, 0.38 human) are the relevant comparison, not the Recognition rows. The paper's figures come from social media, video and smartphone data, not U.S. senators. The claimed pass pattern (E and A pass both panels; O, C, N one each) was not checked against outputs. | moderate | VERIFIED-SOFTENED |
| [[pennebaker-2003-psychological-aspects-natural]] | 2003 | qualifies | In at least two studies (Pennebaker and Francis 1996; Berry et al. 1997), judges' ratings of emotionality correlated more highly with the language of texts and speech than with the writers' or speakers' self-reports. No coefficients are given, and the review raises the gold-standard question. | Observer panels rate senators partly from the same public speech the readers score, so convergence can reflect shared observable cues. That halo mechanism is the analyst's inference, not stated in the review. The evidence is two studies in emotion, not Big Five panels. | weak | VERIFIED-SOFTENED |
| [[lechner-2025-mapping-political-personality]] | 2025 | qualifies | Observer trait ratings of politicians carry rater-ideology bias (proximity r up to .48). Several 4-item facets have low self-rating reliability (alpha .44-.52: Compromise .51, Humility .46, Loyalty .44, Creativity .45), which limits attainable convergent correlations. | PoliPers raters were crowdworkers rating two prominent leaders, not experts rating senators, so the bias evidence is a reason to check panel ratings for partisanship, not proof that they are biased. The limit on convergent correlations is the analyst's inference from reliability. | weak | VERIFIED-SOFTENED |
| [[mondak-nd-online-appendix-personality]] |  | measurement-source | Table A1 gives Senate-insider and APSA-Fellow mean Big Five ratings for 129 senators, feeding main-paper Tables 2-5. Table A2 gives academic ratings for 126 senator name entries (text count), used only in main-paper Table 1. Each row carries a rating count. Observed values run 1.00-7.00 with 0.50 increments noted by the authors. | Senate-only and historic; era unstated. Validates rank order on overlap with current members only, as the code caveat at b5_external_gold_validate.py:209 says. Ratings are as coded by the raters, not self-report. | strong | VERIFIED-SOFTENED |
| [[mondak-nd-online-appendix-personality]] |  | qualifies | Senators rated by only one coder are excluded from all analyses in the main paper but still appear in both tables. Bold and italic top and bottom designations apply only to senators with at least two coders. | Same data and same inclusion rule. The size of the effect depends on how many current-roster matches are k=1 insiders, which was not counted here. Any k>=2 re-run is a new look under single-look discipline, not a free re-score. | strong | VERIFIED |
| [[mondak-nd-online-appendix-personality]] |  | qualifies | The extremes of both panels are party-skewed. Democrats are 62% of the insider highest scorers and 65% of the lowest (main-paper Table 3, as cited). In the academic table 80% of high scores go to Democrats (Angus King counted as Democrat) and 80% of low scores to Republicans. The note calls the academic table A3; it is labelled A2. | Party is a known fault line here. Panel-level correlations should be quoted with party strata or an UNSTRATIFIED label. The suggestion that O passes only against academics because of party is new agent analysis and stays UNVERIFIED (DE-062). The 62/65 and 80/80 figures describe only the extremes. | moderate | VERIFIED-SOFTENED |
| [[soto-2017-next-five-inventory]] | 2017 | measurement-source | The BFI-2 has 15 facets, three per domain. E: Sociability, Assertiveness, Energy Level. A: Compassion, Respectfulness, Trust. C: Organization, Productiveness, Responsibility. N: Anxiety, Depression, Emotional Volatility. O: Intellectual Curiosity, Creative Imagination, Aesthetic Sensitivity. Domain scales have 12 balanced-keyed items; the 60-item form and scoring key are in the Appendix. | This paper does not contain the BFI-2-XS item set. The one-item-per-facet fact and the verbatim check come from the companion short-form paper (Soto and John 2017, JRP), which is not in this item. Three items per domain cannot give facet scores or balanced keying, so the acquiescence results here transfer to the XS panel only weakly. The panel is CANCELED per the memory index, so this bearing is lineage only. | moderate | VERIFIED-SOFTENED |
| [[yang-2023-getting-personal-deep]] | 2023 | context | Validity evidence is agreement with self-report surveys on non-elite authors. User-level Pearson correlations are at least 10 to 18 points above the best comparator and MSE at least 10 percent lower, reported in Online Appendix B and not in the body. No observer, expert or criterion validation is reported, and executive scores are used without survey validation. | The paper's standard is self-report agreement on non-elite authors. It is weaker than and different in kind from the system's observer-panel standard, so its figures cannot substitute for a gold-panel look. | weak | VERIFIED-SOFTENED |
| [[mairesse-2007-automatic-recognition-personality]] | 2007 | supports | On EAR, observer-rated Naive Bayes beats baseline for E 73.00 vs 47.78, ES 73.89 vs 51.11 and C 67.67 vs 47.78. No self-report classifier or regressor significantly beats baseline. Mean inter-observer pair r is 0.54 for E, 0.29 for ES and 0.51 for C (18 observers, 31 to 33 pairs). | The EAR design is verified: observers rated audio only, without knowing the participants, in three groups, and the ratings were averaged (EAR description p.464-465). The Senate known-figure reputation point is outside the paper, and the paper never discusses Senate panels. Linking the ES observer agreement to the system's N gate is the verifier's inference. | moderate | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: moderate, and the criterion itself is qualified.** The hub has 13 rows (11 distinct papers): 2 VERIFIED, 11 VERIFIED-SOFTENED and 0 UNVERIFIED.

**The panels as a criterion**
- Mondak's appendix (strong, VERIFIED-SOFTENED) confirms the panels: 129 insider ratings, and 125 academic ratings in the artifact against 126 in the text. They describe perceived public persona, not self-report, so the Brunswik "Perception" benchmarks are the right comparison: about .30 for machines and .38 for humans (Celli).
- Koutsoumpis predicts the observed pass pattern. Overt traits (E, A) should match observers best and ES worst, and E and A do pass both panels. This counts as a fit to shared cues, not as evidence of trait accuracy.

**Problems with the criterion**
- **k=1 raters** (VERIFIED). Mondak excludes senators rated by a single coder. The primary validators (`b5_gold_multitrait_validate.py`, `b5_external_gold_validate.py`) do not. Any k≥2 rerun is a new look, not a free re-score.
- **Party.** The extremes of both panels are party-skewed (Democrats are 62% of the insiders' highest-rated senators; 80/80 split in the academic table). Lechner shows observer ratings track the rater's ideological proximity, with r up to .48. Neither primary validator includes a party term, so panel correlations should carry party strata or an UNSTRATIFIED label. The idea that O passes only with academics *because of* party is an UNVERIFIED lead.
- **Electoral situation.** The criterion C vs votes_cast (rho +0.155, PASS) mixes trait with electoral situation. Fouirnaies 2022 (VERIFIED) finds a within-person drop of 2.70 points in the final term (mean 90.48, in state legislatures). Quote the PASS with an UNSTRATIFIED label on electoral status, and do not re-run it.
- **Shipped composites.** `b5_profile_basis.py` uses gold-fitted weights, so only the out-of-fold figures are clean evidence.

**Unsourced figures**
The A-ES halo split (about .44 population plus about .21 text tone) is not in the brain topic. The .44 is the RKH anchor (.443), and the .21 attribution was not found.

**Contradictions**
- Mondak excludes senators with a single coder (k=1). The primary validators average or keep them (VERIFIED code-level mismatch).
- build_big5_senate_gold.py silently drops the academic Feingold row (125 records against 126 in the text), and its except-branch comment is wrong.
- The A-ES '.21 text-tone halo' figure was not located. The .44 is the RKH reference anchor, not a system result.
- The panel pass pattern in the component text could not be checked against the brain topic for C. The brain records one-panel support for O, C and N, while the component says C has only an insider association.

**Gaps**
- Party-stratified panel correlations (D and R separately), or an explicit UNSTRATIFIED label on every quoted figure.
- A count of how many current-roster matches are k=1 insiders, so the size of the k=1 issue is known before any new look.
- Any House-side criterion. A study that would fill it: blind expert or staff ratings of a random House sample on BFI-2-S items, collected before any text scoring.

**Paul's position vs the evidence.** Paul highlighted Birkenmaier's agreement figures. The brain ruling of 2026-07-04 that cites them is wrong in two ways. Birkenmaier does not call its task 'easier', and .72 is the alpha for detecting any cue, not for agency or communion. The ruling's use of the figure to set aside the single-rater test therefore rests on a misreading.

**Open questions**
- What share of the matched senators are k=1 insiders?
- Does the E/A pass survive party stratification?
- Should the C-participation PASS be quoted only with an UNSTRATIFIED-on-electoral-status label?

**Read first**
- [[mondak-nd-online-appendix-personality]]
- [[koutsoumpis-2024-kernel-truth-text]]
- [[lechner-2025-mapping-political-personality]]
- [[fouirnaies-2022-electoral-incentives-affect]]
- [[celli-nd-twenty-years-personality]]

## LLM item-level reader (Pink), ridge hybrid and CT-CM latent model

**Computes.** De-identified, chunked LLM ratings on 20 items; ridge hybrid on lexical, interaction and persona features; CT-CM SEM with an LLM method factor.

**Method.** The LLM reader removes names before scoring and codes 'cannot tell' as 0. It is display-only. Pink x behavioural diagonal mean +0.064 < 0.12 fails. CT-CM is a one-look capstone.

**Code refs.** `fec/members/big5_llm.py`, `fec/members/big5_hybrid.py`, `fec/members/big5_ctcm.py`

**Assumptions**
- De-identification removes reputational priors (Cao & Kosinski)
- Judge halo is a single method factor

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[birkenmaier-2025-measuring-politicians-public]] | 2025 | qualifies | LLM prompting with a codebook and few-shot examples classifies defined sentence-level cues. Most sentences carry no cue: 94 percent 'none' for strong cues and 89 percent for weak plus strong. | The paper validates a narrow codebook cue task, not item-level Big Five ratings, so its success does not transfer to the Pink reader. Treating cannot-tell as 0 matches the paper's base rate, but it is the builder's design choice. | weak | VERIFIED-SOFTENED |
| [[rammstedt-2025-going-global-language]] | 2025 | qualifies | Item rewording is treated as a comparability risk: each change request was coded by MQM type and approved only for major issues, 3 requests were rejected as preferential, and mean item change was 10 percent per version pre-Field-Trial (range 0-80) and 6 percent post (range 0-53, 13 of 39 versions). | Holds only as a governance analogy. Translation of self-report items is a different measurement problem from an LLM rater applying observer items to political text. The paper reports no psychometric consequences of its changes, so it cannot calibrate how much rewording matters. The system's itemset versioning already enforces the point. | weak | UNVERIFIED |
| [[celli-nd-twenty-years-personality]] |  | qualifies | Zero-shot GPT-3.5 and GPT-4 infer Big Five from Facebook status updates at average r = 0.29, comparable to specialized ML (Peters and Matz 2024), which is parity rather than superiority. LLM output shows consistent trait profiles (high Agreeableness, high Openness, low Neuroticism), and the paper says these are not psychological states. | The paper's parity result uses users' own casual posts against self-report. The system scores de-identified political text, so a lower figure is expected, and the paper does not support LLM readers beating lexical ones. The Pink x behavioral diagonal (+0.064 against a 0.12 bar, failing) and the display-only status were not checked. | moderate | VERIFIED-SOFTENED |
| [[azucar-2017-predicting-personality-traits]] | 2017 | context | Combining several footprint types gave significant gains for O (beta .27, R2 .16), C (beta .25, R2 .20) and N (beta .21, R2 .14). E showed a trend (beta .18, R2 .12, p=.08). A showed no gain. Text or pictures alone added nothing over other types. | The paper concerns social-media footprint types measured against self-report among lay users. The hybrid adds interaction-channel features that are not footprint types, and its targets are behavioral. The paper gives no basis for expecting a gain from interaction features. In the system's own output the added features gave no held-out gain on any target, so the paper does not support the current hybrid. Any future claim needs a pre-registered look and second-path reproduction (DE-062). | weak | VERIFIED-SOFTENED |
| [[lechner-2025-mapping-political-personality]] | 2025 | supports | Observer trait ratings track the rater's ideological proximity (r=.36-.48 on Integrity, Intellect and Creativity) and global evaluation (facet-likeability r up to .76). The authors read this as raters being less objective depending on where they stand politically, and separate positivity from descriptive normativity. | The paper establishes the human-rater analogue only. That the same bias holds for an LLM's reputational priors is an inference. The CT-CM outcome is a separate matter and is not supported by this paper. | moderate | VERIFIED-SOFTENED |
| [[naz-2025-machine-deep-learning]] | 2025 | context | LLMs are listed as an open challenge and future direction for personality detection, without an evaluation. BERT and GPT are named most effective in the RQ answers. The paper does not discuss identity leakage, reputational priors or halo. | The system's LLM design is already more careful than anything the survey covers (de-identification, observer-keyed items, held-out behavioural criteria). The paper adds no evidence that would move the reader off display-only status. | weak | UNVERIFIED |
| [[soto-2017-next-five-inventory]] | 2017 | qualifies | PCA of the 60 raw items gave six components (Big Five plus acquiescence) in both samples. Within-person centering removed the sixth. The five centered components had primary loadings averaging .62 (internet) and .61 (student), with congruence .97 or higher. The 15 facet scales gave five components above the scree line (eigenvalues 4.05, 2.16, 1.90, 1.60, 1.32, then 0.56 or lower). Averaging raw unreversed items gives an acquiescence index from 1.00 to 5.00. | The acquiescence mechanism carries over only if the LLM shows a +1 tendency on observer items, which is untested. Under the paper's logic a yea-saying tendency lifts O, C, E and A together and leaves ES alone, so it would not create the A-ES correlation by itself. The paper's content balancing controls acquiescence at the scale level, not at the item level. | moderate | VERIFIED-SOFTENED |
| [[yang-2023-getting-personal-deep]] | 2023 | qualifies | On PANDORA BERT-Base reaches AUC 61.5, RoBERTa 63.2 and PersonaBERT 63.4, against DeepPerson 75.0. DeepPerson was pretrained on 800 million tokens vs 3.3 billion (BERT-Base) and 4.1 billion (PersonaBERT); RoBERTa used about 30 billion or more. The paper's ablation credits pseudo-labeling (SPDFiT) with about a 5-point AUC drop when removed. | The paper compares fine-tuned encoder language models (the BERT family) on self-report labels, not zero-shot instruction-tuned LLM item readers. It bears on model size only indirectly, so it cannot confirm the Pink miss. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: weak. Nothing supports moving the LLM reader off display-only status.** The hub has 8 rows (8 distinct papers): 0 VERIFIED, 6 VERIFIED-SOFTENED and 2 UNVERIFIED.

**What the literature says about LLM readers**
- The best general evidence is parity, not superiority. Zero-shot GPT-3.5 and GPT-4 reach mean r .29 against self-report on Facebook posts (Peters & Matz 2024). That copy is now downloaded, and it also reports accuracy differences by sex and age.
- Birkenmaier validates a narrow sentence-cue task, not item-level Big Five ratings, so its success does not carry over to the 'Pink' reader. The system's own result is a behavioural diagonal of +0.064 against a 0.12 bar: a MISS.

**Halo and reputational priors**
- Lechner shows the human analogue: observer ratings track ideological proximity (r .36 to .48) and liking (r up to .76).
- Scan lead (UNVERIFIED): Cao & Kosinski 2024 (Scientific Reports, downloaded). GPT-3 name embeddings alone predict perceived Big Five for 226 public figures at r .78 to .88 (attenuation-corrected). That drops to .53 to .70 after controlling for likability and demographics. This directly supports the assumption that LLMs carry reputational priors about named figures, which justifies de-identification. It also shows likability is a large part of perceived personality, which is a reason to model the halo as a method factor in CT-CM.

**Acquiescence**
Soto 2017 shows yea-saying is a scale-level factor. The reader's keying is 3:1 for O, C, E and A and 2:2 for ES. An all-yes response would lift O, C, E and A and leave ES at 0. That cannot create the A-ES correlation, but it can inflate correlations among the other four traits.

**Hybrid**
The committed `big5_v3_hybrid.json` shows no held-out gain over lexical-only on any target (A .056 vs .059, C .273 vs .281, E .564 vs .563, O .130 vs .135). The interaction features are degraded by unresolved nicknames, so this is a floor. Azucar 2017 gives no reason to expect these features to add signal.

**CT-CM**
No bearing tests CT-CM. It remains a one-look capstone with no literature behind it.

**Contradictions**
- The big5_llm.py docstring says 'balanced keying', but the items are keyed 3:1 for O, C, E and A and 2:2 for ES (VERIFIED-SOFTENED).
- The hybrid ridge shows no held-out gain over lexical-only on any behavioural target in the committed artifact.

**Gaps**
- No human-gold check of the de-identified LLM reader against the Senate panels. A study that would fill it: score panel senators' de-identified text, then compare that to a named-input run to measure reputational leakage, as Cao & Kosinski do from the other direction.
- No estimate of the LLM's acquiescence tendency on observer items. Running a content-free control prompt would measure it.
- CT-CM has no external evidence. Its one look should state in advance what share of variance the method factor must take.

**Paul's position vs the evidence.** Paul's note that GPT-4o and DeepSeek-V3 were the only models validated is accurate for Birkenmaier's sentence-cue task. Reading it as support for LLM *trait* reading is a transfer error. The paper validates cue classification, and the system's own item-level trait reader failed its bar.

**Open questions**
- How much of the Pink reader's variance is a single halo or likability factor once de-identification is applied?
- Does the LLM say 'yes' more often than 'no' on the observer items?
- Is the hybrid worth keeping after the nickname re-ingest, or should it be retired?

**Read first**
- [[celli-nd-twenty-years-personality]]
- [[lechner-2025-mapping-political-personality]]
- [[birkenmaier-2025-measuring-politicians-public]]
- [[soto-2017-next-five-inventory]]
- [[azucar-2017-predicting-personality-traits]]

## Behavioural dispositions

**Computes.** Cooperation, diligence (roll-call participation, LEP), dominance (turn statistics, leadership), visibility (interviews), breadth (portfolio) and reactivity (interview cutoff).

**Method.** z-score composites with coverage floors; missing values are neutral.

**Code refs.** `fec/members/behavioral_dispositions.py:1-120`, `fec/members/interaction_features.py`

**Assumptions**
- Observed behaviour indicates disposition
- Turn-share statistics index dominance (Rienks & Heylen; Valente)

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[fouirnaies-2022-electoral-incentives-affect]] | 2022 | qualifies | Floor-vote participation is state-dependent within the same legislator. It falls 2.70 points (SE 0.53) from a mean of 90.48 in the final term, about a 27% proportional rise in abstention. Committee activity falls about 8% (-0.31 from 3.84). | Moderate. Missing votes is a universal congressional behaviour, and the paper's within-person identification is cleaner than anything the system has. However, the state final-term drop of about 3 points is small next to the cross-member spread in congressional votes_cast. It matters most for members in their final Congress, or those campaigning for another office, whose absences are campaign-driven. The disposition reading holds only after electoral status is held fixed. | moderate | VERIFIED |
| [[ramey-2016-measuring-elite-personality]] | 2016 | contradicts | The 2016 paper's only Conscientiousness outcome is the ceremonial-bill share (Table 3). It has no roll-call participation analysis; roll calls appear only in passing as a source of ideal points (p.5). | The 2016 text does not support the attribution. Whether another RKH work (the 2015 manuscripts cited at pp.15 and 19) uses participation cannot be checked from this text. Treat as a citation fix, not a modelling finding. | moderate | VERIFIED-SOFTENED |
| [[azucar-2017-predicting-personality-traits]] | 2017 | context | Social-media activity statistics (posts, friends, network density, likes received, comments, tags) gave a marginal increase in E accuracy (beta .19, R2 .18, p=.06). The paper reports no other significant moderator effects; it does not report a tested null for each trait. | The constructs differ. The paper's activity variables are social-media posting, friend counts and network density; the system's variable counts press or broadcast interviews. Any link is an analogy, not support, and it bears on E only, not on the lexical E it was meant to contrast with. The analyst's point that interview counts track press-office scheduling and seniority is their inference and was not checked against the repo. | weak | VERIFIED-SOFTENED |
| [[amsalem-2018-effect-politicians-personality]] | 2018 | qualifies | Extraversion is positive and significant in the pooled OLS: b = 0.129 (SE 0.052), beta .117, p < .05 (Model 1, n = 306, R2 = .483, country dummies and controls). Also significant on news websites (Model 3, b = 0.117, p < .05). | The extraversion direction holds in the paper, but the effect is small (beta .117). The paper's outcome is earned mentions by journalists, not interviews a member gives. The sample is MPs in Belgium, Canada and Israel, 2011-2015, with self-reported TIPI. Office is the dominant predictor there, so a US interview count needs office controls before it reads as temperament. Any claim that visibility_z double-counts dominance_z is an untested inference, not a paper finding. | moderate | VERIFIED-SOFTENED |
| [[salam-2022-learning-personalised-models]] | 2022 | context | Among generic single modalities, talk-turn features without sentiment (duration, content, speech rate) average AMSE 0.874 across the five traits, audio 0.920, and FaceBody 1.325 (Table 1, p.65). The paper does not isolate extraversion or dominance and does not test broadcast interviews. | Indirect, five-trait-averaged evidence that turn structure carries trait signal in dyadic lab tasks. It does not validate dominance_z or its committee term, and says nothing about congressional floor speech. Any use should name the window and denominator. | weak | VERIFIED-SOFTENED |
| [[yang-2023-getting-personal-deep]] | 2023 | suggests-extension | The same GBDT and RFR models are run with and without personality features. Average MSE improvement: GBDT 14.29 percent (CH 20.91, SG&A 31.74, ROA 12.52), RFR 6.67 percent. GBDT gains from CNN-1 features 3.38, CNN-2 minus 1.80, BERT 8.62. Gains for all eight DVs are significant by Wilcoxon, GBDT at p below 0.01. Observations per DV range 405 to 519 (Table 10). A chronological split is reported in Online Appendix G. | Borrow the design only: the same learner with and without a trait block, a held-out chronological split, and paired tests, applied to compare text traits against dispositions on a pre-registered responsiveness outcome. The lift magnitudes come from executives and firm accounting outcomes with reverse causality the authors leave unresolved, so they must not be quoted as evidence that text traits predict member behaviour. | moderate | VERIFIED |

**Assessment**

**Evidence weight: moderate on the confounds, none on the predictive claim.** The hub has 6 rows (6 distinct papers): 2 VERIFIED, 4 VERIFIED-SOFTENED and 0 UNVERIFIED.

**Diligence**
- Fouirnaies 2022 (VERIFIED, moderate) shows roll-call participation depends on the legislator's situation. Within the same person it falls 2.70 points (SE 0.53, from a mean of 90.48) in the final term, and committee activity falls about 8%. These are US state legislatures.
- In the system, retiring members get lower diligence_z, and therefore higher susceptibility (weight -0.20), while also getting incent 0.60. Electoral status enters the pressure term twice with opposite signs and is never modelled explicitly.
- The disposition reading holds only after electoral status is held fixed. Transfer is moderate: missing votes is a universal behaviour, but the state effect is small next to the spread across members of Congress.

**Citation error**
The docstring attributes roll-call participation as a C criterion to Ramey 2016. The paper's only C outcome is the share of ceremonial bills (VERIFIED-SOFTENED, 'contradicts'). This is a citation fix, not a modelling finding.

**Visibility**
Amsalem 2018 finds extraversion predicts earned media mentions (beta .117, n 306 MPs in Belgium, Canada and Israel, 2011 to 2015). Office is the dominant predictor there. A US interview count needs office and leadership controls before it reads as temperament.

**Dominance**
Salam 2022 gives only indirect evidence, from dyadic lab tasks averaged over five traits, that turn structure carries trait signal.

**Untested hypothesis**
The headline question of whether dispositions beat text traits on responsiveness (H1/H2) is untested. Yang 2023 offers a reusable design: the same learner with and without the trait block, a chronological holdout and paired tests. Its effect sizes must not be quoted.

**Contradictions**
- The behavioral_dispositions.py docstring credits Ramey (RKH) with roll-call participation as the C criterion. Ramey 2016 has no participation analysis.
- Electoral status enters susceptibility twice with opposite signs: through diligence_z at -0.20 and through incent 0.60.

**Gaps**
- Nothing yet shows that dispositions beat text traits on lobbying responsiveness. Needed: a pre-registered prospective H1/H2 look using Yang's paired with/without design on a chronological split.
- Diligence_z has not been re-estimated with members in their final Congress or seeking other office excluded or stratified.
- visibility_z has not been residualized on office and leadership.

**Paul's position vs the evidence.** Fouirnaies does show effort allocation is observable, but it shows it is situational, which cuts against reading it as a disposition. The other half of Paul's note, that Ramey links conscientiousness to sponsorship, is accurate only for ceremonial-bill share. Ramey does not link it to participation.

**Open questions**
- How many current members are in their final Congress or running statewide, and how much does stratifying on that move diligence_z?
- Should electoral status be modelled once and explicitly in the pressure term?

**Read first**
- [[fouirnaies-2022-electoral-incentives-affect]]
- [[amsalem-2018-effect-politicians-personality]]
- [[ramey-2016-measuring-elite-personality]]
- [[yang-2023-getting-personal-deep]]
- [[salam-2022-learning-personalised-models]]

## Cooperation v2 (share-based bridging)

**Computes.** Mean of within-chamber z of cross-party tie share and directional cross-party cosponsorship share (at least 10 cosponsorships). Reciprocity is reported only.

**Method.** Volume-free by design. Promoted 2026-07-10. Correlates rho .888 with bpf.

**Code refs.** `fec/members/coop_v2_candidate.py:1-130`

**Assumptions**
- Cross-party share measures cooperation independent of volume

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | supports | Average between-party and within-party pairwise distances (Figs 7-8, 1879-1998) show polarization declining to WWII, stable until the late 1970s, and rising steadily since. The paper does not state that the most extreme members sit farthest from the opposing party; that is an analyst inference. | The paper supplies context for why an extremity control matters in a polarized chamber; it does not test the cooperation measure. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: thin, close to a gap.** The hub has 1 rows (1 distinct papers): 0 VERIFIED, 1 VERIFIED-SOFTENED and 0 UNVERIFIED. Poole 1999 gives context for why an extremity control matters in a polarized chamber: cooperation_v2_z vs |dim1| is -0.250 for n = 532 of 533 members (2026-07-10). It does not test the measure.

**Scan lead: Harbridge-Yong, Volden & Wiseman (JOP 2023; working-paper copy downloaded)**
- Members of the 93rd to 114th Congresses (1973 to 2016) who attract a larger *share* of their cosponsors from the other party are more effective lawmakers. The result holds for majority and minority members and over time.
- This is the closest published construct to cooperation_v2's share-based bridging, and it gives predictive-validity context. Two transfer notes:
  - Their share counts cosponsors *attracted* to the member's own bills. The system's directional share needs to be checked for whether it counts attracted or offered cosponsorships.
  - They identify reciprocity as the path to attracting cross-party cosponsors. Reciprocity is NOT validated in this project (D-104..D-108), and cooperation_v2 rightly reports it only. Do not import the paper's mechanism.

**Redundancy**
cooperation_v2 correlates rho .888 with bpf, so it is nearly redundant with that existing measure. Its separate value is unshown.

**Normalization flag**
The measure is volume-free by design, using shares. That is a member-level share, not per-edge volume normalization of the cosponsorship graph, so it does not reopen the rejected SNA proposal. Any paper that argues for per-edge normalization should be flagged, not recommended.

**Contradictions**
- (none)

**Gaps**
- No test-retest evidence. Needed: the intraclass correlation (ICC) of a member's cross-party cosponsorship share in consecutive Congresses, stratified by majority/minority status and chamber, with a stated window (for example the 110th to 118th Congresses) and a minimum of 10 cosponsorships per Congress.
- No criterion validity. Test whether cooperation_v2 predicts out-of-sample cross-party co-voting or LES-style success, with party and |dim1| strata.
- It is unclear whether the 'directional' share is attracted or offered. Harbridge-Yong's predictive result applies to the attracted share.

**Paul's position vs the evidence.** Paul has no annotation on this component. The citations it uses (Bacik 2026, Aref & Neal 2021) have no bearings in the hub, so the citation base is effectively empty. This is a gap, not an error.

**Open questions**
- Is cooperation_v2 stable across Congresses (trait-like) or does it track majority status?
- Given rho .888, what does cooperation_v2 add over bpf?
- Does the directional share measure attracted or offered cross-party cosponsorship?

**Read first**
- [[poole-1999-nominate-after-years]]

## Affective neuroticism slot and N genre-transfer tests

**Computes.** affective_n = z(LIWC i + negemo - posemo) + z(LLM pessimism, anxiety, depression, volatility), residualized on E and party.

**Method.** Gate 5/5 passed: |r(N,E)| .039; party .000; LIWC-LLM convergence +.236; split-half SB .742. Promoted 2026-07-11. Genre transfer of interview N to floor and hearing text FAILED (rho .050, .161).

**Code refs.** `fec/members/emit_affective_n.py`, `fec/members/affective_n_validate.py`

**Assumptions**
- Negative-emotion and first-person-singular words track N (Pennebaker)
- No gold N criterion exists, so the gate tests construct integrity only

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[ramey-2016-measuring-elite-personality]] | 2016 | qualifies | Emotional Stability's link to anxiety words is somewhat weaker than those for Openness, Conscientiousness and Extraversion (p.11). Agreeableness is also described as weaker (p.11). ES predicts survey response (Table 2) and loads 0.20 (p<0.01) on Common Space in Table 1 Model 1. | The paper's weak ES-anxiety link is a face-validity remark, not a ranking of ES as weakest. Party-residualizing may remove ES signal that the paper's ideology check carries. | weak | VERIFIED-SOFTENED |
| [[marrero-2022-evaluating-voice-samples]] | 2022 | qualifies | Neuroticism was predicted better by linguistic than by vocal features (out-of-sample R-squared about 0.15, about 0.56 after disattenuation; Table 3 best-model rows). Table 2 lists LIWC top-5 features for N as function words, anx, Tone, Analytic and negemo; the text names no single top LIWC feature for N, and first-person singular is not in the N top five. | The paper is student self-report with about 92-word samples; the system measure is an LLM affective-facet score on legislator text. The paper supports text over voice for N only in its own sample. Its top N predictors differ from the system anchor terms (i, negemo, posemo), so the paper does not validate the anchor index. | weak | VERIFIED-SOFTENED |
| [[byrne-2014-chokes-under-pressure]] | 2014 | qualifies | The paper explains N-driven choking by working memory: pressure and anxiety use up working memory, so neurotic people take the immediate reward. The authors measured no anxiety, mood or working-memory capacity and ran no pressure manipulation check. Self-rated importance did not differ by condition (Low M = 4.68, SD 2.42; High M = 5.17, SD 2.14; ns). | Byrne gives a theoretical reason to keep an N slot but no evidence that text-measured affective content captures the anxiety facet the paper invokes. The paper's mechanism was never measured, so a null in legislators neither confirms nor refutes Byrne. The N contribution keeps its UNVERIFIED label (DE-062). | moderate | VERIFIED |
| [[koutsoumpis-2024-kernel-truth-text]] | 2024 | supports | Emotional Stability relates negatively to negative-emotion words (cue validity rho -.12, p. 853) and to first-person singular pronouns (cue utilization rho -.29). Emotional Stability carries 6 of the 20 kernel markers (p. 853). The Emotional Stability negative-emotion validity effect weakens with publication year (Table 6, b = .006). No positive-emotion effect for Emotional Stability is reported. | The paper supports the sign structure for Emotional Stability only, and it reports heterogeneity by mode and formality. An expected anchor size near .24 is the analyst's inference, not a paper result. This is direction support only, not evidence that N predicts yielding. | weak | VERIFIED-SOFTENED |
| [[pennebaker-2003-psychological-aspects-natural]] | 2003 | method-source | Neuroticism goes with more negative-emotion words, fewer positive-emotion words and more first-person singular. Big Five self-report correlations with word use are r = 0.10 to 0.16 in several hundred college students (Pennebaker and King 1999). Depressed students (Rude et al. 2002) used more I, with me, my and mine comparable. Emotion words are under 5 percent of words in speech, writing and poetry. | Paper evidence is student self-reports, student essays and student conversation, not media-trained legislators speaking on the floor or in interviews. The Rude et al. sample is currently versus never depressed students writing personal essays, not a clinical sample as the analyst wrote. Untested for N in politicians. | moderate | VERIFIED-SOFTENED |
| [[azucar-2017-predicting-personality-traits]] | 2017 | qualifies | Demographics (age, gender) as a moderator raised accuracy for N (beta .25, p<.05, R2 .19) and A (beta .25, R2 .19). O showed a marginal gain (beta .26, R2 .12, p=.09). | The paper's demographic moderator is between-study and univariate: whether a study used demographic features, with no adjustment. It does not show an individual-level mechanism. The paper says its demographic finding is consistent with survey literature on demographic differences in self-reported traits (Goldberg 1998; Soto 2011). The analyst's claim that the gain comes from group membership correlating with self-reported traits, a predictive shortcut, goes beyond the paper. Do not adopt demographic predictors on the strength of this paper. | moderate | VERIFIED-SOFTENED |
| [[amsalem-2018-effect-politicians-personality]] | 2018 | suggests-extension | Net of office, activity, Facebook output and controls, MPs who are more neurotic (beta -.111) and more agreeable (beta -.116) appear less in national news (Table 1 Model 1). The authors attribute this to temperament, not to interview selection, which they do not test. | The paper finds airtime selects on temperament among MPs, which would make interview-derived N range-restricted. Whether US interview coverage selects this way is untested. The paper concerns earned news coverage, not interviews a member gives. Test: correlate interview coverage with non-text proxies such as reactivity_z after chamber-leadership controls. The affective N is also residualized on text-E and party, which the paper does not discuss. | moderate | VERIFIED |
| [[rice-2018-corpus-based-dictionaries]] | 2018 | context | LIWC trails the domain-built measure on benchmark accuracy (69.4% vs 80.4% in-sample; 69.7% vs 79.5% held-out). Its unit-level ranks diverge: Burton ranks 1st in the corpus measure and 50th of 84 under LIWC. | The paper measures document polarity in movie reviews and court opinions, not a trait from interview transcripts. Only the general point transfers: closed-vocabulary counts can misfire outside their home register. The paper says nothing about personality or speech. | weak | VERIFIED-SOFTENED |
| [[naz-2025-machine-deep-learning]] | 2025 | supports | Text personality datasets lack a direct measure of emotional stability or neuroticism (Sec 1, p.2). The survey's experiment reports only Extraversion, Conscientiousness, Agreeableness and Openness, with no N/ES result. | This is a statement about the field, not a test. It confirms that the literature offers no ready-made N criterion the system has missed. It does not validate the system's affective-N construct. It applies regardless of population, because the gap concerns the datasets themselves. | weak | UNVERIFIED |
| [[soto-2017-next-five-inventory]] | 2017 | suggests-extension | Negative Emotionality facets predict different criteria. Peer-rated stress resistance is predicted by Emotional Volatility (r -.47, beta -.32) and Anxiety (r -.46, beta -.28); positive relations by Depression (r -.53, beta -.38). Facets average R2 .33 against .27 for domains across 20 criteria, and BFI-2 facets add significant variance over BFI domains for 14 of 20 criteria. Predictive utility depends more on facet quality than on facet count. | The pressure mechanism fits Anxiety and Volatility more closely than Depression or pessimism. Keeping those components apart is a later measurement question that would need its own prereg and look. Transfer is weak because the paper's criteria are general-population self-report and peer ratings, not legislators' decisions. The genre-transfer rho values and the H5 null were not checked here. | weak | VERIFIED-SOFTENED |
| [[celli-2012-role-emotional-stability]] | 2012 | qualifies | Validation against Analyzewords (a LIWC-based proxy, worried = neurotic, upbeat = secure) on 18 users: neurotic P 0.8 / R 0.615 / F1 0.695; secure F1 0.462; average F1 0.578. The authors concede the proxy does not measure the Big Five and that upbeat does not equal secure. | Celli's validation is circular: a LIWC-family feature set scored against a LIWC-family proxy, so agreement partly reflects shared method. The system's LIWC anchor is the same kind of convergence. The paper therefore reinforces the system's own caveat (gate tests construct integrity, not validity) and adds no criterion evidence. It does not carry over as support for N validity. | weak | UNVERIFIED |
| [[yang-2023-getting-personal-deep]] | 2023 | supports | The authors restrict their evaluation to social posts, forum messages and essays, and say speech transcripts and written articles may warrant exploration. The downstream case applies a model fine-tuned on Facebook labels to executives' tweets, and the text reports no executive-level survey validation. | Support is for caution only. The paper runs no transfer test of its own, so it neither confirms nor explains the system's genre-transfer result. | weak | VERIFIED-SOFTENED |
| [[mairesse-2007-automatic-recognition-personality]] | 2007 | supports | In conversation only a high word count and a low mean pitch mark ES in both self and observer reports. Observed ES tracks swearing and anger words but self-assessed ES does not. Self-references and negative-emotion words mark neuroticism in essays, citing Pennebaker and King. | The paper compares essays and student conversation, not political genres, so the link to floor speech is an analogy. The genre-transfer rho values (.050, .161) are not checked here and are not in the paper. | weak | VERIFIED |

**Assessment**

**Evidence weight: weak to moderate on direction, none on the yielding mechanism.** The hub has 13 rows (13 distinct papers): 3 VERIFIED, 8 VERIFIED-SOFTENED and 2 UNVERIFIED.

**Direction of the lexical signs**
Pennebaker, Koutsoumpis and Mairesse support the signs of the lexical anchor: N goes with more negative emotion, fewer positive-emotion words and more first-person singular. The sizes are small: cue validity of ES with negative emotion is rho -.12 (self-report), and with first-person singular -.29 (observer utilization). The evidence comes from students and social media.

**What the emitted score actually is**
Verified code rows show `affective_n` is now the E-residualized, party-controlled sum of four LLM facets (pessimism, anxiety, depression, volatility). LIWC is kept only as a convergent anchor (r about +.24). The component text 'z(LIWC) + z(LLM)' and the docstring are stale.

**Mechanism**
- Byrne 2014 (VERIFIED) supplies the theory for why N should predict yielding under pressure, but it never measured anxiety, mood or working memory. The project's H5 test of N × loss domain was null (partial r +0.036, n 441). The N contribution stays UNVERIFIED.
- Soto 2017: the pressure mechanism fits Anxiety and Volatility, not Depression or pessimism. Pooling all four facets may dilute it. Separating them would need its own pre-registration.

**Why genre transfer failed (scan leads, UNVERIFIED)**
The failed transfer of interview N to floor and hearing text (rho .050 and .161) has a ready explanation: in legislative speech, affect follows the audience and the context, not the person.
- Osnabrügge, Hobolt & Rodon 2021 (APSR; about 2 million UK and Irish speeches; downloaded): emotive rhetoric rises in high-audience debates such as Prime Minister's Questions.
- Rheault et al. 2016 (PLoS ONE; a century of House of Commons debates; downloaded): emotional polarity tracks the national economy.
- Aroyehun 2026 (arXiv preprint; Congress 1973 to 2024; downloaded): emotion varies by policy domain and ideology.

These read as evidence that affective words in public speech index situation and strategy, which contradicts reading them as a stable trait outside the interview genre. The score is residualized on party but not on ideology within party, nor on topic or period.

**Selection**
Amsalem 2018 (VERIFIED) finds more neurotic MPs appear less in national news (beta -.111). If US interview coverage selects the same way, interview-derived N is range-restricted.

**Contradictions**
- Osnabrügge 2021 and Rheault 2016 (scan leads, UNVERIFIED): affect in parliamentary speech varies with audience and economic conditions. This works against treating affective word use as a trait across genres, and it fits the system's own failed genre transfer.
- The component description (z(LIWC) + z(LLM)) and the emit_affective_n.py docstring are stale. LIWC is now only a convergent anchor, not a component.
- The genre-transfer values (.050, .161) were not located at the cited code lines.

**Gaps**
- No criterion for N. The literature confirms none exists for elites (Naz 2025). Needed: an observed-behaviour test of the yielding mechanism. For example, pre-register whether high-N members shift roll-call position more after a primary threat or a negative press shock, using a within-member design.
- No residualization on topic, ideology or period. Needed: re-estimate affective N within policy-topic × Congress cells and check whether the member ranking survives.
- No check of selection into interviews (Amsalem). Correlate interview coverage with reactivity_z after leadership controls.

**Paul's position vs the evidence.** Paul has no annotation. This component cites only Ramey, whose ES remark is about face validity, not a ranking of ES. No error.

**Open questions**
- Is the member-level variance in affective N mostly topic, audience and period, as the parliamentary-emotion literature suggests?
- Should the anxiety and volatility facets be separated from depression and pessimism for the Byrne channel? This would need a new prereg.
- Does party residualization remove real ES signal (Ramey's ideology loading of 0.20)?

**Read first**
- [[koutsoumpis-2024-kernel-truth-text]]
- [[byrne-2014-chokes-under-pressure]]
- [[amsalem-2018-effect-politicians-personality]]
- [[soto-2017-next-five-inventory]]
- [[pennebaker-2003-psychological-aspects-natural]]

## Agency-communion persona channel

**Computes.** persona_z = 0.60 z(communion) - 0.40 z(agency) from LLM few-shot cue classification (strong 1.0, weak 0.5), blended at PERSONA_W 0.30.

**Method.** Functional gate 18/18. Party signature: communion D .099 vs R .049. Persona homophily is not earned (|r| <= 0.015).

**Code refs.** `fec/members/agency_communion_llm.py`, `fec/persona_homophily_validate.py`

**Assumptions**
- Projected persona is what a lobbyist meets
- Communal rhetoric signals approachability

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[birkenmaier-2025-measuring-politicians-public]] | 2025 | method-source | Few-shot prompting with a codebook of strong and weak agency and communion cues, positive poles only, reaches macro-F1 0.77 (DeepSeek-V3) and 0.76 (GPT-4o) on strong-balanced human labels, the two top configurations. The paper says only these two models pass the functional test units, on strong labels. | The paper's macro-F1 figures belong to GPT-4o and DeepSeek-V3 on German sentences scored against three trained coders. The system has no human-labelled sentence test, so its macro-F1 is unknown. The 18 units were written by the builder in English; the paper's Appendix J was not available. Only the design and the functional gate transfer. | strong | VERIFIED-SOFTENED |
| [[birkenmaier-2025-measuring-politicians-public]] | 2025 | qualifies | Models told to detect only strong cues perform better than strong-plus-weak definitions, as expected. Table 2 gives DeepSeek-V3 few-shot balanced 0.77 (strong, SD 0.02) against 0.76 (weak plus strong, SD 0.01). No delta is reported. | The 0.01 gap sits inside the reported SDs and is a single configuration, so the paper supports a direction, not a magnitude. A strong-only score could be computed from the existing cache at no API cost to test whether member ordering changes. | moderate | VERIFIED-SOFTENED |
| [[birkenmaier-2025-measuring-politicians-public]] | 2025 | supports | Party-level communion share falls from left to right on CHES: LINKE 19 percent, Greens 12, SPD 10, CDU 8, AfD and FDP about 6. The paper shows regression lines and standard-error bars but gives no coefficients, N or intervals in the main text. | Six German parties against a two-party US Congress, with sentence-level shares against member-level scores. The paper gives no pooled coefficient to compare magnitudes. The gap supports convergent validity but also means the persona score carries party, so any pooled use is UNSTRATIFIED across D and R. | moderate | VERIFIED |
| [[byrne-2014-chokes-under-pressure]] | 2014 | qualifies | The agreeableness result rests on self-report BFI agreeableness predicting worse task performance. It is exploratory and significant only in the uncontrolled Exp 2 (b = .36, p < .01). The paper has nothing on projected or communal rhetoric. | Byrne concerns self-reported trait A and says nothing about communal rhetoric in public statements. Citing Byrne for the persona direction is a second inferential step the paper does not cover. PERSONA_W is owner policy; this note is about provenance only. | weak | VERIFIED |
| [[palm-2025-influence-personality-traits]] | 2025 | qualifies | Agreeableness is the most susceptible trait, which fits communal rhetoric as a sign of being approachable. Extraversion is above average in susceptibility, with liking and commitment working best, which runs against treating assertive or agentic presentation as resistance. | Agency and communion are not Big Five dimensions, and mapping agency onto extraversion is loose. The positive communion sign gets mild support; the negative agency sign gets none. PERSONA_W stays, per the owner's ruling, and this bearing concerns only the signs inside the channel. | weak | UNVERIFIED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | qualifies | Few-shot LLM classification can match or beat crowd and expert coding, but results depend heavily on prompt design and need checks against hand labels the prompt was not tuned on. | The paper validates document-level measures against expert and crowd codes; the cues here are sentence-level and aggregated per member. A paper-style check would need a hand-coded cue sample that was not used to write the codebook. PERSONA_W 0.30 is an owner ruling and is not changed by this review. | weak | VERIFIED-SOFTENED |
| [[selfhout-2010-emerging-late-adolescent]] | 2010 | qualifies | Trait similarity is the strongest selection effect: openness 1.45, extraversion 1.46, agreeableness 1.55 (all p<.001), against ego and alter effects of .01 to .14. It is estimated jointly with outdegree, reciprocity, transitivity, distance-2 and sex, and it varies by group: significant in 7/10, 8/10 and 10/10 groups. Overall profile similarity was not modelled here; the authors' 2009 paper on the same sample found it did not predict friendship intensity. | Transfer is weak. The paper's similarity effect appears while self-rated strangers choose friends in the first months, with network structure held fixed. Cosponsorship among sitting legislators is an established, party- and committee-driven tie, not a choice among strangers. The paper does not overturn the null and is no reason to re-propose homophily edge weighting. Its one transferable point: a cross-sectional correlation with no reciprocity or closure controls, using a combined two-trait distance, is a different test from trait-specific similarity estimated inside a selection model. The null therefore holds for the test that was run, not for homophily in general. Any SIENA rerun would be new analysis and would stay UNVERIFIED (DE-062). | weak | UNVERIFIED |
| [[lechner-2025-mapping-political-personality]] | 2025 | method-source | PoliPers defines 15 facets. Table 1 maps Assertiveness & Will to take control to AG: Assertiveness and Empathy & Compassion to COM: Warmth, and gives each facet a named opposite pole (Empathy's is Apathy & Indifference). | The facet definitions transfer as construct definitions. The psychometric evidence does not: it comes from German survey ratings of traits, not from LLM coding of English congressional sentences. | strong | VERIFIED-SOFTENED |
| [[lechner-2025-mapping-political-personality]] | 2025 | qualifies | The Assertiveness facet is not unidimensional. In ideal-politician ratings (Study 3, N=589) it gave two factors because the 'power-hungry, claims leadership' item loaded reversed, and its alpha was not computed (Table A4). In observer ratings of Trump and Merkel (pooled, Study 4, N=211) its alpha was .40 and omega .48 (Table A7). In ideal ratings Assertiveness was the only facet that differed by politician gender (d=.19, p=.025). | The split was measured in citizen ratings of traits, not in congressional sentences. Whether power-claiming and leadership-taking cues split in text is untested, so this is a plausibility warning, not a measured defect. | moderate | VERIFIED |
| [[rubio-2024-feasibility-data-analytics]] | 2024 | context | Strongest voice correlates came from zero-acquaintance expert ratings made from video plus audio (E 0.48, N -0.47, C 0.44), not from self-report. The authors read this as judges estimating purely from video and audio evidence. Expert Openness ICC was 0.23 on 20 videos. | Loosely supports presenting audio as perceived persona rather than trait. Two raters, 20 ICC videos and an all-female student sample give no validation of any Melange persona weight. PERSONA_W remains an owner ruling and is not tested by this paper. | weak | VERIFIED-SOFTENED |
| [[celli-2012-role-emotional-stability]] | 2012 | contradicts | Users tend to converse with others who share their emotional-stability trait (trait homophily). This is reported as a frequency pattern in Fig. 4, with no coefficient and no test. | The trait differs (ES vs agency/communion), and so does the population (2011 Twitter users vs members of Congress). The relation (@-conversation) differs from cosponsorship, and Celli gives no inferential test. The system's measured null on its own data outranks this. Do not cite Celli as grounds to reopen homophily routing. | weak | UNVERIFIED |

**Assessment**

**Evidence weight: moderate for the instrument design, none for the receptivity direction.** The hub has 11 rows (8 distinct papers): 3 VERIFIED, 5 VERIFIED-SOFTENED and 3 UNVERIFIED.

**Instrument design**
- Birkenmaier 2025 (strong method source) validates the codebook design. Few-shot macro-F1 is 0.77 for DeepSeek-V3 and 0.76 for GPT-4o, scored against three trained coders on German sentences. Lechner 2025 supplies the facet definitions.
- The system runs claude-sonnet-5 against English congressional text. It has no human-labelled sentence test, so its macro-F1 is unknown. The functional gate (18/18) was written by the builder. Ornstein 2024 asks for checks against hand labels the prompt was not tuned on, and that check is missing.

**Construct issues**
- Lechner (VERIFIED) finds the Assertiveness facet is not unidimensional. The power-claiming item loads reversed, and in observer ratings alpha is .40. The self-test uses a power-claiming sentence as gold agency. This is a plausibility warning, not a measured defect.
- The party signature (communion D .099 vs R .049) matches Birkenmaier's left-right gradient across six German parties. It also means persona carries party, so any pooled use is UNSTRATIFIED across D and R.
- Scan lead (UNVERIFIED): Hargrave & Blumenau 2022 (UK MPs 1997 to 2019; downloaded) show gendered style shifting over time, with women adopting more agentic styles. Persona_z likely also carries sex and era, so it needs sex strata or an UNSTRATIFIED label.

**Receptivity direction**
- The positive communion sign has only mild support (Palm 2025, UNVERIFIED: agreeableness is the most susceptible trait).
- The negative agency sign has none. Palm finds extraversion also above average in susceptibility.
- Byrne concerns self-reported trait agreeableness, not rhetoric, so citing it here takes a second inferential step the paper does not cover.
- Scan lead: Decter-Frain & Frimer 2016 (House floor 1996 to 2014; downloaded) find only the 'helping' form of communion relates to public approval, and only in an aggregate time series. The relevant point is construct-level: communion splits into belonging and helping. It is not member-level evidence.
- PERSONA_W 0.30 is owner policy and is not in question here.

**Homophily**
The homophily null (|r| ≤ 0.015) stands. Selfhout 2010 and Celli 2012 (both UNVERIFIED, different relations and populations) give no grounds to reopen persona-homophily edge weighting. The null covers the cross-sectional test that was run, not homophily in general.

**Contradictions**
- Byrne 2014 is cited for the persona direction but covers only self-reported trait agreeableness, not communal rhetoric.
- Palm 2025 (UNVERIFIED) finds extraversion is also above average in susceptibility, which works against the -0.40 agency sign.

**Gaps**
- No human-gold accuracy on congressional sentences. Needed: 300 to 500 English sentences from the Congressional Record, coded blind by 2 to 3 coders who did not write the prompt, reporting Krippendorff alpha and model macro-F1.
- No behavioural test of the receptivity direction. Needed: a pre-registered test of whether high-communion members accept more outside meetings or cosponsor more bills from outside requesters, stratified by party.
- Nobody has checked whether member rankings move under strong-only scoring. It can be computed from the existing cache at no API cost.

**Paul's position vs the evidence.** Paul's notes on Birkenmaier (pre-built codebooks; turning a trait into a node is hard; ambiguous statements are hard) agree with the evidence. His 'weaknesses' note is borne out: the paper validates sentence cues, and turning those into a member-level receptivity weight is not covered by it. No error.

**Open questions**
- Does persona_z carry sex and era as well as party?
- Does the agency score mix leadership-taking with power-claiming cues in congressional text?
- What is the receptivity sign of agency once communion and party are held fixed?

**Read first**
- [[birkenmaier-2025-measuring-politicians-public]]
- [[lechner-2025-mapping-political-personality]]
- [[ornstein-2024-train-your-stochastic]]
- [[palm-2025-influence-personality-traits]]
- [[byrne-2014-chokes-under-pressure]]

## Interview weighting (C_INT_W) and gated shrinkage (P6)

**Computes.** Conscientiousness aggregates interview chunks at 2x. Members with fewer than 8 measurements are shrunk.

**Method.** The P6 reliability claim (.748 -> .831) was retracted as double-shrink inflation; the change was kept on external criterion gains.

**Code refs.** `big5_llm.py (C_INT_W)`, `fec/big5_improvements_eval.py --p6`

**Assumptions**
- Shrinkage is judged only on external criteria, not split-half reliability

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment**

**GAP: no bearings.** No paper in the hub addresses C_INT_W (interview chunks weighted 2x for C) or P6 (shrinkage below 8 measurements).

Two items elsewhere in the hub bear on it indirectly.
- Ramey 2016 (VERIFIED-SOFTENED, filed under lexical-big5) ties precision to text volume: a CI width of about 0.3 or less above about 5,000 words per member-year. That is far above the lexical 1,500-token floor, which suggests thin-corpus shrinkage matters more than the system's threshold assumes.
- The project's own retraction stands. The P6 reliability gain (.748 → .831) was double-shrink inflation, and the change was kept on external-criterion gains only. That is the right standard, since shrinkage toward the mean mechanically raises split-half agreement.

Shrinkage itself is standard empirical-Bayes practice and needs no literature. What is missing is evidence specific to this system. The 2x interview weight has no basis. The genre literature (Osnabrügge 2021; the system's own failed N transfer) suggests interview and floor text measure different things, so the weight should be judged by criterion validity per genre, not assumed.

**Contradictions**
- (none)

**Gaps**
- Whether P6 improves criterion validity needs its own pre-registered test. Compare shrunk and unshrunk scores against k≥2 gold-panel senators and against votes_cast, with electoral-status strata, reporting n per token bin.
- No reliability-by-volume curve. Needed: split-half reliability by token bin (for example under 1.5k, 1.5–5k and over 5k tokens per member-Congress) to set the shrinkage threshold from data.
- C_INT_W 2x has no basis. Needed: genre-specific criterion validity for C (interview-only vs floor-only C against the same criterion) before choosing a weight.

**Paul's position vs the evidence.** Paul's position is 'no literature', and that is correct. The component needs its own validation study more than citations.

**Open questions**
- What is the empirical reliability of C at 1,500, 5,000 and 11,000 tokens on this corpus?
- Does the 2x interview weight beat equal weighting on any external criterion?

**Read first**
- [[ramey-2016-measuring-elite-personality]]
- [[mondak-nd-online-appendix-personality]]
- [[marrero-2022-evaluating-voice-samples]]

## Across components

**Patterns across components**

1. **Shared method props up convergence.** Several components support each other with criteria built from the same method:
   - the lexical traits are refit to RKH, which is itself Mairesse-type text scoring;
   - the affective-N LIWC anchor converges with LIWC-family scores (the same circularity Celli 2012 shows);
   - persona and the gold panels share public-persona cues with the text.

   Only the Senate gold panels and behavioural criteria (votes_cast, LEP) are independent, and both carry confounds: party skew and k=1 raters for the panels, electoral situation for votes_cast.
2. **Genre and situation, not trait.** The failed N transfer (interview to floor, rho .050), Pennebaker's 4-week retest of .24 for psychological-process words, and the scan leads (Osnabrügge, Rheault, Aroyehun) all suggest that affect and style in public speech follow audience, topic, period and electoral position. This bears on lexical-big5 (genre pooling), affective-n, C_INT_W (the 2x interview weight) and diligence (Fouirnaies). One MTMM study with genre as the method would answer it for all of them.
3. **Party and sex are fault lines that are not stratified anywhere.** These include the panel extremes (Mondak), the communion signature (D .099 vs R .049), rater-ideology bias (Lechner) and gendered style (Hargrave & Blumenau; Peters & Matz on accuracy by sex). Every pooled coefficient should carry strata or an UNSTRATIFIED label.
4. **Stale system descriptions.** The component texts disagree with the code and artifacts the verifiers read: the CV-against-RKH figures, active weights, the O party reversal, the composition of affective N, LLM keying, the Ramey participation citation and the gold-panel row count. Several of these are actual errors (see the overview). Fixing the descriptions comes before any further literature work.
5. **Validation stance.** Behavioural dispositions replaced lexical traits for receptivity because lexical C reversed on participation (r_alerting -0.76). The evidence supports that move. It also means the remaining text-trait components (LLM reader, hybrid, persona) carry weight only if they beat dispositions on a pre-registered responsiveness look, which has not happened.
6. **Rejected SNA proposals.** None of the bearings argues for dropping regular cosponsorship edges or for per-edge volume normalization. cooperation_v2's member-level share is not per-edge normalization. The homophily null stands, and Selfhout and Celli are not grounds to reopen it.

## Research agenda

1. Fix the actual errors first: the Ramey participation citation in behavioral_dispositions.py, the 'balanced keying' claim in big5_llm.py, the silent Feingold drop and wrong comment in build_big5_senate_gold.py, the stale LIWC wording in emit_affective_n.py, and the brain ruling's misreading of Birkenmaier's alpha. Then re-source or retract the CV-against-RKH figures and the O party-reversal claim in the component descriptions.
2. Count k=1 insider matches on the current roster and report party-stratified panel correlations from the existing out-of-fold outputs (a description, not a new look). Any k≥2 or party-adjusted re-score is a new single look and needs a prereg.
3. Genre MTMM study: traits × genres (floor, interview, hearing, social) on the same members. This one design answers lexical genre pooling, affective-N transfer and the C_INT_W 2x weight.
4. Affective N: re-estimate within policy-topic × Congress cells and residualize on within-party ideology, following the Osnabrügge, Rheault and Aroyehun leads. Pre-register a within-member test of the yielding mechanism (behaviour after a primary-threat or press shock).
5. Cooperation_v2: compute test-retest ICC across consecutive Congresses by majority status and chamber. Settle whether the directional share is attracted or offered, and check its added value over bpf (rho .888). Do not import Harbridge-Yong's reciprocity mechanism (D-104..D-108).
6. Persona: hand-code 300 to 500 Congressional Record sentences blind to the prompt for agency and communion, with Krippendorff alpha and model macro-F1. Compute strong-only scores from the cache. Report persona_z by party and sex.
7. LLM reader: run a named vs de-identified comparison on panel senators to measure reputational leakage (Cao & Kosinski lead), and a content-free prompt to measure acquiescence.
8. Shrinkage: build a split-half reliability-by-token-bin curve on this corpus, then pre-register a look of shrunk vs unshrunk scores against k≥2 panel senators.
9. Ingest the eight scan downloads into papers/ with UNVERIFIED status and an adversarial verification pass. Fetch Cao & Kosinski 2024 (PNAS Nexus) and Park et al. 2015 (JPSP) manually; scripted fetch is blocked or paywalled.
