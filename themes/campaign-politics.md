---
type: theme
name: "Campaign Politics"
paper_count: 18
tags: [theme]
---

# Campaign Politics

**Campaign Politics: 18 works. Paul's notes cover 6 source documents: the IM-tiers addendum, two unannotated bibliography lists, the annotated network-stack list, the sector-growth study design, and the contact-identification state record.**

The theme answers three questions that Paul's systems keep asking: what campaign money buys, how electoral context should be measured, and how voters and partisans respond to messages and cues. On the first question the works mostly agree. Most federal campaign money comes from individuals and works as consumption, participation or ideological expression, not as a purchase of policy ([[ansolabehere-2002-little-money-politics]], [[bonica-2013-mapping-ideological-marketplace]], [[bouton-2022-small-campaign-donors]], [[bonica-2025-money-campaign-finance]]). The one randomized test of a money mechanism finds that money buys *access*: a donor label raised senior-staff meetings in House offices. It does not find that money buys votes ([[kalla-nd-campaign-contributions-facilitate]]). Observational roll-call evidence finds PAC effects on votes small or null once member or district fixed effects are added ([[ansolabehere-2002-little-money-politics]]).

A second group of works documents money that never passes through a candidate committee: 501(c)(4) dark money ([[irvin-2023-dark-investigation-dark]]), the crypto super-PAC complex ([[chohan-2025-crypto-oligarchy-cryptocurrency]]), and executive and trade-association channels ([[anon-2025-cross-industry-political]]). The evidence in this group is almost entirely descriptive.

A third group covers cues and persuasion, all of it measured on the mass public. Two independent designs find that an out-party cue *repels* the receiver ([[bakker-nd-understanding-partisan-receptivity]], [[merkley-nd-party-cues-news]]). Persuasion returns depend on context and are hidden by pooled averages ([[broockman-2026-should-moving-middle]], [[tappin-2023-quantifying-potential-persuasive]], [[kalla-2023-which-narrative-strategies]], [[madsen-2017-strategic-advantages-micro]]).

The last group supplies the measurement infrastructure for electoral context: Cook ratings ([[report-2024-2024-house-glance]]), redistricting ensembles ([[mccartan-2022-simulated-redistricting-plans]]), election-night identification of partisan effects ([[snowberg-2006-partisan-impacts-economy]]) and candidate platform text ([[porter-2025-campaignview-campaign-platforms]]).

What the theme means for the systems, in brief:
- **Melange flip model.** The theme supports the access half of the flip model's citation and nothing beyond it.
- **Competitiveness multiplier.** No work here validates its premise that dollars buy more in competitive seats. Reciprocity stays NOT validated (D-104..D-108).
- **cpf_pressure.** The engine's positive x0.5 out-party pressure runs against the sign found in two mass-public studies.
- **Mentat.** The works give concrete error profiles for identity linkage and conduit duplicates, and a reverse-causation warning for the SEATS-W test.
- **Terrier.** Its one-dimensional "persuadability peaks at 50% support" prior is qualified by evidence that a mixed partisan score usually marks a cross-pressured voter.

Several bearings are UNVERIFIED single-agent leads (DE-062), namely those from [[kalla-2023-which-narrative-strategies]], [[madsen-2017-strategic-advantages-micro]] and [[mccartan-2022-simulated-redistricting-plans]]. They are not findings.

Paul's own notes add a frame that the works do not supply. The sector-growth design asks whether industry performance predicts political spending (H1/H1-alt) and whether expected majority control shifts its amount and partisan direction (H2-H4). It leans on [[ansolabehere-2002-little-money-politics]] and [[snowberg-2006-partisan-impacts-economy]]. The contact-identification record concludes that money channels do not identify lobbying contacts. It holds that donations are only co-occurring evidence and must never carry causal verbs. Two of Paul's bibliography source files are bare citation lists with no annotations, so his own takeaways for much of this category are missing (see gaps).

## Strands

### Why money is given: consumption, ideology and the donor pool

These four works agree that the main federal money stream is individual and expressive, not investment.

**[[ansolabehere-2002-little-money-politics]]** restates the Tullock puzzle and answers it with consumption. In the 1999-2000 cycle, individuals gave about $2.4B of about $3B in campaign money. Only 4% of PAC gifts to House and Senate candidates were at or near the $10,000 limit. Giving also scales with income: in the state panel, the gubernatorial income elasticity is about 1.1 and the stakes have no effect.

**[[bonica-2013-mapping-ideological-marketplace]]** tests donor motives directly. The sample is 5,000 individual donors with 25 or more federal gifts, 2004-2012, compared by log-likelihood. The ideological model beats both the partisan-electoral model and the investor model, and the investor model barely beats the baseline. For corporate and trade PACs the investor model is competitive with the ideological one, and that is why Bonica excludes those PACs from scaling. A pooled CFscore-to-DW-NOMINATE correlation of r = 0.92 is UNSTRATIFIED. Within party it falls to 0.56-0.72 (House D 0.56, House R 0.66, Senate D 0.64, Senate R 0.72). So the pooled figure is mostly party separation, and it should never be quoted alone.

