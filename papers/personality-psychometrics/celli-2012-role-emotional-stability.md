---
type: paper
title: "The Role of Emotional Stability in Twitter Conversations"
authors: ["Fabio Celli", "Luca Rossi"]
year: "2012"
venue: "Proceedings of the 13th Conference of the European Chapter of the ACL (EACL 2012), Avignon, France, pp. 10-17. Metadata hint confirmed."
doi: ""
category: "Personality & Psychometrics"
systems: ["melange-influence-engine", "melange-psychometrics"]
components: ["melange-psychometrics.lexical-big5", "melange-psychometrics.affective-n", "melange-influence-engine.receptivity-layer", "melange-psychometrics.persona-agency-communion"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "unverified"
source: "[[_sources/celli-2012-role-emotional-stability.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\W12-0602.pdf"]
sha12: ["a8112dd8d508"]
tags: [paper, personality-psychometrics, type/paper, theme/personality-psychometrics, evidence/unverified]
---

# The Role of Emotional Stability in Twitter Conversations

**Fabio Celli, Luca Rossi (2012).** *Proceedings of the 13th Conference of the European Chapter of the ACL (EACL 2012), Avignon, France, pp. 10-17. Metadata hint confirmed..* Theme: [[themes/personality-psychometrics|Personality & Psychometrics]].

> [!abstract] Summary
> Celli and Rossi (EACL 2012) built an unsupervised tagger that labels Twitter users as neurotic, secure or balanced. It borrows published correlations: linguistic cues from Mairesse et al. 2007 (punctuation, emoticons, long words) and follower/following/retweet counts from Quercia et al. 2011. The tagger ran on about 200,000 English posts sampled from the public timeline in December 2011, giving 5,392 ego-networks. Validation is very small:
> - 0.625 accuracy against a 0.5 baseline on 8 Personage users.
> - Average F1 0.578 against Analyzewords on 18 users. Analyzewords is a LIWC-style proxy that does not measure the Big Five.
> 
> The descriptive results:
> - Users tend to talk with others who share their trait. Fig. 4 gives no coefficient and no test.
> - Neurotic users post and retweet more. Calm users reply slightly more.
> - Neurotic subgraphs have longer chains: average path length 1.551 vs 1.334, diameter 6 vs 5. Calm subgraphs have more mutual ties.
> 
> There are no inferential tests and no identification strategy.
> 
> For Paul's systems this is a weak, era-bound source. Its main use is as a lineage and caution marker for the psychometrics stack:
> - big5_traits.py uses the same Mairesse-style closed-vocabulary ES/N weights.
> - The affective-N slot is anchored for convergence to LIWC, and Celli's validation has the same method-shared circularity: a LIWC-family tagger scored against a LIWC-family proxy.
> - The trait-homophily claim is the only finding that touches a system decision. The system already tested persona homophily on its own corpus and found it UNEARNED (|r| <= 0.015). An uncoefficiented 2011 Twitter frequency count cannot overturn that.
> 
> Nothing here bears on susceptibility to pressure, which is what the receptivity layer's N slot assumes. Every bearing below is UNVERIFIED (DE-062).

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Do users with the same emotional-stability trait (Big Five; neurotic vs secure/calm) interact differently on Twitter? The paper examines homophily in who talks to whom, posting and retweeting rates, and the shape of the resulting networks (Sec. 1, pp. 10-11; Sec. 4, pp. 14-16).

**Theory:** Big Five model (Norman 1963; Goldberg 1992). Offline evidence that secure people have more interaction partners (Kanfer and Tanaka 1993) and that socially anxious youths have fewer friends and choose anxious friends (Van Zalk et al. 2011), which the authors say they test online (Sec. 2.1, p. 11). No formal hypotheses are stated beyond expected trait homophily and structural differences.

