---
type: paper
title: "Why Is There So Little Money in U.S. Politics?"
authors: ["Stephen Ansolabehere", "John M. de Figueiredo", "James M. Snyder Jr."]
year: 2002
venue: "NBER Working Paper 9409 (December 2002)"
doi: ""
category: "Campaign Politics"
systems: [melange-allocation, melange-influence-engine, mentat-giving-forecast, mentat-sector-growth-study]
components: ["melange-influence-engine.money-response-defection", "melange-influence-engine.flip-model", "melange-influence-engine.competitiveness-multiplier", "melange-allocation.stance-money-gate", "mentat-sector-growth-study.growth-spending-regressions", "mentat-giving-forecast.cycle-model"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/ansolabehere-2002-little-money-politics.pdf]]"
original_paths: []
sha12: ["044a24a736e4"]
tags: [paper, campaign-politics, type/paper, theme/campaign-politics, evidence/verified]
---

# Why Is There So Little Money in U.S. Politics?

**Stephen Ansolabehere, John M. de Figueiredo, James M. Snyder Jr. (2002).** *NBER Working Paper 9409 (December 2002).* Theme: [[themes/campaign-politics|Campaign Politics]].

> [!abstract] Summary
> Ansolabehere, de Figueiredo and Snyder (NBER WP 9409, 2002) restate the Tullock puzzle. In 2000 about 3 billion dollars went to campaigns, against 2 trillion of federal spending. They answer that campaign money is mostly individual consumption and participation, not policy purchase. Individuals gave about 2.4 of the 3 billion dollars. Only 4 percent of PAC gifts are near the limit. In nearly 40 roll-call studies, PAC effects are null or wrongly signed three times in four. In their own House panel (1978-1994, N = 3400), 60000 dollars of corporate PAC money moves a 0-100 Chamber score by at most 2 points, while party moves it by more than 30. District fixed effects nearly remove the money effect, member fixed effects remove it entirely, and IV reverses the corporate sign. Spending tracks income: the gubernatorial income elasticity is about 1.1, the stakes (government spending) have no effect, and closeness raises spending. Executives give about 0.05 percent of income, close to the national rate. For Paul's systems the paper is load-bearing. The Melange money engine already cites Ansolabehere to justify cutting MAXSHIFT from 40 to 18 and weighting money below peers. The paper supports that cut on votes. It does not support the access half of the code comment, and it gives no evidence that dollars buy more in competitive seats: closeness raises the supply of money, mostly small individual gifts. On the Mentat side, the consumption account fits the sector growth study's null for industry growth and suggests that donor or national income, not industry revenue, scales giving.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Why is there so little money in U.S. politics given the large value of policies at stake (Tullock 1972 puzzle), and why do organized interests give at all? The authors argue that campaign giving is mainly consumption and participation by individuals, not investment in legislation.

**Theory:** Campaign contributions as investments in a political market for private benefits (Snyder 1990; Grossman and Helpman 1994; Denzau and Munger 1986; monopoly-donor models of Dal-Bo 2001 and Helpman and Persson 2001) versus contributions as consumption of participation, like charitable giving (consumer contributors who are ideologically motivated, asked by friends, and civically active). For interest groups the authors list four or five possible reasons for giving: small return, buying access, electing friends, coordinating individual donors, and consumption by executives.

**Data:** (a) Survey of nearly 40 roll-call studies (Table 1). (b) Chamber of Commerce roll-call score, U.S. House 1978-1994, N = 3400 (Table 2). (c) FEC-era totals 1978-2000 and presidential spending 1884-2000 (Figure 1). (d) Gubernatorial state panel 1976-2000, N = 326 (Table 3), with BEA income and Census finance data. (e) House elections 1992-2000 with district income and margins (Table 4, 1735 races) and Senate spending by state size. (f) 94 executives at 12 large firms, 1997-98 cycle (ExecuComp, Center for Responsive Politics). (g) 2000 totals from FEC and PAC accounts; 2000 ANES survey.

**Method:** Descriptive accounting of contributions by source; tabulation of prior roll-call studies by significance and sign; OLS and two-stage IV regressions of the CCUS score on corporate and labor PAC money with year effects and either party plus district preference, district fixed effects, or member fixed effects; OLS state fixed-effects panel of log per-capita gubernatorial spending; cross-section regressions of House race spending on district income and margin with year, party and incumbency controls; time-series ratios of spending to GDP and national income.

