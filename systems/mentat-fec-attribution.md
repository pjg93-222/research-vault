---
type: system
name: "Mentat FEC money attribution and accounting"
slug: mentat-fec-attribution
components:
  - mentat-fec-attribution.employer-match
  - mentat-fec-attribution.identity-ladder
  - mentat-fec-attribution.receipt-role-and-exclusion
  - mentat-fec-attribution.dedup-and-versions
  - mentat-fec-attribution.cash-proportional-attribution
  - mentat-fec-attribution.transfer-graph-and-direction
  - mentat-fec-attribution.collection-completeness
brain_notes:
  - actors/mentat-fec-pipeline
  - concepts/mentat-fec-accounting-methodology
  - concepts/exclude-on-contradiction-not-ambiguity
  - concepts/pac-cash-proportional-attribution
  - actors/mentat-fec-pipeline-history-audit-r12
paper_count: 14
tags: [system, type/system-hub]
---

# Mentat FEC money attribution and accounting

The R/Python pipeline (C:/Users/pgoff/Mentat) that attributes FEC Schedule A, Schedule B and PAS2 money to roster companies and industries. Covers employer and identity matching, receipt-role classification, contradiction-only exclusion, conduit and memo dedup, filing-version identity, the PAC transfer graph, cash-proportional attribution, beneficiary direction, geography and collection completeness.