**[[bouton-2022-small-campaign-donors]]** extends the consumer-donor picture to sub-$200 donors by parsing ActBlue and WinRed earmarks (about 340M FEC individual contributions, 2005-2020). Small donors grew from 5.0M in the 2006 cycle to 21.8M in 2020 but supplied only 12.8% of all 2020-cycle contributions. About 88% of small-donor money goes out of district. They do not concentrate on close seats: the close-seat coefficient is +0.11 pp (n.s.) against +0.18 pp for large donors (Democratic congressional candidates, 2012-2020 only).

**[[bonica-2025-money-campaign-finance]]** adds the age gradient. In the 2020 cycle the median contribution dollar came from a 66-year-old and the median donor was 59, against a median general-election voter of 52. Donors aged 40 or younger supplied 9% of dollars. Within districts, each year of candidate age goes with about 0.21 years of average donor age (N = 6,098 candidate-cycles, 2012-2020).

**Where they differ:** the competitive-seat question.
- Ansolabehere's House cross-section finds that closeness raises race spending.
- Bouton finds that individual small donors do not steer toward close races, and that large donors do so only weakly.
- Bonica finds the partisan-electoral motive loses to ideology for individuals.

These are different estimands (total race spending, donor share, donor-level motive), so they are not in direct contradiction. None of them tests whether a dollar buys *more* in a competitive seat.

**Data lineage:** Bonica 2025 and Bouton both run on DIME or CFscores built in Bonica 2013. Both also flag the same conduit hazard: WinRed exists only from 2019, so pre-2020 Republican small-dollar estimates are unstable.

Works: [[ansolabehere-2002-little-money-politics]], [[bonica-2013-mapping-ideological-marketplace]], [[bouton-2022-small-campaign-donors]], [[bonica-2025-money-campaign-finance]]

### What money buys: access, not votes

**[[kalla-nd-campaign-contributions-facilitate]]** is the only randomized evidence in the theme on a money mechanism. In 2013, 191 House offices of one party were asked for a district meeting. A chief of staff or more senior official attended in 12.5% of the 64 offices told the attendees were "local campaign donors", against 2.4% of the 127 offices told "local constituents" (p = .006). For the Member personally, the figures were 7.8% vs 2.4% (p = .07, not significant at .05). Lower-ranked staff showed no reliable difference. Three limits apply: the treatment was a *label*, not money; the outcome is access, not influence; and only 9 offices cosponsored the bill during the study, so the cosponsorship comparison carries no information.

**[[ansolabehere-2002-little-money-politics]]** supplies the votes side. It surveys nearly 40 roll-call studies; in three of four, PAC effects are null or wrongly signed. In its own House panel (1978-1994, N = 3,400), about $60,000 of corporate PAC money (about one SD) moves a 0-100 Chamber score by at most 2 points, while a party switch moves it by more than 30. District fixed effects nearly remove the money effect, member fixed effects remove it entirely, and IV reverses the corporate sign.

**How the two fit together:** they agree on a two-part picture. Money opens doors at the senior-staff tier, and it moves recorded votes little or not at all. Bonica 2013's finding that the investor motive is competitive only for corporate and trade PACs is consistent with this, though Bonica measures motive, not access. Bonica 2025 cites Kalla and Broockman for its own link from donating to access.

**Bearing:** this is the strand behind flip_model.py's "money buys access/effort > votes" comment. It supports the access half and the MAXSHIFT cut (40 to 18). It does not calibrate the 0.35/0.65 money/peer split.

Paul's contact-identification record gives a consistent operational result. Money channels (Mentat workbooks, FEC org-PACs, LD-203 gifts) did not help identify which member an LDA activity contacted. Donations stay co-occurring evidence and never take causal verbs (H3).

Works: [[kalla-nd-campaign-contributions-facilitate]], [[ansolabehere-2002-little-money-politics]], [[bonica-2013-mapping-ideological-marketplace]], [[bonica-2025-money-campaign-finance]]

### Money outside the candidate committee: dark money, super PACs, executives

These works describe money that per-member PAC models never see. The evidence is weak throughout.

**[[irvin-2023-dark-investigation-dark]]** builds a 0-9 opacity index from IRS Form 990 fields for 54 501(c)(4) economic-policy groups (2016 filings). The index runs from 0.8 to 8.3, and its case studies show near-shell vehicles. One example: One Nation spent $63.3M in 2016 and granted $21.7M to the Senate Leadership Fund. But in a logistic regression with controls, the total index does *not* predict the OpenSecrets dark-money label for those 54 groups.

**[[chohan-2025-crypto-oligarchy-cryptocurrency]]** is a narrative essay on Fairshake: $203M raised for the 2024 cycle (press-sourced) and 253 candidates supported (self-reported, with no win rate and no counterfactual). It calls the influence "decisive" without estimating anything.

