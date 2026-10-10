---
type: paper
title: "A causal approach for detecting team-level momentum in NBA games"
authors: ["Louis Weimer", "Zachary C. Steinert-Threlkeld", "Kevin Coltin"]
year: "2023"
venue: "Journal of Sports Analytics 9 (2023) 117–132, IOS Press (pre-press 27 June 2023; published 3 July 2023; received 19 Aug 2021; accepted 9 Dec 2022)"
doi: "10.3233/JSA-220592"
category: "Causal & Methodological"
systems: ["melange-influence-engine", "melange-validation"]
components: ["melange-validation.prospective-contract", "melange-validation.single-look-discipline", "melange-influence-engine.scenario-runner", "melange-validation.sensitivity-and-mc"]
paul_annotated: true
paul_markup_count: 57
evidence_status: "verified"
source: "[[_sources/weimer-2023-causal-approach-detecting.pdf]]"
original_paths: ["C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705-013300\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\A_causal_approach_for_detecting_team-level_momentu.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20260705\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\A_causal_approach_for_detecting_team-level_momentu.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009-191315\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\A_causal_approach_for_detecting_team-level_momentu.pdf", "C:\\Users\\pgoff\\Documents\\research\\_raw\\bib-20261009\\Annotated Bibliographies & Literature Synthesis\\Adjacent\\A_causal_approach_for_detecting_team-level_momentu.pdf"]
sha12: ["3cce7a4c1fc9"]
tags: ["paper", "causal-methodological"]
---

# A causal approach for detecting team-level momentum in NBA games

