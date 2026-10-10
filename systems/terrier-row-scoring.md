---
type: system
name: Terrier (CU Geography) person and area scoring
slug: terrier-row-scoring
components: [terrier-row-scoring.cua-index, terrier-row-scoring.participation-index, terrier-row-scoring.sphere-of-influence, terrier-row-scoring.partisan-lean, terrier-row-scoring.overall-standing, terrier-row-scoring.uncertainty-draws, terrier-row-scoring.transforms-and-rules, terrier-row-scoring.area-eb-aggregation]
brain_notes:
  - C:/Users/pgoff/Documents/brain/terrier/terrier.md
  - C:/Users/pgoff/Documents/brain/terrier/2026-10-06-terrier-rulings.md
  - C:/Users/pgoff/Documents/brain/topics/2026-09-feature-cu-geography-luna-and-row-framework.md
  - C:/Users/pgoff/Documents/brain/projects/cu-geography.md
paper_count: 7
tags: [system, type/system-hub]
---

# Terrier (CU Geography) person and area scoring

The row framework that scores every voter-file person (Colorado, OR-05, CT-01). Covers the CUA index, Participation Index, Sphere of Influence, partisan lean, the district-specific overall standing, expected-value cells with uncertainty draws, categorical transforms and rules, empirical-Bayes area aggregation and descriptive area measures.

