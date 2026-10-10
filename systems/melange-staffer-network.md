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
  - C:/Users/pgoff/Documents/brain/concepts/concurrent-service-overlap-edge-rule.md
  - C:/Users/pgoff/Documents/brain/actors/staffer-live-sidecar.md
  - C:/Users/pgoff/Documents/brain/topics/2026-09-feature-staffer-network-data-and-math.md
  - C:/Users/pgoff/Documents/brain/topics/2026-09-feature-staffer-network-data-and-math-sna-math-and-clique-lane.md
paper_count: 9
tags: [system, type/system-hub]
---

# Melange congressional staffer network

The people-only graph below members, built from House and Senate payroll and Quorum directories. Covers identity matching and consolidation, the concurrent-service OVERLAP edge rule, the co-tenure and exposure weights, layer-balanced PageRank, eigenvector and bridging measures, noisy-OR evidence, access uncertainty, and outreach ranking. It is descriptive only: there is no composite influence score.

Brain notes: [concepts/concurrent-service-overlap-edge-rule](obsidian://open?vault=brain&file=concepts/concurrent-service-overlap-edge-rule); [actors/staffer-live-sidecar](obsidian://open?vault=brain&file=actors/staffer-live-sidecar); [topics/2026-09-feature-staffer-network-data-and-math](obsidian://open?vault=brain&file=topics/2026-09-feature-staffer-network-data-and-math); [topics/2026-09-feature-staffer-network-data-and-math-sna-math-and-clique-lane](obsidian://open?vault=brain&file=topics/2026-09-feature-staffer-network-data-and-math-sna-math-and-clique-lane)

## Melange congressional staffer network: evidence assessment

**Bottom line.** The literature in the vault is thin for this system. It has **15 bearing rows from 9 papers on 6 components**. Of these, 6 are VERIFIED, 8 are VERIFIED-SOFTENED and 1 is UNVERIFIED (a lead only). By their own strength field, 9 rows are weak and 6 moderate; none is strong. **No paper estimates any constant the system uses.** That covers the kappa proximity ladder, the 365.24-day co-tenure time constant, the 0.05 shared-role attenuation, the headcount dilution form or exponent, the equal-per-layer walk split, the 5-year half-life, the 0.8 per-hop route factor and the noisy-OR independence assumption. Each is still owner design, not something the evidence calibrates.

**What the evidence does establish.**
- **Montgomery & Nyhan 2017** is the only Congress-specific empirical result: House personal offices, 105th-111th Congresses, observational. A member-member tie built from shared staff carries a small signal only when the staff are *senior*: senior-tie LES pooled rho 0.043, 95% CI 0.0095 to 0.096. Ties through lower-level staff are a null placebo. A future-senior placebo CI (-0.033 to 0.071) overlaps the headline, so homophily is not ruled out. The outcome is member-level similarity, not transmission, and the paper says nothing about whether staffer-staffer ties stay usable.
- **Azaiez & Mandel 2025** (EC lobby meetings) and **Porter et al. 2005** (House committee interlock) agree that treating a large group's co-presence as pairwise cliques inflates its members, so size has to be dealt with somewhere. Neither gives the form of the correction, and Azaiez's remedy is a hypergraph-native measure, not per-edge down-weighting.
- **Baptista et al. 2022** shows top ranks from a multilayer random walk can move with the walk parameters, to be measured on each network.
- **Haythornthwaite 1996** offers principles only: tie strength is a value, duration is one input, brokerage is separate from prominence, and organization membership is an *a priori* class, not an observed exchange.

**Where the evidence pushes against the system.**
1. The OVERLAP rule counts every concurrent pair in a container regardless of title. The one Congress study says only senior and policy staff carry signal.
2. Under the evidence, co-presence in a container is a membership class, not observed contact.
3. The scored co-tenure operator weights by container kind only, while the sidecar exposure math dilutes by concurrent headcount. This is a live internal inconsistency.

**Wording that must hold (D-025, D-021, DE-062).** PageRank values, bridging diversity, exposure strength and the access-uncertainty factor are heuristic model outputs, not closeness, access or introduction probabilities. Revolving-door and office-move edges take no influence language. Haythornthwaite's "control of flow" wording and the "probability of influence" in Fink et al.'s title must not be carried onto staffer edges. LDA and LobbyView can supply identities, never edges or weights.

**Flags.** Azaiez and Porter can be read as arguing for per-edge size normalization. This project measured per-edge volume normalization and REJECTED it, and the Katz finding is that per-edge reweighting cannot remove a degree artifact under sum-based centrality. They are evidence for Paul's open container-size ruling, **not a recommendation**. The 0.87 correlation between degree and largest office size is a v5, pre-admin-exclusion figure and must carry that label.

## Concurrent-service OVERLAP edge rule

**What it computes.** A person-person edge exists only if two people share a container and their payroll intervals intersect.

**Method.** Weight = guaranteed overlap x container proximity kappa (personal office 1.00, committee same side 0.75, unknown 0.60, opposite 0.40, leadership 0.75, admin 0.50). 16.0M of 31.1M candidate pairs rejected for zero overlap. After admin exclusion (v6) 4.66M guaranteed pairs remain. Admin containers carried 70.8% of pairs; degree vs largest office size r = 0.87.

**Code refs.** `audits/staffer_network/build_staffer_social_network.py:1150-1194`, `fec/staffer_network/employment_history.py:1965-2124`, `staffer_graph_snapshot.py:241-245`

**Assumptions.**
- Concurrent service in a container indicates a real connection
- Sequence is a career trajectory, not a tie
- Very large containers are suspected catch-all buckets

**Evidence.** 4 rows from 3 distinct papers; VERIFIED 1, VERIFIED-SOFTENED 3.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | qualifies | Projecting meetings onto pairwise cliques inflates nodes that have low hypergraph degree but attend a few large meetings. Within each framework the measures agree (clique coreness C vs Cstak r = 0.93; hypercoreness R vs Rstak r = 0.79); cross-framework correlations are weaker (exact values only in Fig. 6). Separately, edge betweenness correlates 0.74 with meeting size. | The mechanism carries over: clique projection of any large hyperedge inflates its members. Magnitudes do not: EC meetings are single simultaneous events, payroll containers are multi-year co-presence. The paper's remedy is a hypergraph-native measure (hypercoreness), not per-edge down-weighting; the project's Katz finding (STATE.md:886-896) is that per-edge reweighting cannot remove a degree artifact under sum-based centrality. Evidence for Paul's open ruling, not a recommendation. The 0.87 figure is pre-admin-exclusion and must carry that label. | moderate | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | qualifies | Structure is derived from observed relationships, not from a priori classes. The article lists membership in organizations among the a priori classifications it avoids as a basis for groups. Cliques form among similar people and limit nonredundant information. Density is actual over possible links, with n(n-1) possible directed ties (380 for n = 20). | The article would treat container co-presence as a membership-based classification, close to the a priori organizational class it warns against, not as an observed exchange. Density and clique statistics inside a container are produced by construction. The article gives no link between co-presence and actual exchange. No edge may be described in influence or access terms. | moderate | VERIFIED-SOFTENED |
| [[montgomery-2017-effects-congressional-staff]] | 2017 | supports | The tie is a member-member count of senior or policy staffers who worked for both offices in the current or previous two Congresses, undirected and row-standardized (p.11, p.18). Senior-tie LES pooled rho is 0.043, 95% CI 0.0095 to 0.096; senior-only ideology is -0.003, CI -0.029 to 0.020. The future-senior placebo CI of -0.033 to 0.071 overlaps the headline, so homophily is not excluded (p.29). | House personal offices, 105th-111th Congresses; observational; member-level outcome similarity, not transmission. The text gives Congress numbers only, so the 1997-2010 window is a calendar inference, not the paper's statement. | moderate | VERIFIED-SOFTENED |
| [[montgomery-2017-effects-congressional-staff]] | 2017 | qualifies | Only senior and policy staff carry signal. Lower-level staff ties are a placebo with a null LES lag (pooled -0.016, text CI -0.070 to 0.027). Adding policy staff to senior ties weakens the LES lag (pooled 0.013, CI -0.015 to 0.037; Table 2 p.24). | Applies if shared_with or shared_staff is ever read as a member relationship signal. The paper's senior-only and 3-Congress specification is the comparison; shipped counts are descriptive only. | moderate | VERIFIED |

**Assessment.**

**Evidence weight: moderate, and it mostly qualifies the rule.** Montgomery 2017 appears twice (one VERIFIED, one VERIFIED-SOFTENED); Haythornthwaite 1996 and Azaiez 2025 are VERIFIED-SOFTENED.

- **Montgomery & Nyhan 2017 (VERIFIED, qualifies).** The only Congress result. The tie is undirected and row-standardized, and counts *senior or policy* staffers who worked for both member offices in the current or two previous Congresses (House personal offices, 105th-111th). Senior-tie LES lag 0.043 (95% CI 0.0095 to 0.096). Lower-level staff ties are a null placebo (pooled -0.016, CI -0.070 to 0.027). Adding policy staff to senior ties *weakens* the lag (0.013, CI -0.015 to 0.037). The system's `shared_staff` / `shared_with` counts have no title filter and no time window at the count stage.
- **Montgomery (VERIFIED-SOFTENED, supports).** Treating a job sequence as not a social edge (office_moves emitted with canonical_eligible False) is consistent with the paper's homophily caution. The future-senior placebo CI (-0.033 to 0.071) overlaps the headline, so selection is not ruled out. The tie is member-member co-service, and the outcome is similarity, not transmission.
- **Haythornthwaite 1996 (VERIFIED-SOFTENED, qualifies).** Structure should come from *observed* relationships. Organizational membership is the a priori class the article avoids. Clique and density statistics inside a container arise by construction.
- **Azaiez 2025 (VERIFIED-SOFTENED, qualifies).** Projecting a large hyperedge onto pairwise cliques inflates its members. The mechanism transfers; magnitudes do not (EC meetings are single events, payroll containers are multi-year co-presence).

**Net.** Concurrent service is defensible as a necessary condition, not as a sufficient one for a real connection. The evidence points to two missing restrictions, a seniority filter and a time window. Container size remains the open ruling (Paul's).

**Contradictions.**
- Montgomery 2017 (VERIFIED): only senior and policy staff ties carry signal; lower-level ties are a null placebo. The OVERLAP rule and the shared_staff count apply no seniority or title filter. Every concurrent pair counts, with kappa set by container kind only.
- Haythornthwaite 1996: membership in an organization is an a priori class, not an observed relationship. The rule's first assumption treats membership as a tie.
- Azaiez 2025: a clique projection of large hyperedges inflates their members. The rule is exactly a clique expansion of container hyperedges. Admin containers carried 70.8% of pairs (v5, before the admin exclusion), and degree vs largest office size had r = 0.87 (v5, pre-admin-exclusion; must carry that label).

**Gaps.**
- No staffer-staffer validation that concurrent service predicts later joint activity (co-movement to the same office, joint later employment). Needed: a matched comparison of overlapping vs non-overlapping same-office peers on later co-movement, stratified by chamber and container kind, reporting window and denominator.
- No evidence supports the kappa ladder values (1.00/0.75/0.60/0.40/0.75/0.50). Needed: a held-out estimate of later co-movement rates by container kind.
- No study decomposes staffer degree into container size vs other structure after the admin exclusion (v6, 4.66M pairs). The 0.87 figure is pre-exclusion.
- Bacik et al. 2026 and Furnas et al. 2022 are in Paul's citations for this component but have no bearing rows here.

**Paul's position vs the evidence.**

Paul's sources frame staffers as gatekeepers and professional ties as conduits, and he does not discuss the overlap rule itself. The evidence does not support reading co-tenure edges as conduits: Montgomery's signal is member-level outcome similarity, homophily is not excluded, and the signal exists only for senior staff. Under D-025 and the revolving-door rule, conduit or influence wording cannot attach to these edges. The evidence is compatible with the rule as a *descriptive* co-presence graph, and argues for a seniority restriction and a time window if the graph is ever read as a relationship signal.

**Open questions.**
- Should the operator restrict person-person edges to senior or policy titles, or add a senior-only layer, to match the one Congress-validated specification (Montgomery 2017)? Paul's ruling.
- Should edges carry a time window (Montgomery: current plus two prior Congresses) rather than lifetime overlap?
- Container-size normalization is an OPEN ruling. The literature favors a hypergraph-native measure (Azaiez), not per-edge reweighting, which this project already REJECTED. Does Paul want a hypergraph treatment considered?
- After the v6 admin exclusion, what is the current correlation between degree and largest office size? Re-measure under a named window and denominator.

**Read first.** [[montgomery-2017-effects-congressional-staff]], [[azaiez-2025-hypergraph-analysis-european]], [[haythornthwaite-1996-social-network-analysis]], [[porter-2005-network-analysis-committees]]

## Co-tenure weight and workplace exposure index

**What it computes.** Co-tenure weight = kappa x tier x (1 - exp(-days/365.24)) x shared-role attenuation (0.05). Exposure = sum over workplaces of kappa x alloc_i x alloc_j x integral of 1/max(1, n-1) concurrent headcount; strength = 1 - exp(-exposure/TAU).

**Method.** Equal allocation across concurrent workplaces; headcount census of observed people. Shared factors: House 0.02, Senate 0.05.

**Code refs.** `fec/staffer_network/ranking_math.py:11-59`, `fec/staffer_network/closeness_math.py:16-200`

**Assumptions.**
- Shared days indicate contact opportunity with diminishing returns
- Larger offices dilute pairwise contact (1/n)

**Evidence.** 3 rows from 3 distinct papers; VERIFIED-SOFTENED 3.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | supports | Hypergraphs are warranted where coordination or simultaneous interaction of more than two actors matters (p.3, citing prior literature). Clique projection explodes a d-person meeting into d-choose-2 links, so nodes with low hypergraph degree but a few large meetings get inflated coreness; hypercoreness avoids this (p.16-17). | Supports the direction of the correction, not the 1/(n-1) form or the gamma value. The paper never estimates a dilution exponent. The evidence comes from EC meeting attendance and only loosely fits multi-year office co-tenure. It also shows a live inconsistency: the sidecar dilutes by headcount and the scored operator does not. | moderate | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | supports | Relationship strength is its intensity, for example how many goods are exchanged or how often actors meet, and it can be kept as a value rather than dichotomized. At the tie level, measures combine frequency of contact, duration, intimacy, reciprocal services and kinship, and they depend on context. | Principle only: strength should be valued and duration is one input. The article gives no functional form, time constant or proximity multiplier, so tau and the kappa ladder remain owner design. The kappa ladder values were not located in ranking_math.py and are unverified here. The weight is a heuristic exposure score, not a contact probability (D-025). | weak | VERIFIED-SOFTENED |
| [[porter-2005-network-analysis-committees]] | 2005 | supports | Porter et al. normalize shared membership against the count expected if same-size committees were assigned at random (normalized interlock, 1 = chance), so container size does not by itself produce strong ties. The paper does not claim that raw shared membership is uninformative. | Porter's normalization is an observed-over-expected ratio at the container level. The Melange dilution is a duration-weighted 1/n^gamma on person-person exposure. Different unit and functional form, and Porter gives no basis for the exponent. The output is exposure, not closeness or access (D-025). | weak | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: weak, about direction only.** All three rows are VERIFIED-SOFTENED: Azaiez 2025 (moderate), Haythornthwaite 1996 (weak), Porter 2005 (weak).

- **Haythornthwaite 1996.** Tie strength is an intensity value; duration and contact frequency are inputs. This supports a valued, duration-increasing weight in principle but gives no functional form, so the `1 - exp(-days/365.24)` saturation, the tier factor and the 0.05 shared-role attenuation are owner design.
- **Porter et al. 2005.** Normalized interlock compares observed shared membership with the count expected from random assignment to same-size committees. This supports *some* size correction, but as a container-level observed/expected ratio. Melange's is a duration-weighted `1/max(1, n-1)^gamma` on pairwise exposure. Unit and form differ, and there is no basis for gamma.
- **Azaiez & Mandel 2025.** Clique projection inflates nodes in a few large meetings. This supports the direction of headcount dilution, not its form.

**Internal inconsistency (VERIFIED in the rows).** The sidecar closeness math dilutes each shared day by concurrent headcount. The scored co-tenure operator weights by container *kind* only, through kappa.

**Flag.** Inverse-headcount dilution sits close to the per-edge volume normalization this project measured and REJECTED. Nothing here recommends extending dilution to the scored operator. These papers are inputs to Paul's open container-size ruling. The output stays a heuristic exposure score, not a contact or closeness probability (D-025).

**Contradictions.**
- The sidecar exposure index dilutes by concurrent headcount (closeness_math.py). The scored co-tenure operator uses kind-only kappa with no size term. The papers (Porter, Azaiez) side with a size correction in direction only.
- The component text states 1/max(1, n-1). The verified code reading is duration / max(1, count-1)^gamma for peers and max(1, count)^gamma for members. Spec and code differ by the gamma exponent.

**Gaps.**
- No study supports tau = 365.24 days, the tier factor, the 0.05 shared-role attenuation, or the House 0.02 / Senate 0.05 shared factors.
- No study estimates a dilution exponent gamma or tests 1/(n-1) against alternatives. Needed: a predictive comparison of raw vs inverse-headcount exposure on later co-movement between offices, stratified by chamber, reporting window and denominator.
- Equal allocation across concurrent workplaces (alloc_i) is not examined by any paper.

**Paul's position vs the evidence.**

Paul highlighted tie strength and the value of weak ties (Haythornthwaite). The evidence agrees that strength should be valued, not dichotomized, and that duration is a legitimate input. His weak-ties emphasis argues against designs that crush small ties toward zero; the 0.05 shared-role attenuation and steep headcount dilution do that, and no paper calibrates either. He has no annotation on exposure weighting, and the literature does not fill that gap beyond the direction of the size correction.

**Open questions.**
- Should the scored operator and the sidecar agree on container size, and which is the rule of record? Paul's ruling, tied to the open container-size question.
- Is gamma documented anywhere with a rationale, and is it 1 in production?
- Given the rejected per-edge volume normalization, should sidecar headcount dilution be re-examined under the same rejection, or is it distinct because it acts on exposure, not on edge weights fed to centrality?

**Read first.** [[porter-2005-network-analysis-committees]], [[azaiez-2025-hypergraph-analysis-european]], [[haythornthwaite-1996-social-network-analysis]], [[montgomery-2017-effects-congressional-staff]]

## Layer-balanced PageRank, eigenvector and bridging diversity

**What it computes.** PageRank (d = 0.85; sensitivity 0.70 and 0.95) on a walk splitting mass equally across active layers, with group-balanced teleport. Per-component eigenvector (diagnostic). Bridging = normalized Simpson diversity of walk mass across target kinds.

**Method.** Duplicate support is collapsed by max. No composite influence score.

**Code refs.** `fec/staffer_network/network_math.py:25-339`, `fec/staffer_network/graph_build.py:236-461`

**Assumptions.**
- Equal walk mass per layer is a modeling prior
- Values are descriptive, with no causal direction

**Evidence.** 4 rows from 4 distinct papers; VERIFIED 3, VERIFIED-SOFTENED 1.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | qualifies | Core measures computed on the clique expansion diverge from hypergraph-native hypercoreness. The authors say a hypergraph representation is 'more than a minor modeling choice'. | The paper compares coreness, not PageRank, so it only implies that walk-based ranks on a projection may diverge from a hypergraph treatment. Under D-025 the ranks are heuristic outputs, not access measures, in either representation. | weak | VERIFIED |
| [[baptista-2022-universal-multilayer-network]] | 2022 | qualifies | Top-ranked nodes can shift as walk parameters change, so stability must be measured for each network rather than assumed. The 125-parameter-set exploration was run on the biological gene and disease network only. Airport top-rankings were highly variable in a supplementary figure (Fig S15) with no parameter-set count stated. | The operator transfers because both are stochastic walks over typed layers and node kinds. The stability result does not transfer by itself: the paper says stability depends on the network, so the same allocation and teleport sweep must be run on the staffer graph. The paper gives no guidance on which allocation is correct. Outputs remain descriptive connectedness, never access or introduction probabilities (D-025). | moderate | VERIFIED-SOFTENED |
| [[haythornthwaite-1996-social-network-analysis]] | 1996 | suggests-extension | Brokerage is a positional property separate from prominence. Actors on a path between networks, or spanning a structural hole between unconnected clusters, control flow between them, and betweenness measures this. A path that spans a structural hole is particularly useful. | Any span score added must describe graph structure only, not access or introduction probability (D-025). Staffer edges are co-tenure and revolving-door ties, so the paper's control-of-flow wording must not carry over. Payroll container cliques are built in by construction. The r = 0.87 degree versus office size figure was not checked. | weak | VERIFIED |
| [[ardekani-2026-computational-graph-theoretic]] | 2026 | context | The authors argue against PageRank for legislative networks because its static formulation is sensitive to network growth, citing Mariani, Medo and Zhang 2015. No numerical comparison with PageRank is shown. | Weak. The growth critique applies to citation networks, where older nodes accumulate in-links. A co-tenure graph takes the argument only if snapshots across vintages are compared. The paper offers an argument, not evidence. The co-tenure graph description comes from project context and was not checked here. | weak | VERIFIED |

**Assessment.**

**Evidence weight: weak to moderate. Nothing validates the ranks.** Baptista 2022 (VERIFIED-SOFTENED, moderate); Azaiez 2025, Haythornthwaite 1996 and Ardekani 2026 (VERIFIED, weak).

- **Baptista et al. 2022 (moderate).** Closest operator match: random walk with restart over typed layers. Top ranks can move with walk parameters, and stability depends on the network. The 125-parameter-set exploration ran only on the gene-disease network, and airport rankings were highly variable (Fig S15). The current damping sweep (0.70/0.85/0.95, L1 distance and top-1000 overlap) covers damping only, *not* layer allocation or the teleport vector. The equal-per-layer split is labelled a modeling prior in graph_build.py but not in network_math.py.
- **Azaiez 2025.** Coreness on a clique projection diverges from hypercoreness; applies to PageRank only by implication.
- **Haythornthwaite 1996.** Brokerage is distinct from prominence, measured by betweenness. The system has no betweenness or Burt-constraint score. Bridging is Simpson diversity of walk mass across target kinds, a different quantity.
- **Ardekani 2026 (context).** An argument against static PageRank because of sensitivity to network growth, borrowed from citation networks. No numbers; applies only if snapshots are compared across vintages.

**Net.** The rows support labelling ranks as descriptive connectedness under a stated prior. No paper shows layer-balanced PageRank tracks any outcome. Ranks are heuristic outputs (D-025), never access or introduction probabilities.

**Contradictions.**
- Haythornthwaite 1996 treats brokerage or structural-hole spanning as a separate positional property measured by betweenness. The system's 'bridging' is a diversity-of-target-kinds score, so the label may suggest a brokerage reading the math does not compute.
- Ardekani 2026 argues against PageRank for legislative networks. It is an argument only, citing citation-network growth literature, with no comparison. It matters only for cross-vintage comparisons.

**Gaps.**
- No predictive validation. Needed: a prospective holdout freezing the graph at Congress t, with outcomes at t+1/t+2 (promotion to senior title, departure to registered lobbying), comparing PageRank vs degree vs size-adjusted degree, stratified by chamber, naming window and denominator. Lobbyist status may come from LDA only as an identity label (D-021), never as an edge or weight.
- No allocation and teleport sensitivity sweep on the staffer graph. Baptista says stability must be measured for each network.
- No measured comparison of the projection vs a hypergraph-native ranking.

**Paul's position vs the evidence.**

Paul's note that more research on forms of centrality is needed matches the evidence: parameter sensitivity that depends on the network (Baptista), divergence between projection and hypergraph treatments (Azaiez), and a brokerage dimension the system does not measure (Haythornthwaite). Ardekani, which Paul cites, offers an argument rather than evidence and should not carry weight alone.

**Open questions.**
- Should the sensitivity sweep extend from damping to layer allocation and teleport weights, as Baptista indicates?
- Should a betweenness or constraint score be added as a separate descriptive measure, labelled structural only, with no control-of-flow wording?
- Should 'bridging diversity' be renamed so it is not read as structural-hole brokerage?

**Read first.** [[baptista-2022-universal-multilayer-network]], [[haythornthwaite-1996-social-network-analysis]], [[azaiez-2025-hypergraph-analysis-european]], [[ardekani-2026-computational-graph-theoretic]]

## Staff directory matching and identity consolidation

**What it computes.** Quorum staffer to office matches, and automatic duplicate-person merges.

**Method.** A never-guess name ladder (nicknames, accents, surname + chamber uniqueness). A payroll-evidence pin plus record-pair title threshold >= 8 characters (raised from 6; the removals were bare 'counsel'). The web-reference loop has made 0 automatic merges. Exact-name candidates feed only an exploratory layer.

**Code refs.** `fec/build_staff_directory.py`, `staffer_identity_auto.py:148`, `staffer_payroll_identity.py`

**Assumptions.**
- Payroll account identity with a corroborating record pair means the same person
- Short title matches are false matches

**Evidence.** 1 rows from 1 distinct papers; VERIFIED 1.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | qualifies | LobbyView's lobbyist name disambiguation is score-based and uncertain. Client matching relies on search-assisted lookup that resolves to third-party directories and fails for defunct firms. | The domains differ: LDA lobbyist names versus congressional payroll staff. Wherever LobbyView lobbyist IDs enter the staffer graph as identities (D-021 allows identity use), treat them as probabilistic merges, not ground truth. The system's ladder is stricter than LobbyView's. | weak | VERIFIED |

**Assessment.**

**Evidence weight: weak, essentially a gap.** One row: Bacik et al. 2025 (VERIFIED, weak, qualifies).

- **Bacik 2025.** LobbyView's lobbyist name disambiguation is score-based and uncertain. Client matching relies on search-assisted lookup that resolves to third-party directories and fails for defunct firms. The domain differs (LDA lobbyist names vs congressional payroll staff), but wherever LobbyView lobbyist IDs enter the staffer graph as identities (allowed under D-021) they are probabilistic merges, not ground truth. The system's never-guess ladder, with a payroll pin and a title threshold of at least 8 characters, is stricter than LobbyView's.

No paper in the vault measures precision or recall of a name-matching ladder on congressional staff. The 0 automatic merges from the web-reference loop and the threshold raised from 6 to 8 characters are internal choices, not literature-backed.

**Contradictions.**
- None identified.

**Gaps.**
- No precision and recall estimate for the matching ladder. Needed: a stratified hand-labelled sample of Quorum-to-payroll matches and auto-merges (by chamber, name commonness, title length), with CIs and the denominator named (candidate pairs, not people).
- No evidence supports the 8-character title threshold. The same sample should measure false-match rates at 6 vs 8 characters.
- Kim, Stuckatz & Wolters (graph clustering for donor identity) is cited in Paul's position but has no bearing row here. The vault note kim-2020-strategic-sequential-links is tagged to the mentat-fec-attribution identity ladder and should be checked for transfer.

**Paul's position vs the evidence.**

Paul highlighted LobbyView's 'John Smith problem' and Kim et al.'s graph clustering for donor identity. The one verified row agrees: LobbyView identities are uncertain and must be treated as probabilistic when imported. The evidence does not test the system's own ladder. Kim's clustering is a lead for an alternative or cross-check, not evidence about the staffer ladder.

**Open questions.**
- Do any LobbyView lobbyist IDs currently enter the staffer graph as merged identities, and are they tagged as probabilistic?
- Is a hand-labelled precision/recall sample planned, and who labels it?
- Does Kim-Stuckatz-Wolters graph clustering transfer to payroll-name consolidation, where names come with dated office spans?

**Read first.** [[bacik-2025-measuring-dynamical-evolution]], [[kim-2020-strategic-sequential-links]], [[kim-2018-lobbyview-firm-level]]

## Recency and access-uncertainty factor

**What it computes.** cost = 0.35 + ln2 x years since overlap / 5 + 0.25 x workplace concentration; factor = exp(-cost). Not a probability.

**Method.** Exposure-weighted last overlap per joint run.

**Code refs.** `fec/staffer_network/relationship_uncertainty.py:10-57`

**Assumptions.**
- Relationship usability decays with a 5-year half-life

**Evidence.** 2 rows from 2 distinct papers; UNVERIFIED 1, VERIFIED 1.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[cao-2025-social-network-group]] | 2025 | qualifies | Discount-factor and exponential-decay operators penalize long trust chains, but the drivers of decay (path length, node importance) and their effect on network reliability have not been systematically modelled. Narrative claim only; no decay rate is estimated. | Only the taxonomy transfers: the system uses a fixed-discount, exponential-decay operator of the kind the review names, and the review confirms that no literature-standard decay rate exists. SNGDM trust is trust elicited inside small expert panels. Staffer edges are dated employment exposure. Neither the 0.8 hop factor nor the 5-year half-life gains or loses support from this paper. Route costs stay heuristic and are never closeness or introduction probabilities (D-025). | weak | UNVERIFIED |
| [[montgomery-2017-effects-congressional-staff]] | 2017 | context | Ties built over the current plus two prior Congresses carry the LES signal. Narrowing to a two-Congress horizon leaves the senior LES interval barely including zero (-0.0006 to 0.1088; 94% interval 0.0016 to 0.1066). This is window sensitivity, not a decay estimate. | Weak. The paper's window concerns member-member outcome similarity, not whether staffer-staffer relationships remain usable. It cannot calibrate the half-life. | weak | VERIFIED |

**Assessment.**

**Evidence weight: very weak. This is a calibration gap.** Two rows: Montgomery 2017 (VERIFIED, weak, context) and Cao 2025 (UNVERIFIED, a lead only).

- **Montgomery & Nyhan 2017 (context).** The LES signal uses ties over the current plus two prior Congresses. Narrowing to two Congresses leaves the senior interval barely including zero (-0.0006 to 0.1088). That is *window sensitivity* for member-member outcome similarity, not a decay estimate for staffer-staffer relationships. It cannot calibrate the 5-year half-life. It suggests a horizon of a few Congresses (roughly 4-6 years), not inconsistent with the half-life's order of magnitude, but that is not support.
- **Cao et al. 2025 (UNVERIFIED lead, DE-062).** A review saying discount-factor and exponential-decay operators penalize trust chains, with no standard decay rate. Not a finding.

The cost function (0.35 + ln2 x years/5 + 0.25 x concentration) and the 0.8 per-hop route factor are uncalibrated. The code correctly labels the result as not a probability. It must stay a heuristic factor, never an access or introduction probability (D-025).

**Contradictions.**
- None identified.

**Gaps.**
- No source estimates the half-life of former co-worker ties. Needed: a survival or hazard analysis on the payroll data: for pairs whose overlap ended at time t, model later joint activity (re-co-location, consecutive moves to the same destination) by years since overlap, stratified by chamber and seniority, reporting window and denominator.
- The 0.35 base cost, the 0.25 concentration coefficient and the 0.8 per-hop factor have no empirical basis.
- Kim, Stuckatz & Wolters and Fink et al. 2023 are in Paul's citations here, but neither has a bearing row. Fink is a Twitter interaction network and does not address decay.

**Paul's position vs the evidence.**

Paul wants relationship decay modeled and notes that no source estimates the half-life of staffer ties. The evidence confirms this: the only Congress result is window sensitivity for member-level similarity, and the review row is UNVERIFIED. The 5-year half-life is an assumption awaiting the study above.

**Open questions.**
- Can the payroll history support the survival study without external data, and is that a lane Paul wants opened?
- Should the 0.8 per-hop factor and the 5-year half-life be exposed as named dials with a sensitivity band on any surface that shows route costs?

**Read first.** [[montgomery-2017-effects-congressional-staff]], [[cao-2025-social-network-group]]

## Noisy-OR evidence combination and outreach ranking

**What it computes.** Evidence strength = 1 - prod over sources of (1 - max source strength). Outreach candidates are ordered by PageRank, then route cost.

**Method.** Noisy-OR is defined but not in the build path.

**Code refs.** `fec/staffer_network/network_math.py:69-88`, `fec/staffer_network/ranking_math.py:62-82`

**Assumptions.**
- Distinct source systems are independent evidence

**Evidence.** 1 rows from 1 distinct papers; VERIFIED-SOFTENED 1.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kamarthi-2020-influence-maximization-unknown]] | 2020 | suggests-extension | A cheap query rule can prioritize which nodes to reveal under a fixed budget. H2 (minimum-degree node among those discovered in the last step) beats CHANGE on voles4 (72.8 vs 58.9), damascus (104.9 vs 95.2) and israel (34.2 vs 30.6), Table 5. The text says H2 picks nodes with higher true betweenness than CHANGE. | Usable only as a data-collection priority, judged on graph-completeness metrics. Never judged on spread, access or introduction probability (D-025). Influence language stays barred on revolving-door edges. Any learned policy stays UNVERIFIED until a second path reproduces it (DE-062). | weak | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: weak and indirect. This is a gap.** One row: Kamarthi et al. 2020 (VERIFIED-SOFTENED, weak, suggests-extension).

