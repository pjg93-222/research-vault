---
type: theme
name: "Machine Learning & Sentiment Analysis"
paper_count: 24
tags: [theme]
---

# Machine Learning & Sentiment Analysis

**Scope.** The theme file holds 24 extracted works. Paul's own annotated bibliography for this category lists 8 citations. Only 4 have his annotation text: Birkenmaier & Lechner, Kim et al., Peng et al. and Ramey et al. Of those 4, only Ramey ([[ramey-2016-measuring-elite-personality]]), Birkenmaier ([[birkenmaier-2025-measuring-politicians-public]]) and Kim ([[kim-2025-measuring-interest-group]]) appear here, and only as related links, not as extracted works. Peng et al. and Alkazzi & Okumura are absent. Three of the 24 works are in Paul's bibliography: [[steinertthrelkeld-2018-twitter-data]], [[joo-2022-image-data-automated]] and [[taherdoost-2023-artificial-intelligence-sentiment]]. All three were citation-only there. So most of this synthesis comes from the extractions, not from Paul's words.

**Paul's argument.** Paul says machine learning and NLP on public political text (floor speech, press statements, lobbying filings) can produce actor-level attributes: personality, position and emotion. Those attributes would sit on the nodes of an SNA graph and be used for political intelligence, such as lobbying strategy, industry alignment and how to pitch a member. The gap he names most often is the **conversion step**: how a model's output becomes a node attribute or vector. He raises it for Birkenmaier, Peng and Ramey.

**What the 24 works say, in one paragraph.** The literature splits into four measurement programmes plus a group of data-source and application papers:
- (1) Big Five from text, the personality-computing lineage.
- (2) Voice and audio as data.
- (3) Stance detection.
- (4) Sentiment and tone dictionaries.

Across all four, the strongest shared result is a **ceiling**:
- Closed-vocabulary text explains about 5% of self-reported trait variance ([[koutsoumpis-2024-kernel-truth-text]]).
- Text-based trait recognition sits at about r 0.30 ([[celli-nd-twenty-years-personality]]).
- Voice adds at most low double digits of variance in students ([[marrero-2022-evaluating-voice-samples]]).

These measures improve sharply when the criterion is an **observer** rather than a self-report, and when the dictionary or model is **domain-built** rather than general. In stance, the works agree on two points: stance is not sentiment ([[burnham-2023-stance-detection-practical-guide]]), and transformer, NLI or LLM readers beat bag-of-words on political text. They disagree on whether an LLM should be the primary labeller. Melange's own small-sample results run against the literature's "transformer wins" headline. Those results are LinearSVC 0.816 versus transformer 0.757 macro-F1, and DEBATE 0.839 / kappa 0.742 on n=87 held-out, member-grouped. Every system-side figure is a project report, not a literature finding.

**What this means for the systems.** The theme mostly **qualifies** existing Melange choices rather than proposing new ones:
- Lexical Big Five is retired from receptivity.
- Audio stays display-only.
- DEBATE NLI sets stance direction, and LSD2015 is used for intensity only.

It also exposes a few actual errors and some open validation debts. Those are listed under "What it means for the systems".

## Strands

### Big Five from text: the personality-computing lineage and its ceiling

**Lineage.** [[mairesse-2007-automatic-recognition-personality]] is the root. It runs LIWC, MRC and prosody features through SMOreg/Naive Bayes/RankBoost on 2,479 student essays (self-report) and 96 EAR speakers (self-report plus 18 observers). Ramey, Klingler and Hollibaugh ran these pretrained models on congressional floor speech, and Melange's lexical reader is fitted to their RKH estimates. [[celli-nd-twenty-years-personality]] confirms that this is still the field's core. Among 45 text systems, SVM was the most common learner (20) and psycholinguistic features the most common feature type (16).

**Agreement: the signal is small and depends on the criterion.**
- [[koutsoumpis-2024-kernel-truth-text]] pools 31 samples (N=85,724). The 52 shared LIWC categories explain a mean 5.1% of **self-reported** trait variance, with the strongest |rho| at .08-.14. They explain 38.5% of **observer** ratings, but that figure rests on only 5 samples (n=1,637). Only 20 of 260 trait-by-category markers agree in direction across the two criteria.
- [[mairesse-2007-automatic-recognition-personality]] finds the same asymmetry. On EAR conversation, observer-rated traits reach 73-74% binary accuracy for E and ES, while no self-report model beats the baseline.
- [[celli-nd-twenty-years-personality]] puts text recognition at r of about 0.30 against a 0.133 baseline. Computer perception reaches 0.30 against 0.38 for human judges.
- [[azucar-2017-predicting-personality-traits]] reports higher pooled r of .29-.40 across 16 lay-user social-media studies. The authors admit these are optimistic, because each study contributed its best model or strongest feature. Heterogeneity is I² ≥ 93 for every trait, and the result is UNSTRATIFIED by platform.
- Agreeableness is the least recoverable trait in Azucar, and Emotional Stability has the lowest inter-observer agreement in Mairesse.

**Disagreement: architecture.**
- [[yang-2023-getting-personal-deep]] (DeepPerson) reports AUC 75.0 versus 64.6 for the best comparator on PANDORA, with feature-based rows lowest. LIWC-type concept embeddings still add about 2 pp inside the deep model.
- [[naz-2025-machine-deep-learning]] says transformers and Bi-LSTMs lead, while classical TF-IDF stays competitive on small labelled sets. Its own experiment is accuracy-only on MBTI-labelled data, with no split reported and screening counts that do not reconcile.
- [[kumari-nd-five-personality-predictions]] is a design sketch with no data or results. Its follower and tweet-count features are volume proxies of the kind the system already rejects.

**Instrument side.** [[rammstedt-2025-going-global-language]] is the only questionnaire paper. Its comparability rests on the translation procedure, not on data. Its one transferable point is that small item-wording changes are treated as material, which matches the itemset versioning of the LLM reader.

