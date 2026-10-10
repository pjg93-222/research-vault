---
type: system
name: Mentat giving and PAC forecast
slug: mentat-giving-forecast
components: [mentat-giving-forecast.cycle-model, mentat-giving-forecast.slope-anchor-selection, mentat-giving-forecast.calculator-and-nowcast, mentat-giving-forecast.refit-gates, mentat-giving-forecast.pac-accuracy-attempts, mentat-giving-forecast.seasonal-naive-baseline]
brain_notes: [C:/Users/pgoff/Documents/brain/actors/giving-forecast-pipeline.md, C:/Users/pgoff/Documents/brain/projects/forecast-redesign.md, C:/Users/pgoff/Documents/brain/topics/2026-10-feature-forecast-giving-redesign-giving-by-cycle-live-model.md, C:/Users/pgoff/Documents/brain/topics/2026-09-feature-forecast-giving-redesign-six-year-lookback-lane.md]
paper_count: 14
tags: [system]
---

# Mentat giving and PAC forecast

Forecasts of industry and PAC political giving by election cycle. Covers the R refit (cycle-phase log models, 50/50 growth rule, slope and anchor auto-pick, G0-G9 gates), the console calculator's application rules, the company-PAC nowcast and BLEND, the PAC-to-candidate accuracy attempts under a rolling three-cycle cap, the remainder fix, and the Insights seasonal-naive baseline.

