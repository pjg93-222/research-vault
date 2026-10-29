---
type: system
name: Melange lobbying data and LDA linkage
slug: melange-lobbying
components: [melange-lobbying.finance-client-classification, melange-lobbying.lobbied-member-link, melange-lobbying.fec-lda-bridge, melange-lobbying.contact-inference, melange-lobbying.cosponsor-forecaster, melange-lobbying.contested-bills]
brain_notes:
  - C:/Users/pgoff/Documents/brain/actors/lobbyview-collector.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-data-collection-lanes-lobbyview-lda-rebuild-and-bridge.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-data-collection-lanes-contact-inference-design-and-evaluation.md
paper_count: 26
tags: [system, type/system-hub]
---

# Melange lobbying data and LDA linkage

LobbyView/LDA ingestion and its analytic uses: finance-client classification, the finance report join, the member-year lobbying network, the circular lobbied-member link, the FEC-to-LDA bridge test, contact inference, the ridge-logistic cosponsor forecaster, the contested-bills registry and the industry-axes tensor.

Brain notes: [actors/lobbyview-collector](obsidian://open?vault=brain&file=actors/lobbyview-collector); [topics/2026-07-feature-data-collection-lanes-lobbyview-lda-rebuild-and-bridge](obsidian://open?vault=brain&file=topics/2026-07-feature-data-collection-lanes-lobbyview-lda-rebuild-and-bridge); [topics/2026-07-feature-data-collection-lanes-contact-inference-design-and-evaluation](obsidian://open?vault=brain&file=topics/2026-07-feature-data-collection-lanes-contact-inference-design-and-evaluation)

### Melange lobbying data and LDA linkage: evidence assessment

**Scope.** This covers 6 components and 47 evidence rows (paper bearings) from 26 distinct papers. 36 rows are VERIFIED-SOFTENED: the paper's claim was confirmed but needed narrowing. 10 are VERIFIED. One is UNVERIFIED: Selfhout 2010, a study of adolescent friendship networks. Several rows that are VERIFIED-SOFTENED on the paper side also mark the transfer to this system as UNVERIFIED (DE-062). Those transfers are treated below as leads, not findings.

**Overall verdict.** The evidence is strongest where the system has already reached a negative conclusion. It is weakest where the system makes a positive bet.

- **Settled by convergent evidence.** LD-2 filings never name the members lobbied. So the bill-roster member link is a join, not contact (de Figueiredo 2013; Bacik 2025; Kim & Kunisky 2020; Kim 2018; Fremeth 2018). The system's REFUTED verdict on that link as influence evidence is well supported. It transfers fully, because every source describes the same federal form.
- **Narrowed rather than settled.** The FEC-to-LDA bridge null (round 3, 2023-2026, pooled N = 213 member x industry x cycle pairs; four-control partial -0.0002) neither refutes nor confirms the literature.
  - The strongest donation-then-lobbying evidence (Kim, Stuckatz & Wolters 2020/2023) is a within-pair onset design.
  - Its sample is public manufacturing firms, 2008-2018. Its strongest arm is committee referral, which the system's join lacks.
  - Furnas et al.: only 10-15% of politically active organizations per year both lobby and give through a PAC (1998-2018, CRP definitions). FEC therefore cannot be a general proxy for lobbying.
- **Gaps or near-gaps, with no paper evidence on the system's own empirical question:**
  - Contact inference: there is no precision study against blind labels.
  - Cosponsor forecaster: nothing compares it with a Brandenberger/Russo reciprocity baseline, and the frozen forecast is unscored.
  - Contested bills: the closest evidence (Kang's all-pay contest models; Baumgartner et al.) points to *small* effects of expenditure on outcomes, and to roughly equal mobilization on both sides.

**Plain contradictions to resolve:**
1. The "FEC leads LDA by 42 months" heuristic is a data-freshness gap: FEC data runs about 42 months past LobbyView coverage. It is not a measured behavioural lag. Calling it a "leading indicator" is wrong.
2. The component description says NAICS is "linkage metadata only". In the code, NAICS assigns a low-confidence side as a fallback when no name rule fires.
3. The collector's per-side dollar totals add self-filer expense and hired-firm income together. Whether the "ABA $228M vs CUNA $93M" contrast mixes the two series is UNVERIFIED.
4. `build_dyad_signs` carries Kim 2025 bill-level positions onto members through the sponsor/cosponsor roster, and the result feeds `contest_index`. That brings the refuted circular mapping back into a scored term.
5. Several system-side facts in the hub could not be found in the repo:
   - the 265,396-prediction frozen forecast and its freeze clause;
   - the brain-memo claim about the "argmax cautionary chain";
   - the round-3 raw rho of +0.153, which was located in only some checks.

## LDA finance-client classification and report join

**Computes.** Bank, credit union or other classification with HIGH/LOW confidence. Joined finance reports: 322,473 issue-coded reports; ABA $228M vs CUNA $93M.

**Method.** Name regex with guards. NAICS is linkage metadata only. Fail-closed guards. Vintage 2024-10-25; coverage reliable through 2021.

**Code refs.** `fec/lobbying/collect_lobbyview.py`, `fec/lobbying/collect_lda_filings.py`

**Assumptions**
- Client names are decisive for finance classification

**Evidence table** (9 evidence rows from 8 distinct papers; VERIFIED: 2, VERIFIED-SOFTENED: 7)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | supports | For the SOPA case study, only about 40% of the clients lobbying on SOPA in Q4 2011 could be mapped to NAICS sectors through Compustat, and the probabilistic analysis is restricted to them. Client disambiguation is search-assisted and the paper says it is weak for long-bankrupt firms and non-public-facing firms that resolve to third-party directories. The analysis window runs 1999-2023. | The 40% figure is for one bill-quarter sample (SOPA, Q4 2011), not all LobbyView clients; no all-client or finance-client coverage share is reported. It supports not relying on NAICS for credit-union versus bank classification only as an illustration. Whether the post-2021 issue-code collapse is a bulk-export vintage artifact remains UNVERIFIED and should be checked against the OSF release. | moderate | VERIFIED-SOFTENED |
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | supports | Bombardini and Trebbi (2012) report an average share of lobbying by individual firms of 67% (median 96%); industries with lower concentration and lower product differentiation spend a higher share of lobbying through trade associations. Huneeus and Kim (2019) put business-association lobbying at less than a quarter of all lobbying expenses. | The prediction is cross-industry, from Bombardini and Trebbi's 2012 trade-issue sample. The review gives no CU-specific figure. | moderate | VERIFIED |
| [[harrison-2018-application-data-mining]] | 2018 | qualifies | The LDA XML amount fields do not separate lobbying expenditures from income, and no identifier lets a researcher split them. Expenses under $5,000 are a checkbox with no amount, and client income over $5,000 is rounded to the nearest $10,000. | The hazard carries over: the LD-2 amount semantics the paper describes still apply. The paper's claim that the two series cannot be separated does not carry over to LobbyView, which exposes is_self_filer; the collector could split them and does not in these aggregates. The LobbyView window and the 2024-10-25 vintage were not re-checked here. Whether the $228M ABA figure is the mixed total_amount, and how large the overlap is, stay UNVERIFIED until a second path reproduces them (DE-062). | moderate | VERIFIED-SOFTENED |
| [[harrison-2018-application-data-mining]] | 2018 | supports | Pediatric items sit in single filings alongside unrelated issues (Medicare Part B vaccine coverage, Medicaid pediatric vaccines, FDA biologics, pandemic preparedness), so dollars spent on pediatric lobbying cannot be isolated. The paper nonetheless reports dollar sums by umbrella code from a Filing x Issue merge: Taxation/Internal Revenue Code $527,147,333 and Health Issues $436,651,121 for Q4 2009, with no filing-count denominator. | Applies to any LDA source where one report carries several issue codes. That the paper's per-code totals count a multi-code filing under each code is an inference from the merge description, not stated by the authors; treat the totals as non-benchmarks either way. | moderate | VERIFIED |
| [[kim-2025-measuring-interest-group]] | 2025 | supports | Firm size is associated with position type. Comparing firms at the 90th and 10th employment percentiles, larger firms are 19.5 p.p. more likely to lobby and 4.5 p.p. more likely to adopt Monitor. Smaller firms are more likely to Oppose (1.8 p.p.) or Amend (1.3 p.p.). These are associations, not causal effects. Sample: Compustat firm-years 2009-2022, 5,421-7,014 firms per year. | Kim's sample is Compustat firms and says nothing about trade associations. The claim that the system's 22 pinned fintech/crypto firms hold 56 positions with one directional label was not found and is UNVERIFIED. Treat any reading that associations are the directional filers as a descriptive lead, not a replication. | weak | VERIFIED-SOFTENED |
| [[huneeus-2018-effects-firms-lobbying]] | 2018 | method-source | LDA client names were matched to Compustat in four steps: FuzzyWuzzy against 61,478 unique client names; Bureau van Dijk batch search for ISIN and ticker; CRP standardized client names; and a random 5 percent sample of client names checked for publicly traded firms that were missed. The 5 percent sample is a miss (recall) check, not a precision audit. The paper names messy spellings and abbreviations as the main hazard. The matched universe is 67,842 unique lobbying client names (Introduction). | The transferable piece is the 5 percent sample as a recall audit for missed names. The fuzzy matcher is not transferable, and the project's strict-names rule rejects fuzzy acceptance. H&K's firm match leaves out most CUs (not in Compustat, per the analyst; not checked against the paper) and trade-association lobbying such as CUNA and NAFCU, so their match rates and Fact 5 do not carry to CU lobbying. | moderate | VERIFIED-SOFTENED |
| [[kim-2018-lobbyview-firm-level]] | 2018 | qualifies | Firms (clients) are grouped into industries by NAICS2 (or SIC) to compute per-industry Herfindahl indices. The example run is NAICS for 1996-2011. Output is then merged with Compustat firm data and a trade-issue indicator (Sections 5.5, 5.5.1, 5.5.2). | The procedure claim holds. The paper does not say NAICS is usable for research aggregates; it only uses NAICS2 as the grouping key. The reason given for the system's rule (codebook gives NAICS no provenance) is not in the code checked, so it is UNVERIFIED. The system ruling that NAICS is linkage metadata was not checked here. Herfindahl outputs are industry descriptors and must not be edge or weight sources (D-021, not re-checked). | moderate | VERIFIED-SOFTENED |
| [[delano-2025-bicam-congressional-lobbying]] | 2025 | context | BICAM supplies Bill-LDA linkages, not the LDA filings, and points to LobbyView and OpenSecrets as the compilations of LDA data. Members are keyed by bioguide_id and icpsr. Discrepancies between the two APIs, such as GovInfo missing bioguide_id for nickname members, are fixed by a rules-based merge. | Context only. BICAM does not replace the client or filer source. Any post-2021 bill linkage from it would serve identity resolution, not classification input. | weak | VERIFIED-SOFTENED |
| [[note-sector-growth-and-political-spending-source]] | 2026 | context | Melange holds bank and CU lobbying aggregates for 1999-2023 and cached annual series for 2021-2025 plus partial 2026 (bank, CU, crypto, fintech). Life insurance and auto are not cached. Organizations still need alignment to the research sectors under one expenditure definition. | DEFAULT_YEARS covers the LDA sector-spend build, not the 1999-2023 bank and CU aggregates the note cites. Using LDA spend as an outcome per organization is an identity-source use consistent with D-021; no edge or weight use is implied. | weak | VERIFIED-SOFTENED |

**Assessment**

**What the system does.** It classifies LDA clients as bank, credit union (CU) or other from their names, with HIGH/LOW confidence. The data is the LobbyView export of 2024-10-25. The joined finance reports total 322,473 issue-coded reports (a report count).

**Evidence weight: moderate, mostly methodological. Nothing tests the classifier directly.**
- **Huneeus & Kim 2018 (VERIFIED-SOFTENED, method-source).** Their pipeline matched 67,842 lobbying client names to Compustat, then hand-checked a random 5% sample as a *recall* audit for missed names. That audit is the transferable piece. Their fuzzy matcher is not, because the project's strict-names rule bars fuzzy acceptance. Their universe is Compustat firms, which leaves out most CUs and trade associations such as CUNA and NAFCU. Their match rates therefore do not carry to CU lobbying.
- **Bacik 2025 (VERIFIED-SOFTENED).** In the SOPA case (Q4 2011), only about 40% of lobbying clients could be mapped to NAICS through Compustat. This is one bill-quarter, not an all-client rate. It supports not relying on NAICS to separate CUs from banks, but only as an illustration.
- **Kim 2018 LobbyView.** NAICS2 is used purely as a grouping key and is never validated. This neither licenses nor forbids NAICS aggregates.
- **Bombardini & Trebbi, via the 2020 review (VERIFIED).** Firms do on average 67% of their own lobbying (median 96%; trade-issue sample, 2012). Industries with lower concentration route more lobbying through trade associations. This is consistent with CU lobbying sitting with CUNA and NAFCU, but the review gives no CU-specific figure.
- **Harrison 2018 (VERIFIED and VERIFIED-SOFTENED).** It names two hazards, and both transfer.
  - (a) LD-2 amount fields mix self-filer *expense* with hired-firm *income*. Expenses under $5,000 are only a checkbox, and income is rounded to the nearest $10,000.
  - (b) A multi-code report cannot apportion dollars to individual issue codes. The collector counts reports per code rather than spreading dollars across codes, which handles (b) correctly.
  - For (a), LobbyView exposes `is_self_filer`, yet the collector's `amount_sum`, `finance_amount_sum` and `total_amount` add both series. Whether the ABA $228M vs CUNA $93M contrast is a mixed total is UNVERIFIED. The commit that shows in-house and outside-firm lobbying side by side may cover display surfaces only.

**Coverage window.** Issue-code coverage collapses after filing year 2021 in this vintage. Whether that is an artifact of the bulk-export vintage or a real gap is UNVERIFIED. It should be checked against the OSF release.

**Empirical question: name-based classification vs Kim et al.'s positions-based industry assignment.** No paper bears on it directly. Kim 2025 is Compustat-only, 2009-2022.

**Contradictions**
- Component text says NAICS is 'linkage metadata only'. The code's classify_client falls back to a NAICS-based (low-confidence) side when no name rule fires (Huneeus 2018 and Kim 2018 rows). NAICS therefore does assign side for some clients.
- The collector's dollar aggregates add self-filer expense and hired-firm income, a mix that Harrison 2018 warns against and that LDA guidance says can double-count. The '$228M vs $93M' contrast may rest on the mixed total_amount (UNVERIFIED).
- The hub says the 22 pinned fintech/crypto firms hold 1 directional position out of 56. The Kim 2025 row could not find that figure (UNVERIFIED).

**Gaps**
- No accuracy audit of the name classifier exists. Needed: a stratified random sample of client names (by HIGH/LOW confidence and by rule fired), hand-labelled blind to the classifier output, reporting precision and recall per class with the window and denominator named. A Huneeus-style 5% recall sample of the 'other' class would catch missed CUs and banks.
- Name rules vs Kim et al. positions-based industry assignment: needs a crosswalk on the overlapping clients up to the 117th Congress, reporting the agreement rate and the disagreement cases.
- Post-2021 coverage: needs a vintage comparison (2024-10-25 bulk export vs the current OSF release vs the LDA API rebuild) on 2021-2023 report counts by issue code.

**Paul's position vs the evidence**

Paul treats LobbyView as the fundamental source and Harrison as an introductory pipeline guide.

- **LobbyView as the fundamental source.** The evidence supports this only partly. LobbyView is an identity and measurement source (D-021), but its own methods document is incomplete (Kim 2018 Introduction). Its industry key is unvalidated NAICS2.
- **Harrison as an introductory guide.** Harrison is more than that: it is the clearest warning about LD-2 amount semantics. That warning bears directly on the ABA vs CUNA dollar contrast Paul's materials quote.
- **The 2021 coverage limit.** Paul did not foresee it, and it is real for this vintage. Whether it is intrinsic to LobbyView is still UNVERIFIED.

**Open questions**
- Is the ABA $228M / CUNA $93M figure the mixed total_amount, or the self-filer sub-sum? What window does it cover?
- Should NAICS-fallback rows be excluded from research aggregates, or shown under a LOW-confidence label?
- Is the post-2021 issue-code collapse an artifact of the 2024-10-25 vintage?

**Read first**: [[harrison-2018-application-data-mining]], [[huneeus-2018-effects-firms-lobbying]], [[bacik-2025-measuring-dynamical-evolution]], [[kim-2018-lobbyview-firm-level]], [[bombardini-2020-empirical-models-lobbying]]

**System record (brain fleeting pass, added 2026-10-11).** Internal record, not literature; not counted in the 9 rows above. Figures and method only. Brain note: [topics/2026-07-feature-primacy-and-industry-tier-boards-telecom-industry](obsidian://open?vault=brain&file=topics%2F2026-07-feature-primacy-and-industry-tier-boards-telecom-industry).

| Source | Date | Relation | Record shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| memory `telecom-industry-lane-2026-10-07.md` | 2026-10-07 | internal (system record) | Telecom vs credit-union comparison. Federal LDA, OpenSecrets-style totals: telecom $102.7M vs CU $7.4M in 2025 (14x). 2025 lobbyists 526 vs 81; covered positions 196 vs 25. PAC giving to House and Senate candidates, 2024: about 3:1. Method: the full LDA sweep (539,782 filings, 2021 to 2026 Q2), read by the scratchpad script `lda/lda_sector_compare.py` (not in the repo as of 2026-10-28; memory-only path); LobbyView cross-check within 1% (telecom) and 4% (CU); Mentat Telecom and Credit Union workbooks. Index file `audits/staffer_live_runtime/lda_activity_v2.sqlite` holds IDs only: no names, no amounts. | Descriptive sector totals; LDA here identifies and totals filings and is not an edge or weight source (D-021). The memory does not say how "OpenSecrets-style" treats self-filer expense against hired-firm income, which is the Harrison hazard above. The Mentat giving-forecast hub records a different telecom 2025 figure from a Melange page ($102.4M in-house + $37.0M outside-firm fees), as does the brain note [actors/mentat-console](obsidian://open?vault=brain&file=actors%2Fmentat-console); the two are not reconciled here. The sweep runs past the 2021 LobbyView coverage limit, and the LobbyView agreement figures do not say which years they compare. | internal (lane memory) | UNVERIFIED |

## Lobbying member-year network and circular lobbied-member link

**Computes.** client x bioguide x year dyads (63,991 member-years). Edges are the sponsor and cosponsors of lobbied bills.

**Method.** P(joined | lobbied) = 1 by construction, so it is REFUTED as evidence of influence. LD-2 filings never name members.

**Code refs.** `fec/lobbying/build_network_2021_rebuild.py`, `fec/lobbying/lobby_interactions.py:4`

**Assumptions**
- The bill-roster join is not a lobbying-contact measure

**Evidence table** (13 evidence rows from 12 distinct papers; VERIFIED: 3, VERIFIED-SOFTENED: 10)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | context | Each edge comes from an official register entry that names both the EC official and the interest representative in a dated meeting. Duplicate meetings were fuzzy-merged at partial ratio >= 70 (460 pairs). | The paper shows what a valid lobbying-contact layer requires: a register that names both parties to the contact. No US congressional equivalent exists, so the design does not carry over to LDA. It is a benchmark for any future contact source, not a method to apply. | weak | VERIFIED-SOFTENED |
| [[kim-2020-strategic-sequential-links]] | 2020 | qualifies | Outcome Y=1 if firm i lobbies any bill in year t related to politician j as sponsor, co-sponsor, or member of the committee the bill is assigned to. For the descriptive Lobby $ column, a firm's annual lobbying spend is divided by the number of unique bills lobbied and the per-bill amount is allocated across all connected politicians (sponsor, co-sponsors, committee members); the authors call this a highly noisy measure and use the binary link in the analysis. Donation onset is the separately measured treatment. | Kim uses the same roster join only as an outcome against an independently timed treatment (donation onset), the non-circular use. That supports keeping the join as 'whose bills did this org lobby on', never as contact. Kim adds a committee-referral channel the system's join lacks. The paper gives no warrant to read the join as contact. Adopting Kim's dollar allocation would make LDA a weight source (barred by D-021). | moderate | VERIFIED-SOFTENED |
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | supports | The LDA does not require registrants to disclose direct contacts with legislators, so the paper infers legislator connections from lobbyists' disclosed past employment; a legislator is included if at least one active lobbyist worked for them. About 90% of lobbyists have no documented government or legislator association, and 20% had a known government connection in 2023. | The evidence carries over fully because it describes the same federal LD-2 form. The paper uses the same data and states plainly that legislator contact is unobserved, so nothing in LD-2 can rescue the bill-roster join. An independent contact measure is still required. The paper's own legislator edges come from revolving-door disclosure, so they are not contact evidence either, and influence language is barred on them. | strong | VERIFIED-SOFTENED |
| [[kim-2025-measuring-interest-group]] | 2025 | qualifies | Positions are measured as interest group x bill. Legislators enter only as context nodes in the GNN graph, through sponsor/cosponsor relations and legislator-lobbyist prior-work relations. The selected configuration is 'Base Graph + legislator-lobbyist'. The paper does not measure a position or contact between a group and a member. | Kim's measure stops at the bill. Pushing it onto members through the bill roster brings back the circular sponsor/cosponsor mapping the system already refuted as influence evidence. The dyad counts describe which bills members sponsored, not whom an org targeted. Kim's legislator-lobbyist relation is a revolving-door edge, so influence language is barred on it. | strong | VERIFIED |
| [[kim-2020-mapping-political-communities]] | 2020 | supports | Lobbying reports need not identify the politicians contacted, so the network is built indirectly: A_ij = number of bills group i lobbied that politician j sponsored (113th Congress, k = 8). The authors call this 'a type of political connection' and read recurring co-occurrence as 'shared involvement on specific political issues', while stating that lobbying a bill 'does not necessarily imply political ties to its sponsor'. They make no claim that the link is contact or that lobbying causes sponsorship. Window: 113th Congress; denominator: lobbied bills with an identified sponsor. | Same bill-roster construction, but the system's is broader (adds cosponsors) while the paper uses sponsors only, so the dyads are not identical. The paper avoids circularity only because it makes no lobbying-predicts-sponsorship claim; any Melange reuse must stay descriptive. Under D-021 the matrix is an identity source, not an edge or weight source. | strong | VERIFIED-SOFTENED |
| [[kim-2023-systemic-sequential-links]] | 2023 | qualifies | Outcome is a firm lobbying a bill tied to politician j through sponsorship, co-sponsorship or committee referral, with bills matched by number and title because LDA reports have no member names or unique bill IDs. Bill-linked reports are 0.25M of 1.27M (about 20%) and cover about 97% of lobbying spending. The authors call the estimates a conservative lower bound. | The paper uses the same roster join, but its treatment is donation timing, not the join itself, and it treats the outcome as bill involvement, not contact. Neither the paper nor the code supports reading the join as influence or contact (consistent with D-021). The paper's own committee-referral arm is absent from the system's join. | moderate | VERIFIED-SOFTENED |
| [[boxsteffensmeier-2018-taking-congress-interest]] | 2018 | supports | Footnote 11 (p. 170): a member reviewing a Dear Colleague letter cannot distinguish an endorsement the member solicited from one the interest group solicited from a sponsor. P. 171: endorsers listed together on a letter may not have any interaction, and endorsements were likely pulled together by a member's office. | The paper's endorsement channel is observed by members, which is a stronger channel than an LDA bill mention. Its authors leave solicitation direction unresolved, which supports caution about reading bill co-occurrence as influence. The paper does not discuss LDA filings. | moderate | VERIFIED-SOFTENED |
| [[kim-2018-lobbyview-firm-level]] | 2018 | supports | The abstract says LobbyView bridges politicians' sponsorship of bills with reported lobbying by interest groups. Section 2 steps 1-5 describe only matching report text to bill identities (number, Congress, title, range). No step describes a legislator link or any contact. The word sponsor appears once in the paper, in the Abstract. | Holds for the system join. The paper's sponsorship wording is only the abstract phrase, and the system's join is wider: sponsor plus all cosponsors. collect_lobbyview.py line 446 records a 2026-08-01 check that BECU 2021 has 181 edges equal to 1 sponsor plus 180 cosponsors of H.R. 1996. That is a code comment; it was not re-run here. | strong | VERIFIED-SOFTENED |
| [[kim-2018-lobbyview-firm-level]] | 2018 | suggests-extension | Filing-year Congress assignment fails. Google's 2013 report cites H.R. 2577 (Safe Data Act), a 112th Congress bill, not the 113th. LobbyView considers the three preceding Congresses by default, picks the candidate with highest cosine similarity to surrounding text (Eq. 1), falls back to the filing-year Congress if no number matches, and propagates a Congress across the report by majority vote. | Paper side reproduced. Under the rebuild rule, the paper's own example would bind to the 113th Congress. Risk concentrates in filings made early in a new Congress (odd-year first sessions). The paper gives no rate, so the size of the error in the rebuild is UNVERIFIED. | moderate | VERIFIED |
| [[goff-nd-cross-industry-finance]] |  | qualifies | 2026 LDA reports show members Zach Nunn, Andy Kim, Young Kim, Bill Huizenga and others with an average increase of 20 interactions within the year, read as the community-bank industry pushing harder. | Applies only if the report's interaction counts come from the bill-roster join, which the report does not say. If they come from described contacts, only the window and denominator flags remain. | moderate | VERIFIED-SOFTENED |
| [[delano-2025-bicam-congressional-lobbying]] | 2025 | qualifies | Bill numbers and titles extracted from LD-2 Section 16 are matched by distance measures to the Bills component (421,764 bills, 6th-119th Congress); low-confidence matches get extra scrutiny. The linkage is said to detect significantly more bill lobbying than Kim and Kunisky 2021 and to capture ex-post lobbying. The main text gives no match count, precision, recall or threshold. | BICAM fits only as a candidate second path for bill-mention resolution. A bill-to-sponsor join from it is a roster edge with P(credited given lobbied) = 1 by construction and adds no contact signal. Precision and recall sit in the Supplementary Information, which this check did not have. | moderate | VERIFIED-SOFTENED |
| [[defigueiredo-2013-advancing-empirical-research]] | 2013 | supports | Transaction (LDA) reports show lobbying intensity but not message content, and the US federal transaction data does not say which legislator is the target, so researchers cannot map lobbying efforts to individual legislators. | Holds for any federal LD-2 vintage because the form structure, not the sample, creates the gap. A member-level contact measure would need an independent source. | strong | VERIFIED |
| [[fremeth-2018-spillovers-regulating-corporate]] | 2018 | supports | LDA disclosures do not identify the candidates or members lobbied. The authors relate only firm-level lobbying totals to PAC behaviour and approximate the mapping with a stepwise function. | Confirms only that LDA rows do not name members, a measurement fact. The paper offers no contact measure to reuse. | weak | VERIFIED-SOFTENED |

**Assessment**

**What the system does.** It builds client x bioguide x year dyads (63,991 member-years) by crediting every sponsor and cosponsor of each lobbied bill. The system has ruled the link REFUTED as influence evidence, because P(joined | lobbied) = 1 by construction.

**Evidence weight: strong and convergent. This is the best-supported verdict in the system.** Five independent sources state that federal LD-2 filings do not identify which legislators were contacted:
- de Figueiredo 2013 (VERIFIED);
- Bacik 2025 (strong);
- Kim & Kunisky 2020, which builds the same kind of roster matrix and says lobbying a bill 'does not necessarily imply political ties to its sponsor';
- Kim 2018 LobbyView, whose Section 2 describes bill matching only;
- Fremeth 2018.

The gap is in the form itself, not in any sample, so the finding transfers fully to every LD-2 vintage. Kim, Stuckatz & Wolters 2020/2023 use the same join, but only as an *outcome* against an independently timed treatment (donation onset). That is the non-circular use of the join, and it shows what a legitimate design looks like. Azaiez 2025 (EC register, which names both parties to each dated meeting) is the benchmark for what a real contact layer needs. No US congressional equivalent exists.

**Measurement details that qualify the join:**
- **Wider than the papers' joins.** The system credits the sponsor *plus all* cosponsors; Kim & Kunisky use sponsors only. Example: BECU 2021 has 181 edges, from 1 sponsor plus 180 cosponsors of H.R. 1996.
- **No committee-referral arm.** Kim 2020/2023 include one. `committee_referrals` is parsed in the rebuild but not credited.
- **Congress binding differs from LobbyView.** The 2021+ rebuild binds citations to the filing Congress unless an 'of 20XX' suffix says otherwise. It has no cosine lookback across the three preceding Congresses. Mis-binding risk is concentrated in filings made early in a new Congress, and its size is UNVERIFIED.
- **No hyphen-range expansion.** The rebuild does not expand ranges such as H.R. 4182-4186 (Kim 2018 bridge row).
- **BICAM (Delano 2025)** is a candidate second path for bill-mention resolution. It adds no contact signal.

**Contradictions**
- build_dyad_signs pushes Kim 2025 bill-level positions onto members through the sponsor/cosponsor roster (support if probability >= 0.7, oppose if <= 0.3). signed_non_cu_opposition_z then enters contest_index. This brings the refuted circular mapping back into a scored term. Kim 2025 measures positions only at group x bill and does not measure group-member positions or contact.
- Paul's own Cross Industry Finance report reads a 2026 'average increase of 20 interactions' for named members as the community-bank industry 'pushing harder'. If those counts come from the bill-roster join (the report does not define 'interactions'), they measure bill-roster overlap, not targeting or contact. The 2026 window is also a partial year.
- Paul's notes read Kim & Kunisky as a tool for identifying champions. The paper itself frames its matrix as descriptive co-involvement and disclaims ties to sponsors.

**Gaps**
- The empirical question (with an independent contact measure, does lobbying precede cosponsorship more than chance?) has no evidence. Filling it requires a contact source that names both parties, never derived from LD-2. Candidates: member office public schedules or calendars, FARA contact reports (foreign principals only), agency visitor logs, or a hand-coded sample of Dear Colleague endorsements with dates. The design should be onset-timed in the style of Kim 2020: a contact first observed at time t, cosponsorship at t+k, and a matched non-contact control. Report it by party and majority status, or label it UNSTRATIFIED.
- Error rate of Congress binding in the rebuild versus LobbyView's lookback: needs a hand-checked sample of filings made in odd-year Q1-Q2.
- A committee-referral arm is absent. If added, it must remain an outcome-side descriptor, never contact.

**Paul's position vs the evidence**

The evidence decisively supports the system's REFUTED verdict, and it does not support reading Kim & Kunisky as a champion-identification tool. At most, the roster matrix answers 'whose bills did this organization lobby on'. Paul's notes have not caught up with the circularity ruling. Two live uses still treat roster overlap as targeting: the dyad-sign term inside contest_index, and the 'interactions' language in his own report.

**Open questions**
- Should signed_non_cu_opposition_z be removed from contest_index, or relabelled as bill-level opposition without member attribution?
- Where do the 'interactions' counts in the Cross Industry Finance report come from? Which join, which window, and which denominator?
- Is any US contact source with both parties named and dated (calendars, logs) obtainable for financial-services members?

**Read first**: [[defigueiredo-2013-advancing-empirical-research]], [[kim-2020-mapping-political-communities]], [[bacik-2025-measuring-dynamical-evolution]], [[kim-2023-systemic-sequential-links]], [[kim-2025-measuring-interest-group]], [[azaiez-2025-hypergraph-analysis-european]]

## FEC to LDA bridge test

**Computes.** Relation between FEC giving and LDA lobbying network change.

**Method.** Round 1 VOID. Round 2 p = .098 (Spearman +0.259, N = 52). Round 3: raw rho +0.153, but the four-control partial is -0.0002. Closed pending a new vintage. FEC as a 42-month leading indicator is a candidate heuristic.

**Code refs.** `fec/lobbying/build_network_2021_rebuild.py`

**Assumptions**
- FEC giving leads lobbying (unvalidated)

**Evidence table** (9 evidence rows from 8 distinct papers; VERIFIED-SOFTENED: 9)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2020-strategic-sequential-links]] | 2020 | qualifies | Firm-politician-year LPMs (Table 3, N=16,428,834, public manufacturing firms NAICS 31-33 x members of Congress, 2008-2017): a first-time donation in year t raises P(firm lobbies a bill the politician sponsored, cosponsored or handled in committee, same year) by 0.102-0.118 (cols 1-4), against a base of about 0.05. The table text sets L = t-1; the matched DiD uses a 3-year no-donation history (L=3). Matched DiD (Fig 5): about +8.5 pp at t for the Senate, falling to about 3 pp (caption: 3.2) at t+3; the House is described as stronger but no number is given. Fig 6: effects larger for corporate PACs and particularly stronger for committee membership; the authors say the effects disappear within two to three years. | Estimands differ: Kim identifies within-pair onset timing (pair FE or matched DiD on first-time donations) among public manufacturing firms; the bridge is a between-member cross-section over concurrent windows dominated by financial trade associations. The bridge Y credits only sponsor and cosponsor rosters, so the committee-referral outcome channel that carries Kim's strongest effect is absent; the bridge's committee control is a covariate (Kim also controls for 'powerful committee' membership), so 'controls away Kim's channel' is an interpretation, not a demonstrated fact. Appendix E's trade-association slope (beta 0.8 vs corporations beta 2) is a cube-root, 1999-2000, organization-level replication of Ansolabehere et al., not Kim's dyad design. So the bridge null does not refute Kim and Kim does not license predictor language. Kim does NOT bear on the '42-month lead' as written, because that number is a data-recency gap, not a behavioral lead; at most Kim argues against reading it as one. A successor onset/pair/committee-referral test is a new prereg and Paul's gate. | strong | VERIFIED-SOFTENED |
| [[furnas-nd-conflating-lobbying-pacs]] |  | supports | Organizations that lobby and organizations that give through PACs barely overlap. Per year, as a share of all politically active organizations: 1998 (n=9,076) 64% lobby only, 24% PAC only, 12% both; 2008 (n=15,729) 70/20/10. Pooled 1998-2018 (Figure 4): 3,427 organizations (6.3%) did both; the 6.3% is printed, and its denominator 54,517 is the sum of the four Figure 4 groups (34,038 only lobby, 9,528 only contribute, 3,427 both, 7,524 not active), so it includes inactive organizations. The authors put the yearly overlap at 10-15% and tell researchers not to conflate lobbying and electioneering. | The paper is about organizations; the bridge is about members (where money exposure and lobbying attention land). Different estimands, and the paper does not explain the zero partial. What carries over is the population point: organizations that lobby without a PAC (64-70% of all politically active organizations per year, about 84-88% of lobbying organizations) leave no organizational FEC trace, so FEC cannot be a general lobbying proxy. Federal organizations, 1998-2018, CRP definitions. UNVERIFIED (DE-062). | moderate | VERIFIED-SOFTENED |
| [[furnas-nd-conflating-lobbying-pacs]] |  | qualifies | Among all politically active organizations, 64-70% per year lobby without making PAC contributions; among lobbying organizations alone that is roughly 84% (1998) to 88% (2008). The share of PAC contribution dollars coming from organizations that also lobby fell from more than 65% (1998) to about 30% (2018). Contributions by employees or contract lobbyists are not counted as organizational giving. | Low overlap undercuts any reading in which one organization's giving foreshadows its own later lobbying, because most lobbying organizations do not give through a PAC. The paper measures organizational/affiliated-PAC money, not the employee giving this prior uses, so it bounds the lead wording rather than refuting it. The freshness reading of lead_months is unaffected. UNVERIFIED (DE-062). | moderate | VERIFIED-SOFTENED |
| [[kim-2020-mapping-political-communities]] | 2020 | suggests-extension | Table 1 lists the 10 dyads whose observed interactions most exceed biLCM predictions (e.g. Shaheen-NOVO NORDISK 12 observed vs 1.61 estimated; Marino-CVS 10 vs 0.55). The authors suggest explanations from geography, campaign contributions (IHEARTMEDIA with Conaway and Barrasso) and bill-specific stakes, without testing them. | Suggests testing whether FEC money concentrates on dyads with positive community-model residuals rather than correlating levels. Hypothesis only; anecdotes from one Congress. Residuals on a bill-roster join inherit its circularity for any join outcome; a different control set on the same window is not a successor test. | weak | VERIFIED-SOFTENED |
| [[kim-2023-systemic-sequential-links]] | 2023 | qualifies | Within-politician first-time-donation designs find donations followed by lobbying on the member's bills. Table 1 pooled coefficients 0.041, 0.048, 0.047, 0.023 (SE 0.006-0.007; N 16,434,912 in table, 16,428,834 in text). Matched DiD Senate 0.075 to 0.110 and House 0.086 to 0.127 over t+0 to t+3 (3,778 Senate and 5,497 House instances). Committee-linked PAC lobbying rises 0.103 to 0.147, versus sponsorship 0.029 to 0.037. Lobbying does not predict later donations, except a small sponsor-lobbying term in App K. | Paper design: firm-politician pair fixed effects, year FE, first-time donation timing, 2008-2018 manufacturing firms. Bridge: member-level cross-section, 2023-26, 22 financial-sector clients, committee control. Different estimand, so the paper does not test the bridge. The paper does not say its committee channel is absorbed by a committee control in a cross-section; that overcontrol reading is the analyst inference (and the code's OVERCONTROL_SENTENCE). The paper is observational and says so (pp.3, 29). | moderate | VERIFIED-SOFTENED |
| [[bickmore-2025-corporate-lobbying-political]] | 2025 | context | Lobbying (Tables 3-5, annual N = 116,242) and PAC (Tables 6-7, election-year N = 62,198 and 62,558) are estimated in separate regressions on different samples. The paper does not estimate them jointly, and 'never in sequence' is an inference from table structure, not an explicit statement. | The paper gives no evidence on whether PAC giving leads lobbying or how much the two overlap, so it neither revives nor refutes the bridge. The claim that Furnas et al. show the sharper overlap relation comes from the vault, not from this paper. | weak | VERIFIED-SOFTENED |
| [[kim-2018-lobbyview-firm-level]] | 2018 | qualifies | Close bill numbers with the same prefix and numbers differing by at most 10 are expanded into ranges. The Mattel 2002 citation H.R.4182-4186 becomes five bills (H.R. 4182 to 4186). Bills are also matched by title and the Congress is propagated across the report. No error rates are reported. | Paper side reproduced. The claim that the bridge used the LobbyView network.csv vintage for 2021-2022 was dropped, since no source for it was checked. Whether the rule difference is measured on an overlap year is UNVERIFIED. | moderate | VERIFIED-SOFTENED |
| [[defigueiredo-2013-advancing-empirical-research]] | 2013 | qualifies | Money is not transferred explicitly to politicians as campaign contributions are, so lobbying is a distinct channel (p.4). Lobbying expenditures are about five times interest-group campaign contributions (pp.5-6). The essay does not address whether campaign giving leads lobbying. | The paper gives no evidence that giving leads lobbying. A lead test would need an exogenous-shock design rather than more controls. | weak | VERIFIED-SOFTENED |
| [[fremeth-2018-spillovers-regulating-corporate]] | 2018 | qualifies | Firm-cycles at the PAC limit associate with +549,166 dollars of LDA lobbying (firm FE and Fama-French 49 industry-by-cycle FE; 2000-2008; 787 firms; 2,783 firm-cycles). The at-limit by post-BCRA interaction is +539,008 (Table 6; 787 firms). LDA filings do not name candidates, so firm-candidate lobbying cannot be linked. | The unit is the firm, not the member, so the paper neither revives nor contradicts the member-level null. A transferable design would test an organization's giving at the cap against that organization's own LDA spend, with time fixed effects; that design is the analyst's suggestion, not the paper's. | moderate | VERIFIED-SOFTENED |

**Assessment**

**What the system found.** The bridge has been run three times.
- Round 1: VOID.
- Round 2: Spearman +0.259, p = .098, N = 52.
- Round 3 (LOCKED look): member x industry x cycle, 2023-2026; pooled N = 213 pairs from fintech and crypto, with CU strata limited by coverage. Raw rho +0.153 (member-blocked p = .015), but the partial correlation after four controls is -0.0002. Status: UNCORROBORATED, window CLOSED, predictor language barred.

**Evidence weight: moderate. It qualifies the bridge rather than deciding it.**
- **Kim, Stuckatz & Wolters 2020/2023 (VERIFIED-SOFTENED, strong).** Within firm-politician pairs (public manufacturing firms, 2008-2018), a first-time donation raises the probability of lobbying on that member's bills.
  - The 2023 Table 1 pooled estimate is +0.04-0.05.
  - The matched DiD estimate is roughly +0.08 to +0.13, decaying by t+3. Its strongest arm runs through committee referral.
  - Lobbying does not predict later donations.
  - **Direction:** this is the best evidence *for* Paul's 'donations lead lobbying' expectation. But the estimand is within-pair onset timing, while the bridge is a between-member cross-section over concurrent windows, dominated by financial trade associations. The bridge outcome also has no committee-referral arm.
  - **Consequence:** the bridge null does not refute Kim, and Kim does not license predictor language for the bridge.
  - The reading that the committee control 'absorbs Kim's channel' is the analyst's inference, recorded in the code's OVERCONTROL_SENTENCE. It has not been demonstrated.
- **Furnas, LaPira & Brock (VERIFIED-SOFTENED; transfer UNVERIFIED per DE-062).** Each year, 64-70% of politically active organizations lobby without a PAC. Among lobbying organizations alone, that share is about 84-88% (1998-2018, CRP). Most lobbying therefore leaves no organizational FEC trace, so FEC cannot be a general lobbying proxy. This is an organization-level estimand. The paper measures affiliated-PAC money, not the employee giving the opposition prior uses.
- **Fremeth 2018.** Firms at the PAC limit spend about $549k more on LDA lobbying (2000-2008, 787 firms, with firm and industry-by-cycle fixed effects). This is firm-level substitution. It neither revives nor contradicts the member-level null.
- **de Figueiredo 2013.** Lobbying is a distinct channel and runs about five times contributions. It offers no lead evidence.
- **Bickmore 2025.** Context only.
- **Kim & Kunisky 2020.** A residual-dyad hypothesis (does money concentrate on dyads above the community-model prediction?). This is anecdotal, from one Congress. On a roster join it inherits the join's circularity.

**Contradictions**
- The '42-month lead' (lobbyview-collector.md:124; memory lda_bridge.lead_months = 42; the component text calls it 'a candidate heuristic') is computed as the months between the end of LobbyView's finance-joined coverage and the latest FEC transaction. It is a data-freshness gap, not a measured behavioural lag. Calling FEC a '42-month leading indicator' is not supported by any evidence and should be renamed.
- Kim 2020/2023 find donations precede lobbying within pairs, while the bridge finds a partial of about 0. These are different estimands, so this is not a true contradiction, but it is often read as one in both directions.
- The Bickmore row says the bridge script builds a Pearson rho. Other rows describe a fractional-rank Spearman. The raw +0.153 was found in the results block of the prereg (Fremeth row) but was not located in the folder checked in two other passes. The statistic name and the result file should be pinned.
- The component text gives round 2 as N = 52 and round 3 as pooled N = 213. These are different units and windows, so the two rounds are not directly comparable.

**Gaps**
- The empirical question (does industry FEC giving lead LDA lobbying expenditure, controlling for firm size and sector?) is untested in this system's population. Filling it needs an organization-level panel of financial-sector clients matched to their own PAC giving: onset of first-time giving to a member as treatment, the organization's own later LDA reports on that member's bills (including committee-referral bills) as outcome, with pair and year fixed effects. It would be a new prereg and Paul's gate, and should be stratified by trade association vs firm. Note that most lobbying organizations have no PAC (Furnas), so the population must be defined as PAC-and-lobby organizations, with the denominator named.
- An exogenous-shock design (for example Fremeth's PAC-limit/BCRA logic) is the route de Figueiredo recommends over adding controls.

**Paul's position vs the evidence**

Paul's notes expect donations to lead lobbying.

- **Where the evidence agrees.** Kim 2020/2023 support this at the within-pair level for manufacturing firms.
- **Where it does not.** Nothing supports it at the member-level cross-section the bridge measures, and nothing supports it for financial trade associations. Furnas shows that most lobbying organizations do not give through PACs at all.
- **The 42-month figure.** The heuristic Paul's materials carry is not evidence for his expectation. It is a measure of data freshness.

The system's closure (FEC as a target-side money prior only) is consistent with the evidence.

**Open questions**
- Rename lead_months to a data-freshness label everywhere it appears as a 'lead'?
- Is a successor onset/pair/committee-referral test worth a new prereg once a new vintage exists?
- Which statistic did round 3 compute (Pearson or fractional-rank Spearman), and which file holds +0.1525?

**Read first**: [[kim-2023-systemic-sequential-links]], [[kim-2020-strategic-sequential-links]], [[furnas-nd-conflating-lobbying-pacs]], [[fremeth-2018-spillovers-regulating-corporate]], [[defigueiredo-2013-advancing-empirical-research]]

## Contact inference (activity to member)

**Computes.** Matches lobbying activity descriptions to members.

**Method.** Gold positives name the member. Negatives come from the same organisation's roster join, split 50/50 by organisation. Contamination is estimated by a recall sample.

**Code refs.** `contact inference lane (code at fec/lobbying/contact_inference/ on branch lane/contact-inference-v1, unmerged as of 2026-10-28 (D-374, D-388, D-389); nothing is shown on the boards until v1 ships (D-373))`

**Assumptions**
- Roster-join negatives are not contacts

**Evidence table** (3 evidence rows from 3 distinct papers; VERIFIED: 1, VERIFIED-SOFTENED: 2)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2020-strategic-sequential-links]] | 2020 | suggests-extension | The DiD design keys on first-time donations (firm i gives to politician j for the first time in L=3 years). The effect appears in year t and declines (Senate about 8.5 pp to about 3 pp by t+3). Corporate PAC donations show larger effects than CEO or employee donations, though all show ties. Effects are particularly stronger for committee membership. | Design hints only: an onset feature (first gift to m in 3 years) instead of gave-in-cycle, a short decay horizon, and a C2 x C3 interaction. They must fit the 10-feature cap and the prereg/one-look discipline. Kim's outcome is lobbying a related bill, not contact, so the paper cannot serve as validation (LobbyView-as-validation is barred). Sample transfer is weak: public manufacturing firms vs financial associations and CUs. | moderate | VERIFIED |
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | method-source | The paper defines a government-association function g(l,y), which is monotone in year, and a political-association function p(l,y), which is not monotone. Both are built from one-time disclosures of covered positions, and lobbyists give no timelines for those positions, so recency cannot be established. | The g and p definitions can be reused as documented, timestamp-free association labels. They are not contact evidence and must not become edge weights (D-021). Any use needs a 'no timeline' label, because the paper itself says it cannot date the ties. | moderate | VERIFIED-SOFTENED |
| [[kim-2018-lobbyview-firm-level]] | 2018 | supports | The Congress assignment takes an argmax over candidate Congresses (Eq. 1) and falls back to the filing-year Congress when no bill matches. The paper says filing-year guessing often leads to erroneous matches (Section 2). It says the full methods description is still to come, with the quoted sentence 'This document currently contains only some technical details of the database' (Introduction). | Paper claims hold, with two corrections. The paper never calls title or range matching heuristic. The methods-deferred quote sits in the Introduction (Section 1), not Section 4.1. The claim that Paul's memos treat LobbyView as a cautionary chain because argmax always returns an answer was not found in the brain vault and is dropped. | moderate | VERIFIED-SOFTENED |

