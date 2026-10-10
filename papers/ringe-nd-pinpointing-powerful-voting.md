---
type: paper
title: "Pinpointing the Powerful: Co-voting Network Centrality as a Measure of Political Influence (Online Appendix)"
authors: ["Nils Ringe", "Steven L. Wilson"]
year: ""
venue: "Legislative Studies Quarterly (supplementary online appendix; article DOI not visible in the text)"
doi: ""
category: "Social Network Analysis"
systems: ["melange-influence-engine", "melange-sna", "moneyball-rank-pillars"]
components: ["melange-influence-engine.cascade-kernel", "melange-sna.signed-coalitions", "moneyball-rank-pillars.network-centrality-influencescore"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/ringe-nd-pinpointing-powerful-voting.docx]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\lsq12129-sup-0001-suppinfo.docx"]
sha12: ["172f9f14d970"]
tags: [paper, social-network-analysis]
---

# Pinpointing the Powerful: Co-voting Network Centrality as a Measure of Political Influence (Online Appendix)

**Nils Ringe, Steven L. Wilson ().** *Legislative Studies Quarterly (supplementary online appendix; article DOI not visible in the text).* Theme: [[themes/social-network-analysis|Social Network Analysis]].

> [!abstract] Summary
> This is the online appendix to Ringe and Wilson's paper on co-voting centrality. It holds a proof, simulations and descriptive tables from the European Parliament. The authors build a network from pairwise phi correlations of members' binary voting records. They score each member by an absolute-value (sign-blind) centrality: a member followed reliably by allies (positive phi) or reliably opposed by opponents (negative phi) both count toward that member's centrality.
> 
> **Proof.** For a single-party legislature, they state that the leader's centrality is at least as high as any follower's, with the condition reducing to q2 < 1 and q3 < 1. The algebra is omitted from the appendix text (line 344), so the proof cannot be checked here. The inequalities are weak (greater than or equal), and the text's wording is inconsistent between "greatest" for four voters (line 352) and "greater than or equal" for three.
> 
> **Simulations.** There were 1,000 networks per complexity class, with 100 legislators and 1,000 voting rounds.
> - The absolute-value measure separates the cue-provider from followers and from "majoritarians" (members who usually vote with the majority).
> - The traditional signed measure can rank majoritarians above the true cue-provider when a third group follows at 35% (Figure 4).
> - The measure correlates highly with closeness and weighted degree (no coefficient reported), but only about 0.25 with betweenness.
> - Moderate and complex networks lose structure when converted to voting data.
> - The authors say the results are "highly dependent on parameterization".
> 
> **Empirical side.** The data cover 897 MEPs with centrality scores (mean .283). The parliamentary term is not stated in the appendix.
> 
> **Why it matters here.**
> - Melange has one co-voting layer: the signed vote backbone in the Draft Systems R script, plus its party-controlled Option C. That layer found the raw signed backbone was about 95% party, then dropped the negative side. The paper never discusses party clustering or backbone construction, so the majoritarian explanation for the first finding is the analyst's hypothesis. The paper's use of negative correlations suggests negative ties may carry cue-provider information, which is an inference, not a critique of Melange.
> - The paper does not address the June 2026 retirement of cosponsor centrality from the Influence pillar or the system's two-axis rule; any link is analyst interpretation.
> - None of this transfers directly. The evidence is analytic and simulated, with European Parliament data, and it concerns co-voting, not cosponsorship.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Can network centrality computed from co-voting records (correlations between members' voting records) identify the actual cue-provider or leader in a legislature, and in particular does a proposed absolute-value centrality measure separate cue-providers from followers and majoritarians where the traditional centrality approach cannot? (Inferred from the appendix; the main question is stated in the published article, which is not in this text.)

**Theory.** Leader or cue-provider theory: a leader whose followers vote with her some percentage of the time (ally followers, opponent followers, or others at other rates). The appendix argues the leader should have the highest network centrality in a co-voting network built from phi correlations of voting records (lines 318-353). Majoritarians are members who vote with the majority; the traditional centrality approach can rank majoritarians above the actual cue-provider (Figure 4 text, line 303).

**Data.** Empirical data (Table 1, line 12-120): 940 MEP observations (centrality N = 897 after network construction; left-right distance to EP median N = 868; pro/anti-EU distance to EP median N = 872). Unit is the MEP. Variables include centrality (mean .283, SD .138, range .006-.502), party leader (mean .05), committee leader (.09), EP leader (.03), coordinator (.23), reports drafted (mean 1.05), reports amended (6.73), opinions (.72), motions (6.82), speeches (29.07), questions (20.07), declarations (.58), tenure (1.58), party size (172.01), female (.30). Period of the European Parliament data is not stated in the appendix text. Simulated data (Figures 1-6, lines 289-313): 1000 simulated networks per network class (simple, moderate, complex); 1000 simulations; 100 legislators; 1000 rounds of voting in the Figure 5 runs. Figure 6 uses three parameterizations: US (1 cue-provider, 1 majoritarian, 40 allies, 40 opponents, 20 others at 50%), Supermajority (1 cue-provider, 1 majoritarian, 67 allies, 33 opponents), and EP (1 cue-provider, 1 majoritarian, 37 allies, 25 opponents, 38 others at varied rates, roughly the seven-party EP seat allotment).

**Method.** Centrality is built from pairwise phi correlations of binary voting records (the 2x2 contingency table underlying phi referred to at line 331 is missing from the extracted text; Table 1 is descriptive statistics). The appendix uses an absolute-value version of this centrality, referred to as the "posterior" measure, which is compared with the traditional (signed) centrality calculation. Analytic method: Hypothesis 1 (leader centrality >= follower centrality) is stated as proved for a three-voter legislature (the algebra is omitted, line 344) (leader plus two followers) with follow probabilities q2 and q3 and leader Yes-probability p; the condition reduces to q2 < 1 and q3 < 1 (lines 319-345). The result is extended to n voters by a term-by-term comparison, arguing that each phi term depends only on p and the relevant follow probabilities, so it is unchanged as voters are added (lines 347-353). Simulation method: Monte Carlo generation of cueing networks in three complexity classes, computing the posterior centrality from simulated voting records and correlating it with betweenness, closeness, and weighted degree (Figure 5); and t-tests of cue-provider centrality against the maximum follower and a majoritarian across rounds of voting (Figure 6). Figures 1-4 show cue-provider centrality as a function of follow probabilities.

**Scope.** Population: members of the European Parliament (party groups named in Table 2: EPP, EPP-ED, PES, ALDE, G/EFA, EUL/NGL, IND/DEM, UEN), plus stylized simulated legislatures (US, supermajority, and EP-style). Institution: European Parliament (empirical) and abstract legislatures (theory). Era: not stated in the appendix text. The theoretical result is stated for single-party legislatures under the stated assumptions.

## Key findings

- Under the assumptions, in a three-voter legislature the leader's centrality is greater than or equal to each follower's centrality; the condition reduces to q2 < 1 and q3 < 1, which hold by inspection. Numbers: Conditions: q2 < 1 and q3 < 1 (follow probabilities are probabilities).. (Complete Proof of Leader Centrality, Hypothesis 1 proof (lines 318-345))
- The three-voter result is extended to n voters for the single-party case: the cue-provider has greater or equal centrality than any follower regardless of exogenous parameter values. The text's wording is inconsistent: line 352 says 'greatest' for four voters, and lines 344-345 give 'greater than or equal' for three. Numbers: Proof given for 3, 4, and arbitrary n voters.. (Generalization to n-voters (lines 347-353))
- Posterior absolute-value centrality recovers the true network centralities of simulated cueing networks; it is highly correlated with closeness and weighted degree even in complex networks, and only weakly correlated with betweenness. Numbers: Betweenness correlation about 0.25; closeness and weighted degree described as 'highly correlated' (no coefficient reported); 1000 simulations, 100 legislators, 1000 rounds of voting; 1000 networks per class.. (Figure 5 text (line 308); Figure 5 caption (line 307))
- Simple networks are recovered most accurately; moderate and complex networks less so, because network structure is lost when networks are converted to voting data. Numbers: No coefficients reported by class.. (Figure 5 text (line 308))
- Cue-provider centrality is significantly higher than the maximum follower and the majoritarian; significance grows with the number of voting rounds, and is described as comparable to available EP data at 1,000 rounds. Numbers: Three parameterizations (US, Supermajority, EP); t-scores are described as significant but no t-values are given in the text.. (Figure 6 text (line 309))
- With allies and opponents following at 65% and 35%, cue-provider centrality is clearly higher than majoritarians and followers across the 35%-65% range, though the gap is less pronounced than at 80%/20%. Numbers: Parameterization 65%/35% versus 80%/20% in the published article.. (Figure 2 text (line 296))
- With allies at 80% and opponents at 35%, the absolute-value centrality yields systematically higher cue-provider centrality, while the traditional approach cannot distinguish the cue-providers. Numbers: Allies 80%, opponents 35%.. (Figure 3 text (line 297))
- With a third group following at 35%, the traditional calculation makes majoritarians systematically more central than the actual cue-provider; the absolute-value measure corrects this. Numbers: Allies 80%, opponents 20%, others 35%.. (Figure 4 text (line 303))
- Cue-provider centrality is maximized when follow probabilities approach 0 or 1 and minimized when both are 50%. Numbers: Follow probabilities on the x and y axes; minimum at (50%, 50%).. (Figure 1 text (line 291))
- Descriptive: the top-ranked MEP overall is Braghetto (EPP) at centrality 0.5019; the top 17 are from the EPP; the 18th is Sakalas (PES) at 0.475. Group leaders: PES Sakalas 0.475; ALDE Duff 0.4441; G/EFA Onesta 0.2046; EUL/NGL Svensson 0.1737; IND/DEM Wojciechowski 0.3248; UEN Masiel 0.3893. Numbers: Overall mean centrality .283 (Table 1); EPP-ED block lists 5 ranks with the second entry labelled '5' (apparent labeling error in the table, lines 157-171).. (Table 2 (lines 125-286))

## Limitations

- Results are described as highly dependent on parameterization; three sample parameterizations are used (lines 309).
- Betweenness centrality correlates only about 0.25 with the posterior measure, so the measure does not recover betweenness well (line 308).
- Moderate and complex cueing networks lose network structure when converted to voting data, reducing recovery accuracy (line 308).
- The proof gives weak inequalities (greater than or equal to), the algebra is omitted (line 344), and the text's wording is inconsistent between 'greatest' and 'greater than or equal' (lines 344-353).
- The proof is restricted to a single-party legislature and to the stated assumptions (independent follow probabilities, fixed p); multi-party extensions are not proved here (line 352).
- No t-scores or correlation coefficients are reported for closeness and weighted degree; significance is asserted, not quantified (lines 308-309).
- The empirical sample period, the Parliament term, and the construction of the co-voting network are not stated in this appendix text.
- The appendix refers to a published article for the main figures and the 80%/20% parameterization, which is not in this text; claims beyond the appendix cannot be checked here.
- Table 2 contains a rank-label error (the second EPP-ED entry is labelled '5').

## Short quotes

- "The results are highly dependent on parameterization" (Figure 6 text (line 309))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-sna#Signed-network coalition partition\|Signed-network coalition partition]] | qualifies | Signed (traditional) co-voting centrality cannot separate the cue-provider from majoritarians; with a third group following at 35 percent it makes majoritarians systematically more central than the actual cue-provider. The absolute-value measure corrects this. In simulation, cue-provider centrality is lowest when both follow probabilities are 50 percent and highest as they approach 0 or 1. (Figure 4 text (line 303); Figure 3 text (line 297); Figure 1 text (line 291)) | Vote backbone built with R backbone sdsm(signed=TRUE, alpha 0.05, BH) on a member by (vote,side) bipartite matrix, so the projection counts agreements. Brain vote-edges section (2026-06-25): the raw signed backbone is about 95 percent party-aligned (positive edges 93 percent same-party, negative edges 99 percent cross-party). Option C kept cross-party positive edges only and fed Vote Affinity into the Signed Coalitions sheet as a 30 percent booster. fec/network/signed_coalitions.py partitions the signed mention layer (member_references.json), not votes. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/_vote_backbone.R:2,23; C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/network/signed_coalitions.py:6-8,24; C:/Users/pgoff/Documents/brain/topics/2026-06-feature-cosponsor-and-sector-networks-june-network-plan.md:52-57` | Paper is single-party cue-taking with simulated independent follow probabilities plus European Parliament MEP data, not US House or party-aligned backbones. The paper gives no account of why signed backbone edges cluster on party, so the majoritarian explanation is the analyst hypothesis, not a paper claim. That dropping negative ties loses cue-provider information is an inference from the paper's use of negative correlations, not a paper critique of Melange. | moderate | VERIFIED-SOFTENED |
| [[systems/moneyball-rank-pillars#Cosponsor-network InfluenceScore (computed, weight 0)\|Cosponsor-network InfluenceScore (computed, weight 0)]] | qualifies | In a three-voter single-party legislature the leader's centrality is at least each follower's, with the condition reducing to q2 below 1 and q3 below 1; the result is extended to n voters by a term-by-term argument. In simulation, absolute-value centrality correlates highly with closeness and weighted degree (no coefficients reported) and about 0.25 with betweenness. (Complete Proof of Leader Centrality, lines 318-345; n-voter generalization, lines 347-353; Figure 5 text, line 308) | InfluenceScore = 0.60 weighted out-degree + 0.25 betweenness + 0.15 eigenvector on the cosponsor network (brain line 25). Its centrality weight was set to 0 on 2026-06-25 (AB13 0.65 to 0) as direction-blind; Influence is now committee-only (AB12 set to 1.0). compute_pillars.py defines Influence as committee influence divided by the cross-member maximum. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/engine/compute_pillars.py:11,210,250; C:/Users/pgoff/Documents/brain/topics/2026-06-feature-cosponsor-and-sector-networks-june-network-plan.md:25,65-67` | The proof is in signed phi terms (leader-follower correlation at least follower-follower), while simulations use the absolute-value measure; the appendix does not show the inequality holds for that version. The algebra is omitted (line 344). The result concerns co-voting correlations, not cosponsorship weights. The paper's 0.25 is a simulated correlation with betweenness, not a weight, so it says nothing about InfluenceScore's 0.25 betweenness weight. Any revived centrality rank is a heuristic model output, never a closeness, access or influence probability (D-025). | weak | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Cascade activation kernel build_P and partisan edge factors\|Cascade activation kernel build_P and partisan edge factors]] | qualifies | Converting cueing networks into voting data loses network structure: simple networks are recovered best, moderate and complex networks less well, and betweenness recovery is only about 0.25 correlation. (Figure 5 text and caption, lines 307-308) | build_P computes p(u to v) = w times cpf times sigma_v divided by (w times cpf times sigma_v + 6), a saturation kernel over cosponsorship-weighted adjacency with partisan factors cpf_diffusion and cpf_pressure. The kernel does not infer edges from roll-call co-voting, so the paper's conversion loss does not apply directly. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/moneyball_core.py:543-562` | The paper's result concerns recovering node centralities from simulated voting records; it says nothing about cascade edge probabilities or the saturation constant 6. The analyst's claim that kernel edges come from cosponsorship weights was not checked, because load_network was not opened in this pass. Relevance is only as a caution against calibrating kernel edges from roll-call agreement. The evidence is simulation, not US data. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: ext says Table 1 is the 2x2 contingency table underlying phi. In the extracted text Table 1 is descriptive statistics; the 2x2 table referred to at line 331 is missing from the extraction.
> - Summary issue: ext and rel describe Hypothesis 1 as proved algebraically. The appendix omits the algebra (line 344: we won't replicate that algebra), so the proof cannot be checked from this text.
> - Summary issue: The proof inequality (lines 327-329) is stated in signed phi terms. The absolute-value posterior measure used in simulation is not shown to satisfy it; neither ext nor rel flags this.
> - Summary issue: rel summary_md says the paper offers a mechanism for the raw signed backbone's 95 percent party clustering. The paper never discusses party clustering or backbone construction; that link is the analyst's hypothesis.
> - Summary issue: rel summary_md and paul_alignment say the paper supports the June 2026 retirement and the two-axis rule. The paper does not address influence versus alignment axes or Melange's rules; this is analyst interpretation.
> - Summary issue: rel bearing 2 transfer_conditions says the paper implies the 0.25 betweenness weight is least recoverable. The paper's 0.25 is a simulated correlation with betweenness, not a weight; removed.
> - Summary issue: rel related entries for aref-2021 and aref-2020 are labelled same-data with roll-call networks. Both Aref and Neal papers use bill cosponsorship networks; relations corrected to complements.
> - Summary issue: rel related entry for battaglini says Katz-Bonacich; the paper says Bonacich centrality and never uses the term Katz.
> - Summary issue: ext key_finding says the text states greater for n = 4. Line 352 says greatest for four voters, and lines 344-345 give greater than or equal for three voters; the wording is inconsistent in the text itself.
> - Summary issue: rel summary_md says the measure tracks closeness and weighted degree closely; the text reports no coefficients for them, only high correlation in words (line 308).
> - Summary issue: paul_alignment cites a Sotoudeh highlight. The quoted sentence is in the Sotoudeh text, but that item has no paul_entries, so whether Paul highlighted it was not verified.
> - Summary issue: Figure 5 caption reads completeness where closeness is meant (typo in the paper); the analyst's bearing correctly uses closeness, matching the body text at line 308.

## Gaps and flags

- No component computes a leadership or cue-provider score from roll-call co-voting. The roll-call ledger exists (fec/legislation/collect_official_rollcall_events.py), but nothing turns it into a co-voting network apart from the June workbook-era Draft Systems _vote_backbone.R affinity layer.
- The vote-backbone affinity layer discards negative (reliable-opposition) ties. The paper's use of negative correlations suggests these carry cue-provider information (an inference), so the system cannot tell a reliable cross-pressure leader from a maverick, a confound the brain itself records (the Massie/Gosar/Roy negatives).
- Nothing tests whether the raw signed backbone's collapse onto party is a majoritarian artifact rather than true party cueing; the paper does not discuss party clustering, so the majoritarian explanation is the analyst's hypothesis.
- The centrality retirement was judged on CU alignment (opponents rising), not on any leadership or effectiveness criterion. No test checks whether sign-blind centrality predicts effectiveness (CEL LES, bill advancement) on the influence axis.
- Flag: The paper's title frames co-voting centrality as 'a measure of political influence'. If adopted, any centrality rank must be worded as a heuristic model output, never as closeness, access or an influence probability (D-025).
- Flag: The paper does not argue for either REJECTED SNA proposal (dropping regular cosponsorship edges, or per-edge volume normalization). Phi correlation is a per-pair standardized agreement on votes, not a reweighting of cosponsor edges, so it must not be cited as support for per-edge volume normalization.
- Flag: The component description for melange-sna.signed-coalitions says 'signed backbone' (vote-based), but fec/network/signed_coalitions.py:6-8 partitions the signed mention layer (member_references.json). The vote backbone lives only in Draft Systems/_vote_backbone.R and the workbook. The component inventory conflates the two.
- Flag: The empirical evidence comes from European Parliament MEPs (term not stated) and the rest is simulation. Any claim transferred to the US House or Senate goes beyond this evidence. The proof gives weak inequalities only and is restricted to a single-party legislature.
- Flag: The empirical leader ranking pools 897 MEPs across party groups. Table 2 gives within-group leaders, but the overall rank is UNSTRATIFIED across the party fault line (the top 17 are all EPP).

Paul has no annotation on this paper. His centrality-component position is that more research on forms of centrality is needed (Ardekani), that directionality matters (Haythornthwaite), and that eigenvector centrality is not a strong indicator of bill passage (per Sotoudeh; whether he highlighted it was not verified). The paper does not address Melange's influence-versus-alignment axes or the June 2026 retirement. His notes are not stale. The system's June decision to keep only positive cross-party vote-backbone edges is a choice his notes never address.

## Related papers

- [[aref-2021-identifying-hidden-coalitions]] — complements — Corrected from same-data: Aref and Neal (Sci Rep 2021) partition signed House networks whose edges come from bill cosponsorship, not roll-call votes. Both treat signed ties as informative for coalitions; Ringe and Wilson score nodes from signed voting correlations.
- [[aref-2020-detecting-coalitions-optimally]] — complements — Corrected from same-data: Aref and Neal (Sci Rep 2020) partition signed House and Senate networks built from bill cosponsorship, not co-voting. It is a coalition-level partition, while Ringe and Wilson score individual nodes.
- [[sotoudeh-2024-network-based-measure]] — complements — Sotoudeh reports on cosponsorship networks that eigenvector centrality is not a strong indicator of bill passage (text confirmed). Ringe and Wilson validate centrality only against simulated ground truth, so together they bound what centrality shows.
- [[boxsteffensmeier-2018-taking-congress-interest]] — complements — Both ground cue-taking in legislatures. Box-Steffensmeier measures cues from Dear Colleague letters, while Ringe and Wilson infer the cue-provider from voting correlations.
- [[poole-1999-nominate-after-years]] — same-data — Both use roll-call matrices: NOMINATE scales ideology and Ringe and Wilson extract leadership centrality from binary votes. Melange rejected DW-NOMINATE vote edges (brain vote-edges section).
- [[battaglini-2016-influencing-connected-legislators]] — complements — Battaglini and Patacchini (NBER w22739) use Bonacich centrality of connected House legislators, not the term Katz-Bonacich. Melange has fec/engine/kb_benchmark.py. Ringe and Wilson recover closeness and weighted degree well in simulation.

### Linked from
- [[porter-2005-network-analysis-committees]] — same-data — Verified item (online appendix): co-voting network centrality from House roll calls, a different measure from Porter's SVD.
- [[ringe-2013-pinpointing-powerful-voting]] — same-data — Item title confirmed as co-voting network centrality in the EP (same first author). Relation to this paper outcome is plausible. Content of that item not re-verified.
- [[christopher-2016-mathematical-modeling-analysis]] — complements — Item file exists. Ringe et al. treat network centrality as a candidate measure of political influence, a contrast case for this paper, which assumes rather than tests that centrality stands for power. Content not re-read here.
- [[battaglini-2018-effectiveness-connected-legislators]] — complements — Item exists, but it is the online appendix (supplementary) to the co-voting centrality paper, not the article. It is an alternative influence measure, as the rel says.