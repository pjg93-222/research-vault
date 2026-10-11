---
type: system
name: "Mentat giving and PAC forecast"
slug: mentat-giving-forecast
components: [mentat-giving-forecast.cycle-model, mentat-giving-forecast.slope-anchor-selection, mentat-giving-forecast.calculator-and-nowcast, mentat-giving-forecast.refit-gates, mentat-giving-forecast.pac-accuracy-attempts, mentat-giving-forecast.seasonal-naive-baseline]
brain_notes: ["C:/Users/pgoff/Documents/brain/actors/giving-forecast-pipeline.md", "C:/Users/pgoff/Documents/brain/projects/forecast-redesign.md", "C:/Users/pgoff/Documents/brain/topics/2026-10-feature-forecast-giving-redesign-giving-by-cycle-live-model.md", "C:/Users/pgoff/Documents/brain/topics/2026-09-feature-forecast-giving-redesign-six-year-lookback-lane.md"]
paper_count: 16
tags: [system, type/system-hub]
---

# Mentat giving and PAC forecast

Forecasts of industry and PAC political giving by election cycle. Covers the R refit (cycle-phase log models, 50/50 growth rule, slope and anchor auto-pick, G0-G9 gates), the console calculator's application rules, the company-PAC nowcast and BLEND, the PAC-to-candidate accuracy attempts under a rolling three-cycle cap, the remainder fix, and the Insights seasonal-naive baseline.

**Brain notes:** [actors/giving-forecast-pipeline](obsidian://open?vault=brain&file=actors/giving-forecast-pipeline); [projects/forecast-redesign](obsidian://open?vault=brain&file=projects/forecast-redesign); [topics/2026-10-feature-forecast-giving-redesign-giving-by-cycle-live-model](obsidian://open?vault=brain&file=topics/2026-10-feature-forecast-giving-redesign-giving-by-cycle-live-model); [topics/2026-09-feature-forecast-giving-redesign-six-year-lookback-lane](obsidian://open?vault=brain&file=topics/2026-09-feature-forecast-giving-redesign-six-year-lookback-lane)

## Mentat giving and PAC forecast: evidence assessment

**Scope.** The hub file lists 6 components and 21 evidence rows covering 16 distinct papers or notes. By status: 6 VERIFIED, 15 VERIFIED-SOFTENED. Several rows contain sub-claims marked unverified, kept as leads below.

**Where the rows fall.** Rows per component: 10 in Cycle-aware giving refit (phase and drift log models, 50/50 growth); 0 in Slope (R0/R1/R2) and anchor auto-pick; 9 in Calculator rules, company-PAC nowcast and BLEND; 2 in Refit gates G0-G9; 0 in PAC-to-candidate accuracy attempts (rolling three-cycle cap); 0 in Insights seasonal-naive baseline and remainder fix. Components with no direct rows are gaps.

**Overall reading.** The literature in the vault strongly supports the system's accounting rules:
- Lobbying in-house expenses and outside-firm income are kept separate and never summed.
- LDA filings are grouped by filing time, and amendments supersede the original.
- Giving and lobbying are treated as distinct measures.

The literature does **not** yet support the system's forecasting choices: the presidential/midterm phase effect on dollars, the 50/50 IBISWorld growth rule, partial pooling of slopes, the pace-nowcast timing, and the 2022 common-shock reading.

Every forecasting paper in the hub is about firm-level lobbying **participation** (Kerr 2014: 92% persistence in a balanced 3,260-firm Compustat panel, 1998-2006; Huneeus & Kim 2018: more than 80% of 2016 lobbiers lobbied again in 2017). None is about sector-level PAC dollars, so transfer to industry PAC-to-candidate giving by cycle is weak throughout.

**Contradictions to fix first.**
1. Paul's reading of Bacik: the verified bearing says its synchronization is in lobbying *targets*, with no election-phase effect on lobbying *dollars*, annual aggregation, and no PACs.
2. The live system already exempts lobbying from the phase adjustment (Phase 6 rulings of 2026-10-01), so the position and the code disagree about lobbying.
3. The "3.63 vs 3.99 MAE" figure is labeled seasonal-naive vs growth in the component question, but the evidence rows describe a last-year naive on LobbyView lobbying targets for 2014-2023, and it was not found under sector_growth_study. Treat it as an unverified lead with the wrong label.

## Component status at a glance
| Component | Rows | Status counts |
|---|---|---|
| Cycle-aware giving refit (phase and drift log models, 50/50 growth) | 10 | 1 VERIFIED, 9 VERIFIED-SOFTENED |
| Slope (R0/R1/R2) and anchor auto-pick | 0 | GAP |
| Calculator rules, company-PAC nowcast and BLEND | 9 | 5 VERIFIED, 4 VERIFIED-SOFTENED |
| Refit gates G0-G9 | 2 | 2 VERIFIED-SOFTENED |
| PAC-to-candidate accuracy attempts (rolling three-cycle cap) | 0 | GAP |
| Insights seasonal-naive baseline and remainder fix | 0 | GAP |

## Cycle-aware giving refit (phase and drift log models, 50-50 growth)

**What it computes.** Projected measures as log models of presidential vs midterm phase, trajectory and inflation. Growth = 50% prior cycle + 50% IBISWorld budget with a one-year lag.

**Method.** Live bundle refit-20261007. Presidential uplift +9% (D-119). Lobbying uses a log refit.
- Correction 2026-10-11 (memory `mentat-forecast-overnight-2026-10-07.md`): D-119's +9% is superseded; the live factors are D-121's (general giving x1.8635 both ways, company PACs x1.09 on the 2027-28 step). See "System records" at the end of this section.

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
| [[ansolabehere-2002-little-money-politics]] | 2002 | qualifies | In the gubernatorial panel, campaign spending scales with per-capita income at an elasticity of 1.07 to 1.17, and government spending adds nothing once income and state FE are included. In aggregate, campaign spending shows no trend relative to national income over 1976-2000, and presidential spending as a share of GDP was flat over 1912-2000. | The paper's evidence is aggregate and state-level spending, not industry-attributed giving. Industry revenue proxies firm size and stakes, not donor income, so the bearing is weak. The elasticity should be attributed to the gubernatorial panel, not to campaign spending in general. | weak | VERIFIED-SOFTENED |
| [[kerr-2014-dynamics-firm-lobbying]] | 2014 | supports | Participation persistence: P(lobby this year given lobbied last year) = 92 percent in a balanced 3,260-firm Compustat panel, 1998-2006 (6.2 percent lobby in a given year; 10.0 percent ever lobby). Above 80 percent in almost all two-digit NAICS industries. 96 percent of expenditures in a typical year come from prior-year lobbyists. Entry clusters in the year before an election, not the election year; exit is unrelated to the cycle. | Evidence is participation for 1998-2006 public firms, not sector dollars. The 96 percent is one sample's expenditure share. Election timing in the paper concerns entry, a small share of dollars, so it does not show phase has zero effect on spend; that stays untested here. | moderate | VERIFIED |
| [[note-request-for-review-industry-economic-performance]] | 2026 | supports | Election-cycle controls use Pres and Midterm dummies with odd years as the baseline. Industry forecasts (IBISWorld, trade outlooks) are used only to predict 2026 spending after estimation and are never mixed into the estimated growth variable. | Supports only the forecast product split between estimation and projection. Does not cover the sector study C3 and L3 tests, and the presidential-difference term is not the memo Midterm and Pres dummy set. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-industry-growth-and-political-spending-research]] | 2026 | qualifies | Predicted industry growth is the variable of direct interest, and the sign of the growth-spending relation is to be estimated, not assumed; IBISWorld forecasts proxy public expectations only and do not establish any company's private forecast or whether a donor read the report. | The plan supplies a design principle, not evidence for the 50/50 term. The rel's claim that Paul ruled roster mismatch a stated limitation does not appear in the plan and is unverified. The build's no-stable-effect reading is single path and UNVERIFIED under DE-062. No backtest of the 50/50 term was checked here. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-review-industry-growth-and-political-spending]] | 2026 | qualifies | Value added, gross output, profits and assets are different quantities; output growth is not spare cash; corporate PAC budgets come from voluntary SSF contributions, so a capacity mechanism fits lobbying more than PAC giving. | The backtest does not isolate any revenue-to-spending link, so it cannot be cited as evidence that the IBIS budget rule is weak. Use only as a fact about the growth term's forecast error in that window. | internal (design doc) | VERIFIED-SOFTENED |
| [[note-sector-growth-and-political-spending-source]] | 2026 | measurement-source | IBISWorld U.S. Key Statistics revenue is in constant prices, so growth ratios need no CPI adjustment. Previous-year growth (k = 1) is one of two actual-growth timings. Predicted growth uses the prior-year forecast for year t; 55 sector-year values, 53 passing the main checks. | The note supplies only the measurement and lag convention. It says nothing about forecasting giving. The analyst's backtest figures (MAE 3.63 vs 3.99; growth beats naive in 45 percent of targets; LobbyView 2014-2023) were not found in the sector_growth_study folder and are removed. The D-113 attribution for the 50/50 weight is unverified. | internal (design doc) | VERIFIED-SOFTENED |

