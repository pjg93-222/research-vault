# Vault link check
Script: scratchpad\vault_check.py (stdlib + yaml if present); 198 .md scanned (outside _raw/_text/_annots/_work).

| check | before | after |
|---|---|---|
| broken wikilink targets | 0 | 0 |
| bad #heading | 20 | 0 |
| ambiguous basename links | 110 | 0 |
| YAML frontmatter parse failures | 0 | 0 |
| paper/notes missing _sources file | 0 | 0 |
| papers not linked from index.md | index.md absent | index.md absent (re-run when written) |

Fixes (mechanical, link syntax only):
- 20 links of form [[t#H/H]] (alias separator mangled to "/") -> [[t#H|H]]; H verified as a heading in target (azaiez-2025, bonica-2025, linde-2016, noble-2024 papers).
- 110 links [[note-...]] matched both notes/note-*.md and a copy in _sources/ -> [[notes/note-...]] (papers/*, systems/mentat-giving-forecast, mentat-sector-growth-study, melange-lobbying).

Remaining: only the unindexed-papers check, pending index.md (132 papers).
