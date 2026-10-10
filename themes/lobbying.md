---
type: theme
name: "Lobbying"
paper_count: 19
tags: [theme]
---

# Lobbying

The Lobbying theme holds 19 works. Most of them depend on one data source: the Lobbying Disclosure Act (LD-2) filings as cleaned and linked by In Song Kim's LobbyView group. Paul's own notes on the category come from his Lobbying annotated bibliography, the returns-and-access annotation file, the contact-inference spec and evaluation plan, the contact-identification state record, the sector growth study plan and data memos. They take two stances that run through everything below.

1. **The dataset is the main asset, and its lineage is a warning.** Paul treats LobbyView ([[kim-2018-lobbyview-firm-level]], [[bacik-2025-measuring-dynamical-evolution]]) as the foundational source. [[harrison-2018-application-data-mining]] is his pipeline tutorial. His contact-inference spec, however, treats the LobbyView chain ([[kim-2018-lobbyview-firm-level]], [[kim-2020-mapping-political-communities]], [[kim-2025-measuring-interest-group]]) as one cautionary lineage, because it asserts undocumented inference as if it were capability. One example is the bill-to-Congress argmax, which always returns an answer.
2. **Lobbying is an input that can be measured and optimized.** [[huneeus-2018-effects-firms-lobbying]] shows lobbying spend returning revenue. [[furnas-2019-moneyed-interests-information]] shows information and ideology shaping how staff receive a request. Paul's stated implication for allocation is to set fundraising targets, fund aligned think tanks as a channel he likens to venture capital, and choose targets by state-level industry presence.

The literature's strongest shared finding is structural. LD-2 never names the members a lobbyist contacted. Every member-level lobbying link in these works is therefore a join through a bill roster or committee assignment: [[kim-2020-mapping-political-communities]], [[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]], [[bacik-2025-measuring-dynamical-evolution]], [[delano-2025-bicam-congressional-lobbying]]. That independently backs the system's ruling that the lobbied-member link is circular. It also backs Paul's own measured result in the contact-identification lane: about 0.3% of LDA activities name anyone, so all contact gold is a floor.

The second shared finding is about concentration and persistence:
- A small minority of organizations does both lobbying and PAC giving, and that minority holds most of the money ([[kim-2020-strategic-sequential-links]], [[furnas-nd-conflating-lobbying-pacs]]).
- Lobbying is highly persistent year to year ([[huneeus-2018-effects-firms-lobbying]], [[bombardini-2020-empirical-models-lobbying]]).

The theme splits on two questions:
- **Does money buy access?** Observational dyad panels and a staff survey experiment point in different directions.
- **How large are the returns to lobbying?** The estimates range from an elasticity of about 0.2 to a ratio of 220:1. They measure different things and none of them carries over to member behaviour.

No work in the theme estimates, with a causal design, how lobbying changes a legislator's vote or flip.

## Strands

### Data infrastructure: LDA, LobbyView and how bills get resolved

These works treat LD-2 disclosures as raw material and differ in how much they clean, link and infer.

**[[harrison-2018-application-data-mining]]** parses one quarter of Senate XML (Q4 2009). Its value is a list of hazards:
- The amount field mixes in-house expenses with outside-firm income.
- Expenses under $5,000 appear only as a checkbox, and income is rounded to the nearest $10,000.
- One filing bundles many issues, so dollars cannot be assigned to a single issue.
- Files are grouped by filing date, so a Q4 2009 quarter carries activity dated 2002-2008.

Paul calls it the pipeline tutorial and notes that LobbyView supersedes it. Mentat's `sector_spend.py` now enforces its main warning: in-house and outside-firm series are kept apart and never added (D-064, commit 914623152).

**[[kim-2018-lobbyview-firm-level]]** is a partial technical manual. Bill mentions are found by regex. Each mention is assigned to a Congress by the argmax of cosine similarity over the three preceding Congresses, then propagated across the report by majority vote, and nearby bill numbers (gap of 10 or less) are expanded into ranges. No error rate is reported for any of these steps. Paul's contact-inference spec singles out this argmax-always-answers behaviour as the defect his own layer avoids by making abstention a first-class output.

**[[delano-2025-bicam-congressional-lobbying]]** (BICAM) re-links LD-2 Section 16 bill text to 421,764 bills (6th-119th Congress) using distance scores, with low-confidence matches reviewed by hand. It claims to find significantly more bill lobbying than [[kim-2020-mapping-political-communities]]. Its precision and recall are in an SI that was not available here.

**[[kim-2025-measuring-interest-group]]** adds signed positions (Support, Oppose or Engage) to LobbyView reports for 2009-2022. The figures:
- 279,104 group-by-bill positions in total. 70.5% of them are GNN imputations, at 78.5% accuracy and an Oppose F1 of 64.
- LLM agreement with human coders is 96.9%, but only on the 391 items where both coders agreed.
- When the system rebuilt the labeller with Haiku and scored it at natural base rates, Support and Oppose came out at precision 0.42 and recall 0.49.

**[[bacik-2025-measuring-dynamical-evolution]]** analyzes 1,277,411 filings (1999-2023) and states plainly that legislators enter the data only through lobbyists' undated covered-position disclosures.

**Where they agree.** The bill, not the member, is the unit LD-2 supports. Industry classification is fragile: NAICS covers about 40% in Bacik, and the system has ruled NAICS to be linkage metadata only.

**Where they disagree.** On bill resolution, LobbyView's similarity argmax, BICAM's distance-plus-review, and Paul's own 2021+ rebuild (filing-year binding, no range expansion) are three different instruments. The seams between them have not been measured.

Works: [[kim-2018-lobbyview-firm-level]], [[harrison-2018-application-data-mining]], [[delano-2025-bicam-congressional-lobbying]], [[kim-2025-measuring-interest-group]], [[bacik-2025-measuring-dynamical-evolution]]

### Network structure of the lobbying system

