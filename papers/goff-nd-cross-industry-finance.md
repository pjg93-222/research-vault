---
type: paper
title: "Cross Industry Finance and Lobbying Disclosure Report: Analysis & Recommendations"
authors: ["Paul Goff"]
year: "2026 (inferred, not stated on the page: the text cites 2026 LDA reports and a projected 2025-2026 cycle)"
venue: "None. Unpublished client report addressed to Trey Hawkins and Hannah Benito."
doi: ""
category: "Paul's Research Notes"
systems: ["melange-allocation", "melange-lobbying", "mentat-fec-attribution", "mentat-giving-forecast"]
components: ["mentat-giving-forecast.cycle-model", "melange-lobbying.lobbied-member-link", "mentat-fec-attribution.transfer-graph-and-direction", "mentat-fec-attribution.employer-match", "melange-allocation.break-even-and-roi"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/goff-nd-cross-industry-finance.docx]]"
original_paths: ["C:\\Users\\pgoff\\OneDrive\\Desktop\\Cross Industry Finance and Lobbying Disclosure Report.docx"]
sha12: ["0b0cbbaaa98a"]
tags: ["paper", "paul-s-research-notes"]
---

# Cross Industry Finance and Lobbying Disclosure Report: Analysis & Recommendations

**Paul Goff (2026 (inferred, not stated on the page: the text cites 2026 LDA reports and a projected 2025-2026 cycle)).** *None. Unpublished client report addressed to Trey Hawkins and Hannah Benito..* Theme: [[themes/paul-s-research-notes|Paul's Research Notes]].

> [!abstract] Summary
> Paul's own unpublished client report (to Trey Hawkins and Hannah Benito; year not printed, 2026 inferred) surveys political money in financial services and makes three recommendations to Credit Unions: raise giving 41% over the 2023-2024 baseline to about $4.3M (the $4.3M target and its implied baseline are derived by the analyst, not stated in the text), push employee and middle-management giving to trade groups, and run campaigns that set CUs apart from banks. The extract only covers the Crypto and Community Banks sections. Crypto giving rose from $1.76M (2021-22) to $145.5M (2023-24). The text reports +8,162.6%; the gap to about +8,167% from the rounded figures is explained by rounding (an unrounded base near 1.7609), which the report does not state. The report gives the C-suite share of that giving as going from 69% to 98.8%, and super PACs as taking 71.9% of non-direct giving, which is itself 70% of the total. A 2025-26 projection of more than $175M would be a midterm-cycle rise, which the report attributes to the CLARITY Act. The report says crypto-backed candidates won 38 of 40 primaries (no window or baseline), and opinion runs 31% oppose to 27% support (no source given). Community-bank member LDA activity is reported as rising by an average of 20 interactions, a term the report never defines. The method is descriptive throughout: no model, no uncertainty, no identification design. Against the system as built, the report matters in three places. (1) Mentat's cycle model applies a symmetric x1.8635 presidential factor on the midterm step, but only to the general_giving measure; whether crypto runs through that measure was not checked. If it does, a large 2025-26 drop is projected, the opposite of the report's projection. (2) If the per-member LDA interaction counts come from the bill-roster join that the lobbying lane rejected as circular (an inference, since the report names no join), they would use LDA as a weight source, which D-021 bars. (3) The super-PAC share and the C-suite share both point to gaps in what Mentat currently splits out. The abstract promises five industries; the extract covers only two.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Across five financial-services industries (Crypto, Community Banks, Financial Technologies, Credit Arms, Credit Unions), how do campaign giving and lobbying differ in scale, direction and effectiveness, and what should Credit Unions do to gain political traction? The question is stated in the abstract (lines 4-5), not as a formal hypothesis.

**Theory.** No formal theory is stated. The narrative relies on three informal claims: (1) in politics, attention is the scarce resource and money buys attention (line 12); (2) public visibility of giving signals to peers that lobbying is in vogue (line 9); (3) rising spending draws scrutiny and skepticism, creating a "self-destructive feedback loop" (line 14). None of these is tested.

**Data.** Sources named: FEC, FDIC, SEC and LDA databases (line 4). The extract gives no sample size, unit of analysis or exact date ranges beyond the election cycles 2021-2022, 2023-2024 and a projected 2025-2026. Quoted items are from press interviews and public statements (Bankman-Fried, Armstrong, Dimon, Lummis, ICBA CEOs). Public-opinion figures (31% oppose, 27% support) are given without a source (line 14). LDA member examples are Zach Nunn, Andy Kim, Young Kim and Bill Huizenga (line 17).

**Method.** Descriptive. Campaign-finance and lobbying information was "extracted at scale" from the named databases, with percentage changes and shares computed from those extracts, plus qualitative material gathered for narrative (line 4). No model, regression, statistical test or uncertainty estimate appears in the extract.

**Identification.** None. The report makes no causal identification claims with a design. Causal language such as CLARITY Act push as the "impetus" for the 2025-2026 increase (line 9) is asserted, not tested.

**Scope.** US federal politics and financial services, election cycles 2021-2026. Population: Crypto and Community Banks industry giving and lobbying, with Credit Unions as the recommendation target. Institution and era: post-2021 crypto political spending and the CLARITY Act debate.

## Key findings

- Crypto industry spending rose from $1.76M in 2021-2022 to $145.5M in 2023-2024. The report calls this an 8,162.6% increase. ($1.76M -> $145.5M; reported +8,162.6%; Crypto section, paragraph 1 (line 7))
- Share of Crypto giving from C-suite donors rose sharply between cycles. (69% (2021-2022) -> 98.8% (2023-2024); Line 7)
- Super PACs take most non-direct-to-candidate giving, which is itself most of industry giving. (Super PACs = 71.9% of non-direct giving; non-direct = 70% of industry giving; Line 7)
- Fairshake is described as the 2nd largest Super PAC and is entirely funded by crypto. (Rank 2; 100% crypto (qualitative); Line 9)
- Projected Crypto spending for the 2025-2026 cycle is above $175M, an off-year increase the report attributes to the CLARITY Act push. (north of $175M (projection); Line 9)
- Crypto-backed candidates won most primaries they were backed in. (38 of 40 primaries; Line 13)
- Crypto outspends key allies by a large factor, which the report says leaves Credit Unions unable to compete if they come into conflict with the industry. The graph meant to show this is missing. (factor of about 100 (stated; no underlying figures given); Line 12 (refers to placeholder on line 11))
- Public support for Crypto legislation is weak: more Americans oppose than support it. (31% oppose vs 27% support (source not given); Line 14)
- Community Bank members' LDA activity increased in 2026 reports. (average increase of 20 interactions within the year ("interactions" is undefined in the text); Line 17)
- Recommendation 1: raise Credit Union giving 41% over the 2023-2024 baseline. (+41%; $4.3M total giving, a target the analyst derived. The baseline dollar figure is not stated; $4.3M/1.41 implies about $3.05M, also derived and not in the text.; Abstract (line 5))
- Recommendation 2: Credit Unions should encourage employees and middle management to give to industry trade organizations, framed as command, communication and community. (; Abstract (line 5))
- Recommendation 3: run information campaigns with policymakers and the public on how Credit Unions differ from banks. (; Abstract (line 5))
- Credit Unions risk losing political capital by opposing the CLARITY Act alongside the ABA and Dimon, and the report says the ACU later signaled support. (; Line 10)

## Limitations

- Stated in the text: none. The report has no limitations section and no methods note.
- Observed gap: the $1.76M-to-$145.5M figures imply about an 8,167% increase, not the reported 8,162.6%. The difference is small and is explained by rounding (an unrounded base near 1.7609 gives 8,162.6%), but the report does not state the unrounded base.
- Observed gap: the abstract promises five industries, but the extract covers only Crypto and Community Banks, with no Credit Union or Credit Arms analysis.
- Observed gap: the 'factor of 100' outspending claim and the 'graph' are not backed by any table, figure or source in the extract.
- Observed gap: the 'interactions' LDA measure and the 31%/27% public-opinion figures have no definition or source.
- Observed gap: no sample, unit, or date range is given for the database extracts, and no uncertainty is reported.
- Observed gap: the causal claims (CLARITY Act as the 'impetus', a 'self-destructive feedback loop', and the claim that Armstrong's denial of knowledge is 'doubtful') are the author's judgment, not tested results.

## Short quotes

- "We are not taking advantage of the opportunities to leverage our capital in Washington" — Community Banks section (line 17), quoting an unnamed ICBA-member CEO
- "lenders, community banks, and credit unions would oppose the legislation if it moves forward in its current form." — Crypto section (line 10), quoting Jamie Dimon via the report
- "Crypto advocacy is very important for our mission." — Crypto section (line 7), quoting Brian Armstrong (2022)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-giving-forecast#Cycle-aware giving refit (phase and drift log models, 50-50 growth)\|Cycle-aware giving refit (phase and drift log models, 50-50 growth)]] | qualifies | Crypto industry giving is projected north of 175 million dollars for 2025-2026, versus 145.5 million in 2023-2024, an off-year rise the report calls an outlier and attributes to the CLARITY Act push. (Crypto section, line 9 of the text extract) | Code applies one presidential factor u = 1.8635 to the general_giving measure on both cycle steps (UPLIFT_BOTH_STEPS at giving_forecast.py line 1425; gate at line 1888), so the 2025-26 midterm step is scaled by 1/1.8635, about minus 46 percent. Company PACs keep the fitted phi on the midterm step. `C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py:1410-1425` | Only applies if crypto giving is run through the general_giving measure, which was not checked. The report's 175 million is a projection with no stated method, window or source, so it is a counterexample to test, not evidence. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Lobbying member-year network and circular lobbied-member link\|Lobbying member-year network and circular lobbied-member link]] | qualifies | 2026 LDA reports show members Zach Nunn, Andy Kim, Young Kim, Bill Huizenga and others with an average increase of 20 interactions within the year, read as the community-bank industry pushing harder. (Community Banks section, line 17) | lobby_interactions.py reuses the build_network_2021_rebuild sponsor and cosponsor join for organization, bill and member attribution. The rebuild docstring (line 13) says each lobbied bill credits every legislator who sponsored or cosponsored it, so per-member counts from this join measure bill-roster overlap, not observed contact. The report never defines interactions or names its join. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/lobbying/build_network_2021_rebuild.py:13` | Applies only if the report's interaction counts come from the bill-roster join, which the report does not say. If they come from described contacts, only the window and denominator flags remain. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-fec-attribution#PAC transfer graph and beneficiary direction partition\|PAC transfer graph and beneficiary direction partition]] | suggests-extension | Super PACs take 71.9 percent of crypto non-direct-to-candidate giving, which is 70 percent of industry giving; Fairshake is the 2nd largest super PAC and wholly crypto-funded; backed candidates won 38 of 40 primaries. (Crypto section, lines 7, 9 and 13) | fec_build_beneficiary_tables seeds the traversal from each industry's funded committees (fec_beneficiary.R traversal_seed, around lines 1041-1060). Downstream rows carry a Support or Oppose direction (fec_beneficiary.R lines 136-145 and 348), so oppose money can be split from support money. An attributed-estimate view exists in the UI, but the route refuses with 409 until an estimate bundle exists (app/evidence-flow.js lines 8-10). A comment at R/export_guards.R line 278 says 89 percent of one shipped beneficiary sheet was opposition-direction rows. `C:/Users/pgoff/Mentat/mentat/R/fec_beneficiary.R:1041-1060` | The 38 of 40 count has no window, no denominator of contested races and no baseline win rate. The 71.9 and 70 percent shares have no stated cycle or source. Reuse needs the support and oppose split by party and a stated cycle. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-fec-attribution#Employer-string roster matching and overrides\|Employer-string roster matching and overrides]] | suggests-extension | The C-suite share of crypto donations rose from 69 percent in 2021-22 to 98.8 percent in 2023-24; the report frames the credit union shortfall as missing employee and middle-management giving. (Crypto section, line 7; abstract line 5 for the employee and middle-management framing) | collect.R assigns Schedule A rows to roster companies by employer key (company_employer_keys, line 363). Occupation text enters only as an optional scoped regex on override rules (lines 520-529), keeping the most specific match. No C-suite or employee role taxonomy was found in that section. `C:/Users/pgoff/Mentat/mentat/R/collect.R:363-381 and 520-529` | An occupation-role split would need a hand-coded precision check. The report gives no coding rule for C-suite, so its 69 and 98.8 percent figures cannot be reproduced from the extract. | moderate | VERIFIED |
| [[systems/melange-allocation#Break-even CU spend and ROI floor\|Break-even CU spend and ROI floor]] | qualifies | Crypto outspends key allies by factors of 100, so credit unions would have no chance in a monetary fight with the industry; the supporting graph is a missing placeholder. (Line 12 (graph placeholder at line 11)) | The head-to-head pull uses a saturating term (x+1) over (x+1) plus HALFF times rcp, weighted by WM and added to a coalition term WP (make_dashboard_html_00.tpl lines 1096-1105). The dashboard prints the cannot fully offset message only when q is at least 1. The functional form is an assumption and is not validated against outcomes. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/surfaces/templates/make_dashboard_html_00.tpl:1096-1105` | The factor of 100 has no source, window or denominator, so the system cannot size the disadvantage. The bounded term shows only that the form caps the monetary pull, not that money decides outcomes. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The rounding explanation for the 8,162.6 percent versus about 8,167 percent gap holds: an unrounded base near 1.7609, displayed as 1.76, gives 8,162.6 percent. The report should state the unrounded base.
> - Summary issue: The summary reads the community-bank 20-interaction figure as lobbying-contact evidence. The report never defines interactions, so the figure should be labeled undefined rather than refuted.
> - Summary issue: The summary says the per-member LDA counts use LDA as a weight source that D-021 bars. That is an inference from the bill-roster join, not a statement in the report, and should be marked inferred.
> - Summary issue: The summary places the C-suite framing in the abstract. Abstract line 5 names employee and middle-management giving; C-suite appears only at line 7.
> - Summary issue: The transfer-graph code_ref is narrowed: fec_beneficiary.R lines 1041-1060 cover seeding only. The Support/Oppose partition is at lines 136-145 and 348. The 89 percent figure counts rows in one sheet (R/export_guards.R line 278), not dollars.
> - Summary issue: The summary implies the midterm factor applies to crypto-like industries. The code applies it only to the general_giving measure; the measure assignment for crypto was not checked.
> - Summary issue: The summary states that Fairshake is the 2nd largest super PAC and wholly crypto-funded. Those are the report's claims at line 9, with no date or metric, not verified facts.
> - Summary issue: The summary omits that the abstract promises five industries but the extract contains only Crypto and Community Banks. Credit Union, Financial Technologies and Credit Arms sections are absent from this file.
> - Summary issue: The summary's descriptions of a self-destructive feedback loop and money buying attention are author judgments with no test, not findings.
> - Summary issue: The 4.3 million credit union target and its implied baseline of about 3.05 million are derived by the analyst and do not appear in the text. Flag as derived, not reported.
> - Summary issue: The extract has no Paul markup (no highlights or notes), so Paul-note items are none; coverage is the full 21-line extract.

## Gaps and flags

- No industry-specific regime-shift term in the cycle model: if crypto runs through the general_giving measure (not checked), a bill-driven midterm surge (crypto 2025-26) is forecast as a symmetric midterm drop under that factor.
- Mentat produces no occupation-role (C-suite vs employee) split of individual giving, which the report's main CU recommendation depends on.
- The support/oppose attributed-estimate producer for super-PAC money is unbuilt, so super-PAC share of industry giving cannot yet be read as support for candidates.
- No primary-election outcome measure exists to test claims like 38 of 40 backed primaries won.
- No independent member-level lobbying contact measure exists; per-member LDA counts can only come from the circular bill-roster join.
- D-021 and refuted lobbied-member link: the report never defines interactions (average +20), so the figure is undefined, not refuted. It is inferred, not stated, that the counts come from the bill-roster join; if so they would use LDA as a member weight and be circular by construction. Do not cite as lobbying contact.
- Window and denominator missing: factor of 100 outspending, average of 20 interactions (which members, which baseline year), 38 of 40 primaries (which cycles), 31% vs 27% opinion (no source), Fairshake rank 2 (no date or metric).
- Arithmetic: $1.76M to $145.5M is about +8,167%, versus the reported +8,162.6%; rounding explains the gap (unrounded base near 1.7609), but the report does not state the unrounded base.
- Causal and influence language without a design: CLARITY Act as the impetus, money buys attention, self-destructive feedback loop; all asserted, untested.
- Coverage: abstract promises five industries but the extract contains only Crypto and Community Banks; the CU baseline dollar figure is not stated ($4.3M / 1.41 implies about $3.05M; both the $4.3M target and the baseline are analyst-derived).

No bibliography entries; this is Paul's own report. Its focus on employee giving and CU distinctiveness fits the built 16-PAC CU cohort. Two points bear on the system: the per-member LDA interaction counts are undefined in the report and, if they come from the bill-roster link the lobbying lane refuted as circular (an inference), D-021 would bar them as a weight; and the midterm crypto surge would clash with Mentat's symmetric general-giving midterm factor (D-121) if crypto runs through that measure, which was not checked. Money-buys-attention is untested here.

## Related papers

- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — A 2025 work on the crypto lobby in American politics; the report's Crypto section could be checked against it. Not checked against the text here.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Furnas, LaPira and Brock find low overlap between organizational lobbying and PAC spending, which bears on the report's implicit reading of LDA activity and giving as one push. The report does not state that reading, so this is not a direct contradiction. Relation changed from contradicts to complements.
- [[kalla-nd-campaign-contributions-facilitate]] — complements — Randomized field experiment on contributions and access; the nearest tested version of the report's money-buys-attention claim, which the report leaves untested. Use access wording, not influence.
- [[kim-2018-lobbyview-firm-level]] — same-data — LobbyView bill-level LDA data is the likely source of the bill-roster join behind the per-member counts. The report does not name its source, so this link is inferred.
- [[bacik-2025-measuring-dynamical-evolution]] — complements — Work on dynamics of the US lobbying network, relevant to the cycle-phase assumption. Its cycle-synchronization claim was not checked against the Bacik text here.
- [[kim-2020-strategic-sequential-links]] — complements — Tests the sequencing of donations and lobbying, the assumption behind combining FEC and LDA evidence in the report. The report does not test sequencing itself.

### Linked from
- [[harrison-2018-application-data-mining]] — same-data — Paul's report draws on LDA disclosures, so the same caution about mixing the two series and about rounding applies.
- [[anon-2025-cross-industry-political]] — complements — Paul-authored cross-industry finance and lobbying report (author Goff) covering credit unions and community banks. Overlap with this memo may be real, but the memo names are redacted, so shared data lineage is unverified.
- [[fremeth-2018-spillovers-regulating-corporate]] — complements — Real item (title: Cross Industry Finance and Lobbying Disclosure Report). Complement claim not re-read.
- [[note-request-for-review-industry-economic-performance]] — complements — Paul earlier report on five financial sectors using FEC, FDIC, SEC and LDA data. Overlapping sectors and sources, but a snapshot report, not the sector-year panel.
- [[note-industry-growth-and-political-spending-research]] — same-data — Item record exists (Goff, Cross Industry Finance and Lobbying Disclosure Report). Title checked only; the same-data claim was not re-read.
- [[note-review-industry-growth-and-political-spending]] — same-data — Paul's client report on LD-2 lobbying disclosure is plausibly built on the same LDA filings. The review does not mention it and no data-level match was checked.
- [[note-sector-growth-and-political-spending-source]] — complements — Paul's unpublished client report on financial-services political money covers bank and credit-union sections, overlapping the note's sectors. The note does not cite it.