**Assessment**

**What the design is.** Contact inference matches lobbying activity descriptions to members.
- Gold positives are filings that name the member.
- Negatives come from the same organization's roster join, split 50/50 by organization.
- Contamination is estimated with a recall sample.

**Evidence weight: thin. There are 3 rows, and none tests precision.**
- **Kim 2020 (VERIFIED).** It gives design hints for the donation channel C3:
  - an onset feature (first gift to the member in 3 years) instead of 'gave in cycle';
  - a short decay horizon, since effects fade within 2-3 years;
  - a committee x donation interaction.
  
  Kim's outcome is lobbying a related bill, not contact, so the paper cannot validate contact inference (LobbyView-as-validation is barred). Sample transfer is weak: manufacturing firms vs financial associations and CUs. Any feature must fit the 10-feature cap and one-look discipline.
- **Bacik 2025 (VERIFIED-SOFTENED).** It defines g(l,y) and p(l,y), association functions built from one-time revolving-door disclosures with no timeline. They are reusable only as labelled, timestamp-free association descriptors. They are not contact evidence and never edge weights (D-021). Influence language is barred on these revolving-door edges.
- **Kim 2018 (VERIFIED-SOFTENED).** LobbyView's Congress assignment is an argmax that always returns an answer, with a fallback to the filing year. The paper itself defers its full methods.

