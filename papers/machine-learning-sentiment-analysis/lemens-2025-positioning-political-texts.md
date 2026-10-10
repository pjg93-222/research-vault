---
type: paper
title: "Positioning Political Texts with Large Language Models by Asking and Averaging"
authors: ["Gael Le Mens", "Aina Gallego"]
year: 2024
venue: "arXiv 2311.16639 (v3, 5 Sep 2024); text states conditionally accepted in Political Analysis"
doi: ""
category: "Machine Learning & Sentiment Analysis"
systems: ["melange-psychometrics", "melange-stance"]
components: ["melange-stance.debate-nli-classifier", "melange-stance.supervised-stance-stack", "melange-stance.media-blend", "melange-stance.two-hop-decomposition", "melange-psychometrics.llm-and-hybrid-readers"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/lemens-2025-positioning-political-texts.pdf]]"
original_paths: []
sha12: ["9b4e8f6de1bb"]
tags: [paper, machine-learning-sentiment-analysis, type/paper, theme/machine-learning-sentiment-analysis, evidence/verified]
---

# Positioning Political Texts with Large Language Models by Asking and Averaging

**Gael Le Mens, Aina Gallego (2024).** *arXiv 2311.16639 (v3, 5 Sep 2024); text states conditionally accepted in Political Analysis.* Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Le Mens and Gallego (arXiv 2311.16639, v3 dated September 2024) test a simple measurement recipe: ask an instruction-tuned LLM for a 0 to 100 position score for each short text (a tweet or one manifesto sentence), let it answer NA when the text is not on the dimension, and average the answered scores to place longer documents or actors. Across four settings the best models track human benchmarks closely. On 899 Congress tweets, GPT-4o reaches r=0.92 with a mean of about 20 crowd raters. For 117th-Congress senators, the comparison is with the first Nokken-Poole roll-call dimension, and r=0.97. On 18 UK manifestos, r=0.98 on the economic dimension and 0.94 on the social one, against expert coding. On 36 European Parliament coal-subsidy speeches in 10 languages, r=0.93 with crowd coding. Fine-tuned BERT, tf-idf and GloVe classifiers score lower. The headline figures are mostly between-party, though. Within-party correlations fall to 0.66 to 0.72 for tweets and senators. On the pro/anti-subsidy dimension, the Against group drops to 0.04 to 0.07 for every model except GPT-4o. All validity evidence is concurrent correlation, and Claude models were not tested. For Paul's systems, the paper supports using an LLM as a stance rater and contrasts with classical TF-IDF readers. It also points to three things the CU stance pipeline does not yet have: a continuous position score, explicit not-applicable handling kept separate from neutral, and validity reported within party. Because the paper's dimensions are left-right and pro/anti-subsidy, it cannot validate the target-specific credit-union stance on its own.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Can instruction-tuned LLMs position short texts (tweets, sentences) on ideological or policy dimensions by directly asking for a 0 to 100 score and averaging those scores to position longer texts and political actors, and how do the estimates compare with expert, crowd, roll-call, campaign-finance and classifier benchmarks?

**Theory:** No substantive theory is tested. The framing is measurement: text scaling estimates latent positions in ideological or policy spaces, and the paper builds on the direct-query typicality method of Le Mens et al. (2023a).

**Data:** (1) 899 US Congress tweets after GPT-4 cutoff (from 900 in Le Mens et al. 2023a); benchmark mean of about 20 Prolific raters per tweet (597 raters; 14,362 responses, Fig A1); tweet dates not given. (2) 98 senators (Fig 2 caption; App A.2 says 97) of the 117th Congress, 100 random tweets each; benchmark DW-NOMINATE first dimension, plus 2020 CFscore (App A8). (3) 18 British manifestos (Benoit et al. 2016), economic and social scales; expert and crowd sentence coding benchmarks. (4) 36 speeches, one European Parliament coal-subsidy debate, 10 languages; benchmark mean of six crowd codings.

**Method:** Direct query: LLM gives a 0 to 100 JSON Score on a left-right, economic, social or pro/anti-subsidy scale, NA allowed for non-political text; temperature 0, max 20 tokens. Manifestos and speeches scored sentence by sentence and averaged. Comparators: fine-tuned BERT, GloVe and tf-idf classifiers. Validity by Pearson r with benchmarks; split-half reliability with Spearman-Brown. Robustness: dimension prompts, whole-document prompts, typicality scoring, ENO. No inference beyond correlations.

**Identification:** None in the causal sense. Validity is established by concurrent correlation with external benchmarks (crowd ratings, expert coding, roll-call DW-NOMINATE, campaign-finance CFscore) and by comparison with supervised classifiers.

