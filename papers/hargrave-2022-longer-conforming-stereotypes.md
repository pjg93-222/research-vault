---
type: paper
title: "No Longer Conforming to Stereotypes? Gender, Political Style, and Parliamentary Debate in the UK"
authors: ["Lotte Hargrave", "Jack Blumenau"]
year: 2021
venue: "UCL working paper"
doi: ""
category: "Machine Learning & Sentiment Analysis"
systems: ["melange-psychometrics"]
components: ["melange-psychometrics.persona-agency-communion", "melange-psychometrics.affective-n", "melange-psychometrics.lexical-big5"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/hargrave-2022-longer-conforming-stereotypes.pdf]]"
original_paths: []
sha12: ["6d06ef768995"]
tags: [paper, machine-learning-sentiment-analysis]
---

# No Longer Conforming to Stereotypes? Gender, Political Style, and Parliamentary Debate in the UK

**Lotte Hargrave, Jack Blumenau (2021).** *UCL working paper.* Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> Hargrave and Blumenau (UCL working paper; the text is dated October 2021, filed here as 2022) track how gendered debating style changed in the UK House of Commons between May 1997 and March 2019. The corpus is 418,147 MP-debate observations: 1,422 MPs across 14,864 debates. They score eight styles. Three are communal: human narrative, affect and positive emotion. Five are agentic: fact, complexity, repetition, aggression and negative emotion. Six of the measures come from seed dictionaries expanded with GloVe embeddings, and these beat standard dictionaries against human coding for every style except fact (positive emotion about 0.70 against 0.38). Each style goes through a Bayesian dynamic hierarchical model with debate random effects, so gender gaps are estimated within debates. Early in the period, women used more communal styles and men more aggression and complexity. Women also used more fact, repetition and negative emotion, which runs against the stereotype. For six of eight styles the share of between-MP variation explained by gender fell, by 29 to 60 percent comparing the periods before and after 2007. Fact and negative emotion went the other way, rising to about 4 and 9 percent of MP-level variation after 2007. The change sits mostly with women, whose communal styles declined. The work is observational and does not separate mechanisms. For Paul's systems, the main point is that agency, communion and negative-emotion text scores can carry a gender signature that changes over time. The persona channel and affective N in the receptivity layer check for a party signature but not for gender. This is UK floor debate measured with dictionaries, not the US Congress, so any transfer is a hypothesis to test, not a finding.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Have gender gaps in parliamentary debating style, measured against gender stereotypes, narrowed in the UK House of Commons since 1997, and are women moving toward agentic styles over time (p.2-3, p.12)?

**Theory:** Gender role and role congruity theory (Eagly and Karau 2002): stereotypes produce conformity through internalised standards and social sanctions. Stereotypes are dynamic (Diekman and Eagly 2000): changing social roles, public attitudes and women's rising parliamentary presence should weaken conformity pressure (p.9-13). Rival feminist institutionalism predicts women move toward communal styles as their numbers and power grow (p.13, p.26-28). The authors do not separate the mechanisms (p.12, p.29).

**Data:** UK House of Commons debate speeches, May 1997 to March 2019 (source not named in text). Unit: one MP's speech-document per debate. Sample: 14,864 debates, 1,422 MPs (370 female, 1,052 male), 418,147 MP-debate observations (p.14-15); nearly half a million speeches (p.4). 20 parliamentary sessions, about 635 MPs per session, each MP in about 9 sessions on average (p.20). Speeches under 50 words, the Speaker's contributions and debates with fewer than five participants are dropped (p.15).

**Method:** Eight style measures from text. Six use word-embedding expanded seed dictionaries (GloVe, 150 dimensions, window 3; sigmoid rescaling a=40, c=0.35; TF-IDF weighted sentence scores) (p.15-17, S2-S7). Complexity uses rescaled Flesch-Kincaid. Repetition uses Lempel-Ziv compression ratio. Measures standardised to mean 0, SD 1. Outcome model: Bayesian dynamic hierarchical normal model in Stan, one fit per style (three chains, 500 iterations after 250 burn-in); MP-by-session intercepts, debate random effects, random-walk priors on male baseline and female gap across sessions (p.19-22).

