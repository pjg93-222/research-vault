---
type: system
name: Melange congressional staffer network
slug: melange-staffer-network
components:
  - melange-staffer-network.overlap-edge-rule
  - melange-staffer-network.cotenure-exposure-weights
  - melange-staffer-network.layer-balanced-pagerank
  - melange-staffer-network.identity-resolution
  - melange-staffer-network.access-uncertainty
  - melange-staffer-network.noisy-or-and-outreach
brain_notes:
  - concepts/concurrent-service-overlap-edge-rule
  - actors/staffer-live-sidecar
  - topics/2026-09-feature-staffer-network-data-and-math
  - topics/2026-09-feature-staffer-network-data-and-math-sna-math-and-clique-lane
paper_count: 8
tags: [system]
---

# Melange congressional staffer network

The people-only graph below members, built from House and Senate payroll and Quorum directories. Covers identity matching and consolidation, the concurrent-service OVERLAP edge rule, the co-tenure and exposure weights, layer-balanced PageRank, eigenvector and bridging measures, noisy-OR evidence, access uncertainty, and outreach ranking. It is descriptive only: there is no composite influence score.

**Brain notes:** [concurrent-service-overlap-edge-rule](obsidian://open?vault=brain&file=concepts%2Fconcurrent-service-overlap-edge-rule) · [staffer-live-sidecar](obsidian://open?vault=brain&file=actors%2Fstaffer-live-sidecar) · [2026-09-feature-staffer-network-data-and-math](obsidian://open?vault=brain&file=topics%2F2026-09-feature-staffer-network-data-and-math) · [2026-09-feature-staffer-network-data-and-math-sna-math-and-clique-lane](obsidian://open?vault=brain&file=topics%2F2026-09-feature-staffer-network-data-and-math-sna-math-and-clique-lane)

**Melange congressional staffer network: evidence assessment (hub read 2026-10-09)**

The hub has 12 evidence bearings on 6 components. None of them comes from a study of congressional staff ties. They transfer from other settings: European Commission meeting attendance (Azaiez 2025), information-exchange SNA methods (Haythornthwaite 1996), House committee **member** assignments (Porter 2005), a biological and airport multilayer walk (Baptista 2022), a New Zealand statute network (Ardekani 2026), LDA lobbyist disambiguation (Bacik 2025), trust networks in expert panels (Cao 2025, **UNVERIFIED**) and influence maximisation by graph sampling (Kamarthi 2019).

By status: 4 are VERIFIED, 7 VERIFIED-SOFTENED and 1 UNVERIFIED. By strength: 4 moderate and 8 weak. No bearing **validates** any numeric constant in the system: the kappa ladder, tau = 365.24 d, the 1/(n-1)^gamma dilution, the shared-role factors, the damping value, equal layer allocation, the 5-year half-life, the 0.35 and 0.25 cost terms, or the 8-character title threshold. All of these remain owner design priors.

The strongest finding is about a mechanism, not a calibration. Projecting a large group onto pairwise cliques inflates the members of large groups (Azaiez, moderate, VERIFIED-SOFTENED). The project had already found this internally. In the v5 generation, before the admin exclusion, Pearson(co-tenure degree, largest office size - 1) = 0.8694 over 144,693 person nodes, and 54 admin containers held 70.8% of 12,498,349 guaranteed pairs. The v6 admin exclusion cut this to 4,660,388 guaranteed pairs.

Container-size normalisation is still Paul's OPEN ruling. **FLAG:** read as per-edge reweighting, the size-correction arguments in Azaiez and Porter come close to the **per-edge volume normalisation that this project measured and REJECTED**. They are evidence for Paul's ruling, not a recommendation. Azaiez's own remedy is a measure computed natively on the hypergraph, not per-edge down-weighting. The project's Katz finding also says per-edge reweighting cannot remove a degree artifact under sum-based centrality.

Wording rules apply throughout. Every rank, exposure, strength, route cost and access-uncertainty factor is a heuristic model output, never closeness, access or introduction probability (D-025). Influence or control-of-flow language is barred on revolving-door edges. LDA may enter only as an identity source (D-021).

Two components are effectively gaps. Identity resolution has one weak bearing and no precision/recall study. Noisy-OR and outreach have one weak bearing that applies only to data-collection priority, and nothing on whether the sources are independent. Access uncertainty has only an UNVERIFIED lead, so it is also a gap in evidential terms.

## Concurrent-service OVERLAP edge rule

**What it computes.** A person-person edge exists only if two people share a container and their payroll intervals intersect.

**Method.** Weight = guaranteed overlap x container proximity kappa (personal office 1.00, committee same side 0.75, unknown 0.60, opposite 0.40, leadership 0.75, admin 0.50). 16.0M of 31.1M candidate pairs rejected for zero overlap. After admin exclusion (v6) 4.66M guaranteed pairs remain. Admin containers carried 70.8% of pairs; degree vs largest office size r = 0.87 (v5, before admin exclusion).

**Code refs.**
- `audits/staffer_network/build_staffer_social_network.py:1150-1194`
- `fec/staffer_network/employment_history.py:1965-2124`
- `staffer_graph_snapshot.py:241-245`

**Assumptions.**
- Concurrent service in a container indicates a real connection
- Sequence is a career trajectory, not a tie
- Very large containers are suspected catch-all buckets

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | qualifies | Projecting meetings onto pairwise cliques inflates nodes that have low hypergraph degree but attend a few large meetings. Within each framework the measures agree (clique coreness C vs Cstak r = 0.93; hypercoreness R vs Rstak r = 0.79); cross-framework correlations are weaker (exact values only in Fig. 6). Separately, edge betweenness correlates 0.74 with meeting size. | The mechanism carries over: clique projection of any large hyperedge inflates its members. Magnitudes do not: EC meetings are single simultaneous events, payroll containers are multi-year co-presence. The paper's remedy is a hypergraph-native measure (hypercoreness), not per-edge down-weighting; the project's Katz finding (STATE.md:886-896) is that per-edge reweighting cannot remove a degree artifact under sum-based centrality. Evidence for Paul's open ruling, not a recommendation. The 0.87 figure is pre-admin-exclusion and must carry that label. | moderate | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | qualifies | Structure is derived from observed relationships, not from a priori classes. The article lists membership in organizations among the a priori classifications it avoids as a basis for groups. Cliques form among similar people and limit nonredundant information. Density is actual over possible links, with n(n-1) possible directed ties (380 for n = 20). | The article would treat container co-presence as a membership-based classification, close to the a priori organizational class it warns against, not as an observed exchange. Density and clique statistics inside a container are produced by construction. The article gives no link between co-presence and actual exchange. No edge may be described in influence or access terms. The 1.00 to 0.40 kappa ladder and the 70.8 percent figure were not located by that verifier. | moderate | VERIFIED-SOFTENED |

**Assessment.** **Evidence weight: low to moderate, and it qualifies the rule rather than supporting it.**

Two bearings apply, both VERIFIED-SOFTENED and both moderate, and both carry the relation *qualifies*:

- **Azaiez 2025.** Projecting meetings onto pairwise cliques inflates nodes that attend a few large meetings. Within one framework the measures agree (clique coreness r = 0.93; hypercoreness r = 0.79). Edge betweenness correlates 0.74 with meeting size. The **mechanism** transfers to payroll containers, which are a clique expansion of a container hyperedge. The **magnitudes** do not: EC meetings are single simultaneous events, while containers are multi-year co-presence.
- **Haythornthwaite 1996.** Structure should come from observed relationships, not a-priori classes, and organisational membership is named as such a class. Density and clique statistics inside a container are therefore produced by construction. The article gives no link between co-presence and actual exchange.

**What the rule gets right.** The overlap predicate itself is sound and firing. In the v5 generation, 16,001,459 of 31,139,699 candidate same-container pairs (51.4%) were rejected for zero overlap. An internal check found 0 mismatches in 2,000 sampled pairs. No paper disputes the temporal-intersection requirement. Requiring concurrency is the conservative choice, and it is consistent with Haythornthwaite's warning against assuming ties from class membership.

**What it does not establish.** No evidence shows that concurrent payroll co-presence is a real interpersonal tie. The kappa ladder (personal office 1.00 down to opposite side 0.40, admin 0.50) has no literature basis. The Haythornthwaite verifier could not locate the ladder values in ranking_math.py. The brain note records them only as design section 6.2.

Figures must carry their window and denominator:
- r = 0.87 is **v5, before admin exclusion**, over 144,693 nodes. The hub's method field quotes it without that label.
- 70.8% is the share of v5 guaranteed co-tenure pairs held by 54 admin containers.
- 4.66M is the v6 guaranteed-pair count after admin exclusion.

**Contradictions.**
- Haythornthwaite 1996 (VERIFIED-SOFTENED) treats organisational membership as an a-priori class to avoid as a basis for structure. The rule's assumption that concurrent service in a container indicates a real connection, and Paul's own framing of it as "pretty close... an immediate connection", rest on exactly that kind of class. Container-internal cliques are produced by construction, not observed.
- Azaiez 2025 (VERIFIED-SOFTENED) shows that clique projection inflates members of large hyperedges. The rule enforces overlap in letter, but within an era it still produces whole-container cliques: the largest container had 3,373 people and the maximum degree was 4,321 (v5). Paul's 2026-09-07 clique finding is the internal confirmation.
- The evidence rows disagree on what was verified in code. The Haythornthwaite row says the kappa ladder and the 70.8% figure were not located. The Porter row locates 70.8% at employment_history.py line 83. The kappa ladder is still unverified in code.
- The hub's component method quotes r = 0.87 with no generation label. It is a v5 figure from before the admin exclusion and must not be read as describing the shipped v6 graph.

**Gaps.**
- No study tests whether concurrent co-tenure among congressional staff produces interaction or exchange. To fill this, use a validation sample of staffer dyads, for example a survey or interview roster in the style of congressional staff surveys, comparing co-tenured same-office pairs with non-overlapping same-office pairs. The comparison must be stratified by chamber and container kind.
- There is no predictive-validity test. Check whether GUARANTEED co-tenure pairs later re-co-locate (joint office moves in payroll) more often than matched non-overlapping peers, with container size held fixed. LDA may enter only as identity linkage, never as an edge or weight (D-021).
- No source gives a basis for the kappa ladder values. A study would need to compare outcome association across container kinds (personal office, same-side committee, leadership, opposite side), stratified by chamber.
- Two of Paul's cited sources have no extracted bearing on this component. Bacik et al. bears only on identity resolution. 'Furnas et al. 2022' does not resolve to a vault paper slug: the vault has furnas-2019-moneyed-interests-information and furnas-nd-conflating-lobbying-pacs. A citation check is needed.

**Paul's position vs the evidence.** Paul's sources (Haythornthwaite 1996, Bacik et al. 2026, Furnas et al. 2022) treat staffers as gatekeepers and professional ties as conduits. He does not discuss the overlap rule in them.

The hub says Paul's sources do not discuss the overlap rule. That is true of the cited literature, but the rule itself is Paul's ruling of record (2026-09-03, "REMEMBER, OVERLAP"), and his 2026-09-07 rulings excluded admin containers and relabelled the figure.

The evidence agrees with the conservative half of his position: no edge without concurrency, and sequence treated as trajectory. No paper argues for edges between non-overlapping pairs.

The evidence does not support the strong half, that concurrent co-presence is a close tie. Haythornthwaite would classify co-presence as membership, not observed exchange. Azaiez shows that large containers mechanically inflate degree.

His cited sources frame staff as gatekeepers and ties as conduits. That is conceptual framing and does not validate this edge definition. Container-size normalisation remains his OPEN ruling.

**FLAG:** if Azaiez is read as support for per-edge size down-weighting, that is the REJECTED per-edge volume-normalisation proposal and must not be recommended.

**Open questions.**
- Has the snapshot read at staffer_graph_snapshot.py:241-245 been filtered to status == GUARANTEED? The brain records that 2,639,891 POSSIBLE_ONLY edges were read back without a status filter in v5. Check the later landing record before quoting edge counts.
- Was the Office of Congressional Accessibility Services catch-all bucket resolved? In v5 it held 3,373 people against a real office of about 10, and 18.1% of pairs. It is admin-kind and so excluded in v6, but the parse defect may affect other containers.
- What is Pearson(degree, largest office size) on the v6 graph? Only the v5 pre-exclusion figure exists.
- Should a hypergraph-native measure, such as Azaiez's hypercoreness, be computed as a diagnostic beside the clique projection, without changing edge weights? This is Paul's call, under the OPEN size-normalisation ruling.

**Read first:** [[azaiez-2025-hypergraph-analysis-european]], [[haythornthwaite-1996-social-network-analysis]], [[porter-2005-network-analysis-committees]]

## Co-tenure weight and workplace exposure index

**What it computes.** Co-tenure weight = kappa x tier x (1 - exp(-days/365.24)) x shared-role attenuation (0.05). Exposure = sum over workplaces of kappa x alloc_i x alloc_j x integral of 1/max(1, n-1) concurrent headcount; strength = 1 - exp(-exposure/TAU).

**Method.** Equal allocation across concurrent workplaces; headcount census of observed people. Shared factors: House 0.02, Senate 0.05.

**Code refs.**
- `fec/staffer_network/ranking_math.py:11-59`
- `fec/staffer_network/closeness_math.py:16-200`

**Assumptions.**
- Shared days indicate contact opportunity with diminishing returns
- Larger offices dilute pairwise contact (1/n)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | supports | Hypergraphs are warranted where coordination or simultaneous interaction of more than two actors matters (p.3, citing prior literature). Clique projection explodes a d-person meeting into d-choose-2 links, so nodes with low hypergraph degree but a few large meetings get inflated coreness; hypercoreness avoids this (p.16-17). | Supports the direction of the correction, not the 1/(n-1) form or the gamma value. The paper never estimates a dilution exponent. The evidence comes from EC meeting attendance and only loosely fits multi-year office co-tenure. It also shows a live inconsistency: the sidecar dilutes by headcount and the scored operator does not. | moderate | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | supports | Relationship strength is its intensity, for example how many goods are exchanged or how often actors meet, and it can be kept as a value rather than dichotomized. At the tie level, measures combine frequency of contact, duration, intimacy, reciprocal services and kinship, and they depend on context. | Principle only: strength should be valued and duration is one input. The article gives no functional form, time constant or proximity multiplier, so tau and the kappa ladder remain owner design. The kappa ladder values were not located in ranking_math.py and are unverified here. The weight is a heuristic exposure score, not a contact probability (D-025). | weak | VERIFIED-SOFTENED |
| [[porter-2005-network-analysis-committees]] | 2005 | supports | Porter et al. normalize shared membership against the count expected if same-size committees were assigned at random (normalized interlock, 1 = chance), so container size does not by itself produce strong ties. The paper does not claim that raw shared membership is uninformative. | Porter's normalization is an observed-over-expected ratio at the container level. The Melange dilution is a duration-weighted 1/n^gamma on person-person exposure. Different unit and functional form, and Porter gives no basis for the exponent. The output is exposure, not closeness or access (D-025). | weak | VERIFIED-SOFTENED |

**Assessment.** **Evidence weight: directional support only. No functional form is supported.**

Three bearings apply, all VERIFIED-SOFTENED, with relation *supports*:

- **Haythornthwaite 1996 (weak).** Tie strength is intensity, should be kept as a value rather than dichotomised, and duration is one input. This supports a valued, duration-based weight in principle. It gives no functional form, time constant or proximity multiplier.
- **Azaiez 2025 (moderate).** Clique projection of large groups inflates their members, and hypergraphs are warranted where simultaneous multi-actor interaction matters. This supports the *direction* of a headcount correction, not the 1/(n-1) form or any gamma.
- **Porter 2005 (weak).** Normalised interlock compares shared committee membership with the count expected under random same-size assignment, where 1 = chance. That is a container-level observed-over-expected ratio for House **members**. It is a different unit and functional form from Melange's duration-weighted 1/n^gamma on person pairs. Porter does not say raw shared membership is uninformative.

**Unsupported constants.** None of the following has a literature basis: tau = 365.2425 d, the 0.05 shared-role attenuation, the House 0.02 and Senate 0.05 shared factors, gamma, TAU, and equal allocation across concurrent workplaces.

The saturating form 1 - exp(-days/365.24) reaches about 0.95 after about 3 years, so tenure beyond 3 years adds almost nothing. That is a strong unexamined assumption, recorded as finding M3 in the 2026-09-05 SNA audit.

The exposure index and the co-tenure weight disagree with each other (see contradictions). Every output here is a heuristic exposure score, not a contact probability (D-025).

**Contradictions.**
- The system is internally inconsistent, as the Azaiez bearing records. The sidecar exposure index (closeness_math.py) dilutes each shared day by concurrent headcount, as duration / max(1, n-1)^gamma for peers. The scored co-tenure operator (ranking_math.py) uses a kappa that depends only on container KIND. The same pair in a 3,000-person container gets diluted exposure but undiluted co-tenure weight.
- The hub's component text gives the exposure term as 1/max(1, n-1) with no exponent. The Porter bearing cites code using an exponent gamma (closeness_math.py:83-84), and peer and member terms differ (count-1 versus count). The hub description is incomplete.
- **FLAG (rejected-proposal collision).** Porter's normalised interlock and Azaiez's clique-inflation argument both point toward size normalisation. Applied as per-edge reweighting, that is the per-edge volume normalisation this project measured and REJECTED. The project's Katz finding says per-edge reweighting cannot remove the degree artifact under sum-based centrality. Treat both papers as evidence for Paul's OPEN ruling, never as a recommendation.

**Gaps.**
- No study estimates a dilution exponent, a saturation constant or a kind-specific proximity multiplier for workplace co-presence. To fill this, run a held-out predictive comparison of the kind-only kappa weight against the headcount-diluted exposure index and a hypergraph-native measure. The outcome would be later co-movement between offices in payroll data.
- That comparison must be reported by chamber: House payroll is quarterly and Senate payroll semiannual, and the shared factors differ (0.02 vs 0.05). Any pooled coefficient must carry an UNSTRATIFIED label.
- Nothing in the evidence tests whether tenure beyond about 3 years carries information. The saturation at about 3 years is untested, so a dose-response check of overlap length against the outcome is needed.
- Equal allocation across concurrent workplaces has no source. An allocation sensitivity sweep on the staffer graph would cover it.

**Paul's position vs the evidence.** Paul highlighted tie strength and the value of weak ties in Haythornthwaite (Haythornthwaite 1996, tie strength), and has no annotation on exposure weighting. The evidence agrees with valuing strength rather than dichotomising it.

There is a tension with his weak-ties emphasis. In the weak-ties argument, weak ties matter because they *bridge* clusters. This component spends its design effort on grading strong ties: kappa, duration and the headcount correction. Weak-tie value would surface, if anywhere, in bridging or office-diversity measures, not in this weight. No evidence row tests weak-tie value on staffer data.

The pending decay and container-size normalisation rulings remain Paul's, and nothing in the evidence decides them.

**Open questions.**
- Should the co-tenure operator and the exposure index share one size treatment, or is the divergence intended? This is Paul's ruling, and it is open.
- What gamma value ships, and was it ever swept?
- Is the shared-role attenuation (0.05) still applied after the v8 cutover, which damped shared-role cells instead of excluding them? The hub and the brain duration-ranking note should be reconciled.

**Read first:** [[azaiez-2025-hypergraph-analysis-european]], [[porter-2005-network-analysis-committees]], [[haythornthwaite-1996-social-network-analysis]]

## Layer-balanced PageRank, eigenvector and bridging diversity

**What it computes.** PageRank (d = 0.85; sensitivity 0.70 and 0.95) on a walk splitting mass equally across active layers, with group-balanced teleport. Per-component eigenvector (diagnostic). Bridging = normalized Simpson diversity of walk mass across target kinds.

**Method.** Duplicate support is collapsed by max. No composite influence score.

**Code refs.**
- `fec/staffer_network/network_math.py:25-339`
- `fec/staffer_network/graph_build.py:236-461`

**Assumptions.**
- Equal walk mass per layer is a modeling prior
- Values are descriptive, with no causal direction

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | qualifies | Core measures computed on the clique expansion diverge from hypergraph-native hypercoreness. The authors say a hypergraph representation is 'more than a minor modeling choice'. | The paper compares coreness, not PageRank, so it only implies that walk-based ranks on a projection may diverge from a hypergraph treatment. Under D-025 the ranks are heuristic outputs, not access measures, in either representation. | weak | VERIFIED |
| [[baptista-2022-universal-multilayer-network]] | 2022 | qualifies | Top-ranked nodes can shift as walk parameters change, so stability must be measured for each network rather than assumed. The 125-parameter-set exploration was run on the biological gene and disease network only. Airport top-rankings were highly variable in a supplementary figure (Fig S15) with no parameter-set count stated. | The operator transfers because both are stochastic walks over typed layers and node kinds. The stability result does not transfer by itself: the paper says stability depends on the network, so the same allocation and teleport sweep must be run on the staffer graph. The paper gives no guidance on which allocation is correct. Outputs remain descriptive connectedness, never access or introduction probabilities (D-025). | moderate | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | suggests-extension | Brokerage is a positional property separate from prominence. Actors on a path between networks, or spanning a structural hole between unconnected clusters, control flow between them, and betweenness measures this. A path that spans a structural hole is particularly useful. | Any span score added must describe graph structure only, not access or introduction probability (D-025). Staffer edges are co-tenure and revolving-door ties, so the paper's control-of-flow wording must not carry over. Payroll container cliques are built in by construction. The r = 0.87 degree versus office size figure was not checked. | weak | VERIFIED |
| [[ardekani-2026-computational-graph-theoretic]] | 2026 | context | The authors argue against PageRank for legislative networks because its static formulation is sensitive to network growth, citing Mariani, Medo and Zhang 2015. No numerical comparison with PageRank is shown. | Weak. The growth critique applies to citation networks, where older nodes accumulate in-links. A co-tenure graph takes the argument only if snapshots across vintages are compared. The paper offers an argument, not evidence. The co-tenure graph description comes from project context and was not checked here. | weak | VERIFIED |

**Assessment.** **Evidence weight: moderate on the method question (stability has to be measured), weak elsewhere. Nothing validates the ranks as meaningful.**

Four bearings apply:

- **Baptista 2022 (VERIFIED-SOFTENED, moderate, qualifies).** The multilayer random walk with restart transfers as an operator: stochastic walks over typed layers and node kinds. Top-ranked nodes can shift with walk parameters, and stability depends on the network. Baptista explored 125 parameter sets, but only on the gene-disease network. The staffer graph sweeps damping (0.70 / 0.85 / 0.95, using L1 distance and top-1000 overlap). It does **not** sweep the layer-allocation prior or the teleport vector, the two parameters Baptista says must be checked. The paper gives no guidance on which allocation is correct.
- **Azaiez 2025 (VERIFIED, weak, qualifies).** Measures on a clique projection diverge from hypergraph-native measures. This is shown for coreness, not PageRank, so it applies here only by implication.
- **Haythornthwaite 1996 (VERIFIED, weak, suggests extension).** Brokerage is a positional property separate from prominence. No betweenness or Burt-constraint score exists in fec/staffer_network. Any span score added would describe structure only. The paper's control-of-flow wording is barred on revolving-door edges.
- **Ardekani 2026 (VERIFIED, weak, context).** An argument against static PageRank because it is sensitive to network growth, with no numbers. It transfers only if snapshots are compared across vintages.

The project's own history matches Baptista's warning. In v5, the kind-balanced teleport gave 36 policy nodes the same restart mass as 144,693 persons, which inflated policy-linked persons' PageRank 11.6x. The v6 control reduced that to 1.74x. The teleport choice is a first-order lever and has never been swept.

Ranks are heuristic descriptive outputs, not access, closeness or introduction probabilities (D-025).

**Contradictions.**
- Ardekani 2026, cited by Paul, argues against PageRank for legislative networks, yet the system uses PageRank(0.85) as the primary outreach ordering key. The paper provides no evidence, only argument, so this is a tension, not a refutation.
- The modeling prior is documented in only one of two places. Baptista's verifier found the equal-layer split labelled as an explicit prior in graph_build.py but not in network_math.py.
- The hub may be stale. It describes bridging as normalised Simpson diversity across target kinds (bridging_diversity). The brain records that v6 replaced bridging_diversity with office_diversity. Confirm which measure ships before quoting it.

**Gaps.**
- There is no external validity test of any staffer rank. A prospective check would test whether layer-balanced PageRank ranks staffers who later reach senior titles (chief of staff, staff director) in payroll above a degree baseline and a container-size baseline, reported by chamber. The ranks would still be described as heuristic either way.
- There is no allocation or teleport sweep. To follow Baptista on this graph, vary the per-layer mass split and the teleport weights by node kind, and report top-k overlap.
- There is no brokerage measure (betweenness or constraint). One could be added only as a structural descriptor, after removing admin cliques, which otherwise dominate shortest paths.
- Ardekani's growth critique has not been tested across vintages: compare rank stability across rebuilds (v5, v6, v7, v8) with the same parameters.

**Paul's position vs the evidence.** Paul (citing Haythornthwaite 1996 and Ardekani et al. 2026) notes that more research on forms of centrality is needed, and the evidence agrees. The one moderate bearing (Baptista) says stability cannot be assumed and must be measured per network. Haythornthwaite separates brokerage from prominence, which the system currently measures only through a diversity proxy. Ardekani, his own citation, questions PageRank outright.

Nothing in the evidence argues that layer-balanced PageRank is the right centrality for staff, and nothing shows it tracks any outcome.

**Open questions.**
- Which ranking key does outreach use after v6: PageRank with the policy-link control, or a person-only variant? The 2026-09-05 audit judged the v5 key wrong (M1).
- Is the component eigenvector still normalised per component and therefore not comparable across components (finding M5)? If so, should it be published at all, or kept diagnostic only?
- Should an allocation and teleport sweep join the existing damping sweep? This is Paul's call.

**Read first:** [[baptista-2022-universal-multilayer-network]], [[azaiez-2025-hypergraph-analysis-european]], [[haythornthwaite-1996-social-network-analysis]], [[ardekani-2026-computational-graph-theoretic]]

## Staff directory matching and identity consolidation

**What it computes.** Quorum staffer to office matches, and automatic duplicate-person merges.

**Method.** A never-guess name ladder (nicknames, accents, surname + chamber uniqueness). A payroll-evidence pin plus record-pair title threshold >= 8 characters (raised from 6; the removals were bare 'counsel'). The web-reference loop has made 0 automatic merges. Exact-name candidates feed only an exploratory layer.

**Code refs.**
- `fec/build_staff_directory.py`
- `staffer_identity_auto.py:148`
- `staffer_payroll_identity.py`

**Assumptions.**
- Payroll account identity with a corroborating record pair means the same person
- Short title matches are false matches

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | qualifies | LobbyView's lobbyist name disambiguation is score-based and uncertain. Client matching relies on search-assisted lookup that resolves to third-party directories and fails for defunct firms. | The domains differ: LDA lobbyist names versus congressional payroll staff. Wherever LobbyView lobbyist IDs enter the staffer graph as identities (D-021 allows identity use), treat them as probabilistic merges, not ground truth. The system's ladder is stricter than LobbyView's. | weak | VERIFIED |

**Assessment.** **Evidence weight: weak. Effectively a GAP for validation.**

One bearing applies: **Bacik 2025 (VERIFIED, weak, qualifies)**. LobbyView's lobbyist name disambiguation is score-based and uncertain. Client matching depends on search-assisted lookup that fails for defunct firms.

It transfers only as a caution. Wherever LobbyView lobbyist IDs enter the staffer graph as identities (permitted under D-021), they are probabilistic merges, not ground truth. The domains differ: LDA lobbyist names versus congressional payroll staff. The Melange ladder (never-guess names, payroll-evidence pins, record-pair titles of at least 8 characters) is stricter than LobbyView's.

No bearing gives a precision or recall estimate for any comparable ladder. None supports the 8-character threshold (raised from 6 after bare 'counsel' false matches).

The web-reference loop has made 0 automatic merges. That keeps precision safe, but recall is unknown, and missed merges split one person into several nodes. That would deflate their degree and PageRank and inflate the node count.

**Contradictions.**
- No contradiction with the evidence. Bacik's description of uncertain, score-based disambiguation is consistent with the system's never-guess design, and the system is the stricter of the two.

**Gaps.**
- No precision or recall measurement exists. To fill this, hand-label an audit sample of matched and unmatched staffer-office and person-person pairs. Stratify it by chamber, name commonality (the John Smith problem) and source (Quorum vs payroll), and report precision and recall with denominators.
- There is no evidence on threshold choice for title matching. Compare false-merge and missed-merge rates at thresholds 6, 8 and 10 on the labelled sample.
- There is no estimate of downstream sensitivity: how much do degree and rank move under plausible missed-merge rates?
- Paul cites Kim et al. graph clustering for donor identity. The vault holds kim-2020-strategic-sequential-links, which is mapped to mentat-fec-attribution.identity-ladder, but no bearing on this staffer component was extracted. Whether its identity method transfers is unassessed.

**Paul's position vs the evidence.** Paul (citing Bacik et al. 2026 and Kim, Stuckatz & Wolters 2020) highlighted LobbyView's lobbyist disambiguation (the John Smith problem) and Kim et al.'s graph clustering for donor identity. The one verified bearing agrees with his concern: LDA-side identity is uncertain.

His interest in graph clustering points toward a probabilistic merge method. The system deliberately does not do this (never-guess; exact-name candidates only in an exploratory layer). The evidence does not decide between them. It supports treating any external IDs as probabilistic, and it leaves recall of the strict ladder unmeasured.

**Open questions.**
- What is the recall cost of 0 automatic merges from the web-reference loop? How many likely-same-person pairs remain split?
- Do any LobbyView lobbyist IDs currently enter the staffer graph as identities? If so, are they flagged as probabilistic?
- Should the exploratory exact-name layer ever be promoted? Only with a measured precision, and that is Paul's ruling.

**Read first:** [[bacik-2025-measuring-dynamical-evolution]], [[kim-2020-strategic-sequential-links]], [[kim-2018-lobbyview-firm-level]]

## Recency and access-uncertainty factor

**What it computes.** cost = 0.35 + ln2 x years since overlap / 5 + 0.25 x workplace concentration; factor = exp(-cost). Not a probability.

**Method.** Exposure-weighted last overlap per joint run.

**Code refs.**
- `fec/staffer_network/relationship_uncertainty.py:10-57`

**Assumptions.**
- Relationship usability decays with a 5-year half-life

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[cao-2025-social-network-group]] | 2025 | qualifies | Discount-factor and exponential-decay operators penalize long trust chains, but the drivers of decay (path length, node importance) and their effect on network reliability have not been systematically modelled. Narrative claim only; no decay rate is estimated. | Only the taxonomy transfers: the system uses a fixed-discount, exponential-decay operator of the kind the review names, and the review confirms that no literature-standard decay rate exists. SNGDM trust is trust elicited inside small expert panels. Staffer edges are dated employment exposure. Neither the 0.8 hop factor nor the 5-year half-life gains or loses support from this paper. Route costs stay heuristic and are never closeness or introduction probabilities (D-025). | weak | UNVERIFIED |

