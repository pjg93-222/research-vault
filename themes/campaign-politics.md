---
type: theme
name: "Campaign Politics"
paper_count: 15
tags: [theme]
---
# Campaign Politics

**Campaign Politics** has 15 works in the file. They fall into five groups: (1) why people give and what a donation buys; (2) money that moves outside candidate-level PAC counts; (3) how partisan cues land, including backlash; (4) how voters are persuaded and targeted; (5) the measurement base for elections (race ratings, simulated district maps, candidate-platform text). Most are recent (2020-2026). Two classics anchor the money side: the CFscore paper [[bonica-2013-mapping-ideological-marketplace]] and the donor-access field experiment [[kalla-nd-campaign-contributions-facilitate]].

**Paul's notes for this category are thin.**
- Two of the six documents attached to the theme are bare citation lists with no annotation prose (sha12 1091449c88da and ed1fc0f63ea9). Paul's views on those entries are missing. They are not "no view".
- The other four documents are design or state records, not annotations of these 15 works:
  - the RL influence-maximization addendum (one-shot greedy, then adaptive greedy, then an RL envelope; congressional coalition formation as a possible complex contagion);
  - the sector growth-and-spending design (H1-H4, Cox & Magar majority-status logic, a Cook-rating toughness control);
  - an annotated list on text-to-network methods;
  - the LDA contact-identification state record.
- Where those notes bear on the works here, this synthesis uses them and says so.

**Bottom line for the systems.** These works mostly *qualify* existing engine choices. None calibrates a constant.
- The donor-access experiment backs only the access half of the flip model's "money buys access/effort > votes" comment. It says nothing about the 0.35/0.65 money/peer split.
- Two cue studies, both on the mass public, show an out-party cue pushing receivers *away*. That conflicts in sign with `cpf_pressure`'s positive x0.5 out-party pull. This is a direction-of-sign check on non-legislators. Any recalibration is Paul's ruling.
- Cook's own 2024 table, Bonica's motive test and the redistricting ensembles all show that a PVI-only competitiveness multiplier leaves things out. None of them tests reciprocity (whether dollars buy more in competitive seats). Reciprocity stays NOT validated (D-104..D-108).
- The voter-persuasion strand bears on Terrier's canvass scoring, not on Melange's legislator engine.

**Bearing status.** Bearings marked UNVERIFIED are single-agent leads, not findings (DE-062). They cover all of [[kalla-2023-which-narrative-strategies]], [[madsen-2017-strategic-advantages-micro]] and [[mccartan-2022-simulated-redistricting-plans]].

## Strands

### Money, motive and access: why people give and what a donation buys
**Who gives, and why: [[bonica-2013-mapping-ideological-marketplace]].** Bonica scales more than 100M federal and state contribution records (1979-2012) by correspondence analysis into common-space CFscores.
- Pooled across parties, candidate CFscores track DW-NOMINATE at r = 0.92. That figure is UNSTRATIFIED over the party fault line. Within party it falls to 0.56 (House D), 0.66 (House R), 0.64 (Senate D) and 0.72 (Senate R).
- The motive test is what matters for the systems. It uses 5,000 individual donors with 25 or more federal gifts, 2004-2012. By log-likelihood the ideological model (-1,334,200) beats the partisan-electoral model (-1,405,855) and the investor model (-1,504,601). The investor model barely beats the baseline (-1,520,312).
- For corporate and trade PACs the investor model is competitive with the ideological one, and in its unconstrained form it fits better (Table 2). That is why those PACs are excluded from estimation.
- So for individuals, the partisan-electoral motive Paul highlighted (give where seats are competitive) *loses* to ideology. Bank and CU PAC money sits in the mixed-motive class.

**Who the donors are: [[bonica-2025-money-campaign-finance]].** This paper uses the same DIME data family, linked to the L2 voter file.
- Donors are much older than the electorate. In the 2020 cycle the median contribution dollar comes from a 66-year-old and the median donor is 59. The median general-election voter is 52 and the median non-voter is 39. Donors aged 40 or under give 9% of dollars.
- Within districts, each year of candidate age adds about 0.21 years to average donor age (2012-2020, N = 6,098 candidate-cycles, district and cycle FE).
- The design is descriptive plus two-way FE, and the authors disclaim any causal effect.