**Identification:** Observational design, no causal identification strategy. Gender gaps are identified from within-debate variation because debate random effects absorb topic and debate differences (p.22-23). Robustness checks: covariate model (S20-S24), debate-type models (S25-S29), within-MP versus replacement decomposition (S30-S35), topic confounding with correlated topic models K=10 to 80 (S36-S43), and a participation test (S44-S46).

**Scope:** UK House of Commons floor debate, 1997-2019. Population is MPs who speak in debates with five or more participants. Not campaign speech, votes, or voter sanctions. Covers the post-1997 era through 2019, with women rising from 18% to 32% of seats (p.11-12).

## Key findings

- Early period: female MPs use more human narrative, affect and positive emotion, while men use more aggression and complexity, at least before 2010 (p.23-24).
- Fact, repetition and negative emotion run against stereotype: women use them more than men for much of the period (p.23-24).
- Six of eight styles: MP gender explains less between-MP variation in style at the end of the period than at the start (p.4).
- For styles that converge, the share of MP-level variation explained by gender falls by between 29% and 60% comparing periods before and after 2007 (p.25).
- Fact and negative emotion diverge: gender explains about 0.6% and 2% of variation in 1997-2007, rising to about 4% and 9% after 2007 (p.25).
- Aggression, complexity, affect and positive emotion gaps shrink to no consistent difference in recent years; the repetition gap stays relatively stable (p.23-26).
- Change is concentrated among women: their communal styles decline while their negative emotion, fact and aggression rise, and men stay relatively stable on those (p.26-27).
- Within-MP versus replacement: neither mechanism alone explains the convergence; divergence in negative emotion is almost entirely differential replacement of MPs (p.28; S30-S35).
- Validation: text measures outperform standard dictionaries against human codings for every style except fact (p.S17-S19).
- Covariate model (party, government status, frontbench, committee chair, age, margin, degree, occupation) leaves the main trends intact; aggression and complexity attenuate (p.28; S21-S22).
- Topic confounding: a style gap on a topic largely does not predict that topic's growth; results hold for 10 to 80 topics (p.28; S40-S43).
- Participation: an MP's average style does not predict how many speeches they make, so selection into debate does not drive the trends (p.28-29; S44-S45).

## Limitations

- Voter sanctions for counter-stereotypic behaviour are not measured, so the mechanism linking debate style to voter penalties is untested (p.29-30).
- Mechanisms are not separated: the authors do not disentangle the three pathways they outline (p.12, p.29).
- Only legislative debate is studied; campaign communication, where voter penalties may bite harder, is not examined (p.31).
- Seed dictionaries for aggression and human narrative were built by the authors from close reading (p.15).
- Repetition is validated only at sentence level, which the authors concede is less suited to that concept (S18 note 3).
- Complexity gap shift between 2008 and 2013 is confounded by covariates, and aggression and complexity trends attenuate (p.28; S21-S22).
- Flesch-Kincaid chosen over domain-specific complexity measures for simplicity (p.17-18).
- Sampler run is short (three chains, 500 iterations after 250 burn-in) (p.22).
- Number of topics K is not fixed a priori; results rely on the sweep 10 to 80 (S41).

## Short quotes

