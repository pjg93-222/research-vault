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
paper_count: 26
tags: [system]
---

# Melange member psychometrics (Big Five, dispositions, persona)

Measuring legislator personality and behavioural dispositions from text and behaviour. Covers the lexical Big Five pipeline (Ramey-style, with SMOreg and RKH calibration), the LLM item-level reader, the ridge hybrid, the CT-CM latent model, behavioural dispositions, cooperation_v2, affective N, the agency/communion persona channel, shrinkage rules, and the genre-transfer tests. These feed the receptivity layer.

**Brain notes:** [topics/2026-07-feature-personality-psychometrics](obsidian://open?vault=brain&file=topics%2F2026-07-feature-personality-psychometrics); [topics/2026-07-feature-personality-psychometrics-external-criteria-and-open-a-search](obsidian://open?vault=brain&file=topics%2F2026-07-feature-personality-psychometrics-external-criteria-and-open-a-search); [concepts/single-look-holdout-discipline-case-1-moneyball-gate2](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline-case-1-moneyball-gate2); [concepts/single-look-holdout-discipline-cases-2-3-n-slot-and-gold-template](obsidian://open?vault=brain&file=concepts%2Fsingle-look-holdout-discipline-cases-2-3-n-slot-and-gold-template)

**Melange member psychometrics: evidence assessment (hub snapshot, 70 bearings over 8 components)**

The bearings are spread unevenly. Lexical Big Five has 20, gold-panel validation 12, affective N 12, persona 11, LLM/hybrid readers 8 and behavioural dispositions 6. Cooperation v2 has 1, a weak context row. Shrinkage and weighting has 0, so it is a GAP. Most rows are VERIFIED-SOFTENED: the paper side checked out, but the analyst's framing was trimmed. Only 9 rows are fully VERIFIED, and 11 are UNVERIFIED. Under DE-062 the UNVERIFIED rows count as leads only.

The literature says three things clearly:

1. **Closed-vocabulary text is weak against self-report and fairly strong against observer ratings.** For self-report, |rho| is about .08 to .16 and mean R² about 5% (Koutsoumpis 2024: k=25 samples, n≈84k, about 87% of it from one study; Pennebaker 2003: several hundred students). Observer cue utilization reaches mean R² about 38.5% (k=5, n=1,637, almost all zero-acquaintance raters). So Melange's traits are best read as measures of perceived public persona, not inner disposition. That is consistent with how the receptivity layer uses them. It also means convergence with the Senate panels shows that the text and the raters use shared cues; it does not show trait accuracy.
2. **None of the strong evidence comes from Congress.** Nearly every effect size comes from students, social-media users, German party sentences or MPs in other countries. The only congressional sources are Ramey 2016 (House, with no ground-truth validation) and the Mondak appendix (Senate, historic, era not stated).
3. **Several figures in the system's own records could not be located by the verifiers.** These are the held-out CV against RKH (E .510 / A .406 / C .363 / ES .258 / O .214, mean .350), the 0.12 Pink bar, the .21 text-tone halo, the genre-transfer rhos (.050 / .161) and "O reversed on party check". The committed artifact data/members/big5_v2.json instead reports held-out r against behavioural targets: A .059, C .281, E .563, O .135, and no ES value. Until these numbers are found, provenance is a bigger problem than any paper finding.

The verified findings that most affect the system:

- The Mondak appendix excludes senators rated by only one coder (k=1). The primary validators do not apply that filter (VERIFIED, strong).
- Floor-vote participation shifts within the same legislator with electoral status, falling 2.70 points from a mean of 90.48 in the final term (Fouirnaies & Hall 2022, US state legislatures; VERIFIED). This affects both diligence_z and the recorded C PASS.
- The agency facet is not unidimensional in PoliPers observer ratings (alpha .40 for Trump/Merkel pooled, N=211; VERIFIED).
- Among MPs in Belgium, Canada and Israel (n=306), more neurotic members appear less in national news (beta -.111; VERIFIED). By analogy, interview-derived N may be range-restricted.

## Lexical Big Five (Ramey-style closed vocabulary)

**What it computes:** Corpus-relative Big Five z (O, C, E, A, ES; N = -ES) from category rates of floor speech and interviews.

**Method:** Rates per 1000 words (LIWC-analog, Empath, MATTR, Brysbaert concreteness) are z-scored and combined with linear weights from published correlates or SMOreg (Mairesse) fit on essays, then refit to RKH targets per trait (hybrid). Shrinkage z x sqrt(tokens/1500). Held-out CV vs RKH: E .510, A .406, C .363, ES .258, O .214 (mean .350). O was reversed on the party check.

**Code refs:** `fec/members/big5_traits.py:1-286`, `fec/members/fit_smoreg_rkh.py`, `fec/members/fit_smoreg_essays.py`, `fec/liwc/mairesse_smoreg.json`

**Assumptions**
- Closed-vocabulary rates carry trait signal
- RKH estimates are a valid reference (method-shared, so convergence is inflated)
- Scores are meaningful only within this corpus

**Evidence**

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

**Assessment**

**Evidence weight: moderate on the general method, weak on validity in Congress.** There are 20 bearings: 1 VERIFIED, 14 VERIFIED-SOFTENED and 5 UNVERIFIED leads (Rammstedt, Simchon, Naz, Kumari, Celli 2012).

**What the evidence supports**
- The procedure follows Ramey et al. 2016: corpus-standardized features, scores relative to the corpus, and N = -ES (VERIFIED-SOFTENED, strong as a method source).
- Several SMOreg weight signs match Koutsoumpis's bivariate cue-validity signs: A anger, A swear, C anger, C swear and O sixltr.

**What limits it**
1. **Ceiling.** Closed-vocabulary categories explain about 5% of self-report variance. Koutsoumpis's meta-analysis (k=25 self-report samples, n≈84k, about 87% of n from Schwartz 2013) gives |rho| at most .08 to .14. Pennebaker and King give r .10 to .16 (several hundred students). Koutsoumpis also shows that formal registers weaken these markers, and floor speech is formal and partly staff-written. Simchon 2024 (UNVERIFIED) adds that openness-styled text can be produced on demand, so prepared speech may reflect communications strategy rather than the author.
2. **Circular reference.** RKH is itself built on Mairesse-type SVR (Birkenmaier §2.5). Birkenmaier calls Ramey's pretrained SVR outdated and not validated against human labels. Refitting to RKH therefore turns convergence with RKH into a fit statistic, not validation (Ramey and Pennebaker bearings).
3. **Precision floor.** Ramey's bootstrap CI width of about 0.3 assumes at least about 5,000 words per member-year (median about 11,000). The system's 1,500-token lowconf floor is far below that band, so Ramey's precision claim does not carry over.
4. **Benchmarks do not compare.** The SotA figure of about r .30 (Celli), Azucar's pooled r .29 to .40 (16 effect sizes per trait, I² 93 to 97%, best-model selection; UNSTRATIFIED across 4 platforms, 2012-2017) and Naz's MBTI accuracies all use lay self-report criteria. None of them benchmarks an RKH or panel figure.

**Contradictions inside the system record**
- The hub describes the active weights as refit to RKH. The Celli bearing says fit_smoreg_rkh.py produces candidate weights, promoted only on REFIT WINS. The Mondak bearing says the v2 builder (big5_lexcal_v2.py) learns weights against behavioural criteria and treats RKH and Mairesse weights as v1 priors. Which weights are live is therefore unresolved.
- The held-out CV figures (.350 mean, E .510) were not found by six separate verifiers.
- Three Koutsoumpis sign disagreements remain UNVERIFIED (DE-062) and are audit leads only: ES first-person singular +.0094, ES negemo about 0, E posemo -.0339. Multivariate suppression can flip signs legitimately.

**Contradictions**
- Hub says the active weights are refit to RKH. Celli bearing: the RKH refit is a candidate promoted only on REFIT WINS. Mondak bearing: the v2 builder learns weights against behavioural criteria, with RKH and Mairesse as v1 priors.
- Held-out CV against RKH (E .510, A .406, C .363, ES .258, O .214, mean .350) was not found in the fec tree. big5_v2.json reports held-out r against behavioural targets instead (A .059, C .281, E .563, O .135).
- Birkenmaier calls Ramey's pretrained Mairesse SVR outdated and not validated against human labels, yet RKH, built on it, is the system's refit target.
- Ramey's precision band (CI width about 0.3 at 5,000 words or more per member-year) is set against the system's 1,500-token lowconf floor.
- Koutsoumpis signs disagree with three SMOreg weights (ES first-person singular, ES negemo, E posemo). These are UNVERIFIED leads and may be suppression effects.

**Gaps**
- No congressional study links text-derived traits to a non-text criterion, other than the Senate panels (handled in gold-panel-validation).
- No measurement-invariance test across genres (floor, interview, hearing, social). Rammstedt's analogy is UNVERIFIED and supplies no numbers.
- Equivalence between the system's exemplar word lists and licensed LIWC categories is not tested.
- No staff-authorship control: prepared and staff-written text is not separated from extemporaneous speech.

**Paul's position vs the evidence**

Paul calls Ramey 'dated but foundational' and asks for frameworks built for each venue. The evidence supports both halves. Ramey is the procedural backbone (VERIFIED-SOFTENED, strong), but it had no ground-truth validation for legislators. Its SVR is the part Birkenmaier criticizes, and the system inherits that weakness by calibrating to RKH. The call for venue-specific frameworks is backed by Koutsoumpis's evidence on formality and heterogeneity (significant in 96% of cue-validity effects), by the failed genre transfer, and by Lechner's finding that contextualized measures beat generic ones. Lechner's evidence is about survey items, not text, so a venue-specific text reader would still need its own validation. Peng et al. 2022 is cited but has no bearing row, so its contribution cannot be judged here.

**Open questions**
- Which weight file is live today: published-correlate weights, the Mairesse SMOreg json, the RKH refit or the v2 behavioural fit?
- Where did the .350 held-out CV against RKH come from, and can it be reproduced from a committed artifact?
- Do the ES and E sign disagreements with Koutsoumpis survive when weights are fit univariately, or are they suppression?
- What share of each member's floor text is prepared or staff-written, and does lexical O, the weakest trait, track that share?

**Read first:** [[koutsoumpis-2024-kernel-truth-text]], [[ramey-2016-measuring-elite-personality]], [[birkenmaier-2025-measuring-politicians-public]], [[pennebaker-2003-psychological-aspects-natural]], [[celli-nd-twenty-years-personality]], [[mondak-nd-online-appendix-personality]]

## Big Five validation against Senate gold panels and criteria

**What it computes:** Pass or miss of trait validity looks. E and A pass both panels; O passes academics only; N passes insiders only; C has an insider association only.

**Method:** Pre-registered looks: interview C vs votes cast rho +0.155 PASS; C vs follow-through +0.112 PASS. Several improvement rounds MISSed. A-ES about .44 is population-real (RKH benchmark) plus about .21 text-tone halo. Status labels must accompany any external claim.

**Code refs:** `fec/big5_improvements_eval.py`, `fec/b5_residual_eval.py`, `fec/b5_stylometry_eval.py`

**Assumptions**
- Gold panels are independent ratings
- k=1 insider ratings are noisy

**Evidence**

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

**Assessment**

**Evidence weight: moderate, and it mainly qualifies the validation rather than supporting it.** There are 12 bearings: 2 VERIFIED, 10 VERIFIED-SOFTENED, 0 UNVERIFIED.

**The gold source checks out, but the validators diverge from it**
- **Gold artifact.** The Mondak appendix (VERIFIED-SOFTENED, strong) confirms 129 insider ratings and 126 academic name entries on a 1-7 scale. The frozen artifact has 125 academic rows. The malformed academic Feingold row is dropped silently by an except branch whose comment is wrong.
- **k=1 rule (VERIFIED, strong).** The main paper excludes senators rated by a single coder. The primary validators (b5_gold_multitrait_validate.py, b5_external_gold_validate.py) do not filter on ratings_n. Only the side probes stratify k=1 against k≥2. How many current-roster matches are k=1 was not counted. A k≥2 re-score would be a new look, not a free correction.
- **Party fault line.** At the extremes the panels are party-skewed: Democrats are 62% and 65% of the insider top and bottom scorers, and 80% of academic high scores go to Democrats and 80% of low scores to Republicans. Neither primary validator has a party term. Panel correlations must therefore carry party strata or an UNSTRATIFIED label. The idea that O passes academics only because of party is an UNVERIFIED lead (DE-062).
- **Panels no longer untouched.** Full-sample gold-fitted weights flow into b5_profile_basis.py, so only the out-of-fold figures are clean. Shipped composites are partly fit to the panels.

**What the panels can and cannot show.** Koutsoumpis and Celli place the panels on the Perception side of the Brunswik lens. Machine perception r is about .30 against human .38 (Hinds & Joinson 2024; settings not comparable). Zero-acquaintance observers predict E and A best and ES worst, which matches the recorded pass pattern (E and A pass both panels). So the pattern is what shared observable cues would produce, and it is not evidence of trait accuracy. Lechner 2025 shows that observer ratings track the rater's ideological proximity (r up to .48), but that evidence comes from crowdworkers rating two leaders, not experts rating senators.

**Criterion side.** Fouirnaies & Hall (VERIFIED) show that the C PASS against votes_cast (rho +.155) is cross-sectional and mixes trait with electoral situation. Quote it as UNSTRATIFIED on electoral status, and do not re-run it (single-look).

**Brain ruling at odds with the paper.** The 2026-07-04 ruling that set aside the single-rater persona gold test (n=30) said Birkenmaier's coders reached only about α .72 on an easier task. In the paper, .72 to .73 is the cue-presence alpha, agency and communion run .70 to .78, and the paper never calls its task easier. The decision may still stand, but its stated rationale misreads the source.

**Contradictions**
- Mondak's main paper excludes k=1-rated senators. The primary validators keep them (VERIFIED).
- Academic panel: the artifact has 125 rows against 126 entries in the source text. The Feingold academic row is dropped silently, and the code comment says only headings reach that branch.
- The 2026-07-04 brain ruling's rationale (coders only about α .72 on an easier task) misstates Birkenmaier: .72 is the presence alpha, agency and communion are .70 to .78, and the paper never calls its task easier.
- The hub component says 'O was reversed on the party check'. The ONC candidate file marks O NOT_LOOKED_CRITERION_STOP_BAR_MISS, and no reversal result was found.
- Hub claims of an A-ES of about .44 plus about .21 halo: .443 is the RKH anchor, and the .21 halo attribution was not located.
- Gold panels are described as untouched external criteria, but full-sample gold-fitted weights are emitted into b5_profile_basis.

**Gaps**
- Ratings exist for senators only, so nothing validates House members, about 80% of the roster.
- Panel era is not stated, so whether ratings match the text window is unknown.
- No study separates rater ideology from target trait in expert ratings of US legislators.
- No non-rating criterion exists for O or ES.

**Paul's position vs the evidence**

Paul highlighted Birkenmaier's validation framework: codebook, Krippendorff α .70 to .78, and macro-F1 below .80 for every computational model. That framework measures sentence-level cue agreement among three trained coders. It does not validate member-level traits, so it cannot stand in for the panel looks. His notes do not reflect the system's single-look MISS record, and the evidence makes that record more important. The panel PASSes for E and A are what shared public cues predict. The un-applied k=1 rule and the missing party term mean the PASS and MISS pattern should be quoted with strata or an UNSTRATIFIED label. His instinct toward codebook-style reliability is sound, but it applies to the persona channel more than to the Big Five panel looks.

**Open questions**
- How many current-roster gold matches are k=1 insider-only? Does Paul want a new pre-registered k≥2 look?
- Should panel correlations be reported with D and R strata from now on, as a reporting change rather than a new look?
- Is the 2026-07-04 persona-gold set-aside to be re-justified on corrected grounds, or reopened? That is Paul's ruling.
- Should shipped composites stop using full-sample gold-fitted weights so the panels stay a clean holdout?

**Read first:** [[mondak-nd-online-appendix-personality]], [[koutsoumpis-2024-kernel-truth-text]], [[celli-nd-twenty-years-personality]], [[fouirnaies-2022-electoral-incentives-affect]], [[birkenmaier-2025-measuring-politicians-public]], [[lechner-2025-mapping-political-personality]]

## LLM item-level reader (Pink), ridge hybrid and CT-CM latent model

**What it computes:** De-identified, chunked LLM ratings on 20 items; ridge hybrid on lexical, interaction and persona features; CT-CM SEM with an LLM method factor.

**Method:** The LLM reader removes names before scoring and codes 'cannot tell' as 0. It is display-only. Pink x behavioural diagonal mean +0.064 < 0.12 fails. CT-CM is a one-look capstone.

**Code refs:** `fec/members/big5_llm.py`, `fec/members/big5_hybrid.py`, `fec/members/big5_ctcm.py`

**Assumptions**
- De-identification removes reputational priors (Cao & Kosinski)
- Judge halo is a single method factor

**Evidence**

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

**Assessment**

**Evidence weight: weak to moderate, and it does not support moving the readers off display-only.** There are 8 bearings: 0 VERIFIED, 6 VERIFIED-SOFTENED, 2 UNVERIFIED (Rammstedt, Naz).

**Pink item-level reader**
- The nearest benchmark is Peters & Matz 2024 via Celli. Zero-shot GPT-3.5 and GPT-4 reach average r .29 against self-report on users' own Facebook posts. That is parity with specialized machine learning, not superiority, and LLM outputs show stereotyped profiles (high A, high O, low N).
- On de-identified, staff-mediated political text a lower figure is expected. The recorded diagonal of +0.064 against behavioural targets is consistent with that. The 0.12 bar itself was not located in the brain.
- Birkenmaier validates a narrow sentence-cue task, so its success does not carry over to item-level Big Five rating.
- **Keying.** Soto 2017 (VERIFIED-SOFTENED) shows that acquiescence forms its own component in raw item responses. Contrary to the docstring's "balanced keying", big5_llm.py keys O, C, E and A at 3:1 and ES at 2:2. An all-yes rater would add +0.5 to O, C, E and A and 0 to ES. Whether the LLM has a yes-tendency on observer items is untested. A uniform yes-tendency would not by itself create the A-ES correlation.
- **De-identification.** Lechner 2025 shows that human observer ratings track the rater's ideological proximity (r .36 to .48). That supports de-identification by analogy only; LLM reputational priors are inferred, not shown.

**Ridge hybrid.** The committed big5_v3_hybrid.json shows no held-out gain over lexical-only on any target: A .056 vs .059, C .273 vs .281, E .564 vs .563, O .130 vs .135. Azucar's multi-footprint gains (lay users, self-report) do not carry over to interaction-channel features. The docstring notes that interaction features are degraded by nickname matching until re-ingest, so these numbers are a floor, not a final verdict.

**CT-CM.** No bearing tests it. The method-factor rationale is inferred from the human-rater halo literature only.

**Contradictions**
- big5_llm.py docstring line 7 says balanced keying. The ITEMS table keys O, C, E and A 3:1 and ES 2:2.
- The 0.12 pass bar cited for the Pink diagonal was not located in the brain; only +0.064 was.
- The hybrid was designed to add signal over lexical-only, but its committed artifact shows no held-out gain on any of the four targets.

**Gaps**
- No study validates de-identified, item-level LLM observer ratings of politicians against expert panels. This is the comparison Pink needs.
- No test of LLM acquiescence or yes-tendency on observer-keyed items applied to political text.
- No measurement of how much reputational prior survives de-identification for well-known members, for example a name-recovery probe.
- CT-CM method-factor adequacy has no external benchmark.

**Paul's position vs the evidence**

Paul endorses LLM-based trait reading on the grounds that GPT-4o and DeepSeek-V3 were the only models validated. The evidence narrows that endorsement. Birkenmaier validated those two models on German sentence-level agency and communion cue classification (macro-F1 .76 and .77), not on Big Five item ratings. The best LLM Big Five evidence (Peters & Matz via Celli) shows parity with older methods (r about .29), not an advantage. The system's own Pink reader fails its behavioural bar, and the persona channel runs on claude-sonnet-5, which Birkenmaier never tested. The endorsement fits the cue-classification design. It does not yet fit trait reading.

**Open questions**
- Does the LLM rater show a yes-tendency on observer items? A content-free control chunk would test this cheaply.
- Should the keying be rebalanced? Under itemset versioning that would be a new cache key and a new look.
- Is the nickname-degraded interaction channel re-ingested yet, so the hybrid null can be read as final?

**Read first:** [[birkenmaier-2025-measuring-politicians-public]], [[celli-nd-twenty-years-personality]], [[soto-2017-next-five-inventory]], [[lechner-2025-mapping-political-personality]], [[azucar-2017-predicting-personality-traits]]

## Behavioural dispositions

**What it computes:** Cooperation, diligence (roll-call participation, LEP), dominance (turn statistics, leadership), visibility (interviews), breadth (portfolio) and reactivity (interview cutoff).

**Method:** z-score composites with coverage floors; missing values are neutral.

**Code refs:** `fec/members/behavioral_dispositions.py:1-120`, `fec/members/interaction_features.py`

**Assumptions**
- Observed behaviour indicates disposition
- Turn-share statistics index dominance (Rienks & Heylen; Valente)

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[fouirnaies-2022-electoral-incentives-affect]] | 2022 | qualifies | Floor-vote participation is state-dependent within the same legislator. It falls 2.70 points (SE 0.53) from a mean of 90.48 in the final term, about a 27% proportional rise in abstention. Committee activity falls about 8% (-0.31 from 3.84). | Moderate. Missing votes is a universal congressional behaviour, and the paper's within-person identification is cleaner than anything the system has. However, the state final-term drop of about 3 points is small next to the cross-member spread in congressional votes_cast. It matters most for members in their final Congress, or those campaigning for another office, whose absences are campaign-driven. The disposition reading holds only after electoral status is held fixed. | moderate | VERIFIED |
| [[ramey-2016-measuring-elite-personality]] | 2016 | contradicts | The 2016 paper's only Conscientiousness outcome is the ceremonial-bill share (Table 3). It has no roll-call participation analysis; roll calls appear only in passing as a source of ideal points (p.5). | The 2016 text does not support the attribution. Whether another RKH work (the 2015 manuscripts cited at pp.15 and 19) uses participation cannot be checked from this text. Treat as a citation fix, not a modelling finding. | moderate | VERIFIED-SOFTENED |
| [[azucar-2017-predicting-personality-traits]] | 2017 | context | Social-media activity statistics (posts, friends, network density, likes received, comments, tags) gave a marginal increase in E accuracy (beta .19, R2 .18, p=.06). The paper reports no other significant moderator effects; it does not report a tested null for each trait. | The constructs differ. The paper's activity variables are social-media posting, friend counts and network density; the system's variable counts press or broadcast interviews. Any link is an analogy, not support, and it bears on E only, not on the lexical E it was meant to contrast with. The analyst's point that interview counts track press-office scheduling and seniority is their inference and was not checked against the repo. | weak | VERIFIED-SOFTENED |
| [[amsalem-2018-effect-politicians-personality]] | 2018 | qualifies | Extraversion is positive and significant in the pooled OLS: b = 0.129 (SE 0.052), beta .117, p < .05 (Model 1, n = 306, R2 = .483, country dummies and controls). Also significant on news websites (Model 3, b = 0.117, p < .05). | The extraversion direction holds in the paper, but the effect is small (beta .117). The paper's outcome is earned mentions by journalists, not interviews a member gives. The sample is MPs in Belgium, Canada and Israel, 2011-2015, with self-reported TIPI. Office is the dominant predictor there, so a US interview count needs office controls before it reads as temperament. Any claim that visibility_z double-counts dominance_z is an untested inference, not a paper finding. | moderate | VERIFIED-SOFTENED |
| [[salam-2022-learning-personalised-models]] | 2022 | context | Among generic single modalities, talk-turn features without sentiment (duration, content, speech rate) average AMSE 0.874 across the five traits, audio 0.920, and FaceBody 1.325 (Table 1, p.65). The paper does not isolate extraversion or dominance and does not test broadcast interviews. | Indirect, five-trait-averaged evidence that turn structure carries trait signal in dyadic lab tasks. It does not validate dominance_z or its committee term, and says nothing about congressional floor speech. Any use should name the window and denominator. | weak | VERIFIED-SOFTENED |
| [[yang-2023-getting-personal-deep]] | 2023 | suggests-extension | The same GBDT and RFR models are run with and without personality features. Average MSE improvement: GBDT 14.29 percent (CH 20.91, SG&A 31.74, ROA 12.52), RFR 6.67 percent. GBDT gains from CNN-1 features 3.38, CNN-2 minus 1.80, BERT 8.62. Gains for all eight DVs are significant by Wilcoxon, GBDT at p below 0.01. Observations per DV range 405 to 519 (Table 10). A chronological split is reported in Online Appendix G. | Borrow the design only: the same learner with and without a trait block, a held-out chronological split, and paired tests, applied to compare text traits against dispositions on a pre-registered responsiveness outcome. The lift magnitudes come from executives and firm accounting outcomes with reverse causality the authors leave unresolved, so they must not be quoted as evidence that text traits predict member behaviour. | moderate | VERIFIED |

