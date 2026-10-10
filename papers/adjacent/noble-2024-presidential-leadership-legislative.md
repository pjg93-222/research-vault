---
type: paper
title: "Presidential Leadership and Legislative Polarization"
authors:
  - "Benjamin S. Noble"
  - "Ian R. Turner"
year: "2024"
venue: "Leadership (online, per filename; unverified)"
doi: "10.31235/osf.io/sa9ke"
category: "Adjacent"
systems: ["melange-influence-engine"]
components: ["melange-influence-engine.salience-shock"]
paul_annotated: true
paul_markup_count: 118
evidence_status: "verified"
source: "[[_sources/noble-2024-presidential-leadership-legislative.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\NobleTurner-Leadership-Online-2-2024.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\NobleTurner-Leadership-Online-2-2024.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\NobleTurner-Leadership-Online-2-2024.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\NobleTurner-Leadership-Online-2-2024.pdf", "C:\\Users\\pgoff\\Downloads\\NobleTurner-Leadership-Online-2-2024.pdf"]
sha12: ["8de7b6c92e80"]
tags: [paper, adjacent, type/paper, theme/adjacent, evidence/verified]
---

# Presidential Leadership and Legislative Polarization

**Benjamin S. Noble, Ian R. Turner (2024).** *Leadership (online, per filename; unverified).* [10.31235/osf.io/sa9ke](https://doi.org/10.31235/osf.io/sa9ke). Theme: [[themes/adjacent|Adjacent]].

> [!abstract] Summary
> Noble and Turner build a formal model with no data. A partisan president decides whether to "go public" on one binary issue before two party pivots vote. Going public activates both party bases. The bases then rally, retreat (co-partisans move toward the president, out-partisans away) or backfire. It also ties the president's reelection to the outcome, which gives co-partisans an electoral stake (delta).
> 
> Results:
> - Going public always widens the gridlock interval (Prop 1).
> - Because the status quo matters, presidents go public over a wider range of costs when the status quo already favors them (c(-1) > c(1), Prop 2). The authors call this a "defensive appeal" aimed at fair-weather co-partisans.
> - Under partisan retreat the president never goes public against an unfavorable status quo.
> - Higher sincere polarization (z) makes going public more attractive in defense and less attractive on offense (Prop 4).
> - Higher uncertainty (psi) weakly reduces going public and blunts its sensitivity to the status quo (Prop 5).
> - The authors propose these as predictions to test. Nothing in the paper estimates them.
> 
> This matters because Melange implements the paper almost line for line in fec/engine/salience_shock.py and the dashboard's exec cue. The party asymmetry carries over faithfully. Several mappings do not. The estimator ties rally and retreat odds to the elite stance gap, psi is built from missing stance data, and the paper's presidential-reelection channel is moved onto legislators and onto a CU advocacy actor.
> 
> The sharpest problem is in the live advisory. It credits "defections prevented" on every issue, whatever its status quo, so its four GO PUBLIC verdicts (written 2026-10-08) all fall on offense issues. That reverses the paper's central prediction. A second read of the live salience_advisory.json and salience_shock.py reproduced it (the same source artifact, so not an independent replication).

## Paul's annotation

From [[annotations/Adjacent - Adjacent - Annotated Bibliography|Adjacent - Adjacent - Annotated Bibliography]]:

> Noble, Benjamin S., and Ian R Turner. *Presidential Leadership and Legislative Polarization*, 21 Jan. 2024, https://doi.org/10.31235/osf.io/sa9ke.

## Paul's markup in the PDF

- p1 Highlight: "doing so risks polarizing lawmakers." — 
- p1 Highlight: "This creates potential risks and rewards for presidential leadership" — 
- p1 Highlight: "trade-off in a model of policymaking where two pivotal legislators must agree to change policy and a president can go public to support a policy." — 
- p1 Highlight: "activates the parties’ bases and ties the president’s reelection to policy outcomes" — 
- p1 Highlight: "we also establish the logic of “defensive appeals”" — 
- p1 Highlight: "even while alienating out-partisans." — 
- p2 Highlight: "he salience of an issue, a president can encourage Congress to enact legislation closer to his policy position.." — 
- p2 Highlight: "Presidents use the bully pulpit to raise the salience of issues, increase public support for their policies, and influence Congressional action" — 
- p2 Highlight: "“antidote for ‘gridlock’" — 
- p2 Highlight: "he theory of “going public” has come under some scrutin" — 
- p2 Highlight: "rather than the polarizing party leaders they are today" — 
- p2 Highlight: "presidents are “dividers, not uniters;”" — 
- p2 Highlight: "it would seem that going public might be counter-productive." — 
- p2 Highlight: "He remarked to the press that “one of the things that I hear some of you guys saying is, ‘Why doesn’t Biden say what a good deal it is?’ Why would Biden say what a good deal it is before" — 
- p3 Highlight: "game theoretic model of presidential leadership and legislative polarization in which two pivotal legislators vote over left and right policy alternatives." — 
- p3 Highlight: "Legislators in our model are partisans" — 
- p3 Highlight: "two factors:" — 
- p3 Highlight: "baseline level of ideological polarization" — 
- p3 Highlight: "common preference shock drawn before the vote." — 
- p3 Highlight: "depends on the location of the status quo." — 
- p3 Highlight: "If the preference shock is large enough to overcome ideological polarization, a new policy is enacted. If the preference shock falls within the gridlock interval, the status quo is locked in.1" — 
- p3 Highlight: "partisan president who can choose to “go public” before the preference shock is revealed." — 
- p3 Highlight: "determined by the aforementioned factor" — 
- p3 Highlight: "she raises the salience of the issue and induces legislators to consider the popularity of that policy" — 
- p3 Highlight: "Rally Around the President e" — 
- p3 Highlight: "Left and Right" — 
- p3 Highlight: "Partisan Retreat (cf." — 
- p3 Highlight: "legislative parties move in opposite direction" — 
- p3 Highlight: "Third, Backfire," — 
- p3 Highlight: "both parties’ bases, and thus legislato" — 
- p3 Highlight: "going public changes the odds that the President wins reelection" — 
- p4 Highlight: "If the President stays silent, she is reelected or replaced by an out-partisan challenger with equal probability." — 
- p4 Highlight: "First, we show that going public always expands the gridlock region (" — 
- p4 Highlight: "but that can sometimes help the president achieve legislative success" — 
- p4 Highlight: "highlight another potential effect of going public: Partisan Retrea" — 
- p4 Highlight: "In this case, the President’s decision to go public depends on the location of the status quo." — 
- p4 Highlight: "status quo is unfavorable (from the President’s perspective), going public is unlikely to help her" — 
- p5 Highlight: "nests the classic conception of going public" — 
- p5 Highlight: "broader framework that explicitly accounts for increasing polarization of the presidency" — 
- p5 Highlight: "developing a new logic of defensive presidential appeals to combat co-partisan defection," — 
- p5 Highlight: "llustrate a trade- off between partisan pressure and legislative incentives to help their party win the White House" — 
- p5 Highlight: "To achieve these goals, presidents have increasingly adopted a strategy of “going public,”" — 
- p6 Highlight: "Presidents are salient party leaders whose reputations affect both parties’ prospects for success" — 
- p6 Highlight: "This core tension is fundamental to the model we develop below." — 
- p6 Highlight: "However, the lack of party labels in this model leaves open the question of who the representative legislator and voter are." — 
- p6 Highlight: "we are able to make predictions about when the president goes public as we vary elite and mass polarization as well as change the relevant veto pivot." — 
- p7 Highlight: "which highlights presidents’ first- and second-mover advantages." — 
- p7 Highlight: "Our results suggest that empirical scholars should care- fully consider the status quo when refining theories of presidential leadership." — 
- p8 Highlight: "President P" — 
- p8 Highlight: "left legislative party L" — 
- p8 Highlight: "ight legislative party" — 
- p8 Highlight: "on- strategic challenger who may replace the President" — 
- p8 Highlight: "x ∈{−1,1}" — 
- p8 Highlight: "ex ante preference for x = −1" — 
- p8 Highlight: "right party has an ex ante preference for x = 1." — 
- p8 Highlight: "assume that the incumbent President always prefers x = −" — 
- p8 Highlight: "aligned with the left party." — 
- p8 Highlight: "The status quo is only changed if both parties vote for a new policy." — 
- p8 Highlight: "eginning of the game" — 
- p8 Highlight: "President chooses to go public s = 1 or stay silent s = 0" — 
- p8 Highlight: "specifically among their party’s base." — 
- p8 Highlight: "only after going public does she learn whether each party’s base ultimately supports the proposal." — 
- p8 Highlight: "βL > βR so that the left party is more supportive after the President goes public than the right party." — 
- p8 Highlight: "both βL and βR could be negative," — 
- p8 Highlight: "eft party’s reaction to the President is ‘less negative’ than the right party’s." — 
- p9 Highlight: "Table 1 fragment (garbled extraction): βL > 0 βR > 0 Rally Around the President P{βL > 0,βR > 0} = p Both party bases more supportive Left base more than Right base Not possible P{βL < 0,βR > 0} = 0 (βL > βR) βR < 0 Partisan Retreat P{βL > 0,βR < 0} = q" — 
- p9 Highlight: "βL < 0" — 
- p9 Highlight: "left legislator becomes more supportive of the President and the right legislator becomes more opposed to the President (βL > 0 > βR" — 
- p9 Highlight: "legislators become less supportive of the President, but the right legislator becomes even more opposed than the left (0 > βL > βR)" — 
- p9 Highlight: ", a common preference shock θ is drawn from a Uniform distribution over [−ψ,ψ], ψ > 0, t" — 
- p9 Highlight: "for preference shocks to be realized that lead the parties to vote for the same policy." — 
- p10 Highlight: "They then simultaneously vote for x = −1 or x = 1, denoted by vi ∈{−1,1}, i ∈{L,R}" — 
- p10 Highlight: "If the President does not go public then she wins the election with probability 1/2" — 
- p10 Highlight: "he Challenger takes office." — 
- p10 Highlight: "uL(θt,z) = x(θ −z−sβL)+1Iδ," — 
- p10 Highlight: "uR(" — 
- p10 Highlight: "1 j is an indicator function that takes the value of one when politician j ∈{I,C} wins the election and zero otherwise." — 
- p11 Highlight: "uI(st,xt) = −x−cs+1Iδ," — 
- p11 Highlight: "ncumbent’s utility" — 
- p11 Highlight: "where 1I is defined as above. The Incumbent pays a cost, c > 0" — 
- p11 Highlight: "ure strategy subgame perfect equilibrium (SPE)" — 
- p11 Highlight: "how they will vote given s, θ, and βj, j ∈{L,R}" — 
- p11 Highlight: ", z represents ex-ante individual-level preferences" — 
- p11 Highlight: "can shape the contours of policy developmen" — 
- p11 Highlight: "We capture the potential for information to shape preferences through ψ" — 
- p12 Highlight: "Influence from the bases may lead lawmakers to move in the president’s direction, in opposite directions" — 
- p12 Highlight: "away from the presiden" — 
- p12 Highlight: "uncontroversial assumption that the president’s co-partisans always favor her proposal more than out-partisans" — 
- p13 Highlight: "Presidents who stay silent on an issue are unaffected by its passage or defeat." — 
- p13 Highlight: "using backward inductio" — 
- p13 Highlight: "gridlock regions when the president stays silent and when the president goes public" — 
- p13 Highlight: "policy change occurs if and only if it benefits the left party" — 
- p13 Highlight: "when the status quo is favorable to the right party, xsq = 1, policy change occurs if and only if the right party would vote for x = −1." — 
- p14 Highlight: "When βj >" — 
- p14 Highlight: "party j’s threshold to vote for x = −1 to the right" — 
- p14 Highlight: "Rallying Around the President privileges the President in that it makes it more likely that both parties will vote for x = −1" — 
- p14 Highlight: "Backfire cuts against the President’s interests in that it makes it more likely that both parties will benefit from x = 1" — 
- p14 Highlight: "the President benefits when the status quo is" — 
- p15 Highlight: "already in her favor, x = −1," — 
- p15 Highlight: "but it harms the President’s chances at a policy win when the status quo is unfavorable, x = 1," — 
- p15 Highlight: "going public widens the overall gridlock region—" — 
- p17 Highlight: "The relevant veto player in this case is the left party: as long as the left party does not benefit from voting for x = 1 the final policy will remain x = −1." — 
- p17 Highlight: "is increasing in δ" — 
- p17 Highlight: "That is, as sincere polarization increases, it is less likely policy change will occur, whereas more uncertainty about the size of the preference shock makes it more likely." — 
- p19 Highlight: "President can induce compliance among co-partisans by setting the agenda." — 
- p20 Highlight: "the more likely it is that she will go public." — 
- p21 Highlight: "she will be less likely to use her bully pulpit" — 
- p21 Highlight: "status quo is favorable an increase in the probability going public causes Partisan Retreat makes the President more likely to go public, analogous to an increase in p" — 
- p23 Highlight: "These are issues where presidents and their parties hold strongly polarized positions, and raising their salience induces partisans to harden their stances." — 
- p23 Highlight: "—presidents always have an incentive to go public when the status quo is favorable" — 
- p24 Highlight: "However, when the status quo is favorable, party L is the relevant pivot." — 
- p24 Highlight: "analyzing it can provide insight into how presidents’ expectations of a potential Backfire may constrain position-taking." — 
- p26 Highlight: "our model would predict that going public should occur more often in defense of a favorable status quo rather than to persuade the parties to take up policy change away from an unfavorable status quo." — 
- p27 Highlight: "Lawmakers already have enough information that informs their ex-ante posture toward the policy." — 
- p27 Highlight: "President is less likely to use her bully pulpit to influence legislative policymaking on issues with high levels of uncertainty, all else equal." — 
- p28 Highlight: "President will be less likely to go public on new or volatile policy areas for which there are high levels of uncertainty relative to issues that are well-known or have otherwise low levels of uncertainty." — 
- p28 Highlight: "the President’s behavior is less sensitive to the status quo as uncertainty increases" — 
- p28 Highlight: "the president is less likely to go public overall, but when she does, it is not much more likely under a favorable status quo than under an unfavorable one for the reasons described above." — 

## Study

**Question:** How can presidential leadership, specifically going public, both raise issue salience to move Congress toward the president's policy and increase legislative polarization? And how does this tension shape a president's decision to go public in the first place? (p.1-2)

**Theory:** Going public raises issue salience and activates party bases, which pressure co-partisan and out-partisan legislators (Canes-Wrone 2006 versus Lee 2009's partisan retreat). The model combines pivotal-politics bargaining (Krehbiel 1998; Dziuda and Loeper 2018; Gehlbach 2021) with a partisan president whose reelection is tied to the policy outcome. Party branding and legislative-cartel logic (Cox and McCubbins 2005, 2007) motivates "defensive appeals," where the president goes public to deter co-partisan defection rather than to persuade out-partisans. Sincere polarization (z) and strategic polarization from base pressure (beta) and electoral stakes (delta) are manipulated separately (p.5-6, p.10-11).

**Data:** None. This is a formal model with no sample, unit, period, or N. Empirical material is limited to illustrations: Biden's 2023 debt-ceiling remarks (p.2), Biden's COVID emergency and abortion appeals (p.3, p.23), Trump's ACA repeal attempt (p.24), and a Pew 2019 poll showing a partisan gap of seven points on infrastructure versus 46 on health care (p.22). These are cited as illustrations, not tested.

**Method:** Game-theoretic model. Pure-strategy subgame perfect equilibrium solved by backward induction (p.11, p.13). Players are a president P, a left party L, a right party R, and a non-strategic challenger. The preference shock is theta ~ Uniform[-psi, psi]. Gridlock regions and cost thresholds for going public (c(1), c(-1)) are derived in closed form (Prop. 1-2, App. A.1-A.2). Comparative statics are reported as propositions (Prop. 3-5). Figures 1, 3, 4, and 5 give numerical illustrations at stated parameter values. Section 4 extends the model so the president's silent-reelection probability rho(x_sq) differs by status quo (Prop. 6, App. A.4).

**Identification:** Not applicable: no estimation or causal identification. Claims are model-internal comparative statics. The authors call for future empirical work to test the predictions (p.5, p.25-26).

**Scope:** Contemporary US presidency and two-party Congress with separation of powers, as motivated by 2021-2023 examples. The model covers one binary policy dimension, one issue, and two veto pivots (one per party). The president has no formal agenda power (p.4). The authors note the framework could also fit a mid-twentieth-century "golden age" of less polarized parties (p.23). Single-issue and single-dimension scope is stated as a limitation (p.31).

## Key findings

- Going public always widens the gridlock region, even when both party bases rally behind the president. Silent gridlock region is (-z, z). Public gridlock region is bounded by -z + beta_R - delta/2 and z + beta_L + delta/2 (bar notation on beta lost in extraction). Asymmetric base influence drives this 'effective polarization.' (Prop. 1 (p.13); Fig. 1 and note (p.14); proof in App. A.1 (p.38)) Numbers: Fig. 1 illustrative values: z = 1; beta_L > 0 = 0.75; beta_L < 0 = -0.5; beta_R > 0 = 0.5; beta_R < 0 = -0.75; delta = 0.5
- Going public can raise presidential legislative success, not only polarization, because co-partisans move further toward the president than out-partisans under a Rally. This harmonizes Canes-Wrone (2006) with Lee (2009). (Intro (p.4); Sec. 2.1 (p.14-15); Conclusion (p.29-30))
- The president goes public for a wider range of opportunity costs when the status quo is favorable: c(-1) > c(1) always. Three regions result: c <= c(1) always goes public; c in (c(1), c(-1)] goes public only if status quo favorable; c > c(-1) never goes public. (Eqs. (7)-(8) (p.16-17); Prop. 2 (p.17); proof App. A.2 (p.39-40)) Numbers: c(-1) - c(1) is shown to be positive for 0<p<1, 0<q<1, ps i > z > 0, delta > 0 (App. A.2, p.40)
- Defensive appeal: under Partisan Retreat (q = 1), the president never goes public when the status quo is unfavorable (c_q(1) strictly negative), but always has an incentive to go public when the status quo is favorable (c_q(-1) strictly positive). (Partisan Retreat, p.22-23)
- Under Backfire (1-p-q = 1), the president never goes public with an unfavorable status quo (c_b(1) = c_q(1)). With a favorable status quo she sometimes profits from going public even though it is certain to backfire. The threshold is beta_L < -delta(delta + 2z + 2) / (2(delta + 2)), which falls as z and delta rise. (Backfire, p.24)
- Rally Around the President (p = 1): c_p(-1) and c_p(1) rise with the out-party base's support beta_R (for unfavorable status quo). The gap c_p(-1) - c_p(1) falls as beta_R rises. c_p(1) need not be positive, so the president may not go public even under the most favorable base reactions, because a high delta lets the out-party resist. (Rallying Around the President, p.21-22)
- Prop. 3 (beliefs about reactions): c(1) increases in p; c(-1) increases in p and in q. Higher Backlash probability (1-p-q) reduces going public when the status quo is favorable. When unfavorable, Backlash or Retreat matters only if it comes at the expense of p. (Prop. 3 (p.19); proof App. A.3.1 (p.41-42)) Numbers: dc(1)/dp = (beta_R-bar - beta_R)(2+delta)/(2psi) > 0; dc(-1)/dq > 0 (sign as in proof)
- Prop. 4 (legislative polarization z): going public becomes more likely with favorable status quo and less likely with unfavorable status quo as z rises. (Prop. 4 (p.25); discussion of cross-sectional predictions (p.25-26); proof App. A.3.2 (p.42)) Numbers: dc(-1)/dz = delta/(2psi) > 0; dc(1)/dz = -delta/(2psi) < 0; d(Delta c)/dz = delta/psi > 0
- Prop. 5 (policy uncertainty psi): the president is weakly less likely to go public as psi rises. The gap between favorable and unfavorable thresholds shrinks as psi rises, so the president's behavior is less sensitive to the status quo under high uncertainty. (Prop. 5 (p.26); discussion (p.26-28); proof App. A.3.3 (p.42-43))
- Electoral strength extension: higher silent-reelection probability rho lowers the go-public range (Eqs. 12, 14). If rho(-1) > rho(1) and psi is large (psi > psi*, Eq. 15), the ordering of thresholds can flip so the president goes public more under an unfavorable status quo ('gambling for reelection', after Downs and Rocke 1994). (Sec. 4 (p.28-29); Prop. 6 (p.44); Eq. 15 (p.45); footnote 13 (p.29))
- Illustrative empirical gap: Biden's 2021 infrastructure law fits the case of elite polarization exceeding mass polarization (z > 0, beta_R = beta_R-bar). The authors cite a Pew 2019 partisan gap of seven points on infrastructure versus 46 on health care. This is an illustration, not a test. (Prop. 3 discussion, Rally case (p.22)) Numbers: partisan gap 7 points (infrastructure) vs 46 points (health care), Pew Research Center 2019

## Limitations

- Stated by the authors: the election is blunt. The president wins or loses with certainty given the policy outcome, which ignores other electoral factors (p.30).
- Stated: the model covers a single issue and a single dimension. Presidents strategically choose among issues and rhetorical arguments, which the model does not capture (p.30-31).
- Stated: the model is policy-focused. A president may go public to signal commitment and mobilize a core constituency even when that dampens passage prospects (p.30).
- Stated: the lack of party labels leaves open who the representative legislator and voter are (p.6). The two veto pivots are modeled as party representatives (fn. 4, p.8).
- Stated: a Backfire is rarely observed, which may be endogenous to whether presidents raise salience on unpopular issues (p.24).
- Stated: if z < psi, no preference shock could ever lead the parties to agree, so going public would never matter. The authors set this case aside (fn. 5, p.8).
- Stated: the reelection probability after staying silent is fixed at 1/2 in the main model. Section 4 relaxes this (p.10, p.28).
- Stated: the empirical implications (partisan gap, status quo, uncertainty) are not tested with data. The paper gives only illustrative examples.
- Stated: several comparative statics hold only weakly, since cost thresholds can be negative and then parameter changes have no effect on behavior (fn. 10, p.18; App. A.3).

## Short quotes

- "The core insight is that "effective polarization" is not synonymous with partisan retreat." (p.14, Sec. 2.1)
- "Presidents must be strategic about when they use the bully pulpit, which may backfire under certain conditions." (p.6, Related Literature)
- "presidents may instead be targeting fair-weather friends." (p.31, Conclusion)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Salience shock (Noble-Turner going public)|Salience shock (Noble-Turner going public)]] | contradicts | Going public is more attractive when the status quo is favorable: c(-1) > c(1) always (Prop 2). Under Partisan Retreat the president never goes public against an unfavorable status quo (c_q(1) < 0) but always has an incentive to defend a favorable one. The model predicts that going public occurs more often in defense of a favorable status quo than to win policy change (p.25). The defensive appeal logic depends on the co-partisan being the relevant pivot, which holds only when the status quo is favorable (p.15, p.22-23). (Eqs. 7-8 and Prop. 2 (p.17); veto-pivot logic (p.15, p.22-23); Partisan Retreat (p.22); Prop. 4 discussion (p.25); App. A.2 (p.39-40)) | defensive_appeal_value() runs for every issue with no status-quo check. The advisory value is E_dspread + 1.5 x len(defections_prevented) for offense and defend issues alike. In the live salience_advisory.json (2026-10-08 15:37, 18 issues), the four GO PUBLIC verdicts (top quartile, cut 68.27) are all sq=offense, and each is driven by held counts: Technology & Emerging (E_dspread -3.87, held 67, q=0.83), Member Business Lending (+5.83, held 60), Data Privacy (-19.35, held 68) and Fraud Prevention (-9.73, held 52). The three defend issues rank 13th to 15th. Tech & Emerging is close to the paper's pure-retreat case on an unfavorable status quo, where the paper says never go public, yet it is ranked first. The dashboard bundle surfaces these verdicts as 'GO PUBLIC · holds N'. The figures were reproduced by the verifier from the same 2026-10-08 file and code. They are heuristic advisory scores over 18 issues, not measured effects. (`fec/engine/salience_shock.py:127-138,162,224,229-238; fec/surfaces/build_dashboard_bundle.py:677-682`) | No empirical transfer is needed. This is an internal-consistency check against the model the code claims to implement. The contradiction holds whenever the defensive credit is added on sq='offense' issues. Gating held to sq=='defend' would restore the paper's ordering. Whether the paper's ordering is right for a CU advocacy campaign is a separate, untested question. | strong | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Salience shock (Noble-Turner going public)|Salience shock (Noble-Turner going public)]] | qualifies | Sincere legislative polarization z and base-pressure reactions (beta_L, beta_R, with probabilities p, q, 1-p-q) are separate primitives that the authors vary independently. The infrastructure illustration (Pew 2019 partisan gap of 7 points vs 46 on health care) is presented as a case where elite polarization exceeds mass polarization and a Rally is still possible. (Sec. 1 and Table 1 (p.8-9); Prop. 3 Rally discussion (p.22); Prop. 4 (p.25)) | salience_issue_params estimates z as the absolute D-R gap in mean legislator stance tier. It then sets q = (1-b) x min(1, z/3) and p = (1-b)(1 - z/3). The elite stance gap therefore sets the mass-base reaction odds. The resulting regime mixture drives both the advisory and the dashboard exec cue. (`fec/moneyball_core.py:736-750; fec/surfaces/build_dashboard_bundle.py:670-672`) | The mapping is a design choice, not something the paper implies. It holds only if base reactions on CU issues track legislator stance gaps. That would need a base-side or mass-opinion measure, such as a partisan gap in public or credit-union-member opinion per issue, which the system does not have. The paper's own example warns that the two can diverge. | moderate | VERIFIED |
| [[systems/melange-influence-engine#Salience shock (Noble-Turner going public)|Salience shock (Noble-Turner going public)]] | qualifies | psi bounds a common preference shock and stands for informational policy uncertainty. Higher psi makes the president weakly less likely to go public and shrinks the favorable-vs-unfavorable gap (Prop 5). The paper never links psi to the probability of Backfire. Under a favorable status quo, going public can pay even when backfire is certain (the threshold on beta_L falls as z and delta rise). (Prop. 5 (p.26-28); Backfire (p.24); App. A.3.3 (p.42-43)) | psi = 1 - (share of members with a stance tier) + 0.3 x (True-Neutral share). This measures missing posture data, not uncertainty about preferences. Backfire is b = min(0.35, 0.05 + 0.10 psi), labelled 'Prop 5 caution'. The actual Prop 5 effect (damping go-public value) sits behind SAL_PSI_DAMP, which defaults to 0, so it appears only as a text tag at psi >= 0.8. Backfire steps are always non-positive, so the favorable-status-quo case where backfire still pays is not represented. (`fec/moneyball_core.py:744-747; fec/engine/salience_shock.py:92-93,109-110,219,224,239`) | Data missingness stands in for psi only if a member's lack of a stance label reflects real preference uncertainty rather than collection gaps. Coverage is known to differ by party and chamber (press collector). The b-psi link has no support in this paper and should be cited as a design prior. | moderate | VERIFIED |
| [[systems/melange-influence-engine#Salience shock (Noble-Turner going public)|Salience shock (Noble-Turner going public)]] | qualifies | Section 4 lets the president's silent-reelection probability rho(x_sq) vary. A higher rho lowers the go-public range (Sec. 4 text; Prop 6's first bullet, p.44, says the opposite for an electorally weak president, so the paper is internally inconsistent here). When rho(-1) > rho(1) and psi > psi*, the president may 'gamble for reelection' by going public on an unfavorable status quo (Prop 6, Eq. 15). Legislators' delta is their stake in their party winning the White House, not in their own seats. Caveat: Prop 6's first bullet (p.44) states the opposite direction for an electorally weak President, so the paper is internally inconsistent on the rho direction. (Sec. 4 (p.28-29); Prop. 6 and Eq. 15 (p.44-45); utilities (p.10-11)) | VULN multiplies every legislator's salience step by 1.35 if the member has a defend or contest electoral play, with the comment 'Noble A.4: gambling for reelection'. In backfire, co-partisans with a live race are held at 0 ('delta dominates base: Sec 3.1'). (`fec/engine/salience_shock.py:56-63,104,110`) | This paper does not support the amplifier. Its rho and the gambling logic concern the actor who goes public (the president), not the responsiveness of rank-and-file legislators. Any support for 'electorally vulnerable members respond more' has to come from elsewhere (for example, the electoral-incentives literature), and the 1.35 constant is uncalibrated. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Salience shock (Noble-Turner going public)|Salience shock (Noble-Turner going public)]] | method-source | There are three base-reaction regimes: Rally (both toward), Partisan Retreat (co toward, out away) and Backfire (both away, out more). The single restriction is beta_L > beta_R: co-partisans always favor the president's proposal more than out-partisans do. Going public adds the electoral stake delta to the thresholds as z + beta_L + delta/2 and -z + beta_R - delta/2 (Eqs. 1-2). (Table 1 (p.8-9); p.12; Prop. 1 (p.13-14); App. A.1 (p.38)) | steps_for() sets the base co-partisan step above the out-partisan step in all three regimes. Per-member VULN and susc_mult multipliers apply before rounding, so the ordering can fail for individual members. Rally is co +1, out +0.5 only if bpf >= 0.30. Retreat is co +1, out -1. Backfire is co 0 or -0.5, out -1. The dashboard exec cue (EXECP='R') applies the same p, q, b mixture to bystander mobilization. The defensive channel instead scales the money-defection threshold multiplicatively, by DTHRESH x (1 + DELTA/2) with DELTA = 0.5. The actor is generalized from the president to a CU-side campaign whose party is set by --party. (`fec/engine/salience_shock.py:99-111,127-138; fec/surfaces/templates/make_dashboard_html_00.tpl:657-661`) | The regime structure transfers most cleanly to the dashboard's executive cue, which matches the paper's presidential actor in a federal two-party Congress. It transfers less well to an ACU or industry 'going public' actor, which has no reelection tied to the outcome. The delta channel exists only because co-partisans share the president's electoral fate. The bpf >= 0.30 cutoff, the step sizes and the multiplicative delta are the code's own design, not the paper's. The paper's delta is additive in the vote threshold and has no money-defection analog. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Salience shock (Noble-Turner going public)|Salience shock (Noble-Turner going public)]] | qualifies | 'Gridlock region' means the range of the common preference shock theta over which two pivots keep the status quo. Going public always widens it, even under a Rally, because asymmetric base pressure creates 'effective polarization'. (Prop. 1 (p.13); Fig. 1 (p.14); Sec. 2.1 (p.14-15)) | gridlock_width counts members whose tier sits in a five-rung MIDBAND, and E_dmidband reports its change. The comments read a falling count as 'Prop 1 hollowing' and a rising count on defend issues as a win. The count is reported but not used in the verdict. (`fec/engine/salience_shock.py:50,125,159,174-177`) | Occupancy of a middle band among 533 members is a different quantity from a shock interval between two pivots. Prop 1 makes no prediction about its sign, so the metric cannot be read as testing or reproducing Prop 1. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The DE-062 single-path caveat is now partly met. This verifier re-derived the four GO PUBLIC offense verdicts, the 68.27 cut and the defend ranks 13-15 from the live salience_advisory.json and salience_shock.py:224,229-238. The source artifact is the same, so the finding should be called 'reproduced on a second read' rather than independently replicated.
> - Summary issue: The summary omits the paper's internal inconsistency on electoral strength: the Sec. 4 text says a weak President is more likely to go public, while Prop 6's first bullet (p.44) says less likely.
> - Summary issue: Not in summary_md, but in the bearing method-source: the paper's go-public thresholds are z + beta_L + delta/2 and -z + beta_R - delta/2, not (z + beta_L + delta)/2.
> - Summary issue: Not in summary_md, but in paul_alignment: it says 'both annotation files', while p.annot_sha12s lists one annotation file (ext coverage also says a single file).

## Gaps and flags

- The status quo does not gate the defensive-appeal credit. The paper confines defensive appeals to a favorable status quo, but the advisory adds 1.5 x defections_prevented on offense issues too, and its top-quartile GO PUBLIC verdicts follow from that.
- No base-side or mass-opinion measure per issue. The paper separates elite polarization z from base reactions p, q and beta, but the system derives both from the legislator stance matrix.
- The Prop 4 interaction between z and the status quo is not modeled. Higher z should favor going public on defend issues and disfavor it on offense issues, but the system has no z-by-status-quo term beyond retreat odds.
- The Prop 5 uncertainty damping is off by default (SAL_PSI_DAMP = 0), and psi is measured as missing stance data, not as preference-shock uncertainty.
- The actor's own electoral stake (rho or delta for the campaigning actor) and an opportunity cost c per issue are absent. The opportunity cost is replaced by a fixed top-quartile cut.
- There is no favorable-status-quo case in which going public pays despite a certain backfire, because backfire steps are always non-positive.
- There is no prospective test of any going-public prediction. For example: after a CU-side public campaign, did co-partisans shift more than out-partisans, and did retreat-regime issues polarize further? The paper explicitly calls for such empirical tests.
- The finding that the live salience_advisory.json ranks only offense issues GO PUBLIC because of the defensive-held credit was reproduced on a second read of the 2026-10-08 file and code (salience_shock.py:224,229-238; same source artifact, not independent replication).
- The advisory's 'GO PUBLIC · holds N' verdict reaches the dashboard (build_dashboard_bundle.py:677-682). Under the code's own Noble framing, it currently points the CU shop toward going loud on the issues where the paper says quiet work dominates.
- The VULN x1.35 amplifier and b = 0.05 + 0.10 psi are labelled in code comments as Noble A.4 and Prop 5. This paper supports neither, so both should be cited as design priors.
- Model outputs here (E_dspread, defections_prevented, p, q and b per issue) are heuristic outputs of a formal model with no data. They are not probabilities of real-world rally or retreat, and each figure should name its window (the current stance matrix) and its denominator (533 members, or the members with a tier on that issue).
- No conflict with D-021 (LDA), the two rejected SNA proposals or reciprocity (D-104..D-108). The paper does not touch networks or lobbying data.

Paul highlighted the paper heavily but wrote no annotation. The annotation file holds only the bibliography, with no takeaways. His highlights concentrate on the regimes (rally, retreat, backfire), defensive appeals, status-quo dependence and uncertainty. They include "going public is unlikely to help her" under an unfavorable status quo (p.4), "presidents always have an incentive to go public when the status quo is favorable" (p.23) and the p.26 prediction about defending a favorable status quo. They also include "less likely to go public on new or volatile policy areas" (p.27-28). The system adopted the regime structure he marked. However, the live advisory runs against the status-quo highlights: all four GO PUBLIC issues are offense issues, and Technology & Emerging is the most retreat-prone of them. The uncertainty highlights appear only as a text caution, because the PSI_DAMP dial defaults to 0. His notes are not stale so much as empty. The component cites the paper as its basis, but the paper's own status-quo logic is applied inconsistently in the verdict.

## Related papers

- [[merkley-nd-party-cues-news]] — complements — Merkley and Stecula give empirical evidence of out-party backlash to elite party cues (a Republican backlash to Democratic elite cues on climate). That is the Backfire/Retreat behavior Noble and Turner model only in theory. (The analyst's claim that the two are cited together in the salience-shock component is not supported: no Merkley citation appears in salience_shock.py, moneyball_core.py or the dashboard template.)
- [[bakker-nd-understanding-partisan-receptivity]] — complements — Bakker et al. test why partisans accept co-partisan cues and reject out-party cues. That is the micro-foundation for Noble and Turner's assumed beta_L > beta_R, which the system's steps_for and cpf_pressure both encode.
- [[fouirnaies-2022-electoral-incentives-affect]] — complements — Fouirnaies and Hall supply the empirical support (state legislators' effort responds to electoral incentives) that the VULN x1.35 amplifier needs and that Noble and Turner's Sec. 4 does not provide. Their evidence comes from state legislatures, so it bears on but does not settle federal salience response.
- [[linde-2016-politicians-take-risks]] — complements — Linde and Vis give experimental evidence on politicians' risk-taking in the loss domain. That is a behavioral basis for the 'gambling for reelection' logic Noble and Turner borrow from Downs and Rocke, but it applies to legislators rather than to the president who goes public.
- [[poole-1999-nominate-after-years]] — complements — NOMINATE offers a roll-call measure of elite ideological polarization. It is an alternative, independent estimate of z to the system's stance-tier D-R gap, and it is separate from the base-reaction odds the system derives from that same gap.

### Linked from
- [[joo-2022-image-data-automated]] — complements — salience_shock.py implements Noble & Turner's going-public model and takes its optional (non-default) campaign intensity from Joo-style frame alignment (salience_shock.py:198-209).
- [[vanderdoes-2022-strategic-identity-signaling]] — complements — Noble and Turner model rally, retreat and backfire when an actor goes public, and the salience-shock component implements that model. This paper adds the audience-composition side of outgroup backlash and shows covert messaging only partly avoids it.