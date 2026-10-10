---
type: paper
title: "Machine and deep learning for personality traits detection: a comprehensive survey and open research challenges"
authors: ["Anam Naz", "Hikmat Ullah Khan", "Amal Bukhari", "Bader Alshemaimri", "Ali Daud", "Muhammad Ramzan"]
year: "2025"
venue: "Artificial Intelligence Review 58:239 (accepted 25 April 2025; published online 9 May 2025). Metadata hint verified against the PDF text; no correction needed."
doi: "10.1007/s10462-025-11245-3"
category: "Machine Learning & Sentiment Analysis"
systems: ["melange-psychometrics", "melange-stance"]
components: ["melange-psychometrics.lexical-big5", "melange-psychometrics.affective-n", "melange-stance.supervised-stance-stack", "melange-psychometrics.llm-and-hybrid-readers"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "unverified"
source: "[[_sources/naz-2025-machine-deep-learning.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\s10462-025-11245-3.pdf"]
sha12: ["d389e5781b8a"]
tags: [paper, machine-learning-sentiment-analysis]
---

# Machine and deep learning for personality traits detection: a comprehensive survey and open research challenges

**Anam Naz, Hikmat Ullah Khan, Amal Bukhari, Bader Alshemaimri, Ali Daud, Muhammad Ramzan (2025).** *Artificial Intelligence Review 58:239 (accepted 25 April 2025; published online 9 May 2025). Metadata hint verified against the PDF text; no correction needed..* [10.1007/s10462-025-11245-3](https://doi.org/10.1007/s10462-025-11245-3). Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Naz et al. (2025) is a survey of machine-learning and deep-learning methods that detect personality from text. It covers 171 studies, selected from 305 search hits by a screening funnel whose numbers do not add up. It sorts methods into shallow ML, ensembles, sequential deep learning and transformers, catalogues 14 text datasets (several of them no longer available), and runs one experiment on the Kaggle MBTI data. In that experiment, Bi-LSTM with sentence embeddings (Extraversion 92, Agreeableness 91) and DistilBERT (Openness 92, Conscientiousness 91) beat TF-IDF SVM (Openness 89, Conscientiousness 80).
>
> The empirical part is weak. It reports accuracy only, with no train/test split, cross-validation, class balance or variance. It reports Big Five traits on a dataset labeled with MBTI types and never explains the mapping. Its text and tables disagree in several places. Its Table 9 ranks results from different papers as if they used the same data and protocol. The authors' answer to their research questions is that transformers and deep embeddings perform best, while traditional ML stays interpretable and competitive on small data.
>
> For Paul's system the paper is mostly context. Its population is ordinary social-media users who labeled their own personality type. The system instead measures legislators' public speech and checks it against expert and insider panels and against the Ramey, Klingler and Hollibaugh (RKH) congressional estimates, using single-look held-out correlations. The paper's accuracy figures are not comparable to the system's held-out r (mean .350 vs RKH) and should not be quoted as benchmarks. Two points do carry over. First, the paper states that personality datasets do not directly measure emotional stability or neuroticism, which matches the system's record that no gold criterion exists for N. Second, its finding that classical TF-IDF models stay competitive on small labeled sets matches the stance stack's measured result: LinearSVC at 83.9% beat the transformer at 0.757 on 87 test rows. That second point tempers Paul's view that bag-of-words SVMs are outdated.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Six research questions (Sec 3, p.3; answered in Sec 13, pp.45-46): RQ1 Which ML models suit personality trait detection in online text? RQ2 Which DL models suit it? RQ3 How do diverse features and ML/DL models affect accuracy? RQ4 What features and dataset sources exist? RQ5 What application areas exist and how do they affect fields? RQ6 What open research challenges exist and what strategies address them?

**Theory:** No single theory is tested. Background personality frameworks are summarized (Sec 6, pp.8-10, Table 4): MBTI (four dichotomies, 16 types), Big Five/OCEAN, HEXACO (adds Honesty-Humility), PEN (adds Psychoticism), and DISC (Dominance, Influence, Steadiness, Conscientiousness). The authors state they integrate psychological theory with computational linguistics (Sec 4, p.4) but do not show an operational model linking text features to constructs.

**Data:** Literature corpus: 305 initial search hits (IEEE Xplore, Springer, DBLP, ACM, MDPI, Google Scholar) narrowed to 171 primary studies (Sec 5, p.7). The text says the search covers "the last five-year literature" (p.6), while the cited works span 2018-2025. Empirical experiment (Sec 9, pp.25-32): the Kaggle MBTI dataset, unit = text sample or post labeled with one of 16 MBTI types. Sample size is not stated in Sec 9.1 (p.25). Sec 10.2 (p.34) states 8,675 volunteers, "fifty tweet texts" each, and 422,845 labeled data points; these figures are internally inconsistent (8,675 x 50 = 433,750), and Table 13 (p.33) lists 8,600 instances. Dataset survey: 14 textual personality datasets (Table 13, pp.33-37). Results tables (10-12) report Big Five trait labels (Extroversion, Conscientiousness, Agreeableness, Openness) on what is described as the MBTI dataset; the mapping is not explained.

**Method:** Systematic-style review: keyword search strings (p.6, Table 3 p.10), duplicate and title screening, abstract screening, full review, and quality questions on domain and venue (p.7; criteria not reported in detail). Taxonomy by shallow ML, ensemble, sequential DL (RNN/CNN/LSTM/Bi-LSTM/GRU), and transformers (Sec 7, pp.11-20; Tables 5-8). Preprocessing: lowercasing, URL/punctuation/number removal, stop-word removal, lemmatization and stemming, tokenization (Sec 7.1, pp.11-13; Sec 9.1, p.25). Features: TF-IDF, POS tags, BoW, N-grams, Word2Vec, GloVe, FastText, sentence embeddings (Sec 7.2, pp.13-14; Sec 9.2, p.26). Models: SVM, NB, LR, DT, KNN, GB, RF, XGBoost, AdaBoost, LSTM, Bi-LSTM, BERT, DistilBERT (Sec 9.3, pp.26-27). Empirical comparison is tabulated accuracy only (Tables 10-12).

**Identification:** None. Descriptive and comparative review; no causal design, no instrument validation, no inferential statistics.

**Scope:** Population: English-language social media and related user-generated text (Twitter, Facebook, Reddit, forums, YouTube transcripts, essays, dialogue corpora). Era: studies roughly 2018-2025. Institutions: none; the authors are from Pakistan, Saudi Arabia, and the UAE. The text says the focus is natural language content, not image, video, or audio (Sec 14, p.47). Lobbying, legislators, path-finding, allocation, turnout, and persuasion are absent. Elections appear only in passing (intro citation of Zarouali et al. 2024, p.2; political campaign paragraph, p.39).

## Key findings

- Search funnel: 305 hits, 270 after duplicate/title screening, '184 excluded' after abstract screening, 171 final. The stated numbers do not reconcile (270 - 184 = 86, not 171). (305 -> 270 -> 171 (exclusion stated as 184); Sec 5, p.7)
- Shallow ML on MBTI-labeled text (TF-IDF, Table 10): SVM is described as best; the authors cite Openness 89% and Conscientiousness 80% for SVM. GB reports Extroversion 86% and Openness 90%. DT with POS features is the lowest (Conscientiousness about 53%). (SVM: Openness 89, Conscientiousness 80; GB: Extroversion 86, Openness 90; XGB Openness 90; POS-DT Conscientiousness 53; Table 10 p.27; text p.28)
- Bi-LSTM with sentence embeddings gives the highest reported accuracy across embedding types, with the top figure at Extroversion 92 and Agreeableness 91. (Bi-LSTM + sentence embeddings: Ext 92, Cons 90, Agr 91, Open 89; Bi-LSTM + Word2Vec Ext 88, Cons 86, Agr 85, Open 85; Table 11 p.29; text p.28-29)
- DistilBERT outperforms BERT on all four traits in the authors' experiment, and is the top model overall (authors state 92% accuracy in the discussion). (DistilBERT: Ext 88, Cons 91, Agr 89, Open 92; BERT: Ext 82, Cons 77, Agr 82, Open 88; Table 12 p.31; discussion p.32)
- Text claims LSTM/Bi-LSTM improve by almost 25% over traditional ML, but no computation or table supports the figure. (claimed about 25% improvement, unsupported; Sec 9.5, p.28)
- Text reports LSTM with sentence embeddings at 91% Agreeableness versus 86% (Word2Vec) and 85% (GloVe), but Table 11 gives LSTM+GloVe Agreeableness as 89, not 85. (text 85 vs table 89 for LSTM+GloVe Agreeableness; Sec 9.5 p.29 vs Table 11 p.29)
- Text says DistilBERT 'has 40% of BERT model's predictive accuracy' while also outperforming BERT. The phrasing is self-contradictory; the 40% figure likely refers to parameter count, which the text does not state. (40% (ambiguous); Sec 9.6 p.31)
- Literature comparison on the MBTI dataset (Table 9) compiles accuracies from separate papers with different splits and preprocessing. The text nonetheless says the comparison is on 'the same dataset'. Highest listed: Xue et al. (2018) LSTM+DEC 97; Garg and Garg (2021) 90; El-Keiey et al. (2022) NB+LR 90; Canbay (2024) BERT 66. (Table 9 accuracies range 66-97; Table 9 p.21; Sec 7.5 p.21-22)
- Sec 7.5 says shallow ML accuracies fall in 79-85%, but Table 9 lists El-Keiey et al. NB+LR at 90. (79-85 (text) vs 90 (table); Sec 7.5 p.21 vs Table 9 p.21)
- Transformer results on MBTI (Table 9) vary widely: AWS-EP 78, Rauf et al. BERT 78, Akber et al. BERT+SBERT 90, Canbay BERT 66. (78 / 78 / 90 / 66; Table 9 p.21; Sec 7.5 p.21)
- MBTI dataset is described as the largest freely available personality dataset, with 8,675 volunteers and 422,845 labeled points. These counts are inconsistent with each other and with Table 13 (8,600 instances). (8,675 volunteers; 422,845 points; 8,600 instances (Table 13); Sec 10.2 p.34; Table 13 p.33)
- Dataset availability: MyPersonality (Facebook, 10,000 posts from 250 users) reported unavailable; Essays II unavailable; ASCERTAIN and the Conversation dataset reported no longer available; Pandora available on request. (14 datasets catalogued (Table 13, pp.33-37); Sec 10.3 p.35; 10.4 p.35; 10.6 p.36; 10.12 p.37; 10.13 p.37)
- Applications framed in Sec 11 (recruitment, marketing, counseling, healthcare, HRI, gaming, smart home), with ethics and bias risks. Political campaign targeting and social-influence analysis in social networks are mentioned in passing. (no quantitative content; Sec 11.1.1 p.39; Sec 11.2.2 p.40)
- Open challenges (Sec 12, pp.41-45) are listed as 13 items, including LLMs, personalization vs generalization, dataset scarcity, lack of attention models, privacy and bias, label ambiguity, behavioral data integration, social-engineering attacks, and multimodal fusion. (13 challenges, no quantitative content; Sec 12.1-12.13, pp.41-45)
- Answers to RQs: transformers (BERT, GPT) and deep embeddings are described as most effective; traditional ML remains interpretable and competitive on small data. (none; Sec 13, pp.45-46)

## Limitations

- Stated by authors: scope is textual data only; image, video, and audio personality detection are deferred to future work (Sec 14, p.47).
- Stated by authors: XAI is proposed but not implemented (Sec 14, pp.47-48).
- Observed (not stated by authors): the empirical experiment reports accuracy only, with no train/test split, cross-validation, random seeds, class balance, or variance across runs described anywhere in the text.
- Observed: the MBTI dataset is evaluated with Big Five trait labels in Tables 10-12, and no mapping between MBTI dichotomies and Big Five traits is given.
- Observed: Table 9 aggregates reported accuracies across different studies, datasets, and protocols, so cross-study ranking is not like-for-like despite the text calling it the same dataset.
- Observed: screening arithmetic does not reconcile (305, 270, 184 excluded, 171 final), and screening and quality-appraisal criteria are not reported in detail.
- Observed: numeric inconsistencies between text and tables (Sec 9.5 GloVe 85 vs Table 11 89; Sec 7.5 79-85% vs Table 9 90; MBTI counts in Sec 10.2 vs Table 13).
- Observed: no inferential statistics, effect sizes, or confidence intervals; claims such as '25% improvement' are unsupported.
- Observed: the text says the search covers the last five years but cites works from 2018 onward; the review window is not precisely defined.
- Observed: several cited dataset sources are unavailable (MyPersonality, Essays II, ASCERTAIN, Conversation), limiting reproducibility.

## Short quotes

- "Personality datasets do not directly measure emotional stability or neurotic tendencies." (Sec 1, p.2)
- "The initial search yielded a pool of 305 research papers." (Sec 5, p.7)
- "Transformers do not process data word by word as traditional models, and LSTMs do" (Sec 9.6, p.32)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | qualifies | Deep and transformer readers beat TF-IDF/closed-vocabulary shallow ML at trait detection. MBTI experiment: Bi-LSTM + sentence embeddings Ext 92 / Cons 90 / Agr 91 / Open 89; DistilBERT Open 92 / Cons 91; SVM (TF-IDF) Open 89 / Cons 80; POS-DT Cons about 53. The paper reports accuracy only, with no split, cross-validation or variance. (Tables 10-12, pp.27-31; Sec 9.5-9.6, pp.28-32; Sec 13, pp.45-46) | The lexical reader works in the style of Ramey et al. It z-scores LIWC-analog, Empath, MATTR and concreteness rates within this corpus and combines them with published-correlate or SMOreg weights, which are then refit to RKH with ridge or LinearSVR. Held-out CV r vs RKH: E .510, A .406, C .363, ES .258, O .214 (mean .350). Members with fewer than 1,500 tokens are flagged lowconf, and consumers shrink them toward 0. `fec/members/big5_traits.py:3-22,28,251; fec/members/fit_smoreg_rkh.py:64-65` | Weak transfer. The paper's labels are MBTI types that ordinary social-media users reported about themselves, its unit is the post or user, and its metric is accuracy on dichotomized traits. The system's targets are expert and insider ratings or RKH estimates for about 147 to 533 legislators, scored by held-out correlation. Fine-tuning a deep reader would need thousands of labeled members, and the system has no such set. The paper therefore gives no usable effect size for replacing the lexical reader, and its accuracies must not be compared with the system's r. | weak | UNVERIFIED |
| [[systems/melange-psychometrics#Affective neuroticism slot and N genre-transfer tests\|Affective neuroticism slot and N genre-transfer tests]] | supports | Text personality datasets lack a direct measure of emotional stability or neuroticism (Sec 1, p.2). The survey's experiment reports only Extraversion, Conscientiousness, Agreeableness and Openness, with no N/ES result. (Sec 1, p.2; Tables 10-12, pp.27-31) | affective_n combines a Pennebaker LIWC base (first-person singular, negative and positive emotion) with LLM affective facets, controlled for party. Because no gold N criterion exists, its gate tests construct integrity only. Genre transfer of interview N to floor and hearing text failed (rho .050, .161). `fec/members/emit_affective_n.py:1-20` | This is a statement about the field, not a test. It confirms that the literature offers no ready-made N criterion the system has missed. It does not validate the system's affective-N construct. It applies regardless of population, because the gap concerns the datasets themselves. | weak | UNVERIFIED |
| [[systems/melange-stance#Supervised and stacked stance readers\|Supervised and stacked stance readers]] | qualifies | Traditional ML (SVM and similar on TF-IDF) remains interpretable and competitive on small data. Deep and transformer gains appear with large labeled corpora. The text claims about a 25% LSTM gain over traditional ML, but no table supports the figure. (Sec 13, pp.45-46; Sec 9.5, p.28) | The best classical stance reader is LinearSVC on word (1-2 gram) plus char_wb (3-5 gram) TF-IDF: 83.9% accuracy and macro-F1 .816 on 87 untouched test rows, versus a transformer at 0.757 on the same comparison. Supervised tweet stance reached 0.737 CV on 399 golds. `fec/stance/train_stance_domain.py:66-84` | The paper studies personality, not stance, so this is a pattern about small labeled sets of a few hundred rows, not stance evidence. It fits the system's measured outcome. It does not show that transformers lose at larger n, and it says nothing about LLM zero-shot readers (Birkenmaier, Kim J.), which Paul's view rests on. | weak | UNVERIFIED |
| [[systems/melange-psychometrics#LLM item-level reader (Pink), ridge hybrid and CT-CM latent model\|LLM item-level reader (Pink), ridge hybrid and CT-CM latent model]] | context | LLMs are listed as an open challenge and future direction for personality detection, without an evaluation. BERT and GPT are named most effective in the RQ answers. The paper does not discuss identity leakage, reputational priors or halo. (Sec 12.1-12.13, pp.41-45; Sec 13, pp.45-46) | The item-level LLM reader removes author identity (name, state, party, title) before scoring, codes 'cannot tell' as 0, and averages 1,200-word chunks. It is display-only, and its behavioural diagonal mean of +0.064 falls short of the 0.12 bar. The ridge hybrid combines lexical, interaction and persona features under nested CV against behavioural targets. `fec/members/big5_llm.py:3-22; fec/members/big5_hybrid.py:1-12` | The system's LLM design is already more careful than anything the survey covers (de-identification, observer-keyed items, held-out behavioural criteria). The paper adds no evidence that would move the reader off display-only status. | weak | UNVERIFIED |

## Gaps and flags

- The system has no labeled corpus large enough to fine-tune a transformer trait reader on legislators. Every reader is either closed-vocabulary with weights fit to RKH, or a zero-shot LLM. The survey's best results all depend on thousands of labeled users.
- The survey treats sentence embeddings with a Bi-LSTM or DistilBERT as the strongest representation. The system's lexical reader has no embedding-based variant tested under the single-look protocol against RKH or the gold panels. This is a possible candidate, but it would need its own preregistration and remains UNVERIFIED.
- The survey names XAI and multimodal fusion (text plus audio) as open challenges. The system's audio prosody channel already MISSed (N=90) and is display-only, so the survey offers no new route there.

- Do not quote this paper's accuracies (88-92%) as benchmarks for the system's trait readers. They are accuracy-only, with no train/test split, cross-validation or variance reported. They are Big Five labels on an MBTI-typed dataset with no stated mapping, and text and tables disagree (Sec 9.5 GloVe 85 vs Table 11 89; Sec 7.5 79-85% vs Table 9 90).
- Table 9 ranks accuracies compiled from different studies with different protocols and calls them results on 'the same dataset'. That is not a like-for-like comparison, so no ranking from it should enter brain notes.
- The screening funnel does not reconcile (305 -> 270, 184 excluded, 171 final), and the MBTI sample counts conflict (8,675 x 50 vs 422,845 vs 8,600). Treat the review's coverage claims as approximate.
- The survey mentions applications to political microtargeting and social influence (Sec 11, pp.39-40), but it provides no evidence that detected traits predict response to persuasion. Nothing here validates the receptivity layer's trait-to-susceptibility weights, whose sign choices remain design priors.
- There is no conflict with D-025, D-021, the rejected SNA proposals, or the reciprocity rulings. The paper does not touch networks or lobbying.

There is no annotation from Paul on this paper. It partly agrees and partly disagrees with his reading elsewhere. It agrees with his endorsement of transformer and LLM readers (via Birkenmaier and Kim J.) in that it ranks BERT and GPT highest. It also tempers his highlighted view that bag-of-words SVM baselines are outdated: the survey's own conclusion is that classical models stay competitive on small labeled sets, and the system's stance stack measured exactly that (LinearSVC 83.9% vs transformer 0.757, n=87). On personality, Paul calls Ramey 'dated but foundational'. This survey neither replaces Ramey for legislators nor supplies a validated alternative, because none of its data are politicians and none of its measures use expert ratings. The system's record (lexical traits indicted for behavioural prediction, the LLM reader display-only, N without a gold criterion) remains more pessimistic, and better evidenced, than this survey's optimism. His notes on LLM trait reading should be read alongside that record.

## Related papers

- [[yang-2023-getting-personal-deep]] — cites — Cited as the deep-learning artifact for text-based personality measurement, which is the strongest measurement-validity work in the survey's reference list.
- [[azucar-2017-predicting-personality-traits]] — complements — A meta-analysis with pooled effect sizes for Big Five prediction from social-media footprints. It supplies the quantitative synthesis this narrative survey lacks.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — A meta-analysis of LIWC-Big Five relations, which bounds what the closed-vocabulary features the survey treats as baseline (and the system's lexical reader) can recover.
- [[ramey-2016-measuring-elite-personality]] — complements — The system's lexical reader copies Ramey's pipeline for legislators. The survey covers the general-population ML lineage, which does not address elites.
- [[birkenmaier-2025-measuring-politicians-public]] — complements — A validated politician-specific LLM trait reading (agency and communion, Krippendorff alpha, macro-F1). It is the counterpart for political elites that this survey does not cover.
- [[celli-nd-twenty-years-personality]] — same-method — Another review of personality computing that covers the same ML pipeline families and their threats.
- [[kumari-nd-five-personality-predictions]] — same-method — Predicts the Big Five from Twitter text, the shallow-ML setting the survey catalogues.
- [[b-2025-comparing-fine-tuned]] — complements — Compares fine-tuned RoBERTa with traditional ML on political-tweet stance, the same classical-versus-transformer comparison applied to the stance task the system actually runs.

### Linked from
- [[salam-2022-learning-personalised-models]] — complements — Corrected: Naz is a review of ML and DL trait detection in online text. It mentions multimodal and audio approaches only in passing, so same-method overstates the overlap with Salam's multimodal NAS.