**Assessment**

**Evidence weight: moderate, and the strongest bearing is a confound warning.** There are 6 bearings: 2 VERIFIED, 4 VERIFIED-SOFTENED.

**Diligence_z**
- Fouirnaies & Hall 2022 (VERIFIED, moderate) use within-legislator identification in US state legislatures. Floor-vote participation falls 2.70 points (SE .53, mean 90.48) in the final term, and committee activity falls about 8%. The transfer to Congress is moderate: the drop is small next to the cross-member spread, but it is concentrated in retiring members and those running for other office.
- Diligence_z enters susceptibility at -0.20. A retiring member therefore gets lower diligence and higher susceptibility, while the incentive term gives them 0.60. Electoral status enters the pressure term twice, with opposite signs, and is never modelled explicitly. The disposition reading holds only after electoral status is held fixed.
- **Citation error.** The behavioral_dispositions.py docstring calls roll-call participation RKH's strongest C criterion. Ramey 2016's only C outcome is the ceremonial-bill share, and participation is not analysed (VERIFIED-SOFTENED, contradicts). This is a citation fix, not a modelling finding.

**Visibility_z.** Amsalem 2018 (VERIFIED-SOFTENED) finds extraversion predicts media presence among MPs in Belgium, Canada and Israel 2011-2015 (n=306, beta .117). The effect is small, and office is the dominant predictor. The outcome there is earned journalist mentions, not interviews given. A US interview count needs office and leadership controls before it reads as temperament. Azucar's social-media activity moderator (E only, marginal p=.06) is an analogy only.