**Identification:** IV for PAC money using electoral competition (opponent spending, absolute vote share minus .5, unopposed dummy) and member power (party leader, committee chair, Ways and Means or Energy and Commerce membership), following Chappell 1982 and Welch 1982. Member fixed effects absorb time-invariant member preferences. Gubernatorial estimates rely on within-state variation with state fixed effects. Time-series, executive and aggregate-share results are descriptive with no causal identification.

**Scope:** U.S. federal (House, Senate, president) and state (governor) elections under FECA, mainly 1976-2000; presidential spending 1884-2000; roll-call evidence for the U.S. House 1978-1994; PAC activity in 2000; executives in the 1997-98 cycle. Units are contributors (individuals, PACs, firms), legislators and states. Claims about the whole system are broad, drawn from these pieces.

## Key findings

- Individuals gave nearly 2.4 billion of about 3 billion dollars in 1999-2000; about 21 million people (about 10 percent of adults) gave, averaging about 115 dollars (p.7).
- Only 4 percent of PAC contributions to House and Senate candidates are at or near the 10000 dollar limit; the average PAC gift is 1700 dollars and the modal contribution is zero (p.8).
- In 2000 PACs spent 579.4 million dollars in total; 280.8 million went to or for candidates and about 300 million to overhead and other activity that could have been paid by the sponsor's treasury (p.9).
- In a survey of nearly 40 roll-call studies, PAC contributions had no significant effect or the wrong sign in three of four instances (Table 1, p.15).
- CCUS model, House 1978-1994, N = 3400: an extra 60000 dollars of corporate PAC money (about one SD) moves the 0-100 score by at most 2 points, while a change of party moves it more than 30 points (pp.18-19).
- District fixed effects nearly eliminate the contribution effects, and member fixed effects eliminate them entirely; coefficients are tiny and insignificant in OLS and IV (Table 2, p.19).
- IV estimates shrink the effects and reverse the sign on corporate money (column 4 coefficient -0.30, Table 2); the authors conclude the evidence for substantial vote influence is rather thin (pp.19-20).
- Interest groups spent about 3 billion dollars on lobbying in 1997-98, roughly ten times the 300 million they spent on PAC contributions (p.32, citing Ansolabehere Snyder and Tripathi 2002).
- Tullock puzzle restated: 2000 campaign spending was about 3 billion dollars against 2 trillion of federal spending; 192000 dollars of 1985 sugar-industry contributions preceded a subsidy vote, with the subsidy worth over 5 billion dollars over five years (pp.11-13).
- Executives: 94 top executives at 12 firms gave about 7500 dollars per cycle, or 51 dollars per 100000 of income (0.05 percent), close to the 0.04 percent of national income spent on campaigns in 2000 (p.22).
- Real campaign spending roughly doubled 1976-2000 but has no trend relative to national income; presidential spending as a share of GDP was flat 1912-2000 and about three times higher in the 1880s and 1890s (pp.23-24).
- Gubernatorial panel 1976-2000 (N = 326, state fixed effects): income elasticity about 1.07 to 1.17, government spending has no independent effect (-0.09), and closeness has a strong positive effect (2.93) (Table 3, pp.26-27). House races 1992-2000: each 1000 dollars of district income adds about 20000 dollars of spending; close races attract 60 percent of money from individuals versus 31 percent from PACs (pp.28-29).

## Limitations

- Literature magnitudes are hard to compare because most studies do not report means or standard deviations (p.16).
- Simultaneity between contributions and votes, and omitted district and donor-strength controls, may bias the roll-call estimates (pp.16-17).
- The IV results are sensitive to instrument choice; sign and size change across specifications (p.19).
- The consumption account lacks specific empirical predictions that separate it from rent-seeking; the authors say the theory is undeveloped (p.31).
- The party coefficient may partly reflect party money or interest-group money routed through parties (endnote 9, p.58).
- Party account transfers may double count about 10 percent of party money; the 380 million soft-money figure is estimated from FEC individual donor files; industry totals are called overestimates (endnotes 3-5).
- The House regression is reported only in text, with a linear statement (20000 dollars per 1000) and a log footnote (elasticity 0.43) that are not reconciled (p.28, endnote 12).
- Table 2 extraction is garbled; one labor IV cell is starred significant in the extraction while the text calls legislator-FE effects insignificant, and the text's 6-point labor effect has no matching positive coefficient.
- Executive sample is 94 people at 12 firms; state data are limited to 1976-2000; the presidential series is of lower data quality by the authors' own account (p.22).
- The 20 percent return on investment for a subset of PACs is an illustration, not an estimate (p.32).