**What a donation buys: [[kalla-nd-campaign-contributions-facilitate]].** This is the only randomized money study in the theme.
- Design: 191 House offices of one party, none of which had cosponsored the bill, June-August 2013. In 64 offices the meeting request called attendees "local campaign donors"; in 127 it called them "local constituents".
- A chief of staff or more senior official attended in 12.5% of donor-label offices and 2.4% of constituent offices (p = .006). The Member personally attended in 7.8% vs 2.4% (p = .07, not significant at .05). Lower-ranked staff showed no reliable difference (48.4% vs 43.3%, p = .26).
- The treatment was a label, not money. The outcome is access, not votes.

**Where they agree and differ.** [[bonica-2025-money-campaign-finance]] cites the Kalla result for its donation-to-access link. The three papers are compatible. Bonica 2013 is about donor *motive*, and Kalla is about the *officeholder's response* to a donor signal. Neither shows that money changes votes.

**Paul's own notes point the same way.**
- The contact-identification record found that money channels (Mentat workbooks, FEC org-PACs, LD-203 gifts) do not help identify which member was contacted.
- It binds a rule that donations are co-occurring evidence and never take causal verbs (H3).
- His sector growth design builds H2 (access-seeking: the Republican share of giving rises with expected Republican control) on the Cox & Magar logic that majority status carries access. That is consistent with Kalla's access finding. The Cox & Magar paper itself is not among this theme's works.

### Outside money, channel-shifting and opacity
All three works describe political money moving into channels that candidate-level PAC counts never see: 501(c)(4) grants, super PACs, and executive or family vehicles. None of them estimates an effect.

**[[irvin-2023-dark-investigation-dark]]** builds a 0-9 opacity index from nine IRS Form 990 and website components, applied to 54 501(c)(4) economic-policy groups using 2016 filings.
- Scores ran from 0.8 to 8.3. One Nation spent $63.3M in 2016 and granted $21.7M to the Senate Leadership Fund.
- Validation is weak. 13 of the 54 groups appear on OpenSecrets' 2016 dark-money list, but the total index does not predict that label once controls are added. Only log expenses is significant.

**[[chohan-2025-crypto-oligarchy-cryptocurrency]]** is a qualitative essay on Fairshake. It reports $203M raised for the 2024 cycle and 253 supported candidates, both self-reported or press-sourced, with no win rate and no counterfactual. Its 'decisive influence' claim is not estimated.

**[[anon-2025-cross-industry-political]]** is an unsigned internal memo dated 12/10/2025.
- It covers the top 10 firms by revenue in four industries.
- Its per-industry dollar averages carry no cycle window, source or N. Example: commercial-bank CEO average $16,781, executive average $125,925, window and denominator not stated. Every figure is UNVERIFIED.
- It may share lineage with Mentat's own cross-industry sheet, in which case it is not an independent check.

**Agreement.** All three say the money that matters for financial and crypto industries increasingly flows outside direct PAC-to-candidate contributions, and often opposes candidates rather than supporting them.

**Disagreement.** The term "dark money" means different things. Irvin uses it for non-disclosing nonprofits. The memo uses it for *disclosed* super PAC donations.

**A conflict in Paul's own notes.** His sector growth design records Fairshake at "roughly $260 million over 2023-24" and marks that figure for checking. Chohan gives $203M for the 2024 cycle. The two figures may use different windows or definitions (raised vs spent, PAC network vs Fairshake alone). Neither should be quoted until the window and denominator are pinned.

### Partisan cues and out-party backlash
**Two independent designs on the US mass public reach the same sign result.** An out-party cue pushes receivers away from the cued position, and an in-party cue is weak.

**[[bakker-nd-understanding-partisan-receptivity]]** runs four survey experiments (2016-2018) on non-partisan issues: food irradiation and farm subsidies.
- Out-party cue: -0.06 on a 0-1 scale in Study 1 (N = 747) and -0.07 in Study 2 (N about 1,302).
- In-party cue: about +0.03 and not significant.
- Bounded rationality gets no support. Expressive utility gets partial support: two of six three-way terms are significant, and the MTurk replication (Study 4, N = 2,509) shows nothing.

**[[merkley-nd-party-cues-news]]** combines dictionary-coded NYT/WaPo climate coverage, a quarterly latent skepticism series, and a 2019 MTurk experiment (N = 2,970).
- The share of coverage carrying Democratic cues Granger-precedes aggregate skepticism (chi2 6.96, p = 0.01) and Republican skepticism (chi2 7.35, p = 0.03).
- In the experiment, a Democratic cue moves strong Republicans +0.06 on a 0-1 scale toward skepticism. A consensus cue produces no movement.
- Granger tests establish temporal ordering only, so the causal claim rests on the one MTurk experiment.

