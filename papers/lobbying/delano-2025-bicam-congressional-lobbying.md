---
type: paper
title: "Bulk Ingestion of Congressional Actions and Materials Dataset (BICAM)"
authors: ["Ryan Delano", "Aaron Rudkin", "In Song Kim"]
year: "2025"
venue: "Scientific Data 12:1567 (2025)"
doi: "10.1038/s41597-025-05737-8"
category: "Lobbying"
systems: ["melange-legislative-data", "melange-lobbying"]
components: ["melange-legislative-data.congress-activity-score", "melange-lobbying.finance-client-classification", "melange-lobbying.lobbied-member-link"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/delano-2025-bicam-congressional-lobbying.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\s41597-025-05737-8.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\adoring-yalow-ad763f\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\agent-a2168cba4e2fdc5a2\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\agent-a5628f8fee4a9c869\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\amazing-lewin-409893\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\codex-websocket-error-468fa9\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\ecc-benchmark-methodology-c4f259\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\evidence-csv-fetch-streaming-8bedff\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\forecasting-model-audit-5603f2\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\melange-math-eval-skill-18e907\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\melange-members-section-fixes-6613db\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\melange-ui-redesign-372c43\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\mentat-github-migration-b06195\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\mentat-systems-production-file-53552c\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\pac-map-downstream-viz-c2bca4\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\sector-growth-political-spending-9cc0c8\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\silly-einstein-57ef31\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\unfinished-work-handoff-a3bbad\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\vibrant-khayyam-65f943\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\vibrant-sammet-a549a5\\research\\2025_SciData_Delano_BICAM.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\research\\2025_SciData_Delano_BICAM.pdf"]
sha12: ["58dbc0a2b055"]
tags: [paper, lobbying, type/paper, theme/lobbying, evidence/verified]
---

# Bulk Ingestion of Congressional Actions and Materials Dataset (BICAM)

**Ryan Delano, Aaron Rudkin, In Song Kim (2025).** *Scientific Data 12:1567 (2025).* [10.1038/s41597-025-05737-8](https://doi.org/10.1038/s41597-025-05737-8). Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> BICAM (Delano, Rudkin and Kim, Scientific Data 2025) is a data descriptor, not a research article. It merges the Congress.gov and GovInfo APIs into one PostgreSQL/CSV release covering 421,764 bills (6th-119th Congress), 123,061 amendments, 2,597 members keyed by bioguide_id and icpsr, 226 committees and 587 subcommittees, 74,918 hearings, reports, prints, nominations and treaties. Its two external linkages are a bill crosswalk to Voteview and a Bill-to-LDA linkage that matches bill numbers and titles extracted from LD-2 Section 16 text to the Bills table by distance scores, with low-confidence matches reviewed by hand. The authors say this finds significantly more bill lobbying than earlier sources (Kim and Kunisky 2021), including ex-post lobbying. The main text gives no match counts, precision, recall or threshold; that material is in the Supplementary Information, which was not available here. Validation checks integrity only (completeness, types, unique keys, cross-source agreement) and treats Congress itself as ground truth. For Paul's system the paper matters at one point: how bill mentions in LD-2 filings are resolved. Melange's lobbying network uses its own parser for those mentions in build_network_2021_rebuild.py, which has a known 0.5 percent slash-citation loss, and then credits every sponsor and cosponsor of each lobbied bill. BICAM could be a second, independent path for checking bill resolution. It does not fix the circularity: the LDA still names bills, never the members contacted, so a bill-to-member join made from BICAM would still be P(joined given lobbied) = 1 by construction. Second, BICAM's bulk bills and cosponsors could replace or cross-check the congress.gov scrapers behind the activity score. That is a sourcing option and is UNVERIFIED until it is run.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** No research question is posed. The paper's aim is to provide a standardized, linked, bulk-downloadable database of official U.S. congressional records (1789 to present) and to make Bill-to-LDA lobbying linkages available for interest-group research.
**Theory.** None stated. Background cites literature on committees, agenda control, bill text, amendments and hearings as motivation for the data, not as tested theory.
**Data.** Official sources: Congress.gov API and GovInfo.gov API, merged into a PostgreSQL relational database and exported as CSV. Eleven components: Bills (421,764; 6th-119th Congress), Amendments (123,061; 97th-119th), Members (2,597; 81st-119th), Committees (226 committees and 587 subcommittees; 1st-119th), Committee Reports (106,818; 16th-119th), Committee Prints (4,346; 67th-119th), Committee Meetings (12,238; 114th-119th), Hearings (74,918; 79th-119th), Treaties (758; 81st-119th), Nominations (43,858; 97th-119th), Congresses (119 sessions). Unit is the record; the lobbying linkage is between Bills and LDA Section 16 filings (LD-2 reports). LDA data come from the House and Senate; LobbyView and OpenSecrets are noted as existing compilations.
**Method.** Data engineering rather than statistical analysis. Steps: (1) gather from both government APIs with a custom Python package (pause/resume, error recovery, parallel download); (2) import into PostgreSQL, enforce types and unique keys, and connect components by identifiers; (3) merge the two upstream sources with rules-based corrections, keeping the most complete composite record; (4) add record linkages to Voteview (custom bill identifier, crosswalk to Voteview) and to LDA Section 16 filings by matching extracted bill numbers and titles to Bills using distance measures, with low-confidence matches manually scrutinized.
**Identification.** Not applicable. No causal or inferential design is used.
**Scope.** Population: all electronically available official U.S. congressional records. Institution: U.S. Congress and its committees, with sub-components varying in temporal start. Era: 1789 to the 119th Congress, with the download current to the paper's publication window and updated on bicam.net. Excluded: full Congressional Record transcripts, Congressional Communications, CRS reports, member press releases, executive-branch data (FEC donations and election results), and rollcall and lobbying data themselves (only linkages provided).

## Key findings

- BICAM contains 421,764 bills spanning the 6th to 119th Congress, linked via bill_id (Methods, Bills section, p.6).
- The Bills component includes 123,061 amendments from the 97th to 119th Congress (Amendments section, p.6).
- All 2,597 members who served from the 81st to 119th Congress are included, with icpsr and bioguide_id keys (Members section, p.6).
- BICAM covers 226 committees and 587 subcommittees, with committee history from the 1st to 119th Congress (Committees section, p.6).
- Committee reports: 106,818 from the 16th to 119th Congress; committee prints: 4,346 from the 67th to 119th (p.6-7).
- Hearings: 74,918 from the 79th to 119th Congress; committee meetings: 12,238 from the 114th to 119th Congress (p.7).
- Nominations: 43,858 presidential nominations from the 97th to 119th Congress; treaties: 758 from the 81st to 119th (p.7).
- Retrieving the full dataset via the Congress.gov API requires over 100 endpoints and a 5,000 requests per hour cap; at least 10 requests are needed for one bill (Methods, p.4).
- The paper claims the Bill-LDA linkage detects significantly more bill lobbying than existing sources and captures ex-post lobbying (Introduction and Methods, p.4-5); no counts or precision/recall are given in the main text.
- Validation is integrity-focused: completeness, type checks, unique identifiers, linkage consistency and cross-source agreement; the paper states that Congress itself is treated as ground truth (Technical Validation, p.7-8).
- Discrepancies between the two APIs, such as missing bioguide_id for nickname members, are resolved by a rules-based merge (Technical Validation, p.8).
- Discontinued third-party sources (Sunlight Foundation in 2016, ProPublica congressional data in 2024, NYT Congress API, GovTrack API) motivate the open, reproducible design (Introduction, p.2-3).

## Limitations

- Main text reports no quantitative accuracy or error rate for the LDA linkage or for the overall data; validation is integrity-focused (p.7-8).
- Bill-LDA matching uses distance measures on extracted bill numbers and titles; lower-confidence matches get extra scrutiny but the rules and threshold are in the Supplementary Information, not verified here (p.8).
- Committee meeting witness and supporting documents remain in PDF and are not yet extracted to text (p.6).
- Excluded components: full Congressional Record, CRS reports, congressional communications, member press releases, and rollcall and lobbying data themselves (p.5).
- Sub-components have different temporal coverage, so historical analyses may be limited (p.3, p.6-7).
- Manual effort is still needed for some discrepancies between sources (p.8).
- The authors note that data availability depends on the continued operation of the upstream government APIs (p.3).

## Short quotes

- "BICAM's primary advantage is its wide breadth, easy record linkages, and rolling updates"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-lobbying#Lobbying member-year network and circular lobbied-member link|Lobbying member-year network and circular lobbied-member link]] | qualifies | Bill numbers and titles extracted from LD-2 Section 16 are matched by distance measures to the Bills component (421,764 bills, 6th-119th Congress); low-confidence matches get extra scrutiny. The linkage is said to detect significantly more bill lobbying than Kim and Kunisky 2021 and to capture ex-post lobbying. The main text gives no match count, precision, recall or threshold. (Intro p.4-5 (text lines 176-188); Bills p.6 (lines 233-234); Technical Validation p.8 (lines 408-416)) | build_network_2021_rebuild.py credits every sponsor and cosponsor of each lobbied bill (line 13). Generous outputs keep ambiguous-title fan-out; strict outputs need a same-segment formal citation. Slash-joined citations parse only the left side (line 19; 2 of 376 distinct bills, 0.5 percent, 2023-24 association corpus). LD-2 has no member or position field (lobby_interactions.py line 18). `fec/lobbying/build_network_2021_rebuild.py:13; fec/lobbying/build_network_2021_rebuild.py:19; fec/lobbying/lobby_interactions.py:18` | BICAM fits only as a candidate second path for bill-mention resolution. A bill-to-sponsor join from it is a roster edge with P(credited given lobbied) = 1 by construction and adds no contact signal. Precision and recall sit in the Supplementary Information, which this check did not have. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#LDA finance-client classification and report join|LDA finance-client classification and report join]] | context | BICAM supplies Bill-LDA linkages, not the LDA filings, and points to LobbyView and OpenSecrets as the compilations of LDA data. Members are keyed by bioguide_id and icpsr. Discrepancies between the two APIs, such as GovInfo missing bioguide_id for nickname members, are fixed by a rules-based merge. (Background p.3-4 (lines 89-94, 177-178); Members p.6 (lines 259-266); Technical Validation p.8 (lines 398-407)) | collect_lobbyview.py is a standalone ingest of the LobbyView export dated 2024-10-25 (VINTAGE, line 73). The n_bills caveat at line 446 describes sponsored or cosponsored attention to bills, not disclosed contact. `fec/lobbying/collect_lobbyview.py:73; fec/lobbying/collect_lobbyview.py:446` | Context only. BICAM does not replace the client or filer source. Any post-2021 bill linkage from it would serve identity resolution, not classification input. | weak | VERIFIED-SOFTENED |
| [[systems/melange-legislative-data#congress.gov legislative activity score with Congress decay|congress.gov legislative activity score with Congress decay]] | measurement-source | Bulk bills, amendments, committees and members from the official APIs with rolling updates. Retrieving the full set directly needs over 100 endpoints, a 5,000 requests per hour cap, and at least 10 requests per bill. (Background p.3 (lines 114-116); Methods p.4 (lines 133-137); Data Records p.6-7) | sync_sentinel_bill_ledger.py decay_weight gives 1.0 for Congress 119, 0.8 for 118 and 0.64 for 117, described in the code as the frozen legacy archive schedule. The code read does not show that this weight feeds the activity score. The congress.gov search-URL scraping in R was not inspected. `fec/legislation/sync_sentinel_bill_ledger.py:96` | Sourcing cross-check only. Reconciling scraped counts against BICAM sponsor and cosponsor tables would need a run first. Remains UNVERIFIED. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The summary implies the Bill-to-LDA linkage reaches 421,764 bills. The paper names the Bills component as the match target and gives no match count.
> - Summary issue: The summary calls BICAM a second independent path for bill resolution. BICAM uses the same kind of LDA text extraction, so it is a comparator until its precision and recall are read from the Supplementary Information.
> - Summary issue: The summary says Melange credits every sponsor and cosponsor. The network has generous and strict variants that disambiguate titles differently.
> - Summary issue: The summary ties a frozen 0.8 decay to the activity counts. The code shows a 1.0, 0.8, 0.64 schedule written as a weight on ledger rows, and the feed into the activity score is not shown.
> - Summary issue: The summary says finance-client classification is reliable through 2021. This was not found in the code read or the paper.
> - Summary issue: Not in the summary: ext.json lists the 123,061 amendments under the Bills component, and lists Lauderdale and Clark 2014 as a linkage BICAM makes. BICAM cites that paper only as text-scaling research (refs 24-26).

## Gaps and flags

- The bill-mention parser in build_network_2021_rebuild.py has no independent second path. Its 0.5 percent slash-citation loss is measured on one corpus only, and BICAM's distance-scored linkage is an available comparator.
- The Bill-LDA linkage quality figures are in the BICAM Supplementary Information, which is not in the vault text, so no precision or recall can be quoted.
- BICAM's ex-post lobbying coverage (lobbying of enacted laws) is not separated out in Melange's network; whether the parser captures post-enactment citations is unmeasured.
- The paper's claim of more bill lobbying detected must not be carried over as more lobbying influence or contact. LD-2 never names members (D-021), and any bill-to-member join stays circular.
- No accuracy figures appear in the main text, so the claim of high-quality linkages is unquantified here.

Paul has no annotation entries for this work, so there is no takeaway to compare. The paper is consistent with the system's stance that LDA links bills, not members; it offers a data source, not a finding.

## Related papers

- [[kim-2020-mapping-political-communities]] — extends — Real item (vault dates it 2020, online). BICAM says its Bill-LDA linkage improves on Kim and Kunisky 2021, ref 17. Same paper; link holds.
- [[kim-2018-lobbyview-firm-level]] — complements — Real item, a 2018 working paper. BICAM cites LobbyView as a 2024 technical report (ref 15). Likely the same lineage but a different version, so the link holds only as a version link.
- [[kim-2025-measuring-interest-group]] — complements — Real item. BICAM does not cite it. The complement judgment is the analyst's inference from the title, not a BICAM statement.
- [[poole-1999-nominate-after-years]] — complements — Real item. BICAM names Voteview/NOMINATE and builds a Voteview crosswalk (lines 89 and 238-239), citing Lewis 2024 (ref 8), not Poole 1999. Link holds on subject, not on citation.

### Linked from
_None._