**Dominance_z.** Salam 2022 is five-trait-averaged lab evidence that turn structure carries trait signal. It does not validate floor turn statistics or the committee-leadership term.

**Comparison design.** Yang 2023 offers a design to borrow, not effect sizes: the same learner with and without the trait block, a chronological split and paired tests. That is the right shape for the prospective H1/H2 comparison of text traits against dispositions. Its lift magnitudes come from executives and firm outcomes and must not be quoted.

**Contradictions**
- The behavioral_dispositions.py docstring attributes roll-call participation as RKH's strongest C criterion. Ramey 2016 uses only the ceremonial-bill share and does not analyse participation.
- Electoral status enters pressure twice with opposite signs (incentive +0.60, and through diligence_z -0.20 in susceptibility) and is never modelled explicitly.

**Gaps**
- No congressional within-member study of how stable votes_cast, LEP follow-through or turn statistics are across Congresses. This is needed to call them dispositions rather than states.
- No validation of turn-share dominance on legislative floor or hearing speech.
- No test that interview counts reflect temperament rather than press-office scheduling, seniority or office.

**Paul's position vs the evidence**

Paul's position is indirect support: effort allocation is observable, and conscientiousness relates to sponsorship. On observability the evidence agrees, and Fouirnaies's within-person design is cleaner than anything the system has. But the same paper shows that the observed effort is partly situational, so labelling it a disposition needs an electoral-status stratum. On conscientiousness and sponsorship, Ramey's C link is to ceremonial-bill share, not to participation. The system's docstring overstates what Ramey showed.

