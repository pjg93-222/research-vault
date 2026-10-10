---
type: paper
title: "Party Cues in the News: Democratic Elites, Republican Backlash and the Dynamics of Climate Skepticism"
authors:
  - "Eric Merkley (Munk School of Global Affairs & Public Policy, University of Toronto)"
  - "Dominik A. Stecula (Annenberg Public Policy Center, University of Pennsylvania; Simon Fraser University)"
year: ""
venue: "British Journal of Political Science (marked \"Forthcoming\" in the PDF; no volume, issue or DOI given)"
doi: ""
category: "Campaign Politics"
systems:
  - "melange-influence-engine"
  - "melange-stance"
components:
  - "melange-influence-engine.cascade-kernel"
  - "melange-influence-engine.lt-priming"
  - "melange-influence-engine.salience-shock"
  - "melange-stance.lsd-tone"
paul_annotated: true
paul_markup_count: 51
evidence_status: "verified"
source: "[[_sources/merkley-nd-party-cues-news.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Downloads\\MerkleyStecula-BJPolSPre-print.pdf"
sha12:
  - "049ac0d19237"
tags: [paper, campaign-politics]
---

# Party Cues in the News: Democratic Elites, Republican Backlash and the Dynamics of Climate Skepticism

**Eric Merkley (Munk School of Global Affairs & Public Policy, University of Toronto), Dominik A. Stecula (Annenberg Public Policy Center, University of Pennsylvania; Simon Fraser University) ().** *British Journal of Political Science (marked "Forthcoming" in the PDF; no volume, issue or DOI given).* Theme: [[themes/campaign-politics|Campaign Politics]].

> [!abstract] Summary
> Merkley and Stecula ask whether climate skepticism in the US public grew because people reacted against Democratic elites, with news coverage carrying those cues. They combine three methods. First, a Lexicoder dictionary flags party cues in NYT and WaPo climate coverage. Second, they run time-series models on a latent skepticism index built from about 172 poll questions. Third, they run a 2019 MTurk experiment with N=2,970.
> 
> In the time series, the share of articles carrying Democratic cues Granger-causes skepticism, both in the aggregate (chi2 6.96, p=0.01) and among Republicans (chi2 7.35, p=0.03). The reverse direction does not hold. Republican cues have no aggregate effect (p=0.94). In the experiment, a single Democratic cue moves strong Republicans +0.06 toward skepticism on a 0-1 scale (about 0.2 SD). A polarized cue, Democrats for and Republicans against, moves them +0.09. A consensus cue, with Republicans joining Democrats, shows no polarizing effect and no de-polarizing effect. More coverage alone, with cues held fixed, goes with lower Republican skepticism (-0.4 SD per +100 articles a quarter, in the Republican-skepticism model, Model 3). The Granger tests show only which series moves first, not causation. The causal evidence is one MTurk experiment on mass partisans, not legislators.
> 
> For Melange, the paper's value is about direction. An out-party cue is not a weaker positive cue. Among mass partisans it pushes the other way. Three parts of the engine encode out-party cues differently:
> - The cascade kernel's cpf_pressure gives an out-party source 0.5x pull, a weaker push in the same direction.
> - The LT-priming cross-cue is clamped at 0 and never goes negative.
> - The salience shock already has a Retreat regime that moves out-partisans -1, but its Rally regime still gives +0.5 to out-partisans whose bpf is at least 0.30. The paper's consensus condition found no polarizing effect.
> 
> The paper does not supply a legislator-level backfire magnitude. Every transfer runs from the mass public to members of Congress and from climate to credit-union issues, so any recalibration remains Paul's decision.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Paul's markup in the PDF