**What the system does.** The live bundle is refit-20261007, with projection M = B exp(phi dP + d_h)(1 + x/2). Here dP is the presidential-cycle difference and x is IBISWorld real growth lagged one year. The growth weight is 50/50 between the prior cycle and the IBISWorld budget. Company PACs take x = 0 (D-116). Lobbying is exempt from the phase adjustment.

**Evidence weight.** There are 10 rows from 10 distinct papers or notes: 1 VERIFIED, 9 VERIFIED-SOFTENED.

**1. The phase effect.** Nothing in the hub measures a presidential-vs-midterm difference in industry PAC dollars net of inflation.
- Bacik et al. 2025 (moderate): the four-year, administration-linked periodicity is in *whom* lobbyists target. The paper reports no phase effect on lobbying dollars, uses annual aggregation, and excludes PACs. It cannot justify a phase coefficient on dollars and says nothing about D-092 (PAC giving).
- Kerr et al. 2014 (moderate): lobbying *entry* clusters in the year **before** an election, not the election year, and exit is unrelated to the cycle. The window is 1998-2006 and the denominator is public firms, so this concerns participation, not sector dollars. Entry is a small share of dollars, so it neither confirms nor refutes the lobbying exemption.
- Bottom line: the PAC phase term (D-092) and the +9% presidential uplift (D-119) rest entirely on Mentat's own fits.

**2. The growth term.** The evidence qualifies it rather than supporting it.
- Ansolabehere, de Figueiredo & Snyder 2002 (weak): campaign spending scales with per-capita income at an elasticity of 1.07-1.17, but in a *gubernatorial* state panel. Aggregate spending shows no trend relative to national income over 1976-2000. Industry revenue is not donor income.
- Review note (moderate): output growth is not spare cash. Corporate PAC budgets come from voluntary SSF contributions, so a capacity mechanism fits lobbying better than PAC giving. This is consistent with D-116 dropping x for company PACs and implies the term should be questioned for general giving too.
- Research-plan note: the sign of the growth-spending relation is to be *estimated, not assumed*. The 50/50 rule assumes a positive half-weight.
- Source-check note: confirms only the measurement (constant-price revenue, k = 1 lag; 55 sector-year values, 53 passing the main checks). The D-113 attribution for the 50/50 weight is unverified.
- Bombardini & Trebbi 2020 (filed under calculator): elasticity of lobbying expenditure to firm sales is about 1, conditional on lobbying. Firm-level and lobbying-only; it does not cover PACs.

