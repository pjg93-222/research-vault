---
type: paper
title: "The persuasive effects of political microtargeting in the age of generative artificial intelligence"
authors:
  - "Almog Simchon"
  - "Matthew Edwards"
  - "Stephan Lewandowsky"
year: "2024"
venue: "PNAS Nexus, 2024, 3(2), pgae035 (Brief Report; advance access 29 January 2024)"
doi: "10.1093/pnasnexus/pgae035"
category: "Machine Learning & Sentiment Analysis"
systems:
  - "melange-influence-engine"
  - "melange-psychometrics"
  - "terrier-canvass"
components:
  - "melange-influence-engine.receptivity-layer"
  - "melange-psychometrics.lexical-big5"
  - "terrier-canvass.untapped-potential"
paul_annotated: false
paul_markup_count: 0
evidence_status: "unverified"
source: "[[_sources/simchon-2024-persuasive-effects-political.pdf]]"
original_paths:
  - "C:\\Users\\pgoff\\Downloads\\pgae035.pdf"
sha12:
  - "2c2abbe8bafc"
tags: [paper, machine-learning-sentiment-analysis]
---

# The persuasive effects of political microtargeting in the age of generative artificial intelligence

**Almog Simchon, Matthew Edwards, Stephan Lewandowsky (2024).** *PNAS Nexus, 2024, 3(2), pgae035 (Brief Report; advance access 29 January 2024).* [10.1093/pnasnexus/pgae035](https://doi.org/10.1093/pnasnexus/pgae035) Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Simchon, Edwards and Lewandowsky (PNAS Nexus 2024, brief report) test whether matching a political ad's style to a reader's openness to experience raises perceived persuasiveness, and whether LLMs can mass-produce such matched ads. Four UK Prolific survey experiments (n = 440, 804, 803, 804) score each ad with a previously validated reader-side openness model and regress 1-5 perceived persuasion on person openness, ad openness and the mismatch |z(O_person) - z(O_ad)| in a mixed model with random intercepts for ads and participants. Mismatch lowers ratings with real Facebook ads (b = -0.07 and -0.08 per SD of mismatch) and with ChatGPT rewrites (b = -0.03, p < 0.001); GPT-3 rewrites miss significance (b = -0.01, p = 0.057). The authors call the effects small and convert them, by simulation on a midpoint cutoff, into 2,490 extra persuaded per 100,000 (median mismatch to none). The outcome is self-reported perceived persuasion, not attitude change or behaviour; only one trait is tested; reported t degrees of freedom do not square with the sample sizes, and study 2a/2b sample sizes are swapped between text and figure. For Paul's systems the paper is mostly a design contrast. Melange's receptivity layer treats Big Five traits as person-level main effects on susceptibility and seed probability, with no message-by-trait term, while this paper locates the effect in the interaction, above and beyond person openness. That is a modelling gap, not a recommendation: the population is the UK public rating ads, not legislators meeting lobbyists, the effects are small, and Melange's text-derived openness is its weakest trait. The paper's openness score also measures the appeal of text to open readers, a different construct from the author-trait openness Melange extracts from floor speech.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Does personality-congruent political messaging (ads matched to a reader's openness to experience) increase perceived persuasiveness, and can generative AI automate producing and validating such personalized ads at scale?

**Theory.** Message tailoring and personality-matching persuasion (matching to audience characteristics; cites a systematic review with r = 0.17 and Hirsh et al. 2012 personalized persuasion). No formal theory beyond this; the Big Five openness trait is the test case, and the paper frames the risk as a manipulation machine exploiting vulnerabilities.

**Data.** Study 1a: n = 440 UK Prolific participants (mean age 39.45), each rating 10 ads (only 9 distinct ads due to a design error). Study 1b: n = 804 (mean age 40.58), 10 ads. Ad corpus: 1,552 real political ads published on Facebook to UK users Dec 2019 to Dec 2021; 10 selected for study 1. Study 2a: n = 803 in Results and 804 in Methods (GPT-3 generated ads, 10 ad pairs, mean age 41.76 per Methods). Study 2b: n = 804 in Results and 803 in Methods (ChatGPT gpt-3.5-turbo, Feb 2023 version, 10 ad pairs, mean age 40.59 per Methods). Unit: participant-by-ad perceived persuasion rating, 1-5. Study period: 2023 fielding; ads from 2019-2021.

**Method.** Linear mixed model predicting perceived persuasion from the participant's openness score, the ad's openness score and the matching score Matching_ij = |z(Openness_i) - z(Openness_j)|, with random intercepts for ads and participants. Study 2 generates low- and high-openness rewrites of each ad with GPT-3 or ChatGPT using a prompt containing the openness definition, an instruction and the original ad; openness of each ad scored by a previously validated language model (ref 13). Generated pairs with maximal openness difference selected, then run through the study 1 pipeline. Algorithmic validation via t-tests on generated ad openness scores; a smaller human-rater validation is reported in the supplement. Llama 2 70B replication of the algorithmic validation reported in the supplement.

**Identification.** Study 2 randomly assigns each participant to the low or high openness version of each generated ad, so ad version is randomized within participant. Study 1 uses existing real ads and self-reported openness with no manipulation of matching; the matching effect there is observational across existing participant and ad scores.

**Scope.** UK adults recruited via Prolific, mean age about 40, online survey, about 6 minutes; political-style Facebook-format ads; one trait (openness to experience) only; two LLMs (GPT-3 and gpt-3.5-turbo) with one generation per ad; 2023 data collection.

## Key findings

- Study 1a: larger personality mismatch lowers perceived persuasion (matching effect negative and significant), with real Facebook ads and 440 participants. (b = -0.07, 95% CI (-0.09, -0.05), t(26348) = -7.40, p < 0.001; Results, study 1, p.2)
- Study 1b: replication with 804 participants and the corrected 10-ad design shows the same negative matching effect. (b = -0.08, 95% CI (-0.10, -0.07), t(48173) = -12.87, p < 0.001; Results, study 1, p.2)
- Algorithmic validation: GPT-3 low- and high-openness rewrites differ in predicted openness in the expected direction, but the effect is small. (Study 2a: t(870) = 2.07, p = 0.039, Cohen's d = 0.07, 95% CI (0.00, 0.14); Results, study 2 validation, p.2)
- Algorithmic validation: ChatGPT rewrites differ in predicted openness in the expected direction, with a larger effect than GPT-3. (Study 2b: t(995) = 16.01, p < 0.001, Cohen's d = 0.51, 95% CI (0.44, 0.57); Results, study 2 validation, p.2)
- Study 2a (GPT-3-generated ads): matching effect narrowly fails conventional significance. (b = -0.01, 95% CI (-0.02, 0.00034), t(48173) = -1.91, p = 0.057; Results, study 2a, p.2-3)
- Study 2b (ChatGPT-generated ads): matching effect is significant; the authors say the more powerful model is what produced the significant effect. (b = -0.03, 95% CI (-0.04, -0.02), t(48233) = -4.25, p < 0.001; Results, study 2b, p.3)
- Authors state the effect sizes are rather small, and argue that small effects can become substantial at scale. (Effect sizes described qualitatively; no single headline effect size given in the Discussion beyond the coefficients above; Discussion, p.3)
- Simulation: out of 100,000 individuals exposed to personality-tailored messages, 2,490 would be persuaded (cutoff 3 on the 5-point scale), based on the median mismatch versus no mismatch change. (2,490 per 100,000 at median-to-no-mismatch change; extrapolated to 11,405 per 100,000 from worst to best match; Discussion, p.3)
- The authors report that the openness-scoring validation replicates with Meta's open-source Llama 2 70B, addressing the closed-source concern (supplementary material; not verifiable from the text file). (Qualitative: similar conclusions reported; no numbers in main text; Discussion, p.3)
- Generative tools have no built-in safeguard against microtargeted rephrasing, since it does not violate the OpenAI usage policy as the authors read it. (; Discussion, p.3)

## Limitations

- Authors acknowledge the demonstrated effect sizes are rather small.
- Study 1a used only nine distinct ads due to a design error; corrected in study 1b.
- Study 2b was not preregistered; study 2a and study 1a were preregistered, but the preregistered binary ad classification was replaced by a continuous openness measure.
- The GPT-3 matching effect (study 2a) did not reach conventional significance (p = 0.057).
- Only one generation per ad was used; the authors acknowledge stochastic LLM outputs may vary, and argue the variation is likely inconsequential.
- Perceived persuasiveness is self-reported rather than behavioral (no click-through or vote outcomes); the authors justify this as attitudinal endorsement.
- Only the openness trait was tested; the authors note other traits and moral reframing may be effective but did not test them here.
- ChatGPT model is closed source; the Llama 2 70B replication is only for algorithmic validation, and the human-rater validation is only in the supplement.
- Sample sizes for studies 2a and 2b are inconsistent between Results and Methods and the Figure 1 caption.
- Reported t degrees of freedom exceed the number of ratings implied by the sample sizes, so the unit of analysis is unclear from the text.
- Participants are UK Prolific respondents, so generalizability to voters and to other countries or platforms is untested.
- Simulated persuasion counts are extrapolations from a 5-point self-report cutoff, not observed behavior.

## Short quotes

- "personalized political ads tailored to individuals personalities are more effective than nonpersonalized ads" — Abstract, p.1
- "the larger the deviance from matching, the less persuasive the message was rated" — Results, study 1, p.2
- "small effect sizes can turn substantial at scale" — Discussion, p.3

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Receptivity layer (susceptibility, incentive, prospect domain, seed probability)\|Receptivity layer (susceptibility, incentive, prospect domain, seed probability)]] | qualifies | Persuasion depends on the fit between message style and recipient openness, above and beyond the recipient's own openness: mismatch coefficient b = -0.07 (study 1a, n = 440) and -0.08 (study 1b, n = 804) per SD of \|z(O_person) - z(O_ad)\| on a 1-5 perceived-persuasion scale, b = -0.03 for ChatGPT-tailored ads (n = 804), with person and ad openness held as separate predictors. (Results, study 1 and study 2b, p.2-3; Fig. 1) | The receptivity layer maps traits to person-level multipliers only: susc_trait = .40A + .30N - .20C - .10E, susc_mult and edge_mult as affine gains of that one scalar, and on the lexical basis seed_prob = clip(0.60 + .10A + .06O + .04E, 0.30, 0.90). The default basis is behavioral (RECEPT_TRAITS defaults to behavioral), so lexical O does not enter seed_prob at all by default. No term varies with the content or framing of the ask; receptivity is message-invariant. `fec/members/receptivity_layer.py:65; fec/members/receptivity_layer.py:189; fec/members/receptivity_layer.py:210-216` | Holds only if trait-message fit operates for legislators receiving lobbying asks as it does for UK Prolific adults rating Facebook ads; the outcome is self-reported perceived persuasion, not a vote or cosponsorship; effects are small (simulated 2,490 per 100,000 at a midpoint cutoff). Any matching term would need a message-side trait score Melange does not compute, and would stay UNVERIFIED until tested against the prospective H1/H2 contract. | weak | UNVERIFIED |
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | qualifies | The ad openness score comes from a language model trained to predict how attractive a text is to readers high in openness (Simchon et al. 2023, ref 13), not who wrote it; prompted ChatGPT rewrites shift that score by Cohen's d = 0.51 and GPT-3 rewrites by only d = 0.07, so the style can be produced on demand. (Results, study 1 and study 2 validation, p.2) | Melange scores author openness from floor speech and interview text with a closed-vocabulary linear map (O weights sixltr .30, insight .25, article .20, i -.15, certain -.10), refit to RKH targets. Held-out CV vs RKH for O is .214, the weakest of the five traits, and O is checked against an expected negative party sign. `fec/members/big5_traits.py:184; fec/members/big5_traits.py:276` | The paper shows openness-styled text is easy to generate on demand (d = 0.51 with a simple prompt), which supports the worry that legislators' public text reflects staff and communications strategy rather than author trait; it does not validate author-side lexical O in either direction. Applies to Melange only as a construct-validity caution for O on prepared speech. | weak | UNVERIFIED |
| [[systems/terrier-canvass#Untapped potential\|Untapped potential]] | context | Personality-matched messaging produces small attitudinal increments (b = -0.03 to -0.08 per SD of mismatch on a 1-5 self-report), which the authors translate to 2,490 to 11,405 extra persuaded per 100,000 under a midpoint cutoff, with no behavioural outcome. (Discussion, p.3) | Untapped potential values persuasion as (1 - \|2s - 1\|) x t, a function of the voter's support position and turnout only; it is message-invariant and uses no personality data. `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography-v3/server/research/untapped.py:9-10` | Only relevant if Terrier ever tailors canvass scripts by inferred voter traits; the paper's effects are on perceived persuasion of ads in a UK online panel, not doorstep vote choice, so it does not argue for changing the potential formula. | weak | UNVERIFIED |

## Gaps and flags

- No message-side model anywhere in the receptivity layer: susceptibility and seed probability are person-level only, so the system cannot represent the trait-by-message fit this paper identifies as the active ingredient.
- Under the default behavioral basis, text-derived openness does not enter seed_prob, so any openness-based tailoring idea would have no live trait input to attach to.
- Melange's author-side lexical openness (CV .214 vs RKH) is not distinguished from reader-appeal openness of text, a construct the paper's scoring model measures; prepared floor text may carry staff-crafted appeal style rather than member trait.
- Paper's own reporting inconsistencies: t degrees of freedom (26,348 for 440 participants x 9 ads; 48,173 for 804 x 10) exceed the rating counts implied by the samples, and study 2a/2b sample sizes are swapped between Methods and the Fig. 1 caption; quote the coefficients with this caveat.
- Outcome is self-reported perceived persuasion; the 2,490 per 100,000 figure is a simulated extrapolation on a midpoint cutoff (window: 2023 fielding, UK Prolific adults; denominator: 100,000 simulated exposures), not observed persuasion.
- Any matching term added to the receptivity layer would be new analysis and stays UNVERIFIED until a second path reproduces it (DE-062); it would also be a rank-moving recalibration that is Paul's call.
- No conflict with the barred SNA proposals, D-021 or D-025: the paper makes no network, edge-weight or LDA claims.

Paul has no annotated-bibliography entry and no markup for this work (0 annotations), so there is no takeaway of his to test. His receptivity-layer position (traits adjust response to pressure) treats traits as person-level modifiers, as the code does. This paper qualifies that: its trait effect lives in message-recipient fit. It does not justify a matching term: public ad ratings, small effects, and Melange's weakest trait (O).

## Related papers

- [[tappin-2023-quantifying-potential-persuasive]] — cites — Cited as ref 10 for microtargeting working even on a single attribute; Tappin measures persuasive returns to targeting with real attitude outcomes, a stronger test than this paper's perceived-persuasion ratings.
- [[palm-2025-influence-personality-traits]] — complements — A systematic review of personality as a moderator of persuasion strategies; it places this paper's openness-matching result within the wider trait-by-message literature.
- [[madsen-2017-strategic-advantages-micro]] — complements — An agent-based model of why microtargeted campaigning pays at scale; it is the theoretical counterpart to this paper's argument that small per-person effects add up across 100,000 exposures.
- [[byrne-2014-chokes-under-pressure]] — complements — Byrne supplies the person-level trait-to-susceptibility composite in Melange's receptivity layer; Simchon shows a message-by-trait interaction that a main-effects composite cannot capture.
- [[azucar-2017-predicting-personality-traits]] — complements — A meta-analysis of inferring Big Five traits from digital footprints, the inference step this paper's manipulation machine assumes on the recipient side.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — A meta-analysis of LIWC and Big Five links that bounds how much openness signal closed-vocabulary text carries, relevant to both this paper's text scoring and Melange's lexical O.

### Linked from
- [[broockman-2026-should-moving-middle]] — complements — It tests microtargeted persuasion with generative AI. This paper explains why tailoring by issue should matter: cross-pressured voters respond with opposite signs on different issues.
- [[kalla-2023-which-narrative-strategies]] — complements — A microtargeting persuasion study. It is a contrast to interpersonal narrative persuasion when deciding whether Terrier's persuade term should model the message or only the target.
- [[marrero-2022-evaluating-voice-samples]] — complements — Cited with Marrero in Addendum 1 as the payoff argument, conditioned on the voice pilot; the framing is the audio prereg's, not the paper's.
- [[cagala-2021-optimal-targeting-fundraising]] — complements — Item file title matches Simchon et al. 2024 on persuasive effects of political microtargeting; it parallels the policy-tree vs all-gift comparison in persuasion.
- [[celli-nd-twenty-years-personality]] — complements — Existence confirmed. Not cited in the survey; complement claim unverified.