**Where they differ.** The two papers differ on how weak the in-party side is. Bakker's in-party effect is null. In Merkley's Table 2, Model 3, Republican cues are associated with Republican skepticism (0.04 per 10 points), while the Granger results for Republican cues are weak or inconsistent.

**Fit with Paul's notes.** Neither paper studies legislators. Paul's text-to-network notes ask how to treat negative sentiment in influence edges and whether reply and mention edges are negative. Both papers argue that a negative sign is real and not a weak positive. But they give mass-public magnitudes only.

### Voter persuasion and targeting
**[[broockman-2026-should-moving-middle]]** is a preregistered conjoint with 6,000 verified 2024 voters. Pooled, a move to the 'elite middle' on one of three issues gains Democrats +1.1 pp and Republicans +1.8 pp. The authors insist the pooled figure be read only with its strata:
- Republicans gain +3.2 pp where public opinion is most liberal.
- Republicans lose -2.4 pp on issues where a majority already backs the Republican position.
- Respondents who already hold the Republican position penalize the move by -3.6 pp.

Fewer than 10% answered consistently across five issues. The averages hide offsetting issue-level responses.

**[[tappin-2023-quantifying-potential-persuasive]]** comes from a Feb 2022 Lucid sample (5,284 analyzed adults).
- Study 1 favors microtargeting: 5.96 pp vs 3.48 for a single best message and 1.79 for random messages. These figures are precision-weighted over two modules, and almost all of the gain comes from the Citizenship Act module. The UBI module is not significant.
- Study 2, across 36 issues, does *not* favor targeting by issue: 12.30 vs 15.42, P = 0.11.
- Targeting on one covariate, such as party, did as well as targeting on several.

**[[kalla-2023-which-narrative-strategies]]** reports deep-canvass field RCTs. A perspective-getting-only script (7.3 min) matched the full script (17.1 min): the 95% CI on the difference ran from -0.05 to 0.09 SD, in Study 2 (CA and TN, about 2,050 per arm). Outcomes are attitudes only, with no vote or turnout. The vault slug's year (2020) is wrong: the work is AJPS 2023.

**[[madsen-2017-strategic-advantages-micro]]** is an agent-based simulation. It has 10,000 unconnected agents, one figure, and no intervals or sensitivity analysis. At equal reach, credibility-gated targeting wins. A random campaign with about 2x the reach cancels that advantage.

**Agreement.** All four say targeting effects depend heavily on context, and that one average effect is misleading.

**Disagreement.**
- Madsen predicts, without testing, that the value of targeting rises with richer voter data. Tappin's experiments find no gain beyond one covariate. Tappin's evidence is empirical, so it outranks a simulation prediction.
- Kalla and Broockman locate the effect in *what is said* (content and issue position). Tappin and Madsen locate it in *who is contacted*.

**Missing for the systems.** Terrier's untapped-potential score ([[systems/terrier-canvass]]) models only the who: persuasion peaks at 50% support. It has no message, issue-position or messenger-credibility term.

### Electoral terrain and measurement infrastructure
These are data sources, not hypothesis tests.

**[[report-2024-2024-house-glance]]** is the Cook House table dated 12 September 2024.
- It shows Cook's PVI and Cook's race rating diverging within the same table. FL-27 is EVEN and rated Solid R. CO-08 and NE-02 are EVEN and rated Toss Up. AZ-04 is D+2 and Solid D, while OR-05 is D+2 and Toss Up.
- The 25 Toss Ups span R+8 to D+5, by substring count across 435 seats.
- The rating tally sums to 436 against 435 seats, and there is a `#REF!` cell at MA-01. Treat the counts as approximate.

**[[mccartan-2022-simulated-redistricting-plans]]** releases 5,000 SMC-simulated 2020-cycle congressional plans per state. A seat's partisan lean depends on the map, and the enacted-plan outlier test needs an ensemble. Its partisan metrics are not comparable across states, and two authors disclose paid expert-witness work.

**[[porter-2025-campaignview-campaign-platforms]]** releases House primary candidates' campaign-website text for 2018, 2020 and 2022.
- Coverage: 5,228 of 6,016 eligible candidates (86.9%) and 43,465 platform points.
- Each point is hand-coded into one of 14 topics: kappa 0.84 on a 20% relabel sample of 8,584 points.
- Missingness is non-random: incumbents and better-funded candidates are less often missing.

