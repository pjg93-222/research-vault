---
type: paper
title: "Threshold Models for Competitive Influence in Social Networks"
authors: ["Allan Borodin", "Yuval Filmus", "Joel Oren"]
year: 
venue: ""
doi: ""
category: "Adaptive Influence Maximization"
systems: ["melange-allocation", "melange-influence-engine"]
components: ["melange-allocation.joint-optimizer", "melange-influence-engine.competitive-cascade-crn", "melange-allocation.budget-frontier", "melange-influence-engine.lt-priming"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/borodin-nd-threshold-models-competitive.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\doc_1.pdf"]
sha12: ["ee2aebd28f83"]
tags: [paper, adaptive-influence-maximization, type/paper, theme/adaptive-influence-maximization, evidence/verified]
---

# Threshold Models for Competitive Influence in Social Networks

**Allan Borodin, Yuval Filmus, Joel Oren ().** *.* Theme: [[themes/adaptive-influence-maximization|Adaptive Influence Maximization]].

> [!abstract] Summary
> Borodin, Filmus and Oren (University of Toronto; no year or venue in the text, references run to 2010) ask whether the Kempe-Kleinberg-Tardos linear threshold model can be extended to two competing technologies while keeping the greedy (1-1/e) seed-selection guarantee. Mostly it cannot. A weight-proportional model, where an activated node adopts A with probability equal to A's share of active incoming weight, is not even monotone: adding an A seed moves a root's A-probability from 11/40 to 103/400. A separated-threshold model with per-technology weights and thresholds is monotone but not submodular, and adding a forcing step does not fix this under any tie-breaking rule. With technology-specific weights, the follower problem is NP-hard to approximate within Omega(N^(1/2-eps)). The same reduction refutes a Carnes et al. conjecture. Only the OR model, where each technology spreads independently and a monotone submodular decision function picks the winner at the end, keeps the greedy guarantee, including repeated rounds. For the system as built, the paper is already cited (scenario.py, write_im_sheet.py) as the reason the FLIP side carries no guarantee. That citation is broadly right but loosely mapped. FLIP's pflip = a/(a+b) has the same form as Borodin's weight-proportional adoption split, but it has no random thresholds and no propagation. In a fixed stance state the objective is separable by member; the optimizer checks concavity at run time (concavity_violations) and conditions its exact certificate on that check, so concavity is checked, not proven. The paper bites in two places: modeled-conversion replanning, where the max-normalized neighbour term can make adding pro mass lower other members' pflip, and the coalition frontier's lazy greedy. The frontier keeps one shared kernel for both colours, which stays clear of the Theorem 8 hardness, but its objective includes contest seeds, the seed gate and the leverage factor, and nothing proves those submodular.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** How can the linear threshold model of influence diffusion be extended to two competing technologies so that a player can choose early adopters to maximize its own spread, and does the greedy (1-1/e) approximation for influence maximization still apply to these competitive extensions?

**Theory:** Extends the Kempe-Kleinberg-Tardos linear threshold model with competition. Four variants are proposed: a weight-proportional model where active neighbours of either technology share one threshold and adoption is split by weight; a separated-threshold model with technology-specific weights and thresholds; a forcing extension in which remaining inactive nodes adopt a technology at a final step; and an OR model where each technology spreads independently and a decision function chooses between them at the end, with a repeated-OR variant. Key properties studied are monotonicity and submodularity of the expected A-spread, which determine whether the Nemhauser-Wolsey-Fisher greedy guarantee applies.

**Data:** No empirical data. Theoretical instances: directed graph G=(V,E) with per-technology edge weights in [0,1] whose incoming weights sum to at most 1, initial adopter sets for A and B, a budget k, and node thresholds drawn uniformly from [0,1] (or intervals). Sample sizes are illustrative trees and graphs in Figures 1-2 (for example a 7-node tree for the monotonicity counterexample). The hardness reduction builds a graph with N = O(n^(2 alpha+2)) nodes from a Vertex Cover instance.

**Method:** Formal model definitions; monotonicity and submodularity counterexamples on small trees (Theorems 1-2, 4, 6; Appendices A, C); an inductive coupling proof over fixed thresholds and coin flips for monotonicity of the separated-threshold model (Lemma 3, Theorem 3, Appendix B); a live-edge equivalence argument for submodularity of the OR model (Lemma 2); a reduction from Vertex Cover for NP-hardness of approximation (Theorems 7-8, Appendix D); and application of Mossel-Roch's result for generalized submodular thresholds (Theorems 10-11).

**Identification:** Not applicable: proof-based theory paper with no causal or empirical identification strategy.

**Scope:** Abstract directed weighted social networks of arbitrary size; no population, institution or era. Main text uses two competing technologies, with a claim (no proof given) that results extend to several. Time is discrete steps with no calendar period. Results are worst-case or existence statements, not average-case.

## Key findings

- Weight-proportional competitive linear threshold model is not monotone: adding an A seed can lower the spread of A. (Node r activation probability for A: 11/40 before and 103/400 after adding a seed (0.275 vs 0.2575); Section 2, Theorem 1; proof in Appendix A, Theorem 12 with Figure 1(a))
- Weight-proportional model is also not submodular. (For S={w} vs T={w,y} (probabilities 3/10 to 17/60 for S; 7/10 to 17/50 for T); Section 2, Theorem 2; proof in Appendix A, Theorem 13 with Figure 1(b))
- Separated-threshold model is monotone: adding A seeds never reduces the probability a node becomes A-active at any step, and never raises the probability of B. (None; qualitative inequality alpha-hat at step t >= alpha at step t; Section 3, Theorem 3; proof in Appendix B, Lemma 3 and Theorem 14)
- Separated-threshold model is not submodular in general; the claimed counterexample is only in a full version not included in this text. (None; Section 3, Theorem 4 (stated without proof here))
- Adding a forcing step keeps monotonicity-type properties but does not restore submodularity; the result holds for any tie-breaking and any forcing rule. (Forced-model counterexample: A-probability at root 0 for S and 0 for T, but 1 for T with the added seed, violating submodularity; Section 4, Theorem 5 (monotonicity) and Theorem 6 (non-submodularity); counterexample in Appendix C, Figure 2)
- Separated-threshold competitive problem is NP-hard to approximate within a factor better than N^(1/2 - eps), for any eps > 0. (Ratio bound Omega(N^(1/2 - eps)); gap exponent (alpha+3)/(2 alpha+2) tends to 1/2 with beta = alpha+2; Section 5, Theorem 7; reduction in Appendix D, Theorem 16 from Vertex Cover)
- Same hardness applies to the separate-edge-weight Wave Propagation model of Carnes et al., giving a negative answer to their conjecture that the process remains monotone and submodular. (Same Omega(N^(1/2 - eps)) bound; Section 5, Theorem 8 and the paragraph before it; reference to Carnes et al. 2007)
- OR model (technologies spread independently, then a decision step with monotone submodular decision functions) is monotone and submodular in the A-spread. (None; Section 6, Lemma 1 (monotone) and Lemma 2 (submodular))
- Greedy (1-1/e-eps) approximation is achievable for the OR model and its generalized version with any monotone submodular node-level functions. (Approximation factor 1-1/e-eps in polynomial steps; Section 6, Theorem 9 and Theorem 10 (via Mossel-Roch [8]))
- Repeating the OR process for several iterations (e.g. daily spread in an election, with edge weights possibly changing between iterations) with one final decision step remains monotone and submodular. (Number of iterations l is arbitrary; no numeric bound; Section 6.1, Theorem 11)
- Authors argue that no single true model exists and suggest game-theoretic settings where competing firms bid for strategic nodes. (None; Section 7, Conclusions)

## Limitations

- Theorem 4 (separated-threshold non-submodularity) is stated without proof; the counterexample is said to appear only in a full version on the authors' websites.
- Proof of Lemma 3 is omitted for lack of space and deferred to the full version.
- Main text treats two technologies; extension to several is asserted, not proven.
- Hardness results are asymptotic worst-case bounds (N^(1/2 - eps)); they do not characterize typical instances or give an algorithm.
- Greedy guarantee applies only to the OR model (and generalized OR) under monotone submodular decision functions; the paper concedes most other models are unfit for the Nemhauser et al. technique and leaves open whether efficient approximations exist.
- Model is one-shot diffusion with uniformly random thresholds; partial-knowledge intervals are mentioned but not analyzed.
- No empirical validation of any model on real networks or election data.

## Short quotes

- "the original greedy approach cannot be used" (Abstract)
- "there is no single true model" (Section 7, Conclusions)
- "The process is not submodular in general." (Section 3, after Theorem 3)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-allocation#Joint CU budget optimizer\|Joint CU budget optimizer]] | qualifies | In the weight-proportional competitive LT model (Eq. 1) the A-spread is not monotone (Theorem 1: root A-probability 11/40 before and 103/400 after adding one A seed, Appendix A Theorem 12) and not submodular (Theorem 2). The Nemhauser-Wolsey-Fisher greedy bound does not transfer to that model. (Section 2, Eq. 1, Theorems 1-2; Appendix A Theorems 12-13, Figure 1) | pflip = a/(a+b) (flip_model.py 92-97) has the form of Eq. 1, but a and b each add a money term to WP*nA/mAt, so it is not the same model. Stance is fixed inside a run, so the objective is separable by member. optimize.py checks concavity at run time (concavity_violations) and labels only the fixed-state run as fixed_state_lazy_greedy. A modeled conversion re-runs topic_setup, where mAt = max(nA) (flip_model.py 138-140), so one conversion can lower another member's WP*nA/mAt term. Such a run is labeled modeled_threshold_replanning (optimize.py 190) and carries no certificate. `fec/engine/flip_model.py:92-97, 138-140; fec/engine/optimize.py:3-12, 120-123, 142-144, 190` | Structural analogy only. Borodin non-monotonicity runs through multi-step propagation under random thresholds, which pflip does not have. The conversion-path concern applies only when conversion_reachable is true. Whether the mAt renormalisation lowers total winnability after a conversion is not measured in this text. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Competitive independent cascade with common random numbers\|Competitive independent cascade with common random numbers]] | supports | With technology-specific edge weights, the separated-threshold competitive problem (Theorem 7) and the Carnes wave-propagation variant with technology-specific probabilities (Theorem 8) are NP-hard to approximate within Omega(N^(1/2-eps)) by a Vertex Cover reduction (Appendix D, Theorem 16). This refutes the Carnes et al. conjecture for that case. (Section 5, Theorems 7-8; Appendix D, Theorem 16, Figure 3) | The engine builds one kernel P = build_P(adj) and uses it for both colours (budget_frontier.py 77, per_topic_coalition.py 37, scenario.py 39). Pro and anti frontiers draw the same keyed splitmix value per (iteration, edge) (budget_frontier.py 83-94; per_topic_coalition.py 44-55), and the first colour to reach a node keeps it. The paper states its hardness for models with separate per-technology edge weights, so the shared kernel sits outside that stated scope. The paper does not describe the Bharathi-Kempe-Salek weights, so calling them shared-weight is the analyst reading, not the paper. `fec/moneyball_core.py:555-562; fec/engine/budget_frontier.py:77, 83-94; fec/engine/per_topic_coalition.py:37, 44-55; fec/engine/scenario.py:3-12, 39` | Holds only while the kernel stays shared. Giving anti-side diffusion its own edge weights or physics, such as cpf_pressure on the B side only (moneyball_core.py 546), would move the engine into the stated hardness scope. Tie-breaking by frontier order is not analysed in this paper. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Budget frontier with knee and electoral modes\|Budget frontier with knee and electoral modes]] | qualifies | Most natural competitive extensions lose submodularity. Monotonicity can survive while submodularity fails (Theorems 3-6). The greedy (1-1/e) guarantee is shown only for the OR model with monotone submodular decision functions (Theorems 9-11). Section 7 leaves other models open. (Sections 3-4 and 6, Theorems 3-6 and 9-11; Section 7) | budget_frontier.py runs a CELF-style lazy greedy on a heap. spread() applies the seed gate (line 128) and removes activated contest seeds from the anti set (line 131). marg() multiplies the raw gain by fac, which is REPFAC times leverage for contest seeds (line 143, applied at 156), and floors at zero. audit_frontier.py 27-35 counts marginal inversions on clamped spread and passes up to 35 percent of marginals as MC noise (tolerance 0.35 times the marginal count). `fec/engine/budget_frontier.py:118-134, 143, 153-167; fec/audit/audit_frontier.py:27-35` | Applies to electoral mode (contest and defend seeds). This paper gives no submodularity result for the electoral objective. Any coalition-mode greedy guarantee comes from the competitive IC literature cited to Bharathi et al. (Borodin ref 1), not from this paper, so coalition-only mode is not covered here. Separating MC noise from structure needs a read-only check of raw marginals under common random numbers. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Linear-threshold priming (cheapest tip)\|Linear-threshold priming (cheapest tip)]] | suggests-extension | In the separated-threshold competitive LT model a node is activated by whichever technology weighted active neighbours first exceed that technology own threshold. Adding A seeds never raises the probability that B propagates (Lemma 3 part 1, used in Theorem 3), but the process is not submodular in general (Theorem 4, counterexample only in an unincluded full version). (Section 3, Theorems 3-4; Appendix B, Lemma 3) | lt_priming.py prime() (96-103) sets theta from the tier ladder (base THETA runs from 0.0 for Ally to 3.0 for Opponent, lines 19-21), adds a committed-opponent term and a money-captured term, and sums pressure over pro sources only (proset). Anti neighbours exert no competing threshold pressure. The recruit search (131-140) picks cheapest recruits per target rather than optimising a seed set, so submodularity is not invoked. `fec/engine/lt_priming.py:19-21, 96-103, 131-140` | Thresholds here are not normalised to Borodin uniform [0,1] draws (base ladder 0.0 to 3.0 plus additive terms). Adding an anti-side threshold would be a model change for Paul to decide. Borodin monotonicity result is for the separated-threshold model and says nothing about this solver recruit lists, for which the paper gives no guarantee. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: summary_md says the FLIP objective is separable and concave so the exact certificate holds. The code only checks concavity at run time (concavity_violations in optimize.py) and conditions the certificate on that check. Reword as checked, not proven.
> - Summary issue: summary_md and flags say FLIP is not any of Borodin four models. It is closest to the Section 2 weight-proportional split but has no thresholds or propagation, so the accurate claim is that it is not that model either.
> - Summary issue: Bearings 2 and 3 use Bharathi-Kempe-Salek characterisations (shared-weight family; fixed-opponent proof excluding contest seeds) that are not in the Borodin text. Removed as paper-based support.
> - Summary issue: paul_alignment claims (complex-contagion position on lt-priming; greedy plus CELF as operational layer) have no source in this item: paul_entries is empty and n_annots is 0. Treat as unverified.
> - Summary issue: related cai-2025 was marked extends and described as taking up Borodin open question. The Cai text never cites Borodin or uses submodularity, so the relation is changed to complements.
> - Summary issue: related bharathi why overstated that Bharathi follower problem keeps the greedy guarantee. Borodin only says greedy has been applied to competitive extensions [2,1]. Wording corrected.
> - Summary issue: Lt-priming theta range in the bearing was 0.6 to 3.0; the base ladder runs 0.0 (Ally) to 3.0 (Opponent). Corrected in the bearing.

## Gaps and flags

- write_im_sheet.py:26 and scenario.py:11-12 call FLIP competitive Linear Threshold (Borodin), but flip_model.pflip is a one-step weight-proportional ratio with no random thresholds and no propagation. The citation names the right failure (no guarantee) but the wrong model. Only the conversion-replanning path is threshold-like.
- There is no test of whether a modeled conversion lowers total winnability through the mAt = max(nA) renormalization in flip_model.py:140, which is the Borodin-Theorem-1-style non-monotone channel.
- audit_frontier.py:27-35 tolerates up to 35 percent marginal inversions as MC noise and reads clamped marginals. Nothing tests submodularity of the electoral-mode objective (contest seeds shrink the opponent set; fac multipliers), which no cited proof covers.
- The frontier's same-round tie between colours is resolved by list-processing order, not at random. No cited paper establishes submodularity under this rule.
- LT priming ignores anti-neighbour competition entirely. A separated-threshold (Borodin Section 3) or OR-model (Section 6) treatment of opposing pressure is not built.
- Borodin's OR model with a final decision step (Section 6, Theorem 11 for repeated rounds) is the one competitive LT variant with a greedy guarantee, and the system has no counterpart to it.
- Citation-accuracy flag: the repo cites Borodin et al. for FLIP being competitive LT (fec/surfaces/write_im_sheet.py:26; fec/engine/scenario.py:11-12). The built FLIP is closest to the Section 2 weight-proportional split but has no thresholds or propagation, so it is not that model either. Borodin's results support the no-guarantee label by analogy, not by direct application.
- Paper is pure theory with no data. Its election examples (forcing as no-abstention voting, Section 4; daily spread in an election race, Section 6.1) are motivation only and give no evidence about legislators or cosponsorship.
- No conflict with the rejected SNA proposals: the paper does not argue for dropping cosponsorship edges or per-edge volume normalization. Its influence language concerns abstract diffusion, not revolving-door edges.
- Theorem 4 and Lemma 3 proofs are deferred to a full version that is not in this text, so the separated-threshold non-submodularity counterexample is unseen here.

Paul has no annotations on this work (n_annots 0), so there is no takeaway of his to check and nothing here asserts his position. The repo's one-line gloss of Borodin is slightly stale: flip_model is now a ratio model.

## Related papers

- [[bharathi-nd-competitive-influence-maximization]] — cites — Borodin cites Bharathi-Kempe-Salek [1] among works that applied the greedy approach to competitive extensions. The system competitive cascade follows that family, and Borodin results mark where the guarantee stops for LT variants. Borodin does not say Bharathi follower problem keeps the guarantee.
- [[golovin-2011-adaptive-submodularity-theory]] — complements — Borodin does not cite Golovin-Krause. The link is through the code: optimize.py uses a Golovin-Krause data-dependent upper bound as a diagnostic. Borodin shows which competitive objectives lose the submodularity such bounds need.
- [[cai-2025-competitive-influence-maximization]] — complements — Competitive influence maximization in voter dynamics with timing and network heterogeneity. Its text does not cite Borodin or discuss submodularity, so it is a neighbouring competitive-IM study on a different diffusion model, not a follow-up to Borodin.
- [[moreno-2021-shadowing-shielding-effective]] — complements — Heuristics for competitive continuous influence maximisation under voting dynamics. A practical alternative when Borodin-style results remove greedy guarantees.
- [[chen-2023-complex-contagion-influence]] — complements — Learned policies for complex-contagion seeding, which the text describes as non-submodular. Fits Borodin finding that threshold competition breaks submodularity.
- [[almazaydeh-2026-heuristic-centrality-methods]] — complements — Compares greedy CELF++ with sampling and centrality heuristics on cost-effectiveness. Relevant once Borodin shows greedy has no guarantee in most competitive LT variants.

### Linked from
- [[ma-2022-adversarial-attack-graph]] — complements — Title matches a competitive threshold-model paper, where submodularity can fail. The Ma et al. result gives the single-campaign condition. Item title checked only; the content was not re-read here.
- [[chen-2023-complex-contagion-influence]] — complements — fec/engine/scenario.py lines 11-12 cite Borodin et al. for competitive LT, where submodularity can fail. This is the competitive threshold counterpart to the K-complex model here.