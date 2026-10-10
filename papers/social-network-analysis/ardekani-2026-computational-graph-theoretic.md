---
type: paper
title: "Computational and Graph-Theoretic Analysis of Legislative Networks: New Zealand's Mental Health Act as a Case Study"
authors: ["Iman Ardekani", "Maryam Ildoromi", "Neda Sakhaee", "Sewmini Gunawardhana", "Parmida Raeis"]
year: "2026"
venue: "Information (MDPI), vol. 17, no. 2, article 161; received 18 Dec 2025, accepted 3 Feb 2026, published 5 Feb 2026. Metadata hint confirmed."
doi: "10.3390/info17020161"
category: "Social Network Analysis"
systems: ["melange-sna", "melange-staffer-network"]
components: ["melange-sna.katz-bonacich-benchmark", "melange-sna.press-hcoi-communities", "melange-sna.sector-networks", "melange-staffer-network.layer-balanced-pagerank"]
paul_annotated: true
paul_markup_count: 116
evidence_status: "verified"
source: "[[_sources/ardekani-2026-computational-graph-theoretic.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\information-17-00161-v2.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\information-17-00161-v2.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\information-17-00161-v2.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\information-17-00161-v2.pdf"]
sha12: ["d3b90168ce7a"]
tags: [paper, social-network-analysis, type/paper, theme/social-network-analysis, evidence/verified]
---

# Computational and Graph-Theoretic Analysis of Legislative Networks: New Zealand's Mental Health Act as a Case Study