**[[kim-2020-mapping-political-communities]]** fits a bipartite link-community model to group-by-politician counts for the 113th Congress, with k = 8. It finds eight communities that match committee jurisdictions. Specialists (at least 85% of activity in one community) coexist with generalists (no community above 25%). It concludes that narrow industry interest drives most lobbying. Its edges are bill-roster joins, so the paper never claims contact.

**[[bacik-2025-measuring-dynamical-evolution]]** shows the same system over time, 1999-2023:
- 200-500 registrants enter each year, and about 20% of the registrants active in 2000 were still active in 2020.
- Up to one-third of client-registrant ties are rewired each year.
- Attachment is linearly preferential, and money is concentrated (Gini about 0.8).
- Senate cross-party cosponsorship fell about 40% while lobbyist bipartisanship did not.

**[[huneeus-2018-effects-firms-lobbying]]** adds a sparsity fact: across 65,047 bills, the median bill draws 2 lobbying clients (mean 3.45).

**[[pritoni-2025-lobbying-coalitions-determinants]]** is a scoping review of 80 articles (1985-2023). Its findings:
- SNA is the main design in only 4 of 80 articles (5%), and SNA-based tests have the weakest support record, 5 of 9.
- Across 77 hypotheses, 74% are supported. That figure pools US and EU studies and is UNSTRATIFIED by polity.
- The evidence on whether coalitions win is contradictory and left open.

**[[boxsteffensmeier-2018-taking-congress-interest]]** builds a centrality measure from the amicus co-signing network. It shows that a highly central endorser raises cosponsor counts: predicted cosponsors go from 33 with no endorsement to 51 with a top-centrality endorser (6,597 House bills, 106th-111th Congresses). Centrality has no direct effect on committee reporting or passage.

**Agreement.** Committees are the organizing axis of the lobbying network: in Kim-Kunisky's communities, in Huneeus-Kim's committee-share instrument, and in the Kim-Stuckatz-Wolters committee channel.

**Tension.** Kim-Kunisky's mixed-membership communities, Bacik's heavy churn and Huneeus's median of 2 clients per bill describe a fragmented, specialist market. Pritoni's coalition literature assumes groups pool resources. The two are reconcilable: the communities are group-by-member blocs, while Pritoni's coalitions are group-to-group coordination. Neither shows that coalitions raise influence.

Paul's named gaps for this strand: no path-finding algorithm over a bipartite lobbying graph, and no temporal tracking of coalition ideology against national mood.

Works: [[bacik-2025-measuring-dynamical-evolution]], [[kim-2020-mapping-political-communities]], [[pritoni-2025-lobbying-coalitions-determinants]], [[boxsteffensmeier-2018-taking-congress-interest]], [[huneeus-2018-effects-firms-lobbying]]

### Money and lobbying: sequence, overlap and access

**Agreed: overlap is low and concentrated.**
- [[kim-2020-strategic-sequential-links]] / [[kim-2023-systemic-sequential-links]]: in 2013, 519 of 11,829 public firms (about 4%) both donated and lobbied and held about 85% of $3.1bn. The 2023 version gives 518 of 9,791 (5%) holding 89% of $2.86bn.
- [[furnas-nd-conflating-lobbying-pacs]]: pooled over 1998-2018, 3,427 of 54,517 organizations (6.3%) did both and accounted for about 75% of lobbying spend.

The populations differ (public manufacturing firms versus all CRP organizations), but the shape is the same.

**Contested: does money buy access?** The Kim-Stuckatz-Wolters panel covers 2,703 public manufacturing firms and 979 members, 2008-2017/18. A first-time donation precedes lobbying on bills tied to that member, mostly through committee referral and corporate PAC money. The authors read this as access-buying timing, not quid pro quo.

[[furnas-2019-moneyed-interests-information]] cuts against it. In a vignette survey of 441 staffers (5.2% response), donors get no meeting advantage over constituents and staff say they use donor information less. That evidence is stated intention, underpowered: 46 of 11,904 pairwise tests survive FDR correction.

[[holyoke-2015-learnable-skills-unteachable]] argues without data that campaign finance matters less than assumed. Paul's annotation file mislabels this cluster: an entry cited as Kalla and Broockman carries a summary of the Furnas staffer survey, and needs his confirmation.

**Internal version conflict.** The two Kim-Stuckatz-Wolters drafts disagree:

| | 2020 draft | 2023 version |
|---|---|---|
| Within-pair FE coefficient | 0.102-0.118 (base rate about 0.05) | 0.023-0.048 (base rate about 0.07) |
| Senate matched DiD over time | about 8.5 pp at t, falling to about 3 pp by t+3 | 7.5 pp at t+0, rising to 11 pp at t+3 |

Paul's note that donation effects fade after about 6 months matches neither version and was already flagged as unchecked.

**Partisan channel.** [[furnas-2019-partisan-ties-lobbying]] (Furnas, Heaney and LaPira) covers 1,603 contract registrants and 33,243 firm-quarters, 2008Q1-2016Q3. Alignment with the House majority adds about $5,000-6,000 per lobbyist per quarter, supported in 12 of 13 House tests. Senate ties are weak, supported in 3 of 13. Bipartisan firms earn the most. That is a market valuation of perceived access, not a measure of persuasion and not an access probability.

Works: [[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]], [[furnas-nd-conflating-lobbying-pacs]], [[furnas-2019-moneyed-interests-information]], [[furnas-2019-partisan-ties-lobbying]], [[holyoke-2015-learnable-skills-unteachable]]

### Returns to lobbying for the firm

Three empirical works estimate private returns, each with a different estimand.

**[[huneeus-2018-effects-firms-lobbying]]** links LDA filings (1999-2015) to Compustat public firms:
- It uses a shift-share instrument built from committee seats.
- The IV elasticity of sales on lobbying is 0.319 for all years (first-stage F 14.3) and 0.227 after 2007 (F 13.1). OLS gives about 0.04-0.05.
- The structural model says removing the return to lobbying raises aggregate productivity by 22% at sigma_C = 4. That figure moves from 38% to 13% as sigma_C goes from 2 to 6.