Brain notes: [actors/giving-forecast-pipeline](obsidian://open?vault=brain&file=actors%2Fgiving-forecast-pipeline); [projects/forecast-redesign](obsidian://open?vault=brain&file=projects%2Fforecast-redesign); [topics/2026-10-feature-forecast-giving-redesign-giving-by-cycle-live-model](obsidian://open?vault=brain&file=topics%2F2026-10-feature-forecast-giving-redesign-giving-by-cycle-live-model); [topics/2026-09-feature-forecast-giving-redesign-six-year-lookback-lane](obsidian://open?vault=brain&file=topics%2F2026-09-feature-forecast-giving-redesign-six-year-lookback-lane)

## Mentat giving and PAC forecast: evidence assessment

**What the evidence base is.** The hub holds 18 evidence rows from 14 distinct papers for 6 components. 5 rows are VERIFIED and 13 are VERIFIED-SOFTENED. 0 are UNVERIFIED, but several rows point to UNVERIFIED sub-claims inside them, and those stay leads. The rows sit on only 3 components:

| Component | Rows |
|---|---|
| cycle-model | 8 |
| slope-anchor-selection | 0 (**GAP**) |
| calculator-and-nowcast | 9 |
| refit-gates | 1 |
| pac-accuracy-attempts | 0 (**GAP**) |
| seasonal-naive-baseline | 0 (**GAP**) |

9 of the 18 rows (7 of 14 distinct papers) are Paul's own or grey documents, not independent literature: 4 `note-*` research notes (5 rows), `goff-nd-cross-industry-finance`, the `anon-2025` memo and the 2018 Harrison data-mining paper's form-level description. The notes are useful as design rules. They are **not** external evidence that the model is right.

**Bottom line.**

1. **The no-sum rule has the strongest support in the base.** Lobbying is shown as in-house expenses beside outside-firm fees and never added, and it is kept apart from PAC giving. Several sources describe the same federal LD-2 accounting, and all of them transfer.
2. **The forecasting claims have almost no external support.** These are the presidential-phase effect on dollars, the 50/50 IBISWorld growth rule, the pace nowcast, partial pooling, the gate criterion and the PAC accuracy bar.
3. **The citation Paul relies on for the cycle claim does not say what it is used for.** Bacik et al. find election-cycle "synchronization" in *whom* lobbyists target, using annual data with PAC channels excluded. The paper reports no phase effect on lobbying dollars and nothing on PAC giving.
4. **The two citations next to Bacik do not supply it either.** Huneeus & Kim and Furnas, LaPira & Brock support persistence and the separation of measures, not phase.
5. **The pieces that do bear on forecasting point toward naive baselines being hard to beat.** These are firm-level lobbying persistence plus Mentat's own lobbying MAE results, where the refit (4.050) and the growth term (3.99) both lose to naive (3.936 and 3.63).

All quoted coefficients are pooled across sectors unless stated otherwise and should carry an UNSTRATIFIED label. This covers the presidential uplift of +9% (D-119), the u = 1.8635 general-giving factor and the lobbying refit.


## Cycle-aware giving refit (phase and drift log models, 50-50 growth)

**What it computes.** Projected measures as log models of presidential vs midterm phase, trajectory and inflation. Growth = 50% prior cycle + 50% IBISWorld budget with a one-year lag.

**Method.** Live bundle refit-20261007. Presidential uplift +9% (D-119). Lobbying uses a log refit.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/forecast/refit (run_refit.R)`, `C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py`

**Assumptions.**
- Giving depends on election-cycle phase (D-092)
- Industry budget is a growth reference (D-113)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | qualifies | The paper's election-cycle 'synchronization' refers to target portfolios: agency-approach correlations form blocks by presidential administration, and some agencies such as the National Economic Council follow the presidential cycle. It reports no effect of election phase on lobbying dollars, and it aggregates data annually. | The paper supports a four-year, administration-linked periodicity in whom lobbyists target, not in how much they spend. It cannot justify a phase coefficient on lobbying dollars, which has to rest on Mentat's own backtests. The PAC side (D-092) is not addressed at all, because the paper excludes PAC channels. | moderate | VERIFIED-SOFTENED |
| [[chohan-2025-crypto-oligarchy-cryptocurrency]] | 2025 | supports | Fairshake raised a record $203 million for the 2024 cycle, reported as more than MAGA Inc. and Senate Majority PAC. These are receipts. The paper also reports $78 million raised toward the 2026 midterms 'to date' as of its January 2025 date. The text gives no FEC source for either figure and cites press items. | The figures show a large money series appearing for 2024 and early 2026. They do not establish a level break, a first cycle, or any per-cycle history before 2024, so they cannot by themselves justify a break term. Any admission would still need FEC-sourced totals. | moderate | VERIFIED-SOFTENED |
| [[goff-nd-cross-industry-finance]] |  | qualifies | Crypto industry giving is projected north of 175 million dollars for 2025-2026, versus 145.5 million in 2023-2024, an off-year rise the report calls an outlier and attributes to the CLARITY Act push. | Only applies if crypto giving is run through the general_giving measure, which was not checked. The report's 175 million is a projection with no stated method, window or source, so it is a counterexample to test, not evidence. | moderate | VERIFIED-SOFTENED |
| [[zeileis-nd-econometric-computing-with]] |  | qualifies | On the public schools data (51 states, Wisconsin NA dropped, N=50), the quadratic income term has SE 830, z 1.91, p 0.056 under HC0 and SE 5489, z 0.29, p 0.77 under HC4. Paper attributes the gap to one high-leverage outlier (Alaska) and HC4's leverage correction. It cites Long and Ervin (2000) for HC3 as best in small samples and makes HC3 the vcovHC default. | The paper's HC0 vs HC4 contrast is an estimator-choice comparison on one 50-state cross-section with one outlier. It does not show HC1 understates SEs in the refit's ~11 df regressions. The HC1 understatement point is the analyst's inference from cited small-sample literature, not a result in this paper. The paper has no simulation. | weak | VERIFIED-SOFTENED |
| [[note-request-for-review-industry-economic-performance]] | 2026 | supports | Election-cycle controls use Pres and Midterm dummies with odd years as the baseline. Industry forecasts (IBISWorld, trade outlooks) are used only to predict 2026 spending after estimation and are never mixed into the estimated growth variable. | Supports only the forecast product split between estimation and projection. Does not cover the sector study C3 and L3 tests, and the presidential-difference term is not the memo Midterm and Pres dummy set. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-industry-growth-and-political-spending-research]] | 2026 | qualifies | Predicted industry growth is the variable of direct interest, and the sign of the growth-spending relation is to be estimated, not assumed; IBISWorld forecasts proxy public expectations only and do not establish any company's private forecast or whether a donor read the report. | The plan supplies a design principle, not evidence for the 50/50 term. The rel's claim that Paul ruled roster mismatch a stated limitation does not appear in the plan and is unverified. The build's no-stable-effect reading is single path and UNVERIFIED under DE-062. No backtest of the 50/50 term was checked here. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-review-industry-growth-and-political-spending]] | 2026 | qualifies | Value added, gross output, profits and assets are different quantities; output growth is not spare cash; corporate PAC budgets come from voluntary SSF contributions, so a capacity mechanism fits lobbying more than PAC giving. | The backtest does not isolate any revenue-to-spending link, so it cannot be cited as evidence that the IBIS budget rule is weak. Use only as a fact about the growth term's forecast error in that window. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-sector-growth-and-political-spending-source]] | 2026 | measurement-source | IBISWorld U.S. Key Statistics revenue is in constant prices, so growth ratios need no CPI adjustment. Previous-year growth (k = 1) is one of two actual-growth timings. Predicted growth uses the prior-year forecast for year t; 55 sector-year values, 53 passing the main checks. | The note supplies only the measurement and lag convention. It says nothing about forecasting giving. The analyst's backtest figures (MAE 3.63 vs 3.99; growth beats naive in 45 percent of targets; LobbyView 2014-2023) were not found in the sector_growth_study folder and are removed. The D-113 attribution for the 50/50 weight is unverified. | internal (design doc) | VERIFIED-SOFTENED |

**Assessment.**

### Cycle-aware giving refit

**Evidence weight on the core claim is weak.** The core claim is that giving and lobbying dollars depend on presidential vs midterm phase (D-092). The one external paper on periodicity, `bacik-2025-measuring-dynamical-evolution` (VERIFIED-SOFTENED, qualifies), finds four-year, administration-linked blocks in *which agencies* registrants target. Its data are annual federal LDA, and it reports **no** effect of election phase on lobbying dollars. It excludes PAC channels, so it says nothing about the PAC side of D-092. A phase coefficient on dollars therefore has to rest entirely on Mentat's own backtests. No external study in this base tests whether PAC-to-candidate giving is higher in presidential cycles net of inflation.

**The growth term (50/50 IBISWorld, one-year lag, D-113) has no external empirical support.** The related rows are all Paul's own notes:
- `note-industry-growth-and-political-spending-research` says the sign of the growth-spending relation is "to be estimated, not assumed". The live rule assumes a positive 0.5 pass-through.
- `note-review-industry-growth-and-political-spending` argues that output growth is not spare cash. Corporate PAC budgets come from voluntary SSF contributions, so a capacity mechanism fits lobbying better than PAC giving. This is consistent with D-116 setting x = 0 for company PACs. It cuts against keeping x for general giving, which includes PAC-like money.
- `note-sector-growth-and-political-spending-source` confirms only the measurement convention: IBIS revenue is in constant prices, k = 1 lag, and 53 of 55 sector-year values pass the main checks.
- `note-request-for-review-industry-economic-performance` supports keeping forecast growth out of estimation. The cycle model complies.

The "no stable effect" reading of the build is single-path and UNVERIFIED (DE-062), so it is a lead, not a finding.

**Inference.** The robustness block reports an HC1 row on models with about 11 degrees of freedom. `zeileis-nd-econometric-computing-with` shows that HC estimators diverge sharply under leverage: the quadratic-income SE is 830 under HC0 and 5489 under HC4, on a 50-state cross-section with one outlier. The paper does not show that HC1 understates SEs here, so it is a weak lead. The same block already prints CR2 and a wild cluster restricted bootstrap; headline inference should rest on those rather than HC1.

**Counterexamples to test (not evidence).** Two rows bear on crypto:
- `goff-nd-cross-industry-finance` projects crypto giving above $175M for 2025-26, against $145.5M in 2023-24. The source is unmethodical: no window, no source.
- If crypto ran through general_giving, the u = 1.8635 factor would scale the midterm step down by about 46%.
- `chohan-2025-crypto-oligarchy-cryptocurrency` reports Fairshake receipts of $203M for 2024 and $78M toward 2026 as of January 2025, sourced to press, not FEC.
- Crypto is currently excluded from the pac, control and lobbying models, and that exclusion was verified.

Neither row establishes a level break.

**Contradictions.**
- Paul cites Bacik et al. for 'lobbying synchronizes with the election cycle and more money is given in election years'. Bacik reports synchronization of target portfolios by administration, not of dollars, and reports no election-phase effect on lobbying spending (annual data, PAC channels excluded). The citation does not support the dollar claim.
- The other two system citations for this component (Huneeus & Kim 2018; Furnas, LaPira & Brock 2025) bear on firm-level lobbying persistence and on keeping lobbying and PAC measures separate. Neither addresses election-phase effects.
- Inconsistent uplift figures. The component states a presidential uplift of +9% (D-119), while the goff-nd bearing reads code applying u = 1.8635 to general_giving on both cycle steps (giving_forecast.py:1425, gate at 1888), about -46% on the midterm step. These may be different measures (company-PAC phi vs general giving), but the record does not reconcile them, and both are pooled (UNSTRATIFIED) figures.
- Paul's own research plan says the growth-spending sign is to be estimated, not assumed. The live 50/50 rule fixes a positive 0.5 weight from an owner ruling (2026-10-07), and the D-113 attribution for the weight is itself unverified per the source-check bearing.
- The memo specification (Pres and Midterm dummies, odd-year baseline) differs from the live dP = Pres(c) - Pres(c-2) difference term. The sector study puts predicted_growth_pct on the right-hand side in C3/L3 (sgs_registry.R:30), which the memo prohibits. That departure is outside this component but is the same research program.

**Gaps.**
- No external study tests whether industry or corporate PAC-to-candidate giving is higher in presidential cycles than in the preceding midterm, net of inflation. Needed: a multi-cycle FEC panel (for example 2000-2024), sector-stratified, real dollars, with company PAC and general giving estimated separately.
- No evidence that industry revenue growth improves giving forecasts over seasonal naive. Needed: a pseudo-out-of-sample forecast comparison with a named target window and denominator (sectors x cycles), reporting per-sector errors rather than a pooled MAE.
- No evidence on the lobbying log refit's phase term beyond the system's own backtests.

**Paul's position vs the evidence.** Paul's position is that lobbying synchronizes with the election cycle and more money flows in election years. The verified reading of his own source supports only the first half, and only for *targets*. Bacik finds administration-linked periodicity in which agencies are lobbied, not in amounts, and does not cover PAC giving. The 'more money in election years' half has no supporting evidence in this base. It currently rests on Mentat's internal fits (+9% presidential uplift, D-119, UNSTRATIFIED). The 50/50 growth rule is not supported by any external study, and Paul's own plan says the sign should be estimated, not assumed.

**Open questions.**
- Are the +9% (D-119) uplift and the u = 1.8635 general_giving factor different measures, and which is live for which series?
- Does any crypto series flow through general_giving, where the 1/1.8635 midterm scaling would apply?
- What is the out-of-sample error of the 50/50 IBIS term for general giving against seasonal naive, by sector, over a stated window?
- Is the presidential uplift stable across sectors, or does the pooled figure hide sector-level sign differences?
- Should headline inference switch from HC1 to CR2 or the wild cluster bootstrap, given about 11 df?

**Read first:** [[bacik-2025-measuring-dynamical-evolution]], [[note-review-industry-growth-and-political-spending]], [[note-industry-growth-and-political-spending-research]], [[note-sector-growth-and-political-spending-source]], [[note-request-for-review-industry-economic-performance]], [[zeileis-nd-econometric-computing-with]]

## Slope (R0-R1-R2) and anchor auto-pick

**What it computes.** Pooled, own or DerSimonian-Laird partially pooled slopes per industry. The same-phase anchor is used only when it wins by 5% in both backtests.

**Method.** Own slopes beat the blend (LOEO 10.8 vs 14.3).

**Code refs.** `C:/Users/pgoff/Mentat/mentat/forecast/refit`

**Assumptions.**
- The rule that wins both backtest designs generalizes

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment.**

### Slope (R0/R1/R2) and anchor auto-pick: GAP

No evidence row bears on this component. The DerSimonian-Laird estimator is cited in the system as a method reference, but no paper in the base tests whether partially pooling industry slopes reduces forecast error in small sectors. Nothing tests the auto-pick rule either: the same-phase anchor is chosen only when it wins by 5% in both backtests.

The only supporting figure is internal: own slopes beat the blend, LOEO 10.8 vs 14.3. The record does not state that figure's unit, sector set or election window, so it cannot be quoted as is.

Two design risks follow from the structure, not from evidence:
1. A rule chosen because it won two backtests on the same short history is a selection step, and its generalization needs a nested or forward holdout.
2. Random-effects variance estimators behave differently when the number of groups is small. That is a reason to compare estimators (DL vs REML or Paule-Mandel), not a finding against DL here.

The reading list names adjacent papers only: persistence, small-sample inference and the growth measurement source. None of them bears directly.

**Contradictions.**
- None recorded.

**Gaps.**
- No study of partial pooling (DL or otherwise) against own-slope models for forecasting political giving in small-sample sectors. Needed: a simulation plus a historical pseudo-out-of-sample comparison of R0/R1/R2 by sector, with sector size as a moderator.
- No evidence that the two-backtest, 5% margin auto-pick rule generalizes. Needed: a nested holdout in which the rule is chosen on early cycles and scored once on a later cycle.
- The LOEO 10.8 vs 14.3 comparison lacks a named window and denominator in the record.

**Paul's position vs the evidence.** Paul states no literature for this component, and the evidence base agrees: there is none. The design rests entirely on Mentat's internal LOEO backtests.

**Open questions.**
- What are the unit, sector count and election window behind LOEO 10.8 vs 14.3?
- How many sectors fall into R2, and with how many cycles each?
- Would REML or Paule-Mandel tau-squared change which slope wins in small sectors?
- Has the auto-pick rule ever been scored on a cycle that was not used to choose it?

**Read first:** [[zeileis-nd-econometric-computing-with]], [[huneeus-2018-effects-firms-lobbying]], [[note-sector-growth-and-political-spending-source]]

## Calculator rules, company-PAC nowcast and BLEND

**What it computes.** Relative change on previous-cycle anchors. Company-PAC nowcast = O + (1-s) B, with s = geometric mean of pace shares. The BLEND (D-099) is overridden by the panel nowcast (D-116). CU cohort of 16 PACs.

**Method.** Anchors exclude non-giving receipts. Lobbying is shown in-house vs outside, never summed.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py`, `C:/Users/pgoff/Mentat/mentat/forecast/anchors/build_giving_anchors.py`

**Assumptions.**
- Within-cycle pace is informative

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | supports | Registrants split into K-Street firms that file for others and in-house self-filers. The monetary field differs between them (income for firms, expenses for self-filers), and self-filers have a higher reporting threshold. | This is the same LD-2 accounting, so the evidence carries over fully. The paper also assigns low-cost reports a threshold value as an upper bound. If Mentat's sector_spend rows ever take values from LobbyView, check whether they include that imputation. | moderate | VERIFIED |
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | supports | Lobbying is persistent: 92% of firms that lobby in a given year also lobby the next (Kerr et al. 2014), explained by large sunk costs. Conditional on lobbying, the elasticity of lobbying expenditure to firm sales is around one (Huneeus and Kim 2019). | The 92% figure is a firm-level extensive-margin rate (does a firm lobby at all), not dollar-level persistence for an industry. The review offers no evidence on how lobbying dollars move within a cycle. | moderate | VERIFIED |
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | supports | Bertrand et al. (2014), about 37,000 active lobbyists over 1999-2008: the share of in-house LOBBYISTS fell from nearly 60% to under 40%, and the roughly 30% growth in lobbying expenses was almost entirely due to external lobbying, which doubled. | The 60%->40% figure is a share of lobbyists (headcount), not of lobbying dollars, over all federal LDA lobbying 1999-2008. No finance or CU sector breakdown is reported, so this supports showing the two channels separately but provides no sector-specific shares. | weak | VERIFIED-SOFTENED |
| [[harrison-2018-application-data-mining]] | 2018 | supports | The XML amounts do not differentiate lobbying expenditures from income, and greater statistical analysis of the currency values is limited until the forms and procedures used to collect lobbying data are adjusted. | The paper describes the federal LD-2 form, not an empirical estimate, so it carries over to 2021-2026 Senate LDA data. It supports the D-064 rule; it does not measure the overlap. | strong | VERIFIED-SOFTENED |
| [[harrison-2018-application-data-mining]] | 2018 | supports | LDA records are grouped by time of filing, not time of activity: Q4 2009 files include records from 2002-2008 filed after the fact, and the authors add a filter to keep 2009 events only. The paper describes no amendment handling. | Carries over: same federal LDA filing structure. The system is stricter than the paper, which only year-filters and does not deduplicate amendments. | moderate | VERIFIED |
| [[furnas-2019-partisan-ties-lobbying]] | 2019 | qualifies | A change in House control moves contract-lobbying revenue between partisan firms: in 2011 lobbyists at Republican-aligned firms gained just over $10,000 for the year and ousted Democratic-aligned firms lost more than $40,000 in annual revenue (firm-level difference-in-differences on revenue per lobbyist). In Figure 2 (90% threshold, time-fixed measure only) Republican and Democratic firms were roughly at parity, with a marginal advantage to Democratic firms in 2008-2010 and to Republican firms in 2011-2016; bipartisan firms consistently earned greater average revenue. | The paper's population (federal contract registrants, registrant differs from client) corresponds to the outside-firm fees series, not in-house expenses. The effect is distributional across firm partisanship; the paper does not identify a sector-total change. A common control shock would be absorbed by factor(year) in the fit but cannot be projected forward. Before adding a control-flip term, test whether outside-firm totals step after the 2011, 2015 and 2019 flips. | weak | VERIFIED-SOFTENED |
| [[furnas-nd-conflating-lobbying-pacs]] |  | supports | Lobbying and PAC spending have different distributions: lobbying spenders concentrate between $10,000 and $1 million and few disclose under $10,000, while PAC giving is more uneven, with many groups giving under $10,000. A PAC contribution can be one-off, while hiring a lobbyist stretches over months or years. The authors argue the umbrella term 'money in politics' is imperfect and that researchers should say which type they study. | The paper supports keeping giving and lobbying as separate measures with separate models. It does not speak to the in-house vs outside-firm no-sum rule, which is about LDA double counting; the paper says only that its lobbying totals include both in-house and contracted-firm spending and does not describe deduplication. Mentat's sector growth study config separately records a median 9-26% double count in undifferentiated LobbyView sums (C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_config.R:118-120). Descriptive evidence only. | weak | VERIFIED-SOFTENED |
| [[anon-2025-cross-industry-political]] | 2025 | context | Credit unions have the lowest CEO and executive averages of the four industries (CEO $5,147, executive $3,009). Their giving is coordinated through the ACU trade association, with a near-perfect bipartisan balance (memo gives no figure), emphasis on open seats and special elections, and hedging across opposing candidates. | Supports treating credit-union political money as PAC- and association-led, so the panel cohort is the right unit. The near-perfect ratio and the volatility claims have no figures, so they cannot check the pinned split. | weak | VERIFIED |
| [[note-review-industry-growth-and-political-spending]] | 2026 | supports | LDA in-house expenses include retained-firm payments and lobbying-related association dues, so adding client expenses, firm revenues and association expenses double-counts; keep the series separate or deduplicate. | Direct for any sector total. Trade-association overlap is not yet handled separately, per the bearing; not confirmed by code read here. | internal (design doc) | VERIFIED |

**Assessment.**

### Calculator rules, company-PAC nowcast and BLEND

**The accounting rules are well supported.** Several rows independently support keeping LDA outside-firm fees (income_total) and in-house expenses (expenses_total) separate and never summed:
- `harrison-2018-application-data-mining`: the LD-2 XML amounts do not distinguish expenses from income. This is a form-level description, so it transfers fully.
- `note-review-industry-growth-and-political-spending`: in-house expenses include retained-firm payments and association dues, so summing double-counts.
- `bacik-2025-measuring-dynamical-evolution` (VERIFIED): K-Street firms report income, self-filers report expenses, and the two face different reporting thresholds.
- `bombardini-2020-empirical-models-lobbying`: the in-house lobbyist share fell from nearly 60% to under 40%, 1999-2008. That is a share of headcount, not dollars, across all federal LDA, so its strength is weak.
- `furnas-nd-conflating-lobbying-pacs`: lobbying and PAC spending have different distributions and timing and should be modelled separately. This is descriptive.

The amendment handling is stricter than Harrison's. Harrison groups records by filing time and only year-filters; sector_spend keeps the latest dt_posted per registrant, client, year and period.

**The nowcast itself has no evidence.** The formula is O + (1-s)B, with s the geometric mean of pace shares. No row tests within-cycle pace or the month at which a pace nowcast beats a model. Bombardini's 92% persistence figure (Kerr et al. 2014) is an extensive-margin, firm-level rate: does a firm lobby at all in year t+1. It is not persistence of dollars at sector level, and the bearing explicitly says the review offers nothing on within-cycle dollar movement.

**Lobbying projection.** `furnas-2019-partisan-ties-lobbying` shows House control flips moving contract-lobbying revenue *between* partisan firms (2011: about +$10k per lobbyist at R-aligned firms, more than -$40k at D-aligned firms). This is distributional and firm-level, and the paper does not identify a change in sector totals. Before any control-flip term is added, test whether outside-firm totals step after the 2011, 2015 and 2019 flips.

**Credit-union cohort.** `anon-2025-cross-industry-political` is a grey memo with no figures for its balance or volatility claims. It is consistent with treating CU money as PAC- and association-led, so the 16-PAC panel cohort (D-117) is a sensible unit. It cannot check the pinned D/R split.

**Contradictions.**
- The bombardini-2020 system_side records that the earlier year-effect slope model never beat last-period naive (G9). The lobbying projection is now a nowcast carried forward unchanged, so the model path it replaced had already failed against naive. This is consistent with the evidence, but it means the lobbying 'forecast' is effectively persistence plus pace.
- Trade-association overlap is not handled separately, per the note-review bearing (not confirmed by a code read), even though that note identifies association dues as a double-count source. The no-sum rule covers the firm vs self-filer overlap only.

**Gaps.**
- No evidence on when a within-cycle pace nowcast outperforms the model forecast. Needed: a month-by-month backtest across past cycles of O + (1-s)B against the model and seasonal naive, by sector, with error reported per month of the cycle.
- No evidence that the geometric-mean pace share s is a better estimator than alternatives such as the median, the last cycle, or phase-matched pace.
- The LobbyView threshold imputation (Bacik: low-cost reports assigned a threshold value as an upper bound) has not been checked against Mentat sector_spend rows.

**Paul's position vs the evidence.** Paul states no literature. In fact the accounting side is well covered: the D-064 no-sum rule and the separation of lobbying from PAC giving are supported by VERIFIED and VERIFIED-SOFTENED rows on the same LD-2 structure. The nowcast and BLEND logic, which is the forecasting part, has no literature, so his statement is accurate for that part.

**Open questions.**
- Do any sector_spend values come from LobbyView with Bacik's threshold-value imputation?
- At what month of the cycle does the pace nowcast first beat the model, by sector?
- Do outside-firm fee totals step after the 2011, 2015 and 2019 House control flips?
- Should trade-association dues be deduplicated from in-house expenses for sectors with large associations (CU, banking)?

**Read first:** [[harrison-2018-application-data-mining]], [[bacik-2025-measuring-dynamical-evolution]], [[note-review-industry-growth-and-political-spending]], [[bombardini-2020-empirical-models-lobbying]], [[furnas-2019-partisan-ties-lobbying]], [[furnas-nd-conflating-lobbying-pacs]]

## Refit gates G0-G9

**What it computes.** FAIL, HOLD or PUBLISH. G6: no industry's leave-one-election-out MAE may exceed the calculator's. G9 warns when the refit fails to beat naive.

**Method.** Waivers applied. Log lobbying MAE 4.050 vs naive 3.936.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/forecast/refit/run_refit.R`

**Assumptions.**
- LOEO MAE is an acceptable criterion

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[huneeus-2018-effects-firms-lobbying]] | 2018 | context | Firm lobbying is persistent. Fact 4 (printed p.10): more than 80 percent of firms that lobbied in 2016 also lobbied in 2017. Figure 3 (printed p.10): adjacent-year intensive-margin log-lobbying slope 0.922 (t = 157.88, conditional on lobbying in both years). The Figure 3 caption says over 70 percent of firms lobby in two consecutive years. Persistence dropped during the 2007-2009 financial crisis. The paper does not address forecasting. | The paper's persistence is firm-level on public firms in a 1999-2018 sample; the forecast works on sector aggregates. The claim that naive is a strong baseline is the analyst's inference from persistence, not the paper's. The 2007-2009 break lies outside the 2014-2023 target window but may fall in training data for later origins; the backtest start year was not checked, so 'regime shifts are not in the backtest' is not established. Persistence says nothing about whether growth adds signal. | moderate | VERIFIED-SOFTENED |

**Assessment.**

### Refit gates G0-G9

One row bears on this component, as context only. `huneeus-2018-effects-firms-lobbying` (VERIFIED-SOFTENED) documents strong firm-level lobbying persistence:
- More than 80% of firms that lobbied in 2016 also lobbied in 2017.
- The adjacent-year intensive-margin log-lobbying slope is 0.922, conditional on lobbying in both years.
- The sample is public firms, 1999-2018.
- Persistence dipped in 2007-2009.

That persistence makes it plausible that naive is a strong baseline. The paper does not say so and does not address forecasting, so the link is the analyst's inference, and it is at firm level, not for the sector aggregates the gates score.

The internal record agrees with that inference. The README records the log-lobbying refit at MAE 4.050 against naive 3.936, so G9 fires and the refit does not beat naive; this is a warning only, and waivers were applied. Whether the 2007-2009 break sits in the training data for later rolling origins was not checked.

No row tests whether LOEO MAE is a valid gate criterion, meaning whether refits that pass G6 forecast the next cycle better than the published calculator. With few elections per industry, LOEO MAE differences are noisy, and no row supplies a significance or stability standard for them.

**Contradictions.**
- Waivers let a refit go forward even though G9 shows the lobbying refit failing to beat naive (MAE 4.050 vs 3.936, window and unit as recorded in the README, not re-checked). The gate system flags the failure but does not block on it, and Huneeus-type persistence makes this failure expected rather than incidental.

**Gaps.**
- No study validates LOEO MAE as a predictor of next-cycle performance. Needed: prospective scoring of G6-passing refits against the published calculator on the next realized cycle (2026), recorded as a single pre-committed look.
- No standard for whether an MAE difference between refit and calculator exceeds noise at the available number of elections. Needed: a forecast-comparison test (Diebold-Mariano-type or a bootstrap over elections) with the window and denominator stated.
- Persistence evidence is firm-level, so sector-aggregate persistence of LDA dollars needs its own descriptive check.

**Paul's position vs the evidence.** Paul states no literature, and the base has only contextual support. The persistence finding is consistent with what the gates observe (refits struggle to beat naive), but it does not validate LOEO MAE as the criterion.

**Open questions.**
- Which waivers were applied, and to which industries?
- Does the rolling-origin training data for later origins include 2007-2009?
- How many elections enter LOEO per industry, and what is the sampling noise of the MAE gap?

**Read first:** [[huneeus-2018-effects-firms-lobbying]], [[bombardini-2020-empirical-models-lobbying]], [[zeileis-nd-econometric-computing-with]], [[note-review-industry-growth-and-political-spending]]

## PAC-to-candidate accuracy attempts (rolling three-cycle cap)

**What it computes.** Scored ORIGIN and nowcast forecasts, 2018-2024. Bar: A >= 90 overall and >= 75 per phase.

**Method.** v6: ORIGIN B1 plus N/(N+1) cross-sector pace shrinkage. Only telecom passes. A common 2022 drop defeats window rules (DE-066/067).

**Code refs.** `C:/melange-data/forecast-redesign-20261001-review (scratch)`

**Assumptions.**
- Gains must come from method, not more history

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment.**

### PAC-to-candidate accuracy attempts: GAP

No evidence row bears on this component. The central empirical question has no study behind it: was the 2022 drop in industry PAC-to-candidate giving a common shock across sectors? The record (DE-066/067) says the drop defeats window rules. It is plausible that something cycle-specific hit corporate PACs between 2020 and 2022, and announced pauses after January 2021 are an obvious hypothesis. That is a lead to check against FEC data, not a finding.

The accuracy bar is A >= 90 overall and >= 75 per phase, scored over 2018-2024. That is 4 cycles, roughly 2 per phase, so per-phase accuracy rests on very few targets. Only telecom passes under v6 (ORIGIN B1 plus N/(N+1) cross-sector pace shrinkage), and that pass should be read with the same small-n caution.

Adjacent papers give context only:
- `furnas-nd-conflating-lobbying-pacs`: PAC giving is lumpier than lobbying, with many groups under $10k, which bears on volatility.
- `chohan-2025` and `goff-nd`: new money series, such as crypto, can appear abruptly.
- `anon-2025`: CU giving is association-coordinated.

**Contradictions.**
- None recorded.

**Gaps.**
- No study of a common 2022 shock. Needed: a sector-by-sector FEC decomposition of corporate PAC-to-candidate giving for 2018, 2020, 2022 and 2024, separating the number of active PACs from dollars per active PAC, and testing whether the 2022 change is common across sectors or concentrated.
- No literature on forecasting PAC-to-candidate totals at all, including the rolling three-cycle cap. Needed: a forecast-comparison study across sectors with a pre-registered, single-look holdout.
- No evidence on whether cross-sector pace shrinkage, N/(N+1), is appropriate when sectors share a common shock. Shrinking toward a shocked mean could transmit the shock.

**Paul's position vs the evidence.** Paul states no literature, and that is correct. The 'gains must come from method, not more history' rule (the six-year lookback directive) is a design choice. No evidence row tests it against longer-history alternatives.

**Open questions.**
- Was the 2020-2022 drop driven by fewer active corporate PACs, smaller per-PAC giving, or a few large PACs?
- How many targets per phase does the 2018-2024 window give each sector, and what is the sampling uncertainty of A at that n?
- Does telecom pass because of method or because it was least affected by the 2022 shock?

**Read first:** [[furnas-nd-conflating-lobbying-pacs]], [[anon-2025-cross-industry-political]], [[chohan-2025-crypto-oligarchy-cryptocurrency]], [[goff-nd-cross-industry-finance]]

## Insights seasonal-naive baseline and remainder fix

**What it computes.** Repeat the same-phase prior cycle; withhold without 4 complete cycles. Conditional additive remainder replaces subtraction (7 of 19 shares had gone negative).

**Method.** Seasonal naive.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/app/analysis_metrics.py:690-794`, `apply_f2.py`

**Assumptions.**
- Four-year electoral periodicity

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment.**

### Insights seasonal-naive baseline and remainder fix: GAP on direct evidence

No evidence row bears on this component. Paul's stated support is Bacik, which shows four-year, administration-linked periodicity in lobbying *targets*, aggregated annually. That is a reason to expect some four-year structure. It is not evidence that dollar levels repeat by election phase, and it does not cover PAC giving.

The persistence literature (Huneeus & Kim; Bombardini) concerns *adjacent-year* persistence. That supports a last-period naive more than a same-phase, four-year-lag naive. These are different baselines and should not be conflated.

The comparison quoted in the empirical question (3.63 vs 3.99 MAE) has disputed provenance:
- `note-sector-growth-and-political-spending-source` records that the figures were **not found** in the sector_growth_study folder and removes them.
- `note-review-industry-growth-and-political-spending` carries them as a brain-note fact (LobbyView targets 2014-2023) and warns that they test the study's growth term, not the IBIS budget rule.
- Either way, that comparison is last-year naive against growth, not seasonal naive.

The remainder fix replaced subtraction with a conditional additive remainder after 7 of 19 shares went negative. It is an accounting correction and needs no literature, only a check that shares now sum and stay non-negative.

**Contradictions.**
- Provenance conflict over the MAE 3.63 vs 3.99 backtest. One VERIFIED-SOFTENED row says the figures were not found in the sector_growth_study folder and removes them; another treats them as a recorded brain-note fact. Until the source is located, the figure should not be quoted.
- Mismatched baselines. The component's empirical question asks whether *seasonal* naive beats growth models, but the 3.63 figure is a *last-year* naive (per note-review), so it does not test the seasonal-naive baseline Insights uses.
- The citation to Bacik for election-cycle periodicity applies to target portfolios, not to the dollar levels the seasonal-naive baseline repeats.

**Gaps.**
- No evidence that same-phase seasonal naive beats last-period naive or growth-augmented models for giving or lobbying dollars. Needed: a three-way pseudo-out-of-sample comparison (seasonal naive, last-period naive, growth model) by sector and measure, with the window and denominator named.
- No evidence on the 4-complete-cycles withholding threshold. Needed: an error-vs-history-length check showing where seasonal naive stabilizes.

**Paul's position vs the evidence.** Paul says Bacik highlights election-cycle periodicity. That is true for lobbying targets (which agencies, by administration), not for spending amounts or PAC giving. As support for a same-phase dollar baseline, the citation overreaches. The baseline may still be a good choice, but that has to be shown by Mentat's own backtests, and the quoted backtest is of a different naive with disputed provenance.

**Open questions.**
- Where is the source of the 3.63 vs 3.99 figures, and what exactly were the baseline and the target window?
- Does seasonal naive beat last-period naive for PAC giving, where a phase effect is more plausible than for lobbying?
- After the remainder fix, do all 19 shares stay non-negative and sum correctly across sectors?

**Read first:** [[bacik-2025-measuring-dynamical-evolution]], [[huneeus-2018-effects-firms-lobbying]], [[bombardini-2020-empirical-models-lobbying]], [[note-review-industry-growth-and-political-spending]]

## Across components

1. **Bacik is load-bearing in two places and supports neither as cited.** Paul uses Bacik et al. for the cycle model ("more money in election years") and for the seasonal-naive baseline (periodicity). The verified reading supports administration-linked periodicity in *lobbying targets* only. The data are annual, PAC channels are excluded, and the paper reports no phase effect on dollars. The same paper *does* firmly support the in-house vs outside-firm no-sum rule, which is not where Paul cites it.

2. **The citations sit on the wrong components.** The three citations the cycle model uses each have VERIFIED-SOFTENED bearings, but all of them land elsewhere:
   - Bacik: targets, not dollars.
   - Huneeus & Kim: firm-level persistence. This is relevant to the gates and baselines.
   - Furnas, LaPira & Brock: keep PAC and lobbying measures apart. This is relevant to the calculator.

   The presidential phase effect on PAC giving (D-092) has **no** external support in the base.

3. **One consistent signal: naive is hard to beat for lobbying dollars.** It shows up in three internal results and one paper:
   - The lobbying refit loses to naive (MAE 4.050 vs 3.936, G9).
   - The year-effect slope model never beat last-period naive.
   - The growth term loses to last-year naive (3.63 vs 3.99, provenance disputed).
   - Firm-level persistence (Huneeus: slope 0.922, more than 80% repeat lobbying, 2016 to 2017, public firms) fits this pattern.

   This is consistent with the shift to the lobbying nowcast. It cuts against keeping the IBIS 50/50 growth term in general giving without an out-of-sample test.

4. **The growth rule conflicts with Paul's own research plan.** The plan says the growth-spending sign is to be estimated. The review note says a capacity mechanism fits lobbying more than PAC giving. D-116 already drops x for company PACs, and the remaining x for general giving rests on an owner ruling, not evidence. The D-113 attribution for the 50/50 weight is unverified.

5. **Small-n and pooling discipline applies everywhere.** The affected figures are:
   - LOEO 10.8 vs 14.3, which has no window or denominator in the record.
   - The +9% uplift, which is pooled (UNSTRATIFIED).
   - The u = 1.8635 factor, which is pooled and does not reconcile with +9%.
   - The PAC accuracy bar, scored on 4 cycles with about 2 per phase.
   - The about 11 df refit regressions with an HC1 row.

   None of these should be quoted without its window, its denominator, and either strata or an UNSTRATIFIED label.

6. **Internal documents are not independent evidence.** 9 of the 18 rows are Paul's notes, his report or a grey memo. They are good as design rules and as statements of intent. They cannot corroborate the model they were written to guide.

7. **No row in this base touches the SNA proposals that the project has rejected**, so nothing needs flagging under that rule. LDA is used here only as an accounting and identity source, consistent with D-021.

## Research agenda

1. Sector-stratified FEC panel test of the presidential vs preceding-midterm effect on industry and corporate PAC-to-candidate giving, in real dollars, over at least 2000-2024. Estimate company PAC and general giving separately; this is the missing evidence for D-092.
2. Pseudo-out-of-sample forecast comparison of seasonal naive, last-period naive and growth-augmented (IBIS 50/50) models by sector and measure, with a named target window, a sectors x cycles denominator and a Diebold-Mariano-type or bootstrap-over-elections test.
3. Locate the source of the MAE 3.63 vs 3.99 backtest. One bearing says it is absent from sector_growth_study. Record exactly which naive and which window it used before quoting it again.
4. Reconcile the +9% presidential uplift (D-119) with the u = 1.8635 general_giving factor (giving_forecast.py:1425, 1888). Record which series each applies to, and report sector strata or label them UNSTRATIFIED.
5. Sector-by-sector FEC decomposition of the 2020-2022 corporate PAC-to-candidate drop (active-PAC count vs dollars per PAC) to test the common-shock hypothesis behind DE-066/067. Treat post-January-2021 PAC pauses as a hypothesis to check, not a finding.
6. Month-by-month backtest of the company-PAC pace nowcast O + (1-s)B against the model and naive across past cycles, to find when pace starts to win; compare the geometric-mean s with alternatives.
7. Simulation plus historical comparison of own vs pooled vs partially pooled (DL, REML, Paule-Mandel) industry slopes, with sector size as moderator; nested holdout for the two-backtest 5% auto-pick rule.
8. Single pre-committed prospective look: score the G6-passing refits against the published calculator on the realized 2026 cycle, to test LOEO MAE as a gate criterion.
9. Before adding any House-control term, test whether outside-firm LDA fee totals step after the 2011, 2015 and 2019 control flips (anon-2019 lead).
10. Check whether any sector_spend values inherit LobbyView's threshold-value imputation for low-cost reports (Bacik), and decide on trade-association dues deduplication for association-heavy sectors.
11. Move headline inference in the about 11 df refit regressions from HC1 to the CR2 and wild cluster bootstrap rows already printed (Zeileis lead; small-sample case not yet shown for these models).
12. Literature search for any published study of electoral-cycle timing or levels in corporate PAC contributions and in forecasting political giving. The current base has none on either.
