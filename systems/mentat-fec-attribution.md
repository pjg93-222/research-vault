---
type: system
name: Mentat FEC money attribution and accounting
slug: mentat-fec-attribution
components: [mentat-fec-attribution.employer-match, mentat-fec-attribution.identity-ladder, mentat-fec-attribution.receipt-role-and-exclusion, mentat-fec-attribution.dedup-and-versions, mentat-fec-attribution.cash-proportional-attribution, mentat-fec-attribution.transfer-graph-and-direction, mentat-fec-attribution.collection-completeness]
brain_notes: [C:/Users/pgoff/Documents/brain/actors/mentat-fec-pipeline.md, C:/Users/pgoff/Documents/brain/concepts/mentat-fec-accounting-methodology.md, C:/Users/pgoff/Documents/brain/concepts/exclude-on-contradiction-not-ambiguity.md, C:/Users/pgoff/Documents/brain/concepts/pac-cash-proportional-attribution.md, C:/Users/pgoff/Documents/brain/actors/mentat-fec-pipeline-history-audit-r12.md]
paper_count: 12
tags: [system]
---

# Mentat FEC money attribution and accounting

The R/Python pipeline (C:/Users/pgoff/Mentat) that attributes FEC Schedule A, Schedule B and PAS2 money to roster companies and industries. Covers employer and identity matching, receipt-role classification, contradiction-only exclusion, conduit and memo dedup, filing-version identity, the PAC transfer graph, cash-proportional attribution, beneficiary direction, geography and collection completeness.