**Iman Ardekani, Maryam Ildoromi, Neda Sakhaee, Sewmini Gunawardhana, Parmida Raeis (2026).** *Information (MDPI), vol. 17, no. 2, article 161; received 18 Dec 2025, accepted 3 Feb 2026, published 5 Feb 2026. Metadata hint confirmed..* DOI: [10.3390/info17020161](https://doi.org/10.3390/info17020161). Theme: [[themes/social-network-analysis|Social Network Analysis]].

> [!abstract] Summary
> Ardekani et al. build a citation network of New Zealand Acts. It is seeded on the Mental Health Act 1992 and grows breadth-first for three layers: 27, then 2,159, then 6,021 of 17,709 Acts. Edges are amendment, repeal and citation relations pulled from XML with hand-written regex rules. The rules beat an LLM extractor: precision 0.96 and recall 0.89 in Table 1, against 0.86 and 0.78 averaged over 10 LLM runs. The text gives 93% precision for the rules, which does not match the table.
>
> Nodes are ranked by Katz prestige, which counts incoming paths. The seed Act ranks 2nd in the 2-layer graph and drops out of the top 50 at 3 layers. The authors call it "locally central but not systemically dominant" and say part of the 2-layer rank comes from the seed being the root of its own graph. Louvain finds 14 communities at modularity 0.393 and 15 at 0.392. These communities match committee keyword sets only weakly (community-level Jaccard 0.10-0.66; the macro-community Maori Affairs score is 0.67). There are no significance tests, no time dimension, and alpha, the Louvain resolution and the Louvain seed are not reported.
>
> For Paul's systems the paper adds no causal evidence, but it sharpens two methodological points. First, centrality computed on a graph cut to one topic depends on where the cut is made. That bears on the Katz-Bonacich benchmark, which runs on the CU-only kernel, and on comparisons between sector networks. Second, a Louvain partition should come with its modularity and an external alignment check. Press HCoI reports neither. All nodes in the paper are statutes, not legislators. That limits every transfer to Melange's member graphs to the method, not the findings.

## Paul's annotation

From [[annotations/Annotated Bibliographies & Literature Synthesis - Curated Annotations|Annotated Bibliographies & Literature Synthesis - Curated Annotations]]:

> Ardekani, Iman, et al. “Computational and Graph-Theoretic Analysis of Legislative Networks: New Zealand’s Mental Health Act as a Case Study.” MDPI, Multidisciplinary Digital Publishing Institute, 5 Feb. 2026, www.mdpi.com/2078-2489/17/2/161.
>
> The article presents a case study of a computational framework used to analyze legislative citations for a single act in the New Zealand Parliament. This framework can be applied to the present research regarding how they mathematically compute the various nodes, vertices, and vectors. Additionally, machine learning methods used in the article, such as Named Entity Recognition (NER) or IE, can be applied to data extraction and processing in the applied product. Finally, the combination of structural graph analytics and semantic representation offers a potential layer to the ultimate analysis. The main drawback of this article is that it focuses on the legal citations of a bill and does not track key elements of lobbying and consulting, such as co-sponsorships, monetary congestion, or other variables. More thought will be needed as to how this can be practically applied to the intended product. However, the article goes into depth on the Louvian method, which can be used by communities and coalitions. Additional research into different forms of centrality is needed.

From [[annotations/Social Network Analysis - Social Network Analysis - Annotated Bibliography|Social Network Analysis - Social Network Analysis - Annotated Bibliography]]:

> Ardekani, Iman, et al. “Computational and Graph-Theoretic Analysis of Legislative Networks: New Zealand’s Mental Health Act as a Case Study.” *MDPI*, Multidisciplinary Digital Publishing Institute, 5 Feb. 2026, [www.mdpi.com/2078-2489/17/2/161](http://www.mdpi.com/2078-2489/17/2/161).
>
> The article presents a case study of a computational framework used to analyze legislative citations for a single act in the New Zealand Parliament. This framework can be applied to the present research regarding how they mathematically compute the various nodes, vertices, and vectors. Additionally, machine learning methods used in the article, such as Named Entity Recognition (NER) or IE, can be applied to data extraction and processing in the applied product. Finally, the combination of structural graph analytics and semantic representation offers a potential layer to the ultimate analysis. The main drawback of this article is that it focuses on the legal citations of a bill and does not track key elements of lobbying and consulting, such as co-sponsorships, monetary congestion, or other variables. More thought will be needed as to how this can be practically applied to the intended product. However, the article goes into depth on the Louvian method, which can be used by communities and coalitions. Additional research into different forms of centrality is needed.

## Paul's markup in the PDF

- p1 Highlight (yellow): "Abstract: framework for building a focal legislative citation network (paraphrased locator)" — 
- p1 Highlight (yellow): "Abstract: building and analysing the focal citation network" — 
- p1 Highlight (yellow): "Abstract: how the seed Act's influence changes with network radius" — 
- p1 Highlight (green): "Abstract: LLM-assisted topic modelling turned into a TF-IDF document-term matrix" — 
- p1 Highlight (yellow): "Intro: legal instruments linked by citations, amendments, repeals, judicial interpretation" — 
- p1 Highlight (yellow): "Intro: formal graph definition G=(V,E)" — 
- p1 Highlight (yellow): "Intro: vertices are instruments, edges are legal relationships" — 
- p2 Highlight (yellow): "Intro: attributes too rich for simple numeric or categorical encoding" — 
- p2 Highlight (green): "Intro: semantically rich graphs called knowledge graphs" — 
- p2 Highlight (pink): "Intro: simplifying to plain graphs drops the legal-text layer" — 
- p2 Highlight (yellow): "Intro: documents now processable across formats and quality levels" — 
- p2 Highlight (green): "Intro: jurisdictions publish machine-readable legislation" — 
- p2 FreeText (margin note): "No highlighted text. Margin note sits beside the paragraph on machine-readable publication." — Same for the US?
- p2 Highlight (yellow): "Intro: vision-language models beat traditional OCR" — 
- p2 Highlight (yellow): "Intro: NER as the first processing step" — 
- p2 Highlight (yellow): "Intro: NER entity types and IE relation types" — 
- p2 Highlight (yellow): "Intro: rule-based pipelines are brittle and costly to maintain" — 
- p2 Highlight (yellow): "Intro: combining semantics with graph analytics" — 
- p2 Highlight (yellow): "Intro: LLM prompt-based extraction and adaptability" — 
- p3 Highlight (yellow): "Intro: graph analysis after NER/IE construction" — 
- p3 Highlight (yellow): "Intro: centrality, community detection and semantics combined" — 
- p3 Highlight (yellow): "Intro: full-network analysis overlooks domain-specific Acts" — 
- p3 Highlight (yellow): "Intro: prior work neglects the semantic content of legal text" — 
- p3 Highlight (yellow): "Intro: limits on local-environment and semantic-similarity analysis" — 
- p4 Highlight (yellow): "Contribution 1 heading: focal framework for network construction" — 
- p4 Highlight (yellow): "Contribution 1: depth-limited expansion" — 
- p4 Highlight (yellow): "Contribution 1: layer-by-layer expansion method" — 
- p4 Highlight (yellow): "Contribution 2: deterministic rule-based extraction pipeline" — 
- p4 Highlight (yellow): "Contribution 2: XML-formatted input documents" — 
- p4 Highlight (yellow): "Contribution 3: LLM-assisted LDA gives higher-coherence topics" — 
- p4 Highlight (yellow): "Contribution 4: Louvain communities with LDA embeddings" — 
- p4 Highlight (yellow): "Contribution 4: thematic clusters checked against committee domains" — 
- p4 Highlight (yellow): "Contribution 5: generalisability to any corpus or jurisdiction" — 
- p4 Highlight (yellow): "Sec. 2 opening: holistic analysis may miss citation patterns around a focal Act" — 
- p4 Highlight (yellow): "Sec. 2 opening: astronomy analogy for focal, zoomed-in analysis" — 
- p5 Highlight (yellow): "Sec. 2 analogy: faint sources hidden without targeted magnification" — 
- p5 Highlight (yellow): "Sec. 2: isolating the immediate and extended neighbourhood of an Act" — 
- p5 Highlight (yellow): "Sec. 2: graph and semantic attributes of the neighbourhood" — 
- p5 Highlight (yellow): "Sec. 2: building the whole corpus network is costly and mostly irrelevant" — 
- p5 Highlight (yellow): "Sec. 2: three-layer limit for tractability and relevance" — 
- p6 Highlight (yellow): "Sec. 2.1: Act counts from index queries and XML verification" — 
- p6 Highlight (yellow): "Sec. 2.1: legal-domain tokenisation keeps references atomic" — 
- p6 Highlight (yellow): "Sec. 2.2: LLM run-to-run stochasticity observed" — 
- p6 Highlight (yellow): "Sec. 2.2: LLM prompt cues for amendment, repeal and citation" — 
- p7 Highlight (yellow): "Sec. 2.2: relations define the edges of the graph" — 
- p7 Highlight (yellow): "Sec. 2.2: metadata stored as node attributes" — 
- p7 Highlight (yellow): "Sec. 2.3: rule-based extraction precision result" — 
- p7 Highlight (yellow): "Sec. 2.3: LLM method run ten times and averaged" — 
- p7 Highlight (yellow): "Sec. 2.3: definition of true positives" — 
- p7 Highlight (yellow): "Sec. 2.3: definition of false negatives" — 
- p7 Highlight (yellow): "Sec. 2.3: definition of false positives" — 
- p7 Highlight (yellow): "Sec. 2.3: precision formula" — 
- p7 Highlight (yellow): "Sec. 2.3: recall formula" — 
- p8 Highlight (yellow): "Sec. 2.3: why extraction accuracy matters for Katz, communities and topics" — 
- p8 Highlight (yellow): "Sec. 2.4: focal Act leaves top 50 beyond layer three" — 
- p8 Highlight (yellow): "Sec. 2.4: three-layer limit is a trade-off" — 
- p9 Highlight (yellow): "Sec. 2.5: LDA used for both topic models" — 
- p9 Highlight (yellow): "Sec. 2.5.1 rule-based LDA: lowercasing step" — 
- p9 Highlight (yellow): "Sec. 2.5.1 rule-based LDA: TF-IDF document-term matrix step" — 
- p10 Highlight (yellow): "Sec. 2.5.2: two semantic artefacts per Act" — 
- p10 Highlight (yellow): "Sec. 2.5.2: LLM used only for node-level abstraction" — 
- p10 Highlight (yellow): "Sec. 2.5.2: deterministic, rule-based graph construction" — 
- p10 Highlight (yellow): "Sec. 2.5.2: rule-based method reference" — 
- p10 Highlight (yellow): "Sec. 2.5.2: graph analyses reproducible and unaffected by LLM outputs" — 
- p10 Highlight (yellow): "Prompt: system role line" — 
- p10 Highlight (yellow): "Prompt: exclude procedural and drafting language" — 
- p10 Highlight (yellow): "Prompt: 'System instruction' label" — 
- p11 Highlight (yellow): "Prompt: 150-word summary limit" — 
- p11 Highlight (yellow): "Prompt: fragment of prompt numbering" — 
- p11 Highlight (yellow): "Prompt: 10-15 keywords instruction" — 
- p11 Highlight (yellow): "Prompt: exclude structural and procedural terms" — 
- p11 Highlight (yellow): "Prompt: citation boilerplate exclusion" — 
- p11 Highlight (yellow): "Sec. 2.5.3: why the Mental Health Act is the case study" — 
- p11 Highlight (yellow): "Sec. 2.5.3: Table 3 top topics for LLM and rule-based LDA" — 
- p11 Highlight (yellow): "Sec. 2.5.3: rule-based topics keep procedural terms" — 
- p12 Highlight (yellow): "Sec. 3: Katz counts all directed paths with down-weighting of longer ones" — 
- p12 Highlight (yellow): "Sec. 3: closeness centrality unstable with disconnected components" — 
- p12 Highlight (yellow): "Sec. 3: Katz suited to procedural importance (citing [2])" — 
- p12 Highlight (yellow): "Sec. 3: PageRank sensitivity to network growth (citing [35])" — 
- p12 Highlight (yellow): "Sec. 3: prestige from incoming links from highly connected nodes" — 
- p13 Highlight (yellow): "Sec. 3: convergence condition on the attenuation factor" — 
- p13 Highlight (yellow): "Sec. 3: alternative hubs and indirect pathways emerge in larger networks" — 
- p14 Highlight (yellow): "Sec. 3: Legislation Act 2019 and Secondary Legislation Act 2021 stay top-ranked" — 
- p14 Highlight (yellow): "Sec. 3: sharp decline suggests centrality is primarily local" — 
- p14 Highlight (yellow): "Sec. 3: broader influence diminishes as scope grows" — 
- p14 Highlight (yellow): "Sec. 3: conclusion that the Act is locally central, not systemically dominant" — 
- p15 FreeText (margin note): "No highlighted text. Margin note sits beside the Sec. 4 heading and Louvain intro (Community Detection)." — Important! Coalition
- p15 Highlight (yellow): "Sec. 4.1 heading and Louvain method description" — 
- p15 Highlight (yellow): "Sec. 4.1: modularity optimisation" — 
- p15 Highlight (yellow): "Sec. 4.1: modularity above 0.3 treated as meaningful" — 
- p15 Highlight (yellow): "Sec. 4.1: communities as structural and thematic organisation" — 
- p15 Highlight (yellow): "Sec. 4.1: communities of densely interconnected Acts" — 
- p15 Highlight (yellow): "Sec. 4.1: modularity 0.393 in the two-layer network" — 
- p16 Highlight (yellow): "Sec. 4.1: targeted policy analysis from structure" — 
- p16 Highlight (yellow): "Sec. 4.2: communities cross-referenced with committee domains" — 
- p16 Highlight (yellow): "Sec. 4.2: Jaccard similarity method and best-match assignment" — 
- p17 Highlight (yellow): "Sec. 4.2: agglomerative merging into macro-communities" — 
- p17 Highlight (yellow): "Table 6: Maori Affairs label" — 
- p17 Highlight (yellow): "Table 6: Maori Affairs similarity 0.67" — 
- p17 Highlight (yellow): "Sec. 5: structured, scalable approach to the local legal environment" — 
- p17 Highlight (yellow): "Sec. 5: captures structural dependencies and thematic content" — 
- p17 Highlight (yellow): "Sec. 5: framework is computational, emphasising reproducibility" — 
- p17 Highlight (yellow): "Sec. 5: audience of policy analysts" — 
- p17 Highlight (yellow): "Sec. 5: tracing dependencies supports impact assessment" — 
- p17 Highlight (yellow): "Sec. 5: useful in mental health, criminal justice, health and welfare" — 
- p18 Highlight (yellow): "Sec. 5: RegTech tools for consistency checks and gap finding" — 
- p18 Highlight (yellow): "Sec. 5: suitability for cross-jurisdiction and temporal analysis" — 
- p18 Highlight (yellow): "Sec. 5: practical value for long-term planning" — 
- p18 Highlight (yellow): "Sec. 5: disciplines involved (graph theory, IE, semantic modelling)" — 
- p18 Highlight (yellow): "Sec. 5: jurisdiction-specific elements limited to configurable rules" — 
- p18 Highlight (yellow): "Sec. 6: generalisability claim" — 
- p18 Highlight (yellow): "Sec. 6: focused exploration of how influence propagates from a chosen Act" — 
- p19 Highlight (yellow): "Sec. 6: avoiding dominance of globally influential hubs" — 
- p19 Highlight (yellow): "Sec. 6: LLM-assisted LDA gives higher coherence than rule-based" — 
- p19 Highlight (yellow): "Sec. 6: integrated structure plus semantics as the main finding" — 
- p19 Highlight (purple): "Data availability statement: code for rule-based pipeline in a public GitHub repository" — 

## Study

**Question.** No explicit research question is stated. Stated aim (Sec. 1, p.3-4): (1) build a focal legislative citation network around a seed Act via depth-limited, layer-by-layer expansion, avoiding dominance by global hubs; (2) add LLM-assisted semantic representation of Acts; (3) detect thematic communities and check their alignment with NZ parliamentary committee domains. Scope limited to Acts (historical, repealed, active); court cases excluded (Sec. 1, p.4).

**Theory.** Only implicit. Influence is treated as accumulating through layered statutory dependence and procedural cascade risk (a change to one Act may force reconsideration of dependent Acts), operationalised with Katz prestige centrality (Sec. 3, p.12-13). Argues against PageRank (static formulation sensitive to network growth, citing [35]) and harmonic centrality (shortest-path proximity assumed to mean legal relatedness) on p.13. Also argues that focal, seed-centred subgraphs reveal local influence that whole-corpus rankings hide (Sec. 2, p.4-5). No formal model of legal influence is estimated.

**Data.** Unit: Act (node); edges are directed legal references from citing Act to cited Act. Corpus: 17,709 NZ Acts in XML (active, repealed, historical) from the NZ Parliamentary Counsel Office / legislation.govt.nz (Sec. 2.1, p.6). Seed: Mental Health (Compulsory Assessment and Treatment) Act 1992. Subgraph sizes (Table 2, p.9): layer 1 = 27 Acts (0.15%); layer 2 = 2,159 (12.2%); layer 3 = 6,021 (34.0%). Six relation types: AMD, AMD_S, PR, PR_S, R_S, CIT. Period: no date range stated for the corpus; repealed and historical Acts are included. Validation samples: 50 relations per type, 300 total (Sec. 2.3, p.7); full-document validation on 10 Acts (p.7). LLM method run 10 times and averaged. Code is said to be at github.com/maryamild/Thesis (accessed 14 Jan 2026); the NZLII reference is dated "31 June 2025", which is not a valid date.

**Method.** 1) Rule-based NER and relation extraction on XML Acts using regular expressions triggered by phrases such as "inserted by", "repealed by", "within the meaning of" (Sec. 2.2, p.6-7). An LLM prompt-based extractor was tried and dropped for run-to-run variability and cost. 2) Breadth-first, layer-by-layer expansion from the seed, merging duplicate nodes by title and legislative ID, expanding only newly found nodes, stopping at three layers (Sec. 2.4, p.8-9). 3) Katz prestige centrality by power iteration, x_i = alpha * sum_j A_ji x_j + beta, beta typically 1, alpha < 1/lambda_max; directed graph with all six relation types as edges; rankings reported, not raw scores (Sec. 3, p.12-13). 4) Louvain community detection on 2-layer and 3-layer graphs, modularity reported (Sec. 4.1, p.15). 5) Two LDA topic models per Act: rule-based LDA on full legislative text (TF-IDF, lemmatisation, stopword removal) and LLM-assisted LDA on an LLM summary (max 150 words) plus 10-15 keywords, temperature 0, one pass per Act (Sec. 2.5, p.9-12). Number of topics and priors chosen by coherence score. 6) Jaccard similarity between community LDA keywords and predefined committee keyword sets; each community assigned to its best-matching committee (Sec. 4.2, p.16). 7) Agglomerative clustering merges similar Louvain communities into 6 macro-communities (Table 6, p.17).