**Together.** All three say a single scalar of seat lean or candidate position loses information: incumbent and candidate factors, map dependence, issue-level platforms.

**Paul's sector growth design** already plans a `toughness_general` control averaging Cook, Inside Elections and Sabato on a 0-3 scale. It also flags Cook's accuracy percentages (about 94% Lean, 97% Likely, 100% Solid) as claims to verify. The Cook table is a dated input for that control, but it does not validate those accuracy figures.

## Agreements and debates

**1. Strategic vs ideological giving, and what money buys.**
- [[bonica-2013-mapping-ideological-marketplace]] finds individual donors give on ideology. The partisan-electoral and investor models lose by log-likelihood (5,000 donors, 2004-2012). Corporate and trade PACs are the exception: there the investor model is competitive.
- [[kalla-nd-campaign-contributions-facilitate]] finds that a donor label raises senior-staff access. Chief of staff or above attended in 12.5% of 64 offices vs 2.4% of 127 (June-August 2013).
- These do not contradict each other. One is about motive and the other about office response. Neither supports vote-buying.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] asserts 'decisive' crypto influence on 2024 races without an estimate. Against the randomized access-only evidence, that claim is unsupported, not refuted.

**2. Does targeting pay, and on what?**
- [[madsen-2017-strategic-advantages-micro]] (simulation) says targeting wins at equal reach and loses its edge at about 2x opposing reach.
- [[tappin-2023-quantifying-potential-persuasive]] (experiment) finds a targeting gain in one of two single-issue modules, none across 36 issues, and nothing gained beyond one covariate. That directly undercuts Madsen's untested prediction that richer data raises returns.
- [[broockman-2026-should-moving-middle]] and [[kalla-2023-which-narrative-strategies]] move the question from *who* to *what*: issue position and script content drive the effect.
- The open question for Terrier is whether persuasion potential belongs to the target, the message, or the match between them.

**3. Size and sign of partisan cues.**
- [[bakker-nd-understanding-partisan-receptivity]] and [[merkley-nd-party-cues-news]] agree that out-party cues repel: -0.06 to -0.07 (Bakker) and +0.06 toward skepticism among strong Republicans (Merkley), all on 0-1 scales.
- They differ on the in-party side. Bakker finds it null. In Merkley the observational association is positive, but the Granger evidence is weak.
- Bakker's mechanism result is also fragile. Expressive utility holds in two of six three-way terms and fails in the MTurk replication.
- Both studies are mass-public and one-shot, so neither settles a legislator-to-legislator magnitude.

**4. What counts as 'dark money'.**
- [[irvin-2023-dark-investigation-dark]] uses the term for non-disclosing 501(c)(4) vehicles.
- [[anon-2025-cross-industry-political]] uses it for disclosed super PAC money.
- Any Mentat label needs one definition. Irvin's own index also fails to predict the watchdog label once controls are added.

**5. How to measure seat competitiveness.**
- [[report-2024-2024-house-glance]] shows PVI and Cook rating diverging within Cook's own table.
- [[mccartan-2022-simulated-redistricting-plans]] shows lean depends on the map.
- [[bonica-2013-mapping-ideological-marketplace]] gives no support to the premise that individual donors chase competitive seats.
- None of the three tests reciprocity, whether dollars buy more in competitive seats. That stays NOT validated (D-104..D-108).

**Flag check on rejected SNA proposals.** No work in this theme argues for dropping regular cosponsorship edges or for per-edge volume normalization. Two related notes:
- [[kalla-nd-campaign-contributions-facilitate]] shows cosponsorship to be a low-base-rate *outcome*: 9 of 191 offices cosponsored during the study. That is an outcome-design caution and does not argue against cosponsorship edges.
- Paul's contact-identification record keeps per-edge normalization on the kill ledger. It stays settled.

## Methods

