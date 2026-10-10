---
type: paper
title: "Using Linguistic Cues for the Automatic Recognition of Personality in Conversation and Text"
authors: ["Francois Mairesse", "Marilyn A. Walker", "Matthias R. Mehl", "Roger K. Moore"]
year: "2007"
venue: "Journal of Artificial Intelligence Research 30 (2007) 457-500"
doi: ""
category: "Machine Learning & Sentiment Analysis"
systems: ["melange-audio-speech", "melange-psychometrics"]
components: ["melange-audio-speech.prosody-and-disfluency", "melange-psychometrics.affective-n", "melange-psychometrics.gold-panel-validation", "melange-psychometrics.lexical-big5"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/mairesse-2007-automatic-recognition-personality.pdf]]"
original_paths: []
sha12: ["27d675bc4570"]
tags: [paper, machine-learning-sentiment-analysis]
---

# Using Linguistic Cues for the Automatic Recognition of Personality in Conversation and Text

**Francois Mairesse, Marilyn A. Walker, Matthias R. Mehl, Roger K. Moore (2007).** *Journal of Artificial Intelligence Research 30 (2007) 457-500.* Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Mairesse, Walker, Mehl and Moore (JAIR 30, 2007) is the source of the SMOreg personality models that Ramey, Klingler and Hollibaugh ran on congressional floor speech. Those RKH estimates are the targets the lexical Big Five weights are now fitted to. The paper trains classifiers, regressors and RankBoost rankers on LIWC, MRC, speech-act and prosody features. It uses two corpora: 2,479 student essays with self-reports, and 96 EAR speakers with self-reports plus averaged ratings from 18 observers. Three results bear on the system. (1) On essays the signal is weak. In regression, only openness (r 0.33) and emotional stability (r 0.24) beat the mean baseline, and SMOreg is significant only for openness (Table 17). The system's 07-03 essays refit landed in the same weak band and was later replaced by the RKH refit. (2) Ramey footnote 4 says RKH used the observer-trained models, which are the EAR models with n = 96. In Table 19, SMOreg on EAR observer ratings is worse than predicting the mean for every trait, with relative errors from 131.75 to 213.20 percent. The reference instrument behind the held-out r of .350 against RKH therefore had no demonstrated out-of-sample accuracy in its own paper. (3) Observer-rated personality is easier to model than self-report in conversation, and emotional stability has the lowest inter-observer agreement (pair r .29). This fits the system's choice of observer gold panels and its weak N results. Prosody helps extraversion most. In self-reports, neuroticism goes with low, constant voice intensity, which supports the 2026-07-09 finding that the audio ES composite was mis-signed. No result here was reproduced by a second path, so the bearings are UNVERIFIED (DE-062).

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Can the Big Five personality traits be recognised automatically from linguistic cues in written essays and in conversation, using self-reports and observer ratings, and which model type (classification, regression or ranking) and feature set performs best? Sec 1.

**Theory:** Lexical Hypothesis and Big Five structure (Sec 1). Psycholinguistic links between language and traits (Sec 2, Table 1): extraversion with verbosity, implicit style and positive emotion; neuroticism with self-reference and negative emotion; conscientiousness with avoiding negation. Hogan (1982) observer vs agent perspective and the Brunswikian lens model are used to justify observer ratings (Sec 8). Hypotheses: observed personality is easier to model than self-report (Sec 3.1); personality varies continuously, so regression or ranking should beat classification (Sec 7-8).

**Data:** Two corpora. (1) Essays corpus: 2479 stream-of-consciousness essays by psychology students (1.9 million words, about 766 words per subject), written in 20 minutes, with Big Five Inventory self-reports; from Pennebaker and King (1999), Sec 3.1, p.463-464. (2) EAR corpus: 96 participants, 97468 words and 15269 utterances (about 1015 words per subject), random snippets of conversation captured by an Electronically Activated Recorder; only participant utterances transcribed; audio also available. Personality by self-report and by 18 observers who rated audio on a 7-point scale, averaged per participant; mean inter-observer ICC r = 0.84 (Mehl et al. 2006), Table 4 p.465. Unit: individual author or speaker. Period: data from 1999 and 2006 studies; paper submitted 05/07. Language: English.

