---
type: paper
title: "Measuring Interest Group Positions on Legislation: An AI-Driven Analysis of Lobbying Reports"
authors: ["Jiseon Kim", "Dongkwan Kim", "Joohye Jeong", "Alice Oh", "In Song Kim"]
year: "2025"
venue: "arXiv preprint arXiv:2504.15333v1 [cs.CY] (21 Apr 2025); manuscript formatted for PNAS and marked DRAFT. Metadata hint confirmed; author list corrected to full names."
doi: "\"\""
category: "Lobbying"
systems: ["melange-labels-and-tiers", "melange-lobbying", "melange-stance", "melange-validation"]
components: ["melange-validation.gold-set-and-rank-gate", "melange-labels-and-tiers.industry-boards-crown", "melange-lobbying.contested-bills", "melange-lobbying.lobbied-member-link", "melange-stance.bill-sign-handling", "melange-lobbying.finance-client-classification", "melange-stance.debate-nli-classifier"]
paul_annotated: true
paul_markup_count: 128
evidence_status: "verified"
source: "[[_sources/kim-2025-measuring-interest-group.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\Measuring_Interest_Group_Positions_on_Legislation_.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\Measuring_Interest_Group_Positions_on_Legislation_.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\Measuring_Interest_Group_Positions_on_Legislation_.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\Measuring_Interest_Group_Positions_on_Legislation_.pdf"]
sha12: ["ef8b6c693fc3", "a60235faab0f", "f01fffae842f"]
tags: [paper, lobbying, type/paper, theme/lobbying, evidence/verified]
---

# Measuring Interest Group Positions on Legislation: An AI-Driven Analysis of Lobbying Reports

