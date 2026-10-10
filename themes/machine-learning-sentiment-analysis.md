---
type: theme
name: "Machine Learning & Sentiment Analysis"
paper_count: 22
tags: [theme]
---
# Machine Learning & Sentiment Analysis

**Theme: Machine Learning & Sentiment Analysis (22 works).**

**Scope note first.** Paul's own notes for this category come from an 8-work annotated bibliography (sha12 b0774147a262). Only 3 of those 8 are among the 22 works filed under this theme: [[steinertthrelkeld-2018-twitter-data]], [[joo-2022-image-data-automated]], and Taherdoost and Madanchian ([[taherdoost-2023-artificial-intelligence-sentiment]]). All 3 are citation-only in his document, so none carries his annotation text. The 4 works he did annotate are treated differently:
- Birkenmaier and Lechner, Kim et al. and Ramey et al. appear here only as cross-references ([[birkenmaier-2025-measuring-politicians-public]], [[kim-2025-measuring-interest-group]], [[ramey-2016-measuring-elite-personality]]).
- Peng et al. (emotion-model landscape) has no slug in this file.
- Alkazzi and Okumura (multi-agent path-finding, unannotated) also has no slug in this file.

His stated questions are therefore answered by works other than the ones he wrote about. The gaps section maps each question to what this set does and does not cover.

**Paul's argument.** ML and NLP run over public political text (floor speech, press and social posts, lobbying filings) can produce actor-level attributes: personality traits, positions or stance, emotion and tone. Those attributes can then sit on the nodes of a social-network-analysis graph and serve political intelligence, meaning lobbying strategy, industry alignment, and how to pitch a specific member. His most repeated gap is the *conversion step*: how a model's output becomes a node attribute or vector.

**What the 22 works say together:**
1. **Text carries little self-reported trait signal, and the ceiling is well established.**
   - Self-report criteria give pooled or state-of-the-art correlations around r ≈ .30, with single LIWC categories at |rho| .08-.14 ([[koutsoumpis-2024-kernel-truth-text]], [[azucar-2017-predicting-personality-traits]], [[celli-nd-twenty-years-personality]]).
   - Observer criteria give far more, but that gain reflects shared cues, not validity.
2. **Domain-built measurement beats generic lexicons** on every political or specialized corpus tested ([[rice-2018-corpus-based-dictionaries]], [[haselmayer-2017-sentiment-analysis-political]], [[park-2025-automatic-construction-direction]]). Learned models (fine-tuned transformers, LLMs) beat both on benchmark data.
3. **LLMs and transformers are not uniformly better.** They lead on public benchmarks ([[ornstein-2024-train-your-stochastic]], [[dai-2025-large-language-model]], [[b-2025-comparing-fine-tuned]]). The evidence still shows classical models staying competitive on small labelled sets, and LLM probabilities being badly calibrated.
4. **Audio measures arousal, not valence, and needs within-speaker and within-sex normalization** ([[dietrich-2019-pitch-perfect-vocal]], [[mestre-2026-potential-pitfalls-audio]], [[salam-2022-learning-personalised-models]], [[marrero-2022-evaluating-voice-samples]]).

**Bearing on Paul's systems.** Most of these works *qualify* components Melange already has rather than offering new ones: the lexical Big Five reader; the receptivity layer's behavioural basis; the CU stance stack (DEBATE/NLI direction × LSD2015 intensity); the two-hop stance-by-valence rule; the audio lane, including its display-only status; the frame-alignment diagnostic. None of the 22 argues for either of the two SNA proposals this project measured and rejected.

## Strands

### Personality from text: ceilings, criteria and model families
**Shared question.** How much Big Five signal can be recovered from text, and against which criterion?

**Where the works agree: the ceiling is low against self-report.**
- [[koutsoumpis-2024-kernel-truth-text]] pools 31 samples (N = 85,724) using the 52 LIWC categories shared by LIWC2001, 2007 and 2015.
  - Self-report (26 samples): strongest |rho| .08-.14. All 52 categories together explain about 5.1% of trait variance.
  - Observer report (only 5 samples, n = 1,637): |rho| .18-.39, with mean R2 38.5%.
  - Only 20 of 260 trait-by-category markers agree in direction for both criteria (the "kernel of truth").
- [[celli-nd-twenty-years-personality]] (129 included papers by its count, PRISMA 2020) reports state-of-the-art text Recognition at rho ≈ .30, against a .133 baseline (WASSA shared tasks). Zero-shot GPT reaches r = .29 on Facebook status updates. Computer Perception reaches .30, against .38 for human judges, with a comparability caveat.
- [[azucar-2017-predicting-personality-traits]] pools 16 studies of lay users (Facebook, Twitter, Sina Weibo, Instagram; search through May 2017). Pooled r runs from .29 (Agreeableness) to .40 (Extraversion).
  - This pool is UNSTRATIFIED by platform, and I² ≥ 93 for every trait.
  - Each study contributed its best model, so the figures are optimistic.

**Apparent disagreement, resolved by design.** Azucar's .29-.40 looks far above Koutsoumpis's per-category .08-.14. Azucar pools best multi-feature models; Koutsoumpis reports single categories. Koutsoumpis's all-category R2 of 5.1% implies a multiple R of about .23, which is in the same band as Celli's .30.

**Model-family claims conflict in strength of evidence.**
- [[yang-2023-getting-personal-deep]] reports DeepPerson AUC 75.0 vs 64.6 for the best comparator on PANDORA (Reddit, 1,568 users; median-split self-report labels). The feature-based IBM baseline sits near 56, so closed-vocabulary scoring is the weakest family. Even so, LIWC-type concept embeddings add about 2 pp inside the deep model.
- [[naz-2025-machine-deep-learning]] reports 88-92% accuracy on the Kaggle MBTI set. It gives no split, variance or class balance, reports Big Five on MBTI labels without explaining the mapping, and its search funnel does not reconcile (270 - 184 ≠ 171). Its "about 25%" deep-learning gain is unsupported.
- [[kumari-nd-five-personality-predictions]] reports no numbers at all. Paul's file reads it as a negative example.