**Assessment.** **Evidence weight: none verified. GAP. The only bearing is an UNVERIFIED lead.**

**Cao 2025 (UNVERIFIED, weak, qualifies)** is a single-agent lead, not a finding (DE-062). It says discount-factor and exponential-decay operators exist in trust networks, but the drivers of decay have not been systematically modelled and no decay rate is estimated. If verified, it would only place the system's operator in a taxonomy. It would neither support nor undercut the 5-year half-life, the 0.35 base cost, the 0.25 concentration term or the 0.8 route-hop factor. Trust networks in expert panels are also a poor match for dated employment exposure.

The factor exp(-cost) is explicitly not a probability, and route costs stay heuristic (D-025).

The half-life itself has drifted. The 2026-09-05 SNA audit (finding M3) records that design section 6.3 specified a **12-year** half-life that was never implemented. The current factor uses ln2 x years / 5, a **5-year** half-life. Neither value has an empirical source.

**Contradictions.**
- The system has two half-lives. The design record (section 6.3, per the 2026-09-05 SNA audit) specified 12 years. relationship_uncertainty.py implements 5 years. Nothing in the evidence chooses between them, and the brain says the decay ruling stayed pending.
- Paul's two citations for this component do not estimate tie decay. Kim, Stuckatz & Wolters is about firm donation-to-lobbying timing. Fink et al. 2023 is a Congressional Twitter network dataset, which the system uses elsewhere for volume-normalised press and social edges. Neither has an extracted bearing here.