- p1 Highlight: "Paraphrase: thesis that backlash to Democratic elite out-group cues drove the shift in climate skepticism. (Exact passage at index 1 of annotation file.)" — 
- p1 Highlight: "Paraphrase: plan to build time-series measures of possible contributors to skepticism." — 
- p1 Highlight: "Paraphrase: measures come from automated media content analysis." — 
- p2 Highlight: "Paraphrase: Republicans once held climate beliefs comparable to Democrats (1997 gap example)." — 
- p2 Highlight: "Paraphrase: Republican skepticism may have grown from polarizing party cues carried by news media." — 
- p3 Highlight: "Paraphrase: news-media dynamics of cues plus frames (economic cost, scientific uncertainty) as contributors." — 
- p3 Highlight: "Paraphrase: contribution claim, combining text analysis, time series and experiments on a high-salience issue." — 
- p5 Highlight: "Paraphrase: elite cueing theory passes a placebo test, with no effect where elites are not divided." — 
- p6 Highlight: "Paraphrase: H1 is tested with a vector autoregression." — 
- p6 Highlight: "Paraphrase: VAR limitation, no contemporaneous relationships." — 
- p6 Highlight: "Paraphrase: the key variable is the prevalence of Democratic Party elite cues." — 
- p6 Highlight: "Paraphrase: the outcome is aggregate levels of skepticism toward climate science." — 
- p7 Highlight: "Paraphrase: H2 predicts a positive contemporaneous correlation, holding other factors constant." — 
- p7 Highlight: "Paraphrase: the holding-constant clause of H2." — 
- p7 Highlight: "Paraphrase: gap, out-group cueing is little studied and climate change has rarely been a test case." — 
- p9 Highlight: "Paraphrase: opinion measures are standardized for interpretability." — 
- p9 Highlight: "Paraphrase: NYT and WaPo chosen as high-circulation agenda-setting papers." — 
- p10 Highlight: "Paraphrase: definition of a party cue as a stance on climate science or policy attributed to party elites." — 
- p10 Highlight: "Paraphrase: Lexicoder plus a dictionary of key terms is used to find cues." — 
- p10 Highlight: "Paraphrase: the dictionaries are in supplementary section B." — 
- p10 Highlight: "Paraphrase: 97% of Democratic-elite articles were wholly supportive of the consensus." — 
- p13 Highlight: "Paraphrase: regression equation (1), lagged skepticism, Democratic cues and controls." — 
- p13 Highlight: "Paraphrase: coding rule for the uncertainty frame (questioning elements of the IPCC consensus)." — 
- p14 Highlight: "Paraphrase: SVM classifier used on manually coded articles." — 
- p14 Highlight: "Paraphrase: definition of SVM as a hyperplane separating classes." — 
- p14 Highlight: "Paraphrase: classifier and manual coding agree reasonably closely." — 
- p14 Highlight: "Paraphrase: Republican cues correlate with economic-cost frames (0.64 quarterly, 0.55 annual)." — 
- p14 Highlight: "Paraphrase: Republican cues correlate only weakly with uncertainty frames (0.34 and -0.07)." — 
- p15 Highlight: "Paraphrase: Democratic cues correlate highly with salience (0.57, quarterly and annual)." — 
- p15 Highlight: "Paraphrase: Republican cues do not show that salience correlation (-0.45 and 0.11)." — 
- p15 Highlight: "Paraphrase: Republican cues weakly Granger-cause Republican skepticism (p~0.09)." — 
- p15 Highlight: "Paraphrase: skepticism does not Granger-cause Republican cues." — 
- p16 Highlight: "Paraphrase: earlier analyses do not show contemporaneous relationships." — 
- p17 Highlight: "Paraphrase: Republicans become less skeptical as salience rises, after controlling for party cues." — 
- p17 Highlight: "Paraphrase: 100 more articles per quarter linked to a 0.4 SD drop in Republican skepticism." — 
- p18 Highlight: "Paraphrase: the MTurk sample skews younger, wealthier and more educated." — 
- p18 Highlight: "Paraphrase: control condition received no cue." — 
- p18 Highlight: "Paraphrase: description of the Democratic, opposing Republican and polarized cue conditions." — 
- p19 Highlight: "Paraphrase: possible moderation by trust in scientists, independent of partisanship." — 
- p20 Highlight: "Paraphrase: claim that results provide strong support for H3." — 
- p21 Highlight: "Paraphrase: supporting Republican and consensus cues did not polarize respondents." — 
- p21 Highlight: "Paraphrase: effects modest but notable given decades of polarization and thin treatments." — 
- p22 Highlight: "Paraphrase: scholars should study over-time dynamics in the news environment." — 
- p22 Highlight: "Paraphrase: Democratic out-group cues caused attitudinal backlash among Republican voters." — 
- p22 Highlight: "Paraphrase: claim of central importance of out-group cues, using text, time series and experiments." — 
- p22 Highlight: "Paraphrase: Democratic elite cues are the most consistent predictor of skepticism, especially among Republicans." — 
- p22 Highlight: "Paraphrase: Democratic-supporter null may reflect a ceiling effect." — 
- p23 Highlight: "Paraphrase: party elites must weigh the costs of aggressively communicating their stance in media." — 
- p23 Highlight: "Paraphrase: consensus cue did not depolarize; a stronger cue from respected Republicans may work." — 
- p23 Highlight: "Paraphrase: mobilizing an elite consensus across partisan lines is needed." — 
- p24 Highlight: "Paraphrase: cross-partisan elite consensus is the most promising strategy for bringing opinion in line with science." — 