Brain notes: [actors/mentat-fec-pipeline](obsidian://open?vault=brain&file=actors/mentat-fec-pipeline); [concepts/mentat-fec-accounting-methodology](obsidian://open?vault=brain&file=concepts/mentat-fec-accounting-methodology); [concepts/exclude-on-contradiction-not-ambiguity](obsidian://open?vault=brain&file=concepts/exclude-on-contradiction-not-ambiguity); [concepts/pac-cash-proportional-attribution](obsidian://open?vault=brain&file=concepts/pac-cash-proportional-attribution); [actors/mentat-fec-pipeline-history-audit-r12](obsidian://open?vault=brain&file=actors/mentat-fec-pipeline-history-audit-r12)

**System:** Mentat FEC money attribution and accounting. **Evidence base:** 21 bearings from 14 works, spread over 6 components. 3 rows are VERIFIED and 18 are VERIFIED-SOFTENED; 0 are UNVERIFIED. **Strength:** 13 moderate, 8 weak, 0 strong.

**The short version.** The literature gives Mentat useful *design references* and *scope warnings*. It gives no *benchmarks*. None of the works reports a precision or recall figure, an error rate or a validation that could be carried over to Mentat's matching, dedup or attribution rules. The closest are Bonica 2013's record-linkage split and merge rates and CampaignView's hand-check counts, and both measure different linkers on different populations.

**Where the evidence sits (rows counted from the evidence file):**
- **Identity ladder:** 6 bearings, the most of any component except the transfer graph.
- **Transfer graph and beneficiary direction:** 7 bearings, mostly qualitative or scope-limiting.
- **Employer matching:** 4 bearings, all definitional or extension ideas.
- **Dedup and versions:** 2 bearings.
- **Collection completeness:** 1 bearing.
- **Receipt-role exclusion:** 1 weak bearing.
- **Cash-proportional attribution:** 0. It is a complete GAP.

**Three findings Paul should see first:**
1. **Earmark double count.** Bouton et al. 2022 drop the committee-itemized leg of an earmarked gift because the same gift appears in both the conduit report and the recipient's report. They coupled 87% of earmarked committee-itemized donations to their conduit twin. Mentat counts conduit legs and removed guard G3 (export_guards.R:110-113, 2026-07-27, R5/R8 recipient-as-shown ruling). The size of any resulting double count in Mentat books is unmeasured.
2. **The 89% oppose figure.** The component method states "89% of beneficiary dollars were oppose". Three verification passes could not find it in fec_transfer_graph.py, report.R or the brain folder. The only trace is a comment at R/export_guards.R:278 saying 89% of *one shipped beneficiary sheet* was opposition-direction *rows*. That is a share of rows, not dollars, with no stated cycle window and a one-sheet denominator. It should not be quoted as a dollar share.
3. **The AUTO rung needs only exact name plus same zip.** It has no street check and no name-frequency weighting. Bonica 2013's only wrong merges were same-employer family members (Gates Sr/Jr, Walton family). Bouton 2022 requires three exact fields including street. Mentat's rung is looser than both published designs on the dimension that adds a stranger's money to a company total.

Two sources are not independent evidence. goff-nd is Paul's own report, so it cannot corroborate his system. The anon-2025 memo has no stated author and gives figures without denominators. Both are kept as leads for extensions only.

## Employer-string roster matching and overrides

**What it computes.** Assigns Schedule A rows to roster companies by normalized employer key, prefix tokens and context; override rules use maximum-specificity matching.

**Method.** Strict-names rule (roster plus verified aliases only). Acronym collisions are open owner calls.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/collect.R:363-1080`, `C:/Users/pgoff/Mentat/mentat/R/ingest.R:43-205`

**Assumptions.**
- Employer text identifies the employer
- Unlisted spellings are unmatched

**Evidence rows:** 4 (VERIFIED 2, VERIFIED-SOFTENED 2)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[furnas-nd-conflating-lobbying-pacs]] |  | measurement-source | Organizational giving is campaign donations from the organization or its officially affiliated PAC; contributions by employees and contract lobbyists are not counted. Dark-money contributions are not observed. Independent expenditures (often by Super PACs) are not discussed; the article covers contributions to candidates only. | Comparing Mentat industry totals or participation counts with the paper's overlap or distribution figures requires restricting Mentat to organizational and affiliated-PAC receipts first. Melange's opposition prior already records that employee/executive giving and industry PAC money (congestion.json) are different instruments, with lean signs agreeing for only 157/276 members (57%) (fec/lobbying/build_opposition_prior.py:113). | moderate | VERIFIED-SOFTENED |
| [[anon-2025-cross-industry-political]] | 2025 | suggests-extension | Charles Schwab's company PAC and executives halted giving. Per the memo, Chairman Schwab and his wife moved giving to a charity fund that invested over $250M in conservative causes in 2021-2024 and gave to Republican Super PACs (memo figures, partly redacted). The memo's thesis is that spending is offloaded onto company executives, measured as CEO and executive averages per industry. | An extension would link executive-controlled vehicles (foundations, donor-advised funds, family entities) to roster people. That needs a verified-affiliation rule consistent with the strict-names policy, not a fuzzy guess. The memo never defines executive, so its averages cannot be reconciled with the Chief or Middle Management tiers. | weak | VERIFIED-SOFTENED |
| [[goff-nd-cross-industry-finance]] |  | suggests-extension | The C-suite share of crypto donations rose from 69 percent in 2021-22 to 98.8 percent in 2023-24; the report frames the credit union shortfall as missing employee and middle-management giving. | An occupation-role split would need a hand-coded precision check. The report gives no coding rule for C-suite, so its 69 and 98.8 percent figures cannot be reproduced from the extract. | moderate | VERIFIED |
| [[fremeth-2018-spillovers-regulating-corporate]] | 2018 | suggests-extension | Within firm-cycle and candidate-cycle fixed effects, a candidate receiving the PAC maximum is 0.066 more likely (SE 0.006) to receive a CEO gift and receives about 110 dollars more (SE 10.05) (Table 2; 1992-2008; 950 firms; 6,803,661 observations). The text reports 7.7 percent and 123 dollars, which do not match Table 2. Table 7 gives 123.88. | A per-candidate join of company PAC-at-limit status with Chief-tier gifts would test the CEO-substitution pattern in the industries Paul covers. Depends on executive employer-match recall and the PAC-to-candidate recipient table. Any result stays UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED |

**Assessment.**

**Evidence:** 4 bearings, all VERIFIED or VERIFIED-SOFTENED. None measures matching accuracy.

**Furnas, LaPira & Brock (n.d.)** (measurement-source, moderate). Its "organizational giving" means the organization or its affiliated PAC. It explicitly excludes employees and contract lobbyists, and it does not cover independent expenditures or dark money.
- Mentat's company totals include individual employee giving matched by contributor_employer. They therefore measure a different, broader quantity.
- This is a definitional mismatch, not an error. Any comparison with Furnas overlap figures must first restrict Mentat to organizational and affiliated-PAC receipts.
- Melange already records that the two instruments diverge: lean signs agree for 157 of 276 members (57%; fec/lobbying/build_opposition_prior.py:113).

**Fremeth et al. 2018** (VERIFIED, moderate, extension).
- Finding: within firm-cycle and candidate-cycle fixed effects, a candidate who received the PAC maximum is 0.066 (SE 0.006) more likely to receive a CEO gift. Window 1992-2008; 950 firms; 6,803,661 observations; Table 2.
- The paper is internally inconsistent: its text gives 7.7% and $123, Table 2 gives $110, and Table 7 gives $123.88. Quote Table 2 with its window.
- Use for Mentat: a CEO-substitution test that joins PAC-at-limit status to Chief-tier gifts. It depends on executive employer-match recall, which is itself unmeasured.

**goff-nd** (Paul's own report) and **the anon-2025 memo** both argue that giving is offloaded onto executives:
- goff-nd: C-suite share of crypto donations went from 69% in 2021-22 to 98.8% in 2023-24.
- Neither defines "executive" or gives a coding rule, so neither can be reconciled with Mentat's Chief and Middle Management tiers (report.R spending_title_role).

**Verdict.** Mentat's empirical question is the precision and recall of employer-string matching against a hand-coded sample, by company size. That question is still a GAP. The study to fill it is a stratified hand-coded sample of Schedule A rows per roster company, by company size and by employer-string variant class, scored for both:
- false matches: acronym collisions, and prefix bugs such as CITIZENS being read as CITI;
- misses: unlisted spellings.

Kim, Stuckatz & Wolters 2020 used a hand-coded ground truth but published no figures, so it is a template, not a benchmark.

**Contradictions.**
- Furnas defines organizational giving to exclude employee contributions. Mentat's company totals include employee giving matched by employer. Same word, different quantity, so the two cannot be compared without restricting Mentat first.
- Fremeth 2018 reports the CEO-gift effect inconsistently: the text gives 7.7% and $123, Table 2 gives 0.066 and about $110, and Table 7 gives $123.88.

**Gaps.**
- No study reports precision or recall for FEC employer-string matching. A hand-coded, size-stratified validation sample against Mentat's strict-names output is needed.
- There is no evidence on how much money the strict-names rule leaves unmatched (recall loss from unlisted spellings) compared with fuzzy or gvkey-crosswalk approaches.

**Paul's position vs the evidence.**

Paul highlighted Kim, Stuckatz & Wolters' match to Compustat gvkey, SOC executive codes and hand-coded ground truth, and he notes that Mentat's strict-names rule is more conservative.

The evidence agrees that Kim et al. validated against hand-coded truth. It also shows they report no precision or recall, so their approach cannot show that Mentat is either too strict or adequate.

The Furnas bearing adds a point Paul's position does not address: Mentat's company totals are not "organizational giving" in the Furnas sense. Paul's conservative posture is consistent with the literature's precision-first habit, but its recall cost is unmeasured.

**Open questions.**
- Should Mentat publish an organizational-only total (company PAC plus affiliated PAC) alongside the employee-inclusive total, so it can be compared with Furnas-style figures? This is Paul's call.
- What is the dollar-weighted recall of strict-names matching for small roster companies, where a few variant spellings may carry most of the money?
- Does executive-match recall support a Fremeth-style CEO-substitution test, or would a null be uninterpretable?

**Read first.** [[kim-2020-strategic-sequential-links]], [[furnas-nd-conflating-lobbying-pacs]], [[fremeth-2018-spillovers-regulating-corporate]], [[bonica-2013-mapping-ideological-marketplace]], [[goff-nd-cross-industry-finance]]

## Contributor identity match ladder

**What it computes.** AUTO, HIGH, REVIEW or DISCARD for same-name candidates by name parts, zip, city, state, occupation and employer contradiction.

**Method.** Ordered rules (exact name + zip -> AUTO, etc.). No name-frequency weighting.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/collect.R:1933-2157`

**Assumptions.**
- Same name + zip implies the same person

**Evidence rows:** 6 (VERIFIED 1, VERIFIED-SOFTENED 5)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bonica-2025-money-campaign-finance]] | 2025 | method-source | The DIME-to-L2 donor linkage is built to limit false positives. It links 75% of DIME individual contributors from 2012-2020, about 17M donors. Misses come from address differences (most common), donors who are not registered, and name differences. Unlinked donors have slightly more conservative CFscores and larger average gifts. The authors judge these imbalances small and not theoretically important. The r = 0.95 validation against Project Vote Smart (fn. 11) is for the CANDIDATE birth-year linkage, not the donor linkage. | Both pipelines prefer precision to recall, which is consistent with Mentat's fail-closed posture. The paper reports that unlinked donors give slightly larger amounts, and the authors call this unimportant. It is weak evidence at most that precision-first matching under-recovers larger donors, and it does not establish a downward dollar bias in Mentat. Mentat matches contributor rows to one another within an employer roster, which is a different task from voter-file linkage. Neither 75% nor r = 0.95 is a benchmark for Mentat. | weak | VERIFIED-SOFTENED |
| [[kim-2020-strategic-sequential-links]] | 2020 | method-source | FEC has no unique donor IDs (about 53M itemized individual contributions 1980-2018, 4M employer names, about 827,000 occupation names). Authors match PAC and employer names to Compustat gvkey and occupations to SOC codes (executives 11-1011, government relations 27-3030), standardize names with NLP as in Bonica (2017), and run unsupervised constrained graph clustering on address, donation history and targets; employer and occupation matches are cross-validated against a hand-coded ground truth and corporate PAC matches checked and revised manually. | Both work on FEC Schedule A, so the method transfers in kind. The paper reports no precision or recall, so it is a design reference, not a benchmark. Adopting clustering would loosen Mentat's rule-based ladder, and receipt-counting is frozen without per-change OK; any change is Paul's. The transferable piece is a hand-coded labelled sample to measure the ladder's precision and recall. | moderate | VERIFIED-SOFTENED |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | measurement-source | Supplement record-linkage section (text lines about 5315-5350, approx PDF p.51): on corporate board-member cases, 57 percent got a single ID, 42 percent split (type 1 errors), and in 13 percent of the split cases the algorithm failed to put 90 percent of records under one ID. Two type 2 errors (wrong merges), both family members at the same employer (Gates Sr and Jr; Walton family). Address and occupation disclosure was significantly lower in the 1980s and early 1990s. Method is fuzzy matching plus supervised machine learning. | The paper's type 2 errors were same-employer family members (the Gates Jr and Sr pair differs by a suffix). Mentat sends any suffix difference to REVIEW, so a missing-suffix pair does not get AUTO under the current rule. The split rates come from a different linker and are a reference point, not an error-rate estimate for Mentat. | moderate | VERIFIED-SOFTENED |
| [[kim-2023-systemic-sequential-links]] | 2023 | method-source | Donor and firm linkage uses constrained graph clustering and fastLink probabilistic record linkage (Enamorado, Fifield and Imai 2019). Executives are classed by SOC 11-1011. The paper reports no precision or recall for its linkage. | The paper confirms the fastLink method and the 11-1011 code only. SOC 27-3030 for government-relations staff is not in the paper text, so that part of the claim is refuted. Suggesting that frequency-weighted fastLink would fix the common-name blind spot is the analyst's opinion, and the paper gives no benchmark. Any change to the ladder is a ruling for Paul under D-040 (not checked here). | weak | VERIFIED-SOFTENED |
| [[porter-2025-campaignview-campaign-platforms]] | 2025 | context | Cross-cycle candidate names linked by probabilistic record linkage on pretrained embeddings, blocked on party and state. Estimated matches under 50% were hand-checked: 15 wrong for 2018-2020, 1 for 2020-2022, none for 2018-2022. | The paper links candidates (a small, ID-rich population), not donors; its error counts do not speak to donor-level false-match rates. Member bioguide lookup is a different problem from cross-cycle name linkage. | weak | VERIFIED-SOFTENED |
| [[bouton-2022-small-campaign-donors]] | 2022 | method-source | Donor grouping uses explicit rules. Two records are one person if they match exactly on three of first name, surname, full street and ZIP and fuzzily on the fourth (first-name variant index, fuzzy surname, fuzzy street, or city-state in place of ZIP). Occupation and employer are too inconsistent to use as grouping variables. The authors expect a very small false-positive rate but do not measure it. | The paper groups across all committees; Mentat links same-name rows to a seed within a company run (seed logic not traced in this check). A false merge adds a stranger's money to a company total, so the cost is higher. The paper's rules have no measured precision, so they are a design benchmark, not a validated standard. | moderate | VERIFIED |

