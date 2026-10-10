---
type: paper
title: "Empirical Models of Lobbying"
authors: ["Matilde Bombardini", "Francesco Trebbi"]
year: "2020"
venue: "Annual Review of Economics 12:391-413 (first published online May 13, 2020)"
doi: "10.1146/annurev-economics-082019-024350"
category: "Lobbying"
systems: ["melange-influence-engine", "melange-allocation", "mentat-giving-forecast", "melange-lobbying", "melange-pathfinding", "mentat-sector-growth-study"]
components: ["melange-influence-engine.flip-model", "melange-allocation.break-even-and-roi", "melange-influence-engine.money-response-defection", "mentat-giving-forecast.calculator-and-nowcast", "melange-lobbying.finance-client-classification", "melange-pathfinding.staffer-closeness-routes", "melange-lobbying.contested-bills", "melange-allocation.stance-money-gate", "mentat-sector-growth-study.growth-spending-regressions"]
paul_annotated: true
paul_markup_count: 106
evidence_status: "verified"
source: "[[_sources/bombardini-2020-empirical-models-lobbying.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\annurev-economics-082019-024350.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\annurev-economics-082019-024350.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\annurev-economics-082019-024350.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\annurev-economics-082019-024350.pdf", "C:\\Users\\pgoff\\Downloads\\annurev-economics-082019-024350.pdf"]
sha12: ["30b660e13db8"]
tags: [paper, lobbying, type/paper, theme/lobbying, evidence/verified]
---

# Empirical Models of Lobbying

