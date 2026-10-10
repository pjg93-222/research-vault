---
type: system
name: "Melange legislative activity and effectiveness data (incl. Draft Systems R pipeline)"
slug: melange-legislative-data
components: [melange-legislative-data.congress-activity-score, melange-legislative-data.ballotpedia-leadership-scores, melange-legislative-data.press-collector-cap, melange-legislative-data.category-taxonomy, melange-legislative-data.effectiveness-shadow]
brain_notes: [actors/draft-systems-r-pipeline-architecture, actors/draft-systems-r-pipeline-code-catalogue, actors/sentinel-collectors, topics/2026-07-feature-legislative-pillar-acu-review-effectiveness-shadow-lane]
paper_count: 13
tags: [system]
---

# Melange legislative activity and effectiveness data (incl. Draft Systems R pipeline)

Upstream legislative inputs: the congress.gov activity scrape with Congress decay, Ballotpedia committee, caucus and party-leadership scores, the signal-prioritized press collector cap, the release category taxonomy, synthetic billstatus rows, the sentinel bill ledger with strongest-role dedup, and the five-stage legislative-effectiveness shadow (Volden-Wiseman style) with its CEL criterion.

Brain notes: [actors/draft-systems-r-pipeline-architecture](obsidian://open?vault=brain&file=actors/draft-systems-r-pipeline-architecture); [actors/draft-systems-r-pipeline-code-catalogue](obsidian://open?vault=brain&file=actors/draft-systems-r-pipeline-code-catalogue); [actors/sentinel-collectors](obsidian://open?vault=brain&file=actors/sentinel-collectors); [topics/2026-07-feature-legislative-pillar-acu-review-effectiveness-shadow-lane](obsidian://open?vault=brain&file=topics/2026-07-feature-legislative-pillar-acu-review-effectiveness-shadow-lane)

**System:** Melange legislative activity and effectiveness data, including the Draft Systems R pipeline. It has five components and 14 evidence rows (13 distinct papers).

**Evidence base, all 14 rows:**
- **Status:** 1 VERIFIED, 13 VERIFIED-SOFTENED. None is UNVERIFIED.
- **Strength:** 1 strong, 6 moderate, 7 weak.
- **Where the rows sit:** congress-activity-score 1, ballotpedia-leadership-scores 2, press-collector-cap 2, category-taxonomy 3, effectiveness-shadow 6.

**Overall verdict.** The literature supports the *structure* of the effectiveness shadow: five stages, impact weights 1/5/10, and N/5 normalisation within each chamber-term. That support comes from a state-legislature paper (Bucchianeri et al. 2025), so it is a structural adaptation, not a Congress validation.

No row validates any of these hand-set values:
- the 0.8 per-Congress decay, used in three places
- the 3/2/1 role weights
- the 180-day cosponsor-timing half-life
- letters forced to weight 5
- the 70/30 advocacy/results composite
- the leadership constants (Chair 1.0 down to Member 0.2; Speaker 1.0 down to Minority Whip 0.2)
- the 1000-URL press cap
- the regex topic tags

The press cap and the taxonomy have *method* sources only: designs for checking them (double-coding with kappa, intruder tests, sampling-frame audits), and no result about Melange's own tags.

**The one local empirical test is null and unstratified.** It is the RKH ceremonial-share criterion: rho of about 0.00 to 0.02, n = 440, pooled over the 115th to 118th Congresses. Ramey et al. report the effect is concentrated in the minority party, so the local test is UNSTRATIFIED and attenuated. It does not refute the paper, and it does not support the C criterion either.

**Paul's positions vs the evidence.**
- Several of the cited works do not bear on the component they are attached to. Fouirnaies & Hall and Harrison et al. are cited under the activity score. "Aref & Neal 2021" is cited as the LES source, but it uses CEL LES as an outcome; it is not the LES source.
- Paul's "leadership insulates" reading of Gaynor is not what the extracted Gaynor finding says.

No paper in this set argues for either of the two rejected SNA proposals (dropping regular cosponsorship edges; per-edge volume normalisation), so nothing needs flagging on that score. One distinction does need keeping clear. The 2026-06-30 removal of the billstatus synthetic rows took cosponsorship out of the *press layer*. It is not the rejected network proposal to drop cosponsorship *edges*, and the two should not be conflated.

## congress.gov legislative activity score with Congress decay

**What it computes:** Per member x 17 sections x 3 roles: bill counts, average cosponsors and success rate, with counts decayed by 0.8^age per Congress (138 columns).

**Method:** Role swap on search URLs. Success comes from status text with a fallback classifier. The sentinel ledger also applies 0.8^(119-c) and keeps the strongest role per bill.

**Code refs:** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/congress_member_scraper.R`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/congress_search_urls.R`, `fec/legislation/sync_sentinel_bill_ledger.py`, `build_archive_legislative.py`

**Assumptions:**
- Recent Congresses matter more by a constant factor
- Search terms capture CU-relevant legislation

**Empirical questions:**
- Does a 0.8 per-Congress decay maximize prediction of current-Congress CU activity from past activity?

**Evidence (1 rows, 1 distinct papers; status: 1 VERIFIED-SOFTENED; strength: 1 weak):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[delano-2025-bicam-congressional-lobbying]] | 2025 | measurement-source | Bulk bills, amendments, committees and members from the official APIs with rolling updates. Retrieving the full set directly needs over 100 endpoints, a 5,000 requests per hour cap, and at least 10 requests per bill. | Sourcing cross-check only. Reconciling scraped counts against BICAM sponsor and cosponsor tables would need a run first. Remains UNVERIFIED. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: very thin.** There is one row: delano-2025 (BICAM), a weak, VERIFIED-SOFTENED measurement source. It establishes that federal bill, amendment, committee and member data can be pulled in bulk from the official APIs, with real costs: more than 100 endpoints, a cap of 5,000 requests per hour, and at least 10 requests per bill. It is a sourcing cross-check and says nothing about whether the score is valid.

