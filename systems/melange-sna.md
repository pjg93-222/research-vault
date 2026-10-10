---
type: system
name: "Melange member social-network analysis"
slug: melange-sna
components:
  - melange-sna.cosponsor-network
  - melange-sna.bipartisanship-bpf
  - melange-sna.multilayer-social-press-edges
  - melange-sna.press-hcoi-communities
  - melange-sna.co-appearance-and-references
  - melange-sna.signed-coalitions
  - melange-sna.press-coalition-ward
  - melange-sna.sector-networks
  - melange-sna.katz-bonacich-benchmark
brain_notes:
  - C:/Users/pgoff/Documents/brain/topics/2026-06-feature-cosponsor-and-sector-networks.md
  - C:/Users/pgoff/Documents/brain/topics/2026-06-feature-cosponsor-and-sector-networks-june-network-plan.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-influence-engine-internals-salience-sna-wave.md
  - C:/Users/pgoff/Documents/brain/actors/melange-dashboard-june-capstone.md
paper_count: 31
tags: [system]
---

# Melange member social-network analysis

Construction and measurement of member-to-member networks: the cosponsorship network (R Stage 3 and scraper weights), multilayer layers (cosponsor, press engagement, HCoI, social reposts and mentions), centrality and bipartisanship, co-appearance and signed reference layers, signed coalition partitioning, press-cluster coalitions, sector network configs and the Katz-Bonacich benchmark.