Brain notes: [terrier/terrier](obsidian://open?vault=brain&file=terrier%2Fterrier); [terrier/2026-10-06-terrier-rulings](obsidian://open?vault=brain&file=terrier%2F2026-10-06-terrier-rulings); [topics/2026-09-feature-cu-geography-luna-and-row-framework](obsidian://open?vault=brain&file=topics%2F2026-09-feature-cu-geography-luna-and-row-framework); [projects/cu-geography](obsidian://open?vault=brain&file=projects%2Fcu-geography)

### Terrier person and area scoring: evidence assessment

**What the vault holds.** There are 9 evidence rows across 7 papers, and they bear on 6 of the 8 components. By status: 7 VERIFIED-SOFTENED, 1 UNVERIFIED, 1 VERIFIED. The UNVERIFIED row (Holyoke 2015 on Sphere of Influence) is a lead, not a finding. Strength is 5 weak, 4 moderate. **No paper in the vault validates any Terrier score against an outcome.** None tests whether CUA standing predicts credit-union membership, whether Participation standing predicts validated turnout, or whether Sphere or overall standing predicts contact with a member of Congress. Every question this system is meant to answer still has to be answered from Terrier's own data.

**Where evidence exists:**
- **Method level.** DeLuca et al. 2022 supports summing probabilities over taking the argmax (modal assignment). This backs the expected-value design (PR2) and the area aggregation. Harrison 2010 confirms the mechanics of inverse-CDF sampling for the draws.
- **Confounds and qualifications on Sphere of Influence.**
  - Bonica & Grumbach 2025 show that political giving and turnout rise steeply with age. Their data: 2020 cycle, DIME itemized contributions linked to the L2 national file.
  - Kalla & Broockman and Furnas et al. 2019 disagree about whether donor status buys access. Neither study covers individual constituents acting alone.
- **Partisan lean.** Broockman 2026 qualifies the one-axis assumption.

**Two components have no evidence at all:** Participation Index and the ridit transforms and rule cells. Several components are gaps on their central empirical question.

**Things Paul should see first:**
1. **Internal contradiction on partisan lean.** The component and the evidence row both say lean is "never weighted". But Paul's O-11 answer (2026-10-06) keeps "numeric partisan and support layers" in the overall score, and candidate districts give "candidate support" 25%. Whether lean feeds that support layer has to be settled from the code.
2. **The calibration bottleneck.** CUA, the uncertainty draws and the EB area shares all assume the imputation class probabilities are calibrated. No expected calibration error (ECE) has been measured. The brain records that the Luna holdout has been used twice and that no untouched labelled data remains, so an honest ECE needs new labels.
3. **In-house diagnostics matter more than the literature here.** From the P0 run on the Colorado CD-05 file (491,308 rows, 2026-10-01):
   - Under 45/45/10, the Sphere top decile is 98.9% volunteers.
   - CUA has rho 0.698 with consumer-purchase (CPI) volume.
   - The recession item's item-rest correlation is -0.35.
   - The overall top decile is 42.1% registered R, against 27.4% R in the whole file.

**How to read the scores.** All Terrier standings, Sphere of Influence included, are heuristic model outputs. They are not probabilities of contact, access or influence.

## CUA (Credit Union Alignment) index

**What it computes.** Expected weighted exposure over 16 fields in 4 themes (credit .40; digital, auto/insurance, housing .20 each); midrank percentile standing.

**Method.** Expected values over class probabilities (PR2), never argmax. Grade D if measured share <= 1/2.

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/rowfw/engine.py:49-593`; `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/framework_config.json`

**Assumptions.**
- Vendor consumer fields measure exposure to ACU issues
- Council weights are unratified

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[deluca-2022-validating-applicability-bayesian]] | 2022 | supports | Summed BISG posteriors (PSM) give lower absolute error than modal assignment (PM) in nearly all districts. In NC, PSM Black-share error never exceeds about 5 pp; in GA it peaks about 5 pp. PSM White-share error is near 0 in NC and reaches about 10 pp in GA. | The paper validates area-level aggregates of voter-level race posteriors for Black vs White in two segregated states. CUA is a person-level expected value for vendor consumer fields. Support for the person-level index is indirect and holds only where the class probabilities are roughly calibrated; calibration is the open ECE question. | weak | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: weak and indirect. One VERIFIED-SOFTENED row (DeLuca et al. 2022, weak).**

**What the paper shows.** Summed BISG race posteriors (PSM) give lower district-level error than modal assignment in nearly all districts. In North Carolina the error on Black share stays within about 5 pp. In Georgia, the error on White share reaches about 10 pp.

**What it supports here.** The design choice to use expected values over class probabilities rather than the argmax (PR2, engine.py 264-270).

**Why it transfers only weakly.** The paper validates *area aggregates* of *race* posteriors in two segregated Southern states. CUA is a *person-level* expected value over *vendor consumer fields*. The support holds only to the extent that the class probabilities are calibrated, and nobody has measured that.

**What the evidence does not reach.** The construct claim, that vendor credit, digital, auto/insurance and housing fields measure exposure to the issues on America's Credit Unions' agenda. No paper in the vault addresses it.

**In-house warnings (Colorado CD-05, P0, 2026-10-01, 491,308 rows).**
- CUA correlates rho 0.698 with consumer-purchase (CPI) volume. CUA may be tracking how much consumer data a person has, not alignment.
- The recession-sensitivity item's item-rest correlation is -0.35. It runs against its own theme, yet it was kept by ruling ("recession +", 2026-10-01).

**Status of the weights.** The theme weights (credit .40; other themes .20 each; within credit: cc .16, card .16, recession .08) are UNRATIFIED council defaults.

**Contradictions.**
- The recession item is kept with a positive sign by ruling (2026-10-01), but its item-rest correlation is -0.35 (Colorado P0). Either the sign or the theme assignment is at odds with the data.
- The engine says "expected values, never argmax", which is the design DeLuca supports. But the walk list's Political outlook column now "collapses neighbouring vendor flags" (2026-10-09). Check that no CUA input passes through a collapsed or modal path.

**Gaps.**
- Criterion validity: no study links CUA standing to actual credit-union membership, CU product use, or response to CU canvassing. To fill it: match a de-identified CU member list, or post-canvass response codes, against CUA deciles in one district, with a pre-registered gate.
- Construct validity of vendor consumer fields: no evidence that vendor-modelled credit, card or housing codes agree with the true values. To fill it: a validation of vendor consumer models against survey or administrative data (a literature search for vendor-file accuracy studies).
- Calibration (ECE) of the imputed class probabilities feeding CUA. To fill it: new labelled rows, since the existing holdout has been used twice.
- Discriminant check against CPI volume. To fill it: partial out data volume and re-rank (an in-house computation).

**Paul's position vs the evidence.**

Paul records no literature for CUA ("No annotated literature"). The only paper evidence supports the expected-value mechanics, not the index's meaning. Nothing in the vault contradicts his design. Equally, nothing supports the weights, which remain unratified council defaults, or the premise that vendor fields measure CU-issue exposure. His "recession +" ruling stands against an in-house item-rest correlation of -0.35.

**Open questions.**
- Does CUA standing predict CU membership or canvass response in any district?
- How much of CUA's rank order is driven by consumer-data volume (rho 0.698 with CPI volume, Colorado P0)?
- Should the recession item stay in the credit theme given its item-rest correlation of -0.35? (The ruling is Paul's.)
- Are the CUA theme weights to be ratified, or kept labelled as council defaults?

**Read first.** [[deluca-2022-validating-applicability-bayesian]]

## Participation Index

**What it computes.** Four themes at 1/4 each over ten sai_* turnout and engagement scores; code 50 on digital reachability is left unscored.

**Method.** Weighted index with midrank standing.

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/rowfw/engine.py:376-383`

**Assumptions.**
- Vendor propensities are valid

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment.**

**Evidence weight: gap on the central question. No evidence row is attached to this component.**

**What touches it indirectly.** Bonica & Grumbach 2025 (VERIFIED-SOFTENED, attached to overall standing) show steep age-grading of turnout: median non-voter 39, median voter 52, median primary voter 59 (2020, L2 national file of about 208M registrants). Participation's Turnout theme will carry this gradient. The paper says nothing about the Early/mail, Activism or Digital themes.

**The cited paper is missing.** Paul's listed citation, Nickerson & Rogers 2014, is marked "listed, missing" and is not in the vault. It is the obvious first read on what vendor turnout and support scores are and how accurate they are.

**In-house calibration evidence.** The strongest evidence available is internal, from the 2026-10-09 calibration ruling. In OR-05, the district mean of t (the mean of five turnout-only sai_* scores divided by 100) was 0.55. The mean of people's own general-election record (share of eligible general elections voted) was 0.84. People with t between 0.2 and 0.4 voted in 74% of the general elections they were eligible for. So vendor turnout scores are badly miscalibrated in *level* against past behaviour. The Participation standing is a midrank, so level errors do not move it.

**What remains untested.** Whether the *rank order* of vendor propensities predicts validated future turnout. The calibration against past record is in-sample and backward-looking.

**Contradictions.**
- The vendor scores read as probabilities (t mean 0.55 in OR-05) disagree sharply with people's own general-election record (0.84). This contradicts the assumption that "vendor propensities are valid" as levels. It does not contradict their use as ranks.

**Gaps.**
- Prospective validity: does Participation standing (or t) rank people by validated turnout in a future election? The council default "pre-register the 3 Nov 2026 turnout check" is the right study. It should be scored once against the voter-file history update after November 2026, by district.
- Nickerson & Rogers 2014 has to be obtained and read before any claim about the accuracy of vendor turnout scores.
- There is no evidence on the Activism and Digital themes, or on whether code 50 (digital reachability, left unscored) is missing at random.
- Age stratification: Participation standing should be reported within age bands to show how much is age.

**Paul's position vs the evidence.**

Paul's position is a listed but missing citation. The literature in the vault neither supports nor contradicts the index. The in-house finding (mean t 0.55 against a record of 0.84 in OR-05) contradicts the vendor scores as probabilities. Paul has already acted on it by approving per-district PAV calibration on 2026-10-09, with ranking unchanged. That is consistent with the evidence. It does not show that the ranking is valid.

**Open questions.**
- Will the 3 Nov 2026 turnout check be pre-registered before the election-history update arrives?
- Is the PAV calibration being applied to anything beyond t, such as the Participation standing itself? (It should not change ranks.)
- How strongly age-graded is Participation standing within OR-05 and CT-01?

**Read first.** [[bonica-2025-money-campaign-finance]]

## Sphere of Influence

**What it computes.** 0.45 giving + 0.45 volunteering + 0.10 household; midrank percentile.

**Method.** Ratified 2026-10-01 (owner 'leaning').

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/framework_config.json (groups.soi)`

**Assumptions.**
- Givers and volunteers engage members of Congress

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bonica-2025-money-campaign-finance]] | 2025 | qualifies | Political contribution dollars to congressional candidates are concentrated among older people. In the 2020 cycle the median dollar comes from a 66-year-old and the mean dollar from a 64-year-old (63.9), and only 9% of dollars come from donors aged 40 or younger. People who have ever given the $2,900 individual limit average 63 years old. Cohorts aged 70-80 donate about 200-300x more per capita than 18-year-olds (Fig. 4, 2020). Denominator: itemized individual contributions to congressional candidates in DIME, linked to L2 (the linked set is 17M donors over 2012-2020, not 2020 alone). | The paper's age gradient is for federal itemized contributions in 2020. Terrier's field is a vendor's modelled flag for high political contribution amount in a district voter file (CO, OR-05, CT-01). The direction should carry over because both index political-donor wealth. The size will differ by vendor model and by district age structure. The paper says nothing about whether donors contact members more. Its access link rests on the cited Kalla and Broockman experiment, which is about announced donor status, not about individual donors' outreach. | moderate | VERIFIED-SOFTENED |
| [[holyoke-2015-learnable-skills-unteachable]] | 2015 | supports | Members of interest groups contact lawmakers more than non-members do (Holyoke 2013, cited). Entrenched interests show an issue's importance by having prominent constituents call first. Members should target their own elected officials, and personal constituent stories persuade most (Fitch 2010, cited). | Only the direction carries over: civically engaged people are likelier to contact their own representative. The claims are cited, not tested, and are about national federal contact. Vendor charitable-giving fields are not the political group membership the paper names. Nothing here supports the 45/45/10 weights. | weak | UNVERIFIED |
| [[furnas-2019-moneyed-interests-information]] | 2019 | qualifies | Relative to a constituent, a requester presented as a campaign donor gets no meeting advantage, staff are less likely to use the donor's information, and all-staff ratings put donors as less representative of the district or state (null among Republican staff). | Terrier's people are constituents. The paper sets 'donor' against 'constituent' as alternative identities, so it cannot say whether a constituent who also gives is received better or worse. At most it suggests that a request framed around giving buys no extra staff access. It gives no reason to change the 45/45/10 split (recorded as Paul ruling (leaning), PR3), and it supports volunteer and constituent standing as the more credible channel. | weak | VERIFIED-SOFTENED |
| [[kalla-nd-campaign-contributions-facilitate]] |  | qualifies | When an organized group tells an office its members are local campaign donors, senior officials attend more often than when described as constituents: three to four times in the abstract; Table 1 ratios about 3.2 for Member, 3.4 for legislative director or above, and 5.2 for chief of staff or above. | The paper bears only on donor status made known to an office through an organized group's request. It says nothing about charitable giving, volunteering, or individuals acting alone, and it covers one liberal organization in 2013. Use it to justify revealing donor status in organized asks, not to reweight the giving term. The config does not define contrib_high, so its mapping to donor status is unverified. | weak | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: four rows, none of which tests the component's premise.** The premise is that givers and volunteers in a district voter file contact their member of Congress more than others do.

**The rows:**
- **Holyoke 2015 (UNVERIFIED, weak, supports).** Interest-group members contact lawmakers more, and prominent constituents "call first". This is a *lead*: the claims are cited, not tested, and concern political group membership, not vendor charitable-giving fields.
- **Kalla & Broockman (VERIFIED-SOFTENED, weak).** In a field experiment, offices met senior officials roughly 3-5x more often when an organized group's members were described as donors rather than constituents. Table 1 ratios: about 3.2 for the Member, 3.4 for legislative director or above, 5.2 for chief of staff or above. Denominator: congressional offices contacted by one liberal organization in 2013. The vault row carries no year; Melange elsewhere cites it as 2016.
- **Furnas et al. 2019 (VERIFIED-SOFTENED, weak).** In a staff survey experiment, a donor requester got *no* meeting advantage over a constituent. Staff were less likely to use the donor's information and rated donors less representative (null among Republican staff).
- **Bonica & Grumbach 2025 (VERIFIED-SOFTENED, moderate).** Political donor dollars are concentrated among the old: in 2020, the median dollar to congressional candidates came from a 66-year-old, and 9% came from donors aged 40 or under. Denominator: itemized individual contributions in DIME linked to L2.

**Net reading.** The direction "civically engaged people contact representatives more" rests only on an UNVERIFIED lead. The two access experiments conflict and do not cover individuals acting alone. Bonica adds an age confound to the giving term.

**In-house fact that changes the stakes (Colorado CD-05, P0, 2026-10-01).** Under 45/45/10, the Sphere top decile is 98.9% volunteers. The contested giving term barely shapes who is at the top. The volunteering term, which no paper here addresses directly, does.

**Wording rule.** Sphere of Influence is a heuristic standing. It is not a probability of contact, access or influence on a member.

**Contradictions.**
- Kalla & Broockman (donor status raises senior-official access about 3-5x; field experiment, one organization, 2013) and Furnas et al. 2019 (donor requester gets no meeting advantage and less information use; staff survey experiment) disagree on whether donor identity helps. The designs differ: revealed status in an organized ask against a hypothetical requester identity. Neither settles the question for an individual constituent who also gives.
- The component's "method" field calls the weights "Ratified 2026-10-01 (owner 'leaning')" and an evidence row calls them a "leaning ruling (PR3)". The brain's P1 entry records "Sphere 45/45/10 kept (ratified)". Status wording differs across surfaces.
- The evidence rows say giving carries 45% of the weight. But the Colorado P0 top decile is 98.9% volunteers, so in practice the giving term barely affects the top of the ranking.

**Gaps.**
- Direct test of the premise: do people in Terrier's top Sphere decile contact their representative more? To fill it: match against a constituent-contact record (an advocacy-platform contact log, or a survey item such as CES "contacted a public official") by Sphere decile, within age bands.
- Volunteering: no paper in the vault addresses whether vendor-modelled volunteers contact officials. The civic voluntarism literature (Verba, Schlozman & Brady) is the obvious search.
- Holyoke 2015 needs verification against the cited primary studies (Holyoke 2013; Fitch 2010) before it counts as evidence.
- Age: there is no within-age standing and no age control. Bonica shows political giving is strongly age-graded.

**Paul's position vs the evidence.**

Paul cites Holyoke et al. 2015 and Bonica & Grumbach 2025, and highlights that prominent constituents call their own members first and that donors are old and wealthy. His ruling reason is that volunteers engage members and stick around.

- The "call first" point comes only from Holyoke, which is UNVERIFIED and cites rather than tests it.
- The "donors are old" point is VERIFIED-SOFTENED (Bonica, 2020 federal itemized donors). It is a confound on the giving term, not support for it.
- The volunteer emphasis is the better-supported channel by default: Furnas suggests the constituent framing beats the donor framing. Still, no vault paper tests volunteers directly.
- Nothing in the vault supports or contradicts the specific 45/45/10 split.

**Open questions.**
- Is the 98.9%-volunteer top decile (Colorado P0) the intended behaviour of 45/45/10, or does it signal a scaling difference between the giving and volunteering inputs?
- What exactly does contrib_high encode in the vendor dictionary? The config does not define it.
- Should Sphere be shown within age band alongside the overall midrank? (Display only, Paul's call.)

**Read first.** [[furnas-2019-moneyed-interests-information]], [[kalla-nd-campaign-contributions-facilitate]], [[bonica-2025-money-campaign-finance]], [[holyoke-2015-learnable-skills-unteachable]]

## Partisan lean (descriptive, never weighted)

**What it computes.** Signed lean from registration (0.5) and vendor partisan fields; bands at +/-25 and +/-75; Undetermined when band confidence < 0.80.

**Method.** Excluded from the overall score.

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/rowfw/engine.py:441,669`

**Assumptions.**
- Registration and vendor codes lie on one axis

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[broockman-2026-should-moving-middle]] | 2026 | qualifies | Public opinion is multidimensional, and voters are often liberal on some issues and conservative on others. Fewer than 10% are consistent across five issues. The authors note an alternative reading: preferences could be one-dimensional with a mapping from that dimension to specific issues that differs between voters and elites. | The stakes are low because lean never enters a weighted score. The paper implies that a mid-band or Undetermined lean often marks cross-pressure on issues rather than a centrist or noisy measurement. That reading applies only if the vendor fields embed issue content; registration does not. | weak | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: one VERIFIED-SOFTENED row, weak (Broockman 2026, qualifies).**