**What the code shows.** The Bacik row found no shipped code path for this lane. The nearest surface ships `position_basis = 'not_disclosed'`. The gold-positive design therefore cannot be verified from code.

**Contradictions**
- The hub's Paul position says his memos treat the LobbyView lineage as a 'cautionary chain (argmax always returns an answer)'. The Kim 2018 row searched the brain vault, did not find it, and dropped the claim. The brain memo instead requires a caveat that LDA does not disclose contacts, and bars LobbyView-as-validation and lobbied-member wording.
- Roster-join negatives come from the same join that is P(joined | lobbied) = 1 by construction. Some of those 'negatives' may be real contacts, which the recall sample is meant to estimate. No result from that sample is recorded.

**Gaps**
- The core empirical question (precision of description-based contact inference against blind labels) has no evidence. Filling it needs a blind-labelled sample of inferred (organization, member, quarter) rows. Labels should come from a source independent of LD-2 and LobbyView (member schedules, hearing witness lists, press releases naming the meeting), with coders blind to the score. Report precision at fixed score thresholds, stratified by chamber and by whether the member sits on the committee of referral, with the window and denominator named.
- Contamination of the negatives: the recall-sample result needs recording before any precision figure is quoted.
- No shipped code path was found, so the design cannot yet be audited.

**Paul's position vs the evidence**

