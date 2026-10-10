---
type: system
name: Melange lobbying data and LDA linkage
slug: melange-lobbying
components:
  - melange-lobbying.finance-client-classification
  - melange-lobbying.lobbied-member-link
  - melange-lobbying.fec-lda-bridge
  - melange-lobbying.contact-inference
  - melange-lobbying.cosponsor-forecaster
  - melange-lobbying.contested-bills
brain_notes:
  - C:/Users/pgoff/Documents/brain/actors/lobbyview-collector.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-data-collection-lanes-lobbyview-lda-rebuild-and-bridge.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-data-collection-lanes-contact-inference-design-and-evaluation.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-legislative-pillar-acu-review-cosponsor-forecaster.md
paper_count: 23
tags: [system]
---

# Melange lobbying data and LDA linkage

LobbyView/LDA ingestion and its analytic uses: finance-client classification, the finance report join, the member-year lobbying network, the circular lobbied-member link, the FEC-to-LDA bridge test, contact inference, the ridge-logistic cosponsor forecaster, the contested-bills registry and the industry-axes tensor.

Brain notes: [actors/lobbyview-collector](obsidian://open?vault=brain&file=actors/lobbyview-collector); [topics/2026-07-feature-data-collection-lanes-lobbyview-lda-rebuild-and-bridge](obsidian://open?vault=brain&file=topics/2026-07-feature-data-collection-lanes-lobbyview-lda-rebuild-and-bridge); [topics/2026-07-feature-data-collection-lanes-contact-inference-design-and-evaluation](obsidian://open?vault=brain&file=topics/2026-07-feature-data-collection-lanes-contact-inference-design-and-evaluation); [topics/2026-07-feature-legislative-pillar-acu-review-cosponsor-forecaster](obsidian://open?vault=brain&file=topics/2026-07-feature-legislative-pillar-acu-review-cosponsor-forecaster)

## Melange lobbying data and LDA linkage: evidence assessment

**Scope.** 41 evidence rows across 6 components: 7 VERIFIED, 33 VERIFIED-SOFTENED, 1 UNVERIFIED. No component is empty. But the evidence answers a different question in each place. For the **lobbied-member link** it is strong and in agreement. It agrees that LD-2 does not record which members were contacted, so the bill-roster join cannot be read as contact or influence. For the **finance classifier** and the **FEC-LDA bridge** it is moderate, and mostly it qualifies what the system does or warns about hazards. For **contact inference**, the **cosponsor forecaster** and the **contested-bills registry** it only supplies design inputs. None of those three has evidence on its own empirical question (precision against blind labels, lift over baseline, outcome on contested bills). They are therefore validation gaps, even though the hub file has rows for them.

**Three things to say plainly:**
1. **The 'FEC leads LDA by 42 months' wording is not a behavioral lead.** Two rows, both checked against code, find that `lead_months` = 42 is the gap between the latest FEC transaction and the end of LobbyView finance coverage, a data-freshness gap. Within-pair research (Kim, Stuckatz & Wolters) does find donation onset followed by lobbying on the member's bills. That result comes from public manufacturing firms, 2008-2017/18, using a design the bridge does not use. It does not license the 42-month number or predictor language.
2. **The circular roster join comes back through a side door.** Paul's position cites Kim & Kunisky for identifying champions. Kim & Kunisky themselves say that lobbying a bill 'does not necessarily imply political ties to its sponsor'. Separately, `build_dyad_signs` projects signed organization-by-bill positions (Kim et al. 2025) onto sponsor and cosponsor rosters, and the result feeds `signed_non_cu_opposition_z` in `contest_index`. That is the same bill-roster mapping the system has already REFUTED as evidence of influence.
3. **Several system-side figures could not be reproduced from the repo in this pass:**
   - the 265,396 frozen predictions
   - the raw bridge rho of +0.153 (located in project memory, not in a results file)
   - the attribution of ABA $228M vs CUNA $93M (a mixed self-filer-expense plus hired-firm-income total is possible)
   - '22 pinned firms, 56 positions, 1 directional'

   Under DE-062 these stay leads until a second path reproduces them.

**Paul's positions against the evidence.** The evidence supports these:
- LobbyView as the identity backbone
- per-bill (not global) conflict structure
- prediction over reaction

The evidence cuts against or reframes these:
- Harrison is a hazard source on amount semantics, not a pipeline guide.
- Furnas et al. find low organizational overlap between lobbying and PAC giving: 84-88% of lobbying organizations give no PAC money, 1998/2008. That bounds any general 'donations lead lobbying' reading.
- The design-memo position that 'the LobbyView lineage is a cautionary chain because argmax always returns an answer' was **not found in the brain vault**, and the verifier dropped it.

## LDA finance-client classification and report join

**What it computes.** Bank, credit union or other classification with HIGH/LOW confidence. Joined finance reports: 322,473 issue-coded reports; ABA $228M vs CUNA $93M.

**Method.** Name regex with guards. NAICS is linkage metadata only. Fail-closed guards. Vintage 2024-10-25; coverage reliable through 2021.

**Code refs.** `fec/collect_lobbyview.py`, `fec/collect_lda_filings.py`

**Assumptions.**
- Client names are decisive for finance classification

**Evidence.** 9 rows (2 VERIFIED, 7 VERIFIED-SOFTENED); 8 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | supports | For the SOPA case study, only about 40% of the clients lobbying on SOPA in Q4 2011 could be mapped to NAICS sectors through Compustat, and the probabilistic analysis is restricted to them. Client disambiguation is search-assisted and the paper says it is weak for long-bankrupt firms and non-public-facing firms that resolve to third-party directories. The analysis window runs 1999-2023. | The 40% figure is for one bill-quarter sample (SOPA, Q4 2011), not all LobbyView clients; no all-client or finance-client coverage share is reported. It supports not relying on NAICS for credit-union versus bank classification only as an illustration. Whether the post-2021 issue-code collapse is a bulk-export vintage artifact remains UNVERIFIED and should be checked against the OSF release. | moderate | VERIFIED-SOFTENED: Client names decide classification and NAICS is only suggestive. High confidence requires a name rule. In the 2024-10-25 bulk-export vintage, issue-code coverage collapses after filing_year 2021. |
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | supports | Bombardini and Trebbi (2012) report an average share of lobbying by individual firms of 67% (median 96%); industries with lower concentration and lower product differentiation spend a higher share of lobbying through trade associations. Huneeus and Kim (2019) put business-association lobbying at less than a quarter of all lobbying expenses. | The prediction is cross-industry, from Bombardini and Trebbi's 2012 trade-issue sample. The review gives no CU-specific figure. | moderate | VERIFIED: Finance clients are classified bank, CU or other from client names. The brain records that directional positions sit with trade associations (CUNA 174 Support / 12 Oppose, NAFCU 168 Support / 23 Oppose; the 22 pinned fintech and crypto firms hold 1 directional position out of 56). ABA lobbying is $228M against CUNA's $93M. |
| [[harrison-2018-application-data-mining]] | 2018 | qualifies | The LDA XML amount fields do not separate lobbying expenditures from income, and no identifier lets a researcher split them. Expenses under $5,000 are a checkbox with no amount, and client income over $5,000 is rounded to the nearest $10,000. | The hazard carries over: the LD-2 amount semantics the paper describes still apply. The paper's claim that the two series cannot be separated does not carry over to LobbyView, which exposes is_self_filer; the collector could split them and does not in these aggregates. The LobbyView window and the 2024-10-25 vintage were not re-checked here. Whether the $228M ABA figure is the mixed total_amount, and how large the overlap is, stay UNVERIFIED until a second path reproduces them (DE-062). | moderate | VERIFIED-SOFTENED: The LobbyView collector parses is_self_filer per report (line 847) and parses a blank amount as 0.0 (line 844). Its per-side, per-year amount_sum (905) and finance_amount_sum (909), and its top-spender total_amount (932), add every report's amount whether self-filer expense or hired-firm income; self-filer dollars are kept only as a sub-sum (912). The auto-memory note lobbyview-integration-2026-07-31.md (not the brain) records 'ABA $228M vs CUNA $93M' and 'self-filer = expense, hired firm = gross income'. The claim that an in-house expense figure already includes payments to outside firms comes from LDA guidance as cited in commit 914623152, not from this paper. |
| [[harrison-2018-application-data-mining]] | 2018 | supports | Pediatric items sit in single filings alongside unrelated issues (Medicare Part B vaccine coverage, Medicaid pediatric vaccines, FDA biologics, pandemic preparedness), so dollars spent on pediatric lobbying cannot be isolated. The paper nonetheless reports dollar sums by umbrella code from a Filing x Issue merge: Taxation/Internal Revenue Code $527,147,333 and Health Issues $436,651,121 for Q4 2009, with no filing-count denominator. | Applies to any LDA source where one report carries several issue codes. That the paper's per-code totals count a multi-code filing under each code is an inference from the merge description, not stated by the authors; treat the totals as non-benchmarks either way. | moderate | VERIFIED: For finance-joined reports the collector increments a per-code report count (per_code[code] += 1) and does not spread or repeat dollars across codes. The auto-memory receipt '322,473 finance-coded reports' is a report count. |
| [[kim-2025-measuring-interest-group]] | 2025 | supports | Firm size is associated with position type. Comparing firms at the 90th and 10th employment percentiles, larger firms are 19.5 p.p. more likely to lobby and 4.5 p.p. more likely to adopt Monitor. Smaller firms are more likely to Oppose (1.8 p.p.) or Amend (1.3 p.p.). These are associations, not causal effects. Sample: Compustat firm-years 2009-2022, 5,421-7,014 firms per year. | Kim's sample is Compustat firms and says nothing about trade associations. The claim that the system's 22 pinned fintech/crypto firms hold 56 positions with one directional label was not found and is UNVERIFIED. Treat any reading that associations are the directional filers as a descriptive lead, not a replication. | weak | VERIFIED-SOFTENED: classify_client assigns side (credit_union, bank or other) from decisive name rules first. When no name rule fires, it falls back to a NAICS-based classification, and a NAICS conflict with a name match is tagged in the rule string. |
| [[huneeus-2018-effects-firms-lobbying]] | 2018 | method-source | LDA client names were matched to Compustat in four steps: FuzzyWuzzy against 61,478 unique client names; Bureau van Dijk batch search for ISIN and ticker; CRP standardized client names; and a random 5 percent sample of client names checked for publicly traded firms that were missed. The 5 percent sample is a miss (recall) check, not a precision audit. The paper names messy spellings and abbreviations as the main hazard. The matched universe is 67,842 unique lobbying client names (Introduction). | The transferable piece is the 5 percent sample as a recall audit for missed names. The fuzzy matcher is not transferable, and the project's strict-names rule rejects fuzzy acceptance. H&K's firm match leaves out most CUs (not in Compustat, per the analyst; not checked against the paper) and trade-association lobbying such as CUNA and NAFCU, so their match rates and Fact 5 do not carry to CU lobbying. | moderate | VERIFIED-SOFTENED: classify_client assigns Bank, CU or other. Credit-union names are checked first and are decisive, then trade-association names, then bank names with exclusions. Only when no name rule fires does the function fall back to a NAICS-based side. So NAICS does drive side assignment as a fallback, not only linkage metadata. No random-sample accuracy audit of the classification is recorded. |
| [[kim-2018-lobbyview-firm-level]] | 2018 | qualifies | Firms (clients) are grouped into industries by NAICS2 (or SIC) to compute per-industry Herfindahl indices. The example run is NAICS for 1996-2011. Output is then merged with Compustat firm data and a trade-issue indicator (Sections 5.5, 5.5.1, 5.5.2). | The procedure claim holds. The paper does not say NAICS is usable for research aggregates; it only uses NAICS2 as the grouping key. The reason given for the system's rule (codebook gives NAICS no provenance) is not in the code checked, so it is UNVERIFIED. The system ruling that NAICS is linkage metadata was not checked here. Herfindahl outputs are industry descriptors and must not be edge or weight sources (D-021, not re-checked). | moderate | VERIFIED-SOFTENED: collect_lobbyview.py classifies clients by name rules first (credit-union and bank rules). NAICS is used only as a fallback and yields a low-confidence side. A '+naics-conflict' suffix is recorded when a validated NAICS code disagrees with a name rule (classify_client, lines 213-263). The paper uses NAICS2 as its industry key and does not validate NAICS codes. |
| [[delano-2025-bicam-congressional-lobbying]] | 2025 | context | BICAM supplies Bill-LDA linkages, not the LDA filings, and points to LobbyView and OpenSecrets as the compilations of LDA data. Members are keyed by bioguide_id and icpsr. Discrepancies between the two APIs, such as GovInfo missing bioguide_id for nickname members, are fixed by a rules-based merge. | Context only. BICAM does not replace the client or filer source. Any post-2021 bill linkage from it would serve identity resolution, not classification input. | weak | VERIFIED-SOFTENED: collect_lobbyview.py is a standalone ingest of the LobbyView export dated 2024-10-25 (VINTAGE, line 73). The n_bills caveat at line 446 describes sponsored or cosponsored attention to bills, not disclosed contact. |
| [[note-sector-growth-and-political-spending-source]] | 2026 | context | Melange holds bank and CU lobbying aggregates for 1999-2023 and cached annual series for 2021-2025 plus partial 2026 (bank, CU, crypto, fintech). Life insurance and auto are not cached. Organizations still need alignment to the research sectors under one expenditure definition. | DEFAULT_YEARS covers the LDA sector-spend build, not the 1999-2023 bank and CU aggregates the note cites. Using LDA spend as an outcome per organization is an identity-source use consistent with D-021; no edge or weight use is implied. | internal (design doc) | VERIFIED-SOFTENED: fec/lobbying/sector_spend.py line 49 sets DEFAULT_YEARS to 2021-2026, consistent with the cached 2021-2025 plus partial 2026 window. The analyst's claim that LobbyView finance classification is name-regex based with coverage reliable through 2021 (vintage 2024-10-25) was not checked. |

**Assessment.**

**Evidence weight: moderate on the hazards, absent on accuracy.** 9 rows (2 VERIFIED, 7 VERIFIED-SOFTENED).

**What holds:**
- **Harrison 2018, VERIFIED.** A multi-code LD-2 filing cannot have its dollars split across issues. The system does the safe thing: `per_code[code] += 1` counts reports and does not spread dollars. So '322,473 finance-coded reports' is a report count, not a dollar figure. Window: LobbyView 2024-10-25 vintage. Denominator: finance-joined reports.
- **Bombardini & Trebbi 2020 review, VERIFIED.** Across industries, individual firms average 67% of lobbying (median 96%), and associations account for less than a quarter of lobbying expense (Huneeus & Kim). That fits the brain's observation that directional positions sit with CUNA and NAFCU. But it is cross-industry, with no figure specific to credit unions.
- **Huneeus & Kim 2018.** Its 5% random-sample check for missed names (a recall audit) is the one transferable audit design. Its fuzzy matcher is not transferable, because the strict-names rule bars it.

**Hazard (Harrison, VERIFIED-SOFTENED).** LD-2 amount fields mix two things: a self-filer's own expense and a hired firm's income from a client. The collector parses `is_self_filer` but adds both kinds into `amount_sum`, `finance_amount_sum` and `total_amount`. Whether the ABA $228M / CUNA $93M comparison uses that mixed total, and how large the overlap is, stays UNVERIFIED. Commit 914623152 ('side by side, never added') may already cover the display path; the aggregates were not re-checked. Money collection is frozen, so any change here needs Paul's OK for that specific change.

**Coverage.** Bacik et al. map only about 40% of clients to NAICS. That figure covers one bill-quarter (SOPA, Q4 2011), so it does not estimate all-client coverage. Whether the collapse in issue-code coverage after 2021 is an artifact of the bulk-export vintage is UNVERIFIED and should be checked against the OSF release.

**Empirical question unanswered.** Nothing compares name-based classification with Kim et al.'s positions-based industry assignment. Kim 2025's sample is Compustat firms 2009-2022 with no trade associations, so it cannot supply that comparison.

**Contradictions.**
- The component says 'NAICS is linkage metadata only'. Three rows (Huneeus & Kim, Kim 2018, Kim 2025) read classify_client and find a NAICS fallback that assigns side (with LOW confidence) when no name rule fires. NAICS therefore drives classification for unmatched names.
- The component reports ABA $228M vs CUNA $93M as a joined total. Per Harrison and LDA guidance, an in-house expense figure already includes payments to outside firms, while the collector adds self-filer expense to hired-firm income. The comparison may double count or mix the two series (UNVERIFIED).
- Paul treats Harrison as an introductory pipeline guide. The verified content of Harrison is a warning that amount semantics and multi-issue dollars cannot be separated.

**Gaps.**
- No precision/recall audit of the name-regex classifier. Needed: a blind hand-labelled random sample of LobbyView clients (stratified by HIGH vs LOW confidence and by bank / CU / other / trade association), reporting precision and recall per class plus a recall check for missed names in the style of Huneeus & Kim.
- No comparison with Kim et al. positions-based industry assignment. Needed: a crosswalk on the overlapping clients for 2009-2022, with an agreement rate and its denominator.
- No measured split between self-filer expense and hired-firm income for ABA, CUNA or NAFCU. Needed: a recomputation keyed on is_self_filer for one stated window, with Paul's per-change OK because money collection is frozen.
- Coverage after 2021: no check of the 2024-10-25 bulk export against the OSF release.

**Paul's position vs the evidence.**

The evidence supports LobbyView as the identity backbone, consistent with D-021: an identity source, not an edge or weight source. It does not support Harrison as a pipeline guide. Harrison's verified contribution is two hazards, the mixed amount semantics and multi-issue dollars that cannot be split, and the second of these is handled correctly here while the first may not be. Paul did not foresee the reliability cutoff at 2021. The evidence neither confirms nor explains that cutoff, which is still a vintage question.

**Open questions.**
- Is the ABA $228M / CUNA $93M figure the mixed total_amount, and does commit 914623152 fix the aggregates or only the display?
- How many finance clients are classified only by the NAICS fallback (LOW confidence), and should the component text stop calling NAICS 'linkage metadata only'?
- Is the issue-code collapse after 2021 an artifact of the bulk export or real missing coverage in LobbyView?

**Read first.** [[harrison-2018-application-data-mining]], [[huneeus-2018-effects-firms-lobbying]], [[bombardini-2020-empirical-models-lobbying]], [[kim-2018-lobbyview-firm-level]], [[bacik-2025-measuring-dynamical-evolution]]

## Lobbying member-year network and circular lobbied-member link

**What it computes.** client x bioguide x year dyads (63,991 member-years). Edges are the sponsor and cosponsors of lobbied bills.

**Method.** P(joined | lobbied) = 1 by construction, so it is REFUTED as evidence of influence. LD-2 filings never name members.

**Code refs.** `fec/build_network_2021_rebuild.py`, `lobby_interactions.py:4`

**Assumptions.**
- The bill-roster join is not a lobbying-contact measure

**Evidence.** 11 rows (2 VERIFIED, 9 VERIFIED-SOFTENED); 10 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[azaiez-2025-hypergraph-analysis-european]] | 2025 | context | Each edge comes from an official register entry that names both the EC official and the interest representative in a dated meeting. Duplicate meetings were fuzzy-merged at partial ratio >= 70 (460 pairs). | The paper shows what a valid lobbying-contact layer requires: a register that names both parties to the contact. No US congressional equivalent exists, so the design does not carry over to LDA. It is a benchmark for any future contact source, not a method to apply. | weak | VERIFIED-SOFTENED: LD-2 filings never name a member, so the bill-roster join gives P(joined / lobbied) = 1 by construction. It is REFUTED as evidence of influence, and LDA stays an identity source only (D-021). |
| [[kim-2020-strategic-sequential-links]] | 2020 | qualifies | Outcome Y=1 if firm i lobbies any bill in year t related to politician j as sponsor, co-sponsor, or member of the committee the bill is assigned to. For the descriptive Lobby $ column, a firm's annual lobbying spend is divided by the number of unique bills lobbied and the per-bill amount is allocated across all connected politicians (sponsor, co-sponsors, committee members); the authors call this a highly noisy measure and use the binary link in the analysis. Donation onset is the separately measured treatment. | Kim uses the same roster join only as an outcome against an independently timed treatment (donation onset), the non-circular use. That supports keeping the join as 'whose bills did this org lobby on', never as contact. Kim adds a committee-referral channel the system's join lacks. The paper gives no warrant to read the join as contact. Adopting Kim's dollar allocation would make LDA a weight source (barred by D-021). | moderate | VERIFIED-SOFTENED: The shipped network credits every sponsor and cosponsor of each lobbied bill (build_network_2021_rebuild.py docstring line 13; lobby_interactions.py:4-5 'did this organization lobby a bill this member sponsored or cosponsored'). The brain records the lobbied-member link as circular (P(joined / lobbied)=1 by construction); LD-2 never names members. committee_referrals is parsed (build_network_2021_rebuild.py:5466-5697) but not used to credit members. |
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | supports | The LDA does not require registrants to disclose direct contacts with legislators, so the paper infers legislator connections from lobbyists' disclosed past employment; a legislator is included if at least one active lobbyist worked for them. About 90% of lobbyists have no documented government or legislator association, and 20% had a known government connection in 2023. | The evidence carries over fully because it describes the same federal LD-2 form. The paper uses the same data and states plainly that legislator contact is unobserved, so nothing in LD-2 can rescue the bill-roster join. An independent contact measure is still required. The paper's own legislator edges come from revolving-door disclosure, so they are not contact evidence either, and influence language is barred on them. | strong | VERIFIED-SOFTENED: The member-year network credits every sponsor and cosponsor of a lobbied bill (build_network_2021_rebuild.py:13), so the system treats the link as REFUTED as evidence of influence. The interaction ledger states that the LD-2 form has no position field and ships position_basis='not_disclosed'. |
| [[kim-2025-measuring-interest-group]] | 2025 | qualifies | Positions are measured as interest group x bill. Legislators enter only as context nodes in the GNN graph, through sponsor/cosponsor relations and legislator-lobbyist prior-work relations. The selected configuration is 'Base Graph + legislator-lobbyist'. The paper does not measure a position or contact between a group and a member. | Kim's measure stops at the bill. Pushing it onto members through the bill roster brings back the circular sponsor/cosponsor mapping the system already refuted as influence evidence. The dyad counts describe which bills members sponsored, not whom an org targeted. Kim's legislator-lobbyist relation is a revolving-door edge, so influence language is barred on it. | strong | VERIFIED: build_dyad_signs joins signed org-bill rows to sponsor/cosponsor rosters by canonical bill, counting probability >= 0.7 as support and <= 0.3 as oppose. efficiency_layers z-scores signed_non_cu_opposition_count into signed_non_cu_opposition_z, which is one component of contest_index. |
| [[kim-2020-mapping-political-communities]] |  | supports | Lobbying reports need not identify the politicians contacted, so the network is built indirectly: A_ij = number of bills group i lobbied that politician j sponsored (113th Congress, k = 8). The authors call this 'a type of political connection' and read recurring co-occurrence as 'shared involvement on specific political issues', while stating that lobbying a bill 'does not necessarily imply political ties to its sponsor'. They make no claim that the link is contact or that lobbying causes sponsorship. Window: 113th Congress; denominator: lobbied bills with an identified sponsor. | Same bill-roster construction, but the system's is broader (adds cosponsors) while the paper uses sponsors only, so the dyads are not identical. The paper avoids circularity only because it makes no lobbying-predicts-sponsorship claim; any Melange reuse must stay descriptive. Under D-021 the matrix is an identity source, not an edge or weight source. | strong | VERIFIED-SOFTENED: build_network_2021_rebuild.py credits every legislator who sponsored OR cosponsored each lobbied bill and emits lob_id x bioguide_id x year x n_bills rows. Project memory records this link as circular: P(joined / lobbied) = 1 by construction, so it cannot serve as influence evidence. |
| [[kim-2023-systemic-sequential-links]] | 2023 | qualifies | Outcome is a firm lobbying a bill tied to politician j through sponsorship, co-sponsorship or committee referral, with bills matched by number and title because LDA reports have no member names or unique bill IDs. Bill-linked reports are 0.25M of 1.27M (about 20%) and cover about 97% of lobbying spending. The authors call the estimates a conservative lower bound. | The paper uses the same roster join, but its treatment is donation timing, not the join itself, and it treats the outcome as bill involvement, not contact. Neither the paper nor the code supports reading the join as influence or contact (consistent with D-021). The paper's own committee-referral arm is absent from the system's join. | moderate | VERIFIED-SOFTENED: fec/lobbying/lobby_interactions.py lines 4-5 describe the shipped network as (organization, member, year) counts of whether an organization lobbied a bill the member sponsored or cosponsored. The claim P(joined given lobbied) equals 1 by construction is the analyst gloss and is not stated in those lines. |
| [[boxsteffensmeier-2018-taking-congress-interest]] | 2018 | supports | Footnote 11 (p. 170): a member reviewing a Dear Colleague letter cannot distinguish an endorsement the member solicited from one the interest group solicited from a sponsor. P. 171: endorsers listed together on a letter may not have any interaction, and endorsements were likely pulled together by a member's office. | The paper's endorsement channel is observed by members, which is a stronger channel than an LDA bill mention. Its authors leave solicitation direction unresolved, which supports caution about reading bill co-occurrence as influence. The paper does not discuss LDA filings. | moderate | VERIFIED-SOFTENED: bill_lobbying_sidecar.py docstring (lines 10-20) confirmed: LDA filings never name the members lobbied, and the sidecar ships only a bill-level interaction total. The stronger claim that P(joined given lobbied) = 1 by construction, so the link was refuted, was NOT found in the cited code or sidecar. |
| [[kim-2018-lobbyview-firm-level]] | 2018 | supports | The abstract says LobbyView bridges politicians' sponsorship of bills with reported lobbying by interest groups. Section 2 steps 1-5 describe only matching report text to bill identities (number, Congress, title, range). No step describes a legislator link or any contact. The word sponsor appears once in the paper, in the Abstract. | Holds for the system join. The paper's sponsorship wording is only the abstract phrase, and the system's join is wider: sponsor plus all cosponsors. collect_lobbyview.py line 446 records a 2026-08-01 check that BECU 2021 has 181 edges equal to 1 sponsor plus 180 cosponsors of H.R. 1996. That is a code comment; it was not re-run here. | strong | VERIFIED-SOFTENED: The system's join is stated in code. lobby_interactions.py docstring says the network answers whether an organization lobbied a bill the member sponsored or cosponsored. build_network_2021_rebuild.py line 13 says each lobbied bill credits every sponsor and cosponsor. So the member link is a bill-roster join, not a contact record. The paper does not describe this join itself. |
| [[kim-2018-lobbyview-firm-level]] | 2018 | suggests-extension | Filing-year Congress assignment fails. Google's 2013 report cites H.R. 2577 (Safe Data Act), a 112th Congress bill, not the 113th. LobbyView considers the three preceding Congresses by default, picks the candidate with highest cosine similarity to surrounding text (Eq. 1), falls back to the filing-year Congress if no number matches, and propagates a Congress across the report by majority vote. | Paper side reproduced. Under the rebuild rule, the paper's own example would bind to the 113th Congress. Risk concentrates in filings made early in a new Congress (odd-year first sessions). The paper gives no rate, so the size of the error in the rebuild is UNVERIFIED. | moderate | VERIFIED: The 2021+ rebuild binds each formal citation to the filing Congress unless an 'of 20XX' suffix in the same segment resolves to an earlier Congress (_resolve_mention_congress; current mentions set the resolved Congress to filing_congress). The rebuild has no cosine-similarity lookback and no report-level propagation. A search for cosine, propagate, majority and argmax returned no hits. |
| [[goff-nd-cross-industry-finance]] |  | qualifies | 2026 LDA reports show members Zach Nunn, Andy Kim, Young Kim, Bill Huizenga and others with an average increase of 20 interactions within the year, read as the community-bank industry pushing harder. | Applies only if the report's interaction counts come from the bill-roster join, which the report does not say. If they come from described contacts, only the window and denominator flags remain. | moderate | VERIFIED-SOFTENED: lobby_interactions.py reuses the build_network_2021_rebuild sponsor and cosponsor join for organization, bill and member attribution. The rebuild docstring (line 13) says each lobbied bill credits every legislator who sponsored or cosponsored it, so per-member counts from this join measure bill-roster overlap, not observed contact. The report never defines interactions or names its join. |
| [[delano-2025-bicam-congressional-lobbying]] | 2025 | qualifies | Bill numbers and titles extracted from LD-2 Section 16 are matched by distance measures to the Bills component (421,764 bills, 6th-119th Congress); low-confidence matches get extra scrutiny. The linkage is said to detect significantly more bill lobbying than Kim and Kunisky 2021 and to capture ex-post lobbying. The main text gives no match count, precision, recall or threshold. | BICAM fits only as a candidate second path for bill-mention resolution. A bill-to-sponsor join from it is a roster edge with P(credited given lobbied) = 1 by construction and adds no contact signal. Precision and recall sit in the Supplementary Information, which this check did not have. | moderate | VERIFIED-SOFTENED: build_network_2021_rebuild.py credits every sponsor and cosponsor of each lobbied bill (line 13). Generous outputs keep ambiguous-title fan-out; strict outputs need a same-segment formal citation. Slash-joined citations parse only the left side (line 19; 2 of 376 distinct bills, 0.5 percent, 2023-24 association corpus). LD-2 has no member or position field (lobby_interactions.py line 18). |

