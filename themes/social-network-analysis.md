---
type: theme
name: "Social Network Analysis"
paper_count: 21
tags: [theme]
---

# Social Network Analysis

The theme holds 21 works. Paul's annotated bibliographies describe a single ambition: turn heterogeneous political records (cosponsorship, committee rosters, donations, tweets, press text, lobbying filings) into graphs. The graphs would then support communities, coalitions, champions, path-finding and allocation. In his notes, community detection (Louvain) and centrality are the default tools. He tags [[malaterre-2024-inferring-social-networks]] as "Priority Very High, Foundational". His recurring open problems are:

- converting bipartite or multi-layer data into a graph that path-finding can run on;
- temporal weighting;
- the sign of edges;
- which centrality to use.

Read together, the works answer those problems more sceptically than the original plan assumed. Four messages repeat across independent papers:

1. **Construction decides the result.** Projecting group events (meetings, bills, committees) onto pairwise ties changes who looks central ([[azaiez-2025-hypergraph-analysis-european]], [[porter-2005-network-analysis-committees]]). So does cutting a graph to one topic ([[ardekani-2026-computational-graph-theoretic]]), and so does adding a layer whose links barely overlap the base graph ([[baptista-2022-universal-multilayer-network]]).
2. **Off-the-shelf centrality is a weak stand-in for influence.** It barely separates passed from failed House bills ([[sotoudeh-2024-network-based-measure]]). Degree, closeness and betweenness explain no PAC money ([[battaglini-2016-influencing-connected-legislators]]). In the EC register, centrality tracks resources and embeddedness ([[azaiez-2025-hypergraph-analysis-european]]). The signed form rewards majoritarians over cue-providers ([[ringe-nd-pinpointing-powerful-voting]]). This backs the June 2026 retirement of cosponsor centrality from the Influence pillar. It also backs D-025: graph ranks are heuristic model outputs, never closeness, access or introduction probabilities.
3. **Sign matters, but observed negative signals are noisy.** Negative ties define the House's fluid third coalition ([[aref-2021-identifying-hidden-coalitions]]). Contact with adversaries goes with *less* co-voting ([[ringe-2013-pinpointing-powerful-voting]]). But negative interactions are low-precision proxies for negative ties ([[tang-2015-negative-link-prediction]]), and partitions need a rewired null before a faction claim can stand ([[leskovec-2010-predicting-positive-negative]]).
4. **Text-inferred ties are attention similarity, not relationships.** [[malaterre-2024-inferring-social-networks]] says this outright. Two system nulls agree with it: 49% stance co-movement on the HCoI layer, and 4 of 41 cosponsor-isolated members recovered.

Most works are descriptive and drawn from other domains: online trust platforms, biology and airports, philosophy of science, adolescent friendship. Only a handful use inferential network-dependence models: [[gaynor-2022-financial-ties-bind]] (ERGM), [[selfhout-2010-emerging-late-adolescent]] (SAOM), [[battaglini-2016-influencing-connected-legislators]] (SAR) and [[ringe-2013-pinpointing-powerful-voting]] (crossed random effects). None of the 21 has a causal design.

Two caveats apply to everything that follows. Several system bearings in the file are single-agent readings. Those marked UNVERIFIED are leads, not findings (DE-062). Two works ([[cao-2025-social-network-group]] and [[vanderdoes-2022-strategic-identity-signaling]]) and parts of [[selfhout-2010-emerging-late-adolescent]] carry only UNVERIFIED bearings.

Housekeeping:
- The slug [[ringe-2013-pinpointing-powerful-voting]] has no title metadata. Its summary identifies it as Ringe, Victor and Gross (BJPS 2013).
- [[vanderdoes-2022-strategic-identity-signaling]] is van der Does, Galesic, Dunivin and Smaldino (PNAS 2022).
- Paul's own notes name several SNA works that are not in this theme file: Kim and Kunisky, Bacik et al., Bonica, Valujeva et al., Zhang et al., Huneeus and Kim, Furnas et al., Oropeza-Martinez et al., and Rongkuan et al.

## Strands

### Constructing the graph: projection, co-membership, hypergraphs and layers

Works: [[porter-2005-network-analysis-committees]], [[azaiez-2025-hypergraph-analysis-european]], [[baptista-2022-universal-multilayer-network]], [[haythornthwaite-1996-social-network-analysis]], [[fink-2023-congressional-social-network-dataset]], [[gaynor-2022-financial-ties-bind]], [[ardekani-2026-computational-graph-theoretic]]

This strand asks how political records become nodes and edges. Paul's repeated stated gap is the step from a bipartite or two-mode source to a graph that algorithms can use. He raises it for Kim and Kunisky, and for Mason, Porter and Mucha (members and committees on one graph).

**Projection is the default, and it distorts.**
- [[porter-2005-network-analysis-committees]] projects the member-by-committee network for the 101st-108th Houses onto committee pairs. It weights each pair by a "normalized interlock" (observed shared members divided by the count expected by chance) and finds nested levels of structure. The Intelligence cluster runs at 14.4-26.6 times chance. No interlock has a significance test.
- [[azaiez-2025-hypergraph-analysis-european]] keeps EC meetings (Dec 2019 - Nov 2024; 5,717 nodes, 18,561 meetings) as hyperedges. It shows that clique projection inflates nodes that attend a few large meetings: coreness on the projection agrees only weakly with hypercoreness. This is the most portable result in the theme for Melange. Melange's cosponsor network and its staffer co-tenure containers are both clique projections. The paper is direct evidence for the open owner ruling on staffer container-size normalization, and it helps explain the recorded r = 0.87 between staffer degree and office size.

**Multiplexity and layers.**
- [[haythornthwaite-1996-social-network-analysis]] defines a tie as a bundle of relations with content, direction and strength. It warns against thresholding strength. Read against the built graphs: Melange keeps strength (role weights scaled by 1/sqrt(C)) but loses direction (keys are sorted and the two directions averaged) and multiplexity (a pair tied in several channels never gets a stronger tie).
- [[baptista-2022-universal-multilayer-network]] adds the test discipline. A layer helps only when its bipartite links overlap the base graph: only 23% of genes and 5% of diseases are shared in the failing biological case. Randomizing the links erases the gain. The Congress multilayer builder merges five layers into one weight. No held-out test checks whether any layer earns its place, and the staffer PageRank is checked for sensitivity on damping only, never on how mass is split across layers.