**Assessment.**

**Evidence:** 6 bearings, the best-covered component after the transfer graph. All are VERIFIED or VERIFIED-SOFTENED. There are design references but no benchmark.

**Shared caveat.** No paper reports measured precision or recall for donor linkage:
- Kim 2020 and Kim 2023 (fastLink plus constrained graph clustering) report none.
- Bouton 2022 expects a very small false-positive rate but does not measure it.
- Bonica 2025's r = 0.95 is for *candidate* birth-year linkage, not donors.
- Bonica 2025's 75% linkage rate (about 17M DIME donors, 2012-2020) is for voter-file linkage, a different task.

**Bonica 2013** (moderate) is the only source with error counts, from its corporate-board-member check:
- 57% of cases got a single ID and 42% were split.
- There were two wrong merges, both same-employer family members (Gates Sr/Jr, Walton family).

This maps directly onto Mentat's weakest rung:
- Mentat sends any *visible* suffix difference to REVIEW.
- Exact name plus same zip gives AUTO, with no street check and no name-frequency weighting. If neither record carries the suffix, a father and son at the same zip and employer merge automatically.
- Bouton 2022 (VERIFIED, moderate) requires exact match on three of first name, surname, full street and ZIP. Mentat collects street (collect.R:1415-1416) but the ladder does not read it.
- Bouton also finds occupation and employer too inconsistent to group on. Mentat uses employer contradiction only to change the grade, which is consistent with that.

**Context only:**
- Porter 2025 (CampaignView) links candidates, not donors.
- Bonica 2025 reports that unlinked donors give slightly larger gifts. That is weak at most as evidence that precision-first linkers under-recover large donors, and it does not establish a dollar bias in Mentat.

**Cost asymmetry.** In Mentat a false merge adds a stranger's money to a company total. That makes precision more important than in DIME-style donor scoring, which supports Mentat's precision-first stance. The AUTO rung is less strict than the published rule designs.

Any ladder change is Paul's under the frozen receipt-counting rule and D-040.

**Contradictions.**
- Kim 2023's paper text does not contain SOC 27-3030 for government-relations staff. The analyst's claim was partly refuted, and only 11-1011 and fastLink are confirmed for 2023. Kim 2020's bearing does list 27-3030. Cite 27-3030 to Kim 2020 only.
- Bouton 2022 requires street in its exact-three-of-four rule. Mentat's AUTO rung grants identity on name plus zip without street, so it is looser than the published design.

**Gaps.**
- No measured false-merge rate for Mentat's ladder. A hand-labelled sample of AUTO and HIGH pairs is needed, oversampling common surnames and suffix-free family pairs at the same zip and employer.
- There is no head-to-head benchmark of rule ladders against frequency-weighted probabilistic linkage (fastLink) on FEC donors. A comparison on a shared labelled set would answer the component's empirical question.