> gender stereotypes are worse descriptors for actual political behaviour in the UK now
> women in the House of Commons demonstrate less communal and more agentic styles
> The pursuit of a better politics requires more than vaguely hoping

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-psychometrics#Agency-communion persona channel\|Agency-communion persona channel]] | qualifies | Communal styles (human narrative, affect, positive emotion) were higher among female MPs early in the period. Positive emotion and affect show no consistent significant gender gap by the latest years; the human narrative gap narrows gradually but is not stated to reach zero. For six of eight styles gender explains less between-MP style variation at the end of the period than at the start. For the converging styles the share of MP-level variation explained by gender falls 29 to 60 percent comparing pre-2007 with post-2007; the converging styles are not named individually. Window: UK Commons May 1997 to March 2019. Denominator: MP-by-session variation (R2-style metric). (p.23-27 (results); p.9 Table 1; p.25 (29-60 range); p.26-27 (communal decline); p.4 (six of eight)) | persona_susc_z = .60 z(communion) - .40 z(agency) from LLM cue shares (agency_communion_llm.py), blended at PERSONA_W 0.30 for members with 15 or more classified sentences (receptivity_layer.py). No gender term in the persona code. Comments record cooperation_v2 gender -.021 and reactivity as gender-clean. The claimed party signature (communion D .099 vs R .049) was not found in fec/members or the audit files searched. `fec/members/receptivity_layer.py lines 139-158 and 191 (persona build and blend); fec/members/receptivity_layer.py lines 70 and 98 (gender-clean comments); fec/members/agency_communion_llm.py (cue codebook)` | The paper gap is on UK floor speech using embedding-expanded dictionaries; the system uses Birkenmaier-Lechner LLM cue shares on US congressional sentences, a different instrument. Transfer holds only if US cue shares show a gender gap. Check: persona_susc_z against the legislators.csv gender column, partialled on party. Stays UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Affective neuroticism slot and N genre-transfer tests\|Affective neuroticism slot and N genre-transfer tests]] | qualifies | Negative emotion runs against the stereotype: women use more of it than men for much of 1997-2019. Gender explains about 2 percent of MP-level variation in negative emotion in 1997-2007 and about 9 percent after 2007. For negative emotion the divergence is almost entirely differential replacement of MPs, with a near-zero within-MP gender difference (supplement Fig S8). The main text says neither replacement nor within-MP change alone explains the convergence across styles. Window: UK Commons 1997-2019. (p.23-25 and p.25 (R2 figures); p.28 (main-text mechanism statement); S30-S35 and Fig S8 (replacement decomposition)) | affective_n is the sum of LLM affective facets (pessimism, anxiety, depression, volatility), residualized on E then on party and z-scored (emit_affective_n.py). The LIWC composite is kept only as a convergent anchor (r +.24 per code comment). The file has no gender term, so there is no gender residualization or gender audit. Input source (interview text) was not traced further. `fec/members/emit_affective_n.py lines 39-56 (facet sum, E and party residualization, z-score, LIWC anchor comment); no gender term in the file` | Applies only if US members negative-affect expression differs by gender in the interview genre. The paper measures negative emotion as a dictionary score on UK floor speech, not the LLM facets, so this is a different operationalisation. Any chamber-composition effect needs a stated window. The interview-genre transfer figures cited by the analyst were not rechecked. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | method-source | Seed dictionaries expanded with GloVe word embeddings correlate with human coder judgements at 0.61 to 0.85 per style across the comparison and intensity tasks. Standard dictionaries do worse, e.g. positive emotion 0.38 and 0.34 against 0.70 and 0.71 for the embedding measure. Each coder completed about 70 comparisons per style on average. Six of eight styles use the embedding expansion, and the embedding measure beats standard dictionaries for every style except fact (bootstrap test). (p.15-17 (method); Table S4 p.S17; p.S19 (comparison with standard dictionaries)) | Closed-vocabulary posemo, negemo, anx and anger rates use published-exemplar word lists of about 20 to 32 words each (big5_traits.py), replaced by licensed LIWC .dic files if dropped into fec/liwc. Rates are z-scored and enter trait fits through weights. The paper validation covers UK debate sentences, not these lists. `fec/members/big5_traits.py lines 77-87 (CAT word lists) and 108-114 (dictionary hooks); word counts checked` | The paper validation is on UK House of Commons sentences with two coders. Using its embedding expansion on congressional text needs a fresh human-coded sample. Any change to trait inputs falls under change control and single-look discipline. This is not a finding that the current lists are wrong. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Year: the item file says 2022 but the text reads This version October 08, 2021 and the extraction corrects to 2021. No venue is named in the text. Correct the item year or record the source for 2022.
> - Summary issue: Summary states six of eight styles fall 29 to 60 percent. The text gives six of eight at p.4 and the 29-60 range for converging styles at p.25, without naming the six. The six are inferred, not stated.
> - Summary issue: Summary says the whole measurement is dictionary-based. Only six styles use embedding-expanded dictionaries; complexity uses rescaled Flesch-Kincaid and repetition uses Lempel-Ziv compression ratio (p.17-18).
> - Summary issue: Summary says the persona channel and affective N check party but not gender. The persona party signature (communion D .099 vs R .049) was not located in the files searched, so the party-check claim is unverified.
> - Summary issue: Summary says divergence in negative emotion is near-entirely differential replacement. This holds for negative emotion only (S8); the main text says neither mechanism alone explains convergence (p.28).
> - Summary issue: Related slugs rheault, ramey, aroyehun, lechner and birkenmaier are real items but are not cited in the Hargrave text; their links are analyst judgements and are labelled complements.
> - Summary issue: Rice item slug year 2018 differs from the paper citation Rice and Zorn 2021; the title match is confirmed.
> - Summary issue: The paul_alignment field and the components.txt staleness claim were not verifiable from the paper text and were not checked. No Paul markup exists for this work (0 items, 0 dropped).