Brain notes: [topics/2026-06-feature-cosponsor-and-sector-networks](obsidian://open?vault=brain&file=topics%2F2026-06-feature-cosponsor-and-sector-networks); [topics/2026-06-feature-cosponsor-and-sector-networks-june-network-plan](obsidian://open?vault=brain&file=topics%2F2026-06-feature-cosponsor-and-sector-networks-june-network-plan); [topics/2026-07-feature-influence-engine-internals-salience-sna-wave](obsidian://open?vault=brain&file=topics%2F2026-07-feature-influence-engine-internals-salience-sna-wave); [actors/melange-dashboard-june-capstone](obsidian://open?vault=brain&file=actors%2Fmelange-dashboard-june-capstone)

## Melange member SNA: evidence assessment

**What was weighed.** There are 53 evidence rows across 9 components: 8 VERIFIED, 42 VERIFIED-SOFTENED and 3 UNVERIFIED. The 3 UNVERIFIED rows (Steinert-Threlkeld 2018, Zhang 2022, Cao 2025) are leads only (DE-062). They do not count toward any verdict below.

**Bottom line.** No component's main empirical question is answered by the evidence. Every component lists a predictive question, such as "do X ties predict future cosponsorship or co-voting?", and no paper in the hub tests one on US congressional data under conditions that transfer. Most of the strong rows **qualify** the system rather than support it. Each cited method source is used looser than the paper, or on a different substrate:

- **Sotoudeh.** The paper scores pairs by a pass/total ratio. The system applies a volume-weighted x3 success boost.
- **Fink.** The social layer divides by the volume of the member who engages, which is the opposite end of the dyad from Fink. This is a CONTRADICTION.
- **Malaterre.** The paper describes attention similarity, not alignment and not social ties.
- **Aref & Neal.** The paper uses House-only, per-Congress SDSM cosponsorship backbones. The system runs on a pooled, two-chamber, LLM-valenced mention layer, using local search without the paper's optimality guarantee.
- **Battaglini & Patacchini.** The paper deliberately does not use cosponsorship as its network, because cosponsorship is determined together with contributions.

**What holds up.** The mechanics transfer: Malaterre's pipeline, the Aref frustration objective, Katz-Bonacich as computed, and the idea of weighting ties by bill size and joint passage. Paul's own cautions match the evidence:
- Baptista: more layers are not always better.
- Sign matters: Jennifer and Tang (negative interactions mark negative relationships far above chance).
- The press layer was null on homophily: Malaterre (text ties are heuristic only).

**Where Paul's positions outrun the evidence:**
- The cosponsor graph is cited to Sotoudeh, but Sotoudeh is undirected and treats sponsor and cosponsor alike. It does not support direction or the specific weight constants.
- Reading HCoI as "office closeness" goes past Malaterre, and D-025 bars closeness language.
- Aref's exact method is not computationally cheap: 3.3 to 14 h per House session, 48,823 s for the 113th House. Only the local-search approximation is cheap, and it carries no guarantee.
- Ardekani's Katz counts incoming paths (prestige) on an Act-citation graph. It does not validate an out-walk Katz-Bonacich (KB) score on a legislator kernel.

**Flags.** Several papers argue for something close to the REJECTED per-edge volume normalization:
- the SDSM degree-preserving null (Aref 2020, 2021)
- Porter's observed/expected weighting
- Tang's count-based reliability weight
- Ahmad's normalization by the retweeter's volume

Each is FLAGGED, not recommended. No paper in the hub argues for dropping regular cosponsorship edges.

**Housekeeping.** Several component descriptions are stale against code or artifacts:
- HCoI: 8 vs 6 communities, 19 vs 20 categories, 385 vs 155 members profiled.
- Co-appearance: 1,151 vs 2,481 pairs.
- Reference edges: 4,358 vs 7,153 vs 7,944.
- Ward: 9 blocs vs at most 8, and the "all pro-CU" mislabel is not supported by the code.
- Regular-cosponsor weight: 0.5 vs 0.2.
- Two outputs whose docstrings say "not consumed by the chain" reach the member desk: the signed-coalition splinter flag and the KB percentile.


## Cosponsorship network construction

**What it computes.** Member-to-member weighted cosponsor graph: symmetric (mean of reverse duplicates) for cascades and directed (sponsor -> cosponsor) for recruitment.

**Method.** R Stage 3: original cosponsor 1.0, regular 0.5, summed per sponsor's bills; igraph centralities with 1/weight distances. Scraper weights: role weight x SUCCESS x 0.8^(119 - congress) / sqrt(C), passed-origin x3 (Sotoudeh-Porter-Krishnagopal). Five NET_CONFIGs (cu published, holistic, holistic_norm, bank_reg, crypto).

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/congress_member_scraper.R (Stage 3)`, `fec/moneyball_core.py:371-542 (load_network, load_network_directed)`, `fec/network/build_multilayer_network.py:9-19`

**Assumptions.**
- Cosponsorship is an influence and alignment tie
- Original cosponsorship is a stronger tie than plain cosponsorship
- Big bills are weaker ties (1/sqrt(C))

**Evidence.** 8 rows (8 VERIFIED-SOFTENED), 8 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | supports | Large hyperedges inflate attendees' centrality when projected to cliques, yet those large hyperedges are what connects the network (edge betweenness correlates 0.74 with size). | Supports discounting by hyperedge (bill) size, which is distinct from the REJECTED per-edge volume normalisation (sqrt(Out*In) or row-stochastic, STATE.md:895-896). It gives no basis for 1/sqrt(C) over log or 1/C. A bill roster is an asynchronous set of signatures with sponsor direction, not a simultaneous meeting. The data are EC lobbying meetings, not US House cosponsorship. | weak | VERIFIED-SOFTENED |
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | suggests-extension | Builds a signed network from the legislator-bill bipartite projection using SDSM (backbone R package 1.5.0). The null preserves the expected values of both degree sequences. Positive and negative edges are significant deviations at FWER alpha = 0.01 (Holm-Bonferroni). | A signed SDSM backbone could be added as a separate diagnostic layer feeding signed_coalitions. It should NOT replace or reweight kernel edges. SDSM's degree-preserving null acts like per-edge volume normalization, a proposal that was measured and REJECTED in this project, so FLAG and do not recommend it. The paper uses all co-sponsorships and does not argue for dropping regular cosponsorship edges. | moderate | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | qualifies | Relationships carry three attributes: content, direction and strength. A pair is tied by all the relationships it maintains. Strength can be carried as the value of a link, or considered within a presence-absence dichotomy by requiring a minimum frequency. Reported directed ties can be asymmetric, with the two sides reporting different amounts. The article does not say that thresholding discards strength. | The article gives design principles and no estimate. It does not address 2:1 role weights, the success multiplier, the decay factor or the sqrt size norm, and it does not show that symmetrizing changes diffusion. It is not evidence for keeping or dropping regular-cosponsor edges, so the rejected drop must not be defended from this paper. Asymmetric reports are a survey feature, not a graph-construction rule. | weak | VERIFIED-SOFTENED |
| [[sotoudeh-2024-network-based-measure]] | 2024 | qualifies | Influence accrues per legislator pair as a decayed ratio C_pass / C_tot of mutually cosponsored bills that eventually pass the House. Sponsor and cosponsor are treated identically, edges are undirected, decay is monthly with half-life 6, 12 or 24 months, and scores are normalized by party size (Eq. 1-4). Passage means House passage. The authors call the result only a proxy (p. 17). | The paper supports only the general idea of weighting ties by joint passage, for House bills 2009-2019 with House passage as outcome. It gives no evidence for the x3 multiplier, the 1/sqrt(C) discount, the 2:1 or 5:1 role ratio, the per-Congress decay, Senate ties or CU-topic bill universes. Read the citation as inspired by, not implements. | strong | VERIFIED-SOFTENED |
| [[porter-2005-network-analysis-committees]] | 2005 | suggests-extension | Porter et al. project a two-mode representative-by-committee assignment network onto committees, weighting each committee pair by observed shared members over the count expected under random same-size assignment (1 = chance). The structure result uses assignment data only. The political-correlation result uses roll-call votes, so it is not free of political information. | No committee co-membership layer exists, so any such layer would be new and unvalidated. Its observed/expected weighting must not be applied to cosponsor edges, which was measured and rejected (brain topic note, line 101, per-edge normalization REJECTED). The item's Paul-annotation entries link Porter to a committee layer, not to the cosponsor graph. | weak | VERIFIED-SOFTENED |
| [[ringe-2013-pinpointing-powerful-voting]] |  | qualifies | Authors use measured staff-reported contacts rather than proxies such as cosponsorship, and argue this raises confidence in their measures. Reported contact was sparse: 6.1 percent density (118 of 1,920 possible directed ties in the asymmetric all-respondent subnetwork), mean point connectivity 2.2 (median 2), and 12.4 percent of dyads at zero connectivity. | The paper does not test cosponsorship, so it cannot show cosponsor edges are wrong. It shows measured contact and agreement can diverge for adversaries, which bears on the alignment assumption for cross-party cosponsor edges. It does not argue for dropping regular cosponsor edges or for per-edge volume normalization; both were measured and rejected in this project. | weak | VERIFIED-SOFTENED |
| [[aref-2020-detecting-coalitions-optimally]] | 2020 | qualifies | A tie is positive or negative according to whether joint cosponsorships are significantly many or few against an SDSM null that preserves each legislator's cosponsorship volume and each bill's cosponsor count. Raw co-cosponsorship counts are not used as tie strengths. The authors note inferred ties may reflect avoidance. | SDSM answers whether a tie is significant, not how strong it is. Because it conditions on member volume it works as a degree-corrected filter. The project's rejected per-edge volume normalization is the nearest analogue, so the paper does not argue for re-weighting the cascade graph. Any significance-filtered signed overlay would need its own validation. | weak | VERIFIED-SOFTENED |
| [[kamarthi-2020-influence-maximization-unknown]] | 2019 | qualifies | Geometric-DQN, trained per family and tested on held-out networks of that family, improves influence by 7.91 to 26.6 percent of the CHANGE-to-OPT gap (best variant per family, 13 test networks, IC p = 0.1, budget 10, T = 5 queries). Discovered graphs are almost always larger than CHANGE's, yet size does not correlate with influence across Geometric-DQN policies. | The paper discovery setting applies only if cosponsorship is an incomplete sample of a latent contact network. The paper numbers come from other networks under single-colour IC, so they cannot be carried over as system gains. | weak | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: thin and indirect.** There are 8 rows, all VERIFIED-SOFTENED and none VERIFIED. One is strong (Sotoudeh 2024) and one moderate (Aref 2021). The rest are weak: Aref 2020, Haythornthwaite 1996, Azaiez 2025, Porter 2005, Jennifer (n.d.) and Kamarthi 2019.

**Sotoudeh 2024, the cited weight source, supports only the general idea of weighting a tie by joint passage.**
- Its scope: House bills 2009-2019, House passage as the outcome, and a pair score equal to the decayed ratio of the pair's mutually cosponsored bills that pass to all such bills.
- Its edges are undirected, sponsor and cosponsor are treated the same, and scores are normalized by party size. The authors call the measure a proxy.
- Melange instead sums a directed, volume-weighted score: role weight x 3.0 if passed x 0.8^(119-congress) x 1/sqrt(C).
- The paper gives no support for the x3 multiplier, the 1/sqrt(C) discount, the 2:1 or 5:1 role ratios, per-Congress decay, Senate ties or CU-topic bill universes.
- Read the citation as "inspired by", not "implements".
- A data caveat: 2,652 of 37,076 tie-contributing bills, carrying about 9.6% of ties, depend on the last-action passage regex.

**Bill-size discount.** Azaiez 2025 (weak, EC lobby meetings) supports discounting large hyperedges, which is distinct from the rejected per-edge normalization. It says nothing that favors 1/sqrt(C) over log(C) or 1/C.

**Flags.**
- Aref 2021 and 2020 build signed SDSM backbones. SDSM conditions on each member's cosponsorship volume, which makes it the nearest analogue of the REJECTED per-edge volume normalization. It must not reweight kernel edges. At most it could be a separate diagnostic layer.
- Porter 2005's observed/expected interlock weighting belongs to a committee layer that does not exist. It must not be applied to cosponsor edges, for the same reason.
- No paper argues for dropping regular cosponsorship edges.

**Bearing on the "influence and alignment tie" assumption.**
- Jennifer (EP staff contact data) shows that measured contact and agreement diverge for adversaries. This weakly questions reading cross-party cosponsor edges as alignment.
- Battaglini (filed under katz-bonacich-benchmark) treats cosponsorship as determined together with contributions, which complicates reading it as an influence tie.
- Kamarthi frames the observed graph as possibly an incomplete sample of a latent contact network. The system models no missing ties.

### Contradictions
- The regular-cosponsor weight is stated three ways. The component and R Stage 3 say 0.5. The holistic base builder uses 0.2. The Jennifer row's code read found no 0.5 dial for the cu base graph. The 2:1 ratio in the component's assumptions is therefore not a single fact across configs.
- Sotoudeh, the cited source, is undirected with sponsor and cosponsor treated identically, and it uses a pass/total ratio. The system is directed (sponsor to cosponsor) and uses a volume-summed x3 success boost. The citation does not support either the direction or the functional form.
- Sotoudeh decays monthly (half-life 6, 12 or 24 months). The system decays 0.8 per Congress. The paper offers no basis for the per-Congress constant.

### Gaps
- No held-out test of the weighting. The study needed: predict next-Congress cosponsorship or co-voting from raw counts, role-weighted, success-weighted and Sotoudeh-ratio graphs. Run the House and Senate separately and name the window and denominator. This would answer both empirical questions (success weighting, and the 2:1 role ratio).
- No evidence on the bill-size discount form (1/sqrt(C) vs log vs 1/C). A sensitivity study on the same held-out target would fill it.
- No evidence on directed vs symmetrized edges for recruitment paths on US data.

### Paul's position vs the evidence

Paul wants weighted, directed, time-decayed cosponsor edges, as his sources use cosponsorship. Building on cosponsorship is consistent with the literature: Sotoudeh, Aref and Porter all use cosponsorship or assignment data. The specific choices are not grounded:
- Direction is not in Sotoudeh, which is undirected.
- The decay constant, role ratio, x3 multiplier and size discount have no empirical support in the hub.
- Sotoudeh supports joint-passage weighting only for the House.

Haythornthwaite supports carrying strength and direction as design principles but provides no estimate. The jointly-determined caveat (Battaglini fn 30) and the contact-vs-agreement divergence (Jennifer) both cut against reading these edges as influence or alignment. Graph ranks built on them remain heuristic model outputs (D-025).

### Open questions
- Which regular-cosponsor weight (0.5 or 0.2) does the published cu graph actually use, and should the component description name both by config?
- Should the Sotoudeh citation be relabelled "inspired by" in the brain and docstrings?
- Does the last-action passage regex over- or under-score the 9.6% of ties on ambiguous bills?

### Read first
- [[sotoudeh-2024-network-based-measure]]
- [[aref-2021-identifying-hidden-coalitions]]
- [[aref-2020-detecting-coalitions-optimally]]
- [[haythornthwaite-1996-social-network-analysis]]
- [[ringe-2013-pinpointing-powerful-voting]]
- [[azaiez-2025-hypergraph-analysis-european]]

## Bipartisanship (bpf)

**What it computes.** Per-member cross-party tie share from cosponsorship only (bipartisan.json / 100, default 0.24).

**Method.** Restored to cosponsor-only by S2-01 (bf4a703); BP_SRC=aug gives the unratified augmented definition.

**Code refs.** `fec/moneyball_core.py:68-72`, `fec/build_dashboard_bundle.py`

**Assumptions.**
- Cosponsorship alone measures cross-party bridging

**Evidence.** 1 rows (1 VERIFIED-SOFTENED), 1 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | qualifies | The Senate bipartisan cosponsorship index fell about 40% over the last 25 years (1999-2023), and legislator modularity is higher than lobbyists' and rising for senators. Lobbyist cross-party collaboration stayed relatively stable and lobbyist modularity is near zero. | The paper's index is a chamber-level aggregate, not a member-level trait. It shows the cross-party baseline is strongly era-dependent, so a bpf or default fixed in one window should not be carried across Congresses without naming the window. It does not validate bpf as a measure of bridging. | weak | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: near-GAP.** There is 1 row, Bacik 2025 (VERIFIED-SOFTENED, weak, qualifies).

Bacik reports that the Senate bipartisan cosponsorship index fell about 40% over 1999-2023. Legislator modularity is high and rising, while lobbyist cross-party collaboration stayed stable.

This is a chamber-level aggregate, not a member-level trait, so it does not validate bpf as a measure of bridging. What it does show is that the cross-party baseline depends strongly on the era. A per-member bpf, and above all the 0.24 default for missing members, must name its Congress window. It should not be carried across Congresses.

No row tests whether a member's cross-party cosponsorship share predicts recruitment success. No row compares cosponsor-only bpf with the unratified augmented (BP_SRC=aug) definition.

### Contradictions
- Paul's position cites "Bacik et al. 2026". The hub row is the 2025 paper "Measuring the dynamical evolution of the United States lobbying network". Either a different version or a year discrepancy. Confirm before citing.

### Gaps
- Member-level validity. The study needed: a held-out, next-Congress test of whether bpf predicts cross-party cosponsor recruitment on a member's own bills. Run it per chamber, controlling for DW-NOMINATE dim1 and district partisanship, and name the window and denominator.
- A head-to-head of cosponsor-only bpf vs the BP_SRC=aug definition on the same target.
- The basis for the 0.24 default is undocumented. Report the empirical distribution per chamber and per Congress, and state where 0.24 falls in it.

### Paul's position vs the evidence

Paul highlighted two findings from Bacik: legislators polarized while lobbyists did not, and bipartisan teams earn more. The verified row supports the first, at chamber level for the Senate. The "bipartisan teams earn more" finding is not in the verified claim. It is unconfirmed here and should not be quoted from this hub. Neither finding validates bpf as a member trait.

### Open questions
- Is "bipartisan teams earn more" a finding about lobbyist teams, and if so, does it bear on legislators at all?
- In which Congress window was bipartisan.json computed?

### Read first
- [[bacik-2025-measuring-dynamical-evolution]]
- [[aref-2021-identifying-hidden-coalitions]]
- [[aref-2020-detecting-coalitions-optimally]]
- [[poole-1999-nominate-after-years]]

## Social and press-engagement layers

**What it computes.** Directed edge probabilities from reposts (w 1.0) and mentions (0.4), Fink author-volume normalized (floor 25 posts, pair cap 0.25). Kernel-inverse weight w = 6p/(1-p), cap 2.0; reinforcement x(1 + 0.3 min(1, p/0.10)). Press-title engagement uses ENG_MIN_ITEMS 5.

**Method.** Precision-first name matching.

**Code refs.** `fec/network/social_interaction_layer.py:15-19`, `fec/network/press_engagement_layer.py:6-11`, `fec/network/build_multilayer_network.py`

**Assumptions.**
- Reposts and mentions indicate relational ties (Fink et al. 2023)

**Evidence.** 8 rows (1 UNVERIFIED, 1 VERIFIED, 6 VERIFIED-SOFTENED), 7 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[steinertthrelkeld-2018-twitter-data]] | 2018 | qualifies | Rebuilding an author's network takes the REST API: follower and friend lists, plus the 3,200 most recent tweets per account at 180 requests per 15 minutes and 200 tweets per request. The 1% streaming sample carries no information on the author's network. Network analysis itself is deferred to Steinert-Threlkeld (2016). | The access limits in the paper (3,200 cap, 1% stream, rate limits) do not bind here, because the corpus comes from a licensed Quorum export and not from the API. Those limits also describe the 2016 API, which X has since restricted and made paid. The useful point is a distinction, not evidence: a mention or repost network is a different object from a follower network, so the layer's 'relational tie' reading rests on Fink et al., not on this excerpt. No figure in the paper speaks to edge weights or normalization. | weak | UNVERIFIED |
| [[ahmad-nd-shortest-path-algorithms]] |  | qualifies | Twitter influence(X,Y) = Retweets(Y,X) / sum_Z Retweets(Y,Z): the share of Y's retweets that go to X. This is a directed weight normalized by the retweeter's total retweet volume (Eq. 3). The paper's reason for directed weights is that influence is asymmetric (Sec. 3.2). | The paper's evidence is descriptive: a 15-day undated Twitter crawl in which strongest paths are longer, with no outcome. It does not show that directed retweet edges predict anything legislative. At most it gives weak support for keeping a directed variant of this layer alongside the symmetric one before path or recruitment use. Per-edge volume normalization was measured and REJECTED in this project's SNA tests: do not read this as support for wider volume normalization. | weak | VERIFIED-SOFTENED |
| [[baptista-2022-universal-multilayer-network]] | 2022 | qualifies | Adding a layer helped in the airport case where bipartite overlap was high (German layer) and did not help in the biological case where the drug layer had low overlap (23% of genes and 5% of diseases shared). The paper says overlap might explain this. Randomizing the boosted bipartite links pushed LOOCV back to the two-network baseline. The authors conclude that more network data is not always better. | The design principle transfers: test each layer's held-out gain and report its overlap with the base graph. The 23% and 5% figures are biology-specific. Melange's builder merges layers into one weight, so the paper's per-layer jump parameters have no counterpart in the code. Gains and losses remain UNVERIFIED until measured on congressional data (DE-062). | moderate | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | qualifies | Pairs are tied by all the relationships they maintain. The strength of a tie may depend on the number and types of relationships a pair maintains, and on the strength of each relationship. | Multiplexity is conceptual and comes from survey-based ego networks. Here the layers are corpora of different quality. The untied and max rules are deliberate guards ruled by Paul, not recommendations to sum layers. | weak | VERIFIED-SOFTENED |
| [[zhou-2026-measuring-stance-dynamics]] | 2026 | suggests-extension | Active per-period user-user edges (mentions, quotes and replies, given as examples) add about 3 percent macro-F1 on both datasets. Passive follower or friend edges add little, and combining both does not help. The paper's Act graph is directed from the initiating user to the target. | The paper's evidence is that interaction edges help stance inference, not that they carry influence. Member accounts post far less than the paper's users, so expect sparser edges. Any test belongs in the descriptive lane. The layer's author-volume normalisation is a separate choice that this paper does not validate, and it must not be cited as support for the rejected per-edge volume normalisation. | weak | VERIFIED-SOFTENED |
| [[hargittai-2015-bigger-always-better]] | 2015 | qualifies | People do not select into SNS use at random, and selection differs by site and demographic group. Pew 2013 logistic coefficients (full sample, N 1,510 to 1,528, log-odds): female on Facebook 0.46 (p<.001), female on LinkedIn -0.39 (p<.01), Black on Twitter 0.63 (p<.01). Black on Facebook -0.31 is p<.10 only and -0.06 among Internet users, so it is not a reliable selection effect. Users of one platform are not the population. | Her coefficients do not transfer; her frame logic may. Her data are US adults (2013) and one 2009-2012 student cohort, not legislators or Bluesky. That X adoption bias is mostly gone for legislators is the analyst's inference: the paper never discusses institutional accounts (502 of 515 member rows have an X handle, read-only). The Bluesky handle field is populated for all 515 rows, so 135 is posting, not account adoption. Party-skewed posting may carry a party-skewed edge pattern into the layer; untested, UNVERIFIED (DE-062). | moderate | VERIFIED-SOFTENED |
| [[fink-2023-congressional-social-network-dataset]] | 2023 | contradicts | Influence of i on j is the count of j's retweets, quote tweets, replies and mentions of i's tweets divided by the tweets i issued in the window. Members with fewer than 100 window tweets are excluded, leaving 475 of 530 active accounts (525 with window tweets). | The pair weight in code is (ev(a,b)/vol(a) + ev(b,a)/vol(b))/2 with vol floored at 25 and each direction capped at 0.25. A Fink-consistent version would divide by vol(b) and vol(a) respectively. This is a code-read finding and stays UNVERIFIED (DE-062) until social_interactions.json is recomputed both ways. The source data are X/Quorum post text and Bluesky interactions, not Fink's API fields, so the analogy is looser than the docstring implies. | strong | VERIFIED-SOFTENED |
| [[fink-2023-congressional-social-network-dataset]] | 2023 | method-source | Edge weight is an empirical probability of influence: summed interactions of j on i divided by tweets issued by i. A minimum of 100 tweets per member is required so that the probabilities are meaningful. | Naming a co-actor in a press title is an analogy for engagement, not a reply or retweet. A 5-item floor on the source is far below Fink's 100-tweet floor, so the rates are much noisier. Fink validates nothing, so the docstring claim of validating cosponsor-edge probabilities has no basis in this paper. | moderate | VERIFIED |

### Assessment

**Evidence weight: moderate, and it contains the system's clearest contradiction.** There are 8 rows:
- Fink 2023: one VERIFIED-SOFTENED strong row that contradicts, and one VERIFIED moderate method-source row.
- Baptista 2022 and Hargittai 2015: moderate.
- Zhou 2026, Ahmad, and one Haythornthwaite row: weak.
- Steinert-Threlkeld: UNVERIFIED lead.

**Fink contradiction (social layer).** Fink's weight for i to j is j's interactions with i's tweets divided by the tweets i issued. `social_interaction_layer.py` instead divides by max(25, posts of the engaging member), which is the opposite end of the dyad. It then averages the two directions and drops direction.
- The floor is 25 posts, not Fink's 100-tweet exclusion.
- Quotes and replies collapse into 0.4 mentions.
- This is a code-read finding. It stays UNVERIFIED until social_interactions.json is recomputed both ways (DE-062).

**Fink and the press-engagement layer.** That layer uses a 5-item source floor against Fink's 100, so its rates are much noisier. Its docstring says it "validates cosponsor-edge probabilities". Fink validates nothing of the kind. Fink also calls its weight a "probability of influence". Melange must not inherit that language (D-025).

**Baptista: more layers are not always better.** Baptista found that a layer helped when its overlap with the base graph was high and did not help when overlap was low. No LOOCV, link-prediction or held-out test of layer gain exists in fec/network. The Provenance sheet is read by `leg_evidence_recompute` and the dashboard bundle, so layer tags reach output.

**Hargittai: platform selection.** The read-only counts show that Bluesky mentions are 392 of 411 Democrat-to-Democrat, and posters are 117 D, 16 R and 2 I. Bluesky events enter the numerator only. A party-skewed edge pattern is plausible but untested.

**Zhou 2026.** Active interaction edges (mentions, quotes, replies) add about 3% macro-F1 to stance inference. That is evidence for stance inference, not for influence. Reposts, the layer's heaviest edge, are not among Zhou's edge types.

**Haythornthwaite.** Multiplexity is conceptual only. The untied-pair and max rules are Paul's deliberate guards, not a mandate to sum layers.

**Flags.** Ahmad's normalization by retweeter volume, and the author-volume normalization in general, must not be cited as support for the REJECTED per-edge volume normalization of cosponsor edges.

### Contradictions
- CONTRADICTS Fink 2023, the cited framework. The social layer normalizes by the engaging member's post volume. Fink normalizes by the source member's tweets. The floor is 25 vs 100. Direction is discarded after averaging. The docstring's claim of "the same Fink discipline" is false as written (code-read; UNVERIFIED until recomputed).
- The press_engagement_layer docstring says the layer validates cosponsor-edge probabilities. Fink, its stated source, contains no validation, so the claim has no basis.
- Baptista row: the claim that the engine never reads the Provenance sheet is wrong. `leg_evidence_recompute.py` and `build_dashboard_bundle.py` read it.

### Gaps
- The component's empirical question (do Twitter interaction ties predict cosponsorship beyond party and state?) has no evidence. The study needed: per-layer held-out gain on next-Congress cosponsorship, with each layer's overlap with the cosponsor base reported, House and Senate separately.
- No evidence for the constants: kernel-inverse w=6p/(1-p) capped at 2.0, reinforcement x(1+0.3 min(1,p/0.10)), mention weight 0.4, pair cap 0.25.
- No study of party or platform skew in the social layer. The study needed: compare the D/R composition of edges with the roster, per platform, and test whether layer gain differs by party.
- Temporal weighting, one of Paul's critiques, has no evidence row. Zhou's per-period edges are the closest lead.

### Paul's position vs the evidence

Paul treats Fink as the pre-built framework. The evidence shows the implementation departs from Fink on the denominator, the floor and direction, so "pre-built" overstates the fidelity.

Paul's critiques of Fink (sign, third-party sentiment, temporal weighting) are reasonable but untested here. Jennifer and Tang, filed elsewhere, support the view that unsigned contact is not alignment.

His note from Baptista that more layers are not always better is directly supported, and it implies a per-layer held-out test that has not been run.

### Open questions
- Was the reversed denominator deliberate (normalizing by the engager's activity), or a transcription of Fink?
- Should the directed variant be kept alongside the symmetric one before any path or recruitment use?
- Should Bluesky events also enter the denominator, or should Bluesky be dropped from the numerator, to keep the rate coherent?

### Read first
- [[fink-2023-congressional-social-network-dataset]]
- [[baptista-2022-universal-multilayer-network]]
- [[hargittai-2015-bigger-always-better]]
- [[zhou-2026-measuring-stance-dynamics]]
- [[haythornthwaite-1996-social-network-analysis]]
- [[ahmad-nd-shortest-path-algorithms]]

## Press HCoI communities

**What it computes.** Eight Louvain communities over recency-weighted press-category profiles (Pearson >= 0.6, seed 7); topic-affinity edges <= 0.12 for cosponsor-isolated members.

**Method.** Hellinger genealogy across time. Homophily replication at press resolution was null.

**Code refs.** `fec/press_hcoi.py`

**Assumptions.**
- Shared issue attention indicates coalition (Malaterre & Lareau 2024)

**Evidence.** 7 rows (1 UNVERIFIED, 6 VERIFIED-SOFTENED), 5 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[zareie-2020-similarity-based-link]] | 2020 | qualifies | Indirect similarity (Pearson correlation of neighbourhood vectors) is combined multiplicatively with direct common neighbours as (1 + CN)(1 + Corr), and the paper never evaluates the correlation term alone. In Exp. 4 (pairs with equal common-neighbour counts), DICN is clearly worst on UAL (0.5684 vs SHOPI 0.7921); the paper gives no structural reason. The authors say small training sets make the latent relationship harder to detect. | The analogy is loose. HCoI correlates topic-attention profiles, not neighbourhood vectors. The transferable point is a design point: in DICN the profile correlation is one factor, anchored on direct shared ties. Placing cosponsor-isolated members (D4) by correlation alone is loosely analogous to Exp. 3 (no common neighbours), but that is still a heuristic placement, not evidence of a relationship (D-025). | weak | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | qualifies | Block modelling finds structurally equivalent actors by correlating all pairs of cases and then clustering the correlations. Structural equivalence means identical ties to and from all other actors. Structure should come from observed relationships rather than attribute classes. The article does not name tie-matrix rows as the profile that is correlated. | The procedure resembles block modelling but correlates attributes rather than relationships, so under the article's definitions HCoI is attribute similarity. That supports keeping it as a weak, surgical layer. The article says nothing on whether attention similarity predicts collaboration. | weak | VERIFIED-SOFTENED |
| [[zhang-2022-data-assisted-social]] | 2022 | context | Builds a term co-occurrence graph from cross-correlation of burst vectors (edge threshold 0.1) and partitions it with Louvain to find event communities; no modularity, stability or external-validity figure is reported. | The pipeline shape is the same (correlation, threshold, Louvain), but the nodes are terms, not legislators, and the threshold (0.1 vs 0.6) is unjustified and unvalidated. That gives no grounds to retune HCOI_CORR or to read the null homophily result differently. Malaterre remains the governing source. | weak | UNVERIFIED |
| [[malaterre-2024-inferring-social-networks]] | 2024 | method-source | LDA k=25 on 16,917 articles; author topic profiles per roughly 21-year period, weighted by authorship share; keep authors with weighted publications above 2; Pearson r at least 0.6; Louvain (Gephi default); community profile = average of member profiles; genealogy by closest Hellinger pairing between adjacent periods; modularity 0.427 to 0.834 and communities 4 to 13. | Mechanics carry over. The evidence does not: the paper covers an 88-year academic corpus in four periods with over 80 percent new authors per period and only qualitative validation. Here the split is two half-periods of one press stream, so NOVEL and PERSISTS labels reflect the script's own thresholds. Quote member denominators as 155 profiled, not the 385 in the script docstring, which is stale. | strong | VERIFIED-SOFTENED |
| [[malaterre-2024-inferring-social-networks]] | 2024 | qualifies | HCoIs are about who-talks-about-the-same-things-as-who. Links are mediated by texts, are not social ties, and serve as a heuristic for possible real ties. (The analyst gloss that communities are not labelled from alignment is not stated in the paper.) | The attention-not-alignment limit is definitional and transfers fully. Reading an HCoI as a coalition needs a signed profile (attention times stance), which neither the paper nor press_hcoi.py builds. | strong | VERIFIED-SOFTENED |
| [[malaterre-2024-inferring-social-networks]] | 2024 | qualifies | The paper offers no evidence that text-similarity ties carry influence or transmission; it frames HCoI mapping as a heuristic for actual social connections that would still need confirmation. | Using HCoI edges as a low-weight reachability patch fits the paper's heuristic framing. Treating them as transmission edges in cascade, coalition or route outputs goes beyond what the paper tested, and any output depending on them must say it rests on a topical-similarity heuristic. The 2026-08-18 ruling is cited only in code comments and was not checked. | moderate | VERIFIED-SOFTENED |
| [[ardekani-2026-computational-graph-theoretic]] | 2026 | suggests-extension | Louvain on the LDA-based Act graph gives 14 communities at modularity 0.393 (two-layer) and 15 at 0.392 (three-layer). Modularity above 0.3 is treated as meaningful. Community-level Jaccard alignment with committee keyword sets runs 0.10 to 0.66. Agglomerative merging into 6 macro-communities raises the Maori Affairs match to 0.67. | Adding a modularity Q and an external alignment check, such as Jaccard against committee rosters or cosponsor communities, is cheap and transferable. The paper's Q and Jaccard values come from Act citation graphs and are not benchmarks for member press graphs. The weak committee alignment is a caution that topical communities need not match institutions. The homophily null result is cited from project memory and was not checked here. | moderate | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: strong on mechanics, and against the coalition reading.** There are 7 rows:
- Malaterre 2024: three VERIFIED-SOFTENED rows (method-source strong, qualifies strong, qualifies moderate).
- Ardekani 2026: moderate.
- Haythornthwaite and Zareie: weak.
- Zhang 2022: UNVERIFIED lead.

**The pipeline follows Malaterre.** Melange profiles members over 20 curated press categories (standing in for Malaterre's LDA topics), links pairs at r >= 0.6, runs Louvain, and matches communities across periods by Hellinger distance.

**The evidence does not transfer.** Malaterre covered an 88-year academic corpus in four periods with qualitative validation only. Melange splits one press stream into two halves, so NOVEL and PERSISTS labels mostly reflect the script's own cutoffs (0.25 and 0.45).

**Malaterre's own definitions block the coalition reading.** HCoIs are "who talks about the same things". Links are mediated by texts, are not social ties, and serve only as a heuristic for possible ties. `profiles()` never reads stance, so a community can hold members on both sides of an issue.

**Haythornthwaite** classifies this as attribute similarity, not relational structure.

**Edge use.** Low-weight reachability patching fits Malaterre's framing: the ceiling is 0.12, about 0.02 kernel probability per step. Using HCoI edges as transmission edges in cascades, coalitions or routes goes beyond what was tested. Any output that depends on them must say it rests on topical similarity.

**Missing checks.** Ardekani suggests two cheap ones that do not exist: modularity Q, and external alignment such as Jaccard against committee rosters or cosponsor communities. Ardekani's weak committee alignment (0.10-0.66) is itself a caution.

**Stored artifact.** data/stance/press_hcoi.json holds:
- 155 members profiled
- 6 communities
- 1,900 edges
- 20 categories
- D4: 14 isolated members, 0 placed in a community

The 857-edge holistic count and the 4-of-41 reconnection count were not reproduced.

### Contradictions
- The component assumption "shared issue attention indicates coalition (Malaterre & Lareau 2024)" is contradicted by Malaterre itself. HCoIs are attention similarity, and the paper says they are not social ties or coalitions.
- The component says 8 communities over 19 categories. The stored artifact has 6 communities over 20 categories. The docstring's 385 members is stale against 155 profiled.
- D4 placement: the stored block shows 0 of 14 cosponsor-isolated members placed. The 4-of-41 reconnection figure cited elsewhere was not reproduced.

### Gaps
- The empirical question (do press-attention communities predict cosponsorship within community above chance?) has no evidence. The study needed: within-community cosponsorship rates against a degree- and party-preserving null, House and Senate separately, next Congress, reporting modularity Q.
- There is no signed profile (attention x stance), so no test of alignment is possible. Build one and re-run the homophily test before any coalition reading.
- Temporal genealogy uses only two half-periods. Testing stability needs more windows, or bootstrap resampling of the Louvain partitions.

### Paul's position vs the evidence

Paul tags Malaterre Very High and Foundational, and wants office closeness inferred from text with a temporal element via Louvain. The mechanics are faithfully built.

"Office closeness" is not what Malaterre measures: it measures topical co-attention, a heuristic needing confirmation. D-025 bars closeness or access language for any such score.

The null homophily result is what Malaterre's own caveat predicts. It is not a failure of the port. Paul's temporal element exists, but with two periods it cannot yet show anything about stability.

### Open questions
- Was the HCoI 2026-08-18 ruling (cited only in code comments) intended to keep HCoI as a reachability patch only?
- Should HCOI_CORR stay at Malaterre's 0.6 given 20 hand categories rather than 25 LDA topics?

### Read first
- [[malaterre-2024-inferring-social-networks]]
- [[ardekani-2026-computational-graph-theoretic]]
- [[haythornthwaite-1996-social-network-analysis]]
- [[zareie-2020-similarity-based-link]]

## Co-appearance and signed member-reference layers (display only)

**What it computes.** Pairs who both appeared substantively in the same interview episode (>= 4 turns or >= 200 words; 1,151 pairs). Signed directed references with LLM valence per context window (4,358 edges; green above +0.15, red below -0.15).

**Method.** Episodes with more than 8 members are skipped. Edges need at least 2 scored contexts. Name resolution never guesses.

**Code refs.** `fec/co_appearance_layer.py`, `member_reference_layer.py`

**Assumptions.**
- Appearing in the same episode indicates a relation
- LLM valence of references measures relation sign

**Evidence.** 6 rows (1 VERIFIED, 5 VERIFIED-SOFTENED), 5 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[zareie-2020-similarity-based-link]] | 2020 | context | The paper defines link prediction on plain unlabelled edges (E is a set of links, U = V x V) and has no treatment of negative, signed or valenced ties. It lists directed and weighted networks as future work. | There is no transfer to the signed layer, because the paper offers nothing on negative ties or valence. At most it could score unsigned co-appearance pairs for missing-link candidates. | weak | VERIFIED-SOFTENED |
| [[leskovec-2010-predicting-positive-negative]] | 2010 | method-source | Epinions balanced-sample accuracy about 93.4 percent (6.58 percent error, All23 model, minimum embeddedness 25, 10-fold CV). Slashdot and Wikipedia accuracies appear only in plotted Figure 1, not in the text. Balanced AUC about 0.90 and full-data AUC about 0.95 (text). Wikipedia self-trained accuracy 0.8021 (Table 6). Triad features add little at low embeddedness. | Needs an embeddedness census first: about 7,153 signed edges over 520 nodes, and many dyads may have few common neighbours, where triad features carry little information. Predictability measures consistency, not validity of the labels, and does not replace gold-label review. State the base rate and denominator with any reported accuracy. | moderate | VERIFIED-SOFTENED |
| [[leskovec-2010-predicting-positive-negative]] | 2010 | qualifies | In the undirected 4-type model, learned coefficients match balance theory for the three non-mm triad types in all three datasets. The enemy-of-enemy (mm) coefficient is negative in all three (-0.0149 Epinions, -0.0262 Slashdot, -0.0465 Wikipedia), disagreeing with balance. The authors say this is consistent with Davis weak balance, which makes no prediction for mm. | The pilot pattern is directed (a and b both point negatively at c). The paper's evidence is for the undirected collapse only; the Table 2 directed rows are hard to read in the text extraction, so no directed-level sign is claimed. The paper shows the mm pattern is not balance-like; it does not show the pattern carries no sign. | moderate | VERIFIED-SOFTENED |
| [[tang-2015-negative-link-prediction]] | 2015 | qualifies | Pairs with negative interactions hold negative links far above chance: t-test p = 5.72e-89 (Epinions) and 1.93e-109 (Slashdot). The rule negIn reaches precision 0.2097 and F1 0.2826 on Epinions, and precision 0.1483 and F1 0.1986 on Slashdot. The ratio of negative-link pairs tends to increase with the number of negative interactions K. Reliability weight f(Nij) = 1 - 1/log(1+Nij) applies to pairs with negative interactions; other pairs get a constant r. | Tang checks interaction-derived signs against declared negative links, which Congress lacks, so the transfer concerns interpretation only, not a calibrated precision. Tang's interaction labels are thresholded helpfulness ratings and MPQA lexicon polarity, a cruder reader than an LLM. Whether LLM-valenced precision is higher or lower is untested. Tang's count-based reliability weighting is a per-edge volume weight of the kind this project rejected, so it is flagged here, not recommended. | moderate | VERIFIED-SOFTENED |
| [[ringe-2013-pinpointing-powerful-voting]] |  | qualifies | Unsigned contact is not alignment. Between adversaries more contact goes with less agreement (about -10 percent odds of co-voting per connectivity unit, opposed-bloc dyads, EP Environment Committee 2004-2008). Authors identify ally or adversary status from voting bloc and party membership, not from the contact itself (p. 617). | Supports keeping the reference layer signed and warns against reading cross-party co-appearance as closeness or alignment (D-025). A broadcast pairing of opponents may be adversarial. The settings differ (EP staff contact vs US media co-appearance), so the evidence is weak. | weak | VERIFIED-SOFTENED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | supports | Results from closed, vendor-served models can become impossible to reproduce. The GPT-3 models behind the paper results were removed from the public-facing API in January 2024. | Caching keeps outputs auditable after a model is retired, but does not regenerate them; a re-score on a successor model is a new instrument. The warning applies because both layers call Anthropic-served models. The component label is a weak fit for LLM caching and should name the member-reference and persona LLM layers. | moderate | VERIFIED |

### Assessment

**Evidence weight: moderate, almost entirely from non-legislative web data.** There are 6 rows:
- Ornstein 2024: VERIFIED, moderate, supports.
- Leskovec 2010: two VERIFIED-SOFTENED moderate rows.
- Tang 2015: VERIFIED-SOFTENED, moderate.
- Jennifer and Zareie: weak.

**Tang 2015: negative interactions mark negative links, but with low precision.** Pairs with negative interactions hold declared negative links far above chance (p ~ 1e-89 to 1e-109). Precision is low, though: 0.15 to 0.21. On the Melange side, one scored negative context yields net = -1.0, and 567 of 1,161 negative edges (about 49%) rest on a single scored context. Those counts come from one read path and stay UNVERIFIED.

**Leskovec 2010** offers a sign-predictability consistency check (93% balanced accuracy on Epinions). It needs an embeddedness census first. Predictability measures consistency, not validity of the labels. Leskovec also shows that the enemy-of-enemy triad is not balance-like. That bears on the pilot's shared-negative-target stratum but does not give it a sign.

**Jennifer.** Between adversaries, more contact goes with less agreement. This supports keeping references signed, and warns against reading cross-party co-appearance as closeness (D-025).

**Ornstein.** Results from vendor-served models can become unreproducible. Model and response IDs are cached, which keeps outputs auditable, but re-scoring on a successor model is a new instrument.

**Zareie,** which Paul cites here, has no treatment of negative or signed ties. It gives no support for the signed layer.

**Flags.** Tang's count-based reliability weight is a per-edge volume weight of the REJECTED kind. An n_scored floor is a count threshold of the same family. Both are FLAGGED, not recommended on these papers' authority.

No row validates LLM valence against human gold in a congressional setting.

### Contradictions
- Edge counts disagree across descriptions. The component says 1,151 co-appearance pairs and 4,358 reference edges. co_appearance.json holds 2,481 pairs (1,182 bipartisan). member_references.json holds 7,944 edges. The signed-coalitions artifact holds 7,153 edges on 520 nodes. Each figure needs its vintage and filter named.
- The component says edges need at least 2 scored contexts. That holds only for the dashboard draw. Downstream, signed_coalitions takes every edge with no n_scored floor (MINW 0.0), so single-context negatives enter the partition at full weight.
- Zareie & Sakellariou 2020 is cited for this component but contains nothing on negative or valenced ties.

### Gaps
- The empirical question (do negatively signed references predict lower future cosponsorship for the pair?) is untested. The study needed: pair-level next-Congress cosponsorship conditioned on net sign, controlling for party, chamber and prior cosponsorship. State the denominator.
- No gold-label validation of LLM valence on congressional mentions. The study needed: a stratified human-coded sample with inter-rater agreement, comparing LLM precision against Tang's lexicon baseline.
- No embeddedness census or Leskovec-style sign predictability run on the mention layer.
- Ringe, Victor & Gross 2013 is on disk with ink only and has no verified bearing. It is a candidate source for signed-tie theory in legislatures.

### Paul's position vs the evidence

Paul's answer to negative ties is LLM valence per context window. Signing is supported in direction: Tang shows interaction-derived negatives carry signal, and Jennifer shows unsigned contact misleads for adversaries.

The evidence also says such signs are noisy, and half the negative edges rest on one context. Nothing validates the LLM reader for Congress. The Zareie citation does not bear on negative ties.

### Open questions
- Should signed_coalitions inherit the dashboard's n_scored >= 2 rule? This is a count-threshold choice in the flagged family, so it needs Paul's ruling and its own sensitivity run.
- Which of the three reference-edge counts (4,358, 7,153 or 7,944) is the current production vintage?

### Read first
- [[tang-2015-negative-link-prediction]]
- [[leskovec-2010-predicting-positive-negative]]
- [[ringe-2013-pinpointing-powerful-voting]]
- [[ornstein-2024-train-your-stochastic]]
- [[zareie-2020-similarity-based-link]]

## Signed-network coalition partition

**What it computes.** Coalitions from a signed backbone with frustration-minimizing local search seeded by positive Louvain.

**Method.** R backbone sdsm (alpha .05, signed). DW-NOMINATE vote edges were rejected in favour of the signed backbone.

**Code refs.** `fec/network/signed_coalitions.py`

**Assumptions.**
- Signed-network partitioning reveals hidden coalitions (Aref & Neal)

**Evidence.** 11 rows (3 VERIFIED, 8 VERIFIED-SOFTENED), 6 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | suggests-extension | Parties in Congress are, for the most part, spatially adjacent ideological coalitions (p.17, citing Krehbiel 1993), and party overlap is near zero (p.15). The paper does not discuss partitions of networks or cutting-line members. | Benchmark only: checking hidden-coalition candidates against \|dim1\| would show whether the mention layer adds information beyond roll-call ideology. Does not reinstate DW-NOMINATE vote edges, which Paul rejected (brain cosponsor-and-sector-networks topic, line 93). | weak | VERIFIED-SOFTENED |
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | method-source | The k-partition objective minimizes frustrated edges: negative edges inside a cluster plus positive edges between clusters (Eq. 1, giving C_k(G)). Eq. 2 finds the smallest k that reaches the minimum over all partitions (k*min). Both are solved to global optimality on 19 House networks (n ~445, 4,954 <= m <= 31,936 edges). Eq. 1 averages 3.3 h and Eq. 2 averages 14 h. | The objective transfers directly. The guarantee does not, because local search can stop in a local optimum. At 520 nodes and 7,153 edges the graph has fewer edges than the paper's largest network but slightly more nodes than the paper's n ~445, and the ILP's variable count grows as O(n^2). An exact Eq. 1 run is plausibly feasible but has not been demonstrated at this node count. | strong | VERIFIED-SOFTENED |
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | qualifies | On signed co-sponsorship backbones, House-only and per session, C_party > C_2 > C_3 holds in every one of the 19 sessions. A distinct third coalition of 4 members (113th, 2013) to 69 members (111th, 2009) appears. Negative ties are plentiful: members of the traditional coalitions have 2.68 negative edges per positive edge. | The evidence does not carry over. Mention valence measures expressed sentiment, while SDSM signs measure collaboration and avoidance against a degree-preserving null. The system's graph also mixes chambers and pools across Congresses, where the paper is House-only and per session. The system's null third-coalition result says nothing for or against the paper. A test needs a House-only, per-Congress signed co-sponsorship backbone. | strong | VERIFIED |
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | qualifies | The third coalition has a distinctive signed-tie pattern: 21.18 negative edges per positive edge, against 2.68 for traditional-coalition members, and 25.6% of its negative edges go to co-partisans, against 8.44%. Its members are the most effective coalition in most sessions. | The paper's ties come from co-sponsorship, not roll-call votes, so this is not a like-for-like contradiction. The relation is 'qualifies', not 'contradicts'. The paper shows that same-party negative ties distinguish the effective third coalition in co-sponsorship backbones. Whether the dropped vote-backbone negatives carry the same structure is untested. | moderate | VERIFIED-SOFTENED |
| [[leskovec-2010-predicting-positive-negative]] | 2010 | qualifies | Global balance is not significant. Best two-set partitions satisfy 0.8344 / 0.8105 / 0.7809 of edges (Epinions / Slashdot / Wikipedia), near the trivial all-positive value (about 0.80) and below degree-preserving rewired networks (0.8993 / 0.8310 / 0.8316). Sign-permuted baselines: 0.8562 / 0.7779 / 0.7866. | Paper signs are actor-declared on web platforms; ours are LLM-valenced speech mentions weighted by \|net\|. Only the comparison logic transfers. Any null for the Melange layer must preserve weights as well as degrees. | strong | VERIFIED |
| [[leskovec-2010-predicting-positive-negative]] | 2010 | suggests-extension | Global status ordering: 0.7905 / 0.8221 / 0.8538 of edges respect the best ordering, versus 0.7241 / 0.7568 / 0.7767 under sign permutation and 0.6377 / 0.6644 / 0.6321 under rewiring (random 0.50). Directed triad: a positive v-to-w-to-u path is evidence for a negative u-to-v edge, BB coefficients -0.0692 / -0.0293 / -0.0259 in the all-positive subgraph model. | Only meaningful if a negative public mention is an asymmetric judgement by the speaker about the target. Any ordering is a heuristic model output, never an influence, access or closeness claim (D-025). Dyads with edges in both directions are set aside by the paper and are not reciprocity evidence (D-104..D-108). | moderate | VERIFIED |
| [[tang-2015-negative-link-prediction]] | 2015 | qualifies | Balanced triads are 92.31 percent (Epinions) and 93.01 percent (Slashdot) of undirected triads (Sec. 3.2 p. 89). Setting negative-sample reliability weights to 1 (cj = 1) cuts precision by 8.70 percent on Epinions (Table 4) and 11.08 percent on Slashdot (Table 5). The balance regularizer raises precision from Cb = 0 to 0.001 (+4.02 and +4.41 percent), is relatively stable from 0.001 to 0.1 on Epinions, and drops sharply after 0.05 on Slashdot (Sec. 5.4, Fig. 5). Large Cb hurts. The paper does not say the regularizer helps only at small weight. | Tang's balance figures come from declared trust networks. The system's signs are interaction-derived, which is the paper's noisy-label case, and the paper does not estimate how much frustration comes from noise. Tang's count-based reweighting is the rejected per-edge volume kind. The n_scored >= 2 floor the summary proposes is a count threshold of the same family, so it is flagged, not recommended on this paper's authority. Any sensitivity run needs its own project approval. | moderate | VERIFIED-SOFTENED |
| [[ringe-nd-pinpointing-powerful-voting]] |  | qualifies | Signed (traditional) co-voting centrality cannot separate the cue-provider from majoritarians; with a third group following at 35 percent it makes majoritarians systematically more central than the actual cue-provider. The absolute-value measure corrects this. In simulation, cue-provider centrality is lowest when both follow probabilities are 50 percent and highest as they approach 0 or 1. | Paper is single-party cue-taking with simulated independent follow probabilities plus European Parliament MEP data, not US House or party-aligned backbones. The paper gives no account of why signed backbone edges cluster on party, so the majoritarian explanation is the analyst hypothesis and stays UNVERIFIED for US data and the 117th-119th Congresses until a second path reproduces it (DE-062). That dropping negative ties loses cue-provider information is an inference from the paper's use of negative correlations, not a paper critique of Melange. | moderate | VERIFIED-SOFTENED |
| [[aref-2020-detecting-coalitions-optimally]] | 2020 | method-source | The exact two-group frustration index comes from a binary program (Eq. 8: n+m binary variables, 2m+1 constraints) warm-started from party labels and bounded below by an LP relaxation with triangle constraints (Eq. 7). Heuristic partitioning is 'not guaranteed to provide the optimal solution'. Senate instances average 4 s (SD 6 s). The 113th House (m = 75,771) takes 48,823 s: 5,300 s for the LP (28,486,271 constraints) and 43,523 s for the binary solve. | The paper says the LP bound is not theoretically required, so Eq. 8 alone is exact. The LP is not the cost driver in the paper's own instance (5,300 s vs 43,523 s). The rel figure of about 23M triangle constraints at n=520 is C(520,3), the complete-graph triangle count. A read-only recount of the live mention graph on the 520-node set gives about 23.5k triangles, so Eq. 7 has about 100k rows (m+4T), not 23M. Solve times remain UNVERIFIED (DE-062). | strong | VERIFIED-SOFTENED |
| [[aref-2020-detecting-coalitions-optimally]] | 2020 | qualifies | Edges come from a significance-tested SDSM cosponsorship backbone (two-tailed alpha 0.05), built per chamber and per Congress. The near-party partition and the coalition-partisanship effect hold for the House and not the Senate. The paper reports F(G)=1-2L/m so networks can be compared. | The paper's findings rest on cosponsorship backbones within one chamber and session. A pooled mention partition crosses the House/Senate fault line, where the paper finds a House effect and none in the Senate, so any coalition reading needs an UNSTRATIFIED label. The splinter is a heuristic partition output, not measured antagonism. | strong | VERIFIED-SOFTENED |
| [[aref-2020-detecting-coalitions-optimally]] | 2020 | supports | Both chambers show high partial balance that rose over 1979-2016, which the paper reads as rising polarization that largely mirrors party lines. Triangle and normalized frustration indices correlate at 0.95 (House) and 0.91 (Senate). | Different substrate (roll-call agreement, not cosponsorship) and different era (2021-2026, not 1979-2016). The paper gives no numeric party-alignment statistic for its optimal partition; the party link is qualitative. The paper does not discuss the project's vote backbone at all. | moderate | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: strongest in the system, and it mostly says the current run cannot show what it is labelled to show.** There are 11 rows:
- Aref 2021: 3 rows.
- Aref 2020: 3 rows.
- Leskovec 2010: 2 VERIFIED rows (strong and moderate).
- Tang 2015, Ringe and Poole 1999: 1 row each.

**Method.** The Aref frustration objective transfers directly. The optimality guarantee does not: the system uses single-node local search with 60 restarts, k=1..3 only, and never searches for k*min.

Aref 2020 shows the exact binary program is feasible. A read-only recount gives about 23.5k triangles, so the LP-relaxation lower bound (Eq. 7) has about 100k constraints, not 23M; Eq. 8 alone is already exact. Solve times stay UNVERIFIED.

**Substrate.** This is the core problem.
- Aref's findings rest on SDSM cosponsorship backbones, House-only and per session.
- The near-party partition and coalition effect hold in the House, not the Senate.
- Melange partitions LLM-valenced media mentions, pooling House and Senate across Congresses.
- So the null third-coalition result says nothing for or against Aref. Any coalition reading crosses the chamber fault line and must carry an UNSTRATIFIED label.

**Null models.** Leskovec shows that global balance can look high and still be insignificant against degree-preserving rewiring. Melange has no null: 83.3% of edges are positive, about the trivial all-positive level. F(2) is 15.31% of total weight against F(1) 16.1%, a gap of 0.76 points. The k=2 result is a 506-member bloc plus a 14-member splinter, which is not evidence of structure.

**Direction.** Leskovec's status model needs the edge direction the partition discards.

**Dropped negatives and party.**
- Aref 2021 shows that same-party negatives distinguish the effective third coalition (21.18 negative-to-positive ratio). Melange's Option C vote backbone dropped all negatives. The substrates differ (votes vs cosponsorship), so this row qualifies rather than contradicts.
- Ringe suggests signed co-voting cannot separate cue-providers from majoritarians. The majoritarian explanation for the 95%-party vote backbone is the analyst's own hypothesis, UNVERIFIED.
- Aref 2020 reports rising partial balance that mirrors party lines, consistent with that 95% figure.
- Poole: compare the splinter candidates against dim1 as a benchmark only. This does not reinstate NOMINATE vote edges.

**Flags.** Tang's reliability reweighting and an n_scored floor are the volume or count family. Both are FLAGGED.

**Display reach.** The output reaches the member desk as an "antag" flag from splinter_k2, and is also read by dashboard_integrity and the shadow recompute. That is a heuristic partition output, not measured antagonism (D-025).

### Contradictions
- The docstring says the chain does not consume the output. In fact make_member_desk.py reads splinter_k2 and sets an 'antag' flag, and dashboard_integrity and run_legislation_shadow_recompute read the file. The claim holds only for the engine.
- The component method says "R backbone sdsm (alpha .05, signed)", but signed_coalitions.py partitions the mention layer. The sdsm backbone was the separate June vote backbone (Option C), so the description conflates two substrates.
- Paul values Aref & Neal as computationally efficient. Aref's exact method takes hours per House session (3.3 to 14 h on average; 48,823 s for the 113th). Only the system's local search is cheap, and it carries no optimality guarantee.
- Aref 2020 and 2021 find the coalition effect in House cosponsorship, not the Senate. The system pools both chambers and Congresses, so the paper cannot be cited as support.

### Gaps
- The empirical question (do signed-partition coalitions predict joint voting on CU roll calls better than party?) is untested. The study needed: a House-only, per-Congress signed SDSM cosponsorship backbone, partitioned exactly (Aref 2020 Eq. 8). Score held-out CU roll-call co-voting against a party-only baseline. Treat it as a diagnostic, and FLAG that SDSM conditions on member volume.
- No null model. Add degree- and weight-preserving rewired nulls and sign-permuted nulls (Leskovec) before reading any F(k) gap.
- No tracking over time and no directed analysis. Per-Congress partitions with Aref's normalized F(G), plus a Leskovec status test on directed mentions, would address Paul's stated gaps.

### Paul's position vs the evidence

Paul values Aref & Neal as a computationally efficient alternative layer, and names two gaps: no path-finding and no tracking over time. The objective is well grounded. The efficiency claim holds only for the heuristic, which forfeits the paper's guarantee.

Tracking over time is exactly what Aref 2020 does (1979-2016, per Congress). The fix is per-Congress, per-chamber runs, not a new method.

The larger issue Paul did not name is substrate. Aref's evidence is cosponsorship backbones, not media-mention valence, so the current partition borrows the method without the evidence.

### Open questions
- Should the member-desk 'antag' flag be suppressed or relabelled until a null-model test exists?
- Is a signed cosponsorship backbone acceptable as a diagnostic-only layer, given SDSM's volume conditioning sits next to the rejected per-edge normalization?
- Should the dropped negative side of the vote backbone be re-examined for same-party negatives, per Aref 2021's third-coalition pattern?

### Read first
- [[aref-2021-identifying-hidden-coalitions]]
- [[aref-2020-detecting-coalitions-optimally]]
- [[leskovec-2010-predicting-positive-negative]]
- [[tang-2015-negative-link-prediction]]
- [[ringe-nd-pinpointing-powerful-voting]]
- [[poole-1999-nominate-after-years]]

## Press-lean coalition detection (Ward) and bloc profiles

**What it computes.** Ward.D2 clusters on member lean profiles (9 blocs) and a combined bloc index.

**Method.** COALITION_LEAN_WEIGHT 3.0, min 6 members. Static snapshot. All blocs are labelled pro-CU (a known mislabel).

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R (detect_coalitions)`

**Assumptions.**
- Similar press lean profiles form coalitions

**Evidence.** 3 rows (1 UNVERIFIED, 2 VERIFIED-SOFTENED), 3 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[pritoni-2025-lobbying-coalitions-determinants]] | 2025 | qualifies | Following Junk (2020b), the review separates three definitions of a lobbying coalition: policy camp (shared preferences only; 12/80 articles), behavioral coordination ('a non-trivial degree of coordinated activity over time'; 59/80) and formal organization (6/80). For the 16-article meta-analysis the authors keep only studies whose unit of analysis is the individual interest group or lobbyist, and they drop policy-camp studies (selection criteria i-ii). | The paper supplies a definition only. The group-or-lobbyist unit is a selection rule for the meta-analysis subsample, not a finding about the whole literature, and its determinants (weakness, brokers) concern groups choosing allies. They should not be read onto legislator blocs. If the blocs keep the name 'coalition', the label should say they are shared-position clusters with no coordination evidence. | moderate | VERIFIED-SOFTENED |
| [[malaterre-2024-inferring-social-networks]] | 2024 | suggests-extension | Communities in adjacent periods are matched by closest Hellinger distance to trace persistence, splits and novel communities; four novel communities appear in 1974-95 with no clear filiation. | The genealogy step would answer the stability question only if blocs were re-run per period and matched by Hellinger distance. Unlike HCoIs these blocs include stance lean, so a genealogy here would track alignment. The paper's evidence for the step is qualitative, so this stays a weak bearing. | weak | VERIFIED-SOFTENED |
| [[cao-2025-social-network-group]] | 2025 | qualifies | Pseudoconsensus: clustering raises homogeneity within subgroups but also heterogeneity between them, so high internal consensus need not mean the subgroups agree with one another. | The point is logical, not empirical, so it carries over to any clustering. Agreement within a bloc says nothing about alignment across blocs or with CU positions. A bloc label needs a between-bloc check of direction against an external criterion such as votes. No numbers transfer. | weak | UNVERIFIED |

