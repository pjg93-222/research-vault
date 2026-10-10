---
type: paper
title: "Spillovers from Regulating Corporate Campaign Contributions"
authors: ["Adam Fremeth", "Brian Kelleher Richter", "Brandon Schaufele"]
year: 2018
venue: "MPRA Paper 87612, Munich Personal RePEc Archive (posted 02 Jul 2018; dated June 2018)"
doi: ""
category: "Lobbying"
systems: ["melange-allocation", "melange-influence-engine", "melange-lobbying", "mentat-fec-attribution"]
components: ["melange-allocation.joint-optimizer", "melange-influence-engine.flip-model", "mentat-fec-attribution.employer-match", "melange-lobbying.fec-lda-bridge", "melange-lobbying.lobbied-member-link"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/fremeth-2018-spillovers-regulating-corporate.pdf]]"
original_paths: []
sha12: ["0dc33fec3429"]
tags: [paper, lobbying, type/paper, theme/lobbying, evidence/verified]
---

# Spillovers from Regulating Corporate Campaign Contributions

**Adam Fremeth, Brian Kelleher Richter, Brandon Schaufele (2018).** *MPRA Paper 87612, Munich Personal RePEc Archive (posted 02 Jul 2018; dated June 2018).*  Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Fremeth, Richter and Schaufele (MPRA working paper, 2018) ask whether the federal per-candidate cap on corporate PAC gifts (5,000 dollars per election, 10,000 per cycle) pushes firms into other political channels. They use S&P 500 firms with PACs, 1992-2008 FEC data (6.8M firm-candidate-cycle rows) and LDA lobbying for 2000-2008 (about 2,800 firm-cycles). Within a firm-cycle, a candidate who receives the PAC maximum is 6.6 points more likely to also receive a gift from the firm's CEO (Table 2), about 110 dollars more on average. Firms whose PAC maxes out any candidate spend about 549,000 dollars more on lobbying per cycle, with firm and industry-by-cycle fixed effects. A one-sided regression discontinuity at the limit gives jumps of 1.6M to 3.0M dollars, but the authors call its frictions assumption strong. Placebo outcomes (R&D, advertising, dividends, acquisitions) show no jump. The authors say no exogenous variation in the limit exists, so the estimates are partial correlations. Some numbers in the text do not match the tables. For Paul's systems, the paper bears mainly on how money is bounded. The Melange optimizer treats the 10,000-dollar PAC limit as the ceiling on CU spend per member, and the flip model counts competitor money only as bank and finserv PAC dollars. The paper suggests firms at the cap shift resources into executive giving and lobbying, which neither ledger sees. Mentat already tiers Chief-level employee giving by company, so a PAC-at-limit by Chief-giving join is a cheap extension. The paper's firm-level link between giving and lobbying does not contradict the member-level FEC-LDA bridge null. That bridge asks a different question, and the paper itself notes that LDA filings name no candidates.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Do statutory limits on corporate PAC campaign contributions generate spillover increases in other corporate political activity, namely CEO contributions to the same candidates and firm lobbying expenditures, at the regulated per-candidate contribution threshold?

**Theory.** Revealed-preference firm model (Section 2, pp.5-7): firms give to candidates until marginal benefit equals the dollar; a binding per-candidate cap creates a positive shadow price, so firms substitute toward other channels. Lobbying is also read as a legislative subsidy (Hall and Deardorff 2006) and as a possible cost of access. The authors state the design is theory-agnostic and is not a test of theories of firm political behavior (p.4).

**Data.** S&P 500 firms present at any point 1991-2008; 950 firms in contribution data. Contributions: FEC filings via Center for Responsive Politics bulk data; CEO identity and firm characteristics from COMPUSTAT/ExecuComp; PAC-to-firm mapping from Myers (2005). Unit for PAC and CEO models: firm-candidate-cycle, 6,803,661 observations over nine federal cycles, 1992-2008. Lobbying: LDA disclosures for engagements over 10,000 dollars, 2000-2008, firm-cycle unit; 2,796 firm-cycle observations stated in Section 3 (p.8); regression samples 2,783 observations and 787 firms (Table 3, p.29), 1,908 observations and 579 lobbying firms (Table 3 columns 4-6), 4,821 observations and 943 firms (placebos, Table 4), 2,200 or 2,191 observations and 700 or 693 firms (RD, Table 5).