## Short quotes

- "campaign contributions are not a form of policy-buying, but are rather a form of political participation and consumption"
- "the question is not why there is so little money politics, but rather why organized interests give at all"
- "Money has little leverage because it is only a small part of the political calculation that a reelection oriented legislator makes"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Money response (lean shift) and money-triggered defection\|Money response (lean shift) and money-triggered defection]] | supports | In the most common House specification (Column 1, 1978-1994, N = 3400, CCUS 0-100 score) an extra 60000 dollars of corporate PAC money (about 1 SD) changes the score by at most 2 points; 50000 dollars of labor PAC money changes it by 6; a change of party changes it by more than 30 points (40 with district FE). Member fixed effects eliminate the contribution effects, and IV reverses the sign on corporate money. Changes in donations to an individual legislator do not translate into changes in that legislator's roll-call votes. (Table 2 and text pp.18-20 (printed NBER pages)) | shift_lean uses MAXSHIFT times r/(r+HALF) with r = c times amount over receipts. MAXSHIFT defaults to 18 with a code comment citing Ansolabehere for the cut from 40. On median receipts recomputed from congestion.json (1743281, n = 517), a 10k gift shifts lean by about 0.98, below DTHRESH 11.25. Defection needs roughly 1/6 of receipts before the pressure_mult and 1.10 LOSSFRAME factors, which can lower that fraction. (`fec/engine/money_response.py:15-25 and :60-80`) | The paper estimates business-wide PAC money against a Chamber score at ordinary contribution sizes, pooled across the House. The defection channel fires only on scenario shocks near 1/6 of receipts, outside the paper's support. The member-FE null puts the within-member slope at zero, so any nonzero MAXSHIFT is owner judgment, not an estimate. The 2-vs-30 contrast is conditional on party and is not stratified; quote it with an UNSTRATIFIED label. | strong | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | qualifies | Money is a small part of a reelection-minded legislator's calculation: party moves the score by more than 30 points against about 2 for money. The paper lists buying access as one of several group motives and does not test it. It cites 3 billion dollars of lobbying against 300 million of PAC contributions in 1997-98, and Langbein's estimate of about 10000 dollars per legislator hour. Its main thesis is consumption rather than access. (pp.18-20 (votes); pp.31-33 (access, lobbying totals, 20 percent illustration)) | WM 0.35 and WP 0.65 at flip_model.py:21, with a comment that money buys access and effort more than votes, citing Ansolabehere, Hall and Wayman, and Kalla and Broockman. pflip = a/(a+b) with saturating money terms, each carrying a +1 smoothing term. (`fec/engine/flip_model.py:21 and :88-100`) | The paper supports money as the minority partner on votes, not on access. The access attribution rests on Kalla and Broockman, which is outside this paper. Peer proximity (cpf_pressure) is not party; the 30-point party effect is only the nearest analogue. With peer terms zero, pflip approaches 0 or 1 only at extreme money ratios because of the +1 smoothing. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Competitiveness multiplier c (COMPET_MULT)\|Competitiveness multiplier c (COMPET_MULT)]] | qualifies | Closeness raises the quantity of money. In the gubernatorial panel (1976-2000, N = 326, state FE) the closeness coefficient on log per-capita spending is 2.93. In House races of 1992-2000, close seats (under 5 points) draw 60 percent of funds from individuals and 31 percent from PACs, while safe seats (30 points or more) draw 48 and 46 percent. Most of the gap in the individual share comes from gifts under 500 dollars. (Table 3 (pp.26-27); The Marginal Dollar (pp.28-29)) | c = 1 + exp(-max(lean,0)/5) scales effective dollars on both money legs; receipts stay in the denominators of pflip and shift_lean. (`fec/moneyball_core.py:83-93 and fec/engine/flip_model.py:95-96`) | The paper shows the supply side only: more money, mostly from individuals, flows to close races. It says nothing about per-dollar influence. The PAC evidence is in shares, not dollar levels. Higher receipts in close seats shrink r and partly offset c. Reciprocity is not validated (D-104..D-108), and this paper does not validate it. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-allocation#Stance money gate (F1)\|Stance money gate (F1)]] | supports | With member fixed effects, changes in donations to an individual legislator do not translate into changes in that legislator's roll-call voting. A third explanation for group giving is to elect sympathetic legislators rather than to change the views of sitting ones. (p.19; pp.32-34) | STANCE_GATE_BLOCKED (Skeptic, Highly Skeptical, Opponent) is defined at moneyball_core.py:717 and applied to money in optimize.py:52 (GATED), per_topic_flip.py:37 and per_topic_coalition.py:26. The rel claim that gated members receive zero money in every allocator, and still count in the objective, was not confirmed in this pass. (`fec/moneyball_core.py:717 and fec/engine/optimize.py:52`) | The evidence is general business and labor PAC money against CCUS scores (1978-1994), not CU-specific bills or the current Congress. It supports not paying to convert opposed members. It does not measure electoral help to allies. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions\|Sector growth vs giving, partisan share and lobbying regressions]] | supports | In the gubernatorial panel (N = 326, state FE) the income elasticity of spending is 1.07 and 1.17 across Table 3 specifications, and per-capita state government spending has no independent effect (-0.09). Real campaign spending roughly doubled 1976-2000 but shows no trend relative to national income. 94 top executives at 12 large firms gave about 7500 dollars per cycle, or 51 dollars per 100000 of annual compensation (0.05 percent), against 0.04 percent of national income in 2000. (Table 3 (pp.26-27); executives and time series (pp.22-24)) | sgs_config.R lines 67-76 define the job tiers and NATIONAL_VARS (presidential and midterm year, party control, seat forecasts, GB538 margin). No income term appears there. The study's own result (no stable growth effect, no adjusted R2 gain) was not checked in this pass. (`C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_config.R:67-76`) | The paper's units are states and aggregate federal spending, not industry sectors. The consumption account predicts that employee and executive giving follows donor income rather than industry fortunes, consistent with the study's null. A real-dollar trend may partly reflect income growth. The executive figure is a share of compensation, which the paper says overstates the income share. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-giving-forecast#Cycle-aware giving refit (phase and drift log models, 50-50 growth)\|Cycle-aware giving refit (phase and drift log models, 50-50 growth)]] | qualifies | In the gubernatorial panel, campaign spending scales with per-capita income at an elasticity of 1.07 to 1.17, and government spending adds nothing once income and state FE are included. In aggregate, campaign spending shows no trend relative to national income over 1976-2000, and presidential spending as a share of GDP was flat over 1912-2000. (pp.23-27, Table 3) | giving_forecast.py lines 1393-1397 hold the cycle formula M = B exp(phi dP + d_h)(1 + x/2), where x is the one-year-lagged IBISWorld real industry growth path. The D-116 exemption (company PACs take x = 0) was not checked in this pass. (`C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py:1393-1397`) | The paper's evidence is aggregate and state-level spending, not industry-attributed giving. Industry revenue proxies firm size and stakes, not donor income, so the bearing is weak. The elasticity should be attributed to the gubernatorial panel, not to campaign spending in general. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Executive figure: 0.05 percent is a share of annual compensation, not income. The paper says this overstates the income share (p.22). Summary wording should change.
> - Summary issue: Marginal-dollar wording: the paper says most of the individual-share gap between close and safe seats (60 vs 48) comes from gifts under 500 dollars. It does not say close-race marginal dollars are mostly small gifts.
> - Summary issue: Defection threshold: the code gives about 0.98 at median receipts (1743281), not 0.9. The 1/6 boundary ignores pressure_mult and LOSSFRAME.
> - Summary issue: The consumption account 'fits' the sector growth null: the study's own result was not checked in this pass, so the sentence rests only on its config.
> - Summary issue: Alexander 2009 is described as very large lobbying returns; its text reports positive, significant returns. The contradiction is a framing, not a direct test.
> - Summary issue: The de Figueiredo 2013 link's stated point (lobbying spending dwarfs PAC giving) was not found in that text.
> - Summary issue: The summary says the paper supports the MAXSHIFT cut on votes. That holds for the direction, but the paper does not estimate MAXSHIFT, so the cut stays a calibration choice.

