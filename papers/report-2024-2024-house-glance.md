---
type: non-article
title: "2024 House At-A-Glance"
authors: ["Cook Political Report (publisher named in the text; no individual author listed)"]
year: "2024"
venue: "Cook Political Report, dated September 12, 2024"
doi: ""
category: "Campaign Politics"
systems: ["melange-influence-engine", "mentat-sector-growth-study"]
components: ["melange-influence-engine.competitiveness-multiplier", "melange-influence-engine.receptivity-layer", "melange-influence-engine.electoral-plays", "mentat-sector-growth-study.growth-spending-regressions", "mentat-sector-growth-study.seats-weekly-test"]
paul_annotated: false
paul_markup_count: 0
evidence_status: "verified"
source: "[[_sources/report-2024-2024-house-glance.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\Codex\\2026-09-26\\loo\\work\\seat_gain_extension\\cook_20240912_house.pdf"]
sha12: ["546c919128d4"]
tags: ["paper", "campaign-politics"]
---

# 2024 House At-A-Glance

**Cook Political Report (publisher named in the text; no individual author listed) (2024).** *Cook Political Report, dated September 12, 2024.* Theme: [[themes/campaign-politics|Campaign Politics]].

> [!abstract] Summary
> This is not a research paper. It is the Cook Political Report's House race table from September 12, 2024. For each of the 435 districts it lists the incumbent (or OPEN/VACANT), the Cook PVI and the Cook race rating. It has no method section and no inference. Parsed per district row, the ratings sum to 435: 24 Toss Ups and 18 Lean races. There is also a broken `#REF!` cell at MA-01.
> 
> What matters for Paul's system is that **in Cook's own table, PVI and rating do not line up**. The 24 Toss Ups run from R+8 (AK-AL, Peltola) to D+5 (CA-22 Valadao, NY-04 D'Esposito). Seats with the same small lean get very different ratings:
> - The six EVEN seats carry five different ratings: FL-27 (Salazar) Solid R, PA-01 (Fitzpatrick) Likely R, NH-01 Likely D, PA-17 Lean D, and CO-08 and NE-02 Toss Up.
> - AZ-04 (Stanton) is D+2 and Solid D, while OR-05 (Chavez-DeRemer) is D+2 and Toss Up.
> 
> The rating includes incumbent strength and candidate factors that PVI leaves out.
> 
> Melange's competitiveness multiplier c (COMPET_MULT, on by default) is a function of Cook PVI alone. With the dial on, it also turns off the receptivity layer's candidate-specific competitiveness term, which came from Ballotpedia vote results. So the system currently gives a safe EVEN-PVI incumbent (Salazar) the same c (2.0) as a toss-up EVEN-PVI incumbent (Bacon).
> 
> The table is also a dated source of Cook ratings for the sector growth study's `toughness_general` control, which averages Cook, Inside Elections and Sabato ratings on a 0 (safe) to 3 (toss-up) scale, and possibly for the expert-rating path of the seats test. Nothing in the table bears on whether dollars buy more in competitive seats. That reciprocity assumption stays unvalidated. The bearings below were checked against the text and code (see the failure callout for what did not hold).

## Paul's annotation

_Not in Paul's annotated bibliographies._

## Study

**Question:** 

**Theory:** 

**Data:** Table of 2024 U.S. House races grouped by state. Unit = congressional district (435 district markers counted in the text, matching the 435 seats). Each block gives the district description, incumbent or OPEN/VACANT note, Cook PVI, Cook rating, and challengers with party and a short descriptor. A bullet marks candidates who have announced or filed. Snapshot date September 12, 2024. No survey sample or respondents.

**Method:** None stated. The text presents the table only and does not describe how the ratings or PVI values were produced.

**Identification:** None. Descriptive table; no causal or inferential design.

**Scope:** Population: all 2024 U.S. House districts, Alaska (AK-AL) through Wyoming (WY-AL). Institution: U.S. House of Representatives. Era: 2024 election cycle.

## Key findings

- Rating tally parsed per district row: Solid D 174, Solid R 192, Likely D 16, Likely R 11, Lean D 13, Lean R 5, Toss Up 24. These sum to 435, matching the seats. (Solid 366; Likely 27; Lean 18; Toss Up 24; Whole document, race rows in state blocks (lines 17-2782))
- Open seats are flagged as OPEN with the departing incumbent's name in parentheses. The text shows 41 such lines, plus 4 VACANT seats (NJ-09, NJ-10, TX-18, WI-08 per the text). (41 OPEN lines; 4 VACANT lines; Race rows, e.g., AZ-08 (line 138), CA-12 (line 209), TN-07 (line 1287 region) and similar rows)
- Six races carry the EVEN PVI label in the text (e.g., NH-01, CO-08, PA-01, PA-17, NE-02, FL-27), and the file shows Toss Up ratings in the 24 district rows including CA-22, CA-27, CA-45, AZ-01, AZ-06, MI-07, MI-08, NC-01, NJ-07, NM-02, NY-04, NY-17, NY-19, OH-13, OR-05, PA-07, PA-08, TX-34 region rows and others. (EVEN appears 6 times; PVI and rating columns in state blocks)

## Limitations

- No methodology, rating criteria, or PVI computation is described in the text.
- Line 1120 (MA-01 challenger slot) reads #REF!, a broken spreadsheet reference in the source.
- Snapshot as of September 12, 2024; later rating changes are not reflected.
- Highlighting for Lean and Toss Up rows (the header says competitive races are highlighted) is not recoverable from plain text.

## Short quotes

- "Competitive races (Lean and Toss Up) are highlighted." (Header, lines 6-7)
- "indicates that a candidate has announced or filed to run." (Header, line 6)

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-influence-engine#Competitiveness multiplier c (COMPET_MULT)|Competitiveness multiplier c (COMPET_MULT)]] | qualifies | Cook's table separates PVI from race rating. Parsed per district row (435 rows), the 24 Toss Ups span R+8 (AK-AL) to D+5 (CA-22, NY-04). The six EVEN seats get five different ratings: FL-27 Solid R, PA-01 Likely R, NH-01 Likely D, PA-17 Lean D, CO-08 and NE-02 Toss Up. Seats at D+2 or D+3 are Solid D (AZ-04, CT-02, GA-02), Lean D (CT-05, NY-03) or Toss Up (OR-05, CA-45, NY-17; all three held by Republicans). Window: 2024 cycle as of 2024-09-12. Denominator: 435 districts. (Race rows: AK-AL lines 17-20; FL-27 lines 682-685; PA-01 lines 2070-2073; CO-08 lines 487-490; NE-02 lines 1570-1573; AZ-04 lines 117-120; OR-05 lines 2054-2057 (_text/546c919128d4.txt)) | c = 1 + 1.0*exp(-max(lean,0)/5), lean = Cook PVI oriented to the member's party (Cook 2026 district PVI for House). Any seat at EVEN or leaning against the member gets the maximum c = 2.0. Illustration with this table's 2024 PVIs: Salazar (EVEN, Solid R) and Bacon (EVEN, Toss Up) both get 2.0; Stanton (D, D+2, Solid D) gets about 1.67, whereas the table's D+2 Toss Up incumbents (Chavez-DeRemer OR-05, Steel CA-45) are Republicans, so lean = -2 and c = 2.0; Courtney (D+3, Solid D) and Suozzi (D+3, Lean D) both get about 1.55. Verifier arithmetic on 2024 PVIs, not the live 2026 artifact. `fec/moneyball_core.py:90-93; fec/engine/build_competitiveness.py:7-8` | The structural point carries over: PVI is a partisan baseline, while the rating adds incumbent and candidate factors. The numbers do not carry over: they use 2024 PVIs, which come from the 2016 and 2020 presidential results, and 2024 ratings, while the system reads Cook 2026 PVIs for the 119th Congress roster. It applies only to House members; senators use Paul's state PVI table. The table says nothing about whether money works better in competitive seats, so it neither validates nor refutes the reciprocity premise behind c. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-influence-engine#Receptivity layer (susceptibility, incentive, prospect domain, seed probability)|Receptivity layer (susceptibility, incentive, prospect domain, seed probability)]] | qualifies | Cook's rating differs from PVI for incumbents with personal electoral strength. Fitzpatrick is EVEN and Likely R. Salazar is EVEN and Solid R. Cuellar is D+3 and Likely D, while Hayes is D+3 and Lean D. A candidate-specific vulnerability signal therefore carries information that PVI lacks. (PA-01 lines 2070-2073; FL-27 lines 682-685; TX-28 lines 2473-2476; CT-05 lines 523-526) | The Fouirnaies-Hall incentive term 1+.30*(comp-0.5) takes comp = 1 - security from Ballotpedia vote results, so it is candidate-specific. With COMPET_MULT on (the default), this term is set to neutral (1.0) and the PVI-only c carries competitiveness instead. As a result, the engine path no longer sees candidate-specific vulnerability. `fec/members/receptivity_layer.py:129-137` | This is the clearest example of the PVI-versus-rating gap affecting a live dial. It holds as long as COMPET_MULT=1 neutralizes the Ballotpedia comp term. A 2024 snapshot cannot calibrate 119th-Congress values; it only shows that the two measures diverge. | moderate | VERIFIED |
| [[systems/melange-influence-engine#Electoral security, leverage and contest-defend plays|Electoral security, leverage and contest-defend plays]] | suggests-extension | The table gives a forward-looking expert risk category (Solid, Likely, Lean, Toss Up) for each seat, along with OPEN and VACANT flags: 41 OPEN lines and 4 VACANT seats as of 2024-09-12. (Header lines 4-7; OPEN rows such as MI-07 lines 1273-1276 and MI-08 lines 1279-1282) | build_electoral.py (writes electoral.json, not electoral_plays.json): security is a percentile of Ballotpedia's backward-looking Election_Average; leverage = 1 + LELEV*(1 - security), LELEV default 0.8; unmatched members get neutral security 0.5. `fec/engine/build_electoral.py:7,22-31` | Useful only as the design pattern for a dated, forward-looking risk input. Using it would require the current-cycle Cook ratings, dated before the decision, not this 2024 table. Open-seat retirees would also need the same retiring treatment the receptivity layer already gives them. | weak | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#Sector growth vs giving, partisan share and lobbying regressions|Sector growth vs giving, partisan share and lobbying regressions]] | measurement-source | Dated Cook ratings for all 435 2024 House races on a four-step scale (Solid, Likely, Lean, Toss Up): per-row totals Solid D 174, Solid R 192, Likely D 16, Likely R 11, Lean D 13, Lean R 5, Toss Up 24 (sum 435). One source quirk: the MA-01 challenger slot (line 1120) holds a broken #REF! cell. (Whole table, lines 17-2782; #REF! at line 1120) | The champions models in the sector growth study use toughness_general, the mean competitiveness of the Cook, Inside Elections and Sabato ratings (0 safe to 3 toss-up), as a regressor with cycle and board fixed effects. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_registry.R:173; C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_champions.R:341,460` | Valid as the Cook input for the 2024 cycle only, and only for analysis windows after 2024-09-12. The #REF! cell should be handled before ingestion. Whether this table is the source the study actually used is not established. | moderate | VERIFIED-SOFTENED |
| [[systems/mentat-sector-growth-study#SEATS-W weekly seat-gain test (2022 House)|SEATS-W weekly seat-gain test (2022 House)]] | measurement-source | A single dated (2024-09-12) Cook rating per House race, which could serve as one rating observation in a 2024 extension. (Header line 3; race rows) | SEATS-W uses 538 probability classes, which it states are never Cook ratings. Expert ratings form a separate categorical path that admits only rows dated strictly before the window. Unrated races count as unknown, never safe. `C:/Users/pgoff/Mentat/sector_growth_study/code/R/sgs_seats_race.R:14-15,27-30,37` | The live test covers the 2022 House only, so this 2024 snapshot applies only if the test is extended to 2024. Even then it gives one date per race, not the weekly series the test needs. | weak | VERIFIED |

> [!failure]- Claims that did not survive verification
> - Summary issue: '25 Toss Ups' is wrong: the row-level parse gives 24. The 25th substring hit is the header sentence 'Competitive races (Lean and Toss Up) are highlighted.'
> - Summary issue: 'the rating totals come to 436 against 435 seats, so the table has a small error' is wrong: per-row ratings sum to 435 (174 Solid D, 192 Solid R, 16 Likely D, 11 Likely R, 13 Lean D, 5 Lean R, 24 Toss Up). The 436 comes from the analyst's substring method, not the table.
> - Summary issue: EVEN seats carry five ratings, not the four implied: PA-17 (EVEN, Lean D) is omitted.
> - Summary issue: AZ-04 (D+2, Solid D) vs OR-05 (D+2, Toss Up) is a valid raw-PVI contrast, but under the system's party-oriented lean OR-05's Republican incumbent has lean -2 (c = 2.0) vs Stanton's 1.67, so c does not treat them alike; the summary's broader point (EVEN incumbents Salazar and Bacon both get c = 2.0) holds only as an illustration on 2024 PVIs, since the live system uses Cook 2026 PVIs.
> - Summary issue: Related link to broockman-2026-should-moving-middle dropped: the paper concerns returns to ideological moderation and has no Cook PVI or rating content; the connection to this table is not real.

## Gaps and flags

- Competitiveness has no candidate-specific signal on the engine path. With COMPET_MULT on, c is a function of PVI alone and the candidate-specific Ballotpedia term is turned off, so EVEN-PVI incumbents get the maximum c (2.0) whatever their Cook rating, for example Salazar (Solid R) and Bacon (Toss Up).
- No ingested expert-rating input exists for the 119th or 2026 cycle in Melange; the forward-looking risk category is absent from electoral plays.
- Open-seat and vacancy status from the rating sheet does not reach c. Retirees are handled only through Paul's ballot-district overrides.
- Reciprocity is NOT validated (D-104..D-108). This table rates electoral risk only and must not be cited as evidence that dollars buy more in competitive seats.
- Source data quirk: MA-01 has a #REF! cell (line 1120). Handle it before ingesting any row.
- The c values computed above (for example about 1.67 for Stanton at D+2) are verifier arithmetic on the 2024 PVIs, not the live 2026 artifact.
- Every figure is for the 2024 cycle as of 2024-09-12, over 435 districts; later rating changes are not reflected.

Paul supports Cook PVI as a vulnerability measure (through Gaynor) and reads competitive seats as responsive. This table, from Cook itself, partly undercuts PVI-only vulnerability: EVEN seats range from Solid R to Toss Up, and D+5 seats can be Toss Ups. So the PVI-only c is a coarser signal than Paul's reading implies. It is consistent with his position note that no source he annotated shows dollars buying more in competitive seats, and this table adds nothing on that point. The annotations supplied with this item (a DRL-IM frontier note and two IBIS forecast memos) do not concern this table, so there is no Paul takeaway to compare it with and none is stale. The system has moved to Cook's 2026 PVI, so the 2024 snapshot is a historical, structural check rather than a current input.

## Related papers

- [[gaynor-2022-financial-ties-bind]] — complements — Gaynor uses Cook PVI as an electoral-vulnerability measure, and Paul cites that use to ground c. This table shows Cook's own ratings departing from PVI.
- [[bonica-2013-mapping-ideological-marketplace]] — complements — Bonica finds that donors direct funds to competitive races. Cook's Lean and Toss Up categories are the standard way to mark which races those are.
- [[fouirnaies-2022-electoral-incentives-affect]] — complements — The receptivity layer's code labels its candidate-specific competitiveness term 'Fouirnaies-Hall' (receptivity_layer.py:128-136); COMPET_MULT neutralizes that term. The paper itself studies term limits in state legislatures, not Cook ratings.
- [[linde-2016-politicians-take-risks]] — complements — The receptivity layer's Linde-Vis pt_mult (loss-domain) term is the factor a Toss Up or Lean rating would mark; the paper is a prospect-theory experiment on MPs and does not use race ratings.

### Linked from
- [[best-2022-redistricting-commissions-avoid]] — complements — Cook's district partisanship ratings are the type of measure the competitiveness multiplier consumes. Best et al. show that such district partisanship shifts by 1.5-4 points depending on how the map is drawn.
- [[becker-nd-gameability-redistricting-criteria]] — complements — A 2024 Cook Political Report ratings piece in the same input family as the Cook district PVI that drives COMPET_MULT. The link is conceptual only: Becker and Gold measures say how far a district moved, which governs whether a PVI value carries over. Report content was not checked here.