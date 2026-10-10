---
type: theme
name: "Campaign Politics"
paper_count: 16
tags: [theme]
---

# Campaign Politics

**Campaign Politics: literature synthesis** (16 works in the theme file, plus Paul's six notes for this category)

The works answer four separate questions. Only the first touches the engine's core money assumptions.

1. **Who gives, why, and what a gift buys.** Covered by [[bonica-2013-mapping-ideological-marketplace]], [[kalla-nd-campaign-contributions-facilitate]], [[bouton-2022-small-campaign-donors]] and [[bonica-2025-money-campaign-finance]].
2. **Money that moves outside candidate-committee receipts.** Covered by [[irvin-2023-dark-investigation-dark]], [[chohan-2025-crypto-oligarchy-cryptocurrency]] and [[anon-2025-cross-industry-political]].
3. **Whether party cues pull people toward the cue-giver or push them away.** Covered by [[bakker-nd-understanding-partisan-receptivity]] and [[merkley-nd-party-cues-news]].
4. **How campaigns persuade voters.** Covered by [[broockman-2026-should-moving-middle]], [[kalla-2023-which-narrative-strategies]], [[tappin-2023-quantifying-potential-persuasive]] and [[madsen-2017-strategic-advantages-micro]].

A fifth, smaller group is terrain and measurement infrastructure: [[report-2024-2024-house-glance]], [[mccartan-2022-simulated-redistricting-plans]] and [[porter-2025-campaignview-campaign-platforms]].

**The main finding for Paul's systems.** Across the theme, individual donors give on ideology, not on electoral leverage. Only organized PAC money looks like investment. A donor signal buys access to more senior staff, but nothing in the theme shows it buys votes. Nothing here validates the assumption that dollars buy more in competitive seats (reciprocity), so it stays NOT validated (D-104..D-108). The two cue studies agree that an out-party cue repels mass partisans. That is the opposite sign from the engine's positive out-party pull in cpf_pressure.

**How Paul's notes relate to the works.** Most of his notes for this category sit beside the 16 works rather than on top of them.
- His bibliography addendum lays out a submodular / adaptive-submodular / RL tier argument for cross-campaign seeding. None of the 16 works tests it.
- His sector-growth design (H1-H4: deep pockets, distress, access-seeking, stakes, lobbying tracks current control) rests on anchor papers that are not in this file, such as Cox & Magar and Grier et al.
- His contact-identification state record concludes that dollars and event timing do not identify which member was contacted.
- Two of the six source documents are citation lists with no annotation prose. Their content enters here only as the works themselves.

**Coverage limits.** Most of the evidence is about voters and donors, not legislators. Every transfer to Melange's legislator-facing engine is by analogy, and every bearing in the file is a single-agent lead unless it is marked VERIFIED (DE-062).

## Strands

### Why money is given and what it buys

Works: [[bonica-2013-mapping-ideological-marketplace]], [[kalla-nd-campaign-contributions-facilitate]], [[bouton-2022-small-campaign-donors]], [[bonica-2025-money-campaign-finance]]

**Why individuals give.** The core result is the motive test in [[bonica-2013-mapping-ideological-marketplace]]. It covers 5,000 individual donors with 25 or more federal gifts over 2004-2012. For these donors the ideological model beats both the partisan-electoral model and the investor model by log-likelihood: -1,334,200 against -1,405,855 and -1,504,601, with a baseline of -1,520,312. The investor model barely beats the baseline, and it fits better than the ideological model for only about 5% of individuals. For corporate and trade PACs the investor model is competitive. In its unconstrained form it fits better than the ideological model (-1,687,949 against -1,694,458), which is why those PACs are left out of the CFscore estimation.

**Validity of CFscores.** Bonica reports that CFscores track DW-NOMINATE at r = 0.92. That figure is pooled across parties and is UNSTRATIFIED. Within party the correlation falls to 0.56 (House Democrats) through 0.72 (Senate Republicans). Only the within-party range should be quoted when CFscores stand in for positions inside a caucus.

**Small donors.** [[bouton-2022-small-campaign-donors]] parses ActBlue and WinRed earmark memos across about 340M FEC individual contributions, 2005-2020. The results line up with Bonica's:
- Small donors (at most $200 per committee per cycle) grew to 21.8M in the 2020 cycle but supplied only 12.8% of 2020-cycle money.
- About 88% of small-donor money goes out of district.
- Small donors do not concentrate on close races. The close-seat coefficient is +0.11 pp and not significant, against +0.18 pp for large donors, for Democratic congressional candidates only, 2012-2020.
- Small-donor giving spikes on salient events, not on electoral leverage.

**Donor age.** [[bonica-2025-money-campaign-finance]] adds a demographic axis using the same DIME family of data. In the 2020 cycle the median contribution dollar comes from a 66-year-old, against a median general-election voter of 52. Each year of candidate age adds 0.21 years of average donor age (6,098 candidate-cycles, 2012-2020, district fixed effects). It is descriptive throughout.

**What a gift buys.** [[kalla-nd-campaign-contributions-facilitate]] is the only randomized evidence in the theme on this question. In 191 House offices in 2013, labeling attendees 'local campaign donors' rather than 'local constituents' raised access to chief of staff or above from 2.4% to 12.5% (p=.006; 64 donor-label offices against 127 constituent-label offices). Access to the Member personally rose from 2.4% to 7.8% (p=.07). Lower-rank staff showed no reliable difference.

**What the four works say together.** Individual money is mostly expressive. A donor identity changes which tier of staff engages. Nothing in the strand measures a vote, position or cosponsorship effect. Only nine offices cosponsored during the Kalla study, so its cosponsorship comparison is uninformative.

### Money outside candidate-committee receipts: dark money, super PACs, executive offloading

Works: [[irvin-2023-dark-investigation-dark]], [[chohan-2025-crypto-oligarchy-cryptocurrency]], [[anon-2025-cross-industry-political]]

All three works describe money that per-member receipt models never see. None of them estimates an effect.

**Nonprofit vehicles.** [[irvin-2023-dark-investigation-dark]] builds a nine-part opacity index from IRS Form 990 fields for 54 501(c)(4) economic-policy groups, using 2016 filings. Scores range from 0.8 to 8.3 on a 0-9 scale. The index fails its own validation: in a logistic regression against OpenSecrets' 2016 dark-money label (13 of 54 groups), the total index does not predict the label, and only log expenses is significant. It does document C4-to-Super-PAC grants, for example One Nation granting $21.7M to the Senate Leadership Fund in 2016. Those grants enter FEC data as receipts with no upstream path.

**Crypto super PACs.** [[chohan-2025-crypto-oligarchy-cryptocurrency]] is a narrative essay. Its figures are press-sourced or self-reported by Fairshake: $203M raised for the 2024 cycle and 253 supported candidates, with no win rate and no counterfactual. Its 'decisive influence' claims are not estimates. Its usable content is structural. Corporate-treasury money is aimed at elections, often in opposition, sometimes in primaries, and often through ads that never mention crypto.

**Executive offloading.** [[anon-2025-cross-industry-political]] is an unsigned memo dated 12/10/2025. It covers the top 10 firms by revenue in each of four industries. Its claim is that firms push giving onto executives and adapt it to executive-branch scrutiny regardless of party. Its averages have no cycle years, source or N, so every figure is UNVERIFIED with no window or denominator.

**Where they agree, and where they conflict.** All three point the same way: a growing share of politically aimed money travels through Super PACs, C4s, trade associations and executive or family vehicles. They disagree on what 'dark money' means. Irvin means non-disclosing nonprofits, and the memo means disclosed Super PAC donations. Treat the two as different measurement objects.

### Party cues: in-party pull versus out-party backlash

Works: [[bakker-nd-understanding-partisan-receptivity]], [[merkley-nd-party-cues-news]]

Two independent designs reach the same direction-of-sign result: an out-party cue moves people away from the cued position, and an in-party cue is weak.

**Survey experiments.** [[bakker-nd-understanding-partisan-receptivity]] ran four US survey experiments, 2016-2018, on issues that were not yet partisan. Out-party cues reduce support by .06 (Study 1, N=747) and .07 (Study 2, N=1,302 in the text) on a 0-1 scale. In-party cues are about +.03 and not significant. Bounded rationality gets no support. Expressive utility gets partial support (two of six three-way terms). The study 4 MTurk replication shows nothing for either model.

**Time series plus experiment.** [[merkley-nd-party-cues-news]] finds that Democratic cues in NYT and WaPo climate coverage Granger-precede Republican skepticism (chi2 7.35, p=0.03, 2 lags). Republican cues have no aggregate effect (p=0.94). The causal piece is one 2019 MTurk experiment (N=2,970). A single Democratic cue moves strong Republicans +0.06 toward skepticism on a 0-1 scale, and a polarized cue moves them +0.09. A consensus cue, with Republicans joining Democrats, produces no movement.

**Limits.** Both studies use mass partisans reading one-shot stimuli. Neither studies legislators cueing legislators, so they bound the sign of a parameter, not its size. Granger precedence shows order, not causation.

### Voter persuasion, targeting and message content

Works: [[broockman-2026-should-moving-middle]], [[kalla-2023-which-narrative-strategies]], [[tappin-2023-quantifying-potential-persuasive]], [[madsen-2017-strategic-advantages-micro]]

The four works split along two axes: whom to contact, and what to say.

**Pooled effects hide offsetting strata.** [[broockman-2026-should-moving-middle]] is a pre-registered conjoint with 6,000 verified 2024 voters. Pooled, a move to the elite middle gains Democrats +1.1 pp and Republicans +1.8 pp. The pooled figure hides offsetting strata:
- Republicans gain +3.2 pp in the most liberal-opinion tercile.
- Republicans lose -2.4 pp on issues where a majority already backs the party's position.
- Voters who hold the party's position punish the move: -3.6 pp among holders of the Republican position.

A partisan midpoint therefore marks a cross-pressured voter, not a voter who is persuadable in general.

**Targeting.** [[tappin-2023-quantifying-potential-persuasive]] used a February 2022 Lucid sample (5,284 analyzed). In Study 1, microtargeting beat a single best message, 5.96 pp against 3.48 pp, precision-weighted over two modules. Almost all of that came from one module, the Citizenship Act (5.17 against 2.41), and the UBI module was not significant. Targeting on one covariate did as well as targeting on four or more. In Study 2, across 36 issues, microtargeting did not beat one best issue ad (12.30 against 15.42, P=0.11). The authors call the advantage highly context dependent and probably overstated by forced exposure.

**Simulation.** [[madsen-2017-strategic-advantages-micro]] is a 10,000-agent ABM with no network. Targeted contact wins at equal reach, and a random campaign with about 2x the reach cancels the advantage. It reports one figure, with no intervals and no sensitivity analysis, so its outputs are simulation outputs, not estimates.

**Content.** [[kalla-2023-which-narrative-strategies]] moves the question from whom to what. In field RCTs, a perspective-getting-only script of 7.3 minutes matched the full 17.1-minute deep canvass (95% CI on the difference -0.05 to 0.09 SD; about 2,050 per arm in CA and TN, 2020). Effects persisted at least 11 weeks. All outcomes are issue attitudes. Vote choice and turnout were not measured.

**Where they agree.** Persuasion depends on the issue, the receiver's own position and the message. A single scalar for persuadability throws that structure away.

### Electoral terrain and measurement infrastructure

Works: [[report-2024-2024-house-glance]], [[mccartan-2022-simulated-redistricting-plans]], [[porter-2025-campaignview-campaign-platforms]]

These are data sources, not tests.

**Cook House table.** [[report-2024-2024-house-glance]] is Cook's House table as of September 12, 2024. Its main lesson is that PVI and rating diverge:
- FL-27 is EVEN and rated Solid R, while CO-08 and NE-02 are EVEN and rated Toss Up.
- AZ-04 and OR-05 are both D+2, and are rated Solid D and Toss Up respectively.
- The 25 Toss Ups span R+8 to D+5.

The rating carries incumbent and candidate factors that PVI does not. The printed rating counts sum to 436 against 435 seats, and MA-01 has a #REF! cell, so the tallies are approximate.

**Redistricting ensembles.** [[mccartan-2022-simulated-redistricting-plans]] supplies 5,000 SMC-simulated 2020-cycle plans per state. They give a map-dependence baseline that a single PVI point per seat ignores. They cover 2020-cycle maps only, not the 2025-26 mid-decade redraws.

**Candidate platforms.** [[porter-2025-campaignview-campaign-platforms]] covers House primary candidates in 2018-2022: 5,228 of 6,016 ballot-eligible candidates (86.9%) and 43,465 platform points, hand-coded into 14 topics (Cohen kappa 0.84 on a 20% relabel sample). It gives candidate-level challenger positions. Missingness is not random: incumbents and better-funded candidates are less often missing.

**Coverage gap.** All three are House-centred.

## Agreements and debates

### 1. Do donors give for ideology or for access, and does it matter for competitive seats?

Two works agree that individual donors give on ideology:
- In [[bonica-2013-mapping-ideological-marketplace]], the ideological model wins for 5,000 individual donors (2004-2012). The partisan-electoral model, the one that would make competitive seats attract money, loses to it.
- [[bouton-2022-small-campaign-donors]] finds the same on another population. Small donors show no significant close-seat concentration (+0.11 pp, Democratic congressional candidates only, 2012-2020).

**Tension.** [[kalla-nd-campaign-contributions-facilitate]] shows that being known as a donor buys senior-staff access (2.4% to 12.5% for chief of staff or above, 191 offices, 2013). The two findings are reconcilable. A gift can be ideologically motivated and still yield access.

**The PAC side.** Bonica finds the investor model competitive for corporate and trade PACs. An online scan adds outside support that the theme file does not hold. Fouirnaies & Hall (2014, JOP) attribute the financial incumbency advantage mainly to access-oriented groups from heavily regulated industries. Myers, Silfa, Fouirnaies & Hall (2025) report that this advantage fell 25-50% among individual donors over the last decade while holding steady or rising among corporate PACs. These are online-scan leads, not vault works, and are unread here.

**Net reading.** Organized bank and CU PAC money is the mixed-motive class. No work in the theme shows dollars buying more in competitive seats.

### 2. What counts as competitiveness?

[[report-2024-2024-house-glance]] shows Cook ratings and Cook PVI departing at the same lean, for example EVEN-PVI seats rated anywhere from Solid R to Toss Up. [[mccartan-2022-simulated-redistricting-plans]] shows that partisan metrics depend on the map and on which statewide elections exist. Neither supports a PVI-only competitiveness measure. Neither tests reciprocity.

### 3. Sign of out-party cues

[[bakker-nd-understanding-partisan-receptivity]] and [[merkley-nd-party-cues-news]] agree that out-party cues repel and in-party cues are weak. They also have internal disagreements:
- Bakker's MTurk replication (study 4) is null.
- Merkley's consensus cue produces no movement, so it neither rallies nor depolarizes.

Both use mass-public samples, so neither settles legislator-to-legislator behaviour.

### 4. Does targeting pay?

- [[madsen-2017-strategic-advantages-micro]] (simulation) says targeting wins at equal reach but loses to 2x reach.
- [[tappin-2023-quantifying-potential-persuasive]] (experiment) finds a real advantage in one module, none in another, and no advantage across 36 issues (MT 12.30 against SBM 15.42, P=0.11).
- [[broockman-2026-should-moving-middle]] implies that any pooled effect hides offsetting strata.
- [[kalla-2023-which-narrative-strategies]] puts the leverage in message content.

The online scan also surfaced Kalla & Broockman (2018, APSR, 49 field experiments). It estimates a zero average persuasive effect of campaign contact in general elections, except for unusually unpopular positions or very early contact that decays. That is the strongest counterweight to any canvass-persuasion term, and it is not in the vault.

### 5. Definitions and deduplication

- **Dark money.** [[irvin-2023-dark-investigation-dark]] uses it to mean non-disclosing nonprofits. [[anon-2025-cross-industry-political]] uses it to mean disclosed Super PAC donations.
- **Earmark duplicates.** [[bouton-2022-small-campaign-donors]] pairs and drops 87% of conduit/recipient earmark duplicates. Mentat counts both legs under ruling R8. This is FLAGGED only. Money collection is frozen, and any change is Paul's per-change call.

## Methods

**Methods that dominate**

- **Descriptive or data-descriptor work, about half the theme:** [[report-2024-2024-house-glance]], [[porter-2025-campaignview-campaign-platforms]], [[mccartan-2022-simulated-redistricting-plans]], [[irvin-2023-dark-investigation-dark]], [[chohan-2025-crypto-oligarchy-cryptocurrency]], [[anon-2025-cross-industry-political]], and most of [[bonica-2025-money-campaign-finance]] and [[bouton-2022-small-campaign-donors]]. The weakness is that none can carry a causal or calibration claim. Chohan and the memo have no comparison group. The memo's figures have no window, source or N. Irvin's index fails its own external validation.
- **Randomized experiments.** One elite field RCT: [[kalla-nd-campaign-contributions-facilitate]], 191 offices, with randomization inference. Voter field RCTs: [[kalla-2023-which-narrative-strategies]]. Survey and conjoint experiments: [[broockman-2026-should-moving-middle]], [[tappin-2023-quantifying-potential-persuasive]], [[bakker-nd-understanding-partisan-receptivity]], and the experimental arm of [[merkley-nd-party-cues-news]].
  - Internal validity is high.
  - Most subjects are members of the public, so carrying the results over to legislators is by analogy only.
  - Forced exposure inflates survey effects, as Tappin concedes.
  - Outcomes are attitudes or access, never votes. Kalla-nd's treatment is a label, not money.
- **Scaling and linkage.** [[bonica-2013-mapping-ideological-marketplace]] runs correspondence analysis on a contributor-by-recipient matrix. Its pooled r=0.92 against DW-NOMINATE is UNSTRATIFIED, and within-party correlations are 0.56-0.72. Its record-linkage appendix reports merge errors that joined family members at the same employer.
- **Panel and fixed-effects regression.** Bonica-2025 uses two-way FE on donor-candidate pairs. Bouton uses donor-FE linear probability models, a border-county TV design, and a heterogeneity-robust event study. Two-way FE estimates are associations, not causal effects. Bouton's close-race result is for Democrats only.
- **Time-series precedence.** Merkley uses VAR and Granger tests. Precedence is not causation, and the series are short (N=55 quarters).
- **Simulation.** [[madsen-2017-strategic-advantages-micro]] is an ABM with one figure and no sensitivity analysis. Mccartan's SMC sampler reports proper convergence diagnostics, with Gelman-Rubin R of about 1.05 or less.

**Two cross-cutting weaknesses**

1. **The legislator gap.** The only legislator-side causal design is Kalla-nd, and its outcome is meeting seniority.
2. **The outcome gap.** No work measures a vote, cosponsorship or position change caused by money or messaging.

**Paul's own design.** His sector-growth design flags the same fragility in its own setting: about 9 elections, 5 clusters with wild-cluster bootstrap, and reverse causality in same-year growth.

## How the works relate

- [[bonica-2025-money-campaign-finance]] — same-data — [[bonica-2013-mapping-ideological-marketplace]]: Both rest on DIME contribution records; the 2025 paper uses DIME v4.0 and builds on the 2013 data family.
- [[bouton-2022-small-campaign-donors]] — builds-on — [[bonica-2013-mapping-ideological-marketplace]]: Bouton et al. use Bonica's CFscores for candidate ideology terciles (citing the 2014 AJPS version).
- [[bouton-2022-small-campaign-donors]] — agrees-with — [[bonica-2013-mapping-ideological-marketplace]]: Both find individual giving is not electorally strategic. Bonica: the partisan-electoral model loses to the ideological one. Bouton: small donors show no significant close-seat concentration (Democratic congressional candidates, 2012-2020).
- [[bonica-2025-money-campaign-finance]] — cites — [[kalla-nd-campaign-contributions-facilitate]]: Bonica and Grumbach cite the donor-access field experiment (p.3) for their link from donating to access.
- [[bouton-2022-small-campaign-donors]] — cites — [[kalla-nd-campaign-contributions-facilitate]]: Bouton et al. cite Kalla and Broockman (2016) on elite responsiveness to donors in their introduction.
- [[bouton-2022-small-campaign-donors]] — same-data — [[bonica-2025-money-campaign-finance]]: Both use FEC itemized individual contributions and both find donors demographically unrepresentative: older in Bonica-2025, richer than the population in Bouton.
- [[kalla-nd-campaign-contributions-facilitate]] — in-tension — [[bonica-2013-mapping-ideological-marketplace]]: A donor label buys senior access, yet individual donors fit the ideological model better than the investor model. The two are reconcilable: access can accrue to ideologically motivated giving.
- [[kalla-2023-which-narrative-strategies]] — same-authors — [[broockman-2026-should-moving-middle]]: Same authors, Kalla and Broockman. One asks which door conversation moves attitudes; the other asks when candidate moderation pays.
- [[kalla-2023-which-narrative-strategies]] — same-authors-distinct-study — [[kalla-nd-campaign-contributions-facilitate]]: Same authors, different studies. The money-access citation in flip_model.py is the field experiment, not the canvass paper; keep the two distinct.
- [[broockman-2026-should-moving-middle]] — agrees-with — [[tappin-2023-quantifying-potential-persuasive]]: Both find persuasion returns are context dependent and hidden by pooled averages (by issue and voter position in Broockman; by module and issue in Tappin).
- [[madsen-2017-strategic-advantages-micro]] — partially-contradicts — [[tappin-2023-quantifying-potential-persuasive]]: Madsen's simulation says targeting wins at equal reach. Tappin's Study 2 finds no targeting advantage across 36 issues (MT 12.30 vs SBM 15.42, P=0.11). Tappin is the empirical check on Madsen's simulated claim.
- [[kalla-2023-which-narrative-strategies]] — complements — [[tappin-2023-quantifying-potential-persuasive]]: Tappin addresses whom to target and Kalla-2023 addresses what to say; together they separate audience from message.
- [[bakker-nd-understanding-partisan-receptivity]] — agrees-with — [[merkley-nd-party-cues-news]]: Independent designs (survey experiments; news-content time series plus an MTurk experiment) both find out-party cues repel and in-party cues are weak.
- [[broockman-2026-should-moving-middle]] — qualifies — [[bonica-2013-mapping-ideological-marketplace]]: Cross-pressure across issues warns against reading a single-axis CFscore as a position on any particular issue.
- [[porter-2025-campaignview-campaign-platforms]] — complements — [[bonica-2013-mapping-ideological-marketplace]]: Candidate positions from platform text, against candidate positions inferred from donor money; the two could cross-validate.
- [[report-2024-2024-house-glance]] — complements — [[mccartan-2022-simulated-redistricting-plans]]: Cook PVI is a single partisan point per seat; McCartan's ensembles show partisan metrics depend on the map and on the elections available.
- [[irvin-2023-dark-investigation-dark]] — conflicting-definitions — [[anon-2025-cross-industry-political]]: Irvin's 'dark money' means non-disclosing 501(c)(4)s; the memo uses it for disclosed Super PAC donations.
- [[irvin-2023-dark-investigation-dark]] — complements — [[chohan-2025-crypto-oligarchy-cryptocurrency]]: Both describe politically aimed money outside candidate-committee receipts: C4 grants to Super PACs (Irvin) and the crypto super PAC network (Chohan).
- [[anon-2025-cross-industry-political]] — complements — [[chohan-2025-crypto-oligarchy-cryptocurrency]]: Both describe giving migrating into Super PAC and executive channels. Both are descriptive with no comparison group.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — [[kalla-nd-campaign-contributions-facilitate]]: Chohan asserts decisive electoral influence without an estimate; Kalla-nd's randomized evidence covers access only, not outcomes.
- [[bonica-2025-money-campaign-finance]] — complements — [[irvin-2023-dark-investigation-dark]]: Bonica-2025 covers itemized individual contributions only and excludes dark money; Irvin covers the excluded nonprofit channel.

## What it means for the systems

Bearings below are single-agent leads unless marked VERIFIED in the file (DE-062). Every recalibration named here is Paul's decision.

### Melange influence engine ([[systems/melange-influence-engine]])

- **Competitiveness multiplier (COMPET_MULT, c = f(PVI)).** [[report-2024-2024-house-glance]] shows Cook's own ratings departing from PVI at the same lean. COMPET_MULT also neutralizes the receptivity layer's candidate-specific competitiveness term, which came from Ballotpedia vote results. So, as the code stands, a safe EVEN-PVI incumbent such as FL-27 scores the same as a true Toss Up. [[mccartan-2022-simulated-redistricting-plans]] could supply a map-dependence baseline, but only for 2020-cycle maps. [[bonica-2013-mapping-ideological-marketplace]] and [[bouton-2022-small-campaign-donors]] both find individual money does not chase close seats. Nothing in the theme validates reciprocity, which remains NOT validated (D-104..D-108; see [[systems/melange-validation]]).
- **Flip model.** flip_model.py:21 cites Kalla & Broockman for 'money buys access/effort > votes' and labels the 0.35/0.65 money/peer split CALIBRATED. [[kalla-nd-campaign-contributions-facilitate]] supports the access half of that line only. It cannot calibrate the money/peer split or anything about votes. The flip model's money term counts direct bank and finserv PAC receipts, so it is blind to the Super PAC, C4 and executive channels that [[chohan-2025-crypto-oligarchy-cryptocurrency]], [[irvin-2023-dark-investigation-dark]] and [[anon-2025-cross-industry-political]] describe. Because individual donors give on ideology (Bonica 2013), any test of whether money moves members needs an ideology control.
- **Cascade kernel (cpf_pressure), LT-priming and salience shock.** [[bakker-nd-understanding-partisan-receptivity]] and [[merkley-nd-party-cues-news]] give the opposite sign to cpf_pressure's positive out-party weight (x0.5). Out-party cues repel. The in-party effect is also much weaker than x1.7 implies. The LT-priming cross-cue clamp at 0 is closer to the evidence. The salience-shock Retreat regime (out-partisans -1) fits. Its Rally regime (+0.5 for bridge-builders) is not supported by Merkley's consensus-cue null. All of this evidence comes from the mass public, so it checks the direction of sign, not the constants.
- **Persuadability table and overall tier.** [[broockman-2026-should-moving-middle]] warns that one scalar of persuadability averages over offsetting issue-level responses. This holds for Melange only by analogy.
- **Electoral plays.** [[porter-2025-campaignview-campaign-platforms]] could replace the same-party average CU-score proxy with real challenger platforms, for House seats in 2018-2022 only. [[chohan-2025-crypto-oligarchy-cryptocurrency]] confirms that industry money contests primaries in practice.
- **Paul's RL / submodular addendum (my reading, UNVERIFIED).** The greedy 1-1/e guarantees assume monotone spread. If out-party cues repel among legislators, as they do among mass partisans, adding a seed can lower spread. That would put the cross-party part of the problem outside the submodular regime for a different reason than complex contagion. None of the 16 works tests Paul's complex-contagion hypothesis for coalition formation. (Related: [[systems/melange-allocation]], [[systems/melange-pathfinding]].)

### Mentat ([[systems/mentat-fec-attribution]], [[systems/mentat-sector-growth-study]])

These are flags, not recommendations. Receipt-counting is frozen without Paul's per-change OK.
- **Earmark duplicates.** [[bouton-2022-small-campaign-donors]] pairs and drops 87% of conduit/recipient earmark duplicates. Mentat counts both legs under R8.
- **Identity matching.** Bouton requires a street address for donor grouping. Mentat's identity ladder auto-accepts exact name plus ZIP. [[bonica-2013-mapping-ideological-marketplace]] documents merge errors that joined family members at the same employer. [[bonica-2025-money-campaign-finance]] uses precision-first linkage.
- **Conduit coverage.** Conduit coverage changes over time (91.1% of contributions in 2020; WinRed only from 2019). Cross-cycle partisan comparisons of employee giving therefore carry a visibility artifact.
- **Transfer graph.** The graph traces only FEC 24G/24K committee transfers, so C4-to-Super-PAC money (Irvin) enters as a receipt with no upstream path.
- **Sector growth study.** The Cook table is a dated source for the toughness_general control. Bouton's small-donor against large-donor close-seat contrast is a candidate design for the seats test. If the memo's executive-offloading claim holds, company-PAC-only spending measures would undercount.

### Terrier

- **Canvass layer.** Untapped potential places persuadability at 0.5 support. [[broockman-2026-should-moving-middle]] says a midpoint marks a cross-pressured voter, not a voter persuadable in general. [[tappin-2023-quantifying-potential-persuasive]] suggests a cheap message dimension: one-covariate targeting, with Terrier's own pretest. [[kalla-2023-which-narrative-strategies]] puts the effect in the script. [[madsen-2017-strategic-advantages-micro]] names the missing messenger-credibility term.
- **Row scoring.** [[bonica-2025-money-campaign-finance]] shows giving and turnout fields are steeply age-graded, and standings are not age-stratified.

### Contact inference and wording

- Paul's contact-identification record finds that dollars and timing do not identify which member was contacted. That is consistent with [[kalla-nd-campaign-contributions-facilitate]]: a donor signal changes access tier in a randomized setting, which says nothing about identifying contacts from FEC data.
- LDA remains an identity source, not an edge or weight source (D-021).
- Graph ranks are heuristic model outputs, never access probabilities (D-025).

## Gaps

### What is missing

1. **Organizational access-money.** The theme holds only one causal study of what money buys, [[kalla-nd-campaign-contributions-facilitate]], and its treatment is a label. It holds nothing on PAC access logic. A brief online scan suggests these anchors (leads only, not read):
   - Fouirnaies & Hall (2014, JOP, financial incumbency advantage). An RD design attributes a 20-25 pp shift in donation share mainly to access-oriented donors from regulated industries.
   - Myers, Silfa, Fouirnaies & Hall (2025). The incumbency advantage fell 25-50% among individual donors while holding or rising among corporate PACs.
   - Powell's 'Money in Exile'. Regulated-sector PACs drop members exiled from committees.

   These bear directly on bank and CU PAC behaviour and on Paul's H2 (access-seeking).
2. **The sector-growth anchor papers are not in the theme.** Cox & Magar, Snowberg et al., Grier-Munger-Roberts, Kerr-Lincoln-Mishra, de Figueiredo & Richter and Ansolabehere et al. are named in Paul's design notes but are not works in this theme. The sign of the performance-spending relation (his Q10) has no evidence here.
3. **Null persuasion effects.** Kalla & Broockman (2018, APSR, 49 field experiments) estimate a zero average persuasive effect of campaign contact in general elections. Any Terrier persuasion term should be checked against it. It is not in the vault.
4. **Legislator-side evidence.** There are no legislator-side cue or persuasion experiments, and no work measures a vote, cosponsorship or position change caused by money or campaign messaging.
5. **Effects of outside spending.** Nothing estimates the electoral effect of Super PAC or crypto spending. Chohan's 'decisive' claims stay unestimated, and Fairshake's 2024 cycle is a structural break with no comparison series.
6. **Senate coverage.** The Cook table and CampaignView are House only.
7. **Current maps.** Redistricting evidence stops at 2020-cycle maps. The 2025-26 mid-decade redraws flagged in COMPET_MULT are uncovered.
8. **Paul's three open hypotheses** from the addendum are untested by any work here:
   - whether coalition formation in Congress is a complex contagion;
   - whether relationship and persuasion costs can be learned by probing;
   - whether probabilistic lobbying commitment changes optimal seeding.

### Errors in the sources

- [[kalla-2023-which-narrative-strategies]] was published in AJPS in 2023, so the vault's 2020 year and OSF DOI were wrong.
- [[kalla-nd-campaign-contributions-facilitate]] is undated in the vault. Bouton cites it as 2016. Its Table 1 labels 56.7%/51.6% as 'No Meeting', which reverses the text's meaning.
- [[bonica-2013-mapping-ideological-marketplace]] is the SSRN draft. Downstream papers cite the 2014 AJPS version.
- The Cook rating counts in [[report-2024-2024-house-glance]] sum to 436 against 435 seats, and MA-01 has a #REF! cell.
- The Senate flip that Paul's sector-growth notes date to 2020 took effect in January 2021, after the Georgia runoffs.

## All works

- [[report-2024-2024-house-glance]] (2024) — 2024 House At-A-Glance
- [[broockman-2026-should-moving-middle]] (2026) — Should Moving to the Middle Win Candidates Votes? It Depends Where Voters Are
- [[kalla-2023-which-narrative-strategies]] (2023) — Which Narrative Strategies Durably Reduce Prejudice? Evidence from Field and Survey Experiments Supporting the Efficacy of Perspective-Getting
- [[bonica-2025-money-campaign-finance]] (2025) — Old money: Campaign finance and gerontocracy in the United States
- [[bonica-2013-mapping-ideological-marketplace]] (2013) — Mapping the Ideological Marketplace
- [[kalla-nd-campaign-contributions-facilitate]] () — Campaign Contributions Facilitate Access to Congressional Officials: A Randomized Field Experiment
- [[irvin-2023-dark-investigation-dark]] (2023) — How Dark Is It? An Investigation of Dark Money Operations in U.S. Nonprofit Political Advocacy Organizations
- [[bakker-nd-understanding-partisan-receptivity]] () — Understanding Partisan Cue Receptivity: Tests of Predictions from the Bounded Rationality and Expressive Utility Perspectives
- [[anon-2025-cross-industry-political]] (2025) — Cross-Industry Political Spending - Analysis Overview (memo)
- [[merkley-nd-party-cues-news]] () — Party Cues in the News: Democratic Elites, Republican Backlash and the Dynamics of Climate Skepticism
- [[tappin-2023-quantifying-potential-persuasive]] (2023) — Quantifying the potential persuasive returns to political microtargeting
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] (2025) — Crypto-Oligarchy: The Cryptocurrency Lobby in American Politics
- [[madsen-2017-strategic-advantages-micro]] (2017) — The strategic advantages of micro-targeted campaigning: A proof of principle Bayesian Agent-Based Model
- [[porter-2025-campaignview-campaign-platforms]] (2025) — CampaignView, a database of policy platforms and biographical narratives for congressional candidates
- [[mccartan-2022-simulated-redistricting-plans]] (2022) — Simulated redistricting plans for the analysis and evaluation of redistricting in the United States
- [[bouton-2022-small-campaign-donors]] (2022) — Small Campaign Donors
