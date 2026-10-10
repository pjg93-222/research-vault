---
type: system
name: Mentat FEC money attribution and accounting
slug: mentat-fec-attribution
components: [mentat-fec-attribution.employer-match, mentat-fec-attribution.identity-ladder, mentat-fec-attribution.receipt-role-and-exclusion, mentat-fec-attribution.dedup-and-versions, mentat-fec-attribution.cash-proportional-attribution, mentat-fec-attribution.transfer-graph-and-direction, mentat-fec-attribution.collection-completeness]
brain_notes: [actors/mentat-fec-pipeline, concepts/mentat-fec-accounting-methodology, concepts/exclude-on-contradiction-not-ambiguity, concepts/pac-cash-proportional-attribution, actors/mentat-fec-pipeline-history-audit-r12]
paper_count: 13
tags: [system]
---

# Mentat FEC money attribution and accounting

The R/Python pipeline (C:/Users/pgoff/Mentat) that attributes FEC Schedule A, Schedule B and PAS2 money to roster companies and industries. Covers employer and identity matching, receipt-role classification, contradiction-only exclusion, conduit and memo dedup, filing-version identity, the PAC transfer graph, cash-proportional attribution, beneficiary direction, geography and collection completeness.

**Brain notes:** [actors/mentat-fec-pipeline](obsidian://open?vault=brain&file=actors/mentat-fec-pipeline); [concepts/mentat-fec-accounting-methodology](obsidian://open?vault=brain&file=concepts/mentat-fec-accounting-methodology); [concepts/exclude-on-contradiction-not-ambiguity](obsidian://open?vault=brain&file=concepts/exclude-on-contradiction-not-ambiguity); [concepts/pac-cash-proportional-attribution](obsidian://open?vault=brain&file=concepts/pac-cash-proportional-attribution); [actors/mentat-fec-pipeline-history-audit-r12](obsidian://open?vault=brain&file=actors/mentat-fec-pipeline-history-audit-r12)

**What the evidence base is.** The hub has 20 evidence rows from 13 papers. 2 are VERIFIED, both narrow: Bouton 2022 on identity rules, and Paul's own cross-industry report on C-suite shares. The other 18 are VERIFIED-SOFTENED, and none is UNVERIFIED. No row validates a Mentat number. Every row is either a design reference (how someone else matched, deduped or traced) or a scope statement (what FEC data cannot see). Not one paper reports precision or recall for a linkage that Mentat could use as a benchmark. Paul's own report and the anonymous memo are not independent evidence. Each has unstated windows and denominators.

**Where the evidence matters:**
- **Identity ladder.** This is the best-covered component, and the evidence cuts against the ladder. Bouton et al. (2022) is the only published rule-based donor grouping in the set. It requires exact first name, surname and ZIP plus a fuzzy street match, and it rejects employer and occupation as grouping keys. Mentat grants AUTO on exact name plus ZIP without reading street, which is looser than that benchmark. Paul's stated goal is to limit false positives.
- **Conduit and memo dedup.** Bouton (2022) shows that an earmarked gift over $200 appears twice, once in the conduit report and once in the recipient's report. Bouton paired 87% of the recipient-side copies with their conduit twins. Mentat counts conduit legs as real rows and removed guard G3 on 2026-07-27, so a double count is plausible. Its size is unmeasured.
- **Cross-cycle individual giving.** Bonica (2025) and Bouton (2022) both document a reporting break at the 2019-2020 WinRed launch. Any partisan or level comparison of employee giving that spans 2018 to 2020 contains a reporting artifact.
- **Transfer graph.** The evidence is a scope statement: money routed through 501(c)(4)s and LLCs is invisible upstream of the first committee. The "89% oppose" figure is not reproducible as stated. The only trace is a code comment about opposition-direction rows in one shipped sheet, not dollars system-wide.

**Gaps.** PAC cash proportional attribution has no literature, and collection completeness has one indirect row. A web scan found no published method for proportional tracing of pooled PAC cash. The right fill for both is an internal sensitivity study or reconciliation study, not more papers. Receipt-role exclusion has only an indirect bearing, so its fill is a manual audit of excluded rows.

**Actual errors to name:**
1. The CITIZENS-read-as-CITI prefix bug produced $29.3M of the $32.5M excluded in the Banking run, with the window and denominator from the hub. This shows that prefix-token matching breaks the strict-names rule. It is a matching defect, not an exclusion-policy result.
2. The 89% figure is rows-versus-dollars ambiguous and unsourced in the cited files.
3. The two Kim rows disagree on whether SOC 27-3030 appears in the paper. The 2023 row calls it refuted, and the version cited varies between 2020, 2023 and JOP 2026.
4. The known $176,942.05 off-by-cents memo-parent double count in Crypto is still open.

None of this recommends a change to the receipt-counting code, which is frozen. Every change below is a per-change ruling for Paul. The studies proposed here are measurements only.

## Employer-string roster matching and overrides

**Computes.** Assigns Schedule A rows to roster companies by normalized employer key, prefix tokens and context; override rules use maximum-specificity matching.

**Method.** Strict-names rule (roster plus verified aliases only). Acronym collisions are open owner calls.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/collect.R:363-1080`, `C:/Users/pgoff/Mentat/mentat/R/ingest.R:43-205`

**Assumptions.**
- Employer text identifies the employer
- Unlisted spellings are unmatched

**Evidence** (3 rows, 3 papers; 1 VERIFIED, 2 VERIFIED-SOFTENED).

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[furnas-nd-conflating-lobbying-pacs]] |  | measurement-source | Organizational giving is campaign donations from the organization or its officially affiliated PAC; contributions by employees and contract lobbyists are not counted. Dark-money contributions are not observed. Independent expenditures (often by Super PACs) are not discussed; the article covers contributions to candidates only. | Comparing Mentat industry totals or participation counts with the paper's overlap or distribution figures requires restricting Mentat to organizational and affiliated-PAC receipts first. Melange's opposition prior already records that employee/executive giving and industry PAC money (congestion.json) are different instruments, with lean signs agreeing for only 157/276 members (57%) (fec/lobbying/build_opposition_prior.py:113). | moderate | VERIFIED-SOFTENED |
| [[anon-2025-cross-industry-political]] | 2025 | suggests-extension | Charles Schwab's company PAC and executives halted giving. Per the memo, Chairman Schwab and his wife moved giving to a charity fund that invested over $250M in conservative causes in 2021-2024 and gave to Republican Super PACs (memo figures, partly redacted). The memo's thesis is that spending is offloaded onto company executives, measured as CEO and executive averages per industry. | An extension would link executive-controlled vehicles (foundations, donor-advised funds, family entities) to roster people. That needs a verified-affiliation rule consistent with the strict-names policy, not a fuzzy guess. The memo never defines executive, so its averages cannot be reconciled with the Chief or Middle Management tiers. | weak | VERIFIED-SOFTENED |
| [[goff-nd-cross-industry-finance]] |  | suggests-extension | The C-suite share of crypto donations rose from 69 percent in 2021-22 to 98.8 percent in 2023-24; the report frames the credit union shortfall as missing employee and middle-management giving. | An occupation-role split would need a hand-coded precision check. The report gives no coding rule for C-suite, so its 69 and 98.8 percent figures cannot be reproduced from the extract. | moderate | VERIFIED |

**Assessment.**

**Evidence weight: weak to moderate. Nothing benchmarks Mentat.**

- **Kim, Stuckatz & Wolters (2020).** The row is filed under identity, but it describes employer matching. It matches PAC and employer names to Compustat gvkey and checks the matches against hand-coded ground truth, yet it reports no precision or recall. It is a design reference, not a standard to beat.
- **Furnas, LaPira & Brock (n.d., VERIFIED-SOFTENED, moderate).** This is the most consequential row. It defines organisational giving as the organisation plus its affiliated PAC and excludes employees. Mentat company totals include employee giving matched by employer. That is a definitional difference, not an error, but Mentat must be restricted to organisational and affiliated-PAC rows before any comparison with that literature. Melange already records that the two instruments' lean signs agree for only 157 of 276 members (57%; per fec/lobbying/build_opposition_prior.py:113, window not stated there).
- **Paul's report (goff-nd, VERIFIED) and the anonymous memo (weak).** Both propose role splits (C-suite share) and executive-controlled vehicles such as foundations and donor-advised funds. Neither gives a coding rule, so the 69% to 98.8% C-suite shift cannot be reproduced.

**Internal contradiction.** The component's method says "strict names (roster plus verified aliases only)", but the matcher also uses prefix tokens. The Banking run excluded $32.5M, and $29.3M of it came from one prefix collision (CITIZENS read as CITI). That is a precision failure of prefix matching, caught downstream by exclusion. So "more conservative than the literature" is a claim about policy, not about measured behaviour.

**Online scan lead (UNVERIFIED).** Stuckatz, "Political alignment between firms and employees" (PSRM), has an appendix section on cross-validating firm-code matches. Its numbers were not confirmed.

**Contradictions.**
- The method claims strict names only, but prefix-token matching produced the CITIZENS to CITI collision. That collision was $29.3M of the $32.5M excluded in the Banking run.
- Furnas et al. exclude employee giving from 'organizational giving', while Mentat company totals include it. Figures are not comparable without restriction.
- The Kim/Stuckatz/Wolters citation appears as 2020 (positions), 2023 (evidence) and JOP 2026 (system citations). The SOC 27-3030 claim is VERIFIED-SOFTENED in the 2020 row and called refuted in the 2023 row.

**Gaps.**
- No precision or recall estimate for employer-string matching. Fill it with a hand-coded random sample of Schedule A rows, stratified by company size and by match path (exact key, verified alias, prefix token, override), scoring false matches and missed spellings.
- No coding rule for executive or C-suite roles that can be checked against report.R spending_title_role.

**Paul's position vs the evidence.** Paul highlighted FEC employer matching to Compustat gvkey, SOC executive codes and hand-coded ground truth. Mentat's strict-names rule is more conservative. Paul points to gvkey/SOC matching with hand-coded ground truth and calls Mentat's strict-names rule more conservative. The papers he cites never publish their accuracy, so there is no number for Mentat to be more conservative than. Mentat's own prefix path broke the strict-names rule at scale in Banking. The position holds as policy intent, but it is unmeasured as behaviour.

**Open questions.**
- Is prefix-token matching still live after the CITIZENS fix, and is it limited to verified aliases?
- Should company totals show organisational/PAC giving and employee giving as separate, never-summed channels to match the Furnas definition?
- Which Kim/Stuckatz/Wolters version is the citation of record, and does it contain SOC 27-3030?

**Read first.** [[kim-2020-strategic-sequential-links]], [[furnas-nd-conflating-lobbying-pacs]], [[goff-nd-cross-industry-finance]], [[anon-2025-cross-industry-political]]

## Contributor identity match ladder

**Computes.** AUTO, HIGH, REVIEW or DISCARD for same-name candidates by name parts, zip, city, state, occupation and employer contradiction.

**Method.** Ordered rules (exact name + zip -> AUTO, etc.). No name-frequency weighting.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/collect.R:1933-2157`

**Assumptions.**
- Same name + zip implies the same person

**Evidence** (6 rows, 6 papers; 1 VERIFIED, 5 VERIFIED-SOFTENED).

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bonica-2025-money-campaign-finance]] | 2025 | method-source | The DIME-to-L2 donor linkage is built to limit false positives. It links 75% of DIME individual contributors from 2012-2020, about 17M donors. Misses come from address differences (most common), donors who are not registered, and name differences. Unlinked donors have slightly more conservative CFscores and larger average gifts. The authors judge these imbalances small and not theoretically important. The r = 0.95 validation against Project Vote Smart (fn. 11) is for the CANDIDATE birth-year linkage, not the donor linkage. | Both pipelines prefer precision to recall, which is consistent with Mentat's fail-closed posture. The paper reports that unlinked donors give slightly larger amounts, and the authors call this unimportant. It is weak evidence at most that precision-first matching under-recovers larger donors, and it does not establish a downward dollar bias in Mentat. Mentat matches contributor rows to one another within an employer roster, which is a different task from voter-file linkage. Neither 75% nor r = 0.95 is a benchmark for Mentat. | weak | VERIFIED-SOFTENED |
| [[kim-2020-strategic-sequential-links]] | 2020 | method-source | FEC has no unique donor IDs (about 53M itemized individual contributions 1980-2018, 4M employer names, about 827,000 occupation names). Authors match PAC and employer names to Compustat gvkey and occupations to SOC codes (executives 11-1011, government relations 27-3030), standardize names with NLP as in Bonica (2017), and run unsupervised constrained graph clustering on address, donation history and targets; employer and occupation matches are cross-validated against a hand-coded ground truth and corporate PAC matches checked and revised manually. | Both work on FEC Schedule A, so the method transfers in kind. The paper reports no precision or recall, so it is a design reference, not a benchmark. Adopting clustering would loosen Mentat's rule-based ladder, and receipt-counting is frozen without per-change OK; any change is Paul's. The transferable piece is a hand-coded labelled sample to measure the ladder's precision and recall. | moderate | VERIFIED-SOFTENED |
| [[bonica-2013-mapping-ideological-marketplace]] | 2013 | measurement-source | Supplement record-linkage section (text lines about 5315-5350, approx PDF p.51): on corporate board-member cases, 57 percent got a single ID, 42 percent split (type 1 errors), and in 13 percent of the split cases the algorithm failed to put 90 percent of records under one ID. Two type 2 errors (wrong merges), both family members at the same employer (Gates Sr and Jr; Walton family). Address and occupation disclosure was significantly lower in the 1980s and early 1990s. Method is fuzzy matching plus supervised machine learning. | The paper's type 2 errors were same-employer family members (the Gates Jr and Sr pair differs by a suffix). Mentat sends any suffix difference to REVIEW, so a missing-suffix pair does not get AUTO under the current rule. The split rates come from a different linker and are a reference point, not an error-rate estimate for Mentat. | moderate | VERIFIED-SOFTENED |
| [[kim-2023-systemic-sequential-links]] | 2023 | method-source | Donor and firm linkage uses constrained graph clustering and fastLink probabilistic record linkage (Enamorado, Fifield and Imai 2019). Executives are classed by SOC 11-1011. The paper reports no precision or recall for its linkage. | The paper confirms the fastLink method and the 11-1011 code only. SOC 27-3030 for government-relations staff is not in the paper text, so that part of the claim is refuted. Suggesting that frequency-weighted fastLink would fix the common-name blind spot is the analyst's opinion, and the paper gives no benchmark. Any change to the ladder is a ruling for Paul under D-040 (not checked here). | weak | VERIFIED-SOFTENED |
| [[porter-2025-campaignview-campaign-platforms]] | 2025 | context | Cross-cycle candidate names linked by probabilistic record linkage on pretrained embeddings, blocked on party and state. Estimated matches under 50% were hand-checked: 15 wrong for 2018-2020, 1 for 2020-2022, none for 2018-2022. | The paper links candidates (a small, ID-rich population), not donors; its error counts do not speak to donor-level false-match rates. Member bioguide lookup is a different problem from cross-cycle name linkage. | weak | VERIFIED-SOFTENED |
| [[bouton-2022-small-campaign-donors]] | 2022 | method-source | Donor grouping uses explicit rules. Two records are one person if they match exactly on three of first name, surname, full street and ZIP and fuzzily on the fourth (first-name variant index, fuzzy surname, fuzzy street, or city-state in place of ZIP). Occupation and employer are too inconsistent to use as grouping variables. The authors expect a very small false-positive rate but do not measure it. | The paper groups across all committees; Mentat links same-name rows to a seed within a company run (seed logic not traced in this check). A false merge adds a stranger's money to a company total, so the cost is higher. The paper's rules have no measured precision, so they are a design benchmark, not a validated standard. | moderate | VERIFIED |

