# Completeness and accuracy review of the research vault (2026-10-09)

Scope: read-only critic pass. No note was edited. The scripts are in the session scratchpad (`excl_scan.py`, `sys_scan.py`, `comp_scan.py`, `gapchk.py`, `annot_scan.py`, `annot_contam.py`, `unver_rows.py`, `count_check.py`, `wording_scan.py`, `theme_links.py`, `annots_map.py`, `excl_overlap.py`, `dropped_scan.py`). Line numbers refer to `_text/<sha12>.txt` unless stated otherwise.

## Headline

1. **Paul's own prose is missing from, or attached to the wrong, paper notes.** This is the largest defect, and it breaks the "annotations read first" requirement at the paper level. At least 13 paper notes that Paul annotated in prose show only a citation line, say "Not in Paul's annotated bibliographies", or carry another work's annotation. Two notes also carry unrelated forecast-source material. Downstream verification passes then marked true statements about Paul's views as "unsupported" (details in 1b).
2. **Fink et al. 2023 has no paper note.** It is a load-bearing work: it has prose in Curated and in SNA, 23 PDF highlights, 23 mentions in the framework and mentions in 5 system notes. It was misclassified in `excluded.md` as `dataset-or-code-doc`.
3. **21 of the 127 registry components have zero evidence rows.** 10 have an explicit "(none: GAP)" table and 11 have no evidence table at all, only a prose "No evidence rows" statement.
4. **The 8 sampled paper notes are numerically accurate against their source text.** The errors are elsewhere: claims that the notes' own failure boxes refuted are still in the summaries and gaps, the status wording is stale, one research note has the wrong author, and one relation label overclaims.
5. **No barred wording was found unguarded.** The framework, though, has drifted from the system assessments. It states some causal and timescale claims that the system notes have since contradicted.

---

## 1. Coverage

### 1a. Academic files in excluded.md that should have been papers

All 701 rows were scanned for academic markers (abstract, references, DOI, journal, et al.). Rows tagged `paul-*` and IBISWorld, Statista and disbursement rows were skipped.

| Priority | File (sha12) | Current class | Why it should be a paper |
|---|---|---|---|
| **High** | Fink, Omodt, Zinnecker et al. 2023, *Data in Brief* 50:109521, "A Congressional Twitter network dataset quantifying pairwise probability of influence" (`445139e681c1`) | dataset-or-code-doc, yes | Paul wrote prose on it in Curated Annotations and in SNA (Viral Centrality, sign of replies and mentions, temporal weighting). It has 23 highlights in `_annots/445139e681c1.json`. It is cited throughout `framework.md` and `systems/melange-sna`, `melange-staffer-network`, `moneyball-rank-pillars`, `melange-stance` and `melange-influence-engine`. It has no paper note and is not in `missing.md`. Framework open question 19 already admits the gap. |
| High | Porter, Case & Treul 2025, *Scientific Data* 12:1237, CampaignView (`7e7d42901395`) | dataset-or-code-doc, yes | A peer-reviewed data article. `missing.md` and the framework's "Cited but not on disk" list it as **not on disk**, which is wrong. |
| High | Delano, Rudkin & Kim 2025, *Scientific Data* 12:1567, BICAM (`58dbc0a2b055`) | dataset-or-code-doc, yes | Same problem: `missing.md` says it is not on disk, but it is here. It shows the LDA-to-congressional-data linkage that `melange-lobbying` relies on. |
| Medium | McCartan, Kenny, ... & Imai 2022, *Scientific Data* 9:689, simulated redistricting plans (`b4df69e16e12`) | dataset-or-code-doc, maybe | A sibling of the two redistricting paper notes (becker-nd, best-2022). It is peer-reviewed. |
| Medium | Sverdrup, Hadad, Athey, Tibshirani & Wager, "An introduction to grf" (`c356deb40dc5`) | dataset-or-code-doc, yes | Causal-forest heterogeneity is directly relevant to Terrier persuasion targeting, which currently has GAP components. |
| Low | Zeileis sandwich, HC/HAC and clustered-covariance papers (`02a5be949f84`, `2c2eeae381ad`, `2c95fc847117`) | dataset-or-code-doc, maybe | Peer-reviewed JSS methods papers. `missing.md` lists Cameron, Gelbach & Miller cluster SE as missing, and these are the closest on-disk method sources for the sector-growth inference. |
| Low | A large-scale geographically explicit synthetic population with social networks (`6ec7293aebf2`) | dataset-or-code-doc, maybe | A tangential network and agent-based-model data paper. |