Nothing in the evidence tests:
- the 0.8^age per-Congress decay (a half-life of about 3.1 Congresses)
- the strongest-role-per-bill dedup
- the fallback success classifier
- the recall of the CU search terms

**Indirect bearing on the decay.** Sotoudeh et al. (2024), filed under the effectiveness shadow, tested time-decay half-lives of only 6 to 24 months. Shorter half-lives separated passed from failed bills slightly better: relative differences 0.248, 0.223 and 0.203 at 6, 12 and 24 months. That outcome (bill passage) is different from this component's target (predicting current-Congress CU activity), and the 0.8 per-Congress value lies far outside the tested range. So it neither supports nor contradicts the decay value; it only shows that nobody in the set has tested decay at this scale.

**Code-tracing ambiguity.** The BICAM row reports that sync_sentinel_bill_ledger.py applies 0.8^(119-c) as a "frozen legacy archive schedule". It also reports that the code it read does not show that weight feeding the activity score. The component description says it does. That is unresolved.

**Contradictions**
- The component says the sentinel ledger's 0.8^(119-c) decay feeds the activity score. The delano-2025 row reports that the code read does not show this weight feeding the score; it is described as a frozen legacy archive schedule. Which decay reaches the 138 columns is unconfirmed.
- Paul cites Sotoudeh et al. 2024 here, but its decay evidence covers 6 to 24 month half-lives on bill passage. The system's 0.8 per Congress (half-life of about 3.1 Congresses, or about 6 years) is well outside anything the paper tested.

**Gaps**
- Decay value (GAP). Needed: a held-out prediction study. Fit past CU activity (113th to 118th Congresses) at decay values from 0.5 to 1.0 against observed 119th-Congress CU activity, scored by rank correlation or log-loss, and report the curve with its window and denominator.
- Search-term recall (GAP). Needed: a recall audit. Compare search-URL bill sets against BICAM or govinfo BILLSTATUS bills, coded by policy area or hand-labelled CU relevance, and report per-section recall and precision.
- Success-rate fallback classifier (GAP). Needed: an agreement check against official latest-action and law-status fields on a random sample of bills.
- Strongest-role dedup (GAP). Needed: a sensitivity check of rank movement with and without the dedup. This is diagnostic, not literature.

**Paul's position vs the evidence**

Paul wants a pipeline from public records and asked whether US legislation is machine-readable, as Ardekani says of his own setting ('Same for the US?'). BICAM answers yes at the data level: official APIs carry bills, actions, committees and members in bulk, and the system already uses govinfo BILLSTATUS. So the real open question is narrower: does the scraped search-URL count match the bulk record? None of the three works Paul cites tests this component. Fouirnaies & Hall is about electoral incentives and is used in the receptivity layer. Harrison et al. is lobbying record mining. Sotoudeh's decay range does not reach the per-Congress scale.

**Open questions**
- Does the 0.8 per-Congress weight actually enter the 138-column activity score, or only the sentinel ledger and archive?
- What recall do the congress.gov search terms achieve for CU-relevant bills against a bulk-data baseline?
- Is the 0.8 decay a deliberate choice or a carried-over legacy constant? If it is legacy, has Paul ruled on keeping it?

**Read first:** [[delano-2025-bicam-congressional-lobbying]], [[sotoudeh-2024-network-based-measure]], [[aref-2020-detecting-coalitions-optimally]]

## Ballotpedia committee, caucus and party-leadership scores

**What it computes:** Committee leadership (Chair 1.0 ... Member 0.2), caucus leadership, party leadership (Speaker 1.0 ... Minority Whip 0.2) and a weighted electoral average (2018 x2).

**Method:** Bioguide-keyed (this fixed a five-Johnsons bug). Sub Layer AW:BA is normalized by x/max.