**Gaps.**
- No empirical half-life exists for former co-worker ties among congressional staff. To fill this, run a survival or hazard analysis of former co-tenured pairs, with the outcome being later joint activity (re-co-employment in the same new office, or a sequential move into the same office). The model should be a function of years since last overlap, overlap length and container kind, stratified by chamber, with non-overlapping same-office pairs as the baseline.
- The concentration term and base cost have no basis. Run an ablation that holds out later joint moves and compares cost with and without each term.
- The 0.8 per-hop route discount has no basis. Test path-length against later re-co-employment, as a descriptive check only.
- The Kim, Stuckatz & Wolters paper, as summarised in the vault (not an extracted bearing), reports a firm-member donation-to-lobbying association. In the Senate matched difference-in-differences it is about 8.5 points at year t and about 3 points by t+3, over 2,703 firms, 2008-2017, against a base rate of about 0.05. That is an organisational money-to-lobbying sequence, not an interpersonal tie, so it cannot calibrate a staffer half-life.

**Paul's position vs the evidence.** Paul (citing Kim, Stuckatz & Wolters 2020/2026 and Fink et al. 2023) wants relationship decay modelled, and the hub correctly states that no source estimates a half-life for staffer ties. The evidence supports his premise that decay is unmodelled in the literature. The only bearing, Cao (UNVERIFIED), says as much in another domain.

