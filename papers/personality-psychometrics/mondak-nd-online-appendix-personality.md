---
type: paper
title: "Personality on the Hill: Online Appendix (Tables A1-A2)"
authors:
  - "Mondak"
year: ""
venue: "Online appendix to an article in a political science journal (venue not shown)"
doi: ""
category: "Personality & Psychometrics"
systems:
  - "melange-psychometrics"
components:
  - "melange-psychometrics.gold-panel-validation"
  - "melange-psychometrics.lexical-big5"
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/mondak-nd-online-appendix-personality.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Downloads\\online_appendix.pdf"
sha12:
  - "f0024efe0e39"
tags: [paper, personality-psychometrics, type/paper, theme/personality-psychometrics, evidence/verified]
---

# Personality on the Hill: Online Appendix (Tables A1-A2)

**Mondak ().** *Online appendix to an article in a political science journal (venue not shown).* Theme: [[themes/personality-psychometrics|Personality & Psychometrics]].

> [!abstract] Summary
> This is the online appendix to *Personality on the Hill*, not the article. It has two tables of mean Big Five ratings of U.S. senators. Table A1 has about 129 rows from Senate insiders and APSA Congressional Fellows and feeds the main paper's Tables 2-5. Table A2 has about 126 rows from academics and is used only in the main paper's Table 1. Each row gives a rating count. The front note adds two facts that matter for anyone using the data. First, senators rated by only one coder appear in the tables but are excluded from every analysis in the main paper. Second, partisan composition is lopsided at the extremes. Democrats are 62% of the insiders' highest scorers and 65% of their lowest. In the academic table, 80% of high scores go to Democrats and 80% of low scores to Republicans. The appendix states no scale anchors, aggregation rule or era.
> 
> For Paul's system this is not background reading: it is the frozen external gold. `build_big5_senate_gold.py` turns the PDF into `big5_senate_expert_gold.json` (129 insider rows, 125 academic rows), and the trait validators score against it. So the appendix bears directly on how that gold is used:
> - **Rating count.** The main validators keep single-coder rows. The paper drops them. Only some side probes stratify or weight by count.
> - **Panel averaging.** The multitrait validator averages the two panels into one target, although the authors treat the academic panel as secondary.
> - **Party.** Neither primary validator partials out party, while the academic panel's extremes are 80/80 party-sorted. The recorded pattern that O passes only against academics may relate to this; that link is a hypothesis (DE-062), not a finding.
> - **Extraction.** The academic count of 125 against roughly 126 text rows, plus a malformed '4..33' cell, shows the extractor silently skips that row (the academic Feingold row is absent from the artifact).

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Not stated in this appendix. It only describes its two supplemental tables.

**Theory.** Not stated in this appendix.

**Data.** Unit: U.S. senator. Table A1 has 129 senator rows of mean Big Five ratings from Senate insiders and APSA Congressional Fellows (row count made from the text); these feed Tables 2-5 of the main article. Table A2 has 126 senator rows from academic experts (row count made from the text); used only in the second section of Table 1 of the main article. Each row gives the number of ratings. Time period and the analytic N in the main paper are not stated.

**Method.** Expert coding: multiple raters score each senator on five traits (openness to experience, conscientiousness, extraversion, agreeableness, emotional stability). Decimal values (e.g., 4.67, 4.17) suggest averaging across raters, but the aggregation rule is not stated. Raters sometimes used 0.50 increments, retained as coded. Observed values range from 1.00 to 7.00; the scale anchors are not stated.

**Identification.** Not stated in this appendix.

**Scope.** Current-era U.S. Senate members appearing in the two tables (names span multiple Congresses); the appendix does not state the population or era explicitly.

## Key findings

- Democrats hold 62% of senators receiving the highest personality scores in the main article's Table 3, and 65% of the lowest-scoring senators. (62% high; 65% low (Democrats); Online appendix p.1, lines 11-14 (refers to main-article Table 3))
- Using academic ratings (Table A2 in the text, labelled Table A3 in the front note), 80% of the high scores go to Democrats (Angus King counted as a Democrat) and 80% of the low scores go to Republicans. (80% high Democrats; 80% low Republicans; Online appendix p.1, lines 15-17)

## Limitations

