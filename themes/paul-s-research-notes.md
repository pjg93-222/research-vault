---
type: theme
name: "Paul's Research Notes"
paper_count: 5
tags: [theme]
---

# Paul's Research Notes

**Scope.** This theme holds five works. All five are Paul's own documents, not published literature. Four are linked design and data notes from 24-25 September 2026 behind the Mentat sector growth study: [[notes/note-request-for-review-industry-economic-performance]], [[notes/note-review-industry-growth-and-political-spending]], [[notes/note-industry-growth-and-political-spending-research]] and [[notes/note-sector-growth-and-political-spending-source]]. The fifth is his earlier client report on financial-services political money, [[goff-nd-cross-industry-finance]]. The theme's `paul_notes` block is much wider: 42 extracted documents, mostly annotated bibliographies on SNA, path-finding, ML/stance, lobbying and RL influence-maximization. Most of it does not concern these five works. Only the parts that do (the cross-industry memo extraction, the research-plan extraction, and the IBISWorld/IEM source-check memos) are used here.

**The question running through the theme.** Does a sector's economic performance (past, current or forecast growth) predict how much it spends on federal politics, through PAC/organizational giving and through LDA lobbying? Does expected partisan control shape the amount and direction of that money? The report asks a practical cousin of the question: how should Credit Unions respond to being outspent by crypto and banks?

**State of the evidence.** None of the four design documents reports an estimate. Every result quoted below comes from the later R build and is carried in the works' own summaries as agent runs (UNVERIFIED under DE-062). So far the build shows no stable growth signal:
- **Fit:** growth adds no adjusted R2 (0.884717 vs 0.884690, LobbyView lobbying).
- **Predicted-growth lobbying slope (L3):** positive with sector FE, but it flips to -0.477 under sector plus year FE. This is a pooled, UNSTRATIFIED coefficient. The notes give its pre-flip size inconsistently: +0.352 in two notes, 0.242 M 2025 USD per pp in a third.
- **Partisan shift:** -0.04 (p 0.76, 2021-25).

The design review predicted this fragility in advance. The client report is purely descriptive (no model, no uncertainty). It is useful mainly as a contrast case: it shows what Mentat's cycle model and attribution splits do not yet capture.

## Strands

### Specifying the growth-to-spending model: brief, review, plan
These three documents form a design dialogue spread over two days.

**The Astra brief (24 Sep).** [[notes/note-request-for-review-industry-economic-performance]] sets out five hypotheses: deep pockets, distress, access-seeking, stakes, and lobbying tracking current power. It proposes three pooled sector-FE models:
- (1) log contributions on prior- and same-year growth together as a distributed lag, plus a negative-growth dummy, Midterm/Pres, Tossup and a trend;
- (2) Republican two-party share on Shift = P minus RMajNow;
- (3) log lobbying with a lagged dependent variable and a Unified-control term.

Growth is z-scored within sector. Inference uses a wild sector-cluster bootstrap with Driscoll-Kraay errors. The panel is 2008-2025 and five sectors.

**The review (same day).** [[notes/note-review-industry-growth-and-political-spending]] is the methodological centre of the theme. It argues that three of the brief's specifications do not test their hypotheses:
- A contraction dummy is not an asymmetric slope; use g+ and g- instead.
- Shift plus current control is just a reparameterization: f(P-M)+gM = fP+(g-f)M.
- A Unified term cannot separate expected control from current control.

It also argues that z-scores change scale, not meaning, so it uses raw percent real growth. It makes prior-year growth the main question, fit as log S on sector and year FE plus g(t-1), with PAC giving and lobbying modelled separately and never summed. Expectations become a separate, exploratory study, because more sectors add no more elections: about 9 elections and 6 chamber flips. Its data warnings:
- Do not fill missing control probabilities with realized outcomes.
- 538 models use fundraising, which creates a feedback loop, so prefer a polls-only sensitivity.
- LDA in-house totals already include retained-firm payments and association dues.

On inference, it rules out GMM with five to ten sectors and calls for the restricted wild-cluster bootstrap with leave-one-sector-out checks.