### Assessment

**Evidence weight: definitional only.** There are 3 rows: Pritoni 2025 (VERIFIED-SOFTENED, moderate), Malaterre 2024 (VERIFIED-SOFTENED, weak) and Cao 2025 (UNVERIFIED lead).

**Pritoni (following Junk) defines a coalition by coordination.** Its three definitions are policy camp (shared preferences only), behavioral coordination over time, and formal organization. Ward.D2 on legislators' press-lean profiles observes no coordination, so the blocs are at most policy camps. If the label "coalition" stays, it should read "shared-position clusters, no coordination evidence".

Pritoni's determinants (organizational weakness, brokers, salience) concern interest groups choosing allies. They should not be read onto legislator blocs.

**Malaterre's Hellinger genealogy** would answer the stability question only if blocs were re-run per period and matched. Because these blocs include stance lean, a genealogy here would track alignment, unlike HCoI. Malaterre's evidence for the step is qualitative.

**Cao (UNVERIFIED lead):** within-bloc homogeneity says nothing about between-bloc agreement or alignment with CU. Bloc labels need an external directional check, such as votes.

**Code read.** k = max(2, min(8, round(sqrt(n/2)))), so there are at most 8 blocs, not 9. Labels come from mean aggregate lean (pro, hostile or mixed). The recorded "all blocs pro-CU" mislabel is not supported by current code; it may describe an older run.