The implemented 5-year half-life is therefore an owner prior. It also departs from the 12-year value in the original design. Neither value should be described as evidence-based.

**Open questions.**
- Which half-life is Paul's ruling, 5 years (implemented) or 12 years (design 6.3)? Or should it stay pending until an empirical estimate exists?
- Should the access-uncertainty factor be shown on any surface before a decay study exists? If it is shown, it must carry a 'heuristic, not a probability' label (D-025).
- Can Cao 2025 be promoted from UNVERIFIED by a second-path check? Even if it is, its bearing is taxonomy only.

**Read first:** [[cao-2025-social-network-group]], [[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]]

## Noisy-OR evidence combination and outreach ranking

**What it computes.** Evidence strength = 1 - prod over sources of (1 - max source strength). Outreach candidates are ordered by PageRank, then route cost.

**Method.** Noisy-OR is defined but not in the build path.

**Code refs.**
- `fec/staffer_network/network_math.py:69-88`
- `fec/staffer_network/ranking_math.py:62-82`

**Assumptions.**
- Distinct source systems are independent evidence

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kamarthi-2020-influence-maximization-unknown]] | 2019 | suggests-extension | A cheap query rule can prioritize which nodes to reveal under a fixed budget. H2 (minimum-degree node among those discovered in the last step) beats CHANGE on voles4 (72.8 vs 58.9), damascus (104.9 vs 95.2) and israel (34.2 vs 30.6), Table 5. The text says H2 picks nodes with higher true betweenness than CHANGE. | Usable only as a data-collection priority, judged on graph-completeness metrics. Never judged on spread, access or introduction probability (D-025). Influence language stays barred on revolving-door edges. Any learned policy stays UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED-SOFTENED |

