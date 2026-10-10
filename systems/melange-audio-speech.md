---
type: system
name: "Melange audio and speech lane"
slug: melange-audio-speech
components: [melange-audio-speech.speaker-verification, melange-audio-speech.prosody-and-disfluency, melange-audio-speech.identity-two-check-design]
brain_notes: [actors/melange-audio-pipeline, topics/2026-07-feature-audio-and-speech-lane-n-audio-route-and-measurements]
paper_count: 6
tags: [system]
---

# Melange audio and speech lane

Speaker verification from captions, eGeMAPS prosody, Whisper disfluency rates, the closed acoustic speaker-ID attempt, the proposed two-check identity design, and the display-only audio descriptor layers. No rank or engine consumer.

Brain notes: [actors/melange-audio-pipeline](obsidian://open?vault=brain&file=actors%2Fmelange-audio-pipeline) · [topics/2026-07-feature-audio-and-speech-lane-n-audio-route-and-measurements](obsidian://open?vault=brain&file=topics%2F2026-07-feature-audio-and-speech-lane-n-audio-route-and-measurements)

## Melange audio and speech lane: evidence assessment

**Scope.** This system covers caption-based speaker verification, eGeMAPS prosody and Whisper disfluency descriptors, the closed acoustic speaker-ID attempt and the proposed two-check identity design. It has **no rank or engine consumer**, and every audio output is display-only or a research candidate.

**Evidence base.** There are 16 bearing rows from 6 papers: Marrero 2022, Celli (n.d.), Dietrich 2019, Mestre 2026, Salam 2022 and Rubio 2024. One row is VERIFIED (Rubio, on the sex-loading fix not being on main). The other 15 are VERIFIED-SOFTENED, which means the paper claim checks out but the transfer to Melange is narrower than first drafted. No row is fully UNVERIFIED, but several rows contain UNVERIFIED sub-claims (DE-062 leads), and those are flagged below.

**Coverage per component:**
- **Prosody and disfluency** has 12 rows, nearly all on prosody or pitch. None covers Whisper disfluency measurement.
- **Two-check identity design** has 3 rows, all from Mestre 2026.
- **Caption-based speaker verification** has 1 row (Dietrich 2019, suggests-extension). In practice it is a GAP: no paper validates an LLM reading YouTube auto-captions as an identity check.

**Bottom line.** The literature supports the lane's current conservative posture:
- within-speaker pitch standardisation
- excluding absolute F0 and inferred sex from personality
- audio as display-only

It does not support using audio to score between-member traits. Three things in the system go beyond what any cited paper backs:
1. Per-topic "emotional intensity" is computed from 2 to 5 speech units. At that size the noise is several times larger than the published effects.
2. The cross-member z-scoring pools members with no loudness or channel normalisation.
3. The word-floor and word-match bars in the identity design have no calibration source.

**Live defect.** The sex-loaded `pitch_variability_raw` (r(sex=F) = -0.266 over 1,324 clips / 309 members, brain-sourced) is still on main. The semitone-SD fix, commit 4193f0140, sits on branch claude/melange-speech-recognition-cfa049 and is not an ancestor of HEAD. Two rows checked this with git merge-base.

## Caption-based speaker verification

**What it computes.** Per clip: SOLO, MULTI or NOT_SPEAKING, and MATCH, MISMATCH or NONE identity, from auto-captions with Haiku 4.5. Keep = SOLO and not MISMATCH.

**Method.** Hard identity cues only. About 33.7% of keeps rest on weak evidence. 2,192 clips, 1,761 kept.

**Code refs.** `fec/members/b5_audio_*.py`, `fec/audio_rules.py:17-39`

**Assumptions.**
- Hard cues in captions identify the speaker

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | suggests-extension | Corpus is Clerk HouseLive video, audio and closed captions: 6,432 hours, 863 House debates, Jan 2009 to Aug 2014; 74,158 speeches of 50+ words split on caption timestamps. Vendor asserts about 95% word accuracy; authors' check of 100 random speeches found captions essentially mirrored them. The paper does not say the chamber attributes each speech to a member. | Clerk speaker attribution is not established, so using it as an identity source needs a separate metadata check. If confirmed, House floor video from 2009 on could replace guessed identity on floor clips. It does not reach interview, Sunday-show, Senate or hearing audio, where the identity gap is largest. The brain's 33.7% no-identifying-content share and 38% pre-2026-07-09 contamination figure are unchecked. | moderate | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: thin. One VERIFIED-SOFTENED row of moderate strength, and it is an extension lead, not a validation.**

Dietrich 2019 used Clerk HouseLive video, audio and closed captions: 6,432 hours, 863 House debates, January 2009 to August 2014. Speeches were split on caption timestamps into 74,158 units of 50 words or more. The vendor claims about 95% word accuracy, and the authors' own check of 100 random speeches found the captions essentially mirrored them.

What this establishes is caption *word* accuracy on House floor video. It does **not** establish that the chamber attributes each speech to a member, so Clerk metadata as an identity source is still a lead that needs its own check. Even if that check passes, it only covers House floor clips from 2009 on. It does not reach interview, Sunday-show, Senate or hearing audio, which is where the identity gap is largest.

**What the system actually does.** Haiku 4.5 reads YouTube auto-captions and keeps a clip when it returns SOLO_TARGET with identity MATCH or NEUTRAL. No paper in the evidence set measures how accurate this kind of LLM-on-auto-caption speaker check is. Two brain figures are unchecked, so they are leads:
- 33.7% of kept clips carry no identifying content (the NEUTRAL share; denominator is all kept clips, window not stated).
- 38% contamination before 2026-07-09.

If the NEUTRAL share is anywhere near a third, a large part of the kept corpus rests on the absence of contrary evidence rather than on a positive identification.

**GAP: the study that would fill it.** A human-coded audit of kept clips:
- stratified by venue (floor, WJ, Sunday shows, NPR, hearings) and by MATCH vs NEUTRAL
- reporting precision of the kept set against human speaker identification
- with the window and clip denominators named

Add a recall check on rejected clips, and a separate check of whether Clerk floor-video metadata carries per-speech member attribution.

### Contradictions

- Paul's cited source for this component (Ramey et al., 'auditory transcripts') is used in the system only for the lexical Big Five pipeline. No evidence row ties Ramey to speaker attribution or to audio constraints, so the citation does not bear on the verification method.
- Dietrich 2019 is sometimes treated as an identity precedent, but the paper validates caption word accuracy, not speaker attribution.

### Gaps

- No study validates LLM (Haiku 4.5) speaker verification from YouTube auto-captions. Needed: a stratified human-coded precision/recall audit of SOLO_TARGET MATCH vs NEUTRAL clips per venue.
- No evidence on Clerk HouseLive per-speech member attribution. Needed: a metadata audit against the Congressional Record speaker tags.
- Off-floor venues (interviews, Sunday shows, Senate, hearings) have no identity-source literature at all.
- The 33.7% NEUTRAL share and the 38% pre-2026-07-09 contamination figure are brain-sourced and unchecked.

### Paul's position vs the evidence

Paul says a framework is needed for the format's 'contextual or auditory constraints'. The evidence agrees a framework is missing: nothing in the set validates the current caption-plus-LLM check. But his cited support (Ramey et al.) concerns lexical personality from transcripts and supplies no such framework. Dietrich 2019 is the only bearing paper. It offers a partial route for House floor clips only, and only if Clerk attribution is confirmed. So Paul's stated need stands. What it lacks is a source that answers it.

### Open questions

- Does Clerk HouseLive floor-video metadata attribute each caption segment to a member, and is that attribution independent of the caption text?
- What fraction of kept clips are NEUTRAL (no identifying content) per venue under the post-2026-07-09 rules, and what is their human-verified precision?
- Should NEUTRAL clips stay in the kept set at all for descriptor surfaces, or be shown with a separate identity-confidence label?
- Could a voice-predicted-sex vs recorded-member-sex mismatch serve as a cheap screen for misattribution? Marrero shows sex is recoverable from voice in its own sample. This is an untested idea, not a finding.

### Read first

1. [[dietrich-2019-pitch-perfect-vocal]]
2. [[mestre-2026-potential-pitfalls-audio]]
3. [[marrero-2022-evaluating-voice-samples]]

## eGeMAPS prosody and Whisper disfluency rates

**What it computes.** 88 eGeMAPS features per clip; member descriptors (pause, pace, volume, strain, pitch); hesitation rate per 1,000 words via prompted faster-whisper.

**Method.** ICC and Spearman-Brown reliability: pause .92 > pace .86 > volume .85. Venue confound: live about 26 per 1k vs NPR 0.22 per 1k. The audio-prosody N pilot MISSed at N=90.

**Code refs.** `fec/members/b5_audio_build.py`, `fec/members/b5_speech_quirks.py`, `fec/members/b5_audio_align_turns.py:249`

**Assumptions.**
- Vocal features carry stable between-member signal
- Sex confounds pitch variability (r = -.266)

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[marrero-2022-evaluating-voice-samples]] | 2022 | method-source | N=614 students, 100-fold CV, out-of-sample R-squared averaged over Big Five: vocal 6.24%, LIWC 3.13%, joint 10.67%, described as exceeding the sum. E, C, N reach 10-20% with the joint set. | Paper criterion is self-report BFI-2 from 18-year-old students, one prompted smartphone sample each; Melange uses Pink (reliability about 0.67, text-derived, shares source with the linguistic block) or behavioral criteria, edited broadcast audio, 90 legislators, held-out r. Not comparable. Super-additivity failed its registered bars in three looks; the +0.119 context result is exploratory. | strong | VERIFIED-SOFTENED |
| [[marrero-2022-evaluating-voice-samples]] | 2022 | supports | Vocal features predicted gender with over 90% of variance explained (N=564; 374 F, 190 M). Gender was not used as a feature or control for personality. | Gender is recoverable from voice in its own sample only; do not cite it for any adult speech corpus. The sex-conditioning lesson applies in general terms; r -0.266 is the system's own evidence. | moderate | VERIFIED-SOFTENED |
| [[celli-nd-twenty-years-personality]] | n.d. | suggests-extension | Biel et al. 2012: Extraversion and Emotional Stability have the largest and fewest informative audio-visual cues. OpenSMILE is a common prosody tool. | Biel's counts come from vlog impressions, not congressional audio. Prioritising E for audio is an inference (prosodic SVMs also did best for E and C). The N pilot MISS at N=90 was not checked; no estimate for edited broadcast audio. | weak | VERIFIED-SOFTENED |
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | method-source | Intensity = speech mean F0 z-scored against the speaker's own baseline. Table 1: N1 = 74,151 speeches, N2 = 619 members, 2009-2014 House floor; female x 'women' mentioned 0.090 (SE 0.027), 0.112 with controls. Authors call the raw magnitude 'slight enough'. | The within-speaker z transform transfers. The inference does not: the paper uses a cross-speaker contrast over 74,151 speeches; the system computes one member's topic mean from 2-5 units (SE about 0.45-0.7 SD by arithmetic, no MDE computed) against effects of 0.09-0.38 SD. | strong | VERIFIED-SOFTENED |
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | qualifies | Pitch measures arousal, not valence (fn 1, p. 942). Controls for duration, one-minute dummy, election year. Question intonation acknowledged but not controlled (fn 8, p. 945). One venue: House floor via HouseLive. | Applies wherever the system compares a member's pitch across topics; the system has no duration, question or speech-type term and pools channels. Program or channel offsets can exceed 0.1 SD. Must not be read as anger or enthusiasm. | moderate | VERIFIED-SOFTENED |
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | supports | Table 2: female raw F0 about 203-206 Hz vs male about 151 Hz. Deviation from own baseline removes stable between-speaker level by construction. | Excluding absolute F0 from personality is directly supported. No evidence pitch measures a between-member disposition such as N, consistent with the closed audio-N route. The 'emotional disposition' gloss is untested by the authors. | moderate | VERIFIED-SOFTENED |
| [[mestre-2026-potential-pitfalls-audio]] | 2026 | supports | Three of the top five candidates by average pitch are women (34 candidates, four women; s4.2 p.319). Authors suggest within-speaker deviation from own baseline or speaker-dependent validation (p.325). No effect size for the gender pattern. | Carrying the direction to congressional speech rests on analyst reasoning (physiology), not paper text, UNVERIFIED. The cross-speaker caution (p.319) says comparisons are confounded by recording factors. | moderate | VERIFIED-SOFTENED |
| [[mestre-2026-potential-pitfalls-audio]] | 2026 | qualifies | Energy normalized per debate (s4.2 p.319). Cross-speaker and time comparisons confounded by microphone placement, compression, sampling. Recommends normalization or baselines. Figure 3a itself ranks by raw cross-speaker pitch. | Applies to cross-member volume dynamics, pitch level and strain; `b5_audio_build.py` lines 85-104 z-score over pooled members with no loudness or channel normalization. Within-member and ICC uses are less exposed. Whether pause and pace are channel-robust is UNVERIFIED. | moderate | VERIFIED-SOFTENED |
| [[salam-2022-learning-personalised-models]] | 2022 | qualifies | Gender-split fusion gives AMSE 0.690 vs 0.818 for generic bimodal fusion (-0.128); age-split 0.796. One challenge split; no test N, intervals or seed variance. | Paper has no pitch or jitter result (MFCC band statistics). Transfers only as a sex-conditioning diagnostic for pitch and jitter (1,324 clips / 309 members, strata or UNSTRATIFIED label). Not a basis for per-sex trait models: self-report, 147 volunteers vs Melange expert/insider gold. | weak | VERIFIED-SOFTENED |
| [[rubio-2024-feasibility-data-analytics]] | 2024 | supports | Single-feature voice-trait correlations about 0.30-0.48 in Sample 1 (n=78) did not replicate in Sample 2 (n=44); two different microphones may have contributed, plus noise, colds, fatigue. Voice alone about 60% best-case three-class accuracy. | No effect-size estimate for channel instability. Defensible claim: feature-level findings did not replicate across two samples recorded on different microphones. Brain venue numbers (WJ about 26/1k, NPR 0.22/1k, live median 21.8/1k) must cite source. The N look was missed 2026-08-25. | moderate | VERIFIED-SOFTENED |
| [[rubio-2024-feasibility-data-analytics]] | 2024 | qualifies | Top-5-feature random forest, LOO on N=100, three classes. Accuracies: self-report N 50, E 46, O 46, A 53, C 43; expert 53-60; close others 45-57. Constant central-class predictor gets 50% (p.14). No multiple-comparison correction across 1,941 features. | Not a bar or prior for Melange. Self-report cells mostly at or below the 50% baseline, but A at 53 is above. The Melange N look used n=110 (r -0.0016, perm-p .511), not 90. The superseded +0.208 must not be presented as current. | moderate | VERIFIED-SOFTENED |
| [[rubio-2024-feasibility-data-analytics]] | 2024 | qualifies | All 100 participants are female (Sample 1: 78 psychology students 18-27; Sample 2: 44 aged 18-54); the authors list this as a limit. | A single-sex sample says nothing about mixed-sex pitch effects. Practical rule: any Melange pitch descriptor needs sex conditioning or the de-normalised SD. Commit 4193f0140 (branch claude/melange-speech-recognition-cfa049) is not an ancestor of main HEAD; live code still emits stddevNorm at line 217. | strong | VERIFIED |

