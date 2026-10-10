---
type: paper
title: "Influence maximization in social networks using graph embedding and graph neural network"
authors:
  - "Sanjay Kumar"
  - "Abhishek Mallik"
  - "Anavi Khetarpal"
  - "B.S. Panda"
year: "2022"
venue: "Information Sciences, vol. 607, Aug. 2022, pp. 1617-1636"
doi: "10.1016/j.ins.2022.06.075"
category: "Adaptive Influence Maximization"
systems:
  - melange-allocation
  - melange-influence-engine
  - moneyball-rank-pillars
components:
  - melange-allocation.budget-frontier
  - melange-influence-engine.competitive-cascade-crn
  - moneyball-rank-pillars.network-centrality-influencescore
paul_annotated: true
paul_markup_count: 0
evidence_status: verified
source: "[[_sources/kumar-2022-influence-maximization-social.html]]"
original_paths:
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Path-Finding Algorithums\\Influence maximization in social networks using graph embedding and graph neural network - ScienceDirect.html"
sha12:
  - af01c359270d
tags: [paper, adaptive-influence-maximization, type/paper, theme/adaptive-influence-maximization, evidence/verified]
---

# Influence maximization in social networks using graph embedding and graph neural network

**Sanjay Kumar, Abhishek Mallik, Anavi Khetarpal, B.S. Panda (2022).** *Information Sciences, vol. 607, Aug. 2022, pp. 1617-1636.* [10.1016/j.ins.2022.06.075](https://doi.org/10.1016/j.ins.2022.06.075). Theme: [[themes/adaptive-influence-maximization|Adaptive Influence Maximization]].

> [!abstract] Summary
> Kumar, Mallik, Khetarpal and Panda (Information Sciences 607, 2022) propose SGNN, which treats influence maximization as a pseudo-regression task. Each node gets a struc2vec structural-identity embedding. A GNN regressor with LSTM neighbourhood aggregation is trained on labels made by simulating every node's individual spread under SIR and IC diffusion. The training network is chosen by a parametric sweep over synthetic graphs. The trained model then predicts node influence on an unseen target network, and the top-k predicted nodes become the seed set. The abstract says SGNN outperforms some of the classical and recently proposed methods on several real-life networks. The local text is only a ScienceDirect preview, so datasets, network sizes, metrics, effect sizes and limitations cannot be checked. Every performance claim here is the authors' own and is UNVERIFIED. The paper's stated motivation is cost: greedy IM is accurate and has a guarantee but is expensive, while centrality heuristics are cheap but network-specific. For Melange the cost argument mostly does not apply. The budget frontier already runs lazy (CELF-style) greedy with common random numbers on a single 533-member chamber graph and caps at 60 seeds, so the expensive side of the trade-off is affordable. Three features of SGNN also clash with choices Melange made on purpose. Seeds are ranked by each node's standalone spread, which ignores overlap between seeds. The embedding has no stance or direction, the same reason InfluenceScore's centrality weight was set to 0 in the Influence pillar (it is kept at 0.5 in the underdog lens). And the labels come from SIR and IC diffusion simulations, not the competitive two-colour race the engine models. The paper is best read as a scale-motivated alternative that Melange does not need, and as a caution against top-k standalone ranking.

## Paul's annotation

From [[annotations/Path-Finding Algorithums - Path-Finding Algorithums - Annotated Bibliography|Path-Finding Algorithums - Path-Finding Algorithums - Annotated Bibliography]]:

> Kumar, Sanjay, et al. “Influence maximization in social networks using graph embedding and graph neural network.” *Information Sciences*, vol. 607, Aug. 2022, pp. 1617–1636, [https://doi.org/10.1016/j.ins.2022.06.075](https://doi.org/10.1016/j.ins.2022.06.075).

## Study

**Question:** Can influence maximization be recast as a pseudo-regression task, in which a GNN regressor predicts each node's influence (computed under SIR and IC diffusion) from struc2vec node embeddings, so that the top-k predicted nodes form the seed set?

**Theory:** Structural identity of nodes (struc2vec embeddings) as features; message-passing graph neural networks with an LSTM neighborhood aggregator; influence maximization as seed selection under SIR and IC diffusion. The introduction contrasts centrality-based heuristics (cheap but network-specific) with greedy methods (accurate with guarantees but computationally intensive).

**Data:** Synthetic networks are used for training (a training network is selected by parametric analysis on synthetic test networks). Several real-life networks are used as target networks for evaluation. Network names, node and edge counts, sample sizes and period are not in the accessible text (dataset section body not accessible). Unit of analysis: network nodes (users or entities in an online social network).

**Method:** (1) Compute struc2vec embeddings for every node of the network. (2) Label each node with its influence under SIR and IC diffusion. (3) Train a GNN-based regressor (LSTM as neighborhood aggregator) mapping embeddings to influence, minimizing error between calculated and predicted influence. (4) Choose the training network by parametric analysis on synthetic networks. (5) Apply the trained model to predict node influence on the target network and take the top-k nodes as seeds. Comparison is against classical and recently proposed IM methods under SIR and IC.

**Identification:** No causal identification strategy. Evaluation is out-of-sample: a model trained on a chosen synthetic training network is applied to separate real-life target networks (per abstract). Specific validation split details are not in the accessible text.

**Scope:** Population: static social-network graphs (online social networks such as Twitter, Facebook and Instagram are named as motivating examples in the introduction). Application: viral marketing and information spread under SIR and IC models. Era: published 2022. Not about lobbying, legislators, elections, persuasion or text measurement.

## Key findings

- The proposed SGNN approach is reported to outperform several classical and recently proposed influence maximization methods on several real-life networks across various evaluation metrics (author claim, not verifiable from accessible text). (Abstract)
- Influence maximization is reframed as a pseudo-regression task predicting continuous node influence values, enabling deep learning and machine learning regressors to be used. (Introduction, contribution (ii); Proposed Work section opening)
- Greedy IM methods are described as generally more accurate with a theoretical performance guarantee but computationally intensive; centrality methods are cheap but network-specific. (Introduction, paragraph 2)
- Influence labels for regression are computed under the SIR and IC models; a parametric analysis on synthetic networks is used to select the training network. (Abstract; Introduction; Experimental Results and Analysis section heading)

## Short quotes

- "the results are network-specific that cannot be generalized effectively to other networks" (Introduction, paragraph 2 (on centrality-based approaches))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-allocation#Budget frontier with knee and electoral modes\|Budget frontier with knee and electoral modes]] | qualifies | Introduction says greedy IM methods are generally more accurate with a theoretical performance guarantee but computationally intensive, while centrality methods are cheap but network-specific. SGNN replaces the greedy step with a learned per-node influence regressor and top-k selection. No SGNN numbers appear in the accessible preview. (Introduction, the paragraph citing Kempe [8] and refs [20] and [21]. The flattened preview text has no reliable paragraph numbers, so the label paragraph 2 is approximate.) | budget_frontier.py runs a lazy greedy: a heap of stale marginals, where each candidate marginal is spread(S+c) minus spread(S), clipped at zero and multiplied by a per-candidate factor fac. MC defaults to 20 edge-keyed CRN cascades. It stops at MAXSEEDS (default 60) or at zero marginal and reports a knee. `fec/engine/budget_frontier.py:153-170` | The submodular guarantee is not shown for this two-colour engine with leverage factors, so the claim that the greedy carries a guarantee here should not be made. A learned surrogate would trade away the marginal accounting for speed. Speed matters only if per-candidate MC greedy is infeasible, and the 533-member, 60-seed setting is not shown to be infeasible. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Competitive independent cascade with common random numbers\|Competitive independent cascade with common random numbers]] | qualifies | Training labels are each node's individual influence computed under SIR and IC diffusion, and the seed set is the top k nodes by predicted influence. The preview does not say the simulations are single-source, and the paper does not model competition between seed sets. (Abstract; Introduction contribution (ii) and the sentence stating that the seed set is the top k nodes by predicted influence) | The cascade is a two-colour first-to-reach race: a node takes the colour of the first neighbour that reaches it, and spread counts only A-coloured activations. Pro seeds (Aseed) are A and anti seeds form the opposing set. Neutral bystanders are activated with probability min(0.9, ind/400000), which is a money-tied activation rather than a fixed-probability cascade. `fec/engine/budget_frontier.py:83-94` | Single-colour standalone-spread labels do not encode competition or seed overlap. A surrogate would need retraining on competitive-race marginal labels before standing in for this engine, and the paper never tests that case. | moderate | VERIFIED-SOFTENED |
| [[systems/moneyball-rank-pillars#Cosponsor-network InfluenceScore (computed, weight 0)\|Cosponsor-network InfluenceScore (computed, weight 0)]] | qualifies | SGNN features come from struc2vec structural identity, and labels come from SIR and IC diffusion simulations. The regressor therefore predicts diffusion-based influence on structure, not purely structural spreading power. (Abstract (struc2vec features and diffusion labels); Introduction paragraph describing structural identity of nodes) | The cosponsor InfluenceScore (0.60 weighted out-degree, 0.25 betweenness, 0.15 eigenvector) had its centrality weight set to 0 in the Influence pillar on 2026-06-25 (AB13 from 0.65 to 0.0), with the stated reason that centrality is direction-blind. It still feeds the underdog lens at 0.5 weight. `C:/Users/pgoff/Documents/brain/actors/moneyball-workbook.md:133` | Applies only if someone proposes learned structural embeddings as an influence signal. Any such signal would need stance gating and would remain a heuristic model output, never closeness or access (D-025). | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Overstated performance claim: summary says SGNN beats classical and recent IM baselines on several real networks. The abstract says it outperforms some of the classical and recently proposed methods on several real-life networks. Author claim, unverified.
> - Summary issue: Overstated InfluenceScore status: summary says InfluenceScore was retired. Its centrality weight was set to 0 in the Influence pillar (committee-only) on 2026-06-25 and it is kept at 0.5 in the underdog lens.
> - Summary issue: Wrong citation: the InfluenceScore bearing cites moneyball-workbook-lineage-and-june-audit.md line 62, which does not state the weight-0 change. Correct source is moneyball-workbook.md lines 133 and 137.
> - Summary issue: Unsupported detail: summary and bearing 2 describe the paper's labels as single-source diffusion. The preview only says influence is computed under SIR and IC; single-source is not stated.
> - Summary issue: Unsupported guarantee: summary and bearing 1 imply the greedy frontier carries a submodular guarantee for this engine. Not established for the two-colour engine with leverage factors.
> - Summary issue: Paragraph numbers in the bearings (paragraph 2, paragraph 3) cannot be confirmed from the flattened preview text; locators are approximate.
> - Summary issue: Coverage note in ext.json: says the text is 24,677 chars, while the item metadata n_chars is 24,620. The file is 24,677 bytes; the difference is encoding, not missing text. Dataset, N, metrics and limitations are absent from the preview, so all SGNN numbers remain UNVERIFIED.

## Gaps and flags

- Melange has no test of whether any structural or learned node ranking, such as KB centrality, InfluenceScore or an embedding regressor, recovers the greedy frontier's seed order. A rank comparison against the lazy-greedy path would show how much the greedy step adds over a cheap heuristic.
- The frontier's seed-set stability at MC=20 is checked by mc_stability_check, but its accuracy gap against an exhaustive or higher-MC greedy is not reported.
- Only a ScienceDirect preview is available locally. Datasets, N, metrics and effect sizes were not readable, so every SGNN performance claim is UNVERIFIED and the authors' own.
- Influence language: SGNN's predicted influence is a diffusion-model output on a structural graph. If carried into Melange it must be framed as a heuristic model output, never as closeness, access or introduction probability (D-025).
- The paper does not argue for dropping regular cosponsorship edges or for per-edge volume normalization, so neither rejected SNA proposal is triggered.

Paul's two bibliography entries list this work with no takeaways. It sits in the Path-Finding folder and is cross-referenced only by title to Zareie's link-prediction work. There is no annotation to agree or disagree with. The evidence suggests the work belongs with influence maximization rather than path-finding, since it has no routing content. It is not stale relative to the system, because no component was built on it. Its main use is as a foil: the system's lazy greedy on a 533-node graph already makes SGNN's cost motivation unnecessary.

## Related papers

- [[almazaydeh-2026-heuristic-centrality-methods]] — complements — Both papers weigh cheap node rankings against greedy optimization in IM. Kumar builds a learned ranker; Almazaydeh tests whether centrality heuristics can match greedy. Item exists and title matches.
- [[trivedi-2020-efficient-influence-maximization]] — complements — Both address efficient IM under the Independent Cascade model, which Kumar uses for labels alongside SIR. Relation changed from same-method because the methods differ (GNN regression versus a sampling or search method).
- [[ma-2022-adversarial-attack-graph]] — complements — Both join graph neural networks with influence maximization from opposite directions: Ma frames a GNN attack as IM, while Kumar uses a GNN to solve IM. Item exists and title matches.
- [[zareie-2023-influence-maximization-social]] — complements — Survey of behaviour-aware IM methods, which places a structure-only learned ranker against methods that model user behaviour. Item exists; survey content not re-read in this verification.
- [[bharathi-nd-competitive-influence-maximization]] — complements — Bharathi et al. model competitive IC, which the Melange engine follows in spirit. Kumar's single-colour labels do not model competition, so the two are not contradictory; Kumar simply does not cover the competitive case. Relation changed from contradicts.
- [[kamarthi-2020-influence-maximization-unknown]] — complements — Another learning-based IM approach. Kamarthi et al. learn graph-sampling policies where Kumar learns a node-influence regressor; both trade guarantees for generalization across networks. Title matches.
- [[zareie-2020-similarity-based-link]] — complements — Paul's annotation cross-references this social-graph learning work by title only. It shares the representation-learning theme but addresses link prediction, not seeding. Title matches.

### Linked from
- [[zhou-2026-measuring-stance-dynamics]] — complements — Item exists (influence maximization with graph embedding and GNN). Both use graph embeddings on social graphs, but Kumar addresses seed selection and the Zhou paper addresses stance classification, so same-method overstates the link.
- [[arya-2025-relink-edge-activation]] — same-method — Graph embedding and graph neural networks for influence maximization, a topical link to RELINK's embedding pipeline.
- [[chen-2021-contingency-aware-influence]] — same-method — Verified title: influence maximization using graph embedding and graph neural networks. Shares the graph-embedding approach of the paper's structure2vec backbone.
- [[chen-2023-complex-contagion-influence]] — same-method — Graph embedding and GNN encoders for IM. This paper does not cite it, so the same-method link is an analyst judgment, parallel to the GAT encoder here.
- [[kamarthi-2020-influence-maximization-unknown]] — same-method — Graph embedding plus a GNN regressor for influence maximization. The embedding is struc2vec, not DeepWalk, and labels come from SIR and IC simulation, so the shared machinery is GNN-based embedding rather than DeepWalk and GCN.