## Gaps and flags

- No component estimates a within-member money-to-position response. MAXSHIFT, HALF, DTHRESH and WM rest on citation, and this paper's member-FE estimate is zero. A check would be a member-FE panel of CU and finance PAC dollars against CU-relevant roll-call or cosponsor alignment, under single-look discipline.
- The competitiveness multiplier assumes per-dollar influence rises in close seats, but the only evidence here is that money supply rises (mostly small individual gifts). The effect-heterogeneity question stays untested.
- Neither the sector growth study nor the cycle model has a donor-income or national-income scale. The paper finds the spending-to-income ratio flat over 1976-2000, so a ratio-to-income baseline is the natural null.
- The system has no consumption or participation channel: every CU dollar is treated as an instrument of influence. Mentat reports employer-matched individual giving as industry giving, which on this paper's account measures participation more than industry strategy.
- Access is not modeled separately from votes, although the engine comments say that money buys access.
- Citation precision: comments at fec/engine/money_response.py:15 and fec/engine/flip_model.py:21 credit Ansolabehere for the claim that money buys access or effort. This paper lists access as one untested explanation among four or five and argues consumption. Narrow the citation to the claim that money does not buy votes.
- The 2-points-vs-30 comparison is a pooled House 1978-1994 coefficient conditional on party controls. Quote it with an UNSTRATIFIED label. The IV and FE estimates change sign and size across specifications.
- The 3 billion lobbying vs 300 million PAC figure comes from LDA-era disclosure totals. Under D-021, LDA is an identity source only, so this figure must not justify an LDA-based edge or weight.
- The 20 percent return for a subset of PACs (p.32) is an illustration, not an estimate. Do not cite it for an ROI floor.
- The extraction notes a garbled Table 2 in which one labor IV cell is starred, against the text's statement that FE effects are insignificant. Cell-level numbers stay UNVERIFIED until a second path reproduces them (DE-062).
- No SNA proposal is argued, so neither rejected proposal (dropping cosponsorship edges, per-edge volume normalization) is in play.