Brain notes: [actors/mentat-fec-pipeline](obsidian://open?vault=brain&file=actors%2Fmentat-fec-pipeline); [concepts/mentat-fec-accounting-methodology](obsidian://open?vault=brain&file=concepts%2Fmentat-fec-accounting-methodology); [concepts/exclude-on-contradiction-not-ambiguity](obsidian://open?vault=brain&file=concepts%2Fexclude-on-contradiction-not-ambiguity); [concepts/pac-cash-proportional-attribution](obsidian://open?vault=brain&file=concepts%2Fpac-cash-proportional-attribution); [actors/mentat-fec-pipeline-history-audit-r12](obsidian://open?vault=brain&file=actors%2Fmentat-fec-pipeline-history-audit-r12)

**Evidence base.** There are 17 paper bearings (evidence rows) from 12 distinct papers across 7 components. By status: 16 VERIFIED-SOFTENED, 1 VERIFIED. By strength: 9 moderate, 8 weak. **No bearing gives a precision, recall, error-rate or bias benchmark that transfers to Mentat.** The papers act as design references, scope statements and cautions. None validates the pipeline.

**Coverage is lopsided:**
- **Employer-string roster matching and overrides:** 3 bearings (3 distinct papers).
- **Contributor identity match ladder:** 5 bearings (5 distinct papers).
- **Receipt-role classification and contradiction-only exclusion:** 1 bearings (1 distinct papers).
- **Conduit legs, memo dedup, filing-version identity and transfer rule:** 1 bearings (1 distinct papers).
- **PAC cash proportional attribution:** 0 bearings (0 distinct papers) (GAP).
- **PAC transfer graph and beneficiary direction partition:** 7 bearings (6 distinct papers).
- **Collection completeness (zero provenance, count reconciliation, key pool):** 0 bearings (0 distinct papers) (GAP).

The components that carry the most accounting weight (version identity, cash attribution, completeness) are the ones with the least or no literature. They rest only on internal invariants: cent-level conservation, count reconciliation and the cash gate. Their validation has to come from internal audits against FEC bulk files and hand-coded samples, not from citations.

**Headline contradictions:**
1. **The 89% oppose figure.** The component record says "89% of beneficiary dollars were oppose". Three bearings could not find that figure in fec_transfer_graph.py, report.R or the brain folder searched. The only trace is a comment at R/export_guards.R:278 saying 89% of **one shipped beneficiary sheet** was **opposition-direction rows**. That is rows, not dollars, and one sheet, not the beneficiary total. Until it is recomputed with a named cycle window and dollar denominator, the figure should not be quoted as a dollar share.
2. **The Banking exclusion audit is really an employer-match failure.** $29.3M of the $32.5M excluded (about 90%, Banking run, 29,816 rows) came from one employer-key prefix bug (CITIZENS read as CITI).
3. **Kim et al. citation years.** The paper is cited as 2020 in Paul's positions and as JOP 2026 in the system's citation list. Evidence exists for two separate slugs, a 2020 paper and a 2023 paper. The SOC 27-3030 government-relations code appears in the 2020 extract but is refuted for the 2023 paper text.

Paul's stated positions favor fail-closed, false-positive-limiting matching and treat late or duplicate filings as hazards. The evidence agrees in direction with both, but it does not measure either.

## Employer-string roster matching and overrides

**What it computes.** Assigns Schedule A rows to roster companies by normalized employer key, prefix tokens and context; override rules use maximum-specificity matching.

**Method.** Strict-names rule (roster plus verified aliases only). Acronym collisions are open owner calls.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/collect.R:363-1080`; `C:/Users/pgoff/Mentat/mentat/R/ingest.R:43-205`

**Assumptions.**
- Employer text identifies the employer
- Unlisted spellings are unmatched

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[furnas-nd-conflating-lobbying-pacs]] |  | measurement-source | Organizational giving is campaign donations from the organization or its officially affiliated PAC; contributions by employees and contract lobbyists are not counted. Dark-money contributions are not observed. Independent expenditures (often by Super PACs) are not discussed; the article covers contributions to candidates only. | Comparing Mentat industry totals or participation counts with the paper's overlap or distribution figures requires restricting Mentat to organizational and affiliated-PAC receipts first. Melange's opposition prior already records that employee/executive giving and industry PAC money (congestion.json) are different instruments, with lean signs agreeing for only 157/276 members (57%) (fec/lobbying/build_opposition_prior.py:113). | moderate | VERIFIED-SOFTENED; system side: Mentat attributes FEC receipts to roster companies by normalized key, using contributor_employer for individual contributions and contributor_name otherwise, with legal-suffix and bank N.A. stripping on employer keys, so its company giving includes individual employee giving the paper excludes. |
| [[anon-2025-cross-industry-political]] | 2025 | suggests-extension | Charles Schwab's company PAC and executives halted giving. Per the memo, Chairman Schwab and his wife moved giving to a charity fund that invested over $250M in conservative causes in 2021-2024 and gave to Republican Super PACs (memo figures, partly redacted). The memo's thesis is that spending is offloaded onto company executives, measured as CEO and executive averages per industry. | An extension would link executive-controlled vehicles (foundations, donor-advised funds, family entities) to roster people. That needs a verified-affiliation rule consistent with the strict-names policy, not a fuzzy guess. The memo never defines executive, so its averages cannot be reconciled with the Chief or Middle Management tiers. | weak | VERIFIED-SOFTENED; system side: collect.R company_cross_reference_people (lines 1009-1035) builds the trusted name set from roster executives and from contributions whose match reason is verified_employer_alias or verified_roster_alias. report.R spending_title_role (line 2528 onward) assigns Chief to CEO, CFO, general counsel, founder, owner, chairman and president titles, and Middle Management to vice president, director, partner, head and manager titles. The 501c category is supplementary at report.R line 32. |
| [[goff-nd-cross-industry-finance]] |  | suggests-extension | The C-suite share of crypto donations rose from 69 percent in 2021-22 to 98.8 percent in 2023-24; the report frames the credit union shortfall as missing employee and middle-management giving. | An occupation-role split would need a hand-coded precision check. The report gives no coding rule for C-suite, so its 69 and 98.8 percent figures cannot be reproduced from the extract. | moderate | VERIFIED; system side: collect.R assigns Schedule A rows to roster companies by employer key (company_employer_keys, line 363). Occupation text enters only as an optional scoped regex on override rules (lines 520-529), keeping the most specific match. No C-suite or employee role taxonomy was found in that section. |

**Assessment.**

**Evidence weight: thin, and none of it measures matching accuracy.** Three bearings apply:

- **goff-nd (moderate, VERIFIED).** Paul's own Cross-Industry report says the C-suite share of crypto donations rose from 69% (2021-22) to 98.8% (2023-24). The report gives no coding rule for C-suite, so neither figure can be reproduced. Mentat has no occupation-role taxonomy in collect.R's employer section; occupation enters only as an optional scoped regex on override rules (lines 520-529). This is a suggested extension, not validation, and the figures must not be reused until they are reproduced with a stated rule and denominator.
- **furnas-nd (moderate, VERIFIED-SOFTENED).** This paper is a measurement-scope statement. Organizational giving there means the organization or its affiliated PAC only, excluding employee and contract-lobbyist giving. Mentat's company totals include employee giving matched by contributor_employer, so any comparison with Furnas-type overlap figures first needs Mentat restricted to org and affiliated-PAC receipts.
- **anon-2025 memo (weak).** The memo says executives route giving through charity funds and Super PACs (the Schwab example). If true, it marks a scope boundary of strict employer matching. Executive-controlled foundations, donor-advised funds and family entities are invisible to it. Closing that boundary needs a verified-affiliation rule, not fuzzy matching.

**Internal evidence that matters more than the papers:** the Banking exclusion run lost $29.3M of $32.5M (29,816 rows) to one prefix collision (CITIZENS read as CITI). That is a direct observation that prefix-token matching produces false positives at scale, concentrated in name families like CITI/CITIZENS. The literature offers no comparable error rate.

**GAP:** precision and recall of employer-string matching against a hand-coded sample remain unmeasured. The study to fill it is a stratified, hand-coded audit of Schedule A rows, stratified by company size, name ambiguity (acronyms, prefix families, common words) and cycle. It should report precision and recall with their window and denominator. Kim, Stuckatz & Wolters (gvkey and hand-coded ground truth) is the design template, but that paper reports no numbers to benchmark against.

**Contradictions.**
- furnas-nd defines company giving as organization plus affiliated-PAC giving only; Mentat company totals include employee giving matched by employer string. These are two different quantities, so any cross-comparison is invalid unless Mentat is first restricted.
- goff-nd (Paul's own report) reports C-suite shares of 69% and 98.8%, but Mentat has no C-suite taxonomy and the report gives no coding rule, so the system cannot reproduce its own report's figures.
- Melange's opposition prior records that employee/executive lean and industry-PAC lean agree in sign for only 157 of 276 members (57%). That is a reminder the two instruments diverge. The window for this figure is not stated in the bearing and must be added before it is quoted.

**Gaps.**
- No study measures precision or recall of employer-string matching for FEC data, for Mentat or any comparable pipeline; Kim et al. and Bonica describe hand-coded checks but report no numbers in the extracts.
- No evidence on the scale of executive giving routed through foundations, donor-advised funds or family entities that strict matching cannot see.
- No published rule for an occupation/role (C-suite vs employee) taxonomy that could be checked against hand coding.
- Huneeus & Kim 2018, cited in Paul's position, has no evidence row.

**Paul's position vs the evidence.** Paul's strict-names rule (roster plus verified aliases) is more conservative than the Kim et al. gvkey/SOC/clustering design he highlighted. Nothing in the evidence argues against conservatism, and furnas-nd and the anon memo both point to scope limits rather than precision problems. The evidence does not show that strict matching is accurate; the CITIZENS/CITI bug shows the prefix-token layer is not as strict as the policy. The position Paul highlighted, hand-coded ground truth, is exactly the missing study.

**Open questions.**
- What precision and recall does the employer key plus prefix-token matcher achieve on a hand-coded sample, stratified by company size and name-collision family (CITI/CITIZENS type)?
- Should prefix-token matching be gated by a collision list before rows reach the exclusion stage, given that one prefix bug accounted for about 90% of excluded Banking dollars?
- Is an occupation-role tier (Chief / Middle Management, already in report.R spending_title_role) reproducible enough on hand coding to support C-suite share figures like goff-nd's?
- Should Mentat publish an org-plus-affiliated-PAC-only view so totals can be compared with Furnas-type measures?

**Read first.** [[kim-2020-strategic-sequential-links]], [[furnas-nd-conflating-lobbying-pacs]], [[goff-nd-cross-industry-finance]], [[anon-2025-cross-industry-political]], [[bonica-2013-mapping-ideological-marketplace]]

## Contributor identity match ladder

**What it computes.** AUTO, HIGH, REVIEW or DISCARD for same-name candidates by name parts, zip, city, state, occupation and employer contradiction.

**Method.** Ordered rules (exact name + zip -> AUTO, etc.). No name-frequency weighting.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/collect.R:1933-2157`

**Assumptions.**
- Same name + zip implies the same person

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bonica-2025-money-campaign-finance]] | 2025 | method-source | The DIME-to-L2 donor linkage is built to limit false positives. It links 75% of DIME individual contributors from 2012-2020, about 17M donors. Misses come from address differences (most common), donors who are not registered, and name differences. Unlinked donors have slightly more conservative CFscores and larger average gifts. The authors judge these imbalances small and not theoretically important. The r = 0.95 validation against Project Vote Smart (fn. 11) is for the CANDIDATE birth-year linkage, not the donor linkage. | Both pipelines prefer precision to recall, which is consistent with Mentat's fail-closed posture. The paper reports that unlinked donors give slightly larger amounts, and the authors call this unimportant. It is weak evidence at most that precision-first matching under-recovers larger donors, and it does not establish a downward dollar bias in Mentat. Mentat matches contributor rows to one another within an employer roster, which is a different task from voter-file linkage. Neither 75% nor r = 0.95 is a benchmark for Mentat. | weak | VERIFIED-SOFTENED; system side: A rule ladder with no name-frequency weighting. Exact name and same zip gives AUTO, or HIGH when the employer contradicts. Exact name and same city, or nickname and same zip, gives HIGH. Suffix mismatches and weaker combinations go to REVIEW, and everything else is DISCARD (below_identity_threshold). Only AUTO and HIGH are kept (collect.R:2896). |
| [[kim-2020-strategic-sequential-links]] | 2020 | method-source | FEC has no unique donor IDs (about 53M itemized individual contributions 1980-2018, 4M employer names, about 827,000 occupation names). Authors match PAC and employer names to Compustat gvkey and occupations to SOC codes (executives 11-1011, government relations 27-3030), standardize names with NLP as in Bonica (2017), and run unsupervised constrained graph clustering on address, donation history and targets; employer and occupation matches are cross-validated against a hand-coded ground truth and corporate PAC matches checked and revised manually. | Both work on FEC Schedule A, so the method transfers in kind. The paper reports no precision or recall, so it is a design reference, not a benchmark. Adopting clustering would loosen Mentat's rule-based ladder, and receipt-counting is frozen without per-change OK; any change is Paul's. The transferable piece is a hand-coded labelled sample to measure the ladder's precision and recall. | moderate | VERIFIED-SOFTENED; system side: identity_match_ladder in Mentat collect.R: AUTO (exact name + same zip), HIGH (exact name + same zip cross-employer, exact name + same city, nickname + same zip), REVIEW (suffix mismatch, nickname + same city, exact name + state + similar occupation, employer contradiction), else DISCARD. No name-frequency weighting in this function. Employer agreement is tested against company_variants. |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | measurement-source | Supplement record-linkage section (text lines about 5315-5350, approx PDF p.51): on corporate board-member cases, 57 percent got a single ID, 42 percent split (type 1 errors), and in 13 percent of the split cases the algorithm failed to put 90 percent of records under one ID. Two type 2 errors (wrong merges), both family members at the same employer (Gates Sr and Jr; Walton family). Address and occupation disclosure was significantly lower in the 1980s and early 1990s. Method is fuzzy matching plus supervised machine learning. | The paper's type 2 errors were same-employer family members (the Gates Jr and Sr pair differs by a suffix). Mentat sends any suffix difference to REVIEW, so a missing-suffix pair does not get AUTO under the current rule. The split rates come from a different linker and are a reference point, not an error-rate estimate for Mentat. | moderate | VERIFIED-SOFTENED; system side: identity_match_ladder: suffix_mismatch (any suffix difference, including a suffix present on one record only) returns REVIEW first. Exact name plus same zip returns AUTO unless the employer contradicts the company, which gives HIGH. No name-frequency weighting. |
| [[kim-2023-systemic-sequential-links]] | 2023 | method-source | Donor and firm linkage uses constrained graph clustering and fastLink probabilistic record linkage (Enamorado, Fifield and Imai 2019). Executives are classed by SOC 11-1011. The paper reports no precision or recall for its linkage. | The paper confirms the fastLink method and the 11-1011 code only. SOC 27-3030 for government-relations staff is not in the paper text, so that part of the claim is refuted. Suggesting that frequency-weighted fastLink would fix the common-name blind spot is the analyst's opinion, and the paper gives no benchmark. Any change to the ladder is a ruling for Paul under D-040 (not checked here). | weak | VERIFIED-SOFTENED; system side: C:/Users/pgoff/Mentat/mentat/R/collect.R lines 2110-2156 implement an ordered rule ladder: exact name and same zip gives AUTO, with HIGH if the employer contradicts; exact name and same city gives HIGH; nickname and same zip gives HIGH; nickname and same city gives REVIEW; exact name, same state and similar occupation gives REVIEW; employer contradiction with a name match gives REVIEW; otherwise DISCARD. No name-frequency weighting appears in this ladder. |
| [[porter-2025-campaignview-campaign-platforms]] | 2025 | context | Cross-cycle candidate names linked by probabilistic record linkage on pretrained embeddings, blocked on party and state. Estimated matches under 50% were hand-checked: 15 wrong for 2018-2020, 1 for 2020-2022, none for 2018-2022. | The paper links candidates (a small, ID-rich population), not donors; its error counts do not speak to donor-level false-match rates. Member bioguide lookup is a different problem from cross-cycle name linkage. | weak | VERIFIED-SOFTENED; system side: member_identity.py Resolver (about lines 118-135) maps a bioguide ID to a roster name and returns None as a refusal rather than guessing. The cited lines contain no contributor rule ladder (name parts, zip, city, occupation, employer) and no name-frequency logic, and a keyword search of member_identity.py found no occupation or employer rules. |