## Gaps and flags

- No gender audit exists for the persona channel (persona_susc_z) or for affective_n, although cooperation_v2, reactivity and voice ES were all checked against gender. legislators.csv carries a gender column, so a party-partialled check is cheap.
- Psychometric text scores are corpus-relative z with no control for topic or debate context. The paper estimates style gaps within debates (debate random effects), while the system cannot separate a member's style from the topics or venues they speak in.
- The persona channel assumes communal rhetoric signals approachability and susceptibility. The paper shows stereotype-linked style is time-varying and driven by chamber composition, so a static communal-means-susceptible prior may drift. Nothing in the system re-checks the prior over time.
- Emotion categories in big5_traits.py use short exemplar lists with no human-coded validation sample on congressional text.
- Metadata: the text is dated October 08, 2021, while the vault slug and hint say 2022. The venue is a UCL working paper.
- components.txt describes affective_n as z(LIWC) + z(LLM). The live code (emit_affective_n.py:39-56) uses the LLM facets only, with LIWC as a convergent anchor, so the component row is stale.
- Any gender-residualization suggestion drawn from this paper is new agent analysis and stays UNVERIFIED until a second path reproduces it (DE-062). Changing receptivity inputs is not recommended here, only checking them.
- No conflict with the SNA rejections (D-021, D-025, D-104..D-108). The paper has no network content.

No Paul annotations or entries exist for this work (0 markup, 0 dropped). The closest of Paul's positions is on persona-agency-communion, built on Birkenmaier and Lechner 2025: codebooks and node design, with his concerns being the conversion from trait to node and ambiguous statements. This paper adds a concern Paul's notes do not raise, namely that agency and communion text scores can encode gender and that the encoding shifts over time. This sharpens his trait-to-node worry, since PERSONA_W 0.30 may partly carry gender. His note is not stale, only silent on demographic confounds.

## Related papers

- [[birkenmaier-2025-measuring-politicians-public]] — complements — Item exists and its title matches agency and communion text measurement. Not cited in the Hargrave text; the link is analyst judgement. Birkenmaier-Lechner is the codebook behind the system's persona instrument.
- [[lechner-2025-mapping-political-personality]] — complements — Item exists (PoliPers framework, title confirmed). Not cited in the Hargrave text; analyst judgement link to the agency and communion codebook.
- [[osnabrugge-2021-playing-gallery-emotive]] — cites — Real link: the Hargrave text cites Osnabrugge, Hobolt and Rodon 2021 (Playing to the Gallery) on emotive rhetoric in parliaments.
- [[rice-2018-corpus-based-dictionaries]] — same-method — Item exists with matching title (Corpus-Based Dictionaries for Sentiment Analysis of Specialized Vocabularies). The Hargrave text cites it as Rice and Zorn 2021 for the corpus-based dictionary approach; the item slug year 2018 differs from that citation.
- [[rheault-2016-measuring-emotion-parliamentary]] — complements — Item exists. Its text builds word vectors on the full House of Commons Hansard corpus (embedding route confirmed in the text). Not cited by Hargrave; analyst judgement link.
- [[pennebaker-2003-psychological-aspects-natural]] — complements — Item exists (2003 Annual Review). The Hargrave text uses LIWC 2015 (Pennebaker et al 2015), not this review; the link is about the closed-dictionary tradition, not a citation.
- [[aroyehun-2026-politics-of-feelings]] — complements — Item exists (title confirmed: emotional expression and legislative effectiveness in US Congress). Not cited by Hargrave; analyst judgement link on the US venue.
- [[ramey-2016-measuring-elite-personality]] — complements — Item exists (title confirmed: Measuring Elite Personality Using Speech). Not cited by Hargrave; the closed-vocabulary description in the analyst output was not checked.

### Linked from
_None._