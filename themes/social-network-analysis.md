---
type: theme
name: "Social Network Analysis"
paper_count: 23
tags: [theme, type/theme]
---

# Social Network Analysis

**Social Network Analysis: literature synthesis (23 works, written 2026-10-10)**

The theme holds 23 works. They range from a 1996 expository primer ([[haythornthwaite-1996-social-network-analysis]]) to 2025-2026 papers on hypergraphs, multilayer networks and legislative citation networks. Read together, they make one argument that runs against how network outputs are usually quoted: **network position is a measurable property of how a graph was built. It is not, by default, a measure of influence, access or closeness.** The strongest legislative papers converge on this from different directions:

- Off-the-shelf centralities barely separate passed from failed House bills ([[sotoudeh-2024-network-based-measure]]).
- Those centralities lose significance once endogeneity in cosponsorship is corrected ([[battaglini-2018-effectiveness-connected-legislators]]).
- Centrality tracks resources and embeddedness, not influence ([[azaiez-2025-hypergraph-analysis-european]]).
- Centrality depends on where a topic-limited graph is cut ([[ardekani-2026-computational-graph-theoretic]]).
- A centrality ranking built from expert-weighted measures is never checked against an outcome ([[christopher-2016-mathematical-modeling-analysis]]).

This fits the project's D-025 wording rule and the June retirement of the direction-blind InfluenceScore. It does not license new influence claims.

Paul's own notes, gathered from six annotation documents that feed this theme, frame SNA as a transferable toolkit. The idea is to turn heterogeneous sources (citations, filings, tweets, donations, committee rosters) into weighted, directed or multilayer graphs, then run community detection (Louvain) and centrality. His tagged priorities are [[malaterre-2024-inferring-social-networks]] (Very High, Foundational) and [[haythornthwaite-1996-social-network-analysis]] (Medium, Theory). He repeatedly names four gaps:
1. Two-mode data must be converted into one-mode graphs before path-finding can run.
2. Temporal weighting is missing.
3. There are no rules for the sign of a tie, such as negative replies and mentions ([[fink-2023-congressional-social-network-dataset]]).
4. Directionality and causal order are unresolved ([[porter-2005-network-analysis-committees]], [[haythornthwaite-1996-social-network-analysis]]).

The corpus answers some of these well: sign (the five signed-network works) and projection bias (see [[azaiez-2025-hypergraph-analysis-european]]). It answers others poorly: temporal dynamics, path-finding and US lobbying networks.

Five strands organise the works:
1. Foundations and vocabulary.
2. Centrality as a measure of legislative power, which is contested.
3. Signed ties, balance and coalitions.
4. Group co-membership, projection and multilayer construction.
5. Ties inferred from text and interaction traces, and how ties form.

Most bearings on Paul's systems are VERIFIED or VERIFIED-SOFTENED readings against named components. The ones from [[cao-2025-social-network-group]], [[selfhout-2010-emerging-late-adolescent]] and [[vanderdoes-2022-strategic-identity-signaling]] are UNVERIFIED single-agent leads (DE-062), not findings.

Two of the corpus's methods resemble project proposals that were measured and REJECTED. Both are flagged wherever they appear:
- **Per-edge volume normalization:** Porter's normalized interlock and Aref's SDSM backbone, if they were used to fix the degree artifact.
- **Dropping regular cosponsorship edges:** the 2016 Battaglini-Patacchini avoidance of cosponsorship could be misread as support for this.

## Strands

### Foundations: what a tie is, and what the decision-making frame adds

Both works are **vocabulary sources, not evidence**. Neither has a sample that transfers to legislators, staffers or lobbyists.

