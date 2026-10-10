---
type: theme
name: "Personality & Psychometrics"
paper_count: 8
tags: [theme]
---

# Personality & Psychometrics

This theme has eight works. They fall into three groups: the lexical tradition of reading personality from text ([[pennebaker-2003-psychological-aspects-natural]], [[ramey-2016-measuring-elite-personality]], [[celli-2012-role-emotional-stability]]), newer frameworks for politicians' projected persona ([[lechner-2025-mapping-political-personality]], [[birkenmaier-2025-measuring-politicians-public]]), and the measurement criteria that any reader of text or voice is scored against ([[soto-2017-next-five-inventory]], [[mondak-nd-online-appendix-personality]], [[rubio-2024-feasibility-data-analytics]]). For Paul's systems none of these is background reading. Five are named or functional method sources: the lexical Big Five in `big5_traits.py`, the affective-N slot, the agency/communion persona coder in `agency_communion_llm.py`, the frozen Senate expert gold in `big5_senate_expert_gold.json`, and the display-only audio lane.

The works agree on one thing. A trait read from text or voice is a weak, cue-based signal, and how strong it looks depends on which criterion it is scored against. They disagree on how much indirect validity counts, on whether the target is the intrinsic trait or the projected persona, and on whether observer ratings are a better gold standard or a more biased one. No work in the theme tests what the system most relies on: how a legislator's personality changes their response to lobbying or pressure. The receptivity-layer signs therefore remain design priors and are not supported by this literature.

Paul's own notes add two framing points. The first is his ML/sentiment annotated bibliography: these methods are meant to produce actor-level attributes that become graph nodes, and the step from model output to a node or vector is never specified. Ramey is the older foundation and Birkenmaier the newer method with a codebook behind it. The second is his internal lane documents. The 2026-08-29 speech/audio status report records that every attempt to validate audio as a Big Five channel missed or was confounded (ruling D-013, display-only). The 2026-08-31 approach-brief handoff sets the construct rules: agreeableness means low antagonism, neuroticism means affective content, and percentiles stay out of prose.

A note on sourcing. Paul's notes for this category come from four documents. Two of them, the shortest-path/pathfinding reading list and the approach-brief handoff, cite none of the eight works. Where they appear below, they are Paul's design context, not evidence from the literature.

## Strands

### Closed-vocabulary (lexical) trait inference: from word counts to legislators

This is the lineage behind the system's lexical Big Five. [[pennebaker-2003-psychological-aspects-natural]] is a narrative review with no new data and no pooled effects. It argues that function-word use is a stable individual difference. Its own evidence sets a low ceiling. Successive 2-minute speech segments correlate at r = 0.51 (21 categories). Four-week test-retest in everyday student speech is 0.41 for standard linguistic categories and 0.24 for psychological-process categories. Big Five self-reports correlate with word use at only r = 0.10 to 0.16 (Pennebaker & King 1999; several hundred college students). Emotion words make up under 5% of all words. The review also reports that judges' ratings of a text's emotionality track its language better than the writers' self-reports do. That finding comes back in the criterion debate below.

[[ramey-2016-measuring-elite-personality]] carries this toolkit (LIWC + MRC features through pre-trained Mairesse SMOreg models) to every House member-year from 1996 to 2014. Its scores are on a 1-7 scale defined relative to the Congressional Record. Precision is good: above about 5,000 words a year, intervals narrow to about plus or minus 0.15. Validity is only indirect. Lifetime trait means explain R2 = 0.10 of Common Space ideology (N = 844 members). Emotional Stability predicts NPAT response, rising from about 30% to 53% between -2 and +2 SD (N = 598). Conscientiousness lowers the share of ceremonial bills (-0.39 to -0.58; N = 3,648 member-Congresses, 104th-112th), and a Majority x C interaction of +0.27 to +0.29 places the effect mostly in the minority party. There is no inventory criterion. The speechwriter defence covers floor speech only. The paper's precision should not be read as validity, and Pennebaker's r = .10-.16 is the reason.