**Louis Weimer, Zachary C. Steinert-Threlkeld, Kevin Coltin (2023).** *Journal of Sports Analytics 9 (2023) 117–132, IOS Press (pre-press 27 June 2023; published 3 July 2023; received 19 Aug 2021; accepted 9 Dec 2022).* [10.3233/JSA-220592](https://doi.org/10.3233/JSA-220592). Theme: [[themes/causal-methodological|Causal & Methodological]].

> [!abstract] Summary
> Weimer, Steinert-Threlkeld and Coltin test whether team-level scoring momentum exists in the NBA. Their identification idea is the transferable part. The authors argue that coach-called timeouts are a response to momentum, so comparing scoring before and after them is biased. TV timeouts fall at points the rulebook fixes, so they act as near-random pauses. The authors take scoring runs of 6 or more unanswered points from 13 seasons (2004-05 to 2016-17) and match runs interrupted by a TV timeout 1:1 on propensity scores to runs with no timeout. In 5,148 matched runs, a TV timeout cuts the running team's points in the next 3 minutes by 11.2% (b = -0.119, about 0.56 points). The effect is about the same for runs of 10 or more (11.57%, n = 576) and larger only for runs of 15 or more (30.79%, n = 48, which the authors say to read cautiously). It is about 41% smaller when neither team substitutes (n = 4,644) and fades after 3 minutes. Coaches' timeouts (Appendix E) give largely similar, slightly larger effects, not significantly different from TV timeouts, and the authors concede TV timeouts may not be fully exogenous. A Monte Carlo placebo with zero true momentum returns a null coefficient (b = 0.002, n = 1,336), so the result is not produced by how runs were selected.
> 
> None of the basketball findings transfer to Melange, Mentat or Terrier. The method lessons do. First, a treatment someone chooses (a lobbyist's contact, a PAC's spend) is endogenous in the same way a coach's timeout is, so naive before/after or contact-conditioned correlations are biased. Second, a simulated null is a cheap way to show that an estimator or a selection rule does not manufacture effects. The system's prospective receptivity contract already limits its claims to forecasting. The contact-conditioned arm it plans shares the problem of selection on a chosen treatment, though it is a subgroup Spearman among contacted members rather than a before/after-contact comparison. A repo search of fec/engine for placebo, negative control and null simulation found none (the claim is scoped to fec/engine; negative controls exist in fec/tests/run_tests.py as code-test controls, not estimator placebos).

## Paul's annotation

From [[annotations/Adjacent - Adjacent - Annotated Bibliography|Adjacent - Adjacent - Annotated Bibliography]]:

> Weimer, Louis, et al. “A causal approach for detecting team-level momentum in NBA games.” *Journal of Sports Analytics*, vol. 9, no. 2, 3 July 2023, pp. 117–132, [https://doi.org/10.3233/jsa-220592](https://doi.org/10.3233/jsa-220592).

## Paul's markup in the PDF

- pPDF 1 (journal 117) Highlight (yellow): "Deloitte Consulting LLP," — 
- pPDF 1 (journal 117) Highlight (yellow): "team-level momentum" — 
- pPDF 1 (journal 117) Highlight (yellow): "natural experiment in the flow of basketball game" — 
- pPDF 1 (journal 117) Highlight (yellow): "(TV) timeouts" — 
- pPDF 1 (journal 117) Highlight (yellow): "at points exogenous to momentum," — 
- pPDF 1 (journal 117) Highlight (green): "TV timeouts cause an 11.2% decline in the number of points" — 
- pPDF 1 (journal 117) Highlight (yellow): "emphasize the importance of using timeouts to halt opponents’ momentum" — 
- pPDF 1 (journal 117) Highlight (yellow): "Perhaps the most popular way to conceptualize momentum is the “hot hand” theory," — 
- pPDF 2 (journal 118) Highlight (yellow): "Comparing points before and after a timeout is fallacious, however, since a coach often calls a timeout explicitly to stop momentum." — 
- pPDF 2 (journal 118) Highlight (yellow): "TV timeouts, also referred to as mandatory timeouts or official timeouts, are timeouts that occur at predetermined times, independent of the score or flow of the game" — 
- pPDF 2 (journal 118) Highlight (yellow): "Exploiting the uncorrelated nature of TV timeouts, we find that momentumed teams score 0.56 fewer points on average during the 3 minutes after a TV timeout as compared to matched teams that do not experience a timeout." — 
- pPDF 2 (journal 118) Highlight (yellow): "an NBA player who just made a string of shots would be more likely to make his next shot than his long-term shooting average would suggest." — 
- pPDF 3 (journal 119) Highlight (yellow): "As a result, academic consensus has shifted, and “the existence of the hot hand remains highly disputed”" — 
- pPDF 3 (journal 119) Highlight (yellow): "the extent and size of effect remains controversial." — 
- pPDF 3 (journal 119) Highlight (yellow): "Since timeouts interrupt the flow of the game, previous studies have used them as a mechanism to attempt to observe the existence of momentum." — 
- pPDF 4 (journal 120) Highlight (yellow): "Both studies failed to find a momentum effect:" — 
- pPDF 4 (journal 120) Highlight (yellow): "causal inference research design combined with using TV timeouts as an exogenous source of breaks in momentum." — 
- pPDF 4 (journal 120) Highlight (yellow): "They find that pauses in play benefit players by providing a chance to psychologically recover from negative momentum, and the benefit holds even when no coaching instructions were given during the timeout." — 
- pPDF 5 (journal 121) Highlight (yellow): ", if timeouts were ineffective at slowing a momentumed team on a scoring run, then exogenous pauses unrelated to the game situation should have no measurable effect on subsequent scoring." — 
- pPDF 5 (journal 121) Highlight (yellow): "momentum." — 
- pPDF 5 (journal 121) Highlight (yellow): "The researcher randomly issues a timeout during" — 
- pPDF 5 (journal 121) Highlight (yellow): "The paper uses TV timeouts because they occur at pre-specified moments in the game." — 
- pPDF 5 (journal 121) Highlight (green): "Consider Table 1’s invocation of the ideal experiment to determine the existence of momentum" — 
- pPDF 5 (journal 121) Highlight (yellow): "may differ from those that did not select into that group." — 
- pPDF 5 (journal 121) Highlight (yellow): "the assignment is random." — 
- pPDF 5 (journal 121) Highlight (yellow): "By comparison, timeouts called by coaches are very much a function of momentum," — 
- pPDF 5 (journal 121) Highlight (yellow): "Key to matching in this context is that substitutions can occur during dead balls, not just in timeouts." — 
- pPDF 6 (journal 122) Highlight (yellow): "if a TV timeout occurs and one team has scored at least six unanswered points immediately prior to the timeout, the TV timeout interrupts momentum." — 
- pPDF 6 (journal 122) Highlight (yellow): "out interrupts momentum." — 
- pPDF 6 (journal 122) Highlight (yellow): "One is to simply examine whether a team on an unanswered scoring streak had that streak snapped." — 
- pPDF 7 (journal 123) Highlight (yellow): "This approach uses all matching variables as independent variables in a logistic regression with the treatment dummy as the dependent variable." — 
- pPDF 7 (journal 123) Highlight (yellow): "We then discard all unmatched observations so that the data consists of pairs of matched runs, half of which are interrupted by a TV timeout and half of which are not interrupted by any type of timeout." — 
- pPDF 7 (journal 123) Highlight (yellow): "Table A1 shows a covariate balance test between the treated and control groups; no statistically significant difference between the two groups exists." — 
- pPDF 7 (journal 123) Highlight (yellow): "the number of unanswered points a team has scored." — 
- pPDF 7 (journal 123) Highlight (yellow): "This can range from zero to five players substitute" — 
- pPDF 7 (journal 123) Highlight (yellow): "Substitutions by opposing team:" — 
- pPDF 7 (journal 123) Highlight (yellow): "dependent variable is the number of points scored by the momentumed team during the three minutes of game time subsequent to the TV timeout" — 
- pPDF 7 (journal 123) Highlight (yellow): "We model the outcome using Poisson regression, consistent with standard approaches for count data." — 
- pPDF 9 (journal 125) Highlight (yellow): "To decompose these effects" — 
- pPDF 11 (journal 127) Highlight (yellow): "the opposing team’s part, then the number of substitutions should increase as run length increases" — 
- pPDF 11 (journal 127) Highlight (yellow): "there is no relationship between run length and the number of substitutions by either team, and a regression (not shown) confirms that the slope of this relationship is not distinguishable from zero." — 
- pPDF 11 (journal 127) Highlight (yellow): "equivalent to approximately an entire NBA season, including all regular season and playoff games" — 
- pPDF 11 (journal 127) Highlight (yellow): "Table 5 and Fig. 6 show that the Monte Carlo simulation does not produce a momentum effect." — 
- pPDF 11 (journal 127) Highlight (yellow): "they do not differ from points scored during an uninterrupted run." — 
- pPDF 12 (journal 128) Highlight (yellow): "In addition, play during garbage time is likely to be substantively different than play outside of it, as coaches use players deep on the bench and are likely to care less about strategy." — 
- pPDF 12 (journal 128) Highlight (yellow): "Finally, teams should be more likely to modify strategy during “crunch time,”" — 
- pPDF 13 (journal 129) Highlight (yellow): "However, the point estimate of the coefficient steadily decreases as more post-treatment time is included: the momentum dampening effect of TV timeouts decreases as gameplay progresses after them." — 
- pPDF 13 (journal 129) Highlight (yellow): "there is no significant relationship between the TV timeout and points scored between the next three and four minutes." — 
- pPDF 13 (journal 129) Highlight (yellow): "weakening the treatment effect." — 
- pPDF 13 (journal 129) Highlight (yellow): "Similarly, we rerun the analysis by matching runs interrupted by any timeout with uninterrupted runs." — 
- pPDF 13 (journal 129) Highlight (yellow): "Future research could measure non-substitution strategic changes with notational analysis data." — 
- pPDF 14 (journal 130) Highlight (yellow): "the Monte Carlo simulation serves as a negative control which demonstrates that the effect is not a spurious artifact of selecting runs." — 
- pPDF 14 (journal 130) Highlight (yellow): "If coaches can affect momentum by calling timeouts, then they should be more liberal with their use of them," — 
- pPDF 14 (journal 130) Highlight (yellow): "It may not be possible to directly attribute a win to the usage of timeouts" — 
- pPDF 14 (journal 130) Highlight (yellow): "should not substitute players during a TV timeout since doing so causes a greater decrease in subsequent points scored than the timeout or opponent’s substitutions." — 
- pPDF 14 (journal 130) Highlight (yellow): "The original hot hand research met with skepticism from coaches (Kahneman, 2011)," — 
- pPDF 14 (journal 130) Highlight (yellow): "But many questions regarding strategy are fundamentally causal in nature, such as the effect of a timeout on an NFL kicker’s chance of making a field goal or the effect of relieving a starting pitcher after a given pitch count…" — 

## Study

**Question:** Does team-level, in-game momentum exist in NBA basketball, and can a pause in play interrupt it? The test is whether a TV timeout (mandatory, scheduled independently of the game) during a scoring run reduces the momentumed team's subsequent scoring, compared with matched runs that are not interrupted (abstract p.117; Sec. 1 p.118-119).

**Theory:** Momentum is defined as team-level serial correlation in scoring: after unanswered points, a team scores more than its baseline would predict (Sec. 1 p.118; Sec. 2 p.119). The paper contrasts this with the hot-hand literature (Gilovich, Vallone and Tversky 1985, and later critiques by Miller and Sanjuro 2018a, Bocskocsky et al. 2014, Csapo et al. 2015). Coaches call timeouts when they expect them to stop a run, so coach-called timeouts are endogenous and naive before/after comparisons are biased (Sec. 2 p.120; Sec. 3 p.121). If timeouts stop momentum, then exogenous pauses should not affect scoring only if timeouts are ineffective, which gives a testable prediction (Sec. 3 p.120). The paper also cites Den Hartigh and Gernigon (2018) that pauses let players recover from negative momentum even without coaching.

**Data:** Play-by-play scoring-play rows from BigDataBall (2020) for 13 NBA seasons, 2004-05 to 2016-17, including playoffs (Sec. 4 p.122). Each row is one scoring play and records lineups, points, time remaining, a following-timeout flag, and whether that timeout was a TV timeout. Unit of analysis: a scoring run (treated = run interrupted by a TV timeout; control = run with no timeout), matched 1:1 to its nearest untreated neighbour on propensity scores from logistic regression (MatchIt; unmatched observations discarded; matched pairs can come from different games and teams) (Sec. 4 p.122-123). Sample sizes in Table 3 (p.125): 5,148 matched runs (run >= 6), 576 (run >= 10), 48 (run >= 15). Table 4 (no substitutions by either team): 4,644. Monte Carlo (Table 5): 130,000 simulated scoring plays, regression n = 1,336. Full-season N and number of games are not stated in the extracted text. Fig. 1 notes 11.85% of runs reach six or more unanswered points (p.123).

**Method:** Matched observational design with a natural experiment. Propensity-score matching (logistic regression, nearest neighbour; Rosenbaum and Rubin 1983; Ho et al. 2011, MatchIt), with Mahalanobis matching as a robustness check (Table A2, not in text). Outcome: points scored by the momentumed team in the three minutes of game time after the TV timeout (or the run's next play). Estimator: Poisson regression (count outcome, Sec. 4 p.124; Table 3 p.125), with covariates run size and substitutions by each team. Robustness: negative binomial (Appendix B, reported as substantively identical), alternative windows from 60 to 240 seconds (Appendix D), coaches' timeouts as treatment (Appendix E), and a Monte Carlo placebo with zero true effect (Table 5, Fig. 6, p.127-128). Model comparison by bootstrapped AIC, 1,000 resamples of size 48 or 576 (Fig. 3, p.124-126).

**Identification:** Treatment is a TV timeout occurring during a run. The paper treats its timing as random assignment: TV timeouts are mandatory, fixed by rulebook procedure, and cannot be self-selected by teams (Sec. 3 p.121). Key assumption, stated by the authors as the key to the paper: TV timeouts are exogenous to momentum and to strategy (Sec. 3 p.120; Sec. 5.3 p.129). Propensity-score matching balances observables (Table A1, no significant imbalance, not in text). Substitutions by each team are a proxy for strategic change during the stoppage, and matching on them isolates the stoppage effect (Sec. 3 p.122). Substitutions can happen at dead balls, not only timeouts, which the matching design relies on. Identification also relies on the Monte Carlo placebo (null result, Table 5) and on subset tests (no substitutions; garbage time excluded; crunch time excluded; first TV timeouts only).</identification>
<parameter name="measures">["Momentum indicator m = 1 if the team scored at least 6 unanswered points immediately before the TV timeout (Eq. 1, p.122); alternative thresholds 10 and 15", "Run size: unanswered points by the momentumed team", "Substitutions by momentumed team and by opposing team (0 to 5 players), counted during the TV timeout for treated runs and at the dead ball after the basket for controls (p.123-124)", "Dependent variable: points scored by the momentumed team in the next 3 minutes of game time (alternative windows 60 s to 4 min)", "Model fit: AIC and log-likelihood, bootstrapped for comparability across sample sizes"]

**Scope:** Population: NBA regular-season and playoff games, 2004-05 to 2016-17 (13 seasons), team-level momentum within a game. Institutional setting: NBA rulebook TV-timeout procedure (first stoppage after the 9:00 mark of the second quarter unless a timeout was already called in that quarter; 100-second TV timeouts; coaches' timeouts 60 or 100 seconds) (Sec. 1 p.118). Era: pre-2017 NBA. The paper argues the question matters for all sports (Sec. 6 p.131) but tests basketball only. No seasons after 2016-17 and no notational or player-tracking data were used.

## Key findings

- A TV timeout during a run lowers the momentumed team's points in the next 3 minutes. Coefficient -0.119 (SE 0.012), p < 0.01, so e^b - 1 = -11.2%. Average gap about 0.56 points. (b = -0.119 (0.012); -11.2%; 0.56 points fewer; matched n = 5,148; Abstract p.117; Sec. 5.1 p.124; Table 3 col. 1 p.125; Conclusion p.130)
- Opposing-team substitutions also reduce the momentumed team's scoring, by less than the TV timeout does (about half as strong, per text). (b = -0.077 (0.017), e^b - 1 = -7.4%; text says TV effect is about 50% larger; Sec. 5.1 p.124; Table 3 p.125)
- Substitutions by the momentumed team have the largest effect in the 6-point model. The authors read this as breaking up the momentumed lineup. (b = -0.176 (0.028), about -16.1% (text states 16%); Sec. 5.1 p.124)
- The TV timeout effect is about the same for runs of 10 or more as for 6 or more, and larger only at 15 or more (n = 48, read cautiously). Opposing-team substitutions lose significance at longer runs. (run >= 10: b = -0.123 (0.036), text says 11.57% decrease, n = 576; run >= 15: b = -0.368 (0.129), text says 30.79%, n = 48; Sec. 5.1 p.124-125; Table 3 p.125)
- No mean reversion: the current-run-size coefficient is not significant in any model. (Text: .001. Table 3 shows -0.002 (0.003), -0.003 (0.009), -0.018 (0.031). The two disagree; see limitations.; Sec. 5.1 p.124; Table 3 p.125)
- The effect is largest right after play resumes. It is about four times as large in the first minute as in the 3-minute window. The 0.56-point figure is therefore a lower bound. (first-minute effect roughly 4x the 3-minute effect (text); Sec. 5.1 p.124)
- Model fit favours the 6- and 10-point run definitions over 15-point runs. The 6-point model fits as well as the 10-point model. (bootstrapped AIC over 1,000 resamples (n = 48 and 576); Sec. 5.1 p.124-125; Fig. 3 p.126)
- Effect is consistent across run sizes up to 17 points, so it is not an artefact of a particular threshold. (TV timeouts reduce scoring for runs of 17 points or fewer; Sec. 5.1 p.125; Fig. 4 p.126)
- TV timeouts with no substitutions by either team still reduce scoring, by about 41% less than timeouts with substitutions. The authors attribute about 59% of the total effect to stoppage of play and the rest to strategy change. (b = -0.068 (0.013), -6.57%, n = 4,644; about 41% smaller than Table 3 effect; Sec. 5.1 p.125; Table 4 p.125)
- Substitution counts do not rise with run length, which argues against substitutions being a strategic response to runs. (slope not distinguishable from zero (regression not shown in text); Sec. 5.1 p.126-127; Fig. 5 p.127)
- Monte Carlo placebo with zero true momentum (130,000 plays, one TV timeout per 50 plays): the TV timeout coefficient is null. (b = 0.002 (0.022), n = 1,336; Sec. 5.2 p.127-128; Table 5 p.128; Fig. 6 p.128)
- The TV timeout coefficient shrinks as the outcome window lengthens. There is no significant effect in the 3-to-4-minute window. (effect weakens with longer window; no significant effect between minutes 3 and 4; Sec. 5.2 p.129)
- Robustness: garbage-time exclusion, crunch-time exclusion, first TV timeouts only, and negative binomial all leave results substantively unchanged. Coaches' timeouts give a slightly larger effect, and the difference from TV timeouts is not significant. (no numbers given in main text for these subsets; Sec. 5.2 p.128-129; Appendices B, C, E (not in text))

## Limitations

- Authors: TV timeouts may not be exogenous to gameplay change. Strategy changes other than substitutions (defensive or pick-and-roll assignments, speeches) are not measured (Sec. 5.3 p.129).
- Authors: no readily available notational or tracking data, so non-substitution strategy changes are not tested directly (p.129).
- Authors: estimates for runs of 15 or more points rest on n = 48; the 30.79% estimate should be read cautiously (p.125).
- Authors: matching on more covariates would shrink the already small samples (5,148 / 576 / 48) (p.129).
- Authors concede strategic changes probably do occur during TV timeouts; the checks suggest they do not explain the results but do not rule them out (p.129-130).
- Authors: the TV timeout effect and the coaches'-timeout effect are both present, so TV timeouts contain elements of stoppage and strategy (p.129).
- Reader observation, not stated by the authors: the text reports the mean-reversion coefficient as .001, but Table 3 reports -0.002, -0.003 and -0.018 for the three run thresholds. The text and table disagree.
- Reader observation: the text says even one unused timeout adds points to the opponent's score (p.130), but no supporting numbers appear in the main text; the claim may depend on the unseen appendices.
- Reader observation: the paper recommends that coaches of momentumed teams avoid substitutions during TV timeouts (p.130). That conclusion treats the substitution count, a strategy proxy the authors did not randomise, as a causal lever.
- Reader observation: the 6-unanswered-point threshold was chosen a priori; only 11.85% of runs meet it (Fig. 1 p.123). Sensitivity to threshold is shown only at 10 and 15, and the 15-point models have n = 48.
- Reader observation: the appendices (A-E) were not in the extracted text, so balance tests, Mahalanobis and negative binomial results are reported only as the main text describes them.

## Short quotes

- "TV timeouts are mandatory and therefore do not permit teams to self-select into treatment" (Sec. 3, p.121 (journal numbering))

## Bearing on the systems

| Component | Relation | Paper shows | System does | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[systems/melange-validation#Prospective receptivity contract (H1-H2) and forecast ledger|Prospective receptivity contract (H1-H2) and forecast ledger]] | qualifies | The authors argue that comparing points before and after a coach-called timeout is fallacious because coaches often call timeouts to stop momentum. They use TV timeouts, which fall at predetermined times, and match runs interrupted by a TV timeout to uninterrupted runs on propensity scores (pairs; unmatched runs dropped). Result: -11.2% points over the next 3 minutes (b = -0.119, SE 0.012), n = 5,148 matched runs. Caveat from the same paper: Appendix E (as described in the main text) finds coaches' timeouts give largely similar, slightly larger effects, and the TV-vs-coach coefficient difference was not significant; Sec. 5.3 concedes TV timeouts may not be fully exogenous. (Sec. 1 p.118 (text lines 81-86); Sec. 2 p.119-120; Sec. 4 p.122-123 (propensity matching); Table 3 p.125; Sec. 5.2 p.129 (Appendix E summary); Sec. 5.3) | H2 scores frozen pressure_mult against \|overall-tier movement\| with an unadjusted cross-sectional Spearman. PROTOCOL_TEXT plans an engaged-only run among members the In-contact log marks engaged, but states it is unavailable until separately specified, receipted contact data exists; the result field is engaged_only = NOT_IMPLEMENTED_NO_CONTACT_CONTRACT. CLAIM_SCOPE = receptivity_forecast_only_not_spending_effects. `fec/engine/outcome_contract.py:17-22 (PROTOCOL_TEXT, CLAIM_SCOPE); fec/engine/outcome_contract.py:278 (h2 Spearman); fec/engine/outcome_contract.py:282 (engaged_only NOT_IMPLEMENTED); fec/members/receptivity_sensitivity.py:14-16` | Design logic only, not effect size. Lobbyist-chosen contact is endogenous like a coach's timeout, so an engaged-only rho would confound pressure_mult with targeting unless contact timing is exogenous or matched on targeting covariates. The engaged-only arm is a subgroup correlation, not a before/after-contact comparison, so the analogy is to the selection problem, not an identical design. The paper's own Appendix E finding (coach vs TV timeouts not significantly different) means it does not empirically show the endogenous design is badly biased in its setting. Single observational study with matching. | moderate | VERIFIED-SOFTENED |
| [[systems/melange-validation#Single-look holdout discipline and look ledger|Single-look holdout discipline and look ledger]] | supports | A Monte Carlo with zero true momentum (130,000 independent 50/50 scoring plays, a TV timeout after one play in every 50, substitutions not simulated) yields a null treatment coefficient (b = 0.002, SE 0.022, n = 1,336). The Fig. 6 caption calls it a negative control showing the matched design does not find an effect where none exists. (Sec. 5.2 p.127-128; Table 5 p.128; Fig. 6 caption) | The discipline already used a simulated null to calibrate its protocol: a joint-null false-pass simulation (pass-both-panels about 1.5-2%, dual dominance under 1%) supported exactly one more look per trait. `C:/Users/pgoff/Documents/brain/concepts/single-look-holdout-discipline.md:56` | Supports the general practice of simulating under a known null to check that a selection or gating rule cannot manufacture a pass. Says nothing about the 1.5-2% figure or pre-registration; the brain simulation concerns look false-pass rates, not engine outcome estimators. | weak | VERIFIED |
| [[systems/melange-influence-engine#Scenario runner (what-if)|Scenario runner (what-if)]] | qualifies | The paper's Monte Carlo is a negative control for an estimator applied to observed data: simulate data with no effect, re-run the matching estimator, expect a null. (Sec. 5.2 p.127-128; Table 5; Fig. 6) | scenario.py uses Monte Carlo as a forward model: baseline vs scenario competitive-cascade spread under edge-keyed common random numbers (SEED=7, MC default 40), so an unperturbed scenario gives a zero delta by construction. That is an identity property of the simulator, not a negative control on an inferred effect. `fec/engine/scenario.py:27 (SEED=7, MC default 40); fec/engine/scenario.py:41-55 (CRN cascade and spread); fec/engine/scenario.py:102-106 (baseline vs scenario spread deltas)` | A Weimer-style placebo applies to empirical estimators that read observed outcomes (forecasters, the contract), not to the forward cascade's deltas, which are model outputs. | weak | VERIFIED |
| [[systems/melange-validation#Receptivity sensitivity harness, MC stability and F5 power gate|Receptivity sensitivity harness, MC stability and F5 power gate]] | suggests-extension | Appendix D (described in the main text) re-tallies post-treatment points over windows from 60 s to 4 minutes in 10 s steps; the negative effect persists but the point estimate steadily decreases as the window lengthens, and there is no significant effect for points scored between minutes 3 and 4. The first-minute effect is about four times the 3-minute effect. (Sec. 5.1 p.124 (four times as large); Sec. 5.2 p.129 (Appendix D summary)) | mc_stability_check.py reruns one topic's budget frontier at MC=20/100 and SEED=7/1/13 and reports Jaccard set overlap; receptivity_sensitivity.py sweeps receptivity constants and labels ROBUST when top-20 overlap >= 18. The outcome contract has no window sweep: the window is whatever elapses between freeze and score, with a 60-day minimum and 150 movers before it stops returning UNDERPOWERED. `fec/engine/mc_stability_check.py:15-18 (MC/SEED runs); fec/members/receptivity_sensitivity.py:113 (ROBUST if o20 >= 18); fec/engine/outcome_contract.py:262 (days<60 or movers<150 => UNDERPOWERED)` | Suggests pre-registering a small set of outcome windows inside the contract lock rather than choosing one afterward (basis-shopping). Minute-scale decay says nothing about congressional time scales. | weak | VERIFIED-SOFTENED |

> [!failure]- Claims that did not survive verification
> - Summary issue: 'The effect is larger for longer runs (n = 576 and 48)' overstates: for runs >= 10 (n = 576) the effect is 11.57% vs 11.2% for runs >= 6, essentially the same; only runs >= 15 (n = 48) show a larger effect (30.79%), which the authors say to interpret cautiously given the small n.
> - Summary issue: The summary omits that Appendix E (per main text p.129) finds coaches' timeouts give largely similar, slightly larger effects and the TV-vs-coach difference was not significant, and that Sec. 5.3 concedes TV timeouts may not be fully exogenous. 'Coach timeouts bias the comparison' is the authors' design argument, not a bias they demonstrated in their data.
> - Summary issue: 'The contact-conditioned arm it plans is exactly the coach-timeout design' overstates: the engaged-only arm is a subgroup Spearman among contacted members, not a before/after-contact comparison, and PROTOCOL_TEXT already says it is unavailable until separately specified contact data exists. The shared issue is selection on a chosen treatment, not an identical design.
> - Summary issue: Minor: '1:1 on propensity scores' is a fair paraphrase (each treated run matched to a maximally similar control, pairs kept), but the paper does not use the term 1:1.
> - Summary issue: The 'no negative control in fec/engine' claim holds for fec/engine (no placebo / negative control / null simulation hits); negative controls exist in fec/tests/run_tests.py but are code-test controls, not estimator placebos. Keep the claim scoped to fec/engine and UNVERIFIED under DE-062.

## Gaps and flags

- No negative control or placebo exists for any empirical estimator in fec/engine. A repo search of fec/engine for placebo, negative control and null simulation found none. A simulated-null run of the cosponsor forecaster or the H1/H2 scorer (shuffled predictors, or a synthetic panel with zero effect) would show whether selection or roster filtering can produce a rho of +.20 by itself.
- The planned engaged-only H2 arm (outcome_contract.py:17-21) conditions on lobbyist-chosen contact, which is endogenous like a coach's timeout. No exogenous source of contact timing and no matching design is specified. The arm is NOT_IMPLEMENTED (line 283), so this can be fixed before any contact contract is written.
- The contract's H1/H2 Spearman is unadjusted: no party or chamber strata and no matched comparison. Any later attempt to read it as more than a forecast would need the matching or exogenous-assignment structure the paper models.
- The outcome window is a single choice (at least 60 days) with no pre-registered window sweep. The paper shows estimates can change substantially with window length.
- Category conflict: one of Paul's bibliography files puts this paper in the folder 'Adjacent', but the component positions place it in the Causal & Methodological folder. I categorized it Causal & Methodological because only its method transfers. Paul may want to settle which folder it belongs in.
- Paul's highlight of the closing recommendation (coaches of momentumed teams should not substitute during TV timeouts) rests on substitution counts that were not randomized. It is a causal claim on a non-randomized proxy and should not be used as a model for prescriptive rules in Melange.
- Text-table inconsistency in the source: the text gives the mean-reversion coefficient as .001, while Table 3 shows -0.002, -0.003 and -0.018. Quote Table 3 if this paper is cited at all.
- No conflict with D-021, D-025, D-104..D-108, the two rejected SNA proposals, or the barred influence language. The paper makes no network, LDA or reciprocity claim.

Paul's bibliography entries for this paper contain only the citation, with no takeaways. His 57 highlights mark the identification argument (endogenous coach timeouts versus exogenous TV timeouts), propensity matching, and the Monte Carlo negative control. The component positions tie it to competitive-cascade-crn, scenario-runner and single-look-discipline as a Monte Carlo tool or negative control. That reading is partly off target. The engine's Monte Carlo (CRN cascades in scenario.py and budget_frontier.py) is a forward simulator, and a null scenario returns zero by construction, so Weimer's placebo does not validate it. What the paper validates is putting a simulated null on empirical estimators, and the system does this only for look false-pass rates (brain note line 56). The sharper lesson in his highlights, that a self-selected intervention biases before/after comparisons, bears most on the engaged-only H2 arm of the prospective contract. His notes do not mention that arm. I found no bearing on competitive-cascade-crn.

## Related papers

- [[imai-2021-fixed-effects-regression]] — complements — Imai & Kim recast two-way fixed effects as a matching estimator and show its causal limits; Weimer uses matching on a natural experiment. Both bear on reading observational looks causally.
- [[blackwell-2026-effect-political-advertising]] — complements — Blackwell & Yamauchi adjust for unmeasured confounding of strategically placed interest-group ads; Weimer concedes unmeasured strategy changes during timeouts as a limitation. Same strategic-treatment confounding problem.
- [[kalla-nd-campaign-contributions-facilitate]] — complements — Kalla & Broockman randomize which congressional offices receive a donor-identified meeting request: the randomized ideal of Weimer's Table 1, in the political-contact domain.
- [[harrison-2010-introduction-monte-carlo]] — complements — Harrison is a general Monte Carlo tutorial that runs no simulation itself; Weimer applies Monte Carlo as a negative control. Background reference, not the same method application (relation changed from same-method).

### Linked from
_None._