### Contradictions
- The component says 9 blocs. The code caps k at 8 when no k is passed, and the call passes none.
- The component and brain record "all blocs are labelled pro-CU (a known mislabel)". Current code assigns pro, hostile or mixed by mean lean of +/-0.1, and two independent code reads found no basis for the claim. Either the description is stale or a stored artifact predates the fix.
- Naming these blocs "coalitions" conflicts with Pritoni and Junk's definitions, which require coordinated activity.

### Gaps
- The empirical question (are press-lean clusters stable across time windows?) is untested. The study needed: re-run Ward per window (rolling or per Congress), match blocs by Hellinger distance or ARI, and report bootstrap stability per chamber.
- No external validation. The study needed: test whether bloc co-membership predicts co-voting on CU roll calls beyond party, with a between-bloc directional check.
- The choice of COALITION_LEAN_WEIGHT 3.0 and the 6-member minimum is not sourced.

### Paul's position vs the evidence

Paul highlighted that SNA is underused in coalition studies, and that salience and third-party brokers drive coalitions. He left no annotation on this component.

This component is not network analysis: it is attribute clustering. Pritoni's broker and salience findings are about interest groups. So the component neither implements nor tests Paul's highlighted points. A network-based coalition test would live in signed-coalitions, or in a broker-centrality analysis on the cosponsor graph.