**Assessment.**

**Evidence weight: strong and in agreement.** 11 rows (2 VERIFIED, 9 VERIFIED-SOFTENED).

**The refutation holds.** The system has refuted the bill-roster join as evidence of influence, and the literature agrees:
- **Bacik et al. 2025 (strong).** It uses the same federal LD-2 form and states that registrants need not disclose contacts with legislators.
- **Kim & Kunisky (strong).** It builds A_ij from sponsors only (113th Congress, k = 8; denominator: lobbied bills with an identified sponsor). It says this is 'a type of political connection', not contact.
- **Kim 2018 LobbyView (strong).** Its only sponsorship wording is one phrase in the abstract.
- **Kim et al. 2025 positions, VERIFIED.** Its measure stops at interest group x bill.

The system's join is wider than any of these: sponsor plus all cosponsors. The code comment records BECU 2021 = 181 edges = 1 sponsor + 180 cosponsors of H.R. 1996; that comment was not re-run.

**The non-circular use (Kim, Stuckatz & Wolters 2020/2023).** The roster join is legitimate as an outcome against a separately timed treatment (donation onset). The papers also add a committee-referral channel that the system parses (`committee_referrals`) but does not use to credit members. Kim 2023 reports that bill-linked reports are about 20% of all reports (0.25M of 1.27M) but about 97% of spending.