**Assessment.** **Evidence weight: weak and indirect. GAP on the core assumption.**

**Noisy-OR.** Noisy-OR evidence combination is defined but not in the build path. Its central assumption, that distinct source systems are independent evidence, has no bearing at all. Independence is doubtful on its face, because directory vendors and payroll records may share upstream sources. That is a hypothesis to test, not a finding.

**Outreach ranking.** Outreach ranking orders candidates by PageRank(0.85), then by origin-plus-target route cost. Its evidential standing is inherited from the PageRank component, so it rests on an unvalidated rank. In v5 that rank used a ranking key the 2026-09-05 audit judged wrong (M1).

**Kamarthi 2019 (VERIFIED-SOFTENED, weak, suggests extension).** A cheap query heuristic (H2, the minimum-degree node among those discovered in the last step) beat CHANGE on three graphs in Table 5 (72.8 vs 58.9, 104.9 vs 95.2, 34.2 vs 30.6). This transfers only as a **data-collection priority**: which directories or people to resolve next. It must be judged on graph-completeness metrics, never on spread, access or introduction probability (D-025). Any learned policy stays UNVERIFIED until a second path reproduces it (DE-062).

Influence language stays barred on revolving-door edges. LDA cannot be a noisy-OR source for tie strength, because LDA is identity only (D-021).

