---
type: paper
title: "CampaignView, a database of policy platforms and biographical narratives for congressional candidates"
authors: ["Rachel Porter", "Colin R. Case", "Sarah A. Treul"]
year: "2025"
venue: "Scientific Data (Nature Portfolio), 12:1237"
doi: "10.1038/s41597-025-05491-x"
category: "Campaign Politics"
systems: ["melange-influence-engine", "melange-legislative-data", "mentat-fec-attribution"]
components: ["melange-influence-engine.electoral-plays", "melange-legislative-data.category-taxonomy", "mentat-fec-attribution.identity-ladder"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/porter-2025-campaignview-campaign-platforms.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\s41597-025-05491-x.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\adoring-yalow-ad763f\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\agent-a2168cba4e2fdc5a2\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\agent-a5628f8fee4a9c869\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\amazing-lewin-409893\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\codex-websocket-error-468fa9\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\ecc-benchmark-methodology-c4f259\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\evidence-csv-fetch-streaming-8bedff\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\forecasting-model-audit-5603f2\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\melange-math-eval-skill-18e907\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\melange-members-section-fixes-6613db\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\melange-ui-redesign-372c43\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\mentat-github-migration-b06195\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\mentat-systems-production-file-53552c\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\pac-map-downstream-viz-c2bca4\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\sector-growth-political-spending-9cc0c8\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\silly-einstein-57ef31\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\unfinished-work-handoff-a3bbad\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\vibrant-khayyam-65f943\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\.claude\\worktrees\\vibrant-sammet-a549a5\\research\\2025_SciData_Porter_CampaignView.pdf", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Melange Systems\\research\\2025_SciData_Porter_CampaignView.pdf"]
sha12: ["7e7d42901395"]
tags: [paper, campaign-politics, type/paper, theme/campaign-politics, evidence/verified]
---

# CampaignView, a database of policy platforms and biographical narratives for congressional candidates