**Data-quality lead (Kim 2018, VERIFIED).** The 2021+ rebuild binds each citation to the filing Congress. It has no cosine-similarity lookback and no Congress propagation across a report, so LobbyView's own example (Google 2013 citing H.R. 2577) would bind to the wrong Congress. The error rate is unmeasured; the risk is concentrated in odd-year first-session filings.

**Benchmark (Azaiez 2025).** A valid contact layer needs a register that names both parties to a dated meeting, as the EC register does. No US congressional equivalent exists. Revolving-door legislator edges (Bacik's g/p functions, Kim 2025's legislator-lobbyist relation) are not contact evidence, and influence language is barred on them.

**Contradictions.**
- Paul reads Kim & Kunisky as a tool for identifying champions. The verified text says lobbying a bill 'does not necessarily imply political ties to its sponsor', and it makes no predictive or contact claim.
- build_dyad_signs joins signed org-bill positions to sponsor/cosponsor rosters, and the result enters contest_index through signed_non_cu_opposition_z. That reintroduces the circular roster mapping the system has refuted (VERIFIED row, Kim et al. 2025).
- Paul's own 'Cross Industry Finance and LDA' report reads 2026 increases of about 20 interactions per member (Nunn, Andy Kim, Young Kim, Huizenga and others) as the community-bank industry 'pushing harder'. If those counts come from lobby_interactions.py, they measure overlap with bill rosters, not contact. The report never defines 'interactions'.
- The brain notes do not record the refutation, according to the hub position text, while the code docstrings do.