**Assessment.**

**Evidence weight: moderate design support, no benchmark.** This component has the most method references: five bearings, all VERIFIED-SOFTENED, two moderate and three weak.

- **kim-2020 (moderate).** This paper confirms the core problem Paul highlighted: FEC has no unique donor IDs (about 53M itemized individual contributions, 1980-2018). Its answer is NLP name standardization plus constrained graph clustering on address, history and targets, with hand-coded cross-validation. It reports no precision or recall, so it is a design reference only.
- **kim-2023 (weak).** This paper uses fastLink, which is frequency-weighted probabilistic linkage, and also reports no linkage accuracy. The suggestion that fastLink would fix common-name false merges is an analyst opinion that the paper does not demonstrate.
- **bonica-2013 (moderate).** This is the only bearing with actual error counts. On corporate board-member cases, 57% got a single ID and 42% were split. There were two wrong merges, both same-employer family members (Gates Sr/Jr, Walton family). Mentat sends any suffix difference to REVIEW, so a Sr/Jr pair with both suffixes recorded would not be auto-merged. The residual risk is a same-name, same-zip family pair with **no** suffix on either record, which the current AUTO rule would merge. Bonica's linker is different, so the split rates are not Mentat error rates.
- **bonica-2025 (weak).** The DIME-to-L2 linkage was tuned to limit false positives and linked 75% of donors (2012-2020, about 17M). Unlinked donors gave slightly larger amounts, which is at most weak evidence that precision-first linkage under-recovers bigger donors. The r = 0.95 validation concerns candidate birth-year linkage and does not apply here.
- **porter-2025 (weak, context).** This is candidate linkage, not donor linkage, and is irrelevant to donor false-match rates.

**Net:** the literature agrees on precision-first linkage and on frequency weighting as the standard probabilistic upgrade. **No source gives a false-merge or false-split rate that transfers.** The ladder's real accuracy is unmeasured.

**GAP to fill:** a hand-labelled sample of same-name candidate pairs drawn from Mentat's own roster matches. It should be stratified by name frequency (for example surname frequency from the Census surname file), zip and suffix presence. The ladder and a fastLink-style frequency-weighted linker should both be scored on the same labels and reported as precision and recall per tier (AUTO, HIGH, REVIEW). Any change to the ladder is Paul's ruling, and receipt-counting is frozen without per-change OK.

**Contradictions.**
- Kim et al. citation year: Paul's position cites Kim, Stuckatz & Wolters 2020, the citation list cites JOP 2026, and the evidence holds separate 2020 and 2023 slugs. The SOC 27-3030 government-relations code appears in the 2020 extract and is refuted for the 2023 paper text.
- bonica-2025's r = 0.95 Project Vote Smart validation is sometimes read as donor-linkage accuracy, but it is candidate birth-year linkage. It must not be cited as donor-linkage validation.

**Gaps.**
- No transferable false-merge or false-split rate for rule-based FEC donor matching, by tier.
- No head-to-head study of a rule ladder versus frequency-weighted probabilistic linkage (fastLink) on FEC donors.
- No evidence on common-name false-merge rates under exact name plus zip (the AUTO tier) when suffixes are missing on both records.