**Contradictions.**
- The independence assumption is untested, and it is plausibly violated if Quorum directories and payroll share upstream records. No evidence row addresses this either way.
- There is a framing risk around Kamarthi. Its objective is influence maximisation, and the transfer note restricts it to graph completion. Adopting it under an influence objective would breach D-025 and the revolving-door wording rule.

**Gaps.**
- No error-correlation study exists. To fill this, take a labelled sample of staffer ties or identities and estimate the joint error rates of each source pair (House payroll, Senate payroll, Quorum). Then test whether conditional independence holds before noisy-OR is wired into the build.
- There is no validation of the outreach ordering. At minimum, run a rank-stability check across damping, allocation and teleport settings for the top-k outreach list. The order would still be described as heuristic, not a likelihood of access.
- There is no budgeted acquisition study. If Paul wants it, run an offline replay that measures how fast each query rule (Kamarthi H2, degree, random) recovers held-out nodes and edges, scored on completeness only.

**Paul's position vs the evidence.** The hub records "No literature" for Paul, and the evidence does not change that. The one bearing (Kamarthi) concerns data-collection order, not evidence combination or outreach validity. Nothing supports or refutes noisy-OR independence, or ordering outreach by PageRank.

**Open questions.**
- Should noisy-OR stay out of the build path until a source error-correlation estimate exists? It is currently defined but unused.
- Which ranking key does rank_outreach_candidates consume after the v6 policy-link control? Is the shared-employee exclusion from top_staff applied there too?
- Is there any appetite for budgeted acquisition of directory resolution? If so, it would be scored on completeness only.

