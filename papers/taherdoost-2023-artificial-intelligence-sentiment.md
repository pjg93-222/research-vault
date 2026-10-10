---
type: paper
title: "Artificial Intelligence and Sentiment Analysis: A Review in Competitive Research"
authors: ["Hamed Taherdoost", "Mitra Madanchian"]
year: "2023"
venue: "Computers (MDPI), 12(2), article 37; published 7 Feb 2023"
doi: "10.3390/computers12020037"
category: "Machine Learning & Sentiment Analysis"
systems: ["melange-stance"]
components: ["melange-stance.debate-nli-classifier", "melange-stance.lsd-tone"]
paul_annotated: true
paul_markup_count: 0
evidence_status: unverified
source: "[[_sources/taherdoost-2023-artificial-intelligence-sentiment.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\computers-12-00037.pdf"]
sha12: ["996dcf96b3c8", "acee3d982957"]
tags: [paper, machine-learning-sentiment-analysis]
---

# Artificial Intelligence and Sentiment Analysis: A Review in Competitive Research

**Hamed Taherdoost, Mitra Madanchian (2023).** *Computers (MDPI), 12(2), article 37; published 7 Feb 2023.* [10.3390/computers12020037](https://doi.org/10.3390/computers12020037) Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Taherdoost and Madanchian (2023) is a scoping review, not an empirical study. They searched Scopus for 2012-2022 journal papers that combine AI or machine learning with sentiment analysis and business or competitive terms. Of 133 hits, 43 papers were kept; the text also says 90 were examined and never reconciles the two counts. The review counts papers by year, subject, keyword, author country and citations. Its narrative section repeats claims from the papers it cites. The one useful methodological point is borrowed from Alantari et al. (2022): neural-network classifiers predict best but diagnose poorly, and topic models do the reverse. The review runs no accuracy tests of its own. It does not separate sentiment from stance or address target-dependent valence. It does not cover political text, and it describes no quality appraisal or dual screening.
> 
> For the system as built, its bearing is thin. The CU stance stack already does what the review recommends, in a stricter form. stage5_debate.py uses a fine-tuned transformer NLI reader for direction, validated on held-out human gold: 0.839, kappa 0.742, n=87, member-grouped. Lexicoder LSD2015 is kept only as a tone-intensity term (stage5_lsd.R; run_stage5.R:141-143) and never decides direction. The predictive-versus-diagnostic split gives a framing for that division: the neural reader handles prediction and the transparent dictionary handles interpretable intensity. But that rests on consumer-review evidence and one cited study. The review says nothing about the system's real bottleneck, the valence hop at 0.689 in the two-hop decomposition, or about adversarial and sign-flip targets (0.571 on goldeval). Treat it as background, not as evidence for any component's design or constants.

## Paul's annotation

From [[annotations/Machine Learning & Sentiment Analysis - Machine Learning & Sentiment Analysis - Annotated Bibliography|Machine Learning & Sentiment Analysis - Machine Learning & Sentiment Analysis - Annotated Bibliography]]:

> Taherdoost, Hamed, and Mitra Madanchian. “Artificial Intelligence and sentiment analysis: A review in Competitive Research.” Computers, vol. 12, no. 2, 7 Feb. 2023, p. 37, [https://doi.org/10.3390/computers12020037](https://doi.org/10.3390/computers12020037).

## Study

**Question.** RQ1: What is the current state of research on AI-based sentiment analysis in competitive research (document type, subject area, keywords, author geography, most-cited work)? RQ2: What is the development of the major AI-based sentiment analysis approaches? RQ3: What are the difficulties and prospects of AI-based sentiment analysis in competitive research? (Section 3.1, p.4.) Stated aim: help new researchers keep up with the field and improve future research quality (p.2).

**Theory.** None formally stated. Descriptive literature review. Background frames competitive research as comparing a firm and its products with rivals (p.2), and sentiment analysis as a subtype of affective computing (p.2). No hypotheses are tested.

**Data.** Unit: a publication (journal article or review). Source: Scopus. Search results: 133 items (Sec 3.2, p.4: 133 Scopus results examined up to 26 Dec 2022); Sec 4 (p.6) says 90 were examined; 43 publications included. Window: 2012-2022. Inclusion: publication 2012-2022, journal scope. Exclusion: articles in press, non-English articles (Table 1, p.5). Document mix: articles 86%, reviews/editorials 14% (p.6).

**Method.** Scoping review following a PRISMA-style flow (Figure 1, image only). Boolean search strings combining AI / artificial intelligence / machine learning / deep learning with "Sentiment Analysis" and business terms (Competitive, Business, Market, Customer, Product, Service) (Table 1, pp.5-6). Descriptive bibliometric tallies (year, document type, subject area, keywords, author country, citation counts) and narrative review of selected papers for RQ2-RQ3. No quality appraisal, dual coding or inter-rater procedure is described; author contributions list H.T. for methodology and validation (p.13).

**Identification.** None. No causal or identification claims; evidence is descriptive counts plus narrative synthesis of selected papers.

**Scope.** Population: Scopus-indexed English-language journal articles and reviews from 2012-2022 that combine AI or machine learning with sentiment analysis and business or competitive terms. Era: 2012-2022 (search closed 26 Dec 2022). Not a study of a single institution or firm.

## Key findings

- Corpus size: 43 publications included out of 133 Scopus results. (133 results, 90 examined, 43 included (the 133 vs 90 counts are not reconciled in the text); Sec 3.2, p.4; Sec 4 Selection Results, p.6)
- Annual publication counts rise after 2018; text says no documents in 2012-2017 but also reports one 2017 paper. (2017: 1 (2.32%); 2018: 0; 2019: 5 (11.63%); 2020: 9 (20.93%); 2021: 12 (27.91%); 2022: 16 (37.21%); RQ1, pp.6-7 (Figure 4 is image-only))
- Document type is mostly articles. (Articles 86%; reviews and editorials 14%; RQ1, p.6 (Figure 2))
- Subject area is dominated by Computer Science. (Computer Science 31; Engineering 17; Business, Management and Accounting 8; Mathematics 7; Social Sciences 6; Decision Sciences 5; Materials Science 5; RQ1, p.7 (Figure 3 image-only))
- Most frequent keywords across selected papers. (Sentiment Analysis 32; Deep Learning 19; Machine Learning 16; Artificial Intelligence and Learning Algorithms 8; NLP and Social Networking (online) 7; Data Mining and Feature Extraction 6; CNN and Learning Systems 5; Table 2, pp.8-9)
- Author geography is concentrated in a few countries. (India 19; China 7; Malaysia 6; Saudi Arabia 6; Taiwan 5; United States 5; p.8 and Figure 6 (image-only))
- Most-cited papers in the set, with citation counts as listed by the review. (Yang et al. 2020 (ref 33): 145; Hung et al. 2020 (ref 34, social network analysis of COVID-19 sentiments): 98; Yi and Liu 2020 (ref 35): 44; Patel et al. 2020 (ref 36): 43; Zhao et al. 2021 (ref 37): 37; Feizollah et al. 2019 (ref 38): 33; Paredes-Valverde et al. 2017 (ref 39): 32; Nandal et al. 2020 (ref 40): 28; Chakriswaran et al. 2019 review (ref 41): 27; Mehta et al. 2021 (ref 42): 21; Table 3, pp.9-10)
- Claim that machine learning needs human-chosen features and that no universally agreed strategy exists for business prediction. (None (narrative claim citing ref 44); RQ2, p.10)
- Reported result from a cited study: a random forest model anticipated a business support system's health status one hour before an emergency. (Average inaccuracy of 14 points, as reported in the review; RQ2, p.10, citing Chen et al. (ref 47))
- Comparison of predictive versus diagnostic abilities of text-based models. (None; qualitative: neural network ML gives the most accurate predictions; topic models poor at prediction; neural networks poor at diagnosis; RQ2, pp.11-12, citing Alantari et al. (ref 50))
- Stated future directions (not tested): hybrid ML for sentiment categorization, sarcasm detection via linguistic patterns, micro-text and anaphora resolution, high-inflection languages and emotive lexicons, psychologically motivated reasoning for customer behaviour prediction. (None; RQ3, pp.12 (citing refs 54-58 and 49))
- Conclusion that the profession is becoming more academic, and that sentiment analysis will play a significant role in customer engagement; both are asserted rather than tested. (Based on 43 articles; no statistical test; Conclusions, p.13)

## Limitations

- Database restriction: review text says the database, which is mostly Scopus, restricts the review process (p.6).
- Acknowledged incompleteness: 'not all of the excellent works of literature were included in the search criteria for a variety of reasons' (p.4).
- Exclusion of articles in press and non-English articles (Table 1, p.5); scope limited to journal articles.
- Internal inconsistencies: 133 results vs 90 examined (pp.4, 6); 'no documents from 2012 to 2017' vs one 2017 paper (p.6).
- No dedicated limitations section. No described quality appraisal, dual screening or inter-rater procedure. Methodology appears to be a single author's (H.T.) work (p.13).
- Figures 1, 5 and 6 are image-only in extraction; PRISMA counts cannot be checked from text.
- Citation counts are as listed in Table 3 without an extraction date.
- Conclusion about the field becoming 'more academic' rests only on yearly paper counts.

## Short quotes

- "AI comprehends the tone of a statement, as opposed to merely recognizing whether particular words within a group of text" (Abstract, p.1)
- "Topic models, however, are not well suited for making predictions, and neural network models are not well suited for making diagnoses." (RQ2, p.12 (citing Alantari et al.))
- "Results indicate that this profession is becoming more academic with time." (Conclusions, p.13)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-stance#Fine-tuned Political DEBATE NLI stance reader|Fine-tuned Political DEBATE NLI stance reader]] | context | The paper asserts that AI understands the tone of a statement rather than just recognising words. Citing Alantari et al. 2022, it says neural-network ML gives the most accurate predictions of the text-based methods compared. The review reports no accuracy figures of its own, and its corpus is 43 business and competitive-research papers from 2012-2022. (Abstract p.1; RQ2 pp.11-12 (citing ref 50)) | A fine-tuned Political DEBATE NLI model (mlburnham/Political_DEBATE_base_v1.0) gives the CU stance direction. stance_sign is support, oppose or neutral, and the code states that this is independent of LSD tone. On held-out human gold it scored 0.839 accuracy / kappa 0.742 (n=87). It scored 0.796 on the 760-row frame and 0.571 on the adversarial goldeval frame. (`C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_debate.py:3-12,26-31`) | Weak. The cited evidence covers consumer reviews and business social media, not congressional press releases. It also concerns sentiment polarity, not target-specific stance toward CU policy interests. It neither supports nor tests the system's validated accuracy, which comes from a stricter protocol (held-out, member-grouped human gold). | weak | UNVERIFIED |
| [[systems/melange-stance#Lexicoder LSD2015 tone intensity|Lexicoder LSD2015 tone intensity]] | qualifies | Alantari et al., as summarised by the review, find a trade-off: neural networks predict well but diagnose poorly, while more transparent methods are poorer predictors. The review frames word-matching approaches as the baseline that AI improves on. The trade-off is qualitative only and no numbers are given. (RQ2 pp.11-12 (citing ref 50); Abstract p.1) | Lexicoder LSD2015 counts run with negation phrases removed first. They yield lsd_net_norm = net/words and lsd_charge = (pos+neg)/words. Only the magnitude (\|net_norm\| or charge) enters press_value = final_sign x intensity, so the dictionary sets intensity and never direction. (`C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_lsd.R:48-60; C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R:141-143`) | The paper agrees with the system's split of roles: an opaque predictive reader for direction and a transparent dictionary for an interpretable intensity scalar. It gives no evidence that dictionary intensity is valid on political press releases. The open question of whether LSD intensity matches human-coded intensity on this corpus is untouched. The source evidence is consumer reviews. | weak | UNVERIFIED |

## Gaps and flags

- Neither the review nor the system validates LSD tone intensity against human-coded intensity on congressional press releases. The predictive-versus-diagnostic framing makes this gap more visible, because the dictionary is kept for interpretability, and an interpretability claim needs a validity check.
- The review treats sentiment and stance as one construct and covers no target-dependent valence. The system's measured bottleneck is the valence hop (0.689 on 400 tweet golds), so the review offers nothing on the hardest part of the stack.
- The review's future directions are untested suggestions: sarcasm detection, hybrid ML for sentiment categorisation, and micro-text and anaphora resolution. Anaphora and target resolution may matter for releases that name a bill only once in a roundup, which is the failure behind the rejected bill-sign override. The review offers no method or evidence for this.
- Paul cites Taherdoost & Madanchian 2023 as support for LLM- and transformer-based position extraction under melange-stance.debate-nli-classifier. The paper cannot carry that weight. It is a single-author scoping review with internally inconsistent counts (133 vs 90 screened; 'no documents 2012-2017' vs one 2017 paper). It reports no accuracy figures and covers no LLM or political-text evidence. Cite it as background only.
- The review conflates sentiment with stance. The system's supervised-stance-stack assumes that sentiment is not stance, and stage5_debate.py:11-12 keeps the two separate. Do not cite this review for any stance-accuracy claim.
- The review's only network-analysis item is Hung et al. 2020 (ref 34, a social network analysis of COVID-19 sentiment). It is not used to argue for dropping regular cosponsorship edges or for per-edge volume normalisation, so neither rejected SNA proposal is raised. No influence or closeness wording is implied.

Paul's one recorded link files this review under melange-stance.debate-nli-classifier. His takeaway there endorses LLM and transformer position extraction and flags the difficulty of ambiguous statements. The review agrees only in general direction (AI and neural nets beat word matching) and adds no evidence for that position. Kim J. 2025 and Birkenmaier & Lechner 2025 carry the actual evidence. The review does not address ambiguous statements, apart from listing sarcasm and anaphora as future work. Paul's annotation is empty (no takeaways, no anchor), so nothing he wrote is stale. The system has already moved past what the review describes: a fine-tuned NLI reader on held-out human gold, a human-label overlay, a two-hop stance-times-valence decomposition and a dictionary used for intensity only.

## Related papers

- [[b-2025-comparing-fine-tuned]] — complements — This is the direct empirical comparison the review lacks: fine-tuned RoBERTa against traditional ML for stance in political tweets. It bears on the DEBATE-versus-LinearSVC question.
- [[rice-2018-corpus-based-dictionaries]] — complements — Corpus-specific dictionaries are the evidence-based answer to the review's dismissal of word-matching methods. They also bear on whether a general LSD2015 suits congressional text.
- [[haselmayer-2017-sentiment-analysis-political]] — complements — Combines a political-communication dictionary with crowdcoding validation, the validation step that the review never addresses for dictionary tone.
- [[zhang-2022-data-assisted-social]] — cites — The review cites a 2022 Inf Process Manag paper on social media analytics for competitive business decisions (ref 16). It appears to be the same work as this vault entry, though the first author is listed differently.
- [[ornstein-2024-train-your-stochastic]] — complements — Gives the political-text LLM evidence that the review does not cover, and is relevant to Paul's LLM-extraction stance.
- [[park-2025-automatic-construction-direction]] — complements — A direction-aware lexicon (the one behind the rejected verb lexicon) addresses the sentiment-versus-direction distinction that the review collapses.

### Linked from
_None._