**Paul's position vs the evidence.** Paul highlighted 'no unique identifiers', constrained graph clustering and algorithms that limit false positives. The evidence confirms the no-ID problem (kim-2020) and that the leading pipelines are precision-first (bonica-2025, kim-2023), which is consistent with Mentat keeping only AUTO and HIGH. No evidence shows that clustering or fastLink outperforms the ladder on this task, since neither Kim paper reports linkage accuracy. Paul's preference for limiting false positives is supported in direction but not quantified. The ladder's lack of name-frequency weighting is its main untested exposure.

**Open questions.**
- What are AUTO-tier and HIGH-tier precision on a hand-labelled sample, stratified by surname frequency?
- How many AUTO merges involve exact name plus zip with no suffix on either record (the Bonica family-merge pattern)?
- Would adding name-frequency weighting only as a demotion rule (AUTO to REVIEW for very common names) keep the fail-closed posture? This is Paul's call.
- Do REVIEW and DISCARD rows skew toward larger donors, as Bonica 2025's unlinked-donor finding weakly suggests?

**Read first.** [[kim-2020-strategic-sequential-links]], [[bonica-2013-mapping-ideological-marketplace]], [[kim-2023-systemic-sequential-links]], [[bonica-2025-money-campaign-finance]], [[porter-2025-campaignview-campaign-platforms]]

## Receipt-role classification and contradiction-only exclusion

**What it computes.** Economic role per receipt from form, line and description. A row is excluded only when its own text names another organisation; ambiguous rows are kept with a flag.

**Method.** The Banking run excluded 29,816 rows ($32.5M), of which $29.3M came from one prefix bug (CITIZENS read as CITI). Frozen code (2026-09-27).

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/collect.R:122-261`; `C:/Users/pgoff/Mentat/mentat/tools/cross_reference_engine.py`

**Assumptions.**
- Form/line plus text identify the role
- Ambiguity is not contradiction (D-040)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[irvin-2023-dark-investigation-dark]] | 2023 | suggests-extension | Opacity scores run from 0.8 (Fix the Debt Coalition) to 8.3 (Majority Forward) on a 0-9 scale. Scores above 7 are described as very opaque, including many large advocacy groups with very small boards, paltry websites and no staff. Scores up to 3 could be characterized as transparent. Donations average 92 percent of revenue. The total index does not predict the OpenSecrets dark-money label once controls are added (N=54, Nagelkerke R2 0.529, only ln total expenses significant). | Any change to the exclusion is Paul's decision; this bearing recommends none. The index is unvalidated in this sample, so at most a display-only heuristic label carrying its 2016 and N=54 window. Form 990 data is published two or more years late and Mentat does not collect it. | weak | VERIFIED-SOFTENED; system side: The earlier analysis describes Paul's 2026-07-18 ruling excluding 527 and 501c recipients; that ruling was not found in the memory index or in the analyst files checked, so its date is unverified here. The code keeps 501c-marked recipients as classified rows, tags them EXCLUDED_527_501C, and drops them from reported totals as one block. No transparency distinction is made among 501c recipients. |

**Assessment.**

**Evidence weight: effectively none.** The one bearing is irvin-2023 (weak, suggests-extension). Its opacity index for 501(c) advocacy groups runs from 0.8 to 8.3 on a 0-9 scale, and it does **not** predict the OpenSecrets dark-money label once controls are added. That result is N = 54, 2016, Nagelkerke R2 0.529 with only ln total expenses significant. Irvin's own sample shows the index is unvalidated, so it could at most be a display-only label carrying its window, and Mentat does not collect Form 990 data. The bearing recommends no change.

Furnas, LaPira & Brock 2025, the paper Paul's position cites for this component, appears in the evidence only as a measurement-scope bearing on employer-match. Nothing in the evidence addresses receipt-role classification from form, line and description, or the contradiction-only rule (D-040).

**Internal facts that matter:**
- The Banking run excluded 29,816 rows ($32.5M), and $29.3M of that came from one prefix bug. So the exclusion stage's observed failures come mostly from upstream employer matching, not from role logic.
- The bearing notes a **separate** exclusion path: 501(c)- and 527-marked recipients are tagged EXCLUDED_527_501C and dropped from reported totals as one block. The ruling behind that path (described as 2026-07-18) could not be verified in the memory index or analyst files checked. This is a category-level exclusion, distinct from the contradiction-only row rule. It should be documented as its own ruling with its own provenance so the two are not conflated.

**GAP:** the empirical question is what share of excluded rows are true misattributions. The study to fill it is a manual audit of a random sample of excluded rows and a random sample of kept-but-flagged ambiguous rows, stratified by exclusion reason and dollar size. It should report the true-contradiction rate and the false-exclusion rate, with run, window and denominator named. Code is frozen (2026-09-27), so this is measurement only.

**Contradictions.**
- The component is described as excluding a row 'only when its own text names another organisation', but the code also drops 501(c)/527-marked recipients from reported totals as a block (EXCLUDED_527_501C). The ruling behind that block exclusion is unverified, so the contradiction-only description is incomplete.
- The cited source (Furnas, LaPira & Brock 2025) does not bear on receipt-role classification in any evidence row. The citation is attached to the wrong component or is unsupported here.

**Gaps.**
- No study of receipt-role classification accuracy from FEC form/line/description fields.
- No manual audit of excluded versus kept-ambiguous rows (true-misattribution rate).
- No provenance-verified ruling for the 501(c)/527 block exclusion.
- No literature on contradiction-only versus ambiguity-based exclusion rules in campaign-finance attribution.

**Paul's position vs the evidence.** Paul's position records that no annotation addresses this directly, and the evidence confirms it. The single bearing (Irvin) is a weak suggestion of a transparency label that its own data fails to validate. D-040 (ambiguity is not contradiction) neither gains nor loses support from the literature. Its main empirical test is internal, and the one internal result so far (the CITIZENS/CITI bug) points at the upstream matcher rather than the exclusion rule.

**Open questions.**
- Of excluded rows (Banking run, 29,816 rows), what share are true misattributions once the $29.3M prefix-bug block is set aside?
- Where is the 501(c)/527 block-exclusion ruling recorded, and with what date?
- Should prefix-collision rows be counted as an employer-match defect rather than an exclusion outcome when the audit is reported?

**Read first.** [[irvin-2023-dark-investigation-dark]], [[furnas-nd-conflating-lobbying-pacs]], [[anon-2025-cross-industry-political]]

## Conduit legs, memo dedup, filing-version identity and transfer rule

**What it computes.** Conduit legs are counted; memo legs are deduped by tuple; one winning amendment version per transaction; 24G/24K transfers; PAS2 index v6 count_latest.

**Method.** Known defect: an off-by-cents memo parent ($176,942.05 Crypto double count).

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/schema.R:598-1098`; `C:/Users/pgoff/Mentat/mentat/tools/transfer_rule.py`

