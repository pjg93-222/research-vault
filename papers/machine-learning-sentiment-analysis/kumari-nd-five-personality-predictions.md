---
type: paper
title: "Big Five Personality Predictions From Twitter Using Regression"
authors: ["Jyoti Kumari", "Pragyesha Raghava Bhardwaj", "Rishabh Goyal", "Rishabh Yadav", "Sonam Jahan"]
year: ""
venue: "SSRN (ssrn.com/abstract=5213216); authors affiliated with Computer Science & Engineering, MIET College, Meerut. No journal or conference named."
doi: ""
category: "Machine Learning & Sentiment Analysis"
systems: ["melange-influence-engine", "melange-psychometrics"]
components: ["melange-psychometrics.lexical-big5", "melange-influence-engine.receptivity-layer"]
paul_annotated: false
paul_markup_count: 0
evidence_status: unverified
source: "[[_sources/kumari-nd-five-personality-predictions.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\ssrn-5213216.pdf"]
sha12: ["094aa1e64249"]
tags: [paper, machine-learning-sentiment-analysis, type/paper, theme/machine-learning-sentiment-analysis, evidence/unverified]
---

# Big Five Personality Predictions From Twitter Using Regression

**Jyoti Kumari, Pragyesha Raghava Bhardwaj, Rishabh Goyal, Rishabh Yadav, Sonam Jahan ().** *SSRN (ssrn.com/abstract=5213216); authors affiliated with Computer Science & Engineering, MIET College, Meerut. No journal or conference named..* Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> **What the paper is.** This is a short SSRN proposal from a student engineering group at MIET College, Meerut. It describes a pipeline that would take a Twitter handle, clean and tokenize the tweets, and build TF-IDF, LIWC-style, sentiment and profile features (follower count, tweet frequency, bio). A classifier or regression would then output each Big Five trait as a percentage, shown in a pie chart. The paper reports no sample size, no data period, no source for its trait labels, and no accuracy figures. The model list changes between sections. The trait names are non-standard ("Creativity, Dependability, Sociability, Kindness, Anxiety"), and several citations do not match the reference list. The abstract's claim about stress detection has no method behind it. In short, this is a design sketch, not evidence.
> 
> **Why it matters here, and how little.** The only component it touches is Melange's lexical Big Five reader. That reader already does more than the paper proposes: LIWC-analog rates, Mairesse SMOreg weights, a refit to RKH targets, token-based low-confidence shrinkage, and reported held-out CV (mean .350 across the five traits, window and N as recorded in the brain). The project's validation record goes further still. The pre-registered nomological look rated lexical Big Five as unfit for behavioural prediction, and receptivity now runs on a behavioural basis. The paper offers nothing that could test or recalibrate any of this, and none of its features should be adopted on its authority. Two cautions transfer. First, its profile features (follower count, tweet frequency) are volume proxies of the kind the system has already rejected. Second, it outputs absolute "percentages" with no reference norm, while the system deliberately reports traits as z-scores relative to its own corpus. Read it as a negative example of an unvalidated trait-from-text claim, not as support.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Can the Big Five personality traits of a Twitter user be predicted automatically from their tweets using NLP, sentiment analysis and machine learning/regression, as a scalable alternative to questionnaire-based assessment? (Abstract; Section 1.) The text never states a formal hypothesis.

**Theory:** Big Five trait model, which the paper labels as "Creativity, Dependability, Sociability, Kindness, and Anxiety" (Sections 1-2, 3.3). These labels are non-standard: the conventional Big Five are Openness, Conscientiousness, Extraversion, Agreeableness and Neuroticism. Section 2 itself refers to "agreeableness and neuroticism", so the labelling is internally inconsistent. The paper also draws on the claim that linguistic cues and sentiment link to personality traits (Section 2), citing a Facebook meta-analysis at "[14]".

**Data:** The paper describes a planned data pipeline, not an analysed dataset. Tweets are collected via the Twitter API using keywords, hashtags and user handles; fields include tweet text, user info, timestamps and interactions (Section 3.1). The system takes a Twitter handle and a user-specified number of tweets (Section 1). Unit: presumably one user's tweet set. Sample size (N): not reported. Time period: not reported. Source of trait labels for training: not stated ("Tweets are labelled according to personality traits using models like the Big Five", Section 3.3). No dataset name, size or download source is given.