**Method.** Linear probability and OLS. CEO model (eq. 3, p.10): binary CEO-to-candidate contribution on PAC contributed and PAC at-limit indicators with firm-by-cycle and candidate-by-cycle fixed effects. Lobbying model (eq. 4, p.13): firm-cycle aggregate lobbying on PAC contributed and at-limit indicators with firm fixed effects and Fama-French 49 industry-by-cycle fixed effects. One-sided regression discontinuity (eq. 5, p.16): polynomial (linear or quadratic) in PAC dollars with a jump at the limit, with and without industry-cycle fixed effects. Difference-in-differences around BCRA (post-2003 cycle indicator, Table 6, p.18). Standard errors clustered by firm. Altonji, Elder and Taber (2005) selection-on-unobservables ratio reported for the main lobbying model (p.14).

**Identification.** CEO model: within firm-cycle, cross-candidate variation across the statutory per-candidate limit, assuming firm-by-candidate-by-cycle factors are orthogonal to CEO giving (p.4). Lobbying model: firm and industry-cycle fixed effects, plus assumed smoothness of lobbying absent regulation, which the authors test on advertising, R&D and dividends in the appendix (p.4-5, p.A-4 to A-7). RD: assumes optimization frictions so firms cannot perfectly control PAC dollars around the limit; authors call this strong (p.17). BCRA 2002 soft-money ban is the only exogenous policy change; it leaves PAC limits intact. The authors say there is no exogenous variation in FEC limits, so results are partial correlations (p.14).

**Scope.** US federal elections; large publicly traded S&P 500 firms with PACs; contributions 1992-2008; lobbying 2000-2008, so only a single pre-BCRA lobbying cycle. Excludes state politics, super PACs (post-window), independent expenditures and in-kind or philanthropic channels (pp.19-20).

## Key findings

- CEO supplementary giving (Table 2, p.28): at-limit PAC raises the probability a CEO gives to that candidate by 0.066 (SE 0.006). The text instead reports 7.7 percent (Section 4.1, p.11), a mismatch noted here.
- CEO dollars (Table 2, p.28): at-limit PAC associated with 109.92 extra dollars (SE 10.05). Text reports 123 dollars (p.11); Table 7 (p.A-4) gives 123.88 (SE 10.55). Figure 2 shows 59.91 for the 4,500-4,999 bin versus the at-limit value.
- Lobbying main estimate (Table 3 column 1, p.29; text p.14): firms whose PAC reaches the limit for any candidate spend 549,166 dollars more per cycle (SE 225,013; significant at 1 percent). Abstract gives a range of 549,000 to 1.6M dollars.
- Lobbying firms only (Table 3 column 6, p.29; text p.15): at-limit coefficient 662,590 dollars (SE 305,066; significant at 5 percent).
- Large-expenditure control (Table 3 columns 2-3, p.14-15): PAC-large-expenditure indicator is not significant; at-limit remains significant when both are included, about 250,000 dollars larger than the large-expenditure coefficient per text. Altonji-Elder-Taber ratio stated as 7.7 (p.14).
- Placebo outcomes (Table 4, p.30; text p.15): at-limit coefficients are R&D 63.07 (SE 68.56), advertising 4.71 (SE 34.74), dividends -23.02 (SE 66.93), acquisitions 5.32 (SE 103.31); none significant. Units not stated in the paper.
- One-sided RD on lobbying (Table 5, p.30; text pp.16-17): jump of 3,001,984 (linear, no industry-cycle FE), 2,427,612 (quadratic), 2,250,564 (linear, industry-cycle FE) and 1,630,744 (quadratic, industry-cycle FE), all significant at 1 percent. Figure 3 (p.26) plots the discontinuity.
- BCRA difference-in-differences on lobbying (Table 6, p.31): at-limit times post-BCRA coefficient 539,008.1 (SE 220,949.6), significant at 1 percent. Text reports a 145 percent increase (p.18), with a caveat about the single pre-BCRA lobbying cycle.
- BCRA on CEO giving (Table 6, p.31): at-limit times post-BCRA coefficient 16.54 (SE 11.77), not significant. Text reports a 16 percent increase (p.18), which does not match the dollar units of the table.
- Descriptives (Table 1, p.27; text p.8): average PAC contribution to all candidates 68.48 dollars (SD 553.79); 2,266.53 among recipients; 14.4 percent of recipients receive the maximum. CEO average 2.00 overall and 1,323.98 among recipients. Mean lobbying 1,101,868 dollars for all firms and 2,898,788 for lobbying firms.
- Authors' summary (Section 1, pp.3-5; Conclusion, p.19): statutory limits on corporate PACs are effectively non-binding, and spillovers into lobbying can exceed the contribution limit by more than 100 times.
- Smoothness and placebo figures (Appendix B, Figures 4-6, pp.A-5 to A-7): advertising, R&D and tax-expenditure outcomes show no comparable jump at the PAC limit, supporting the smoothness assumption.