**Assessment.**

**Evidence weight: moderate. The direction is against the ladder's precision claim.** Six papers bear on this component (the table gives each row's status). Porter 2025 is context only, because it links candidates, not donors.

- **Bouton et al. (2022).** This is the closest analogue: explicit rules on FEC donors. To count two records as one person it requires an exact match on three of first name, surname, street and ZIP, plus a fuzzy match on the fourth. It rejects occupation and employer as too inconsistent. Mentat grants AUTO on exact name plus ZIP, with no street check (street is collected at collect.R:1415-1416 but never read). It grants HIGH on exact name plus city. Both are looser than Bouton. Bouton's own false-positive rate is unmeasured, so the comparison is about design, not a validated standard. A false merge in Mentat adds a stranger's money to a company total, which raises the cost of looseness.
- **Bonica (2013).** Its two wrong merges were same-employer family members (Gates Sr and Jr; Walton). Mentat's suffix-mismatch rule sends any suffix difference to REVIEW, which covers the suffixed case. A father and son in the same ZIP who both omit a suffix would still get AUTO.
- **Kim (2020, 2023) and Bonica (2025).** These use constrained clustering, fastLink or voter-file linkage, all precision-first. None reports precision or recall usable for Mentat. The 75% link rate and r = 0.95 in Bonica 2025 are not Mentat benchmarks; the 0.95 is a candidate-level figure. That unlinked donors give slightly larger amounts is weak evidence that precision-first matching under-recovers big donors. It does not establish a downward dollar bias.
- **No name-frequency weighting.** Common names in dense ZIPs are the known blind spot. That fastLink would fix it is analyst opinion, not a paper result.

