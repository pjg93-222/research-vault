---
type: system
name: "Melange legislative activity and effectiveness data (incl. Draft Systems R pipeline)"
slug: melange-legislative-data
components:
  - melange-legislative-data.congress-activity-score
  - melange-legislative-data.ballotpedia-leadership-scores
  - melange-legislative-data.press-collector-cap
  - melange-legislative-data.category-taxonomy
  - melange-legislative-data.effectiveness-shadow
brain_notes:
  - C:/Users/pgoff/Documents/brain/actors/draft-systems-r-pipeline-architecture.md
  - C:/Users/pgoff/Documents/brain/actors/draft-systems-r-pipeline-code-catalogue.md
  - C:/Users/pgoff/Documents/brain/actors/sentinel-collectors.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-legislative-pillar-acu-review-effectiveness-shadow-lane.md
paper_count: 17
tags: [system, type/system-hub]
---

# Melange legislative activity and effectiveness data (incl. Draft Systems R pipeline)

Upstream legislative inputs: the congress.gov activity scrape with Congress decay, Ballotpedia committee, caucus and party-leadership scores, the signal-prioritized press collector cap, the release category taxonomy, synthetic billstatus rows, the sentinel bill ledger with strongest-role dedup, and the five-stage legislative-effectiveness shadow (Volden-Wiseman style) with its CEL criterion.

**Brain notes:** [actors/draft-systems-r-pipeline-architecture](obsidian://open?vault=brain&file=actors%2Fdraft-systems-r-pipeline-architecture); [actors/draft-systems-r-pipeline-code-catalogue](obsidian://open?vault=brain&file=actors%2Fdraft-systems-r-pipeline-code-catalogue); [actors/sentinel-collectors](obsidian://open?vault=brain&file=actors%2Fsentinel-collectors); [topics/2026-07-feature-legislative-pillar-acu-review-effectiveness-shadow-lane](obsidian://open?vault=brain&file=topics%2F2026-07-feature-legislative-pillar-acu-review-effectiveness-shadow-lane)

## Melange legislative activity and effectiveness data: evidence assessment

**Evidence base.** There are 18 evidence rows across 5 components from 17 distinct papers (16 VERIFIED-SOFTENED, 2 VERIFIED). However, several transfer notes say the *reproduction* each paper would need inside Melange stays UNVERIFIED until a second path confirms it (DE-062).

**Spread.** Evidence is concentrated on the five-stage effectiveness shadow, which has 10 of the 18 rows. The congress.gov activity score has a single weak sourcing row. The press cap has two weak cautionary rows. Nothing anywhere tests a hand-set constant this system uses:
- the 0.8 per-Congress decay
- the 180-day cosponsor half-life
- role weights 3/2/1
- leadership ladders 1.0 to 0.2
- the 70/30 composite
- the 1,000-URL cap

**What the evidence does well.** It justifies the *structure* of the effectiveness shadow. Bucchianeri et al. 2025 is a strong method source: 15 indicators, significance weights 1/5/10, and N/5 normalization per chamber-term. It also fixes CEL LES as the accepted outcome variable. Montgomery, Aroyehun, Battaglini and Aref all use LES as their dependent variable.

**What it does not do.** It does not validate the departures the shadow makes from LES: cosponsor role credit, timing decay, Congress decay, letters at weight 5, and the advocacy composite. The convergence check that would test them, shadow vs CEL LES for the same members and Congresses, has not been run. The data for it is already local (CEL 93rd-118th .dta). The only LES look that exists scores the *blended* score, not the shadow.

**The one stored criterion result is uninformative.** The ceremonial-share criterion is null: Spearman rho 0.0026 / 0.0176 / 0.0003, n = 440 members, 115th-118th Congresses, at least 8 bills. But it is an UNSTRATIFIED pooled rank test of an effect that RKH show is conditional on majority status. Read it as neither support nor refutation.

**Rejected SNA proposals.** No paper in this hub argues for either one: dropping regular cosponsorship edges, or per-edge volume normalization. The 2026-06-30 removal of synthetic billstatus "(co)sponsored" rows from the *press* layer is a different decision and must not be read as dropping cosponsorship edges.

## congress.gov legislative activity score with Congress decay

**Computes.** Per member x 17 sections x 3 roles: bill counts, average cosponsors and success rate, with counts decayed by 0.8^age per Congress (138 columns).

**Method.** Role swap on search URLs. Success comes from status text with a fallback classifier. The sentinel ledger also applies 0.8^(119-c) and keeps the strongest role per bill.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/congress_member_scraper.R`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/congress_search_urls.R`, `fec/legislation/sync_sentinel_bill_ledger.py`, `build_archive_legislative.py`

**Assumptions.**
- Recent Congresses matter more by a constant factor
- Search terms capture CU-relevant legislation

**Evidence table** (1 rows: 1 VERIFIED-SOFTENED)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[delano-2025-bicam-congressional-lobbying]] | 2025 | measurement-source | Bulk bills, amendments, committees and members from the official APIs with rolling updates. Retrieving the full set directly needs over 100 endpoints, a 5,000 requests per hour cap, and at least 10 requests per bill. | Sourcing cross-check only. Reconciling scraped counts against BICAM sponsor and cosponsor tables would need a run first. Remains UNVERIFIED. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: very thin. One VERIFIED-SOFTENED row of weak strength, and it is about sourcing only.**

Delano et al. 2025 (BICAM) shows that bulk bills, amendments, committees and members can be ingested from the official APIs, with rolling updates. Retrieval costs are real: more than 100 endpoints, a cap of 5,000 requests per hour, and at least 10 requests per bill.

The row does not address any of the component's three parts: the 0.8^age Congress decay; whether the search terms capture CU-relevant legislation (recall and precision); the accuracy of the status-text success classifier and its fallback.

The row also notes something unresolved. The sentinel ledger's `decay_weight` (1.0 for the 119th, 0.8 for the 118th, 0.64 for the 117th) is described in code as the 'frozen legacy archive schedule'. The code read does not show that this weight feeds the activity score. So it is unconfirmed whether the 0.8 decay in the ledger and the 0.8 in the R scrape are the same mechanism.