**Code refs:** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/ballotpedia_119th_cu_scraper.R`, `merge_committee_caucus.py`

**Assumptions:**
- Leadership positions indicate CU leverage (hand-set constants)

**Empirical questions:**
- Does party leadership rank predict effectiveness on CU bills?

**Evidence (2 rows, 2 distinct papers; status: 2 VERIFIED-SOFTENED; strength: 1 moderate, 1 weak):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2020-mapping-political-communities]] |  | qualifies | Legislators with the most legislation-community memberships are often senior politicians or party leaders, and high-entropy legislators (entropy >= 2, roughly 4+ communities) sit on power committees. The paper measures breadth of lobbying interaction, not leverage. | Consistent with scoring leadership as general reach rather than CU-specific leverage; the 'diluted industry share' reading is the analyst's inference. Says nothing about effectiveness on CU bills. One Congress, descriptive. | weak | VERIFIED-SOFTENED |
| [[gaynor-2022-financial-ties-bind]] | 2022 | suggests-extension | Leadership predicts higher outward LPAC degree. Paul Ryan gave to 195 members and Joe Crowley to 80. Republican leaders give to 86% of members but about 30% of Freedom Caucus members (Table 4 Freedom -0.816***). Leader models use the full-party networks; the 238 R and 168 D connected-node counts are in the Table 2-3 notes, and Tables 4-5 give no N. | Paper treats ideological intraparty caucus membership as a structural variable for within-party money flows. System records only policy-caucus leadership. Adding faction membership needs a dated 119th Congress roster; the paper's Yellow Book membership is self-reported (fn.6). Membership would describe position in party, not CU leverage. | moderate | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: weak to moderate. Both rows are VERIFIED-SOFTENED, and neither tests the component's question,** which is whether leadership rank predicts effectiveness on CU bills.

- **kim-nd (weak, qualifies).** Legislators with the most legislation-community memberships tend to be senior members or party leaders, and high-entropy legislators sit on power committees. This measures breadth of lobbying interaction, not leverage. It covers one Congress and is descriptive. The paper builds its communities from lobbying data. Under D-021, LDA is an identity source, not an edge or weight source, so community breadth must not be turned into a weight on the leadership score. Under D-025, a community-membership score is a heuristic model output, never closeness, access or an introduction probability.
- **gaynor-2022 (moderate, suggests-extension).** Leadership predicts higher outward leadership-PAC giving. Paul Ryan gave to 195 members and Joe Crowley to 80. Republican leaders give to 86% of members but to only about 30% of Freedom Caucus members (Table 4, Freedom -0.816***). The leader models use full-party networks (238 R and 168 D connected nodes); Tables 4 and 5 give no N. The paper treats ideological faction membership as structural. The system records only policy-caucus leadership in 13 CU caucuses, and rank-and-file membership scores 0.

**What is unsupported.** Nothing in the evidence supports the specific hand-set ordering of the constants, the 2018 double-weight in the electoral average, or the x/max normalisation.

**Contradictions**
- Paul says leadership insulates members from the broader ecosystem, citing Gaynor. The extracted Gaynor finding points the other way for leaders: they have the widest outward money reach (Ryan to 195 members; Republican leaders to 86% of members). The insulation-like result concerns Freedom Caucus *members* receiving less from leaders, not leaders being insulated.
- Paul's two readings pull in opposite directions: Gaynor read as insulation, Kim & Kunisky read as the broadest community memberships. The evidence rows support only the breadth reading. Kim & Kunisky measures breadth of lobbying interaction, not leverage, and its 'diluted industry share' interpretation is the analyst's own inference.

**Gaps**
- Leadership-to-effectiveness link (GAP). Needed: regress CEL LES (local files, 93rd to 118th Congresses), or advancement of CU bills, on committee and party leadership positions, stratified by majority status. Compare the implied ordering and spacing with the hand-set constants (1.0, 0.8, 0.6, 0.4, 0.2). Any pooled coefficient across majority and minority must be stratified or labelled UNSTRATIFIED.
- Caucus leadership value (GAP). Needed: test whether chairing one of the 13 CU caucuses predicts CU-bill sponsorship or advancement beyond committee position.
- Electoral-average weighting, with 2018 counted twice (GAP). No rationale or evidence was found. Needed: sensitivity of ranks to equal weighting.

**Paul's position vs the evidence**

Paul's position rests on two readings: leaders are insulated (Gaynor) and senior leaders hold the broadest community memberships (Kim & Kunisky). The evidence supports the breadth reading only weakly, from one Congress and descriptively. It does not support the insulation reading for leaders. Neither paper links leadership to CU effectiveness, which is the component's actual question. The constants therefore remain design priors, not evidence-backed weights.

**Open questions**
- Do the local CEL .dta files carry chair, subcommittee-chair and party-leadership indicators that would let the constants be tested without new collection?
- Should ideological faction membership (Freedom Caucus, Progressive Caucus, and so on) be recorded as context? If so, from which dated 119th-Congress roster? Gaynor's Yellow Book data is self-reported.
- Is the leadership score meant to measure general reach or CU-specific leverage? The evidence supports only the former.

**Read first:** [[gaynor-2022-financial-ties-bind]], [[kim-2020-mapping-political-communities]], [[bucchianeri-2025-legislative-effectiveness-american]], [[aref-2021-identifying-hidden-coalitions]]

## Signal-prioritized press collection cap

**What it computes:** Per-member URL cap (default 1000) prioritized by CU-term titles.

**Method:** Coverage 86.6% before fixes. P(CU-relevant | CU-term title) = 0.825 vs 0.0089 for generic titles. Coverage: D 92.5% vs R 87%; Senate 83% vs House 96%.

**Code refs:** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_text_collector.R`

**Assumptions:**
- CU-term titles proxy CU relevance; missing pages carry about 10% CU signal

**Empirical questions:**
- Does differential coverage by party or chamber bias the press pillar?