[[celli-2012-role-emotional-stability]] uses the same Mairesse-style cues in an unsupervised Twitter tagger, built on about 200,000 English posts from December 2011 (5,392 ego-networks). Validation is tiny: 0.625 accuracy against a 0.5 baseline on 8 users, and average F1 0.578 on 18 users against Analyzewords, a LIWC-style proxy. The paper reports no inferential tests. It is a lineage and caution marker. Like the system's affective-N anchor, it scores a LIWC-family tagger against a LIWC-family proxy, which is shared-method circularity.

Paul's ML-bibliography notes treat Ramey as the older foundation of this strand. He names its gap as no pipeline for textual venues beyond its own, and no framework for contextual or auditory constraints.

Works: [[pennebaker-2003-psychological-aspects-natural]], [[ramey-2016-measuring-elite-personality]], [[celli-2012-role-emotional-stability]]

### Public persona: facet frameworks and LLM coding of projected traits

This strand changes the target. It measures the traits a politician projects, not the traits they have. [[lechner-2025-mapping-political-personality]] (PoliPers v0.1, a working paper that is not peer reviewed) defines 15 politically relevant facets and maps each to the Big Five, HEXACO, VIA, the Big Two (agency/communion) and leadership traits. It tests them in German samples: self-ratings (N = 404), ideal-politician ratings (N = 589), and crowd ratings of Trump and Merkel (N = 211). Self-rating reliability is uneven: alpha runs from .44 to .87, and Loyalty, Humility and Compromise all fall at or below .51. Observer ratings carry a large evaluative component. Empathy correlates with likeability at r = .70/.76, and Integrity correlates with the rater's ideological proximity at .46 for Trump. The paper calls these associations "predictive validity", but all of them are concurrent.

[[birkenmaier-2025-measuring-politicians-public]] turns two PoliPers facets into a text codebook: Agency (Assertiveness & Will to Take Control) and Communion (Empathy & Compassion), positive poles only. It frames the work with Brunswik's lens model, under which observed cues mix true personality with strategic self-presentation. The data are 45,000 German sentences from 46 party leaders, 2017-2024. Few-shot DeepSeek-V3 and GPT-4o reach macro-F1 0.74-0.77, fine-tuned XLM-R 0.70, a bag-of-words SVM 0.55-0.63, and Llama 3-8B about 0.55-0.60. Human coder alpha is only .70-.78, and 89-94% of sentences carry no cue. Strong-only definitions beat strong-plus-weak. Only GPT-4o and DeepSeek pass the 18-unit functional test. Communion share falls from left to right (LINKE 19% down to AfD/FDP about 6%; denominator is cue-bearing sentences per party).

In Paul's notes this is the newer, codebook-backed method that improves on Ramey. His stated gaps are that it does not separate ambiguous statements and does not say how a classifier output becomes a graph node. Neither paper tests susceptibility to pressure, lobbying, cosponsorship or votes.

Works: [[lechner-2025-mapping-political-personality]], [[birkenmaier-2025-measuring-politicians-public]]

### Criterion instruments and gold panels: what trait readers are scored against

[[soto-2017-next-five-inventory]] supplies the general-population benchmark. The BFI-2 has 60 items, 15 facets and balanced keying. Domain alpha is at least .83 and facet alpha .66-.85 (independent samples: internet N = 1,000, student N = 470). Self-peer agreement averages .56 for domains and .49 for facets (peer n = 184). Facets explain more criterion variance than domains (mean R2 .33 vs .27 across 20 criteria). Raw-item PCA shows an acquiescence component, which within-person centering removes. Two numbers matter most for the system: .56 is a realistic ceiling for convergence between any trait reader and an other-rater gold, and the acquiescence finding explains how unbalanced keying can create halo.

[[mondak-nd-online-appendix-personality]] is not background reading. It is the frozen external gold. It gives mean Big Five ratings of US senators from insiders and APSA Fellows (about 129 rows) and from academics (about 126 rows). It states no scale anchors, aggregation rule or era. Senators rated by one coder appear in the tables but are excluded from the main paper's analyses. The extremes are party-sorted: Democrats are 62%/65% of the insiders' high/low scorers, and in the academic table 80% of high scores go to Democrats and 80% of low scores to Republicans. The main article was not read, only this appendix.