- **Kamarthi 2020.** A cheap query heuristic (H2, the minimum-degree node among those just discovered) beats CHANGE at revealing an unknown graph under a budget (voles4 72.8 vs 58.9, damascus 104.9 vs 95.2, israel 34.2 vs 30.6; Table 5). This transfers only as a *data-collection priority*, deciding which directories or people to resolve next, judged on graph-completeness metrics, never on spread, access or introduction probability (D-025). Any learned policy stays UNVERIFIED until a second path reproduces it (DE-062).

**Noisy-OR.** Defined but not in the build path. Its assumption that distinct source systems are independent evidence has no literature support and is untested. **D-021 constraint:** if any 'source' combined by noisy-OR is LDA or LobbyView, it may contribute identity only, never add to a tie's evidence strength.

**Outreach ranking.** Candidates are ordered by PageRank then route cost. Both inputs are heuristic, so the ordering is a descriptive list of structural position, not likelihood of access.

**Contradictions.**
- None identified.

**Gaps.**
- No study tests source independence. Needed: an error-correlation check on a hand-labelled set of staffer ties, comparing false-positive co-occurrence across payroll, Quorum and directory sources. Where errors correlate, noisy-OR overstates strength.
- No validation of the outreach ordering against any outcome. Any test must be framed as structural ranking quality, not access.
- Paul has no literature for this component (his stated position).

