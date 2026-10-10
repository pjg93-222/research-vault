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
paper_count: 36
tags: [system, type/system-hub]
---

# Melange member psychometrics (Big Five, dispositions, persona)

Measuring legislator personality and behavioural dispositions from text and behaviour. Covers the lexical Big Five pipeline (Ramey-style, with SMOreg and RKH calibration), the LLM item-level reader, the ridge hybrid, the CT-CM latent model, behavioural dispositions, cooperation_v2, affective N, the agency/communion persona channel, shrinkage rules, and the genre-transfer tests. These feed the receptivity layer.

Brain notes: [topics/2026-07-feature-personality-psychometrics](obsidian://open?vault=brain&file=topics%2F2026-07-feature-personality-psychometrics); [topics/2026-07-feature-personality-psychometrics-external-criteria-and-open-a-search](obsidian://open?vault=brain&file=topics%2F2026-07-feature-personality-psychometrics-external-criteria-and-open-a-search); [concepts/single-look-holdout-discipline-case-1-moneyball-gate2](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline-case-1-moneyball-gate2); [concepts/single-look-holdout-discipline-cases-2-3-n-slot-and-gold-template](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline-cases-2-3-n-slot-and-gold-template)

## Melange member psychometrics: evidence assessment

**Basis.** This assessment rests on 95 bearings (36 distinct papers): 14 VERIFIED, 69 VERIFIED-SOFTENED, 12 UNVERIFIED. I treat VERIFIED and VERIFIED-SOFTENED bearings as evidence. UNVERIFIED bearings are single-agent leads, not findings (DE-062).

**Overall verdict.** The literature supports the system's *caution* more than its *instruments*.
- **Lexical channel.** The text-to-trait channel is weak against self-report everywhere it has been measured:
  - Koutsoumpis 2024 finds LIWC-to-self-report |rho| of .08 to .14, with a mean R2 of 5.07% over k = 25 samples.
  - Mairesse 2007 SMOreg on observer ratings performs worse than the mean baseline for all five traits (relative absolute error 131 to 213%, 96 EAR speakers, 10-fold CV).
  - The same literature shows text tracks *observer impressions* much better: a mean observer R2 of 38.5% (k = 5, n = 1,637, mostly zero-acquaintance raters). That fits the system's choice of observer gold panels. It also means panel convergence shows shared public cues, not trait accuracy.
- **LLM channel.** The evidence base is non-elite casual posts scored against self-report: Peters & Matz 2024, n = 1,000, mean r about .29. It shows parity with specialized models, not superiority. It does not transfer to de-identified congressional text.
- **Persona channel.** This has the most direct methodological source: the Birkenmaier & Lechner 2025 codebook, with macro-F1 .76 to .77 on German sentences. However, the system runs a different model, in a different language, with no human-labelled test.

**Two components are effectively gaps.** Shrinkage has one weak contextual bearing. Cooperation v2 has two weak bearings and nothing on its own empirical question, which is stability across Congresses.

**Bookkeeping problems the verifiers found.** These are record and citation defects, not modelling findings:
- The held-out CV against RKH (mean .350; E .510 down to O .214) was not located in the repo by several verifiers. The committed `big5_v2.json` instead holds held-out r against *behavioural* targets (A .059, C .281, E .563, O .135).
- The affective-N component record says z(LIWC) + z(LLM). The code has LIWC as a convergent anchor only.
- The `big5_llm.py` docstring cites Cao & Kosinski with the wrong venue and the wrong figures.
- `behavioral_dispositions.py` attributes roll-call participation to RKH as its strongest C criterion. The 2016 paper has no participation analysis.
- The gold artifact silently drops the academic Feingold row (125 records, against 126 text entries).

**Paul's positions against the evidence.**
- *Ramey is "dated but foundational".* The evidence agrees and goes further: the Mairesse instrument underneath Ramey is itself near chance.
- *LLM trait reading is endorsed because GPT-4o and DeepSeek-V3 were validated.* That validation is for sentence-level agency/communion cues, not item-level Big Five. It also does not cover the Claude models the system actually runs. The endorsement over-reaches for the Pink reader, but fits the persona channel's design.
- *Birkenmaier's alpha range.* Paul's notes cite it, but they do not reflect the system's single-look MISS record. A brain ruling also misreads the alpha: .72 is the cue-presence alpha, not evidence that the task was "easier".

## Lexical Big Five (Ramey-style closed vocabulary)

**What it computes.** Corpus-relative Big Five z (O, C, E, A, ES; N = -ES) from category rates of floor speech and interviews.

**Method.** Rates per 1000 words (LIWC-analog, Empath, MATTR, Brysbaert concreteness) are z-scored and combined with linear weights from published correlates or SMOreg (Mairesse) fit on essays, then refit to RKH targets per trait (hybrid). Shrinkage z x sqrt(tokens/1500). Held-out CV vs RKH: E .510, A .406, C .363, ES .258, O .214 (mean .350). O was reversed on the party check.

**Code refs.** `fec/members/big5_traits.py:1-286`, `fec/members/fit_smoreg_rkh.py`, `fec/members/fit_smoreg_essays.py`, `fec/liwc/mairesse_smoreg.json`

**Assumptions.**
- Closed-vocabulary rates carry trait signal
- RKH estimates are a valid reference (method-shared, so convergence is inflated)
- Scores are meaningful only within this corpus

### Evidence

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
| [[decter-frain-2016-impressive-words-linguistic]] | 2016 | qualifies | Chamber-level LIWC densities in House floor speech co-move with monthly approval. Exploratory stepwise standardized betas, UNSTRATIFIED: tentativeness +0.46, prepositions +0.41, humans +0.25, anxiety +0.24, positive emotion +0.24, numbers +0.15; second person -0.18, conjunctions -0.26 (8 of 63 categories kept, N = 206 months). | Inference, not tested: if category rates drift by period at the chamber level, members whose speech is concentrated in different years could pick up period variance in their trait z. The paper shows co-movement, not the size of the drift. A per-year de-meaning test on Paul's corpus would check it. | weak | VERIFIED-SOFTENED |
| [[hargrave-2022-longer-conforming-stereotypes]] | 2022 | method-source | Seed dictionaries expanded with GloVe word embeddings correlate with human coder judgements at 0.61 to 0.85 per style across the comparison and intensity tasks. Standard dictionaries do worse, e.g. positive emotion 0.38 and 0.34 against 0.70 and 0.71 for the embedding measure. Each coder completed about 70 comparisons per style on average. Six of eight styles use the embedding expansion, and the embedding measure beats standard dictionaries for every style except fact (bootstrap test). | The paper validation is on UK House of Commons sentences with two coders. Using its embedding expansion on congressional text needs a fresh human-coded sample. Any change to trait inputs falls under change control and single-look discipline. This is not a finding that the current lists are wrong. | weak | VERIFIED |
| [[osnabrugge-2021-playing-gallery-emotive]] | 2021 | qualifies | Within the same MP (MP fixed effects), PMQ speeches score 1.562 to 1.663 points higher on the emotive-minus-neutral scale than other debates (Table 3 Models 3-5; speech-level SD 8.95; UK 2001-2019, 958,925 speeches). The Queen's Speech opening day is 2.064 in Model 5. The authors read this as a venue pattern and say it is not causally identified. | Applies only if US floor speech, interviews and social posts differ in audience exposure the way PMQs differs from ordinary Commons debate. The paper covers UK and Irish lower houses with a valence-agnostic ANEW-based score, not LIWC posemo/negemo. The speech-level shift is about 0.18 SD. Observational, not causal. | moderate | VERIFIED-SOFTENED |

### Assessment

## Lexical Big Five (Ramey-style closed vocabulary)

**Evidence weight.** 25 bearings (22 distinct papers): 2 VERIFIED, 18 VERIFIED-SOFTENED, 5 UNVERIFIED. The weight of evidence is **against treating lexical scores as trait measures**, and for treating them as a corpus-relative, public-persona signal at best.

**Ceiling against self-report (VERIFIED-SOFTENED).**
- Koutsoumpis 2024 meta-analysis: self-reported Big Five correlate with LIWC categories at most |rho| .08 to .14. The 52 categories explain a mean R2 of 5.07% (k = 25; about 87% of n is one study, Schwartz 2013). Heterogeneity is significant for 96% of the effects, and formality attenuates the markers.
- Pennebaker 2003: r = .10 to .16 in college students.
- Marrero 2022: 3.13% out-of-sample R2 (N = 614 students, about 92 words each).
- Azucar 2017 pools r = .29 to .40. That figure is UNSTRATIFIED (16 studies, 4 platforms, 2012 to 2017, I2 above 93%) and inflated by best-model selection, so it is not a benchmark for this system.

**The underlying instrument fails (VERIFIED-SOFTENED, strong).** Mairesse 2007 SMOreg trained on EAR observer ratings (96 speakers, 10-fold CV) has relative absolute error *above* the mean baseline for every trait (E 140, ES 162, A 174, C 132, O 213 percent). On the essays corpus with self-report labels, SMOreg is indistinguishable from baseline for four traits; only O is significant (94.19). Birkenmaier 2025 calls Ramey's pretrained Mairesse SVR outdated and unvalidated against human annotation. Because the RKH reference targets are themselves Mairesse-type SVR output, refitting to RKH makes RKH convergence a *fit statistic, not validation* (Ramey bearing).

**Transfer conditions.**
- Ramey's precision band assumes about 5,000 or more words per member-year (median member about 11,000). The system's 1,500-token lowconf floor is far below that band, so Ramey's precision claim does not carry.
- Osnabrugge 2021: within-MP venue shifts of about 0.18 SD in emotive rhetoric, measured in UK and Irish houses.
- Decter-Frain 2016: chamber-level category rates co-move with approval over time (an UNSTRATIFIED exploratory stepwise fit over 206 House months).
- Together these mean that pooling social posts, CREC and interview text, with genre only as a validation hook, can import venue and period variance into a member's z. These are cross-national or aggregate results, so they are a plausible confound, not a measured one.
- The system uses exemplar word lists, not licensed LIWC dictionaries, so category equivalence with the literature is unverified. Hargrave 2022 (VERIFIED method-source) shows embedding-expanded seed dictionaries beat standard ones against human coders (positive emotion .38/.34 against .70/.71). That result is for UK Commons sentences.

**Record discrepancies.**
- The .350 held-out CV against RKH, quoted in the component record, was not found by four separate verifiers. The repo's `big5_v2.json` holds held-out r against behavioural targets instead.
- Two bearings say the RKH refit weights are *candidate*, promoted only on REFIT WINS, and that the v2 lexical builder learns weights against behavioural criteria, with RKH/Mairesse weights as v1 priors.
- The component description is therefore stale on what the active instrument is.

### Contradictions

- Mairesse 2007 (VERIFIED-SOFTENED, strong): SMOreg on observer ratings is worse than the mean baseline for all five traits (RAE 131 to 213 percent, 96 speakers). The system's active weights descend from this model family, and RKH targets are built on it.
- Koutsoumpis 2024 sign comparisons: the SMOreg weights for ES first-person singular (+.0094) and E positive emotion (-.0339) run against the paper's bivariate directions. These stay UNVERIFIED leads (DE-062): SMOreg weights are multivariate, so suppression can flip signs.
- The component record says the active weights are refit to RKH, with held-out mean .350. Verifiers found no stored .350 output. The repo holds held-out r against behavioural targets (A .059, C .281, E .563, O .135), and the RKH refit is described as candidate-only.
- The Ramey citation year conflicts within the repo: 2016 in code and PERSONALITY_RECEPTIVITY_LAYER, 2019 in big5_methods.md.
- The 1,500-token lowconf floor sits well below Ramey's roughly 5,000-word band for CI width of about 0.3 or less.

### Gaps

- No study tests closed-vocabulary trait scoring on US congressional floor speech or interviews against an independent criterion. RKH is method-shared, and the Senate panels are tested in the gold-panel component. Filling this needs a pre-registered look of lexical scores against the k>=2 Senate panel rows, with party strata, on text from the panels' era.
- No measurement-invariance evidence across genres. Rammstedt 2025 (UNVERIFIED) is an analogy only. A within-member MTMM or invariance study across floor, interview and social text, with genre-specific z-scoring, would fill it.
- There is no evidence on period drift at the member level. A per-year de-meaning test on Paul's corpus (Decter-Frain suggests the mechanism) would show whether members whose speech falls in different years pick up period variance.

### Paul's position vs the evidence

Paul's view that Ramey is 'dated but foundational' is supported, and the evidence is harsher than his word 'dated'. The Mairesse model underneath Ramey does not beat baseline on observer ratings, and Birkenmaier's critique (VERIFIED-SOFTENED) holds. His call for venue-specific frameworks has direct support:
- Lechner 2025 (VERIFIED): contextualized measures outperform global ones.
- Osnabrugge 2021: venue-dependent emotive shifts.
- Koutsoumpis: formality attenuates the markers.

The tension he noted, that RKH calibration inherits Ramey's flaws, is confirmed. A refit to RKH is a fit statistic, not validity evidence.

### Open questions

- Where is the .350 held-out CV against RKH stored, and is the active lexical instrument the RKH refit, the published-correlate weights, or the v2 behavioural-criterion fit?
- Do member trait z-scores change when genre is z-scored separately rather than pooled?
- Does lexical O survive once staff-written or prepared text is separated from spontaneous speech? Simchon 2024 (UNVERIFIED) shows openness-styled text can be produced on demand.
- Should the sign disagreements against Koutsoumpis (ES fps, E posemo) be audited with a bivariate check on the system's own corpus?

### Read first

- [[koutsoumpis-2024-kernel-truth-text]]
- [[mairesse-2007-automatic-recognition-personality]]
- [[ramey-2016-measuring-elite-personality]]
- [[birkenmaier-2025-measuring-politicians-public]]
- [[osnabrugge-2021-playing-gallery-emotive]]
- [[hargrave-2022-longer-conforming-stereotypes]]

## Big Five validation against Senate gold panels and criteria

**What it computes.** Pass or miss of trait validity looks. E and A pass both panels; O passes academics only; N passes insiders only; C has an insider association only.

**Method.** Pre-registered looks: interview C vs votes cast rho +0.155 PASS; C vs follow-through +0.112 PASS. Several improvement rounds MISSed. A-ES about .44 is population-real (RKH benchmark) plus about .21 text-tone halo. Status labels must accompany any external claim.

**Code refs.** `fec/big5_improvements_eval.py`, `fec/b5_residual_eval.py`, `fec/b5_stylometry_eval.py`

**Assumptions.**
- Gold panels are independent ratings
- k=1 insider ratings are noisy

### Evidence

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
| [[cao-2024-language-models-personality]] | 2024 | qualifies | Perceptions of public figures track likability and demographics: birth year with extraversion, human r = .35 and model r = .44; women are perceived as more agreeable; an individual rater agrees with the aggregate at r = .56-.66. | The paper's raters are lay Prolific workers rating world figures they recognise, not Hill insiders. The halo's size in Hill expert panels is untested by this paper. The gold-panel component is affected only if its raters use reputation and likability. | weak | VERIFIED-SOFTENED |
| [[peters-2024-language-models-infer]] | 2024 | qualifies | In 68 users rated by Facebook friends on a 10-item IPIP, LLM-observer agreement averaged r .269 (GPT-3.5) and .276 (GPT-4), against .304 for self-report-observer agreement. For Conscientiousness, LLM-observer agreement exceeded the agreement of either the LLM or observers with self-report. The analysis is described as preliminary. | If LLM readings track the same outward impressions that observers form, convergence with expert or insider panels may index shared reputation or presentation rather than the underlying trait. The evidence is preliminary (n = 68) and rests on friends rating peers, not experts rating senators. | moderate | VERIFIED-SOFTENED |

### Assessment

## Big Five validation against Senate gold panels and criteria

**Evidence weight.** 15 bearings (13 distinct papers): 2 VERIFIED, 13 VERIFIED-SOFTENED, 0 UNVERIFIED. The panels are the right *kind* of criterion. Koutsoumpis and Mairesse both show text tracks observer ratings far better than self-report. Mairesse's EAR observer models beat baseline for E, ES and C while no self-report model did; Celli gives machine perception rho .30 against human .38. But several verified defects bound what a PASS means.

**1. Inclusion rule (Mondak appendix, VERIFIED, strong).** The source paper excludes senators rated by only one coder from all analyses. The primary validators do not:
- `b5_gold_multitrait_validate.py` averages the panels with no count weight.
- `b5_external_gold_validate.py` filters only on null traits.
- Only side probes stratify k=1 against k>=2, or weight by sqrt(ratings_n).

Any k>=2 re-run is a *new look* under single-look discipline, not a free re-score.

**2. Party fault line (VERIFIED-SOFTENED).** The panels' extremes are party-skewed: Democrats are 62% of insider top scorers and 65% of bottom scorers; in the academic table, 80% of high scores go to Democrats and 80% of low scores to Republicans. Neither primary validator has a party term. Panel correlations must be quoted with party strata or an UNSTRATIFIED label. One verifier confirmed the pass/miss pattern against `big5_gold_anchored.json`: E and A pass both panels, O passes academics only (insider p .104), and N passes insiders only (academic p .195). Whether party explains O's academic-only pass remains UNVERIFIED.

**3. Artifact integrity (VERIFIED-SOFTENED).**
- The academic Feingold row (malformed cell) is silently dropped by an except branch, whose comment wrongly says only headings reach it. That leaves 125 academic records against 126 text entries.
- Full-sample gold-fitted weights are emitted after the OOF verdict and passed into the shipped composites. Only the OOF figures are clean; for shipped composites the panels are no longer untouched.

**4. Criterion contamination (Fouirnaies 2022, VERIFIED).** Roll-call participation falls 2.70 points (SE 0.53) from a mean of 90.48 in a legislator's final term. The data are US state legislatures, so transfer is moderate. The interview C criterion (rho +.155 PASS against votes_cast) is cross-sectional with no electoral-status stratum, so external claims should carry an UNSTRATIFIED label on retiring or running for other office. The PASS stands; it must not be re-run.

**5. What convergence means.**
- Koutsoumpis: panel convergence shows raters and text use the same cues, not trait accuracy. SOKA predicts the overt traits (E, A) pass and ES is weakest, which is the observed pattern.
- Cao 2024: perceptions of public figures track likability and demographics.
- Lechner 2025: observer ratings carry rater-ideology bias (r up to .48).
- Both of those are lay raters, not Hill insiders, so they are reasons to check, not proof of bias.

**Cross-chamber.** The panels are Senate-only and historic, while Ramey's validation is a House paper. Rank-order validity holds only on overlap with current senators.

### Contradictions

- The Mondak appendix excludes k=1 senators from all analyses. The primary validators include them with no count weighting (VERIFIED, strong).
- The brain ruling of 2026-07-04 reads Birkenmaier's alpha of about .72 as an easier ternary task, and uses that to set aside the single-rater gold test (agency rho +.035, communion -.245, n = 30). The paper does not call its task easier, and .72 is the cue-presence alpha, not the agency or communion alpha (.70 to .78).
- The brain records 125 academic gold records against 126 in the appendix text: the Feingold row is silently dropped.
- The panels are described as untouched gold, but full-sample gold-fitted weights feed the shipped composites through b5_profile_basis.py.
- The claim that lexical O 'was reversed on the party check' is not supported. The ONC candidate file marks O as NOT_LOOKED_CRITERION_STOP_BAR_MISS.

### Gaps

- No bearing tests whether informed Hill insiders, as opposed to lay raters, show halo or ideology bias. Filling this needs within-panel analysis: correlate each panel's ratings with party and an ideal-point measure, and compare the insider and academic panels on the same senators.
- No study gives a House criterion. A House observer panel, or a validated House behavioural criterion, would be needed before any House-wide validity claim.
- The '.21 text-tone halo' attribution for A-ES was not located by verifiers. It needs a recorded computation or should be withdrawn from the record.

### Paul's position vs the evidence

Paul highlighted Birkenmaier's validation framework (codebook; Krippendorff alpha .70 to .78; no model above macro-F1 .80). That framework is about sentence-cue coding by three trained coders. It is not member-level trait validation, so it does not speak to the Senate panel looks. His notes do not record the system's MISS record. The evidence suggests even the PASSes (E and A on both panels) are bounded by shared public cues and possible party and likability halo. They should travel with status labels and party strata.

### Open questions

- How many current-roster panel matches are k=1 insiders, and would a pre-registered k>=2 look be worth spending a look on?
- Do panel-level pass correlations survive party stratification?
- Should the C PASS on votes_cast be re-labelled UNSTRATIFIED on electoral status in every external claim?
- Should shipped composites stop using full-sample gold-fitted weights, so the panels stay clean for future looks? (Paul's ruling.)

### Read first

- [[mondak-nd-online-appendix-personality]]
- [[koutsoumpis-2024-kernel-truth-text]]
- [[fouirnaies-2022-electoral-incentives-affect]]
- [[lechner-2025-mapping-political-personality]]
- [[cao-2024-language-models-personality]]
- [[celli-nd-twenty-years-personality]]

## LLM item-level reader (Pink), ridge hybrid and CT-CM latent model

**What it computes.** De-identified, chunked LLM ratings on 20 items; ridge hybrid on lexical, interaction and persona features; CT-CM SEM with an LLM method factor.

**Method.** The LLM reader removes names before scoring and codes 'cannot tell' as 0. It is display-only. Pink x behavioural diagonal mean +0.064 < 0.12 fails. CT-CM is a one-look capstone.

**Code refs.** `fec/members/big5_llm.py`, `fec/members/big5_hybrid.py`, `fec/members/big5_ctcm.py`

**Assumptions.**
- De-identification removes reputational priors (Cao & Kosinski)
- Judge halo is a single method factor

### Evidence

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
| [[cao-2024-language-models-personality]] | 2024 | supports | Perceived Big Five of public figures is predictable from the name embedding alone: cross-validated r = .78 (extraversion) to .88 (openness) after attenuation correction, raw .70-.80, N = 226 figures, 20-fold CV ridge on GPT-3 name embeddings; accuracy higher for more popular figures (r = .16 with log Wikipedia page views). | The paper uses GPT-3 name embeddings and lay ratings of globally famous figures, not a chat model reading congressional text. Transfer to the Pink reader is by analogy and untested. | strong | VERIFIED-SOFTENED |
| [[cao-2024-language-models-personality]] | 2024 | qualifies | The paper uses name embeddings only. It does not test whether removing names from surrounding text removes the signal. It notes predictions reflect only the period in the training data, and that extension to people absent from training data is untested. | Matters only where Pink scores feed a validity claim. The de-identification rationale is the code docstring's, not the paper's; the paper discusses a privacy threat from extracting traits from data (Discussion p.6), not text de-identification. The rel's Pink display-only status and the +.064 diagonal figure were not found in the brain concepts folder and remain unverified. | moderate | VERIFIED-SOFTENED |
| [[cao-2024-language-models-personality]] | 2024 | qualifies | Controlling for likability plus birth year and gender lowers accuracy from corrected r = .78-.88 to r = .53-.70 (raw .43-.61). The paper attributes the likability-trait links to the personality halo effect and reports that the least likable figures sit at undesirable extremes of several traits. | The paper's halo is in lay perceptions of famous people, with likability measured per figure. The rel says Melange has no per-member likability covariate; that was not checked here. The method factor is identified only by model structure, so the evidence bears on the halo's shape, not its size in congressional text. | weak | VERIFIED-SOFTENED |
| [[lemens-2025-positioning-political-texts]] | 2025 | qualifies | Sec 3.3 and Figs 3-4: sentence-by-sentence then averaged GPT-4o scores track expert coding on manifestos at r=0.98 (economic) and 0.94 (social). GPT-4 gives 0.98 and 0.92. Single whole-manifesto prompts (App G.2, Figs A12-A13) give economic r 0.96 to 0.98, similar to sentence scoring, and social r 0.83 to 0.92 with weaker within-party fit. Sec 4 says the paper has no clear recommendation on chunk versus single prompt. Sentences with NA are dropped from the average, not coded 0. | The paper's units are single sentences from party manifestos and speeches, not chunks of member speech, and it drops NA rather than coding it 0. The paper does not discuss de-identification or name priors, so the claim that its senator r=0.97 reflects name priors is the analyst's speculation. The paper does not say whether names were removed from senators' tweets. The traits differ, so it cannot rescue the failed behavioural gate. | weak | VERIFIED-SOFTENED |
| [[peters-2024-language-models-infer]] | 2024 | qualifies | Zero-shot holistic 1-5 Big Five ratings by GPT-4 from the last 200 Facebook updates (20-message chunks, chunk means, three queries averaged) correlate with IPIP-100 self-report at mean r .31 for GPT-4 and .27 for GPT-3.5 (n = 1,000 users; pooled average .29; per-trait range .22-.33 across both models). Supervised in-domain models reach about .37 (Park et al., as cited in Sec 4.1). | Holds only for informal first-person posts by ordinary young users scored against self-report. Member floor speech and interviews differ in genre and have no self-report criterion. The paper reports one accuracy level for one holistic prompt on one sample. It does not state a ceiling, so the figure is a reference point, not a bound. The item-level, de-identified design differs from the paper's holistic prompt. | moderate | VERIFIED-SOFTENED |
| [[peters-2024-language-models-infer]] | 2024 | suggests-extension | Each inference was queried three times and averaged. Between-round correlations ranged from .73 (GPT-4, Openness) to .96 (GPT-3.5, Agreeableness), so single-call noise is not negligible. | Applies if the item-level Claude ratings show between-rep variance like the paper's. The paper is a holistic prompt on GPT models, so the size does not transfer directly. Any change to published scores would be new agent analysis, UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED |

### Assessment

## LLM item-level reader (Pink), ridge hybrid and CT-CM latent model

**Evidence weight.** 14 bearings (11 distinct papers): 1 VERIFIED, 11 VERIFIED-SOFTENED, 2 UNVERIFIED. The evidence supports the system's *defensive* design choices. It gives no support for promoting Pink out of display-only.

**De-identification rationale (Cao & Kosinski 2024, VERIFIED-SOFTENED, strong).**
- Perceived Big Five of public figures is predictable from the *name embedding alone*: corrected cross-validated r .78 to .88 (raw .70 to .80), N = 226 figures, 20-fold CV ridge. This supports removing names.
- But the paper never tests whether blanking names in surrounding text removes the signal.
- `deidentify` leaves committee names, district cities and bill names. Its case-insensitive name-part matching blanks common-word surnames in ordinary text.
- The `--named` probe is coded, but no `_named` artifact exists. De-identification efficacy is therefore *untested*.
- The docstring cites the wrong venue (PNAS Nexus 2024) and wrong figures (.76 to .87).

**Accuracy reference (Peters & Matz 2024).**
- Zero-shot holistic GPT ratings of Facebook posts reach mean r about .29 against self-report (n = 1,000). Celli (VERIFIED-SOFTENED) calls this parity with specialized ML, not superiority.
- Peters (VERIFIED): between-round correlations range .73 to .96, so single-call noise is non-trivial. Pink publishes rep 0 only and does not average reps.
- The paper's population is young casual posters, scored against self-report, so a lower figure on de-identified political text is expected.

**Keying imbalance (Soto 2017, VERIFIED-SOFTENED).** The docstring says 'balanced keying', but O, C, E and A are keyed 3 true to 1 false, and ES 2 to 2. An all-yes response yields +0.5 on four traits and 0 on ES. Whether the LLM has a yea-saying tendency on observer items is untested. If it does, acquiescence would lift O, C, E and A together; it would not create the A-ES correlation.

**Hybrid (Azucar 2017 bearing, citing the committed artifact).** `big5_v3_hybrid.json` shows no held-out gain from adding interaction and persona features: A .056 vs .059 lexical-only, C .273 vs .281, E .564 vs .563, O .130 vs .135. The docstring says interaction features are nickname-degraded, so these numbers are a floor. As things stand the hybrid is not earning its complexity.

**CT-CM.** No bearing tests a method-factor model. Lechner (human raters, ideology and likability bias) and Cao (halo: accuracy falls from .78 to .88 down to .53 to .70 after a likability control) support that a halo exists in *human* perception of public figures. That an LLM carries the same halo, and that a single orthogonal factor absorbs it, is inference.

**Birkenmaier and Lemens do not transfer.** Birkenmaier validates a narrow sentence-cue task, not item-level Big Five. Lemens 2025 (GPT-4o manifesto positioning, r .94 to .98) drops NA where Pink codes it 0, scores ideology rather than traits, and cannot rescue the failed behavioural gate (+.064 against the .12 bar; one verifier found .064 in the vault but not the .12 bar).

### Contradictions

- The big5_llm.py docstring says the items are balanced-keyed. The ITEMS table is 3:1 for O, C, E and A (Soto bearing, VERIFIED-SOFTENED).
- The docstring cites Cao & Kosinski as PNAS Nexus 2024 with r = .76 to .87, which matches neither the paper's venue nor its figures (corrected .78 to .88, raw .70 to .80).
- Paul's endorsement of LLM trait reading rests on GPT-4o and DeepSeek-V3 passing Birkenmaier's cue task. Pink runs Claude models on an item-level Big Five task the paper did not validate.
- The hybrid's stated purpose is a gain over lexical, but the committed artifact shows equal or lower held-out r on all four targets.

### Gaps

- No evidence on whether text de-identification removes reputational priors for well-known legislators. Fill: run the coded --named probe against de-identified scores on the same chunks, extend scrubbing to committees, districts and bills, and compare the trait shifts against member prominence (for example, page views).
- No evidence on LLM acquiescence in observer-keyed items. Fill: score an all-neutral or reversed-only control, or re-key to 2:2 and compare.
- No study validates a CT-CM or halo method-factor structure for LLM raters. Fill: a multi-rater, multi-method design on a held-out criterion. This is already a one-look capstone, so the look must be pre-registered.

### Paul's position vs the evidence

Paul endorses LLM-based trait reading because GPT-4o and DeepSeek-V3 were the only models validated. The evidence supports that endorsement for *sentence-level agency/communion cue classification on German text*, which is the persona channel's task. It does not support it for item-level Big Five reading. Other models were not shown to fail on the system's task; they were not tested on it. The broader LLM literature (Peters, Celli) shows parity with specialized models against self-report on casual posts, not superiority, and nothing on elite political text. Pink's failed behavioural gate (+.064) is consistent with that literature, not in tension with it.

### Open questions

- Should published Pink scores average several reps, given between-round r of .73 to .96 in Peters?
- Does the hybrid deserve to stay once interaction features are re-ingested, or should it be retired on its null held-out gain?
- Is the 0.12 behavioural-diagonal bar recorded anywhere (prereg or brain)? One verifier could not locate it.

### Read first

- [[cao-2024-language-models-personality]]
- [[peters-2024-language-models-infer]]
- [[birkenmaier-2025-measuring-politicians-public]]
- [[soto-2017-next-five-inventory]]
- [[lechner-2025-mapping-political-personality]]
- [[lemens-2025-positioning-political-texts]]

## Behavioural dispositions

**What it computes.** Cooperation, diligence (roll-call participation, LEP), dominance (turn statistics, leadership), visibility (interviews), breadth (portfolio) and reactivity (interview cutoff).

**Method.** z-score composites with coverage floors; missing values are neutral.

**Code refs.** `fec/members/behavioral_dispositions.py:1-120`, `fec/members/interaction_features.py`

**Assumptions.**
- Observed behaviour indicates disposition
- Turn-share statistics index dominance (Rienks & Heylen; Valente)

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[fouirnaies-2022-electoral-incentives-affect]] | 2022 | qualifies | Floor-vote participation is state-dependent within the same legislator. It falls 2.70 points (SE 0.53) from a mean of 90.48 in the final term, about a 27% proportional rise in abstention. Committee activity falls about 8% (-0.31 from 3.84). | Moderate. Missing votes is a universal congressional behaviour, and the paper's within-person identification is cleaner than anything the system has. However, the state final-term drop of about 3 points is small next to the cross-member spread in congressional votes_cast. It matters most for members in their final Congress, or those campaigning for another office, whose absences are campaign-driven. The disposition reading holds only after electoral status is held fixed. | moderate | VERIFIED |
| [[ramey-2016-measuring-elite-personality]] | 2016 | contradicts | The 2016 paper's only Conscientiousness outcome is the ceremonial-bill share (Table 3). It has no roll-call participation analysis; roll calls appear only in passing as a source of ideal points (p.5). | The 2016 text does not support the attribution. Whether another RKH work (the 2015 manuscripts cited at pp.15 and 19) uses participation cannot be checked from this text. Treat as a citation fix, not a modelling finding. | moderate | VERIFIED-SOFTENED |
| [[azucar-2017-predicting-personality-traits]] | 2017 | context | Social-media activity statistics (posts, friends, network density, likes received, comments, tags) gave a marginal increase in E accuracy (beta .19, R2 .18, p=.06). The paper reports no other significant moderator effects; it does not report a tested null for each trait. | The constructs differ. The paper's activity variables are social-media posting, friend counts and network density; the system's variable counts press or broadcast interviews. Any link is an analogy, not support, and it bears on E only, not on the lexical E it was meant to contrast with. The analyst's point that interview counts track press-office scheduling and seniority is their inference and was not checked against the repo. | weak | VERIFIED-SOFTENED |
| [[amsalem-2018-effect-politicians-personality]] | 2018 | qualifies | Extraversion is positive and significant in the pooled OLS: b = 0.129 (SE 0.052), beta .117, p < .05 (Model 1, n = 306, R2 = .483, country dummies and controls). Also significant on news websites (Model 3, b = 0.117, p < .05). | The extraversion direction holds in the paper, but the effect is small (beta .117). The paper's outcome is earned mentions by journalists, not interviews a member gives. The sample is MPs in Belgium, Canada and Israel, 2011-2015, with self-reported TIPI. Office is the dominant predictor there, so a US interview count needs office controls before it reads as temperament. Any claim that visibility_z double-counts dominance_z is an untested inference, not a paper finding. | moderate | VERIFIED-SOFTENED |
| [[salam-2022-learning-personalised-models]] | 2022 | context | Among generic single modalities, talk-turn features without sentiment (duration, content, speech rate) average AMSE 0.874 across the five traits, audio 0.920, and FaceBody 1.325 (Table 1, p.65). The paper does not isolate extraversion or dominance and does not test broadcast interviews. | Indirect, five-trait-averaged evidence that turn structure carries trait signal in dyadic lab tasks. It does not validate dominance_z or its committee term, and says nothing about congressional floor speech. Any use should name the window and denominator. | weak | VERIFIED-SOFTENED |
| [[yang-2023-getting-personal-deep]] | 2023 | suggests-extension | The same GBDT and RFR models are run with and without personality features. Average MSE improvement: GBDT 14.29 percent (CH 20.91, SG&A 31.74, ROA 12.52), RFR 6.67 percent. GBDT gains from CNN-1 features 3.38, CNN-2 minus 1.80, BERT 8.62. Gains for all eight DVs are significant by Wilcoxon, GBDT at p below 0.01. Observations per DV range 405 to 519 (Table 10). A chronological split is reported in Online Appendix G. | Borrow the design only: the same learner with and without a trait block, a held-out chronological split, and paired tests, applied to compare text traits against dispositions on a pre-registered responsiveness outcome. The lift magnitudes come from executives and firm accounting outcomes with reverse causality the authors leave unresolved, so they must not be quoted as evidence that text traits predict member behaviour. | moderate | VERIFIED |

### Assessment

## Behavioural dispositions

**Evidence weight.** 6 bearings (6 distinct papers): 2 VERIFIED, 4 VERIFIED-SOFTENED, 0 UNVERIFIED. The evidence bears on *measurement confounds* in two of six dispositions (diligence and visibility). It bears weakly on one more (dominance), and not at all on cooperation, breadth or reactivity. Nothing tests the component's empirical question: whether dispositions predict lobbying responsiveness better than text traits.

**Diligence (Fouirnaies & Hall 2022, VERIFIED).**
- Floor-vote participation is state-dependent within the same legislator: -2.70 points (SE 0.53) from a mean of 90.48 in the final term, about 27% more abstention. Committee activity falls about 8%.
- The data are US state legislatures. Transfer to Congress is moderate: missing votes is universal, and the identification is cleaner than anything in the system.
- In the system, diligence_z enters susceptibility at -0.20, so retiring members get *higher* susceptibility, while also receiving incentive 0.60. Electoral status enters the pressure term twice with opposite signs and is never modelled explicitly.
- The disposition reading holds only after electoral status is held fixed.

**Citation error (Ramey 2016, VERIFIED-SOFTENED, contradicts).** The docstring calls roll-call participation RKH's strongest C criterion. The 2016 paper's only C outcome is the ceremonial-bill share (Table 3), with no participation analysis. This is a citation fix, not a modelling finding.

**Visibility (Amsalem 2018, VERIFIED-SOFTENED).**
- Extraversion predicts news presence (b = .129, SE .052, beta .117; n = 306 MPs in Belgium, Canada and Israel, 2011 to 2015; self-report TIPI). Office is the dominant predictor.
- The outcome is earned journalist mentions, not interviews given.
- visibility_z needs office and leadership controls before it reads as temperament.

**Dominance (Salam 2022).** Turn-structure features carry trait signal, averaged over five traits, in dyadic lab tasks. This does not validate dominance_z or its committee-leadership term.

**Design template (Yang 2023, VERIFIED).** Run the same learner with and without a trait block, on a chronological split, with paired tests. This is the right design for the H1/H2 comparison of text traits against dispositions. The effect sizes (GBDT 14.29% MSE improvement on firm outcomes) must not be quoted for legislators.

### Contradictions

- behavioral_dispositions.py lines 13-14 cite roll-call participation as RKH's strongest C criterion. Ramey et al. 2016 contains no participation analysis (VERIFIED-SOFTENED).
- Electoral status enters susceptibility twice with opposite signs: lower diligence raises susceptibility, while the retirement incentive enters separately. Fouirnaies shows the diligence drop is partly electoral, not trait.

### Gaps

- Cooperation, breadth and reactivity have no external evidence at all. Fill: a test-retest study across Congresses (trait-likeness), plus a convergent check against an independent observer measure such as the Senate panels' A or C.
- The core question, whether dispositions predict lobbying responsiveness better than text traits, has no evidence. Fill: the pre-registered prospective H1/H2 test in Yang's with/without design, with a chronological split.
- No US congressional study separates trait from situation in participation. Fill: a within-member panel of votes_cast across terms, with retiring and running-for-other-office indicators.

### Paul's position vs the evidence

Paul's position is indirect support: effort allocation is observable, and conscientiousness relates to sponsorship. Fouirnaies supports observability but cuts against reading participation as a pure trait: a sizeable within-person component is electoral. Ramey's conscientiousness link is to ceremonial-bill *share*, not participation, so the 'C relates to sponsorship' citation does not carry over to diligence_z as written.

### Open questions

- Should diligence_z be computed with electoral status held fixed (excluding or stratifying final-term and other-office runs)?
- Does visibility_z survive office and leadership controls?
- Is there overlap between visibility_z and dominance_z, given both carry leadership-related variance? This is untested and remains an inference.

### Read first

- [[fouirnaies-2022-electoral-incentives-affect]]
- [[ramey-2016-measuring-elite-personality]]
- [[amsalem-2018-effect-politicians-personality]]
- [[yang-2023-getting-personal-deep]]
- [[salam-2022-learning-personalised-models]]

## Cooperation v2 (share-based bridging)

**What it computes.** Mean of within-chamber z of cross-party tie share and directional cross-party cosponsorship share (at least 10 cosponsorships). Reciprocity is reported only.

**Method.** Volume-free by design. Promoted 2026-07-10. Correlates rho .888 with bpf.

**Code refs.** `fec/members/coop_v2_candidate.py:1-130`

**Assumptions.**
- Cross-party share measures cooperation independent of volume

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | supports | Average between-party and within-party pairwise distances (Figs 7-8, 1879-1998) show polarization declining to WWII, stable until the late 1970s, and rising steadily since. The paper does not state that the most extreme members sit farthest from the opposing party; that is an analyst inference. | The paper supplies context for why an extremity control matters in a polarized chamber; it does not test the cooperation measure. | weak | VERIFIED-SOFTENED |
| [[harbridge-yong-2023-bipartisan-path-effective]] | 2023 | supports | Offering cross-party cosponsorships predicts attracting them (Table 4; DV attracted share, IV offered share). House pooled Model 4.1 0.626 SE 0.039; House member FE Model 4.2 0.317 SE 0.036; Senate pooled Model 4.3 0.739 SE 0.045; Senate FE Model 4.4 0.547 SE 0.047; all p<0.001. Distance from median: House pooled -0.154, FE 0.005 (n.s.); Senate pooled -0.114, FE -0.096. | The paper is a member-level aggregate over 1973-2016, by chamber. Pooled and FE coefficients differ: the House roughly halves under FE, the Senate falls by about a quarter, so quote by chamber with strata. The paper shows offered-to-attracted reciprocity only, not that the composite tracks any effectiveness outcome. | weak | VERIFIED-SOFTENED |

### Assessment

## Cooperation v2 (share-based bridging)

**Evidence weight.** 2 bearings (2 distinct papers): 0 VERIFIED, 2 VERIFIED-SOFTENED, 0 UNVERIFIED. Both are weak. **This is effectively a GAP.** Neither paper tests the composite or its empirical question, which is whether cross-party cosponsorship share is stable across Congresses.

**Harbridge-Yong et al. 2023.** At member level, offering cross-party cosponsorships predicts attracting them (1973 to 2016, by chamber):
- House: pooled .626 (SE .039); member fixed effects .317 (SE .036).
- Senate: pooled .739 (SE .045); member fixed effects .547 (SE .047).

The coefficient roughly halves in the House under fixed effects and falls about a quarter in the Senate, so any use must be quoted **by chamber with strata**. This is an offered-to-attracted *association*. It is not validation of the system's reciprocity term (c3), which correctly stays report-only. Reciprocity is not validated in this project (D-104 to D-108), and this paper does not change that. Nor does it show the composite tracks any effectiveness outcome. Distance from the median is significant pooled but not under House fixed effects, which is a hint that ideology-driven cross-member variation inflates pooled figures.

**Poole 1999.** Context only: between-party distances have risen steadily since the late 1970s. This justifies the system's extremity control (cooperation_v2_z vs |dim1| = -.250, n = 532 of 533, 2026-07-10). The paper does not say extreme members sit farthest from the other party; that is inference.

**Internal overlap.** rho +.888 with bpf (`receptivity_layer.py:183`) makes v2 a near-duplicate of an existing measure. That is a system-internal fact, not paper evidence.

**Design note.** The component is a member-level share, not per-edge volume normalization. It is not one of the two rejected SNA proposals, and neither bearing argues for those proposals.

### Contradictions

- Pooled versus member fixed-effects estimates diverge sharply by chamber (House .626 to .317; Senate .739 to .547). A pooled cross-party-share figure over both chambers would hide that fault line.

### Gaps

- Trait-likeness (the stated empirical question) is untested. Fill: a member panel of cross-party cosponsorship share over consecutive Congresses, by chamber, with a variance decomposition (between-member vs within-member). Stratify by majority status and party-control changes, since majority status shifts who offers bills.
- No criterion validity. Fill: test the composite against an independent cooperation criterion (for example, insider-panel A, or cross-party bill passage outcomes) in a pre-registered look.
- Paul's cited sources for this component (Bacik et al. 2026; Aref & Neal 2021) have no bearings in the evidence set, so they have not been assessed against it.

### Paul's position vs the evidence

Paul has no annotation on this component. The evidence base is too thin to support or contradict the design. The one substantive paper (Harbridge-Yong) argues for chamber-stratified quoting, and the system's within-chamber z-scoring is consistent with that.

### Open questions

- Given rho .888 with bpf, does cooperation_v2 add anything that bpf does not, on any held-out outcome?
- Is cross-party share stable enough across Congresses to call it a disposition rather than a positional or majority-status artifact?

### Read first

- [[harbridge-yong-2023-bipartisan-path-effective]]
- [[poole-1999-nominate-after-years]]
- [[lechner-2025-mapping-political-personality]]

## Affective neuroticism slot and N genre-transfer tests

**What it computes.** affective_n = z(LIWC i + negemo - posemo) + z(LLM pessimism, anxiety, depression, volatility), residualized on E and party.

**Method.** Gate 5/5 passed: |r(N,E)| .039; party .000; LIWC-LLM convergence +.236; split-half SB .742. Promoted 2026-07-11. Genre transfer of interview N to floor and hearing text FAILED (rho .050, .161).

**Code refs.** `fec/members/emit_affective_n.py`, `fec/members/affective_n_validate.py`

**Assumptions.**
- Negative-emotion and first-person-singular words track N (Pennebaker)
- No gold N criterion exists, so the gate tests construct integrity only

### Evidence

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
| [[aroyehun-2026-politics-of-feelings]] | 2026 | qualifies | Mixed models of speech-level emotion (Tables 5-6; n = 1,766,113 speeches; session and topic fixed effects): senators express less anger (-0.041, CI -0.043 to -0.039) and more pride (+0.009) and joy (+0.008); women express more fear (+0.004) and sadness (+0.006); majority members express less anger (-0.035); ideology score predicts more anger (+0.022). Mann-Kendall trends (Table 3, 26 Congresses): 15 of 16 party-by-emotion series increase significantly; Democratic joy does not (tau 0.243, FDR p 0.086). | Holds only weakly. Within one Congress majority status is nearly collinear with party, so the party residual may cover it. Chamber and gender are not covered and are a plausible structural confound. The paper's coefficients are mostly 0.001 to 0.04 in standardized units. The paper's facets are fear and sadness on floor text, not the interview facets used here. | weak | VERIFIED-SOFTENED |
| [[hargrave-2022-longer-conforming-stereotypes]] | 2022 | qualifies | Negative emotion runs against the stereotype: women use more of it than men for much of 1997-2019. Gender explains about 2 percent of MP-level variation in negative emotion in 1997-2007 and about 9 percent after 2007. For negative emotion the divergence is almost entirely differential replacement of MPs, with a near-zero within-MP gender difference (supplement Fig S8). The main text says neither replacement nor within-MP change alone explains the convergence across styles. Window: UK Commons 1997-2019. | Applies only if US members negative-affect expression differs by gender in the interview genre. The paper measures negative emotion as a dictionary score on UK floor speech, not the LLM facets, so this is a different operationalisation. Any chamber-composition effect needs a stated window. The interview-genre transfer figures cited by the analyst were not rechecked. | moderate | VERIFIED-SOFTENED |
| [[osnabrugge-2021-playing-gallery-emotive]] | 2021 | supports | The authors argue, without causal identification, that legislators raise emotive rhetoric when addressing voters: PMQ speeches are higher within the same MP (about +1.6, Table 3) and Irish Leaders' Questions higher by +2.5 to +4.6 (Table 5, N=945,734). The paper does not test whether speakers keep their rank order in affect across venues. | Explains, does not prove, the transfer failure. The paper tests average venue shifts, not member rank-order preservation, which is what rho measures. Does not license re-scoring floor text. | weak | VERIFIED-SOFTENED |
| [[peters-2024-language-models-infer]] | 2024 | qualifies | Inferred Neuroticism correlates with self-report at r .29 for GPT-4 and .26 for GPT-3.5 (n = 1,000). Both models overestimate N. Absolute N residuals are larger for men (t = -4.55 GPT-3.5; t = -2.39, p = .017 GPT-4), while within-gender correlations do not differ significantly, so the bias is in level, not rank order. | The paper's pattern suggests within-pool z removes common level bias, but a demographic level offset would pass into a mixed-gender roster. Relevant only if the Haiku facet prompts on interview text share GPT's level bias by gender. The paper's N is an IPIP trait score on social media posts; the system input is a facet sum on interview text. | weak | VERIFIED |
| [[rheault-2016-measuring-emotion-parliamentary]] | 2016 | qualifies | Labor disputes Granger-cause opposition polarity (chi2 8.048, p=0.005) but not government polarity (chi2 0.092, p=0.761), from annual pairwise augmented VAR tests on the Westminster series (Table 4 bottom). The text says the labor-dispute impulse response appears greater for opposition (S5 Fig, not extracted). The authors frame these as associations and argue the mechanism from theory, not from an exogenous shock. | The paper covers one Westminster chamber, aggregated over time, not individual-level US data. The extracted text does not state the frequency of the Table 4 tests, and S5 also has quarterly models. Within one Congress, party and majority status coincide, so the party control absorbs role; the chamber-flip window in the rel is the analyst's premise, not from this paper, and is untested here. Treat it as a possible confound only. | weak | VERIFIED-SOFTENED |

### Assessment

## Affective neuroticism slot and N genre-transfer tests

**Evidence weight.** 18 bearings (18 distinct papers): 4 VERIFIED, 12 VERIFIED-SOFTENED, 2 UNVERIFIED. The evidence supports the *direction* of the lexical anchor. It gives **no criterion validity for affective N as neuroticism**, and it names several plausible confounds the residualization does not cover.

**Direction support (VERIFIED-SOFTENED).**
- Koutsoumpis 2024: ES relates negatively to negative-emotion words (cue validity rho -.12) and to first-person singular (observer cue utilization rho -.29).
- Pennebaker 2003 and Mairesse 2007 (VERIFIED): self-references and negative emotion mark N in student essays.
- These are student or social-media samples against self-report. The genre gap to media-trained legislators is large.
- Marrero 2022: text beats voice for N in students, but its top N features differ from the system's anchor.

**Mechanism unmeasured (Byrne 2014, VERIFIED).** Byrne's working-memory mechanism for N-driven choking was never measured in the paper: there was no anxiety measure, no manipulation check, and self-rated importance did not differ by condition. The project's own H5 test (N x loss domain, partial r +.036, n = 441) is null. The N contribution keeps its UNVERIFIED label.

**Selection into the interview corpus (Amsalem 2018, VERIFIED).** More neurotic MPs appear *less* in national news (beta -.111), net of office and activity. If US interview coverage selects the same way, interview-derived N is range-restricted. That is untested for the US, and the paper concerns earned coverage, not interviews given.

**Confounds not residualized (VERIFIED-SOFTENED).** affective_n is residualized on E and on a Republican dummy only.
- Aroyehun 2026 (1.77M US floor speeches): senators show less anger (-.041); women show more fear and sadness; ideology predicts anger (+.022). Coefficients are small (.001 to .04 SD).
- Hargrave 2022 (UK, 1997 to 2019): the gender gap in negative emotion grows across the period, from about 2% of MP-level variance in 1997-2007 to about 9% after 2007, driven by replacement of MPs rather than within-MP change.
- Peters 2024 (VERIFIED): LLMs overestimate N, with a gender *level* bias (absolute residuals larger for men) but no rank-order difference within gender. A within-pool z on a mixed-gender roster would pass a level offset through.
- Rheault 2016: opposition and government affect differ. Within one Congress majority status coincides with party, so the party control absorbs it.
- Chamber and gender are the open confounds.

**Genre transfer.** Osnabrugge 2021 offers a venue mechanism for the failed transfer (rho .050, .161): within-MP emotive rhetoric rises when addressing voters. It explains the failure, does not prove it, and does not license re-scoring floor text. Pennebaker's 4-week retest of .24 for psychological-process words is consistent with weak transfer, but that link is inference. Several verifiers could not locate the .050 and .161 values in code; they are brain-recorded.

**Facet choice (Soto 2017).** Anxiety and Volatility predict stress resistance more closely than Depression. Separating facets would be a new prereg question.

### Contradictions

- The component record says affective_n = z(LIWC i + negemo - posemo) + z(LLM facets). The code (multiple verifiers; emit_affective_n.py lines 42 and 59) has LIWC as a convergent anchor only (r +.24), not a component. The docstring at lines 3-4 is stale.
- The 'LIWC' anchor is a hand-written regex approximation (FPS, NEG and POS patterns), not the LIWC dictionaries, despite the LIWC framing.
- The brain records raw r -.11 (E-cleaned -.04, not significant) between N and RKH-ES at n 147. That is weak agreement with the very reference the lexical pipeline inherits N = -ES from.
- The receptivity layer labels the N slot as Byrne's channel, but Byrne never measured the mechanism, and the project's own H5 test is null (partial r +.036, n = 441).

### Gaps

- There is no gold N criterion, and the field offers none (Naz 2025, UNVERIFIED, confirms the dataset gap). The nearest available criterion is the Senate insider panel, on which N passes. Fill: a pre-registered look of affective_n against insider N with k>=2 rows and party strata, on overlapping senators.
- Yielding under pressure has never been measured behaviourally. Fill: a prospective test of affective_n against an observed pressure outcome (for example, position change after targeted contact), in Yang's with/without design.
- No US test of interview selection on temperament. Fill: correlate interview coverage with non-text proxies (reactivity_z) after leadership and office controls.
- No gender or chamber audit. Fill: regress affective_n on gender and chamber, partialled on party, and report stratified.

### Paul's position vs the evidence

Paul has no annotation on this component and cites only Ramey. Ramey supplies N = -ES and calls ES's link to anxiety words somewhat weaker than for O, C and E (a face-validity remark). Nothing in the evidence validates affective N as neuroticism. The gate (5/5) tests construct integrity only, as the component record itself says, and the literature agrees with that limit.

### Open questions

- Should affective_n be residualized on gender and chamber in addition to E and party?
- Does keeping Anxiety and Volatility apart from Depression and pessimism change member ordering? This would need its own prereg.
- Is interview-derived N range-restricted because neurotic members give fewer interviews?
- Should the N slot in receptivity continue to cite Byrne, given that the mechanism is unmeasured and H5 is null?

### Read first

- [[koutsoumpis-2024-kernel-truth-text]]
- [[byrne-2014-chokes-under-pressure]]
- [[amsalem-2018-effect-politicians-personality]]
- [[osnabrugge-2021-playing-gallery-emotive]]
- [[aroyehun-2026-politics-of-feelings]]
- [[peters-2024-language-models-infer]]

## Agency-communion persona channel

**What it computes.** persona_z = 0.60 z(communion) - 0.40 z(agency) from LLM few-shot cue classification (strong 1.0, weak 0.5), blended at PERSONA_W 0.30.

**Method.** Functional gate 18/18. Party signature: communion D .099 vs R .049. Persona homophily is not earned (|r| <= 0.015).

**Code refs.** `fec/members/agency_communion_llm.py`, `fec/persona_homophily_validate.py`

**Assumptions.**
- Projected persona is what a lobbyist meets
- Communal rhetoric signals approachability

### Evidence

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
| [[decter-frain-2016-impressive-words-linguistic]] | 2016 | qualifies | Broad communion correlates r = -0.36 with monthly Gallup approval (Model 3 beta +0.04, n.s.) while the prosocial helping facet correlates r = +0.55 (Model 3 beta +0.14, p < 0.05). Agency r = -0.16 and Model 3 beta -0.11 (p < 0.05). Window Jan 1996 to Nov 2014, N = 206 House months. Pooled over the whole chamber, not stratified. | The paper is about an aggregate chamber-month audience outcome (approval of Congress), not member-level susceptibility to lobbyists. The facet lesson transfers only as a construct-definition check: a pooled communion score can mix facets with opposite signs. Ecological inference from chamber months to individual members is not licensed. | moderate | VERIFIED-SOFTENED |
| [[decter-frain-2016-impressive-words-linguistic]] | 2016 | suggests-extension | Agency and communion indexes are built from coder-weighted LIWC category densities: five blind coders, ICC agency 0.82 and communion 0.90, averaged into category weights (weights independent, r = -0.03). A 127-word prosocial dictionary is scored alongside. Robustness to the coding is reported only in the Supplement. | A dictionary path would be a convergence check (DE-062 second path), not a validity test. The coder weights were set on chamber aggregates, so member-level densities on short self-referential samples would be noisy. The paper's ICCs describe agreement on category weights, not on member-level scores. | weak | VERIFIED-SOFTENED |
| [[hargrave-2022-longer-conforming-stereotypes]] | 2022 | qualifies | Communal styles (human narrative, affect, positive emotion) were higher among female MPs early in the period. Positive emotion and affect show no consistent significant gender gap by the latest years; the human narrative gap narrows gradually but is not stated to reach zero. For six of eight styles gender explains less between-MP style variation at the end of the period than at the start. For the converging styles the share of MP-level variation explained by gender falls 29 to 60 percent comparing pre-2007 with post-2007; the converging styles are not named individually. Window: UK Commons May 1997 to March 2019. Denominator: MP-by-session variation (R2-style metric). | The paper gap is on UK floor speech using embedding-expanded dictionaries; the system uses Birkenmaier-Lechner LLM cue shares on US congressional sentences, a different instrument. Transfer holds only if US cue shares show a gender gap. Check: persona_susc_z against the legislators.csv gender column, partialled on party. Stays UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED |

### Assessment

## Agency/communion persona channel

**Evidence weight.** 14 bearings (10 distinct papers): 3 VERIFIED, 8 VERIFIED-SOFTENED, 3 UNVERIFIED. The channel has the most direct method source in the system. The evidence supports the *design* and the *construct definitions*. It does not establish the instrument's accuracy, or the susceptibility signs the channel applies.

**Method source (Birkenmaier & Lechner 2025, VERIFIED-SOFTENED, strong).**
- Few-shot codebook prompting on positive poles reaches macro-F1 .77 (DeepSeek-V3) and .76 (GPT-4o) on strong-balanced human labels of German sentences. These are the only configurations passing the functional test units.
- The system ports the codebook and the positive-pole rule, weights strong cues 1.0 and weak cues 0.5, and runs an 18-unit self-test (pass bar 16 of 18) on claude-sonnet-5.
- The 18 units were builder-written in English, and there is no human-labelled sentence test. **The system's macro-F1 is unknown.** Only the design and the functional-gate idea transfer.
- Strong-only cues perform slightly better in the paper (.77 against .76, a gap inside the SDs, so direction only, not magnitude). A strong-only score can be computed from the existing cache at no API cost.

**Party signature (Birkenmaier, VERIFIED).** Communion share falls from left to right in six German parties (LINKE 19%, AfD and FDP about 6%), with no coefficients given. This supports the system's D .099 vs R .049 communion signature as convergent validity. It also means the persona score carries party, so any pooled use is UNSTRATIFIED across D and R. One verifier could not locate the .099/.049 figures in the repo.

**Construct problems.**
- Lechner 2025 (VERIFIED): the Assertiveness facet is not unidimensional. The 'power-hungry, claims leadership' item loaded reversed; observer alpha was .40 and omega .48 (N = 211). The system's self-test treats a power-claiming sentence as gold agency, and the agency share pools those sentences. This is a plausibility warning measured in survey ratings, not in text.
- Decter-Frain 2016: broad communion correlates r -.36 with monthly congressional approval, while the prosocial facet correlates +.55 (206 House months, 1996 to 2014, UNSTRATIFIED chamber aggregate). A pooled communion score can mix facets with opposite signs. The ecological inference to members is not licensed.
- Hargrave 2022: the UK gender gap in communal styles narrows across 1997 to 2019. There is no gender term in the persona code.

**Signs inside the channel.**
- Byrne 2014 (VERIFIED): the agreeableness result is self-report BFI A, exploratory and significant only in an uncontrolled experiment. Citing Byrne for 'communal rhetoric means approachable' is a second inferential step the paper does not cover.
- Palm 2025 (UNVERIFIED): extraversion is above average in susceptibility, which runs against the negative agency sign. Lead only.
- PERSONA_W 0.30 is Paul's ruling and is not in question here.

**Homophily.** Selfhout 2010 and Celli 2012 (both UNVERIFIED) report trait homophily in other populations and relations. The system's measured null (|r| <= .015) outranks them, so they are not grounds to reopen homophily edge weighting. The one transferable point: the null holds for the cross-sectional test that was run, not for homophily in general.

### Contradictions

- The channel's directional prior (communal means susceptible; agentic means resistant) is sourced to Byrne, whose result concerns self-reported trait A, not projected rhetoric (VERIFIED). Palm 2025 (UNVERIFIED) points the agency sign the other way.
- The self-test treats power-claiming sentences as gold agency, while Lechner (VERIFIED) shows power-claiming loads reversed against leadership-taking in observer ratings.
- Paul's 'validated models' are GPT-4o and DeepSeek-V3. The system runs Claude Sonnet, which the paper did not test.

### Gaps

- No human-labelled English congressional sentence sample exists. Fill: hand-code a held-out cue sample that was not used to write the codebook (Ornstein 2024's requirement), and estimate the system's macro-F1 and agreement.
- There is no evidence that communal persona predicts meeting acceptance or cosponsorship with outside requesters (the component's empirical question). Fill: a prospective, party-stratified test against an observed access or cosponsorship outcome.
- No gender audit. Fill: persona_susc_z against legislators.csv gender, partialled on party, reported stratified.

### Paul's position vs the evidence

Paul calls this the most direct transfer from his notes (pre-built codebooks and node design), and the evidence agrees for the codebook and the construct definitions. The two weaknesses he flagged are both confirmed:
- *Trait-to-node conversion.* No paper supports the communion-to-susceptibility sign as applied.
- *Ambiguous statements.* Lechner's Assertiveness split and Decter-Frain's facet sign reversal show that pooled agency and communion shares can mix opposite-signed content.

His model-specific validation note does not cover the Claude instrument the system actually runs.

### Open questions

- Does a strong-only persona score, computed from the existing cache, change member ordering?
- Do power-claiming and leadership-taking agency cues split in congressional text?
- Should communion be restricted to the prosocial-helping facet, given Decter-Frain's opposite-signed facets? This would need a prereg.

### Read first

- [[birkenmaier-2025-measuring-politicians-public]]
- [[lechner-2025-mapping-political-personality]]
- [[ornstein-2024-train-your-stochastic]]
- [[byrne-2014-chokes-under-pressure]]
- [[decter-frain-2016-impressive-words-linguistic]]
- [[hargrave-2022-longer-conforming-stereotypes]]

## Interview weighting (C_INT_W) and gated shrinkage (P6)

**What it computes.** Conscientiousness aggregates interview chunks at 2x. Members with fewer than 8 measurements are shrunk.

**Method.** The P6 reliability claim (.748 -> .831) was retracted as double-shrink inflation; the change was kept on external criterion gains.

**Code refs.** `big5_llm.py (C_INT_W)`, `fec/big5_improvements_eval.py --p6`

**Assumptions.**
- Shrinkage is judged only on external criteria, not split-half reliability

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[peters-2024-language-models-infer]] | 2024 | context | Correlations rise with message volume from one 20-message chunk to ten chunks, but a substantial share of variance is captured with 20 messages and most correlations are close to their maximum well before 200 messages. Conscientiousness levels off earliest (Sec 3.1, Fig 2). | The saturation volume comes from personal, cue-dense posts. Political text carries few personality cues, so saturation probably needs far more text. The paper's curve should not be used to relax the n_chunks gate. 20 messages at 17.10 words each is about 342 words, one 1,200-word chunk is much larger. | weak | VERIFIED-SOFTENED |

### Assessment

## Interview weighting (C_INT_W) and gated shrinkage (P6)

**Evidence weight.** 1 bearings (1 distinct papers): 0 VERIFIED, 1 VERIFIED-SOFTENED, 0 UNVERIFIED. Weak context only. **This is effectively a GAP.**

**Peters & Matz 2024.** LLM-to-self-report correlations rise with message volume, from one 20-message chunk (about 342 words) to ten chunks, then approach their maximum well before 200 messages. Conscientiousness levels off earliest. The saturation point comes from personal, cue-dense posts. Political text carries few personality cues (Birkenmaier reports 89 to 94% of political sentences carry no agency or communion cue), so saturation likely needs far more text. **The paper's curve must not be used to relax the 8-chunk gate.**

**Indirect evidence from other components.**
- Ramey's precision band (CI width about .3 or less) assumes about 5,000 or more words per member-year.
- Pennebaker's 4-week retest is r .24 for psychological-process word categories.
- Both suggest thin-corpus estimates are noisy, which supports shrinking them.

**The 2x interview weight for C (C_INT_W).** No literature addresses it. Osnabrugge's venue effects suggest genres differ systematically, so up-weighting one genre changes what is measured, not only its precision.

**The P6 retraction.** The project retracted its split-half reliability claim (.748 to .831) as double-shrink inflation, and judges shrinkage on external criteria only. That is consistent with the general measurement principle the evidence implies; no bearing contradicts it.

### Contradictions

- (none recorded)

### Gaps

- No study tests shrinkage thresholds for politician text. Fill: a subsampling study. For members with large corpora, draw k chunks at random and compute correlation with a held-out external criterion as a function of k. Fit the Spearman-Brown or saturation curve, then set the gate from where the curve flattens. Pre-register; one look.
- No evidence for the 2x interview weight on C. Fill: compare genre-weighted against unweighted C on a pre-registered held-out criterion. The votes_cast criterion is already spent and should be stratified on electoral status.
- No evidence on whether shrinkage toward 0 biases the scores of low-coverage groups (junior members, those with few interviews). Fill: an audit of how shrinkage falls by seniority and chamber.

### Paul's position vs the evidence

Paul records 'no literature', and the evidence set confirms it: one weak contextual bearing that warns against relaxing the gate. His project's rule, judging shrinkage only on external criteria, is the defensible position, and nothing contradicts it.

### Open questions

- What k does a subsampling curve on congressional text recommend, and is 8 chunks of about 1,200 words above or below it?
- Does the 2x interview weight on C survive a held-out criterion that was not used to tune it?

### Read first

- [[peters-2024-language-models-infer]]
- [[ramey-2016-measuring-elite-personality]]
- [[pennebaker-2003-psychological-aspects-natural]]

## Across components

**1. The system measures perceived public persona, not trait.**
- Koutsoumpis (text tracks observers: mean R2 38.5% against 5.07% for self-report), Mairesse (observer models beat baseline; self-report models do not) and Cao (public-figure perception tracks likability and reputation) converge.
- Lexical, LLM, persona and gold-panel convergence can all reflect the same observable public cues.
- Birkenmaier & Lechner's own title, 'public personality', frames the construct this way.
- *Proposal for Paul:* label all trait outputs as perceived public persona. Convergence among channels is not trait validity.

**2. Party is a fault line in four components.**
- Gold panels: party-skewed extremes, and no party term in the primary validators.
- Persona: party signature D .099 against R .049.
- Affective N: residualized on a Republican dummy only.
- Lexical O: checked against an expected party sign.
- Pooled figures across D and R should carry strata or an UNSTRATIFIED label.
- Gender and chamber are unaudited in affective N and persona (Aroyehun, Hargrave and Peters bearings).

**3. Electoral status contaminates two components.**
- Fouirnaies (VERIFIED, state legislatures, within-person) affects both the gold C criterion (votes_cast, rho .155 PASS) and diligence_z. In the receptivity layer, diligence_z also enters susceptibility with a sign opposite to the retirement incentive.
- One stratification, on retiring and running for other office, would serve both.

**4. Genre and venue mixing appears in three components.**
- Lexical pools social, CREC and interview text; genre transfer of affective N failed; C_INT_W up-weights interviews.
- Osnabrugge (venue shifts within MP), Decter-Frain (period co-movement, UNSTRATIFIED) and Pennebaker (retest .24 for psychological-process words) point the same way. Genre should be modelled, not pooled.

**5. Records and citations are stale (bookkeeping; needs fixing before any external claim).**
- The .350 held-out CV against RKH was not found.
- The refit-to-RKH weights are described as candidate-only, and the v2 builder learns weights against behavioural criteria.
- The affective-N formula in the record keeps a LIWC component the code removed.
- The 'LIWC' anchor is a regex approximation.
- The Cao & Kosinski venue and figures in `big5_llm.py` are wrong.
- The Ramey year is inconsistent (2016 against 2019).
- RKH is credited with a participation criterion it does not contain.
- The Feingold academic row is silently dropped.
- The Pink 'balanced keying' docstring is wrong (3:1).
- Full-sample gold weights feed the shipped composites, so the gold panels are no longer untouched.

**6. Methods that cut across components.**
- Several bearings (Yang 2023's with/without trait-block design, Ornstein 2024's held-out hand labels, Peters' rep-averaging) are reusable across the LLM, persona and disposition components.

**7. Guardrails held.**
- No bearing argues for the two rejected SNA proposals (dropping regular cosponsorship edges, per-edge volume normalization).
- Harbridge-Yong's offered-to-attracted association does not validate reciprocity (D-104 to D-108 stand).
- The homophily leads (Selfhout, Celli) are UNVERIFIED and are outranked by the system's own measured null. They are not grounds to reopen homophily routing.
- LDA plays no role in these components.

## Research agenda

1. Fix the bookkeeping first. These are records-only edits for Paul to approve:
   - Locate or withdraw the .350 RKH held-out CV.
   - Update the affective-N record to say LIWC is an anchor only, built from a regex approximation.
   - Correct the Cao & Kosinski venue and figures in big5_llm.py.
   - Resolve the Ramey 2016/2019 year.
   - Drop the RKH-participation attribution in behavioral_dispositions.py.
   - Make the Feingold row failure loud rather than silent.
   - Correct the Pink 'balanced keying' docstring.
2. Count how many current-roster Senate panel matches are k=1 insiders. Then decide (Paul's call) whether to spend a pre-registered k>=2 look, since the Mondak appendix excludes k=1 senators from all analyses.
3. Report gold-panel correlations stratified by party, or label them UNSTRATIFIED. Add an electoral-status stratum (retiring, running for other office) to any external quote of the C votes_cast PASS and to diligence_z.
4. Decide whether shipped composites should stop using full-sample gold-fitted weights, so the Senate panels stay clean for future looks.
5. Hand-code a held-out English congressional sentence sample for agency/communion cues. Estimate the Claude instrument's macro-F1 against the Birkenmaier benchmark (.76 to .77).
6. Compute a strong-only persona score from the existing cache, at no API cost, and check whether member ordering changes. Separately, test whether power-claiming and leadership-taking agency cues split in text.
7. Test de-identification efficacy:
   - Run the coded --named probe.
   - Extend scrubbing to committee, district and bill names.
   - Compare trait shifts against member prominence.
8. Measure Pink between-rep reliability and consider averaging reps. Test for LLM acquiescence given the 3:1 item keying.
9. Audit affective_n and persona_susc_z for gender and chamber effects, partialled on party. Test whether interview coverage selects on temperament (Amsalem mechanism) using reactivity_z after office controls.
10. Model genre explicitly in the lexical pipeline: per-genre z-scores, or an MTMM/invariance test across floor, interview and social text. Run a per-year de-meaning test for period drift.
11. Run a test-retest study of cooperation_v2 across consecutive Congresses, by chamber and majority status. Decide whether it adds anything over bpf (rho .888).
12. Run a thin-corpus subsampling study (criterion correlation as a function of chunk count) to set the P6 gate, and a held-out test of the 2x interview weight on C.
13. Pre-register the prospective H1/H2 comparison of behavioural dispositions against text traits on lobbying responsiveness. Use Yang 2023's design: the same learner with and without the trait block, a chronological split, and paired tests.
14. Decide (Paul's ruling) whether to relabel all trait outputs as perceived public persona rather than personality, given the observer-cue evidence across components.