**Online scan lead.** Enamorado, Fifield & Imai (2019, APSR), the fastLink paper, is the standard frequency-weighted alternative. It is a lead only and has not been verified against Mentat.

**Contradictions.**
- Paul's stated aim is to limit false positives, but AUTO on exact name plus ZIP without street is looser than the one published rule-based FEC donor benchmark (Bouton et al. 2022).
- Bouton et al. reject employer as a grouping variable, while Mentat uses employer contradiction to change the grade, from AUTO to HIGH or from a name match to REVIEW. Mentat only downgrades on employer, so the conflict is mild.

**Gaps.**
- No measured false-merge rate for AUTO and HIGH. Fill it with a hand-labelled sample of AUTO and HIGH pairs, stratified by surname frequency and ZIP population. Score each pair against street address and other public evidence. Compare the ladder, Bouton's street rule and fastLink on the same sample, as a measurement only.

**Paul's position vs the evidence.** Paul highlighted 'no unique identifiers for individual donors', constrained graph clustering, and algorithms that limit false positive matches. Paul highlights the lack of unique donor IDs and the use of algorithms that limit false positives. The literature agrees on the precision-first posture. Mentat's ladder, however, is less strict than the rule set closest to it, and it has no frequency control. The position is right, but the implementation does not yet meet it.

