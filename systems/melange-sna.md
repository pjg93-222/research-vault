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
paper_count: 33
tags: [system, type/system-hub]
---

# Melange member social-network analysis

Construction and measurement of member-to-member networks: the cosponsorship network (R Stage 3 and scraper weights), multilayer layers (cosponsor, press engagement, HCoI, social reposts and mentions), centrality and bipartisanship, co-appearance and signed reference layers, signed coalition partitioning, press-cluster coalitions, sector network configs and the Katz-Bonacich benchmark.

Brain notes: [topics/2026-06-feature-cosponsor-and-sector-networks](obsidian://open?vault=brain&file=topics%2F2026-06-feature-cosponsor-and-sector-networks); [topics/2026-06-feature-cosponsor-and-sector-networks-june-network-plan](obsidian://open?vault=brain&file=topics%2F2026-06-feature-cosponsor-and-sector-networks-june-network-plan); [topics/2026-07-feature-influence-engine-internals-salience-sna-wave](obsidian://open?vault=brain&file=topics%2F2026-07-feature-influence-engine-internals-salience-sna-wave); [actors/melange-dashboard-june-capstone](obsidian://open?vault=brain&file=actors%2Fmelange-dashboard-june-capstone)

**Melange member social-network analysis: evidence assessment (hub melange-sna, 56 evidence rows across 9 components)**

Status mix: 9 VERIFIED, 44 VERIFIED-SOFTENED and 3 UNVERIFIED (Steinert-Threlkeld 2018, Zhang 2022, Cao 2025). The UNVERIFIED rows are leads only (DE-062) and carry no weight below.

**Bottom line.** The literature backs the *mechanics* Paul adopted:
- Aref-Neal frustration objective
- Malaterre HCoI pipeline
- Fink-style empirical-rate edges
- Battaglini-Patacchini Katz-Bonacich (KB) logic

It does not validate the *specific parameters*, such as:
- 1.0/0.5 (or 0.2) role weights
- the x3 passage multiplier
- 0.8 per-Congress decay
- the 1/sqrt(C) bill-size discount
- the 25-post floor
- 0.6 / 0.12 HCoI caps
- 3x hub rule
- COALITION_LEAN_WEIGHT 3

It also does not validate the substrates Melange actually uses. Signed coalitions run on LLM-valenced media mentions pooled across chambers, while the source papers use per-chamber, per-Congress cosponsorship backbones. KB is computed on the cosponsor graph, which Battaglini-Patacchini 2016 deliberately exclude as endogenous.

**Evidence is mostly qualifying, not confirming.** Only 3 rows are 'supports':
- Azaiez: discounting large hyperedges, weak
- Ornstein: LLM reproducibility caching
- Aref 2020: partial balance rising with polarization, on a different substrate

**Code-vs-documentation contradictions recur across components.** These are the most actionable findings:
- The Fink denominator is reversed in the social layer.
- KB and signed-coalition outputs are documented as 'not consumed' but are read by the member desk, the audit and shadow-recompute.
- The Ward bloc count and labels differ from the component description.
- The regular-cosponsor weight is 0.5 in the R pipeline vs 0.2 in the holistic builder.
- The signed-coalition description names sdsm, but the code partitions the mention layer.

**Gap.** No component has a held-out predictive test on congressional data of the kind its own empirical question asks for (future co-voting, cosponsorship, recruitment or passage).

**Flags on rejected proposals.** Several papers rest on per-edge or degree-preserving volume normalization:
- the Aref SDSM null
- Tang reliability weights
- Ahmad Eq. 3
- Battaglini 2018 row-share
- Porter observed/expected

This project measured and REJECTED that kind of normalization. Each is FLAGGED and none is recommended. No paper in the set argues for dropping regular cosponsorship edges.

## Cosponsorship network construction

**What it computes.** Member-to-member weighted cosponsor graph: symmetric (mean of reverse duplicates) for cascades and directed (sponsor -> cosponsor) for recruitment.

**Method.** R Stage 3: original cosponsor 1.0, regular 0.5, summed per sponsor's bills; igraph centralities with 1/weight distances. Scraper weights: role weight x SUCCESS x 0.8^(119 - congress) / sqrt(C), passed-origin x3 (Sotoudeh-Porter-Krishnagopal). Five NET_CONFIGs (cu published, holistic, holistic_norm, bank_reg, crypto).

**Code refs.**
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/congress_member_scraper.R (Stage 3)`
- `fec/moneyball_core.py:371-542 (load_network, load_network_directed)`
- `fec/network/build_multilayer_network.py:9-19`

**Assumptions.**
- Cosponsorship is an influence and alignment tie
- Original cosponsorship is a stronger tie than plain cosponsorship
- Big bills are weaker ties (1/sqrt(C))

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | supports | Large hyperedges inflate attendees' centrality when projected to cliques, yet those large hyperedges are what connects the network (edge betweenness correlates 0.74 with size). | Supports discounting by hyperedge (bill) size, which is distinct from the REJECTED per-edge volume normalisation (sqrt(Out*In) or row-stochastic, STATE.md:895-896). It gives no basis for 1/sqrt(C) over log or 1/C. A bill roster is an asynchronous set of signatures with sponsor direction, not a simultaneous meeting. The data are EC lobbying meetings, not US House cosponsorship. | weak | VERIFIED-SOFTENED |
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | suggests-extension | Builds a signed network from the legislator-bill bipartite projection using SDSM (backbone R package 1.5.0). The null preserves the expected values of both degree sequences. Positive and negative edges are significant deviations at FWER alpha = 0.01 (Holm-Bonferroni). | A signed SDSM backbone could be added as a separate diagnostic layer feeding signed_coalitions. It should NOT replace or reweight kernel edges. SDSM's degree-preserving null acts like per-edge volume normalization, a proposal that was measured and REJECTED in this project, so FLAG and do not recommend it. The paper uses all co-sponsorships and does not argue for dropping regular cosponsorship edges. | moderate | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | qualifies | Relationships carry three attributes: content, direction and strength. A pair is tied by all the relationships it maintains. Strength can be carried as the value of a link, or considered within a presence-absence dichotomy by requiring a minimum frequency. Reported directed ties can be asymmetric, with the two sides reporting different amounts. The article does not say that thresholding discards strength. | The article gives design principles and no estimate. It does not address 2:1 role weights, the success multiplier, the decay factor or the sqrt size norm, and it does not show that symmetrizing changes diffusion. It is not evidence for keeping or dropping regular-cosponsor edges, so the rejected drop must not be defended from this paper. Asymmetric reports are a survey feature, not a graph-construction rule. | weak | VERIFIED-SOFTENED |
| [[sotoudeh-2024-network-based-measure]] | 2024 | qualifies | Influence accrues per legislator pair as a decayed ratio C_pass / C_tot of mutually cosponsored bills that eventually pass the House. Sponsor and cosponsor are treated identically, edges are undirected, decay is monthly with half-life 6, 12 or 24 months, and scores are normalized by party size (Eq. 1-4). Passage means House passage. The authors call the result only a proxy (p. 17). | The paper supports only the general idea of weighting ties by joint passage, for House bills 2009-2019 with House passage as outcome. It gives no evidence for the x3 multiplier, the 1/sqrt(C) discount, the 2:1 or 5:1 role ratio, the per-Congress decay, Senate ties or CU-topic bill universes. Read the citation as inspired by, not implements. | strong | VERIFIED-SOFTENED |
| [[porter-2005-network-analysis-committees]] | 2005 | suggests-extension | Porter et al. project a two-mode representative-by-committee assignment network onto committees, weighting each committee pair by observed shared members over the count expected under random same-size assignment (1 = chance). The structure result uses assignment data only. The political-correlation result uses roll-call votes, so it is not free of political information. | No committee co-membership layer exists, so any such layer would be new and unvalidated. Its observed/expected weighting must not be applied to cosponsor edges, which was measured and rejected (brain topic note, line 101, per-edge normalization REJECTED). The item's Paul-annotation entries link Porter to a committee layer, not to the cosponsor graph. | weak | VERIFIED-SOFTENED |
| [[ringe-2013-pinpointing-powerful-voting]] | 2013 | qualifies | Authors use measured staff-reported contacts rather than proxies such as cosponsorship, and argue this raises confidence in their measures. Reported contact was sparse: 6.1 percent density (118 of 1,920 possible directed ties in the asymmetric all-respondent subnetwork), mean point connectivity 2.2 (median 2), and 12.4 percent of dyads at zero connectivity. | The paper does not test cosponsorship, so it cannot show cosponsor edges are wrong. It shows measured contact and agreement can diverge for adversaries, which bears on the alignment assumption for cross-party cosponsor edges. It does not argue for dropping regular cosponsor edges or for per-edge volume normalization; both were measured and rejected in this project. | weak | VERIFIED-SOFTENED |
| [[aref-2020-detecting-coalitions-optimally]] | 2020 | qualifies | A tie is positive or negative according to whether joint cosponsorships are significantly many or few against an SDSM null that preserves each legislator's cosponsorship volume and each bill's cosponsor count. Raw co-cosponsorship counts are not used as tie strengths. The authors note inferred ties may reflect avoidance. | SDSM answers whether a tie is significant, not how strong it is. Because it conditions on member volume it works as a degree-corrected filter. The project's rejected per-edge volume normalization is the nearest analogue, so the paper does not argue for re-weighting the cascade graph. Any significance-filtered signed overlay would need its own validation. | weak | VERIFIED-SOFTENED |
| [[kamarthi-2020-influence-maximization-unknown]] | 2020 | qualifies | Geometric-DQN, trained per family and tested on held-out networks of that family, improves influence by 7.91 to 26.6 percent of the CHANGE-to-OPT gap (best variant per family, 13 test networks, IC p = 0.1, budget 10, T = 5 queries). Discovered graphs are almost always larger than CHANGE's, yet size does not correlate with influence across Geometric-DQN policies. | The paper discovery setting applies only if cosponsorship is an incomplete sample of a latent contact network. The paper numbers come from other networks under single-colour IC, so they cannot be carried over as system gains. | weak | VERIFIED-SOFTENED |
| [[battaglini-2018-effectiveness-connected-legislators]] | 2018 | qualifies | Cosponsorship links are endogenous. The control-function selection term xi = -0.4133 (SE 0.0741) is significant in the corrected NLLS, and correcting moves phi from 0.44 to 0.83. Alumni ties predict links with coefficient 0.0387 per dyad (Table 1 col 2, reported as 3.8658 multiplied by hundreds; 945,382 dyads), R-squared 0.079. | The paper's estimand is effectiveness spillover, not stance diffusion, so the size of the bias does not transfer; the sign that naive cosponsor coefficients are biased does. The paper's row-share weight is a per-member volume normalization this project rejected, so it is a description only and not a recommendation. | moderate | VERIFIED |

**Assessment.**

**Evidence weight: moderate on the general idea, nil on the parameters.** There are 9 rows: 1 VERIFIED (Battaglini 2018 endogeneity), 8 VERIFIED-SOFTENED, mostly 'qualifies'.

**Sotoudeh-Porter-Krishnagopal 2024 (strong, qualifies)** is the cited source. It supports only the broad idea of weighting pair ties by joint eventual passage, and only within its scope:
- House bills, 2009-2019
- House passage as the outcome
- undirected edges, sponsor and cosponsor treated alike
- monthly decay with 6/12/24-month half-lives
- party-size normalization

Melange's construction differs in kind:
- role weight x 3.0 if the last action matches a passage regex x 0.8^(119-congress) x 1/sqrt(C)
- summed per directed pair
- no party normalization

That is a volume-weighted success boost, not a pass/total ratio, so the citation reads 'inspired by', not 'implements'. The rows also record a last-action caveat: about 9.6% of ties sit on bills whose passage status may be misread, worth roughly 14.9% of unrolled tie weight. That rests on a code comment and has not been re-derived.

**Battaglini et al. 2018 (VERIFIED, moderate)** shows cosponsorship links are endogenous. The selection term is significant, and correcting for it moves phi from 0.44 to 0.83 in the House, 109th-113th Congresses. The size of the bias does not transfer to stance diffusion, but its sign does: naive cosponsor-based coefficients are biased.

**Azaiez 2025 (weak, supports)** backs discounting by hyperedge size: large meetings inflate clique-projected centrality yet carry connectivity. That is distinct from the rejected per-edge volume normalization. It gives no basis for 1/sqrt(C) over log or 1/C, and the data are EC lobby meetings.

**Ringe et al. 2013** (European Parliament staff contact) shows measured contact and agreement diverge between adversaries. This bears on the 'alignment tie' assumption for cross-party edges, but the paper never tests cosponsorship.

**Aref 2020/2021** build signed significance backbones (SDSM). These answer 'is the tie significant', not 'how strong is it'.

**Haythornthwaite 1996** and **Kamarthi 2020** are design or context only.

**Contradictions.**
- Regular-cosponsor weight: the component description and the R pipeline say 0.5. The holistic builder (build_holistic_base.py) uses 0.2, and one row found no 0.5 dial for the cu base graph. The documented 2:1 ratio is therefore not uniformly what runs.
- Sotoudeh is cited as the source of the edge weights, but its estimator (decayed pass/total ratio, undirected, party-normalized, House passage) is not what the code computes (x3 multiplicative boost, directed sum, no party normalization, origin-chamber passage regex).
- Assumption 'cosponsorship is an influence and alignment tie' vs Battaglini 2016/2018 and Ringe 2013. The links are endogenous to the outcomes they are used to explain, and contact can run against agreement between adversaries.

**Gaps.**
- No held-out test of the component's own empirical questions: do success-weighted ties predict future co-voting or joint sponsorship better than raw counts, and is 2:1 original-to-plain supported? Study needed: a prospective link-prediction comparison. Train on Congress t edges under alternative weightings (raw, role-only, role+success, +decay, +size). Score them on Congress t+1 cosponsorship and co-voting, per chamber, with the window and denominator named.
- No justification in any row for the x3 passage multiplier, the 0.8 per-Congress decay or 1/sqrt(C) against alternatives. Needs a sensitivity sweep with rank-delta reporting.
- No selection or endogeneity correction anywhere in the engine (Battaglini 2018). Needs an instrumented or control-function check, e.g. alumni or committee ties as exogenous predictors of cosponsor links, before any causal reading of cosponsor-network effects.
- Senate ties and CU or sector bill universes have no source evidence at all (Sotoudeh is House-only).

**Paul's position vs the evidence.**

Paul's position is to build on cosponsorship as his sources do, with weighted, directed, time-decayed edges.

The sources do build on cosponsorship. Directed and decayed weighting has a precedent: Sotoudeh uses time decay, and the sponsor-to-cosponsor direction is a reasonable design. Two qualifications apply. Sotoudeh itself is undirected, and the cu config mean-symmetrizes edges anyway, so 'directed' survives only in holistic_norm and the recruitment loader.

The evidence does not validate Paul's specific weights. It warns, via Battaglini 2016/2018, that cosponsorship is endogenous to the money and effectiveness outcomes Melange later correlates against it.

The Aref SDSM null and Battaglini's row-share weights are degree or volume normalizations of the rejected kind. FLAG them and do not recommend them. No paper here argues for dropping regular cosponsorship edges; Haythornthwaite must not be used to defend that rejected drop.

**Open questions.**
- Which regular-cosponsor weight (0.5 or 0.2) is canonical for each NET_CONFIG, and is the documentation to be corrected or the builder?
- Should the passage multiplier key on origin-chamber passage (current regex) or eventual House passage (Sotoudeh)? And how many ties move under the last-action caveat once re-derived?
- Is a signed SDSM overlay wanted strictly as a separate diagnostic layer, given that its null is in the rejected normalization family?

**Read first.** [[sotoudeh-2024-network-based-measure]], [[battaglini-2018-effectiveness-connected-legislators]], [[aref-2020-detecting-coalitions-optimally]], [[azaiez-2025-hypergraph-analysis-european]], [[ringe-2013-pinpointing-powerful-voting]]

## Bipartisanship (bpf)

**What it computes.** Per-member cross-party tie share from cosponsorship only (bipartisan.json / 100, default 0.24).

**Method.** Restored to cosponsor-only by S2-01 (bf4a703); BP_SRC=aug gives the unratified augmented definition.

**Code refs.**
- `fec/moneyball_core.py:68-72`
- `fec/build_dashboard_bundle.py`

**Assumptions.**
- Cosponsorship alone measures cross-party bridging

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | qualifies | The Senate bipartisan cosponsorship index fell about 40% over the last 25 years (1999-2023), and legislator modularity is higher than lobbyists' and rising for senators. Lobbyist cross-party collaboration stayed relatively stable and lobbyist modularity is near zero. | The paper's index is a chamber-level aggregate, not a member-level trait. It shows the cross-party baseline is strongly era-dependent, so a bpf or default fixed in one window should not be carried across Congresses without naming the window. It does not validate bpf as a measure of bridging. | weak | VERIFIED-SOFTENED |
| [[harbridge-yong-2023-bipartisan-path-effective]] | 2023 | qualifies | Within-member (legislator FE, one-tailed) coefficient on the attracted share, the per-bill mean of other-party cosponsor share: House 0.433 (SE 0.097) Model 1.2; Senate 0.330 (SE 0.147, p<0.05) Model 1.5. Offered share not significant with controls: House -0.234 (Model 1.3), Senate -0.420 (Model 1.6). Window 93rd-114th Congresses (1973-2016), about 9,000 House and 2,170 Senate legislator-Congress rows. | Paper variable is a mean over a member's own bills of other-party cosponsor share; bpf is a roster-snapshot weighted tie share with prior shrinkage, so window and denominator differ. Paper coefficients are by chamber, 1973-2016, one-tailed; quote by chamber only. Transfer to the current roster is UNVERIFIED until reproduced (DE-062). | strong | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: one strong but non-transferring source, one weak contextual source.** Both rows are VERIFIED-SOFTENED.

**Harbridge-Yong et al. 2023 (strong, qualifies)** is the key paper. Window: 93rd-114th Congresses (1973-2016), within-member fixed effects, one-tailed, about 9,000 House and 2,170 Senate legislator-Congress rows. Quote these by chamber only.

The *attracted* share predicts effectiveness:
- House 0.433 (SE 0.097)
- Senate 0.330 (SE 0.147)

The *offered* share does not, once controls are added:
- House -0.234
- Senate -0.420

bpf is a roster-snapshot tie share that mixes offered and attracted directions. Undirected pairs are averaged over both directions, and the share is shrunk with a 6-pseudo-count prior. So the direction the paper finds informative is diluted by the direction it finds uninformative. Transfer to the current roster is UNVERIFIED until reproduced.

**Bacik 2025 (weak)** shows the Senate bipartisan cosponsorship index fell about 40% from 1999 to 2023. That is a chamber-level aggregate. It implies any bpf value, or the 0.24 default, is era-bound and must name its window. It does not validate bpf as a bridging measure.

**Contradictions.**
- The assumption 'cosponsorship alone measures cross-party bridging' meets Harbridge-Yong. Direction matters: offered cross-party ties are not significant, and bpf pools the two directions.

**Gaps.**
- No member-level validation of bpf against the component's own question: does bpf predict cross-party cosponsor recruitment success? Study needed: compute an attracted-only share (other-party cosponsors on a member's own bills) alongside bpf. Test both against next-Congress cross-party recruitment and LES, per chamber, with window and denominator named.
- No evidence for the 0.24 default or the 6-pseudo-count prior. Needs a check of how many members fall to the default and how far ranks move under alternatives.

**Paul's position vs the evidence.**

Paul highlighted Bacik's findings that legislators polarized while lobbyists did not, and that bipartisan teams do better.

The Bacik evidence held here is chamber-level and era-bound. The 'bipartisan teams earn more' point is not among the verified rows. The stronger, verified support for 'bipartisanship pays' comes from Harbridge-Yong, which pins it to *attracted* cross-party support.

Paul's direction is therefore supported. The current bpf construction is not the measure the evidence supports.

**Open questions.**
- Should bpf be split into attracted and offered shares (directed rows already exist in the audit fields), or should an attracted-share variant be added as a shadow measure first?
- Which Congress window is bipartisan.json built on, and should the 0.24 default be window-specific?
- Is BP_SRC=aug still unratified, and is it out of scope for this assessment?

**Read first.** [[harbridge-yong-2023-bipartisan-path-effective]], [[bacik-2025-measuring-dynamical-evolution]]

## Social and press-engagement layers

**What it computes.** Directed edge probabilities from reposts (w 1.0) and mentions (0.4), Fink author-volume normalized (floor 25 posts, pair cap 0.25). Kernel-inverse weight w = 6p/(1-p), cap 2.0; reinforcement x(1 + 0.3 min(1, p/0.10)). Press-title engagement uses ENG_MIN_ITEMS 5.

**Method.** Precision-first name matching.

**Code refs.**
- `fec/network/social_interaction_layer.py:15-19`
- `fec/network/press_engagement_layer.py:6-11`
- `fec/network/build_multilayer_network.py`

**Assumptions.**
- Reposts and mentions indicate relational ties (Fink et al. 2023)

**Evidence.**

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

**Assessment.**

**Evidence weight: moderate on method, with one strong contradiction.** There are 8 rows: 1 VERIFIED (Fink, press layer), 6 VERIFIED-SOFTENED and 1 UNVERIFIED (Steinert-Threlkeld, lead only).

**Fink et al. 2023 (VERIFIED-SOFTENED, contradicts, strong).** Fink defines influence of i on j as j's interactions with i's tweets divided by *i's* tweet count, with a 100-tweet exclusion. The social layer instead divides by the volume of the *engaging* member (max(25, posts(a))), which is the opposite end of the dyad. It also:
- uses a 25-post floor
- folds replies and quotes into 0.4-weight mentions
- averages the two directions into a symmetric edge, discarding direction

The paper definition is verified. The code divergence is a code read and stays UNVERIFIED until social_interactions.json is recomputed both ways.

The press-engagement layer (VERIFIED) follows Fink's direction, but with a 5-item floor, far noisier than 100. Its docstring claim to 'validate cosponsor-edge probabilities' has no basis: Fink validates nothing.

**Baptista 2022 (moderate)** says more layers are not always better. A layer helped only where its overlap with the base was high, and randomized links erased the gain. No held-out layer-gain test exists in the repo.

**Zhou 2026 (weak)** finds active interaction edges add about 3 macro-F1 points for stance inference, not for influence. Its edges are directed, and reposts are not among its edge types.

**Hargittai 2015 (moderate)** warns of platform selection. Read-only Bluesky counts make this concrete: 392 of 411 Bluesky mentions are D-to-D, and 117 of 135 posters are Democrats. That party skew could carry into edge patterns. This is untested and UNVERIFIED.

**Ahmad (n.d.)** uses a per-retweeter volume normalization. It is descriptive, and it is FLAGGED as the rejected normalization family.

**Contradictions.**
- The social_interaction_layer.py denominator is the engaging member's post volume, the reverse of Fink's source-volume denominator, while its docstring claims Fink discipline (code read; UNVERIFIED pending recompute).
- The press_engagement_layer docstring says the layer validates cosponsor-edge probabilities. Fink provides no validation of any kind.
- One row says the engine never reads the Provenance sheet. Baptista's row finds leg_evidence_recompute.py and build_dashboard_bundle.py read it.
- Paul's critique asks for temporal weighting and sign. The layer has neither: it is unsigned, symmetric and window-pooled.

**Gaps.**
- No test of the component's empirical question: do interaction ties predict cosponsorship beyond party and state? Study needed: a held-out link-prediction test. Fit party, state and committee baselines, then add each layer (social, press-engagement, HCoI) and report its gain and its overlap with the cosponsor base, per chamber and per window (the Baptista design).
- No sensitivity check of the Fink-consistent denominator against the current one.
- No check of whether Bluesky's party-skewed posting biases cross-party edge density.

**Paul's position vs the evidence.**

Paul treats Fink as the pre-built framework, and the evidence supports that choice of framework. The implementation departs from Fink in its denominator, floor, event types and direction, so the layer is not Fink-consistent today.

Paul's own critiques (sign, third-party sentiment, temporal weighting) are not addressed in code. Zhou supports keeping direction.

Paul's Baptista highlight ('more layers are not always better') is exactly what the evidence says. The system has not acted on it: no layer is tested for held-out gain.

Flags: Ahmad's per-retweeter normalization and Tang-style count weights belong to the rejected per-edge volume-normalization family. Do not cite them as support for wider normalization.

**Open questions.**
- Was the reverse denominator a deliberate choice (engagement propensity of the actor) or a slip? If deliberate, the Fink attribution should be reworded.
- Should a directed variant of the social layer be kept for recruitment and path uses?
- Is the 5-item press floor to be justified, or relabelled as a noisy rate?

**Read first.** [[fink-2023-congressional-social-network-dataset]], [[baptista-2022-universal-multilayer-network]], [[zhou-2026-measuring-stance-dynamics]], [[hargittai-2015-bigger-always-better]], [[ahmad-nd-shortest-path-algorithms]]

## Press HCoI communities

**What it computes.** Eight Louvain communities over recency-weighted press-category profiles (Pearson >= 0.6, seed 7); topic-affinity edges <= 0.12 for cosponsor-isolated members.

**Method.** Hellinger genealogy across time. Homophily replication at press resolution was null.

**Code refs.**
- `fec/press_hcoi.py`

**Assumptions.**
- Shared issue attention indicates coalition (Malaterre & Lareau 2024)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[zareie-2020-similarity-based-link]] | 2020 | qualifies | Indirect similarity (Pearson correlation of neighbourhood vectors) is combined multiplicatively with direct common neighbours as (1 + CN)(1 + Corr), and the paper never evaluates the correlation term alone. In Exp. 4 (pairs with equal common-neighbour counts), DICN is clearly worst on UAL (0.5684 vs SHOPI 0.7921); the paper gives no structural reason. The authors say small training sets make the latent relationship harder to detect. | The analogy is loose. HCoI correlates topic-attention profiles, not neighbourhood vectors. The transferable point is a design point: in DICN the profile correlation is one factor, anchored on direct shared ties. Placing cosponsor-isolated members (D4) by correlation alone is loosely analogous to Exp. 3 (no common neighbours), but that is still a heuristic placement, not evidence of a relationship (D-025). | weak | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | qualifies | Block modelling finds structurally equivalent actors by correlating all pairs of cases and then clustering the correlations. Structural equivalence means identical ties to and from all other actors. Structure should come from observed relationships rather than attribute classes. The article does not name tie-matrix rows as the profile that is correlated. | The procedure resembles block modelling but correlates attributes rather than relationships, so under the article's definitions HCoI is attribute similarity. That supports keeping it as a weak, surgical layer. The article says nothing on whether attention similarity predicts collaboration. | weak | VERIFIED-SOFTENED |
| [[zhang-2022-data-assisted-social]] | 2022 | context | Builds a term co-occurrence graph from cross-correlation of burst vectors (edge threshold 0.1) and partitions it with Louvain to find event communities; no modularity, stability or external-validity figure is reported. | The pipeline shape is the same (correlation, threshold, Louvain), but the nodes are terms, not legislators, and the threshold (0.1 vs 0.6) is unjustified and unvalidated. That gives no grounds to retune HCOI_CORR or to read the null homophily result differently. Malaterre remains the governing source. | weak | UNVERIFIED |
| [[malaterre-2024-inferring-social-networks]] | 2024 | method-source | LDA k=25 on 16,917 articles; author topic profiles per roughly 21-year period, weighted by authorship share; keep authors with weighted publications above 2; Pearson r at least 0.6; Louvain (Gephi default); community profile = average of member profiles; genealogy by closest Hellinger pairing between adjacent periods; modularity 0.427 to 0.834 and communities 4 to 13. | Mechanics carry over. The evidence does not: the paper covers an 88-year academic corpus in four periods with over 80 percent new authors per period and only qualitative validation. Here the split is two half-periods of one press stream, so NOVEL and PERSISTS labels reflect the script's own thresholds. Quote member denominators as 155 profiled, not the 385 in the script docstring, which is stale. | strong | VERIFIED-SOFTENED |
| [[malaterre-2024-inferring-social-networks]] | 2024 | qualifies | HCoIs are about who-talks-about-the-same-things-as-who. Links are mediated by texts, are not social ties, and serve as a heuristic for possible real ties. (The analyst gloss that communities are not labelled from alignment is not stated in the paper.) | The attention-not-alignment limit is definitional and transfers fully. Reading an HCoI as a coalition needs a signed profile (attention times stance), which neither the paper nor press_hcoi.py builds. | strong | VERIFIED-SOFTENED |
| [[malaterre-2024-inferring-social-networks]] | 2024 | qualifies | The paper offers no evidence that text-similarity ties carry influence or transmission; it frames HCoI mapping as a heuristic for actual social connections that would still need confirmation. | Using HCoI edges as a low-weight reachability patch fits the paper's heuristic framing. Treating them as transmission edges in cascade, coalition or route outputs goes beyond what the paper tested, and any output depending on them must say it rests on a topical-similarity heuristic. The 2026-08-18 ruling is cited only in code comments and was not checked. | moderate | VERIFIED-SOFTENED |
| [[ardekani-2026-computational-graph-theoretic]] | 2026 | suggests-extension | Louvain on the LDA-based Act graph gives 14 communities at modularity 0.393 (two-layer) and 15 at 0.392 (three-layer). Modularity above 0.3 is treated as meaningful. Community-level Jaccard alignment with committee keyword sets runs 0.10 to 0.66. Agglomerative merging into 6 macro-communities raises the Maori Affairs match to 0.67. | Adding a modularity Q and an external alignment check, such as Jaccard against committee rosters or cosponsor communities, is cheap and transferable. The paper's Q and Jaccard values come from Act citation graphs and are not benchmarks for member press graphs. The weak committee alignment is a caution that topical communities need not match institutions. The homophily null result is cited from project memory and was not checked here. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: strong on method provenance, strong on the limits of interpretation, nil on validity.** There are 7 rows: 6 VERIFIED-SOFTENED and 1 UNVERIFIED (Zhang 2022, lead only).

**Malaterre & Lareau 2024 (strong)** is a faithful method source. press_hcoi.py reproduces the same pipeline: profile, Pearson >= 0.6, Louvain, Hellinger genealogy. The paper's evidence does not transfer, for three reasons:
- It covers an 88-year academic corpus with qualitative validation only.
- Here the genealogy is a single median-date split of one press stream, so the PERSISTS and NOVEL labels reflect the script's own 0.25/0.45 thresholds.
- The paper says plainly that HCoI ties are text-mediated 'who-talks-about-the-same-things' links. They are a heuristic for possible ties, not social or influence ties.

profiles() never reads stance, so a community can contain both sides of an issue. Reading an HCoI as a coalition is not supported.

**Haythornthwaite 1996** classifies this as attribute similarity, not structural equivalence.

**Zareie 2020** notes that in DICN profile correlation is one factor anchored on direct ties. Placing cosponsor-isolated members by correlation alone is heuristic placement.

**Ardekani 2026 (moderate, suggests-extension)** recommends reporting modularity Q and an external alignment check. press_hcoi.py computes neither.

Stored artifact: 155 members profiled, 6 communities, 1,900 edges, 20 categories. The D4 block shows 14 isolated members, 0 of them placed in a community. Under the CU config this means the HCoI reconnection patch currently reconnects no one; that is a single read and UNVERIFIED.

**Contradictions.**
- The component assumption 'shared issue attention indicates coalition' conflicts with the paper's own definition: attention, not alignment, and a heuristic, not a tie.
- Category count: the component text and some rows say 19, while the Malaterre method row reads 20 in code and the artifact. The member denominator in the script docstring (385) is stale against the 155 profiled.
- The holistic dominance rule admits HCoI edges generally, while the component says only cosponsor-isolated members. That restriction holds only for the CU config.
- D4 placement: the stored artifact shows 0 of 14 isolated members placed, against a recorded 4-of-41 reconnection count that was not reproduced.

**Gaps.**
- No test of the empirical question: do press-attention communities predict cosponsorship within community above chance? Study needed: within- vs between-community cosponsorship rates against a degree-preserving or party-and-state permutation null, per chamber and per window. The homophily null already recorded at press resolution suggests the answer may be no.
- No modularity Q, seed-stability check (Louvain seed 7 only) or external alignment (committee rosters or cosponsor communities).
- No multi-period genealogy with more than one split, which Paul's 'temporal element' needs.

**Paul's position vs the evidence.**

Paul tags Malaterre Very High and Foundational, and wants office closeness inferred from text with a temporal element, using Louvain.

The method faithfully follows Malaterre, so the provenance is sound. The paper does not license reading HCoIs as office closeness: it says the links are not social ties. Under D-025 any 'closeness' wording is barred anyway.

The built component's null homophily result is consistent with the paper's caution, not a contradiction of it. The temporal element exists only as a two-period split.

**Open questions.**
- Should HCoI edges remain in holistic cascades and routes at all, given the paper frames them as a heuristic? Outputs that depend on them would need a topical-similarity label.
- Should a signed profile (attention x stance) be built if a coalition reading is wanted?
- Is the 0-of-14 D4 placement a regression or expected under current thresholds?

**Read first.** [[malaterre-2024-inferring-social-networks]], [[ardekani-2026-computational-graph-theoretic]], [[haythornthwaite-1996-social-network-analysis]], [[zareie-2020-similarity-based-link]]

## Co-appearance and signed member-reference layers (display only)

**What it computes.** Pairs who both appeared substantively in the same interview episode (>= 4 turns or >= 200 words; 1,151 pairs). Signed directed references with LLM valence per context window (4,358 edges; green above +0.15, red below -0.15).

**Method.** Episodes with more than 8 members are skipped. Edges need at least 2 scored contexts. Name resolution never guesses.

**Code refs.**
- `fec/co_appearance_layer.py`
- `member_reference_layer.py`

**Assumptions.**
- Appearing in the same episode indicates a relation
- LLM valence of references measures relation sign

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[zareie-2020-similarity-based-link]] | 2020 | context | The paper defines link prediction on plain unlabelled edges (E is a set of links, U = V x V) and has no treatment of negative, signed or valenced ties. It lists directed and weighted networks as future work. | There is no transfer to the signed layer, because the paper offers nothing on negative ties or valence. At most it could score unsigned co-appearance pairs for missing-link candidates. | weak | VERIFIED-SOFTENED |
| [[leskovec-2010-predicting-positive-negative]] | 2010 | method-source | Epinions balanced-sample accuracy about 93.4 percent (6.58 percent error, All23 model, minimum embeddedness 25, 10-fold CV). Slashdot and Wikipedia accuracies appear only in plotted Figure 1, not in the text. Balanced AUC about 0.90 and full-data AUC about 0.95 (text). Wikipedia self-trained accuracy 0.8021 (Table 6). Triad features add little at low embeddedness. | Needs an embeddedness census first: about 7,153 signed edges over 520 nodes, and many dyads may have few common neighbours, where triad features carry little information. Predictability measures consistency, not validity of the labels, and does not replace gold-label review. State the base rate and denominator with any reported accuracy. | moderate | VERIFIED-SOFTENED |
| [[leskovec-2010-predicting-positive-negative]] | 2010 | qualifies | In the undirected 4-type model, learned coefficients match balance theory for the three non-mm triad types in all three datasets. The enemy-of-enemy (mm) coefficient is negative in all three (-0.0149 Epinions, -0.0262 Slashdot, -0.0465 Wikipedia), disagreeing with balance. The authors say this is consistent with Davis weak balance, which makes no prediction for mm. | The pilot pattern is directed (a and b both point negatively at c). The paper's evidence is for the undirected collapse only; the Table 2 directed rows are hard to read in the text extraction, so no directed-level sign is claimed. The paper shows the mm pattern is not balance-like; it does not show the pattern carries no sign. | moderate | VERIFIED-SOFTENED |
| [[tang-2015-negative-link-prediction]] | 2015 | qualifies | Pairs with negative interactions hold negative links far above chance: t-test p = 5.72e-89 (Epinions) and 1.93e-109 (Slashdot). The rule negIn reaches precision 0.2097 and F1 0.2826 on Epinions, and precision 0.1483 and F1 0.1986 on Slashdot. The ratio of negative-link pairs tends to increase with the number of negative interactions K. Reliability weight f(Nij) = 1 - 1/log(1+Nij) applies to pairs with negative interactions; other pairs get a constant r. | Tang checks interaction-derived signs against declared negative links, which Congress lacks, so the transfer concerns interpretation only, not a calibrated precision. Tang's interaction labels are thresholded helpfulness ratings and MPQA lexicon polarity, a cruder reader than an LLM. Whether LLM-valenced precision is higher or lower is untested. Tang's count-based reliability weighting is a per-edge volume weight of the kind this project rejected, so it is flagged here, not recommended. | moderate | VERIFIED-SOFTENED |
| [[ringe-2013-pinpointing-powerful-voting]] | 2013 | qualifies | Unsigned contact is not alignment. Between adversaries more contact goes with less agreement (about -10 percent odds of co-voting per connectivity unit, opposed-bloc dyads, EP Environment Committee 2004-2008). Authors identify ally or adversary status from voting bloc and party membership, not from the contact itself (p. 617). | Supports keeping the reference layer signed and warns against reading cross-party co-appearance as closeness or alignment (D-025). A broadcast pairing of opponents may be adversarial. The settings differ (EP staff contact vs US media co-appearance), so the evidence is weak. | weak | VERIFIED-SOFTENED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | supports | Results from closed, vendor-served models can become impossible to reproduce. The GPT-3 models behind the paper results were removed from the public-facing API in January 2024. | Caching keeps outputs auditable after a model is retired, but does not regenerate them; a re-score on a successor model is a new instrument. The warning applies because both layers call Anthropic-served models. The component label is a weak fit for LLM caching and should name the member-reference and persona LLM layers. | moderate | VERIFIED |

**Assessment.**

**Evidence weight: moderate for interpretation and reproducibility. No validity evidence for LLM valence as a relation sign.** There are 6 rows: 1 VERIFIED (Ornstein), 5 VERIFIED-SOFTENED.

**Tang 2015 (moderate)** shows interaction-derived negative signals do track declared negative links far above chance, with precision rising with the count of negative interactions. Absolute precision is low (0.21 Epinions, 0.15 Slashdot). Congress has no declared negative links to calibrate against, so only the interpretation transfers.

Read-only counts from member_references.json (UNVERIFIED, single path):
- 7,944 edges in total
- 1,161 with net below 0
- 567 of those negative edges (about 49%) rest on a single scored context

net = (pos-neg)/(pos+neg+neu), so one negative context gives net -1.0. The dashboard filters to n_scored >= 2.

**Leskovec 2010 (moderate)**
- Leave-one-out sign prediction could serve as a consistency check, not a validity check. It needs an embeddedness census first, and accuracy must be reported against the positive base rate.
- The enemy-of-enemy (mm) coefficient is negative in all three datasets. It is not balance-like, but it is not sign-free either, which bears on the pilot's shared-negative stratum.

**Ringe 2013 (weak)** supports keeping references signed. Cross-party co-appearance must not be read as closeness or alignment: a broadcast pairing of opponents may be adversarial.

**Ornstein 2024 (VERIFIED)** shows vendor model retirement breaks reproducibility. The per-row model caching is correct practice, but a re-score on a successor model is a new instrument.

**Zareie 2020** has nothing on signed ties. Its citation for this component is a weak fit.

**Contradictions.**
- Citation fit: Zareie & Sakellariou 2020 is cited for this component, but the paper has no treatment of negative, signed or valenced ties.
- Display vs analysis: the dashboard requires n_scored >= 2, while signed_coalitions.py consumes the same edges with no floor. About half the negative edges (single-read count) enter the partition at full weight from one context.

**Gaps.**
- No gold-label validation of LLM valence as relation sign. Study needed: a hand-coded sample of reference contexts stratified by party pair and n_scored, with agreement statistics, before any sign is read as a relation.
- No test of the empirical question: do negative references predict lower future cosponsorship? Study needed: compare next-Congress cosponsorship between negatively and positively referenced pairs, conditioning on party pair and prior cosponsorship, per chamber.
- No structural consistency check (Leskovec leave-one-out) and no embeddedness census.

**Paul's position vs the evidence.**

Paul's question about negative ties is answered *operationally* by LLM valence per context window.

The evidence (Tang, Leskovec) says interaction-derived signs carry real but noisy signal. It also says predictability measures consistency, not label validity.

No verified row validates LLM valence as a relationship sign. The layer is display-only, so it should stay that way until a gold check exists.

Tang's count-based reliability weight is a per-edge volume weight of the rejected kind: FLAGGED, not recommended.

**Open questions.**
- Is an n_scored floor for the signed partition wanted? The evidence rows treat such a count floor as in the rejected per-edge volume family, so it needs Paul's explicit ruling and its own sensitivity run.
- Which model version scored the current member_references cache, and would a re-score be treated as a new instrument?

**Read first.** [[tang-2015-negative-link-prediction]], [[leskovec-2010-predicting-positive-negative]], [[ringe-2013-pinpointing-powerful-voting]], [[ornstein-2024-train-your-stochastic]]

## Signed-network coalition partition

**What it computes.** Coalitions from a signed backbone with frustration-minimizing local search seeded by positive Louvain.

**Method.** R backbone sdsm (alpha .05, signed). DW-NOMINATE vote edges were rejected in favour of the signed backbone.

**Code refs.**
- `fec/network/signed_coalitions.py`

**Assumptions.**
- Signed-network partitioning reveals hidden coalitions (Aref & Neal)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | suggests-extension | Parties in Congress are, for the most part, spatially adjacent ideological coalitions (p.17, citing Krehbiel 1993), and party overlap is near zero (p.15). The paper does not discuss partitions of networks or cutting-line members. | Benchmark only: checking hidden-coalition candidates against /dim1/ would show whether the mention layer adds information beyond roll-call ideology. Does not reinstate DW-NOMINATE vote edges, which Paul rejected (brain cosponsor-and-sector-networks topic, line 93). | weak | VERIFIED-SOFTENED |
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | method-source | The k-partition objective minimizes frustrated edges: negative edges inside a cluster plus positive edges between clusters (Eq. 1, giving C_k(G)). Eq. 2 finds the smallest k that reaches the minimum over all partitions (k*min). Both are solved to global optimality on 19 House networks (n ~445, 4,954 <= m <= 31,936 edges). Eq. 1 averages 3.3 h and Eq. 2 averages 14 h. | The objective transfers directly. The guarantee does not, because local search can stop in a local optimum. At 520 nodes and 7,153 edges the graph has fewer edges than the paper's largest network but slightly more nodes than the paper's n ~445, and the ILP's variable count grows as O(n^2). An exact Eq. 1 run is plausibly feasible but has not been demonstrated at this node count. | strong | VERIFIED-SOFTENED |
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | qualifies | On signed co-sponsorship backbones, House-only and per session, C_party > C_2 > C_3 holds in every one of the 19 sessions. A distinct third coalition of 4 members (113th, 2013) to 69 members (111th, 2009) appears. Negative ties are plentiful: members of the traditional coalitions have 2.68 negative edges per positive edge. | The evidence does not carry over. Mention valence measures expressed sentiment, while SDSM signs measure collaboration and avoidance against a degree-preserving null. The system's graph also mixes chambers and pools across Congresses, where the paper is House-only and per session. The system's null third-coalition result says nothing for or against the paper. A test needs a House-only, per-Congress signed co-sponsorship backbone. | strong | VERIFIED |
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | qualifies | The third coalition has a distinctive signed-tie pattern: 21.18 negative edges per positive edge, against 2.68 for traditional-coalition members, and 25.6% of its negative edges go to co-partisans, against 8.44%. Its members are the most effective coalition in most sessions. | The paper's ties come from co-sponsorship, not roll-call votes, so this is not a like-for-like contradiction. The relation is 'qualifies', not 'contradicts'. The paper shows that same-party negative ties distinguish the effective third coalition in co-sponsorship backbones. Whether the dropped vote-backbone negatives carry the same structure is untested. | moderate | VERIFIED-SOFTENED |
| [[leskovec-2010-predicting-positive-negative]] | 2010 | qualifies | Global balance is not significant. Best two-set partitions satisfy 0.8344 / 0.8105 / 0.7809 of edges (Epinions / Slashdot / Wikipedia), near the trivial all-positive value (about 0.80) and below degree-preserving rewired networks (0.8993 / 0.8310 / 0.8316). Sign-permuted baselines: 0.8562 / 0.7779 / 0.7866. | Paper signs are actor-declared on web platforms; ours are LLM-valenced speech mentions weighted by /net/. Only the comparison logic transfers. Any null for the Melange layer must preserve weights as well as degrees. | strong | VERIFIED |
| [[leskovec-2010-predicting-positive-negative]] | 2010 | suggests-extension | Global status ordering: 0.7905 / 0.8221 / 0.8538 of edges respect the best ordering, versus 0.7241 / 0.7568 / 0.7767 under sign permutation and 0.6377 / 0.6644 / 0.6321 under rewiring (random 0.50). Directed triad: a positive v-to-w-to-u path is evidence for a negative u-to-v edge, BB coefficients -0.0692 / -0.0293 / -0.0259 in the all-positive subgraph model. | Only meaningful if a negative public mention is an asymmetric judgement by the speaker about the target. Any ordering is a heuristic model output, never an influence, access or closeness claim (D-025). Dyads with edges in both directions are set aside by the paper and are not reciprocity evidence (D-104..D-108). | moderate | VERIFIED |
| [[tang-2015-negative-link-prediction]] | 2015 | qualifies | Balanced triads are 92.31 percent (Epinions) and 93.01 percent (Slashdot) of undirected triads (Sec. 3.2 p. 89). Setting negative-sample reliability weights to 1 (cj = 1) cuts precision by 8.70 percent on Epinions (Table 4) and 11.08 percent on Slashdot (Table 5). The balance regularizer raises precision from Cb = 0 to 0.001 (+4.02 and +4.41 percent), is relatively stable from 0.001 to 0.1 on Epinions, and drops sharply after 0.05 on Slashdot (Sec. 5.4, Fig. 5). Large Cb hurts. The paper does not say the regularizer helps only at small weight. | Tang's balance figures come from declared trust networks. The system's signs are interaction-derived, which is the paper's noisy-label case, and the paper does not estimate how much frustration comes from noise. Tang's count-based reweighting is the rejected per-edge volume kind. The n_scored >= 2 floor the summary proposes is a count threshold of the same family, so it is flagged, not recommended on this paper's authority. Any sensitivity run needs its own project approval. | moderate | VERIFIED-SOFTENED |
| [[ringe-nd-pinpointing-powerful-voting]] |  | qualifies | Signed (traditional) co-voting centrality cannot separate the cue-provider from majoritarians; with a third group following at 35 percent it makes majoritarians systematically more central than the actual cue-provider. The absolute-value measure corrects this. In simulation, cue-provider centrality is lowest when both follow probabilities are 50 percent and highest as they approach 0 or 1. | Paper is single-party cue-taking with simulated independent follow probabilities plus European Parliament MEP data, not US House or party-aligned backbones. The paper gives no account of why signed backbone edges cluster on party, so the majoritarian explanation is the analyst hypothesis and stays UNVERIFIED for US data and the 117th-119th Congresses until a second path reproduces it (DE-062). That dropping negative ties loses cue-provider information is an inference from the paper's use of negative correlations, not a paper critique of Melange. | moderate | VERIFIED-SOFTENED |
| [[aref-2020-detecting-coalitions-optimally]] | 2020 | method-source | The exact two-group frustration index comes from a binary program (Eq. 8: n+m binary variables, 2m+1 constraints) warm-started from party labels and bounded below by an LP relaxation with triangle constraints (Eq. 7). Heuristic partitioning is 'not guaranteed to provide the optimal solution'. Senate instances average 4 s (SD 6 s). The 113th House (m = 75,771) takes 48,823 s: 5,300 s for the LP (28,486,271 constraints) and 43,523 s for the binary solve. | The paper says the LP bound is not theoretically required, so Eq. 8 alone is exact. The LP is not the cost driver in the paper's own instance (5,300 s vs 43,523 s). The rel figure of about 23M triangle constraints at n=520 is C(520,3), the complete-graph triangle count. A read-only recount of the live mention graph on the 520-node set gives about 23.5k triangles, so Eq. 7 has about 100k rows (m+4T), not 23M. Solve times remain UNVERIFIED (DE-062). | strong | VERIFIED-SOFTENED |
| [[aref-2020-detecting-coalitions-optimally]] | 2020 | qualifies | Edges come from a significance-tested SDSM cosponsorship backbone (two-tailed alpha 0.05), built per chamber and per Congress. The near-party partition and the coalition-partisanship effect hold for the House and not the Senate. The paper reports F(G)=1-2L/m so networks can be compared. | The paper's findings rest on cosponsorship backbones within one chamber and session. A pooled mention partition crosses the House/Senate fault line, where the paper finds a House effect and none in the Senate, so any coalition reading needs an UNSTRATIFIED label. The splinter is a heuristic partition output, not measured antagonism. | strong | VERIFIED-SOFTENED |
| [[aref-2020-detecting-coalitions-optimally]] | 2020 | supports | Both chambers show high partial balance that rose over 1979-2016, which the paper reads as rising polarization that largely mirrors party lines. Triangle and normalized frustration indices correlate at 0.95 (House) and 0.91 (Senate). | Different substrate (roll-call agreement, not cosponsorship) and different era (2021-2026, not 1979-2016). The paper gives no numeric party-alignment statistic for its optimal partition; the party link is qualitative. The paper does not discuss the project's vote backbone at all. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: strong for the objective, strong *against* transferring the source findings to the substrate in use.** This is the best-evidenced component: 11 rows, 3 VERIFIED.

**What transfers from Aref & Neal 2020/2021 (method-source, strong).** The k-partition frustration objective transfers directly.

**What does not transfer:**
- *Optimality.* Melange runs single-node local search with 60 restarts for k=1..3. It reports upper bounds only, never searches k*min, and its local search 'is not guaranteed' optimal.
- *Substrate.* Aref's ties are SDSM cosponsorship backbones, per chamber and per Congress. Melange partitions member_references.json: LLM-valenced media mentions, House and Senate pooled, Congresses pooled.
- *Chamber.* Aref 2020 finds the near-party partition and the coalition-partisanship effect in the House, not the Senate. A pooled partition crosses that fault line, so any coalition reading must be labelled UNSTRATIFIED.

**Stored artifact (2026-07-19):** 520 nodes, 7,153 edges.
- F(1) = 1,092.31 of 6,799.18 total weight (16.1%)
- F(2) <= 15.31%
- k=2 is a 506-member bloc plus a 14-member splinter, not a bipolar party split, so party_agreement_k2 is null.

The 14-member splinter is a heuristic partition output, not measured antagonism.

**Leskovec 2010 (VERIFIED, strong).** Best two-set partitions on real signed networks sit near the all-positive trivial value and *below* rewired nulls. Global balance was not significant. Melange has no rewired or sign-permuted null, so the 0.76-point F(1)-to-F(2) gap cannot be judged. Any null must preserve weights as well as degrees.

**Exact solve looks feasible.** Aref 2020's LP bound is optional. A recount gives about 23.5k triangles, not the 23M claimed elsewhere. Solve times remain UNVERIFIED.

**Ringe (n.d.) and Aref 2021 (moderate)** suggest that negative ties carry information. In Aref, same-party negatives distinguish the effective third coalition; in Ringe, they carry cue-provider information. The June vote backbone dropped its negative side (Option C). Whether that loses comparable structure is untested.

**Poole 1999** supports a DW-NOMINATE |dim1| check as a benchmark only. It does not reinstate the rejected NOMINATE vote edges.

**Contradictions.**
- The component description gives the method as 'R backbone sdsm (alpha .05, signed)'. The rows show signed_coalitions.py partitions the LLM-valenced mention layer; the sdsm vote backbone fed a separate Vote Affinity sheet. Description and code disagree on the substrate.
- The docstring says 'not consumed by the chain'. make_member_desk.py reads splinter_k2 and sets an 'antag' flag, and the shadow-recompute and dashboard_integrity scripts read the output. It reaches display and audit surfaces.
- Aref 2021 finds a distinct third coalition in all 19 House sessions. Melange's k=3 gives 506/13/1. This is not a contradiction of the paper, because the substrates differ, but it must not be read as a replication failure either.
- The assumption that 'signed partitioning reveals hidden coalitions' meets Leskovec: two-set partitions can sit below a rewired null, so a partition alone does not show structure.

**Gaps.**
- No null model (weighted, degree-preserving rewire or sign permutation) for F(k).
- No House-only, per-Congress signed cosponsorship backbone, which is the substrate on which the source findings hold. That is the test needed before any 'hidden coalition' claim.
- No test of the empirical question: do signed-partition coalitions predict joint voting on CU roll calls better than party? Study needed: a per-chamber comparison of within-coalition co-voting on held-out CU roll calls against a party-only baseline.
- No temporal tracking (Paul's stated gap) and no path-finding over the partition.

**Paul's position vs the evidence.**

Paul values Aref & Neal as a computationally efficient alternative layer. He names two gaps: no path-finding and no tracking over time.

The evidence supports the objective and suggests an exact solve is plausibly feasible at this size. But what runs is a heuristic, not Aref's optimal method, on a substrate Aref never used.

Paul's two gaps are real and remain open. A larger gap he did not list is the substrate and chamber mismatch.

The SDSM degree-preserving null is in the rejected per-edge volume-normalization family: FLAG it, do not recommend it as a reweighting of kernel edges. Any SDSM backbone stays a separate diagnostic layer.

**Open questions.**
- Should the member-desk 'antag' flag stay live while the partition is pooled across chambers and has no null?
- Should a House-only, per-Congress SDSM cosponsorship backbone be built as a diagnostic substrate for the partition?
- Should the vote-backbone negative side, dropped under Option C, be re-examined in light of Aref 2021 and Ringe?

**Read first.** [[aref-2020-detecting-coalitions-optimally]], [[aref-2021-identifying-hidden-coalitions]], [[leskovec-2010-predicting-positive-negative]], [[tang-2015-negative-link-prediction]], [[ringe-nd-pinpointing-powerful-voting]], [[poole-1999-nominate-after-years]]

## Press-lean coalition detection (Ward) and bloc profiles

**What it computes.** Ward.D2 clusters on member lean profiles (9 blocs) and a combined bloc index.

**Method.** COALITION_LEAN_WEIGHT 3.0, min 6 members. Static snapshot. All blocs are labelled pro-CU (a known mislabel).

**Code refs.**
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R (detect_coalitions)`

**Assumptions.**
- Similar press lean profiles form coalitions

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[pritoni-2025-lobbying-coalitions-determinants]] | 2025 | qualifies | Following Junk (2020b), the review separates three definitions of a lobbying coalition: policy camp (shared preferences only; 12/80 articles), behavioral coordination ('a non-trivial degree of coordinated activity over time'; 59/80) and formal organization (6/80). For the 16-article meta-analysis the authors keep only studies whose unit of analysis is the individual interest group or lobbyist, and they drop policy-camp studies (selection criteria i-ii). | The paper supplies a definition only. The group-or-lobbyist unit is a selection rule for the meta-analysis subsample, not a finding about the whole literature, and its determinants (weakness, brokers) concern groups choosing allies. They should not be read onto legislator blocs. If the blocs keep the name 'coalition', the label should say they are shared-position clusters with no coordination evidence. | moderate | VERIFIED-SOFTENED |
| [[malaterre-2024-inferring-social-networks]] | 2024 | suggests-extension | Communities in adjacent periods are matched by closest Hellinger distance to trace persistence, splits and novel communities; four novel communities appear in 1974-95 with no clear filiation. | The genealogy step would answer the stability question only if blocs were re-run per period and matched by Hellinger distance. Unlike HCoIs these blocs include stance lean, so a genealogy here would track alignment. The paper's evidence for the step is qualitative, so this stays a weak bearing. | weak | VERIFIED-SOFTENED |
| [[cao-2025-social-network-group]] | 2025 | qualifies | Pseudoconsensus: clustering raises homogeneity within subgroups but also heterogeneity between them, so high internal consensus need not mean the subgroups agree with one another. | The point is logical, not empirical, so it carries over to any clustering. Agreement within a bloc says nothing about alignment across blocs or with CU positions. A bloc label needs a between-bloc check of direction against an external criterion such as votes. No numbers transfer. | weak | UNVERIFIED |