The 10 `academic-article` / `working-paper` / `book-or-chapter` rows marked "no" (Gemini, embodied AI, UAV simulator and similar) are correctly excluded as off-topic.

Excluded rows that are also notes: 4 research notes appear both in `excluded.md` and in `notes/`: `bc2c16a312a8`, `46495cfa916e`, `1cb2685ab2e7` and `17c10df8e2db`. The rescue list is out of date.

The sweep was confirmed against the roots it skipped. The 1,120 rows dropped by `manifest_filter_log.md` are html, md and txt only, with no PDF or docx. The 9 dropped `Draft Systems/research_txt/*.pdf.txt` extractions all have their source PDF in the kept manifest. `C:\Users\pgoff\Mentat\research` holds 2 PDFs, both already papers (RELINK and MOPBD*). `C:\melange-data` holds only R package vignettes. No uncovered article was found.

### 1b. Works in Paul's annotated bibliographies with no paper note

**Denominator:** 83 distinct references across the 12 bibliography documents (78 citations plus 5 bare links). 50 have a paper note. 33 do not:

- **On disk, but with no note (1):** Fink et al. 2023 (see 1a).
- **Listed in `missing.md` as not on disk (32):**
  - Works with Paul's prose: Rongkuan et al. 2024 and Oropeza-Martínez et al. 2024 (both "Priority" designations); Peng et al. 2022; Borg et al. 2015; and the addendum's Leskovec 2007 CELF, Sutton & Barto, Ng et al. 1999, Watkins & Dayan, Khalil 2017 and Manchanda 2020 (GCOMB).
  - Citation-only works: Kempe 2003; Nemhauser 1978; MaDGNN; Amoore; Raychaudhuri (Harrison 2010 is on disk instead); Bischof; Bouton; Ceron; Denton; Gibson; Joathan; Nai; Nickerson; Keele; Alkazzi; Valujeva; Zhang; the Google Books ID; 3 Neural Networks links; and the FAIA link.
  - None of these has been retrieved. Oropeza (traits as edge parameters) and Rongkuan are central to Paul's Chain B and C.

