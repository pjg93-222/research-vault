---
type: paper
title: "Sentiment analysis of political communication: combining a dictionary approach with crowdcoding"
authors: ["Martin Haselmayer", "Marcelo Jenny"]
year: "2017 (print, Qual Quant 51:2623-2646); published online 21 September 2016"
venue: "Quality & Quantity (Quality & Quantity 51, 2017)"
doi: "10.1007/s11135-016-0412-4"
category: "Machine Learning & Sentiment Analysis"
systems: ["melange-stance", "moneyball-rank-pillars"]
components: ["melange-stance.lsd-tone", "melange-stance.legacy-regex-scorer", "moneyball-rank-pillars.press-pillar"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/haselmayer-2017-sentiment-analysis-political.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\s11135-016-0412-4.pdf"]
sha12: ["e0ba4d46f68b", "3fdf2702887c"]
tags: [paper, machine-learning-sentiment-analysis]
---

# Sentiment analysis of political communication: combining a dictionary approach with crowdcoding

**Martin Haselmayer, Marcelo Jenny (2017 (print, Qual Quant 51:2623-2646); published online 21 September 2016).** *Quality & Quantity (Quality & Quantity 51, 2017).* [10.1007/s11135-016-0412-4](https://doi.org/10.1007/s11135-016-0412-4). Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Haselmayer and Jenny build a negativity dictionary for German-language Austrian political text. About 480 CrowdFlower coders rated 12,713 sentences on a 0-4 negativity scale, ten ratings per sentence, for about 2,000 EUR. Each word's score is the mean score of the sentences it appears in, which yields 5,001 lemmatized words. To score a new sentence, the method takes its single most negative dictionary word (the max rule). A negator drops the word that follows it, and an intensifier multiplies a word's score by 1.25.
>
> Validation on 200 sentences:
> - The averaged lay crowd agrees with expert coding at r = 0.82.
> - The dictionary agrees with experts at r = 0.65, matching 84% of sentences.
> - On 755 statements coded by a single expert, the dictionary gives r = 0.63 at 82% coverage.
> - Two translated general-purpose German dictionaries score r = 0.13 and 0.19. The authors read this as evidence that a customized dictionary is needed for their own domain and language.
> - Preprocessing and aggregation choices move r from 0.13 to 0.65.
> - In a regression on the 2013 campaign, dictionary scores keep the expert-coded signs but shrink the effects. The government-pair coefficient is -0.13 with the dictionary versus -0.40 with expert coding.
>
> The paper matters here in two places:
> 1. The live press intensity is stock English Lexicoder LSD2015, scored per release as |net| / words, where net flips negated hits rather than removing them. Its only recorded validation is a three-case smoke test, and it has never been compared with human-coded intensity. The paper's design would supply that test cheaply.
> 2. Paul's retired graded tone scorer cites this paper by name, but its code departs from the paper. It sums weighted cues across sentences where the paper takes the strongest cue, uses a two-token negator window, adds a downtoner, and uses about 45 hand-weighted patterns rather than crowd-derived word scores.
>
> The evidence is about Austrian German, covers negativity only, and rests on a corpus spanning 1995-2013: the 200-sentence validation sample pools four campaigns from 2002-2013, and only the two applications use the 2013 campaign. None of it validates the US congressional tone measures directly.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Can a negative sentiment dictionary for a language and domain of choice be built cheaply through crowdcoding, and does its word-based tonality score validly measure fine-grained negativity relative to expert and crowd coding? The authors illustrate use with (a) negative campaigning among Austrian parties in the 2013 national election and (b) media tonality toward parties and candidates.

**Theory:** Sentiment as gradual negativity or tonality (not binary polarity), with a bag-of-words assumption and only the neutral-to-negative range covered because of negativity asymmetry (Peeters 1971; Baumeister et al. 2001; Rozin and Royzman 2001). Negativity bias in news (Galtung and Ruge 1965). Negative campaigning intensity matters for voters (Mutz and Reeves 2005; Fridkin and Kenney 2011). Government and coalition-partner restraint in attacks (Walter and Van der Brug 2013; Dolezal et al. 2015); electoral losers attack more (Damore 2002).

**Data:** Unit of rating: sentences (coded by crowd coders); word-level scores are derived from sentences. Source corpus: about 470,000 sentences from Austrian party press releases, parliamentary debate minutes and media reports on election campaigns, 1995-2013 (Sec. 4.1). Seed-word prefilter reduced it to about 215,000 sentences; 13,000 were randomly drawn; 12,713 remained after removing duplicates and incomplete sentences (fn 5). Crowd sample: about 130,000 valid codings, 10 per sentence, from 480 coders on CrowdFlower (Austria/Germany only), cost about 2,000 EUR (about 2,200 USD; Table 7 sums to about 2,198 USD). Resulting dictionary: 5,001 unique word forms (Sec. 4.5). Validation sample: 200 sentences from party press releases of four national campaigns (2002-2013) plus media reports from the most recent campaign, coded by two expert authors and ten lay coders (Sec. 6). Extended validation: 755 negative party press release statements from the 2013 campaign, single expert coder (App. 3). Application 1: 1,958 party press releases from the final 6 weeks of the 2013 campaign, of which 755 are directed negative relations between two parties (612 with a tonality score per Table 2; 619 observations in Table 3). Application 2: 15,096 news reports from 15 Austrian dailies, final 8 weeks of the 2013 campaign, 439,954 sentences, about one in five referring to a party or top candidate.

**Method:** Five-step procedure (Sec. 4): (1) sample sentences containing seed negative words; (2) crowdcode each sentence on a 0-4 negativity scale (0 not negative to 4 very strongly negative) plus an "uncodable" option, with test questions (4 initial, 1 in 5 later; two adjacent categories accepted); coders below 75% test accuracy excluded; (3) sentence score = mean of 10 coder ratings; (4) word score = mean of sentence scores in which the word occurs; word forms lemmatized with TreeTagger and POS-tagged, with manual post-processing; (5) remove words under 3 characters, high-frequency function words, names of persons, parties and organizations (AUTNES list), positive words from existing lexicons, and words occurring in only one sentence. Scoring of new texts (Sec. 5): bag-of-words; sentence tonality = most negative dictionary word (max rule); negation word immediately before a dictionary word excludes that word (no polarity flip); intensifier before a word multiplies its score by 1.25, capped at the scale maximum. Validation (Sec. 6, App. 2-3): Pearson correlations with expert and crowd means, coverage, and comparison with two translated German dictionaries (SentiWS, Momtazi). Applications: OLS regression of press-release tonality with government/opposition and coalition-pair indicators, party fixed effects, poll-performance and days-to-election controls, standard errors clustered by party pair (Table 3); descriptive comparison of mean sentence tonality with and without an actor reference, by day (Sec. 7.2, Fig. 6).

**Identification:** None causal. Observational OLS with party fixed effects and standard errors clustered across party pairs; controls for net poll change and days to election. Robustness is expert-coded dependent variable (App. 3, Table 6).

**Scope:** Population: German-language political communication in Austria (party press releases, parliamentary debates, and 15 Austrian daily newspapers). Era: corpus 1995-2013; the two applications cover the 2013 national election campaign. Institutions: six parliamentary parties (SPO, OVP, FPO, Greens, BZO, Team Stronach). The procedure is presented as portable to other languages and domains, but no other language or domain is tested.

## Key findings

- Crowd sentence scores: 480 coders passed the tests with 92% mean accuracy (SD 0.07); about 130,000 valid codings, ten per sentence, for 12,713 sentences; the test-question design implies a 4% chance of passing by guessing. (480 coders; 92% (SD 0.07); ~130,000 codings; 12,713 sentences; ~2,000 EUR (~2,200 USD); Sec. 4.2, p. 2628 (fn 5-6); Table 7, p. 2640)
- Dictionary scope and size: the rated bag of words falls from about 40,000 initial words to about 5,000 after removing short, frequent, rare, positive, and named-entity words. (5,001 dictionary words; scores 0.06-3.8 on the 0-4 scale; mean 2.04 (SD 0.65); Sec. 4.5, p. 2630)
- Lay crowd aggregates replicate expert coding of the 200-sentence test sample, with a slight centrist bias. (Pearson r = 0.82 (aggregated expert vs aggregated crowd, n = 200); Sec. 6, p. 2631)
- Dictionary validity against expert coding: a moderate correlation at 84% coverage. Setting the 32 uncovered sentences to zero lowers the correlation. (r = 0.65, coverage 84% (n = 200); r = 0.6 after assigning zero to 32 uncovered sentences; Sec. 6, pp. 2631-2632)
- Replication on a larger sample: single expert coding of 755 statements gives similar coverage and correlation; the dictionary-based estimates are described as more conservative than manual-coding estimates. (n = 755; coverage 82%; r = 0.63; App. 3, p. 2639)
- Generic German dictionaries perform far worse on the same sample, which the authors take as evidence that a customized dictionary is needed for their own domain and language. (Momtazi: r = 0.13, coverage 46%; SentiWS: r = 0.19, coverage 31%; political dictionary: r = 0.65, coverage 84%; Table 1, p. 2633; Sec. 6, pp. 2630-2632)
- Preprocessing and aggregation choices matter: stemming alone gives the lowest correlation; the best combination uses lemmatization, POS tagging, stop-word and named-entity deletion, mean word scores, the max rule for sentences, and booster/negation handling. (Table 4 r: stemming 0.13; lemmatizing 0.34; stemming+POS/deletion 0.42; lemmatizing+POS/deletion 0.65 (coverage 84%). Table 5 best: mean word score, max sentence, booster/negation yes: r = 0.65 (n = 168); App. 1-2, Tables 4-5, p. 2638)
- Application 1, negative campaigning 2013: government parties are less negative than opposition parties, and government-coalition pairs are less negative than other pairs. Electoral losers attack more; no escalation toward the end of the campaign. (Government sender -0.10 (p < 0.05); government pair -0.13 (p < 0.001); electoral losses +0.12 (p < 0.001) in Model 1, not significant in Model 2 (0.10, SE 0.06); proximity -0.01 (n.s.); constant 2.28; n = 619; adjusted R2 = 0.03; Sec. 7.1, Table 3, Fig. 5, p. 2634-2635)
- Application 1 descriptive counts: the three largest parties issue most negative releases. Government parties SPO and OVP and opposition FPO together account for about 4 in 5 negative releases. (755 negative statements; FPO 232 (30.7%), OVP 199 (26.4%), SPO 164 (21.7%), BZO 68 (9.0%), Greens 59 (7.8%), Team Stronach 33 (4.4%); mean tonality 2.23; dictionary matched 82% of the 755 statements; Sec. 7.1, Table 2, p. 2634)
- Application 2, media tone: sentences that mention a party or candidate are more negative than sentences that do not. Coverage was slightly negative on average. (15,096 reports in 15 dailies; 439,954 sentences; about 1 in 5 references an actor; 55% contain a dictionary word (rest coded neutral); mean tonality 1.23 with actor reference vs 1.00 without; Sec. 7.2, Fig. 6, pp. 2635-2636)
- Appendix 3 check: the sign pattern of the Table 3 coefficients is the same whether scored with the dictionary or with expert coding, but dictionary-based effect sizes are weaker. (Dictionary vs expert, government-pair coefficient -0.13*** vs -0.40***; electoral losses 0.12*** vs 0.23***; App. 3, Table 6, p. 2639)

## Limitations

- Bag-of-words scoring cannot handle irony, sarcasm, rhetorical questions, or separable verbs; the authors say these are rare enough that coverage is acceptable, but do not measure their share.
- Validation rests on 200 sentences rated by two expert authors and ten lay coders, plus a single-coder extension of 755 statements, from one country and language.
- Mean aggregation of ordinal ratings produces a centrist bias and decimal scores; the authors acknowledge that purists may object.
- Lemmatization and POS tags needed manual cleaning; current German NLP tools were judged unsatisfactory for separable verbs, compounds, dialect and neologisms.
- Negation excludes the following word rather than flipping polarity; the procedure focuses on negativity only and does not model positive sentiment.
- Non-matching sentences in the media application (45% of sentences) are coded neutral on the assumption that the dictionary covers overtly negative statements; this is not validated for that subset beyond the 200-sentence test.
- Application 2 is explicitly illustrative; the authors state it cannot by itself show a critical or cynical journalistic perspective.
- Sample size and budget (about 2,000 EUR) were set by a budget limit, not a power calculation.
- Reporting inconsistency: Table 2 gives 612 statements with a tonality score, while its party counts sum to 619 and Table 3 reports 619 observations.
- Table 3 is text-extracted and its column layout is ambiguous, so the coefficient-to-model assignment above follows the text, not the table grid.
- The dictionary's generalization to other domains, languages, or time periods is not tested; the authors recommend custom dictionaries and cautious use of general-purpose ones.

## Short quotes

- "The crowdbased dictionary provides efficient and valid measurement of sentiment." (Abstract, p. 2623 (PDF p. 1))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-stance#Lexicoder LSD2015 tone intensity\|Lexicoder LSD2015 tone intensity]] | measurement-source | Validation on 200 sentences: aggregated lay crowd vs aggregated expert r = 0.82. Domain dictionary vs expert r = 0.65 at 84% coverage, about 0.6 when the 32 unmatched sentences are set to zero. Momtazi r = 0.13 (46% coverage) and SentiWS r = 0.19 (31% coverage). Authors state that dictionary analysis requires a customized dictionary and recommend caution with general-purpose dictionaries. (Sec. 6 pp. 2631-2632; Table 1 p. 2633; Conclusion p. 2637; App. 3 p. 2639) | press_value tone intensity is stock LSD2015, uncustomized. stage5_lsd.R lsd_tone (lines 48-61) removes negator-plus-word hits and flips them: negated negatives count as positive and negated positives as negative. lsd_net_norm = (pos - neg) / words. run_stage5.R line 141 takes abs(lsd_net_norm) as heat; line 143 sets press_value = final_sign x heat. The brain note at debate-stance-model.md line 102 records only a three-case smoke test (pro +4 / negated 0 / neutral 0) and calls LSD validated, (a functional check by the paper standard). `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_lsd.R:48-61; C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R:141-143; C:/Users/pgoff/Documents/brain/actors/debate-stance-model.md:102` | The validation protocol transfers: about 200 releases rated 0-4 by lay coders and experts, correlated with \|lsd_net_norm\|. The size of the generic-dictionary penalty does not transfer directly, because the paper's comparators are machine-translated German lexicons, not LSD2015. This paper does not establish that LSD2015 performs well on English news; the Young and Soroka citation is general sentiment work, not an LSD validation. The paper scores sentences for negativity only, while LSD here is bidirectional and scores whole release bodies. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-stance#Legacy CU press regex stance and graded tone scorer\|Legacy CU press regex stance and graded tone scorer]] | contradicts | Sentence tonality is set by the single most negative dictionary word (max rule). A negator immediately before a word excludes that word, and an intensifier multiplies it by 1.25. Word scores are crowd-derived means on a 0-4 scale. Mean word scores with the max rule and booster/negation handling give r = 0.65 (n = 168 test sentences). Other aggregation combinations give r from 0.38 to 0.58. (Sec. 5 pp. 2630-2631; App. 2 Table 5 p. 2638) | The graded layer header in cu_press_release_scraper.R cites Haselmayer and Jenny and says sentence tonality follows the strongest cue. The code does not: weighted_tone (about lines 2443-2475) sums every fired cue across all CU sentences into Wp and Wn, and graded_from_signed buckets \|Wp - Wn\| by GRADED_CUTS. The negator window covers two preceding tokens; a 0.75 downtoner is added (line 139), not in the paper. TONE_LEXICON has 44 hand-weighted entries (weights 1, 2, 3), not crowd-derived scores. run_stage5.R lines 146-160 reuse the magnitude only for display score_label; press_value does not use it. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R:121-132 (header), 138-139 (AMPLIFY_FACTOR, DOWNTONE_FACTOR), 2320-2360 (TONE_LEXICON), 2379 (graded_from_signed), 2443-2475 (weighted_tone), 2786-2803; C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R:146-160` | This is a fidelity finding, not an empirical one. It matters for the display score_label magnitude and the comparison column only, because press_value and the press pillar do not consume the graded layer. No external claim that these graded labels follow Haselmayer and Jenny should be made. | strong | VERIFIED |
| [[systems/moneyball-rank-pillars#Press pillar (own-domain, signed raw-max)\|Press pillar (own-domain, signed raw-max)]] | qualifies | Dictionary-based tonality runs in the same direction as expert coding but compresses effects. In App. 3 Table 6 the government-pair coefficient is -0.13 with the dictionary vs -0.40 with expert coding, and electoral losses are 0.12 vs 0.23. Authors call dictionary estimates more conservative than manual coding. Pooled regression with party fixed effects, standard errors clustered by party pair; adjusted R2 0.03 with n = 619 (Table 3 and Table 6 dictionary columns). (App. 3 Table 6 p. 2639; Sec. 7.1 Table 3 p. 2635) | press_value = final_sign x LSD heat (run_stage5.R lines 141-143). The per-category rollup in run_stage5.R (lines 171-182) is a decay-weighted mean of press_value, not a sum. The Python side (fec/moneyball_core.py lines 214-237) reads workbook raw values, applies normalize_signed_extrema from fec/stance/media_rules.py (about lines 215-230), which divides positive values by the largest positive and negative values by the absolute largest negative, and uses tier weights from row 535. Whether sign x intensity beats sign alone has not been tested; that repo-status claim was not verified here. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R:141-143,171-182; C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/moneyball_core.py:214-237; C:/Users/pgoff/OneDrive/Desktop/Melange Systems/fec/stance/media_rules.py:215-230` | The evidence is one Austrian campaign, in German, on negativity only. It suggests dictionary intensity carries a weaker, noisier version of the human-coded signal, so multiplying stance sign by that intensity may add noise. The paper does not test a stance x intensity product, so this is a caution, not a refutation. The paper's pooled coefficients should be labelled as pooled across parties with party fixed effects if quoted. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The summary says the evidence comes from 2013. The corpus spans 1995-2013, the validation sample pools four campaigns from 2002-2013, and only the two applications use the 2013 campaign.
> - Summary issue: The related link to b-2025-comparing-fine-tuned was removed. That study compares a fine-tuned RoBERTa with TF-IDF classifiers for Roman Urdu stance; it does not test dictionary intensity, so the contradicts relation is not supported.
> - Summary issue: The summary says the intensity measure is scored as |net| / words. That is correct for the default INTENSITY setting, but net itself flips negated hits rather than removing them (stage5_lsd.R lines 48-61).
> - Summary issue: Paul's highlight claims and the paul_alignment statements about Birkenmaier and the stale components list were not verified; this item has no Paul entries of its own.
> - Summary issue: The ext coverage field says Appendices 1-6 were read; the text also has Appendix 7 (CrowdFlower coding task, p. 2640 onward). Minor.

## Gaps and flags

- The LSD2015 intensity used in press_value has never been correlated with human-coded intensity on congressional press releases. The paper's protocol (about 200 items, a 0-4 scale, about 10 lay coders, 2 experts, reporting r and coverage) is a cheap test the system has not run.
- The release-level metric |pos - neg| / words cancels mixed praise and attack, and it penalizes long releases. The paper shows that the aggregation rule alone moves validity (mean vs max; App. 2 Table 5). No alternative aggregation (max per sentence, CU-sentence-only scoring) has been compared.
- There is no domain-specific tone dictionary for CU or financial policy. The Rice & Zorn corpus-expansion helper is opt-in and offline, and live scoring uses a generic dictionary, which is the configuration the paper warns about.
- LSD hits are counted over the whole release body, not restricted to the sentences that mention credit unions. Releases where LSD finds no tone words get zero intensity, so press_value becomes 0 even when DEBATE assigns a stance. That implicit 'non-match = neutral' rule is the one the paper flags as unvalidated outside its test sample.
- No ablation has tested stance sign x intensity against stance sign alone, using a behavioural criterion such as later CU cosponsorship or votes.

- CODE/DOC MISMATCH: the header of cu_press_release_scraper.R (lines 125-131) cites Haselmayer & Jenny and says 'Sentence tonality follows the strongest cue', but weighted_tone (lines 2443-2475) sums cues across sentences. The layer is display-only, and run_stage5 4b still uses its magnitude in score_label. Do not describe those labels as following Haselmayer & Jenny.
- The brain note debate-stance-model.md:102 calls LSD 'validated' on the basis of a three-case smoke test. By this paper's standard that is a functional check, not a validity result. Under DE-062, any claim that LSD intensity measures how heated a release is should stay UNVERIFIED until it has been compared with human coding.
- The paper's Table 3 coefficients are pooled across six parties with party fixed effects. If quoted, label them as from a pooled model with no party strata. Reporting is also inconsistent: Table 2 says 612 statements while its counts sum to 619, the n in Table 3.
- Nothing in the paper argues for either rejected SNA proposal, and it does not touch LDA, graph ranks or reciprocity, so there is no conflict with D-021, D-025 or D-104..D-108.

Paul has no annotation of this paper. It adds one point to the system's tone question: intensity is still bag-of-words, and its validity has never been measured. The legacy graded scorer adopted this paper's vocabulary (ordinal bands, 1.25 amplifier, negator exclusion) but not its max rule or crowd-derived weights.

## Related papers

- [[rice-2018-corpus-based-dictionaries]] — complements — Both argue for domain-specific sentiment dictionaries. Rice and Zorn derive one from the corpus with word vectors; this paper derives one from crowd ratings. The legacy scorer cites both papers. Verified against the Rice item's research question.
- [[merkley-nd-party-cues-news]] — complements — Merkley and Stecula use Lexicoder with a custom dictionary to detect party cues, not to measure sentiment intensity. The link is a method precedent for custom dictionaries, not a validation of tone. Verified against the Merkley item text.
- [[park-2025-automatic-construction-direction]] — same-method — Also a domain-lexicon construction method. The rejected bill-sign-handling test (3 good / 43 bad) is claimed in the analyst text and was not checked here; the link rests on the shared point that preprocessing and aggregation choices drive validity.
- [[ornstein-2024-train-your-stochastic]] — complements — Offers LLM few-shot coding of political text as an alternative to the crowdcoding step used for intensity gold labels. Verified against the Ornstein item research question.
- [[taherdoost-2023-artificial-intelligence-sentiment]] — complements — Scoping review of AI sentiment analysis in competitive research. The lexicon-versus-ML framing was not confirmed in the item text, so this is a weak link kept only because the item exists and covers sentiment methods.

### Linked from
- [[pennebaker-2003-psychological-aspects-natural]] — complements — Real item (2017, German-language Austrian political text; dictionary plus crowd-coded negativity). It pairs a dictionary with crowd judgments but does not test the review judge-versus-self-report finding. Link is loose.
- [[taherdoost-2023-artificial-intelligence-sentiment]] — complements — Combines a political-communication dictionary with crowdcoding validation, the validation step that the review never addresses for dictionary tone.
- [[burnham-2023-stance-detection-practical-guide]] — complements — Title matches a dictionary-plus-crowdcoding sentiment study. Neither text cites the other; Burnham's sentiment-is-not-stance argument bears on it indirectly, so contradicts was softened.
- [[osnabrugge-2021-playing-gallery-emotive]] — complements — Not cited by this paper; analyst link on dictionary plus crowd-coding validation of political negativity.
- [[rheault-2016-measuring-emotion-parliamentary]] — complements — Builds a political sentiment dictionary validated by crowdcoding on Austrian political text, which speaks to Rheault's film-review-only validation gap. Verified in the Haselmayer item.