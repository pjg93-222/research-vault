---
type: paper
title: "Corporate lobbying, political action committees, and market concentration"
authors:
  - "Elizabeth Bickmore"
  - "Benjamin M. Blau"
  - "Todd Griffith"
  - "Ryan J. Whitby"
year: "2025"
venue: "Public Choice"
doi: "10.1007/s11127-025-01337-7"
category: "Lobbying"
systems:
  - "melange-allocation"
  - "melange-lobbying"
  - "mentat-sector-growth-study"
components:
  - "mentat-sector-growth-study.growth-spending-regressions"
  - "melange-allocation.break-even-and-roi"
  - "mentat-sector-growth-study.seats-weekly-test"
  - "melange-lobbying.fec-lda-bridge"
paul_annotated: true
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/bickmore-2025-corporate-lobbying-political.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\s11127-025-01337-7.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\s11127-025-01337-7.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\s11127-025-01337-7.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\s11127-025-01337-7.pdf"
  - "C:\\Users\\pgoff\\Downloads\\s11127-025-01337-7.pdf"
sha12:
  - "535cf83bf4d4"
tags: [paper, lobbying, type/paper, theme/lobbying, evidence/verified]
---

# Corporate lobbying, political action committees, and market concentration

**Elizabeth Bickmore, Benjamin M. Blau, Todd Griffith, Ryan J. Whitby (2025).** *Public Choice.* [10.1007/s11127-025-01337-7](https://doi.org/10.1007/s11127-025-01337-7) Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Bickmore, Blau, Griffith and Whitby (Public Choice, 2025) ask whether corporate lobbying and PAC giving raise a firm's share of its NAICS industry. They merge Compustat with Center for Responsive Politics lobbying and PAC data for 1998-2016 (237,849 firm-years; 116,242 with full controls). Lobbying appears in 4.23 percent of firm-years (the paper's text says of firms). With industry and year fixed effects, firm-clustered errors and no firm fixed effects, a lobbying firm's share of industry assets is 0.0743 higher (Table 3, full controls; 0.0907 without controls). Because the outcome is a 0-1 share with mean 0.0727 and median 0.0010, that is 7.4 share points, about the size of the mean share. The paper's abstract and text call the same figure 7.4 percent higher, and its Exxon example treats it as relative (12 to nearly 13 percent), which conflicts with its share-point units. Results for market-cap and sales shares are similar. PAC contributors show about 0.11 higher share (election years, N = 62,198). Democratic and Republican giving show similar coefficients. Money to winning candidates is significant (0.062, SE 0.027) and money to losing candidates is not (0.040, SE 0.036), but the paper never tests the difference between the two. For causal evidence the authors use a difference-in-differences around the January 2006 Abramoff plea, 2003Q1-2009Q1. Lobbying firms' share falls 2.65 share points (assets) and 3.36 (market cap) relative to non-lobbyists, which the paper reads as percent. However, the pre-trend begins to bend in late 2004, and the placebo results appear only in figures. For Paul's systems this is firm-level evidence about economic returns to political activity. It does not measure member behaviour. Its main use is as a caution on how the ROI and deep-pockets readings in Mentat's sector growth study and Melange's break-even widget are worded. The pooled estimate is UNSTRATIFIED by industry (interaction checks are described but not tabulated).

## Paul's annotation

From [[annotations/Trey - Articles - Report - Annotated Bibliography|Trey - Articles - Report - Annotated Bibliography]]:

> Bickmore, Elizabeth, et al. “Corporate lobbying, political action committees, and market concentration.” *Public Choice*, 17 Oct. 2025, [https://doi.org/10.1007/s11127-025-01337-7](https://doi.org/10.1007/s11127-025-01337-7).
>
> A recent study that robustly tests the idea of political lobbying as a means of enhancing company market share, using various linear and multivariate tests and controlling for industry fixed effects. The authors find that companies that deploy lobbying firms exhibit a **7.5% to 10.5%** higher market concentration than companies of comparable size in the same industry.

## Study

**Question:** Do firms' political activities (corporate lobbying and PAC contributions) raise their share of industry sales, assets or market capitalization, and is any such association causal rather than reflecting larger firms lobbying more?

**Theory:** Public choice rent-seeking (Tullock 1967; Stigler 1971; Tollison 1982) and policymaker rent extraction (McChesney 1987, 1997; Coate and McChesney 1992). Political connections are argued to create barriers to entry and weaken competition, raising market concentration.

**Data:** Compustat firm financials merged by company name with Center for Responsive Politics lobbying and PAC data (from Senate Office of Public Records disclosures). About 26,500 unique firms. Annual panel 1998-2016: 237,849 firm-year observations for the baseline ShareAssets regressions (116,242 with complete controls); text says roughly 250,000. Lobbying in 4.23 percent of firm-years; mean LobbyAmt 33,764 dollars, maximum above 45 million. PAC tests use election years: N = 62,198 (Table 6) and 62,558 (Table 7). Difference-in-differences uses quarterly data 2003Q1 to 2009Q1 with N = 139,711 in full-control specifications. Unit of observation is the firm within NAICS industry.

**Method:** OLS panel regressions of firm market share (firm total divided by NAICS industry total for assets, market cap or sales) on a Lobby indicator or ln(LobAmt+1), with firm controls (Assets, D/E, current ratio, ROE, asset turnover, profit margin, price range, share turnover), industry and year fixed effects, and firm-clustered standard errors. PAC regressions use PAC, DemPAC and GopPAC indicators, then WinPAC, LosePAC and OtherPAC split by election outcome. Robustness: univariate correlations, industry-by-lobbying interactions, exclusion of top and bottom 5 percent by market cap, SIC instead of NAICS codes.

**Identification:** Difference-in-differences around the Abramoff guilty plea (3 January 2006), with Post = 2006Q1 to 2009Q1. Treated firms are lobbying firms (Lobby or ln amount); controls are non-lobbying firms. Eq. 3 includes Treatment, Post, the interaction, controls, and industry and year fixed effects; no firm fixed effects are stated. Parallel pre-trends checked visually (Fig. 1). Five randomly drawn placebo event dates (Figs 2-4). Strategy follows Borisov, Goldman and Gupta (2016), who treat the plea as a negative shock to lobbying returns.

**Scope:** Firms in Compustat matched to CRP lobbying and PAC data, 1998-2016 for annual tests and 2003-2009 for the event study. Post-1998 lobbying disclosure era. US firms implied by the Senate and Abramoff context. Concentration measured within NAICS industries. Not a full population of firms, since only name-matched firms enter.

## Key findings

- Cross-section: lobbying firms have higher industry concentration. Lobby coefficient on ShareAssets is 0.0907 with no controls and 0.0743 with full controls, described as about 7.4 percent higher. Numbers: 0.0907 and 0.0743, SE 0.0078 and 0.0094, all p<0.01. (Section 4.2, Table 3 columns 1 and 3; abstract)
- Same pattern for ShareMcap and ShareSales: Lobby coefficients 0.0996 and 0.0804 (market cap) and 0.0936 and 0.0759 (sales), baseline and full-controls. Numbers: ShareSales text range 7.6 to 9.9 percent; ShareMcap text 'more than 8 percent'. (Section 4.2, Tables 4 and 5)
- Lobbying intensity: ln(LobAmt+1) is positive and significant across measures and specifications. Numbers: ShareAssets 0.0078 baseline, 0.0062 with controls; SE 0.0007 and 0.0008. (Section 4.2, Table 3 columns 4-6)
- PAC contributors have about 0.11 higher industry share (the paper says 11 percent) than non-contributors, with full controls and industry and year fixed effects. Numbers: PAC 0.1100 (ShareAssets), 0.1226 (ShareMcap), 0.1062 (ShareSales); N = 62,198. (Section 4.3, Table 6 odd columns)
- Party split: DemPAC and GopPAC coefficients are similar and significant, so the authors find no systematic partisan difference. Numbers: DemPAC 0.0717, 0.0659, 0.0641; GopPAC 0.0678, 0.0847, 0.0718. (Section 4.3, Table 6 even columns)
- Winning-candidate PAC money drives the PAC result; contributions to losing candidates show no effect. Numbers: WinPAC 0.0620, 0.0710, 0.0603 (significant at 5 percent); LosePAC 0.0404, 0.0306, 0.0423 (not significant); OtherPAC 0.0495, 0.0696, 0.0504. (Section 4.3, Table 7; conclusion)
- Abramoff plea DiD, assets: Treatment x Post falls relative to non-lobbying firms, a 2.65 share-point decline (read by the paper as percent) in the three post years with full controls; authors say over 30 percent of the pre-event average. Numbers: Lobby -0.0180, -0.0220, -0.0265 (SE 0.0096 full); ln -0.0015, -0.0018, -0.0023. (Section 4.4, Table 9 column 3 and column 6 text)
- Abramoff plea DiD, market cap: larger decline in the full specification, read by the paper as a 3.4 percent drop (3.36 share points). Numbers: Lobby -0.0336 (SE 0.0099); ln -0.0030 (SE 0.0008); all significant at 1 percent. (Section 4.4, Table 10 column 3)
- Abramoff plea DiD, sales: full specification significant at 5 percent; reduced specification not significant. Numbers: Lobby col 3 -0.0219 (SE 0.0093); col 1 -0.1218 (SE 0.0776, n.s.); col 2 -0.0176 (10 percent); ln col 6 -0.0020. (Section 4.4, Table 11)
- Placebo: the actual Abramoff estimate lies below the lower bound of the placebo confidence intervals for all three concentration measures, which the authors read as a causal response. Numbers: Five random placebo dates plus their average; no numeric placebo values given in text. (Section 4.4, Figures 2-4 (prose only; no placebo table))
- Pre-trend caveat: lobbying firms' concentration begins to fall in late 2004, before the January 2006 plea. The authors attribute this to the Senate Indian Affairs Committee investigation and still judge parallel trends satisfied. Numbers: Shift from late 2004 (quarterly data). (Section 4.4, text after Fig. 1 discussion; Fig. 1)
- Robustness claims not tabulated: industry-by-lobbying interactions leave results intact; excluding top and bottom 5 percent by market cap keeps Lobby, ln and PAC significant; SIC codes give similar results. Numbers: None reported in text. (Footnotes 9, 10 and 11)

## Limitations

- Paper has no limitations section; the caveats below are the ones the authors state or that the design text makes explicit.
- Pre-event decline in lobbying firms' concentration starts late 2004, before the 3 January 2006 plea; authors attribute it to the investigation.
- Causal claim rests on one event shock with three post years (2006Q1-2009Q1); placebo dates are randomly drawn and placebo values are shown only in figures.
- Robustness checks (SIC codes, industry interactions, excluding top and bottom 5 percent) are described but not tabulated.
- Lobbying and PAC data are matched to Compustat by company name; match quality is not evaluated in the text.
- Stated specifications include industry and year fixed effects but no firm fixed effects.
- Internal inconsistencies: text says roughly 250,000 observations versus 237,849 in tables; Table 6 and 7 notes say 1998-2018 while text says 2000-2016 election years; Table 8 caption describes 2003Q1-2005Q4 while text describes 2003Q1-2009Q1; intro gives 1.8 to 3.4 percent versus Table 9 to 10 coefficients of -2.65 and -3.36 percent.
- Lobbying is rare (4.23 percent of firm-years) and heavily skewed; the authors report intensity results alongside the indicator.

## Short quotes

- "lobbying firms experienced a significant decline in market concentration, relative to non-lobbying firms" (Section 4.4 Difference-in-difference regressions (also Conclusion))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | qualifies | Lobby coefficient on ShareAssets is 0.0743 (SE 0.0094) with full controls and 0.0907 with none; ShareMcap 0.0804 and ShareSales 0.0759 at full controls. Industry and year FE, no firm FE. Full-control sample N = 116,242 firm-years (1998-2016). ShareAssets mean 0.0727, median 0.0010. Coefficients are level shifts in a 0-1 share. (Section 4.2; Table 3 columns 1 and 3; Table 4 column 3; Table 5 column 3; summary statistics table) | The sector growth study runs sector-by-year regressions of LobbyView real dollars on growth and sector controls. Verified: L3-LV base coefficient 0.352 and year-FE variant -0.477 (sgs_review.R line 33), and the year-effects guard at sgs_estimate.R line 386. The clause that growth added no adjusted R2 was not located and is not asserted here. `Mentat/sector_growth_study/code/R/sgs_registry.R:34 (L-control set), :155 (L-LV spec); code/R/sgs_estimate.R:386; code/R/sgs_review.R:33 (paths under C:/Users/pgoff/Mentat)` | The paper estimates a between-firm cross-sectional association within NAICS industries, with lobbying on the right-hand side. The sector study asks a within-sector, over-time question with lobbying as the outcome. The paper has no within-firm or within-sector-over-time evidence, so it cannot support the growth-to-lobbying lead. Pooled across industries, so label UNSTRATIFIED. Imai and Kim caution is the analysts own point, not the paper's. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Break-even CU spend and ROI floor\|Break-even CU spend and ROI floor]] | qualifies | Lobby on ShareAssets 0.0743 with full controls; PAC on ShareAssets 0.1100 (election years, N = 62,198). Abramoff DiD Treatment x Post on ShareAssets with full controls is -0.0265 (SE 0.0096); on ShareMcap it is -0.0336 (SE 0.0099). Post window 2006Q1-2009Q1; pre-period 2003Q1-2005Q4 per the Table 8 description. (Sections 4.2-4.4; Tables 3, 6, 9, 10; Table 8 description (text line about 1104)) | The dashboard head-to-head block computes CU versus industry flip odds using WM, HALFF, the competitiveness multiplier and receipts, then solves for break-even CU dollars. When q is at least 1 the displayed text reads that CU money alone cannot fully offset the competitor spend. The rel's quoted message 'cannot offset' is not the literal wording. `fec/surfaces/templates/make_dashboard_html_00.tpl:1096-1105 (formula and message verified). fec/engine/money_response.py:31 is the LOSSFRAME default (1.10), not the share ratio, so cite it only for the loss-frame parameter. Paths under C:/Users/pgoff/OneDrive/Desktop/Melange Systems.` | The paper's return is a firm-level industry-share outcome from Compustat matched to CRP by company name, 1998-2016. It does not measure votes, cosponsorship or flips, so it cannot calibrate WM, HALFF or LOSSFRAME. It supports only a framing claim that political spending associates with firm share gains, reported in share points with the unit stated. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#SEATS-W weekly seat-gain test (2022 House)\|SEATS-W weekly seat-gain test (2022 House)]] | qualifies | WinPAC 0.0620 (SE 0.0267, significant at 5 percent) and LosePAC 0.0404 (SE 0.0356, not significant) in the Table 7 region. The authors conclude contributions to winning candidates drive the PAC effect. The text reports no equality test of WinPAC against LosePAC. (Section 4.3, Table 7 and the text after it) | sgs_seats.R defines the SEATS-W estimand A (EXPLORATORY): weekly Democratic share change regressed on the prior-week change in expected Democratic seats, with residual block bootstrap within cycle. The rel's -3.47 pp estimate and its bootstrap p-values were not located in the sector growth study folder and are not asserted here. `Mentat/sector_growth_study/code/R/sgs_seats.R:15-18 (estimand description verified; numeric result not located)` | Bickmore classifies money by realised election outcome after the fact and does not observe donors' responses to forecasts, so it cannot show donors anticipate winners. WinPAC versus LosePAC is a significant versus non-significant comparison with overlapping intervals, so treat it as a hypothesis. The paper's outcome is firm share, not weekly Democratic share. | weak | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#FEC to LDA bridge test\|FEC to LDA bridge test]] | context | Lobbying (Tables 3-5, annual N = 116,242) and PAC (Tables 6-7, election-year N = 62,198 and 62,558) are estimated in separate regressions on different samples. The paper does not estimate them jointly, and 'never in sequence' is an inference from table structure, not an explicit statement. (Sections 4.2-4.3; Table 6 and Table 7 notes) | fec_lda_bridge3_look.py builds a Pearson rho and a partial correlation controlling for party, chamber, volume and committee (CONTROL_NAMES line 57; partial() and result dict around lines 1066-1085). The four-control partial of -0.0002 appears in lobby_forecast.py and lobby_champions.py comments. The raw rho of +0.153 was not located in the cited folder and is not asserted here. `fec/lobbying/fec_lda_bridge3_look.py:57 and 1066-1085 (verified); Melange paths under C:/Users/pgoff/OneDrive/Desktop/Melange Systems` | The paper gives no evidence on whether PAC giving leads lobbying or how much the two overlap, so it neither revives nor refutes the bridge. The claim that Furnas et al. show the sharper overlap relation comes from the vault, not from this paper. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The summary says the 0.0743 coefficient is about the size of the mean share and not a 7.4 percent relative rise. The paper's abstract and text call it 7.4 percent higher, and its Exxon example treats it as relative (12 to nearly 13 percent). The summary should state that the paper's wording conflicts with its share-point units.
> - Summary issue: The summary says lobbying appears in 4.23 percent of firm-years. The paper text says 4.23 percent of firms in the sample; Table 1 is firm-year level, so the figure stands but the wording should match.
> - Summary issue: The summary says nothing in Melange measures market concentration. No repo-wide search was run for this claim, so it is unverified here.
> - Summary issue: The paper alignment says Paul's comparable-size firms overstates the control. The full specifications do control for Assets, so the defensible point is residual size confounding without firm fixed effects.
> - Summary issue: The flags say lobbying is concentrated in the largest firms. The paper does not say this; it reports 4.23 percent of firm-years with lobbying and a robustness check excluding the top and bottom 5 percent by market cap. Remove the claim.
> - Summary issue: The flags say Paul's 7.5 percent low end is off. Confirmed: the abstract and text give 7.4 percent; the ext record's conclusion reading of 7 percent is a rounding of the same figure.
> - Summary issue: The Table 6-7 notes give 1998-2018 while the text gives 2000-2016 election years, and the sample ends in 2016. Confirmed as an internal inconsistency.
> - Summary issue: The flag about the pre-trend starting in late 2004 and the placebo results appearing only in figures is confirmed (text around lines 1017-1024 and 1389-1397; Figures 2-4 hold the numbers).

## Gaps and flags

- The ROI framing in break-even and the sector growth study has no firm-level outcome. Mentat's roster companies could be joined to Compustat or a share-of-sector measure to test returns directly, but that would be new analysis.
- Neither system separates political money by realised winner versus loser at the donor level. Mentat's beneficiary direction partition (support, oppose, party) could carry that cut, but the attributed-estimate producer is unbuilt.
- The system has no event-study instrument for lobbying returns. The Abramoff plea design (after Borisov, Goldman and Gupta 2016) is a possible template for credit-union versus bank exposure, though Paul has not ruled on it.
- Unit conflict: the coefficients are level shifts in a 0-1 industry share (mean 0.0727, median 0.0010, 1998-2016 Compustat-CRP firms), yet the paper's own text calls them percent higher. Paul's '7.5% to 10.5% higher market concentration' reads them as relative, and the low end in the paper is 7.4 (0.0743).
- Paul's 'comparable-size firms': the full specifications do control for Assets, so the defensible point is residual size confounding without firm fixed effects.
- The pooled coefficient spans all NAICS industries. Industry-by-lobbying interactions and the SIC robustness check are described in footnotes but not tabulated, so any figure quoted should carry an UNSTRATIFIED label.
- The causal claim rests on one event with three post years. Lobbying firms' share starts falling in late 2004, before the January 2006 plea, and placebo values appear only in figures.
- Internal inconsistencies: 'roughly 250,000' versus 237,849 observations; the Table 6-7 notes say 1998-2018 while the text says 2000-2016; the introduction gives 1.8-3.4 percent versus the -2.65 and -3.36 coefficients.
- The WinPAC versus LosePAC conclusion ('only when directed toward successful candidates') is not backed by an equality test.
- No brain-rule conflict: the paper uses CRP lobbying (LDA-derived) as firm-level spending, not as a member edge (consistent with D-021). It proposes none of the rejected SNA changes, and its influence wording is not about revolving-door edges.

The direction agrees: lobbying and PAC firms hold larger industry shares under industry and year FE. Three details differ: the figures are share points, though the paper's text calls them percent; the low end is 7.4, not 7.5; and 'comparable-size' glosses residual size confounding in a design without firm FE. His note omits the Abramoff DiD, which carries the causal claim and has a pre-trend. It is stale against the system: the sector growth study found the lobbying lead flips under year FE.

## Related papers

- [[huneeus-2018-effects-firms-lobbying]] — complements — Huneeus and Kim are not cited in this paper. The link is topical only: firm-level lobbying and firm size or revenue. Keep as thematic, not paper-grounded.
- [[alexander-2009-lobbying-analysis-under]] — complements — Alexander et al. are not cited in this paper; the only Alexander reference is Borisov, Alexander, Goldman and Gupta 2016, a different work. Keep as thematic only.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Furnas is not cited in this paper. The paper's separate lobbying and PAC regressions make the overlap question topically relevant. Keep as thematic only.
- [[kim-2020-strategic-sequential-links]] — complements — Not cited in this paper, and the paper does not test sequence from donations to lobbying. Keep as thematic only.
- [[kim-2018-lobbyview-firm-level]] — complements — Not same data: Bickmore uses CRP lobbying matched to Compustat by company name, not LobbyView. Relation changed from same-data to complements because the enum has no context value.
- [[imai-2021-fixed-effects-regression]] — complements — Imai is not cited in this paper. The paper's industry and year FE without firm FE makes the two-way FE caution topically relevant. Keep as analyst context.

### Linked from
- [[bombardini-2020-empirical-models-lobbying]] — complements — Both relate firm lobbying to firm size, the stylized fact the review reports from Huneeus and Kim.
- [[alexander-2009-lobbying-analysis-under]] — complements — Item exists. Link between corporate lobbying and firm size or concentration is plausible; content not checked beyond existence.
- [[fremeth-2018-spillovers-regulating-corporate]] — same-data — Real item (title: Corporate lobbying, political action committees, and market concentration). Same-data claim not verified beyond title.
- [[kerr-2014-dynamics-firm-lobbying]] — complements — Item title links corporate lobbying and PACs to firm size and market concentration, extending Kerr's size gradient. Kerr does not cite it.
- [[note-industry-growth-and-political-spending-research]] — complements — Item record exists as an ext file only. Content and the rel's market-concentration link were not re-read.
- [[note-review-industry-growth-and-political-spending]] — complements — Title covers corporate lobbying, PACs and market concentration; the review asks for concentration and active-actor counts, so the topical link holds.
- [[note-sector-growth-and-political-spending-source]] — complements — Its title covers corporate lobbying, PAC spending and market concentration, a related economic-driver design.