**The research plan (25 Sep).** [[notes/note-industry-growth-and-political-spending-research]] turns this into six core tests: C1-C3 for contributions and L1-L3 for lobbying, each using prior actual, same-year actual, or IBISWorld predicted growth. It keeps sector-FE OLS in millions of 2025 dollars, with Pres/Midterm and IEM expected-control covariates. It adds job-tier (Chief / Middle Management / Employee) and per-chamber Democratic-share extensions. Its lasting contribution is a set of discipline rules:
- report every null result;
- read results as associations with public forecasts, not causation;
- treat effect size and consistency ahead of p-values;
- independent-row OLS errors are insufficient with nine or fewer clusters;
- its own readiness check finds contribution years overlapping IEM forecasts only in 2021-2022.

**What the build took and left.** The built study adopted the review's raw percent growth, g_pos/g_neg, the restricted wild-cluster bootstrap with a few-cluster guard, LOSO/LOYO, and never-summed lobbying series. It kept the plan's dollar levels and national phase controls as primary, with logs and year FE only as robustness. It added Tossup and trend terms from the brief, and filled 2023/2025 expected control retrospectively by Paul's ruling, which is the step the review warned against. The L3 sign flip under year FE (UNSTRATIFIED, 5 sectors) bears out the review's central warning.

Works: [[notes/note-request-for-review-industry-economic-performance]], [[notes/note-review-industry-growth-and-political-spending]], [[notes/note-industry-growth-and-political-spending-research]]

### Data provenance and forecast-vintage discipline
[[notes/note-sector-growth-and-political-spending-source]] is the START HERE data inventory, and it marks itself as superseded. Its contents:
- IBISWorld real-revenue growth for four sectors (commercial banks, credit unions, life insurance, auto);
- a 72-row sector-year panel (4 sectors x 18 years, 2008-2025);
- actual chamber control verified against the House and Senate histories;
- IEM conditional expected control for 2008-2022 only;
- CPI multipliers to 2025 dollars.

The built `sgs_registry.R` carries its three growth variables and four controls almost exactly. Since then the study has moved to nine sectors and a 2020-2026 window, so the note is stale on scope but still binding on its warnings: keep blanks as missing, do not deflate IBISWorld twice, and treat expected-control means as concurrent, not causal.

The companion memos in `paul_notes` set the same provenance rules. Each sets one standard:
- **Forecast vintage:** a forecast counts only if published by 31 December of t-1, with numerator and denominator from the same vintage.
- **Combining reports:** add comparable revenue levels before computing growth; never average percentages.
- **Price basis:** evidence is ranked in tiers (direct PDF statement, internal identity, provider-methodology inference, unknown).
- **Proxies and relabeling:** never insert a proxy silently, never convert a qualitative statement into a rate, and never relabel a later outcome as a prediction.
- **IEM control data:** the derived control share excludes OTHER contracts, so it is a conditional market share, not an unconditional probability. 1,601 of 4,164 selected days have zero volume. A complete-case model with both chambers has only 15 distinct annual observations, because annual values repeat across industries and add no independence.

The collection is called a verified partial collection, not an exhaustive one. In the memos' own counts, 55 IBISWorld forecasts were collected, 53 pass strict checks, and 12 supplemental observations are kept apart. Crypto has a single Statista proxy (-3.78% for 2025). Life insurance 2008-2010 remains missing.

This strand is the theme's strongest methodological asset. It is also where its weakest identification point sits: national covariates repeated across sectors give only a handful of effective observations.

Works: [[notes/note-sector-growth-and-political-spending-source]], [[notes/note-industry-growth-and-political-spending-research]]

### Descriptive client reporting on financial-services money
[[goff-nd-cross-industry-finance]] is Paul's report to Trey Hawkins and Hannah Benito covering five financial-services industries. The year is not printed; 2026 is inferred. The extract covers only the Crypto and Community Banks sections. Its numbers:
- crypto giving rose from $1.76M (2021-22) to $145.5M (2023-24);
- the C-suite share rose from 69% to 98.8%;
- super PACs took 71.9% of non-direct giving, which is itself 70% of the total;
- it projects more than $175M for 2025-26 and attributes the rise to the CLARITY Act;
- crypto-backed candidates won 38 of 40 primaries.

It makes three recommendations to Credit Unions:
- raise giving 41% over the 2023-24 baseline, to about $4.3M (average contribution $82 to $115.62 a year);
- consolidate giving through trade groups and employee and middle-management donors;
- run campaigns that set Credit Unions apart from banks.