**Assessment.**

**Evidence weight: weak, definitional only.** There are 3 rows: 2 VERIFIED-SOFTENED and 1 UNVERIFIED (Cao 2025, lead only).

**Pritoni 2025 (moderate)** separates three definitions of a coalition: policy camp (shared preferences), behavioural coordination and formal organization. Ward clusters of press leans observe no coordination. In Junk's terms they are at most a legislator *policy camp*. If the 'coalition' label is kept, it should read 'shared-position clusters, no coordination evidence'. Pritoni's determinants (weakness, brokers) concern interest groups choosing allies. They should not be read onto legislator blocs.

**Malaterre 2024 (weak)** offers Hellinger genealogy as the way to answer the component's stability question, if blocs are re-run per period. Because these blocs include stance lean, a genealogy would track alignment, unlike HCoI.

**Cao 2025 (UNVERIFIED lead)** raises pseudoconsensus: within-bloc homogeneity says nothing about between-bloc direction.

**Code read (Malaterre row).** k = max(2, min(8, round(sqrt(n/2)))) with no k passed, so there are at most 8 blocs, not 9. Labels are pro-CU / hostile / mixed by mean lean (+/-0.1). This does not match the component's 'all blocs labelled pro-CU' claim.

**Contradictions.**
- The component says 9 blocs, all labelled pro-CU. The verified code read finds at most 8 blocs with three-way lean labels, so the 'known mislabel' is not supported by the current code. The Cao row repeats the 9 and pro-CU claim but is UNVERIFIED.
- Calling the blocs 'coalitions' conflicts with Pritoni's definitions: no coordination is observed.