**Method:** Supervised learning with Weka and 10-fold cross-validation. Classification: each trait split into two equal-size bins; six learners (J48, 1-NN, Naive Bayes, JRip, AdaBoostM1, linear SVM SMO) vs majority-class baseline (Sec 4). Regression: linear regression, M5' regression tree, M5' model tree, REPTree, SVM regression vs mean baseline, scored by relative absolute error (Sec 5). Ranking: RankBoost with 100 boosting rounds trained on ordered pairs from two individuals, indicator functions from thresholded feature values, loss = share of misordered pairs; random baseline 0.50 (Sec 6). Feature-set ablations for each model type; interpretation via decision trees, rules and regression trees (Sec 4.3, 5.3, 6.3). Pearson correlations of features with ratings (p<0.05) in Sec 3.3, Tables 8-11. Speech acts tagged by heuristic parse rules (Sec 3.2.2).

**Identification:** No causal identification. Predictive accuracy on held-out data from 10-fold cross-validation, framed as recognising unseen subjects. The text does not say whether folds were grouped by speaker; this is unclear and matters for the EAR corpus. Significance from two-tailed paired t-tests at p < .05 across folds (Sec 4-6).

**Scope:** Population: psychology students (essays) and 96 EAR participants; the EAR sample is not otherwise described. Language: English. Era: data from 1999 and 2006 studies; paper 2007. Applications argued: dialogue system adaptation, meeting analysis, dating, tutoring, opinion mining (Sec 1). No generalisation test across corpora was run (Sec 8).

## Key findings

- Essays, self-reports, binary (two equal bins), 10-fold CV: SMO best. Openness 62.1 percent vs 50.3 baseline; emotional stability 57.4 percent; conscientiousness hardest, about 55 percent with only two classifiers significant. Table 12, p.475.
- EAR, observer ratings, binary: Naive Bayes reaches 73.0 percent for extraversion (baseline 47.8), 73.9 percent for emotional stability (baseline 51.1), 67.7 percent for conscientiousness and 61.3 percent for agreeableness. Openness not significant. Table 14, p.476-477.
- EAR, self-reports: no classifier significantly beats the baseline (Table 14), and no regression self-report model does either (Sec 5.2, p.482).
- EAR observer feature sets, Naive Bayes: prosody alone 67.6 percent for extraversion and LIWC alone 68.9 percent, both below the full-set 73.0 percent; MRC alone best for conscientiousness (66.8 percent); prosody alone best for openness (64.6 percent). Table 15, p.477.
- Essays, binary: LIWC features beat MRC features for every trait; LIWC alone gives 62.5 percent for openness, and LIWC alone performs slightly better than the full feature set. Table 13 and Sec 4.1, p.476.
- Essays, regression, self-reports: openness relative error 93.3 percent (M5 model tree, Pearson r 0.33) and emotional stability 96.4 percent (r 0.24); other traits not significant. Table 17, p.480.
- EAR, regression, observer ratings: extraversion relative error 79.9 percent (REPTree, r 0.54), emotional stability 86.8 percent (M5, r 0.47), conscientiousness 82.7 percent (M5R, r 0.44). Agreeableness and openness do not beat the mean baseline. Table 19, p.481-482.
- Table 20 feature ablations (EAR observer regression): LIWC alone gives extraversion 76.4 percent (REPTree) and conscientiousness 82.1 percent (M5R); prosody alone gives emotional stability 85.3 percent (M5). Table 20, p.482-483.
- Essays, ranking (loss; random 0.50): openness 0.39 (best), emotional stability 0.42, conscientiousness 0.44, extraversion 0.44, agreeableness 0.46. LIWC carries all gains and adding MRC never reduces loss. Tables 21, p.483-484.
- EAR, ranking, observer ratings: extraversion with prosody only 0.26 (best overall; the text gives a 74 percent chance of ordering two unseen extracts correctly); agreeableness with all features 0.31; conscientiousness with all features 0.33; openness with LIWC 0.37; emotional stability best with MRC only (0.39), full set 0.41 not significant. Table 22, p.485.
- Ranking vs classifier (Naive Bayes, 5 equal bins, EAR observer): RankBoost loss significantly lower on four of five traits. 5-bin accuracy is higher for ranking on agreeableness (37.8 vs 28.4) and openness (26.8 vs 19.8); Naive Bayes is higher on conscientiousness (34.7 vs 30.3); extraversion 32.3 vs 32.1 and emotional stability 21.9 vs 21.9 are ties. Table 26, p.489.
- Observer vs self: the authors conclude observed personality may be easier to model than self-reports in conversational data, with a hypothesis that high-visibility traits are better assessed by observers. Sec 8, p.492; not tested directly.

