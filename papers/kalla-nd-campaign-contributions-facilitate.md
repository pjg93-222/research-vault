---
type: paper
title: "Campaign Contributions Facilitate Access to Congressional Officials: A Randomized Field Experiment"
authors:
  - "Joshua L. Kalla"
  - "David E. Broockman"
year: "2016"
venue: "American Journal of Political Science, Vol. 60, No. 3 (July 2016), pp. 545-558 (copyright 2015, Midwest Political Science Association). Metadata hint corrected: full author names and venue added."
doi: "10.1111/ajps.12180"
category: "Campaign Politics"
systems:
  - "melange-influence-engine"
  - "melange-pathfinding"
  - "melange-validation"
  - "terrier-row-scoring"
components:
  - "melange-influence-engine.flip-model"
  - "melange-validation.prospective-contract"
  - "melange-pathfinding.staffer-closeness-routes"
  - "terrier-row-scoring.sphere-of-influence"
  - "melange-influence-engine.competitiveness-multiplier"
  - "melange-influence-engine.money-response-defection"
paul_annotated: true
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/kalla-nd-campaign-contributions-facilitate.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\Hertel-Fernandez-2.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\Hertel-Fernandez-2.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\Hertel-Fernandez-2.pdf"
  - "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Trey - Articles\\Hertel-Fernandez-2.pdf"
  - "C:\\Users\\pgoff\\Downloads\\Hertel-Fernandez-2.pdf"
sha12:
  - "35da1353facd"
tags: [paper, campaign-politics]
---

# Campaign Contributions Facilitate Access to Congressional Officials: A Randomized Field Experiment