The method is wholly descriptive. Figures are extracted at scale; the report's own footnote says they come from the top 10 companies per industry and are not representative of whole industries. It reports no uncertainty, no counterfactual and no test. Several body claims have no matching source:
- the stated +8,162.6% does not match its own figures, which give about +8,167%;
- Fairshake's ranking as the second-largest super PAC;
- the 2026 LDA interaction counts;
- the 31% oppose / 27% support poll.

The ABA letter citation names a different bill from the one the body describes.

Its money-buys-attention reasoning is the claim that [[kalla-nd-campaign-contributions-facilitate]] actually tests, with an access outcome. Any reuse should keep access wording, not influence wording. A separate `paul_notes` flag applies to that same citation: in Paul's Trey-folder bibliography, the Kalla & Broockman citation carries annotation text describing a Furnas/LaPira staffer survey, so the citation and annotation may be mismatched. Paul should resolve that before relying on either.

Works: [[goff-nd-cross-industry-finance]]

## Agreements and debates

**Where the works agree**
- **Separate outcomes.** PAC/organizational giving and LDA lobbying are modelled separately and never summed. The brief, review, plan and source check all do this, and [[furnas-nd-conflating-lobbying-pacs]] (low organizational overlap) is consistent with it.
- **Prior-year growth is the main test.** Same-year growth can be caused by the spending itself ([[notes/note-request-for-review-industry-economic-performance]], [[notes/note-review-industry-growth-and-political-spending]]).
- **Few clusters.** With five to nine sectors, naive OLS errors are not credible. The brief uses a wild cluster bootstrap plus Driscoll-Kraay; the review a restricted WCB plus LOSO; the plan rules out independent-row errors at nine or fewer clusters.
- **Few elections.** About 9 elections and 6 flips limit any expected-control claim, and adding sectors or tiers does not fix it ([[notes/note-industry-growth-and-political-spending-research]], [[notes/note-review-industry-growth-and-political-spending]]).
- **Associations only.** Results are associations with public forecasts, not causal effects of firms' private expectations.

**Where they disagree**

| Issue | Brief | Review | Plan (and build) |
|---|---|---|---|
| Functional form | log outcomes, prior and same-year growth together as a distributed lag | log S with sector + year FE | dollar levels in millions of 2025 USD, each growth timing in its own model; zero or negative totals may not be dropped to make logs work; the build kept levels primary |
| Growth scale | z-scored within sector | raw percent real growth ("z-scores change scale, not meaning") | raw percent growth |
| Asymmetry | negative-growth dummy | g+ and g- slopes; a dummy cannot test asymmetry | build adopted g_pos/g_neg |
| Partisan terms | Shift = P - RMajNow; Unified term | Shift is a reparameterization of P plus M; Unified cannot separate expected from current control | Tossup was kept in the build |
| Dynamics | lagged-dependent lobbying model | dynamic FE only as a sensitivity, bias-corrected, no GMM with so few units | build has no lagged lobbying term |
| Year fixed effects | robustness check | primary design | robustness check; the L3 slope flips sign under year FE (pooled, UNSTRATIFIED, 5 sectors) |
| Missing expected control | — | never fill with realized results; prefer polls-only over 538 because of the fundraising feedback loop | 2023/2025 filled retrospectively by Paul's ruling; 538 Lite seats used |

**Report vs. cycle model.** [[goff-nd-cross-industry-finance]] projects a 2025-26 midterm-cycle rise in crypto money. Mentat's cycle model applies a symmetric ×1.8635 presidential factor on the midterm step, which would project a large drop for a crypto-like industry. The brief's Pres/Midterm controls (with [[bacik-2025-measuring-dynamical-evolution]] cited for cycle synchronization, a claim not re-read here) side with the cycle model. The report's projection is an unmodelled assertion. This is a live tension, not a resolved one.

**Internal numeric inconsistencies to reconcile**
- **L3 slope:** the pre-flip coefficient is given as +0.352 in two notes and as 0.242 M 2025 USD per pp (p 0.033) in a third. The Holm p-values are 0.0375 vs 0.1125.
- **Sample window:** it is given as 2020-2025, 2020-2026, 2008-2023 and 2021-25 in different places.

Any figure quoted from this strand must name which window and which sector set (denominator) it comes from.

