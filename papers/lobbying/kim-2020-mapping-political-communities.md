---
type: paper
title: "Mapping Political Communities: A Statistical Analysis of Lobbying Networks in Legislative Politics"
authors: ["In Song Kim (MIT, Political Science)", "Dmitriy Kunisky (NYU Courant Institute, Mathematics)"]
year: "2020"
venue: "Political Analysis (Cambridge University Press, for the Society for Political Methodology)"
doi: "10.1017/pan.2020.29"
category: "Lobbying"
systems: ["melange-labels-and-tiers", "melange-legislative-data", "melange-lobbying", "melange-pathfinding", "melange-sna", "moneyball-rank-pillars"]
components: ["melange-lobbying.lobbied-member-link", "melange-pathfinding.multi-target-and-budget-modes", "moneyball-rank-pillars.committee-pillar", "melange-labels-and-tiers.industry-boards-crown", "melange-legislative-data.ballotpedia-leadership-scores", "melange-sna.sector-networks", "melange-lobbying.fec-lda-bridge"]
paul_annotated: true
paul_markup_count: 157
evidence_status: "verified"
source: "[[_sources/kim-2020-mapping-political-communities.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\network.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\network.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\network.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\network.pdf"]
sha12: ["edb70a4800ee", "0d56770b8d81"]
tags: [paper, lobbying, type/paper, theme/lobbying, evidence/verified]
---

# Mapping Political Communities: A Statistical Analysis of Lobbying Networks in Legislative Politics

