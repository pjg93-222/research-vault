---
type: paper
title: "Large Language Models Can Infer Psychological Dispositions of Social Media Users"
authors:
  - "Heinrich Peters"
  - "Sandra Matz"
year: 2024
venue: "arXiv preprint arXiv:2309.08631v2 (cs.CL); authors at Columbia University"
doi: ""
category: "Machine Learning & Sentiment Analysis"
systems:
  - melange-psychometrics
components:
  - melange-psychometrics.llm-and-hybrid-readers
  - melange-psychometrics.llm-and-hybrid-readers
  - melange-psychometrics.gold-panel-validation
  - melange-psychometrics.affective-n
  - melange-psychometrics.shrinkage-and-weighting
paul_annotated: false
paul_markup_count: 0
evidence_status: verified
source: "[[_sources/peters-2024-language-models-infer.pdf]]"
original_paths:
sha12:
  - 02d78d450d8c
tags: [paper, machine-learning-sentiment-analysis]
---

# Large Language Models Can Infer Psychological Dispositions of Social Media Users

**Heinrich Peters, Sandra Matz (2024).** *arXiv preprint arXiv:2309.08631v2 (cs.CL); authors at Columbia University.* Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Peters and Matz test whether off-the-shelf chat models can read Big Five personality from ordinary social media text with no training. They took 1,000 MyPersonality Facebook users (2007-2012, mean age 24, 63 percent women) who had completed the 100-item IPIP and had at least 200 status updates. GPT-3.5 and GPT-4 (2023 snapshots) were asked zero-shot for a 1-5 score on each trait. Text went in 20-message chunks, chunk scores were averaged, and each call was repeated three times and averaged. Inferred scores correlate with self-report at a mean r of .29: .31 for GPT-4 and .27 for GPT-3.5, with a per-trait range of .22-.33. Supervised in-domain models reach about .37. Accuracy is mostly reached by about 20 messages. Both models shift levels: they underestimate Conscientiousness and Agreeableness and overestimate Neuroticism. Absolute errors are larger for men on C, A and N, and for older users under GPT-3.5. Within-group correlations do not differ, so the bias is in level, not rank order. In a subsample of 68 users rated by friends, LLM-observer agreement (about .27) is close to self-observer agreement (.30). For Conscientiousness the LLM agrees more with observers than with self-report. For Melange, the paper backs the design of the display-only LLM item reader (chunk, then average) and sets a realistic ceiling for zero-shot accuracy. It also warns that agreement with observer panels may reflect shared impressions rather than traits. And it gives a reason to check the live affective-N input, which is partly LLM-derived, for gender as well as party bias. The population, text genre and criterion are all far from congressional speech, so none of its numbers can stand in as validity evidence for Melange.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Can zero-shot GPT-3.5 and GPT-4 infer the Big Five personality traits of social media users from their Facebook status updates, and does inference accuracy vary across gender and age groups?

**Theory:** Zero-acquaintance judgment accuracy: people can judge strangers traits from behavioral traces (refs 9-13). Digital footprints predict traits when supervised models are trained on self-reports (refs 14-19). Authors propose LLMs may have a human-like ability to profile people from text without direct interaction. Bias explanations considered: training-data bias versus differences in online self-expression by group. Neither is tested.

**Data:** Sample of 1,000 adult MyPersonality Facebook app users, randomly subsampled (Sec 2.1). Mean age 24.2 (SD 8.8); 63.1 percent female. Eligible users completed the full 100-item IPIP and had at least 200 status updates; the most recent 200 were used. Unit is the user. Mean update length 17.10 words (SD 15.03). App active 2007-2012 (Sec 4.2). Self-report: IPIP 100-item Big Five. Third-person subsample: 68 users rated by Facebook friends on a 10-item IPIP (Sec 3.3). Models: gpt-3.5-turbo-0301 and gpt-4-0314 via API. IRB approval Columbia protocol AAAU8559.

