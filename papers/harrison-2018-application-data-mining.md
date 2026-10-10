---
type: paper
title: "An Application of Data Mining Techniques to Explore Congressional Lobbying Records for Patterns in Pediatric Special Interest Expenditures Prior to the Affordable Care Act"
authors: ["Elizabeth Harrison", "Caitlin Dreisbach", "Nada Basit", "Jessica Keim-Malpass"]
year: "2018"
venue: "Frontiers in Big Data, Data Report, vol. 1, article 3 (published 20 August 2018; received 24 April 2018; accepted 2 August 2018)"
doi: "10.3389/fdata.2018.00003"
category: "Lobbying"
systems: ["melange-lobbying", "mentat-giving-forecast"]
components: ["melange-lobbying.finance-client-classification", "mentat-giving-forecast.calculator-and-nowcast"]
paul_annotated: true
paul_markup_count: 11
evidence_status: "verified"
source: "[[_sources/harrison-2018-application-data-mining.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\fdata-01-00003.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\fdata-01-00003.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\fdata-01-00003.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\fdata-01-00003.pdf", "C:\\Users\\pgoff\\Downloads\\fdata-01-00003.pdf"]
sha12: ["47b10c7a3512"]
tags: [paper, lobbying]
---

# An Application of Data Mining Techniques to Explore Congressional Lobbying Records for Patterns in Pediatric Special Interest Expenditures Prior to the Affordable Care Act