**[[bickmore-2025-corporate-lobbying-political]]** uses CRP plus Compustat (1998-2016, 116,242 firm-years with full controls). Lobbying firms hold 0.0743 more of their industry's assets. That is 7.4 percentage points on a 0-1 share with mean 0.0727. It is not a 7.4% relative rise. The estimate has no firm fixed effects and is UNSTRATIFIED by industry. The Abramoff difference-in-differences shows a pre-trend bending in late 2004.

**[[alexander-2009-lobbying-analysis-under]]** studies the 2004 AJCA repatriation deduction (93 lobbying firms out of 476 repatriators) and reports about 220:1. The ratio is driven by outliers: the per-company mean is 869:1 and the median 172:1. The lobbying denominator is an upper bound, and the abstract's claim that cash holdings do not matter is contradicted by its own Table 5.

**[[bombardini-2020-empirical-models-lobbying]]** is the referee for this strand. It argues that ROI figures computed from observable political money are routinely implausible. Its examples are a naive Farm Bill return of about 600,000% and Kang's implied value of more than $500m per policy. It reads these as missing channels or misspecification.

**Agreement.** Lobbying is rare, firm size and lobbying go together, and lobbying persists (more than 80% of 2016 lobbyists lobbied again in 2017; 92% year to year in Kerr et al. as reviewed).

**Disagreement.** Mostly scale, from an elasticity of about 0.2-0.3 to 220:1. The three are not comparable: an economy-wide elasticity, an industry-share gap, and a single-provision tax rent.

Paul's annotations accept the returns cluster (Alexander, Bickmore, Huneeus-Kim). He adds the caveat that part of the revenue gain comes at the wider economy's expense, and uses the cluster for setting allocation targets. Huneeus's causal direction (lobbying raises revenue) is a simultaneity warning for his sector growth study, which posits that growth raises spending (H1, deep pockets).

Works: [[huneeus-2018-effects-firms-lobbying]], [[bickmore-2025-corporate-lobbying-political]], [[alexander-2009-lobbying-analysis-under]], [[bombardini-2020-empirical-models-lobbying]]

### From lobbying signals to legislative outcomes

This is the thinnest strand, and the one closest to Melange's flip and recruitment engines.

**[[kim-2025-measuring-interest-group]]** describes how positions move across the legislative process:
- Support rises toward enactment.
- Oppose and Amend rise late in bills that fail, a finding that rests on 3 vetoed bills.
- Its one-dimensional group score correlates only weakly with DIME CFscores (r = 0.19, n = 218).

**[[boxsteffensmeier-2018-taking-congress-interest]]** finds outside endorsements act early, on cosponsorship. At the floor, only breadth (the count of endorsing groups) predicts passage, and the largest passage predictors are majority-party sponsorship and hot-bill status.

**[[jacoby-2025-mathematical-modeling-analysis]]** runs a compartmental ODE with every parameter fixed in advance, checked against one House vote (Waxman-Markey, 2009). Adding donations moves 1 member. Adding lobbying resolves 8 undecided members. Widening the centrist band from ±0.25 to ±0.35 reverses the result. These are contrasts between simulation runs, not estimates. Its lobbyist counts come from revolving-door staff, so they cannot back any influence reading (influence language is barred on revolving-door edges).

**[[bombardini-2020-empirical-models-lobbying]]** reports Kang's energy-policy estimates: only 8% of 538 policies are enacted, and lobbying raises enactment by 0.021% to 0.415%.

**[[furnas-2019-moneyed-interests-information]]** finds staff side more often with requests to stop a bill than to propose one.

**Convergence.** Kim 2025's late-Oppose finding and Furnas's stop-a-bill asymmetry both suggest opposition carries weight late in the process. Both are thin: 3 vetoed bills, and stated intention in a survey.

**Agreement.** Money is a small, saturating channel relative to whether a member is movable at all. That supports the flip model's structure but calibrates none of its constants.

Works: [[kim-2025-measuring-interest-group]], [[boxsteffensmeier-2018-taking-congress-interest]], [[jacoby-2025-mathematical-modeling-analysis]], [[bombardini-2020-empirical-models-lobbying]], [[furnas-2019-moneyed-interests-information]]

## Agreements and debates

**1. Does campaign money buy access, and does it buy anything beyond access?**

The two sides:
- **For an access-buying sequence:** [[kim-2020-strategic-sequential-links]] and [[kim-2023-systemic-sequential-links]]. In a within firm-politician panel (public manufacturing firms, 2008-2017/18), a first-time donation precedes lobbying on bills tied to that member. The link is strongest for corporate PACs and committee referral. [[kim-2023-systemic-sequential-links]] finds no reverse link: lobbying does not predict later donations, apart from a small t+0 sponsor effect.
- **Against a donor premium:** [[furnas-2019-moneyed-interests-information]] (441 staffers, vignette design) finds no donor meeting advantage and sets itself explicitly against Kalla and Broockman. [[holyoke-2015-learnable-skills-unteachable]] argues, without data, that campaign finance matters less than assumed.

On whether money changes votes:
- [[jacoby-2025-mathematical-modeling-analysis]] gets "donations move 1 member", but only from fixed parameters.
- [[bombardini-2020-empirical-models-lobbying]] concludes that money's measured effects on policy are small, and that implied returns are implausible unless electoral and informational channels are added.

Resolution in the literature is partial. The sides use different units (dyad-years versus stated staff intentions) and different outcomes (lobbying a related bill versus taking a meeting). They are not strictly contradictory. Nothing here supports a money-to-vote coefficient.

**2. How fast does the donation-lobbying link decay?** The two versions of the Kim-Stuckatz-Wolters paper disagree on direction:
- [[kim-2020-strategic-sequential-links]]: Senate effect about 8.5 pp at t, falling to about 3 pp by t+3.
- [[kim-2023-systemic-sequential-links]]: 7.5 pp rising to 11 pp (Senate) and 8.6 to 12.7 pp (House).

