---
type: paper
title: "A Congressional Twitter network dataset quantifying pairwise probability of influence"
authors: ["Christian G. Fink", "Nathan Omodt", "Sydney Zinnecker", "Gina Sprint"]
year: "2023"
venue: "Data in Brief 50 (2023) 109521"
doi: "10.1016/j.dib.2023.109521"
category: "Social Network Analysis"
systems: ["melange-influence-engine", "melange-sna"]
components: ["melange-influence-engine.cascade-kernel", "melange-sna.multilayer-social-press-edges"]
paul_annotated: true
paul_markup_count: 23
evidence_status: "verified"
source: "[[_sources/fink-2023-congressional-social-network-dataset.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\1-s2.0-S2352340923006212-main.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\1-s2.0-S2352340923006212-main.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\1-s2.0-S2352340923006212-main.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\1-s2.0-S2352340923006212-main.pdf", "C:\\Users\\pgoff\\Downloads\\1-s2.0-S2352340923006212-main.pdf"]
sha12: ["445139e681c1"]
tags: [paper, social-network-analysis]
---

# A Congressional Twitter network dataset quantifying pairwise probability of influence

**Christian G. Fink, Nathan Omodt, Sydney Zinnecker, Gina Sprint (2023).** *Data in Brief 50 (2023) 109521.* [10.1016/j.dib.2023.109521](https://doi.org/10.1016/j.dib.2023.109521). Theme: [[themes/social-network-analysis|Social Network Analysis]].

> [!abstract] Summary
> Fink, Omodt, Zinnecker and Sprint (Data in Brief 50, 2023) publish a directed, weighted Twitter network for the 117th Congress. Tweets were pulled on June 9 2022 (3,200-tweet API cap) for 530 active accounts. The common window is Feb 9 to June 9 2022: 179,974 tweets from 525 accounts, though Sec 2 lists 117,974 tweet IDs. After a 100-tweet floor, 475 members remain as nodes. The edge weight from i to j counts the retweets, quotes, replies and mentions that j makes of i's tweets, all weighted equally, and divides by the number of tweets i issued. The authors call this an empirical probability of influence. They pitch it as heterogeneous transmission probabilities for Independent Cascade or SIR simulations, in place of one arbitrary uniform rate. They also say their Viral Centrality approximates ICM Monte Carlo results, but only the companion Physica A paper shows this; this article does not test it. Limitations: one snapshot, possibly non-stationary rates, and no uncertainty on any edge. For Paul's system the article is a direct method source. press_engagement_layer.py implements Fink's normalization with the source-volume denominator. social_interaction_layer.py cites Fink but divides by the engaging member's own post volume, which is the opposite end of the dyad. It also uses a 25-post floor instead of 100, weights mentions 0.4 and skips quotes and replies. Both layers are mean-symmetrized, which discards Fink's direction. They are then mapped through w = 6p/(1-p) so that the cascade kernel reproduces p before party and receptivity factors. The denominator finding comes from reading the code only and stays UNVERIFIED (DE-062).

## Paul's annotation

From [[annotations/Annotated Bibliographies & Literature Synthesis - Curated Annotations|Annotated Bibliographies & Literature Synthesis - Curated Annotations]]:

> Fink, Christian G., et al. “A Congressional twitter network dataset quantifying pairwise probability of influence.” *Data in Brief*, vol. 50, 28 Aug. 2023, p. 109521, [https://doi.org/10.1016/j.dib.2023.109521](https://doi.org/10.1016/j.dib.2023.109521).
>
> This article presents a novel dataset highlighting the probabilities of influence among members of Congress, derived from Twitter’s API.  The data is presented as a social network, with edge weights representing the probabilities of influence, composed of quotes, retweets, mentions, and replies. It provides a fundamental, pre-built framework for pipelining the data required for the present research. The Viral Centrality Algorithm should be interrogated further, both for its ability to reduce compute time and for its ability to simulate scenarios that, by design, are similar to a Monte Carlo simulation. The two main weaknesses of the article are as follows: whether it quantifies a reply or mention as negative; how it quantifies agreement between two or more actors within the ecosystem when the sentiment is negative toward another member; and the necessity for temporal weighting to capture shifting relationships. Nonetheless, it is a fundamental framework for capturing the psychological and relational characteristics of actors within the ecosystem and among them.

From [[annotations/Social Network Analysis - Social Network Analysis - Annotated Bibliography|Social Network Analysis - Social Network Analysis - Annotated Bibliography]]:

> Fink, Christian G., et al. “A Congressional twitter network dataset quantifying pairwise probability of influence.” *Data in Brief*, vol. 50, 28 Aug. 2023, p. 109521, [https://doi.org/10.1016/j.dib.2023.109521](https://doi.org/10.1016/j.dib.2023.109521).
>
> This article presents a novel dataset highlighting the probabilities of influence among members of Congress, derived from Twitter’s API.  The data is presented as a social network, with edge weights representing the probabilities of influence, composed of quotes, retweets, mentions, and replies. It provides a fundamental, pre-built framework for pipelining the data required for the present research. The Viral Centrality Algorithm should be interrogated further, both for its ability to reduce compute time and for its ability to simulate scenarios that, by design, are similar to a Monte Carlo simulation. The two main weaknesses of the article are as follows: whether it quantifies a reply or mention as negative; how it quantifies agreement between two or more actors within the ecosystem when the sentiment is negative toward another member; and the necessity for temporal weighting to capture shifting relationships. Nonetheless, it is a fundamental framework for capturing the psychological and relational characteristics of actors within the ecosystem and among them.

## Paul's markup in the PDF

- p1 Highlight: "Twitter’s application programming interface (API) V2 was used to determine the number of times each member of Congress retweeted, quote tweeted, replied to, or mentioned other Congressional members, and the probability of influence was found by normalizing the summed influence" — 
- p2 Highlight: "Tweepy Python package to interact with Twitter’s AP" — 
- p2 Highlight: "most recent 3,200 tweets of all members of the 117 th United States Congress." — 
- p2 Highlight: "We also excluded any Congressional member who tweeted less than 100 times during this window of time." — 
- p2 Highlight: "(through retweets, quote tweets, replies, or mentions)" — 
- p2 Highlight: "the only publicly available Twitter dataset that empirically quantifies pairwise influence between users." — 
- p2 Highlight: "but it does not synthesize these data into influence probabilities." — 
- p2 Highlight: "most users in the dataset only tweeted a few times, making the inference of empirical probabilities problematic." — 
- p2 Highlight: "There is a vast literature on identifying ‘influential spreaders’ within social or epidemiological networks" — 
- p2 Highlight: "The centrality measure’s performance is often compared with 'ground truth' results from Monte Carlo simulations of a simple epidemiological mode" — 
- p2 Highlight: "these Monte Carlo simulations typically just assume a homogeneous transmission probability, set to an arbitrarily chosen value" — 
- p3 Highlight: "All data files and code are hosted on the Zenodo repository linked above" — 
- p4 Highlight: "he tweet IDs are provided in the congress_tweet_ids.txt file described previously in the Data Description sectio" — 
- p4 Highlight: "so we went as far back in time as possible before first exhausting the 3,200-tweet limit for any one Congressional member." — 
- p4 Highlight: "Furthermore, in order for probabilities of influence to be meaningful, we required that a given Congressional member issue at least 100 tweets over this timeframe in order to be included in this dataset" — 
- p4 Highlight: "475 Congressional members with tweets included in the social network" — 
- p4 Highlight: "Our Twitter API rehydration code is available in a Zenodo repository at this URL: https://zenodo.org/record/8253486 ." — 
- p4 Highlight: "To quantify influence of member i on member j , we considered the number of times member j retweeted, quoted tweeted, replied to, or mentioned a tweet by member i ." — 
- p4 Highlight: "Code that calculates influence using this technique and produces a directed, weighted network given tweets IDs is available at the repository URL provided above" — 
- p4 Highlight: "Viral Centrality efficiently approximates Monte Carlo simulations of the Independent Cascade Model" — 
- p4 Highlight: "Our Viral Centrality algorithm provides very good approximation to ICM results without the need for extensive Monte Carlo simulation, drastically cutting down on computation time." — 
- p5 Highlight: "Congressional Twitter Network (Original data) (Zenodo)." — 
- p5 Highlight: "Furthermore, it should be understood that the probabilities of influence which constitute edge weights are empirical estimates taken from a particular window of time, and may not represent their statistically stationary values." — 

## Study

**Question.** Can a public Twitter dataset quantify empirical pairwise probabilities of influence among members of the 117th US Congress, for use in information diffusion and centrality research? Descriptive data article; no hypothesis is tested.
**Theory.** Information diffusion on complex networks: Independent Cascade Model and SIR epidemic models, with the influential-spreader centrality literature as motivation (Sec 1, p.2). Political theory is not engaged; the authors suggest political scientists may find it useful.
**Data.** Twitter API v2, most recent 3,200 tweets per account, for 530 active member accounts of 535, collected June 9 2022. Window Feb 9 to June 9 2022: 179,974 tweets from 525 accounts (Sec 3, p.4). Members with at least 100 window tweets kept: N = 475 nodes. Unit: directed member-to-member dyad. Section 2 (p.3) lists 117,974 tweet IDs, conflicting with 179,974 in Sec 3 and Ethics (p.4-5). Shared: user IDs, tweet IDs, network JSON and edgelist on Zenodo, DOI 10.5281/zenodo.8253486.
**Method.** Descriptive dataset construction; no statistical model. Tweepy and Twitter /2/users endpoints fetched tweets with referenced_tweets, in_reply_to_user_id and entities parameters (Sec 3, p.4-5). For each ordered pair, retweets, quote tweets, replies and mentions of member i tweets by member j are summed, then divided by the total tweets issued by member i. Viral Centrality code from the companion paper is applied to the network; its ICM-approximation claim is asserted here (p.4), not tested.
**Identification.** None. Descriptive data article with no causal or inferential identification strategy.
**Scope.** Members of the 117th United States Congress with active Twitter accounts on June 9 2022 (530 of 535); a four-month 2022 window; Twitter interactions only, not Congressional behavior overall.

## Key findings

- 530 active congressional Twitter accounts identified of 535 members; 525 yielded tweets in the window (Sec 3, p.4).
- 179,974 tweets from 525 accounts in the Feb 9 to June 9 2022 window (Sec 3, p.4); conflicts with 117,974 IDs in Sec 2 (p.3).
- 475 members with at least 100 window tweets form the final network nodes (Sec 3, p.4).
- Network is directed and weighted; each edge is an empirical influence probability from summed interactions over source tweet count (Sec 3, p.4).
- Authors contrast with the Higgs dataset (ref 2): no interaction-to-probability synthesis there, and most users tweeted only a few times (Sec 1, p.2). Motivating comparison, not a measured result.
- Claim: Viral Centrality approximates ICM Monte Carlo results without extensive simulation (Sec 3, p.4). Supported only by the companion paper; not shown in this article.
- Authors note the network is a single snapshot and the probabilities may not be stationary (Sec 4, p.5).

## Limitations

- API tweets can change over time through deletion, edits or privacy changes; network reflects June 9 2022 only (Sec 4, p.4-5).
- Probabilities come from one four-month window and may not be statistically stationary (Sec 4, p.5).
- Only 475 of 530 active members are retained after the 100-tweet threshold, which the authors justify on estimate reliability (Sec 3, p.4).
- Summing retweets, quotes, replies and mentions with equal weight is assumed; no uncertainty intervals are reported for edge probabilities (Sec 3, p.4).
- Tweet count conflict between 117,974 (Sec 2, p.3) and 179,974 (Sec 3, p.4; Ethics p.5) is not reconciled in the article.
- Redistribution is limited to IDs under Twitter terms, so full content requires rehydration through API access (Ethics, p.5).
- The 3,200-tweet API cap limits how far back the common window can reach (Sec 3, p.4).

## Short quotes

- "the only publicly available Twitter dataset that empirically quantifies pairwise influence between users"

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-sna#Social and press-engagement layers|Social and press-engagement layers]] | contradicts | Influence of i on j is the count of j's retweets, quote tweets, replies and mentions of i's tweets divided by the tweets i issued in the window. Members with fewer than 100 window tweets are excluded, leaving 475 of 530 active accounts (525 with window tweets). (Sec 3, p.4) | social_interaction_layer.py divides each weighted event count by max(25, posts of the ENGAGING member a), so the denominator is the reposter or mentioner volume, the reverse end of the dyad from Fink, which divides by the source i tweets. It uses a 25-post floor, not Fink's 100-tweet exclusion. Mentions are weighted 0.4. There is no separate quote or reply event type: RT-prefixed rows count as reposts, and any @handle in a non-RT post, including a reply or quote, counts as a 0.4 mention. The docstring claims the same Fink discipline as press_engagement_layer. `fec/network/social_interaction_layer.py:15-19,25,33-56` | The pair weight in code is (ev(a,b)/vol(a) + ev(b,a)/vol(b))/2 with vol floored at 25 and each direction capped at 0.25. A Fink-consistent version would divide by vol(b) and vol(a) respectively. This is a code-read finding and stays UNVERIFIED (DE-062) until social_interactions.json is recomputed both ways. The source data are X/Quorum post text and Bluesky interactions, not Fink's API fields, so the analogy is looser than the docstring implies. | strong | VERIFIED-SOFTENED |
| [[systems/melange-sna#Social and press-engagement layers|Social and press-engagement layers]] | method-source | Edge weight is an empirical probability of influence: summed interactions of j on i divided by tweets issued by i. A minimum of 100 tweets per member is required so that the probabilities are meaningful. (Abstract; Specifications table p.2; Sec 3, p.4) | press_engagement_layer.py sets P(i to j) = mentions of i by j divided by items issued by i, capped at 1.0 (line 61), with MIN_ITEMS = 5 on the source (line 58, env ENG_MIN_ITEMS). A code comment calls this the analog of Fink's 100-tweet rule. The docstring says the layer validates cosponsor-edge probabilities. `fec/network/press_engagement_layer.py:3-11,56-61` | Naming a co-actor in a press title is an analogy for engagement, not a reply or retweet. A 5-item floor on the source is far below Fink's 100-tweet floor, so the rates are much noisier. Fink validates nothing, so the docstring claim of validating cosponsor-edge probabilities has no basis in this paper. | moderate | VERIFIED |
| [[systems/melange-influence-engine#Cascade activation kernel build_P and partisan edge factors|Cascade activation kernel build_P and partisan edge factors]] | method-source | Fink offers the directed pairwise probabilities as empirical transmission probabilities that can vary across edges. The paper argues that Monte Carlo SIR-type benchmarks usually assume one homogeneous, arbitrarily chosen rate. (Value of the Data, p.2; Sec 3, p.4) | For untied pairs, engagement (build_multilayer_network.py lines 117-122) and social (lines 158-166) probabilities enter as w = 6p/(1-p), capped by ENG_CAP and SOC_CAP, both default 2.0. In moneyball_core.py build_P returns x/(x+6) with x = w times physics times edge_mult, so it returns p when those factors are 1. For tied pairs, social multiplies the weight by (1 + 0.3 min(1, p/0.10)). Engagement on tied pairs goes through admit(), which drops them unless MEDIA_ADMIT is dominance, the default only in holistic runs. `fec/network/build_multilayer_network.py:117-122,146-166,71; fec/moneyball_core.py:556-562` | The mapping holds only for untied pairs and only when physics and edge_mult equal 1. Fink rates are directed and normalized per source tweet, while the system symmetrizes them and reads them as per-campaign activation odds, which changes unit and meaning. The 2.0 cap gives p_max of 0.25. Fink covers one 2022 window and says the rates may not be stationary. | moderate | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Summary says the social layer skips quotes and replies. The code has no quote or reply event type; @mentions inside replies and quotes count at 0.4.
> - Summary issue: Summary says the uniform-rate critique covers IC or SIR simulations. The paper's homogeneous-rate sentence refers to SIR-type Monte Carlo benchmarks and does not name ICM.
> - Summary issue: Summary says the companion Physica A paper shows the Viral Centrality approximation. That paper is not in the text file, so the claim is UNVERIFIED here, not confirmed.
> - Summary issue: Summary omits that the social layer inputs are an RT-prefix parse of X/Quorum post text plus Bluesky interactions, not Fink's API fields, so the analogy is looser than presented.
> - Summary issue: The flags gloss calling Fink's per-source normalization row-stochastic-like is loose: dividing by the source tweet count does not make outgoing weights sum to one. Drop the gloss or qualify it.

## Gaps and flags

- Engagement rates carry no uncertainty or shrinkage. Fink reports none, and the system's floors (25 posts for social, 5 items for press) sit far below Fink's 100-tweet bar. Nobody has counted how many admitted edges rest on fewer than 3 events.
- Both layers mean-symmetrize and so throw away direction, although Fink's network and the ICM it targets are directed. The cascade engine's directed loader covers cosponsorship only.
- The component's own question, whether Twitter interaction ties predict cosponsorship beyond party and state, is still untested; Fink supplies data, not that test.
- Fink's Viral Centrality, an analytic approximation of ICM spread, has not been compared with the Katz-Bonacich diagnostic or with Monte Carlo cascade output on the live kernel.
- The social layer cites Fink's normalization but divides by the engaging member's own volume, the reverse of Fink and of press_engagement_layer (social_interaction_layer.py:56 vs press_engagement_layer.py:60-61). UNVERIFIED code-read (DE-062).
- The paper calls its edge weights probabilities of influence. On Melange surfaces they must read as engagement rates or heuristic model weights, never as influence, closeness or access probabilities (D-025).
- Fink divides interaction counts by the source's volume, a per-source, row-stochastic-like normalization. Per-edge volume normalization, row-stochastic included, was MEASURED AND REJECTED for the cosponsor degree artifact (Spearman Katz vs raw volume +0.477). Do not cite this paper as support for extending it to the cosponsor layer.
- The press_engagement_layer.py docstring says the layer empirically validates cosponsor-edge probabilities. Fink validates nothing, and an overlap count is not a validation test.
- The article contradicts itself: 117,974 tweet IDs in Sec 2 vs 179,974 tweets in Sec 3 and Ethics. Any figure quoted must say which one it uses.

Paul has no written takeaway: 23 highlights, no notes. The highlights cover method: source-volume normalization, the 100-tweet floor, 475 nodes, and the homogeneous-rate critique. The press layer follows that method. The social layer (2026-07-02) cites it but reverses the denominator and lowers the floor to 25. The component's assumption that reposts and mentions are relational ties (Fink 2023) goes beyond the paper, which defines the measure but never tests it.

## Related papers

- [[trivedi-2020-efficient-influence-maximization]] — complements — Influence maximization under the Independent Cascade Model, the diffusion model whose edge probabilities Fink supplies. Checked against the item title and venue.
- [[almazaydeh-2026-heuristic-centrality-methods]] — same-method — Heuristic centrality versus greedy optimization in influence maximization, the same cheap-heuristic-versus-simulation question as Viral Centrality. Checked against the item title only; it does not test Fink's measure.
- [[steinertthrelkeld-2018-twitter-data]] — complements — Methods book on collecting and using Twitter data. Item exists and is titled Twitter as Data; topical match checked on title only.

### Linked from
- [[vanderdoes-2022-strategic-identity-signaling]] — complements — Same medium. Fink builds congressional Twitter interaction ties that feed the social layer. This paper implies the content on those ties is audience-shaped, and that heterogeneous followings yield more covert political tweets.