**Conceptual bridge.** Celli's Recognition-versus-Perception split (Brunswik lens) is the same distinction as Koutsoumpis's cue validity versus cue utilization. Both say that agreement with observers can reflect raters using the same cues the model uses, not true personality.

[[rammstedt-2025-going-global-language]] adds an instrument-side lesson: across 39 BFI-2-XS versions, comparability is asserted procedurally rather than shown, because Field Trial psychometrics are confidential.

**Agreeableness.** Azucar finds A the least recoverable trait. Naz notes that no dataset directly measures Neuroticism. Both bear on Melange's move from lexical A to behavioural cooperation and on its lack of a gold criterion for N.

Works: [[koutsoumpis-2024-kernel-truth-text]], [[azucar-2017-predicting-personality-traits]], [[celli-nd-twenty-years-personality]], [[naz-2025-machine-deep-learning]], [[yang-2023-getting-personal-deep]], [[kumari-nd-five-personality-predictions]], [[rammstedt-2025-going-global-language]]

### Voice and audio as political data
**Shared question.** What can acoustic features measure about speakers, and under what normalization?

**Agreement 1: within-speaker baselines.**
- [[dietrich-2019-pitch-perfect-vocal]] z-scores mean F0 within each speaker across 74,158 House floor speeches (2009-2014, each at least 50 words). Female members rise about 0.09-0.11 SD above their own baseline when mentioning women. Partisans rise 0.29-0.38 SD on party-owned topics.
- [[mestre-2026-potential-pitfalls-audio]] uses within-speaker pitch deviation and normalizes energy by each debate's loudness. Its corpus is 45 presidential and VP debates (1960-2020).

**Agreement 2: sex dominates the raw acoustic signal.**
- [[marrero-2022-evaluating-voice-samples]] predicts gender at over 90%, against 10.67% average out-of-sample R2 for the Big Five (614 students, 100-fold CV).
- [[salam-2022-learning-personalised-models]] gets almost all of its gain from a gender split: average MSE 0.690 gender-wise vs 0.818 generic fusion, on the UDIVA challenge test split. Test N is not reported.
- Mestre's highest-pitch speakers are mostly the women candidates.

**Agreement 3: audio measures intensity, not valence.** Dietrich says pitch tracks arousal (footnote 1). Mestre finds off-the-shelf emotion models biased by gender and pitch and at odds with expert coding.

**Disagreement: the paper's claims versus Melange's attempts to reproduce them.**
- Super-additivity: Marrero's headline is that voice and LIWC together exceed their parts (joint 10.67% vs vocal 6.24% vs LIWC 3.13%, averaged over the Big Five). Melange's July 2026 one-look tests (N = 90 legislators) missed super-additivity every time. Vocal added +0.001 over linguistic for E against Pink.
- Speaker identification: Mestre's wav2vec2 head reaches 0.914 accuracy over 156 in-domain debate speakers. Melange's run of the same method on congressional audio gave a fresh-21 EER of .226, against a bar of 5% or less. That run used a source- and event-split with weak caption labels.
- Detection power: Dietrich's 0.1-0.4 SD effects rest on roughly 120 speeches per speaker and a cross-speaker (female × topic) contrast. Melange's floors are 5 units per member and 2 per topic, with no comparison group, so effects of that size are undetectable at those floors.

**Net effect.** The strand supports keeping audio display-only and keeping absolute F0 out of trait scores.

Works: [[marrero-2022-evaluating-voice-samples]], [[dietrich-2019-pitch-perfect-vocal]], [[mestre-2026-potential-pitfalls-audio]], [[salam-2022-learning-personalised-models]]

### Sentiment dictionaries, domain specificity and direction
**Shared question.** Can lexicon methods measure tone validly in specialized political or financial text?

**Strong agreement: generic lexicons misfire, and domain-built ones do much better.**
- [[rice-2018-corpus-based-dictionaries]] (IMDB, 50,000 reviews): the seed-plus-GloVe corpus dictionary scores 80.4% in-sample and 79.5% held out, against about 69-71% for LIWC and AFINN. It needs about 5,000 or more documents to win.
- Rice on about 26,000 Supreme Court majority opinions (1792-2010): justice-level polarity ranks correlate only .29 with LIWC and .16 with AFINN. So the choice of dictionary reorders the units.
- [[haselmayer-2017-sentiment-analysis-political]] (200 Austrian German sentences, expert-coded): the crowd-built negativity dictionary reaches r = .65 at 84% coverage, against r = .13 and .19 for two translated general-purpose dictionaries.
- [[park-2025-automatic-construction-direction]] (Financial PhraseBank subsets): adding direction-dependent pairs to Loughran-McDonald raises weighted F1 from about .59-.62 to .71-.82.

**Agreement on the limits of lexicons.**
- Dictionaries still trail learned models: Park's lexicon reaches .82 on DS100 against fine-tuned RoBERTa at about .96, and stays below .5 on SemEval and FiQA. Rice falls short of the supervised 87-88% benchmarks.
- Document-level polarity cannot say *what* the sentiment is about (Rice). Park attacks one version of this, direction-dependent polarity ("cost decrease" is positive), but only on headline-length sentences.

**Disagreement and weak links.**
- [[zhang-2022-data-assisted-social]] applies the general Affin lexicon to 882,371 retail tweets with no domain check, no holdout and no metrics. That contradicts Rice in practice.
- [[taherdoost-2023-artificial-intelligence-sentiment]] (43 included papers from Scopus 2012-2022; its 133-vs-90 screening counts never reconcile) collapses sentiment and direction and covers no political text. Its one useful borrowed point is that neural classifiers predict well but diagnose poorly, while topic models and dictionaries do the reverse (Alantari et al.).
- Haselmayer's scoring rule takes the single most negative word in a sentence (the max rule). The legacy scorer that cites it sums weighted cues instead, which departs from the paper.

