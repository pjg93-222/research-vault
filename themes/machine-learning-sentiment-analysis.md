---
type: theme
name: "Machine Learning & Sentiment Analysis"
paper_count: 33
tags: [theme]
---

# Machine Learning & Sentiment Analysis

## Machine Learning & Sentiment Analysis: synthesis

**What the theme is for, in Paul's words.** Paul's annotated bibliography for this category makes one argument. Machine learning and NLP, run on public political text such as floor speech, press releases, public statements and lobbying filings, can produce actor-level attributes: personality traits, policy positions and emotion. Those attributes can then sit on the nodes of a social-network graph and serve political intelligence work, such as lobbying strategy, industry alignment, and how to pitch a policy to a particular member. His most repeated gap is the **conversion step**: how a model's output becomes a node attribute or vector. He also names gaps on ambiguity, inter-industry conflict, temporal lag, model limits (long-distance dependency) and venue coverage. He wrote no annotation text for the three works his bibliography shares with this theme ([[joo-2022-image-data-automated]], [[taherdoost-2023-artificial-intelligence-sentiment]], [[steinertthrelkeld-2018-twitter-data]]). The four works he did annotate (Birkenmaier and Lechner, Kim et al., Peng et al., Ramey et al.) are not among the 33 works filed here. Their ideas reach this synthesis only through cross-references such as [[ramey-2016-measuring-elite-personality]] and [[birkenmaier-2025-measuring-politicians-public]].

**What the 33 works add up to.** The corpus falls into seven strands:
- personality computing and its validity ceilings;
- LLMs used as raters of personality and position;
- stance detection (supervised classifiers vs NLI vs in-context LLMs);
- audio and voice as data;
- domain-built sentiment and emotion dictionaries;
- emotion and style in legislatures, read as performance for an audience;
- a weaker tier of platform-data tutorials and applied pipelines.

Four conclusions run across the corpus:
1. **Text-derived personality carries little signal against self-report criteria.**
   - LIWC categories explain about 5.1% of self-reported trait variance (31 samples, N = 85,724) [[koutsoumpis-2024-kernel-truth-text]].
   - Text recognition peaks at about rho .30 [[celli-nd-twenty-years-personality]].
   - Zero-shot GPT gets r ≈ .29 on 1,000 Facebook users [[peters-2024-language-models-infer]].
   - Observer and perception criteria look much stronger, but partly because raters and models share the same cues, and because of reputation [[cao-2024-language-models-personality]].
