---
type: paper
title: "Politics of Feelings: Emotional Expression and Legislative Effectiveness in the U.S. Congress"
authors: ["Segun Aroyehun"]
year: 2026
venue: "arXiv preprint arXiv:2609.10198 (v1, 9 Sep 2026); author affiliation University of Konstanz"
doi: ""
category: "Machine Learning & Sentiment Analysis"
systems: [melange-legislative-data, melange-psychometrics, melange-stance]
components: ["melange-legislative-data.effectiveness-shadow", "melange-psychometrics.affective-n", "melange-stance.lsd-tone"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/aroyehun-2026-politics-of-feelings.pdf]]"
original_paths: []
sha12: ["cc88a043bcb2"]
tags: [paper, machine-learning-sentiment-analysis]
---

# Politics of Feelings: Emotional Expression and Legislative Effectiveness in the U.S. Congress

**Segun Aroyehun (2026).** *arXiv preprint arXiv:2609.10198 (v1, 9 Sep 2026); author affiliation University of Konstanz.* Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Aroyehun (arXiv 2609.10198, Sep 2026) scores 1,766,113 Congressional Record floor speeches (93rd-118th Congresses, 1973-2024) for eight discrete emotions with a transformer multilabel classifier (Widmann and Wich 2023), z-standardized over the corpus. Four questions: trends (Mann-Kendall over 26 Congress means: nearly every emotion rises in both parties, read as intensification rather than a negative turn), policy domains (all eight differ across 21 CAP topics, but epsilon squared only 0.013-0.034), member correlates (mixed models: majority members show less anger, -0.035; senators less anger, -0.041; women more enthusiasm, +0.020; ideology score more anger, +0.022; marginal R2 0.003-0.071), and legislative effectiveness. On the Volden-Wiseman LES (member-Congress, n = 13,671), enthusiasm (+0.016) and pride (+0.011) are positive, anger (-0.037) is the only negative, and emotional diversity (+0.026 to +0.031) and valence (+0.020) beat intensity (-0.014). Marginal R2 moves only from about 0.113 to 0.119, so emotion adds little beyond the controls. The design is associational: no identification strategy, reverse causation is not addressed, validation is 50 extreme-scored speeches per emotion in-domain only, the classifier was built on German political text, and the paper has internal inconsistencies (random intercepts vs fixed effects, table labels). For Paul's systems it matters in three places. It uses the same CEL LES that fec/members/lep_criterion.py pulls as an external criterion. It shows speech-emotion features vary with chamber, majority status and gender, which the affective-N slot controls only partly. And its valence vs intensity split matches the two LSD fields already computed in Draft Systems. Nothing here justifies adding an emotion feature to rank. All coefficients are UNVERIFIED in this project (DE-062).

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** RQ1: how have discrete emotions in U.S. congressional speeches evolved over time, and how do Democrats and Republicans differ? RQ2: how do discrete emotions vary across CAP policy domains? RQ3: how does legislators emotional expression vary with demographic and socio-political characteristics? RQ4: do discrete emotions and broader emotional characteristics (valence, intensity, diversity) explain variation in legislative effectiveness?

**Theory:** Functional appraisal accounts of emotion (Keltner and Gross 1999); emotions shape attention and persuasion in politics (Marcus 2000); emotion as social information regulating cooperation (Van Kleef 2009); emodiversity and adaptive functioning (Quoidbach et al 2014); stance and appraisal theory (Du Bois 2007; Martin and White 2005). Emotions are treated as communicative resources; the mechanism is not tested.

**Data:** 1,766,113 Congressional Record floor speeches, 93rd to 118th Congresses (1973 to 2024), House and Senate (Sec 3.1). Duplicates, speeches under 10 words and procedural or presiding-officer speeches removed (zero-shot Llama 3). Unit: speech for emotion scores; legislator-Congress for LES (n = 13,671, Tables 7-9). Trend analysis: 26 Congress-level means per party. Each session at least 20,000 speeches (Fig 5). Topics: 21 CAP major topics, classifier macro F1 0.857 (Sec 3.2.4). LES: Volden and Wiseman, 1973 to 2024.