**Open questions**
- Should diligence_z be computed excluding final-Congress and running-for-other-office member-Congresses, or carry an electoral-status stratum? Any such change is Paul's call.
- Is visibility_z still informative after committee-leadership and office controls?
- Is the H1/H2 prospective comparison pre-registered with a chronological split?

**Read first:** [[fouirnaies-2022-electoral-incentives-affect]], [[ramey-2016-measuring-elite-personality]], [[amsalem-2018-effect-politicians-personality]], [[yang-2023-getting-personal-deep]], [[salam-2022-learning-personalised-models]]

## Cooperation v2 (share-based bridging)

**What it computes:** Mean of within-chamber z of cross-party tie share and directional cross-party cosponsorship share (at least 10 cosponsorships). Reciprocity is reported only.

**Method:** Volume-free by design. Promoted 2026-07-10. Correlates rho .888 with bpf.

**Code refs:** `fec/members/coop_v2_candidate.py:1-130`

**Assumptions**
- Cross-party share measures cooperation independent of volume

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | supports | Average between-party and within-party pairwise distances (Figs 7-8, 1879-1998) show polarization declining to WWII, stable until the late 1970s, and rising steadily since. The paper does not state that the most extreme members sit farthest from the opposing party; that is an analyst inference. | The paper supplies context for why an extremity control matters in a polarized chamber; it does not test the cooperation measure. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: effectively a GAP.** There is one bearing, VERIFIED-SOFTENED, weak, and it is context only. Poole 1999 shows that polarization has risen since the late 1970s, which explains why an extremity control matters. The system records cooperation_v2_z against |dim1| at r = -0.250 (n = 532 of 533 members, 2026-07-10). No bearing tests the measure itself.