## Limitations

- The EAR corpus is small (96 participants); the authors expect more data to help and note linear regression and SVMs perform poorly there. Sec 5.2, p.482; Sec 8.
- Self-report models on EAR never beat the baseline, and openness is not significant for any EAR observer model. Table 14, Sec 5.2.
- Prosodic features were computed without filtering out other speakers picked up by the head microphone; the authors state this assumption is untested. Sec 3.2.3, p.468-469.
- Speech recognition noise was not tested; models used human transcripts. Sec 8, p.492.
- Utterance-type tagger: 88 percent accuracy on 100 utterances, command class accuracy 0.50; utterance-type features never beat the baseline on their own. Table 7, Sec 3.2.2, Sec 8.
- Learner parameters were left at Weka defaults and not optimised; the authors say optimisation may change which model wins. Sec 3.4, Sec 8, p.491.
- Gender was not used as a feature although the authors say language correlates depend on gender. Sec 8, p.492.
- Only one feature family per corpus on essays (LIWC and MRC, 102 features); no test that the feature sets generalise to another dataset. Sec 8, p.491.
- Internal inconsistencies: Sec 5.2 gives a 14.3 percent improvement for observed conscientiousness, but Table 19 M5R 82.68 implies about 17.3 percent. Sec 7 gives 63 percent SMO accuracy for EAR emotional stability, but Table 14 shows 60.33 percent. Both are reported as stated in the text and should be checked against the tables before reuse.
- Essay correlations are weak (Sec 3.3); the authors say it was not obvious that these features would improve accuracy on unseen subjects. Sec 8, p.491.
- The authors state it is unclear whether the accuracies are high enough to be useful in practice. Sec 8, p.492.

## Short quotes