- Senators with data from only a single coder are excluded from all analyses in the main paper, but single-coder rows still appear in the tables.
- Table A2 data are not used in the main paper except for Table 1.
- Bold and italic designations for top and bottom three scores are only for senators with at least two coders; they are not visible in the plain text.
- Front note cites Table A3 for the academic comparison, but the academic table is labelled Table A2.
- Some cells are blank or malformed (e.g., Baldwin, Heitkamp, Ensign, Capito and Feingold rows; '4..33' in Table A2), and there are a few inconsistent values such as 4.0 vs 4.00 formatting.
- Aggregation method, scale anchors, sample period and main-analysis N are not stated.

## Short quotes

- "62% of senators receiving the highest personality scores are Democrats" — Online appendix p.1, lines 13-14
- "Senators with data from only a single coder are excluded from all analyses reported in the main paper." — Table A1 note, appendix p.4
- "Data in this table are from academics. Except for Table 1, these data are not used in the main paper." — Table A2 note, appendix p.5 onward

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-psychometrics#Big Five validation against Senate gold panels and criteria\|Big Five validation against Senate gold panels and criteria]] | measurement-source | Table A1 gives Senate-insider and APSA-Fellow mean Big Five ratings for 129 senators, feeding main-paper Tables 2-5. Table A2 gives academic ratings for 126 senator name entries (text count), used only in main-paper Table 1. Each row carries a rating count. Observed values run 1.00-7.00 with 0.50 increments noted by the authors. (Appendix p.1 front note; Table A1 (pp.2-4) and note; Table A2 (pp.5-7) and note) | build_big5_senate_gold.py writes a SHA-256-pinned frozen artifact (data/stance/big5_senate_expert_gold.json) keeping panel, source name, page and ratings_n. EXPECTED_COUNTS is 129 insiders and 125 academics (line 20); the 1-7 range check is at line 62. The artifact has 129 insider and 125 academic records. The academic count is one short of the 126 text entries: the academic Feingold row (malformed cell 4..33, text line 1297) is absent; Feingold appears only as an insider with ratings_n 2. The except branch at lines 56-61 drops that row silently, and its comment that only headings reach it is wrong. The usage contract at lines 85-90 is as stated. Both validators score per panel via map_gold. `fec/members/build_big5_senate_gold.py:20,46-49,56-67,75-90; fec/members/b5_external_gold_validate.py:86-102,114-118` | Senate-only and historic; era unstated. Validates rank order on overlap with current members only, as the code caveat at b5_external_gold_validate.py:209 says. Ratings are as coded by the raters, not self-report. | strong | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Big Five validation against Senate gold panels and criteria\|Big Five validation against Senate gold panels and criteria]] | qualifies | Senators rated by only one coder are excluded from all analyses in the main paper but still appear in both tables. Bold and italic top and bottom designations apply only to senators with at least two coders. (Table A1 note (appendix p.4, text lines 950-953); Table A2 note (text lines 1864-1869)) | The primary validators do not filter on ratings_n. averaged_gold (b5_gold_multitrait_validate.py:83-97) averages insider and academic values with no count weight. oof_variant (126-134) keeps any row with a non-null trait. row_correlations (b5_external_gold_validate.py:114-118) filters only on null traits. Side probes respect the rule: b5_gold_criterion_probes.py:58-68 stratifies insider k=1 vs k>=2, and b5_o_weighted_look.py:399 weights by sqrt(ratings_n) with a k>=2 subset at line 417. `fec/members/b5_gold_multitrait_validate.py:83-97,126-134; fec/members/b5_external_gold_validate.py:114-118; fec/members/b5_gold_criterion_probes.py:58-68; fec/members/b5_o_weighted_look.py:399,417` | Same data and same inclusion rule. The size of the effect depends on how many current-roster matches are k=1 insiders, which was not counted here. Any k>=2 re-run is a new look under single-look discipline, not a free re-score. | strong | VERIFIED |
| [[systems/melange-psychometrics#Big Five validation against Senate gold panels and criteria\|Big Five validation against Senate gold panels and criteria]] | qualifies | The extremes of both panels are party-skewed. Democrats are 62% of the insider highest scorers and 65% of the lowest (main-paper Table 3, as cited). In the academic table 80% of high scores go to Democrats (Angus King counted as Democrat) and 80% of low scores to Republicans. The note calls the academic table A3; it is labelled A2. (Appendix p.1 front note, text lines 11-17) | The panel pass/miss pattern matches the artifact big5_gold_anchored.json: E and A pass both panels; O passes academics only (insider one-sided permutation p 0.104); N passes insiders only (academic p 0.195). Neither primary validator controls for party: b5_gold_multitrait_validate.py and b5_external_gold_validate.py contain no party term. The ONC look partials out female and republican covariates (b5_onc_dual_panel_look.py:121-164, covariates at 151-153, loader at 188). The claim that lexical O was reversed on a party check is not supported: the ONC candidate file marks O NOT_LOOKED_CRITERION_STOP_BAR_MISS, and no such result was found in the brain notes. `fec/members/b5_gold_multitrait_validate.py:137-169; fec/members/b5_onc_dual_panel_look.py:121-164,188; data/stance/big5_gold_anchored.json; data/members/big5_onc_crosspanel_successor.json.candidate` | Party is a known fault line here. Panel-level correlations should be quoted with party strata or an UNSTRATIFIED label. The suggestion that O passes only against academics because of party is new agent analysis and stays UNVERIFIED (DE-062). The 62/65 and 80/80 figures describe only the extremes. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | measurement-source | Insider, APSA Fellow and academic raters supply Big Five ratings of senators' public persona, usable as an external criterion for text-derived traits. The appendix calls these Big Five measures built from insider and academic data. It does not call them independent or observer ratings. (Appendix p.1 front note; Tables A1-A2) | The panels serve as external gold in b5_external_gold_validate.py and b5_gold_multitrait_validate.py. Full-sample gold-fitted weights are emitted after the OOF verdict (b5_gold_multitrait_validate.py:4-7, 136, 165). b5_profile_basis.py loads the gold artifact at line 128 and passes selected_weights into member_basis at lines 36-44, so for shipped composites the panels are no longer untouched; only the OOF figures are clean. The cited code does not refit the lexical instrument to RKH targets. The v2 lexical builder (big5_lexcal_v2.py docstring, lines 3-12) learns weights against behavioral criteria and describes the RKH and Mairesse weights as v1 priors. `fec/members/b5_gold_multitrait_validate.py:4-7,136,165; fec/members/b5_profile_basis.py:36-44,128-140; fec/members/big5_lexcal_v2.py:3-12` | Applies to senators who overlap the appendix and to text from the same public-persona era. Does not carry to House members or to genres (floor, interview, social) the raters never saw. Ratings describe perceived public persona, not self-report. | moderate | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Value range wrong: the appendix minimum is 1.00 (e.g. the DeMint academic-table row region), not 1.50; maximum 7.00 is correct.
> - Summary issue: Academic row count: text has 126 senator name entries (insider 129 verified); the artifact has 125. Confirmed, not just agent-counted: the academic Feingold row with the malformed 4..33 cell (text line 1297) is absent from the artifact. The comment at build_big5_senate_gold.py:60 that only headings reach the except branch is wrong.
> - Summary issue: Claim that lexical O was reversed on the party check is unsupported by any artifact found (ONC candidate marks O NOT_LOOKED); remove.
> - Summary issue: Claim that the lexical Big Five is refit to RKH targets is not supported by the cited code; big5_lexcal_v2.py learns weights against behavioral criteria.
> - Summary issue: Paul alignment unverified: no mention of Mondak or Personality on the Hill found in brain notes beyond the gold-anchor entries, so the 'on disk only' listing and the stale-notes claim cannot be confirmed. Paul highlighting Birkenmaier codebook, Krippendorff alpha and macro-F1 is also unverified.
> - Summary issue: The gold-anchor pass/miss record (E and A both panels; O academic-only; N insider-only) is correctly reported and matches big5_gold_anchored.json.
> - Summary issue: Coverage: text file read via searches and counts; the PDF and page images were not available. All 129 insider and 126 academic name entries were counted from the text file. All four bearings and four related links were checked. paul_markup empty (no markup file). key_findings not present in the rel output, so none verified. Brain notes and research item files read only.

## Gaps and flags

- The primary gold validators do not apply the authors' exclusion of single-coder senators. A k>=2 filter, or weighting by ratings_n, exists only in side probes and the O-weighted look.
- Neither primary validator controls for party, although both panels' extremes are party-skewed (academic 80% D high / 80% R low). The panel pass/miss verdicts are effectively UNSTRATIFIED.
- b5_gold_multitrait_validate averages insider and academic values into one target. The authors use the academic panel only for their Table 1, so averaging imports its stronger partisan sorting into the target.
- Possible extraction loss. EXPECTED_COUNTS is 125 for academics, but the text shows about 126 rows, and Table A2 has a malformed '4..33' cell. The parse-failure branch at build_big5_senate_gold.py:56-61 silently skips rows, and its comment claims only headings reach it. The row-count difference (126 text entries vs 125 in the artifact) is confirmed: the academic Feingold row is absent from the artifact.
- Scale anchors and the aggregation rule are not in the appendix. The code asserts a 1-7 panel mean (build_big5_senate_gold.py:62,82) as an inference.
- The panel era is unstated. Current-roster overlap measures only rank agreement for senators who served in the appendix era.
- Rule 'pooled coefficient over a known fault line': the panel-level E/A/O/N verdicts pool across party, and the appendix documents strong party skew at the extremes. Quote them with party strata or an UNSTRATIFIED label.
- DE-062: the hypothesis that O passing only against academics is a party artefact is new agent analysis (a hypothesis, not a finding).
- Gold-independence: once full-sample gold-fitted weights ship (b5_gold_multitrait_validate.py:4-7, consumed by b5_profile_basis.py:128), the appendix panels stop being an independent criterion for those composites. Only the OOF figures count as validation.
- Any re-scoring with a k>=2 filter or a party partial is a new confirmatory look under single-look discipline and is not a free re-run.

The appendix has been the frozen external gold since the July psychometrics lanes (big5_senate_expert_gold.json). It adds two cautions for using the panels as a criterion: the authors drop single-coder senators, and both panels, especially the academics, are party-sorted at the extremes. Both cautions temper reading the panel 'passes' as clean validity.

## Related papers

- [[birkenmaier-2025-measuring-politicians-public]] — complements — Item exists. Birkenmaier and Lechner 2025 is a text-based agency and communion measure; brain notes give prompting macro-F1 0.74-0.77. Paul highlighting codebook, Krippendorff alpha and macro-F1 is not found in the brain notes, so that part is unverified. Supplies a validation approach, not this appendix's criterion.
- [[ramey-2016-measuring-elite-personality]] — complements — Item exists; ext describes Big Five measurement from floor speech text for US legislators, so the complement relation holds. Calling it the template for the lexical Big Five in Melange is not verified here.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — Item exists; ext describes Big Five and LIWC relations across 52 categories. Using it as a realistic convergence ceiling for these panels is the analyst's inference, not the paper's claim.
- [[lechner-2025-mapping-political-personality]] — complements — Item exists; ext describes ideal and actual politician personality profiles, which supports the complement relation to observer-rated public-persona tables.

### Linked from
- [[byrne-2014-chokes-under-pressure]] — complements — Expert ratings of US senators' traits, including N. Could test whether the system's N-slot measure tracks neuroticism, which Byrne's mechanism needs before transfer.
- [[celli-nd-twenty-years-personality]] — complements — Existence confirmed. The survey does not cite it. The panel role is a Melange-side claim.
- [[amsalem-2018-effect-politicians-personality]] — complements — Item title confirms expert evaluations of US senators' traits. These are an alternative to the low-reliability TIPI self-report (agreeableness alpha .15) that Amsalem depends on.
- [[rubio-2024-feasibility-data-analytics]] — complements — Real item (title confirmed: expert evaluations of senators' traits). Analogous to the zero-acquaintance expert criterion; the paper itself does not mention it.
- [[soto-2017-next-five-inventory]] — complements — Expert ratings of U.S. senators' traits, the kind of other-rater criterion whose convergence the BFI-2 self-peer figures help calibrate. Title confirmed.
- [[mairesse-2007-automatic-recognition-personality]] — complements — Online appendix to expert evaluations of senators. The link to this paper is conceptual (observer or expert ratings); the appendix does not mention Mairesse 2007.
- [[cao-2024-language-models-personality]] — complements — Expert evaluations of senators' traits (Personality on the Hill) are a perception measure of public figures, the construct Cao and Kosinski predict from names, so reputation and halo can enter both. Cao does not cite it; the link is the analyst's.
- [[peters-2024-language-models-infer]] — complements — The appendix gives the Senate insider and academic observer panels used as the system criterion. Its text-count of academic rows (about 126) differs from the 125 in the gold-set note.