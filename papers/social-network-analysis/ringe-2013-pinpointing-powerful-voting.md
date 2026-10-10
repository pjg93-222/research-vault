---
type: paper
title: "Keeping Your Friends Close and Your Enemies Closer? Information Networks in Legislative Politics"
authors: ["Nils Ringe", "Jennifer Nicoll Victor", "Justin H. Gross"]
year: "2013"
venue: "British Journal of Political Science, vol. 43, no. 3 (July 2013), pp. 601-628; first published online 24 October 2012"
doi: "10.1017/S0007123412000518"
category: "Social Network Analysis"
systems: ["melange-influence-engine", "melange-sna", "melange-stance"]
components: ["melange-influence-engine.cascade-kernel", "melange-influence-engine.lt-priming", "melange-stance.stance-dynamics", "melange-sna.co-appearance-and-references", "melange-sna.cosponsor-network"]
paul_annotated: true
paul_markup_count: 31
evidence_status: verified
source: "[[_sources/ringe-2013-pinpointing-powerful-voting.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\BJPS-Friends-Enemies.pdf"]
sha12: ["e98be213beff"]
tags: [paper, social-network-analysis, type/paper, theme/social-network-analysis, evidence/verified]
---

# Keeping Your Friends Close and Your Enemies Closer? Information Networks in Legislative Politics