## Methods

**Dominant method: pooled sector fixed-effects panel regression.** It runs on a short annual sector-year panel with national election-phase and expected-control covariates and cluster-robust or bootstrap inference ([[notes/note-request-for-review-industry-economic-performance]], [[notes/note-industry-growth-and-political-spending-research]], [[notes/note-sector-growth-and-political-spending-source]], [[notes/note-review-industry-growth-and-political-spending]]).

Its weaknesses, all named in the works themselves:
1. **Effective sample is tiny.** There are 5-9 sector clusters and about 9 elections. Expected-control covariates repeat across sectors, which leaves about 15 distinct annual complete-case observations for both chambers (IEM memo). Year FE then absorbs most of the identifying variation, which is why the L3 slope is not robust to it.
2. **Measures lack a common mechanism.** Asset growth, premiums, production and crypto valuations are not the same economic quantity, and within-sector z-scoring hides this.
3. **Covariate construction.** IEM shares are conditional (OTHER is excluded) and unweighted, with many zero-volume days. The 538 forecasts embed fundraising. Back-filled expected control mixes realized information into a forecast variable.
4. **Outcome construction.** LDA in-house totals already include retained-firm and dues payments. The expenses_total vs income_total accounting rule is still unresolved in the plan.
5. **Identification.** There is no design for causality. Public forecasts proxy expectations that are unobserved.
6. **Multiple testing and forking paths.** Six core tests plus extensions make the Holm adjustment and the plan's report-all-nulls rule load-bearing.

**Secondary method: descriptive extraction and shares** ([[goff-nd-cross-industry-finance]]). It is fast and readable. Its weaknesses: a top-10-company sample, no uncertainty, an arithmetic slip, unsourced claims, and causal language ("traction", attribution to the CLARITY Act) without a test.

**Strength.** The provenance discipline is unusually rigorous for this kind of study: vintage rules, price-basis evidence tiers, blanks kept as missing, hash-checked extraction CSVs. It is what makes the null results interpretable rather than an artifact of the data.

The theme's `paul_notes` also list a Causal & Methodological reading set that matches these weaknesses directly. It includes [[imai-2021-fixed-effects-regression]] on what two-way FE identifies, alongside Keele and Blackwell & Yamauchi (not filed as works here). That set is citation-only, with no annotations by Paul.

## How the works relate