Borrowing only definitions that can be documented (for example Bacik's g and p, labelled as having no timeline) fits the evidence. The specific 'argmax cautionary chain' framing attributed to Paul's memos was not found in the vault and should not be cited as his position. The memo's actual discipline (inferences carry a caveat; no LobbyView-as-validation) is consistent with every evidence row.

**Open questions**
- Where is the contact-inference code? Was it shipped, or is it design-only?
- What did the recall sample estimate for contamination of the negatives?
- Which independent label source is feasible for financial-services members?

**Read first**: [[bacik-2025-measuring-dynamical-evolution]], [[kim-2018-lobbyview-firm-level]], [[kim-2020-strategic-sequential-links]], [[azaiez-2025-hypergraph-analysis-european]]

## Ridge-logistic cosponsor forecaster

**Computes.** Predicted cosponsor joins on open bills. A frozen forecast of 265,396 predictions is unscored.

Code check 2026-10-28: the count 265,396 appears nowhere in fec/; source artifact not located.

**Method.** Ridge logistic regression. Scoring against the lobbied-member table would be circular.

**Code refs.** `fec/lobbying/cosponsor_forecast.py`, `fec/engine/forecast_ledger.py`

**Assumptions**
- Network and lobbying features predict joins

**Evidence table** (5 evidence rows from 5 distinct papers; UNVERIFIED: 1, VERIFIED-SOFTENED: 4)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[zareie-2020-similarity-based-link]] | 2020 | suggests-extension | DICN = (1 + CN)(1 + Corr), where Corr is the Pearson correlation of second-order neighbourhood vectors. At beta = 80% training it has the highest mean AUC (15 random splits) in 8 of 9 networks (e.g. NSC 0.9747 vs CNDP 0.9195; KHN 0.8839 vs SHOPI 0.8509). It loses on UAL (0.9367 vs NCC 0.9416). On test pairs whose nodes share no common neighbour (Exp. 3, compared only with PA, CCPA and SHOPI), it is best in 5 of 9 networks (e.g. KRT 0.8319, NSC 0.8471). | The paper uses random edge holdout on nine static benchmark graphs (34-3,772 nodes) that it does not describe as weighted or directed; it lists those as future work. The forecaster is temporal (the Congress t-1 graph predicts Congress t joins) and bill-level. Its prior graph is an undirected weighted projection, and its affinity feature is ordered (member, sponsor). Paper margins over the best baseline run from about 0.007 to 0.055 AUC, with no variance reported. A sponsor-candidate CN or DICN feature on the prior-Congress graph is a cheap candidate to test against the affinity baseline. Exp. 3 concerns pairs with no common neighbour, which is a different subset from pairs with zero direct prior affinity, so it should not be read as evidence about zero-affinity pairs. Any lift is UNVERIFIED until shown on the forecaster's own temporal split (DE-062). | moderate | VERIFIED-SOFTENED |
| [[leskovec-2010-predicting-positive-negative]] | 2010 | suggests-extension | Using all 16 signed directed triad counts (which include the 4 positive-only path types) instead of only those 4 positive-only path counts raises accuracy for predicting a positive edge on matched non-edge pairs (chance 0.50): 0.5612 to 0.5911 Epinions, 0.5579 to 0.5953 Slashdot, 0.6983 to 0.7114 Wikipedia (gains 1.3 to 3.7 points). | Gains are 1.3 to 3.7 points on a matched-embeddedness task and may vanish against a cosponsorship-history baseline, which the forecaster must beat first. Only references dated before the forecast Congress may enter. The paper's comparison also changes feature count, so the gain is attributable to the added signed types only. | weak | VERIFIED-SOFTENED |
| [[tang-2015-negative-link-prediction]] | 2015 | supports | Accuracy is misleading under extreme imbalance: a trivial all-missing classifier reaches 99.99 percent accuracy (Sec. 5.1 pp. 92-93). Imbalance is 1:4131 and 1:2534 (Sec. 4.1 p. 90). Parameterized baselines are reported at their best setting, and NeLP parameters are set per site, with no held-out tuning protocol described (Sec. 5.2 p. 93). | The metric choice carries across domains. Tang's unspecified tuning protocol and best-setting baseline reporting are a negative example of protocol, not a template for the system. | weak | VERIFIED-SOFTENED |
| [[boxsteffensmeier-2018-taking-congress-interest]] | 2018 | suggests-extension | Bill-level endorsement signals predict cosponsor counts: max endorser standardized eigenvector centrality coef 0.055 (SE 0.011) and endorser count coef 0.007 (SE 0.001), both p<.001, negative binomial with Congress FE, N = 6,597 House bills with supportive Dear Colleague letters, 106th-111th Congresses. Predicted cosponsors: 33 for a bill endorsed only by unconnected groups (score -0.2053) or with no endorsements; 38 with centrality 2.51 (Anti-Defamation League); 51 with centrality 7.66 (Planned Parenthood). | Paper estimates are pooled across Congresses and both parties with no party or majority strata reported, so quote as UNSTRATIFIED. The outcome is a bill-level cosponsor count, not a dyad join. Any added bill-level signal must be dated before introduction. Endorsements are public pro-bill signals, but the paper does not show they reflect group influence because solicitation direction is unresolved (footnote 11). LDA counts are not a substitute under D-021. | moderate | VERIFIED-SOFTENED |
| [[selfhout-2010-emerging-late-adolescent]] | 2010 | suggests-extension | Reciprocity 1.73 (SE .11), transitivity .14 (SE .02) and distance-2 -.52 (SE .06) were all significant. Adding transitivity and distance-2 improved fit in all 10 groups (chi-square 15.68 to 93.43, df 1), so a model of independent pairs misfits network tie formation. | Transfer is moderate in method and weak in evidence. Reciprocity and closure in friendship nominations do not establish the same effects in cosponsorship; the component's own question about Brandenberger reciprocity is the legislative test. Here 'reciprocity' means tie reciprocation and must not be confused with the money reciprocity that is still unvalidated (D-104..D-108). Any added feature counts only after the frozen 265,396-prediction forecast is scored against a non-circular outcome, and stays UNVERIFIED until a second path reproduces it. | weak | UNVERIFIED |

