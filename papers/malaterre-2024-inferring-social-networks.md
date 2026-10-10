---
type: paper
title: "Inferring social networks from unstructured text data: A proof of concept detection of hidden communities of interest"
authors: ["Christophe Malaterre (UQAM, Departement de philosophie and CIRST)", "Francis Lareau (UQAM, Departement d'informatique)"]
year: "2024"
venue: "Data & Policy (Cambridge University Press), vol. 6, article e5. Received 24 May 2023, revised 30 Oct 2023, accepted 30 Nov 2023. Correction to the vault hint: the journal IS visible in the extracted text. Manuscript ID DAP_2300048 does not appear in the extracted text and could not be confirmed."
doi: "10.1017/dap.2023.48"
category: "Social Network Analysis"
systems: ["melange-legislative-data", "melange-sna", "melange-stance"]
components: ["melange-sna.press-hcoi-communities", "melange-sna.press-hcoi-communities", "melange-sna.press-hcoi-communities", "melange-stance.stance-dynamics", "melange-legislative-data.category-taxonomy", "melange-legislative-data.press-collector-cap", "melange-sna.press-coalition-ward"]
paul_annotated: true
paul_markup_count: 86
evidence_status: "verified"
source: "[[_sources/malaterre-2024-inferring-social-networks.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\inferring-social-networks-from-unstructured-text-data-a-proof-of-concept-detection-of-hidden-communities-of-interest.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\inferring-social-networks-from-unstructured-text-data-a-proof-of-concept-detection-of-hidden-communities-of-interest.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\inferring-social-networks-from-unstructured-text-data-a-proof-of-concept-detection-of-hidden-communities-of-interest.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\inferring-social-networks-from-unstructured-text-data-a-proof-of-concept-detection-of-hidden-communities-of-interest.pdf", "C:\\Users\\pgoff\\Downloads\\inferring-social-networks-from-unstructured-text-data-a-proof-of-concept-detection-of-hidden-communities-of-interest.pdf"]
sha12: ["f0f9356bb79c"]
tags: [paper, social-network-analysis]
---

# Inferring social networks from unstructured text data: A proof of concept detection of hidden communities of interest

