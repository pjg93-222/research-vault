---
type: paper
title: "Introduction To Monte Carlo Simulation"
authors: ["Robert L. Harrison"]
year: "2010"
venue: "AIP Conference Proceedings 1204:17-21 (published 2010 Jan 5; PMC author manuscript, available 2011). Metadata hint confirmed."
doi: "10.1063/1.3295638"
category: "Causal & Methodological"
systems: ["melange-influence-engine", "melange-validation", "terrier-row-scoring"]
components: ["melange-influence-engine.competitive-cascade-crn", "terrier-row-scoring.uncertainty-draws", "melange-validation.sensitivity-and-mc", "melange-influence-engine.scenario-runner"]
paul_annotated: true
paul_markup_count: 31
evidence_status: "verified"
source: "[[_sources/harrison-2010-introduction-monte-carlo.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\nihms219206.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\nihms219206.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\nihms219206.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\nihms219206.pdf"]
sha12: ["f271853a3a6e"]
tags: [paper, causal-methodological, type/paper, theme/causal-methodological, evidence/verified]
---

# Introduction To Monte Carlo Simulation

**Robert L. Harrison (2010).** *AIP Conference Proceedings 1204:17-21 (published 2010 Jan 5; PMC author manuscript, available 2011). Metadata hint confirmed..* [10.1063/1.3295638](https://doi.org/10.1063/1.3295638). Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> Harrison (2010) is a short tutorial and history of Monte Carlo simulation. It has no data and runs no simulation. The history runs from Buffon's needle (an intersection probability of 2/pi gives an estimate of pi) through De Forest, Galton and Gosset to von Neumann and Ulam's neutron-shielding work. It then gives a generic recipe: define the model as probability density functions, sample from them repeatedly, and tally the statistics of interest. The paper covers pseudo-random generators (linear congruential generators are poor; it recommends Mersenne Twister), the inversion method, acceptance-rejection, biased table approximation, and normal and Poisson sampling. For the last two it advises using library routines. Its warnings matter most for Paul's systems: simulation gives no exact answers, it is only as good as its model and inputs, it is prone to bugs, and analytic alternatives should be considered first.
> 
> The paper is a textbook source, not evidence. It cannot validate any Melange or Terrier parameter. The Melange cascade engines follow the recipe directly: a Bernoulli edge draw u < p on the build_P kernel, a tally of A-coloured activations, and the mean over MC iterations. Terrier's uncertainty draws use inversion for categorical cells, which is the analyst's extension of the paper's inversion method (the paper shows it only for distributions with an invertible CDF, with an exponential example). The system goes beyond the paper in three ways: it keys draws to individual edges to get common random numbers, it uses splitmix64 and PCG64 generators, and it checks MC stability across seeds. The paper covers none of these. Its main use here is as a caution. The kernel saturation constant 6 and the bystander scale of $400k are unestimated, so the paper's point that simulation is only as good as its inputs applies directly. One gap shows up in the code: the scenario, frontier and coalition spreads are reported as point means (tot/MC) with no Monte Carlo standard error. That finding rests on a search of four files and is UNVERIFIED system-wide.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Paul's markup in the PDF

- p1 Highlight: "Paraphrase: Monte Carlo defined as random sampling plus statistical modelling to estimate functions and mimic complex systems." — 
- p1 Highlight: "Paraphrase: The skeptical opening question about whether random events can yield concrete results." — 
- p1 Highlight: "Paraphrase: The coin-flip analogy: many repeated flips used to probe nuclear reactors, weather and recessions." — 
- p1 Highlight: "Paraphrase: Using randomness in a deterministic manner was revolutionary, traced back to the 18th century." — 
- p1 Highlight: "Paraphrase: Buffon's result that a needle-line intersection has probability 2/pi." — 
- p1 Highlight: "Paraphrase: 19th and early 20th century simulation used as an experimental check on theory and data." — 
- p1 Highlight: "Paraphrase: Galton's modified-dice method for generating a normal distribution." — 
- p2 Highlight: "Paraphrase: Gosset ('Student') used simulation to develop and test the t-test." — 
- p2 Highlight: "Paraphrase: Von Neumann and Ulam first suggested the method for neutron shielding problems and named it after the casino." — 
- p2 Highlight: "Paraphrase: The statement that no single Monte Carlo method exists." — 
- p2 Highlight: "Paraphrase: Step 2 of the generic pattern: repeatedly sample from the PDFs." — 
- p2 Highlight: "Paraphrase: Step 3 of the generic pattern: tally or compute the statistics of interest." — 
- p2 Highlight: "Paraphrase: The lead-in sentence that many simulations follow a pattern." — 
- p2 Highlight: "Paraphrase: Buffon's needle model, specifically the intersection probability." — 
- p2 Highlight: "Paraphrase: The lead-in to the checklist of key points for defining a model (green highlight, the only one in this colour)." — 
- p3 Highlight: "Paraphrase: The desired outputs are the key starting point of model definition." — 
- p3 Highlight: "Paraphrase: Required accuracy depends on use, e.g. picnic planning versus a farmer's rainfall totals." — 
- p3 Highlight: "Paraphrase: Weather models use fluid dynamics and thermodynamics with measured inputs, not subatomic detail." — 
- p3 Highlight: "Paraphrase: Weather forecasting became practical as computers and input measurements improved." — 
- p3 Highlight: "Paraphrase: Input measurement density limits how accurate predictions can be." — 
- p3 Highlight: "Paraphrase: Deciding how to process inputs into outputs is the final modelling step." — 
- p3 Highlight: "Paraphrase: A Monte Carlo simulation always contains randomness, often at many points." — 
- p3 Highlight: "Paraphrase: PDFs give the range of possibilities and their relative probabilities at each step." — 
- p3 Highlight: "Paraphrase: A PDF must be non-negative and integrate to one." — 
- p4 Highlight: "Paraphrase: Truly random sources exist, e.g. timing of radioactive decays, but computers use pseudo-random generators." — 
- p6 Highlight: "Paraphrase: The normal distribution's parameters are the mean and standard deviation." — 
- p6 Highlight: "Paraphrase: Advice to use a library function for normal and Poisson sampling." — 
- p6 Highlight: "Paraphrase: Monte Carlo runs repeat the same processes, producing a series of recorded events." — 
- p6 Highlight: "Paraphrase: Summary: Monte Carlo is a key tool for analytically intractable problems." — 
- p6 Highlight: "Paraphrase: Summary: the modern form arose in the WWII push to develop nuclear weapons." — 
- p6 Highlight: "Paraphrase: A single event samples many PDFs, some many times, and is tallied in many ways." — 

## Study

**Question:** What Monte Carlo simulation is, where it came from, and how to build one: defining a model as probability density functions, generating random numbers, sampling from standard distributions (uniform, exponential, normal, Poisson), and tallying results. Stated scope: an overview with emphasis on techniques used in medical imaging simulation.

**Theory:** No substantive theory beyond the generic Monte Carlo recipe the paper gives: model a system as probability density functions, sample repeatedly from them, tally statistics of interest. The paper states there is no single Monte Carlo method. It also cites the central limit theorem to justify normal approximation.

**Data:** No empirical dataset. Illustrative examples only: Buffon's needle (p.1, p.8 Fig.1); a qualitative weather-forecast example (p.3); exponential, normal and Poisson sampling. Sample, unit, period and N: not applicable.

**Method:** Narrative review and tutorial. No estimation, no statistical test, no simulation run. Covers: the three-step pattern (p.2); PDF/CDF requirements (p.3); pseudo-RNGs, with LCG (eq. 3) and Mersenne Twister (p.4); inversion method, exemplified by exponential sampling (eqs. 4-6, p.5); acceptance-rejection with steps 1-5 (p.5); piecewise-linear table approximation (p.5); normal sampling via CLT and library routines (pp.5-6); Poisson sampling by counting exponential inter-arrival draws (eq. 8, p.6); tallying recorded events (p.6).

**Identification:** Not applicable. Metadata hint (title, author, 2010, AIP Conf Proc 1204:17-21, DOI 10.1063/1.3295638) matches the paper's own title block and PMC stamp.

**Scope:** Population: none (methods paper). Institution: University of Washington Radiology (single author). Era: 18th-century precursors (Buffon, 1707-1788) through the 19th-century statistical examples, the WWII Manhattan Project, and modern simulation. Field emphasis: medical imaging, though the methods are general.

## Key findings

- Buffon's needle: for a needle the same length as the line spacing, the probability of intersecting a line is 2/pi. Repeated tosses estimate pi as 2N/X, where X is the number of intersections. (P = 2/pi; pi estimate = 2N/X; p.1 (History); Fig.1 caption p.8)
- Systematic Monte Carlo was developed during the Manhattan Project by von Neumann and Ulam, to study neutron travel through radiation shielding; the method is named after the Monte Carlo Casino. (none; p.2 (History))
- Pre-modern simulation examples: De Forest (card deck, mortality tables, 1834-1888); G.H. Darwin (spinner, curve smoothing, 1845-1912); Galton (modified dice for a normal distribution, 1822-1911); Gosset/'Student' (labelled cards, t-test, 1876-1937). Paper calls them seminal but distinct from modern simulation, which solves deterministic problems probabilistically. (dates only; p.1 and p.2 (History))
- Simulation drawbacks: it can need huge computing resources, gives no exact solutions, is only as good as the model and inputs, and is prone to software bugs. The author says analytic and experimental alternatives should be considered first. (none; p.2 (end of History section))
- Weather example: forecasts are more accurate in densely sampled regions (many stations, satellites, balloons) than near poorly sampled regions, so input measurement limits prediction. (none; p.3 (Defining the Model))
- A PDF must be non-negative and integrate to 1. The CDF gives the probability that a sampled value is at most x (eq. 2; equation body not extracted). (integral = 1; p.3 (Sampling from PDFs))
- Most RNGs sample integers 0..N and divide by N to get [0,1). Linear congruential generators are old and do poorly on Diehard-type tests. Mersenne Twister is 'rapidly becoming the generator of choice', passing most tests except those aimed at encryption RNGs. (u = sample/N; LCG eq. 3; p.4 (RNGs))
- Inversion method: draw u uniform on [0,1) and invert the CDF. Exponential sampling is the worked case (eqs. 4-6). The method works for any distribution with an invertible CDF. (eqs. 4-6 (bodies not extracted); p.4-5 (Inversion))
- Acceptance-rejection is an exact method (not an approximation). It needs two uniform draws per accepted sample. Accept v when c*u <= f(v)/g(v); on average c iterations are needed. (c iterations on average; 2 uniforms per sample; p.5 (Other methods))
- Piecewise-linear table approximation with interpolation can be among the fastest samplers, but it adds bias. Removing the bias with acceptance-rejection costs an extra uniform draw, which often cancels the speed gain. (none; p.5 (Other methods))
- Normal distribution: the CLT is given as the justification for approximating unknown distributions by normals, though it only holds in the limit. The CDF has no simple inverse, so the paper recommends fast acceptance-rejection library routines. (mu = mean, sigma = s.d.; p.5-6 (Normal and Poisson))
- Poisson sampling can count exponential inter-arrival draws until their sum exceeds the period; the count is the number of draws minus one. The author calls this slow and recommends a library routine. The Poisson mean and variance are both lambda. (lambda = mean = variance; eq. 8; p.6 (Poisson))
- Tallying: each event is recorded by its properties, and the process is repeated. A single event may sample many PDFs and be tallied in many ways. (none; p.6 (Tallying; Summary))

## Limitations

- No single definition of Monte Carlo; any definition omits valid cases (p.2).
- Simulation needs large computing resources, gives no exact solutions, depends on model and input quality, and software can have bugs (p.2).
- Simulation may be overused as a shortcut instead of analytic or experimental work (p.2).
- Pseudo-random generators only mimic randomness; LCGs do poorly on standard tests; Mersenne Twister fails some encryption-oriented tests (p.4).
- Piecewise-linear approximation introduces bias; removing it costs speed (p.5).
- Normal approximation via CLT holds only in the limit; the paper calls its use 'not necessarily unjustified' rather than proving it (p.5).
- Review only: no worked simulation output; the emission-tomography example is deferred to another paper in the volume (p.2, p.6).
- Equation bodies and figure images are absent from the text extraction, so eqs. 1-8 and the exponential/Poisson formulas are unverified here.
- No empirical validation, error analysis or comparison of samplers is reported.

## Short quotes

- "Analytic and experimental alternatives should be considered before committing to simulation." (p.2 (end of History section))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Competitive independent cascade with common random numbers|Competitive independent cascade with common random numbers]] | qualifies | Many simulations follow three steps: model the system as PDFs, sample from them repeatedly, and tally the statistics of interest. Drawbacks: simulation can need huge computing resources, gives no exact solutions, is only as good as the model and inputs, and its software is prone to bugs. Analytic and experimental alternatives should be considered before committing to simulation. (p.2, end of the 'History of Monte Carlo Simulation' section (drawbacks and analytic-first advice); p.2, 'The Monte Carlo Method' (three-step pattern); pp.2-3 'Defining The Model') | The cascade fits the three-step pattern. Each edge is a Bernoulli draw with p = x/(x+6), x = w * cpf_diffusion(u,v) * edge_mult(v), from build_P. An edge fires when the splitmix64 draw keyed by (iteration, edge) is below p. Each run counts A-coloured nodes (seeds included) and the spread is tot/MC over iterations (MC = 20 default in budget_frontier.py, 40 in scenario.py). Unestimated inputs: the saturation constant 6, the bystander rule min(0.9, $/400k), and the 0.4 + 0.6 sqrt(bpf_u bpf_v) cross-party factor. `fec/moneyball_core.py:543-562; fec/engine/scenario.py:27,44-61; fec/engine/budget_frontier.py:6-7,85-94` | The recipe and caveats are generic, so they carry over fully. They say nothing about whether the cascade parameters are right, which IC model to use, or whether MC = 20 is enough. The paper does not cover common random numbers or variance reduction. Read it as a warning about inputs, not as support for the design. | weak | VERIFIED-SOFTENED |
| [[systems/terrier-row-scoring#Expected-value cells and uncertainty draws (C0, C3)|Expected-value cells and uncertainty draws (C0, C3)]] | method-source | Inversion method: draw u uniform on [0,1), locate it on the CDF, and take the corresponding x. The paper demonstrates it on the exponential distribution and says it works for any distribution with an invertible CDF. It does not discuss discrete or binary cases; extending it to a categorical inverse CDF (and u < p for binary) is standard but is the analyst's extension, not the paper's text. (pp.4-5 ('Sampling From The Exponential Distribution Using The Inversion Method'); p.3 (PDF non-negative, integrates to 1)) | C0 draws give each (replicate, field, row) one PCG64 uniform via numpy default_rng; C3 draws give one uniform per (replicate, household), and only under C3 do the four negative lean fields use 1 - u. A binary cell is set when u < p. A categorical cell takes idx = count of cumulative thresholds at or below u, with codes ordered highest exposure first (docstring), i.e. inverse-CDF sampling. Results feed streamed accumulators (Welford moments, histograms, cut counters, top-10 counts). `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/rowfw/draws.py:1-7,58-91` | The sampling mechanics transfer because the math is the same. Correct mechanics do not make the output correct: the draws are only as good as the imputation class probabilities, whose calibration remains open (the paper's input-quality caveat). Independence across cells in C0 is a model assumption the paper does not address. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-validation#Receptivity sensitivity harness, MC stability and F5 power gate|Receptivity sensitivity harness, MC stability and F5 power gate]] | context | The paper reports no error analysis, convergence diagnostic or comparison of samplers. On randomness quality it says LCGs do not do well on many Diehard-type tests and that the Mersenne Twister is becoming the generator of choice, passing most tests except some for encryption RNGs. (p.4 ('Random Number Generators'); whole paper (absence of error analysis)) | mc_stability_check.py reruns the single-topic frontier at MC = 100 (SEED 7) and at MC = 20 with SEED 1 and 13, comparing picked seed sets by Jaccard overlap, first-k identical picks and final-spread delta. It sets no numeric pass bar. Bystander rolls in scenario.py use Python random (Mersenne Twister). Edge draws use splitmix64, which the paper does not discuss. `fec/engine/mc_stability_check.py:3-44; fec/engine/scenario.py:56-61` | The system goes further than the source. The paper can endorse the generator choice but supplies no criterion for MC budget or stability. The ROBUST bar (top-20 overlap >= 18) lives in fec/members/receptivity_sensitivity.py:113, a constant-sensitivity check, not in mc_stability_check.py; any MC-stability bar would have to come from elsewhere (the script's docstring cites Almazaydeh et al. 2026 as motivation only). | weak | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Scenario runner (what-if)|Scenario runner (what-if)]] | context | Monte Carlo is a key tool for analytically intractable problems. Some feel it is overused as a lazy way out, and analytic and experimental alternatives should be considered before committing to simulation. (p.2 (end of History section); p.6 (Summary)) | The scenario runner reruns the competitive cascade at MC = 40 under edge-keyed CRN for money, defection and stance moves. The sibling flip channel is closed-form: flip_model.pflip returns a/(a+b). The in-browser desk computes break-even/marginal effects as tgt * (pflip_new - pfbase) from the same closed form. Simulation is used for the cascade spread. `fec/engine/scenario.py:27,44-61; fec/engine/flip_model.py:92-97; fec/surfaces/staffer_live/approved/network.html:1087` | The split between closed-form flip math and MC spread fits the paper's advice. The paper offers nothing on which analytic proxy (Viral Centrality, Katz-Bonacich) might replace the cascade MC; that comparison would need its own test. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The analytic-first advice and the drawbacks list are in the History section (p.2), not the Introduction as the bearings imply.
> - Summary issue: The paper presents inversion only for distributions with an invertible CDF (exponential example); calling Terrier's categorical and binary draws 'the paper's inversion method' is a fair extension but not something the paper states.
> - Summary issue: 'Paul's wish to test an analytic proxy such as Viral Centrality' and 'his Fink note' cannot be checked from this item: Paul's only entry here (Raychaudhuri 2008) is empty. Treat as UNVERIFIED cross-item context.
> - Summary issue: The no-MC-standard-error gap is supported for scenario.py, budget_frontier.py and per_topic_coalition.py (each returns tot/MC) but remains UNVERIFIED system-wide, as the summary itself says.
> - Summary issue: Bearing 4's break-even equation is not in the engine code as written; only pflip = a/(a+b) (flip_model.py) and a pfbase difference in the browser desk were found.

## Gaps and flags

- Scenario, coalition and frontier spreads are returned as point means (tot/MC) with no Monte Carlo standard error or interval. The paper's warning that simulation gives no exact answers implies these figures should carry an MC error. UNVERIFIED: based on a search of scenario.py, budget_frontier.py, per_topic_coalition.py and mc_stability_check.py only.
- Nobody has compared the cascade MC with an analytic proxy (Viral Centrality, or using the kb_benchmark Katz-Bonacich as a predictor of spread). The paper's advice to try analytic methods first points to this comparison.
- The kernel inputs (saturation constant 6, bystander scale $400k, cross-party factor) are unestimated. The paper says the output is only as good as these inputs, and nothing validates them.
- Terrier's draws assume that imputation class probabilities are calibrated. Draws by inversion from miscalibrated probabilities would be exact samples from the wrong PDF.
- Paul's annotation entry for this cluster names Raychaudhuri 2008 (WSC), not Harrison 2010, and it is empty: no takeaways, system links or anchor. Harrison's markup is in the extraction but has no annotation note. The 'Raychaudhuri 2008 / Harrison 2010' citation pairing in the component positions treats them as one entry, so the two papers should not be conflated.
- The paper cannot validate the MC = 20 or MC = 40 budgets, the CRN design or any cascade parameter. Calling it support for the competitive cascade would overstate it.
- No conflict found with D-025, D-021 or D-104..D-108, and the paper argues for neither of the rejected SNA proposals.

Paul's PDF highlights (the three-step pattern, the green highlight on the key points for defining a model, the point that input density limits accuracy, and the advice to use library functions) fit how the system is built: Python's Mersenne Twister generates the bystander draws, the cascade follows sample-and-tally, and Terrier uses inversion. The system went with MC plus CRN and has not tested an analytic proxy. His annotation record is empty, so his notes do not record that the built cascade uses edge-keyed splitmix64 common random numbers, which neither tutorial covers.

## Related papers

- [[weimer-2023-causal-approach-detecting]] — complements — Weimer runs a Monte Carlo simulation as a negative control for an empirical momentum estimator, an applied use of the sample-and-tally recipe Harrison describes. Weimer's own analysis notes it does not validate the forward cascade MC.
- [[almazaydeh-2026-heuristic-centrality-methods]] — complements — It evaluates CELF++ with 50 Monte Carlo simulations per estimate (capturing 11.2% of sampling-based spread), the MC-budget noise evidence that motivates mc_stability_check.py and that Harrison's tutorial does not cover.
- [[bharathi-nd-competitive-influence-maximization]] — complements — It defines the competitive extension of the independent cascade model whose spread the engine estimates with the Monte Carlo recipe Harrison describes.
- [[trivedi-2020-efficient-influence-maximization]] — complements — It states that IC spread is approximated by large Monte-Carlo simulations whose accuracy scales with the number of runs, the computing cost Harrison flags as a drawback.

### Linked from
- [[jacoby-2025-mathematical-modeling-analysis]] — complements — Jacoby's sensitivity runs are deterministic sweeps of 1000 samples. Monte Carlo practice is the stochastic extension the authors say would enhance the model (p.22). Not cited by Jacoby.
- [[mccartan-2022-simulated-redistricting-plans]] — built-on-by — The SMC sampler and its convergence diagnostics (Gelman-Rubin R of about 1.05 or less, resampling weights) extend the basic Monte Carlo sampling that Harrison introduces.