**Gaps.**
- No test of the empirical question: are press-lean clusters stable across windows? Study needed: re-run Ward per rolling window, match blocs by Hellinger distance or the adjusted Rand index, and report persistence with window and denominator.
- No external check of bloc direction (between-bloc lean against CU roll-call votes), which the pseudoconsensus logic requires before labels are trusted.
- No sensitivity check of COALITION_LEAN_WEIGHT 3.0 or the 6-member minimum.

**Paul's position vs the evidence.**

Paul highlighted that SNA is underused in coalition studies, and that issue salience and third-party brokers drive coalitions. He left no annotation.

The component uses no network structure and no brokers: it is attribute clustering. So it does not act on the points Paul highlighted.

The evidence supports renaming the output, or treating it as a policy-camp map, rather than claiming coalition detection.

**Open questions.**
- Is the 'all blocs pro-CU' mislabel note stale, given the code now assigns three labels? Which R script version produced the workbook blocs?
- Should 'coalition' be renamed (for example 'press-lean bloc') on surfaces?

**Read first.** [[pritoni-2025-lobbying-coalitions-determinants]], [[malaterre-2024-inferring-social-networks]], [[cao-2025-social-network-group]]

## Sector network configurations

**What it computes.** Bank-regulation and crypto cosponsor graphs alongside the CU graph.

