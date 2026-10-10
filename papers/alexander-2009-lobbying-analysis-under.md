---
type: paper
title: "Measuring Rates of Return for Lobbying Expenditures"
authors:
  - "Raquel Alexander"
  - "Susan Scholz"
  - "Stephen Mazza"
year: "2009"
venue: "SSRN working paper (abstract 1375082). Authors at the University of Kansas. No journal version is named in the text."
doi: "SSRN 1375082"
category: "Lobbying"
systems:
  - "melange-allocation"
  - "melange-influence-engine"
  - "melange-lobbying"
  - "mentat-sector-growth-study"
components:
  - "melange-allocation.break-even-and-roi"
  - "melange-influence-engine.flip-model"
  - "mentat-sector-growth-study.growth-spending-regressions"
  - "melange-lobbying.contested-bills"
paul_annotated: true
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/alexander-2009-lobbying-analysis-under.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\ssrn-1375082.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\ssrn-1375082.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\ssrn-1375082.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\ssrn-1375082.pdf"
  - "C:\\Users\\pgoff\\Downloads\\ssrn-1375082.pdf"
sha12:
  - "2380ccbb8665"
tags: [paper, lobbying]
---

# Measuring Rates of Return for Lobbying Expenditures

**Raquel Alexander, Susan Scholz, Stephen Mazza (Not printed in the text. Likely 2009: the acknowledgments cite a 2009 conference and the references run to Feb 2009. SSRN abstract 1375082.).** *SSRN working paper (abstract 1375082). Authors at the University of Kansas. No journal version is named in the text..* SSRN 1375082 Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Alexander, Scholz and Mazza study the one-time dividend repatriation deduction in the 2004 American Jobs Creation Act (s.965). They treat lobbying as an investment and divide estimated tax savings (amount repatriated x 35% x 85%) by each firm's LDA-reported lobbying spend. A "repat!" keyword search of 10-Ks for 2004-2006 flagged 1,350 candidate firms, of which 476 reported repatriating, with dollar amounts for 450 (total $298bn). The introduction's figures differ from the body (496 firms vs 476; $62.5bn savings vs $62.05bn in Table 6). 93 of those firms lobbied. The headline ROI is about 220:1 ($62bn of savings on $282.7M of lobbying). Coalition members return 257:1 and solo lobbyists 154:1. Outliers drive the result: the per-company mean is 869:1 and the median 172:1. The lobbying denominator is an upper bound, and the design is purely cross-sectional, with no causal identification. Logit and OLS models (N=434 for the amount) show that size, profitability and operating cash flow predict who repatriated, and that size, cash/assets and operating cash flow predict how much (the pre-tax income indicator is not significant for amount, p=.330). The abstract and conclusion say cash holdings do not matter, but Table 5 contradicts this (cash/assets 3.70, p<.001).
> 
> For Paul's systems this is a firm-side, policy-rent return on lobbying expenditure. It tells us nothing about how legislators respond, and it does not cover PAC contributions. Melange's "ROI" and break-even figures measure expected member flips per PAC dollar, so the 220:1 figure does not transfer to them. The paper does sharpen two points. First, the paper reports five firms that lobbied against the provision (four repatriated anyway, fn 99) and never studies contested bills, so any design implication for the contested-bills registry is an inference. Second, its upper-bound denominator is the broadest coded lobbying total (fn 97), and Mentat keeps in-house and outside-firm LDA dollars separate, a concern that is the analyst's and not the paper's.

## Paul's annotation

From [[annotations/Trey - Articles - Report - Annotated Bibliography|Trey - Articles - Report - Annotated Bibliography]]:

> Alexander, Raquel Meyer, et al. “Measuring rates of return for lobbying expenditures: An empirical case study of tax breaks for multinational corporations.” *SSRN Electronic Journal*, 2009, [https://doi.org/10.2139/ssrn.1375082](https://doi.org/10.2139/ssrn.1375082).  
>
> “*We find firms lobbying for this provision have a return in excess of $220 for every $1 spent on lobbying, or* ***22,000%****. Repatriating firms are more profitable overall, but surprisingly, profitability is not a predictor of repatriation amount. Rather, industry and firm size are most predictive of repatriation.*” While this study is an outlier in terms of the size of its findings, it is a dramatic illustration of the ROI that political lobbying brings to companies that successfully pass legislation beneficial to their industry. Additionally, it addresses the question of scale and company size as factors in achieving such success, highlighting a significant positive correlation between the two variables.

## Study

**Question:** Do firms that lobbied for the AJCA s.965 one-time dividend repatriation deduction earn a measurable return on lobbying, and which firm characteristics predict whether and how much a firm repatriated?

**Theory:** Lobbying is treated as an investment with a rate of return (estimated tax savings divided by lobbying expense). Background comes from the deferral and lock-out literature on US taxation of foreign earnings (Sec. II.A). The authors argue that transparent, dollar-estimable tax benefits should yield low lobbying returns if firms were pricing lobbying efficiently, and that their data suggest the opposite (Sec. IV, pp. 31-32). No formal model is presented.

**Data:** Unit: firm. Period: 10-K fiscal years from Oct 2004 through Dec 2006 (filings to Mar 2007). Repatriation identification: Lexis-Nexis 10-K search for "repat!" within 50 words of AJCA, Homeland Investment, or HIA; 1,350 candidate firms. Group 1 (repatriated): 476 firms; amounts available for 450 (443 exact, 7 estimated from tax benefits), totaling $298.36bn. Group 2 (declined): 647. Group 3 (considered, then silent): 227. Compustat FY2004 covers 466 repatriators and 774 non-repatriators (p.19). Lobbying data: hand-collected from Senate LDA semi-annual filings (sopr.senate.gov), FEC, CapitalEye.org, and annual reports. Lobbying-firm sample N = 93 (39 coalition, 54 non-coalition). IRS SOI benchmark: 843 of about 9,700 eligible corporations (p.15).

**Method:** (1) Descriptive comparison of repatriating vs non-repatriating firms: means and medians with significance tests (Table 1). (2) Industry tabulation by one-digit and two-digit SIC (Table 2). (3) Logistic regression of a repatriation indicator on ln(total assets), cash/assets, liabilities/assets, pre-tax income indicator, operating cash flow/sales, and industry dummies (Table 3). (4) OLS of ln(amount repatriated) on the same regressors, N = 434, adjusted R2 = .48 (Table 5, fn 91). (5) Return on lobbying = estimated tax savings / estimated lobbying expense, with savings = amount repatriated x 35% x 85% (Table 6, Sec. III.B.2). Also a subsample of 48 firms spending over $1M.

**Identification:** No causal identification design. The authors exploit the one-year, elective, audited-disclosure tax holiday so that the benefit is observed directly. Comparisons are cross-sectional: no instrument, no difference-in-differences, no pre-period. Lobbying spend is set to an upper bound to avoid overstating ROI (p.28, fn 97). The text does not show the regression with lobbying as a regressor that the introduction describes (p.3).

**Scope:** US publicly traded firms that filed 10-Ks and disclosed AJCA s.965 repatriations. Institution: the one-time dividends-received deduction, its IRS guidance (Notices 2005-10, 2005-38, 2005-64), and congressional lobbying over it. Era: 2004-2006. Population is limited to firms with material or disclosed repatriation and foreign operations.

## Key findings

- Estimated return on lobbying for the AJCA repatriation provision is about $220 of tax savings per $1 spent (22,000%). Coalition firms return 257; non-coalition lobbyists 154; per-company average 869; the >$1M subsample 243; per-company median 172.2. Numbers: Overall ROI 220; savings $62.05bn (46,157.5 + 15,897.0) on $282.7M lobbying; coalition ROI 257; non-coalition 154; mean per-company 869; >$1M subsample 243. (Sec. III.B.2, Table 6 (pp.29-30); abstract (p.1))
- The 93 lobbying firms repatriated about 70% of total dollars. Coalition members (39) repatriated 52% of total dollars, and non-coalition lobbyists (54) 18%. Non-lobbying firms (357, 80% of repatriators) account for 30%. Numbers: Lobbyists $209.1bn of $298.36bn; coalition $155.7bn; non-coalition $53.4bn; non-lobbying $89.3bn; average lobbyist spend $3.0M, range $10,000 to $24.7M (GE). (Table 6 (p.29); Sec. III.B.2 (pp.28-29))
- Repatriating firms are much larger, more profitable, and hold less cash than non-repatriating firms. Medians differ significantly on assets, cash ratio, leverage, pre-tax margin, and operating cash flow. Numbers: Mean assets $24,833M vs $6,703M; median $2,511M vs $570M; mean cash/assets 15% vs 20%; percent profitable 91% vs 75%; mean pre-tax income/revenue 12% vs -32%. (Table 1 (p.20); Sec. III.A.1 (pp.19-21))
- Logit: repatriation decision is predicted by size, profitability indicator, and operating cash flow. Cash and leverage are not significant. Industry dummies are not significant. Numbers: ln assets .415 (p<.001); pre-tax income indicator .729 (p=.001); OCF/sales 1.918 (p<.001); cash/assets .225 (p=.640); liabilities/assets -.409 (p=.269); logit N not reported. (Table 3 (p.23); text (p.23))
- OLS on amount repatriated (N=434, adj R2 .48): size, cash ratio, and cash flow are significant, but pre-tax profitability is not. Numbers: ln total assets .915 (p<.001); cash/assets 3.702 (p<.001); OCF/sales 1.679 (p=.023); liabilities/assets 1.186 (p=.085); pre-tax income indicator -.314 (p=.330); SIC 3800 indicator 2.196 (p=.085). (Table 5 (p.26); text (p.26))
- Repatriation dollars are concentrated in pharmaceutical and manufacturing firms. SIC 2830 (drugs) accounts for $96bn from 26 firms, 63% of SIC 2000 totals. Average repatriation per firm is $663M, about 8% of assets. Numbers: Total $298,360M across 450 firms; SIC 2000 more than half of dollars; SIC 2830 $96bn; SIC 3600 $22.5bn from 44 firms. (Table 4 (pp.24-25); text (p.25))
- Disclosing firms are 56% of the IRS's 843 repatriating corporations but over 95% of dollars. Numbers: 476 firms vs 843; $298bn vs $312bn qualifying. (Sec. III.A (p.19))
- The authors find no evidence that the repatriation produced job growth. ASA and BLS data show no 2005 employment spike. The Sinai 660,000-job projection cited by supporters is treated as unsupported. Numbers: Sinai projection $265-406bn repatriation and 660,000 jobs; ASA buybacks S&P 500 $169bn (2004) to $334bn (2005). (Sec. II.B (p.9) and Sec. II.D (pp.17-18))
- Internal inconsistency on cash. The abstract and conclusion say cash on hand is not associated with the repatriation amount. Table 5 shows cash/assets significant at p<.001 for amount. The abstract also says industry and size are 'most predictive', but no industry dummy is significant in Table 3 and only SIC 3800 is close in Table 5 (p=.085). Numbers: cash/assets 3.702 p<.001 (Table 5) vs claim of no association (p.1, p.31); SIC 3800 p=.085. (Abstract (p.1); Sec. IV (p.31); Table 5 (p.26); Table 3 text (p.23))
- Policy conclusion: tax benefits went to older, larger, declining firms. The authors argue another repatriation holiday is highly questionable and that lobbying returns of this size question firms' acceptable-return logic. Numbers: n/a (qualitative). (Sec. IV (pp.31-32))

## Limitations

- Lobbying expense is an upper bound. Most firms do not report AJCA-specific spend, so the authors use the broadest coded lobbying total (fn 97, p.28). ROI is therefore conservative but not an exact measure of AJCA spend.
- Savings assume a 35% marginal rate times 85% deduction (fn 6, pp.29-30), because few firms quantify the benefit. Disclosed benefit definitions vary (fn 81).
- ROI is driven by outliers: some firms spending very little returned over $13,000 per dollar (p.30). The authors add a >$1M subsample (48 firms, ROI 243). Mean and median ROI both reported.
- Support classification is uncertain for five lobbying firms. Excluding them raises ROI to 228:1 (fn 100, p.30). Five firms that lobbied against the AJCA are excluded (fn 99, p.28).
- Non-disclosure: Group 3 firms (227) are assumed to have not repatriated or to have repatriated immaterial amounts (p.19). Repatriators below disclosure thresholds are unobserved (fn 10, p.3).
- Measurement of repatriation relies on the 'repat!' keyword search within 50 words of AJCA terms (p.18). Recall of this search is not validated.
- No causal design. Cross-sectional correlations only. The text does not show the regression with lobbying as a regressor, which the introduction says exists (p.3). The only lobbying test is the group comparison in Table 6.
- Logit sample size is not stated. OLS uses ln transforms for amount, assets, leverage and OCF on an ad hoc basis (fn 91), with N=434.
- Internal numeric discrepancies: introduction gives $62.5bn savings and $208bn repatriated by lobbyists (p.3); Table 6 implies $62.05bn and $209.1bn. Introduction gives 496 repatriating firms vs 476 elsewhere (p.3 vs p.18).
- Single tax holiday in one era. External validity to other lobbying or tax provisions is limited. The authors' planned follow-up on the second repatriation holiday is not in the text (p.32).
- Draft status: placeholders in the text (e.g., 'Cite updated Sinai study', fn 35; blank citations in fns 24, 36-37, 42, 65). The working paper has not been finalized.

## Short quotes

- "Firms lobbying for the repatriation provision were quite effective in receiving lucrative tax benefits" (Sec. IV, Conclusion (p.31))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-allocation#Break-even CU spend and ROI floor\|Break-even CU spend and ROI floor]] | qualifies | Overall return on lobbying 220 = (46,157.5 + 15,897.0) / 282.7, i.e. about 62.05bn estimated savings on 282.7M lobbying across 93 lobbying repatriators (FY2004-2006). Coalition 257, non-coalition 154, per-company mean 869, per-company median 172.2, lobbying over 1M subsample (48 firms) 243. (Sec. III.B.2, Table 6 (pp.28-30); fn 97 (upper bound, p.28); fn 99-100 (pp.28-30)) | desk_roi in fec/surfaces/make_member_desk.py returns expected weighted flips gained from the next 10k increment (CHUNK) of CU money on a member. The dashboard head-to-head block solves for the CU money that offsets competitor spend. Neither is a firm return on lobbying dollars. `fec/surfaces/make_member_desk.py:2528-2576 (desk_roi); fec/surfaces/templates/make_dashboard_html_00.tpl:1096-1105` | No numeric transfer. The paper numerator is a firm tax benefit from a one-time provision and the denominator is LDA spend (upper bound). Melange numerator is modeled member movement and denominator is FEC PAC money. Any quote needs the window FY2004-2006, the 93-firm denominator, and the median 172 next to the mean 869. The bearing describes the PAC side as capped at 10k; the code uses 10000 as a marginal increment, so describe it as a per-10k increment. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | context | Lobbying firms repatriated about 70% of dollars: 209.1bn of 298.36bn (Table 6; the introduction says 208bn). Table 5 has no lobbying regressor, so the only lobbying test is the group comparison in Table 6. The introduction promises a regression with lobbying, and no such regression appears in the text. (Table 5 (p.26); Table 6 (p.29); introduction p.3; Sec. IV pp.31-32) | flip_model.py sets WM,WP = 0.35,0.65 and computes pflip as a/(a+b), where the money term saturates against receipts (HALFF times R). A grep of the file finds no reference to lobbying expenditure, so the model takes no lobbying input. `fec/engine/flip_model.py:21 (WM,WP line; the bearing cited line 20, which is off by one)` | Does not bear on WM, WP or HALFF. The paper measures a firm benefit, not member movement, and measures lobbying rather than PAC giving. Furnas (item furnas-nd-conflating-lobbying-pacs) finds only a small minority of organizations do both. The paper gives no basis for raising the money weight. The clause about Paul's ROI enthusiasm has no support in the item file and is dropped. | weak | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | qualifies | Logit (Table 3, N not stated): ln assets .415 (p<.001), pre-tax income indicator .729 (p=.001), OCF/sales 1.918 (p<.001), cash/assets .225 (p=.640); no industry indicator significant. OLS (Table 5, N=434, adj R2 .48): ln assets .915 (p<.001), cash/assets 3.702 (p<.001), OCF/sales 1.679 (p=.023), pre-tax income indicator -.314 (p=.330), SIC 3800 2.196 (p=.085). (Table 3 (p.23); Table 5 (p.26); text p.26 (N=434, adj R2 .48); abstract p.1; Sec. IV p.31) | sgs_config.R defines the four LDA and LobbyView dependent variables (general giving, outside-firm income, self-filer expenses, undifferentiated LobbyView) and records that the undifferentiated LobbyView sum double counts self-filer fees, with a median 9-26 percent by sector. The regression design and the claimed results (no stable growth effect; lobbying lead flips sign under year FE) sit outside the cited lines and were not checked. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_config.R:80-82 (DV_LABELS) and 119-121 (LobbyView double-count note); both line ranges checked` | These are firm-level predictors of capturing a 2004-2006 benefit in a cross-sectional design with no causal identification. They are weak support for a firm-size covariate and say nothing about sector-year giving or growth. Cite as single-era and cross-sectional. Any pooled coefficient over the industry fault line needs strata or an UNSTRATIFIED label. | weak | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Contested-bills registry\|Contested-bills registry]] | suggests-extension | Five firms lobbied against the AJCA for alternative tax plans and are not counted in the lobbying group (fn 99, p.28). A sensitivity analysis drops five repatriating lobbyists with uncertain support and raises ROI to 228:1 (fn 100, p.30). Coalition lobbyists out-returned solo lobbyists (257 vs 154). The paper does not describe the provision as lacking organized counter-lobbying. (fn 99 (p.28); fn 100 (p.30); Table 6 (p.29)) | fec/lobbying/contested_bills.py is a per-bill standard, advisory or contested registry. Its docstring gives ratification 2026-07-31, contested records supply an explicit per-bill counterparty set, and no consumer reads it yet (CONTESTED_V1 is planned). Verified. `fec/lobbying/contested_bills.py:1-15 (module docstring)` | The paper supports only a design implication: returns on a provision with little organized opposition may overstate what contested bills would show. The counter-lobbying evidence is five firms on the other side, so the uncontested label should be softened to five firms lobbied against it. The paper never studies contested bills. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary says size, profitability and operating cash flow predict how much was repatriated. In Table 5 the pre-tax income indicator is not significant (-.314, p=.330). Profitability predicts only the repatriation decision (logit, p=.001).
> - Summary issue: Summary says the upper-bound denominator would double-count in-house and outside-firm LDA dollars. The paper never discusses an in-house vs outside-firm split. Its double-count risk comes from coding lobbying across several categories (fn 97). The Mentat concern is the analyst's inference, not the paper's.
> - Summary issue: Summary says the biggest returns came on an essentially uncontested provision. Footnote 99 reports five firms that lobbied against the AJCA, four of which repatriated anyway. Uncontested is an inference, not a finding.
> - Summary issue: Summary omits the introduction's numeric discrepancies: 496 repatriating firms (p.3) vs 476 in Sec. II (p.18), and 62.5bn savings (p.3) vs 62.05bn in Table 6. These should be noted.
> - Summary issue: Summary's paul_alignment asserted Paul's views (a wish to include competing-industry conflict, a stale note, ROI enthusiasm in the flip-model bearing). Paul's annotation does support that he called the figure an outlier, which is restored in the closing paragraph. The item's paul_entries contain no takeaways or annotations, so these statements have no support in the item file and should be removed or sourced.
> - Summary issue: Summary says the repat! search identified 476 repatriating firms. The search flagged 1,350 candidate firms; 476 firms reported repatriating (Group 1). Wording should say so.
> - Summary issue: Footnote 99 (five firms that lobbied against, excluded from the lobbying group) and footnote 100 (five repatriating lobbyists with uncertain support; ROI 228 when excluded) are different sets. The bearings conflate them.
> - Summary issue: Table 1 Panel A shows repatriators held significantly less cash (cash/assets 15 vs 20 percent), which sits awkwardly with the positive OLS cash coefficient in Table 5. The paper does not reconcile this. Noted, not an error in the summary.
> - Summary issue: Summary's headline figures (220:1, 257, 154, 869, median 172, 243, N=434, adj R2 .48, the cash inconsistency) all check out against the text. Coverage: all four bearings and all six related links were checked. Key_findings and paul_markup are not in this output schema and were not returned.

## Gaps and flags

- The system has no measure of the policy value at stake per bill or provision (dollar rents). The paper shows this, not legislator movement, is where very large 'returns' appear.
- There is no coalition-membership signal for industry actors. The paper reports coalition firms returning 257:1 vs 154:1 for solo lobbyists, without strata or tests.
- No component separates provisions with concentrated benefits and little organized opposition from contested bills when interpreting returns. The contested-bills loader exists but has no consumer.
- The paper's 220:1 return is a firm-level policy-rent ratio on LDA spend. It must not be relabeled as a return on PAC money or a per-member persuasion rate in Melange's ROI or break-even surfaces.
- D-021: the paper uses LDA dollars as an ROI denominator (a weight). Any proposal to import LDA spend as an edge or weight in Melange's engines conflicts with D-021 (LDA is an identity source only).
- Denominator hazard: the paper uses the 'broadest coded' lobbying total as an upper bound. Mentat rules keep in-house expenses and outside-firm income separate and never summed (giving_forecast.py:379,554,2074), and sgs_config.R:119 records a 9-26% self-filer double count. A replication must not add the two series.
- Quoting rule: the coalition (257) vs non-coalition (154) gap and the 220 headline are pooled over a heavily outlier-driven distribution (mean 869 vs median 172). Quote them with window and denominator (FY2004-2006, 93 lobbying repatriators) and with the median.
- Internal inconsistency in the paper: the abstract and conclusion say cash is not associated with the repatriation amount, but Table 5 has cash/assets 3.702, p<.001. Do not cite the abstract's claim.

Partly aligned. In his Trey bibliography Paul calls this study an outlier in the size of its findings, and a dramatic illustration of the ROI that lobbying brings to firms that pass legislation. The paper is consistent with the outlier reading: it reports a per-company mean of 869 against a median of 172 and adds a >$1M subsample (243) because tiny spenders reach 13,000:1. His wider 'lobbying as ROI' framing is not supported at the legislator level. The paper never observes members, so it cannot justify the system moving away from its conservative 'money buys access, not votes' flip model. The system's ROI (desk_roi, flips per $10k PAC) is a different quantity from the firm-rent sense of ROI in this paper.

## Related papers

- [[huneeus-2018-effects-firms-lobbying]] — complements — Item exists. Firm lobbying returns or rents theme is plausible; content not checked beyond existence.
- [[bickmore-2025-corporate-lobbying-political]] — complements — Item exists. Link between corporate lobbying and firm size or concentration is plausible; content not checked beyond existence.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Item exists and its summary confirms low overlap: most organizations do only one of lobbying or PAC giving (1998: 12 percent both; 2008: 10 percent both). Supports the transfer limit.
- [[kim-2018-lobbyview-firm-level]] — same-data — Item exists. Both rest on firm-level LDA filings; the claim that LobbyView is the processed form of the same Senate records was not checked.
- [[bombardini-2020-empirical-models-lobbying]] — complements — Item exists and its summary is a review of empirical lobbying designs, including identification of policy supply and demand. Relevant to the identification gap; confirmed from summary.
- [[pritoni-2025-lobbying-coalitions-determinants]] — complements — Item exists and its summary is a review of lobbying coalition research (80 articles). Relevant to the coalition-gap point; confirmed from summary.

### Linked from
_None._