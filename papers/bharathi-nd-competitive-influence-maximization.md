---
type: paper
title: "Competitive Influence Maximization in Social Networks"
authors: ["Shishir Bharathi", "David Kempe", "Mahyar Salek"]
year: ""
venue: ""
doi: ""
category: "Adaptive Influence Maximization"
systems: ["melange-allocation", "melange-influence-engine"]
components: ["melange-influence-engine.competitive-cascade-crn", "melange-influence-engine.competitive-cascade-crn", "melange-allocation.budget-frontier", "melange-allocation.per-topic-allocators", "melange-influence-engine.scenario-runner"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/bharathi-nd-competitive-influence-maximization.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\viral-competitive.pdf"]
sha12: ["0f0236283641"]
tags: [paper, adaptive-influence-maximization]
---

# Competitive Influence Maximization in Social Networks

**Shishir Bharathi, David Kempe, Mahyar Salek (n.d.).** *venue unknown.* Theme: [[themes/adaptive-influence-maximization|Adaptive Influence Maximization]].

> [!abstract] Summary
> Bharathi, Kempe and Salek extend the independent cascade model so that several players compete. Each player seeds a set of nodes. Every node adopts the first colour that reaches it, activation delays are exponentially distributed (which avoids arrival ties), and random tie-breaking applies only when several players choose the same seed node. The paper proves four things. (1) If the opponent's seeds are fixed, a player's expected reach is monotone and submodular in its own seeds, so greedy seeding achieves (1-1/e) of the best response. This holds only for the last player to commit. (2) Uncoordinated equilibria lose at most a factor of 2 against a single controller of all seeds (the price of competition). (3) Pure Nash equilibria need not exist, and the first-mover problem is solved only on disjoint lines. (4) An FPTAS exists for single-player influence maximization on bidirected trees. The paper is purely theoretical: it uses no data, estimates no parameters, and reports no experiments.
> 
> The paper matters because Melange explicitly names it as the basis of its A/B coalition cascade (scenario.py:6-12). The CU colour is A and the anti colour is B, where B is the anti-tier members plus neutrals mobilized by industry money with probability min(0.9, $/400k). The scenario.py docstring (lines 9-12) and a per_topic_coalition comment cite the paper's submodularity result.
> 
> Reading the code turned up three departures.
> - **Tie-breaking:** the cascade advances in synchronous hops and always processes A nodes first, so pro-CU wins every same-hop tie. The paper uses continuous exponential delays to avoid arrival ties and randomizes only between players who choose the same seed.
> - **Frontier greedy:** the budget frontier ranks seeds by marginal spread times a leverage or competitiveness factor, not by raw marginal spread, so the clean (1-1/e) guarantee on spread does not apply. Electoral mode also removes opponent seeds, which Lemma 1 does not cover.
> - **Coalition allocator:** it water-fills money in proportion to single-seed marginals boosted by (1 + 0.8 bpf) and runs no greedy at all.
> 
> The paper also leaves an open question for the system: industry is modelled as a fixed opponent that never responds to CU's choices. The paper gives first-mover algorithms only for a duopoly on disjoint lines and no guarantee on general graphs.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** How do multiple players (companies or innovations) competing by viral word-of-mouth choose initial seed sets in a social network under a generalized independent cascade model? Specifically: best responses to an opponent's strategy, the efficiency of equilibria ("price of competition"), first-mover strategies in a two-player duopoly, and whether single-player influence maximization admits an FPTAS on bidirected trees.

**Theory:** Independent cascade model of diffusion (Goldenberg et al.; Kempe, Kleinberg, Tardos), extended to multiple competing colors. Motivated by Hotelling's model of competition and by competitive facility location and Voronoi games (text lines 47-48). Uses submodular utility systems (Vetta) and submodular maximization via greedy (Nemhauser, Wolsey, Fisher).

**Data:** None. Purely theoretical analysis; no sample, unit of analysis, time period, or N is reported.

**Method:** Formal model definition with exponentially distributed activation delays and random color tie-breaking (Section 2, lines 63-85). Best-response analysis via a deterministic equivalent of the random process and proof of monotone submodularity of expected reach (Lemma 1, Section 3). Approximation via greedy for submodular maximization. Equilibrium efficiency via Vetta's utility-system theorem (Section 3). First-mover analysis on lines via dynamic programming over a cutoff point c, with Lemma 2 (optimal solution cuts each line into equal pieces) (Section 4). FPTAS for bidirected trees via dynamic programming over subtree values G(v,k,q+,q-) with probability rounding at granularity delta = epsilon/n^3 and an inductive error bound (Theorem 3, Section 5).

**Identification:** Not applicable. No empirical identification strategy; results are worst-case approximation and proof-based guarantees.

**Scope:** Population, institution, and era: none. Scope is abstract graphs: directed graphs G=(V,E) with edge activation probabilities; two-player duopoly for first-mover results (Section 4); multi-player game for best response and price of competition; bidirected trees for the FPTAS (with the extension to bounded treewidth claimed but not shown).

## Key findings

- Best response to fixed opponent strategies: player i's expected reach is a monotone and submodular function of S_i, so greedy gives a (1-1/e) approximation to the best response for the last player to commit. Numbers: approximation factor (1 - 1/e). (Section 3, Lemma 1 and Theorem 1 (abstract lines 15-16; intro lines 51-52; proof lines 92-120))
- Price of competition (uncoordinated equilibria vs. single controller with all seeds) is at most 2, via Vetta's utility-system theorem applied to the total activated set, which is monotone submodular in the union of seeds. Numbers: factor 2. (Section 3, Theorem 2 (intro lines 53-54; proof lines 121-128))
- Game has no pure-strategy Nash equilibrium in general, but mixed-strategy Nash equilibria exist. (Section 2 (lines 83-85))
- Two-player first-mover game on disjoint lines: the optimal solution cuts every line into equal-sized pieces, so the red player need only try n*k candidate cutoff values l_i/j (i=1..n, j=1..k), giving a strongly polynomial dynamic program. Numbers: n*k candidate cutoff values; lines of lengths l_1..l_n; k cuts. (Section 4, Lemma 2 and paragraph at lines 149-153)
- FPTAS for single-player influence maximization on bidirected trees, including unequal probabilities in opposite edge directions: a 1-epsilon approximation, with rounding granularity delta = epsilon/n^3 and additive error at most delta*n^3 <= epsilon <= epsilon*OPT. Numbers: delta = epsilon/n^3; approximation 1-epsilon; additive error <= epsilon*OPT. (Section 5, Theorem 3 and the closing paragraph after its proof (lines 176-231))
- Conjecture (not proven): influence maximization is NP-complete even for arborescences directed into a root. The general problem is already known NP-complete (cited to [3,4]) and APX-hard (cited to [3]). (Section 1 (lines 58-60); Section 5 opening (lines 174-175))

## Limitations

- Several proofs are deferred to a full version because of space constraints: Lemma 3 proof, and the extensions of the first-mover line algorithm to directed lines and outdirected arborescences (lines 168-171, 299-300).
- The arborescence NP-completeness result is only conjectured (lines 59-60).
- The FPTAS extension to bounded-treewidth graphs is asserted with a significant increase in complexity but not shown (lines 177-178).
- The game has no pure-strategy Nash equilibria in general (lines 83-85), so equilibrium results rely on mixed strategies, and the reasoning about the price of competition relies on Vetta's theorem as cited.
- Footnote 1 says more realistic marketing models reduce to the model here, citing [3]; this reduction is not argued in the text (lines 40-41).
- The first-mover analysis is restricted to a duopoly with two players and to disjoint or directed line structures (Section 4).
- No empirical evaluation, no real network data, and no computational experiments are reported.

## Short quotes

- "We give a (1−1/e) approximation algorithm for computing the best response to an opponent's strategy" (Abstract, lines 13-16)
- "Our model extends Hotelling's model of competition [6], and is related to competitive facility location and Voronoi games" (Section 1, lines 46-48)
- "we conjecture it is so even for arborescences directed into a root." (Section 1, lines 59-60)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Competitive independent cascade with common random numbers\|Competitive independent cascade with common random numbers]] | method-source | Independent cascade extended to two colours. Each edge gets its activation coin and an exponential delay fixed before the run. Lemma 1 builds a deterministic equivalent in which each edge outcome is fixed in advance and a node takes the colour of the path that reaches it. The paper's random tie-breaking applies only when several players select the same seed node, not to simultaneous arrivals. (Section 2 model (text lines 63-85); Section 3 Lemma 1 proof (lines 92-120)) | Edge draws are keyed by iteration, u and v with no colour in the key, so one draw per edge serves whichever colour reaches it, and a coloured node is never recoloured. This matches the fixed-in-advance edge equivalence. The code drops the exponential delays and runs hop-synchronous spread. B is anti-tier members plus neutral bystanders drawn with probability min(0.9, industry dollars/400000); the code matches this. `fec/engine/scenario.py:42-53; fec/engine/budget_frontier.py:83-94; fec/engine/per_topic_coalition.py:44-57` | Edge-level CRN and first-colour-wins carry over. Timing does not: unit hops replace the paper's continuous delays, so the paper's equivalence does not cover the system's timing. The bystander mapping (0.9 cap, 400000 scale) and the kernel constant 6 have no basis in the paper. | strong | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Competitive independent cascade with common random numbers\|Competitive independent cascade with common random numbers]] | contradicts | The paper avoids ties between simultaneous activation attempts by giving each attempt a continuous exponential delay. It randomizes only between players who select the same seed node. The rel claim that simultaneous arrivals are tie-broken at random is not in the text. (Section 2 (text lines 63-85), the sentence beginning 'To avoid tie-breaking for simultaneous activation attempts') | The cascade is hop-synchronous. The frontier is built A-first and children are appended in processing order, so by induction every A node in a layer is processed before any B node. A node reached in the same hop by both colours goes to A. Same-hop ties are therefore resolved deterministically in A's favour. `fec/engine/scenario.py:45-51; fec/engine/budget_frontier.py:86-92; fec/engine/per_topic_coalition.py:47-55` | The departure from the paper's no-tie design is confirmed by reading the code. Its effect on CU reach is unmeasured. A rerun with B-first or randomized frontier order under the same draws would measure it. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Budget frontier with knee and electoral modes\|Budget frontier with knee and electoral modes]] | qualifies | Lemma 1 gives monotone submodularity of a player's expected reach in its own seeds, holding the other players' seed sets fixed. Greedy over that function gives a (1-1/e) approximation to the best response (Theorem 1). (Section 3, Lemma 1 and Theorem 1 (text lines 92-120); abstract lines 13-16) | Lazy greedy with MAXSEEDS 60 ranks each candidate by max(0, spread gain) times fac. fac is leverage, or the competitiveness multiplier if the LEV source is set to it, and REPFAC times leverage for contest plays. Seeds are gated at random. Contest plays remove B seeds. The bystander set excludes any node already in the A seed set, so the opponent set changes with S, which Lemma 1 does not cover. `fec/engine/budget_frontier.py:65,67,118-136,143,153-171; fec/engine/scenario.py:9-12` | The raw-spread guarantee applies only with a fixed opponent set and A-seed additions. Independent per-seed gating is an expectation over independent masks and keeps submodularity. The fac-weighted ranking is a heuristic, not greedy on spread. Contest removal and the S-dependent bystander exclusion fall outside Lemma 1. MC of 20 adds noise. The bound holds only with fac equal to 1, which is the LEV-off case. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Per-topic flip and coalition allocators\|Per-topic flip and coalition allocators]] | qualifies | Reach is submodular in a player's own seeds, so summed single-seed marginals overstate the joint gain; the (1-1/e) guarantee belongs to greedy, not to individual marginals (Lemma 1, Theorem 1). (Section 3, Lemma 1 and Theorem 1 (text lines 92-120)) | The allocator computes single-seed marginals against the base, multiplies each by (1 + 0.8 times bpf), water-fills 2,000,000 dollars with a 50,000 per-member cap in proportion to those boosted values, runs no greedy, and reports the joint spread of the funded set. The code comment cites submodularity for that report. `fec/engine/per_topic_coalition.py:6 (BUDGET, CAP); fec/engine/per_topic_coalition.py:30-135 (BETA_C, coal_waterfill, allocation loop)` | Submodularity implies the joint report is at most the naive additive sum, so the joint report is supported as inference. The paper gives no guarantee for a proportional water-fill. The bystander set excludes funded seeds, so the spread is not Lemma 1's fixed-opponent function. The figures are one allocation run per topic at MC 20. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Scenario runner (what-if)\|Scenario runner (what-if)]] | qualifies | The best-response guarantee holds only for the last committer. Pure Nash equilibria need not exist and mixed ones do (lines 83-85). Theorem 2 bounds the price of competition at 2 on total activations, not on any one player's share. First-mover results cover only a duopoly on disjoint lines (Lemma 2, lines 149-153). (Section 2 lines 83-85; Section 3 Theorem 2 lines 121-128; Section 4 Lemma 2 lines 149-153) | Scenarios hold industry fixed. Competitor money enters only as exogenous bystander probabilities (industry dollars over 400000, capped at 0.9) and as the comp leg of money_response, which was not opened. Industry never re-optimizes against CU's seeds. The scenario docstring limits the (1-1/e) claim to a follower facing a fixed opponent. `fec/engine/scenario.py:6-12, 42-53` | The fixed-opponent framing holds only if industry commits first and never reacts. FEC filings are public, so if industry reacts, CU becomes a first mover on a general graph, where the paper gives no algorithm or guarantee. The price-of-competition bound says nothing about CU's own share. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Overall verdict: softened, not refuted. The code-structure claims check out against the text and code. The summary misstates the paper's tie-breaking and overstates the Paul-alignment and lines-only claims.
> - Summary issue: Summary says simultaneous arrivals are broken at random. The paper uses continuous exponential delays to avoid arrival ties and randomizes only between players who select the same seed (Section 2).
> - Summary issue: Summary says guarantees exist only for lines once the rival best-responds. The paper gives first-mover algorithms only for a duopoly on disjoint lines and no guarantee on general graphs. Reword accordingly.
> - Summary issue: Summary says the system cites the paper's submodularity result to justify lazy greedy. Only the scenario docstring (9-12) and the per_topic_coalition comment were found citing it. No lazy-greedy citation was located, so that part is unsupported.
> - Summary issue: Summary and bearing 4 say the water-fill is proportional to single-seed marginals. The code uses marginals boosted by (1 + 0.8 bpf). Correct the wording.
> - Summary issue: paul_alignment cites a Chen 2023 note on submodularity failing under linear-threshold contagion. The Chen item is dated 2021 and is a contingency-aware RL influence-maximization paper, not a linear-threshold paper. The 'not diminishing' phrase was not found in the Chen item. Unsupported as written.
> - Summary issue: paul_alignment cites Paul's margin note on Kim et al. 2025 and a Fink note. The item paul_entries array is empty, the ext paul_markup array is empty, and no matching note was located elsewhere in the research items. Cannot verify; remove or source.
> - Summary issue: paul_alignment says the built Monte Carlo is a faithful live-edge implementation. Only the edge draws are faithful. The timing and bystander exclusions are departures (see bearings 1-3).
> - Summary issue: ext coverage says Lemma 4 is given. Lemma 4 is stated as an identity without proof, and Lemma 3 is deferred. Minor, but correct the coverage note.
> - Summary issue: The rel says the fac multiplier is the competitiveness c when COMPET_MULT is on. budget_frontier.py:65 switches on LEV_SRC equal to competitiveness_c; the COMPET_MULT to LEV_SRC link was not checked.
> - Summary issue: Brain IDs D-025, D-104 to D-108 and DE-062 cited in the rel were not re-checked; the flag on brain note lines 80 and 85 is consistent with the June plan text.