**Gaps.**
- Independent contact measure (the component's empirical question). Needed: a source that names the member and is dated independently of LD-2, such as member calendars or meeting logs, FARA contact reports, or Dear Colleague endorsements with solicitation direction coded. Test whether contact precedes cosponsorship more than a matched placebo would.
- Congress-binding error rate of the rebuild against LobbyView's lookback rule, measured on an overlap year (for example 2019-2021) with a stated denominator of citations.
- Committee-referral crediting is parsed but unused. Adding it would not cure circularity; it matters only for a Kim-style design with a separately timed treatment.

**Paul's position vs the evidence.**

The system's REFUTED status is what every row supports, and that agreement is as strong as anything in this hub. Paul's stated reading of Kim & Kunisky as a champion finder is not supported by the paper. It is descriptive community structure on a sponsor-only roster join. Any 'champion' output built on this join is a heuristic model output, never closeness, access or contact. Paul's own 2026 report appears to use the refuted reading.

**Open questions.**
- Does contest_index (via build_dyad_signs) count as a use of the refuted join, and should its label carry a bill-roster caveat?
- Where do the interaction counts in the Cross Industry report come from?
- Should the brain record the REFUTED status explicitly, given that the notes reportedly do not?

**Read first.** [[bacik-2025-measuring-dynamical-evolution]], [[kim-2020-mapping-political-communities]], [[kim-2025-measuring-interest-group]], [[kim-2023-systemic-sequential-links]], [[kim-2018-lobbyview-firm-level]], [[azaiez-2025-hypergraph-analysis-european]]

## FEC to LDA bridge test

**What it computes.** Relation between FEC giving and LDA lobbying network change.

**Method.** Round 1 VOID. Round 2 p = .098 (Spearman +0.259, N = 52). Round 3: raw rho +0.153, but the four-control partial is -0.0002. Closed pending a new vintage. FEC as a 42-month leading indicator is a candidate heuristic.

**Code refs.** `fec/build_network_2021_rebuild.py`

**Assumptions.**
- FEC giving leads lobbying (unvalidated)

**Evidence.** 7 rows (7 VERIFIED-SOFTENED); 6 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2020-strategic-sequential-links]] | 2020 | qualifies | Firm-politician-year LPMs (Table 3, N=16,428,834, public manufacturing firms NAICS 31-33 x members of Congress, 2008-2017): a first-time donation in year t raises P(firm lobbies a bill the politician sponsored, cosponsored or handled in committee, same year) by 0.102-0.118 (cols 1-4), against a base of about 0.05. The table text sets L = t-1; the matched DiD uses a 3-year no-donation history (L=3). Matched DiD (Fig 5): about +8.5 pp at t for the Senate, falling to about 3 pp (caption: 3.2) at t+3; the House is described as stronger but no number is given. Fig 6: effects larger for corporate PACs and particularly stronger for committee membership; the authors say the effects disappear within two to three years. | Estimands differ: Kim identifies within-pair onset timing (pair FE or matched DiD on first-time donations) among public manufacturing firms; the bridge is a between-member cross-section over concurrent windows dominated by financial trade associations. The bridge Y credits only sponsor and cosponsor rosters, so the committee-referral outcome channel that carries Kim's strongest effect is absent; the bridge's committee control is a covariate (Kim also controls for 'powerful committee' membership), so 'controls away Kim's channel' is an interpretation, not a demonstrated fact. Appendix E's trade-association slope (beta 0.8 vs corporations beta 2) is a cube-root, 1999-2000, organization-level replication of Ansolabehere et al., not Kim's dyad design. So the bridge null does not refute Kim and Kim does not license predictor language. Kim does NOT bear on the '42-month lead' as written, because that number is a data-recency gap, not a behavioral lead; at most Kim argues against reading it as one. A successor onset/pair/committee-referral test is a new prereg and Paul's gate. | strong | VERIFIED-SOFTENED: Bridge round 3 is a LOCKED corroboration look: X = FEC relational exposure, Y = rebuilt LDA attention, per member x industry (credit_union, fintech, crypto) x cycle, 2023-2026; pooled fractional-rank Spearman with member-blocked permutation (5,000) and four controls (party, chamber, volume-within-party, House Financial Services / Senate Banking membership). Recorded result: raw rho +0.1525 (member-blocked p=0.0146), four-control partial -0.0002; UNCORROBORATED, window CLOSED, predictor language barred. The brain's '42-month lead' (lobbyview-collector.md:124; memory lda_bridge.lead_months=42) is a data-freshness gap (FEC data runs about 42 months past LobbyView coverage), not a measured behavioral donation-to-lobbying lag. |
| [[furnas-nd-conflating-lobbying-pacs]] |  | supports | Organizations that lobby and organizations that give through PACs barely overlap. Per year, as a share of all politically active organizations: 1998 (n=9,076) 64% lobby only, 24% PAC only, 12% both; 2008 (n=15,729) 70/20/10. Pooled 1998-2018 (Figure 4): 3,427 organizations (6.3%) did both; the 6.3% is printed, and its denominator 54,517 is the sum of the four Figure 4 groups (34,038 only lobby, 9,528 only contribute, 3,427 both, 7,524 not active), so it includes inactive organizations. The authors put the yearly overlap at 10-15% and tell researchers not to conflate lobbying and electioneering. | The paper is about organizations; the bridge is about members (where money exposure and lobbying attention land). Different estimands, and the paper does not explain the zero partial. What carries over is the population point: organizations that lobby without a PAC (64-70% of all politically active organizations per year, about 84-88% of lobbying organizations) leave no organizational FEC trace, so FEC cannot be a general lobbying proxy. Federal organizations, 1998-2018, CRP definitions. UNVERIFIED (DE-062). | moderate | VERIFIED-SOFTENED: Bridge round 3 (look dated 2026-08-01) is CLOSED and UNCORROBORATED: pooled member-level Spearman rho +0.153 (p=0.015) but four-control partial -0.0002, window 2023-2026, pooled N=213 member x industry x cycle pairs from fintech and crypto, two cycles each (credit_union strata coverage-limited and not entering). Fixed consequence wording: FEC exposure is a target-side money prior only and must not be presented as a lobbying-relationship predictor. The LDA side uses a locked 22-lob_id crosswalk. |
| [[furnas-nd-conflating-lobbying-pacs]] |  | qualifies | Among all politically active organizations, 64-70% per year lobby without making PAC contributions; among lobbying organizations alone that is roughly 84% (1998) to 88% (2008). The share of PAC contribution dollars coming from organizations that also lobby fell from more than 65% (1998) to about 30% (2018). Contributions by employees or contract lobbyists are not counted as organizational giving. | Low overlap undercuts any reading in which one organization's giving foreshadows its own later lobbying, because most lobbying organizations do not give through a PAC. The paper measures organizational/affiliated-PAC money, not the employee giving this prior uses, so it bounds the lead wording rather than refuting it. The freshness reading of lead_months is unaffected. UNVERIFIED (DE-062). | moderate | VERIFIED-SOFTENED: The 'FEC leads LDA' heuristic number comes from _lead_months, computed as months from the end of the LobbyView finance-joined filing year to the latest FEC transaction date: a data-freshness gap, not an estimated lag. The opposition prior's FEC basis is itemized individual giving by employees and executives of 10 sample companies per industry plus limited corporate rows, not industry-complete. |
| [[kim-2020-mapping-political-communities]] |  | suggests-extension | Table 1 lists the 10 dyads whose observed interactions most exceed biLCM predictions (e.g. Shaheen-NOVO NORDISK 12 observed vs 1.61 estimated; Marino-CVS 10 vs 0.55). The authors suggest explanations from geography, campaign contributions (IHEARTMEDIA with Conaway and Barrasso) and bill-specific stakes, without testing them. | Suggests testing whether FEC money concentrates on dyads with positive community-model residuals rather than correlating levels. Hypothesis only; anecdotes from one Congress. Residuals on a bill-roster join inherit its circularity for any join outcome; a different control set on the same window is not a successor test. | weak | VERIFIED-SOFTENED: The preregistered round-3 FEC x LDA bridge test found money contributes -0.0002 beyond structural controls, so FEC data is excluded as a feature of the lobbying champions residual rank. Project memory records the raw rho as +0.153 and the result as UNCORROBORATED. |
| [[kim-2023-systemic-sequential-links]] | 2023 | qualifies | Within-politician first-time-donation designs find donations followed by lobbying on the member's bills. Table 1 pooled coefficients 0.041, 0.048, 0.047, 0.023 (SE 0.006-0.007; N 16,434,912 in table, 16,428,834 in text). Matched DiD Senate 0.075 to 0.110 and House 0.086 to 0.127 over t+0 to t+3 (3,778 Senate and 5,497 House instances). Committee-linked PAC lobbying rises 0.103 to 0.147, versus sponsorship 0.029 to 0.037. Lobbying does not predict later donations, except a small sponsor-lobbying term in App K. | Paper design: firm-politician pair fixed effects, year FE, first-time donation timing, 2008-2018 manufacturing firms. Bridge: member-level cross-section, 2023-26, 22 financial-sector clients, committee control. Different estimand, so the paper does not test the bridge. The paper does not say its committee channel is absorbed by a committee control in a cross-section; that overcontrol reading is the analyst inference (and the code's OVERCONTROL_SENTENCE). The paper is observational and says so (pp.3, 29). | moderate | VERIFIED-SOFTENED: Per the analyst: a member-level cross-sectional Spearman-type rank correlation (the code output label says pooled fractional-rank Spearman rho) between member FEC exposure and LobbyView n_bills for 22 lob_ids, with four controls (party, chamber, volume, committee). Raw rho +0.153 and four-control partial -0.0002 were not checked against a results file in this pass. Constants and wording confirmed in fec/lobbying/fec_lda_bridge3_look.py lines 57-80 (CONTROL_NAMES, OVERCONTROL_SENTENCE, UNCORROBORATED wording). |
| [[bickmore-2025-corporate-lobbying-political]] | 2025 | context | Lobbying (Tables 3-5, annual N = 116,242) and PAC (Tables 6-7, election-year N = 62,198 and 62,558) are estimated in separate regressions on different samples. The paper does not estimate them jointly, and 'never in sequence' is an inference from table structure, not an explicit statement. | The paper gives no evidence on whether PAC giving leads lobbying or how much the two overlap, so it neither revives nor refutes the bridge. The claim that Furnas et al. show the sharper overlap relation comes from the vault, not from this paper. | weak | VERIFIED-SOFTENED: fec_lda_bridge3_look.py builds a Pearson rho and a partial correlation controlling for party, chamber, volume and committee (CONTROL_NAMES line 57; partial() and result dict around lines 1066-1085). The four-control partial of -0.0002 appears in lobby_forecast.py and lobby_champions.py comments. The raw rho of +0.153 was not located in the cited folder and is not asserted here. |
| [[kim-2018-lobbyview-firm-level]] | 2018 | qualifies | Close bill numbers with the same prefix and numbers differing by at most 10 are expanded into ranges. The Mattel 2002 citation H.R.4182-4186 becomes five bills (H.R. 4182 to 4186). Bills are also matched by title and the Congress is propagated across the report. No error rates are reported. | Paper side reproduced. The claim that the bridge used the LobbyView network.csv vintage for 2021-2022 was dropped, since no source for it was checked. Whether the rule difference is measured on an overlap year is UNVERIFIED. | moderate | VERIFIED-SOFTENED: The rebuild's continuation parser (CONTINUATION_RE) accepts only comma, semicolon, ampersand and 'and' separators, so a hyphenated range is not expanded. No hyphen-range expansion was found in the rebuild. Congress binding differs (see the Congress bearing). |