### Open questions
- Is the "all pro-CU" mislabel a stale note to retire, or does a stored workbook still carry it?
- Should the bloc label change to "shared-position clusters"?

### Read first
- [[pritoni-2025-lobbying-coalitions-determinants]]
- [[malaterre-2024-inferring-social-networks]]
- [[cao-2025-social-network-group]]

## Sector network configurations

**What it computes.** Bank-regulation and crypto cosponsor graphs alongside the CU graph.

**Method.** Hub concentration flag at hub degree >= 3x the second highest. Recorded: bank_reg 4,742 dyads, 76.1% novel vs CU; crypto p_max 0.23 vs CU 0.52 (below the engine floor).

**Code refs.** `fec/moneyball_core.py NETWORKS registry`

**Assumptions.**
- Sector bill universes define sector-specific influence networks

**Evidence.** 3 rows (2 VERIFIED, 1 VERIFIED-SOFTENED), 3 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | suggests-extension | Core-periphery and rich-club claims are tested against hypergraph configuration null models (Chodrow 2020) that preserve degree and edge size, using rho(k) = phi(k)/phi_rand(k). Stakeholders show rho > 1 for all k. Large meetings drive connectivity. | A null-model rich-club or core test would show whether a sector's concentration exceeds what its degree and bill-size sequence already implies. The 3x rule cannot separate that. This only holds if sector bill rosters are treated as hyperedges. With the thin samples (crypto p_max 0.23), null-model variance will be large. Any result is UNVERIFIED until reproduced (DE-062). | moderate | VERIFIED |
| [[kim-2020-mapping-political-communities]] |  | suggests-extension | Legislation communities (Healthcare, Veterans' Affairs, Technology & Telecommunications, Energy, Finance & Insurance, and others) are estimated from interest-group x politician lobbying counts and named ex post. A permutation test against random networks with similar actor activity distributions finds interest groups in the null have many more community memberships, indicating strong community structure. | A biLCM on the existing org x bioguide counts could only be a descriptive diagnostic of whether hand-drawn sector boundaries appear in lobbying. Under D-021 its outputs must not become edges or weights; UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED |
| [[ardekani-2026-computational-graph-theoretic]] | 2026 | qualifies | Where the network boundary is drawn changes who is central: the top-10 Katz ranks reshuffle between the 2-layer and 3-layer subgraphs. The authors attribute part of the seed's 2-layer prominence to construction bias, since the seed is the root of the subgraph. | The abstract point transfers: cross-universe centralization comparisons depend on how each bill universe was screened. The paper bounds by citation hops from one seed, not by topic screen, so its effect size does not transfer. Any cross-config comparison must name its window and denominator. The bank_reg 76.1 percent novelty figure is a run output not checked here. | weak | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: weak to moderate, all suggesting extensions.** There are 3 rows: Azaiez 2025 (VERIFIED, moderate), Kim & Kunisky (VERIFIED, weak) and Ardekani 2026 (VERIFIED-SOFTENED, weak).

**Azaiez: the 3x hub rule needs a null model.** The rule (hub degree at least 3x the second-highest) cannot tell real concentration apart from what a sector's degree and bill-size sequence already imply. A hypergraph configuration null, with a rich-club rho(k), would. The crypto "dominated" flag is one 167-degree star from the single CBDC cosponsor roster. That is a bill-size artifact, which is exactly the case the null controls for. With thin samples (crypto p_max 0.23, below the engine floor), null-model variance will be large.

**Ardekani: boundaries change centrality.** Drawing the boundary differently reshuffles who is central. Cross-universe centralization comparisons (cu vs bank_reg vs crypto) depend on how each universe was screened and must name their window and denominator.

**Kim & Kunisky.** A latent-community model on lobbying counts could serve as a descriptive check of whether hand-drawn sector boundaries appear in lobbying. Under D-021, LDA is an identity source only: no output may become an edge or weight. Any result stays UNVERIFIED until reproduced.

The bank_reg figures (4,742 dyads, 76.1% novel vs CU) are run outputs not re-checked here.

### Contradictions
- No direct contradictions. The 3x hub-concentration rule is called into question: Azaiez's null-model logic implies the crypto 'dominated' flag may be a single-bill-size artifact.

### Gaps
- The empirical question (does sector-network centralization predict bill passage rates?) is untestable with three sector configs. The study needed: many sector universes (CRS policy areas) across several Congresses, with a centralization measure computed against a hypergraph configuration null, regressed on passage rates. Report per chamber, or label UNSTRATIFIED.
- Paul's rival-industry conflict mapping has no evidence row. The study needed: identify members central in two opposed sector graphs, and test whether their cosponsorship or votes split by sector. LDA may be used only to identify sector actors (D-021).
- No sensitivity of sector membership to the screening rule (CRS terms vs title screen).

### Paul's position vs the evidence

Paul wants rival-industry champions and conflict mapped across sectors. None of the three rows addresses conflict between sectors. The evidence speaks only to whether the boundaries and concentration flags are sound, and the answer is: not yet, without a null model.

The Kim citations point to lobbying-network communities. D-021 limits these to identity, not to edges or weights, so they cannot carry the conflict map directly.

### Open questions
- Should bill rosters be treated as hyperedges for the sector diagnostic, as Azaiez does?
- Is the crypto network usable at all, given p_max 0.23 below the engine floor?
- "Kim J. et al. 2025" in Paul's citations vs kim-2020-mapping-political-communities (Kim & Kunisky): same work or different?

### Read first
- [[azaiez-2025-hypergraph-analysis-european]]
- [[kim-2020-mapping-political-communities]]
- [[ardekani-2026-computational-graph-theoretic]]

## Katz-Bonacich benchmark (diagnostic)

**What it computes.** Katz-Bonacich centrality on the live kernel (delta = 0.85 / lambda_max), Spearman-correlated with allocations, money and legislative score.

**Method.** Power iteration. Not consumed by the chain.

**Code refs.** `fec/engine/kb_benchmark.py:1-104`

**Assumptions.**
- Equilibrium contributions are proportional to Katz-Bonacich centrality (Battaglini & Patacchini)

**Evidence.** 6 rows (1 VERIFIED, 5 VERIFIED-SOFTENED), 5 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[ardekani-2026-computational-graph-theoretic]] | 2026 | qualifies | Katz prestige counts incoming directed paths. On one seed the Mental Health Act ranks 2nd at 2 layers (2,159 Acts) and outside the top 50 at 3 layers (6,021 Acts). Corrections Act moves 7th to 35th; Health Practitioners Competence Assurance Act moves 8th to 37th. The attenuation factor alpha is not reported. | Only the radius-sensitivity lesson transfers: ranks computed on a bounded subgraph are local. The paper's Katz counts in-paths (prestige) on an Act-citation graph, while the system's KB counts out-walks on a legislator kernel, so the paper does not validate the benchmark's direction. The transferable check is KB rank stability across cu and holistic, UNVERIFIED until reproduced (DE-062). KB values are heuristic model outputs, never influence probabilities (D-025). | moderate | VERIFIED-SOFTENED |
| [[battaglini-2016-influencing-connected-legislators]] | 2016 | method-source | Office-motivated legislators with log utility get transfers exactly proportional to their Bonacich centrality b = (I - beta*G)^-1 * 1 (Prop 1, eq 4, eq 17-18). With policy-motivated legislators the weights become modified Bonacich centralities depending on preferences and pivot probabilities (eq 15, Prop 2), which converge to the office-motivated allocation as the number of legislators grows (Prop 3). Reported network externality phi = 0.2088 (SE 0.0697) committee, 0.2165 (SE 0.0703) corrected, 0.0837 (SE 0.0262) alumni (Table 2). | Exact proportionality holds only under office motivation with log utility; the docstring overstates it for the general case. Under policy motivation the weights depend on preferences and pivot probabilities. The paper's empirical networks are undirected; the system kernel is directed. The delta-versus-phi comparison (0.85 of the bound versus phi 0.08-0.25) is not like-for-like: delta is a Katz decay on the kernel, phi is an SAR coefficient. | strong | VERIFIED-SOFTENED |
| [[battaglini-2016-influencing-connected-legislators]] | 2016 | qualifies | The paper deliberately does not use cosponsorship as the network, because cosponsorship is determined simultaneously with contributions (fn 30). Sample is U.S. House members in the 109th-113th Congresses (2004-2012 cycles); outcome is total PAC money in logs, trimmed at the 1st and 99th percentiles. Controls in Table 2: margin of victory -0.897, DW_ideology -1.077, chair +0.401. | A KB-versus-money correlation on the cosponsorship graph cannot test the equilibrium the paper derives, since cosponsorship is the endogenous network the paper excludes. The pooled two-chamber estimates need an UNSTRATIFIED label. Paper evidence is House-only, 2004-2012; the money window for the system file is not recorded. | moderate | VERIFIED-SOFTENED |
| [[trivedi-2020-efficient-influence-maximization]] | 2020 | qualifies | PrKatz (Katz-based, Alshahrani et al. ref [1]) does worse than plain degree under ICM and on NetHept falls below Random for some k. The authors partly blame pre-processing that splits the graph into components. Katz centrality alone matches degree at small k and declines as k grows. | The paper concerns Katz as a seed-selection proxy for cascade spread and finds it weak. The system uses KB as an equilibrium-pricing benchmark, so the evidence only discourages promoting KB into seed choice. The rel's claim that nothing in the chain consumes KB is wrong: refresh_table.py:261 runs it as a DIAG step, surfaces/make_member_desk.py:825 reads kb_benchmark.json for display, legislation/run_legislation_shadow_recompute.py:403 holds an immutable-input contract on it, and audit/dashboard_integrity.py:625 checks it. KB values are heuristic model outputs, never access or closeness probabilities (D-025). | weak | VERIFIED-SOFTENED |
| [[moreno-2021-shadowing-shielding-effective]] | 2021 | qualifies | The degree correlation of optimal allocations is a by-product of shielding: Kendall tau is generally above 0.5 for K above 30 for both optimal and pure-shielding plans (Fig 5b). When the passive controller targets nodes whose neighbours have the lowest degree, \|tau\| for optimal allocations falls below 0.25 while shielding gives tau down to about -0.5 (Fig 5c). The authors conclude that optimal allocations can be explained almost perfectly by shielding without degree preference (p.11). | An allocation-centrality correlation, high or low, can come from where the opponent places money rather than from a preference for centrality, so any reading of rho should be stratified by industry-money placement or labelled UNSTRATIFIED. Rho is a heuristic diagnostic, not closeness or influence evidence (D-025). The paper's evidence is one email network. | weak | VERIFIED |
| [[almazaydeh-2026-heuristic-centrality-methods]] | 2026 | suggests-extension | Simple centrality seed rules come close to sampling-based IM under IC (PageRank 96.5 percent of IMM) at far lower runtime (2,304 vs 2.95 spread per second at 1000 nodes). | Adding a seed-spread column would test the paper's question on Melange's own kernel, as a diagnostic only, labelled UNSTRATIFIED, with window and denominator named. KB remains a model output, not a measure of access (D-025). Nothing in the chain consumes it. | weak | VERIFIED-SOFTENED |