**Christophe Malaterre (UQAM, Departement de philosophie and CIRST), Francis Lareau (UQAM, Departement d'informatique) (2024).** *Data & Policy (Cambridge University Press), vol. 6, article e5. Received 24 May 2023, revised 30 Oct 2023, accepted 30 Nov 2023. Correction to the vault hint: the journal IS visible in the extracted text. Manuscript ID DAP_2300048 does not appear in the extracted text and could not be confirmed..* DOI: [10.1017/dap.2023.48](https://doi.org/10.1017/dap.2023.48). Theme: [[themes/social-network-analysis|Social Network Analysis]].

> [!abstract] Summary
> Malaterre and Lareau (Data & Policy 6, e5, 2024) is a proof of concept for finding "hidden communities of interest" (HCoIs) when no tie data exist. They fit a 25-topic LDA to 16,917 philosophy-of-science articles (1930-2017). They average each author's topic shares within each of four roughly 21-year periods. Authors with more than 2 weighted publications are kept, which leaves at most 8,009 authors. Authors are linked when the Pearson correlation of their profiles is at least 0.6, and Louvain then finds communities. Communities in adjacent periods are matched by Hellinger distance to trace their history. Over time, modularity rises (0.427 to 0.834), density falls overall (0.167 in 1930-51 to 0.030 in 1996-2017, not monotone), and the number of communities goes from 4 to 13. The only validation is that the results agree with the known history of the field. The authors also say plainly that HCoI ties are mediated by text and are a heuristic for possible real ties, not social ties. They argue the method suits policy text but do not test it on any.
>
> For Paul's systems this paper is not new: it is the stated source of fec/network/press_hcoi.py. That script copies the pipeline almost step for step, except that it uses 20 regex-tagged CU press categories in place of LDA topics. The paper supports building the layer, but it does not support reading HCoIs as coalitions, as closeness, or as influence channels. Those are the readings the system's assumption text, Paul's annotation, and the holistic edge admission lean toward.

## Paul's annotation

From [[annotations/Annotated Bibliographies & Literature Synthesis - Curated Annotations|Annotated Bibliographies & Literature Synthesis - Curated Annotations]]:

> Malaterre, Christophe, and Francis Lareau. “Inferring social networks from unstructured text data: A proof of concept detection of hidden communities of interest.” Data &amp; Policy, vol. 6, 2024, https://doi.org/10.1017/dap.2023.48.
>
> This article focuses on the methodological approach to turning unstructured data, such as long policy memos, into refined data that can be input as vectors and edges within a social network analysis graph. The main theory emphasizes the use of shared semantic content between seemingly unrelated groups using a fitted topic model. It provides a good foundational overview of how Social Network Analysis operates and how to craft your own framework. One potential extension of the current research is to infer the level of closeness between two congressional offices based on mapping their co-occurrences within texts or on general textual similarities—requiring a temporal element. Additionally, this topic model could be domain-specific to the trade association or consulting partner assigned to it. This also mentions Louvian community detection. Finally, note how they slimmed down the articles to increase computational efficiency. The author explicitly mentions this method in its application to advocacy. Diagnosis: Priority – Very High – Foundational

From [[annotations/Social Network Analysis - Social Network Analysis - Annotated Bibliography|Social Network Analysis - Social Network Analysis - Annotated Bibliography]]:

> Malaterre, Christophe, and Francis Lareau. “Inferring social networks from unstructured text data: A proof of concept detection of hidden communities of interest.” *Data &amp*; Policy, vol. 6, 2024, [https://doi.org/10.1017/dap.2023.48](https://doi.org/10.1017/dap.2023.48).
>
> This article focuses on the methodological approach to turning unstructured data, such as long policy memos, into refined data that can be input as vectors and edges within a social network analysis graph. The main theory emphasizes the use of shared semantic content between seemingly unrelated groups using a fitted topic model. It provides a good foundational overview of how Social Network Analysis operates and how to craft your own framework. One potential extension of the current research is to infer the level of closeness between two congressional offices based on mapping their co-occurrences within texts or on general textual similarities—requiring a temporal element. Additionally, this topic model could be domain-specific to the trade association or consulting partner assigned to it. This also mentions Louvian community detection. Finally, note how they slimmed down the articles to increase computational efficiency. The author explicitly mentions this method in its application to advocacy. Diagnosis: Priority – Very High – Foundational

## Paul's markup in the PDF

- p1 Highlight: "[yellow] Paper title line" — 
- p1 Highlight: "[yellow] SNA gives insights for policymaking (abstract)" — 
- p1 Highlight: "[green] Social data for networks often unavailable" — 
- p1 Highlight: "[green] Interpreting networks relies on outside knowledge" — 
- p1 Highlight: "[yellow] Method: topic model plus actor profile correlations" — 
- p1 Highlight: "[yellow] Definition of hidden communities of interest" — 
- p1 Highlight: "[yellow] Diachronic view via networks over time periods" — 
- p1 Highlight: "[yellow] Mapping actor claims matters for policy" — 
- p1 Highlight: "[yellow] Mapping pros and cons of infrastructure projects" — 
- p1 Highlight: "[green] Computational approach using actors' texts (policy statement)" — 
- p1 Highlight: "[green] Texts used to infer underlying networks" — 
- p1 Highlight: "[pink] Main claim: text reveals latent groups of interest" — 
- p2 Highlight: "[yellow] SNA scope: influence flows among individuals, groups, institutions" — 
- p2 Highlight: "[yellow] SNA scope: evolution over time" — 
- p2 Highlight: "[yellow] SNA applied across many social structures" — 
- p2 Highlight: "[green] Network as a structure capturing a political phenomenon" — 
- p2 Highlight: "[yellow] Political science network citations (Lazer; Ward et al.; Victor et al.)" — 
- p2 Highlight: "[yellow] Citation: Praet et al. 2021" — 
- p2 Highlight: "[yellow] Advocacy networks in international relations" — 
- p2 Highlight: "[yellow] Citation: Varone et al. 2017" — 
- p2 Highlight: "[green] Latent structures and hidden colleges in science" — 
- p2 Highlight: "[yellow] Scientists' networks over problem selection and careers" — 
- p2 Highlight: "[green] Data sources for political networks (press, legislation, email, social media)" — 
- p2 Highlight: "[yellow] Interpreting networks needs supplementary information" — 
- p2 Highlight: "[green] Nodes and edges alone insufficient for structural features" — 
- p2 Highlight: "[green] Example question about an advocacy group" — 
- p2 Highlight: "[yellow] Example question about legislators' success" — 
- p2 Highlight: "[yellow] Relational networks plus metadata to interpret communities" — 
- p2 Highlight: "[yellow] Earlier work mining text with topic models for networks" — 
- p3 Highlight: "[yellow] Topic-model extensions for directional networks (social media)" — 
- p3 Highlight: "[yellow] Co-authorship topic-model extensions" — 
- p3 Highlight: "[green] Semi-supervised community detection and graph neural networks" — 
- p3 Highlight: "[yellow] Topic models as a first step to product opportunities" — 
- p3 Highlight: "[pink] Motivating case: text abundant, relational data scarce or absent" — 
- p3 Highlight: "[yellow] Shared semantic content still reveals communities" — 
- p3 Highlight: "[yellow] HCoIs reflect latent networks worth studying" — 
- p3 Highlight: "[yellow] Semantic networks in business (public-private partnerships)" — 
- p3 Highlight: "[yellow] Semantic networks infer relations from term co-occurrence" — 
- p3 Highlight: "[yellow] Health-policy vaccine-hesitant influencer example (Ruiz et al.)" — 
- p3 Highlight: "[yellow] Contribution: topic modelling plus community detection reconstructs author communities" — 
- p4 Highlight: "[green] LDA on the corpus gives topic distributions per article" — 
- p4 Highlight: "[yellow] Author correlation networks built per period" — 
- p4 Highlight: "[yellow] Corpus size: 16,917 articles" — 
- p4 Highlight: "[yellow] Eight philosophy-of-science journals" — 
- p5 Highlight: "[yellow] Multi-authored share rises from 4% to 16%" — 
- p6 Highlight: "[yellow] LDA yields 25 topic distributions over the lexicon" — 
- p7 Highlight: "[yellow] Topics grouped by inter-topic correlation" — 
- p7 Highlight: "[yellow] Topics match known research themes in philosophy of science" — 
- p7 Highlight: "[yellow] Hellinger-based topic similarity measure (Gensim)" — 
- p8 Highlight: "[purple] Table 2 header: top 10 words" — 
- p8 Highlight: "[purple] Table 2 caption: topics and keywords" — 
- p8 Highlight: "[purple] Empty highlight (no covered text)" — 
- p8 Highlight: "[yellow] Choosing k: metrics plus interpretability" — 
- p9 Highlight: "[yellow] Author topic profiles per time period" — 
- p9 Highlight: "[yellow] Term 'author topic profiles' (defined)" — 
- p9 Highlight: "[green] Community profile = averaged author profiles" — 
- p9 Highlight: "[yellow] Closest-pairing Hellinger distances across periods" — 
- p9 Highlight: "[yellow] Community evolution and relative importance over time" — 
- p9 Highlight: "[yellow] Density falls while betweenness, cohesion, modularity rise" — 
- p9 Highlight: "[yellow] Threefold rise in specialised communities" — 
- p10 Highlight: "[yellow] 1930s-40s cluster: logical positivists and Vienna circle" — 
- p11 Highlight: "[yellow] Novel community on philosophy of mind and neuroscience (1970s)" — 
- p12 Highlight: "[yellow] Empty highlight (no covered text)" — 
- p12 Highlight: "[pink] Diffuse community with a traditional philosophical standpoint" — 
- p13 Highlight: "[yellow] Community bridging probability and logic (game theory, rational choice)" — 
- p13 Highlight: "[yellow] Text-associated actors give topical proximity profiles" — 
- p14 Highlight: "[yellow] Marginalisation of the former core of philosophy of science" — 
- p14 Highlight: "[yellow] Stabilisation of communities with emerging themes" — 
- p14 Highlight: "[yellow] Knowledge-theory community" — 
- p14 Highlight: "[yellow] Genealogical analysis transferable to other contexts" — 
- p14 Highlight: "[yellow] Community topic profile as a trait for proximity over time" — 
- p15 Highlight: "[green] Findings from combined topic modelling and community detection" — 
- p15 Highlight: "[yellow] Identify communities from semantic content without social ties" — 
- p15 Highlight: "[yellow] Works with zero relationship data" — 
- p15 Highlight: "[green] Temporal dimension yields genealogies" — 
- p15 Highlight: "[yellow] Relations inferred from thematic similarity of texts" — 
- p15 Highlight: "[yellow] HCoI links mediated by texts" — 
- p15 Highlight: "[yellow] HCoI mapping as a heuristic for real ties" — 
- p15 Highlight: "[yellow] Topic-based community profiles aid interpretation" — 
- p16 Highlight: "[yellow] Representativeness is crucial for conclusions" — 
- p16 Highlight: "[yellow] Monolingual corpus limitation" — 
- p16 Highlight: "[green] Other topic models possible if assignments are continuous" — 
- p16 Highlight: "[yellow] Political legitimacy from data-driven policy" — 
- p16 Highlight: "[yellow] Policy a posteriori evaluation via aggregated perceptions" — 
- p16 Highlight: "[purple] Term: topic modelling and community detection methods" — 
- p16 Highlight: "[yellow] Results consistent with expert knowledge lend credibility" — 

## Study

**Question.** Can communities of actors who share similar semantic content ("hidden communities of interest", HCoIs) be recovered from the texts those actors produce when relational (social tie) data are absent, and can their evolution over time be traced as community genealogies?

**Theory.** Actors' shared topical content is treated as a relational proxy: "who-talks-about-the-same-things-as-who" (Section 4, p.15). The HCoI networks are described as capturing ties mediated by texts rather than actual social ties. Grounding is in SNA, semantic network approaches (Carley 1993; Danowski 2011), and topic modelling. No formal theory of influence, persuasion or behaviour is stated.

**Data.** Corpus of 16,917 full-text English-language research articles from eight philosophy-of-science journals (Table 1, p.5), published 1930-2017. Assembled in Malaterre and Lareau (2022). Unit of analysis for the topic model is the article; the network nodes are authors. Authors N = 8,009, with co-authored articles split equally across authors. Four periods of roughly 21 years each: 1930-1951, 1952-1973, 1974-1995, 1996-2017 (p.4). Article counts rise from 1,575 to 8,300 (5.3-fold); author count rises about eightfold; multi-authored share rises from 4% to 16% (p.5). About 65% of authors publish only once; the share of new authors per period is above 80% but declining (p.5-6). Lexicon after POS filtering (nouns, verbs, adverbs, adjectives; TreeTagger, Penn tags) and removal of words in fewer than 50 sentences: 23,672 terms (p.4, p.14). Abstract N = 16,917 refers to articles, not authors.

**Method.** 1) Latent Dirichlet allocation (Blei et al. 2003; Griffiths and Steyvers 2004) on full articles via Gensim, k = 25 topics. k chosen as a compromise between Cv coherence (Roder et al. 2015) over k = 5-100 and manual inspection of top words; k above 35 gave no coherence gain (p.6, Fig. 3). Topics labelled manually into groups A-H (p.7-8, Table 2). 2) Each article's topic distribution is averaged per author per period, weighted by authorship share, giving author topic profiles (p.9). 3) Pearson correlations between author profiles within each period; correlation networks built in Gephi with default Louvain community detection. Authors with weighted publications above 2 retained; correlation threshold 0.6 (p.9). 4) Community topic profile = average of member author profiles (p.9). 5) Hellinger distances between community profiles across adjacent periods; closest pairings used for genealogies (p.9, p.13-14, Fig. 9). Descriptive network statistics computed in Gephi (Table 3, p.10). Inter-topic similarity = 1 minus Hellinger distance (p.7, Fig. 4).