**[[anon-2025-cross-industry-political]]** is an unsigned internal memo. It reports per-industry dollar averages with no cycle years, source or N, plus case stories. It may share data lineage with Mentat's own cross-industry template, so it may not be independent.

**[[bouton-2022-small-campaign-donors]]** adds the conduit channel. An earmarked gift above the $200 aggregate appears twice, once in the conduit report and once in the recipient committee report.

**Agreement:** money moves across channels, through super PACs, 501(c)(4) grants, trade associations and executives' personal giving. Counting one channel undercounts effort.

**Disagreement:** the definition of "dark money". Irvin means nonprofit non-disclosure; the memo uses the term for *disclosed* Super PAC donations. Neither the essay nor the memo offers a comparison group or a test, so no outcome claim in this strand can be used as evidence.

Works: [[irvin-2023-dark-investigation-dark]], [[chohan-2025-crypto-oligarchy-cryptocurrency]], [[anon-2025-cross-industry-political]], [[bouton-2022-small-campaign-donors]]

### Partisan cues: out-party backlash

Two independent designs agree on the sign of the out-party cue.

**[[bakker-nd-understanding-partisan-receptivity]]** ran survey experiments on US partisans, 2016-2018, on issues that were not yet partisan. An out-party cue lowered support by 0.06 (Study 1, N = 747) and 0.07 (Study 2) on a 0-1 scale, a significant repulsion. The in-party cue moved support by about +0.03 and was not significant. The bounded-rationality account, in which low-resource citizens rely on cues, gets no support. The expressive-utility account gets only partial support: two of six three-way terms are significant, and the MTurk replication (Study 4) shows nothing.

**[[merkley-nd-party-cues-news]]** reaches the same direction with different tools.
- In the time series, Democratic cue share in NYT and WaPo climate coverage Granger-precedes aggregate climate skepticism (chi2 6.96, p = 0.01). The reverse direction does not hold, and Republican cues have no aggregate effect. Granger precedence is not causation.
- In a 2019 MTurk experiment (N = 2,970), a Democratic cue moves strong Republicans +0.06 toward skepticism on a 0-1 scale, and a consensus cue produces no movement.

**Agreement:** an out-party cue is not a weaker version of an in-party cue. It pushes the other way, and in-party cues are weak.

**Limits:** both studies use the mass public and one-shot vignettes or news exposure, not legislators pressing legislators. They check the direction of sign only; they cannot set constants.

This strand also bears on Paul's IM-tiers addendum. A signed, possibly repulsive cue is a further reason to doubt that the cross-member problem is monotone. That doubt fits his hypothesis that the problem is non-submodular (complex contagion) but does not test it.

Works: [[bakker-nd-understanding-partisan-receptivity]], [[merkley-nd-party-cues-news]]

### Voter persuasion and targeting

The works agree that persuasion returns are real but depend heavily on context, and that pooled averages hide offsetting effects.

**[[broockman-2026-should-moving-middle]]** ran a pre-registered conjoint with 6,000 verified 2024 voters. Pooled across issues, moving to the "elite middle" gains Democrats +1.1 pp and Republicans +1.8 pp. That pooled figure must be quoted with its strata:
- Where the party's position is unpopular, Republicans gain up to +3.2 pp.
- Where a majority already backs the party's position, the effect is null or negative (Republicans -2.4 pp).
- Voters who hold the party's own position punish the move (Republican-position holders -3.6 pp).

The authors read this as voters who vote on issues along several dimensions at once, so gains and losses cancel in the average.

**[[tappin-2023-quantifying-potential-persuasive]]** (Lucid, Feb 2022, 5,284 adults analyzed) finds the microtargeting advantage is context-dependent. In Study 1, pooled across two modules, microtargeting moved support 5.96 pp against 3.48 for the single best message, but almost all of that came from one module (Citizenship Act 5.17 vs 2.41; the UBI module was not significant). Targeting on one covariate did as well as targeting on many. In Study 2, across 36 issues, issue-level targeting did not beat one best ad (12.30 vs 15.42, P = 0.11).

**[[kalla-2023-which-narrative-strategies]]** shifts attention from *whom* to *what is said*. In field RCTs, a perspective-getting-only door script took 7.3 minutes and matched the 17.1-minute full script. The 95% CI on the difference ran from -0.05 to 0.09 SD (Study 2, about 2,050 per arm). Outcomes are attitudes, not votes or turnout.

**[[madsen-2017-strategic-advantages-micro]]** is a single-figure agent-based simulation with no replications or sensitivity analysis. At equal reach, targeting wins; a random campaign with about 2x the reach cancels the advantage.

**Disagreements:**
- Madsen predicts, without testing, that targeting gains rise with more voter data. Tappin's finding that one covariate is enough runs against that prediction.
- Broockman's large issue-level heterogeneity sits uneasily with Tappin's null for issue targeting. Broockman's heterogeneity runs across *voter positions*; Tappin's ML targeting ran across *covariate profiles*, which may not capture those positions.