2. **For stance and position, transformer NLI models and LLMs beat bag-of-words and general dictionaries on most benchmarks.**
   - MCC: about 0.49–0.51 for bag-of-words vs 0.60–0.68 for transformers vs 0.78 for GPT-4, on about 2,135 Trump-approval items [[burnham-2023-stance-detection-practical-guide]].
   - 88.4% for GPT-3 vs 38.3% for the best dictionary on 945 SCOTUS tweets [[ornstein-2024-train-your-stochastic]].
   - Small domain-adapted NLI encoders can match LLMs [[burnham-2024-political-debate-zero]].
   - Classical models stay competitive on small labelled sets ([[naz-2025-machine-deep-learning]], and the system's own LinearSVC result).
3. **Generic sentiment lexicons misfire in political and specialised domains.** Corpus-built or crowd-built dictionaries consistently beat them ([[rice-2018-corpus-based-dictionaries]], [[haselmayer-2017-sentiment-analysis-political]], [[rheault-2016-measuring-emotion-parliamentary]]).
4. **Emotional language in legislatures is largely state and strategy, not trait.** It moves with:
   - venue and audience [[osnabrugge-2021-playing-gallery-emotive]];
   - opposition status and economic shocks [[rheault-2016-measuring-emotion-parliamentary]];
   - majority status, chamber and gender [[aroyehun-2026-politics-of-feelings]];
   - gender, in a way that itself changes over time [[hargrave-2022-longer-conforming-stereotypes]].

   This directly challenges any reader that pools a member's affect words across venues and calls the result a disposition.

**For Paul's systems.** Most of these works are method sources or qualifiers for components already built:
- the lexical Big Five reader, its Mairesse/RKH lineage and the LLM trait readers;
- the Political DEBATE stance reader, the supervised stance ladder, the two-hop rule and the LSD2015 tone intensity;
- the audio lane;
- the frame-alignment diagnostic.

None of them validates a component on legislators against an outcome related to lobbying. On the conversion step, the corpus offers aggregation recipes: probability vectors [[ornstein-2024-train-your-stochastic]], score averaging with NA handling [[lemens-2025-positioning-political-texts]], and chunk-then-average [[peters-2024-language-models-infer]]. It offers no validated mapping from text output to a graph node. Paul's central gap is therefore still open.

## Strands

### Personality computing and its validity ceilings

**The question.** How much Big Five signal can be recovered from language, and against which criterion?

**The lineage.** [[mairesse-2007-automatic-recognition-personality]] is the root. It trained LIWC/MRC/prosody models on two corpora: 2,479 student essays with self-reports, and 96 EAR speakers with self-reports plus ratings from 18 observers. Ramey, Klingler and Hollibaugh later ran its SMOreg models on congressional floor speech ([[ramey-2016-measuring-elite-personality]]), and the system's lexical Big Five is now fitted to those RKH estimates. Mairesse's own results are weak:
- On essays, only openness (r .33) and emotional stability (r .24) beat the mean baseline in regression.
- Ramey's footnote says RKH used the EAR observer-trained models. In Mairesse's own table, SMOreg regression on EAR observer ratings is worse than predicting the mean for every trait, with relative errors of 131.75–213.20%.
- So the reference instrument behind the system's held-out r = .350 against RKH had no demonstrated out-of-sample accuracy in its source paper. This is a single-agent reading, UNVERIFIED per DE-062.

**The meta-analyses agree on a low ceiling against self-report.**
- [[koutsoumpis-2024-kernel-truth-text]] (31 samples, N = 85,724; 52 LIWC categories):
  - Cue validity against self-report: |rho| .08–.14 and R² of about 5.1%.
  - Cue utilization against observer ratings (only 5 samples, n = 1,637): |rho| .18–.39 and R² of about 38.5%.
  - Only 20 of 260 trait-by-category markers agree in direction across the two (the "kernel of truth").
  - Formality weakens some markers.
- [[azucar-2017-predicting-personality-traits]] (16 studies of lay social-media users, searched to May 2017): pooled r of .29 (A) to .40 (E). Two cautions:
  - The figures are inflated by best-model selection.
  - They are UNSTRATIFIED across platforms, with I² ≥ 93.
  - Agreeableness is the least recoverable trait. Demographics raise "accuracy" for A and N.
- [[celli-nd-twenty-years-personality]] (PRISMA review; the screening arithmetic gives 126 papers where the paper reports 129):
  - Text Recognition state of the art is about rho .30, against a .133 baseline.
  - Computer Perception is .30, against .38 for human judges.
  - SVMs on LIWC-type features dominate.
  - The review places these tasks on Brunswik's lens model.

**Deep models do better on self-report labels, but not on politicians.**
- [[yang-2023-getting-personal-deep]] (DeepPerson):
  - AUC 75.0 vs 64.6 for the best comparator on PANDORA (1,568 Reddit users, labels median-split self-report).
  - Closed-vocabulary feature models rank lowest, yet LIWC-type concept embeddings still add about 2 pp inside the deep model.
- [[naz-2025-machine-deep-learning]] reports transformer accuracies near 90% on MBTI-labelled Kaggle data, with no split, no class balance and no variance reported, so the figures are not usable as benchmarks.

**Weak tier.**
- [[kumari-nd-five-personality-predictions]] is an unvalidated design sketch: no N, no labels, no metrics. Its volume-proxy profile features (follower count, tweet frequency) are the kind the system already rejected.
- [[rammstedt-2025-going-global-language]] documents the translation of a self-report instrument (39 BFI-2-XS versions). Its only lesson here is that comparability across administrations is asserted procedurally, not shown.

**Agreement.** The strand agrees that:
- text gives modest signal against self-report and larger agreement against observer ratings;
- Agreeableness and Openness are hard;
- Neuroticism and Emotional Stability have the weakest criteria (Mairesse's inter-observer pair r is .29; [[naz-2025-machine-deep-learning]] says datasets rarely measure N directly).

**Disagreement.** The strand splits on what the observer advantage means. Koutsoumpis reads it as cue utilization (raters and models using the same cues), not validity. Celli's and Yang's state-of-the-art framings treat it as progress.

Works: [[mairesse-2007-automatic-recognition-personality]], [[koutsoumpis-2024-kernel-truth-text]], [[azucar-2017-predicting-personality-traits]], [[celli-nd-twenty-years-personality]], [[naz-2025-machine-deep-learning]], [[yang-2023-getting-personal-deep]], [[kumari-nd-five-personality-predictions]], [[rammstedt-2025-going-global-language]]

### LLMs as raters of personality and political position

**The question.** Can instruction-tuned LLMs replace coders or trained models as measurement instruments?

**Position and sentiment.**
- [[ornstein-2024-train-your-stochastic]] (GPT-3/GPT-4 few-shot):
  - 88.4% sign accuracy on 945 SCOTUS tweets, against 64.3% for TweetNLP and 38.3% for the best dictionary.
  - Ideology scores correlate with experts at rho .92 (economic) and .80 (social) across 18 UK manifestos.
  - Costs are about 1000x lower than crowd coding.
  - Its method points matter more than these numbers: build continuous measures from the label-probability vector, not the argmax; GPT-4 (RLHF) is overconfident (>99% on one label for over 70% of posts); validate in three steps.
- [[lemens-2025-positioning-political-texts]] asks directly for a 0–100 score with NA allowed, then averages to the document or actor.
  - Overall: r .92 on 899 Congress tweets against the crowd mean, and r .97 for 117th-Congress senators against Nokken-Poole dimension 1. These figures are UNSTRATIFIED by party.
  - Within party they fall to .66–.72.
  - On the anti-subsidy group, every model except GPT-4o drops to .04–.07.

**Personality.**
- [[peters-2024-language-models-infer]] (zero-shot GPT-3.5/4, 1,000 MyPersonality users with 200+ posts, 20-message chunks averaged):
  - Mean r .29 against self-report; supervised in-domain models reach about .37.
  - Level biases by gender and age, but not rank-order biases.
  - LLM–observer agreement (about .27, n = 68) is close to self–observer agreement (.30).
- [[cao-2024-language-models-personality]] predicts *perceived* Big Five for 226 public figures from GPT-3 **name embeddings alone**:
  - Corrected r .78–.88; .53–.70 after controlling for likability and demographics.
  - So an LLM reading a famous politician carries a reputational prior that can stand in for reading the text.

**Agreement.** All four find strong concurrent correlation with human benchmarks at low cost, and all rely on concurrent validity only.

**The joint warning.** Headline correlations are between-group (party, figure fame), and observer agreement may reflect shared impressions rather than traits. No paper here tests whether de-identifying text removes the prior Cao demonstrates. Ornstein also notes that the GPT-3 models were retired in January 2024, so the results cannot be reproduced. That is a structural weakness of the whole strand.

Works: [[ornstein-2024-train-your-stochastic]], [[lemens-2025-positioning-political-texts]], [[peters-2024-language-models-infer]], [[cao-2024-language-models-personality]]

### Stance detection: supervised vs NLI vs in-context LLMs

**The question.** Which classifier family should classify a document's position toward a target?

**[[burnham-2023-stance-detection-practical-guide]]** defines stance as entailment, separate from sentiment, and benchmarks three families on about 2,135 Trump-approval items scored by MCC:
- bag-of-words: .49–.51;
- fine-tuned transformers: .60–.68;
- zero-shot NLI: DeBERTaV3 base .40, large .65;
- GPT-4: .78–.80.

The key finding concerns context. Human coders disagree more when the target is not named (34.0% vs 19.5%), and NLI degrades most in that case. Burnham recommends NLI as the reproducible default. He treats LLMs as tools for expanding or validating training data, not as the main label source.

**[[burnham-2024-political-debate-zero]]** releases Political DEBATE (DeBERTa V3 base 86M and large 304M, trained on PolNLI, 201,691 documents) — the checkpoint behind the system's CU press-stance reader.
- On quote-tweet stance: MCC .29 for Claude 3.5, .62 for base, .88 for large.
- 10–25 labelled examples often suffice.
- But the base model failed a bespoke concept (COVID threat minimization) even at 100 examples, while the large model matched the supervised classifier (MCC .66) at 10.

**[[b-2025-comparing-fine-tuned]]** (Khan et al., Roman Urdu tweets; the method source for `train_stance_domain.py`) claims RoBERTa 0.97 over random forest 0.95.
- RoBERTa was scored on a different, augmented test set (support 6,699 vs 1,675), so the comparison is not like-for-like.
- There is one split and no confidence intervals.
- On Paul's data the claim reversed: tuned LinearSVC 0.816 vs transformer 0.757 macro-F1. This is a project-side figure.

**[[dai-2025-large-language-model]]** (FOLAR) elicits first-order-logic rules from LLMs into a logic tensor network and fuses three LLMs.
- Gains are small in-target (+1.7 to +2.0 F_avg) and larger zero-shot on VAST (+4.2 and +6.2).
- No variance is reported.
- It is the soft, learned version of the system's hard-sign two-hop rule.

**Agreement.** Implicit, sarcastic or unnamed-target stance is the shared failure mode (Burnham's ambiguity finding; Khan's error notes; Dai's error on unrelated positive words). This is Paul's ambiguity gap, measured.

**Disagreement.**
- Reproducibility: Burnham argues for NLI over LLM labels; [[ornstein-2024-train-your-stochastic]] and [[lemens-2025-positioning-political-texts]] argue for LLM coding.
- Model size: Burnham 2024's own evidence favours *large* over the base checkpoint the system runs.

Works: [[burnham-2023-stance-detection-practical-guide]], [[burnham-2024-political-debate-zero]], [[b-2025-comparing-fine-tuned]], [[dai-2025-large-language-model]]

### Voice and audio as political data

**The question.** Does the voice carry trait or state information beyond the words?

**[[marrero-2022-evaluating-voice-samples]]** (614 students, about 51 s each; XGBoost with Tree-SHAP; 100-fold CV out-of-sample R²):
- joint features 10.67%, vocal alone 6.24%, LIWC alone 3.13%, averaged across the Big Five;
- the authors claim super-additivity, but the paper gives no permutation null;
- the LIWC-alone figure sits close to Koutsoumpis's 5.1% ceiling.

Melange's three one-look audio tests at N = 90 legislators (July 2026, project-side) partly reproduced it and largely failed:
- Pink-E: held-out r +.284;
- super-additivity missed every time (+0.001 and −0.004);
- voice did not measure C (r −.081);
- the A–ES halo worsened (|r| .819 and .946).

**[[salam-2022-learning-personalised-models]]** (UDIVA dyadic video, 147 participants; mean squared error, lower is better):
- per-gender models give the lowest error (0.690, against 0.818 for the generic fusion model);
- almost all of the gain comes from the gender split;
- there is one test split and no N is reported.

The transferable lesson is that acoustic features partly encode speaker sex.

**[[dietrich-2019-pitch-perfect-vocal]]** (74,158 House floor speeches, 2009–2014):
- mean F0 is z-scored within speaker as a measure of arousal;
- women rise 0.09–0.11 SD when mentioning women;
- partisans rise 0.29–0.38 SD on topics their party owns;
- it relies on roughly 120 speeches per speaker and a cross-speaker contrast.

**[[mestre-2026-potential-pitfalls-audio]]** (45 presidential and VP debates, 1960–2020):
- validates Aeneas forced alignment (85.26% agreement);
- wav2vec2 speaker ID reaches 0.914 across 156 speakers, against 0.081 for MFCC;
- warns that off-the-shelf emotion models carry gender and pitch bias.

**Agreement.** All four support within-speaker normalisation and sex conditioning. They also support treating emotion-from-voice as unreliable.

**Disagreement.** Marrero's trait claims rest on student self-report. Dietrich and Mestre deliberately measure states, not traits. Melange's own replication sides with the state reading: audio stays display-only.

**System contrast (UNVERIFIED).** Melange's neural speaker ID reached EER .226 on fresh congressional audio, against Mestre's in-domain .914 accuracy. Its Dietrich-style per-topic deviation floors (5 units per member, 2 topics) cannot detect effects of 0.1–0.4 SD.

Works: [[marrero-2022-evaluating-voice-samples]], [[salam-2022-learning-personalised-models]], [[dietrich-2019-pitch-perfect-vocal]], [[mestre-2026-potential-pitfalls-audio]]

### Domain-built sentiment and emotion dictionaries

**The question.** Can lexicons be made valid for specialised political vocabularies?

**[[rice-2018-corpus-based-dictionaries]]**: seed words plus corpus-trained GloVe vectors.
- Accuracy: 79.5% held out on IMDB vs about 70% for LIWC and AFINN; it needs ≥5,000 documents.
- Court opinions: justice-level ranks correlate only .29 and .16 with LIWC and AFINN, so the choice of dictionary reorders the units.
- Dictionary terms drift over time: only 19.6–34.0% carry over between Court eras.

**[[haselmayer-2017-sentiment-analysis-political]]**: a crowd-rated negativity dictionary for Austrian German (12,713 sentences, about 2,000 EUR).
- Agreement with experts on 200 sentences: r .65 at 84% coverage.
- Translated generic dictionaries score r .13–.19.
- Preprocessing choices alone move r from .13 to .65.
- Effects shrink against expert coding (−0.13 vs −0.40).

**[[rheault-2016-measuring-emotion-parliamentary]]**: GloVe seed expansion on Hansard 1909–2013.
- Recessions lower annual polarity change (−0.198, N = 104 years, pooled over government and opposition, so UNSTRATIFIED).
- Split by side, labor disputes Granger-cause polarity only through the opposition.
- The lexicon is validated only on film reviews.

**[[park-2025-automatic-construction-direction]]**: direction-dependent pairs ("cost decrease" is positive).
- Raises Financial PhraseBank weighted F1 from about .60 to .71–.82, still far below RoBERTa (.96).
- Below .50 on longer or other sets.
- Its whole-text adaptation in the system made 46 sign changes on gold, 3 good and 43 bad, and was rejected.

**[[taherdoost-2023-artificial-intelligence-sentiment]]** is a thin scoping review. Its one useful borrowed point is that neural classifiers predict well but diagnose poorly, and topic or dictionary models do the reverse.

**Agreement.** Every empirical paper here says domain construction beats generic lexicons and that validation against human coding is needed.

**The weaknesses.**
- Dictionaries cannot tell which target the sentiment is about (Rice says so; Burnham's sentiment-is-not-stance argument applies).
- They drift over time.
- They fail on long multi-actor texts (Park). This is Paul's long-distance-dependency gap, now on the lexicon side.

Works: [[rice-2018-corpus-based-dictionaries]], [[haselmayer-2017-sentiment-analysis-political]], [[park-2025-automatic-construction-direction]], [[rheault-2016-measuring-emotion-parliamentary]], [[taherdoost-2023-artificial-intelligence-sentiment]]

### Emotion and style in legislatures as audience performance

**The question.** What drives affective and stylistic language on the floor, and does it relate to outcomes?

**[[osnabrugge-2021-playing-gallery-emotive]]** (958,925 UK Commons speeches 2001–2019; 945,734 Irish Dáil speeches):
- Prime Minister's Questions speeches are 1.56–1.66 points more emotive *within the same MP* (MP fixed effects).
- Topic fixed effects do not remove the gap.
- Reading: emotive rhetoric is strategic and depends on the audience.

**[[aroyehun-2026-politics-of-feelings]]** (1,766,113 Congressional Record speeches, 1973–2024; transformer emotion classifier built on German political text):
- Nearly all emotions rise in both parties.
- Topic effects are tiny (epsilon² .013–.034).
- Majority status, chamber, gender and ideology shift emotion scores.
- Against Volden-Wiseman LES (member-Congress n = 13,671), anger is the only negative correlate. Emotion lifts marginal R² only from about .113 to .119.

**[[decter-frain-2016-impressive-words-linguistic]]** (206 months 1996–2014, House floor language against Gallup approval of Congress):
- Broad communion correlates negatively (r −.36).
- The prosocial (helping) facet correlates positively (r +.55, β +.14 with controls).
- So "communion" splits into facets that behave in opposite directions.

**[[hargrave-2022-longer-conforming-stereotypes]]** (UK Commons 1997–2019, 418,147 MP-debate observations):
- Gender gaps in communal and agentic styles shrank for six of eight styles: gender explains 29–60% less of the between-MP variation after 2007.
- The negative-emotion gap grew (gender explains about 2% of the variation before 2007, about 9% after).

**Agreement.** All four are observational and say so. All four find affect and style rates moving with venue, role, period and gender. That fits Paul's call for venue-specific frameworks and argues against treating pooled affect rates as stable traits.

**Disagreement, on what the signal is worth.**
- Aroyehun finds emotion barely adds to effectiveness models.
- Decter-Frain finds facet-level language tracks institutional approval.
- Neither outcome is a member's receptivity to lobbying.

Works: [[osnabrugge-2021-playing-gallery-emotive]], [[aroyehun-2026-politics-of-feelings]], [[decter-frain-2016-impressive-words-linguistic]], [[hargrave-2022-longer-conforming-stereotypes]]

### Platform data, applied pipelines and persuasion

**The question.** How do you acquire and use platform data, and what is it for?

**[[steinertthrelkeld-2018-twitter-data]]** is a 2016-era Twitter API tutorial. It covers the 1% stream sample, the 3,200-tweet cap and metadata taken at request time. These limits do not bind on the system, which uses a licensed Quorum export and Bluesky collection. Two points carry over: 2016 elite coverage (every Senator and 430 House members) and the provenance timing of metadata.

**[[joo-2022-image-data-automated]]** is the image counterpart by the same author:
- 15,647 candidate Facebook photos;
- chi-squared tests of label against party, run separately by gender, with no correction for testing about 1,700 labels;
- a near-duplicate framing rate across five protest episodes, with hypotheses holding in 3 of 5.

The system ported both instruments to press text (`frame_alignment.py`), and the port breaks on the unit of analysis:
- 816 of 2,388 rows are americascreditunions.org articles attributed to every member they mention.
- In a single-path recompute (UNVERIFIED), the General CU duplication rate falls from 60.2% (n = 442) to 5.2% (n = 116) once only official .senate.gov and .house.gov rows are kept.

**[[zhang-2022-data-assisted-social]]** chains generic components:
- the general Affin lexicon;
- Louvain on a co-occurrence graph thresholded at 0.1;
- a burst hotness index;
- 882,371 retail tweets;
- no holdout, no baseline and no metrics behind its "highest accuracy" claim.

**[[simchon-2024-persuasive-effects-political]]** (four UK Prolific experiments, n = 440–804):
- Matching an ad's openness to the reader's openness raises *perceived* persuasiveness slightly (b −0.07 to −0.08 per SD of mismatch).
- The effect sits in the trait-by-message **interaction**.
- The outcome is self-reported perceived persuasion, not attitude change.

**Shared weakness.** Thin or absent validation. [[celli-nd-twenty-years-personality]] separately lists personality-aware persuasion in elections as an ethical threat, which bears on how Simchon-style ideas are framed.

Works: [[steinertthrelkeld-2018-twitter-data]], [[joo-2022-image-data-automated]], [[zhang-2022-data-assisted-social]], [[simchon-2024-persuasive-effects-political]]

## Agreements and debates

**1. Classical vs transformer vs LLM: which wins?**
- *The majority view* is that transformers and LLMs win:
  - bag-of-words MCC .49–.51 vs transformers .60–.68 vs GPT-4 .78 [[burnham-2023-stance-detection-practical-guide]];
  - GPT-3 88.4% vs dictionary 38.3% [[ornstein-2024-train-your-stochastic]];
  - LLMs > BERT > tf-idf on tweets (overall r) [[lemens-2025-positioning-political-texts]];
  - RoBERTa .96 vs lexicon .82 [[park-2025-automatic-construction-direction]].
- *The dissent:*
  - [[naz-2025-machine-deep-learning]] finds classical models competitive on small labelled sets.
  - The system's own CU stance stack measured LinearSVC 0.816 > transformer 0.757 macro-F1 (project-side, held-out n = 87–156).
  - The pro-transformer claim in [[b-2025-comparing-fine-tuned]] does not compare like with like, because RoBERTa was scored on a different test set.
- *Resolution the corpus supports:* the winner depends on how many clean labels you have and whether the concept is bespoke. [[burnham-2024-political-debate-zero]] shows the base NLI model failing a bespoke concept that the large model learns from 10 examples.

**2. Are LLM labels a primary measurement or a validation aid?**
- [[ornstein-2024-train-your-stochastic]] and [[lemens-2025-positioning-political-texts]] treat LLM coding as the instrument.
- [[burnham-2023-stance-detection-practical-guide]] argues against making LLMs the main label source, on reproducibility and archiving grounds. Ornstein concedes that point: GPT-3 was retired in January 2024.
- [[dai-2025-large-language-model]] fuses several LLMs, a design that implicitly treats any one LLM's labels as biased.

**3. Self-report vs observer criteria for personality.**
- [[koutsoumpis-2024-kernel-truth-text]]: observer R² of about 38.5% vs self-report about 5.1%.
- [[mairesse-2007-automatic-recognition-personality]]: observer ratings are easier to model.
- [[peters-2024-language-models-infer]]: LLM–observer agreement (about .27) is close to self–observer agreement (.30).
- These look like support for observer gold panels. But Koutsoumpis warns that observer convergence is partly cue *utilization*, and [[cao-2024-language-models-personality]] shows perceived personality is predictable from a name alone.
- Live tension: is the system's convergence with the Senate panels validity, or shared cues plus reputation?

**4. Sentiment is not stance.**
- [[burnham-2023-stance-detection-practical-guide]] separates the two explicitly.
- [[taherdoost-2023-artificial-intelligence-sentiment]] and [[zhang-2022-data-assisted-social]] collapse them.
- [[ornstein-2024-train-your-stochastic]] measures sentiment toward rulings, not stance, so its accuracy should not be read as stance accuracy.
- [[park-2025-automatic-construction-direction]] and [[dai-2025-large-language-model]] each attack the gap between valence and direction from a different side (lexical direction pairs vs soft logic).

**5. Generic vs domain dictionaries.**
- Consensus for domain dictionaries: [[rice-2018-corpus-based-dictionaries]], [[haselmayer-2017-sentiment-analysis-political]], [[rheault-2016-measuring-emotion-parliamentary]], [[osnabrugge-2021-playing-gallery-emotive]], [[hargrave-2022-longer-conforming-stereotypes]].
- Only [[zhang-2022-data-assisted-social]] applies a general lexicon (Affin) without checking it, and Rice's evidence contradicts that practice.
- The live disagreement is whether dictionaries should survive at all. Ornstein's 38.3% suggests not for direction. [[taherdoost-2023-artificial-intelligence-sentiment]]'s point that neural models predict while dictionaries diagnose suggests keeping them only for transparent intensity.

**6. Does voice add to text?**
- [[marrero-2022-evaluating-voice-samples]] claims super-additivity in students.
- Melange's N = 90 legislator replication did not reproduce it (project-side).
- [[dietrich-2019-pitch-perfect-vocal]] and [[mestre-2026-potential-pitfalls-audio]] use voice for *state*, not trait. Mestre says emotion models are biased by gender and pitch.

**7. Trait or performance?**
- Trait readers ([[mairesse-2007-automatic-recognition-personality]], [[yang-2023-getting-personal-deep]]) assume a stable person-level signal.
- The legislative-speech papers find affect varies with:
  - audience [[osnabrugge-2021-playing-gallery-emotive]];
  - opposition role and recession [[rheault-2016-measuring-emotion-parliamentary]];
  - majority status, chamber and gender [[aroyehun-2026-politics-of-feelings]];
  - era [[hargrave-2022-longer-conforming-stereotypes]].

  [[koutsoumpis-2024-kernel-truth-text]]'s formality moderator points the same way.

**8. Main effects vs interaction in persuasion.**
- [[simchon-2024-persuasive-effects-political]] places the effect in the trait-by-message interaction. That differs from a model where traits act as person-level main effects. The population (UK public rating ads) and the effect size (small) limit transfer.

## Methods

**Closed-vocabulary rates (LIWC, MRC, LSD2015, ANEW)**
- Used in: [[mairesse-2007-automatic-recognition-personality]], [[koutsoumpis-2024-kernel-truth-text]], [[decter-frain-2016-impressive-words-linguistic]], [[marrero-2022-evaluating-voice-samples]].
- Weaknesses:
  - small effects against self-report (about 5% R² in the meta-analysis);
  - sensitivity to formality, genre and spoken vs written text;
  - no target attribution.

**Corpus-expanded seed dictionaries (GloVe or skip-gram)**
- Used in: [[rice-2018-corpus-based-dictionaries]], [[rheault-2016-measuring-emotion-parliamentary]], [[osnabrugge-2021-playing-gallery-emotive]], [[hargrave-2022-longer-conforming-stereotypes]]. Crowd-built and PMI-built variants: [[haselmayer-2017-sentiment-analysis-political]], [[park-2025-automatic-construction-direction]].
- Weaknesses:
  - validation is often outside the domain (Rheault validates only on film reviews);
  - drift over time (Rice: 19.6–34% of terms carry over between eras);
  - corpus-size floors (≥5,000 documents);
  - failure on long multi-actor texts (Park).

**Supervised classifiers (TF-IDF with SVM, LR or RF, up to fine-tuned RoBERTa or DeBERTa)**
- Used in: [[b-2025-comparing-fine-tuned]], [[burnham-2023-stance-detection-practical-guide]], [[naz-2025-machine-deep-learning]], [[yang-2023-getting-personal-deep]].
- Weaknesses:
  - single splits with no CIs or seeds (Khan, Naz);
  - test sets that differ between models;
  - median-split self-report labels (Yang);
  - MBTI labels mapped to the Big Five with no stated mapping (Naz).

**NLI zero-shot and few-shot**
- Used in: [[burnham-2023-stance-detection-practical-guide]], [[burnham-2024-political-debate-zero]].
- Weaknesses:
  - degrades when the target is not named;
  - sensitive to hypothesis wording;
  - the base model fails on bespoke concepts;
  - PolNLI labels were validated by GPT-4o, so GPT-4o could not be benchmarked.

**In-context LLM prompting**
- Used in: [[ornstein-2024-train-your-stochastic]], [[lemens-2025-positioning-political-texts]], [[peters-2024-language-models-infer]], [[dai-2025-large-language-model]]. Embeddings variant: [[cao-2024-language-models-personality]].
- Weaknesses:
  - concurrent validity only;
  - headline correlations are between-group and drop sharply within party (Le Mens: .92/.97 overall, UNSTRATIFIED by party, vs .66–.72 within);
  - RLHF overconfidence (GPT-4 puts >99% on one label for over 70% of posts);
  - level biases by gender and age (Peters);
  - model retirement, which makes results irreproducible;
  - a reputational prior for famous names (Cao).

**Acoustic features (F0, MFCC, eGeMAPS, wav2vec2)**
- Used in: [[dietrich-2019-pitch-perfect-vocal]], [[mestre-2026-potential-pitfalls-audio]], [[marrero-2022-evaluating-voice-samples]], [[salam-2022-learning-personalised-models]].
- Weaknesses:
  - recording-channel and microphone confounds;
  - features that partly encode speaker sex;
  - in-domain accuracy that does not transfer;
  - small within-speaker effects (0.1–0.4 SD) that need about 100+ speeches per speaker.

**Meta-analysis and review**
- Used in: [[koutsoumpis-2024-kernel-truth-text]], [[azucar-2017-predicting-personality-traits]], [[celli-nd-twenty-years-personality]], [[naz-2025-machine-deep-learning]], [[taherdoost-2023-artificial-intelligence-sentiment]].
- Weaknesses:
  - I² ≥ 82–96%;
  - best-model selection inflates pooled effects (Azucar);
  - screening arithmetic that does not reconcile (Celli, Naz, Taherdoost);
  - pooling across platforms that is UNSTRATIFIED.

**Observational regression on legislative text**
- Used in: [[aroyehun-2026-politics-of-feelings]], [[osnabrugge-2021-playing-gallery-emotive]], [[rheault-2016-measuring-emotion-parliamentary]], [[decter-frain-2016-impressive-words-linguistic]], [[hargrave-2022-longer-conforming-stereotypes]], [[dietrich-2019-pitch-perfect-vocal]].
- Weaknesses:
  - no identification;
  - reverse causation not addressed;
  - tiny marginal R² gains (Aroyehun: about .113 to .119);
  - coefficients pooled over government and opposition (Rheault), which must be quoted as UNSTRATIFIED.

**Cross-cutting.** Almost no paper reports calibration, test–retest reliability of the machine score, or validity within the group of interest. [[marrero-2022-evaluating-voice-samples]] and [[koutsoumpis-2024-kernel-truth-text]] disattenuate their effects for reliability. The system never applies that correction to its own validity coefficients.

## How the works relate

- [[celli-nd-twenty-years-personality]] — cites — [[mairesse-2007-automatic-recognition-personality]]: Celli cites Mairesse 2007 (ref 179) as a foundation of personality computing with accuracy below 62%.
- [[celli-nd-twenty-years-personality]] — cites — [[peters-2024-language-models-infer]]: Celli's state-of-the-art table uses Peters and Matz's zero-shot GPT r = .29 on Facebook posts.
- [[peters-2024-language-models-infer]] — cites — [[azucar-2017-predicting-personality-traits]]: Peters cites Azucar et al. as ref 15 for the digital-footprint benchmark.
- [[naz-2025-machine-deep-learning]] — cites — [[yang-2023-getting-personal-deep]]: Naz cites DeepPerson as the deep-learning artifact for text-based personality measurement.
- [[naz-2025-machine-deep-learning]] — extends — [[azucar-2017-predicting-personality-traits]]: Naz covers ML and DL methods after Azucar's May 2017 search cutoff, but adds no pooled effect sizes.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — [[azucar-2017-predicting-personality-traits]]: parallel meta-analyses of Big Five recovery: closed-vocabulary LIWC (about 5.1% R² against self-report) and multi-type social-media footprints (r .29–.40, best-model inflated).
- [[koutsoumpis-2024-kernel-truth-text]] — agrees — [[mairesse-2007-automatic-recognition-personality]]: both find observer ratings easier to recover from LIWC-type features than self-reports. Koutsoumpis quantifies the gap as cue utilization vs cue validity.
- [[koutsoumpis-2024-kernel-truth-text]] — agrees — [[marrero-2022-evaluating-voice-samples]]: Marrero's LIWC-alone 3.13% out-of-sample R² sits at the meta-analytic ceiling of about 5% for self-report.
- [[yang-2023-getting-personal-deep]] — complements — [[koutsoumpis-2024-kernel-truth-text]]: Yang shows closed-vocabulary linear scoring is the weakest family, yet LIWC concept embeddings add about 2 pp inside a deep model, consistent with a small but real LIWC signal.
- [[kumari-nd-five-personality-predictions]] — same-method — [[naz-2025-machine-deep-learning]]: both describe shallow-ML and transformer trait prediction from social text. Kumari lists the models without comparing them or reporting any metric.
- [[rammstedt-2025-going-global-language]] — complements — [[koutsoumpis-2024-kernel-truth-text]]: Rammstedt documents comparability of a self-report instrument procedurally; Koutsoumpis supplies the validity side for text-derived traits that Rammstedt omits.
- [[cao-2024-language-models-personality]] — complements — [[peters-2024-language-models-infer]]: Peters reads traits from users' text; Cao shows LLM perception of public figures is predictable from names alone, a reputational prior that bears on how LLM–observer agreement is read.
- [[lemens-2025-positioning-political-texts]] — builds-on — [[ornstein-2024-train-your-stochastic]]: Le Mens benchmarks against Ornstein's GPT-3 probability-based manifesto scaling (rho .92/.80) and replaces token probabilities with a direct 0–100 score query.
- [[lemens-2025-positioning-political-texts]] — same-method — [[peters-2024-language-models-infer]]: both ask an LLM about each short text and average the answers to place a person or document.
- [[burnham-2024-political-debate-zero]] — builds-on — [[burnham-2023-stance-detection-practical-guide]]: same first author; DEBATE operationalizes the guide's NLI-as-default recommendation and cites its finding that larger NLI models generalize better.
- [[burnham-2023-stance-detection-practical-guide]] — contradicts — [[ornstein-2024-train-your-stochastic]]: Burnham advises against LLMs as the primary label source, on reproducibility grounds; Ornstein advocates LLM few-shot coding as the instrument, though it concedes that its GPT-3 models were retired.
- [[burnham-2023-stance-detection-practical-guide]] — agrees — [[b-2025-comparing-fine-tuned]]: both rank fine-tuned transformers above bag-of-words classifiers for political-tweet stance. Burnham's comparison is like-for-like by MCC; Khan's is not, because RoBERTa was scored on a different test set.
- [[naz-2025-machine-deep-learning]] — contradicts — [[b-2025-comparing-fine-tuned]]: Naz concludes classical TF-IDF models stay competitive on small labelled data; Khan headlines that the transformer beats all classical models.
- [[dai-2025-large-language-model]] — complements — [[burnham-2024-political-debate-zero]]: alternative designs for the same stance-reader slot: fused LLM-elicited soft logic vs a small domain-adapted NLI encoder.
- [[lemens-2025-positioning-political-texts]] — complements — [[burnham-2024-political-debate-zero]]: generative direct-query positioning and NLI classification are competing options for the system's stance reader.
- [[park-2025-automatic-construction-direction]] — complements — [[dai-2025-large-language-model]]: both address valence vs direction: Park via direction-dependent lexicon pairs, Dai via LLM-elicited first-order-logic rules. Dai's error analysis names clause-level direction failures that Park targets.
- [[park-2025-automatic-construction-direction]] — complements — [[rice-2018-corpus-based-dictionaries]]: both build domain lexicons from corpus statistics: PMI on direction tags vs seed words plus GloVe similarity.
- [[rice-2018-corpus-based-dictionaries]] — same-method — [[rheault-2016-measuring-emotion-parliamentary]]: both use seed words expanded by corpus-trained GloVe vectors to build domain sentiment lexicons.
- [[haselmayer-2017-sentiment-analysis-political]] — complements — [[rice-2018-corpus-based-dictionaries]]: both argue for domain dictionaries. Haselmayer derives word scores from crowd-rated sentences, Rice from embeddings.
- [[rice-2018-corpus-based-dictionaries]] — contradicts — [[zhang-2022-data-assisted-social]]: Rice shows general lexicons misfire on specialized vocabulary; Zhang applies the general Affin lexicon to retail tweets with no domain check.
- [[ornstein-2024-train-your-stochastic]] — complements — [[haselmayer-2017-sentiment-analysis-political]]: Ornstein's LLM coding at about 1000x lower cost is an alternative to the crowdcoding step Haselmayer uses to build and validate intensity labels.
- [[osnabrugge-2021-playing-gallery-emotive]] — builds-on — [[rheault-2016-measuring-emotion-parliamentary]]: Osnabrugge takes its 2001–2014 Commons speech data from Rheault et al. and cites them for embedding-based seed scoring.
- [[osnabrugge-2021-playing-gallery-emotive]] — same-method — [[rice-2018-corpus-based-dictionaries]]: Osnabrugge expands its ANEW dictionary following Rice and Zorn's corpus-based approach.
- [[hargrave-2022-longer-conforming-stereotypes]] — cites — [[osnabrugge-2021-playing-gallery-emotive]]: Hargrave cites Osnabrugge et al. on emotive rhetoric in parliaments.
- [[hargrave-2022-longer-conforming-stereotypes]] — same-method — [[rice-2018-corpus-based-dictionaries]]: Hargrave cites Rice and Zorn for its embedding-expanded seed dictionaries.
- [[aroyehun-2026-politics-of-feelings]] — cites — [[rheault-2016-measuring-emotion-parliamentary]]: Aroyehun cites Rheault et al. for emotion in parliamentary debate and moves the question to the US Congress with a transformer classifier.
- [[aroyehun-2026-politics-of-feelings]] — cites — [[osnabrugge-2021-playing-gallery-emotive]]: Aroyehun cites Osnabrugge for emotive rhetoric; both read affect as varying with institutional context rather than as fixed member traits.
- [[decter-frain-2016-impressive-words-linguistic]] — complements — [[aroyehun-2026-politics-of-feelings]]: both link congressional floor language to an outcome: chamber-month approval vs member-level legislative effectiveness.
- [[decter-frain-2016-impressive-words-linguistic]] — complements — [[hargrave-2022-longer-conforming-stereotypes]]: both score agentic and communal style in parliamentary speech. Decter-Frain splits communion into opposing facets; Hargrave shows gender gaps in those styles change over time.
- [[dietrich-2019-pitch-perfect-vocal]] — complements — [[aroyehun-2026-politics-of-feelings]]: acoustic (within-speaker F0) and textual measures of emotional intensity in US congressional floor speech.
- [[mestre-2026-potential-pitfalls-audio]] — same-method — [[dietrich-2019-pitch-perfect-vocal]]: both normalize pitch within speaker to read emotional state from political audio; Mestre cites Dietrich for pitch tracking emotional state.
- [[marrero-2022-evaluating-voice-samples]] — complements — [[salam-2022-learning-personalised-models]]: both predict self-reported Big Five from voice or multimodal features. Salam shows most of the gain comes from conditioning on speaker sex.
- [[dietrich-2019-pitch-perfect-vocal]] — complements — [[marrero-2022-evaluating-voice-samples]]: Dietrich measures a vocal state (arousal) in Congress; Marrero tests vocal trait information in students. They answer different questions.
- [[mestre-2026-potential-pitfalls-audio]] — complements — [[joo-2022-image-data-automated]]: same 'X as data' methods-tutorial genre (audio vs images) for political research.
- [[joo-2022-image-data-automated]] — complements — [[steinertthrelkeld-2018-twitter-data]]: same author (Steinert-Threlkeld) and matching tutorial format, extending from tweet text to images of political actors.
- [[taherdoost-2023-artificial-intelligence-sentiment]] — cites — [[zhang-2022-data-assisted-social]]: the review cites a 2022 Inf Process Manag social-media analytics paper for competitive decisions that appears to be the same work, with the first author listed differently.
- [[taherdoost-2023-artificial-intelligence-sentiment]] — complements — [[b-2025-comparing-fine-tuned]]: Khan supplies the direct transformer-vs-classical comparison the review only narrates, on political rather than consumer text.
- [[simchon-2024-persuasive-effects-political]] — complements — [[azucar-2017-predicting-personality-traits]]: Simchon's microtargeting assumes recipient traits can be inferred; Azucar quantifies how well digital footprints recover them.
- [[simchon-2024-persuasive-effects-political]] — complements — [[koutsoumpis-2024-kernel-truth-text]]: the LIWC–Big Five ceiling bounds how much openness signal text-based scoring can carry, for both ad scoring and author-trait readers.

## What it means for the systems

All system-side figures below are the project's own reports. Bearings tagged UNVERIFIED are single-agent leads, not findings (DE-062).

Systems touched: [[systems/melange-psychometrics]] (lexical Big Five, LLM readers, affective N, receptivity, audio lane), [[systems/melange-stance]] (DEBATE NLI, supervised stack, two-hop rule, LSD tone), [[systems/melange-sna]] (frame alignment, salience, communities), [[systems/melange-lobbying]] (LDA identity-only, D-021), [[systems/moneyball-rank-pillars]] (press pillar).

### Melange psychometrics: lexical Big Five, LLM readers, affective N, receptivity

- **Lexical Big Five (`big5_traits.py`).** This is the Mairesse → Ramey closed-vocabulary pipeline applied to formal congressional speech.
  - [[koutsoumpis-2024-kernel-truth-text]], [[azucar-2017-predicting-personality-traits]], [[celli-nd-twenty-years-personality]] and [[yang-2023-getting-personal-deep]] set a low ceiling on this method.
  - [[mairesse-2007-automatic-recognition-personality]] adds a sharper qualifier (UNVERIFIED). The EAR observer SMOreg models that RKH reportedly used were worse than predicting the mean in their own paper. So the held-out r = .350 against RKH measures agreement with a same-method reference, not validity.
  - This supports existing choices: moving receptivity off the lexical basis onto behaviour, token shrinkage, and status labels on any external claim.
  - The .350 must never be presented as matching Azucar's r .29–.40. The populations, criteria and designs differ, and Azucar's figures are UNSTRATIFIED across platforms.
- **Observer gold panels.** [[koutsoumpis-2024-kernel-truth-text]] (cue utilization), [[peters-2024-language-models-infer]] (LLM–observer ≈ self–observer) and [[cao-2024-language-models-personality]] (name-based reputational prior) together say that convergence with the Senate expert and insider panels can partly reflect shared cues and halo. This fits the CT-CM halo-plus-traits design.
- **LLM trait reader.**
  - [[peters-2024-language-models-infer]] backs the chunk-then-average design and sets a realistic zero-shot ceiling of r ≈ .29 against self-report.
  - [[cao-2024-language-models-personality]] is the motivation for de-identification. No work tests whether de-identification removes the prior, and the repo's `--named` probe has no output artifact (UNVERIFIED).
  - The code cites Cao as PNAS Nexus with r .76–.87. The vault copy is Scientific Reports with corrected r .78–.88, so the citation needs checking.
- **Affective N and the persona channel.** The slot controls for party, but this corpus shows affect and style vary with:
  - venue: [[osnabrugge-2021-playing-gallery-emotive]];
  - opposition role and period: [[rheault-2016-measuring-emotion-parliamentary]];
  - majority status, chamber and gender: [[aroyehun-2026-politics-of-feelings]];
  - gender, changing over time: [[hargrave-2022-longer-conforming-stereotypes]];
  - gender bias in LLM inferences: [[peters-2024-language-models-infer]].

  Pooling social posts, floor speech and interviews per member before z-scoring is exactly what these papers caution against. The interview-to-floor transfer of N has already failed. Paul's venue-coverage gap is directly supported.
- **Communion is not one construct.** [[decter-frain-2016-impressive-words-linguistic]] shows communion splits into facets with opposite signs. The persona codebook sits near the helping facet. The paper's outcome is institutional approval, not receptivity, so it neither validates nor undercuts the PERSONA_W = 0.30 mapping. It offers a cheap dictionary second path for checking the LLM cue shares.
- **Receptivity layer.** [[simchon-2024-persuasive-effects-political]] places persuasion in a trait-by-message interaction, while the layer uses main effects. This is a modelling gap to note, not a recommendation: the population, outcome and effect size do not transfer.

### Melange stance: DEBATE NLI, supervised stack, two-hop rule, LSD tone

- **DEBATE reader.** [[burnham-2024-political-debate-zero]] is the checkpoint itself, and it points to four open choices:
  - base vs large, given that base failed bespoke concepts;
  - short single-condition hypotheses vs the compound neutral hypothesis;
  - MCC against humans vs accuracy and kappa against Claude pre-labels;
  - per-label probabilities, which `run_stage5.R` discards ([[ornstein-2024-train-your-stochastic]]).

  [[burnham-2023-stance-detection-practical-guide]] explains why zero-shot missed implicit anti-CU stance (oppose F1 0.08) and why fine-tuning on 552 labels fixed it. [[lemens-2025-positioning-political-texts]] adds two things the stack lacks: NA kept separate from neutral, and validity reported within party.
- **Supervised stack.** [[b-2025-comparing-fine-tuned]] is cited as the method source (`train_stance_domain.py`). Its transferable value is the evaluation conventions: an 'unrelated' class and macro-F1. Its ranking claims did not hold on Paul's data. The code docstring should flag that RoBERTa was scored on a different test set.
- **Two-hop rule and bill sign.**
  - [[dai-2025-large-language-model]] is the soft, learned version of the hard-sign registry. Its fusion result suggests a cheap check on single-labeller bias in the Claude pre-labels.
  - [[park-2025-automatic-construction-direction]] explains the rejected 3-good / 43-bad whole-text override. Its evidence transfers only to short headline-scope text, which is the system's open sentence-scope option.
- **LSD2015 tone intensity.**
  - [[rice-2018-corpus-based-dictionaries]] shows domain terms ('credit' positive; 'debt*', 'risk*', 'fraud*' negative) are scored unmasked. A one-path check (UNVERIFIED) found 4.5% of positive mass and 7.0% of negative mass come from these terms, with a sign flip on 4.1% of 1,563 releases and a rank correlation of .953.
  - [[haselmayer-2017-sentiment-analysis-political]] gives a cheap human-validation design for the intensity term, which so far has only a three-case smoke test.
  - [[osnabrugge-2021-playing-gallery-emotive]] warns that press heat may index performance for the public rather than commitment.
  - [[taherdoost-2023-artificial-intelligence-sentiment]]'s predictive/diagnostic split frames the current division: NLI decides direction, the dictionary supplies intensity.

### Melange audio lane

- [[marrero-2022-evaluating-voice-samples]] is the benchmark the lane tested and largely failed to reproduce at N = 90.
- [[dietrich-2019-pitch-perfect-vocal]] supports within-speaker F0 and keeping absolute F0 out of the score. The system's floors (5 units per member, 2 topics) cannot detect 0.1–0.4 SD effects.
- [[mestre-2026-potential-pitfalls-audio]] is copied in `b5_audio_neural_head.py`. Its 0.914 accuracy is in-domain debate audio and is not a target (system EER .226).
- [[salam-2022-learning-personalised-models]] supports sex-conditioned comparison. The semitone-SD fix is on an unmerged branch.
- Together these keep audio display-only. Two leads remain open: MFCC, chroma and spectral features that eGeMAPS lacks, and reliability disattenuation.

### SNA, frame alignment and salience

- [[joo-2022-image-data-automated]]'s duplication instrument, ported into `frame_alignment.py`, breaks on attribution. ACU articles credited to every member they mention inflate echo rates (UNVERIFIED recompute: General CU 60.2% → 5.2%). It reaches `salience_shock` only under `--intensity auto`, which the chain does not pass.
- [[zhang-2022-data-assisted-social]]'s burst hotness index is at most an UNVERIFIED lead for a time-varying salience trigger. Its Louvain-on-correlation-graph step adds nothing beyond `press_hcoi.py`'s existing 0.6 threshold.
- Any community or rank output from such graphs is a heuristic model output. Per D-025 it is never closeness, access or introduction probability.
- No work in this theme argues for dropping regular cosponsorship edges or for per-edge volume normalization, so there is nothing to flag on those REJECTED proposals.
- Nothing here bears on reciprocity, which remains unvalidated (D-104..D-108).
- Paul's inter-industry-conflict gap (from Kim et al., which uses LDA filings) must respect D-021: LDA identifies actors and cannot supply edges or weights.

### Moneyball press pillar

[[haselmayer-2017-sentiment-analysis-political]] and [[osnabrugge-2021-playing-gallery-emotive]] qualify any press-tone input. Tone validity is unshown in US congressional press, and affect is audience-driven.

## Gaps

1. **Paul's conversion step is still unspecified.** The corpus offers aggregation recipes:
   - probability vectors [[ornstein-2024-train-your-stochastic]];
   - score averaging with NA handling [[lemens-2025-positioning-political-texts]];
   - chunk-then-average [[peters-2024-language-models-infer]].

   No work validates how a text-derived attribute should become a node attribute or edge in a political graph, or tests whether doing so improves any downstream prediction. [[joo-2022-image-data-automated]]'s port into the system shows the failure mode: an attribution error in the unit of analysis.
2. **No criterion of any kind for legislators.**
   - Every trait-validity number comes from students, lay users or public perception: [[azucar-2017-predicting-personality-traits]], [[peters-2024-language-models-infer]], [[marrero-2022-evaluating-voice-samples]], [[cao-2024-language-models-personality]].
   - No legislator self-report exists.
   - No work links a text-derived trait to receptivity to lobbying.
3. **Validity within the group that matters.** Headline correlations are between-party or between-figure. Only [[lemens-2025-positioning-political-texts]] reports within-party validity, and it drops to .66–.72. Nothing reports within-member, across-venue reliability.
4. **A venue-specific framework.** Paul asks for one, and [[osnabrugge-2021-playing-gallery-emotive]], [[koutsoumpis-2024-kernel-truth-text]] (formality) and [[dietrich-2019-pitch-perfect-vocal]] (a single venue) show why it is needed. No work supplies a method for harmonising floor speech, press, social posts and interviews.
5. **Staff authorship.** Member posts and press are largely staff-written. No work models the author vs speaker vs office distinction for trait or affect readings.
6. **Ambiguity and long multi-actor texts.** Ambiguity is measured: 34.0% vs 19.5% coder disagreement [[burnham-2023-stance-detection-practical-guide]]. Long multi-actor text is shown to fail [[park-2025-automatic-construction-direction]]. Neither is solved for press releases that quote the opposing side. This connects to Paul's long-distance-dependency gap from Peng et al.
7. **Temporal drift and lag.**
   - [[rice-2018-corpus-based-dictionaries]]: dictionary terms turn over between eras.
   - [[aroyehun-2026-politics-of-feelings]]: emotion trends rise over time.
   - [[hargrave-2022-longer-conforming-stereotypes]]: gender gaps change.

   No work offers a refresh or recalibration protocol. Paul's temporal-lag gap (Kim et al.) has no predictive counterpart here.
8. **Inter-industry conflict over a bill.** Nothing in this theme measures it. Any LDA-based approach must keep LDA as an identity source only (D-021).
9. **Calibration and reproducibility of LLM readers.**
   - GPT-4 overconfidence is documented [[ornstein-2024-train-your-stochastic]].
   - So are model retirement and LLM-label circularity (PolNLI was validated by GPT-4o [[burnham-2024-political-debate-zero]]).
   - No work calibrates self-reported LLM confidence, as the Claude stance rater uses.
10. **De-identification is untested.** [[cao-2024-language-models-personality]] shows the reputational prior exists. No work tests whether stripping names, party and state removes it.
11. **Causal designs.** Every legislative-speech paper is associational. Persuasion evidence uses perceived persuasion in the UK public [[simchon-2024-persuasive-effects-political]], not attitude or vote change among elites.
12. **Paul's own coverage.**
    - The three works shared with his bibliography (Joo, Taherdoost, Steinert-Threlkeld) are citation-only, with no annotation from him.
    - The four he annotated (Birkenmaier and Lechner, Kim et al., Peng et al., Ramey et al.) are not filed under this theme.
    - He plans a second reading of Peng et al. once he has more technical background.
    - The Alkazzi and Okumura path-finding link is not in his words.
    - Re-filing or annotating these works would close the gap between his stated argument and the corpus.

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
- [[mairesse-2007-automatic-recognition-personality]] (2007) — Using Linguistic Cues for the Automatic Recognition of Personality in Conversation and Text
- [[burnham-2023-stance-detection-practical-guide]] (2023) — Stance Detection: A Practical Guide to Classifying Political Beliefs in Text
- [[aroyehun-2026-politics-of-feelings]] (2026) — Politics of Feelings: Emotional Expression and Legislative Effectiveness in the U.S. Congress
- [[burnham-2024-political-debate-zero]] (2024) — Political DEBATE: Efficient Zero-shot and Few-shot Classifiers for Political Text
- [[cao-2024-language-models-personality]] (2024) — Large language models know how the personality of public figures is perceived by the general public
- [[decter-frain-2016-impressive-words-linguistic]] (2016) — Impressive Words: Linguistic Predictors of Public Approval of the U.S. Congress
- [[hargrave-2022-longer-conforming-stereotypes]] (2022) — No Longer Conforming to Stereotypes? Gender, Political Style, and Parliamentary Debate in the UK
- [[lemens-2025-positioning-political-texts]] (2025) — Positioning Political Texts with Large Language Models by Asking and Averaging
- [[osnabrugge-2021-playing-gallery-emotive]] (2021) — Playing to the Gallery: Emotive Rhetoric in Parliaments
- [[peters-2024-language-models-infer]] (2024) — Large Language Models Can Infer Psychological Dispositions of Social Media Users
- [[rheault-2016-measuring-emotion-parliamentary]] (2016) — Measuring Emotion in Parliamentary Debates with Automated Textual Analysis
