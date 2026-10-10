---
type: paper
title: "Advancing the Empirical Research on Lobbying"
authors: ["John M. de Figueiredo", "Brian Kelleher Richter"]
year: 2013
venue: "NBER Working Paper 19698 (December 2013)"
doi: ""
category: "Lobbying"
systems: ["melange-influence-engine", "melange-lobbying", "melange-pathfinding", "mentat-sector-growth-study", "moneyball-rank-pillars"]
components: ["melange-lobbying.lobbied-member-link", "mentat-sector-growth-study.growth-spending-regressions", "melange-influence-engine.flip-model", "melange-lobbying.contested-bills", "moneyball-rank-pillars.committee-pillar", "melange-pathfinding.staffer-overlap-routes", "melange-lobbying.fec-lda-bridge"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/defigueiredo-2013-advancing-empirical-research.pdf]]"
original_paths: []
sha12: ["dee4e24c7f60"]
tags: [paper, lobbying]
---

# Advancing the Empirical Research on Lobbying

**John M. de Figueiredo, Brian Kelleher Richter (2013).** *NBER Working Paper 19698 (December 2013).*  Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> De Figueiredo and Richter (NBER WP 19698, 2013) is a narrative review with no new estimates. It sets out what the field agreed on about lobbying by 2013 and how to study it causally. The scale facts: federal lobbying was about 3.5 billion dollars in 2012, roughly five times interest-group campaign contributions. Corporations and trade associations account for over 84 percent of it. Only about 10 percent of public firms lobby, and a firm that lobbied last year lobbies this year with 92 percent probability. Its main argument is that lobbying panels are persistent, selected and confounded by unobserved channels. Fixed effects alone therefore cannot identify effects, and research should use exogenous shocks. The designs it reviews are difference-in-differences on legislator exits, event studies such as the Jeffords switch, cost-shifter instruments, Heckman selection, and structural all-pay contest models. It cites conditional returns: earmark gains only where the home legislator sits on Appropriations, a 1.07 percentage point lower tax rate per 1 percent more lobbying, and about 23 percent of lobbyist revenue lost when a former boss leaves office. It also notes that transaction (LDA) data record spending but not which legislator was targeted. For Paul's systems the essay mostly confirms choices already made. It agrees that the lobbied-member link is circular (LD-2 names no member) and explains why the sector-growth lobbying coefficient flips under year fixed effects. It also shows that the influence engine's money legs cover only PAC contributions, the smaller channel. Its shock-based designs point to tests the system has not run. Nothing in it argues for either rejected SNA proposal. Its revolving-door evidence must not be turned into influence language on staffer edges.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** What empirical facts about lobbying are broadly agreed, what challenges block causal inference on lobbying effects, which quasi-experimental and structural methods can overcome them, and which data sources and open questions should drive the next generation of research (essay has no original estimates).

**Theory.** Informational lobbying: lobbying is the transfer of information in private meetings between interest groups and politicians; politicians maximize re-election, promotion and ideological goals (p.4). Hannan-Freeman-Carroll population ecology (legitimation and competition) for counts of interest groups (pp.8-9). Expertise versus connections (who you know vs what you know) (pp.9-10). Counteractive lobbying (Austen-Smith and Wright) versus legislative subsidy (Hall and Deardorff) for targeting (p.11). Calls for empirical testing of formal models (EITM) (p.40).

**Data.** No sample of its own. Cited studies: Blanes i Vidal et al (2012) former congressional staffers who are contract lobbyists, 22 periods (pp.18-19); de Figueiredo and Silverman (2006) university earmarks 1997-1999 (pp.22-24); Richter et al (2009) public firms, tax rates, about 10 percent lobby (pp.24-26); de Figueiredo and Cameron (2012) 38 US states (p.8); Baumgartner et al (2009) 98 issues, 2,200 advocates (p.13); Kang (2012) energy groups (p.27); Jayachandran (2006) stock returns, 2001 Jeffords switch (pp.20-21).

**Method.** Narrative review and methods primer. Surveys the empirical facts on who lobbies, how much, and how it is organized (Sec II), then reviews five designs: difference-in-differences with exogenous shocks, event studies, instrumental variables (2SLS), two-step Heckman selection models, and structural modeling (Sec III.B). Also lists matching, regression discontinuity and synthetic control as untried for lobbying (p.29). Compares data classes: surveys, registries, transaction reports (Sec IV).

