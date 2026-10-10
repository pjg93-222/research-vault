---
type: non-article
title: "House Committee Requests Codebook (unverified metadata hint; no document supplied)"
authors: []
year: ""
venue: "None"
doi: ""
category: "Causal & Methodological"
systems: ["moneyball-rank-pillars"]
components: ["moneyball-rank-pillars.committee-pillar"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "unverified"
source: "[[_sources/anon-nd-house-committee-requests.doc]]"
original_paths: ["C:\\Users\\pgoff\\OneDrive\\Desktop\\Committee Data\\frisch_kelly\\Data-for-Polity-Article\\House-Committee-Request-Data-extracted\\House Committee Requests Codebook.doc", "C:\\Users\\pgoff\\OneDrive\\Desktop\\Committee Data\\frisch_kelly\\House-Committee-Request-Data\\House Committee Requests Codebook.doc"]
sha12: ["ecf4f70c2541"]
tags: [paper, causal-methodological, type/non-article]
---

# House Committee Requests Codebook (unverified metadata hint; no document supplied)

**Unknown author (n.d.).** *None.* Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> **No document was read.** The extraction returned 0 characters. A targeted search of `C:\Users\pgoff\Documents\research` for "frisch", "kelly", "request" and "codebook" found only four unrelated LobbyView codebooks (clients, issues, network, reports). So this paper has no findings to relate to the system.
> 
> Paul's annotation identifies the work as **Frisch & Kelly 2007, House Committee Request Data**. That is a dataset, not an argument. It is generally known as a record of the committee assignments House members asked for, set against the assignments they got. That description comes from general knowledge, not from the codebook, so it is UNVERIFIED (DE-062) and supports no numbers here.
> 
> The dataset could matter for one component. The Committee/Influence pillar (40% of the live blend) scores seats with owner-set tiers and multipliers: Highly 3.0, Moderately 1.8 and Slightly 0.9, with a subcommittee seat at 1/6 of its parent, plus a Chair/Ranking/Vice ladder (`fec/engine/committee_scores.py:25-36`). It treats a seat as structural CU power and never asks how the member came to hold it. Paul flags a chicken-or-egg problem between partisanship and committee selection. Request data is the standard tool for separating self-selection (members ask for committees that match their interests) from the effect of holding the seat. If a member asked for Financial Services, a high committee score may reflect prior CU interest rather than leverage.
> 
> The data would also come with limits. It covers past Congresses, the House only, and freshman or transfer requests. It does not cover the 119th Congress, so it can inform design but cannot be joined to the current roster.
> 
> **What Paul should do:** supply the codebook PDF so the extraction can be rerun. Until then this entry should carry no weight in any decision.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

## Key findings


## Limitations

- The task supplied no file path and reported 0 characters of full text, so the work could not be read or extracted.
- The task's metadata hint ('House Committee Requests Codebook') could not be matched to any file. A targeted search of C:\Users\pgoff\Documents\research for 'codebook' returned only four unrelated codebook PDFs under _raw\zips: clients_codebook, issues_codebook, network_codebook, reports_codebook. None was used, because no file was named and matching them would be a guess.
- Paul's markup was not read. The task listed no annotation files. The _annots and annotations folders were not searched.
- No fields were filled from memory or inferred from the title. Paul should supply the PDF path, or confirm which of the codebook PDFs above is meant, and the extraction can be rerun.

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/moneyball-rank-pillars#Committee - Influence pillar\|Committee - Influence pillar]] | suggests-extension | UNVERIFIED and unread: the codebook (0 chars extracted) could not be opened. Paul's annotation names it 'Frisch & Kelly 2007, House Committee Request Data'. From general knowledge only, it records House members' committee-assignment requests next to the assignments they received. No numbers can be quoted. (none; document not supplied (codebook unread)) | The Committee pillar prices seats as structural CU power. Each seat gets an owner-judgment tier weight: Highly 3.0, Moderately 1.8 or Slightly 0.9, with subcommittee seats at 1/6. Leadership multipliers are Chair 3.0 full / 7.5 sub, Ranking 2.0 / 4.5 and Vice 1.0 / 2.0. The score is (Membership + Leadership) / 2. Nothing models why a member sits on the committee: self-selection by prior interest is not separated from leverage conferred by the seat. The pillar feeds Influence at 40% of the composite. `fec/engine/committee_scores.py:25-36` | This applies only if the codebook confirms request-vs-assignment fields. Even then the data are historical (not the 119th Congress) and House-only, and they cover new assignments and transfers rather than continuing seats. They could support a design check: whether high committee scores reflect pre-existing interest (selection). They could not be used as a live input or an edge or weight source. Strength is weak until the document is read and a second path reproduces any result (DE-062). | weak | UNVERIFIED |

## Gaps and flags

- The Committee pillar has no selection model. Seat weights treat holding a CU-relevant seat as leverage, with no correction for members who requested that committee because they already held CU-aligned interests. Request-vs-assignment data is the usual way to test this, and Paul's own chicken-or-egg note points to it.
- The four-pillar blend has no external check that committee scores carry information beyond prior interest. The open question 'do committee scores predict CU-bill markup or advancement' cannot separate the two effects without a selection instrument.
- NO DOCUMENT READ: the extraction was 0 chars and no matching file exists in C:\Users\pgoff\Documents\research; only the four unrelated LobbyView codebooks under _raw\zips were found. Every statement about the dataset's contents comes from general knowledge and is UNVERIFIED (DE-062).
- The vault slug 'anon-nd-house-committee-requests' carries no author or year. Paul's annotation attributes it to Frisch & Kelly 2007. The vault metadata should be corrected once the PDF is supplied.
- No conflict with brain rules. The dataset is not proposed as an edge or weight source, and it makes no SNA, reciprocity or influence-language claim.

Paul's annotation lists only the citation, with no takeaways or system links. It fits his chicken-or-egg note on the Committee pillar (under Kim et al. and Huneeus & Kim's committee-assignment instrument): request data is the natural way to separate self-selection into committees from the effect of the seat. That reading fits his concern but is untested, because the document was never read. His note is not stale relative to the system: the built pillar (committee_scores.py) still ignores selection entirely.

## Related papers

- [[porter-2005-network-analysis-committees]] — complements — Porter et al. map House committee structure from assignment co-membership. Request data would explain how members sort into that structure, rather than describing it after the fact.
- [[thakur-2025-matching-theory-perspective]] — complements — A matching-theory account of how committees are assigned. Request-vs-assignment data is the empirical input such a matching model takes.
- [[huneeus-2018-effects-firms-lobbying]] — complements — Paul cites Huneeus & Kim's committee-assignment instrument. Assignment requests bear on whether assignment is plausibly exogenous to member interest.

### Linked from
_None._