**Nils Ringe, Jennifer Nicoll Victor, Justin H. Gross (2013).** *British Journal of Political Science, vol. 43, no. 3 (July 2013), pp. 601-628; first published online 24 October 2012.* [10.1017/S0007123412000518](https://doi.org/10.1017/S0007123412000518) Theme: [[themes/social-network-analysis|Social Network Analysis]].

> [!abstract] Summary
> Ringe, Victor and Gross (BJPS 2013) ask whether legislators' social ties with allies and adversaries relate to how they vote. Their argument is that legislators treat colleagues as sources of cues. A tie to an adversary is useful when the two disagree, because a predictable opponent gives a reliable signal for confirming or revising one's own position.
> 
> The data come from staff-reported contacts among the 65 offices on the European Parliament's Environment Committee. The survey ran in 2007 and 32 of the 65 offices responded (47%). Votes are 40 final roll calls on committee bills from 2004 to 2008. The authors model the dyadic co-voting rate with OLS, with a linear mixed model that has crossed random effects for respondent and alter, and with a log-odds mixed model.
> 
> For pairs from opposing voting blocs, each extra unit of point connectivity goes with about 10% lower odds of voting together (95% CI 7-13%; n = 1,319 non-unanimous dyads). On the probability scale, predicted co-voting moves from 52% to 44% across ±1 SD of connectivity. Bloc membership dominates (log-odds 2.91), and party adds nothing once bloc is controlled. The allies hypothesis (H2) could not be tested, because 574 of 594 same-bloc dyads (97%) always voted together.
> 
> The design is cross-sectional and covers one committee in one policy area, with no identification strategy. The authors report that results are sensitive to specification.
> 
> The paper matters here for three reasons:
> 1. Several of Paul's components read a tie, or a cross-party tie, as a positive conduit for alignment. This paper is evidence that contact between opponents is associated with disagreement, not agreement.
> 2. It supports conditioning network signals on bloc or party, and keeping edge sign. It does not support pooling ties without sign.
> 3. It shows that dyadic tests need explicit modelling of network dependence, which none of the system's dyad-level checks has.
> 
> Every effect here is an association in one EP committee, measured from reported staff contact rather than cosponsorship. It transfers to the US House cosponsorship graph only as a direction to test, not as a calibrated number.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Paul's markup in the PDF

- p2 Highlight (green, 2 line rects): "Paraphrase: abstract states the aim to test how ties to allies and adversaries shape floor voting, using social network analysis and a multilevel model that handles network dependence." — 
- p3 Highlight (yellow, 4 line rects): "Paraphrase: core claim that voting is interdependent, since legislators decide votes by weighing colleagues' ties and expected votes." — 
- p3 Highlight (yellow, 4 line rects): "Paraphrase: legislators benefit from ties with partners they expect to disagree with, because those partners supply cues that update beliefs." — 
- p4 Highlight (yellow, 2 line rects): "Paraphrase: a multilevel model is used to estimate the relationship between voting tendencies and social connectedness." — 
- p4 Highlight (yellow, 2 line rects): "Paraphrase: interdependence arising from network data is modelled directly, rather than assumed away as standard econometrics would do." — 
- p5 Highlight (yellow, 4 line rects): "Paraphrase: ties reflect strategic choices to maximize the utility of the information traded." — 
- p5 Highlight (yellow, 1 line rect): "Paraphrase: measuring actual staff-reported contacts (not proxies like cosponsorship) raises confidence in the measures." — 
- p6 Highlight (yellow, 3 line rects): "Paraphrase: the spatial model treats legislators as atomistic, while evidence shows they seek cues and signals from others before voting or cosponsoring." — 
- p7 Highlight (yellow, 4 line rects): "Paraphrase: research on social networks shows people tend to choose like-minded conversation partners." — 
- p7 Highlight (yellow, 3 line rects): "Paraphrase: the authors suspect legislators' networks include political friends and political enemies, not only like-minded ties." — 
- p7 Highlight (yellow, 3 line rects): "Paraphrase: relying only on individually aligned information would put a legislator at a strategic disadvantage." — 
- p8 Highlight (yellow, 2 line rects): "Paraphrase: the core question is the utility of exchanging cues with political allies as well as political adversaries." — 
- p8 Highlight (yellow, 3 line rects): "Paraphrase: an adversary's unexpected support for the alternative may prompt a legislator to reconsider its own position." — 
- p9 Highlight (yellow, 3 line rects): "Paraphrase: within one interaction adversaries may misreport positions, but in an iterative relationship cheating is costly because trustworthiness is at stake." — 
- p10 Highlight (yellow, 2 line rects): "Paraphrase: cues from a predictable source carry more informational utility, whether from allies or adversaries." — 
- p10 Highlight (yellow, 3 line rects): "Paraphrase: voting agreement depends on two characteristics of relationships: how connected legislators are, and whether they expect to agree." — 
- p13 Highlight (yellow, multiple line rects): "Paraphrase: legislative staff are treated as extensions of their legislators, so staff networks are a proxy for office networks." — 
- p13 Highlight (yellow, multiple line rects): "Paraphrase: DeGregorio's work on US staff argues staffers are influence extenders of their bosses rather than entrepreneurs." — 
- p13 Highlight (yellow, multiple line rects): "Paraphrase: Whiteman describes congressional offices as networks in which staff and members exchange specialized information." — 
- p13 Highlight (yellow, multiple line rects): "Paraphrase: staffers use networks to gather information, build coalitions and affect legislation." — 
- p26 Highlight (magenta, 2 line rects): "Paraphrase: the main finding is that the more closely connected a pair of legislators from opposing blocs is, the less often they vote together." — 
- p26 Highlight (yellow, 3 line rects): "Paraphrase: the result confirms Hypothesis 1, the counterintuitive claim that socially connected MEPs from opposing blocs vote together less often." — 
- p26 Highlight (yellow, 3 line rects): "Paraphrase: Hypothesis 2 cannot be evaluated because same-coalition members show little voting variation beyond coalition membership." — 
- p26 Highlight (yellow, 2 line rects): "Paraphrase: joint nationality and seniority difference are not significant in any model, and party membership adds nothing once voting bloc is controlled." — 
- p27 Highlight (yellow, 2 line rects): "Paraphrase: a one-unit rise in point connectivity cuts the odds of co-voting for non-bloc pairs by about 10%." — 
- p27 Highlight (yellow, 3 line rects): "Paraphrase: without social-contact information, opposing-bloc members are expected to vote together about half the time." — 
- p28 Highlight (yellow, 2 line rects): "Paraphrase: the authors estimated alternative specifications to tease out causality, and they note the small N makes results sensitive to specification changes." — 
- p28 Highlight (yellow, 5 line rects): "Paraphrase: only a network approach with principled modelling (including mixed effects) can operationalize the propositions and test the evidence." — 
- p29 Highlight (green, 4 line rects): "Paraphrase: expectation that ties predict agreement with ideological allies and disagreement with ideological opponents (the opposed-side prediction is what the data confirm)." — 
- p29 Highlight (yellow, 3 line rects): "Paraphrase: the relationship between voting and social connection is conditional on connectivity and ideology." — 
- p29 Highlight (yellow, 3 line rects): "Paraphrase: a mixed-model approach with random effects for each member of each dyad corrects for dependence between observations." — 

## Study

**Question.** Do legislators' social ties with political allies and political adversaries affect how they vote? Specifically, does more social connectedness predict more voting agreement between allies, and less agreement between adversaries (the "keep your friends close and enemies closer" logic)? (pp. 601-603, 609-610)

**Theory.** Legislators use social contacts as information sources. Ties to allies are valuable when the two agree, and ties to adversaries are valuable when they disagree, because a predictable source of cues helps a legislator confirm or revise a policy position. Legislators therefore form expectations about whether a colleague will agree, and the link between connectedness and voting depends on those expectations (ideology and party are the basis for the expectations). Hypothesis 1: the more socially connected two political adversaries are, the less likely they are to vote the same way. Hypothesis 2: the more socially connected two political allies are, the more likely they are to vote the same way. The authors state the causal direction is simultaneous and endogenous (social ties can drive co-voting, and anticipated co-voting can drive ties) and limit claims to symmetric association (pp. 605-610, 620).

**Data.** Population of interest: legislative staffers (assistants) of MEPs on the European Parliament's Committee on Environment, Public Health and Food Safety, 2004-2009 term. Network data: web questionnaire (8 responses) and interviews (24, June-July 2007) with staff of 32 of 65 committee members' offices (47% response; 3 members from Bulgaria and Romania were excluded because they joined in January 2007). Full dyadic census of the committee: 65 offices, 4,160 directed or 2,080 symmetric dyads. Dyadic response was 46% for directed reports (1,920 of 4,160) and 71.4% when reports on non-respondents' offices are used. Density was 5.7% on the respondent-alter subnetwork (9.7% if symmetrized) and 6.1% (118 of 1,920 possible ties) on the full asymmetric subnetwork. Voting data: 40 final plenary roll-call votes on Environment Committee bills plus amendments, July 2004 to July 2008 (sixth term). Analysed unit: dyad (pair of MEPs). Observations: 1,920 dyads with network information (1,912 in the OLS and linear models; 1,319 in the log-odds model after dropping 0% and 100% co-voting). Ideology: NOMINATE scores from Hix and Noury (sixth EP). Sample description and missingness tests: Tables 1-2.

**Method.** Dependent variable is the co-voting rate per dyad (share of roll calls where both voted yes or both no). Model 1: OLS on all observations, presented as misspecified for comparison. Model 2: linear mixed-effects model (lmer, REML) with crossed random effects for respondent and alter, using all observations. Model 3: logit-transformed co-voting rate (log-odds) with random effects for respondent and alter, fitted with lmer and a Laplace approximation, dropping unanimous (0% and 100%) dyads. Predictors: same party, same voting bloc (GUE/NGL plus Greens/EFA vs PES, ALDE, UEN and EPP-ED, inferred from voting; four members belong to neither bloc), same nationality, absolute difference in seniority, point connectivity, and point connectivity x same bloc. Robustness: the authors estimated alternative specifications (lagged votes, joint-membership instrument, reverse-causality models) and report that results are sensitive to specification changes (fn. 81, p. 627); the chosen specification is defended as most theoretically consistent.

**Identification.** No causal identification strategy. The authors acknowledge a simultaneity or reverse-causality problem (pp. 610, 620, 627). They address (a) non-independence of dyads with crossed random effects for respondent and alter (pp. 623-624), (b) reverse causation partly by controlling for expected co-voting through voting-bloc membership (p. 620), and (c) the ordinal/bounded outcome with a log-odds transform (p. 623). They assume non-respondents are missing at random, and they do not test this beyond comparing response by region (Table 1, chi-square 0.935, p = 0.817) and EU accession date (Table 2, chi-square 0.3752, p = 0.540) (pp. 614-616).

**Scope.** Population: MEPs on one committee (Environment, Public Health and Food Safety) in the European Parliament, 2004-2009, and their staff networks. Policy area: environment and consumer protection (p. 613). Era: sixth parliamentary term (2004-2008 votes; 2007 survey). The authors frame the theory generally for legislatures (US and EP examples), but the data cover one committee in one policy area with 65 offices, so they caution about generalization (pp. 613, 627-628).

## Key findings

- Hypothesis 1 supported: higher point connectivity between opposing-bloc MEPs predicts less frequent co-voting, and the coefficient is negative and distinguishable from zero in all three specifications, controlling for party and bloc. (Point connectivity coefficient: OLS -0.0716 (SE 0.0043); linear multilevel -0.0234 (SE 0.0051); log-odds multilevel -0.1020 (SE 0.0307).; p. 625 (Results); Table 3, p. 626)
- Log-odds effect size: a one-unit increase in point connectivity lowers the odds of co-voting by about 10 per cent for non-coalition pairs. (Drop of 10% in odds (95% CI 7-13%). On the probability scale, moving from -1 SD (0.6) to +1 SD (3.8) connectivity lowers predicted co-voting for non-coalition pairs from 52% to 44%. A one-unit increase around the median (2) lowers it from 50% to 47%.; p. 626)
- Baseline: with no social information, opposing-bloc members are predicted to vote together about half the time, which is the empirical mean for such dyads. (Predicted co-voting about 50% at the empirical mean.; p. 626; Fig. 7, p. 627)
- Same-bloc membership strongly predicts co-voting; joint party membership, nationality and seniority difference do not add explanatory power once bloc is controlled. (Joint bloc membership: OLS 0.3552 (SE 0.0147); linear 0.5590 (SE 0.0135); log-odds 2.9103 (SE 0.0985), i.e., odds roughly 300% higher. Party: 0.0164 / 0.0063 / -0.0953 (none significant). Nationality and seniority: not significant.; p. 625; Table 3, p. 626)
- Hypothesis 2 (allies co-vote more with more connection) cannot be tested with these data because there is almost no variation among same-coalition dyads. (Of 594 same-coalition dyads, 574 (97%) voted together 100% of the time on the forty Environment Committee bills.; p. 625)
- Co-voting distribution is bimodal: about one-third of all pairs vote together 100% of the time, and the authors therefore drop unanimous observations in the log-odds model. (1,920 observations; 594 pairs at 100% co-voting; only five pairs at 0%.; p. 618 (Fig. 3); p. 623)
- Connectivity structure: contact is sparse and many pairs are poorly connected, though the measure has a meaningful spread. (Density 5.7% (9.7% symmetric) in the both-respondent subnetwork; 6.1% (118 of 1,920 possible directed ties) in the asymmetric all-respondent subnetwork. Mean point connectivity 2.2, median 2, maximum 12 (one Spanish EPP and British Conservative pair). 12.4% of dyads have zero connectivity. 39.5% of office pairs are cut off by removing one node.; pp. 617, 620-621 (Fig. 5))
- Response bias tests show no evident bias by region or EU accession date, but the authors stress that network data require caution below 100% response. (Region: Pearson chi-square 0.935, p = 0.817 (N = 65). Accession date: chi-square 0.3752, p = 0.540. Response 32 of 65 offices (47%).; pp. 614-616 (Tables 1-2, Fig. 2))
- Theory and method contribution: the authors argue voting interdependence must be modelled at the dyad level with network dependence handled explicitly; the conditional relationship with ideology is central. (; pp. 601-603, 628)

## Limitations

- Small network: 32 of 65 offices responded (47%), and the authors say non-response is assumed missing at random (p. 614). A 47% response rate means the network needs caution, and the authors say results below 100% response are less certain (p. 616).
- The data are restricted to one committee and one policy area (environment and consumer protection) for one term (p. 613). The authors say generalization is limited (p. 627).
- Causality is not resolved: the authors describe a simultaneous, endogenous process between ties and co-voting (pp. 610, 620, 627) and say their empirical tests are compromised by an intractable causality problem (p. 628).
- Results are sensitive to specification changes (lagged votes, joint-membership instrument, reverse-causality models) (fn. 81, p. 627). The authors defend the chosen specification but do not show the alternatives.
- Hypothesis 2 is not testable: nearly all same-coalition dyads vote together, leaving little variance (p. 625).
- Relational measures: connectivity is based on reported contact, which is directed and then symmetrized (p. 617); the connectivity measure is sensitive to specification (fn. 72, p. 620; max-flow alternatives were tried).
- The data collection is hard for social network data because staff are reluctant to reveal contacts; the authors say the survey was very specialized and precluded immediate data additions (p. 627).
- Missing data and lack of variance on some key terms limit inference (p. 628). Data cannot be shared publicly because of the sensitivity of personal connections (p. 602, fn. on first page).

## Short quotes

- "legislators' voting behaviour is highly interdependent" (p. 602)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Cascade activation kernel build_P and partisan edge factors|Cascade activation kernel build_P and partisan edge factors]] | qualifies | Among opposing-bloc MEP dyads, higher point connectivity predicts less co-voting. Table 3 model 3 log-odds multilevel coefficient is -0.1020 (SE 0.0307), about 10 percent lower odds per unit (95 percent CI 7-13 percent). Predicted co-voting falls from 52 to 44 percent between connectivity 0.6 and 3.8 (about -1 to +1 SD). Window: 40 Environment Committee roll calls, July 2004 to July 2008. Denominator: 1,319 non-unanimous dyads (perfect co-voters excluded in model 3). (pp. 625-627; Table 3 p. 626; Fig. 7 p. 627) | cpf_diffusion gives every cross-party edge between D and R members a contagion weight of 0.4 + 0.6*sqrt(bpf_u*bpf_v), so non-bridge out-party pairs keep a floor of 0.4. build_P maps the weight to activation probability x/(x+6) with x = w*cpf*edge_mult. (`fec/moneyball_core.py:543-546 (cpf_diffusion); fec/moneyball_core.py:555-560 (build_P)`) | Paper tie is staff-reported contact, not cosponsorship. Setting is a multi-party EP committee, not two-party US Congress. Result is cross-sectional association with acknowledged simultaneity and specification sensitivity (fn. 81). It does not show cross-party diffusion is zero or negative in US cosponsorship. Use it to motivate a test stratified by party pair, not to change a constant. | weak | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Linear-threshold priming (cheapest tip)|Linear-threshold priming (cheapest tip)]] | supports | Social connection between political adversaries goes with less voting agreement, not more. Theory predicts this when the pair expects to disagree (H1, Fig. 1). Opposed-bloc baseline co-voting is about 50 percent (p. 626). (pp. 609 (H1 theory, Fig. 1); pp. 625-626 (results)) | The BAKKER_XCUE signing passes a cross-party pro-cue through sqrt(bpf_u*bpf_v), so cues from non-bridge out-partisans fall to about 0. The value is never negative because it is a square root of products of non-negative bridge-factor values. Priming pressure does not treat out-party ties as persuasive. (`fec/engine/lt_priming.py:56-64 (comment block 56-62; xf at line 64)`) | Direction agrees: out-party ties should not count as pro-pressure. The association is not causal and is EP staff contact, so it does not justify adding a negative backfire term. Keeping the floor at 0 remains the default. | moderate | VERIFIED |
| [[systems/melange-stance#Stance dynamics and neighbour imputation|Stance dynamics and neighbour imputation]] | qualifies | The connectivity to agreement link depends on whether the pair are expected allies or adversaries (bloc). Same-bloc dyads co-vote in 574 of 594 cases (about 97 percent), so H2 is untestable. For opposed-bloc pairs more contact predicts less co-voting (p. 625; Table 3). (pp. 609-610 (H1 and H2 theory); p. 625 (574 of 594); p. 628 (conclusion)) | Neighbour imputation takes the unweighted mean of TSCORE over every cosponsor neighbour, with at least 5 neighbours (SD_MINNB) and absolute estimate at least 0.75 (SD_CONF). It does not condition on party. Masking validation reports sign agreement but has no party-only baseline. The co-movement test on cosponsor and HCoI ties is reported as null. (`fec/stance/stance_dynamics.py:132-146 (imputation); fec/stance/stance_dynamics.py:94-110 (comovement)`) | The paper outcome is votes, not stance labels, so transfer is conceptual. The next diagnostic is a party-only baseline and a split of same-party from cross-party neighbours. The 77 percent figure stays UNVERIFIED as a network effect until that baseline exists (DE-062). | moderate | VERIFIED |
| [[systems/melange-sna#Co-appearance and signed member-reference layers (display only)|Co-appearance and signed member-reference layers (display only)]] | qualifies | Unsigned contact is not alignment. Between adversaries more contact goes with less agreement (about -10 percent odds of co-voting per connectivity unit, opposed-bloc dyads, EP Environment Committee 2004-2008). Authors identify ally or adversary status from voting bloc and party membership, not from the contact itself (p. 617). (pp. 605-610; pp. 617, 625-626) | The co-appearance layer records unsigned, undirected pairs with a bipartisan flag. data/network/co_appearance.json holds 2,481 pairs of which 1,182 are bipartisan. The member-reference layer keeps net = (pos - neg)/scored per dyad. Both layers are display-only. (`fec/network/co_appearance_layer.py:61 (bipartisan flag); fec/members/member_reference_layer.py:368-372 (net formula)`) | Supports keeping the reference layer signed and warns against reading cross-party co-appearance as closeness or alignment (D-025). A broadcast pairing of opponents may be adversarial. The settings differ (EP staff contact vs US media co-appearance), so the evidence is weak. | weak | VERIFIED-SOFTENED |
| [[systems/melange-sna#Cosponsorship network construction|Cosponsorship network construction]] | qualifies | Authors use measured staff-reported contacts rather than proxies such as cosponsorship, and argue this raises confidence in their measures. Reported contact was sparse: 6.1 percent density (118 of 1,920 possible directed ties in the asymmetric all-respondent subnetwork), mean point connectivity 2.2 (median 2), and 12.4 percent of dyads at zero connectivity. (pp. 603-604; p. 617 (density); pp. 620-621 (connectivity, Fig. 5)) | The cosponsor graph is the default substrate for cascades, routes and imputation. load_network reads the newest augmented congress_cosponsor workbook and mean-symmetrizes it for the cu config. Original-cosponsor weight 1.0 appears in legislation workbook code. No regular-cosponsor weight of 0.5 was found for the cu base graph; the only regular dial found is 0.2 in the holistic base builder. (`fec/moneyball_core.py:461-507 (load_network); fec/moneyball_core.py:392 (NETWORKS cu symmetrize True); fec/network/build_holistic_base.py:14 (regular dial 0.2)`) | The paper does not test cosponsorship, so it cannot show cosponsor edges are wrong. It shows measured contact and agreement can diverge for adversaries, which bears on the alignment assumption for cross-party cosponsor edges. It does not argue for dropping regular cosponsor edges or for per-edge volume normalization; both were measured and rejected in this project. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Bipartisan co-appearance count 1,151 is wrong: data/network/co_appearance.json has 2,481 pairs of which 1,182 are bipartisan. Corrected in the co-appearance bearing.
> - Summary issue: Density line conflated subnetworks: 5.7 percent is the both-respondent subnetwork (9.7 percent symmetric); 6.1 percent with 118 of 1,920 is the asymmetric all-respondent subnetwork. Corrected in the cosponsor bearing.
> - Summary issue: Response count: the text (p. 616) reports 32 responses (24 interviews, 8 questionnaires; 47 percent). Paper Tables 1 and 2 (pp. 615-616) list 30 respondents of 65. The summary follows the text; the paper is internally inconsistent here.
> - Summary issue: Paul alignment cannot be checked: the paul_entries list in the item is empty and the PDF annotations carry no note text. Highlight topics on pp. 609 and 625-627 were seen on rendered pages; the claim that his only listed position is ink-only remains unverified.
> - Summary issue: The text file Documents/research/_text/e98be213beff.txt is 56 bytes of blank lines. Verification used page images rendered from Downloads/BJPS-Friends-Enemies.pdf with PyMuPDF into the scratchpad; the PDF has no text layer and no OCR tool is installed. Pages 2, 4-5, 10, 15-19, 21-22 and 26-28 were read.
> - Summary issue: Related link haythornthwaite-1996-social-network-analysis dropped: no Haythornthwaite citation was found in pages read; the paper cites Hanneman and Riddle for network methods (p. 620 fn. 73). The same-method link is unsupported.
> - Summary issue: Related links for ringe-nd-pinpointing, aref-2021, bakker-nd-2020, boxsteffensmeier-2018 and leskovec-2010 were checked for item existence and title only; their content-level claims were not re-verified against those papers.
> - Summary issue: Summary says 97 percent of same-bloc dyads always co-voted; the exact figure is 574 of 594 (96.6 percent). Acceptable rounding, but the denominator should be stated.
> - Summary issue: Summary cites N 1,319 without noting that Table 3 model 3 excludes perfect co-voting dyads; state the non-unanimous subset wherever the coefficient is quoted.
> - Summary issue: Original cpf_diffusion cite 540-542 was off by three lines (actual 543-546); corrected in the cascade-kernel bearing.

## Gaps and flags

- No dyad-level test in the system models network dependence, such as crossed random effects for both members of a dyad, or QAP and ERGM approaches. This applies to the co-movement tests in stance_dynamics.py, to any future test of whether negative references predict lower cosponsorship, and to cross-party diffusion checks. The paper shows that dyads sharing a member are not independent.
- No component conditions the meaning of a tie on whether the pair are expected allies or adversaries (bloc or party pair) before using it as alignment evidence. Neighbour imputation and the 0.4 cross-party diffusion floor both pool or attenuate ties without sign.
- No layer treats ties to opponents as information channels, that is, cue-taking from a predictable adversary that confirms one's own position. The system models cross-party ties only as attenuated positive contagion or as zero pressure.
- There is no external test of the cross-party sign in the cascade kernel. Neither direction of the cross-party term (positive diffusion at 0.4 + 0.6*sqrt(bpf)) has been checked against US co-voting or cosponsorship outcomes, stratified by party pair.
- The paper does not argue for either rejected SNA proposal (dropping regular cosponsorship edges; per-edge volume normalization). Its preference for measured contact over cosponsorship proxies must NOT be read as support for dropping cosponsor edges.
- The point-connectivity coefficients are heuristic association measures from one EP committee. Under D-025 they must not be restated as closeness, access or introduction probabilities, and influence language should not be used.
- The -10% odds figure is a stratum result (opposed-bloc pairs, 1,319 non-unanimous dyads, 40 roll calls 2004-2008). It must not be quoted as a pooled tie effect. The same-bloc hypothesis (H2) was untestable (574 of 594 same-bloc dyads at 100%), so the paper gives no evidence on ties among allies.
- The authors report sensitivity to specification (fn. 81) and an unresolved simultaneity problem.

Paul's highlights centre on the mechanism. Voting is interdependent, ties are strategic choices about information, and adversaries supply cues that update beliefs. He also highlighted the method (multilevel models with crossed random effects for dependence), the use of staff networks as a proxy for office networks, and the headline H1 result. The evidence matches his reading. However, the system has absorbed little of it. Cross-party ties appear only as attenuated positive contagion (cpf_diffusion) or as zero priming pressure (Bakker clamp). No layer uses adversary ties as information channels, and no dyadic test models dependence the way he highlighted. The paper's main transfer is a caution that unsigned contact is not alignment, plus support for keeping the reference layer signed and stratifying by party pair. The highlight on staff as extensions of their legislators fits the staffer-network premise. But the staff ties in this paper are reported contacts, not concurrent payroll overlap, so it does not validate the overlap edge rule.

## Related papers

- [[ringe-nd-pinpointing-powerful-voting]] — same-data — Item title confirmed as co-voting network centrality in the EP (same first author). Relation to this paper outcome is plausible. Content of that item not re-verified.
- [[aref-2021-identifying-hidden-coalitions]] — complements — Item title confirmed (hidden coalitions in the US House, 2021). The bloc-not-party and signed-tie parallel is not re-verified against that item text.
- [[bakker-nd-understanding-partisan-receptivity]] — complements — Item title confirmed (partisan cue receptivity). The out-party cue claim is not re-verified against that item text; the lt_priming clamp relies on it.
- [[boxsteffensmeier-2018-taking-congress-interest]] — complements — Item title confirmed (cue-taking in Congress, interest-group letters, 2018). Shared cue-taking framing is supported by titles only.
- [[sotoudeh-2024-network-based-measure]] — complements — Changed from contradicts. Sotoudeh builds a cosponsorship influence measure on bill passage. This paper questions cosponsorship as a social-tie proxy (p. 604) but does not test that measure, so it is a premise dispute, not a contradiction.
- [[leskovec-2010-predicting-positive-negative]] — complements — Item title confirmed (positive and negative links). The shared point that tie sign changes what a tie predicts is a framing link only; content not re-verified.

### Linked from
_None._