**Method:** Transformer multilabel classifier (Widmann and Wich 2023) gives continuous scores for eight emotions per speech, z-standardized over the corpus (Sec 3.2.1). RQ1: Mann-Kendall with Sen slope, Benjamini-Hochberg FDR (Table 3). RQ2: CAP topic means and Kruskal-Wallis across 21 topics (Table 4). RQ3: linear mixed-effects per emotion, legislator random intercept, Congress fixed effects, topic controls (Tables 5-6). RQ4: mixed-effects models of member-Congress LES on each emotion or on valence, intensity, diversity (Tables 7-9).

**Identification:** None stated. The design is associational: the paper estimates whether emotion scores co-vary with LES and does not present an instrument, natural experiment, panel design or sensitivity analysis. Congress fixed effects and topic controls absorb shared time shifts and policy domain only. Reverse direction (effective members changing how they speak) and confounding by leadership or majority status are not separately addressed beyond the included controls.

**Scope:** U.S. Congress floor speeches, 1973 to 2024 (93rd to 118th). Population: members of the House and Senate who speak on the floor. Not state or supranational legislatures; authors name these as future tests. Measures apply to speech text, not to behavior beyond LES.

## Key findings

- RQ1 trends (Sec 4.1, Table 3): Mann-Kendall finds significant increasing trends in mean emotion scores for nearly all emotions in both parties across 26 Congresses. Exception: Democratic joy (tau 0.243, FDR p 0.086). Democratic enthusiasm Sen slope 0.030 per Congress.
- Parallel paths (Sec 4.1, Sec 5): Democrats and Republicans follow similar trajectories for most emotions. Authors read the rise as broad intensification of emotional expression, not a shift toward negative rhetoric, since positive and negative emotions both rise.
- Anger is volatile (Sec 4.1, Fig 1): Democratic anger peaks in the early 2000s and around 2017; Republican anger is higher in 2021 to 2024. Authors link this volatility to prevailing political context.
- Policy domains (RQ2, Sec 4.2, Table 4, Fig 2): Kruskal-Wallis differences significant for all eight emotions across 21 CAP topics, with small effect sizes (epsilon squared 0.013 to 0.034). International Affairs has highest mean fear; Immigration highest mean anger; Education highest pride with elevated joy and enthusiasm; Macroeconomics pairs high anger with low joy and pride.
- Demographic and party correlates (RQ3, Sec 4.3, Tables 5-6): female legislators higher enthusiasm (0.020), pride (0.012), joy (0.007), sadness (0.006) and fear (0.004) in standardized units. Senators: lower anger (-0.041) and enthusiasm (-0.007), higher pride (0.009) and joy (0.008). Ideology score: higher anger (0.022), lower joy (-0.004), enthusiasm (-0.002) and pride (-0.006). Marginal R2 ranges 0.003 to 0.071.
- Majority party (Tables 5-6): majority members express more joy (0.003), enthusiasm (0.006) and pride (0.009), and less anger (-0.035) and fear (-0.001). Republican coefficients are small, and Republicans express more joy (0.005) and pride (0.005) but less anger (-0.012).
- LES, discrete emotions (RQ4, Sec 4.4, Figure 4, Tables 7-8, n = 13,671): enthusiasm 0.016 (95 pct CI 0.007 to 0.025, p under 0.001); pride 0.011 (CI 0.003 to 0.019, p under 0.01); anger -0.037 (CI -0.046 to -0.029, p under 0.001) is the only significant negative. Fear, disgust, sadness, joy and hope are not significant.
- Broader characteristics (Sec 4.4, Table 9): emotional diversity 0.026 (CI 0.017 to 0.034) and 0.031 (CI 0.024 to 0.039) across two specifications; valence 0.020 (CI 0.011 to 0.028); intensity -0.014 (CI -0.025 to -0.004, p under 0.01). Marginal R2 is 0.118 to 0.119 in Table 9 versus 0.113 to 0.118 for discrete-emotion models; differences are small.
- Classifier validation (Sec 3.2.1, App B, Table 1): in-domain AUC on 50 hand-labeled speeches per emotion (25 highest, 25 lowest scored): anger, joy and hope 1.000; sadness 0.989; enthusiasm 0.982; pride 0.981; fear 0.952; disgust 0.922. No out-of-domain test is reported.
- Face validity (Sec 3.2.1, App B, Fig 6, Table 2): top-scoring texts read as plausible exemplars of each emotion (pride as acknowledging colleagues, anger as criticism). Negative emotions correlate weakly and positively with each other; cross-valence correlations are weak.
- Corpus scale (Sec 3.1, Fig 5): 1,766,113 speeches; each session at least 20,000; z-standardization preserves each emotion distribution (Sec 3.2.1). Given this N, very small coefficients (0.001 to 0.02 SD) reach p under 0.001, so significance here says little about substantive size.
- Interpretation limit (Sec 5): authors call emotions consequential and say expression shapes persuasion, coordination and relationships, but no mechanism, causes or targets of emotion are tested; authors list identifying causes and targets as future work.

