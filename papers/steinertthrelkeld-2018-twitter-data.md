---
type: paper
title: "Twitter As Data"
authors: ["Zachary C. Steinert-Threlkeld"]
year: "2018"
venue: "Cambridge University Press, Elements in Quantitative and Computational Methods for the Social Sciences (ISBN 978-1-108-43833-9 confirmed on p.1; running heads read Quantitative and Computational Methods for Social Science; ISSN 2398-4023 online, 2514-3794 print)"
doi: ""
category: "Machine Learning & Sentiment Analysis"
systems: ["melange-sna", "melange-stance"]
components: ["melange-sna.multilayer-social-press-edges", "melange-stance.media-blend"]
paul_annotated: true
paul_markup_count: 22
evidence_status: "unverified"
source: "[[_sources/steinertthrelkeld-2018-twitter-data.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Machine Learning & Sentiment Analysis\\9781108438339_excerpt.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Machine Learning & Sentiment Analysis\\9781108438339_excerpt.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Machine Learning & Sentiment Analysis\\9781108438339_excerpt.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Machine Learning & Sentiment Analysis\\9781108438339_excerpt.pdf", "C:\\Users\\pgoff\\Downloads\\9781108438339_excerpt (1).pdf", "C:\\Users\\pgoff\\Downloads\\9781108438339_excerpt.pdf"]
sha12: ["fcd4bed2b670"]
tags: [paper, machine-learning-sentiment-analysis]
---

# Twitter As Data

**Zachary C. Steinert-Threlkeld (2018).** *Cambridge University Press, Elements in Quantitative and Computational Methods for the Social Sciences (ISBN 978-1-108-43833-9 confirmed on p.1; running heads read Quantitative and Computational Methods for Social Science; ISSN 2398-4023 online, 2514-3794 print).* Theme: [[themes/machine-learning-sentiment-analysis|Machine Learning & Sentiment Analysis]].

> [!abstract] Summary
> **What the paper is.** This is a short Cambridge Elements methods tutorial on collecting and processing Twitter data for social scientists. The excerpt on disk covers printed pages 1-10 only and stops partway through section 2.1.1. There is no estimator, no test and no causal design. Its content is descriptive figures from 2016 and a description of the data pipes:
> - **Scale:** 319M monthly active users and about 500M tweets a day.
> - **US adult usage:** 24% use Twitter; 42% of users use it daily.
> - **Elite coverage:** every Senator and 430 House members have official accounts.
> - **Geotags:** geotagged tweets are about 2-3% of traffic and skew urban and higher-income.
> - **Access limits:** the streaming API gives about a 1% sample with no information on the author's network. The REST API caps each account at its 3,200 most recent tweets and is needed to rebuild follower networks. Author metadata reflects the time of the API request, not the time the tweet was posted.
>
> The rest of the excerpt compares R packages (streamR, twitteR, RCurl) with Python packages (twython, tweepy, pandas). Network analysis is deferred to a separate 2016 work by the same author.
>
> **Why it matters here, and how little.** The system's social channel does not use the Twitter API at all. X, Facebook and Truth Social posts come from a licensed Quorum export; Bluesky posts are collected directly. The social network layer is built from reposts and @mentions found in post text, not from follower or friend lists. So the 3,200-tweet cap, the rate limits and the stream-sampling limits do not bind on the system as built.
>
> Two points do carry over, both as context:
> - **Coverage:** the elite-coverage figure supports the assumption that nearly every member has a social channel to read, though the figure is from 2016.
> - **Metadata timing:** handle-to-member mapping and attribution happen at collection time. This is a provenance caveat for the member-attributed corpus.
>
> Nothing in the excerpt bears on stance classification, influence modeling, path-finding or allocation. Its access claims describe the 2016 API and the 2016 platform, not today's X.

## Paul's annotation

From [[annotations/Machine Learning & Sentiment Analysis - Machine Learning & Sentiment Analysis - Annotated Bibliography|Machine Learning & Sentiment Analysis - Machine Learning & Sentiment Analysis - Annotated Bibliography]]:

> Steinert-Threlkeld, Zachary C. *Twitter As Data*, 2018, [https://doi.org/10.4135/9781526468857](https://doi.org/10.4135/9781526468857).

## Paul's markup in the PDF

- p1 Highlight: "Paraphrase: the Element's data and code are posted on the publisher's companion web page." — 
- p1 Highlight: "Paraphrase: large datasets let behavior be studied at very fine time and place resolution." — 
- p1 Highlight: "Paraphrase: such data let scholars test rival theories and build new ones." — 
- p1 Highlight: "Paraphrase: Twitter combines size, global reach and easy data access, so it is the usual platform in academic work." — 
- p2 Highlight: "Paraphrase: the new data tools are no harder to learn than standard qualitative or quantitative methods." — 
- p2 Highlight: "Paraphrase: introduces the 21-account Egypt and Bahrain running example." — 
- p2 Highlight: "Paraphrase: scale claim (hundreds of millions of accounts, about 500 million messages a day) and easier data access than Facebook." — 
- p2 Highlight: "Paraphrase: scope note that analysis covers text and space, with network analysis deferred to a 2016 source." — 
- p3 Highlight: "Paraphrase: raw tweets must be processed into tabular form before analysis." — 
- p3 Highlight: "Paraphrase: roadmap note on Facebook and Instagram, and why Twitter stays the main source of social network data." — 
- p4 Highlight: "Paraphrase: range of Twitter users, from heads of state and firms to ordinary people." — 
- p4 Highlight: "Paraphrase: survey-based income and geography profile of US Twitter users." — 
- p4 Highlight: "Paraphrase: both the streaming and REST APIs are open to any account holder." — 
- p5 Highlight: "Paraphrase: API use still requires programming, though less than in the past." — 
- p6 Highlight: "Paraphrase: gathering Twitter data needs new skills, but modeling and visualizing can use conventional tools." — 
- p7 Highlight: "Paraphrase: geotagged tweets skew urban and higher-income, so reliability depends on the question." — 
- p7 Highlight: "Paraphrase: geotagged tweets are about 2-3 percent of the total." — 
- p8 Highlight: "Paraphrase: R comparison, where RCurl is the most flexible option but needs the most code." — 
- p8 Highlight: "Paraphrase: three R routes to Twitter (streamR for streaming, twitteR for REST and less robust, RCurl for raw HTTP)." — 
- p8 Highlight: "Paraphrase: Python comparison, where twython covers both APIs with built-in exception handling and pandas offers data-frame tools." — 
- p9 Highlight: "Paraphrase: switching languages depends on cost and benefit, since human time is the scarcest resource." — 
- p9 Highlight: "Paraphrase: Python matters more for large data or non-social-science collaborators; those who know neither language should learn both." — 

## Study

**Question.** No testable research question. The Element aims to introduce social scientists to acquiring, processing and analyzing Twitter data, with scripts and examples (p.1).

**Theory.** None stated beyond the abstract's general claim that such data let scholars adjudicate between competing theories and develop new ones, compared to the microscope and germ theory (p.1).

**Data.** Running example: 21 Twitter accounts from Egypt and Bahrain, 19 civil-society actors from four social movements and 2 Bahraini government accounts (p.2). Unit = account; sample N = 21 accounts. Collection period not stated in the excerpt. Platform figures are from Twitter (2016) and a November 2016 US adult survey (p.4).

**Method.** Methods tutorial. Covers acquisition via the REST API (user timelines, specific tweet IDs, search, follower and friend lists) and the streaming API (random 1 percent sample, filters by language, keyword, account, place) (pp.2, 5, 10). Then processing raw tweets into rectangular data, text analysis, event detection, spatial analysis of geotagged tweets (pp.2-3, 7). Software: R (streamR, twitteR, RCurl) and Python (twython, tweepy, pandas, statsmodels, NLP and ML libraries) (pp.7-9). No estimator, model or statistical test appears in the excerpt.</method>
<parameter name="identification">Not applicable. No causal or identification strategy in the excerpt.

**Scope.** Twitter as a platform, for political science and computational social science. Population framing is all Twitter users, with US adult survey data (2016) used to characterize them (p.4). Era: data as of 2016. Excerpt addresses elections, political communication, conflict and spatial diffusion only as research areas (p.2). Lobbying, path-finding, resource allocation, persuasion and turnout are not discussed in the excerpt. Legislators appear only through Table 1 (Congress literature) and the fact that every Senator and 430 House members have official accounts (pp.5, 7). Text measurement appears only as an intro to NLP tools, with no measurement model (pp.2, 8-9). Network analysis is deferred to Steinert-Threlkeld (2016) (p.2).

## Key findings

- Twitter scale: 319 million monthly active users from almost every country, and over 1 billion US dollars annual revenue (Twitter 2016, cited). Numbers: 319M MAU; over 1B USD annual revenue (p.4, section 1.1)
- Volume: over 300 million accounts and about 500 million messages per day. Numbers: 300M+ accounts; 500M messages per day (p.2 (intro); p.4 (section 1.1))
- US adult usage (Nov 2016, Greenwood et al. 2016 cited): 24 percent of adults use Twitter. Of users, 42 percent use daily and 24 percent weekly; for Facebook the comparable figures are 76 percent daily and 15 percent weekly. Numbers: 24 percent of US adults; 42 percent daily, 24 percent weekly (Twitter); 76 percent daily, 15 percent weekly (Facebook) (p.4, section 1.1)
- Twitter user demographics: men and women use it equally; plurality aged 18-29 (majority 18-49); plurality hold a college degree; majority earn over 50,000 USD; users evenly spread across urban, suburban and rural areas. Numbers: 20 percent have high school or less (p.4, section 1.1)
- Elite coverage: every US Senator and 430 House members have official Twitter accounts; most news organizations too. Numbers: All Senators; 430 House members (p.7, section 1.2)
- Geotagged tweets are a small share of traffic and skew urban and higher-income, so their reliability depends on the research question. Numbers: 2-3 percent of all tweets (p.7, section 1.2 (citing Leetaru et al. 2013; Malik et al. 2015))
- Streaming API returns about 1 percent of all tweets daily, filterable by keyword, user location, user ID or language. The stream carries no information on the author's network. Numbers: 1 percent stream (p.5 (section 1.1) and p.6)
- REST API returns a user's 3,200 most recent tweets, follower and friend lists, and profile data. Full network reconstruction requires the REST API. Numbers: 3,200 tweets per account (p.5 (section 1.1); p.6 (section 1.2))
- Rate arithmetic: 180 timeline requests per 15 minutes, up to 200 tweets per request, so an account with 3,200 or more tweets needs 16 requests. Numbers: 180 requests per 15 min; 200 tweets per request; 16 requests for 3,200 tweets (p.10, section 2.1.1)
- Specific tweets by ID can go beyond the 3,200 cap. Terms of Service bar sharing more than 50,000 original tweets per day, but unlimited tweet IDs may be shared. Numbers: 50,000 original tweets per day sharing cap; unlimited IDs (p.10, section 2.1.1)
- Facebook's research team has veto power over proposals and publications, and Facebook tightened control after the feed-manipulation controversy (Kramer et al. 2014 cited). Twitter's norm of public conversation makes it easier to study. Numbers:  (p.6, section 1.1)
- Scholars began quantitative Twitter work only in the last five years. Topics: conflict dynamics, social networks, political preferences, legislative responsiveness, economic outcomes, mobilization. Numbers: last five years (p.3 (roadmap))
- Table 1 literature: Twitter is used for prediction (box office, coups, crime, stock market, unrest), polarization, Congress, demographics, economics, geography, sentiment and disaster response. Numbers:  (p.5, Table 1)

## Limitations

- Historic data are hard to access and the user must maintain their own infrastructure (p.9, section 2.1 advantages/disadvantages).
- Per-account cap of 3,200 most recent tweets through the timeline endpoint (pp.5, 10).
- Geotagged tweets are more common in urban areas and among higher-income users (p.7).
- Tweet author metadata reflects when the API request was made, not when the tweet was created (p.10).
- Redistribution limit: researchers depend on the original acquirer's goodwill to share tweets (p.10).
- Facebook data for private users require an app or a research-team agreement (p.6).
- The 1 percent stream has no author network information (p.6).
- Excerpt says section 6 covers limits on inference from individual tweets and ethics for protected groups such as children; that section is not in the file (p.3).

## Short quotes

- "The primary purpose of this Element is to help the reader gain that knowledge." (p.5, section 1.1)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-sna#Social and press-engagement layers|Social and press-engagement layers]] | qualifies | Rebuilding an author's network takes the REST API: follower and friend lists, plus the 3,200 most recent tweets per account at 180 requests per 15 minutes and 200 tweets per request. The 1% streaming sample carries no information on the author's network. Network analysis itself is deferred to Steinert-Threlkeld (2016). (pp.5-6 (section 1.1, 1.2); p.10 (section 2.1.1); p.2 (scope note)) | The social layer does not use follower or friend graphs. Directed ties come from in-text reposts ('RT @handle', weight 1.0) and @mentions (weight 0.4) in Quorum-exported X posts, plus Bluesky mentions. Each direction's value is the weighted event count divided by max(25, the author's post count), and each pair is capped at 0.25. So the layer is an interaction network, not a follow network. `fec/network/social_interaction_layer.py:6-19,25,56; fec/network/build_social_master.py:4-11` | The access limits in the paper (3,200 cap, 1% stream, rate limits) do not bind here, because the corpus comes from a licensed Quorum export and not from the API. Those limits also describe the 2016 API, which X has since restricted and made paid. The useful point is a distinction, not evidence: a mention or repost network is a different object from a follower network, so the layer's 'relational tie' reading rests on Fink et al., not on this excerpt. No figure in the paper speaks to edge weights or normalization. | weak | UNVERIFIED |
| [[systems/melange-stance#Media blend (press + social) with recency decay|Media blend (press + social) with recency decay]] | context | Every US Senator and 430 House members had official Twitter accounts (2016). Tweet author metadata reflects when the API request was made, not when the tweet was created. (p.7 (section 1.2); p.10 (section 2.1.1)) | The media blend gives social posts half the weight of each member-category cell, with SOCIAL_W 0.5, a 0.5-year half-life (decay 0.5^(age/HL)) and nz/15 shrinkage where social signal is thin. Members are attributed through a handle registry; unverified Bluesky handles are flagged and kept out of the member rollup. `fec/stance/build_media_blend.py:29,33-36,118-126; fec/network/build_social_master.py:6-9` | The coverage figure supports the assumption that almost all members have a social channel, but it counts Twitter accounts in 2016. It does not count the 119th Congress, it does not cover the platform mix the system reads (X, Facebook, Truth Social, Bluesky), and it says nothing about how many posts each member makes on CU topics. The metadata-timing caveat applies to handle-to-member mapping done at collection time. It does not apply to decay, which uses the post date. The paper offers nothing on ghost-writing, on the half-life, or on whether social posts add predictive validity over press releases. | weak | UNVERIFIED |

## Gaps and flags

- The paper separates follow-graph data (REST API) from interaction data (stream or timeline text). The system's social layer uses only interaction data, and no note records that a follower or co-follow layer was considered and set aside. The paper does not test whether one beats the other, so this gap is a documentation item, not a demonstrated lack.
- Provenance timing: the paper notes that account metadata reflects collection time. The system's handle registry (members.twitter and members.bluesky in social_media.db) has no recorded check on handle changes or reassignments across the corpus window. The only safeguard is the unverified-Bluesky flag.
- Platform coverage drift: the paper's elite-coverage figure is a 2016 Twitter count. The system reads four platforms but does not report member-level social coverage by party or chamber, the way the press collector's 86.6% figure (D 92.5% vs R 87%) is reported. A coverage table for the social channel would make a party-skewed social signal visible before it reaches the 50% social share of the blend.
- Citation mismatch. Paul's annotation documents cite 'Twitter As Data, 2018' with DOI 10.4135/9781526468857, which is a SAGE prefix. The file on disk is the Cambridge University Press Element (ISBN 978-1-108-43833-9 on p.1). Paul should confirm which edition the bibliography means. The 2018 year is also not verified from the text, which shows no year.
- Coverage limit: only printed pp.1-10 are in the file. Sections 2.1.2 to 6 are missing, including section 6, which the roadmap says covers limits on inference from individual tweets and ethics. Any claim attributed to those sections is unverified.
- No conflict with the brain rules. The paper does not argue for dropping regular cosponsorship edges or for per-edge volume normalization. It uses no graph ranks, LDA data or reciprocity claims. (The social layer's author-volume normalization comes from Fink et al., not from this paper.)
- Era: every platform and access figure is from 2016 Twitter. X's API terms changed after 2023, so the access ease the paper describes, and that Paul highlighted, should not be read as current.

Paul recorded no takeaway; both annotation entries are citation-only. His 22 highlights are all on pp.1-9 and none carries a note. They track the paper's practical pitch: scale and ease of access, the R and Python tooling comparison, the cost of human time, and the urban, higher-income skew of geotags. None of them makes a modeling claim. The positions table lists Steinert-Threlkeld 2018 as a source for melange-sna.multilayer-social-press-edges. On the excerpt, that is a data-access citation, not a method source: the paper defers network analysis to a 2016 companion and never discusses reposts, mentions, edge probabilities or normalization. The layer as built rests on Fink et al. 2023, and the code says so. Paul's note is not stale so much as thin: it cannot underwrite any system choice. Its main residual value is caution. Interaction ties are not follow ties, and the 2016 picture of platform coverage and access is out of date.

## Related papers

- [[hargittai-2015-bigger-always-better]] — complements — This paper notes geotag skew and describes the US user base. Hargittai shows how data from social network sites is biased by who uses them. Together they frame the representativeness caveat. For this system the caveat matters more for any citizen-side social data than for legislators' own posts.
- [[joo-2022-image-data-automated]] — complements — Same author (Steinert-Threlkeld) and a matching 'X as data' methods format, extending the pipeline from tweet text to images of political actors. Paul filed both in the same ML & Sentiment bibliography.
- [[b-2025-comparing-fine-tuned]] — builds-on — This paper stops at an introduction to NLP tooling for tweets. Khan et al. take political-tweet text to fine-tuned stance classification, which is the step the system's tweet-stance readers actually depend on.
- [[kumari-nd-five-personality-predictions]] — builds-on — Uses Twitter text as data to predict Big Five traits, a downstream use of the acquisition and processing pipeline this Element teaches. It bears on whether social posts could feed the lexical trait readers.

### Linked from
- [[baptista-2022-universal-multilayer-network]] — complements — Item exists. Baptista does not cite it. Analyst link only: the social repost and mention layer is the kind of layer Baptista's per-layer test would cover.
- [[zhang-2022-data-assisted-social]] — complements — Gives the methodological grounding for Twitter collection and its limits that Zhang's Twitter-API case study omits.
- [[malaterre-2024-inferring-social-networks]] — complements — Both are text-data resources for social scientists. Steinert-Threlkeld covers Twitter data acquisition and processing and does not treat politicians' text as relational data; representativeness is mentioned only briefly. Use only for data-collection caveats.
- [[zhou-2026-measuring-stance-dynamics]] — complements — Item exists (Twitter As Data). Relevant to the Twitter measurement issues behind hashtag-proxy labels. The July 2026 provenance claim in the rel was not checked.
- [[vanderdoes-2022-strategic-identity-signaling]] — complements — A methods primer for Twitter data. This paper's sampling of seed accounts by news diet and its rater-based labels are a worked example, with the same platform-selection caveats.
- [[fink-2023-congressional-social-network-dataset]] — complements — Methods book on collecting and using Twitter data. Item exists and is titled Twitter as Data; topical match checked on title only.