**Paul's position vs the evidence.**

Paul highlighted the lack of unique donor identifiers, constrained graph clustering, and algorithms that limit false-positive matches.

The evidence confirms all three as features of the published designs (Kim 2020 and 2023, Bonica 2025). It also shows that none of those designs published a measured false-positive rate.

The evidence goes beyond Paul's position in one place. The literature's concrete false-merge cases (Bonica 2013 family members) and its stricter exact-field rule (Bouton's street requirement) both point at Mentat's AUTO rung, which Paul's annotations do not discuss.

**Open questions.**
- How many AUTO merges in a current company run involve suffix-free same-name pairs at the same zip?
- Would requiring street agreement for AUTO (and demoting name plus zip alone to HIGH or REVIEW) change company totals materially? This is a measurement question only, since any change is Paul's.
- Does the absence of name-frequency weighting concentrate false merges in high-frequency surnames in dense zips?

**Read first.** [[bouton-2022-small-campaign-donors]], [[bonica-2013-mapping-ideological-marketplace]], [[kim-2020-strategic-sequential-links]], [[kim-2023-systemic-sequential-links]], [[bonica-2025-money-campaign-finance]]

## Receipt-role classification and contradiction-only exclusion

**What it computes.** Economic role per receipt from form, line and description. A row is excluded only when its own text names another organisation; ambiguous rows are kept with a flag.

**Method.** The Banking run excluded 29,816 rows ($32.5M), of which $29.3M came from one prefix bug (CITIZENS read as CITI). Frozen code (2026-09-27).

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/collect.R:122-261`, `C:/Users/pgoff/Mentat/mentat/tools/cross_reference_engine.py`

**Assumptions.**
- Form/line plus text identify the role
- Ambiguity is not contradiction (D-040)

**Evidence rows:** 1 (VERIFIED-SOFTENED 1)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[irvin-2023-dark-investigation-dark]] | 2023 | suggests-extension | Opacity scores run from 0.8 (Fix the Debt Coalition) to 8.3 (Majority Forward) on a 0-9 scale. Scores above 7 are described as very opaque, including many large advocacy groups with very small boards, paltry websites and no staff. Scores up to 3 could be characterized as transparent. Donations average 92 percent of revenue. The total index does not predict the OpenSecrets dark-money label once controls are added (N=54, Nagelkerke R2 0.529, only ln total expenses significant). | Any change to the exclusion is Paul's decision; this bearing recommends none. The index is unvalidated in this sample, so at most a display-only heuristic label carrying its 2016 and N=54 window. Form 990 data is published two or more years late and Mentat does not collect it. | weak | VERIFIED-SOFTENED |

**Assessment.**

**Effectively a GAP.** There is 1 weak, VERIFIED-SOFTENED bearing, and it does not test the component.

**Irvin 2023** (suggests-extension, weak):
- It scores 501(c) opacity from 0.8 to 8.3 on a 0-9 scale.
- The index does not predict the OpenSecrets dark-money label once controls are added (N = 54, 2016 economic-policy C4s, Nagelkerke R2 0.529, only ln total expenses significant).
- So it is an unvalidated index in its own sample. At most it could be a display-only heuristic label carrying its 2016 and N = 54 window.
- Form 990 data arrive two or more years late, and Mentat does not collect them.
- The bearing recommends no change to the EXCLUDED_527_501C block.
- The 2026-07-18 ruling it refers to could not be found in the files checked, so its date is unverified.

**Nothing in the corpus tests:**
- whether form and line plus description text identify the economic role of a receipt;
- whether contradiction-only exclusion (D-040) achieves a lower misattribution rate than broader exclusion.

**The Banking run** shows why this matters. It excluded 29,816 rows ($32.5M), and $29.3M of that (about 90%) came from one prefix bug, CITIZENS read as CITI. The dominant failure mode was a matching defect upstream, not the exclusion rule itself.

**Study needed:** a stratified random manual audit of excluded rows and kept-with-flag rows. Stratify by exclusion reason and drop the prefix-bug block (or treat it separately). Score each row as a true misattribution or a false exclusion, and report the share with its run window and row and dollar denominators.

**Contradictions.**
- The ruling the Irvin bearing cites as 2026-07-18 could not be located in the memory index or the analyst files checked, so its date is unverified. The code behaviour itself (the EXCLUDED_527_501C block) is confirmed.

**Gaps.**
- There is no literature on classifying FEC receipt roles from form, line and memo text. The FEC's own line-number documentation and a manual audit would serve better than academic sources.
- No manual audit has measured the true misattribution share among excluded rows, which is the component's empirical question.
- There is no evidence comparing contradiction-only exclusion with ambiguity-inclusive exclusion on error rates.

**Paul's position vs the evidence.**

Paul cites Furnas, LaPira & Brock 2025 for this component, and his annotations say nothing addresses it directly.

The evidence agrees: no bearing speaks to role classification or to the contradiction-versus-ambiguity rule. The Furnas bearing is filed under employer matching and concerns what counts as organizational giving, not receipt roles.

The D-040 position stands without external support or contradiction.

**Open questions.**
- Of the excluded dollars outside the CITIZENS/CITI prefix block (about $3.2M of $32.5M in the Banking run), what share are true misattributions?
- Should the 501(c) block carry any transparency distinction, or stay a single supplementary category? Paul's call. Irvin's index is not validated for this use.

**Read first.** [[irvin-2023-dark-investigation-dark]], [[furnas-nd-conflating-lobbying-pacs]]

## Conduit legs, memo dedup, filing-version identity and transfer rule

**What it computes.** Conduit legs are counted; memo legs are deduped by tuple; one winning amendment version per transaction; 24G/24K transfers; PAS2 index v6 count_latest.

**Method.** Known defect: an off-by-cents memo parent ($176,942.05 Crypto double count).

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/schema.R:598-1098`, `C:/Users/pgoff/Mentat/mentat/tools/transfer_rule.py`

**Assumptions.**
- Same amount or date means the same transaction version

**Evidence rows:** 2 (VERIFIED-SOFTENED 2)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bonica-2025-money-campaign-finance]] | 2025 | measurement-source | Small-donor coverage is near-complete only from 2020. ActBlue and WinRed itemize sub-$200 gifts, and WinRed launched during the 2020 cycle, so earlier cycles lack most small Republican donors. Pre-2020 Republican \|Age Gap\| coefficients come out unexpectedly positive. The authors attribute this to lower age variance among Republican donors, made more extreme by the missing small donors, and to possible partisan differences in age-related preferences. The coverage hole is not the sole cause. | This applies to any Mentat cross-cycle comparison of employee (individual) giving or its partisan share that spans 2018 to 2020. The 2020 jump in itemized Republican small-dollar conduit rows is a reporting artifact, not a behavioural change. It does not affect PAC-to-candidate measures, which are committee transfers, or the current anchor cycle (2024), which falls after the break. | weak | VERIFIED-SOFTENED |
| [[bouton-2022-small-campaign-donors]] | 2022 | qualifies | A recipient committee must itemize an earmarked contribution when the donor's cycle aggregate to it exceeds 200 dollars, so the same gift appears in both the conduit report and the committee report. The authors coupled 87 percent of earmarked committee-itemized donations to their conduit twin and dropped the committee leg. The remaining committee-itemized donations are 3 percent of all contributions. | Paper population is employees whose cycle aggregate to the recipient exceeds 200 dollars. Sub-200 gifts are visible only through the conduit (paper Appendix A.4.3). The size of the double count in Mentat books is unmeasured. Money collection is frozen; any change needs Paul's per-change OK. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence:** 2 bearings, both VERIFIED-SOFTENED. One is consequential.