Works: [[rice-2018-corpus-based-dictionaries]], [[haselmayer-2017-sentiment-analysis-political]], [[park-2025-automatic-construction-direction]], [[taherdoost-2023-artificial-intelligence-sentiment]], [[zhang-2022-data-assisted-social]]

### Stance detection and LLM labelling of political text
**Shared question.** Which model family should label stance or sentiment in political text, and how should its labels be validated?

**Agreement on public benchmarks: larger models win.**
- [[ornstein-2024-train-your-stochastic]] (945 SCOTUS tweets): few-shot GPT-3 gets 88.4% of negative/positive signs right, against 64.3% for TweetNLP (a fine-tuned RoBERTa), 57.7% for Naive Bayes and 38.3% for the best dictionary. Ideology on 18 UK manifestos correlates with expert scores at rho .92 (economic) and .80 (social).
- [[dai-2025-large-language-model]] (FOLAR):
  - In-target gains are small: +1.7 F_avg on LA and +2.0 on DT_I (SemEval-2016 and ISD), and FOLAR is not best on FM or HC.
  - Zero-shot VAST gains are larger: +4.2 with full training data, +6.2 with 10%.
  - Its about 5.8-point fusion ablation compares fusion against a single GPT-3.5 model, so it does not separate fusion from the choice of LLM.
- [[b-2025-comparing-fine-tuned]] (Roman Urdu tweet-reply pairs, about 8,374 labelled): RoBERTa scores .97 accuracy, but on an augmented set of 6,699 rows, while the classical models (RF .95 to NB .89) were scored on 1,675 test rows. The headline comparison is therefore not like-for-like.

**Disagreement: the small-data regime and calibration.**
- [[naz-2025-machine-deep-learning]] concedes that classical TF-IDF models stay competitive on small data.
- Melange's own press data agree: a tuned word+char LinearSVC scored .816 macro-F1 against .757 for a transformer on the augmented press set.
- Ornstein shows that GPT-4 put more than 99% probability on one label for over 70% of 945 posts, and chose Neutral more often than the human coders.
- Neither Dai nor Khan reports seeds or standard deviations.

**Methods that carry over regardless of which family wins:**
- treat "unrelated" as its own class (Khan);
- report macro-F1 as the headline (Khan);
- validate in three steps: tune the prompt on one hand-labelled set, check it on a second, then apply (Ornstein);
- build continuous measures from the label probability vector, not the argmax (Ornstein). This is the closest thing in the set to an answer to Paul's conversion-step gap.

**Error profiles.** Khan's errors concentrate on sarcastic, ambiguous and implicit statements. Dai's error analysis shows the LLM linking unrelated positive words to the target. That is the same failure that sank Melange's automatic-registry variants.

Works: [[b-2025-comparing-fine-tuned]], [[dai-2025-large-language-model]], [[ornstein-2024-train-your-stochastic]]

### Platforms, images and other "X as data" pipelines
**Shared genre.** Methods tutorials or frameworks for turning platform content into data. None tests a causal claim.

**[[steinertthrelkeld-2018-twitter-data]]** (excerpt covers printed pp. 1-10 only) documents the 2016 Twitter API:
- about a 1% streaming sample;
- a 3,200-tweet REST cap per account;
- author metadata that reflects request time, not post time;
- 2016 elite coverage of every Senator and 430 House members.

**[[joo-2022-image-data-automated]]** (same author) has two studies:
- Study 1 runs chi-squared tests by party, separately by gender, on 15,647 Facebook photos from 677 candidates in 2018. It tests about 1,700 labels with no multiple-testing correction.
- Study 2 defines framing as the near-duplicate share per event and label, with a distance threshold of 0.2 picked from one histogram. H1 and H2 each hold in 3 of 5 protest events.

**[[zhang-2022-data-assisted-social]]** chains lexicon sentiment, Louvain on a cross-correlation graph thresholded at 0.1, and a |z| > 30 burst rule. None of it is validated.

**Where they agree.** Representativeness and timing caveats matter. For example, geotags cover about 2-3% of tweets and skew urban and higher-income. All three choose thresholds by convention or by eye.

**Lesson from porting.** When Melange ported Joo's duplication instrument to press text, the transfer broke on the unit of analysis. Joo's sharer chooses the image. In the text port, an association headline was attributed to every member it mentions (see bearing).

Works: [[steinertthrelkeld-2018-twitter-data]], [[joo-2022-image-data-automated]], [[zhang-2022-data-assisted-social]]

### From traits to persuasion
**[[simchon-2024-persuasive-effects-political]]** puts the persuasion effect in a message-by-trait *interaction*.
- Design: four UK Prolific experiments (n = 440, 804, 803, 804), a mixed model, and outcome = self-reported perceived persuasion on a 1-5 scale.
- Mismatch between ad and reader openness lowers ratings:
  - b = -0.07 and -0.08 per SD of mismatch with real Facebook ads;
  - b = -0.03 with ChatGPT rewrites;
  - b = -0.01 (p = .057) with GPT-3 rewrites.
- Limits: one trait, a perceived (not behavioural) outcome, and degrees of freedom that do not square with the sample sizes.

**[[celli-nd-twenty-years-personality]]** lists personality-aware election persuasion as an ethical threat.

**Together.** One paper shows the effect exists and is small; the other warns about using it. Neither involves legislators or lobbying contacts. The openness Simchon scores is the appeal of the ad text to open readers, a different construct from the author-trait openness Melange extracts from floor speech.

Works: [[simchon-2024-persuasive-effects-political]], [[celli-nd-twenty-years-personality]]

