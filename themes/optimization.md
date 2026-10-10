---
type: theme
name: "Optimization"
paper_count: 1
tags: [theme]
---

# Optimization

**The Optimization theme has no literature to synthesize yet.** The theme file holds one work, [[anon-nd-untitled-document-single]]. It is a bibliography stub: a Word document in Paul's Optimization folder whose only content is one URL (https://journals.sagepub.com/doi/pdf/10.3233/FAIA397#page=583).

- It has no title, authors, year, article text or annotations (n_annots 0). No reviewer sidecar exists for sha 7c84a3b830f0.
- The DOI prefix 10.3233 belongs to IOS Press, and FAIA is its *Frontiers in Artificial Intelligence and Applications* series. The DOI has no article-level suffix, so it most likely points to a whole proceedings volume (397), with the page-583 anchor marking one paper inside it. None of this has been verified, and the URL was not fetched.
- Paul's notes for this category contain no prose, takeaways, research questions or system links. Nothing here is attributed to him beyond the fact that he filed this link under "Optimization".

Because there is no content, this synthesis cannot say where works agree or disagree, which methods dominate, or what the literature means for Paul's systems. Any such claim would be invented. The rest of this page records what is known, what is guessed (labelled as a guess), and what Paul needs to do to make the theme usable.

## Strands

### Unresolved citation stub (one paper inside an IOS Press FAIA proceedings volume)

The only member of this strand is [[anon-nd-untitled-document-single]]. Its content is a URL to page 583 of FAIA volume 397. Its school, research question, method, data and findings are all **unknown**. The extraction recorded no question, no method, no findings and no bearings, and that is correct.

The folder name and the FAIA series (an AI conference proceedings series) suggest the paper is probably about combinatorial optimization or influence maximization. **That is a guess based on where the file sits, not on what the paper says.** It must not be used to place the paper in any school of thought until the PDF is read.

The extraction also records a sibling stub, [[anon-nd-untitled-document-link]]. It is a link list rather than a single link, and it has the same problem: no article text. It is not a member of this theme's work list.

Works: [[anon-nd-untitled-document-single]]

## Agreements and debates

**There are no agreements or disagreements to report.** The theme has one work, and that work has no extracted text, so nothing can be compared.

Once the paper is in hand, these are the questions worth comparing it against (they are questions, not findings):

- **Allocator design.** Does it argue that a greedy method with a lazy-evaluation speed-up (the CELF approach, Cost-Effective Lazy Forward selection) is good enough? Or does it argue for learned policies, such as GCOMB-style learned heuristics for combinatorial problems on graphs?
- **Edge weights.** If it is an influence-maximization paper that changes how network edges are weighted, check it against the two proposals this project measured and REJECTED: dropping regular cosponsorship edges, and per-edge volume normalization. **If the paper argues for either one, FLAG it. Do not recommend it.**

## Methods

**No methods can be described.** The work record's `method` field is empty, and the source has no article text.

Paul's notes describe the allocation layer as "adaptive greedy plus CELF today, a learned policy (GCOMB) to come". When a paper on this topic is read, check these known weaknesses:

- **Submodularity.** Greedy and CELF guarantees, including the (1−1/e) bound, hold only if the objective is monotone submodular. Check whether the paper proves this for its objective or simply assumes it.
- **Learned policies.** Results for GCOMB-style learned policies depend on the training distribution of graphs. Check whether they are tested on graphs that differ from the training set.
- **Simulated outcomes.** Influence-spread objectives are usually estimated by Monte Carlo simulation. Check whether the paper reports enough samples, or a convergence check, for its results to hold.

These are review criteria to apply later, not properties of the cited work.

## How the works relate

- [[anon-nd-untitled-document-single]] — complements — [[anon-nd-untitled-document-link]]: This relation was recorded during extraction. Both are 'Untitled document' stubs in Paul's bibliography: one holds a single link and the other a link list. Neither has article text, and both need the same fix: fetch the linked PDFs and re-extract. The relation does not claim any overlap in content, because neither work's content is known.

## What it means for the systems

**No bearing is recorded for any component.** The work's `bearings` list is empty, and adding one would be made up. That includes the joint optimizer (CELF lazy greedy, fec/engine/optimize.py), the budget frontier (fec/engine/budget_frontier.py) and the per-topic allocators. The system most likely to be affected, if the paper proves relevant, is [[systems/melange-allocation]]; the network-weighting caveats would touch [[systems/melange-sna]] and [[systems/melange-influence-engine]]. These links are pointers to where to look, not findings.

The paper sits in a dedicated Optimization folder, so it **may** turn out to matter for the allocation layer. That is still unverified: a lead to follow up, not a finding (DE-062).

When the paper is read and a bearing is drafted, these project rules apply:

- Graph ranks and allocator scores are heuristic model outputs. They are never closeness, access or introduction probabilities (D-025).
- LDA is an identity source, not a source of edges or weights (D-021).
- Reciprocity is not validated (D-104..D-108).
- Any figure quoted must name its window and denominator.
- A coefficient pooled across a known fault line is quoted only with its strata, or labelled UNSTRATIFIED.
- The two rejected SNA proposals (dropping regular cosponsorship edges, per-edge volume normalization) are flagged if the paper argues for them, never recommended.

## Gaps

**The whole theme is a gap.** Specifically:

1. **No identified paper.** Paul needs to open page 583 of FAIA volume 397 and note its title, authors and year. Then save the PDF into C:\Users\pgoff\Documents\research\_raw and rerun extraction and this synthesis step.
2. **No annotation prose.** The annotation file (sha 7c84a3b830f0) contains only frontmatter and the URL. It has no takeaways, system links or cross-references, and there is no _annots sidecar.
3. **No coverage of the methods the system actually uses.** The theme contains no work on:
   - lazy greedy and CELF selection, or submodular maximization guarantees
   - budgeted or knapsack-constrained allocation (relevant to the budget frontier)
   - learned combinatorial policies such as GCOMB (the planned replacement)
   - robustness of allocations when the input scores are noisy model outputs

   These topics are named by the system's design, not by Paul's notes. If Paul intends the folder to support the allocation layer, they are the obvious reading targets.
4. **The sibling stub [[anon-nd-untitled-document-link]] is unresolved too.** Its link list should be fetched in the same pass, in case some of its links belong in this theme.

## All works

- [[anon-nd-untitled-document-single]] (n.d.) — Untitled document (single link)