**Directed, normalized edges from behaviour.**
- [[fink-2023-congressional-social-network-dataset]] builds a directed Twitter interaction graph for the 117th Congress (window Feb 9 - June 9 2022; 475 members after a 100-tweet floor). Each edge from i to j counts j's interactions with i's tweets and is divided by i's tweet count. It is the method source for press_engagement_layer.py.
- social_interaction_layer.py cites Fink but divides by the engaging member's own volume, which is the opposite end of the dyad. It also uses a 25-post floor and drops quotes and replies.
- Both layers are mean-symmetrized, which discards Fink's direction. The denominator reading comes from code only and is UNVERIFIED (DE-062).

**Money and co-membership as ties.** [[gaynor-2022-financial-ties-bind]] builds the member-to-member Leadership PAC network for the 115th House: 450 nodes and 12,870 directed edges, with ties treated as binary and weights ignored. Mentat already holds the FEC sponsor links needed to rebuild it. [[ardekani-2026-computational-graph-theoretic]] shows how a breadth-first cut from a seed node shapes every downstream statistic. In its New Zealand Acts graph the seed Act ranks 2nd at 2 layers and falls out of the top 50 at 3.

**Caution.** Porter's normalized interlock is a per-edge size normalization. Per-edge volume normalization of cosponsor edges was measured in this project and REJECTED. Porter must not be cited to reopen it. Azaiez's projection-inflation result bears on the staffer container question, not on that rejected cosponsor proposal.

### Does network position measure influence? Centrality against outcomes

Works: [[sotoudeh-2024-network-based-measure]], [[battaglini-2016-influencing-connected-legislators]], [[ringe-nd-pinpointing-powerful-voting]], [[christopher-2016-mathematical-modeling-analysis]], [[ardekani-2026-computational-graph-theoretic]], [[azaiez-2025-hypergraph-analysis-european]], [[haythornthwaite-1996-social-network-analysis]]

Paul's notes treat centrality choice as an open research item: "centrality types need more research" (Ardekani), and Viral Centrality needs testing (Fink). The works converge on a negative answer for generic centralities and a conditional one for theory-derived measures.

**Evidence against generic centrality.**
- [[sotoudeh-2024-network-based-measure]] (111th-115th House, 2009-2019, non-overlapping 4-month windows). Eigenvector, closeness and strength centralities on the pass/total matrix barely separate passed from failed bills: relative difference 0.002-0.004. An outcome-built pairwise score does much better, 0.248 for max-cosponsor at a 6-month half-life. The study is descriptive, with no controls or tests, and has leakage: a bill counts toward its own score, and edges can include later passage.
- [[battaglini-2016-influencing-connected-legislators]] (House, 2004-2012 cycles) gives a game-theoretic reason why interest groups should pay legislators in proportion to Bonacich centrality. Degree, closeness and betweenness add nothing to a PAC-money regression (R^2 stays at 0.148). It is an NBER working paper and not peer-reviewed.
- [[azaiez-2025-hypergraph-analysis-european]] (1,178 EC-registered companies) finds hypercoreness tracks lobbying FTE (standardized 0.464) and assets (0.253). It reads centrality as embeddedness driven by resources, association only. Its own Discussion slips into "access and influence" wording, which must not carry over.
- [[ringe-nd-pinpointing-powerful-voting]] shows in simulation (1,000 networks per class, 100 legislators) that signed co-voting centrality can rank majoritarians above the true cue-provider. A sign-blind (absolute-value) measure recovers the cue-provider. The authors call this "highly dependent on parameterization".

**Construction-dependence.** [[ardekani-2026-computational-graph-theoretic]] uses in-direction Katz prestige on a citation graph, which differs from Battaglini's out-direction Katz-Bonacich. The two must not be conflated in kb_benchmark.py.

**What not to do.** [[christopher-2016-mathematical-modeling-analysis]] ranks nodes of a dark-money network with an AHP-weighted TOPSIS over eight centralities, led by out-degree (weight 0.502). It never checks the composite against an outcome. That is the same shape as the retired cosponsor InfluenceScore. Its consistency ratio and one-weight sensitivity sweep are documentation devices, not validation.

[[haythornthwaite-1996-social-network-analysis]] supplies the vocabulary: betweenness as brokerage, and degree and closeness as prominence. It tests none of them.

**Net reading.** Across House cosponsorship, House PAC money and the EC register, the three empirical centrality studies agree on one point: standard centralities are weak, unstratified signals of the outcomes studied, each measured in its own window and population. This supports the system's two-axis rule (influence and alignment kept separate), the retirement of cosponsor centrality from the Influence pillar, and D-025 wording. Any check of a passage-weighted InfluenceScore against bill passage would be circular, because the edges are already weighted by passage.

### Signed networks, balance and hidden coalitions

Works: [[aref-2020-detecting-coalitions-optimally]], [[aref-2021-identifying-hidden-coalitions]], [[leskovec-2010-predicting-positive-negative]], [[tang-2015-negative-link-prediction]], [[ringe-2013-pinpointing-powerful-voting]], [[ringe-nd-pinpointing-powerful-voting]], [[cao-2025-social-network-group]]

Paul's stated gaps include the sign of reply and mention edges, and how to score agreement when sentiment is negative (Fink). This strand is where the theme speaks most directly to the built diagnostic signed-coalition layer.

**The Aref and Neal programme.**
- [[aref-2020-detecting-coalitions-optimally]] builds signed SDSM backbones of cosponsorship for each chamber, 96th-114th Congress (1979-2016), and computes the exact two-group frustration index. Balance is high and rising, which the authors read as polarization. Exact House solves take up to about 13.5 h (113th Congress, 75,771 edges); Senate solves take about 4 s.
- [[aref-2021-identifying-hidden-coalitions]] extends this to k groups for 19 House sessions (1981-2017). In every session, party fits worse than the optimal 2-partition, which fits worse than the optimal 3-partition. The small third coalition (4-69 members) is ideologically fluid. It is the most effective coalition on CEL LES in most sessions. It is defined by negative ties: 21.18 negative per positive, against 2.68 in the main blocs. All of this is descriptive, with no controls.