### Assessment

**Evidence weight: the best-covered component, with 12 rows (1 VERIFIED, 11 VERIFIED-SOFTENED). They cover prosody and pitch only. Disfluency has no paper evidence.**

**1. Audio as a between-member trait signal: weak, and not replicated in Melange.**
- **Marrero 2022** (strong, method-source). In 614 students with self-report BFI-2 and 100-fold CV, vocal features gave out-of-sample R² 6.24%, LIWC 3.13% and the joint set 10.67%, which the authors describe as super-additive.
- **Melange's pre-registered look** (big5_audio_faithful_prereg, N = 90 legislators, Pink criterion). Results:
  - eGeMAPS-88 ridge reached held-out r +0.284 for Pink-E (perm-p 0.030) and +0.298 for Pink-ES (perm-p 0.017). Both are UNSTRATIFIED by sex as recorded in the row.
  - A and C were null.
  - Super-additivity missed its registered bar in all three looks (vocal added +0.001 for E; C -0.046).
  - Round 2 vocal against diligence: r -0.081 (perm-p 0.73).
- **Criterion caveat.** Pink has reliability of about 0.67, is text-derived and shares a source with the linguistic block, so even the E/ES hits are partly a same-source result.
- **Exploratory result mislabelled.** The +0.119 E-against-visibility result was **unregistered** and has been labelled a win. It is exploratory only.
- **Superseded figure.** An earlier +0.208 held-out r was superseded the same day as gender-confounded and must not be cited.
- **Audio-N look.** It was spent and missed on 2026-08-25 (brain: r -0.0016, perm-p .511).
- **Rubio 2024** (moderate). Single-feature correlations of about 0.30 to 0.48 in an all-female Sample 1 (n = 78) did not replicate in Sample 2 (n = 44). The authors say different microphones may have contributed, alongside noise, colds and fatigue. Their three-class accuracies sit near the 50% constant-central-class baseline: self-report 43 to 53%, expert 53 to 60%. Do not use them as a prior or bar.
- **Celli** (weak). Its cue counts (Biel 2012, E and ES) come from vlog impressions. Prioritising E for audio is an inference, not a transfer.