**Evidence (2 rows, 2 distinct papers; status: 2 VERIFIED-SOFTENED; strength: 2 weak):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[malaterre-2024-inferring-social-networks]] | 2024 | qualifies | The quality of an HCoI network depends on the representativeness of the corpus, which is crucial for conclusions. The authors also flag the venues and languages they left out. | The caution applies. Before reading HCoI membership substantively, check party and chamber gaps in who is profiled (155 members profiled in the current artifact). Any figure should name its window and denominator. | weak | VERIFIED-SOFTENED |
| [[hargittai-2015-bigger-always-better]] | 2015 | suggests-extension | A sampling frame tied to the outcome can bias findings (Schwartz et al. 2013 opt-in app users, p. 65). Studies should state the frame scope and keep sampling separate from the outcome. | Hargittai's argument applies only if coverage gaps correlate with CU stance or volume. The cap drops only generic pages (about 0.9 percent CU-relevant per the registry). The cap is not party- or chamber-specific, so 'favours House and Democratic offices' does not follow from the code; it rests on unmeasured coverage rates. The collector comment assumes the skipped bulk is missing at random, which is the untested premise. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: weak.** There are two VERIFIED-SOFTENED rows, both general sampling-frame cautions. Nothing studies this kind of capped collector directly.

- **malaterre-2024 (weak, qualifies).** Network quality depends on how representative the corpus is. Before HCoI membership is read substantively, party and chamber gaps among the profiled members should be checked. The current artifact profiles 155 members; that is the denominator any coverage figure must name.
- **hargittai-2015 (weak, suggests-extension).** A sampling frame tied to the outcome can bias findings. The transfer row is careful on three points:
  - The cap drops only generic pages, which the registry puts at about 0.9% CU-relevant.
  - The cap is not party- or chamber-specific in code, so 'favours House and Democratic offices' does not follow from the code.
  - The collector's missing-at-random premise is untested.

**Key point: the coverage figures are unverified.** D 92.5% vs R 87% and Senate 83% vs House 96% were not found in code and were not re-measured from the corpus. They currently rest on registry text alone, so the bias question in the component cannot yet be posed with measured inputs.

**Contradictions**
- The component states coverage gaps (D 92.5% vs R 87%; Senate 83% vs House 96%) and 86.6% overall. Both evidence rows report these figures were not located in code or re-measured; they match registry text only.
- The assumption that 'missing pages carry about 10% CU signal' sits unreconciled beside P(CU-relevant | generic title) = 0.0089. The two can agree only if a large mass of generic pages sums to about 10% of total signal. The denominator for the 10% figure is not stated.

**Gaps**
- Differential-coverage bias (GAP). Needed: a stratified audit. Draw a random sample of skipped generic pages per party-by-chamber stratum, hand-code CU relevance, estimate missed CU signal per stratum with CIs, then recompute press-pillar ranks with inverse-probability weights and report rank movement.
- Re-measured coverage (GAP). Needed: recompute D/R and House/Senate coverage from the current corpus with an explicit window and denominator.
- Title-proxy precision (partial). The 0.825 vs 0.0089 figures need a documented sample, coder protocol and date. A Porter-style double-coded sample with kappa would settle it.

**Paul's position vs the evidence**

Paul's position is 'No literature.' That is accurate: neither row studies a capped, title-prioritised collector. The rows add two cautions, corpus representativeness (Malaterre) and outcome-linked sampling frames (Hargittai). They also show that the party and chamber bias claim is not established. Coverage gaps are asserted from registry text, and the code cap itself is party- and chamber-neutral.

**Open questions**
- Where were the 86.6%, D/R and House/Senate coverage figures computed: which script, window and denominator?
- Does the 'about 10% CU signal' in missing pages mean the share of total CU signal or the per-page relevance? It is unreconciled with 0.0089.
- Do coverage gaps correlate with CU stance or volume? That is the only condition under which Hargittai's concern bites.

**Read first:** [[hargittai-2015-bigger-always-better]], [[malaterre-2024-inferring-social-networks]], [[porter-2025-campaignview-campaign-platforms]], [[ornstein-2024-train-your-stochastic]]

## Release category taxonomy and billstatus synthetic rows

**What it computes:** 19 Layer-2 categories under 7 groups via regex, net and count per member; synthetic 'X (co)sponsored' rows from BILLSTATUS XML.

**Method:** Billstatus rows were removed from Stage 5 on 2026-06-30; 110 members then had no real releases.