**Data:** Corpus "Personalitwit2" of about 200,000 posts (Sec. 3, p. 14). Sampled from the Twitter public timeline on 25-28 Dec 2011, plus each sampled user's page (up to 20 recent tweets), so most posts predate the window (p. 13). Retweets excluded. Over 13,000 users and about 7,800 ego-networks in total (public user as centre, related users as edges). English-only subset: 5,392 ego-networks (p. 14). Validation: Personage dataset, 8 balanced users x 20 sentences (Sec. 2.4, p. 12); 18 users (10 neurotic, 8 secure) checked against Analyzewords scores (Sec. 2.4, p. 13). Unit of analysis: post for scoring, user for the personality label (users with one post discarded), ego-network for the structure analysis.

**Method:** Unsupervised personality tagger (Sec. 2.3, pp. 11-13). It uses Pearson correlations from Mairesse et al. 2007 (linguistic cues: exclamation/question marks, emoticons, numbers, long words, word repetition) and Quercia et al. 2011 (following, followers, retweets) in Table 2. A 2,000-post random sample sets the feature averages. Each post yields a secure (s), neurotic (n) or balanced (o) model; the user label is the modal model. Descriptive statistics on the ego-networks, the trait-homophily frequency count (Fig. 4), a posting/retweet/reply comparison (Fig. 5), and Gephi network analysis of neurotic vs calm subgraphs with path length and diameter (Figs. 6-7, p. 15). No inferential tests are reported.

**Identification:** None. The design is descriptive and correlational. The authors do not address endogeneity, confounding or selection, and their own sampling is described as producing many scattered small networks (p. 15).

**Scope:** English-language Twitter users in the public timeline sample, late Dec 2011 (2011 era). Population is not representative: the sampling is seeded from the public timeline. Results are not generalised beyond this sample by the authors.

## Key findings

- Personality tagger accuracy on human-rated Personage data. Numbers: accuracy 0.625 vs majority baseline 0.5; 8 users x 20 sentences; described as in line with state of the art. (Sec. 2.4, p. 12)
- Agreement with Analyzewords proxy scores on 18 Twitter users. Numbers: neurotic: precision 0.8, recall 0.615, F1 0.695; secure: precision 0.375, recall 0.6, F1 0.462; average P 0.587, R 0.607, F1 0.578. (Table 3 and text, p. 13)
- Trait distribution in the English corpus. Numbers: calm 56.1%, neurotic 39.2%, balanced 4.7%. (Sec. 4, p. 14)
- Homophily: users tend to converse with others sharing their trait. Numbers: no coefficients in the text; described as a general tendency. (Sec. 4, Fig. 4, p. 14)
- Neurotic users post and retweet more; calm users are slightly more inclined to reply. Numbers: no values in the text layer. (Sec. 4, Fig. 5, p. 15)
- Neurotic networks have longer chains; calm networks have more mutual ties. Numbers: average path length neurotic 1.551 vs calm 1.334; network diameter 6 (neurotic) vs 5 (calm). (Sec. 4, p. 15)
- Giant component of neurotic network is more complex than calm, despite smaller ego-networks at the user level. Numbers: no further numbers; the claim rests on the diameter/path-length difference above. (Sec. 4 and Fig. 7, p. 15-16; Conclusions p. 16)
- Corpus-level tagger output quality. Numbers: average confidence 0.601; average variability 0.049. (Sec. 3, p. 14)

## Limitations

- Sampling from the public timeline yields many scattered networks of few interactions, so structure is only visible in ego-networks (p. 15).
- The authors say future work should change sampling to capture larger networks (p. 16).
- Validation is small: 8 users (Personage) and 18 users checked manually against a LIWC proxy (pp. 12-13).
- The Analyzewords proxy does not measure the Big Five. The authors map worried to neurotic and upbeat to secure, and they note that upbeat and secure do not correspond perfectly (p. 13).
- Personality labels are unsupervised and derived from correlations in other studies' samples (pp. 11-12); no direct self-report ground truth on Twitter is reported.
- Posts predate the sampling window (p. 13). Retweets are excluded (p. 13).

## Short quotes

