---
type: paper
title: "Measuring Elite Personality Using Speech"
authors: ["Adam J. Ramey", "Jonathan D. Klingler", "Gary E. Hollibaugh, Jr."]
year: "2016"
venue: "Political Science Research and Methods"
doi: "10.1017/psrm.2016.12"
category: "Personality & Psychometrics"
systems: ["melange-influence-engine", "melange-legislative-data", "melange-psychometrics"]
components: ["melange-psychometrics.lexical-big5", "melange-legislative-data.effectiveness-shadow", "melange-psychometrics.behavioral-dispositions", "melange-psychometrics.gold-panel-validation", "melange-psychometrics.affective-n", "melange-influence-engine.receptivity-layer"]
paul_annotated: true
paul_markup_count: 69
evidence_status: "verified"
source: "[[_sources/ramey-2016-measuring-elite-personality.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Machine Learning & Sentiment Analysis\\MeasuringElitePersonalityUsingSpeech.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Machine Learning & Sentiment Analysis\\MeasuringElitePersonalityUsingSpeech.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Machine Learning & Sentiment Analysis\\MeasuringElitePersonalityUsingSpeech.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Machine Learning & Sentiment Analysis\\MeasuringElitePersonalityUsingSpeech.pdf", "C:\\Users\\pgoff\\Downloads\\MeasuringElitePersonalityUsingSpeech (1).pdf", "C:\\Users\\pgoff\\Downloads\\MeasuringElitePersonalityUsingSpeech.pdf"]
sha12: ["5e248be1ab0d"]
tags: [paper, personality-psychometrics]
---

# Measuring Elite Personality Using Speech