**Rachel Porter, Colin R. Case, Sarah A. Treul (2025).** *Scientific Data (Nature Portfolio), 12:1237.* [10.1038/s41597-025-05491-x](https://doi.org/10.1038/s41597-025-05491-x). Theme: [[themes/campaign-politics|Campaign Politics]].

> [!abstract] Summary
> Porter, Case and Treul (Scientific Data 2025) release CampaignView, a data descriptor rather than a hypothesis test: campaign-website text for U.S. House major-party primary candidates in 2018, 2020 and 2022. Of 6,016 ballot-eligible candidates, 5,228 (86.9%) are in the corpus, giving 43,465 platform points and 5,114 biographies. Text was scraped about one week before each primary, with Wayback Machine backfill (7.8% of candidates). Every platform point is hand-coded into one of 14 policy topics. On a 20% relabel sample (8,584 points) agreement is 80% and Cohen kappa is 0.84. Rows carry FEC and BioGuide IDs, and candidate names were linked across cycles by embedding-based record linkage. Missingness is not random: across the 6,016 candidates, 11.9% have no website and 25.1% have no platform, and incumbents, unopposed candidates and better-funded candidates are much less likely to be missing (Table 5 logit, associations only). In descriptive platform shares, Democrats emphasize environment, healthcare and social welfare, while Republicans emphasize immigration and government operations. For Paul's systems the paper has no network, lobbying or allocation content. It matters in two places. First, the electoral-plays contest-primary route scores an Opponent's primary vulnerability against the same-party average CU score, not against any real challenger. CampaignView gives candidate-level challenger platforms that could replace that proxy for House seats in 2018-2022. Second, its hand-coded topic labels with reported reliability are a template for the open question of how precise the press-release regex topic tagger is. Coverage limits: House only, three cycles, website text only. The topic codes do not map to credit-union categories.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** No hypothesis is tested. The paper asks how to supply systematic, real-time text of congressional candidate campaign websites (platforms and biographies) for 2018 to 2022 House primaries, so that questions on polarization, agenda-setting and representation can be studied (Abstract p.1; Background p.2).
**Theory.** Not tested. Motivation cites candidate self-presentation and position-taking (refs 4-6), issue emphasis shaping voter perceptions and media attention (refs 7-9), campaign priorities predicting later legislative priorities (refs 10-12), and primaries as the pivotal competitive stage (refs 13-18), p.1-2.
**Data.** Population: 6,016 major-party, ballot-eligible House primary candidates, 2018-2022. Corpus: 5,228 candidates (86.9%); 43,465 platform points (candidate-year-point); 5,114 biographies (candidate-year). Collection: manual scrape of campaign sites one week before each primary, plus Wayback Machine for 1,111 candidates (archived text is 7.8%). Auxiliary: party, incumbency, vote shares, FEC ID, BioGuideID, fundraising. Release: Harvard Dataverse doi 10.7910/DVN/JIEAPT (CC0).
**Method.** Descriptive data construction. Name standardization by probabilistic record linkage on pretrained word embeddings, blocked on party and state; matches under 50% probability hand-checked (p.8-9). Every platform point hand-labeled into one of 14 Major Policy Topic codes by trained annotators with a codebook (p.3). Topic term lists from keyness analysis (Table 2, p.7). Missingness described by logistic regression of website, biography and platform absence on covariates (Table 5, p.9). Relabeling check on a 20% random sample (8,584 points) by other coders (p.10).
**Identification.** None. Table 5 missingness coefficients are associations only. No causal design.
**Scope.** U.S. House major-party, ballot-eligible primary candidates, 2018 to 2022 cycles. Population of campaign websites and platforms only, not all candidate communications. Era: 2018-2022 redistricting decade. Partisan dynamics are reported for platforms only (Fig. 3, p.10).

## Key findings

- {'finding': 'Corpus covers 5,228 candidates, 86.9% of 6,016 eligible House primary candidates, 2018-2022; 43,465 platform points and 5,114 biographies.', 'location': 'Abstract p.1; Methods p.4'}
- {'finding': 'Campaign website identified for 5,300 candidates (88.1%); 85.0% of those had a platform and 96.5% a biography.', 'location': 'Methods p.4'}
- {'finding': 'Missingness: no biography for 14.99%, no platform for 25.08%, no website at all for 11.9% of the 6,016 candidates.', 'location': 'Technical Validation p.8'}
- {'finding': 'Archived Wayback data supplies 7.8% of candidates; 239 in 2018, 101 in 2020, 67 in 2022. 1,111 candidates (18.5%) had no primary-season data.', 'location': 'Methods p.4'}
- {'finding': 'Unopposed primary is negatively associated with missing website, biography and platform (-0.342, -0.349, -0.320; all significant at 95%).', 'location': 'Table 5 p.9'}
- {'finding': 'Current incumbent MC has the largest negative association with missing website (-2.967), biography (-2.000) and platform (-0.314). Logged fundraising is negative for all three (-0.222, -0.194, -0.136).', 'location': 'Table 5 p.9'}
- {'finding': 'Three-quarters of candidates without a website raised under 100,000 dollars. Website-plus-platform present for 88.3% of general election winners and biography for 99.0%.', 'location': 'Technical Validation p.8'}
- {'finding': 'Hand-coding reliability on a 20% relabel (8,584 points): percent agreement 80%, Cohen kappa 0.84, weighted kappa 0.74. Most disagreements were on Unknown/Other.', 'location': 'Technical Validation p.10'}
- {'finding': 'Name linkage across cycles: 15 wrong matches for 2018-2020, 1 for 2020-2022, none for 2018-2022.', 'location': 'Technical Validation p.9'}
- {'finding': 'Democrats consistently emphasize Energy and Environment, Healthcare and Social Welfare; Republicans emphasize Immigration and Government Operations more often.', 'location': 'Usage Notes p.10, Fig. 3'}
- {'finding': 'Education partisan gap about 25 points before 2022, narrowing to about 5 points in 2022. Healthcare gap 23 points in 2020 and over 40 points in 2022 (driven by fewer Republicans discussing it).', 'location': 'Usage Notes p.10'}
- {'finding': 'Platform text is cleaned and parsed to the policy level, so one row is one platform point (statement_id), aggregable to candidate-year.', 'location': 'Data Records p.5, p.10'}

## Limitations

- A
- b
- o
- u
- t
-  
- 1
- 2
- %
-  
- o
- f
-  
- e
- l
- i
- g
- i
- b
- l
- e
-  
- c
- a
- n
- d
- i
- d
- a
- t
- e
- s
-  
- h
- a
- v
- e
-  
- n
- o
-  
- w
- e
- b
- s
- i
- t
- e
-  
- a
- n
- d
-  
- 2
- 5
- %
-  
- h
- a
- v
- e
-  
- n
- o
-  
- p
- l
- a
- t
- f
- o
- r
- m
- ,
-  
- w
- i
- t
- h
-  
- m
- i
- s
- s
- i
- n
- g
- n
- e
- s
- s
-  
- c
- o
- n
- c
- e
- n
- t
- r
- a
- t
- e
- d
-  
- a
- m
- o
- n
- g
-  
- u
- n
- c
- o
- m
- p
- e
- t
- i
- t
- i
- v
- e
-  
- a
- n
- d
-  
- l
- o
- w
- -
- f
- u
- n
- d
- r
- a
- i
- s
- i
- n
- g
-  
- c
- a
- n
- d
- i
- d
- a
- t
- e
- s
- .
-  
- E
- a
- c
- h
-  
- p
- l
- a
- t
- f
- o
- r
- m
-  
- p
- o
- i
- n
- t
-  
- g
- e
- t
- s
-  
- o
- n
- e
-  
- t
- o
- p
- i
- c
-  
- (
- m
- a
- j
- o
- r
- i
- t
- y
-  
- c
- o
- n
- t
- e
- n
- t
- )
- ,
-  
- s
- o
-  
- m
- u
- l
- t
- i
- -
- t
- o
- p
- i
- c
-  
- i
- s
- s
- u
- e
- s
-  
- s
- u
- c
- h
-  
- a
- s
-  
- w
- o
- m
- e
- n
-  
- r
- i
- g
- h
- t
- s
-  
- o
- r
-  
- r
- e
- p
- r
- o
- d
- u
- c
- t
- i
- v
- e
-  
- h
- e
- a
- l
- t
- h
-  
- m
- a
- y
-  
- b
- e
-  
- u
- n
- d
- e
- r
- -
- c
- o
- d
- e
- d
-  
- (
- p
- .
- 1
- 0
- )
- .
-  
- T
- o
- p
- i
- c
-  
- c
- o
- d
- e
- s
-  
- a
- r
- e
-  
- o
- n
- l
- y
-  
- p
- a
- r
- t
- l
- y
-  
- a
- l
- i
- g
- n
- e
- d
-  
- t
- o
-  
- P
- A
- P
-  
- a
- n
- d
-  
- C
- R
- S
-  
- s
- c
- h
- e
- m
- e
- s
-  
- (
- T
- a
- b
- l
- e
-  
- 6
- )
- .
-  
- R
- e
- l
- i
- a
- b
- i
- l
- i
- t
- y
-  
- c
- h
- e
- c
- k
-  
- c
- o
- v
- e
- r
- s
-  
- o
- n
- l
- y
-  
- t
- h
- e
-  
- 2
- 0
- 2
- 2
-  
- r
- e
- l
- a
- b
- e
- l
-  
- s
- a
- m
- p
- l
- e
- .
-  
- R
- e
- d
- i
- s
- t
- r
- i
- c
- t
- i
- n
- g
-  
- c
- h
- a
- n
- g
- e
- s
-  
- d
- i
- s
- t
- r
- i
- c
- t
-  
- I
- D
- s
- ,
-  
- s
- o
-  
- c
- d
-  
- i
- s
-  
- n
- o
- t
-  
- a
-  
- r
- e
- c
- o
- m
- m
- e
- n
- d
- e
- d
-  
- k
- e
- y
- .
-  
- W
- e
- b
- s
- i
- t
- e
-  
- t
- e
- x
- t
-  
- i
- s
-  
- a
-  
- s
- n
- a
- p
- s
- h
- o
- t
-  
- o
- n
- e
-  
- w
- e
- e
- k
-  
- b
- e
- f
- o
- r
- e
-  
- p
- r
- i
- m
- a
- r
- y
- .

## Short quotes

- "Our corpus covers 5,228 candidates, representing 86.9% of major-party, ballot-eligible contenders who ran for the U.S. House"
- "Currently, there is no open-source database of text from congressional candidate campaign websites."
- "Percent agreement between re-labeled documents and their original topical coding was 80%."

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Electoral security, leverage and contest-defend plays|Electoral security, leverage and contest-defend plays]] | suggests-extension | Platform text for 5,228 of 6,016 (86.9%) major-party ballot-eligible House primary candidates 2018-2022, with 43,465 hand-topic-coded platform points. Identifiers are FECCandID (not universal) and BioGuideID (incumbents only). (Abstract p.1; Methods p.4; Data Records p.5; Usage Notes p.9-10 (ID caveats)) | build_electoral_plays.py: the contest route primary term takes, per category, the Opponent's shortfall against the same-party average avgP, keeps only the negative parts, and divides the sum by the number of categories. So it is a mean-style proxy, not a plain sum. No challenger data enters. `fec/engine/build_electoral_plays.py:26-31` | House only, 2018-2022 cycles; the 14 topic codes are not CU categories. BioGuideID is incumbent-only and the paper advises against aggregating by either ID. Challenger coverage gaps (low fundraising, uncompetitive races) are where the proxy matters most. New analysis stays UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED |
| [[systems/melange-legislative-data#Release category taxonomy and billstatus synthetic rows|Release category taxonomy and billstatus synthetic rows]] | method-source | Each platform point is hand-coded by trained annotators to one of 14 Major Policy Topic codes using a codebook. A 20% relabel (8,584 points, by 2022 coders) gave 80% agreement, Cohen kappa 0.84 and weighted kappa 0.74, with most disagreement on Unknown/Other. (Methods p.3; Technical Validation p.10) | cu_press_release_scraper.R defines 19 Layer-2 categories by regex term lists plus bill-based tags. detect_categories tags a release with EVERY category it touches, so the tagger is multi-label, not single-topic. A human-label kappa exists for stance (run_stage5.R line 133: regex 0.17, DEBATE 0.34). No topic-level reliability benchmark was found. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R:1730-1739 and 1921-1927; run_stage5.R:133` | Transfers as a validation design (double-code a random sample, report kappa), not as labels: genre and topic scheme differ from CU categories. The paper's single-topic rule under-codes multi-topic text, but the regex tagger does not share that rule. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-fec-attribution#Contributor identity match ladder|Contributor identity match ladder]] | context | Cross-cycle candidate names linked by probabilistic record linkage on pretrained embeddings, blocked on party and state. Estimated matches under 50% were hand-checked: 15 wrong for 2018-2020, 1 for 2020-2022, none for 2018-2022. (Methods p.8-9; Technical Validation p.9) | member_identity.py Resolver (about lines 118-135) maps a bioguide ID to a roster name and returns None as a refusal rather than guessing. The cited lines contain no contributor rule ladder (name parts, zip, city, occupation, employer) and no name-frequency logic, and a keyword search of member_identity.py found no occupation or employer rules. `fec/members/member_identity.py:118-135` | The paper links candidates (a small, ID-rich population), not donors; its error counts do not speak to donor-level false-match rates. Member bioguide lookup is a different problem from cross-cycle name linkage. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Coverage: all 3 bearings and all 3 related links were checked against the paper text and code. Paper figures in the summary (5,228/6,016, 86.9%, 43,465, 8,584, 80%, kappa 0.84, 0.74, 15/1/0 wrong matches, 11.9%, 25.1%, one-week scrape, 7.8% Wayback) reproduce.
> - Summary issue: Keying: the summary says platform text is keyed by FEC ID and BioGuideID. The paper says BioGuideID exists only for incumbents, FECCandID is missing for some, and aggregation by either is not recommended.
> - Summary issue: Relabel scope: the summary omits that the 20% relabel was done only on 2022 primary text by coders from that cycle. The reliability statistics therefore cover one cycle only.
> - Summary issue: Missingness direction: the summary's claim that unopposed and better-funded candidates are less likely to be missing matches Table 5 signs. But the paper's prose says contested-primary candidates are less likely to be missing, which contradicts its own table. The summary should flag this internal inconsistency.
> - Summary issue: Missingness pooling: Table 5 coefficients are pooled across parties and cycles. The summary should quote them as UNSTRATIFIED associations.
> - Summary issue: Contest route: the summary says the proxy is a sum of negative gaps against the same-party average. The code divides by the number of categories, so it is a mean-style shortfall. The summary also omits the general (cross-party) route, which takes the max of the two.
> - Summary issue: Tagger: the summary says the single-topic-per-item caveat also applies to the regex press-release tagger, and that no human benchmark exists. Both are refuted or overstated by the Draft Systems code (multi-label tagging; stance kappa in run_stage5.R line 133).
> - Summary issue: Scrape timing: the summary says text was scraped about one week before each primary. The paper also runs searches one month before and after the season, so the window is wider than one week.

## Gaps and flags

- electoral-plays primary route uses the same-party average as a stand-in challenger; no actual primary-challenger data (positions, fundraising, quality) enters the contest value.
- No human-coded reliability benchmark (kappa or precision) exists for the regex press-release topic tagger; CampaignView shows the minimal design (20% double-code).
- Systems have no campaign-stage issue-emphasis input; whether a member's campaign platform emphasis predicts later CU-relevant press or legislative priority is untested.
- No conflict with brain rules: paper makes no network, closeness, influence or LDA claims.
- Table 5 missingness coefficients are pooled across parties and cycles; quote as UNSTRATIFIED associations if cited.
- Item kind corrected upstream to data descriptor; abstract says 5,228 corpus candidates while Methods says 5,300 had a website; both figures use denominator 6,016.

No annotated-bibliography entry and no markup from Paul for this work, so there is no takeaway to check. Nothing is stale.

## Related papers

- [[bonica-2013-mapping-ideological-marketplace]] — complements — Vault summary confirms Bonica's CFscores are donor-based candidate ideal points. The pairing with CampaignView platform text is the analyst's suggestion; the paper does not cite Bonica.
- [[broockman-2026-should-moving-middle]] — complements — Vault title and summary confirm a study of when moving to the middle wins votes. CampaignView is a candidate-level source of primary positions, as the analyst says; the paper does not cite Broockman.
- [[ornstein-2024-train-your-stochastic]] — complements — Vault file confirms it is about LLMs for political texts (Political Science Research and Methods, 2025). The analyst's use as a scalable check on CampaignView's hand codes is a suggestion, not a claim in either paper.

### Linked from
_None._