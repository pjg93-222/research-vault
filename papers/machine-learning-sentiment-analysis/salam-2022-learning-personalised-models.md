---
type: paper
title: "Learning Personalised Models for Automatic Self-Reported Personality Recognition"
authors:
  - "Hanan Salam"
  - "Viswonathan Manoranjan"
  - "Jian Jiang"
  - "Oya Celiktutan"
year: "2022"
venue: "Proceedings of Machine Learning Research (PMLR) vol. 173, pp. 53-73; Understanding Social Behavior in Dyadic and Small Group Interactions"
doi: ""
category: "Machine Learning & Sentiment Analysis"
systems:
  - melange-audio-speech
  - melange-psychometrics
components:
  - melange-audio-speech.prosody-and-disfluency
  - melange-psychometrics.lexical-big5
  - melange-psychometrics.behavioral-dispositions
paul_annotated: false
paul_markup_count: 0
evidence_status: verified
source: "[[_sources/salam-2022-learning-personalised-models.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Downloads\\salam22a.pdf"
sha12:
  - b62297c9f1d7
tags: [paper, machine-learning-sentiment-analysis, type/paper, theme/machine-learning-sentiment-analysis, evidence/verified]
---

# Learning Personalised Models for Automatic Self-Reported Personality Recognition

**Hanan Salam, Viswonathan Manoranjan, Jian Jiang, Oya Celiktutan (2022).** *Proceedings of Machine Learning Research (PMLR) vol. 173, pp. 53-73; Understanding Social Behavior in Dyadic and Small Group Interactions.* Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Salam, Manoranjan, Jiang and Celiktutan (PMLR 173, 2022) ask whether one Big Five recognizer for everyone is worse than separate models for each user profile. They test this on UDIVA dyadic video (188 interactions, 147 participants, 90.5 hours, mostly Spanish speakers). For each profile (female/male; age 30 or under/over 30) and each modality, they run an Auto-Keras architecture search. Modalities are face/body landmarks, talk-turn duration, content, sentiment and speech rate, SBERT text and MFCC audio. Predictions are then fused at the decision level. The outcome is self-reported OCEAN, scored by mean squared error. Fusing the two best modalities within each gender gives an average MSE of 0.690. Age-wise fusion gives 0.796 and generic fusion 0.818. Dyadformer scores 0.722 and the authors' earlier challenge ensemble 0.769. Almost all of the gain comes from the gender split. Age adds little, and in the generic setting fusion barely beats the best single modality (0.818 vs 0.820). Talk-turn features without sentiment beat face/body features (0.874 vs 1.325). The evidence is thin. There is one challenge test split, with no test N, subgroup sizes, intervals or seed variance. Gender is binary, the fairness claim is untested, and the text and Table 1 disagree on the headline number. For Paul's systems, the transferable lesson is narrow. Acoustic features whose signal partly is speaker sex need sex-conditioned comparison. The Melange audio lane already ran a Salam-style check: pace and volume are sex-clean, while pitch variability (r = -.266 over 1,324 clips / 309 members) and jitter (-.260) are not. The semitone-SD fix cuts the pitch correlation to -.065 but sits on an unmerged branch. The paper gives no basis for building per-sex trait models for legislators. The headline 0.690 is read from the test partition in Table 1, so selection optimism is possible.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Does learning separate neural architectures per user profile (gender, or age split at 30) improve automatic self-reported Big Five personality recognition over one generic model, using multimodal behavioural features from dyadic interactions?

**Theory:** Personality computing built on the Big Five (OCEAN) model. Premise: self-reported traits and the way traits are expressed in nonverbal behaviour differ by gender, age and culture, so one-fits-all models may be suboptimal. No formal theory is tested.

**Data:** UDIVA dataset: 188 dyadic face-to-face interactions, 147 participants, 90.5 hours of recordings, 55.1 percent male, ages 4 to 84, 22 nationalities (68 percent Spain), five interaction contexts (talk, animals game, Lego building, ghost blitz card game, gaze events). Unit of analysis: one-minute per-participant segments (25 fps, 1,500 frames). Outcome: self-reported Big Five scores. Model trained on challenge train and validation partitions; evaluated on the unseen challenge test partition. Test-set N and profile subgroup sizes are not reported.

**Method:** Per-modality features: FaceBody landmark mean and SD (552 dims); talk-turn duration (5), content (5), sentiment (10, Spanish-only library), speech rate (5); SBERT sentence embeddings; MFCC-20 band-energy statistics. For each profile (female, male; age 30 or under, over 30) and each modality, Auto-Keras neural architecture search (network morphism with Bayesian optimisation), 100 trials, up to 1,000 epochs, Adam optimiser, MSE loss, early stopping patience 30, 85/15 train/validation split, default start of two 32-unit dense layers. Decision fusion: per-minute scores averaged over sessions, then modality predictions averaged, using the best two modalities or all modalities. Comparison against generic models and challenge-team results.