The only literature value that bears on decay is indirect, and it sits on another component. Sotoudeh et al. 2024 tested half-lives of 6, 12 and 24 months. Shorter decay separated passed from failed bills slightly better: relative differences 0.248 / 0.223 / 0.203, SE about 0.01-0.014. A 0.8 per-Congress decay equals a half-life of about 3.1 Congresses, roughly 74 months, far outside Sotoudeh's tested range, and Sotoudeh's outcome is not this component's outcome (predicting current-Congress CU activity). So it cannot set or reject 0.8. It does show the constant has no empirical anchor.

**Contradictions**
- Paul's cited sources for this component do not bear on the Congress decay or the search-term recall: Fouirnaies & Hall 2022 is about electoral incentives, Harrison et al. 2018 about lobbying-record mining, and Sotoudeh 2024 about cosponsor-network decay at 6-24 months. The citation set does not support the component's core assumptions.
- The 0.8 per-Congress decay (half-life about 3.1 Congresses, roughly 74 months) sits far outside the only tested decay range in the hub (Sotoudeh 2024: 6-24 months, where shorter was slightly better). The outcomes differ, so this is tension, not refutation.

**Gaps**
- No study tests the 0.8 decay. What would fill it: a rolling-origin backtest that predicts each member's Congress-c CU bill activity from Congresses c-1..c-k under decay values 0.5-1.0. Score by rank correlation or Poisson deviance, report House and Senate separately, and stratify by majority status.
- No recall or precision evidence for the CU search terms. What would fill it: a hand-coded reference set of CU-relevant bills (for example, a random sample of the 115th-118th Congresses drawn from Financial Sector / Commerce policy-area bills), with recall and precision of the search URLs reported for each of the 17 sections.
- No accuracy evidence for the status-text success classifier. What would fill it: a hand audit of a random bill sample, modelled on Bucchianeri 2025 (95.2% of 49,037 sampled bills coded correctly, over 99% after revision), with the error rate reported by stage.
- No reconciliation of scraped counts against official bulk data. What would fill it: a per-member diff of sponsor and cosponsor counts against BICAM tables for one Congress.

**Paul's position vs the evidence**

Paul wants a pipeline built from public records, and asked 'Same for the US?' next to Ardekani's point that legislation is machine-readable. BICAM answers the feasibility question with yes: US bill, member, committee and action records can be bulk-ingested from official APIs, at the cost of the stated rate limits. That is the only part of his position the evidence touches. Nothing in the hub supports the specific design choices: constant 0.8 decay, search-term capture, and success read from status text. His three cited works do not address them.

**Open questions**
- Does the sentinel ledger's 0.8^(119-c) decay_weight actually feed the activity score, or is it a separate frozen archive schedule?
- The component reports 138 columns, but 17 sections x 3 roles x 3 statistics = 153. Which section-role cells are missing or merged?
- Would a BICAM-based ingest replace the role-swapped search-URL scrape, or only audit it?
- Does strongest-role dedup in the ledger interact with the decay, for example a member sponsoring in Congress c-1 and cosponsoring in c?

**Read first:** [[delano-2025-bicam-congressional-lobbying]], [[sotoudeh-2024-network-based-measure]], [[bucchianeri-2025-legislative-effectiveness-american]]

## Ballotpedia committee, caucus and party-leadership scores

**Computes.** Committee leadership (Chair 1.0 ... Member 0.2), caucus leadership, party leadership (Speaker 1.0 ... Minority Whip 0.2) and a weighted electoral average (2018 x2).