The component's two cited sources, Bacik et al. 2026 and Aref & Neal 2021, have no bearing rows, so their support cannot be judged here.

**Design checks**
- The measure is a member-level share: the cross-party share of a member's ties and cosponsorships, with a floor of 10 or more cosponsorships. It is not per-edge volume normalization of graph edges, a proposal that was measured and REJECTED in this project. Keep the two distinct, and do not extend cooperation_v2 into edge-weight normalization.
- Reciprocity is reported only. That is consistent with reciprocity NOT being validated (D-104..D-108).
- The recorded rho .888 with bpf raises a redundancy question that no paper addresses.

**Contradictions**
- None recorded.

**Gaps**
- Trait stability: a test-retest study of cross-party cosponsorship share across successive Congresses for the same members (for example an ICC over 2 or more Congresses), with chamber and majority-status strata.
- Convergent validity: correlate it with an independent bipartisanship measure, such as an externally published bipartisan cosponsorship index, within the same window and denominator.
- Discriminant validity: separate cooperation from ideological moderateness (the recorded r = -.250 with |dim1|) and from district partisanship.
- Redundancy: incremental validity over bpf given rho .888.
- Mechanism: whether share-based bridging reflects disposition or committee and caucus assignment.

**Paul's position vs the evidence**

No annotation of Paul's addresses this component, and the evidence base is nearly empty. The design choice (volume-free share, reciprocity reported only) is consistent with project rulings but has no external empirical support or contradiction in the hub.