## Agreements and debates

**1. How much trait signal does text carry?**
- Self-report criteria cluster around r ≈ .30:
  - Celli: rho ≈ .30 (WASSA);
  - Azucar: pooled .29-.40 over 16 lay-user studies, UNSTRATIFIED by platform and built from best models;
  - Koutsoumpis: single LIWC categories at .08-.14, and R2 5.1% for all 52 categories.
- Observer criteria look far better (Koutsoumpis R2 38.5%, from only 5 samples, n = 1,637). [[koutsoumpis-2024-kernel-truth-text]] and [[celli-nd-twenty-years-personality]] both warn that this is cue *utilization*, not validity.
- [[naz-2025-machine-deep-learning]] reports 88-92% accuracies. They come from a protocol-free MBTI experiment and are not comparable to the others.

**2. Transformers and LLMs versus classical models.**
- On benchmark data the larger models lead: [[ornstein-2024-train-your-stochastic]], [[dai-2025-large-language-model]], [[yang-2023-getting-personal-deep]], and [[b-2025-comparing-fine-tuned]] (though the last does not compare on the same test set).
- Against that: Naz concedes classical models stay competitive on small data, and Melange's own LinearSVC beat a transformer on its press set (.816 vs .757 macro-F1).
- Celli reports that zero-shot GPT (r = .29) roughly ties specialized models on Facebook posts.
- Net reading: which family wins depends on the task, the label count and the domain, not on model size alone.

**3. Generic lexicon, domain lexicon, or learned model?**
- Domain-built dictionaries clearly beat generic ones: [[rice-2018-corpus-based-dictionaries]], [[haselmayer-2017-sentiment-analysis-political]], [[park-2025-automatic-construction-direction]].
- They still trail learned models (Park .82 vs RoBERTa .96 on DS100).
- [[zhang-2022-data-assisted-social]] ignores the issue.
- [[taherdoost-2023-artificial-intelligence-sentiment]] contributes the predictive-versus-diagnostic framing: neural models predict, transparent dictionaries explain.

**4. Is audio additive to text?**
- [[marrero-2022-evaluating-voice-samples]] says yes (joint 10.67% vs 6.24% vocal and 3.13% LIWC, averaged across traits, 614 students).
- Melange's July 2026 replication on 90 legislators says no: super-additivity missed every time.
- [[mestre-2026-potential-pitfalls-audio]] and [[dietrich-2019-pitch-perfect-vocal]] limit what audio can claim to arousal measured within speaker.

**5. Can LLM labels be trusted?**
- Ornstein: GPT-4 is overconfident, with more than 99% on one label for over 70% of 945 posts. The GPT-3 models used were retired in January 2024, so the results cannot be reproduced.
- Dai: no standard deviations, no named significance test, and an interpretability check on only 100 items with 3 raters.
- Cost is the one claim that holds up across the set: about $0.60 vs $565.20 for 935 ads (Ornstein).

**6. Main effect or interaction?**
- [[simchon-2024-persuasive-effects-political]] locates the persuasion effect in a message-by-trait match, not in the reader's trait.
- That conflicts in design with any layer that treats traits only as person-level main effects. It is a design contrast for discussion, not evidence for a change.

## Methods

**Dominant methods and their recurring weaknesses**

| Family | Works | Main weaknesses in this set |
|---|---|---|
| Closed-vocabulary dictionaries (LIWC, LSD2015, AFINN, Loughran-McDonald) | [[koutsoumpis-2024-kernel-truth-text]], [[rice-2018-corpus-based-dictionaries]], [[haselmayer-2017-sentiment-analysis-political]], [[park-2025-automatic-construction-direction]], [[zhang-2022-data-assisted-social]], [[kumari-nd-five-personality-predictions]] | Small self-report validity (\|rho\| .08-.14); generic lists misfire on domain words (Rice: justice ranks r .29 and .16 vs the corpus dictionary); polarity has no target; validated only on short units (Park: headlines) |
| Classical supervised models on TF-IDF or psycholinguistic features (SVM, RF, NB) | [[b-2025-comparing-fine-tuned]], [[naz-2025-machine-deep-learning]], [[celli-nd-twenty-years-personality]] (SVM is the most common text system: 20 of 45) | Single 80/20 splits, no seeds or confidence intervals; competitive on small data |
| Fine-tuned transformers and deep encoders (RoBERTa, BERT, ResNet-50, wav2vec2, ELMo-HAN) | [[b-2025-comparing-fine-tuned]], [[yang-2023-getting-personal-deep]], [[joo-2022-image-data-automated]], [[mestre-2026-potential-pitfalls-audio]], [[park-2025-automatic-construction-direction]] | Unequal test sets (Khan: 6,699 vs 1,675 rows); in-domain accuracy that does not transfer (Mestre 0.914 vs Melange EER .226); median-split labels (Yang) |
| LLM prompting (few-shot, chain-of-thought, logic rules) | [[ornstein-2024-train-your-stochastic]], [[dai-2025-large-language-model]], [[simchon-2024-persuasive-effects-political]] | Overconfident probabilities; model retirement breaks reproducibility; ablations confounded with the choice of LLM |
| Meta-analysis and systematic review | [[koutsoumpis-2024-kernel-truth-text]], [[azucar-2017-predicting-personality-traits]], [[celli-nd-twenty-years-personality]], [[naz-2025-machine-deep-learning]], [[taherdoost-2023-artificial-intelligence-sentiment]] | I² ≥ 93 (Azucar) and 96% heterogeneous self-report effects (Koutsoumpis); best-model selection; screening counts that do not reconcile (Naz, Celli, Taherdoost) |
| Acoustic features (F0, MFCC, eGeMAPS, pyAudioAnalysis) | [[dietrich-2019-pitch-perfect-vocal]], [[marrero-2022-evaluating-voice-samples]], [[salam-2022-learning-personalised-models]], [[mestre-2026-potential-pitfalls-audio]] | Sex and channel confounds; arousal, not valence; needs about 100+ units per speaker for 0.1-0.4 SD effects |