**Assessment**

**What the forecaster is.** A ridge-logistic model (fixed lambda 1.0) over (bill, member) candidates, trained on prior-Congress features.
- **Features:** prior sponsor affinity, member propensity, degree and eigenvector centrality on an undirected weighted sponsor-cosponsor projection, NOMINATE distances, party, state, chamber, committee terms, and bill-level covariates.
- **Evaluation already built in:** `split_verdict` compares average precision with the best of three baselines (B1 member propensity, B2 sponsor affinity, B3 party match). precision@k is bill-averaged at k = 10, 25 and 50. No accuracy metric is used.

**Evidence weight: weak to moderate. It consists only of candidate extensions; nothing tests the forecaster.**
- **Tang 2015.** Supports the metric choice: under 1:2,500-1:4,100 imbalance, accuracy is meaningless. Its own tuning protocol is a negative example.
- **Box-Steffensmeier 2018.** The eigenvector centrality of interest-group endorsers on Dear Colleague letters predicts bill cosponsor counts. Predicted counts are 33 -> 38 -> 51 across centrality levels: House, 106th-111th Congresses, N = 6,597 bills, negative binomial with Congress fixed effects. It is pooled across parties and Congresses, so it is UNSTRATIFIED. The outcome is a bill-level count, not a dyad join. The direction of solicitation is unresolved (footnote 11), and LDA counts cannot substitute (D-021).
- **Zareie 2020.** A common-neighbour/DICN sponsor-candidate feature wins 8 of 9 static benchmarks by about 0.007-0.055 AUC under random edge holdout. A cheap candidate, but it is UNVERIFIED on the forecaster's temporal split.
- **Leskovec 2010.** Signed triads add 1.3-3.7 points on a matched-embeddedness task. The forecaster has no signed layer, and the gain may vanish against a history baseline.
- **Selfhout 2010 (UNVERIFIED).** Reciprocity and closure in adolescent friendship networks. A lead only. 'Reciprocity' there means tie reciprocation and must not be conflated with the money reciprocity that is not validated (D-104..D-108).