**Method:** Zero-shot prompting with default system prompt 'You are a helpful assistant' and an inference prompt asking for 1-5 scores on each Big Five trait, numbers only (Sec 2.2). Text fed in chunks of 20 messages; chunk scores averaged. Each inference queried three times and averaged (inter-round r from .73 to .96). Analyses: Pearson correlations between inferred and self-reported scores with Bonferroni correction; mean comparisons across gender and a median-split age groups by t-tests on inferred scores and on absolute residuals; within-group correlations and within-group z-standardization to test whether bias is in level versus rank order (Sec 3.2); correlations by message volume from 20 to 200 messages (Sec 3.1); observer-rating comparison (Sec 3.3).

**Identification:** None causal. Zero-shot design means no training or test split; accuracy is a correlation with self-report. Group bias claims rest on mean differences and residual t-tests. Not controlled for demographics passed to the model, since none were provided.

**Scope:** Facebook users who took MyPersonality psychometric tests in 2007-2012, mostly young adults, with 63 percent women. Country and language are not stated. Models are 2023 GPT versions. Authors state the sample may not represent the wider population.

## Key findings

- Average Pearson r between inferred and self-reported traits: GPT-3.5 .27, GPT-4 .31 (Sec 3.1). Abstract reports average .29, range .22-.33 across both models.
- GPT-4 per-trait r: Openness .33, Extraversion .32, Agreeableness .32, Neuroticism .29, Conscientiousness .26. GPT-3.5: Openness .28, Extraversion .29, Agreeableness .30, Neuroticism .26, Conscientiousness .22 (Sec 3.1).
- All correlations significant at p below .001 after Bonferroni correction. Differences between GPT-3.5 and GPT-4 not significant for any single trait (Sec 3.1).
- Accuracy rises with message volume; most of the maximum is reached by about 20 messages. Conscientiousness levels off earlier (Sec 3.1, Fig 2).
- Score distributions: both models underestimate Conscientiousness and Agreeableness and overestimate Neuroticism. GPT-3.5 underestimates Openness and Extraversion; GPT-4 overestimates Extraversion (Sec 3.1, Fig 1).
- Gender: inferred Openness, Conscientiousness, Extraversion and Agreeableness are higher for women in both models (Agreeableness t=13.63 for GPT-4). No significant inferred Neuroticism difference (Sec 3.2.1, Fig 3).
- Gender accuracy: absolute residuals are larger for men on Conscientiousness, Agreeableness and Neuroticism in both models (Agreeableness t=-9.22 for GPT-3.5). Within-gender correlations do not differ significantly (Sec 3.2.1, Fig 4).
- Age: inferred Conscientiousness and Agreeableness are higher and Neuroticism lower for older users in both models. Openness and Extraversion are lower for older users only in GPT-3.5 (Sec 3.2.2, Fig 3).
- Age accuracy: GPT-3.5 residuals are larger for older users on Openness (t=4.78) and Conscientiousness (t=2.64). GPT-4 shows no residual age difference. Within-age correlations do not differ (Sec 3.2.2, Fig 4).
- Observer comparison (n=68): self-report versus observer mean r .304; LLM versus observer mean r .269 for GPT-3.5 and .276 for GPT-4. Authors call LLM accuracy on par with human observers (Sec 3.3).
- For Conscientiousness, LLM-observer agreement exceeds the agreement of either LLM or observers with self-reports (Sec 3.3, Sec 4.1).
- Against supervised models: Park et al. report average r .37 (range .26-.41) with in-domain training; zero-shot LLM average .29 is slightly lower (Sec 4.1).

## Limitations