**Method:** Pipeline (Sections 3.1-3.6): (1) cleaning, removing URLs, mentions, hashtags, lowercasing and stopword removal; (2) tokenization; (3) TF-IDF vectorization, plus LIWC and other psycholinguistic features; (4) user profile features (follower count, tweet frequency, bio); (5) sentiment analysis of positive and negative tone; (6) classifiers named as Random Forest, Neural Networks and SVM (Section 3.3), and elsewhere Naive Bayes, SVM, RNN or BERT (Section 3.5 text), with supervised regression to output a percentage per Big Five trait (Sections 1, 3.5, Figure 5). Training uses cross-validation to limit overfitting. Evaluation uses accuracy, precision, recall and confusion matrices (Section 3.4). Feature importance is analysed (Section 3.5). The results are visualised as pie charts (Abstract; Section 4). Regression is described only in words and in Figure 5 (caption only). The paper contains no equations beyond the TF-IDF formula caption (Figure 2).

**Identification:** None. The paper is a predictive pipeline with cross-validation. It makes no causal, quasi-experimental or instrumental claim, and no identification strategy is stated.

**Scope:** Population: Twitter users generally, with English-language tokenization implied (Section 3.2 discusses English word boundaries). No demographic, country or language restriction is stated. Institution: a student engineering project at MIET College, Meerut (CSE department). Era: not stated. The paper's references run to 2024, so the work is presumably from around 2024-2025. Generalisability: not discussed beyond cultural-context future work.

## Key findings

- The paper claims a machine learning approach can predict Big Five traits from Twitter data via sentiment analysis, with results shown as percentage pie charts. (Abstract; Section 4) — none reported
- Regression is used to output each Big Five trait as a percentage per user. (Section 1 (paragraph on the model); Section 3.5 final bullet; Figure 5 caption) — none reported
- Classifier choice (Random Forest, Neural Networks, SVM) is justified by stated effectiveness only; no comparison is run. (Section 3.3) — none reported
- Model accuracy, precision and recall are said to be measured with cross-validation and confusion matrices. No values are reported. (Section 3.4) — none reported
- The abstract claims the model reveals health-related indicators such as stress levels. No method, label or result for stress appears in the body. (Abstract vs. Sections 3-4) — none reported
- The paper concludes the approach is more efficient and scalable than questionnaires, and that it suits business and mental-health uses. No comparison data is shown. (Section 4 (Conclusion)) — none reported
- A personalised video link giving advice on personality improvement is described as a unique feature. Its design, content and evaluation are not described. (Section 1; Section 4) — none reported

## Limitations

- The authors state no limitations section. The text gives only future-research items: refine models for diverse cultural contexts and address data privacy and ethical concerns (Section 4; ethics also in Section 2).
- No sample size, data period, label source or validation results are reported, so the claimed accuracy cannot be assessed.
- Trait labels are non-standard (Creativity, Dependability, Sociability, Kindness, Anxiety) and do not match the conventional Big Five names used in the paper's own Section 2, which cites agreeableness and neuroticism.
- Classifiers and regression are both named, but the regression target and how training percentages were obtained are never explained.
- Model lists are inconsistent between Section 3.3 (Random Forest, NN, SVM), Section 3.5 (Naive Bayes, SVM, RNN, BERT) and the conclusion (SVM and deep learning).
- Several in-text citations do not match the reference list. For example, the Facebook meta-analysis is cited as [14], which is a GeeksforGeeks page. Reference [3] shares its title with this paper.
- Several references are to blog or tutorial pages, and some URLs look like placeholders (e.g., a Medium URL ending in 123456789). This weakens the evidentiary basis of the literature review.

## Short quotes

