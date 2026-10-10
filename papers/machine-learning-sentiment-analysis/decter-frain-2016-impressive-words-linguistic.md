---
type: paper
title: "Impressive Words: Linguistic Predictors of Public Approval of the U.S. Congress"
authors: ["Ari Decter-Frain", "Jeremy A. Frimer"]
year: 2016
venue: "Frontiers in Psychology 7:240"
doi: "10.3389/fpsyg.2016.00240"
category: "Machine Learning & Sentiment Analysis"
systems: ["melange-psychometrics"]
components: ["melange-psychometrics.persona-agency-communion", "melange-psychometrics.persona-agency-communion", "melange-psychometrics.lexical-big5"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/decter-frain-2016-impressive-words-linguistic.pdf]]"
original_paths: []
sha12: ["fd2288224c1a"]
tags: [paper, machine-learning-sentiment-analysis, type/paper, theme/machine-learning-sentiment-analysis, evidence/verified]
---

# Impressive Words: Linguistic Predictors of Public Approval of the U.S. Congress

**Ari Decter-Frain, Jeremy A. Frimer (2016).** *Frontiers in Psychology 7:240.* [10.3389/fpsyg.2016.00240](https://doi.org/10.3389/fpsyg.2016.00240) Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Decter-Frain and Frimer (Frontiers in Psychology 2016) ask whether the language spoken on the House floor tracks public approval of Congress. They built a monthly series from January 1996 to November 2014 (N = 206 months, about 124 million words) and scored it with LIWC 2007. Agency and communion indexes come from five coders who weighted each category (ICC 0.82 and 0.90). A separate 127-word prosocial (helping) dictionary is scored alongside them. The outcome is monthly Gallup approval. Both broad indexes correlate negatively with approval: agency r = -0.16 and communion r = -0.36. With controls, agency stays negative (beta -0.11) and communion is null. Prosocial language correlates r = +0.55 and survives the controls (beta +0.14). Media tone partly mediates all three. An exploratory stepwise pass keeps 8 of 63 categories, and the authors call these only suggestive. Moderation by the gender and party make-up of the chamber flips some signs. The design is correlational, aggregate and chamber-level, and the authors rule out causal claims. For Paul's systems the sharpest point is about construct definition. Communion is not one thing. The helping facet and the broader communion or belonging facet behave in opposite directions against an audience-perception outcome. Melange's persona channel uses one communion share from an LLM cue codebook (empathy and compassion), and that share feeds receptivity at PERSONA_W 0.30. That codebook already sits close to the helping facet, which is reassuring. But the paper's outcome is public approval of the institution, not a member's openness to lobbying, so it neither validates nor undercuts the susceptibility mapping. It also offers a cheap dictionary-based second path for checking the LLM cue shares.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Which demeanor makes the best impression in a professional setting: agentic (competent) language, communal (warm) language, or a specific linguistic category? Tested by predicting monthly public approval of the U.S. Congress from the words used in House floor debates, first theory-driven, then exploratory.

**Theory.** Agency and communion as the two primary dimensions of social cognition (Bakan 1966; Fiske et al 2002; Cuddy et al 2008). Communion is split into belonging (group intimacy) and prosociality (helping others), following Frimer et al 2011. H1: agentic and communal language both positively predict approval. H2: the prosocial facet, not communion broadly, predicts approval. H3a: communal language is a stronger predictor than agentic. H3b: agentic is stronger than communal. Exploratory: all 63 LIWC categories, to find blind spots in agency/communion theory.

**Data.** House floor-debate transcripts, Jan 1996 to Nov 2014 (Congressional Record via GPO API; extension of remarks excluded); 123,781,226 words. 22 transcripts under 5,000 words dropped; 206 monthly transcripts analysed (N = 206 months). Outcome: monthly mean of Gallup approval polls (198 of 227 months polled; gaps interpolated; mean 33%, SD 15%). Controls: President news conferences (411; 2,205,168 words); BLS unemployment; Michigan consumer sentiment; partisan vote share (mean 43%); bills passed; vetoes; female and Democratic member shares; media tone (Frimer et al 2015).

**Method.** Word density per LIWC 2007 category (63) plus a 127-word prosocial dictionary. Five blind coders scored each category +1, 0 or -1 for agency and communion (ICC communion 0.90, agency 0.82). Indexes = z-scored densities times weights, summed. Pearson correlations; OLS Models 1 to 3 (Model 3 adds controls); z tests comparing correlations; Hayes bootstrapped mediation via media tone; exploratory stepwise regression over 63 categories (entry p < 0.005, removal p > 0.01).

**Identification.** None. The authors state the design is correlational and cannot show that congressional language influences approval; reverse causation and third variables are acknowledged. The only strategy is adding exogenous and endogenous covariates to Model 3 (and a 6-month lag found in Frimer et al 2015, cited but not modelled here). The mediation test checks indirect paths via media tone, not causal identification.

**Scope.** Only the U.S. House of Representatives floor speech (not the Senate, committees or extension of remarks); public approval of Congress as a whole (not individual members); monthly aggregates over 1996 to 2014.

## Key findings

- Simple correlations with approval are negative for both agency (r = -0.16, p < 0.05) and communion (r = -0.36, p < 0.001). Hypothesis 1 not supported. Table 2, p.7.
- With all controls (Model 3), agency is a significant negative predictor (beta -0.11, p < 0.05) and communion is not (beta +0.04). Table 2, p.7.
- Prosocial language correlates r = +0.55 (p < 0.001) with approval and stays a positive predictor with controls (beta +0.14, p < 0.05). Table 3, p.8. Supports Hypothesis 2.
- Prosocial and agency correlations differ significantly (z = 7.89, p < 0.001); communion and agency also differ (z = 2.18, p = 0.03), but both are negative. The authors read this as support for Hypothesis 3b. Results, p.8.
- Media tone carries indirect effects: agency B = -0.11 [-0.30, -0.02], communion -0.06 [-0.16, -0.01], prosocial +1.82 [0.17, 5.26]. Direct effects of all three are also significant. Table 4, p.8.
- Moderation (text only; numbers are in the Supplement): agency predicts disapproval only when Congress is male-dominated; communal language is approved when Congress is male-dominated and disapproved otherwise; prosocial effect is stronger with fewer men and more Republicans. Results, p.8. The authors call these results mixed.
- Exploratory stepwise regression keeps 8 of 63 LIWC categories. Positive: tentativeness beta 0.46, prepositions 0.41, humans 0.25, anxiety 0.24, positive emotion 0.24, numbers 0.15. Negative: second-person pronouns -0.18, conjunctions -0.26 (p < 0.005 or better). Table 5, p.8.
- Media tone mediates three of the eight: prepositions indirect B = 1.90 [0.30, 5.02], humans 2.40 [0.45, 6.03], conjunctions -3.51 [-8.77, -0.45]. Not significant for tentativeness, anxiety, positive emotion, numbers or second person. Table 6, pp.9-10.
- Human words correlate with prosocial words r = 0.519 (p < 0.001). Conjunction density correlates with sentence length (r = 0.24) and negatively with six-letter word density (r = -0.38). p.11.
- Headline claim: prosocial (helping) language predicts approval more strongly than agentic language, which the authors read as warmth outweighing competence even in a professional setting. Abstract; Discussion, p.9.

## Limitations

- Correlational design: no causal inference. Reverse causation is raised (for example members bonding more when approval falls). Discussion, pp.9-10.
- Third variables: only the listed controls are used. Political orientation, public age and gender are not controlled, and unidentified variables may suppress an agency effect. p.10.
- Approval is dichotomous (approve or disapprove), which may miss variation. p.3.
- Gallup coverage is 198 of 227 months; the rest are linearly interpolated. p.3.
- Agency and communion weights come from five coders at category level; robustness is claimed only in the Supplement. p.3.
- No belonging dictionary was built, so the belonging-versus-helping split is tested only with Hart et al 2011 dictionaries, in the Supplement. p.10.
- The 63-category stepwise analysis is exploratory with no out-of-sample test; the authors say each result is merely suggestive until replicated. p.10.
- Conjunction and sentence-complexity results are described as inconclusive. p.11.
- Count inconsistency: 22 excluded plus 206 retained is 228 transcripts, but the text says 227 months. Flagged, not resolved. pp.2-3.
- Only the House is analysed; no Senate data; the window ends in November 2014.
- Hypothesis 3 comparisons involve two negative correlations, which the authors acknowledge complicates the reading. p.8.

## Short quotes

- "We found that neither agentic nor communal language positively predicted public approval."
- "To be clear, our approach is correlational, precluding the possibility of causal inference."
- "The American public approved of Congress more when its members spoke about helping, but not when they spoke about belonging."

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-psychometrics#Agency-communion persona channel\|Agency-communion persona channel]] | qualifies | Broad communion correlates r = -0.36 with monthly Gallup approval (Model 3 beta +0.04, n.s.) while the prosocial helping facet correlates r = +0.55 (Model 3 beta +0.14, p < 0.05). Agency r = -0.16 and Model 3 beta -0.11 (p < 0.05). Window Jan 1996 to Nov 2014, N = 206 House months. Pooled over the whole chamber, not stratified. (Tables 2-3, pp.7-8; Discussion pp.9-10) | The persona channel forms a z blend of two LLM cue shares from agency_communion_llm.py: persona z = 0.60 z(communion) - 0.40 z(agency). It is blended into susceptibility at PERSONA_W 0.30 for members with at least 15 classified sentences (510 of the 526 rows in agency_communion.json meet this). The communion share uses the Empathy and Compassion codebook: STRONG cues name concrete disadvantaged people, while WEAK cues include empathic focus on communities and states. Group-intimacy or belonging talk has no dedicated cue. `fec/members/agency_communion_llm.py:35-60; fec/members/receptivity_layer.py:139-156` | The paper is about an aggregate chamber-month audience outcome (approval of Congress), not member-level susceptibility to lobbyists. The facet lesson transfers only as a construct-definition check: a pooled communion score can mix facets with opposite signs. Ecological inference from chamber months to individual members is not licensed. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Agency-communion persona channel\|Agency-communion persona channel]] | suggests-extension | Agency and communion indexes are built from coder-weighted LIWC category densities: five blind coders, ICC agency 0.82 and communion 0.90, averaged into category weights (weights independent, r = -0.03). A 127-word prosocial dictionary is scored alongside. Robustness to the coding is reported only in the Supplement. (Method pp.2-4, Table 1; Limitations p.3) | The persona shares come from one instrument: Claude few-shot cue classification of self-referential floor and hearing sentences, cached per model. agency_communion_llm.py codes no second instrument for the same shares, so a dictionary path would have to be added as a second path. `fec/members/agency_communion_llm.py:3-12,123-140,239` | A dictionary path would be a convergence check (DE-062 second path), not a validity test. The coder weights were set on chamber aggregates, so member-level densities on short self-referential samples would be noisy. The paper's ICCs describe agreement on category weights, not on member-level scores. | weak | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | qualifies | Chamber-level LIWC densities in House floor speech co-move with monthly approval. Exploratory stepwise standardized betas, UNSTRATIFIED: tentativeness +0.46, prepositions +0.41, humans +0.25, anxiety +0.24, positive emotion +0.24, numbers +0.15; second person -0.18, conjunctions -0.26 (8 of 63 categories kept, N = 206 months). (Table 5, p.8; discussion pp.10-11) | big5_traits.py standardizes per-1000-word category rates (posemo and tentat are among the CAT categories) within the corpus. The corpus pools each member's social posts with floor speech and interview text (CREC and INTERVIEW sources; CHRG excluded by default), not floor speech alone. BIG5_BEFORE and BIG5_AFTER are validation hooks that default to off, so no period slicing or de-trending applies by default. `fec/members/big5_traits.py:33-35,64,84,94,220` | Inference, not tested: if category rates drift by period at the chamber level, members whose speech is concentrated in different years could pick up period variance in their trait z. The paper shows co-movement, not the size of the drift. A per-year de-meaning test on Paul's corpus would check it. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary says moderation flips some signs. The paper shows the communal effect reverses sign by chamber make-up, agency goes from negative to null when Congress is less male-dominated, and the prosocial effect changes size but not sign.
> - Summary issue: Summary says the persona channel uses one communion share. It blends communion (weight 0.60) and agency (weight 0.40) z-scores.
> - Summary issue: Bearing 1 and the code cite 526 members with at least 15 sentences. 526 is the total row count; 510 meet the threshold.
> - Summary issue: Bearing 2 says the paper gives a stable chamber-level series. The paper reports correlations and places robustness checks only in the Supplement.
> - Summary issue: Conjunction correlations are reported as r(215) while N is 206 months. Ext and rel did not flag this df mismatch.
> - Summary issue: Count inconsistency confirmed: 22 excluded plus 206 retained is 228 transcripts against 227 months (pp.2-3). Rel's flag is correct.
> - Summary issue: Ext key finding 4 says the authors read the communion-versus-agency difference as support for H3b. The paper uses the prosocial-versus-agency comparison (z = 7.89) for H3b and says the negative communion comparison complicates the reading.
> - Summary issue: paul_alignment attributes a position to Paul (Birkenmaier and Lechner, the trait-to-node worry). There is no Paul entry or markup for this work and no source was checked, so treat it as unverified.
> - Summary issue: Bearing 1 and bearing 2 rely on brain rulings (failed panel of 3 or more raters; functional-gate and party-signature records). Those were not checked here.
> - Summary issue: The paper's discussion labels Table 5 betas as r (for tentativeness, r = 0.46). Quote Table 5 values as standardized betas, not correlations.

## Gaps and flags

- The persona channel has no facet split. Communion is one share, so helping or compassion talk and belonging or we-language cannot be separated, although the paper shows they can carry opposite signs against an audience outcome.
- There is no second instrument for the agency and communion shares. A coder-weighted LIWC index or prosocial dictionary path would supply a DE-062 reproduction route.
- Lexical Big Five rates are pooled across years with no period de-meaning by default. Whether chamber-level drift in categories like tentat and posemo leaks into member z is untested.

- Pooled-over-fault-line: the paper's own moderation (Supplement) shows communal language helps approval when the chamber is male-dominated and hurts otherwise, and the prosocial effect varies with party share. The pooled communion and prosocial coefficients must be quoted as UNSTRATIFIED.
- Do not read the negative agency and communion correlations as grounds to drop or reweight the persona channel. PERSONA_W 0.30 is an owner-ratified keep that reopens only on a failed panel of 3 or more raters to B&L standards. The paper's outcome (aggregate institutional approval) is also scope-mismatched to member susceptibility.
- The headline claim that warmth beats competence rests on comparing two negative correlations, which the authors concede complicates the reading. The design is correlational, with possible reverse causation.
- Count inconsistency in the source: 22 excluded plus 206 kept is 228 transcripts, against the 227 months stated.
- Any new analysis that applies this dictionary path to Paul's corpus stays UNVERIFIED until a second path reproduces it (DE-062).

Paul has no annotated-bibliography entry and no markup for this work, so there is no takeaway to check. His stated component position (from Birkenmaier and Lechner) stresses codebook-based cue design and worries about the step from trait to node. This paper supports that worry from another angle: how communion is defined, as helping versus belonging, changes the sign of its audience effect. Nothing here makes the current system stale, and the B&L communion codebook already leans toward the helping facet.

## Related papers

- [[birkenmaier-2025-measuring-politicians-public]] — complements — Same agency and communion constructs for politicians. Birkenmaier and Lechner use LLM cue prompting, the instrument Melange runs; this paper uses coder-weighted LIWC dictionaries. Together they give two measurement paths.
- [[ramey-2016-measuring-elite-personality]] — same-data — Both score Congressional Record House floor speech over 1996 to 2014 with closed-vocabulary categories. Ramey does it at member level for traits, and this paper does it at chamber-month level for approval. Verified in the Ramey item.
- [[pennebaker-2003-psychological-aspects-natural]] — same-method — LIWC word-category densities as psychological indicators are the measurement basis of this paper.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — Meta-analysis of how far LIWC categories carry Big Five signal. It bounds how much the coder-weighted LIWC agency and communion indexes can mean.
- [[lechner-2025-mapping-political-personality]] — complements — The PoliPers framework for politicians' agency and communion profiles is the source of the codebook Melange uses, per the agency_communion_llm.py docstring. This paper adds the belonging versus helping split.
- [[osnabrugge-2021-playing-gallery-emotive]] — complements — Also treats parliamentary rhetoric as aimed at an outside audience, but works on emotive language where this paper works on agentic and communal language.
- [[aroyehun-2026-politics-of-feelings]] — complements — Emotional expression in congressional speech linked to legislative effectiveness at member level; this paper links language to chamber-level approval.
- [[amsalem-2018-effect-politicians-personality]] — complements — Politician personality and media visibility relate to this paper's finding that media tone mediates the language-approval link.

### Linked from
_None._