All bearings from [[kalla-2023-which-narrative-strategies]] and [[madsen-2017-strategic-advantages-micro]] are UNVERIFIED (DE-062).

Works: [[broockman-2026-should-moving-middle]], [[tappin-2023-quantifying-potential-persuasive]], [[kalla-2023-which-narrative-strategies]], [[madsen-2017-strategic-advantages-micro]]

### Measuring electoral context: competitiveness, maps, expected control, platforms

These are data and design sources, not hypothesis tests.

**[[report-2024-2024-house-glance]]** (Cook, 12 Sep 2024; 435 seats, with rating tallies summing to 436, so approximate) shows that PVI and race rating come apart:
- FL-27 is EVEN and rated Solid R, while CO-08 and NE-02 are EVEN and rated Toss Up.
- The 25 Toss Ups span R+8 to D+5.

The rating carries incumbent and candidate factors that PVI leaves out.

**[[mccartan-2022-simulated-redistricting-plans]]** gives 5,000 simulated 2020-cycle plans per state. Any single PVI point is therefore one draw from a map-dependent distribution. The plans do not cover the 2025-26 redraws.

**[[snowberg-2006-partisan-impacts-economy]]** supplies the key method lesson. In 2004, regressing asset prices on win probabilities over the campaign period gave equity coefficients about ten times the election-night estimate and flipped the oil sign. The cause is reverse causation (the economy moves the odds) plus common shocks. Only discrete, exogenous probability jumps identify the effect. This applies to Paul's projected-control variables (Shift, Tossup, RShare), which his sector-growth design builds on prediction markets.

**[[porter-2025-campaignview-campaign-platforms]]** releases hand-coded House primary platforms for 2018-2022: 5,228 of 6,016 eligible candidates, with relabel agreement of kappa 0.84 on 8,584 points. Missingness is not random: incumbents and better-funded candidates are less often missing.

**Agreement:** a single static competitiveness number (PVI) is a coarse and map-dependent stand-in. Ex-ante expected control has to be identified from discrete shocks, not from a running correlation. None of the four bears on whether dollars buy more in competitive seats.

Works: [[report-2024-2024-house-glance]], [[mccartan-2022-simulated-redistricting-plans]], [[snowberg-2006-partisan-impacts-economy]], [[porter-2025-campaignview-campaign-platforms]]

## Agreements and debates

**1. Does campaign money buy anything?**
- [[ansolabehere-2002-little-money-politics]]: PAC money has little or no effect on votes once member or district fixed effects are added (House panel, 1978-1994, N = 3,400).
- [[kalla-nd-campaign-contributions-facilitate]]: a donor label buys senior-staff access (191 House offices, one party, 2013).
- [[chohan-2025-crypto-oligarchy-cryptocurrency]]: claims "decisive" electoral influence for Fairshake, with no estimate.
- [[anon-2025-cross-industry-political]]: claims industries shift giving onto executives and respond to executive-branch scrutiny, with no test.

The first two are reconcilable because they measure different outcomes: access versus recorded votes. The last two are assertions, not counter-evidence.

**2. Do donors steer money toward competitive seats?**
- [[ansolabehere-2002-little-money-politics]]: closeness raises total House race spending.
- [[bouton-2022-small-campaign-donors]]: small donors do not concentrate on close seats (+0.11 pp, n.s.), and large donors do so only weakly (+0.18 pp). Both figures cover Democratic congressional candidates, 2012-2020, only.
- [[bonica-2013-mapping-ideological-marketplace]]: for individual donors (5,000 donors, 2004-2012), the partisan-electoral motive loses to ideology.

**Flag:** the vault's related-link from [[report-2024-2024-house-glance]] says Bonica "finds that donors direct funds to competitive races". That misstates Bonica's own model comparison, so the link should be corrected. None of the three works tests the reciprocity premise behind the competitiveness multiplier, which remains NOT validated (D-104..D-108).

**3. Is an out-party cue a weak push or a backlash?**
[[bakker-nd-understanding-partisan-receptivity]] and [[merkley-nd-party-cues-news]] agree it is a backlash among mass partisans. The engine encodes the opposite sign (cpf_pressure out-party x0.5, positive). No work in the theme measures legislators, so the transfer is open, and recalibration is Paul's decision.

**4. How much does targeting add?**
- [[madsen-2017-strategic-advantages-micro]] (simulation only): targeting wins at equal reach, and gains should grow with more data.
- [[tappin-2023-quantifying-potential-persuasive]]: one covariate is enough, and issue-level targeting does not beat one best ad.
- [[broockman-2026-should-moving-middle]]: issue-by-voter heterogeneity is large but cancels in the pooled average.

Tappin is the empirical check on Madsen's untested prediction and runs against it.

**5. What counts as "dark money"?**
For [[irvin-2023-dark-investigation-dark]] it means nonprofit non-disclosure. For [[anon-2025-cross-industry-political]] it means disclosed Super PAC donations. The two must not be pooled.

