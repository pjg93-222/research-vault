---
type: paper
title: "Universal multilayer network exploration by random walk with restart"
authors: ["Anthony Baptista", "Aitor Gonzalez", "Anaïs Baudot"]
year: "2022"
venue: "Communications Physics 5:170 (received 13 Sep 2021; accepted 8 Jun 2022)"
doi: "10.1038/s42005-022-00937-9"
category: "Social Network Analysis"
systems: ["melange-pathfinding", "melange-sna", "melange-staffer-network", "melange-stance"]
components: ["melange-staffer-network.layer-balanced-pagerank", "melange-sna.multilayer-social-press-edges", "melange-stance.stance-dynamics", "melange-pathfinding.member-recruitment-path"]
paul_annotated: true
paul_markup_count: 34
evidence_status: "verified"
source: "[[_sources/baptista-2022-universal-multilayer-network.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Path-Finding Algorithums\\s42005-022-00937-9.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Path-Finding Algorithums\\s42005-022-00937-9.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Path-Finding Algorithums\\s42005-022-00937-9.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Path-Finding Algorithums\\s42005-022-00937-9.pdf"]
sha12: ["9605470975c0"]
tags: [paper, social-network-analysis]
---

# Universal multilayer network exploration by random walk with restart

**Anthony Baptista, Aitor Gonzalez, Anaïs Baudot (2022).** *Communications Physics 5:170 (received 13 Sep 2021; accepted 8 Jun 2022).* [10.1038/s42005-022-00937-9](https://doi.org/10.1038/s42005-022-00937-9). Theme: [[themes/social-network-analysis|Social Network Analysis]].

> [!abstract] Summary
> Baptista, Gonzalez and Baudot (2022) extend random walk with restart (RWR, a seed-personalized PageRank) to "universal" multilayer networks. These combine any number of multiplex networks, where the same nodes appear on several layers, linked by bipartite networks. They build one column-normalized supra-heterogeneous transition matrix with separate dials for layer jumps (delta), jumps between networks (lambda) and restart allocation (eta, tau). The method ships as the MultiXrank Python package.
> 
> They test it with leave-one-out ranking (LOOCV) and link prediction on two examples: a gene/drug/disease network and a three-country airport network. In the biological network, adding the drug multiplex did not help. The paper says low bipartite overlap might explain this: only 23% of genes and 5% of diseases are shared across the bipartite links. In the airport network, adding the German layer did help, because overlap there is high (the paper says overlap might explain this). Adding artificial transit nodes raised performance, and gradually randomizing those links lowered it again. Across 125 parameter sets on the biological network, top-ranked outputs were stable in parts of it; the airport network's top rankings were highly variable in a supplementary figure (Fig S15), with no parameter-set count stated.
> 
> Three parts of Paul's system are close to this. The staffer network's layer-balanced PageRank already runs a random walk over several layers: each node splits its transition mass equally across its active layers, and teleport is balanced across entity kinds. However, it checks sensitivity only on damping (0.70/0.85/0.95), never on how mass is split across layers. The Congress multilayer builder merges the cosponsor, engagement, HCoI, social and co-appearance layers into a single edge weight, so it has no layer-jump parameter. No held-out test checks whether each added layer improves anything; the only per-layer evidence is null neighbour co-movement on the cosponsor and HCoI layers. The paper's main lesson for this system is to test whether each layer earns its place, and how much its links overlap with the base graph, before treating more layers as more signal. It gives no evidence about legislatures, lobbying or social ties.

## Paul's annotation

From [[annotations/Path-Finding Algorithums - Path-Finding Algorithums - Annotated Bibliography|Path-Finding Algorithums - Path-Finding Algorithums - Annotated Bibliography]]:

> Baptista, Anthony, et al. “Universal Multilayer Network exploration by random walk with restart.” *Communications Physics*, vol. 5, no. 1, 1 July 2022, [https://doi.org/10.1038/s42005-022-00937-9](https://doi.org/10.1038/s42005-022-00937-9).

## Paul's markup in the PDF

- p1 Highlight: "However, current random walk approaches are limited in the combination and heterogeneity of networks they can handle." — 
- p1 Highlight: "We propose here MultiXrank, a method and associated Python package that enables Random Walk with Restart on any kind of multilayer network." — 
- p2 Highlight: "Among the different modes of representation of data, networks appear as particularly successful." — 
- p2 Highlight: "a comprehensive understanding of biological systems would require the integrated analysis of dozens of different datasets produced at different molecular, cellular or tissular scales." — 
- p2 Highlight: "The authors collected dozen of heterogeneous networks, i.e networks with various types of nodes such as genes, drugs or diseases, to prioritize drugs for repurposing." — 
- p2 Highlight: "Heterogeneous networks link networks composed of different types of nodes thanks to bipartite interactions." — 
- p2 Highlight: "A wide range of methods have been developed in the recent years to analyze multilayer networks." — 
- p2 Highlight: "They can be described as simulated particles that walk iteratively from one node to one of its neighbors with some probability." — 
- p2 Highlight: "RWR strategies have been shown to significantly outperform methods based on local distance measures for the prioritization of gene-disease associations." — 
- p2 Highlight: "associations" — 
- p2 Highlight: "Each multiplex network contains different types of nodes" — 
- p2 Highlight: "linked by six bipartite networks" — 
- p2 Highlight: "All the edges of the universal multilayer networks can be weighted and/or directed." — 
- p2 Highlight: "networks and (iii) multiplex-heterogeneous In RWR, the degrees of freedom are summarized in the Transition rate matrix, and correspond to the available transitions between the different nodes of the graph." — 
- p2 Highlight: "These evaluations reveal that more network data is not always better and highlight the critical influence of the bipartite networks." — 
- p3 Highlight: "The parameters delta are associated with the probability to jump from one layer to another in a given multiplex network" — 
- p3 Highlight: "d" — 
- p3 Highlight: "All the normalized possible" — 
- p3 Highlight: "transitions can be included in the Transition rate matrix." — 
- p3 Highlight: "between the (replica) nodes of the different Multiplex networks can be represented by Supra-adjacency matrices" — 
- p3 Highlight: "and l and m represent the layers of the multiplex network k." — 
- p3 Highlight: "we need to column-normalize the Supra-adjacency matrix defined in the equation 15" — 
- p3 Highlight: "The Supra-adjacency matrix representing the multiplex network k can be written as described in equation" — 
- p4 Highlight: "Universal multilayer networks are composed of any combination of multiplex networks, linked by any combination of bipartite networks" — 
- p4 Highlight: "We will now detail the Bipartite network matrices, and how to combine intra- and inter- multiplex networks information to obtain the Supra-heterogeneous adjacency matrix." — 
- p4 Highlight: "Universal multilayer networks unify the representation of heterogeneous multiplex networks" — 
- p4 Highlight: "the walker can stay in the multiplex ... defines the transition probability between two different multiplex networks." — 
- p4 Highlight: "Finally, if the node exists only in the bipartite network, the normalization corresponds to the standard normalization weighted by a modified jump probability." — 
- p6 Highlight: "Given these low overlaps, the drug multiplex network might not contribute significantly to connecting gene and disease nodes during the random walks." — 
- p6 Highlight: "These high overlaps might explain why the addition of the third multiplex network in this case increases the predictive power" — 
- p6 Highlight: "To do so, we progressively randomized the edges in the bipartite networks with artificially increased connectivity, until obtaining completely random bipartite networks." — 
- p7 Highlight: "We next computed a consensus Similarity matrix with a normalized euclidean norm of each individual Similarity matrix" — 
- p8 Highlight: "ity measures for the optimization of the loss MultiXrank can produce such a similarity measure respecting the global topology of the multilayer network." — 
- p8 Highlight: "The MultiXrank package can be applied to any kind of multilayer network such as social, economic, or ecological multilayer networks." — 

## Study

**Question:** Can random walk with restart (RWR) be generalized to explore any combination of multiplex, monoplex and bipartite networks (a "universal multilayer network"), and how do added network layers and bipartite links change prediction performance and the stability of outputs to input parameters?

**Theory:** Random walks as iterative stochastic processes on graphs; RWR as a steady-state diffusion / personalized PageRank measuring proximity of all nodes to seed(s) (p.1-2, citing Lovasz 1993, Brin and Page 1998, Gomez et al. 2013). Multilayer formalism following De Domenico et al. 2013 and Kivela et al. 2014: supra-adjacency matrices for multiplex layers (with inter-layer jump probability delta), bipartite matrices for heterogeneous links, and a column-normalized supra-heterogeneous transition matrix with inter-network jump probabilities lambda and restart parameters eta (network) and tau (layer) (Eqs 5-16, Fig. 2, p.2-4). No behavioural or social theory is used.

**Data:** Two empirical multilayer networks, no sample sizes given in the main text. (1) Biological network: a gene multiplex (gene physical and functional relationships), a drug multiplex (drug clinical and chemical relationships), and a disease monoplex (phenotypic similarity), linked by gene-disease, drug-gene and drug-disease bipartite networks (p.4, detailed in Supplementary Note 3.B). Unit: genes, drugs, diseases as nodes. (2) Airports network: French, British and German airport multiplex networks (nodes = airports; edges = national flight connections for three airline companies), linked by French-German, British-German and French-British transnational bipartite networks (p.4, Supplementary Note 3.A). Node and edge counts, time period and original sources are not stated in the main text. Data and code are on OSF (doi 10.17605/OSF.IO/ZSMUA) and GitHub (anthbapt/multixrank). Comparison dataset in Methods: the PPI network (p.4-5).

**Method:** Formal: derivation of RWR on universal multilayer networks (Eqs 1-16), with the normalization generalized from Li and Patra 2010 and Valdeolivas et al. 2018 to N multiplex networks. Implemented as the Python package MultiXrank (optimized, pip installable; complexity analysis in Supplementary Note 2). Evaluation: (a) Leave-One-Out Cross-Validation (LOOCV): each known gene-disease association left out in turn, remaining associations used as seeds, rank of left-out gene recorded and CDF of ranks plotted (p.4-5, Fig. 3a-b). (b) Link Prediction (LP): a gene-disease edge removed and its gene ranked with the disease as seed (p.6, Figs S11, S14). (c) Controls: artificially added transit drug nodes linking gene-disease pairs (1, 2 or 5 per association; Fig 3c-d) and progressive random rewiring of the enhanced bipartite networks (p.5-6). (d) Parameter-space exploration: 125 parameter sets; pairwise similarity of rank output between parameter sets (Eqs 17-18, rank-based distance normalized across multiplex networks), PCA projection, k-means on first two PCs, comparison of top-5 and top-100 nodes per cluster (p.6-8, Fig. 4).

**Identification:** No causal identification strategy. Predictive performance is assessed against held-out known associations (LOOCV and LP). Causal-style claims about bipartite overlap are made by artificially manipulating bipartite connectivity (transit nodes) and then randomizing edges (p.5-6). Stability of outputs is assessed by varying parameters across 125 sets on the biological network (p.7).

**Scope:** Methodological paper. Two example networks only: a biological gene/drug/disease network and a three-country airport network. No population or institution; the airport network is a current-era transport network and the biological network is drawn from unspecified databases. Claims of general applicability (social, economic, ecological networks, p.8) are not tested in the paper.

## Key findings

- RWR is generalized to universal multilayer networks by a column-normalized supra-heterogeneous transition matrix, with inter-multiplex jump parameters lambda and restart parameters eta and tau. The normalization is the step the authors describe as previously unsolved beyond two heterogeneous multiplex networks. (Results, Eqs 10-16, p.3-4; Introduction p.2 (normalization unsolved for N multiplex networks)) — Parameters delta, lambda, eta, tau; sum of lambda over alpha = 1 for each beta; sum over j of tau_kj = 1
- In the gene-disease LOOCV, combining the gene multiplex with the disease monoplex improves ranking over PPI alone and over the gene multiplex alone. Adding the drug multiplex does not further improve performance. (Evaluations, p.4-5, Fig. 3a) — No numeric values given in text; comparison is by CDF curves
- In the airports network, adding the German multiplex (with French-German and British-German bipartites) increases RWR's ability to predict French-British airport associations, unlike the drug addition in the biological network. (Evaluations, p.5, Fig. 3b) — Three networks: French, British, German airport multiplexes
- Bipartite overlap might explain the difference, as the paper suggests: only 23% of genes in the gene-disease bipartite network appear in the drug-gene bipartite, and only 5% of diseases appear in the disease-drug bipartite. The airport bipartites show high overlap. (Evaluations, p.5-6; Fig. S10 (supplementary)) — 23% genes; 5% diseases
- Artificially adding transit drug nodes that link existing gene-disease associations raises LOOCV performance drastically, and the same holds for the airport network. (Evaluations, p.6, Fig. 3c-d) — 1, 2 or 5 transit nodes per gene-disease association (biological); 1, 2 or 5 transit German nodes per French-British association (airports); no performance values in text
- Progressively randomizing the enhanced bipartite networks continuously lowers LOOCV performance, down to the level of using only two multiplex networks. (Evaluations, p.6-7, Fig. S13 (supplementary)) — Randomization runs to completely random bipartite networks; no values in text
- Link prediction gives behaviour similar to LOOCV. (Evaluations, p.6, Figs S11 and S14 (supplementary)) — None in text
- Output sensitivity to input parameters differs by network. Top-ranked gene nodes vary more than top-ranked disease nodes across 125 parameter sets. The biological gene-disease network has stable regions, while the airport network has highly variable top-rankings (Supplementary Fig S15, no set count stated). (Parameter space exploration, p.6-8, Fig. 4a-c; Supplementary Figs S15-S16) — 125 parameter sets; PCA space; 8 clusters; clusters 4 and 6 give similar top-100 nodes
- Authors conclude that more network data does not always increase predictive power, and that bipartite network connectivity is critical. (Abstract p.1; Results p.5-6; Discussion p.7)

## Limitations

- Paper has no dedicated limitations section; limitations stated in text: the formalism for some universal networks 'might sometimes require some adaptations' (Discussion, p.7).
- Billion-scale networks are not handled; the authors name BEAR (exact or approximate) and BEPI (approximate) as strategies to consider (Discussion, p.8).
- Performance on the biological network might depend on bipartite overlap, which is low for drug-gene and disease-drug links (23% and 5%) (p.5-6).
- Parameter-space stability differs by network (airport network variable; biological network stable in some regions) (p.8).
- Only two example networks are evaluated (p.4-8).

## Short quotes

- "These evaluations reveal that more network data is not always better" (Abstract/Introduction, p.2 (also restated Discussion p.7))
- "the addition of interaction network layers also brings additional degrees of freedom" (Discussion, p.7)
- "the progressive randomization of the bipartite networks continuously decreases the predictive power" (Evaluations, p.6-7)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-staffer-network#Layer-balanced PageRank, eigenvector and bridging diversity\|Layer-balanced PageRank, eigenvector and bridging diversity]] | qualifies | Top-ranked nodes can shift as walk parameters change, so stability must be measured for each network rather than assumed. The 125-parameter-set exploration was run on the biological gene and disease network only. Airport top-rankings were highly variable in a supplementary figure (Fig S15) with no parameter-set count stated. (Parameter space exploration p.6-8, Eqs 17-18, Fig. 4a-c; airport result stated p.8 citing Supplementary Fig S15) | Confirmed: a bidirectional row-stochastic walk where each node gives 1/active_layers of its mass to each active layer, split by weight within the layer. Teleport is equal across node kinds. PageRank runs at damping 0.70, 0.85 and 0.95 with L1 distance and top-1000 overlap against 0.85. The equal-layer split is labelled an explicit modeling prior in graph_build.py, not in network_math.py. `fec/staffer_network/network_math.py:91-167 (layer_balanced_transition, confirmed); network_math.py:253-262 (group_balanced_teleport, confirmed); network_math.py:264 onward (connectedness_pagerank); fec/staffer_network/graph_build.py:258-275 (damping sweep, confirmed); graph_build.py:405 (modeling prior label)` | The operator transfers because both are stochastic walks over typed layers and node kinds. The stability result does not transfer by itself: the paper says stability depends on the network, so the same allocation and teleport sweep must be run on the staffer graph. The paper gives no guidance on which allocation is correct. Outputs remain descriptive connectedness, never access or introduction probabilities (D-025). | moderate | VERIFIED-SOFTENED |
| [[systems/melange-sna#Social and press-engagement layers\|Social and press-engagement layers]] | qualifies | Adding a layer helped in the airport case where bipartite overlap was high (German layer) and did not help in the biological case where the drug layer had low overlap (23% of genes and 5% of diseases shared). The paper says overlap might explain this. Randomizing the boosted bipartite links pushed LOOCV back to the two-network baseline. The authors conclude that more network data is not always better. (Evaluations p.4-7, Fig. 3a-d, Fig S13; Discussion p.7) | Confirmed: engagement, HCoI, social and co-appearance (holistic only) layers are merged into one edge weight in fec/network/build_multilayer_network.py. Under the CU config (MEDIA_ADMIT untied) media edges are admitted only for untied pairs and HCoI only for cosponsor-isolated members. Under the holistic config (dominance) a tied pair takes the max weight and its layer tag gains the channel. Social reinforcement of existing ties (factor 1 + 0.3 times min(1, p/0.10)) applies under both configs, not only CU. Layer tags are written to a Provenance sheet (build_multilayer_network.py:200). That sheet is read by fec/legislation/leg_evidence_recompute.py and fec/surfaces/build_dashboard_bundle.py, so the claim that the engine never reads it is wrong. A repo search of fec/network, fec/stance and fec/staffer_network found no LOOCV, link-prediction or held-out test of layer gain. `fec/network/build_multilayer_network.py:73-81 (admit, confirmed); :111-170 (layer branches, confirmed); :200 (Provenance sheet); fec/stance/stance_dynamics.py:94-121 (co-movement test); fec/legislation/leg_evidence_recompute.py:2415 (Provenance reader); fec/surfaces/build_dashboard_bundle.py:226 and :299 (Provenance reader)` | The design principle transfers: test each layer's held-out gain and report its overlap with the base graph. The 23% and 5% figures are biology-specific. Melange's builder merges layers into one weight, so the paper's per-layer jump parameters have no counterpart in the code. Gains and losses remain UNVERIFIED until measured on congressional data (DE-062). | moderate | VERIFIED-SOFTENED |
| [[systems/melange-stance#Stance dynamics and neighbour imputation\|Stance dynamics and neighbour imputation]] | suggests-extension | RWR propagates proximity from seeds over several hops and across layers and is validated by LOOCV. Combining layers helped in the gene-disease task: gene multiplex plus disease monoplex beat PPI alone and the gene multiplex alone. The drug layer with low bipartite overlap added nothing, while the airport layer with high overlap helped. The paper does not state that layers help only when overlap is high in the gene-disease case. (Results Eqs 1-16 p.2-4; LOOCV p.4-5, Fig. 3a-b) | Confirmed: imputation is the mean tier score of a member's direct neighbours on the cosponsor graph only (adjacency built at fec/stance/stance_dynamics.py:90), with at least 5 neighbours (MINNB) and absolute estimate at least 0.75 (CONF). HCoI is used only for the co-movement test. The proposal file data/stance/stance_imputation_proposals.csv has 308 lines including the header, so 307 proposals is reproduced. The 77% masking sign agreement was not reproduced. `fec/stance/stance_dynamics.py:90 (single-layer adjacency, confirmed); :131-148 (imputation and masking check, confirmed); :94-121 (co-movement test)` | A per-member RWR seeded on known stances could replace the one-hop mean, and the existing masking protocol already has the form of LOOCV. Neighbour co-movement was null, so a multi-hop walk may not help either. This is a candidate needing a prereg and a single look, not evidence. Stance labels are Paul's domain: proposals only, and the matrix is never edited. | weak | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Sponsor-to-target max-product path and chain hold\|Sponsor-to-target max-product path and chain hold]] | suggests-extension | RWR scores proximity by summing over walks rather than taking a single path, and RWR has been shown to outperform local distance measures for gene-disease prioritization (Kohler et al. 2008, ref 21). The paper defines RWR as steady-state proximity from all nodes to the seed but does not use the phrase summing over all walks. (Introduction p.2 (ref 21); Results p.2-4 for the RWR definition) | Confirmed: the template uses a max-product Dijkstra (distToTarget multiplies passive probabilities and keeps the maximum), which orders paths the same way as a shortest path on -log p, though not literally on -log p. ASKM=0.07 and chainHold as the product of activeP along the chain are confirmed. bestSponsors ranks by chainHold, then cost, as stated. `fec/surfaces/templates/make_dashboard_html_00.tpl:734 (distToTarget); :740 (tracePath); :748-751 (ASKM, activeP, chainHold); :760-766 (bestSponsors)` | An RWR score seeded at the target, or at the pro-sponsor set, would add a multi-route robustness measure beside the single best chain. The paper's support comes from biological prioritization, not recruitment, and gives no congressional estimate. Any such score is a heuristic proximity, never an ask-success or access probability (D-025). | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary says the 125 parameter sets showed airport instability. The 125-set exploration was run on the biological network only; airport variability appears in Supplementary Fig S15 with no set count stated.
> - Summary issue: Summary states bipartite overlap as the cause of both the drug-layer failure and the German-layer gain. The paper says overlap might explain these.
> - Summary issue: Claim that layer identity sits on a Provenance sheet the engine never reads is wrong. fec/legislation/leg_evidence_recompute.py (about line 2415) and fec/surfaces/build_dashboard_bundle.py (lines 226 and 299) read that sheet.
> - Summary issue: Social-edge reinforcement by multiplication applies under both CU and holistic configs, not only under CU.
> - Summary issue: paul_alignment says Paul's one substantive position is a highlight attached to melange-sna.multilayer-social-press-edges. The item file shows that highlight has an empty note and no component link, so the attachment is unsupported. The 'since he read it' timeline is not in the record.
> - Summary issue: Co-movement figures of 52% and 49%, masking agreement of 77%, admission shares of 1.5% and 28.8%, and the zero-HCoI-edges statement are not reproduced. They come from run outputs or code comments.
> - Summary issue: Reproduced: 307 imputation proposals (CSV has 308 lines with header); the layer-balanced operator, teleport split, and damping sweep; the ASKM and chainHold code; and the bestSponsors ordering.
> - Summary issue: Pathfinding bearing: the template uses max-product Dijkstra, not literally -log p. Ordering is equivalent.
> - Summary issue: Dropped related link chen-2016-most-reliable-path (contradicts): Baptista does not discuss Chen or most-reliable-path, so the contradiction relation is unsupported.
> - Summary issue: Dropped related link azaiez-2025-hypergraph-analysis-european: Baptista does not discuss lobbying or hypergraphs, so no grounded link remains.
> - Summary issue: ext.references_of_note cites Choobdar et al. 2019 as the source of not-always-better. In Baptista, Choobdar appears only as ref 11 for community detection.
> - Summary issue: Coverage: the extracted text has 1277 lines, matching ext.coverage. Paul markup has 34 highlights, none with a note; not separately verified beyond that count.

## Gaps and flags

- No per-layer held-out evaluation, such as leave-one-out or link prediction, of whether the engagement, HCoI, social or co-appearance layers improve any prediction over the cosponsor-only graph. The builder logs admission counts, not predictive gain.
- No measure of overlap or connectivity between the media layers and the cosponsor base. This is the quantity the paper suggests might explain whether a layer helps. Recorded media shares (1.5% of holistic pairs vs 28.8% of CU pairs) are admission statistics from code comments (not re-derived), not an overlap test.
- The Congress multilayer graph is merged into one weight per pair, so there is no per-layer walk or jump parameter and no way to switch a layer's contribution up or down without a rebuild.
- The staffer PageRank sensitivity harness varies only damping. The equal-active-layer split and the per-kind teleport split, both called modelling priors, are never swept or checked for top-k stability.
- No randomization control: rewiring a layer's edges to check that its contribution is structural rather than an artifact of density.
- Do not read 'more network data is not always better' as support for dropping regular cosponsorship edges. That proposal was measured and REJECTED in this project, and the paper's claim concerns whole added layers with low bipartite overlap, not edge classes within the cosponsor layer.
- The paper's column normalization is the standard stochastic normalization of a walk operator, as in the staffer layer_balanced_transition. It is not per-edge volume normalization of cosponsorship weights, which was measured and REJECTED. Do not cite this paper for it.
- If RWR or PageRank outputs are adopted anywhere (staffer connectedness, recruitment proximity, stance imputation), they are heuristic model outputs and never closeness, access or introduction probabilities (D-025). Any rank figure must name its window and denominator.
- The paper's own evidence is limited to biology and airport networks.

## Related papers

- [[malaterre-2024-inferring-social-networks]] — complements — Item exists. Baptista does not cite Malaterre. Analyst link only: the HCoI layer's added value is the kind of layer Baptista's overlap and ablation logic would test.
- [[steinertthrelkeld-2018-twitter-data]] — complements — Item exists. Baptista does not cite it. Analyst link only: the social repost and mention layer is the kind of layer Baptista's per-layer test would cover.
- [[zareie-2020-similarity-based-link]] — same-method — Item exists. Baptista does use link prediction to score RWR (LP protocol, p.6), so the method overlap is real. Baptista does not cite Zareie.
- [[ardekani-2026-computational-graph-theoretic]] — complements — Item exists. Baptista defines RWR as personalized PageRank (p.1), so the PageRank versus Katz comparison applies. Baptista does not discuss Katz.
- [[hargittai-2015-bigger-always-better]] — complements — Item exists. Baptista's Discussion (p.7) makes the same more-data-is-better intuition point about network layers. The domain differs and Baptista does not cite Hargittai.
- [[kim-2020-mapping-political-communities]] — complements — Item exists. Baptista's central claim that bipartite connectivity decides whether a heterogeneous walk adds information (p.5-6) applies to a lobbying-to-legislator bipartite. Analyst link, not a Baptista citation.

### Linked from
_None._