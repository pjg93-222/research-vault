---
type: theme
name: "Legislative Behavior & Effectiveness"
paper_count: 2
tags: [theme]
---

# Legislative Behavior & Effectiveness

This theme holds **two works**, and both study U.S. **state** legislatures. Neither is a congressional study.

- [[fouirnaies-2022-electoral-incentives-affect]] asks whether reelection incentives change what legislators *do*. Term limits supply the variation.
- [[bucchianeri-2025-legislative-effectiveness-american]] asks whether lawmaking *effectiveness* can be measured comparably across 97 chambers. It builds the SLES to do that.

Together they make one argument that matters for Paul's systems: **observable legislative activity is driven heavily by situation and structure.** Situation means electoral status. Structure means majority status, chair positions and chamber type. Activity volume is also not the same thing as effectiveness, and neither one is the same as a legislator's *position*. Fouirnaies and Hall find that effort falls in a binding final term while ideology does not move. Bucchianeri, Volden and Wiseman find that effectiveness is driven most by majority status and chair positions, and that raw hit rates are a worse measure than a stage-and-significance-weighted score.

**Mismatch with Paul's category notes.** Paul's annotation file for this category (sha12 4b53f8cc145e) discusses six *different* works. They cover lobbying returns (Alexander; Bickmore; Huneeus and Kim), donations and access (Kim, In Song et al.), staffer information filtering (the entry cited as Kalla and Broockman but annotated with Furnas, LaPira et al.) and position-mapping pipelines (Kim, Jiseon et al.). **None of those six works is in this theme's file.** So his four arguments cannot be tested here:

1. Lobbying pays, partly at economy-wide cost.
2. Donations buy committee-centred involvement that fades.
3. Staffers' ideology filters information.
4. Position pipelines are reactive and blind to conflict.

Below, his notes are used only to frame the gaps. The two works here bear on a narrower question: *how legislators' activity should and should not be read* by the rank, receptivity and disposition layers.

## Strands

### Electoral incentives move effort, not position

[[fouirnaies-2022-electoral-incentives-affect]] uses binding term limits in 14 states (16 chambers, about 1991-2016). It compares each legislator's final term with their own earlier terms, using legislator fixed effects and chamber-year fixed effects.

- **Productivity index:** falls -0.24 SD (SE 0.03), N=11,109 legislator-terms and 4,642 legislators. This is pooled across all 16 chambers and is UNSTRATIFIED in the main text. Appendix A.3 reports larger effects under lifetime limits, in professionalized chambers and where pay is higher, but gives no numbers in the extracted text.
- **Range across counterfactuals:** -0.09 with seniority FE or cohort-year FE, up to -0.41 with matched NY/TX controls (N=11,416, 1,506 legislators).
- **Components:** floor-vote participation falls 2.70 points from a mean of 90.48. Sponsored bills fall 1.63 from a mean of 27.32 (N=16,727). The committee index falls 0.31 from a mean of 3.84 (N=16,255).
- **Positions:** |W-NOMINATE| moves +0.02. The party-split estimates are about 0. Interest-group ratings across seven issue areas show no systematic shift.

The authors' reading is that elections shape *visible effort*, not *positions*. Legislators who leave to run for another office keep their electoral incentive, so the authors treat the estimates as a lower bound (footnote 14).

It follows that roll-call participation and sponsorship volume are partly a function of the legislator's situation, not purely a stable trait or costly signal. The paper measures **effort**. It does not measure **responsiveness to pressure**, and its null on positions argues against reading the effort drop as movability.

Works: [[fouirnaies-2022-electoral-incentives-affect]]

### Measuring lawmaking effectiveness

[[bucchianeri-2025-legislative-effectiveness-american]] adapts the congressional LES to the states.

**Scale and construction.** The SLES covers 80,344 legislator-term scores in 97 chambers, 1987-2018, built from about 1.8 million bills. Each score weights 15 indicators: the share of a member's sponsored bills reaching each of 5 stages, at 3 significance levels weighted 1, 5 and 10. Scores are normalized to mean 1 within each chamber-term. The authors explicitly warn against comparing absolute scores across chambers or eras.

**Validity checks.**
- **Coding audit:** 46,693 of 49,037 sampled bills (95.2%) were coded correctly, rising above 99% after fixes.
- **Criterion validity:** in North Carolina, 2005-2012, the SLES alone explains about half the variance in the NCCPPR subjective effectiveness rankings and beats hit-rate metrics. This is a single-state criterion.
- **Persistence:** lagged correlation rho is 0.38 when the legislator entered the majority, 0.40 when entered the minority, and 0.66 when control did not change.