**Open questions.**
- What share of kept dollars come from AUTO matches on common surnames in high-population ZIPs?
- Should street agreement become an input to AUTO? This is Paul's ruling under the receipt-counting freeze.
- How does the seed logic for same-name expansion within a company run work? It has not been traced.

**Read first.** [[bouton-2022-small-campaign-donors]], [[kim-2020-strategic-sequential-links]], [[bonica-2013-mapping-ideological-marketplace]], [[kim-2023-systemic-sequential-links]], [[bonica-2025-money-campaign-finance]]

## Receipt-role classification and contradiction-only exclusion

**Computes.** Economic role per receipt from form, line and description. A row is excluded only when its own text names another organisation; ambiguous rows are kept with a flag.

**Method.** The Banking run excluded 29,816 rows ($32.5M), of which $29.3M came from one prefix bug (CITIZENS read as CITI). Frozen code (2026-09-27).

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/collect.R:122-261`, `C:/Users/pgoff/Mentat/mentat/tools/cross_reference_engine.py`

**Assumptions.**
- Form/line plus text identify the role
- Ambiguity is not contradiction (D-040)

**Evidence** (1 rows, 1 papers; 1 VERIFIED-SOFTENED).

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[irvin-2023-dark-investigation-dark]] | 2023 | suggests-extension | Opacity scores run from 0.8 (Fix the Debt Coalition) to 8.3 (Majority Forward) on a 0-9 scale. Scores above 7 are described as very opaque, including many large advocacy groups with very small boards, paltry websites and no staff. Scores up to 3 could be characterized as transparent. Donations average 92 percent of revenue. The total index does not predict the OpenSecrets dark-money label once controls are added (N=54, Nagelkerke R2 0.529, only ln total expenses significant). | Any change to the exclusion is Paul's decision; this bearing recommends none. The index is unvalidated in this sample, so at most a display-only heuristic label carrying its 2016 and N=54 window. Form 990 data is published two or more years late and Mentat does not collect it. | weak | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: near zero. Treat this component as a GAP.**

The only bearing is Irvin (2023; weak, suggests-extension). Its opacity index (0-9) does not predict the OpenSecrets dark-money label once controls are added (N = 54, 2016 economic-policy 501(c)(4)s). It therefore cannot justify finer treatment of 501(c) recipients, beyond a display-only heuristic label that carries that window. Paul's cited Furnas, LaPira & Brock (2025) row bears on employer-match definitions, not on the role classifier.

The contradiction-only rule (D-040) is a policy choice, and no paper tests it. The empirical question is internal: on manual audit, how many excluded rows are true misattributions? The Banking run already gives the answer for the largest block. $29.3M of $32.5M excluded was a prefix bug, a matching error that exclusion happened to catch. Without that block the remaining exclusion base is about $3.2M, but no audit has measured what share of it is true misattribution.

The date of the 501c/527 exclusion ruling (2026-07-18) could not be verified from the files checked.

Literature is not needed here. An audit is.

**Contradictions.**
- The large majority of Banking-run exclusions ($29.3M of $32.5M) came from an upstream matching bug, not from the contradiction rule. The headline exclusion total says little about how the exclusion rule performs.

**Gaps.**
- Fill it with a manual audit of a stratified random sample of excluded rows, by exclusion reason, after the CITIZENS fix. Report the share that are true misattributions, and the share of kept-but-flagged ambiguous rows that a human would exclude.
- No study of the 501c/527 block exclusion; only Paul's ruling, which is not re-litigated here.

**Paul's position vs the evidence.** No annotation addresses it directly. There is no literature position to compare. The exclude-on-contradiction policy is untested, but nothing in the evidence contradicts it.

**Open questions.**
- What is the post-fix exclusion total and its composition for Banking?
- Where is the 501c/527 exclusion ruling recorded, and on what date?

**Read first.** [[irvin-2023-dark-investigation-dark]], [[furnas-nd-conflating-lobbying-pacs]], [[anon-2025-cross-industry-political]]

## Conduit legs, memo dedup, filing-version identity and transfer rule

**Computes.** Conduit legs are counted; memo legs are deduped by tuple; one winning amendment version per transaction; 24G/24K transfers; PAS2 index v6 count_latest.

**Method.** Known defect: an off-by-cents memo parent ($176,942.05 Crypto double count).

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/schema.R:598-1098`, `C:/Users/pgoff/Mentat/mentat/tools/transfer_rule.py`