**Randomized experiments** (6 of 15 works) are the strongest evidence in the theme. All but one study the mass public.
- [[kalla-nd-campaign-contributions-facilitate]] is a blocked field RCT on House offices, analyzed with randomization inference. Weaknesses: the treatment is a label, not money; one party; one issue; a single summer (2013).
- [[kalla-2023-which-narrative-strategies]] is a set of field RCTs with placebo arms. Weaknesses: intent-to-treat among people who came to the door; outcomes are attitudes, not votes; Study 2 was stopped early by COVID.
- [[broockman-2026-should-moving-middle]] is a preregistered conjoint. Weaknesses: forced choice over hypothetical 2028 pairs; the estimand is an AMCE that averages over offsetting strata.
- [[tappin-2023-quantifying-potential-persuasive]] combines GRF calibration with an experimental phase. Weaknesses: forced-exposure estimates likely overstate field returns, by the authors' own account, and candidate races were not tested.
- [[bakker-nd-understanding-partisan-receptivity]] runs survey vignettes. Weaknesses: online panels; the key three-way interactions are fragile and fail to replicate on MTurk.
- [[merkley-nd-party-cues-news]] runs an MTurk experiment. Its causal leg rests on a single study.

**Observational time series and fixed effects.**
- [[merkley-nd-party-cues-news]] uses a VAR with Granger tests (ordering, not causation). Its quarterly N is 55 and its annual N is 28.
- [[bonica-2025-money-campaign-finance]] uses two-way FE linear probability models. The usual caution about reading 2FE causally applies. Pre-2020 Republican estimates are unstable because of WinRed coverage.

**Measurement and scaling.**
- [[bonica-2013-mapping-ideological-marketplace]] uses correspondence analysis on a contributor-by-recipient matrix. It is strong on pooled validity but much weaker within party (r = 0.56-0.72 vs 0.92 UNSTRATIFIED). Record-linkage errors cluster on family members at the same employer.
- [[irvin-2023-dark-investigation-dark]] uses an equal-weight additive index. Its validation fails against the external label once controls are added.

**Simulation.**
- [[madsen-2017-strategic-advantages-micro]] is an agent-based model with fixed parameters. It reports a single figure, with no replications, intervals or sensitivity analysis, and has no voter network.
- [[mccartan-2022-simulated-redistricting-plans]] uses SMC with convergence diagnostics (Gelman-Rubin R about 1.05 or less). Its compactness constraints are untuned in most states.

**Data descriptors and grey literature.**
- [[porter-2025-campaignview-campaign-platforms]] has strong inter-coder reliability, but its missingness is non-random.
- [[report-2024-2024-house-glance]] is a ratings table with a tally error.
- [[anon-2025-cross-industry-political]] and [[chohan-2025-crypto-oligarchy-cryptocurrency]] give descriptive averages and press-sourced figures, with no comparison group, window or denominator.

**Weakness common to the theme.** Most causal evidence is about voters, not legislators. Every transfer to Melange's legislator engine is therefore by analogy and can at most check a sign.

## How the works relate

