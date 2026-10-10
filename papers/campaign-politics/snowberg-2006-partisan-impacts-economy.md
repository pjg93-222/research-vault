---
type: paper
title: "Partisan Impacts on the Economy: Evidence from Prediction Markets and Close Elections"
authors:
  - "Erik Snowberg"
  - "Justin Wolfers"
  - "Eric Zitzewitz"
year: 2006
venue: "IZA Discussion Paper No. 1996 (Institute for the Study of Labor, Bonn)"
doi: ""
category: "Campaign Politics"
systems:
  - mentat-sector-growth-study
components:
  - mentat-sector-growth-study.seats-weekly-test
  - mentat-sector-growth-study.seats-weekly-test
  - mentat-sector-growth-study.growth-spending-regressions
paul_annotated: false
paul_markup_count: 0
evidence_status: verified
source: "[[_sources/snowberg-2006-partisan-impacts-economy.pdf]]"
original_paths:
sha12:
  - 4e609691d10f
tags: [paper, campaign-politics, type/paper, theme/campaign-politics, evidence/verified]
---

# Partisan Impacts on the Economy: Evidence from Prediction Markets and Close Elections

**Erik Snowberg, Justin Wolfers, Eric Zitzewitz (2006).** *IZA Discussion Paper No. 1996 (Institute for the Study of Labor, Bonn).* Theme: [[themes/campaign-politics|Campaign Politics]].

> [!abstract] Summary
> Snowberg, Wolfers and Zitzewitz (IZA DP 1996, 2006; later QJE 2007) estimate how U.S. presidential outcomes move asset prices. Their key move is identification: on election nights 2004 and 2000, shocks to the market probability of a Bush win (a faulty afternoon exit poll that cut his odds from about 55 to 30 percent and then reversed; the Florida calls in 2000) are treated as exogenous, and asset changes are divided by probability changes (Wald ratios). In 2004 a Bush win raised S&P futures about 1.5-2 percent (0.016 at 10-minute differences, n=104), 10-year yields 11-12 basis points and December 2004 oil about 1.07 dollars. Over 1880-2004 a market-based partisan surprise (Republican indicator minus pre-election probability) implies 2-3 percent higher equities under a Republican president. The methodological lesson matters most for Paul: a daily campaign-period regression of prices on win probabilities gave equity coefficients about ten times the election-night estimate and flipped the oil sign, because the economy moves the odds (reverse causation) and common news moves both. OLS is only an upper bound if political and economic shocks correlate non-negatively. For Mentat this bears on the sector growth study: SEATS-W regresses weekly Democratic giving share on prior-week changes in 538 expected seats (2022 House, -3.47 pp per seat, one election, EXPLORATORY), and SHIFT uses expected-minus-current control. Lagging the forecast fixes ordering but not common shocks, so SEATS-W is a correlation, not a donor response. The paper also suggests a sharper design: event windows around discrete forecast jumps and a surprise-coded shock. It says nothing about networks, lobbying or ranks.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Do U.S. presidential election outcomes (a Republican versus Democratic winner, and Bush versus Kerry or Gore) causally move equity prices, bond yields, oil prices and the dollar?

**Theory:** Partisan business-cycle models (parties have different preferences over inflation, interest rates, output and capital and labor shares) versus median-voter policy convergence (Downs 1957). The economic voting channel (Fair 1978) implies reverse causation in campaign-period data, which the paper uses to explain why naive time-series estimates differ.

**Data:** 2004 and 2000 election-night 10- and 30-minute price changes in CME equity and currency futures, CBOT bond futures and NYMEX crude futures; Tradesports Bush contract (2004); Centrebet odds (2000). Daily closes June 2003 to October 2004 for the campaign analysis. Historical 1880-2004 event returns from Schwert (1990), CRSP and Kalinke (2004); pre-election probabilities from press reports and bookmakers. N: 104 ten-minute and 35 thirty-minute S&P observations (2004). Table 2 and historical N not stated; Figure 4 labels 34 elections.

**Method:** OLS of change in log financial variable on change in re-election probability, 10- and 30-minute first differences, inverse-period weights, White SEs (Scholes-Williams and 60-minute checks similar). Newey-West SEs for daily, 5-day and 20-day campaign regressions. Two-equation structural system with OLS bound. Wald ratio for yields using TIP and IEF funds. Historical event study of returns on a GOP indicator and partisan shock, plus an incumbent-party term.