**What the paper shows.** Public opinion is multidimensional. Fewer than 10% of voters are consistent across five issues. The authors allow an alternative reading: one dimension, but voters and elites map it onto issues differently.

**What it means for lean.** The lean score is 100 x (w_reg x registration + w_vendor x the mean of the vendor partisan fields). On the paper's reading, a mid-band or Undetermined lean often marks issue cross-pressure rather than a centrist or a noisy measurement. That applies only if the vendor fields carry issue content; registration does not.

**Why the stakes might be low, and why that may not hold.** The evidence row treats the stakes as low because "lean never enters a weighted score". That premise is contested inside the system itself (see contradictions).

**What the vault lacks.** No evidence on how well vendor partisan scores agree with registration or primary-ballot choice, which is the component's empirical question.

**In-house findings (Colorado P0, 2026-10-01).**
- Test T12 was KILLED as written over a "conservative tag among D" issue. That is a direct sign of the cross-pressure Broockman describes.
- The overall top decile is 42.1% registered R, against 27.4% R in the whole file.

The band cuts (+/-25, +/-75) and the 0.80 confidence rule for Undetermined have no literature behind them.

**Contradictions.**
- The component title and the evidence row say lean is "descriptive, never weighted" and absent from ovr_score. Paul's O-11 answer (2026-10-06) says "numeric partisan and support layers stay in the overall score", and candidate districts (OR-05, CT-01) weight candidate support at 25%. If candidate support uses lean or vendor partisan fields, lean is weighted in those districts. The code (districts.py, engine.py:669-675) has to settle this.
- The design assumes registration and vendor codes lie on one axis. Broockman's multidimensionality finding and the in-house "conservative tag among D" (Colorado P0, T12) both cut against that assumption.