**Unsettled.** No work here has an **elite** self-report criterion. Every effect size comes from students or lay users, so none of them is a like-for-like benchmark for Melange's held-out r .350 against RKH.

Works: [[mairesse-2007-automatic-recognition-personality]], [[koutsoumpis-2024-kernel-truth-text]], [[azucar-2017-predicting-personality-traits]], [[celli-nd-twenty-years-personality]], [[naz-2025-machine-deep-learning]], [[yang-2023-getting-personal-deep]], [[kumari-nd-five-personality-predictions]], [[rammstedt-2025-going-global-language]]

### Voice and audio as data: trait, state and identity

The four works ask three different questions, and they should not be pooled.

**Trait.** [[marrero-2022-evaluating-voice-samples]] uses 614 students, about 51 s of speech each, XGBoost with SHAP selection and 100-fold CV R². The joint feature set explains 10.67% of trait variance, vocal features 6.24% and LIWC 3.13%. E, C and N reach 10-20%, and gender is predicted at over 90%. The headline claim is super-additivity of voice and text. [[salam-2022-learning-personalised-models]] runs UDIVA dyadic video through architecture search. Splitting models by gender lowers average MSE from 0.818 to 0.690, and age barely helps. The evidence is thin: one test split, with no test N or intervals.

**State.** [[dietrich-2019-pitch-perfect-vocal]] measures arousal, not trait. It z-scores mean F0 within each speaker across 74,158 House floor speeches (2009-2014). Women members speak about +0.09 to 0.11 SD above baseline when they mention women, and partisans about +0.29 to 0.38 SD on topics their party owns. The day-level response models are explicitly non-causal.

**Identity and pipeline.** [[mestre-2026-potential-pitfalls-audio]] is a methods paper on 45 presidential and VP debates (1960-2020):
- Aeneas forced alignment gets 85.3% percentage agreement, with about 5% of pairs not aligned at all.
- wav2vec2 embeddings reach 0.914 speaker-ID accuracy across 156 speakers, against 0.081 for an MFCC CNN.
- Off-the-shelf emotion models are biased by gender and pitch.