**Identification:** Election-night shocks to the market probability of a Bush win are treated as exogenous: a leaked exit poll around 3pm that later reversed, and the 2000 Florida calls. Wald ratios divide the asset response by the probability response. For the campaign period, one further assumption on the correlation of political and economic shocks is needed; OLS is an upper bound if that correlation is non-negative. For 1880-2004 the assumption is that markets respond to election results rather than other news over a longer window.

**Scope:** U.S. presidential elections only. Intraday natural experiments cover 2000 and 2004; campaign-period daily data cover June 2003 to October 2004; historical event study covers 1880-2004 (Table 3 sample 1928-1996 and 1880-2004). Outcomes reflect trader expectations, not realized policy.

## Key findings

- 2004 election night (Table 1, p.7): Bush-versus-Kerry effect on log S&P 500 futures is 0.016 (SE 0.004, n=104) at 10-minute differences and 0.020 (SE 0.005, n=35) at 30-minute differences; text says equity prices 1.5 to 2 percent higher under Bush.
- Table 1, p.7: Dow 0.014 and Nasdaq 100 0.019 at 10 minutes (both significant at 1 percent); Nasdaq 0.024 at 30 minutes. Dollar 0.004 (not significant) at 10 minutes and 0.005 (5 percent) at 30 minutes.
- Table 1, p.7: December 2004 crude oil futures rise by 1.07 dollars (SE 0.35, 1 percent) at 10 minutes and 1.63 (SE 0.59) at 30 minutes; December 2006 contract is negative and insignificant.
- Table 1, p.7-8: 2-year note 0.095 (10 percent) and 10-year note 0.108 (5 percent) at 10 minutes; text says 10-year yields 11-12 basis points and 2-year yields 10 basis points higher under Bush.
- TIP and IEF Wald estimates, p.8-9: real yields about 16 basis points higher under Bush from the exit-poll experiment and 11 from the second; nominal about 16 and 15, so the authors conclude nominal moves mostly reflect expected real rates, consistent with expansionary fiscal policy.
- Exit-poll episode, p.5-6: Bush win probability fell from about 55 to 30 percent with stocks down about one percent, then rebounded to 95 percent as the exit poll was found faulty and stocks rebounded.
- Campaign-period daily closes (Table 2, p.10): S&P 0.087 (daily), 0.128 (5-day), 0.243 (20-day); text says the effect is roughly ten times the Table 1 estimate. Oil coefficients turn negative (-7.27 at 5-day, -12.57 at 20-day), read as reverse causation from oil to Bush odds.
- Reverse channel, p.11: a 10 percent S&P rise raises Bush re-election probability by 2.3 points (SE 0.8) using daily differences and 3.3 points (SE 1.3) using 5-day differences; authors call these small relative to economic voting literature.
- Bush versus Gore 2000 (p.13-14): with a Centrebet prior of about 60 percent for Bush, lower bounds are S&P +1.3 percent, Nasdaq 100 +3.7 percent and dollar +0.6 percent; the second Florida call gives 1.9, 3.0 and 0.7 percent.
- Century of elections (Table 3, p.17; Figure 4, p.16): the market-based partisan shock is significant at 1 percent, with coefficients of about 0.025 (SE about 0.008) in the extracted specifications; authors state a 2-3 percent equity rise with a Republican president. Incumbent-party term adds nothing in the joint specification (text: -0.0046, SE 0.0084).
- Bond yields (Figure 5, p.18): no reaction to elections from 1920 to 1976; from 1980 a Republican president raised the 10-year yield by about 13 basis points (p=0.15). Regression change in yields = 0.03 + 1.48 Post1980 + dProb x (0.05 + 12.79 Post1980), R-squared 0.23.
- Reagan 1980 (p.18): 10-year yield rose 15 basis points when markets priced Reagan at 80 percent. Congressional control tests across margins 0.5 to 0.72 found no significant effect of winning with control (fn 19, p.17).

## Limitations