### Assessment

**Evidence weight: strong, and it undercuts the benchmark's design.** There are 6 rows:
- Battaglini & Patacchini 2016: strong method-source and moderate qualifies.
- Ardekani 2026: moderate.
- Moreno 2021: VERIFIED, weak.
- Trivedi 2020 and Almazaydeh 2026: weak.

**Battaglini: the equilibrium does not transfer as stated.**
- Exact proportionality of transfers to Bonacich centrality holds only for office-motivated legislators with log utility. Under policy motivation the weights become modified centralities that depend on preferences and pivot probabilities. The docstring's "in proportion" overstates this.
- The paper's networks are undirected (committee and alumni); the kernel here is directed.
- KB_DELTA 0.85 of the bound is a Katz decay. Battaglini's phi (0.08-0.25) is a spatial autoregressive (SAR) coefficient. The two are not comparable.

**Battaglini also excludes cosponsorship by design (fn 30).** Cosponsorship is determined simultaneously with contributions, so a KB-vs-money correlation on the cosponsor graph cannot test the equilibrium. The paper is House-only, 109th-113th Congresses.

The stored Spearman correlations pool 533 members of both chambers, with no controls and no recorded money window:
- optimizer: 0.2468
- CU money: 0.1347
- bank plus finserv money: 0.2304
- legislative score: 0.6535