**Bouton et al. 2022** (qualifies, moderate):
- When a donor's cycle total to a recipient committee exceeds $200, that committee must itemize an earmarked gift. The same gift then appears in both the conduit report and the committee report.
- Bouton et al. coupled 87% of earmarked committee-itemized donations to their conduit twin and dropped the committee leg. The uncoupled remainder is 3% of all contributions.

**Mentat's practice and what is unverified:**
- Mentat counts conduit legs. It removed guard G3 against conduit double counting on 2026-07-27 under the R5/R8 recipient-as-shown ruling (export_guards.R:110-113).
- It dedups only same-recipient memo redisclosures that have an eligible counterpart.
- The verification could not show from the code that the two legs carry different sub_ids, or that the committee leg is counted.
- So the double count is a plausible hazard of unmeasured size, not a confirmed defect.

**Bonica 2025** (weak):
- Small-donor coverage is near-complete only from 2020, after WinRed launched (2019).
- So any cross-cycle comparison of employee giving or partisan share that spans 2018 to 2020 contains a reporting artifact.
- It does not affect PAC-to-candidate totals or the 2024 anchor cycle.

**GAP: filing versions.** No paper speaks to amendments, count_latest, or whether same amount or date identifies the same transaction version. The known off-by-cents memo-parent defect ($176,942.05 Crypto double count) shows the tuple key fails on cent differences.

**Study needed:** an empirical census of FEC amendment chains from bulk files:
- how often an amendment changes the amount versus the date versus the tran_id;
- totals under count_latest compared with tran_id-level resolution, by committee type and cycle.

Money collection is frozen. Any change needs Paul's per-change OK.

**Contradictions.**
- Bouton et al. treat the committee-itemized leg of an earmarked gift as a duplicate of the conduit leg and drop it. Mentat removed its conduit double-count guard (G3) and counts each receipt to the recipient as shown. These cannot both be right for the same gift unless Mentat's memo dedup already catches the committee leg, and the code check could not confirm that it does.

**Gaps.**
- There is no literature on FEC amendment and version behaviour (amount changes versus date changes, and bias from count_latest). An FEC bulk amendment-chain census is needed.
- The size of the conduit and committee double count in Mentat books is unmeasured. A per-industry, per-cycle count of committee-itemized earmarked rows with a conduit twin would measure it.
- The memo-parent tuple key has no tolerance-or-identifier study. The off-by-cents case suggests a sub_id or tran_id-based key needs testing.

**Paul's position vs the evidence.**

Paul cites Harrison et al. 2018 on late-filed records and flags late and duplicate filings as a pipeline hazard. The evidence supports that concern but from a different direction: Bouton documents a *structural* duplicate (the same earmarked gift reported by both the conduit and the recipient), not a late-filing duplicate.

No bearing addresses late filings or amendments, so Harrison's concern remains uncorroborated by this corpus.

The recipient-as-shown ruling (R5/R8) is Paul's. Bouton's practice differs from it, and the evidence is enough to warrant measuring the overlap, not to overturn the ruling.

**Open questions.**
- Do Mentat's committee-itemized earmarked rows and their conduit twins share a sub_id or tran_id? If not, are both counted?
- How many dollars per industry and cycle sit in committee-itemized earmarked rows that have a conduit twin?
- Should cross-cycle employee-giving comparisons that span 2018-2020 carry a coverage-break label?

**Read first.** [[bouton-2022-small-campaign-donors]], [[bonica-2025-money-campaign-finance]]

## PAC cash proportional attribution

**What it computes.** A company's share of each PAC payment = its share of labelled cash just before the payment.

