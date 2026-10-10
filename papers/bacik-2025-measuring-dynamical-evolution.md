---
type: paper
title: "Measuring the dynamical evolution of the United States lobbying network"
authors: ["Karol A. Bacik", "Jan Ondras", "Aaron Rudkin", "Jorn Dunkel", "In Song Kim"]
year: "2025 (arXiv:2503.11745; PDF is v2 dated February 6, 2026)"
venue: "arXiv preprint, arXiv:2503.11745 [physics.soc-ph]. No journal publication is stated in the text."
doi: "arXiv:2503.11745"
category: "Lobbying"
systems: ["melange-lobbying", "melange-pathfinding", "mentat-giving-forecast", "melange-sna", "melange-staffer-network"]
components: ["melange-lobbying.lobbied-member-link", "melange-lobbying.finance-client-classification", "melange-lobbying.contact-inference", "melange-pathfinding.staffer-overlap-routes", "mentat-giving-forecast.calculator-and-nowcast", "mentat-giving-forecast.cycle-model", "melange-sna.bipartisanship-bpf", "melange-staffer-network.identity-resolution"]
paul_annotated: true
paul_markup_count: 217
evidence_status: "verified"
source: "[[_sources/bacik-2025-measuring-dynamical-evolution.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\2503.11745v2.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\2503.11745v2.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\2503.11745v2.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\2503.11745v2.pdf", "C:\\Users\\pgoff\\Downloads\\2503.11745v2.pdf", "C:\\Users\\pgoff\\Downloads\\2503.11745v2 (1).pdf"]
sha12: ["a0c3322fb2cd", "1e0566ca5277"]
tags: [paper, lobbying]
---

# Measuring the dynamical evolution of the United States lobbying network