**Gaps.**
- Agreement study: cross-tabulate vendor partisan scores against registration and, where the state records it, primary ballot choice, by district. This is an in-house computation needing no new data in states with party primaries.
- There is no literature on vendor partisanship models' accuracy. Search for validation of commercial partisan scores (for example, the Pew 2018 voter-file vendor comparison).
- There is no justification for the 0.80 Undetermined threshold or the 25/75 band cuts. A sensitivity check on band membership would cover it.

**Paul's position vs the evidence.**

Paul records no literature. The one paper qualifies the one-axis assumption without refuting the descriptive use. His O-11 ruling that numeric partisan layers stay in the overall score conflicts with how the component is described here. That has to be reconciled before any "low stakes" reading of Broockman can stand.

**Open questions.**
- Does candidate support (25% in OR-05 and CT-01) draw on lean or vendor partisan fields? If so, is lean weighted after all?
- How often do vendor partisan fields disagree with registration, by district?
- Should mid-band lean be labelled as possible cross-pressure rather than centrist on display? (Wording is Paul's call.)

**Read first.** [[broockman-2026-should-moving-middle]]

## Overall standing (district weights)

**What it computes.** Colorado: 0.40 CUA + 0.35 Participation + 0.25 Sphere. Candidate districts: 30 CUA / 25 support / 25 participation / 20 sphere. Re-ranked by midrank.

**Method.** Fixed linear weights (PR1; approved 2026-10-07).

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/tools/districts.py:50-154`; `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/rowfw/engine.py:675`

**Assumptions.**
- Standings are commensurable

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bonica-2025-money-campaign-finance]] | 2025 | qualifies | Participation is age-graded at every stage. The median non-voter is 39, the median voter 52, and the median primary voter and median donor are both 59. The authors call these gaps larger than the gap between candidates and donors. Figures are for 2020, from the L2 national file of about 208M registrants. | Federal-level age medians from L2 transfer directionally to any US voter file. The size within OR-05 or CT-01 is unknown until it is measured on Terrier's own file. This bearing is a confound warning, not evidence that age-tilted standings are wrong for advocacy. Paul's ruling is that volunteers and givers are the people who engage members. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: a confound warning only. One VERIFIED-SOFTENED row, moderate (Bonica & Grumbach 2025, qualifies).**

**What the paper shows.** Participation is age-graded at every stage: median non-voter 39, median voter 52, median primary voter and median donor both 59 (2020, L2 national file of about 208M registrants).

**How that reaches the overall score.** Participation (35% in Colorado, 25% in candidate districts) contains turnout. Sphere (25% or 20%) contains political giving. Both are inputs the paper shows to be age-graded. The paper gives no evidence on Digital, Activism, Volunteer or Household. Calling the full weight "age-loaded" would go beyond it.

**What no evidence covers.**
- Whether the overall standing ranks people by realized advocacy action.
- Whether midrank standings are commensurable.
- Either weight set: 40/35/25, approved as the lobbyist weights, or 30/25/25/20, approved 2026-10-07.

**In-house observations (Colorado CD-05, P0, 2026-10-01).**
- The overall top decile is 42.1% registered R, against a 27.4% R share in the file.
- The top decile is 98.1% grade A.

These are composition facts, not validity findings. The only stored stratified view is by registration group; nothing is stratified by age.

**Contradictions.**
- (none recorded)

**Gaps.**
- Criterion validity: does overall standing rank people by realized action (turnout, contacts, canvass response)? The pre-registered 3 Nov 2026 turnout check covers only the Participation part. An advocacy-action criterion needs contact or response data from ACU campaigns.
- Weight sensitivity: how much do top-decile memberships change under alternative weights? Use Kendall tau-b or top-decile overlap (an in-house computation, already in the spec's toolkit).
- Age stratification of the overall top decile in each district, to size the Bonica confound locally.
- Commensurability: summing midrank percentiles assumes equal-interval standings. Nothing tests this.

**Paul's position vs the evidence.**

Paul records no literature. His weights are approved rulings (40/35/25 for Colorado; 30/25/25/20 approved 2026-10-07), and his stated rationale is that volunteers and givers are the people who engage members. The one paper does not argue that the weights are wrong for advocacy. It warns that the standing will partly track age, and the size of that effect in OR-05 and CT-01 is unknown until it is measured on Terrier's files.

**Open questions.**
- What share of each district's overall top decile is aged 60 or over, against the file share?
- Is an age-band view wanted next to the registration-group view? (Display only.)
- What outcome data from ACU campaigns could serve as a realized-action criterion?

**Read first.** [[bonica-2025-money-campaign-finance]]

## Expected-value cells and uncertainty draws (C0, C3)

**What it computes.** Monte Carlo redraws of 26 estimated fields (independent C0, household-correlated C3); summed P(top10); analytic SD assuming independence.

**Method.** Seed 20260930.

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/rowfw/draws.py`; `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/rowfw/engine.py:447-510`

**Assumptions.**
- Imputation probabilities are calibrated
- Cells are independent (C3 probes this)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[harrison-2010-introduction-monte-carlo]] | 2010 | method-source | Inversion method: draw u uniform on [0,1), locate it on the CDF, and take the corresponding x. The paper demonstrates it on the exponential distribution and says it works for any distribution with an invertible CDF. It does not discuss discrete or binary cases; extending it to a categorical inverse CDF (and u < p for binary) is standard but is the analyst's extension, not the paper's text. | The sampling mechanics transfer because the math is the same. Correct mechanics do not make the output correct: the draws are only as good as the imputation class probabilities, whose calibration remains open (the paper's input-quality caveat). Independence across cells in C0 is a model assumption the paper does not address. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: the mechanics are supported, the inputs are not.**

**What supports the mechanics.**
- Harrison 2010 (VERIFIED-SOFTENED, moderate, method-source) supports inverse-CDF sampling. Binary u < p and the categorical cumulative-threshold draws are standard extensions of it, though they are not in the paper's text.
- DeLuca 2022 (applied here by analogy; its evidence rows are attached to CUA and area aggregation) supports propagating class probabilities rather than collapsing to the modal class.

**What no paper addresses.**
- Whether the 26 estimated fields' class probabilities are calibrated. That is the input-quality caveat Harrison himself raises.
- Whether cells are independent. C0 assumes independence. C3 probes it with one uniform per household, and only the four negative lean fields get 1 - u. The analytic SD also assumes independence.

**The calibration bottleneck.** The brain records that the Luna holdout has been used twice and no untouched labelled data remains. Top-1 holdout accuracy is about 86.7% (Colorado, section 30). That is an accuracy figure, not calibration. ECE on held-out data cannot be computed honestly without new labels.

**Open display ruling.** Paul's O-4b answer ("Uncertainty ranges: not answered, so the default stays off") means the draws are computed but not displayed by default.

**Contradictions.**
- (none recorded)

**Gaps.**
- ECE / reliability diagrams for the imputed class probabilities. These need new labelled rows: a fresh vendor vintage that fills previously blank sci_* fields, or a newly purchased match sample. Score them once against a pre-registered gate.
- Dependence structure: compare C0 against C3 on P(top10) and decile churn, then decide whether the analytic SD should carry a household design effect. This is an in-house computation.
- Monte Carlo sufficiency: no record of a convergence check on P(top10) for the replicate count used. The literature on MC standard error is standard; the check itself is in-house.

**Paul's position vs the evidence.**

Paul records no literature. The draws are consistent with his "Expected value + variant" ruling. The evidence backs the sampling arithmetic only. Whether the resulting uncertainty bands mean anything depends entirely on calibration, which is unmeasured.

**Open questions.**
- Where could new labelled rows come from (a vendor vintage refresh, a match sample)?
- How large is the gap between C0 and C3 on P(top10)? Does C3 change any area band?
- Should uncertainty ranges ever be shown, given O-4b left them off by default?

**Read first.** [[harrison-2010-introduction-monte-carlo]]

## Ridit transforms and rule cells

**What it computes.** Ridit = (mass below + half mass at code)/N. Children rule D5a. Ordinal maps for owner, homeowner and dwelling.

**Method.** Bross ridit.

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/rowfw/engine.py:205`

**Assumptions.**
- Ordinal order is supported; spacing is arbitrary

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment.**

**Evidence weight: none. GAP.**

No evidence row bears on the ridit transforms, the children rule (D5a), or the ordinal maps for owner, homeowner and dwelling.

**Method source only.** The system cites Bross (1958) for the ridit, but there is no paper in the vault assessing ridit scoring against alternatives in a ranking context.

**The assumption at stake.** The ordinal order is supported and the spacing is arbitrary. Ridits are a defensible choice under that assumption, because they make the spacing follow the sample distribution rather than equal intervals. But ridits are distribution-dependent: the same code gets a different ridit in OR-05 and CT-01 if the code shares differ. That matters for any cross-district comparison.

Whether any of this materially changes rankings is an in-house computation, not a literature question.

**Contradictions.**
- (none recorded)

**Gaps.**
- Sensitivity study: re-rank each district under equal-interval coding and under ridits, and report top-decile overlap and Kendall tau-b. This is an in-house computation with no new data; Kendall tau-b is already in the spec's toolkit.
- Cross-district comparability: ridits are relative to each district's own distribution. Document whether any surface compares standings across districts.
- Literature: a methodological source on ridit analysis and ordinal scaling (for example, Agresti on ridits, or optimal-scaling work) to anchor the choice. No such paper is in the vault.
- The children rule D5a and the dwelling/owner ordinal maps have no recorded justification beyond design choice.

**Paul's position vs the evidence.**

Paul records no literature, and the vault offers none to compare against. The choice is an unexamined design default. It is not contradicted by any evidence.

**Open questions.**
- Does ridit against equal-interval coding change top-decile membership by more than a trivial share in any district?
- Are cross-district standings ever compared on a surface, given that ridits are district-relative?

**Read first.** (none)

## Area aggregation with empirical-Bayes shrinkage

**What it computes.** Beta-binomial EB-shrunk top-decile share per area (block -> tract -> file) using a design effect; area standing and bands.

**Method.** Summed probabilities, not flags. Households use the mean of member percentiles. Suppression is superseded in the app (nothing withheld).

**Code refs.** `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/rowfw/areas.py:406-458`; `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/tools/measures.py`

**Assumptions.**
- Areas are exchangeable within the parent

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[deluca-2022-validating-applicability-bayesian]] | 2022 | supports | Researchers should aggregate to a polygonal unit by summing estimated probabilities of racial membership. Assigning race to single voters makes errors drastically higher. | The summed-probability step matches the paper's recommended aggregation. The paper does not test EB shrinkage or top-decile shares, so it says nothing about the shrinkage layer. Its expectation of larger error in heterogeneous areas is where shrinkage matters most. This is an analogy for the aggregation choice, not a validation of Terrier area values. | moderate | VERIFIED |

**Assessment.**

**Evidence weight: the aggregation step is supported, the shrinkage step is not. One VERIFIED row, moderate (DeLuca et al. 2022, supports).**

**What is supported.** The paper recommends aggregating to a polygon by *summing estimated probabilities* rather than assigning classes to individuals, which it reports makes errors "drastically higher". Terrier does exactly this: summed P(top10) contributions divided by row count (areas.py 435-438). This is the best-supported design choice in the whole system.

**Why it is still an analogy.** The paper validates race shares from BISG, not top-decile shares of a composite score.

**What is not tested.**
- The beta-binomial EB shrinkage with a design-effect variance (areas.py 444-448).
- The assumption that areas are exchangeable within their parent (block, then tract, then file).

DeLuca's expectation of larger error in heterogeneous areas is where shrinkage would matter most. Even so, the paper says nothing about whether shrinkage helps.

**Other notes.** Households use the mean of member percentiles, which has no evidence either way. Suppression is superseded: nothing is withheld in any district, by Paul's ruling of 2026-10-08. The P0 review had found areas could be rebuilt by subtraction. No paper in the vault speaks to that disclosure risk, and it is noted here as a fact only.

**Contradictions.**
- (none recorded)

**Gaps.**
- Stability study: do EB-shrunk area rankings agree better across file vintages, or across random split-halves of each area's people, than raw shares do? Split-half within one vintage is possible now; across vintages needs the next vendor file.
- Exchangeability: test whether area top-decile shares cluster by covariates (density, registration mix) within a tract. If they do, a covariate-informed prior (Fay-Herriot-style small-area estimation) would beat a plain beta-binomial. Search the literature on small-area estimation and EB disease-mapping (Clayton & Kaldor 1987).
- Design effect: document how the deff is estimated and whether it uses the household clustering that C3 probes.

**Paul's position vs the evidence.**

Paul records no literature. The VERIFIED paper supports the summed-probability aggregation that Terrier uses. Nothing in the vault evaluates the EB shrinkage layer, so it is neither supported nor contradicted.

**Open questions.**
- Does EB shrinkage materially change area bands in small blocks, now that nothing is withheld?
- Is the beta-binomial prior parent-level only, or does it use covariates?
- Does Paul want the subtraction-rebuild disclosure finding from P0 re-examined under the nothing-withheld ruling? (This is a question for Paul, not a recommendation.)

**Read first.** [[deluca-2022-validating-applicability-bayesian]]

## Across components

### Patterns across components

**1. One calibration assumption carries three components.** CUA's expected values, the Monte Carlo draws, and the EB area shares all assume the imputed class probabilities are calibrated. DeLuca 2022 supports *using* probabilities rather than argmax, but only if they are roughly calibrated. No ECE has been measured. The brain records that the Luna holdout has been used twice and no untouched labelled data remains. The 86.7% top-1 holdout accuracy (Colorado, section 30) does not answer the calibration question. New labelled rows are the single highest-value input across the system.

**2. Age runs through Participation, Sphere and Overall.** Bonica & Grumbach 2025 (VERIFIED-SOFTENED) show that turnout and political giving are steeply age-graded (2020, L2 national file and DIME-linked itemized donors). Two of the three Colorado overall inputs (60% of the weight) and three of four candidate-district inputs contain age-graded items. Only a registration-group stratification is stored. This is a confound to measure locally, not proof the standings are wrong.

**3. In-house diagnostics outweigh the literature.** From the Colorado P0 run (491,308 rows, 2026-10-01), unless noted:
- The Sphere top decile is 98.9% volunteers.
- CUA rho 0.698 with CPI volume.
- The recession item's item-rest correlation is -0.35.
- The overall top decile is 42.1% R against 27.4% in the file.
- The vendor turnout mean t is 0.55 against a 0.84 general-election record (OR-05, 2026-10-09).

These say more about Terrier than any paper in the vault. They should be read before the papers.

**4. Internal contradiction on partisan lean.** The component is labelled "never weighted", but O-11 (2026-10-06) keeps numeric partisan and support layers in the overall score, and candidate districts weight candidate support at 25%. This has to be reconciled from code before lean evidence is interpreted.

**5. Transfer conditions are weak throughout.**
- Every behavioural paper is national or federal: 2020 DIME/L2, one organization's 2013 field experiment, a congressional staff survey.
- The method paper is about race posteriors in North Carolina and Georgia.
- None is a district voter-file study of the outcome Terrier ranks on.
- The two access experiments (Kalla & Broockman; Furnas et al.) disagree, and neither covers individual constituents acting alone.

**6. Project wording rules.**
- Sphere of Influence and the overall standing are heuristic standings, never contact, access or influence probabilities.
- Every quoted figure above names its window and denominator.
- No paper here argues for the two rejected SNA proposals (dropping regular cosponsorship edges; per-edge volume normalization), and none uses LDA, so there is nothing to flag on those rules.
- The one UNVERIFIED bearing (Holyoke 2015) is a lead only.

## Research agenda

1. Reconcile the partisan-lean contradiction from code: does candidate support (25% in OR-05 and CT-01) or anything in ovr_score draw on lean or vendor partisan fields, given Paul's O-11 answer that numeric partisan layers stay in the overall score?
2. Obtain new labelled rows (a fresh vendor vintage or a match sample) and pre-register a single ECE / reliability look on the imputed class probabilities. This unblocks CUA, the uncertainty draws and the EB area shares together.
3. Pre-register the 3 Nov 2026 turnout check (already a council default): score Participation standing and calibrated t once against validated turnout by district, before the history update arrives.
4. Age-stratify the top deciles: report the age composition of the overall, Participation and Sphere top deciles against the file share in CO, OR-05 and CT-01, to size the Bonica & Grumbach confound locally (display ruling Paul's).
5. Find a realized-action criterion for Sphere and overall standing (an ACU campaign contact log, canvass response codes, or a survey contact item) and test decile gradients within age bands.
6. Obtain and read Nickerson & Rogers 2014 (listed by Paul, missing from the vault) and search for validation studies of commercial voter-file turnout, partisan and consumer models.
7. Verify Holyoke 2015's cited claims against the primary sources (Holyoke 2013; Fitch 2010) before treating it as evidence for Sphere of Influence; search the civic voluntarism literature on volunteers' contact rates.
8. Run the in-house sensitivity computations: ridit against equal-interval re-ranking (top-decile overlap and Kendall tau-b); alternative overall weights; CUA partialled on CPI volume; C0 against C3 P(top10) churn.
9. Test EB shrinkage stability: compare raw and shrunk area rankings with split-half (now) and across file vintages (next file), and check within-parent exchangeability against density and registration mix (search small-area-estimation and EB disease-mapping literature).
10. Ask Paul whether the recession item stays in the CUA credit theme given its item-rest correlation of -0.35, and whether the CUA theme weights are to be ratified.