## Limitations

- Stated by authors (Sec 5 Limitations): findings are for the U.S. Congress only; generalization to state legislatures or the European Parliament is open.
- Stated by authors: causes and targets of emotional expression are not identified; CAP topics are the only context control.
- Stated by authors: the eight emotions are a selective set; curiosity, guilt, shame and nostalgia are not examined.
- Observed in text, not stated as a limitation: validation uses 50 speeches per emotion, in-domain only; annotator identity and protocol are not described.
- Observed in text: the classifier source (Widmann and Wich 2023) is titled as a study of German political text; applicability to English Congressional Record speech is not tested here.
- Observed in text: no identification strategy; LES models are associational and reverse causation is not addressed.
- Observed in text: internal inconsistencies. Sec 3.3 says random intercepts for legislators, while Table 5-6 notes say session and topic fixed effects; Sec 4.4 wording for the Table 9 specifications does not match the table column labels; the Table 7 caption reads Tables 7, 7, and 9.
- Observed in text: ideology score source and direction are not described; the effect size scale for LES coefficients is not defined beyond the regression output.

## Short quotes

- "congressional speeches are becoming emotionally expressive over time"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-legislative-data#Five-stage legislative effectiveness shadow and CEL criterion\|Five-stage legislative effectiveness shadow and CEL criterion]] | qualifies | On member-Congress LES (n = 13,671; Tables 7-9, all models with session and topic fixed effects): anger -0.037 (95% CI -0.046 to -0.029, p < 0.001) is the only significant negative emotion term; enthusiasm +0.016 (CI 0.007 to 0.025) and pride +0.011 (CI 0.003 to 0.019, p < 0.01) are positive; fear, disgust, sadness, joy and hope are not significant. Marginal R2 is 0.113 to 0.118 in Tables 7-8 and 0.118 to 0.119 in Table 9. The paper reports no controls-only model, so an incremental R2 for emotion terms is not stated. (Sec 4.4, Figure 4, Tables 7-9 (text labels read 'Tables 7, 7, and 9')) | lep_criterion.py loads CEL House and Senate LES 93rd-118th and scores it against the blended score (bar B: rho >= +0.30 convergent). Its docstring says LES rewards majority and chair status. The claim that the five-stage shadow is not activated was not checked (outside the lines read). (`fec/members/lep_criterion.py:2-22 (confirmed)`) | Transfers as context only: speech tone is a small associational correlate of LES once structural controls are in, so a shadow-vs-LES gap should not be read as a rhetoric effect. No identification, so not usable as a predictor. Quote coefficients as UNSTRATIFIED with window 93rd-118th Congresses and n = 13,671 member-Congresses; the paper pools over majority-flip fault lines. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Affective neuroticism slot and N genre-transfer tests\|Affective neuroticism slot and N genre-transfer tests]] | qualifies | Mixed models of speech-level emotion (Tables 5-6; n = 1,766,113 speeches; session and topic fixed effects): senators express less anger (-0.041, CI -0.043 to -0.039) and more pride (+0.009) and joy (+0.008); women express more fear (+0.004) and sadness (+0.006); majority members express less anger (-0.035); ideology score predicts more anger (+0.022). Mann-Kendall trends (Table 3, 26 Congresses): 15 of 16 party-by-emotion series increase significantly; Democratic joy does not (tau 0.243, FDR p 0.086). (Sec 4.1 and Table 3; Sec 4.3 and Tables 5-6) | affective_n is the mean of LLM affective facets (pessimism, anxiety, depression, volatility), residualized on E and then on a Republican dummy only; it is not residualized on chamber, gender or ideology. The LIWC-style anchor is a hand-written regex approximation (FPS, NEG, POS patterns), not the LIWC dictionary. The interview-to-floor transfer result (rho .050) was not located in this file. (`fec/members/emit_affective_n.py:36-56 (confirmed)`) | Holds only weakly. Within one Congress majority status is nearly collinear with party, so the party residual may cover it. Chamber and gender are not covered and are a plausible structural confound. The paper's coefficients are mostly 0.001 to 0.04 in standardized units. The paper's facets are fear and sadness on floor text, not the interview facets used here. | weak | VERIFIED-SOFTENED |
| [[systems/melange-stance#Lexicoder LSD2015 tone intensity\|Lexicoder LSD2015 tone intensity]] | supports | In LES models (Table 9): valence (positive minus negative emotion) +0.020 (CI 0.011 to 0.028) and intensity -0.014 (CI -0.025 to -0.004, p < 0.01). The paper reads the rise in emotion since 1973 as intensification with both positive and negative emotions rising, not as a negative turn. In the paper, intensity is the sum of the eight z-scored emotion scores. (Sec 4.1, Sec 4.4 and Table 9, Sec 5) | stage5_lsd.R emits lsd_net (pos minus neg), lsd_net_norm (net divided by words) and lsd_charge ((pos plus neg) divided by words) per text. lsd_charge is a normalized dictionary-count magnitude, not a sum of classifier scores. (`C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_lsd.R:45-60 (confirmed; lsd_charge at line 59)`) | Supports keeping direction and intensity as separate fields rather than collapsing them. Weak transfer: floor speech vs press releases, dictionary vs transformer, and LES outcome vs CU stance or behavior. It does not show whether sign times intensity predicts behavior. Do not treat lsd_charge as the same construct as the paper's intensity; they are analogous only. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: 'The classifier was built on German political text' is inferred from the cited title of Widmann and Wich 2023; the paper does not state the training language. Say the cited source is a German-text study.
> - Summary issue: 'Marginal R2 moves only from about 0.113 to 0.119, so emotion adds little beyond the controls' is not supported: no controls-only model is reported, and the 0.113 to 0.119 range comes from models that all include emotion terms.
> - Summary issue: 'Its valence vs intensity split matches the two LSD fields' is overstated; see bearing 3. Analogous, not the same construct.
> - Summary issue: 'It uses the same CEL LES that lep_criterion.py pulls' is plausible (code reads CEL 93-118) but equivalence to the paper's Volden-Wiseman file was not checked at data level.
> - Summary issue: 'Interview-to-floor genre transfer already FAILED (rho .050)' is not in emit_affective_n.py and was not verified elsewhere.
> - Summary issue: Effect-size ranges are inconsistent: the summary-level text says 0.001 to 0.02 SD in one place and 0.004 to 0.04 SD in another; Tables 5-6 absolute coefficients run 0.000 to 0.041.
> - Summary issue: 'Paul's recorded position on the effectiveness shadow' (paul_alignment) is unverified: the item has no paul_entries and the brain was not read. Drop it or verify it before use.
> - Summary issue: Summary's 'nearly every emotion rises in both parties' is correct (15 of 16 series); the one exception, Democratic joy, should be named where the trend claim is repeated.
> - Summary issue: Summary's note on internal inconsistencies (random intercepts in Sec 3.3 vs fixed-effects table notes; Table 9 text vs column labels) is correct.

## Gaps and flags

- affective_n residualizes on E and party only (emit_affective_n.py:52-56). Chamber and gender, which the paper finds shift floor-speech emotion, are not controlled. Is a House/Senate offset present in affective_n? This is an untested diagnostic.
- The melange-stance.lsd-tone component card says intensity = |net|/words, but stage5_lsd.R:59 computes lsd_charge = (pos+neg)/words. The card is stale relative to the code.
- The system has no floor-speech discrete-emotion layer. If one is ever proposed, the paper's measures cannot be lifted directly: validation is 50 extreme speeches per emotion, in-domain only, and the classifier is German-trained.
- lep_criterion bar B compares LES with the blended score but does not report how much of LES is structural (majority, chair). The paper's R2 of about 0.11-0.12 with full controls is a reminder that a weak bar-B rho may reflect LES composition rather than a score fault.
- Associational only. Coefficients are pooled over 1973-2024 and over majority-flip fault lines, and LES models are not stratified by chamber or majority. Quote them only as UNSTRATIFIED, with window 93rd-118th Congresses and denominator n = 13,671 member-Congresses.
- Influence language: the paper calls emotion consequential for persuasion and coordination without testing any mechanism. Do not carry that wording into Melange surfaces.
- With 1.77M speeches, effects of 0.001-0.02 SD reach p < 0.001. Per the project's trend-over-significance rule, read size and consistency, not p.
- The paper has internal inconsistencies: Sec 3.3 says legislator random intercepts while the table notes say fixed effects, and the Sec 4.4 wording does not match the Table 9 columns. The ideology score's source and direction are undisclosed.
- Nothing here concerns the rejected SNA proposals, LDA edges or reciprocity. No conflict with D-021, D-025 or D-104..D-108.
- All figures from this work are UNVERIFIED in this project until a second path reproduces them (DE-062).

Paul has not annotated this work (no paul_entries). His recorded position on the effectiveness shadow is to highlight LES as the criterion. The paper fits that use: rhetoric adds little to LES once majority, chair and seniority are controlled, as the lep_criterion docstring already notes. For affective-N, this is a newer emotion source, but it uses floor speech, the genre where interview-derived N already failed to transfer, so it is not evidence for the slot. The LSD card describes intensity differently from the code and is stale.

## Related papers

- [[rheault-2016-measuring-emotion-parliamentary]] — cites — Cited by this paper for emotion in parliamentary debates. The item uses lexicons (dictionaries), not the transformer classifier used here, so it is an earlier lexicon approach rather than a same-method link.
- [[osnabrugge-2021-playing-gallery-emotive]] — complements — Cited for emotive rhetoric in parliaments. The item uses a dictionary-based measure. This paper mentions strategic and communicative functions of emotion in its limitations but does not test them.
- [[dietrich-2019-pitch-perfect-vocal]] — complements — Not cited by this paper. The item measures emotional intensity of congressional floor speech from vocal pitch, an acoustic counterpart to the text intensity measure here.
- [[ramey-2016-measuring-elite-personality]] — complements — Not cited by this paper. The item infers member traits from speech and links them to sponsorship behavior. The same-data label was not confirmed, so the relation was changed to complements.
- [[bucchianeri-2025-legislative-effectiveness-american]] — complements — Not cited by this paper. This paper names subnational legislatures as a future test (Limitations); the item is an LES study of American state legislatures.
- [[harbridge-yong-2023-bipartisan-path-effective]] — same-data — Not cited by this paper. The item uses the Volden-Wiseman effectiveness measure, the same outcome this paper uses, and offers a bipartisanship account that an emotion account would need to be separated from.
- [[battaglini-2018-effectiveness-connected-legislators]] — same-data — Not cited by this paper. The item uses sponsorship networks with the Volden-Wiseman effectiveness outcome, a competing non-rhetorical correlate that this paper does not control.
- [[rice-2018-corpus-based-dictionaries]] — complements — Not cited by this paper. The item builds corpus-based sentiment dictionaries for specialized vocabularies, a dictionary alternative relevant to the LSD-tone vs classifier contrast.

### Linked from
- [[decter-frain-2016-impressive-words-linguistic]] — complements — Emotional expression in congressional speech linked to legislative effectiveness at member level; this paper links language to chamber-level approval.
- [[hargrave-2022-longer-conforming-stereotypes]] — complements — Item exists (title confirmed: emotional expression and legislative effectiveness in US Congress). Not cited by Hargrave; analyst judgement link on the US venue.
- [[montgomery-2017-effects-congressional-staff]] — same-method — Vault item exists; uses Volden-Wiseman LES as outcome, but the same underlying data was not verified, so same-data is downgraded.