- "This study proposes a machine learning approach to predict the Big Five personality traits using Twitter data" (Abstract)
- "Regression techniques then help predict each of the Big Five traits as a percentage" (Section 1 (Introduction))
- "Supervised learning techniques, particularly regression models, may be applied to predict the exact percentages for each Big Five trait" (Section 3, closing methodology bullet (before Figure 5))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | context | Proposes predicting Big Five trait percentages from tweets with TF-IDF, LIWC/psycholinguistic categories, sentiment and profile features, using RF/SVM/NN (Sec 3.3) or NB/SVM/RNN/BERT (Sec 3.5) plus regression. No N, no label source and no accuracy, precision or recall values are reported. It says cross-validation and confusion matrices are used (Sec 3.4) but shows none. (Sections 1, 3.2-3.5; Figure 5 caption; Section 4) | A closed-vocabulary LIWC-analog reader (rates per 1000 words, with low-confidence flags below 1,500 tokens), weighted by Mairesse SMOreg and refit to RKH targets. Traits are z-scored relative to this corpus only. Held-out CV against RKH: E .510, A .406, C .363, ES .258, O .214. The system already implements, and has measured, the feature family the paper only proposes. `fec/members/big5_traits.py:11-28,109-118,189,255` | It does not transfer as evidence. There are no labels, no sample, no results, and the genre (general Twitter users) differs from legislators' floor speech and interviews. At most it is a list of candidate features. Its absolute 'percentage' outputs conflict with the system's rule that scores are meaningful only within this corpus. Its accuracy claim stays unverified and should never be cited as a benchmark (DE-062 spirit). | weak | UNVERIFIED |
| [[systems/melange-influence-engine#Receptivity layer (susceptibility, incentive, prospect domain, seed probability)\|Receptivity layer (susceptibility, incentive, prospect domain, seed probability)]] | context | Pitches text-derived personality as directly usable for business targeting and mental-health uses (Sec 4), with no validation of whether traits predict any behaviour. (Section 4 (Conclusion); Abstract) | The project already ran this check and found against the paper's premise. The pre-registered nomological validation rated lexical Big Five as unfit for behavioural prediction (C reversed on participation; E reads as broadcast talkativeness), so receptivity susceptibility now defaults to a behavioural basis (.40 coop + .30 N - .20 diligence - .10 dominance). RECEPT_TRAITS=lexical is only a revert switch. `fec/members/receptivity_layer.py:58-65,186-189` | This is only a caution. The paper's implicit claim that a text-derived trait is action-ready is exactly what the system's single-look record refuted for legislators. Nothing in the paper should move the system back to a lexical basis. | weak | UNVERIFIED |

## Gaps and flags

- None substantiated. The paper shows no result, so it cannot show that anything the system lacks matters.
- At most it is a reminder that the system has no Twitter-genre trait read validated against a gold panel. The system's own genre-transfer failure for affective N (interview to floor/hearing, rho .050 and .161) suggests a tweet-derived read would need its own look, not borrowed validity.
- NON-EVIDENCE: a proposal-style SSRN paper with no N, no label source, no data window and no reported metrics. Any accuracy or scalability claim it makes must not be quoted as a benchmark for Melange's trait readers.
- VOLUME-PROXY RISK: its user-profile features (follower count, tweet frequency) are volume measures. The system has already rejected volume proxies as trait signals (the Guiraud index, at r .70 with token count, is noted at fec/members/big5_traits.py:145, and the cooperation_z volume-proxy finding is recorded in receptivity_layer.py around line 95). Do not import these features.
- Non-standard trait labels ('Dependability' for C, 'Anxiety' for N, and so on) do not match the paper's own Section 2. Do not map its outputs onto O/C/E/A/N or ES slots without checking.
- Citation integrity is poor: in-text [14] points to a GeeksforGeeks page, ref [3] shares this paper's title, and a Medium URL looks like a placeholder. Its literature review should not be used as a source trail.
- The paper does not argue for either rejected SNA proposal, and makes no network, LDA or influence claims, so no D-021, D-025 or D-104..D-108 wording conflict arises.

Paul has not annotated this paper. On lexical Big Five, his position calls Ramey "dated but foundational" and highlights Birkenmaier's criticism of unvalidated SVM readers. This paper is weaker than both: it proposes classical SVM/RF/NB readers with no validation at all. It therefore reinforces Paul's scepticism and gives no support for text-derived traits. His notes are somewhat stale against the system in one respect. The system has gone past the "outdated SVM" critique to a measured verdict: the lexical Big Five was rated unfit for behavioural prediction, and receptivity now runs on behavioural dispositions. Nothing here reopens that verdict.

## Related papers

- [[azucar-2017-predicting-personality-traits]] — complements — A meta-analysis of Big Five prediction from social-media footprints. It supplies the real effect sizes this paper claims but never measures.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — A meta-analysis of LIWC-Big Five correlations. It gives realistic, small magnitudes for the LIWC features this paper proposes, and bears on the closed-vocabulary weights in big5_traits.py.
- [[ramey-2016-measuring-elite-personality]] — same-method — Closed-vocabulary trait measurement from speech, applied to legislators. This is the elite-population version of the pipeline this paper sketches for general Twitter users.
- [[birkenmaier-2025-measuring-politicians-public]] — complements — Provides the validation framework (codebook, Krippendorff alpha, macro-F1) that this paper entirely lacks.
- [[naz-2025-machine-deep-learning]] — same-method — A survey of the same ML and deep-learning trait-detection methods (SVM, RF, RNN, BERT) that this paper lists without comparing.
- [[celli-2012-role-emotional-stability]] — same-data — Twitter-based emotional stability research. It is an empirical counterpart to this paper's unsupported 'Anxiety'/stress claim.

### Linked from
- [[steinertthrelkeld-2018-twitter-data]] — built-on-by — Uses Twitter text as data to predict Big Five traits, a downstream use of the acquisition and processing pipeline this Element teaches. It bears on whether social posts could feed the lexical trait readers.
- [[hargittai-2015-bigger-always-better]] — complements — Item exists; title (Big Five Personality Predictions From Twitter Using Regression) matches. Kept.