**Open questions**
- Do the Bacik et al. 2026 and Aref & Neal 2021 citations actually bear on member-level cooperation share, or only on network structure?
- Is cooperation_v2 distinct enough from bpf (rho .888) to justify a separate slot?

**Read first:** [[poole-1999-nominate-after-years]]

## Affective neuroticism slot and N genre-transfer tests

**What it computes:** affective_n = z(LIWC i + negemo - posemo) + z(LLM pessimism, anxiety, depression, volatility), residualized on E and party.

**Method:** Gate 5/5 passed: |r(N,E)| .039; party .000; LIWC-LLM convergence +.236; split-half SB .742. Promoted 2026-07-11. Genre transfer of interview N to floor and hearing text FAILED (rho .050, .161).

**Code refs:** `fec/members/emit_affective_n.py`, `fec/members/affective_n_validate.py`

**Assumptions**
- Negative-emotion and first-person-singular words track N (Pennebaker)
- No gold N criterion exists, so the gate tests construct integrity only

**Evidence**

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

**Assessment**

**Evidence weight: weak.** The bearings support direction only. None gives a criterion, and one VERIFIED bearing raises a selection problem. There are 12 bearings: 2 VERIFIED, 8 VERIFIED-SOFTENED, 2 UNVERIFIED (Naz, Celli 2012).

**Direction.** Pennebaker 2003 and Koutsoumpis 2024 both link N or low ES to more negative emotion (cue validity rho -.12) and to first-person singular (observer cue utilization rho -.29, k=5). Both use students and lay users, and Koutsoumpis notes that the negemo effect weakens with publication year. Marrero 2022 finds linguistic features beat vocal ones for N in students (about 92 words per sample). Its top N predictors are not the system's anchor terms. No study tests N in politicians' speech.

**What is actually emitted.** Per emit_affective_n.py and affective_n_validate.py, affective_n is now the E-residualized, party-controlled sum of four LLM facets: pessimism, anxiety, depression and volatility. The LIWC base was removed as a component and kept as a convergent anchor at r +.24; the code comment cites one member moving from rank 1 to 412. Rice & Zorn 2018 supports dropping a closed-vocabulary component outside its home register, as a general point only. Both the hub component description and the module docstring still say the measure combines LIWC and LLM. They are stale.

**Mechanism.** Byrne 2014/2015 (VERIFIED) is the reason for having the slot, but it never measured anxiety, mood or working memory, and its pressure manipulation check was null. The project's H5 test of N x loss domain (partial r +.036, n=441) is therefore neither confirmed nor refuted by Byrne. The N contribution stays UNVERIFIED (DE-062).

**Selection.** Amsalem 2018 (VERIFIED) finds that, net of office and activity, more neurotic MPs in Belgium, Canada and Israel appear less in national news (beta -.111, n=306). If US interview coverage selects the same way, interview-derived N is range-restricted. That is untested.

**Facets.** Soto 2017 finds that Anxiety and Volatility predict different criteria than Depression. The pressure mechanism fits Anxiety and Volatility better than the pessimism and depression facets, so summing all four may dilute the slot. Changing that would need its own prereg.

**Party residualization.** Ramey's ES loads .20 on Common Space (N=844), so controlling for party may remove real ES signal. The brain records raw r -0.11 against RKH-ES (n=147), falling to -0.04 (not significant) once E is removed.

No gold N criterion exists, and Naz (UNVERIFIED) confirms the field has none ready-made. The 5/5 gate therefore tests construct integrity only, as the system already says.

**Contradictions**
- The hub component description and the emit_affective_n.py docstring (lines 3-4) say affective_n combines z(LIWC) and z(LLM). The code emits LLM facets only, with LIWC as a convergent anchor.
- The receptivity layer labels the N slot as Byrne's channel, but Byrne never measured the anxiety or working-memory mechanism, and the system's own H5 test is null (partial r +.036, n=441).
- The genre-transfer failure figures (rho .050, .161) were not located in the cited files by any verifier.

**Gaps**
- Criterion study: an observable yielding-under-pressure outcome, for example position change after targeted pressure campaigns, scored prospectively for high and low affective-N members with party and electoral-status strata.
- Selection study: whether interview availability in the US correlates with non-text reactivity proxies after leadership and office controls (the test Amsalem implies).
- A facet-separated look (Anxiety and Volatility versus Depression and pessimism) under prereg.
- An invariance study of affective N across interview, floor and hearing genres, to establish why transfer failed.

**Paul's position vs the evidence**

No annotation of Paul's addresses this component. The evidence supports keeping the slot as a theoretically motivated, UNVERIFIED contributor and does not support any validity claim. The strongest new information is Amsalem's selection finding and Soto's facet distinction. Both are leads for a pre-registered look, not changes.

**Open questions**
- Should party residualization stay, given Ramey's ES-ideology loading?
- Is the four-facet sum the intended construct, or should the slot track Anxiety and Volatility only (new prereg)?
- Where are the genre-transfer rho values recorded?