**Karol A. Bacik, Jan Ondras, Aaron Rudkin, Jorn Dunkel, In Song Kim (2025 (arXiv:2503.11745; PDF is v2 dated February 6, 2026)).** *arXiv preprint, arXiv:2503.11745 [physics.soc-ph]. No journal publication is stated in the text..* [arXiv:2503.11745](https://arxiv.org/abs/2503.11745) Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Bacik, Ondras, Rudkin, Dunkel and Kim build a yearly five-layer directed network from LobbyView, which is derived from LDA LD-2 filings. The layers are clients, registrants (K-Street firms and in-house departments), lobbyists, government entities and legislators. The analyzed sample is 1,277,411 filings from 1999 to 2023. The work is descriptive network physics, not causal inference. Its main findings: 200-500 K-Street registrants enter and leave each year, and only 20% of the firms active in 2000 were still active in 2020. Up to one-third of client-registrant ties are rewired each year. Attachment is linearly preferential, which gives a heavy-tailed distribution of clients per firm (exponent about 1.5, cutoff about 85-86). Money is concentrated: the cumulative Gini is about 0.8 for clients and firms. Agency-target portfolios form blocks by presidential administration. Senate cross-party cosponsorship fell about 40% while lobbyist bipartisanship did not fall. A single SOPA case (Q4 2011, 67 public firms) shows the IT sector over-represented, with P(IT | SOPA) above 0.4 against a base rate of about 0.1.
> 
> For Paul's systems the paper matters mostly through what LDA data cannot show. Legislators enter the paper's network only through lobbyists' one-time disclosures of past covered positions. Those disclosures have no dates, and the LDA does not require registrants to disclose direct contacts with legislators. That independently supports the system's refutation of the lobbied-member link and its fail-closed handling of contacts and identities. The paper's SOPA case maps only about 40% of the clients lobbying on SOPA in Q4 2011 to NAICS, which illustrates, for one bill-quarter sample, why client names decide classification. Its K-Street versus in-house distinction supports Mentat's rule of never adding hired-firm and in-house lobbying. Its "election synchronization" finding is about which agencies are targeted, not about money, so it does not support a phase effect on lobbying dollars. The paper contains no press or sentiment data.

## Paul's annotation

From [[annotations/Lobbying - Lobbying - Annotated Bibliography|Lobbying - Lobbying - Annotated Bibliography]]:

> Bacik, Karol A, et al. *Measuring the Dynamical Evolution of the United States Lobbying Network*, 6 Feb. 2026.
>
> This article by Massachusetts Institute of Technology professors, spearheaded by Professors Karol A. Bacik and In Song Kim, explores the evolution of the congressional lobbying ecosystem, while also presenting a novel database called LobbyView that is highly relevant to the present research and is a fundamental source of data points in models being outlined within the present research. It should be noted that the researchers use case studies to derive empirical observations, such as preferential attachment as a key trait in lobbyists' careers, the role of the upcoming election cycle in terms of who and what is being targeted, and the value of adaptation in response to emerging political events. However, the magnum opus of this article is the data set itself, which can be used to create probabilistic models. These models should be reviewed and dissected to gain a fuller understanding of how they function and can ultimately be augmented. One such route of augmentation is the incorporation of news cycles, social media feeds, and press statements, analyzed via sentiment analysis, as determinants of the likelihood of change within the current pathway and dynamic.

## Paul's markup in the PDF

- p1 Highlight: "topic: data scarcity for political systems; motivates LDA-based time-resolved data (idx 0)"
- p1 Highlight: "topic: LDA public reporting mandate (idx 1)"
- p1 Highlight: "topic: LDA filings are unstructured and hard to extract (idx 2)"
- p1 Highlight: "topic: LobbyView as integrated, disambiguated database (idx 3)"
- p1 Highlight: "topic: LobbyView scale, 1.6M reports, linkage tools (idx 4)"
- p1 Highlight: "topic: reconstructing network connectivity and evolution (abstract, idx 5)"
- p1 Highlight: "topic: accumulation of contacts in few firms (abstract, idx 6)"
- p1 Highlight: "topic: synchronization with electoral cycles (abstract, idx 7)"
- p1 Highlight: "topic: probabilistic framework for bills, issues, firms (idx 8)"
- p1 Highlight: "topic: LobbyView as resource for interdisciplinary research (idx 9)"
- p1 Highlight: "topic: what determines scale and strategy of lobbying (intro question, idx 10)"
- p1 Highlight: "topic: network reorganization after crises (intro question, idx 11)"
- p1 Highlight: "topic: comparison with other complex social and biological networks (idx 12)"
- p2 Highlight: "topic: summary findings: preferential attachment, cycles, shock responses (idx 13)"
- p2 Highlight: "topic: LDA and HLOGA legal background (idx 14)"
- p2 Highlight: "topic: LD-2 quarterly reports filed on client behalf (idx 15)"
- p2 Highlight: "topic: filings break down spending across 79 issues (idx 16)"
- p2 Highlight: "topic: unstructured disclosure text; bills and agencies (idx 17)"
- p2 Highlight: "topic: name and firm disambiguation, unique identifiers (idx 18)"
- p2 Highlight: "topic: identifiers for external databases and donations (idx 19)"
- p2 Highlight: "topic: registrant types: in-house vs external (idx 20)"
- p2 Highlight: "topic: in-house registrant label (figure legend, idx 21)"
- p2 Highlight: "topic: K-Street firm definition (idx 22)"
- p2 Highlight: "topic: edge construction client-registrant-lobbyist (idx 23)"
- p2 Highlight: "topic: professional network via past employment (idx 24)"
- p2 Highlight: "topic: section heading: reconstructing the network (idx 25)"
- p2 Highlight: "topic: layer legend, clients and registrants (idx 26)"
- p2 Highlight: "topic: layer legend fragment (idx 27)"
- p2 Highlight: "topic: layer legend fragment, registrant-lobbyist ties (idx 28)"
- p2 Highlight: "topic: layer legend fragment, agencies and legislators (idx 29)"
- p2 Highlight: "topic: layer legend fragment (idx 30)"
- p2 Highlight: "topic: five primary categories of lobbying actors (idx 31)"
- p4 Highlight: "topic: adaptation to political events; example attachment and detachment component (Fig 2 caption, idx 32)"
- p4 Highlight: "topic: registrant survival and quadratic attachment form (Fig 2 caption, idx 33)"
- p4 Highlight: "topic: 2000 vs 2020 registrant survival, 20% (idx 34)"
- p4 Highlight: "topic: client rewiring up to one-third of ties per year (idx 35)"
- p5 Highlight: "topic: evidence for linear preferential attachment (idx 36)"
- p5 Highlight: "topic: heavy-tailed registrant degree; elite registrants (idx 37)"
- p5 Highlight: "topic: preferential attachment as explanation of hierarchy (idx 38)"
- p5 Highlight: "topic: attachment and detachment as countervailing forces (idx 39)"
- p5 Highlight: "topic: global degree statistics stable over 25 years (idx 40)"
- p5 Highlight: "topic: crises and HLOGA as plausible causes of firm decline (idx 41)"
- p5 Highlight: "topic: COVID-era spikes in disaster and small-business issues (idx 42)"
- p5 Highlight: "topic: issue portfolio evolution over years (idx 43)"
- p5 Highlight: "topic: political business cycle, presidential election synchronization (idx 44)"
- p5 Highlight: "topic: portfolios as feature vectors; correlation method (idx 45)"
- p5 Highlight: "topic: target correlation block structure by administration (idx 46)"
- p5 Highlight: "topic: quick adaptation on leadership change (idx 47)"
- p6 Highlight: "topic: year-to-year correlation of government approach portfolios (Fig 3 caption, idx 48)"
- p7 Highlight: "topic: SOPA case: 67 public firms, pathway probabilities (Fig 4 caption, idx 49)"
- p7 Highlight: "topic: example pathway: EOP approach and bipartisan portfolio (idx 50)"
- p8 Highlight: "topic: NAICS base rate vs IT conditional on SOPA (idx 51)"
- p8 Highlight: "topic: conditional probability shift for IT firms (idx 52)"
- p8 Highlight: "topic: lobbyist-legislator connections question for SOPA (idx 53)"
- p8 Highlight: "topic: IT firms run a broader lobbying campaign (idx 54)"
- p8 Highlight: "topic: diverse lobbying portfolio and legislative success hypothesis (idx 55)"
- p8 Highlight: "topic: LobbyView potential as research resource (idx 56)"
- p8 Highlight: "topic: preferential attachment as significant mechanism (Discussion, idx 57)"
- p8 Highlight: "topic: election cycle role for target selection (Discussion, idx 58)"
- p8 Highlight: "topic: adaptability to critical events; probabilistic template (idx 59)"
- p8 Highlight: "topic: future work on higher-order and multilayer interactions (idx 60)"
- p8 Highlight: "topic: multiscale community structure; LobbyView updates (idx 61)"
- p8 Highlight: "topic: future avenues, higher-order interactions (idx 62)"
- p12 Highlight: "topic: SI heading: lobbying network construction, nodes (idx 63)"
- p12 Highlight: "topic: SI heading: network visualization, node placement (idx 64)"
- p12 Highlight: "topic: SI heading: data sources and report structure (idx 65)"
- p13 Highlight: "topic: data and code availability (idx 66)"
- p13 Highlight: "topic: probabilistic analysis methodology (idx 67)"
- p14 Highlight: "topic: LobbyView structure; LD-1, LD-2, LD-203 forms (idx 68)"
- p14 Highlight: "topic: LD-1 new client and LD-203 contribution forms (idx 69)"
- p14 Highlight: "topic: LD-2 periodic report contents (idx 70)"
- p14 Highlight: "topic: LD-2 income, expenses, issue codes (idx 71)"
- p14 Highlight: "topic: 79 issue codes per report section (idx 72)"
- p14 Highlight: "topic: free-text block extraction (idx 73)"
- p14 Highlight: "topic: free text with no fixed format (idx 74)"
- p14 Highlight: "topic: pre-2008 paper filings need OCR (idx 75)"
- p14 Highlight: "topic: regular expressions for text extraction (idx 76)"
- p14 Highlight: "topic: standardized datasets and firm identifiers (idx 77)"
- p15 Highlight: "topic: data gathered from the Senate LDA API (idx 78)"
- p15 Highlight: "topic: LDA disclosure website as source (idx 79)"
- p15 Highlight: "topic: regex bill matching; firm matching to Compustat and Orbis (idx 80)"
- p15 Highlight: "topic: congressional data ingestion into PostgreSQL (idx 81)"
- p15 Highlight: "topic: data access options (idx 82)"
- p16 Highlight: "topic: lobbying datum tuple definition (idx 83)"
- p16 Highlight: "topic: annualizing periodic filings across the 2008 change (idx 84)"
- p17 Highlight: "topic: self-filer flag and filings on behalf of others (idx 85)"
- p17 Highlight: "topic: most reports are filed on behalf of others (idx 86)"
- p17 Highlight: "topic: missing dollar values and reporting thresholds (idx 87)"
- p17 Highlight: "topic: monetary field differs for self-filers vs third parties (idx 88)"
- p17 Highlight: "topic: higher threshold for self-filers (idx 89)"
- p17 Highlight: "topic: monetary heuristic formula for low-cost reports (idx 90)"
- p17 Highlight: "topic: low-cost lobbying is a small share; heuristic immaterial (idx 91)"
- p17 Highlight: "topic: scope limitation: informal advocacy not captured (idx 92)"
- p18 Highlight: "topic: search-assisted client disambiguation via websites (idx 93)"
- p19 Highlight: "topic: in-house vs K-Street registrant classification (idx 94)"
- p19 Highlight: "topic: formal definitions of registrant types (idx 95)"
- p19 Highlight: "topic: client-registrant note fragment (idx 96)"
- p19 Highlight: "topic: registrant-report association fragment (idx 97)"
- p19 Highlight: "topic: lobbyist association fragment (idx 98)"
- p20 Highlight: "topic: lobbyist name disambiguation (idx 99)"
- p20 Highlight: "topic: government entities definition (idx 100)"
- p20 Highlight: "topic: multiplicity of government entities in a report (idx 101)"
- p20 Highlight: "topic: 79-item standardized issue-area list (idx 102)"
- p20 Highlight: "topic: linking lobbying to bills (idx 103)"
- p20 Highlight: "topic: mandatory disclosure of past professional relations (idx 104)"
- p20 Highlight: "topic: covered-official disclosure rules, revolving door (idx 105)"
- p20 Highlight: "topic: one-time disclosure of covered positions (idx 106)"
- p20 Highlight: "topic: missing timelines for covered positions (idx 107)"
- p21 Highlight: "topic: government association function g(l,y) (idx 108)"
- p21 Highlight: "topic: political association function p(l,y) (idx 109)"
- p21 Highlight: "topic: function arguments and outputs (idx 110)"
- p21 Highlight: "topic: outputs by year (idx 111)"
- p21 Highlight: "topic: monotonicity of g in year (idx 112)"
- p21 Highlight: "topic: notation for g (idx 113)"
- p21 Highlight: "topic: legislator associations are not monotone in time (idx 114)"
- p21 Highlight: "topic: time-indexed monotonicity fragment (idx 115)"
- p21 Highlight: "topic: subset fragment (idx 116)"
- p21 Highlight: "topic: legislator partisan affiliation data (idx 117)"
- p21 Highlight: "topic: Party and Chamber functions (idx 118)"
- p21 Highlight: "topic: affiliation data and per-year graphs (idx 119)"
- p21 Highlight: "topic: graph description fragment (idx 120)"
- p21 Highlight: "topic: edge set fragment (idx 121)"
- p21 Highlight: "topic: five node sets in layers (idx 122)"
- p21 Highlight: "topic: union of filings per year fragment (idx 123)"
- p21 Highlight: "topic: union fragment (idx 124)"
- p21 Highlight: "topic: registrant type subdivision (idx 125)"
- p21 Highlight: "topic: notation fragment (idx 126)"
- p21 Highlight: "topic: set-membership fragment (idx 127)"
- p21 Highlight: "topic: union fragment (idx 128)"
- p22 Highlight: "topic: legislators with historical employment link (idx 129)"
- p22 Highlight: "topic: client-registrant edge definition (idx 130)"
- p22 Highlight: "topic: client-registrant edge existence rule (idx 131)"
- p22 Highlight: "topic: registrant-lobbyist contract edge definition (idx 132)"
- p22 Highlight: "topic: if-and-only-if fragment (idx 133)"
- p22 Highlight: "topic: multi-edges disregarded (idx 134)"
- p23 Highlight: "topic: unweighted network; monetary weights optional (idx 135)"
- p23 Highlight: "topic: weights fragment (idx 136)"
- p23 Highlight: "topic: unweighted default; monetary weights (idx 137)"
- p23 Highlight: "topic: weights notation (idx 138)"
- p23 Highlight: "topic: legislator inclusion via active lobbyist (idx 139)"
- p23 Highlight: "topic: out-degree fragment (idx 140)"
- p23 Highlight: "topic: K-Street subset fragment (idx 141)"
- p23 Highlight: "topic: In-House subset fragment (idx 142)"
- p24 Highlight: "topic: network distributions by year (idx 143)"
- p24 Highlight: "topic: order of the network; registrant counts (idx 144)"
- p24 Highlight: "topic: distinct government entities and legislators (idx 145)"
- p26 Highlight: "topic: average government entity degree (idx 146)"
- p27 Highlight: "topic: average in/out-degree figure caption (idx 147)"
- p28 Highlight: "topic: CCDF definition for in-degree (idx 148)"
- p28 Highlight: "topic: proportionality fragment (idx 149)"
- p28 Highlight: "topic: shape-parameter fragment (idx 150)"
- p28 Highlight: "topic: MLE fitting (idx 151)"
- p28 Highlight: "topic: parameter sweep for augmented power law (idx 152)"
- p28 Highlight: "topic: best fit and saturation interpretation (idx 153)"
- p28 Highlight: "topic: exponent comparison with Barabasi-Albert (idx 154)"
- p28 Highlight: "topic: bipartite caveat for Barabasi-Albert comparison (idx 155)"
- p28 Highlight: "topic: exponential lobbyist contract distributions (idx 156)"
- p29 Highlight: "topic: correlation of registrant and lobbyist counts (idx 157)"
- p31 Highlight: "topic: 80-20 concentration of expenses across clients (idx 158)"
- p32 Highlight: "topic: monetary Gini index definition (idx 159)"
- p32 Highlight: "topic: Gini about 0.8 for clients and K-Street firms (idx 160)"
- p34 Highlight: "topic: subsampling and Gini index bias (idx 161)"
- p35 Highlight: "topic: years 1999-2023 plotted (idx 162)"
- p37 Highlight: "topic: figure fragment (idx 163)"
- p38 Highlight: "topic: monetary flow normalization (idx 164)"
- p38 Highlight: "topic: Fruchterman-Reingold layout (idx 165)"
- p38 Highlight: "topic: node size and color encoding (idx 166)"
- p38 Highlight: "topic: K-Street node color encoding (idx 167)"
- p41 Highlight: "topic: rejuvenation comparison across years (idx 168)"
- p41 Highlight: "topic: decay curves by starting year (idx 169)"
- p41 Highlight: "topic: universal decay law of operation probability (idx 170)"
- p41 Highlight: "topic: exit probability depends on individual characteristics (idx 171)"
- p41 Highlight: "topic: age predicts exit; aging over about 10 years (idx 172)"
- p41 Highlight: "topic: aging curve timescale (idx 173)"
- p42 Highlight: "topic: noisy attachment function estimates (idx 174)"
- p42 Highlight: "topic: linear attachment and detachment (idx 175)"
- p43 Highlight: "topic: lobbying contract dynamics, K-Street and In-House (idx 176)"
- p44 Highlight: "topic: sublinear attachment for registrant-lobbyist contracts (idx 177)"
- p44 Highlight: "topic: In-House registrant-lobbyist dynamics (idx 178)"
- p44 Highlight: "topic: sample component selection algorithm (idx 179)"
- p46 Highlight: "topic: component tracking two years forward and back (idx 180)"
- p46 Highlight: "topic: bipartite subgraph analysis intro (idx 181)"
- p46 Highlight: "topic: clustering coefficient definition (idx 182)"
- p47 Highlight: "topic: focusing characteristic of registrant layer (idx 183)"
- p47 Highlight: "topic: largest connected component definition (idx 184)"
- p47 Highlight: "topic: giant component only in client-K-Street subgraph (idx 185)"
- p47 Highlight: "topic: fragment on bipartite components (idx 186)"
- p47 Highlight: "topic: lobbyist-legislator transition with component growth (idx 187)"
- p49 Highlight: "topic: portfolio matrix normalization (idx 188)"
- p50 Highlight: "topic: top-79 entity ranking and rare-entity sensitivity (idx 189)"
- p51 Highlight: "topic: PCA path of target portfolio over time (idx 190)"
- p55 Highlight: "topic: NAICS hierarchy and 20 broad sectors (idx 191)"
- p55 Highlight: "topic: client sector as random variable (idx 192)"
- p55 Highlight: "topic: sector variable fragment (idx 193)"
- p56 Highlight: "topic: registrant profile of drawn client (idx 194)"
- p56 Highlight: "topic: association variable categories (idx 195)"
- p56 Highlight: "topic: government entity probability in SOPA sections (idx 196)"
- p60 Highlight: "topic: independent legislators neglected in polarization (idx 197)"
- p60 Highlight: "topic: bill cosponsorship network definition (idx 198)"
- p60 Highlight: "topic: edges between sponsors and cosponsors (idx 199)"
- p60 Highlight: "topic: bipartisan cosponsorship counts (idx 200)"
- p60 Highlight: "topic: bipartisan index for House and Senate, declining (idx 201)"
- p61 Highlight: "topic: four categories of agents by party connection (idx 202)"
- p61 Highlight: "topic: agent party-profile classification (idx 203)"
- p61 Highlight: "topic: relative size of bipartisan set as polarization measure (idx 204)"
- p61 Highlight: "topic: lobbyists vs legislators polarization; Senate cosponsorship down about 40% (idx 205)"
- p61 Highlight: "topic: contrast of polarization trends (idx 206)"
- p61 Highlight: "topic: agents by profile figure fragment (idx 207)"
- p61 Highlight: "topic: lobbying industry not polarizing like Congress (idx 208)"
- p61 Highlight: "topic: bipartisan teams and strategic lobbying (idx 209)"
- p61 Highlight: "topic: strategic rather than ideological lobbying hypothesis (idx 210)"
- p62 Highlight: "topic: bipartisan share increasing among agents (idx 211)"
- p62 Highlight: "topic: bipartisan registrants earn higher income (idx 212)"
- p62 Highlight: "topic: modularity definition for cosponsorship (idx 213)"
- p62 Highlight: "topic: lobbyist modularity near zero (idx 214)"
- p62 Highlight: "topic: data and code on OSF (idx 215)"
- p63 Highlight: "topic: LobbyView website access, free registration (idx 216)"

## Study

**Question.** How does the US federal lobbying network (clients, registrants, lobbyists, government entities, legislators) evolve over time? Specifically: (a) what growth and attachment rules govern client-registrant ties; (b) how does the network adapt to crises and to election or administration cycles; (c) can LDA disclosure data support probabilistic analysis of lobbying pathways for individual bills, issues, or firms, demonstrated on the Stop Online Piracy Act (SOPA, 2011).

**Theory.** Network science and statistical physics rather than a formal political-economy model. Preferential attachment and cumulative advantage (Barabasi-Albert; Price's Matthew effect) explain hierarchy in K-Street registrant client counts. Attachment and detachment are treated as countervailing forces that stabilize the network. A five-layer directed multipartite graph is used. The political business cycle (Nordhaus 1975) is the framing for election-synchronized targeting. Partisan polarization is measured with cosponsorship and co-association bipartisan indices.

**Data.** LobbyView relational database built from LDA LD-2 quarterly/semiannual filings. Period 1999-2023. Analyzed sample: 1,277,411 filings (SI Fig S2). Full LobbyView holds more than 1.6 million reports covering about 87 billion USD (main p. 2; SI Fig S4 gives 87.5 billion). Unit of observation: the filing, aggregated into one directed multilayer network per year. Layers: clients, registrants (K-Street external firms and In-House departments), lobbyists, government entities (agencies and chambers), legislators (members of Congress). Example snapshot 2017 (Fig 1B): 10,694 clients, 4,405 registrants, 11,543 lobbyists, 128 government entities, 374 legislators. Industry sectors use NAICS 2-digit codes (20 sectors) via S&P Compustat; about 40% of the clients lobbying on SOPA in Q4 2011 could be mapped (SI XII.A). Issues: 79 LDA issue codes; bills extracted by regex. Legislator data: US Digital Service congressional database. External linkage: Compustat, BoardEx, Orbis, NOMINATE. SOPA case: 67 publicly traded firms lobbying in Q4 2011 (main Fig 4 caption).

**Method.** Descriptive and empirical statistics on a time-indexed multilayer directed graph. Steps: (1) in- and out-degree CCDFs by layer; MLE fits of an augmented power law with exponential cutoff, a log-normal and a Weibull, with the power law preferred (Fig 2F, SI Fig S11). (2) Preferential attachment: attachment and detachment functions by registrant in-degree, cumulated, with linearity tested (Fig 2E, SI VIII.B). (3) Survival or retention probability by year and age-conditioned exit (SI VIII.A). (4) Pareto charts and Gini indices of monetary flow (SI V). (5) Issue and target portfolios: client-share matrices, year-to-year Spearman correlation, PCA (SI XI). (6) Bipartite clustering coefficients and largest connected component, against a configuration-model null (SI IX). (7) SOPA pathway probabilities: uniform client draw, conditional probability shift Delta P(sector | SOPA) (Fig 4, SI XII). (8) Polarization: bipartisan cosponsorship index, lobbyist bipartisan index, modularity (SI XIII).

**Identification.** None causal. The analysis is descriptive. Event-timing associations (2007-08 financial crisis, HLOGA 2007, 9/11, COVID-19, presidential terms) are shown as overlays without a counterfactual or instrument. Null comparisons use a bipartite configuration model for clustering and component size (SI IX). The SOPA result is a single case study using conditional probability shifts against a baseline (main p. 8). The paper's own wording is 'supporting the hypothesis' and 'lobbying is strategic'.

**Scope.** US federal lobbying disclosed under the Lobbying Disclosure Act (LDA) of 1995 as amended by HLOGA 2007, years 1999-2023. Population is registrants, clients and lobbyists in LD-2 filings. Excludes informal or unreported advocacy (stated scope condition, SI I.C), PAC and donation channels (only mentioned), LD-203 contribution reports, 'no activity' filings, and superseded filings. Institutions covered: Congress (legislators and committees) and federal agencies and departments.

## Key findings

- Annual churn of K-Street registrants: 200-500 new registrants enter each year, with a comparable number exiting. (main text p. 4; Fig 2B; 200-500 entrants per year)
- Long-run turnover: of the registrants active in 2000, only a minority remain active in 2020. (main text p. 5 (cites Fig 2C; survival curve is labeled Fig 2D in caption); 20% of 2000 registrants still active in 2020)
- Client-registrant rewiring is large: a substantial share of clientship ties is replaced every year. (main text p. 5; up to one-third of clientship connections each year)
- Linear preferential attachment: the chance a new client selects a registrant is proportional to that registrant's current client count. Attachment and detachment are both linear in degree and act as countervailing forces. (main text pp. 4-5 (Fig 2E); SI VIII.B (Fig S27); cumulative attachment function ~ c^2; quadratic functional form)
- Heavy-tailed clients-per-K-Street-registrant distribution: a small elite dominates the lobbying market. (main text p. 5 (Fig 2F caption p. 4); SI IV.B (Fig S11); power-law exponent d = 1.46 (caption) / about 1.5 (text) / 1.54 (SI); cutoff c0 = 86 (caption) / about 85 (text) / 86 (SI))
- Income concentration among registrants. (main text pp. 5-6 (cites SI V); about 80% of K-Street income from 20% of registrants)
- Global layer-to-layer degree distributions were stable across 25 years despite high member churn. (main text p. 5; stable 1999-2023 (no numeric change reported))
- Short-term decline in active K-Street firms after 2009, persisting for much of the following decade. Causes are offered as plausible (2007-08 crisis; HLOGA 2007 revolving-door limits) but not tested. (main text p. 6; Fig 2B; decline after 2009 (no magnitude stated in text))
- Issue portfolios respond to crises: spikes in disaster-planning and small-business lobbying in early COVID-19; portfolio rearrangement after 9/11 and during COVID-19. (main text pp. 5-6; Fig 3A-B, Fig 3E; no numeric values given in text)
- Target (agency) portfolios are block-diagonal across presidential terms; lobbyists keep approaching the same agencies within an administration but adapt quickly on leadership change. Some agencies such as the National Economic Council follow the presidential election cycle. (main text pp. 5-6; Fig 3F; SI XI (Figs S34-S38); qualitative; PCA: e1 explains more than 48% (text) or 38% (caption) of variance; e2 explains 13%)
- SOPA 2011: IT-sector firms were far more represented among SOPA lobbyists than in the general client base, and IT firms hired multiple K-Street firms, approached the Executive Office of the President more often, and assembled bipartisan lobbying teams more often. (main text p. 8; Fig 4 (inset Fig 4B; panels C-E); SI XII; P(IT | SOPA) > 0.4 vs P(IT) about 0.1 (shift about +0.3, derived from the stated values); no numeric values stated for panels C-E)
- Monetary concentration of lobbying is persistently high; lobbyist income concentration is lower but above US income Gini. (SI V.B (Figs S18-S19); pp. 19-20; cumulative Gini about 0.8 for clients and K-Street registrants over 2 decades; about 0.6 for lobbyists; US income Gini about 0.4 (World Bank, as cited))
- Client-level 80-20 rule for lobbying expenditure. (SI V.A (Fig S15B); p. 19; 80% of expenses from 20% of clients)
- Registrant-lobbyist contracts are hierarchical at the firm level but egalitarian at the individual lobbyist level. (SI V.A (Fig S17); SI IV.B (Fig S12C-D); pp. 16, 19; 20% of K-Street registrants hold more than 70% of lobbyist contracts; about 90% of lobbyists have no documented government or legislator association)
- Exit probability of lobbying actors falls with age, with a shared timescale of about 10 years across clients, registrants and lobbyists. (SI VIII.A (Fig S26D-F); p. 28; characteristic timescale about 10 years)
- Network-structure checks: clustering coefficients of bipartite projections match the configuration-model null. Only the client-K-Street subgraph has a giant connected component at all times. (SI IX.A-B (Figs S30-S31); pp. 34-35; qualitative match; no numeric values in text)
- Registrant income is high when a registrant reaches government or legislator associations; each association type alone is sufficient for high income. (SI X (Fig S32); pp. 35-36; qualitative; income in USD by association count)
- Polarization contrast: Senate cosponsorship across parties fell sharply, while lobbying shows no matching trend. Bipartisan lobbying teams earn more income per lobbyist. (SI XIII (Fig S46); pp. 48-50; main text p. 8 (Fig 4D); Senate bipartisan cosponsorship down about 40% over 25 years; lobbyist bipartisan index not decreasing; lobbyist modularity near zero)
- Legislator-lobbyist ties via past employment are common but incomplete. (main text pp. 2-3; SI III (Fig S9); p. 10; 20% of lobbyists had a known government connection in 2023; main text says 80% of Senators and 60% of Representatives have at least one active connected lobbyist, while SI III says about 80% and 50% (inconsistent))

## Limitations

- Revolving-door and legislator ties are inferred only from past-employment disclosures. Lobbyists do not give timelines for covered positions, so recency cannot be established (SI I.D).
- Direct registrant-legislator contacts are not required by the LDA and are not observed.
- The client-registrant-lobbyist layered graph is a first-order approximation that obscures polyadic interactions; higher-order and multilayer structure is deferred to future work (main pp. 2-3; SI VI; Discussion p. 8).
- Entity resolution is imperfect. Client matching uses search-assisted disambiguation, which the paper says fails for long-defunct firms and non-public-facing firms, and resolves to third-party directories (SI I.C fn. 2). Lobbyist name resolution is score-based and uncertain.
- Low-cost filings are assigned a reporting-threshold value as an upper bound, a heuristic the paper says is immaterial to results (SI I.C).
- Filings for 1999-2007 were on paper and needed OCR, and the 2008 switch from semiannual to quarterly filing doubled report counts; the paper aggregates to annual data.
- Only about 40% of the clients lobbying on SOPA in Q4 2011 could be mapped to NAICS sectors via Compustat, and the probabilistic analysis is restricted to them (SI XII.A).
- The SOPA analysis is a single bill, a single quarter, and a restricted subsample; the paper says subsampling can bias results (SI V).
- The paper states that fitting a network-evolution model to the lobbying network is beyond its scope (SI VIII). Its preferential-attachment interpretation is descriptive and does not test competing mechanisms.
- Polarization analysis is described by the authors as exploratory (SI XIII).
- Reported numbers conflict across the text: power-law exponent (1.46 / 1.5 / 1.54); PCA variance (48% vs 38%); legislator connection shares (60% vs 50% for Representatives); survival-curve panel (Fig 2C vs 2D); filing count (1.6 million vs 1,277,411 analyzed).
- Anonymized relational data and code are on OSF (osf.io/y2hnm); full identifiers and some fields require contacting the authors (SI XIV).

## Short quotes

- "lobbying is a strategic, rather than ideological activity" — SI XIII (Polarization analysis), printed p. 49; Bacik et al., arXiv:2503.11745

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-lobbying#Lobbying member-year network and circular lobbied-member link\|Lobbying member-year network and circular lobbied-member link]] | supports | The LDA does not require registrants to disclose direct contacts with legislators, so the paper infers legislator connections from lobbyists' disclosed past employment; a legislator is included if at least one active lobbyist worked for them. About 90% of lobbyists have no documented government or legislator association, and 20% had a known government connection in 2023. (main pp. 2-3; SI I.D (pp. 20-21); SI III (Fig S9); SI IV.B (Fig S12C-D); SI p. 23) | The member-year network credits every sponsor and cosponsor of a lobbied bill (build_network_2021_rebuild.py:13), so the system treats the link as REFUTED as evidence of influence. The interaction ledger states that the LD-2 form has no position field and ships position_basis='not_disclosed'. `fec/lobbying/build_network_2021_rebuild.py:13; fec/lobbying/lobby_interactions.py:19-24` | The evidence carries over fully because it describes the same federal LD-2 form. The paper uses the same data and states plainly that legislator contact is unobserved, so nothing in LD-2 can rescue the bill-roster join. An independent contact measure is still required. The paper's own legislator edges come from revolving-door disclosure, so they are not contact evidence either, and influence language is barred on them. | strong | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#LDA finance-client classification and report join\|LDA finance-client classification and report join]] | supports | For the SOPA case study, only about 40% of the clients lobbying on SOPA in Q4 2011 could be mapped to NAICS sectors through Compustat, and the probabilistic analysis is restricted to them. Client disambiguation is search-assisted and the paper says it is weak for long-bankrupt firms and non-public-facing firms that resolve to third-party directories. The analysis window runs 1999-2023. (SI XII.A (p. 55); SI I.C fn. 2 (p. 18); SI Fig S2; main Fig 3A-B) | Client names decide classification and NAICS is only suggestive. High confidence requires a name rule. In the 2024-10-25 bulk-export vintage, issue-code coverage collapses after filing_year 2021. `fec/lobbying/collect_lobbyview.py:12-18,34,208-224` | The 40% figure is for one bill-quarter sample (SOPA, Q4 2011), not all LobbyView clients; no all-client or finance-client coverage share is reported. It supports not relying on NAICS for credit-union versus bank classification only as an illustration. Whether the post-2021 issue-code collapse is a bulk-export vintage artifact remains UNVERIFIED and should be checked against the OSF release. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-lobbying#Contact inference (activity to member)\|Contact inference (activity to member)]] | method-source | The paper defines a government-association function g(l,y), which is monotone in year, and a political-association function p(l,y), which is not monotone. Both are built from one-time disclosures of covered positions, and lobbyists give no timelines for those positions, so recency cannot be established. (SI I.D, pp. 20-21) | No shipped code path for the contact-inference lane was found; the nearest shipped surface records position_basis='not_disclosed'. The gold-positive design described for the component is not verifiable from code. `component lists no code path (lane path not recorded); nearest shipped surface: fec/lobbying/lobby_interactions.py:19-24 (position_basis='not_disclosed')` | The g and p definitions can be reused as documented, timestamp-free association labels. They are not contact evidence and must not become edge weights (D-021). Any use needs a 'no timeline' label, because the paper itself says it cannot date the ties. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-pathfinding#Ordered person-overlap routes and employment-routing overlay\|Ordered person-overlap routes and employment-routing overlay]] | supports | Covered-position disclosures are made once and have no dates, so recency cannot be established. Co-listing on filings shows registrant-lobbyist contracts, not dated co-work. (SI I.D (pp. 20-21); SI p. 22 (registrant-lobbyist contract edge definition)) | Filing-year-only episodes are routed separately as 'co_reported_firm_association'. They carry temporal_status 'filing_year_association_only' and weak=True, and are reachable only when include_weak is set (default False, filing_cost 5 >= historical_cost 3). Dated Politico co-work stays the main tie source. `fec/surfaces/staffer_routing.py:80-88,157-159,191,346-365` | The paper describes federal LDA lobbyists, and the routes cover congressional staff. The paper's point that filing data cannot date relationships applies directly to the filing-year hops. It says nothing about the dated payroll-overlap hops. Routes are heuristic introduction paths, not access probabilities (D-025). | moderate | VERIFIED |
| [[systems/mentat-giving-forecast#Calculator rules, company-PAC nowcast and BLEND\|Calculator rules, company-PAC nowcast and BLEND]] | supports | Registrants split into K-Street firms that file for others and in-house self-filers. The monetary field differs between them (income for firms, expenses for self-filers), and self-filers have a higher reporting threshold. (SI p. 17 (self-filer flag, monetary field and threshold); SI p. 19 (formal registrant-type definitions)) | The lobbying card shows hired_firms (income_total) and in_house (expenses_total) side by side and never adds them. `C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py:553-566` | This is the same LD-2 accounting, so the evidence carries over fully. The paper also assigns low-cost reports a threshold value as an upper bound. If Mentat's sector_spend rows ever take values from LobbyView, check whether they include that imputation. | moderate | VERIFIED |
| [[systems/mentat-giving-forecast#Cycle-aware giving refit (phase and drift log models, 50-50 growth)\|Cycle-aware giving refit (phase and drift log models, 50-50 growth)]] | qualifies | The paper's election-cycle 'synchronization' refers to target portfolios: agency-approach correlations form blocks by presidential administration, and some agencies such as the National Economic Council follow the presidential cycle. It reports no effect of election phase on lobbying dollars, and it aggregates data annually. (main pp. 5-6; Fig 3F; SI XI (Figs S34-S38); SI p. 16 (annualization)) | Mentat's analysis baseline repeats the latest complete cycle from the same four-year election phase (seasonal-naive), and giving_forecast.py carries a presidential-cycle term and same-phase share anchors; the lobbying card shows hired-firm and in-house levels separately. `C:/Users/pgoff/Mentat/mentat/app/analysis_metrics.py:690-693; C:/Users/pgoff/Mentat/mentat/app/giving_forecast.py:384-386,501,621-624` | The paper supports a four-year, administration-linked periodicity in whom lobbyists target, not in how much they spend. It cannot justify a phase coefficient on lobbying dollars, which has to rest on Mentat's own backtests. The PAC side (D-092) is not addressed at all, because the paper excludes PAC channels. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-sna#Bipartisanship (bpf)\|Bipartisanship (bpf)]] | qualifies | The Senate bipartisan cosponsorship index fell about 40% over the last 25 years (1999-2023), and legislator modularity is higher than lobbyists' and rising for senators. Lobbyist cross-party collaboration stayed relatively stable and lobbyist modularity is near zero. (SI XIII (Fig S46), pp. 48-50) | bpf is a static per-member cross-party cosponsorship share read from bipartisan.json with a default of 0.24 for missing members (moneyball_core.py:68-72). `fec/moneyball_core.py:68-72` | The paper's index is a chamber-level aggregate, not a member-level trait. It shows the cross-party baseline is strongly era-dependent, so a bpf or default fixed in one window should not be carried across Congresses without naming the window. It does not validate bpf as a measure of bridging. | weak | VERIFIED-SOFTENED |
| [[systems/melange-staffer-network#Staff directory matching and identity consolidation\|Staff directory matching and identity consolidation]] | qualifies | LobbyView's lobbyist name disambiguation is score-based and uncertain. Client matching relies on search-assisted lookup that resolves to third-party directories and fails for defunct firms. (SI I.C fn. 2 (p. 18); SI p. 20 (lobbyist disambiguation)) | A never-guess ladder plus payroll-evidence pins, with record-pair titles and organization strings required to be at least 8 characters. `fec/surfaces/staffer_identity_auto.py:106,176,211-215` | The domains differ: LDA lobbyist names versus congressional payroll staff. Wherever LobbyView lobbyist IDs enter the staffer graph as identities (D-021 allows identity use), treat them as probabilistic merges, not ground truth. The system's ladder is stricter than LobbyView's. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: The '40% NAICS coverage' is misattributed: the paper's ~40% Compustat/NAICS mapping is for clients that lobbied on SOPA in Q4 2011 (SI XII.A), not all LobbyView clients, so 'the paper's 40% NAICS coverage supports the rule that client names decide classification' overstates the evidence.
> - Summary issue: The cut-off is c0 ~ 85 in the main text and 86 in the Fig. 2 caption/SI; the exponent is 1.46 (Fig. 2F caption) or 1.54 (SI fit). 'About 1.5' is fine; quote the cut-off as 85-86.
> - Summary issue: The summary says the LDA 'never records registrant-legislator contact'; the paper says the LDA does not require registrants to disclose direct contacts with legislators. Use the paper's wording.
> - Summary issue: The revolving-door figures are internally inconsistent in the paper (main text 80% Senators / 60% Representatives; SI Fig. S9 text ~80% / 50%); any later use must name both and carry the no-dates caveat.
> - Summary issue: The claim that the House cross-party index also fell (bearing on bpf) was not located in the text; only the Senate ~40% drop and rising senator modularity are stated.

## Gaps and flags

- The system has no government-entity (agency or chamber) layer from LD-2. Lobbying links reach Congress only through bill citations. The paper shows agency targeting shifts by administration, and that executive-branch targeting (EOP) separated the IT firms on SOPA.
- Lobbying relationships are not treated as time-varying. Up to one-third of client-registrant ties rewire each year and about 20% of registrants survive two decades, so any static organization-to-firm or firm-to-lobbyist mapping goes stale within a year.
- There is no configuration-model null for the staffer or cosponsor graphs. The paper tests clustering and giant components against a bipartite configuration model, which would directly test whether staffer degree is container size (r = 0.87 recorded).
- Whether the bulk export's post-2021 issue-code collapse is a vintage artifact is unchecked. The paper plots issue portfolios through 2023 and points to OSF data (osf.io/y2hnm). This is UNVERIFIED.
- The low-cost-report upper-bound imputation and the 2008 change from semiannual to quarterly filing are not documented as checks in Melange's or Mentat's LDA level series.
- Paul's press-pillar and press-value-aggregate positions credit a 'Bacik augmentation' (sentiment-scored press as a determinant of pathway change). The paper contains no press, news-cycle, social or sentiment data. This is Paul's own extension and should not be cited as Bacik's evidence.
- Paul's cycle-model position reads Bacik as showing that lobbying synchronizes with the election cycle and that more money flows in election years. The paper's synchronization claim is about which agencies are targeted (blocks by presidential administration), not about dollars, so do not cite it for a phase coefficient on lobbying spend.
- The revolving-door figures (80% of Senators and 60% of Representatives in the main text, versus about 80% and 50% in the SI, with at least one connected lobbyist) are inconsistent within the paper. They are past-employment ties with no dates. Never quote them as access or influence (influence language is barred on revolving-door edges, and D-025 applies).
- The paper's networks are unweighted and disregard multi-edges by convention (SI pp. 22-23). That is not an argument for per-edge volume normalization or for dropping regular cosponsorship edges, both of which were REJECTED in this project. Do not cite it that way.
- Check staffer_routing.py against D-021. It builds person-to-person hops from co-listing on LDA filings ('co_reported_lobbying_activity' at reported_activity_cost 2, and 'co_reported_firm_association' as a weak opt-in at filing_cost 5). It is unconfirmed whether the reported-activity hop is on by default. If it is, LDA is acting as an edge source.
- Preferential attachment is descriptive only. The authors say fitting an evolution model is outside their scope, so it should not seed any weight or prior.
- SOPA is one bill, one quarter and 67 public firms. P(IT | SOPA) above 0.4 versus about 0.1 has no strata or uncertainty and should be quoted as a single-case illustration.

Paul's markup (217 highlights, no notes) focuses on LobbyView construction, disambiguation, the g and p association functions, registrant types, preferential attachment, election-cycle targeting and the polarization contrast. The parts that bear on the built system agree with it: the system treats LobbyView as an identity source, keeps names decisive over NAICS, keeps in-house and hired-firm spending apart, and borrows only documented definitions for contact inference. The paper's own admission that legislator contact is unobserved strengthens the system's refutation of the lobbied-member link. Paul's cited positions do not record that refutation, so they are stale on that point. Two of Paul's positions over-read the paper. The 'Bacik augmentation' with press sentiment is not in the paper, and the election-cycle synchronization concerns agency targets, not money. His readings for cooperation-v2 and bpf come from the lobbyist bipartisan index, which classifies agents and is not a member-level cooperation trait. The paper does document that the Senate cross-party cosponsorship baseline fell about 40%, which argues for naming the window on bpf.

## Related papers

- [[kim-2018-lobbyview-firm-level]] — same-data — The LobbyView database paper; Bacik et al. analyze the current LobbyView database (cited as ref 7, lobbyview.org).
- [[kim-2020-mapping-political-communities]] — same-data — Kim and Kunisky build bipartite lobbying networks from the same LobbyView and LDA bill links. They give the community-structure counterpart to Bacik's time evolution.
- [[kim-2025-measuring-interest-group]] — same-data — It adds LLM-coded bill positions to LobbyView reports. That fills the position field LD-2 lacks, which Bacik's network also lacks.
- [[azaiez-2025-hypergraph-analysis-european]] — extends — It takes the hypergraph and higher-order approach that Bacik defers to future work, applied to the EU lobby network.
- [[bombardini-2020-empirical-models-lobbying]] — cites — Bacik cites it (ref 1) as the survey of empirical lobbying models that the descriptive network work sits beside.
- [[furnas-2019-partisan-ties-lobbying]] — cites — Bacik cites it as ref 47 (Furnas, Heaney and LaPira 2019, Research & Politics) on the partisan ties of lobbying firms, which bears on its finding that lobbying is not polarized.
- [[furnas-nd-conflating-lobbying-pacs]] — complements — It covers the PAC channel that Bacik excludes, and finds low overlap between lobbying and PAC spending.
- [[kim-2020-strategic-sequential-links]] — complements — It links donations to lobbying, the step Bacik only mentions. This is relevant to the FEC-LDA bridge.
- [[harrison-2018-application-data-mining]] — same-data — It mines LDA records for spending patterns using the same disclosure source and faces similar extraction problems.
- [[kalla-nd-campaign-contributions-facilitate]] — cites — Bacik cites the access experiment (ref 49) when discussing access. The LDA data cannot observe that access.

### Linked from
- [[furnas-2019-partisan-ties-lobbying]] — complements — Bacik reports that registrants employing lobbyists with connections to both parties generate higher income per lobbyist (Fig. S46C) while legislators polarize; this paper finds bipartisan lobbying firms earn the highest average revenue.
- [[holyoke-2015-learnable-skills-unteachable]] — complements — Bacik et al. measure lobbyist networks over time, the empirical counterpart to the paper's argument that a lobbyist's value is their relationship portfolio.
- [[kim-2020-mapping-political-communities]] — same-data — Also built from LobbyView (co-authored by In Song Kim), as a yearly multi-layer network over time, where Kim-Kunisky give a single-Congress (113th) snapshot.
- [[goff-nd-cross-industry-finance]] — complements — Work on dynamics of the US lobbying network, relevant to the cycle-phase assumption. Its cycle-synchronization claim was not checked against the Bacik text here.
- [[kerr-2014-dynamics-firm-lobbying]] — complements — Item title covers change over time in the US lobbying network, a network-level counterpart to Kerr's firm-level persistence facts.
- [[montgomery-2017-effects-congressional-staff]] — complements — Vault item exists; title confirms a US lobbying network over time. Montgomery does not model lobbying, so the bracketing claim is conceptual.
- [[note-request-for-review-industry-economic-performance]] — complements — Reports lobbying network dynamics with periodicity synchronized with the presidential election cycle, supporting the memo Pres and Midterm controls in the lobbying model.
- [[note-industry-growth-and-political-spending-research]] — complements — Item record exists (Bacik et al. 2025, dynamical evolution of the US lobbying network). Title checked only; election-cycle synchronization claim not re-read.
- [[note-review-industry-growth-and-political-spending]] — complements — Paper text reports lobbying patterns synchronized with electoral cycles, which is the phase pattern full year effects absorb. The review does not cite it; the link is topical.