**Corroborating that negative ties carry information.**
- [[ringe-2013-pinpointing-powerful-voting]] (Ringe, Victor and Gross 2013; EP Environment Committee, staff survey 2007 with 32 of 65 offices responding; 40 roll calls 2004-2008). Among opposing-bloc dyads, each extra unit of connectivity goes with about 10% lower odds of co-voting (95% CI 7-13%, n = 1,319 non-unanimous dyads). Contact with an adversary is a cue for disagreement, not a conduit for agreement.
- [[ringe-nd-pinpointing-powerful-voting]] argues that negative co-voting correlations carry cue-provider information. That bears against the June vote backbone, which dropped its negative side.

**Cautions from link-prediction work** (online trust platforms, not legislatures).
- [[leskovec-2010-predicting-positive-negative]] (Epinions, Slashdot, Wikipedia). Local balance and status predict edge signs well, at about 93-94% on balanced high-embeddedness samples. But global two-faction partitions score below degree-preserving rewired nulls, so no significant faction structure is found. Any partition claim needs a rewired or sign-permuted null.
- [[tang-2015-negative-link-prediction]] (Epinions and Slashdot). A negative interaction is a real but low-precision proxy for a negative link: precision 0.2097 and 0.1483 for the negIn rule.

**Pseudoconsensus.** [[cao-2025-social-network-group]] warns that agreement inside a subgroup is not agreement across subgroups. The bearing is UNVERIFIED and concerns the all-pro-CU Ward press blocs.

**System consequence.** signed_coalitions.py cites Aref 2021 but differs from it in five ways:
- it partitions an LLM-valenced mention graph that is 83% positive by edge count;
- it pools House and Senate;
- it uses heuristic local search capped at k = 3;
- it compares only against the one-cluster baseline, with no rewired null;
- it discards direction and has no evidence-count floor on signs.

Its stored artifact (2026-07-19) shows one main bloc plus a 14-member splinter. So the system has neither reproduced nor tested Aref's finding. That finding stays UNVERIFIED until a House-only signed cosponsor-backbone reproduction runs; CEL LES is already on disk. The mention-layer negatives are best read as negative *interaction*, not negative *ties* (Tang).

**Caution.** The SDSM backbone null (Aref) must not be adopted as a fix for degree artifacts in the cosponsor network. That would amount to the per-edge volume normalization measured and REJECTED in this project.

### Inferring ties without tie data: text similarity, link prediction and random walks

Works: [[malaterre-2024-inferring-social-networks]], [[zareie-2020-similarity-based-link]], [[baptista-2022-universal-multilayer-network]], [[ardekani-2026-computational-graph-theoretic]], [[leskovec-2010-predicting-positive-negative]]

Paul's notes describe text-to-network pipelines as a core technique. He names Malaterre (topic models), Ardekani (NER and information extraction) and Kim and Kunisky (ML on lobbying reports), with Malaterre as the "Very High, Foundational" item.

**[[malaterre-2024-inferring-social-networks]]** is the stated source of fec/network/press_hcoi.py. It fits a 25-topic LDA to 16,917 philosophy-of-science articles (1930-2017), builds author profiles in four periods of about 21 years, links authors at Pearson r of 0.6 or more, and runs Louvain. Modularity rises from 0.427 to 0.834 across the periods. The only validation is agreement with the field's known history. The authors say plainly that HCoI ties are text-mediated heuristics for *possible* ties, not social ties.

The system's own results back that caveat: the HCoI layer was null for stance co-movement (49%) and recovered only 4 of 41 cosponsor-isolated members. The paper supports building the layer. It does not support reading HCoIs as coalitions, closeness or influence channels, which is the reading Paul's annotation and the holistic edge admission lean toward.

**[[ardekani-2026-computational-graph-theoretic]]** adds two reporting duties: give the modularity, and run an external alignment check. Ardekani's Louvain communities match NZ committee keywords only weakly (Jaccard 0.10-0.67), and press HCoI reports neither figure. It also found hand-written regex extraction beat an LLM extractor (precision 0.96 vs 0.86 in Table 1), although the text gives a conflicting 93%.

**[[zareie-2020-similarity-based-link]]** shares Malaterre's correlation-of-profiles idea, but applies it to structural neighbourhoods and multiplies it by a direct common-neighbour term. It tops AUC in 8 of 9 small, static, unweighted networks, with margins of about 0.01-0.055 and no variance reported. Its use for Melange is as a candidate dyadic feature for the cosponsor forecaster, whose only pair-level network term is the direct prior-affinity count. Any such score is heuristic similarity under D-025 and is UNVERIFIED until reproduced on the forecaster's temporal split (DE-062).

**[[leskovec-2010-predicting-positive-negative]]** shows signed triads modestly improve positive-link prediction (Epinions 0.5612 to 0.5911, chance 0.50).

**[[baptista-2022-universal-multilayer-network]]** generalizes random walk with restart (seed-personalized PageRank) to any mix of multiplex and bipartite layers. Its evidence is biological and airport networks, with nothing on politics. Its transferable lesson is the per-layer ablation and overlap check described in the construction strand.

**Common thread.** Every inference method here is validated, if at all, on held-out links or face validity within its own domain. None is validated against a political relationship ground truth.

### Money as network structure

Works: [[gaynor-2022-financial-ties-bind]], [[battaglini-2016-influencing-connected-legislators]], [[christopher-2016-mathematical-modeling-analysis]], [[azaiez-2025-hypergraph-analysis-european]]

Paul's allocation interest (where advocacy money should go) meets SNA in four works. All four describe where money or resources sit in a network. None shows that money moves a legislator.

- **[[gaynor-2022-financial-ties-bind]]** (115th House; ERGM, binary ties, each party modelled separately). Same-caucus members give to each other more (Freedom Caucus +1.208, Tea Party +0.686, Progressive +0.381, log-odds). Republican leaders give to 86% of members but only about 30% of Freedom Caucus members. Vulnerable members (Cook PVI "likely", October 2018) are about 70-75% more likely to receive a tie, and announced retirees about 40-45% less likely. This supports Cook PVI as the vulnerability measure donors act on. It does **not** support the system's reciprocity claim that a dollar buys more in a competitive seat (D-104..D-108, NOT validated). It also exposes a structural blind spot: Melange treats co-partisans as one class and has no data on the ideological intraparty caucuses.
- **[[battaglini-2016-influencing-connected-legislators]]** predicts Bonacich-proportional PAC allocation. Its estimated spillover is phi of about 0.08-0.25: 0.209 with committee ties (N = 2,128 member-Congress rows) and 0.084 with alma-mater ties (N = 1,166). The paper deliberately avoids cosponsorship ties because cosponsorship is itself driven by money.
- **[[christopher-2016-mathematical-modeling-analysis]]** maps a dark-money nonprofit and LLC network that FEC data cannot see. Mentat's PAC transfer graph follows FEC-reported committee cash only.
- **[[azaiez-2025-hypergraph-analysis-european]]** links company core position to lobbying FTE and assets in the EC register, association only. Its design needs a register that names both parties to each contact. US LD-2 filings do not, and LDA is an identity source, not an edge or weight source (D-021), so the design cannot be rebuilt from LDA.