**3. Specification departures.**
- The review-request memo specifies Pres and Midterm dummies with an odd-year baseline and keeps industry forecasts out of estimation. The live model keeps forecasts out of estimation, but uses a presidential *difference* dP, not the memo's dummy set.
- The sector study puts predicted_growth_pct on the right-hand side in C3 and L3, which the memo prohibits.
- Zeileis (weak): with about 11 df, HC1 standard errors are suspect; HC3 is the small-sample default. The refit prints CR1, CR2 and a wild cluster bootstrap beside HC1, so read the CR2/bootstrap rows.

**4. Counterexamples.**
- Goff (n.d.) projects crypto giving above $175M for 2025-26 vs $145.5M for 2023-24, an off-year rise. Its window and method are unstated, so it is a test case, not evidence.
- If crypto ever runs through general_giving, the code's u = 1.8635 factor on both steps would scale the midterm step by about -46%, the opposite direction.
- Chohan 2025: Fairshake's $203M in 2024-cycle receipts is press-sourced. Mentat's registry correctly declines crypto for now.

**Contradictions.**
- Paul cites Bacik et al. for 'lobbying synchronizes with the election cycle and more money is given in election years'. The verified bearing says Bacik's synchronization is in lobbying targets (agency blocks by administration), with no reported election-phase effect on lobbying dollars, annual aggregation, and no PAC data.
- Paul's position applies the cycle to lobbying, but the live system exempts lobbying from the midterm/presidential phase adjustment (Phase 6 rulings 2026-10-01, per the Kerr row).
- Kerr et al. 2014 find lobbying entry clusters in the year BEFORE an election, not the election year (participation, public firms, 1998-2006). This cuts against an election-year spending peak for lobbying, though entry is a small share of dollars.
- The method states a presidential uplift of +9% (D-119), but the Goff row reports the code applies u = 1.8635 to general_giving on both cycle steps (giving_forecast.py about line 1425), scaling the midterm step by about -46%. These may be different measures, but the hub does not reconcile them.
- The review-request memo specifies Pres and Midterm dummies with an odd-year baseline; the live model uses a presidential difference dP. The sector study's C3/L3 put predicted growth on the right-hand side, which the memo prohibits.
- The research-plan note says the sign of the growth-spending relation must be estimated, not assumed; the 50/50 rule fixes a positive half-weight a priori.

**Gaps.**
- No study in the hub measures industry PAC giving in presidential vs preceding midterm cycles, net of inflation. Fill with an FEC panel of industry PAC-to-candidate totals by two-year cycle (e.g., 2004-2024), estimating the phase contrast with industry and trend controls, stratified by industry or labeled UNSTRATIFIED if pooled.
- No out-of-sample test isolates the IBISWorld 50/50 growth term against the same model with x = 0, for general giving. Fill with a rolling-origin backtest naming its target window, horizon and number of industries.
- Furnas, LaPira & Brock 2025 and Huneeus & Kim 2018 are cited by Paul for this component, but neither has a bearing on the cycle/phase question in the hub.
- No source attributes the 50/50 weight to D-113; the attribution is marked unverified.

**Paul's position vs the evidence.**

Paul's stated position is that lobbying synchronizes with the election cycle and more money is given in election years, citing Bacik 2026 (the hub's Bacik is 2025), Furnas, LaPira & Brock 2025 and Huneeus & Kim 2018.

- Bacik does not support the dollar claim. Its synchronization is about which agencies are targeted, by administration. It reports no phase effect on dollars and covers no PACs.
- No hub row shows Furnas/LaPira/Brock bearing on cycle timing. Huneeus & Kim speak to persistence, not phase.
- Kerr points the other way on timing. Entry peaks in the pre-election year, not the election year.
- The system has already diverged from the position for lobbying, which it exempts from the phase adjustment.

For PAC giving, 'more money in presidential years' is plausible and is what D-092 encodes, but the only support is Mentat's own backtests, not the cited literature.

**Open questions.**
- Is the +9% presidential uplift (D-119) a pooled coefficient across industries? If so, quote it per industry or label it UNSTRATIFIED.
- How do D-119's +9% and the code's u = 1.8635 on general_giving relate? Different measures, or is one stale?
- Does the growth term x improve general-giving forecasts over x = 0 out of sample? Over what target window and how many industries?
- Should general giving also drop x, given the SSF-capacity argument that justified D-116 for company PACs?
- If crypto is ever admitted, would its giving route through general_giving and inherit the both-steps presidential factor?

**Read first.** [[bacik-2025-measuring-dynamical-evolution]], [[kerr-2014-dynamics-firm-lobbying]], [[note-review-industry-growth-and-political-spending]], [[note-industry-growth-and-political-spending-research]], [[ansolabehere-2002-little-money-politics]], [[note-request-for-review-industry-economic-performance]]

