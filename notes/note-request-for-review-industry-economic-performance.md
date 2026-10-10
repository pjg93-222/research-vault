---
type: research-note
title: "Request for Review: Industry Economic Performance and Political Spending"
author: Paul
year: "2026"
category: "Paul's Research Notes"
systems: ["mentat-giving-forecast", "mentat-sector-growth-study"]
components: ["mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.seats-weekly-test", "mentat-giving-forecast.cycle-model"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/note-request-for-review-industry-economic-performance-source.md]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\Astra_Review_Brief_Sector_Growth_and_Political_Spending.md"]
sha12: ["bc2c16a312a8"]
tags: [research-note, paul-s-research-notes, type/research-note]
---

# Request for Review: Industry Economic Performance and Political Spending

**Paul Goff (2026).** *Research note, Georgetown University affiliation, dated September 24, 2026, addressed to an advisor.* Theme: [[themes/paul-s-research-notes|Paul's Research Notes]].

> [!abstract] Summary
> Paul's September 24, 2026 design memo to a secondary advisor (Astra) asks whether a sector's year-over-year economic performance predicts its federal campaign giving and lobbying, and how expected partisan control shapes the amount and direction of that money. It reports no estimates. It sets out five hypotheses (deep pockets, distress, access-seeking, stakes, lobbying tracks current power) and three pooled sector fixed-effects models. Model 1 is log contributions on prior- and same-year growth together (a distributed lag), a negative-growth dummy, Midterm/Pres, a Tossup term and a trend. Model 2 is Republican two-party share on Shift = P minus RMajNow. Model 3 is log lobbying with a lagged dependent variable and Unified control. Inference is a wild sector-cluster bootstrap with Driscoll-Kraay errors. The proposed panel is 2008-2025 with five sectors, about 9 elections and 6 chamber flips. The memo is the method source for the Mentat sector growth study. The R build diverges from it. Each growth timing enters its own model (C1/C2/C3) rather than one distributed lag. Totals carry exp_dem controls (per-year IEM means, a binary forecaster final call, or a market snapshot) where the memo calls for Tossup. There is no negative-growth dummy, trend or lagged lobbying term. The primary window is 2020-2025 (three elections), not 2008-2025. Results so far are null or fragile: the core annual partisan SHIFT tests are not estimable in report 221444 (only a below-minimum exploratory preview exists), and the lobbying growth lead flips sign under year FE (-0.477, not significant, p 0.15). The memo's leverage worries therefore apply to the build with more force. The within-cycle access-seeking signal, SEATS-W (House 2022, 17 weeks, regressor the prior-week change in 538 Lite expected Democratic seats), is -3.36 pp per seat in the pooled-dollar primary row (p 0.11 and 0.10, not significant) and -3.47 pp in the equal-sector variant (p 0.0086 and 0.0174); other House rows are also negative, and all rest on one election.

## About this note

Category: Paul's Research Notes. Original path: `C:\Users\pgoff\Downloads\Astra_Review_Brief_Sector_Growth_and_Political_Spending.md`.

## Study

**Question.** Does a U.S. industry's year-over-year economic performance predict how much it spends on federal politics, through campaign contributions and through lobbying? How does expected partisan control of Congress shape both the amount and the partisan direction of that spending?

**Theory.** Competing accounts: deep pockets (H1, better years mean more spending); distress (H1-alt, bad years mean more lobbying for relief, as in 2008-09 bailouts); access-seeking (H2, contributions shift toward the party expected to hold the majority); stakes (H3, total contributions rise when a chamber is a toss-up); lobbying tracks current power (H4). Majority status is treated as worth access (committee chairs, agenda control), citing Cox and Magar 1999. Section 8 cites Grier, Munger and Roberts 1994 on potential government benefits and collective-action limits.

**Data.** Five initial sectors: commercial banks, credit unions, life insurance, auto manufacturing, crypto (crypto analyzed separately). Unit: sector by year, with sector by chamber by year for RShare. Proposed period 2008-2025, annual; extension to 1998 possible. About 9 elections in sample, six of which flipped a chamber (House 2010, 2018, 2022; Senate 2014, 2020, 2024). Sources: OpenSecrets bulk data, DIME v4.0 (1979-2024), FEC bulk data, OpenSecrets lobbying, LDA.gov API (quarterly LD-2 since 2008), Iowa Electronic Markets, Cook Political Report ratings (1984-present), 538 generic-ballot averages (back to 1996), FRED. N is not stated; the text implies about 18 observations per sector, roughly 90 sector-years (computed here, not stated).

**Method.** Pooled panel with sector fixed effects, not per-sector regressions. (1) log contributions on prior-year growth, same-year growth, negative-growth dummy, Midterm, Pres, Tossup, trend. (2) RShare by chamber on growth, Shift, RMajNow, Midterm, Pres. (3) log lobbying on lagged log lobbying, growth, Neg, Unified, Midterm, Pres, sample from 2008. Growth is real, z-scored within sector. Robustness: year fixed effects; Shift x RLean; expected seat change. Inference: wild cluster bootstrap by sector; Driscoll-Kraay where year-level terms enter. Forecasts are used only after estimation, to predict 2026.

**Identification.** No causal claims (Section 7). Prior-year growth is the main test because same-year growth may be caused by the spending and early-year money precedes most growth (Section 4.1). Sector fixed effects absorb time-invariant differences. Year fixed effects give the cleanest growth test but absorb projected control. Projected control rests on about 9 elections; Shift x RLean adds sector-by-year variation. Lobbying uses current control (unified vs divided) because lobbyists work on the sitting Congress.

**Scope.** Federal campaign contributions and federal lobbying (LDA) only. Five sectors, possibly expanded to every BEA industry matchable to OpenSecrets codes (about 70 BEA industries). Era 2008-2025 proposed, with possible extension to 1998. Life insurance is mainly state-regulated (McCarran-Ferguson), so federal-only scope is an open question (Section 8D). Fairshake outside spending is not candidate contributions.

## Key findings

- No models estimated yet; the note reports design decisions only, with no empirical results. (0 models; Header, Stage line)
- Projected control is measured as probability of majority, not raw expected seat gains; seat change kept as robustness check. (; Section 4.3)
- Prior-year growth is the main test; same-year growth is secondary because it can be caused by the spending itself. (; Section 4.1, item 1)
- Cook Political Report final ratings (1984-2022) were correct about 94 percent of the time for Lean, 97 percent for Likely, about 100 percent for Solid. (94 / 97 / 100 percent; Section 6.3)
- Cox and Magar (1999) estimate House majority status worth about 36,000 dollars per member in corporate and trade PAC receipts around the 1994 switch; cited in the note, not verified. (36,000 dollars per member; Section 4.3)
- Only about 9 elections in the sample; six flipped one chamber. (9 elections; 6 flips; Section 7)
- Fairshake super PAC raised roughly 260 million dollars over 2023-24, mostly from a handful of firms and founders; outside spending, not contributions. (about 260 million dollars; Section 7)
- OpenSecrets API was discontinued April 15, 2025; bulk data remain free for non-commercial use. (April 15, 2025; Section 6.3)

## Limitations

- Few elections (about 9); projected-control estimates rest on few independent observations (Section 7)
- Few sectors (5 clusters) make inference fragile; wild cluster bootstrap proposed (Sections 5, 7)
- Reverse causality for same-year growth (Section 7)
- Growth measures not comparable across sectors (Section 7)
- OpenSecrets codes do not match NAICS; BEA combines banks with credit unions and life insurers with other carriers (Section 7)
- Regime changes: Citizens United and SpeechNow (2010), McCutcheon (2014), 2008 LDA reporting change, CUNA and NAFCU merger effective January 1, 2024 (Section 7)
- Crypto: short history and very concentrated donors (Section 7)
- Unobserved spending: 501(c) dark money and unregistered lobbying (Section 7)
- Correlational design; no causal claims (Section 7)
- Post-2007-08 drop in registered lobbyists, possibly shadow lobbying (Section 8C.8)
- Odd-year prediction markets often not open, so P is hard to measure in odd years (Section 8B.5)
- Open question whether 5 to 10 sectors allow reliable inference (Section 8E.13)

## Short quotes

- "Growth is correlated from year to year, so separate models would each pick up part of the other's effect." — Section 4.1, item 1
- "Access comes with majority status: committee chairs and control of the agenda." — Section 4.3
- "Actual election results would be hindsight, and they are partly caused by the money." — Section 4.3

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | method-source | Pooled sector-FE models: (1) log Contrib on G(t-1), G(t), Neg(t-1), Midterm, Pres, Tossup, Trend; (2) RShare by chamber on growth, Shift, RMajNow, Midterm, Pres; (3) log Lobby on lagged log Lobby, growth, Neg, Unified. Growth z-scored within sector; CPI-deflated dollars; wild cluster bootstrap by sector; proposed sample 2008-2025 with 5 initial sectors (expansion to about 70 BEA industries allowed). (Sections 4.1, 4.2, 4.3 and 5 (equations 1-3); Section 6.1) | sgs_registry.R builds C1-C3 and L1-L3 as sector-FE tests with wild cluster restricted bootstrap by sector (Webb six-point weights); sgs_estimate.R header confirms this inference. Growth z-scoring and log DVs not checked in code here (the registry default DV is levels; see next bearing). `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R:112-120; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:1-10` | Holds only as a statement of the memo design. The build departs from it (next bearing), so build results must not be read as memo tests. | strong | VERIFIED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | contradicts | Both growth timings belong in one regression (distributed lag) because separate models would each pick up part of the other effect. Totals get Tossup = 1 - abs(2P - 1), not a signed Republican-gains term. A Neg dummy, a Trend and lagged log lobbying are included. Lobbying uses Unified control. (Section 4.1 items 1, 7 and 8; Section 4.3; Section 5 equations 1 and 3) | Registry runs C1, C2 and C3 as separate single-growth models, not jointly. Contribution controls are pres_year, midterm_year, exp_dem_house, exp_dem_senate; the core has no Tossup, Neg or Trend term. Lobbying adds cur_dem_house and cur_dem_senate and has no lagged DV. Contribution DV defaults to real dollar levels (contributions_real_m), with logs only in robustness. Primary window 2020-2025 (R8). The g_pos/g_neg asymmetry split is an EXTENSION family from the 2026-09-26 rulings, not the core spec, so the rel statement that decline years use it as the core is softened. The expected-control terms are not P in levels: per year they come from an IEM annual mean, a binary forecaster final call (Democrats at or above 218 House or 51 Senate seats), or a market snapshot. The 2023 value is a final call flagged as measured Nov 2024, after the year. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R:30-34, 112-114, 313-316; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_config.R:38-41; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_run.R:29, 190; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_panel.R:638, 1380; C:/Users/pgoff/Mentat/sector_growth_study/results/report_analysis-R-20260926T221444Z.md (input flags)` | Applies to every CORE C and L result. Rulings R4-R8 and the 2026-09-26 rulings may supersede parts of the memo, but results from separate-timing, signed-control models should not be reported as tests of memo H1 or H3. | strong | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#SEATS-W weekly seat-gain test (2022 House)\|SEATS-W weekly seat-gain test (2022 House)]] | qualifies | Probability of majority control is the preferred projected-control measure because access comes with majority status (Cox and Magar 1999, about 36,000 USD per member, cited and not verified here). Expected seat change is kept only as a robustness check. Q4 asks how quickly PAC giving tracks changing expectations within a cycle; Q7 asks about an event study around expected flips. (Section 4.3; Section 8B questions 4 and 7) | SEATS-W is a weekly test in report 221444: 17 weeks, 2022 House only (2020 and 2024 not usable yet). Dependent variable is the Democratic share of gross positive receipts. Regressor is the prior-week change in 538 Lite expected Democratic seats, with a week-of-cycle trend. House level, pooled-dollar (the first row of the primary table): -3.36 pp per seat, block-bootstrap p 0.11 and 0.10, not significant. The -3.47 pp figure with p 0.0086 and 0.0174 (Holm 0.034 and 0.070) is the equal-sector variant. Senate level is null in both weightings (equal-sector +6.23, p 0.65; pooled-dollar +3.99, p 0.81). `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_seats.R:13-22, 270-285; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_config.R:195-206; C:/Users/pgoff/Mentat/sector_growth_study/results/report_analysis-R-20260926T221444Z.md (primary table, lines 338-352; House level blocks near lines 613-640 and 723-735)` | Exploratory, one-cycle reading only, 2022 House. The report itself calls it EFFECTIVELY A ONE-ELECTION TEST. Any headline must state the weighting; the pooled-dollar result is not significant. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-giving-forecast#Cycle-aware giving refit (phase and drift log models, 50-50 growth)\|Cycle-aware giving refit (phase and drift log models, 50-50 growth)]] | supports | Election-cycle controls use Pres and Midterm dummies with odd years as the baseline. Industry forecasts (IBISWorld, trade outlooks) are used only to predict 2026 spending after estimation and are never mixed into the estimated growth variable. (Section 4.2 (cycle controls); Section 4.1 item 6 (forecasts used only at prediction)) | The cycle model projects with M = B exp(phi dP + d_h) (1 + x/2), where dP = Pres(c) minus Pres(c-2) is a presidential-cycle difference, not a Midterm dummy with odd-year baseline. The IBISWorld real growth path, lagged one year, enters only through the projection term x. Forecast growth is kept out of estimation in this model. The sector study does put predicted_growth_pct on the right-hand side in C3 and L3 (sgs_registry.R:30), which the memo prohibits; that departure is outside this bearing. `C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py:1391-1397; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R:30` | Supports only the forecast product split between estimation and projection. Does not cover the sector study C3 and L3 tests, and the presidential-difference term is not the memo Midterm and Pres dummy set. | moderate | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: SEATS-W headline -3.47 pp (p 0.0086 and 0.0174) is the equal-sector variant. The pooled-dollar primary row is -3.36 pp with p 0.11 and 0.10 and is not significant. The rel does not say which weighting it reports.
> - Summary issue: SEATS-W regressor is the prior-week change in 538 Lite expected Democratic seats with a week-of-cycle trend, not the level of expected seats as the summary states.
> - Summary issue: The no-stable-shift figure (-0.04, p 0.76) is the SHIFT-Chamber-PV preview: 80 rows, 4 residual df, below the 6-year minimum. Core SHIFT-Chamber and SHIFT-House are NOT ESTIMABLE in report 221444 (5 of 6 years). The SATURATED at 2 years wording does not match that report.
> - Summary issue: Majority probability P is not what enters as exp_dem levels. The panel uses per-year IEM annual means, a binary forecaster final call (Democrats at or above 218 House or 51 Senate seats), or a market snapshot. The 2023 value is a final call measured Nov 2024, after the year, which departs from the memo measured-before-money rule (Section 4.3). The rel does not record this.
> - Summary issue: L3-LV year-FE coefficient -0.4765 (rel -0.477) is not significant (p 0.15, CI -1.02 to 0.73). The rel presents the sign flip without that qualifier.
> - Summary issue: The within-cycle signal claim is overstated: equal-sector House lag (-3.05, p 0.014) and 538 Classic equal-sector level (-3.59, p 0.036) are also negative. All rest on one cycle, as the report itself says.
> - Summary issue: Related link anon-2025-cross-industry-political is not same-data. The memo covers executive and CEO giving for the top ten firms by revenue in four industries and has no lobbying measure. Dropped from related.
> - Summary issue: Imai and Kim (2021) text has no discussion of lagged dependent variables with fixed effects; the rel attribution is unsupported and was removed.
> - Summary issue: The ORIGINAL_MODEL_CONTRACT note and its transcription claim were not located in the sector_growth_study folder. Unverified.
> - Summary issue: Confirmed by absence in code/R: no Tossup, Driscoll-Kraay or Shift x RLean term in the registry tests. Not checked: the paul_alignment claim that growth adds no adjusted R2.
> - Summary issue: Coverage: all 4 bearings and all 9 related links checked; 8 kept, 1 dropped. Text-only review; page images not available.

## Gaps and flags

- No distributed-lag core model: the build never enters G(t-1) and G(t) jointly in one regression, which the memo's main argument (Section 4.1 item 1) requires; C1 and C2 each risk absorbing the other's effect.
- No Tossup (stakes, H3) term exists anywhere in the R study code; contribution totals carry signed exp_dem controls, which the memo argues against for total-dollar outcomes.
- Lobbying models lack the lagged dependent variable the memo specifies (Kerr et al. persistence), and the memo's open question Q13 on Nickell bias with FE plus a lagged DV over about 18 years is unaddressed.
- Driscoll-Kraay errors for year-level terms are not implemented; the build uses two-way cluster SE as its sensitivity check instead (contract: neither may be silently replaced).
- The primary window of 2020-2025 gives about 3 elections against the memo's 9, so the projected-control leverage the memo already calls weak is thinner still.
- The Shift x RLean interaction, which the memo calls the stronger test because it survives year FE, is not among the registry tests located here.
- The exp_dem terms are per-year IEM annual means, a binary forecaster final call (Democrats at or above 218 House or 51 Senate seats) or a market snapshot, not majority probability P in levels; the 2023 value is a final call measured Nov 2024, after the year, contrary to the memo's measured-before-money rule (Section 4.3). The memo's Shift = P - RMajNow appears only as the derived exp_shift reparametrisation, not as the primary SHIFT focal term.
- C3/L3 regress spending on IBISWorld predicted growth in estimation, contrary to the memo's rule that forecasts never enter the growth variable; this is ruled (R6) but not labelled as a departure from the memo.
- Pooled coefficients across sectors with incomparable growth sources (crypto market cap vs bank assets) cross a known fault line; quote them with per-sector strata (the SLOPES family) or label them UNSTRATIFIED.
- Every figure from the build must name its window (2020-2025 primary, 2021-2025 preview, 2022-only SEATS-W) and its denominator (sector-years or unit-weeks); the memo's 2008-2025 framing does not describe the built results.
- Cox and Magar's 36,000 USD per member figure and Cook's 94/97/100 percent accuracy figures are cited and were not verified against sources on disk.
- LDA spending as a dependent variable is consistent with D-021, since it is not an edge or weight; the memo's issue-code alternative (Q8) would still be a measurement choice and must not become a member-level lobbying edge.
- No conflict with the rejected SNA proposals or the reciprocity rule; the memo does not touch networks.

No annotations; this is Paul own design memo. Evidence so far does not back his deep-pockets or access-seeking hypotheses: L3-LV +0.352 flips to -0.477 (not significant) under sector plus year FE, and the core annual SHIFT tests are not estimable, matching the memo own caution (no causal claims, few elections, 5 clusters). The memo is stale against the build: window moved to a 2020-2025 primary (R8), scope grew to nine sectors, and joint timings, Tossup, Neg, lagged lobbying and Driscoll-Kraay are not what the R registry runs.

## Related papers

- [[imai-2021-fixed-effects-regression]] — complements — Imai and Kim (2021) on two-way fixed effects for causal inference supports caution about the memo year FE plan. The text has no lagged-dependent-variable discussion, so the rel attribution about a lagged DV with few units is not carried over.
- [[kim-2020-strategic-sequential-links]] — complements — Kim, Stuckatz and Wolters (2020) find interest-group donations directly tied to subsequent lobbying, supporting separate modeling of giving and lobbying. The unit is interest group, not firm as the rel says.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Finds minimal overlap between organizations that lobby and organizations making campaign expenditures. Bears on memo questions 2 and 8.
- [[furnas-2019-partisan-ties-lobbying]] — complements — Partisan ties with the House majority party are valued by clients, and the paper tests firm value around the 2011 change in House control. Relevant to the access-with-majority logic in H2 and H4.
- [[huneeus-2018-effects-firms-lobbying]] — complements — Estimates firm-level returns to lobbying (abstract and counterfactuals). Relevant to the deep-pockets reading; the link to H1 is interpretive.
- [[bonica-2013-mapping-ideological-marketplace]] — complements — Introduces the common-space CFscores that DIME uses for the RShare outcome.
- [[bacik-2025-measuring-dynamical-evolution]] — complements — Reports lobbying network dynamics with periodicity synchronized with the presidential election cycle, supporting the memo Pres and Midterm controls in the lobbying model.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — Covers Fairshake and the concentrated crypto lobby, supporting the memo choice to analyze crypto separately.
- [[goff-nd-cross-industry-finance]] — complements — Paul earlier report on five financial sectors using FEC, FDIC, SEC and LDA data. Overlapping sectors and sources, but a snapshot report, not the sector-year panel.

### Linked from
_None._