[[haythornthwaite-1996-social-network-analysis]] (Paul's tag: Priority Medium, Theory) defines three attributes of a relationship: content, direction and strength. A tie bundles all the relationships between two actors. The article sets out five principles (cohesion, structural equivalence, prominence, range, brokerage) and warns that cutting tie strength at a threshold throws information away. Paul's annotation calls it thin on graph construction and flags directionality and information availability as validity threats. Used as a checklist against the built graphs, it exposes four things:
- The R Stage 3 scrape keeps sponsor-to-cosponsor direction, but `load_network` averages the two directions. The cascade kernel therefore runs undirected.
- Strength is kept as a value (original 1.0, regular 0.5, times 1/sqrt(C)), which the article endorses.
- Multiplexity never strengthens a tie. Media edges either fill pairs with no tie or take the maximum of the two weights.
- Brokerage (betweenness) lives only inside the retired InfluenceScore.

The article's split between sources that inform and close ties that legitimate (after Coleman, Katz and Menzel) loosely parallels the engine's two-factor kernel. It is a parallel, not support.

[[cao-2025-social-network-group]] is a narrative review with a bibliometric map: 917 Web of Science articles, snapshot 25 Sept 2024. It covers 'social network group decision making' among small expert panels whose trust is elicited or preset. It contributes three framings:
- Decay of trust along chains (path length, node importance) is unstudied. That is exactly the status of the staffer router's uncalibrated 0.8-per-hop factor, its 5-year half-life, and the ASKM product in the recruitment chain.
- A single most-reliable path is different from aggregating trust over many paths. The recruitment chain's own comment calls its single-path result a floor.
- 'Pseudoconsensus': agreement inside a subgroup is not agreement between subgroups. This applies to the Ward press-lean blocs, which are all labelled pro-CU.

Every Cao bearing is UNVERIFIED (DE-062).

Where they agree: both say relational structure has to be specified (direction, strength, propagation rule) before any measure means anything. Neither validates a constant.

Works: [[haythornthwaite-1996-social-network-analysis]], [[cao-2025-social-network-group]]

### Centrality as a measure of legislative power (contested)

This is the strand with the most direct bearing on Moneyball's rank pillars and Melange's Katz-Bonacich benchmark. **The works agree that generic centralities are weak or fragile signals. They disagree on whether a model-derived measure rescues the idea.**

- [[sotoudeh-2024-network-based-measure]] covers the House, 111th-115th Congresses (2009-2019), in non-overlapping 4-month windows of bills. A decayed, party-normalized pass/total cosponsorship score separates passed from failed bills (relative difference 0.248 for the max-cosponsor score versus 0.139 for the mean, at a 6-month half-life). Eigenvector, closeness and strength on the same matrix barely do (0.002-0.004). The study is descriptive only: no controls, no significance tests, no strata. A bill also counts toward its own score.
- [[battaglini-2016-influencing-connected-legislators]] is a game-theoretic NBER working paper, not peer reviewed. Interest groups pay legislators in proportion to their Bonacich centrality. Its SAR fit uses House PAC totals for the 2004-2012 cycles, on N=2,128 member-Congress rows for committee ties and N=1,166 for alumni ties. The spillover phi is 0.209 on committee ties and 0.084 on alumni ties. Degree, closeness and betweenness add nothing (R^2 stays 0.148). The authors **deliberately avoid a cosponsorship network because cosponsorship is itself driven by money** (fn. 30).
- [[battaglini-2018-effectiveness-connected-legislators]] is its empirical twin, also an NBER working paper. It covers the House, 109th-113th Congresses, N=2,176 member-Congress rows, with LES as the outcome. It does use cosponsorship, correcting it with an alumni-tie control function. Phi rises from 0.44 to 0.83 after the correction. Betweenness, closeness and eigenvector are significant naively but not after correction. Only the model-derived Katz-Bonacich term survives a horse race. Cross-party and within-party ties have similar coefficients (0.87 vs 0.83), so this is reported by stratum, not pooled.
- [[ringe-nd-pinpointing-powerful-voting]] is an online appendix only: a proof, 1,000 simulated networks per complexity class, and 897 MEPs in an unstated term. It shows a sign-blind (absolute-value) co-voting centrality picks out the cue-provider where the signed measure rewards 'majoritarians'. The authors call the results 'highly dependent on parameterization'.
- [[christopher-2016-mathematical-modeling-analysis]] ranks 8 dark-money organisations with TOPSIS over eight centralities, weighted by the authors' own AHP judgments (out-degree weight 0.502). It is never validated against an outcome, and it admits 'no significant changes' versus plain out-degree.
- [[ardekani-2026-computational-graph-theoretic]] (Paul's annotation) builds a New Zealand Acts citation network to depth 3 (6,021 of 17,709 Acts). The seed Act ranks 2nd on Katz prestige at 2 layers and leaves the top 50 at 3 layers. Centrality on a topic-cut graph therefore depends on the cut. Paul notes the network does not track lobbying, consulting, co-sponsorship or money, and that 'centrality types need more research'.

**Agreement.** Generic degree, closeness, betweenness and eigenvector centralities do not hold up against outcomes (Sotoudeh on passage, Battaglini 2016 on money, Battaglini 2018 on LES). This backs the retirement of the direction-blind InfluenceScore and the two-axis rule that keeps influence and alignment separate.

**Disagreement.**
- Cosponsorship as a substrate: Battaglini 2016 rejects it, Battaglini 2018 corrects for it, Sotoudeh uses it raw.
- Whether sign-blindness is a flaw or a feature: Ringe-nd treats it as a property of a leadership measure.

**FLAG:** Battaglini 2016's decision not to build a cosponsorship network must not be read as support for dropping regular cosponsorship edges. That proposal was measured and REJECTED in this project.

### Signed ties, structural balance and coalitions

This strand answers Paul's 'sign of ties' gap ([[fink-2023-congressional-social-network-dataset]] annotation) most directly. **The works agree that sign carries information. They disagree on whether global faction structure is real or an artifact of the baseline.**

- [[aref-2020-detecting-coalitions-optimally]] builds an SDSM signed backbone of cosponsorship for each chamber and Congress, 96th-114th (1979-2016). It solves the 2-group frustration problem exactly. Balance is high and rising: the triangle and frustration indices correlate at 0.95 in the House and 0.91 in the Senate. In session-level mediation on 19 sessions per chamber (association only), coalition partisanship partly offsets the House decline in passage (indirect beta 0.510). The Senate shows no effect, so the results are chamber-stratified.
- [[aref-2021-identifying-hidden-coalitions]] builds on it. It covers 19 House sessions (1981-2017, about 445 members each), solved to global optimality for k groups. In every session, frustrated edges fall from the party split to the optimal 2-partition to the optimal 3-partition. The third coalition (4-69 members) is ideologically fluid and is the most effective coalition on CEL LES in most sessions. It is defined by negative ties: 21.18 negative per positive tie, against 2.68 for the main blocs. The study is descriptive, with no controls. Paul's annotation values the computational efficiency and notes there is no path-finding and no temporal tracking of coalitions.
- [[leskovec-2010-predicting-positive-negative]] uses Epinions (85.0% positive edges), Slashdot (77.4%) and Wikipedia adminship votes (78.7%). Triad features predict edge sign at about 93-94% accuracy on balanced samples of edges with at least 25 common neighbours. **A global two-faction partition does not beat degree-preserving rewired networks**, so there is no significant faction structure in these online networks.
- [[tang-2015-negative-link-prediction]] builds on Leskovec's triad features. It finds that a negative interaction is a real but low-precision proxy for a negative link: the negIn rule reaches precision 0.2097 on Epinions and 0.1483 on Slashdot, and the best negative-class F1 is 0.3242 / 0.2441. Signed triads are mostly balanced (92.31% / 93.01%).
- [[ringe-2013-pinpointing-powerful-voting]] covers one European Parliament committee (32 of 65 offices responding to the 2007 staff survey; 40 final roll calls, 2004-2008; n=1,319 non-unanimous dyads). For opposing-bloc pairs, each unit of connectivity goes with about 10% lower odds of co-voting. A contact with an opponent is therefore associated with disagreement, not alignment. Bloc dominates (log-odds 2.91).

**Agreement.** Local balance holds (Tang, Leskovec). Negative ties carry information that unsigned or positive-only graphs lose (Aref 2021, Ringe 2013). Party is not the best partition (Aref).

**Tension.** Aref compares optimal partitions only against the party and one-cluster baselines. Leskovec shows a partition objective can look good while losing to rewired nulls. Any frustration result needs a rewired or sign-permuted null.

**FLAG:** the SDSM backbone in Aref 2020/2021 must not be adopted as a fix for the cosponsor network's degree artifact. That would be per-edge volume normalization, which was measured and REJECTED.

### Group co-membership, projection and multilayer construction

This strand addresses Paul's 'bipartite to one-mode' gap: how to show members and committees, or members and events, on one graph. **Each work warns that the projection step changes the answer.**

- [[porter-2005-network-analysis-committees]] (Paul cites it as Mason and Mucha) projects House member-by-committee rosters onto a committee graph for the 101st-108th Congresses. Edges are weighted by a 'normalized interlock' (observed shared members divided by the number expected at random). Clustering shows subcommittees nested inside their parents; the Intelligence cluster shows interlocks 14.4-26.6 times chance. A rank-2 SVD of the 107th House roll calls (990 votes) splits into a bipartisan axis (45.3% of the sum of squares) and a partisan axis (29.6%). No significance tests are reported. Paul's annotation raises the chicken-or-egg problem (partisanship versus committee selection) and the lack of a way to show members and committees together. **FLAG:** the normalized interlock is a per-edge size normalization. It must not be carried over to cosponsor edges, where per-edge normalization was measured and REJECTED.
- [[azaiez-2025-hypergraph-analysis-european]] covers European Commission meetings from Dec 2019 to Nov 2024 (5,717 nodes, 18,561 meetings). Its core methodological result: **clique projection inflates nodes that attend a few large meetings**, and projected coreness agrees only weakly with hypergraph coreness. Hypercoreness rises with lobbying FTE (0.464) and assets (0.253) in a standardized OLS over 1,178 companies (R^2 0.398, association only, reverse causation possible). The authors read centrality as embeddedness, not influence.
- [[baptista-2022-universal-multilayer-network]] generalises random walk with restart to any mix of multiplex and bipartite layers. Its gene and airport tests show an added layer helps only when its bipartite links overlap the base graph (23% of genes and 5% of diseases overlap in the case that failed). Top-ranked outputs were unstable across 125 parameter sets in the airport network. There is no evidence about legislatures.
- [[gaynor-2022-financial-ties-bind]] (Paul's annotation) covers House member-to-member Leadership PAC giving, 115th Congress (450 nodes, 12,870 directed edges), with an ERGM per party. Cross-party giving is almost nil. Caucus co-membership raises giving inside the caucus (Freedom Caucus +1.208, Tea Party +0.686, Progressive +0.381). Republican leaders give to 86% of members but about 30% of Freedom Caucus members. Vulnerable members (Cook PVI 'likely', Oct 2018) are about 70-75% more likely to receive a tie, retirees about 40-45% less likely. Ties are binary, and text and tables disagree in places. Paul notes the study omits committee action, voting and co-sponsorship.
- [[montgomery-2017-effects-congressional-staff]] covers House personal-office staff shared within the current or previous two Congresses, 105th-111th, using Bayesian SAR models. It finds a small senior-staff lag on LES (pooled rho 0.043, CI 0.0095-0.096) and weaker or null results otherwise. Placebos do not rule out homophily, and committee staff are excluded.

**Agreement.** The construction rule (projection, normalization, layer merge, time window) is a modelling decision with measurable consequences. Party and caucus boundaries are fault lines that should be modelled separately: Gaynor models each party on its own, and Montgomery runs one model per Congress.

**Disagreement.** Porter corrects for size per edge. Azaiez keeps the group as a hyperedge. Baptista keeps layers separate with jump parameters.

### Ties inferred from text and interaction traces; how ties form

This strand is the closest to Paul's stated pipeline of text and filings into nodes and edges. **The works agree that inferred ties are heuristics whose meaning must be bounded.**

- [[malaterre-2024-inferring-social-networks]] is Paul's Priority Very High, Foundational work. It runs a 25-topic LDA over 16,917 philosophy-of-science articles (1930-2017) and builds author profiles in four periods of about 21 years each (at most 8,009 authors). Authors are linked at Pearson r of at least 0.6, communities come from Louvain, and community histories are traced with Hellinger distance. Modularity rises from 0.427 to 0.834. Validation is only agreement with the known history of the field. The authors say HCoI ties are heuristics for possible ties, not social ties. Paul wants a temporal element and possibly domain-specific topic models. It is the method source for `press_hcoi.py`.
- [[fink-2023-congressional-social-network-dataset]] covers the 117th Congress on Twitter. The common window is 9 Feb-9 Jun 2022, with 475 members after a 100-tweet floor. The edge i to j counts j's retweets, quotes, replies and mentions of i, divided by i's tweet count. The authors call this a 'probability of influence'. **Under D-025 it must be quoted as a heuristic engagement rate.** It is one snapshot with no edge uncertainty, and the Viral Centrality approximation to the independent cascade model is asserted, not tested. Paul's notes ask for sign and temporal rules and want Viral Centrality tested for compute time and as a Monte Carlo analogue.
- [[zareie-2020-similarity-based-link]] proposes the DICN score, (1 + common neighbours) x (1 + correlation of second-order profiles). It has the highest AUC in 8 of 9 small static networks (20% hold-out, 15 repeats). Margins are about 0.01-0.055, with no variances reported.
- [[selfhout-2010-emerging-late-adolescent]] follows 205 Dutch freshmen in 10 groups over 5 monthly waves using SIENA models. Personality acts mainly through similarity between the two people (similarity effects 1.45-1.55, significant in 7-10 of 10 groups). An individual's trait level matters far less (ego .10, alter .14). Reciprocity (1.73) and closure terms are large, so pure dyadic models misfit. Bearings are UNVERIFIED.
- [[vanderdoes-2022-strategic-identity-signaling]] (n=481, 2020-era partisan Twitter users) finds that a larger outgroup share in the audience raises covert sharing (AME 0.73) and cuts total sharing. The authors warn that classifiers catch mostly overt speech. Bearings are UNVERIFIED.

**Agreement.**
- Text-derived or interaction-derived ties measure exposure or similarity, not relationships (Malaterre, Fink).
- Pairwise similarity matters more than a single person's attributes (Selfhout, Zareie).
- What actors say on observed channels is shaped by who is listening (van der Does), which limits any text-to-node pipeline.

**Tension.** Fink names its weights 'influence', while Malaterre explicitly disclaims a social reading of its ties.

## Agreements and debates

**1. Does network centrality measure power or influence?**
- *No, or only weakly, for off-the-shelf measures:*
  - [[sotoudeh-2024-network-based-measure]]: eigenvector, closeness and strength separate passed from failed bills by only 0.002-0.004. This is relative difference within 4-month windows, House 111th-115th.
  - [[battaglini-2018-effectiveness-connected-legislators]]: betweenness, closeness and eigenvector lose significance after the control function. House 109th-113th, N=2,176 member-Congress rows.
  - [[battaglini-2016-influencing-connected-legislators]]: degree, closeness and betweenness leave R^2 at 0.148. House PAC money, 2004-2012 cycles.
  - [[azaiez-2025-hypergraph-analysis-european]]: centrality reflects embeddedness and resources.
- *Yes, if the measure is derived from a model or an outcome:* the Katz-Bonacich term in Battaglini 2018 (phi 0.71 in the horse race), Sotoudeh's passage-based score, and the sign-blind cue-provider centrality in [[ringe-nd-pinpointing-powerful-voting]] (simulation only, parameter-dependent).
- *Assumed, never tested:* [[christopher-2016-mathematical-modeling-analysis]].
- *Depends on graph scope:* [[ardekani-2026-computational-graph-theoretic]].
- **Net:** no work in the theme supports reading a raw centrality rank as influence. D-025 wording holds.

**2. Is cosponsorship a valid tie substrate?**
- [[battaglini-2016-influencing-connected-legislators]] rejects it as endogenous to money.
- [[battaglini-2018-effectiveness-connected-legislators]] keeps it but corrects with an alumni-tie instrument.
- [[aref-2020-detecting-coalitions-optimally]] and [[aref-2021-identifying-hidden-coalitions]] filter it through an SDSM significance backbone.
- [[sotoudeh-2024-network-based-measure]] uses it raw.
- [[ringe-2013-pinpointing-powerful-voting]] questions it as a social-tie proxy. That is a premise dispute, not a test.
- **FLAG:** this debate does not reopen the REJECTED proposal to drop regular cosponsorship edges, nor the REJECTED per-edge volume normalization that the SDSM backbone would amount to if it were used as a degree fix.

**3. Is there real global faction structure in signed networks?**
- Aref 2020/2021 find that optimal partitions beat party in every House session, 1979-2017.
- [[leskovec-2010-predicting-positive-negative]] finds that two-faction partitions do not beat degree-preserving rewired nulls in three online networks.
- The two sets of results are not directly comparable: different substrates, and Aref uses no rewired null. Together they make a null model mandatory.
- [[tang-2015-negative-link-prediction]] adds that signs derived from interactions are noisy (negIn precision 0.21 / 0.15).

**4. Do ties transmit, or do similar actors simply connect?**
- [[montgomery-2017-effects-congressional-staff]] cannot rule out homophily: the placebo intervals overlap the headline estimates.
- [[selfhout-2010-emerging-late-adolescent]] shows that selection on similarity drives tie formation.
- [[malaterre-2024-inferring-social-networks]] disclaims any social reading of its ties.
- [[fink-2023-congressional-social-network-dataset]] labels engagement 'probability of influence' with no test.

**5. How to treat group events: project, normalise, or keep as hyperedges?**
- [[porter-2005-network-analysis-committees]] normalizes interlocks.
- [[azaiez-2025-hypergraph-analysis-european]] shows clique projection inflates attendees of large events.
- [[baptista-2022-universal-multilayer-network]] keeps layers separate.
- Unresolved for legislatures. No work runs a hypergraph treatment of cosponsorship.

**6. Do ties to opponents produce alignment?**
- [[ringe-2013-pinpointing-powerful-voting]] (one EP committee): no. Connectivity to the opposing bloc goes with about 10% lower odds of co-voting.
- [[battaglini-2018-effectiveness-connected-legislators]]: cross-party ties are about as productive as within-party ties for LES (0.87 vs 0.83).
- The outcomes differ (co-voting vs effectiveness), so this is a tension, not a contradiction.

## Methods

**Dominant methods**

1. **Centrality computation:** degree, closeness, betweenness, eigenvector, Katz/Bonacich, PageRank. Used in [[sotoudeh-2024-network-based-measure]], [[battaglini-2016-influencing-connected-legislators]], [[battaglini-2018-effectiveness-connected-legislators]], [[ringe-nd-pinpointing-powerful-voting]], [[christopher-2016-mathematical-modeling-analysis]], [[ardekani-2026-computational-graph-theoretic]] and [[azaiez-2025-hypergraph-analysis-european]].
   - *Weakness:* usually reported without an outcome test. When tested, the generic measures fail (strand 2). Attenuation and normalization choices are often not reported (Ardekani does not report alpha).
2. **Community detection and partitioning.**
   - Louvain: [[malaterre-2024-inferring-social-networks]], [[ardekani-2026-computational-graph-theoretic]], [[azaiez-2025-hypergraph-analysis-european]].
   - Single-linkage clustering: [[porter-2005-network-analysis-committees]].
   - Exact frustration minimization: [[aref-2020-detecting-coalitions-optimally]], [[aref-2021-identifying-hidden-coalitions]].
   - *Weakness:* Louvain resolution and seed are rarely reported. External validation is face-validity only (Malaterre) or weak keyword overlap (Ardekani, Jaccard 0.10-0.67). Partition objectives are compared against party baselines but not against rewired nulls (contrast [[leskovec-2010-predicting-positive-negative]]).
3. **Statistical network models that handle dependence.**
   - ERGM: [[gaynor-2022-financial-ties-bind]].
   - SIENA actor-oriented models: [[selfhout-2010-emerging-late-adolescent]].
   - Spatial autoregressive models: [[battaglini-2016-influencing-connected-legislators]], [[battaglini-2018-effectiveness-connected-legislators]], [[montgomery-2017-effects-congressional-staff]].
   - Crossed-random-effects dyad models: [[ringe-2013-pinpointing-powerful-voting]].
   - *Strength:* these are the only designs in the theme that model network dependence, and Battaglini 2018 is the only one with an endogeneity correction.
   - *Weakness:* binary ties discard weights (Gaynor). The instrument validity of alumni ties is asserted. Both Battaglini papers are unrefereed NBER working papers.
4. **Link and sign prediction.**
   - Similarity scores: [[zareie-2020-similarity-based-link]].
   - Triad-feature logistic regression: [[leskovec-2010-predicting-positive-negative]].
   - Reliability-weighted SVM: [[tang-2015-negative-link-prediction]].
   - *Weakness:* the data are small static benchmark networks or 2010-2015 online trust platforms. Evaluation is AUC-only with no variances (Zareie). None uses legislative data.
5. **Text-to-network inference.** LDA topic profiles, a correlation threshold, then Louvain ([[malaterre-2024-inferring-social-networks]]). Rule-based extraction beats an LLM extractor ([[ardekani-2026-computational-graph-theoretic]], precision 0.96 vs 0.86 in Table 1, although the text says 93%).
   - *Weakness:* thresholds (r of at least 0.6) discard strength, the very thing [[haythornthwaite-1996-social-network-analysis]] warns against. There is no ground truth.
6. **Interaction-rate edges:** [[fink-2023-congressional-social-network-dataset]].
   - *Weakness:* one snapshot, no edge uncertainty, a 100-tweet floor, and all interaction types weighted equally.
7. **Higher-order and multilayer structure:** hypergraph rich-club and hypercore analysis ([[azaiez-2025-hypergraph-analysis-european]]); random walk with restart on universal multilayer networks ([[baptista-2022-universal-multilayer-network]]).
   - *Weakness:* the null models cannot test the full network in Azaiez. Baptista's top ranks are unstable across parameter sets and its evidence is non-social.
8. **Multi-criteria decision analysis:** AHP/TOPSIS ([[christopher-2016-mathematical-modeling-analysis]]).
   - *Weakness:* the authors set the weights themselves, and there is no validation.

**Cross-cutting weaknesses**
- Most legislative studies are descriptive, with no controls or significance tests: Sotoudeh, Porter, both Aref papers, Ardekani, Malaterre.
- Causal identification is rare: only the Battaglini 2018 control function and the Montgomery placebos.
- Several works run on non-legislative populations: Selfhout, Cao, Leskovec, Tang, Baptista, van der Does.
- Senate coverage is almost absent: only [[aref-2020-detecting-coalitions-optimally]] covers the Senate, and finds no effect there.
- Temporal dynamics are thin: Sotoudeh's decay, Selfhout's waves, and the Malaterre and Aref period slices are the only temporal treatments.

## How the works relate

- [[aref-2021-identifying-hidden-coalitions]] — builds-on — [[aref-2020-detecting-coalitions-optimally]]: Same authors. The 2021 paper extends the exact 2-group frustration partition of signed cosponsorship backbones to k groups and to the minimal k.
- [[tang-2015-negative-link-prediction]] — builds-on — [[leskovec-2010-predicting-positive-negative]]: Tang uses Leskovec et al.'s 16 signed-triad features in its 45-feature set and the same Epinions and Slashdot platforms.
- [[battaglini-2018-effectiveness-connected-legislators]] — builds-on — [[battaglini-2016-influencing-connected-legislators]]: Same lead author and the same Katz-Bonacich equilibrium concept. The 2018 paper applies it to LES and adds an alumni-tie control function, but uses the cosponsorship network that the 2016 paper avoided as endogenous to money.
- [[sotoudeh-2024-network-based-measure]] — cites / shared author — [[porter-2005-network-analysis-committees]]: Porter is second author on Sotoudeh. Sotoudeh cites Porter et al. 2005 and lists committee assignments as a missing control.
- [[gaynor-2022-financial-ties-bind]] — cites — [[porter-2005-network-analysis-committees]]: Gaynor cites Porter et al. 2005 as prior network analysis of House committee assignments, and builds her own network from Leadership PAC money instead.
- [[aref-2020-detecting-coalitions-optimally]] — cites — [[porter-2005-network-analysis-committees]]: Aref and Neal cite Porter's committee network as a substrate they set aside in favour of cosponsorship.
- [[ardekani-2026-computational-graph-theoretic]] — same-method — [[malaterre-2024-inferring-social-networks]]: Paul's own cross-reference: both use Louvain community detection. Ardekani builds communities from a relational citation network, Malaterre from text similarity alone.
- [[zareie-2020-similarity-based-link]] — shared technique — [[malaterre-2024-inferring-social-networks]]: Both infer ties from the Pearson correlation of profiles. Zareie correlates structural neighbourhood vectors and multiplies by a common-neighbour term; Malaterre correlates topic profiles and applies a 0.6 threshold.
- [[baptista-2022-universal-multilayer-network]] — same-method (evaluation) — [[zareie-2020-similarity-based-link]]: Baptista scores random walk with restart with a link-prediction protocol, the same evaluation design Zareie uses.
- [[leskovec-2010-predicting-positive-negative]] — qualifies — [[aref-2021-identifying-hidden-coalitions]]: Leskovec finds that two-faction partitions of signed networks do not beat degree-preserving rewired nulls. Aref compares its optimal partitions only with party and lower-k baselines, so its coalition result needs that null.
- [[leskovec-2010-predicting-positive-negative]] — qualifies — [[aref-2020-detecting-coalitions-optimally]]: Both use a frustration-type partition objective. Leskovec finds no global faction signal against rewired nulls, so any partition result on signed data needs a null model.
- [[tang-2015-negative-link-prediction]] — complements — [[aref-2021-identifying-hidden-coalitions]]: Aref's third coalition is defined by negative ties. Tang shows negative signs derived from interactions are real but low-precision proxies for antagonistic links.
- [[sotoudeh-2024-network-based-measure]] — agrees — [[battaglini-2018-effectiveness-connected-legislators]]: Both find off-the-shelf centralities (eigenvector, closeness and others) weak against legislative outcomes, and that a model-derived or outcome-derived measure does better. The outcomes differ: bill passage in Sotoudeh, LES in Battaglini.
- [[battaglini-2016-influencing-connected-legislators]] — disagrees on substrate — [[sotoudeh-2024-network-based-measure]]: Battaglini and Patacchini refuse to build their network from cosponsorship because cosponsorship is driven by money. Sotoudeh builds its influence measure directly from cosponsorship.
- [[ringe-2013-pinpointing-powerful-voting]] — premise dispute — [[sotoudeh-2024-network-based-measure]]: Ringe et al. question cosponsorship as a proxy for social ties but do not test Sotoudeh's measure. This is a dispute over premises, not a contradiction.
- [[ringe-2013-pinpointing-powerful-voting]] — same author line, European Parliament — [[ringe-nd-pinpointing-powerful-voting]]: Both share first author Ringe and use European Parliament data on cue-taking and co-voting. Whether they use the same dataset is not verified, and the slug and title labels of the two items appear swapped or misassigned (see gaps).
- [[ringe-nd-pinpointing-powerful-voting]] — complements — [[aref-2021-identifying-hidden-coalitions]]: Both treat negative ties as informative. Ringe-nd scores nodes from signed co-voting correlations; Aref partitions signed cosponsorship networks into coalitions.
- [[ringe-nd-pinpointing-powerful-voting]] — contrasts — [[christopher-2016-mathematical-modeling-analysis]]: Ringe-nd tests whether centrality recovers a known cue-provider, though only in simulation. Christopher assumes centrality stands for power and weights centralities by expert judgment, with no outcome test.
- [[christopher-2016-mathematical-modeling-analysis]] — contrasts — [[ardekani-2026-computational-graph-theoretic]]: Both face the choice of which centrality to use. Ardekani chooses Katz on substantive grounds; Christopher blends eight centralities using AHP weights the authors set themselves.
- [[azaiez-2025-hypergraph-analysis-european]] — qualifies — [[porter-2005-network-analysis-committees]]: Porter projects member-by-committee rosters onto a one-mode graph. Azaiez shows that projecting group events onto pairwise cliques changes core rankings relative to a hypergraph treatment.
- [[azaiez-2025-hypergraph-analysis-european]] — qualifies — [[sotoudeh-2024-network-based-measure]]: Sotoudeh's cosponsorship network is a projection of bill rosters. Azaiez gives a general reason why centrality on a projected graph can mislead.
- [[azaiez-2025-hypergraph-analysis-european]] — complements (reverse direction) — [[battaglini-2016-influencing-connected-legislators]]: Battaglini models money allocated according to legislators' centrality. Azaiez finds interest-group resources associated with the groups' own centrality, as association only with reverse causation acknowledged.
- [[montgomery-2017-effects-congressional-staff]] — same-method / same outcome — [[battaglini-2018-effectiveness-connected-legislators]]: Both fit spatial autoregressive models of House LES on a member network. Montgomery's shared-staff network is a substrate less endogenous than cosponsorship.
- [[montgomery-2017-effects-congressional-staff]] — same-method — [[battaglini-2016-influencing-connected-legislators]]: Both use spatial autoregressive models with a row-standardized member network on House data.
- [[gaynor-2022-financial-ties-bind]] — complements — [[aref-2021-identifying-hidden-coalitions]]: Both find structure below the party level: intraparty caucus factions in Leadership PAC ties (Gaynor), and a fluid third coalition in signed cosponsorship (Aref).
- [[christopher-2016-mathematical-modeling-analysis]] — complements — [[gaynor-2022-financial-ties-bind]]: Both analyse directed money-flow networks among political organisations: nonprofit dark-money transfers in Christopher, member Leadership PAC giving in Gaynor.
- [[haythornthwaite-1996-social-network-analysis]] — extended-by — [[aref-2021-identifying-hidden-coalitions]]: Haythornthwaite describes ties by content, direction and strength, with no sign dimension. Signed-network methods add sign.
- [[selfhout-2010-emerging-late-adolescent]] — complements — [[haythornthwaite-1996-social-network-analysis]]: Haythornthwaite gives the directed, tie-strength framing. Selfhout estimates reciprocity and transitivity in a longitudinal directed-tie model rather than assuming them.
- [[cao-2025-social-network-group]] — complements — [[aref-2021-identifying-hidden-coalitions]]: Cao's 'pseudoconsensus' warning (agreement inside a subgroup is not agreement between subgroups) is what signed partitioning between subgroups addresses.
- [[vanderdoes-2022-strategic-identity-signaling]] — complements (same medium) — [[fink-2023-congressional-social-network-dataset]]: Fink builds congressional Twitter interaction ties. Van der Does implies that the content on such ties is shaped by the audience, with mixed audiences producing more covert political posts.
- [[leskovec-2010-predicting-positive-negative]] — complements — [[zareie-2020-similarity-based-link]]: Leskovec's signed triad features address the negative-tie question that Zareie's unsigned similarity score leaves open.
- [[tang-2015-negative-link-prediction]] — complements — [[zareie-2020-similarity-based-link]]: Tang predicts negative links from positive links and interaction content, filling the signed-tie gap in Zareie's method.
- [[baptista-2022-universal-multilayer-network]] — complements — [[ardekani-2026-computational-graph-theoretic]]: Baptista's random walk with restart is personalized PageRank; Ardekani uses Katz prestige. Together they frame the PageRank-versus-Katz choice.
- [[malaterre-2024-inferring-social-networks]] — complements — [[aref-2021-identifying-hidden-coalitions]]: HCoI topic profiles carry no sign. Aref's generalized-balance partition encodes agreement and disagreement. The two are complementary routes to recovering alignment.
- [[ringe-nd-pinpointing-powerful-voting]] — same data type — [[porter-2005-network-analysis-committees]]: Both extract structure from roll-call matrices: Porter through a rank-2 SVD (bipartisan and partisan axes), Ringe through co-voting phi-correlation centrality.

## What it means for the systems

Status labels follow the bearings in the theme file. VERIFIED-SOFTENED and VERIFIED are checked readings against named components. **UNVERIFIED items are single-agent leads, not findings (DE-062).** Nothing below proposes a change. Rulings remain Paul's.

**Melange: cosponsor network ([[systems/melange-sna]], `melange-sna.cosponsor-network`)**
- [[sotoudeh-2024-network-based-measure]] is cited as the source of the edge weights, but the built weights depart from it on nearly every parameter:
  - an additive x3 success multiplier, not a pass/total ratio;
  - a per-Congress decay of 0.8 (half-life about 6 years), not 6-24 months;
  - a 1/sqrt(C) size discount;
  - unequal role weights;
  - no party normalization.
- Code comments also cite Sotoudeh for recruitment chains and for influence versus cosponsor count, which the paper never tests. Paul's 6-month half-life note transfers a decay that applies to accruing influence, not to stance salience. (VERIFIED-SOFTENED / VERIFIED)
- [[haythornthwaite-1996-social-network-analysis]]: sponsor-to-cosponsor direction is scraped and then averaged away in `load_network`.
- [[battaglini-2018-effectiveness-connected-legislators]]: the live graph is used without any endogeneity correction.
- [[azaiez-2025-hypergraph-analysis-european]] supports the existing 1/sqrt(C) discount for large rosters.
- **FLAGS:** two ideas in this corpus must not be carried onto cosponsor edges.
  - Porter's normalized interlock and Aref's SDSM backbone would amount to per-edge volume normalization, which was measured and REJECTED.
  - Battaglini 2016's avoidance of cosponsorship is not an argument for dropping regular cosponsorship edges, which was also REJECTED.

**Moneyball: InfluenceScore retirement and the two-axis rule ([[systems/moneyball-rank-pillars]])**
- Supported by [[sotoudeh-2024-network-based-measure]], [[battaglini-2016-influencing-connected-legislators]], [[battaglini-2018-effectiveness-connected-legislators]], [[azaiez-2025-hypergraph-analysis-european]] and [[christopher-2016-mathematical-modeling-analysis]]. The retired score has the same shape as Christopher's composite: hand-weighted centralities led by out-degree.
- [[ringe-nd-pinpointing-powerful-voting]] treats sign-blindness as a property of a leadership measure, which fits keeping influence and alignment on separate axes.
- Any check of InfluenceScore against bill passage would be circular, because the edges are already weighted by passage.

**Melange: Katz-Bonacich benchmark (`fec/engine/kb_benchmark.py`, [[systems/melange-sna]])** (VERIFIED-SOFTENED)
- Its method source is [[battaglini-2016-influencing-connected-legislators]], but it departs from the paper in three ways:
  - it runs on the cosponsorship kernel the paper avoids;
  - it pools the House and Senate, while the paper is House-only;
  - it sets attenuation at 0.85 of the convergence bound, against an estimated spillover phi of 0.08-0.25.
- Stored results, file 2026-08-18: Spearman rho 0.135 against CU money and 0.230 against bank plus financial-services money, n=533 members across both chambers. **These are UNSTRATIFIED, single-path and stale.**
- [[ardekani-2026-computational-graph-theoretic]]: centrality on the CU-only kernel depends on that cut.
- [[battaglini-2018-effectiveness-connected-legislators]] suggests a cheap House-only check of Katz-Bonacich against LES, since CEL LES data are already on disk. It has not been run.

**Melange: signed coalitions (`fec/network/signed_coalitions.py`, [[systems/melange-sna]])**
- The script cites [[aref-2021-identifying-hidden-coalitions]] but differs from it in four ways:
  - it partitions the LLM-valenced member-mention layer, which is 83% positive by edge count, not a signed cosponsorship backbone;
  - it pools the House and Senate;
  - it uses a heuristic local search capped at k=3 with no lower bound;
  - it has no rewired null ([[leskovec-2010-predicting-positive-negative]]) and no evidence-count floor on signs ([[tang-2015-negative-link-prediction]]).
- Artifact of 2026-07-19: one main bloc plus a 14-member splinter at k=2. At k=3 the third cluster has 1 member, and weighted frustration moves from 1040.79 to 1039.79. **The paper's finding has not been reproduced or tested and stays UNVERIFIED.**
- [[aref-2020-detecting-coalitions-optimally]] suggests an exact solve may be tractable at 520 members and 7,153 edges. That estimate is derived, not tested, and is UNVERIFIED.
- The same paper says the earlier vote backbone being about 95% party is expected under balance. [[ringe-nd-pinpointing-powerful-voting]] and Aref 2021 argue that dropping the negative side threw away information about cue-providers and coalitions.

**Melange: press HCoI (`fec/network/press_hcoi.py`, [[systems/melange-sna]])**
- Built from [[malaterre-2024-inferring-social-networks]], using 19 regex-tagged CU press categories in place of LDA topics.
- Recorded system results agree with the authors' own caveat: the layer was null for stance co-movement (49%) and recovered 4 of 41 cosponsor-isolated members. HCoIs are therefore not coalitions, closeness or influence channels.
- [[ardekani-2026-computational-graph-theoretic]]: every Louvain partition should report its modularity and an external alignment check. HCoI reports neither. (VERIFIED-SOFTENED, single-agent per its own note)

**Melange: social and press engagement layers ([[systems/melange-sna]], [[systems/melange-stance]])**
- [[fink-2023-congressional-social-network-dataset]] is the method source for both.
  - `press_engagement_layer.py` uses Fink's denominator, the source's post volume.
  - `social_interaction_layer.py` divides by the engaging member's own volume (the opposite end of the dyad), uses a 25-post floor rather than 100, weights mentions 0.4, and skips quotes and replies.
  - Both layers are mean-symmetrized, which discards direction.
- This reading comes from code only and is UNVERIFIED.
- Fink's label 'probability of influence' must be shown as a heuristic engagement rate (D-025).
- [[baptista-2022-universal-multilayer-network]]: the multilayer builder merges cosponsor, engagement, HCoI, social and co-appearance layers into one weight without a held-out test of each layer. Each layer should show it adds value, including how much it overlaps the base graph.

**Melange: staffer network ([[systems/melange-staffer-network]])**
- [[azaiez-2025-hypergraph-analysis-european]] is direct evidence on the open owner ruling about container-size normalization in co-tenure weights. It also helps explain the recorded r = 0.87 between staffer degree and office size. The ruling remains Paul's. The evidence concerns staffer containers and does not carry over to cosponsor edges.
- [[montgomery-2017-effects-congressional-staff]] supports the OVERLAP rule: a sequence of offices is a trajectory, not a person-to-person edge. It finds only a weak signal confounded by homophily (House 105th-111th, personal staff only). Shared-staff counts are descriptive, not transmission channels. **Influence language is barred on revolving-door edges.**
- [[baptista-2022-universal-multilayer-network]]: layer-balanced PageRank is checked for sensitivity only on damping, never on how mass is split across layers.
- [[cao-2025-social-network-group]] (UNVERIFIED): the 0.8-per-hop decay and the 5-year half-life are uncalibrated ([[systems/melange-pathfinding]]).

**Melange influence engine, receptivity and competitiveness ([[systems/melange-influence-engine]])**
- [[gaynor-2022-financial-ties-bind]] (VERIFIED) supports Cook PVI as a vulnerability signal that donors act on. It does **not** support the claim that a dollar buys more in a competitive seat. Reciprocity is NOT validated (D-104..D-108).
- [[ringe-2013-pinpointing-powerful-voting]] cautions against reading cross-party ties as conduits for alignment in the cascade kernel.
- [[selfhout-2010-emerging-late-adolescent]] (UNVERIFIED): every receptivity input describes one member, while tie formation is driven by similarity between pairs.

**Melange: cosponsor forecaster**
- The forecaster lacks the following features, and every lead stays UNVERIFIED until reproduced under the forecaster's own temporal split:
  - dyadic common-neighbour and second-order similarity ([[zareie-2020-similarity-based-link]]);
  - signed triad features ([[leskovec-2010-predicting-positive-negative]], [[tang-2015-negative-link-prediction]]);
  - reciprocity and closure terms ([[selfhout-2010-emerging-late-adolescent]]).

**Moneyball committee pillar and say-do gate ([[systems/moneyball-rank-pillars]], [[systems/melange-psychometrics]])**
- [[porter-2005-network-analysis-committees]]: subcommittee seats sit largely inside the parent committee, yet are added on top at 1/6 weight.
- [[battaglini-2018-effectiveness-connected-legislators]]: the committee-chair effect shrinks about 18% (1.97 to 1.61) once the network enters the model.
- Porter's dominant bipartisan roll-call axis makes a single final-passage Yea weak corroboration in the say-do gate.
- [[vanderdoes-2022-strategic-identity-signaling]] (UNVERIFIED): the press pillar and the DEBATE stance reader may understate members facing mixed audiences. The say-do gate only lowers tiers, so it cannot correct for this.

**Mentat ([[systems/mentat-fec-attribution]], [[systems/melange-lobbying]])**
- [[gaynor-2022-financial-ties-bind]]: Mentat's FEC leadership-PAC sponsor links could rebuild the member-to-member LPAC network. It would be a donation-tie layer, not a closeness or influence measure.
- [[christopher-2016-mathematical-modeling-analysis]]: the FEC transfer graph cannot see the nonprofit dark-money layer.
- [[azaiez-2025-hypergraph-analysis-european]]: the EC register names both parties to each meeting. LD-2 filings do not, so LDA stays an identity source, not an edge or weight source (D-021).

**Effectiveness shadow ([[systems/melange-legislative-data]])**
- [[aref-2021-identifying-hidden-coalitions]], [[battaglini-2018-effectiveness-connected-legislators]] and [[montgomery-2017-effects-congressional-staff]] all relate network structure to LES, and the CEL LES data are on disk. These are candidate extensions, not results.

## Gaps

**Gaps in Paul's notes and in the source mapping**
- Paul's own annotation text exists for only some works in this theme:
  - With annotations: [[haythornthwaite-1996-social-network-analysis]], [[malaterre-2024-inferring-social-networks]], [[ardekani-2026-computational-graph-theoretic]], [[gaynor-2022-financial-ties-bind]], [[porter-2005-network-analysis-committees]] (annotated as 'Mason and Mucha'; the citation lists three authors but the annotation names four), [[fink-2023-congressional-social-network-dataset]], and 'Aref and Neal' (one note; it is unclear whether he meant the 2020 or 2021 paper).
  - Bare citations only: [[battaglini-2016-influencing-connected-legislators]] and [[zareie-2020-similarity-based-link]].
  - No notes in any of the attached documents: the remaining works.
- The theme's paul_notes block attaches 13 documents. Six are not literature: the staffer-network plan of record (14bfbd31b736), the staffer next-steps memo (55280df284a0), the Telecom scope note, the crypto forecast recheck, the forecast-collection recheck, and the cross-industry finance memo. A seventh, 'Neural Networks', is four bare links. The sha mapping should be checked.
- Label problems:
  - [[ringe-2013-pinpointing-powerful-voting]] has no title, and its content is Ringe, Victor and Gross (BJPS 2013) on ties to allies and adversaries, not 'Pinpointing the Powerful'.
  - [[ringe-nd-pinpointing-powerful-voting]] is only the online appendix to Ringe and Wilson. The main article is not in the vault.
  - Haythornthwaite's printed year is 1998.
  - Both Battaglini papers are unrefereed NBER working papers.
- [[vanderdoes-2022-strategic-identity-signaling]]: an earlier relation pass said the text was blank. That result is stale.

**Gaps Paul names that the corpus does not close**
- **Converting two-mode data to one-mode graphs for path-finding.** [[porter-2005-network-analysis-committees]] and [[azaiez-2025-hypergraph-analysis-european]] diagnose the projection problem, but no work gives a projection a path-finder can use. None of the 23 works is a path-finding paper; [[cao-2025-social-network-group]] only names how propagation operators are built.
- **Temporal weighting** (Fink, Malaterre). Only [[sotoudeh-2024-network-based-measure]] (exponential decay), [[selfhout-2010-emerging-late-adolescent]] (actor-oriented models over waves) and period slicing (Malaterre, Aref) address time. There is no tracking of coalitions over time ([[aref-2021-identifying-hidden-coalitions]]).
- **Sign rules for replies and mentions.** [[tang-2015-negative-link-prediction]] and [[leskovec-2010-predicting-positive-negative]] are online trust platforms. Congress has no ground truth for declared foes.

**Substantive gaps in the literature**
- **US lobbying networks.** The only lobbying network here is European ([[azaiez-2025-hypergraph-analysis-european]]). The LDA-based works (Bacik, Kim and Kunisky) sit outside this theme, and LD-2 cannot supply member edges (D-021).
- **The Senate.** Only [[aref-2020-detecting-coalitions-optimally]] covers it, and finds no effect there. Any pooled House-plus-Senate figure is UNSTRATIFIED.
- **Causal identification.** Only [[battaglini-2018-effectiveness-connected-legislators]] (control function) and [[montgomery-2017-effects-congressional-staff]] (placebos) attempt it. Everything else is descriptive.
- **Reciprocity and the value of money by seat competitiveness.** No work tests either. D-104..D-108 remain unvalidated.
- **Committee-staff networks** are excluded by Montgomery. The staffer plan treats committee payrolls as the likely source of cross-party ties, and no literature here tests that.
- **Hypergraph or multilayer treatment of cosponsorship.** No legislative work tests whether added layers improve prediction ([[baptista-2022-universal-multilayer-network]] is biological and airport data).
- **Covert signalling among legislators.** [[vanderdoes-2022-strategic-identity-signaling]] studies ordinary partisan users.

**Cheap checks the works point to (not run; all would stay UNVERIFIED until a second path reproduces them)**
1. A House-only replication of Katz-Bonacich against LES using CEL data ([[battaglini-2018-effectiveness-connected-legislators]]).
2. A House-only signed cosponsorship backbone with an exact k-partition ([[aref-2021-identifying-hidden-coalitions]]). It must be scoped so it does not become the REJECTED per-edge normalization of live cosponsor weights.
3. A rewired and sign-permuted null for `signed_coalitions.py` ([[leskovec-2010-predicting-positive-negative]]).
4. A test of whether negative-reference dyads sit close together in the cosponsor graph ([[tang-2015-negative-link-prediction]]).
5. Per-layer ablation and overlap checks for the multilayer builder and for layer-balanced PageRank ([[baptista-2022-universal-multilayer-network]]).
6. Reporting modularity plus an external alignment check for press HCoI ([[ardekani-2026-computational-graph-theoretic]]).

## All works

- [[azaiez-2025-hypergraph-analysis-european]] (2025) — A hypergraph analysis of the European Commission lobby network
- [[aref-2021-identifying-hidden-coalitions]] (2021) — Identifying hidden coalitions in the US House of Representatives by optimally partitioning signed networks based on generalized balance
- [[zareie-2020-similarity-based-link]] (2020) — Similarity-based link prediction in social networks using latent relationships between the users
- [[baptista-2022-universal-multilayer-network]] (2022) — Universal multilayer network exploration by random walk with restart
- [[haythornthwaite-1996-social-network-analysis]] (1996) — Social Network Analysis: An Approach and Technique for the Study of Information Exchange
- [[sotoudeh-2024-network-based-measure]] (2024) — A Network-Based Measure of Cosponsorship Influence on Bill Passing in the United States House of Representatives
- [[malaterre-2024-inferring-social-networks]] (2024) — Inferring social networks from unstructured text data: A proof of concept detection of hidden communities of interest
- [[ardekani-2026-computational-graph-theoretic]] (2026) — Computational and Graph-Theoretic Analysis of Legislative Networks: New Zealand's Mental Health Act as a Case Study
- [[gaynor-2022-financial-ties-bind]] (2022) — The (Financial) Ties That Bind: Social Networks of Intraparty Caucuses
- [[porter-2005-network-analysis-committees]] (2005) — A network analysis of committees in the U.S. House of Representatives
- [[battaglini-2016-influencing-connected-legislators]] (2016) — Influencing Connected Legislators
- [[cao-2025-social-network-group]] (2025) — Social network group decision making: Characterization, taxonomy, challenges and future directions from an AI and LLMs perspective
- [[leskovec-2010-predicting-positive-negative]] (2010) — Predicting Positive and Negative Links in Online Social Networks
- [[tang-2015-negative-link-prediction]] (2015) — Negative Link Prediction in Social Media
- [[ringe-2013-pinpointing-powerful-voting]] (2013) — 
- [[christopher-2016-mathematical-modeling-analysis]] (2016) — Mathematical modeling and analysis of a dark money network
- [[selfhout-2010-emerging-late-adolescent]] (2010) — Emerging Late Adolescent Friendship Networks and Big Five Personality Traits: A Social Network Approach
- [[ringe-nd-pinpointing-powerful-voting]] () — Pinpointing the Powerful: Co-voting Network Centrality as a Measure of Political Influence (online appendix)
- [[aref-2020-detecting-coalitions-optimally]] (2020) — Detecting coalitions by optimally partitioning signed networks of political collaboration
- [[vanderdoes-2022-strategic-identity-signaling]] (2022) — Strategic identity signaling in heterogeneous networks
- [[fink-2023-congressional-social-network-dataset]] (2023) — A Congressional Twitter network dataset quantifying pairwise probability of influence
- [[battaglini-2018-effectiveness-connected-legislators]] (2018) — Effectiveness of Connected Legislators
- [[montgomery-2017-effects-congressional-staff]] (2017) — The Effects of Congressional Staff Networks in the U.S. House of Representatives