**Method.** Ordered events, cent-level conservation, withhold on unresolved inputs. The first application failed the cash gate.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/tools/pac_cash_model.py`

**Assumptions.**
- Cash is fungible within a committee
- Same-day receipts come before payments

**Evidence rows:** 0

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment.**

**Complete GAP.** No bearing, no citation in the system, and Paul notes there is no literature.

The rule assigns a company a share of each PAC payment equal to its share of labelled cash just before the payment. It rests on two assumptions:
- cash is fungible within a committee;
- same-day receipts come before payments.

Neither has been tested, and the first application failed the cash gate.

**Christopher 2016** describes allocate() in the transfer graph as a "conditional pro-rata scenario, not traced dollars". That is the right framing, and it applies equally here: proportional attribution is a modelling convention, not an observed flow.

**Studies that would fill the gap:**
1. A sensitivity study on Mentat's own ledgers. Recompute attributed totals under:
   - proportional, FIFO, LIFO and lowest-intermediate-balance rules (from legal fund-tracing doctrine);
   - alternative same-day orderings: receipts-first, payments-first, and file-order.

   Report the spread per company and cycle as an uncertainty band.
2. A literature search outside political science, in forensic accounting and legal tracing of commingled funds, for which rules courts and auditors accept and why.
3. A check of the failed cash gate, to see whether the failure comes from unresolved inputs or from ordering.

**Contradictions.**
- (none identified)

**Gaps.**
- No literature addresses attribution of commingled committee cash to sources. Look to forensic accounting and legal commingled-fund tracing doctrine for candidate rules.
- There is no sensitivity analysis of attributed totals to the same-day ordering assumption or to FIFO versus proportional rules, which is the component's own empirical question.
- There is no diagnosis of why the first application failed the cash gate.

**Paul's position vs the evidence.**

Paul says there is no literature, and the corpus agrees: zero bearings. The method's discipline (ordered events, cent-level conservation, withhold on unresolved inputs) is sound engineering. Its central assumption, fungibility with proportional shares, is a modelling choice with no external validation.

Attributed figures should be presented as conditional estimates, named with their rule and ordering assumption.

**Open questions.**
- How far apart are proportional and FIFO attributed totals for the largest roster companies in the 2024 cycle?
- Is the cash-gate failure an ordering artifact or a missing-input artifact?

**Read first.** [[christopher-2016-mathematical-modeling-analysis]]

## PAC transfer graph and beneficiary direction partition

**What it computes.** PAC -> PAC -> candidate flows, and support/oppose/party four-bucket attribution (Dem/Rep = downstream - oppose).

**Method.** 89% of beneficiary dollars were oppose. The attributed-estimate producer is unbuilt.

**Code refs.** `fec_transfer_graph.py`, `C:/Users/pgoff/Mentat/mentat/R/report.R`

**Assumptions.**
- Direction codes identify support vs oppose

**Evidence rows:** 7 (VERIFIED-SOFTENED 7)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[gaynor-2022-financial-ties-bind]] | 2022 | method-source | Each member-to-member LPAC donation is a directed edge. In the 115th Congress 282 members headed an LPAC, 373 received LPAC money, there are 12,870 unique directed edges, and the average donation is $3,682.19 per tie. Source is OpenSecrets compiled from FEC records. | The R functions default to cycles 2022, 2024, 2026. Gaynor's 115th and 116th windows need the 2018 and 2020 cycles, which the defaults do not cover; the cycles argument can override. Sponsor name-key matching can give false or missing links. Any such edge is a donation record, not an access or influence edge (D-025), and must name its cycle window and denominator. | moderate | VERIFIED-SOFTENED |
| [[irvin-2023-dark-investigation-dark]] | 2023 | qualifies | Two high-opacity C4s are described as moving money into political committees as grants. One Nation gave 21.7 million dollars to the Senate Leadership Fund, a PAC at the same address, out of 63.3 million spent in 2016. Freedom Frontier made a 4.4 million dollar grant to a PAC, with almost 75 percent of its expenses political. The paper presents these as anecdotes, not a sample-wide pattern. Grants between C4s are not classified as political, so such flows fall outside its C6 component. | The structural point holds for the graph as read. The paper covers 2016 economic-policy C4s and gives no size estimate for C4 money reaching Super PACs, so no magnitude may be inferred. Mentat scope (2022-2026 industry PACs) was not checked here. | moderate | VERIFIED-SOFTENED |
| [[irvin-2023-dark-investigation-dark]] | 2023 | supports | Beckel 2018, cited and not re-analysed, is reported as showing that political spending routed through dark money organizations is strongly negative, asking voters to defeat candidates, per FEC filings. Section 12 calls the dark-money media financing overwhelmingly negative. The paper gives no count or share, and the claim concerns dark money vehicles in general, not this C4 sample specifically. | Qualitative, second-hand, 2016 C4-linked spending. Mentat's beneficiary split is a modelled conditional scenario for a different period and denominator. At most directional agreement; no numeric confirmation. | weak | VERIFIED-SOFTENED |
| [[christopher-2016-mathematical-modeling-analysis]] | 2016 | suggests-extension | Money-flow network among politically active 501(c) nonprofits and SMLLCs. A tie means funds were exchanged, tie width is dollar amount, and node size is betweenness (Figure 1). The top-ranked organization, Freedom Partners Chamber of Commerce, has out-degree 90 (Table 5). The paper does not state how its network data were built: its background sources are news and IRS material from 2010-2014, and the network comes from earlier unpublished work. | Scope statement only. Money that passes through a non-disclosing nonprofit before reaching a committee is invisible to Mentat's committee graph. The paper gives no data-construction method, so its network cannot validate any edge set. Its network is a single partisan donor network from about 2010-2015. Do not import its ranking as evidence about any industry; any key-node ranking on Mentat's graph is a heuristic output (D-025). | moderate | VERIFIED-SOFTENED |
| [[anon-2025-cross-industry-political]] | 2025 | qualifies | Mutual-fund Super PAC money goes particularly to PACs aligned with the GOP (memo wording; the memo does not say mostly). The memo calls this a vast reservoir of partisan spending. House and Senate Majority PAC donations linked to JPMorgan are +90% from the company rather than individuals (memo wording, no denominator). Credit unions report $0 dark money to the four biggest Super PACs. | Quote the memo's partisan and dark-money readings only after recomputing with the support/oppose split and the 501c and 527 separation. The attributed-estimate producer status claimed by the analyst was not checked. | moderate | VERIFIED-SOFTENED |
| [[chohan-2025-crypto-oligarchy-cryptocurrency]] | 2025 | supports | Fairshake, Protect Progress and Defend American Jobs ran ads that often did not mention cryptocurrency, focusing on candidate integrity or economic policy. Named races: Fairshake backed Moreno against Brown in Ohio and opposed Tester in Montana. The paper also cites opposition to Porter in a California Senate primary and to Bowman's re-election bid. Coinbase, Ripple and Andreessen Horowitz each gave tens of millions of dollars, with no exact figures. No oppose or support split of dollars is given. | Qualitative evidence for federal races in 2024 only. It supports keeping support and oppose separate rather than netting them, and supports the point that ad topic cannot be read from ad content. It does not support any oppose share, and the paper's own examples include backing candidates as well as opposing them. | weak | VERIFIED-SOFTENED |
| [[goff-nd-cross-industry-finance]] |  | suggests-extension | Super PACs take 71.9 percent of crypto non-direct-to-candidate giving, which is 70 percent of industry giving; Fairshake is the 2nd largest super PAC and wholly crypto-funded; backed candidates won 38 of 40 primaries. | The 38 of 40 count has no window, no denominator of contested races and no baseline win rate. The 71.9 and 70 percent shares have no stated cycle or source. Reuse needs the support and oppose split by party and a stated cycle. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence:** 7 bearings, all VERIFIED-SOFTENED. They are mainly scope limits and qualitative direction claims. None is quantitative confirmation.

**Scope (moderate):**
- **Irvin 2023** (2016 economic-policy C4s, anecdotes): One Nation gave $21.7M of $63.3M in 2016 spending to the Senate Leadership Fund, and Freedom Frontier made a $4.4M grant.
- **Christopher 2016:** its nonprofit network has an unstated construction method and covers about 2010-2015.
- Mentat's graph reads only OTH 24G and 24K committee transfers, with C-plus-eight-digit node IDs. A 501(c)(4) upstream of a Super PAC therefore appears only as a receipt, and money that passes through non-disclosing nonprofits is invisible. This is a scope statement, not a magnitude: neither paper estimates how much C4 money reaches Super PACs.
- Christopher's betweenness ranking must not be imported, and any key-node ranking on Mentat's graph is a heuristic model output (D-025).

**Gaynor 2022** (method-source, moderate):
- It treats member-to-member LPAC giving as directed edges. 115th Congress figures: 12,870 unique directed edges and an average of $3,682.19 per tie.
- Mentat's R readers can produce such edges but default to cycles 2022-2026. Gaynor's window needs 2018 and 2020.
- Such edges are donation records, not access or influence edges (D-025).

**Direction:**
- **Chohan 2025** (weak, 2024 federal races, qualitative): crypto Super PAC ads often did not mention crypto, and the PACs both backed and opposed candidates (Moreno backed; Brown, Tester, Porter and Bowman opposed). This supports keeping support and oppose separate rather than netting them, and supports not reading the issue from ad content. It gives no oppose share.
- **Irvin 2023's second bearing** relays Beckel 2018 second-hand: dark-money spending is "strongly negative". There is no count or share, so at most it shows directional agreement.
- **anon-2025 memo and goff-nd** give partisan and Super PAC shares without windows or denominators. Examples: 71.9% and 70% for crypto, 38 of 40 primaries won with no baseline, and JPMorgan-linked "+90%".

**The 89% figure.**
- The component's "89% of beneficiary dollars were oppose" was not found in fec_transfer_graph.py, report.R or the brain folder.
- The only trace is an R/export_guards.R:278 comment: 89% of one shipped beneficiary sheet's *rows* were opposition-direction.
- Until it is recomputed with a stated cycle, industry and dollar denominator, it is a row share from one sheet.

The attributed-estimate producer is unbuilt; the route returns 409.

**Contradictions.**
- The component states that 89% of beneficiary *dollars* were oppose. The only located source (R/export_guards.R:278 comment) says 89% of one sheet's *rows* were opposition-direction. The figure was not found in fec_transfer_graph.py, report.R or the brain folder. Rows are not dollars, and one sheet is not the industry total.
- The goff-nd and anon-2025 shares (71.9%, 70%, 38 of 40, +90%) carry no cycle window or denominator. They cannot be reconciled with Mentat's support, oppose and party partition as stated.

**Gaps.**
- No quantitative study of how industry Super PAC money divides into support and oppose by party is in the corpus. Recompute from FEC Schedule E with support and oppose codes, by cycle and industry, and audit a sample of IE records to check that the direction codes match ad content.
- There is no estimate of how much non-disclosing nonprofit money enters the committee graph upstream of industry PACs. Form 990 grant data linked to FEC committee IDs would bound it.
- Leadership-PAC edges need 2018-2020 cycles and a validated sponsor-to-member key (name-key matching can give false or missing links).

**Paul's position vs the evidence.**

Paul cites Gaynor 2022, Irvin 2023 and Couch, Fox & Everton 2016 (the same work as the christopher-2016 slug). He ties dark-money giving to Furnas's ideological infrastructure and notes that no annotation addresses PAC flows.

The evidence supports using Gaynor-style directed edges as donation records. It shows that the dark-money papers mark a boundary of Mentat's graph rather than validating its edges or rankings.

On direction, the qualitative sources agree that negative spending is common. None supports a specific oppose share. The 89% figure Paul's system carries is unconfirmed as a dollar share.

**Open questions.**
- What is the support, oppose and party split by dollars, with cycle and industry named, once recomputed from Schedule E?
- Does the export_guards.R:278 89% refer to rows of a single industry's sheet, and which cycle?
- Should the beneficiary UI label the C4 and nonprofit upstream boundary explicitly?

**Read first.** [[gaynor-2022-financial-ties-bind]], [[irvin-2023-dark-investigation-dark]], [[chohan-2025-crypto-oligarchy-cryptocurrency]], [[christopher-2016-mathematical-modeling-analysis]], [[anon-2025-cross-industry-political]], [[goff-nd-cross-industry-finance]]

## Collection completeness (zero provenance, count reconciliation, key pool)

**What it computes.** Verified zeros, count reconciliation, and pagination-shortfall rejection; key rotation with an hourly budget.

**Method.** Only the browser lane stamps zero_verified. Three reconcile passes. The per-key vs per-IP rate limit is unverified.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/fec_api.R`, `C:/Users/pgoff/Mentat/mentat/R/fec_page.R:158`, `C:/Users/pgoff/Mentat/mentat/tools/api_request_pool.py`

