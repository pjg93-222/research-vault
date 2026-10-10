---
type: theme
name: "Lobbying"
paper_count: 22
tags: [theme, type/theme]
---

# Lobbying

The Lobbying theme holds 22 works: 21 dated 2009-2025 and one undated ([[furnas-nd-conflating-lobbying-pacs]], published in PS in October 2025). They fall into six strands:

1. LDA data infrastructure and measurement.
2. The network and coalition structure of lobbying.
3. Whether donations and lobbying are linked channels or separate ones.
4. Firm-side returns, persistence and selection.
5. Two review essays that set the agenda for identification.
6. How lobbying reaches legislators: information, cues, lobbyist craft and vote models.

Paul's own notes frame lobbying in two ways. It is a measurable input that can be optimized (Huneeus & Kim: spending returns to revenue; Alexander: ROI), and it is an information and ideology channel (Furnas et al.: aligned think tanks). He treats LobbyView as the foundational dataset to take apart and extend. His recurring question is how to turn bipartite lobbying data and filing text into graph nodes and edges that path-finding and allocation can use.

Read together, the works answer a narrower question than the one Paul asks: what LDA disclosure can and cannot support. Four points recur across strands.

- **LD-2 never names members.** It names bills and agencies, never the members contacted. Every member-level lobbying link in this literature is therefore a bill-roster join, and none of these works treats it as a contact record ([[kim-2018-lobbyview-firm-level]], [[kim-2020-mapping-political-communities]], [[bacik-2025-measuring-dynamical-evolution]], [[defigueiredo-2013-advancing-empirical-research]], [[kim-2023-systemic-sequential-links]]). That independently backs the system's ruling that the lobbied-member link is circular, and D-021 (LDA is an identity source, not an edge or weight source).
- **Lobbying is rare, concentrated and persistent.**
  - In a 3,260-firm Compustat panel (1998-2006), 92% of firms that lobbied in year t-1 lobbied again in year t ([[kerr-2014-dynamics-firm-lobbying]]).
  - Among public firms that lobbied in 2016, more than 80% lobbied in 2017 ([[huneeus-2018-effects-firms-lobbying]]).
  - Among K-Street registrants (1999-2023), about 80% of income goes to 20% of firms ([[bacik-2025-measuring-dynamical-evolution]]).
- **Lobbying and PAC giving come mostly from different organizations, but the overlap holds most of the money.** Pooled over 1998-2018, 6.3% of 54,517 organizations did both, and those organizations account for about 75% of lobbying spend ([[furnas-nd-conflating-lobbying-pacs]]). Among public firms in 2013, 519 of 11,829 did both and held about 85% of $3.1B ([[kim-2020-strategic-sequential-links]]).
- **Return-to-lobbying figures are fragile, firm-side and never measure how a member responds.** See [[bombardini-2020-empirical-models-lobbying]], [[alexander-2009-lobbying-analysis-under]] and [[bickmore-2025-corporate-lobbying-political]].

The main disagreements are three:

- The time profile of the donation-to-lobbying link. Two versions of the same Kim-Stuckatz-Wolters project report opposite shapes.
- Whether donors get more access than constituents.
- How large returns to lobbying are.

No work in this theme argues for either SNA proposal this project measured and rejected: dropping regular cosponsorship edges, or per-edge volume normalization. No work validates reciprocity either; it remains unvalidated under D-104..D-108.

Many system bearings in the source file are single-agent analysis. Those marked UNVERIFIED (all four for [[holyoke-2015-learnable-skills-unteachable]], and the DE-062 caveats on [[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]], [[huneeus-2018-effects-firms-lobbying]] and [[furnas-2019-partisan-ties-lobbying]]) are leads, not findings.

## Strands

### LDA data infrastructure and measurement: what the filings can carry

Works: [[kim-2018-lobbyview-firm-level]], [[delano-2025-bicam-congressional-lobbying]], [[harrison-2018-application-data-mining]], [[kim-2025-measuring-interest-group]]

These four works are about building data, not estimating effects. Paul's notes treat LobbyView as the main asset and Harrison as the pipeline tutorial. The works themselves mostly show how much inference sits between an LD-2 filing and any variable a model uses.

**Bill resolution.** [[kim-2018-lobbyview-firm-level]] documents LobbyView's steps: regex bill detection, Congress assignment by the argmax of cosine similarity over three preceding Congresses, majority-vote propagation within a report, and expansion of bill ranges with gaps of 10 or less.
- It reports no error rate for any of these steps, and the argmax always returns an answer.
- Paul's contact-inference spec calls this lineage a cautionary chain because it asserts undocumented inference as capability. It is the reason his layer is abstention-first.
- The paper names one failure: the naive filing-year rule misassigns Google's 2013 citation of H.R. 2577. Paul's own 2021+ rebuild uses that same filing-year rule and does not expand ranges. The network.csv vintage (2000-2022) and the rebuild are therefore different instruments, and the seam between them has not been measured.

[[delano-2025-bicam-congressional-lobbying]] (BICAM) re-links LD-2 Section 16 text to a Congress.gov/GovInfo bill table by distance scores, with low-confidence matches reviewed by hand. It says it finds more bill lobbying than Kim and Kunisky. Its match precision and recall are in an SI that was not available, so for now it is a candidate second path for checking bill resolution, not a validated one. It does not fix circularity: a BICAM bill-to-member join is still joined-given-lobbied = 1 by construction.

**Amount semantics.** [[harrison-2018-application-data-mining]] parses one quarter (Q4 2009, Senate XML) and runs no statistics. Its value is a list of hazards:
- in-house expenses and outside-firm income are mixed in one field;
- amounts under $5,000 appear only as a checkbox, and income is rounded to $10,000;
- one filing bundles many issues;
- filing date differs from activity date.

Mentat now enforces the first hazard: outside-firm fees and in-house expenses are never added (D-064, commit 914623152). The older LobbyView collector still sums them. The brain's "ABA $228M vs CUNA $93M" figure probably comes from that mixed sum (UNVERIFIED).

**Positions.** [[kim-2025-measuring-interest-group]] produces 279,104 group-by-bill Support/Oppose/Engage labels (12,032 groups, 42,475 bills, 2009-2022). This is the text analysis Harrison proposed as future work.
- 70.5% of the labels come from GNN imputation, not from the LLM.
- The LLM's 96.9% agreement is measured only on the 391 items where both human coders agreed.
- Oppose is the weakest class (GNN F1 64).
- The system replicated the labelling with Haiku against Kim's labels at natural base rates. Support and Oppose came out at precision 0.42 and recall 0.49, and Amend was never recovered. That result set the rule that inferred positions are evidence for retrieval and adjudication, never automatic weights.

Paul's annotation calls Kim 2025 reactive, not predictive, and blind to inter-industry conflict over a bill. The works agree with the first point: filings lag. On the second, the per-bill labels are exactly the raw material for conflict detection, which the system's contested-bills registry already computes.

Across the strand, every author agrees that identity resolution (firm names to gvkey or Compustat, bill mentions to bill IDs) is the heavy lifting. Only Huneeus and Kim's 5% random validation sample (strand 4) and Kim's hand-coded checks report any accuracy at all.

### Network structure and coalitions of lobbying

Works: [[kim-2020-mapping-political-communities]], [[bacik-2025-measuring-dynamical-evolution]], [[pritoni-2025-lobbying-coalitions-determinants]]