## Gaps and flags

- No opponent response: industry is an exogenous bystander-mobilization rate, never a strategic seeder. Under the paper's results, CU's greedy plan is a best response only if industry has already committed and will not react. The system has no first-mover or equilibrium analysis and no sensitivity test of a reacting rival.
- The tie-break is hop-synchronous with A processed first, not random, so pro-CU wins every same-hop contest. The size of this bias is unmeasured. A CRN rerun with randomized or B-first frontier order would bound it.
- The cascade has no timing model. The paper's exponential delays make arrival order depend on path timing, while the system uses unit hops. That affects who wins contested neutrals and cannot be calibrated, because the system has no join-timing data.
- The (1-1/e) claim in scenario.py's docstring is broader than what the code satisfies. It holds only for coalition-mode frontier greedy with fac=1. It does not hold for leverage-weighted ranking, for contest plays that remove opponent seeds, or for the boosted-marginal water-fill coalition allocator.
- The paper gives no empirical calibration: no edge probabilities, no saturation constant, and no money-to-seed mapping. The bystander rule min(0.9, $/400k) and the kernel constant 6 have no source in it.
- The brain notes (topics/2026-06-feature-cosponsor-and-sector-networks-june-network-plan.md:80,85) and scenario.py:9-12 say the coalition side is 'Submodular, so greedy (1-1/e)'. The code departs from the paper's conditions through deterministic A-first tie-breaking, leverage-weighted marginals, contest-seed removal, and boosted-marginal water-fill allocation in per_topic_coalition. The guarantee claim should be narrowed.
- Expected spread is a heuristic model output. It is not a probability that members will join or a measure of access (D-025), and the paper has no legislative data to calibrate it.
- The competitiveness multiplier enters the frontier's greedy ranking through fac. Its premise that dollars buy more in competitive seats is not validated (D-104..D-108), and the paper does not support it.
- The paper proposes neither of the two rejected SNA changes (dropping regular cosponsorship edges; per-edge volume normalization). It does not use LDA as an edge source.