**Assumptions.**
- Same amount or date means the same transaction version

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bonica-2025-money-campaign-finance]] | 2025 | measurement-source | Small-donor coverage is near-complete only from 2020. ActBlue and WinRed itemize sub-$200 gifts, and WinRed launched during the 2020 cycle, so earlier cycles lack most small Republican donors. Pre-2020 Republican \|Age Gap\| coefficients come out unexpectedly positive. The authors attribute this to lower age variance among Republican donors, made more extreme by the missing small donors, and to possible partisan differences in age-related preferences. The coverage hole is not the sole cause. | This applies to any Mentat cross-cycle comparison of employee (individual) giving or its partisan share that spans 2018 to 2020. The 2020 jump in itemized Republican small-dollar conduit rows is a reporting artifact, not a behavioural change. It does not affect PAC-to-candidate measures, which are committee transfers, or the current anchor cycle (2024), which falls after the break. | weak | VERIFIED-SOFTENED; system side: Conduit legs are kept and counted to their named recipients. Only same-recipient memo redisclosures with an eligible counterpart are removed. Employee candidate giving feeds the employee_candidate share basis in the giving forecast. |

**Assessment.**

**Evidence weight: almost nil on the component's actual mechanics.** The single bearing, bonica-2025 (weak), concerns a **coverage break**, not deduplication or amendment handling. Sub-$200 itemization through ActBlue and WinRed makes small-donor coverage near-complete only from 2020, and WinRed launched during the 2020 cycle. So the jump in itemized Republican small-dollar conduit rows across 2018 to 2020 is a reporting artifact.

For Mentat this matters for any cross-cycle comparison of employee giving or its partisan share spanning 2018 to 2020. It does not matter for PAC-to-candidate measures or for the 2024 anchor cycle, which falls after the break. The bearing supports **labelling** pre-2020 vs post-2020 employee series as non-comparable on the Republican side. It says nothing about conduit-leg counting.

Nothing in the evidence addresses:
- amendment frequency;
- whether amendments change amounts or dates;
- the count_latest rule;
- memo-tuple dedup;
- 24G/24K transfer identity.

Harrison et al. 2018 (late-filed records), cited in Paul's position, has no evidence row.

The known internal defect is an off-by-cents memo parent that produced a $176,942.05 Crypto double count. It shows the assumption 'same amount or date means the same transaction version' is brittle at cent level. No literature calibrates how often that happens.

**GAP:** this needs an empirical study of FEC amendment behaviour. Pair original and amended filings (FEC bulk files carry amendment indicators and filing IDs) for a sample of committees and cycles. Measure the share of amended transactions where amount changes, date changes, both change, or rows appear or disappear. Then recompute totals under count_latest versus transaction-level version matching to size any bias. A targeted check of memo parents with cent-level mismatches should go alongside.

**Contradictions.**
- The only evidence row (bonica-2025) is about the 2020 small-donor itemization break, not deduplication or versioning. The component's empirical question (amendment amount vs date changes, count_latest bias) has no bearing at all.
- The assumption 'same amount or date means the same transaction version' is contradicted internally by the off-by-cents memo-parent defect ($176,942.05 Crypto double count).

**Gaps.**
- No study of FEC amendment frequency or what amendments change (amount, date, presence).
- No evidence on bias from count_latest versus transaction-level version identity.
- No literature on memo/conduit double-counting rates in Schedule A.
- Harrison et al. 2018 (late-filed records) is cited in Paul's position but has no evidence row.

**Paul's position vs the evidence.** Paul highlighted late-filed and duplicate filings as a pipeline hazard. The evidence neither confirms nor measures that hazard; the one bearing concerns a different hazard (pre-2020 small-donor under-itemization). The internal off-by-cents double count confirms the hazard is real in Mentat, but its frequency is unmeasured.

**Open questions.**
- How often do FEC amendments change amounts versus dates, by form type and cycle?
- Does count_latest raise or lower totals relative to transaction-level matching, and by how much per industry?
- How many memo parents differ from their child legs by cents, beyond the known Crypto case?
- Should employee-giving series spanning 2018 to 2020 carry a coverage-break label on the Republican side?

**Read first.** [[bonica-2025-money-campaign-finance]], [[bonica-2013-mapping-ideological-marketplace]], [[kim-2020-strategic-sequential-links]]

## PAC cash proportional attribution

**What it computes.** A company's share of each PAC payment = its share of labelled cash just before the payment.