**Cross-cutting weaknesses**
1. **Criterion mismatch.** Nearly every trait benchmark uses lay-user self-report, and no such criterion exists for legislators.
2. **Thresholds chosen by eye.** Joo's 0.2 distance, Zhang's 0.1 correlation and |z| > 30.
3. **No multiple-testing correction.** Joo tests about 1,700 labels uncorrected.
4. **Unit-of-analysis drift when a method is ported.** Example: Joo's image sharer versus attributed press text.
5. **Missing variance and seeds.** Khan, Naz, Salam and Dai report no variance or seeds.

**The strongest designs in the set:**
- [[ornstein-2024-train-your-stochastic]]: pre-registered, three-step validation.
- [[haselmayer-2017-sentiment-analysis-political]]: crowd agrees with experts at r = .82 (n = 200), with a held-out replication on 755 statements.
- [[koutsoumpis-2024-kernel-truth-text]]: random-effects meta-analysis with moderators and a publication-bias test.

## How the works relate
- [[joo-2022-image-data-automated]] — same-author companion — [[steinertthrelkeld-2018-twitter-data]]: Both are Steinert-Threlkeld "X as data" methods tutorials, image and Twitter text respectively. Paul filed both in the same ML & Sentiment bibliography.
- [[b-2025-comparing-fine-tuned]] — builds-on — [[steinertthrelkeld-2018-twitter-data]]: Steinert-Threlkeld stops at NLP tooling for tweets. Khan et al. take political-tweet text on to fine-tuned stance classification.
- [[kumari-nd-five-personality-predictions]] — builds-on — [[steinertthrelkeld-2018-twitter-data]]: Kumari uses Twitter text as input to Big Five prediction, a downstream use of the acquisition pipeline the Element teaches.
- [[joo-2022-image-data-automated]] — same genre — [[mestre-2026-potential-pitfalls-audio]]: Parallel image-as-data and audio-as-data methods tutorials for political research.
- [[joo-2022-image-data-automated]] — complements — [[dietrich-2019-pitch-perfect-vocal]]: Joo cites Dietrich's 2018 video measure. Dietrich 2019 is the same author's audio-as-data counterpart.
- [[taherdoost-2023-artificial-intelligence-sentiment]] — cites (probable) — [[zhang-2022-data-assisted-social]]: The review's ref 16 appears to be the same 2022 social-media analytics paper, though the first author is listed differently.
- [[zhang-2022-data-assisted-social]] — contradicts — [[rice-2018-corpus-based-dictionaries]]: Rice and Zorn show general lexicons misfire on specialized vocabularies. Zhang applies the general Affin lexicon to retail tweets with no domain check.
- [[zhang-2022-data-assisted-social]] — same genre — [[taherdoost-2023-artificial-intelligence-sentiment]]: Both are business and competitive sentiment works with no validation on political text.
- [[marrero-2022-evaluating-voice-samples]] — same-method — [[koutsoumpis-2024-kernel-truth-text]]: Both relate LIWC categories to Big Five self-report. Marrero's LIWC-alone 3.13% of variance sits inside Koutsoumpis's 5.1% all-category ceiling.
- [[marrero-2022-evaluating-voice-samples]] — complements — [[salam-2022-learning-personalised-models]]: Both recognize self-reported traits from audio. Marrero predicts gender at over 90%; Salam's gain comes from a gender split. Together they show the sex confound.
- [[marrero-2022-evaluating-voice-samples]] — complements (different question) — [[dietrich-2019-pitch-perfect-vocal]]: Marrero tests trait information in voice. Dietrich tests within-speaker arousal on issues, with no personality outcome.
- [[marrero-2022-evaluating-voice-samples]] — paired in the audio prereg — [[simchon-2024-persuasive-effects-political]]: Melange's audio prereg Addendum 1 cites them together: Marrero for the voice-trait range, Simchon for the payoff argument.
- [[dietrich-2019-pitch-perfect-vocal]] — same-method — [[mestre-2026-potential-pitfalls-audio]]: Both use within-speaker pitch deviation. Mestre cites Dietrich for pitch tracking emotional state.
- [[dietrich-2019-pitch-perfect-vocal]] — complements — [[salam-2022-learning-personalised-models]]: Dietrich standardizes F0 within speaker; Salam conditions models on sex. These are two answers to the same confound.
- [[mestre-2026-potential-pitfalls-audio]] — complements — [[salam-2022-learning-personalised-models]]: Both caution that acoustic features carry speaker-sex and recording-channel signal.
- [[dai-2025-large-language-model]] — complements — [[b-2025-comparing-fine-tuned]]: Same task (stance in political tweets). Dai targets the implicit and reasoning-dependent stance that Khan names as the main error source but does not solve.
- [[dai-2025-large-language-model]] — same-method — [[ornstein-2024-train-your-stochastic]]: Both use LLMs to label political text. Ornstein uses few-shot probability vectors; Dai elicits logic rules into an LTN.
- [[dai-2025-large-language-model]] — complements — [[park-2025-automatic-construction-direction]]: Park's direction-aware lexicon addresses clause-level polarity direction, one of FOLAR's two named error types.
- [[rammstedt-2025-going-global-language]] — complements — [[koutsoumpis-2024-kernel-truth-text]]: Rammstedt documents instrument comparability with no validity data. Koutsoumpis supplies the text-to-trait validity side.
- [[koutsoumpis-2024-kernel-truth-text]] — parallel meta-analyses — [[azucar-2017-predicting-personality-traits]]: Closed-vocabulary LIWC categories versus best-model digital footprints. The gap in magnitude (.08-.14 per category vs .29-.40 pooled) comes from design.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — [[yang-2023-getting-personal-deep]]: Both benchmark text-based trait measurement. Yang shows that LIWC-type concepts still add about 2 pp inside a deep model, consistent with a small but real kernel of signal.
- [[koutsoumpis-2024-kernel-truth-text]] — convergent framing — [[celli-nd-twenty-years-personality]]: Cue validity versus utilization in Koutsoumpis corresponds to Recognition versus Perception in Celli. Celli does not cite Koutsoumpis.
- [[celli-nd-twenty-years-personality]] — same-method — [[naz-2025-machine-deep-learning]]: Both are reviews of ML personality computing over the same pipeline families.
- [[celli-nd-twenty-years-personality]] — complements — [[azucar-2017-predicting-personality-traits]]: Celli's text state of the art (rho ≈ .30) and Azucar's pooled r .29-.40 occupy the same band.
- [[celli-nd-twenty-years-personality]] — complements — [[simchon-2024-persuasive-effects-political]]: Celli lists personality-aware election persuasion as a threat. Simchon measures a small version of that effect.
- [[naz-2025-machine-deep-learning]] — extends — [[azucar-2017-predicting-personality-traits]]: Naz covers ML and DL methods after Azucar's May 2017 search cutoff, but without pooled effect sizes.
- [[naz-2025-machine-deep-learning]] — cites — [[yang-2023-getting-personal-deep]]: Naz cites Yang's DeepPerson as the deep-learning artifact for text-based personality.
- [[naz-2025-machine-deep-learning]] — same-method — [[kumari-nd-five-personality-predictions]]: Kumari lists the SVM, RF, RNN and BERT family that Naz surveys, without comparing them.
- [[naz-2025-machine-deep-learning]] — same comparison, different task — [[b-2025-comparing-fine-tuned]]: Both compare classical models with transformers. Naz concedes classical models stay competitive on small data, which tempers Khan's transformer-wins headline.
- [[azucar-2017-predicting-personality-traits]] — same-method (evidence vs sketch) — [[kumari-nd-five-personality-predictions]]: Azucar pools the real effect sizes for Big Five from social-media footprints that Kumari claims but never measures.
- [[simchon-2024-persuasive-effects-political]] — depends-on — [[azucar-2017-predicting-personality-traits]]: Simchon's microtargeting machine assumes recipient-side trait inference from digital footprints, which is what Azucar quantifies.
- [[simchon-2024-persuasive-effects-political]] — bounded-by — [[koutsoumpis-2024-kernel-truth-text]]: Koutsoumpis bounds how much openness signal closed-vocabulary text carries, which limits text-scored openness on either side of the match.
- [[rice-2018-corpus-based-dictionaries]] — complements (co-cited) — [[haselmayer-2017-sentiment-analysis-political]]: Both argue for domain-specific dictionaries, built from word vectors and from crowd ratings respectively. Melange's press scraper cites them together.
- [[rice-2018-corpus-based-dictionaries]] — same-method — [[park-2025-automatic-construction-direction]]: Both build domain lexicons from corpus statistics: GloVe seed similarity versus PMI on direction tags.
- [[haselmayer-2017-sentiment-analysis-political]] — same-method — [[park-2025-automatic-construction-direction]]: Both construct domain lexicons. Haselmayer's crowdcoding addresses the inter-annotator gap that Park states.
- [[ornstein-2024-train-your-stochastic]] — complements — [[rice-2018-corpus-based-dictionaries]]: Ornstein shows a general dictionary at 38.3% against GPT-3 at 88.4% on 945 tweets. It does not test corpus-specific dictionaries such as Rice's.
- [[ornstein-2024-train-your-stochastic]] — alternative to — [[haselmayer-2017-sentiment-analysis-political]]: LLM few-shot coding is an alternative to Haselmayer's crowdcoding step for building intensity gold labels.
- [[ornstein-2024-train-your-stochastic]] — complements — [[b-2025-comparing-fine-tuned]]: Ornstein's TweetNLP baseline is a fine-tuned RoBERTa that GPT-3 beats (88.4 vs 64.3%). The tasks differ: sentiment versus stance.
- [[park-2025-automatic-construction-direction]] — consistent ordering — [[b-2025-comparing-fine-tuned]]: Park's RoBERTa .96 vs lexicon .82 on DS100 matches Khan's direction that transformers lead. The tasks differ, so this is not a contradiction.
- [[taherdoost-2023-artificial-intelligence-sentiment]] — complements — [[b-2025-comparing-fine-tuned]]: Khan supplies the direct classical-versus-transformer comparison that the scoping review lacks.
- [[taherdoost-2023-artificial-intelligence-sentiment]] — tension — [[rice-2018-corpus-based-dictionaries]]: The review dismisses word-matching methods. Rice and Haselmayer show that domain-built dictionaries are valid where generic ones fail.
- [[taherdoost-2023-artificial-intelligence-sentiment]] — complements — [[park-2025-automatic-construction-direction]]: Park's direction-aware lexicon addresses the sentiment-versus-direction distinction that the review collapses.