**Paper notes whose Paul-annotation section is wrong.** This was checked by comparing each note's `## Paul's annotation` section with the bibliography text:

| Paper note | What the bibliography has | What the note shows |
|---|---|---|
| `huneeus-2018-effects-firms-lobbying` | Prose in Curated, Lobbying and Trey: fundraising-goal use; donations not separated from events and meetings; representatives not differentiated; the state layer | Title line only. The closing paragraph says "Paul's annotation record for this work is empty: no takeaways" (false). |
| `kim-nd-mapping-political-communities` | Prose in Curated, Lobbying and SNA (champions, congestion, the bipartite-to-graph gap) | "_Not in Paul's annotated bibliographies._" (false). The prose sits on `kim-2020-strategic-sequential-links` instead, together with an Imai & Kim citation. |
| `kim-2023-systemic-sequential-links` (JOP 2026) | Trey prose: the 8-11 pp quote and "favor ... pronounced for only 6 months" | Title line only. Its failure box says "the ext record says no annotation file was found". |
| `furnas-2019-moneyed-interests-information` (the APSA working version of Furnas et al. 2022, "More than mere access") | Prose in Curated and Lobbying (epistemic closure; think-tank allocation) | The citation for a different paper, "The partisan ties of lobbying firms", followed by "Paul recorded no takeaways". |
| `anon-2019-partisan-ties-lobbying` | Listed in the Lobbying bibliography | "_Not in Paul's annotated bibliographies._" |
| `gaynor-2022-financial-ties-bind` | SNA prose (ERGM, DW-NOMINATE nodes, Cook PVI, LPAC directed edges, leadership insulation) | "_Not in Paul's annotated bibliographies._" |
| `bacik-2025-measuring-dynamical-evolution` | Lobbying prose (preferential attachment; sentiment augmentation) | "_Not in Paul's annotated bibliographies._" |
| `bonica-2013-mapping-ideological-marketplace` | Listed in SNA | "_Not in Paul's annotated bibliographies._" |
| `birkenmaier-2025-measuring-politicians-public` | Prose in Curated and ML | Title line only |
| `ramey-2016-measuring-elite-personality` | ML prose | Title line only |
| `alexander-nd-lobbying-analysis-under` | Trey prose ("While this study is an outlier ...") | Title line only |
| `golovin-2011-adaptive-submodularity-theory`, `kamarthi-2019-...`, `chen-nd-complex-contagion-influence` | Addendum prose for each | A title line, or a title plus a link |
| `ren-2022-multi-objective-path` | (Path-Finding: not listed) | Carries Bouton et al. "Small campaign donors" twice, plus five Grand View Research crypto-forecast source entries |
| `report-2024-2024-house-glance` | (not in any bibliography) | Carries the addendum's MaDGNN "Frontier" paragraph, a Florida Revenue Estimating Conference entry and an IBISWorld life-insurance entry |

**Consequence:** verifiers judged true statements about Paul's views to be unsupported.
- `huneeus-2018` failure box: "the claim that Paul calls the alexander item an outlier is not supported." Paul's Trey prose says exactly that.
- `alexander-nd` failure box (line 109): the same removal.
- `kim-2023`: Paul's "6 months" claim could not be checked, so the note attributes a different claim to him (see 2.2).

### 1c. System components with zero evidence

**Denominator:** 127 components in `_work/state/meta.json`, 116 of which have an evidence table. 21 have zero evidence rows.

- **Explicit "(none: GAP)" tables (10):**
  - melange-demographic-potential: v2.1 growth room
  - melange-labels-and-tiers: action-based rank honors; Primacy tier board; per-area primacy grade (D-031)
  - melange-legislative-data: congress.gov activity score with Congress decay
  - melange-psychometrics: interview weighting (C_INT_W) and P6 shrinkage
  - melange-stance: ACU speaker mode (planned)
  - terrier-imputation: prior-corrected rare-class repair; household model; GPU XGBoost and fill-status rules
- **No evidence table, prose "No evidence rows" only (11):**
  - moneyball-rank-pillars: demographic-pillar; sublayer-efficiency-repair; legacy-workbook-cascade
  - mentat-fec-attribution: cash-proportional-attribution; collection-completeness
  - mentat-giving-forecast: slope-anchor-selection; pac-accuracy-attempts; seasonal-naive-baseline
  - terrier-row-scoring: participation-index; transforms-and-rules
  - terrier-canvass: turnout-calibration

  The format is inconsistent with the other 116, so any table-based count misses these 11.
- **Papers cited by no system evidence table (4):** `anon-2022-simulating-role-knowledge` (Paul-annotated, UNVERIFIED), `anon-nd-strategic-identity-signaling` and the two link-list documents.
- **Note-level inconsistency:** `terrier-imputation` says it swept "all 127 paper notes". The vault holds 132.

---

## 2. Accuracy: 8 paper notes re-read against `_text`

**Sample:** 6 VERIFIED, 1 UNVERIFIED and 1 research note. Five are Paul-annotated. The themes covered are Lobbying, SNA, AIM, Adjacent, Personality and Paul's Research Notes.

| # | Note | Status | Numbers checked against source | Verdict |
|---|---|---|---|---|
| 1 | huneeus-2018-effects-firms-lobbying | VERIFIED, Paul | 766/7,646 (l.250); 11.8% (l.251); 0.319 / 0.227 / F 14.30 (l.9986-10030); 0.2% per 1% preferred (l.10061-62); 22% and 41% via entry (l.10394-10403); 38% and 13% sensitivity (l.10446-48); 61,478 and 67,842 names (l.10677, l.203); >80% persistence (l.2566) and "over 70%" (l.8941); coefficient 0.922 (l.8935); HQ-state connection (l.9605-07) | Figures correct. Errors listed below. |
| 2 | kim-2023-systemic-sequential-links | VERIFIED, Paul (Trey) | 8-11 pp (l.19, l.110); 2.3-4.8 pp (l.907); 979 members and 2,703 firms (l.675-676); about 8 rising to 11 pp (l.1145-46, 1178-79) | Figures correct. Paul's position is misattributed (below). |
| 3 | aref-2021-identifying-hidden-coalitions | VERIFIED, Paul | 4,954 ≤ m ≤ 31,936 (l.241); 4-69 members (l.384); 21.18 vs 2.68 and 25.6% vs 8.44% (l.471-473); 3.3 h and 14 h (l.481-483); Holm at α=.01 (l.532); Smith 8.44 and Royce 5.46. The system side was checked against `fec/network/signed_coalitions.py` l.2-12 and l.92-117 (mention layer, local search, diagnostic only). | No errors found. |
| 4 | chen-nd-complex-contagion-influence | VERIFIED, Paul (addendum) | Greedy 0.0396 vs RL4CCIM 0.5347 (l.723, 763, 818); 17 of 70 and 24.3% (l.432-435) vs 24.2% (l.545); ablations 4.6 / 9.3 / 21.3 / 35.2% (l.1031-33); IC/LT independence of neighbour count (l.258, 274) | No errors. The slug says "nd" although the year is known (IJCAI 2023), and Paul's addendum prose is not attached. |
| 5 | linde-2016-politicians-take-risks | VERIFIED | 46 MPs, 30% (l.117, 346); 76.2% right-wing (l.353); 39.13 / 76.09%, p=.0002 (l.495-504); 88.46 / 30% (l.601-602); n=164 (l.564); OR 8.684, p=.033 and OR 14.633 (l.626-629); no loss aversion, house money (l.211-229). Code checked: `receptivity_layer.py` l.22-25 and 208-209 (pt 1.15/0.92, sec<0.45); `money_response.py` l.27-31 and 74 (LOSSFRAME 1.10). | No errors. The "contradicts" bearing on LOSSFRAME is sound. |
| 6 | kalla-2020-which-narrative-strategies | UNVERIFIED | AJPS 67(1):185-204, DOI ajps.12657 (l.55-57); 18.5 vs 15.5 min (l.636-637); 17.1 vs 7.3 min (l.842-844); n=369,317 (l.608); 81% engaged (l.681) | Content correct. The note itself flags that its slug year (2020) and the OSF DOI are wrong, but the slug was never fixed. |
| 7 | ramey-2016-measuring-elite-personality | VERIFIED, Paul | ~11,000 words; CI about 0.3 above 5,000 words (l.266-267); r 0.99 and τB 0.72 (l.154); N 844, 598 and 3,648 (l.606, 974, 1355); about 53% (l.837). `data/members/lep_criterion.json`: rho 0.0026 / 0.0176 / 0.0003, n=440, confirmed. | Figures correct. Errors listed below. |
| 8 | notes/note-review-industry-growth-and-political-spending | research note, VERIFIED | $36,000 Cox-Magar premium (l.84); $260.071m / $195.827m / $40.662m (l.198); about nine elections (l.12) | Figures correct. The author is wrong (below). |

### 2.1 huneeus-2018: specific errors

- **Paul's annotation:** the note says the record is empty. Paul wrote three prose annotations (see 1b).
- **Refuted claims are still in the text.**
  - The failure box says the "lobbying lead flips sign under year FE" was "not found". The closing paragraph still asserts it.
  - The failure box says the claim that "Paul's positions cite H&K under flip model, joint optimizer ..." is unverified. The Gaps section still asserts it as fact.
  - The failure box moves the exogeneity concession to printed p.25. Gaps still says p.26.
  - The failure box says the ratio is 4.5 to 7.3. The Key findings still say "about 4.5 to 7 times".
  - The failure box says "uses the full-sample estimate" is inference. The Summary still states it flatly.
- **A wrong correction in the failure box:** it removes "Paul calls the alexander item an outlier". Paul did write that, in the Trey bibliography.
- **Contradictory status:** the summary ends "All transfers are UNVERIFIED (DE-062)", while the frontmatter says `evidence_status: verified` and every row is VERIFIED or VERIFIED-SOFTENED.

### 2.2 kim-2023-systemic-sequential-links: specific errors

- **The "fade" claim is attributed to the wrong source.** Gaps (l.143) and the closing paragraph say "Paul's media-blend note says Kim's effects fade within 2 to 3 years". Paul's annotation actually says favor lasts "only **6 months**" and then compounds if renewed. The 2-to-3-year disappearance is the **2020 working paper's** wording ("the effect disappearing only two or three years after the donation", `331171275468` l.158-159; Paul pink-highlighted it on kim-2020 p.5).
- **This version says the opposite.** The text read here says the link "gets larger two or three years after the donation is made" (`ee6caa08758d` l.137-139). Neither version supports Paul's "6 months".
- **The summary keeps three claims the failure box corrected:**
  - "lobbying does not predict later donations" (Table K.1 has sponsor t+0 = 0.0074, SE 0.0028);
  - "CEO and employee donations show much weaker links" (Table J.3 has a CEO co-sponsorship value of 0.28 at t+2);
  - "It supports the engine's choice to measure money from direct PACs only" (the failure box says the paper discusses neither the PAC-only scope nor the flip weights).
- **Metadata:** `venue: ""` and `year: "2023"`, although the DOI is 10.1086/734531, which Paul cites as JOP 88(1), 2026.

### 2.3 ramey-2016: specific errors

- **The relation label overclaims.** The bearing on "Big Five validation against Senate gold panels" is labelled **supports**. Its own text says the paper "has no direct validation against ground-truth personality measures", that the analyst's panel description "could not be verified", and that the NPAT argument "does not directly apply to the panels". It should be qualifies or context.
- **Paul's ML prose is not attached.** That prose contains a factual slip the note should flag: Paul says the data come from "auditory and textual transcripts of congressional floor speeches", but the paper uses Congressional Record text only. "Audio" appears nowhere in `5e248be1ab0d`.

### 2.4 Research note: specific errors

- **Wrong author.** The frontmatter says `author: Paul` and the header says "Paul Goff (2026)". The source reads "Review of Astra_Review_Brief_Sector_Growth_and_Political_Spending.md ... Recommendations below are my methodological judgments" and names no author. `excluded.md` says "Author not stated", and `systems/mentat-sector-growth-study` calls it "the Codex Review". It is a review **of** Paul's brief, not Paul's note.
- **Design advice rated as strong evidence.** Three of its bearings are rated **strong**. Across the system notes, 21 evidence rows come from `notes/`, and 8 of them are rated strong.
- **Housekeeping:** `components` lists the same component five times, and the note's source also appears in `excluded.md`.

### 2.5 Other note-level errors found in passing

- **`anon-nd-strategic-identity-signaling`** says "A filename search ... found no PDF or other copy of the source". The PDF exists at `_sources/anon-nd-strategic-identity-signaling.pdf` and at `Downloads\Strategic identity signaling in heterogeneous networks.pdf` (11,519,264 bytes). It fails to extract because of damaged flate streams ("Bad block header" in `pdftotext`), so it needs repair or OCR.
- **Duplicate works:** `golovin-2011-adaptive-submodularity-approach` (year field 2010) and `-theory` are the same work (arXiv 1003.3967), counted as two papers.
- **Slug and year errors:**
  - `jennifer-nd` is Ringe, Victor & Gross 2013, BJPS.
  - `anon-2019-partisan-ties-lobbying` is Furnas, Heaney & LaPira 2019.
  - `h-2023` is Taherdoost & Madanchian.
  - `kalla-2020` should be 2023.
  - `kamarthi-2019` should be 2020 (AAMAS).
  - `alexander-nd` should be 2009, `chen-nd` 2023 and `kim-nd-mapping` 2020.
- **Non-articles counted as papers (4 of 132):** the two link-list documents (`anon-nd-untitled-document-*`), the Frisch & Kelly `.doc` codebook, and the Cook "House At-A-Glance" sheet.

---

## 3. Hubs and themes

**Do assessments follow from the tables?** Mostly yes. The bottom lines ("direction supported, size not"; "nothing validates any SNA constant"; "no external paper studies the estimand") match tables that are dominated by weak and moderate "qualifies" rows. Weak rows are 285 of 588, moderate 245 and strong 58. All 56 rows that cite UNVERIFIED papers are labelled UNVERIFIED and rated weak.

Defects:

- **Stated counts do not match the tables.**
  - melange-influence-engine says "37 VERIFIED, 58 VERIFIED-SOFTENED and 13 UNVERIFIED". The table has 34 / 63 / 11 (n=108).
  - melange-sna says 9 / 39 / 3. The table has 7 / 41 / 3 (n=51).
  - mentat-sector-growth-study says "Fourteen of the 29 rows" are internal documents. 15 rows cite `notes/`.
- **Stale status text:** 51 of the 107 VERIFIED papers still say that their bearings "stay UNVERIFIED until a second path reproduces" them.
- **The framework has drifted from the system assessments.**
  - Section 3 says `cpf_pressure 1.7/0.5 rests on Furnas/Bakker` and that "the pt and incent multipliers [rest] on Linde & Vis and Fouirnaies & Hall". The influence-engine assessment shows the engine **reverses** Furnas's asymmetry (+0.7 gain vs -0.5 penalty), that Linde & Vis found **no** loss aversion (LOSSFRAME 1.10), and that F&H do not test competitiveness. `framework.md` mentions LOSSFRAME or loss aversion 0 times.
  - Section 6.8 says Fink's temporal gap "is answered by media-blend half-lives ... and Sotoudeh's 6-month decay". `systems/melange-stance` l.32 says those half-lives "have no empirical basis", and that Sotoudeh's figure concerns cosponsorship capital.
  - Section 2 says Kim, Stuckatz & Wolters "find that donations **raise** a targeted member's involvement ... by 8-11 pp". That is a causal verb with no window. The paper says its estimates "should not be interpreted as causal" (pp.3, 29). The window is manufacturing firms, 2008-2018, and the outcome is firm lobbying on member-linked bills.
  - The section 2 one-line summary ("money buys access that decays") depends on the 2020 draft. The published version shows the link growing through t+3.
  - Section 7 correctly quotes the 2020 draft, but section 3 (media blend) and open question 7 present "fade within 2 to 3 years" without naming the draft.
- **Figures without a window or denominator in the framework:** "Contact inference reaches 53.6% top-1"; "10% more lobbying brings 3% more revenue" (Compustat public firms, LDA 1999-2015; full-sample column; the preferred column is 0.2% per 1%); "8-11 pp".
- **Other framework errors:**
  - Section 1 says "Only six of them carry Paul's own prose", then lists seven.
  - Open question 19 says the Ringe, Victor & Gross BJPS PDF has "no paper slug". It is `jennifer-nd` (VERIFIED).
  - `missing.md` still lists `BJPS-Friends-Enemies.pdf` under "need OCR".
  - `missing.md` has a heading "All 10 flagged in prep_log (pdf under 500 chars):" with no list under it.
  - `framework.md` repeats its H1 title twice.
- **Wording rules:**
  - The scan found no barred wording left unguarded. Access, introduction and closeness probability: 102 sentences, all negated or framed as the rule. Influence on revolving-door edges: 33, all guarded. Reciprocity is never claimed as validated. The rejected SNA proposals appear only as flags. LDA appears as an edge or weight only in warnings.
  - One borderline case: the framework's "Fink 2023 gives empirical influence probabilities" should quote the dataset's own term ("probability of influence", built from Twitter interactions) so it is not read as a validated influence estimate.
- **Themes:** they handle the hard cases correctly.
  - `themes/lobbying.md` reconciles the two Kim versions and flags Paul's "6 months" as not found.
  - `themes/adaptive-influence-maximization.md` says RL4CCIM shows single-campaign non-submodularity only, not the cross-campaign claim.
  - Every paper is linked from its theme note, and every paper-to-system claim appears in that system note.

---

## 4. What Paul asked for that is missing

1. **Annotations read first, at the paper level.** Paul's prose did not reach 13+ paper notes, and it reached 2 notes with foreign content (see 1b). This needs a re-join of the annotation entries to slugs by DOI or title, not by surname plus year. The join probably went wrong because of the many Kim papers.
2. **Fink et al. 2023 paper note** (see 1a).
3. **Index:** `index.md` now exists. A re-run of `vault_check.py` gives unindexed papers 0, broken links 0, bad headings 0, but **4 new ambiguous links** in `index.md`: `[[note-request-for-review-industry-economic-performance]]`, `[[note-industry-growth-and-political-spending-research]]`, `[[note-review-industry-growth-and-political-spending]]` and `[[note-sector-growth-and-political-spending-source]]`. Each matches both `notes/` and `_sources/`.
4. **Not on disk:** 32 bibliography works were never retrieved. They include the Paul-prose items Oropeza 2024, Rongkuan 2024, Peng 2022, the addendum's RL and CELF set, and Borg. `missing.md` lists them but gives no retrieval plan.
5. **Unreadable PDFs:** the Strategic identity signaling PDF is damaged and needs repair or OCR. `missing.md` still lists the BJPS PDF as unreadable, though it has been read.
6. **The 21 zero-evidence components** (see 1c). Terrier is the weakest system: 3 of 4 imputation components and 3 further canvass and row-scoring components have no literature at all.
7. **Housekeeping:** remove the 4 notes that are also in `excluded.md`; merge the duplicate Golovin entries; fix the slug and year errors; give the 11 prose-GAP components a "(none: GAP)" table so that counts can be computed from tables.