- [[notes/note-review-industry-growth-and-political-spending]] — contradicts — [[notes/note-request-for-review-industry-economic-performance]]: The review is a critique of the Astra brief. It finds that the contraction dummy, Shift = P - M, Unified, z-scored growth and the lagged-DV choices do not test their hypotheses, and it replaces them with g+/g-, P plus current control, raw percent growth, and dynamic FE as a sensitivity only.
- [[notes/note-review-industry-growth-and-political-spending]] — builds-on — [[notes/note-request-for-review-industry-economic-performance]]: Same research question and the same sector/election data; the review keeps the brief's core growth-to-spending hypothesis and its separate giving and lobbying outcomes.
- [[notes/note-industry-growth-and-political-spending-research]] — builds-on — [[notes/note-review-industry-growth-and-political-spending]]: The plan, dated the day after the review, adopts its few-cluster inference caution and its separate outcomes, but keeps dollar levels and national phase controls rather than the review's log-plus-year-FE primary.
- [[notes/note-industry-growth-and-political-spending-research]] — contradicts — [[notes/note-request-for-review-industry-economic-performance]]: The plan fits each growth timing in its own model (C1-C3, L1-L3) in dollar levels, against the brief's single distributed lag in logs, and it drops the negative-growth dummy and lagged lobbying term.
- [[notes/note-sector-growth-and-political-spending-source]] — same-data — [[notes/note-industry-growth-and-political-spending-research]]: The source check is the data inventory for the plan's six core regressions: the same IBISWorld growth series, IEM expected control, chamber control and CPI multipliers. It refers to the plan as its linked plan.
- [[notes/note-sector-growth-and-political-spending-source]] — same-data — [[notes/note-request-for-review-industry-economic-performance]]: Both rest on the 2008-2025 sector-year panel with election dummies and expected chamber control; the source check's coverage limits (IEM through 2022 only) constrain the brief's Tossup and Shift terms.
- [[goff-nd-cross-industry-finance]] — precursor — [[notes/note-request-for-review-industry-economic-performance]]: The memo cites Paul's earlier report on the same financial sectors and sources (FEC, LDA). The report is a descriptive snapshot; the memo turns its sectors into a sector-year panel question.
- [[goff-nd-cross-industry-finance]] — same-data — [[notes/note-sector-growth-and-political-spending-source]]: Overlapping bank and credit-union sectors and LDA lobbying data. This is inferred: neither document cites the other and no data-level match was checked.
- [[goff-nd-cross-industry-finance]] — contradicts — [[notes/note-request-for-review-industry-economic-performance]]: The report projects a midterm-year rise in crypto money; the brief's Pres/Midterm phase structure, and the Mentat cycle model it supports, implies a midterm drop for presidential-heavy giving. The tension is unresolved.
- [[furnas-nd-conflating-lobbying-pacs]] — supports — [[notes/note-review-industry-growth-and-political-spending]]: Low overlap between organizations that lobby and those that make PAC expenditures is consistent with the review's never-summed, separate-outcome rule (topical; the review does not cite it).
- [[imai-2021-fixed-effects-regression]] — qualifies — [[notes/note-review-industry-growth-and-political-spending]]: Its cautions on what two-way fixed effects identify apply directly to the review's choice of sector plus year FE as the primary design.
- [[kalla-nd-campaign-contributions-facilitate]] — qualifies — [[goff-nd-cross-industry-finance]]: A field experiment on contributions and access is the nearest tested version of the report's untested money-buys-attention claim; the wording should be access, not influence. The citation/annotation mismatch in Paul's bibliography must be resolved first.
- [[kim-2018-lobbyview-firm-level]] — same-data — [[notes/note-sector-growth-and-political-spending-source]]: LobbyView firm-level LDA data underlies the LobbyView lobbying series on which the build's L3 result is reported. The source note itself does not name LobbyView for its bank/CU aggregates, so this is background only.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — [[goff-nd-cross-industry-finance]]: Covers Fairshake and the concentrated crypto lobby, against which the report's unsourced crypto claims could be checked. Not checked here.
- [[kim-2020-strategic-sequential-links]] — complements — [[notes/note-request-for-review-industry-economic-performance]]: Finds interest-group donations tied to subsequent lobbying, which supports modelling giving and lobbying separately while leaving open a lead-lag term that none of the five works estimates.

## What it means for the systems

All bearings below are carried from the works' own bearing tags. Results from the build are agent runs and stay **UNVERIFIED (DE-062)**. They are leads, not findings.

**Mentat sector growth study ([[systems/mentat-sector-growth-study]]; `growth-spending-regressions`, `seats-weekly-test`).** Four of the five works are its method source. The build follows [[notes/note-sector-growth-and-political-spending-source]] almost exactly. It diverges from [[notes/note-request-for-review-industry-economic-performance]] in functional form, Tossup vs signed exp_dem, and the missing negative-growth dummy, trend and lagged lobbying term. Its primary window (three elections) is far shorter than the brief's 2008-2025.

[[notes/note-review-industry-growth-and-political-spending]] is the best guide to reading the results. Under sector plus year FE the L3 slope is -0.477 (UNSTRATIFIED, 5 sectors; the window is reported inconsistently across notes). The partisan shift is null. The only within-cycle access signal, SEATS-W (-3.47 pp per expected D seat, House 2022), comes from a single election and relies on expected seats, the measure the brief demoted to a robustness check. Treat it as exploratory. The 2023/2025 back-fill of expected control is exactly the move the review warned against; flag it on any output that uses those years.

**Mentat giving forecast ([[systems/mentat-giving-forecast]]; `cycle-model`, `calculator-and-nowcast`).**
- **Growth input.** The live forecast still feeds IBISWorld lagged real growth into general giving at a 50/50 weight. The sector growth study has so far found no stable growth signal, so that input has no empirical support from Paul's own study. This qualifies the cycle model; it does not overturn it, given the null power.
- **Cycle factor.** The symmetric ×1.8635 presidential factor conflicts with [[goff-nd-cross-industry-finance]]'s projected midterm rise for crypto. A per-industry or asymmetric phase factor is a candidate extension, not a recommendation.