**Joshua L. Kalla, David E. Broockman (2016).** *American Journal of Political Science, Vol. 60, No. 3 (July 2016), pp. 545-558 (copyright 2015, Midwest Political Science Association). Metadata hint corrected: full author names and venue added..* [10.1111/ajps.12180](https://doi.org/10.1111/ajps.12180) Theme: [[themes/campaign-politics|Campaign Politics]].

> [!abstract] Summary
> Kalla and Broockman ran a blocked randomized field experiment in 2013. CREDO Action asked 191 House offices for a district meeting about a chemical-ban bill. All offices belonged to one party and none had cosponsored the bill. In one office per covariate-matched triplet (64 offices, a number derived from the design), the request called the attendees "local campaign donors". The other 127 offices were told "local constituents", and nothing else changed. The outcome was the rank of the most senior official who attended. Over June to August 2013, a chief of staff or more senior official attended in 12.5% of donor-label offices and 2.4% of constituent-label offices (p=.006). For legislative director or above the figures were 18.8% and 5.5% (p=.005), and for the Member personally 7.8% and 2.4% (p=.07). Lower-ranked staff showed no reliable difference (48.4% vs 43.3% at any district staffer or above, p=.26). The ordered probit gave p=.05.
> 
> The randomized treatment was the donor label, not real money. The paper cannot say why the label works, and it measures access, not influence. Only nine legislators cosponsored during the study, with the same share in both arms, so the cosponsorship comparison tells us little.
> 
> For Paul's systems, the paper mainly bounds an existing citation. flip_model.py:21 cites Kalla and Broockman for "money buys access/effort > votes" and labels the 0.35/0.65 money/peer split CALIBRATED. The paper supports the access half of that line. It says nothing about votes, positions, cosponsorship or the money-to-peer ratio, so it cannot calibrate those constants.
> 
> The paper's best transferable idea is its outcome measure: the seniority of the official who takes the meeting. The prospective contract and the staffer routes have no outcome of that kind. The contract scores new cosponsorships, which is the same low-base-rate outcome whose estimate the authors call too uncertain to bear on access effects. Nothing here supports the competitiveness multiplier.

## Paul's annotation

From [[annotations/Trey - Articles - Report - Annotated Bibliography|Trey - Articles - Report - Annotated Bibliography]]:

> Kalla, Joshua L., and David E. Broockman. “Campaign contributions facilitate access to Congressional officials: A randomized field experiment.” *American Journal of Political Science*, vol. 60, no. 3, 2 Apr. 2015, pp. 545–558, [https://doi.org/10.1111/ajps.12180](https://doi.org/10.1111/ajps.12180).
>
> This article by Professors Furnas, LaPira, and others explores how the identity of a political information giver affects the recipient's receptiveness to that information in the context of legislative advocacy. By using a novel survey of 400 federal congressional staffers, where the identity of the individual or interest, the nature of the request, and the source of the statistics for said recommendation are provided, the authors reveal an “*epistemic closure among partisan staffers*.” They make clear their recommendation that an interest group should base its findings on evidence from a think tank that is ideologically aligned with the office it is engaging with. Put simply, the staffers want to hear only what they want to hear, will be psychologically averse to institutions they perceive as against their interests, and will be hesitant towards groups they perceive as pursuing their own goals. The implications of these findings are profound and can be seen as an explanation for the donation behavior of large dark-money funds. By channeling funds toward the building of ideological infrastructure, such groups are not recruiting their own champions, but cultivating them. Finally, regarding the current research, allocating venture capital funds to the growth of industry-aligned think tanks should be considered, scouted, and ultimately incorporated into future allocation models.

## Study

**Question:** Can an interest group obtain superior access to senior congressional officials because its members have donated to political campaigns? Put as a counterfactual: would House offices have treated the same group members differently had the offices not known they were donors? (p.545, p.548)

**Theory:** Campaign contributions may buy access through quid pro quo exchange, expected future donations, forestalling donations to rivals, and signals of ideology, expertise or preference intensity; access is a precondition for influence and legislators' time is finite. Observational work is confounded by shared ideology, and contributors may give to legislators who already support them or to those whose support is in doubt. (pp.546-548)

**Data:** Unit: congressional office (district scheduler contacted by e-mail). Sample: every U.S. House representative of one party who had not already cosponsored the bill, 191 offices after one of an original 192 was dropped because of a misdirected e-mail (fn 5, p.549). Recruited attendees: around a dozen CREDO Action members per district who had previously donated to campaigns (p.549). Organization: CREDO Action, a liberal political organization with about 3.5 million members (p.549). Period: implementation June, July and August 2013 (p.551). Bill: a measure to ban a chemical; no vote was held (p.553, fn 11). Per-arm counts are not reported in the text; the design implies 64 Revealed Donor offices (one per block) and 127 Constituent offices, which matches the Table 1 percentages (derived, not stated). Party is not named in the text.

**Method:** Blocked randomized field experiment. Offices were blocked into 64 triplets using blockTools (Mahalanobis distance) on six covariates; one office per block randomly assigned to Revealed Donor and two to Constituent (pp.549-550). The request e-mail described attendees as "local campaign donors" (Revealed Donor) or "local constituents" (Constituent); nothing else varied, and correspondence was standardized by a prespecified protocol (pp.550-551). Outcome: the rank of the official who would attend, recorded more than 24 hours in advance and confirmed after the meeting, coded into six ordered categories by two coders blind to assignment (p.552). Analysis: exact p-values from randomization inference under the sharp null, one comparison per cumulative rank; Bonferroni check across five comparisons; ordered probit with randomization-inference p-value as an omnibus test (pp.552-554).

**Identification:** Identification rests on random assignment of offices to the donor-label versus constituent-label condition, within covariate blocks, so covariates (balance omnibus p=.92, chi-square 1.44, 5 df, p.549) and chance differences in official availability are balanced by design. The randomized treatment is the donor status of attendees as revealed to the office, not actual contributions; this is a "mechanism experiment" that approximates randomizing donor status (fn 4, p.548). Donors' actual giving was not randomized and no deception occurred: the meetings were real, attendees knew donations might be disclosed, and the requests asked for nothing illegal (p.551). The design does not establish that the label operates through quid pro quo rather than the other mechanisms listed under theory.

**Scope:** Population: U.S. House offices of one party that had not cosponsored one bill, one advocacy group, one set of meetings. Institution: House district offices and their schedulers, not Senate or committee-level access. Era: 2013, after Citizens United (2010); the paper frames findings against Citizens United and McCutcheon v. FEC (p.556). Generalization to corporate or wealthy donors and to other contexts is explicitly untested (p.555).

## Key findings

- Senior access rose when the donor label was revealed: chief of staff or above was available in 12.5% of Revealed Donor offices vs 2.4% of Constituent offices (cumulative), p=.006 by randomization inference. Numbers: 12.5% vs 2.4%; p=.006. (Table 1, p.552; text p.553)
- Legislative director or deputy chief of staff or above: 18.8% vs 5.5% (cumulative), p=.005. The text describes this as more than a threefold increase. Numbers: 18.8% vs 5.5%; p=.005; ~3.4x. (Table 1, p.552; text p.553)
- Member of Congress meetings: 7.8% vs 2.4%, p=.07. Not significant at .05 on its own, though the text describes it as more than three times as likely. Numbers: 7.8% vs 2.4%; p=.07; ~3.2x. (Table 1, p.552; text p.553-554)
- Abstract summary: senior policy makers were available between three and four times more often when the donor label was revealed. Ratios implied by Table 1 are about 3.2x (Member), 3.4x (LD or above), 5.2x (chief of staff or above). Numbers: 3-4x (abstract); 3.2x, 3.4x, 5.2x (derived). (Abstract p.545; Table 1 p.552)
- Lower-ranked staff showed no reliable difference: DC-based legislative assistant or local district director cumulative 37.5% vs 30.7%, p=.17; other district-based staffer cumulative 48.4% vs 43.3%, p=.26 (Revealed Donor figure first in each pair). Numbers: 37.5% vs 30.7%, p=.17; 48.4% vs 43.3%, p=.26. (Table 1, p.552)
- Any meeting: the text says 56.7% of Constituent and 51.6% of Revealed Donor offices scheduled any meeting, and that about half of offices granted meetings overall. Table 1 instead labels 56.7% and 51.6% as 'No Meeting', which reverses the text's meaning. The table columns sum to 100% under the 'No Meeting' reading, so the text wording looks inconsistent with the table. Numbers: 56.7% vs 51.6% (label conflict). (Text p.552 and p.553 (lines on 'scheduled any meeting'); Table 1 p.552)
- Ordered probit across the six ranks, with randomization-inference p-value: overall effect of the donor label on access level, p=.05. Numbers: p=.05. (p.554)
- Senior comparisons (chief of staff and above; LD and above) remain significant after a conservative Bonferroni correction across five comparisons at the .05 level; the text reports ps < .01 for these two. Numbers: ps < .01 (text); table p=.006 and .005. (p.554, fn 12 p.554)
- Cosponsorship outcome: only nine legislators cosponsored during the study, with the same share in both conditions. The authors say the estimate is too uncertain to bear on access effects; it tells us little. Numbers: 9 cosponsors total; equal shares. (Footnote 11, p.553)

## Limitations

- One experiment cannot establish which mechanism (quid pro quo, expected future donations, expertise, preference intensity, prognostic value of donors) produces the effect; the authors call mechanism 'ripe for future research' (p.555).
- The treatment is a label in a meeting request, not actual donations; the 'revealed donor' cue could also signal that the group cared more about the issue (fn 4, p.548).
- The design compares donors with constituents only; all offices were told attendees were constituents, so the relative importance of being a constituent versus a contributor is not measured (p.555).
- Access is not influence; the study does not show whether greater access changes policy outcomes (p.555-556). Cosponsorship results are too imprecise and may reflect outreach, not the meetings (p.553).
- Single organization (CREDO Action), single bill, one party's House members who had not cosponsored, and one three-month window (June-August 2013); replication in other settings is needed (p.555).
- Staffers may have read the donor label as meaning group members had given to that legislator, which the authors note (p.550).
- Table 1 'No Meeting' figures conflict with the text's 'scheduled any meeting' wording (p.552-553).
- Per-arm sample sizes are not reported in the text; the 64/127 split is derived from the design. The 192-to-191 reduction is explained in fn 5 (p.549).
- Supporting Tables S1 and S2 are not in the text file, so the covariate balance and response rules are cited but not checked.
- The abstract's 'three and four times' summary understates the ratio for chief of staff or above (about 5.2x by Table 1 arithmetic).

## Short quotes

- "senior policy makers made themselves available between three and four times more often" (Abstract, p.545)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | qualifies | Donor label raised senior access in 191 House offices of one party, June to August 2013: chief of staff or above 12.5% vs 2.4% (p=.006), legislative director or above 18.8% vs 5.5% (p=.005), Member 7.8% vs 2.4% (p=.07). Ratios implied by Table 1: about 5.2, 3.4 and 3.2. The paper reports no effect on votes or positions. (Table 1 p.552; text p.553-554; fn 11 p.553; influence limitation p.555) | pflip(n) = a/(a+b) at flip_model.py:92-96 puts a saturating CU-money term with WM 0.35 directly into a member's flip probability, against a peer term with WP 0.65. The CALIBRATED comment at flip_model.py:21 lists Kalla and Broockman among sources for money buys access/effort greater than votes. `fec/engine/flip_model.py:4-6,21,92-96` | The paper supports only the access step: one 2013 experiment, one liberal group, one bill, one party's House offices, a randomized donor label rather than dollars. It gives no estimate of WM, WP, HALFF or saturation, and no effect on votes or positions (p.555). The citation supports access before votes only if narrowed to access; the not-votes half is outside the paper. | strong | VERIFIED-SOFTENED |
| [[systems/melange-validation#Prospective receptivity contract (H1-H2) and forecast ledger\|Prospective receptivity contract (H1-H2) and forecast ledger]] | suggests-extension | The seniority of the official who attends responded to a randomized donor label across 191 offices over about three months. Cosponsorship as an outcome tells us little: nine legislators cosponsored in total, with the same share in both arms, and the authors say the estimate is too uncertain to bear on access effects. (Table 1 p.552; outcome coding p.552; fn 11 p.553) | H1 correlates frozen seed_prob with new cosponsorship count deltas by Spearman rank, requiring at least 60 days and at least 150 members with any delta. These thresholds match PROTOCOL_TEXT at outcome_contract.py:17-20. The summary's F5 replay figure of 72 of 511 new-event members was not found in the engine code or docs searched. `fec/engine/outcome_contract.py:17-20,245-277` | The transfer concerns outcome choice, not effect size. A seniority-of-attendee outcome needs a real outreach log, which the system does not have, plus its own pre-registered single look. Do not use the 72 of 511 figure until it is located. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Staffer closeness route search\|Staffer closeness route search]] | suggests-extension | Who is asked changes which tier takes the meeting. The donor label raised chief-of-staff-or-above access (12.5% vs 2.4%, p=.006) but not access at the district-staff tier (48.4% vs 43.3%, p=.26). (Table 1 p.552; design pp.550-552; fn 7 p.550) | The router returns the least-cost introduction route to staff anchored at a member office. A member-context modifier of 1 + MEMBER_BETA times value is applied to terminal costs (staffer_closeness_routes.py:913-918 and 1014). MEMBER_BETA = 0.15 is defined at closeness_math.py:21. Route costs are heuristic outputs, not access probabilities (D-025). `fec/surfaces/staffer_closeness_routes.py:913-918,1002,1014; fec/staffer_network/closeness_math.py:21` | The paper supports recording the seniority of the person reached as a validation target, matching its six-rank coding. It does not justify converting route costs or ranks into access probabilities, which would break D-025. The paper measured a requester attribute in district-office meetings, not a warm introduction through a co-worker tie. | moderate | VERIFIED-SOFTENED |
| [[systems/terrier-row-scoring#Sphere of Influence\|Sphere of Influence]] | qualifies | When an organized group tells an office its members are local campaign donors, senior officials attend more often than when described as constituents: three to four times in the abstract; Table 1 ratios about 3.2 for Member, 3.4 for legislative director or above, and 5.2 for chief of staff or above. (Abstract p.545; Table 1 p.552; limitations p.555) | Sphere of Influence = 0.45 giving + 0.45 volunteering + 0.10 household. Giving G = (charitable + cpi_donor)/4 + contrib_high/2. The weights status is a leaning ruling (framework_config.json:2556-2557). `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/framework_config.json:2556-2557` | The paper bears only on donor status made known to an office through an organized group's request. It says nothing about charitable giving, volunteering, or individuals acting alone, and it covers one liberal organization in 2013. Use it to justify revealing donor status in organized asks, not to reweight the giving term. The config does not define contrib_high, so its mapping to donor status is unverified. | weak | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Competitiveness multiplier c (COMPET_MULT)\|Competitiveness multiplier c (COMPET_MULT)]] | context | Offices were blocked on Obama's 2012 two-party vote share among six covariates, with omnibus balance p=.92 (chi-square 1.44, 5 df). The paper reports no heterogeneity of the donor-label effect by seat competitiveness. (Blocking and balance pp.549-550 (balance result p.549)) | c = 1 + AMPLITUDE*exp(-max(lean,0)/SCALE) multiplies effective dollars on both money legs. AMPLITUDE = 1.0 and SCALE = 5.0 are judgment constants, labeled uncalibrated, and the reciprocity they encode is not validated. `fec/moneyball_core.py:74-81,83,90-93` | Blocking on partisanship balances it across arms but does not estimate whether the donor effect differs in competitive seats; 64 treated offices could not have estimated that. The paper gives no support for c. The multiplier stays unvalidated (D-104..D-108). | weak | VERIFIED |
| [[systems/melange-influence-engine#Money response (lean shift) and money-triggered defection\|Money response (lean shift) and money-triggered defection]] | qualifies | The measured outcome was access: the rank of the official who attended. Positions and votes were not measured. Cosponsorship was nine legislators in total, with equal shares in both arms, and the authors say it tells little about effects. (Outcome p.552; fn 11 p.553; influence limitation p.555) | MAXSHIFT = 18, labeled CALIBRATED from 40 on the premise that money moves positions little and buys access and effort, not votes (money_response.py:15-17). LOSSFRAME = 1.10 treats competitor spending as a loss frame (money_response.py:31). The code cites Ansolabehere and Fowler-Garro-Spenkuch for this premise, not this paper. `fec/engine/money_response.py:15-17,31` | The paper can be cited for the access measure, but not for a null on positions: its cosponsorship estimate is uninformative rather than null. It says nothing about MAXSHIFT, HALF, DTHRESH or LOSSFRAME. The not-votes half of the premise lies outside the paper (p.555). | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: rel.summary_md presents the access half of the flip_model citation as support, but the code cites the paper for money buys access/effort greater than votes. The votes half is not in the paper and the summary does not say so.
> - Summary issue: rel.bearings flip_model and Terrier use 3 to 5 times; the paper says three and four times in the abstract, with Table 1 ratios of about 3.2, 3.4 and 5.2.
> - Summary issue: rel.bearings staffer routes claims MEMBER_BETA is explicitly not access evidence; no such comment exists at the cited lines or in closeness_math.py.
> - Summary issue: rel.bearings prospective contract cites an F5 replay of 72 of 511 new-event members; the figure was not found in the repo code or docs searched.
> - Summary issue: rel.related bombardini-2020-empirical-models-lobbying (cites) is removed: the review text does not mention Kalla or Broockman.
> - Summary issue: ext balance p=.92 is cited to p.550; it is on p.549. The no-vote note is cited to p.549 and p.554; it is on p.553 (fn 11).
> - Summary issue: rel.summary_md and ext say 9 offices cosponsored; the paper says nine legislators (p.553 fn 11). Same count, wording should match the paper.
> - Summary issue: Table 1 labels 56.7% and 51.6% as No Meeting while p.553 says those shares scheduled any meeting. ext flags the conflict; rel.summary_md omits it and should not present either figure as a meeting rate.
> - Summary issue: ext key findings 6 and 7 give cumulative pairs with the Revealed Donor figure first without stating the order. The values match Table 1 once the order is read that way.
> - Summary issue: rel.paul_alignment asserts Paul's stance and says his annotation is stale. The Paul entry does confirm the mislabel as Furnas et al., but no other Paul file was checked, so the stance claim is unverified and the Paul entry has no takeaways to attribute.

## Gaps and flags

- No measure of access in any component: nothing records which tier of official (Member, chief of staff, LD, LA, district staff) actually took a meeting. That is the outcome that responded to treatment in this paper.
- No outreach or contact log, so the staffer routes and the H1 seed_prob contract have no direct contact outcome. Cosponsorship deltas are a rare-event stand-in.
- The engine has no access stage between money and position. The flip family maps dollars straight into pflip, so the step the paper actually demonstrates (money to access) is not represented separately from the step it does not test (access to position).
- No estimate of how the donor-access effect varies with seat competitiveness. The competitiveness multiplier assumes such variation without evidence.
- The annotation is mislabeled. Paul's annotation record for this citation says its text is actually Furnas et al., so no Paul takeaway can be attributed to Kalla & Broockman until it is re-linked.
- Citation over-reach in code. fec/engine/flip_model.py:21 lists Kalla & Broockman among the sources behind 'CALIBRATED' WM/WP 0.35/0.65. The paper supports only the access step, not the votes half of that line, and calibrates no constant.
- Quote figures with their windows and denominators: 64 Revealed Donor vs 127 Constituent offices (derived from the design, not stated), one party's non-cosponsors, June to August 2013. The abstract's '3 to 4 times' understates the chief-of-staff ratio, which is about 5.2x by Table 1 arithmetic. Table 1's 'No Meeting' label contradicts the text's 'scheduled any meeting' (56.7% vs 51.6%).
- These are access rates measured in the experiment. Do not carry them over as access or introduction probabilities for graph routes or ranks (D-025).
- Do not use this paper to support the competitiveness multiplier. Reciprocity remains NOT validated (D-104..D-108).

It is a randomized test of the access half only. It does not support the money weight in pflip or any effect on positions. Paul's annotation record for this paper is misfiled, since the stored text belongs to Furnas et al., so there is no takeaway of his own to attribute to Kalla & Broockman. The code already cites the paper (flip_model.py:21) more strongly than the paper can bear.

## Related papers

- [[furnas-2019-moneyed-interests-information]] — complements — Survey experiment on 441 DC congressional staffers about money and information. Together with this field experiment it bears on the mechanism question the authors leave open.
- [[kim-2020-strategic-sequential-links]] — complements — Kim, Stuckatz and Wolters test whether donations precede lobbying on bills in observational LDA and FEC data. This randomized donor label supports the access step those sequences assume.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Furnas, LaPira and Brock show low overlap between organizational lobbying and PAC spending, which limits how often a donor label is available to lobbying actors.
- [[blackwell-2026-effect-political-advertising]] — complements — Blackwell and Yamauchi address unmeasured confounding in observational political-ad effects after Citizens United, the confounding problem this randomized design avoids.
- [[bonica-2025-money-campaign-finance]] — complements — Bonica and Grumbach show campaign money is much older than the electorate and wealth is concentrated. Their text also cites this paper's donor-access result. Relevant to who can credibly present as a donor.
- [[holyoke-2015-learnable-skills-unteachable]] — complements — Holyoke, Brown and LaPira describe lobbyist relationships built on mutual need. This experiment shows the donor signal shifts which staff tier engages.

### Linked from
- [[weimer-2023-causal-approach-detecting]] — complements — Kalla & Broockman randomize which congressional offices receive a donor-identified meeting request: the randomized ideal of Weimer's Table 1, in the political-contact domain.
- [[kalla-2023-which-narrative-strategies]] — complements — Same authors, different study. It is the money-buys-access field experiment that flip_model.py:21 actually cites. Keeping the two distinct prevents misattributing persuasion evidence to the flip model.
- [[bacik-2025-measuring-dynamical-evolution]] — cited-by — Bacik cites the access experiment (ref 49) when discussing access. The LDA data cannot observe that access.
- [[bombardini-2020-empirical-models-lobbying]] — complements — The review's account of money as access with small measured policy effects is consistent with Kalla and Broockman's evidence that contributions buy access.
- [[jacoby-2025-mathematical-modeling-analysis]] — complements — Kalla and Broockman provide experimental evidence that money buys access. Jacoby's finding that donations barely move votes is a simulated parallel with no causal design, and Kalla is the stronger basis for the flip model's WM < WP. Not cited by Jacoby.
- [[bonica-2013-mapping-ideological-marketplace]] — complements — Not cited in this paper. Conceptual pairing only: the investor model tests access-type motives without measuring access. Kalla's content is not verified against this text.
- [[kim-2023-systemic-sequential-links]] — complements — The paper cites Kalla and Broockman (2016) at p.4 as the closest field-experiment benchmark on donor access (text lines 68 and 131). Link is real.
- [[cagala-2021-optimal-targeting-fundraising]] — same-method — Item file title matches Kalla and Broockman, a randomized field experiment on a costly political instrument (campaign contributions and access). Both use randomized field designs for a costly political treatment.
- [[chohan-2025-crypto-oligarchy-cryptocurrency]] — complements — Randomized field experiment finding that contributions facilitate access to House offices, not electoral outcomes. It is adjacent to Chohan's unestimated influence claim rather than a direct contradiction, so the relation was changed from contradicts.
- [[goff-nd-cross-industry-finance]] — complements — Randomized field experiment on contributions and access; the nearest tested version of the report's money-buys-attention claim, which the report leaves untested. Use access wording, not influence.
- [[bouton-2022-small-campaign-donors]] — complements — Bouton et al. cite Kalla and Broockman (2016), a randomized field experiment titled Campaign Contributions Facilitate Access to Congressional Officials, in their introduction on elite responsiveness. Bouton et al. study where donor money goes, not whether it buys access.