## Limitations

- No exogenous variation in FEC limits; estimates are partial correlations (p.14).
- Lobbying is observed at firm-cycle level, so the at-limit indicator is coarser and identifying variation is reduced (Section 4.2, p.13).
- RD needs optimization frictions so firms cannot perfectly control PAC dollars; the authors call this assumption strong (p.17).
- CEO estimates are a lower bound because only CEOs are studied; other managers may also give (p.11).
- LDA filings do not name candidates, so firm-candidate lobbying cannot be linked; the mapping rests on an approximation with a stepwise function (A.3-A.4, pp.A-1 to A-3).
- Only one pre-BCRA lobbying cycle; the authors caution against over-interpreting the BCRA results (p.18).
- Theory-agnostic design; no welfare or optimal-regulation counterfactual (pp.6, 19).
- Excludes state politics, super PACs and independent expenditures outside the window (pp.19-20).
- Reverse causality and joint determination of lobbying and at-limit giving acknowledged; firm and industry-cycle fixed effects mitigate but do not remove it (pp.13-14).
- Internal inconsistencies between text and tables: CEO probability 7.7 percent versus 0.066 in Table 2; CEO dollars 123 versus 109.92; BCRA CEO 16 percent versus a dollar coefficient; Table 3 column labels in the prose do not match the printed layout.

## Short quotes