**Method.** Hub concentration flag at hub degree >= 3x the second highest. Recorded: bank_reg 4,742 dyads, 76.1% novel vs CU; crypto p_max 0.23 vs CU 0.52 (below the engine floor).

**Code refs.**
- `fec/moneyball_core.py NETWORKS registry`

**Assumptions.**
- Sector bill universes define sector-specific influence networks

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | suggests-extension | Core-periphery and rich-club claims are tested against hypergraph configuration null models (Chodrow 2020) that preserve degree and edge size, using rho(k) = phi(k)/phi_rand(k). Stakeholders show rho > 1 for all k. Large meetings drive connectivity. | A null-model rich-club or core test would show whether a sector's concentration exceeds what its degree and bill-size sequence already implies. The 3x rule cannot separate that. This only holds if sector bill rosters are treated as hyperedges. With the thin samples (crypto p_max 0.23), null-model variance will be large. Any result is UNVERIFIED until reproduced (DE-062). | moderate | VERIFIED |
| [[kim-2020-mapping-political-communities]] | 2020 | suggests-extension | Legislation communities (Healthcare, Veterans' Affairs, Technology & Telecommunications, Energy, Finance & Insurance, and others) are estimated from interest-group x politician lobbying counts and named ex post. A permutation test against random networks with similar actor activity distributions finds interest groups in the null have many more community memberships, indicating strong community structure. | A biLCM on the existing org x bioguide counts could only be a descriptive diagnostic of whether hand-drawn sector boundaries appear in lobbying. Under D-021 its outputs must not become edges or weights; UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED |
| [[ardekani-2026-computational-graph-theoretic]] | 2026 | qualifies | Where the network boundary is drawn changes who is central: the top-10 Katz ranks reshuffle between the 2-layer and 3-layer subgraphs. The authors attribute part of the seed's 2-layer prominence to construction bias, since the seed is the root of the subgraph. | The abstract point transfers: cross-universe centralization comparisons depend on how each bill universe was screened. The paper bounds by citation hops from one seed, not by topic screen, so its effect size does not transfer. Any cross-config comparison must name its window and denominator. The bank_reg 76.1 percent novelty figure is a run output not checked here. | weak | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: weak to moderate, all extension or caution, none validating.** There are 3 rows: 2 VERIFIED, 1 VERIFIED-SOFTENED.

**Azaiez 2025 (VERIFIED, moderate)** tests core and rich-club claims against hypergraph configuration nulls that preserve degree and edge size. The 3x hub rule cannot tell whether a sector's concentration exceeds what its degree and bill-size sequence already imply. Crypto is recorded as one 167-degree star from a single large CBDC roster, against 25 for second place, which is exactly the large-hyperedge artefact Azaiez describes. With thin samples (crypto p_max 0.23), null variance will be large.

**Ardekani 2026 (weak)** shows that where the boundary is drawn changes who is central. Cross-universe centralization comparisons depend on how each bill universe was screened (CRS terms vs title screen), so every cross-config comparison must name its window and denominator.

**Kim & Kunisky 2020 (VERIFIED, weak)** estimates legislation communities from interest-group x politician lobbying counts. A biLCM on the existing org x bioguide counts could only be a *descriptive* check of whether hand-drawn sector boundaries show up in lobbying. Under D-021, LDA is an identity source, never an edge or weight source, so outputs must not become edges or weights.

The bank_reg 76.1% novelty and crypto p_max are run outputs not re-checked in the rows.

**Contradictions.**
- The assumption 'sector bill universes define sector-specific influence networks' meets Ardekani and Azaiez. Measured centralization is partly an artefact of the boundary and of bill size (the crypto star is one roster), so the networks partly reflect screening choices.

**Gaps.**
- No test of the empirical question: does sector-network centralization predict passage rates? Study needed: compare centralization with a null-model rho(k) across sectors against sector passage rates over several Congresses. With only a handful of sectors this is low-power and can only be descriptive.
- No null-model concentration test to replace the 3x rule.
- No robustness check of sector universes to alternative screens (CRS terms vs title, single-bill exclusion).

**Paul's position vs the evidence.**

Paul wants rival-industry champions and conflict mapped across sectors. None of the rows addresses cross-sector rivalry or conflict, so this is an evidence GAP.

Kim & Kunisky concerns lobbying communities, not rivalry. Using LDA lobbying counts to build cross-sector conflict edges would breach D-021.

A defensible route would be a descriptive overlap or contrast of sector cosponsor graphs, labelled as a heuristic model output (D-025).

**Open questions.**
- Should single-roster stars, like the crypto CBDC bill, be flagged or excluded before hub diagnostics?
- How should rival-industry conflict be represented without LDA-derived edges (D-021)? Through signed cosponsor or position data instead?

**Read first.** [[azaiez-2025-hypergraph-analysis-european]], [[ardekani-2026-computational-graph-theoretic]], [[kim-2020-mapping-political-communities]]

## Katz-Bonacich benchmark (diagnostic)

**What it computes.** Katz-Bonacich centrality on the live kernel (delta = 0.85 / lambda_max), Spearman-correlated with allocations, money and legislative score.

**Method.** Power iteration. Not consumed by the chain.

**Code refs.**
- `fec/engine/kb_benchmark.py:1-104`

**Assumptions.**
- Equilibrium contributions are proportional to Katz-Bonacich centrality (Battaglini & Patacchini)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[ardekani-2026-computational-graph-theoretic]] | 2026 | qualifies | Katz prestige counts incoming directed paths. On one seed the Mental Health Act ranks 2nd at 2 layers (2,159 Acts) and outside the top 50 at 3 layers (6,021 Acts). Corrections Act moves 7th to 35th; Health Practitioners Competence Assurance Act moves 8th to 37th. The attenuation factor alpha is not reported. | Only the radius-sensitivity lesson transfers: ranks computed on a bounded subgraph are local. The paper's Katz counts in-paths (prestige) on an Act-citation graph, while the system's KB counts out-walks on a legislator kernel, so the paper does not validate the benchmark's direction. The transferable check is KB rank stability across cu and holistic, UNVERIFIED until reproduced (DE-062). KB values are heuristic model outputs, never influence probabilities (D-025). | moderate | VERIFIED-SOFTENED |
| [[battaglini-2016-influencing-connected-legislators]] | 2016 | method-source | Office-motivated legislators with log utility get transfers exactly proportional to their Bonacich centrality b = (I - beta*G)^-1 * 1 (Prop 1, eq 4, eq 17-18). With policy-motivated legislators the weights become modified Bonacich centralities depending on preferences and pivot probabilities (eq 15, Prop 2), which converge to the office-motivated allocation as the number of legislators grows (Prop 3). Reported network externality phi = 0.2088 (SE 0.0697) committee, 0.2165 (SE 0.0703) corrected, 0.0837 (SE 0.0262) alumni (Table 2). | Exact proportionality holds only under office motivation with log utility; the docstring overstates it for the general case. Under policy motivation the weights depend on preferences and pivot probabilities. The paper's empirical networks are undirected; the system kernel is directed. The delta-versus-phi comparison (0.85 of the bound versus phi 0.08-0.25) is not like-for-like: delta is a Katz decay on the kernel, phi is an SAR coefficient. | strong | VERIFIED-SOFTENED |
| [[battaglini-2016-influencing-connected-legislators]] | 2016 | qualifies | The paper deliberately does not use cosponsorship as the network, because cosponsorship is determined simultaneously with contributions (fn 30). Sample is U.S. House members in the 109th-113th Congresses (2004-2012 cycles); outcome is total PAC money in logs, trimmed at the 1st and 99th percentiles. Controls in Table 2: margin of victory -0.897, DW_ideology -1.077, chair +0.401. | A KB-versus-money correlation on the cosponsorship graph cannot test the equilibrium the paper derives, since cosponsorship is the endogenous network the paper excludes. The pooled two-chamber estimates need an UNSTRATIFIED label. Paper evidence is House-only, 2004-2012; the money window for the system file is not recorded. | moderate | VERIFIED-SOFTENED |
| [[trivedi-2020-efficient-influence-maximization]] | 2020 | qualifies | PrKatz (Katz-based, Alshahrani et al. ref [1]) does worse than plain degree under ICM and on NetHept falls below Random for some k. The authors partly blame pre-processing that splits the graph into components. Katz centrality alone matches degree at small k and declines as k grows. | The paper concerns Katz as a seed-selection proxy for cascade spread and finds it weak. The system uses KB as an equilibrium-pricing benchmark, so the evidence only discourages promoting KB into seed choice. The rel's claim that nothing in the chain consumes KB is wrong: refresh_table.py:261 runs it as a DIAG step, surfaces/make_member_desk.py:825 reads kb_benchmark.json for display, legislation/run_legislation_shadow_recompute.py:403 holds an immutable-input contract on it, and audit/dashboard_integrity.py:625 checks it. KB values are heuristic model outputs, never access or closeness probabilities (D-025). | weak | VERIFIED-SOFTENED |
| [[moreno-2021-shadowing-shielding-effective]] | 2021 | qualifies | The degree correlation of optimal allocations is a by-product of shielding: Kendall tau is generally above 0.5 for K above 30 for both optimal and pure-shielding plans (Fig 5b). When the passive controller targets nodes whose neighbours have the lowest degree, /tau/ for optimal allocations falls below 0.25 while shielding gives tau down to about -0.5 (Fig 5c). The authors conclude that optimal allocations can be explained almost perfectly by shielding without degree preference (p.11). | An allocation-centrality correlation, high or low, can come from where the opponent places money rather than from a preference for centrality, so any reading of rho should be stratified by industry-money placement or labelled UNSTRATIFIED. Rho is a heuristic diagnostic, not closeness or influence evidence (D-025). The paper's evidence is one email network. | weak | VERIFIED |
| [[almazaydeh-2026-heuristic-centrality-methods]] | 2026 | suggests-extension | Simple centrality seed rules come close to sampling-based IM under IC (PageRank 96.5 percent of IMM) at far lower runtime (2,304 vs 2.95 spread per second at 1000 nodes). | Adding a seed-spread column would test the paper's question on Melange's own kernel, as a diagnostic only, labelled UNSTRATIFIED, with window and denominator named. KB remains a model output, not a measure of access (D-025). Nothing in the chain consumes it. | weak | VERIFIED-SOFTENED |
| [[battaglini-2018-effectiveness-connected-legislators]] | 2018 | method-source | Equilibrium effectiveness equals a characteristic-weighted Bonacich (Katz-Bonacich) centrality of the network (Prop 1; the rel wording proportional is looser than the paper's equal to). Corrected NLLS phi = 0.8324 (SE 0.0516) on row-share cosponsorship weights (Table 2 col 3). Horse race (Table 4 col 7): phi = 0.7095 (SE 0.0817), with betweenness, closeness and eigenvector not significant. N = 2,176 House member-Congresses, 109th-113th. | phi multiplies a row-stochastic W, so phi is a spectral-radius fraction, comparable in kind to the benchmark's 0.85 spectral fraction. Transfer to Melange still needs a KB-to-LES test on the same Congresses. The paper is House-only with row-share weights, while Paul's graph mixes chambers and uses a different weight normalization. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: strong theory source, but the theory does not hold on the graph used.** There are 7 rows: 1 VERIFIED (Moreno), 6 VERIFIED-SOFTENED.

**Battaglini & Patacchini 2016 (method-source, strong).** Transfers are proportional to Bonacich centrality *only* for office-motivated legislators with log utility. Under policy motivation the weights depend on preferences and pivot probabilities. The kb_benchmark docstring ('in proportion to KB centrality') overstates the general case.

The same paper deliberately does not use cosponsorship as the network, because cosponsorship is set simultaneously with contributions (fn 30). The benchmark computes KB on the cosponsor graph and correlates it with money. That cannot test the equilibrium the paper derives.

Stored rho values are raw Spearman with no controls, n = 533, both chambers pooled. They must carry an UNSTRATIFIED label, and the money window is not recorded:
- optimizer allocation 0.2468
- CU money 0.1347
- bank plus finserv 0.2304
- legislative score 0.6535

**Battaglini et al. 2018 (moderate).** Effectiveness equals characteristic-weighted KB, with phi = 0.83 on row-share weights, House-only, 109th-113th. Melange uses unweighted KB on a mixed-chamber kernel. Its 'legislative score' is the composite MBC.scores(), not the legislative pillar, so the 0.65 is partly circular. A KB-to-LES test on the same Congresses is needed.

**Moreno 2021 (VERIFIED, weak).** Allocation-centrality correlation can come from opponent placement (shielding) rather than any preference for centrality. Reading rho needs strata by industry-money placement.

**Trivedi 2020 and Almazaydeh 2026** concern seed selection. Evidence on Katz as a seed proxy is mixed and weak, which discourages promoting KB into the optimizer.

**Ardekani 2026** is in-path prestige on a citation graph, so it does not validate the out-walk direction. Only its lesson that ranks shift with radius transfers.

KB values are heuristic model outputs, never influence, access or closeness probabilities (D-025).

**Contradictions.**
- The 'not consumed by the chain' / 'diagnostic' claim is contradicted by several consumers: make_member_desk.py shows kb_pct, refresh_table.py:261 runs it as a step, shadow-recompute holds an input contract on it, and dashboard_integrity checks it.
- The docstring says groups allocate in proportion to KB. Battaglini 2016 makes that exact only under office motivation with log utility.
- Computing the benchmark on cosponsorship conflicts with Battaglini 2016 fn 30, which excludes cosponsorship as endogenous to contributions.
- The 'legislative score' correlate is the composite score, not the legislative pillar, so the 0.6535 is not a like-for-like test of Battaglini 2018.
- Battaglini 2016 rows give 6 senators, not 7, in the top-decile unallocated list.

**Gaps.**
- No test of the empirical question on an exogenous network. Study needed: compute KB on an exogenous graph (committee or alumni ties, as Battaglini used) and regress PAC receipts on it with margin, ideology and chair controls. Run it House-only per Congress, with the money window named.
- No KB-to-LES test on matched Congresses (Battaglini 2018 replication).
- No stability check of KB ranks across cu and holistic, and no seed-spread column for KB-top-k (Almazaydeh extension, diagnostic only).

**Paul's position vs the evidence.**

Paul lists Battaglini & Patacchini without annotation and highlighted Ardekani's choice of Katz over PageRank and closeness.

The Ardekani choice is a design preference on a statute-citation graph. It is not evidence that Katz outperforms on legislator networks. Battaglini 2018's horse race, where betweenness, closeness and eigenvector were not significant, is the closer support, for the House 2004-2012 only.

The benchmark as built cannot test the Battaglini equilibrium because of the endogenous cosponsor substrate. Its rho values should be presented as UNSTRATIFIED descriptive diagnostics.

Battaglini 2018's row-share weights are a per-member volume normalization of the rejected kind: FLAGGED, description only.

**Open questions.**
- Should kb_pct stay on the member desk while the benchmark's documented status is 'not consumed'? If it stays, the docstring should be corrected.
- Should an exogenous-network KB (committee co-membership) be built for the benchmark?
- What money window and denominator underlie the stored CU and industry rho values?

**Read first.** [[battaglini-2016-influencing-connected-legislators]], [[battaglini-2018-effectiveness-connected-legislators]], [[moreno-2021-shadowing-shielding-effective]], [[almazaydeh-2026-heuristic-centrality-methods]], [[ardekani-2026-computational-graph-theoretic]], [[trivedi-2020-efficient-influence-maximization]]

## Across components

**1. Method borrowed, evidence not transferred.** Every component cites a source whose *procedure* Melange follows but whose *evidence* comes from a different substrate, chamber, era or platform:
- Sotoudeh: House 2009-2019, pass/total ratio
- Aref: per-chamber, per-Congress SDSM cosponsorship
- Malaterre: an 88-year academic corpus
- Fink: the Twitter API, 100-tweet floor
- Battaglini: an exogenous network, House-only

In each case, read the citation as 'inspired by', not 'validated by'.

**2. Chamber pooling is the main fault line.** Several outputs pool both chambers: the signed partition, the KB benchmark rho values (n=533) and the mixed-chamber kernel. Aref 2020 finds its effect in the House and not the Senate. Harbridge-Yong reports different coefficients by chamber. Any pooled coefficient or partition must be stratified or labelled UNSTRATIFIED.

**3. Documentation drift.** These recur across components:
- 'Diagnostic, not consumed' claims: KB and signed coalitions are read by the member desk, audit and shadow-recompute.
- Stale parameter claims: regular weight 0.5 vs 0.2, 9 vs at most 8 Ward blocs, all-pro-CU labels vs three labels, 19 vs 20 HCoI categories, 385 vs 155 members.
- Attribution mismatches: Fink's denominator reversed; the signed-coalitions substrate described as sdsm.

These are cheap to fix and should be resolved before any evidence claim is published.

**4. Endogeneity.** Battaglini 2016/2018 and Ringe 2013 together say cosponsorship is neither exogenous to money and effectiveness nor equivalent to alignment. That bears on the cosponsor network, the KB benchmark and bpf at once.

**5. Rejected-family flags.** Several rows touch the rejected per-edge or degree volume normalization:
- Aref SDSM null
- Tang reliability weights
- Ahmad Eq. 3
- Battaglini row-share
- Porter observed/expected
- the proposed n_scored count floor

None argues for dropping regular cosponsorship edges. All are FLAGGED, none recommended.

**6. Missing validation pattern.** No layer or partition has a held-out predictive test, null-model comparison or gold-label check on congressional data. The same design would fill most gaps: per-chamber, per-Congress link prediction against party and state baselines, plus weighted degree-preserving nulls.

**7. Reciprocity.** No row validates reciprocity. The Leskovec bidirectional-dyad note is explicitly not reciprocity evidence (D-104..D-108).

**8. Unverified leads.** Steinert-Threlkeld 2018, Zhang 2022 and Cao 2025 are UNVERIFIED leads only. The code-read counts in several rows are also single-path and UNVERIFIED (DE-062): the Fink denominator, the 49% single-context negatives, the Bluesky party skew and the D4 0-of-14 placement.

## Research agenda

1. Cosponsor weighting link-prediction study: train on Congress t under alternative edge weightings (raw, role, +success, +decay, +1/sqrt(C)) and score on Congress t+1 cosponsorship and co-voting, per chamber, with window and denominator named. This answers the 2:1 role and success-weight questions directly.
2. Fink-consistency recompute of social_interactions.json with a source-volume denominator against the current engaging-member denominator, with rank-delta on the cascade outputs. This settles the code-read contradiction (DE-062).
3. Held-out layer-gain test (Baptista design) for the social, press-engagement, HCoI and co-appearance layers: report each layer's predictive gain over the cosponsor base and its overlap with that base, per chamber.
4. House-only, per-Congress signed SDSM cosponsorship backbone, built as a separate diagnostic substrate. Re-run the frustration partition with a weighted degree-preserving rewired null and a sign-permuted null, and attempt the exact Aref Eq. 8 solve at n=520. Keep the SDSM null out of kernel edge weights (rejected normalization family).
5. Gold-label check of LLM reference valence: a hand-coded stratified sample by party pair and n_scored, with agreement statistics, plus an embeddedness census and Leskovec leave-one-out sign consistency reported against the base rate.
6. Attracted vs offered bipartisanship shares (Harbridge-Yong replication) on the current roster, per chamber, against next-Congress cross-party recruitment and LES.
7. KB on an exogenous network (committee or alumni ties) against PAC receipts with margin, ideology and chair controls, House-only per Congress. Also a KB-to-LES test on matched Congresses using the legislative pillar, not the composite score.
8. HCoI validation: within- vs between-community cosponsorship against a permutation null, modularity Q, Louvain seed stability, committee-roster alignment, and a multi-window Hellinger genealogy.
9. Ward bloc stability across rolling windows (ARI or Hellinger matching), plus a between-bloc direction check against CU roll calls. Rename blocs as policy camps pending coordination evidence.
10. Sector hub diagnostic: replace the 3x rule with a hypergraph configuration-null rich-club test. Check universe robustness to alternative screens and to single-roster stars.
11. Documentation reconciliation: list every stale consumption claim and parameter claim (KB, signed coalitions, regular weight, Ward k and labels, HCoI category and member counts, press-engagement 'validates' docstring) for Paul's ruling.