**Method.** Bioguide-keyed (this fixed a five-Johnsons bug). Sub Layer AW:BA is normalized by x/max.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/ballotpedia_119th_cu_scraper.R`, `merge_committee_caucus.py`

**Assumptions.**
- Leadership positions indicate CU leverage (hand-set constants)

**Evidence table** (2 rows: 2 VERIFIED-SOFTENED)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2020-mapping-political-communities]] | 2020 | qualifies | Legislators with the most legislation-community memberships are often senior politicians or party leaders, and high-entropy legislators (entropy >= 2, roughly 4+ communities) sit on power committees. The paper measures breadth of lobbying interaction, not leverage. | Consistent with scoring leadership as general reach rather than CU-specific leverage; the 'diluted industry share' reading is the analyst's inference. Says nothing about effectiveness on CU bills. One Congress, descriptive. | weak | VERIFIED-SOFTENED |
| [[gaynor-2022-financial-ties-bind]] | 2022 | suggests-extension | Leadership predicts higher outward LPAC degree. Paul Ryan gave to 195 members and Joe Crowley to 80. Republican leaders give to 86% of members but about 30% of Freedom Caucus members (Table 4 Freedom -0.816***). Leader models use the full-party networks; the 238 R and 168 D connected-node counts are in the Table 2-3 notes, and Tables 4-5 give no N. | Paper treats ideological intraparty caucus membership as a structural variable for within-party money flows. System records only policy-caucus leadership. Adding faction membership needs a dated 119th Congress roster; the paper's Yellow Book membership is self-reported (fn.6). Membership would describe position in party, not CU leverage. | moderate | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: weak to moderate, and none of it tests the constants or the CU-leverage assumption.** There are two VERIFIED-SOFTENED rows.

**Gaynor 2022 (moderate)** is about party leaders as hubs of leadership-PAC money. Leadership predicts higher outward LPAC degree:
- Paul Ryan gave to 195 members and Joe Crowley to 80.
- Republican leaders gave to 86% of members, but to about 30% of Freedom Caucus members (Table 4, Freedom coefficient -0.816***).
- Node counts are 238 R and 168 D connected members (Tables 2-3 notes). Tables 4-5 report no N.

The system scores only policy-caucus *leadership* across 13 CU caucuses. Rank-and-file membership scores 0, and no ideological intraparty caucus is on the list. So the factional position Gaynor finds structurally important is invisible to the score.

**Kim & Kunisky 2020 (weak)** is descriptive and covers one Congress. Members with the most legislation-community memberships tend to be senior members or party leaders. High-entropy members (entropy >= 2, about 4+ communities) sit on power committees. The paper measures *breadth of lobbying interaction*, not leverage.

Neither paper validates the hand-set ladders: committee Chair 1.0 / Ranking 0.8 / Vice Chair 0.6 / Vice Ranking 0.4 / Member 0.2; party Speaker and President pro tempore 1.0 / Majority Leader 0.8 / Minority Leader 0.6 / Majority Whip 0.4 / Minority Whip 0.2; the 2018 x2 electoral weighting. Neither tests effectiveness on CU bills.

**Effectiveness-shadow evidence bears directly on this component.** Montgomery & Nyhan 2017 estimate a committee-chair direct effect on LES of 1.853 (90% CI 1.376-2.352), House. The lep_criterion docstring and RKH (Majority x C +0.27/+0.29) both mark majority status as a main driver of effectiveness. The party ladder puts Minority Leader (0.6) above Majority Whip (0.4), and the committee ladder treats Chair and Ranking as adjacent (1.0 vs 0.8). Both encode rank *within* a party and ignore the majority/minority fault line. The LES literature treats that fault line as first-order.

**Contradictions**
- Paul's position says leadership *insulates* members from the broader ecosystem (attributed to Gaynor). The verified Gaynor bearing shows the opposite on money ties: leaders have higher outward LPAC degree and give to most of their party (Ryan 195 members; R leaders 86% of members). The exception is reduced ties to Freedom Caucus members. As recorded, the position does not match what the paper's tables show.
- The ladders score Minority Leader (0.6) above Majority Whip (0.4) and treat Ranking Member (0.8) as near Chair (1.0). Evidence on effectiveness (a Montgomery & Nyhan chair effect of 1.853 on LES; majority-conditional effects in RKH) points to majority status and chair status as the dominant drivers. The constants are not refuted for 'leverage' as Paul defines it, but they run against the effectiveness evidence.

**Gaps**
- No study tests whether these leadership constants predict CU-bill effectiveness. What would fill it: in the 115th-118th Congresses, regress member CU-bill stage counts (AIC/ABC/PASS/LAW) on committee and party leadership indicators with majority status and its interaction. Report House and Senate separately. Then compare the data-derived ordering with the hand-set ladder.
- No evidence for the 2018 x2 weighting of the electoral average.
- No evidence that CU-caucus leadership (versus membership) carries leverage. What would fill it: a membership-vs-leadership comparison against CU-bill cosponsorship or advancement, using a dated 119th caucus roster rather than self-reported Yellow Book membership.

**Paul's position vs the evidence**

Paul's position has two parts. (1) Leadership insulates members from the broader ecosystem (Gaynor). (2) Senior leaders hold the broadest community memberships (Kim & Kunisky).

Part (2) matches the evidence descriptively, but Kim & Kunisky measure breadth of lobbying-community interaction in one Congress, not leverage or effectiveness. Their communities come from lobbying records. Under D-021, LDA-derived structure is an identity source, not an edge or weight source, so it cannot justify the leadership weights.

Part (1) is not what the verified Gaynor bearing shows. Leaders are money hubs with high outward degree, with reduced ties only to the Freedom Caucus faction. If Paul meant insulation from *industry* rather than intraparty ties, that is not in the evidence row, and he would need to source it separately.

Neither paper supports the specific constants.

**Open questions**
- What did Paul mean by 'insulates': insulation from lobbyists and industry, or from party rank-and-file? The Gaynor bearing contradicts the second reading.
- Should the ladders be split by majority/minority status, given that LES evidence treats majority and chair as first-order?
- Should ideological faction membership (e.g. Freedom Caucus) enter as a descriptive position-in-party field? Not as CU leverage, per the Gaynor transfer note.
- Does the x/max normalization of Sub Layer AW:BA compress a heavily skewed leadership distribution, given that most members score 0.2 or 0?

**Read first:** [[gaynor-2022-financial-ties-bind]], [[montgomery-2017-effects-congressional-staff]], [[kim-2020-mapping-political-communities]], [[ramey-2016-measuring-elite-personality]]

## Signal-prioritized press collection cap

**Computes.** Per-member URL cap (default 1000) prioritized by CU-term titles.

**Method.** Coverage 86.6% before fixes. P(CU-relevant | CU-term title) = 0.825 vs 0.0089 for generic titles. Coverage: D 92.5% vs R 87%; Senate 83% vs House 96%.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_text_collector.R`

**Assumptions.**
- CU-term titles proxy CU relevance; missing pages carry about 10% CU signal

**Evidence table** (2 rows: 2 VERIFIED-SOFTENED)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[malaterre-2024-inferring-social-networks]] | 2024 | qualifies | The quality of an HCoI network depends on the representativeness of the corpus, which is crucial for conclusions. The authors also flag the venues and languages they left out. | The caution applies. Before reading HCoI membership substantively, check party and chamber gaps in who is profiled (155 members profiled in the current artifact). Any figure should name its window and denominator. | weak | VERIFIED-SOFTENED |
| [[hargittai-2015-bigger-always-better]] | 2015 | suggests-extension | A sampling frame tied to the outcome can bias findings (Schwartz et al. 2013 opt-in app users, p. 65). Studies should state the frame scope and keep sampling separate from the outcome. | Hargittai's argument applies only if coverage gaps correlate with CU stance or volume. The cap drops only generic pages (about 0.9 percent CU-relevant per the registry). The cap is not party- or chamber-specific, so 'favours House and Democratic offices' does not follow from the code; it rests on unmeasured coverage rates. The collector comment assumes the skipped bulk is missing at random, which is the untested premise. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: weak. Two VERIFIED-SOFTENED rows, both cautionary, neither empirical about this corpus.**