### Actor attributes, tie formation and what the ties carry

Works: [[selfhout-2010-emerging-late-adolescent]], [[vanderdoes-2022-strategic-identity-signaling]], [[cao-2025-social-network-group]], [[haythornthwaite-1996-social-network-analysis]], [[ringe-2013-pinpointing-powerful-voting]]

Paul's notes ask how an individual's text-derived traits (Birkenmaier) become a graph node, and his path-finding notes ask how personality should be parameterized on edges. This strand gives only indirect answers. Its bearings are mostly UNVERIFIED (DE-062).

- **[[selfhout-2010-emerging-late-adolescent]]** (205 Dutch freshmen in 10 groups, five monthly waves, SIENA actor-oriented models with meta-analytic pooling). Trait *similarity* drives friendship selection: openness, extraversion and agreeableness similarity at 1.45-1.55, significant in 7, 8 and 10 of the 10 groups. Trait *levels* have small effects: extraversion ego .10, agreeableness alter .14. Reciprocity (1.73) and closure terms are large, so pair-only tests misfit. This suggests personality belongs on dyads (similarity) more than on nodes. It qualifies the receptivity layer, where every input describes one member, and the persona-homophily null. Transfer is weak: these are strangers forming friendships, not legislators.
- **[[vanderdoes-2022-strategic-identity-signaling]]** (van der Does et al. 2022; 2020 tweets; randomized experiment, n = 481). Senders switch to covert signals as the outgroup share of their audience rises (AME 0.73 from 1 to 9 outgroup members of 10). Automated political-speech classifiers probably catch only overt speech. For the system, members facing mixed audiences may be understated by the press pillar and the DEBATE stance reader. The paper says nothing about edges, weights or reciprocity.
- **[[cao-2025-social-network-group]]** is a taxonomy of trust propagation and consensus with no evidence. It names multiplicative per-hop decay as systematically unstudied. That describes the uncalibrated 0.8-per-hop factor and 5-year half-life in the staffer router and the single-most-reliable-path recruitment chain. It validates no constant.
- **[[haythornthwaite-1996-social-network-analysis]]** separates sources that inform from close ties that legitimate adoption (after Coleman, Katz and Menzel). This is a loose conceptual parallel to the engine's two-factor kernel.
- **[[ringe-2013-pinpointing-powerful-voting]]** completes the point: what a tie carries depends on its sign and on bloc membership. Pooling ties without sign misreads it.

## Agreements and debates

**1. Is cosponsorship a social tie, a money-driven artifact, or an outcome signal?**
- [[sotoudeh-2024-network-based-measure]], [[aref-2020-detecting-coalitions-optimally]] and [[aref-2021-identifying-hidden-coalitions]] treat cosponsorship as the substrate.
- [[battaglini-2016-influencing-connected-legislators]] deliberately avoids it as endogenous to money (fn. 30).
- [[ringe-2013-pinpointing-powerful-voting]] questions it as a social-tie proxy (p. 604) but does not test it.

This is a premise dispute, not a contradiction: no work tests cosponsor ties against an independent tie measure. **FLAG:** neither Battaglini nor Ringe et al. licenses dropping regular cosponsorship edges. That proposal was measured and REJECTED in this project and must not be revived on their authority.

**2. Does centrality measure influence?**
- Agreement: [[sotoudeh-2024-network-based-measure]], [[battaglini-2016-influencing-connected-legislators]] and [[azaiez-2025-hypergraph-analysis-european]] all find generic centralities weak or resource-driven, each on its own outcome and window.
- Disagreement on the remedy: Sotoudeh builds an outcome-derived pairwise score; Battaglini derives Bonacich from theory; [[ringe-nd-pinpointing-powerful-voting]] argues for a sign-blind measure; [[christopher-2016-mathematical-modeling-analysis]] hand-weights a composite and never validates it.
- [[ardekani-2026-computational-graph-theoretic]] adds that the answer moves with where the graph is cut.

**3. Sign: keep it, drop it, or take its absolute value?**
- [[aref-2021-identifying-hidden-coalitions]] and [[ringe-2013-pinpointing-powerful-voting]] find negative ties substantively informative.
- [[ringe-nd-pinpointing-powerful-voting]] uses the absolute value, deliberately blind to sign, for leadership, while keeping alignment separate. This fits the system's two-axis rule.
- [[leskovec-2010-predicting-positive-negative]] finds local balance strong but global factions absent against rewired nulls. That is in tension with Aref's partition results, which have no rewired null.
- [[tang-2015-negative-link-prediction]] finds observed negative interactions are poor proxies for negative ties.

Resolution for the system: sign matters, but signs built from interactions need evidence floors and nulls before coalition claims.

**4. Projection versus higher-order representation.**
- [[porter-2005-network-analysis-committees]] and [[battaglini-2016-influencing-connected-legislators]] project co-membership onto pairs.
- [[azaiez-2025-hypergraph-analysis-european]] shows projection changes core rankings.

Porter's fix is a per-edge chance normalization. **FLAG:** per-edge volume normalization of cosponsor edges was measured and REJECTED. Do not carry Porter's interlock, Aref's SDSM null, or Sotoudeh's pass/total ratio onto cosponsor edges as a normalization fix. The 1/sqrt(C) bill-size discount already in place is a different, retained device.

**5. Direction.**
- [[fink-2023-congressional-social-network-dataset]], [[haythornthwaite-1996-social-network-analysis]] and [[leskovec-2010-predicting-positive-negative]] (status theory) all treat direction as carrying information.
- The built layers and cascade kernel symmetrize.

No work in the theme tests whether direction improves prediction in Congress.

**6. Text-inferred ties as relationships.** [[malaterre-2024-inferring-social-networks]] itself says HCoIs are heuristics. Paul's annotation reads them as communities and coalitions. The system nulls side with the authors.