**Assumptions.**
- Same amount or date means the same transaction version

**Evidence** (2 rows, 2 papers; 2 VERIFIED-SOFTENED).

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bonica-2025-money-campaign-finance]] | 2025 | measurement-source | Small-donor coverage is near-complete only from 2020. ActBlue and WinRed itemize sub-$200 gifts, and WinRed launched during the 2020 cycle, so earlier cycles lack most small Republican donors. Pre-2020 Republican \|Age Gap\| coefficients come out unexpectedly positive. The authors attribute this to lower age variance among Republican donors, made more extreme by the missing small donors, and to possible partisan differences in age-related preferences. The coverage hole is not the sole cause. | This applies to any Mentat cross-cycle comparison of employee (individual) giving or its partisan share that spans 2018 to 2020. The 2020 jump in itemized Republican small-dollar conduit rows is a reporting artifact, not a behavioural change. It does not affect PAC-to-candidate measures, which are committee transfers, or the current anchor cycle (2024), which falls after the break. | weak | VERIFIED-SOFTENED |
| [[bouton-2022-small-campaign-donors]] | 2022 | qualifies | A recipient committee must itemize an earmarked contribution when the donor's cycle aggregate to it exceeds 200 dollars, so the same gift appears in both the conduit report and the committee report. The authors coupled 87 percent of earmarked committee-itemized donations to their conduit twin and dropped the committee leg. The remaining committee-itemized donations are 3 percent of all contributions. | Paper population is employees whose cycle aggregate to the recipient exceeds 200 dollars. Sub-200 gifts are visible only through the conduit (paper Appendix A.4.3). The size of the double count in Mentat books is unmeasured. Money collection is frozen; any change needs Paul's per-change OK. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: moderate on conduits, none on amendment versions.**

**Conduit double count (Bouton et al. 2022, moderate).** A recipient committee must itemize an earmarked gift once the donor's cycle total to it exceeds $200, so the same gift appears in the conduit's report and the recipient's report. Bouton paired 87% of the recipient-side copies with their conduit twins and dropped the recipient-side copy. Mentat counts conduit legs as real rows under the R5/R8 recipient-as-shown ruling, and guard G3 against conduit double counting was removed on 2026-07-27. If a company's employee gift is matched once on the conduit row and again on the recipient's itemized row, it is counted twice. The size of this in Mentat is unmeasured. This is the most likely live accounting error in the system. It is a measurement question first; any change is Paul's per-change call under the money freeze.

**Cross-cycle break (Bonica 2025 and Bouton 2022, weak to moderate).** Sub-$200 Republican conduit giving becomes visible only from the 2019-2020 WinRed launch. Any 2018-to-2020 comparison of employee giving, its partisan share, or a backtest window spanning the break carries a reporting artifact. PAC-to-candidate transfers and the 2024 anchor cycle are not affected.

**Amendments.** No paper bears on amendment handling (count_latest, the same-amount-or-date rule). The known off-by-cents memo-parent double count in Crypto ($176,942.05) shows the tuple match fails on cent drift.

**Online scan leads (UNVERIFIED).** A 2009 Center for Responsive Politics comment to the FEC says amendments filed in varying ways create duplicate and triplicate records that the FEC cannot always pair with originals. The Follow the Crypto FAQ reports double- and triple-reported in-kind crypto receipts.

**Contradictions.**
- Bouton et al. treat the recipient-side earmarked copy and its conduit twin as one gift, and drop one leg. Mentat counts conduit legs and removed guard G3, so both legs may be counted for the same gift.