**Hargittai 2015.** A sampling frame tied to the outcome can bias findings, so a study should state its frame scope and keep sampling separate from the outcome. The evidence row reads the code (cu_text_collector.R lines 112-140) and sharpens what this means here: every CU-candidate-titled page is fetched; generic pages are capped at 1,000 per member, newest first; the cap is neither party- nor chamber-specific. So the cap cannot by itself produce a 'favours House and Democratic offices' bias. Any differential coverage would have to come from upstream (site structure or title conventions), and that is not measured.

**Malaterre & Lareau 2024.** The quality of an HCoI network depends on how representative the corpus is. That applies directly, because this corpus feeds the HCoI profiles: 155 members profiled in the current artifact.

**Registry figures are unverified.** Neither evidence row could re-measure them from the corpus or locate them in code: 86.6% coverage before fixes; P(CU-relevant | CU-term title) 0.825 vs 0.0089 for generic titles; coverage D 92.5% vs R 87%; Senate 83% vs House 96%. They carry no stated window or denominator. Until they are re-measured, they are registry claims, not findings.

**Contradictions**
- The component's assumption that 'missing pages carry about 10% CU signal' and the registry's 0.0089 CU-relevance rate for generic titles are not reconciled anywhere. They may be compatible (a low rate times a large volume), but neither figure was re-measured, and no window or denominator is given.
- The framing that differential coverage biases the press pillar implies the cap causes it. Per the Hargittai row, the cap is uniform across party and chamber, so any D/R or House/Senate gap comes from elsewhere in collection.

**Gaps**
- No measurement of coverage by party and chamber with a named window and denominator. What would fill it: recompute coverage (pages fetched / pages discovered) per member from the collector logs for a stated collection window, then summarise by party and chamber with the member counts.
- No test of the missing-at-random premise behind the cap. What would fill it: fetch a random sample of capped-out generic pages (for example 50 per stratum across party x chamber), hand-code CU relevance, and estimate the CU rate per stratum with confidence intervals.
- No sensitivity test of downstream effects. What would fill it: rebuild press-pillar scores and HCoI memberships with the cap lifted for a sample of members, and report rank movement.

**Paul's position vs the evidence**

Paul's position is 'No literature.' That is accurate in the sense that no paper tests this cap. The two bearings add general cautions (frame-outcome coupling; corpus representativeness for HCoI) and point to the specific check that is missing: whether skipped generic pages differ in CU content by party or chamber. The evidence does not show the cap is biased. It shows the bias premise is untested and the coverage figures behind it are unverified.

**Open questions**
- Where do the 86.6% coverage and the D/R and Senate/House coverage figures come from, and over what window and denominator?
- Do Senate sites (83% coverage per the registry) use title conventions that hide CU content behind generic titles?
- Is 'newest first' for generic pages correlated with any outcome the press pillar measures, such as recent CU salience?

**Read first:** [[hargittai-2015-bigger-always-better]], [[malaterre-2024-inferring-social-networks]]

## Release category taxonomy and billstatus synthetic rows

**Computes.** 19 Layer-2 categories under 7 groups via regex, net and count per member; synthetic 'X (co)sponsored' rows from BILLSTATUS XML.