**Read first:** [[koutsoumpis-2024-kernel-truth-text]], [[amsalem-2018-effect-politicians-personality]], [[byrne-2014-chokes-under-pressure]], [[soto-2017-next-five-inventory]], [[pennebaker-2003-psychological-aspects-natural]], [[rice-2018-corpus-based-dictionaries]]

## Agency-communion persona channel

**What it computes:** persona_z = 0.60 z(communion) - 0.40 z(agency) from LLM few-shot cue classification (strong 1.0, weak 0.5), blended at PERSONA_W 0.30.

**Method:** Functional gate 18/18. Party signature: communion D .099 vs R .049. Persona homophily is not earned (|r| <= 0.015).

**Code refs:** `fec/members/agency_communion_llm.py`, `fec/persona_homophily_validate.py`

**Assumptions**
- Projected persona is what a lobbyist meets
- Communal rhetoric signals approachability

**Evidence**

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

**Assessment**

**Evidence weight: moderate for the construct and method; weak to none for the susceptibility signs.** There are 11 bearings: 3 VERIFIED, 5 VERIFIED-SOFTENED, 3 UNVERIFIED (Palm, Selfhout, Celli 2012).

**Method source.** Birkenmaier & Lechner 2025 (VERIFIED-SOFTENED, strong) is a faithful method source. The system ports its codebook, the positive-pole rule and the strong/weak cue split. Lechner 2025 PoliPers supplies the facet definitions.

The validated performance does not carry over:
- Birkenmaier's macro-F1 of .76 to .77 belongs to GPT-4o and DeepSeek-V3 on German sentences scored against three trained coders.
- The system runs claude-sonnet-5 on English congressional sentences and has no human-labelled sentence test, so its macro-F1 is unknown.
- The 18/18 functional gate was written by the builder; the paper's Appendix J was not available.

Ornstein 2024 says prompt-tuned LLM coding must be checked against hand labels the prompt was not tuned on.

**Strong versus weak cues.** Birkenmaier finds strong-only definitions perform slightly better (.77 vs .76, inside the SDs). A strong-only score can be computed from the existing cache at no API cost.

**Agency is not one thing.** Lechner (VERIFIED) finds the Assertiveness facet splits. The "power-hungry, claims leadership" item loads reversed (Study 3, N=589), and observer alpha is .40 (Study 4, N=211). The system's self-test uses a power-claiming sentence as a gold agency unit. This is a plausibility warning only, because the split was measured in citizen trait ratings, not in sentences.

**Party.** Birkenmaier (VERIFIED) shows communion share falling from left to right across six German parties. The system's D .099 against R .049 matches that, which supports convergent validity. It also means persona carries party, so pooled use is UNSTRATIFIED across D and R.

**Signs in the susceptibility blend (.60 communion, -.40 agency)**
- Byrne (VERIFIED, weak) concerns self-reported agreeableness, not communal rhetoric. Citing it for the persona direction is a second inferential step the paper does not cover.
- Palm (UNVERIFIED lead) gives mild support for the communion sign. It gives none for the negative agency sign, and may run against it, since extraversion is above average in susceptibility.

PERSONA_W 0.30 is an owner ruling and is not at issue. These points concern provenance and signs inside the channel.

**Homophily.** Selfhout 2010 and Celli 2012 (both UNVERIFIED) do not overturn the measured null (|r| ≤ .015). They are not grounds to re-propose homophily edge weighting. The null holds for the cross-sectional test that was run.

**Contradictions**
- The design follows Birkenmaier, but the two validated models (GPT-4o, DeepSeek-V3) are not the model in use (claude-sonnet-5), and no system-side macro-F1 exists.
- The agency self-test treats a power-claiming sentence as gold agency. PoliPers shows power-claiming loads reversed against leadership-taking in observer ratings (VERIFIED).
- The communion→susceptibility direction is attributed to Byrne, but Byrne studied self-reported A, not projected rhetoric.
- The negative agency sign has no support. An UNVERIFIED lead (Palm) points the other way for extraversion-like presentation.

**Gaps**
- A human-labelled sentence sample of English congressional statements, coded by at least 2 trained coders and held out from codebook writing, to give the system a macro-F1.
- A behavioural criterion for the persona→susceptibility link: whether communal-persona members accept more outside requests or meetings, or cosponsor more with outside requesters, with D and R strata.
- A text-level test of whether power-claiming and leadership-taking agency cues split.
- Strong-only against strong-plus-weak member ordering (computable from cache).

**Paul's position vs the evidence**

Paul sees this as the most direct transfer from his notes (pre-built codebooks and node design), and the evidence agrees on the codebook design. His two named weaknesses are both borne out:

- **Trait-to-node conversion.** No paper supports the susceptibility signs, especially the negative agency sign.
- **Ambiguous statements.** These correspond to the weak-cue category and the Assertiveness facet split.

What his notes lack is the gap between Birkenmaier's validated models and the model actually in use, and the absence of any human-labelled test on congressional text.

**Open questions**
- Does Paul want a held-out human-coded sentence sample to estimate the system's macro-F1?
- Should agency be split into leadership-taking and power-claiming cues for a pre-registered look?
- Does member ordering change under strong-only scoring?

**Read first:** [[birkenmaier-2025-measuring-politicians-public]], [[lechner-2025-mapping-political-personality]], [[ornstein-2024-train-your-stochastic]], [[byrne-2014-chokes-under-pressure]], [[selfhout-2010-emerging-late-adolescent]]

## Interview weighting (C_INT_W) and gated shrinkage (P6)

**What it computes:** Conscientiousness aggregates interview chunks at 2x. Members with fewer than 8 measurements are shrunk.

**Method:** The P6 reliability claim (.748 -> .831) was retracted as double-shrink inflation; the change was kept on external criterion gains.

**Code refs:** `big5_llm.py (C_INT_W)`, `fec/big5_improvements_eval.py --p6`

**Assumptions**
- Shrinkage is judged only on external criteria, not split-half reliability

**Evidence**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| _none_ | | | | | | |

**Assessment**

**GAP: no bearings.** No paper in the hub addresses interview up-weighting of C (C_INT_W = 2x) or gated shrinkage of members with fewer than 8 measurements (P6). The P6 reliability claim (.748 to .831) was retracted as double-shrink inflation. The change was kept on external-criterion gains, which is the correct standard, but no literature backs either constant.