No row argues for either of the two SNA proposals this project rejected (dropping regular cosponsorship edges; normalizing volume per edge).

**Contradictions**
- The hub says a frozen forecast of 265,396 predictions exists and is unscored. Neither the count nor any freeze or pre-registration language was found in cosponsor_forecast.py (Tang and Box-Steffensmeier rows). Until an artifact shows it, the 'frozen before scoring' status is unconfirmed.
- An analyst description that the features are 'member-dyad only' is wrong. Bill-level and sponsor-level covariates are included (Box-Steffensmeier row).

**Gaps**
- The empirical question (does it beat a cosponsorship-history baseline such as Brandenberger's reciprocity or Russo et al.?) has no evidence row. Both papers are cited in the system, but neither was assessed here. Filling it needs scoring on a held-out Congress against a non-circular outcome: actual cosponsorship joins from BILLSTATUS, never the lobbied-member table. Comparators would be the B1-B3 baselines plus a reciprocity baseline (has the sponsor previously cosponsored this member's bills?). Report average precision and precision@k by chamber and majority status.
- No feature for reciprocation or closure exists. It should be tested only after the base forecast is scored once.

**Paul's position vs the evidence**

Paul wants prediction, not reaction, which is his critique of Kim J. 2025. The built-in baseline comparison and the AP metric fit that aim and are supported by Tang. But no prediction has been scored yet, so the claim that the forecaster predicts is untested. Every literature row is an idea for an extension, not evidence that the model works.