**Method.** Billstatus rows were removed from Stage 5 on 2026-06-30; 110 members then had no real releases.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R (CATEGORIES, stage_bill_cosponsors)`

**Assumptions.**
- Regex terms identify a release's topic
- Cosponsorship is a press-layer signal (later rejected)

**Evidence table** (3 rows: 3 VERIFIED-SOFTENED)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[malaterre-2024-inferring-social-networks]] | 2024 | suggests-extension | k was chosen by combining Cv coherence (scanned over k = 5 to 100, with no coherence gain above 35) and manual reading of top words; any topic model is acceptable if assignments are continuous probabilities rather than crisp. | The continuous-assignment requirement is met only loosely: member profiles are continuous after averaging, item tags are crisp. A coherence-selected LDA on the same corpus or a precision sample of the regex tags would show whether communities reflect the taxonomy or the text. Any result stays a diagnostic until reproduced. | moderate | VERIFIED-SOFTENED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | suggests-extension | LLM topic labels can be checked with an optimal-label task. In 400 speeches a blinded coder picked the GPT-3 label over three random intruders 80 percent of the time, against 25 percent chance. Keyword presence overcounts: 5332 speeches contain honor, but only 145 are labelled as praising it. | The paper labels are open-ended virtue labels for 2013-2014 House speeches; Melange uses fixed CU policy categories on releases and posts. The intruder protocol transfers as a measurement check on existing regex tags. Adopting LLM-assigned categories would be a separate decision. The R scraper CATEGORIES regex was not opened. | moderate | VERIFIED-SOFTENED |
| [[porter-2025-campaignview-campaign-platforms]] | 2025 | method-source | Each platform point is hand-coded by trained annotators to one of 14 Major Policy Topic codes using a codebook. A 20% relabel (8,584 points, by 2022 coders) gave 80% agreement, Cohen kappa 0.84 and weighted kappa 0.74, with most disagreement on Unknown/Other. | Transfers as a validation design (double-code a random sample, report kappa), not as labels: genre and topic scheme differ from CU categories. The paper's single-topic rule under-codes multi-topic text, but the regex tagger does not share that rule. | moderate | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: moderate on validation *design*, zero on the taxonomy's actual accuracy.** There are three VERIFIED-SOFTENED rows. All three supply ways to check the regex tagger. None reports a check of it.

- **Porter et al. 2025 (CampaignView), method source.** Double-code a random sample and report kappa. A 20% relabel (8,584 platform points) gave 80% agreement, Cohen kappa 0.84 and weighted kappa 0.74. The design transfers; the labels do not, because the genre and topic scheme differ. The regex tagger is multi-label, so CampaignView's single-topic under-coding problem does not apply.
- **Ornstein et al. 2024.** Keyword presence overcounts badly: 5,332 speeches contain 'honor' but only 145 are labelled as praising it. A blinded intruder task found the LLM label 80% of the time against 25% chance (400 speeches). That is a direct warning for regex term lists. The intruder protocol transfers as a check on the existing tags. Adopting LLM categories would be a separate decision.
- **Malaterre & Lareau 2024.** The topic count was chosen by Cv coherence plus manual reading, and assignments should be continuous. In Melange, item tags are crisp and split evenly (press_hcoi.py lines 25-27, 45). Only member-level profiles become continuous after averaging.

The only human-label reliability figure in the system is for *stance*, not topic: regex kappa 0.17 and DEBATE 0.34 (run_stage5.R line 133). That low regex stance kappa is a reason for caution about regex topic tags too, though not evidence about them.

On the billstatus synthetic rows: their removal from Stage 5 (2026-06-30) left 110 members with no real releases. No paper addresses this. It is a coverage consequence, not a measurement question. It is also distinct from the rejected SNA proposal to drop regular cosponsorship edges: this concerns press-layer rows, not network edges.

**Contradictions**
- Category count is inconsistent. The component registry and the Porter row say 19 Layer-2 categories. The Malaterre row counted 20 entries in the CATEGORIES block of cu_press_release_scraper.R and 20 categories in the stored press_hcoi.json.
- The rows describe tagging differently: crisp labels split evenly per item (Malaterre row) versus 'tags a release with EVERY category it touches' (Porter row). These are compatible (multi-label crisp with an even split), but the even split assumes each touched category matters equally, which nothing tests.

**Gaps**
- No precision or recall of the regex topic tags against human coding. What would fill it: a stratified random sample of releases (for example 30 per category), double-coded by two coders using a written codebook. Report per-category precision, recall and Cohen kappa (the CampaignView design), plus an Ornstein-style intruder test on the category labels.
- No coherence or overlap check on the category set. What would fill it: a coherence-selected topic model on the same release corpus, then a comparison of whether HCoI communities track the regex taxonomy or the text (per the Malaterre transfer).
- No evidence for the even-split weighting of multi-label items.

**Paul's position vs the evidence**

Paul's position is that no annotation addresses the taxonomy, and that is accurate. His one citation, Bacik et al. 2026 (79 issue codes), is LDA issue-code structure. Under D-021, LDA is an identity source, not an edge or weight source, so it can inform category naming at most. It cannot calibrate weights. The hub evidence does not contradict the taxonomy. It shows the taxonomy is unvalidated, and Ornstein's keyword-overcount result makes the regex approach's precision the first thing to measure.

**Open questions**
- Is the taxonomy 19 or 20 categories, and which stored artifacts (press_hcoi.json, media blend CATKW) use which set?
- Does build_media_blend.py's CATKW (MR.RAW_PATTERNS) match the R scraper's CATEGORIES regex, or are there two taxonomies?
- For the 110 members left without real releases after billstatus rows were removed, does the press pillar treat them as missing or as zero?
- Which downstream surfaces (media blend, press category weights) consume these tags? The Ornstein row says this was not traced.

**Read first:** [[ornstein-2024-train-your-stochastic]], [[porter-2025-campaignview-campaign-platforms]], [[malaterre-2024-inferring-social-networks]]

## Five-stage legislative effectiveness shadow and CEL criterion

**Computes.** Role-weighted, timing-decayed credit per stage (BILL, AIC, ABC, PASS, LAW), normalized N/5 per chamber-term, decayed 0.8 per Congress. Results-only and 70/30 advocacy composite percentiles. Ceremonial-share criterion against CEL LES.

**Method.** Role weights 3/2/1; cosponsor timing 2^(-delay/180); impact weights 1/5/10. A shadow candidate, not activated.

**Code refs.** `fec/engine/legislative_effectiveness.py:17-498`, `fec/engine/effectiveness_integration.py:23-212`, `fec/members/lep_criterion.py`

**Assumptions.**
- Stage progression measures effectiveness (Volden-Wiseman)
- Earlier cosponsorship is worth more
- More conscientious members sponsor fewer ceremonial bills (RKH)

**Evidence table** (10 rows: 8 VERIFIED-SOFTENED, 2 VERIFIED)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | suggests-extension | Effectiveness is the CEL LES: fifteen indicators, normalized to mean 1 per session. The paper compares coalition-mean LES with 95% CI bands across party, k=2 and k=3 partitions. The third coalition is the most effective in most sessions and is anchored by a few members (e.g. Christopher Smith, mean effectiveness 8.44, 106th-108th). | The LES half of the comparison is already local. Reproducing it also needs a House-only, per-Congress signed co-sponsorship backbone and a k=3 partition, and neither is built. The result would be descriptive, with no controls for majority or chair status, which drive LES. It stays UNVERIFIED until a second path reproduces it (DE-062). Any pooled comparison across sessions must be stratified by majority party or labelled UNSTRATIFIED. | moderate | VERIFIED |
| [[ramey-2016-measuring-elite-personality]] | 2016 | qualifies | Conscientiousness lowers the ceremonial share of sponsored bills: -0.52 (M1), -0.39 (M3), -0.53 (M4), -0.58 (M5, Congress FE), all p<0.01. Majority x C is +0.27 (M4) and +0.29 (M5), so the C effect is smallest in the majority. N = 3,648 member-Congresses in M3 to M5, 104th to 112th Congresses; binomial regression with implicit weighting by bills proposed (fn 37). | The paper's effect is minority-concentrated, so a pooled rank correlation is an UNSTRATIFIED test of a conditional claim. Modern Congresses carry few ceremonial bills, which attenuates the test. A stratified, bill-weighted re-run is new analysis and stays UNVERIFIED until a second path reproduces it. | moderate | VERIFIED-SOFTENED |
| [[sotoudeh-2024-network-based-measure]] | 2024 | suggests-extension | Shorter decay gives slightly more separation within the tested range: relative differences 0.248, 0.223 and 0.203 at 6, 12 and 24-month half-lives (SE 0.0139, 0.0119, 0.0107). The paper lists a comparison with LES as future work and does not run it. | The paper proposes an LES comparison; it does not propose a sub-Congress decay check, and it already tested 6 and 12-month half-lives. Its outcome (separating passed from failed bills) differs from the shadow's goal (convergence with LES), so it cannot set the decay value. Its evidence covers only 6-24 months. | weak | VERIFIED-SOFTENED |
| [[boxsteffensmeier-2018-taking-congress-interest]] | 2018 | supports | Stage-specific coefficients (Tables 3-4): endorser social power predicts cosponsors (0.055) but not reporting (0.020) or passage (-0.035). Endorser count predicts passage (0.013) but not reporting (0.004, ns). Cosponsor count predicts reporting (0.002, p<.01) and passage (0.004, p<.001). | The paper gives stage-specific coefficients on one sample of letter bills (N = 6,597) and does not argue for stage-based modelling; the stage pattern is an empirical observation only. It does not test cosponsor timing decay and gives no support for the 180-day half-life. Pooled, UNSTRATIFIED. | weak | VERIFIED-SOFTENED |
| [[bucchianeri-2025-legislative-effectiveness-american]] | 2025 | method-source | SLES is a weighted average of 15 indicators: five stage shares (introduced, AIC, ABC, passed chamber, law) at three significance weights (1, 5, 10), normalized by N/5 so each chamber-term averages 1 (Eq. 1). Window 1987-2018, 97 chambers, 80,344 legislator-term scores. Hand audit: 46,693 of 49,037 sampled bills (95.2%) correctly coded, over 99% after revision. North Carolina 2005-2012: SLES explains about half the variation in NCCPPR rankings and beats hit-rate metrics. | The stage, weight and normalization structure transfers to the federal shadow as a structural adaptation. The paper's validation covers sponsor-based state SLES with newspaper-coded significance; it does not validate cosponsor role credit, the 180-day timing decay, letters at weight 5, or the 70/30 composite. Those stay UNVERIFIED until checked against CEL LES for the same members and Congresses. | strong | VERIFIED-SOFTENED |
| [[aref-2020-detecting-coalitions-optimally]] | 2020 | supports | House bill passage rate fell 0.11 pp per session (beta -0.528, p<0.05); Senate fell 0.35 pp per session (beta -0.852, p<0.01). What mediates passage differs by chamber. | The paper's passage rate is a chamber-session share of bills. It does not discuss normalization, stage weights or decay, so it supports within-chamber normalization only by analogy. Its measure is chamber-level, covers 1979-2016, and is association rather than cause. | weak | VERIFIED-SOFTENED |
| [[aroyehun-2026-politics-of-feelings]] | 2026 | qualifies | On member-Congress LES (n = 13,671; Tables 7-9, all models with session and topic fixed effects): anger -0.037 (95% CI -0.046 to -0.029, p < 0.001) is the only significant negative emotion term; enthusiasm +0.016 (CI 0.007 to 0.025) and pride +0.011 (CI 0.003 to 0.019, p < 0.01) are positive; fear, disgust, sadness, joy and hope are not significant. Marginal R2 is 0.113 to 0.118 in Tables 7-8 and 0.118 to 0.119 in Table 9. The paper reports no controls-only model, so an incremental R2 for emotion terms is not stated. | Transfers as context only: speech tone is a small associational correlate of LES once structural controls are in, so a shadow-vs-LES gap should not be read as a rhetoric effect. No identification, so not usable as a predictor. Quote coefficients as UNSTRATIFIED with window 93rd-118th Congresses and n = 13,671 member-Congresses; the paper pools over majority-flip fault lines. | moderate | VERIFIED-SOFTENED |
| [[battaglini-2018-effectiveness-connected-legislators]] | 2018 | suggests-extension | Network spillovers are positive at every stage. Corrected phi is 0.5634 (SE 0.0539) for BILL only, 0.8387 (SE 0.0465) for BILL plus AIC and 0.8643 (SE 0.0767) for ABC plus PASS plus LAW, with the selection term significant in each (BILL xi = -0.1601, SE 0.0687). House, 109th-113th. | A spillover term needs a network that is exogenous or corrected relative to the stage outcomes. Paul's cosponsor edges are partly built from the same bills the shadow credits, which is a circularity risk. Any test stays UNVERIFIED until reproduced by a second path. | moderate | VERIFIED |
| [[harbridge-yong-2023-bipartisan-path-effective]] | 2023 | suggests-extension | Stage models use a member's count of bills reaching each stage as the DV, with legislator FE and one-tailed tests. House: committee action (A8.2) 0.894 SE 0.204; beyond committee (A8.3) 0.853; passing chamber (A8.4) 0.800; law (A8.5) 0.485 SE 0.088; all p<0.001. Senate: committee action (A9.2) 1.767, not significant; law (A9.5) 0.589, p<0.05. | The paper DVs are counts of a member's own bills at each stage, not stage rates and not LES. A shadow-vs-LES check would need a per-Congress attracted share (not built) and a comparable LES series. It stays a diagnostic. Quote the coefficients as count-DV associations, House and Senate separately. | moderate | VERIFIED-SOFTENED |
| [[montgomery-2017-effects-congressional-staff]] | 2017 | suggests-extension | Members sharing senior staff have modestly correlated LES net of covariates (pooled rho 0.043). Committee chair direct effect is 1.853 (90% CI 1.376 to 2.352); indirect effect via ties is 0.442 (90% CI 0.131 to 0.826). | Optional second criterion: shadow scores should show a similar small lag on senior-title ties if they track LES. This is UNVERIFIED, needs office_moves restricted to senior titles, and is not evidence that the shadow score is valid. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: the best-covered component (10 rows, 2 VERIFIED), but the evidence validates the *frame*, not this implementation.**

**Structure, strong.** Bucchianeri et al. 2025 is a method source of strong strength. SLES is 15 indicators: five stages at significance weights 1/5/10, normalized N/5 per chamber-term. Its window is 1987-2018: 97 chambers, 80,344 legislator-term scores. A hand audit found 95.2% of 49,037 sampled bills coded correctly. In North Carolina 2005-2012, SLES explains about half the variation in NCCPPR rankings.

This is a state-legislature result. It transfers as structure, because it adapts the federal Volden-Wiseman LES. It does not validate the federal shadow's departures from LES: role credit 3/2/1 (LES credits sponsors); cosponsor timing 2^(-delay/180); Congress decay 0.8; letters forced to weight 5; the 70/30 advocacy/results composite. The code also shows the CU significance weight arrives from an upstream impact record that was not traced.

**Stage logic, supportive but pooled.**
- **Box-Steffensmeier et al. 2018** (Dear Colleague letter bills, N = 6,597, pooled, UNSTRATIFIED). Cosponsor *count* predicts reporting (0.002, p<.01) and passage (0.004, p<.001). Endorser predictors differ by stage. The paper does not test cosponsor *timing*.
- **Harbridge-Yong et al. 2023.** Bipartisanship is associated with a member's bill counts at each stage, with legislator fixed effects. House: committee action 0.894, law 0.485, all p<0.001. Senate: committee action 1.767 is not significant; law 0.589, p<0.05. The shadow has no cross-party term.
- **Aref & Neal 2020.** Chamber-level, 1979-2016. House passage rate fell 0.11 pp per session and Senate 0.35 pp per session. This supports within-chamber normalization only by analogy.

**Convergence with LES has not been run.** The decisive empirical question has no answer. The Harbridge-Yong and Montgomery rows both report that no code compares the *shadow* with CEL LES; verify_effectiveness_shadow.py checks internal consistency only. lep_criterion.py scores the *blended* score against LES. The convergence bar is rho >= +0.30 per the Aroyehun row. The CEL data (93rd-118th) is already local, so this is the cheapest high-value test in the system.

**Ceremonial-share C criterion: null, but not a valid test.** RKH (Ramey et al. 2016) report: C lowers ceremonial share (-0.52 to -0.58, p<0.01); Majority x C is +0.27/+0.29, so the C effect is minority-concentrated; the model is binomial and implicitly bill-weighted; N = 3,648 member-Congresses, 104th-112th Congresses. Melange's stored test is a pooled Spearman: rho 0.0026 / 0.0176 / 0.0003, n = 440, 115th-118th Congresses, at least 8 bills. It has no majority split and no bill weighting, and modern Congresses carry few ceremonial bills. This is an UNSTRATIFIED test of a conditional claim. The null is uninformative; it neither supports nor refutes. The script's own comment flags the attenuation. RKH's year is also recorded inconsistently in the repo (2016 vs 2019).

**Timing and decay constants: no support.**
- Sotoudeh 2024 tested 6, 12 and 24-month half-lives for separating passed from failed bills (relative differences 0.248 / 0.223 / 0.203). The 180-day half-life sits at the paper's best-performing end. But that decay is on cosponsorship-network ties, not on a cosponsor's delay after introduction, so it does not validate 2^(-delay/180).
- The 0.8 Congress decay (about 3.1 Congresses) is far outside the tested range.
- Sotoudeh *proposes* an LES comparison as future work and does not run it.

**Extensions (leads, all UNVERIFIED in Melange until reproduced).**
- **Battaglini et al. 2018, VERIFIED** (House 109th-113th). Network spillovers are positive at every stage: phi 0.5634 for BILL only, 0.8387 for BILL+AIC, 0.8643 for ABC+PASS+LAW. A spillover term in the shadow would face circularity, because Melange cosponsor edges are partly built from the same bills the shadow credits.
- **Aref & Neal 2021, VERIFIED.** A third coalition is most effective in most sessions. This needs a House per-Congress signed backbone and a k=3 partition, neither of which is built. It is descriptive, with no majority or chair controls, so any pooled comparison must be stratified by majority or labelled UNSTRATIFIED.
- **Montgomery & Nyhan 2017.** Members sharing senior staff have LES correlated at pooled rho 0.043. The chair direct effect is 1.853 (90% CI 1.376-2.352). This offers an optional second criterion, not a validity test.
- **Aroyehun 2026** (93rd-118th Congresses, n = 13,671 member-Congresses, UNSTRATIFIED). Anger is -0.037, enthusiasm +0.016 and pride +0.011. Marginal R2 is about 0.11-0.12, with no controls-only baseline. This is context only: a shadow-vs-LES gap should not be read as a rhetoric effect.

**Contradictions**
- The assumption 'more conscientious members sponsor fewer ceremonial bills (RKH)' has a stored null in Melange (rho about 0, n = 440, 115th-118th). It is not a genuine contradiction: RKH's effect is minority-concentrated (Majority x C +0.27/+0.29) and bill-weighted, while the Melange test is pooled and unweighted. It must not be cited as either replication or refutation until a stratified, bill-weighted re-run exists.
- Sotoudeh 2024 is cited as a 'contrast' with LES, but the paper only proposes an LES comparison as future work. It reports no divergence from LES.
- The 0.8 per-Congress decay (about 3.1 Congresses half-life) is outside, and points the other way from, the only tested decay range (Sotoudeh: 6-24 months, shorter slightly better). The outcomes differ, so this is tension, not refutation.
- Senate stage results are weaker and differ from the House (Harbridge-Yong: Senate committee-action term not significant; Aref 2020: Senate passage decline about 3x the House's). Any pooled House+Senate shadow validation would cross a known fault line and must be stratified by chamber.

**Gaps**
- Shadow-vs-CEL LES convergence has not been run. What would fill it: per chamber and per Congress (115th-118th, or 93rd-118th where the shadow can be built), Spearman between the shadow and lesclassic for the same members, stratified by majority/minority and chair status. Then ablate each departure from LES (role credit, timing decay, Congress decay, letters at 5, the 70/30 composite) to see which ones move convergence.
- No evidence for cosponsor timing decay. What would fill it: test whether cosponsor delay after introduction predicts AIC/ABC/PASS for CU bills. Model the bill's advancement on cosponsor join-time distributions, controlling for cosponsor count, majority sponsor and committee, with House and Senate separate.
- The ceremonial-share criterion needs a valid re-run: a bill-weighted binomial model with majority x C, Congress fixed effects, and the same 115th-118th window, reported for majority and minority separately.
- No evidence for the 70/30 advocacy/results composite weights or for letters at significance weight 5.
- Source of the CU-mention significance weight (upstream impact record, line 225) not traced.

**Paul's position vs the evidence**

Paul highlighted LES as the effectiveness criterion. The evidence strongly supports that choice. CEL LES and its state analogue have a validated construction (Bucchianeri's hand audit; SLES explains about half of NCCPPR rankings in North Carolina 2005-2012). LES is also the standard outcome across Montgomery, Battaglini, Aroyehun and Aref.

But choosing LES as the criterion obliges a convergence test, and that test has not been run on the shadow. The existing look covers the blended score.

He also highlighted Sotoudeh's contrast with LES. The verified bearing says Sotoudeh only *proposes* that comparison, so there is no empirical contrast to lean on.

The shadow's departures from LES are unsupported in the hub: role credit, the 180-day timing, the 0.8 decay, letters at 5, and the composite. They are where the shadow could diverge from LES, and they are what the convergence test should ablate.

**Open questions**
- Should the shadow aim to *converge* with LES (making role credit and timing decay unnecessary deviations) or to *improve on* it? If the latter, what external criterion beats LES?
- Where does the CU-mention significance weight come from, and is it independent of the outcome the shadow is validated against?
- Should the ceremonial-share C criterion be retained at all, given the attenuation in modern Congresses? Would a majority-stratified, bill-weighted version still have power at n = 440?
- Can a spillover term (Battaglini) be built on a network exogenous to the credited bills, for example prior-Congress ties or senior-staff ties (Montgomery), to avoid circularity?
- How should the RKH year conflict (2016 vs 2019) be settled in the citation record?

**Read first:** [[bucchianeri-2025-legislative-effectiveness-american]], [[ramey-2016-measuring-elite-personality]], [[harbridge-yong-2023-bipartisan-path-effective]], [[battaglini-2018-effectiveness-connected-legislators]], [[boxsteffensmeier-2018-taking-congress-interest]], [[sotoudeh-2024-network-based-measure]]

## Across components

**1. Hand-set decay constants with no empirical anchor.** The 0.8 per-Congress decay appears in three places: the activity score, the sentinel ledger (0.8^(119-c)) and the effectiveness shadow. The 180-day cosponsor half-life is a second hand-set decay. The only decay evidence in the hub (Sotoudeh 2024: 6-24-month half-lives, with shorter slightly better at separating passed from failed bills) concerns a different quantity and outcome. One rolling-origin backtest across decay values would inform all three components.

**2. Majority status is the unhandled fault line.** RKH's C effect is minority-concentrated. LES rewards majority and chair status (lep_criterion docstring). Montgomery & Nyhan put the chair direct effect on LES at 1.853. Yet the leadership ladders ignore majority status, the ceremonial-share test pools across it, and no shadow validation stratifies by it. House and Senate also behave differently (Harbridge-Yong; Aref 2020). Every pooled coefficient in this system should carry majority and chamber strata, or be labelled UNSTRATIFIED.

**3. Validation designs are available; validations are not.** CampaignView's double-coded kappa, Ornstein's intruder test and Bucchianeri's hand audit are concrete designs for the taxonomy, the success classifier and stage coding. None has been run on Melange data. Several registry figures (coverage percentages, P(CU | title)) are stated without window or denominator and were not re-measured.

**4. Circularity risk.** Melange cosponsor edges are partly built from the bills the shadow credits. Any network-spillover extension (Battaglini) or coalition comparison (Aref 2021) must use a network exogenous to the credited bills. Graph ranks and shared-background scores remain heuristic model outputs, not closeness or access probabilities (D-025).

**5. Guardrails.** LDA-derived structure (Kim & Kunisky communities, Bacik issue codes) can inform identity and naming but not weights (D-021). No paper here argues for either rejected SNA proposal. The press-layer removal of synthetic billstatus rows is not the rejected 'drop cosponsorship edges' proposal. Nothing here bears on reciprocity, which remains not validated (D-104..D-108).

## Research agenda

1. Run shadow vs CEL LES convergence (data already local at fec/lep/*.dta). Use per-chamber, per-Congress Spearman against lesclassic for 115th-118th, stratified by majority/minority and chair status, then ablate role credit, cosponsor timing decay, Congress decay, letters at weight 5 and the 70/30 composite one at a time.
2. Re-run the RKH ceremonial-share criterion validly: a bill-weighted binomial model with majority x C and Congress fixed effects, 115th-118th Congresses, reported for majority and minority separately. Until then, label the stored rho about 0 (n = 440) UNSTRATIFIED and uninformative.
3. Backtest the 0.8 per-Congress decay: a rolling-origin prediction of each member's Congress-c CU activity from prior Congresses under decay values 0.5-1.0, by chamber, with the window and member denominator named. Apply the result to the activity score, the sentinel ledger and the shadow.
4. Test cosponsor timing: does a cosponsor's delay after introduction predict CU-bill AIC/ABC/PASS, net of cosponsor count, majority sponsor and committee, House and Senate separately? This decides whether 2^(-delay/180) stays.
5. Validate the regex taxonomy: settle 19 vs 20 categories; double-code a stratified sample of about 30 releases per category and report per-category precision, recall and kappa; add an Ornstein-style intruder test.
6. Audit press coverage: recompute coverage by party and chamber from collector logs with a named window and denominator; hand-code a random sample of capped-out generic pages per stratum to test the missing-at-random premise; rerun press-pillar ranks with the cap lifted for a sample of members.
7. Test the leadership ladders: regress 115th-118th CU-bill stage counts on committee and party leadership with majority interaction, by chamber, and compare the data-derived ordering with the hand-set 1.0-0.2 constants.
8. Audit success-status coding and search recall: a hand audit of a random bill sample (Bucchianeri-style) for the status-text classifier, a CU reference bill set for search-term recall, and a per-member reconciliation of scraped counts against BICAM for one Congress.
9. Lead only (UNVERIFIED, DE-062): a Battaglini-style spillover or Aref k=3 coalition comparison, only on a network exogenous to the credited bills (prior-Congress or senior-staff ties), with majority-stratified reporting.