**7. Traits on nodes or on dyads.** [[selfhout-2010-emerging-late-adolescent]] finds similarity effects much larger than level effects. Paul's open question frames trait-to-*node* conversion. The evidence suggests dyadic similarity may be the better carrier, but the bearing is UNVERIFIED and comes from a different population.

## Methods

**Dominant methods**

1. **Descriptive construction plus centrality or community detection.** Used by [[porter-2005-network-analysis-committees]], [[sotoudeh-2024-network-based-measure]], [[ardekani-2026-computational-graph-theoretic]], [[malaterre-2024-inferring-social-networks]], [[azaiez-2025-hypergraph-analysis-european]], [[christopher-2016-mathematical-modeling-analysis]] and [[fink-2023-congressional-social-network-dataset]]. Louvain (or Louvain-type) recurs in Malaterre, Ardekani and Azaiez, matching Paul's "Synthesis 1".
   - Weaknesses: few significance tests or null models (Porter, Ardekani, Sotoudeh and Malaterre report none; Azaiez is the exception, with configuration nulls); unreported tuning (Ardekani gives no Katz alpha, Louvain resolution or seed); single snapshots (Fink); validation by face agreement only (Malaterre); leakage (Sotoudeh lets a bill count toward its own score and lets later passage enter edges).
2. **Exact combinatorial optimization on signed graphs.** [[aref-2020-detecting-coalitions-optimally]] and [[aref-2021-identifying-hidden-coalitions]] (binary LPs in Gurobi on SDSM backbones).
   - Strength: globally optimal partitions, not heuristics.
   - Weaknesses: descriptive coalition-effectiveness comparisons with no controls for party, majority or committee; no rewired null for partition significance, which [[leskovec-2010-predicting-positive-negative]] shows matters.
3. **Statistical network models that handle dependence.**
   - ERGM: [[gaynor-2022-financial-ties-bind]]. Binary ties only, weights ignored, and text and tables disagree in places.
   - Stochastic actor-oriented models: [[selfhout-2010-emerging-late-adolescent]].
   - Spatial autoregression: [[battaglini-2016-influencing-connected-legislators]].
   - Crossed-random-effects dyadic models: [[ringe-2013-pinpointing-powerful-voting]]. One committee, cross-sectional, results sensitive to specification.

   These are the only works that model network dependence in the dyads. None of the system's dyad-level checks does.
4. **Supervised and similarity-based link prediction.** [[leskovec-2010-predicting-positive-negative]], [[tang-2015-negative-link-prediction]], [[zareie-2020-similarity-based-link]], and [[baptista-2022-universal-multilayer-network]] (LOOCV and link prediction for RWR).
   - Weaknesses: online-platform or biological domains; random edge holdout rather than temporal holdout (Zareie); AUC with no variance (Zareie); low negative-class precision (Tang, best F1 0.3242 on Epinions).
5. **Simulation and formal proof.** [[ringe-nd-pinpointing-powerful-voting]] and the theory half of [[battaglini-2016-influencing-connected-legislators]]. Ringe and Wilson flag high parameter dependence.
6. **Experiments.** [[vanderdoes-2022-strategic-identity-signaling]], randomized audience composition, the only randomized design in the theme. Its subjects are ordinary Twitter users, not elites.
7. **Narrative and taxonomic reviews.** [[haythornthwaite-1996-social-network-analysis]] and [[cao-2025-social-network-group]]. Vocabulary only; nothing estimated.

**Cross-cutting weaknesses**
- No work in the theme has a causal identification strategy.
- Pooled estimates are rarely stratified. [[gaynor-2022-financial-ties-bind]] splits by party and [[ringe-2013-pinpointing-powerful-voting]] conditions on bloc; most others pool.
- External validity to US congressional influence is limited. Only Porter, Sotoudeh, Aref (2020, 2021), Gaynor, Battaglini and Fink use US Congress data, and of those only Sotoudeh, Aref 2020 and Battaglini link network structure to an outcome, all associationally.
- Any figure carried from these works into the system must keep the work's window, chamber and denominator, and be labelled UNSTRATIFIED when pooled across a known fault line (chamber, party).

## How the works relate