## What it means for the systems

Status labels follow the files. VERIFIED and VERIFIED-SOFTENED bearings were checked against code or brain records. UNVERIFIED bearings are single-agent leads, not findings (DE-062).

**1. Lexical Big Five reader (`big5_traits.py`)** — [[systems/melange-psychometrics]].
- [[koutsoumpis-2024-kernel-truth-text]], [[celli-nd-twenty-years-personality]], [[azucar-2017-predicting-personality-traits]] and [[yang-2023-getting-personal-deep]] all *qualify* it (VERIFIED-SOFTENED). Closed-vocabulary rates carry little self-report signal, and they are the weakest model family on self-report criteria.
- The reader's held-out CV against RKH (mean .350 across five traits; window and N as recorded in the brain) is not comparable to Azucar's .29-.40 or Celli's .30:
  - RKH is itself text-derived, so it shares method with the reader.
  - The Senate panels measure Perception, where cue utilization can masquerade as validity.
  - Do not present .350 as "matching the literature". Related: [[systems/melange-validation]].
- Koutsoumpis gives a direction check: some active SMOreg weights point the opposite way from the kernel markers.
- [[naz-2025-machine-deep-learning]] and [[kumari-nd-five-personality-predictions]] are context only (UNVERIFIED).

**2. Receptivity layer** — [[systems/melange-psychometrics]], [[systems/moneyball-rank-pillars]].
- The move from lexical traits to a behavioural basis is supported by Koutsoumpis and Azucar (VERIFIED for Azucar). Agreeableness is the least recoverable trait, and demographics raise "accuracy" only through group membership.
- [[simchon-2024-persuasive-effects-political]] points to a message-by-trait interaction that a main-effects composite cannot represent. This is UNVERIFIED and a modelling-gap note only. It is not a reason to change PERSONA_W or the composite, because the population (UK public rating ads), the outcome (perceived persuasion) and the construct (reader-appeal openness) all differ.