**Assumptions.**
- The FEC count is the completeness target

**Evidence rows:** 1 (VERIFIED-SOFTENED 1)

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bouton-2022-small-campaign-donors]] | 2022 | measurement-source | Conduits must report every contribution they collect, whatever its amount. Conduit contributions were 76.4 percent of individual contributions 2006-2020 and 91.1 percent in 2020, and processing them more than doubles observed contributions. WinRed launched in 2019. Committee-reported unitemized totals overstate small-donor contributions by a factor of 1.8 to 3.1. | Matters for cross-cycle or partisan splits of individual employee giving that span the 2019 WinRed launch. The paper limits its Republican analysis to 2020. It does not affect PAC-to-candidate totals. Direct gifts under 200 dollars stay invisible in itemized data (paper footnote 12). | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Near-GAP.** There is 1 VERIFIED-SOFTENED bearing (moderate), and it bears on what itemized data can contain, not on openFEC API completeness.

**Bouton et al. 2022:**
- Conduits must report every contribution whatever its size.
- Conduit contributions were 76.4% of individual contributions in 2006-2020 and 91.1% in 2020. Processing them more than doubles observed contributions.
- Committee-reported unitemized totals overstate small-donor contributions by 1.8 to 3.1 times.
- Direct gifts under $200 stay invisible in itemized data.

**Fit with Mentat:**
- Mentat flags conduit legs (collect.R:1452-1454).
- It treats an empty Conduit category as a real zero (export_guards.R:447-449).
- It applies no coverage adjustment that varies by cycle or party.
- So a "verified zero" for conduit giving in a pre-2019 Republican-leaning cycle can reflect platform absence (no WinRed), not absence of giving. This matters for cross-cycle and partisan splits of employee giving, not for PAC-to-candidate totals.