- [[aref-2021-identifying-hidden-coalitions]] — builds-on — [[aref-2020-detecting-coalitions-optimally]]: Same authors; the 2021 paper extends the exact 2-group frustration partition of signed SDSM cosponsorship backbones to k groups and the minimal-k clusterability index.
- [[aref-2021-identifying-hidden-coalitions]] — same-data — [[aref-2020-detecting-coalitions-optimally]]: Both build signed SDSM backbones from congressional bill cosponsorship over overlapping periods (1979-2016 and 1981-2017).
- [[tang-2015-negative-link-prediction]] — builds-on — [[leskovec-2010-predicting-positive-negative]]: Tang uses Leskovec et al.'s 16 signed-triad features in its 45-feature set and works on the same Epinions and Slashdot platforms.
- [[sotoudeh-2024-network-based-measure]] — cites (shared author) — [[porter-2005-network-analysis-committees]]: Sotoudeh cites Porter et al. 2005 (ref 19), Porter is second author of Sotoudeh, and committee assignments are named as Sotoudeh's missing control.
- [[gaynor-2022-financial-ties-bind]] — cites — [[porter-2005-network-analysis-committees]]: Gaynor cites Porter et al. 2005 (p.893) as prior House network analysis, and builds from LPAC money where Porter used committee co-membership.
- [[aref-2020-detecting-coalitions-optimally]] — cites — [[porter-2005-network-analysis-committees]]: Aref and Neal 2020 cite Porter et al. (ref 47) as a committee-network substrate they set aside in favour of cosponsorship.
- [[malaterre-2024-inferring-social-networks]] — same-method (Louvain) — [[ardekani-2026-computational-graph-theoretic]]: Paul's own cross-reference: both use Louvain community detection. Malaterre builds ties from text similarity; Ardekani from a relational citation graph.
- [[zareie-2020-similarity-based-link]] — shared technique — [[malaterre-2024-inferring-social-networks]]: Both infer ties from Pearson correlation of profiles: structural neighbourhood vectors in Zareie, LDA topic profiles in Malaterre. Zareie adds a direct common-neighbour term.
- [[baptista-2022-universal-multilayer-network]] — same-method (evaluation) — [[zareie-2020-similarity-based-link]]: Both score link prediction by holding out existing edges. Baptista uses it to evaluate RWR; Zareie to evaluate the DICN similarity score.
- [[azaiez-2025-hypergraph-analysis-european]] — qualifies — [[porter-2005-network-analysis-committees]]: Porter projects member-by-committee co-membership onto pairs; Azaiez shows clique projection of group events changes core rankings relative to a hypergraph treatment.
- [[azaiez-2025-hypergraph-analysis-european]] — agrees — [[sotoudeh-2024-network-based-measure]]: Both find generic centrality on projected graphs is a poor stand-in for influence. Sotoudeh: centralities barely separate passed and failed bills. Azaiez: centrality reflects embeddedness driven by resources.
- [[battaglini-2016-influencing-connected-legislators]] — premise dispute / partial agreement — [[sotoudeh-2024-network-based-measure]]: Battaglini avoids cosponsorship ties as endogenous to money, while Sotoudeh builds on them. Both find degree, closeness, betweenness-type centralities add little to their outcome (PAC money; bill passage).
- [[ringe-2013-pinpointing-powerful-voting]] — premise dispute — [[sotoudeh-2024-network-based-measure]]: Ringe, Victor and Gross question cosponsorship as a social-tie proxy (p. 604) without testing Sotoudeh's measure; not a contradiction.
- [[ardekani-2026-computational-graph-theoretic]] — contrast (different quantity) — [[battaglini-2016-influencing-connected-legislators]]: Ardekani uses in-direction Katz prestige on statutes; Battaglini derives out-direction Katz-Bonacich for legislators. kb_benchmark.py implements the latter and they must not be conflated.
- [[leskovec-2010-predicting-positive-negative]] — tension / qualifies — [[aref-2021-identifying-hidden-coalitions]]: Leskovec finds global faction partitions score below degree-preserving rewired nulls, so no significant faction structure; Aref reports coalition partitions with no rewired null. Any partition claim needs that null.
- [[tang-2015-negative-link-prediction]] — qualifies — [[aref-2021-identifying-hidden-coalitions]]: Aref's third coalition is defined by negative ties; Tang shows negative interactions are low-precision proxies for negative ties, which bears on the system's LLM-valenced mention signs used with the Aref method.
- [[ringe-2013-pinpointing-powerful-voting]] — agrees — [[aref-2021-identifying-hidden-coalitions]]: Both find structure below party level carried by adversarial ties: bloc-conditioned adversary contact lowers co-voting; negative cosponsor ties define the third coalition.
- [[ringe-nd-pinpointing-powerful-voting]] — same research programme — [[ringe-2013-pinpointing-powerful-voting]]: Same first author and European Parliament setting; both treat colleagues as cue sources and keep the sign of voting relations analytically central.
- [[ringe-nd-pinpointing-powerful-voting]] — complements — [[aref-2020-detecting-coalitions-optimally]]: Both treat signed ties as informative. Ringe and Wilson score nodes from signed co-voting correlations; Aref and Neal partition signed cosponsorship networks into coalitions.
- [[ringe-nd-pinpointing-powerful-voting]] — complements — [[sotoudeh-2024-network-based-measure]]: Together they bound what centrality shows: eigenvector centrality is weak for bill passage (Sotoudeh); co-voting centrality is validated only against simulated ground truth (Ringe and Wilson).
- [[christopher-2016-mathematical-modeling-analysis]] — contrast — [[sotoudeh-2024-network-based-measure]]: Couch et al. hand-weight eight centralities with no outcome check; Sotoudeh tests centralities against bill passage and finds them weak.
- [[christopher-2016-mathematical-modeling-analysis]] — contrast — [[ardekani-2026-computational-graph-theoretic]]: Both face the choice of centrality. Ardekani picks Katz on substantive grounds; Couch et al. weight eight measures by expert judgment.
- [[christopher-2016-mathematical-modeling-analysis]] — complements — [[gaynor-2022-financial-ties-bind]]: Both build directed money-flow networks among political organizations: dark-money nonprofits in Couch et al., member Leadership PACs in Gaynor.
- [[gaynor-2022-financial-ties-bind]] — complements — [[battaglini-2016-influencing-connected-legislators]]: Both model House money allocation on networks: member-to-member LPAC ties shaped by caucus (Gaynor) versus PAC money proportional to Bonacich centrality over committee and alma-mater ties (Battaglini).
- [[porter-2005-network-analysis-committees]] — same-structure (co-membership) — [[gaynor-2022-financial-ties-bind]]: Committee co-membership (Porter) and intraparty caucus co-membership (Gaynor) are both affiliation relations among House members.
- [[porter-2005-network-analysis-committees]] — same-data, different object — [[battaglini-2016-influencing-connected-legislators]]: Both use shared House committee membership. Porter's nodes are committees; Battaglini builds member-level ties.
- [[cao-2025-social-network-group]] — complements — [[aref-2021-identifying-hidden-coalitions]]: Cao's pseudoconsensus warning (within-group agreement is not cross-group agreement) is addressed by Aref's signed partitioning between subgroups.
- [[haythornthwaite-1996-social-network-analysis]] — extended-by — [[aref-2021-identifying-hidden-coalitions]]: Haythornthwaite's content, direction and strength triad has no sign dimension; signed-network partitioning adds it.
- [[haythornthwaite-1996-social-network-analysis]] — complements — [[sotoudeh-2024-network-based-measure]]: Haythornthwaite names degree, closeness and betweenness without testing them; Sotoudeh tests cosponsorship centralities against bill passage.
- [[selfhout-2010-emerging-late-adolescent]] — complements — [[haythornthwaite-1996-social-network-analysis]]: Haythornthwaite gives the directed, tie-strength framing; Selfhout estimates reciprocity and transitivity in a longitudinal directed-tie model rather than assuming them.
- [[baptista-2022-universal-multilayer-network]] — complements — [[ardekani-2026-computational-graph-theoretic]]: Baptista's RWR is personalized PageRank and Ardekani uses Katz prestige, so together they frame the PageRank-versus-Katz choice for walk-based ranking.
- [[vanderdoes-2022-strategic-identity-signaling]] — complements — [[fink-2023-congressional-social-network-dataset]]: Same medium (Twitter). Fink builds congressional interaction ties; van der Does et al. imply the content on such ties is audience-shaped and more covert with heterogeneous followings.
- [[leskovec-2010-predicting-positive-negative]] — complements — [[zareie-2020-similarity-based-link]]: Leskovec shows signed triads improve positive-link existence prediction; Zareie's unsigned similarity leaves the negative-tie question open.

