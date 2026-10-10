---
type: system
name: Melange audio and speech lane
slug: melange-audio-speech
components: [melange-audio-speech.speaker-verification, melange-audio-speech.prosody-and-disfluency, melange-audio-speech.identity-two-check-design]
brain_notes: [C:/Users/pgoff/Documents/brain/actors/melange-audio-pipeline.md, C:/Users/pgoff/Documents/brain/topics/2026-07-feature-audio-and-speech-lane-n-audio-route-and-measurements.md]
paper_count: 7
tags: [system]
---

# Melange audio and speech lane

Speaker verification from captions, eGeMAPS prosody, Whisper disfluency rates, the closed acoustic speaker-ID attempt, the proposed two-check identity design, and the display-only audio descriptor layers. No rank or engine consumer.

Brain notes: [actors/melange-audio-pipeline](obsidian://open?vault=brain&file=actors%2Fmelange-audio-pipeline); [topics/2026-07-feature-audio-and-speech-lane-n-audio-route-and-measurements](obsidian://open?vault=brain&file=topics%2F2026-07-feature-audio-and-speech-lane-n-audio-route-and-measurements)

Computed from the evidence file: 17 evidence rows from 7 distinct papers (1 VERIFIED; 16 VERIFIED-SOFTENED). Rows per component: Caption-based speaker verification 1 rows / 1 papers; eGeMAPS prosody and Whisper disfluency rates 13 rows / 7 papers; Acoustic speaker-ID (closed) and two-check identity design (proposed) 3 rows / 1 papers.

**Melange audio and speech lane: evidence assessment (2026-10-10)**

Coverage is very lopsided (counts per component are in the computed line above):

- **Prosody and disfluency:** the only component with a real evidence base (Marrero 2022, Dietrich 2019, Mestre 2026, Salam 2022, Rubio 2024, Mairesse 2007, plus the Celli survey).
- **Two-check identity design:** all rows from Mestre 2026. That paper is a method source but sets no benchmark for congressional audio.
- **Caption-based speaker verification:** one extension lead from Dietrich 2019 (Clerk HouseLive captions). Nothing measures how often caption-cue verification accepts the wrong speaker. In practice this component is a GAP, and it gates every other component, because each downstream descriptor is computed only on clips it keeps.

**Overall verdict.** The evidence agrees with Paul's ruling that audio is display-only:

- In 3 registered looks, the vocal block missed its super-additivity bar over text (Marrero-style prereg, N=90 legislators). That number is UNSTRATIFIED by sex.
- Feature-level voice-personality results did not replicate across two samples recorded on different microphones (Rubio 2024: n=78, then n=44, all female).
- Dietrich 2019 supports within-speaker pitch deviation only as a measure of arousal, not of a between-member disposition.

The evidence also exposes errors in the lane's own records. These are listed under cross-component findings and include a sex-confound fix that is not on main.

**Online scan (2026-10-10, abstracts and snippets only).** These are UNVERIFIED leads (DE-062), not findings:

- No published EER for ECAPA-TDNN on parliamentary or broadcast political speech turned up.
- The size of cross-domain degradation is illustrated by the EDITnet paper: VoxCeleb-trained ECAPA reaches 17.78% EER on CN-Celeb, against 13.26% when trained in-domain.
- No study that validates LLM speaker attribution from YouTube auto-captions turned up. The Public Discourse Corpus paper (arXiv 2609.20232) says the acoustic speaker-turn evidence is essential and that text-only attribution is unvalidated.
- No study that directly tests whether one person's disfluency rate stays stable across contexts turned up. Shriberg-line work shows disfluency rates swing with speech preparation and speaking context.

## Caption-based speaker verification

**What it computes.** Per clip: SOLO, MULTI or NOT_SPEAKING, and MATCH, MISMATCH or NONE identity, from auto-captions with Haiku 4.5. Keep = SOLO and not MISMATCH.

**Method.** Hard identity cues only. About 33.7% of keeps rest on weak evidence. 2,192 clips, 1,761 kept.

**Code refs.** `fec/members/b5_audio_*.py`, `fec/audio_rules.py:17-39`

**Assumptions.**
- Hard cues in captions identify the speaker

**Evidence.** 1 rows from 1 papers (1 VERIFIED-SOFTENED).

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | suggests-extension | The corpus is Clerk HouseLive video, audio and closed captions: 6,432 hours from 863 House debates, 6 Jan 2009 to 4 Aug 2014. Speeches are split on caption timestamps, giving 74,158 speeches of at least 50 words. The captioning vendor asserts about 95 percent word accuracy (footnote 11). The authors transcribed 100 random speeches and found the captions 'essentially mirrored' them. The paper does not say the chamber attributes each speech to a member. | Clerk speaker attribution is not established by the paper, so using it as an identity source needs a separate check of Clerk floor video metadata. If confirmed, House floor video from 2009 onward could replace guessed identity on floor clips. It does not reach interview, Sunday-show, Senate or hearing audio, where the identity gap is largest. | moderate | VERIFIED-SOFTENED |


**Assessment.**

**Evidence weight: effectively none for the component's actual claim.** The claim is that hard identity cues in auto-captions, read by Haiku 4.5, identify the speaker well enough to keep a clip. The keep rule is SOLO and not MISMATCH; 1,761 of 2,192 clips are kept, and about 33.7% of keeps rest on weak evidence.

The only bearing is Dietrich 2019 (VERIFIED-SOFTENED, suggests-extension). Clerk HouseLive floor captions are about 95% word-accurate by vendor assertion, and an author check of 100 random speeches found they 'essentially mirrored' a hand transcription. That speaks to caption *word* accuracy on House floor video from 2009 to 2014. It does not speak to *speaker* identity. The paper never says the Clerk attributes each speech to a member, so even the floor-video route needs a separate metadata check. It does not reach interviews, Sunday shows, Senate audio or hearings, which is where the identity gap is largest.

**Online scan (UNVERIFIED leads).**
- Auto-captions carry no speaker tags (arXiv 2606.26062). That paper uses LLM guest-or-interviewer labelling, as Melange does, but reports no accuracy for it.
- The Public Discourse Corpus paper (arXiv 2609.20232) says that acoustic speaker-turn evidence is essential. Its one attribution check was 60 utterances at 91.7%, on a commercial diarization pipeline rather than a text-only one.

No published false-accept rate for text-only, caption-based identity was found. The leads point the same way as the closed acoustic attempt: text-only identity is the weakest link, and it has never been measured in this domain.

**Contradictions.**
- The keep label is NONE in the component spec (keep = SOLO and not MISMATCH) but MATCH or NEUTRAL in the Dietrich row's system_side. The two vocabularies need reconciling: decide whether a clip with no identifying content counts as a pass.
- The brain's 33.7% weak-evidence share and the 38% contamination figure from before 2026-07-09 are brain-sourced and were not re-checked in any evidence row. Do not cite them as audited figures.

**Gaps.**
- The clip-level false-accept rate is unknown. The study that fills it: hand-check a stratified random sample of kept clips. Stratify by format (floor, hearing, interview, Sunday show, radio) and by weak versus hard cue. Report the false-accept rate with Wilson intervals per stratum and name the denominator, for example kept clips in a given month window. About 150 or more clips would bound a 10% rate to roughly +/-5 points.
- Clerk HouseLive speaker metadata has not been checked. The study: a direct audit of whether Clerk floor video carries per-speech member attribution. If it does, floor clips get an identity source independent of captions.
- No benchmark exists for LLM speaker attribution from captions in political video. The scan found none, so the hand-checked sample above would itself be the first one.

**Paul's position vs the evidence.**

Paul says the format needs a framework for its 'contextual or auditory constraints', citing Ramey et al. No evidence row tests Ramey on this point. Ramey is a lexical-personality source, so it does not support a caption identity method. The available evidence (Dietrich's caption-accuracy check, and the scan leads that call acoustic turn evidence essential) is consistent with Paul's instinct that captions alone are not a sufficient identity framework, but no study measures it. The position stands as a design concern and has not been tested.

**Open questions.**
- What is the false-accept rate of caption-cue verification on a hand-checked sample, per format?
- Does Clerk HouseLive floor video carry per-speech member attribution that could replace guessed identity on floor clips?
- Should weak-evidence keeps (the brain's 33.7% share) be dropped or flagged until the audit is done? This is Paul's ruling.

**Read first.** [[dietrich-2019-pitch-perfect-vocal]], [[mestre-2026-potential-pitfalls-audio]]

## eGeMAPS prosody and Whisper disfluency rates

**What it computes.** 88 eGeMAPS features per clip; member descriptors (pause, pace, volume, strain, pitch); hesitation rate per 1,000 words via prompted faster-whisper.

**Method.** ICC and Spearman-Brown reliability: pause .92 > pace .86 > volume .85. Venue confound: live about 26 per 1k vs NPR 0.22 per 1k. The audio-prosody N pilot MISSed at N=90.

**Code refs.** `fec/members/b5_audio_build.py`, `fec/members/b5_speech_quirks.py`, `fec/members/b5_audio_align_turns.py:249`

**Assumptions.**
- Vocal features carry stable between-member signal
- Sex confounds pitch variability (r = -.266)

**Evidence.** 13 rows from 7 papers (1 VERIFIED; 12 VERIFIED-SOFTENED).

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[marrero-2022-evaluating-voice-samples]] | 2022 | method-source | Averaged over Big Five traits (N=614 students, 100-fold CV, out-of-sample R-squared): vocal features 6.24 percent, LIWC 3.13 percent, joint set 10.67 percent. Joint set described as exceeding the sum of the two separate sets. E, C and N reach the 10-20 percent range with the joint set. | Paper criterion is self-report BFI-2 from 18-year-old students, one prompted smartphone sample per person, out-of-sample R-squared. System criterion is Pink (reliability about 0.67, text-derived, shares source with the linguistic block) or behavioral visibility and diligence; audio is edited broadcast and floor speech from 90 legislators and the metric is held-out r. Numbers are not directly comparable. Super-additivity failed its registered bars in three looks; the +0.119 context result is exploratory and not a registered test. | strong | VERIFIED-SOFTENED |
| [[marrero-2022-evaluating-voice-samples]] | 2022 | supports | Vocal features predicted gender with over 90 percent of variance explained (N=564; 374 female, 190 male). Gender was one of two tasks where the joint set was not best (the other was Agreeableness). The paper did not use predicted gender as a feature or control when predicting personality. | The paper shows gender is recoverable from its own vocal features in 564 students. It does not show this holds in other corpora, so the claim that sex is near-deterministic in any adult speech corpus is not supported by this paper and should not be cited to it. The sex-conditioning lesson applies in general terms; the r -0.266 figure is the system's own evidence. | moderate | VERIFIED-SOFTENED |
| [[celli-nd-twenty-years-personality]] |  | suggests-extension | Biel et al. 2012 identified Extraversion and Emotional Stability as the traits with the largest and fewest informative audio-visual cues. OpenSMILE is a common tool for prosodic and technical audio features. | Biel's cue counts come from vlog and video impressions, not congressional audio. The extension to prioritize Extraversion for audio is an inference: the paper also reports prosodic SVMs performing best for Extraversion and Conscientiousness. The N pilot MISS at N=90 was not checked, and the survey gives no estimate for edited broadcast audio. | weak | VERIFIED-SOFTENED |
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | method-source | Intensity is each speech's mean F0 z-scored against the speaker's own baseline, in SD units. Table 1 pitch models (N1 = 74,151 speeches, N2 = 619 members, 2009-2014 House floor): female x 'women' mentioned = 0.090 (SE 0.027) without controls and 0.112 (SE 0.027) with controls. About 120 speeches per member is only the average (74,151 / 619); the paper states no required sample size. The authors call the raw magnitude 'slight enough'. | The within-speaker z transform transfers. The inference does not: the paper identifies effects from a cross-speaker contrast over 74,151 speeches, while the system computes one member's topic mean from 2-5 units. Assuming independent units, the SE of a 2-5 unit topic mean is about 0.45-0.7 SD (arithmetic; no minimum detectable effect was computed). The paper's effects are 0.09-0.38 SD. | strong | VERIFIED-SOFTENED |
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | qualifies | Pitch measures arousal, not valence (footnote 1, p. 942). The paper adds controls for speech duration, a one-minute dummy and an election-year dummy (p. 946, Table 1). It acknowledges that question intonation also moves mean F0 (footnote 8, p. 945) but does not control for it, arguing mean pitch remains reliable. The baseline is one venue: the House floor via HouseLive. | Applies wherever the system compares a member's pitch across topics. The paper's controls cover duration and one-minute speeches but not question intonation, so the system's gap is wider than the analyst stated. Program or channel offsets can exceed the paper's 0.1 SD effects, so a topic concentrated on one program picks up that offset. 'Relative emotional intensity' must not be read as anger or enthusiasm. | moderate | VERIFIED-SOFTENED |
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | supports | Raw F0 differs mainly by sex: Table 2 gives female means of about 203-206 Hz and male means of about 151 Hz. Deviation from each speaker's own baseline removes stable between-speaker level by construction. The authors read the measure as an emotional disposition toward an issue, and say the slight changes are consistent with pitch indicating a lawmaker's emotional disposition toward an issue. | Excluding absolute F0 from personality scores is directly supported by Table 2. The paper gives no evidence that pitch measures a between-member disposition such as N, so the closed audio-N route is consistent with it. The paper's own gloss is a topic-specific emotional disposition, which the rel calls a state; it is an interpretation the authors do not test. | moderate | VERIFIED-SOFTENED |
| [[mestre-2026-potential-pitfalls-audio]] | 2026 | supports | Three of the top five candidates by average pitch are women (Palin, Harris, Clinton; 34 candidates, four women; Section 4.2 p.319). Authors suggest within-speaker deviation from each speaker's own baseline (Figure 3c, 5-s windows, p.319) or speaker-dependent human validation (p.325). The paper reports no effect size for the gender pattern and does not say pitch comparisons reflect speaker and voice. | Carrying the direction to congressional speech rests on analyst reasoning (F0 sex differences are physiological), not paper text, and is UNVERIFIED. The paper's cross-speaker caution (p.319) says comparisons are confounded by recording factors, so the speaker-and-voice wording is not the paper's. | moderate | VERIFIED-SOFTENED |
| [[mestre-2026-potential-pitfalls-audio]] | 2026 | qualifies | Energy is normalized by each debate's average loudness (Section 4.2, p.319). Comparisons across speakers and time are subject to confounding by microphone placement, compression and sampling (p.319). The paper recommends normalization or comparison with baselines where possible and does not say normalization leaves residual confounding. Naive cross-speaker interpretation is cautioned (p.325). The paper's own Figure 3a ranks candidates by raw cross-speaker average pitch. | Applies to cross-member readings of volume dynamics, pitch level and strain. Within-member and ICC uses are less exposed. Whether pause and pace are less channel-sensitive is not tested in the paper and remains UNVERIFIED. | moderate | VERIFIED-SOFTENED |
| [[salam-2022-learning-personalised-models]] | 2022 | qualifies | Gender-split fusion of the two best modalities gives AMSE 0.690 against 0.818 for generic bimodal fusion, a 0.128 reduction (abstract and Table 1). Age-split fusion gives 0.796. One challenge test split; no test N, subgroup sizes, intervals or seed variance reported. | The paper has no pitch or jitter result; its audio features are MFCC band statistics. The sex-conditioning lesson transfers only as a diagnostic for features whose signal partly is sex (pitch, jitter), with the denominator (1,324 clips / 309 members) named and strata or an UNSTRATIFIED label. It is not a basis for per-sex trait models: the paper's criterion is self-report in a volunteer sample of 147 dyadic participants, while Melange gold is expert and insider ratings. | weak | VERIFIED-SOFTENED |
| [[rubio-2024-feasibility-data-analytics]] | 2024 | supports | Single-feature voice-trait correlations of about 0.30 to 0.48 in Sample 1 (n=78) did not replicate in Sample 2 (n=44, self-report only): the top features differed, and the authors say the results did not coincide. They note two different microphones may have contributed to this, alongside noise, colds and fatigue. Voice alone reached about 60 percent best-case three-class accuracy against over 75 percent for facial-expression work, and they call personality prediction multimodal. | The paper gives no effect-size estimate for channel instability. Its own wording is that microphones may have contributed, alongside other factors, so the defensible claim is that feature-level findings did not replicate across two samples recorded on different microphones. The claim that a channel change can wipe out findings overstates the paper. Cite the venue numbers from the brain ladder with their source, since the brain also gives a 21.8 per 1k live median. | moderate | VERIFIED-SOFTENED |
| [[rubio-2024-feasibility-data-analytics]] | 2024 | qualifies | Top-five-feature random forest, leave-one-out on N=100, three classes at training 25th and 75th percentiles. All-cases accuracy: self-report N 50, E 46, O 46, A 53, C 43 percent; expert N 57, E 60, O 57, A 56, C 53; close others N 54, E 57, O 45, A 52, C 54. Chance is 33 percent; a constant central-class predictor gets 50 percent (p.14). The paper does not say whether feature selection ran inside each fold, and reports no multiple-comparison correction across 1941 features. | Rubio accuracies are not a bar or prior for Melange looks. Self-report cells mostly sit at or below the 50 percent central-class baseline, but Agreeableness at 53 is above it, so do not say none exceed it. The Melange N look used n=110 (brain: r -0.0016, perm-p .511), not n=90. Do not present the superseded +0.208 as current evidence. | moderate | VERIFIED-SOFTENED |
| [[rubio-2024-feasibility-data-analytics]] | 2024 | qualifies | All 100 participants are female: Sample 1 is 78 psychology students aged 18 to 27, Sample 2 is 44 participants aged 18 to 54. The authors list this as a limit on generalization. The paper does not estimate how sex affects F0-based features. | A single-sex sample says nothing about whether pitch effects survive in a mixed-sex legislature. The transfer is the practical rule: any pitch descriptor in Melange needs sex conditioning or the de-normalised SD. The fix is not on main, so the live pipeline still carries the sex loading. Status stays UNVERIFIED under DE-062 until a second path reproduces it. | strong | VERIFIED |
| [[mairesse-2007-automatic-recognition-personality]] | 2007 | supports | Prosody-only RankBoost gives extraversion ranking loss 0.26 (random 0.50), the best single feature set in ranking. Prosody-only Naive Bayes gives 67.56 percent for E, but LIWC alone (68.89) and MRC alone (68.78) are higher. Self-assessed neurotics show low and constant voice intensity (Int-mean, Int-stddev) that observers do not use. | The 0.26 and 67.56 figures and the self-report intensity result are verified. The strongest-single-feature-set claim holds only for ranking; in classification LIWC and MRC are higher. EAR observers rated audio, so prosody gains are partly shared-channel, as the paper does not test. The sign correction is supported only for self-reported neuroticism, which the rel states. | moderate | VERIFIED-SOFTENED |


**Assessment.**

**Evidence weight: substantial, and mostly against any trait use. This supports display-only.**

- **Marrero 2022 (VERIFIED-SOFTENED, strong method source).** In 614 students with self-report BFI-2 and one smartphone sample each, the joint vocal and LIWC set reached an out-of-sample R-squared of 10.67%, averaged across traits. The Melange replication (N=90 legislators, eGeMAPS-88 ridge, Pink criterion; UNSTRATIFIED by sex) found:
  - held-out r +0.284 for Pink-E and +0.298 for Pink-ES;
  - A and C null;
  - the super-additivity bar missed in all three registered looks (vocal adds +0.001 for E; C -0.046; vocal to diligence r -0.081).
  
  The criteria are not comparable. Pink is text-derived, with reliability around 0.67, and shares a source with the lexical block.
- **Dietrich 2019 (strong method source plus qualifiers).** The within-speaker z of mean F0 transfers as a measure of arousal, not valence. The inference does not transfer. The paper finds effects of 0.09 to 0.38 SD over 74,151 House floor speeches from 619 members. A 2-to-5-unit topic mean has an SE of roughly 0.45 to 0.7 SD (arithmetic, assuming independent units), so per-member topic intensity cannot detect effects that small. The system also lacks the paper's duration and one-minute controls, and neither the paper nor the system controls for question intonation.
- **Rubio 2024 (the one VERIFIED row, plus two softened rows).** Voice-trait correlations of about 0.30 to 0.48 in n=78 did not replicate in n=44 recorded on a different microphone. The sample is all female. Self-report accuracies sit near the 50% central-class baseline.
- **Mestre 2026.** Cross-speaker comparisons are confounded by microphone placement, compression and sampling. b5_audio_build.py z-scores over pooled members with no channel or loudness normalization, so cross-member volume, pitch-level and strain readings are exposed. Within-member and ICC uses are less exposed.
- **Salam 2022 and the sex rows.** Sex conditioning transfers only as a diagnostic for pitch and jitter. The basis is the system's own r(sex=F) = -0.266 for stddevNorm pitch variability, over 1,324 clips and 309 members. Pace and volume dynamics are sex-clean (r +0.06 and -0.05).
- **Mairesse 2007.** Prosody is the best single set only for E *ranking* (loss 0.26). In classification, LIWC and MRC beat it.

The reliability figures in the spec (ICC: pause .92, pace .86, volume .85) show that members differ stably. Stable between-member variance is not trait validity, and nothing in the evidence links these descriptors to an external criterion.

**Disfluency.** No evidence row covers the Whisper hesitation rate. The scan (UNVERIFIED leads: Shriberg, Kosmala and Crible on prepared versus spontaneous speech, arXiv 2607.05964 on filled pauses in four Slavic parliaments) shows that disfluency rate moves strongly with how prepared the speech is and with speaking context. That fits the venue ladder, with live Washington Journal at about 26 per 1k words against NPR at 0.22 per 1k, an edited channel. No study tests whether one speaker's rate holds across contexts.

**Contradictions.**
- **Actual error: the sex fix is not on main.** The Marrero row says commit 4193f0140 replaced pitch_variability_raw with plain semitone SD. The Salam and Rubio rows verified with git merge-base that the commit is NOT an ancestor of main HEAD; it sits on branch claude/melange-speech-recognition-cfa049. On main, the live candidate still emits stddevNorm alone (PITCH_VAR, lines 27 and 217), so the sex loading of r -0.266 over 1,324 clips and 309 members is still live.
- **Actual error: normalization is described inconsistently.** The Mestre row reads b5_audio_build.py lines 85-104 as a pooled z across members with no within-source normalization, and notes that the cache comment on line 104, 'z-within-source', contradicts the code. The Rubio row describes the same pipeline as 'z-within-source... (verified in code)'. Both cannot be true. Re-read the live file and correct whichever note is wrong.
- **Actual error: the N-look sample size.** The component spec and position say the audio-N pilot MISSed at N=90. The Rubio row records the spent N look at n=110 (r -0.0016, perm-p .511). The Marrero-style faithful prereg was N=90, so the two are probably being conflated.
- Marrero round 2 has an unregistered context result (+0.119 for E against visibility) that is labelled a win. It is exploratory and must not be presented as a pass.
- The brain's +0.208 held-out r was superseded the same day as gender-confounded (locked robustness look 34bf7e4). Do not present it as current evidence.
- b5_audio_eval.py's ES_acoustic composite is mis-signed (2026-07-09 note). Mairesse supports a sign correction only for self-reported neuroticism (low, constant voice intensity), not for observer ratings.
- The status of the Rubio sex row is inconsistent. It is marked VERIFIED, but its transfer text says it 'stays UNVERIFIED under DE-062 until a second path reproduces it'. Read the row as: the paper's facts are verified; the claim that the pipeline still carries the loading is a single-path finding.

**Gaps.**
- No study shows that disfluency rates are stable within a member across venues once editing is controlled. The study: within-member comparison of unedited live sources only (live floor, live call-in), with venue fixed effects, reporting within-member ICC across at least 2 unedited venues per member. Edited broadcast such as NPR should be excluded as a measure of the speaker.
- Prosody has not been tested for trait information beyond text against a criterion independent of text. The study: a registered look against expert or insider gold (not Pink, which shares a source with text), stratified by sex. Report vocal-only and incremental validity with intervals.
- No channel-robustness estimate exists. The study: the same members recorded across channels (studio, remote, telephone), estimating channel offsets on each eGeMAPS descriptor. Mestre's per-debate loudness normalization is a starting template.
- No minimum-detectable-effect calculation exists for the topic-intensity descriptor. Arithmetic on its 2-to-5-unit floors suggests it cannot detect Dietrich-sized effects.

**Paul's position vs the evidence.**

Paul is interested in non-textual channels, and the system concluded audio is display-only. The evidence clearly supports that conclusion:
- vocal features failed super-additivity in 3 registered looks;
- the Rubio sample did not replicate across microphones;
- the Mairesse prosody gain is ranking-only and partly shared-channel;
- Dietrich gives no basis for a between-member disposition.

Two cautions apply even to display use:
1. On main, the pitch-variability descriptor still carries the sex loading, because the fix sits on an unmerged branch.
2. Cross-member readings of volume, pitch level and strain have no channel normalization.

Paul's citations here (Ramey, Joo & Steinert-Threlkeld, and Dietrich) are mostly not audio-trait evidence. Only Dietrich bears directly, and it bears on topic arousal, not traits.

**Open questions.**
- Should 4193f0140 (plain semitone SD) be merged to main, or should the pitch-variability descriptor be withheld from display until it is? This is Paul's call.
- Which normalization is actually live in b5_audio_build.py, pooled across members or within source?
- Was the spent audio-N look n=90 or n=110? Correct the component spec to match RESULT 8bf113948.
- Should topic_emotional_intensity stay CANDIDATE_CONSUMED_BY_NOTHING, given that its unit floors cannot resolve effects of 0.1 SD?
- Are pause and pace less channel-sensitive than volume and pitch? No source tests it.

**Read first.** [[marrero-2022-evaluating-voice-samples]], [[dietrich-2019-pitch-perfect-vocal]], [[rubio-2024-feasibility-data-analytics]], [[mestre-2026-potential-pitfalls-audio]], [[salam-2022-learning-personalised-models]], [[mairesse-2007-automatic-recognition-personality]]

## Acoustic speaker-ID (closed) and two-check identity design (proposed)

**What it computes.** Closed: cosine and neural speaker ID (EER .226-.317, failed). Proposed: forced alignment AND ECAPA-TDNN verification, fail-closed, with witness-turn negative controls.

**Method.** Pilot bars: EER <= 5%. Awaits rulings D1-D12. Production is blocked at 0 profiles.

**Code refs.** `b5_audio_embed/enroll/verify2 (worktree branch)`, `DRAFT big5_audio_identity_prereg.md`

**Assumptions.**
- Speaker embeddings separate members at EER <= 5%

**Evidence.** 3 rows from 1 papers (3 VERIFIED-SOFTENED).

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[mestre-2026-potential-pitfalls-audio]] | 2026 | method-source | Section 4.3 and footnote 9 (p.321): one neural layer over 768-dimensional facebook/wav2vec2-base embeddings for each sentence (no pooling method stated), Adam with learning rate 0.001, cross-entropy loss, batch size 8 for wav2vec2, early stopping with patience 3. Section 4.1 (p.318): aeneas MFCC/dynamic-time-warp forced alignment. The MFCC CNN baseline is far weaker in the paper's speaker-ID results. | The recipe transfers as a method. The paper gives no split protocol and no pooling detail, so mean pooling and max_epochs 200 with seed 0 in HYPERPARAMETERS are Melange choices. Paul position-table claim of No annotated literature was not located in Project Brain or the vault and stays UNVERIFIED. | strong | VERIFIED-SOFTENED |
| [[mestre-2026-potential-pitfalls-audio]] | 2026 | qualifies | Test accuracy across all 156 speakers 0.913568 (0.914) and across 34 candidates 0.945769 (0.946); MFCC CNN 0.081 and 0.134. Per-debate wav2vec2 0.974 (+/-0.024) vs MFCC 0.888 (+/-0.085). Section 4.2 (p.319) warns that cross-speaker acoustic comparisons are confounded by microphone placement, compression and sampling. The paper does not describe its train/test split. | The paper's 0.914 sets no expected level for congressional audio. The paper's confound warning concerns cross-speaker acoustic comparisons, not classifier accuracy, so the channel-leakage explanation for the EER gap is analyst hypothesis, UNVERIFIED (DE-062). Pilot wording is source-disjoint, not event-disjoint. | moderate | VERIFIED-SOFTENED |
| [[mestre-2026-potential-pitfalls-audio]] | 2026 | supports | Aeneas alignment validated on 500 utterances x 5 crowd ratings: PA 85.26% and DA 95.76% (Section 4.1, p.318). Only 5% of pairs rated not aligned at all, concentrated in short or interrupted sentences (Figure 2b). The paper proposes no word floor and no ASR word-match threshold. | The paper supports only that short sentences align poorly. It does not support a 50-word floor or an ASR word_match bar. Vault note concepts/single-look-holdout-discipline-cases-2-3-n-slot-and-gold-template.md line 60 lists the 50-word floor as a paper method, which is wrong. The claim that debate transcripts are cleaner than hearings is analyst reasoning, UNVERIFIED. | moderate | VERIFIED-SOFTENED |


**Assessment.**

**Evidence weight: one method source, no benchmark.** The proposed design is forced alignment AND ECAPA-TDNN verification, fail-closed, with witness-turn negative controls. Its pilot bar is EER <= 5%. Production is blocked at 0 profiles, pending rulings D1-D12.

Mestre 2026 (VERIFIED-SOFTENED):
- **Method.** The wav2vec2 one-layer head recipe (batch 8, patience 3, Adam 0.001) and aeneas forced alignment transfer as methods. On the paper's own debate audio, wav2vec2 reached 0.914 accuracy across 156 speakers, against 0.081 for the MFCC CNN.
- **What it does not license.** The paper gives no train/test split protocol, so 0.914 sets no expectation for congressional audio. It does not support the system's 50-word floor or its 0.80 and 0.70 word-match bars. It supports only that short or interrupted sentences align poorly; 5% of 500 crowd-rated pairs were rated not aligned at all.
- **The closed attempt.** It failed by a wide margin: calibration EER .226 on a fresh 21-member, source-disjoint cohort, and .317 for an earlier 30-member head. Blaming channel leakage for the gap is analyst hypothesis, UNVERIFIED (DE-062).

**Online scan (UNVERIFIED leads).**
- No published ECAPA-TDNN EER on parliamentary or broadcast political speech was found.
- Domain mismatch alone moves ECAPA EER a long way. EDITnet (arXiv 2206.07548) reports about 1.1% EER on VoxCeleb1-O, 17.78% when a VoxCeleb-trained model is applied to CN-Celeb, and 13.26% when trained in-domain.
- Stock ECAPA is trained on VoxCeleb, Fisher, Switchboard and LibriSpeech, with no political or broadcast-hearing audio. It is known to be weak on short utterances and overlapping speech, both of which are common in hearings.
- Speaker-labelled parliamentary corpora exist that could serve as in-domain dev sets: FalAR (Portuguese, 1,180 speakers), Nos_ParlaSpeech-GL and Europarl-ASR. None of them report EER.

On this evidence, EER <= 5% with 3 enrollment clips is an untested and ambitious bar. In-domain adaptation is the documented lever.

**Contradictions.**
- **Actual error: the 50-word floor is misattributed.** The vault note concepts/single-look-holdout-discipline-cases-2-3-n-slot-and-gold-template.md line 60 lists the 50-word floor as a Mestre paper method. The paper proposes no word floor and no ASR word-match threshold, so the 50 words and the 0.80 and 0.70 bars are Melange choices with no calibration artifact in the repo.
- **Actual error: mean pooling is misattributed.** The b5_audio_neural_head.py docstring says the paper mean-pools wav2vec2 embeddings. The paper does not state a pooling method, so mean pooling, max_epochs 200 and seed 0 are Melange choices.
- The position table's 'No annotated literature' conflicts with citations_in_system, which already lists Mestre & Ryan (2026) and Dietrich (2019) for this design. The position is stale.
- The pilot is described as source-disjoint, not event-disjoint. If the same hearing event appears across sources, the EER could be optimistic; the .226 failure is not affected.

**Gaps.**
- No in-domain EER benchmark exists for congressional audio. The study: event-disjoint enrollment and test on hearing and floor audio, with identity labels independent of captions (Clerk metadata or hand-verified clips). Report the EER for stock ECAPA, then for in-domain-adapted ECAPA, with 3 enrollment clips, and report it separately for short utterances and overlapping speech.
- The alignment gates are uncalibrated. The study: code a sample of aligned turns by hand (the 150-clip coding packet is referenced but was not located) and set the 0.80 and 0.70 word-match bars from the observed precision of correct alignment.
- The witness-turn negative-control false-accept rate is unmeasured. It needs to be computed on the same held-out set.

**Paul's position vs the evidence.**

Paul's recorded position is 'No annotated literature', but the system already cites Mestre 2026 and Dietrich 2019 for this design, and Mestre's evidence rows are verified. Evidence does exist, but it is method-only. It supports the fail-closed, two-check architecture in spirit: alignment is unreliable on short turns, and neural embeddings beat MFCC. It sets no expected EER for Congress. The closed attempt's .226 and .317 EERs, together with the domain-mismatch leads, suggest that the <= 5% bar will likely need in-domain adaptation. Without it, the pilot may simply fail its bar.

**Open questions.**
- Can ECAPA-TDNN reach EER <= 5% on event-disjoint congressional hearing audio with 3 enrollment clips, and with in-domain adaptation if not?
- Where do the 0.80 and 0.70 word-match bars come from, and where is the 150-clip coding packet?
- Should the vault note claiming Mestre proposes a 50-word floor be corrected?
- Is the pilot split event-disjoint as well as source-disjoint?

**Read first.** [[mestre-2026-potential-pitfalls-audio]], [[dietrich-2019-pitch-perfect-vocal]]

## Across components

**Identity gates everything downstream.** Every prosody and disfluency descriptor is computed only on clips that pass caption-based verification, and that step has no measured false-accept rate. The two-check acoustic design was meant to replace it, but it is blocked: the closed attempt reached EER .226 to .317 against a bar of <= 5%. Until one identity route is measured, every descriptor carries an unknown amount of wrong-speaker contamination. The brain recorded 38% contamination before 2026-07-09, but that figure was not re-checked.

**The same confound runs through all three components.** Mestre 2026 and Rubio 2024 both point to it: cross-speaker acoustic comparisons are confounded by microphone, compression and sampling. Channel (studio, remote, telephone) is recorded as uncontrolled, and the venue ladder (live WJ about 26 per 1k words, NPR 0.22 per 1k) shows that editing and channel dominate the disfluency signal. It undermines:
- cross-member prosody descriptors;
- disfluency rates;
- acoustic speaker embeddings, where the EER gap is plausibly channel-driven, though that explanation is UNVERIFIED.

**Sex is a second confound that crosses components.** It loads on pitch variability and jitter: r(sex=F) = -0.266 for stddevNorm over 1,324 clips and 309 members. Every pooled audio-to-trait coefficient (Pink-E +0.284, Pink-ES +0.298, N=90) should carry an UNSTRATIFIED label unless re-run with sex strata.

**Actual errors to correct:**
1. Commit 4193f0140, the semitone-SD fix, is not on main, so the live pipeline still emits the sex-loaded descriptor.
2. b5_audio_build.py is described as both pooled-z and z-within-source in different notes.
3. The audio-N look is recorded as N=90 in the spec but n=110 in the result.
4. The vault note attributes a 50-word floor to Mestre, and the neural-head docstring attributes mean pooling to Mestre. Neither appears in the paper.
5. The identity position says 'No annotated literature' while the system already cites Mestre and Dietrich.
6. The Ramey year conflict (2016 vs 2019) is still open in citations_in_system and needs one value.

No paper in this lane argues for either rejected SNA proposal, and the lane has no rank or engine consumer, so D-021, D-025 and D-104..D-108 are not engaged.

## Research agenda

- Run a hand-checked false-accept audit of caption-based speaker verification. Use a stratified random sample of kept clips (about 150 or more), stratified by format and by weak versus hard cue. Report the rate per stratum with Wilson intervals and a named denominator and window.
- Audit Clerk HouseLive floor-video metadata for per-speech member attribution. If it exists, use it as a caption-independent identity source for House floor clips (2009 onward).
- Paul's ruling: merge commit 4193f0140 (plain semitone SD) to main, or withhold pitch_variability_raw from display until it is merged.
- Re-read b5_audio_build.py lines 85-104 to settle pooled-z versus z-within-source, and correct the wrong note. Correct the audio-N look sample size (N=90 vs n=110) against RESULT 8bf113948.
- Correct the vault note concepts/single-look-holdout-discipline-cases-2-3-n-slot-and-gold-template.md line 60 (the 50-word floor is not from Mestre) and the b5_audio_neural_head.py docstring (Mestre does not state mean pooling). Paul decides whether to apply these.
- Before the ECAPA pilot, define an event-disjoint split with identity labels that do not come from captions. Report stock and in-domain-adapted ECAPA EER, with breakouts for short utterances and overlapping speech. Use FalAR, Nos_ParlaSpeech-GL or Europarl-ASR as an out-of-domain parliamentary dev set if useful.
- Run a disfluency stability study within members on unedited live venues only, with venue fixed effects and within-member ICC across at least 2 venues. Exclude edited broadcast.
- Run a channel-offset study: the same members across studio, remote and telephone channels, per eGeMAPS descriptor, using Mestre's per-debate loudness normalization as a template.
- Compute a minimum detectable effect for topic_emotional_intensity at its 2-to-5-unit floors against Dietrich's 0.09-0.38 SD effects. If it cannot resolve them, keep the descriptor unconsumed.
- Online-scan leads to fetch and verify (UNVERIFIED, abstracts only): Public Discourse Corpus arXiv 2609.20232 (https://arxiv.org/pdf/2609.20232); keyword lexicon blindness and LLM speaker filtering, arXiv 2606.26062 (https://arxiv.org/pdf/2606.26062); EDITnet domain adaptation, arXiv 2206.07548 (https://arxiv.org/pdf/2206.07548); Dawalatabad et al. ECAPA-TDNN diarization, arXiv 2104.01466 (https://arxiv.org/pdf/2104.01466); ECAPA2, arXiv 2401.08342 (https://arxiv.org/pdf/2401.08342); FalAR parliamentary speaker corpus (https://huggingface.co/papers/2605.27062); filled pauses in Slavic parliaments, arXiv 2607.05964 (https://arxiv.org/pdf/2607.05964); Kosmala & Crible on filled pauses in prepared vs spontaneous French (https://halshs.archives-ouvertes.fr/halshs-03225622/file/Kosmala-Crible-20-0166.R1_Proof_hi.pdf); Shriberg on the distribution of disfluencies (https://upenn-test.atmire.com/entities/publication/890d6f9a-2a68-4a9e-9647-1d4185f91f52).
