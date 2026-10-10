---
type: system
name: "Melange stance labels, say-do gate and tier boards"
slug: melange-labels-and-tiers
components: [melange-labels-and-tiers.category-stance-ladder, melange-labels-and-tiers.say-do-gate, melange-labels-and-tiers.overall-tier, melange-labels-and-tiers.rank-honors, melange-labels-and-tiers.primacy-board, melange-labels-and-tiers.per-area-grade, melange-labels-and-tiers.industry-boards-crown]
brain_notes: [concepts/say-do-gate.md, concepts/stance-pipeline-integrity-contract.md, concepts/behavior-qualified-recognition.md, topics/2026-07-feature-primacy-and-industry-tier-boards.md]
paper_count: 9
tags: [system]
---

# Melange stance labels, say-do gate and tier boards

Turns signed evidence into displayed labels. It covers the fixed ten-band per-category ladder, the IMP-weighted overall tier, the say-do (costly-signal) gate, the action-based rank honors (rule G), the primacy (CU-purity) tier board, the per-area grade and the per-industry boards with the crown law. Labels are the alignment axis and never feed the rank pillars.

Brain notes: [concepts/say-do-gate](obsidian://open?vault=brain&file=concepts%2Fsay-do-gate); [concepts/stance-pipeline-integrity-contract](obsidian://open?vault=brain&file=concepts%2Fstance-pipeline-integrity-contract); [concepts/behavior-qualified-recognition](obsidian://open?vault=brain&file=concepts%2Fbehavior-qualified-recognition); [topics/2026-07-feature-primacy-and-industry-tier-boards](obsidian://open?vault=brain&file=topics%2F2026-07-feature-primacy-and-industry-tier-boards)

**System:** Melange stance labels, say-do gate and tier boards (`melange-labels-and-tiers`). This layer turns signed evidence into the labels people see: the per-category ladder, the overall tier, the say-do gate, rule G rank honors, the primacy board, the per-area grade, and the industry boards with the crown law. Labels sit on the alignment axis and do not feed the rank pillars.

**Evidence base:** 10 bearing rows from 9 papers.
- 1 row is VERIFIED-only: Fouirnaies & Hall 2022, on the say-do gate. A second VERIFIED row (Poole 1999, method-source) bears on the ladder.
- 7 rows are VERIFIED-SOFTENED.
- 1 row is UNVERIFIED: the anonymous strategic identity-signalling paper. It is a lead only (DE-062).

**Where the rows land:**
- Six of the ten rows land on the **say-do gate**, which makes it the only well-covered component.
- Most of those rows **qualify** the gate's two corroboration paths rather than support them:
  - Path A: sponsorship is partly display aimed at voters.
  - Path V: one Yea on an uncontested or party-sorted roll call shows little about CU commitment.
  - The thin-high-volume rule: low advancement rates track majority, chair and seniority status. This finding comes from pooled state-legislature data.
- The **industry boards** have two moderate rows. Neither tests the crown law.
- The **ladder** and the **overall tier** each have one weak row: a method source and an analogy.

**Gaps:** Three components have no bearing at all: **rank honors (rule G)**, **primacy board** and **per-area grade**. Each needs a Congress-specific prospective validation study, described per component below.

**Paul's positions vs the evidence:**
- Paul's literature positions cover only three components: say-do, rank honors and industry boards.
- In two cases the cited paper's real use in the code, or its verified bearing, differs from the role Paul gives it:
  - Fouirnaies & Hall is cited for say-do, but the code uses it for the receptivity incentive multiplier.
  - Kim & Kunisky is cited for rival-industry champions, but its biLCM carries no sign.
- Kim, Stuckatz & Wolters 2020/2026 has no bearing row on any component here. Its in-system use (leadership PAC edges) is "identified, not implemented".

**Transfer conditions are weak across the board:**
- Bucchianeri and Fouirnaies are state-legislature results.
- Poole ends at the 105th Congress (1998).
- Porter covers the 107th House only.
- Thakur covers Senate Democrats, 1947-1995.
- Broockman studies voters, not legislators.

None of these is a Congress 117-119 result about CU-relevant bills.

Row status counts (computed from the evidence rows): UNVERIFIED 1, VERIFIED 2, VERIFIED-SOFTENED 7; 9 distinct papers.

## Fixed ten-band per-category stance ladder

**Computes:** Label per member x 18 categories from a signed raw score.

**Method:** Bands: >=0.70 Champion/Defender, >=0.35 Ally, >=0.25 Highly Persuadable, >=0.15 Persuadable, >=0.05 Lean Persuadable, >=-0.05 True Neutral, >=-0.15 Lean Skeptic, >=-0.35 Skeptic, >=-0.70 Highly Skeptical, else Opponent; no evidence shows an em dash. Legislation uses a sqrt scale of 2.0 and media a scale of 1.0. Exceptional rounding adds at most +/-0.05. Since 2026-08-03 four domains use signed empirical midrank CDF curves. DOC CONFLICT: MATHEMATICAL_SPECIFICATION.md line 14 describes median+k*IQR cuts.

**Code refs:** `fec/stance/stance_rules.py:12-20`, `fec/stance/per_category_stance.py`

**Assumptions:**
- Missing evidence is never imputed as neutral
- Signed scores are comparable across members within a category

**Brain notes:** [concepts/stance-pipeline-integrity-contract](obsidian://open?vault=brain&file=concepts%2Fstance-pipeline-integrity-contract)

**Evidence (1 rows; VERIFIED 1):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | method-source | Appendix eqs. A2-A3: utility for an outcome is beta times exp of minus the weighted squared distance plus a normal error; the choice probability is a probit on the utility difference between Yea and Nay. Table 2: beta 4.125 (House, 2D linear). | Testing band edges against vote probabilities needs a member x CU roll-call matrix with dates, which the brain records as not in the repo. Paul's FreeText 'Most important part' sits on the appendix. | weak | VERIFIED |

**Assessment**

**Evidence weight: very thin. Effectively a GAP for validation.**

Only one row bears on the ladder: `poole-1999-nominate-after-years` (VERIFIED, method-source, weak).
- It supplies a vote-choice model: utility is beta times exp of minus the weighted squared distance, and choice is a probit on the Yea-Nay utility difference (appendix eqs. A2-A3).
- The model could map band edges onto vote probabilities. The House 2D linear beta is 4.125 (Table 2).
- It does not validate the cut points 0.70/0.35/0.25/0.15/0.05/-0.05/-0.15/-0.35/-0.70.
- The test needs a dated member x CU roll-call matrix. The brain records that matrix as not in the repo.
- Paul's FreeText note "Most important part" sits on that appendix. That suggests he sees the paper as the route to calibrating the bands, not as support for the bands as they stand.

**Internal inconsistency (not literature):** three descriptions of how the cuts are set coexist.
- MATHEMATICAL_SPECIFICATION.md line 14 describes median + k*IQR cuts.
- The code uses FIXED_BANDS.
- Since 2026-08-03, four domains use signed empirical midrank CDF curves.

The ladder's band meaning therefore differs by domain. No evidence says whether the fixed or the curved bands are better calibrated.

**The gap and the study that would fill it:**
1. Build a dated member x CU roll-call matrix restricted to contested votes.
2. Fit a one-dimensional probit/IRT per category, in Poole's A2-A3 form.
3. Map each band edge to a predicted P(CU-aligned vote) on a held-out later Congress.
4. Compare fixed bands with the curved CDF domains on calibration (reliability curves, Brier score), stratified by party, because the CU categories sort by party (see the say-do Poole row).

The cross-member comparability assumption ("signed scores are comparable across members within a category") is also untested. Members with very different evidence volume may not be on the same scale.

**Contradictions**
- Doc conflict: MATHEMATICAL_SPECIFICATION.md line 14 (median + k*IQR) vs FIXED_BANDS in fec/stance/stance_rules.py:12-20 vs signed empirical midrank CDF curves in four domains since 2026-08-03. One ladder cannot be all three.

**Gaps**
- No study tests whether band edges correspond to vote-probability thresholds on CU roll calls; Poole 1999 gives only the model form.
- No evidence compares calibration of curved-domain labels vs fixed bands against subsequent behaviour.
- The assumption that signed scores are comparable across members within a category is untested. Members differ widely in evidence volume.
- The data dependency is missing: a dated member x CU roll-call matrix is recorded as not in the repo.

**Paul's position vs the evidence**

Paul cites no literature for the ladder. His only engagement is a FreeText note marking the Poole appendix's vote-probability model as the most important part. That points to a calibration method; it is not evidence that the current cut points are right. Neither his note nor the evidence addresses the three-way documentation conflict on how the cuts are set.

**Open questions**
- Which description of the ladder is canonical: fixed bands, median+k*IQR, or curved CDF per domain? Should MATHEMATICAL_SPECIFICATION.md line 14 be corrected?
- Is building a dated member x CU roll-call matrix (contested votes only) in scope, so the band edges can be mapped to vote probabilities?
- For the curved domains, is the midrank CDF computed within party, within chamber, or pooled? A pooled curve over a party-sorted category would need an UNSTRATIFIED label.

**Read first:** [[poole-1999-nominate-after-years]], [[porter-2005-network-analysis-committees]], [[broockman-2026-should-moving-middle]]

## Say-do (costly-signal) gate and delivery multiplier

**Computes:** Caps Champion/Defender cells to Ally unless corroborated, applies an overall Champion audit cap, and sets a delivery discount on engine targets.

**Method:** Cell gate (fail-closed since 742b68aec): path A = authored or original-cosponsored bill in the category; path V = CU-aligned final-passage vote (lean >= 0.5). Path J (committee gatekeeping) was removed; VERIFIED absent from the current moneyball_core.py. Overall gate: at least 6 top cells, 2/3 delivered, and for >=10 direct bills at least 20% advanced (waived at >=20 bills with all top cells delivered). The overall gate is display-only since 391a4a8b5 and fails open. delivery_mult = clip(1 - 0.15 max(0, gap-1), 0.70, 1.00).

**Code refs:** `fec/moneyball_core.py:880-966 (cap_uncorroborated, overall_say_do_gate)`, `fec/vote_category_lean.json`, `say_do_gap.py`

**Assumptions:**
- Sponsorship and final-passage votes are costly signals; committee office is not an action
- High-volume introduction with little advancement indicates cheap talk

**Brain notes:** [concepts/say-do-gate](obsidian://open?vault=brain&file=concepts%2Fsay-do-gate); [topics/2026-07-feature-influence-engine-internals-gate2-and-say-do](obsidian://open?vault=brain&file=topics%2F2026-07-feature-influence-engine-internals-gate2-and-say-do)

**Evidence (6 rows; UNVERIFIED 1, VERIFIED 1, VERIFIED-SOFTENED 4):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[fouirnaies-2022-electoral-incentives-affect]] | 2022 | qualifies | Sponsorship is effort directed at voters' inferences, not a revelation of position: it falls when reelection ends while ideology stays flat. Legislators advertise bills introduced on their webpages. | Weak. The paper supports the thin_high_volume reading, since introduction volume responds to electoral display. It also cautions that path A sponsorship is partly advertising, though a bill signed in the CU direction is still more than talk. The evidence comes from state legislatures with bill-introduction caps in half the states (p.666), and the paper does not test whether a bill's direction corroborates rhetoric. | weak | VERIFIED |
| [[poole-1999-nominate-after-years]] | 1999 | qualifies | The second dimension now adds less than 1 percent improvement in classification over the first in both chambers (p.7). Party overlap, measured as the share of a party's members closer to the opposing party's mean than to their own, is under 1.5 percent in both chambers for the past 5 Congresses, with none in the past 3 Senates (p.15). The paper does not discuss final-passage votes specifically; the inference that a single final-passage vote mostly records party position is the analyst's, not the paper's. | Paper evidence ends at the 105th Congress (1998). The party split is a re-run of the same function, not an independent second method, so it stays UNVERIFIED under DE-062 and is quoted stratified by party only. The qualification applies to the party-sorted categories, not to Mortgage & Housing, Fraud Prevention or Defense CU Issues. | strong | VERIFIED-SOFTENED |
| [[porter-2005-network-analysis-committees]] | 2005 | qualifies | In the 107th House roll-call matrix (990 votes, 2001-02) the leading SVD coordinate is the bipartisan one, which groups nearly all members together. It holds 45.3% of the sum of squares, against 29.6% for the partisan coordinate. Always guessing 'pass' is right on 735 of 990 votes (74.2%). The Optimal Classification comparison keeps 749 of 990 roll calls after dropping votes with a small minority share; the threshold glyph is lost in extraction. | Paper supports only that bipartisan variance dominates the 107th House matrix. Its 45.3% is for all 990 votes in 2001-02, not a 117-119 statistic and not a share of lopsided votes in CU data. The share of admitted roll calls with minority at or below a small threshold is unmeasured; this is a candidate check, UNVERIFIED until reproduced (DE-062). The code read does not establish that the source is final-passage only. | moderate | VERIFIED-SOFTENED |
| [[thakur-2025-matching-theory-perspective]] | 2025 | supports | Freshman Senate Democrats list Prestige committees as first choice 8% less often than non-freshmen (p=0.15, Frisch-Kelly 1947-1995), consistent with strategic hedging under the Boston mechanism. Republican freshmen show no such gap (p=0.80, Dole archive, first choices only, 91st and 103rd Congresses). The paper treats Democratic strategic reporting as a prediction it cannot test (Prediction 1). | Senate Democrats, descriptive, 1947-1995. The 8 percent gap is not significant at conventional levels (p=0.15). Supports excluding seat-holding as corroboration of an action; says nothing about markup or floor votes. | moderate | VERIFIED-SOFTENED |
| [[bucchianeri-2025-legislative-effectiveness-american]] | 2025 | qualifies | Bill advancement is tilted toward majority members (+0.370 pooled, 58 percent above minority mean), chairs (+0.507 pooled; 0.609 lower, 0.307 upper), senior members (+0.032 per term) and moderates (distance from median -0.114). These are associational estimates; the paper does not measure effort and does not show that low minority advancement reflects position rather than effort. | The coefficients are state-level pooled estimates and must carry strata or labels. A low advancement rate is not evidence of cheap talk without a within-chamber, majority-adjusted baseline, so the rel's caution holds. | weak | VERIFIED-SOFTENED |
| [[vanderdoes-2022-strategic-identity-signaling]] |  | suggests-extension | Under mixed audiences people cut overt signaling but keep some covert signals, and those who switched strategy earned more (SI Fig. S8). Low visible talk is a strategic response to audience, not lack of position. | An action-without-talk audit would be a display-side diagnostic, not a rank change. It goes through zero-rank-delta and change control, and it stays UNVERIFIED until a second path reproduces it. The paper gives a mechanism, not legislator evidence. | weak | UNVERIFIED |

**Assessment**

**Evidence weight: the best-covered component.** It has 6 rows: 1 VERIFIED, 4 VERIFIED-SOFTENED and 1 UNVERIFIED. The net direction is that the gate is the right idea but each corroboration path is leakier than the code assumes.

**Path A: authored or original-cosponsored bill**
- `fouirnaies-2022-electoral-incentives-affect` (VERIFIED, weak, state legislatures) finds that sponsorship falls when reelection incentives end while ideology stays flat. Legislators also advertise the bills they introduce.
- So sponsorship is effort aimed at voters' inferences, not a revelation of position.
- A bill signed in the CU direction is still more than talk. But path A partly measures display, and the paper does not test whether bill direction corroborates rhetoric.
- Transfer caveat: about half the states in the sample cap bill introductions (p.666), and Congress has no such cap.

**Path V: CU-aligned vote, lean >= 0.5 with n >= 1**
- `porter-2005-network-analysis-committees` (VERIFIED-SOFTENED, moderate) covers the 107th House, 990 roll calls, 2001-02:
  - The bipartisan SVD coordinate holds 45.3% of the sum of squares, against 29.6% for the partisan coordinate.
  - Always guessing "pass" is right on 735 of 990 votes (74.2%).
- The verifier's code read found that `build_vote_category.py` applies no contestedness or minority-share filter across HS117-119. So one Yea on a lopsided roll call gives lean 1.0 with n=1 and passes the gate.
- Two caveats. The 45.3% figure is a 2001-02 statistic, not a 117-119 one. And the share of admitted CU roll calls that are lopsided is unmeasured, so the size of the leak is UNVERIFIED (DE-062).
- `poole-1999-nominate-after-years` (VERIFIED-SOFTENED, strong on the paper claim):
  - The second dimension adds under 1% classification improvement in either chamber.
  - Party overlap is under 1.5% in both chambers for the last 5 Congresses, and zero in the last 3 Senates. The paper's data end at the 105th Congress (1998).
- A same-function re-run on 2026-10-09 shows that path V corroboration sorts by party in several categories. Window: current vote_category_lean.json. Denominators: D 259 and R 271 from moneyball_core.party(); I 3 excluded.
  - CFPB: D 4/259 (2%) vs R 179/271 (66%).
  - Technology & Emerging: D 18/259 (7%) vs R 263/271 (97%).
  - Examination & Supervision: D 106/259 (41%) vs R 261/271 (96%).
  - CU Capital Requirements: D 9/259 (3%) vs R 149/271 (55%).
  - Charter Modernization: D 215/259 (83%) vs R 70/271 (26%).
  - NCUA Budget & Insurance: D 145/259 (56%) vs R 51/271 (19%).
  - The pattern does not hold everywhere. Mortgage & Housing is D 78% vs R 74%, Fraud Prevention 63% vs 68%, and Defense CU Issues 57% vs 58%.
- This split re-ran the same function rather than using an independent method, so it stays an **UNVERIFIED lead** under DE-062. It may be quoted only stratified by party, as above.
- **Reading:** in the party-sorted categories, path V may corroborate party membership rather than CU commitment.

**Path J removal (committee seat as corroboration)**
- `thakur-2025-matching-theory-perspective` (VERIFIED-SOFTENED, moderate) gives modest support for dropping committee seat-holding as evidence of action.
- Freshman Senate Democrats list Prestige committees first 8% less often than non-freshmen. The difference is not significant (p=0.15; Frisch-Kelly data, 1947-1995). Republican freshmen show no gap (p=0.80).
- That is consistent with assignments reflecting strategic hedging, not revealed commitment.
- It is weak and old, and it says nothing about markup or committee floor activity.

**Overall gate: thin_high_volume rule (at least 10 direct bills, at most 20% advanced)**
- Two rows pull in opposite directions:
  - Fouirnaies & Hall supports reading introduction volume as display.
  - `bucchianeri-2025-legislative-effectiveness-american` (VERIFIED-SOFTENED, weak) finds advancement tilted toward particular members. Its estimates are pooled across state legislatures (UNSTRATIFIED by state; the chair estimate is split by chamber):
    - majority members: +0.370, 58% above the minority mean;
    - chairs: +0.507 pooled (0.609 lower chamber, 0.307 upper chamber);
    - seniority: +0.032 per term;
    - moderates: distance from the median -0.114.
- A low advancement rate is therefore not evidence of cheap talk unless it is judged against a within-chamber, majority-adjusted baseline.
- The rule makes no adjustment for majority, chair or seniority. A minority-party CU sponsor is structurally more likely to be capped.
- Two mitigations limit the harm: the overall gate is display-only (since 391a4a8b5) and fails open.

**Lead (UNVERIFIED)**
- `vanderdoes-2022-strategic-identity-signaling` suggests that muted talk can be a strategic response to a mixed audience.
- The gate only caps rhetoric that runs ahead of action. Nothing flags members whose actions are CU-aligned while their rhetoric is neutral.
- An action-without-talk display diagnostic is a candidate idea, not a finding. It would need zero-rank-delta and change control.

**Net:** no row contradicts having a say-do gate. Every row on the two corroboration paths says they need filters: contestedness for V, party stratification for V, and a majority/chair baseline for the advancement rule. No row is Congress-era evidence on CU bills.

**Contradictions**
- Assumption 'final-passage votes are costly signals' vs Porter 2005 (107th House, 990 roll calls: 74.2% predictable by always guessing 'pass') and Poole 1999 (party overlap under 1.5%). Path V admits any Yea with n>=1 and no contestedness filter. The leak size on CU roll calls is unmeasured.
- Assumption 'high-volume introduction with little advancement indicates cheap talk' vs Bucchianeri 2025: advancement tracks majority status, chair and seniority (state legislatures, pooled, UNSTRATIFIED by state). The rule has no majority/chair adjustment. Fouirnaies & Hall 2022 partly supports the display reading, so the two papers pull in opposite directions.
- Paul's sources frame committee activity as costly effort, while the system removed path J (committee) and Thakur 2025 (weak, p=0.15) supports excluding seat-holding. This is not a true conflict, because seat-holding is not activity, but committee markup activity is now unrepresented in the gate.
- Fouirnaies & Hall 2022: sponsorship is effort aimed at voters' inference, not a revelation of position. That qualifies path A's treatment of sponsorship as a costly signal of position.

**Gaps**
- No Congress-era (117-119) study of whether gate-failing members show lower later CU-aligned behaviour than corroborated members with the same rhetoric. This needs a prospective comparison matched on rhetoric score, stratified by party and chamber.
- The share of admitted CU roll calls that are lopsided (minority share below a small threshold) is unmeasured. It is a simple counting check on build_vote_category.py output.
- No majority/chair/seniority-adjusted baseline for advancement rates in Congress. Volden-Wiseman-style LES data (cited elsewhere in the system) could supply one, but no bearing row covers it here.
- Kim, Stuckatz & Wolters 2020/2026, cited by Paul for this component, has no bearing row.

**Paul's position vs the evidence**

Paul's sources (Fouirnaies & Hall 2022; Kim, Stuckatz & Wolters) frame sponsorship and committee activity as costly, observable effort. The verified Fouirnaies & Hall bearing narrows that: sponsorship is effort, but effort directed at voter display, and it falls when reelection incentives end. In the system, the paper is actually used for the receptivity incentive multiplier, not the gate. Kim, Stuckatz & Wolters has no verified bearing on this component. His notes do not discuss the gate itself. The system design is stricter than his sources in one respect, since committee is not counted, and laxer in another, since any single Yea counts.

**Open questions**
- Should path V require a contested roll call (minority share above a threshold) or n >= 2 before it corroborates? That is a display change requiring zero-rank-delta and change control, and the decision is Paul's.
- Should path V corroboration be reported stratified by party in the party-sorted categories (CFPB, Technology & Emerging, Examination & Supervision, CU Capital Requirements, Charter Modernization, NCUA Budget & Insurance)? An independent second method is needed first to lift the split out of UNVERIFIED.
- Does build_vote_category.py admit only final-passage votes or all roll calls? The verifier's code read did not establish this.
- Should the thin_high_volume rule compare against a within-chamber majority/minority baseline?
- Is an action-without-talk diagnostic wanted? Its source is UNVERIFIED and it is a lead only.

**Read first:** [[porter-2005-network-analysis-committees]], [[poole-1999-nominate-after-years]], [[fouirnaies-2022-electoral-incentives-affect]], [[bucchianeri-2025-legislative-effectiveness-american]], [[thakur-2025-matching-theory-perspective]], [[vanderdoes-2022-strategic-identity-signaling]]

## Overall tier (IMP-weighted aggregate) and Champion crown

**Computes:** Overall label = band of the IMP-weighted mean of tier prototypes (Champion 4 ... Opponent -4), with a +/-0.05 nudge. The display Champion is the primacy board top 13.

**Method:** Band cutoffs at 3.0, 2.0, 1.0, 0.4, 0.0, -0.3, -0.75, -1.5. IMP weights include Interchange 2.0, NCUA 1.0, Exam 0.75 and Capital 0.25. Unknown cells are excluded.

**Code refs:** `fec/moneyball_core.py:631-712`, `fec/stance/overall_stance_policy.py`, `fec/sync_stance_clean.py`

**Assumptions:**
- IMP weights reflect relative issue importance (judgment)
- Silence does not dilute earned tiers

**Brain notes:** [concepts/stance-pipeline-integrity-contract](obsidian://open?vault=brain&file=concepts%2Fstance-pipeline-integrity-contract)

**Evidence (1 rows; VERIFIED-SOFTENED 1):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[broockman-2026-should-moving-middle]] | 2026 | qualifies | Gains and losses across issues and voters can net to modest aggregate effects, so a small average conceals larger, opposite-signed issue-level responses. In a stylized example (Figure A7), 50% more voters change their votes than the average net effect implies. | This is an analogy on how aggregation behaves, with a different population: legislators' issue positions are far more one-dimensional than voters'. The point is a reading caution. A middle overall tier should not be read as 'persuadable everywhere'. Before acting on the overall label, check the dispersion of the per-topic cells. The paper provides no legislator evidence. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: one weak analogy. Effectively a GAP for validation.**

The only row is `broockman-2026-should-moving-middle` (VERIFIED-SOFTENED, weak).
- When gains and losses across issues net out, a small average can hide larger opposite-signed issue-level responses. In a stylized example (Fig. A7), 50% more voters change their votes than the net effect implies.
- The overall tier is a band of the IMP-weighted mean of tier prototypes (Champion 4 ... Opponent -4) plus a +/-0.05 nudge.
- So a member who is Champion on Interchange and Opponent on Capital can average into a middle band.
- The design's "silence does not dilute" rule excludes unknown cells. It does not prevent cancellation between opposite-signed cells.

**Transfer is weak.**
- Broockman studies voters. Legislators' positions are far more one-dimensional (see Poole 1999 under the say-do gate), so cancellation may be rarer in Congress than among voters.
- The paper gives a **reading caution**, not evidence: a middle overall tier should not be read as "persuadable everywhere" without checking how spread out the per-category cells are.

**Why this matters downstream:** `fec/engine/money_response.py` reads the overall tier. A cancelled-out middle label can therefore travel into the money layer.

**The gap and the study that would fill it:**
- The IMP weights (Interchange 2.0, NCUA 1.0, Exam 0.75, Capital 0.25) are judgment calls with no empirical source.
- The study: on a held-out Congress, compare whether the IMP-weighted overall tier, an unweighted mean and a dispersion-aware summary (for example, mean plus within-member SD, or the highest-IMP cell alone) predict votes on the highest-IMP issues.
- Report it stratified by party and chamber.
- Also count how many members have opposite-signed top cells, to see whether cancellation happens often enough to matter.

**Contradictions**
- No direct contradiction. Broockman 2026 qualifies the aggregation: an IMP-weighted mean can net opposite-signed cells into a middle band, which the 'silence does not dilute' rule does not guard against.

**Gaps**
- No study tests whether the IMP-weighted overall stance predicts highest-IMP votes better than an unweighted mean (the component's own empirical question).
- No empirical source for the IMP weights (Interchange 2.0, NCUA 1.0, Exam 0.75, Capital 0.25). They are judgment calls, which is acceptable if labelled as such.
- The frequency of opposite-signed top cells within a member is unmeasured. It is a simple count on the current label table.
- No legislator-level evidence on aggregation; the only row concerns voters.

**Paul's position vs the evidence**

Paul cites no literature here. The design (IMP weights as judgment; unknown cells excluded) is consistent with his stated intent, and nothing in the evidence contradicts it. The one bearing is an analogy, from a different population, that warns against reading middle bands at face value.

**Open questions**
- Should the overall label carry a dispersion flag when per-category cells have opposite signs?
- Labels are said never to feed the rank pillars, yet money_response.py reads the overall tier. Is the money layer outside the rank pillars by definition, and is that boundary documented?
- Is the 'display Champion = primacy board top 13' crown consistent with the IMP-weighted overall label when they disagree, and which wins on screen?

**Read first:** [[broockman-2026-should-moving-middle]], [[poole-1999-nominate-after-years]], [[porter-2005-network-analysis-committees]]

## Action-based rank honors (rule G ladder)

**Computes:** Recognition labels per policy field from accepted actions only.

**Method:** Base: signed action sum with role weights 3/2/1, votes 1, and Congress decay 1/0.8/0.64; cutoffs at 5/15/20% of N. Rule G (default since 2026-09-16): a signed rate per service year from 2021-01-03 (years floored 0.5, capped 6), votes 0.4, no decay; positive and negative sub-cohort shares as recorded; True Neutral for zero record.

**Code refs:** `fec/stance/rank_honors_policy.py`, `fec/stance/build_rank_honors.py`, `fec/stance/RANK_HONORS.md (pre-rule-G only)`

**Assumptions:**
- Only accepted actions earn labels; silence earns none
- Low rank never implies opposition

**Brain notes:** [concepts/behavior-qualified-recognition](obsidian://open?vault=brain&file=concepts%2Fbehavior-qualified-recognition)

**Evidence (0 rows; none):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment**

**Evidence weight: none. GAP.** No bearing row targets rule G, rate-per-service-year, the 3/2/1 role weights, the 0.4 vote weight or the 5/15/20% cutoffs.

Adjacent verified bearings, recorded under the say-do gate, raise transfer cautions:
- `fouirnaies-2022-electoral-incentives-affect` (VERIFIED, state legislatures): sponsorship volume falls when reelection incentives end.
  - A rate per service year therefore mixes position with electoral status. Retiring members' rates would drop for reasons unrelated to CU alignment.
  - Rule G does not adjust for this.
- `bucchianeri-2025-legislative-effectiveness-american` (VERIFIED-SOFTENED, state legislatures, pooled, UNSTRATIFIED by state): seniority is associated with +0.032 advancement per term, and majority and chair status with much more.
  - Rule G counts actions, not advancement, so this transfers only weakly.
  - It does suggest that action opportunities are unequal across members. The 0.5-year floor and 6-year cap only partly address that.
- `porter-2005-network-analysis-committees`: votes on lopsided roll calls carry little information. Rule G's 0.4 weight on votes, with no contestedness filter, inherits the same leak described under the say-do gate.

The design principles (only accepted actions earn labels; low rank never implies opposition) are normative display rules and need no empirical support. The **fairness/predictive claim** (that a rate is better than cumulative counts for junior members) is empirical and untested.

**The study that would fill it:**
1. Compute rule G and the pre-rule-G cumulative ladder from 117th-118th Congress actions.
2. Predict CU-aligned actions in the 119th.
3. Compare rank correlation and top-k precision, stratified by tenure cohort (freshman vs senior), majority status and chamber.
4. Treat members who retired or lost as a separate stratum, given Fouirnaies & Hall.

**Contradictions**
- No direct contradiction. Fouirnaies & Hall 2022 (state legislatures) implies that per-year action rates move with electoral incentive, a confound rule G does not model.

**Gaps**
- No study bears on rate-per-service-year vs cumulative counts as a predictor of future CU actions (the component's own empirical question). It needs a prospective test across Congresses, stratified by tenure, majority status and chamber.
- No source for the role weights 3/2/1, the vote weight 0.4, the year floor 0.5 or cap 6, or the 5/15/20% cutoffs.
- No evidence on whether removing Congress decay (1/0.8/0.64, used in the base ladder) under rule G helps or hurts prediction.
- fec/stance/RANK_HONORS.md documents only the pre-rule-G ladder, so there is a documentation gap.

**Paul's position vs the evidence**

Paul cites Fouirnaies & Hall 2022 as indirect support for rewarding observable actions only. The verified bearing of that paper is that sponsorship is visible effort that responds to electoral incentive. That supports 'observable' but complicates 'rate': per-year rates fall when incentives fall. In the system the paper is actually used for the receptivity multiplier. No annotation addresses rate-per-service-year, and no evidence row does either.

**Open questions**
- Should rule G flag members in their final term (retiring or defeated) so a falling rate is not read as falling alignment?
- Should votes counted in rule G share whatever contestedness filter is adopted for say-do path V?
- Is a refresh of RANK_HONORS.md for rule G wanted?

**Read first:** [[fouirnaies-2022-electoral-incentives-affect]], [[bucchianeri-2025-legislative-effectiveness-american]], [[porter-2005-network-analysis-committees]]

## Primacy (CU-purity) tier board

**Computes:** Tier 1/2/3 (13/24/48) of members whose legislative acts are most purely CU-focused.

**Method:** factor_raw = sum(role_w x act_w) / sum(role_w) with role weights sponsor 1, original 0.5, cosponsor 0.25. Act weights: cu_primary 1, shared .5, other industry .15, offtopic 0, oppose -1; letters 0.6, amendments x0.5; tensor cell overrides. Shrinkage lambda = min(1, credit/15) toward the agenda-weighted baseline, then an uncertainty penalty K = 0.25. Evidence floor cu_primary >= 0.75. diagnostic_v1 (CLASS_DOCK_PRESS_GRADE_ALL, within-chamber) is default since 2026-09-20. Volume-blind by design.

**Code refs:** `fec/engine/primacy_factors.py:28-490`, `fec/engine/primacy_method.py:1-12`, `fec/tier_util.py primacy_scores()`, `fec/legislation_measure_key.py`

**Assumptions:**
- CU purity, not volume, defines primacy (owner intent)
- Thin portfolios shrink toward the roster mean

**Brain notes:** [topics/2026-07-feature-primacy-and-industry-tier-boards](obsidian://open?vault=brain&file=topics%2F2026-07-feature-primacy-and-industry-tier-boards); [topics/2026-07-feature-primacy-and-industry-tier-boards-primacy-factor-drift-crown](obsidian://open?vault=brain&file=topics%2F2026-07-feature-primacy-and-industry-tier-boards-primacy-factor-drift-crown)

**Evidence (0 rows; none):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment**

**Evidence weight: none. GAP.** No bearing row addresses issue-purity scoring, the role weights (sponsor 1, original 0.5, cosponsor 0.25), the act weights, the shrinkage anchor of 15 credits toward an agenda-weighted baseline, the uncertainty penalty K = 0.25, or the cu_primary >= 0.75 evidence floor.

**Volume-blindness** is owner intent: CU purity, not volume, defines primacy. That is a definitional choice, not an empirical claim, and the evidence should not be read as arguing against it.
- One note for this project: per-edge volume normalization was measured and REJECTED in the SNA work. Primacy's volume-blindness is a different mechanism, a portfolio-share ratio, and nothing here proposes revisiting either decision.

Adjacent caution (verified bearings recorded elsewhere):
- The say-do gate rows show that sponsorship is partly display (Fouirnaies & Hall 2022) and that cosponsor-level acts are cheap.
- A purity ratio over a small portfolio of cheap acts can therefore look "pure" without much behind it. The shrinkage and the evidence floor are the system's guards against this, but their settings are unvalidated.

**Studies that would fill it:**
- **Predictive validity:** do high-purity, low-volume members deliver more enacted CU-aligned provisions than high-volume, low-purity members in a later Congress? Outcomes come from enacted text or LES-style advancement stages; stratify by majority status and chamber.
- **Shrinkage tuning:** compare out-of-sample rank stability of the top 13/24/48 across Congresses (for example, 118th fit vs 119th) for anchors of about 5, 15 and 30 credits, and K values around 0.25.
- **Default setting:** check whether diagnostic_v1 (CLASS_DOCK_PRESS_GRADE_ALL, within-chamber), the default since 2026-09-20, changes tier membership relative to the earlier setting. This is a zero-rank-delta-style diagnostic, not a validity test.

**Contradictions**
- None found. No evidence row bears on this component.

**Gaps**
- No study tests purity vs volume as a predictor of enacted CU-aligned provisions (the component's own empirical question).
- No evidence on the shrinkage anchor of 15 credits or the uncertainty penalty K = 0.25. It needs a rank-stability study across Congresses.
- No source for the act weights (cu_primary 1, shared 0.5, other industry 0.15, oppose -1, letters 0.6, amendments x0.5) or the role weights 1/0.5/0.25.
- The 13/24/48 tier sizes are judgment calls, with no evidence on whether they match natural breaks in the score distribution.

**Paul's position vs the evidence**

Paul cites no literature. Volume-blindness is his stated intent, and the evidence neither supports nor contradicts it. The adjacent say-do evidence suggests the shrinkage and the evidence floor carry most of the weight in keeping thin, cheap portfolios out of Tier 1, and those settings are untested.

**Open questions**
- What share of current Tier 1 members sit near the shrinkage anchor (credit close to 15) and are therefore sensitive to the anchor choice?
- Should cosponsor-only portfolios be barred from Tier 1 regardless of purity, given that cosponsorship is the cheapest act?
- Is there an enacted-provision outcome dataset to support a predictive-validity look, and would that look be preregistered?

**Read first:** [[fouirnaies-2022-electoral-incentives-affect]], [[bucchianeri-2025-legislative-effectiveness-american]], [[porter-2005-network-analysis-committees]]

## Per-area primacy grade (D-031)

**Computes:** grade = sum over areas of IMP x (0.80 n_leg + 0.20 n_press), with within-chamber percentiles.

**Method:** leg = role-weighted signed acts x (1 + 0.5 tensor) + 0.4 x votes, per service year (clipped 0.5-6). Press = (pos - neg)/items when items >= 5. Role weights 0.40/0.25/0.15.

**Code refs:** `fec/engine/per_area_grade.py:1-526`, `fec/engine/primacy_press.py:1-62`

**Assumptions:**
- Within-chamber percentiles avoid cross-chamber comparison (D-027)
- Press proportion applies uniformly to all areas (no per-item area tags)

**Brain notes:** [topics/2026-07-feature-primacy-and-industry-tier-boards](obsidian://open?vault=brain&file=topics%2F2026-07-feature-primacy-and-industry-tier-boards)

**Evidence (0 rows; none):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment**

**Evidence weight: none. GAP.** No bearing row targets the D-031 grade: IMP x (0.80 n_leg + 0.20 n_press), the role weights 0.40/0.25/0.15, the 0.4 vote term per service year, the press proportion (pos - neg)/items with items >= 5, or the within-chamber percentiles.

**Two assumptions are testable and currently untested:**
- **Within-chamber percentiles avoid cross-chamber comparison (D-027).** This is a sound design guard. Evidence on chamber differences recorded elsewhere supports it: Bucchianeri's chair effect is 0.609 in the lower chamber vs 0.307 in the upper, in state legislatures. That transfers only as a reason to stratify, not as a Congress estimate.
- **One press proportion applies uniformly to all areas, because there are no per-item area tags.** This is a real measurement weakness. A member's press valence on one area is spread across every area. No row tests it, and it is the most likely source of error in the grade.

The legislative term inherits the say-do cautions: sponsorship is partly display, and votes have no contestedness filter. The press term inherits whatever validity the stance press classifier has. Press classification is Paul's domain and is not assessed here.

**The study that would fill it:** do per-area grades predict area-specific roll-call alignment on contested votes in a later Congress, better than an area-blind overall score? Run it stratified by chamber and party. Also run an ablation that sets the press weight to 0 to see whether the uniform press term adds or subtracts predictive signal.

**Contradictions**
- None found. No evidence row bears on this component.

**Gaps**
- No study tests whether per-area grade predicts area-specific roll-call alignment (the component's own empirical question).
- The uniform press proportion across areas is untested. Per-item area tags or an ablation with press weight 0 would be needed.
- No source for the 0.80/0.20 legislative/press split, the 0.40/0.25/0.15 role weights, or the 0.4 vote weight.
- No test of the items >= 5 press floor (stability of the press proportion at small item counts).

**Paul's position vs the evidence**

Paul cites no literature. The within-chamber percentile rule (D-027) is consistent with the general lesson that chamber matters (Bucchianeri's chamber split for chairs, state legislatures). The evidence offers nothing on the uniform-press assumption either way.

**Open questions**
- Is per-item area tagging of press items feasible, so the press term can become area-specific?
- Would Paul accept a press-weight-0 ablation as a display diagnostic under zero-rank-delta?
- Does the per-area grade share the vote-contestedness issue raised for say-do path V?

**Read first:** [[bucchianeri-2025-legislative-effectiveness-american]], [[porter-2005-network-analysis-committees]], [[fouirnaies-2022-electoral-incentives-affect]]

## Per-industry tier boards, industry-axes tensor and crown law

**Computes:** Separate CU, bank, crypto and fintech boards, and an industry tensor tie-break.

**Method:** Same desk method per industry. The tensor cell (relevance x valence) is a secondary tie-break, never blended. Crown law: 12-13 Tier-1 champions, and a champion cannot be a threat in another sector.

**Code refs:** `fec/lobbying/industry_tiers.py`, `fec/engine/primacy_factors.py:111-194`, `tier_overrides.json`

**Assumptions:**
- Industries can be scored independently by the same method
- Missing tensor cells are not zero

**Brain notes:** [topics/2026-07-feature-primacy-and-industry-tier-boards](obsidian://open?vault=brain&file=topics%2F2026-07-feature-primacy-and-industry-tier-boards)

**Evidence (2 rows; VERIFIED-SOFTENED 2):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2025-measuring-interest-group]] | 2025 | supports | Oppose is the GNN's hardest class. The text says Oppose F1 is 'in the 60s', but SI Table 12 shows Oppose F1 from 57.65 to 64.06 across the 17 graph configurations, with five configurations below 60. Engage dominates GNN output: 137,023 of 188,639 Engage-class labels (72.6%) come from the GNN stage. Fig 5 orders industries by mean LPscore, with commercial sectors at one end and social/progressive sectors at the other. | The paper's Oppose weakness belongs to its GNN stage. The system's weak Oppose agreement belongs to its Haiku lane, scored against Kim labels. They point the same way, but they are different readers. The bank-column pilot result (about 70% of Oppose labels wrong, zero rows signed) was not found in the cited code or in a repo search, so it is dropped here as UNVERIFIED. | moderate | VERIFIED-SOFTENED |
| [[kim-2020-mapping-political-communities]] |  | qualifies | 'Finance & Insurance' is one of the eight named legislation communities. The paper states the data 'do not distinguish between lobbying for or against' a bill, and therefore analyses domain broadening rather than alliances. (That co-lobbying would place rival finance interests in one community is an inference, not a paper finding.) | The biLCM cannot supply rival-industry champions because it carries no sign. Separating banks from CUs needs a signed position source (e.g. Kim J. et al. 2025). The paper neither supports nor refutes the crown law. | moderate | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: moderate on inputs, none on the crown law itself.** There are 2 rows, both VERIFIED-SOFTENED and moderate.

**`kim-2025-measuring-interest-group` (supports, with caveats)**
- Oppose is the hardest class in the paper's GNN stage. The text says Oppose F1 is "in the 60s", but SI Table 12 shows 57.65 to 64.06 across 17 graph configurations, with five below 60.
- The GNN stage produces 137,023 of the 188,639 Engage-class labels (72.6%).
- Fig. 5 orders industries by mean LPscore, with commercial sectors at one end and social/progressive sectors at the other.
- The system already tags Kim-format positions as `inferred_<variant>` and treats a Support badge as a lead, not a fact. In industry_tiers the adjudicated CU cell owns the CU tensor weight, and the older raw position overlay (acu_position) has been removed from it.
- The paper's Oppose weakness (GNN stage) and the system's weak Oppose agreement (Haiku lane, scored against Kim labels) point the same way. They are different readers, so they agree in direction but do not replicate each other.
- A bank-column pilot claim (about 70% of Oppose labels wrong) could not be found and is dropped as UNVERIFIED.

**`kim-2020-mapping-political-communities` (qualifies)**
- The biLCM puts "Finance & Insurance" in one of eight legislation communities.
- The paper states that the data do not distinguish lobbying for a bill from lobbying against it.
- It therefore **cannot** supply rival-industry champions or separate banks from CUs. Doing that needs a signed position source.
- It neither supports nor refutes the crown law. That co-lobbying would put rival finance interests in one community is an inference, not a finding of the paper.

**Rule check (D-021):** LDA is an identity source, not an edge or weight source. Kim 2025 positions are derived from LDA report text. If an `inferred_<variant>` valence feeds the tensor tie-break cell for the bank, crypto, fintech or telecom columns, that would be LDA-derived content used as a weight. The rows say only that the CU cell is adjudicated, so the other columns' valence source should be confirmed.

**The crown law** (a Tier-1 champion cannot be a threat in another sector) is an empirical bet: no member is genuinely aligned with both banks and CUs on contested bills. No evidence tests it.
- **Study that would fill it:** take contested bank-vs-CU bills (interchange and similar) with a signed position source on both sides. Count members whose recorded actions align with both industries. If such members exist, the crown law suppresses a real category rather than catching an error.

**Contradictions**
- Paul cites Kim & Kunisky for this component, but its verified bearing is that the biLCM carries no sign. It cannot identify rival-industry champions or commitment level, which is the thing Paul wants from it.
- Possible D-021 tension (unconfirmed): Kim 2025 positions are LDA-derived. If they supply tensor valence for non-CU industry columns, LDA is being used as a weight source.

**Gaps**
- No study tests the crown law, that is, whether cross-industry champions (aligned with both banks and CUs on contested bills) exist empirically.
- No evidence that one desk method scores industries independently and comparably (assumption 1). Per-industry calibration is unmeasured.
- Commitment level, which Paul explicitly wants, has no measurement source with verified validity; Oppose is the weakest class in both the paper and the system lane.
- The 12-13 Tier-1 champion count is a judgment call with no evidence.

**Paul's position vs the evidence**

Paul wants champions identified for rival and allied industries, with their level of commitment. His margin note says he may need to account for conflicting industries. The crown law and direction-aware, mutually exclusive boards implement that intent. But the evidence he cites does not deliver it:
- Kim & Kunisky's communities are unsigned.
- Kim J. et al. 2025 supplies signed positions, but Oppose is its weakest class (F1 57.65-64.06 across 17 configurations).
His note about conflicting industries is the right worry. Whether the crown law answers it correctly is untested.

**Open questions**
- What is the valence source for the bank, crypto, fintech and telecom tensor columns? If it is Kim-format LDA-derived positions, does that conflict with D-021?
- Would Paul accept an empirical count of cross-industry aligned members on contested bills as a check on the crown law before it is treated as settled?
- How is 'level of commitment' meant to be measured, given that Oppose labels are the least reliable class?

**Read first:** [[kim-2025-measuring-interest-group]], [[kim-2020-mapping-political-communities]], [[porter-2005-network-analysis-committees]]

## Across components

**1. One leak, several components.** Several components count votes or sponsorships without a contestedness filter or a party/majority baseline: say-do path V, rule G's 0.4 vote weight, the per-area grade's vote term, and the primacy role weights.
- The evidence on vote informativeness is consistent:
  - Porter 2005 (107th House, 990 roll calls): 74.2% are predictable by always guessing "pass".
  - Poole 1999: party overlap under 1.5%, data ending in 1998.
  - Fouirnaies & Hall 2022 (state legislatures): sponsorship is partly display.
- A single contestedness rule, defined once in `build_vote_category.py` where all callers read it, would address all of these components at once. The share of admitted CU roll calls that are lopsided is unmeasured, so the size of the leak is an UNVERIFIED lead (DE-062).

**2. Party sorting.**
- A same-function re-run splits path V corroboration by party in 6 categories, for example CFPB D 4/259 (2%) vs R 179/271 (66%). Window: current vote_category_lean.json; denominators from the moneyball_core.party() roster.
- It does not split in 3 categories (Mortgage & Housing, Fraud Prevention, Defense CU Issues).
- Any pooled label-quality figure over the party-sorted categories should be quoted stratified by party or marked UNSTRATIFIED.
- The split is not yet independently reproduced.

**3. Citation drift.** Three cited works do not do the job they are cited for here:
- Fouirnaies & Hall 2022 is cited by Paul for say-do and rank honors, but the code uses it for the receptivity incentive multiplier.
- Kim & Kunisky is cited for industry champions but carries no sign.
- Kim, Stuckatz & Wolters is cited for say-do but has no bearing row, and its in-system use (leadership PAC edges) is unimplemented.

**4. Transfer.**
- Not one evidence row is a Congress 117-119 result on CU-relevant legislation.
- The bearings come from state legislatures (Fouirnaies, Bucchianeri), 1947-1995 Senate Democrats (Thakur), the 107th House (Porter), data ending in 1998 (Poole), voters (Broockman) and LDA text classification (Kim 2025).

**5. Documentation and design drift.**
- The ladder has three conflicting descriptions: the specification's median+IQR, the code's fixed bands, and curved CDFs in four domains.
- RANK_HONORS.md predates rule G.
- The overall tier feeds money_response.py, yet labels are said never to feed the rank pillars. That boundary should be stated explicitly.

**6. Rules respected.**
- None of the papers argues for dropping regular cosponsorship edges or for per-edge volume normalization; both were rejected in this project. Primacy's volume-blindness is owner intent and a different mechanism.
- No label or tier output is described here as closeness, access or introduction probability (D-025).
- One possible D-021 issue (LDA-derived valence in non-CU tensor columns) is raised as a question, not asserted.

## Research agenda

1. Build a dated member x CU roll-call matrix (117th-119th Congress, contested votes only, chamber and party recorded). Several items below depend on it: ladder calibration, say-do path V testing, per-area grade validation and rule G prediction.
2. Measure the share of roll calls admitted by build_vote_category.py with a minority share below a small threshold (for example 10%), and whether the source is final-passage only. This sizes the path V leak, which is currently UNVERIFIED (DE-062).
3. Independently reproduce the party split of path V corroboration with a second method (for example, recompute from raw vote records rather than vote_category_lean.json), so it can leave UNVERIFIED status. Quote it stratified by party only.
4. Ladder calibration: fit a per-category probit/IRT (Poole A2-A3 form) and map each FIXED_BANDS edge, and each curved-CDF edge, to a predicted P(CU-aligned vote) on a held-out Congress. Compare calibration, stratified by party. Resolve the MATHEMATICAL_SPECIFICATION.md line 14 conflict first.
5. Say-do validity: prospectively compare gate-failing vs corroborated members matched on rhetoric score, using CU-aligned behaviour in the next Congress, stratified by party and chamber.
6. Thin-high-volume rule: rebuild the advancement-rate threshold against a within-chamber, majority-adjusted (and chair-adjusted) Congress baseline, for example from Volden-Wiseman LES stage data, and check how many Champion caps change.
7. Rule G: test rate-per-service-year against cumulative counts on 117th-118th actions predicting 119th CU actions, stratified by tenure cohort, majority status and chamber, with retiring or defeated members as a separate stratum.
8. Primacy: test purity vs volume as a predictor of enacted CU-aligned provisions, and test the rank stability of tiers 13/24/48 across Congresses for shrinkage anchors of about 5, 15 and 30 and K around 0.25.
9. Per-area grade: test prediction of area-specific contested-vote alignment, with a press-weight-0 ablation to test the uniform-press assumption, plus a feasibility check on per-item area tagging.
10. Overall tier: count members with opposite-signed top cells, and test the IMP-weighted mean vs an unweighted mean vs a dispersion-aware summary on predicting highest-IMP (Interchange) votes.
11. Crown law: on contested bank-vs-CU bills with a signed position source on both sides, count members aligned with both industries. Confirm the valence source for the non-CU tensor columns against D-021.
12. Literature search: Congress-specific studies of costly signalling via sponsorship vs floor votes, issue specialization and portfolio purity, and legislative-effectiveness baselines by majority status. None currently has a VERIFIED bearing on this system.