This strand is where Paul's network program meets the lobbying literature most directly. His notes cite Kim & Kunisky for community detection, and he names their open problem as converting a bipartite lobbying model into a graph that path-finding can use. He spells the co-author "Kuinsky" in the annotation; the citation reads Kunisky.

**[[kim-2020-mapping-political-communities]].** The Bipartite Link Community Model is a Poisson model fitted by EM, with k = 8 and 50 starts, on the 113th Congress only.
- The group-by-politician count is the number of bills a group lobbied that the politician sponsored. That is the same bill-roster construction as Melange's lobbied-member link, and the paper never reads it as contact or influence.
- It recovers eight communities that line up with committee jurisdictions. Specialists such as Microsoft and Arch Coal place at least 85% in one community. Generalists such as the Chamber of Commerce place no more than 25% in any one.
- Senior members and generalists cluster on power committees.
- Co-lobbying is unsigned, so the model cannot separate lobbying for a bill from lobbying against it.
- The dashboard cites this paper for "exploitable community structure" in the cosponsor graph. That is a misattribution: the paper's communities are group-by-member lobbying communities.

**[[bacik-2025-measuring-dynamical-evolution]].** A yearly five-layer directed network built from LobbyView: 1,277,411 filings, 1999-2023.
- 200-500 K-Street registrants enter each year, and 20% of registrants active in 2000 were still active in 2020.
- Up to one-third of client-registrant ties are rewired each year.
- Attachment is linearly preferential. The heavy-tailed fit has exponent about 1.5 and cutoff about 86, and the three figures inside the paper differ slightly.
- Agency-target portfolios form blocks by presidential administration.
- Legislators enter only through lobbyists' undated covered-position disclosures, so the network has no registrant-to-legislator contact layer. That is independent support for the circularity ruling and for fail-closed identity handling.
- The "election synchronization" finding is about which agencies are targeted, not about money.
- There is no press or sentiment data, so Paul's augmentation idea (news cycles and sentiment as drivers of change) has no support here yet.

The two papers agree that the lobbying market is narrow and concentrated: specialists dominate (Kim-Kunisky) and income is concentrated (Bacik). They differ in time scale. Kim-Kunisky is a single-Congress snapshot; Bacik is a 25-year panel with no mixed membership.

**[[pritoni-2025-lobbying-coalitions-determinants]].** A PRISMA-ScR review of 80 articles (1985-2023), with a vote-count meta-analysis of 77 hypotheses from 16 articles. It adds two cautions.
- First, SNA is rare in coalition research: 4 of 80 articles. SNA-based tests have the weakest record of any design, 5 of 9 hypotheses supported. The overall 74% support rate (57 of 77) is UNSTRATIFIED by polity, pooling US and EU work.
- Second, its coalitions are among interest groups, while Melange's coalitions are mostly blocs of legislators (Ward clusters on press lean, signed partitions). Two narrower points transfer. Alignment depends on the issue, which favors the per-bill contested registry over fixed global sides. And weaker groups pool resources ("weapon of the weak", 11 of 12 tests), while CU is modeled as a lone actor.
- The review calls evidence on whether coalitions win contradictory. Nothing here justifies adding allied money as a weight.

### Donations and lobbying: linked channels or separate populations

Works: [[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]], [[furnas-nd-conflating-lobbying-pacs]], [[fremeth-2018-spillovers-regulating-corporate]], [[furnas-2019-partisan-ties-lobbying]]

This strand bears directly on the system's closed FEC-to-LDA bridge and its "FEC leads LDA by 42 months" heuristic.

**The Kim-Stuckatz-Wolters project.** It appears twice: [[kim-2020-strategic-sequential-links]] (draft) and [[kim-2023-systemic-sequential-links]] (later version). Both use a firm-politician-year panel of 2,703 public manufacturing firms and 979 members, about 16.4M rows. Window: 2008-2017 in the draft, 2008-2018 in the later version. Both ask whether a first-time donation, defined as none in the prior 3 years, precedes lobbying on bills the recipient sponsors, cosponsors or handles in committee.

The versions disagree on magnitude and on time profile:

| | 2020 draft | 2023 version |
|---|---|---|
| Fixed-effects estimate | 10-12 pp over a base rate of about 0.05 | 2.3-4.8 pp over a base rate of about 0.07 (pooled; UNSTRATIFIED by chamber or donor type) |
| Matched DiD, Senate | about 8.5 pp at year t, falling to about 3 pp by t+3 | 7.5 pp at t+0, rising to 11 pp at t+3 |
| Matched DiD, House | not given | 8.6 pp rising to 12.7 pp |

The source file advises comparing the two before preferring either. Neither version supports Paul's annotation claim that donation effects fade after about 6 months. Neither supports a 42-month lead either: in the code the 42-month figure is only how much fresher FEC data is than LDA data.

The versions agree that the link is strongest for corporate PAC money and for committee-referral lobbying. In the 2023 version, PAC committee lobbying runs 0.103 to 0.147 from t+0 to t+3, and lobbying does not predict later donations. The authors call the results associations and access-buying timing, not quid pro quo.

The system's bridge is a cross-sectional member-by-industry-by-cycle rank correlation. It credits only sponsors and cosponsors, controls for finance-committee seats, and is dominated by trade associations. That design leaves out, or controls away, the channel Kim finds strongest. Its four-control partial of about 0 therefore answers a different question and is not contradicted.