**Read first:** [[kamarthi-2020-influence-maximization-unknown]], [[baptista-2022-universal-multilayer-network]]

## Across components

**1. One artifact runs through every component: large containers.**

The overlap rule is honoured in letter, but clique expansion of large payroll containers still drives degree. The v5 figure, before the admin exclusion, was Pearson(degree, largest office size - 1) = 0.8694 over 144,693 nodes. That artifact then flows into the co-tenure weight (kind-only kappa) and into PageRank and outreach (walks over row-normalised strengths).

The best external evidence (Azaiez 2025, moderate, VERIFIED-SOFTENED) supports the mechanism. Its remedy is a hypergraph-native measure. **FLAG:** do not translate this into per-edge volume normalisation, a proposal this project measured and REJECTED. Container-size normalisation remains Paul's OPEN ruling.

**2. The system is inconsistent with itself.**

The sidecar exposure index dilutes by concurrent headcount, as 1/max(1, n-1)^gamma. The scored co-tenure operator does not. Any surface that shows both can rank the same pair differently. Reconciling them is a ruling for Paul, not something the evidence settles.

**3. The evidence does not transfer well.**

No bearing studies congressional staff. Porter 2005 is about House **members'** committee assignments. Azaiez studies meetings at the European Commission. Ardekani studies a New Zealand statute network. Baptista studies biological and airport networks. Bacik studies LDA lobbyists. Cao, which is UNVERIFIED, covers trust networks in expert panels.