**Method.** Ordered events, cent-level conservation, withhold on unresolved inputs. The first application failed the cash gate.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/tools/pac_cash_model.py`

**Assumptions.**
- Cash is fungible within a committee
- Same-day receipts come before payments

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) |  |  |  |  |  |  |

**Assessment.**

**GAP: no evidence.** No paper bearing addresses how a committee's outgoing payments should be attributed to its incoming sources, and Paul's position records no literature.

The nearest material is not evidence for this component:
- christopher-2016 and irvin-2023 describe money-flow networks but give no attribution rule.
- Mentat's own transfer-graph allocate() is already labelled a conditional pro-rata scenario, not traced dollars. That framing should carry over to cash-proportional output.

The component's assumptions are design choices with no external test:
- cash is fungible within a committee;
- same-day receipts come before payments.

The first application failed the cash gate, so internal validation is also outstanding.

**What kind of study fills it:**
1. **Sensitivity analysis on Mentat's own ledgers.** Recompute attributed totals under alternative rules (proportional, FIFO, LIFO, and a lowest-intermediate-balance tracing rule from trust and forensic-accounting doctrine). Also vary same-day ordering (receipts first, payments first, randomized). Report the spread of each company's attributed total as a range, with cycle and committee denominator.
2. **A doctrinal and literature review** of fund-tracing rules (forensic accounting, trust law) and of any FEC or academic treatment of earmarked versus general-treasury attribution. This would tell Paul whether proportional attribution is the conventional default or one choice among several.
3. **Cash-gate failure diagnosis** before any output is used.

Until the first study exists, attributed shares should be presented as model outputs under a stated rule, not as traced dollars.

**Contradictions.**
- (none)

**Gaps.**
- No literature on attributing committee disbursements to their funding sources (proportional vs FIFO vs tracing rules).
- No sensitivity study of attributed totals to same-day ordering.
- The first application failed the cash gate; no passing internal validation exists yet.

**Paul's position vs the evidence.** Paul's position is 'No literature', and the evidence agrees. The proportional rule rests on a fungibility assumption that is defensible as a convention but untested. Nothing supports or contradicts it.

**Open questions.**
- How much do company-attributed totals move between proportional, FIFO and lowest-intermediate-balance rules, per committee and cycle?
- How sensitive are totals to the same-day receipts-before-payments assumption?
- Why did the first application fail the cash gate, and is the failure in the inputs (unresolved rows) or in the ordering logic?
- Should output be labelled a conditional scenario, matching allocate()'s own docstring?

**Read first.** [[christopher-2016-mathematical-modeling-analysis]], [[irvin-2023-dark-investigation-dark]], [[goff-nd-cross-industry-finance]]

## PAC transfer graph and beneficiary direction partition

**What it computes.** PAC -> PAC -> candidate flows, and support/oppose/party four-bucket attribution (Dem/Rep = downstream - oppose).

**Method.** 89% of beneficiary dollars were oppose. The attributed-estimate producer is unbuilt.

**Code refs.** `fec_transfer_graph.py`; `C:/Users/pgoff/Mentat/mentat/R/report.R`

**Assumptions.**
- Direction codes identify support vs oppose

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[gaynor-2022-financial-ties-bind]] | 2022 | method-source | Each member-to-member LPAC donation is a directed edge. In the 115th Congress 282 members headed an LPAC, 373 received LPAC money, there are 12,870 unique directed edges, and the average donation is $3,682.19 per tie. Source is OpenSecrets compiled from FEC records. | The R functions default to cycles 2022, 2024, 2026. Gaynor's 115th and 116th windows need the 2018 and 2020 cycles, which the defaults do not cover; the cycles argument can override. Sponsor name-key matching can give false or missing links. Any such edge is a donation record, not an access or influence edge (D-025), and must name its cycle window and denominator. | moderate | VERIFIED-SOFTENED; system side: Mentat fec_read_candidate_committee_links keeps designation D (Leadership PAC linked to candidate) from ccl files, and fec_read_leadership_pacs reads leadershipYYYY.csv sponsor files, with a Leadership PAC sponsor relationship. fec_transfer_graph.py counts only 24G and 24K cash-transfer records and describes itself as a committee cash-transfer graph. |
| [[irvin-2023-dark-investigation-dark]] | 2023 | qualifies | Two high-opacity C4s are described as moving money into political committees as grants. One Nation gave 21.7 million dollars to the Senate Leadership Fund, a PAC at the same address, out of 63.3 million spent in 2016. Freedom Frontier made a 4.4 million dollar grant to a PAC, with almost 75 percent of its expenses political. The paper presents these as anecdotes, not a sample-wide pattern. Grants between C4s are not classified as political, so such flows fall outside its C6 component. | The structural point holds for the graph as read. The paper covers 2016 economic-policy C4s and gives no size estimate for C4 money reaching Super PACs, so no magnitude may be inferred. Mentat scope (2022-2026 industry PACs) was not checked here. | moderate | VERIFIED-SOFTENED; system side: Header line 3 states OTH 24G/24K outgoing records only. CASH_CODES (line 22) and the COMMITTEE regex (line 23) accept only a C plus eight digits, so a non-committee contributor such as a 501(c)(4) gets no node and its money appears only as a receipt into the Super PAC. allocate() conserves each root's funding forward over observed edges, so upstream attribution past a committee is not available. The earlier 89 percent oppose figure was not found by grep in fec_transfer_graph.py or report.R and is not confirmed here. |
| [[irvin-2023-dark-investigation-dark]] | 2023 | supports | Beckel 2018, cited and not re-analysed, is reported as showing that political spending routed through dark money organizations is strongly negative, asking voters to defeat candidates, per FEC filings. Section 12 calls the dark-money media financing overwhelmingly negative. The paper gives no count or share, and the claim concerns dark money vehicles in general, not this C4 sample specifically. | Qualitative, second-hand, 2016 C4-linked spending. Mentat's beneficiary split is a modelled conditional scenario for a different period and denominator. At most directional agreement; no numeric confirmation. | weak | VERIFIED-SOFTENED; system side: allocate() docstring keeps opposition money on the named target, never its rivals (line 489). The 89 percent beneficiary oppose share cited in the earlier analysis was not found in fec_transfer_graph.py or report.R, so it is unconfirmed and must not be cited as corroboration. |
| [[christopher-2016-mathematical-modeling-analysis]] | 2016 | suggests-extension | Money-flow network among politically active 501(c) nonprofits and SMLLCs. A tie means funds were exchanged, tie width is dollar amount, and node size is betweenness (Figure 1). The top-ranked organization, Freedom Partners Chamber of Commerce, has out-degree 90 (Table 5). The paper does not state how its network data were built: its background sources are news and IRS material from 2010-2014, and the network comes from earlier unpublished work. | Scope statement only. Money that passes through a non-disclosing nonprofit before reaching a committee is invisible to Mentat's committee graph. The paper gives no data-construction method, so its network cannot validate any edge set. Its network is a single partisan donor network from about 2010-2015. Do not import its ranking as evidence about any industry; any key-node ranking on Mentat's graph is a heuristic output (D-025). | moderate | VERIFIED-SOFTENED; system side: Mentat walks FEC-reported committee-to-committee edges breadth-first to depth 3 (reachable_rows default max_depth=3). allocate() spreads each root's funding pro rata over observed outflows and raises an error if root funding is not conserved. Unattributable dollars go to unresolved buckets (depth_limit, cycle, no_observed_outflow). The allocate() docstring calls the result a conditional pro-rata scenario, not traced dollars. |
| [[anon-2025-cross-industry-political]] | 2025 | qualifies | Mutual-fund Super PAC money goes particularly to PACs aligned with the GOP (memo wording; the memo does not say mostly). The memo calls this a vast reservoir of partisan spending. House and Senate Majority PAC donations linked to JPMorgan are +90% from the company rather than individuals (memo wording, no denominator). Credit unions report $0 dark money to the four biggest Super PACs. | Quote the memo's partisan and dark-money readings only after recomputing with the support/oppose split and the 501c and 527 separation. The attributed-estimate producer status claimed by the analyst was not checked. | moderate | VERIFIED-SOFTENED; system side: report.R lines 32-34 define 527 and 501c as supplementary categories. Lines 55-77 define the channel columns, including Super PAC, Trade Association and Company PAC: To Super PAC, each with a partisan split. spending_title_role at line 2528 onward and spending_contributor_tier assign Corporate to entity contributors. fec_transfer_graph.R lines 30-42 hold the F12 date-walk rule. The brain note that 89 percent of beneficiary dollars were oppose-coded was not found in the Project Brain melange-systems-brain folder searched. |
| [[chohan-2025-crypto-oligarchy-cryptocurrency]] | 2025 | supports | Fairshake, Protect Progress and Defend American Jobs ran ads that often did not mention cryptocurrency, focusing on candidate integrity or economic policy. Named races: Fairshake backed Moreno against Brown in Ohio and opposed Tester in Montana. The paper also cites opposition to Porter in a California Senate primary and to Bowman's re-election bid. Coinbase, Ripple and Andreessen Horowitz each gave tens of millions of dollars, with no exact figures. No oppose or support split of dollars is given. | Qualitative evidence for federal races in 2024 only. It supports keeping support and oppose separate rather than netting them, and supports the point that ad topic cannot be read from ad content. It does not support any oppose share, and the paper's own examples include backing candidates as well as opposing them. | weak | VERIFIED-SOFTENED; system side: Crypto workbook builder has a Super PAC scope and a Corporate tier (scopes list near lines 550-583) and a support_oppose field on rows (near line 177). The transfer graph reads only 24G and 24K outgoing OTH records and never counts independent expenditures again as transfers (docstring, lines 1-7). Code side verified. |
| [[goff-nd-cross-industry-finance]] |  | suggests-extension | Super PACs take 71.9 percent of crypto non-direct-to-candidate giving, which is 70 percent of industry giving; Fairshake is the 2nd largest super PAC and wholly crypto-funded; backed candidates won 38 of 40 primaries. | The 38 of 40 count has no window, no denominator of contested races and no baseline win rate. The 71.9 and 70 percent shares have no stated cycle or source. Reuse needs the support and oppose split by party and a stated cycle. | moderate | VERIFIED-SOFTENED; system side: fec_build_beneficiary_tables seeds the traversal from each industry's funded committees (fec_beneficiary.R traversal_seed, around lines 1041-1060). Downstream rows carry a Support or Oppose direction (fec_beneficiary.R lines 136-145 and 348), so oppose money can be split from support money. An attributed-estimate view exists in the UI, but the route refuses with 409 until an estimate bundle exists (app/evidence-flow.js lines 8-10). A comment at R/export_guards.R line 278 says 89 percent of one shipped beneficiary sheet was opposition-direction rows. |

**Assessment.**

**Evidence weight: the richest component (7 bearings, all VERIFIED-SOFTENED, 5 moderate, 2 weak), but it bounds scope rather than validating results.**

- **gaynor-2022 (moderate).** This is a method source for leadership-PAC member-to-member directed edges. The 115th Congress had 12,870 unique directed edges, an average of $3,682.19 per tie, from OpenSecrets data built on FEC records. Mentat already reads leadership-PAC designations and sponsor files. Its R defaults cover cycles 2022, 2024 and 2026, so reproducing Gaynor's 115th and 116th windows needs the 2018 and 2020 cycles. Any such edge is a donation record, not an access or influence edge (D-025), and must name its cycle window and denominator.
- **irvin-2023 qualifies (moderate).** 501(c)(4) money reaching Super PACs as grants (One Nation gave $21.7M to Senate Leadership Fund in 2016) is invisible upstream of the committee in Mentat's graph. Only C-plus-8-digit committees get nodes. These are anecdotes, with no magnitude estimate.
- **christopher-2016 (moderate).** This is a scope statement. Nonprofit-to-nonprofit flows are outside the FEC committee graph. The paper does not say how its network was built, so it cannot validate any edge set, and its centrality rankings must not be imported.
- **irvin-2023 supports (weak).** Second-hand: dark-money spending is described as overwhelmingly negative (Beckel 2018), with no count or share. This gives directional agreement at most.
- **chohan-2025 (weak).** Qualitative evidence for the 2024 federal races. It supports keeping support and oppose separate rather than netting them, since Fairshake both backed and opposed candidates. It also shows ad topic cannot be inferred from ad content. It gives no oppose share.
- **anon-2025 memo (moderate).** Its partisan readings (mutual-fund Super PAC money going to GOP-aligned PACs; '+90%' for JPMorgan with no denominator) may be quoted only after recomputation with the support/oppose split and the 501(c)/527 separation.
- **goff-nd (moderate).** Paul's own report gives 71.9% and 70% Super PAC shares with no stated cycle or source, and '38 of 40 primaries' with no contested-race denominator or baseline win rate. Under the brain's wording rules these figures cannot be reused as they stand.

**The 89% oppose figure is unsupported as stated.** The component says '89% of beneficiary dollars were oppose'. Three bearings could not find it in fec_transfer_graph.py, report.R or the brain folder. goff-nd traced it to a comment at R/export_guards.R:278 describing 89% of **one shipped beneficiary sheet** as **opposition-direction rows**. Rows are not dollars, and one sheet is not the total. The attributed-estimate producer is unbuilt (the UI route returns 409 until a bundle exists).

**GAP:** what share of industry Super PAC money ultimately supports versus opposes candidates by party is unmeasured in the literature and unproduced in Mentat. Fill it by building the attributed-estimate producer and computing dollar-weighted support/oppose shares by party, per industry and cycle, with the unresolved buckets (depth_limit, cycle, no_observed_outflow) reported as their own share.

**Contradictions.**
- The component record states '89% of beneficiary dollars were oppose'. The only located source (R/export_guards.R:278 comment) says 89% of ONE shipped beneficiary sheet were opposition-direction ROWS. That is a different denominator and unit, and the figure was not found in fec_transfer_graph.py, report.R or the brain folder searched.
- chohan-2025's own examples include backing candidates (Moreno) as well as opposing them (Brown, Tester, Porter, Bowman), so it cannot corroborate any high oppose share.
- goff-nd (Paul's report) quotes Super PAC shares and a 38-of-40 primary record with no window, denominator or baseline. These conflict with the project's rule that every figure names its window and denominator.

**Gaps.**
- No dollar-weighted support/oppose-by-party share for industry Super PAC money; the attributed-estimate producer is unbuilt.
- No coverage of 501(c)(4) and other non-committee upstream flows; Mentat's graph cannot see them by construction, and no source sizes them for 2022-2026 industry PACs.
- No validation of leadership-PAC sponsor name-key matching (false or missing links).
- Gaynor-window cycles (2018, 2020) are outside Mentat's defaults.

**Paul's position vs the evidence.** Paul ties dark-money donation behavior to Furnas's ideological infrastructure and has no annotation on PAC flows. The evidence supports two structural design choices: keep support and oppose separate (chohan), and treat committee-graph attribution as a conditional pro-rata scenario with explicit unresolved buckets (christopher, irvin). It also warns that the graph is blind upstream of committees. It does not support any specific oppose share, including the 89% figure, which is unsupported as a dollar share. Gaynor offers a well-specified leadership-PAC edge definition that fits Mentat's data, but only as donation records under D-025.

**Open questions.**
- What is the dollar-weighted oppose share by party once the attributed-estimate producer exists, per industry and cycle, with unresolved buckets reported?
- Should the '89% oppose' line in the component record be corrected to 'rows on one shipped sheet', pending recomputation?
- Should leadership-PAC edges for the 115th/116th Congress (cycles 2018, 2020) be built to reproduce Gaynor as a check on sponsor matching?
- How should goff-nd's 71.9%, 70% and 38-of-40 figures be re-sourced with cycle and denominator before reuse?

**Read first.** [[gaynor-2022-financial-ties-bind]], [[irvin-2023-dark-investigation-dark]], [[chohan-2025-crypto-oligarchy-cryptocurrency]], [[christopher-2016-mathematical-modeling-analysis]], [[goff-nd-cross-industry-finance]], [[anon-2025-cross-industry-political]]

## Collection completeness (zero provenance, count reconciliation, key pool)

**What it computes.** Verified zeros, count reconciliation, and pagination-shortfall rejection; key rotation with an hourly budget.

**Method.** Only the browser lane stamps zero_verified. Three reconcile passes. The per-key vs per-IP rate limit is unverified.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/fec_api.R`; `C:/Users/pgoff/Mentat/mentat/R/fec_page.R:158`; `C:/Users/pgoff/Mentat/mentat/tools/api_request_pool.py`