No Paul annotations (0 entries, 0 annotations). Melange already cites Ansolabehere for its money calibration, and the evidence agrees with MAXSHIFT 18 and WM 0.35 on votes. The paper cuts against Paul's view that money buys access and returns and against his ROI enthusiasm (Alexander 2009): it argues consumption and does not test access. It agrees with the sector growth null, which already undercuts his deep-pockets reading. His competitiveness note (no source says dollars buy more in competitive seats) still holds.

## Related papers

- [[kalla-nd-campaign-contributions-facilitate]] — complements — Randomized field experiment (Kalla and Broockman) on donor access to congressional officials. It supplies the access half of the engine's citation, which this paper only hypothesizes. Item exists.
- [[alexander-2009-lobbying-analysis-under]] — contradicts — Lobbying ROI analysis under the 2004 American Jobs Creation Act. Its text reports positive, significant lobbying returns, set against this paper's small-return account. The link is a framing contrast, not a direct test. Item exists.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Reports minimal overlap between organizations that lobby and those that make PAC expenditures. It is a check on this paper's claim that big givers also lobby heavily. Item exists.
- [[bouton-2022-small-campaign-donors]] — builds-on — Studies small individual donors, the consumer contributor this paper puts at the center of campaign finance. Item exists.
- [[bonica-2013-mapping-ideological-marketplace]] — complements — Shows ideologically sorted donors, consistent with giving as ideological participation. Item exists.
- [[fouirnaies-2022-electoral-incentives-affect]] — complements — Fouirnaies and Hall on electoral incentives in U.S. state legislatures. Bears on the competitiveness lever, but for state, not federal, legislators. Item exists.
- [[kim-2020-strategic-sequential-links]] — complements — Links donation timing to lobbying, which complicates a pure consumption reading for organized interests. Item exists.
- [[defigueiredo-2013-advancing-empirical-research]] — complements — A co-author's later review of lobbying empirics. The claim that lobbying spending dwarfs PAC giving was not found in its text, so only the agenda link stands. Item exists.
- [[note-sector-growth-and-political-spending-source]] — complements — Paul's sector growth source note. The paper's null for stakes and its income elasticity bear on the growth-giving hypothesis. Item exists.

### Linked from
- [[callaway-2025-difference-differences-continuous]] — complements — The money_response.py calibration comment cites Ansolabehere for the small MAXSHIFT, so the link is real in the code. The Ansolabehere paper text was not re-read here.
- [[fremeth-2018-spillovers-regulating-corporate]] — complements — Real item (title: Why Is There So Little Money in U.S. Politics?). Complement claim not re-read.