**Identification.** None stated. Descriptive, computational network analysis on a single seed Act. No causal identification, no inferential statistics and no significance tests are reported; "influence" is a centrality score, not an estimated effect.

**Scope.** Population: all New Zealand Acts in the official XML corpus (17,709, including historical and repealed Acts), with analysis restricted to a seeded subgraph around one Act. Institution: NZ legislation and its parliamentary committee structure. Era: not defined; corpus is cumulative and undated in the text. Excluded: court cases and judicial interpretation (p.4). Authors state the method is jurisdiction-agnostic, but only NZ data is analysed.

## Key findings

- Layered subgraph growth: the seed's 1-, 2- and 3-layer neighbourhoods reach 27, 2,159 and 6,021 Acts; 3 layers covers 34% of the corpus. 27 (0.15%), 2,159 (12.2%), 6,021 (34.0%) of 17,709 (Sec. 2.4, Table 2, p.8-9)
- Authors stop expansion at three layers as a deliberate trade-off between thematic relevance and processability (Sec. 2.4, p.8); the focal Act also drops out of the top-50 Katz ranking beyond layer 3. Mental Health Act not in top 50 by Katz at layer 3 (Sec. 2.4, p.8)
- Rule-based relation extraction outperforms the LLM-based method on both precision and recall. Rule-based precision 0.96, recall 0.89. LLM average precision 0.86, recall 0.78. Text gives rule-based overall precision as 93% on 300 sampled relations (p.7), which does not match Table 1. (Table 1, p.8; text p.7)
- Mental Health Act is 2nd by Katz prestige in the 2-layer subgraph but falls out of the top 50 in the 3-layer subgraph. Authors read this as local, not systemic, centrality. Rank 2 (2-layer) to >50 (3-layer) (Sec. 3, Table 4, Figure 2, p.13-14)
- Other top-10 Katz movements: Legislation Act 2019 stays 1st; Secondary Legislation Act 2021 3rd to 2nd; Public Service Act 2020 9th to 7th; Criminal Procedure Act 2011 4th to 3rd; Corrections Act 2004 7th to 35th; Health Practitioners Competence Assurance Act 2003 8th to 37th; Policing Act 2008 10th to 14th. Rank pairs as listed (2-layer to 3-layer) (Table 4, p.13; discussion p.14)
- Authors attribute the seed's high 2-layer rank partly to construction bias: the seed is the root, so many short paths pass through it. The bias fades as the network grows. No measured magnitude given (Sec. 3, p.13)
- Louvain finds 14 communities (modularity 0.393) in the 2-layer graph and 15 communities (modularity 0.392) in the 3-layer graph. 14 communities, Q=0.393; 15 communities, Q=0.392 (Sec. 4.1, p.15; Figures 3-4, p.15-16)
- Louvain communities align only weakly with committee keyword sets (Jaccard). Several committees repeat across communities; the authors then merge communities into 6 macro-communities; only the Maori Affairs match moves (0.66 to 0.67), and the other Table 6 values repeat or are reduced copies of Table 5 values. Best community-level Jaccard scores: 0.66 (community 1, Maori Affairs), 0.50 (several), range 0.10-0.66; macro-community Maori Affairs 0.67, Foreign Affairs 0.50, Transport 0.36, Education 0.29, Justice 0.23, Finance 0.14 (Table 5 p.16-17; Table 6 p.17)
- LLM-assisted LDA gives a more concentrated, procedure-free topic set for the seed Act than rule-based LDA, which mixes in procedural words such as section and subsection. Top topic weight 0.43 (LLM) vs 0.37 (rule-based); second 0.28 vs 0.33; third 0.19 vs 0.21 (Table 3, p.11-12)
- Authors claim LLM-assisted LDA gives higher-coherence topics; no coherence values are reported anywhere in the paper. None reported (Sec. 2.5.3 p.12; Sec. 6 p.19)