PoliPers ([[lechner-2025-mapping-political-personality]]) belongs here too. It validates its facets against the BFI-2-XS, and it is the clearest evidence in the theme that observer ratings of politicians fold in liking and ideological proximity.

Works: [[soto-2017-next-five-inventory]], [[mondak-nd-online-appendix-personality]], [[lechner-2025-mapping-political-personality]]

### Paralinguistic channel: personality from voice

[[rubio-2024-feasibility-data-analytics]] (Sensors 2024) recorded about 7-minute mock job interviews with Spanish women, mostly students. It correlated 1,941 openSMILE AVEC-2011 functionals with three Big Five criteria: NEO-FFI self-report, close-other ratings and zero-acquaintance expert ratings. In Sample 1 (n = 78), the best single features reach r of about 0.30-0.48, and expert ratings track voice most closely (E 0.48, N -0.47). The central result is negative: the top features did not replicate in Sample 2 (n = 44), and the authors suggest a microphone change. Three-class random-forest prediction from self-report scores 43-53%, no better than the 50% you get by always predicting "medium". The paper does not say whether features were selected inside each fold, applies no multiple-comparison control across 1,941 features, and its Discussion numbers contradict its Results (0.38/0.40 vs 0.47/0.48; it also states N = 100 while its samples add to 122).

This matches Paul's own 2026-08-29 audio status report. That report found no Big Five channel in audio. ES held-out r = +0.208 fell to a partial r of -0.029 after gender and party. E audio-alone r = +0.228 sat inside the permutation noise floor. The N prereg missed all four bars (r = -0.0016, n = 110). Audio A and ES are halo-inseparable (|r| = 0.819). The report also found that some descriptive speech habits are reliable (quirk rate Spearman-Brown 0.794 at 5 units), but filler rates differ about 100x across venues. Rubio and Paul's lane reach the same verdict: channel and venue changes erase feature-level findings, and audio stays descriptive.

Works: [[rubio-2024-feasibility-data-analytics]]

## Agreements and debates

**1. Which criterion counts as truth?** Three works independently find that outside judges track surface cues better than self-reports do. [[pennebaker-2003-psychological-aspects-natural]] finds that judges' ratings of a text's emotionality match its language more closely than the writer's self-report does. [[rubio-2024-feasibility-data-analytics]] finds zero-acquaintance experts correlate with voice most strongly (E 0.48) and predict best (60% vs 43-53% for self-report). [[soto-2017-next-five-inventory]] shows that peers converge with self only at about .56. Read one way, observer criteria are the right target for a public-persona system. [[lechner-2025-mapping-political-personality]] and [[mondak-nd-online-appendix-personality]] support the other reading: observer ratings of politicians carry liking (Empathy x likeability r = .70/.76), ideological proximity (Integrity x proximity r = .46 for Trump) and party sorting (80/80 at the academic extremes). A text reader that converges with an observer gold may be converging on partisanship or halo, not on personality. The theme does not settle this. Any convergence figure against the Senate gold should be quoted with party partialled out, or labelled UNSTRATIFIED.

**2. Indirect validity versus human-labelled validity.** [[ramey-2016-measuring-elite-personality]] relies on precision (bootstrap CIs) and on indirect criteria: ideology signs, NPAT nonresponse and ceremonial bills. [[birkenmaier-2025-measuring-politicians-public]] calls its pre-trained SVM outdated and unvalidated against human annotation, and runs it as a baseline (SVM 0.55-0.63 vs LLM 0.74-0.77 macro-F1). This is a criticism of method, not a contradiction of Ramey's findings. The two papers measure different constructs (Big Five vs Big Two) in different languages and venues.

**3. Intrinsic trait or projected persona?** Ramey assumes floor speech reveals the member, and its speechwriter defence covers floor speech only. Birkenmaier's lens model says observed cues always mix trait with strategic self-presentation, and PoliPers builds an inventory for perceived personality. Paul's construct rules in the approach-brief handoff take the Birkenmaier side in practice. Agreeableness is read as low antagonism, neuroticism as affective content, and setting differences are descriptive, never causal.