**Open questions**
- Where does the 265,396-row forecast live, and is it hash-frozen?
- Which outcome file is the non-circular scoring target?
- Should Brandenberger reciprocity be added as baseline B4 before scoring?

**Read first**: [[tang-2015-negative-link-prediction]], [[boxsteffensmeier-2018-taking-congress-interest]], [[zareie-2020-similarity-based-link]], [[leskovec-2010-predicting-positive-negative]]

## Contested-bills registry

**Computes.** Per-bill mode (standard, advisory or contested) adding a counterparty term (Romero Moreno 2021).

**Method.** Loader shipped; engine wiring not built.

Code check 2026-10-28: fec/surfaces/build_dashboard_bundle.py:741-751 loads the contested-bills registry, so the dashboard consumes it; engine wiring still not traced.

**Code refs.** `fec/lobbying/contested_bills.py`

**Assumptions**
- Owner ratification is the only admission route

**Evidence table** (8 evidence rows from 7 distinct papers; VERIFIED: 4, VERIFIED-SOFTENED: 4)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | suggests-extension | Kang (2016) models lobbying as Tullock contests among four energy coalitions, mapped at the level of policy provisions decoupled from bills. Policies with no lobbying are enacted about 1% of the time and those with two or more lobbying players about 24%, which the review says implies substantial selection. The structural effect of lobbying is much smaller (0.021%-0.415%); entry has multiple equilibria resolved by a selection assumption. | A contest success function is a candidate form for the planned counterparty term, but Kang's model carries misspecification symptoms and selects among multiple equilibria by assumption. The 1% vs 24% contrast reflects selection and must not be quoted as a lobbying effect. Weighting contests by LDA spend would conflict with D-021. | weak | VERIFIED |
| [[pritoni-2025-lobbying-coalitions-determinants]] | 2025 | supports | Issue-context hypotheses are supported 6 of 7 (85.7%; 16-article subsample, unstratified by polity). The authors conclude that groups coalesce more in a highly salient and conflictual policy context (condition i). Similar groups are not the only ones to ally: 'strange bedfellows' coalitions form when the policy context makes them strategically useful (Junk 2019; Beyers and De Bruycker 2018). Prior work found that issue characteristics can matter more than organization-level factors. | The evidence comes from interest-group studies, mostly US and EU, about whether groups form coalitions, not whether they win. It supports per-bill alignment as a structural choice. It says nothing about how large any counterparty term should be, which stays owner policy. | moderate | VERIFIED-SOFTENED |
| [[pritoni-2025-lobbying-coalitions-determinants]] | 2025 | suggests-extension | Relative-weakness hypotheses are supported 11 of 12 (91.7%; Table 4, 16-article subsample, unstratified). The paper says weaker groups 'lobby in packs' to pool resources. A significant minority of studies (Beyers and Donas 2014; Gray and Lowery 1998) finds the opposite, which suggests a minimum resource threshold. The authors leave the question open. | An ally field would be descriptive (who stood with CUs on bill X). LDA may identify allies, but under D-021 co-lobbying cannot become an edge or a weight. The coalition-to-influence link is contradictory (p. 8), so the field should not change any score without its own validation. | weak | VERIFIED |
| [[kim-2025-measuring-interest-group]] | 2025 | suggests-extension | Bill-level positions show opposing interest groups on the same bill. Coal-deregulation bills (H.R. 3409, 112th; H.R. 806, 115th) are opposed by environmental groups and supported by commercial entities, while an environmental-protection bill (S. 847, 112th) shows the reverse. Subject Oppose ratios: Public lands and natural resources 0.354 (51 bills), Environmental protection 0.280 (108 bills). The paper does not tabulate inter-industry conflict per bill. | Kim's data ends at the 117th Congress (2022). It can nominate historical candidate contested bills and calibrate the alliance-conflict recompute. Current bills depend on the weaker Haiku lane. Registry admission stays an owner ruling. An OPPOSED pair from inferred labels is a lead, not a fact. | moderate | VERIFIED-SOFTENED |
| [[alexander-2009-lobbying-analysis-under]] | 2009 | suggests-extension | Five firms lobbied against the AJCA for alternative tax plans and are not counted in the lobbying group (fn 99, p.28). A sensitivity analysis drops five repatriating lobbyists with uncertain support and raises ROI to 228:1 (fn 100, p.30). Coalition lobbyists out-returned solo lobbyists (257 vs 154). The paper does not describe the provision as lacking organized counter-lobbying. | The paper supports only a design implication: returns on a provision with little organized opposition may overstate what contested bills would show. The counter-lobbying evidence is five firms on the other side, so the uncontested label should be softened to five firms lobbied against it. The paper never studies contested bills. | weak | VERIFIED-SOFTENED |
| [[moreno-2021-shadowing-shielding-effective]] | 2021 | method-source | Against a passive opponent with known allocation w_b, the low-allocation optimum has a term linear in w_bi with common coefficient c = (Ba/Bb - 1): shadow the opponent's targets if Ba > Bb, avoid them if Ba < Bb (Eq 5, low-allocation limit only; a degree term enters only when positivity constraints bind). Continuous shadowing plus shielding is within 1.89e-3 of the numerical optimum under budget advantage (degree 2.09e-2, uniform 2.14e-2). Under disadvantage it is 7.28e-4 vs 2.12e-3 for discrete degree targeting (K=16, 15 instances). | Design template only. w_b is relational FEC-associated exposure drawn from a sample of 10 companies per industry (build_opposition_prior.py line 108; not industry-complete), not LDA (D-021). The paper's dynamics are an unnoised voter model on one academic email network, so c and the gap sizes are not calibrated for Congress. The sign of the response needs the per-bill budget ratio measured first. UNVERIFIED for this system until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED |
| [[defigueiredo-2013-advancing-empirical-research]] | 2013 | method-source | Kang (2012) builds an all-pay contest model of counteractive lobbying, estimated on energy groups' support and opposition for policies in 110th Congress bills, and finds the expenditure effect on enactment is very small. Baumgartner et al. (98 issues) find both sides mobilize roughly equal resources and policy is usually stable. | An all-pay contest needs bill-level spending by both sides. LDA issue codes and bill mentions may supply that at the bill level, not the member level. The equal-mobilization finding warns that relative spend may predict little. Admission must stay with Paul's ratification. | moderate | VERIFIED |
| [[kerr-2014-dynamics-firm-lobbying]] | 2014 | qualifies | LDA filings do not require an amount per specific topic (p.19). Issue intensity is proxied by the number of years a topic is listed and by keyword matches in specific-issue text (H-1B, L-1, PERM, high-skill bills), pp.5-6. | A relative-spend test per bill needs report dollars apportioned across issues and bills, which Kerr avoids; a filing or issue-mention count is the defensible proxy. LDA stays an identity and measurement source, never an edge or weight (D-021). | weak | VERIFIED |