- "modelling personality using continuous models is more accurate"
- "observed personality may be easier to model than self-reports, at least in conversational data"
- "ranking models perform best overall"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | contradicts | SMOreg trained on EAR observer ratings (96 speakers, 10-fold CV) has relative absolute error above the mean baseline for all five traits: E 140.05, ES 162.05, A 173.97, C 131.75, O 213.20 percent (baseline 100). The authors say linear regression and SVMs perform poorly and suggest they need a bigger dataset. (Table 19 p.482 (Sec 5.2 starts p.481); Ramey et al. 2016 footnote 4 (text line 122)) | fit_smoreg_rkh.py takes Ramey per-member trait averages as targets. big5_traits.py loads liwc/mairesse_smoreg.json as ACTIVE weights when the file exists, replacing the published-correlate weights. `fec/members/fit_smoreg_rkh.py:3-11; fec/members/big5_traits.py:186-189; fec/liwc/mairesse_smoreg.json` | Holds as a statement about the Mairesse observer model and Ramey fn 4. The paper never says what data the distributed observer model was fit on, so the claim that it was fit on all 96 speakers is an assumption. Ramey line 191 calls the training sample the essay sample, so Ramey is internally inconsistent on provenance. | strong | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | qualifies | On the essays corpus with self-reports, SMO binary accuracy is 54.9 to 62.1 percent (openness 62.1 vs 50.3 baseline). SMOreg relative error is E 100.65, ES 98.35, A 100.28, C 99.30 (none significant) and O 94.19 (significant). Best Pearson r is 0.33 for openness and 0.24 for emotional stability. (Table 12 p.475; Table 17 p.481 (Sec 5.1 p.480); Pearson r in Sec 5.1 p.481) | fit_smoreg_essays.py refits a LinearSVR on binary y/n labels (0/1 targets) and searches 12 settings on the same folds. `fec/members/fit_smoreg_essays.py:4-12; fec/members/fit_smoreg_essays.py:104-110; fec/members/fit_smoreg_essays.py:121-131` | Paper figures verified. Table 12 SMO values are E 54.93, ES 57.35, A 55.78, C 55.29, O 62.11, so the 55 to 62 range is rounded. The grid search picks the best of 12 settings on the same folds, so its CV figure is optimistic, as stated. The paper's binary accuracies are not comparable to the system's refit since the features differ (Empath-augmented). | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Big Five validation against Senate gold panels and criteria\|Big Five validation against Senate gold panels and criteria]] | supports | On EAR, observer-rated Naive Bayes beats baseline for E 73.00 vs 47.78, ES 73.89 vs 51.11 and C 67.67 vs 47.78. No self-report classifier or regressor significantly beats baseline. Mean inter-observer pair r is 0.54 for E, 0.29 for ES and 0.51 for C (18 observers, 31 to 33 pairs). (Table 14 p.476-477; Sec 4.2 p.476; Table 19 and pair r in Sec 5.2 p.482; Sec 8 is p.490-491 (rel says p.492)) | b5_gold_multitrait_validate.py evaluates predictions separately against the senate_insiders and academic_experts panels; the E and N pass results are system outputs, not checked here. `fec/members/b5_gold_multitrait_validate.py:6; fec/members/b5_gold_multitrait_validate.py:28; fec/members/b5_gold_multitrait_validate.py:211` | The EAR design is verified: observers rated audio only, without knowing the participants, in three groups, and the ratings were averaged (EAR description p.464-465). The Senate known-figure reputation point is outside the paper, and the paper never discusses Senate panels. Linking the ES observer agreement to the system's N gate is the verifier's inference. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-audio-speech#eGeMAPS prosody and Whisper disfluency rates\|eGeMAPS prosody and Whisper disfluency rates]] | supports | Prosody-only RankBoost gives extraversion ranking loss 0.26 (random 0.50), the best single feature set in ranking. Prosody-only Naive Bayes gives 67.56 percent for E, but LIWC alone (68.89) and MRC alone (68.78) are higher. Self-assessed neurotics show low and constant voice intensity (Int-mean, Int-stddev) that observers do not use. (Table 22 p.485; Table 15 p.478 (rel says p.477); Sec 3.3 p.469-470 (Table 11)) | b5_audio_eval.py defines ES_acoustic as minus the mean of the stability columns (line 97), and a 2026-07-09 note marks the composite as mis-signed. `fec/members/b5_audio_eval.py:5; fec/members/b5_audio_eval.py:8; fec/members/b5_audio_eval.py:12-14; fec/members/b5_audio_eval.py:97` | The 0.26 and 67.56 figures and the self-report intensity result are verified. The strongest-single-feature-set claim holds only for ranking; in classification LIWC and MRC are higher. EAR observers rated audio, so prosody gains are partly shared-channel, as the paper does not test. The sign correction is supported only for self-reported neuroticism, which the rel states. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Affective neuroticism slot and N genre-transfer tests\|Affective neuroticism slot and N genre-transfer tests]] | supports | In conversation only a high word count and a low mean pitch mark ES in both self and observer reports. Observed ES tracks swearing and anger words but self-assessed ES does not. Self-references and negative-emotion words mark neuroticism in essays, citing Pennebaker and King. (Sec 3.3 p.469-470, Tables 8-11) | emit_affective_n.py keeps the LIWC i + negemo - posemo term only as a convergent anchor (r +.24), not as a component. `fec/members/emit_affective_n.py:38-43` | The paper compares essays and student conversation, not political genres, so the link to floor speech is an analogy. The genre-transfer rho values (.050, .161) are not checked here and are not in the paper. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Paper figures and table cites in the rel summary largely check out against the text. Page cites are off: Table 17 is p.481, Section 8 is p.490-491, Table 15 is p.478.
> - Summary issue: Unverified: that the active mairesse_smoreg.json holds the RKH refit for all five traits. fit_smoreg_rkh.py writes liwc/rkh_calibrated.json.candidate, which is absent; the essays script writes mairesse_smoreg.json.candidate. The 'replaced trait by trait' history is not in code read here.
> - Summary issue: Overstated: the claim that big5_traits.py:13-16 describes the model as essays-trained. Those lines describe a pre-trained standardized SMOreg and say nothing about essays; essays wording is only in fit_smoreg_essays.py:4-5, which matches Mairesse's own setup. Ramey is itself inconsistent (fn 4 observer models, line 191 essay sample), so calling this a factual code error is not established.
> - Summary issue: Wrong: prosody as the strongest single feature set for extraversion holds only in ranking (Table 22). In Table 15 LIWC (68.89) and MRC (68.78) beat prosody (67.56). The summary line that prosody helps extraversion most needs the same qualification.
> - Summary issue: Unverified: the essays CV range of 53 to 58 percent and the claim that the essays weights were active on 07-03.
> - Summary issue: Unsupported in the paper: that the distributed observer model was fit on all 96 speakers; that the paper itself warns no cross-corpus test was run (search found no such statement); and that the EAR observer design is the gold standard for the system's N gate.
> - Summary issue: paul_alignment is unverifiable: paul_entries is empty, so statements about Paul's notes and position have no source here.
> - Summary issue: Outside this paper, not verified here: held-out r values vs RKH (.510 for E, .350 mean) and the acquaintance-level accuracy label, which come from brain records.

