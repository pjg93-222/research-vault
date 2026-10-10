---
type: paper
title: "The Dynamics of Firm Lobbying"
authors: ["William R. Kerr", "William F. Lincoln", "Prachi Mishra"]
year: 2011
venue: "NBER Working Paper 17577 (November 2011); JEL D72 D73 D78 F22 F23 J61 O31 O38"
doi: ""
category: "Lobbying"
systems: ["melange-lobbying", "mentat-giving-forecast", "mentat-sector-growth-study"]
components: ["mentat-giving-forecast.cycle-model", "mentat-giving-forecast.refit-gates", "mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.growth-spending-regressions", "melange-lobbying.contested-bills"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/kerr-2014-dynamics-firm-lobbying.pdf]]"
original_paths: []
sha12: ["95789b5d8869"]
tags: [paper, lobbying]
---

# The Dynamics of Firm Lobbying

**William R. Kerr, William F. Lincoln, Prachi Mishra (2011).** *NBER Working Paper 17577 (November 2011); JEL D72 D73 D78 F22 F23 J61 O31 O38.* Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Kerr, Lincoln and Mishra (NBER WP 17577, Nov 2011; the AEJ: Economic Policy version is dated 2014) study why firm lobbying is so persistent. In a balanced Compustat panel of 3,260 US-headquartered public firms, 1998-2006, only 10 percent lobby in any year and 6.2 percent in a given year. Among firms that lobbied last year, 92 percent lobby again. In a typical year, 96 percent of lobbying dollars come from firms that lobbied the year before. The authors read this as evidence of sunk entry costs in a Roberts-Tybout model. A Blundell-Bond linear probability model puts the lagged-status effect at about 0.84-0.88. Within-firm fixed-effects OLS gives about 0.44, which the authors say is biased downward (Nickell). Entry clusters in the year before an election, not the election year itself, and exit does not follow the cycle. The policy test uses the 2004 lapse of the expanded H-1B cap. They interact each firm's 2001 dependence on foreign workers with a 2004-2006 dummy, with firm and year fixed effects, in 171 large patenting firms (846 core firm-years). Dependence raises lobbying on high-skill immigration (0.043, SE 0.014, LCA measure). It does not raise the chance of lobbying at all (-0.008, SE 0.009). No highly dependent non-lobbyist entered. All of the response came from firms that were already lobbying (the intensive margin). For Paul's systems this is mainly evidence about forecasting and specification. It backs Mentat's persistence-anchored lobbying forecasts, which leave out election-phase effects. It warns about the lagged-outcome lobbying equation with sector fixed effects in the sector growth study. It also shows that LDA reports cannot measure spending per issue, which is what the contested-bills question would need. The firm-level, 1998-2006 public-firm scope limits how far any of this transfers.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** What determines the dynamics of firm lobbying over time? Do up-front entry costs explain the persistence of lobbying status, and how do firms respond to a policy shock (the 2004 expiry of the H-1B cap expansion) at the extensive versus intensive margin?

**Theory:** Dynamic entry-cost model in the tradition of Roberts and Tybout (1997): a one-time sunk cost F0 to start lobbying and re-entry costs Fj after j periods without lobbying. Continuing creates an option value, so lobbying is state dependent even after controlling for profitability (Sec 3, pp.8-10). Lobbying is a legislative subsidy and signal to politicians (Deardorff and Hall 2006), per the introduction.

**Data:** Unit: firm-year. (1) Compustat panel of US-headquartered publicly traded firms, 1998-2006, balanced: 3,260 firms and 29,340 observations (Table 1; regressions use 26,080 obs per Table 2 note). Lobbying from LDA semi-annual reports via Center for Responsive Politics and Senate SOPR. Industry imports from Feenstra, Romalis, Schott (UC Davis). (2) H-1B sample: 171 major firms, 2001-2006, 1,026 obs, core 846 obs (Tables 3-4). Department of Labor LCA records (1.8 million records 2001-2006); USPTO patents and inventors with ethnicity inferred from names.

**Method:** Reduced-form linear probability model of the lobbying indicator with lagged lobbying status and up to two lagged re-entry indicators (eq 2, p.10). Table 2: Blundell-Bond GMM (cols 2-6, lag-two instruments), OLS with state, industry and year FE (col 1), within firm-FE OLS (cols 7-8, Nickell bias acknowledged). Section 5: eq 3 interacts 2001 firm dependence (log LCA count, log Chinese and Indian patenting) with a 2004-2006 binding-cap dummy, with firm and year FE, clustered by firm. Table 4: cross-section, year FE. Table 6 uses always-lobbying firms (443 obs). Table 7 tabulates by quintile.