- [[bonica-2025-money-campaign-finance]] — same-data — [[bonica-2013-mapping-ideological-marketplace]]: Both rest on DIME contribution records. The 2025 paper uses DIME v4.0, the data infrastructure introduced with CFscores.
- [[bonica-2025-money-campaign-finance]] — cites — [[kalla-nd-campaign-contributions-facilitate]]: Bonica and Grumbach cite the Kalla field experiment (p. 3) as their evidence that donors get more access than non-donor constituents.
- [[bonica-2013-mapping-ideological-marketplace]] — complements — [[kalla-nd-campaign-contributions-facilitate]]: Bonica tests the donor-side investor motive without measuring access. Kalla measures the office-side access response without measuring motive. Together they separate why donors give from what donors get.
- [[kalla-2023-which-narrative-strategies]] — same-authors — [[broockman-2026-should-moving-middle]]: Both are by Kalla and Broockman. One tests which door-canvass script content persuades, the other where candidate moderation pays. Both say the effect depends on content and issue, not only on the target.
- [[kalla-2023-which-narrative-strategies]] — same-authors — [[kalla-nd-campaign-contributions-facilitate]]: These are different studies by the same authors. The flip model's money-buys-access comment cites the donor-access experiment, not the canvass persuasion study, so the two must not be conflated.
- [[tappin-2023-quantifying-potential-persuasive]] — contradicts — [[madsen-2017-strategic-advantages-micro]]: Madsen predicts, without testing, that targeting returns rise with richer voter data. Tappin's experiment finds one covariate does as well as several, and finds no advantage for issue-level targeting across 36 issues.
- [[tappin-2023-quantifying-potential-persuasive]] — complements — [[broockman-2026-should-moving-middle]]: Both find persuasion effects strongly context- and issue-dependent, with averages hiding heterogeneity.
- [[tappin-2023-quantifying-potential-persuasive]] — complements — [[kalla-2023-which-narrative-strategies]]: Tappin isolates the return to choosing the message for an audience. Kalla shows script content carries the effect. Together they separate who-to-target from what-to-say.
- [[madsen-2017-strategic-advantages-micro]] — complements — [[kalla-2023-which-narrative-strategies]]: Madsen models canvass contact abstractly. Kalla supplies field evidence on door-to-door persuasion.
- [[bakker-nd-understanding-partisan-receptivity]] — agrees — [[merkley-nd-party-cues-news]]: Both find out-party cues push mass partisans away from the cued position and in-party cues are weak. One uses survey experiments on non-partisan issues, the other news-coded time series plus an experiment on climate.
- [[irvin-2023-dark-investigation-dark]] — contradicts — [[anon-2025-cross-industry-political]]: They define dark money incompatibly. Irvin means non-disclosing 501(c)(4) vehicles. The memo means disclosed super PAC donations.
- [[irvin-2023-dark-investigation-dark]] — complements — [[chohan-2025-crypto-oligarchy-cryptocurrency]]: Both document money reaching elections through vehicles outside candidate PAC receipts. Irvin supplies an organization-level opacity measure that Chohan lacks.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — [[anon-2025-cross-industry-political]]: Both describe financial-sector money shifting into super PAC and outside channels. Both are descriptive, with no test or denominator.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — qualifies — [[kalla-nd-campaign-contributions-facilitate]]: Chohan's unestimated 'decisive influence' claim sits against randomized evidence that money-signals raise access, not votes or outcomes.
- [[anon-2025-cross-industry-political]] — complements — [[bonica-2025-money-campaign-finance]]: The memo profiles executive giving by firm. Bonica and Grumbach profile individual donors by age. Both are individual-contribution views that exclude or blur PAC money.
- [[report-2024-2024-house-glance]] — complements — [[mccartan-2022-simulated-redistricting-plans]]: Both show a single PVI point per seat leaves information out: Cook ratings diverge from PVI, and simulated ensembles show lean depends on the map.
- [[report-2024-2024-house-glance]] — complements — [[bonica-2013-mapping-ideological-marketplace]]: Cook's ratings mark competitive races. Bonica's partisan-electoral donor model, which would chase those races, loses to the ideological model for individual donors.
- [[porter-2025-campaignview-campaign-platforms]] — complements — [[broockman-2026-should-moving-middle]]: CampaignView supplies candidates' real issue-level platform positions. Broockman shows issue-level positions, not a single axis, drive voter response.
- [[porter-2025-campaignview-campaign-platforms]] — complements — [[bonica-2013-mapping-ideological-marketplace]]: They place candidates two ways: from campaign-website text by topic, and from donor networks on one ideological dimension.
- [[broockman-2026-should-moving-middle]] — qualifies — [[bonica-2013-mapping-ideological-marketplace]]: Broockman's finding that voters are cross-pressured across issues cautions against reading a one-dimensional CFscore as a position on any particular issue.
- [[madsen-2017-strategic-advantages-micro]] — complements — [[bakker-nd-understanding-partisan-receptivity]]: Both make persuasion depend on the receiver's view of the source. Madsen gates contact on source credibility. Bakker shows out-party sources repel.

## What it means for the systems

**Melange influence engine ([[systems/melange-influence-engine]], legislator side). Every item here is a qualification, not a calibration.**

- **Flip model.**
  - [[kalla-nd-campaign-contributions-facilitate]] supports only the *access* half of `flip_model.py:21`'s "money buys access/effort > votes". It says nothing about votes, positions or the 0.35/0.65 money/peer split, so the split's CALIBRATED label is not backed by this source.
  - [[anon-2025-cross-industry-political]] and [[chohan-2025-crypto-oligarchy-cryptocurrency]] add that the industry-money term sees only direct PAC receipts. It cannot see trade-association, super PAC or family-vehicle money.
- **cpf_pressure, LT-priming and salience shock.**
  - [[bakker-nd-understanding-partisan-receptivity]] carries a VERIFIED-SOFTENED *contradicts* bearing on the cascade kernel. `cpf_pressure` gives out-party sources a positive x0.5 pull, while Bakker and [[merkley-nd-party-cues-news]] find out-party cues repel. The LT-priming cross-cue clamp at 0 fits these findings better.
  - The salience shock's Rally +0.5 for bridge-builders has no support. Merkley's consensus cue produced no movement, but that null is not a direct test of Noble's rally regime.
  - All of this evidence is from the mass public. It checks direction of sign only, and any recalibration is Paul's ruling.