**Jiseon Kim, Dongkwan Kim, Joohye Jeong, Alice Oh, In Song Kim (2025).** *arXiv preprint arXiv:2504.15333v1 [cs.CY] (21 Apr 2025); manuscript formatted for PNAS and marked DRAFT. Metadata hint confirmed; author list corrected to full names..* [""](https://doi.org/""). Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Kim, Kim, Jeong, Oh and In Song Kim (arXiv 2025, PNAS-format draft) label which way interest groups lean on individual bills, using the free-text bill lines in LDA quarterly reports from 2009 to 2022 (111th-117th Congresses). The pipeline has two stages. GPT-4 at temperature 0 labels keyword-filtered lines, producing 82,421 positions. A heterogeneous GraphSAGE model then fills in where the text is unclear, producing 196,683 more, about 70.5% of the total. The final dataset has 279,104 group-by-bill positions covering 12,032 groups and 42,475 bills, labelled Support, Oppose, or Engage (Engage merges Amend and Monitor). The LLM agrees with human coders 96.9% of the time, but only on the 391 items where both coders agreed. It matches MapLight on 91% of 1,182 overlapping pairs. The GNN reaches 78.5% accuracy, and Oppose is its weakest class (F1 64). On the substance:
> - Support rises toward enactment.
> - Oppose and Amend rise late in bills that go on to fail. The vetoed-bill figure uses only 3 bills (SI Table 13); the SI reports the same rise in Oppose and Amend in 74 provisionally killed bills.
> - Comparing firms at the 90th and 10th percentiles of size, larger firms are 19.5 pp more likely to lobby and 4.5 pp more likely to Monitor.
> - A one-dimensional IRT score for groups (LPscore) correlates only weakly with DIME CFscores (r = 0.19, n = 218).
> 
> This matters here because the system has already taken in this dataset and copied its method. It downloaded the Dataverse release, rebuilt Kim's labelling step with Haiku, and benchmarked it against Kim's published labels at natural base rates. On that benchmark, agreement with Kim's labels over the 876-pair overlap is weak for Support and Oppose (precision 0.42, recall 0.49), and Amend is never recovered (0 of 23). That result led to the standing rule that inferred positions are evidence for retrieval and adjudication, never automatic weights. The paper's real value to Paul is as calibration gold for 2009-2022 and as raw material for per-bill industry conflict, which the system already computes. It is not a feed that can be wired straight into the graph.

## Paul's annotation

From [[annotations/Machine Learning & Sentiment Analysis - Machine Learning & Sentiment Analysis - Annotated Bibliography|Machine Learning & Sentiment Analysis - Machine Learning & Sentiment Analysis - Annotated Bibliography]]:

> Kim, Jison, et al. *Measuring Interest Group Positions on Legislation: An AI-Driven Analysis of Lobbying Reports*, 23 Apr. 2025.
>
> Professors from the Massachusetts Institute of Technology and the Korea Advanced Institute of Science and Technology use Large Language Models and Graph Neural Networks to analyze and underpin the correlation between lobbying firms, the direction and shape of policy preferences, and ultimately derive insights regarding lobbying strategies and firm behavior. This article is foundational to the current research, as it provides a novel, large-scale, and pre-built data pipeline that can be directly implemented on a social network analysis vector graph, yielding insights into the behavior of competing firms. There are three main findings uncovered by the authors: A strong correlation between the bills' place and the legislative process and the positions taken by the interest group; the firm's size plays a key role in the adoption of monitoring positions; and industries align their efforts on issue-specific stakes. Finally, there are two main weaknesses relevant to the current research. First, this method is reactive rather than predictive due to the time lag between the release of filing reports and current industry positions. Finally, this method doesn’t explicitly highlight conflict between industries over a specific bill, which is vital for political intelligence.

From [[annotations/Annotated Bibliographies & Literature Synthesis - Curated Annotations|Annotated Bibliographies & Literature Synthesis - Curated Annotations]]:

> Kim, Jison, et al. Measuring Interest Group Positions on Legislation: An AI-Driven Analysis of Lobbying Reports, 23 Apr. 2025.
>
> Professors from the Massachusetts Institute of Technology and the Korea Advanced Institute of Science and Technology use Large Language Models and Graph Neural Networks to analyze and underpin the correlation between lobbying firms, the direction and shape of policy preferences, and ultimately derive insights regarding lobbying strategies and firm behavior. This article is foundational to the current research, as it provides a novel, large-scale, and pre-built data pipeline that can be directly implemented on a social network analysis vector graph, yielding insights into the behavior of competing firms. There are three main findings uncovered by the authors: A strong correlation between the bills' place and the legislative process and the positions taken by the interest group; the firm's size plays a key role in the adoption of monitoring positions; and industries align their efforts on issue-specific stakes. Finally, there are two main weaknesses relevant to the current research. First, this method is reactive rather than predictive due to the time lag between the release of filing reports and current industry positions. Finally, this method doesn’t explicitly highlight conflict between industries over a specific bill, which is vital for political intelligence.

From [[annotations/Trey - Articles - Report - Annotated Bibliography|Trey - Articles - Report - Annotated Bibliography]]:

> Kim, Jiseon, et al. *Measuring Interest Group Positions on Legislation: An AI-Driven Analysis of Lobbying Reports*. 2025.
>
> Professors from the Massachusetts Institute of Technology and the Korea Advanced Institute of Science and Technology use Large Language Models and Graph Neural Networks to analyze and underpin the correlation between lobbying firms, the direction and shape of policy preferences, and ultimately derive insights regarding lobbying strategies and firm behavior. This article is foundational to the current research, as it provides a novel, large-scale, and pre-built data pipeline that can be directly implemented on a social network analysis vector graph, yielding insights into the behavior of competing firms. There are three main findings uncovered by the authors: A strong correlation between the bills' place and the legislative process and the positions taken by the interest group; the firm's size plays a key role in the adoption of monitoring positions; and industries align their efforts on issue-specific stakes. Finally, there are two main weaknesses relevant to the current research. First, this method is reactive rather than predictive due to the time lag between the release of filing reports and current industry positions. Finally, this method doesn’t explicitly highlight conflict between industries over a specific bill, which is vital for political intelligence.

From [[annotations/Lobbying - Lobbying - Annotated Bibliography|Lobbying - Lobbying - Annotated Bibliography]]:

> Kim, Jiseon, et al. *Measuring Interest Group Positions on Legislation: An AI-Driven Analysis of Lobbying Reports*, 23 Apr. 2025.
>
> This article by professors from the Massachusetts Institute of Technology, spearheaded by

## Paul's markup in the PDF

- p1 Highlight [E1-2, Y]: "Paper's first-large-scale claim and headline scope (42k bills, 279k positions, 12k groups)." — 
- p1 Highlight [E3-6, G]: "Findings list: legislative progression, firm size, bill subject, industry heterogeneity." — 
- p1 Highlight [E7-10, Y]: "Motivation: indirect measures and donation-based scores may miss actual stances; activist datasets cover few bills." — 
- p1 Highlight [A1-2, G]: "Abstract's first-large-scale claim and the GNN method label." — 
- p1 Highlight [A3, Y]: "Industry heterogeneity finding." — 
- p2 Highlight [E11-15, Y]: "Fig. 1 pipeline: LLM extraction, GNN refinement where text is unclear, reduced human annotation." — 
- p2 Highlight [A4-7, Y]: "Fig. 1 caption fragments: LLM stage, GNN stage, refinement step." — 
- p2 Highlight [A8-9, G]: "Scalability and coverage of real-world lobbying types." — 
- p2 Highlight [E16-17, Y]: "Nuanced stances beyond support and oppose; dataset as a foundational resource." — 
- p2 Highlight [A10, Y]: "Non-support activities: amendments, continuous monitoring, incremental adjustments." — 
- p2 Highlight [E18-19, Y]: "Method names (LLMs, GNNs)." — 
- p2 Highlight [E20-21, Y]: "Contribution: lobbying-strategy patterns and latent group interests." — 
- p2 Highlight [A11, Y]: "Network connections among groups, lobbyists and policymakers as a signal of position-taking." — 
- p3 Highlight [E22, G]: "LLM input: bill IDs, titles and raw lobbying descriptions." — 
- p3 Highlight [E23, Y]: "Output totals: 279,104 positions, 12,032 groups, 42,475 bills." — 
- p3 Highlight [E24-25, Y/G]: "Multi-activity descriptions segmented into individual lines." — 
- p3 Highlight [E26-27, Y]: "Filter to bill-ID mentions and position keywords." — 
- p3 Highlight [E28-30, Y]: "Azure OpenAI GPT-4 at temperature 0; prompt carries bill ID and title." — 
- p3 Highlight [E31, Y]: "Human-vs-GPT-4 agreement figures (accuracy, F1)." — 
- p3 Highlight [A12, Y]: "Network step helps when lobbying reports alone are unclear." — 
- p3 Highlight [A13, Y]: "Quarterly lobbying report submissions across the 111th-117th Congresses." — 
- p3 Highlight [A14-17, G/Y]: "Segmenting, keyword filtering, Azure GPT-4 labelling at temperature 0." — 
- p3 Highlight [A18, Y]: "Ambiguous cases labelled separately to avoid misclassification." — 
- p3 Highlight [A19, Y]: "Network insights complement the text analysis." — 
- p3 Highlight [A20, Y]: "Legislative network built from sponsorship and cosponsorship links." — 
- p4 Highlight [E32, Y]: "Four-class version: Amend class much less accurate; motivates merging." — 
- p4 Highlight [A21, Y]: "Decision to merge Amend and Monitor into one Engage class." — 
- p4 Highlight [E33-34, Y]: "MapLight dataset description; weak Pearson link to CFscores." — 
- p4 Highlight [E35, B]: "Data availability: code and Dataverse replication files." — 
- p4 Highlight [A22-25, Y]: "GNN best-configuration accuracy, F1 and per-class F1 values." — 
- p4 Highlight [A26-27, G]: "Proxy labels kept only above class probability 0.9." — 
- p4 Highlight [A28, G]: "One-dimensional latent preference embedding for groups." — 
- p4 Highlight [A29-31, Y]: "Graded IRT on Support and Oppose; Engage excluded; minimum-count filter." — 
- p4 Highlight [A32, G]: "IRT standard errors below the reliability threshold." — 
- p4 Highlight [A33-34, Y]: "DIME CFscores used as the comparison measure." — 
- p4 Highlight [A35-37, Y]: "Pearson correlation value and its interpretation (weak)." — 
- p4 Highlight [A38, G]: "Campaign-based and lobbying-based scores capture different behaviour." — 
- p5 Highlight [E36 and A39, Y]: "Analysis aim two: corporate lobbying and firm size (same passage in both files)." — 
- p5 Highlight [E37, Y]: "Analysis aim four: preference variation across industries." — 
- p5 Highlight [A40, Y]: "Analysis aim three: preferences across bill topics." — 
- p5 Highlight [A41-42, Y]: "Lobbying intensity rising toward enactment; dominant groups shift by stage." — 
- p5 Highlight [E38, Y]: "Overall lobbying frequency rises as bills progress." — 
- p5 Highlight [E39, G]: "Lobbying becomes more focused and aligned near enactment." — 
- p5 Highlight [E40, Y]: "Post-enactment monitoring of the new law." — 
- p5 Highlight [E41, Y]: "Analysis includes both enacted and vetoed bills." — 
- p5 Highlight [E42, Y]: "Support associated with advancing bills; Oppose and Amend with failure." — 
- p5 Highlight [A43, Y]: "Oppose and Amend rise after floor passage in bills that fail." — 
- p5 Highlight [E43-44, G]: "Late-stage opposition, and increased visibility mobilising opponents." — 
- p5 Highlight [A44, Y]: "Media attention rises as enactment nears." — 
- p5 Highlight [A45-46, G/Y]: "Firm size as a cost-based determinant; two decisions: whether to lobby, and which positions." — 
- p6 Highlight [E45, Y]: "Logistic model of the lobbying decision with employee count." — 
- p6 Highlight [A47, Y]: "Fig. 3 caption: firm-year data, year fixed effects, block-bootstrap intervals." — 
- p6 Highlight [A48, P]: "Employee count as the firm-size measure." — 
- p6 Highlight [E46-47, G] and [A49, P]: "Dirichlet regression for proportional position composition." — 
- p6 Highlight [E48, P] and [A50, P]: "Estimand: average difference in predicted probabilities, 90th vs 10th percentile (formula fragment)." — 
- p6 Highlight [E49, Y]: "Single-character text-layer fragment." — 
- p6 Highlight [E50-51, Y] and [A51-52, Y]: "19.5 percentage-point higher lobbying probability at 90th vs 10th percentile." — 
- p6 Highlight [E52, G] and [A53, G]: "Larger firms significantly more likely to adopt Monitor." — 
- p6 Highlight [E53, Y] and [A54, Y]: "Monitoring reflects resource advantages and long-term relationships with policymakers." — 
- p6 Highlight [E54-55, G/Y] and [A55, Y]: "Smaller firms more reactive; may reflect resource disadvantages." — 
- p6 Highlight [E56, Y]: "No statistical difference found for Support positions." — 
- p6 Highlight [E57, Y]: "Robustness with industry FE, fixed assets and COGS controls (SI)." — 
- p6 Highlight [E58, Y]: "Strategic positioning within the policy process, beyond the lobbying decision." — 
- p6 Highlight [E59, Y]: "Support is most common; Oppose and Engage shares vary by subject." — 
- p6 Highlight [E60, G] and [A58, Y]: "Taxation and health bills have high Support ratios." — 
- p6 Highlight [A56-57, Y]: "Dirichlet log-likelihood and observed-proportion definitions (formula fragments)." — 
- p6 Highlight [E61-62, Y] and [A59-60, Y]: "Bills with clearly defined beneficiaries draw little opposition and high support." — 
- p7 Highlight [E63, Y] and [A66, Y]: "Bills affecting many stakeholders attract more lobbying entities." — 
- p7 Highlight [E64, Y]: "Conflicting-interest industries grouped by colour in Fig. 5." — 
- p7 Highlight [E65, Y] and [A67, Y]: "Top of Fig. 5: economic/industrial and conservative-aligned industries." — 
- p7 Highlight [E66, Y]: "Bottom of Fig. 5: social-value, service and progressive industries." — 
- p7 Highlight [E67, G]: "Kernel-density panel description (Fig. 6A)." — 
- p7 Highlight [A61, Y]: "Incentives attract support from varied industries." — 
- p7 Highlight [A62-63, Y]: "Low-Support bills concentrate in public-policy and regulatory topics." — 
- p7 Highlight [A64, Y]: "Environmental bills: environmental groups oppose, commercial groups support." — 
- p7 Highlight [A65, G]: "High-Engage bills concentrate in public-sector legislation." — 
- p7 Highlight [A68-69, Y]: "Example industry names and the industries positioned at the bottom." — 
- p8 Highlight [E68, Y]: "Fig. 6 bill-level distribution description." — 
- p8 Highlight [E69-70, Y]: "Support where group and bill preferences align (upward diagonal)." — 
- p8 Highlight [A70-71, Y]: "Axis definitions: group LPscore and bill latent score." — 
- p8 Highlight [E71, Y]: "Engage concentrated in the centre with no directional trend." — 
- p8 Highlight [E72, Y]: "Republican-sponsored bills sit higher on the latent axis." — 
- p8 Highlight [E73-74, Y]: "Business and conservative groups on the right; welfare and liberal groups on the left." — 
- p8 Highlight [E75-76, Y] and [A72, Y]: "Oppose where preferences diverge (downward diagonal)." — 
- p8 Highlight [A73, Y]: "Broad-impact economic bill draws diverse groups." — 
- p8 Highlight [A74, Y]: "Positions depend on both group and bill." — 
- p8 Highlight [A75, Y]: "Group engaged on the bill tends to support it." — 
- p9 Highlight [A76, Y]: "Figure label fragment (industry name)." — 
- p9 Highlight [E77, G]: "Strong alignment within sectors vs divisions between industries." — 
- p9 Highlight [A77, G]: "Stark division between economic and environmental interests in one bill." — 
- p9 Highlight [A78, Y]: "Niche-sector support for some bills." — 
- p9 Highlight [A79, G]: "Opposing-industry conflicts and broad-stakeholder bills." — 
- p9 Highlight [E78, B]: "Second finding: larger firms monitor; smaller firms use reactive oppose and amend." — 
- p9 FreeText [A80]: "Free-text note placed in the Analysis 4 section, after the sentence on sectoral alignment and industry conflict." — In a system I may need to take into account the pressance of conflicting industries
- p9 Highlight [A81-82, Y]: "Summary of key insights; strategic engagement across stages." — 
- p9 Highlight [A83, Y]: "Single-character text-layer fragment." — 
- p9 Highlight [A84, G]: "Dataset as a comprehensive, structured resource." — 
- p9 Highlight [A85, Y]: "Transparency and understanding of legislative processes." — 
- p9 Highlight [A86-90, P]: "Conclusion: pipeline, progression, firm size, topic and bill-level findings." — 
- p10 Highlight [E79, P]: "Table 3 heading (keyword list)." — 
- p10 Highlight [A91, Y]: "Structured approach allows targeted analysis of group-bill engagement." — 
- p10 Highlight [E80, Y]: "Keywords reflect the primary lobbying stances tracked by groups." — 
- p10 Highlight [E81-82, Y]: "GPT-4 via Azure OpenAI; temperature 0." — 
- p10 Highlight [E83, P]: "Five-label output set." — 
- p10 Highlight [E84-85, Y]: "'Without explanation' instruction; simplest prompt chosen." — 
- p10 Highlight [A92, Y]: "LLM prompt template highlighted." — 
- p10 Highlight [A93-97, Y]: "Example phrases from Table 2 (support and endorse, defeat, advocacy, educated)." — 
- p11 Highlight [E86-87, Y]: "Mention label when the activity does not seek to change the bill; describing an amending bill is not amendment." — 
- p11 Highlight [A98, Y]: "Fig. 7 caption: lobbying and legislative networks." — 
- p11 Highlight [A99, Y]: "Lobbyist-legislator prior-work ties." — 
- p11 Highlight [A100, Y]: "Human-guideline rule: classify under the corresponding category." — 
- p11 Highlight [E88, Y] and [A101, Y]: "Network data used when text is insufficient." — 
- p11 Highlight [E89, Y]: "Legislative network: sponsorship and cosponsorship." — 
- p11 Highlight [A102, Y]: "Registrants and lobbyists tied to groups and bills." — 
- p11 Highlight [A103, Y]: "Heterogeneous graph with multiple entity types." — 
- p11 Highlight [E90-92, Y]: "Related-bill edges and inter-bill context." — 
- p11 Highlight [E93-94, Y]: "One-hot categorical features; tf-idf text features." — 
- p11 Highlight [E95, Y] and [A104, Y]: "Base Graph + sponsor configuration row." — 
- p11 Highlight [E96, Y]: "17 graph configurations constructed." — 
- p12 Highlight [E97, Y]: "Hyperparameter combinations run once; best per graph in Table 11." — 
- p12 Highlight [A105-107, Y]: "Fixed model settings; other hyperparameters searched; best settings run five times." — 
- p12 Highlight [A108, Y]: "More nodes and relations generally raise accuracy and F1." — 
- p13 Highlight [E98, Y]: "Final-status statistics for enacted and vetoed bills." — 
- p13 Highlight [E99, Y] and [A109, Y]: "Provisionally killed bills show more Oppose and Amend." — 
- p13 Highlight [A110, Y]: "Firm-size difference in predicted probabilities, 90th vs 10th percentile." — 
- p13 Highlight [A111, Y]: "Negative firm-size effect on opposing positions significant in all but one specification." — 
- p15 Highlight [A112, Y]: "Party polarisation in sponsor latent scores." — 
- p15 Highlight [A113, Y]: "Bill position distributions across bills and groups." — 

## Study

**Question:** Can the positions that U.S. interest groups take on individual bills (Support, Oppose, or Engage, where Engage = Amend + Monitor) be measured directly from Lobbying Disclosure Act reports across all bills from the 111th to 117th Congresses? The paper then asks how those positions vary by legislative stage, firm size, bill subject, and industry.

**Theory:** No formal model is stated. Framing draws on cited work: lobbying as a legislative subsidy (ref 3); firm size shapes corporate political activity through fixed and variable costs, affecting both whether firms lobby and which positions they take (refs 30, 31); lobbying varies by legislative stage (refs 27, 28; ex post lobbying, ref 29); legislative and lobbying networks predict political activity (refs 14, 21); latent preferences are estimated from positions the way roll-call ideal points are (refs 22, 23); donation-based ideology measures may not reflect expressed policy stances (refs 17, 18).

**Data:** Sample: LDA quarterly lobbying reports filed 2009-2022 (111th-117th Congresses), collected from the Senate Office of Public Records and linked to bill metadata from Congress.gov. About 1.4 million bill-line text pairs before a keyword filter. Final dataset: 279,104 bill positions; 12,032 interest groups; 42,475 bills. Unit: interest group x bill pair. LLM-labelled set: 82,421 positions (6,495 groups; 22,820 bills). GNN-labelled set: 196,683 positions (11,185 groups; 36,824 bills), about 70.5% of all positions. Validation: 391 samples on which two human coders agreed; 1,182 interest group-bill pairs overlap MapLight. GNN training: 22,635 MapLight pairs (18,180 Support, 4,455 Oppose). LPscore sample: 1,796 groups and 2,020 bills. Firm analysis: Compustat firm-year data, 2009-2022, 5,421-7,014 firms per year. CFscore comparison: 218 identically named groups. Bill final-status counts (Table 13, SI p.14): Enacted 715 bills (3,640 groups); Vetoed 3 bills (162 groups); Passed 28 bills (128 groups); Provisionally Killed 74 bills (473 groups).

**Method:** (1) LLM labelling: GPT-4 via Azure OpenAI at temperature 0, zero-shot prompt with bill ID, short and official titles, and lobbying text. Five labels in the LLM stage (Support, Oppose, Amend, Monitor, Mention); Mention covers ambiguous or non-amending activity. (2) GNN refinement: heterogeneous GraphSAGE (SI Table 9: 3 classes, hidden size 90, ELU, dropout 0.2, concat decoder) predicts the interest group-bill link type (Support, Oppose, Engage). Amend and Monitor are merged into Engage because four-class accuracy on Amend was low. Training uses LLM labels plus MapLight labels, with a 7:1:2 split. Seventeen graph configurations are compared (base graph; add sponsors, legislators, registrants, lobbyists, related bills, and combinations); the best is base graph + legislator-lobbyist. Predictions are kept as proxy labels only when class probability exceeds 0.9. (3) LPscores: graded-response IRT (Samejima; R mirt) on Support and Oppose labels, with Engage excluded, after keeping groups with at least 10 bills and bills with at least 10 groups. (4) Analyses: descriptive position shares by legislative stage (Fig 2; stage timing proxied by quarterly report deadline); logistic regression for the decision to lobby and Dirichlet regression for position composition, with predicted-probability differences between firms at the 90th and 10th employment percentiles (Fig 3; year FE; optional NAICS FE, PPENT, COGS; SI Tables 19-20); descriptive position ratios by subject (Fig 4, SI Table 21); LPscore distributions by industry (Fig 5); bill-level latent-score scatter and KDE (Fig 6); Pearson correlation with DIME CFscores (n=218).

**Identification:** No causal identification strategy. Firm-size and stage results are conditional associations. The paper frames them as associations that call for further study. Stage timing relies on an assumption: lobbying in a quarterly report is directed at the bill's most recent legislative stage, with the report deadline used as the timing proxy (p.5).

**Scope:** Population: U.S. federal interest groups that filed LDA reports on named bills (S. and H.R. bills), 2009-2022 (111th-117th Congresses). Firm analysis: Compustat-listed companies only. Institutions: Congress (committee, floor, enactment, veto stages) and the executive via presidential action. Groups that do not file lobbying reports are outside the dataset, which the paper highlights when comparing with MapLight.

## Key findings

- Dataset size: 279,104 bill positions across 12,032 interest groups and 42,475 bills. LLM stage yields 82,421 positions and GNN stage yields 196,683. (279,104 / 12,032 / 42,475; LLM 82,421 (29.5%); GNN 196,683 (70.5%); Table 1, p.3; Results overview p.3)
- LLM annotation agrees with two human coders on clear-cut cases. (accuracy 96.93, F1 97.19 on 391 samples where both coders agreed; Validation, p.3)
- LLM positions match MapLight positions on overlapping pairs. (1,076 of 1,182 overlapping pairs match (91.03%); MapLight labels only Support and Oppose; Validation, p.3)
- Best GNN configuration (base graph + legislator-lobbyist), averaged over five seeds. (accuracy 78.51 +/- 0.035; F1 74.65; per-class F1 Support 79.19, Oppose 64.06, Engage 80.70; 7:1:2 split; proxy-label threshold 0.9 (Youden cutoff about 0.58 noted, not used); Link Type Prediction, p.4; SI Tables 11-12 (SI p.14))
- Adding nodes and relations generally raises GNN accuracy and F1; Oppose is the hardest class. (Base graph accuracy 76.07; best 78.51; Oppose F1 57.65 to 64.06 across the 17 configurations, five of them below 60; SI Appendix 2, Best Parameter Result (SI p.12-14))
- The paper's dataset covers far more groups and bills than MapLight once mapped to LDA lobbying records. Only 13.1% of MapLight pairs match. (MapLight 194,479 pairs; 25,382 match (13.1%), covering 2,195 groups and 5,780 bills; this dataset has 12,032 groups and 42,475 bills; Comparison with MapLight, pp.4-5)
- LPscores are reliable one-dimensional latent positions for groups. (1,796 groups and 2,020 bills; EAP reliability 0.95; standard errors 0.03 to 0.30 vs threshold 0.32; LPscores, p.4)
- LPscores and DIME CFscores are only weakly correlated. (Pearson r = 0.191 on 218 identically named groups; CFscore vs LPscore comparison, p.4)
- Position mix shifts across legislative stages. Support rises at enactment; Monitor rises after enactment; Oppose and Amend rise after floor passage for bills that later fail. (Enacted bills: Support rises at Enacted stage; Vetoed bills: Oppose and Amend rise after passing the floor. Vetoed sample is only 3 bills (SI Table 13); Analysis 1, pp.5 (Fig 2); SI Tables 14-17 (SI p.15))
- Larger firms are much more likely to lobby, and larger firms are more likely to choose Monitor. Smaller firms lean toward Oppose and Amend, though by modest margins. (90th vs 10th percentile of employees: +19.5 pp probability of lobbying; Monitor +4.5 pp; Oppose -1.8 pp; Amend -1.3 pp. Text says no difference for Support.; Analysis 2, p.6 (Fig 3); SI Tables 19-20 (SI p.16))
- Bill subject shapes position mix. Taxation and Health are Support-heavy; Public lands and Environment are Oppose-heavy; Economics and Armed Forces are Engage-heavy. (Taxation Support 0.833 (239 bills); Health Support 0.789 (342); Public lands Oppose 0.354 (51); Environmental protection Oppose 0.280 (108); Economics and public finance Engage 0.421 (162); Armed forces Engage 0.357 (78); Analysis 3, p.6 (Fig 4); SI Table 21 (SI p.16))
- Industry LPscores separate economic and conservative sectors from social-value and progressive sectors. Conflicting pairs sit at opposite ends. (1,414 groups across 40 industries (of 94 OpenSecrets categories); Republican/Conservative, Anti-Abortion, Gun Rights at top; Democratic/Liberal, Pro-Abortion, Gun Control at bottom; Analysis 4, p.7 (Fig 5))
- Bill-level latent scores: Support clusters where group and bill preferences align, Oppose where they diverge; Republican-sponsored bills sit higher on the bill axis. (qualitative pattern; no coefficient reported; Fig 6 (A), p.8; sponsor-party distribution SI Fig 12 (SI p.20))
- Engage positions dominate the GNN-labelled set, so Engage-based findings depend heavily on GNN predictions. (137,023 GNN-labelled Engage positions of 188,639 overall Engage positions (72.6%); Table 1, p.3 (reader calculation))

## Limitations

- Stated by authors: Oppose is harder for the GNN because it is less frequent in the data (p.12, SI Table 12 discussion).
- Stated by authors: the proxy-label threshold is set at 0.9 for reliability, above the Youden cutoff of about 0.58 (p.4 footnote).
- Stated by authors: the stage timing uses the quarterly report deadline as a proxy for lobbying timing, assuming reported efforts target the bill's latest stage (p.5).
- Stated by authors: the firm-size results are described as associations that call for further investigation of firm-level factors (p.6).
- Stated by authors: 13.1% MapLight match rate is read as most MapLight groups not actively lobbying (p.4). This is interpretive.
- Stated by authors: the explanation for late-stage opposition (urgency; media attention) is offered as a suspicion, not tested (p.5).
- Stated by authors: the MapLight comparison used the original dataset before excluding conflicting labels (p.4 footnote), so the 91.03% concordance is on pre-exclusion data.
- Reader check (not stated by authors): the abstract claims coverage of all introduced bills, but the methods say bills 'are limited to those that are formally enacted as law, specifically Senate and House bills' (p.3). The two statements are inconsistent, and the stage tables include non-enacted bills.
- Reader check: the LLM validation sample is the 391 cases where two coders agreed, which selects clear-cut texts and likely overstates accuracy on ambiguous Mention and Amend cases.
- Reader check: MapLight pairs both train the GNN and serve as the external concordance benchmark, and the paper does not say whether the 1,182 concordance pairs were held out from training.
- Reader check: most positions (70.5%) and most Engage positions are GNN predictions, so validation of the LLM alone does not cover most of the data.
- Reader check: SI Table 14 (Enacted) row totals add to 27,767, but the printed grand total is 29,763. SI Table 15 (Vetoed) row totals add to 340, but the printed total is 364. Column sums are not reconciled.
- Reader check: Fig 3 caption says errors come from firm-level block bootstrap, while the SI table notes say clustered standard errors. The inference method is not consistent across the text.
- Reader check: text says no statistical difference for Support positions (p.6), but SI Table 19 shows Support coefficients significant at the 10% level in two of five specifications (SI p.16). Amend is significant at the 10% level in three of five.
- Reader check: Fig 2 caption colours (Support blue, Oppose red, Amend yellow, Monitor green) differ from the body text (green, pink, yellow, purple).
- Reader check: SI Table 4 lists model gpt-4-1106-Preview while the main text names GPT-4 (2023-09-01-preview), which is the API version. Reproducibility depends on the model identifier.
- Reader check: the Fig 4 caption does not say which annotation set (LLM, GNN, or combined) produces the subject ratios.

## Short quotes

- "we decided to merge Amend and Monitor into a single Engage category" (Kim et al., Link Type Prediction, p.4 (main text))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-validation#Gold-set validation and zero-rank-delta gate\|Gold-set validation and zero-rank-delta gate]] | qualifies | LLM labels agree with human coders at 96.93% accuracy and F1 97.19, but only on the 391 samples where two coders agreed. MapLight concordance is 91.03% (1,076 of 1,182 pairs) and covers Support/Oppose only; MapLight labels were also part of the GNN training labels. GNN best configuration: accuracy 78.51, F1 74.65; per-class F1 Support 79.19, Oppose 64.06, Engage 80.70. (Validation p.3 (MapLight footnote); GNN Annotation p.4; SI Table 12) | The system's benchmark of the Kim-format Haiku lane uses the 876 (organisation, bill) pairs where Kim's published dataset overlaps the system's work items, at natural base rates with no stratification, and separates Kim's two annotation lanes (541 gnn pairs, 335 llm pairs). The join module records Support/Oppose recall 0.49 and precision 0.42, and Amend 0 found against Kim's 23. The DEBATE 0.571 adversarial-frame figure comes from audit documents, not these files. `fec/lobbying/lobby_position_benchmark.py:1-40; fec/lobbying/lobby_position_join.py:13-19` | The paper's human validation uses only coder-agreed lines, so its 96.93% does not carry over to ambiguous, Amend or Mention lines. The system benchmark measures agreement with Kim's labels, not accuracy: 541 of its 876 reference pairs are Kim GNN predictions (a stage with 78.51% test accuracy) and 335 are GPT-4 labels. Denominator: the 876-pair overlap (the benchmark file states no year window). UNVERIFIED as accuracy until a human-gold path reproduces it (DE-062). | strong | VERIFIED-SOFTENED |
| [[systems/melange-labels-and-tiers#Per-industry tier boards, industry-axes tensor and crown law\|Per-industry tier boards, industry-axes tensor and crown law]] | supports | Oppose is the GNN's hardest class. The text says Oppose F1 is 'in the 60s', but SI Table 12 shows Oppose F1 from 57.65 to 64.06 across the 17 graph configurations, with five configurations below 60. Engage dominates GNN output: 137,023 of 188,639 Engage-class labels (72.6%) come from the GNN stage. Fig 5 orders industries by mean LPscore, with commercial sectors at one end and social/progressive sectors at the other. (SI Appendix 2 p.12 and Table 12; Table 1 p.3; Analysis 4 p.7 Fig 5) | Kim-format positions are joined onto the ledger with basis tag inferred_<variant>, and the module states that a Support badge is a lead, not a fact. In industry_tiers the adjudicated credit-union cell owns the CU tensor weight, and the older raw position overlay (acu_position) is removed from it. `fec/lobbying/lobby_position_join.py:1-19; fec/lobbying/industry_tiers.py:375-382` | The paper's Oppose weakness belongs to its GNN stage. The system's weak Oppose agreement belongs to its Haiku lane, scored against Kim labels. They point the same way, but they are different readers. The bank-column pilot result (about 70% of Oppose labels wrong, zero rows signed) was not found in the cited code or in a repo search, so it is dropped here as UNVERIFIED. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Contested-bills registry\|Contested-bills registry]] | suggests-extension | Bill-level positions show opposing interest groups on the same bill. Coal-deregulation bills (H.R. 3409, 112th; H.R. 806, 115th) are opposed by environmental groups and supported by commercial entities, while an environmental-protection bill (S. 847, 112th) shows the reverse. Subject Oppose ratios: Public lands and natural resources 0.354 (51 bills), Environmental protection 0.280 (108 bills). The paper does not tabulate inter-industry conflict per bill. (Analysis 3 pp.6-7 (Fig 4, Fig 6, SI Table 21); Analysis 4 p.7 (Fig 5)) | The contested-bills registry is a loader for per-bill counterparty sets that Paul ratifies. No consumer exists today (CONTESTED_V1 is unopened), and the bundle surfaces it for display only. Separately, org_stance_priors recomputes ALIGNED, OPPOSED and UNKNOWN per bill for each directional sector pair from label_3_class. It raises CCCAValidationError if a CCCA anchor bill reads OPPOSED for the bank\|credit_union pair. `fec/lobbying/contested_bills.py:1-15; fec/surfaces/build_dashboard_bundle.py:731-741; fec/lobbying/org_stance_priors.py:783-860` | Kim's data ends at the 117th Congress (2022). It can nominate historical candidate contested bills and calibrate the alliance-conflict recompute. Current bills depend on the weaker Haiku lane. Registry admission stays an owner ruling. An OPPOSED pair from inferred labels is a lead, not a fact. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Lobbying member-year network and circular lobbied-member link\|Lobbying member-year network and circular lobbied-member link]] | qualifies | Positions are measured as interest group x bill. Legislators enter only as context nodes in the GNN graph, through sponsor/cosponsor relations and legislator-lobbyist prior-work relations. The selected configuration is 'Base Graph + legislator-lobbyist'. The paper does not measure a position or contact between a group and a member. (Graph Configuration p.4; SI Appendix 2 (Fig 7, Tables 11-12)) | build_dyad_signs joins signed org-bill rows to sponsor/cosponsor rosters by canonical bill, counting probability >= 0.7 as support and <= 0.3 as oppose. efficiency_layers z-scores signed_non_cu_opposition_count into signed_non_cu_opposition_z, which is one component of contest_index. `fec/lobbying/org_stance_priors.py:631-700; fec/engine/efficiency_layers.py:175-211` | Kim's measure stops at the bill. Pushing it onto members through the bill roster brings back the circular sponsor/cosponsor mapping the system already refuted as influence evidence. The dyad counts describe which bills members sponsored, not whom an org targeted. Kim's legislator-lobbyist relation is a revolving-door edge, so influence language is barred on it. | strong | VERIFIED |
| [[systems/melange-stance#Bill-sign valence (direction table, prose matcher; override rejected)\|Bill-sign valence (direction table, prose matcher; override rejected)]] | suggests-extension | The released dataset gives measured group positions on named S./H.R. bills for the 111th-117th Congresses (2009-2022). It is on Dataverse at doi:10.7910/DVN/D0QWM2. (Dataset section and Table 1 p.3; data availability statement) | A bill's CU direction comes from a 60-row hand table, cfpb_bill_directions.csv (Direction 1 = Reduce, -1 = Expand; 5 Expand rows). `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cfpb_bill_directions.csv:1-61` | Useful as an external audit of the hand-set direction table, but only for 111th-117th Congress bills that CU trade associations named in LD-2 lines. 118th and 119th Congress bills would need the Haiku lane. Use it to adjudicate the table, never to overwrite it. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#LDA finance-client classification and report join\|LDA finance-client classification and report join]] | supports | Firm size is associated with position type. Comparing firms at the 90th and 10th employment percentiles, larger firms are 19.5 p.p. more likely to lobby and 4.5 p.p. more likely to adopt Monitor. Smaller firms are more likely to Oppose (1.8 p.p.) or Amend (1.3 p.p.). These are associations, not causal effects. Sample: Compustat firm-years 2009-2022, 5,421-7,014 firms per year. (Analysis 2 p.6 (Fig 3); SI Tables 18-20) | classify_client assigns side (credit_union, bank or other) from decisive name rules first. When no name rule fires, it falls back to a NAICS-based classification, and a NAICS conflict with a name match is tagged in the rule string. `fec/lobbying/collect_lobbyview.py:226-263` | Kim's sample is Compustat firms and says nothing about trade associations. The claim that the system's 22 pinned fintech/crypto firms hold 56 positions with one directional label was not found and is UNVERIFIED. Treat any reading that associations are the directional filers as a descriptive lead, not a replication. | weak | VERIFIED-SOFTENED |
| [[systems/melange-stance#Fine-tuned Political DEBATE NLI stance reader\|Fine-tuned Political DEBATE NLI stance reader]] | qualifies | GPT-4 zero-shot labelling at temperature 0 reaches 96.93% accuracy on 391 coder-agreed lobbying lines. The lines pass a keyword filter for position terms first (e.g. endorse, defeat, advocate, watch). (Validation p.3; SI Table 3 (keywords), Table 4 (prompt)) | stage5_debate.py runs Political DEBATE zero-shot NLI to score each press release's stance toward credit unions (support, oppose and neutral hypotheses). Audit documents (not the code) report the fine-tuned reader at 0.839 accuracy on a held-out human-gold test (n=87) and 0.571 on the adversarial frame. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_debate.py:1-49; AUDIT_FINDINGS_2026-07-02_RUN2.md:118; AUDIT_FINDINGS_2026-07-04.md:452` | Does not transfer. Kim's text is pre-filtered to explicit position terms about a named bill, and the validation set excludes coder disagreements. Press stance toward CUs needs a target-valence step that Kim's task lacks. The 96.9% is not a benchmark for this reader. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: 'That finding rests on 3 vetoed bills' is incomplete. Fig 2 does use only 3 vetoed bills (SI Table 13), but the SI reports the same rise in Oppose and Amend in 74 'Provisionally Killed' bills. Quote the vetoed pattern with n=3 and add the provisionally-killed replication.
> - Summary issue: 'Oppose is its weakest class (F1 64)' is right for the selected model. The bearing gloss that Oppose F1 is 'in the 60s across all 17 configurations' is wrong: five configurations score 57.65-59.08 (SI Table 12).
> - Summary issue: The system-benchmark figures (precision 0.42, recall 0.49, Amend 0 of 23) measure agreement with Kim labels over the 876-pair overlap, and 541 of those pairs are Kim GNN predictions. The summary should say 'agreement with Kim', not imply accuracy, and should name the 876-pair denominator. The flags' '2021-2022' window was not found in the benchmark code.
> - Summary issue: The Bonica relation was mislabelled 'contradicts'. r = 0.191 (n = 218) shows that the measures differ, not that one refutes the other.
> - Summary issue: System figures not found in the cited code or a repo search were removed as UNVERIFIED: the bank-column pilot's '~70% of Oppose labels wrong', CUNA 174/12 and NAFCU 168/23, 22 fintech firms with 56 positions, and the member-prior override 0.839 to 0.770.
> - Summary issue: Dropped the related link to moreno-2021-shadowing-shielding-effective. It is an influence-maximisation paper tied to the system's registry, not to this paper.
> - Summary issue: All of this agent analysis stays UNVERIFIED until a second path reproduces it (DE-062).

## Gaps and flags

- There is no human-gold check of the Kim-format lane. All system benchmarks score agreement with Kim's labels, and 541 of the 876 benchmark pairs are Kim GNN predictions, a stage with 78.51% test accuracy.
- Kim positions end at the 117th Congress. Current-Congress coverage depends on the Haiku lane, whose Oppose and Support recall is about 0.49, so per-bill conflict for live bills is weakly measured.
- Paul's 'reactive, not predictive' critique is unaddressed. Positions arrive after the quarterly deadline, so they cannot be features in the cosponsor forecaster without leakage controls. The forecaster currently uses no position features.
- The positions wiring gap noted in the brain: an lda_rebuild ledger regeneration wipes the position columns, and lobby_position_join is not chained, so downstream priors can go stale silently.
- No group-level latent scale (LPscore analogue) exists to place CU, bank, crypto and fintech associations on a shared axis. Such a scale would make 'conflicting industries' measurable, not only adjudicated.
- Possible conflict with D-021 and the standing rule that inferred positions are never automatic weights. efficiency_layers.py:175-211 puts signed_non_cu_opposition_z, built from inferred Oppose labels pushed onto members through the sponsor/cosponsor roster (org_stance_priors.py:631-700), into contest_index. That artifact is in run_pipeline and shown on the member desk. It moves no rank, but it is an LDA-derived weight inside a displayed score, and its member mapping is the refuted circular roster join. Paul should rule on whether it stays, is labelled UNVERIFIED, or is removed.
- Paul's annotation calls the pipeline something to 'implement directly on an SNA vector graph'. Under D-021 (LDA is an identity source, not an edge or weight source) and the standing rule, Kim positions cannot become graph edges or weights. Flag this; do not recommend it.
- Kim's best GNN configuration adds legislator-lobbyist prior-work (revolving-door) relations. If that relation is ever borrowed, influence language is barred on those edges.
- The paper's 96.93% headline is measured on coder-agreed items only. Paul's position on melange-stance.debate-nli-classifier and melange-validation.gold-set-and-rank-gate cites it as LLM accuracy without that qualifier.
- Internal inconsistencies to cite with care: the abstract says all bills, but the methods say enacted S./H.R. bills; the vetoed-stage pattern uses 3 bills (the SI adds a 74-bill provisionally-killed replication); SI Table 14/15 totals do not reconcile; the text says Support shows no difference, but SI Table 19 has Support significant at p<.10 in 2 of 5 specifications.
- Every figure quoted from the system's benchmark (precision 0.42, recall 0.49, 0 of 23 Amend) is agreement with Kim labels on 876 overlap pairs, not accuracy against human truth. Keep that denominator attached.

Partly aligned, and partly stale. Paul's three findings (stage, firm size and Monitor, industry alignment) match the paper, though the stage result for failing bills is thin (3 vetoed bills, with a 74-bill provisionally-killed replication in the SI). His call that the paper is a 'foundational pre-built pipeline' has already been acted on. The system downloaded the Dataverse release, rebuilt the labelling prompt on Haiku, and benchmarked it. That benchmark is why the pipeline is NOT used to feed the graph directly. Support and Oppose agreement is weak, and the standing rule plus D-021 keep LDA-derived positions as evidence, not edges or weights. His Weakness 2 (no inter-industry conflict per bill) is accurate about the paper's presentation, but the data allows it. The system already builds per-bill ALIGNED and OPPOSED labels for each sector pair, with a CCCA anchor (org_stance_priors.recompute_alliance_conflict), while the contested-bills registry stays display-only pending his ratifications. His Weakness 1 (reactive, not predictive) still holds and is not addressed. His margin note on conflicting industries is borne out by the industry boards' per-axis divergence. His notes do not record that the pipeline was benchmarked, nor that an inferred-Oppose count now feeds the displayed contest_index.

## Related papers

- [[kim-2018-lobbyview-firm-level]] — same-data — Both rest on LDA reports organised in LobbyView. The paper maps MapLight onto lobbying pairs organised in LobbyView.
- [[kim-2020-mapping-political-communities]] — cites — Cited as ref 14 (Kim and Kunisky, Political Analysis 2021), prior work on lobbying-network structure that motivates the GNN graph of groups, bills and legislators.
- [[bonica-2013-mapping-ideological-marketplace]] — cites — Cited as ref 17 (Bonica 2014). The paper compares LPscores with DIME CFscores for 218 groups and finds Pearson r = 0.191, meaning the two measures capture different behaviour. That does not contradict Bonica.
- [[bacik-2025-measuring-dynamical-evolution]] — complements — Same LobbyView group (In Song Kim is a co-author). It studies how the lobbying network evolves over time, while this paper measures each group's position on each bill.
- [[huneeus-2018-effects-firms-lobbying]] — complements — Both relate firm size to lobbying, and In Song Kim co-authored both. Huneeus and Kim study lobbying and resource misallocation. Kim et al. find larger firms lobby more and take Monitor positions more often.
- [[ornstein-2024-train-your-stochastic]] — same-method — Both use zero-shot LLM labelling of political text, validated against human coders.
- [[harrison-2018-application-data-mining]] — same-data — Both mine LDA congressional lobbying records. Kim et al. add bill-position labels at scale.

### Linked from
- [[bombardini-2020-empirical-models-lobbying]] — complements — Kang's mapping of lobbying to provision-level policies required extensive NLP preanalysis; position extraction from lobbying reports addresses the same problem.
- [[pritoni-2025-lobbying-coalitions-determinants]] — complements — Kim et al.'s per-bill Support/Oppose/Engage positions from LDA reports could show the issue-specific group alignments Pritoni describes. D-021 still limits LDA to identity, not edges or weights.
- [[kim-2020-mapping-political-communities]] — complements — Supplies signed bill positions from LDA text, which Kim-Kunisky say their data lack (no for/against distinction).
- [[dai-2025-large-language-model]] — same-method — Both use LLMs to extract positions toward a named target. Kim reports 96.9 percent agreement with two human coders, but only on the 391 items both coders agreed. Dai reports no equivalent agreement for its stance outputs.
- [[boxsteffensmeier-2018-taking-congress-interest]] — complements — Real item (verified). Infers group bill positions from LDA reports, an alternative source of group support, as the analyst said.
- [[moreno-2021-shadowing-shielding-effective]] — complements — Item title matches (Measuring Interest Group Positions on Legislation, 2025); it supplies per-bill opponent placement, which is the input the shadowing response would need.
- [[park-2025-automatic-construction-direction]] — complements — Real item (bill-level Support/Oppose/Engage positions from Lobbying Disclosure Act reports). Its link to bill-sign handling in Paul's system was not verified here.
- [[delano-2025-bicam-congressional-lobbying]] — complements — Real item. BICAM does not cite it. The complement judgment is the analyst's inference from the title, not a BICAM statement.
- [[burnham-2023-stance-detection-practical-guide]] — complements — Title matches. Text reports accuracy 96.93 and F1 97.19, which the analyst rounds to 96.9; the versus-human comparison was not checked.
- [[burnham-2024-political-debate-zero]] — complements — Paul's vault cites it as the anchor for LLM position labels (GPT-4 zero-shot, 96.9 percent on keyword-prefiltered lobbying lines). DEBATE is the small open NLI alternative the system runs. Kim's task does not transfer directly.