**2. Within-speaker pitch as arousal: supported as a transform, unsupported at Melange's sample sizes.**
- **Dietrich 2019** (strong, method-source). Per-speech mean F0 is z-scored against the speaker's own baseline. Effects are about 0.09 to 0.38 SD, estimated over 74,151 speeches and 619 members (House floor, 2009 to 2014). The authors call the raw magnitude 'slight'.
- **Melange's topic_emotional_intensity.** It reports a member's topic mean from as few as 2 units, with floors of 5 units per member and 2 per topic. With independent units, the standard error (SE) of a 2-to-5-unit mean is about 0.45 to 0.7 SD, several times the published effects. As built, the descriptor cannot detect a Dietrich-sized shift for one member and one topic.
- **Qualifications from Dietrich:**
  - Pitch measures arousal, not valence, so the descriptor must never be read as anger or enthusiasm.
  - The paper controls for duration, one-minute speeches and election year but not question intonation. Melange controls for none of these.
  - Program and channel offsets can exceed 0.1 SD, so a topic concentrated on one program picks up that offset. This applies equally to the Washington Journal subject_effects table, which shows shifts from as few as 2 reactions.
- **Status.** The descriptor's status is CANDIDATE_CONSUMED_BY_NOTHING_AWAITING_USER_RATIFICATION, which fits this evidence.