- Only two close elections with overnight trading (2000 and 2004); the authors trade generality for precision (p.21).
- Estimates reflect trader expectations, not actual partisan policy differences; effects are specific to Bush versus Kerry or Gore, not parties (p.21).
- Cannot separate output-growth expectations from tax or capital-versus-labor policy channels; no firm welfare conclusions drawn (p.7, p.21).
- Assumes traders do not hedge election wealth effects (fn 5, p.4) and that no other news moved markets on election night (fn 7, p.5).
- Reverse causation within election night tested only by splitting the sample at 8pm and 10pm (fn 9, p.6); feedback from markets to votes is not fully excluded.
- Campaign-period economy effect is only bounded, not identified; it needs an assumed correlation of shocks (p.11-12, fn 14). Instrument search was not completed (fn 13, p.12).
- Bond evidence is thin: bond futures closed for three hours and TIP and IEF data are sparse (p.8-9); authors call these data sparser than they would like.
- The 2000 Iowa market tracked the popular vote, not the electoral college, so 2000 bounds rest on Centrebet election-morning odds (p.13).
- Historical event window is long and identification tenuous (p.14-15); 1964-72 probabilities are imputed from Gallup with sigma 4.9 (fn 16, p.15); partisan shock validity is questionable for 1916 and 2000 (fn 17); candidate quality varies across Republicans (p.17).
- Effect of congressional control cannot be tested directly because markets did not track it before 1994 (fn 19, p.17).
- Platform complexity prevents attributing effects to specific policies (p.21). Paper is a provisional IZA discussion paper (p.1).

## Short quotes

- "markets anticipated higher equity prices, interest rates and oil prices and a stronger dollar under a Bush presidency than under Kerry"
- "a simple time series regression of financial prices on re-election probabilities will confound the causal effects"
- "suggesting that electing a Republican President raises equity valuations by 2-3 percent"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#SEATS-W weekly seat-gain test (2022 House)\|SEATS-W weekly seat-gain test (2022 House)]] | qualifies | Table 2 daily-close campaign regressions give S&P coefficients 0.087 (daily), 0.128 (5-day) and 0.243 (20-day); oil -7.269 (5-day) and -12.570 (20-day). Text says the Table 2 equity effect is roughly a factor of ten larger than Table 1 (0.016 at 10-min differences, n=104). Authors attribute the gap to bias from reverse causation and common shocks. The OLS upper-bound statement (p.11, fn 14) concerns the reverse regression of the Bush probability on the S&P (2.3 and 3.3 pp per 10 percent rise), and holds if the shock correlation is non-negative, a sufficient condition. (Table 1 p.7; Table 2 p.10; text p.10-11 and fn 14 p.11) | SEATS-W regresses the week's Democratic share of giving on the prior week's change in 538 expected Democratic seats, pairing each week with the latest forecast available strictly before the week starts. 2022 House, 17 weeks, -3.47 pp per seat, EXPLORATORY. The code header presents it as a correlation and does not address common shocks. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_seats.R:15-21 (design and as-of pairing); sgs_seats.R:52-54 (SEATS-W gate); sgs_seats.R:66-75 (inference notes)` | The size of the bias does not transfer to PAC giving (lumpy, deadline-driven, different series), but the existence of a common-shock confound does: the as-of lag removes within-week ordering, not shared news moving forecasts and giving together. The negative slope cannot be read as donors moving toward or away from the expected winner. An upper-bound argument needs a stated correlation sign, which SEATS-W does not supply. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#SEATS-W weekly seat-gain test (2022 House)\|SEATS-W weekly seat-gain test (2022 House)]] | context | Identification uses two discrete election-night jumps in the Bush probability: the 2004 exit-poll leak (about 55 to 30 percent, then 95) and the 2000 Florida calls. Equity effects are 10- and 30-minute regressions (Table 1); a Wald estimator is stated explicitly only for yields (TIP and IEF, p.8-9). Footnote 9 splits the sample at 8pm and 10pm to test feedback. The paper does not recommend event-window designs; footnote 13 names instrumental variables as the ideal and leaves it open. (p.4-9; Figures 1-3; Table 1 p.7; fn 9 p.6; fn 13 p.12) | SEATS-W uses every pre-election week as an observation, with a residual block bootstrap conditional on the forecast path. No event-window design around large forecast moves exists in sgs_seats.R. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_seats.R:66-75 (bootstrap and inference notes); sgs_seats.R:93-94 (dated 2022 forecast histories in hand)` | An event-window design needs forecast jumps from news plausibly unrelated to donor behaviour, and transaction-dated giving fine enough around each jump. The 2022 538 run timestamps could support a search, which stays UNVERIFIED until a second path reproduces it (DE-062) and names its window and denominator. A Wald ratio needs a donor-side response series that the paper does not have. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | method-source | Partisan shock is the indicator of a Republican president minus the pre-election prediction-market probability of a Republican president. Table 3, 1880-2004: coefficient 0.0255 (SE 0.0082) without the incumbent term and 0.0248 (SE 0.0084) with it; incumbent-party term -0.0046 (SE 0.0084). Congressional control could not be tested because markets did not track it before 1994 (fn 19). (Historical evidence section p.15-17; Table 3 p.17; fn 19 p.17) | SHIFT in sgs_panel.R builds exp_shift as expected Democratic control minus current Democratic control, the Democratic-sign version. The brain study contract defines Shift as P minus RMajNow with P the Republican majority probability. Preview: chamber-stacked -0.04, p 0.76 (2021-25). `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_panel.R:230-235; brain topics/2026-09-feature-sector-growth-study.md (SHIFT preview lines near 268-270)` | Shift is an expected change in control before the election. The paper's shock is realized outcome minus prior expectation, so it transfers only to post-election giving. Independent shocks equal the number of elections, so any estimate is few-cluster and must say so. The sign convention must be stated as Democratic control, not Republican P. | moderate | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: paul_alignment says the sector growth plan cites the 2007 QJE version and lists it as a missing source. Unsupported: no Snowberg or Zitzewitz reference found in C:/Users/pgoff/Mentat/sector_growth_study or the brain topics folder. Strike or source it.
> - Summary issue: summary_md says OLS is only an upper bound if shocks correlate non-negatively. The paper states this for the reverse regression of the Bush probability on the S&P (p.11), and non-negative correlation is sufficient, not only-if.
> - Summary issue: summary_md says asset changes are divided by probability changes (Wald ratios) for identification. The paper uses a Wald estimator explicitly only for bond yields (p.8-9); equity effects are 10- and 30-minute regressions (Table 1).
> - Summary issue: summary_md's sharper design (event windows around forecast jumps, surprise-coded shock) is the analyst's inference. The paper does not recommend it and names instrumental variables as the ideal (fn 13).
> - Summary issue: summary_md and bearing 3 describe SHIFT as Shift = P minus RMajNow with P Republican. sgs_panel.R:230-235 uses Democratic expected minus current control; the sign convention must be stated.
> - Summary issue: Extraction garbles some Table 1 and Table 3 cells, so column alignment is uncertain; only cleanly aligned coefficients are verified here.
> - Summary issue: Version: the file is the IZA DP 1996 (March 2006) draft, not the QJE 2007 version. Cite numbers as the 2006 draft.
> - Summary issue: Scope: the Melange repo was not searched for the 2007 citation; the sector growth plan was searched in Mentat and the brain only.