**Assessment.**

**Evidence weight: moderate. It qualifies the bridge but does not test it.** 7 rows (7 VERIFIED-SOFTENED). The Furnas rows carry the DE-062 caveat that transfer to this system is UNVERIFIED.

**Kim, Stuckatz & Wolters (2020/2023) are the strongest papers, and their estimand differs.** Their setup:
- public manufacturing firms (NAICS 31-33) x members of Congress, 2008-2017/18
- firm-politician pair fixed effects or matched DiD on first-time donations

Their results:
- A first-time donation raises P(firm lobbies a bill tied to the member, same year) by 0.102-0.118 (Table 3, N about 16.4M; base about 0.05).
- Matched DiD: Senate about +8.5 pp at t, falling to about 3 pp at t+3. Kim 2023 reports Senate 0.075-0.110 and House 0.086-0.127.
- Effects are strongest through committee referral (0.103-0.147, vs sponsorship 0.029-0.037).
- Lobbying does not predict later donations, apart from a small sponsor term in App K.

The bridge is a different design: a member-level cross-section with concurrent windows, 2023-2026, 22 financial-sector lob_ids. Pooled N = 213 member x industry x cycle pairs from fintech and crypto, with credit_union strata coverage-limited and not entering. Its Y credits sponsor and cosponsor rosters only, with no committee-referral channel. So the bridge null does not refute Kim, and Kim does not license predictor language. The claim that the committee control 'absorbs Kim's channel' is the code's OVERCONTROL_SENTENCE, an inference.

**Furnas, LaPira & Brock.** Organizations that lobby and organizations that give through PACs overlap little, among federal organizations under CRP definitions:
- yearly overlap 10-15%
- 1998 (n = 9,076 active organizations): 64% lobby only, 24% PAC only, 12% both
- 2008 (n = 15,729): 70 / 20 / 10
- pooled 1998-2018: 6.3% of 54,517, a denominator that includes inactive organizations
- among lobbying organizations alone, about 84-88% give no PAC money

FEC therefore cannot be a general proxy for lobbying. The bridge's FEC side is mostly employee giving (10 sample companies per industry), which Furnas does not measure, so Furnas bounds the 'lead' wording rather than refuting it.

**The bridge result must be quoted as UNSTRATIFIED.** Recorded: raw rho +0.153 (member-blocked p = 0.015), four-control partial -0.0002, CLOSED, UNCORROBORATED. It is pooled across fintech and crypto, and industries are a known fault line.

**The other rows are weak:**
- Bickmore 2025: context only.
- Kim & Kunisky: residual-dyad hypothesis only.
- Kim 2018: the rebuild does not expand hyphenated bill ranges such as H.R. 4182-4186, which affects Y; the size of the effect is unmeasured.

**Contradictions.**
- The '42-month lead' in the brain and memory ('FEC leads LDA 42mo'; lda_bridge.lead_months = 42) is computed as months from the end of LobbyView finance coverage to the latest FEC transaction. That is a data-freshness gap, not a measured behavioral lag. Kim's within-pair timing (an effect at t that fades by t+2 to t+3) is nowhere near 42 months.
- Paul's notes expect donations to lead lobbying. Kim finds that direction within dyads, among manufacturing firms. Furnas finds that 84-88% of lobbying organizations give no PAC money. The bridge partial is -0.0002. The expectation can hold within pairs while failing as a population-level predictor.
- Statistic label inconsistency: rows describe the bridge as a pooled fractional-rank Spearman, but one row (Bickmore SYS) reads fec_lda_bridge3_look.py as building a Pearson rho plus a partial. The raw +0.153 was not located in a results file in two passes.
- The component text gives Round 2 as N = 52 and rho +0.259; the round 3 rows give pooled N = 213. These are different looks. Do not quote them together without labelling which round each comes from.

**Gaps.**
- A successor test matching Kim's estimand: financial-sector organizations (CUNA, NAFCU, ABA, fintech, crypto), first-time donation onset, pair fixed effects or matched DiD, a committee-referral outcome channel, on a new LDA vintage. It needs a new prereg and is Paul's gate; a different control set on the same window is not a successor.
- A behavioral lag estimate (any lag, not 42 months), with the window and denominator named.
- Measurement of the Y-side parser loss: missing hyphen-range expansion and Congress binding, against LobbyView on an overlap year.

**Paul's position vs the evidence.**

The system's own closure (UNCORROBORATED, predictor language barred) is well supported. Paul's expectation that donations lead lobbying has partial support only within dyads and in another sector. It fails as a population proxy (Furnas), and the bridge does not test it. The '42-month lead' should be relabelled a data-freshness gap wherever it appears. As written, it misstates what was measured.

**Open questions.**
- Will Paul relabel the '42-month lead' (memory index line 'FEC leads LDA 42mo', the lobbyview-collector.md:124 note, and the lda_bridge.lead_months config) as a freshness gap?
- Pearson or Spearman: which statistic did fec_lda_bridge3_look.py actually compute, and where is the results file holding +0.153?
- Does Paul want an onset/pair/committee-referral successor prereg once a new vintage lands?

**Read first.** [[kim-2023-systemic-sequential-links]], [[kim-2020-strategic-sequential-links]], [[furnas-nd-conflating-lobbying-pacs]], [[kim-2020-mapping-political-communities]], [[kim-2018-lobbyview-firm-level]]

## Contact inference (activity to member)

**What it computes.** Matches lobbying activity descriptions to members.

**Method.** Gold positives name the member. Negatives come from the same organisation's roster join, split 50/50 by organisation. Contamination is estimated by a recall sample.

**Code refs.** `contact inference lane (path not recorded)`

**Assumptions.**
- Roster-join negatives are not contacts

**Evidence.** 3 rows (1 VERIFIED, 2 VERIFIED-SOFTENED); 3 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2020-strategic-sequential-links]] | 2020 | suggests-extension | The DiD design keys on first-time donations (firm i gives to politician j for the first time in L=3 years). The effect appears in year t and declines (Senate about 8.5 pp to about 3 pp by t+3). Corporate PAC donations show larger effects than CEO or employee donations, though all show ties. Effects are particularly stronger for committee membership. | Design hints only: an onset feature (first gift to m in 3 years) instead of gave-in-cycle, a short decay horizon, and a C2 x C3 interaction. They must fit the 10-feature cap and the prereg/one-look discipline. Kim's outcome is lobbying a related bill, not contact, so the paper cannot serve as validation (LobbyView-as-validation is barred). Sample transfer is weak: public manufacturing firms vs financial associations and CUs. | moderate | VERIFIED: DRAFT spec channel C3 (donation co-occurrence): org-PAC and (with Gate G1) lobbyist money reaching member m near quarter q, as gave-in-quarter, gave-in-cycle and amount tiers; zero = no-evidence; binding bar on donation-predicts/causes wording. C2 = dated committee activity on cited bills as venue relevance. Fused feature budget hard cap 10. |
| [[bacik-2025-measuring-dynamical-evolution]] | 2025 | method-source | The paper defines a government-association function g(l,y), which is monotone in year, and a political-association function p(l,y), which is not monotone. Both are built from one-time disclosures of covered positions, and lobbyists give no timelines for those positions, so recency cannot be established. | The g and p definitions can be reused as documented, timestamp-free association labels. They are not contact evidence and must not become edge weights (D-021). Any use needs a 'no timeline' label, because the paper itself says it cannot date the ties. | moderate | VERIFIED-SOFTENED: No shipped code path for the contact-inference lane was found; the nearest shipped surface records position_basis='not_disclosed'. The gold-positive design described for the component is not verifiable from code. |
| [[kim-2018-lobbyview-firm-level]] | 2018 | supports | The Congress assignment takes an argmax over candidate Congresses (Eq. 1) and falls back to the filing-year Congress when no bill matches. The paper says filing-year guessing often leads to erroneous matches (Section 2). It says the full methods description is still to come, with the quoted sentence 'This document currently contains only some technical details of the database' (Introduction). | Paper claims hold, with two corrections. The paper never calls title or range matching heuristic. The methods-deferred quote sits in the Introduction (Section 1), not Section 4.1. The claim that Paul's memos treat LobbyView as a cautionary chain because argmax always returns an answer was not found in the brain vault and is dropped. | moderate | VERIFIED-SOFTENED: The brain memo on the contact-inference design, Claims discipline bullet (line 55), requires a mandatory caveat that LDA does not require disclosure of contacts and that scores are inferences. It bars LobbyView-as-validation and lobbied-member wording for inferred rows. The memo does not mention argmax or any cautionary lineage. |