**Assumptions.**
- The FEC count is the completeness target

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) |  |  |  |  |  |  |

**Assessment.**

**GAP: no evidence.** No bearing addresses openFEC API completeness, recordsTotal accuracy, false zeros, pagination shortfalls or rate-limit behaviour, and Paul's position records no literature. This is unsurprising, because the question is about a data service rather than a research topic. It still means the 'FEC count is the completeness target' assumption is untested.

Adjacent context only:
- bonica-2025 and bonica-2013 show that FEC disclosure completeness varies by era (lower address and occupation disclosure in the 1980s and early 1990s; small-donor itemization near-complete only from 2020).
- kim-2020 gives the scale of Schedule A (about 53M itemized contributions, 1980-2018).

None of these speaks to API versus bulk-file agreement.

Internal state: only the browser lane stamps zero_verified, there are three reconcile passes, and whether rate limits are per-key or per-IP is unverified.

**What kind of study fills it:**
1. **A reconciliation audit.** For a stratified sample of committees and cycles, compare openFEC API recordsTotal and paged row counts with FEC bulk-file counts for the same filters. Measure the false-zero rate, the pagination-shortfall rate and the direction of disagreement, reported with window and denominator.
2. **Settle the rate-limit question from documentation** (api.data.gov and openFEC docs) and from response headers already captured in Mentat's logs. Do not use load or launch-loop testing, which is barred on this machine.