**4. Effect-size ceiling.** Pennebaker's r = .10-.16 between word use and self-reported Big Five sits against the much tighter numbers in the elite lexical work. Ramey's narrow CIs measure reliability within the Mairesse model, not agreement with any external trait measure. In both [[celli-2012-role-emotional-stability]] and the system's own RKH comparison, apparent agreement comes partly from scoring a LIWC-family reader against a LIWC-family criterion.

**5. Trait homophily.** [[celli-2012-role-emotional-stability]] reports that Twitter users talk with others who share their emotional-stability label. It gives no coefficient and no test. The system already measured persona homophily on its own corpus and recorded it as UNEARNED (|r| <= 0.015; the window and denominator are not stated in the theme notes and should be cited from the receipt). A 2011 Twitter frequency count does not overturn that.

**6. Replication.** Rubio's voice features did not replicate across two samples, Celli runs no inferential tests, and PoliPers is a v0.1 working paper. Only [[soto-2017-next-five-inventory]] reports independent-sample replication of its structure.

## Methods

**Dominant methods**
- **Dictionary counts with pre-trained regressors.** LIWC/MRC features run through Mairesse SMOreg ([[ramey-2016-measuring-elite-personality]], [[celli-2012-role-emotional-stability]]; foundations in [[pennebaker-2003-psychological-aspects-natural]]). Weaknesses: the regressors were trained on student essays and transferred to floor ritual; word counts ignore context and irony; criteria often share the method; validation is indirect or tiny (8 and 18 users in Celli).
- **Codebook-guided LLM coding.** Few-shot prompting validated against a human test set, with functional test items and partisan-sensitivity checks under ValiText ([[birkenmaier-2025-measuring-politicians-public]]). Weaknesses: the human ceiling is only alpha .70-.78; 89-94% of sentences carry no cue, so per-actor scores rest on few cues; results come from German and proprietary models and are tied to specific model versions.
- **Classical inventory psychometrics.** Alpha, omega, PCA with Horn parallel analysis, CFA with acquiescence factors, and self-peer multitrait-multimethod checks ([[soto-2017-next-five-inventory]], [[lechner-2025-mapping-political-personality]]). Weaknesses: general-population or German citizen samples; PoliPers labels concurrent associations as "predictive".
- **Expert or observer panels as gold** ([[mondak-nd-online-appendix-personality]]). Weaknesses: no stated anchors or aggregation rule; single-coder rows; party-sorted extremes; observer evaluative halo.
- **Acoustic functionals with ML** ([[rubio-2024-feasibility-data-analytics]]). Weaknesses: 1,941 features with no multiple-comparison control; possible feature-selection leakage under leave-one-out; one-sex sample that hides the pitch-sex confound; channel instability.

**Cross-cutting weaknesses**
1. No work uses legislators' behavioural response to pressure as a criterion.
2. Most criteria are concurrent and taken on the same occasion.
3. Shared-method convergence (LIWC against LIWC) inflates agreement.
4. Party and ideology run through every criterion, and no work stratifies by them when validating.
5. Samples are students, Germans, or one-sex groups, so transfer to US members of Congress is assumed, not shown.

## How the works relate