**3. Channel and normalisation.**
- **Mestre 2026.** Cross-speaker acoustic comparisons are confounded by microphone, compression and sampling. The paper normalises energy per debate and recommends baselines.
- **b5_audio_build.py**, per the Mestre row (lines 85 to 104), z-scores over *all pooled members* with no loudness or channel normalisation. Cross-member readings of pitch level, volume dynamics and strain are therefore exposed.
- `volume_dynamics_raw` uses loudness stddevNorm, which is partly level-invariant.
- Whether pause and pace are channel-robust is UNVERIFIED.

**4. Sex confound: the strongest transfer, and a live defect.**
- **Literature support.** Dietrich Table 2 gives raw F0 of about 203 to 206 Hz for women vs about 151 Hz for men. Marrero recovers gender with over 90% variance explained in its own 564 students; it does not show this holds generally. Mestre finds 3 of its top 5 by pitch are women. Together these support the output contract: `absolute_f0_enters_personality = False` and `audio_may_modify_big_five = False`.
- **System's own figures.** For `pitch_variability_raw` (stddevNorm), r(sex=F) = -0.266 and d = -0.944 over 1,324 clips / 309 members. This is brain-sourced; the r is also quoted in the 4193f0140 commit message. De-normalised semitone SD gives -0.065. Jitter is -0.260. Pace (+0.06) and volume_dynamics (-0.05) are sex-clean.
- **Status of the fix.** Commit 4193f0140 is **not on main**, as both the Rubio and Salam rows verified via git merge-base. The live candidate file still emits stddevNorm at line 217, and its percentiles pool men and women within outlet and program.
- **Salam 2022.** Its gender-split fusion lowers AMSE from 0.818 to 0.690, but on MFCC features with self-report from 147 volunteers. It supports a sex-conditioning *diagnostic*, not per-sex trait models.

