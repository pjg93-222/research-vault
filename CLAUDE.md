# Research vault contract

## Purpose
Paul's research vault: the empirical backbone of his systems (Melange path-finding / SNA / allocation / stance / psychometrics, Moneyball rank pillars, Mentat FEC attribution / giving forecast / sector-growth study, Terrier). Start at [[index]]; the synthesis is [[framework]]. This vault is NOT under git; nothing here commits or pushes.

## Folder map
- `papers/` one note per paper (slug = file name).
- `notes/` Paul's research notes (e.g. the sector-growth study).
- `systems/` one note per system component; `themes/` one note per literature theme.
- `annotations/` Paul's words, verbatim. NEVER edit (`_versions/` holds dated copies).
- `_sources/` original files; `_text/` extracted text; `_annots/` extracted PDF markup.
- `_raw/` unzipped inputs; `_work/` manifests and pipeline state (`_work/state/meta.json` is the registry of papers, systems, themes, excluded).
- `framework.md`, `missing.md` (cited or scanned but unavailable), `excluded.md` (rescue list).

## Evidence status (per paper)
- VERIFIED: reproduced by an independent adversarial pass.
- VERIFIED-SOFTENED: held only with corrected wording.
- UNVERIFIED: single agent read; a lead, not a finding (DE-062).

## Adding a paper
Put the original in `_sources/`, extract text to `_text/`, write `papers/<author-year-keywords>.md` (year, theme, systems, whether Paul annotated, evidence status), link it from its theme and system notes, and add it to `index.md` and `_work/state/meta.json`. Paul's annotations go in `annotations/` verbatim.

## Wording rules
- Graph ranks and shared-background scores are heuristic model outputs, never closeness, access or introduction probabilities (D-025).
- Influence language is barred on revolving-door edges.
- Every figure names its window and denominator.
- LDA is an identity source, not an edge or weight source (D-021).
- Reciprocity is NOT validated.
- UNVERIFIED bearings are single-agent leads, not findings (DE-062).
- Rejected SNA proposals (dropping regular cosponsorship edges, per-edge volume normalization) are flagged, never recommended.

## Brain links
Link into the brain vault with `obsidian://open?vault=brain&file=<path>` URIs, not wikilinks.

## Layout update 2026-10-10

- Papers are filed by theme: `papers/<theme-slug>/<slug>.md`; non-articles in `papers/_non-articles/`. Links stay by basename (`[[slug]]`), so moves do not break them.
- Every note carries a `type/*` tag (paper, research-note, system-hub, theme, annotation, index, non-article); papers also carry `theme/*` and `evidence/*` tags. Graph colour groups and the `vault-colors` CSS snippet key off these tags and folders.
- The combined view lives at `C:\Users\pgoff\Documents\linked` (folder junctions to this vault and the brain vault, plus `bridges/` notes).