**Assessment**

**What exists.** A per-bill registry that marks each bill standard, advisory or contested. Contested records carry an explicit counterparty set, ratified by Paul on 2026-07-31.
- The loader has shipped. No engine consumer exists: CONTESTED_V1 is a planned dial, and the bundle shows the registry for display only.
- The registry file contested_bills.json was not found in the repo root.

**Evidence weight: moderate on structure, weak on effect size. The effect-size evidence leans against Paul's bet.**
- **Pritoni 2025 (systematic review, 16-article subsample, unstratified by polity).** Groups coalesce more in salient, conflictual contexts (issue-context hypotheses supported in 6 of 7 studies). 'Strange bedfellows' coalitions form when strategically useful. This supports per-bill alignment as a structural choice. It says nothing about how large a counterparty term should be.
- **de Figueiredo 2013 and Bombardini 2020, on Kang's contest models.** Kang's all-pay or Tullock contests over energy-policy provisions find a structural expenditure effect on enactment that is *very small* (0.021%-0.415%). Baumgartner et al. (98 issues) find both sides mobilize roughly equal resources and policy is usually stable.
  - Policies with no lobbying are enacted about 1% of the time, against about 24% with two or more lobbying players. That contrast reflects *selection*, not a lobbying effect.
  - These findings warn that relative spend may predict little.
- **Kerr 2014.** LDA does not require an amount per topic, so a per-bill relative-spend test needs dollars apportioned across issues. Filing or issue-mention counts are the defensible proxy.
- **Kim 2025.** Shows bills with opposing groups (for example, coal-deregulation bills). It can nominate historical contested-bill candidates up to the 117th Congress (2022). It does not tabulate conflict between industries. Inferred OPPOSED pairs are leads.
- **Alexander 2009.** A 220:1-type ROI on a provision with little organized opposition (five firms lobbied against it) may overstate what contested bills show.
- **Romero Moreno 2021 (method-source; transfer UNVERIFIED, DE-062).** Shadowing and shielding is optimal against a passive opponent in an unnoised voter model. The sign of the response depends on the budget ratio. Here w_b is FEC-associated exposure from 10 sampled companies per industry, not industry-complete. This is a design template only.

**Contradictions**
- Paul's position treats inter-industry conflict as vital, and the planned term would make relative lobbying weight matter. The closest outcome evidence (Kang; Baumgartner et al.) finds that expenditure effects in contested settings are small and that mobilization is roughly balanced. This does not contradict conflict being *informative*, but it does cut against a large counterparty *effect*.
- The registry rejects any industry on the 'ours' side in counterparty_industries and has no field for allies. Pritoni documents strange-bedfellow alliances, which the schema cannot represent.
- Weighting contests by LDA spend inside the engine would conflict with D-021. A relative-spend analysis must stay a research measurement, not an engine weight.

**Gaps**
- The empirical question (on bills where banks and CUs oppose each other, does relative lobbying spend predict the outcome?) is untested. Filling it needs a bill-level sample of bank-vs-CU contested bills, nominated from Kim 2025 positions up to the 117th Congress and ratified by Paul. Relative effort should be measured by filing or issue-mention counts (Kerr), not apportioned dollars. Outcomes should be stage-wise (committee action, floor, enactment), with selection handled explicitly, since lobbied bills differ from unlobbied ones (Bombardini). Report it by Congress and majority control, or label it UNSTRATIFIED.
- No evidence sizes the counterparty term. Its size remains owner policy until the bill-level study exists.

**Paul's position vs the evidence**

Calling explicit inter-industry conflict 'vital for political intelligence' is supported as a *descriptive* claim. Conflict and coalition structure are real, they depend on context, and they shape who mobilizes (Pritoni; Kim 2025). The literature does not support assuming that relative spend decides contested outcomes; Kang's and Baumgartner's work points the other way. Keeping admission to the registry as Paul's ratification is consistent with the evidence, and so is not wiring an engine consumer before the bill-level test.

**Open questions**
- Should the registry gain a descriptive allies field that has no effect on scoring?
- Where is contested_bills.json, and how many records does it hold?
- Is a Kang-style contest form wanted for CONTESTED_V1, or only a display-mode flag?

**Read first**: [[defigueiredo-2013-advancing-empirical-research]], [[bombardini-2020-empirical-models-lobbying]], [[pritoni-2025-lobbying-coalitions-determinants]], [[kim-2025-measuring-interest-group]], [[kerr-2014-dynamics-firm-lobbying]], [[moreno-2021-shadowing-shielding-effective]]

## Across components

### Cross-component findings

**1. One constraint, read the same way everywhere.** The same structural fact recurs across five components: LD-2 does not name the members contacted.
- It makes the member link circular.
- It makes contact inference necessary.
- It removes the committee channel from the bridge outcome.
- It bars the forecaster from being scored against the lobbied-member table.
- It reaches contested bills through `build_dyad_signs`.

Every paper that bears on it agrees (de Figueiredo 2013, Bacik 2025, Kim & Kunisky 2020, Kim 2018, Fremeth 2018). The system's D-021 ruling is the most strongly evidenced position in this hub.

**2. Where the circularity has leaked back in.** It is ruled out in the member-link component but still appears in three places:
- a scored term: `signed_non_cu_opposition_z` inside `contest_index`;
- Paul's own report: 2026 'interactions' read as industry pressure;
- the brain's framing of Kim & Kunisky as a tool for identifying champions.

All three should be audited together.

**3. Estimand mismatch is the main reading error.** Papers by Kim and coauthors (2020, 2023, 2025) recur across four components. Each time the transfer note says the same thing: the paper's unit (a firm-politician pair, or a group x bill) differs from the system's unit (a member cross-section, or a member dyad). The sample also differs: manufacturing firms or Compustat, not financial trade associations and CUs. The papers are design templates, not validation. LobbyView-as-validation stays barred.

**4. Mislabelled numbers.**
- The '42-month lead' is a data-freshness gap.
- The ABA vs CUNA dollar contrast may mix expense and income (UNVERIFIED).
- The 265,396-row frozen forecast and its freeze clause were not found.
- The round-3 statistic name differs between checks.

Each needs a second path (DE-062) before it is quoted again.

**5. Gap pattern.** Every *empirical question* in the hub lacks direct evidence, while most *assumptions* get qualifying evidence. The missing studies share a need: independent ground truth (blind contact labels, BILLSTATUS joins, or bill-stage outcomes) rather than more LobbyView-derived data.

**6. Wording rules.**
- Bacik's g/p and Kim 2025's legislator-lobbyist relation are revolving-door edges, so influence language is barred on them.
- Box-Steffensmeier's estimates must be quoted as UNSTRATIFIED.
- No row argues for either rejected SNA proposal, so there is nothing to flag there.
- Selfhout's 'reciprocity' means reciprocated ties and must stay separate from the money reciprocity that is not validated (D-104..D-108).

## Research agenda

1. Audit for leaked circularity: decide whether signed_non_cu_opposition_z (built from Kim 2025 positions mapped to members through the sponsor/cosponsor roster) stays in contest_index. Also check how 'interactions' is defined in the Cross Industry Finance report.
2. Rename the 'FEC leads LDA by 42 months' heuristic to a data-freshness label in the brain, memory and the collector note. Remove the term 'leading indicator'.
3. Dollar-semantics check, by a second path: recompute the ABA and CUNA totals split by is_self_filer for a named window. Report whether $228M / $93M are mixed totals.
4. Accuracy audit of the finance classifier: a blind hand-labelled stratified sample (by HIGH/LOW confidence and by rule fired) plus a Huneeus-style 5% recall sample of the 'other' class. Report precision and recall per class, with the denominator named.
5. Vintage check: compare 2021-2023 issue-code report counts across the 2024-10-25 bulk export, the current OSF release and the LDA API rebuild.
6. Score the cosponsor forecaster once on a held-out Congress against BILLSTATUS joins, with baselines B1-B3 plus a Brandenberger reciprocity baseline. First locate and hash the 265,396-row forecast, or record that it does not exist.
7. Contact-inference precision study: blind labels from a source independent of LD-2 (member schedules, witness lists, dated public meeting records). Stratify by chamber and committee of referral. Record the result of the contamination recall sample first.
8. A possible prereg successor to the bridge (Paul's gate): an organization-level onset design among organizations that both give through a PAC and lobby. The outcome would include committee-referral bills, with pair and year fixed effects, stratified by trade association vs firm.
9. Contested-bills study: nominate bank-vs-CU contested bills from Kim 2025 positions (up to the 117th Congress) for Paul's ratification. Test relative filing counts against stage-wise outcomes with selection handled explicitly. No engine consumer before then.
10. Measure the error rate of Congress binding in the rebuild versus LobbyView's lookback, on a hand-checked sample of odd-year Q1-Q2 filings. Also test whether adding hyphen-range expansion changes edge counts.