**5. Disfluency (Whisper filler rates): GAP.**
- No paper bears on Whisper's accuracy for fillers or on disfluency as a trait or state signal.
- The brain's venue ladder (live WJ about 26 per 1k, with a stated live median of 21.8 per 1k; NPR 0.22 per 1k) points to a roughly hundredfold venue effect. That is consistent with editing removing fillers and suggests the rate measures the venue as much as the member. This is a lead, not a tested finding.
- **Study that would fill it:**
  - a human-coded filler transcription on a venue-stratified clip sample, to measure Whisper filler recall and precision
  - a within-member cross-venue reliability analysis (ICC), restricted to unedited live venues

### Contradictions

- Marrero 2022 reports super-additivity of vocal plus LIWC features. Melange's registered looks missed the super-additivity bar three times (E +0.001, C -0.046). The criteria differ (student self-report vs text-derived Pink, R-squared vs held-out r), so this is non-replication under different conditions, not refutation.
- Internal contradiction on normalisation: the Rubio row says b5_audio_build z-scores 'within source (verified in code)'. The Mestre row says lines 85-104 z-score over all pooled members, and that the line-104 cache note claiming z-within-source contradicts the code. One of the two verifications is wrong, so read b5_audio_build.py lines 85-104 to settle it.
- Internal contradiction on the audio-N look sample: the Celli and Dietrich rows refer to an N MISS at N = 90, while the Rubio row says the N look used n = 110 (r -0.0016, perm-p .511) and not 90.
- The +0.119 E-against-visibility result is labelled a win in the system record but was not a registered test. Under the one-look discipline it is exploratory.
- The system cites Dietrich 2019 for topic_emotional_intensity, but the paper's effects (0.09-0.38 SD) are identified across 74,151 speeches. At 2-5 units per member-topic (SE about 0.45-0.7 SD), the descriptor cannot resolve effects of that size.
- Mestre's own Figure 3a ranks candidates by raw cross-speaker pitch, while its text cautions against naive cross-speaker comparison. Cite the caution, not the figure.

### Gaps

- Whisper disfluency and filler rates: no paper evidence on ASR filler capture or on disfluency as a trait or state signal. Needed: a human-coded filler benchmark per venue, plus a within-member cross-venue reliability (ICC) study on unedited live audio.
- No study estimates channel effects (studio, remote, telephone) on eGeMAPS functionals in broadcast political audio. Needed: a within-member, cross-channel variance decomposition.
- No minimum detectable effect has been computed for per-member, per-topic pitch deviation. Needed: a power calculation from observed within-member unit variance.
- Question intonation is uncontrolled in both Dietrich and Melange. Needed: a question/statement tag per speech unit as a covariate.
- Venue numbers (WJ about 26/1k vs 21.8/1k live median; NPR 0.22/1k) are brain-sourced. Cite them with source and denominator.

### Paul's position vs the evidence