**Identification:** None in the causal sense. Profile-specific models are compared with a generic model and challenge entries on one fixed test split. No significance tests, intervals, or repeated-seed variance are reported in the text.

**Scope:** Population: adult and minor participants in dyadic video interactions, mostly Spanish-speaking (dominant language Spanish, then Catalan and English). Institution: ChaLearn LAP DYAD challenge at ICCV 2021, automatic self-reported personality track. Era: 2021-2022 challenge; collection date of UDIVA not stated in text.

## Key findings

- Gender-wise bimodal NAS (fusion of two best modalities) gives the lowest AMSE in the paper; generic and age-wise bimodal fusion are higher. Numbers: AMSE 0.690 gender-wise; 0.796 age-wise; 0.818 generic fusion (Table 1 p.65; Section 4.2.1 p.64)
- Abstract's error reduction is measured against generic bimodal fusion, not the generic best single modality. Numbers: Reduction 0.128 vs generic fusion; generic best single modality 0.820, so gap 0.130 (Abstract p.53; Table 1 p.65)
- In the generic setting, bimodal fusion barely improves on the best single modality; the gain is concentrated in the gender split. Numbers: Generic fusion 0.818 vs best single 0.820; gender fusion 0.690 vs gender best single 0.695 (Table 1 p.65)
- Age-wise personalisation helps only slightly over the generic model. Numbers: Age-wise fusion 0.796 vs generic fusion 0.818 (Section 4.2.1 p.64)
- Against the challenge comparison, gender-wise bimodal NAS beats Dyadformer and the authors' prior winning ensemble on AMSE. Numbers: Gender bimodal 0.690; Dyadformer 0.722; prior SMART-SAIR ensemble 0.769; challenge baseline 0.818 (Table 2 p.66; Section 4.2.2 p.66)
- Per-trait results for gender-wise bimodal NAS; it beats the prior ensemble on all traits except Agreeableness, where the prior ensemble is ahead by 0.002. Numbers: O 0.684; C 0.588; E 0.830; A 0.550 (prior 0.548); N 0.796 (Table 2 p.66; text p.66)
- The best modality varies by trait and profile; no consistent feature wins across personalisation strategies. Numbers: e.g. Duration best for Openness generic and gender-wise; SBERT best for Openness age-wise (Section 4.2.1 pp.64-65)
- FaceBody is the weakest generic single modality, while talk-turn without sentiment and audio do better. Numbers: Generic AMSE: FaceBody 1.325; Talk Turn (NS) 0.874; Audio 0.920 (Table 1 p.65)
- Adding sentiment to talk-turn features helps in the gender split but hurts in the generic and age splits; the text had expected it to hurt. Numbers: Generic NS 0.874 vs WS 0.942; gender NS 0.844 vs WS 0.784; age NS 0.883 vs WS 1.074 (Table 1 p.65; Section 4.2.1 p.64)
- Searched architectures show no shared trend across traits or profiles; male architectures are deeper than female for Openness and Neuroticism in most features. Numbers: Parameter counts range from 236 to 302,866 across cells of Table 3 (Section 4.3 p.66; Table 3 p.67; Table 4 p.67)
- Dataset scale for the study. Numbers: 188 interactions; 147 participants; 90.5 hours; 55.1 percent male (Section 3.1 p.57)

## Limitations

- Needs gender and age metadata at inference to assign a profile (Section 5, p.68).
- Gender is treated as binary, which the authors themselves note conflicts with fairness concerns (Section 5, p.68).
- The fairness advantage over data balancing or adversarial debiasing is asserted, not tested (Section 5, p.68).
- Architectures show no interpretable trend; variation is partly attributed to random initialisation; explanation tools left to future work (Section 4.3, p.67).
- Modalities are trained separately on one-minute segments and averaged; feature-level fusion left to future work (Section 6, p.68).
- Subgroup sizes, test-set N, confidence intervals and repeated-run variance are not reported.
- Internal inconsistency: text on p.64 calls 0.796 the fused AMSE of the best model, but Table 1 gives 0.690 for gender-wise and 0.796 for age-wise.
- Sentiment uses a Spanish-only library applied to Catalan and English turns (Section 3.2.2 p.61; Section 4.2.1 p.64).
- Profile comparisons are observational; splits are not tested for significance against the generic model.
- Dyadformer row in Table 2 reports AMSE only, so the trait-level comparison is incomplete.
- Single challenge test split from one dataset, 68 percent Spain-based participants; generalisability not tested.