## Gaps and flags

- The docstrings at big5_traits.py:13-16 and fit_smoreg_essays.py:4-5 describe the RKH/Mairesse model as essays-trained SMOreg, but Ramey fn 4 says RKH used the observer-trained (EAR) models. The essays refit replicated the wrong arm, and the docstring still describes published-correlate linear weights that the active JSON overrides.
- No external check of RKH itself: the held-out r vs RKH (mean .350) is reported as accuracy, but the source model's own CV error on EAR observer ratings was above the mean baseline for every trait.
- fit_smoreg_essays.py regresses on binary y/n labels and reports the best of 12 grid settings on the same folds, with no nested CV.
- The component card for affective-n still lists the LIWC term as a component; the code at emit_affective_n.py:41-43 says it is now an anchor only.
- Ranking loss (pairwise order) is never used as an evaluation metric, although the paper finds ranking models best overall and the system's consumers use ranks.
- Brain wording conflict: the v6 note calls held-out r vs RKH congressional ground truth and calls E .51 acquaintance-level accuracy. Per this paper (Table 19) and Ramey fn 4, RKH is the output of a model with no demonstrated out-of-sample accuracy, so the figure is method agreement on n = 146, not accuracy. It needs an UNVALIDATED or method-shared label.
- Provenance error: system docstrings say Ramey used Mairesse's essays SMOreg; Ramey fn 4 says observer-trained models. This is an actual factual error in the code documentation.
- Ramey p.2 says SMOreg best recovers personality in written trials, but this paper's Table 17 shows SMOreg significant only for openness on essays and Table 19 shows it worse than baseline on EAR. The citation chain overstates the source.

Paul has no annotated entry for this paper. His position on the lexical component (Ramey is dated but foundational, and each venue needs its own framework) is reinforced. The model RKH inherits was trained on 96 student conversations and never shown to generalise beyond them, and the paper itself warns that no cross-corpus test was run. His notes do not mention that the RKH targets now drive all five active weights. The system's move from essays weights to an RKH refit replaced weak but real signal with agreement with an unvalidated instrument. That point is newer than his notes.

## Related papers

- [[ramey-2016-measuring-elite-personality]] — builds-on — Ramey et al. apply the pre-trained Mairesse SMOreg to floor speech (text lines 130 and 183-190). Their footnote 4 says observer-trained models; footnote 8 says SMOreg versus M5 made little difference. Their line 191 calls the training sample essay, so the provenance is inconsistent within Ramey.
- [[pennebaker-2003-psychological-aspects-natural]] — complements — Review of LIWC and personality language. It shares the LIWC dictionary with this paper but does not supply the essays data, which is Pennebaker and King 1999 (cited, not in this review).
- [[koutsoumpis-2024-kernel-truth-text]] — complements — Meta-analysis of 52 LIWC categories against the Big Five that reports small effect sizes. This supports the point about small closed-vocabulary signal; the paper does not cite Mairesse 2007.
- [[mondak-nd-online-appendix-personality]] — complements — Online appendix to expert evaluations of senators. The link to this paper is conceptual (observer or expert ratings); the appendix does not mention Mairesse 2007.
- [[salam-2022-learning-personalised-models]] — complements — Personalised models for self-reported personality recognition. It does not cite Mairesse 2007, so the link is topical only; the rel labelled it extends, which is not supported.
- [[celli-nd-twenty-years-personality]] — cites — Cites Mairesse 2007 as reference 179 and calls it the foundation of personality computing with accuracy below 62 percent. Celli discusses threats in general, not Mairesse-specific validity threats.

### Linked from
_None._