## Gaps and flags

- SEATS-W states no identifying assumption about common shocks between forecasts and giving; the as-of lag handles ordering only. The -3.47 pp per seat slope (2022 House, 17 weeks, one election) is a correlation until an exogenous-jump design or a second cycle reproduces it.
- The sector growth study has no event-window or surprise-coded design; 538 run timestamps for 2022 would allow one.
- SHIFT and SEATS use expected control or seats but never the post-election surprise (realized minus expected), the quantity with the cleanest identification in this literature.
- The plan cites the QJE 2007 version; the file on disk is the IZA 2006 working paper, and numbers may differ between versions.
- Any reading of the SEATS-W negative slope as donors moving toward or away from the expected winner would conflict with this paper's evidence that campaign-period time-series slopes can be inflated about tenfold or sign-reversed; keep it EXPLORATORY and quote it with its window (2022 House, 17 weeks) and denominator.
- Version mismatch: the plan and missing_sources.md cite Snowberg, Wolfers and Zitzewitz 2007 QJE 122(2); this file is the 2006 IZA DP 1996 draft. Cite numbers as the 2006 draft.
- No conflict with the SNA, LDA (D-021), D-025 or reciprocity rules: the paper concerns asset prices, not networks, edges or lobbying weights.

Paul has no annotated entry and no markup for this work. His sector growth plan cites the 2007 published version as background on seat-gain and control effects (it was listed as a missing source). The evidence supports using forecast probabilities as the predictor, as SEATS and SHIFT already do, but cuts against a causal reading of the weekly slope: the paper's core point is that campaign-period regressions mix effect and feedback. The current build answers his seat-gain question descriptively, matching the EXPLORATORY label in sgs_seats.R; nothing in his notes is stale.

## Related papers

- [[furnas-2019-partisan-ties-lobbying]] — complements — Furnas et al. estimate revenue effects for party-tied lobbying firms after House-control flips, a control-shock design close to the SEATS and SHIFT question; the outcome is firm revenue, not donations.
- [[imai-2021-fixed-effects-regression]] — complements — Imai and Kim show that two-way fixed-effects estimates rely on linearity for a causal reading, which bounds what the SHIFT and SEATS-W panel regressions can claim.

### Linked from
_None._