**6. Pooled numbers over known fault lines.**
Several headline figures in the theme are pooled over a split the authors themselves show matters:
- Bonica's r = 0.92 is UNSTRATIFIED; within party it is 0.56-0.72.
- Broockman's +1.1/+1.8 pp must carry its strata.
- Tappin's pooled 5.96 pp is driven by one of two modules.

Any use of these figures in the systems must carry the strata or an UNSTRATIFIED label.

**Rejected SNA proposals:** no work in this theme argues for dropping regular cosponsorship edges or for per-edge volume normalization. Per-edge normalization appears only in Paul's contact-identification kill-ledger, where it stays settled.

## Methods

**Randomized field experiments**
- Works: [[kalla-nd-campaign-contributions-facilitate]] and [[kalla-2023-which-narrative-strategies]].
- Strength: the strongest internal validity in the theme.
- Weaknesses:
  - Small or narrow samples: 191 offices of one party, a single 2013 issue.
  - Treatments are labels or scripts, not money or votes.
  - Outcomes such as access rank and explicit attitudes are proxies.
  - Low base rates make some contrasts uninformative (only 9 cosponsorships).
  - Effects are intent-to-treat among people who came to the door.

**Survey and conjoint experiments**
- Works: [[bakker-nd-understanding-partisan-receptivity]], [[broockman-2026-should-moving-middle]], [[tappin-2023-quantifying-potential-persuasive]], and the experimental arm of [[merkley-nd-party-cues-news]].
- This is the dominant method in the persuasion and cue strands.
- Weaknesses:
  - Forced exposure, which Tappin says probably overstates field returns.
  - Online panels (Lucid, MTurk, Verasight), where replications can fail: Bakker's Study 4 shows nothing.
  - Hypothetical candidates.
  - Respondents are always the mass public, never legislators.

**Observational panels with fixed effects**
- Works: [[ansolabehere-2002-little-money-politics]], [[bonica-2025-money-campaign-finance]], [[bouton-2022-small-campaign-donors]].
- This is the dominant method in the money strands.
- Weaknesses:
  - Confounding, and two-way-FE linearity assumptions.
  - Results move under specification: the money effect vanishes with member FE and reverses sign under IV.
  - Coverage artifacts: WinRed exists only from 2019, and conduit coverage was 76.4% of contributions over 2006-2020.

**Measurement and scaling**
- Works: [[bonica-2013-mapping-ideological-marketplace]] (correspondence analysis on contributor-by-recipient matrices) and [[irvin-2023-dark-investigation-dark]] (an equal-weight 0-9 index).
- Weaknesses:
  - Bonica's validity is mostly party separation (r = 0.92 pooled, 0.56-0.72 within party).
  - Irvin's index fails its own external validation.

**Time-series precedence**
- Work: [[merkley-nd-party-cues-news]] (VAR and Granger tests).
- Weakness: it shows order, not causation, and N = 55 quarters.

**Event-window identification**
- Work: [[snowberg-2006-partisan-impacts-economy]], using election-night probability shocks.
- Strength: the best observational design in the theme.
- It is also the design that exposes how far campaign-period correlations are inflated.

**Data descriptors**
- Works: [[porter-2025-campaignview-campaign-platforms]], [[mccartan-2022-simulated-redistricting-plans]], [[report-2024-2024-house-glance]].
- Weaknesses:
  - Coverage windows are narrow: House only and 2018-2022 for Porter; 2020-cycle maps only for McCartan; one dated snapshot for Cook.
  - Missingness is non-random (Porter).

**Agent-based simulation**
- Work: [[madsen-2017-strategic-advantages-micro]].
- Weakness: one figure, with no intervals and no sensitivity analysis. Its outputs are simulations, not estimates.

**Essay and memo**
- Works: [[chohan-2025-crypto-oligarchy-cryptocurrency]], [[anon-2025-cross-industry-political]].
- Weakness: no comparison group, no test, press-sourced figures. They are usable only as descriptive context.

**Overall:** causal designs exist only for access and for mass-public attitudes. No work gives causal evidence that money moves legislator votes or positions, or that competitive seats change the return per dollar.

## How the works relate