**Melange lobbying ([[systems/melange-lobbying]]; `lobbied-member-link`, `finance-client-classification`).** The report's per-member LDA interaction counts resemble the bill-roster join the lobbying lane rejected as circular. They also use LDA as a weight source, which **D-021 bars**: LDA is an identity source only. None of the report's per-member counts should enter Melange edges or weights.

**Mentat FEC attribution ([[systems/mentat-fec-attribution]]; `transfer-graph-and-direction`, `employer-match`).** The report's C-suite share (69% to 98.8%) and super-PAC share (71.9% of non-direct giving) point to splits Mentat does not currently make. The plan's Chief / Middle Management / Employee tiers are the same idea in study form. Both suggest an extension.

**Melange allocation ([[systems/melange-allocation]]; `break-even-and-roi`).** The report's "raise giving 41% to about $4.3M" target comes from no ROI model. It should not be read as a validated allocation target, and nothing in this theme says money buys influence (access wording at most).

**SNA guardrails ([[systems/melange-sna]]).** None of the five works argues for dropping regular cosponsorship edges or for per-edge volume normalization, and none touches graph ranks or reciprocity. There is nothing to flag on those rejected proposals.

## Gaps

1. **No estimates in the theme's own documents.** All four design notes predate the build. Results live only in single-path agent runs. A frozen, reproduced results note is missing: which window, which sectors, which L3 coefficient (0.242 vs +0.352).
2. **Expected-control coverage.** IEM runs only through 2022, as a conditional share excluding OTHER. There is no unconditional probability series, and the 2023-2025 values are back-filled or 538-based, which brings the fundraising feedback loop with it. A polls-only expected-control series, as the review recommended, has not been built.
3. **Outcome definition.** The lobbying accounting rule (expenses_total vs income_total) is unreconciled. The in-house double-count of retained-firm payments and dues is noted but not handled. Lobbying totals for life insurance, auto, Credit Arms, P&C and Telecom still need classification. Fintech has no comparable annual series.
4. **Sector coverage of the growth regressor.** Crypto has one Statista proxy and no IBISWorld series. Life insurance 2008-2010 forecasts are missing. Auto 2009-2010 lacks a common price base, and the 2023 life and auto price bases are unresolved.
5. **No identification strategy.** Every work frames results as associations. Nothing addresses reverse causality (spending to growth), common shocks, or firms' private expectations. The Causal & Methodological reading list (with [[imai-2021-fixed-effects-regression]]) is citation-only. So is Paul's "Look at Washington University at St. Louis and UCSD" pointer. Neither has been connected to this study.
6. **Untested sequencing.** No work estimates donation-to-lobbying lead-lag ([[kim-2020-strategic-sequential-links]]). The job-tier and per-chamber Democratic-share extensions remain unrun.
7. **The client report is incomplete as extracted.** Only the Crypto and Community Banks sections are present. Several claims lack sources, and there is an arithmetic slip and an ABA bill-name mismatch. The Kalla citation/annotation mismatch in Paul's bibliography is unresolved.
8. **Stale START HERE note.** [[notes/note-sector-growth-and-political-spending-source]] describes four sectors and 2008-2025; the study now uses nine sectors and a 2020-2026 window. An updated entry point would prevent agents from re-deriving the old design.
9. **No published literature is filed under this theme.** Every related outside work sits in other themes and was checked only by title. These include [[furnas-nd-conflating-lobbying-pacs]], [[huneeus-2018-effects-firms-lobbying]], [[bacik-2025-measuring-dynamical-evolution]] and [[chohan-2025-crypto-oligarchy-cryptocurrency]]. Their claims (cycle synchronization, low overlap, crypto concentration) still need reading against the text before they can back any statement here.

## All works

- [[goff-nd-cross-industry-finance]] (n.d.) — Cross Industry Finance and Lobbying Disclosure Report: Analysis and Recommendations
- [[notes/note-request-for-review-industry-economic-performance]] (2026) — Request for Review: Industry Economic Performance and Political Spending
- [[notes/note-industry-growth-and-political-spending-research]] (2026) — Industry Growth and Political Spending: Research Plan
- [[notes/note-review-industry-growth-and-political-spending]] (2026) — Review: industry growth and political spending
- [[notes/note-sector-growth-and-political-spending-source]] (2026) — Sector growth and political spending: source check (START HERE)