**In Song Kim (MIT, Political Science), Dmitriy Kunisky (NYU Courant Institute, Mathematics) (2020).** *Political Analysis (Cambridge University Press, for the Society for Political Methodology).* [10.1017/pan.2020.29](https://doi.org/10.1017/pan.2020.29). Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Kim and Kunisky propose the Bipartite Link Community Model (biLCM). It is a Poisson model of a two-mode count matrix in which each interaction between an interest group and a politician is assigned to one of k latent "link communities". This allows mixed memberships, where an actor belongs to several communities, and mixed interactions, where one pair interacts in several communities. The model is fitted by EM with 50 random starts and k = 8. The application covers the 113th Congress only. A_ij counts the bills that group i lobbied and politician j sponsored, so the network is built indirectly because LD-2 reports never name the members contacted. The paper finds eight legislation communities that match committee jurisdictions. Specialists (MICROSOFT and ARCH COAL, at least 85% in one community) coexist with generalists (the CHAMBER OF COMMERCE, no community above 25%). Generalists and senior leaders cluster on power committees. Civil Society is the only cross-industry domain. The paper concludes that narrow industry interest drives most lobbying. The latent space model and the dc-biSBM give well-aligned communities, and the dc-biSBM discerns more structure than the LSM, but both lack mixed membership.
> 
> For Paul's systems, the most direct point is this. The paper's lobbying network uses the same construction as Melange's lobbied-member link: an organisation is credited to a member through a bill roster. The paper calls the link "a type of political connection" but makes no contact or causal claim, which is consistent with the system's ruling that the link is circular. Three other bearings qualify claims the system makes. First, a brain-vault note cites the paper for community structure in the cosponsor graph, but the paper's communities are group-by-member lobbying communities, and budgetRecruit has no community term. Second, co-lobbying cannot separate lobbying for a bill from lobbying against it, so the paper cannot identify rival-industry champions. Third, leaders' broad memberships show how widely they draw lobbying, not leverage on credit-union issues. Everything here is descriptive, comes from one Congress, and is not causal.

## Paul's annotation

From [[annotations/Annotated Bibliographies & Literature Synthesis - Curated Annotations|Annotated Bibliographies & Literature Synthesis - Curated Annotations]]:

> Kim, In Song, and Dmitriy Kunisky. “Mapping political communities: A statistical analysis of lobbying networks in Legislative Politics.” Political Analysis, vol. 29, no. 3, 23 Nov. 2020, pp. 317–336, https://doi.org/10.1017/pan.2020.29.
>
> This article by MIT professors In Song Kim and Dimitriy Kuinsky applies the theory of Social Network Analysis to the lobbying industry and members of Congress, using a novel Bipartite Link Community Model that captures mixed membership and representative congestion within an MOC. On the topic of “mixed interactions observed between actors,” what this means specifically is that, within the model, one can see that different interactions between the same two actors as a result of one of the actors, such as the Chamber of Commerce, taking on multiple, each distinct, issues. Regarding the present research, it is crucial for both data pipelines to capture congestion, as well as to identify champions for rival or ally industries and measure the level of commitment those champions have toward those industries. Its unique lobbying database is incredibly extensive and utilizes textual extraction from lobbying reports using machine learning. A weakness of this article as it pertains to the present research, is how to transform the bipartite nature of the current model into a graph format, where path-finding algorithms can be run. Regardless, the current model provides a useful tool for identifying champions and connections across rival industries.

From [[annotations/Lobbying - Lobbying - Annotated Bibliography|Lobbying - Lobbying - Annotated Bibliography]]:

> Kim, In Song, and Dmitriy Kunisky. “Mapping political communities: A statistical analysis of lobbying networks in Legislative Politics.” *Political Analysis*, vol. 29, no. 3, 23 Nov. 2020, pp. 317–336, [https://doi.org/10.1017/pan.2020.29](https://doi.org/10.1017/pan.2020.29).

From [[annotations/Social Network Analysis - Social Network Analysis - Annotated Bibliography|Social Network Analysis - Social Network Analysis - Annotated Bibliography]]:

> Kim, In Song, and Dmitriy Kunisky. “Mapping political communities: A statistical analysis of lobbying networks in Legislative Politics.” *Political Analysis*, vol. 29, no. 3, 23 Nov. 2020, pp. 317–336, [https://doi.org/10.1017/pan.2020.29](https://doi.org/10.1017/pan.2020.29).
>
> This article by MIT professors In Song Kim and Dimitriy Kuinsky applies the theory of Social Network Analysis to the lobbying industry and members of Congress, using a novel Bipartite Link Community Model that captures mixed membership and representative congestion within an MOC. On the topic of “mixed interactions observed between actors,” what this means specifically is that, within the model, one can see that different interactions between the same two actors as a result of one of the actors, such as the Chamber of Commerce, taking on multiple, each distinct, issues. Regarding the present research, it is crucial for both data pipelines to capture congestion, as well as to identify champions for rival or ally industries and measure the level of commitment those champions have toward those industries. Its unique lobbying database is incredibly extensive and utilizes textual extraction from lobbying reports using machine learning. A weakness of this article as it pertains to the present research, is how to transform the bipartite nature of the current model into a graph format, where path-finding algorithms can be run. Regardless, the current model provides a useful tool for identifying champions and connections across rival industries.

## Paul's markup in the PDF

- p1 Highlight: "inferring political actors' latent memberships" — 
- p1 Highlight: "collec- tive activity" — 
- p1 Highlight: "o two groups of acto" — 
- p1 Highlight: "actors may be members of more than one communit" — 
- p1 Highlight: "actors to interact in more than one way" — 
- p1 Highlight: "f special interest groups and politician" — 
- p1 Highlight: "difficulty of observing the ties between interest groups and politicians directly" — 
- p1 Highlight: "rangingfromnarrowtargeted interactions according to industry interests and jurisdictional committee membership to broad multifaceted connections across multiple policy domains." — 
- p1 Highlight: "[whole-page highlight, first 40 words] Mapping Political Communities: A Statistical Analysis of Lobbying Networks in Legislative Politics In Song Kim 1 and Dmitriy Kunisky2 1Associate Professor, Department of Political Science, Massachusetts Institute of Technology, Cambridge, MA 02139" — 
- p1 Highlight: "network analysis has benefited from advances in computing" — 
- p1 Highlight: "uccessfully partitioned massive networks of social and political actors into similarly-behaving communities that are simpler to understand" — 
- p1 Highlight: "existing approaches seldom account for various important properties of political networks." — 
- p1 Highlight: "political actors ofen come in distinct types" — 
- p1 Highlight: "formed by interactions between two types of actors, are prevalent in politics" — 
- p1 Highlight: "voters and politicians" — 
- p1 Highlight: "private firms and government entities," — 
- p1 Highlight: "democratic and autocratic countries" — 
- p1 Highlight: "mixed memberships" — 
- p1 Highlight: "multiple interests, motivations, or roles that drive their behaviors" — 
- p2 Highlight: "other times as a local representativ" — 
- p2 Highlight: "combine different alignments of their motivati" — 
- p2 Highlight: "itizen contributes to a politician's campaign may depend on how much they agree with the politician's views on many issues" — 
- p2 Highlight: "Bipartite Link Community Mode" — 
- p2 Highlight: "Expectation-Maximization (EM) algorithm" — 
- p2 Highlight: "identify legislative communities of special interest groups and politicians in the U.S. Congress" — 
- p2 Highlight: "Though these inquiries have yielded many insights, they are limited in scope and treat only a few policy domains in isolati" — 
- p2 Highlight: "combining two types of political behavior related to a bill:" — 
- p2 Highlight: "distilled from a larger database of lobbying data," — 
- p2 Highlight: "natural language processing techniques on mandatory reports filed by lobbyists to identify the interest groups that lobbied on 108,086 congressional bills introduced between the 106th and 114th Congress." — 
- p2 Highlight: "recurring instances of lobbying that involve the same interest group" — 
- p2 Highlight: "targeted lobbying of members of committees" — 
- p2 Highlight: "jurisdiction over their narrow interests." — 
- p2 Highlight: "we find that it is rare for a diverse collection of specialist interest groups to lobby on the same legislation." — 
- p3 Highlight: "redirectlyrelatedtotheirmission" — 
- p3 Highlight: "In fact, our model identifies just one major issue area that is lobbied by diverse interest groups: legislation concerning social and human rights issues." — 
- p3 Highlight: "ivic groups tend to participate in interest group alliances rather than lobbying alone." — 
- p3 Highlight: "distinguish genuinely significant political connections from routine alignments of lobbying and legislative activity" — 
- p3 Highlight: "In our setting, they would require that the numerous interactions between a pair of powerful actors like Senator Barbara Boxer (D-CA) and the CHAMBER OF COMMERCE all arise from a single shared issue of interest." — 
- p3 Highlight: "distinguishing shared broad interests from shared focused interests and numerous interactions on different issues from numerous interactions on the same issue." — 
- p3 Highlight: "The open-source sofware polnet is available as an R package for implementing the proposed methods" — 
- p3 Highlight: "Political Analysis Dataverse (Kim and Kunisky 2020a) and Code Ocean (Kim and Kunisky 2020b)." — 
- p3 Highlight: "and the visualization tools used in preparing this paper will be made publicly available at https://www.lobbyview.org (Kim 2018)." — 
- p3 Highlight: "Wethengiveanabstractmathematical formulation and derive an EM algorithm to estimate the model parameter" — 
- p3 Highlight: "implementation of the proposed method that can be applied by other researchers to analyze bipartit" — 
- p4 Highlight: "Interest groups and politicians also form communities who interact via campaign contributions" — 
- p4 Highlight: "political actor belongs to one communit" — 
- p4 Highlight: "assortative" — 
- p4 Highlight: "but it leaves out a great deal of detail about community structure." — 
- p4 Highlight: "y, political actors ofen have mixed membership" — 
- p4 Highlight: "participate in multiple communities." — 
- p4 Highlight: "represent the interests of diverse constituents." — 
- p4 Highlight: "interested in numerous policy domains (" — 
- p4 Highlight: "A human analyst tasked with identifying legislation communities would certainly not conclude that all three bills belong to a single issue area." — 
- p4 Highlight: "mmSBM family would bizarrely insist that a single common community membership must account for every interaction involving the two actors." — 
- p5 Highlight: "we arrive at a model similar to the link community model (LCM) of Ball, Karrer, and Newman (2011) that we adapt specifically to the bipartite setting." — 
- p5 Highlight: "our model provides an underlying probabilistic generative model and therefore a statistical interpretation." — 
- p5 Highlight: "organized into legislation communities," — 
- p5 Highlight: "two disjoint groups of political acto" — 
- p5 Highlight: "i ∈U = {1,...,m}" — 
- p5 Highlight: "j ∈V = {1,...,n}." — 
- p5 Highlight: "i,j by Ai,j" — 
- p5 Highlight: "interaction matrix A ∈Rm×n." — 
- p5 Highlight: "another parameter κ er of interactions bet isson variables are ind" — 
- p5 Highlight: "e Poisson variables are independent," — 
- p5 Highlight: "The number of total interactions between i and j in community z is modeled as Poisson with a mean proportional to alpha_i,z beta_j,z." — 
- p6 Highlight: "[Poisson likelihood product, eq. (1), extraction garbled]" — 
- p6 Highlight: "the expectation" — 
- p6 Highlight: "[eqs. (2)-(4), extraction garbled]" — 
- p6 Highlight: "beta_j,z = ... rithm is not guaranteed to co [eq. (5) and sentence on EM convergence, extraction garbled]" — 
- p6 Highlight: "50 randomly initialized runs and choose the parameters that attain the highest likelihood value." — 
- p6 Highlight: "Our analysis illus- trates a clear distinction between 'specialist' and 'generalist' interest groups and identifies the legislators connected to both types." — 
- p6 Highlight: "Finally, we discuss the benefits of this methodology over other network models." — 
- p6 Highlight: "However, organiza- tions need not identify their political contacts." — 
- p7 Highlight: "inally, we identify all the actors of the lobbying sector (lobbyists, their lobbying firms, the interest groups that hire them, and so forth) that are involved in lobbying on each bill" — 
- p7 Highlight: "1,111,859 lobbying report" — 
- p7 Highlight: "sponsored by Senator Mitch McConnell (R-KY), the DISTILLED SPIRITS COUNCIL was the only interest group to lobby." — 
- p7 Highlight: "Ai,j to count the number of bills interest grou" — 
- p8 Highlight: "illustrate the range of lobbying behaviors the biLCM identifies in the network." — 
- p8 Highlight: "MICROSOFT and ARCH COAL lobby on a single topic," — 
- p8 Highlight: "th politicians in a single legislation community" — 
- p8 Highlight: "interacting in any single community with probability no more than 25%" — 
- p8 Highlight: "legislators with the most legislation community memberships are ofen senior politicians or party leaders." — 
- p8 Highlight: "(vertical links on the lef and right)" — 
- p9 Highlight: "examples of interest groups," — 
- p9 Highlight: "Legislation communities in the biLCM" — 
- p9 Highlight: "Links indicate the number of interactions belonging to these communities under the biL" — 
- p9 Highlight: ". An entropy of H may then be interpreted as, roughly speaking, an actor typically [garbled] participating in 2H legislation communities." — 
- p9 Highlight: "actors with higher entropy values are am cam c w. members of more legislation communities than actors with lower entropy values. [garbled extraction]" — 
- p10 Highlight: "as the data do not distinguish between lobbying for or against a given piece of legislation." — 
- p10 Highlight: "when do nominally domain-specific interest groups broaden their lobbying activity and lobby on legislation outside of their main interest area?" — 
- p10 Highlight: "wheninterestgroupsbroadentheirinterests,itistolobby on social issue" — 
- p11 Highlight: "thusaccountsforonlyamodestfractionoflobbyingactivity.Therefore, we find evidence that narrow, industry-specific interests remain the primary determinant of lobbying activity, and other motivations play at most a secondary role." — 
- p11 Highlight: "the lobbying network are organized around actors' interests" — 
- p11 Highlight: "groups with a variety of interests may lobby either a variety of politicians or specific politicians with broad power." — 
- p11 Highlight: "Finally, sometimes the interest group and politician connect over individual legislation that is important to a relevant industry" — 
- p11 Highlight: ", for instance often appearing in electoral materials or receiving other publicity." — 
- p11 Highlight: "[empty covered text; small rectangle at page foot]" — 
- p12 Highlight: "obtain a null distribution of random networks with similar activity distribution of individual actors" — 
- p12 Highlight: "we find that the biLCM successfully describes the multiple community memberships of political actors and how interactions among actors occur in those communities." — 
- p12 Highlight: "It thereby provides a useful guide to the varied interests and complex preference aggregation that drives lobbying in legislative politics" — 
- p15 Highlight: "Unlike the LSM, the biSBM explicitly models interactions as a function of community memberships; however, it lacks the mixed memberships and mixed interactions of the biLCM." — 
- p15 Highlight: "The dc-biSBM suffers from the same drawbacks as the LSM: it identifies single community memberships in the lobbying network but not the multiple memberships that reflect actors' actual varied interests." — 
- p16 Highlight: "5. The figure shows that the LSM and dc-biSBM community structures are remarkably well-aligned." — 
- p16 Highlight: "and actors in their intersections indeed tend to lobby on bills related to the subject matters of both communities" — 
- p17 Highlight: "Second, the biLCM can be used to estimate ideal points (Poole and Rosenthal 2011) by considering roll calls as bipartite networks between legislators and bills." — 
- p17 Highlight: "unsuitable for existing community detection methods" — 
- p17 Highlight: "As an example where our model reveals new network structure, we consider interest group politics." — 
- p17 Highlight: "We therefore construct a network of interactions in the 113th Congress by connecting interest groups to legislators via the bills that were lobbied, information the groups must disclose." — 
- p17 Highlight: "Interest groups with concerns in these domains lobby politicians who sit on committees with relevant jurisdiction," — 
- p17 Highlight: "stly,interestgroups' activity is typically dictated by their domain-specific interests rather than broader commitments, with the important exception of social issues." — 
- p17 Highlight: "It would be interesting to either perform rigorous model selection" — 
- p18 Highlight: "tensor regression." — 
- p18 Highlight: "ours is the first statistical study of lobbying networks in legislative politics that examines both politicians and interest groups using micro data." — 
- p1 Highlight: "1) applies to two groups of actors" — 
- p1 Highlight: "akes into account that actors may be members of more than one communit" — 
- p1 Highlight: "3) allows a pair of actors to interact in more than one way" — 
- p1 Highlight: "Previous empirical studies of interest group politics have been limited by the difficulty of observing the ties between interest groups and politicians directly" — 
- p1 Highlight: "connects the politicians who sponsor congressional bills" — 
- p1 Highlight: "biLCMtomakequantitativemeasurementsofactors' communitymembershipsrangingfromnarrowtargeted interactions according to industry interests and jurisdictional committee membership to broad multifaceted connections across multiple policy domains." — 
- p1 Highlight: "etwork analysis has benefited from advances in computing," — 
- p1 Highlight: "ffer unprecedented capacity to record and analyze large quantities of political interaction" — 
- p1 Highlight: "however, existing approaches seldom account for various important properties of political networks." — 
- p1 Highlight: "political actors ofen come in distinct types that determine the nature of their interactions" — 
- p1 Highlight: "bipartite networks, formed by interactions between two types of actors, are prevalent in politics" — 
- p1 Highlight: "Ignoring this extra network structure may severely compromise the validity of network analyses" — 
- p1 Highlight: "ofen driven by mixed memberships in political communities" — 
- p2 Highlight: "allowing researchers to apply this method to bipartite networks with mixed memberships and interactions and thereby measure how actors' activity is distributed across communities." — 
- p2 Highlight: "First, we find that 'specialist' and 'generalist' interest groups coexist in U.S. legislative politics." — 
- p2 Highlight: "target politicians who are members of broad 'power committees'" — 
- p3 Highlight: "dentifying connections that deviate most from the predictions of the biLCM, we are able to distinguish genuinely significant political connections from routine alignments of lobbying and legislative activity, such as energy firms lobbying on energy bills written by members of an energy committee." — 
- p3 Highlight: "Political networks are ofen organized into distinct communities within which actors typically interact." — 
- p4 Highlight: "In these models, each political actor belongs to one community," — 
- p4 Highlight: "In reality, political actors ofen have mixed memberships, meaning they participate in multiple communities." — 
- p4 Highlight: "that have distinct legislative jurisdictions." — 
- p4 Highlight: "mixed- membership stochastic block model (mmSBM)" — 
- p4 Highlight: "and members in the same community tend to interact more frequently with each other than with actors in other communities." — 
- p4 Highlight: "Yet a model in the mmSBM family would bizarrely insist that a single common community membership must account for every interaction involving the two actors." — 
- p5 Highlight: "In this way, we arrive at a model similar to the link community model (LCM)" — 
- p5 Highlight: "who treat the link community detection task in bipartite graphs as an optimization problem that they solve with an ad hoc genetic algorithm." — 
- p5 Highlight: "that actors are ofen members of multiple communities, and that two actors may interact in more than one community" — 
- p5 Highlight: "less prominent actors with narrower interests within the same model." — 
- p5 Highlight: "We denote the number of interactions between the pair i,j by A i,j ," — 
- p5 Highlight: "Each actor i and j has a vector of parameters alpha i,z and beta j,z, respectively," — 
- p6 Highlight: "The update equa- tions produced by our derivation are given below," — 
- p6 Highlight: "q i,j (1),...,q i,j (k )" — 
- p6 Highlight: "we perform 50 randomly initialized runs and choose the parameters that attain the highest likelihood value." — 
- p6 Highlight: "All lobbying organizations are required by law to file quarterly reports describing the issues on which lobbyists have engaged in political activities," — 
- p7 Highlight: "The lef panel of Figure 2 shows that about 12,000 bills are introduced in each Congress and the majority of them are lobbied by at least one interest group. V" — 
- p8 Highlight: "finding that the legislators with the most legislation community memberships are ofen senior politicians or party leaders." — 
- p9 Highlight: "Simply put, actors with higher entropy values are am cam c w. members of more legislation communities than actors with lower entropy values." — 
- p10 Highlight: "These committees hold the 'power of the purse,' controlling various financial aspects of governance" — 
- p12 Highlight: "In the specific case of legislation communities, the biLCM allows researchers to relate interest groups with specialized and general interests to politicians with narrow and broad political roles." — 
- p12 Highlight: "respectively, and assume that interactions have independent Poisson distributions." — 
- p16 Highlight: "The dc-biSBM thus furnishes yet more evidence that mixed membership modeling is required in order to account fully for the organization of the lobbying network." — 
- p17 Highlight: "(1) networks consisting of two distinct types of actors, (2) interactions between actors driven by 'mixed memberships' or multiple interests and motivations, and (3) pairs of actors having 'mixed interactions' due to different alignments of interests." — 
- p17 Highlight: ".Lastly, interest groups' activity is typically dictated by their domain-specific interests rather than broader commitments, with the important exception of social issues." — 
- p18 Highlight: "To the best of our knowledge, ours is the first statistical study of lobbying networks in legislative politics that examines both politicians and interest groups using micro data." — 

## Study

**Question:** How can latent memberships of political actors in the communities that drive their observable interactions be inferred, when the network is bipartite, actors hold several memberships, and a pair of actors can interact in several ways? Application: which legislation communities organize interest-group lobbying of legislators in the 113th Congress, and when do domain-specific groups lobby outside their domain?

**Theory:** Interest-group influence runs through repeated, narrow issue ties. Lobbying is organised into legislation communities that group interest groups with the legislators who hold relevant jurisdiction (e.g. Energy, Veterans' Affairs). Actors can have mixed memberships, because groups with broad interests and politicians with broad roles span several communities. A pair can also have mixed interactions, since one dyad's bills can belong to different issue communities. Industry-specific interests are the primary driver of lobbying; other motives play at most a secondary role. Social issues (the 'Civil Society' community) are the one broad cross-industry domain (pp. 10-11, 17). Power committees (Ways and Means, Appropriations) carry high-entropy politicians (p. 10). Anomalous dyads with more interactions than predicted suggest geography, campaign-contribution ties, or bill-specific stakes (p. 11). The model is motivated by the biSBM and mmSBM literatures (pp. 3-5).

**Data:** Lobbying reports: 1,111,859 reports filed 1999-2017, yielding 2,396,693 issue corpora, 20,092 special interest groups and 1,164 current or former members of Congress (p. 7). Bills: 108,086 bills introduced in the 106th-114th Congresses were identified by NLP on lobbying reports (p. 2). About 12,000 bills are introduced per Congress, and most are lobbied by at least one group (Figure 2, p. 7). Analysis sample: the 113th Congress network. Unit: interest group i by politician j dyad, with A_ij = number of bills group i lobbied that politician j sponsored (p. 7). The main text does not state the 113th-Congress N of groups and politicians. Lobbied-bill distribution in the 113th Congress: median 3 groups per bill, maximum 946 (H.R. 1), and 23% of lobbied bills have one lobbying group (Figure 2 caption, p. 7). Session identification for bill references used report text signals, bill-text similarity, and 'Act of [YEAR]' phrases (p. 7).

**Method:** Bipartite Link Community Model (biLCM): A_ij ~ Poisson(sum over z of kappa_z * alpha_iz * beta_jz), with z = 1..k link communities (eq. 1, p. 5). Estimation by EM with ancillary parameters q_ij(z) (eqs. 2-5, p. 6). Normalisation: sum_i alpha_iz = sum_j beta_jz = 1 for each z. k = 8 communities, chosen with reference to community structure seen in the latent space model (fn. 11). 50 random starts; highest likelihood kept (p. 6). Membership shares p_iz and q_jz are normalised kappa_z-weighted alphas and betas; entropy H in bits, with about 2^H communities actively used (p. 9). Comparison models: (a) 2-D latent space model with Poisson mean exp(alpha_i + beta_j - ||theta_i - psi_j||^2), Bayesian in Stan with hierarchical priors (eq. 6, pp. 12-13); (b) degree-corrected bipartite SBM (dc-biSBM), EM from Larremore et al. (2014), k = l = 8, 50 starts (eq. 7, pp. 15-16). Significance: permutation test against a null that keeps actor-level activity distributions (p. 12). Anomalies: observed versus biLCM-estimated counts, ranked by deviation (Table 1, p. 11).

**Identification:** Not causal. The model is identified by the normalisation constraints on alpha and beta. Communities are latent structure inferred from co-lobbying counts; the paper makes no treatment-effect identification claim. Community names and committee matches are the authors' judgement from the leading groups in each community (p. 7).

**Scope:** Population: interest groups and members of Congress who co-appear through lobbying disclosures (1999-2017 database), with the empirical application restricted to the 113th Congress (2013-2014). Institution: US Congress and federal lobbying disclosure. Era: 1999-2017 data; 113th Congress for estimates. The method is general to bipartite networks with mixed memberships; Section 3.4 (pp. 17-18) lists proposed uses in patronage networks, voter-politician contributions, roll-call ideal points and global value chains, none tested in the paper.

## Key findings

- Eight legislation communities are named by their leading groups: Healthcare, Veterans' Affairs, Technology & Telecommunications, Energy, Finance & Insurance, Universities & Research, Civil Society, and Retail & Transportation. (k = 8; p. 7, paragraph 'We name the legislation communities')
- Specialist groups lobby a single community; generalists spread across several. MICROSOFT and ARCH COAL interact with one community at probability of at least 85%. MCAFEE interacts with Technology & Telecom at 31% and Universities & Research at 52%. PHILIPS NORTH AMERICA and the CHAMBER OF COMMERCE have no single community above 25%. (MICROSOFT, ARCH COAL: at least 85% in one community; MCAFEE: 31% and 52%; PHILIPS and CHAMBER: at most 25% per community; p. 8, Figure 3 and text)
- Specialist and generalist interest groups coexist. Targeted groups lobby committees with narrow jurisdiction; broad groups target power committees (Ways and Means, Senate Appropriations). (not stated as a single statistic; p. 2 (intro, para. 'First'); p. 10 (Figure 4))
- Only one broad issue area is lobbied by diverse groups: Civil Society, covering social and human-rights legislation. It is the fourth most active of eight communities by kappa_z, so it accounts for only a modest share of lobbying activity. (fourth of 8 by kappa_z; p. 3 (intro); p. 11 ('On the other hand'))
- Politicians with high entropy, meaning membership in at least four communities, sit on power committees such as Ways and Means and Senate Appropriations, which control the power of the purse. (entropy at least 2 corresponds to at least 4 communities; 52% of the 113th Congress were Republican (Figure 4 caption); p. 10, Figure 4 and text)
- Politicians interacting with a community at probability at least 40% sit on committees matching its subject (e.g. Financial Services for Finance & Insurance; Armed Services for the defence-related group). (threshold of 40% for committee groups; p. 10 (committee analysis))
- Anomalous dyads (observed far above biLCM prediction) are explained by geography, campaign contributions, or bill-specific stakes rather than shared issues. Examples: Tom Marino-CVS 10 observed vs 0.55 expected; Michael Enzi-eBay 11 vs 1.08; Jeanne Shaheen-Novo Nordisk 12 vs 1.61; Mike Conaway-iHeartMedia 7 vs 0.17. (10 dyads listed; largest gap Shaheen-Novo Nordisk 12 observed vs 1.61 estimated; p. 11, Table 1 and text)
- Community structure is significant. In the permutation null (networks with the same actor-activity distributions), interest groups have much higher numbers of community memberships than in the observed network, so the observed network has strong community structure. (not quantified in the main text (test details in Appendix B.2, not read); p. 12, 'Significance of Communities'; fn. 13)
- The 2-D latent space model finds the same single-community clusters (e.g. Technology adjacent to Telecom and Energy; Universities adjacent to Industrial Research and Healthcare). Actors with mixed memberships fall in the centre of the latent space, where the LSM is uninformative. The paper attributes this to stochastic-equivalence confounding (Minhas et al. 2019). (2-D LSM; high-entropy hexagons cluster near the centre; pp. 12-13, Figures 5-6)
- The dc-biSBM recovers community boundaries that match the LSM clusters, and it finds two central communities (Civil Society and Retail & Transportation). It still assigns each actor to one community, so it misses mixed memberships. Chamber of Commerce, BP and McAfee fall in overlaps of community pairs. (k = l = 8 for dc-biSBM; pp. 15-16, Figure 7; fn. 15-16)
- The biLCM is proposed as a general tool: for ideal-point estimation from roll calls treated as bipartite networks, for patronage and campaign-contribution networks, and for allocation of intermediate products in global value chains. These are proposed uses, not tested in the paper. (none; p. 17, Section 3.4 and 'Second')

## Limitations

- The number of communities k must be chosen in advance; the paper calls for model selection or a Dirichlet-process prior (p. 17).
- No covariates are used, though rich politician and group data exist (p. 17).
- The model handles two-mode networks only; multi-partite interactions (group, politician, bill) are left to future work, e.g. tensor regression (pp. 17-18).
- The data do not distinguish lobbying for from lobbying against a bill (p. 10), so alignment is inferred from co-lobbying only.
- Lobbying reports do not name the politicians contacted; the network is built indirectly from sponsored bills (pp. 6-7).
- Bill identification by NLP is error-prone, and session identification for bills referenced only by number is hard; the paper cites OpenSecrets misattributions (fn. 8, p. 7).
- EM is not guaranteed to reach the global maximum, so the paper uses 50 random starts (pp. 6, 16).
- The empirical application uses one Congress (113th); cross-session comparison is preliminary (fn. 5; Appendix B.3).
- Anomaly explanations (geography, contributions, bill salience) are offered as suggestive and are not tested (p. 11).

## Short quotes

- "narrow, industry-specific interests remain the primary determinant of lobbying activity" (p. 11, 'Lobbying Beyond Specific Interests')

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-lobbying#Lobbying member-year network and circular lobbied-member link\|Lobbying member-year network and circular lobbied-member link]] | supports | Lobbying reports need not identify the politicians contacted, so the network is built indirectly: A_ij = number of bills group i lobbied that politician j sponsored (113th Congress, k = 8). The authors call this 'a type of political connection' and read recurring co-occurrence as 'shared involvement on specific political issues', while stating that lobbying a bill 'does not necessarily imply political ties to its sponsor'. They make no claim that the link is contact or that lobbying causes sponsorship. Window: 113th Congress; denominator: lobbied bills with an identified sponsor. (p. 2 (dataset framing); p. 6 (Sec. 3.1, 'organizations need not identify their political contacts'); p. 7 (A_ij definition, k = 8)) | build_network_2021_rebuild.py credits every legislator who sponsored OR cosponsored each lobbied bill and emits lob_id x bioguide_id x year x n_bills rows. Project memory records this link as circular: P(joined \| lobbied) = 1 by construction, so it cannot serve as influence evidence. `fec/lobbying/build_network_2021_rebuild.py:13; fec/lobbying/build_network_2021_rebuild.py:67` | Same bill-roster construction, but the system's is broader (adds cosponsors) while the paper uses sponsors only, so the dyads are not identical. The paper avoids circularity only because it makes no lobbying-predicts-sponsorship claim; any Melange reuse must stay descriptive. Under D-021 the matrix is an identity source, not an edge or weight source. | strong | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Best-sponsor, multi-target tree and budget max-coverage modes\|Best-sponsor, multi-target tree and budget max-coverage modes]] | qualifies | Communities are latent memberships in the bipartite interest-group x politician lobbying network (k = 8, 113th Congress). Legislators with the most community memberships are often senior politicians or party leaders; legislators with entropy at least 2 (roughly 4+ communities) sit on power committees. The dc-biSBM, being a hard partition, assigns mixed actors (CHAMBER OF COMMERCE, BRITISH PETROLEUM, MCAFEE) to only one community, and the LSM places high-entropy actors in an undifferentiated centre region. (p. 8 (Table F.1 reference); p. 10 (Figure 4, entropy >= 2); pp. 13-16 (LSM and dc-biSBM comparison, Figures 5-7)) | The brain-vault note actors/melange-dashboard-june-capstone.md (line 83) cites Kim-Kunisky for 'the cosponsor network has real community structure the pathfinder exploits'. The template itself carries no Kim-Kunisky citation; budgetRecruit's comment cites GCOMB only. budgetRecruit is a max-coverage greedy over member-member cosponsor edges with Noisy-OR coverage and degree pruning and contains no community term. `fec/surfaces/templates/make_dashboard_html_00.tpl:773-795; C:/Users/pgoff/Documents/brain/actors/melange-dashboard-june-capstone.md:83` | Does not carry over as cited: the paper's communities belong to an organisation x member lobbying graph, not the member-member cosponsor graph, and the code uses no communities. The brain note's citation should be re-scoped. If a community term is ever added, the paper argues for mixed membership over a hard partition because hub actors are the ones a partition misassigns. | strong | VERIFIED-SOFTENED |
| [[systems/moneyball-rank-pillars#Committee - Influence pillar\|Committee - Influence pillar]] | supports | Legislators who interact with probability at least 40% in the 'Finance & Insurance' community have committee memberships concentrated in relevant committees such as House Financial Services; high-entropy legislators sit on power committees (House Ways and Means, Senate Appropriations), and broad groups such as the CHAMBER OF COMMERCE target them. Window: 113th Congress; lobbying-attention counts, not outcomes. (p. 2 (introduction); p. 10 (Figure 4 and text)) | Committee score = (Membership + Leadership) / 2, with seat base weights set by the owner-judged CU-relevance tier marker (Highly 3.0, Moderately 1.8, Slightly 0.9), subcommittee seats at 1/6, and Chair/Ranking/Vice leadership multipliers. The score feeds the Influence pillar. `fec/engine/committee_scores.py:125` | Supports the premise that lobbying attention is organised by jurisdiction. It does not show seats produce CU outcomes. One Congress, descriptive. The power-committee finding suggests checking how tax-writing and appropriations seats are tiered; not verified here. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-labels-and-tiers#Per-industry tier boards, industry-axes tensor and crown law\|Per-industry tier boards, industry-axes tensor and crown law]] | qualifies | 'Finance & Insurance' is one of the eight named legislation communities. The paper states the data 'do not distinguish between lobbying for or against' a bill, and therefore analyses domain broadening rather than alliances. (That co-lobbying would place rival finance interests in one community is an inference, not a paper finding.) (p. 7 (community naming); p. 10 ('Lobbying Beyond Specific Interests')) | champions_v2.py builds direction-aware, mutually exclusive champion boards over sectors credit_union, bank, crypto, fintech, telecom, with the crown law: Tier-1 crowns never eligible for any threat board. primacy_factors.py validates a signed industry-axis valence in [-1, 1]. `fec/lobbying/champions_v2.py:7-8; fec/lobbying/champions_v2.py:32; fec/engine/primacy_factors.py:175-181` | The biLCM cannot supply rival-industry champions because it carries no sign. Separating banks from CUs needs a signed position source (e.g. Kim J. et al. 2025). The paper neither supports nor refutes the crown law. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-legislative-data#Ballotpedia committee, caucus and party-leadership scores\|Ballotpedia committee, caucus and party-leadership scores]] | qualifies | Legislators with the most legislation-community memberships are often senior politicians or party leaders, and high-entropy legislators (entropy >= 2, roughly 4+ communities) sit on power committees. The paper measures breadth of lobbying interaction, not leverage. (p. 8; p. 10 (Figure 4)) | Hand-set constants: committee leadership Chair 1.0, Ranking 0.8, Vice Chair 0.6, Vice Ranking 0.4, Member 0.2; party leadership Speaker 1, President pro tempore 1, Majority Leader 0.8, Minority Leader 0.6, Majority Whip 0.4, Minority Whip 0.2. `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/ballotpedia_119th_cu_scraper.R:30-42` | Consistent with scoring leadership as general reach rather than CU-specific leverage; the 'diluted industry share' reading is the analyst's inference. Says nothing about effectiveness on CU bills. One Congress, descriptive. | weak | VERIFIED-SOFTENED |
| [[systems/melange-sna#Sector network configurations\|Sector network configurations]] | suggests-extension | Legislation communities (Healthcare, Veterans' Affairs, Technology & Telecommunications, Energy, Finance & Insurance, and others) are estimated from interest-group x politician lobbying counts and named ex post. A permutation test against random networks with similar actor activity distributions finds interest groups in the null have many more community memberships, indicating strong community structure. (p. 7; p. 12 ('Significance of Communities')) | Sector networks (bank_reg defined by CRS subject terms, among others) are member-member cosponsor graphs over hand-defined sector bill universes, configured in the NETWORKS dict. `fec/moneyball_core.py:391-402` | A biLCM on the existing org x bioguide counts could only be a descriptive diagnostic of whether hand-drawn sector boundaries appear in lobbying. Under D-021 its outputs must not become edges or weights; | weak | VERIFIED |
| [[systems/melange-lobbying#FEC to LDA bridge test\|FEC to LDA bridge test]] | suggests-extension | Table 1 lists the 10 dyads whose observed interactions most exceed biLCM predictions (e.g. Shaheen-NOVO NORDISK 12 observed vs 1.61 estimated; Marino-CVS 10 vs 0.55). The authors suggest explanations from geography, campaign contributions (IHEARTMEDIA with Conaway and Barrasso) and bill-specific stakes, without testing them. (p. 11, Table 1 and 'Anomalous Connections') | The preregistered round-3 FEC x LDA bridge test found money contributes -0.0002 beyond structural controls, so FEC data is excluded as a feature of the lobbying champions residual rank. Project memory records the raw rho as +0.153 and the result as UNCORROBORATED. `fec/lobbying/lobby_champions.py:13-15` | Suggests testing whether FEC money concentrates on dyads with positive community-model residuals rather than correlating levels. Hypothesis only; anecdotes from one Congress. Residuals on a bill-roster join inherit its circularity for any join outcome; a different control set on the same window is not a successor test. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: 'The paper never treats that link as contact or influence' overstates it: the paper calls the bill-roster link 'a type of political connection' and reads recurring co-occurrence as shared issue involvement, while cautioning it does not necessarily imply political ties. It makes no contact or causal claim, but it does not disclaim connection.
> - Summary issue: 'The dashboard cites the paper for exploitable community structure' is wrong: no Kim-Kunisky citation exists anywhere in fec/surfaces (budgetRecruit's comment cites GCOMB). The citation is in the brain-vault note actors/melange-dashboard-june-capstone.md:83. The same error appears in flags[0] ('the dashboard's budget-mode grounding').
> - Summary issue: gaps_exposed item 3 ('the system has no residual or anomaly view' of organisation-member dyads) is wrong: fec/lobbying/lobby_champions.py already publishes a residual rank of organisation-filing x member-bill association after structural adjustment, and champions_v2.py references that 'direction-blind residual attention'.
> - Summary issue: Bearing 1 system_side cited '63,991 member-years'; the figure was not found in the repo, brain or memory and was removed.
> - Summary issue: Bearing 2 attributed 'senior members and party leaders typically span at least 4 communities' to the paper; the paper reports leaders have the most memberships and, separately, that entropy >= 2 legislators sit on power committees.
> - Summary issue: 'A latent space model and a dc-biSBM recover the same blocks' is loose: the paper says LSM and dc-biSBM communities are well aligned with each other and the dc-biSBM discerns more structure than the LSM; both lack mixed membership.

## Gaps and flags

- No mixed-membership or overlapping community model exists anywhere in the member SNA. Press HCoI uses Louvain hard partitions, which the paper shows misassign exactly the high-entropy hub members (leaders, power-committee members).
- The org x member lobbying count matrix (lob_id, bioguide_id, year, n_bills) already exists but has no actor-activity-preserving null model. Any structure read from it is untested against a degree-preserving permutation baseline like the paper's.
- No for/against signal attaches to LDA dyads (position ships empty, position_basis='not_disclosed'), so rival-industry structure cannot be read from lobbying co-attention. That needs a signed position source.
- CITATION MIS-TRANSFER: the brain note in melange-dashboard-june-capstone (line 83) cites Kim-Kunisky for community structure in the COSPONSOR network. The paper studies the bipartite lobbying network, and budgetRecruit (make_dashboard_html_00.tpl:779-795, whose comment cites GCOMB) uses no community term. The brain note's citation overstates the grounding.
- D-021: the paper's core object is an LDA-derived group x politician network. Any proposal to fit a biLCM and feed its memberships or dyad intensities into kernels, edges or weights would breach D-021. Diagnostic and descriptive use only.
- Circularity: the paper's A_ij uses the same bill-roster join as the refuted lobbied-member link. The paper makes no contact or causal claim for lobbied-to-sponsored co-occurrence, and nothing in it licenses reading that co-occurrence as contact or influence.
- Not a rejected-SNA argument: the paper uses sponsors only and models actor activity through degree terms (kappa x alpha x beta, dc-biSBM). Neither is an argument to drop regular cosponsorship edges or to apply per-edge volume normalisation to the member graph, so neither rejected proposal should be read into it.
- Window and denominator: every figure in the paper covers the 113th Congress (2013-14), with bills identified by NLP from 1999-2017 LDA reports. Do not quote its shares (85%, 25%, 40%) as properties of the 119th Congress or of CU lobbying.

Partly aligned. Paul reads Kim and Kunisky as a tool for identifying champions (lobbied-member-link and industry-boards-crown). The paper does not support that reading. It measures shared co-lobbying attention, not alignment or contact, and its finance community would merge banks and CUs, so it cannot find rival-industry champions. His note is stale relative to the system: the system has since ruled the same bill-roster construction circular (P(joined | lobbied) = 1), which the notes do not reflect. His committee-pillar citation (money and lobbying concentrate in jurisdictional committees) is supported by the 40%-threshold committee-matching result. His leadership citation is supported as breadth, not leverage. His path-finding gap, turning bipartite lobbying data into a graph path-finding can run on, is real, but the brain-vault note's Kim-Kunisky citation for budget mode claims more grounding than the code uses. No annotation was recorded in his takeaways for this work.

## Related papers

- [[kim-2018-lobbyview-firm-level]] — same-data — The paper's lobbying dataset is distilled from LobbyView (cited as Kim 2018) and shares its NLP bill-identification pipeline.
- [[kim-2025-measuring-interest-group]] — complements — Supplies signed bill positions from LDA text, which Kim-Kunisky say their data lack (no for/against distinction).
- [[porter-2005-network-analysis-committees]] — cites — Kim-Kunisky cite Porter et al. 2005 when proposing biLCM for networks of legislators and committees; both are also cited together in the brain-vault dashboard capstone note (not in dashboard code).
- [[bacik-2025-measuring-dynamical-evolution]] — same-data — Also built from LobbyView (co-authored by In Song Kim), as a yearly multi-layer network over time, where Kim-Kunisky give a single-Congress (113th) snapshot.
- [[azaiez-2025-hypergraph-analysis-european]] — complements — A hypergraph treatment of a lobby network; Kim-Kunisky list multi-partite (group-politician-bill) extension as future work.
- [[aref-2021-identifying-hidden-coalitions]] — complements — Signed partitioning of the House co-sponsorship network supplies a sign Kim-Kunisky's co-lobbying lacks, but is a hard partition without mixed membership.

### Linked from
- [[defigueiredo-2013-advancing-empirical-research]] — complements — Kim and Kunisky on lobbying networks built from bill-level LDA links. The essay's targeting gap in transaction data applies to member edges built this way; the item's content was not re-read.