**[[furnas-nd-conflating-lobbying-pacs]].** This paper supplies the population base rate. Of organizations in each year:
- 1998 (n = 9,076): 12% did both lobbying and PAC giving.
- 2008 (n = 15,729): 10% did both.
- Pooled 1998-2018: 6.3% of 54,517 (the denominator is the extractor's sum of the figure counts).

That minority accounts for about 75% of lobbying spend. The share of PAC dollars coming from lobbying organizations fell from over 65% in 1998 to about 30% in 2018. Employee giving, Super PACs and dark money are excluded, so the overlap figures do not carry over to Mentat's employer-string totals without first aligning definitions.

Kim and Furnas agree in kind: low overlap, with the overlapping group holding most of the money. Their denominators differ: public firms in 2013 for Kim, all registered organizations for Furnas. The two Kim versions also differ on the 2013 snapshot: 519 of 11,829 firms holding 85% of $3.1B, versus 518 of 9,791 holding 89% of $2.86B.

**[[fremeth-2018-spillovers-regulating-corporate]].** S&P 500 firms with PACs; FEC data 1992-2008 and LDA data 2000-2008.
- A candidate who receives the PAC maximum is 6.6 points more likely to also receive CEO money. The text says 7.7%, and that mismatch is noted.
- Firms whose PAC maxes out any candidate spend about $549,000 more on lobbying per cycle.
- Placebo outcomes show no jump.
- The authors call these partial correlations; no exogenous variation in the limit exists.

It suggests money at the per-candidate cap shifts into channels that neither the optimizer's $10,000 ceiling nor the flip model's PAC-only competitor ledger can see.

**[[furnas-2019-partisan-ties-lobbying]].** Uses lobbyists' FEC giving only as a partisan-identity signal for 1,603 contract firms (33,243 firm-quarters, 2008 Q1-2016 Q3). House-majority alignment adds about $5,000-6,000 of revenue per lobbyist per quarter, against a mean of $74k. Senate alignment depends on the specification: House ties support the revenue hypothesis in 12 of 13 tests, Senate ties in 3 of 13. That is a market premium clients pay. It is not persuasion and not an access probability, and the effect is distributional between firms.

### Firm-side returns, persistence and selection

Works: [[huneeus-2018-effects-firms-lobbying]], [[kerr-2014-dynamics-firm-lobbying]], [[alexander-2009-lobbying-analysis-under]], [[bickmore-2025-corporate-lobbying-political]]

Paul's annotations group Alexander, Bickmore and Huneeus & Kim as the "lobbying pays" cluster. He takes allocation implications from them: set fundraising targets from the revenue elasticity, and differentiate representatives by state-layer industry presence. The works support "lobbying is associated with firm-side gains." They give no evidence on how members respond, and their magnitudes vary by orders of magnitude depending on design.

**[[alexander-2009-lobbying-analysis-under]].** A cross-sectional ROI of about 220:1: $62bn of estimated tax savings on $282.7M of lobbying, for 93 lobbying firms among 450 repatriators under AJCA s.965 (2004-2006).
- Outliers drive it: the per-company mean is 869:1 and the median 172:1.
- The lobbying denominator is an upper bound that would mix in-house and outside-firm dollars.
- The provision was essentially uncontested.
- The abstract says cash holdings do not matter, but Table 5 shows cash/assets significant (3.70, p<.001).

**[[huneeus-2018-effects-firms-lobbying]].** LDA 1999-2015 linked to Compustat. The instrument is a shift-share of committee seats held by members from the firm's home state.
- IV sales elasticity: 0.319 over all years (first-stage F 14.3) and 0.227 after 2007 (F 13.1). OLS gives about 0.04-0.05.
- The headline "3% per 10%" uses the full-sample estimate; the paper's stated preferred estimate is 0.2% per 1%. [[bombardini-2020-empirical-models-lobbying]] repeats the 3% headline.
- A structural SMM model, run with an identity weighting matrix, finds a 22% productivity loss. That figure moves from 38% to 13% as sigma_C goes from 2 to 6.

**[[bickmore-2025-corporate-lobbying-political]].** Compustat x CRP, 1998-2016, 116,242 firm-years with full controls.
- Lobbying firms' share of industry assets is 0.0743 higher. That is 7.4 percentage points on a share whose mean is 0.0727; it is not a 7.4% relative rise.
- Industry and year fixed effects only, no firm fixed effects. UNSTRATIFIED by industry.
- Winning-candidate PAC money is significant and losing-candidate money is not, but the difference is never tested.
- The Abramoff DiD (2003Q1-2009Q1) has a pre-trend that bends in late 2004.

**[[kerr-2014-dynamics-firm-lobbying]].** Supplies the persistence fact that the reviews repeat.
- 3,260 firms, 1998-2006: 6.2% lobby in a given year, 92% persistence, and 96% of a year's dollars come from prior-year lobbyists.
- Blundell-Bond puts the lagged-status effect at 0.84-0.88. Within-firm fixed effects give about 0.44, which is Nickell-biased downward.
- Entry clusters in the year before an election.
- Under the 2004 H-1B shock, all of the response was on the intensive margin: dependence raised spending on high-skill immigration (0.043) but not the chance of lobbying at all (-0.008).

The four works agree on size gradients and persistence. Huneeus finds more than 80% of firms that lobbied in 2016 lobbied in 2017; Kerr finds 92%. They also share selection: only about 6-12% of public firms lobby in a given year, which makes cross-sectional ROI comparisons selected samples.

For Paul's sector-growth study (H1 deep pockets, with lagged lobbying as a control citing Kerr), the strand carries two warnings:
- Huneeus's causal direction runs lobbying to revenue, a simultaneity threat to growth-to-spending regressions.
- Kerr's lagged-outcome dynamics with sector fixed effects are the textbook Nickell case. Paul's design has about 9 elections and 5 clusters, so that matters.

### Reviews and the identification agenda

Works: [[bombardini-2020-empirical-models-lobbying]], [[defigueiredo-2013-advancing-empirical-research]]

The two reviews sit seven years apart and agree on the core diagnosis. Lobbying panels are persistent, selected and confounded by unobserved channels, so fixed effects alone do not identify effects. Credible designs use exogenous shocks or structural models.

**[[defigueiredo-2013-advancing-empirical-research]]** (NBER WP 19698) sets out the scale facts.
- About $3.5bn of federal lobbying in 2012, against about $750m a year of interest-group contributions: roughly 5 times.
- Corporations and trade associations account for over 84% of lobbying spend.
- About 10% of public firms lobby, with 92% persistence (citing Kerr).
- It surveys DiD on legislator exits, event studies, cost-shifter IVs, Heckman selection and all-pay contest models.
- It notes that transaction data record spending but not which legislator was targeted.

**[[bombardini-2020-empirical-models-lobbying]]** (Annual Review of Economics) organizes the field as quid pro quo versus informational. It shows how fragile structural magnitudes are.
- Kang (2016), 538 energy policies in the 110th Congress: an enactment lift of only 0.021-0.415% over an 8% baseline, yet more than $500m of implied value per policy.
- A naive Farm Bill comparison gives about a 600,000% return: $22bn of 2000 subsidies against under $3.7m of agribusiness PAC money. The authors read this as evidence of omitted channels.
- It reports the in-house share of lobbying falling from about 60% to under 40% between 1999 and 2008.

The reviews disagree mildly on the lobbying-to-contribution ratio because they use different denominators. de Figueiredo compares about 5 times interest-group contributions. Bombardini sets $3-3.5bn a year of lobbying against $200-250m a year of PAC donations. [[huneeus-2018-effects-firms-lobbying]] reports more than 6 times PAC, party and candidate contributions, excluding individuals, over 1999-2016. Any figure must name which contributions it counts.

Both reviews cite revolving-door revenue evidence: about 23-24% of lobbyist revenue is lost when the former boss leaves office (Blanes i Vidal et al. 2012). That is a statement about lobbyist labor-market revenue. Under the project's rules it must not become influence language on staffer or revolving-door edges. At most it suggests a validity condition for staffer routes: whether the anchoring member is still in office. Route costs remain heuristic (D-025).

Neither review argues for dropping regular cosponsorship edges or for per-edge volume normalization.

### How lobbying reaches legislators: information, cues, craft and vote models

Works: [[furnas-2019-moneyed-interests-information]], [[boxsteffensmeier-2018-taking-congress-interest]], [[holyoke-2015-learnable-skills-unteachable]], [[jacoby-2025-mathematical-modeling-analysis]]

This is the only strand that looks inside Congress. Its evidence is the weakest in the theme: stated intentions, pooled observational models, a conceptual essay, and a simulation with fixed parameters.

**[[furnas-2019-moneyed-interests-information]].** The 2019 APSA working paper of the study published in 2023. Paul's annotation cites it as Furnas, LaPira & Hertel-Fernandez 2022, and in one annotation file it sits under a Kalla & Broockman citation, so Paul should confirm which paper he meant. It is a 4x2x4 vignette run on 441 staffers, a 5.2% response rate, in the 2017 Congressional Capacity Survey.
- Donors get no more meetings than constituents, which runs against Kalla & Broockman's field experiment ([[kalla-nd-campaign-contributions-facilitate]], outside this theme).
- Staff are less likely to use donor information.
- Pooled across staff, think-tank evidence barely changes whether staff side with the requester. Split by party, staff side with their own side's think tank, and a non-aligned source costs about twice what an aligned one gains.
- After FDR correction, 46 of 11,904 pairwise tests survive.

Paul's proposal to fund aligned think tanks like venture capital therefore rests on a party-split stated-intention effect, not a behavioral one. The system attributes two claims to this paper that it does not make: "money is capped, info is the lever" and "district-relevant information." Its asymmetry also runs the opposite way from cpf_pressure's.

**[[boxsteffensmeier-2018-taking-congress-interest]].** 6,597 House bills with Dear Colleague letters, 106th-111th Congresses.
- Endorser eigenvector centrality in the amicus co-signing network raises cosponsor counts: 33 predicted cosponsors with no endorsement, 51 with a top-centrality endorser.
- Centrality has no direct effect on reporting or passage. Endorser count predicts passage, and cosponsor count carries an indirect path.
- Majority-party sponsorship (1.353) and hot-bill status (2.428) are the largest passage predictors.
- Estimates are UNSTRATIFIED across six Congresses and both parties.
- Endorsements may be solicited by the sponsor's office, the same reverse-direction problem that makes the lobbied-member link circular.
- The paper treats cosponsorship as a meaningful indirect channel to passage. That is consistent with keeping regular cosponsorship edges.

**[[holyoke-2015-learnable-skills-unteachable]].** A curriculum essay with no data. It argues that relationships rest on mutual need and constituency stake, not on time in the same office. Its four system bearings are all UNVERIFIED single-agent leads. At most it supports reading the staffer overlap router's tie strength as a heuristic cost, never as relationship strength or access (D-025).

**[[jacoby-2025-mathematical-modeling-analysis]].** Applies an epidemic-style compartment ODE model to one vote (Waxman-Markey 2009), with every parameter taken from the literature.
- Adding lobbying resolves 8 more undecided House members.
- Adding donations moves 1.
- Widening the centrist band from ±0.25 to ±0.35 reverses the House result.
- The paper claims accuracy within 5%, but its own errors are 5.02%, 5.48% and 7.55%.

Its usable content is structural: members who have decided stay fixed, and who counts as movable drives the outcome. Its lobbying-versus-donations contrast compares simulation runs and is not an estimate. Its lobbyist counts would also breach D-021 if imported as weights.

## Agreements and debates

**1. Does money buy access?**

- *Observational evidence:* in the dyad studies, a first-time donation precedes committee-linked lobbying of the same member, which the authors read as access-buying timing, not quid pro quo ([[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]]).
- *Survey experiment:* donors get no meeting advantage over constituents, and staff discount donor information ([[furnas-2019-moneyed-interests-information]]). The study sets itself explicitly against Kalla & Broockman's field experiment ([[kalla-nd-campaign-contributions-facilitate]]).
- *Essay:* campaign finance matters less than people think ([[holyoke-2015-learnable-skills-unteachable]]).
- *Simulation:* donations move 1 of 435 House members ([[jacoby-2025-mathematical-modeling-analysis]]), but that is a parameter-driven run, not evidence.

The defensible reading for the flip model is that money is a weak, access-level prior. It is never a vote mechanism.

**2. What is the time profile of the donation-to-lobbying link?**

The two Kim-Stuckatz-Wolters versions disagree:

| Version | Senate DiD effect | Trend |
|---|---|---|
| [[kim-2020-strategic-sequential-links]] | about 8.5 pp at t | falls to about 3 pp by t+3 |
| [[kim-2023-systemic-sequential-links]] | 7.5 pp at t+0 | rises to 11 pp at t+3 (House 8.6 to 12.7) |

Neither version supports Paul's annotated "fades after about 6 months." Neither supports a 42-month FEC-leads-LDA lead time. Resolve this by reading both papers' tables side by side before citing either.

**3. Are lobbying and PAC giving complements or separate worlds?**

The works agree on the shape: a small overlap that holds most of the money.

- [[furnas-nd-conflating-lobbying-pacs]]: 6.3% of 54,517 organizations did both (pooled 1998-2018), holding about 75% of lobbying spend.
- [[kim-2020-strategic-sequential-links]]: 519 of 11,829 public firms did both (2013), holding about 85% of $3.1B.
- [[fremeth-2018-spillovers-regulating-corporate]]: firms at the PAC cap substitute into CEO giving and lobbying (partial correlations).
- [[bickmore-2025-corporate-lobbying-political]]: both channels are associated with industry share, but it never tests them jointly.

The conflict is one of interpretation. Furnas warns against treating one channel as a proxy for the other. Fremeth and Kim treat them as linked within firms. Both can hold: they are linked inside the overlap and separate across the population.

**4. How large are returns to lobbying?**

| Source | Estimate | Basis |
|---|---|---|
| [[alexander-2009-lobbying-analysis-under]] | about 220:1 | one uncontested provision, upper-bound denominator |
| Kang via [[bombardini-2020-empirical-models-lobbying]] | 137-152% returns | 0.021-0.415% enactment lift |
| [[huneeus-2018-effects-firms-lobbying]] | sales elasticity 0.32 (IV) | vs about 0.04 OLS |
| [[bickmore-2025-corporate-lobbying-political]] | 7.4 pp of industry asset share | association only |
| Naive Farm Bill comparison ([[bombardini-2020-empirical-models-lobbying]]) | about 600,000% | omitted channels |

Both reviews treat this dispersion as evidence of misspecification and omitted channels ([[bombardini-2020-empirical-models-lobbying]], [[defigueiredo-2013-advancing-empirical-research]]). None of these figures transfers to Melange's flips per PAC dollar.

**5. House versus Senate.**

- [[furnas-2019-partisan-ties-lobbying]]: House ties support the revenue hypothesis in 12 of 13 tests, Senate ties in 3 of 13.
- [[kim-2023-systemic-sequential-links]]: House DiD effects exceed Senate effects.
- [[jacoby-2025-mathematical-modeling-analysis]]: the Senate barely moves.

The estimands are different, but the direction agrees: expect a weaker Senate signal, and keep the chambers separate.

**6. Is lobbying driven by specialists or coalitions?**

- *Narrow interest dominates:* specialists dominate ([[kim-2020-mapping-political-communities]]); the median bill has 2 clients across 65,047 bills ([[huneeus-2018-effects-firms-lobbying]]); income is concentrated, with a cumulative Gini of about 0.8 ([[bacik-2025-measuring-dynamical-evolution]]).
- *Pooling:* [[pritoni-2025-lobbying-coalitions-determinants]] finds weaker groups pool resources (supported in 11 of 12 tests).

The two are compatible. Coalitions are the weak actor's strategy, and specialists are the strong actor's.

**7. Information versus exchange.**

[[bombardini-2020-empirical-models-lobbying]] frames the field as quid pro quo versus informational models. [[furnas-2019-moneyed-interests-information]] (source alignment) and [[boxsteffensmeier-2018-taking-congress-interest]] (endorsement cues) supply informational-side evidence, but both are pooled or stated-intention evidence. The question stays open.

**8. Can SNA carry this theme?**

- [[pritoni-2025-lobbying-coalitions-determinants]]: SNA-based coalition tests have the weakest support of any design (5 of 9).
- [[boxsteffensmeier-2018-taking-congress-interest]]: eigenvector centrality predicts cosponsor counts but not passage.
- [[kim-2020-mapping-political-communities]]: communities recovered only as group-by-member bill-roster blocks.

Network methods describe structure. In this theme they have not shown outcome leverage.

**Flag (rejected SNA proposals):** no work here argues for dropping regular cosponsorship edges or for per-edge volume normalization. The degree-corrected bipartite SBM in [[kim-2020-mapping-political-communities]] is a comparison baseline that misses mixed membership. It is not an argument for per-edge normalization and must not be read as one.

## Methods

**1. Descriptive data construction (the most common type: 5 works).** [[kim-2018-lobbyview-firm-level]], [[delano-2025-bicam-congressional-lobbying]], [[harrison-2018-application-data-mining]], [[bacik-2025-measuring-dynamical-evolution]] and [[furnas-nd-conflating-lobbying-pacs]].

Weaknesses:
- Accuracy of bill resolution and entity linkage goes unreported. LobbyView reports no error rates, and BICAM's precision and recall sit in an unavailable SI.
- Argmax heuristics never abstain.
- Amount fields have mixed semantics (in-house expenses vs outside income) and are rounded.
- Issue codes are bundled within filings.
- Filing date is not activity date.
- Population definitions differ (CRP vs LobbyView vs SOPR; organizational vs employee giving), so counts cannot be compared without alignment.

**2. Firm-level panel regressions on LDA linked to Compustat or FEC.** [[huneeus-2018-effects-firms-lobbying]], [[kerr-2014-dynamics-firm-lobbying]], [[bickmore-2025-corporate-lobbying-political]], [[fremeth-2018-spillovers-regulating-corporate]], [[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]] and [[alexander-2009-lobbying-analysis-under]].

Shared weaknesses:
- Strong persistence (92% year-to-year) makes fixed effects with a lagged outcome biased (Nickell), as Kerr shows: 0.44 within-firm vs 0.84-0.88 Blundell-Bond.
- Selection: only about 6-12% of public firms lobby in a given year.
- Simultaneity between size and lobbying.
- Samples are restricted to public firms, often manufacturing or S&P 500.
- The member side is reached only through bill rosters or committee referral.
- Several pooled coefficients are UNSTRATIFIED over known fault lines: [[bickmore-2025-corporate-lobbying-political]] by industry; the [[kim-2023-systemic-sequential-links]] fixed-effects table by chamber and donor type.
- Text and table mismatches recur: Fremeth (CEO probability 6.6 vs 7.7), Alexander (cash significance), Bacik (power-law exponent), and the two Kim versions.

**3. Quasi-experimental designs.**

| Design | Source | Caveat |
|---|---|---|
| Matched DiD | [[kim-2023-systemic-sequential-links]] | the two versions give opposite time profiles |
| Kernel-weighted DiD around chamber flips | [[furnas-2019-partisan-ties-lobbying]] | 2015 Senate flip null |
| Abramoff-plea DiD | [[bickmore-2025-corporate-lobbying-political]] | pre-trend bends in late 2004; placebos shown only in figures |
| H-1B cap shock | [[kerr-2014-dynamics-firm-lobbying]] | 171 firms |
| Committee-seat shift-share IV | [[huneeus-2018-effects-firms-lobbying]] | first-stage F about 13-14 (marginal); assumes committee moves are exogenous to firms |
| One-sided RD at the PAC cap | [[fremeth-2018-spillovers-regulating-corporate]] | authors call the frictions assumption strong |

These are the best-identified results in the theme. All of them identify firm-side outcomes.

**4. Network and latent-community models.** [[kim-2020-mapping-political-communities]] (biLCM, compared with a latent space model and a dc-biSBM) and [[bacik-2025-measuring-dynamical-evolution]] (degree distributions, preferential attachment, configuration-model nulls). Weaknesses: one Congress (Kim-Kunisky) or no mixed membership (Bacik); unsigned edges; member links built from bill rosters; k chosen by reference to another model.

**5. LLM and GNN labelling.** [[kim-2025-measuring-interest-group]]. Weaknesses:
- Validation is limited to items where the human coders agreed.
- The majority of labels (70.5%) are GNN imputations above a 0.9 probability cutoff.
- The Oppose class is weak.
- The system's own replication at natural base rates came out far lower (precision 0.42, recall 0.49 on Support and Oppose).

**6. Survey experiment.** [[furnas-2019-moneyed-interests-information]]. Stated intentions, a 5.2% response rate, and an underpowered preregistered design (46 of 11,904 tests survive FDR).

**7. Cosponsor and passage count models.** [[boxsteffensmeier-2018-taking-congress-interest]]. Negative binomial and logit with Congress fixed effects; no causal design; endorsements possibly solicited.

**8. Deterministic simulation.** [[jacoby-2025-mathematical-modeling-analysis]]. Parameters are fixed rather than estimated, one validation point per chamber, and conclusions flip with the centrist band.

**9. Reviews.** Narrative reviews ([[bombardini-2020-empirical-models-lobbying]], [[defigueiredo-2013-advancing-empirical-research]]) and a vote-count scoping review ([[pritoni-2025-lobbying-coalitions-determinants]]). Vote counting ignores effect size and power, and the 74% figure pools US and EU studies (UNSTRATIFIED). Plus one conceptual essay ([[holyoke-2015-learnable-skills-unteachable]]).

**Recurring hidden method: identity resolution.** Gvkey matching with constrained graph clustering (Kim-Stuckatz-Wolters), and fuzzy matching with a 5% random validation sample (Huneeus & Kim). This is the method content most directly reusable for Mentat's identity ladder and the LDA client classifier.

## How the works relate

- [[kim-2023-systemic-sequential-links]] — builds-on — [[kim-2020-strategic-sequential-links]]: Later version of the same Kim-Stuckatz-Wolters project with the same first-time-donation design and firm-politician panel.
- [[kim-2023-systemic-sequential-links]] — contradicts — [[kim-2020-strategic-sequential-links]]: The versions disagree on magnitude and time profile. Fixed effects: 2.3-4.8 pp over a 0.07 base vs 10-12 pp over a 0.05 base. Senate DiD: rising 7.5 to 11 pp by t+3 vs falling from about 8.5 to about 3 pp. The 2013 snapshot denominators also differ (518 of 9,791 vs 519 of 11,829).
- [[kim-2020-strategic-sequential-links]] — builds-on — [[kim-2018-lobbyview-firm-level]]: Cites LobbyView as the public source of its firm-bill lobbying data.
- [[bacik-2025-measuring-dynamical-evolution]] — same-data — [[kim-2018-lobbyview-firm-level]]: Builds its 1999-2023 multilayer network from the current LobbyView database.
- [[kim-2025-measuring-interest-group]] — same-data — [[kim-2018-lobbyview-firm-level]]: Labels positions on LDA reports organized in LobbyView and maps MapLight onto LobbyView pairs.
- [[kim-2020-mapping-political-communities]] — same-data — [[kim-2018-lobbyview-firm-level]]: Its 113th-Congress lobbying network is distilled from LobbyView and shares its bill-identification pipeline.
- [[huneeus-2018-effects-firms-lobbying]] — same-data — [[kim-2018-lobbyview-firm-level]]: Uses LobbyView's firm-disambiguated LDA data merged with Compustat.
- [[delano-2025-bicam-congressional-lobbying]] — extends — [[kim-2020-mapping-political-communities]]: BICAM's Bill-to-LDA linkage says it finds significantly more bill lobbying than Kim and Kunisky's data. Precision and recall are not in the available text.
- [[delano-2025-bicam-congressional-lobbying]] — complements — [[kim-2018-lobbyview-firm-level]]: Same In Song Kim lineage. BICAM is a candidate second path for checking LD-2 bill resolution; it cites a 2024 LobbyView report, so this is a version link.
- [[kim-2025-measuring-interest-group]] — extends — [[harrison-2018-application-data-mining]]: Does the LDA text analysis Harrison proposes as future work, labelling bill positions at scale for 2009-2022.
- [[harrison-2018-application-data-mining]] — same-data — [[kim-2018-lobbyview-firm-level]]: Both parse LDA reports. LobbyView cleans and links them at full scale, where Harrison covers one quarter (Q4 2009).
- [[kim-2025-measuring-interest-group]] — complements — [[kim-2020-mapping-political-communities]]: Supplies the signed for/against positions that Kim-Kunisky's unsigned co-lobbying network lacks.
- [[pritoni-2025-lobbying-coalitions-determinants]] — complements — [[kim-2025-measuring-interest-group]]: Pritoni's finding that coalition alignment depends on the issue could be observed through Kim et al.'s per-bill positions. D-021 still limits LDA to identity, not edges or weights.
- [[bombardini-2020-empirical-models-lobbying]] — extends — [[defigueiredo-2013-advancing-empirical-research]]: The later review updates the same identification agenda (shocks, IV, structural models) and the same persistence and scale facts.
- [[defigueiredo-2013-advancing-empirical-research]] — cites — [[kerr-2014-dynamics-firm-lobbying]]: Cites the Kerr, Lincoln and Mishra working paper for the roughly 10% participation and 92% persistence facts.
- [[bombardini-2020-empirical-models-lobbying]] — cites — [[kerr-2014-dynamics-firm-lobbying]]: Reports the 92% year-to-year persistence from Kerr et al. 2014.
- [[bombardini-2020-empirical-models-lobbying]] — cites — [[huneeus-2018-effects-firms-lobbying]]: Reviews Huneeus and Kim's structural model: 10% more lobbying gives 3% more revenue; 22% productivity loss split 59/41.
- [[bombardini-2020-empirical-models-lobbying]] — cites — [[kim-2018-lobbyview-firm-level]]: Cites LobbyView as the state-of-the-art Compustat-linked lobbying data.
- [[bacik-2025-measuring-dynamical-evolution]] — cites — [[bombardini-2020-empirical-models-lobbying]]: Cites the review (ref 1) as the empirical-models survey its descriptive network work sits beside.
- [[kim-2020-strategic-sequential-links]] — cites — [[huneeus-2018-effects-firms-lobbying]]: Cites Huneeus and Kim for strong returns to corporate lobbying, in contrast to the absence of quid-pro-quo returns to donations.
- [[kim-2023-systemic-sequential-links]] — cites — [[huneeus-2018-effects-firms-lobbying]]: Cites Huneeus and Kim (2019 version) on firm-level returns to lobbying.
- [[kim-2020-strategic-sequential-links]] — cites — [[bombardini-2020-empirical-models-lobbying]]: Places itself within the firm-level lobbying literature that the review surveys.
- [[furnas-nd-conflating-lobbying-pacs]] — cites — [[furnas-2019-moneyed-interests-information]]: The PS article cites the published 2023 version of this survey experiment as contrary evidence on buying access.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — [[kim-2020-strategic-sequential-links]]: Agrees in kind: low overlap, with the overlapping group holding most money. Denominators differ: 6.3% of 54,517 organizations (pooled 1998-2018) vs 519 of 11,829 public firms (2013). Population base rate vs dyad sequencing.
- [[fremeth-2018-spillovers-regulating-corporate]] — complements — [[furnas-nd-conflating-lobbying-pacs]]: Within-firm substitution at the PAC cap into CEO giving and lobbying, alongside Furnas's population-level separation of the channels. Fremeth's own related notes were not re-read against the body.
- [[fremeth-2018-spillovers-regulating-corporate]] — cites — [[defigueiredo-2013-advancing-empirical-research]]: Listed as cited in the source file. The citation claim was not re-read there, so treat it as unverified.
- [[furnas-2019-partisan-ties-lobbying]] — complements — [[bacik-2025-measuring-dynamical-evolution]]: Both find bipartisan lobbying organizations earn more (Furnas: highest average revenue; Bacik: higher income per lobbyist) while legislators polarize.
- [[furnas-2019-partisan-ties-lobbying]] — complements — [[furnas-nd-conflating-lobbying-pacs]]: Same first author. Low lobbying-PAC overlap explains why a lobbying-revenue premium need not appear in PAC giving-share tests.
- [[kerr-2014-dynamics-firm-lobbying]] — complements — [[huneeus-2018-effects-firms-lobbying]]: Both report high persistence and a size gradient: 92% persistence (3,260 firms, 1998-2006) vs more than 80% (public firms, 2016-2017). Kerr does not cite Huneeus and Kim.
- [[bickmore-2025-corporate-lobbying-political]] — complements — [[huneeus-2018-effects-firms-lobbying]]: Both relate firm lobbying to firm size or share; the link is topical, not cited. Different outcomes: industry share vs a firm-size distortion.
- [[alexander-2009-lobbying-analysis-under]] — complements — [[bickmore-2025-corporate-lobbying-political]]: Both are firm-side returns-to-political-activity evidence, cross-sectional or associational; the returns cluster in Paul's annotation.
- [[defigueiredo-2013-advancing-empirical-research]] — complements — [[alexander-2009-lobbying-analysis-under]]: The essay's selection and omitted-channel warnings apply directly to Alexander's single-policy ROI with its upper-bound denominator.
- [[bickmore-2025-corporate-lobbying-political]] — complements — [[kim-2018-lobbyview-firm-level]]: Not the same data: Bickmore uses CRP lobbying matched to Compustat by name, not LobbyView.
- [[kerr-2014-dynamics-firm-lobbying]] — complements — [[kim-2018-lobbyview-firm-level]]: Corrected in the source from same-data: Kerr uses CRP and SOPR filings, not LobbyView.
- [[boxsteffensmeier-2018-taking-congress-interest]] — complements — [[furnas-2019-moneyed-interests-information]]: Both study interest-group signals as cues inside Congress: endorsement centrality vs the ideological source of evidence.
- [[holyoke-2015-learnable-skills-unteachable]] — complements — [[furnas-nd-conflating-lobbying-pacs]]: Furnas's low overlap gives empirical weight to Holyoke's unquantified claim that few lobbyists engage in campaign finance.
- [[holyoke-2015-learnable-skills-unteachable]] — complements — [[pritoni-2025-lobbying-coalitions-determinants]]: Both draw on Hula (1999) on informed lobbyists attracting coalition partners.
- [[jacoby-2025-mathematical-modeling-analysis]] — complements — [[furnas-nd-conflating-lobbying-pacs]]: Jacoby's lobbying-vs-donations split rests on parameter choices. Furnas separates the two channels in data. Not cited.
- [[bombardini-2020-empirical-models-lobbying]] — complements — [[furnas-nd-conflating-lobbying-pacs]]: Both separate lobbying from PAC money. The review gives $3-3.5bn a year of lobbying against $200-250m a year of PAC donations.
- [[kim-2020-mapping-political-communities]] — same-data — [[bacik-2025-measuring-dynamical-evolution]]: Both are built from LobbyView with In Song Kim as co-author: a single-Congress (113th) snapshot vs a yearly network over 1999-2023.

## What it means for the systems

**Melange lobbying and contact inference ([[systems/melange-lobbying]]): the lobbied-member link.** This is the most consistent signal in the theme.

- LD-2 names no members, so member links are bill-roster joins. Supporting works: [[kim-2018-lobbyview-firm-level]], [[kim-2020-mapping-political-communities]], [[bacik-2025-measuring-dynamical-evolution]], [[defigueiredo-2013-advancing-empirical-research]], [[fremeth-2018-spillovers-regulating-corporate]], and [[boxsteffensmeier-2018-taking-congress-interest]] by analogy (solicited endorsements). This supports the circularity ruling and D-021.
- [[delano-2025-bicam-congressional-lobbying]] could serve as a second, independent check on bill resolution. It does not lift circularity.
- Paul's own LDA rebuild binds citations without an "of 20XX" suffix to the filing-year Congress. [[kim-2018-lobbyview-firm-level]] names that rule as the one that fails. The seam between the network.csv vintage and the rebuild is unmeasured.
- The contact-inference lane's abstention-first and "inferred contact plausibility" wording rules are consistent with every work here. None reports an accuracy benchmark for contact inference.

**Melange legislative data ([[systems/melange-legislative-data]]) and FEC attribution ([[systems/mentat-fec-attribution]]): the FEC-to-LDA bridge and the 42-month heuristic.**

- [[kim-2020-strategic-sequential-links]] and [[kim-2023-systemic-sequential-links]] find the donation-lobbying link strongest through committee referral. The bridge counts only sponsors and cosponsors and controls for committee seats, so its null answers a different question.
- If the bridge is reopened, add a committee-referral outcome. That is a measurement extension only; it does not license predictor language.
- [[furnas-nd-conflating-lobbying-pacs]] supports the rule that FEC exposure is never presented as a lobbying predictor. It also supports reading 42 months only as data-freshness lag, never as a lead time.

**Melange influence engine ([[systems/melange-influence-engine]]): flip model, money-response, persuadability.**

- Reviews support money as a minority, saturating channel: [[bombardini-2020-empirical-models-lobbying]], [[defigueiredo-2013-advancing-empirical-research]].
- The competitor-money leg sees only PAC dollars, so it misses lobby-only organizations ([[furnas-nd-conflating-lobbying-pacs]]) and spending displaced at the cap into CEO giving and lobbying ([[fremeth-2018-spillovers-regulating-corporate]]).
- [[furnas-2019-moneyed-interests-information]] does not calibrate cpf_pressure 1.7/0.5. Its asymmetry runs the other way, and two system comments misattribute claims to it.
- [[jacoby-2025-mathematical-modeling-analysis]] suggests sweeping the WM/WP channel weights and the PERS table in the sensitivity harness. Do not import its lobbying-vs-donations contrast.

**Melange allocation ([[systems/melange-allocation]]): break-even, ROI, joint optimizer.**

- None of the firm-side ROI figures ([[alexander-2009-lobbying-analysis-under]], [[huneeus-2018-effects-firms-lobbying]], [[bickmore-2025-corporate-lobbying-political]]) transfers to expected flips per PAC dollar. Word ROI widgets accordingly.
- Paul's think-tank-as-VC idea rests on a party-split, stated-intention, underpowered effect ([[furnas-2019-moneyed-interests-information]]). It is a hypothesis, not an allocation input.
- [[pritoni-2025-lobbying-coalitions-determinants]] questions CU as a lone actor but gives no basis for allied-money weights.

**Melange path-finding ([[systems/melange-pathfinding]]), SNA ([[systems/melange-sna]]) and staffer routes ([[systems/melange-staffer-network]]).**

- Route costs are heuristic model outputs, never closeness, access or introduction probabilities (D-025). [[holyoke-2015-learnable-skills-unteachable]] (UNVERIFIED) says relationships come from mutual need, not shared office time.
- The revolving-door revenue evidence ([[bombardini-2020-empirical-models-lobbying]], [[defigueiredo-2013-advancing-empirical-research]]) suggests at most a route-validity condition: whether the anchoring member is still in office. It must not become influence language on staffer or revolving-door edges.
- Paul's stated gap, converting bipartite lobbying models into path-finding graphs, is not solved by any work here. [[kim-2020-mapping-political-communities]] supplies mixed-membership communities, not paths. The dashboard's "exploitable community structure" citation of it should be corrected.

**Melange stance ([[systems/melange-stance]]): contested bills.**

- Issue-specific alignment ([[pritoni-2025-lobbying-coalitions-determinants]]) and per-bill positions ([[kim-2025-measuring-interest-group]]) support the per-bill registry.
- Positions stay evidence for adjudication, never weights, given the system's replication (precision 0.42, recall 0.49).
- LDA cannot measure spending per issue ([[kerr-2014-dynamics-firm-lobbying]], [[harrison-2018-application-data-mining]]).
- Alexander's largest returns came on an uncontested provision.

**Moneyball rank pillars ([[systems/moneyball-rank-pillars]]).**

- The committee channel is where the donation-lobbying link is strongest ([[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]]). [[huneeus-2018-effects-firms-lobbying]] computes committee relevance from data, where the pillar's tier weights are owner judgment.
- [[boxsteffensmeier-2018-taking-congress-interest]] shows passage depends heavily on majority-party sponsorship. The legislative pillar's efficiency term credits raw advancement rates without that adjustment.

**Mentat ([[systems/mentat-giving-forecast]], [[systems/mentat-sector-growth-study]]).**

- *Persistence nowcast:* backed by 92% persistence ([[kerr-2014-dynamics-firm-lobbying]]) and more than 80% ([[huneeus-2018-effects-firms-lobbying]]). Kerr's entry clusters in the pre-election year, so an omitted election-phase term is a known simplification.
- *Separate series:* in-house and outside-firm amounts are never added ([[harrison-2018-application-data-mining]], [[bacik-2025-measuring-dynamical-evolution]], [[bombardini-2020-empirical-models-lobbying]]). Paul's telecom report headlines about $139.4M a year. The Harrison bearing records that commit 914623152 removed a double-counted $139.4M telecom total on 2026-10-09, so the report figure should be rechecked against the separated series. The ABA-vs-CUNA figure likely carries the same mix (UNVERIFIED).
- *Sector-growth study:* four warnings.
  - Simultaneity from lobbying to revenue ([[huneeus-2018-effects-firms-lobbying]]).
  - Nickell bias from a lagged DV with fixed effects ([[kerr-2014-dynamics-firm-lobbying]]).
  - Why the coefficient flips under year fixed effects ([[defigueiredo-2013-advancing-empirical-research]]).
  - ROI and deep-pockets wording ([[bickmore-2025-corporate-lobbying-political]], [[alexander-2009-lobbying-analysis-under]]).

  [[furnas-2019-partisan-ties-lobbying]] supports modeling House and Senate separately.
- *Identity and attribution:* method sources are gvkey matching with constrained clustering ([[kim-2020-strategic-sequential-links]]) and 5% validation of fuzzy matches ([[huneeus-2018-effects-firms-lobbying]]). A PAC-at-limit x Chief-tier giving join is a cheap extension ([[fremeth-2018-spillovers-regulating-corporate]]). Furnas's organizational definition excludes employees, so align definitions before comparing to employer-match totals.

**Terrier.** Only [[holyoke-2015-learnable-skills-unteachable]] (UNVERIFIED) and [[furnas-2019-moneyed-interests-information]] (qualifies) touch Sphere of Influence. Neither backs a weight.

**Rules this theme does not disturb.** No work argues for dropping regular cosponsorship edges or for per-edge volume normalization; both remain rejected. No work bears on reciprocity, which remains not validated (D-104..D-108).

## Gaps

**No member-level causal evidence.** Every well-identified result in the theme concerns firm-side outcomes (revenue, share, lobbying spend) or stated staff intentions. None measures how a member's vote, cosponsorship or stance responds to lobbying. Jacoby is a simulation, and Box-Steffensmeier is observational and pooled. The influence engine's core question has no direct evidence here.

**No contact data and no accuracy benchmarks.**
- LDA records no contacts.
- LobbyView reports no error rates for bill resolution, and BICAM's precision and recall are in an SI that was not available.
- No work reports accuracy for contact inference.
- Paul's contact-identification results (focused activities 53.6% top-1 on unseen orgs, with LDA gold as a floor) have no published comparator.
- Fetch BICAM's Supplementary Information.

**Unresolved version conflict.** The 2020 and 2023 Kim-Stuckatz-Wolters versions disagree on magnitude and time profile. The published JOP version, which Paul cites as 2026, is not in the vault. It is needed before any timing claim is made, including Paul's "fades after about 6 months."

**Sector coverage.** Nothing here studies credit unions, community banks, fintech or crypto. The empirical anchors are manufacturing (Kim), AJCA repatriators led by pharma (Alexander), H-1B-dependent patenting firms (Kerr), energy (Kang via Bombardini) and climate (Jacoby). Trade associations, which dominate the system's bridge, are outside the firm-level samples.

**Unsigned, static networks.**
- No lobbying network here is both signed and temporal.
- Kim-Kunisky is one Congress and unsigned; Bacik is temporal and unsigned.
- Paul's gaps on temporal weighting, negative ties and inter-industry conflict remain open. Kim 2025 positions are the only signed raw material, and they are not validated for weighting.

**Path-finding.** No work converts bipartite lobbying structure into a graph that path-finding can use. That was Paul's explicit gap for Kim & Kunisky, and it remains open. Any attempt must respect D-021.

**Press, sentiment and news.** Paul's augmentation theme (news cycles and sentiment as drivers of change) has no work in this theme. Bacik has no press data.

**Coalition effects.** Pritoni finds evidence on whether coalitions win contradictory. Nothing here tests whether allied lobbying changes outcomes.

**Channels outside the data.** Employee giving, Super PACs, 501(c) dark money and unregistered or shadow lobbying are excluded by Furnas and flagged by Paul's own sector-growth design. Fremeth's cap displacement into CEO giving is the only work that touches channel shifting.

**State-level lobbying.** Only de Figueiredo's citation (19% higher state lobbying in budget periods, 38 states) touches it. Paul's Q11 (life insurance state regulation) has no support.

**Missing anchors Paul cites.** These appear in his notes or the related links but are not works in this theme:
- Kalla & Broockman, central to the access debate;
- Bonica (DIME);
- Ansolabehere et al. 2002;
- Cox & Magar;
- Grier, Munger & Roberts, the only theory anchor for the sign of the performance-spending relation;
- Imai & Kim 2021, the two-way fixed-effects caution;
- Aref & Neal.

Several may sit in other hubs or in the download queue.

**Annotation hygiene.** Four items need correction or confirmation:
- Paul's Kalla & Broockman citation is annotated with the Furnas staffer-survey summary.
- The Furnas information paper appears as 2019 (working paper), 2022 (Paul) and 2023 (published).
- Kunisky is spelled "Kuinsky" in Paul's notes.
- The huneeus-2018 slug year does not match the 2019 version that others cite.

**Coverage note.** This theme file holds 22 works, not 24. If the run expected 24 new works here, two are not in this theme's state file.

## All works

- [[alexander-2009-lobbying-analysis-under]] (2009) — Lobbying ROI: An Analysis under the American Jobs Creation Act of 2004 (also: Measuring Rates of Return for Lobbying Expenditures)
- [[bacik-2025-measuring-dynamical-evolution]] (2025) — Measuring the dynamical evolution of the United States lobbying network
- [[bickmore-2025-corporate-lobbying-political]] (2025) — Corporate lobbying, political action committees, and market concentration
- [[bombardini-2020-empirical-models-lobbying]] (2020) — Empirical Models of Lobbying
- [[boxsteffensmeier-2018-taking-congress-interest]] (2018) — Cue-Taking in Congress: Interest Group Signals from Dear Colleague Letters
- [[defigueiredo-2013-advancing-empirical-research]] (2013) — Advancing the Empirical Research on Lobbying
- [[delano-2025-bicam-congressional-lobbying]] (2025) — Bulk Ingestion of Congressional Actions and Materials Dataset (BICAM)
- [[fremeth-2018-spillovers-regulating-corporate]] (2018) — Spillovers from regulating corporate campaign contributions
- [[furnas-2019-moneyed-interests-information]] (2019) — Moneyed Interests, Information, and Action in Congress: A Survey Experiment
- [[furnas-2019-partisan-ties-lobbying]] (2019) — The partisan ties of lobbying firms
- [[furnas-nd-conflating-lobbying-pacs]] () — Conflating Lobbying and PACs: The Surprisingly Low Overlap in Organizational Lobbying and Campaign Expenditures
- [[harrison-2018-application-data-mining]] (2018) — An Application of Data Mining Techniques to Explore Congressional Lobbying Records for Patterns in Pediatric Special Interest Expenditures Prior to the Affordable Care Act
- [[holyoke-2015-learnable-skills-unteachable]] (2015) — Learnable Skills, or Unteachable Instinct? What Can and What Cannot Be Taught in the Lobbying Profession
- [[huneeus-2018-effects-firms-lobbying]] (2018) — The Effects of Firms' Lobbying on Resource Misallocation
- [[jacoby-2025-mathematical-modeling-analysis]] (2025) — Mathematical Modeling, Analysis and Simulation Utilizing Machine Learning Tools for Assessing the Impact of Climate Lobbying
- [[kerr-2014-dynamics-firm-lobbying]] (2014) — The Dynamics of Firm Lobbying
- [[kim-2018-lobbyview-firm-level]] (2018) — LobbyView: Firm-level Lobbying & Congressional Bills Database
- [[kim-2020-mapping-political-communities]] (2020) — Mapping Political Communities: A Statistical Analysis of Lobbying Networks in Legislative Politics
- [[kim-2020-strategic-sequential-links]] (2020) — Strategic and Sequential Links between Campaign Donations and Lobbying
- [[kim-2023-systemic-sequential-links]] (2023) — Systemic and Sequential Links between Campaign Donations and Lobbying
- [[kim-2025-measuring-interest-group]] (2025) — Measuring Interest Group Positions on Legislation: An AI-Driven Analysis of Lobbying Reports
- [[pritoni-2025-lobbying-coalitions-determinants]] (2025) — Lobbying coalitions and their determinants: a systematic literature review