## Limitations

- Construction bias: the seed Act is the root of its own subgraph, so its centrality is partly structural (authors' own admission, Sec. 3, p.13). Results at 2 layers are most affected.
- The 3-layer cap is a judgement trade-off (thematic relevance vs processability), not an optimised or tested choice (Sec. 2.4, p.8).
- Internal inconsistency: text reports 93% rule-based precision (p.7); Table 1 reports 0.96 (p.8). Sampling basis for recall (10-Act full-document validation vs 300-relation sample) is not fully specified.
- Validation is a manual check by the authors; no inter-coder reliability or independent gold standard is reported.
- LLM stochasticity is acknowledged (Sec. 2.2, 2.3) but Sec. 2.5.2 says temperature 0 and a single pass give only limited lexical variation; no run-to-run variance numbers are reported.
- Coherence claims (higher coherence, cleaner separation) rest on qualitative reading of one Act's topics; no coherence scores or hyperparameter values are given.
- Katz attenuation factor alpha and the exact beta are not reported, so the Katz results are not fully reproducible from the paper alone. Louvain resolution, random seed and agglomerative-clustering linkage and cut level are also unstated.
- Only one seed Act and one jurisdiction are analysed; generalisability claims (Sec. 5-6) are asserted, not tested. No cross-jurisdiction or temporal results.
- No time dimension: legal evolution and policy impact are named as aims but no temporal analysis is done; the paper defers this to future work.
- Centrality choice is argued against PageRank and harmonic centrality, but no numerical comparison with those measures is shown.
- Table 6 caption reads 'Columns 4-6' and its rows are a reduced set of the earlier table, so the mapping from communities to macro-communities is hard to follow.
- Citation error: NZLII reference is dated 31 June 2025 (invalid date). Author acknowledgment discloses AI-assisted language editing.
- The rule-based pipeline depends on hand-written regular expressions tuned to NZ drafting conventions; the authors say this is adaptable but do not show it on another corpus.

## Short quotes

- "the Act is locally central but not systemically dominant" (Sec. 3, p.14)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-sna#Katz-Bonacich benchmark (diagnostic)\|Katz-Bonacich benchmark (diagnostic)]] | qualifies | Katz prestige counts incoming directed paths. On one seed the Mental Health Act ranks 2nd at 2 layers (2,159 Acts) and outside the top 50 at 3 layers (6,021 Acts). Corrections Act moves 7th to 35th; Health Practitioners Competence Assurance Act moves 8th to 37th. The attenuation factor alpha is not reported. (Sec. 3 p.12-14; Table 4 p.13; Table 2 p.9) | kb_benchmark.py builds the kernel P with build_P on load_network(), which defaults to NET_CONFIG cu. It sums weighted OUT-walks, b = sum over k of (delta A)^k 1, with delta = 0.85 divided by the spectral radius, and writes kb_benchmark.json. The file calls itself a diagnostic not consumed by the chain, but make_member_desk.py reads it and shows a kb_pct percentile on the member desk. `fec/engine/kb_benchmark.py:24,27-40,48-58; fec/moneyball_core.py:461-465 (load_network, cu default); fec/surfaces/make_member_desk.py:825,843,1152` | Only the radius-sensitivity lesson transfers: ranks computed on a bounded subgraph are local. The paper's Katz counts in-paths (prestige) on an Act-citation graph, while the system's KB counts out-walks on a legislator kernel, so the paper does not validate the benchmark's direction. The transferable check is KB rank stability across cu and holistic. KB values are heuristic model outputs, never influence probabilities (D-025). | moderate | VERIFIED-SOFTENED |
| [[systems/melange-sna#Press HCoI communities\|Press HCoI communities]] | suggests-extension | Louvain on the LDA-based Act graph gives 14 communities at modularity 0.393 (two-layer) and 15 at 0.392 (three-layer). Modularity above 0.3 is treated as meaningful. Community-level Jaccard alignment with committee keyword sets runs 0.10 to 0.66. Agglomerative merging into 6 macro-communities moves only the Maori Affairs match (0.66 to 0.67). (Sec. 4.1 p.15; Sec. 4.2 p.16-17; Tables 5-6) | press_hcoi.py builds member profiles over 19 curated press categories, links pairs at Pearson 0.6, runs Louvain with seed 7 and keeps communities of size 3 or more. It writes communities, profiles, edges and genealogy. It computes no modularity and no committee alignment score. `fec/network/press_hcoi.py:4-5 (docstring pipeline), 64 (Louvain seed 7 and size filter); no modularity or Jaccard term anywhere in the file` | Adding a modularity Q and an external alignment check, such as Jaccard against committee rosters or cosponsor communities, is cheap and transferable. The paper's Q and Jaccard values come from Act citation graphs and are not benchmarks for member press graphs. The weak committee alignment is a caution that topical communities need not match institutions. The homophily null result is cited from project memory and was not checked here. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-sna#Sector network configurations\|Sector network configurations]] | qualifies | Where the network boundary is drawn changes who is central: the top-10 Katz ranks reshuffle between the 2-layer and 3-layer subgraphs. The authors attribute part of the seed's 2-layer prominence to construction bias, since the seed is the root of the subgraph. (Sec. 2.4 p.8; Sec. 3 p.13-14; Table 4) | The NETWORKS registry defines separately bounded universes: cu (published), holistic, holistic_norm (directed), bank_reg (CRS subject terms) and crypto (title screen). build_sector_diagnostic.py reports pct_novel against published CU and flags a hub as dominated when the top degree is at least 3 times the second. `fec/moneyball_core.py:391-410 (NETWORKS registry); fec/network/build_sector_diagnostic.py:351-352 (pct_novel), 390-411 (_concentration, dominated = top degree at least 3x second)` | The abstract point transfers: cross-universe centralization comparisons depend on how each bill universe was screened. The paper bounds by citation hops from one seed, not by topic screen, so its effect size does not transfer. Any cross-config comparison must name its window and denominator. The bank_reg 76.1 percent novelty figure is a run output not checked here. | weak | VERIFIED-SOFTENED |
| [[systems/melange-staffer-network#Layer-balanced PageRank, eigenvector and bridging diversity\|Layer-balanced PageRank, eigenvector and bridging diversity]] | context | The authors argue against PageRank for legislative networks because its static formulation is sensitive to network growth, citing Mariani, Medo and Zhang 2015. No numerical comparison with PageRank is shown. (Sec. 3 p.12-13; reference [35]) | Staffer PageRank uses damping 0.85 as primary, with sensitivity runs at 0.70 and 0.95, on a layer-balanced transition operator with a group-balanced teleport vector. `fec/staffer_network/graph_build.py:258-268 (damping loop, primary 0.85); fec/staffer_network/network_math.py:253-294 (group_balanced_teleport, connectedness_pagerank)` | Weak. The growth critique applies to citation networks, where older nodes accumulate in-links. A co-tenure graph takes the argument only if snapshots across vintages are compared. The paper offers an argument, not evidence. The co-tenure graph description comes from project context and was not checked here. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Overall: not refuted. Most paper figures check out in the text: Table 1 gives 0.96 and 0.89 against LLM 0.86 and 0.78; layer sizes are 27, 2159 and 6021 with coverage 0.15, 12.2 and 34.0 percent of 17,709; Louvain gives 14 and 15 communities at Q 0.393 and 0.392; Table 4 rank moves are as stated.
> - Summary issue: rel.summary_md gives Jaccard 0.10-0.67 for the communities. At community level Table 5 runs 0.10 to 0.66; 0.67 is the macro-community Maori Affairs score in Table 6.
> - Summary issue: rel.summary_md and ext.key_findings say the merge into macro-communities raises the matches. Only the Maori Affairs match moves (0.66 to 0.67). The other Table 6 values repeat or are reduced copies of Table 5 values.
> - Summary issue: rel.bearings transfer text says the chain does not consume kb_benchmark. The file's own comment says that, but make_member_desk.py reads kb_benchmark.json (lines 825, 843, 1152) and displays kb_pct on the member desk. Reword to: not consumed by the chain; shown as a member-desk percentile.
> - Summary issue: rel.paul_alignment item (2) and rel.flags say Louvain seeds the signed-coalition search (positive-Louvain seed). REFUTED: fec/network/signed_coalitions.py optimises an Aref-Neal frustration partition from a party-split seed and random restarts. Louvain appears in fec only in press_hcoi.py. Remove the claim.
> - Summary issue: ext.key_findings 2 and ext.limitations: the paper gives a deliberate trade-off between thematic relevance and processability as its first reason for the three-layer cut (Sec. 2.4 p.8). The top-50 loss is an observation alongside that. Also ext.limitations says temperature 0 gives consistent outputs; the paper says only limited lexical variation.
> - Summary issue: rel.paul_alignment item (3) says the system retired direction-blind cosponsor centrality (AB13 = 0) and that HCoI homophily came back null. Neither could be confirmed here: AB13 has no match in the fec Python tree and the homophily result was not checked. Treat both as unverified project history.
> - Summary issue: Confirmed internal inconsistency: rule-based precision is 93 percent in Sec. 2.3 p.7 but 0.96 in Table 1 p.8. Alpha, the Louvain resolution and seed, and the linkage are unreported, so Katz and Louvain results cannot be reproduced from the paper.
> - Summary issue: Confirmed: the NZLII reference is dated 31 June 2025 (text line 1170), which is not a valid date. Paul's margin note Important! Coalition on p.15 is present in the annotations. No separate ext or rel override files exist, so ext and rel were taken from the item JSON.

## Gaps and flags

- Press HCoI Louvain output reports no modularity Q and no external alignment check (committee or cosponsor-community overlap). The paper reports both.
- No radius or boundary sensitivity test for centrality: the KB benchmark and the cosponsor InfluenceScore are computed on the CU-bounded graph only, and nobody has measured how ranks move on the holistic universe.
- Regex topic tagging (CATEGORIES in cu_press_release_scraper.R:1739) has no per-category precision/recall sample. The paper's 50-items-per-type validation gives a template.
- The underdog lens still uses Centrality% from a topic-bounded cosponsor graph. The paper's local-versus-systemic finding suggests labelling that input as local to the CU universe.
- Paul's 'Important! Coalition' margin note on Louvain: the paper's communities are clusters of STATUTES in a citation graph, not legislator coalitions, and they align only weakly with committee domains (community-level Jaccard 0.10-0.66). Any carryover to member coalitions is analogical. Melange already uses Louvain in press HCoI; the signed-coalition search does not use Louvain (it optimises a frustration partition from a party-split seed). The reported HCoI homophily null is unverified project history.
- Wording: the paper calls Katz rank 'influence' that 'propagates'. Under D-025, graph ranks are heuristic model outputs, never influence, closeness or access probabilities, so do not import that language.
- Acronym collision: 'LDA' in this paper means Latent Dirichlet Allocation, not the Lobbying Disclosure Act. This is unrelated to D-021 (LDA filings as an identity source only).
- Direction mismatch: Paul highlighted the paper's Katz PRESTIGE, which counts in-paths, while the system's Katz-Bonacich benchmark counts out-walks (kb_benchmark.py:33-40). The highlight does not validate the benchmark.
- The paper is internally inconsistent: rule-based precision is 93% in the text (p.7) and 0.96 in Table 1 (p.8). Alpha, the Louvain resolution and seed, and the clustering linkage are unreported, so the results cannot be reproduced from the paper.
- The paper does not argue for either rejected SNA proposal (dropping regular cosponsorship edges; per-edge volume normalization). No conflict there.

Paul's takeaways are mostly accurate. Two parts are stale relative to the system as built.

(1) His note that NER/IE could feed data extraction is right, but the paper actually rejected LLM extraction in favour of deterministic regex on XML (precision 0.96 vs 0.86). That matches how Melange's collectors already work: regex category tagging and BILLSTATUS XML. His margin question 'Same for the US?' has a yes answer the system already uses: BILLSTATUS XML and the uscode.house.gov sentinel.

(2) 'Louvain can find communities and coalitions' predates the system's own use. Louvain already runs in press HCoI (seed 7); the signed-coalition search uses a different method. The paper also shows weak alignment between Louvain communities and committee domains, which undercuts a direct coalition reading.

(3) His 'more research on centrality is needed' is consistent with the system. It keeps Katz-Bonacich as a diagnostic only, and runs damping sensitivity on staffer PageRank. The paper's sharpest contribution is that centrality on a bounded subgraph is local, and his notes do not record it.

His stated drawback (citations only; no lobbying, co-sponsorship or money) is correct.

## Related papers

- [[malaterre-2024-inferring-social-networks]] — same-method — Item exists. Malaterre and Lareau's press HCoI pipeline (topic profiles, Pearson 0.6, Louvain) is the template for fec/network/press_hcoi.py, confirmed in the script docstring.
- [[battaglini-2016-influencing-connected-legislators]] — complements — Item exists. Battaglini and Patacchini's out-direction Katz-Bonacich is what kb_benchmark.py implements; the paper's in-direction prestige is a different quantity, so the two should not be conflated.
- [[porter-2005-network-analysis-committees]] — complements — Item exists. Porter et al. provide committee-network structure for the US House, the natural external reference for an equivalent committee-alignment check.
- [[sotoudeh-2024-network-based-measure]] — complements — Item exists. Both are network-centrality studies of legislation; together they show that centrality conclusions depend on graph construction.
- [[kim-2020-mapping-political-communities]] — complements — Item exists. Kim and Kunisky detect communities in lobbying networks, the actor-level setting that the citation-only Act graph does not cover.
- [[aref-2021-identifying-hidden-coalitions]] — complements — Item exists. Aref and Neal partition signed legislator networks into coalitions, which is the coalition reading Paul wants. Note that signed_coalitions.py uses this method, not Louvain.

### Linked from
- [[baptista-2022-universal-multilayer-network]] — complements — Item exists. Baptista defines RWR as personalized PageRank (p.1), so the PageRank versus Katz comparison applies. Baptista does not discuss Katz.
- [[haythornthwaite-1996-social-network-analysis]] — complements — The article surveys degree, closeness and betweenness. The Katz and PageRank choices attributed to Ardekani are not in this article, so the link is only the shared centrality family.
- [[christopher-2016-mathematical-modeling-analysis]] — complements — Item file exists. Both papers face the choice of which centrality to use; Ardekani chooses Katz on substantive grounds, while this paper weights eight centralities by expert judgment.