## What it means for the systems

Statuses follow the bearing tags in the file. VERIFIED and VERIFIED-SOFTENED bearings are checked readings. UNVERIFIED bearings are single-agent leads, not findings (DE-062). Graph ranks and shared-background scores are heuristic model outputs, never closeness, access or introduction probabilities (D-025).

**Cosponsor network ([[systems/melange-sna]], [[systems/melange-influence-engine]])**
- [[sotoudeh-2024-network-based-measure]] is cited as the source of the CU cosponsor edge weights, but the built weights depart from it on nearly every parameter: an additive x3 success multiplier, a per-Congress 0.8 decay (about a 6-year half-life), the 1/sqrt(C) size discount, unequal role weights, and no party normalization. The citation should say "inspired by", not "implements".
- Code comments and brain notes also cite Sotoudeh for recruitment chains and for influence versus cosponsor count. The paper tests neither.
- Paul's 6-month half-life note is a mis-transfer: Sotoudeh's decay applies to accruing cosponsorship influence, not to stance salience.
- **FLAG (rejected proposals):** [[porter-2005-network-analysis-committees]] (normalized interlock), [[aref-2020-detecting-coalitions-optimally]] (SDSM backbone null) and Sotoudeh's pass/total ratio could each be read as arguments for per-edge volume normalization of cosponsor edges. That proposal was measured and REJECTED.
- **FLAG:** [[battaglini-2016-influencing-connected-legislators]] and [[ringe-2013-pinpointing-powerful-voting]] question cosponsorship as a tie. That is not a case for dropping regular cosponsorship edges, which was also measured and REJECTED.
- [[azaiez-2025-hypergraph-analysis-european]] supports the existing 1/sqrt(C) big-bill discount.

**InfluenceScore and the rank pillars ([[systems/moneyball-rank-pillars]])**
- The June 2026 retirement of direction-blind cosponsor centrality from the Influence pillar is supported by [[sotoudeh-2024-network-based-measure]], [[battaglini-2016-influencing-connected-legislators]] (money outcome only), [[azaiez-2025-hypergraph-analysis-european]] and [[ringe-nd-pinpointing-powerful-voting]].
- It is qualified by [[christopher-2016-mathematical-modeling-analysis]], whose unvalidated hand-weighted composite has the same shape, and by [[haythornthwaite-1996-social-network-analysis]].
- Any check of InfluenceScore against bill passage is circular, because the edges are already passage-weighted.
- Committee pillar: [[porter-2005-network-analysis-committees]] shows subcommittee membership largely nests inside the parent committee, so the added one-sixth-weight subcommittee seats partly double count.
- Say-do gate: Porter's roll-call SVD puts most variance on a bipartisan axis (45.3% of the sum of squares, 107th House, 990 votes), so a single final-passage Yea is weak corroboration.

**Signed coalitions, diagnostic ([[systems/melange-sna]], [[systems/melange-validation]])**
- signed_coalitions.py cites [[aref-2021-identifying-hidden-coalitions]] but partitions a different graph: an LLM-valenced mention layer that is 83% positive, pools both chambers, uses heuristic local search capped at k = 3, has no rewired null ([[leskovec-2010-predicting-positive-negative]]) and has no evidence floor on signs ([[tang-2015-negative-link-prediction]]).
- Aref's third-coalition result is therefore UNVERIFIED in this system.
- A House-only reproduction on a signed cosponsor SDSM backbone is cheap, since the CEL LES data is on disk. At 520 members and 7,153 edges, the exact program of [[aref-2020-detecting-coalitions-optimally]] looks tractable; that estimate is derived and UNVERIFIED.
- [[ringe-nd-pinpointing-powerful-voting]] argues against having dropped the negative side of the vote backbone.