**Paul's position vs the evidence.**

Paul states there is no literature, and the evidence confirms that. The single row is about budgeted graph exploration, not evidence combination or outreach ranking. Kamarthi 2020 is a different work from the Chen, Kamarthi, Wilder & Tambe UAI 2021 contingency-aware IM paper cited elsewhere in Melange; they should not be conflated.

**Open questions.**
- Is noisy-OR intended to enter the build path? If so, which sources does it combine, and does any derive from LDA (barred as an edge or weight source by D-021)?
- Should a budgeted directory-resolution priority (in the style of Kamarthi H2) be a separate data-collection lane, judged only on graph completeness?

**Read first.** [[kamarthi-2020-influence-maximization-unknown]], [[baptista-2022-universal-multilayer-network]], [[bacik-2025-measuring-dynamical-evolution]]

## Across components

**1. Container size runs through three components and is one open ruling.** Size shapes the OVERLAP rule (clique expansion of containers), the exposure weights (headcount dilution in the sidecar but not in the operator) and PageRank (walks over the projected cliques). The papers (Azaiez, Porter) agree that size must be handled somewhere, but Azaiez's remedy is hypergraph-native measurement, and the project's Katz finding is that per-edge reweighting cannot remove a degree artifact under sum-based centrality. **Per-edge volume normalization was measured and REJECTED here, and nothing in this note reopens it.** The evidence bears on Paul's open container-size ruling, which is his to make. The 0.87 correlation between degree and office size is a v5, pre-admin-exclusion figure.