The paper gives the formal basis for treating rival industries as a competing colour. Only the edge draws of the built Monte Carlo are faithful to its live-edge model; the timing and bystander exclusions are departures. The paper says greedy works only when the rival has already moved and will not react. If the rival responds, no pure equilibrium is guaranteed, and first-mover algorithms exist only for a duopoly on disjoint lines. Two caveats follow: industry is never strategic in the build, and the cascade breaks every same-hop tie in CU's favour.

## Related papers

- [[borodin-nd-threshold-models-competitive]] — complements — The scenario docstring cites Borodin for the competitive linear-threshold channel, where submodularity can fail. The item exists.
- [[chen-2021-contingency-aware-influence]] — extends — budget_frontier.py gates seed activation with a contingency draw layered on the cascade. The item exists and is dated 2021 in its metadata.
- [[golovin-2011-adaptive-submodularity-theory]] — complements — The scenario docstring cites Golovin for guarantee scope. The item exists.
- [[cai-2025-competitive-influence-maximization]] — extends — Competitive influence maximization with explicit timing, which the hop-synchronous cascade drops. The item exists; its timing content was not re-read.
- [[moreno-2021-shadowing-shielding-effective]] — complements — Competitive heuristics under voting dynamics. The item exists; the claimed basis for the contested-bills term was not checked in code.
- [[trivedi-2020-efficient-influence-maximization]] — same-method — Single-player independent-cascade seeding by centrality heuristics. The item exists, and the June brain plan names it as the centrality companion to Bharathi.

