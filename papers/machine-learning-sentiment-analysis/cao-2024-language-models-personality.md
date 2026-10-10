---
type: paper
title: "Large language models know how the personality of public figures is perceived by the general public"
authors: ["Xubo Cao", "Michal Kosinski"]
year: 2024
venue: "Scientific Reports 14:6735"
doi: "10.1038/s41598-024-57271-z"
category: "Machine Learning & Sentiment Analysis"
systems: ["melange-psychometrics"]
components: ["melange-psychometrics.llm-and-hybrid-readers", "melange-psychometrics.llm-and-hybrid-readers", "melange-psychometrics.llm-and-hybrid-readers", "melange-psychometrics.gold-panel-validation"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/cao-2024-language-models-personality.pdf]]"
original_paths: []
sha12: ["31ea20118bb8"]
tags: [paper, machine-learning-sentiment-analysis, type/paper, theme/machine-learning-sentiment-analysis, evidence/verified]
---

# Large language models know how the personality of public figures is perceived by the general public

**Xubo Cao, Michal Kosinski (2024).** *Scientific Reports 14:6735.* [10.1038/s41598-024-57271-z](https://doi.org/10.1038/s41598-024-57271-z) Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Cao and Kosinski (Scientific Reports 14:6735, 2024) fit ridge regressions from 12,288-dimensional GPT-3 embeddings of public figures' NAMES to mean human ratings of perceived Big Five traits and likability (N = 226 Pantheon figures from 43 countries, 600 Prolific raters, about 19 ratings per figure, TIPI items). With 20-fold cross-validation, predictions correlate with aggregate perceptions at r = .78 to .88 after attenuation correction (raw .70 to .80). Controlling for likability and demographics (birth year, gender), accuracy falls to corrected r = .53 to .70 (raw .43 to .61). Accuracy rises with popularity (r = .16 with log Wikipedia views). The design is predictive, not causal, and concerns PERCEIVED, not actual, personality. For Melange the paper is the cited motivation for de-identifying text before the Pink LLM trait reader: a model that knows a politician's name carries a reputational prior that can stand in for reading the text. The paper shows the prior exists for famous names; it does not test whether stripping names, titles, party and states from congressional text removes it, and the repo's --named contamination probe has no output artifact in data/members. The likability result bears on the halo questions: a large evaluative factor runs through perceptions of public figures, yet trait-specific signal survives its removal, consistent with the CT-CM design of one halo method factor plus traits, and a caution that perception-based gold panels are not halo-free criteria. The code cites the work as PNAS Nexus 2024 with r = .76 to .87; the vault copy is Scientific Reports with corrected r = .78 to .88, so that citation needs checking.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question.** Can the perceived personality of public figures (Big Five traits and likability) be predicted from the location of their names in GPT-3's semantic embedding space, and does this hold after controlling for likability and demographics?

**Theory.** Perceptions of others' personality are consequential and matter regardless of accuracy. Word embeddings act as a functional analogue of human semantic memory and encode word meanings, group stereotypes, and (per the authors) individual differences. The authors also invoke the personality halo effect, where likable people are perceived as having desirable personalities, as the reason likability and demographics explain part of the signal.

**Data.** Sample of 226 public figures (N = 226) drawn from the 300 most popular figures among 11,341 in the Pantheon 1.0 dataset, across 43 countries; born 1900 or later; artists capped at 100; popularity proxied by Wikipedia page views 2008-2013; 74 figures removed for fewer than 10 raters. Human ratings from 600 Prolific raters, each rating 10 random figures on likability (200-point scale) and Big Five traits via the 10-item TIPI; mean 18.89 ratings per figure (SD 10.38). Predictors are 12,288-dimensional GPT-3 embeddings of each name. Unit of analysis: public figure. Data and code on OSF (854w2).

**Method.** Ridge regression of mean human ratings on column-standardized GPT-3 name embeddings, with 20-fold cross-validation (each figure predicted by a model trained on all others) and alpha tuned in an inner 20-fold loop. Controlled models first regress each trait on likability plus demographics (birth year, gender) and then predict the residuals from embeddings. Correlations are corrected for attenuation by dividing by the square root of split-half reliability (0.79-0.88). Effect sizes converted to Cohen's d. Interpretation by projecting 525 person-descriptive adjectives onto the fitted regression lines.

**Identification.** None in the causal sense. The design is out-of-sample predictive validity (cross-validation) with face-validity checks (top and bottom figures, adjective extremes). No causal claim is made about embeddings producing perceptions.

**Scope.** Population: globally famous public figures in Pantheon 1.0 with sufficient rater coverage, born 1900 or later, from 43 countries. Era: the GPT-3 training corpus, whose date range the text does not state; the authors note predictions reflect only the training-data period. Not tested on private individuals or on people absent from the training data.

## Key findings

- Without controls, cross-validated predictions correlate with human ratings r = 0.78 (extraversion) to 0.88 (openness) after attenuation correction; raw r 0.70-0.80. Results p.2 and Figure 1 green bars.
- Translated to Cohen's d, the no-controls correlations give d = 2.49 to 3.75 (labelled huge effects). Results p.2.
- Controlling for likability and demographics, accuracy falls to r = 0.53 (emotional stability) to 0.70 (extraversion) after correction; raw r 0.43-0.61 (d = 1.25-1.96). Results p.4 and Figure 1 red bars.
- Benchmarks used by authors: no-controls accuracy compared with CT detection of head and neck metastases (r = 0.64) and ultrasound for peripheral artery disease (r = 0.83); controlled accuracy compared with dental X-ray cavity detection (r = 0.43) and ultrasound for deep venous thrombosis (r = 0.60). Results p.3-4.
- Self-reported Big Five from language is cited as about r = 0.40 for comparison. Results p.4.
- Embeddings predict aggregate human judgments better than individual judgments do: single-rater to aggregate correlation r = 0.56-0.66. Results p.3.
- Accuracy is higher for more popular figures: profile similarity correlates with log Wikipedia page views at r = 0.16. Results p.3.
- Face validity: top openness figures are mostly artists (Freddie Mercury, David Bowie, Michael Jackson, Lady Gaga, Jimi Hendrix, Quentin Tarantino); bottom openness includes Kim Jong-un, Kim Jong-il and conservative politicians. Results p.3-4, Table 1.
- Adjective check across 525 person-descriptive adjectives: 'quiet', 'lonely', 'depressed' sit at the low extraversion pole and 'entertaining', 'hilarious', 'lively' at the high pole; 'alcoholic' ranks 3rd (of 525) for low conscientiousness and 3rd for low emotional stability. Results p.5, Table 3.
- Birth year correlates with perceived extraversion: human ratings r = 0.35 and GPT-3 predictions r = 0.44 (text, p.4; Table 2). Women are perceived as more agreeable and are absent from the bottom of predicted agreeableness. Results p.4.
- Perceived personality tracks likability: many of the least likable figures (Lee Harvey Oswald, Charles Manson, both Kims) sit at undesirable extremes of several traits. Results p.4.
- Discussion argues that intimate traits of public figures can be extracted from widely available LLMs, framed as a privacy threat, and that the approach could extend to people absent from training data. Discussion p.6-7; the extension is not tested in the paper.

## Limitations

- Requires collecting human ratings to train the regression (authors suggest adjective-distance or LLM-description approaches as untested substitutes).
- Predictions reflect only the period in the training data, so changed public perceptions lag until retraining.
- Extension to people absent from the training data is untested.
- Sample is 226 public figures after dropping 74 with fewer than 10 raters and excluding those born before 1900; raters were Prolific workers recognising the names.
- Correlations concern perceived, not actual, personality.
- Demographic and likability controls are partial.
- Authors note Wikipedia-based popularity is a proxy.
- Supplementary materials hold the ICC and popularity details, not read here.

## Short quotes

- "We show that people's perceptions of public figures' personalities can be accurately predicted from their names' location in GPT-3's semantic space."
- "Even when controlling for demographics and likability, name embeddings allow for diagnosing public figures' perceived personalities."
- "Our models remained accurate even when controlling for public figures' demographics and overall likability."

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-psychometrics#LLM item-level reader (Pink), ridge hybrid and CT-CM latent model\|LLM item-level reader (Pink), ridge hybrid and CT-CM latent model]] | supports | Perceived Big Five of public figures is predictable from the name embedding alone: cross-validated r = .78 (extraversion) to .88 (openness) after attenuation correction, raw .70-.80, N = 226 figures, 20-fold CV ridge on GPT-3 name embeddings; accuracy higher for more popular figures (r = .16 with log Wikipedia page views). (Abstract; Results p.2-3 (Figure 1 green bars); Methods p.2 (CV, attenuation); Results p.3 (popularity r = .16)) | fec/members/big5_llm.py de-identifies name parts, titles, party words and 50 state names before item scoring (deidentify, lines 196-204). The module docstring (lines 11-14) cites Cao and Kosinski as the reason, but gives the venue as PNAS Nexus 2024 and r = .76-.87, which match neither the paper's venue nor its figures. `fec/members/big5_llm.py:11-14; fec/members/big5_llm.py:196-204` | The paper uses GPT-3 name embeddings and lay ratings of globally famous figures, not a chat model reading congressional text. Transfer to the Pink reader is by analogy and untested. | strong | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#LLM item-level reader (Pink), ridge hybrid and CT-CM latent model\|LLM item-level reader (Pink), ridge hybrid and CT-CM latent model]] | qualifies | The paper uses name embeddings only. It does not test whether removing names from surrounding text removes the signal. It notes predictions reflect only the period in the training data, and that extension to people absent from training data is untested. (Discussion p.6 (lines 624-635); the paper has no separate Limitations heading) | deidentify (big5_llm.py lines 196-204) blanks name parts longer than 2 characters, a fixed title list, party words and 50 state names, within 1,200-word chunks (CHUNK_WORDS = 1200 at line 51). Committee names, district cities and bill names are not removed. The --named probe (line 172, 30 members) is coded, but data/members holds only big5_llm_opus.json and big5_llm_sonnet.json plus caches, with no _named artifact. Name-part matching is case-insensitive, so common-word surnames also blank ordinary text. `fec/members/big5_llm.py:39-42; fec/members/big5_llm.py:51; fec/members/big5_llm.py:172; fec/members/big5_llm.py:196-204` | Matters only where Pink scores feed a validity claim. The de-identification rationale is the code docstring's, not the paper's; the paper discusses a privacy threat from extracting traits from data (Discussion p.6), not text de-identification. The rel's Pink display-only status and the +.064 diagonal figure were not found in the brain concepts folder and remain unverified. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#LLM item-level reader (Pink), ridge hybrid and CT-CM latent model\|LLM item-level reader (Pink), ridge hybrid and CT-CM latent model]] | qualifies | Controlling for likability plus birth year and gender lowers accuracy from corrected r = .78-.88 to r = .53-.70 (raw .43-.61). The paper attributes the likability-trait links to the personality halo effect and reports that the least likable figures sit at undesirable extremes of several traits. (Results p.4 (lines 280-294), Figure 1 red bars; Table 2 p.4; halo discussion p.4 and p.5-6) | big5_ctcm.py (lines 8-9) puts an orthogonal PINK METHOD factor over the five Pink indicators to absorb judge halo. big5_ctcm2.py (lines 3-8) repeats this with the embedding Blue as third indicator. The ct_p2 function in big5_improvements_eval.py (around line 223) removes the first principal component as the general evaluative factor. The paper contains no method factor and no single-factor model; the consistency with the CT-CM design is the analyst's inference. `fec/members/big5_ctcm.py:8-9; fec/members/big5_ctcm2.py:3-8; fec/members/big5_improvements_eval.py:223` | The paper's halo is in lay perceptions of famous people, with likability measured per figure. The rel says Melange has no per-member likability covariate; that was not checked here. The method factor is identified only by model structure, so the evidence bears on the halo's shape, not its size in congressional text. | weak | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Big Five validation against Senate gold panels and criteria\|Big Five validation against Senate gold panels and criteria]] | qualifies | Perceptions of public figures track likability and demographics: birth year with extraversion, human r = .35 and model r = .44; women are perceived as more agreeable; an individual rater agrees with the aggregate at r = .56-.66. (Results p.3 (lines 115-116); Results p.4 (lines 267-276, 272-273); Table 2 p.4) | The brain file (single-look-holdout-discipline-case-1-moneyball-gate2.md, line 35) states A-ES is about .44 population-real (RKH benchmark .443 House, .393 Senate) plus about .21 text-tone halo. That line does not mention gold panels, expert or insider ratings, k=1 insiders or rater independence. The code_ref big5_improvements_eval.py:10 is a --p1 docstring line and does not discuss gold panels. The RKH benchmark appears to be the Ramey, Klingler and Hollibaugh text measure, not a rater panel. `C:/Users/pgoff/Documents/brain/concepts/single-look-holdout-discipline-case-1-moneyball-gate2.md:35; fec/members/big5_improvements_eval.py:10` | The paper's raters are lay Prolific workers rating world figures they recognise, not Hill insiders. The halo's size in Hill expert panels is untested by this paper. The gold-panel component is affected only if its raters use reputation and likability. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Code citation mismatch: big5_llm.py line 12 cites Cao and Kosinski as PNAS Nexus 2024 with r = .76-.87. The paper is Scientific Reports 14:6735, with corrected r = .78-.88 and raw .70-.80. The summary flags this correctly; the code docstring needs fixing.
> - Summary issue: The summary says the paper is the cited motivation for de-identifying text. The paper does not discuss de-identifying text. The motivation comes from the code docstring; the paper's privacy argument concerns extracting traits from digital data (Discussion p.6).
> - Summary issue: The summary's statement that a large evaluative factor fits the CT-CM design is inference. The paper models no method or halo factor.
> - Summary issue: The summary's statement that the paper shows the prior lives in the name token overstates it. The paper shows name-embedding prediction of perceived traits, not that the signal is a reputational prior.
> - Summary issue: The summary's gold-panel link cites the brain file for k=1 insiders and independence, but line 35 of that file does not mention gold panels. The gold-panel link is unverified.

## Gaps and flags

- The --named contamination probe in fec/members/big5_llm.py has no output in data/members, so how much reputational prior survives de-identification is unmeasured on this corpus.
- No re-identification check: nothing tests whether an LLM can name the member from a de-identified 1,200-word chunk (committee, district cities, signature bills remain).
- The embedding Blue (fec/members/b5_emb_build.py: LSA word and char n-grams, MiniLM) is built on raw text with no de-identification; LSA has no outside prior, but the pretrained MiniLM basis could carry name or state identity into A/C/O.
- No likability or favorability covariate per member, so halo in Pink, persona and gold panels cannot be partialled out as the paper does.
- Code citation (PNAS Nexus 2024, r = .76-.87) does not match the vault copy (Scientific Reports 14:6735, corrected r = .78-.88); the follow-up ChatGPT study (paper ref 51) is not in the vault.

- Citation mismatch: fec/members/big5_llm.py:12 and the brain campaign record cite Cao and Kosinski as PNAS Nexus 2024 with r = .76-.87; the vault work is Scientific Reports 14:6735 (2024) with corrected r = .78-.88 (raw .70-.80). Verify which paper is meant before reusing the figure.
- The component assumption that de-identification removes reputational priors (attributed to Cao and Kosinski) overstates the source: the paper motivates de-identification but does not show it suffices. UNVERIFIED until the --named probe or a re-identification test runs.
- Paper correlations are corrected for attenuation; quote raw r (.70-.80; controlled .43-.61) beside corrected values, with the window N = 226 figures and mean 18.89 raters each.

No annotated-bibliography entry and no PDF markup from Paul for this work. The system's use of it (de-identify before LLM trait scoring) agrees with what the paper shows about a name-borne reputational prior, but the venue and numbers cited in code and brain do not match this copy, and the claim that de-identification removes the prior goes beyond the evidence; the named probe that would test it has no recorded result.

## Related papers

- [[mondak-nd-online-appendix-personality]] — complements — Expert evaluations of senators' traits (Personality on the Hill) are a perception measure of public figures, the construct Cao and Kosinski predict from names, so reputation and halo can enter both. Cao does not cite it; the link is the analyst's.
- [[birkenmaier-2025-measuring-politicians-public]] — complements — LLM prompting for politicians' agency and communion from text. The coarse 3-point, cannot-tell-as-zero design in big5_llm.py cites Birkenmaier and Lechner. Cao does not cite it; the link is by topic and by that code citation.
- [[ramey-2016-measuring-elite-personality]] — complements — Text-based measurement of elite Big Five from floor speech (Ramey, Klingler, Hollibaugh). Cao does not cite it. It marks the text route that a reputation prior must be kept separate from.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — Meta-analysis of LIWC-Big Five correlations, which are mostly small. Cao does not cite it, and it measures a different target (self and observer reports from text), so the rel's comparison of Cao's r with LIWC effects is not like for like.
- [[azucar-2017-predicting-personality-traits]] — complements — Meta-analysis of Big Five prediction from digital footprints. Cao's about r = .40 language benchmark is cited to Park et al. 2015 (ref 39), not to this meta-analysis, so the same-method label is not supported.
- [[celli-nd-twenty-years-personality]] — complements — Review of personality computing that lists data privacy among its threats. Cao's privacy framing (Discussion p.6) rests on Kosinski's work instead, so the two are complementary rather than sources for each other.
- [[amsalem-2018-effect-politicians-personality]] — complements — Politicians' Big Five personality predicts national media coverage. Cao does not test popularity against it. The perceived-personality-drives-visibility gloss is the analyst's; the excerpt read does not establish that the traits are perceived.

### Linked from
- [[peters-2024-language-models-infer]] — complements — Cao and Kosinski show that LLM-derived perceptions of public figures can be predicted from name embeddings alone (text check of the paper). That is the reputational-prior risk the de-identification step guards against, and it bears on how observer agreement is read.