**Assessment.**

**Evidence weight: design input only, no validation. This is a validation GAP.** 3 rows (1 VERIFIED, 2 VERIFIED-SOFTENED).

**No code path could be checked.** The Bacik row's code check found no shipped code path for this lane. The nearest shipped surface records `position_basis='not_disclosed'`, so the gold-positive / roster-negative design described for the component cannot be confirmed from code. The component's own code_refs say 'path not recorded'.

**What each row contributes:**
- **Kim, Stuckatz & Wolters 2020, VERIFIED.** Design hints for donation channel C3: an onset feature (first gift in 3 years) instead of gave-in-cycle, a short decay horizon, and a C2 x C3 committee interaction, all within the 10-feature cap. Its outcome is lobbying a related bill, not contact, so it cannot validate this lane; LobbyView-as-validation is barred. Sample transfer is weak: manufacturing firms vs financial associations and CUs.
- **Bacik et al. 2025.** Its g(l,y) and p(l,y) functions can be reused only as timestamp-free association labels with a 'no timeline' tag. They are revolving-door based, so influence language is barred, and they are not edges or weights (D-021).
- **Kim 2018.** The argmax fallback to the filing-year Congress is verified. But the verifier did not find, and dropped, the claim that Paul's memos treat LobbyView as a cautionary chain on that ground. The memo does contain a mandatory caveat that LDA does not require disclosure of contacts and bars the 'lobbied-member' label on inferred rows.

Nothing addresses the component's empirical question, the precision of description-based inference against blind labels. Nothing tests its assumption that roster-join negatives are not contacts.

**Contradictions.**
- The stated Paul position (memos treat the LobbyView lineage as a cautionary chain because argmax always returns an answer) was not found in the brain vault; the verifier dropped it. The verifiable position is narrower: a no-disclosure caveat plus bans on LobbyView-as-validation and on lobbied-member wording.
- The component describes a gold-positive / organization-split negative design with a contamination recall sample, but no shipped code path could be located to confirm it exists.

**Gaps.**
- Precision against blind labels. Needed: a random sample of LD-2 activity descriptions, coded blind by at least two coders for whether a specific member was contacted, reporting precision and recall with the window and denominator named.
- The negative-class assumption. Needed: a recall sample drawn from roster-join 'negatives' to estimate how many are true contacts (contamination rate), stratified by organization type (association vs firm).
- An external contact criterion with a register-style source naming both parties (the Azaiez-style benchmark), if any US source can be found.

**Paul's position vs the evidence.**

The defensible part of Paul's stance, borrowing only documented definitions and refusing LobbyView-as-validation, is consistent with the evidence. The stronger stated rationale (an argmax 'cautionary chain') is not documented in the vault. Nothing in the evidence shows that contact can be inferred from activity text at any precision. Until that is measured, any output must stay labelled as an inference.

**Open questions.**
- Where is the code for the contact-inference lane, and was the evaluation ever run (the memory index says UNMERGED)?
- Should the hub's Paul-position text be corrected to the documented caveat-and-ban wording?

**Read first.** [[bacik-2025-measuring-dynamical-evolution]], [[kim-2020-strategic-sequential-links]], [[kim-2018-lobbyview-firm-level]], [[azaiez-2025-hypergraph-analysis-european]]

## Ridge-logistic cosponsor forecaster

**What it computes.** Predicted cosponsor joins on open bills. A frozen forecast of 265,396 predictions is unscored.

**Method.** Ridge logistic regression. Scoring against the lobbied-member table would be circular.

**Code refs.** `fec/lobbying/cosponsor_forecast.py`, `fec/engine/forecast_ledger.py`

**Assumptions.**
- Network and lobbying features predict joins

**Evidence.** 5 rows (4 VERIFIED-SOFTENED, 1 UNVERIFIED); 5 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[zareie-2020-similarity-based-link]] | 2020 | suggests-extension | DICN = (1 + CN)(1 + Corr), where Corr is the Pearson correlation of second-order neighbourhood vectors. At beta = 80% training it has the highest mean AUC (15 random splits) in 8 of 9 networks (e.g. NSC 0.9747 vs CNDP 0.9195; KHN 0.8839 vs SHOPI 0.8509). It loses on UAL (0.9367 vs NCC 0.9416). On test pairs whose nodes share no common neighbour (Exp. 3, compared only with PA, CCPA and SHOPI), it is best in 5 of 9 networks (e.g. KRT 0.8319, NSC 0.8471). | The paper uses random edge holdout on nine static benchmark graphs (34-3,772 nodes) that it does not describe as weighted or directed; it lists those as future work. The forecaster is temporal (the Congress t-1 graph predicts Congress t joins) and bill-level. Its prior graph is an undirected weighted projection, and its affinity feature is ordered (member, sponsor). Paper margins over the best baseline run from about 0.007 to 0.055 AUC, with no variance reported. A sponsor-candidate CN or DICN feature on the prior-Congress graph is a cheap candidate to test against the affinity baseline. Exp. 3 concerns pairs with no common neighbour, which is a different subset from pairs with zero direct prior affinity, so it should not be read as evidence about zero-affinity pairs. Any lift is UNVERIFIED until shown on the forecaster's own temporal split (DE-062). | moderate | VERIFIED-SOFTENED: The forecaster scores (bill, member) candidates with ridge logistic regression on prior-Congress HR/S features. The only dyadic feature drawn from the cosponsor graph is the direct prior member-to-sponsor affinity count and rate. The other dyadic terms are attribute-based: same party, state and chamber, NOMINATE distances, and an optional prior committee co-service term. Prior degree and eigenvector centrality are member-level and are computed on an undirected weighted sponsor-cosponsor projection. There is no common-neighbour, Jaccard, Adamic-Adar or second-order similarity feature between sponsor and candidate. |
| [[leskovec-2010-predicting-positive-negative]] | 2010 | suggests-extension | Using all 16 signed directed triad counts (which include the 4 positive-only path types) instead of only those 4 positive-only path counts raises accuracy for predicting a positive edge on matched non-edge pairs (chance 0.50): 0.5612 to 0.5911 Epinions, 0.5579 to 0.5953 Slashdot, 0.6983 to 0.7114 Wikipedia (gains 1.3 to 3.7 points). | Gains are 1.3 to 3.7 points on a matched-embeddedness task and may vanish against a cosponsorship-history baseline, which the forecaster must beat first. Only references dated before the forecast Congress may enter. The paper's comparison also changes feature count, so the gain is attributable to the added signed types only. | weak | VERIFIED-SOFTENED: cosponsor_forecast.py feature list uses prior cosponsorship history, network degree and eigenvector, NOMINATE, party, state, chamber, seniority, committee terms, bill type and policy area. It has no signed or mention-derived feature. prior_sponsor_affinity is a count of prior-Congress HR/S cosponsorships with the sponsor. METHOD_NOTE confirms a fixed ridge lambda of 1.0. |
| [[tang-2015-negative-link-prediction]] | 2015 | supports | Accuracy is misleading under extreme imbalance: a trivial all-missing classifier reaches 99.99 percent accuracy (Sec. 5.1 pp. 92-93). Imbalance is 1:4131 and 1:2534 (Sec. 4.1 p. 90). Parameterized baselines are reported at their best setting, and NeLP parameters are set per site, with no held-out tuning protocol described (Sec. 5.2 p. 93). | The metric choice carries across domains. Tang's unspecified tuning protocol and best-setting baseline reporting are a negative example of protocol, not a template for the system. | weak | VERIFIED-SOFTENED: Confirmed: precision_at_k uses cutoffs 10, 25 and 50, bill-averaged (lines 1796-1853). split_verdict compares model average precision with the best of named baselines B1 member propensity, B2 sponsor affinity and B3 party match (lines 1865-1869 and 2063-2105). Average precision is the comparison metric, and no accuracy metric appears in cosponsor_forecast.py. Not confirmed: the frozen-before-scoring clause. That file has no freeze or pre-registration language, so the clause is removed here unless another artifact shows it. |
| [[boxsteffensmeier-2018-taking-congress-interest]] | 2018 | suggests-extension | Bill-level endorsement signals predict cosponsor counts: max endorser standardized eigenvector centrality coef 0.055 (SE 0.011) and endorser count coef 0.007 (SE 0.001), both p<.001, negative binomial with Congress FE, N = 6,597 House bills with supportive Dear Colleague letters, 106th-111th Congresses. Predicted cosponsors: 33 for a bill endorsed only by unconnected groups (score -0.2053) or with no endorsements; 38 with centrality 2.51 (Anti-Defamation League); 51 with centrality 7.66 (Planned Parenthood). | Paper estimates are pooled across Congresses and both parties with no party or majority strata reported, so quote as UNSTRATIFIED. The outcome is a bill-level cosponsor count, not a dyad join. Any added bill-level signal must be dated before introduction. Endorsements are public pro-bill signals, but the paper does not show they reflect group influence because solicitation direction is unresolved (footnote 11). LDA counts are not a substitute under D-021. | moderate | VERIFIED-SOFTENED: cosponsor_forecast.py feature list (lines 117-142) is a ridge-logistic model. It uses member-dyad features (prior sponsor affinity, prior member cosponsorships, network degree and eigenvector, NOMINATE distances, party, state, seniority, prior coservice, member on referred committee) PLUS bill-level and sponsor-level covariates (bill_committee_referrals, bill_type, policy_area, sponsor_majority_party). The analyst claim of member-dyad features only is wrong. No endorsement or outside-support feature exists (confirmed). The 265,396-row count and unscored status were not found in the repo. |
| [[selfhout-2010-emerging-late-adolescent]] | 2010 | suggests-extension | Reciprocity 1.73 (SE .11), transitivity .14 (SE .02) and distance-2 -.52 (SE .06) were all significant. Adding transitivity and distance-2 improved fit in all 10 groups (chi-square 15.68 to 93.43, df 1), so a model of independent pairs misfits network tie formation. | Transfer is moderate in method and weak in evidence. Reciprocity and closure in friendship nominations do not establish the same effects in cosponsorship; the component's own question about Brandenberger reciprocity is the legislative test. Here 'reciprocity' means tie reciprocation and must not be confused with the money reciprocity that is still unvalidated (D-104..D-108). Any added feature counts only after the frozen 265,396-prediction forecast is scored against a non-circular outcome, and stays UNVERIFIED until a second path reproduces it. | weak | UNVERIFIED: The ridge-logistic forecaster uses pair and individual features: prior sponsor affinity, prior member cosponsorships, network degree and eigenvector, NOMINATE distances, same party/state/chamber, committee features. It has no term for reciprocation (has the sponsor previously cosponsored this member's bills?) and none for closure (shared cosponsor partners). |

