---
type: paper
title: "Social Network Analysis: An Approach and Technique for the Study of Information Exchange"
authors: ["Caroline Haythornthwaite"]
year: "1998"
venue: "Library & Information Science Research, vol. 18, pp. 323-342"
doi: ""
category: "Social Network Analysis"
systems: ["melange-influence-engine", "melange-sna", "melange-staffer-network", "moneyball-rank-pillars"]
components: ["melange-sna.cosponsor-network", "melange-influence-engine.cascade-kernel", "moneyball-rank-pillars.network-centrality-influencescore", "melange-staffer-network.layer-balanced-pagerank", "melange-staffer-network.cotenure-exposure-weights", "melange-staffer-network.overlap-edge-rule", "melange-sna.multilayer-social-press-edges", "melange-sna.press-hcoi-communities"]
paul_annotated: true
paul_markup_count: 91
evidence_status: "verified"
source: "[[_sources/haythornthwaite-1996-social-network-analysis.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\1-s2.0-S0740818896900031-main.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\1-s2.0-S0740818896900031-main.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\1-s2.0-S0740818896900031-main.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Social Network Analysis\\1-s2.0-S0740818896900031-main.pdf"]
sha12: ["af58a638ea0f"]
tags: [paper, social-network-analysis]
---

# Social Network Analysis: An Approach and Technique for the Study of Information Exchange

**Caroline Haythornthwaite (1998).** *Library & Information Science Research, vol. 18, pp. 323-342.* Theme: [[themes/social-network-analysis|Social Network Analysis]].

> [!abstract] Summary
> Haythornthwaite's article (LISR 18:323-342; the printed header says 1998, and it began as a 1996 ALISE talk) is an expository introduction to social network analysis. It has no data, sample or identification strategy. It defines a relationship by three attributes: content, direction and strength. A tie between two actors bundles all the relationships they maintain. Strength can be carried as the value of a link or considered within a presence-absence dichotomy by requiring a minimum frequency. It sets out five principles (cohesion, structural equivalence, prominence, range and brokerage) and the standard measures: density, centralization, cliques, degree, closeness, betweenness and block modelling. Its Figure 3 shows why betweenness can capture brokerage that degree misses. Drawing on Coleman, Katz and Menzel (1966), it separates sources that inform from close ties that legitimate adoption. It also argues that information becomes useful only when someone forwards it.
> 
> For Paul's systems this is a vocabulary paper, not evidence. No number in it carries over. Its real use is as a checklist against the graphs as built:
> - **Direction.** The R Stage 3 scrape keeps sponsor-to-cosponsor direction. The AUG workbook sorts each edge's keys and `load_network` averages the two directions, so under the cu config (symmetrize True) the cascade kernel runs on an undirected graph; holistic_norm stays directed (moneyball_core.py:398-400).
> - **Strength.** Edges are valued (original cosponsor 1.0, regular 0.5, times a per-bill factor f_bill, which combines a 3.0 success multiplier, a bill decay weight and a 1/sqrt size norm), so strength is kept as a value, as the article says it can be.
> - **Multiplexity.** Layers are never summed. Media edges are admitted only for untied pairs on the CU graph; under the holistic config's dominance rule a tied pair takes the larger of the two weights, so a second channel can raise a tied weight to the larger value.
> - **Brokerage.** Betweenness exists only inside the retired InfluenceScore. The staffer graph measures diversity across node kinds, not structural holes.
> 
> The informing-versus-legitimating split is a loose conceptual parallel to the engine's two-factor kernel: one factor for diffusion, one for pressure.

## Paul's annotation

From [[annotations/Social Network Analysis - Social Network Analysis - Annotated Bibliography|Social Network Analysis - Social Network Analysis - Annotated Bibliography]]:

> Haythornthwaite, Caroline. “Social Network Analysis: An approach and technique for the study of Information Exchange.” *Library &amp; Information Science Research*, vol. 18, no. 4, Sept. 1996, pp. 323–342, [https://doi.org/10.1016/s0740-8188(96)90003-1](https://doi.org/10.1016/s0740-8188(96)90003-1).
>
> This article is the foundational study of the Social Network Analysis method, providing an original overview of its methodology, reasoning, and empirical workings. By dissecting the method of how it works, various relationships ultimately leading to a causal outcome, Social Network Analysis can be applied empirically, with an almost unlimited variation of data collection, to different domains. When applying the method across domains, the article notes that one must consider how the general availability of information affects the ultimate validity of the structure, which is particularly important in the field of politics, where word of mouth and behind-closed-doors dealings are common. Finally, it emphasizes the directionality and structure of the ultimate social network as paramount when examining emerging leaders, trends, and overall findings. While the article is excellent at viewing social networks from a theoretical perspective, it is thin on how to construct graphs and build data pipelines, likely due to its age. Diagnosis: Priority – Medium – Theory

## Paul's markup in the PDF

- pPDF p.1 (printed 323) Highlight: "Paraphrase: SNA is an approach for studying the exchange of resources among actors." — 
- pPDF p.1 (printed 323) Highlight: "Paraphrase: actors are nodes and information-exchange relationships are the connecting links." — 
- pPDF p.1 (printed 323) Highlight: "Paraphrase: relationships structure how information flows among actors." — 
- pPDF p.1 (printed 323) Highlight: "Paraphrase: information opportunities for individuals and groups." — 
- pPDF p.1 (printed 323) Highlight: "Paraphrase: opportunities are assessed by exposure to and control of information." — 
- pPDF p.2 (printed 324) Highlight: "Paraphrase: each relationship refers to one type of resource exchange." — 
- pPDF p.2 (printed 324) Highlight: "Paraphrase: actors can be organizations or concepts, such as libraries in an interlibrary loan network." — 
- pPDF p.2 (printed 324) Highlight: "Paraphrase: relationship patterns show exposure to information and how far it is trusted." — 
- pPDF p.2 (printed 324) Highlight: "Paraphrase: position in the network determines whether an actor can facilitate or control flow." — 
- pPDF p.2 (printed 324) Highlight: "Paraphrase: a health-information dissemination example of what relationship questions to ask." — 
- pPDF p.2 (printed 324) Highlight: "OCR-garbled fragment (graph and matrix form of networks, compared with roads)." — 
- pPDF p.2 (printed 324) Highlight: "OCR-garbled fragment (nodes and networks heading)." — 
- pPDF p.2 (printed 324) Highlight: "Paraphrase: network structures can be drawn as graphs or matrices." — 
- pPDF p.3 (printed 325) Highlight: "Paraphrase: social structure is a familiar concept in models of society." — 
- pPDF p.3 (printed 325) Highlight (green): "Paraphrase: social structure describes the environment through actors' control of and access to resources." — 
- pPDF p.3 (printed 325) Highlight: "Paraphrase: structure is derived from observed relationships, not from a priori classes." — 
- pPDF p.3 (printed 325) Highlight: "Paraphrase: co-workers form networks around the exchange of job-related information." — 
- pPDF p.4 (printed 326) Highlight: "Paraphrase: relationship-centred maxim for SNA, attributed to Wasserman and Faust." — 
- pPDF p.4 (printed 326) Highlight: "Paraphrase: concepts, data and tests in SNA are all relational." — 
- pPDF p.4 (printed 326) Highlight: "Paraphrase: pairs linked by a relationship are tied by all the relationships they maintain." — 
- pPDF p.4 (printed 326) Highlight (green): "Paraphrase: three attributes of relationships are content, direction and strength." — 
- pPDF p.4 (printed 326) Highlight: "Paraphrase: relationships are characterized by the content they carry." — 
- pPDF p.4 (printed 326) Highlight: "Paraphrase: question phrasing reveals who gives what to whom." — 
- pPDF p.5 (printed 327) Highlight: "Paraphrase: direction shows the flow of information or resources." — 
- pPDF p.5 (printed 327) Highlight: "Paraphrase: a manager-to-employee work-giving tie is an example of a one-way relationship." — 
- pPDF p.5 (printed 327) Highlight: "Paraphrase: strength is the intensity of a relationship." — 
- pPDF p.5 (printed 327) Highlight: "Paraphrase: a tie can be dichotomized by a frequency threshold, and strength can be kept as a value." — 
- pPDF p.5 (printed 327) Highlight: "Paraphrase: tie strength depends on the number and types of relationships a pair maintains." — 
- pPDF p.6 (printed 328) Highlight: "Paraphrase: Granovetter's point that weakly tied contacts reach different circles and information." — 
- pPDF p.6 (printed 328) Highlight: "Paraphrase: each tie type indicates how connected an individual is within networks." — 
- pPDF p.6 (printed 328) Highlight: "Paraphrase: information flows directionally between actors." — 
- pPDF p.6 (printed 328) Highlight: "Paraphrase: definition of egocentric networks, seen from one actor's perspective." — 
- pPDF p.6 (printed 328) Highlight: "Paraphrase: definition of whole networks, covering the whole environment." — 
- pPDF p.6 (printed 328) Highlight: "Paraphrase: egocentric methods suit large or ill-bounded populations." — 
- pPDF p.7 (printed 329) Highlight: "Paraphrase: East York residents have several close intimates." — 
- pPDF p.7 (printed 329) Highlight: "Paraphrase: East York residents have several routinely contacted ties." — 
- pPDF p.7 (printed 329) Highlight: "Paraphrase: whole networks map ties among all members of an environment." — 
- pPDF p.7 (printed 329) Highlight: "Paraphrase: the n(n-1) tie count, with 380 links for n=20." — 
- pPDF p.7 (printed 329) Highlight: "Paraphrase: egocentric models of seeking behaviour can guide new clients." — 
- pPDF p.8 (printed 330) Highlight: "Heading: Network Principles." — 
- pPDF p.8 (printed 330) Highlight: "Paraphrase: network principles are used to explore relational properties of networks." — 
- pPDF p.8 (printed 330) Highlight: "Paraphrase: what subgroups of interconnected actors exist." — 
- pPDF p.8 (printed 330) Highlight: "Paraphrase: positional properties, such as who occupies which network position." — 
- pPDF p.8 (printed 330) Highlight: "Paraphrase: cultural roles type people, but their behaviour may differ from the role." — 
- pPDF p.8 (printed 330) Highlight: "Paraphrase: locating informal routes, and the roles that respond to needs, matters for information study." — 
- pPDF p.8 (printed 330) Highlight: "Paraphrase: new roles and networks can be independent of traditional roles." — 
- pPDF p.8 (printed 330) Highlight: "Paraphrase: empirical evaluation of interactions may reveal emerging roles and networks." — 
- pPDF p.9 (printed 331) Highlight: "Paraphrase: a technological gatekeeper is recognized by others in the organization (Allen 1977)." — 
- pPDF p.9 (printed 331) Highlight: "Paraphrase: a competence-based hierarchy formed in place of an organizational-position hierarchy (Burkhardt and Brass)." — 
- pPDF p.10 (printed 332) Highlight: "Paraphrase: cohesion describes attributes of the whole network." — 
- pPDF p.10 (printed 332) Highlight: "Paraphrase: density and centralization are the whole-network cohesion measures." — 
- pPDF p.10 (printed 332) Highlight: "Paraphrase: clusters and cliques can be revealed by finding dense regions." — 
- pPDF p.10 (printed 332) Highlight: "Paraphrase: cliques and circles show how cliques influence larger social circles." — 
- pPDF p.10 (printed 332) Highlight: "Paraphrase: density is the degree to which members are connected to all others." — 
- pPDF p.10 (printed 332) Highlight: "Paraphrase: actors in high-density networks are more in touch than in low-density networks." — 
- pPDF p.11 (printed 333) Highlight: "Paraphrase: centralization measures how a set of actors is organized around a central point (Figure 3 example)." — 
- pPDF p.12 (printed 334) Highlight: "Paraphrase: in a centralized network, information from one actor to another can pass through one intermediary." — 
- pPDF p.12 (printed 334) Highlight: "Paraphrase: information specialists can rearrange routes to reduce transfer problems." — 
- pPDF p.12 (printed 334) Highlight: "Paraphrase: block modelling uses pairwise correlations and clustering to find structurally equivalent cases." — 
- pPDF p.12 (printed 334) Highlight: "Paraphrase: the most central actor with many lines is the network star." — 
- pPDF p.12 (printed 334) Highlight: "Paraphrase: an isolate has no connections within a network." — 
- pPDF p.13 (printed 335) Highlight: "Paraphrase: the actor with the lowest total shortest-path distance is the most globally central." — 
- pPDF p.13 (printed 335) Highlight: "Paraphrase: global centrality (closeness) is calculated from shortest paths in the network." — 
- pPDF p.13 (printed 335) Highlight: "Paraphrase: the first step to becoming central is to map the existing network structure." — 
- pPDF p.13 (printed 335) Highlight: "Paraphrase: more ties give more access to social resources." — 
- pPDF p.14 (printed 336) Highlight: "Paraphrase: betweenness example: after arms are removed, Actor A still mediates between B and C." — 
- pPDF p.14 (printed 336) Highlight: "Paraphrase: an information provider can occupy a structural-hole position to control flow." — 
- pPDF p.14 (printed 336) Highlight: "Paraphrase: an established information service can provide unique brokerage." — 
- pPDF p.14 (printed 336) Highlight: "Paraphrase: network data can support lobbying for resources to provide services (phrase relevant to lobbying)." — 
- pPDF p.14 (printed 336) Highlight: "Paraphrase: close ties promote exchange but also constrain actors." — 
- pPDF p.15 (printed 337) Highlight: "Paraphrase: cliques form from similar people who establish norms that exclude others." — 
- pPDF p.15 (printed 337) Highlight: "Paraphrase: a clique member with outside ties can bring new information into the clique." — 
- pPDF p.15 (printed 337) Highlight: "Paraphrase: outside ties raise opportunities for all clique members." — 
- pPDF p.15 (printed 337) Highlight: "Paraphrase: Coleman et al. found informing sources and legitimating sources of information." — 
- pPDF p.15 (printed 337) Highlight: "Paraphrase: other doctors were the legitimating and most influential sources in adoption." — 
- pPDF p.15 (printed 337) Highlight: "Paraphrase: people can make the most of the information opportunities in their networks." — 
- pPDF p.15 (printed 337) Highlight: "Paraphrase: access requires contacts who can use the information, not just receive it." — 
- pPDF p.15 (printed 337) Highlight: "Paraphrase: the network legitimates the member who receives and forwards information." — 
- pPDF p.16 (printed 338) Highlight: "Paraphrase: a path spanning a structural hole in an information network is especially useful." — 
- pPDF p.16 (printed 338) Highlight: "Paraphrase: the provider can use the network model to structure services for these actors." — 
- pPDF p.17 (printed 339) Highlight: "Paraphrase: the type and strength of ties indicate how likely an individual is to meet a willing source." — 
- pPDF p.17 (printed 339) Highlight: "Heading: Measures of the strength of relations." — 
- pPDF p.17 (printed 339) Highlight: "Paraphrase: tie strength shows how far actors can legitimate the information they deliver." — 
- pPDF p.17 (printed 339) Highlight: "Paraphrase: network relationships also legitimate the sender as an information source." — 
- pPDF p.17 (printed 339) Highlight: "Paraphrase: information becomes useful through forwarding (see short_quotes)." — 
- pPDF p.17 (printed 339) Highlight: "Paraphrase: information routes show where information travels in an environment." — 
- pPDF p.17 (printed 339) Highlight: "Paraphrase: a well-structured actor can regulate flow between sets of actors, with good sources and outlets." — 
- pPDF p.17 (printed 339) Highlight: "Paraphrase: network structures show how information flows around the whole environment." — 
- pPDF p.17 (printed 339) Highlight: "OCR-garbled fragment (groups and roles that exist without titles)." — 
- pPDF p.18 (printed 340) Highlight: "Paraphrase: research based on titles or existing routes shows what happens to those holders or routes, not where new things happen." — 
- pPDF p.18 (printed 340) Highlight: "Paraphrase: SNA helps information professionals advise clients on positioning in available networks." — 

## Study

**Question.** How can social network analysis (SNA) be used to study information exchange, and what does it offer information professionals (for example, librarians and information providers) for assessing and changing information routes and services?

**Theory.** Relational or network view of social structure: structure is derived empirically from observed relationships rather than from a priori classes (Wellman; Wasserman & Faust). Strength-of-weak-ties argument (Granovetter 1973). Structural holes and brokerage (Burt 1992a). Legitimation of information through close, reciprocated ties (Coleman, Katz & Menzel 1966). Graph theory is the formal base.

**Data.** None. The article has no sample, unit of analysis, N or observation period. Numerical content is illustrative or cited from other studies: a hypothetical health-information example; the arithmetic of n(n-1) possible ties (n=20 gives 380 links per relationship); Granovetter's job-search sample (over 80% vs under 17%); Wellman et al.'s East York egocentric findings; Haythornthwaite, Wellman & Mantei (1995) with 24 relationships reduced to six dimensions; Wegener (1991) with 11 measures reduced to three attributes.

**Method.** Narrative review and conceptual synthesis. It sets out SNA principles (relationships, ties, content, direction, strength; egocentric vs whole networks), five network principles (cohesion, structural equivalence, prominence, range, brokerage), and measurement techniques (graph and matrix representation, density, centralization, cliques, degree, closeness, betweenness, block modelling). It then maps these onto five information concepts: needs, exposure, legitimation, routes and opportunities. No estimation is performed.

**Identification.** Metadata correction: the hint's "1996" conflicts with the printed header in the text, which reads "LISR 18, 323-342 (1998)". The text says the article was first presented at the 1996 ALISE conference, San Antonio. The page range and volume match. The hint says the PII reads 1996, but no PII appears in the text, so that is unverified. The DOI is not in the text and is left blank. Author's affiliation: Graduate School of Library and Information Science, University of Illinois at Urbana-Champaign.

**Scope.** Population: information exchange among individuals, groups, organizations and concepts. Examples include library interlibrary-loan networks, hypertext subjects, co-workers, doctors, and East York residents. Institutional setting: libraries and information services, with a practitioner orientation. Era: mid-1990s, citing WWW, e-mail and online journals as new access routes (printed p. 330).

## Key findings

- Relationships are characterized by content, direction and strength, and ties aggregate multiple relationships between a pair. (Three attributes: content, direction, strength) — Principles: Relationships and Ties, printed pp. 326-328 (PDF pp. 4-6)
- Granovetter's job-search data: information came mostly from weak ties. Strength of weak ties is cited as a challenge to the strong-ties-only view, while later work is said to reaffirm strong ties. (Over 80% received instrumental job information from occasional or rare contacts; under 17% from frequent contacts) — Printed p. 328 (PDF p. 6)
- Egocentric networks suit large or ill-bounded populations. Whole-network designs need responses from all members, and that limits feasible size. (n actors give n(n-1) possible directed ties; n=20 gives 380 links per relationship) — Egocentric and whole networks, printed pp. 328-329 (PDF pp. 6-7)
- East York residents' community ties are maintained through social ties rather than geographic proximity. Typical residents are described by a few close intimate ties and routinely contacted ties. (At least 4 socially close intimates; at least 3 persons contacted three times a week or more; most have at least 1 close and routinely contacted tie) — Printed p. 329 (PDF p. 7), citing Wellman et al. (1988)
- Five network principles are proposed: cohesion, structural equivalence, prominence, range and brokerage. These are used to explore relational and positional properties of networks. () — Network Principles, printed pp. 330-331 (PDF pp. 8-9)
- Roles and groups can be identified empirically from behavior rather than titles. Informal roles emerge: technological gatekeepers are identified by others, and a technology guru's identity can change with the technology. () — Printed pp. 330-331 (PDF pp. 8-9), citing Allen (1977) and Burkhardt & Brass (1990)
- Burkhardt & Brass report that when members sought information from early adopters, a hierarchy formed based on competence rather than organizational position. () — Printed p. 331 (PDF p. 9)
- Cohesion and density: density is the ratio of actual to possible links, and higher density means more information flow. Cliques and clusters reveal subgroups. (Density = actual links / possible links) — Cohesion, printed pp. 332-333 (PDF pp. 10-11); Figures 1-2
- Centralization and structural equivalence: in the centralized Figure 3, information from B to C passes through one intermediary. Block modelling reorders cases by correlations to find structurally equivalent actors. (Figure 3 is a sociogram with one central actor) — Printed pp. 333-334 (PDF pp. 11-12); Figure 3
- Prominence: degree centrality counts lines per actor. The most central actor is the network star. Closeness sums shortest-path distances, and the lowest sum is the most globally central. (Actor A has degree 5 in Figure 3. Lowest sum of shortest-path distances = most central) — Prominence, printed pp. 334-335 (PDF pp. 12-13)
- Betweenness and brokerage: removing three arms of Figure 3 leaves B and C each with degree 4 and A with degree 2. A keeps an intermediary role between peripheral groups, so betweenness can exceed degree as a measure of brokerage. (B and C degree 4 each; A degree 2 after removal) — Brokerage, printed pp. 335-336 (PDF pp. 13-14)
- Structural holes give brokers control over flow between unconnected clusters. Established information services can show unique or redundant brokerage, and network data can support requests for resources. () — Printed p. 336 (PDF p. 14), citing Burt (1992a)
- Close ties promote exchange but also constrain actors through norms of similarity. Cliques form around similar people and limit nonredundant information unless a member has outside ties. () — Information effects of network structures, printed pp. 336-337 (PDF pp. 14-15)
- Coleman, Katz & Menzel (1966) found two kinds of information sources: those that inform (mass media, drug salesmen) and those that legitimate adoption (other doctors, via close reciprocated ties). (Study unit is physician adoption of a new antibiotic; no sample size is given in the text) — Printed p. 337 (PDF p. 15), citing Coleman et al. (1966)
- Information opportunities depend on (a) who one can contact, (b) what that contact can provide, and (c) who in the network one can forward it to. Well-structured networks yield access, timing and referrals. () — Printed pp. 337-338 (PDF pp. 15-16), citing Burt (1992a); Summary, printed p. 338-339
- The article proposes five information concepts for study (needs, exposure, legitimation, routes, opportunities). It argues that information professionals can locate unmet needs, fill structural holes, and advise clients on positioning. () — Social Network Analysis and the Study of Information, printed pp. 338-340 (PDF pp. 16-18)

## Limitations

- The article has no limitations section. Its own stated constraints are the following: whole-network designs need responses from all members, and the number of possible ties grows as n(n-1), so researchers are often limited in actors or relationships (printed pp. 328-329). Incomplete-data methods exist but are cited, not applied (Stork & Richards 1992).
- Strength measures are context-dependent and vary by what is studied, where and whom (printed pp. 327-328). Strength can be carried as a link value or considered within a presence-absence dichotomy by requiring a minimum frequency (printed p. 327).
- Reported ties can be asymmetric: A may report many contacts with B while B reports few (printed pp. 326-327).
- No empirical test is reported for the article's claims. Examples come from other studies or are hypothetical (health-information example; n=20 arithmetic). Recommendations for librarians (e.g., reorganizing circulated documents, rearranging routes) are not evaluated (printed pp. 330-331, 334, 340).
- Research relying on titles or existing routes is said to miss new developments (printed pp. 339-340). This is a normative argument, not a tested finding.
- The text is a 1996 conference paper and reflects mid-1990s technologies (printed p. 330).
- Source PDF text layer is OCR-garbled in places (e.g., printed pp. 325, 332, 337), so some quoted text here is unreliable.

## Short quotes

- "Information is made useful to an actor by being forwarded to others." (Information Routes section, printed p. 339 (PDF p. 17))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-sna#Cosponsorship network construction\|Cosponsorship network construction]] | qualifies | Relationships carry three attributes: content, direction and strength. A pair is tied by all the relationships it maintains. Strength can be carried as the value of a link, or considered within a presence-absence dichotomy by requiring a minimum frequency. Reported directed ties can be asymmetric, with the two sides reporting different amounts. The article does not say that thresholding discards strength. (Relationships and Ties, printed pp. 326-328 (PDF pp. 4-6); asymmetric reporting at printed p. 327) | Stage 3 builds sponsor-to-cosponsor edges summed per (From,To). Weight = role (1.0 original, 0.5 regular) times f_bill, where f_bill = success multiplier (3.0 if the bill passed its origin chamber, else 1) x bill decay weight x size norm (1/sqrt C under the default dial). moneyball_core NETWORKS cu config sets symmetrize True, so the cu cascade loader mean-symmetrizes each pair; holistic_norm keeps direction. The AUG workbook re-keys edges as (min,max); load_network_directed reads the BASE vintage. `Draft Systems/congress_member_scraper.R:1766-1782, 1927-1935, 1945-1951; fec/moneyball_core.py:383-410, 485-492, 497-507` | The article gives design principles and no estimate. It does not address 2:1 role weights, the success multiplier, the decay factor or the sqrt size norm, and it does not show that symmetrizing changes diffusion. It is not evidence for keeping or dropping regular-cosponsor edges, so the rejected drop must not be defended from this paper. Asymmetric reports are a survey feature, not a graph-construction rule. | weak | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Cascade activation kernel build_P and partisan edge factors\|Cascade activation kernel build_P and partisan edge factors]] | supports | Citing Coleman, Katz and Menzel (1966): some sources inform (mass media, drug salesmen) while others legitimate adoption; the legitimating sources, other doctors, were most influential, and close reciprocated ties carried legitimacy. Citing Granovetter (1973): over 80 percent of job information came from occasional or rare contacts, under 17 percent from frequent contacts. (Printed p. 328 (Granovetter) and p. 337 (Coleman et al.) (PDF pp. 6, 15)) | cpf_diffusion is 1.0 within party and 0.4+0.6*sqrt(bpf_u*bpf_v) across party. cpf_pressure is 1.7 co-partisan and 0.5 across party. Both feed p = x/(x+6) on the adjacency passed in. `fec/moneyball_core.py:543-548, 555-562` | Conceptual parallel only. The cited evidence concerns physicians and job seekers, not legislators, and the article gives no Coleman sample size or data-collection dates. It supports separating an information channel from a legitimation channel. It does not support the 1.7 and 0.5 ratios, the 0.4 floor or the constant 6. Coleman's reciprocated ties must not be imported as a reciprocity weight, since reciprocity is not validated (D-104 to D-108). | weak | VERIFIED |
| [[systems/moneyball-rank-pillars#Cosponsor-network InfluenceScore (computed, weight 0)\|Cosponsor-network InfluenceScore (computed, weight 0)]] | qualifies | Prominence is measured by degree (lines per actor) and by closeness (lowest sum of shortest-path distances). Brokerage is measured by betweenness. In Figure 3, after three arms are removed, actors B and C each keep degree 4 and actor A drops to degree 2, yet A still sits between the peripheral groups and keeps an intermediary role. (Prominence, printed pp. 334-335; Brokerage, printed pp. 335-336 (PDF pp. 12-14), Figure 3) | InfluenceScore = 0.60 weighted out-degree + 0.25 betweenness + 0.15 eigenvector, renormalized over components present, each min-max scaled. Computed with igraph on the directed weighted graph with path distance 1/W. `Draft Systems/congress_member_scraper.R:1796-1804, 1964-2001` | A taxonomy of measures, with no evidence on which predicts legislative influence. It does not bear on the stance-sign retirement. Whether centrality predicts passage belongs to Sotoudeh 2024 and the system data. | weak | VERIFIED |
| [[systems/melange-staffer-network#Layer-balanced PageRank, eigenvector and bridging diversity\|Layer-balanced PageRank, eigenvector and bridging diversity]] | suggests-extension | Brokerage is a positional property separate from prominence. Actors on a path between networks, or spanning a structural hole between unconnected clusters, control flow between them, and betweenness measures this. A path that spans a structural hole is particularly useful. (Brokerage, printed pp. 335-336; position and paths, printed p. 338 (PDF pp. 13-14, 16)) | Staffer graph publishes PageRank at damping 0.85 (sensitivity at 0.70 and 0.95) and a component-local eigenvector. Bridging score is normalized Simpson diversity of row-normalized tie mass across target kinds. No betweenness or Burt-constraint score found in fec/staffer_network. `fec/staffer_network/network_math.py:1-9, 264-296, 310-339; fec/staffer_network/graph_build.py:258-296` | Any span score added must describe graph structure only, not access or introduction probability (D-025). Staffer edges are co-tenure and revolving-door ties, so the paper's control-of-flow wording must not carry over. Payroll container cliques are built in by construction. The r = 0.87 degree versus office size figure was not checked. | weak | VERIFIED |
| [[systems/melange-staffer-network#Co-tenure weight and workplace exposure index\|Co-tenure weight and workplace exposure index]] | supports | Relationship strength is its intensity, for example how many goods are exchanged or how often actors meet, and it can be kept as a value rather than dichotomized. At the tie level, measures combine frequency of contact, duration, intimacy, reciprocal services and kinship, and they depend on context. (Strength and Tie Strength, printed pp. 327-328 (PDF p. 5)) | Co-tenure weight = kappa x tier x (1 - exp(-days/tau)) x attenuation, with tau = 365.2425. Shared-role factor 0.05 attenuates shared titles. `fec/staffer_network/ranking_math.py:11, 51-59` | Principle only: strength should be valued and duration is one input. The article gives no functional form, time constant or proximity multiplier, so tau and the kappa ladder remain owner design. The kappa ladder values were not located in ranking_math.py and are unverified here. The weight is a heuristic exposure score, not a contact probability (D-025). | weak | VERIFIED-SOFTENED |
| [[systems/melange-staffer-network#Concurrent-service OVERLAP edge rule\|Concurrent-service OVERLAP edge rule]] | qualifies | Structure is derived from observed relationships, not from a priori classes. The article lists membership in organizations among the a priori classifications it avoids as a basis for groups. Cliques form among similar people and limit nonredundant information. Density is actual over possible links, with n(n-1) possible directed ties (380 for n = 20). (Printed pp. 325-326, 332-333, 336-337 (PDF pp. 3-4, 10-11, 14-15)) | Person-person edge requires concurrent service in the same container, weighted by guaranteed overlap times kappa. Admin containers are excluded. Rule of record is the brain note concepts/concurrent-service-overlap-edge-rule.md, which exists. The 1.00 to 0.40 kappa ladder and the 70.8 percent figure were not located and are unverified. `fec/staffer_network/ranking_math.py:19-22; Documents/brain/concepts/concurrent-service-overlap-edge-rule.md` | The article would treat container co-presence as a membership-based classification, close to the a priori organizational class it warns against, not as an observed exchange. Density and clique statistics inside a container are produced by construction. The article gives no link between co-presence and actual exchange. No edge may be described in influence or access terms. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-sna#Social and press-engagement layers\|Social and press-engagement layers]] | qualifies | Pairs are tied by all the relationships they maintain. The strength of a tie may depend on the number and types of relationships a pair maintains, and on the strength of each relationship. (Printed pp. 326-327 (PDF pp. 4-5)) | On the CU base a media edge is admitted only for pairs with no cosponsor tie (untied). Holistic configs default to dominance: a tied pair takes max(cosponsor, media). HCoI edges are admitted only for cosponsor-isolated members, at weight up to 0.12. A second channel can raise a tied pair to the larger of the two weights, but the layers are never summed. `fec/network/build_multilayer_network.py:9-19, 59-81` | Multiplexity is conceptual and comes from survey-based ego networks. Here the layers are corpora of different quality. The untied and max rules are deliberate guards ruled by Paul, not recommendations to sum layers. | weak | VERIFIED-SOFTENED |
| [[systems/melange-sna#Press HCoI communities\|Press HCoI communities]] | qualifies | Block modelling finds structurally equivalent actors by correlating all pairs of cases and then clustering the correlations. Structural equivalence means identical ties to and from all other actors. Structure should come from observed relationships rather than attribute classes. The article does not name tie-matrix rows as the profile that is correlated. (Structural equivalence, printed p. 334 (PDF p. 12); relational view, printed p. 325) | press_hcoi.py correlates members' press-category attention profiles across 19 categories, keeps r >= 0.6, and runs Louvain with seed 7. The multilayer builder admits HCoI only for cosponsor-isolated members, at weight 0.6*r*HCOI_W capped at 0.12, as an explicit non-collaboration claim. The code docstring cites Malaterre and Lareau 2024 as the method source. `fec/network/press_hcoi.py:4-5, 22, 52-64; fec/network/build_multilayer_network.py:15-19` | The procedure resembles block modelling but correlates attributes rather than relationships, so under the article's definitions HCoI is attribute similarity. That supports keeping it as a weak, surgical layer. The article says nothing on whether attention similarity predicts collaboration. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: Year: the text header reads LISR 18, 323-342 (1998); the 1996 hint is the ALISE conference year. The ext correction to 1998 is right.
> - Summary issue: summary_md says cutting strength at a frequency threshold throws information away. The article does not say that; it says strength can still be considered within a presence-absence dichotomy.
> - Summary issue: summary_md says the article warns against threshold cutting. No such warning appears; the wording should be neutral.
> - Summary issue: summary_md says edges are scaled by 1/sqrt of the cosponsor count. f_bill also includes the 3.0 success multiplier and the bill decay weight (congress_member_scraper.R:1927).
> - Summary issue: summary_md and gaps_exposed say the cascade kernel runs on an undirected graph. That holds for the cu config (symmetrize True); holistic_norm stays directed (moneyball_core.py:398-400). Scope the claim.
> - Summary issue: gaps_exposed direction claim should name the cu config and the line range 485-507 rather than all configs.
> - Summary issue: flags say Paul's note about InfluenceScore retirement is wrong about direction; the retirement date 2026-06-25 and AB13 flag were not verified here.
> - Summary issue: related: fink-nd-twitter does not exist in the research items or the brain; dropped.
> - Summary issue: related: sotoudeh and ardekani why-strings attributed eigenvector, Katz and PageRank to the article, which does not contain them; corrected.
> - Summary issue: related: malaterre relation contradicts was not supported by the article; changed to complements.
> - Summary issue: related: selfhout same-method was not supported by the article; changed to complements.
> - Summary issue: Not verified and left open: 70.8 percent admin share, kappa ladder 1.00 to 0.40, r = 0.87 degree versus office size, HCoI homophily null result, Paul's paul_entries takeaways beyond the text, and the build_staffer_social_network.py:1150-1194 code_ref.
> - Summary issue: Paper has no data, sample or identification strategy; all empirical numbers are cited from other studies and are cited in the article as such.

## Gaps and flags

- No live brokerage or structural-hole measure exists anywhere in the chain. Betweenness appears only inside InfluenceScore, which carries weight 0. The staffer 'bridging diversity' measures spread across node kinds, not spans between unconnected clusters (network_math.py:310-339).
- Edge direction is discarded before diffusion under the cu config. The AUG workbook re-keys edges as (min,max) and load_network mean-symmetrizes (symmetrize True), so cascades on that config treat one-way sponsor-to-cosponsor ties as two-way (moneyball_core.py:485-507); holistic_norm stays directed. Only recruitment reads the directed BASE graph.
- Multiplexity is only partly represented. Layers are never summed; under dominance a tied pair takes the larger of the channel weights, so a pair tied by several channels is no stronger than its strongest channel alone (build_multilayer_network.py:59-81).
- Tie content is pooled. Within a NET_CONFIG, cosponsor edges merge all bill topics, so the 'content' attribute the paper treats as definitional is lost except through the separate sector configs.
- No whole-network cohesion summary (density, centralization) is reported per configuration beyond the sector hub-concentration flag. Comparisons across configs therefore lack the paper's basic descriptors, each of which would need to state its window and denominator.
- The paper's informing-versus-legitimating distinction has no empirical test in the system. cpf_pressure / cpf_diffusion encode it by design, and no estimate shows that legitimation runs through close co-partisan ties among legislators.
- D-025: the paper's phrases 'more ties give more access to social resources' (p. 335) and 'global centrality' must not be carried onto staffer PageRank, route costs or shared-background scores. These are heuristic model outputs, never closeness, access or introduction probabilities.
- Influence language barred on revolving-door edges: the paper's 'control of information flow' and structural-hole brokerage wording (pp. 336, 339) must not be applied to staffer-overlap or employment-routing edges.
- Reciprocity is not validated (D-104..D-108). The Coleman et al. claim that legitimation runs through 'close, reciprocated ties' (p. 337) must not justify a reciprocity weight, in cooperation_v2 or anywhere else.
- Rejected SNA proposals: the paper argues for neither dropping regular cosponsorship edges nor per-edge volume normalization.
- Paul's takeaway credits the paper with a 'causal link from relationships to outcomes'. The article has no data, sample or identification strategy, and every empirical figure is cited from other studies. Treat it as theory only.
- Metadata conflict: the vault slug and Paul's citations say 1996 (vol. 18 no. 4), but the printed header in the text reads 'LISR 18, 323-342 (1998)'. The DOI 10.1016/s0740-8188(96)90003-1 in Paul's citation is not in the text.
- The overlap-edge-rule code_ref in the component list (build_staffer_social_network.py:1150-1194) covers interval-precision handling, not the edge rule itself. The bearing cites the brain rule-of-record note instead.

Mostly aligned, but Paul overstates the paper's empirical weight. His takeaway that 'directionality and network structure are paramount' matches the paper's content/direction/strength framing. The system honours it only in part: direction is kept in the BASE scrape and the recruitment loader, and averaged away in the AUG graph and the cu-config cascade kernel. His note that the paper is 'strong on theory, thin on graph construction and data pipelines' is accurate. His claim of a 'causal link from relationships to outcomes' is not: the article is an uncontrolled expository review with no data of its own. His concern that the general availability of information affects validity has a partial anchor in the paper's remarks on asymmetric reporting and missing whole-network responses (pp. 326-329). In the system its live analogue is that LD-2 filings never name members, which keeps LDA an identity source and not an edge source (D-021). The paper does not address that point directly. The direction gap is the symmetrized cu-config cascade graph.

## Related papers

- [[sotoudeh-2024-network-based-measure]] — complements — The article names degree and closeness (prominence) and betweenness (brokerage) without testing them; it does not name eigenvector. Sotoudeh tests cosponsorship centrality measures against bill passage.
- [[ardekani-2026-computational-graph-theoretic]] — complements — The article surveys degree, closeness and betweenness. The Katz and PageRank choices attributed to Ardekani are not in this article, so the link is only the shared centrality family.
- [[malaterre-2024-inferring-social-networks]] — complements — press_hcoi.py takes its method from Malaterre and Lareau (2024), which correlates text profiles. The article's relational view bears on attribute-based grouping, but it does not discuss text profiles, so no direct contradiction is shown.
- [[porter-2005-network-analysis-committees]] — complements — The article's cohesion and clustering section (density, cliques, clusters, printed pp. 332-333) and its co-membership examples (printed p. 326) are the framework that committee co-membership analysis applies. Porter's content was not checked here.
- [[boxsteffensmeier-2018-taking-congress-interest]] — complements — The article cites Coleman, Katz and Menzel for the inform versus legitimate split (printed p. 337). Box-Steffensmeier offers congressional evidence on cue-taking. Its content was not checked here.
- [[bakker-nd-understanding-partisan-receptivity]] — complements — The article says close reciprocated ties carry legitimacy (Coleman et al., printed p. 337), the in-group legitimation mechanism that partisan cue work would test. Bakker's content was not checked here.
- [[aref-2021-identifying-hidden-coalitions]] — extends — The article's content, direction and strength triad (printed pp. 326-328) has no sign dimension, which signed-network methods add.
- [[selfhout-2010-emerging-late-adolescent]] — complements — The article's tie-strength and similarity-selection discussion (printed pp. 327-328, 336) is conceptual background for friendship tie-formation work. Selfhout's content was not checked here.

### Linked from
- [[ahmad-nd-shortest-path-algorithms]] — complements — Both treat tie strength (Granovetter) and direction as core tie properties. Paul cites Haythornthwaite for directionality, and in the staffer-closeness-routes and SNA components.