**Identification:** Persistence: state dependence in lagged lobbying status identified from within-firm variation under Blundell-Bond GMM with firm effects; authors argue lagged status proxies entry costs. Policy: the 2004 expiry of the expanded H-1B cap is treated as an exogenous natural experiment, since the expiry date was fixed years earlier and the issue was not central to firms during 2001-2003 (p.13). Test is the interaction of 2001 dependence with the post-2004 dummy plus firm and year FE. Entry (extensive margin) versus adjustment (intensive margin) comparisons in Tables 5-7.

**Scope:** US publicly traded, US-headquartered firms in Compustat for lobbying-participation facts (1998-2006). H-1B analysis covers 171 large patenting firms present in Compustat in all of 2001-2006 and at least 0.05 percent of US domestic patents. Era of the 65,000 cap versus the 195,000 cap (1998-2004). Excludes private firms and individual lobbyists.

## Key findings

- Few firms lobby: 10 percent of 3,260 firms lobby in at least one year 1998-2006; 6.2 percent lobby in a given year (Table 1; intro p.2).
- Persistence: P(lobby this year | lobbied last year) = 92 percent, above 80 percent in almost all two-digit NAICS industries (Sec 2, p.7).
- Size gradient: lobbying firms average 5,407 vs 1,423 ($m) sales and 23k vs 7k employees for non-lobbying firms (Table 1); among lobbyists, correlation with spending is 28 percent for sales and 19 percent for employment (p.7).
- Entry costs: lagged lobbying raises the probability of lobbying in the current year by about 88 percent in the baseline Blundell-Bond specification (text, p.10; Table 2 values 0.84-0.88 across cols 2-6). Within-FE and some other OLS values are smaller (recovered 0.44 and 0.34), but still significant per the text (p.12).
- Concentration: in a typical year 96 percent of lobbying expenditure came from firms that lobbied the previous year (p.7).
- Election timing: entry clusters in the year before an election, not the election year; exit is unrelated to the cycle (p.7).
- Policy shock: share of firms lobbying for immigration rose from 6 to 12 percent between 2003 and 2004, tracking the ratio of H-1B issuances to the cap (Fig 5, p.16).
- Dependence response (Table 5): LCA-based dependence x binding cap = 0.043 (SE 0.014) for high-skill lobbying; patenting-based = 0.029 (SE 0.012). Text: 10 percent higher foreign-worker dependence raises high-skill lobbying probability by about 0.3 to 0.4 percent (p.17).
- No extensive-margin response: overall any-issue lobbying coefficients near zero (-0.008 SE 0.009; -0.019 SE 0.014, column mapping inferred). Panel E of Table 7 shows 0.00 entry for the most dependent firms that had not lobbied in 2001-2003 under both measures.
- Intensive margin (Table 6, 443 always-lobbying firm-years): above-median-spending firms show elasticity 0.042 (SE 0.021) LCA and 0.033 (SE 0.022) patenting; interaction terms have t-statistics under 0.5 (p.18).
- Table 7 Panel D: share of most-dependent firms lobbying for high-skill immigration in 2004-2006 is 0.48 (LCA) and 0.50 (patenting) among firms that lobbied in 2001-2003, vs 0.28 and 0.27 in Panel C for 2001-2003.
- Falsification: LCA and Chinese and Indian patenting show no significant link to lobbying on Clean Air and Water, Consumer Product Safety, or Retirement (pp.15-16).

## Limitations

- Sample is publicly traded firms in Compustat; private firms unobserved; conglomerates excluded from baseline (fn 16, p.10).
- Lobbying reporting threshold of 10,000 dollars per half-year; authors argue measurement error is minimal (p.6).
- Dynamic GMM with T=9 relies on lag-two instruments; Arellano-Bond results more sensitive across variants (p.12).
- Within FE likely biased downward (Nickell 1981); authors present the OLS results as bounds-style checks (pp.11-12).
- H-1B sample is small (171 firms, 846 core obs) and selected on Compustat coverage and patenting share (p.15); core sample is 846 not the 855 maximum (fn 25).
- Dependence measured only in 2001 (predetermined); LCA counts are a demand proxy; ethnicity inferred from names, not immigration status.
- Authors call the evidence suggestive in Sec 5 (p.16); topic-specific spending is not disclosed, so intensity is proxied by years listed (p.19).
- Abstracts from the in-house versus outside-lobbyist choice (fn 6) and from the mechanism by which lobbying raises profits (fn 14).
- Cap expiry assumed exogenous, though firms knew of it years in advance (p.13).