- [[bonica-2025-money-campaign-finance]] — same-data — [[bonica-2013-mapping-ideological-marketplace]]: Both rest on DIME contribution records; the 2025 paper uses DIME v4.0 built on the 2013 scaling and linkage work.
- [[bouton-2022-small-campaign-donors]] — same-data — [[bonica-2013-mapping-ideological-marketplace]]: Bouton et al. use Bonica's CFscores for candidate ideology terciles (cited as the 2014 AJPS version).
- [[bouton-2022-small-campaign-donors]] — builds-on — [[ansolabehere-2002-little-money-politics]]: Extends the consumer-donor account to sub-$200 donors made visible through ActBlue and WinRed earmarks.
- [[bouton-2022-small-campaign-donors]] — qualifies — [[ansolabehere-2002-little-money-politics]]: Ansolabehere finds closeness raises House race spending; Bouton finds small donors do not concentrate on close seats (+0.11 pp n.s., Dem congressional 2012-2020). Different estimands, partial tension.
- [[bonica-2013-mapping-ideological-marketplace]] — agrees — [[ansolabehere-2002-little-money-politics]]: Ideological model beats investor model for individual donors, consistent with giving as participation rather than policy purchase.
- [[ansolabehere-2002-little-money-politics]] — complements — [[kalla-nd-campaign-contributions-facilitate]]: Null or small effects of PAC money on votes vs randomized evidence that a donor label buys senior-staff access; together they split access from votes.
- [[bonica-2025-money-campaign-finance]] — cites — [[kalla-nd-campaign-contributions-facilitate]]: Bonica and Grumbach cite the field experiment for their link from donating to access.
- [[bouton-2022-small-campaign-donors]] — cites — [[kalla-nd-campaign-contributions-facilitate]]: Bouton et al. cite Kalla and Broockman (2016) in their introduction on elite responsiveness.
- [[bonica-2013-mapping-ideological-marketplace]] — complements — [[kalla-nd-campaign-contributions-facilitate]]: Bonica's investor model tests access-type motives without measuring access; Kalla measures access directly.
- [[bonica-2025-money-campaign-finance]] — complements — [[bouton-2022-small-campaign-donors]]: Both document an unrepresentative donor pool (age; race, gender, home value) and the same WinRed-from-2019 coverage hazard for pre-2020 Republican estimates.
- [[bakker-nd-understanding-partisan-receptivity]] — agrees — [[merkley-nd-party-cues-news]]: Independent designs (survey experiments; news time series plus MTurk experiment) both find out-party cues repel and in-party cues are weak.
- [[kalla-2023-which-narrative-strategies]] — same-authors — [[broockman-2026-should-moving-middle]]: Kalla and Broockman; one studies what is said at the door, the other when candidate moderation pays.
- [[kalla-2023-which-narrative-strategies]] — same-authors — [[kalla-nd-campaign-contributions-facilitate]]: Same authors, distinct studies; keeping them apart prevents misattributing persuasion evidence to the flip model's money-buys-access citation.
- [[kalla-2023-which-narrative-strategies]] — complements — [[tappin-2023-quantifying-potential-persuasive]]: Message content (perspective-getting) vs whom to target; together they separate what-to-say from who-to-target.
- [[broockman-2026-should-moving-middle]] — complements — [[tappin-2023-quantifying-potential-persuasive]]: Both find context-dependent persuasion; Broockman's issue-by-voter heterogeneity sits in tension with Tappin's null for issue-level targeting.
- [[tappin-2023-quantifying-potential-persuasive]] — contradicts — [[madsen-2017-strategic-advantages-micro]]: Madsen predicts (untested) that targeting gains grow with more voter data; Tappin finds one covariate captures the gain and multi-issue targeting does not beat one best ad.
- [[broockman-2026-should-moving-middle]] — qualifies — [[bonica-2013-mapping-ideological-marketplace]]: Multidimensional, cross-pressured voter preferences caution against reading a single-axis CFscore as a position on any particular issue.
- [[porter-2025-campaignview-campaign-platforms]] — complements — [[bonica-2013-mapping-ideological-marketplace]]: Candidate positions from platform text vs from donor-based ideal points; not cited by Porter.
- [[porter-2025-campaignview-campaign-platforms]] — complements — [[broockman-2026-should-moving-middle]]: CampaignView supplies real candidate-level primary positions of the kind Broockman's conjoint varies hypothetically.
- [[irvin-2023-dark-investigation-dark]] — contradicts — [[anon-2025-cross-industry-political]]: Incompatible definitions of dark money: nonprofit non-disclosure (Irvin) vs disclosed Super PAC donations (memo).
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — [[irvin-2023-dark-investigation-dark]]: Both describe electoral money outside candidate committees; Irvin gives an organization-level opacity measure Chohan lacks.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — [[anon-2025-cross-industry-political]]: Both describe industry money flowing through Super PACs and channels per-member PAC models do not see; both are descriptive with no test.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — [[kalla-nd-campaign-contributions-facilitate]]: Kalla's randomized access result is adjacent to Chohan's unestimated influence claim; the vault downgraded this from contradicts.
- [[report-2024-2024-house-glance]] — misattributed-link — [[bonica-2013-mapping-ideological-marketplace]]: The vault link says Bonica finds donors direct funds to competitive races; Bonica's own comparison has the partisan-electoral model losing to ideology for individual donors. Correct the link.
- [[mccartan-2022-simulated-redistricting-plans]] — complements — [[report-2024-2024-house-glance]]: A single PVI point is one draw from a map-dependent distribution; the ensemble supplies the map baseline Cook's table cannot.
- [[snowberg-2006-partisan-impacts-economy]] — complements — [[report-2024-2024-house-glance]]: Both feed Paul's sector-growth design's ex-ante electoral-control measures (prediction-market probability; Cook ratings for toughness_general).
- [[snowberg-2006-partisan-impacts-economy]] — co-cited-in-design — [[ansolabehere-2002-little-money-politics]]: Both anchor Paul's sector-growth study: Ansolabehere for the consumption account and income scaling, Snowberg for identifying projected-control effects.