**Nothing in the corpus addresses:**
- openFEC recordsTotal misreporting;
- false zeros relative to bulk files;
- pagination shortfalls;
- whether the rate limit is per key or per IP.

**Study needed:** a systematic reconciliation of openFEC API counts and sums against FEC bulk files for a sample of committees and cycles. Measure the false-zero and short-count rates by endpoint, and test the rate-limit unit on a single key and IP pair at normal load (logs only, no stress test).

**Contradictions.**
- (none identified)

**Gaps.**
- No study measures openFEC recordsTotal accuracy or false zeros against bulk files. An API versus bulk reconciliation sample is needed.
- There is no evidence on whether the openFEC rate limit is per key or per IP. Read the API documentation and inspect existing logs rather than running load tests.
- There is no cycle- or party-varying coverage label for conduit zeros before WinRed (2019).

**Paul's position vs the evidence.**

Paul says there is no literature. The corpus agrees on the API-completeness question. Bouton adds an adjacent point Paul's position does not address: for pre-2019 Republican employee giving, a verified zero in conduit-routed money can be a platform-coverage artifact. Paul's completeness target ("the FEC count") is sound for API fidelity, but it does not guarantee economic completeness.

**Open questions.**
- How often does openFEC return zero rows where bulk files show receipts, by endpoint and cycle?
- Should conduit zeros in cycles before 2020 carry a coverage note on surfaces that split by party?

**Read first.** [[bouton-2022-small-campaign-donors]], [[bonica-2025-money-campaign-finance]]

## Across components

**1. Bouton et al. 2022 cuts across three components.** It is the single most consequential source for this system:
- **Identity ladder:** it is a stricter rule design than Mentat's.
- **Dedup and versions:** it treats the committee leg of an earmarked gift as a duplicate of the conduit leg.
- **Collection completeness:** it finds that conduit coverage changes with WinRed's launch (2019).

It also bears on how receipt roles are read. Read it first.

**2. The coverage break around 2019-2020.** Bonica 2025 and Bouton 2022 independently place a break in small-donor and conduit coverage around 2019-2020. Any Mentat surface that compares employee giving, or its partisan share, across 2018 and 2020 carries a reporting artifact. It needs a coverage label naming the window. PAC-to-candidate totals and the 2024 anchor cycle are unaffected.

**3. Benchmarks are missing everywhere.** Every component that matches, dedups or attributes lacks a measured error rate, and the literature cannot supply one:
- Kim 2020 and 2023, Bouton 2022 and Bonica 2025 all report no precision or recall for their own linkers.
- The common fix is one hand-coded audit programme. One labelled sample of Schedule A rows, with truth for employer, identity and role, would answer the empirical questions of employer-match, identity-ladder and receipt-role together.

**4. Precision-first is consistent across the literature but asymmetric in cost for Mentat.**
- **Precision:** a false merge or false employer match puts a stranger's money in a company total, so Mentat's conservative posture is justified.
- **Recall:** the recall cost is unmeasured. The AUTO rung (exact name plus same zip, no street) is actually looser than published rule designs.

**5. Numbers with no window or denominator.** Several figures circulating in Mentat's documentation and in the two non-independent sources must not be quoted until they are recomputed with a cycle window and denominator:
- 89% oppose (rows of one sheet, not dollars);
- 71.9% and 70% Super PAC shares;
- 38 of 40 primaries;
- +90% (JPMorgan-linked);
- 98.8% C-suite.

**6. Graph and edge wording.**
- Edges built from LPAC, PAC or Super PAC transfers are donation records.
- Any centrality or key-node ranking on them is a heuristic model output, never an access, closeness or influence measure (D-025).
- LDA stays an identity source, not an edge or weight source (D-021).

None of the bearings proposes dropping cosponsorship edges or per-edge volume normalization, so no rejected SNA proposal is in play here.

**7. Change control.** Receipt-counting and money collection are frozen. Every bearing that implies a code change (street in AUTO, frequency weighting, earmark-leg dedup, 501(c) labelling) is a measurement question first and a ruling for Paul second. This assessment recommends measuring, not changing.

## Research agenda

1. Earmark double count (Bouton 2022 versus guard G3's removal under R5/R8). Count the committee-itemized earmarked rows that have a conduit twin, and their dollars, per industry and cycle. Check whether the twin pairs share a sub_id or tran_id. Measurement only; any change is Paul's.
2. Hand-coded audit sample. Build one labelled sample of Schedule A rows covering employer truth, person identity and receipt role, stratified by company size, surname frequency and exclusion reason. Use it to estimate employer-match precision and recall, the false-merge rate by ladder rung, and the true misattribution share among excluded rows (excluding the CITIZENS/CITI block).
3. AUTO-rung stress check from existing data. Count AUTO merges that involve suffix-free same-name pairs at the same zip and employer, and high-frequency surnames in dense zips. Estimate how company totals would move if street agreement were required. Report only.
4. Cash-proportional sensitivity. Recompute attributed PAC totals under proportional, FIFO, LIFO and lowest-intermediate-balance rules, and under alternative same-day orderings, for 2024. Report the spread per company as an uncertainty band. Also diagnose why the first application failed the cash gate.
5. The 89% oppose figure. Find its source (the R/export_guards.R:278 comment), then recompute the support, oppose and party split in dollars from Schedule E with the cycle and industry named. Retire the 89% dollar wording until that is done.
6. FEC amendment-chain census from bulk files. Measure how often amendments change the amount, the date or the tran_id, and compare totals under count_latest with tran_id-level resolution. Test a sub_id or tran_id key against the off-by-cents memo-parent defect.
7. openFEC versus bulk reconciliation. Measure false-zero and short-count rates by endpoint and cycle for a committee sample. Settle per-key versus per-IP rate limiting from the documentation and existing logs, with no load testing.
8. Coverage labels. Add a coverage-break note naming the window to any cross-cycle or partisan split of employee or conduit giving that spans 2018-2020 (Bonica 2025, Bouton 2022). This is display wording for Paul to approve.
9. Literature search to fill the gaps. Look at forensic-accounting and legal commingled-fund tracing (for cash attribution), FEC line-number and memo-code documentation (for receipt role), and any published precision and recall for fastLink on FEC donors (for the identity ladder).
10. Extension leads, all UNVERIFIED until a second path reproduces them (DE-062). (a) A Fremeth-style test of CEO substitution when the PAC is at its limit, on Chief-tier gifts. (b) A verified-affiliation rule linking executive foundations and donor-advised funds to roster people, consistent with strict-names. (c) Leadership-PAC donation edges for 2018-2020, labelled as donation records only (D-025).