**Press HCoI and the multilayer social/press edges ([[systems/melange-sna]], [[systems/melange-stance]])**
- [[malaterre-2024-inferring-social-networks]] (Paul's Very High, Foundational item) is the method source. The system's nulls (49% stance co-movement; 4 of 41 isolates recovered) match the authors' caveat that HCoI ties are text-mediated heuristics. Do not read HCoIs as coalitions, closeness or influence channels.
- [[ardekani-2026-computational-graph-theoretic]]: report modularity and an external alignment check, which press HCoI currently omits.
- [[fink-2023-congressional-social-network-dataset]] is the method source for press_engagement_layer.py. Its bearing on social_interaction_layer.py is tagged *contradicts*: that layer divides by the wrong end of the dyad, uses a 25-post floor, weights mentions 0.4, drops quotes and replies, and symmetrizes. The reading is code-only and UNVERIFIED.
- Fink calls its weights a "probability of influence". In this system they are heuristic transmission weights (D-025), not measured influence.
- [[baptista-2022-universal-multilayer-network]]: before treating more layers as more signal, test each layer's overlap with the base graph and run a held-out ablation per layer.

**Cascade kernel and the influence engine ([[systems/melange-influence-engine]], [[systems/melange-allocation]], [[systems/melange-pathfinding]])**
- [[ringe-2013-pinpointing-powerful-voting]] qualifies any reading of cross-party ties as positive conduits for alignment. Opposing-bloc contact goes with less co-voting.
- [[gaynor-2022-financial-ties-bind]] supports Cook PVI as the vulnerability measure donors act on (competitiveness multiplier). It does **not** validate reciprocity: reciprocity is NOT validated (D-104..D-108). It also exposes the missing intraparty-caucus structure.
- kb_benchmark.py cites [[battaglini-2016-influencing-connected-legislators]] but deviates from it in three ways: it runs on the cosponsor kernel the paper avoids, pools Senate with House, and sets attenuation at 0.85 of the bound against the paper's phi of about 0.08-0.25.
- Its stored results are Spearman rho 0.135 against CU money and 0.230 against bank plus financial-services money (n = 533 members, both chambers, file written 2026-08-18). They are UNSTRATIFIED by chamber, unverified and stale. Diagnostic only.

**Staffer network ([[systems/melange-staffer-network]])**
- [[azaiez-2025-hypergraph-analysis-european]] is direct evidence for the open owner ruling on container-size normalization of co-tenure edges. That ruling is Paul's to make; this synthesis does not recommend an outcome. The paper also helps explain the r = 0.87 between degree and office size.
- [[baptista-2022-universal-multilayer-network]]: the layer-balanced PageRank has been checked for sensitivity on damping only, not on how mass is split across layers.
- [[cao-2025-social-network-group]] (UNVERIFIED): the 0.8-per-hop and 5-year decay constants are uncalibrated.
- Revolving-door edges carry no influence language.

**Mentat ([[systems/mentat-fec-attribution]], [[systems/melange-lobbying]])**
- [[gaynor-2022-financial-ties-bind]]: the LPAC member-to-member network can be rebuilt from FEC sponsor links Mentat already holds. It would be a donation-tie layer, not closeness.
- [[christopher-2016-mathematical-modeling-analysis]]: the PAC transfer graph cannot see dark-money nonprofit flows.
- [[azaiez-2025-hypergraph-analysis-european]]'s meeting design cannot be rebuilt from LD-2, because LDA is an identity source, not an edge or weight source (D-021).

**Forecaster, stance and psychometric layers ([[systems/mentat-giving-forecast]], [[systems/melange-stance]], [[systems/melange-psychometrics]])**
- Pair-level features are missing from the cosponsor forecaster: common-neighbour and second-order similarity ([[zareie-2020-similarity-based-link]]), signed triads ([[leskovec-2010-predicting-positive-negative]]), and reciprocity and closure ([[selfhout-2010-emerging-late-adolescent]], UNVERIFIED). All are leads that need a temporal-split reproduction.
- [[vanderdoes-2022-strategic-identity-signaling]] (UNVERIFIED): the press pillar and DEBATE reader may understate members who code their messages for mixed audiences.

**Terrier** ([[systems/terrier-row-scoring]], [[systems/terrier-imputation]], [[systems/terrier-canvass]]) is not addressed by any work in this theme.

## Gaps

**Gaps Paul named that the theme only partly closes**
- *Bipartite-to-graph conversion for path-finding* (Kim and Kunisky; Mason, Porter and Mucha). [[porter-2005-network-analysis-committees]], [[azaiez-2025-hypergraph-analysis-european]] and [[baptista-2022-universal-multilayer-network]] show the trade-offs: projection distorts, and walks on heterogeneous layers need overlap. No work gives a path-finding-ready conversion for lobbying-to-legislator data. Kim and Kunisky are not in this theme file.
- *Temporal weighting.* Only [[sotoudeh-2024-network-based-measure]] (exponential decay), [[malaterre-2024-inferring-social-networks]] (period slicing) and [[selfhout-2010-emerging-late-adolescent]] (panel waves) address time. Nothing tests tie-decay constants for legislators or staffers. The system's decay constants (0.8 per Congress; 0.8 per hop; 5-year half-life) have no literature calibration.
- *Edge sign for replies and mentions.* [[tang-2015-negative-link-prediction]] warns that interaction-derived signs are low-precision. No legislative ground truth for negative ties exists in the theme.
- *Which centrality.* The theme says generic centralities are weak. It offers no validated alternative for US congressional influence beyond Sotoudeh's descriptive, leakage-prone, outcome-built score.
- *Viral Centrality.* Fink asserts it approximates ICM Monte Carlo. The proof sits in a companion paper that is not in the theme, and it remains untested here.

**Missing evidence types**
- No causal or quasi-experimental study links network position to legislative behaviour or outcomes.
- No temporal-holdout validation of any network feature on US Congress data.
- No directed-versus-symmetrized comparison for congressional ties.
- No null-model practice for the system's own diagnostics: signed coalitions and HCoI both lack rewired or permuted nulls.
- No reproduction of Aref's third coalition within the system. It stays UNVERIFIED until the House-only signed-backbone run.
- No literature on staffer or revolving-door networks, despite the staffer network being a built component.
- No work on lobbyist-to-member ties in the US. LD-2 does not name members, and LDA stays an identity source (D-021).
- Nothing supports the reciprocity claim (D-104..D-108).
- Nothing on multi-path trust aggregation versus single most-reliable path. [[cao-2025-social-network-group]] only names the gap.
- No intraparty-caucus membership data for the main ideological caucuses, which [[gaynor-2022-financial-ties-bind]] shows structure money flows.
- No inferential test of the projection-inflation effect on cosponsorship graphs. A hypergraph treatment of bill rosters is absent.

**Coverage and metadata gaps in the vault**
- Paul's SNA bibliography lists Battaglini and Patacchini, Bonica, Valujeva et al. and Zhang et al. as bare citations. [[battaglini-2016-influencing-connected-legislators]] is now summarized here, but Paul's own takeaway on it is missing.
- Works named in Paul's notes but absent from this theme file: Kim and Kunisky, Bacik et al., Huneeus and Kim, Furnas et al., Kim (Jiseon) et al., Oropeza-Martinez et al., Rongkuan et al., and Birkenmaier and Lechner. They cannot be related here.
- The slug [[ringe-2013-pinpointing-powerful-voting]] has empty title and year. It should be re-keyed to Ringe, Victor and Gross (BJPS 2013).
- [[vanderdoes-2022-strategic-identity-signaling]] should carry van der Does et al. (PNAS 2022).
- [[haythornthwaite-1996-social-network-analysis]]: the printed header says 1998.
- Citation discrepancies Paul's notes record: "Kuinsky" vs "Kunisky"; Porter's author count (three in the citation, four in the annotation).
- Paul's Neural Networks document holds four bare links with no annotations, so any SNA-relevant thinking in it is not captured.

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
- [[ringe-2013-pinpointing-powerful-voting]] (n.d.) — (untitled; Ringe, Victor and Gross, BJPS 2013)
- [[christopher-2016-mathematical-modeling-analysis]] (2016) — Mathematical modeling and analysis of a dark money network
- [[selfhout-2010-emerging-late-adolescent]] (2010) — Emerging Late Adolescent Friendship Networks and Big Five Personality Traits: A Social Network Approach
- [[ringe-nd-pinpointing-powerful-voting]] (n.d.) — Pinpointing the Powerful: Co-voting Network Centrality as a Measure of Political Influence (online appendix)
- [[aref-2020-detecting-coalitions-optimally]] (2020) — Detecting coalitions by optimally partitioning signed networks of political collaboration
- [[vanderdoes-2022-strategic-identity-signaling]] (n.d.) — Strategic identity signaling in heterogeneous networks
- [[fink-2023-congressional-social-network-dataset]] (2023) — A Congressional Twitter network dataset quantifying pairwise probability of influence