## What it means for the systems

### Melange influence engine ([[systems/melange-influence-engine]])

**Flip model (flip_model.py:21).** The code cites Kalla and Broockman for "money buys access/effort > votes" and labels the 0.35/0.65 money/peer split CALIBRATED.
- [[kalla-nd-campaign-contributions-facilitate]] supports the access half only.
- [[ansolabehere-2002-little-money-politics]] supports weighting money below peers and the MAXSHIFT cut from 40 to 18 on votes.
- No work calibrates the ratio itself, so the CALIBRATED label overstates the evidence.
- [[bonica-2013-mapping-ideological-marketplace]] shows money and stance are entangled through donor ideology. Any money-moves-members test therefore needs an ideology control. This supports the stance-money gate ([[systems/melange-stance]]).
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] and [[anon-2025-cross-industry-political]] show industry money the flip model's PAC-receipts term cannot see: super PACs, trade associations and executives.

**Competitiveness multiplier c (COMPET_MULT, D-104).** Nothing in this theme validates reciprocity, the premise that dollars buy more in competitive seats. It stays NOT validated (D-104..D-108).
- [[report-2024-2024-house-glance]] shows that PVI-only c treats a safe EVEN-PVI incumbent (FL-27, Solid R) like a true Toss Up. With the dial on, c also neutralizes the receptivity layer's candidate-specific competitiveness term.
- [[mccartan-2022-simulated-redistricting-plans]] adds map dependence (UNVERIFIED lead).
- The donor works show closeness affects money *supply*, not money *returns*.

**cpf_pressure, LT-priming and salience shock.**
- [[bakker-nd-understanding-partisan-receptivity]] and [[merkley-nd-party-cues-news]] contradict the positive x0.5 out-party pull in sign: among the mass public, out-party cues repel.
- The LT-priming clamp at 0 is closer to the evidence but still never goes negative.
- The salience shock's Retreat regime fits the evidence. Its Rally +0.5 is untested (Merkley's consensus null is not a rally test).
- All of this is mass-public evidence. The transfer to legislators is unverified, and any recalibration is Paul's decision.

**Electoral plays.** [[porter-2025-campaignview-campaign-platforms]] could replace the same-party-average proxy for primary vulnerability with real challenger platforms, for the House only and 2018-2022 only. [[chohan-2025-crypto-oligarchy-cryptocurrency]] shows that primary-contest plays happen in practice.

**Persuadability table.** [[broockman-2026-should-moving-middle]] warns that an average across issues hides offsetting issue-level responses.

### Mentat ([[systems/mentat-fec-attribution]], [[systems/mentat-giving-forecast]], [[systems/mentat-sector-growth-study]])

**Identity ladder.**
- [[bonica-2013-mapping-ideological-marketplace]] gives an error profile: bad merges joined family members at the same employer.
- [[bouton-2022-small-campaign-donors]] always requires a street address and rejects employer and occupation as grouping keys.
- Mentat auto-accepts on exact name plus ZIP. This is flagged for Paul, not recommended as a change.

**Dedup and conduits.** Bouton pairs and drops 87% of earmark duplicates. Mentat counts both legs under ruling R8. This is flagged only: money collection is frozen without per-change approval. Conduit coverage changes over time (WinRed only from 2019), which puts a visibility artifact into cross-cycle partisan comparisons.

**Transfer graph.** Under [[irvin-2023-dark-investigation-dark]], a 501(c)(4) grant to a Super PAC enters as a receipt with no upstream path. The 501c exclusion (2026-07-18 ruling) removes a mix of policy shops and near-shells that the index cannot reliably separate.

**Sector-growth study** ([[systems/mentat-sector-growth-study]]).
- [[ansolabehere-2002-little-money-politics]]: the consumption account and income elasticity fit the null for industry growth.
- [[snowberg-2006-partisan-impacts-economy]]: SEATS-W (-3.47 pp per expected seat, 2022 House, one election, EXPLORATORY) is a correlation, not a donor response. Lagging the forecast fixes ordering but not common shocks. Event windows around discrete forecast jumps would be sharper.
- [[report-2024-2024-house-glance]] is one dated input to toughness_general.

**Giving forecast** ([[systems/mentat-giving-forecast]]). [[chohan-2025-crypto-oligarchy-cryptocurrency]] documents the 2024 series break, which supports declining a crypto forecast for now.

### Terrier

**Untapped potential and candidate support.**
- The s = 0.5 persuasion peak is a one-dimensional reading. [[broockman-2026-should-moving-middle]] suggests a mixed partisan score marks a cross-pressured voter.
- [[tappin-2023-quantifying-potential-persuasive]] says message choice matters and coarse partisan targeting captures the gain. That is a cheap extension, but only through Terrier's own pretest.
- [[kalla-2023-which-narrative-strategies]] and [[madsen-2017-strategic-advantages-micro]] point to script content and messenger credibility, which are absent terms. Both are UNVERIFIED leads.