**2. Seniority is the one Congress-validated dimension, and the system has none of it.** Montgomery & Nyhan 2017 is the only Congress result (observational, House personal offices, 105th-111th, VERIFIED): only senior and policy staff ties carry signal for member outcome similarity. The OVERLAP rule, co-tenure weight and shared_staff count apply no seniority filter, apart from the 0.05 shared-title attenuation, which points the other way. The result transfers imperfectly (House-only, member-level, pooled, homophily not excluded), so quote it as UNSTRATIFIED by chamber and do not apply it to the Senate without a check.

**3. Every time constant is uncalibrated.** The co-tenure saturation (tau = 365.24 d), the 5-year access half-life, the 0.8 per-hop factor and Montgomery's 3-Congress window. Only Montgomery measures anything time-related, and that is window sensitivity, not decay. One survival analysis of later joint activity by years since overlap on the payroll data would inform all three components at once.

**4. Validation is missing across the board.** Each component's empirical question names an outcome (later co-movement, later senior or lobbyist status, joint activity, precision and recall). No paper in the vault tests any of them on congressional staff. The system is consistently labelled descriptive, with no composite influence score, which is the correct and defensible claim at this evidence level.

**5. Wording discipline.** Haythornthwaite's control-of-flow and conduit language, Fink et al.'s 'probability of influence' and Kamarthi's spread objective must not migrate onto staffer edges or outreach ranks (D-025; no influence language on revolving-door edges). LobbyView and LDA appear only as identity sources (D-021); if they ever feed noisy-OR, they must not add to tie strength.