**System records (brain fleeting pass, added 2026-10-11).** Internal records of what the live system does and how it scored, taken from lane memories; they are not literature and are not counted in the paper-row tallies above. Brain note for all rows: [topics/2026-10-feature-forecast-giving-redesign-giving-by-cycle-live-model](obsidian://open?vault=brain&file=topics%2F2026-10-feature-forecast-giving-redesign-giving-by-cycle-live-model).

| Source | Date | Relation | Record shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| memory `mentat-forecast-overnight-2026-10-07.md` | 2026-10-07 (memory synced 2026-10-09) | internal (system record) | Key model facts, which the memory says were verified by an independent Opus re-fit. Growth input = average annual IBIS revenue growth over the lag window (2026 <- 2023-25; 2028 <- 2025-27), and IBIS growth is REAL. The page sends `growth_basis: "ibis_window_avg"` and the server refuses unmarked growth (stale-page guard). phi_gen 0.738 (the midterm is lower). Company-PAC phi is about 0 (-0.025), which is why D-118/D-119 add an uplift. The Senate presidential term is kept by owner waiver: Senate MAE is 1.32-1.76x worse, inside the 1.25-2.23x range Paul was told. | Describes the live model, not the world. It states the growth input more precisely than the "one-year lag" wording above (a window average of real IBIS growth). The memory gives no fit window or industry count for phi_gen or the company-PAC phi; quote them with that caveat. | internal (lane memory) | VERIFIED |
| memory `mentat-forecast-overnight-2026-10-07.md` | 2026-10-08 | internal (system record) | D-118: Paul's rule that a presidential cycle always projects higher. D-119 (+9%) was superseded by D-120 (x1.80 company PACs, OpenSecrets total cost), which D-121 superseded in turn. D-121, the final rule: general giving x1.8635 both ways (OpenSecrets presidential average $12.56bn vs midterm average $6.74bn) and company PACs x1.09 on the 2027-28 step. Built, live and committed (CHANGES.md entry 15; factor constants in `mentat/app/giving_forecast.py`, first committed in 8e3a85c10). D-120 is not in force. | Answers the open question above on D-119's +9% vs the code's u = 1.8635: the +9% is superseded, 1.8635 is D-121's general-giving factor (12.56 / 6.74 = 1.8635, arithmetic) and 1.09 the company-PAC factor. The factor is an owner rule taken from national OpenSecrets averages, not an industry estimate, so it is UNSTRATIFIED; the memory names neither the cycles averaged nor their denominator. The literature gap on the phase effect stands. | internal (lane memory) | UNVERIFIED |
| memories `mentat-forecast-overnight-2026-10-07.md`, `mentat-pdf-export-and-worksheet-cache-2026-10-08.md` | 2026-10-07 to 2026-10-08 | internal (system record) | Blind results (`blind\RESULTS.md`, `blind\panel_RESULTS.md`): the 50/50 overlay lowered the blind score in 12 of 16 comparisons. Paul was told plainly (2026-10-08): cycle-ahead general giving blind A = 66.1 for banks (biased high); the 97.8 is the in-cycle PAC nowcast; never conflate them. | Bears on the gap above on testing the 50/50 growth term out of sample, as a lead only: the memory does not say which measures, horizons or windows the 16 comparisons cover. The 66.1 is a cycle-ahead score for one industry (banks) and must not be compared with in-cycle nowcast scores. | internal (lane memory) | UNVERIFIED |

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

**GAP: no evidence rows.**

**What the system does.** It chooses among a pooled slope (R0), own industry slopes (R1) and a DerSimonian-Laird partially pooled slope (R2). The same-phase anchor is used only when it wins by 5% in both backtest designs. The reported result is that own slopes beat the blend, LOEO 10.8 vs 14.3. The hub does not restate that figure's window or the number of industries and elections in its denominator, so it must not be quoted bare.

**Methodological cautions** (general methods knowledge, not hub evidence).
- DerSimonian-Laird is known to estimate between-group variance poorly with few groups or few observations per group. Paule-Mandel/REML with Hartung-Knapp intervals are common alternatives.
- LOEO is noisy: it runs over a handful of elections, so a gap like 10.8 vs 14.3 may not be stable.
- Choosing a rule because it wins both backtests by 5% is itself a model-selection step; its out-of-sample error is optimistic unless the selection is nested inside the backtest.
- R0 and the blend pool across industries, a known fault line. Any pooled slope reported should carry industry strata or be labeled UNSTRATIFIED.

**Adjacent hub evidence (indirect).** Kerr 2014: entry-heavy sectors break naive anchors. Kerr 2014 and Huneeus 2018: incumbent persistence is high. Together these suggest industries differ in regime, which argues for own slopes in incumbent-dominated sectors and fits the finding that own slopes beat the blend. These rows are filed under other components.

**Contradictions.**
- The component's empirical question presumes partial pooling should help small-sample sectors. The system's own result (own slopes beat the blend, LOEO 10.8 vs 14.3) runs the other way. No external evidence in the hub adjudicates.

**Gaps.**
- No study on partial pooling of industry-level political-spending trend slopes. Fill with a nested rolling-origin backtest comparing no-pool, full-pool and empirical-Bayes/REML partial-pool slopes, error per industry, window and industry count stated, auto-pick rule refit inside each fold.
- No evidence that the 'wins both backtest designs by 5%' rule generalizes. Fill with a simulation or held-out-cycle check of the selection rule itself.
- No literature on same-phase vs previous-cycle anchors for two-year electoral series.

**Paul's position vs the evidence.**

Paul states no literature, and the hub has none. The design is defensible on internal backtests only. The current 'own slopes win' result is consistent with the indirect persistence/heterogeneity evidence (Kerr, Huneeus), but rests on a small LOEO comparison whose window and denominator are not stated in the hub.

**Open questions.**
- What are the window, number of elections and number of industries behind LOEO 10.8 vs 14.3?
- Is the anchor/slope auto-pick performed inside each backtest fold, or once on the full sample?
- Would a REML or Paule-Mandel tau-squared estimator change the R2 result relative to DerSimonian-Laird?

**Read first.** [[kerr-2014-dynamics-firm-lobbying]], [[huneeus-2018-effects-firms-lobbying]], [[zeileis-nd-econometric-computing-with]]

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

**Evidence weight.** There are 9 rows from 7 distinct papers or notes: 5 VERIFIED, 4 VERIFIED-SOFTENED. All of them bear on the **accounting** rules. None bears on the **pace nowcast** itself.

**1. Strongly supported: the no-sum rule for lobbying.**
- Harrison et al. 2018 (strong): the federal LD-2 XML amounts do not separate income from expenses. A form-level fact that transfers fully to current Senate LDA data. It supports D-064 but does not measure the overlap.
- Bacik et al. 2025 (moderate): firms report income and self-filers report expenses, under different thresholds. Bacik also imputes a threshold value for low-cost reports as an upper bound. If any sector_spend values come from LobbyView, check for that imputation.
- Review note (moderate): in-house expenses include retained-firm payments and association dues, so summing double-counts. Trade-association overlap is not yet handled separately.
- Sector-study config: records a median 9-26% double count by sector in undifferentiated LobbyView sums.

**2. Supported: filing-time grouping and amendment handling.**
- Harrison 2018: LDA records group by filing time, not activity time; late filings for 2002-2008 appeared in Q4 2009 files.
- Mentat is stricter than the paper: it keeps the latest dt_posted per (registrant, client, year, period), so amendments supersede originals, and applies the D-130 quarter-coverage rule.

**3. Supported but weak: keeping giving and lobbying as separate models.**
- Furnas (n.d.) (weak): PAC giving is lumpier and often one-off; lobbying is concentrated between $10K and $1M and spans months.

**4. Context only.**
- Bombardini & Trebbi (Bertrand et al.): the in-house share of *lobbyists* fell from about 60% to under 40% over 1999-2008, a headcount share, not dollars.
- Bombardini (Kerr): 92% persistence is firm participation, not industry dollars.
- Furnas et al. 2019 (weak): House control flips move revenue *between* partisan contract firms; the effect is distributional and does not show a sector-total step.
- Cross-industry memo (weak): credit-union money is PAC- and association-led, supporting the 16-PAC cohort (D-117) as the unit, but it has no figures to check the pinned D/R split.

**5. Not addressed: the pace nowcast.** The nowcast is O + (1-s)B with s the geometric mean of pace shares, and it overrides BLEND (D-099) via D-116. No evidence in the hub speaks to when in the cycle a pace nowcast beats the model.

**Contradictions.**
- None between evidence and code on the accounting rules. One minor provenance caveat: the side-by-side lobbying display claim in the review-note row comes from a Melange commit message and was not read in Mentat code. The Harrison row clarifies that the telecom 2025 de-summing ($139.4M -> $102.4M in-house + $37.0M outside-firm fees) was a Melange industry-page change (commit 914623152), not a change to Mentat's card.

**Gaps.**
- No evidence on within-cycle pace nowcasting: at what month a pace-share nowcast beats the model forecast. Fill with a backtest over past cycles (e.g., 2014-2024) scoring nowcast vs model at each month, per measure and industry, stating target window and denominator.
- No evidence on the geometric-mean pace-share construction vs alternatives (median, last-cycle share).
- Trade-association lobbying overlap is identified as a double-count risk but has no measured size in the hub.
- No test of whether outside-firm sector totals step after the 2011, 2015 and 2019 House control flips (the Furnas 2019 row recommends this before any control term is added).

**Paul's position vs the evidence.**

Paul states no literature, but the evidence is stronger here than in any other component. Harrison 2018, Bacik 2025 and the review note independently support the in-house/outside never-sum rule (D-064) and filing-time handling, and Paul can cite them. The nowcast's core empirical bet, that within-cycle pace is informative, remains uncited and untested by any external source.

**Open questions.**
- Do any sector_spend values come from LobbyView, and do they carry Bacik's low-cost-report threshold imputation?
- How are trade-association dues inside in-house expenses handled for sectors with large associations (e.g., credit unions via ACU)?
- Is the D/R split pinned to the 16-PAC cohort stable across cycles?

**Read first.** [[harrison-2018-application-data-mining]], [[bacik-2025-measuring-dynamical-evolution]], [[note-review-industry-growth-and-political-spending]], [[furnas-nd-conflating-lobbying-pacs]], [[bombardini-2020-empirical-models-lobbying]], [[furnas-2019-partisan-ties-lobbying]]

**System records (brain fleeting pass, added 2026-10-11).** Internal records, not literature; not counted in the paper-row tallies above. Brain note for both rows: [topics/2026-10-feature-forecast-giving-redesign-giving-by-cycle-live-model](obsidian://open?vault=brain&file=topics%2F2026-10-feature-forecast-giving-redesign-giving-by-cycle-live-model) (History, 2026-10-08, and "Company PACs on the panel nowcast").

| Source | Date | Relation | Record shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| memory `mentat-pdf-export-and-worksheet-cache-2026-10-08.md` | 2026-10-08 | internal (system record) | The lobbying projection was replaced by an LDA quarterly NOWCAST (7f13e9247), on Paul's rulings "1 yes, 2 remaining, 3 up-to-3, 4 yes, 5 b": pred = O + (1-s)B, s = geometric mean of the same-quarter share over up to 3 prior years; 80% band = pooled blind errors (bank/CU/telecom/fintech, n=32; log half-widths Q1 .132, Q2 .099, Q3 .041); "~95%" = the largest blind miss, labelled n=32; 2027-28 chained with LobbyView persistence in quadrature. | Extends the O + (1-s)B pace form from company PACs to lobbying. The band is empirical but pooled over four sectors and 32 blind errors, so it is UNSTRATIFIED; the memory names no quarter window for those errors. It does not supply the external evidence the gap above asks for on when a pace nowcast beats a model. | internal (lane memory) | UNVERIFIED |
| memory `mentat-forecast-overnight-2026-10-07.md` | 2026-10-07 to 2026-10-08 | internal (system record) | Blind results (`blind\RESULTS.md`, `blind\panel_RESULTS.md`): the panel nowcast passes 19 of 20 sector x cut cells; P&C month 12 (89.36) fails. Banks company PACs month 18: nowcast 97.8, BLEND 90.2, pace 96.0, model without 50/50 92.6. Credit unions: one PAC 70.5, 16-PAC cohort 94.9 (about 90% one trade association). These sit behind D-116 (company PACs on the panel nowcast) and D-117 (credit unions on the panel's 16 PACs). | In-cycle nowcast scores; not comparable with the cycle-ahead general-giving A of 66.1 (cycle section). The memory does not restate the scoring window or the metric's denominator. The CU cohort being about 90% one trade association fits the cross-industry memo row above (association-led CU money). | internal (lane memory) | UNVERIFIED |

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
| [[kerr-2014-dynamics-firm-lobbying]] | 2014 | supports | Lagged lobbying status raises current-year lobbying probability by 0.84 to 0.88 in Blundell-Bond columns 2-6 (baseline column 2 = 0.8848; column 6 = 0.8810), 26,080 firm-years, 1998-2006 public firms (Table 2 and note). Table 7 Panel E: 0.00 entry 2004-2006 among the most dependent firms not lobbying in 2001-2003, on both LCA and patenting measures. Entry is not zero elsewhere: 4th-quintile LCA non-lobbyists show 0.08, and Nike entered without prior lobbying (fn 26). | Persistence evidence is for binary participation, not dollars. The naive-anchor result for dollars holds only while incumbents dominate sector totals; entrant-heavy sectors (crypto before 2015, fintech) break that. The no-entry result holds only for the top dependency quintile, not all dependent firms. | moderate | VERIFIED-SOFTENED |

**Evidence weight.** There are 2 rows from 2 distinct papers or notes: 2 VERIFIED-SOFTENED. Both are about lobbying persistence, not about the gate criteria themselves.

**What the rows say.**
- Kerr et al. 2014 (moderate): lagged lobbying status raises current-year lobbying probability by 0.84-0.88 (Blundell-Bond; 26,080 firm-years; 1998-2006 public firms). In Table 7 Panel E the most dependent non-lobbying firms show zero entry; entry elsewhere is not zero (0.08 in the 4th LCA quintile).
- Huneeus & Kim 2018 (moderate, context): more than 80% of 2016 lobbiers lobbied again in 2017. The adjacent-year intensive-margin log slope is 0.922, conditional on lobbying in both years (sample 1999-2018). Persistence dropped in the 2007-2009 crisis.

**What follows.** Strong persistence makes naive a hard baseline, consistent with G9's warning that the log-lobbying refit (MAE 4.050) does not beat naive (3.936; README, pinned in test-08). That is the analyst's inference, not either paper's claim. Neither paper addresses forecasting, and both are firm-level participation or intensity, not sector aggregates.

**Caveats.**
- Naive anchoring holds only while incumbents dominate sector totals. Entrant-heavy sectors (crypto before 2015, fintech) break it.
- Whether the 2007-2009 break sits in training data for later origins was not checked.
- Leave-one-election-out MAE as a criterion has no external support or critique in the hub. With few elections, the G6 rule is a many-comparisons gate whose false-fail and false-pass rates are unknown.
- Unverified lead: the sector-study triple (MAE 3.63 / 3.99 / 6.55 with control terms) was not found under sector_growth_study.

**Contradictions.**
- None with the gates' logic. One provenance conflict: the sector-study backtest triple (3.63 vs 3.99 vs 6.55) is referenced in other rows and in a brain note, but was not found in any file under sector_growth_study. It must not be used as gate evidence until located.

**Gaps.**
- No evidence on whether refits that pass G6 forecast the next cycle better than the published calculator. Fill with a prospective, one-look comparison: record G6-passing refits and the calculator at each origin, then score both on the next realized cycle.
- No study of LOEO MAE's reliability as a gate with few elections. Fill with a simulation of G6 false-fail/false-pass rates under the observed number of elections and industries.
- Waivers are applied, but no criterion for granting a waiver is evidenced.

**Paul's position vs the evidence.**

Paul states no literature. The persistence evidence (Kerr, Huneeus) is consistent with the system's own finding that the lobbying refit does not beat naive (4.050 vs 3.936), pointing toward the naive/nowcast route already taken for lobbying. Nothing in the hub validates LOEO MAE as the publication criterion.

**Open questions.**
- What target window and how many industries/elections underlie the 4.050 vs 3.936 log-lobbying MAE?
- What is the backtest start year, and does the 2007-2009 persistence break fall in training data for later origins?
- Which waivers were applied in refit-20261007, and on what grounds?
- Where do the 3.63 / 3.99 / 6.55 figures come from?

**Read first.** [[kerr-2014-dynamics-firm-lobbying]], [[huneeus-2018-effects-firms-lobbying]], [[zeileis-nd-econometric-computing-with]], [[note-review-industry-growth-and-political-spending]]

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

**GAP: no evidence rows.**

**What the system does.** It scores ORIGIN and nowcast forecasts of PAC-to-candidate giving over 2018-2024, under a rolling three-cycle cap. The bar is A >= 90 overall and >= 75 per phase. In v6 (ORIGIN B1 plus N/(N+1) cross-sector pace shrinkage), only telecom passes. A common 2022 drop defeats window rules (DE-066/067).

**Literature.** None in the hub addresses whether 2022 was a common shock, accuracy bars for PAC forecasts, or cross-sector shrinkage.

**Adjacent rows (indirect, filed elsewhere).** Furnas (n.d.): PAC giving is uneven and often one-off, implying high irreducible error per PAC, which makes a 90% bar demanding. Review note: PAC budgets come from voluntary SSF contributions, bounding what revenue- or capacity-based predictors can add.

**A candidate explanation for 2022.** Many corporate PACs announced pauses in candidate giving after January 2021, and 2022 was also the first cycle after redistricting. Both would produce a cross-sector 2022 dip. These are hypotheses from general knowledge, **not hub evidence**, offered as leads to test.

**Contradictions.**
- (none)

**Gaps.**
- No study tests whether the 2022 drop in industry PAC-to-candidate giving was a common cross-sector shock. Fill with a variance decomposition (cycle fixed effects vs sector-by-cycle residuals) on FEC PAC-to-candidate receipts by sector, 2014-2024, stating the sector count.
- No external evidence on the post-January-2021 corporate PAC pauses as the cause. Fill with an event-study on corporate PAC disbursements by announced-pause status.
- No literature on forecast-combination or shrinkage for short electoral panels to justify N/(N+1) cross-sector pace shrinkage. Fill with a methods review plus a nested backtest under the three-cycle cap.
- No basis in the hub for the A >= 90 / >= 75 per-phase bar.

**Paul's position vs the evidence.**

Paul states no literature, and the hub has none. Paul's directive that gains must come from method, not more history (the rolling three-cycle cap), is a design constraint, not an empirical claim, and nothing in the evidence argues for or against it. If 2022 was a single common shock, it is exactly what a three-cycle window cannot learn from; an explicit common-shock term is the method-side response consistent with the cap, but remains to be tested.

**Open questions.**
- What is metric A precisely, and what denominator (PACs, candidate-PAC pairs, dollars) does the >= 90 bar use?
- Does the 2022 drop appear in all sectors, or only in sectors with prominent corporate PAC pauses?
- Why does telecom pass when others do not? Cohort size, pause exposure, or lower volatility?

**Read first.** [[furnas-nd-conflating-lobbying-pacs]], [[note-review-industry-growth-and-political-spending]], [[anon-2025-cross-industry-political]], [[chohan-2025-crypto-oligarchy-cryptocurrency]]

**System records (brain fleeting pass, added 2026-10-11).** Internal records, not literature; not counted in the paper-row tallies above. Brain note for both rows: [topics/2026-09-feature-forecast-giving-redesign-attempt3-all-sector-v6](obsidian://open?vault=brain&file=topics%2F2026-09-feature-forecast-giving-redesign-attempt3-all-sector-v6) ("Pipeline run", "Results", "The ORIGIN dead end").

| Source | Date | Relation | Record shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| memories `pac-accuracy-attempt3-lane-2026-10-05.md`, `attempt3-build6-state.md` | 2026-10-05 | internal (system record) | v6 was scored once: GATE6C PASS, then SCORE6B ran one clean FIRST_LOOK pass (lock engaged, so any later pass is RESCORE); REVIEW6 ACCEPT_WITH_NOTES. Results against bar E1 (sector >= 90, each phase >= 75): banks ORIGIN 86.11 FAIL, m12-L meet (93.65-98.00); SchoolsFirst only m21 meets (92.53); P&C ORIGIN 85.16 / m12 89.02 FAIL, m15+ meet (m15 fragile); credit arms ORIGIN 89.67 FAIL, m12+ meet (m12 fragile); telecom all meet (below persistence at m12/m15); crypto, fintech and community banks UNSCORABLE. | Development numbers: the memory says they are RESCORE / FIRST_LOOK_DEV and never certify; clean evidence comes only from the 2026 live lines and the 2028-2034 prospective lines. The brain note adds that REVIEW6 recomputed 17 cells, all matching. | internal (lane memory) | UNVERIFIED |
| memory `pac-accuracy-attempt3-lane-2026-10-05.md`; brain DE-066, narrowed by DE-067 | 2026-10-05 | internal (system record) | The ORIGIN dead end. The memory: ORIGIN cannot reach 90 from giving history (2022 common shock), the STRAT3 §1.2 bound. After Paul's review the claim was narrowed to what the proof covers (brain note wording): "PAC giving ORIGIN from giving-history window-range rules (and the summary-file arm) cannot reach A >= 90 for banks, SchoolsFirst, P&C under the six-year rolling lookback (2018-2024)". Not covered: an extrapolating predictor, credit arms (89.67) and telecom (meets with B1). | A bound on rules that never predict outside their window's lowest-to-highest level, not an empirical account of why 2022 fell; the corporate-pause and redistricting explanations above stay leads. It points method work at extrapolating predictors or an explicit common-shock term, not at window-range rules. | internal (lane memory) | VERIFIED-SOFTENED |

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

**GAP: no evidence rows filed directly.**

**What the system does.** The Insights baseline repeats the same-phase prior cycle and is withheld when fewer than 4 complete cycles exist. The remainder fix (a conditional additive remainder) replaced subtraction after 7 of 19 shares had gone negative.

**The periodicity claim.** Paul cites Bacik for four-year electoral periodicity. The only Bacik bearing in the hub (filed under the cycle model) says the periodicity is in lobbying *targets*, by administration, and does not cover dollars or PACs. It supports a four-year rhythm in *what is lobbied*, not a four-year seasonal pattern in giving or lobbying amounts.

**Bearing on lobbying.** Kerr and Huneeus show strong year-over-year persistence, favoring a *last-year* naive for lobbying over a same-phase naive, consistent with the system exempting lobbying from phase.

**The quoted 3.63 vs 3.99 MAE.** The component's own empirical question misattributes it:
- The evidence rows describe it as last-year naive vs growth-augmented on LobbyView lobbying targets, 2014-2023, not same-phase seasonal naive on giving.
- Two rows report it was not found in any sector_growth_study file.
- It is an unverified lead and does not test this component.

**The remainder fix.** An arithmetic correction; it needs no literature, only a regression test that shares stay non-negative.

**Contradictions.**
- The empirical question frames 'seasonal naive beats growth-augmented models (3.63 vs 3.99 MAE)', but the evidence rows identify that comparison as last-year naive vs growth on LobbyView lobbying targets (2014-2023), not same-phase seasonal naive on giving. The figures were also not found in the sector_growth_study files.
- Paul cites Bacik for four-year electoral periodicity of the series; Bacik's periodicity is in lobbying targets by administration, with no reported phase effect on dollars.

**Gaps.**
- No study shows same-phase (four-year) seasonal naive outperforms last-cycle naive or growth-augmented models for industry PAC giving. Fill with a rolling-origin comparison of seasonal-naive, last-cycle naive and model forecasts on FEC industry giving, per industry, stating target cycles and industry count.
- No evidence on whether the four-complete-cycle withholding threshold is the right minimum. Fill with an error-vs-history-length curve from the same backtest.
- Locate or regenerate the 3.63 / 3.99 MAE figures with code, window and denominator before citing them anywhere.

**Paul's position vs the evidence.**

Paul's position is that election-cycle periodicity is highlighted in Bacik. The verified reading is narrower: Bacik shows administration-linked periodicity in lobbying targets, not in amounts and not in PAC giving. The persistence literature (Kerr, Huneeus) favors last-year naive for lobbying. For PAC giving the four-year seasonal assumption is plausible but unevidenced in the hub, and the MAE figure cited in support tests a different baseline on a different series.

**Open questions.**
- Which series does the Insights seasonal-naive baseline apply to: giving, lobbying, or both?
- Does a same-phase naive beat a last-cycle naive on industry PAC giving out of sample?
- Where are the 3.63 / 3.99 figures generated, and over what window and denominator?

**Read first.** [[bacik-2025-measuring-dynamical-evolution]], [[kerr-2014-dynamics-firm-lobbying]], [[huneeus-2018-effects-firms-lobbying]], [[note-review-industry-growth-and-political-spending]]

## Across components

**1. Accounting is evidenced; forecasting is not.** Every VERIFIED or strong bearing concerns *measurement*: LD-2 income vs expenses, filing-time grouping, amendment supersession, giving vs lobbying as distinct measures. Every *forecasting* choice (phase term, 50/50 growth, slope pooling, pace nowcast, seasonal naive, the 2022 shock) rests on Mentat's own backtests. Paul marks four components 'No literature'; that is accurate for the forecasting parts but understates support for the calculator's accounting rules.

**2. Firm participation is not sector dollars.** The persistence results (Kerr 2014: 92%; Huneeus & Kim 2018: more than 80% re-lobby rate; Blundell-Bond 0.84-0.88) are firm-level, public-firm, lobbying-only, never PACs. They favor naive, last-period anchors for **lobbying**, which matches what the system does (lobbying exempt from phase, G9 shows the refit does not beat naive, in-year nowcast). They say nothing about **PAC** forecasting.

**3. The Bacik citation is overloaded.** Paul uses Bacik for both the cycle model and the seasonal baseline, but the one verified reading supports administration-linked periodicity in lobbying *targets* only.

**4. The growth term and capacity.** The SSF-capacity argument justified D-116 (x = 0 for company PACs). The only revenue-to-spending elasticity in the hub (about 1) is firm-level *lobbying*. The sign should be estimated, not fixed at a 50/50 positive weight, and the lobbying refit does not beat naive, so the term is unproven even there.

**5. One provenance hole runs through three components.** The '3.63 vs 3.99 MAE' (and the 6.55 control-term figure) is cited in the seasonal-naive question, the refit-gates row and the review-note row, described inconsistently, and not found under sector_growth_study. Resolve it once, centrally.

**6. Pooling across a known fault line.** R0 pooled slopes, the D-119 +9% uplift and the N/(N+1) cross-sector pace shrinkage may pool industries that differ in regime (incumbent-dominated vs entrant-heavy, per Kerr). Quote them per industry or label UNSTRATIFIED.

**7. Small-sample inference.** About 11 df in refit regressions and few elections in LOEO make HC1 SEs and LOEO MAE gaps fragile; report CR2 and the wild bootstrap.

No evidence row argues for either of the two REJECTED SNA proposals, and none uses LDA as an edge or weight source.

## Research agenda

1. Locate or regenerate the '3.63 vs 3.99 (and 6.55) MAE' backtest with code, target window (LobbyView 2014-2023), horizon and denominator; correct the seasonal-naive empirical question.
2. Estimate the presidential-vs-midterm contrast in industry PAC-to-candidate giving net of inflation on an FEC panel (e.g., 2004-2024), per industry with stated industry count, to test D-092 and D-119. Reconcile D-119's +9% with the code's u = 1.8635 factor.
3. Run a nested rolling-origin backtest of general giving with x (IBISWorld 50/50 growth) vs x = 0, estimating the sign rather than fixing it. Report per industry.
4. Decompose the 2022 PAC-to-candidate drop into a cross-sector cycle effect vs sector residuals (2014-2024), and event-study the post-January-2021 corporate PAC pauses. This explanation is a lead, not hub evidence.
5. Score the pace nowcast vs the model forecast month by month within past cycles to find the crossover month, per measure and industry.
6. Compare seasonal (same-phase) naive vs last-cycle naive vs model forecasts for industry PAC giving out of sample, and plot error against history length to test the four-complete-cycle threshold.
7. Test slope pooling with a nested backtest (no-pool / full-pool / REML or Paule-Mandel partial-pool, auto-pick inside each fold). State the window and industry and election counts behind LOEO 10.8 vs 14.3.
8. Simulate G6 false-fail and false-pass rates under the observed number of elections and industries, then run a prospective one-look test of whether G6-passing refits beat the calculator on the next realized cycle.
9. Test for a step in outside-firm lobbying sector totals after the 2011, 2015 and 2019 House control flips before considering any control term.
10. Measure trade-association overlap inside in-house LDA expenses for association-led sectors (e.g., credit unions via ACU), and confirm whether any LobbyView-sourced values carry Bacik's low-cost-report threshold imputation.
