---
type: paper
title: "Systemic and Sequential Links between Campaign Donations and Lobbying"
authors:
  - "In Song Kim"
  - "Jan Stuckatz"
  - "Lukas Wolters Freiheyt"
year: "2026"
venue: "Journal of Politics 88(1)"
doi: "10.1086/734531"
category: "Lobbying"
systems:
  - "melange-influence-engine"
  - "melange-lobbying"
  - "mentat-fec-attribution"
  - "moneyball-rank-pillars"
components:
  - "melange-lobbying.fec-lda-bridge"
  - "melange-lobbying.lobbied-member-link"
  - "melange-influence-engine.flip-model"
  - "moneyball-rank-pillars.committee-pillar"
  - "mentat-fec-attribution.identity-ladder"
paul_annotated: true
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/kim-2023-systemic-sequential-links.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\campaign-lobby.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\campaign-lobby.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\campaign-lobby.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\campaign-lobby.pdf"
  - "C:\\Users\\pgoff\\Downloads\\campaign-lobby.pdf"
sha12:
  - "ee6caa08758d"
tags: [paper, lobbying]
---

# Systemic and Sequential Links between Campaign Donations and Lobbying

**In Song Kim, Jan Stuckatz, Lukas Wolters Freiheyt (2023).** *.* [10.1086/734531](https://doi.org/10.1086/734531) Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Kim, Stuckatz and Wolters build a firm-politician-year panel. It covers 2,703 publicly traded manufacturing firms and 979 members of Congress from 2008 to 2018. Lobbying comes from parsed LDA reports that name bills, and donations come from FEC records. The question is whether a firm's first donation to a member in a 3-year window is followed by the firm lobbying bills that the member sponsors, co-sponsors, or that go to the member's committees.
> 
> **Fixed-effects results.** The models compare firm-politician pairs within the same politician. A new donation goes with a 2.3 to 4.8 percentage-point higher chance of that lobbying. The base rate is about 0.07. These are pooled estimates (UNSTRATIFIED by chamber or donor type).
> 
> **Matched difference-in-differences.** These results are split by chamber. Senate estimates rise from 7.5 pp at t+0 to 11 pp at t+3. House estimates rise from 8.6 to 12.7 pp.
> 
> **Donor type and channel.** The link is strongest for corporate PAC money and for lobbying tied to committees. For PACs, committee-linked lobbying rises from 0.103 to 0.147. CEO and employee donations also show links (Table J.3 has an unexplained CEO co-sponsorship value of 0.28 at t+2). Lobbying predicts later donations only through one small sponsor-lobbying term at t+0. Only about 5% of public firms both donate and lobby (2013).
> 
> **Limits.** The authors say plainly that the results are associations, not causal effects. The lobbying measure reaches the member only through who sponsors the bill or which committee gets it, because LDA filings never name members.
> 
> **What it means for Melange.**
> - It is the closest published counterpart to the closed FEC x LDA bridge test. The paper uses a different unit (firm-member pairs over time, not a member-level cross-section) and different fixed effects.
> - It also points to a gap. The paper's strongest channel, committee referral, is missing from the system's LobbyView outcome, which counts only sponsor and cosponsor links. The system's own committee control may absorb that channel, which is an inference, not a paper statement.
> - It gives context on which donor channel (corporate PAC money) is most associated with later lobbying. It does not speak to votes or flips.

## Paul's annotation

From [[annotations/Campaign Politics - Campaign Politics - Annotated Bibliography|Campaign Politics - Campaign Politics - Annotated Bibliography]] (match confidence: medium):

> Kim, In Song, et al. “Systemic and sequential links between campaign donations and lobbying.” *The Journal of Politics*, vol. 88, no. 1, 1 Jan. 2026, pp. 110–124, https://doi.org/10.1086/734531.

From [[annotations/Trey - Articles - Report - Annotated Bibliography|Trey - Articles - Report - Annotated Bibliography]]:

> Kim, In Song, et al. “Systemic and sequential links between campaign donations and lobbying.” *The Journal of Politics*, vol. 88, no. 1, 1 Jan. 2026, pp. 110–124, [https://doi.org/10.1086/734531](https://doi.org/10.1086/734531).
>
> “*Specifically, we find that previous donations are associated with an increase of* ***8–11 percentage points*** *in the likelihood of the targeted politician’s involvement in lobbied legislative bills. The estimated effects are large, increase over time, and are particularly pronounced for committee-related activities.*” This article is very pertinent to the present discussion as it directly tests the likelihood that a representative is to be influenced by an advocacy group under the pretext of past political contributions. Importantly, the author describes the observed phenomena as something that compounds but quickly fades, as the observed longevity of such favor granted by a political donation is pronounced for only **6 months**, but then fades soon after, unless renewed which then such favor is compounded upon.

## Study

**Question:** Do special-interest groups (mainly publicly traded firms, their corporate PACs and their employees) donate to members of Congress whom they later lobby on related bills? The paper tests a sequential "access-buying" link: a first-time donation to a politician is followed by lobbying on bills sponsored by, co-sponsored by, or assigned to committees of that same politician.

**Theory:** Access-buying hypothesis: donations secure access to legislators, and lobbying then informs them or shapes content (pp.5-6). Alternatives discussed: electoral and ideological motives (Poole and Romer 1985); legislative subsidy (Hall and Deardorff 2006); donations as rewards after favorable action (Snyder 1990, tested in App.K). Three hypotheses (pp.6-8): (H1) donors lobby bills the politician sponsors; (H2) donors lobby bills the politician co-sponsors; (H3) donors lobby bills assigned to the politician's committees, the strongest link expected. Corporate PAC donations are expected to show a stronger link than employee or CEO donations.

**Data:** Main sample: publicly traded US manufacturing firms (NAICS 2-digit 31-33) and all members of Congress serving 2008-2018 (979 members, 2,703 firms). Unbalanced panel of 2,646,237 firm-politician dyads and 16,428,834 observations per text (p.15). Table 1 and App. H report N = 16,434,912, which does not match the text; flagged in limitations. Unit: firm-politician-year dyad. Lobbying source: LDA reports, over 1.2 million parsed, of which 1.27 million (1999-2020) are cited, with 0.25 million (about 20%) mentioning specific bills and covering about 97% of lobbying spending (pp.9-10). Donation source: FEC filings, over 75 million records since 1999 (abstract and p.11); about 53 million itemized individual contributions 1980-2018 (p.11). Interest-group coverage: over 84,000 groups (abstract; p.2). Other samples: 2013 cross-section of all public firms (9,791 firms, 597 members, 5,845,227 possible dyads; App.D); services firms (App.H.2, NAICS 42-81); 1999-2000 interest-group comparison (App.G); 111th Congress (2009-2010) Senate miscellaneous tariff bills (MTBs), 477 bills and 107 lobbying firms (App.L). Covariates from Volden and Wiseman (2018) and Compustat (sales, employees).

**Method:** (1) Linear probability model with fixed effects (Eq. 2, Table 1, App.H). Outcome: firm lobbies a bill tied to politician j in year t through sponsorship, co-sponsorship or committee. Treatment: first-time donation within a 3-year window (Eq. 1, L=3). Fixed-effect structures vary across columns: firm, year, politician, and firm-politician. Standard errors clustered by firm. (2) Difference-in-differences with matching (Eqs. 3-4, Figs. 4-5, Tables J.1-J.3). Each treated pair (i,j) with a first-time donation in t is matched to 10 nearest firms (Mahalanobis distance on sales, employees and lobbying up to t-1) that gave j no donation in t to t-3. Estimand: change from t-1 to t+F in the lobbying outcome, treated minus matched-control mean, averaged over treated pairs. 95% CIs from block bootstrap. Robustness: NAICS 3-digit matching (Table J.2), donor-type splits (Fig.5), lagged donations (Table H.5), lobbying-to-donation reverse test (App.K, Table K.1). (3) Descriptive comparisons with Ansolabehere, Snyder and Tripathi (2002) (App.D, E, G).

**Identification:** Within-politician comparison. Variation is across firm-politician pairs with the same politician, so politician-specific productivity and ideology are absorbed (p.2). The first-time-donation-in-3-years design separates new donations from ongoing ones and makes donation timing identifiable. The DiD assumes parallel trends: the matched controls' pre-period lagged outcome changes are constant (p.24). Covariate balance improves after Mahalanobis matching (App.I). Firm-politician alignment is handled by firm-politician FE (cols 4 and 8 of Table 1). Unobserved firm-politician-time shocks are not handled, and the authors say the estimates are not causal (pp.28-29).

**Scope:** Population: publicly traded US manufacturing firms (services and all-public-firm checks in appendices), lobbying and PAC/employee donations to members of Congress. Institution: US federal Congress (House and Senate), with lobbying under LDA and donations under FECA. Era: main analysis 2008-2018 (LDA electronic filing mandatory after the 2007 HLOGA amendment, p.15); descriptive work 1999-2020; the 1999-2000 comparison uses interest-group data. Results are observational and do not establish causation.

## Key findings

- Firm-politician Table 1 OLS: a first-time donation within 3 years raises the probability that the firm lobbies a bill tied to the politician (sponsor, co-sponsor or committee) by 2.3 to 4.8 percentage points. Base rate of such lobbying is about 0.07. Numbers: Donation coef 0.041, 0.048, 0.047, 0.023 (SE 0.006-0.007); N = 16,434,912 (text says 16,428,834 observations). (Table 1 and text, pp.19-20; FE-heavy col.4 coefficient 0.023)
- Matched DiD: donations raise the probability of lobbying a politician-linked bill, rising over time. Senate effect is about 8 pp at t+0, rising to about 11 pp at t+3; the House effect is larger. Numbers: Senate: 0.075, 0.097, 0.108, 0.110 (t+0 to t+3); House: 0.086, 0.111, 0.127, 0.127; Senate SE about 0.003-0.004, House SE about 0.002-0.003; 3,778 Senate and 5,497 House first-time donation instances. (Figure 4, pp.25-26; Table J.1, p.17; text p.26)
- Corporate PAC donations show the strongest link to later lobbying, especially committee-related lobbying. Employee and CEO donations also show links, but smaller ones. Numbers: Committee lobbying, PAC: 0.103 (t+0) rising to 0.147 (t+3); CEO 0.017 to 0.050; Employee 0.015 to 0.039. Sponsorship, PAC: 0.029 to 0.037; CEO 0.012 to 0.029; Employee 0.014 to 0.022. Co-sponsorship, PAC: 0.097 to 0.102. Table J.3 CEO co-sponsorship at t+2 is 0.28, which is far above the rest of its row and is not discussed in the text. Instance counts: PAC 2,662; CEO 792; Employee 934. (Figure 5, p.27; Table J.3, p.19)
- Donations are followed by lobbying, not the reverse. Lobbying does not raise later donations, except a small effect of lobbying a bill's sponsor at t+0. Numbers: Lobby Sponsor t+0 coef 0.0074 (SE 0.0028); other lags and types near zero; 12,232,896 firm-politician-quarter dyads; lobby-sponsor instances 4,426. (Appendix K, Figure K.1, Table K.1, p.20-21 (text p.26))
- Donations and lobbying are concentrated. About 5% of public firms both donate and lobby, and they account for most political spending. Numbers: 2013: 518 of 9,791 public firms (5%) both donate and lobby, responsible for $2.6bn or 89% of more than $2.8bn. Firm-politician dyads with both: 12,862 of 5,845,227 possible dyads (about 0.22%; the paper says less than 0.1%, which is arithmetically wrong) containing $520m (18%). About 4% of all dyads have any political link. (Data section p.5; Table D.1, p.4 and p.6; App.D)
- Among firms that both lobby and donate, PAC donations and lobbying spending are positively and significantly associated (log-log). Numbers: beta = 0.88, R2 = 0.39; text: a 10% PAC increase is associated with 8.8% more lobbying among firms that also lobby. (Appendix D, Figure D.1, p.5)
- Most donation-only links come from employees, not corporate PACs. Numbers: 19,735 of 23,270 donation-only dyads in 2013 (about 85%) are created by employee donations. (App.D, p.6)
- Miscellaneous tariff bills (MTBs), a descriptive success check: firms that lobby and donate to the senator from their state or to Senate Finance members see MTB approval more often than lobby-only firms. The paper calls the gap significant, but no test is reported. Numbers: Lobby-only: 23 of 66 (0.348) approved. Lobby+donate: 17 of 41 (0.415). Difference 0.067 (+19%). 477 Senate MTBs in 111th Congress; 211 proposed by 46 public firms; 107 public firms lobbied on MTBs. (Appendix L, Table L.1 (duplicated as L.2), pp.24-26)
- Heterogeneity by the dyad and group of interest: the paper says the donation-lobbying link is strongest for corporations, weaker for labor unions; this replicates Ansolabehere et al. (2002) using 1999-2000 data. Numbers: Beta (cube-root PAC vs lobbying): corporations 2.0; membership organizations 1.6; trade associations 0.8; labor unions 0.6; text says no relationship for unions, but Figure G.1 shows 0.6. (App.G, Figure G.1, p.9-10)

## Limitations

- Not causal. The authors say their estimates should not be read as causal and that the study shows an association (pp.3, 28-29). Donation timing may be driven by unobserved firm-politician-time shocks.
- Lobbying is measured only through bill-linked reports: about 20% of 1.27 million reports cite bills. The authors call the results a lower bound (p.10).
- Bill positions are reported for only about 17% of lobbied bills; 82% of those are supportive (fn.12, p.17).
- Parallel trends are assumed; the authors check only that the lagged-outcome difference is roughly constant (p.24). Matching uses sales, employees and lobbying up to t-1 only.
- Sample inconsistencies: text says 16,428,834 observations, while Table 1 and App. H report 16,434,912. The donation-amount effect is stated as 0.03 pp per 10%, which matches only the firm-politician-FE column (0.003), not col.5 (0.006).
- Lobbying data quality: LDA reports lack unique bill IDs, reporting changed from semi-annual to quarterly in 2008, and the sample starts in 2008 for these reasons (fns.5, 9; p.15). Bill matching relies on cross-referencing bill numbers and titles.
- Relationships may reflect the revolving door or association lobbying; lobbyists with covered positions are about 7.8% of lobbyists and their lobbying on sponsored bills is under 1% (fn.15). The authors argue this does not drive results.
- Manufacturing focus: service firms give almost identical results (App.H.2). Donor types are correlated (p.27), so type-specific effects are not independent.
- MTB analysis is one Congress (111th), descriptive, and small (41 vs 66 firms). No significance test is reported despite the word 'significant' on p.25. Table L.2 duplicates Table L.1 in the extraction.
- Unit of donation treatment is first-time donation within 3 years; the authors note that first-time-ever donations give almost identical results (App.H.3-H.5), but this is not a test of a different identification.

## Short quotes

- "previous donations are associated with an 8-11 percentage point increase in the likelihood of the targeted politician's involvement in lobbied legislative bills" (Abstract, p.1)
- "we are not asserting a causal relationship between campaign donations and lobbying actions" (p.3)
- "our estimates should not be interpreted as causal" (p.29)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-lobbying#FEC to LDA bridge test\|FEC to LDA bridge test]] | qualifies | Within-politician first-time-donation designs find donations followed by lobbying on the member's bills. Table 1 pooled coefficients 0.041, 0.048, 0.047, 0.023 (SE 0.006-0.007; N 16,434,912 in table, 16,428,834 in text). Matched DiD Senate 0.075 to 0.110 and House 0.086 to 0.127 over t+0 to t+3 (3,778 Senate and 5,497 House instances). Committee-linked PAC lobbying rises 0.103 to 0.147, versus sponsorship 0.029 to 0.037. Lobbying does not predict later donations, except a small sponsor-lobbying term in App K. (Table 1 pp.19-20; text p.20 (2.3 to 4.8 pp, base rate 0.07); Fig.4 and Table J.1 pp.17 and 25-26; Table J.3 p.19 and Fig.5 p.27; App K p.26 and Table K.1) | Per the analyst: a member-level cross-sectional Spearman-type rank correlation (the code output label says pooled fractional-rank Spearman rho) between member FEC exposure and LobbyView n_bills for 22 lob_ids, with four controls (party, chamber, volume, committee). Raw rho +0.153 and four-control partial -0.0002 were not checked against a results file in this pass. Constants and wording confirmed in fec/lobbying/fec_lda_bridge3_look.py lines 57-80 (CONTROL_NAMES, OVERCONTROL_SENTENCE, UNCORROBORATED wording). `fec/lobbying/fec_lda_bridge3_look.py:57-80` | Paper design: firm-politician pair fixed effects, year FE, first-time donation timing, 2008-2018 manufacturing firms. Bridge: member-level cross-section, 2023-26, 22 financial-sector clients, committee control. Different estimand, so the paper does not test the bridge. The paper does not say its committee channel is absorbed by a committee control in a cross-section; that overcontrol reading is the analyst inference (and the code's OVERCONTROL_SENTENCE). The paper is observational and says so (pp.3, 29). | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Lobbying member-year network and circular lobbied-member link\|Lobbying member-year network and circular lobbied-member link]] | qualifies | Outcome is a firm lobbying a bill tied to politician j through sponsorship, co-sponsorship or committee referral, with bills matched by number and title because LDA reports have no member names or unique bill IDs. Bill-linked reports are 0.25M of 1.27M (about 20%) and cover about 97% of lobbying spending. The authors call the estimates a conservative lower bound. (Eq.2 and pp.9-11; text p.10 (1.27M reports, 0.25M bill-linked, 97% of spending, lower-bound wording); fn.5 and fn.9 p.15) | fec/lobbying/lobby_interactions.py lines 4-5 describe the shipped network as (organization, member, year) counts of whether an organization lobbied a bill the member sponsored or cosponsored. The claim P(joined given lobbied) equals 1 by construction is the analyst gloss and is not stated in those lines. `fec/lobbying/lobby_interactions.py:4-5` | The paper uses the same roster join, but its treatment is donation timing, not the join itself, and it treats the outcome as bill involvement, not contact. Neither the paper nor the code supports reading the join as influence or contact (consistent with D-021). The paper's own committee-referral arm is absent from the system's join. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | context | Corporate PAC donations show the strongest association with later lobbying of the recipient (committee 0.103 to 0.147 versus CEO 0.017 to 0.050 and employee 0.015 to 0.039). The authors say the estimates should not be read as causal (p.29). The amount effect is small: 0.003 on log donation amount in the firm-politician FE column (Table 1 col.8), which is about 0.03 pp per 10 percent (text p.20). (Fig.5 and Table J.3 p.19 and p.27; Table 1 col.8; text p.20 and p.29) | fec/engine/flip_model.py line 21 sets WM and WP to 0.35 and 0.65 and comments that money is the minority partner to peer and network influence. Engine money is direct-PAC only (fec/collect/refresh_fec_money.py docstring, individual contributions out of scope). `fec/engine/flip_model.py:21` | The paper says nothing about the WM to WP ratio or roll-call flips. Its outcome is the firm's lobbying behaviour, not the member's vote, in manufacturing firms 2008-2018. It supports at most the general premise that PAC money is the most-associated channel, so it is context and a weak prior, not evidence for the 0.35 weight. | weak | VERIFIED-SOFTENED |
| [[systems/moneyball-rank-pillars#Committee - Influence pillar\|Committee - Influence pillar]] | supports | Committee-linked lobbying after a donation is the largest channel and grows over time (PAC 0.103 to 0.147 over t+0 to t+3). Hypothesis H3 predicts the committee channel is strongest (pp.6-8). Committee chairmanship enters the model only as a control. (Hypotheses pp.6-8; Table J.3 p.19; Fig.5 p.27; controls p.18) | fec/engine/committee_scores.py lines 30-33 and 125-128 set seat base weights Highly 3.0, Moderately 1.8, Slightly 0.9, subcommittee factor 1/6, and leadership multipliers Chair 3.0 full and 7.5 sub, Ranking 2.0 full and 4.5 sub, Vice 1.0 and 2.0. These values match the code. `fec/engine/committee_scores.py:30-33,125-128` | The paper supports only the claim that committee position is where interest-group money and lobbying concentrate. It does not estimate chair, ranking, or subcommittee weights, and it does not estimate committee effects on bill outcomes. Sample is manufacturing firms, federal level, 2008-2018. | weak | VERIFIED |
| [[systems/mentat-fec-attribution#Contributor identity match ladder\|Contributor identity match ladder]] | method-source | Donor and firm linkage uses constrained graph clustering and fastLink probabilistic record linkage (Enamorado, Fifield and Imai 2019). Executives are classed by SOC 11-1011. The paper reports no precision or recall for its linkage. (Data section pp.11-12; SOC code text at about line 349 of the text file (11-1011 only)) | C:/Users/pgoff/Mentat/mentat/R/collect.R lines 2110-2156 implement an ordered rule ladder: exact name and same zip gives AUTO, with HIGH if the employer contradicts; exact name and same city gives HIGH; nickname and same zip gives HIGH; nickname and same city gives REVIEW; exact name, same state and similar occupation gives REVIEW; employer contradiction with a name match gives REVIEW; otherwise DISCARD. No name-frequency weighting appears in this ladder. `C:/Users/pgoff/Mentat/mentat/R/collect.R:2110-2156` | The paper confirms the fastLink method and the 11-1011 code only. SOC 27-3030 for government-relations staff is not in the paper text, so that part of the claim is refuted. Suggesting that frequency-weighted fastLink would fix the common-name blind spot is the analyst's opinion, and the paper gives no benchmark. Any change to the ladder is a ruling for Paul under D-040 (not checked here). | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Key finding on App D: the item gives 2.6bn of 2.86bn (89%). The paper text says more than $2.8 billion total and $2.6 billion or 89%; the 2.86 figure is not in the paper.
> - Summary issue: Paper-internal error repeated as fact: App D says 12,862 dyads are less than 0.1% of 5,845,227 possible dyads. 12,862 divided by 5,845,227 is about 0.22%. Quote the count and its denominator, and flag the paper's <0.1% claim as wrong.
> - Summary issue: Paul's annotation claim (media-blend note that Kim's effects fade within 2 to 3 years; annotation tagged JOP 2026) is not in this item's inputs: paul_entries has no takeaways and the ext record says no annotation file was found. Cannot verify from the item; remove or source it. This version measures rising estimates from t+0 to t+3 and no fade.
> - Summary issue: Summary says lobbying does not predict later donations. Paper text p.26 says no effects; Table K.1 shows Lobby Sponsor t+0 0.0074 (SE 0.0028) and t+1 0.0041. Say there is one small sponsor-lobbying term at t+0.
> - Summary issue: Summary calls the CEO and employee links much weaker. Table J.3 has a CEO co-sponsorship value of 0.28 at t+2, far above its row and not discussed in the text. Note the anomaly rather than summarizing it as weak.
> - Summary issue: Summary says the paper supports the PAC-only scope and the WM less than WP weighting. The paper discusses neither the scope choice nor flip weights; at most it gives context on which channel is most associated.
> - Summary issue: Paper-internal inconsistency: App G text says the relationship is nonexistent for labor unions while Figure G.1 shows beta 0.6. Note both.
> - Summary issue: Related links: kept kim-2020-strategic-sequential-links (builds-on), kalla-nd-campaign-contributions-facilitate (complements) and huneeus-2018-effects-firms-lobbying (cites, year to be corrected). Dropped furnas-nd-conflating-lobbying-pacs, bombardini-2020-empirical-models-lobbying, imai-2021-fixed-effects-regression, kim-2018-lobbyview-firm-level, kim-2025-measuring-interest-group and kim-2020-mapping-political-communities: the paper does not cite them, and the same-data and same-method claims about LobbyView and Imai-Kim are not supported by the paper text.
> - Summary issue: Confirmed without change: Table 1 coefficients and N mismatch; Fig.4 and Table J.1 Senate and House values and instance counts; Table J.3 committee, sponsorship and co-sponsorship values; App D 518 of 9,791 firms, 19,735 of 23,270 donation-only dyads, beta 0.88 and R2 0.39; App L 23 of 66, 17 of 41, 0.067 (+19 percent), 477, 211, 46 and 107 MTB counts; the causal-caution quotes at pp.3 and 29; the 8-11 pp abstract quote; the 82.11 percent supportive figure (fn.12); the 7.8 percent and under-1 percent lobbyist figures (fn.15).
> - Summary issue: The item key finding that the 0.03 pp per 10 percent amount effect matches only the firm-politician FE column is an inference. 0.003 times ln(1.1) is about 0.03 pp, consistent with col.8; the paper itself does not state which column.
> - Summary issue: The analyst output shows a 2 to 3 year fade claim in paul_alignment; that is unsupported (see Paul annotation issue above). All bearings and summary claims remain UNVERIFIED agent analysis until a second path reproduces them (DE-062).

## Gaps and flags

- The system's LobbyView member outcome (lob_id x bioguide x year n_bills) links members only as sponsor or cosponsor of a lobbied bill. It has no committee-referral arm, which is the channel where the paper's association is largest. Brain note lobbyview-collector.md:127 confirms edges = sponsor + all cosponsors.
- No org x member panel over time: the bridge is a member-level cross-section with a committee control. Nothing in the system can separate a relationship's onset (a first donation) from continued giving, so the paper's within-politician, first-time-donation design has no counterpart.
- Engine money features (flip_model, money_response) are amounts per two-year cycle relative to receipts. They have no timing or relationship-onset signal, even though the paper finds the binary first-donation indicator carries the association and the amount effect is small.
- No reverse-direction check (lobbying followed by donations) exists alongside the bridge. The paper treats that check as a key way to rule out reverse causation (App. K).
- D-021: the paper builds a firm-politician bipartite graph from LDA bill mentions joined to sponsor, co-sponsor and committee rosters (Fig.1, App.F). Adopting those links as Melange edges or weights would breach D-021 (LDA is an identity source only) and reproduce the circular roster join (lobby_interactions.py:4).
- Wording (D-025 family): 'access-buying' is the authors' hypothesis; the results are associations ('not causal', pp.3, 29). No figure from this paper may be presented as an access, closeness or introduction probability, or as donations causing lobbying.
- Pooled-coefficient rule: Table 1's 2.3-4.8 pp estimates pool House and Senate and all donor types; quote them as UNSTRATIFIED (window 2008-2018, denominator 16.4M firm-politician-years). The chamber-stratified DiD (Senate 0.075-0.110, House 0.086-0.127) is the stratified figure.
- Paper-internal inconsistencies: the text says N=16,428,834 while Table 1 and App.H report 16,434,912. Table J.3 has a CEO co-sponsorship value of 0.28 at t+2, out of line with its row. App. L calls the MTB gap (0.415 vs 0.348; 41 vs 66 firms) 'significant' without reporting a test.
- Bridge closure: the round-3 prereg bars a different control set on the 2023-26 window as a successor. This paper supports only a successor prereg on a new LobbyView vintage, never a re-run.
- Paul's annotation says the favor lasts only '6 months'. Neither version supports that: this published version says the link grows two or three years after the donation, and the 2020 working paper (kim-2020-strategic-sequential-links) is the one that says it disappears in 2 to 3 years. Do not use the annotation as a decay timescale for press/social half-lives or staffer-tie decay.
- Neither rejected SNA proposal (dropping regular cosponsorship edges; per-edge volume normalization) is argued in this paper.

The paper agrees with Paul's core expectation that donations precede lobbying. It does so at the firm-member level, within the same politician, and as an association only. It is also consistent with Furnas's low overlap: only about 5% of public firms both donate and lobby. The system's closed bridge null does not contradict the paper. That test uses a member-level cross-section with a committee control, a different unit and possibly an overcontrol of the paper's strongest channel. Paul's notes do not record this distinction. His committee-pillar position (money and lobbying effects strongest in committee activity) is supported in direction. The paper says nothing about the pillar's weights. His annotation says the favor lasts only 6 months, which this version does not support: the estimates rise through t+3 and no fade is measured (the 2-3 year disappearance is the 2020 working-paper wording). The annotation names this work as JOP 2026 (Journal of Politics 88(1), DOI 10.1086/734531); the PDF read here is a December 2023 version.

## Related papers

- [[kim-2020-strategic-sequential-links]] — builds-on — Item title Strategic and Sequential Links between Campaign Donations and Lobbying, same authors, same first-time-donation design, an earlier working version of this paper. Confirmed from the item file title and author fields.
- [[kalla-nd-campaign-contributions-facilitate]] — complements — The paper cites Kalla and Broockman (2016) at p.4 as the closest field-experiment benchmark on donor access (text lines 68 and 131). Link is real.
- [[huneeus-2018-effects-firms-lobbying]] — cites — The paper cites Huneeus and Kim (2019) on firm-level returns to lobbying at pp.28-29 (text lines 1311 and 79465). The slug year 2018 does not match the cited 2019 publication; the link is real but the year label should be corrected.

### Linked from
- [[furnas-nd-conflating-lobbying-pacs]] — complements — The later version of the same donations-then-lobbying sequencing argument. Furnas's low overlap bounds how much of the organizational population such sequencing can describe.
- [[fremeth-2018-spillovers-regulating-corporate]] — complements — Real item (title: Systemic and Sequential Links between Campaign Donations and Lobbying). Only existence and title checked.
- [[note-industry-growth-and-political-spending-research]] — complements — Item record exists (Kim, Stuckatz and Wolters Freiheyt 2023, donations and lobbying links). Title checked only.