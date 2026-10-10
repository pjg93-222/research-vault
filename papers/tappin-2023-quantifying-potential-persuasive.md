---
type: paper
title: "Quantifying the potential persuasive returns to political microtargeting"
authors: ["Ben M. Tappin", "Chloe Wittenberg", "Luke B. Hewitt", "Adam J. Berinsky", "David G. Rand"]
year: "2023"
venue: "PNAS 120(25): e2216261120 (published June 12, 2023)"
doi: "10.1073/pnas.2216261120"
category: "Campaign Politics"
systems: ["melange-influence-engine", "terrier-canvass"]
components: ["melange-influence-engine.persuadability-table", "terrier-canvass.untapped-potential"]
paul_annotated: false
paul_markup_count: 0
evidence_status: verified
source: "[[_sources/tappin-2023-quantifying-potential-persuasive.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\pnas.202216261.pdf"]
sha12: ["4bf1755ddc9a"]
tags: [paper, campaign-politics]
---

# Quantifying the potential persuasive returns to political microtargeting

**Ben M. Tappin, Chloe Wittenberg, Luke B. Hewitt, Adam J. Berinsky, David G. Rand (2023).** *PNAS 120(25): e2216261120 (published June 12, 2023).* [10.1073/pnas.2216261120](https://doi.org/10.1073/pnas.2216261120) Theme: [[themes/campaign-politics|Campaign Politics]].

> [!abstract] Summary
> Tappin, Wittenberg, Hewitt, Berinsky and Rand (PNAS 2023) test whether microtargeting, meaning pretesting messages and using generalized random forests to send each covariate profile the message predicted to persuade it most, beats two simpler strategies: random messages (naive) and one pretested best message for everyone (single-best-message, SBM). In a Feb 2022 Lucid sample (5,284 U.S. adults analyzed), Study 1 on a single policy found microtargeting moved support 5.96 pp vs 3.48 SBM and 1.79 naive (precision-weighted over two modules). Almost all of that came from the Citizenship Act module (5.17 vs 2.41). The UBI module was not significant. The advantage was qualitatively largest with one covariate, such as party or ideology alone, though differences across one, two to four, and more covariates were slight. In Study 2, across 36 issues, letting the targeted issue vary by person did not beat one best issue ad (12.30 vs 15.42, P=0.11), while microtargeting beat naive (12.30 vs 6.19, P=0.008). The authors say the advantage may be highly context dependent. They say forced-exposure survey estimates may overstate the relative advantage of microtargeting, and they did not test candidate races. For Paul's systems, the closest point of contact is Terrier's Untapped potential score. It ranks whom to contact by support closeness to 0.5, chance of voting and sphere of influence. It has no message dimension and no estimate of effect size. In the Citizenship Act module naive messaging was near zero, but the paper does not decompose the effect, and in the UBI module naive (5.70) beat single-best-message (5.03). In Study 1, coarse targeting on one covariate sufficed in-sample. Any message extension for Terrier would need its own pretest. Melange's legislator-side persuadability is barely touched: this is a mass-public survey study. Its relevance there is limited to context on low-information issues. No brain rule is in conflict.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study
**Question.** Does political microtargeting (message pretesting combined with machine learning, so each person sees the message predicted to persuade them most) produce a persuasive advantage over a naive random-message strategy and a single-best-message strategy? Does targeting by more covariates, or targeting issues rather than messages on one issue, add returns?

**Theory.** Microtargeting depends on treatment effect heterogeneity, meaning different people respond differently to different messages. Large-scale studies (refs 24-27) find heterogeneity usually small, so the persuasive returns to microtargeting are theoretically ambiguous. Study 2 frames the choice: if people are most persuadable on the same issues, show everyone one issue message; if persuadability differs across people by issue, targeting issues pays.

**Data.** Study 1 calibration: two existing survey experiments (ref 37). U.S. Citizenship Act (n=17,013; 26 pro-Act treatment messages plus control) and universal basic income (n=6,408; 10 anti-UBI treatment messages plus control). Study 2 calibration: first experiment of ref 38 (n=3,990 U.S. respondents; 9,712 observations; 36 policy issues; 48 professionally produced progressive video ads from the Peoria Project database). Experimental phase: new U.S. adult sample recruited through Lucid in Feb 2022, quota-matched on age, gender, ethnicity and region; 7,531 entered, 5,284 analyzed (completed at least one module). Three modules in random order. Unit: individual respondent. Outcome: policy support (Study 1, same items as calibration) or 5-point agreement with message position (Study 2, message-specific). Per-module N not stated in main text. Preregistered (osf.io/kvd9t). Data on OSF (osf.io/t3dhe).

**Method.** Calibration: for each covariate profile (255 profiles from 8 covariates in Study 1; 511 profiles from 9 covariates in Study 2), generalized random forests (grf R package) trained on 75 percent of data estimate message-level heterogeneity. The message predicted best for each subgroup is evaluated on the 25 percent test set by restricting the treatment group to people assigned their predicted-best message and comparing to control. Monte Carlo cross-validation: 250 random splits; median per profile; top profile in each stratum (1, 2-3, or 4+ covariates) refit on full data. Single-best-message = message with largest OLS ATE in training data. Experimental phase: OLS with condition dummies vs control, robust (HC2) SEs. Study 2: separate OLS per strategy, then difference-in-differences interaction models.

**Identification.** Randomized survey experiment. Condition assignment is independent of pretreatment covariates by design, so filtering the treatment group to predicted-best assignment preserves covariate balance in expectation. Targeting covariates were measured before treatment. Study 1 microtargeting arm further randomized across three covariate profiles with equal probability. Study 2 randomizes strategy first, then treatment vs control within strategy. Balance and attrition checks reported in SI (not reviewed).

**Scope.** Population: U.S. adults recruited online (quota-matched), February 2022. Issues: U.S. Citizenship Act, universal basic income, and 36 multi-issue advocacy topics (e.g., net neutrality, minimum wage, gun control, climate change). Setting: issue advocacy and ideology-shift campaigns; messages delivered by survey video, not field advertising. Era: calibration data from prior survey work; experimental data 2022; publication 2023.

## Key findings
- Study 1, U.S. Citizenship Act: microtargeting produced over twice the persuasive impact of single-best-message, and far more than naive messaging. (MT 5.17 vs SBM 2.41 pp (P=0.016); MT vs naive 5.17 vs -0.96 (P<0.001); p.4, Study 1 Experimental Phase)
- Study 1, UBI: microtargeting point estimates favor it over both alternatives but are not statistically significant. (MT 7.16 vs SBM 5.03 (P=0.12); MT vs naive 7.16 vs 5.70 (P=0.33); p.4, Study 1 Experimental Phase)
- Study 1 precision-weighted across both modules: microtargeting advantage is about 70 percent larger than SBM and over 200 percent larger than naive. (MT 5.96 vs SBM 3.48 (P=0.004); vs naive 1.79 (P<0.001); p.4, Study 1 Experimental Phase)
- No evidence that targeting by more covariates increases persuasive returns; the advantage was qualitatively largest with one covariate, though differences were slight. In Study 1, in-sample, coarse targeting by party or ideology alone produced an advantage. (Fig 2 profile values not mapped in text; differences described as relatively slight; p.4 text; Fig 2, p.5)
- Study 2, multi-issue: microtargeting is directionally but not significantly below single-best-message, and clearly above naive messaging. (MT 12.30 vs SBM 15.42 (P=0.11); MT vs naive 12.30 vs 6.19 (P=0.008); p.6, Study 2 Experimental Phase; Fig 3, p.6)
- Study 2 conclusion: targeting different issues to different people was not more impactful than distributing one pretested ad on one issue to everyone, though pretested targeting clearly beat naive. (; p.6, Study 2 summary)
- Messages predicted most persuasive in Study 2 often dealt with obscure policies (e.g., net neutrality), which the authors list as one hypothesis among several for the Study 2 divergence. (; p.8, Discussion)
- Study 1 headline advantage (70 percent or more) was driven primarily by the U.S. Citizenship Act messaging. (70 percent or more over alternatives; p.6, Discussion)
- Authors find no evidence that the microtargeting advantage requires vast personal data; in Study 1, in-sample, a single covariate was enough to generate the benefit observed. (; p.8, Discussion)
- Authors conclude the advantage of microtargeting may be highly context dependent, with clear gains in a policy-centered campaign and limited gains in a multi-issue campaign. (; p.8, Discussion (closing paragraph))

## Limitations
- Survey, not field, experiments: covariates measured precisely and exposure forced. Authors say this may overstate the advantage because campaigns lack high-quality covariates and mis-targeting may reduce efficacy (p.7).
- Social media ad delivery is nonrandom and idiosyncratic and may attenuate the advantage; engagement optimization might boost it (p.7).
- Forced exposure: respondents engaged with content they might have ignored in real life; roughly half of disseminated political ads may not reach intended audiences (p.7, citing ref 49).
- Only a small set of low-salience issues and messages; high-salience contexts with countermessaging likely yield smaller effects and possibly a smaller relative advantage (p.7).
- U.S. only; candidate-centered campaigns and non-U.S. contexts untested (p.7).
- Cost-effectiveness not assessed; microtargeting needs more time, money and data science than SBM, which could yield more persuasion per dollar (p.8).
- Study 2 divergence unresolved: pretest design was mixed between and within subjects, calibration preceded follow-up by almost a year, and all Study 2 messages were progressive professional videos (p.5, p.8).
- Study 1 excluded messages opposing the Act and supporting UBI due to resource constraints (p.9).
- Well-funded campaigns could train models at finer covariate resolution with larger pretest data, so results may understate the advantage (p.7).
- Per-module experimental N and balance/attrition checks are in SI, not the main text (p.9); SI not reviewed.
- Study 2 outcome variables differ by message, so strategy comparisons rely on a difference-in-differences design (p.9).

## Short quotes
- "we found no evidence that targeting messages by more than one covariate yielded additional persuasive gains" (p.1, Abstract)
- "the persuasive impact of microtargeting is directionally (although not significantly) smaller than the SBM strategy" (p.6, Study 2 Results)
- "this advantage may be highly context dependent" (p.6, Discussion)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/terrier-canvass#Untapped potential\|Untapped potential]] | suggests-extension | Study 1 (Lucid, Feb 2022; 5,284 analyzed across both modules). Citizenship Act: microtargeting 5.17 pp vs single-best-message 2.41 vs naive random message -0.96 (P=0.016 and P<0.001). Precision-weighted across both modules: 5.96 vs 3.48 vs 1.79. UBI module: microtargeting 7.16 vs single-best-message 5.03 vs naive 5.70, not significant. Profiles were ranked with generalized random forests (grf R package) over 250 Monte Carlo 75/25 splits. The advantage was qualitatively largest for one-covariate targeting, though differences were slight. (pp.3-5 Study 1 Calibration and Experimental Phase; Fig 2 p.5; Materials and Methods pp.8-9) | Untapped potential ranks each person by (wp x (1-/2s-1/) x t + (1-wp) x s x (1-t)) x (1 + wi x i), with module defaults wp=0.5 and wi=0.5 that a district can override. It chooses whom to contact, not what to say. It has no message or script variant and no effect-size term, and the (1-/2s-1/) term peaks at s=0.5 by construction. `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/untapped.py:8-10` | Tappin's effects are online survey estimates from Feb 2022 U.S. respondents with forced exposure on low-salience policy attitudes. They are not field or door-canvass effects. The paper says candidate-centered and non-U.S. settings are untested, and it says survey estimates may overstate the relative advantage of microtargeting in the field. A party or ideology split is the coarse covariate the paper found sufficient in-sample, and Terrier holds registration and partisan scores. Any gain stays UNVERIFIED until Terrier runs its own pretest. | moderate | VERIFIED-SOFTENED |
| [[systems/terrier-canvass#Untapped potential\|Untapped potential]] | qualifies | The authors say persuasion returns may be highly context dependent. Study 1 Citizenship Act significant (5.17 vs 2.41, P=0.016); UBI not significant (7.16 vs 5.03, P=0.12). Study 2 across 36 issues: per-person issue targeting was directionally below one best-issue ad (12.30 vs 15.42, P=0.11). The authors say survey estimates may overstate the relative persuasive advantage of microtargeting. Candidate-centered and non-U.S. campaigns are untested. (p.4 Study 1 results; p.6 Study 2 results and Discussion; p.7 limitations) | The persuade term (1-/2s-1/) x t assumes that people near s=0.5 who will vote carry persuasion value, with no magnitude attached. Its balance weight wp=0.5 is a module default, not a ruling. The score therefore treats persuadability and reminder value as comparable units with no effect-size evidence behind either. `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/untapped.py:8-10` | The paper does not estimate effects by distance from s=0.5, so it neither supports nor refutes the peak assumption. It says effect sizes vary by issue and setting and are likely smaller in high-salience contexts with countermessaging. Treat wp as unanchored until Terrier has local contact results. | weak | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Persuadability table PERS\|Persuadability table PERS]] | context | In the Study 2 calibration (n=3,990 respondents, 36 issues, 48 progressive video ads), the messages predicted most persuasive were often on obscure, low-information policies such as net neutrality. The authors offer this as one possible explanation for the Study 2 divergence, alongside a mixed pretest design and a nearly one-year gap before follow-up. They expect smaller effects in high-salience settings with countermessaging. (p.8 Discussion; p.7 limitations) | PERS is a fixed table keyed only on stance tier, with an inverted-U shape: Highly Persuadable 0.65, Opponent 0.08, and no-position default 0.45 via the PERS_NONE environment dial. It has no issue-salience or information term. Salience enters separately through fec/engine/salience_shock.py. `fec/moneyball_core.py:565-568` | Weak. Tappin measures attitude change among public respondents after one video, while PERS models legislators under lobbying. The low-information point might carry over to members on low-visibility CU topics, but nothing here measures that. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: summary_md says message choice drives most of the persuasive return. The paper does not decompose the effect. In the UBI module the naive message (5.70) beat single-best-message (5.03), which the summary omits.
> - Summary issue: summary_md says pretested messages gave 2.4 to 5.2 pp. That range covers only the Citizenship Act module. Across both modules single-best-message ranged 2.41 to 5.03 and microtargeting 5.17 to 7.16.
> - Summary issue: summary_md and bearing 1 say grf causal forests. The paper says generalized random forests (grf R package). The word causal does not appear in the text.
> - Summary issue: summary_md says targeting on one covariate did as well as 2-3 or 4+ covariates. The paper says the one-covariate advantage was qualitatively largest, with relatively slight differences.
> - Summary issue: summary_md says coarse partisan targeting is enough to capture the gain. This is the paper's in-sample claim (p.8 Discussion) and needs the Study 1 and in-sample qualifier.
> - Summary issue: summary_md says both SBM and microtargeting beat naive in Study 2 (6.19). The text confirms only microtargeting vs naive (12.30 vs 6.19, P=0.008). The SBM vs naive comparison appears only in Fig 3, which the text dump does not label, so it is unverified.
> - Summary issue: Bearing 1 system_side says three 0.5 weights. The code has two module defaults, wp and wi (DEFAULT_WEIGHTS = (0.5, 0.5)), each overridable per district.
> - Summary issue: Bearing 2 paper_claim states persuasion returns were highly context dependent as fact. The paper hedges with may be. It also says survey estimates may overstate the relative advantage, not field persuasion.
> - Summary issue: Bearing 3 paper_claim presents the low-information explanation as a finding. The paper lists it as one hypothesis among several (p.8).
> - Summary issue: Bearing 3 code_ref 564-568 includes a comment line. The PERS table is at 565-568.
> - Summary issue: paul_alignment says Paul's recorded position (component_literature) and that Untapped went live 2026-10-09 with no content term. Neither was verified here: the brain was not read because boot was skipped, and paul_entries is empty. The untapped.py header is dated 2026-10-09, which is consistent but not proof.
> - Summary issue: Related links: the item records for Simchon, Kalla, Broockman and Palm exist and their titles match the why text. Only Simchon's item summary was read. The other three were checked by title and metadata, not full text, so their why claims are unverified beyond that.

## Gaps and flags
- Terrier has no message or script dimension. Untapped potential ranks whom to contact but carries no estimate of how much a contact moves anyone, so the persuade-vs-remind weight wp=0.5 is unanchored.
- The Terrier assumption that persuasion value peaks at s=0.5 has no local or cited empirical test. Tappin does not report effects by distance from s=0.5.
- No pretest or heterogeneous-effect estimate (e.g. causal forest over canvass outcomes) exists anywhere in Terrier or Melange. Nothing could learn which covariate profiles respond to which content.
- Melange PERS has no issue-salience or information term. The low-information persuadability point is untested for legislators.
- No conflict with brain rules. The paper does not argue for dropping cosponsorship edges or for per-edge volume normalization, does not use LDA, and makes no reciprocity claim.
- Every effect size here is a survey estimate from Feb 2022 Lucid respondents with forced exposure. Quote them with that window and population, never as field or door-canvass effects.
- The Study 1 headline advantage (about 70 percent over SBM) is pooled over two modules and driven by the Citizenship Act module. Quote it with the per-module split, or label it UNSTRATIFIED.

Paul has no annotated entry for this work. The paper bears on the missing content term in Untapped potential only as survey evidence: naive random messages were near zero in the Citizenship Act module while pretested messages gave 2.41 to 5.17 pp there, but the paper does not decompose the effect and in the UBI module naive (5.70) beat single-best-message (5.03).

## Related papers
- [[simchon-2024-persuasive-effects-political]] — extends — Simchon et al test matching ad style to reader openness and whether LLMs can mass-produce matched ads, which fits the generative-AI extension. Tappin supplies the pretest-and-target baseline. The one-covariate finding is Tappin's own result, not Simchon's.
- [[kalla-2023-which-narrative-strategies]] — complements — Field and survey evidence on which narrative strategies durably persuade, from the canvass side. It bears on the missing content term in Terrier's Untapped score alongside Tappin's survey message-selection result.
- [[broockman-2026-should-moving-middle]] — complements — Broockman and Kalla argue that returns to moderation depend on where voters are, which echoes Tappin's context-dependent results. Their working paper covers candidate races, which Tappin did not test.
- [[palm-2025-influence-personality-traits]] — complements — Palm and Tims review how personality traits moderate persuasion strategies. Tappin's Study 1 moral-foundation profiles did not beat one-covariate targeting, which is a check on rich psychological tailoring.

### Linked from
- [[kalla-2023-which-narrative-strategies]] — complements — It quantifies the persuasive returns to targeting messages by audience. This paper shows message content (perspective-getting) carries the effect. Together they separate who-to-target from what-to-say, the split Terrier's potential formula collapses into target selection.
- [[cagala-2021-optimal-targeting-fundraising]] — complements — Item file title matches Tappin et al. 2023 on potential persuasive returns to political microtargeting; it addresses the same benchmark-relative question in persuasion.
- [[celli-nd-twenty-years-personality]] — complements — Existence confirmed. The persuasion threat in Section 4.2 is consistent with it; the survey does not cite it.
- [[madsen-2017-strategic-advantages-micro]] — complements — Tappin measures persuasive returns to microtargeting with experiments; Madsen and Pilditch only simulate them. Tappin is the empirical check on this paper's claim that targeting has an advantage.
- [[sverdrup-nd-introduction-grf-causal-forests]] — complements — Vault item exists with a tappin-2023 ext and rel file. Rel describes it as measuring how persuasion effects vary across audiences. Kept on existence; content not re-read here.