**Identification.** No causal identification strategy; the paper is an unsupervised method. "Identification" of an HCoI means a Louvain community in a thresholded author correlation network. Validation is qualitative: communities and their change over time are judged consistent with the known history of the field (logical positivism; emergence of philosophy of biology in the 1970s; p.15, and Conclusion p.16). No ground-truth labels or held-out evaluation are reported.

**Scope.** Population: authors of English-language articles in eight philosophy-of-science journals (including Philosophy of Science, Synthese, Erkenntnis, BJPS; Table 1). Era: 1930-2017. Not a policy corpus. The authors argue the approach applies to policy text (public health, international relations, voting, e-petitioning, citizen social-media text; p.3, p.16) but do not test it on policy data. The corpus is claimed to be representative of the discipline, while acknowledging that other venues and non-English publications are excluded (p.4).

## Key findings

- A 25-topic LDA plus author correlation and Louvain pipeline recovers interpretable author communities with no relational data. Communities are labelled by their topic profiles. k = 25; 16,917 articles; 8,009 authors; correlation threshold 0.6; authors with weighted publications > 2 kept (Abstract p.1; Section 2.2 p.6-9; Section 3.1 p.9-12)
- Network structure changes across eight decades. Density falls overall (not monotone) while modularity rises. Authors describe betweenness, cohesion and modularity as increasing, but cohesion is not monotone (see Table 3 check below). Density 0.167 (1930-51), 0.057 (1952-73), 0.027 (1974-95), 0.030 (1996-2017). Modularity 0.427, 0.674, 0.779, 0.834. Number of communities 4, 7, 13, 13. Giant-component node coverage 96.45%, 93.50%, 98.83%, 98.77%. (Section 3.1, text p.9 (lines 419-420); Table 3 p.10)
- The number of specialised communities roughly triples over the period, from 4 to 13. 4 communities (1930-51) to 13 (1996-2017), about 3.3-fold; authors describe this as threefold (Section 3.1 p.9 (line 429); Table 3 p.10)
- Corpus growth is driven partly by growing co-authorship: multi-authored share rises, and articles per author stay roughly constant. Articles 1,575 to 8,300 (5.3-fold); authors about 8-fold; articles per author about 2 in each period; multi-authored share 4% to 16% (Section 2.1 p.5)
- Topics map onto known philosophy-of-science themes, and inter-topic similarity is low overall. Only B-Arguments and H-History show notable similarity, about 0.4 with the other topics in their clusters. 25 topics in 8 groups (A-H); similarities near 0.4 for B-Arguments and H-History clusters (Section 2.2 p.7 (lines 283-300), Fig. 4 p.7)
- Genealogy: some communities persist across periods (logic and language, epistemology, philosophy of biology in later periods), some split or merge, and several appear with no clear predecessor. The 1974-95 period contains four novel communities. Four novel communities appear in 1974-95 without clear filiation (3c, 3f, 3h, 3i); philosophy of biology 2d shows little continuity with earlier communities; 1930s logical positivist cluster 1a maps to 2b (Section 3.2 p.13-14 (lines 577-621), Fig. 9 p.13)
- The authors read the findings as stabilisation after an early growth and diversification phase, and report that the output agrees with known episodes of the field. This is the only validation offered. No quantitative validation figure given (Discussion p.15 (lines 638-644); Conclusion p.16 (lines 728-729))
- The method is computationally cheaper than semantic-network comparison. It uses three matrices, avoiding 8,009 semantic matrices of 23,672 x 23,672 and their pairwise distances. Matrices: author x topic 8,009 x 25; document x topic 16,917 x 25; topic x term 25 x 23,672. Avoided: 8,009 matrices and 8,009 x 8,009 / 2 pairwise comparisons (Discussion p.15 (lines 664-676))

