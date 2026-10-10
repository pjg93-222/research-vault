---
type: paper
title: "Is Bigger Always Better? Potential Biases of Big Data Derived from Social Network Sites"
authors: ["Eszter Hargittai"]
year: 2015
venue: "The Annals of the American Academy of Political and Social Science, vol. 659 (May 2015), pp. 63-76. Special issue: Big Data Methodological Challenges."
doi: "10.1177/0002716215570866"
category: "Causal & Methodological"
systems: ["melange-legislative-data", "melange-sna", "melange-stance"]
components: ["melange-sna.multilayer-social-press-edges", "melange-stance.media-blend", "melange-legislative-data.press-collector-cap"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/hargittai-2015-bigger-always-better.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Downloads\\hargittai-2015-is-bigger-always-better-potential-biases-of-big-data-derived-from-social-network-sites (1).pdf", "C:\\Users\\pgoff\\Downloads\\hargittai-2015-is-bigger-always-better-potential-biases-of-big-data-derived-from-social-network-sites.pdf"]
sha12: ["988ba27f401d"]
tags: [paper, causal-methodological]
---

# Is Bigger Always Better? Potential Biases of Big Data Derived from Social Network Sites

**Eszter Hargittai (2015).** *The Annals of the American Academy of Political and Social Science, vol. 659 (May 2015), pp. 63-76. Special issue: Big Data Methodological Challenges..* [DOI](https://doi.org/10.1177/0002716215570866). Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> Hargittai asks whether people choose social network sites at random. They do not. In the 2013 Pew national survey (N = 1,801; regression N about 1,510-1,528), age, education, income, gender and race each predict use of at least one of Facebook, LinkedIn and Twitter. For example, the LinkedIn log-odds coefficient (full sample, N 1,510) on logged income is 0.72 and on high school or less is -1.37. In a panel of young adults in one required first-year UIC class (2009 wave n = 547; 2009 to 2012, regression N about 517-519), 2009 Internet skills and use frequency predict which sites people adopted by 2012 (skills to Tumblr 0.62, Twitter 0.33). Adoption differs widely by site (2012 wave, 52% return, no N given in Table 3: Facebook 89%, Twitter 38%, LinkedIn 18%). Her conclusion is that data from one site describe that site's users only. A biased sampling frame may come to the exact opposite conclusion, not just add noise. Studies should state their frame in the title, abstract and results.
> 
> **What this means here.** My inference, not the paper's (it never discusses institutional accounts): legislators are elite accounts, so platform adoption bias may mostly disappear on X, where 502 of 515 members in the social_media.db members table have an X handle. It may come back on Bluesky. Bluesky posts run 2023-07-01 to 2026-09-19. Of the 533 roster members (259 D, 271 R, 3 I), 135 posted on Bluesky (posting, not account adoption; the Bluesky handle field is populated for all 515 rows): 117 D, 16 R and 2 I. Of the 411 Bluesky member-to-member mentions, 392 are D to D, 8 D to I, 5 I to D and 6 R to R; none go between D and R, but 13 cross the D/I line. These mentions are added at weight 0.4 to the social interaction layer, which enters the live augmented cascade network. The second problem is the media blend: a member silent on social media in a category gets half their press signal, so silence counts as zero stance. Both counts are my own reading of live files. The paper's coefficients do not carry over to this system. Only its sampling-frame logic does.

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** Are people's choices to use specific social network sites (SNSs) random with respect to demographics, socioeconomic status and Internet experience? If not, can big data studies that sample only users of one SNS generalize their findings to the broader population or to users of other SNSs?

**Theory:** No named theory. The argument rests on sampling-frame logic: a site's users are the sampling frame, non-users are excluded by design, and generalization requires that selection into the site be unrelated to the outcomes studied. The paper also invokes selection on the dependent variable as a second source of bias.

**Data:** Dataset 1 (national): Pew Internet Project survey collected 2013, N = 1,801 adults (80.2% use the Internet); landline and cellular random-digit-dial sample; average age 52, half male, 70% white. Regression N ranges 1,510-1,528 (full sample) and 1,231-1,249 (Internet users only), per Table 1.
Dataset 2 (panel): Author's panel of young adults at the University of Illinois at Chicago (UIC), a required first-year class. Wave 1 spring 2009, n = 547 (80.5% of enrolled students); wave 2 summer 2012 by postal mail, 52% of wave 1 returned valid responses. Regression N = 517-519 (Table 4). Group is 61% women, 44% white non-Hispanic, 24% Asian American, 23% Hispanic, 8% African American; ages 21-22 in 2012; 50% of parents have a college degree or more. Paper-pencil instruments in both waves.

**Method:** Separate logistic regressions for each SNS.
Pew analysis (Table 1): outcome is current use of Facebook, LinkedIn or Twitter (cross-sectional, 2013). Predictors: age, gender, race/ethnicity, education, logged income. Run on full sample and on Internet users only.
Panel analysis (Table 4): outcome is 2012 adoption of Facebook, Twitter, Google+, LinkedIn or Tumblr (MySpace dropped for low uptake). Predictors are 2009 measures: gender, race/ethnicity, parental education, logged use years, logged user autonomy (access locations), use frequency, and Internet skills. Internet skills index = mean of 27 Internet terms rated on a 5-point scale (Cronbach's alpha = .94). Descriptive usage trends are in Table 3.

**Identification:** Temporal ordering only. Panel predictors are measured in 2009 and outcomes in 2012, so the author argues that skills and experiences predate most SNS adoption (Table 3 shows Twitter at 3% in 2009). This is offered as a partial answer to reverse causality from SNS use to skills. No instrument, experiment or quasi-experimental design is used. The Pew analysis is cross-sectional and carries no such ordering.

**Scope:** Population: US adults (Pew, 2013) and a single-institution cohort of first-year students (UIC, 2009-2012, ages 21-22 in 2012). The panel is described as homogeneous on age and education, racially and socioeconomically diverse, and not a random sample of the national population. Era: 2009-2013, a period when Facebook was dominant and Twitter, Google+, LinkedIn and Tumblr were growing.

## Key findings

- Adoption of Facebook, LinkedIn and Twitter is not random with respect to demographics and SES in the national Pew data. Older people are less likely to use all three; the least educated and lower-income people are less likely to use all three. (Age: Facebook -0.05, LinkedIn -0.02, Twitter -0.06 (all p < .001, full sample). Facebook high school or less: -0.61 (p < .001); LinkedIn: -1.37 (p < .001); Twitter: -0.56 (p < .01). LinkedIn income (logged): 0.72 (p < .001). Pattern largely holds among Internet users only. Shares: Facebook 53% of all adults (text says just over half, or two-thirds of Internet users); LinkedIn 19% of all, 24% of Internet users; Twitter 13% of all, 15% of Internet users. R-squared .09-.16.; Pew results, Table 1 and text, pp. 67-68)
- Gender and race differ by site. Women are more likely on Facebook (+0.46) and less likely on LinkedIn (-0.39). Black respondents are more likely on LinkedIn and Twitter; the Facebook coefficient is significant only at p < .10. (Female Facebook 0.46 (p < .001); female LinkedIn -0.39 (p < .01); Black Twitter 0.63 (p < .01); Black LinkedIn 0.68 (p < .01); Black Facebook -0.31 (p < .10 only).; Pew results, Table 1, p. 68)
- In the young-adult panel, 2009 demographics predict 2012 SNS adoption for Twitter and Tumblr: women, African Americans and students with less-educated parents are more likely on Twitter, and women more likely on Tumblr. Asian Americans are less likely on LinkedIn. (Female Tumblr 0.84 (p < .01); female Twitter 0.51 (p < .05); Black Twitter 0.87 (p < .05); high school or less parental education Twitter -0.85 (p < .01), Tumblr -1.08 (p < .01); Asian LinkedIn -0.29 (p < .10). Panel R-squared .03-.08.; Panel results, Table 4 and text, pp. 72-73)
- Internet experience and skills predict later adoption. Higher use frequency predicts adoption of four of five sites (all except Twitter). Internet skills from 2009 predict Twitter, LinkedIn and Tumblr adoption in 2012. (Use frequency: Facebook 0.48, Google+ 0.45, LinkedIn 0.52, Tumblr 0.48 (all p < .05). Internet skills: Twitter 0.33 (p < .05), LinkedIn 0.33 (p < .05), Tumblr 0.62 (p < .001). Skills not significant for Facebook (-0.06) or Google+ (0.22).; Panel results, Table 4 and text, pp. 72-73)
- Adoption is low for most platforms even among this group, which the paper says should give pause about using a single SNS as a sampling frame (p. 71). Facebook is near-saturated while the others vary sharply. (2009 / 2012 adoption: Facebook 87% / 89%; Twitter 3% / 38%; LinkedIn 0% / 18%; Tumblr N/A / 16%; Google+ not launched / 22%; MySpace 36% / 2%. Text: Twitter used by 37.5% in 2012.; Panel usage, Table 3 and text, p. 71)
- Conclusion: SNS-derived log data are biased toward certain populations (demographics, SES, Internet skills, topical interest). Findings may come to the exact opposite conclusion, not just be imprecise (p. 74). Good sampling must not be confounded with the substantive question, and studies must state their sampling-frame scope in titles, abstracts, methods, results and press communication. (No new numbers; the conclusion reasons from Tables 1-4 and cites Hargittai (2011) on opposite-direction conclusions.; Conclusion, pp. 73-74)

## Limitations

- Author's stated limits: Pew did not disaggregate SNS use by service before 2012, did not measure Internet skills, and is cross-sectional with no repeated respondents.
- Panel is one required class at one university (UIC), homogeneous on age and education, not a random sample, and only 547 cases; 52% retention to 2012, with African American share falling from 11% to 8%.
- Paper-pencil surveys, chosen to avoid online-survey bias; the mode choice is a deliberate design decision, not a limitation the author fully resolves.
- Analyses include one SNS at a time and do not control for use of other SNSs (note 3: 28% of Internet users in the Pew sample use more than one SNS; note 4: 56.4% in the panel).
- Study covers only SNS use, not other platforms or offline ties; the Burke and Kraut example shows how single-platform data miss activity elsewhere (pp. 65-66).
- Author limits the paper to one bias (single-SNS sampling frames) due to space constraints (p. 66).
- Temporal ordering is a partial answer to reverse causality, not a causal design.
- Pew dataset was not public at time of writing (note 1).

## Short quotes

- "people do not select into the use of such sites randomly" (Abstract, p. 63)
- "if people do not select into the use of the particular site randomly, then findings cannot be generalized beyond the site's population" (Sampling frame section, p. 65)
- "bigger is not always better; size is not all that matters when it comes to datasets" (Conclusion, p. 74)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-sna#Social and press-engagement layers\|Social and press-engagement layers]] | qualifies | People do not select into SNS use at random, and selection differs by site and demographic group. Pew 2013 logistic coefficients (full sample, N 1,510 to 1,528, log-odds): female on Facebook 0.46 (p<.001), female on LinkedIn -0.39 (p<.01), Black on Twitter 0.63 (p<.01). Black on Facebook -0.31 is p<.10 only and -0.06 among Internet users, so it is not a reliable selection effect. Users of one platform are not the population. (Table 1 (p. 68); sampling-frame section (pp. 64-65); Burke and Kraut discussion (pp. 65-66); conclusion (pp. 73-74)) | social_interaction_layer.py: X reposts 1.0, X mentions 0.4, Bluesky mentions 0.4; pair p = events / max(25, X-post count of author), cap 0.25, so Bluesky events enter the numerator only (confirmed). build_multilayer_network.py: new edge w = 6p/(1-p) cap 2.0, or up to +30 percent reinforcement on existing ties (confirmed). Read-only counts from social_media.db on 2026-10-09: 411 Bluesky mentions = 392 D to D, 8 D to I, 5 I to D, 6 R to R, 0 D to R; 135 posters = 117 D, 16 R, 2 I; roster 533 = 259 D, 271 R, 3 I; posts 2023-07-01 to 2026-09-19. JSON written 2026-09-19 holds 739 pairs, 275 reposts, 953 mentions (X and Bluesky). Unverified: rows present at JSON write time, since the DB was modified 2026-10-04. `fec/network/social_interaction_layer.py:25,36-56; fec/network/build_multilayer_network.py:146-166 (both confirmed)` | Her coefficients do not transfer; her frame logic may. Her data are US adults (2013) and one 2009-2012 student cohort, not legislators or Bluesky. That X adoption bias is mostly gone for legislators is the analyst's inference: the paper never discusses institutional accounts (502 of 515 member rows have an X handle, read-only). The Bluesky handle field is populated for all 515 rows, so 135 is posting, not account adoption. Party-skewed posting may carry a party-skewed edge pattern into the layer; untested. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-stance#Media blend (press + social) with recency decay\|Media blend (press + social) with recency decay]] | qualifies | Non-users are excluded from a site-based sampling frame by design. If selection is related to outcomes, results from users may reach the opposite conclusion about the wider population. Sampling should not be tied to the substantive question. The paper does not discuss how missing social-media activity should be coded. (Sampling-frame section (pp. 64-65); conclusion (pp. 73-74)) | build_media_blend.py: SOCIAL_W = 0.5; f = min(1, nz/15), with nz the members carrying a nonzero social value in the category (shrink on by default). Cell = (1 - 0.5f) x press/p80 + 0.5f x social/p80. A member with no social value gets sv = 0, so at f = 1 the press side is halved and silence scores as zero, not missing (confirmed at lines 29, 463, 479). The downstream Press Release - Raw link was not checked. `fec/stance/build_media_blend.py:29,463,479 (confirmed)` | The paper gives no evidence about missing-data coding. The concern bites only if posters differ from non-posters on stance in categories where f = 1; that is untested, and the analyst's own note says so. The stance-ladder rule on missing evidence was not checked. The zero-coding critique is an analyst extension, not a paper claim. | weak | VERIFIED-SOFTENED |
| [[systems/melange-legislative-data#Signal-prioritized press collection cap\|Signal-prioritized press collection cap]] | suggests-extension | A sampling frame tied to the outcome can bias findings (Schwartz et al. 2013 opt-in app users, p. 65). Studies should state the frame scope and keep sampling separate from the outcome. (pp. 65-66 (selection on the dependent variable); conclusion (pp. 73-74)) | cu_text_collector.R lines 112-140: every CU-candidate-titled page is fetched; generic pages are capped at CAP = 1000 per member, newest first (confirmed). Registry figures (P(CU-relevant given CU title) 0.825 vs 0.0089; coverage D 92.5 vs R 87, Senate 83 vs House 96) match the component registry text; not re-measured from the corpus. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_text_collector.R:112-140 (confirmed)` | Hargittai's argument applies only if coverage gaps correlate with CU stance or volume. The cap drops only generic pages (about 0.9 percent CU-relevant per the registry). The cap is not party- or chamber-specific, so 'favours House and Democratic offices' does not follow from the code; it rests on unmeasured coverage rates. The collector comment assumes the skipped bulk is missing at random, which is the untested premise. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Overstated: 'age, education, income, gender and race each predict use of Facebook, LinkedIn and Twitter'. Gender is null for Twitter (-0.00), income is null for Facebook among Internet users (-0.12), and Hispanic is null for all three. Should say each predicts use of at least one site.
> - Summary issue: The 0.72 (LinkedIn, logged income) and -1.37 (LinkedIn, high school or less) are log-odds coefficients from the full Pew sample (N 1,510 for LinkedIn), not effect sizes. Name the scale and window.
> - Summary issue: 'A biased sampling frame can reverse the direction of a finding' overstates the paper's 'may come to the exact opposite conclusion' (p. 74). Use 'may'.
> - Summary issue: 'Legislators are elite accounts, so platform adoption bias mostly disappears on X' is the analyst's inference, not the paper's; the paper never discusses institutional accounts. Mark it as inference.
> - Summary issue: 'none cross party' holds only for D vs R. The same paragraph's counts show 8 D to I and 5 I to D mentions; state that 13 cross the D/I line.
> - Summary issue: Denominators are mixed: 502 of 515 have an X handle (social_media.db members table, 515 rows) while party counts use the 533 roster. The Bluesky handle field is populated for all 515 rows, so 135 posters is posting, not account adoption. Name the denominator at each figure.
> - Summary issue: The UIC panel 2012 figures (Facebook 89, Twitter 38, LinkedIn 18 percent) come from a 2012 wave with 52 percent return and no N in Table 3; name that. 'UIC students' should read young adults in one required first-year class (2009 wave n = 547).
> - Summary issue: Paul-alignment claims (Fink framework; critiques on sign, third-party sentiment and temporal weighting) were not checked against his annotation files; treat as unverified.
> - Summary issue: Extraction issues in ext: the 'space constraints' limitation is on p. 66, not p. 65; Black on Facebook -0.31 and Asian on LinkedIn -0.29 need p<.10 flags; 'no single SNS can serve as a sampling frame' is the extractor's inference, while the paper says the usage rates should give pause (p. 71).
> - Summary issue: Coverage: all 3 bearings and all 4 related links checked; Tables 1, 3 and 4 re-read in the paper text; code ranges re-read; DB and roster counts re-derived read-only. Not checked: brain figures beyond component registry text, Paul's annotation claims beyond the Baptista highlight, and the 400 tweet-gold count.

## Gaps and flags

- The social layers carry no per-platform frame statement. social_interactions.json has a provenance string but no per-platform event counts or party mix, so the Bluesky D-skew is invisible downstream.
- Volume normalization in the social layer uses an X-only post count as the denominator for events from X and Bluesky combined. Cross-platform activity is not on a common base. Any change here would need its own ruling; this is not a recommendation to normalize per edge.
- The media blend treats social silence as zero stance rather than missing. No test checks whether non-posters differ systematically (party, chamber) from posters in the categories where the social share is fully on (f = 1).
- Gold sets (400 tweet golds) and stance accuracy figures describe members who post. Their scope does not say that the accuracies cover only the posting population.
- Press, social and Bluesky corpora have no published per-party and per-chamber coverage tables next to the pillar. Coverage figures live only in brain notes.
- The Bluesky party mix (135 posters of the 533 roster: 117 D / 16 R / 2 I; 411 mentions: 392 D->D, 8 D->I, 5 I->D, 6 R->R, 0 D-R; window 2023-07-01 to 2026-09-19) is a read of social_media.db joined to responsiveness_layers.json party. A check is still open on whether these rows existed when social_interactions.json was built on 2026-09-19.
- Not a call for per-edge volume normalization (REJECTED in this project). The code already applies per-author normalization; the issue raised is the mismatched X-only denominator and the unstated frame. Any fix must avoid re-proposing the rejected option.
- Not a call to drop regular cosponsorship edges (REJECTED). Hargittai's argument concerns social-platform layers, not the cosponsor base.
- Edge weights in this layer are heuristic model inputs, never closeness or access probabilities (D-025). A D-heavy Bluesky subgraph must not be described as Democrats being 'closer'.
- The media blend scores social silence as zero, which sits awkwardly with the stance contract's stated rule that missing evidence is never imputed as neutral (category-stance-ladder assumption). Recorded as an observation for Paul to rule on, not a finding.

Paul has no annotations on this paper. His related Baptista highlight reads that more network data is not always better (highlight only, no note). Hargittai's argument bears on that caution from a sampling angle: each added platform layer brings its own selected population. For legislators that may show up as party skew on Bluesky, not as the age, income or education skew Hargittai found in the public. How Paul's other positions on the affected components relate to this paper was not checked against his annotation files.

## Related papers

- [[steinertthrelkeld-2018-twitter-data]] — complements — Item exists and its title matches (Twitter as Data, 2018). Its content on representativeness was not re-checked in the text. Kept.
- [[baptista-2022-universal-multilayer-network]] — complements — Item exists. Paul's annotations on it include a highlight reading more network data is not always better (highlight only, no note). That supports the caution, but the wording is network data, not layers. Kept with corrected wording.
- [[azucar-2017-predicting-personality-traits]] — complements — Item exists; title matches. Its extracted summary says the criterion came from ordinary users of Facebook, Twitter, Weibo and Instagram, so the frame caveat applies. Kept.
- [[kumari-nd-five-personality-predictions]] — complements — Item exists; title (Big Five Personality Predictions From Twitter Using Regression) matches. Kept.

### Linked from
- [[zhang-2022-data-assisted-social]] — contradicts — Hargittai shows that big data from social network sites carries sampling biases. Zhang treats 882,371 tweets as representative of consumer sentiment and never examines who posts.
- [[zhou-2026-measuring-stance-dynamics]] — complements — Item exists. The paper concedes a non-representative Santiago news-follower sample (Sec. 3.2, Sec. 5.1 p.12, Sec. 6.1 p.16), and Hargittai is relevant to such biases. The paper does not cite her, so 'explains' should read 'relevant to'.
- [[esmalian-2022-characterizing-equitable-access]] — complements — Hargittai documents sampling biases in big data drawn from digital traces. Esmalian rests on StreetLight cell/GPS traces with an undescribed penetration rate and black-box home-zone inference, which is exactly that kind of bias. Read together, this limits how far its findings by income can be trusted.