**Scope:** US Congress members (tweets, 117th Congress senators), UK party manifestos (economic and social policy dimensions), European Parliament debate on a coal state-aid proposal in 10 languages. Era: tweets from 2023-era Congress, senators 2021-2023, manifestos from 1987-2010 labels in figures, EU debate policy horizon 2010 to 2014. Generalization is explicitly limited by the authors to the settings tested.

## Key findings

- Tweets (N=899, Sec 3.1, Fig 1): GPT-4o r=0.92 with crowd mean (557 scored tweets); GPT-4 r=0.90; MiXtral 8X22B 0.90; Llama 3 70Bq4 0.88; Llama 3 8B 0.80; fine-tuned BERT 0.78; tf-idf 0.66; GloVe 0.63.
- Tweets within party (Fig 1): GPT-4o r=0.68 among Democratic tweets and 0.66 among Republican tweets. Text says no classifier matches the best LLMs, especially for within-party differences.
- Senators of the 117th Congress (Sec 3.2, Fig 2): GPT-4o r=0.97 with first-dimension DW-NOMINATE; GPT-4 0.97; Llama 3 70Bq4 0.97; MiXtral 8X22B 0.96; Llama 3 8B 0.96; BERT 0.93; tf-idf 0.90; GloVe 0.86. Within-party GPT-4o r=0.72 (Democrats) and 0.72 (Republicans).
- Senators and CFscore (App A8, figure-only): LLM positions correlate highly with 2020 campaign-finance scores overall but less so within party; coefficients are not stated in the text.
- UK manifestos, economic dimension (Sec 3.3, Fig 3): GPT-4o r=0.98 overall with expert coding (Labour 0.98, Conservatives 0.33, Liberal Democrats 0.25). Crowd coding r=0.96; fine-tuned BERT 0.96; GPT-4 0.98; MiXtral 8X22B 0.97.
- UK manifestos, social dimension (Fig 4): GPT-4o r=0.94 overall (Conservatives 0.79, Liberal Democrats 0.31, Labour 0.96); crowd coding 0.92; BERT 0.92; Llama 3 8B 0.88 overall with Labour only 0.46.
- Whole-manifesto single prompts (App G.2, Figs A12-A13): overall economic r about 0.96 to 0.98 and social about 0.83 to 0.92, with weaker within-party fit; GPT-4 Turbo failed to return social scores for two manifestos. Authors prefer sentence-level scoring.
- EU speeches in 10 languages (Sec 3.4, Fig 5): GPT-4o r=0.93 overall with six-language crowd average (Against speakers 0.91, For speakers 0.80); GPT-4 Turbo 0.86 (Against 0.07); Llama 3 70B 0.82 (Against 0.05); MiXtral 8X22B 0.78 (Against 0.04); Aya 23 35B 0.74; Llama 3 8B 0.52.
- Cost and speed (Sec 4): GPT-4 API cost about 1.5 USD for the 900 tweets, versus about 1,626 pounds for the crowd coding of the same tweets; the authors note sentence-by-sentence scoring takes about one to two seconds per sentence on local models.
- Non-response pattern (App E, Table A1): tweets that received an LLM score had more human ratings on average (GPT-4o 18.7) than unscored tweets (11.6), consistent with NA signaling low political content.
- Effective number of observations (App E.3, Table A2): LLM scores for all tweets are worth about 3 to 7 human raters. Text says 5 to 7 for GPT-4, MiXtral and Llama 3 70B; the table lists 7 for GPT-4o.
- Typicality-difference variant for tweets (App D, E.2, figure-only): correlations similar to baseline, some within-party gains, scores for all tweets.

## Limitations

- Authors state validity is limited by the information in the input texts and that LLM estimates may differ from expert-survey positions because inputs differ.
- Authors warn LLMs can be biased or unreliable in some settings and that case-by-case empirical validation remains essential before use in other domains or dimensions.
- Multilingual accuracy varies by model and language, and systematic differences can bias downstream econometric analysis; authors call this an open problem.
- Within-party correlations are weak for some groups (e.g. Conservative and Liberal Democrat economic positions, Labour social positions for Llama 3 8B), and the tweet within-party gap persists.
- Single-prompt scoring of whole documents lowers performance for manifestos; the best single-prompt model setting for long texts is unresolved.
- Training-data overlap with benchmark texts is unknown; the authors say the LLMs were most likely not trained specifically on these manifestos.
- Reproducibility holds only for open models with fixed weights; API models change over time, and temperature 0 leaves residual ties and non-determinism.
- Internal inconsistencies: senator N is 98 in the Fig 2 caption but 97 in App A.2; figure captions say 118th Congress for the tweets while the text says the 117th for senators; ENO values in text and Table A2 differ for GPT-4o.
- Non-response (NA) is not modeled; scores are averages over only the sentences or tweets an LLM answered, which can change the composition of the average.
- The paper is a research note; many robustness results (A8, H.2, Fig 5 subsets) are figure-only and not numerically reported in the text.