Paul is interested in non-textual channels (images, video, audio) and in Birkenmaier's paraverbal cues. The system concluded audio is display-only, and the evidence supports that conclusion:
- Vocal trait signal in the literature is small (Marrero 6.24% R-squared in students).
- Feature-level results failed to replicate across samples (Rubio).
- Melange's own registered looks were mixed: E and ES hit against a text-derived criterion, A, C and N missed, super-additivity missed three times, and the +0.208 result was confounded.
- Pitch is dominated by sex unless within-speaker standardised (Dietrich, Mestre, plus the system's own r -0.266).

On Paul's citations: Ramey et al. and Joo & Steinert-Threlkeld 2022 have no evidence rows bearing on audio prosody. Ramey is lexical and Joo & Steinert-Threlkeld is image and frame work, so they motivate the interest but give no support for an audio method. Dietrich 2019 is the one cited paper that does bear, and it supports a within-member arousal descriptor, not a personality input. The evidence also goes further than Paul's position in one respect: the display-only descriptors themselves need sex conditioning and minimum-unit floors before even display, and the sex fix is not yet on main.

### Open questions

- Will Paul ratify landing commit 4193f0140 (semitone SD replacing stddevNorm) on main? Until then the live pitch_variability_raw carries r(sex=F) -0.266 (1,324 clips / 309 members, brain-sourced).
- Which is correct: does b5_audio_build.py z-score within source or over all pooled members?
- Was the audio-N look run at n = 90 or n = 110?
- Should topic_emotional_intensity and the WJ subject_effects table get higher per-topic unit floors, or carry an explicit noise band given SE of about 0.45-0.7 SD at 2-5 units?
- Should the disfluency descriptor be limited to unedited live venues, given the roughly hundredfold WJ-vs-NPR gap?
- Should the Pink-E and Pink-ES results (+0.284, +0.298) be re-reported with sex strata, given the earlier +0.208 was superseded as gender-confounded?

### Read first

1. [[dietrich-2019-pitch-perfect-vocal]]
2. [[mestre-2026-potential-pitfalls-audio]]
3. [[rubio-2024-feasibility-data-analytics]]
4. [[marrero-2022-evaluating-voice-samples]]
5. [[salam-2022-learning-personalised-models]]
6. [[celli-nd-twenty-years-personality]]

## Acoustic speaker-ID (closed) and two-check identity design (proposed)

**What it computes.** Closed: cosine and neural speaker ID (EER .226-.317, failed). Proposed: forced alignment AND ECAPA-TDNN verification, fail-closed, with witness-turn negative controls.

**Method.** Pilot bars: EER <= 5%. Awaits rulings D1-D12. Production is blocked at 0 profiles.

**Code refs.** `b5_audio_embed/enroll/verify2` (worktree branch), `DRAFT big5_audio_identity_prereg.md`, `fec/audio_rules.py`, `b5_audio_neural_head.py`

**Assumptions.**
- Speaker embeddings separate members at EER <= 5%

### Evidence

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[mestre-2026-potential-pitfalls-audio]] | 2026 | method-source | s4.3 and fn 9 (p.321): one neural layer over 768-dim facebook/wav2vec2-base sentence embeddings (no pooling stated), Adam lr 0.001, cross-entropy, batch 8, early stopping patience 3. s4.1 (p.318): aeneas MFCC/DTW forced alignment. MFCC CNN baseline far weaker. | The recipe transfers as a method. No split protocol or pooling detail in the paper, so mean pooling, max_epochs 200 and seed 0 are Melange choices; the head docstring's pooling claim is unsupported. Paul's 'No annotated literature' claim was not located and stays UNVERIFIED. | strong | VERIFIED-SOFTENED |
| [[mestre-2026-potential-pitfalls-audio]] | 2026 | qualifies | Test accuracy 0.914 across 156 speakers, 0.946 across 34 candidates (MFCC CNN 0.081, 0.134). Per-debate wav2vec2 0.974 vs MFCC 0.888. s4.2 warns cross-speaker acoustic comparisons are confounded by microphone, compression, sampling. Split not described. | 0.914 sets no expected level for congressional audio. Melange pilot: 21 members, six sources, caption-derived labels, calibration EER .226, FAILED_WEAK_LABEL_SMOKE; earlier 30-member logistic head EER .317. The channel-leakage explanation for the gap is analyst hypothesis, UNVERIFIED (DE-062). Pilot is source-disjoint, not event-disjoint. | moderate | VERIFIED-SOFTENED |
| [[mestre-2026-potential-pitfalls-audio]] | 2026 | supports | Aeneas validated on 500 utterances x 5 crowd ratings: PA 85.26%, DA 95.76% (s4.1, p.318). Only 5% not aligned at all, concentrated in short or interrupted sentences. No word floor or ASR word-match threshold proposed. | Supports only that short sentences align poorly. `audio_rules.py` bars (MIN_CLIP_TARGET_SEC 30, MIN_CLIP_TARGET_WORDS 50, ALIGN_MIN_TURN_WORD_MATCH 0.80, ALIGN_MIN_HEARING_WORD_MATCH 0.70) have no paper or located calibration source. Vault note line 60 wrongly lists the 50-word floor as a paper method. | moderate | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: three VERIFIED-SOFTENED rows, all from Mestre 2026 (one strong, two moderate). They support the method recipe and the aligner, but neither the expected accuracy nor the gating thresholds.**

**Method transfer (strong).** Mestre sections 4.1 and 4.3 and footnote 9 describe:
- a single neural layer over 768-dimensional facebook/wav2vec2-base sentence embeddings
- Adam at learning rate 0.001, cross-entropy loss, batch size 8, early stopping with patience 3
- aeneas MFCC/DTW forced alignment