## Limitations

- Authors state the methods work best with a monolingual corpus. For multilingual corpora they suggest machine-translating non-English texts into English (p.16, lines 688-693).
- Authors state that the quality of HCoI networks depends on corpus representativeness, which is crucial for conclusions (p.16, lines 682-684). They acknowledge that philosophy of science is also published in other venues and in other languages (p.4).
- Topic model choice: LDA was used, but any topic model is acceptable if topic assignments are continuous probabilities rather than crisp (p.16, lines 694-699).
- The number of topics k is a compromise between coherence and human interpretability. Metrics alone do not settle k (p.6-8, lines 264-268, 311-313).
- Topics depend on the modelled text, and corpus size and structure affect the choice of k and of units of analysis (p.8, lines 307-311).
- HCoI networks are not social networks. Ties are mediated by texts, so they are a heuristic for possible real ties, not a substitute for them (p.15, lines 652-663).
- Policy applicability is asserted but not tested in this paper (p.3, p.13, p.16).
- Validation is qualitative and rests on consistency with expert knowledge of the field (p.15-16).

## Short quotes

- "In short, HCoI’s are about who-talks-about-the-same-thing-as-who." (Section 4, p.15 (lines 655-657))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-sna#Press HCoI communities\|Press HCoI communities]] | method-source | LDA k=25 on 16,917 articles; author topic profiles per roughly 21-year period, weighted by authorship share; keep authors with weighted publications above 2; Pearson r at least 0.6; Louvain (Gephi default); community profile = average of member profiles; genealogy by closest Hellinger pairing between adjacent periods; modularity 0.427 to 0.834 and communities 4 to 13. (Section 2.2 p.6-9 (text lines 260-403); Table 3 p.10 (lines 449-484); Section 3.2 p.13-14) | press_hcoi.py follows the same steps but uses 20 curated regex-tagged press categories, not 19, in place of LDA topics. Each item's weight is split evenly over its labels and weighted by recency. r cutoff 0.6 (HCOI_CORR); MIN_W 3.0 as the transient-author filter; Louvain seed 7; communities under 3 members dropped; one median-date split into two periods; Hellinger matching; PERSISTS / drifts / NOVEL cutoffs 0.25 and 0.45. Stored data/stance/press_hcoi.json: 155 members profiled, 6 communities, 1900 edges, 20 categories. `fec/network/press_hcoi.py:22-27,37-77,102-116` | Mechanics carry over. The evidence does not: the paper covers an 88-year academic corpus in four periods with over 80 percent new authors per period and only qualitative validation. Here the split is two half-periods of one press stream, so NOVEL and PERSISTS labels reflect the script's own thresholds. Quote member denominators as 155 profiled, not the 385 in the script docstring, which is stale. | strong | VERIFIED-SOFTENED |
| [[systems/melange-sna#Press HCoI communities\|Press HCoI communities]] | qualifies | HCoIs are about who-talks-about-the-same-things-as-who. Links are mediated by texts, are not social ties, and serve as a heuristic for possible real ties. (The analyst gloss that communities are not labelled from alignment is not stated in the paper.) (Section 4 Discussion p.15 (text lines 652-663)) | profiles() in press_hcoi.py counts attention across categories only and never reads the stance column, so a community can in principle hold members on both sides of an issue. The coalition assumption text attributed to the paper sits in a component file that was not opened; the paper does not support reading HCoIs as coalitions. The 49 percent co-movement null is not reproduced (see the stance bearing). `fec/network/press_hcoi.py:37-51` | The attention-not-alignment limit is definitional and transfers fully. Reading an HCoI as a coalition needs a signed profile (attention times stance), which neither the paper nor press_hcoi.py builds. | strong | VERIFIED-SOFTENED |
| [[systems/melange-sna#Press HCoI communities\|Press HCoI communities]] | qualifies | The paper offers no evidence that text-similarity ties carry influence or transmission; it frames HCoI mapping as a heuristic for actual social connections that would still need confirmation. (p.15 (text lines 660-663); Conclusion p.16 (lines 738-741)) | build_multilayer_network.py admits HCoI edges at weight 0.6 x r x HCOI_W. HCOI_W defaults to 0.2, so 0.12 is the ceiling at r = 1 (a maximum from the formula, not a clamp in code), about 0.02 kernel probability per step. The CU config admits only edges touching a cosponsor-isolated member; the holistic dominance rule admits HCoI edges generally. The 857-edge holistic count and the 4 of 41 reconnection count were not reproduced. The stored press_hcoi.json D4 block reads 14 isolated members and 0 placed in a community. `fec/network/build_multilayer_network.py:10-25 (header), 71-82 (admit rule), 130-140 (HCoI branch)` | Using HCoI edges as a low-weight reachability patch fits the paper's heuristic framing. Treating them as transmission edges in cascade, coalition or route outputs goes beyond what the paper tested, and any output depending on them must say it rests on a topical-similarity heuristic. The 2026-08-18 ruling is cited only in code comments and was not checked. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-stance#Stance dynamics and neighbour imputation\|Stance dynamics and neighbour imputation]] | supports | HCoI ties are links from thematic similarity of texts, mediated by the texts rather than social ties. The paper does not discuss stance, agreement or change in agreement. (p.15 (text lines 652-663)) | stance_dynamics.py runs a neighbour co-movement test on HCoI same-community pairs (lines 111-121). Imputation proposals use only cosponsor adjacency; hadj is used only in the co-movement test. Thresholds: at least 5 neighbours (SD_MINNB) and confidence at least 0.75 (SD_CONF). The stored stance_dynamics.json co_movement block reads same 134, opposite 122, which is 52 percent with no layer label, so the 49 percent null is not reproduced. `fec/stance/stance_dynamics.py:94-124,132` | The paper's silence on stance supports keeping HCoI out of stance imputation only as an argument from the construct, not from the paper's evidence. The null result needs a labelled layer in the output before it is cited. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-legislative-data#Release category taxonomy and billstatus synthetic rows\|Release category taxonomy and billstatus synthetic rows]] | suggests-extension | k was chosen by combining Cv coherence (scanned over k = 5 to 100, with no coherence gain above 35) and manual reading of top words; any topic model is acceptable if assignments are continuous probabilities rather than crisp. (p.6-8 (text lines 264-267, 311-313); p.16 (lines 694-699)) | The HCoI profile input is a regex categorisation into 20 CU press categories, not 19: 20 entries in the CATEGORIES block of cu_press_release_scraper.R and 20 categories in the stored press_hcoi.json. Each item's labels are crisp and split evenly (press_hcoi.py lines 25-27, 45). No precision check against human coding and no coherence or overlap check on the categories was found. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R:1739-1847; fec/network/press_hcoi.py:25-27,45` | The continuous-assignment requirement is met only loosely: member profiles are continuous after averaging, item tags are crisp. A coherence-selected LDA on the same corpus or a precision sample of the regex tags would show whether communities reflect the taxonomy or the text. Any result stays a diagnostic until reproduced. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-legislative-data#Signal-prioritized press collection cap\|Signal-prioritized press collection cap]] | qualifies | The quality of an HCoI network depends on the representativeness of the corpus, which is crucial for conclusions. The authors also flag the venues and languages they left out. (p.4 (text lines 188-193); p.16 (lines 682-684)) | cu_text_collector.R caps each member at 1000 URLs by default (CU_TC_CAP, line 128), taking CU-candidate titled pages first and a sample of the rest. The coverage percentages in the analyst output (D 92.5 vs R 87; Senate 83 vs House 96) were not located in the code and were not verified. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_text_collector.R:112-128` | The caution applies. Before reading HCoI membership substantively, check party and chamber gaps in who is profiled (155 members profiled in the current artifact). Any figure should name its window and denominator. | weak | VERIFIED-SOFTENED |
| [[systems/melange-sna#Press-lean coalition detection (Ward) and bloc profiles\|Press-lean coalition detection (Ward) and bloc profiles]] | suggests-extension | Communities in adjacent periods are matched by closest Hellinger distance to trace persistence, splits and novel communities; four novel communities appear in 1974-95 with no clear filiation. (Section 3.2 p.13-14 (text lines 577-601); Fig. 9) | Ward.D2 clustering on z-scored category profiles plus a lean feature weighted 3.0 (COALITION_LEAN_WEIGHT, line 3089); minimum 6 members (line 3085). When no k is passed, k = max(2, min(8, round(sqrt(n/2)))), and the call at line 3327 passes none, so the bloc count is at most 8, not 9. Bloc labels come from mean aggregate lean (pro-credit-union, hostile-to-credit-union, or mixed), so the claim that all blocs are mislabelled as pro-CU is not supported by the code. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R:3085,3089,3327 and label code near 3186-3196` | The genealogy step would answer the stability question only if blocs were re-run per period and matched by Hellinger distance. Unlike HCoIs these blocs include stance lean, so a genealogy here would track alignment. The paper's evidence for the step is qualitative, so this stays a weak bearing. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The summary says press_hcoi.py was built on 2026-07-01. Git history for fec/network/press_hcoi.py starts at the 2026-07-02 baseline commit 62a209e2f, so the date is unconfirmed.
> - Summary issue: The 19 regex-tagged categories (summary, gap 3, bearing 5) are wrong: the R source and data/stance/press_hcoi.json both have 20 categories.
> - Summary issue: The HCoI stance co-movement null (49 percent) is not reproduced: stance_dynamics.json co_movement reads same 134, opposite 122 (52 percent), with no layer label.
> - Summary issue: The recovery figure of 4 of 41 cosponsor-isolated members is not reproduced from the current press_hcoi.json, whose D4 block reads 14 isolated and 0 placed in a community. Name the run or admission count before quoting it.
> - Summary issue: Gap 5 refers to 8 communities; the current press_hcoi.json has 6 (sizes 50, 42, 20, 19, 11, 7).
> - Summary issue: The 385-member denominator in the press_hcoi.py docstring is stale; the current artifact profiles 155 members (MIN_W 3.0).
> - Summary issue: The summary says density falls from 0.167 to 0.030, but density is not monotone (0.027 in 1974-95, then 0.030). Table 3 cohesion values (31,558; 18,876; 25,465; 41,115) and betweenness values cannot match their labels at N of about 8,009: density 0.167 implies average degree near 1,300. Do not quote those table values without checking the source table.
> - Summary issue: Gap 4 and the Ward bloc count of 9 are wrong: the default caps the bloc count at 8.

## Gaps and flags

- The HCoI layer has never been checked against a quantitative criterion. The open question of whether press-attention communities predict within-community cosponsorship above chance has not been answered. The paper also validates only qualitatively, so it offers no bar to borrow.
- There is no signed HCoI variant (attention times stance sign). The current communities cannot tell allies from opponents who fight over the same issue, such as the H-5 CU-Bank defensive-fight cluster.
- The 20-category regex taxonomy that feeds the profiles has no measured precision and no counterpart to the paper's coherence-based choice of k. The paper's requirement that topic assignments be continuous is only partly met, because each item's tags are crisp and split evenly.
- The genealogy uses one median-date split into two periods, with PERSISTS / drifts / NOVEL cutoffs (0.25 / 0.45) that are not anchored in the paper. There is also no member-overlap check alongside the profile distance.
- Louvain runs once with a single seed (7), and there is no seed or resolution stability check on the 6 communities in the stored artifact.
- There is no representativeness audit of who gets profiled (MIN_W 3.0) by party or chamber, even though collection coverage differs (D 92.5 vs R 87%; Senate 83 vs House 96%).
- The component's assumption text, 'Shared issue attention indicates coalition (Malaterre & Lareau 2024)', overstates the paper. The paper says HCoI ties are mediated by text, are not social ties, and are only a heuristic for possible real ties. Under D-025, HCoI communities and edges must be described as topical-similarity heuristics, never as coalition, closeness, or access.
- Paul's takeaway, 'infer closeness between two congressional offices from co-occurrence or textual similarity', uses closeness language that D-025 bars and that the paper itself disclaims. A safer wording is 'topical-similarity heuristic between offices'.
- Paul's note that the paper 'explicitly applies the method to advocacy' is not accurate. The paper argues the method could apply to policy and advocacy, but it tests only a philosophy-of-science corpus.
- Under the holistic configs, the dominance admission rule (Paul's 2026-08-18 ruling) puts HCoI similarity edges into the cascade kernel (the 857-edge count was not reproduced). The paper gives no evidence that such ties transmit influence. This is flagged as beyond the paper's evidence, not as a recommendation to reverse the ruling.
- The PERSISTS / drifts / NOVEL genealogy labels come from the system's own thresholds, not from the paper. Treat them as descriptive labels only.
- Caution when quoting the paper: its text says betweenness, cohesion and modularity all increase, but cohesion in Table 3 is not monotone. Quote the table values, not the prose.
- The paper does not argue for dropping regular cosponsorship edges or for per-edge volume normalization, so neither rejected SNA proposal is implicated. LDA identity sources are also not involved.

Paul's annotation agrees with the paper on method: shared semantic content becomes vectors, then correlation edges, then Louvain, with a time element. The system has this in place (fec/network/press_hcoi.py) as a two-period Hellinger genealogy, so his note is stale. It still reads as a future extension, but it is built. His idea of a domain-specific topic model per trade association or client was carried out in a different form: a curated 20-category CU taxonomy instead of a per-client LDA, and that taxonomy has never been validated. Two parts of his reading go beyond the paper. One is the framing of office 'closeness', which conflicts with D-025 and with the authors' own heuristic caveat. The other is the claim that the paper 'applies' the method to advocacy, when it only argues that it could. His note that the authors 'slimmed down the articles' is partly right. Their efficiency comes from part-of-speech and frequency filtering of the lexicon, and from working with three small matrices instead of 8,009 semantic matrices.

## Related papers

- [[ardekani-2026-computational-graph-theoretic]] — same-method — Paul's cross-reference notes for Malaterre list Ardekani et al. for Louvain community detection. Ardekani builds communities from a relational legislative citation network; Malaterre builds them from text similarity alone. Link verified.
- [[aref-2021-identifying-hidden-coalitions]] — complements — Aref partitions a signed network under generalized balance, which encodes agreement and disagreement. HCoI attention profiles carry no sign, so the two are complementary on alignment. The system counterpart named in the analyst output was not checked.
- [[pritoni-2025-lobbying-coalitions-determinants]] — complements — Pritoni's systematic review covers determinants of lobbying coalitions, including issue salience and brokers (both recur in the item). That is the coalition theory the HCoI-as-coalition reading would need, and Malaterre does not supply it.
- [[zhou-2026-measuring-stance-dynamics]] — complements — Zhou models stance change on temporal interaction graphs (user-hashtag and user-user). Topical-similarity graphs do not carry the communication ties that modelling assumes. The 49 percent null the analyst cites is not reproduced, so the link should not lean on it.
- [[steinertthrelkeld-2018-twitter-data]] — complements — Both are text-data resources for social scientists. Steinert-Threlkeld covers Twitter data acquisition and processing and does not treat politicians' text as relational data; representativeness is mentioned only briefly. Use only for data-collection caveats.
- [[kim-2020-mapping-political-communities]] — complements — Kim infers latent community memberships from bipartite lobbying networks; Malaterre infers communities from text alone. The two partitions could be compared on the same members as independent views.

### Linked from
- [[joo-2022-image-data-automated]] — complements — Both papers were ingested in the same 2026-07-01 wave and both were ported to press text (Malaterre to HCoI shared-attention communities, Joo to duplication-rate frame alignment), so both share the source-author attribution exposure.
- [[zareie-2020-similarity-based-link]] — complements — Malaterre infers ties from Pearson correlation of LDA topic profiles at a 0.6 threshold and then runs Louvain, the recipe press_hcoi.py implements. Zareie also uses Pearson correlation, but on structural neighbourhood vectors, and multiplies it by a direct common-neighbour term. The overlap is technique-level, not the same method.
- [[baptista-2022-universal-multilayer-network]] — complements — Item exists. Baptista does not cite Malaterre. Analyst link only: the HCoI layer's added value is the kind of layer Baptista's overlap and ablation logic would test.
- [[haythornthwaite-1996-social-network-analysis]] — complements — press_hcoi.py takes its method from Malaterre and Lareau (2024), which correlates text profiles. The article's relational view bears on attribute-based grouping, but it does not discuss text profiles, so no direct contradiction is shown.
- [[zhang-2022-data-assisted-social]] — same-method — Both threshold a correlation graph and run Louvain to find communities. Malaterre supplies the 0.6 threshold press_hcoi.py actually uses. Zhang uses 0.1 on burst vectors and gives no validation.