They need an UNSTRATIFIED label. MEA-6 holistic KB-vs-allocation +0.225 (n=533) is likewise pooled.

**Moreno: an allocation-centrality correlation can come from where opponents place money,** not from a preference for centrality. Stratify by industry-money placement.

**Ardekani's Katz** counts incoming paths (prestige) on Act citations. KB here counts out-walks on a legislator kernel, so the paper does not validate the direction. Its transferable lesson is rank sensitivity to the network boundary: check KB rank stability across cu and holistic.

**Seed selection.** Trivedi finds Katz a poor seed proxy, at or below degree. This discourages promoting KB into seed choice. Almazaydeh suggests adding a seed-spread column as a diagnostic.

KB values are heuristic model outputs, never influence, access or closeness probabilities (D-025).

### Contradictions
- The benchmark computes KB on the cosponsorship network. Battaglini & Patacchini explicitly reject cosponsorship as the network for this test, because it is determined simultaneously with contributions (fn 30). The benchmark cannot test the paper's equilibrium as built.
- The kb_benchmark.py docstring says groups allocate "in proportion to KB centrality". The paper shows that only under office motivation with log utility.
- The docstring says the output is "not consumed by the chain". It is read by make_member_desk.py (kb_pct percentile on the desk), run as a DIAG step in refresh_table.py, held under an immutable-input contract in run_legislation_shadow_recompute.py, and checked by dashboard_integrity.py.
- Paul highlighted Ardekani's choice of Katz over PageRank and closeness. That Katz is in-path prestige on a citation graph, the opposite direction from the system's out-walk KB.

