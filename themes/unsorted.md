---
type: theme
name: "Unsorted"
paper_count: 2
tags: [theme, type/theme]
---
# Unsorted

**Theme: Unsorted. It holds 2 works, and neither has readable content. There is no literature to synthesise yet.** Paul has left no notes for this theme. Both records are vault artifacts that still need resolving:

- [[anon-nd-untitled-document-link]] is a 448-character file of four bare URLs: a redalyc PDF, two EBSCO openurl records and one ScienceDirect pii. It has no title, authors, text or annotations, and none of the links has been fetched.
- [[vanderdoes-2022-strategic-identity-signaling]] has a title, but its extracted text is 18 bytes of blank lines and no source PDF was found on disk. The annotation file shows 47 empty Ink highlights on pages 1 and 2 (42 yellow, 5 green, dated 2026-06-01). They record where Paul marked the page, not what he marked.

Neither work has a question, method, findings or bearings. Everything below says what is missing, not what the works say. No claim in this theme should be cited as evidence for or against any Melange or Mentat component.

## Strands

### Unresolved link-only artifacts
[[anon-nd-untitled-document-link]] is a list of pointers, not a work. It contains four URLs:

- redalyc journal 2971, article 297176235002
- EBSCO gcd:162692885
- EBSCO gcd:183882663
- ScienceDirect pii S0306457326000968

The only inference comes from the identifier. A pii starting with S0306-4573 matches the ISSN of *Information Processing & Management*, a venue that often publishes stance, sentiment and network-text work. That would put it near melange-stance or melange-sna. This is a guess from an ID, not a finding, and it supports no bearing.

The record lists a 'complements' link to `anon-nd-untitled-document-single`, a second link-only file. That slug is not in this theme file, so it is not used as a relation here. It belongs in the same triage batch.

Each of the four links should become its own vault entry once fetched. Downloading needs Paul's go.

Works: [[anon-nd-untitled-document-link]]

### Annotated paper with lost text (identity signaling)
Only the metadata title survives for [[vanderdoes-2022-strategic-identity-signaling]]: 'Strategic identity signaling in heterogeneous networks'. Authors, year, venue and DOI are unknown, and nothing on disk confirms the title.

The annotation pattern shows engagement. Paul made 47 highlights on the first two pages and used green sparingly on page 2, which suggests he found the abstract or framing important. The highlight text was not captured.

Three components to re-read against once the text is recovered (leads only, not bearings): the cascade kernel's partisan cue factors (`cpf_pressure`, the cross-party `sqrt(bpf)` term), the persona channel, and the ACU speaker mode. No agreement, disagreement or method can be stated.

Works: [[vanderdoes-2022-strategic-identity-signaling]]

## Agreements and debates

**There are no debates to report.** Neither work has stated findings, so there is nothing that agrees or conflicts.

Both records were handled with the same caution: no bearings were made because the content is missing. That shared restraint is a feature of the processing, not a shared scholarly position.

If the identity-signaling paper turns out to argue that partisan or identity signals drive network ties or cue-taking, check its claims against the project's rulings before they bear on anything:
- Reciprocity is not validated (D-104..D-108).
- Graph ranks are heuristic outputs, not closeness or access probabilities (D-025).

If either paper argues for dropping regular cosponsorship edges or for per-edge volume normalisation, flag it. Both proposals were measured and rejected in this project.

## Methods

**No research methods can be described.** Neither record has a method field or readable text.

The only "methods" here are the vault pipeline's own, and both failed:
- **Link resolution was not done.** [[anon-nd-untitled-document-link]] holds URLs that were never fetched.
- **Text extraction failed.** For [[vanderdoes-2022-strategic-identity-signaling]] it produced an empty file, and no source PDF can be found, so re-extraction with OCR cannot run yet.
- **Highlight capture failed.** The Ink annotations kept position and colour but not the text they cover.

The underlying weakness is that ingestion only looks at metadata. It accepts a record with a title or a URL list as if it were literature, and nothing flags empty bodies. Those records end up in 'Unsorted' rather than being marked unresolved.

## How the works relate

- [[anon-nd-untitled-document-link]] — same-triage-status — [[vanderdoes-2022-strategic-identity-signaling]]: Neither has readable content, findings or bearings, and both need a source recovered (links fetched, or the PDF found and re-extracted) before they can be related to any component or to each other. This is a shared state of the files, not a relation between their ideas.

## What it means for the systems

**No bearings on Melange or Mentat.** Neither work supplies a claim with a page cite. Any bearing would be invented evidence, which DE-062 rules out.

Pointers for the re-read only:
- **[[vanderdoes-2022-strategic-identity-signaling]], if its title is accurate:** the cascade kernel's partisan cue factors (`cpf_pressure`, the cross-party `sqrt(bpf)` term), the persona channel, and ACU speaker mode, all under [[systems/melange-influence-engine]]. Melange has two cpf physics that must never be merged, so check which one any recovered claim touches.
- **[[anon-nd-untitled-document-link]]:** the ScienceDirect item may touch [[systems/melange-stance]] or [[systems/melange-sna]]. This is inferred only from the journal's ISSN prefix.

When either is resolved, these wording rules apply:
- Graph ranks or shared-background scores are heuristic model outputs. Never call them closeness, access or introduction probability (D-025).
- LDA is an identity source, not an edge or weight source (D-021).
- No influence language on revolving-door edges.
- Every figure names its window and denominator.
- Quote a pooled coefficient over a known fault line only with strata, or label it UNSTRATIFIED.

## Gaps

Everything is missing. What to do, in order:

1. **Fetch the four links in [[anon-nd-untitled-document-link]].** This needs Paul's go because it involves downloads. Ingest each as its own work with real metadata and text, then delete or relabel the link-list record as "unresolved links".
2. **Triage `anon-nd-untitled-document-single` in the same batch.** Its record cites it as a sibling, but it is not in this theme file.
3. **Find the source PDF for [[vanderdoes-2022-strategic-identity-signaling]].** Search by title outside `_raw`, `annotations` and `_work`: Zotero storage, Downloads or the browser PDF cache. Then re-extract the text (OCR if a scan) and map the 47 highlight rectangles, especially the 5 green ones on page 2, to the text they cover.
4. **Add an empty-body guard to ingestion.** Records whose text file is under a threshold, or that hold only URLs, should go to an 'Unresolved' queue rather than 'Unsorted'.
5. **Re-run the relation and theme passes once content exists.** Then move each work into its proper theme (likely SNA or partisan-cue / stance). 'Unsorted' should end up empty.

**Open question for Paul:** do you remember what the identity-signaling paper is (authors or venue)? That would shortcut step 3.

## All works

- [[anon-nd-untitled-document-link]] () — Untitled document (link list)
- [[vanderdoes-2022-strategic-identity-signaling]] () — Strategic identity signaling in heterogeneous networks