**6. Citations without bearings.** Paul's positions cite Furnas et al. 2022, Bacik et al. 2026, Kim, Stuckatz & Wolters 2020/2026 and Fink et al. 2023 for this system, but they generate no evidence rows on these components. Fink is cited elsewhere in Melange for author-volume-normalized social and press edges. Whether that normalization falls under the REJECTED per-edge volume normalization should be checked in the SNA hub. Flagged, not asserted.

## Research agenda

1. Co-movement validation of the OVERLAP rule. Using payroll histories, compare overlapping vs non-overlapping same-office peers on later joint office moves or joint later employment. Stratify by chamber, container kind and seniority, report window and denominator, and test whether a senior/policy-only restriction (Montgomery 2017 spec) carries the signal.
2. Tie-decay survival study. For pairs whose overlap ended, model the hazard of later joint activity by years since overlap. This calibrates or replaces the 5-year half-life, the co-tenure tau and the 0.8 per-hop factor, all as heuristic constants, never as access probabilities.
3. Container-size ruling input (for Paul, not a recommendation). After the v6 admin exclusion, re-measure degree vs office size, and compare the kind-only kappa operator, the sidecar headcount dilution and a hypergraph-native measure (hypercoreness). Per-edge volume normalization is REJECTED and not a candidate.
4. PageRank stability and predictive holdout. Extend the damping sweep to layer allocation and teleport weights (Baptista 2022), then test whether layer-balanced PageRank beats degree at predicting later senior-title or lobbying status, frozen at Congress t with outcomes at t+1/t+2. LDA is used only as an identity label.
5. Identity-ladder audit. Draw a stratified hand-labelled sample of Quorum-to-payroll matches and auto-merges for precision and recall with CIs (denominator = candidate pairs), including false-match rates at the 6- vs 8-character title threshold. Check whether Kim-Stuckatz-Wolters graph clustering transfers.
6. Source-independence check before noisy-OR enters the build. Measure error correlation across payroll, Quorum and directory sources on labelled ties, and confirm no LDA-derived source contributes to tie strength (D-021).
7. Literature search to fill gaps: congressional staff tenure and turnover studies, revolving-door career-path studies (outcomes only, no influence framing), and organizational-network work on co-worker tie persistence. The vault holds no paper on staffer-staffer tie persistence or Senate staff networks.