**3. Affective N.** Koutsoumpis (negative-emotion kernel markers), Yang and Naz support the record that no gold criterion exists for N. [[rammstedt-2025-going-global-language]] shows that comparability across administrations is asserted procedurally rather than demonstrated. That parallels the failed interview-to-floor transfer. See [[systems/melange-psychometrics]].

**4. Audio lane (display-only status)** — [[systems/melange-audio-speech]].
- [[marrero-2022-evaluating-voice-samples]] is the benchmark Melange already tested at N = 90 legislators and largely failed to reproduce: no super-additivity, and C not measured (r -.081). That failure supports display-only.
- [[dietrich-2019-pitch-perfect-vocal]] is the method source for `topic_emotional_intensity` (VERIFIED-SOFTENED). At the system's floors (5 units per member, 2 topics, 2 units per topic), 0.1-0.4 SD effects cannot be detected.
- [[mestre-2026-potential-pitfalls-audio]]:
  - It is the method source for the identity head and Aeneas alignment. Its 0.914 debate accuracy is not an expected bar, given Melange's fresh-21 EER of .226.
  - It cautions against cross-member pooled eGeMAPS z-scores that lack loudness and channel control.
- [[salam-2022-learning-personalised-models]]: pitch variability and jitter are not sex-clean. The semitone-SD fix is on an unmerged branch.
- Lead (VERIFIED-SOFTENED): Clerk floor video as an identity source, where the chamber has already identified the speaker (Dietrich).

**5. CU stance stack** — [[systems/melange-stance]].
- [[b-2025-comparing-fine-tuned]] is the named method source of `train_stance_domain.py` (VERIFIED-SOFTENED). Its evaluation conventions carry over; its model ranking does not. The docstring repeats .93-.97 as if those figures were comparable, but RoBERTa was scored on a different test set.
- [[ornstein-2024-train-your-stochastic]] (VERIFIED-SOFTENED):
  - `run_stage5.R` discards DEBATE's per-label probabilities and keeps only a sign.
  - The Claude rater's confidence is self-stated, not calibrated.
  - Regex topic routing has no human check.
  - The tweet gold queue shows model labels next to the human gold columns.
- [[dai-2025-large-language-model]]: FOLAR is the soft, learned form of the two-hop stance × valence rule. Its multi-LLM fusion suggests a cheap check on single-labeller bias in the Claude pre-labels that DEBATE distils. It does not show that FOLAR would beat DEBATE or the SVM on 87-156 held-out CU press items.

**6. LSD2015 tone (press_value = DEBATE sign × LSD intensity)** — [[systems/melange-stance]], [[systems/melange-labels-and-tiers]].
- [[rice-2018-corpus-based-dictionaries]]: LSD scores CU domain terms as sentiment ("credit" positive; "debt*", "risk*" negative). A one-path check (UNVERIFIED) over 1,563 text-bearing releases found:
  - 4.5% of positive mass and 7.0% of negative mass comes from those terms;
  - rank correlation of .953 with and without them;
  - the net sign flips on 4.1% of releases.
- [[haselmayer-2017-sentiment-analysis-political]] supplies a cheap validation design for intensity, which has never been compared with human coding; its only recorded validation is a three-case smoke test. The legacy regex scorer cites Haselmayer but contradicts its scoring rule (VERIFIED).

**7. Bill-sign handling and the two-hop rule** — [[systems/melange-stance]], [[systems/melange-legislative-data]]. [[park-2025-automatic-construction-direction]] explains why the whole-text directional override was rejected: 46 sign changes on gold, 3 good and 43 bad. The paper's evidence transfers only under the still-open restriction to the sentence or headline that names the bill.

**8. Frame alignment and salience shock** — [[systems/melange-lobbying]], [[systems/mentat-fec-attribution]].
- [[joo-2022-image-data-automated]] is the method source for `frame_alignment.py`. The port breaks on the unit of analysis (VERIFIED):
  - 816 of 2,388 rows in `cu_press_stance_tone.csv` are association articles attributed to each member they mention.
  - Restricting to *.senate.gov and *.house.gov rows moves General CU duplication from 60.2% (n = 442 rows) to 5.2% (n = 116) in a single-path recompute (UNVERIFIED).
  - The output is a diagnostic last written 2026-07-01. It reaches salience_shock only under `--intensity auto`, which the chain does not pass.
- [[zhang-2022-data-assisted-social]]'s burst index is an UNVERIFIED lead for a time-varying salience trigger. It cannot justify changing any threshold, half-life or weight.