## Study

**Question.** Does out-group cueing from Democratic elites, delivered through news coverage, drive the rise of climate skepticism in the US public, especially among Republicans, and can that effect be shown to be causal rather than a by-product of confounders?

**Theory.** Elite cueing and out-group backlash. Partisans use party elites as cognitive shortcuts and affective anchors (Cohen 2003; Kam 2005; Lenz 2012; Lodge and Taber 2013). Out-group cues can repel rather than persuade partisans, plausibly via affective polarization (Iyengar et al. 2012). The authors contrast this with motivated-reasoning and ideology accounts, which they say cannot easily explain the over-time dynamics, since Republicans once matched Democrats on climate beliefs. News media carry elite cues to the mass public (pp. 2-6). Hypotheses: H1, Democratic elite cues lead aggregate skepticism; H2, a positive contemporaneous correlation holds after controls; H3, exposure to a Democratic cue raises skepticism among Republicans (pp. 5-8).

**Data.** Aggregate opinion: about 172 Roper Center poll questions (the abstract says "nearly 200 polls") combined into a latent skepticism index. Quarterly 2001-2014 (N=55 in Table 2 Model 1; N=54 for the Republican series) and annual 1986-2014 (N=28). Republican-only series is the Partisan Climate Change Threat Index of Carmichael and Brulle, reverse-coded (correlation 0.48 with the authors' quarterly series). News: climate coverage in the New York Times and Washington Post (LexisNexis), with 1,000+ articles hand-screened for relevance. Validation: 700 party-referencing articles (about 80% contain a cue); 3,000 articles coded for stance (97% of Democratic-cue articles fully pro-consensus); 2,179 newspaper articles coded for economic-cost and uncertainty frames (classifier training sets of 1,878 and 1,179). Experiment: MTurk, 2019, N=2,970 analysed (condition sizes 471-534), with 85% passing screeners and 130 (4%) dropped for failing both (p. 19 and fn 10).

**Method.** Text-as-data pipeline: Lexicoder plus a custom dictionary of party names, office titles and leaders to flag elite cues. Share of articles with Democratic cues and Republican cues. Republican cue measure is the cue share multiplied by the share of hostile Republican messages (validated against LCV scores, r=0.72). Frames via SVM (80% accuracy for cost frames, recall 0.76 and precision 0.80; 77% accuracy for uncertainty frames, recall 0.70 and precision 0.68). Time series: reduced-form VAR with lags up to four quarters (higher lag chosen when tests disagree; stability checked) and Granger causality tests, with controls for NOAA climate index, oil prices, unemployment and quarter dummies (pp. 11-13). Contemporaneous models: lagged-dependent-variable OLS with robust SEs (Eq. 1), controls for Republican cues, uncertainty and cost frames, salience (NYT plus WaPo volume), climate, oil, unemployment (Table 2). Experiment: OLS (Eq. 2) interacting five treatment dummies (Democratic cue; opposing GOP cue; polarized cues; supporting GOP cue; consensus cues) with 7-point partisanship and with controls (trust in scientists, political interest) and their interactions with treatment. Outcome is a 7-point agree/disagree item on human-caused warming, rescaled 0-1 (pp. 18-21).

**Identification.** Time series: Granger tests establish temporal precedence only, and the authors state the design is not causal (pp. 6, 12, 17). Contemporaneous models add controls for in-group cues, frames, salience, and lagged skepticism, but remain observational. Experiment: random assignment to six conditions (control; Democratic cue; opposing GOP cue; polarized; supporting GOP cue; consensus), so the cue effect is identified within the randomized conditions. Partisanship is not randomized and enters as a moderator (p. 19). The effects reported are conditional on strong partisanship. Effects are estimated through the PID-by-treatment interaction, not a simple randomized mean difference.

**Scope.** Population: US public (aggregate polls) and a US online MTurk sample (2019) that the authors say matches the public on gender, race, partisanship and ideology but skews younger, wealthier and more educated (p. 18). Institution: national newspaper coverage (NYT and WaPo) only. Era: 1986-2014 for annual opinion and 2001-2014 for quarterly series; experiment in 2019. Issue: climate change science and mitigation.

## Key findings

- Democratic elite cues Granger-cause aggregate climate skepticism; the reverse does not hold. (Dem cues -> skepticism chi2 6.96, p=0.01 (1 lag); skepticism -> Dem cues chi2 1.01, p=0.32; Results, Table 1, p. 15)
- Democratic cues Granger-cause skepticism among Republicans; the reverse does not hold. (Dem cues -> GOP skepticism chi2 7.35, p=0.03 (2 lags); GOP skepticism -> Dem cues chi2 0.29, p=0.86; Table 1, p. 15)
- Republican cues show weak and inconsistent cue-taking: no effect on aggregate skepticism, weak effect on Republican skepticism. (GOP cues -> aggregate chi2 0.00, p=0.94; GOP cues -> GOP skepticism chi2 2.94, p=0.09; skepticism -> GOP cues p=0.82 and p=0.99; Table 1, p. 15)
- Contemporaneous association: Democratic cues correlate with aggregate skepticism after controls, weaker in the quarterly model. (Quarterly coefficient 0.02 (SE 0.01), p~0.07; 10-point rise in Dem cue share = 0.2 SD in skepticism; N=55; Table 2 Model 1 and text p. 16)
- Annual aggregate model shows the same association. (Coefficient 0.02 (p~0.09); 10-point rise = 0.2 SD; R2=0.89; N=28. Republican cues not significant (p~0.60); Table 2 Model 2 and text p. 17)
- Republican-specific skepticism responds to Democratic cues (H2 support) and to Republican cues. (Dem cues 0.03 (p<0.01) = 0.3 SD per 10 points; GOP cues 0.04 (p~0.02) = 0.4 SD per 10 points; N=54; Table 2 Model 3 and text p. 17)
- Unemployment and salience also correlate with Republican skepticism; more coverage is associated with lower Republican skepticism after controls. (1-point unemployment rise = 0.3 SD (p<0.001); +100 articles per quarter = -0.4 SD (p~0.03); Table 2 Model 3 and text p. 17)
- Descriptive: Democratic cues track salience; Republican cues track economic-cost framing more than uncertainty framing. (Dem cues vs salience 0.57 quarterly and 0.57 annual; Rep cues vs salience -0.45 and 0.11; Rep cues vs cost frames 0.64 quarterly and 0.55 annual; vs uncertainty 0.34 and -0.07; p. 14-15)
- Democratic-cue articles are almost uniformly pro-consensus; Republican hostility to climate action is measured via a composite. (97% of Democratic-cue articles fully supportive of consensus (3,000-article hand-coded sample); cue screen validated at about 80% (700 articles); Rep composite correlates 0.72 with LCV scores; coder agreement alpha 0.98 (Dem), 0.83 (GOP); p. 10-11, fn 5)
- Survey experiment: a Democratic elite cue polarizes strong Republicans toward skepticism (H3 support). (Strong Republicans +0.06 on 0-1 scale (about 0.2 SD, p~0.03); interaction p~0.05. Strong Democrats: no significant effect; Experimental Results, Figure 3, p. 20)
- Opposing GOP cue and polarized cues (Democratic plus opposing Republican) raise Republican skepticism; polarized effect is numerically the largest, though not reported as significantly different from the other cue effects. (Opposing GOP: +0.06 (p~0.02; interaction p~0.02). Polarized: +0.09 (about 0.3 SD, p~0.002; interaction p~0.003); strong Democrats +0.04 (p~0.09); p. 20-21, Figure 3 Panels B-C)
- Supporting GOP cue and consensus cue (Democratic plus supportive Republican) produce no polarization; the consensus cue attenuates the Democratic backlash. (No significant effect for supporting GOP or consensus conditions; no depolarizing effect reported; p. 21 and discussion p. 22-23)
- Control condition baseline: 80% of respondents agree with the consensus at some level and 13% disagree. (80% agree at some level; 13% disagree; scale rescaled 0-1; p. 19)

## Limitations

- VAR and Granger analyses cannot speak to contemporaneous relationships or causality; authors say so (pp. 6, 12, 16).
- Aggregate evidence is described as only suggestive; unmeasured confounders over time remain (p. 17).
- Results are sensitive to lag length; the authors pick the higher lag when tests disagree and note the sensitivity (fn 6, p. 12).
- Small time series (N=28 annual, 54-55 quarterly) with many controls.
- Cue measure is a share of articles from dictionary-based classification, not message volume; volume robustness is only in supplementary Table D1 (p. 10).
- Republican hostility measure is a composite built from a 3,000-article coding and an LCV correlation (0.72); the authors call the stance indicator valid but it is not directly measured for every period (pp. 10-11).
- Only NYT and WaPo are used; the authors acknowledge partisan online and social media are excluded (fn 3, p. 9).
- Inconsistent counts: the abstract says nearly 200 polls, the body uses 172 poll questions (p. 8). The abstract says about 3,000 respondents; 2,970 were analysed.
- Experimental sample is MTurk, skewing younger, wealthier and more educated; screeners address bot and VPN concerns (pp. 18-19, fn 10).
- Partisanship is not randomized; treatment effects are moderated by PID, which may carry confounding (p. 19).
- No effect among Democrats, possibly from a ceiling effect (p. 22).
- Treatments are thin and the outcome is a single 7-point item (pp. 18-21).
- Consensus cue did not depolarize; the authors suggest a stronger treatment from respected Republicans may be needed (p. 23).
- Table 2 layout is hard to read: the text reports Model 1 Republican cues as non-significant, while the table marks Dem cues only with stars; p-values are reported as approximate (p~).

## Short quotes

- "Republicans are repelled, rather than persuaded by Democratic elite cues" — p. 17 (Results, contemporaneous models; Merkley and Stecula)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Salience shock (Noble-Turner going public)\|Salience shock (Noble-Turner going public)]] | qualifies | A polarized cue (Democrats for, Republicans against) moves strong Republicans +0.09 points on the 0-1 scale (about 0.3 SD, p~0.002). It is numerically the largest cue effect, but the authors do not report it as significantly different from the single Democratic cue or opposing-GOP cue effects. A single Democratic cue moves them +0.06 (p~0.03). Supporting-GOP and consensus cues show no polarizing effect and no de-polarizing effect. Observationally, +100 articles a quarter with cues held fixed goes with -0.4 SD in Republican skepticism (Model 3, PCCTI, N=54; p~0.03). (Experimental results and Figure 3 Panels A-C, pp. 20-21; Table 2 Model 3, p. 17; discussion pp. 22-23) | Going public is three regimes. Retreat moves co-partisans +1 and out-partisans -1, matching the paper's polarized-cue backlash direction. Rally moves out-partisans +0.5 only when bpf is at least 0.30, which the paper's consensus null does not support. Regime probabilities p and q come from the D-R tier gap z in salience_issue_params, and the advisory actor party defaults to R. `fec/engine/salience_shock.py:99-111 (steps_for); fec/engine/salience_shock.py:185 (party default R); fec/moneyball_core.py:736-750 (salience_issue_params)` | Carries over only as a direction-of-effect prior. The paper studies mass partisans reacting to elite cues in national newspapers on climate; the engine moves legislators' stance tiers on credit-union issues. The rally result rests on one thin MTurk treatment, so it weakens but does not refute out-partisan rally movement. Treating z as endogenous to going public is the analyst's inference, not the paper's; the Granger results concern cue shares and skepticism, not the partisan stance gap. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Linear-threshold priming (cheapest tip)\|Linear-threshold priming (cheapest tip)]] | supports | Out-group (Democratic) elite cues move Republicans away from the cued position rather than toward it. Experiment: strong Republicans +0.06 toward skepticism (about 0.2 SD, p~0.03), with a party-ID interaction p~0.05. Time series: Democratic cues Granger-cause Republican skepticism (chi2 7.35, p=0.03, 2 lags); the reverse has p=0.86. (Table 1, p. 15; Figure 3 Panel A and text, p. 20) | The Bakker cross-cue factor xf passes a cross-party pro-cue through sqrt(bpf_u*bpf_v), so a non-bridge out-party cue falls to about 0. The priming term is clamped at 0 and never goes negative; the code comment says backfire magnitude is not invented. `fec/engine/lt_priming.py:56-64 (comment and xf at line 64)` | The paper supports that out-party cues do not persuade and reads as backlash, so a negative sign is consistent with it. But the +0.06 effect is measured on mass Republicans and one climate item, not on legislators reading cosponsorship signals, so it cannot set a backfire magnitude for the priming term. Keeping the zero clamp remains defensible. | moderate | VERIFIED |
| [[systems/melange-influence-engine#Cascade activation kernel build_P and partisan edge factors\|Cascade activation kernel build_P and partisan edge factors]] | qualifies | Democratic cues move Republicans toward skepticism, away from the cued position, in both the experiment and the time series (backlash rather than attenuated persuasion). Republican cues have no aggregate effect (chi2 0.00, p=0.94) and only a weak effect on Republican skepticism (chi2 2.94, p~0.09). The authors say co-partisan sources dominate. (Table 1, p. 15; Figure 3, p. 20; discussion p. 22) | cpf_pressure gives an out-partisan source a 0.5x pull and a co-partisan source 1.7x; cpf_diffusion keeps a 0.4 cross-party floor. Both keep out-party influence positive and weaker, not negative. `fec/moneyball_core.py:543-548 (cpf_diffusion, cpf_pressure); fec/moneyball_core.py:9-12 (physics note)` | The paper supports co-partisan sources dominating, which fits the 1.7 versus 0.5 ordering. It reports backlash, meaning movement away from the cued position, but estimates no multiplier relative to co-partisan influence, so it cannot show the out-partisan sign should be at or below zero rather than 0.5. Cosponsorship ties are observed behaviour rather than cues, so transfer is weak. The paper gives no support for the 1.7/0.5 ratio itself. | weak | VERIFIED-SOFTENED |
| [[systems/melange-stance#Lexicoder LSD2015 tone intensity\|Lexicoder LSD2015 tone intensity]] | qualifies | The authors use Lexicoder with a custom dictionary of party names, office titles and party leaders to detect party cues, not sentiment or tone. They validate detection against hand coding of a random 700 articles (about 80% contain a cue by their definition). Stance is hand-coded on a separate random sample of 3000 articles (97% of articles with Democratic messages are wholly pro-consensus). Frames come from SVM classifiers (77-80% accuracy). (Data and methods, pp. 10-11 (dictionary, 700-article validation, 3000-article stance coding); fn 5, pp. 10-11; pp. 13-14 (SVM frames); fn 9, p. 14 (accuracy)) | stage5_lsd.R runs the LSD2015 dictionary in lsd_tone (lines 49-60), returning net tone and charge per word. Live run_stage5.R defaults the intensity to the absolute net tone per word (INTENSITY default net_norm, line 45), with charge only as an option, and press_value equals final_sign times that intensity at lines 141-143. The bearing's earlier statement that charge per word is the intensity used in press_value was wrong. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_lsd.R:49-60; C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R:45 and 141-143` | The paper is a method precedent only: a dictionary for detecting actors plus hand-coded validation. It never uses or validates sentiment or tone as an intensity measure, so it is no evidence that LSD tone intensity is valid on congressional press releases. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Consensus cue wording overstates the null: summary says it 'produces no movement and does not depolarize'; the paper reports no polarizing effect and no de-polarizing effect found for the consensus and supporting-GOP conditions (pp. 21-23).
> - Summary issue: Summary says 'the paper's consensus condition found no movement at all'; the paper reports no polarizing effect, not zero movement, and the Democratic-cue effect is described as attenuated by the supporting GOP cue (p. 21).
> - Summary issue: Summary calls the Rally regime's +0.5 'bridge-builders'; the code gives +0.5 to out-partisans whose bpf is at least 0.30 (cross-pressured out-partisans, salience_shock.py:99-111).
> - Summary issue: The -0.4 SD per +100 articles a quarter figure comes from Model 3 (Republican skepticism, PCCTI, N=54), not aggregate skepticism; the summary should say so.
> - Summary issue: Summary is otherwise accurate on the Granger statistics (6.96/0.01; 7.35/0.03; reverse 0.32 and 0.86; GOP cues 0.00/0.94), the experiment effects (+0.06 single cue, +0.09 polarized, about 0.2 and 0.3 SD), N=2,970, 172 poll questions, and the 2019 MTurk design.
> - Summary issue: Paul's highlight claims in paul_alignment were not checked against the annotation file (_annots/049ac0d19237.json); they are outside summary_md but the alignment text is unverified.
> - Summary issue: The LSD bearing's system_side and code_ref were corrected in this verification (default intensity is net tone per word; press_value is in run_stage5.R).

## Gaps and flags

- The engine never lets a source's party identity produce a negative cue. The cascade kernel, LT priming and the rally regime all keep out-party influence at zero or above. The only negative out-party movement is the salience shock's Retreat and Backfire regimes, and it never feeds the cascade kernel.
- The salience shock takes partisan polarization (z) as a fixed snapshot of the stance matrix. Nothing models a CU campaign led by one party creating or widening the partisan gap over later quarters, which is the dynamic the Granger results describe.
- Message attribution, meaning which party's elites are seen as owning a CU issue in press and news, is not measured. The frame-alignment and press layers measure duplication and stance, not attribution of party cues in third-party coverage.
- The salience shock has no consensus regime. The paper's consensus condition (both parties supportive) is a distinct case with no polarizing effect, not a rally that moves out-partisans +0.5.
- The paper's aggregate contemporaneous coefficient (0.02, p~0.07 quarterly; p~0.09 annual) pools Democrats and Republicans across the partisan fault line the paper is about. If quoted, label it UNSTRATIFIED and pair it with the Republican-only Model 3 (0.03, p<0.01, N=54, quarterly 2001-2014).
- The Granger results depend on lag choice (the authors take the higher lag when tests disagree, fn 6). Quote them as evidence of which series moves first, never as causal.
- Any proposal to give out-partisan pressure a negative sign, or to set a backfire magnitude in lt_priming, from the +0.06 or +0.09 MTurk effects would be new analysis beyond the paper. Those effects are mass-public climate effects and say nothing about legislators.
- No conflict with the rejected SNA proposals, D-021 or D-025. The paper argues for neither dropping cosponsorship edges nor per-edge volume normalization.

## Related papers

- [[noble-2024-presidential-leadership-legislative]] — complements — Noble and Turner's item covers going-public structure (Prop. 1, p. 13: going public widens the gridlock region), which the salience shock implements. Merkley and Stecula give mass-public evidence that the out-partisan retreat direction is real (polarized-cue backlash). Their consensus null is not a test of Noble's rally regime, so it does not show a rally fails to move out-partisans.
- [[bakker-nd-understanding-partisan-receptivity]] — complements — Bakker et al. (Study 1: out-party cue -0.06 on a 0-1 scale, p<.01) is the cited basis for the LT-priming cross-cue clamp. Merkley and Stecula independently find out-party cues move Republicans against the cue, on climate and in news coverage.
- [[boxsteffensmeier-2018-taking-congress-interest]] — complements — Box-Steffensmeier et al. study legislators taking cues from interest-group Dear Colleague letters; Merkley and Stecula study the mass public taking cues from party elites in news. Together they mark the legislator versus voter boundary for cue parameters.
- [[rice-2018-corpus-based-dictionaries]] — same-method — Rice and Zorn's item shows corpus-based dictionaries beat standard ones (LIWC, AFINN) on specialized vocabulary, which bears on applying LSD2015 to congressional releases. Merkley and Stecula use Lexicoder with a custom party-cue dictionary validated against hand coding.
- [[haselmayer-2017-sentiment-analysis-political]] — same-method — Haselmayer and Jenny pair a dictionary with crowdcoding validation (480 coders, 92% mean accuracy). Merkley and Stecula pair a dictionary with hand-coded validation of 700 articles. Whether the LSD tone layer has an equivalent validation sample was not checked (validate_stage5.R was not read).

### Linked from
- [[vanderdoes-2022-strategic-identity-signaling]] — complements — Merkley and Stecula show outgroup backlash to overt partisan elite cues in news. This paper explains why senders facing mixed audiences shift to covert cues to avoid that backlash.
- [[rheault-2016-measuring-emotion-parliamentary]] — complements — Uses Lexicoder plus a custom dictionary of party names and leaders to detect party cues, not sentiment, and validates that dictionary against hand-coded articles. Verified in the Merkley item; the link is about dictionary practice, not about tone measurement.