The within-pair FE coefficient also drops, from about 0.11 to 0.023-0.048. The system currently cites the 2020 draft's "concurrent and gone within two to three years" reading against its own "FEC leads LDA by 42 months" heuristic. Neither version supports a 42-month lead. The two drafts should be reconciled before either is quoted.

**3. What are lobbying's returns, and to whom?**

| Work | Estimate | What it measures |
|---|---|---|
| [[alexander-2009-lobbying-analysis-under]] | 220:1 | a single uncontested tax provision |
| [[huneeus-2018-effects-firms-lobbying]] | IV elasticity 0.2-0.3 on sales; 22% aggregate productivity loss at sigma_C = 4 | economy-wide |
| [[bickmore-2025-corporate-lobbying-political]] | +7.4 pp of industry assets | cross-sectional, no firm FE |

[[bombardini-2020-empirical-models-lobbying]] says figures like these are fragile by construction. All of them are firm-side rents. None measures member response.

**4. Is lobbying partisan or bipartisan?**
- [[furnas-2019-partisan-ties-lobbying]]: 42-60% of contract firms are partisan, but bipartisan firms earn the most, and House-majority alignment is worth about $5-6k per lobbyist per quarter.
- [[bacik-2025-measuring-dynamical-evolution]]: lobbyist bipartisanship did not fall while Senate cross-party cosponsorship fell about 40%.
- [[furnas-2019-moneyed-interests-information]]: staff penalize evidence from the other side's think tank about twice as much as they reward aligned evidence.
- [[bickmore-2025-corporate-lobbying-political]]: Democratic and Republican PAC coefficients are similar.

The emerging picture: suppliers of lobbying hedge across parties, while the staff who receive it filter by party.

**5. Persistence versus churn.** [[huneeus-2018-effects-firms-lobbying]] (more than 80% of firms repeat) and [[bombardini-2020-empirical-models-lobbying]] (92%) say lobbying persists. [[bacik-2025-measuring-dynamical-evolution]] says only 20% of registrants survive 2000-2020 and up to a third of client-registrant ties rewire each year. These differ in unit, not in fact: clients persist while their hired firms churn. That matters for any edge built on registrants.

**6. Do coalitions matter?** [[pritoni-2025-lobbying-coalitions-determinants]] finds the evidence on coalition success contradictory. [[alexander-2009-lobbying-analysis-under]] reports higher ROI for coalition members (257:1 against 154:1 for solo lobbyists), but cross-sectionally and on an uncontested provision. There is no settled evidence that pooling raises influence.