- [[birkenmaier-2025-measuring-politicians-public]] — builds-on — [[lechner-2025-mapping-political-personality]]: Takes its agency and communion definitions, the positive-pole rule and the 18 functional test items from PoliPers (Sec 3.2.1.1; Sec 4.3). The two papers share authors.
- [[birkenmaier-2025-measuring-politicians-public]] — contradicts (method critique) — [[ramey-2016-measuring-elite-personality]]: Calls Ramey's pre-trained SVM outdated and unvalidated against human annotation, and runs it as the weakest baseline. This criticises the method, not Ramey's findings. Note the year mismatch: Birkenmaier cites Ramey as 2019, PSRM 7(1), but the vault slug says 2016.
- [[ramey-2016-measuring-elite-personality]] — builds-on (loose) — [[pennebaker-2003-psychological-aspects-natural]]: Ramey's LIWC base is Pennebaker & King 1999 and LIWC 2001. The 2003 review is not cited directly, so this is a shared lineage.
- [[celli-2012-role-emotional-stability]] — same-method — [[ramey-2016-measuring-elite-personality]]: Both use Mairesse et al. 2007 closed-vocabulary cue weights with N = -ES. Celli applies them to Twitter users, Ramey to House members.
- [[celli-2012-role-emotional-stability]] — same-method — [[pennebaker-2003-psychological-aspects-natural]]: LIWC word-category foundations (negemo, first-person singular) sit behind Celli's cues and Analyzewords criterion, and behind the system's affective-N anchor.
- [[lechner-2025-mapping-political-personality]] — same-method — [[soto-2017-next-five-inventory]]: PoliPers builds its facets on the BFI-2 hierarchy and validates them against the BFI-2-XS (Study 2, Table 3).
- [[pennebaker-2003-psychological-aspects-natural]] — agrees — [[rubio-2024-feasibility-data-analytics]]: Both find outside judges track surface cues (text, voice) better than self-reports do. Pennebaker: judges vs writers on emotionality. Rubio: expert E r = 0.48 and best prediction at 60%.
- [[lechner-2025-mapping-political-personality]] — complements (qualifies) — [[mondak-nd-online-appendix-personality]]: PoliPers shows observer ratings of politicians carry liking and ideological proximity. The Mondak expert extremes are 62-80% party-sorted. Together they warn that observer gold may encode partisanship. PoliPers concerns citizen raters, Mondak expert raters.
- [[mondak-nd-online-appendix-personality]] — complements — [[ramey-2016-measuring-elite-personality]]: Expert trait ratings are the external criterion that Ramey-style lexical scores lack. Ramey cites Mondak 2010 for personality-ideology links. Note the chamber mismatch: Ramey scores the House, Mondak rates the Senate.
- [[soto-2017-next-five-inventory]] — complements — [[mondak-nd-online-appendix-personality]]: BFI-2 self-peer agreement (domain mean .56, facet .49) gives a realistic ceiling for convergence with an other-rater gold such as the Senate panels.
- [[rubio-2024-feasibility-data-analytics]] — complements — [[mondak-nd-online-appendix-personality]]: Rubio's zero-acquaintance expert criterion is analogous to the Senate expert panels. Rubio does not cite Mondak.
- [[rubio-2024-feasibility-data-analytics]] — complements — [[ramey-2016-measuring-elite-personality]]: Rubio reads the acoustic channel, Ramey the lexical one. Both seek traits without surveys, and Rubio's non-replication is a caution for any added channel.
- [[soto-2017-next-five-inventory]] — complements — [[rubio-2024-feasibility-data-analytics]]: Rubio's close-other vs self agreement (0.32-0.51) sits just below BFI-2 self-peer means (.56/.49). Both show the limits of other-rater convergence. The instruments differ (NEO-FFI vs BFI-2).
- [[pennebaker-2003-psychological-aspects-natural]] — complements — [[birkenmaier-2025-measuring-politicians-public]]: Birkenmaier supplies the elite-population, human-labelled validation that the student-based review lacks.
- [[birkenmaier-2025-measuring-politicians-public]] — complements — [[mondak-nd-online-appendix-personality]]: Both are routes to validating public persona: a human-coded sentence test set vs expert trait ratings. Neither is used to validate the other.
- [[koutsoumpis-2024-kernel-truth-text]] — extends — [[pennebaker-2003-psychological-aspects-natural]]: A later meta-analysis of Big Five x LIWC correlations that plausibly updates the r = .10-.16 figures. It is not in this theme, and its year (2022 vs 2024) is unresolved.
- [[celli-2012-role-emotional-stability]] — does-not-corroborate — [[byrne-2014-chokes-under-pressure]]: Celli's N findings concern conversation structure. Byrne is the N-under-pressure premise behind the receptivity N weight. They do not support each other.

## What it means for the systems