**Matilde Bombardini, Francesco Trebbi (2020).** *Annual Review of Economics 12:391-413 (first published online May 13, 2020).* [10.1146/annurev-economics-082019-024350](https://doi.org/10.1146/annurev-economics-082019-024350) Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Bombardini and Trebbi (2020) is a narrative review of empirical lobbying research. It contains no new estimation. It sorts the literature into quid pro quo models, where political resources are exchanged for policy, and informational models, where interest groups pass expertise to less-informed policymakers. It then asks how each line of work identifies policy supply and demand and the counterfactual of no lobbying.
> 
> The review reaches three conclusions that matter for Paul's systems.
> 
> First, structural estimates of what money buys are fragile. Kang (2016) estimates that lobbying raises enactment by only 0.021% to 0.415% over an 8% baseline (538 energy policies, 110th Congress), yet the same model implies more than $500m of value per policy. A naive Farm Bill comparison gives a return near 600,000% ($22bn of 2000 subsidies against under $3.7m of agribusiness PAC money). The authors read Kang's figures as possible symptoms of misspecification; the separate point that observable contributions miss other influence channels comes from the Farm Bill discussion. Adding an electoral-vote channel (Bombardini and Trebbi 2011) lowers the Farm Bill figure to $13.7 per dollar, which the authors call still high but more reasonable.
> 
> Second, lobbying is persistent: 92% of firms that lobby in one year lobby the next (Kerr et al. 2014). The structure of the industry has also shifted: from 1999 to 2008 the share of in-house lobbyists (headcount, not lobbying spending) fell from about 60% to under 40%, while external lobbying doubled.
> 
> Third, in the revolving-door literature, a lobbyist's revenue depends on the patron staying in office. Former aides lose a persistent 24% of revenue when their senator exits (Blanes i Vidal et al. 2012), and lobbyists follow politicians across committee reassignments (Bertrand et al. 2014).
> 
> For the system as built, the review supports keeping money a minority, saturating channel in the flip and money-response engines, and treating ROI figures as fragile. It supports the persistence-based lobbying nowcast in Mentat and the rule that in-house and outside-firm lobbying are never summed. It also exposes one gap, confirmed only in relationship_uncertainty.py and member_context.py and not repo-wide: the staffer route costs decay with time since overlap but do not account for whether the anchoring member is still in office or holds a powerful committee seat.

## Paul's annotation

From [[annotations/Lobbying - Lobbying - Annotated Bibliography|Lobbying - Lobbying - Annotated Bibliography]]:

> Bombardini, Matilde, and Francesco Trebbi. “Empirical models of lobbying.” *Annual Review of Economics*, vol. 12, no. 1, 2 Aug. 2020, pp. 391–413, [https://doi.org/10.1146/annurev-economics-082019-024350](https://doi.org/10.1146/annurev-economics-082019-024350).

## Paul's markup in the PDF

- p1 (journal p.391) Highlight: "Paraphrase: scope of the review of the recent empirical literature on lobbying"
- p1 (journal p.391) Highlight: "Paraphrase: quid pro quo framing of special-interest politics"
- p1 (journal p.391) Highlight: "Paraphrase: informational issues in special-interest politics"
- p1 (journal p.391) Highlight: "Paraphrase: open question on which equilibrium policies lobbying shapes"
- p1 (journal p.391) Highlight: "Paraphrase: open question on how far equilibrium policy departs from the social optimum"
- p1 (journal p.391) Highlight: "Paraphrase: welfare effects of lobbying are hard to quantify"
- p2 (journal p.392) Highlight: "Paraphrase: core question of whether lobbying is welfare-enhancing or welfare-distorting"
- p2 (journal p.392) Highlight: "Paraphrase: lobbying as a route from concentrated economic power to corruption"
- p2 (journal p.392) Highlight: "Paraphrase: 53% survey result on lobbyists and special interests"
- p2 (journal p.392) Highlight: "Paraphrase: 38% survey comparison for illegal immigration"
- p2 (journal p.392) Highlight: "Paraphrase: review will not settle the bad-or-good question"
- p2 (journal p.392) Highlight: "Paraphrase: direct versus intermediated business-official interaction"
- p3 (journal p.393) Highlight: "Paraphrase: safety regulation as a blocking demand that leaves no visible law change"
- p3 (journal p.393) Highlight: "Paraphrase: non-quid-pro-quo informational perspective in other disciplines"
- p3 (journal p.393) Highlight: "Paraphrase: lobbying industry body (National Institute for Lobbying and Ethics) cited"
- p3 (journal p.393) Highlight: "Paraphrase: industry body's claim that lobbying supplies needed information"
- p4 (journal p.394) Highlight: "Paraphrase: exogenous technology shocks to lobbyist intermediation as identification"
- p4 (journal p.394) Highlight: "Paraphrase: organization into structural quid pro quo versus reduced-form approaches"
- p5 (journal p.395) Highlight: "Paraphrase: PAC and campaign contributions dominated research since the 1990s"
- p5 (journal p.395) Highlight: "Paraphrase: fragment (single letter) from the highlighted text; no substantive content"
- p5 (journal p.395) Highlight: "Paraphrase: lobbying expenditures as a much larger interest-group measure than PACs"
- p5 (journal p.395) Highlight: "Paraphrase: Senate Office of Public Records as primary lobbying-data source"
- p5 (journal p.395) Highlight: "Paraphrase: annual lobbying spend of $3-3.5 billion over the decade"
- p5 (journal p.395) Highlight: "Paraphrase: corporate PAC data over-studied because available early"
- p5 (journal p.395) Highlight: "Paraphrase: dark money as the harder-to-study phenomenon"
- p5 (journal p.395) Highlight: "Paraphrase: 501(c)4 reference (political spending by social welfare groups)"
- p5 (journal p.395) Highlight: "Paraphrase: 501(c)4 spending allowed when not the majority of activity"
- p5 (journal p.395) Highlight: "Paraphrase: lobbyist count declining to 11,654 by 2018"
- p6 (journal p.396) Highlight: "Paraphrase: external lobbying drove growth in lobbying expenses (doubled 1999-2008)"
- p7 (journal p.397) Highlight: "Paraphrase: structural approach requires specifying what F and P each gain from exchange"
- p7 (journal p.397) Highlight: "Paraphrase: structural estimates must fit data without nonsensical parameter values"
- p7 (journal p.397) Highlight: "Paraphrase: structural assumptions can be verified and rejected by data"
- p7 (journal p.397) Highlight: "Paraphrase: structural models yield independently interesting parameters"
- p7 (journal p.397) Highlight: "Paraphrase: Kang models lobbying, outcomes and returns jointly"
- p7 (journal p.397) Highlight: "Paraphrase: key features of Kang (2016) analysis"
- p7 (journal p.397) Highlight: "Paraphrase: Kang separates policies from the bills carrying them"
- p8 (journal p.398) Highlight: "Paraphrase: Kang's policies are carried by bills and must be linked to them"
- p8 (journal p.398) Highlight: "Paraphrase: Kang models lobbying as contests among special-interest coalitions"
- p8 (journal p.398) Highlight: "Paraphrase: Kang assumes collective-action problems solved within coalitions"
- p8 (journal p.398) Highlight: "Paraphrase: policies without lobbying are enacted rarely (about 1%)"
- p8 (journal p.398) Highlight: "Paraphrase: overall enactment rate of about 8%"
- p8 (journal p.398) Highlight: "Paraphrase: baseline enactment probability below one absent other special interests"
- p8 (journal p.398) Highlight: "Paraphrase: Kang's estimation maximizes likelihood of enactment, lobbying and expenditures"
- p8 (journal p.398) Highlight: "Paraphrase: simulation needed because equilibrium objects lack closed form"
- p8 (journal p.398) Highlight: "Paraphrase: counterfactual no-lobbying effect as the key question"
- p9 (journal p.399) Highlight: "Paraphrase: implied $500m+ value per policy flagged as possible misspecification"
- p9 (journal p.399) Highlight: "Paraphrase: excessive political returns common in this literature"
- p9 (journal p.399) Highlight: "Paraphrase: Bombardini and Trebbi model of multilateral bargaining with heterogeneous interests"
- p9 (journal p.399) Highlight: "Paraphrase: multiple influence tools used together with substitution patterns"
- p9 (journal p.399) Highlight: "Paraphrase: large employers need fewer campaign contributions than small groups"
- p9 (journal p.399) Highlight: "Paraphrase: rate of return on political spending recovered from structural model"
- p10 (journal p.400) Highlight: "Paraphrase: low contributions and rarely binding caps versus large policy stakes"
- p10 (journal p.400) Highlight: "Paraphrase: implausible returns should be competed away in political markets"
- p10 (journal p.400) Highlight: "Paraphrase: electoral-weight channel makes implied returns reasonable"
- p10 (journal p.400) Highlight: "Paraphrase: Huneeus and Kim study lobbying-induced capital wedges"
- p10 (journal p.400) Highlight: "Paraphrase: Huneeus and Kim heterogeneous-firm model with fixed lobbying costs"
- p11 (journal p.401) Highlight: "Paraphrase: Huneeus and Kim normalize and calibrate a subset of parameters"
- p11 (journal p.401) Highlight: "Paraphrase: credible shock to lobbying productivity as identification"
- p11 (journal p.401) Highlight: "Paraphrase: committee membership changes as presumably exogenous shocks"
- p11 (journal p.401) Highlight: "Paraphrase: Huneeus and Kim find lobbying welfare-diminishing with large distortions"
- p11 (journal p.401) Highlight: "Paraphrase: quid pro quo as an exchange of political resources for policy (different highlight colour)"
- p12 (journal p.402) Highlight: "Paraphrase: reduced-form work uses demand shocks or supply shocks"
- p12 (journal p.402) Highlight: "Paraphrase: supply shocks determine policy obtained per lobbying dollar"
- p12 (journal p.402) Highlight: "Paraphrase: demand and supply schedules written in terms of lobbying spend and policy"
- p12 (journal p.402) Highlight: "Paraphrase: fragment of the preceding sentence (highlight artifact)"
- p12 (journal p.402) Highlight: "Paraphrase: demand-shifter shift in C identifies supply parameters"
- p12 (journal p.402) Highlight: "Paraphrase: demand shifts tied to firm policy needs"
- p12 (journal p.402) Highlight: "Paraphrase: demand for lobbying services identified by shifts in supply"
- p13 (journal p.403) Highlight: "Paraphrase: university overhead rates make grants more valuable (de Figueiredo and Silverman)"
- p13 (journal p.403) Highlight: "Paraphrase: first stage in which overhead rates predict lobbying"
- p13 (journal p.403) Highlight: "Paraphrase: political mismatch between city and state causes local underprovision"
- p14 (journal p.404) Highlight: "Paraphrase: Kerr et al. classification of firms dependent on high-skill immigration"
- p14 (journal p.404) Highlight: "Paraphrase: Blanes i Vidal et al. measure revenue loss when a connected senator leaves"
- p14 (journal p.404) Highlight: "Paraphrase: senator exit assumed orthogonal to lobbyist fortunes is a strong assumption"
- p15 (journal p.405) Highlight: "Paraphrase: revolving-door lobbyists lose 24% of revenue when patron exits"
- p15 (journal p.405) Highlight: "Paraphrase: larger revenue drops for powerful-committee connections"
- p15 (journal p.405) Highlight: "Paraphrase: lobbyists as providers of information and expertise to Members"
- p15 (journal p.405) Highlight: "Paraphrase: whether lobbyists stick to specific policy issues or to politicians"
- p15 (journal p.405) Highlight: "Paraphrase: access-versus-expertise predictions for lobbyist job assignments"
- p15 (journal p.405) Highlight: "Paraphrase: lobbyists take up new issues as past politicians change committees (Bertrand et al.)"
- p15 (journal p.405) Highlight: "Paraphrase: expertise premium does not rise when an issue becomes more relevant"
- p15 (journal p.405) Highlight: "Paraphrase: access dominates expertise in lobbying"
- p15 (journal p.405) Highlight: "Paraphrase: finding is inconsistent with a pure expertise view"
- p16 (journal p.406) Highlight: "Paraphrase: positive theoretical role for communication between F and P"
- p16 (journal p.406) Highlight: "Paraphrase: information is supplied but not always the kind that serves the public"
- p16 (journal p.406) Highlight: "Paraphrase: premise that the policymaker is less informed than the interest group"
- p16 (journal p.406) Highlight: "Paraphrase: cheap-talk range communication (Crawford and Sobel)"
- p16 (journal p.406) Highlight: "Paraphrase: multiple groups with opposing biases raise welfare in cheap talk"
- p16 (journal p.406) Highlight: "Paraphrase: fixed exogenous lobbying costs in costly-signaling models"
- p17 (journal p.407) Highlight: "Paraphrase: information as a subsidy to resource-strapped legislators"
- p17 (journal p.407) Highlight: "Paraphrase: the lobbying industry as an intermediary"
- p17 (journal p.407) Highlight: "Paraphrase: Hirsch et al. ideology measure via dynamic nominal three-step estimation (DW-NOMINATE)"
- p17 (journal p.407) Highlight: "Paraphrase: DIME campaign-finance ideology scores"
- p17 (journal p.407) Highlight: "Paraphrase: contact with a politician more likely for ideologically aligned lobbyists"
- p17 (journal p.407) Highlight: "Paraphrase: conditional on contact, aligned lobbyists bring fewer clients"
- p18 (journal p.408) Highlight: "Paraphrase: ideologically close or connected lobbyists set a higher certification threshold"
- p18 (journal p.408) Highlight: "Paraphrase: expert lobbyists have more balanced party affiliation"
- p18 (journal p.408) Highlight: "Paraphrase: listening to senders with opposing biases gives a better-informed policymaker"
- p18 (journal p.408) Highlight: "Paraphrase: two types of information, general welfare versus constituency benefit"
- p18 (journal p.408) Highlight: "Paraphrase: lobbyists build reputation by filtering salient dimensions of proposals"
- p18 (journal p.408) Highlight: "Paraphrase: NLP applied to public comments from 150 federal agencies"
- p18 (journal p.408) Highlight: "Paraphrase: nonprofit comment similarity to benefactor rises after a donation"
- p20 (journal p.410) Highlight: "Paraphrase: careful data work is advancing the research frontier"
- p20 (journal p.410) Highlight: "Paraphrase: organization of research as quid pro quo or informational"
- p20 (journal p.410) Highlight: "Paraphrase: welfare effects of lobbying should be studied under the quid pro quo perspective"
- p20 (journal p.410) Highlight: "Paraphrase: some structural models display symptoms of misspecification"

## Study

**Question.** Is lobbying (direct or intermediated business-government interaction over policy) welfare-enhancing or welfare-distorting? The review organizes recent empirical work on quid pro quo and informational lobbying, asks how to identify policy supply and demand and the counterfactual equilibrium with no lobbying, and assesses how distorted equilibrium policy is relative to the social optimum.

**Theory.** Quid pro quo perspective: Grossman and Helpman (1994, 1996) menu-auction and electoral models; policy supply and demand framework (lD = C Q^(1-1/eps), lS = G Q^(1+1/eta)), Figure 1; Tullock (1967) contest function as used by Kang (2016); bargaining across heterogeneous special interests (Bombardini and Trebbi 2011). Informational perspective: cheap talk (Crawford and Sobel 1982; Krishna and Morgan 2001; Battaglini 2002), costly or money-burning signaling (Potters and van Winden 1992; Grossman and Helpman 2001), hybrid access-fee models (Austen-Smith 1995; Lohmann 1995), legislative subsidy (Hall and Deardorff 2006), and information verification by lobbying intermediaries (Groll and Ellis 2014; Hirsch et al. 2019).

**Data.** No original data. Sources surveyed: LDA filings via Senate Office of Public Records (SOPR) and CRP (total lobbying about $3-3.5 billion per year over the decade to 2019 versus $200-250 million PAC donations; average 12,537 unique lobbyists per year 1998-2019, 11,654 in 2018; about 37,000 active lobbyists 1999-2008 per Bertrand et al. 2014); FEC and CRP PAC data; LobbyView/Compustat (Huneeus and Kim 2019: 766 of 7,646 US public firms lobbied in 2017); Washington Representatives directory (about 14,000 of 37,000 lobbyists profiled); LegiStorm staffer salaries (2012); Kang (2016) 538 energy policies in 445 bills, 110th Congress; Hirsch et al. (2019) 219 FARA lobbyists, 110th-111th Congress; federal rulemaking comments from 150 agencies (Bertrand et al. 2018a); Canada Registry (about 9,000 lobbyists, 2017-18); EU Transparency Register (5,431 registrants in 2012, 11,901 in 2018).

**Method.** Narrative review organized as quid pro quo (structural econometric and reduced-form) versus informational. Structural methods reviewed: Kang (2016) Tullock-contest game with simulated maximum likelihood and equilibrium selection; Bombardini and Trebbi (2011) structural first-order conditions yielding a dollar-per-vote cost; Huneeus and Kim (2019) heterogeneous-firm general-equilibrium model estimated by minimum distance on simulated moments. Reduced-form methods reviewed: difference-in-differences (Blanes i Vidal et al. 2012), instrumental variables (de Figueiredo and Silverman 2006 with overhead rates; Goldstein and You 2017 with distance to Washington), natural experiments (Kerr et al. 2014 H-1B cap; Dellavigna et al. 2016 Berlusconi advertising), and observational contact and mobility regressions (Bertrand et al. 2014; Hirsch et al. 2019). Text methods: NLP to classify bill policies (Kang 2016, p.397) and latent semantic analysis of comments (Bertrand et al. 2018a, p.408, n.35).

**Identification.** Varies by paper. Huneeus and Kim (2019): committee-assignment shocks to senators connected to firms (p.401, n.18). Kang (2016): equilibrium selection (maximizes sum of payoffs) and simulated counterfactual with no lobbying. de Figueiredo and Silverman (2006): university overhead rates shift policy demand (p.403). Goldstein and You (2017): political mismatch between city and state, with distance from Washington as IV (p.403, n.20). Kerr et al. (2014): predetermined H-1B cap expiry, differential exposure (p.404). Blanes i Vidal et al. (2012): exit of connected senator, assumed exogenous (p.404). Dellavigna et al. (2016): Berlusconi office-holding shifts ad spending to Mediaset (p.405-406). Bertrand et al. (2014): committee reassignment of politicians and lobbyist following (p.405).

**Scope.** Population: mainly US federal lobbying (LDA data 1999-2019 with 1998 onward totals), public and business interests, mostly the Congress and federal agencies; secondary sections on Canada (Sec. 5.1) and EU (Sec. 5.2). Institutions: Congress, federal agencies, Senate and House committees, and EU Commission and Parliament. Era: literature from the 1990s through 2019, with some references to 1971 FECA disclosure rules and the 2007 LDA amendments.

## Key findings

- Survey context: 53% of respondents called lobbyists and special interest groups in Washington a very big problem, versus 38% for illegal immigration (Pew surveys, Sept 2018-Mar 2019). (Sec. 1 Introduction, p.392; 53% vs 38%)
- Kang (2016) energy-sector lobbying: only 8% of 538 policies in 445 bills are enacted; policies with no lobbying are enacted about 1% of the time; policies with two or more lobbying players about 24%. (Sec. 3.1, p.398; 8% overall; 1% no lobbying; 24% two or more lobbyists)
- Kang's model implies lobbying raises enactment by only 0.021% to 0.415% over an 8% baseline, and average returns to lobbying of 137% (renewables) to 152% (oil and gas). The implied monetary value per policy above $500 million is flagged by the authors as possibly signaling misspecification. (Sec. 3.1, pp.398-399; 0.021%-0.415% effect; 137%-152% returns; >$500m per policy)
- Bombardini and Trebbi (2011) structural bargaining model on Congressional campaign contributions estimates $200-$400 of donations per vote acquired and a precise average rate of return on political spending. Adding the electoral-vote channel makes implied returns far more reasonable. (Sec. 3.1, p.399 and p.400 n.17; $200-$400 per vote; agriculture return about $13.7 per dollar at the margin; simulated returns 10%-20% per year)
- Farm Bill comparison: about $22 billion in 2000 agribusiness subsidies against under $3.7 million in agribusiness PAC contributions implies a naive return of about 600,000%, which the authors argue shows that observable contributions miss other influence channels. (Sec. 3.1, p.400; $22bn vs $3.7m; about 600,000%)
- Huneeus and Kim (2019): a 10% increase in lobbying raises revenue 3%. Federal lobbying reduces aggregate US productivity 22%, with 59% from misallocation to lobbying (less productive) firms and 41% from missing entry of productive firms. (Sec. 3.1, p.401; 10% lobbying -> 3% revenue; 22% productivity loss; 59%/41% split)
- Huneeus and Kim lobbying facts: 766 of 7,646 US public firms lobbied in 2017; lobbying firms have about four times the revenue of non-lobbying firms; elasticity of lobbying to sales about one conditional on lobbying. (Sec. 2.2, pp.396-397; 766/7,646 firms; about 4x revenue; elasticity about 1)
- Lobbying is persistent: 92% of firms that lobby in a year also lobby the next year (Kerr et al. 2014), consistent with large sunk entry costs. (Sec. 2.2, p.397; 92%)
- Industry-association share: individual firms account for an average 67% (median 96%) of lobbying per Bombardini and Trebbi (2012), and industries with lower concentration and differentiation use trade associations more. (Sec. 2.2, p.397 n.11; mean 67%, median 96%)
- Scale of federal lobbying: total spending about $3-3.5 billion per year over the decade, versus $200-250 million per year in PAC donations; about 12,537 unique lobbyists per year 1998-2019 on average. (Sec. 2, p.395 and Sec. 2.1, p.395; $3-3.5bn vs $200-250m; avg 12,537 lobbyists)
- Earmarks (de Figueiredo and Silverman 2006): an extra dollar of lobbying yields about one dollar more in earmarks for universities with no Appropriations-committee ties, and $4.52-$5.24 for universities with connections. (Sec. 3.2.1, p.403; $1 vs $4.52-$5.24)
- Goldstein and You (2017) cities: approximate return of $1.02 in earmarks and $0.47 in 2009 Recovery Act grants per $1 of lobbying, using distance-based IV. (Sec. 3.2.1, p.403; $1.02 earmarks; $0.47 ARRA grants)
- Kerr et al. (2014): after the H-1B visa cap reduction, high-skill-immigration lobbying as a share of total lobbying doubles for dependent firms; the effect is stronger for firms that lobbied on other issues before, consistent with fixed entry costs. (Sec. 3.2.1, p.404; share about doubles)
- Blanes i Vidal et al. (2012): revolving-door lobbyists (former aides) lose a persistent 24% of lobbying revenue when their connected senator exits office. Drops are larger for Finance, Appropriations, or Ways and Means connections and insignificant otherwise; the exit-exogeneity assumption is flagged as strong. (Sec. 3.2.2, pp.404-405; 24% revenue drop)
- Bertrand et al. (2014): about 37,000 active lobbyists in 1999-2008; in-house share fell from nearly 60% to under 40%; external lobbying doubled 1999-2008. Lobbyists follow politicians across committees, and the expertise premium does not rise with issue relevance, which the authors read as access dominating expertise. (Sec. 2.1 p.396 and Sec. 3.2.2 p.405; about 37,000 lobbyists; in-house share 60% to <40%; external lobbying doubled)
- Dellavigna et al. (2016): regulated firms in Italy shift advertising onto Berlusconi-owned Mediaset during his periods in office, only in heavily regulated industries, at a scale of hundreds of millions of euros. (Sec. 3.2.2, pp.405-406; hundreds of millions of euros)
- Hirsch et al. (2019) FARA data: lobbying contact is more likely with ideologically aligned Members, and conditional on contact, lobbyists with ideological closeness or prior connections bring fewer clients and set higher certification thresholds. (Sec. 4, pp.407-408; 219 lobbyists; 110th-111th Congress)
- Bertrand et al. (2018a) with latent semantic analysis of rulemaking comments from 150 agencies: a nonprofit comment's similarity to its benefactor's comments rises immediately after a donation. (Sec. 4, p.408; 150 agencies)
- Canada: about 9,000 registered lobbyists in 2017-18, just over 1,000 external; compared with 11,654 US lobbyists in the same year. Hickey (2014) finds only 6% of Canadian lobbyists follow reshuffled ministers. (Sec. 5.1, p.409; about 9,000 (Canada) vs 11,654 (US); 6% follow ministers)
- EU: the voluntary Transparency Register grew from 5,431 registrants (2012) to 11,901 (2018). Apple reported EUR 2-2.25 million of EU lobbying in 2018 versus $6.7 million in the US. Dellis and Sondermann (2017) match about 2,000 Orbis firms to the register. (Sec. 5.2, pp.409-410; 5,431 to 11,901 registrants; about 2,000 matched firms)

## Limitations

- Authors state that the two main unresolved issues are how to account for lobbying's effect on which equilibrium policies are chosen, and how distorted those policies are relative to the social optimum (abstract, p.391); a comprehensive welfare assessment is described as elusive.
- Authors caution that the review resolves little of the good-or-bad question (Sec. 1, p.392).
- Lobbying may be invisible to researchers when it blocks change, leaving only a status quo with no visible law change (p.393).
- Informational models are rare in empirical work because data on the informational needs of agents and on transmitted expertise are very hard to obtain (p.394). Information is hard to quantify (p.393).
- Structural models show signs of misspecification: Kang's near-zero lobbying effect and $500m+ implied value per policy (pp.398-399); the authors say some structural models display symptoms of misspecification (p.410).
- Structural identification is not assured: Huneeus and Kim's minimum-distance criterion may not have a unique minimizer and should be probed with Monte Carlo (p.401).
- Kang (2016) equilibrium multiplicity: equilibrium is selected by maximizing total payoffs; budget constraints are excluded and games treated as independent (p.398, n.14).
- Exogeneity and exclusion assumptions: Blanes i Vidal et al. assume senator exit is orthogonal to lobbyist fortunes, which is strong if senators retire strategically (p.404); distance IV in Goldstein and You is open to exclusion-restriction criticism (p.403, n.20); Goldstein and You do not use cumulative transfers (p.403, n.21).
- Measurement of lobbying effects: Kang's policy-level and bill-level mapping needed heavy preprocessing; omitting bill refinement would mismeasure lobbying effects (p.398, n.12). Lobbying spending alone is not the full influence arsenal (Sec. 2, p.394).
- Cross-country comparability limited: Canadian Registry lacks fee data (p.409); EU register is voluntary with limited history, and EU firm counts are not directly comparable with Compustat counts (p.410).

## Short quotes

- "An interest group can rarely communicate exactly the value of some important variable" — Sec. 4, p.406 (single quote kept under copyright limit; attribution Bombardini and Trebbi 2020)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | supports | Kang (2016) structural Tullock-contest model of energy policy (538 policies in 445 bills, 110th Congress): lobbying raises enactment probability by only 0.021%-0.415% relative to an 8% baseline, while average returns to lobbying are estimated at 137% (renewables) to 152% (oil and gas). The review reads both as possible misspecification. The separate point that observable contributions miss other influence channels comes from the Farm Bill / Bombardini and Trebbi (2011) discussion, not from Kang. (Sec. 3.1, pp.398-400) | pflip = a/(a+b), where money is the minority partner (WM 0.35 vs peer weight WP 0.65). The money term saturates relative to the member's receipts (HALFF 0.10), on the stated premise that money buys access and effort more than votes. `fec/engine/flip_model.py:3-7,21,34` | Kang measures firm lobbying expenditure against provision-level enactment in the energy sector. Melange's money leg is PAC contributions against member-level winnability on CU topics. The review supports only the direction (small marginal effect of money), not the WM/WP split or the HALFF constant, which remain unestimated design choices. The evidence is federal and from 2007-08. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Break-even CU spend and ROI floor\|Break-even CU spend and ROI floor]] | qualifies | Kang's model implies an average policy value of more than $500m, and the Farm Bill comparison gives a naive return of 600,000% ($22bn in 2000 subsidies vs under $3.7m in agribusiness PAC contributions that cycle). The review says excessive returns are common in this literature. Adding an electoral-vote channel (Bombardini and Trebbi 2011: $200-$400 per vote) lowers the Farm Bill figure to $13.7 per dollar, which the authors call still high but more reasonable; B&T's simulations give average returns of 10%-20% per year. The conclusion notes some structural models display symptoms of misspecification. (Sec. 3.1, pp.399-400 and n.17; Sec. 6 conclusions) | Saturating share-type money term in shift_lean (money_response.py) that break-even computations share. The dashboard template exists at fec/surfaces/templates/make_dashboard_html_00.tpl; no roi_verify.py was found in the repo, so the claim about ROI floors checked by roi_verify.py is unconfirmed. `fec/engine/money_response.py:68-75 (shift_lean saturating form); fec/surfaces/templates/make_dashboard_html_00.tpl (path only)` | This is a caution about any ROI read off contribution-to-outcome ratios. It applies whenever dashboard ROI floors are presented as returns. It does not invalidate the break-even algebra, which is internal to the model. Any ROI shown should carry an UNVERIFIED or model-output label. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Money response (lean shift) and money-triggered defection\|Money response (lean shift) and money-triggered defection]] | supports | Structural work finds a very small marginal enactment effect of lobbying (Kang 0.021%-0.415%) despite large stakes, and campaign contributions are low with federal donation caps very seldom binding (Ansolabehere et al. 2003, as reported). (Sec. 3.1, pp.398-400) | MAXSHIFT was cut from 40 to 18 on the premise that money moves positions little. A defection requires a donation of about one sixth of the member's receipts, so a $10k gift shifts median receipts by about 0.9 lean, far below DTHRESH 11.25. `fec/engine/money_response.py:15-25` | The review addresses enactment and bill outcomes, not shifts in individual member stance. It supports rare money-triggered defection but not the LOSSFRAME 1.10 asymmetry or the specific constants. | weak | VERIFIED |
| [[systems/mentat-giving-forecast#Calculator rules, company-PAC nowcast and BLEND\|Calculator rules, company-PAC nowcast and BLEND]] | supports | Lobbying is persistent: 92% of firms that lobby in a given year also lobby the next (Kerr et al. 2014), explained by large sunk costs. Conditional on lobbying, the elasticity of lobbying expenditure to firm sales is around one (Huneeus and Kim 2019). (Sec. 2.2, p.397) | The lobbying projection is an in-year nowcast (amount filed so far plus (1-s) times the prior full year), carried forward unchanged. Its bands come from LobbyView persistence error. The earlier year-effect slope model never beat last-period naive (G9). `C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py:1290-1297` | The 92% figure is a firm-level extensive-margin rate (does a firm lobby at all), not dollar-level persistence for an industry. The review offers no evidence on how lobbying dollars move within a cycle. | moderate | VERIFIED |
| [[systems/mentat-giving-forecast#Calculator rules, company-PAC nowcast and BLEND\|Calculator rules, company-PAC nowcast and BLEND]] | supports | Bertrand et al. (2014), about 37,000 active lobbyists over 1999-2008: the share of in-house LOBBYISTS fell from nearly 60% to under 40%, and the roughly 30% growth in lobbying expenses was almost entirely due to external lobbying, which doubled. (Sec. 2.1, pp.395-396) | The lobbying card shows hired-firm income and in-house expenses side by side and never adds them. The sector-growth config records that the undifferentiated LobbyView sum double-counts self-filers' outside-firm fees (a median of 9-26% by sector). `C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py:553-562; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_config.R:80-81,119-120` | The 60%->40% figure is a share of lobbyists (headcount), not of lobbying dollars, over all federal LDA lobbying 1999-2008. No finance or CU sector breakdown is reported, so this supports showing the two channels separately but provides no sector-specific shares. | weak | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#LDA finance-client classification and report join\|LDA finance-client classification and report join]] | supports | Bombardini and Trebbi (2012) report an average share of lobbying by individual firms of 67% (median 96%); industries with lower concentration and lower product differentiation spend a higher share of lobbying through trade associations. Huneeus and Kim (2019) put business-association lobbying at less than a quarter of all lobbying expenses. (Sec. 2.2, p.397 and n.11) | Finance clients are classified bank, CU or other from client names. The brain records that directional positions sit with trade associations (CUNA 174 Support / 12 Oppose, NAFCU 168 Support / 23 Oppose; the 22 pinned fintech and crypto firms hold 1 directional position out of 56). ABA lobbying is $228M against CUNA's $93M. `C:/Users/pgoff/Documents/brain/actors/lobbyview-collector.md:93-97,102,132` | The prediction is cross-industry, from Bombardini and Trebbi's 2012 trade-issue sample. The review gives no CU-specific figure. | moderate | VERIFIED |
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | suggests-extension | Former Senate staffers turned lobbyists experience a persistent 24% drop in lobbying revenues when their connected senator exits office (Blanes i Vidal et al. 2012); drops are larger for connections on Finance, Appropriations or Ways and Means and statistically insignificant otherwise. Bertrand et al. (2014) find lobbyists take up new issues as the politicians they contacted change committee assignments. The review flags B-i-V's exogeneity of senator exit as a strong assumption. (Sec. 3.2.2, pp.404-405) | Route cost uses time-decayed exposure (5-year staleness half-life), workplace concentration, and a member-context modifier built from cosponsor-graph proximity times eigenvector centrality. Neither file reads whether the anchoring member is still serving; member_context.py explicitly refuses staff or committee affiliation layers. `fec/staffer_network/relationship_uncertainty.py:11-41; fec/staffer_network/member_context.py:34-66,87-88` | The evidence measures lobbyists' revenue, not introduction success, closeness or influence. Under D-025 and the revolving-door wording rule, an added term may be described only as dependence on the patron's current office, never as access or influence. The review text does not state Blanes i Vidal's sample years. Absence of an office/committee term was checked only in these two files, not repo-wide (UNVERIFIED as a system-wide gap). | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Contested-bills registry\|Contested-bills registry]] | suggests-extension | Kang (2016) models lobbying as Tullock contests among four energy coalitions, mapped at the level of policy provisions decoupled from bills. Policies with no lobbying are enacted about 1% of the time and those with two or more lobbying players about 24%, which the review says implies substantial selection. The structural effect of lobbying is much smaller (0.021%-0.415%); entry has multiple equilibria resolved by a selection assumption. (Sec. 3.1, pp.397-399, n.12, n.14) | A per-bill contested or advisory registry with explicit counterparty industries. The loader has shipped. No engine consumer exists, and the CONTESTED_V1 dial is only planned. `fec/lobbying/contested_bills.py:2-15,33-42` | A contest success function is a candidate form for the planned counterparty term, but Kang's model carries misspecification symptoms and selects among multiple equilibria by assumption. The 1% vs 24% contrast reflects selection and must not be quoted as a lobbying effect. Weighting contests by LDA spend would conflict with D-021. | weak | VERIFIED |
| [[systems/melange-allocation#Stance money gate (F1)\|Stance money gate (F1)]] | supports | Hall and Deardorff (2006) treat information as a subsidy to resource-strapped legislators, which explains why lobbyists lobby friendly legislators and questions the persuasion role. Hirsch et al. (2019; FARA contacts, 219 foreign-interest lobbyists, 110th-111th Congress) find any lobbying contact is more likely for lobbyists ideologically aligned with the politician. (Sec. 4, p.407 and n.32) | Members whose overall tier is Skeptic, Highly Skeptical or Opponent are blocked as money targets in the allocators (per_topic_flip, optimize; lt_priming and dashboard hold copies). The gate fails open for members with no matrix row. `fec/moneyball_core.py:717-729` | The contact evidence concerns foreign-principal lobbyists under FARA, not domestic PAC dollars. It is consistent with funding allies but does not test whether contributions to opposed members change their positions. | weak | VERIFIED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | qualifies | Huneeus and Kim (2019; LobbyView-Compustat): 766 of 7,646 US public firms lobbied in 2017; lobbying firms have almost four times the revenue of non-lobbying firms; conditional on lobbying, the elasticity of lobbying to sales is around one. Using committee-assignment shocks for identification, a 10% increase in lobbying produces a 3% increase in revenue. (Sec. 2.2, pp.396-397; Sec. 3.1, p.401) | Sector-year regressions of giving and lobbying on lagged sector growth with year-FE variants; the cited code handles year-FE variants (no within-sector variation in sector-only regressions). The reported results (lobbying lead flips sign under year FE; growth adds no adjusted R2) are not visible at the code_ref and are UNVERIFIED here. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_estimate.R:384-398` | The review's figures are firm-level cross-sectional size elasticities and a firm-level revenue effect. They do not predict a within-sector growth-to-spending effect once year shocks are absorbed, so a sector-year null does not conflict with them. They should not be cited as support for a deep-pockets hypothesis at sector-year grain. | moderate | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Says the in-house share of lobbying fell from about 60% to under 40%; the paper reports the share of in-house LOBBYISTS (headcount), not of lobbying spending.
> - Summary issue: Says adding an electoral-vote channel brings implied returns down to 'plausible levels'; the paper calls the resulting $13.7 per dollar 'still a high figure' but more reasonable, with B&T simulations at 10-20% per year.
> - Summary issue: Attributes the 'influence channels left out of the model' reading partly to Kang's figures; in the review that explanation is offered for the Ansolabehere/Farm Bill puzzle via Bombardini and Trebbi (2011), while Kang's oddities are read as possible misspecification.
> - Summary issue: Relies on roi_verify.py checking ROI floors and a template at fec/templates/; roi_verify.py was not found and the template lives at fec/surfaces/templates/.
> - Summary issue: The staffer-route gap (no in-office or committee term) was confirmed only in relationship_uncertainty.py and member_context.py, not repo-wide; treat as UNVERIFIED system-wide.
> - Summary issue: Related link poole-1999-nominate-after-years dropped: the review mentions DW-NOMINATE but does not cite Poole and Rosenthal 1999; bonica-2013 relation changed from cites to complements (review cites Bonica 2016).

## Gaps and flags

- Staffer and revolving-door routing (checked only in relationship_uncertainty.py and member_context.py, not repo-wide) has no term for whether the anchoring member is still in office or sits on a high-stakes committee (Finance, Appropriations, Ways and Means). The review's strongest revolving-door evidence ties a connection's revenue value to exactly those facts, while the system models only time decay since overlap.
- There is no independent contact measure for lobbying. LD-2 filings never name members, and the review's contact-level evidence comes from FARA (Hirsch et al.), whose coverage excludes the domestic CU and bank clients this system tracks.
- The planned contested-bill counterparty term has no functional form. The review's main structural template (a Tullock contest between opposing coalitions) relies on equilibrium selection and shows misspecification symptoms.
- The system has no informational channel. Every money and lobbying lever is quid pro quo or pressure, while the review treats the informational view (cheap talk, legislative subsidy, opposing-bias senders) as one of the two main families.
- No owner-ratified mechanism checks ROI and break-even outputs for plausibility. The review shows returns computed from observable contributions are routinely implausible.
- D-104..D-108: the review does not validate the competitiveness reciprocity premise. Bombardini and Trebbi (2011) has an electoral-vote channel via persuadable voters, but it does not estimate that dollars buy more in competitive seats. Do not cite this paper for COMPET_MULT.
- Revolving-door wording rule: Blanes i Vidal's 24% figure is a revenue loss for former aides turned lobbyists. Any use in staffer routing must avoid influence or access language, and under D-025 it must not be treated as an introduction probability.
- D-021: a Kang-style contest weighted by LDA spend would use LDA as a weight source. A contested-bills extension built that way needs an owner ruling first.
- Kang's descriptive enactment rates (1% with no lobbying vs 24% with two or more lobbying players) carry selection bias and are not a lobbying effect. The review's own structural estimate is 0.021%-0.415%.
- The paper does not argue for either rejected SNA proposal (dropping regular cosponsorship edges; per-edge volume normalization). Nothing to flag on those.

Paul recorded no takeaway for this paper; both annotation entries are empty. Measured against his positions on other components, the review backs the system's conservative stance over his ROI enthusiasm. It shows that contribution-based returns are routinely implausible, which is consistent with his calling the Alexander figure an outlier, and it supports keeping money a minority channel in the flip model. It also reports the Huneeus and Kim committee-assignment instrument he cites for the committee pillar. The review does not mention competitive-seat effects, so it gives no support to his Cook PVI responsiveness reading. His notes predate the persistence-based lobbying nowcast and the never-summed in-house/outside rule. The review supports both, so nothing in his notes needs revising on those points.

## Related papers

- [[huneeus-2018-effects-firms-lobbying]] — cites — The review cites Huneeus and Kim (2019 manuscript version of this paper): 22% productivity loss from lobbying, 59/41 misallocation-vs-entry split, committee-assignment identification, and a caution that uniqueness of the minimizing parameter vector is not assured.
- [[kim-2018-lobbyview-firm-level]] — cites — The review cites Kim (2018) LobbyView as the state-of-the-art Compustat-linked lobbying data set used by Huneeus and Kim.
- [[alexander-2009-lobbying-analysis-under]] — complements — The review's warning that returns computed from observable political spending are routinely implausible applies to Alexander's ROI estimate.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Both separate lobbying from PAC money; the review puts total lobbying at $3-3.5bn a year against $200-250m in yearly PAC donations.
- [[kalla-nd-campaign-contributions-facilitate]] — complements — The review's account of money as access with small measured policy effects is consistent with Kalla and Broockman's evidence that contributions buy access.
- [[kim-2025-measuring-interest-group]] — complements — Kang's mapping of lobbying to provision-level policies required extensive NLP preanalysis; position extraction from lobbying reports addresses the same problem.
- [[holyoke-2015-learnable-skills-unteachable]] — complements — The review's access-versus-expertise evidence (Bertrand et al. 2014) bears on which lobbying skills are teachable.
- [[bonica-2013-mapping-ideological-marketplace]] — complements — The review uses DIME campaign-finance ideology scores, citing Bonica (2016) DIME database rather than this 2013 paper; same scoring lineage.
- [[kim-2020-strategic-sequential-links]] — complements — Both treat donations and lobbying as linked instruments; the review stresses multiple influence tools used simultaneously with substitution patterns (Bombardini and Trebbi 2011).
- [[bickmore-2025-corporate-lobbying-political]] — complements — Both relate firm lobbying to firm size, the stylized fact the review reports from Huneeus and Kim.
- [[furnas-2019-partisan-ties-lobbying]] — complements — Partisan ties of lobbyists extend the review's evidence that lobbyists' value rests on connections to particular politicians.

### Linked from
- [[bacik-2025-measuring-dynamical-evolution]] — cited-by — Bacik cites it (ref 1) as the survey of empirical lobbying models that the descriptive network work sits beside.
- [[furnas-2019-partisan-ties-lobbying]] — complements — The review discusses Bertrand et al. (2014) on connections versus expertise, which this paper extends from individual lobbyists to firms.
- [[alexander-2009-lobbying-analysis-under]] — complements — Item exists and its summary is a review of empirical lobbying designs, including identification of policy supply and demand. Relevant to the identification gap; confirmed from summary.
- [[defigueiredo-2013-advancing-empirical-research]] — extended-by — Bombardini and Trebbi (2020, Annual Review of Economics) reviews empirical lobbying models and post-dates the essay. The item exists; the extends relation is loose.
- [[kerr-2014-dynamics-firm-lobbying]] — complements — Item title matches the Bombardini and Trebbi 2020 survey of empirical lobbying models. Kerr does not cite it; its coverage of fixed-cost and participation models is unverified from the item file.