`b5_audio_neural_head.py` implements this without a hyperparameter search. Three caveats:
- The paper gives **no split protocol and no pooling method**, so the head docstring's claim that the paper mean-pools embeddings is unsupported.
- Mean pooling, max_epochs 200 and seed 0 are Melange choices.
- The section 4.3 citation sits in b5_audio_neural_pilot.py at line 225, not in the head file.

**Accuracy does not transfer.** Mestre reports test accuracy of 0.914 across 156 speakers and 0.946 across 34 candidates (MFCC CNN: 0.081 and 0.134), on televised debates with an undescribed split. Melange's results:
- The pilot used a fresh cohort of 21 members across 6 distinct sources (3 train, 1 early-stopping, 1 calibration, 1 untouched test) with caption-derived MACHINE_CAPTION_HARD_SIGNAL labels. It reached calibration EER .226 and ended FAILED_WEAK_LABEL_SMOKE.
- An earlier 30-member logistic head reached EER .317.
- The 11/30 cosine attempt was not located.

The gap between 0.914 and an EER of .226 has no paper-backed explanation. The channel-leakage hypothesis is analyst reasoning and UNVERIFIED (DE-062): Mestre's confound warning concerns cross-speaker acoustic comparisons, not classifier accuracy. The pilot split is source-disjoint, not event-disjoint.

**Alignment gate.** Aeneas was validated on 500 debate utterances with 5 crowd ratings each: PA 85.26%, DA 95.76%. Failures were concentrated in short or interrupted sentences. The paper supports only the claim that short sentences align poorly. None of the following is paper-derived, and no calibration artifact or 150-clip coding packet was located:
- MIN_CLIP_TARGET_WORDS 50
- MIN_CLIP_TARGET_SEC 30
- ALIGN_MIN_TURN_WORD_MATCH 0.80
- ALIGN_MIN_HEARING_WORD_MATCH 0.70

The vault note concepts/single-look-holdout-discipline-cases-2-3-n-slot-and-gold-template.md (line 60) wrongly lists the 50-word floor as Mestre's method.

The claim that debate transcripts are cleaner than hearing transcripts is UNVERIFIED. ECAPA-TDNN appears by name only, as a labelled comparator in b5_audio_embed.py, and has no evidence row.

**Gaps: studies that would fill them.**
- A congressional speaker-verification benchmark with event-disjoint and channel-matched impostor trials, comparing ECAPA-TDNN against the wav2vec2 head on human-verified (not caption-derived) labels.
- A human-coded alignment packet (for example 150 clips, venue-stratified) to calibrate the 0.80 and 0.70 word-match bars and the word floor.

### Contradictions

- The vault note concepts/single-look-holdout-discipline-cases-2-3-n-slot-and-gold-template.md line 60 attributes the 50-word floor to Mestre 2026. The paper proposes no word floor and no ASR word-match threshold.
- The b5_audio_neural_head.py docstring says the paper mean-pools embeddings. The paper does not state a pooling method.
- The paper reports 0.914 speaker-ID accuracy on 156 debate speakers, while the Melange pilot reached calibration EER .226 (21 members) and an earlier .317 (30 members). The settings differ (debates vs mixed broadcast; undescribed split vs source-disjoint), so the paper sets no expected level.
- Paul's position-table entry 'No annotated literature' sits alongside the system's own citation list, which includes Mestre & Ryan (2026) for this design and ECAPA-TDNN by name. The Mestre row also notes the position-table claim could not be located in Project Brain or the vault, so it stays UNVERIFIED.

### Gaps

- No speaker-verification evaluation on congressional audio with event-disjoint splits and channel-matched impostors. Needed: a human-labelled benchmark comparing ECAPA-TDNN against wav2vec2 heads.
- No calibration source for ALIGN_MIN_TURN_WORD_MATCH 0.80, ALIGN_MIN_HEARING_WORD_MATCH 0.70, MIN_CLIP_TARGET_WORDS 50 or MIN_CLIP_TARGET_SEC 30. Needed: a human-coded alignment packet stratified by venue, with the bars set from its ROC.
- Aeneas validation exists for debates only. Hearing and interview alignment quality is untested.
- The channel-leakage explanation for the EER gap is an UNVERIFIED lead (DE-062). Needed: an ablation that holds the channel fixed across train and test.
- The 11/30 cosine attempt and the 150-clip coding packet were not located. Their records need recovering before they are cited.

### Paul's position vs the evidence

Paul's position is 'No annotated literature'. In substance that is close to right: only Mestre 2026 bears on this component, and it supports the recipe and the aligner, not the thresholds or an expected accuracy for congressional audio. Strictly, though, it is inaccurate on two counts:
- The system already cites Mestre & Ryan (2026), and ECAPA-TDNN by name.
- The provenance of the position statement itself was not located (UNVERIFIED).