**Elizabeth Harrison, Caitlin Dreisbach, Nada Basit, Jessica Keim-Malpass (2018).** *Frontiers in Big Data, Data Report, vol. 1, article 3 (published 20 August 2018; received 24 April 2018; accepted 2 August 2018).* [10.3389/fdata.2018.00003](https://doi.org/10.3389/fdata.2018.00003) Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Harrison, Dreisbach, Basit and Keim-Malpass (2018) is a short data report, not a study of influence. It parses one quarter of Senate LDA XML (Q4 2009, just before the ACA) into eight pandas data frames. It sums reported dollars by umbrella issue code (Taxation $527.1M, Health $436.7M, with no denominator for the filing count) and finds 35 records whose Specific Issues text contains "pediatric". It runs no statistics. Its useful content is a list of data hazards:
> - **Two series mixed:** the XML amount field does not separate lobbying expenditures from income (the mapping of expenditures to in-house spending and income to outside-firm fees is the analyst's reading; the paper says only 'expenditures' and 'income').
> - **Imprecise amounts:** expenses under $5,000 appear only as a checkbox, and client income over $5,000 is rounded to the nearest $10,000.
> - **Bundled issues:** one filing lists many issues, so dollars cannot be assigned to a single issue.
> - **Filing time vs activity time:** files are grouped by when they were filed, so a quarter's files can carry activity from 2002 to 2008.
> 
> For Paul's systems, those hazards matter more than the results. Mentat's lobbying card and `sector_spend.py` now enforce the paper's main warning. They keep outside-firm fees and in-house expenses as two series and never add them (D-064). Separately, Melange commit 914623152 changed the industry page (`make_lobby_network_html.py`) on 2026-10-09 to show the two series side by side and removed a summed $139.4M telecom figure; it is not a change to Mentat's card. `sector_spend.py` also assigns filings by activity period and keeps only the latest amendment.
> 
> The older LobbyView collector does not follow the same rule. Its per-side `amount_sum` and its top-spender `total_amount` add self-filer expenses and hired-firm income together. The "ABA $228M vs CUNA $93M" figure in Paul's auto-memory note (lobbyview-integration-2026-07-31.md, not the brain) probably comes from that mixed sum (UNVERIFIED). On issue codes, the collector correctly counts reports rather than dollars, which avoids the bundling trap. The paper does not say its own per-code dollar totals repeat a filing across codes; that is an inference from its merge description. The paper has nothing to say about networks, persuasion, legislators or elections.

## Paul's annotation

From [[annotations/Lobbying - Lobbying - Annotated Bibliography|Lobbying - Lobbying - Annotated Bibliography]]:

> Harrison, Elizabeth, et al. “An application of data mining techniques to explore congressional lobbying records for patterns in pediatric special interest expenditures prior to the Affordable Care Act.” *Frontiers in Big Data*, vol. 1, 20 Aug. 2018, [https://doi.org/10.3389/fdata.2018.00003](https://doi.org/10.3389/fdata.2018.00003).
>
> This article by professors from the University of Virginia, Charlottesville explores the use of data-mining in lobbying records to measure the influence of special interest groups in the legislative process of congressional bills, most notably with the Affordable Care Act (Obamacare). It provides a cursory study into how to construct the models necessary for the pipelining of such information, through the use of Python pandas, where to obtain such information, such as the LDA, and how to transform said data into actionable information. It is relatively important to the current research in that it provides a foundational basis for the processing of such data and the do’s and don’ts of such methods. However, the results from this data are lack-luster and this is likely a result of the authors limitations in-regards to their technical ability, and other articles, such as Professor Kim’s, provide a database of a far greater magnitude. Still, for an introductory overview of the pipelining of such data, this article proves adequate.

## Paul's markup in the PDF

- p1 Highlight: "[paraphrase] Background: long US history of federal advocacy on behalf of children, traced in Sec. 1."
- p1 Highlight: "[paraphrase] First federal child-health funding effort (1921 Sheppard-Towner Act) designated funds for prenatal programs for poor women and children."
- p2 Highlight: "[paraphrase] Python offered as a general-purpose toolbox for political researchers, clinicians and data scientists (Sec. 2)."
- p2 Highlight: "[paraphrase] Q4 2009 chosen as a period of intense healthcare lobbying before the ACA passed (Sec. 2.1)."
- p2 Highlight: "[paraphrase] Each quarter has about 25 congressional lobbying XML files of roughly 1,000 lines each (Sec. 2.1)."
- p2 Highlight: "[paraphrase] ElementTree parses the hierarchical XML into root and child attributes, which are sorted into other structures (Sec. 2.2)."
- p2 Highlight: "[paraphrase] Filings are grouped by filing time, not activity time; some Q4 2009 filings cover 2002-2008 activity (Sec. 2.3)."
- p2 Highlight: "[paraphrase] An effective initial query needs a wide set of related search terms (Sec. 2.4)."
- p3 Highlight: "[paraphrase] Sorted totals by code: Taxation/Internal Revenue Code $527,147,333 is largest; Health Issues $436,651,121 is next (Sec. 3)."
- p4 Highlight: "[paraphrase] The original reporting forms are inefficient, so the report raises concerns about lobby reporting at its source (Sec. 3)."
- p4 Highlight: "[paraphrase] Future work could statistically analyze the currency values in each lobbying record (Sec. 4). This item is the one green highlight; the other ten are yellow."

## Study

**Question.** Stated in Sec. 1 and Sec. 2: the paper illustrates data parsing principles for mining federal lobbying records, and shows how one might investigate trends in how pediatric health concerns enter the political environment, using Q4 2009 LDA filings made before the Affordable Care Act. No formal hypothesis is stated.

**Theory.** Not stated. The background (Sec. 1, p.1) motivates the work with a general concern about the thin empirical literature on financial lobbying influence on health legislation, and with a history of child-health advocacy. No theoretical framework or testable hypothesis is set out.

**Data.** Source: public Lobbying Disclosure Act (LDA) XML downloads from the US Senate Office of Public Records (Sec. 2.1, p.2). Sample period: lobbying filings submitted in Q4 2009, filtered to events dated 2009 (filings in that quarter also contained activity from 2002-2008, Sec. 2.3, p.2). Unit: the lobbying filing (XML records, each grouped by filing time). Size: about 25 XML files per quarter at roughly 1,000 lines each, more than 25,000 lines of XML in total (Sec. 2.1). Eight data frames: Filing, Issue, Entity, Org, Lobbyist, Registrant, Client, Government Entity (Sec. 2.3). Pediatric keyword search returned 35 results from the Q4 2009 frame (Sec. 2.4, p.2). Total number of filings in the quarter is not reported.

**Method.** Descriptive data engineering in Python 3.6.4: ElementTree parses the XML hierarchy; os.listdir enumerates files; a parsing loop (pseudocode in Fig. 2) converts records into Pandas data frames, with defaultdict holding a master dictionary (Sec. 2.2-2.3). Filing and Issue frames are merged, and reported dollar amounts are summed by issue code (a Pandas series grouped by code, Sec. 2.3). A search function returns every record whose Specific Issues text contains a user-supplied term; the term "pediatric" is used (Sec. 2.4). Search hits are compared with the code-level totals (Sec. 2.4). No statistical tests, models or regressions are run.

**Identification.** None. This is a descriptive data-parsing report. It makes no causal or identification claim and does not estimate any effect of lobbying on legislation. Metadata hint confirmed (title, year, venue, DOI); full author first names taken from the byline on p.1.

**Scope.** Population: lobbying disclosures filed under the LDA in one calendar quarter (Q4 2009), covering all issue areas, with a pediatric subset identified by keyword. Institution: US federal Congress and federal agencies (the LDA). Era: pre-ACA (2009-2010 passage). A single-quarter, single-keyword methods illustration, not a population-level or multi-year study.

## Key findings

- Largest sum of reported lobbying dollars for Q4 2009 is filed under the umbrella code Taxation/Internal Revenue Code. (Sec. 3, p.3; $527,147,333)
- Health Issues is the second-largest umbrella code by reported dollars. (Sec. 3, p.3; $436,651,121)
- The pediatric keyword search returns 35 records from the Q4 2009 frame, spread across several umbrella codes. (Sec. 2.4, p.2 (hits); Sec. 3, p.3 (codes); 35 hits; codes include Health Issues, Budget Appropriations, Science/Technology, Education, Disaster Planning & Emergencies, Medicare/Medicaid)
- Pediatric-related issues are bundled with many unrelated issues inside single filings, so dollars spent on pediatric lobbying cannot be isolated. (Sec. 3, p.3; not quantified; illustrated with one filing listing multiple vaccine, Medicare/Medicaid, FDA and pandemic issues together)
- Records are grouped by filing date, so a quarter's files include activity from earlier years. (Sec. 2.3, p.2; activity dated 2002-2008 filed in Q4 2009; a date filter restricts output to 2009 events)
- The XML amounts do not separate lobbying expenditures from income, and no identifier links them, so reported dollars cannot be split by type. (Sec. 3, p.3; not quantified)
- Expenses under a threshold are reported as a checkbox with no amount, and client income over the threshold is rounded to the nearest $10,000. (Sec. 3, pp.3-4; expenses under $5,000 (checkbox); client income over $5,000 rounded to nearest $10,000)
- The authors conclude the public LDA system remains opaque despite searchable data and downloadable XML, and that analysis of the currency values is limited until the forms change. (Sec. 3, p.4; Sec. 4, p.4; not quantified)

## Limitations

- Reported dollars cannot be split into lobbying expenditures versus client income (Sec. 3, p.3).
- Expenses under $5,000 are a checkbox with no amount, and client income above $5,000 is rounded to the nearest $10,000 (Sec. 3, pp.3-4).
- Pediatric-related dollars cannot be isolated because related issues are bundled with unrelated issues in the same filing (Sec. 3, p.3).
- Analysis covers one quarter (Q4 2009) and one search term; the paper states the term was chosen for sensitivity and specificity but does not test other terms (Sec. 2.4).
- Records are grouped by filing date rather than activity date, so the sample mixes activity years (Sec. 2.3, p.2).
- The paper performs no statistical analysis; the authors say deeper statistical manipulation of currency values could be assessed only after the forms change (Sec. 4, p.4).
- Text-level analysis (NLP, topic modeling) is proposed for future work but not done (Sec. 4, p.4).
- Total number of filings in the quarter and the denominator for the 35 hits are not reported.

## Short quotes

- "XML files do not differentiate lobbying expenditures from income" — Sec. 3, p.3 (Analysis)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-lobbying#LDA finance-client classification and report join\|LDA finance-client classification and report join]] | qualifies | The LDA XML amount fields do not separate lobbying expenditures from income, and no identifier lets a researcher split them. Expenses under $5,000 are a checkbox with no amount, and client income over $5,000 is rounded to the nearest $10,000. (Sec. 3, pp.3-4) | The LobbyView collector parses is_self_filer per report (line 847) and parses a blank amount as 0.0 (line 844). Its per-side, per-year amount_sum (905) and finance_amount_sum (909), and its top-spender total_amount (932), add every report's amount whether self-filer expense or hired-firm income; self-filer dollars are kept only as a sub-sum (912). The auto-memory note lobbyview-integration-2026-07-31.md (not the brain) records 'ABA $228M vs CUNA $93M' and 'self-filer = expense, hired firm = gross income'. The claim that an in-house expense figure already includes payments to outside firms comes from LDA guidance as cited in commit 914623152, not from this paper. `fec/lobbying/collect_lobbyview.py:20-22, fec/lobbying/collect_lobbyview.py:844, fec/lobbying/collect_lobbyview.py:847, fec/lobbying/collect_lobbyview.py:905-912, fec/lobbying/collect_lobbyview.py:932; contrast fec/lobbying/sector_spend.py:4-7, fec/lobbying/sector_spend.py:367-377` | The hazard carries over: the LD-2 amount semantics the paper describes still apply. The paper's claim that the two series cannot be separated does not carry over to LobbyView, which exposes is_self_filer; the collector could split them and does not in these aggregates. The LobbyView window and the 2024-10-25 vintage were not re-checked here. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#LDA finance-client classification and report join\|LDA finance-client classification and report join]] | supports | Pediatric items sit in single filings alongside unrelated issues (Medicare Part B vaccine coverage, Medicaid pediatric vaccines, FDA biologics, pandemic preparedness), so dollars spent on pediatric lobbying cannot be isolated. The paper nonetheless reports dollar sums by umbrella code from a Filing x Issue merge: Taxation/Internal Revenue Code $527,147,333 and Health Issues $436,651,121 for Q4 2009, with no filing-count denominator. (Sec. 2.3 (merge), Sec. 3 p.3 (totals and bundling)) | For finance-joined reports the collector increments a per-code report count (per_code[code] += 1) and does not spread or repeat dollars across codes. The auto-memory receipt '322,473 finance-coded reports' is a report count. `fec/lobbying/collect_lobbyview.py:913-918` | Applies to any LDA source where one report carries several issue codes. That the paper's per-code totals count a multi-code filing under each code is an inference from the merge description, not stated by the authors; treat the totals as non-benchmarks either way. | moderate | VERIFIED |
| [[systems/mentat-giving-forecast#Calculator rules, company-PAC nowcast and BLEND\|Calculator rules, company-PAC nowcast and BLEND]] | supports | The XML amounts do not differentiate lobbying expenditures from income, and greater statistical analysis of the currency values is limited until the forms and procedures used to collect lobbying data are adjusted. (Sec. 3, p.3; Sec. 4, p.4) | Mentat's giving_forecast declares LOBBY_SERIES as income_total (outside-firm fees) and expenses_total (in-house expenses), and _lda_history maps hired_firms = income_total and in_house = expenses_total, never added. sector_spend.py declares FORBIDDEN_COMBINED_KEYS (combined_total, income_plus_expenses, lobbying_spend_total, spend_total, total_spend). The removal of the summed telecom 2025 $139.4M (now $102.4M in-house, $37.0M outside-firm fees) was Melange commit 914623152 on the industry page (make_lobby_network_html.py and industry-ui), not a change to Mentat's card. `C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py:379, C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py:553-562, fec/lobbying/sector_spend.py:4-7, fec/lobbying/sector_spend.py:367-377` | The paper describes the federal LD-2 form, not an empirical estimate, so it carries over to 2021-2026 Senate LDA data. It supports the D-064 rule; it does not measure the overlap. | strong | VERIFIED-SOFTENED |
| [[systems/mentat-giving-forecast#Calculator rules, company-PAC nowcast and BLEND\|Calculator rules, company-PAC nowcast and BLEND]] | supports | LDA records are grouped by time of filing, not time of activity: Q4 2009 files include records from 2002-2008 filed after the fact, and the authors add a filter to keep 2009 events only. The paper describes no amendment handling. (Sec. 2.3, p.2) | sector_spend groups filings by (registrant, client, filing_year, filing_period) and keeps the latest parseable dt_posted per group, with filing_uuid breaking ties, so an amendment supersedes the original. make_lobby_network_html.py holds the LD-2 deadlines (Apr 20, Jul 20, Oct 20, Jan 20) used for the quarter-coverage rule (D-130). `fec/lobbying/sector_spend.py:793-831, fec/surfaces/make_lobby_network_html.py:30-31` | Carries over: same federal LDA filing structure. The system is stricter than the paper, which only year-filters and does not deduplicate amendments. | moderate | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Commit 914623152 (which removed the summed $139.4M telecom figure) is a Melange commit to the industry page (make_lobby_network_html.py, industry-ui), not a change to Mentat's lobbying card; the summary attributes it to Mentat and sector_spend.py together.
> - Summary issue: The 'ABA $228M vs CUNA $93M' figure is in Paul's auto-memory note lobbyview-integration-2026-07-31.md, not in the brain; the summary calls it the brain's figure.
> - Summary issue: Rounding: the paper says client income over $5,000 is rounded to the nearest $10,000; the summary drops the over-$5,000 condition.
> - Summary issue: 'the bundling trap that inflates the paper's own per-code dollar totals' is stated as fact; the paper does not say its totals repeat a filing across codes, so this is an inference and should be marked as such.
> - Summary issue: The mapping expenditures = in-house and income = outside-firm fees is the analyst's gloss; the paper says only 'expenditures' and 'income'.

## Gaps and flags

- The LobbyView collector's spend aggregates (amount_sum, finance_amount_sum, top-spender total_amount) add self-filer expenses and hired-firm income. This contradicts the D-064 rule that sector_spend.py and Mentat now enforce. The ABA $228M vs CUNA $93M comparison in Paul's auto-memory note may be a mixed figure. It should be re-derived as two separate series before it is quoted again.
- Amounts reported only as the under-$5,000 checkbox are parsed as 0.0 in the LobbyView ingest, and income above $5,000 is rounded to the nearest $10,000. No receipt counts how many finance-side reports are affected. Small credit-union filers such as state leagues would be understated relative to large bank filers.
- Neither Melange nor Mentat uses the Specific Issues free text. The paper proposes NLP and topic modeling on it as future work, and Kim et al. 2025 do the position-extraction version. The system's issue view is limited to issue codes and report counts.
- Do not quote the paper's per-code dollar totals (Taxation $527.1M, Health $436.7M) as benchmarks. They come from a Filing x Issue merge that probably counts a multi-issue filing's dollars under every code, they mix expenditure and income, and the paper gives no filing-count denominator.
- Paul's positions list cites Harrison for mentat-fec-attribution.dedup-and-versions ('late-filed records'). The paper is about LDA filing-time vs activity-time grouping and says nothing about FEC amendment or memo versions, so the citation does not carry over to that component. The LDA analogue is already handled in sector_spend.py (activity-period assignment plus latest-dt_posted supersession).
- Paul's positions list also cites Harrison for melange-legislative-data.congress-activity-score. The paper does not touch congress.gov bill data, so it has no bearing on that component.
- The paper uses LDA dollars only as spend totals and builds no edges, so it raises no D-021 conflict. Any future reuse must keep LDA as an identity source, not an edge or weight source.

The evidence agrees with Paul's takeaway: the paper is an introductory walkthrough of an LDA pipeline (pandas, where to get the records, how to turn them into usable information), and its results are thin, with one quarter, one keyword and no statistics. His note that Kim's LobbyView is far larger also holds, and the system uses LobbyView as its historical source. His note misses where the paper's real value lies. Its stated limits (expenditures and income not separated, the under-$5,000 checkbox, $10,000 rounding, issue bundling, filing-time grouping) are the hazards the system has since ruled on: D-064 says never add the two series, D-130 sets the deadline-based quarter coverage rule, and the Melange 10-09 commit (industry page) removed the summed telecom figure. In that sense his note is out of date. It treats the paper as a how-to; the system now follows its warnings in Mentat and sector_spend.py but not yet in the LobbyView collector's spend aggregates.

## Related papers

- [[kim-2018-lobbyview-firm-level]] — complements — LobbyView cleans and links the same LDA reports at full scale, the database Paul says supersedes Harrison. The is_self_filer field that allows the expense-vs-income split is in the LobbyView data the collector reads; it is not described in the LobbyView paper extract.
- [[kim-2025-measuring-interest-group]] — extends — It does the text analysis Harrison proposes as future work (Sec. 4), labelling bill positions from LDA report text for 2009-2022.
- [[bacik-2025-measuring-dynamical-evolution]] — same-data — Both use LDA LD-2 filings; Bacik builds yearly multilayer networks over 1999-2023 from LobbyView, which Harrison's one-quarter parse never reaches.
- [[furnas-nd-conflating-lobbying-pacs]] — same-data — Furnas uses CRP-curated LDA lobbying expenditures, so the expense-vs-income semantics Harrison flags bear on its lobbying totals.
- [[goff-nd-cross-industry-finance]] — same-data — Paul's report draws on LDA disclosures, so the same caution about mixing the two series and about rounding applies.

### Linked from
_None._