**Contradictions.**
- (none)

**Gaps.**
- No study of openFEC recordsTotal accuracy or false-zero frequency relative to bulk files.
- No evidence on pagination shortfall rates.
- Per-key versus per-IP rate limiting is unverified; it should be resolved from documentation and logged headers, not load tests.

**Paul's position vs the evidence.** Paul's position is 'No literature', and the evidence agrees. The completeness assumption (FEC count = target) is reasonable but unvalidated. Only an API-versus-bulk reconciliation audit can test it.

**Open questions.**
- What is the false-zero rate of openFEC queries versus bulk files, by schedule and cycle?
- Does recordsTotal ever undercount relative to bulk, which would make count reconciliation pass on incomplete data?
- Is the openFEC/api.data.gov rate limit applied per key or per IP, per documentation and logged response headers?
- Should non-browser lanes be able to stamp zero_verified after a bulk-file cross-check?

**Read first.** [[bonica-2025-money-campaign-finance]], [[bonica-2013-mapping-ideological-marketplace]], [[kim-2020-strategic-sequential-links]]

## Across components

## Cross-component findings

1. **Upstream matching errors show up as exclusion results.** The largest observed failure in the system is that $29.3M of the $32.5M Banking exclusion (29,816 rows) came from one employer-key prefix collision (CITIZENS read as CITI). That is an employer-match defect that surfaced at the exclusion stage. Audits of receipt-role/exclusion should separate upstream matcher defects from true contradiction outcomes. Employer-match precision is the more urgent unmeasured quantity.

2. **The literature bounds scope and validates nothing.** Across all bearings, none reports a precision, recall, false-merge, false-zero or attribution-bias figure that transfers to Mentat. The papers either describe comparable designs (Kim et al., Bonica, Gaynor) or mark what the FEC committee graph cannot see. The blind spots are 501(c)(4) and nonprofit upstream flows, executive-controlled foundations and donor-advised funds, and pre-2020 small-donor under-itemization (Irvin, Christopher, anon memo, Bonica 2025). Validation of Mentat has to come from hand-coded samples and FEC bulk-file reconciliation.

3. **Unit and denominator drift.** Three figures fail the window-and-denominator rule:
   - the '89% of beneficiary dollars oppose' line, which traces to 89% of rows on one shipped sheet;
   - goff-nd's 71.9%, 70% and 38-of-40 figures;
   - the anon memo's '+90%'.

   The 57% (157/276) opposition-prior sign-agreement figure also needs its window stated before it is quoted. These are presentation-layer risks shared by the transfer-graph and employer-match components.

4. **Two different 'company giving' quantities.** Furnas-type measures count org plus affiliated-PAC money only. Mentat's company totals add employee giving matched by employer string. Employee giving across 2018 to 2020 also carries the WinRed/ActBlue itemization break on the Republican side. Any external comparison or cross-cycle trend has to state which quantity and which side of 2020 it uses.

5. **Citation hygiene.** Kim et al. appears as 2020 (positions), JOP 2026 (citation list) and 2020/2023 (evidence slugs), with SOC 27-3030 present in the 2020 extract but refuted for the 2023 paper. The dark-money network paper appears as 'Couch, Fox & Everton 2016' in positions and under the slug christopher-2016, which is probably an author-name parse. Furnas, LaPira & Brock 2025 is attached to receipt-role in positions but bears only on employer-match scope.

6. **Guardrails that apply when these papers are reused.** The Kim et al. papers link donations to lobbying. LDA may enter Mentat only as an identity source, never as an edge or weight (D-021). Gaynor-style leadership-PAC edges and any centrality ranking on the transfer graph are donation records and heuristic outputs, not access, closeness or influence measures (D-025). No bearing argues for either of the two rejected SNA proposals (dropping regular cosponsorship edges; per-edge volume normalization). Christopher's dollar-width ties are a display choice, not a normalization proposal.

7. **Two components rest on internal invariants alone.** Cash-proportional attribution, which has also failed its first cash gate, and collection completeness have zero evidence. Dedup/versions has none on its actual mechanics. These carry the dollar totals that every downstream surface quotes, so their internal audits should come before any further literature work on the better-covered components.

## Research agenda

1. Employer-match precision/recall audit: hand-code a stratified sample of Schedule A rows (by company size, acronym and prefix-collision family such as CITI/CITIZENS, and cycle). Report precision and recall per matcher path with window and denominator. Kim, Stuckatz & Wolters is the design template.
2. Identity-ladder benchmark: hand-label same-name candidate pairs from Mentat roster matches, stratified by surname frequency, zip agreement and suffix presence. Score AUTO/HIGH/REVIEW precision and compare with a frequency-weighted fastLink-style linker on the same labels. Any ladder change is Paul's ruling.
3. Exclusion audit: manually review random samples of excluded and kept-ambiguous rows from the Banking run. Treat the $29.3M prefix-bug block as a separate stratum, and report the true-contradiction and false-exclusion rates. Separately, locate the provenance of the 501(c)/527 block-exclusion ruling.
4. FEC amendment study: pair original and amended filings from bulk data for a committee and cycle sample. Measure how often amendments change amount, date or row presence, and size the count_latest bias against transaction-level version matching. Scan for cent-level memo-parent mismatches beyond the $176,942.05 Crypto case.
5. Cash-attribution sensitivity: recompute attributed totals under proportional, FIFO, LIFO and lowest-intermediate-balance rules and under alternative same-day orderings. Report per-company ranges. Diagnose the first cash-gate failure first. Pair this with a short review of fund-tracing doctrine in forensic accounting and trust law.
6. Beneficiary direction: build the attributed-estimate producer and compute dollar-weighted support/oppose-by-party shares per industry and cycle, with unresolved buckets reported. Replace or relabel the '89% oppose' line, which currently traces to rows on one sheet, not dollars.
7. Collection completeness: reconcile openFEC recordsTotal and paged counts against FEC bulk files for a stratified committee and cycle sample to estimate false-zero and pagination-shortfall rates. Settle per-key vs per-IP rate limiting from documentation and logged headers only, with no load testing.
8. Leadership-PAC edge check: build cycles 2018 and 2020 to reproduce Gaynor's 115th-Congress LPAC edge count as a test of sponsor name-key matching. Label outputs as donation records under D-025.
9. Citation cleanup: reconcile the Kim et al. years (2020 / 2023 / JOP 2026) and the SOC 27-3030 attribution, the Couch/Fox/Everton vs christopher-2016 slug, and where Furnas, LaPira & Brock 2025 is attached. Add evidence rows for Harrison et al. 2018 and Huneeus & Kim 2018, which Paul cites but which have no bearing.
10. Re-source Paul's report figures (goff-nd: 69% / 98.8% C-suite share, 71.9% / 70% Super PAC shares, 38 of 40 primaries) with a coding rule, cycle window, denominator and baseline before reuse.
