---
type: research-note
title: "Sector growth and political spending: source check (START HERE)"
author: Paul
year: "2026"
category: "Paul's Research Notes"
systems: ["melange-lobbying", "mentat-giving-forecast", "mentat-sector-growth-study"]
components: ["mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.growth-spending-regressions", "mentat-giving-forecast.cycle-model", "melange-lobbying.finance-client-classification"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/note-sector-growth-and-political-spending-source-source.md]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\Sector_Research_Source_Check_2026-09-25\\START_HERE.md"]
sha12: ["17c10df8e2db"]
tags: [research-note, paul-s-research-notes]
---

# Sector growth and political spending: source check (START HERE)

**Paul Goff (2026).** Theme: [[themes/paul-s-research-notes|Paul's Research Notes]].

> [!abstract] Summary
> Paul's September 25, 2026 source check is the data inventory and design note behind the Mentat sector growth study. At that point no regressions had been run. It lists IBISWorld real-revenue growth for four sectors: commercial banks, credit unions, life insurance and auto. It also has a 72-row sector-year panel for 2008-2025, election-year dummies and actual chamber control checked against the House and Senate histories. Expected control (conditional IEM measures) covers 2008-2022 only, and CPI multipliers convert spending to 2025 dollars. The note specifies six core regressions: spending on sector baseline plus growth (same-year, previous-year or predicted), presidential and midterm dummies, and expected Democratic House and Senate control, each run for contributions and lobbying. It also defines a Democratic share by chamber and a Chief / Middle Management / Employee tier split. The built system follows this design closely: sgs_registry.R carries the same three growth variables and the same four core controls, but the lobbying tests also add cur_dem_house and cur_dem_senate, which the note treats as optional. The IBISWorld lagged real-growth path also feeds the live giving forecast's 50/50 budget term. The note is now stale. The study has since run its regressions with a primary window of 2020-2025 (ruling R8; the sector count is not established, and the code describes seven study sectors plus community_banks). The study's own Codex review file (sgs_review.R) records reference values for LobbyView lobbying: adjusted R2 0.884717 with growth vs 0.884690 without, and a lobbying coefficient that flips from +0.352 to -0.477 under sector and year fixed effects, with Holm p moving from 0.0375 to 0.1125. The note's warnings still hold: keep blanks as missing, do not deflate IBISWorld twice, and treat expected-control means as concurrent, not causal.

## About this note

Belongs to the study: [[systems/melange-lobbying]], [[systems/mentat-giving-forecast]], [[systems/mentat-sector-growth-study]].

Original path: `C:\Users\pgoff\Downloads\Sector_Research_Source_Check_2026-09-25\START_HERE.md`

## Study

**Question.** Does industry revenue growth (same-year or previous-year) predict political spending (campaign contributions and lobbying) by sector, net of presidential and midterm election-year effects and expected congressional control; and does predicted industry growth do the same? Also whether Democratic share of contributions differs by chamber.

**Theory.** Not stated as a formal theory. The Mermaid flow (line 62-73) hypothesizes that industry revenue growth and congressional expectations, plus presidential and midterm timing, shape sector political spending.

**Data.** Four sectors: Commercial Banking (NAICS 52211), Credit Unions (52213), Life Insurance and Annuities (52411a), Automobile and Light-Duty Motor Vehicle Manufacturing (33611). Unit: sector x year. Panel period 2008-2025 (72 sector-year rows, 4 x 18). IBISWorld revenue: 78 rows including 2006-2007 lag years. Annual control table: 18 rows. Congressional control: verified 2008-2025. Expected control: 2008-2022 only. Contributions: verified receipts 2021-2026 only. Lobbying: bank and CU aggregates 1999-2023; cached 2021-2025 plus partial 2026 for bank, CU, crypto, fintech. No spending outcomes joined yet. Predicted growth: 55 IBISWorld sector-year values (53 pass checks) plus one crypto proxy.

**Method.** Four actual-growth OLS-style regressions (k=1 previous-year growth; k=0 same-year growth; each for contributions and lobbying): Spending(sector, year) = sector baseline + b x Growth(sector, year-k) + c x PresidentialYear + d x MidtermYear + e x ExpectedDemHouse + f x ExpectedDemSenate + error. Plus two predicted-growth tests using the prior-year forecast for year t in place of actual growth; six core tests total per the linked plan. Party direction: Democratic share = Dem / (Dem + Rep), separately for House and Senate recipients; missing when denominator is zero. Job-level comparison: sector x year x tier (Chief, Middle Management, Employee) for contributions only. No regressions have been run yet.

**Identification.** None claimed. Expectation controls are concurrent, not proof that expectations cause spending (line 77). Presidential and midterm dummies are meant to absorb the spending cycle.

**Scope.** U.S. federal politics; four sectors (commercial banks, credit unions, life insurance, auto manufacturing) with crypto only as a separate proxy; election cycles 2008-2025 for the panel; contribution data verified only from 2021 onward; organizational and PAC giving kept separate from individual donors.

## Key findings

- Panel is built as one row per sector per year over four sectors, with growth, election-year indicators, actual and expected chamber control and CPI multiplier, but no spending outcomes yet (72 rows = 4 sectors x 18 years, 2008-2025; Start with these files, bullet 1 (line 26))
- IBISWorld historical revenue captured for four sectors, with same-year and previous-year growth calculated (78 revenue rows; 55 sector-year values, 53 passing main checks, plus 1 crypto proxy (line 7); Readiness table row 1 (line 15); file list (line 27))
- No matching crypto IBISWorld revenue series was found (none; Readiness table row 1 (line 15))
- Actual House and Senate control verified against official chamber histories for every year (2008-2025; 18 annual rows (line 28); Senate 2021 Democratic; Readiness table row 3 (line 17))
- Expected (conditional) chamber control measures prepared only through 2022; 2023-2025 absent; IEM measures exclude the OTHER outcome (2008-2022 coverage; 2023-2025 missing; Readiness table row 4 (line 18))
- Lobbying series cached for bank, CU, crypto and fintech; life insurance and auto not among cached outputs (bank/CU aggregates 1999-2023; cached annual 2021-2025 plus partial 2026; Readiness table row 2 (line 16))
- Verified contribution receipts cover 2021-2026; no pre-2021 corpus verified in scoped local search (2021-2026 verified; Readiness table row 5 (line 19))
- Annual CPI prepared; spending converted to 2025 dollars with a multiplier of 321.9 divided by CPI for the year; not applied to IBISWorld revenue (CPI 2006-2025; multiplier 321.9 / CPI_year; Readiness table row 6 (line 20); Political spending formula (line 40))
- Forecast recheck adds supplemental observations; CU 2024 price basis resolved (12 additional supplemental observations; Recheck paragraph (line 9))
- Header claims a larger expanded scaffold, not reconciled with the four-sector body (nine sectors; 72 original reports; 86 strict annual growth inputs; 162-row forecast/control scaffold; Header paragraph (line 3))
- Six core regressions specified: four actual-growth timings (k=0 and k=1, for contributions and lobbying) plus two predicted-growth tests (6 tests; k=0 or k=1; 2 outcomes; Simple calculations and model (lines 42-56))
- Party-direction outcome defined as Democratic share of House or Senate contributions, missing when denominator is zero (Dem / (Dem + Rep); Simple calculations and model (line 58))

## Limitations

- No regressions run and spending outcomes not yet joined (line 5)
- No crypto IBISWorld revenue series found; crypto only as a separate proxy (line 15)
- 2006 revenue still needed for life insurance and auto 2008 lagged growth (line 15)
- No pre-2021 contribution corpus verified; absence not established elsewhere (line 19)
- Expected-control forecasts absent for 2023-2025; IEM measures are conditional and exclude OTHER (line 18)
- Employee tier includes unrecognized-title fallbacks, so not purely verified rank-and-file (line 60)
- Lobbying is organization-level with no donor-job-level split (line 60)
- Existing Mentat categories do not match the study sector set; P&C not a substitute for life insurance and fintech not for crypto (line 22)
- IBISWorld historical values may include estimates and later revisions (line 38)
- Expectation averages use dates before the targeted election, not the full year (line 77)
- Life insurance and auto lobbying series not among cached outputs (line 16)
- Header describes nine sectors while body describes four; unreconciled (line 3 vs lines 13-32)

## Short quotes

- "Keep blank values as missing. Do not turn absent forecasts, uncovered sectors or incomplete years into zero spending or zero probability." — Start with these files (line 32)
- "No regressions have been run, and spending outcomes have not yet been joined." — Opening paragraph (line 5)
- "Do not apply it again to IBISWorld revenue, which is already inflation-adjusted." — Readiness table row 6 (line 20)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | method-source | Six core tests: Spending(sector, year) = sector baseline + b x Growth(sector, year-k) + c PresidentialYear + d MidtermYear + e ExpectedDemHouse + f ExpectedDemSenate, with k = 0 or k = 1, plus two predicted-growth tests; each run for contributions and lobbying; 72 sector-years (4 sectors x 2008-2025); no regressions run yet. (Text lines 5, 26, 42-56 (opening paragraph; Simple calculations and model)) | sgs_registry.R encodes the same growth variables (growth_previous_year_pct, growth_same_year_pct, predicted_growth_pct), the same four core controls (pres_year, midterm_year, exp_dem_house, exp_dem_senate) and sector fixed effects. The lobbying specification adds cur_dem_house and cur_dem_senate (SGS_L_CONTROLS, line 34), which the note treats as optional rather than core. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R:31-36` | The built study is not the note's scope. The primary sector-year window is 2020-2025 with at least 6 distinct years (SGS_WINDOW_RULE, sgs_registry.R lines 17-18; ruling R8 per sgs_config.R line 34), with a fallback of 10 consecutive years inside 2008-2025 only if the primary fails. The LobbyView tests use the 2008-2025 fallback. The claim that same-year growth is outside a real forecast's information set is not in the note and could not be verified, so it is removed. | strong | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | measurement-source | Real growth = 100 x (rev_t / rev_t-1 - 1) on IBISWorld constant-price revenue with no further CPI adjustment. Spending in 2025 dollars = nominal x 321.9 / CPI_year. Democratic share = D / (D + R) by chamber, missing when the denominator is zero. Blanks stay missing, never zero. (Text lines 20, 32, 36-40, 58) | sgs_run.R lines 281-287 build the dollar-base text for dem-share tests from the base_usd and base_real_m columns, so the denominator is carried explicitly per sector-year. Those lines do not show the zero-denominator missing rule, the growth formula or the CPI multiplier; those were not confirmed in this pass. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_run.R:281-287` | IBISWorld covers whole industries while Mentat contributions cover a roster of companies, a population mismatch the note does not state. The attribution of this mismatch to ruling 6 is not in the note and was not verified. IBISWorld historical values may include estimates and later revisions (text line 38). | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-giving-forecast#Cycle-aware giving refit (phase and drift log models, 50-50 growth)\|Cycle-aware giving refit (phase and drift log models, 50-50 growth)]] | measurement-source | IBISWorld U.S. Key Statistics revenue is in constant prices, so growth ratios need no CPI adjustment. Previous-year growth (k = 1) is one of two actual-growth timings. Predicted growth uses the prior-year forecast for year t; 55 sector-year values, 53 passing the main checks. (Text lines 7, 36-38) | giving_forecast.py lines 1393-1398 comments describe M = B exp(phi dP + d_h)(1 + x/2), with x taken from the IBISWorld real growth path over the lag window c-3 to c-1, and company PACs exempt (x = 0, D-116). These comments confirm the lagged real-growth input but do not show how the 50/50 weight is derived. `C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py:1393-1398` | The note supplies only the measurement and lag convention. It says nothing about forecasting giving. The analyst's backtest figures (MAE 3.63 vs 3.99; growth beats naive in 45 percent of targets; LobbyView 2014-2023) were not found in the sector_growth_study folder and are removed. The D-113 attribution for the 50/50 weight is unverified. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#LDA finance-client classification and report join\|LDA finance-client classification and report join]] | context | Melange holds bank and CU lobbying aggregates for 1999-2023 and cached annual series for 2021-2025 plus partial 2026 (bank, CU, crypto, fintech). Life insurance and auto are not cached. Organizations still need alignment to the research sectors under one expenditure definition. (Text line 16 (readiness table row 2); line 22) | fec/lobbying/sector_spend.py line 49 sets DEFAULT_YEARS to 2021-2026, consistent with the cached 2021-2025 plus partial 2026 window. The analyst's claim that LobbyView finance classification is name-regex based with coverage reliable through 2021 (vintage 2024-10-25) was not checked. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/lobbying/sector_spend.py:49` | DEFAULT_YEARS covers the LDA sector-spend build, not the 1999-2023 bank and CU aggregates the note cites. Using LDA spend as an outcome per organization is an identity-source use consistent with D-021; no edge or weight use is implied. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Window misstated: the summary says the built study runs a 2020-2026 window. sgs_registry.R and sgs_config.R set the primary window to 2020-2025 (R8), with a fallback inside 2008-2025.
> - Summary issue: Sector count not established: the summary says nine sectors. sgs_registry.R lines 242 and 463 describe seven study sectors plus community_banks. Treat nine as unverified.
> - Summary issue: Misattributed quote in paul_alignment: the phrase seems to confirm does not appear in the note. The trend-over-significance rule is not stated in the note either. The note contains no regression results to comment on.
> - Summary issue: Unlocated figures: MAE 3.63 vs 3.99, growth beating naive in 45 percent of targets, 264 tests with 72 estimable, and retrospective 2023 expected-control fill were not found in the sector_growth_study folder by grep. Unverified.
> - Summary issue: Numbers present in the study's own review file: sgs_review.R lines 33-35 hold L3-LV base 0.352 and yearfe -0.477, with Holm p moving 0.0375 to 0.1125; lines 237-238 hold adjusted R2 0.884717 vs 0.884690. This is the study's own Codex review, not an independent second path, so it stays UNVERIFIED under DE-062.
> - Summary issue: Partisan-shift result -0.04 (p 0.76, 2021-25) only partly confirmed: coefficients near -0.0428 and -0.0405 appear in build_receipt_08_contributions_by_chamber.json, but the p-value and window were not located.
> - Summary issue: Design fidelity overstated: the lobbying tests add cur_dem_house and cur_dem_senate (sgs_registry.R line 34), which the note treats as optional, so the built design is not exactly the note's design.
> - Summary issue: Giving-forecast link: the 50/50 budget term fed by the IBISWorld lagged real-growth path is supported by code comments (giving_forecast.py lines 1393-1398). The no-out-of-sample-support point rests on the backtest figures, which could not be located.

## Gaps and flags

- The note is stale against the system. It says no regressions have run, uses a 2008-2025 four-sector panel and leaves 2023-2025 expected control absent. The study has since run its regressions with a primary window of 2020-2025 (ruling R8) over a different sector set than the note's four.
- Expected control for 2023 (Nov-2024 calls) and 2025 (Sep-2026 snapshot) is measured after the year. A strict forecast needs available_at gates, which the note's annual-mean convention does not provide.
- Contribution outcomes before 2021 (2020 now collected) do not exist locally. The note's 18-year contribution panel therefore cannot be built, and the core contribution tests C1-C3 rest on a few cycles.
- Life insurance and auto lobbying series are not cached, and crypto has no IBISWorld growth series, so two or three original sectors stay thin.
- There are few clusters (a handful of sectors) for the wild sector-cluster bootstrap. With 2 sectors the bootstrap cannot reach p below 0.056 (sgs_registry.R:22).
- The live cycle model's IBISWorld 50/50 growth term is fed by the lagged real-growth path (giving_forecast.py lines 1393-1398); no backtest of it was located.
- Flag: The header (nine-sector expansion: 72 reports, 86 growth inputs, a 162-row scaffold) and the four-sector body are unreconciled within the note.
- Flag: Pooled growth coefficient: the note's single b across sectors with a sector baseline must be quoted with per-sector strata or an UNSTRATIFIED label. The built code attaches strata and a stratification label (sgs_estimate.R:377, 401); the sgs_review.R figures were not checked against a printed run result.
- Flag: ExpectedDemHouse/Senate are year-level. Year fixed effects can test growth but cannot identify them, and the predicted-growth lobbying sign flipped (+0.352 to -0.477) once year FE were added.
- Flag: The Employee tier includes unrecognized-title fallbacks, so it is not verified rank-and-file. Any tier comparison must say so.
- Flag: The study figures quoted here are the reference values in the study's own Codex review file (sgs_review.R lines 33-35 and 237-238), not an independent re-run.
- Flag: No conflict with the rejected SNA proposals, D-025 or D-104..D-108; the note makes no network or reciprocity claims.

There are no bibliography entries; this is Paul's own note. The built study follows its design closely, so the two agree on method. The note is stale: the regressions have since run with a 2020-2025 primary window. The study's reference values show no adjusted R2 gain from growth and a lobbying coefficient that flips under year FE.

## Related papers

- [[kim-2018-lobbyview-firm-level]] — complements — LobbyView is a firm-level LDA database. The note does not name LobbyView as its source for the bank and CU aggregates, so this is background for the LDA measure, not confirmed same-data.
- [[goff-nd-cross-industry-finance]] — complements — Paul's unpublished client report on financial-services political money covers bank and credit-union sections, overlapping the note's sectors. The note does not cite it.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Its title reports low overlap between lobbying and PAC spending, which is consistent with running contributions and lobbying as separate outcomes. The note does not state this reasoning; it is the analyst's inference.
- [[kim-2020-strategic-sequential-links]] — complements — Its title covers strategic and sequential links between donations and lobbying, a lead-lag framing the note does not use.
- [[bickmore-2025-corporate-lobbying-political]] — complements — Its title covers corporate lobbying, PAC spending and market concentration, a related economic-driver design.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — Its title and tags cover the cryptocurrency lobby and PAC spending, the sector the note handles only through a proxy.

### Linked from
_None._