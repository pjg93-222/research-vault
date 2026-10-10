---
type: paper
title: "Playing to the Gallery: Emotive Rhetoric in Parliaments"
authors:
  - "Moritz Osnabrugge"
  - "Sara B. Hobolt"
  - "Toni Rodon"
year: 2021
venue: "American Political Science Review 115(3), 885-899"
doi: "10.1017/S0003055421000356"
category: "Machine Learning & Sentiment Analysis"
systems:
  - melange-psychometrics
  - melange-stance
  - moneyball-rank-pillars
components:
  - melange-psychometrics.lexical-big5
  - melange-psychometrics.affective-n
  - melange-stance.lsd-tone
  - moneyball-rank-pillars.press-pillar
paul_annotated: false
paul_markup_count: 0
evidence_status: verified
source: "[[_sources/osnabrugge-2021-playing-gallery-emotive.pdf]]"
original_paths:
sha12:
  - f4f797a4a9d8
tags: [paper, machine-learning-sentiment-analysis]
---

# Playing to the Gallery: Emotive Rhetoric in Parliaments

**Moritz Osnabrugge, Sara B. Hobolt, Toni Rodon (2021).** *American Political Science Review 115(3), 885-899.* [10.1017/S0003055421000356](https://doi.org/10.1017/S0003055421000356). Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Osnabrugge, Hobolt and Rodon (APSR 2021) ask whether legislators use emotive language strategically, raising it when a debate reaches voters rather than only colleagues. They score 958,925 UK House of Commons speeches (2001-2019) and 945,734 Irish Dail speeches (2002-2013) with a dictionary built from ANEW seed words (emotive = clearly positive or negative, neutral = neither) expanded through corpus-trained skip-gram embeddings; each speech gets percent emotive minus percent neutral words (mean 0.88, SD 8.95 on a -100 to 100 scale). OLS with MP-clustered errors shows Prime Minister's Questions speeches score 2.23 points higher than ordinary debates, and 1.56 to 1.66 higher within the same MP once MP fixed effects are added; the Queen's Speech opening day is similar, Ministerial Question Time is slightly lower, and topic fixed effects do not remove the gap. Ireland's Leaders' Questions shows 2.5 to 4.6. The electoral-cycle effect is tiny (0.015 per 100 days). The authors state the design is observational and cannot causally identify audience exposure. For Paul's systems the paper matters in two places. First, it is direct evidence that the same speaker's affect-word rates move with venue and audience, which bears on the lexical Big Five and affective-N slots: Melange pools social posts, floor speech and interviews per member before z-scoring, and the interview-to-floor N transfer already failed. It supports Paul's ask for venue-specific frameworks. Second, its valence-agnostic emotive score is closer to the unused lsd_charge option than to the live abs(net)/words intensity in the press pipeline, and its audience logic cautions that press-release heat may index performance for the public rather than commitment.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** When and why do legislators use emotive rhetoric in parliamentary speeches? Is emotive language used strategically to appeal to voters, so that it rises when a debate has a larger public audience?

**Theory:** Rational choice (Downs 1957): politicians maximise electoral appeal. Emotive language is a persuasion tool that shapes how voters process messages (Brader 2005; Weeks 2015; Bless et al. 1992). Legislators address voters in high-profile debates and colleagues in low-profile ones, so emotive rhetoric should be higher in high-profile debates. Trade-off: overt emotion can signal weak credibility, so speakers balance ethos, logos and pathos depending on audience. Hypothesis H: legislators make greater use of emotive rhetoric in high-profile debates than in low-profile debates (p. 4).

**Data:** Unit: individual speech. UK: 958,925 House of Commons speeches, 2001-2019 (Table 1, p. 6), from Rheault et al. (2016) for 2001-2014 plus TheyWorkForYou. Speeches under 40 characters and Speaker/deputy Speaker speeches dropped. Debate counts: PMQs 34,030; Queen's Speech 15,160; Ministerial Question Time 279,076; Urgent Questions 37,523. MP covariates: party, leadership, minister, shadow cabinet, seniority, chair, gender, age. Ireland: 945,734 Dail speeches, Nov 2002-2013 (Herzog and Mikhaylov 2018). Replication data: Harvard Dataverse doi 10.7910/DVN/QDTLYV.

**Method:** Dictionary score per speech = percent emotive words minus percent neutral words, -100 to 100 (p. 6-7). Dictionary: ANEW seeds (851 emotive, 970 neutral) expanded with skip-gram embeddings (window 10, 250 dims) and cosine scoring against seeds; final 2,015 emotive and 2,095 neutral words (UK), 1,692 and 1,764 (Ireland). OLS, SEs clustered by MP, five specifications (baseline, time trend, MP fixed effects, controls and party FE, length-weighted MP FE). Topic check: scikit-learn classifier trained on Manifesto corpus, seven topics, used as topic fixed effects (Table 4).

**Identification:** No causal identification. The authors say explicitly that the design cannot causally identify the effect of audience exposure (p. 10). Within-MP fixed effects remove time-constant speaker traits. PMQ questioners are selected randomly from interested MPs, which the authors say reduces selection bias (p. 9). Topic fixed effects test the alternative that topic drives the gap.

**Scope:** UK House of Commons, 2001-2019, all speeches meeting the length rule; Irish Dail lower house, November 2002-2013, Leaders' Questions versus other debates. Lower houses only. Analysis is observational across debate formats within two parliaments.

## Key findings

- Baseline model (Table 3, Model 1, p. 8): speeches in PMQs score 2.226 points higher on the emotive-minus-neutral scale (SE 0.128, p<0.01).
- With time trend, MP fixed effects and controls (Model 4, p. 8-9): PMQs effect 1.618 (SE 0.167); MP fixed effects alone (Model 3) 1.562 (SE 0.137).
- Length-weighted MP fixed-effects model (Model 5, p. 9): PMQs 1.663 (SE 0.214) and Queen's Speech opening day 2.064 (SE 0.166).
- Queen's Speech opening day debate: 2.172 in Model 1 (SE 0.214); other Queen's Speech days 0.581 (SE 0.106), p. 8.
- Ministerial Question Time is slightly negative (-0.293 in Model 1; -0.207 in Model 5); Urgent Questions effect not robust (Table 3, p. 8-9).
- Predicted levels from Model 4 (Figure 3, p. 9): about 2.5 for PMQs, about 3.0 for Queen's Speech opening day, 1.5 for other Queen's Speech days, and 1.0 or lower for other debate types.
- Electoral cycle: each 100 days closer to a general election raises emotive rhetoric by 0.015 (p. 10); the authors call this small because the Commons closes 25 working days before elections.
- Topic fixed effects (Table 4, p. 11): PMQs effect 2.020 in Model 1 and 1.383 to 1.525 in other models, all p<0.01; the authors conclude topics do not explain the PMQ gap.
- Topic averages (Figure 4, p. 10): fabric of society has the highest mean emotiveness and economy the lowest.
- Ireland (Table 5, p. 12): Leaders' Questions coefficient 4.591 (Model 1), 3.975 (Model 3), 3.792 (Model 4), 2.497 (Model 5), all p<0.01, N = 945,734 speeches.
- Most speeches are neither highly emotive nor highly neutral: mean emotive score 0.88 with SD 8.95 on the -100 to 100 scale (p. 7).
- Authors conclude that emotive rhetoric is used primarily when legislators address voters as well as colleagues, and that the pattern generalises to Ireland (p. 12-13).

## Limitations

- Authors state the design cannot causally identify audience exposure; the pattern is described as a plausible explanation (p. 10).
- Generic ANEW dictionary was not built for political text, so measurement error is possible (p. 6); domain words were added via embeddings.
- Score is a share of emotive versus neutral words, so the authors note a few emotive words can shift a speech's meaning (p. 7).
- Dictionary method chosen over supervised annotation for cost reasons (p. 5); the study does not measure positive versus negative sentiment in the main text.
- Urgent Questions and some Ministerial Question Time effects are not robust; Ireland covers one debate type and 2002-2013.
- Appendices A-J are not in the extracted text, so the robustness checks cited to them cannot be verified here.
- Abstract reports two million speeches; the body reports 958,925 UK and 945,734 Irish speeches. Treat the body figures as the analysed sample.

## Short quotes

- "the high level of emotive rhetoric in PMQs is not the result"
- "Our argument is that politicians are most likely to employ such rhetoric"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | qualifies | Within the same MP (MP fixed effects), PMQ speeches score 1.562 to 1.663 points higher on the emotive-minus-neutral scale than other debates (Table 3 Models 3-5; speech-level SD 8.95; UK 2001-2019, 958,925 speeches). The Queen's Speech opening day is 2.064 in Model 5. The authors read this as a venue pattern and say it is not causally identified. (Table 3 p. 8 with Models 3-5 on p. 9; SD on p. 7; causal caveat p. 10) | The default corpus pools social posts, CREC floor speech and INTERVIEW turns per member into one text list (CHRG excluded by default). Per-member category rates are per-1000-word. Genre is only a validation hook (BIG5_SOURCES), not a covariate, so members with different venue mixes can differ in affect rates for venue reasons. `fec/members/big5_traits.py:36-73 (corpus pooling and BIG5_SOURCES hook); fec/members/big5_traits.py:220 (per-1000-word rates); fec/members/big5_traits.py:179-182 (posemo/negemo weights in E, ES, A, C)` | Applies only if US floor speech, interviews and social posts differ in audience exposure the way PMQs differs from ordinary Commons debate. The paper covers UK and Irish lower houses with a valence-agnostic ANEW-based score, not LIWC posemo/negemo. The speech-level shift is about 0.18 SD. Observational, not causal. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Affective neuroticism slot and N genre-transfer tests\|Affective neuroticism slot and N genre-transfer tests]] | supports | The authors argue, without causal identification, that legislators raise emotive rhetoric when addressing voters: PMQ speeches are higher within the same MP (about +1.6, Table 3) and Irish Leaders' Questions higher by +2.5 to +4.6 (Table 5, N=945,734). The paper does not test whether speakers keep their rank order in affect across venues. (Theory pp. 3-4; Table 3 p. 8; Table 5 p. 12; causal caveat p. 10) | affective_n is built from INTERVIEW turns only (source filter in emit_affective_n.py), and the recorded transfer of interview N to floor and hearing text failed. The paper gives a plausible venue mechanism for that failure. The rho values cited in the item (.050, .161) were not found in the cited code or in fec/members text files checked in this pass. `fec/members/emit_affective_n.py:30-38 (source='INTERVIEW' filter); fec/members/affective_n_validate.py:44 (correlation helper only; no rho output stored here)` | Explains, does not prove, the transfer failure. The paper tests average venue shifts, not member rank-order preservation, which is what rho measures. Does not license re-scoring floor text. | weak | VERIFIED-SOFTENED |
| [[systems/melange-stance#Lexicoder LSD2015 tone intensity\|Lexicoder LSD2015 tone intensity]] | method-source | Emotiveness is measured valence-agnostically: percent emotive words (ANEW, 851 emotive seeds) minus percent neutral words (970 neutral seeds). Skip-gram embeddings expand the dictionary to 2,015 emotive and 2,095 neutral UK words, following Rice and Zorn (2019). Scale runs from -100 to +100; mean 0.88, SD 8.95. (Measurement section pp. 5-7 (Equation 1; dictionary counts p. 6; scale and SD p. 7)) | Live press intensity is abs(lsd_net_norm), i.e. abs(pos - neg)/words, chosen by the CU_S5_INTENSITY default of net_norm. stage5_lsd.R also emits lsd_charge = (pos + neg)/words, used only when INTENSITY is charge. The LSD tone dictionary is generic with no domain expansion (not re-verified in this pass). `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R:45 (INTENSITY default net_norm); C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R:141-143 (heat and press_value); C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_lsd.R:49-61 (lsd_tone with lsd_net_norm and lsd_charge)` | Applies if 'how heated' should mean emotional charge regardless of direction. The paper validates on parliamentary speech, not press releases. Switching intensity changes press_value and the Press pillar, a change-control decision for Paul, not a recommendation here. | moderate | VERIFIED |
| [[systems/moneyball-rank-pillars#Press pillar (own-domain, signed raw-max)\|Press pillar (own-domain, signed raw-max)]] | qualifies | Hypothesis H (p. 4): legislators use more emotive rhetoric in high-profile debates with a larger general audience. The theory section adds that overt emotion can signal weak credibility, so speakers balance ethos, logos and pathos by audience. The paper studies parliamentary speech only, not press releases. (Hypothesis p. 4; theory pp. 3-4; conclusion pp. 12-13) | press_value = stance sign x heat, where heat defaults to abs(lsd_net_norm) (run_stage5.R:141-143). press_matrix in moneyball_core.py normalizes signed extrema within each domain. Press releases are one public-facing venue, so the intensity term may index messaging style more than commitment; this is analyst inference, not a paper finding. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R:141-143; fec/moneyball_core.py:214-236 (press_matrix)` | Holds only to the extent that between-member intensity differences reflect style rather than commitment. The paper shows within-speaker venue shifts in parliament, not between-member style differences within one venue. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The summary says the paper 'supports Paul's ask for venue-specific frameworks' and that 'his note is not stale'. The item has paul_entries empty and no annotation, so this Paul attribution is unsupported here and should be removed or sourced.
> - Summary issue: The summary's statement that the interview-to-floor N transfer failed (rho .050, .161) has no traceable source in the cited code or fec/members text files checked; unverified.
> - Summary issue: The summary's statement that venue, not speaker, shifts emotive word rates overstates the paper, which says the design cannot causally identify audience effects (p. 10).
> - Summary issue: The summary and bearings place the electoral-cycle and causal-caveat text on p. 10; in the extracted text the running-head page numbers are ambiguous, so the page cite is approximate.
> - Summary issue: Appendices A-J are not in the extracted text; robustness claims that cite them (LIWC, alternative scalings, human validation) are unverified.
> - Summary issue: Four of the seven related links (Ramey, Aroyehun, Haselmayer, Koutsoumpis) are not cited by this paper; they are analyst judgements between vault items and are kept but labeled.
> - Summary issue: Abstract reports two million speeches; body analysed samples are 958,925 UK and 945,734 Irish. The summary uses the body figures correctly.

## Gaps and flags

- No venue or genre adjustment in the lexical Big Five: affect-category rates are pooled across social, floor and interview text per member; a per-genre z-score or genre-mix covariate is absent (BIG5_SOURCES is a validation hook only).
- Press intensity uses abs(net)/words, which cancels mixed-valence heat; the valence-agnostic lsd_charge option exists but has not been validated against human-coded intensity (open question on melange-stance.lsd-tone).
- No test of whether stance sign x intensity predicts CU-aligned behaviour better than stance sign alone; the paper's audience logic makes that test more pressing.
- No domain-expanded dictionary for congressional press or floor text; a Rice and Zorn style embedding expansion is on disk as a paper but unused.
- Observational design: the authors say they cannot causally identify the audience effect (p. 10); quote any use as association.
- Pooled baseline (PMQs 2.226) mixes speakers and parties; quote the within-MP estimates (1.562 to 1.663, Table 3 Models 3-5) when citing a venue shift, with window UK 2001-2019 and denominator 958,925 speeches.
- Abstract says about two million speeches; analysed samples are 958,925 UK (2001-2019) and 945,734 Irish (2002-2013) speeches; cite the body figures.
- Appendices A-J (LIWC, positive/negative split, alternative scalings) are not in the extracted text and were not checked.

Paul has no annotation on this work. The evidence agrees with his stated position on the lexical Big Five that Ramey-style scoring needs venue-specific frameworks: this paper shows the same legislators shift emotive word use by venue and audience. His note is not stale: the system still pools venues by default (big5_traits.py:36-73) with genre restriction only as a validation hook, while the affective-N slot stays interview-only after its transfer test failed, which is consistent with this paper.

## Related papers

- [[rheault-2016-measuring-emotion-parliamentary]] — same-data — The paper takes its 2001-2014 UK Commons speech data from Rheault et al. (2016) (data section) and cites them for seed-word embedding scoring (measurement section). Verified in text.
- [[rice-2018-corpus-based-dictionaries]] — same-method — The paper expands its ANEW dictionary following Rice and Zorn (2019) corpus-based dictionary approach. Verified in text.
- [[dietrich-2019-pitch-perfect-vocal]] — complements — The paper cites Dietrich, Hayes and O'Brien (2019) for visual or audio measurement of political emotion. Verified in text; the vocal-pitch channel is the analyst's comparison.
- [[ramey-2016-measuring-elite-personality]] — complements — Not cited by this paper; analyst link. Ramey infers traits from floor speech, and this paper's venue result bears on that reading.
- [[aroyehun-2026-politics-of-feelings]] — extends — Not cited by this paper; analyst link carrying the emotive-rhetoric construct into US Congress.
- [[haselmayer-2017-sentiment-analysis-political]] — complements — Not cited by this paper; analyst link on dictionary plus crowd-coding validation of political negativity.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — Not cited by this paper; analyst link on LIWC-to-Big-Five meta-analysis bounding trait versus context in affect rates.

### Linked from
- [[decter-frain-2016-impressive-words-linguistic]] — complements — Also treats parliamentary rhetoric as aimed at an outside audience, but works on emotive language where this paper works on agentic and communal language.
- [[hargrave-2022-longer-conforming-stereotypes]] — cited-by — Real link: the Hargrave text cites Osnabrugge, Hobolt and Rodon 2021 (Playing to the Gallery) on emotive rhetoric in parliaments.