**Rejected proposals.** No work in this theme argues for dropping regular cosponsorship edges or for per-edge volume normalization. [[kim-2020-mapping-political-communities]] normalizes community parameters (each community's alphas sum to 1), which is not per-edge volume normalization. If either proposal is read into these works later, it must be flagged, not adopted. Per Paul's contact-identification kill-ledger, per-edge normalization stays settled.

## Methods

**Dominant data source.** LDA LD-2 filings, mostly through LobbyView, underlie 10 of the 19 works directly ([[kim-2018-lobbyview-firm-level]], [[kim-2020-mapping-political-communities]], [[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]], [[huneeus-2018-effects-firms-lobbying]], [[bacik-2025-measuring-dynamical-evolution]], [[kim-2025-measuring-interest-group]], [[delano-2025-bicam-congressional-lobbying]], [[harrison-2018-application-data-mining]], [[alexander-2009-lobbying-analysis-under]]). Three more use CRP-curated LDA ([[furnas-nd-conflating-lobbying-pacs]], [[bickmore-2025-corporate-lobbying-political]], [[furnas-2019-partisan-ties-lobbying]]).

Shared weaknesses of that source:
1. **No contact field.** LD-2 never names members, so every member link is a bill-roster or committee join. P(joined | lobbied) = 1 by construction.
2. **Mixed series.** Expense and income are mixed, and amounts are rounded or shown only as a checkbox below $5,000 ([[harrison-2018-application-data-mining]]).
3. **Unvalidated bill resolution.** Bill-to-Congress heuristics have unreported error rates ([[kim-2018-lobbyview-firm-level]]). BICAM's accuracy figures are not in its main text.
4. **Bundled issues and filing-date timing**, which blur dollars by issue and by period.
5. **Public-firm bias.** Compustat linkage (Kim-Stuckatz-Wolters, Huneeus-Kim, Bickmore, Alexander) restricts samples to public firms. Trade associations, credit unions and other non-public organizations, which dominate Melange's and Mentat's finance scope, fall outside them.

**Estimation designs, with their main weaknesses.**
- **Fixed-effects linear probability panels with matched difference-in-differences** ([[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]]). The authors call the results associations. The pooled FE estimates are UNSTRATIFIED by chamber or donor type, and the two drafts give conflicting time paths.
- **Shift-share IV plus a structural SMM model** ([[huneeus-2018-effects-firms-lobbying]]). First-stage F is about 13-14, which is borderline. The model uses an identity weighting matrix, and the counterfactual swings from 13% to 38% across sigma_C.
- **OLS with industry and year fixed effects but no firm FE, plus an event DiD** ([[bickmore-2025-corporate-lobbying-political]]). The pre-trend violation is visible, and placebo results appear only in figures.
- **Cross-sectional ROI ratios** ([[alexander-2009-lobbying-analysis-under]]). The denominator is an upper bound and there is no identification.
- **Panel regressions and kernel-weighted DiD around chamber flips** ([[furnas-2019-partisan-ties-lobbying]]). Only two flips are available, and the Senate result depends on specification.
- **Factorial survey experiment** ([[furnas-2019-moneyed-interests-information]]). It measures stated intention on hypothetical requests, with a 5.2% response rate, is underpowered, and reports AMCEs only as plots.
- **Statistical network models** ([[kim-2020-mapping-political-communities]]: biLCM fitted by EM, k = 8, one Congress) and **descriptive network physics** ([[bacik-2025-measuring-dynamical-evolution]]). These are descriptive, not causal. Kim-Kunisky's k is chosen by eye from a latent space model.
- **LLM labelling plus GNN imputation** ([[kim-2025-measuring-interest-group]]). 70.5% of the labels are imputed, Oppose is the weakest class, and validation against human coders uses only the items both coders agreed on.
- **Negative binomial and logit models of bill progress** ([[boxsteffensmeier-2018-taking-congress-interest]]). There is no causal design, and endorsements may be solicited by the sponsor's own office.
- **Deterministic ODE simulation** ([[jacoby-2025-mathematical-modeling-analysis]]). Parameters are fixed, not estimated, and the model is checked against one bill.
- **Narrative review** ([[bombardini-2020-empirical-models-lobbying]]) and a **scoping vote-count review** ([[pritoni-2025-lobbying-coalitions-determinants]]). The 74% support rate pools polities and is UNSTRATIFIED. Vote-counting also ignores effect sizes.
- **Conceptual essay** with no data ([[holyoke-2015-learnable-skills-unteachable]]).

**What is missing across all of them.** No work has external ground truth for who was contacted. Paul's evaluation plan notes the LobbyView papers report zero accuracy figures for contact inference. No work estimates member-level response to lobbying with a credible causal design.

## How the works relate

- [[kim-2023-systemic-sequential-links]] — builds-on (later version, same data; estimates diverge) — [[kim-2020-strategic-sequential-links]]: Same authors, design and panel. The FE coefficient falls from 0.102-0.118 to 0.023-0.048, and the Senate DiD path changes direction (2020: about 8.5 pp at t, falling to about 3 pp by t+3; 2023: 7.5 pp rising to 11 pp). Reconcile before citing either.
- [[kim-2020-strategic-sequential-links]] — builds-on — [[kim-2020-mapping-political-communities]]: States that its lobbying data build on the Kim and Kunisky data set (fn 4).
- [[kim-2020-strategic-sequential-links]] — builds-on — [[kim-2018-lobbyview-firm-level]]: LobbyView is cited as the source of the firm-bill lobbying data.
- [[kim-2020-mapping-political-communities]] — builds-on / same-data — [[kim-2018-lobbyview-firm-level]]: Lobbying data distilled from LobbyView, sharing its NLP bill-identification pipeline.
- [[huneeus-2018-effects-firms-lobbying]] — same-data — [[kim-2018-lobbyview-firm-level]]: Uses the firm-disambiguated LobbyView data merged with Compustat.
- [[bacik-2025-measuring-dynamical-evolution]] — same-data — [[kim-2018-lobbyview-firm-level]]: Builds a yearly five-layer network from the current LobbyView database, 1999-2023.
- [[bacik-2025-measuring-dynamical-evolution]] — same-data / complements — [[kim-2020-mapping-political-communities]]: Same LobbyView bill links. Bacik gives time evolution, and Kim-Kunisky give a 113th-Congress community snapshot.
- [[kim-2025-measuring-interest-group]] — cites / extends — [[kim-2020-mapping-political-communities]]: Cites it as prior lobbying-network work and supplies the for/against sign that Kim-Kunisky say co-lobbying lacks.
- [[kim-2025-measuring-interest-group]] — extends — [[harrison-2018-application-data-mining]]: Does at scale (2009-2022) the text analysis of LDA issue text that Harrison proposes as future work.
- [[harrison-2018-application-data-mining]] — same-data (superseded) — [[kim-2018-lobbyview-firm-level]]: Both parse LDA reports. Paul's note treats LobbyView as superseding Harrison's one-quarter parse.
- [[delano-2025-bicam-congressional-lobbying]] — extends — [[kim-2020-mapping-political-communities]]: BICAM claims its Bill-to-LDA linkage finds significantly more bill lobbying than Kim and Kunisky 2021.
- [[delano-2025-bicam-congressional-lobbying]] — alternative bill-resolution path (same lineage) — [[kim-2018-lobbyview-firm-level]]: Distance-score matching with manual review, in place of LobbyView's cosine-similarity argmax. It could serve as an independent check.
- [[bacik-2025-measuring-dynamical-evolution]] — cites — [[bombardini-2020-empirical-models-lobbying]]: Cites the review as the survey of empirical lobbying models.
- [[bacik-2025-measuring-dynamical-evolution]] — cites / agrees — [[furnas-2019-partisan-ties-lobbying]]: Cites Furnas, Heaney and LaPira 2019. Both find lobbying is not polarized and that bipartisan connections earn more.
- [[bombardini-2020-empirical-models-lobbying]] — cites — [[huneeus-2018-effects-firms-lobbying]]: Reports Huneeus-Kim's 10%-to-3% revenue result and 22% productivity loss, and warns that the minimizing parameter may not be unique.
- [[bombardini-2020-empirical-models-lobbying]] — qualifies — [[alexander-2009-lobbying-analysis-under]]: The review's argument that ROI computed from observable political spending is routinely implausible applies directly to the 220:1 figure.
- [[kim-2023-systemic-sequential-links]] — cites — [[huneeus-2018-effects-firms-lobbying]]: Cites Huneeus and Kim (2019) for firm-level returns to lobbying.
- [[furnas-nd-conflating-lobbying-pacs]] — agrees (independent) — [[kim-2020-strategic-sequential-links]]: Both find a small dual-activity minority holding most money (Kim: about 4% of 11,829 public firms hold 85%, 2013; Furnas: 6.3% of 54,517 orgs hold about 75% of lobbying spend, 1998-2018). Not cited by each other.
- [[furnas-nd-conflating-lobbying-pacs]] — cites (same author) — [[furnas-2019-moneyed-interests-information]]: The PS article cites the published 2023 version of the staffer survey experiment as contrary evidence on buying access.
- [[furnas-2019-moneyed-interests-information]] — in tension — [[kim-2023-systemic-sequential-links]]: Survey: donors get no meeting advantage. Panel: first-time donations precede lobbying on the donee's bills. The units and outcomes differ, so this is not a strict contradiction.
- [[furnas-2019-partisan-ties-lobbying]] — complements (same lead author) — [[furnas-2019-moneyed-interests-information]]: Supply side (partisan identity of lobbying firms) against demand side (staff partisan filtering of evidence).
- [[holyoke-2015-learnable-skills-unteachable]] — complements — [[furnas-2019-partisan-ties-lobbying]]: Holyoke's cited claim that lobbyists who give are partisan is tested empirically by Furnas, Heaney and LaPira (LaPira co-authors both).
- [[holyoke-2015-learnable-skills-unteachable]] — supported-by — [[furnas-nd-conflating-lobbying-pacs]]: Furnas's low lobbying-PAC overlap gives data to Holyoke's unquantified claim that few lobbyists engage in campaign finance.
- [[huneeus-2018-effects-firms-lobbying]] — complements (different estimand, different data) — [[bickmore-2025-corporate-lobbying-political]]: Both relate lobbying to firm size. Huneeus uses LobbyView with an IV elasticity, Bickmore uses CRP with industry-share gaps and no firm FE. Not cited by each other.
- [[alexander-2009-lobbying-analysis-under]] — complements (different estimand) — [[huneeus-2018-effects-firms-lobbying]]: Single-provision tax-rent ROI against an economy-wide revenue elasticity. Do not compare magnitudes directly.
- [[jacoby-2025-mathematical-modeling-analysis]] — cites — [[kim-2020-mapping-political-communities]]: Cites Kim and Kunisky only as a data source for vote prediction, not for network modelling.
- [[jacoby-2025-mathematical-modeling-analysis]] — qualified-by — [[furnas-nd-conflating-lobbying-pacs]]: Jacoby's lobbying-versus-donations split rests on chosen parameters. Furnas shows the two channels come largely from different organizations.
- [[boxsteffensmeier-2018-taking-congress-interest]] — complements — [[kim-2025-measuring-interest-group]]: Two bill-level sources of outside-group signals: Dear Colleague endorsements, and LDA-derived positions.
- [[boxsteffensmeier-2018-taking-congress-interest]] — complements — [[furnas-2019-moneyed-interests-information]]: Both study interest-group cues reaching overloaded congressional offices.
- [[pritoni-2025-lobbying-coalitions-determinants]] — complements (different coalition sense) — [[kim-2020-mapping-political-communities]]: Pritoni reviews group-to-group coalitions. Kim-Kunisky model group-by-politician link communities. Pritoni finds SNA designs rare and weakest supported (5 of 9).
- [[kim-2025-measuring-interest-group]] — convergent (thin) — [[furnas-2019-moneyed-interests-information]]: Late Oppose surges in failed bills (3 vetoed bills) and staff siding with stop-a-bill requests both point to opposition asymmetry. Both are weakly evidenced.
- [[bacik-2025-measuring-dynamical-evolution]] — reconcilable (different unit) — [[huneeus-2018-effects-firms-lobbying]]: Registrant churn (20% survive 2000-2020) against client persistence (more than 80% repeat 2016 to 2017). Clients persist while their hired firms churn.

## What it means for the systems

Bearings below come from each work's bearing tags and summaries.
- **VERIFIED-SOFTENED** means verified but with softened wording.
- **UNVERIFIED** bearings are single-agent leads, not findings (DE-062).
- Several summaries (Kim-Stuckatz-Wolters, Huneeus-Kim, Furnas-Heaney-LaPira) self-declare as UNVERIFIED agent analysis until a second path reproduces them, even where the tag is VERIFIED-SOFTENED. Read those as leads.

**Melange: the lobbied-member link and contact inference** ([[systems/melange-lobbying]], [[systems/melange-legislative-data]])
- The theme converges on one point: LD-2 never names the members contacted. This is backed by [[kim-2020-mapping-political-communities]], [[kim-2018-lobbyview-firm-level]] and [[bacik-2025-measuring-dynamical-evolution]] (supports), [[boxsteffensmeier-2018-taking-congress-interest]] (supports), and [[delano-2025-bicam-congressional-lobbying]], [[kim-2025-measuring-interest-group]], [[kim-2020-strategic-sequential-links]] and [[kim-2023-systemic-sequential-links]] (qualifies). That is consistent with the system's ruling that the link is circular, and with D-021: LDA is an identity source, not an edge or weight source.
- BICAM offers a second, independent bill-resolution path to check the 2021+ rebuild. It cannot remove the circularity.
- The contact-plausibility layer must keep saying "inferred contact plausibility", never "lobbied" or "contacted". The literature offers no external accuracy benchmark for contact inference.

**Melange: the FEC-to-LDA bridge** ([[systems/mentat-fec-attribution]], [[systems/melange-lobbying]])
- [[kim-2020-strategic-sequential-links]] and [[kim-2023-systemic-sequential-links]] qualify the bridge's null result. Their effect is within firm-member pairs, timed to the first donation, and driven by committee referral. The bridge credits only sponsors and cosponsors and controls for committee seats, so it may absorb the very channel Kim finds strongest.
- [[furnas-nd-conflating-lobbying-pacs]] supports never presenting FEC exposure as a lobbying predictor.
- Neither the 2020 nor the 2023 draft supports a 42-month FEC lead.

**Melange: influence engine (flip model, money-response, cascade, lt-priming)** ([[systems/melange-influence-engine]])
- Treating money as a minority, saturating channel is supported by [[bombardini-2020-empirical-models-lobbying]] and [[holyoke-2015-learnable-skills-unteachable]] (UNVERIFIED). It is qualified by [[furnas-2019-moneyed-interests-information]], [[huneeus-2018-effects-firms-lobbying]], [[furnas-nd-conflating-lobbying-pacs]] and [[jacoby-2025-mathematical-modeling-analysis]]. No work calibrates a constant.
- The competitor-money leg counts only PAC money, so it cannot see organizations that lobby without a PAC ([[furnas-nd-conflating-lobbying-pacs]]).
- [[furnas-2019-moneyed-interests-information]] supports the lt-priming cross-party gate. Two attributions in the system to this paper are not in it: "money is capped, info is the lever" and "district-relevant information". Its asymmetry also runs the other way from cpf_pressure's 1.7/0.5.
- [[jacoby-2025-mathematical-modeling-analysis]] suggests sweeping the WM/WP channel weights and the PERS table in the sensitivity harness. Its lobbying contrast must not be imported, because it is a simulation contrast built on revolving-door counts.

**Melange: allocation (break-even and ROI widget, joint optimizer)** ([[systems/melange-allocation]])
- [[alexander-2009-lobbying-analysis-under]], [[bickmore-2025-corporate-lobbying-political]] and [[bombardini-2020-empirical-models-lobbying]] qualify ROI wording. Firm-side tax or market-share rents are not flips per PAC dollar.
- [[pritoni-2025-lobbying-coalitions-determinants]] gives no evidence that coalitions raise influence, so it does not justify allied money as a weight.
- Paul's allocation ideas (think-tank funding treated like venture capital, targeting by state-level industry presence) have no direct calibration in the theme.

**Melange: contested bills, stance and sign** ([[systems/melange-stance]], [[systems/melange-labels-and-tiers]])
- [[kim-2025-measuring-interest-group]] suggests per-bill industry conflict as calibration gold for 2009-2022. Under the standing rule, inferred positions are retrieval and adjudication evidence, never automatic weights. The local Haiku rebuild scored Support and Oppose at precision 0.42 and recall 0.49 at natural base rates.
- [[pritoni-2025-lobbying-coalitions-determinants]] supports issue-specific counterparties over fixed global sides.
- [[alexander-2009-lobbying-analysis-under]]: the largest returns came on an uncontested provision.

**Melange: SNA, path-finding and the staffer network** ([[systems/melange-sna]], [[systems/melange-pathfinding]], [[systems/melange-staffer-network]])
- [[bacik-2025-measuring-dynamical-evolution]] qualifies the bipartisanship measure: cosponsorship polarized while lobbyists did not.
- [[kim-2020-mapping-political-communities]] does not support the dashboard's "exploitable community structure" claim for the cosponsor graph. Its communities are group-by-member lobbying communities, and budgetRecruit has no community term.
- [[holyoke-2015-learnable-skills-unteachable]] (UNVERIFIED) and [[furnas-2019-moneyed-interests-information]] qualify the staffer overlap and closeness routes. Their strength is a heuristic cost, never relationship strength, closeness, access or introduction probability (D-025).
- [[bombardini-2020-empirical-models-lobbying]] suggests route costs should consider whether the anchoring member is still in office. Revolving-door edges carry no influence language.

**Moneyball: rank pillars** ([[systems/moneyball-rank-pillars]])
- [[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]] and [[kim-2020-mapping-political-communities]] support committee centrality as an organizing axis. [[huneeus-2018-effects-firms-lobbying]] offers a data-driven committee-relevance weight, where the committee pillar's tier weights are currently owner judgment.
- [[boxsteffensmeier-2018-taking-congress-interest]] qualifies the legislative pillar: majority-party sponsorship should be adjusted for.

**Mentat: lobbying cards, nowcast and the sector growth study** ([[systems/mentat-sector-growth-study]], [[systems/mentat-giving-forecast]])
- In-house and outside-firm dollars are never summed. This is backed by [[harrison-2018-application-data-mining]], [[bacik-2025-measuring-dynamical-evolution]] and [[bombardini-2020-empirical-models-lobbying]].
- The persistence-based nowcast is supported by [[huneeus-2018-effects-firms-lobbying]] and [[bombardini-2020-empirical-models-lobbying]].
- The sector growth study's growth-to-spending regressions are qualified by [[huneeus-2018-effects-firms-lobbying]] (reverse causation), [[bickmore-2025-corporate-lobbying-political]] and [[alexander-2009-lobbying-analysis-under]].
- [[furnas-2019-partisan-ties-lobbying]] supports modelling House and Senate separately and expecting a weaker Senate signal. Its effect is distributional across firms, not a change in sector totals.
- The Kim-Stuckatz-Wolters identity work (gvkey matching, constrained graph clustering, hand-coded validation) is a method source for the donor identity ladder. Huneeus-Kim's 5% random validation sample is a template for auditing the LDA client classifier.
- Flag from Paul's own documents: his telecom report states about $139.4M in annual lobbying. That matches the double-counted telecom total removed in commit 914623152. Re-check the report figure before reuse.

**Terrier** ([[systems/terrier-row-scoring]], [[systems/terrier-imputation]], [[systems/terrier-canvass]])
- Only [[holyoke-2015-learnable-skills-unteachable]] (UNVERIFIED) and [[furnas-2019-moneyed-interests-information]] touch Sphere of Influence, as framing only. None backs a weight.

**Reciprocity** is not validated (D-104..D-108). [[kim-2023-systemic-sequential-links]]'s one-directional donation-then-lobbying result is not evidence for reciprocity edges.

Validation of any of the above should follow [[systems/melange-validation]].

## Gaps

**Gaps Paul named in his notes**
- No path-finding over bipartite or multi-layer lobbying graphs. [[kim-2020-mapping-political-communities]] stops at communities.
- No temporal tracking of coalition ideology against national mood.
- Campaign money is not separated from events and meetings, and representatives are not differentiated ([[huneeus-2018-effects-firms-lobbying]]).
- Position data is reactive, not predictive, and has no per-bill inter-industry conflict ([[kim-2025-measuring-interest-group]]).
- No news, press or sentiment drivers of lobbying change ([[bacik-2025-measuring-dynamical-evolution]] has no press data).
- In the contact lane: no gold labels beyond the disclosed floor (about 0.3% of activities name anyone), and no external accuracy benchmark.

**Missing from the literature itself**
1. **Member response.** No work estimates, with a credible causal design, how lobbying or money changes a legislator's vote, cosponsorship or flip. Every quantity Melange's engine needs is uncalibrated by this theme.
2. **Non-public organizations.** The causal and return studies cover public, Compustat-linked firms ([[kim-2020-strategic-sequential-links]], [[huneeus-2018-effects-firms-lobbying]], [[bickmore-2025-corporate-lobbying-political]], [[alexander-2009-lobbying-analysis-under]]). Trade associations and credit unions, which dominate the system's finance scope, are not studied directly.
3. **Financial services.** No work is specific to the sector, and none covers state-level lobbying (for example life insurance under McCarran-Ferguson, a question Paul raised in his sector growth study).
4. **LD-203 contributions** and lobbyist personal giving are absent. The LD-203 collector gap (G1) stays open.
5. **Revolving-door literature** appears only second-hand: Blanes i Vidal et al. and Bertrand et al. through [[bombardini-2020-empirical-models-lobbying]], and LaPira and Thomas through [[holyoke-2015-learnable-skills-unteachable]]. Any use must keep influence language off revolving-door edges.
6. **Coalition success.** Whether coalitions win is unresolved ([[pritoni-2025-lobbying-coalitions-determinants]]).
7. **Bill-resolution accuracy.** No measured error rate exists for LobbyView's resolution ([[kim-2018-lobbyview-firm-level]]) or for BICAM's linkage (SI not available), so the seam with Paul's 2021+ rebuild is unmeasured.

**Housekeeping and verification items**
- **Version conflict.** Reconcile [[kim-2020-strategic-sequential-links]] with [[kim-2023-systemic-sequential-links]] (opposite DiD time paths, and a fivefold drop in the FE coefficient) before quoting either. The slug year of [[huneeus-2018-effects-firms-lobbying]] does not match the cited 2019 version.
- **Mislabelled annotation.** In Paul's annotation file, the entry cited as Kalla and Broockman carries a summary of the Furnas staffer survey. The Kalla and Broockman field experiment, the main pro-access benchmark, is not a member of this theme.
- **Unanchored claim.** The note that donation effects fade after about 6 months is unchecked and matches neither Kim draft.
- **Incomplete annotations.** Six of the twelve entries in Paul's Lobbying bibliography are citation-only: [[bombardini-2020-empirical-models-lobbying]], [[furnas-2019-partisan-ties-lobbying]], [[furnas-nd-conflating-lobbying-pacs]], [[holyoke-2015-learnable-skills-unteachable]], [[kim-2020-mapping-political-communities]] and [[pritoni-2025-lobbying-coalitions-determinants]]. His [[kim-2025-measuring-interest-group]] entry is truncated mid-sentence. Those works need his own takeaways before they count as reviewed.
- **Unconfirmed source attributions.** Two system claims attributed to [[furnas-2019-moneyed-interests-information]] are not in the paper. The dashboard's community-structure citation of [[kim-2020-mapping-political-communities]] misstates what that paper models.
- **Report figure.** Re-check the $139.4M telecom lobbying figure in Paul's telecom report against the de-duplicated series.

## All works

- [[kim-2020-strategic-sequential-links]] (2020) — Strategic and Sequential Links between Campaign Donations and Lobbying
- [[bacik-2025-measuring-dynamical-evolution]] (2025) — Measuring the dynamical evolution of the United States lobbying network
- [[bombardini-2020-empirical-models-lobbying]] (2020) — Empirical Models of Lobbying
- [[harrison-2018-application-data-mining]] (2018) — An Application of Data Mining Techniques to Explore Congressional Lobbying Records for Patterns in Pediatric Special Interest Expenditures Prior to the Affordable Care Act
- [[furnas-2019-partisan-ties-lobbying]] (2019) — The partisan ties of lobbying firms
- [[holyoke-2015-learnable-skills-unteachable]] (2015) — Learnable Skills, or Unteachable Instinct? What Can and What Cannot Be Taught in the Lobbying Profession
- [[pritoni-2025-lobbying-coalitions-determinants]] (2025) — Lobbying coalitions and their determinants: a systematic literature review
- [[kim-2025-measuring-interest-group]] (2025) — Measuring Interest Group Positions on Legislation: An AI-Driven Analysis of Lobbying Reports
- [[furnas-nd-conflating-lobbying-pacs]] (n.d.) — Conflating Lobbying and PACs: The Surprisingly Low Overlap in Organizational Lobbying and Campaign Expenditures
- [[furnas-2019-moneyed-interests-information]] (2019) — Moneyed Interests, Information, and Action in Congress: A Survey Experiment
- [[kim-2020-mapping-political-communities]] (n.d.) — Mapping Political Communities: A Statistical Analysis of Lobbying Networks in Legislative Politics
- [[huneeus-2018-effects-firms-lobbying]] (2018) — The Effects of Firms' Lobbying on Resource Misallocation
- [[jacoby-2025-mathematical-modeling-analysis]] (2025) — Mathematical Modeling, Analysis and Simulation Utilizing Machine Learning Tools for Assessing the Impact of Climate Lobbying
- [[kim-2023-systemic-sequential-links]] (2023) — Systemic and Sequential Links between Campaign Donations and Lobbying
- [[bickmore-2025-corporate-lobbying-political]] (2025) — Corporate lobbying, political action committees, and market concentration
- [[alexander-2009-lobbying-analysis-under]] (n.d.) — Lobbying ROI: An Analysis under the American Jobs Creation Act of 2004 (also: Measuring Rates of Return for Lobbying Expenditures)
- [[boxsteffensmeier-2018-taking-congress-interest]] (2018) — Cue-Taking in Congress: Interest Group Signals from Dear Colleague Letters
- [[kim-2018-lobbyview-firm-level]] (2018) — LobbyView: Firm-level Lobbying & Congressional Bills Database
- [[delano-2025-bicam-congressional-lobbying]] (2025) — Bulk Ingestion of Congressional Actions and Materials Dataset (BICAM)