**Identification.** Difference-in-differences: politician exits (retirement or defeat) as exogenous shock to lobbyist revenue, with lobbyist and party-chamber-time fixed effects. Event study: Jeffords party switch as shock to firm abnormal returns. IV: university overhead rates as cost shifter instrument for lobbying, excluded from earmark equation. Selection: probit selection equation with liquid assets as excluded variable, inverse Mills ratio added to tax equation. Structural: all-pay contest model estimated on policy votes.

**Scope.** Mostly US federal and state lobbying (LDA federal data, US state transaction data), with brief mention of Japan, China, India, Mexico, Norway and other developed or transition economies (p.35-36). Literature reviewed through 2013; federal disclosure under the Lobbying Disclosure Act of 1995 (p.34). Excludes campaign finance, illegal lobbying and bribery, and bureaucratic lobbying (footnote 1, p.4).

## Key findings

- Federal lobbying spending was about 3.5 billion dollars in 2012, against roughly 750 million a year in interest-group campaign contributions, about five times as large (Sec II.A, pp.5-6).
- Corporations and trade associations account for over 84 percent of federal lobbying expenditures; issue-ideology membership groups about 2 percent (de Figueiredo 2004 as cited, Sec II.A, p.6).
- Only about 10 percent of publicly traded firms lobby on their own behalf (Richter et al 2009; Kerr et al 2013); a firm lobbying last year lobbies this year with 92 percent probability (Sec II.A, p.7).
- Budgeting periods are associated with a 19 percent increase in state interest-group lobbying expenditures across 38 states (de Figueiredo 2014; de Figueiredo and Cameron 2012) (Sec II.A, p.8).
- Lobbyist revenue falls about 23 percent after the senator they formerly served is defeated or retires (Blanes i Vidal et al 2012) (Sec II.C, p.10); the essay attributes about 23 percent of lobbying services value to who you know (Sec III.B.1, p.20).
- Richter et al (2009): a 1 percent increase in lobbying over baseline predicts a 1.07 percentage point lower effective tax rate in the next period, and the result survives a Heckman selection correction (Sec III.B.4, p.26).
- de Figueiredo and Silverman (2006): lobbying raises earmarks only when the district legislator sits on an appropriations committee; no significant effect otherwise (Sec III.B.3, p.24).
- Kang (2012) structural all-pay contest: expenditure effects on enactment are very small, but the marginal return in policy is about 140 percent (Sec III.B.5, p.28).
- Jayachandran (2006): firms more aligned with the former Republican majority lost market value after the Jeffords switch (Sec III.B.2, p.21).
- Baumgartner et al (2009): across 98 issues, both sides mobilize roughly equal resources; policies are stable and change, when it occurs, is substantial (Sec II.D, p.13).
- Targeting: powerful, allied and marginal legislators are lobbied; staunch enemies are not. Counteractive and subsidy accounts give different predictions (Sec II.C, pp.10-11).
- State interest-group counts follow an inverted-U over time from legitimation then competition, and rise with economy size and number of issues (Sec II.B, pp.8-9).

## Limitations

- Essay itself reports no new estimates; its consensus claims are a synthesis of cited work (p.2-3, Sec II).
- Lobbying panels show persistence and little within-group variation, so fixed effects need external shocks (Sec III.A, p.16).
- Omitted variables are pervasive because some instruments of influence are unobserved (p.16-17).
- Endogenous selection into lobbying is not random; about 10 percent of interests lobby, so returns may be biased upward (p.17).
- Difference-in-differences can have inconsistent standard errors; event windows can be confounded; weak instruments and small selection samples can doom IV and selection models; structural assumptions may be unbelievable (footnote 3, p.28).
- Survey data suffers from non-response, non-random samples, recall error and little verifiability (p.32).
- Registries count groups, include zero-spend registrants, and mix rules across jurisdictions (p.33).
- Transaction data gives spending and intensity but not message content and not which legislator was targeted (pp.34-35).
- Outcome variables are often weakly linked to lobbying; authors call for institutional detail on the link (p.30).

## Short quotes