- B
- l
- a
- c
- k
-  
- b
- o
- x
- :
-  
- c
- u
- e
-  
- u
- t
- i
- l
- i
- z
- a
- t
- i
- o
- n
-  
- a
- n
- d
-  
- o
- r
- i
- g
- i
- n
-  
- o
- f
-  
- g
- e
- n
- d
- e
- r
-  
- a
- n
- d
-  
- a
- g
- e
-  
- b
- i
- a
- s
- e
- s
-  
- u
- n
- e
- x
- a
- m
- i
- n
- e
- d
-  
- (
- S
- e
- c
-  
- 4
- .
- 2
- )
- .
-  
- D
- a
- t
- a
-  
- f
- r
- o
- m
-  
- M
- y
- P
- e
- r
- s
- o
- n
- a
- l
- i
- t
- y
- ,
-  
- 2
- 0
- 0
- 7
- -
- 2
- 0
- 1
- 2
-  
- l
- a
- n
- g
- u
- a
- g
- e
-  
- m
- a
- y
-  
- d
- i
- f
- f
- e
- r
-  
- f
- r
- o
- m
-  
- c
- u
- r
- r
- e
- n
- t
-  
- t
- e
- x
- t
- ,
-  
- p
- o
- s
- s
- i
- b
- l
- y
-  
- u
- n
- d
- e
- r
- s
- t
- a
- t
- i
- n
- g
-  
- a
- c
- c
- u
- r
- a
- c
- y
- .
-  
- N
- o
- n
- -
- r
- e
- p
- r
- e
- s
- e
- n
- t
- a
- t
- i
- v
- e
-  
- s
- a
- m
- p
- l
- e
-  
- o
- f
-  
- c
- u
- r
- i
- o
- u
- s
-  
- u
- s
- e
- r
- s
- ;
-  
- O
- p
- e
- n
- n
- e
- s
- s
-  
- u
- n
- d
- e
- r
- e
- s
- t
- i
- m
- a
- t
- i
- o
- n
-  
- m
- a
- y
-  
- r
- e
- f
- l
- e
- c
- t
-  
- t
- h
- i
- s
- .
-  
- O
- n
- l
- y
-  
- t
- h
- e
-  
- 2
- 0
- 0
-  
- m
- o
- s
- t
-  
- r
- e
- c
- e
- n
- t
-  
- p
- o
- s
- t
- s
- ;
-  
- c
- h
- u
- n
- k
- i
- n
- g
-  
- d
- u
- e
-  
- t
- o
-  
- t
- o
- k
- e
- n
-  
- l
- i
- m
- i
- t
- s
- .
-  
- O
- n
- l
- y
-  
- z
- e
- r
- o
- -
- s
- h
- o
- t
-  
- p
- r
- o
- m
- p
- t
- i
- n
- g
-  
- w
- i
- t
- h
-  
- d
- e
- f
- a
- u
- l
- t
-  
- s
- e
- t
- t
- i
- n
- g
- s
- ;
-  
- n
- o
-  
- c
- h
- a
- i
- n
- -
- o
- f
- -
- t
- h
- o
- u
- g
- h
- t
-  
- o
- r
-  
- f
- i
- n
- e
- -
- t
- u
- n
- i
- n
- g
-  
- t
- e
- s
- t
- e
- d
- .
-  
- D
- e
- m
- o
- g
- r
- a
- p
- h
- i
- c
- s
-  
- n
- o
- t
-  
- s
- u
- p
- p
- l
- i
- e
- d
-  
- t
- o
-  
- m
- o
- d
- e
- l
- s
- ;
-  
- a
- u
- t
- h
- o
- r
- s
-  
- s
- u
- g
- g
- e
- s
- t
-  
- t
- h
- i
- s
-  
- c
- o
- u
- l
- d
-  
- h
- e
- l
- p
-  
- o
- r
-  
- a
- m
- p
- l
- i
- f
- y
-  
- b
- i
- a
- s
- .
-  
- O
- b
- s
- e
- r
- v
- e
- r
-  
- a
- n
- a
- l
- y
- s
- i
- s
-  
- i
- s
-  
- p
- r
- e
- l
- i
- m
- i
- n
- a
- r
- y
-  
- w
- i
- t
- h
-  
- n
- =
- 6
- 8
- .
-  
- L
- i
- v
- e
-  
- i
- n
- t
- e
- r
- a
- c
- t
- i
- o
- n
-  
- n
- o
- t
-  
- s
- t
- u
- d
- i
- e
- d
- .
-  
- P
- r
- i
- v
- a
- c
- y
-  
- a
- n
- d
-  
- m
- i
- s
- u
- s
- e
-  
- r
- e
- c
- o
- m
- m
- e
- n
- d
- a
- t
- i
- o
- n
- s
-  
- d
- e
- f
- e
- r
- r
- e
- d
-  
- t
- o
-  
- f
- u
- t
- u
- r
- e
-  
- r
- e
- s
- e
- a
- r
- c
- h
- .
-  
- S
- u
- p
- p
- l
- e
- m
- e
- n
- t
- a
- r
- y
-  
- s
- t
- a
- t
- i
- s
- t
- i
- c
- s
-  
- S
- 1
- -
- S
- 6
-  
- n
- o
- t
-  
- r
- e
- v
- i
- e
- w
- e
- d
- .