- "constraining specific corporate political activities often yields unintended effects (Fremeth, Richter and Schaufele 2018, abstract, p.1)"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-allocation#Joint CU budget optimizer\|Joint CU budget optimizer]] | qualifies | Per-candidate PAC limits bind for 14.4 percent of candidates receiving any PAC money (text Section 3, p.8; not in Table 1). Firm-cycles at the limit show +549,166 dollars of lobbying per cycle (Table 3 col 1, SE 225,013; 2000-2008, 787 firms, 2,783 firm-cycles, pooled across the 2002 BCRA break). Firms at the limit show CEO giving 0.066 more likely (Table 2, 1992-2008, 950 firms). Authors say PAC limits are effectively non-binding and all estimates are partial correlations. (Section 3 p.8; Table 2 p.28; Table 3 p.29; Section 1 pp.3-5; Conclusion p.19) | The optimizer caps each member at PMAX = 10,000 dollars above existing CU money, read as one CU PAC per-candidate per-cycle limit (Paul 2026-09-30 ruling, quoted in the code comment). Nothing outside the PAC channel is modeled. `fec/engine/optimize.py:23-27 (PMAX and pot comment); fec/engine/optimize.py:71 (marg cap check)` | Cap mechanics transfer if the 10,000 per-cycle limit is the same. The paper does not say the limit is unindexed; that is an assumption not stated in the paper. Spillover magnitudes come from large S&P 500 for-profit firms, not CU PACs, and are partial correlations. Supports a caveat that PMAX bounds PAC dollars only; does not justify reweighting. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | qualifies | Firm-cycles whose PAC reaches the limit are associated with 549,000 to 1.6M dollars more lobbying per cycle (abstract; Table 3 col 1; Table 5) and with about 110 dollars more CEO giving per candidate at the limit (Table 2). CEO giving is candidate money outside the PAC channel. Authors describe these as partial correlations. (Abstract p.1; Table 2 p.28; Table 3 p.29; Table 5 p.30) | The b leg of pflip counts competitor money as bank plus finserv PAC dollars per member (ind0), loaded from data/engine/congestion.json, with median imputation for unobserved members. Executive giving and competitor lobbying are not in ind0. `fec/engine/flip_model.py:32 (congestion.json load); fec/engine/flip_model.py:39 (ind0); fec/engine/flip_model.py:92-97 (pflip)` | Hypothesis only: if competitor bank and finserv firms hit the cap as S&P 500 PAC firms do, ind0 undercounts competitor pressure on the members they max out. The paper does not test this. Does not license LDA dollars as a per-member weight (D-021). | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-fec-attribution#Employer-string roster matching and overrides\|Employer-string roster matching and overrides]] | suggests-extension | Within firm-cycle and candidate-cycle fixed effects, a candidate receiving the PAC maximum is 0.066 more likely (SE 0.006) to receive a CEO gift and receives about 110 dollars more (SE 10.05) (Table 2; 1992-2008; 950 firms; 6,803,661 observations). The text reports 7.7 percent and 123 dollars, which do not match Table 2. Table 7 gives 123.88. (Section 4.1 pp.10-12; Table 2 p.28; Table 7 p.A-4) | Mentat attributes individual Schedule A rows to roster companies by employer string and splits company totals into Chief, Middle Management and Employee tiers, with known executives forced to Chief. `C:/Users/pgoff/Mentat/mentat/R/report.R:341-346 (tier loop); C:/Users/pgoff/Mentat/mentat/R/report.R:2528-2544 (spending_title_role, known executive to Chief)` | A per-candidate join of company PAC-at-limit status with Chief-tier gifts would test the CEO-substitution pattern in the industries Paul covers. Depends on executive employer-match recall and the PAC-to-candidate recipient table. Any result stays UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED |
| [[systems/melange-lobbying#FEC to LDA bridge test\|FEC to LDA bridge test]] | qualifies | Firm-cycles at the PAC limit associate with +549,166 dollars of LDA lobbying (firm FE and Fama-French 49 industry-by-cycle FE; 2000-2008; 787 firms; 2,783 firm-cycles). The at-limit by post-BCRA interaction is +539,008 (Table 6; 787 firms). LDA filings do not name candidates, so firm-candidate lobbying cannot be linked. (Section 3 p.8; Section 4.2 pp.13-15; Table 3 p.29; Table 6 p.31; Appendix A pp.A-1 to A-3) | Bridge round 3 tested member-level FEC exposure against LDA attention (2023-2026; credit_union, fintech, crypto). Raw rho +0.1525, four-control partial -0.0002, tier UNCORROBORATED, per the results block of the prereg. FEC framed as a target-side money prior only. `fec/lobbying/fec_lda_bridge3_look.py:57 (CONTROL_NAMES); fec/lobbying/fec_lda_bridge3_look.py:1490-1496 (_tier); results in C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec_lda_bridge3_prereg.md lines 285-290` | The unit is the firm, not the member, so the paper neither revives nor contradicts the member-level null. A transferable design would test an organization's giving at the cap against that organization's own LDA spend, with time fixed effects; that design is the analyst's suggestion, not the paper's. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Lobbying member-year network and circular lobbied-member link\|Lobbying member-year network and circular lobbied-member link]] | supports | LDA disclosures do not identify the candidates or members lobbied. The authors relate only firm-level lobbying totals to PAC behaviour and approximate the mapping with a stepwise function. (Section 3 p.8; Section 4.2 p.13; Appendix A.3-A.4 pp.A-1 to A-3) | The lobby interactions ledger links an organization to a member when it lobbied a bill that member sponsored or cosponsored (docstring lines 4-6). The paper does not say whether P(joined given lobbied) is 1 by construction; the REFUTED-as-influence verdict comes from the analyst and the brain, not this code or the paper. `fec/lobbying/lobby_interactions.py:4-6` | Confirms only that LDA rows do not name members, a measurement fact. The paper offers no contact measure to reuse. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The 549,000 lobbying figure and the 1.6M to 3.0M RD range are pooled across the 2002 BCRA break (2000-2008; one pre-BCRA lobbying cycle). Under the stratification rule they need an UNSTRATIFIED label or stratified figures; the summary states them without one.
> - Summary issue: The summary says the CEO effect is about 110 dollars more on average. Table 2 is the dollar effect for a candidate at the PAC maximum, conditional on firm-cycle and candidate-cycle fixed effects, not an average across candidates.
> - Summary issue: Summary figures lack windows and denominators: the 6.6-point CEO effect (1992-2008, 950 firms), the 549,000 lobbying effect (2000-2008, 787 firms, 2,783 firm-cycles), and the 14.4 percent at-limit share (text p.8, of recipients). Only the flags carry them.
> - Summary issue: The summary and bearing 1 say limits are non-binding on total corporate political activity. The paper says corporate PAC limits are effectively non-binding and that spillovers can be economically meaningful.
> - Summary issue: Bearing text uses causal verbs (move, spend more) for estimates the authors call partial correlations. The summary's flags say partial correlation; the bearings should match.
> - Summary issue: The summary's claim that the limit is unindexed is not in the paper and is unverified.
> - Summary issue: The paul_alignment claim that Paul's component notes expect donations to lead lobbying cannot be checked: the item has no paul_entries and no annotations (n_annots 0). Treat as unverified.
> - Summary issue: The related-link rationales were checked only for existence and title (all nine slugs are real items); their content claims were not re-read. Table 3 columns 4-5 at-limit values do not print in the extraction; only columns 1, 2, 3 and 6 are verified.

## Gaps and flags

- Neither the Melange allocator nor the flip model represents non-PAC channels (executive giving, lobbying) that firms use once PAC dollars hit the per-candidate cap. The PMAX ceiling and the ind0 competitor-money term are PAC-only.
- Mentat tiers Chief-level giving by company but does not join it to company-PAC-at-limit status per candidate, so the substitution pattern this paper documents is unmeasured for Paul's industries.
- No firm-level version of the FEC-LDA bridge (an organization's giving against the same organization's LDA spend) has been specified. All bridge rounds were member-level.
- The flip model treats competitor PAC money at or near the cap the same as smaller gifts. No at-limit indicator exists anywhere in the engine.

- The authors state the estimates are partial correlations (no exogenous variation in the limit). Do not quote the 549,000-dollar or 1.6M-3.0M RD figures as causal effects.
- The paper's text and tables disagree: CEO probability is 7.7 percent in the text vs 0.066 in Table 2, CEO dollars 123 vs 109.92, and the BCRA CEO effect is reported as 16 percent against a dollar coefficient. Cite the table values.
- Estimates pool S&P 500 industries (with industry-by-cycle FE) and span the 2002 BCRA break. The BCRA split appears only in a DiD with one pre-BCRA lobbying cycle, so treat the pre/post pooling as weakly stratified.
- D-021 guard: the paper uses LDA spend as a firm-level outcome, not a member edge. It must not be used to justify LDA-weighted member edges or per-member lobbying weights.
- For any reuse, state the windows and denominators: contributions 1992-2008 (950 firms), lobbying 2000-2008 (787 firms, 2,783 firm-cycles), S&P 500 firms with PACs only.

Paul has no annotated-bibliography entry or markup for this work. His component notes expect donations to lead lobbying. This paper shows same-cycle firm-level co-movement between maxed-out PAC giving and lobbying among S&P 500 firms, not a lead. That partly supports a money-lobbying link at the organization level and leaves the system's member-level bridge null (partial -0.0002, 2023-2026) standing. None of Paul's notes becomes stale. The paper adds a channel caveat to the 2026-09-30 PMAX ruling.

## Related papers

- [[kim-2020-strategic-sequential-links]] — complements — Real item (title: Strategic and Sequential Links between Campaign Donations and Lobbying). Only existence and title checked; the content claim was not re-read.
- [[kim-2023-systemic-sequential-links]] — complements — Real item (title: Systemic and Sequential Links between Campaign Donations and Lobbying). Only existence and title checked.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Real item (title: Conflating Lobbying and PACs: The Surprisingly Low Overlap in Organizational Lobbying). Title consistent with the claim; body not re-read.
- [[bickmore-2025-corporate-lobbying-political]] — same-data — Real item (title: Corporate lobbying, political action committees, and market concentration). Same-data claim not verified beyond title.
- [[kerr-2014-dynamics-firm-lobbying]] — same-data — Real item (title: The Dynamics of Firm Lobbying). Persistence motivation not re-read.
- [[defigueiredo-2013-advancing-empirical-research]] — cites — Real item (title: Advancing the Empirical Research on Lobbying). Citation claim not re-read.
- [[kalla-nd-campaign-contributions-facilitate]] — cites — Real item (title: Campaign Contributions Facilitate Access to Congressional Officials: A Randomized Field Experiment). Citation claim not re-read.
- [[ansolabehere-2002-little-money-politics]] — complements — Real item (title: Why Is There So Little Money in U.S. Politics?). Complement claim not re-read.
- [[goff-nd-cross-industry-finance]] — complements — Real item (title: Cross Industry Finance and Lobbying Disclosure Report). Complement claim not re-read.

### Linked from
_None._