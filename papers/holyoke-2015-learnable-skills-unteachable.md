---
type: paper
title: "Learnable Skills, or Unteachable Instinct? What Can and What Cannot Be Taught in the Lobbying Profession"
authors: ["Thomas T. Holyoke (Dept. of Political Science, California State University, Fresno)", "Heath Brown (Dept. of Public Management, John Jay College of Criminal Justice, CUNY)", "Timothy M. LaPira (Dept. of Political Science, James Madison University)"]
year: "2015"
venue: "Interest Groups & Advocacy (forthcoming, 2015); manuscript dated November 2014"
doi: ""
category: "Lobbying"
systems: ["melange-pathfinding", "terrier-row-scoring", "melange-influence-engine", "melange-demographic-potential"]
components: ["melange-pathfinding.staffer-overlap-routes", "terrier-row-scoring.sphere-of-influence", "melange-influence-engine.flip-model", "melange-demographic-potential.footprint-v30"]
paul_annotated: true
paul_markup_count: 57
evidence_status: "unverified"
source: "[[_sources/holyoke-2015-learnable-skills-unteachable.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\Holyoke__Brown__and_LaPira_2015 (1).pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\Holyoke__Brown__and_LaPira_2015 (1).pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\Holyoke__Brown__and_LaPira_2015 (1).pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Lobbying\\Holyoke__Brown__and_LaPira_2015 (1).pdf"]
sha12: ["90e90ca46723"]
tags: [paper, lobbying]
---

# Learnable Skills, or Unteachable Instinct? What Can and What Cannot Be Taught in the Lobbying Profession

**Thomas T. Holyoke (Dept. of Political Science, California State University, Fresno), Heath Brown (Dept. of Public Management, John Jay College of Criminal Justice, CUNY), Timothy M. LaPira (Dept. of Political Science, James Madison University) (2015).** *Interest Groups & Advocacy (forthcoming, 2015); manuscript dated November 2014.* Theme: [[themes/lobbying|Lobbying]].

> [!abstract] Summary
> Holyoke, Brown and LaPira write a conceptual essay with a curriculum proposal. It is not an empirical study. They argue that most of lobbying can be taught at graduate level, in three areas: knowledge (process, policy and analytics), communication and messaging, and relationship building. Relationship building is only partly teachable because it depends on personality. The paper has no data, sample, window or tests. Every number it gives comes from someone else: about 30,000 DC lobbyists in 2012, 12% of them under 35 (Rehr 2012); 52% came through the revolving door (LaPira and Thomas 2014); 37% knew next to nothing about LDA/HLOGA. Its substantive claims are hedged with words like "arguably" and "suggests". They are: relationships rest on mutual need and are aimed at lawmakers with a relevant constituency stake, not on friendship; group members contact lawmakers more than non-members do; identical mass messages are worthless; campaign finance matters less than people think, and few lobbyists give at all.
> 
> The paper is weak evidence for Paul's systems. It is useful as framing, not calibration. Its sharpest bearing is on the staffer overlap router. In the code (`fec/surfaces/staffer_overlap_routes.py:33-39`), tie strength is proximity × log2(1 + shared days / 30) of dated co-work. The paper says a working relationship comes from mutual need and targeting, not from time spent in the same office. So the router's strength is at best a proxy for the chance of contact. Treat it as a heuristic cost, never as relationship strength or access (D-025). The paper also loosely backs three things: the flip model treating money as the minor channel; Terrier's Sphere of Influence using civic engagement; and the footprint version of Demographic Potential, through constituency-stake targeting. None of these backs any weight or constant.

## Paul's annotation

From [[annotations/Lobbying - Lobbying - Annotated Bibliography|Lobbying - Lobbying - Annotated Bibliography]]:

> Holyoke, Thomas T, et al. “Learnable skills, or unteachable instinct? what can and what cannot be taught in the lobbying profession.” *Interest Groups &amp; Advocacy*, vol. 4, no. 1, 3 Feb. 2015, pp. 7–24, [https://doi.org/10.1057/iga.2014.27](https://doi.org/10.1057/iga.2014.27).

## Paul's markup in the PDF

- p2 Highlight: "Paraphrase: a core set of lobbying skills is teachable at graduate level"
- p2 Highlight: "Paraphrase: three types of knowledge (political process, policy, analytic) can be taught"
- p2 Highlight: "Paraphrase: students should master oral and written advocacy communication"
- p2 Highlight: "Paraphrase: relationship-building best practices can be modelled"
- p3 Highlight: "Paraphrase: practitioners assume lobbying depends on people skills and instincts"
- p3 Highlight: "Paraphrase: lobbying as a teachable subject, limited to the classroom"
- p4 Highlight: "Paraphrase: lobbying spans K Street contract lobbyists and small nonprofit advocates"
- p5 Highlight: "Paraphrase: Rehr figure, only 12 per cent of DC lobbyists under 35"
- p5 Highlight: "Paraphrase: about 52 per cent came through the revolving door, many with several posts"
- p6 Highlight: "Paraphrase: Herring (1929) on professionalised lobbying; good instincts come from training"
- p6 Highlight: "Paraphrase: relationship building; experienced lobbyists know which lawmakers are receptive"
- p7 Highlight: "Paraphrase: an example where experience reveals a committee chair's real behaviour"
- p7 Highlight: "Paraphrase: personality traits (rudeness, poor social skills) that training cannot fix"
- p8 Highlight: "Paraphrase: Kennedy (1956) quote that lobbyists explain complex subjects clearly"
- p8 Highlight: "Paraphrase: knowledge of how to use facts to advance a career"
- p9 Highlight: "Paraphrase: knowledge of the political process is valuable because the process is complex"
- p9 Highlight: "Paraphrase: government structure is teachable; individual personalities are not"
- p10 Highlight: "Paraphrase: teaching lobbying law so courses avoid cynicism"
- p10 Highlight: "Paraphrase: regulations as an entry point into professional ethics"
- p10 Highlight: "Paraphrase: lobbyists often specialise in one or two policy areas"
- p10 Highlight: "Paraphrase: lawmakers vote on policies they do not understand"
- p11 Highlight: "Paraphrase: lobbying content can sit within other political science and policy courses"
- p11 Highlight: "Paraphrase: firms hire economists and statisticians to build the facts they use"
- p11 Highlight: "Paraphrase: research methods and quantitative training should be strengthened"
- p11 Highlight: "Paraphrase: master's courses in applied data analysis and infographics"
- p12 Highlight: "Paraphrase: knowledge is what lobbyists sell and what makes them valuable"
- p12 Highlight: "Paraphrase: informed lobbyists attract coalition partners and networks"
- p12 Highlight: "Paraphrase: focus on government structure and data analysis improves job prospects"
- p13 Highlight: "Paraphrase: basic writing competency is missing in many undergraduates"
- p13 Highlight: "Paraphrase: a long or unclear message will never be read"
- p13 Highlight: "Paraphrase: volume of paper and email reaching congressional offices (CMF)"
- p13 Highlight: "Paraphrase: only persuasive, well-written messages get through to staff"
- p13 Highlight: "Paraphrase: short writing assignments in communication courses (footnote 4)"
- p14 Highlight: "Paraphrase: messages should be brief enough for a Twitter post"
- p14 Highlight: "Paraphrase: lobbyists must write persuasively"
- p14 Highlight: "Paraphrase: good messages move lawmakers and mobilise constituents"
- p14 Highlight: "Paraphrase: doctoral writing norms may not match lobbying firm expectations"
- p15 Highlight: "Paraphrase: trusted groups' websites and Facebook pages are monitored by staff"
- p16 Highlight: "Paraphrase: value of follow-up after grassroots messages"
- p16 Highlight: "Paraphrase: entrenched interests signal importance through prominent constituents calling first"
- p16 Highlight: "Paraphrase: members should target their own elected officials"
- p17 Highlight: "Paraphrase: relationship building straddles the line between teachable and not teachable"
- p17 Highlight: "Paraphrase: part of relationship skill rooted in personality"
- p17 Highlight: "Paraphrase: reciprocal favours build trust, mutual need and obligation"
- p17 Highlight: "Paraphrase: relationships can matter more to lobbyists than to the people they represent"
- p17 Highlight: "Paraphrase: start of the five teachable relationship-building elements"
- p17 Highlight: "Paraphrase: lawmaker relationships differ from personal or business relationships"
- p17 Highlight: "Paraphrase: relationships rest on mutual need and each side's ability to supply something"
- p17 Highlight: "Paraphrase: relationships should be targeted"
- p18 Highlight: "Paraphrase: lobbyists need relationships to get their work done"
- p18 Highlight: "Paraphrase: identify lawmakers responsive to information and with electoral interest in the constituency"
- p18 Highlight: "Paraphrase: students must learn to look at ease socialising with others"
- p18 Highlight: "Paraphrase: attend fundraisers even when unwelcome"
- p18 Highlight: "Paraphrase: fake smiles at events are common"
- p19 Highlight: "Paraphrase: a professional lobbying curriculum needs a code of ethics"
- p19 Highlight: "Paraphrase: confidentiality teaching (annotation text truncated)"
- p19 Highlight: "Paraphrase: ethics teaching should cover lobbyists' role in campaign money"

## Study

**Question.** Can the knowledge, skills and abilities needed for lobbying be taught to pre- and early-career lobbyists at the graduate professional level, and which parts cannot be taught (Executive Summary p.2; "What Can Be Taught" pp.8-19)?

**Theory.** Lobbying is treated as a profession with a growing body of knowledge. Lobbyists' value rests on information lawmakers lack (Hula 1999; Hall and Deardorff 2006, as cited p.11-12), on being trusted agents for principals (Ainsworth and Sened 1993; Kersh 2002, p.13-14), and on reciprocal relationships built on mutual need (Wright 1996; Susman 2008, p.17). The authors divide teachable lobbying into three areas: knowledge (process, policy, analytic), communication and messaging, and relationship building. Relationship building is only partly teachable because it is tied to personality (p.17). Experience and personality are acknowledged as important but not sufficient (p.3-8).

**Data.** No dataset, sample, unit of analysis, time window or N. Evidence is drawn from: (a) published figures cited from others, namely Rehr 2012 (about 30,000 DC lobbyists in 2012; survey finding on LDA/HLOGA knowledge) and LaPira and Thomas 2014 (revolving door), p.5 and p.10-11; (b) interviews cited in the text and notes, namely Dan Hurley (25 Aug 2014, n.1), Brad Fitch of the Congressional Management Foundation (4 Sep 2014, n.5), Alan Rosenblatt (11 Sep 2014, n.7), and a CMF communication from Nicole Folk Cooper (11 Sep 2014, n.5, on file, not published); (c) the authors' own experience, including past work in politics; (d) a Mike House quote from Goldman 2012 (p.3). Era of cited data: 2012-2014.

**Method.** Qualitative and argumentative. Literature synthesis from political science on interest groups and lobbying, interview-informed claims, and personal experience, used to propose a graduate curriculum (seminars, simulations, capstone, internship or practicum). No statistical estimation, coding, or formal test of the proposed curriculum.

**Identification.** None. The paper makes no causal claims and has no identification strategy.

**Scope.** US lobbying profession, centred on Washington DC federal lobbying (p.4-5), with brief mention of state, local and supranational institutions (p.25). Covers contract lobbyists, nonprofit advocates, grassroots managers, regulatory specialists, coalition brokers and PR consultants (p.4-5). Target population is pre- and early-career students in graduate professional programs, with undergraduate political science as a feeder (p.5-6, p.23). Campaign finance discussion is explicitly national (p.20).

## Key findings

- Central claim: much of lobbying, though not all, can be taught at graduate level. Three areas are proposed: knowledge (process, policy, analytic), communication and messaging, and relationship building. (Executive Summary p.2; What Can Be Taught pp.8-19; None)
- Experience and personality limit what training can do; the authors argue good instincts are as much a product of training as of experience or natural ability. (p.6-8; None)
- Lobbyist population and career entry: of roughly 30,000 people who more or less were DC lobbyists in 2012, only 12 per cent are younger than 35 (Rehr 2012). (p.5; 30,000 lobbyists (2012); 12% under 35)
- Revolving door: about 52 per cent of these lobbyists came through government, and 48 per cent of those held two or more government positions (LaPira and Thomas 2014, as cited). (p.5; 52% revolving door; 48% of those with 2+ government positions)
- About 37 per cent of DC lobbyists surveyed by Rehr knew next to nothing about what the LDA and HLOGA required of them. The authors use this to argue for compliance training. (p.10-11 (Rehr 2012, pp.6-7); 37% knew next to nothing about LDA/HLOGA)
- Congressional offices are overloaded: each office receives about a foot and a half of paper a day, plus email and social media (Congressional Management Foundation, cited). (p.13 (Goldschmidt and Ochreiter 2008); About 1.5 feet of paper per office per day)
- Campaign finance: the evidence on its impact is at best mixed (Baumgartner and Leech 1998). Very few lobbyists engage in campaign finance and those who do are partisan (Koger and Victor 2009). The authors conclude campaign finance matters less in lobbying than popular belief holds. (p.20; 'Very few' lobbyists (no figure given))
- Five teachable relationship-building elements: (1) relationships rest on mutual need, not personal friendship; (2) relationships are targeted at lawmakers with a relevant interest; (3) lobbyists must look at ease socialising, including at fundraisers; (4) internships and practicums are where first professional networks form; (5) ethics, including honesty, confidentiality limits, and the lobbyist's primary duty to the client. (p.17-20 (the authors say 'arguably', p.17); Five elements, none quantified)
- Networks and value: a lobbyist's market value, especially for a for-hire firm, is their portfolio of relationships (citing Blanes i Vidal et al. 2012). The authors warn these relationships can come to matter more to lobbyists than to the people they represent (citing Holyoke 2011). (p.17; None)
- Coalitions: lobbyists are more attractive to coalition partners when they bring information others lack and want (Hula 1999). Informed lobbyists also attract other lobbyists' attention. (p.12; None)
- Messaging and targeting: personalised messages are far more effective than duplicates (Fitch 2010); constituent messages with a personal story persuade members most; members of interest groups contact lawmakers more than non-members (Holyoke 2013); identical e-petitions are called worthless (Fitch 2009; Shih 2011). (p.15-17; None)
- Curriculum: teach lobbying mainly at graduate level, with continuing education later. Undergraduate courses should cover fundamentals and fold lobbying into existing classes. Graduate programmes should combine lectures and seminars, communication and PR courses (including social media), simulations, a capstone simulation with surprise shocks, and required internships or practicums. (p.23-25; None; 'three thoughts ... not a steadfast instructional rule' (p.23))
- Simulation modules are proposed in three groups: process (recruiting clients, budgeting, procedure), policy (strategy, stakeholder mapping, policy histories, state and international venues), and analytical (data collection, white papers, testimony, reporting). (p.21-22; None)

## Limitations

- No formal limitations section. The authors say not everything a lobbyist needs can be taught in the classroom, and that experience and personality matter (p.3-4, p.7-8).
- Hedged claims: the five relationship-building elements are introduced with 'arguably' (p.17), and messaging recommendations are framed as 'suggests' (p.17).
- Curricular recommendations are explicitly 'an approach, but not a steadfast instructional rule' (p.23). Nothing has been tested or evaluated.
- Simulations 'often cannot capture the unexpected and unpredictable swings of political warfare' (p.23).
- Authors question whether political scientists are prepared to teach professional writing (p.14).
- Anecdotal support: the claim that short writing assignments are common comes from colleagues ('Or so we have heard', n.4, p.26). Interview claims are not independently verifiable (CMF communication 'on file ... available on request', n.5).
- The ethics code of the American Government Relations Professionals binds only members, and there is little enforcement (n.6, p.26-27).
- Key statistics (30,000 lobbyists, 12% under 35, 52% revolving door, 37% knowledge gap) are cited from secondary or association sources and are not re-derived by the authors.
- Mentions 'cold cynicism' and other normative judgements without empirical support.

## Short quotes

- "Politics today is big business requiring a big education" — p.6 (Holyoke, Brown and LaPira, 'What Perhaps Cannot be Taught')

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-pathfinding#Ordered person-overlap routes and employment-routing overlay\|Ordered person-overlap routes and employment-routing overlay]] | qualifies | Relationships with lawmakers are built on mutual need and each side's ability to supply something, and are targeted, not personal friendships (p.17-18, introduced as 'arguably'). A lobbyist's market value is their portfolio of relationships (citing Blanes i Vidal et al. 2012). About 52% of roughly 30,000 DC lobbyists (2012, Rehr) came through government, and 48% of those held two or more posts (LaPira and Thomas 2014, as cited p.5). These figures are secondary and the authors did not re-derive them. (p.5; p.17-18) | Routes run through dated co-work, with strength = proximity x log2(1 + days/30) and distance = ceil(1e6 x 128 / strength). The overlay costs are: current co-worker 1, reported activity 2, historical 3, filing 5, policy 8. Shared tenure stands in for how usable a tie is. `fec/surfaces/staffer_overlap_routes.py:33-39; fec/surfaces/staffer_routing.py:156-159` | Conceptual only. The paper never measures whether shared tenure produces usable relationships. It implies that co-work measures the opportunity for contact, not the mutual need that keeps a relationship going. Its revolving-door share is a federal DC figure from about 2012-2014 and fits the router's staff-to-lobbyist population. It does not support any cost constant or the log-days form. Route cost must stay a heuristic, not an introduction probability (D-025). No influence wording on revolving-door edges. | weak | UNVERIFIED |
| [[systems/terrier-row-scoring#Sphere of Influence\|Sphere of Influence]] | supports | Members of interest groups contact lawmakers more than non-members do (Holyoke 2013, cited). Entrenched interests show an issue's importance by having prominent constituents call first. Members should target their own elected officials, and personal constituent stories persuade most (Fitch 2010, cited). (p.15-17) | Sphere of Influence = 100 x (0.10 household + 0.45 giving + 0.45 volunteering). Giving is built from charitable-donor, donor-index and high-contribution vendor fields. There is no term for group membership or prior contact with officials. The weights are Paul's 'leaning' ruling. `C:/Users/pgoff/Documents/Codex/2026-09-27/we-are-going-to-have-an/outputs/cu-geography/.local/row-framework-20260930/build/rowfw-1.0.0/framework_config.json (groups.soi formula, weights 1/10, 9/20, 9/20)` | Only the direction carries over: civically engaged people are likelier to contact their own representative. The claims are cited, not tested, and are about national federal contact. Vendor charitable-giving fields are not the political group membership the paper names. Nothing here supports the 45/45/10 weights. | weak | UNVERIFIED |
| [[systems/melange-influence-engine#Flip - winnability model\|Flip - winnability model]] | supports | Evidence on campaign finance's impact is at best mixed (Baumgartner and Leech 1998). 'Very few' lobbyists take part in campaign finance, and those who do are partisan (Koger and Victor 2009). The authors conclude that campaign money matters less to lobbying than popular belief holds. No figure is given. (p.20) | pflip uses WM = 0.35 for money and WP = 0.65 for peer proximity. The code comment says money is the minority partner and buys access, not votes. Money terms saturate relative to receipts. `fec/engine/flip_model.py:3-6,21` | The paper agrees with the ordering (money below relationships and information) but gives no estimate. It relies on secondary sources from 1998-2009. It speaks about lobbyists' personal giving, not PAC money given to members, so it cannot calibrate the 0.35/0.65 split or HALFF. | weak | UNVERIFIED |
| [[systems/melange-demographic-potential#Demographic Potential v3.0 (CU footprint, default)\|Demographic Potential v3.0 (CU footprint, default)]] | context | Relationship targeting means finding lawmakers who respond to information and who have an electoral interest in the group's constituency (p.18). Lobbyists should pick out lawmakers with a relevant interest (p.17). (p.17-18) | The default footprint mode scores a district's existing CU footprint (employment LQ, establishments, branches, deposits) and qualifies a district at employment LQ >= 1.0. Paul's ruling: 'the footprint should be the potential'. The weights are labelled judgmental and unestimated. `fec/district/demographic_potential.py:25-45` | This matches the targeting logic only in concept: aim at members whose constituents hold a stake. The paper does not test whether that stake predicts responsiveness, and Demographic Potential does not feed Demo%. No weight support. | weak | UNVERIFIED |

## Gaps and flags

- The staffer routers have no mutual-need or reciprocity term. They model co-presence only. The paper's account (mutual need, targeting, exchange of favours) has no counterpart, and reciprocity is not validated in this project (D-104..D-108), so there is nothing to wire in yet. It should stay a stated limit of what route cost means.
- There is no measure of personalised versus mass messaging. The paper (citing Fitch) calls identical constituent messages worthless. Meanwhile frame alignment (fec/frame_alignment.py, per its component description, not opened here) treats cross-member near-duplicate text as a coordination signal that raises salience. The two concern different senders (members versus constituents), so this is not a conflict. But no component tests whether duplicated messaging marks low-effort signalling.
- Terrier's Sphere of Influence has no term for organisational membership or prior contact with officials. The paper's cited mechanism (group members contact lawmakers more) has no field in it.
- The system has no lobbyist-side skill or experience attribute. The paper claims experience and personality limit what a lobbyist can do, but the staffer network carries only tenure and position, and no data source for skill exists.
- D-025: the paper says a lobbyist's value is their portfolio of relationships, and that relationships can come to matter more to lobbyists than to their clients. Do not import that as closeness, access or introduction-probability language for route costs, PageRank or shared-background scores. Those stay heuristic model outputs.
- Revolving-door wording: the 52% figure (LaPira and Thomas 2014 via Rehr; about 30,000 DC lobbyists, 2012; secondary, not re-derived) can be quoted with that window and denominator. It must not be used to describe revolving-door edges in the staffer graph as influence.
- D-021, observed in code while confirming this bearing, not raised by the paper, UNVERIFIED (DE-062): fec/surfaces/staffer_routing.py:304-333 builds a 'co_reported_lobbying_activity' hop from shared reported lobbying activity IDs. Its default cost is 2.0 (line 157), cheaper than a historical co-work hop (3.0). The hop is marked cost_overlay_only and review_only, and its scope reads 'employment overlap and personal familiarity not established'. If the activity index comes from LDA filings, it uses LDA as an edge or weight source in an overlay, which is in tension with D-021. Paul should confirm whether this was ruled on.
- No rejected SNA proposal is argued for. The paper says nothing about dropping regular cosponsorship edges or per-edge volume normalisation.
- Evidence-strength flag: every quantitative claim in the paper is secondary, from association surveys, interviews 'on file' or the authors' own experience. Treat it as a framing citation, never a calibration source.

Paul recorded no takeaways for this paper in either annotated bibliography. His 57 highlights (pp.2-19) all have empty notes. They cluster on revolving-door share (p.5), targeted relationships built on mutual need (p.17-18), quantitative and data-analysis training (p.11), and prominent constituents calling their own members (p.16). His component positions cite Holyoke for staffer-overlap-routes ('relationships are targeted and rest on mutual need') and for Terrier's sphere-of-influence. The evidence agrees with those readings only as framing. It undercuts any temptation to read the overlap router's log-days strength as relationship strength: the paper places relationships in mutual need, not co-presence. His notes are not stale, since nothing here was later refuted, but they are thin. The routers built since (dated co-work routes, the reported-activity overlay) have moved past what the paper can support, and nothing in his notes ties the paper to their cost constants.

## Related papers

- [[bonica-2013-mapping-ideological-marketplace]] — cites — The paper cites Bonica (AJPS 58(2), 'Mapping the ideological marketplace') for the partisan spread of campaign money by industry (p.20).
- [[furnas-nd-conflating-lobbying-pacs]] — complements — Furnas et al. find low overlap between organisational lobbying and PAC spending. That gives empirical weight to the paper's unquantified claim (citing Koger and Victor) that few lobbyists engage in campaign finance.
- [[kalla-nd-campaign-contributions-facilitate]] — complements — Kalla and Broockman's field experiment shows contributions buy access. It qualifies the paper's view that campaign finance matters little: money matters for access, not necessarily for votes, which is the flip model's stance.
- [[furnas-2019-partisan-ties-lobbying]] — complements — This work on the partisan ties of lobbying firms tests empirically the paper's cited claim that lobbyists who give are partisan.
- [[bacik-2025-measuring-dynamical-evolution]] — complements — Bacik et al. measure lobbyist networks over time, the empirical counterpart to the paper's argument that a lobbyist's value is their relationship portfolio.
- [[pritoni-2025-lobbying-coalitions-determinants]] — complements — Both draw on Hula (1999). The paper says informed lobbyists attract coalition partners, and Pritoni's review covers the determinants of coalitions, including information and brokers.
- [[boxsteffensmeier-2018-taking-congress-interest]] — complements — Both cover how interest-group signals reach overloaded congressional offices. The paper gives the messaging advice and Box-Steffensmeier measures cue-taking from Dear Colleague letters.

### Linked from
- [[bombardini-2020-empirical-models-lobbying]] — complements — The review's access-versus-expertise evidence (Bertrand et al. 2014) bears on which lobbying skills are teachable.
- [[furnas-2019-moneyed-interests-information]] — complements — Holyoke et al. treat lobbying as largely relationship building. This paper adds staff-side evidence that the information a lobbyist brings, and its ideological source, shapes siding; its point that lobbyists are more persistent than donors is the authors' speculation (p.17), not a finding.
- [[defigueiredo-2013-advancing-empirical-research]] — complements — Holyoke, Brown and LaPira (2015) on what can and cannot be taught in lobbying. Addresses the expertise side of the essay's expertise-versus-connections question.