**Structural drivers** (pooled OLS with term and chamber FE, legislators clustered; each figure is on the 80,344 legislator-term panel):
- **Majority party:** +0.370 pooled, or 58% above the minority mean of 0.636. By chamber: 0.362 lower, 0.387 upper.
- **Committee chair:** +0.507 pooled. By chamber: 0.609 lower vs 0.307 upper. The pooled figure hides a two-to-one chamber split.
- **Seniority:** +0.032 per term.
- **Distance from the chamber median:** -0.114.

Nothing in the paper is causally identified. The authors flag endogeneity themselves: strong parties may choose the rules.

Works: [[bucchianeri-2025-legislative-effectiveness-american]]

### Situational versus dispositional reading of legislative activity

Read together, the two works pull legislative activity apart into three things that should not be merged.

1. **Effort volume** (participation, sponsorship counts) moves inside the same person with electoral status. Source: [[fouirnaies-2022-electoral-incentives-affect]].
2. **Effectiveness** (advancement of one's own bills, weighted by stage and significance) is driven most by structural position: majority status and chairs. Source: [[bucchianeri-2025-legislative-effectiveness-american]].
3. **Position** (ideology and ratings) does not move with incentive in the first paper. In the second it predicts effectiveness only cross-sectionally (moderates score higher).

The SLES persistence figure (rho 0.38-0.66) is the only evidence here of a stable individual component. Even that persistence is visibly modulated by majority change, a structural shift.

The shared lesson for any disposition or trait reading of legislative behaviour: hold situation and structure fixed before attributing activity to the person. Neither paper does this for Congress.

Works: [[fouirnaies-2022-electoral-incentives-affect]], [[bucchianeri-2025-legislative-effectiveness-american]]

## Agreements and debates

**Where they agree**
- Bill-sponsorship records are a usable behavioural signal. Both works build on state sponsorship data.
- Institutional position shapes measured activity. In [[fouirnaies-2022-electoral-incentives-affect]], committee activity is part of the index, and chairs weigh 3 versus 1 for an ordinary member. In [[bucchianeri-2025-legislative-effectiveness-american]], a chair adds +0.609 in lower chambers and +0.307 in upper chambers.
- Neither paper treats ideology as a by-product of activity. In F&H, ideology is flat while effort drops. In BVW, ideology enters only as distance from the chamber median.

**Where they pull apart**
- **Volume versus effectiveness.** F&H's productivity index rests partly on *log bills sponsored*, which is volume. BVW's case for the SLES is that it beats hit-rate measures on the NC criterion (2005-2012), and it deliberately weights stage and significance. Under BVW's framing, a fall in F&H's index is a fall in effort, not shown to be a fall in effectiveness. Neither paper tests whether final-term effort losses reduce SLES. The two share the state-legislature setting but do not cite each other.
- **Identification.** F&H use a quasi-experimental design with exogenous binding limits, but they cannot run pretrend tests (p.669). The TWFE critique in [[imai-2021-fixed-effects-regression]] applies. BVW make no causal claim and say that endogeneity is unaddressed. Neither paper's coefficients should be quoted as effects for Congress.
- **Stability of individual differences.** BVW's term-to-term persistence (rho up to 0.66) leaves room for a stable individual component. F&H show that the inputs to that kind of component (participation, sponsorship) shift within-person by 6-27% with electoral status. These findings are not contradictory, but they limit how much of an activity score can be read as trait.

**Debates with Paul's category notes:** none can be adjudicated here. The lobbying-return, donations-access and staffer-filtering works his notes discuss are not in this theme.

## Methods

**Dominant methods**
1. **Two-way fixed-effects panel regression.** This means legislator FE plus chamber-year FE in [[fouirnaies-2022-electoral-incentives-affect]], and term plus chamber or state FE in [[bucchianeri-2025-legislative-effectiveness-american]]. Standard errors are clustered by legislator in both, and by state chamber in BVW's chamber-level models.
2. **Composite indices built from bill records.** F&H use a PCA first component of log sponsorship, a committee index and participation. BVW use a 15-indicator weighted stage-by-significance score, normalized within chamber-term.
3. **Roll-call scaling for position.** This is W-NOMINATE, from the NOMINATE family reviewed in [[poole-1999-nominate-after-years]], plus interest-group ratings.

**Weaknesses**
- **TWFE with staggered treatment.** F&H admit that pretrend tests are not possible. The estimate shrinks from -0.24 to -0.09 SD once seniority is controlled, so learning and seniority confounds are material. [[imai-2021-fixed-effects-regression]] is the relevant critique.
- **Pooling over known fault lines.** BVW's chair coefficient (+0.507 pooled) hides lower vs upper chamber values of 0.609 vs 0.307. F&H's headline -0.24 SD is pooled over 16 chambers whose heterogeneity is reported only qualitatively in the main text. Treat both headlines as UNSTRATIFIED unless quoted with strata.
- **Within-unit normalization.** SLES scores are comparable only inside a chamber-term. Any cross-chamber pooling of an SLES-like score breaks the paper's own validity condition.
- **Thin criterion validation.** BVW's external criterion is a single state over a single window (NC, 2005-2012, about half the variance explained).
- **Coarse position measures.** F&H note that W-NOMINATE may be too coarse to detect issue-specific shifts (pp.668, 673). Their position null is therefore a null at that resolution.
- **External validity.** Both use state data. Congressional retirement is voluntary and self-selected, unlike a binding term limit, so F&H's effect transfers only weakly.
- **No causal claims in BVW.** Rules and power may be jointly determined.

## How the works relate

- [[bucchianeri-2025-legislative-effectiveness-american]] — complements — [[fouirnaies-2022-electoral-incentives-affect]]: both use U.S. state bill-sponsorship records. F&H measure effort, which is the input side. SLES measures effectiveness, which is the outcome side, and could test whether final-term effort losses reduce effectiveness. Neither cites the other, so the same-data label is downgraded to complements.
- [[bucchianeri-2025-legislative-effectiveness-american]] — measurement tension (analyst inference) — [[fouirnaies-2022-electoral-incentives-affect]]: F&H's productivity index includes log bills sponsored, a volume measure. BVW argue that volume and hit-rate style measures underperform a stage-and-significance-weighted score on the NC criterion. A drop in F&H's index is therefore not shown to be a drop in effectiveness. This is an inference, not a citation.
- [[fouirnaies-2022-electoral-incentives-affect]] — complements (methodological critique applies) — [[imai-2021-fixed-effects-regression]]: F&H rely on individual-level TWFE DiD with a staggered final-term treatment and no pretrend tests (p.669). Imai and Kim's critique of TWFE causal assumptions governs how far the -0.24 SD estimate should transfer.
- [[fouirnaies-2022-electoral-incentives-affect]] — complements (situational confound) — [[ramey-2016-measuring-elite-personality]]: Ramey links text-measured Conscientiousness to sponsorship behaviour. F&H show that sponsorship and participation move with electoral incentive inside the same person, which confounds any trait reading of those behaviours.
- [[fouirnaies-2022-electoral-incentives-affect]] — complements (sibling inputs) — [[linde-2016-politicians-take-risks]]: both feed the receptivity layer's electoral-situation factors: F&H drive incent_mult 0.60 and Linde and Vis drive pt_mult. According to the file's code note, both draw on the same security input.
- [[fouirnaies-2022-electoral-incentives-affect]] — builds on (measurement) — [[poole-1999-nominate-after-years]]: F&H's ideology null rests on W-NOMINATE scaling of state roll calls, from the NOMINATE family Poole reviews. The authors note the scaling may be too coarse for issue-specific shifts.
- [[bucchianeri-2025-legislative-effectiveness-american]] — complements (analyst inference) — [[sotoudeh-2024-network-based-measure]]: both are measures of which legislators get bills advanced. BVW do not cite Sotoudeh, so this link is inferred, not a citation chain.

## What it means for the systems

All bearings in the file are VERIFIED or VERIFIED-SOFTENED. None is UNVERIFIED.

**From [[fouirnaies-2022-electoral-incentives-affect]]**
- **Receptivity layer (qualifies, VERIFIED-SOFTENED)** — see [[systems/melange-influence-engine]]. The 0.60 "less responsive" multiplier on retiring members cites a paper that measured *effort*, not responsiveness to pressure. The paper's null on positions argues against reading reduced effort as reduced movability. Its footnote 14 contradicts the retiring list, which includes members running for other office; those members keep their electoral incentive. The congressional transfer is also weak, because retirement is self-selected. The multiplier's citation should be re-scoped, or the multiplier treated as unsupported by this source.
- **Behavioral dispositions and gold-panel validation (qualifies, VERIFIED)** — see [[systems/melange-psychometrics]] and [[systems/melange-validation]]. Diligence and the C criterion read participation and sponsorship volume as dispositions. The paper shows these move within-person with electoral status: abstention rises about 27% proportionally, and sponsorship falls about 6%. Any such score needs electoral-status controls before it is read as trait.
- **Say-do gate, path A (qualifies, VERIFIED)** — see [[systems/melange-stance]]. Agenda volume used as a costly signal has the same situational confound.
- **Prospective contract, H2 (qualifies, VERIFIED)** — see [[systems/melange-validation]]. Under this paper, retiring members' reduced activity can produce a correlation with no responsiveness behind it. H2's citation does not support a responsiveness reading.
- **Legislative pillar (qualifies, VERIFIED-SOFTENED)** — see [[systems/moneyball-rank-pillars]]. Volume-based legislative activity is partly situational. The finding that effort and position separate *supports* keeping influence and alignment on separate axes.

**From [[bucchianeri-2025-legislative-effectiveness-american]]**
- **Effectiveness shadow (method source, VERIFIED-SOFTENED)** — see [[systems/melange-legislative-data]]. `fec/engine/legislative_effectiveness.py` (unactivated) copies the 5 stages, the 1/5/10 weights and per-chamber-term normalization. It departs from the SLES in two ways: it credits cosponsors, and it measures significance by CU mention frequency. **The paper's validation evidence does not carry over to either departure.** The paper's sponsor-only crediting must *not* be read as an argument to drop regular cosponsorship edges from the graph. That proposal was measured and rejected in this project.
- **Legislative pillar and score blend (qualifies)** — see [[systems/moneyball-rank-pillars]]. Leg% and Influence% are pooled across the House and Senate. By the paper's own validity condition, scores of this kind are comparable only within chamber-term, so the live pooled figures are UNSTRATIFIED over a known fault line. The live efficiency term is a hit-rate-type measure, the kind the SLES outperforms on the NC criterion (2005-2012).
- **Committee pillar (qualifies, VERIFIED-SOFTENED)** — see [[systems/moneyball-rank-pillars]]. Chair advantages differ about two-to-one by chamber (0.609 lower vs 0.307 upper, state panel). Pooling chambers will mis-weight chairs.
- **Missing control.** No layer controls for majority status. In the paper, majority status is the largest structural driver of advancement (+0.370 pooled; 0.362 lower and 0.387 upper; 80,344 state legislator-terms). The brain records that CEL LES is roughly orthogonal to the live score (+0.10 Spearman); its window and denominator are not stated in this file.

**Out of scope for this theme.** Neither work bears on LDA usage (D-021), reciprocity (not validated, D-104..D-108) or SNA edge weighting. Paul's notes propose two things: think-tank funding treated as venture capital in allocation models, and position data feeding the SNA graph. Neither proposal has supporting works in this theme.

## Gaps

1. **The category notes and the works do not match.** Paul's annotation file covers six works that are not in this theme: Alexander, Bickmore, Huneeus and Kim, Kim In Song et al., Kalla and Broockman / Furnas and LaPira, and Kim Jiseon et al. Either they belong in this theme and are missing from the state file, or the notes were attached to the wrong category. Paul should confirm.
2. **Open items carried over from his notes.**
   - The Kalla and Broockman citation is annotated with a summary of the Furnas, LaPira et al. paper. Paul needs to confirm which paper he meant.
   - The claim that donation effects fade after about 6 months is not in the quoted text and needs checking against Kim, In Song et al.
   - The annotation JSON `_annots/4b53f8cc145e.json` does not exist.
3. **No congressional effectiveness work.** CEL LES is referenced only through the brain's +0.10 Spearman note. There is no congressional counterpart to the SLES validation, and no window or denominator is stated for that correlation.
4. **No work measures responsiveness to pressure.** This is what the receptivity multiplier actually needs. Both works measure effort or effectiveness, not movability.
5. **No validation source for the shadow's departures.** Nothing here validates crediting cosponsors or measuring significance by CU mention frequency.
6. **No test linking effort to effectiveness.** No work checks whether electoral-status effort drops reduce effectiveness. The two papers could be combined on state data, but neither does it.
7. **Missing strata.**
   - F&H's heterogeneity table (A.3) has no numbers in the extracted text, so the pooled -0.24 SD cannot yet be quoted stratified.
   - BVW's single-state criterion validation (NC) has no replication.
8. **Data hygiene.** The Bucchianeri `method` field contains a stray tool-markup fragment (`</method>` / `<parameter name="identification">`) that splices the identification text into the method text. The extraction should be re-run.
9. **Gaps Paul stated himself.** No predictive capability. No detection of inter-industry conflict over a bill. No explicit mapping from findings to named Melange, Moneyball, Mentat or Terrier components; any such link in his notes is inference. Nothing in this theme addresses these.

## All works

- [[fouirnaies-2022-electoral-incentives-affect]] (2022) — How Do Electoral Incentives Affect Legislator Behavior? Evidence from U.S. State Legislatures
- [[bucchianeri-2025-legislative-effectiveness-american]] (2025) — Legislative Effectiveness in the American States