**Lexical Big Five (`fec/members/big5_traits.py`)** — [[systems/melange-psychometrics]]
- Method source: [[ramey-2016-measuring-elite-personality]] (VERIFIED-SOFTENED). The Pennebaker-King fallback weights are overridden whenever `mairesse_smoreg.json` is present, and it is present.
- `lep_criterion.py` reran Ramey's Table 3 ceremonial-share test as the paper-exact C criterion. It came back null for all three C measures: rho +0.000 to +0.018, n = 440 members with at least 8 sponsored bills, 115th-118th Congresses pooled. That run did not split majority from minority, which is the fault line Ramey says carries the effect, so the null is **UNSTRATIFIED**. It neither refutes nor confirms Ramey.
- Calibration: the mean held-out r of .350 against RKH is above the r = .10-.16 that [[pennebaker-2003-psychological-aspects-natural]] supports. That fits the shared-method inflation the system already records, since RKH comes from the same Mairesse-LIWC family. RKH should not be treated as ground truth ([[systems/melange-validation]]).
- The theme also qualifies the system's pooling. Ramey's speechwriter defence does not cover the tweets and interviews the system pools in.

**Affective N** — [[systems/melange-psychometrics]]
- Since round 7b, the LIWC base is a convergent anchor only (r = +.24) and is no longer part of the score. [[celli-2012-role-emotional-stability]] has the same LIWC-on-LIWC circularity (UNVERIFIED lead).
- Lead from [[soto-2017-next-five-inventory]]: the affective-N LLM read sums pessimism, anxiety, depression and volatility, and those BFI-2 facets predict different criteria (UNVERIFIED lead per DE-062, although the bearing tags read VERIFIED-SOFTENED; the tags and the summary disagree and should be reconciled).

**Persona agency/communion (`fec/members/agency_communion_llm.py`)** — [[systems/melange-psychometrics]]
- Ports the codebook, the positive-pole rule and the 18-unit test from [[birkenmaier-2025-measuring-politicians-public]] / [[lechner-2025-mapping-political-personality]].
- It reproduces the left-greater-than-right communion pattern (D .099 vs R .049; the window and denominator are not stated in the notes and should be quoted from the receipt).
- It departs from the paper in four ways that matter: Claude Sonnet in English; weak cues kept, although the paper found strong-only works better; 40 self-referential floor/hearing sentences per member; and no human-labelled macro-F1 test set. The last is the largest validity hole.

**Receptivity layer** — [[systems/melange-influence-engine]]
- The .60 z(communion) - .40 z(agency) mapping to lobbying susceptibility is a design prior. No work here tests personality-to-pressure.
- Its one behavioural test (H4, +0.067 on Nokken-Poole drift) is below threshold.
- PERSONA_W stays at 0.30 per standing rulings. This literature argues for labelling the signs as priors, not for changing the weight.

**Gold-panel validation (`build_big5_senate_gold.py`)** — [[systems/melange-validation]]
[[mondak-nd-online-appendix-personality]] bears on four choices (VERIFIED / VERIFIED-SOFTENED):
- Validators keep single-coder rows that the authors drop.
- The two panels are averaged, although the academic panel is secondary.
- Neither primary validator partials out party, despite 80/80 party sorting.
- The extractor yields 125 academic rows against about 126 in the text, and there is a malformed '4..33' cell.

The pattern "O passes only against academics, and lexical O reversed on the party check" may be party-driven. That stays an **UNVERIFIED** hypothesis.

**LLM item reader (`big5_llm.py`)** — [[systems/melange-psychometrics]]
UNVERIFIED lead from [[soto-2017-next-five-inventory]]: the code keys O, C, E and A at 3 true to 1 false and calls this "balanced keying". By BFI-2 acquiescence logic, that could inflate the intercorrelation among O, C, E and A (halo). The BFI-2-XS expert panel is CANCELED and is not re-proposed here.

**Audio lane** — [[systems/melange-audio-speech]]
[[rubio-2024-feasibility-data-analytics]] supports D-013 (display-only, no engine wiring). The sex confound in pitch variability that Melange measured is not fixed on the live branch.

**Graph and network use** — [[systems/melange-sna]], [[systems/melange-pathfinding]]
Paul's bibliography names the unspecified step from model output to graph node as the most repeated gap, and his pathfinding notes want traits on edges. No work in this theme offers either. Any trait-derived node attribute or edge weight is a heuristic model output, never a closeness, access or introduction probability (D-025). Celli's trait homophily is too weak to justify trait edges, and the system's own homophily test was UNEARNED. Neither rejected SNA proposal (dropping regular cosponsorship edges; per-edge volume normalization) is argued for in this theme.