**Gaps.**
- Fill the double-count gap with a coupling study. Pair recipient-side earmarked itemized rows with conduit rows on donor, amount, date and recipient, using Bouton's method, inside each company's kept rows. Report the overlapping dollars by industry and cycle.
- Fill the amendment gap with an amendment census from bulk files. For transactions with more than one version, count how often amount, date or both change. Compare count_latest totals against a sum over all versions and a sum over the first version.

**Paul's position vs the evidence.** Paul highlighted late-filed and duplicate filings as a pipeline hazard. Paul flags late-filed and duplicate filings as a hazard, and the evidence agrees. The sharper hazard in the evidence is structural duplication through the conduit's and recipient's reports. That is not late filing, and the current recipient-as-shown ruling may let it through.

**Open questions.**
- Do the conduit copy and the recipient's itemized copy carry different sub_ids, so that report.R's sub_id guard cannot catch them?
- Does any forecast backtest window span the 2019-2020 WinRed break for employee giving?
- Is the Crypto off-by-cents memo parent fixed or only documented?

**Read first.** [[bouton-2022-small-campaign-donors]], [[bonica-2025-money-campaign-finance]]

## PAC cash proportional attribution

**Computes.** A company's share of each PAC payment = its share of labelled cash just before the payment.

**Method.** Ordered events, cent-level conservation, withhold on unresolved inputs. The first application failed the cash gate.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/tools/pac_cash_model.py`

**Assumptions.**
- Cash is fungible within a committee
- Same-day receipts come before payments

**Evidence** (0 rows, 0 papers; none).

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) | | | | | | |

**Assessment.**

**Evidence weight: none. This is a GAP.** No hub row bears on this component. An online scan found no published empirical method for proportional tracing of pooled PAC cash.

The closest analogues are legal, not empirical. *McCutcheon v. FEC* argues that routed money is diluted by everyone else's contributions to the same PAC. A 1989 California FPPC advice letter attributes PAC money to an original contributor only on evidence of control. Both are leads only. They support treating proportional attribution as a conditional accounting convention, not as traced dollars.

The fungibility and same-day-ordering assumptions are choices, not findings. The first application failed the cash gate. There is also a possible inconsistency inside the system. The transfer graph's allocate() spreads funding pro rata over observed outflows without regard to timing, while pac_cash_model uses a time-ordered share of cash just before each payment. The two rules can give different company shares for the same committee.

Literature will not settle this. A sensitivity study will.

**Contradictions.**
- Two proportional rules coexist. allocate() in the transfer graph is time-blind pro rata over outflows, while pac_cash_model is a time-ordered cash share. They are not reconciled anywhere in the evidence.

**Gaps.**
- Fill it with a sensitivity study on the same committees and cycles. Run proportional, FIFO and LIFO, and permute same-day receipt and payment order. Report the share of attributed dollars per company that move by more than a stated threshold, and name the window and denominator.
- No ground truth. Committees with single-source funding periods give natural test cases where every rule must agree.

**Paul's position vs the evidence.** No literature. Paul cites no literature for this component, and none was found. The method has to stand on a conservation proof and a sensitivity range, not on citations.

**Open questions.**
- What failed in the first cash-gate run, and is that failure fixed?
- Should allocate() and pac_cash_model share one attribution rule?

**Read first.** [[christopher-2016-mathematical-modeling-analysis]], [[irvin-2023-dark-investigation-dark]]

## PAC transfer graph and beneficiary direction partition

**Computes.** PAC -> PAC -> candidate flows, and support/oppose/party four-bucket attribution (Dem/Rep = downstream - oppose).

**Method.** 89% of beneficiary dollars were oppose. The attributed-estimate producer is unbuilt.

**Code refs.** `fec_transfer_graph.py`, `C:/Users/pgoff/Mentat/mentat/R/report.R`

**Assumptions.**
- Direction codes identify support vs oppose

**Evidence** (7 rows, 6 papers; 7 VERIFIED-SOFTENED).

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

**Evidence weight: moderate as scope statements. There is no numeric support for any direction share.**

- **Irvin (2023) and Christopher (2016).** Both VERIFIED-SOFTENED. Money passing through 501(c)(4)s or LLCs before reaching a committee gets no node, and appears only as a receipt into the super PAC. Upstream attribution past the first committee is therefore impossible. Neither paper gives a size, so no magnitude may be inferred. Christopher's network has no stated construction method, and its betweenness ranking must not be imported. Any key-node ranking on Mentat's graph is a heuristic model output (D-025), not closeness or access.
- **Gaynor (2022).** Gives a method for directed leadership-PAC edges (115th Congress: 12,870 directed edges, average $3,682.19 per tie, from OpenSecrets data). Replicating it needs the 2018 and 2020 cycles, which the R defaults (2022-2026) skip. Such an edge is a donation record, never an access or influence edge.
- **Direction share.** Chohan (2025) and Irvin's second-hand claim (via Beckel) support keeping support and oppose separate rather than netting them. Chohan's own examples include support as well as oppose, and neither paper gives a share. The "89% of beneficiary dollars were oppose" figure is not in fec_transfer_graph.py or report.R. The only trace is an export_guards.R:278 comment that 89% of one shipped beneficiary sheet was opposition-direction rows. Rows are not dollars, and one sheet is not the system. Until it is recomputed with a cycle window and a dollar denominator, it is an unsourced figure.
- **Paul's report and the anonymous memo.** Figures such as 38 of 40 primaries won, 71.9% and "+90%" have no window, denominator or baseline win rate. They should not be quoted until recomputed.

**Online scan lead (UNVERIFIED).** Demos and U.S. PIRG report that 6.4% of itemized super PAC contributions since 2010 could not be traced to an original source. That is an order-of-magnitude reference for the upstream blind spot.

**Contradictions.**
- The component states that 89% of beneficiary dollars were oppose. The only located source says 89% of rows in one shipped sheet were opposition-direction.
- Chohan (2025) is cited as support for an oppose-heavy picture, but its own examples include large support spending, such as Fairshake backing Moreno.

**Gaps.**
- The attributed-estimate producer is unbuilt; the UI route returns 409.
- No dollar-weighted support/oppose/party split for any industry. Fill it by running the beneficiary partition over named cycles, reporting dollars, and stating the denominator.
- No estimate of upstream non-disclosing money for Mentat's industries. Fill it by matching super PAC receipts from non-committee entities (501(c)s and LLCs) to Mentat industries and reporting their dollar share by cycle.

**Paul's position vs the evidence.** Paul ties dark-money donation behavior to Furnas's ideological infrastructure. No annotation addresses PAC flows. Paul ties dark-money behaviour to Furnas's ideological-infrastructure framing. The evidence supports the structural point that dark money sits upstream and is invisible to a committee graph. It does not support any oppose share or partisan magnitude.

**Open questions.**
- Is 89% a row share or a dollar share, which sheet and which cycle?
- Should leadership-PAC edges (designation D) enter the graph for 2018 and 2020 to allow a Gaynor replication?
- Should super PAC receipts from non-committee entities get an explicit unresolved-upstream bucket?

**Read first.** [[irvin-2023-dark-investigation-dark]], [[gaynor-2022-financial-ties-bind]], [[christopher-2016-mathematical-modeling-analysis]], [[chohan-2025-crypto-oligarchy-cryptocurrency]], [[goff-nd-cross-industry-finance]], [[anon-2025-cross-industry-political]]

## Collection completeness (zero provenance, count reconciliation, key pool)

**Computes.** Verified zeros, count reconciliation, and pagination-shortfall rejection; key rotation with an hourly budget.

**Method.** Only the browser lane stamps zero_verified. Three reconcile passes. The per-key vs per-IP rate limit is unverified.

**Code refs.** `C:/Users/pgoff/Mentat/mentat/R/fec_api.R`, `C:/Users/pgoff/Mentat/mentat/R/fec_page.R:158`, `C:/Users/pgoff/Mentat/mentat/tools/api_request_pool.py`

**Assumptions.**
- The FEC count is the completeness target

**Evidence** (1 rows, 1 papers; 1 VERIFIED-SOFTENED).

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bouton-2022-small-campaign-donors]] | 2022 | measurement-source | Conduits must report every contribution they collect, whatever its amount. Conduit contributions were 76.4 percent of individual contributions 2006-2020 and 91.1 percent in 2020, and processing them more than doubles observed contributions. WinRed launched in 2019. Committee-reported unitemized totals overstate small-donor contributions by a factor of 1.8 to 3.1. | Matters for cross-cycle or partisan splits of individual employee giving that span the 2019 WinRed launch. The paper limits its Republican analysis to 2020. It does not affect PAC-to-candidate totals. Direct gifts under 200 dollars stay invisible in itemized data (paper footnote 12). | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: weak and indirect. Mostly a GAP.**

Bouton et al. (2022; moderate) and Bonica (2025) speak to coverage, not to API completeness:
- Conduits report every contribution whatever its size. Over 2006-2020, 76.4% of individual contributions came through conduits, and 91.1% in 2020.
- Direct gifts under $200 stay invisible.
- WinRed's 2019 launch changes Republican coverage.

These results bear on whether an empty Conduit category is a real zero. export_guards.R:447-449 treats it as a real zero, which is fine only after 2019 for Republican-leaning industries.

No source checks how often openFEC recordsTotal misreports relative to bulk files, or false zeros, or per-key versus per-IP rate limits. These are engineering facts about one API. A reconciliation study settles them; papers will not.

**Contradictions.**
- (none)

**Gaps.**
- Fill the false-zero gap with an API-versus-bulk reconciliation on a random sample of committee-cycles and company queries. Compare recordsTotal, the number of rows returned and the bulk-file row count. Report the false-zero rate and the shortfall rate.
- Fill the rate-limit gap with a documented test from FEC/api.data.gov documentation on whether limits apply per key or per IP. Read the documentation only; no load tests on the machine.

**Paul's position vs the evidence.** No literature. Paul cites no literature for this component, which is appropriate. The evidence adds one caution: treat a zero Conduit category as a real zero only for cycles after the WinRed launch.

**Open questions.**
- Should zero_verified be stamped by lanes other than the browser lane, once a reconciliation has passed?
- Does any industry-cycle cell before 2020 report a Conduit zero that is really a coverage hole?

**Read first.** [[bouton-2022-small-campaign-donors]], [[bonica-2025-money-campaign-finance]]

## Across components

**1. One defect class runs through matching, exclusion and dedup: loose keys.** Prefix-token employer keys produced the CITIZENS to CITI collision ($29.3M of the $32.5M Banking exclusion). The identity ladder grants AUTO without street. The memo tuple fails on cent drift ($176,942.05 in Crypto). In each case the system's stated posture (strict names, precision-first, exact tuple) is stricter than what the code does at the edges. The literature consistently backs the stated posture. The gap is implementation, not policy.

**2. The 2019-2020 WinRed reporting break.** Bonica (2025) and Bouton (2022) agree independently. It affects dedup (conduit volumes), collection completeness (Conduit zeros) and any downstream giving forecast or backtest whose window spans 2018 to 2020 for employee giving. PAC-to-candidate money is unaffected. Any pooled coefficient over employee giving across that break is UNSTRATIFIED unless it is split at 2020.

**3. Two proportional conventions.** pac_cash_model (time-ordered cash share) and allocate() in the transfer graph (time-blind pro rata over outflows) are two different allocation rules. No paper supports either one over the other. They should either be reconciled or labelled per surface.

**4. Instrument definitions.** Furnas et al. define organisational giving without employees. Mentat company totals include employees, and Melange's own prior shows the two instruments' lean signs agree for only 57% of members (157 of 276). Company PAC money and employee money should be shown side by side and never summed when compared with the literature. This matches the lobbying ruling for LDA in-house versus outside firms.

**5. Numbers that must not be quoted as they stand:** the 89% oppose figure (rows from one sheet versus dollars), the memo's "+90%", Paul's report's 38/40, 71.9% and 70%, and Bonica's 75%/r = 0.95 as a Mentat benchmark. None names a usable window and denominator for Mentat.

**6. Online scan leads (UNVERIFIED, single-pass web results).** These are worth fetching, but none is a finding:
- Enamorado, Fifield & Imai (2019), fastLink, APSR (identity).
- Stuckatz, "Political alignment between firms and employees", PSRM, with a supplementary appendix on firm-code cross-validation (employer match): https://www.cambridge.org/core/journals/political-science-research-and-methods/article/political-alignment-between-firms-and-employees-in-the-united-states-evidence-from-a-new-dataset/0BFC112803DD52C47E04DA476B0E0C68
- Demos/U.S. PIRG on untraceable super PAC money, 6.4% of itemized contributions since 2010 (transfer graph): https://www.demos.org/media/some-super-pac-money-untraceable
- 2009 CRP comment letter on amendment duplicates (dedup): https://fec-prod-cms.app.cloud.gov/documents/2439/8-21-09websitecomments.pdf
- Follow the Crypto FAQ on duplicate in-kind crypto receipts (dedup): https://followthecrypto.org/about/faq
- Validation-of-matching framework (identity and employer audits): https://arxiv.org/pdf/1411.0023
- McCutcheon v. FEC on dilution, and a 1989 California FPPC advice letter on attribution by control (cash attribution, legal analogue only): https://www.fppc.ca.gov/siteassets/documents/legal_div/advice_letters/1980-1989/1989/89-071.pdf

**7. Governance.** Receipt counting is frozen. Every study below measures; no change follows without Paul's per-change ruling. The R5/R8 recipient-as-shown and D-040 rulings are Paul's, and they are not re-argued here. The conduit double count is flagged because it may be an actual error under those rulings, not because the rulings are wrong.

## Research agenda

1. Conduit coupling study (dedup). Following Bouton et al. 2022, pair recipient-side earmarked itemized rows with conduit rows inside each company's kept rows. Report overlapping dollars by industry and cycle. This is the most likely live double count.
2. Identity-ladder audit. Hand-label a sample of AUTO and HIGH pairs, stratified by surname frequency and ZIP population. Score the current ladder, Bouton's street rule and fastLink on the same pairs, and report the false-merge rate and the dollars at risk.
3. Employer-match precision and recall. Hand-code Schedule A rows stratified by company size and match path (exact, alias, prefix token, override). Confirm whether prefix-token matching survives the CITIZENS fix.
4. Exclusion audit. Take a stratified sample of excluded rows after the CITIZENS fix, plus flagged-ambiguous kept rows. Report the true-misattribution share and the share a reviewer would exclude.
5. Recompute the beneficiary direction share. Produce a dollar-weighted support/oppose/party split per industry and named cycle, with a stated denominator, to replace the 89% figure, which is either a row share or an unsourced dollar share.
6. Cash-attribution sensitivity. Compare proportional, FIFO and LIFO, permute same-day ordering, and report the share of attributed dollars that moves. Decide whether allocate() and pac_cash_model should share one rule.
7. Amendment census. Using bulk files, count how often amendments change amount versus date, and compare count_latest against alternatives. Include the $176,942.05 Crypto off-by-cents memo parent as a test case.
8. openFEC versus bulk reconciliation. On a random sample of committee-cycles, measure the false-zero and shortfall rates of recordsTotal. Confirm the rate-limit scope from documentation only, with no load tests.
9. Flag the WinRed break. Mark every employee-giving series, partisan share or backtest window that spans 2018 to 2020 as affected by a reporting artifact, or split it at 2020.
10. Upstream blind-spot sizing. Measure the share of industry-linked super PAC receipts that come from non-committee entities (501(c)s and LLCs), by cycle, and label it as unresolved upstream.
11. Citation hygiene. Settle the Kim/Stuckatz/Wolters version of record (2020 working paper, 2023 version, JOP 2026) and whether SOC 27-3030 appears in it. Fetch and verify the online-scan leads before citing any of them.
