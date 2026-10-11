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
Link into the brain vault with `obsidian://open?vault=brain&file=<path>` URIs, not wikilinks. The one exception is the linked vault's own notes (see "Pipeline lessons and the linked vault" below).

## Layout update 2026-10-10

- Papers are filed by theme: `papers/<theme-slug>/<slug>.md`; non-articles in `papers/_non-articles/`. Links stay by basename (`[[slug]]`), so moves do not break them.
- Every note carries a `type/*` tag (paper, research-note, system-hub, theme, annotation, index, non-article); papers also carry `theme/*` and `evidence/*` tags. Graph colour groups and the `vault-colors` CSS snippet key off these tags and folders.
- The combined view lives at `C:\Users\pgoff\Documents\linked` (folder junctions to this vault and the brain vault, plus `bridges/` notes).

## Pipeline lessons and the linked vault (added 2026-10-11)

Source: memory `research-vault-lane-2026-10-09.md` (the 2026-10-09 build); brain note [projects/research-vault](obsidian://open?vault=brain&file=projects%2Fresearch-vault), sections "Build record and lessons" and "Linked vault". The linked-vault file facts below were also read from disk on 2026-10-11.

Pipeline lessons:
- Workflow resume cache keys are CHAINED by call order: editing any early prompt, a failed early agent, or concurrency reordering makes everything after it re-run live (about 1 h lost in the build). For multi-hour pipelines, persist each stage's output to disk and drive later workflows from disk state, not from resume.
- Concurrency cap per workflow = CPUs - 2 = 6 on the 8-core box; run several workflows in parallel (shards) for throughput. CPU then pins at 100%, largely ECC hook node processes (about 30) per tool call.
- Haiku StructuredOutput rejects big extractions as invalid JSON (backslashes, quotes, over 8 KB); have it write the file via a script with `json.dump` and reload it instead.

Linked vault (`C:\Users\pgoff\Documents\linked`, Obsidian vault "linked"):
- Folder junctions `brain\` (to `Documents\brain`) and `research\` (to this vault), plus `bridges/<system>.md` (one per research system hub, 20) and `Home.md`. Nothing is copied: a note edited there is the same file here.
- Built by `build_linked.py`, which reads the catalog in `_work/state/phase1.json` (system and component `brain_notes`). The script lives only in a temp scratchpad, `C:\Users\pgoff\AppData\Local\Temp\claude\C--Users-pgoff-OneDrive-Desktop-Melange-Systems\693e1754-6a96-45a9-b931-f03762acbd89\scratchpad\build_linked.py`; it is in neither vault and can disappear with the temp folder.
- Ignore filters in `linked\.obsidian\app.json`: `research/_raw/`, `research/_text/`, `research/_annots/`, `research/_work/`, `research/_sources/`, `research/annotations/_versions/`.
- `%APPDATA%\obsidian\obsidian.json` must be written WITHOUT a BOM (PowerShell 5.1 `-Encoding utf8` adds one, and Obsidian then reports "vault not found" or an error); Obsidian reads that file only at startup.
- Regeneration: bridges and `Home.md` list only the system hubs and the brain notes named in `phase1.json`. New sections or rows inside an existing hub need no rebuild; a new hub or new catalog brain notes do. A re-run rewrites every bridge, `Home.md`, `app.json` and `graph.json`, and the live files carry edits the script does not make (bridge tag `type/bridge`, `Home.md` frontmatter, extra `app.json` keys), so update the script before re-running it.
- Brain links, clarified: notes in this vault point at brain notes only through `obsidian://open?vault=brain&file=<path>` URIs. The linked vault's own notes (`bridges/`, `Home.md`) are the exception: they use folder-path wikilinks such as `[[brain/projects/mentat]]` and `[[research/systems/<slug>]]`, which resolve only inside the linked vault. Never copy those wikilinks into this vault.