**Sphere and Participation standings.** [[bonica-2025-money-campaign-finance]] shows giving and turnout are steeply age-graded (2020-cycle median contribution dollar from a 66-year-old). Terrier stratifies by registration but not by age, so its top deciles will lean older.

### Wording guards for any surface

- Donations are co-occurring evidence with no causal verbs (Paul's H3).
- Access is not influence.
- LDA stays an identity source, not an edge or weight source (D-021). The Kalla access outcome gives no license to add LDA-derived edges ([[systems/melange-lobbying]], [[systems/melange-legislative-data]]).
- Every quoted figure keeps its window and denominator.

## Gaps

**Missing annotations**
- Two of Paul's source files are bare citation lists with no annotation prose and no _annots sidecar: 1091449c88da ("Campaign Politics - Annotated Bibliography", 10 citations) and ed1fc0f63ea9 (10 citations).
- Their topics are only partly represented as works here: digital campaigning, campaign donations and lobbying, negative campaigning, campaign sentiment analysis, big data in campaigns, and message complexity and political knowledge. [[bonica-2025-money-campaign-finance]] is the clear overlap.
- The fix is to re-export from the source docx in the "Annotated Bibliographies & Literature Synthesis" zip, or supply the annotated copy, then rerun extraction.

**Evidence gaps**
- No legislator-level cue or pressure experiment in this theme. All cue and persuasion evidence is mass-public, so cpf_pressure, LT-priming and the persuadability constants have no direct source. Box-Steffensmeier et al. (Dear Colleague cue-taking) is referenced in vault links but is not a work in this theme.
- No causal evidence that money moves legislator votes or positions after 2002. The roll-call literature in [[ansolabehere-2002-little-money-politics]] ends in the 1990s.
- Nothing tests reciprocity: whether competitive seats change the return per dollar. This is the competitiveness multiplier's premise, and it stays NOT validated.
- No credit-union or financial-sector-specific campaign evidence. [[anon-2025-cross-industry-political]] is an unverified memo with no windows or denominators.
- No causal estimate of super-PAC or 501(c)(4) spending effects. [[chohan-2025-crypto-oligarchy-cryptocurrency]] and [[irvin-2023-dark-investigation-dark]] are descriptive. Blackwell (post-Citizens United ad effects) appears in vault links but not in this theme.
- No study of the 2025-26 mid-decade redraws. [[mccartan-2022-simulated-redistricting-plans]] covers 2020-cycle maps only.

**Works Paul's sector-growth design relies on that are not anchored here**
- Cox and Magar 1999 (value of majority status; the ~$36,000 per member figure is still to verify).
- Kerr, Lincoln and Mishra 2014 (lobbying persistence).
- de Figueiredo and Richter.
- Grier, Munger and Roberts. This is the only anchor offered for the sign of the performance-spending relation, Paul's open question Q10.

**Replication and robustness holes**
- [[kalla-nd-campaign-contributions-facilitate]]: one party, one issue, 191 offices, 2013. No replication in the theme.
- [[bakker-nd-understanding-partisan-receptivity]]: the expressive-utility pattern fails in the MTurk replication.
- [[madsen-2017-strategic-advantages-micro]]: no sensitivity analysis.
- [[tappin-2023-quantifying-potential-persuasive]]: forced exposure, no candidate races.

**Open from Paul's notes**
- Whether congressional coalition formation is a complex contagion (IM-tiers addendum). Nothing here tests it. The signed cue evidence only makes monotone-spread assumptions less safe.
- The contact-identification lane's responsiveness spin-off (the 4.1x push-to-join coupling) has no counterpart literature in this theme.

**Vault hygiene**
- The [[report-2024-2024-house-glance]] related-link misstates [[bonica-2013-mapping-ideological-marketplace]] on competitive-race giving.
- The [[kalla-2023-which-narrative-strategies]] slug year (2020) disagrees with the AJPS 2023 publication.
- The Cook table's rating tallies sum to 436 against 435 seats, and there is a broken #REF! cell at MA-01.

## All works

- [[report-2024-2024-house-glance]] (2024) — 2024 House At-A-Glance
- [[broockman-2026-should-moving-middle]] (2026) — Should Moving to the Middle Win Candidates Votes? It Depends Where Voters Are
- [[kalla-2023-which-narrative-strategies]] (2023) — Which Narrative Strategies Durably Reduce Prejudice? Evidence from Field and Survey Experiments Supporting the Efficacy of Perspective-Getting
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
- [[bouton-2022-small-campaign-donors]] (2022) — Small Campaign Donors
- [[ansolabehere-2002-little-money-politics]] (2002) — Why Is There So Little Money in U.S. Politics?
- [[snowberg-2006-partisan-impacts-economy]] (2006) — Partisan Impacts on the Economy: Evidence from Prediction Markets and Close Elections