- **Competitiveness multiplier c (COMPET_MULT).**
  - [[report-2024-2024-house-glance]] shows PVI-only c treating safe EVEN-PVI incumbents (FL-27) like true toss-ups (CO-08, NE-02). With the dial on, c also neutralizes the receptivity layer's candidate-specific competitiveness term.
  - [[bonica-2013-mapping-ideological-marketplace]] gives no support to a donor-side 'chase competitive seats' premise.
  - [[mccartan-2022-simulated-redistricting-plans]] could supply a map-dependence baseline, but for 2020-cycle maps only. That bearing is UNVERIFIED.
  - None of this validates reciprocity, which remains NOT validated (D-104..D-108; see [[systems/melange-validation]]).
- **Electoral plays.** [[porter-2025-campaignview-campaign-platforms]] could replace the contest-primary route's same-party-average proxy with real challenger platforms (House, 2018-2022 only). [[chohan-2025-crypto-oligarchy-cryptocurrency]] documents that primary-contest plays occur in practice.
- **Seed gate and stance-money gate ([[systems/melange-stance]]).**
  - [[madsen-2017-strategic-advantages-micro]] fits the design of contacting only receptive targets. That bearing is UNVERIFIED.
  - [[bonica-2013-mapping-ideological-marketplace]] supports the stance-money gate's premise that money and stance are entangled, so any money-moves-members test needs an ideology control.
- **Overall tier ([[systems/melange-labels-and-tiers]]).** [[broockman-2026-should-moving-middle]] qualifies the single-axis reading (VERIFIED-SOFTENED).
- **Wording rules.**
  - CFscores are ideal points. Any shared-donor or shared-background score built from them is a heuristic model output, not closeness, access or introduction probability (D-025).
  - Nothing here changes D-021: LDA remains an identity source, not an edge or weight source ([[systems/melange-lobbying]]). Chohan's account of crypto's electoral channel running apart from LDA activity is consistent with that.