## Short quotes

> We ask an LLM where a tweet or a sentence of a political text stands on the focal dimension and take the average
> This approach is generally more accurate than the positions obtained with supervised classifiers trained on large amounts of research data.
> Case-by-case empirical validation remains essential.

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-stance#Fine-tuned Political DEBATE NLI stance reader\|Fine-tuned Political DEBATE NLI stance reader]] | supports | Asking an LLM directly for a 0-100 position score (JSON only, temperature 0, NA allowed) gives text-level positions. GPT-4o r=0.92 against crowd means, but on the 557 of 899 tweets that received a score (NA excluded). API cost about 1.5 USD for 900 tweets versus 1,626 GBP for human coding. Table A2 ENO for the LLMs in the table is 3 to 7 human raters. (Sec 2.1 (prompt, temperature 0, max 20 tokens, JSON); Sec 3.1 and Fig 1 (NAll=557); Sec 4 Discussion (1.5 USD vs 1,626 pounds); App E.3 Table A2) | claude_stance.py is a Claude API rater that returns JSON only (stance, confidence, reason). Its docstring says it pre-labels the ~400 hand-coding set and is the third ensemble member beside fine-tuned DEBATE and the bill signal. Code matches this description. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/claude_stance.py:3-9,19-25,84-91` | The paper tests GPT-4, GPT-4o, MiXtral and Llama 3 (Table 1 lists no Claude model) on left-right and subsidy dimensions against independent crowd and expert benchmarks. The system rater is categorical (support/oppose/neutral), not a 0-100 scale, and its gold labels were pre-labelled by the same rater. The paper's accuracy figures cannot be quoted as properties of the Claude rater. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-stance#Supervised and stacked stance readers\|Supervised and stacked stance readers]] | qualifies | LLM direct query beats text classifiers on the paper's own benchmarks. Tweets: GPT-4o r=0.92 versus fine-tuned BERT 0.78, tf-idf 0.66, GloVe 0.63. Senators: GPT-4o 0.97 versus BERT 0.93, tf-idf 0.90. The paper says no classifier matches the best LLMs on within-party differences. Its classifiers are typicality readers (naive Bayes tf-idf, BERT, GloVe) trained on about 1 million party-labelled Congress tweets, not stance classifiers trained on human stance labels. No linear SVM is tested. (Abstract; Sec 3.1 and Fig 1 (text on naive Bayes TF-IDF and about 1 million training tweets); Sec 3.2 and Fig 2) | fec/stance/train_tweet_stance.py builds a LinearSVC on word and char_wb TF-IDF with a CV grid over C, so the system's best classical reader is the SVM as described. The 83.9% accuracy, macro-F1 0.816 (n=87), 0.757 transformer and 0.737 CV (399 golds) figures are not in the code and were not checked against outputs. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/stance/train_tweet_stance.py:17,34-39` | The paper compares continuous left-right positions, scored by correlation, against party-typicality readers. The system compares three-class target stance, scored by accuracy, against a stance-trained SVM. The paper does not test a stance-trained linear model, so it cannot contradict the system's SVM-over-transformer result. Treat it as a head-to-head test to run. | weak | VERIFIED-SOFTENED |
| [[systems/melange-stance#Media blend (press + social) with recency decay\|Media blend (press + social) with recency decay]] | suggests-extension | Sec 2.1: a document's position is the average of the sentence or tweet scores for which the LLM returned a numeric score, so NA units are dropped from the average. App E, Table A1 (GPT-4o row): tweets that received a score had 18.7 human ratings on average versus 11.6 for unscored tweets, which the paper reads as NA marking low political content. (Sec 2.1; App E and Table A1 (GPT-4o row)) | SIGN maps neutral to 0.0 and build_media_blend adds sign times intensity times decay divided by len(hits) per post, so neutral posts contribute zero and are not removed from any count. claude_stance.py sets stance to neutral with confidence 0.0 when the API call fails or the reply cannot be parsed, so API failure and real neutrality look the same in the output. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/stance/build_media_blend.py:54,130; C:/Users/pgoff/OneDrive/Desktop/Draft Systems/claude_stance.py:80,89-91,96-99` | The paper validates averaging for left-right placement, not for CU advocacy intensity. Averaging measures position while the blend sums volume. Whether volume should count is Paul's policy call, not a recommendation to normalise. Any change needs Paul's ruling and zero-rank-delta or look discipline. | moderate | VERIFIED |
| [[systems/melange-stance#Two-hop stance x valence decomposition with curated registry\|Two-hop stance x valence decomposition with curated registry]] | qualifies | Sec 3.4 and Fig 5 (36 coal-subsidy speeches, 10 languages, six-language crowd mean): GPT-4o overall r=0.93, with Against 0.91 and For 0.80. Against-group r is 0.07 for GPT-4 Turbo, 0.04 for MiXtral 8X22B and 0.05 for Llama 3 70B, but GPT-4o is strong within Against. Tweets within party for GPT-4o: 0.68 Democrats and 0.66 Republicans (Fig 1). (Sec 3.4 and Fig 5; Sec 3.1 and Fig 1) | apply_two_hop_stance.py docstring states a model stance hop of 0.915 and a model valence hop of 0.689 on the 400 tweet golds, with a composed ceiling of 0.872. The curated-registry accuracy of 0.782 and the bottleneck reading are not in the file and were not checked against outputs. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/stance/apply_two_hop_stance.py:3-5,15,61-62` | The paper tests one policy dimension (coal subsidy, 36 speeches) against crowd benchmarks. The weak within-Against cases belong to models other than GPT-4o, which is strong in both groups. The paper does not discuss within-camp weakness, so any link to the valence hop is the analyst's inference and suggestive only. | weak | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#LLM item-level reader (Pink), ridge hybrid and CT-CM latent model\|LLM item-level reader (Pink), ridge hybrid and CT-CM latent model]] | qualifies | Sec 3.3 and Figs 3-4: sentence-by-sentence then averaged GPT-4o scores track expert coding on manifestos at r=0.98 (economic) and 0.94 (social). GPT-4 gives 0.98 and 0.92. Single whole-manifesto prompts (App G.2, Figs A12-A13) give economic r 0.96 to 0.98, similar to sentence scoring, and social r 0.83 to 0.92 with weaker within-party fit. Sec 4 says the paper has no clear recommendation on chunk versus single prompt. Sentences with NA are dropped from the average, not coded 0. (Sec 3.3 and Figs 3-4; App G.2 and Figs A12-A13; Sec 4 (single prompt versus sentence paragraph)) | big5_llm.py de-identifies author name, state, party and title before scoring, codes cannot-tell as 0 among -1/0/+1 item responses, uses about 1,200-word chunks sampled across each corpus, and averages upward. The +0.064 versus 0.12 gate and the display-only status are not in this file. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/members/big5_llm.py:8-17,51,216-221` | The paper's units are single sentences from party manifestos and speeches, not chunks of member speech, and it drops NA rather than coding it 0. The paper does not discuss de-identification or name priors, so the claim that its senator r=0.97 reflects name priors is the analyst's speculation. The paper does not say whether names were removed from senators' tweets. The traits differ, so it cannot rescue the failed behavioural gate. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Denominator error: the summary says GPT-4o reaches r=0.92 on 899 tweets, but r=0.92 is over the 557 tweets that received an LLM score (NA excluded). Fig 1, Table A1 and Fig A2 all give NAll=557.
> - Summary issue: Wrong range: the summary says the Against group drops to 0.04 to 0.07 for every model except GPT-4o. Only GPT-4 Turbo (0.07), MiXtral 8X22B (0.04) and Llama 3 70B (0.05) are in that range. GPT-3.5 Turbo is 0.56, Llama 3 8B 0.50, Aya 23 35B 0.35 and MiXtral 8X7Bq6 0.16 (Fig 5).
> - Summary issue: Overbroad claim: the summary says fine-tuned BERT, tf-idf and GloVe classifiers score lower. That holds for tweets and senators. On manifestos BERT gets 0.96 (economic) and 0.92 (social), close to GPT-4 levels (Figs 3-4). Restrict the claim to tweets and senators.
> - Summary issue: Overstated reading: the summary says the paper supports using an LLM as a stance rater. The paper scores left-right and pro/anti-subsidy positions and does not test stance classification. Reword as position scoring that may inform a rater.
> - Summary issue: Classifier description: the summary says the paper contrasts with classical TF-IDF readers. The paper's TF-IDF is a naive Bayes party-typicality reader, not a LinearSVC trained on stance labels, and no SVM is tested.
> - Summary issue: Strata missing: the lead quotes pooled overall correlations (0.92 tweets, 0.97 senators, 0.98 and 0.94 manifestos, 0.93 speeches) without within-party or within-camp strata. The rule requires the strata or an UNSTRATIFIED label next to them. Within-party figures are tweets 0.68 D and 0.66 R; senators 0.72 and 0.72.
> - Summary issue: ENO flag is inaccurate: the flag says ENO for GPT-4o is 7 in Table A2 but 5 to 7 in the text. The text gives 5 to 7 for GPT-4, MiXtral 8X22B and Llama 3 70B, which Table A2 lists as 5 each. GPT-4o's 7 is not mentioned in the text. The Discussion says about 4 or more raters, which is a third figure. Correct the flag to this three-way inconsistency.
> - Summary issue: Confirmed flags: the year hint 2025 conflicts with the text (dated September 6, 2024, arXiv v3), and the senator count is 98 in the Fig 2 caption but 97 in App A.2. Both flags are correct as written.

## Gaps and flags

- No continuous LLM position score for CU stance. The rater returns support, oppose or neutral only, so within-category intensity relies on LSD tone rather than a direct position query.
- claude_stance.py records API errors and unparseable replies as neutral with confidence 0. That makes non-response look like a neutral stance. The paper keeps NA separate and documents its composition effect.
- There is no within-party validity figure for the stance pipeline or the press pillar. The paper shows overall correlations of 0.92 to 0.97 shrink to 0.66 to 0.72 within party, and CU stance mostly varies within party.
- There is no reliability estimate (split-half with Spearman-Brown, or effective number of raters) for the Claude rater against independent human coders. Its gold labels were pre-labelled by the same rater.
- There has been no head-to-head test of an LLM direct-query rater against the TF-IDF LinearSVC on the untouched n=87 or the 760-row gold frame.
- Concurrent-correlation validity only. Senator r=0.97 is against the first Nokken-Poole dimension and is largely between-party; quote it only with the within-party figures (0.72 per party), per the stratification rule.
- The paper tests no Claude models and no credit-union dimension, so its accuracy figures cannot be quoted as properties of the system's Claude rater. Any adoption would be a new agent analysis and stays UNVERIFIED until a second path reproduces it (DE-062).
- Year hint 2025 conflicts with the text, which is dated September 2024 (arXiv v3). The slug keeps 2025.
- Internal inconsistencies in the paper: the senator count is 98 in the Fig 2 caption and 97 in App A.2, and ENO for GPT-4o is 7 in Table A2 but 5 to 7 in the text.

Paul has no annotations on this work. It agrees with his recorded positions on the stance components: he endorses LLM position extraction (Kim's GPT-4 labels; Birkenmaier and Lechner's GPT-4o beating SVM) and calls bag-of-words SVMs outdated. Here a TF-IDF LinearSVC is still the system's best classical reader, which this paper argues against. The weak within-party and within-camp fits also support the weakness he flagged, ambiguous statements. His notes do not cover non-response handling or within-party validity, so they are incomplete rather than stale.

## Related papers

- [[ornstein-2024-train-your-stochastic]] — same-method — This is the GPT-3 probability-based scaling the paper benchmarks against (its footnote 2 reports expert correlations of .92 and .8). Both position political texts with LLMs; this paper replaces token probabilities with a direct score query.
- [[burnham-2024-political-debate-zero]] — complements — Political DEBATE is the NLI base model the system fine-tunes for CU stance. Le Mens offers the generative direct-query alternative for the same reader slot.
- [[burnham-2023-stance-detection-practical-guide]] — complements — This guide covers target-specific stance classification. Le Mens covers continuous positioning; together they frame the categorical versus scaled choice for the CU rater.
- [[poole-1999-nominate-after-years]] — complements — The senator benchmark is the first Nokken-Poole roll-call dimension, a period-specific NOMINATE variant.
- [[bonica-2013-mapping-ideological-marketplace]] — complements — CFscores are the second actor-level benchmark (App A8). They correlate highly overall but less within party.
- [[birkenmaier-2025-measuring-politicians-public]] — same-method — This also measures politicians from their text with GPT-4o, here for agency and communion. The Paul-usage part of the why text was not verified in this run; the shared method is real.
- [[b-2025-comparing-fine-tuned]] — complements — This compares fine-tuned RoBERTa with classical ML for political-tweet stance, the same supervised-versus-classical contrast Le Mens extends to LLMs.

### Linked from
- [[peters-2024-language-models-infer]] — same-method — Title and text confirm a design of asking an LLM about each text and averaging the responses, parallel to Peters' three-query averaging.