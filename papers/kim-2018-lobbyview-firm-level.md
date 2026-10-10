---
type: paper
title: "LobbyView: Firm-level Lobbying & Congressional Bills Database"
authors: ["In Song Kim"]
year: "2018"
venue: "Working paper, MIT Department of Political Science (draft dated August 30, 2018). No journal or series named in the text."
doi: ""
category: "Lobbying"
systems: ["melange-lobbying"]
components: ["melange-lobbying.contact-inference", "melange-lobbying.fec-lda-bridge", "melange-lobbying.finance-client-classification", "melange-lobbying.lobbied-member-link"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/kim-2018-lobbyview-firm-level.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\lobbyview (1).pdf", "C:\\Users\\pgoff\\Downloads\\lobbyview.pdf"]
sha12: ["68dd7ab2a4fc"]
tags: [paper, lobbying]
---

# LobbyView: Firm-level Lobbying & Congressional Bills Database

**In Song Kim (2018).** *Working paper, MIT Department of Political Science (draft dated August 30, 2018). No journal or series named in the text..* Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Kim's 2018 LobbyView note is a partial technical manual, not an empirical study. It has no estimates, no N and no stated coverage window. It documents how LDA lobbying reports get linked to congressional bills. First, a regex finds bill numbers. Then each mention is assigned to a Congress by taking the argmax of cosine similarity between the candidate bill text and the text around the mention, looking back three Congresses by default; if nothing matches, the code falls back to the filing-year Congress. The Congress is then propagated across the report by majority vote, titles are matched, and close bill numbers (gap of 10 or less) are expanded into ranges. Industries are grouped by NAICS2/SIC for per-industry Herfindahl indices, with Compustat merged in. The note names one failure case: Google's 2013 report cites H.R. 2577, which is a 112th-Congress bill, so the naive filing-year rule gets it wrong. It gives no error rates for any heuristic.
> 
> This matters for Paul's lobbying lane in three ways:
> 1. **The paper describes no member link.** The system's own join is a bill-roster join (sponsor plus all cosponsors), not a contact record.
> 2. **Paul's own LDA rebuild uses a different bill-resolution rule than the LobbyView pipeline.** A citation without an "of 20XX" suffix binds to the filing-year Congress, which is the naive rule this paper says fails. Hyphenated ranges are not expanded.
> 3. **The paper uses NAICS as an industry grouping key and does not validate NAICS codes.** The system classifies clients by name first, with NAICS as a fallback.
> 
> None of this is a validation of anything. The heuristics' accuracy is unreported, and the argmax always returns an answer.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** No explicit research question. The document describes and documents the LobbyView database, which links congressional bill sponsorship with lobbying reports filed under the Lobbying Disclosure Act of 1995, and the API used to bulk-download it (Abstract; Section 1).

**Theory.** No theory section. The Abstract gives the motivation only: lobbying by special interest groups matters for policymaking, and empirical studies of political representation have been limited by the difficulty of observing a direct link between politicians and interest groups.

**Data.** Source: the universe of lobbying reports filed under the Lobbying Disclosure Act of 1995 (Abstract). Units named in the text: lobbying reports, lobbying issue sections ("specific issues"), bills (ids of the form "110_HR7311", Section 5.2), firms/clients, and industries (NAICS2 groupings, Section 5.5). Sample size N: not reported. Time period: not stated as a single range; the CONGRESSES setting in general.py sets the range and its default values are not given (Section 4.1). Example years used in the text are illustrative (2002 Mattel report, 2011 filter example, 2013 Google report). Firm financials come from Compustat (Section 5.5.2), and industry codes from NAICS and SIC. The 52,418 count in Section 5.3 is a query result over all loaded reports that carry the TRADE (DOMESTIC/FOREIGN) issue; the text gives no year restriction for it.

**Method.** Descriptive data construction, not statistical estimation. Bills in reports are found by regular-expression search for bill numbers (Section 2, step 1). The Congress is assigned by maximizing the cosine similarity between a bag-of-words vector of each candidate bill and a vector of the text around the bill mention, over the candidate Congresses (Section 2, step 2, Eq. 1). Congress numbers propagate across a report, with majority vote where bills disagree (step 3). Bill titles are matched to report text (step 4). Bill ranges are expanded (step 5). Storage uses SQLite with SQLAlchemy and Elixir, and full-text search uses Whoosh over CRS summaries and specific issues (Sections 3.1, 5.4). Herfindahl indices are computed per industry (herfindahl.py), then augmented with a trade-issue indicator and Compustat data (herfindadd.py) (Section 5.5).

**Identification.** None. The paper makes no causal claim and presents no identification strategy.

**Scope.** Population: lobbying reports and firms/clients under the LDA (1995 onward). Institution: US Congress bills and their sponsorship. Era: post-1995 LDA filings, with the Congress range set in configuration. Stated purpose: enable systematic research on lobbying and merging with external datasets (Abstract).

## Key findings

- LobbyView is built from the universe of LDA lobbying reports and links bill sponsorship to reported lobbying, with firm- and industry-level identifiers and an API for bulk download. Numbers: not reported. (Abstract)
- Naive assignment of a bill to the filing-year Congress fails. The Google 2013 report cites H.R. 2577, which is from the 112th Congress, not the 113th. Numbers: 113th naive guess vs 112th correct. (Section 2, Figure 1 discussion)
- Congress identification considers the three preceding Congresses by default and picks the candidate bill with the highest cosine similarity to the report text. If no same-numbered bill exists, the filing-year Congress is used. Numbers: default 3 preceding Congresses. (Section 2, steps 2 and Eq. 1)
- Congress assignment propagates to other bills in the same report, since reports cover a single Congress. Majority vote is used if bills disagree, and the text says this rarely occurs in practice. Numbers: not quantified. (Section 2, step 3)
- Bill range expansion treats close bill numbers as lobbied together. The default takes numbers with the same prefix that differ by at most 10 and expands the range. The Mattel 2002 example expands H.R.4182-4186 into five bills. Numbers: default difference <=10; 5 bills in example. (Section 2, step 5, Figure 2)
- The trade-issue filter on the lobbying reports returns 52,418 reports that include the TRADE (DOMESTIC/FOREIGN) issue. Numbers: 52,418 reports. (Section 5.3 (query example))
- The Herfindahl index measures competition among firms (clients) within an industry. Output is per year per classification system (NAICS or SIC), and the example run covers 1996 to 2011 with NAICS. Numbers: example years 1996-2011. (Sections 5.5, 5.5.1, 5.5.2 (example))

## Limitations

- The author says the document contains only some technical details and that the full methods description (NLP disambiguation of interest group names, collaborative filtering, PostgreSQL/Elasticsearch storage, scalability, descriptive and statistical analyses) will follow (Section 1).
- Sample size, the period covered, and the default CONGRESSES range are not reported.
- TEST MODE for bill detection is described as incomplete, and the text warns against enabling it (Section 4.1).
- Setup from scratch may take up to several days (Section 4.2).
- Setup is tied to the author's Della/tigress environment and paths (Sections 4.1, 4.2).
- If no bill number matches in the Congress range, the code guesses the filing-year Congress, which can be wrong (Section 2, step 2).
- Error rates for bill-title and range matching are not reported.

## Short quotes

- "This document currently contains only some technical details of the database." (Section 1 (Introduction))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-lobbying#Lobbying member-year network and circular lobbied-member link\|Lobbying member-year network and circular lobbied-member link]] | supports | The abstract says LobbyView bridges politicians' sponsorship of bills with reported lobbying by interest groups. Section 2 steps 1-5 describe only matching report text to bill identities (number, Congress, title, range). No step describes a legislator link or any contact. The word sponsor appears once in the paper, in the Abstract. (Abstract; Section 2 steps 1-5) | The system's join is stated in code. lobby_interactions.py docstring says the network answers whether an organization lobbied a bill the member sponsored or cosponsored. build_network_2021_rebuild.py line 13 says each lobbied bill credits every sponsor and cosponsor. So the member link is a bill-roster join, not a contact record. The paper does not describe this join itself. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/lobbying/lobby_interactions.py:4-6 and build_network_2021_rebuild.py:13` | Holds for the system join. The paper's sponsorship wording is only the abstract phrase, and the system's join is wider: sponsor plus all cosponsors. collect_lobbyview.py line 446 records a 2026-08-01 check that BECU 2021 has 181 edges equal to 1 sponsor plus 180 cosponsors of H.R. 1996. That is a code comment; it was not re-run here. | strong | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Lobbying member-year network and circular lobbied-member link\|Lobbying member-year network and circular lobbied-member link]] | suggests-extension | Filing-year Congress assignment fails. Google's 2013 report cites H.R. 2577 (Safe Data Act), a 112th Congress bill, not the 113th. LobbyView considers the three preceding Congresses by default, picks the candidate with highest cosine similarity to surrounding text (Eq. 1), falls back to the filing-year Congress if no number matches, and propagates a Congress across the report by majority vote. (Section 2 (Figure 1; steps 1-3; Eq. 1; fallback sentence)) | The 2021+ rebuild binds each formal citation to the filing Congress unless an 'of 20XX' suffix in the same segment resolves to an earlier Congress (_resolve_mention_congress; current mentions set the resolved Congress to filing_congress). The rebuild has no cosine-similarity lookback and no report-level propagation. A search for cosine, propagate, majority and argmax returned no hits. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/lobbying/build_network_2021_rebuild.py:3698-3721 and 4482-4492` | Paper side reproduced. Under the rebuild rule, the paper's own example would bind to the 113th Congress. Risk concentrates in filings made early in a new Congress (odd-year first sessions). The paper gives no rate, so the size of the error in the rebuild is UNVERIFIED. | moderate | VERIFIED |
| [[systems/melange-lobbying#LDA finance-client classification and report join\|LDA finance-client classification and report join]] | qualifies | Firms (clients) are grouped into industries by NAICS2 (or SIC) to compute per-industry Herfindahl indices. The example run is NAICS for 1996-2011. Output is then merged with Compustat firm data and a trade-issue indicator (Sections 5.5, 5.5.1, 5.5.2). (Sections 5.5, 5.5.1, 5.5.2) | collect_lobbyview.py classifies clients by name rules first (credit-union and bank rules). NAICS is used only as a fallback and yields a low-confidence side. A '+naics-conflict' suffix is recorded when a validated NAICS code disagrees with a name rule (classify_client, lines 213-263). The paper uses NAICS2 as its industry key and does not validate NAICS codes. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/lobbying/collect_lobbyview.py:12-19 and 213-263` | The procedure claim holds. The paper does not say NAICS is usable for research aggregates; it only uses NAICS2 as the grouping key. The reason given for the system's rule (codebook gives NAICS no provenance) is not in the code checked, so it is UNVERIFIED. The system ruling that NAICS is linkage metadata was not checked here. Herfindahl outputs are industry descriptors and must not be edge or weight sources (D-021, not re-checked). | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#FEC to LDA bridge test\|FEC to LDA bridge test]] | qualifies | Close bill numbers with the same prefix and numbers differing by at most 10 are expanded into ranges. The Mattel 2002 citation H.R.4182-4186 becomes five bills (H.R. 4182 to 4186). Bills are also matched by title and the Congress is propagated across the report. No error rates are reported. (Section 2 steps 3-5; Figure 2) | The rebuild's continuation parser (CONTINUATION_RE) accepts only comma, semicolon, ampersand and 'and' separators, so a hyphenated range is not expanded. No hyphen-range expansion was found in the rebuild. Congress binding differs (see the Congress bearing). `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/lobbying/build_network_2021_rebuild.py:575-580` | Paper side reproduced. The claim that the bridge used the LobbyView network.csv vintage for 2021-2022 was dropped, since no source for it was checked. Whether the rule difference is measured on an overlap year is UNVERIFIED. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Contact inference (activity to member)\|Contact inference (activity to member)]] | supports | The Congress assignment takes an argmax over candidate Congresses (Eq. 1) and falls back to the filing-year Congress when no bill matches. The paper says filing-year guessing often leads to erroneous matches (Section 2). It says the full methods description is still to come, with the quoted sentence 'This document currently contains only some technical details of the database' (Introduction). (Section 1 Introduction; Section 2 opening paragraph, step 2 and Eq. 1) | The brain memo on the contact-inference design, Claims discipline bullet (line 55), requires a mandatory caveat that LDA does not require disclosure of contacts and that scores are inferences. It bars LobbyView-as-validation and lobbied-member wording for inferred rows. The memo does not mention argmax or any cautionary lineage. `C:/Users/pgoff/Documents/brain/topics/2026-07-feature-data-collection-lanes-contact-inference-design-and-evaluation.md:55` | Paper claims hold, with two corrections. The paper never calls title or range matching heuristic. The methods-deferred quote sits in the Introduction (Section 1), not Section 4.1. The claim that Paul's memos treat LobbyView as a cautionary chain because argmax always returns an answer was not found in the brain vault and is dropped. | moderate | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Dropped related links: kim-2025-measuring-interest-group (text mentions LobbyView once and never mentions lob_id, so the lob_id keying claim is unsupported) and harrison-2018-application-data-mining (text has no LobbyView mention, and the claim that Paul treats it as an intro guide next to LobbyView is unsupported).
> - Summary issue: summary_md point 1 says every LobbyView-derived member link is a bill-roster join. The paper describes no member link at all; the join is the system's own construction, as the lobby_interactions.py docstring states.
> - Summary issue: summary_md point 2 says network.csv covers 2000-2022 and that an unmeasured seam exists between the vintage and the 2021+ rebuild. The 2000-2022 range and the seam were not verified in the code or the paper. The rebuild's unsuffixed-citation rule does differ from the paper's, which is verified.
> - Summary issue: summary_md point 3 says the system has ruled NAICS linkage metadata only. The code supports name-first classification with NAICS fallback, but the ruling itself was not checked.
> - Summary issue: paul_alignment quotes Paul calling LobbyView the 'fundamental data source' for lobbying classification. That phrase was not found anywhere in the brain vault, and the Paul entry carries no takeaways. The quote and the cautionary-chain characterization are unsupported and should be removed.
> - Summary issue: paul_alignment says network linkage collapses after 2021 with 2022 = 2,101 rows. No 2,101 figure was found in the lobbying code, so it is UNVERIFIED. The code does show issue-code coverage collapsing after filing_year 2021 (collect_lobbyview.py line 34 coverage note).
> - Summary issue: paul_alignment says Paul's rebuild uses the naive filing-year Congress rule for unsuffixed citations. Verified at build_network_2021_rebuild.py lines 4482-4492 and docstring lines 15-18.
> - Summary issue: Coverage: all 5 bearings and all 6 related links were checked against the text and code. ext and rel were read from the item JSON itself, since no separate ext.json or rel.json exists. ext key findings beyond the Congress example and the 52,418 count were not separately verified. The 52,418 count is in the paper (Section 5.3) with no year restriction.

## Gaps and flags

- The 2021+ LDA rebuild has no prior-Congress lookback for unsuffixed bill citations. They bind to the filing Congress, which is the failure case the paper documents (H.R. 2577, 112th Congress, cited in a 2013 report). The system has never measured how often this happens in the finance rebuild.
- Hyphenated bill ranges (e.g., H.R.4182-4186) are not expanded or counted as a diagnostic, so it is unknown how many lobbied bills the rebuild drops compared with the LobbyView vintage.
- No overlap-year (2021) reconciliation between LobbyView network.csv dyads and the system's strict rebuild was found in the notes I read.
- The paper reports no error rates for any linkage heuristic. Both instruments, LobbyView and the rebuild, lack a hand-labelled precision/recall sample for bill-to-Congress resolution.
- Flag: The paper describes the link as bill 'sponsorship', but the system measured that LobbyView network edges are the sponsor plus all cosponsors. Any wording that inherits the paper's description understates the join and should say 'sponsored or cosponsored'.
- Flag: D-021: the paper's design turns lobbying reports into member-linked edges and industry Herfindahl weights. Adopting either as an edge or weight source in the Melange graphs would conflict with D-021, which allows LDA as an identity source only (lob_id, client names).
- Flag: Nothing in the paper licenses influence or contact language about lobbied members. The link is a bill-roster join, so P(joined | lobbied) = 1 by construction.
- Flag: The 52,418 TRADE-issue report count has no window or year restriction in the text. It must not be quoted without stating that it covers 'all loaded reports, period unstated'.
- Flag: The paper's argmax-plus-fallback Congress assignment always returns an answer. Copying it as-is would break the system's never-guess, fail-closed resolution style. Any adoption should flag ambiguous citations instead.

Paul's own rebuild uses the naive filing-year Congress rule for unsuffixed citations, which is exactly what this paper warns against.

## Related papers

- [[kim-2020-mapping-political-communities]] — builds-on — Item exists; its text mentions LobbyView 3 times. Whether its member edges inherit this paper's Congress heuristics was not checked, so the builds-on link is UNVERIFIED beyond the LobbyView mentions.
- [[bacik-2025-measuring-dynamical-evolution]] — same-data — Item exists; its text mentions LobbyView 37 times, so it uses LobbyView data. The 1999-2023 range and the shared disambiguation lineage were not checked.
- [[huneeus-2018-effects-firms-lobbying]] — same-data — Item exists; its text mentions LobbyView 3 times and Compustat 9 times, so it uses firm-level LobbyView data with Compustat. The exact merge against herfindadd.py was not checked.
- [[kim-2020-strategic-sequential-links]] — complements — Item exists; its text mentions LobbyView 3 times and FEC 22 times, so it pairs lobbying with donations. The LobbyView-to-FEC pairing was not checked in detail.

### Linked from
- [[bombardini-2020-empirical-models-lobbying]] — cited-by — The review cites Kim (2018) LobbyView as the state-of-the-art Compustat-linked lobbying data set used by Huneeus and Kim.
- [[harrison-2018-application-data-mining]] — complements — LobbyView cleans and links the same LDA reports at full scale, the database Paul says supersedes Harrison. The is_self_filer field that allows the expense-vs-income split is in the LobbyView data the collector reads; it is not described in the LobbyView paper extract.
- [[furnas-2019-partisan-ties-lobbying]] — same-data — Both build on Lobbying Disclosure Act filings; this paper uses the registrant-versus-client split to isolate contract firms.
- [[kim-2025-measuring-interest-group]] — same-data — Both rest on LDA reports organised in LobbyView. The paper maps MapLight onto lobbying pairs organised in LobbyView.
- [[kim-2020-mapping-political-communities]] — same-data — The paper's lobbying dataset is distilled from LobbyView (cited as Kim 2018) and shares its NLP bill-identification pipeline.
- [[bickmore-2025-corporate-lobbying-political]] — complements — Not same data: Bickmore uses CRP lobbying matched to Compustat by company name, not LobbyView. Relation changed from same-data to complements because the enum has no context value.
- [[alexander-2009-lobbying-analysis-under]] — same-data — Item exists. Both rest on firm-level LDA filings; the claim that LobbyView is the processed form of the same Senate records was not checked.
- [[boxsteffensmeier-2018-taking-congress-interest]] — complements — Real item (verified). LobbyView links firms to bills through lobbying reports, as the analyst said.
- [[goff-nd-cross-industry-finance]] — same-data — LobbyView bill-level LDA data is the likely source of the bill-roster join behind the per-member counts. The report does not name its source, so this link is inferred.
- [[delano-2025-bicam-congressional-lobbying]] — complements — Real item, a 2018 working paper. BICAM cites LobbyView as a 2024 technical report (ref 15). Likely the same lineage but a different version, so the link holds only as a version link.
- [[defigueiredo-2013-advancing-empirical-research]] — complements — LobbyView firm-level lobbying and bill database. The essay calls for bill-level linkage of transaction data but does not name this source. The claim that the system uses it as its fundamental source is not from the essay.
- [[kerr-2014-dynamics-firm-lobbying]] — complements — Corrected from same-data. Kerr uses CRP and SOPR LDA filings (pp.5-6), not the LobbyView database. LobbyView is the source of Mentat's sector-year series, so the link is by data family only.
- [[note-sector-growth-and-political-spending-source]] — complements — LobbyView is a firm-level LDA database. The note does not name LobbyView as its source for the bank and CU aggregates, so this is background for the LDA measure, not confirmed same-data.