**Agreement.**
- Within-speaker normalisation is required (Dietrich, Mestre).
- Speaker sex confounds acoustic features (Marrero's gender result, Salam's gender split, Mestre's pitch ranking).
- Extraversion carries the most vocal cue. Mairesse and Celli say the same: prosody helps E most.

**Disagreement.** The only clear disagreement is between Marrero's super-additivity and Melange's own replication. At N=90 legislators, vocal features added +0.001 over linguistic for E against the Pink criterion. That is a project report, not a literature finding.

Works: [[marrero-2022-evaluating-voice-samples]], [[dietrich-2019-pitch-perfect-vocal]], [[mestre-2026-potential-pitfalls-audio]], [[salam-2022-learning-personalised-models]]

### Stance detection: supervised vs NLI vs LLM

**Framing.** [[burnham-2023-stance-detection-practical-guide]] (published in PSRM 2025) frames stance as entailment toward a proposition, kept separate from sentiment. On about 2,135 Trump-approval items, scored by MCC:
- Bag-of-words gets 0.49-0.51 and supervised transformers 0.60-0.68.
- Zero-shot NLI gets 0.40 with DeBERTaV3 Base and 0.65 with Large.
- GPT-4 gets 0.78-0.80.
- Human coders disagree more when the target is not named: 34.0% versus 19.5% conflicting codes.
- NLI degrades most in that low-context case.

**Agreement that transformers or LLMs beat classical models on political text.**
- [[ornstein-2024-train-your-stochastic]]: GPT-3 few-shot reaches 88.4% sign accuracy on 945 SCOTUS tweets, against 64.3% for TweetNLP and 38.3% for the best dictionary. This is sentiment, not stance.
- [[dai-2025-large-language-model]] (FOLAR): small in-target gains of +1.7 and +2.0 F_avg, and larger zero-shot VAST gains of +4.2 and +6.2.
- [[b-2025-comparing-fine-tuned]] (Khan et al., Roman Urdu): RoBERTa 0.97 against RF 0.95. The comparison is not like-for-like, because RoBERTa was scored on a different, augmented test set (support 6,699 against 1,675).

**Disagreement: the role of the LLM.**
- Burnham says NLI should be the reproducible default, with LLMs used for augmentation or validation, not as primary labels. GPT-4 cost $12.52 per run and cannot be archived.
- Ornstein treats few-shot LLM labels as a usable measure, provided a tune-then-check validation is run. It also documents GPT-4 overconfidence: more than 70% of posts get over 99% on one label. It warns that its GPT-3 models were retired in January 2024, so the results cannot be reproduced.
- Dai fuses three LLMs to hedge single-model bias. Its ~5.8-point fusion ablation compares fusion against a single GPT-3.5, so fusion is confounded with the choice of LLM.

**Shared weakness.** The benchmarks are tweets and short sentences, not long multi-actor press releases. Khan, Dai and Naz report no variance across seeds or splits.

Works: [[burnham-2023-stance-detection-practical-guide]], [[b-2025-comparing-fine-tuned]], [[dai-2025-large-language-model]], [[ornstein-2024-train-your-stochastic]]

### Sentiment and tone dictionaries: domain beats general

**Three works agree that general lexicons misfire on specialised text.**
- [[rice-2018-corpus-based-dictionaries]] seeds GloVe on the domain corpus. It gets 79.5% held-out accuracy on IMDB, against about 70% for LIWC and AFINN. It needs about 5,000 or more documents. On Supreme Court opinions, justice-level polarity ranks correlate only 0.29 with LIWC and 0.16 with AFINN, so the choice of dictionary reorders the units.
- [[haselmayer-2017-sentiment-analysis-political]] builds a crowd-rated negativity dictionary from 12,713 sentences, 10 ratings each, for about 2,000 EUR. It reaches r 0.65 with experts at 84% coverage, against r 0.13 and 0.19 for translated general dictionaries. Its regression keeps the expert-coded signs but shrinks the effects (-0.13 versus -0.40).
- [[park-2025-automatic-construction-direction]] pairs direction words with direction-dependent nouns ("cost decrease" is positive). This lifts Loughran-McDonald weighted F1 from about 0.60 to 0.71-0.82 on headline-length financial sentences. It stays below 0.5 on SemEval and FiQA, and fine-tuned RoBERTa leads by a wide margin.

**Agreement.**
- Ornstein's 38.3% dictionary result points the same way.
- Burnham adds that document polarity cannot locate its target, a limit Rice concedes in its own text.

**The outliers.**
- [[zhang-2022-data-assisted-social]] applies the general Affin lexicon to 882,371 retail tweets with no domain check, no holdout and no metrics. It also mis-cites Affin.
- [[taherdoost-2023-artificial-intelligence-sentiment]] is a bibliometric scoping review. It never separates sentiment from stance and never addresses target-dependent valence. Its one useful point is borrowed from Alantari et al.: neural models predict well but diagnose poorly, and dictionaries do the reverse.

Works: [[rice-2018-corpus-based-dictionaries]], [[haselmayer-2017-sentiment-analysis-political]], [[park-2025-automatic-construction-direction]], [[taherdoost-2023-artificial-intelligence-sentiment]], [[zhang-2022-data-assisted-social]]

### Platform data, images and persuasion applications

These works sit on the edge of the theme. Two are 'X as data' tutorials by the same author:
- [[steinertthrelkeld-2018-twitter-data]] gives 2016 API limits: a 1% stream sample and a 3,200-tweet cap. It also gives elite coverage (every Senator and 430 House members) and the caveat that author metadata reflects request time, not post time.
- [[joo-2022-image-data-automated]] covers images: label-API chi-squared tests on 15,647 candidate photos, and a duplication-rate framing measure on protest images.

The framing hypotheses in Joo hold in 3 of 5 events. There is no correction for testing about 1,700 labels, and the duplicate threshold of 0.2 was picked from one histogram.

[[simchon-2024-persuasive-effects-political]] is the one application paper. Across four UK Prolific experiments, a mismatch between ad openness and reader openness lowers *perceived* persuasiveness. Effects are small: b -0.07 to -0.08 per SD of mismatch with real ads, -0.03 with ChatGPT rewrites, and not significant with GPT-3 (p .057). The outcome is self-rated persuasiveness, not attitude change. The paper puts the effect in a **message-by-trait interaction**. [[celli-nd-twenty-years-personality]] lists this same personality-aware election persuasion as an ethical threat.

Works: [[steinertthrelkeld-2018-twitter-data]], [[joo-2022-image-data-automated]], [[simchon-2024-persuasive-effects-political]]

## Agreements and debates

**1. Does the criterion decide the answer? (self-report vs observer)**
- [[koutsoumpis-2024-kernel-truth-text]]: LIWC explains about 5% of self-reported trait variance and about 38.5% of observer ratings. The observer figure rests on only 5 samples.
- [[mairesse-2007-automatic-recognition-personality]]: observer ratings are learnable in conversation and self-reports are not.
- [[celli-nd-twenty-years-personality]] places both on Brunswik's lens model.

All three agree that observer convergence is easier to get. Koutsoumpis warns that this convergence can reflect raters and models using the **same cues**, which is cue utilization, not validity. So a high correlation with an observer panel does not show that the trait itself is measured. Melange's checks against the Senate expert and insider panels fall under this caveat.

**2. Are transformers worth it on small labelled sets?**
- For: [[burnham-2023-stance-detection-practical-guide]], [[ornstein-2024-train-your-stochastic]], [[yang-2023-getting-personal-deep]], [[park-2025-automatic-construction-direction]] and [[dai-2025-large-language-model]].
- Partly against: [[naz-2025-machine-deep-learning]] (classical stays competitive on small data).
- [[b-2025-comparing-fine-tuned]] claims a transformer win, but its test sets are unequal.

Melange's own numbers sit with Naz: LinearSVC 0.816 against transformer 0.757 macro-F1 on the augmented press set. That is a project report on a small held-out set. Burnham cites Laurer et al.: about 500 examples let language models win. That is consistent with the system's finding that fine-tuning DEBATE on 552 labels fixed zero-shot implicit-oppose recall (F1 0.08).

**3. Should an LLM be the primary labeller?**
- Burnham: no. Use NLI for reproducibility, and LLMs only to augment or validate.
- Ornstein: yes, with a tune-then-check validation, using the probability vector rather than the argmax label. But GPT-4 is overconfident and the GPT-3 models are retired.
- Dai fuses several LLMs, which concedes the single-model bias problem.

This is a live disagreement. It bears directly on the Claude pre-labels that the DEBATE reader distils.

**4. Does voice add trait information beyond text?**
- [[marrero-2022-evaluating-voice-samples]]: yes, super-additively, in students.
- [[mestre-2026-potential-pitfalls-audio]]: emotion models are biased and recording conditions confound cross-speaker comparison.
- [[dietrich-2019-pitch-perfect-vocal]] measures state (arousal), not trait, and needs about 120 speeches per speaker.

Melange's N=90 legislator replication did not reproduce the super-additivity. That is a project result, not a literature result.

**5. Personalise by subgroup or not?**
- [[salam-2022-learning-personalised-models]]: gender-split models win on self-report.
- [[azucar-2017-predicting-personality-traits]]: demographics raise "accuracy" for A and N only because group membership correlates with self-reported traits.

Read together, a demographic gain can be confound capture rather than measurement. Melange's affective-N gate removes that signal on purpose.

**6. Trait as main effect or interaction?**
[[simchon-2024-persuasive-effects-political]] finds the effect in message × trait congruence. A main-effects susceptibility composite cannot represent this. The evidence is small, comes from UK lay raters, and uses a perceived-persuasion outcome, so it is a design contrast, not a recommendation.

**Not present.** No work in this theme argues for either of the two SNA proposals this project rejected: dropping regular cosponsorship edges, or per-edge volume normalization. There is nothing to flag on that front.

## Methods

**Dominant methods**
- **Closed-vocabulary feature rates + linear/SVM learners** (LIWC, MRC; SMOreg, SVM, Naive Bayes): [[mairesse-2007-automatic-recognition-personality]], and catalogued by [[celli-nd-twenty-years-personality]] and [[naz-2025-machine-deep-learning]]. They are cheap and interpretable, but the ceiling is low ([[koutsoumpis-2024-kernel-truth-text]]: about 5% of self-report variance).
- **Fine-tuned transformers and NLI zero-shot:** [[burnham-2023-stance-detection-practical-guide]], [[b-2025-comparing-fine-tuned]], [[yang-2023-getting-personal-deep]]. NLI results depend on model size (MCC 0.40 for Base, 0.65 for Large) and on whether the target is named in the text.
- **Few-shot / chain-of-thought LLMs:** [[ornstein-2024-train-your-stochastic]], [[dai-2025-large-language-model]]. Their weaknesses are poor calibration, model retirement (results cannot be reproduced), cost and single-vendor bias.
- **Domain-built dictionaries:** corpus embeddings in [[rice-2018-corpus-based-dictionaries]], crowd ratings in [[haselmayer-2017-sentiment-analysis-political]], PMI direction pairs in [[park-2025-automatic-construction-direction]]. They are transparent and validate well in-domain. They fail on long multi-actor texts and cannot tell which target the sentiment is about.
- **Within-speaker acoustic normalisation and pretrained speech embeddings:** [[dietrich-2019-pitch-perfect-vocal]], [[mestre-2026-potential-pitfalls-audio]], [[marrero-2022-evaluating-voice-samples]].
- **Random-effects meta-analysis:** [[koutsoumpis-2024-kernel-truth-text]], [[azucar-2017-predicting-personality-traits]].

**Recurring weaknesses**
- **Criterion problems.** Most trait criteria are lay self-report, and Mairesse and Yang use median splits. Celli recommends rank correlation instead of median splits. No elite self-report exists anywhere in the theme.
- **Weak evaluation designs.**
  - A single split with no CIs, seeds or variance: Khan, Naz, Salam.
  - Unequal test sets: Khan.
  - Best-model-per-study pooling: Azucar.
  - No correction for testing many labels: Joo, about 1,700.
  - A threshold picked from one histogram: Joo.
  - No significance test named: Dai.
- **Heterogeneity.** I² ≥ 93 in Azucar, and 96% of self-report effects heterogeneous in Koutsoumpis. Pooled figures are therefore UNSTRATIFIED across platform and genre, and should be quoted that way.
- **Reporting errors.** Screening counts do not reconcile in Naz (305 → 270 → 171 with 184 excluded), Taherdoost (133 vs 90) and Celli (129 vs 126). Simchon swaps the study 2a/2b sample sizes between text and figure. Zhang's equations are garbled and its Affin citation is wrong.
- **Genre mismatch.** Almost every benchmark is tweets, essays, reviews, headlines or debates. None is congressional press releases or floor speech scored against a human-coded gold standard, except Dietrich, which measures pitch, not stance.
- **No reliability disattenuation.** Validity coefficients are not corrected for reliability, except in Marrero and Koutsoumpis.
- **Causal identification.** Essentially none. Dietrich's response models and Yang's downstream forecasts are explicitly predictive or observational.

## How the works relate

- [[koutsoumpis-2024-kernel-truth-text]] — complements — [[mairesse-2007-automatic-recognition-personality]]: Both find closed-vocabulary LIWC/MRC signal is small for self-report and larger for observer ratings; Koutsoumpis quantifies the ceiling meta-analytically (5.1% vs 38.5%) that Mairesse found per-corpus.
- [[celli-nd-twenty-years-personality]] — cites — [[mairesse-2007-automatic-recognition-personality]]: Celli cites Mairesse 2007 (ref 179) as the foundation of personality computing.
- [[naz-2025-machine-deep-learning]] — cites — [[yang-2023-getting-personal-deep]]: Naz cites DeepPerson as the deep-learning artifact for text personality measurement.
- [[yang-2023-getting-personal-deep]] — extends — [[mairesse-2007-automatic-recognition-personality]]: DeepPerson embeds LIWC/MRC-type psychological concepts (the Mairesse feature families) inside a deep encoder and shows they still add about 2 pp AUC, while feature-only baselines rank lowest.
- [[naz-2025-machine-deep-learning]] — extends — [[azucar-2017-predicting-personality-traits]]: Naz covers ML/DL trait-detection methods after Azucar's May 2017 search cutoff; Azucar supplies the pooled effect sizes Naz lacks.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — [[azucar-2017-predicting-personality-traits]]: Parallel meta-analyses: closed-vocabulary LIWC against self/observer reports versus broader social-media footprints against self-report; both report very high heterogeneity.
- [[celli-nd-twenty-years-personality]] — same-method — [[naz-2025-machine-deep-learning]]: Both are reviews of personality-computing pipelines (SVM/LIWC through transformers) with similar method taxonomies.
- [[kumari-nd-five-personality-predictions]] — complements — [[azucar-2017-predicting-personality-traits]]: Kumari proposes Twitter-text Big Five prediction with no reported numbers; Azucar supplies the real pooled effect sizes for that design.
- [[kumari-nd-five-personality-predictions]] — builds-on — [[steinertthrelkeld-2018-twitter-data]]: Kumari's pipeline is a downstream use of the Twitter acquisition and processing that Steinert-Threlkeld teaches.
- [[rammstedt-2025-going-global-language]] — complements — [[koutsoumpis-2024-kernel-truth-text]]: Rammstedt documents self-report instrument comparability without validity data; Koutsoumpis supplies the text-to-trait validity side it omits.
- [[marrero-2022-evaluating-voice-samples]] — complements — [[mairesse-2007-automatic-recognition-personality]]: Both combine linguistic and prosodic features for Big Five; both find voice helps Extraversion most and self-report criteria are hard.
- [[marrero-2022-evaluating-voice-samples]] — complements — [[dietrich-2019-pitch-perfect-vocal]]: Marrero tests vocal trait information; Dietrich measures vocal state (arousal) within speaker. Different questions, shared acoustic toolkit.
- [[mestre-2026-potential-pitfalls-audio]] — same-method — [[dietrich-2019-pitch-perfect-vocal]]: Both normalise pitch to each speaker's own baseline; Mestre cites Dietrich for pitch tracking emotional state.
- [[salam-2022-learning-personalised-models]] — complements — [[marrero-2022-evaluating-voice-samples]]: Both predict self-reported Big Five from audio/behavioural features; both find speaker gender is a dominant signal (Marrero gender >90%, Salam gains concentrate in the gender split).
- [[salam-2022-learning-personalised-models]] — complements — [[azucar-2017-predicting-personality-traits]]: Salam's demographic-split gains and Azucar's demographics-raise-accuracy moderator together suggest demographic gains can be confound capture rather than trait measurement.
- [[celli-nd-twenty-years-personality]] — complements — [[marrero-2022-evaluating-voice-samples]]: Celli's modality cue tables (Extraversion carries the most audio-visual cues) agree with Marrero's trait-by-trait voice results.
- [[burnham-2023-stance-detection-practical-guide]] — same-method — [[b-2025-comparing-fine-tuned]]: Both benchmark bag-of-words classifiers against fine-tuned transformers for political-tweet stance; Burnham does it with MCC and repeated runs, Khan with a single split and unequal test sets.
- [[burnham-2023-stance-detection-practical-guide]] — contradicts — [[ornstein-2024-train-your-stochastic]]: Burnham recommends NLI as the reproducible default and LLMs only for augmentation or validation; Ornstein treats validated few-shot LLM labels as a primary measure.
- [[burnham-2023-stance-detection-practical-guide]] — complements — [[dai-2025-large-language-model]]: Dai's LLM-elicited logic rules target the implicit, low-context stance where Burnham shows NLI degrades most.
- [[ornstein-2024-train-your-stochastic]] — same-method — [[dai-2025-large-language-model]]: Both use LLM prompting for political stance/sentiment labelling; Dai adds logic-tensor training and multi-LLM fusion.
- [[dai-2025-large-language-model]] — complements — [[b-2025-comparing-fine-tuned]]: Same task family (stance in political tweets); Khan supplies the classical-versus-fine-tuned comparison Dai omits, Dai addresses the implicit-stance errors Khan reports but does not solve.
- [[naz-2025-machine-deep-learning]] — complements — [[b-2025-comparing-fine-tuned]]: Same classical-versus-transformer comparison; Naz says classical stays competitive on small labelled sets, tempering Khan's transformer-wins headline.
- [[burnham-2023-stance-detection-practical-guide]] — qualifies — [[haselmayer-2017-sentiment-analysis-political]]: Burnham's stance-is-not-sentiment argument limits what a tone dictionary can deliver: it can inform intensity, not direction toward a target.
- [[rice-2018-corpus-based-dictionaries]] — complements — [[haselmayer-2017-sentiment-analysis-political]]: Both argue for domain-built sentiment dictionaries, via corpus word vectors versus crowd ratings; Melange's legacy scorer cites both.
- [[park-2025-automatic-construction-direction]] — same-method — [[rice-2018-corpus-based-dictionaries]]: Both construct domain lexicons from corpus statistics (PMI on direction tags versus seed-word GloVe similarity).
- [[park-2025-automatic-construction-direction]] — complements — [[dai-2025-large-language-model]]: Clause-level polarity direction is one of FOLAR's named error types; Park's direction-aware pairs address it lexically.
- [[ornstein-2024-train-your-stochastic]] — supports — [[rice-2018-corpus-based-dictionaries]]: Ornstein's general dictionary at 38.3% against GPT-3 at 88.4% on tweet sentiment is consistent with Rice's argument that general lexicons fail on specialised text.
- [[ornstein-2024-train-your-stochastic]] — complements — [[haselmayer-2017-sentiment-analysis-political]]: Ornstein offers LLM few-shot coding as a cheaper alternative to the crowdcoding step Haselmayer uses for gold intensity labels.
- [[zhang-2022-data-assisted-social]] — contradicts — [[rice-2018-corpus-based-dictionaries]]: Zhang applies the general Affin lexicon to retail tweets with no domain check, the practice Rice shows misfires.
- [[taherdoost-2023-artificial-intelligence-sentiment]] — cites — [[zhang-2022-data-assisted-social]]: The review cites a 2022 social-media analytics competitive-decision paper (ref 16) that appears to be this work.
- [[taherdoost-2023-artificial-intelligence-sentiment]] — contradicts — [[burnham-2023-stance-detection-practical-guide]]: Taherdoost collapses sentiment and stance; Burnham argues they must be measured separately.
- [[zhang-2022-data-assisted-social]] — complements — [[steinertthrelkeld-2018-twitter-data]]: Steinert-Threlkeld gives the Twitter collection limits that Zhang's 882,371-tweet case study never discusses.
- [[joo-2022-image-data-automated]] — complements — [[steinertthrelkeld-2018-twitter-data]]: Same author, matching 'X as data' tutorial format; text versus image.
- [[joo-2022-image-data-automated]] — complements — [[mestre-2026-potential-pitfalls-audio]]: Same methods-tutorial genre for a non-text modality (images versus audio).
- [[joo-2022-image-data-automated]] — complements — [[dietrich-2019-pitch-perfect-vocal]]: Joo cites Dietrich's video work as a computer-vision polarization measure; Dietrich 2019 is the audio counterpart.
- [[simchon-2024-persuasive-effects-political]] — complements — [[azucar-2017-predicting-personality-traits]]: Microtargeting assumes recipient traits can be inferred from footprints, the step Azucar quantifies.
- [[simchon-2024-persuasive-effects-political]] — complements — [[koutsoumpis-2024-kernel-truth-text]]: Koutsoumpis bounds how much openness signal closed-vocabulary text carries, which limits both Simchon's ad scoring and any text-derived recipient openness.
- [[celli-nd-twenty-years-personality]] — complements — [[simchon-2024-persuasive-effects-political]]: Celli lists personality-aware persuasion in elections as an ethical threat; Simchon measures a small version of it.

## What it means for the systems

The status in brackets is the extraction's bearing tag. UNVERIFIED bearings are single-agent leads, not findings (DE-062).

**Psychometrics: lexical Big Five, gold panels, receptivity** ([[systems/melange-psychometrics]], [[systems/melange-validation]])
- *The reference instrument is weaker than assumed* [VERIFIED-SOFTENED, contradicts]. [[mairesse-2007-automatic-recognition-personality]] Table 19 shows SMOreg on EAR observer ratings doing worse than predicting the mean for every trait, with relative error 131.75-213.20%. Ramey footnote 4 says RKH used these observer-trained models. So the lexical reader's held-out mean r .350 against RKH measures agreement with an instrument that showed no out-of-sample accuracy in its own paper. It is not validity.
  - Ramey's own text is internally inconsistent: footnote 4 says observer-trained models, line 191 says essays. Worth a note in the brain.
- *The ceiling is low* [VERIFIED-SOFTENED]. Three works support retiring the lexical basis from the receptivity layer and keeping status labels on any external trait claim:
  - [[koutsoumpis-2024-kernel-truth-text]]: about 5% of self-report variance.
  - [[celli-nd-twenty-years-personality]]: r of about 0.30.
  - [[azucar-2017-predicting-personality-traits]]: Agreeableness lowest. This supports moving A from lexical to behavioural cooperation.

  Two more points from the same works:
  - Koutsoumpis gives a direction check for the active SMOreg weights. Some weights point the opposite way, which is worth a look.
  - Convergence with the Senate panels may be cue utilization rather than validity. Celli's lens model classes the RKH comparison as Recognition against a same-method target, and the panels as Perception.
- *Affective N* [supports, VERIFIED/SOFTENED].
  - Mairesse: ES has the lowest inter-observer agreement (pair r .29).
  - Naz: datasets do not measure N directly.
  - Azucar: demographics inflate N "accuracy".

  Together these support the N gate and the record that no gold criterion exists for N.
- *Receptivity design gap* [UNVERIFIED]. [[simchon-2024-persuasive-effects-political]] locates the persuasion effect in a message × trait interaction. The receptivity layer has trait main effects only. This is a modelling gap, not a recommendation: UK lay raters, small effects, a perceived-persuasion outcome, and openness is Melange's weakest text trait.
- *Never present .350 as matching Azucar's .29-.40.* The scales coincide but the designs do not.

**Audio** ([[systems/melange-audio-speech]]; display-only is supported)
- [[marrero-2022-evaluating-voice-samples]] is the benchmark the audio lane already failed to reproduce at N=90 (project report).
  - Its top features (MFCC, chroma, spectral entropy) are mostly missing from eGeMAPS-88. That is an open lead.
- [[dietrich-2019-pitch-perfect-vocal]]: `b5_audio_descriptors_candidate.py` cites it, but runs at floors of 5 units per member, 2 topics and 2 units per topic.
  - Effects of 0.1-0.4 SD cannot be detected at those floors.
  - With two topics, the sign of each topic's deviation is fixed by construction.
  - Clerk floor video is a better identity source [suggests-extension].
- [[mestre-2026-potential-pitfalls-audio]]:
  - The neural speaker-ID head copies its method. Melange's fresh-21 EER was .226 against a ≤5% bar, so the paper's in-domain 0.914 is not a reachable benchmark.
  - It cautions against cross-member pooled eGeMAPS z-scores that have no loudness or channel control.
- [[salam-2022-learning-personalised-models]]: pace and volume are sex-clean; pitch variability and jitter are not. The semitone-SD fix sits on an unmerged branch. That is an open item, not a new proposal.

**Stance stack** ([[systems/melange-stance]])
- [[burnham-2023-stance-detection-practical-guide]] [VERIFIED] supports DEBATE NLI as the main reader. Open items it raises:
  - Base versus Large model size (Base 0.40 vs Large 0.65 MCC in Burnham).
  - No MCC or hypothesis-wording sensitivity check.
  - Fallback to Claude pre-labels.
- [[ornstein-2024-train-your-stochastic]]:
  - `run_stage5.R` discards DEBATE's per-label probabilities and keeps only a sign. Ornstein's continuous measure comes from the probability vector.
  - The Claude rater reports self-stated confidence, not a calibrated probability, and GPT-4-class overconfidence is documented.
  - Regex topic routing has never been checked against human labels.
  - **Possible anchoring error:** the tweet gold queue shows model labels next to the human gold columns.
- [[dai-2025-large-language-model]]: FOLAR is the soft, learned version of the hard sign-multiplication rule in `apply_two_hop_stance.py`. Its fusion design suggests a cheap multi-labeller check on single-labeller bias. It does not show FOLAR would beat fine-tuned DEBATE on 87-156 held-out CU items.
- [[b-2025-comparing-fine-tuned]]: **actual error.** The `train_stance_domain.py` docstring repeats Khan's 0.93-0.97 as comparable numbers. RoBERTa was scored on a different, augmented test set (support 6,699 vs 1,675), so the docstring should say so.
  - What transfers: "unrelated" as its own class and macro-F1 as the headline.
  - What does not transfer: the model ranking. Khan-style augmentation was rejected on 2026-07-03.

**Tone and press value** ([[systems/melange-stance]])
- `press_value` = DEBATE sign × stock LSD2015 intensity.
  - [[haselmayer-2017-sentiment-analysis-political]] [measurement-source]: LSD intensity has only a three-case smoke test and has never been compared with human-coded intensity. Haselmayer's crowd design would supply that test cheaply.
  - [[rice-2018-corpus-based-dictionaries]] [UNVERIFIED single-path check on 1,563 text-bearing releases]: the domain terms ("credit", "debt*", "risk*", "fraud*") account for 4.5% of positive mass and 7.0% of negative mass. Rank correlation with and without them is .953, and the net sign flips on 4.1% of releases. The effect is real but small, and it is not a reason to change rankings.
- *Actual error* [VERIFIED, contradicts]: the retired legacy regex scorer cites Haselmayer but departs from it. It sums cues instead of taking the strongest cue, uses a two-token negator window and about 45 hand-weighted patterns. Its Rice corpus-expansion helper was never run.
- [[park-2025-automatic-construction-direction]]: the whole-text direction override made 46 sign changes on gold, 3 good and 43 bad. It was rejected, and the curated target registry (cfpb_reform +1, cfpb_expansion −1, cfpb_institution 0) survives. Park's evidence covers headlines only. The open question of restricting the rule to the sentence or headline that names the bill is exactly the condition under which Park would transfer.

**Frame alignment and salience shock** ([[systems/melange-influence-engine]])
- [[joo-2022-image-data-automated]] **actual error** [VERIFIED / VERIFIED-SOFTENED]. `fec/network/frame_alignment.py` reads `cu_press_stance_tone.csv` with no source filter. 816 of its 2,388 rows are americascreditunions.org articles, each attributed to every member it mentions, so one ACU headline counts as many members echoing one script.
  - In an UNVERIFIED single-path recompute that keeps only *.senate.gov and *.house.gov rows:
    - General CU duplication falls from 60.2% (n=442) to 5.2% (n=116).
    - CU Tax Status falls from 64.7% (n=190) to 38.1% (n=42).
  - The output is a diagnostic, last written 2026-07-01. It reaches `salience_shock` only under `--intensity auto`, which the chain does not pass. Live impact is currently nil, but the diagnostic is wrong as written.
- [[zhang-2022-data-assisted-social]] [UNVERIFIED]: a sentiment × volume burst index is a possible time-varying salience trigger. The paper validates nothing, so it cannot justify any threshold, half-life or weight.

**SNA and data provenance** ([[systems/melange-influence-engine]], [[systems/melange-pathfinding]])
- [[steinertthrelkeld-2018-twitter-data]]: API limits do not bind on the Quorum and Bluesky channels. Author metadata reflects collection time, which is a provenance caveat for handle-to-member attribution.
- Social edges come from reposts and @mentions in post text, not follower lists. Nothing here bears on edge weighting.
- The two rejected SNA proposals are not argued for by any work.
- LDA stays an identity source, not an edge or weight source (D-021). This matters for Paul's Kim et al. note ([[kim-2025-measuring-interest-group]]), because LDA-derived positions should not become edge weights.
- Reciprocity is not touched and remains unvalidated (D-104..D-108).

**Wording guard.** Any trait, stance or tone score derived from these methods is a heuristic model output. It is never closeness, access or an introduction probability (D-025).

## Gaps

**Paul's stated gaps against what the 24 works cover**
1. **Conversion step (model output → node attribute or vector).** Still open. No work here maps a measure onto a graph node. The nearest pieces are aggregation rules:
   - [[yang-2023-getting-personal-deep]]: person-level attention over posts.
   - [[ornstein-2024-train-your-stochastic]]: a continuous score from the label-probability vector.
   - [[haselmayer-2017-sentiment-analysis-political]]: the max-word rule, sentence to document.
   - [[marrero-2022-evaluating-voice-samples]] and [[koutsoumpis-2024-kernel-truth-text]]: reliability disattenuation.

   A node-attribute spec could assemble these, but no work validates one.
2. **Ambiguity.** [[burnham-2023-stance-detection-practical-guide]] quantifies it: 34.0% versus 19.5% coder disagreement when the target is unnamed. [[b-2025-comparing-fine-tuned]] concentrates its errors on sarcastic and implicit text. Neither separates ambiguous statements as a class, so Paul's Birkenmaier gap is unresolved.
3. **Inter-industry conflict over a bill.** No work in the theme addresses it.
4. **Temporal lag / prediction.** All stance work here is static. Zhou 2026 on stance dynamics ([[zhou-2026-measuring-stance-dynamics]]) is linked from [[dai-2025-large-language-model]] but is not in the theme.
5. **Long-distance dependency (Peng).** Peng et al. is not in the theme file. [[dai-2025-large-language-model]] and [[park-2025-automatic-construction-direction]] touch only clause-level direction.
6. **Venue and auditory constraints (Ramey).** The audio strand covers floor ([[dietrich-2019-pitch-perfect-vocal]]) and debates ([[mestre-2026-potential-pitfalls-audio]]). No work gives a cross-venue framework, and Melange's pooled interview-channel baseline is exactly the uncontrolled case.

**Coverage gaps in the file itself**
- Paul's four annotated works lack extracted entries in this theme: Birkenmaier & Lechner, Kim et al., Peng et al. and Ramey et al. Alkazzi & Okumura (path-finding) is also absent. The synthesis therefore cannot use his annotations against their content.
- No elite self-report criterion exists anywhere. No congressional press-release stance benchmark with human gold exists outside the system's own n=87/156 held-out sets.
- No work on calibrating LLM or NLI probabilities for political text beyond Ornstein's overconfidence observation.
- No multiple-comparison or seed-variance standard in the stance benchmarks.

**Online scan (single search, UNVERIFIED leads only, not read)**
- Walker, Angst et al., PLOS ONE (Nov 2025), "Promises and pitfalls of using LLMs to identify actor stances in political discourse". It uses zero-shot LLM actor stance on 1,710 human-annotated German newspaper paragraphs, and results vary heavily by prompt chain and model. It is the closest human-gold design to the CU press task. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12629487/
- Cal Poly thesis (Pramuk, 2026). It does LLM stance from California legislators' press releases toward specific bills and flags inconsistencies with votes. It is validated only via a ParlVote vote-proxy, with no human-coded press gold. https://digitalcommons.calpoly.edu/theses/3372
- Harvard "StrideStance": zero-shot legislator stance on speeches in the 117th Congress, validated against roll-call-style measures. https://dash.harvard.edu/entities/publication/7f242c13-abbb-4283-be50-5ec9ef2eb017
- Multimodal study of U.S. legislators' cross-platform persona strategies (arXiv 2509.10720). It is relevant to the social channel, not to validation. https://arxiv.org/pdf/2509.10720
- The scan found **no** 2024-2025 LLM-based Big Five measurement of members of Congress. RKH (PSRM 2019) remains the field's elite reference, which makes the Mairesse/RKH instrument weakness above more consequential.

**Small factual corrections worth recording (errors only)**
- [[koutsoumpis-2024-kernel-truth-text]]: the slug says 2024, but the PDF is Psychological Bulletin 2022.
- [[burnham-2023-stance-detection-practical-guide]] was published in PSRM 13 (2025), 611-628.
- [[ornstein-2024-train-your-stochastic]] was published in PSRM 2025.
- [[simchon-2024-persuasive-effects-political]] swaps the study 2a/2b sample sizes between text and figure.

## All works

- [[steinertthrelkeld-2018-twitter-data]] (2018) — Twitter as Data
- [[joo-2022-image-data-automated]] (2022) — Image as Data: Automated Content Analysis for Visual Presentations of Political Actors and Events
- [[zhang-2022-data-assisted-social]] (2022) — Big data-assisted social media analytics for business model for business decision making system competitive analysis
- [[marrero-2022-evaluating-voice-samples]] (2022) — Evaluating voice samples as a potential source of information about personality
- [[dai-2025-large-language-model]] (2025) — Large Language Model Enhanced Logic Tensor Network for Stance Detection
- [[rammstedt-2025-going-global-language]] (2025) — Going Global: 39 Language Versions of the BFI-2-XS
- [[koutsoumpis-2024-kernel-truth-text]] (2024) — The Kernel of Truth in Text-Based Personality Assessment: A Meta-Analysis of the Relations Between the Big Five and the Linguistic Inquiry and Word Count (LIWC)
- [[celli-nd-twenty-years-personality]] (n.d.) — Twenty Years of Personality Computing: Threats, Challenges and Future Directions
- [[azucar-2017-predicting-personality-traits]] (2017) — Predicting the Big 5 personality traits from digital footprints on social media: A meta-analysis
- [[b-2025-comparing-fine-tuned]] (2025) — Comparing Fine-Tuned RoBERTa with Traditional Machine Learning Models for Stance Detection in Political Tweets
- [[taherdoost-2023-artificial-intelligence-sentiment]] (2023) — Artificial Intelligence and Sentiment Analysis: A Review in Competitive Research
- [[ornstein-2024-train-your-stochastic]] (2024) — How to train your stochastic parrot: large language models for political texts
- [[simchon-2024-persuasive-effects-political]] (2024) — The persuasive effects of political microtargeting in the age of generative artificial intelligence
- [[dietrich-2019-pitch-perfect-vocal]] (2019) — Pitch Perfect: Vocal Pitch and the Emotional Intensity of Congressional Speech
- [[mestre-2026-potential-pitfalls-audio]] (2026) — Potential and Pitfalls of Audio as Data for Political Research: Alignment, Features, and Classification Models
- [[rice-2018-corpus-based-dictionaries]] (2018) — Corpus-Based Dictionaries for Sentiment Analysis of Specialized Vocabularies
- [[naz-2025-machine-deep-learning]] (2025) — Machine and deep learning for personality traits detection: a comprehensive survey and open research challenges
- [[park-2025-automatic-construction-direction]] (2025) — Automatic construction of direction-aware sentiment lexicon using direction-dependent words
- [[haselmayer-2017-sentiment-analysis-political]] (2017) — Sentiment analysis of political communication: combining a dictionary approach with crowdcoding
- [[salam-2022-learning-personalised-models]] (2022) — Learning Personalised Models for Automatic Self-Reported Personality Recognition
- [[kumari-nd-five-personality-predictions]] (n.d.) — Big Five Personality Predictions From Twitter Using Regression
- [[yang-2023-getting-personal-deep]] (2023) — Getting Personal: A Deep Learning Artifact for Text-Based Measurement of Personality
- [[mairesse-2007-automatic-recognition-personality]] (2007) — Using Linguistic Cues for the Automatic Recognition of Personality in Conversation and Text
- [[burnham-2023-stance-detection-practical-guide]] (2023) — Stance Detection: A Practical Guide to Classifying Political Beliefs in Text