**Assessment.**

**Evidence weight: weak to moderate on features and metrics. No evidence that the model has skill.** 5 rows (4 VERIFIED-SOFTENED, 1 UNVERIFIED).

**The central question is untouched.** The forecast is unscored, and no row bears on whether it beats a cosponsorship-history baseline. The system-side counts were not found in the repo: the 265,396-row count and the 'frozen-before-scoring' clause.

**What the rows support:**
- **Tang 2015, metric choice.** Under extreme class imbalance (1:4131, 1:2534), accuracy is meaningless. The forecaster correctly uses average precision and bill-averaged precision@10/25/50 against named baselines (B1 member propensity, B2 sponsor affinity, B3 party match). Tang's own tuning protocol is a negative example.
- **Box-Steffensmeier et al. 2018, candidate feature.** Endorser eigenvector centrality (coef 0.055, SE 0.011) and endorser count (0.007, SE 0.001) predict cosponsor counts. Setup: N = 6,597 House bills with supportive Dear Colleague letters, 106th-111th Congresses, negative binomial with Congress FE. Quote as UNSTRATIFIED (pooled across parties and majority status). The outcome is a bill-level count, not a dyad join, and the paper leaves the direction of solicitation unresolved (fn 11). This row also corrects an analyst claim: the model does include bill-level covariates.
- **Zareie 2020.** A common-neighbour or DICN sponsor-candidate feature is a cheap candidate. Its AUC margins of 0.007-0.055 come from random edge holdout on static, unweighted benchmark graphs, not a temporal split.
- **Leskovec 2010.** Signed triads add 1.3-3.7 accuracy points, on a matched-embeddedness task.
- **Selfhout 2010 (UNVERIFIED, lead only).** Reciprocity and closure in adolescent friendship networks. 'Reciprocity' there means a tie being returned. It is not the money reciprocity that remains unvalidated (D-104..D-108).

**Feature list vs assumption.** None of the rows' readings of the feature list contains a lobbying feature. The features are prior affinity, cosponsorships, degree and eigenvector centrality, NOMINATE, party, state, chamber, seniority, committee terms, bill type and policy area. That does not match the component's assumption that 'network and lobbying features predict joins'.

**Contradictions.**
- The component assumption says lobbying features predict joins. The feature lists read from cosponsor_forecast.py in three rows contain no lobbying feature.
- The component states 265,396 frozen, unscored predictions. Neither the count nor any freeze or pre-registration language was found in the repo, so the forecast may not be demonstrably frozen before scoring.

**Gaps.**
- Out-of-sample skill. Needed: score the forecast against realized cosponsorship in the target Congress (never against the lobbied-member table, which is circular). Report AP and precision@k against B1-B3 plus a Brandenberger-style reciprocation baseline, stratified by chamber and majority status.
- A temporal test of any added feature (CN/DICN, endorsements, signed triads) on the forecaster's own Congress t-1 to t split, run only after the base forecast is scored, under one-look discipline.

**Paul's position vs the evidence.**

'Prediction, not reaction' matches the design: temporal split, ranking metrics, named baselines. The evidence does not yet show prediction works, because the forecast is unscored. Paul's critique of Kim J. 2025 does not bear on this, since the positions paper is not a cosponsorship forecaster. None of the papers argues for dropping regular cosponsorship edges or for per-edge volume normalization, so neither rejected SNA proposal is in play here.

**Open questions.**
- Is the forecast actually frozen (with a file hash and date) before scoring, and where is the 265,396 count recorded?
- Does any lobbying-derived feature enter the model? If none does, should the component's assumption be rewritten?
- Which baseline does Paul treat as the bar: B2 sponsor affinity, or a reciprocation baseline?

**Read first.** [[boxsteffensmeier-2018-taking-congress-interest]], [[tang-2015-negative-link-prediction]], [[zareie-2020-similarity-based-link]], [[leskovec-2010-predicting-positive-negative]]

## Contested-bills registry

**What it computes.** Per-bill mode (standard, advisory or contested) adding a counterparty term (Romero Moreno 2021).

**Method.** Loader shipped; engine wiring not built.

**Code refs.** `fec/lobbying/contested_bills.py`

**Assumptions.**
- Owner ratification is the only admission route

**Evidence.** 6 rows (2 VERIFIED, 4 VERIFIED-SOFTENED); 5 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[bombardini-2020-empirical-models-lobbying]] | 2020 | suggests-extension | Kang (2016) models lobbying as Tullock contests among four energy coalitions, mapped at the level of policy provisions decoupled from bills. Policies with no lobbying are enacted about 1% of the time and those with two or more lobbying players about 24%, which the review says implies substantial selection. The structural effect of lobbying is much smaller (0.021%-0.415%); entry has multiple equilibria resolved by a selection assumption. | A contest success function is a candidate form for the planned counterparty term, but Kang's model carries misspecification symptoms and selects among multiple equilibria by assumption. The 1% vs 24% contrast reflects selection and must not be quoted as a lobbying effect. Weighting contests by LDA spend would conflict with D-021. | weak | VERIFIED: A per-bill contested or advisory registry with explicit counterparty industries. The loader has shipped. No engine consumer exists, and the CONTESTED_V1 dial is only planned. |
| [[pritoni-2025-lobbying-coalitions-determinants]] | 2025 | supports | Issue-context hypotheses are supported 6 of 7 (85.7%; 16-article subsample, unstratified by polity). The authors conclude that groups coalesce more in a highly salient and conflictual policy context (condition i). Similar groups are not the only ones to ally: 'strange bedfellows' coalitions form when the policy context makes them strategically useful (Junk 2019; Beyers and De Bruycker 2018). Prior work found that issue characteristics can matter more than organization-level factors. | The evidence comes from interest-group studies, mostly US and EU, about whether groups form coalitions, not whether they win. It supports per-bill alignment as a structural choice. It says nothing about how large any counterparty term should be, which stays owner policy. | moderate | VERIFIED-SOFTENED: The registry stores an explicit per-bill counterparty set, never a global one (docstring lines 4-10). Standard mode is the default when no registry or record exists. Each record needs a non-empty ratified_by and a ratified_date. No consumer is wired yet: the CONTESTED_V1 dial is described as future. |
| [[pritoni-2025-lobbying-coalitions-determinants]] | 2025 | suggests-extension | Relative-weakness hypotheses are supported 11 of 12 (91.7%; Table 4, 16-article subsample, unstratified). The paper says weaker groups 'lobby in packs' to pool resources. A significant minority of studies (Beyers and Donas 2014; Gray and Lowery 1998) finds the opposite, which suggests a minimum resource threshold. The authors leave the question open. | An ally field would be descriptive (who stood with CUs on bill X). LDA may identify allies, but under D-021 co-lobbying cannot become an edge or a weight. The coalition-to-influence link is contradictory (p. 8), so the field should not change any score without its own validation. | weak | VERIFIED: INDUSTRY_SIDE makes credit_union the only 'ours' industry. Community banks, fintech, crypto, telecom and credit_arms are all 'opposition'. The registry rejects any ours-side industry in counterparty_industries, so a record can hold only counterparties and has no field for an allied industry. |
| [[kim-2025-measuring-interest-group]] | 2025 | suggests-extension | Bill-level positions show opposing interest groups on the same bill. Coal-deregulation bills (H.R. 3409, 112th; H.R. 806, 115th) are opposed by environmental groups and supported by commercial entities, while an environmental-protection bill (S. 847, 112th) shows the reverse. Subject Oppose ratios: Public lands and natural resources 0.354 (51 bills), Environmental protection 0.280 (108 bills). The paper does not tabulate inter-industry conflict per bill. | Kim's data ends at the 117th Congress (2022). It can nominate historical candidate contested bills and calibrate the alliance-conflict recompute. Current bills depend on the weaker Haiku lane. Registry admission stays an owner ruling. An OPPOSED pair from inferred labels is a lead, not a fact. | moderate | VERIFIED-SOFTENED: The contested-bills registry is a loader for per-bill counterparty sets that Paul ratifies. No consumer exists today (CONTESTED_V1 is unopened), and the bundle surfaces it for display only. Separately, org_stance_priors recomputes ALIGNED, OPPOSED and UNKNOWN per bill for each directional sector pair from label_3_class. It raises CCCAValidationError if a CCCA anchor bill reads OPPOSED for the bank/credit_union pair. |
| [[alexander-2009-lobbying-analysis-under]] |  | suggests-extension | Five firms lobbied against the AJCA for alternative tax plans and are not counted in the lobbying group (fn 99, p.28). A sensitivity analysis drops five repatriating lobbyists with uncertain support and raises ROI to 228:1 (fn 100, p.30). Coalition lobbyists out-returned solo lobbyists (257 vs 154). The paper does not describe the provision as lacking organized counter-lobbying. | The paper supports only a design implication: returns on a provision with little organized opposition may overstate what contested bills would show. The counter-lobbying evidence is five firms on the other side, so the uncontested label should be softened to five firms lobbied against it. The paper never studies contested bills. | weak | VERIFIED-SOFTENED: fec/lobbying/contested_bills.py is a per-bill standard, advisory or contested registry. Its docstring gives ratification 2026-07-31, contested records supply an explicit per-bill counterparty set, and no consumer reads it yet (CONTESTED_V1 is planned). Verified. |
| [[moreno-2021-shadowing-shielding-effective]] | 2021 | method-source | Against a passive opponent with known allocation w_b, the low-allocation optimum has a term linear in w_bi with common coefficient c = (Ba/Bb - 1): shadow the opponent's targets if Ba > Bb, avoid them if Ba < Bb (Eq 5, low-allocation limit only; a degree term enters only when positivity constraints bind). Continuous shadowing plus shielding is within 1.89e-3 of the numerical optimum under budget advantage (degree 2.09e-2, uniform 2.14e-2). Under disadvantage it is 7.28e-4 vs 2.12e-3 for discrete degree targeting (K=16, 15 instances). | Design template only. w_b is relational FEC-associated exposure drawn from a sample of 10 companies per industry (build_opposition_prior.py line 108; not industry-complete), not LDA (D-021). The paper's dynamics are an unnoised voter model on one academic email network, so c and the gap sizes are not calibrated for Congress. The sign of the response needs the per-bill budget ratio measured first. UNVERIFIED for this system until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED: contested_bills.py docstring says no consumer exists today for the CONTESTED_V1 dial. The registry is display-only: build_dashboard_bundle.py lines 731-745 loads it and shows it in the posture block with consumer False, fed to no engine. build_opposition_prior.py lines 1528-1542 defines w_b as relational exposure (independent expenditures, communication costs and conduit money removed) summing to 1.0, labelled coalition-builder adversary substrate / shadowing layer. No fec/engine file or moneyball_core.py references it. The registry file contested_bills.json is not present in the repo root. |