## Short quotes

- "a level of accuracy that is similar to that of supervised machine learning models specifically trained to infer personality"
- "The findings suggest that GPT's personality inferences are less accurate for men than women."
- "these biases seem to be limited to the absolute measures of accuracy"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-psychometrics#LLM item-level reader (Pink), ridge hybrid and CT-CM latent model\|LLM item-level reader (Pink), ridge hybrid and CT-CM latent model]] | qualifies | Zero-shot holistic 1-5 Big Five ratings by GPT-4 from the last 200 Facebook updates (20-message chunks, chunk means, three queries averaged) correlate with IPIP-100 self-report at mean r .31 for GPT-4 and .27 for GPT-3.5 (n = 1,000 users; pooled average .29; per-trait range .22-.33 across both models). Supervised in-domain models reach about .37 (Park et al., as cited in Sec 4.1). (Abstract; Sec 2.2; Sec 3.1; Sec 4.1) | big5_llm.py is an item-level reader: 20 IPIP-style observer-keyed items (4 per trait), 3-point responses (-1/0/+1), de-identified author text, default model Claude Sonnet (Opus and Haiku optional), chunks of 1,200 words with up to 6 chunks per genre, chunk scores averaged and then z-scored within the scored pool. The vault topic note records a diagonal mean over A, E and O of +0.064 for the Opus run (ES sign-flipped), below the 0.12 bar. `fec/members/big5_llm.py:3-18,51,84-94,206-221,354-361` | Holds only for informal first-person posts by ordinary young users scored against self-report. Member floor speech and interviews differ in genre and have no self-report criterion. The paper reports one accuracy level for one holistic prompt on one sample. It does not state a ceiling, so the figure is a reference point, not a bound. The item-level, de-identified design differs from the paper's holistic prompt. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#LLM item-level reader (Pink), ridge hybrid and CT-CM latent model\|LLM item-level reader (Pink), ridge hybrid and CT-CM latent model]] | suggests-extension | Each inference was queried three times and averaged. Between-round correlations ranged from .73 (GPT-4, Openness) to .96 (GPT-3.5, Agreeableness), so single-call noise is not negligible. (Sec 2.2) | The primary score uses rep 0, a single call per chunk. The --rep flag reruns the same chunks with fresh calls to compute ICC across reps; those reps are not averaged into the published score. `fec/members/big5_llm.py:40` | Applies if the item-level Claude ratings show between-rep variance like the paper's. The paper is a holistic prompt on GPT models, so the size does not transfer directly. Any change to published scores would be new agent analysis, UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED |
| [[systems/melange-psychometrics#Big Five validation against Senate gold panels and criteria\|Big Five validation against Senate gold panels and criteria]] | qualifies | In 68 users rated by Facebook friends on a 10-item IPIP, LLM-observer agreement averaged r .269 (GPT-3.5) and .276 (GPT-4), against .304 for self-report-observer agreement. For Conscientiousness, LLM-observer agreement exceeded the agreement of either the LLM or observers with self-report. The analysis is described as preliminary. (Sec 3.3; Sec 4.1) | Big Five validity is anchored to observer panels: the Personality on the Hill appendix, given in the vault gold-set note as 129 Senate insiders and 125 academics. The research note on that appendix counts about 126 academic rows from the text. The gold-set note names observer panels as the only anchor. `C:/Users/pgoff/Documents/brain/concepts/gold-set-validation.md:19; C:/Users/pgoff/Documents/research/papers/mondak-nd-online-appendix-personality.md` | If LLM readings track the same outward impressions that observers form, convergence with expert or insider panels may index shared reputation or presentation rather than the underlying trait. The evidence is preliminary (n = 68) and rests on friends rating peers, not experts rating senators. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Affective neuroticism slot and N genre-transfer tests\|Affective neuroticism slot and N genre-transfer tests]] | qualifies | Inferred Neuroticism correlates with self-report at r .29 for GPT-4 and .26 for GPT-3.5 (n = 1,000). Both models overestimate N. Absolute N residuals are larger for men (t = -4.55 GPT-3.5; t = -2.39, p = .017 GPT-4), while within-gender correlations do not differ significantly, so the bias is in level, not rank order. (Sec 3.1 and Fig 1; Sec 3.2.1 and Fig 4) | RECEPT_N defaults to affective. The N slot takes an LLM affective-facet measure (the sum of Haiku pessimism, anxiety, depression and volatility labels), E-residualized and party-controlled, and enters the trait susceptibility term at weight .30. Missing values are set to a neutral 0. `fec/members/receptivity_layer.py:74,179,186; fec/members/emit_affective_n.py:3-7,39-43` | The paper's pattern suggests within-pool z removes common level bias, but a demographic level offset would pass into a mixed-gender roster. Relevant only if the Haiku facet prompts on interview text share GPT's level bias by gender. The paper's N is an IPIP trait score on social media posts; the system input is a facet sum on interview text. | weak | VERIFIED |
| [[systems/melange-psychometrics#Interview weighting (C_INT_W) and gated shrinkage (P6)\|Interview weighting (C_INT_W) and gated shrinkage (P6)]] | context | Correlations rise with message volume from one 20-message chunk to ten chunks, but a substantial share of variance is captured with 20 messages and most correlations are close to their maximum well before 200 messages. Conscientiousness levels off earliest (Sec 3.1, Fig 2). (Sec 3.1; Fig 2 (caption and text)) | P6 gated shrinkage pulls members with fewer than 8 chunks toward 0. Interview chunks get double weight (C_INT_W 2.0) for Conscientiousness only. `fec/members/big5_llm.py:352-353,362-387` | The saturation volume comes from personal, cue-dense posts. Political text carries few personality cues, so saturation probably needs far more text. The paper's curve should not be used to relax the n_chunks gate. 20 messages at 17.10 words each is about 342 words, one 1,200-word chunk is much larger. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary says accuracy is mostly reached by about 20 messages. The paper says a substantial share of variance is captured at 20 messages and most correlations are near maximum well before 200. The stronger wording is not supported.
> - Summary issue: Summary says absolute errors are larger for older users under GPT-3.5. That holds only for Openness and Conscientiousness; Agreeableness residuals are smaller for older users under GPT-3.5 (t = -2.64, Sec 3.2.2).
> - Summary issue: Summary says the paper backs the design of the display-only item reader. The paper tests one holistic prompt and never evaluates item-level or chunked designs; chunk-then-average is only its procedure. Endorsement is overstated.
> - Summary issue: Summary says the paper sets a realistic ceiling for zero-shot accuracy. The paper reports one zero-shot level and states no ceiling.
> - Summary issue: Summary and item key_findings quote the pooled r .29 for the whole sample (63.1 percent women, mean age 24.2) without strata. Unless labeled UNSTRATIFIED, it should be quoted with the within-gender and within-age values, which the text reports only as not significantly different; the stratum values sit in S2-S5, not in the text file.
> - Summary issue: Summary says the paper warns that observer agreement may reflect shared impressions. The paper says cues may not be valid predictors of self-perception and LLMs may replicate human judgment biases. Shared impressions is the analyst's gloss and should be labeled as such.
> - Summary issue: paul_alignment says Paul's recorded position names GPT-4o and DeepSeek-V3 as the only validated models. No such statement was found in the vault concepts; DeepSeek-V3 appears in the Birkenmaier research note and in vault framework and psychometrics notes, but not as an 'only validated' claim. Paul's position is unverified here.
> - Summary issue: The academic observer count differs: the gold-set note gives 125 and the appendix note counts about 126 rows from text. Summary and bearings should state which source they use.
> - Summary issue: Slug years differ from publication years for azucar-2017 (published 2018) and koutsoumpis-2024 (published 2022). Naming only; the links point to the right papers.
> - Summary issue: Summary says Peters is a reason to check the live affective-N input for gender bias. This is a reasonable inference from the paper's N-residual gender pattern, but it is the analyst's inference, not a finding of the paper.

## Gaps and flags

- No self-report or friend-rating criterion exists for members, so LLM trait accuracy cannot be compared with the paper's r .29 benchmark on the same footing.
- The affective-N input (live via RECEPT_N=affective) is party-controlled but not checked for gender-dependent level bias, which the paper documents for LLM-inferred N, A and C.
- The primary LLM pass is a single call per chunk. Between-rep variance is measured via --rep, but reps are not averaged into the score.
- Whether LLM agreement with the Senate insider and academic panels reflects shared impression rather than trait is untested. The paper's C result (LLM-observer agreement above self agreement) makes this a live alternative.
- The paper's motivation includes psychological targeting and persuasion at scale (Matz 2017, 2024). Its accuracy numbers must not be quoted as validity for Melange member traits, and any new analysis built on it stays UNVERIFIED until a second path reproduces it (DE-062).
- The near-observer accuracy claim rests on n = 68 and should not be generalized to expert-panel convergence.
- No conflict with the SNA, LDA or reciprocity rules: the paper is silent on networks, lobbying and elections.

Paul has no annotation on this work. His recorded position on the LLM reader is that LLM-based trait reading is endorsed, with GPT-4o and DeepSeek-V3 the only validated models (from Birkenmaier and Lechner). Peters supports feasibility, but only at modest accuracy (r about .3) on non-elite users, against self-report, with demographic level bias. That is weaker than an endorsement implies. The system's own record (reader display-only, diagonal +0.064) is already more cautious. The note is stale: an LLM-derived N facet now feeds live receptivity via affective N.

## Related papers

- [[cao-2024-language-models-personality]] — complements — Cao and Kosinski show that LLM-derived perceptions of public figures can be predicted from name embeddings alone (text check of the paper). That is the reputational-prior risk the de-identification step guards against, and it bears on how observer agreement is read.
- [[azucar-2017-predicting-personality-traits]] — cites — The note confirms Azucar, Marengo and Settanni as the meta-analysis Peters cites as ref 15. Published 2018 (online Dec 2017); the slug year is 2017, a naming difference only.
- [[birkenmaier-2025-measuring-politicians-public]] — complements — The note reports few-shot prompting of DeepSeek-V3 and GPT-4o reaching macro-F1 0.74-0.77 on elite text for agency and communion cues. This is the elite-text genre Peters does not cover.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — Meta-analysis of Big Five and LIWC relations, the closed-vocabulary baseline against which zero-shot LLM inference is compared. Published 2022; the slug year is 2024, a naming difference only.
- [[mondak-nd-online-appendix-personality]] — complements — The appendix gives the Senate insider and academic observer panels used as the system criterion. Its text-count of academic rows (about 126) differs from the 125 in the gold-set note.
- [[lemens-2025-positioning-political-texts]] — same-method — Title and text confirm a design of asking an LLM about each text and averaging the responses, parallel to Peters' three-query averaging.
- [[ramey-2016-measuring-elite-personality]] — complements — Ramey et al. run floor speech through LIWC and MRC features with pre-trained Mairesse SMOreg models, a closed-vocabulary approach on elite speech. Peters is the open-ended LLM alternative on non-elite text.
- [[palm-2025-influence-personality-traits]] — complements — Palm and Tims is a systematic review of whether Big Five traits moderate persuasion strategies. The research note lists its evidence status as unverified, so this link is kept on the note's abstract only.

### Linked from
_None._