The nearest adjacent evidence comes from other components:
- Ramey 2016's bootstrap shows CI width of about 0.3 only at about 5,000 or more words per member-year, so precision degrades sharply below that.
- Pennebaker 2003 reports retest reliability of .24 for psychological-process word categories in student speech, which implies heavy noise at small samples.

Neither tests a shrinkage rule or a genre weight.

**Contradictions**
- None recorded.

**Gaps**
- Length-reliability curve: subsample long-corpus members down to 500, 1,500, 3,000 and 5,000 tokens, many draws each, and measure rank agreement with the full-corpus score. This gives an empirical floor in place of the 1,500-token and 8-measurement thresholds.
- An empirical-Bayes shrinkage comparison judged only on a held-out external criterion (pre-registered, single look), with token count as the precision input.
- Genre-weight study for C_INT_W: estimate the interview-versus-floor weight by multitrait-multimethod analysis or criterion-weighted regression, not by fiat.
- A check that shrinkage toward 0 does not interact with party or seniority (thin-corpus members may be systematically junior).

**Paul's position vs the evidence**

Paul's position is 'No literature', and the hub confirms it. The constants rest on internal criterion gains only.

**Open questions**
- Which external criterion was used to keep P6 after the reliability retraction, and over what window and n?
- Is C_INT_W = 2 a ruled constant or a candidate open to a weighting study?

**Read first:** [[ramey-2016-measuring-elite-personality]]

## Across components

**Five patterns across the components**

**1. Measurement loops.** Several validity checks reuse the same method or data on both sides:
- RKH is Mairesse-SVR based, and the lexical reader is refit to it.
- The Senate panels serve as external gold and also supply full-sample weights to shipped composites.
- The affective-N gate checks LIWC-family terms against LLM facets.
- Celli 2012 is the literature's own example of this circularity.

Each loop makes convergence a fit statistic. Only out-of-fold or prospective criterion results count as validation.

**2. Text measures perceived persona, not self.** Koutsoumpis, Pennebaker, Celli (Brunswik lens) and Rubio agree that text traits converge with observers far better than with self-report. That fits the receptivity use case, where what matters is what an outside requester meets. All trait wording should say perceived public persona, and panel PASSes should be described as shared-cue agreement.

**3. Party is the dominant fault line in every component.** It shows up in the extreme-rating skew of the gold panels, in the persona D/R gap (.099 vs .049), in affective-N residualization (which may strip real ES signal, given Ramey's .20 Common Space loading), and in cooperation's link to ideological extremity (r -.250). Pooled coefficients in any of these should carry D and R strata or an UNSTRATIFIED label.

**4. Electoral status is an unmodelled confound** shared by diligence_z, the C-vs-votes_cast PASS and the incentive term (Fouirnaies & Hall, VERIFIED). It enters pressure twice with opposite signs.

**5. Provenance debt outweighs paper debt.** Several numbers quoted in hub component records could not be located: the .350 RKH CV, the .12 Pink bar, the .21 halo, the genre-transfer rhos, and "O reversed on party". Docstrings are also stale: the affective-N module docstring still says it combines LIWC, big5_llm.py claims "balanced keying", and behavioral_dispositions.py misattributes a C criterion to Ramey. Where numbers were checked, the committed artifacts (big5_v2.json, big5_v3_hybrid.json, big5_gold_anchored.json) tell a less flattering story than the hub text. Fix the records before citing any of these externally.

**Guardrails**
- No bearing argues for dropping regular cosponsorship edges or for per-edge volume normalization.
- Cooperation_v2's member-level share must not be extended into edge normalization.
- The homophily-adjacent papers (Selfhout, Celli 2012) do not reopen the measured null.
- PERSONA_W 0.30 stays an owner ruling. Every bearing that touched it concerned provenance or signs only.

## Research agenda

1. Provenance sweep (no new looks): locate or retract the .350 held-out CV against RKH (E .510 to O .214), the 0.12 Pink bar, the .21 text-tone halo, the genre-transfer rhos (.050, .161) and 'O reversed on party'. Correct the stale docstrings in emit_affective_n.py, big5_llm.py and behavioral_dispositions.py. Establish which lexical weight file is live.
2. Gold-panel hygiene: count k=1 insider-only matches on the current roster, fix the silent Feingold academic-row drop, and report existing panel correlations with D/R strata or an UNSTRATIFIED label. Any k≥2 re-score is a new pre-registered look, and the decision is Paul's.
3. Stop full-sample gold-fitted weights from flowing into b5_profile_basis.py so the Senate panels remain a clean holdout. This is an owner ruling.
4. Electoral-status stratum for diligence_z and the C criterion: flag final-Congress and running-for-other-office member-Congresses (Fouirnaies & Hall). Quote the recorded C PASS (rho +.155) as UNSTRATIFIED on electoral status, and do not re-run it.
5. Persona channel: hand-code a held-out sample of English congressional sentences (2 or more coders, kept out of codebook writing) to estimate macro-F1 for claude-sonnet-5. Compute strong-only scores from cache. Test whether power-claiming and leadership-taking agency cues split.
6. Shrinkage GAP: build a length-reliability curve by subsampling long-corpus members (500 to 5,000 tokens), then a pre-registered external-criterion comparison of empirical-Bayes shrinkage against the current P6 rule and the C_INT_W = 2 weight.
7. Cooperation_v2 GAP: test-retest of cross-party share across Congresses (ICC), convergent check against an independent bipartisanship index, and incremental validity over bpf (rho .888). Keep it member-level and never per-edge.
8. Affective N: test interview-coverage selection (Amsalem's mechanism) against non-text reactivity proxies after leadership controls. Pre-register a facet-separated look (Anxiety and Volatility versus Depression and pessimism) and a prospective yielding-under-pressure criterion with party strata.
9. LLM reader: run an acquiescence probe (content-free chunks) on the 20 observer items. If a yes-tendency appears, version a rebalanced itemset under a new cache key.
10. Text traits against dispositions (H1/H2): adopt Yang 2023's design of the same learner with and without the trait block, a chronological split and paired tests, on a pre-registered responsiveness outcome. Do not quote Yang's effect sizes.