- "neurotic users seem to have the tendency to build longer chains of interacting users" (Sec. 4, p. 15)
- "Secure users instead have more mutual connections and simpler networks." (Abstract, p. 10)
- "neurotic users have smaller social networks at the level of a single user, but they tend to build longer chains" (Sec. 5, p. 16)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | method-source | An unsupervised ES tagger built from Mairesse et al. 2007 linguistic correlations (Table 2) scores 0.625 accuracy against a 0.5 majority baseline on Personage (8 users x 20 sentences). The authors call this in line with the state of the art. Prior text classifiers reached E.Stab. accuracy of 0.531-0.705 (Table 1). (Sec. 2.3-2.4, pp. 11-12; Table 1-2) | big5_traits.py scores ES from closed-vocabulary category rates using Mairesse-lineage weights (ES: anx -.30, negemo -.25, i -.20, negate -.15, posemo +.10) and sets N = -ES. The system's held-out CV against RKH for ES is .258, one of its weakest traits. `fec/members/big5_traits.py:181,251` | Shared lineage only: both apply published Mairesse correlates to new text without local supervision. Celli's evidence covers 2011 English tweets from public users and 8 human-rated users. Congressional floor speech and interviews are a different genre and population. The paper does not show that the ES weights transfer, and its accuracy is barely above chance at that sample size. Use it as a historical benchmark for how weak unsupervised ES reading is, never as validation. | weak | UNVERIFIED |
| [[systems/melange-psychometrics#Affective neuroticism slot and N genre-transfer tests\|Affective neuroticism slot and N genre-transfer tests]] | qualifies | Validation against Analyzewords (a LIWC-based proxy, worried = neurotic, upbeat = secure) on 18 users: neurotic P 0.8 / R 0.615 / F1 0.695; secure F1 0.462; average F1 0.578. The authors concede the proxy does not measure the Big Five and that upbeat does not equal secure. (Sec. 2.4, Table 3, p. 13) | affective_n is built from E-residualized, party-controlled LLM affective facets (pessimism, anxiety, depression, volatility). LIWC (i + negemo - posemo) is kept as the independent convergent-validity anchor (r = +.24, gate G3). No gold N criterion exists. `fec/members/emit_affective_n.py:41-45` | Celli's validation is circular: a LIWC-family feature set scored against a LIWC-family proxy, so agreement partly reflects shared method. The system's LIWC anchor is the same kind of convergence. The paper therefore reinforces the system's own caveat (gate tests construct integrity, not validity) and adds no criterion evidence. It does not carry over as support for N validity. | weak | UNVERIFIED |
| [[systems/melange-influence-engine#Receptivity layer (susceptibility, incentive, prospect domain, seed probability)\|Receptivity layer (susceptibility, incentive, prospect domain, seed probability)]] | qualifies | Neurotic users post and retweet more, and calm users reply slightly more (Fig. 5, no values in the text). Neurotic networks form longer chains (average path 1.551 vs 1.334; diameter 6 vs 5), and calm networks have more mutual ties. (Sec. 4, pp. 15-16; Conclusions p. 16) | The receptivity layer puts affective N into susceptibility with a +.30 weight, on the Byrne premise that N degrades decisions under pressure: susc = .40 coop + .30 N - .20 dil - .10 dom. This feeds susc_mult and edge_mult on the cascade. The brain records that no N susceptibility signal has been demonstrated. `fec/members/receptivity_layer.py:179,186,198-199` | Celli measures communication style and ego-network shape, not response to pressure or persuasion. The data are anonymous 2011 public Twitter users with tagger-inferred labels, not legislators. The result gives no support for N as a susceptibility multiplier. The only thing that transfers is a caution: if text-N tracks posting volume, a volume confound could enter N. The system residualizes on E and party but has no stated volume control. That control is untested here. | weak | UNVERIFIED |
| [[systems/melange-psychometrics#Agency-communion persona channel\|Agency-communion persona channel]] | contradicts | Users tend to converse with others who share their emotional-stability trait (trait homophily). This is reported as a frequency pattern in Fig. 4, with no coefficient and no test. (Sec. 4, Fig. 4, p. 14) | Persona homophily was tested on the settled corpus and found UNEARNED (\|r\| <= 0.015 against an earning bar of \|r\| >= 0.05). Persona-homophily edge weighting in cosponsor_route.py stays OFF. `fec/members/persona_homophily_validate.py:7,60-61` | The trait differs (ES vs agency/communion), and so does the population (2011 Twitter users vs members of Congress). The relation (@-conversation) differs from cosponsorship, and Celli gives no inferential test. The system's measured null on its own data outranks this. Do not cite Celli as grounds to reopen homophily routing. | weak | UNVERIFIED |

## Gaps and flags

- No volume control is documented for the text-derived N slot. Celli finds neurotic-tagged users post and retweet more, so a text N built from rates could still co-vary with output volume. affective_n is residualized on E and party only (emit_affective_n.py), and this is untested.
- The psychometrics stack still has no non-LIWC, non-LLM criterion for N/ES. Celli's validation shows the same circularity (a LIWC-family tagger scored against a LIWC-family proxy), so it does not help close the gap.
- The system does not measure network shape by trait (path length, mutual-tie share) on member interaction layers. Even if it did, it would be descriptive only and should not become an influence claim.
- The homophily claim (Fig. 4) has no coefficients and no inferential test. Do not use it to argue for persona- or trait-homophily routing, which the system measured as UNEARNED (|r| <= 0.015).
- Validation is tiny (8 Personage users and 18 Analyzewords users). Any citation of this paper's ES accuracy as support for big5_traits ES/N must carry the sample size and the note that the proxy does not measure the Big Five.
- The paper does not argue for dropping regular cosponsorship edges or for per-edge volume normalization, so neither rejected SNA proposal is implicated.
- Path-length and diameter differences describe Twitter conversation structure. They are not closeness, access or introduction measures (D-025) and must not be carried over as influence language.

Paul has not annotated this paper. It sits in the lineage he already treats as dated: unsupervised correlational trait reading in the Mairesse/LIWC tradition, older than Ramey, which he calls 'dated but foundational'. It is consistent with his reading of Birkenmaier, whose highlights criticize SVM-era, unvalidated text personality measures. For the receptivity layer, Paul wants traits to act as edge parameters on the response to pressure (Oropeza). Celli does not speak to pressure response at all: it is descriptive network shape for anonymous Twitter users. His note that the system's validity record undercuts trait-driven receptivity still stands, and this paper does not change it. His notes predate the persona-homophily null, and Celli's homophily anecdote should not revive that idea.

## Related papers

- [[celli-nd-twenty-years-personality]] — builds-on — Same first author's later review of personality computing. It places this 2012 unsupervised tagger within the field's later threats and validity critiques.
- [[ramey-2016-measuring-elite-personality]] — same-method — Closed-vocabulary Big Five from text with ES reported and N = -ES, the template big5_traits.py follows. Ramey applies it to legislators, Celli to Twitter users.
- [[koutsoumpis-2024-kernel-truth-text]] — complements — Meta-analysis of LIWC-Big Five correlations. It gives the effect-size ceiling for the Mairesse-style cues Celli relies on.
- [[azucar-2017-predicting-personality-traits]] — complements — Meta-analysis of predicting the Big Five from social media footprints. It benchmarks how weak Celli's 0.625 accuracy and 0.578 F1 are.
- [[selfhout-2010-emerging-late-adolescent]] — complements — Offline Big Five friendship-network selection study. It is a stronger, longitudinal test of the trait homophily that Celli only describes.
- [[byrne-2014-chokes-under-pressure]] — complements — Source of the N-under-pressure premise the receptivity layer uses. Celli's N findings concern conversation structure, not pressure response, so the two do not corroborate each other.
- [[pennebaker-2003-psychological-aspects-natural]] — same-method — LIWC word-category foundations (negemo, first-person singular) behind both Celli's cues and the system's affective-N anchor.
- [[kumari-nd-five-personality-predictions]] — same-data — Big Five prediction from Twitter text. It is the regression-based counterpart to Celli's unsupervised Twitter tagger.

### Linked from
_None._