## Gaps

1. **Personality to pressure response in legislators.** No work tests whether any trait, or projected persona, predicts response to lobbying, cosponsorship or vote change. This is the evidence the receptivity layer most needs.
2. **US-English human test set for agency/communion.** Birkenmaier's validation is German and multi-venue. The system's port has no human-labelled macro-F1. A small hand-coded set of US floor/hearing sentences under strong-only rules is the missing piece. Building it is Paul's call.
3. **Stratified replication of Ramey's C criterion.** The majority/minority split that carries Ramey's effect has not been run. The current null is UNSTRATIFIED.
4. **Discriminant validity against party.** Every criterion in the theme (expert extremes, observer ratings, CHES-tracking communion) is entangled with party. No work, and no recorded system test, reports trait convergence with party partialled out.
5. **Venue and channel transfer.** Pennebaker's evidence is from students. Ramey defends floor speech only. Rubio's features break on a microphone change, and Paul's audio report shows filler rates differ about 100x across venues. No work models venue as a method factor for politicians.
6. **Conversion to graph objects (Paul's stated gap).** No work specifies how a trait or persona score becomes a node attribute or an edge parameter. Paul's pathfinding note adds his own constraint that observable OCEAN detail is limited (in his words, 10 personalities reduce to 4 observable ones).
7. **Ambiguity and disambiguation (Paul).** Birkenmaier gives no procedure for ambiguous statements, and Pennebaker notes word counts miss irony and context.
8. **Reading gaps inside the theme.**
   - Only the Mondak online appendix was read. The main article (scale anchors, aggregation rule, era) is missing.
   - Works cited as calibration anchors are not in this theme: koutsoumpis-2024 (year unresolved), azucar-2017, byrne-2014 (the source of the N-under-pressure premise), celli-nd (the later review), and the Mairesse 2007 source models.
   - The Ramey slug year (2016) conflicts with the PSRM 2019 citation.
   - Rubio's internal number conflicts (N = 100 vs 122; Discussion vs Results correlations) are unresolved.
9. **Human-gold audio.** Paul's status report lists the 150-clip human-gold WER packet, the hearings_seq.db rebuild that fixes the 9.7% (65,912 rows) cross-chamber mis-attribution, and two speech test suites that pass vacuously, as open as of 2026-08-29. Check them against brain STATE before relying on them.
10. **Bearing-tag hygiene.** The Soto entry's summary says all bearings are UNVERIFIED (DE-062), but its tags read VERIFIED-SOFTENED. Celli's tags are all UNVERIFIED. Those are single-agent leads, not findings.

## All works

- [[birkenmaier-2025-measuring-politicians-public]] (2025) — Measuring Politicians' Public Personality Traits Using Computational Text Analysis: A Multimethod Feasibility Study for Agency and Communion
- [[ramey-2016-measuring-elite-personality]] (2016) — Measuring Elite Personality Using Speech
- [[pennebaker-2003-psychological-aspects-natural]] (2003) — Psychological Aspects of Natural Language Use: Our Words, Our Selves
- [[lechner-2025-mapping-political-personality]] (2025) — Mapping the Political Personality (PoliPers): An Integrative Framework for Assessing Ideal and Actual Personality Profiles in Politicians (version 0.1)
- [[mondak-nd-online-appendix-personality]] () — Online Appendix: Personality on the Hill: Expert Evaluations of U.S. Senators' Psychological Traits
- [[rubio-2024-feasibility-data-analytics]] (2024) — Feasibility of Big Data Analytics to Assess Personality Based on Voice Analysis
- [[soto-2017-next-five-inventory]] (2017) — The Next Big Five Inventory (BFI-2): Developing and Assessing a Hierarchical Model with 15 Facets to Enhance Bandwidth, Fidelity, and Predictive Power
- [[celli-2012-role-emotional-stability]] (2012) — The Role of Emotional Stability in Twitter Conversations