### Gaps
- The empirical question (do observed PAC contributions correlate with KB centrality?) needs a design that transfers. The study needed: replicate Battaglini on an exogenous network (committee co-membership or alumni ties), House-only and per Congress, with log PAC money and controls (margin of victory, DW ideology, chair). Name the money window.
- KB rank stability across NET_CONFIGs (cu vs holistic vs holistic_norm) is unmeasured.
- No stratification of the allocation-KB correlation by industry-money placement (Moreno).

### Paul's position vs the evidence

Battaglini & Patacchini is listed without annotation. Paul highlighted Ardekani's choice of Katz centrality. The evidence says:
- The theory source supports Bonacich-proportional allocation only under narrow assumptions, and on a network the paper deliberately avoids.
- The highlighted Ardekani usage is a different Katz (prestige, in-paths) on a different object.

The benchmark is a reasonable diagnostic, but its current Spearman correlations are pooled across chambers and determined jointly with money. They are not a test of the theory.

### Open questions
- Should the docstring be corrected on two points: office-motivated-only proportionality, and the display consumers?
- Is a committee co-membership network available to rebuild the benchmark on an exogenous graph? It would also be the first committee layer, Porter-style, and must not reweight cosponsor edges.
- What money window produced the 0.1347 and 0.2304 correlations?

### Read first
- [[battaglini-2016-influencing-connected-legislators]]
- [[moreno-2021-shadowing-shielding-effective]]
- [[ardekani-2026-computational-graph-theoretic]]
- [[trivedi-2020-efficient-influence-maximization]]
- [[almazaydeh-2026-heuristic-centrality-methods]]

## Across components

## Cross-component patterns

**1. Cited methods are implemented looser than the cited papers.** This pattern repeats across 5 of the 9 components:
- Sotoudeh: a pass/total ratio became a volume-summed x3 boost.
- Fink: the denominator moved to the opposite end of the dyad (a CONTRADICTION).
- Malaterre: attention similarity was read as coalition.
- Aref & Neal: the objective was ported onto a different substrate, using heuristic search.
- Battaglini: the benchmark is built on the network the paper deliberately excludes.

The brain and the docstrings should say "inspired by" wherever the paper's evidence does not transfer.

**2. Fault lines are pooled.** The signed-coalition partition and the KB benchmark both pool House and Senate members, across Congresses. Aref finds the coalition effect in the House and not the Senate. Battaglini is House-only. Every coalition reading or correlation from these outputs must be stratified by chamber and Congress, or carry an UNSTRATIFIED label with window and denominator.

**3. Rejected-proposal flags recur.** Four papers suggest weightings in or next to the REJECTED per-edge volume normalization family:
- the SDSM degree-preserving null (Aref 2020, 2021)
- Porter's observed/expected interlock
- Tang's count-based reliability weight, and by extension an n_scored floor
- Ahmad's normalization by retweeter volume

All are FLAGGED, not recommended. The layer-level author-volume normalization in Fink is a separate design choice and must not be read as support for the rejected one. No paper in the hub argues for dropping regular cosponsorship edges.

**4. Nothing is validated on held-out data.** The evidence suggests a cheap check for most components, and none has been run:
- per-layer held-out gain (Baptista)
- rewired and sign-permuted nulls for partitions (Leskovec)
- modularity and external alignment for communities (Ardekani)
- a hypergraph null for hub concentration (Azaiez)
- stratification by opponent money placement (Moreno)

Every component's stated empirical question is still open.

**5. Diagnostic outputs reach display surfaces.** The signed-coalition splinter flag ('antag') and the KB percentile reach the member desk, and the multilayer Provenance sheet reaches the legislation recompute and the dashboard bundle. Their docstrings say they are not consumed. Because they are displayed, D-025 wording discipline applies: heuristic outputs, never closeness, access or antagonism measures.

**6. Recorded descriptions are stale.** Counts and labels in the component descriptions disagree with code or artifacts:
- HCoI: 8 vs 6 communities, 19 vs 20 categories, 385 vs 155 members.
- Co-appearance: 1,151 vs 2,481 pairs.
- References: 4,358 vs 7,153 vs 7,944 edges.
- Ward: 9 blocs vs at most 8, and the "pro-CU" mislabel is unsupported by code.
- Regular-cosponsor weight: 0.5 vs 0.2.

Every figure quoted from these components should name its vintage, window and denominator.

**7. Transfer distance is large.** Much of the evidence comes from settings other than Congress:
- EC lobby meetings (Azaiez)
- European Parliament staff contacts and EP voting (Jennifer, Ringe)
- New Zealand Act citations (Ardekani)
- Epinions and Slashdot (Leskovec, Tang)
- an academic corpus (Malaterre)
- general-population SNS surveys (Hargittai)

Only Sotoudeh, Aref, Battaglini and Bacik use US Congress data, and each uses a different substrate or chamber than the system.

**8. Reciprocity stays unvalidated** (D-104 to D-108). Leskovec sets aside bidirectional dyads, and nothing in the hub tests reciprocity.

**9. LLM-scored layers.** The member-reference layer depends on vendor-served models. Caching keeps outputs auditable, but re-scoring on a successor model is a new instrument (Ornstein).

## Research agenda

1. Cosponsor weighting held-out benchmark: predict next-Congress cosponsorship and co-voting from raw counts, role-weighted, success-weighted (x3) and Sotoudeh pass/total-ratio graphs, House and Senate separately, with window and denominator named. Do not test the rejected options (dropping regular edges, per-edge volume normalization).
2. Fix-or-confirm the Fink denominator: recompute social_interactions.json with both the engaging-member and the source-member denominator, a 25 vs 100 floor, and directed vs symmetric output. Then measure per-layer held-out gain and overlap with the cosponsor base (Baptista), per chamber, including a party and platform composition check (Hargittai).
3. Signed coalitions on the right substrate: a House-only, per-Congress SDSM cosponsorship signed backbone, partitioned exactly with Aref 2020 Eq. 8, compared with rewired and sign-permuted nulls (Leskovec). Score against held-out CU roll-call co-voting vs party. Diagnostic only. Flag SDSM's volume conditioning.
4. Mention-layer partition hygiene: add weight- and degree-preserving nulls, stratify by chamber and Congress, and report normalized F(G). Suspend or relabel the member-desk 'antag' flag until a null-model test passes.
5. Valence validity: a stratified human gold-label sample of member-reference contexts with inter-rater agreement, plus Leskovec sign predictability after an embeddedness census. Then test whether negative references predict lower next-Congress cosponsorship for the pair.
6. HCoI validation: compute modularity Q, then test within-community cosponsorship against a degree- and party-preserving null per chamber. Build a signed (attention x stance) profile before any coalition reading. Correct the stale counts (6 communities, 20 categories, 155 profiled).
7. KB benchmark redesign: replicate Battaglini on an exogenous network (committee co-membership or alumni), House-only and per Congress, with log PAC money and paper controls, naming the money window. Also measure KB rank stability across NET_CONFIGs, stratify by opponent money placement (Moreno), and correct the docstring's proportionality and consumer claims.
8. bpf member-level validity: test whether cross-party cosponsor share predicts cross-party recruitment on a member's own bills per chamber, controlling for DW dim1 and district lean. Compare with BP_SRC=aug, and document the window behind bipartisan.json and the 0.24 default.
9. Ward bloc stability: re-run per window, match blocs by Hellinger distance or ARI with bootstrap stability, validate directionally against CU roll calls, and rename the blocs 'shared-position clusters' unless coordination evidence exists.
10. Sector concentration under a null model: treat bill rosters as hyperedges, compute rich-club rho(k) against a hypergraph configuration null (Azaiez/Chodrow), and expand to many CRS policy-area universes before testing any centralization-to-passage link. LDA is used for actor identity only (D-021).