**Mentat ([[systems/mentat-fec-attribution]], [[systems/mentat-giving-forecast]], [[systems/mentat-sector-growth-study]]).**
- **Transfer graph.** [[irvin-2023-dark-investigation-dark]] shows 501(c)(4)-to-super PAC money entering as a receipt with no upstream path. It also shows the excluded 501c group (Paul's 2026-07-18 exclusion ruling) mixes transparent policy shops with near-shell vehicles, and that the index cannot reliably tell them apart.
- **Identity ladder.** [[bonica-2013-mapping-ideological-marketplace]] and [[bonica-2025-money-campaign-finance]] give a concrete error profile: false merges of family members at the same employer, and small-donor itemization that is complete only from 2020.
- **Giving forecast.** [[chohan-2025-crypto-oligarchy-cryptocurrency]] supports declining a crypto forecast because the series breaks in 2024.
- **Sector growth study.** [[report-2024-2024-house-glance]] is a dated input for `toughness_general`. The memo's averages carry no window or denominator and should not enter the regressions.

**Terrier ([[systems/terrier-canvass]], [[systems/terrier-row-scoring]]).**
- [[broockman-2026-should-moving-middle]] says a mid-range partisan score usually marks a cross-pressured voter, not a voter persuadable in general.
- [[tappin-2023-quantifying-potential-persuasive]] says message choice changes the effect and that coarse partisan targeting captures most of the gain.
- [[kalla-2023-which-narrative-strategies]] (UNVERIFIED) shows script content and length matter.
- [[madsen-2017-strategic-advantages-micro]] (UNVERIFIED) shows messenger credibility is the variable that decides the result.
- Untapped potential has none of these terms.
- [[bonica-2025-money-campaign-finance]] warns that Sphere and Participation standings, built on vendor giving and turnout fields, will lean older, because Terrier stratifies by registration but not by age.

**Paul's RL-IM addendum.** It asks whether congressional coalition formation is a complex contagion. The cue strand speaks to that only indirectly: out-party cues repel, so a fence-sitter's threshold could depend on the party mix of allies, not only their count. That is a hypothesis this theme does not test.

## Gaps

**Missing annotations.**
- Two of the theme's six source documents are citation-only lists with no annotation prose (sha12 1091449c88da, ed1fc0f63ea9).
- Paul's takeaways for those entries are absent, not neutral. The annotated docx versions need to be re-exported or supplied.
- The title list in ed1fc0f63ea9 names topics with no matching work among these 15: negative campaigning, sentiment analysis of campaigns, big data in campaigns, and message complexity versus political knowledge. Those entries are not yet ingested.

**Missing evidence types.**
1. **Legislators.** There is no causal evidence on legislators responding to party or peer cues. Both cue papers study the mass public, and the closer analogue, legislator cue-taking from interest-group letters, is outside this theme's file.
2. **Money and votes.** There is no causal estimate of money changing votes, positions or cosponsorship. The one randomized money study measures access only and was uninformative on cosponsorship (9 of 191 offices cosponsored).
3. **Reciprocity.** No work tests whether dollars buy more in competitive seats. The Cook table, Bonica's motive test and the redistricting ensembles all bear on measuring competitiveness, not on returns.
4. **Turnout and vote outcomes.** None of the persuasion studies measures them. Kalla's outcomes are attitudes, and Tappin and Broockman are survey-based.
5. **Credit unions and banking.** No work studies the CU or banking sector's campaign behaviour specifically. The memo is unsourced, and Paul's own sector growth design is not yet a study.
6. **Current maps.** Nothing covers mid-decade redistricting (the 2025-26 redraws). The simulated ensembles cover 2020-cycle maps only.
7. **Senate and state legislatures.** There is no state-legislative or Senate candidate-text coverage. CampaignView is House primaries in 2018-2022 only.

**Claims to verify before use.**
- **Fairshake totals.** Paul's design note says roughly $260M over 2023-24; [[chohan-2025-crypto-oligarchy-cryptocurrency]] says $203M for the 2024 cycle. Reconcile the window and definition.
- **Cook accuracy percentages** cited in the sector growth design.
- **Cook tally.** The [[report-2024-2024-house-glance]] count sums to 436 against 435 seats.
- **Slug year.** [[kalla-2023-which-narrative-strategies]] is AJPS 2023, not 2020.
- **Missing year.** [[kalla-nd-campaign-contributions-facilitate]] has no year recorded in the file.
- **Dark money.** Pick a single definition for any Mentat label.

**Unverified bearings.** All bearings for [[kalla-2023-which-narrative-strategies]], [[madsen-2017-strategic-advantages-micro]] and [[mccartan-2022-simulated-redistricting-plans]] are UNVERIFIED single-agent leads (DE-062). They need a second check before they can support any system change.

## All works

- [[report-2024-2024-house-glance]] (2024) — 2024 House At-A-Glance
- [[broockman-2026-should-moving-middle]] (2026) — Should Moving to the Middle Win Candidates Votes? It Depends Where Voters Are
- [[kalla-2023-which-narrative-strategies]] (2020) — Which Narrative Strategies Durably Reduce Prejudice? Evidence from Field and Survey Experiments Supporting the Efficacy of Perspective-Getting
- [[bonica-2025-money-campaign-finance]] (2025) — Old money: Campaign finance and gerontocracy in the United States
- [[bonica-2013-mapping-ideological-marketplace]] (2013) — Mapping the Ideological Marketplace
- [[kalla-nd-campaign-contributions-facilitate]] (n.d.) — Campaign Contributions Facilitate Access to Congressional Officials: A Randomized Field Experiment
- [[irvin-2023-dark-investigation-dark]] (2023) — How Dark Is It? An Investigation of Dark Money Operations in U.S. Nonprofit Political Advocacy Organizations
- [[bakker-nd-understanding-partisan-receptivity]] (n.d.) — Understanding Partisan Cue Receptivity: Tests of Predictions from the Bounded Rationality and Expressive Utility Perspectives
- [[anon-2025-cross-industry-political]] (2025) — Cross-Industry Political Spending - Analysis Overview (memo)
- [[merkley-nd-party-cues-news]] (n.d.) — Party Cues in the News: Democratic Elites, Republican Backlash and the Dynamics of Climate Skepticism
- [[tappin-2023-quantifying-potential-persuasive]] (2023) — Quantifying the potential persuasive returns to political microtargeting
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] (2025) — Crypto-Oligarchy: The Cryptocurrency Lobby in American Politics
- [[madsen-2017-strategic-advantages-micro]] (2017) — The strategic advantages of micro-targeted campaigning: A proof of principle Bayesian Agent-Based Model
- [[porter-2025-campaignview-campaign-platforms]] (2025) — CampaignView, a database of policy platforms and biographical narratives for congressional candidates
- [[mccartan-2022-simulated-redistricting-plans]] (2022) — Simulated redistricting plans for the analysis and evaluation of redistricting in the United States