**Code refs:** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R (CATEGORIES, stage_bill_cosponsors)`

**Assumptions:**
- Regex terms identify a release's topic
- Cosponsorship is a press-layer signal (later rejected)

**Empirical questions:**
- What is the precision of regex topic tagging against human coding?

**Evidence (3 rows, 3 distinct papers; status: 3 VERIFIED-SOFTENED; strength: 3 moderate):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[malaterre-2024-inferring-social-networks]] | 2024 | suggests-extension | k was chosen by combining Cv coherence (scanned over k = 5 to 100, with no coherence gain above 35) and manual reading of top words; any topic model is acceptable if assignments are continuous probabilities rather than crisp. | The continuous-assignment requirement is met only loosely: member profiles are continuous after averaging, item tags are crisp. A coherence-selected LDA on the same corpus or a precision sample of the regex tags would show whether communities reflect the taxonomy or the text. Any result stays a diagnostic until reproduced. | moderate | VERIFIED-SOFTENED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | suggests-extension | LLM topic labels can be checked with an optimal-label task. In 400 speeches a blinded coder picked the GPT-3 label over three random intruders 80 percent of the time, against 25 percent chance. Keyword presence overcounts: 5332 speeches contain honor, but only 145 are labelled as praising it. | The paper labels are open-ended virtue labels for 2013-2014 House speeches; Melange uses fixed CU policy categories on releases and posts. The intruder protocol transfers as a measurement check on existing regex tags. Adopting LLM-assigned categories would be a separate decision. The R scraper CATEGORIES regex was not opened. | moderate | VERIFIED-SOFTENED |
| [[porter-2025-campaignview-campaign-platforms]] | 2025 | method-source | Each platform point is hand-coded by trained annotators to one of 14 Major Policy Topic codes using a codebook. A 20% relabel (8,584 points, by 2022 coders) gave 80% agreement, Cohen kappa 0.84 and weighted kappa 0.74, with most disagreement on Unknown/Other. | Transfers as a validation design (double-code a random sample, report kappa), not as labels: genre and topic scheme differ from CU categories. The paper's single-topic rule under-codes multi-topic text, but the regex tagger does not share that rule. | moderate | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: moderate as method, nil as result.** There are three VERIFIED-SOFTENED, moderate rows. All are designs for checking the taxonomy; none measures Melange's regex tags.

- **porter-2025 (method-source).** Each item is hand-coded to one of 14 major topics, and a 20% relabel (8,584 points) gave 80% agreement and Cohen kappa 0.84. This transfers as a validation design (double-code a random sample, report kappa), not as labels. Melange's tagger is multi-label, so Porter's single-topic rule does not apply.
- **ornstein-2024.** An intruder test checks topic labels: a blinded coder picked the true label 80% of the time against 25% chance. Keyword presence overcounts badly: 5,332 speeches contain 'honor' but only 145 are labelled as praising it. This bears directly against the assumption that regex terms identify a release's topic, though in a different domain (virtue labels in 2013-2014 House speeches).
- **malaterre-2024.** Choose k by Cv coherence plus manual reading, and use continuous assignments. Melange's item tags are crisp; member profiles become continuous only after averaging.

**Warning sign from inside the pipeline.** The only human-label kappa found is for stance, not topic: regex 0.17, DEBATE 0.34. Low regex reliability elsewhere in the same pipeline is a reason to expect the topic regex is unvalidated too.

**Billstatus synthetic rows.** These were removed from Stage 5 on 2026-06-30, leaving 110 members with no real releases. The underlying assumption ('cosponsorship is a press-layer signal') is marked rejected. No evidence row addresses the 110-member coverage hole this created.

**Contradictions**
- Category count conflicts. The component and the porter-2025 row say 19 Layer-2 categories. The malaterre-2024 row counts 20 entries in the CATEGORIES block of cu_press_release_scraper.R and 20 categories in press_hcoi.json.
- Which regex map is in use is unclear. The ornstein-2024 row traces release and post categories to CATKW = MR.RAW_PATTERNS in build_media_blend.py:50 and did not open the R CATEGORIES block. The component describes the R scraper's regex. Whether these are the same taxonomy is unconfirmed.
- The ornstein-2024 finding that keyword presence heavily overcounts (5,332 vs 145) cuts against the component assumption that regex terms identify a release's topic. Domain transfer is partial.

**Gaps**
- Regex tag precision and recall (GAP, and the component's own empirical question). Needed: a stratified random sample of about 300 to 400 releases across categories, party and chamber, double-coded by two humans, with per-category precision, recall and Cohen kappa (Porter design). Add an intruder test on category labels (Ornstein design).
- Taxonomy vs text structure (GAP). Needed: a coherence-selected topic model (k scanned, Cv plus reading) on the same corpus, cross-tabbed against the regex categories, to show whether HCoI communities reflect the taxonomy or the text (Malaterre).
- Post-removal coverage hole (GAP). Needed: profile the 110 members with no real releases by party, chamber and seniority, and state how the press pillar treats them.

**Paul's position vs the evidence**

Paul's position is 'No annotation addresses the taxonomy.' That fits the evidence: the three rows supply validation designs, not validation. Paul cites Bacik et al. 2026 (79 issue codes), which are LDA issue codes. Under D-021, LDA is an identity source, so a crosswalk from CU categories to LDA issue codes is acceptable for naming and alignment but not as a weight or edge source. The evidence favours measuring the existing regex before replacing it; adopting LLM-assigned categories would be a separate decision.

**Open questions**
- Is the live taxonomy 19 or 20 categories, and is the R CATEGORIES block the same map as MR.RAW_PATTERNS in build_media_blend.py?
- Which downstream surfaces (media blend, press category weights, HCoI) consume these tags? That determines how much a low precision would matter.
- How should the 110 members with no real releases be scored: as missing, as zero, or imputed?

**Read first:** [[porter-2025-campaignview-campaign-platforms]], [[ornstein-2024-train-your-stochastic]], [[malaterre-2024-inferring-social-networks]]

## Five-stage legislative effectiveness shadow and CEL criterion

**What it computes:** Role-weighted, timing-decayed credit per stage (BILL, AIC, ABC, PASS, LAW), normalized N/5 per chamber-term, decayed 0.8 per Congress. Results-only and 70/30 advocacy composite percentiles. Ceremonial-share criterion against CEL LES.

**Method:** Role weights 3/2/1; cosponsor timing 2^(-delay/180); impact weights 1/5/10. A shadow candidate, not activated.

**Code refs:** `fec/engine/legislative_effectiveness.py:17-498`, `fec/engine/effectiveness_integration.py:23-212`, `fec/members/lep_criterion.py`

**Assumptions:**
- Stage progression measures effectiveness (Volden-Wiseman)
- Earlier cosponsorship is worth more
- More conscientious members sponsor fewer ceremonial bills (RKH)

**Empirical questions:**
- Does the shadow score converge with published CEL LES for the same members and Congresses?
- Does cosponsor timing (early joiners) predict bill advancement?

**Evidence (6 rows, 6 distinct papers; status: 1 VERIFIED, 5 VERIFIED-SOFTENED; strength: 1 strong, 2 moderate, 3 weak):**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[aref-2021-identifying-hidden-coalitions]] | 2021 | suggests-extension | Effectiveness is the CEL LES: fifteen indicators, normalized to mean 1 per session. The paper compares coalition-mean LES with 95% CI bands across party, k=2 and k=3 partitions. The third coalition is the most effective in most sessions and is anchored by a few members (e.g. Christopher Smith, mean effectiveness 8.44, 106th-108th). | The LES half of the comparison is already local. Reproducing it also needs a House-only, per-Congress signed co-sponsorship backbone and a k=3 partition, and neither is built. The result would be descriptive, with no controls for majority or chair status, which drive LES. It stays UNVERIFIED until a second path reproduces it (DE-062). Any pooled comparison across sessions must be stratified by majority party or labelled UNSTRATIFIED. | moderate | VERIFIED |
| [[ramey-2016-measuring-elite-personality]] | 2016 | qualifies | Conscientiousness lowers the ceremonial share of sponsored bills: -0.52 (M1), -0.39 (M3), -0.53 (M4), -0.58 (M5, Congress FE), all p<0.01. Majority x C is +0.27 (M4) and +0.29 (M5), so the C effect is smallest in the majority. N = 3,648 member-Congresses in M3 to M5, 104th to 112th Congresses; binomial regression with implicit weighting by bills proposed (fn 37). | The paper's effect is minority-concentrated, so a pooled rank correlation is an UNSTRATIFIED test of a conditional claim. Modern Congresses carry few ceremonial bills, which attenuates the test. A stratified, bill-weighted re-run is new analysis and stays UNVERIFIED until a second path reproduces it. | moderate | VERIFIED-SOFTENED |
| [[sotoudeh-2024-network-based-measure]] | 2024 | suggests-extension | Shorter decay gives slightly more separation within the tested range: relative differences 0.248, 0.223 and 0.203 at 6, 12 and 24-month half-lives (SE 0.0139, 0.0119, 0.0107). The paper lists a comparison with LES as future work and does not run it. | The paper proposes an LES comparison; it does not propose a sub-Congress decay check, and it already tested 6 and 12-month half-lives. Its outcome (separating passed from failed bills) differs from the shadow's goal (convergence with LES), so it cannot set the decay value. Its evidence covers only 6-24 months. | weak | VERIFIED-SOFTENED |
| [[boxsteffensmeier-2018-taking-congress-interest]] | 2018 | supports | Stage-specific coefficients (Tables 3-4): endorser social power predicts cosponsors (0.055) but not reporting (0.020) or passage (-0.035). Endorser count predicts passage (0.013) but not reporting (0.004, ns). Cosponsor count predicts reporting (0.002, p<.01) and passage (0.004, p<.001). | The paper gives stage-specific coefficients on one sample of letter bills (N = 6,597) and does not argue for stage-based modelling; the stage pattern is an empirical observation only. It does not test cosponsor timing decay and gives no support for the 180-day half-life. Pooled, UNSTRATIFIED. | weak | VERIFIED-SOFTENED |
| [[bucchianeri-2025-legislative-effectiveness-american]] | 2025 | method-source | SLES is a weighted average of 15 indicators: five stage shares (introduced, AIC, ABC, passed chamber, law) at three significance weights (1, 5, 10), normalized by N/5 so each chamber-term averages 1 (Eq. 1). Window 1987-2018, 97 chambers, 80,344 legislator-term scores. Hand audit: 46,693 of 49,037 sampled bills (95.2%) correctly coded, over 99% after revision. North Carolina 2005-2012: SLES explains about half the variation in NCCPPR rankings and beats hit-rate metrics. | The stage, weight and normalization structure transfers to the federal shadow as a structural adaptation. The paper's validation covers sponsor-based state SLES with newspaper-coded significance; it does not validate cosponsor role credit, the 180-day timing decay, letters at weight 5, or the 70/30 composite. Those stay UNVERIFIED until checked against CEL LES for the same members and Congresses. | strong | VERIFIED-SOFTENED |
| [[aref-2020-detecting-coalitions-optimally]] | 2020 | supports | House bill passage rate fell 0.11 pp per session (beta -0.528, p<0.05); Senate fell 0.35 pp per session (beta -0.852, p<0.01). What mediates passage differs by chamber. | The paper's passage rate is a chamber-session share of bills. It does not discuss normalization, stage weights or decay, so it supports within-chamber normalization only by analogy. Its measure is chamber-level, covers 1979-2016, and is association rather than cause. | weak | VERIFIED-SOFTENED |

**Assessment**

**Evidence weight: the best-supported component in this system, but the support covers structure, not parameters.** It has 6 rows: 1 VERIFIED, 5 VERIFIED-SOFTENED; strength 1 strong, 2 moderate, 3 weak.

- **bucchianeri-2025 (strong, method-source).**
  - **What it validates:** stages, 1/5/10 significance weights and N/5 normalisation per chamber-term match the code (lines 17, 232 and 500). It covers 1987-2018, 97 chambers and 80,344 legislator-term scores, with a hand audit at 95.2% (over 99% after revision). In North Carolina, SLES explains about half the variation in NCCPPR rankings.
  - **Transfer condition:** it is state legislatures, sponsor-only, with newspaper-coded significance. A state result is not a Congress result.
  - **What it does not validate:** cosponsor role credit (3/2/1), the 180-day timing decay, letters forced to weight 5, the 0.8 per-Congress decay, or the 70/30 composite.
- **aref-2021 (VERIFIED, moderate).** Uses CEL LES (15 indicators, mean 1 per session) as an outcome across coalition partitions. The CEL House and Senate files for the 93rd to 118th Congresses are already local. Reproducing its coalition result needs a signed House cosponsorship backbone and a k=3 partition, neither of which is built. It is descriptive, with no majority or chair controls. The paper's claim is verified; Melange's reproduction of it is UNVERIFIED until a second path reproduces it (DE-062).
- **ramey-2016 (moderate, qualifies).** Conscientiousness lowers the ceremonial share of sponsored bills: -0.39 to -0.58, all p<0.01, N = 3,648 member-Congresses, 104th to 112th Congresses. The interaction Majority x C is +0.27 to +0.29, so the effect is concentrated in the minority. The local lep_criterion test is a pooled Spearman over the 115th to 118th Congresses (n = 440, at least 8 bills) and gives rho 0.0026, 0.0176 and 0.0003. That is null and UNSTRATIFIED, with no majority split, in an era with few ceremonial bills.
- **boxsteffensmeier-2018 (weak, supports).**
  - **Finding:** coefficients differ by stage (N = 6,597 bills promoted in Dear Colleague letters). Cosponsor count predicts reporting (0.002) and passage (0.004).
  - **Limits:** pooled and UNSTRATIFIED. It does not argue for stage modelling and gives nothing on timing decay.
- **sotoudeh-2024 (weak).** Lists an LES comparison as future work and does not run it. Shorter half-lives (6 vs 24 months) separate passed from failed bills slightly better. Its outcome differs from the shadow's convergence goal, so it cannot set any decay value.
- **aref-2020 (weak).**
  - **Finding:** passage rates fell per session in both chambers: House -0.11 pp (beta -0.528), Senate -0.35 pp (beta -0.852), over 1979-2016.
  - **Bearing:** supports within-chamber normalisation by analogy only. It is an association.

**Bottom line.** The convergence look against CEL LES, the component's own first empirical question, appears pre-registered but has no result in the evidence. Until it runs, the shadow's federal adaptations are unvalidated.

**Contradictions**
- The RKH conscientiousness criterion fails locally. Ramey et al. find C lowers ceremonial share (-0.39 to -0.58, p<0.01, 104th to 112th Congresses). The local pooled test gives rho of about 0.00 to 0.02 (n = 440, 115th to 118th Congresses). This is not a refutation: the local test is UNSTRATIFIED against a minority-concentrated effect and attenuated by few modern ceremonial bills. It does mean the stated assumption ('more conscientious members sponsor fewer ceremonial bills') has no local support.
- The status on aref-2021 is VERIFIED, but its transfer text says the reproduction stays UNVERIFIED (DE-062). Read the first as the paper's claim being checked and the second as Melange's use of it being unvalidated. They are different objects.
- Paul frames Sotoudeh as a 'contrast' with LES. Sotoudeh only proposes an LES comparison as future work, so no contrast has been measured.
- The Ramey citation year conflicts in the repo: 2016 in PERSONALITY_RECEPTIVITY_LAYER and code, 2019 in big5_methods.md:12. The evidence row uses 2016.

**Gaps**
- Convergence with CEL LES (GAP for the core claim). Needed: run the pre-registered look. Compute Spearman and Kendall between the results-only shadow and CEL lesclassic for the same members and Congresses, stratified by chamber and majority status, with the window and denominator named.
- Cosponsor role credit 3/2/1 and the 180-day timing half-life (GAP). Needed: ablation against CEL LES convergence (sponsor-only vs role-weighted; half-lives of 0, 90, 180 and 365 days), plus a bill-level test of whether early cosponsors predict advancement (the component's second empirical question), with Congress fixed effects and majority strata.
- Letters forced to weight 5 (GAP). Needed: compare against substantive-significance coding (CEL or newspaper-based) on a sample of letter-linked bills.
- The 70/30 advocacy/results composite (GAP). No evidence row bears on it. Needed: an external criterion, such as expert rankings in the style of NCCPPR or Bucchianeri's North Carolina check, against which composite weights can be compared.
- 0.8 per-Congress decay (GAP). Needed: a decay grid scored by convergence with LES or by prediction of next-Congress effectiveness.
- A stratified C criterion re-run (GAP). Needed: a bill-weighted binomial regression on ceremonial share by majority status, extending back to Congresses with more ceremonial bills. This is new analysis, UNVERIFIED until reproduced.

**Paul's position vs the evidence**

Paul highlighted LES as the effectiveness criterion. The evidence backs this well: Bucchianeri gives the structure and validation, Aref 2021 uses CEL LES as outcome, and the CEL data is local. Two corrections apply. First, the LES source is Volden-Wiseman and CEL, not Aref & Neal 2021, which only uses LES. Second, Sotoudeh's 'contrast' with LES is an unrun proposal. Paul also cites 'Ramey et al.' for the ceremonial-share criterion. The paper supports the direction, but the local test is null and unstratified, so the criterion is currently unsupported in Melange's own data.

**Open questions**
- Has the pre-registered convergence look against CEL LES been run? If so, where is the result?
- Should the C criterion be re-specified with a majority split and bill weighting, which would mean a new prereg, or retired?
- Where does the CU-mention significance weight come from upstream (impact record, line 225)? How is 'CU' significance defined relative to CEL's substantive-significance coding?
- Is the 70/30 advocacy/results split a Paul ruling or a placeholder?

**Read first:** [[bucchianeri-2025-legislative-effectiveness-american]], [[ramey-2016-measuring-elite-personality]], [[aref-2021-identifying-hidden-coalitions]], [[boxsteffensmeier-2018-taking-congress-interest]], [[sotoudeh-2024-network-based-measure]], [[aref-2020-detecting-coalitions-optimally]]

## Across components

**1. One decay constant, three places, zero evidence.** The 0.8 per-Congress decay appears in the activity score, the sentinel bill ledger and the effectiveness shadow. No row tests it. The only decay evidence (Sotoudeh) covers 6 to 24 month half-lives on a different outcome. One decay-grid study scored against CEL LES convergence and next-Congress prediction would answer for all three components at once. It should also settle whether the ledger's 'frozen legacy schedule' is the same constant the activity score uses.

**2. Regex measurement is the shared weak point.** The press cap's CU-term-title proxy (0.825 vs 0.0089) and the 19-or-20-category topic regex rely on the same untested assumption, that keyword presence identifies CU content. The stance regex in the same pipeline scored kappa 0.17. One double-coded sample (Porter design) plus an intruder test (Ornstein design) could report precision for both the title proxy and the topic tags.

**3. The CEL LES files are an unused shared criterion.** The 93rd to 118th Congress House and Senate files are local. They can serve both the effectiveness-shadow convergence look and a test of the leadership constants, stratified by majority status. Neither has been run.

**4. Transfer conditions recur.**
- Bucchianeri covers state legislatures.
- Ramey covers the 104th to 112th Congresses, while the local test pools the 115th to 118th.
- Kim & Kunisky is one Congress and descriptive.
- Box-Steffensmeier covers Dear Colleague letter bills only.

None of these is a modern Congress-wide result. Every pooled figure in this system must carry majority strata or an UNSTRATIFIED label.

**5. Wording guards.**
- Community-membership breadth (Kim & Kunisky) and any graph rank are heuristic outputs, never closeness or access (D-025).
- Lobbying-derived communities must not become weights (D-021).
- The billstatus-row removal (press layer) is distinct from the rejected proposal to drop cosponsorship edges (network layer).
- No paper here argues for either rejected SNA proposal, and none bears on reciprocity, which remains not validated (D-104 to D-108).

## Research agenda

1. Run the pre-registered convergence look between the results-only effectiveness shadow and CEL lesclassic for the same members and Congresses, stratified by chamber and majority status, with the window and denominator named.
2. Run a decay-grid study (0.5 to 1.0 per Congress) scored against CEL LES convergence and next-Congress CU-activity prediction. One study answers the activity score, the sentinel ledger and the shadow. First confirm whether the 0.8 actually feeds the 138-column activity score.
3. Ablate the shadow's unvalidated federal adaptations against CEL LES: cosponsor role credit 3/2/1 vs sponsor-only; timing half-lives of 0, 90, 180 and 365 days; letters at weight 5 vs coded significance. Add a bill-level test of whether early cosponsors predict advancement, with Congress fixed effects and majority strata.
4. Re-specify the RKH ceremonial-share C criterion as a bill-weighted, majority-stratified model that extends into Congresses with more ceremonial bills. It needs a new prereg; treat the current pooled null (rho of about 0, n = 440) as UNSTRATIFIED, not as a refutation.
5. Test the leadership constants by regressing CEL LES (or CU-bill advancement) on committee, caucus and party leadership positions by majority status. Compare the implied ordering and spacing with the hand-set 1.0/0.8/0.6/0.4/0.2.
6. Build one double-coded human sample of releases, stratified by party, chamber and category. Report per-category precision, recall and kappa for the topic regex, plus precision of the CU-term-title proxy. Add an intruder test on category labels.
7. Run a stratified audit of pages the press cap skipped (party x chamber): hand-code CU relevance, re-measure coverage from the corpus, and recompute press-pillar ranks under inverse-probability weights to see whether differential coverage moves them.
8. Reconcile the factual conflicts before any write-up: 19 vs 20 categories; whether R CATEGORIES and MR.RAW_PATTERNS are the same map; the source of the coverage figures (86.6%, D/R, House/Senate coverage); 'about 10% CU signal' vs 0.0089; and the Ramey citation year (2016 vs 2019).
9. Reconcile the scraped congress.gov counts against BICAM or govinfo BILLSTATUS sponsor and cosponsor tables for a sample of members to estimate search-term recall.