## Short quotes

- "our proposed approach depends on features extracted from the target subject only" (Section 2.2, pp.56-57)
- "The bimodal fusion strategy decreased the AMSE by 0.106 and 0.128 with respect to the age-wise model and the generic model" (Section 4.2.1, p.64)
- "Even though the multimodal models performed better than the unimodal ones, they did not exhibit a unified architecture" (Section 4.3, p.66)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-audio-speech#eGeMAPS prosody and Whisper disfluency rates\|eGeMAPS prosody and Whisper disfluency rates]] | qualifies | Gender-split fusion of the two best modalities gives AMSE 0.690 against 0.818 for generic bimodal fusion, a 0.128 reduction (abstract and Table 1). Age-split fusion gives 0.796. One challenge test split; no test N, subgroup sizes, intervals or seed variance reported. (Abstract p.53; Table 1 p.65; Section 4.2.1 p.64; Table 2 p.66) | b5_audio_descriptors_candidate.py: pitch_variability_raw is the eGeMAPS F0 stddevNorm (lines 27, 217). Only mean F0 is z-standardized within speaker (around lines 237-252). Percentiles pool men and women within outlet+program (around lines 315-332). Brain records r(sex=F) = -0.266 for pitch_variability_raw over 1,324 clips / 309 members, dropping to -0.065 once de-normalised; jitter r = -0.260; pace and volume sex-clean. Semitone-SD fix 4193f0140 is not an ancestor of HEAD (verified with git merge-base) and sits on branch claude/melange-speech-recognition-cfa049. `fec/members/b5_audio_descriptors_candidate.py:27,217,237-252,315-332; brain C:/Users/pgoff/Documents/brain/actors/melange-audio-pipeline.md:109,111` | The paper has no pitch or jitter result; its audio features are MFCC band statistics. The sex-conditioning lesson transfers only as a diagnostic for features whose signal partly is sex (pitch, jitter), with the denominator (1,324 clips / 309 members) named and strata or an UNSTRATIFIED label. It is not a basis for per-sex trait models: the paper's criterion is self-report in a volunteer sample of 147 dyadic participants, while Melange gold is expert and insider ratings. | weak | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | suggests-extension | Within the paper's NAS talk-turn models, adding sentiment to duration, content and speech-rate features moves AMSE from 0.874 to 0.942 (generic), 0.883 to 1.074 (age split), and 0.844 to 0.784 (gender split). Single test split, no intervals. The authors themselves expected sentiment to hurt, because their Spanish-only sentiment tool was applied to Catalan and English turns. (Table 1 p.65; Section 4.2.1 pp.64-65 (language caveat p.64)) | big5_traits.py builds lexical Big Five from word features with corpus-relative z-scores (lines 17-18), a LOWCONF_TOKENS=1500 flag (line 28), and a party-only external check (around lines 265-275). No sex-stratified check exists in the file. `fec/members/big5_traits.py:17-18,28,265-275` | The paper's sentiment features are a Spanish sentiment classifier applied to talk turns, not a lexical trait dictionary, and the paper's own language-mismatch caveat applies. The transferable item is a diagnostic only: report text-trait correlations against the target stratified by sex, or label them UNSTRATIFIED. This paper supports no per-sex models. | weak | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Behavioural dispositions\|Behavioural dispositions]] | context | Among generic single modalities, talk-turn features without sentiment (duration, content, speech rate) average AMSE 0.874 across the five traits, audio 0.920, and FaceBody 1.325 (Table 1, p.65). The paper does not isolate extraversion or dominance and does not test broadcast interviews. (Table 1 p.65; Section 4.2.1 p.64) | behavioral_dispositions.py: dominance_z is the mean of an interaction-channel term (verbal output, floor-holding Gini, floor defense; lines 84-88) and a committee-leadership z (line 89). A header comment cites turn statistics as the replicated dominance signal (line 17). `fec/members/behavioral_dispositions.py:17,84-90` | Indirect, five-trait-averaged evidence that turn structure carries trait signal in dyadic lab tasks. It does not validate dominance_z or its committee term, and says nothing about congressional floor speech. Any use should name the window and denominator. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Selection optimism is not flagged. Best-two-modality fusion and best-per-trait results are picked per profile from Table 1, which reports the unseen test partition used for inference (Section 4.2). The paper does not say a separate split did the selecting, so the 0.690 headline is likely optimistic.
> - Summary issue: Turn-taking wording is loose. The 0.874 figure is talk-turn features without sentiment (duration, content and speech rate) averaged over five traits, not turn-taking alone.
> - Summary issue: The 'nothing live is affected' claim rests on the v2 artifact having 0 accepted clips (brain line 60). The brain also records 1,410 of 2,192 audio_pilot clips feeding an anchor vault under a PASS receipt (brain line 90), so the claim is not established.
> - Summary issue: The pitch correlations (r = -.266, jitter -.260) lack their window and denominator. Brain line 109 gives 1,324 clips / 309 members, which the summary should name.
> - Summary issue: The summary omits that the semitone-SD fix cuts the pitch sex correlation from -.266 to -.065 (brain line 109). That bears on how much sex confound remains and should be stated.
> - Summary issue: 'A general population' is not what the paper says. It describes 147 volunteer participants aged 4 to 84 in dyadic video sessions, 68 percent from Spain, so the population wording should be softened.
> - Summary issue: Unverified Paul-side claims: 'Paul is interested in non-textual channels', 'a Salam benchmark is on record', and 'criteria are expert ratings' were not traced to any source in the item or brain text reviewed. Only the Salam 2022 sex-confound test (brain line 111) was confirmed. The 'display-only' audio status comes from the memory index and was not re-verified.

