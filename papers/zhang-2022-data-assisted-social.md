---
type: paper
title: "Big data-assisted social media analytics for business model for business decision making system competitive analysis"
authors: ["Honglei Zhang", "Zhenbo Zang", "Hongjun Zhu", "M. Irfan Uddin", "M. Asim Amin"]
year: "2022"
venue: "Information Processing & Management, 59(1), 102762 (received 15 June 2021; available online 27 September 2021)"
doi: "10.1016/j.ipm.2021.102762"
category: "Machine Learning & Sentiment Analysis"
systems: ["melange-influence-engine", "melange-sna", "melange-stance"]
components: ["melange-sna.press-hcoi-communities", "melange-influence-engine.salience-shock", "melange-stance.lsd-tone"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "unverified"
source: "[[_sources/zhang-2022-data-assisted-social.html]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\Big data-assisted social media analytics for business model for business decision making system competitive analysis - ScienceDirect.html", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\Big data-assisted social media analytics for business model for business decision making system competitive analysis - ScienceDirect.html", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\Big data-assisted social media analytics for business model for business decision making system competitive analysis - ScienceDirect.html", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\Big data-assisted social media analytics for business model for business decision making system competitive analysis - ScienceDirect.html"]
sha12: ["0970f83c9875"]
tags: [paper, machine-learning-sentiment-analysis]
---

# Big data-assisted social media analytics for business model for business decision making system competitive analysis

**Honglei Zhang, Zhenbo Zang, Hongjun Zhu, M. Irfan Uddin, M. Asim Amin (2022).** *Information Processing & Management, 59(1), 102762 (received 15 June 2021; available online 27 September 2021).* [DOI: 10.1016/j.ipm.2021.102762](https://doi.org/10.1016/j.ipm.2021.102762). Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Zhang et al. (2022) propose BD-SMAB, a conceptual decision-support framework for business competitive analysis built from social media. It chains together several standard pieces: DMN decision modelling; Affin-lexicon and ensemble-classifier labelling of praise versus complaint; NMF clustering; an L1-penalised linear model of market-value change on phrase-intensity change (Eqs. 1-2); z-scored one-day burst detection over trailing 5- and 25-day windows (|z|>30); a term co-occurrence graph (cross-correlation >= 0.1) partitioned with Louvain; and a hotness index He(t), the sum of sentiment times volume over bursting events. The case study covers 882,371 tweets about five retailers, collected by VOZIQ, a commercial partner one author worked with. Its outputs are descriptive only: Costco category positive shares (jewelry 91%, parking 39%) and mention shares (Wal-Mart 35%, Costco 24%; window and denominator not stated). The paper has no holdout, no baseline and no metrics. Its "highest accuracy" claim has no numbers behind it. The authors call the framework conceptual. The company list is inconsistent, the equations are garbled, and several citations are wrong (the Affin citation points to a photonics paper).
> 
> For Paul's systems this is a weak, context-level source. The closest overlaps are methodological. Louvain on a thresholded correlation graph is what press_hcoi.py already does, using Malaterre's 0.6 threshold. A volume-and-sentiment burst index is a possible time-varying trigger for the salience-shock engine, whose uncertainty and polarization terms are currently static and estimated from the stance matrix. The paper does not validate any of these components, so nothing in it can justify changing a threshold, half-life or weight. Any idea taken from it starts out UNVERIFIED.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Stated objective (Sec. 1, p. 3 of the text, "Introduction"): "the objective of this study is to check the usefulness of the development and implementation of an integrative management information system." The abstract frames it as whether a Big Data-assisted social media analytics model (BD-SMAB) can help decision-makers with marketing strategy and competitive analysis. No explicit hypotheses or formal research questions are stated.

**Theory.** No formal social-science theory. Foundations are knowledge management (KM) and business decision-making, plus the OMG Decision Model and Notation (DMN) standard for modeling decisions (Sec. 2 and Sec. 3). The decision procedure is a sequence of stages (intelligence, design/planning, choice/production of options, implementation). Stage naming is inconsistent: Sec. 3 (around line 162) says "information stage, conceptual design, development, and implementation stage," while later text describes intelligence, planning, produce options, and implementation. Sentiment and burst-detection components draw on text-mining and time-series methods, not a behavioral theory.

**Data.** Case study (Sec. 3.1): 882,371 tweets about five retail businesses, collected via the Twitter search API by VOZIQ, a social media analytics firm one author collaborated with. Unit: individual tweets and comments. Time period is not stated. Sec. 4 software/performance evaluation: company fan pages of FMCG firms on Facebook, six brands, with household-panel purchase data mentioned (sample size and panel size not reported). Fig. 6 refers to ten brands. Table 1: Costco product and service categories (positive comment % and mean %). Table 2: share of social media mentions for five companies (Wal-Mart 35%, Costco 24%, Home Depot 19%, Kmart 11%, Kohl's 11%). The company set is inconsistent across the text: Sec. 3.1 names Costco, eBay, Kohl's, Hardware Depot, Amazon (line 249), and later Costco, Kohls, Walmart, Kmart, Habitat Depot (line 261). N for the Sec. 4 analyses is not reported.

**Method.** Conceptual framework plus a descriptive case study. Components: (1) DMN-style decision modeling in two layers, decision requirements diagrams and decision logic (Sec. 2 and 3); (2) sentiment and phrase classification: Affin lexicon phrase scores, ensemble machine-learning classifiers on hybrid features to label praise vs complaint, and nonnegative matrix factorization for clustering (Sec. 1, around lines 125 and 129); (3) a linear model relating market value changes to weighted phrase/search-intensity changes, ΔC(t+1)=Σ w_k Δx_k(t) (Eq. 1), with weights estimated under an L1 penalty λΣ|w_k| (Eq. 2; the text writes "argmax" but a penalized least-squares minimization is described); (4) z-score normalization of 1-day variations over trailing n-day windows (Eq. 3; n=5 and 25 mentioned, burst threshold |z|>30); (5) burst detection on term frequency, a term co-occurrence/cross-correlation graph (threshold 0.1), Louvain community detection, and an event hotness index He(t)=Σ sentiment×volume over bursting events (Eq. 5); (6) Leximancer concept mapping and VOZIQ category/sentiment dashboards for the case study (Sec. 3.1). Eq. (1) and Eq. (2) are garbled in the extraction, so exact notation should be checked against the PDF.

**Identification.** None. There is no causal identification strategy, no train/test or holdout design, no baseline comparison, no significance testing, and no robustness checks. Claims of superiority ("highest accuracy") are not tied to any measured metric.

**Scope.** Population and setting: social media posts (mainly Twitter, Facebook) about large US-style retail companies (named in text; country not stated explicitly), plus FMCG brands on Facebook. Motivating domains listed in the abstract and conclusion include real estate, organizations, beauty trade fairs, finance, medicine, e-commerce. These are asserted, not analyzed. Era: circa 2021 publication; data period not reported. Institution: Chinese universities (author affiliations); VOZIQ is the commercial partner.

## Key findings

- Costco product-category results: jewelry has the highest positive-comment share (91%) and mean (87%); electronics 68%/73%, clothing 57%/63%, watches 56%/56%; services: returns and exchange 61%/80%, customer service 51%/37%, parking 39%/40% (the column header is garbled in extraction). (Jewelry positive 91%, mean 87%; parking positive 39%, mean 40%) — Sec. 4, Table 1 (lines 283 to 301)
- Share of social media mentions across five retailers: Wal-Mart highest at 35%, Costco 24%, Home Depot 19%, Kmart 11%, Kohl's 11%. The text says Wal-Mart and Costco have the highest monthly purchases among the set, but no purchase figures are shown. (35%, 24%, 19%, 11%, 11%) — Sec. 4, Table 2 and paragraph at lines 311 to 325
- Case study claims Walmart was the most-cited retailer over the whole research period. Costco's 'cash card' theme is favorable with no negative sentiment; 'special offers' is both positive and negative. 'Gift Certificates' and 'Freebies' are strongly connected in the favorable subject map. (882,371 tweets; no per-company counts given in text) — Sec. 3.1, lines 249, 256, 269
- Facebook social media index is reported as higher than Twitter's over the simulation. Exact values are only in Fig. 5, not in the text. (none reported in text) — Sec. 4, lines 303 and Fig. 5
- Medium-mentioned brands have medium purchase history, versus higher purchase for the most and least mentioned brands on Facebook (the text states a pattern but the direction is not clear from the prose). (Ten brands; no coefficients or counts) — Sec. 4, Fig. 6, line 327
- Authors state that the BD-SMAB model 'has the highest accuracy' in analyzing social media, with no accuracy, precision, error, or baseline figures reported anywhere in the text. (none) — Sec. 4, lines 303 and 335
- Stock-market component: the model defines a penalized linear model of market value changes on phrase/search-intensity changes, weights estimated with a penalty, and a hotness index He(t). No estimated coefficients, fit statistics, or forecast errors are reported. (burst threshold |z|>30; cross-correlation threshold 0.1; z-windows 5, 25 and 30 days) — Sec. 3, Eqs. 1 to 5 (lines 194 to 208)

## Limitations

- Authors state the work 'continues to be a conceptual model based on past research findings and aims to assess and improve the framework for future research' (Sec. 5, line 341).
- No validation metrics: 'highest accuracy' claims (lines 303, 335) have no supporting numbers or baselines.
- Case study is a single commercial collaboration (VOZIQ), with one author having collaborated with that firm (Sec. 3.1, line 240); no independence or conflict-of-interest statement appears in the text.
- Sample period, collection dates, number of posts per company, and the company list are inconsistent or missing (Sec. 3.1 lines 249 vs 261; Table 2 company names differ).
- Equations 1 to 3 are garbled and partly inconsistent (argmax used for a penalized minimization; Eq. 3 has undefined symbols). The stock-prediction component is not evaluated against any market data in the text.
- Generalization to real estate, organizations, and beauty trade fairs is asserted (abstract; Sec. 5) but not studied.
- Sec. 4 says the FMCG study used household-panel purchase data but gives no panel size, time window, or matching method for social media to purchases.
- Reference list problems: the Affin lexicon citation (Yang et al., 2016) points to a quantum-dot photonics paper; Gao et al. (2020) is a cloud-data-center deep-learning paper cited for knowledge management; Gheisari et al. 2021a and 2021b are duplicate entries; Hossain et al. (2018) is a political-sharing paper cited for review ranking. Verify before relying on any citation in this paper.
- Figures 1 to 6 are not in the text extraction; figure-only findings (Fig. 4 to Fig. 6) are described qualitatively in the text and could not be checked against images.

## Short quotes

- "the objective of this study is to check the usefulness of the development and implementation of an integrative management information system." (Sec. 1 Introduction, line 121)
- "This approach continues to be a conceptual model based on past research findings and aims to assess and improve the framework for future research." (Sec. 5 Conclusion, line 341)
- "The Wal-Mart company has the highest mention, and the next highest is Costco." (Sec. 4, line 311)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-sna#Press HCoI communities\|Press HCoI communities]] | context | Builds a term co-occurrence graph from cross-correlation of burst vectors (edge threshold 0.1) and partitions it with Louvain to find event communities; no modularity, stability or external-validity figure is reported. (Sec. 3, around Eqs. 4-5 (lines ~194-208)) | HCoI builds member press-category profiles (recency-weighted), links pairs at Pearson >= 0.6 (Malaterre's threshold, env HCOI_CORR) and runs Louvain with seed 7, keeping communities of at least 3. Homophily replication at press resolution was null. `fec/network/press_hcoi.py:22,64` | The pipeline shape is the same (correlation, threshold, Louvain), but the nodes are terms, not legislators, and the threshold (0.1 vs 0.6) is unjustified and unvalidated. That gives no grounds to retune HCOI_CORR or to read the null homophily result differently. Malaterre remains the governing source. | weak | UNVERIFIED |
| [[systems/melange-influence-engine#Salience shock (Noble-Turner going public)\|Salience shock (Noble-Turner going public)]] | suggests-extension | Defines a time-varying event hotness index He(t) = sum over bursting events of sentiment x volume, with bursts flagged by z-scores of one-day variation over trailing 5- and 25-day windows (\|z\|>30). It is proposed as a brand-value measure, and no estimates or forecast errors are reported. (Sec. 3, Eqs. 3 and 5; contributions list line ~137) | Salience parameters (partisan gap z_c, policy uncertainty psi_c, backfire b) are estimated from the static stance matrix. There is no time-series trigger for when an issue becomes salient. Go-public advice is computed per issue, not per moment. `fec/engine/salience_shock.py:29-33,93` | A burst detector on press and social volume per CU category could supply a dated salience-onset signal. Any such signal stays UNVERIFIED until it is tested against Noble-Turner style co-partisan shifts on a second path. The paper's 'sentiment x volume' must not be imported as an influence or importance score: it measures volume, not access or influence. It comes from consumer tweets about retailers, not congressional statements. | weak | UNVERIFIED |
| [[systems/melange-stance#Lexicoder LSD2015 tone intensity\|Lexicoder LSD2015 tone intensity]] | context | Scores phrase sentiment with the general-purpose Affin lexicon plus an ensemble classifier for praise vs complaint. Accuracy is described as 'highest' with no metric, and the Affin citation points to an unrelated photonics paper. (Sec. 1, around lines 125-129; Sec. 4 lines 303, 335; reference list) | Tone intensity is the LSD2015 charge (pos+neg)/words with negation handling, used as intensity only. Direction comes from the stance readers. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_lsd.R:46-59` | There is no evidence here that a general lexicon measures tone validly on congressional press releases. The open question (does LSD intensity agree with human-coded intensity?) is untouched. Rice & Zorn's case for domain dictionaries remains the relevant source. | weak | UNVERIFIED |

## Gaps and flags

- No time-series burst or onset detection on press/social volume per CU category. Salience and media-blend treat attention as static or exponentially decayed (2.0-year press, 0.5-year social half-lives), so the system cannot date when an issue spiked. The paper names the gap but does not show it matters.
- Social posts enter the media blend by platform-agnostic p80 scaling. The paper's claim that Facebook and Twitter indices differ (values only in Fig. 5) suggests platform effects could be checked. It is unvalidated, so this is a question, not evidence.
- The paper's 'highest accuracy' superiority claim has no metric, baseline or holdout. Do not cite it as validation for any sentiment or stance reader. Anything adapted from it stays UNVERIFIED until a second path reproduces it (DE-062).
- The hotness index He(t) sums sentiment x volume and is framed as a brand-value measure. Importing it as an influence, importance or closeness score would breach the wording rules (graph and volume scores are heuristic model outputs, not influence; D-025). Mention shares (Wal-Mart 35% etc.) have no stated window or denominator.
- Conflict of interest: the case-study data came from VOZIQ, a commercial firm one author collaborated with, and no independence statement appears. The company list is inconsistent across sections. Several references are miscited (Affin -> photonics paper; duplicate Gheisari entries). Verify any citation before relying on it.
- The paper does not argue for dropping regular cosponsorship edges or for per-edge volume normalization, so neither rejected SNA proposal is triggered.

Paul has not annotated this paper, and it appears in none of his component positions. The closest is his press-pillar position: he wants press statements, news cycles and social feeds, scored by sentiment, added as determinants of pathway change (the Bacik augmentation). This paper shares that aspiration in a business setting but gives no evidence that sentiment-scored social volume predicts anything, legislative or commercial. It neither supports nor undercuts his takeaway. It is a framework sketch, and it sits in the same genre as Taherdoost & Madanchian 2023, which Paul cites only for context. His notes are not made stale by it. The system's existing stance (sentiment is not stance; volume is not influence) is stricter than anything in this paper.

## Related papers

- [[taherdoost-2023-artificial-intelligence-sentiment]] — complements — Same genre: a sentiment-analysis review for competitive and business research. Both are context-only for the stance stack, and neither validates on political text.
- [[malaterre-2024-inferring-social-networks]] — same-method — Both threshold a correlation graph and run Louvain to find communities. Malaterre supplies the 0.6 threshold press_hcoi.py actually uses. Zhang uses 0.1 on burst vectors and gives no validation.
- [[hargittai-2015-bigger-always-better]] — contradicts — Hargittai shows that big data from social network sites carries sampling biases. Zhang treats 882,371 tweets as representative of consumer sentiment and never examines who posts.
- [[rice-2018-corpus-based-dictionaries]] — contradicts — Rice & Zorn argue that general sentiment lexicons misfire on specialised vocabularies. Zhang applies the general Affin lexicon to retail tweets with no domain check.
- [[steinertthrelkeld-2018-twitter-data]] — complements — Gives the methodological grounding for Twitter collection and its limits that Zhang's Twitter-API case study omits.

### Linked from
- [[taherdoost-2023-artificial-intelligence-sentiment]] — cited-by — The review cites a 2022 Inf Process Manag paper on social media analytics for competitive business decisions (ref 16). It appears to be the same work as this vault entry, though the first author is listed differently.