**Adam J. Ramey, Jonathan D. Klingler, Gary E. Hollibaugh, Jr. (2016).** *Political Science Research and Methods.* [10.1017/psrm.2016.12](https://doi.org/10.1017/psrm.2016.12). Theme: [[themes/personality-psychometrics|Personality & Psychometrics]].

> [!abstract] Summary
> Ramey, Klingler and Hollibaugh (PSRM 2016) score Big Five traits for every sitting House member from 1996 to 2014. They run each member-year of Congressional Record floor speech through LIWC and MRC features, then through Mairesse et al.'s pre-trained SMOreg models. Scores sit on a 1-7 scale that is relative to the Record. The method adds an optional leave-one-year-out jackknife and 100 sentence-level bootstrap replications per member-year. Above about 5,000 words a year, confidence intervals narrow to about ±0.15. The paper never validates directly against a personality inventory. Its validity evidence is indirect:
> - Lifetime trait means relate to Common Space ideology with the expected signs, but explain only R2 = 0.10 (N = 844).
> - Emotional Stability predicts whether a member ever answered the NPAT. Predicted response rises from about 30% to about 53% between -2 SD and +2 SD (N = 598).
> - Conscientiousness lowers the share of ceremonial bills a member sponsors (-0.39 to -0.58; N = 3,648 member-Congresses in Models 3-5, 104th-112th). A positive Majority × C interaction (+0.27 to +0.29) puts most of that effect in the minority party.
> 
> This is the template for Melange's lexical Big Five. fec/members/big5_traits.py names the paper and copies its steps. lep_criterion.py runs the Table 3 ceremonial-share test as its paper-exact C criterion. The test came back null for all three C measures: rho between +0.000 and +0.018, n = 440 members, 115th-118th Congresses pooled, at least 8 sponsored bills each. That run did not split majority from minority, the fault line the paper says carries the effect, so the null is UNSTRATIFIED. The paper has no ground-truth validation of its scores. The system's convergence against the RKH scores is inflated by shared method (same Mairesse-LIWC family). The paper's speechwriter defence is generic to speeches and does not address the tweets and interviews the system pools in.

## Paul's annotation

From [[annotations/Machine Learning & Sentiment Analysis - Machine Learning & Sentiment Analysis - Annotated Bibliography|Machine Learning & Sentiment Analysis - Machine Learning & Sentiment Analysis - Annotated Bibliography]]:

> Ramey, Adam, et al. “Talking heads: Measuring elite personality using speech.” *SSRN Electronic Journal*, 2015, [https://doi.org/10.2139/ssrn.2605644](https://doi.org/10.2139/ssrn.2605644).
>
> This article, while dated, is foundational for examining elite personality through congressional speeches using the Big Five personality traits. While this article is essentially a recap from more recent studies, it is necessary to look back at the original methodology for the sake of understanding what they are examining, how they code to examine it, and how they ultimately apply the data, which is convenient for this article as the authors use a case study of the data. The main source of the data comes from the auditory and textual transcripts of congressional floor speeches. The case study provides a wealth of insight into future political strategy for fine-tuning policy proposals to be most appealing to the MOC being lobbied. Finally, the main remaining weakness is the lack of a pipeline for other venues of textual analysis not covered in the present article. Additionally, beyond the pipeline itself, a framework specific to the format's contextual or auditory constraints is needed to accurately capture the psychological dimensions—see other articles in the collected corpus.

## Paul's markup in the PDF

- p1 Highlight [yellow]: "We also demonstrate the superiority of text-based measurement over survey-based measurement by showing that personality traits are correlated with survey response rates for members of Congress." — 
- p1 Highlight [green]: "The five-factor, or "Big Five," model has been applied widely in the behavior literature to the study of participation, ideology, and vote choice" — 
- p1 Highlight [yellow]: "Openness to Experience, Conscientiousness, Extraversion, Agreeableness, and Emotional Stability." — 
- p1 Highlight [green]: "as the study of personality among the public has yielded many findings linking personality to ideology" — 
- p2 Highlight [yellow]: "In this paper, we present a method for measuring elite personality without surveys." — 
- p2 Highlight [green]: "using floor speeches," — 
- p2 Highlight [green]: "We show that these estimates parallel findings in the behavior literature linking personality with ideology" — 
- p2 Highlight [yellow]: "and that they are robust to authorship and speechwriting effects." — 
- p2 Highlight [yellow]: "requires a method different from direct surveys." — 
- p2 Highlight [yellow]: "traditional psychometric personality inventories in conjunction with written texts and auditory transcriptions to train predictive models for personality" — 
- p2 Highlight [green]: "Mairesse et al. (2007) train several machine learning models to predict personality traits." — 
- p2 Highlight [yellow]: "An example would be to perform a linear regression of some variable y on a matrix X of covariates for a fraction of a sample of data." — 
- p2 Highlight [yellow]: "sample of data. Then, the coefficients β estimated using the "trai[ning]" ... predicting y using only the X for the unused part of the sample." — 
- p3 Highlight [yellow]: "2004; Bishop 2006; Hastie, Tibshirani and Friedman 2009)—best recovers personality measures in written trials. Thus, we use SMOreg in the analyses that follow." — 
- p3 Highlight [yellow]: "Last, we might be concerned that the subject matter reflected in the Pennebaker essay corpus might not map well to legislator floor speeches." — 
- p3 Highlight [yellow]: "the correlation on the untransformed scale is ~0.99. (p<0.001)" — 
- p4 Highlight [yellow]: "process the raw text through LIWC and MRCPD to get counts of word usage across all LIWC and MRCPD categories." — 
- p4 Highlight [green]: "Specifically, all LIWC and MRCPD features are standardized and then fed in to the standardized SMOReg models." — 
- p5 Highlight [yellow]: "This ensures that personality trait estimates in session t are not corrupted by language associated with particular actions in that session" — 
- p5 Highlight [yellow]: "simply because the act of cosponsorship inherently entails using Agreeable language). In short, this correction alleviates concerns of endogeneity." — 
- p5 Highlight [yellow]: "The scale for each trait ranges from 1 to 7." — 
- p5 Highlight [yellow]: "Former Representative Ron Paul (R-TX) is significantly more Open and less Emotionally Stable than the rest of the legislators in this subsample." — 
- p6 Highlight [yellow]: "Whether this clustering reflects the desire of legislators to make themselves look more appealing to constituents or the natural self-selection of certain personality types into elected office has not yet been determined." — 
- p7 Highlight [green]: "We acknowledge legislators may have incentives to misrepresent their "true" personality traits and adopt false personas." — 
- p7 Highlight [pink]: "we do not believe it is a serious impediment to the measurement of legislators' beliefs and/or preferences, as exhibited through the Big Five personality traits." — 
- p7 Highlight [pink]: "is that the "desired" trait or answer is transparent" — 
- p8 Highlight [yellow]: "legislators may consciously adopt speech patterns in order to signal these valence characteristics." — 
- p8 Highlight [yellow]: "and are known to all and can be voluntarily suppressed or performed insincerely." — 
- p8 Highlight [green]: "We suppose legislators also have unconscious verbal cues that signal personality and that the algorithm can read them." — 
- p8 Highlight [yellow]: "the unknown subset of cues should exhibit variation as legislators are simply unaware of their link with personality." — 
- p8 Highlight [yellow]: "Strong and consistent relationships have been uncovered between ideology and Openness, Conscientiousness, and Emotional Stability, with the former being associated with liberalism" — 
- p8 Highlight [yellow]: "the latter two associated with conservatism." — 
- p8 Highlight [yellow]: "Finally, they find a positive relationship between Extraversion and conservatism, though the relationship is weaker than those between ideology and the other four traits." — 
- p8 Highlight [yellow]: "Using Common Space (Poole and Rosenthal 1997) scores, we can examine the direction of the relationships between the Big Five traits and ideology." — 
- p9 Highlight [yellow]: "Finally, the literature is much more mixed on the relationship between ideology and Extraversion; as such, we remain agnostic regarding the expected sign (or significance)" — 
- p9 Highlight [green]: "several ordinary least squares (OLS) models of ideology as a product of personality (as measured by the lifetime means of each trait) and demographic traits." — 
- p9 Highlight [yellow]: "Moreover, these results allay one natural concern with using potentially ideologically-tinged legislative speeches to estimate personality, in that our personality estimates may be simply summaries of legislator ideology." — 
- p10 Highlight [yellow]: "the magnitudes of these are not large. This suggests whatever theoretical concerns one might have about the dependence of the Big Five on ideology or the ideological content of the legislative record are, in practice, not a problem." — 
- p10 Highlight [yellow]: "Figure 4 presents the yearly correlations between the Big Five and one feature per trait. The correlations are remarkably stable over time and generally are distinct from 0 at the 95 percent level." — 
- p11 Highlight [yellow]: "Conscientious members use future tense words at higher rates. Similarly, more Extraverted members should be more sociable and talkative, and they do speak more in our data." — 
- p11 Highlight [yellow]: "more Emotionally Stable members should exhibit less outward anxiety; again, this relationship is found in our data, though it is somewhat weaker than those for Openness, Conscientiousness, and Extraversion." — 
- p11 Highlight [yellow]: "A cursory look at the House disbursement records demonstrates that, outside of the Speaker and a few key members, speechwriters are simply not hired by most members." — 
- p11 Highlight [yellow]: "it is reasonable to assume that many staffers would contribute to many speeches and, if so, our estimates should be unstable or clustered for all members." — 
- p11 Highlight [green]: "Congressional speechwriters should make every effort to become familiar with the speaking style of the Member for whom they are writing, and adjust their drafts accordingly." — 
- p11 Highlight [green]: "Congressional Research Service admonishes those individuals charged with speechwriting for Congressmembers to make every effort to mimic the language and diction of the member for whom they are writing" — 
- p12 Highlight [yellow]: "they are simply variations on a theme." — 
- p12 Highlight [yellow]: "and the difference is still minimal, there should be even less of an effect for members of Congress." — 
- p12 Highlight [yellow]: "However, this approach has limitations. Most important for our purposes here is that the missingness must obey the so-called missing at random (MAR) assumption, which is satisfied if missingness can be modeled as a function of observed data." — 
- p13 Highlight [yellow]: ""respondents may have been concerned about social desirability, leaving them unwilling to admit to possessing certain traits"" — 
- p13 Highlight [yellow]: "We therefore estimate a series of logistic regression models where the dependent variable equals 1 if the member in question ever responded to the NPAT, and 0 otherwise." — 
- p13 Highlight [green]: "(absolute value of their DW-NOMINATE score)." — 
- p13 Highlight [yellow]: "This finding provides strong evidence that the propensity to respond to surveys on which social desirability may be a factor is at least in part a function of personality traits." — 
- p14 Highlight [yellow]: "This increase is enormous and suggests estimates of personality from surveys of elites are highly susceptible to personality-induced selection bias." — 
- p14 Highlight [yellow]: "given that they are more personally intrusive than policy-specific surveys like the NPAT" — 
- p15 Highlight [yellow]: "NPAT and do not offer legislators the ability to take policy positions on important issues of the day." — 
- p15 Highlight [yellow]: "determining the types of bills legislators propose." — 
- p15 Highlight [yellow]: "Finally, we present a substantive application of these scores to the legislative domain—" — 
- p15 Highlight [yellow]: "However, different types of bills also face different hurdles, and more substantive bills are at greater risk of being prevented from reaching the floor due to partisan concerns" — 
- p15 Highlight [violet]: "and they tend to be more driven, goal-oriented, uptight, organized, and have more willpower" — 
- p16 Highlight [yellow]: "Members of the minority will have to work harder and expend more effort shaping policy-relevant bills to the liking of the majority" — 
- p16 Highlight [violet]: "More Conscientious members of the minority, however, should be less deterred by the partisan obstacles in their paths—as they place more weight on future payoffs—and should be similar to highly Conscientious members of the majority." — 
- p16 Highlight [yellow]: "along with 95 percent confidence intervals." — 
- p18 Highlight [violet]: "For example, Conscientiousness seems to least affect those in the majority" — 
- p18 Highlight [pink]: "This paper demonstrates measurement of elite personality is possible and that estimates are consistent and robust over time." — 
- p18 Highlight [yellow]: "requires careful consideration of how personality fits into standard frameworks of elite decisionmaking." — 
- p18 Highlight [yellow]: "First of all, while we believe text-based measures of elite personality are more appropriate for the study of legislative behavior than survey-based measures, there are a number of ways in which the strengths of survey-based methods may be used to enrich the project." — 
- p18 Highlight [violet]: "demonstrated that survey methods for recovering measures of elite personality are problematic at best" — 
- p18 Highlight [violet]: "the Big Five do indeed play a role in legislative decisionmaking." — 
- p19 Highlight [green]: "For the first time, personality scores within the five-factor model are available for virtually all members of the US Congress going back to 1996. With these tools, established findings from personality psychology and the growing literature on personality in political behavior may be applied to develop new theories of legislative behavior." — 

## Study

**Question:** Can Big Five personality traits of US legislators be measured from floor speech text without surveys, and are the resulting estimates valid and substantively useful? Specifically: (a) do text-based estimates parallel known personality-ideology links, (b) do survey nonresponse rates depend on personality, and (c) does Conscientiousness predict the share of symbolic/ceremonial bills proposed, conditional on majority status.

**Theory:** Big Five (Openness, Conscientiousness, Extraversion, Agreeableness, Emotional Stability) personality is related to political behavior and ideology (Gerber et al. 2010, 2011a; Mondak 2010; Jost, Nosek and Gosling 2008). Survey inventories suffer from low response and social desirability, so personality should be inferred from language (Mairesse et al. 2007). Expected ideology signs: Openness, Conscientiousness and Emotional Stability negative toward liberalism; Agreeableness negative to Common Space score; Extraversion agnostic. Symbolic-bill hypothesis: Conscientious legislators propose fewer ceremonial bills because they are goal-oriented and persist against partisan hurdles (Cox and McCubbins 2005; Aldrich 1995); the effect should be larger for the minority, where free-riding on majority leadership is unavailable. Authors also argue Conscientiousness acts as a discount factor (fn 35).

**Data:** Sources: Congressional Record floor speeches by every sitting House member, 104th-113th Congresses (1996-2014). Unit of measurement: member-year (scores estimated per year, then jackknifed across years). Sample for personality scores: all incumbent House members in the period; the paper does not report total speech counts. Table 1: N = 844 House members (lifetime mean personality, 1996-2014 Common Space). Table 2: NPAT response, N = 598 members (622 in Model 1), pooled by member. Table 3: House bills proposed in 104th-112th Congresses, binomial logit at member-Congress level, N = 3,648 (Models 3-5; 3,736 and 3,732 in Models 1-2). Training data for the pre-trained model: Pennebaker and King (1999) essays (~1.9 million words) and Mehl, Gosling and Pennebaker (2006) conversations (~100,000 words), per Mairesse et al. (2007). Median member uttered ~11,000 words per year (p.7).

**Method:** Pre-trained Mairesse et al. (2007) SMOreg (SVM regression, Weka) applied to LIWC (2001) and MRCPD features, standardized, in corpus mode: (1) process each member-year's speeches through LIWC and MRCPD; (2) apply the standardized SMOreg models; (3) save Big Five estimates per year; (4) optional jackknife (average of all other years) to reduce language tied to same-year actions; (5) uncertainty from 100 sentence-level bootstrap replications per member-year (Lowe and Benoit 2011), 95% CIs. Scores are on a 1-7 scale and relative to the Congressional Record, not comparable to general-population inventories (fn 10). Validity is indirect: (a) OLS of Common Space scores on lifetime trait means with demographics (Table 1); (b) yearly correlations of traits with LIWC features (Fig 4); (c) authorship/speechwriter checks (Pelosi, Boehner, Ron Paul; p.11); (d) logistic regression of ever responding to NPAT on traits and controls (Table 2, Fig 5); (e) binomial logit of ceremonial-bill share with Majority Party x trait interactions and Congress FE in Model 5 (Table 3, Fig 6). Bill classification follows Volden and Wiseman (2014) title keywords.

**Identification:** No causal identification strategy. The authors explicitly refrain from claiming causal priority between personality and ideology (fn 20, p.9). Jackknife correction (step 4, p.5) is presented as alleviating endogeneity from same-year action language. The NPAT and bill analyses are conditional associations; the bill model adds party interactions and (Model 5) Congress fixed effects. Selection into survey response is treated as a confound to be shown empirically rather than solved.

**Scope:** Population: incumbent US House members, 1996-2014 (104th-113th Congress) for personality scores; bill analysis covers House bills in 104th-112th Congresses. Institution: US House floor speech (Congressional Record). Era: post-1996 only; pre-1996 Record not digitized (p.19). Excludes Senate, challengers, state legislators, and presidents (presidents discussed only in an argument, p.11-12). The paper states the method may generalize to other text sources but these are not tested.

## Key findings

- Text-based Big Five estimates are stable and precise for incumbent House members over time; uncertainty shrinks with word count. (p.5 (Fig 2 discussion) and p.7 (Fig 3)) — Median member ~11,000 words/year. Above ~5,000 words, CI width about 0.3 or less (point estimate +/-0.15 on 1-7 scale). Average CI width ~0.35 (+/-0.175).
- Congressional floor speech LIWC category usage closely matches the Pennebaker laboratory corpus used to train the model, so the transfer is judged acceptable. (p.3, Fig 1) — Correlation ~0.99 on untransformed scale; Kendall tau-B = 0.72 for rank orderings (p<0.001).
- Personality estimates show the expected relationships with Common Space ideology; the model explains only a modest share of ideology variance. (p.8-9, Table 1) — N = 844. Model 1: Openness -0.26***, Conscientiousness 0.26***, Extraversion -0.15***, Agreeableness -0.17**, Emotional Stability 0.20***; R2 = 0.10. Model 4 (with Female and Birth Year): R2 = 0.16, all four expected-sign traits significant (Agreeableness at p<0.05 in Model 4).
- Emotional Stability predicts response to the National Political Awareness Test (NPAT), indicating survey-based elite personality measures suffer selection bias. (p.13-14, Table 2, Fig 5) — N = 598 (622 in Model 1). Emotional Stability: 0.47* (M1), 0.49* (M2), 0.62** (M3), 0.58** (M4, SE 0.28). Moving Emotional Stability from -2 SD to +2 SD raises predicted response probability from about 30% to about 53% (Model 4).
- Conscientiousness reduces the share of ceremonial/symbolic bills proposed, and this effect is smaller for majority-party members. (p.15-18, Table 3, Fig 6) — Conscientiousness: -0.52*** (M1), -0.39*** (M3), -0.53*** (M4), -0.58*** (M5, with Congress FE). Majority Party x Conscientiousness: +0.27** (M4), +0.29*** (M5). Majority Party x Extraversion: -0.33*** (M4). N = 3,648 (M3-M5). Authors: Conscientiousness affects majority members least (p.17-18); minority members must be highly Conscientious to propose substantive bills.
- Speechwriter concern is judged a non-issue: estimates stay stable through Speaker changes and across members with and without speechwriters. (p.11-12) — Speechwriters not hired by most members per House disbursement records (qualitative). Pelosi and Boehner estimates stable even though neither was Speaker at the start of the data.
- Text-based validity checks support the measure, but the paper provides no direct validation against a ground-truth personality inventory for legislators. (p.6-7 (Face Validity), p.18 (Conclusion)) — None; validity is indirect (ideology correlations, LIWC face validity, NPAT, bills).

## Limitations

- Strategic misrepresentation: legislators may adopt false personas or speech patterns that are known to listeners and voluntarily controllable (p.7-8). Authors argue this would attenuate, not invalidate, results.
- Validity is indirect only; no direct ground-truth personality measure for legislators exists (p.6-7).
- Scores are relative to the Congressional Record and not comparable to general-population inventories such as the TIPI (fn 10, p.4).
- Training corpus is nonpolitical lab essays and conversations; similar LIWC category usage does not establish similar word use (p.3, fn 10).
- Self-selection of members into elected office makes their trait distribution differ from the general population (fn 10, p.4).
- Speechwriter effects are assumed small based on disbursement records and a Reagan analogy; not directly tested for most members (p.11-12).
- Missing data: survey-based approaches fail under MAR assumption when missingness depends on personality (p.12).
- NPAT response is a low bar, which biases against finding significant results (p.13); the outcome is ever-response only, with member-level means.
- Ron Paul example and face validity rely on the authors' interpretations of trait stereotypes (p.5).
- Causal direction between personality and ideology is not resolved (fn 20, p.9).
- Pre-1996 Congressional Record not digitized; estimates not available for earlier periods (p.19).
- Other text sources (challengers, social media, press releases) not tested; comparability unknown (p.18, fn 39).
- Yearly estimation chosen partly for computing constraints; Congress-level estimates would take weeks to months (fn 11, p.4).
- Reviewer-observed inconsistency: Table 3 note says observations are at the Congressmember level, while the text says member-Congress dyads; the bill sample is 104th-112th Congresses while scores cover 1996-2014.

## Short quotes

- "This paper demonstrates measurement of elite personality is possible and that estimates are consistent and robust over time." (p.18, Conclusion (first paragraph))
- "This increase is enormous and suggests estimates of personality from surveys of elites are highly susceptible to personality-induced selection bias." (p.14, Survey vs speech section, after NPAT logit results)
- "Conscientiousness seems to least affect those in the majority" (p.18, Symbolic bills application)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-psychometrics#Lexical Big Five (Ramey-style closed vocabulary)\|Lexical Big Five (Ramey-style closed vocabulary)]] | method-source | SMOreg on standardized LIWC and MRCPD features per member-year; scores relative to the Congressional Record (fn 10); 100 sentence-level bootstraps per member-year; CI width about 0.3 or less above about 5,000 words a year (median member about 11,000 words); optional jackknife over other years. (pp.3-7, Figs 2-3, fn 10-11) | big5_traits.py follows the steps: corpus-standardized features and z-scores relative to this corpus (about lines 236-251); default speech sources CREC and INTERVIEW (lines 36-40); lowconf below LOWCONF_TOKENS=1500 (lines 28, 251); N = -ES (line 22). The docstring (lines 13-16) also says the license-bound SMOreg file is replaced by published-correlate linear weights, a fourth deviation the analyst omitted. The RKH refit exists in fit_smoreg_rkh.py (5-fold held-out r), but no stored output with the .350 figure was found. `fec/members/big5_traits.py:3-23,28,36-40,236-285; fec/members/fit_smoreg_rkh.py` | Transfers as a procedure only. The paper's precision band assumes about 11,000 words per member-year; the 1,500-token floor is far below the paper's 5,000-word band, so the precision claim does not transfer. Refitting to RKH makes RKH convergence a fit statistic, not validation. | strong | VERIFIED-SOFTENED |
| [[systems/melange-legislative-data#Five-stage legislative effectiveness shadow and CEL criterion\|Five-stage legislative effectiveness shadow and CEL criterion]] | qualifies | Conscientiousness lowers the ceremonial share of sponsored bills: -0.52 (M1), -0.39 (M3), -0.53 (M4), -0.58 (M5, Congress FE), all p<0.01. Majority x C is +0.27 (M4) and +0.29 (M5), so the C effect is smallest in the majority. N = 3,648 member-Congresses in M3 to M5, 104th to 112th Congresses; binomial regression with implicit weighting by bills proposed (fn 37). (pp.15-18, Table 3, fn 37, Fig 6) | lep_criterion.py computes Spearman rho between pooled ceremonial share (115th to 118th Congresses, at least 8 bills) and diligence_z, lexical C v1 and behaviour-calibrated C v2. Stored data/members/lep_criterion.json: rho 0.0026, 0.0176 and 0.0003, n = 440, all null. There is no majority split. The script's own comment (lines 127-128) says the test is attenuated. `fec/members/lep_criterion.py:5-10,31-32,45-59,89-100,125-128; data/members/lep_criterion.json` | The paper's effect is minority-concentrated, so a pooled rank correlation is an UNSTRATIFIED test of a conditional claim. Modern Congresses carry few ceremonial bills, which attenuates the test. A stratified, bill-weighted re-run would be new analysis. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Behavioural dispositions\|Behavioural dispositions]] | contradicts | The 2016 paper's only Conscientiousness outcome is the ceremonial-bill share (Table 3). It has no roll-call participation analysis; roll calls appear only in passing as a source of ideal points (p.5). (pp.5, 15-18, Table 3; full-text search found no participation outcome) | behavioral_dispositions.py docstring (lines 13-14) calls diligence_z, which is roll-call participation, RKH's strongest C criterion. diligence_z is now the mean of participation and LEP follow-through (lines 49-71). `fec/members/behavioral_dispositions.py:13-14,49-71` | The 2016 text does not support the attribution. Whether another RKH work (the 2015 manuscripts cited at pp.15 and 19) uses participation cannot be checked from this text. Treat as a citation fix, not a modelling finding. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Big Five validation against Senate gold panels and criteria\|Big Five validation against Senate gold panels and criteria]] | qualifies | The paper has no direct validation against ground-truth personality measures for legislators. Its validity rests on ideology correlations (Table 1 Model 1, R2 0.10, N 844), LIWC face validity, NPAT response and bill shares. For NPAT, Emotional Stability moves predicted response from about 30 to about 53 percent between -2 and +2 SD (Table 2 Model 4, N 598). The paper says this suggests selection bias; it does not show it. (pp.6-7, 9, 12-15, 18 (Tables 1-2, Fig 5)) | The analyst's description of Senate expert and insider panels, pre-registered single looks and the E/A/O/N/C pattern could not be verified. Brain topic lines 25-32 cite RKH results, not panels. The cited code path is wrong; the file is at fec/members/big5_improvements_eval.py and contains no panel code found by grep. `fec/members/big5_improvements_eval.py (path cited without members/)` | Senate panels against a House paper is a cross-chamber assumption. The paper's NPAT argument concerns self-report surveys, not expert ratings, so it does not directly apply to the panels. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-psychometrics#Affective neuroticism slot and N genre-transfer tests\|Affective neuroticism slot and N genre-transfer tests]] | qualifies | Emotional Stability's link to anxiety words is somewhat weaker than those for Openness, Conscientiousness and Extraversion (p.11). Agreeableness is also described as weaker (p.11). ES predicts survey response (Table 2) and loads 0.20 (p<0.01) on Common Space in Table 1 Model 1. (p.11, Table 1 Model 1, Table 2) | N = -ES is carried over (big5_traits.py:22). Affective N is E-residualized and party-controlled (emit_affective_n.py:39-59). Brain topic record 6 reports raw r -0.11 (E-cleaned -0.04, not significant) against RKH-ES at n 147, and broad text-N -0.22. The interview-to-floor rho values (.050, .161) were not checked. `fec/members/emit_affective_n.py:39-59; fec/members/big5_traits.py:22` | The paper's weak ES-anxiety link is a face-validity remark, not a ranking of ES as weakest. Party-residualizing may remove ES signal that the paper's ideology check carries. | weak | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Receptivity layer (susceptibility, incentive, prospect domain, seed probability)\|Receptivity layer (susceptibility, incentive, prospect domain, seed probability)]] | suggests-extension | Conscientiousness acts like a discount factor (fn 35), but fn 35 attributes that model to a separate Ramey, Klingler and Hollibaugh 2015 manuscript. Within this paper, more Conscientious minority members are less deterred by partisan obstacles because they weight future payoffs (pp.15-16); the C effect is smallest for the majority (p.18). (pp.15-18, fn 35, Table 3) | receptivity_layer.py: behavioural basis SUSC_z = .40 coop + .30 N - .20 diligence - .10 dominance (line 173); lexical basis .40 A + .30 N - .20 C - .10 E (line 186). Neither branch has a majority-status term. The code comment calls the weights local calibration, not a design prior. `fec/members/receptivity_layer.py:12,173,186,189` | The paper concerns bill-type choice, not response to lobbying pressure, so transfer is analogical. Conditioning C on majority status would be a new proposal with no validation behind it. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Year: the item and summary say 2016 (DOI 10.1017/psrm.2016.12; text header 2016). Birkenmaier cites the same article as PSRM 7(1) 2019 pp.163-184 (print). Keep 2016 as online date and confirm citation metadata before filing.
> - Summary issue: summary_md says the paper gives no grounds for treating the published RKH scores as ground truth. The paper never discusses RKH scores; it only promises to release its own scores on publication (fn 16). The sentence should say the paper has no ground-truth validation of its scores.
> - Summary issue: summary_md says the speechwriter defence covers floor speech only and not tweets or interviews. That exclusion is the analyst's inference; the paper's defence is generic to speeches and does not address other genres.
> - Summary issue: summary_md gives Table 3 N = 3,648 for the whole table. It applies to Models 3 to 5; Model 1 has 3,736 and Model 2 has 3,732.
> - Summary issue: summary_md says the lexical C criterion reversed on roll-call participation. Only the behavioral_dispositions.py docstring says this; no stored result was checked.
> - Summary issue: summary_md says E and A pass both panels, O academics only, N insiders only and C insider only. Not verified in the cited files.
> - Summary issue: summary_md claims about Paul's pink and violet highlights and a conscientiousness note were not verified; the markup section is empty and the annotation does not mention them.
> - Summary issue: The .350 held-out CV r cited for the RKH refit was not found in any stored output in the Melange repo; fit_smoreg_rkh.py prints cv_r but no saved result was located.
> - Summary issue: Related links: all eight slugs resolve to real item files. Only Birkenmaier's critique was checked against text. Koutsoumpis, Byrne, Amsalem and Lechner are not cited in the Ramey text, so their links are analyst additions.

## Gaps and flags

- No per-year estimation and no leave-one-year-out jackknife: lexical Big Five pools all available text, so trait scores can carry language tied to the same actions they are later used to predict (diligence, sponsorship).
- No bootstrap uncertainty on trait scores: a binary lowconf flag at 1,500 tokens replaces the sentence-level bootstrap CIs, and 1,500 tokens is well below the paper's roughly 5,000-word precision band.
- The RKH ceremonial-share criterion is pooled across majority and minority members and is not bill-weighted. The paper's effect is minority-concentrated (Majority × C +0.27 to +0.29), so the stored null (n = 440, 115th-118th) is UNSTRATIFIED and cannot refute or confirm it.
- Genre mixing (CREC, interviews, social) has no counterpart to the paper's speechwriter defence. The defence rests on House disbursement records for floor speech and does not address other genres.
- Weights refit to RKH targets make RKH convergence a fit statistic, not validation (the .350 held-out CV mean r was not found in any stored output). The paper offers no ground-truth anchor that would rescue it.
- Mis-citation: behavioral_dispositions.py:13-14 calls roll-call participation "RKH's strongest C criterion", but the paper has no participation analysis. Its C criterion is the ceremonial-bill share. This is the second RKH mis-sourcing, after the committee-chair criterion recorded at lep_criterion.py:7.
- Pooled-coefficient rule: any quote of the lep_criterion ceremonial-share result must carry the label UNSTRATIFIED (no majority/minority split), with window 115th-118th and denominator n = 440 members with at least 8 sponsored bills.
- The paper's own claim of 'superiority of text-based measurement' (highlighted by Paul) is not established for this system. The system's RKH-referenced convergence is method-shared,. External claims need status labels per the brain discipline.
- Paul's annotation says the data are 'auditory and textual transcripts' of floor speeches, but the paper uses Congressional Record text only.
- big5_traits.py:276 hard-codes an expected Extraversion sign of -1. The paper explicitly stays agnostic on Extraversion (p.9), so a 'REVERSED' verdict on E is not a paper-based failure.

Paul's position calls Ramey 'dated but foundational' and asks for a framework specific to each format's contextual or auditory constraints. That matches what the system built: a step-by-step synthesis of Ramey, then a Birkenmaier-motivated move to LLM and behavioural readers, with genre-transfer tests that failed for N.  The system's own record is more sceptical. The paper-exact C criterion came back null on the current roster (UNSTRATIFIED, n = 440, 115th-118th), the behavioral_dispositions.py docstring says lexical C reversed on roll-call participation (no stored result was checked), and the receptivity layer moved to a behavioural basis. The one untested refinement is a run stratified by majority status, the minority-party effect the paper reports.

## Related papers

- [[birkenmaier-2025-measuring-politicians-public]] — contradicts — Birkenmaier's item text (highlight p.5 and p.8) calls Ramey's pretrained SVM outdated and unvalidated against human annotation, and uses it as a baseline. Verified.
- [[mondak-nd-online-appendix-personality]] — complements — Mondak 2010 is cited in Ramey for personality-ideology findings (pp.8-9). The claim that its expert ratings supply legislator ground truth was not checked against the item.
- [[koutsoumpis-2024-kernel-truth-text]] — extends — Not cited in the Ramey text, so this link is analyst opinion; the meta-analysis claim was not checked.
- [[pennebaker-2003-psychological-aspects-natural]] — builds-on — Ramey's LIWC base is Pennebaker and King 1999 and LIWC 2001 (refs). Pennebaker 2003 is not cited; the link is looser than stated.
- [[bucchianeri-2025-legislative-effectiveness-american]] — same-data — Ramey's Table 3 outcome uses Volden and Wiseman 2014 ceremonial coding (p.16 and refs). Whether Bucchianeri uses the same coding was not checked.
- [[byrne-2014-chokes-under-pressure]] — complements — Not cited in the Ramey text; the pressure-mechanism link is analyst opinion and was not checked.
- [[amsalem-2018-effect-politicians-personality]] — extends — Not cited in the Ramey text; the media-visibility parallel is analyst opinion and was not checked.
- [[lechner-2025-mapping-political-personality]] — complements — Not cited in the Ramey text; the ideal-versus-actual personality point is analyst opinion and was not checked against the item.

### Linked from
- [[fouirnaies-2022-electoral-incentives-affect]] — complements — Ramey links text-measured Conscientiousness to bill-sponsorship behavior (a lower share of ceremonial bills, Table 3). Fouirnaies & Hall show sponsorship and participation also move with electoral incentive inside the same person, a situational confound for any trait reading of those behaviours (diligence_z, the C criterion).
- [[marrero-2022-evaluating-voice-samples]] — complements — Measuring Elite Personality Using Speech; Ramey is the basis named in big5_traits.py (Ramey-synth). The paper itself does not cite it.
- [[celli-nd-twenty-years-personality]] — same-method — Ramey applies the Mairesse/LIWC Recognition pipeline the survey reviews as the field's text core; big5_traits.py copies it. Item exists; description not checked beyond existence.
- [[azucar-2017-predicting-personality-traits]] — complements — Legislator speech scoring (title verified). The item confirms there is no ground-truth personality inventory for legislators, which is the gap Azucar's lay-user evidence does not fill.
- [[dietrich-2019-pitch-perfect-vocal]] — complements — Measures legislator traits from floor-speech text; Dietrich studies a vocal state. Real item; the text-versus-voice contrast holds.
- [[naz-2025-machine-deep-learning]] — complements — The system's lexical reader copies Ramey's pipeline for legislators. The survey covers the general-population ML lineage, which does not address elites.
- [[salam-2022-learning-personalised-models]] — complements — Verified: Ramey estimates Big Five from legislators' floor speech with LIWC and MRCPD closed-vocabulary features. Salam uses non-verbal and turn channels with subgroup models on self-report labels. Link kept.
- [[rubio-2024-feasibility-data-analytics]] — complements — Real item (title: measuring elite personality using speech). The rel's claim that Ramey works from words rather than acoustics was not verified.
- [[soto-2017-next-five-inventory]] — complements — Speech-based Big Five estimates for elite members, the approach the lexical reader follows. The BFI-2 adds hierarchical facets and self-report benchmarks. Title confirmed.
- [[kumari-nd-five-personality-predictions]] — same-method — Closed-vocabulary trait measurement from speech, applied to legislators. This is the elite-population version of the pipeline this paper sketches for general Twitter users.
- [[celli-2012-role-emotional-stability]] — same-method — Closed-vocabulary Big Five from text with ES reported and N = -ES, the template big5_traits.py follows. Ramey applies it to legislators, Celli to Twitter users.
- [[yang-2023-getting-personal-deep]] — complements — Ramey's closed-vocabulary LIWC plus SMOreg-style pipeline is the template named in the big5_traits.py header. Yang does not cite Ramey and does not test elite speech, so it neither contradicts nor confirms Ramey. Its feature-based rows rank lowest on non-elite self-report criteria. Item existence checked; Ramey content not read.