## Short quotes

> The probability that a firm lobbies in the current year given that it lobbied in the previous year is 92%.
> These results support the existence of significant barriers to entry in the lobbying process.
> All of the adjustments among these major patenting firms in response to the policy change were intensive margin adjustments.

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-giving-forecast#Cycle-aware giving refit (phase and drift log models, 50-50 growth)\|Cycle-aware giving refit (phase and drift log models, 50-50 growth)]] | supports | Participation persistence: P(lobby this year given lobbied last year) = 92 percent in a balanced 3,260-firm Compustat panel, 1998-2006 (6.2 percent lobby in a given year; 10.0 percent ever lobby). Above 80 percent in almost all two-digit NAICS industries. 96 percent of expenditures in a typical year come from prior-year lobbyists. Entry clusters in the year before an election, not the election year; exit is unrelated to the cycle. (Sec 2 p.7 (92 percent, 80 percent, 96 percent, entry and exit timing); Table 1 (6.2 and 10.0 percent)) | Mentat lobbying projection is the one quantity exempt from the midterm/presidential phase adjustment (Phase 6 owner rulings of 2026-10-01, README paragraph near line 152). Calculator rule: last year plus slope times predicted-growth change, with no year effect (rf_v2model.R line 132). `C:/Users/pgoff/Mentat/mentat/forecast/refit/README.md:152; C:/Users/pgoff/Mentat/mentat/forecast/refit/R/rf_v2model.R:132` | Evidence is participation for 1998-2006 public firms, not sector dollars. The 96 percent is one sample's expenditure share. Election timing in the paper concerns entry, a small share of dollars, so it does not show phase has zero effect on spend; that stays untested here. | moderate | VERIFIED |
| [[systems/mentat-giving-forecast#Refit gates G0-G9\|Refit gates G0-G9]] | supports | Lagged lobbying status raises current-year lobbying probability by 0.84 to 0.88 in Blundell-Bond columns 2-6 (baseline column 2 = 0.8848; column 6 = 0.8810), 26,080 firm-years, 1998-2006 public firms (Table 2 and note). Table 7 Panel E: 0.00 entry 2004-2006 among the most dependent firms not lobbying in 2001-2003, on both LCA and patenting measures. Entry is not zero elsewhere: 4th-quintile LCA non-lobbyists show 0.08, and Nike entered without prior lobbying (fn 26). (pp.10-12, Table 2 and note; pp.17-19, Table 7 Panel E, fn 26) | Mentat G9: log-lobbying refit MAE 4.050 vs naive 3.936 (README line 142; test-08-backtest.R line 61 pins 3.936); confirmed. Sector-study backtest triple (MAE 3.63 vs 3.99 with growth and 6.55 with control terms, LobbyView 2014-2023) was NOT found in any file under the sector_growth_study folder; unverified. Bands as split-conformal persistence errors, targets 2014-2023, horizons 1-3: confirmed in rf_lobbying.R persistence function. `C:/Users/pgoff/Mentat/mentat/forecast/refit/README.md:142; C:/Users/pgoff/Mentat/mentat/forecast/refit/R/rf_lobbying.R:87-95; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_forecast.R:4-8` | Persistence evidence is for binary participation, not dollars. The naive-anchor result for dollars holds only while incumbents dominate sector totals; entrant-heavy sectors (crypto before 2015, fintech) break that. The no-entry result holds only for the top dependency quintile, not all dependent firms. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | method-source | Short panel (T=9, 1998-2006). Within-firm FE OLS gives 0.44 (column 7; column 8 gives 0.34), which the authors expect to be biased downward following Nickell 1981. Blundell-Bond GMM with lag-two instruments gives 0.84-0.88 (columns 2-6). Arellano-Bond is more sensitive across variants; its point estimate is 0.52 (SE 0.18) in Appendix Table 3 column 2, which the analyst omitted. (pp.10-12; Table 2 columns 2-8; Appendix Table 3 column 2) | Brain sector-growth topic (line 269) transcribes the original contract equation (3) as log Lobby = alpha_i + rho times lagged log Lobby + growth terms + phase terms, with 5 sector clusters noted. The claim that implemented core L tests are levels with sector FE and no lagged outcome, and that only SEATS-W carries a lagged-outcome sensitivity, was not verified line by line in sgs_registry.R. `C:/Users/pgoff/Documents/brain/topics/2026-09-feature-sector-growth-study.md:269; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R:33-34,39` | The Nickell-bias order (1+rho)/(T-1) is a textbook approximation, not a Kerr result; applying it to Mentat is new agent analysis and stays UNVERIFIED (DE-062). The claim that GMM is not feasible with 5 clusters has no source in Kerr or the brain and was not verified. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | suggests-extension | Mean annual sales 5,407 ($m) for lobbying firms vs 1,423 for non-lobbying firms (Table 1; 327 lobbying firms of 3,260, 1998-2006). Policy response identified from 2001 dependence times a 2004-2006 binding-cap dummy with firm and year FE; high-skilled lobbying 0.043 (SE 0.014) per log LCA count (Table 5, 846 core obs). Falsification on unrelated issues is null (pp.15-16). (Table 1; eq (3) on p.16; Table 5 p.17; pp.15-16) | Growth adds no adjusted R2: 0.884717 with predicted growth vs 0.884690 without, on 77 rows (Codex review note in sgs_review.R line 237, also in brain). L3-LV growth coefficient flips from +0.352 to -0.477 with sector and year FE (brain). Both confirmed. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_review.R:237; C:/Users/pgoff/Documents/brain/topics/2026-09-feature-sector-growth-study.md:233-239; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R:30-34` | Kerr shows size predicts who lobbies, and that the 2004 response came from already-lobbying firms changing issues. Kerr does not show that yearly changes follow shared issue shocks, which the analyst text claims. A dated CU or bank policy design would be a new preregistered estimand, not a rescue of the growth tests. | weak | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Contested-bills registry\|Contested-bills registry]] | qualifies | LDA filings do not require an amount per specific topic (p.19). Issue intensity is proxied by the number of years a topic is listed and by keyword matches in specific-issue text (H-1B, L-1, PERM, high-skill bills), pp.5-6. (pp.5-6; p.19) | contested_bills.py allowed record keys are bill, congress, mode, counterparty_industries, active_from, active_to, ratified_by, ratified_date and note; there is no spend field. Engine wiring and the bank-vs-CU spend question were not checked. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/lobbying/contested_bills.py:28-44` | A relative-spend test per bill needs report dollars apportioned across issues and bills, which Kerr avoids; a filing or issue-mention count is the defensible proxy. LDA stays an identity and measurement source, never an edge or weight (D-021). | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary asserts the AEJ Economic Policy version is dated 2014. The analyst text is the NBER WP 17577 of November 2011 and the AEJ version is not in the file; unverified here.
> - Summary issue: Summary says dependence does not raise the chance of lobbying at all (-0.008, SE 0.009). That is a negative insignificant LCA any-issue estimate (Table 5 column 3); say no detectable effect.
> - Summary issue: Summary says no highly dependent non-lobbyist entered. True only for the most dependent quintile (0.00 on both measures, Table 7 Panel E). The 4th-quintile LCA non-lobbyists show 0.08 entry, and Nike entered in 2004 without prior lobbying (fn 26).
> - Summary issue: Summary says the paper backs Mentat persistence-anchored lobbying forecasts. Evidence is on firm participation, not sector dollars; say it is consistent with the design, with the transfer caveat.
> - Summary issue: Summary says LDA reports cannot measure spending per issue. The paper says only that amounts per specific topic are not required (p.19); reports do list totals.
> - Summary issue: ext key_findings 10 and the paper p.18 say the Table 6 interaction t-statistics are under 0.5. Table 6 LCA interaction is -0.013 (SE 0.020), t about -0.65; patenting 0.008 (SE 0.022), t about 0.36. The error is inherited from the paper text.
> - Summary issue: flags: the claim that Kerr dollar figures do not distinguish in-house from outside-firm spend cites fn 6, but fn 6 says the model abstracts from the in-house versus outside choice. Paper p.5 says filings report in-house and external totals separately; the flag is unsupported as written.
> - Summary issue: paul_alignment (not summary): Paul's cycle-model note says lobbying peaks in election years, and Paul holds that returns motivate deep pockets. Item has no Paul annotations (paul_entries empty); neither statement was found in the refit README or the brain topic files checked.
> - Summary issue: Sector-study backtest MAE values (3.63, 3.99, 6.55) cited in bearing 2 were not located in the sector_growth_study files; treat as unverified until a file reproduces them.

## Gaps and flags

- No extensive-margin view: sector totals over a fixed roster of companies cannot separate entry and exit from changes in incumbent spending. Kerr shows the two behave differently, and crypto and fintech entry would show up only as level jumps.
- The lobbying equation in the sector study's original contract (lagged outcome plus sector FE) has no stated treatment of Nickell bias. The implemented core drops the lag entirely, and neither choice is documented against the dynamic-panel problem.
- No literature backs the 2026-10-01 ruling that exempts lobbying from the election-phase adjustment. Kerr's pre-election-year entry result is a testable reason, but the odd-year entry effect is not measured in Mentat.
- The contested-bills question about relative spend has no measurement, because LDA does not disclose spend per issue or per bill.
- melange-lobbying.fec-lda-bridge: Kerr shows lagged lobbying status and firm size absorb most of the variation in participation. Any later bridge design would need to condition on both. The current product rule bars FEC as a lobbying predictor, so this is noted, not proposed.
- Year mismatch: the text file is NBER WP 17577 (Nov 2011). The slug and the year hint of 2014 refer to the AEJ: Economic Policy version, and coefficients may differ from the published tables.
- Kerr's dollar figures (mean 475k, median 164k per active firm) are CRP totals. They do not distinguish in-house from outside-firm spend (fn 6). Mentat never adds the two, and the sector config notes that undifferentiated sums double count self-filers' outside fees. Do not compare the magnitudes directly.
- The Nickell-bias reading of the sector-study lobbying equation is new agent analysis and is UNVERIFIED (DE-062).
- No conflict with the barred SNA proposals or with D-025. The paper uses LDA for firm identity and participation, not edges, which is consistent with D-021.

No annotations on this work. It is cited in the sector growth study, where Paul holds that returns motivate deep pockets. Kerr partly agrees: size predicts who lobbies. But here yearly changes follow persistence and issue shocks, which fits the null growth result better than a confident ROI reading. Paul's cycle-model note says lobbying syncs with elections and peaks in election years. Kerr qualifies that: entry peaks the year before and exit is acyclical. The built system already exempts lobbying from the phase adjustment (README:152), so his note is stale.

## Related papers

- [[bombardini-2020-empirical-models-lobbying]] — complements — Item title matches the Bombardini and Trebbi 2020 survey of empirical lobbying models. Kerr does not cite it; its coverage of fixed-cost and participation models is unverified from the item file.
- [[kim-2018-lobbyview-firm-level]] — complements — Corrected from same-data. Kerr uses CRP and SOPR LDA filings (pp.5-6), not the LobbyView database. LobbyView is the source of Mentat's sector-year series, so the link is by data family only.
- [[huneeus-2018-effects-firms-lobbying]] — complements — Corrected from builds-on. Kerr does not cite Huneeus and Kim. Shared fixed-cost and selection theme is an analyst judgment, not a documented lineage.
- [[alexander-2009-lobbying-analysis-under]] — complements — Item title covers lobbying ROI under the 2004 American Jobs Creation Act, a 2004 policy event; Kerr does not cite it. Returns view versus Kerr's participation view is consistent with the title.
- [[kim-2020-strategic-sequential-links]] — complements — Item title covers the timing of donations relative to lobbying, which bears on Kerr's pre-election entry result. Kerr does not cite it.
- [[bacik-2025-measuring-dynamical-evolution]] — complements — Item title covers change over time in the US lobbying network, a network-level counterpart to Kerr's firm-level persistence facts.
- [[defigueiredo-2013-advancing-empirical-research]] — complements — Item title is a review of empirical lobbying research (NBER WP 19698). Kerr does not cite it; the framing claim is unverified from the item file.
- [[bickmore-2025-corporate-lobbying-political]] — complements — Item title links corporate lobbying and PACs to firm size and market concentration, extending Kerr's size gradient. Kerr does not cite it.

### Linked from
- [[fremeth-2018-spillovers-regulating-corporate]] — same-data — Real item (title: The Dynamics of Firm Lobbying). Persistence motivation not re-read.