- "lobbying is defined as the transfer of information in private meetings and venues between interest groups and politicians"
- "lobbying expenditures representing five times the dollar volume of interest group campaign contributions"
- "they almost all share a quasi-experimental design structure that has advantages over more common empirical designs"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-lobbying#Lobbying member-year network and circular lobbied-member link\|Lobbying member-year network and circular lobbied-member link]] | supports | Transaction (LDA) reports show lobbying intensity but not message content, and the US federal transaction data does not say which legislator is the target, so researchers cannot map lobbying efforts to individual legislators. (Sec IV.A.3 pp.34-35) | lobby_interactions.py docstring ON POSITION paragraph: LD-2 has no position field, so position ships as not_disclosed. Brain lobbyview-collector.md circular-link section: the member link is manufactured by joining lobbied bills to sponsor and cosponsor rosters, so P(joined given lobbied) = 1 by construction. `fec/lobbying/lobby_interactions.py docstring ON POSITION paragraph; C:/Users/pgoff/Documents/brain/actors/lobbyview-collector.md:139-146` | Holds for any federal LD-2 vintage because the form structure, not the sample, creates the gap. A member-level contact measure would need an independent source. | strong | VERIFIED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | supports | Kerr, Lincoln and Mishra as cited: a firm that lobbied last year lobbies this year with 92 percent probability (firm-level, federal). Within-group variation in lobbying is little, so fixed-effects designs need external shocks; omitted channels and endogenous selection into lobbying bias naive estimates. (Sec II.A p.7; Sec III.A pp.16-17) | Brain topic note: L3-LV coefficient +0.352 becomes -0.477 with sector and year fixed effects; growth adds no adjusted R2 (0.884717 vs 0.884690); first-difference with year effects -0.537 over 71 pairs. `C:/Users/pgoff/Documents/brain/topics/2026-09-feature-sector-growth-study.md:235,239; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_forecast.R:405 (LobbyView backtest call; the coefficient estimation code was not at this line)` | The 92 percent is a firm-level federal figure, not a sector-year figure. The essay gives no sign for any sector coefficient, so the sign reversal is a fixed-effects fragility the essay anticipates, not validation of either sign. Figures must name window and denominator. | moderate | VERIFIED |
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | suggests-extension | Lobbying spending was about 3.5 billion dollars in 2012, roughly five times the approximately 750 million a year in interest-group campaign contributions (pp.5-6). Kang (2012) finds expenditure effects on enactment are very small but the marginal policy return is about 140 percent (p.28), and the value of lobbying depends on timing and targets (p.28). Selection into lobbying (about 10 percent of firms) biases naive return estimates upward only if non-lobbying interests expect no return (p.17). (Sec II.A pp.5-7; Sec III.A p.17; Sec III.B.5 pp.27-28) | flip_model.py takes cu and banks plus finserv PAC money (cu0, ind0) through a saturating receipts-relative term. Lobbying spend does not enter the per-member legs: a case-insensitive grep of flip_model.py for lobby returns no hits. `fec/engine/flip_model.py:4-7,39; fec/engine/money_response.py:16-31,69-75` | Lobbying cannot enter a per-member leg directly, because LD-2 names no member and LDA is an identity source, not an edge or weight source (D-021). Pooled return figures must carry an UNSTRATIFIED label. Kang's 140 percent is a policy return in a structural model, not a dollar ROI. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Contested-bills registry\|Contested-bills registry]] | method-source | Kang (2012) builds an all-pay contest model of counteractive lobbying, estimated on energy groups' support and opposition for policies in 110th Congress bills, and finds the expenditure effect on enactment is very small. Baumgartner et al. (98 issues) find both sides mobilize roughly equal resources and policy is usually stable. (Sec III.B.5 pp.27-28; Sec II.D p.13) | contested_bills.py docstring: per-bill registry ratified by Paul 2026-07-31; contested records supply a per-bill explicit counterparty set; standard mode is the default; advisory records are display-only; CONTESTED_V1 is a future dial and no consumer exists today. `fec/lobbying/contested_bills.py:2-15` | An all-pay contest needs bill-level spending by both sides. LDA issue codes and bill mentions may supply that at the bill level, not the member level. The equal-mobilization finding warns that relative spend may predict little. Admission must stay with Paul's ratification. | moderate | VERIFIED |
| [[systems/moneyball-rank-pillars#Committee - Influence pillar\|Committee - Influence pillar]] | supports | De Figueiredo and Silverman (2006), academic earmarks 1997-1999: lobbying raises earmarks when the district legislator sits on an appropriations committee and has no statistically significant independent effect when not (pp.22-24). Powerful, allied and marginal legislators are the ones lobbied (pp.10-11). (Sec III.B.3 pp.22-24; Sec II.C pp.10-11) | committee_scores.py weights seats by CU-relevance tier (Highly 3.0, Moderately 1.8, Slightly 0.9) with Chair and Ranking multipliers and a subcommittee ratio of 1/6. `fec/engine/committee_scores.py:125-127 (BASE tiers); same file SUBF and KLEAD constants` | The evidence is from academic earmarks on appropriations and supports the conditioning logic only. It does not validate the tier weights or the 1/6 subcommittee ratio, which remain owner judgment. | weak | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Ordered person-overlap routes and employment-routing overlay\|Ordered person-overlap routes and employment-routing overlay]] | qualifies | Blanes i Vidal, Draca and Fons-Rosen (2012): lobbyist revenue of former Senate staffers falls about 23 percent when the senator they served is defeated or retires, in a 22-period panel with lobbyist and party-chamber-time fixed effects (pp.18-20). The essay reads this as the value of who you know (p.20). (Sec II.C p.10; Sec III.B.1 pp.18-20) | Staffer route cost uses shared-work and recency terms with a member-context modifier MEMBER_BETA 0.15. employment_history.py derives a revolving-door sidecar. Route cost is a heuristic model output, not closeness or access (D-025). `fec/staffer_network/closeness_math.py:21; fec/staffer_network/employment_history.py:2083-2092 (sidecar derivation function)` | The essay's evidence is a revenue effect for lobbyists, not a route-cost or access measure. Influence language stays barred on revolving-door edges. | weak | VERIFIED |
| [[systems/melange-lobbying#FEC to LDA bridge test\|FEC to LDA bridge test]] | qualifies | Money is not transferred explicitly to politicians as campaign contributions are, so lobbying is a distinct channel (p.4). Lobbying expenditures are about five times interest-group campaign contributions (pp.5-6). The essay does not address whether campaign giving leads lobbying. (Sec I p.4 (channel definition); Sec II.A pp.5-6 (scale)) | Bridge round-3 look: raw rho +0.153 member-blocked p=.0146, but four-control partial -0.0002, tier UNCORROBORATED. The 42-month FEC lead is recorded as intact in the collector note and is a candidate heuristic only. `C:/Users/pgoff/Documents/brain/actors/lobbyview-collector.md:137 (bridge result) and :124 (42-month lead); the original cite at :84 is a different bullet` | The paper gives no evidence that giving leads lobbying. A lead test would need an exogenous-shock design rather than more controls. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Says the essay agrees the lobbied-member link is circular. The essay never argues or uses that idea; it only says disclosure rules do not require identifying targeted legislators (pp.11, 34-35). Circularity is the system's own finding in the brain.
> - Summary issue: Says the essay explains why the sector-growth lobbying coefficient flips under year fixed effects. The essay does not discuss that regression; it only says persistent panels need external shocks (pp.16-17).
> - Summary issue: Says the essay shows the influence engine's money legs cover only PAC contributions. The essay says nothing about the engine; that is a fact about the code, not an essay finding.
> - Summary issue: Says fixed effects alone cannot identify effects. The essay says they need exogenous shocks, a softer claim than cannot identify.
> - Summary issue: Says lobbyist revenue is lost when a former boss leaves office. The essay's design uses defeat or retirement of the senator served (pp.10, 18-19).
> - Summary issue: Says selection makes return estimates biased upward without condition. The essay conditions this on non-lobbying interests expecting no return (p.17).
> - Summary issue: Says the 1.07 point tax effect is per 1 percent more lobbying. The essay says per 1 percent over the firm's baseline, predicted in the next period (p.26).
> - Summary issue: Says the essay mostly confirms choices already made in Paul's systems. That is an analyst judgment beyond the text, not an essay finding.

## Gaps and flags

- The influence engine's money legs (flip model and money response) model only PAC contributions, the smaller of the two money channels; the system states no explicit position on how sector lobbying, about five times larger, relates to member winnability.
- No lobbying-return number used anywhere in the system is identified by the shock-based designs the essay recommends (member exits, party switches, cost shifters). Break-even and ROI floors rest on an assumed saturating form.
- Selection into lobbying (about 10 percent of firms) is not modeled in the sector-growth lobbying regressions. Sector sums over registrants mix the extensive and intensive margins.
- Legislator exits (retirement or defeat) are an available exogenous shock in the staffer and member data and could support a difference-in-differences test of route or engagement value. This has not been designed.
- The break-even and ROI component's recorded code_ref (fec/templates/make_dashboard_html_00.tpl) does not exist at that path in the live tree, so the component card is stale.

- Revolving-door evidence (23 percent revenue loss) is phrased in the paper as the value of who you know; it must not become influence or access language on staffer or revolving-door edges (brain rule; D-025).
- Cited return estimates (Richter et al 1.07 percentage points per 1 percent lobbying; Kang 140 percent policy return) are pooled over firms and sectors. Quote them only with an UNSTRATIFIED label, and do not use them as dollar-ROI calibration.
- The essay's call to link transaction data to targeted legislators would, if done through LDA bill rosters, recreate the circular lobbied-member link and conflict with D-021 (LDA is an identity source, not an edge or weight source).
- The essay does not argue for dropping regular cosponsorship edges or for per-edge volume normalization; no rejected SNA proposal is involved.
- The essay is a 2013 working paper, not peer reviewed, and reports no original estimates; every number is a secondary citation.

Paul left no annotation on this work (0 notes). By component, Paul frames lobbying as ROI, expects donations to lead lobbying, and read Kim and Kunisky as a way to identify champion members. The essay agrees with the system rather than with those notes on three points. Naive lobbying returns are biased upward by selection and persistence. Contributions and lobbying are distinct channels of very different scale, which fits the near-zero bridge partial. Disclosure data cannot say which legislator was lobbied, which fits the circular-link ruling. His ROI enthusiasm and his expectation of a donation lead are therefore stale relative to the system now. His wish for explicit inter-industry conflict on bills is supported by the contest-model literature the essay reviews.

## Related papers

- [[bombardini-2020-empirical-models-lobbying]] — extends — Bombardini and Trebbi (2020, Annual Review of Economics) reviews empirical lobbying models and post-dates the essay. The item exists; the extends relation is loose.
- [[kerr-2014-dynamics-firm-lobbying]] — cites — Same title as the Kerr, Lincoln and Mishra paper the essay cites as a 2013 working paper. The item is NBER WP 17577; calling it the published version was not verified.
- [[kim-2018-lobbyview-firm-level]] — complements — LobbyView firm-level lobbying and bill database. The essay calls for bill-level linkage of transaction data but does not name this source. The claim that the system uses it as its fundamental source is not from the essay.
- [[huneeus-2018-effects-firms-lobbying]] — complements — Huneeus and Kim (2018 draft) on firms' lobbying and resource misallocation. The committee-assignment instrument described in the rel was not verified against the item.
- [[alexander-2009-lobbying-analysis-under]] — complements — Lobbying ROI under the American Jobs Creation Act of 2004 (Alexander, Scholz, Mazza). A single-policy return estimate, to which the essay's selection and omitted-channel warnings apply.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Furnas, LaPira and Brock on low organizational overlap between lobbying and PAC spending. Matches the essay's point that the channels are distinct, though the essay does not cite it.
- [[imai-2021-fixed-effects-regression]] — complements — Imai and Kim (2021) on two-way fixed effects for causal inference with panels. Formalizes the essay's warning about fixed effects on persistent panels.
- [[furnas-2019-partisan-ties-lobbying]] — complements — The partisan ties of lobbying firms (Research and Politics, 2019). Connection-based evidence; the loss-of-ties-when-party-loses-control claim was not verified against the item.
- [[holyoke-2015-learnable-skills-unteachable]] — complements — Holyoke, Brown and LaPira (2015) on what can and cannot be taught in lobbying. Addresses the expertise side of the essay's expertise-versus-connections question.
- [[kim-2020-mapping-political-communities]] — complements — Kim and Kunisky on lobbying networks built from bill-level LDA links. The essay's targeting gap in transaction data applies to member edges built this way; the item's content was not re-read.

### Linked from
- [[ansolabehere-2002-little-money-politics]] — complements — A co-author's later review of lobbying empirics. The claim that lobbying spending dwarfs PAC giving was not found in its text, so only the agenda link stands. Item exists.
- [[fremeth-2018-spillovers-regulating-corporate]] — cited-by — Real item (title: Advancing the Empirical Research on Lobbying). Citation claim not re-read.