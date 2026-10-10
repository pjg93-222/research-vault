---
type: paper
title: "How Dark Is It? An Investigation of Dark Money Operations in U.S. Nonprofit Political Advocacy Organizations"
authors: ["Renée A. Irvin"]
year: "2023"
venue: "Nonprofit Policy Forum 14(2): 101-129 (received July 15, 2022; accepted February 28, 2023)"
doi: "10.1515/npf-2022-0032"
category: "Campaign Politics"
systems: ["mentat-fec-attribution"]
components: ["mentat-fec-attribution.transfer-graph-and-direction", "mentat-fec-attribution.transfer-graph-and-direction", "mentat-fec-attribution.receipt-role-and-exclusion"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/irvin-2023-dark-investigation-dark.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\10.1515_npf-2022-0032.pdf"]
sha12: ["607758f7c9f3"]
tags: [paper, campaign-politics, type/paper, theme/campaign-politics, evidence/verified]
---

# How Dark Is It? An Investigation of Dark Money Operations in U.S. Nonprofit Political Advocacy Organizations

**Renée A. Irvin (2023).** *Nonprofit Policy Forum 14(2): 101-129 (received July 15, 2022; accepted February 28, 2023).* [10.1515/npf-2022-0032](https://doi.org/10.1515/npf-2022-0032) Theme: [[themes/campaign-politics|Campaign Politics]].

> [!abstract] Summary
> Irvin builds a nine-part opacity index from IRS Form 990 fields, plus one website word count, and applies it to 54 501(c)(4) economic-policy advocacy groups using 2016 filings (25 right-leaning, 2 centrist, 27 left-leaning). The components are board size, volunteers, website length, related 527s, related 501(c)(3)s, the share of spending that is political or lobbying, the salary share, the size of reserves and fundraising intensity. Each is scaled 0-1 and the nine are summed with equal weight, so the index runs 0-9. Scores ranged from 0.8 to 8.3. The most opaque groups had very small or token boards, almost no staff and very short websites. Two anecdotes, not a sample-wide pattern, show large grants to PACs: One Nation spent $63.3M in 2016 and granted $21.7M to the Senate Leadership Fund, and Freedom Frontier made a $4.4M grant to a PAC. Validation is weak. Thirteen of the 54 groups appear on OpenSecrets' 2016 dark-money list, and that label correlates with four of the nine components. But in a logistic regression with controls, the total index does not predict the label (Nagelkerke R2 0.529; only log expenses is significant). The paper also describes this political spending as strongly negative (overwhelmingly negative in Section 12), citing Beckel 2018 without a count.
>
> For Paul's systems this matters mainly in Mentat. The transfer graph traces only FEC committee-to-committee transfers (24G/24K). Money a 501(c)(4) gives a Super PAC therefore enters as a receipt with no upstream path. Separately, under Paul's ruling (its 2026-07-18 date is unverified), 501c recipients are excluded from reported totals. The paper discusses C4s generally and does not examine Mentat's excluded set. It does not show that the index can reliably tell policy groups from conduit-like vehicles. Nothing in the paper bears on Melange's member ranking, stance, SNA or Terrier.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Can observable characteristics in IRS Form 990 filings and one website measure be combined into a transparency (versus opaqueness) index that ranks social welfare 501(c)(4) economic policy advocacy organizations on how likely they are to be vehicles for dark money political activity? Secondary: under what conditions would a high net worth donor benefit from keeping advocacy operations low-profile (p.105-106)?

**Theory:** Combined theory. (1) Elite theory (Gen and Wright 2018): political power is held by a small upper-class segment with uniform preferences that intentionally perpetuates its privileges. (2) Weisbrod (1991) heterogeneous-preferences model: a median voter sets the level of tax-funded service provision; donors with preferences far from the median can fund advocacy to move the median voter. Combined, elites may move the median voter away from the median position and have reasons to stay invisible: insider tactics can yield only temporary wins if voters later punish lawmakers (p.104-105); unpopular causes need broad support, so a funder's identity is a liability (astroturf and authenticity, Walker 2014; "big bad opponent," Gen and Wright 2018; local-donor trust in high-stakes races; avoiding association with negative messaging) (p.105-106).

**Data:** Unit: organization (social welfare 501(c)(4) advocacy organizations active in U.S. or state-level economic policy). N = 54 organizations: 25 right-leaning, 2 centrist, 27 left-leaning (p.110). Sources: IRS Form 990 (Part I lines 4, 6, 9, 15, 16b, 18; Part IX line 11d; Part X line 27; Schedule C Part I-A line 1; Schedule R), website word counts (Weglot counter; seven sites hand-counted), Form 990 expense data for 2015 and 2016, and the OpenSecrets 2016 dark-money list (95 organizations nationally; 13 of the 54 sample organizations appear). Period: fiscal data for 2016 (presidential election year), with 2015 comparison for expense growth; economic policy verbiage checked for 2017; the Tax Cuts and Jobs Act (December 2017) context. Sample rules: founded 2015 or earlier; 2016 total revenue above $100,000; at least four economic policy items on website (testimony, podcasts, blogs, research reports, convenings, press releases) in 2017; sourced from think-tank lists, partisan networks, Post and Boris (2020), and donor-advised fund grantee lists. Author states the dataset covers the population of active organizations in this arena in 2016 (p.110-111). U.S. data only.

**Method:** Descriptive and correlational index construction. Nine components each scaled to 0-1 and summed (maximum 9.0, unweighted by design). Pearson bivariate correlation matrix among components (Table 2, two-tailed significance). Validation against an external watchdog label: second Pearson matrix of the OpenSecrets binary designation against the nine components, plus a logistic regression of OpenSecrets designation on the total index with controls for political lean, natural log of total expenses and founding year; Nagelkerke R-squared reported (p.124). Qualitative case discussion of high-index organizations (p.124-125).

**Identification:** None in the causal sense. Validity is assessed by convergent correlation of components with the OpenSecrets dark-money label and a logistic regression with controls. The author notes the sample was chosen by advocacy topic, not by transparency, so the logistic result (index not significant) is interpreted as a sample-design limitation, and validity is argued from component correlations instead (p.124).

**Scope:** Population: 501(c)(4) social welfare organizations active in U.S. (and state-level) economic policy advocacy, 2016 filings. Institution: U.S. tax-exempt advocacy sector and its links to 501(c)(3) think tanks, 527 organizations and Super PACs. Era: 2015-2017 data, published 2023. The author argues the index transfers to other issue areas (education, Social Security, energy, judicial reform, voting restrictions) but this is not tested (p.109, 125).

## Key findings

- Index values range from 0.8 (Fix the Debt Coalition Inc.) to 8.3 (Majority Forward), out of a possible 9.0. (min 0.8; max 8.3; scale max 9.0; Section 10, p.118)
- Index bands: up to 3.0 could be characterized as transparent; 3.0-5.0 some opaque characteristics (contentious band; includes Americans for Prosperity and Open Society Policy Center); 5.0-7.0 quite opaque; above 7.0 very opaque, including many large advocacy groups with very small boards, sparse websites and no staff. (cut points 3.0, 5.0, 7.0; Section 10, p.118-119)
- Anomaly cases: One Nation spent $63.3M in 2016 (up from $7.1M in 2015), had one board member, and gave $21.7M to the Senate Leadership Fund (PAC at the same address). ($63.3M (2016); $7.1M (2015); $21.7M grant; 1 board member; Introduction, p.101-102)
- Majority Forward spent $35.8M in 2016 with salaries at 1.34% of expenses, an 81-word website and two board members. ($35.8M; 1.34%; 81 words; 2 board members; Introduction, p.102)
- Freedom Frontier spent $5.9M in 2016 (up from $873,707 in 2015); $6.2M of revenue came entirely from grants and contributions with no fundraising expense; almost 75% of expenses went to political activity, including a $4.4M grant to a PAC. ($5.9M; $873,707; $6.2M; ~75%; $4.4M; Introduction, p.102)
- Organizations rely heavily on donated revenue, averaging 92% of total revenue; the data cannot show whether that comes from few large donors or many small donors. (mean 92% of revenue from grants and contributions; Section 9.9, p.116-117)
- Almost all index components are positively correlated with each other; the single negative correlation is not significant. Salaries as a percent of expenses is the component most strongly correlated with the others. (one negative, non-significant correlation; Section 9 (Table 2), p.117-118)
- Only two organizations had membership revenue (Maine People's Alliance; Utah Taxpayers Association), so membership was dropped from the index. (2 of 54 with membership revenue; Section 9, p.116-117)
- Three mid-sized organizations had boards of 26, 34 and 37 members, truncated to 25 for the board component. (26, 34, 37 -> truncated at 25; Section 9.1, p.112-113)
- Two organizations had fundraising expenses exceeding grants and contributions; that component was truncated at zero. (2 orgs; Section 9.9, p.116)
- Validation: 13 of the 54 sample organizations appear on the OpenSecrets 2016 dark-money list of 95 national groups. The OpenSecrets binary label is significantly correlated with four components: related to a 527, political spending as a percent of expenses, salaries as a percent of expenses, and unrestricted net assets as a percent of expenses. (13 of 54; OpenSecrets list 95 orgs; 4 significant component correlations; Section 11, p.123-124)
- Logistic regression of OpenSecrets designation on the total index (controls: lean, ln total expenses, founding year) is not significant for the index. Only total expenses is significant (larger organizations more likely listed). Political lean is not significant. Nagelkerke R-squared = 0.529. (Nagelkerke R2 = 0.529; only ln total expenses significant; Section 11, p.124)
- Political spending across this sample is described as strongly negative, asking voters to defeat candidates, per FEC filings reported by Beckel (2018). (qualitative; no count given in text; Section 4, p.106; Section 12, p.125)
- Expenses rose sharply between 2015 and 2016 (a presidential year). The author considered an expense-growth component and excluded it because political spending peaks in presidential years for any organization. (not quantified in text; Section 9, p.117)
- Context: in the year before the Tax Cuts and Jobs Act (December 2017), polls showed broad majority support for raising taxes on the wealthy (Gallup 2017; Pew 2017; Reuters/Ipsos 2017), and the author treats the 2016 set as a window onto a conservative tax-policy win. (polling majorities; 2017 tax law; Section 7, p.109)
- Author suggests high-index C4s may be illustrating elite issue entrepreneurs (Goss 2016): a single donor or few donors may control the C4 as a for-profit firm (hedged, Section 12 p.125). Author argues this challenges the C4 definition of promoting general welfare and that C4 use as a conduit around FEC individual limits resembles straw donation; recommends publicly reporting the largest donors behind C4 political funding. (qualitative policy recommendation; Section 12 and conclusion, p.124-126)

## Limitations

- No perfect external validation source exists because the process is anonymous; the OpenSecrets list is acknowledged as flawed.
- Every component measures legal behavior; concern arises only from combinations, and the author accepts this rests on judgment.
- Index construction involves truncation of outliers (board size at 25, website words at 100,000, UNA at 3x expenses, fundraising at zero) that are value judgments; OECD (2008) is cited on weighting and truncation.
- Equal weighting is a deliberate simplicity choice, but the author notes organizations would argue for different weights that favor them.
- Form 990 data become public only after two or more years, so organizations can operate unscrutinized for a period.
- Small N (54) limits statistical power; the logistic regression Nagelkerke R2 of 0.529 is described as reflecting the small dataset; index was not significant against OpenSecrets labels.
- Sample was selected on advocacy topic, not on transparency, so the index's validation depends on correlations rather than the OpenSecrets test.
- Grants between C4s are not classified as political or issue spending, so some political flows are excluded from C6.
- Website word counts are hand-counted for seven sites and use related-organization proxies for some C4s.
- Expense and program data are from one presidential year (2016), so cycle-level variation is not modeled.
- Fiscal-year and 2017 website verbiage are used for inclusion, so cross-year timing differences are not fully aligned.
- Index is applicable only to the U.S. context; generalization to other issue areas is argued but not tested.
- Several tables (2-4) are only partially recoverable from the extracted text layer; table-level values could not be fully verified from the text.

## Short quotes

- "little fear of negative consequences" (Abstract, p.101 (author quoting L.H. Mayer 2018, 194))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-fec-attribution#PAC transfer graph and beneficiary direction partition\|PAC transfer graph and beneficiary direction partition]] | qualifies | Two high-opacity C4s are described as moving money into political committees as grants. One Nation gave 21.7 million dollars to the Senate Leadership Fund, a PAC at the same address, out of 63.3 million spent in 2016. Freedom Frontier made a 4.4 million dollar grant to a PAC, with almost 75 percent of its expenses political. The paper presents these as anecdotes, not a sample-wide pattern. Grants between C4s are not classified as political, so such flows fall outside its C6 component. (Introduction p.101-102 (anecdote list); Section 9.6 (C6) p.115-116; limitations list) | Header line 3 states OTH 24G/24K outgoing records only. CASH_CODES (line 22) and the COMMITTEE regex (line 23) accept only a C plus eight digits, so a non-committee contributor such as a 501(c)(4) gets no node and its money appears only as a receipt into the Super PAC. allocate() conserves each root's funding forward over observed edges, so upstream attribution past a committee is not available. The earlier 89 percent oppose figure was not found by grep in fec_transfer_graph.py or report.R and is not confirmed here. `C:/Users/pgoff/Mentat/mentat/tools/fec_transfer_graph.py:3,22-23,484-571 (header, cash codes and committee regex, allocate)` | The structural point holds for the graph as read. The paper covers 2016 economic-policy C4s and gives no size estimate for C4 money reaching Super PACs, so no magnitude may be inferred. Mentat scope (2022-2026 industry PACs) was not checked here. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-fec-attribution#PAC transfer graph and beneficiary direction partition\|PAC transfer graph and beneficiary direction partition]] | supports | Beckel 2018, cited and not re-analysed, is reported as showing that political spending routed through dark money organizations is strongly negative, asking voters to defeat candidates, per FEC filings. Section 12 calls the dark-money media financing overwhelmingly negative. The paper gives no count or share, and the claim concerns dark money vehicles in general, not this C4 sample specifically. (Section 4 p.106; Section 12 p.124-125) | allocate() docstring keeps opposition money on the named target, never its rivals (line 489). The 89 percent beneficiary oppose share cited in the earlier analysis was not found in fec_transfer_graph.py or report.R, so it is unconfirmed and must not be cited as corroboration. `C:/Users/pgoff/Mentat/mentat/tools/fec_transfer_graph.py:489 (allocate docstring)` | Qualitative, second-hand, 2016 C4-linked spending. Mentat's beneficiary split is a modelled conditional scenario for a different period and denominator. At most directional agreement; no numeric confirmation. | weak | VERIFIED-SOFTENED |
| [[systems/mentat-fec-attribution#Receipt-role classification and contradiction-only exclusion\|Receipt-role classification and contradiction-only exclusion]] | suggests-extension | Opacity scores run from 0.8 (Fix the Debt Coalition) to 8.3 (Majority Forward) on a 0-9 scale. Scores above 7 are described as very opaque, including many large advocacy groups with very small boards, paltry websites and no staff. Scores up to 3 could be characterized as transparent. Donations average 92 percent of revenue. The total index does not predict the OpenSecrets dark-money label once controls are added (N=54, Nagelkerke R2 0.529, only ln total expenses significant). (Section 10 p.118-119; Section 9.9 p.116-117; Section 11 p.123-124) | The earlier analysis describes Paul's 2026-07-18 ruling excluding 527 and 501c recipients; that ruling was not found in the memory index or in the analyst files checked, so its date is unverified here. The code keeps 501c-marked recipients as classified rows, tags them EXCLUDED_527_501C, and drops them from reported totals as one block. No transparency distinction is made among 501c recipients. `C:/Users/pgoff/Mentat/mentat/R/ingest.R:722-744; C:/Users/pgoff/Mentat/mentat/R/report.R:33,3042,3176-3177,3213` | Any change to the exclusion is Paul's decision; this bearing recommends none. The index is unvalidated in this sample, so at most a display-only heuristic label carrying its 2016 and N=54 window. Form 990 data is published two or more years late and Mentat does not collect it. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: summary_md says the most opaque groups had one or two board members. The paper says token boards of three or even fewer (Section 12 p.124-125) and very small boards in the above-7 band, with no one-or-two figure.
> - Summary issue: summary_md says the most opaque groups had large grants to affiliated PACs as a general pattern. The paper gives two examples (One Nation, Freedom Frontier) and no sample-wide count.
> - Summary issue: summary_md says the paper describes political spending as mostly negative. The paper says strongly negative (Section 4) and overwhelmingly negative (Section 12), citing Beckel 2018 without a count.
> - Summary issue: summary_md says the paper shows the excluded 501c group mixes transparent shops with near-shell vehicles. The paper does not examine Mentat's excluded set and discusses C4s generally.
> - Summary issue: summary_md attributes a 2026-07-18 ruling to Paul. That ruling was not found in the memory index or the analyst files checked; the date remains unverified.
> - Summary issue: summary_md and bearings 1 and 2 rely on an 89 percent oppose figure for Mentat that the code files checked do not contain. Drop it or label it unverified.
> - Summary issue: Ext notes say the author concludes high-index C4s are elite issue entrepreneurs in a strict sense. The paper hedges this with may be illustrating (Section 12, p.125); the stronger wording should not be carried forward.

## Gaps and flags

- Mentat has no nodes for non-committee entities. A 501(c)(4) or IRS-only 527 that funds a Super PAC appears as an untraceable receipt, so upstream industry money routed through C4s is invisible to allocate() (fec_transfer_graph.py:1-6, 484-571).
- Mentat does not use IRS Form 990, Schedule R or Schedule C. Related-organization links (C4 to 501(c)(3) to 527 to PAC at a shared address, as with One Nation and the Senate Leadership Fund) cannot be represented.
- The block exclusion of 501c recipients (report.R:3175-3177) cannot tell a policy C4 from a conduit-like C4. There is no recipient-level transparency attribute, even as display-only.
- Mentat totals are not checked against an external dark-money list such as OpenSecrets' C4 list, so there is no estimate of how much industry money moves through excluded 501c recipients.
- The index is unvalidated. Against OpenSecrets labels, the total index is not significant (N=54, 2016 filings, Nagelkerke R2 0.529, only ln expenses significant). Any use in Mentat must carry an UNVALIDATED heuristic label, with its window (2016 filings) and denominator (54 economic-policy C4s). It must never be presented as a probability that an organization is dark money.
- The weights are equal and the cut-offs (25 board seats, 100,000 words, three years of expenses) are the author's judgment, much like the uncalibrated constants elsewhere in the system. Treat them as owner-judgment-grade, not estimates.
- Mentat's 501c exclusion is Paul's ruling (date unverified). The paper is grounds for raising a question with Paul, not for an agent to change the exclusion. Money collection is frozen per Paul's standing directive.
- The paper's elite-theory framing (elites moving the median voter) is a theory, not an identified effect. Do not carry its influence language onto Mentat flow edges.
- Tables 2-4 were garbled in the extracted text, so correlation magnitudes are not available.

Paul has no annotations of this paper; it appears only as an on-disk citation for transfer-graph-and-direction. His recorded view links dark-money donation behavior to Furnas's ideological infrastructure. The paper fits that view: the author suggests high-opacity C4s may be vehicles for a few donors, and two anecdotes show grants into PACs. His decision to exclude 501c recipients (date unverified) is not stale, since it was a deliberate scope choice. But it predates any reading of this paper. The paper does not examine Mentat's excluded set and gives no validated way to identify conduits. So there is a question to put to Paul (whether to show a display-only split), not a change to make.

## Related papers

- [[christopher-2016-mathematical-modeling-analysis]] — complements — Item title confirms a model of a dark money network (Couch, Fox, Everton). Real link; the clause that both papers draw on OpenSecrets/Choma evidence was not checked.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Furnas item confirms a finding of low overlap between organizational lobbying and PAC spending. Real link; Irvin adds C4 grants as a third channel.
- [[blackwell-2026-effect-political-advertising]] — complements — Blackwell item is about estimated effects of political advertising after Citizens United. Real link; the framing fits, though Blackwell does not measure C4 opacity.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — Chohan item covers the crypto lobby and the Fairshake super PAC. Real link; Irvin supplies an organization-level opacity measure that Chohan does not.
- [[bonica-2025-money-campaign-finance]] — complements — Item is Bonica and Grumbach on age and campaign finance, and it explicitly excludes dark money. Real link, but the concentration rationale is narrower than stated; keep only as a loose complement.

### Linked from
- [[anon-2025-cross-industry-political]] — complements — Irvin (2023) studies dark-money vehicles among 501(c)(4) advocacy groups, using dark money for nonprofit non-disclosure. The memo instead uses the term for disclosed Super PAC donations.