## Gaps and flags

- The sex-deconfounding fix for pitch_variability_raw (commit 4193f0140, plain semitone SD) is on branch claude/melange-speech-recognition-cfa049 and not in HEAD. The live candidate still percentiles stddevNorm across pooled sexes, at b5_audio_descriptors_candidate.py:27,217,320-333.
- Jitter showed a sex correlation of -.260 in the brain's Salam test, but no code path conditions it on sex.
- Lexical Big Five has no sex-stratified validity check. Only party is checked, at big5_traits.py:265-275.
- The system has no self-report Big Five criterion for legislators. So Salam-style personalisation gains, measured against self-report, cannot be scored against the expert and insider gold panels the system uses.
- Internal inconsistency in the paper: the text on p.64 calls 0.796 the best fused average MSE, but Table 1 gives 0.690 for the gender split and 0.796 for the age split. Quote figures from Table 1.
- The paper reports no test-set N, subgroup sizes, confidence intervals or repeated-seed variance.
- A pooled-over-fault-line issue in the system, not the paper: pitch_variability percentiles pool men and women despite a measured sex correlation of -.266. Under the strata rule, any quote of them needs strata or an UNSTRATIFIED label.
- The paper treats gender as binary and asserts, without testing, that personalisation beats debiasing on fairness. Do not cite it as fairness evidence.
- No conflict with the SNA, LDA, reciprocity or D-025 wording rules. The paper has no network, lobbying or access content.

Paul has no annotated-bibliography entry or markup on this work. Non-verbal and turn-taking channels carry some self-reported trait signal, but the gains are modest and come mostly from conditioning on sex. A Salam 2022 sex-confound test is on record in the brain (line 111); the one stale point is that the test's fix is not merged into HEAD.

## Related papers

- [[ramey-2016-measuring-elite-personality]] — complements — Verified: Ramey estimates Big Five from legislators' floor speech with LIWC and MRCPD closed-vocabulary features. Salam uses non-verbal and turn channels with subgroup models on self-report labels. Link kept.
- [[marrero-2022-evaluating-voice-samples]] — complements — Verified: Marrero tests Big Five self-report information from LIWC and vocal features of voice samples, with XGBoost pipelines. Link kept.
- [[dietrich-2019-pitch-perfect-vocal]] — complements — Verified: Dietrich z-scores mean F0 within each speaker in congressional speech. That is the standardization Melange applies to mean F0. Link kept.
- [[celli-nd-twenty-years-personality]] — complements — Verified loosely: a PRISMA review of personality computing that discusses bias and demographics as threats. Link kept, but the rel summary should not imply it treats the same confounds Salam tests.
- [[naz-2025-machine-deep-learning]] — complements — Corrected: Naz is a review of ML and DL trait detection in online text. It mentions multimodal and audio approaches only in passing, so same-method overstates the overlap with Salam's multimodal NAS.
- [[mestre-2026-potential-pitfalls-audio]] — complements — Verified: Mestre covers forced alignment, within-speaker pitch deviations and audio feature pitfalls for political audio. Link kept.

### Linked from
- [[rubio-2024-feasibility-data-analytics]] — complements — Real item. Melange Salam-2022 sex check (brain line 111) supports the sex-confound complement; Rubio's all-female sample cannot answer that question.
- [[mairesse-2007-automatic-recognition-personality]] — complements — Personalised models for self-reported personality recognition. It does not cite Mairesse 2007, so the link is topical only; the rel labelled it extends, which is not supported.