**9. Social and SNA layers** — [[systems/melange-sna]], [[systems/melange-staffer-network]].
- [[steinertthrelkeld-2018-twitter-data]]: the 2016 API limits do not bind, because posts come from a licensed Quorum export plus direct Bluesky collection. Metadata timing is a provenance caveat for the member-attributed corpus.
- Louvain communities (press_hcoi; Zhang's same-method) are heuristic model outputs. They are never closeness, access or introduction probabilities (D-025).
- **Rejected SNA proposals:** no work in this theme argues for dropping regular cosponsorship edges or for per-edge volume normalization. Kumari's follower-count and tweet-frequency features are volume proxies of the kind already rejected for traits. They are not an edge proposal.
- **Lobbying data:** if Kim-style LDA positions ([[kim-2025-measuring-interest-group]], outside this theme) are ever used, they remain an identity source, never an edge or weight source (D-021). See [[systems/melange-lobbying]].

## Gaps

**A. Paul's six stated questions and what this theme's 22 works cover**

1. **Conversion step (model output → graph node or vector).** No work specifies it.
   - The closest partial answer: build continuous measures from the label probability vector rather than the argmax label ([[ornstein-2024-train-your-stochastic]]). Melange's `run_stage5.R` currently throws that vector away.
   - [[yang-2023-getting-personal-deep]] feeds trait scores into downstream GBDT forecasts. That is a feature-vector use, not a graph-node design.
2. **Ambiguity.** [[b-2025-comparing-fine-tuned]] puts its errors on sarcastic, ambiguous and implicit text but gives no counts. [[dai-2025-large-language-model]] targets implicit stance but does not separate out ambiguous statements. The gap stays open.
3. **Inter-industry conflict over a bill.** No work in this set addresses it. Kim et al. sit outside this theme.
4. **Temporal lag / prediction.**
   - No work offers a leading indicator for positions.
   - [[zhang-2022-data-assisted-social]]'s burst detection is unvalidated.
   - [[dietrich-2019-pitch-perfect-vocal]]'s response results are observational, and the authors say they are not causal. They must not be read as validated reciprocity (D-104..D-108).
5. **Long-distance dependency in emotion models.** Peng et al. is not in this file. [[park-2025-automatic-construction-direction]] works only on headline-length text. Dai's FOL rules are a partial structural answer that is untested on long, multi-actor press releases.
6. **Venue and auditory constraints.**
   - [[mestre-2026-potential-pitfalls-audio]] and Dietrich cover alignment, channel and loudness normalization, and within-speaker baselines.
   - No work covers mixed-venue pooling (floor, interviews, hearings), which is exactly what Melange's pooled baseline does.

**B. Missing from the literature set**
- **Elite criteria.** No trait study uses a legislator criterion. Every benchmark is lay-user self-report (Azucar, Celli, Yang, Naz, Marrero) or student and dyadic samples (Marrero, Salam). Accuracy numbers are UNSTRATIFIED by population and genre relative to Melange.
- **US congressional press releases.** No work validates stance or tone on them. The nearest are SCOTUS tweets (Ornstein), Austrian German press text (Haselmayer) and Roman Urdu tweets (Khan).
- **Target-dependent valence.** No work addresses the valence hop (.689 in the two-hop decomposition) or adversarial and sign-flip targets (.571 on goldeval).
- **LLM calibration.** Beyond Ornstein's one overconfidence observation, there is no calibration study of LLM confidence.
- **Measurement invariance.** No work shows invariance across venues or administrations ([[rammstedt-2025-going-global-language]] asserts comparability procedurally).
- **Disattenuation.** Marrero applies reliability disattenuation; Melange never applies it to its validity coefficients. This is an open methodological lead.

**C. Data and record discrepancies to resolve**
- **Paul's notes versus this theme set.** His 8-work bibliography overlaps this theme on 3 works only, and those 3 are unannotated. His 4 annotated works sit elsewhere or have no slug. Peng et al. and Alkazzi and Okumura are absent from this file. The path-finding link ([[systems/melange-pathfinding]]) is document grouping, not Paul's words.
- **Conflicting stance-stack numbers across the per-work notes.**
  - The Khan note gives LinearSVC .816 macro-F1 vs transformer .757.
  - The Naz note gives "LinearSVC at 83.9%" vs .757 on 87 test rows.
  - The Taherdoost note gives the DEBATE NLI reader .839 (kappa .742, n = 87, member-grouped).
  - Which model holds .839, on which metric, needs checking against the stance_eval record before any of these figures is quoted.
- **Koutsoumpis year.** The slug says 2024, but the PDF is Psychological Bulletin 2022.
- **Self-declared gap.** Paul says his technical grounding is not yet enough for Peng et al. A second reading is planned.

## All works
- [[steinertthrelkeld-2018-twitter-data]] (2018) — Twitter as Data
- [[joo-2022-image-data-automated]] (2022) — Image as Data: Automated Content Analysis for Visual Presentations of Political Actors and Events
- [[zhang-2022-data-assisted-social]] (2022) — Big data-assisted social media analytics for business model for business decision making system competitive analysis
- [[marrero-2022-evaluating-voice-samples]] (2022) — Evaluating voice samples as a potential source of information about personality
- [[dai-2025-large-language-model]] (2025) — Large Language Model Enhanced Logic Tensor Network for Stance Detection
- [[rammstedt-2025-going-global-language]] (2025) — Going Global: 39 Language Versions of the BFI-2-XS
- [[koutsoumpis-2024-kernel-truth-text]] (2024) — The Kernel of Truth in Text-Based Personality Assessment: A Meta-Analysis of the Relations Between the Big Five and the Linguistic Inquiry and Word Count (LIWC)
- [[celli-nd-twenty-years-personality]] () — Twenty Years of Personality Computing: Threats, Challenges and Future Directions
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
- [[kumari-nd-five-personality-predictions]] () — Big Five Personality Predictions From Twitter Using Regression
- [[yang-2023-getting-personal-deep]] (2023) — Getting Personal: A Deep Learning Artifact for Text-Based Measurement of Personality