### Linked from
- [[harrison-2010-introduction-monte-carlo]] — complements — It defines the competitive extension of the independent cascade model whose spread the engine estimates with the Monte Carlo recipe Harrison describes.
- [[kumar-2022-influence-maximization-social]] — complements — Bharathi et al. model competitive IC, which the Melange engine follows in spirit. Kumar's single-colour labels do not model competition, so the two are not contradictory; Kumar simply does not cover the competitive case. Relation changed from contradicts.
- [[zareie-2023-influence-maximization-social]] — complements — Real item (competitive influence maximization in social networks). The survey does not cover competitive IC, so Bharathi is a complement on the two-colour cascade side.
- [[almazaydeh-2026-heuristic-centrality-methods]] — complements — Item exists; title matches (Competitive IM in social networks). Melange's frontier runs competitive two-colour IC, a setting the single-colour benchmark does not test.
- [[chen-2023-complex-contagion-influence]] — complements — fec/engine/scenario.py lines 6 and 11 cite Bharathi, Kempe and Salek for the competitive IC cascade. This paper shows what greedy loses once cascades are threshold-based rather than IC.
- [[kamarthi-2020-influence-maximization-unknown]] — complements — Competitive influence maximization, the setting behind the system two-colour cascade. Kamarthi single-colour IC results do not cover that rival setting. Checked against the title and opening text.
- [[manchanda-2020-gcomb-learning-budget-constrained]] — complements — Title confirms competitive IM, the basis for the system's two-colour cascade. Checked by title and note only; item file exists.
- [[kempe-2015-maximizing-spread-influence]] — extended-by — Competitive follower-vs-fixed-opponent extension of Kempe IC submodularity. Kempe leaves competing cascades open (Conclusions pp.137-138). Item exists; the cited paper text was not re-read here.