**Assessment.**

**Evidence weight: moderate that conflict structure is real and specific to each bill. Nothing sizes a counterparty term.** 6 rows (2 VERIFIED, 4 VERIFIED-SOFTENED).

**The registry is display-only.** It loads in `build_dashboard_bundle.py` with consumer = False. No engine file reads it, CONTESTED_V1 is unopened, and `contested_bills.json` is not present in the repo root.

**Structure (Kim et al. 2025 and Pritoni 2025).** Kim shows opposing interest groups on the same bill. Coal deregulation, for example, is opposed by environmental groups and supported by commercial entities. Oppose ratios by subject: Environmental protection 0.280 (108 bills), Public lands 0.354 (51 bills). The data end at the 117th Congress (2022) and give no per-bill tabulation of conflict between industries. Kim can nominate historical candidate bills; it cannot certify current ones. Pritoni (16-article subsample, unstratified by polity, mostly US and EU) finds issue-context hypotheses supported in 6 of 7 tests: groups coalesce more in salient, conflictual contexts. That supports per-bill alignment rather than a global one. Pritoni studies coalition formation, not winning.

**The cited method (Romero Moreno 2021) is a template, not calibration.** Against a passive opponent, the low-allocation optimum shadows the opponent's targets if Ba > Bb and avoids them if Ba < Bb. That result comes from an unnoised voter model on one academic email network, so the sign of the response depends on a per-bill budget ratio the system has not measured. The planned w_b is FEC relational exposure from 10 sample companies per industry, which is not industry-complete. It stays UNVERIFIED for this system (DE-062).

**Cautions:**
- Kang's contest model (via Bombardini, VERIFIED) is a candidate functional form, but its 1% vs 24% enactment contrast reflects selection, not an effect of lobbying.
- Alexander's AJCA ROI (228:1 sensitivity) concerns a provision with only five firms lobbying against it. Returns on lightly contested provisions likely overstate what contested bills show.
- Weighting contests by LDA spend would conflict with D-021.

**Contradictions.**
- The empirical question ('does relative lobbying spend predict outcome?') uses LDA spend as the predictor. Under D-021, LDA spend may be a descriptive outcome or identity attribute but cannot become an engine weight. Any counterparty term sized by LDA spend would breach it.
- The registry's schema rejects any 'ours'-side industry in counterparty_industries and has no ally field. Pritoni finds weaker groups 'lobby in packs' (11 of 12 tests), so the registry cannot record the coalitions the evidence says form.

**Gaps.**
- Outcome evidence on contested bills. Needed: a retrospective study of bills where bank and CU positions oppose each other. Candidates come from Kim 2025, 2009-2022, ratified by Paul. Measure outcome (passage, markup or amendment adoption) against relative position counts or a non-LDA resource measure, with window and denominator named and results stratified by chamber.
- A measured per-bill budget ratio (Ba/Bb) before any Romero-Moreno-style shadow/avoid sign is adopted.

**Paul's position vs the evidence.**

Paul's view that explicit conflict between industries over a bill is 'vital for political intelligence' is supported as a structural claim: conflict is common, specific to each bill and context-driven. It is unsupported as a quantitative one. No paper sizes the effect of conflict on outcomes for these industries. The evidence does support keeping registry admission as an owner ruling and treating inferred OPPOSED pairs as leads.

**Open questions.**
- Should the registry gain a descriptive ally field (display only, no score effect)?
- Where is contested_bills.json, and are there any ratified records yet?
- Would the counterparty term be sized by anything other than LDA spend, to stay within D-021?

**Read first.** [[kim-2025-measuring-interest-group]], [[moreno-2021-shadowing-shielding-effective]], [[pritoni-2025-lobbying-coalitions-determinants]], [[bombardini-2020-empirical-models-lobbying]], [[alexander-2009-lobbying-analysis-under]]

## Across components

**1. LD-2 contains no contact, and every component inherits that.** Bacik, Kim & Kunisky, Kim 2018, Kim 2025 and Box-Steffensmeier all agree that filings do not name members. Several paths turn org-to-member links into something like contact or influence, and none of them escapes this:
- the lobbied-member link (refuted)
- contest_index via build_dyad_signs (a live leak)
- Paul's 2026 Cross Industry report interaction counts (probable leak)
- 'champions' ranks
- contact inference (unvalidated)

D-021 is the right fence. The open task is to find every consumer of the roster join and label it as overlap with bill rosters.

**2. The Kim, Stuckatz & Wolters design is the one non-circular template.** The roster join works as an outcome against a treatment timed independently of it. That template applies in three places:
- the bridge (a successor test)
- contact inference (onset features)
- the member link (a committee-referral channel)

Its sample (public manufacturing firms, 2008-2018) transfers weakly to financial associations and CUs. Use it as a design, not a number.

**3. The vintage and the rebuild parser are the shared data risk.** LobbyView coverage is reliable through 2021, and the post-2021 collapse is UNVERIFIED as an export artifact. The 2021+ rebuild departs from LobbyView in two ways: no Congress lookback or report-level propagation, and no hyphen-range expansion. Neither error rate is measured. Those errors feed the bridge Y, the member-year network and the forecaster's history. One measurement on an overlap year would serve all three.

**4. Unreproduced system-side figures (DE-062).** The following could not be found or reproduced in the repo during verification:
- 265,396 frozen predictions
- bridge raw rho +0.153, and Pearson vs Spearman
- the composition of ABA $228M / CUNA $93M
- 22 firms / 56 positions / 1 directional
- the frozen-before-scoring clause

Treat them as recorded leads, not findings.

**5. Wording.** These need fixing:
- The '42-month lead' is a freshness gap.
- The bridge rho must carry an UNSTRATIFIED label (pooled over fintech and crypto, CU not entering).
- The Box-Steffensmeier coefficients are pooled across parties.
- Revolving-door edges (Bacik g/p, Kim 2025 legislator-lobbyist) carry no influence language.
- 'Reciprocity' in the forecaster sense (a tie returned) must be kept apart from money reciprocity, which is still not validated (D-104..D-108).

No paper in this hub argues for dropping regular cosponsorship edges or for per-edge volume normalization.

## Research agenda

1. Relabel the '42-month lead' as a data-freshness gap in memory, brain and the lda_bridge.lead_months config. This is a wording fix and needs Paul's ruling.
2. Audit every consumer of the sponsor/cosponsor roster join (contest_index via build_dyad_signs, lobby_champions, lobby_interactions, the Cross Industry report) and label each as overlap with bill rosters, not contact.
3. Blind hand-labelled audit of the finance client classifier: precision and recall by class and by HIGH vs LOW confidence, plus a Huneeus & Kim-style recall check for missed names. Correct the 'NAICS is linkage metadata only' text to describe the NAICS fallback.
4. Recompute ABA, CUNA and NAFCU totals split by is_self_filer for a named window, to settle whether $228M / $93M mixes expense and income. Money collection is frozen, so this needs Paul's per-change OK.
5. Measure the rebuild's error against LobbyView on an overlap year (2019-2021): Congress binding without lookback, and missing hyphen-range expansion. Report a citation-level error rate.
6. Check the 2024-10-25 bulk export against the OSF LobbyView release to settle whether the post-2021 issue-code collapse is a vintage artifact.
7. Score the frozen cosponsor forecast against realized cosponsorship: AP and precision@k vs B1-B3 plus a reciprocation baseline, stratified by chamber and majority status. Locate the freeze record and the 265,396 count first.
8. Successor bridge prereg (Paul's gate) on a new vintage: financial-sector first-time donation onset, pair fixed effects or matched DiD, committee-referral outcome channel. Report results by industry, never pooled unlabelled.
9. Contact-inference validation: blind-coded sample of LD-2 activity descriptions for member contact, plus a contamination estimate on roster-join negatives. Locate the lane's code first.
10. Contested-bills outcome study: bank-vs-CU opposed bills nominated from Kim et al. 2025 (2009-2022) and ratified by Paul. Outcome measured against relative position counts or a non-LDA resource measure, with no LDA spend weights (D-021).