The mechanisms transfer, but no magnitude, constant or functional form does. Every numeric dial in this system is an owner prior with no literature support.

**4. Paul's citations versus the extracted evidence.**

Several cited sources have no extracted bearing on the component they are cited for:
- Bacik is cited for the overlap rule but bears only on identity.
- 'Furnas et al. 2022' does not resolve to a vault slug.
- Fink et al. 2023 is a Twitter dataset.
- Kim, Stuckatz & Wolters covers firm donation-to-lobbying timing.

Ardekani, which Paul cites, argues *against* the system's main centrality choice.

**5. Stratification and labelling.**

Every validation proposed below must report by chamber (House payroll quarterly, Senate semiannual, different shared factors), and any pooled result must carry an UNSTRATIFIED label. The figures in the hub need generation labels: the v5 counts (31.1M, 16.0M, 12.5M, 70.8%, r = 0.87) and the v6 count (4.66M).

**6. Stale descriptions.**

The hub description may be out of date in two places. Bridging diversity may have been replaced by office_diversity in v6. The access-uncertainty half-life is 5 years in code and 12 years in the original design. Confirm both against the live tree before publishing the assessment.

## Research agenda

1. Predictive validity of the edge (overlap rule, weights). Test whether GUARANTEED co-tenure pairs later re-co-locate (joint office moves or re-co-employment in payroll) more often than matched non-overlapping same-office peers, holding container size fixed. Report by chamber and container kind. LDA may enter only through identity linkage, never as an edge or weight (D-021).
2. Hypergraph diagnostic for container size. Compute a hypergraph-native measure (for example hypercoreness, following Azaiez 2025) beside the clique projection on the v6 graph. Recompute Pearson(degree, largest office size) on v6. Treat this as evidence for Paul's OPEN size-normalisation ruling. Do not reintroduce the REJECTED per-edge volume normalisation.
3. Exposure-form comparison. On held-out later co-movement, compare the kind-only kappa co-tenure weight, the headcount-diluted exposure index (sweeping gamma) and the hypergraph measure. Include a dose-response check on overlap length to test the saturation at about 3 years implied by tau = 365.24 d. Report stratified by chamber.
4. Walk-stability sweep (following Baptista 2022). Extend the existing damping sweep (0.70/0.85/0.95) to the layer-allocation prior and the per-kind teleport weights, and report top-k overlap and L1 distance. Add a cross-vintage stability check (v5 to v8) to test Ardekani's growth critique. Outputs remain heuristic (D-025).
5. Rank validity against later career outcomes. Test whether layer-balanced PageRank ranks staffers who later reach senior titles above a degree baseline and a container-size baseline. Describe the result only as descriptive connectedness, never as access.
6. Identity-resolution audit. Hand-label a sample stratified by chamber, name commonality and source. Report precision and recall of the never-guess ladder with denominators, and compare title thresholds of 6, 8 and 10 characters. Measure the recall cost of 0 automatic merges and the sensitivity of degree and rank to missed merges.
7. Tie-decay survival study. Model the hazard of later joint activity among former co-tenured pairs as a function of years since last overlap, by chamber and container kind. Use it to inform Paul's pending decay ruling (5-year half-life in code versus 12-year in design 6.3). Until then, keep the factor labelled heuristic and not a probability.
8. Source error-correlation study before wiring in noisy-OR. Estimate joint error rates across House payroll, Senate payroll and Quorum directories on a labelled sample, and test conditional independence. LDA is excluded as a strength source (D-021).
9. Evidence hygiene. Verify Cao 2025 by a second path (currently UNVERIFIED, DE-062). Resolve the 'Furnas et al. 2022' citation to a vault paper. Extract bearings, if any, from kim-2020-strategic-sequential-links and Fink et al. 2023 for the components where Paul cites them. Locate the kappa ladder values in code. Confirm whether bridging_diversity or office_diversity ships.
