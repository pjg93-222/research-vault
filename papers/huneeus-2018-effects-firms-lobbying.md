---
type: paper
title: "The Effects of Firms' Lobbying on Resource Misallocation"
authors: ["Federico Huneeus (Princeton University)", "In Song Kim (MIT)"]
year: "2018"
venue: "SSRN working paper (SSRN 3275097); first draft Oct 29, 2018, this draft Nov 12, 2018. No journal publication stated."
doi: ""
category: "Lobbying"
systems: ["melange-demographic-potential", "melange-influence-engine", "melange-lobbying", "mentat-giving-forecast", "mentat-sector-growth-study", "moneyball-rank-pillars"]
components: ["mentat-sector-growth-study.growth-spending-regressions", "mentat-giving-forecast.refit-gates", "moneyball-rank-pillars.committee-pillar", "melange-lobbying.finance-client-classification", "melange-influence-engine.flip-model", "melange-demographic-potential.footprint-v30"]
paul_annotated: true
paul_markup_count: 68
evidence_status: "verified"
source: "[[_sources/huneeus-2018-effects-firms-lobbying.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\ssrn-3275097.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\ssrn-3275097.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\ssrn-3275097.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\ssrn-3275097.pdf", "C:\\Users\\pgoff\\Downloads\\ssrn-3275097.pdf"]
sha12: ["a18804866401", "357474c4a88d"]
tags: [paper, lobbying]
---

# The Effects of Firms' Lobbying on Resource Misallocation

**Federico Huneeus (Princeton University), In Song Kim (MIT) (2018).** *SSRN working paper (SSRN 3275097); first draft Oct 29, 2018, this draft Nov 12, 2018. No journal publication stated..* Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Huneeus and Kim link federal LDA lobbying reports (1999-2015 filings, about 972k report rows) to US public firms in Compustat. They ask two questions: does lobbying make a firm bigger, and what does that cost the economy in productivity?
> 
> **Descriptive facts**
> - Lobbying is rare: 766 of 7,646 public firms lobbied in 2017.
> - It is very persistent: more than 80% of 2016 lobbyists lobbied again in 2017, with an intensive-margin slope of 0.922.
> - It runs to more than 6 times PAC-type campaign contributions.
> - Most bills draw a median of 2 lobbying clients.
> 
> **Causal estimate.** The instrument is a shift-share. For each firm it combines its lagged share of lobbied bills going to each committee with the committee seats held by politicians from its headquarters state. The IV elasticity of sales on lobbying is 0.319 for all years (first-stage F 14.3) and 0.227 after 2007 (F 13.1). OLS gives about 0.04-0.05. The paper's "3 percent per 10 percent" headline is consistent with the full-sample estimate (0.319 x 10), though the paper does not name the column (inference), while its stated preferred estimate is 0.2 percent per 1 percent.
> 
> **Structural model.** A Hsieh-Klenow/Melitz model is fitted by SMM with an identity weighting matrix. With sigma_C fixed at 4, removing the return to lobbying raises aggregate productivity by 22%. That figure moves from 38% to 13% as sigma_C goes from 2 to 6.
> 
> **What it means for Paul's systems.** Nothing here estimates how a member of Congress responds to money. The usable pieces are narrower:
> - The committee-relevance weight is computed from data, where the committee pillar's tier weights are owner judgment.
> - The persistence facts explain why naive carry-forward is hard to beat in Mentat's lobbying forecasts.
> - The paper's causal direction (lobbying leads to revenue) is a simultaneity warning for the sector-growth study's growth-leads-to-lobbying regressions.
> - Appendix A's fuzzy matching with a 5% random sample is a template for a recall audit (finding missed names) of the LDA client matching; it does not test classifier precision.

## Paul's annotation

From [[annotations/Annotated Bibliographies & Literature Synthesis - Curated Annotations|Annotated Bibliographies & Literature Synthesis - Curated Annotations]]:

> Huneeus, Federico, and In Song Kim. “The effects of firms’ lobbying on Resource Misallocation.” *SSRN Electronic Journal*, 12 Nov. 2018, [https://doi.org/10.2139/ssrn.3275097](https://doi.org/10.2139/ssrn.3275097).
>
> This article by Massachusetts Institute of Technology Professor In Song Kim, and Princeton Professor Federico Huneeus examines the optimization, and more specifically mis-allocation, of company revenue on political lobbying. The main finding is that a 10% increase in lobbying expenditures leads to a 3% gain in revenue. Additionally, the article introduces a novel methodology used to capture this finding, using the general equilibrium firm model that features endogenous variables. The fundamental application of this paper to the present research is being able to set fundraising goals, and use the current models as a basis to help inform the appropriate amount for future targets and institutions. However, there are several gaps within the literature as they pertain to the present research. First, is that the model does not distinguish campaign donations from events, meetings, etc. Second, is that the models make no differentiation between the representatives, however its state layer can shed insight as to who should be targeted based on general industry presence. In all, Professor Kim delivers another key step into the optimization and empirical application of mathematical models and principals to the realm of political advocacy, serving as the basis for business-applicable models.

From [[annotations/Trey - Articles - Report - Annotated Bibliography|Trey - Articles - Report - Annotated Bibliography]]:

> Huneeus, Federico, and In Song Kim. “The effects of firms’ lobbying on resource misallocation.” *SSRN Electronic Journal*, 2018, [https://doi.org/10.2139/ssrn.3275097](https://doi.org/10.2139/ssrn.3275097).
>
> “*We find that a 10 percent increase in lobbying expenditure leads to a 3 percent gain in revenue.*” This article examines both the macro and micro-level effects of company spending on lobbying, showing a robust increase in individual company revenue, if at the expense of the broader market production, not industry-specific, but broader economy as a whole.

From [[annotations/Lobbying - Lobbying - Annotated Bibliography|Lobbying - Lobbying - Annotated Bibliography]]:

> Huneeus, Federico, and In Song Kim. “The effects of firms’ lobbying on Resource Misallocation.” *SSRN Electronic Journal*, 12 Nov. 2018, [https://doi.org/10.2139/ssrn.3275097](https://doi.org/10.2139/ssrn.3275097).
>
> This article by Massachusetts Institute of Technology Professor In Song Kim and Princeton Professor Federico Huneeus examines the optimization, and more specifically misallocation, of company revenue on political lobbying. The main finding is that a 10% increase in lobbying expenditures leads to a 3% gain in revenue. Additionally, the article introduces a novel methodology to capture this finding, employing a general equilibrium firm model with endogenous variables. The fundamental application of this paper to the present research is the ability to set fundraising goals and use current models as a basis to inform appropriate amounts for future targets and institutions. However, there are several gaps within the literature as they pertain to the present research. First, the model does not distinguish campaign donations from other activities, such as events and meetings. Second, the models make no distinction between the representatives; however, their state layer can provide insight into who should be targeted based on general industry presence. In all, Professor Kim delivers another key step toward the optimization and empirical application of mathematical models and principles in the realm of political advocacy, serving as a basis for business-applicable models.

## Paul's markup in the PDF

- p1 Highlight: "through the distortion of firm size" — 
- p1 Highlight: "their headquarter locations and politicians' districts in the U.S., and trace the value of these networks over time by exploiting politicians' assignment to congressional committees" — 
- p1 Highlight: "a 10 percent increase in lobbying expenditure leads to a 3 percent gain in revenue." — 
- p1 Highlight: "Our counterfactual analysis shows that the return to firms' lobbying activities amounts to a 22 percent decrease in aggregate productivity in the U.S." — 
- p2 Highlight: "by saving and thus accumulating capital, firms can avoid financial constraints" — 
- p2 Highlight: "by choosing different buyers, firms can influence the techniques other firms use to produce" — 
- p2 Highlight: "is their capacity to influence policy-making directly through lobbying" — 
- p2 Highlight: "This dataset not only includes firms' lobbying expenses but also identifies to which congressional committees the lobbying activity is being exerted" — 
- p2 Highlight: "new instrumental variable (IV)." — 
- p2 Highlight: "the value of firms' connections with politicians by tracing the assignment of those politicians to different congressional committees over time" — 
- p2 Highlight: "The identification assumption is that firms cannot influence committee membership." — 
- p3 Highlight: ". We find that a 10 percent increase in lobbying expenditure leads to 3 percent revenue gains." — 
- p3 Highlight: "heterogeneity in productivity, selection into production, and endogenous entry." — 
- p3 Highlight: "Although we impose a functional form assumption for this mapping in our quantitative analysis, we provide one micro foundation for this assumption through a simple game between a policymaker and firms" — 
- p3 Highlight: "We find that the policymaker rewards firms' lobbying differentially as long as it is not possible for her to perfectly substitute individual lobbying expenses." — 
- p4 Highlight: "interest groups that lobby on any given lobbied bill in the last 20 years is just tw" — 
- p4 Highlight: "The second is that, through a general equilibrium effect, wages decline so that entry becomes cheaper increasing the number of firms in the economy." — 
- p4 Highlight: ". To do this, we develop a general equilibrium firm model that features endogenous lobbying and estimate this model to evaluate the macroeconomic effects of firms' lobbying activities." — 
- p5 Highlight: "First, we provide a new instrument to identify the causal effect of firms' political activity on their economic performance." — 
- p5 Highlight: "Second, we quantify not only the firm-level effects of lobbying but also its macroeconomic effects." — 
- p5 Highlight: "Finally, we build a novel dataset that contributes to the rapidly growing empirical literature that examines interest group lobbying" — 
- p5 Highlight: "We find that firm-level lobbying expenditures are significantly larger than that by industry-organizations." — 
- p6 Highlight: "First, we establish a direct link between lobbying clients (i.e., firms) and the list of public firm" — 
- p6 Highlight: "To the best of our knowledge, researchers have either studied firms and trade associations at the level of sectors" — 
- p6 Highlight: "&P 500 corporations (e.g., Bombardini and Trebbi, 2012; Bertrand et al., 2018).5 We overcome this problem and study political behavior of all publicly trading firms from 1999 to 2018. Specifically, we utilize natural language processing, name entity matching algorithms, and manual" — 
- p7 Highlight: "measure the relative importance of congressional committees for each individual firm i at year t by considering the complete list of bills that have been lobbied by the firm up to t −1." — 
- p7 Highlight: "We improve upon this approach by exploiting the direct link between bills that are actually lobbied by individual firms and committees where the bills are considered." — 
- p9 Highlight: "actively lobbying by establishing their own in-house lobbying departmen" — 
- p9 Highlight: "For example, only 6.5% of manufacturing firms employ in-house lobbyists." — 
- p9 Highlight: "Re- searchers still find that campaign contributions are relatively smaller than public spending of the government" — 
- p10 Highlight: "An important distinction that has been studied in the literature is that firms that engage in lobbying tend to be larger than politically inactive firms in the extensive margin" — 
- p10 Highlight: "that the positive correlation between firm size and lobbying expenditure holds in the intensive margin as well." — 
- p11 Highlight: "The right panel shows that there exists a positive and robust correlation between the lobbying expenses in the internsive margin conditional on lobbying in both years" — 
- p11 Highlight: "as we rely on the exogenous increases in the value of lobbying through political connections rather than a strategic response in the amount of lobbying expenses at the firm-level when we evaluate the economic effect of their lobbying activities." — 
- p12 Highlight: "we find that firms' individual lobbying activities are at least as prevalent as those by industry organizations" — 
- p12 Highlight: "Most congressional bills are lobbied by only one or two interest groups." — 
- p13 Highlight: "we develop a heterogeneous firm model with lobbying decisio" — 
- p13 Highlight: "generalizes Hsieh and Klenow (2009)'s framework along the lines of Melitz (2003)." — 
- p14 Highlight: "Section 3.2 presents the second step whereby we provide one micro foundation for the mapping assumed in the previous step." — 
- p14 Highlight: "Grossman and Helpman (1994)'s lobbying model." — 
- p14 Highlight: "selection into production and firm entry" — 
- p14 Highlight: "Lobbying activity entails benefits and costs." — 
- p14 Highlight: "φP, φL and φτ is a Hicks-neutral productivity term, a lobbying productivity term, and an exogenous wedge term, respectively" — 
- p14 Highlight: "all firms that produce varieties with the same φ" — 
- p15 Highlight: "Firms face output distortions τs(·)." — 
- p16 Highlight: "ixed production and lobbying cost, firms' production and lobbying extensive margin decisions" — 
- p17 Highlight: "Similarly, firms with φ = φ∗∗choose to produce and lobby, but gain zero net profits" — 
- p19 Highlight: "This implies that those firms' productivity will influence aggregate productivity more than what they should in the absence of distortions" — 
- p19 Highlight: "In exchange for lobbying expenditures, the government is willing to give away efficiency by creating distortions." — 
- p19 Highlight: "this model proposes one micro foundation for the misallocation of resources between firms" — 
- p19 Highlight: "Dispersion in this measure across firms within industries represents misallocation in this economy." — 
- p19 Highlight: "In the final stage, firms choose how much to produce given the policies and the household chooses consumption" — 
- p24 Highlight: "Firms and politicians with the same shape (e.g., F1 and P1) are assumed to be politically connected." — 
- p24 Highlight: "In contrast, the value of lobbying would decrease for F2 when its connected politician leaves its most valuable red committee." — 
- p25 Highlight: "Moreover, we do not see changes in firms' headquarter locations over time in our dataset." — 
- p26 Highlight: "That is, committee membership changes might be endogenous to firm characteristics and influence as politicians may select into certain committees in order to deliver targeted benefits to their politically connected firms" — 
- p27 Highlight: "Taking column 4 as our preferred estimate, it shows that an increase in 1 percent of lobbying expenditures translate into 0.2 percent increase in revenues." — 
- p32 Highlight: "., firms choose endogenously not to lobby. In that case we find that aggregate productivity would be 22 percent higher than the one where firms obtain the return to lobbying that we estimate from the data" — 
- p34 Highlight: "By estimating the model with micro data, we show that firms' lobbying activity decreases aggregate productivity by 22 percent relative to an economy without lobbying activity." — 
- p34 Highlight: "through lobbying, some firms get bigger than what they should" — 
- p39 Highlight: "Appendix A Construction of Lobbying Dataset" — 
- p39 Highlight: "built from public reports from the SOPR" — 
- p39 Highlight: "When total amount is less than $5,000, registrants should still file a report and include a statement indicating the fact" — 
- p40 Highlight: "Although some of these can be easily addressed by removing dot and suffix, in many cases it is not straightforward to distinguish misspelled client names and abbreviations from their legal firm names." — 
- p40 Highlight: "FuzzyWuzzy string matching algorithm comparing the full list of public firm names from Compustat against 61,478 unique client names." — 
- p40 Highlight: "Bureau van Dijk server's Batch Search functionality to find each firm's ISIN and ticker symbol, which will then be used to find Compustat identifier code of clients." — 
- p40 Highlight: "Center for Responsive Politics lobbying data to check whether any additional matching can be achieved by using their Standardized client variable." — 
- p40 Highlight: "randomly sample 5% of client names to verify" — 

## Study

**Question:** Does firms' lobbying causally increase firm size, and how much does lobbying reduce aggregate productivity through misallocation of resources across firms (distortion of firm size)? (Abstract; Introduction, printed pp.1-3)

**Theory:** Heterogeneous-firm general-equilibrium model with endogenous lobbying, extending Hsieh and Klenow (2009) along Melitz (2003) lines. Firms differ in productivity, lobbying productivity and an exogenous wedge; a fixed lobbying cost induces selection into lobbying; lobbying yields output distortions tau_s(phi) that raise a lobbying firm's revenue (eq. 1, p.14). Microfoundation (Section 3.2, pp.18-20): a three-stage game in which the government's welfare combines household utility with a CES aggregate of lobbying expenditure (a generalization of Grossman and Helpman 1994). Stated rationale for heterogeneous distortions: the government's "love for variety" in lobbying reduces political risk from concentrated influence (p.20). Proposition 3 (p.20): distortions rise with the mass of lobbying firms and fall with the lobbying elasticity sigma_L.

**Data:** Lobbying: parsed LD-1 and LD-2 Lobbying Disclosure Act reports from the Senate Office of Public Records, 1999-2015 filings (Appendix A, printed p.38); 972,005 report-level observations after dropping amendments. More than one million original filings were parsed (p.4). Client names linked to Compustat gvkeys: 67,842 unique client names (p.5). Bills and committees: 65,047 lobbied bills across the 106th-115th Congresses (Figure 5; the 1,862 trade bills are a subset); committee assignments from Stewart and Woon (2011). Firm sample: all US publicly traded firms in Compustat, 1999-2018 (p.4-5). Unit: firm-year (Table 2 regressions). Regression N for log sales: 15,332 (all years) and 8,796 (post-2007); profits: 10,369 and 6,064; value added (Table F.2): 10,851 and 6,097. Industry data: BEA value-added, labor, and intermediate input shares for 2000-2017 (Figures 8-11). Campaign contributions: FEC data (Figure 1). Financial variables are described only as standard characteristics of public firms (the source database is not named in the extracted text).

**Method:** Descriptive stylized facts (Section 2); OLS and IV panel regressions of log sales and log profits on log lobbying expenditure with firm, year and state-year fixed effects, standard errors double-clustered by firm and year (Table 2, Appendix F); structural model estimated by simulated method of moments (nine parameters: production and lobbying fixed costs, variances and covariances of productivity and wedge, and beta), with sigma_C=4, death rate 0.025 and beta_s=beta imposed (pp.27-31); counterfactual with beta=0 and a sensitivity sweep over sigma_C (pp.31-32). The SMM text says the weighting matrix is the generalized inverse of the moment covariance, but for now it uses the identity matrix (fn.22, p.29).

**Identification:** Shift-share instrument (eq. 12, p.21): z_it = sum over politicians j in firm i's connections of [sum over committees c of w_ict-k * d_jct]. Share w_ict-k = firm i's share of lobbied bills assigned to committee c, lagged (preferred t-1; t-2 and t-3 in Tables F.1-F.2). Shift d_jct = indicator that politician j sits on committee c in year t. Connections = politicians representing the state of the firm's headquarters. Identifying assumption: committee moves by politicians are exogenous to firm characteristics and influence (p.22). Relevance: 30 percent average probability a politician changes committee between Congresses (p.22; about 29 percent in Figure C.2).

**Scope:** Population: US publicly traded firms in Compustat (1999-2018) and lobbying clients of federal lobbying disclosure filings. Institution: US federal lobbying under the Lobbying Disclosure Act of 1995 and congressional standing committees (Table C.2; Figure 6 and C.1 show senators, 106th-115th Congresses). Era: 1999-2018. Model economy: multi-industry US economy with 17 industries (Figures 8-11).

## Key findings

- Lobbying is rare: only 766 of 7,646 public US firms lobbied in 2017; average share of public firms lobbying across years is 11.8 percent (text figure; 766/7,646 is about 10.0 percent for 2017 alone). (Section 2.1, Fact 1, printed p.6; Table 1) — 766 of 7,646 firms (2017); 11.8 percent average; manufacturing 15.8 percent lobby, 6.5 percent in-house
- Lobbying spending exceeds campaign contributions: on average lobbying is more than six times larger than campaign contributions (PAC, party, candidate and other federal committee contributions; individual contributions excluded). (Fact 2, Figure 1, printed p.8) — more than 6x on average (2 year cycles, 1999-2016)
- Firm size and lobbying are positively correlated on the intensive margin (conditional on lobbying). (Fact 3, Figure 2, printed p.9) — log lobbying coefficient on log sales = 0.984 (p=0.000, t=28.13)
- Lobbying is highly persistent. More than 80 percent of firms that lobbied in 2016 lobbied in 2017; the intensive-margin correlation of log lobbying across adjacent years is high, though persistence dropped during 2007-2009. (Fact 4, Figure 3, printed pp.9-10) — >80 percent (2016 to 2017); intensive margin coefficient 0.922 (t=157.88)
- Firm-level lobbying exceeds lobbying by business and industry organizations (figure only; no dollar totals stated in the text). (Fact 5, Figure 4, printed p.11) — none stated in text
- Most congressional bills are lobbied by only one or two interest groups (narrow, firm-specific lobbying). (Fact 6, Figure 5, Table C.1, printed pp.11-12) — median 2 clients per bill; mean 3.45 clients across 65,047 bills; trade bills: 1,862 bills, mean 3.21, median 2
- Headline causal result: a 10 percent increase in lobbying expenditure raises revenue by about 3 percent (IV). The text also reports IV estimates are an order of magnitude above OLS; Table 2 shows IV is about 4.5 to 7.3 times OLS. (Abstract; Introduction printed p.2; Table 2 printed p.26) — Table 2 log sales: OLS 0.0436 (all) and 0.0500 (post-2007); IV 0.319 (all, SE 0.0754, first-stage F 14.30) and 0.227 (post-2007, SE 0.0795, F 13.10)
- Internal inconsistency: the 3 percent headline is arithmetically consistent with the full-sample IV coefficient (0.319), though the paper does not name the column (inference); the text calls post-2007 column 4 (0.227, implying about 2.3 percent per 10 percent) the preferred estimate and says 1 percent lobbying gives 0.2 percent revenue. (Printed p.26 (text after Table 2)) — 0.319 vs 0.227; 0.2 percent per 1 percent
- Profit results mirror sales: IV profit coefficients are positive and larger than OLS, though only the post-2007 IV is significant at the 10 percent level. (Table 2 cols 5-8, printed p.26) — OLS profits 0.0241 (all), 0.0468 (post-2007); IV profits 0.322 (all, SE 0.114), 0.236 (post-2007, SE 0.124)
- Robustness to lagged weights and value added: IV sales coefficients stay positive with t-2 and t-3 weights, but first-stage F falls (t-3 all-sample F = 6.0). Value-added IV coefficients are 0.287 (all) and 0.177 (post-2007) with t-1 weights. (Appendix F, Tables F.1-F.2, printed pp.10-12) — t-2 IV sales 0.378 (F 9.30) and 0.304 (F 9.90); t-3 IV sales 0.393 (F 6.00) and 0.381 (F 6.90); VA t-1 IV 0.287 and 0.177
- Calibrated model: the estimated model reproduces the targeted moments approximately. Targeted IV 1-beta is 0.23 in the data and 0.19 in the model (implying beta = 0.81); OLS 1-beta is 0.05 in the data and 0.045 in the model. Other targeted moments are listed in Table 3. (Table 3, printed p.31; Figure 12 caption) — data/model: v(phi_P) 2.8/2.4; v(phi_L) 2.1/1.8; v(phi_tau) 1.1/1.6; corr(phi_P,phi_L) 0.5/0.3; corr(phi_P,phi_tau) -0.5/-0.1; corr(phi_L,phi_tau) 0.2/0.4; 1-beta IV 0.23/0.19; 1-beta OLS 0.05/0.045
- Counterfactual with no return to lobbying (beta=0): aggregate productivity would be 22 percent higher than under the estimated return. About 41 percent of that loss comes through the entry channel (lower wages make entry cheaper); the rest comes from misallocation among incumbents. (Counterfactual, printed pp.31-32; Conclusion printed p.33) — 22 percent productivity loss; 41 percent via entry
- Sensitivity to the elasticity of substitution sigma_C (set to 4): the loss is about 38 percent at sigma_C=2 and about 13 percent at sigma_C=6. (Figure 12 right panel, printed p.32) — 38 percent (sigma_C=2); 22 percent (sigma_C=4, benchmark); 13 percent (sigma_C=6)

## Limitations

- Identification depends on committee-membership moves being exogenous to firms. The authors concede politicians may choose committees to deliver targeted benefits to connected firms (printed p.25) and that committee weights could reflect anticipated changes (tested with a near-zero correlation, p.25).
- Headquarters are assumed fixed; the authors report no headquarter changes in their data, and a politician changing district has under 1 percent probability (p.24-25).
- The instrument's relevance is modest: first-stage F is 13-14 in the preferred specifications and falls to 6-10 with t-2 or t-3 weights (Tables 2, F.1, F.2).
- Headline 3 percent versus preferred-column 0.2 percent inconsistency (see key findings); the text's claim of an order-of-magnitude IV-OLS gap does not match Table 2 (about 4.5 to 7.3 times).
- Elasticity of substitution sigma_C is not estimated; it is fixed at 4 for lack of data, and the paper acknowledges results depend on it (p.27, p.32). Industry-specific beta_s and sigma_C are restricted to be common across industries (p.27).
- Model is simplified: one industry and one factor for exposition (fn.14); demand shifters ignored (fn.9); extensions to decreasing returns and demand shifters deferred to future versions (fn.10); Hsieh-Klenow caveats (Haltiwanger et al. 2018) noted but not addressed; the counterfactual uses aggregate productivity, not output, because there is no capital or human capital accumulation (fn.23). The analysis is partial equilibrium except for the Section 4.2 general-equilibrium counterfactual (fn.13).
- Lobbying-to-distortion mapping is assumed in Section 3.1 and microfounded only through a government welfare function whose lobbying-income motive is not justified (p.19-20, fn.15). Equation (10) assumes truth-telling, and coordination issues are set aside as beyond scope (p.19).
- SMM uses an identity weighting matrix for the baseline, despite the stated generalized-inverse weighting (fn.22, p.29).
- Lobbying amounts are self-reported estimates rounded to the nearest $5,000 (pre-2008 larger rounding bins; Appendix A, fn.25). Client-name matching is algorithmic plus manual, checked on a 5 percent random sample for missed publicly traded firms, a recall check rather than a precision audit (Appendix A, fn.27-28, printed p.38-39).
- Date inconsistency: the data are described as covering 1999-2018 (Introduction), but Appendix A says LD-1 and LD-2 filings from 1999-2015 were used.
- The persistence measure is conservative (two adjacent years only), and the persistence drops during the 2007-2009 crisis (p.10).
- Open questions the authors leave: why entry into lobbying is limited, why persistence is high, the reverse effect of size on lobbying, and business-cycle effects (p.33).

## Short quotes

- "We find that a 10 percent increase in lobbying expenditure leads to a 3 percent gain in revenue." (Abstract, printed p.1 (also Introduction p.2))
- "firms' lobbying activity decreases aggregate productivity by 22 percent relative to an economy without lobbying activity." (Conclusion, printed p.33)
- "Taking column 4 as our preferred estimate, it shows that an increase in 1 percent of lobbying expenditures translate into 0.2 percent increase in revenues." (Section 4.1, printed p.26)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | qualifies | Lobbying raises firm revenue. Table 2 (printed p.26): IV log-sales elasticity 0.319 for all years (SE 0.0754, first-stage F 14.30) and 0.227 for post-2007 (first-stage F 13.10). OLS 0.0436 (all years) and 0.0500 (post-2007). Firm, year and state-year fixed effects. N = 15,332 (all) and 8,796 (post-2007). The paper names column 4 (IV post-2007, 0.227) as its preferred estimate. The reverse effect of size on lobbying is listed as an open question in the Conclusion (printed p.33). (Table 2 printed p.26; Section 4.1 printed p.26; Conclusion printed p.33) | The L-LV family regresses sector LobbyView lobbying in real 2025 USD millions (dv lobby_lv_real_m, the de-duplicated primary column) on lagged sector growth. The log forms (R-log) are separate robustness fits, so the base model is in levels, not logs. The R-yearFE variant swaps national controls for sector and year fixed effects. The recorded result that the lobbying lead flips sign under year FE was not found in the sector_growth_study markdown files checked here. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R lines 640-667 and sgs_config.R lines 78-127 (checked: DV labels, LobbyView dedup column and limitations text match)` | Design warning only. If lobbying feeds revenue, a growth-to-lobbying regression is simultaneous, so OLS with fixed effects cannot sign the lead. The paper's IV is 4.5 to 7.3 times its OLS (0.227/0.0500 = 4.5; 0.319/0.0436 = 7.3), not 4.5 to 7. The paper uses Compustat public firms with federal LDA filings 1999-2015 and no trade associations, while the sector panel includes associations, so the effect size does not transfer. The paper does not say how it handles self-filer and outside-firm double counting; the text mentions only that about 85 percent of lobbying is outsourced. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-giving-forecast#Refit gates G0-G9\|Refit gates G0-G9]] | context | Firm lobbying is persistent. Fact 4 (printed p.10): more than 80 percent of firms that lobbied in 2016 also lobbied in 2017. Figure 3 (printed p.10): adjacent-year intensive-margin log-lobbying slope 0.922 (t = 157.88, conditional on lobbying in both years). The Figure 3 caption says over 70 percent of firms lobby in two consecutive years. Persistence dropped during the 2007-2009 financial crisis. The paper does not address forecasting. (Fact 4 printed p.10; Figure 3 and caption printed p.10) | The lobbying console rule is last year's level plus slope times the change in growth, backtested on rolling origins with 2014-2023 targets. Gate G9 warns when a refit does not beat naive. The README records the log-lobbying MAE as 4.050 against naive 3.936, so the refit does not beat naive (warning only). `C:/Users/pgoff/Mentat/mentat/forecast/refit/R/rf_backtest.R lines 2-7, 68 (console rule), 112; C:/Users/pgoff/Mentat/mentat/forecast/refit/R/rf_gates.R line 14 (G9 warning); C:/Users/pgoff/Mentat/mentat/forecast/refit/README.md line 142 (MAE 4.050 vs 3.936)` | The paper's persistence is firm-level on public firms in a 1999-2018 sample; the forecast works on sector aggregates. The claim that naive is a strong baseline is the analyst's inference from persistence, not the paper's. The 2007-2009 break lies outside the 2014-2023 target window but may fall in training data for later origins; the backtest start year was not checked, so 'regime shifts are not in the backtest' is not established. Persistence says nothing about whether growth adds signal. | moderate | VERIFIED-SOFTENED |
| [[systems/moneyball-rank-pillars#Committee - Influence pillar\|Committee - Influence pillar]] | suggests-extension | Committee relevance is measured from data. Weight w_ict-k is the firm's lagged share of lobbied bills referred to committee c, with t-1 preferred and t-2 and t-3 in robustness (Appendix F). Interacted with committee seats held by politicians from the firm's headquarters state, the first-stage F is 14.30 (all years) and 13.10 (post-2007) in Table 2. Deeper-lag panels in Table F.1 give F of 6.0 to 9.9. About 30 percent of politicians change committee between Congresses (Section 4.1; Figure 6). (Instrument definition in Section 4.1 (printed pp.21-22; eq. 12 page not re-checked); Figure 6; Table 2 printed p.26; Tables F.1-F.2 Appendix F) | Seat relevance is a tier map: Highly 3.0, Moderately 1.8, Slightly 0.9. Subcommittee seats count 1/6. Leadership multipliers: Chair 3.0 full and 7.5 sub, Ranking 2.0 and 4.5, Vice 1.0 and 2.0. No weight is derived from where CU bills are referred. The owner-judgment provenance of the tiers is not visible in the code. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/engine/committee_scores.py lines 120-132 (BASE tiers, SUBF, KLEAD verified)` | Extension as stated: compute each committee's lagged share of CU-category bills referred, from congress.gov billstatus rather than LDA lobbied-bill lists (D-021). H&K's weights are firm-specific and validated only as an instrument for firm revenue, with no evidence that referral shares predict a member's CU effectiveness, so any re-weighting needs its own look. | moderate | VERIFIED |
| [[systems/melange-lobbying#LDA finance-client classification and report join\|LDA finance-client classification and report join]] | method-source | LDA client names were matched to Compustat in four steps: FuzzyWuzzy against 61,478 unique client names; Bureau van Dijk batch search for ISIN and ticker; CRP standardized client names; and a random 5 percent sample of client names checked for publicly traded firms that were missed. The 5 percent sample is a miss (recall) check, not a precision audit. The paper names messy spellings and abbreviations as the main hazard. The matched universe is 67,842 unique lobbying client names (Introduction). (Appendix A printed p.39 (footnotes 27-28); Introduction text on 67,842 names) | classify_client assigns Bank, CU or other. Credit-union names are checked first and are decisive, then trade-association names, then bank names with exclusions. Only when no name rule fires does the function fall back to a NAICS-based side. So NAICS does drive side assignment as a fallback, not only linkage metadata. No random-sample accuracy audit of the classification is recorded. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/lobbying/collect_lobbyview.py lines 176-183 (regexes, CREDIT_UNION_RE, CU_TRADE_RE) and 226-262 (classify_client; NAICS fallback at lines 258-262)` | The transferable piece is the 5 percent sample as a recall audit for missed names. The fuzzy matcher is not transferable, and the project's strict-names rule rejects fuzzy acceptance. H&K's firm match leaves out most CUs (not in Compustat, per the analyst; not checked against the paper) and trade-association lobbying such as CUNA and NAFCU, so their match rates and Fact 5 do not carry to CU lobbying. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | qualifies | The return to lobbying is measured as firm revenue (IV elasticity 0.227 to 0.319), not as member votes or positions (Table 2, printed p.26). Lobbying spend is more than six times PAC, party and candidate contributions on average (Fact 2 and Figure 1, printed pp.8-9; FEC two-year cycles 1999-2016; individual contributions excluded). (Table 2 printed p.26; Fact 2 and Figure 1 printed pp.8-9) | pflip combines saturating money terms for CU and industry dollars, from congestion.json, with industry = banks plus finserv money, plus peer proximity, at WM 0.35 and WP 0.65. No lobbying spend enters the flip model. `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/engine/flip_model.py docstring lines 1-22 (WM and WP constants) and line 46 (ind0 = banks plus finserv)` | The paper has no member-level outcome and cannot calibrate WM, HALFF or the saturation. Its only relevance is the scale fact that lobbying is the largest corporate political channel and is absent from the money terms. Under D-021 that absence is deliberate, so this is not a recommendation to add LDA dollars. The claim that Paul's positions cite H&K under the flip model is not supported by this item (paul_entries holds no takeaways or links), so it was not verified. | weak | VERIFIED |
| [[systems/melange-demographic-potential#Demographic Potential v3.0 (CU footprint, default)\|Demographic Potential v3.0 (CU footprint, default)]] | qualifies | Firm-politician connection is defined as politicians representing the firm's headquarters state. The paper reports no headquarters moves in its data, and the likelihood that a politician changes district is less than 1 percent. The instrument built on the home-state link has first-stage F 14.30 and 13.10 (Table 2). (Section 4.1 printed pp.22-25 (HQ-state definition; no HQ moves and under-1-percent district change on printed p.25); Table 2 printed p.26) | Footprint v3.0 (the default) scores districts by employment LQ 0.30, establishment LQ 0.20, CU branch share 0.25 and CU deposit share 0.25. A district qualifies at unrounded employment LQ of at least 1.0. It does not feed Demo percent (not checked). `C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/district/demographic_potential.py lines 26-37 (default mode, FOOTPRINT_EMPLOYMENT_LQ_MIN = 1.0) and lines 158-165 (weights)` | In H&K the home-state link is an identifying assumption for headquarters only, not an estimated effect of employment or branch footprint, and it is at state level, not district level. It gives weak support to the state-layer idea. It does not validate the footprint weights or any claim that footprint predicts CU-aligned behaviour. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary and gaps say the 5 percent random sample verifies classifier precision. The paper uses it to find publicly traded firms that were missed (recall), so the template is a recall audit.
> - Summary issue: Summary says NAICS is linkage metadata only and that the rules fail closed. classify_client falls back to a NAICS-based side when no name rule fires (collect_lobbyview.py lines 258-262).
> - Summary issue: Flags say the authors concede committee exogeneity may fail at p.26. The concession is on printed p.25 (annotation page 26).
> - Summary issue: Transfer text says the IV is 4.5 to 7 times OLS. The exact ratios are 4.5 (0.227/0.0500) and 7.3 (0.319/0.0436).
> - Summary issue: Summary says the 3 percent headline uses the full-sample estimate. That is arithmetically consistent (0.319 x 10) but the paper does not name the column, so present it as inference.
> - Summary issue: Summary describes the sector-growth base model as log sector lobbying. The base dv is lobby_lv_real_m in real 2025 USD millions; log forms are R-log robustness variants.
> - Summary issue: Summary says the 2007-2009 break is not in the backtest. It is outside the 2014-2023 targets but may be in training data; the backtest start year was not checked.
> - Summary issue: Summary says the lobbying lead flips sign under year fixed effects. No matching text was found in the sector_growth_study markdown files checked; not verified here.
> - Summary issue: Summary says Paul's positions cite H&K under the flip model, joint optimizer, workbook allocator, break-even, ROI, cycle model and employer match, and that several positions are stale. The item's Paul record (68 highlights, zero notes, zero takeaways) does not show this; not verified.
> - Summary issue: Related note: the bickmore link was reworded because it is an industry-level concentration study, not the same misallocation story.
> - Summary issue: Coverage: ext and rel are both inside the item JSON (no separate ext.json or rel.json exist). All 6 bearings, all 10 related links and the Melange and Mentat code ranges were checked. paul_markup has 68 Highlight items with zero notes and no takeaways; the schema has no key_findings or paul_markup field, so they are not returned. Page images were not used.

## Gaps and flags

- Committee relevance tiers are owner judgment. The system has no data-derived check of where CU-category bills are actually referred, lagged by Congress, which is the committee weight H&K build. Any version must use congress.gov referrals, not LDA (D-021).
- The LDA finance-client classifier has no recorded random-sample audit. H&K's 5% client-name sample checks for missed public firms (recall), so it is a recall template, not a precision audit.
- The sector-growth study is all OLS or fixed effects with no exogenous variation. H&K show lobbying feeding revenue, so the system has no way to separate growth driving lobbying from lobbying driving growth.
- No company-level lobbying panel exists in Mentat linking roster companies' LDA spend to their own performance. All lobbying analysis is sector aggregate.
- Firm versus association lobbying is not split for the CU sector. H&K's firm-dominance fact (Fact 5) is for public firms and probably inverts for CUs, which lobby mainly through associations; this has not been measured.
- Headline inconsistency: '3 percent per 10 percent' is consistent with the full-sample IV (0.319; the paper does not name the column, so this is inference), but the authors' stated preferred column (post-2007, 0.227) gives about 0.2 percent per 1 percent. Paul highlighted both. Any quote must name the window (all years versus post-2007, Compustat public firms, federal LDA 1999-2015 filings) and the column.
- The 22% productivity loss depends on the model. It comes from a fixed sigma_C=4 (13-38% across sigma_C 2-6), SMM with an identity weighting matrix, and an industry-common beta. Never quote it as a measured quantity.
- The Table 2 coefficients are pooled across all industries, with beta_s restricted to be equal across industries. Quoting them for finance or CU lobbying requires an UNSTRATIFIED label.
- D-021: neither the committee-weight extension nor the flip-model scale gap may be implemented with LDA as an edge or weight source.
- The paper has no member-level or allocation evidence, so it does not support member-flip pricing or allocation. Paul called the Alexander item (a single-policy lobbying ROI estimate) an outlier (Trey bibliography).
- The instrument rests on committee assignment being exogenous to firms, and the authors concede that it may not be (p.25). Treat the causal claim as moderate (F 13-14, and 6-10 with deeper lags).

Paul's annotation record for this work is empty: no takeaways and no system links. His highlights focus on the identification strategy (headquarters-state connections and committee moves), the 3% and 22% headline numbers, and the Appendix A client-name matching. The evidence matches his committee-pillar reading (lobbying value runs through committee position) and gives weak support to his state-layer and footprint idea. It does not support using H&K to set fundraising goals, price member flips or justify ROI. The paper measures firm revenue returns to lobbying, not legislator responses to PAC dollars. The flip model deliberately excludes LDA dollars (D-021). The highlight inconsistency between 3% and 0.2% is the paper's own, and Paul highlighted both without noting the conflict.

## Related papers

- [[kim-2018-lobbyview-firm-level]] — same-data — LobbyView is the firm-name-disambiguated LDA and bill database that this paper builds on and releases (confirmed from the item title). Ingestion by collect_lobbyview.py was not checked.
- [[kim-2020-strategic-sequential-links]] — same-data — Same LobbyView firm linkage, joined to FEC donations, bearing on the lobbying-to-contribution relation in H&K Fact 2. Checked only by title and authors.
- [[kim-2020-mapping-political-communities]] — same-data — Uses bill-level LobbyView data of the kind H&K use for committee shares and clients per bill (median 2). Checked only by title and authors.
- [[bombardini-2020-empirical-models-lobbying]] — complements — Survey of empirical lobbying models, including returns-to-lobbying designs and the Grossman-Helpman microfoundation that H&K Section 3.2 generalizes.
- [[bickmore-2025-corporate-lobbying-political]] — complements — Links corporate lobbying and PACs to market concentration, a related misallocation question at industry level. The outcome differs from H&K's firm-size distortion.
- [[alexander-2009-lobbying-analysis-under]] — complements — A single-policy lobbying ROI estimate. H&K's economy-wide revenue elasticity is a different target, so compare with care.
- [[imai-2021-fixed-effects-regression]] — complements — Its two-way fixed-effects caution applies to H&K's OLS columns (firm, year and state-year FE) and to the system's sector-growth regressions.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Finds low overlap between organizational lobbying and PAC spending, which qualifies any use of H&K's lobbying-to-contribution ratio.
- [[thakur-2025-matching-theory-perspective]] — complements — Models committee assignment as a matching process, bearing on H&K's identifying assumption that committee moves are exogenous to firms.
- [[porter-2005-network-analysis-committees]] — complements — Network analysis of House committees, relevant to H&K's committee-share weights. Checked only by title and authors.

### Linked from
- [[kim-2025-measuring-interest-group]] — complements — Both relate firm size to lobbying, and In Song Kim co-authored both. Huneeus and Kim study lobbying and resource misallocation. Kim et al. find larger firms lobby more and take Monitor positions more often.
- [[kim-2023-systemic-sequential-links]] — cited-by — The paper cites Huneeus and Kim (2019) on firm-level returns to lobbying at pp.28-29 (text lines 1311 and 79465). The slug year 2018 does not match the cited 2019 publication; the link is real but the year label should be corrected.
- [[alexander-2009-lobbying-analysis-under]] — complements — Item exists. Firm lobbying returns or rents theme is plausible; content not checked beyond existence.
- [[anon-nd-house-committee-requests]] — complements — Paul cites Huneeus & Kim's committee-assignment instrument. Assignment requests bear on whether assignment is plausibly exogenous to member interest.
- [[defigueiredo-2013-advancing-empirical-research]] — complements — Huneeus and Kim (2018 draft) on firms' lobbying and resource misallocation. The committee-assignment instrument described in the rel was not verified against the item.
- [[kerr-2014-dynamics-firm-lobbying]] — complements — Corrected from builds-on. Kerr does not cite Huneeus and Kim. Shared fixed-cost and selection theme is an analyst judgment, not a documented lineage.
- [[note-request-for-review-industry-economic-performance]] — complements — Estimates firm-level returns to lobbying (abstract and counterfactuals). Relevant to the deep-pockets reading; the link to H1 is interpretive.
- [[note-industry-growth-and-political-spending-research]] — complements — Item record exists (Huneeus and Kim 2018, effects of firms' lobbying on resource misallocation). Title checked only; the rel's link to deep-pockets and seek-relief hypotheses was not re-read.