The evidence agrees with the system's decision to close the acoustic speaker-ID attempt (EER .226, FAILED_WEAK_LABEL_SMOKE, production_consumers 0). It gives no support yet for the specific bars in the proposed two-check design.

### Open questions

- Should the vault note line 60 be corrected so it no longer attributes the 50-word floor to Mestre 2026?
- Where are the calibration records for the 0.80 and 0.70 word-match bars and the 150-clip coding packet, or were the bars set by judgment?
- Would an event-disjoint, channel-matched split change the EER materially, and is that look worth spending given the lane has no rank consumer?
- Should ECAPA-TDNN (speaker-verification pretrained) be the primary candidate over a wav2vec2 head trained on caption-derived weak labels?
- Can the identity design use human-verified labels instead of MACHINE_CAPTION_HARD_SIGNAL, so it does not inherit the caption-verification gap?

### Read first

1. [[mestre-2026-potential-pitfalls-audio]]
2. [[dietrich-2019-pitch-perfect-vocal]]
3. [[rubio-2024-feasibility-data-analytics]]

## Across components

**1. Identity is upstream of everything.** Every prosody and disfluency descriptor is computed on clips kept by the caption-plus-LLM speaker check, and that check has no validating literature. If the brain's 33.7% NEUTRAL share (unchecked) is close, about a third of descriptor inputs rest on the absence of contrary evidence. The two-check identity design was meant to close this, but it is closed at EER .226, so the prosody evidence is conditional on an unvalidated filter. Precision of the kept set per venue is the single highest-value measurement for the whole lane.

**2. Channel is the shared confound across all three components.**
- Mestre (cross-speaker comparisons confounded by microphone, compression and sampling)
- Rubio (non-replication across two microphones, among other factors)
- Dietrich (one venue only; program offsets can exceed effect sizes)

All three point the same way. It affects cross-member prosody readings, the speaker-ID EER gap (UNVERIFIED lead) and disfluency rates (WJ about 26/1k vs NPR 0.22/1k). No study in the set quantifies channel variance for broadcast political audio. One within-member, cross-channel variance study would inform all three components at once.

**3. Sex is the main fault line for pitch.** It is well handled in the output contract (absolute F0 and inferred sex excluded from personality), but not in the live descriptor code. The semitone-SD fix (4193f0140) is off main, and percentiles pool men and women within outlet and program. Under the project rule, any pooled pitch coefficient must be quoted with sex strata or labelled UNSTRATIFIED.

**4. Documentation drift.** These internal inconsistencies should be reconciled before any of them is cited:
- the vault note misattributes the 50-word floor to Mestre
- the neural head docstring asserts mean-pooling the paper does not state
- the build-script cache note says z-within-source while the code pools across members
- the audio-N sample is given as both 90 and 110
- the exploratory +0.119 result is labelled a win

**5. Consistent with project rulings.** Nothing in this evidence argues for audio entering rank or engine scoring. The display-only status stands. No row touches SNA edges, LDA, reciprocity or influence language, so none of the barred proposals arise here.

## Research agenda

1. Audit speaker-verification precision and recall. Use a human-coded, venue-stratified sample of kept clips (MATCH vs NEUTRAL) plus rejected clips, and name the window and clip denominators. Highest priority, because every downstream descriptor depends on it.
2. Check whether Clerk HouseLive floor-video metadata carries per-speech member attribution. If it does, use it as an independent identity source for House floor clips from 2009 on.
3. Ask Paul to rule on landing commit 4193f0140 (semitone SD) on main. Then re-run the sex-loading check on pitch_variability_raw and jitter, reporting strata.
4. Resolve the internal contradictions by reading the code and records: b5_audio_build.py lines 85-104 (pooled vs within-source z); the audio-N look sample (90 vs 110); the vault note line 60 misattribution; the neural head docstring's pooling claim.
5. Run a within-member, cross-channel variance decomposition (studio, remote, telephone; program) for eGeMAPS functionals and filler rates on unedited live audio.
6. Compute a minimum detectable effect for per-member, per-topic pitch deviation from observed within-member unit variance. Then set topic floors, or show noise bands, on topic_emotional_intensity and the WJ subject_effects table.
7. Build a Whisper filler benchmark: human-coded filler transcription on a venue-stratified clip sample, giving filler recall and precision per venue and a within-member cross-venue ICC.
8. Calibrate the identity gates with a human-coded alignment packet (for example 150 clips, venue-stratified) to set ALIGN_MIN_TURN_WORD_MATCH, ALIGN_MIN_HEARING_WORD_MATCH and the word and second floors from data.
9. Only if Paul wants to reopen identity: run a pre-registered single look comparing ECAPA-TDNN against the wav2vec2 head on human-verified labels with event-disjoint, channel-matched impostor trials.
10. Re-report the Pink-E and Pink-ES held-out results (+0.284, +0.298; N = 90) with sex strata, or keep them labelled UNSTRATIFIED. Keep the +0.119 visibility result labelled exploratory.
