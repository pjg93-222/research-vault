---
type: system
name: "Melange stance and tone measurement (press and social text)"
slug: melange-stance
components: [melange-stance.debate-nli-classifier, melange-stance.supervised-stance-stack, melange-stance.lsd-tone, melange-stance.press-value-aggregate, melange-stance.two-hop-decomposition, melange-stance.bill-sign-handling, melange-stance.cfpb-axis-isolation, melange-stance.media-blend, melange-stance.legacy-regex-scorer, melange-stance.acu-speaker-mode, melange-stance.stance-dynamics, melange-stance.frame-alignment]
brain_notes: ["actors/debate-stance-model", "concepts/two-hop-stance-decomposition", "topics/2026-06-feature-cu-stance-and-sentiment", "topics/2026-06-feature-cu-stance-and-sentiment-stance-stack-design-and-accuracy", "topics/2026-07-feature-influence-engine-internals-salience-sna-wave"]
paper_count: 30
tags: [system]
---

# Melange stance and tone measurement (press and social text)

Turns member press releases, tweets and other posts into signed CU stance and tone. Covers the Political DEBATE NLI stance reader, Lexicoder tone, two-hop stance x valence composition, bill-sign handling, CFPB axis isolation, human-label overlay, the media blend with recency decay, ACU speaker mode, stance dynamics and frame alignment. It includes the legacy Draft Systems R regex scorer (Stage 5).

Brain notes: [actors/debate-stance-model](obsidian://open?vault=brain&file=actors%2Fdebate-stance-model); [concepts/two-hop-stance-decomposition](obsidian://open?vault=brain&file=concepts%2Ftwo-hop-stance-decomposition); [topics/2026-06-feature-cu-stance-and-sentiment](obsidian://open?vault=brain&file=topics%2F2026-06-feature-cu-stance-and-sentiment); [topics/2026-06-feature-cu-stance-and-sentiment-stance-stack-design-and-accuracy](obsidian://open?vault=brain&file=topics%2F2026-06-feature-cu-stance-and-sentiment-stance-stack-design-and-accuracy); [topics/2026-07-feature-influence-engine-internals-salience-sna-wave](obsidian://open?vault=brain&file=topics%2F2026-07-feature-influence-engine-internals-salience-sna-wave)

## Melange stance and tone measurement: evidence assessment

**Scope.** 12 components and 50 evidence rows (30 distinct papers). Of those rows, 6 VERIFIED, 35 VERIFIED-SOFTENED, 9 UNVERIFIED. Source: `_work/state/hub/melange-stance.json`, read 2026-10-09.

**Bottom line.** None of the literature validates any headline number this system reports. Every accuracy figure (DEBATE 0.839 / kappa 0.742, SVM 83.9% / macro-F1 0.816, two-hop 0.782, masking sign agreement 77.5%) comes from the project's own single-look tests, most on n = 87 to 400. The papers do three things:

- **Explain failure modes.** Readers miss implicit stance. Dictionaries are blind to context. LLMs over-infer.
- **Supply method templates.** Joo's duplication rate, Zhou's stance trajectories, Haselmayer's validation protocol, Park's direction pairs.
- **Show that several borrowed numbers do not transfer.** Kim's 96.9%, Sotoudeh's 6-month half-life and b-2025's transformer-beats-SVM ordering do not carry over.

**Strongest verified findings. These are the system's own measurements, reproduced on a second path:**

1. **Frame alignment is mostly ACU syndication.** The window is the whole `cu_press_stance_tone.csv` (2,388 rows, file dated 2026-06-30). Of those rows, 816 are americascreditunions.org items carrying only 349 distinct titles, with up to 19 members on one title.
   - General CU duplication: 60.2% of 442 items, but 5.2% of the 116 own-domain items.
   - CU Tax Status: 64.7% of 190, but 38.1% of 42 own-domain.
   - Defense, which has little ACU content: 12.2% of 41 and 13.9% of 36.
2. **Stance-dynamics neighbour co-movement is null.** Cosponsor layer: 134 same vs 122 opposite (52.3%). HCoI re-run: 302 vs 296 (50.5%). Both are below the 0.55 threshold, and there is no permutation or common-shock control.
3. **Imputation has no baseline.** Masking sign agreement is 77.5% on n = 1,087 confident cells, with no party-only or majority baseline.
4. **The legacy graded-tone header misstates its method.** It cites Haselmayer & Jenny's strongest-cue (max) rule, but the code sums every cue.

**Gap.** ACU speaker mode has no evidence rows. Several components cite papers that have no evidence rows: Burnham's DEBATE paper, TSE / Stanceformer, Fink 2023, Bacik 2026, and Kim, Stuckatz & Wolters.

**Wording guards for all downstream text.**
- Neighbour and tie weights are heuristic model outputs, never closeness or access (D-025).
- Zhou's "social influence" wording must not be carried over.
- Pooled party chi-squares and CFPB tier counts are UNSTRATIFIED.
- UNVERIFIED rows are single-agent leads, not findings (DE-062).
- Fink et al. 2023 is cited in the system for "author-volume normalized social and press edges". That is the per-edge volume normalization this project measured and REJECTED. It is FLAGGED and not recommended anywhere below.

## Fine-tuned Political DEBATE NLI stance reader

**What it computes.** Per-release (and per-tweet) stance toward credit-union interests: support, oppose or neutral (sign -1/0/+1).

**Method.** mlburnham/Political_DEBATE_base_v1.0 fine-tuned on cu_stance_label_set.csv (552 rows; seed 42; 3 epochs). A Claude rater pre-labelled and human labels override. CPU, about 0.3 s per document. Reported results: 87.2% vs Claude (distillation); about 85% / kappa 0.76 on a member-grouped human-gold test; ft accuracy 0.839 / kappa 0.742 held out; 0.796 on the 760-row gold frame; 0.571 on the adversarial goldeval frame. It is one of five readers in the stacked press stack and the fallback reader in build_media_blend.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_debate.py`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/finetune_debate.py`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/claude_stance.py`, `press_stance_stack.py; tweet_stance_stack.py`

**Assumptions.**
- Policy stance toward CUs is recoverable from text by NLI entailment
- DEBATE neutral means the text is not about the bill, not a missed stance
- The held-out test sets (87 or 156 rows) are representative

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2025-measuring-interest-group]] | 2025 | qualifies | GPT-4 zero-shot labelling at temperature 0 reaches 96.93% accuracy on 391 coder-agreed lobbying lines. The lines pass a keyword filter for position terms first (e.g. endorse, defeat, advocate, watch). | Does not transfer. Kim's text is pre-filtered to explicit position terms about a named bill, and the validation set excludes coder disagreements. Press stance toward CUs needs a target-valence step that Kim's task lacks. The 96.9% is not a benchmark for this reader. | weak | VERIFIED-SOFTENED |
| [[dai-2025-large-language-model]] | 2025 | suggests-extension | Fusing decisions from three LLMs (GPT-3.5-turbo-0301, Claude-2-100k, LLaMA-2-70b-chat) by attention-based routing (Eqs 7-11) beats a single-GPT-3.5 variant: 81.0 vs 75.2 on zero-shot VAST with 100 percent training data (+5.8), and 80.2 vs 74.9 with 10 percent (+5.3). The authors say LLMs often exhibit bias in knowledge elicitation that varies with training data and methods. | The paper's ablation confounds fusion with the number of LLMs (one versus three), so it does not isolate a fusion effect, and no variance is reported. The benchmark inputs are tweets for SEM16 and ISD, while the paper does not describe VAST's source text and gives no collection dates, so a 2016-2020 framing is unsupported. The named model snapshots are the paper's choices; their retirement is not checked here. A second LLM rater would expose labeller-specific bias before human override, but the evidence is weak and new agent output stays UNVERIFIED (DE-062). | weak | VERIFIED-SOFTENED |
| [[b-2025-comparing-fine-tuned]] | 2025 | qualifies | A fine-tuned RoBERTa reaches 0.97 accuracy on political stance. Misclassification concentrates in sarcastic, ambiguous and indirect statements, and some disagree-labelled items are predicted unrelated when explicit negation is absent. | The error-mode finding (implicit and indirect stance is the residual failure) transfers and matches the system's neutral versus support seam. The 0.97 headline should not be a target or prior because it comes from a non-common evaluation set. The DEBATE reader is an NLI entailment model, not a RoBERTa classifier, so the architectures differ. Treat the paper as weak qualitative support only. | weak | VERIFIED-SOFTENED |
| [[taherdoost-2023-artificial-intelligence-sentiment]] | 2023 | context | The paper asserts that AI understands the tone of a statement rather than just recognising words. Citing Alantari et al. 2022, it says neural-network ML gives the most accurate predictions of the text-based methods compared. The review reports no accuracy figures of its own, and its corpus is 43 business and competitive-research papers from 2012-2022. | Weak. The cited evidence covers consumer reviews and business social media, not congressional press releases. It also concerns sentiment polarity, not target-specific stance toward CU policy interests. It neither supports nor tests the system's validated accuracy, which comes from a stricter protocol (held-out, member-grouped human gold). | weak | UNVERIFIED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | supports | Tweets whose meaning depends on outside political context are misread unless the prompt supplies that context. The Masterpiece sentiment prompt included a preamble on the ruling and six few-shot examples, and GPT-3 got the sign right on 88.4 percent of 945 tweets. | The paper task is sentiment toward a court ruling, not stance toward an interest, so the paper gives no accuracy figure for CU stance. The paper used six few-shot examples; claude_stance.py is zero-shot with a policy map. The supported point is the design choice of supplying context in the prompt. | moderate | VERIFIED-SOFTENED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | qualifies | GPT-4, tuned with RLHF, is poorly calibrated and overconfident. For over 70 percent of its posts it gave one label more than 99 percent probability, and it chose Neutral more often than the authors. GPT-4 was slightly worse than GPT-3 on sentiment and little or no better on ideology. Section 4 says newer RLHF models frequently underperform on classification and scaling tasks, and the latest model is not always the best. | These are GPT-3 and GPT-4 results, not Claude results. The module requests no token log-probabilities (none found in the file), so the paper probability-vector method cannot be applied as written. claude_conf is a self-stated number, not a calibrated probability. Neutral rows created by ERR should be counted apart from model-chosen neutral rows. No DEBATE-vs-Claude agreement figure was located in the sources checked. | moderate | VERIFIED-SOFTENED |
| [[vanderdoes-2022-strategic-identity-signaling]] |  | qualifies | Covert tweets were defined partly by moderates rating them neutral in orientation and affect while far copartisans saw political content; the authors argue automated political-speech classifiers will likely detect only overt speech and miss covert signals. | Holds only if members code CU positions for mixed constituencies. The paper's covertness concerns partisan identity on a left-right axis among ordinary Twitter users, and CU issues are comparatively bipartisan, so the size of any neutral-bin leakage in congressional press is unknown. The classifier warning is the authors' conjecture, not a tested result. | weak | UNVERIFIED |

**Assessment.**

**Evidence:** 7 rows (5 VERIFIED-SOFTENED, 2 UNVERIFIED). No VERIFIED row bears on the reported accuracy, and none supports it as a benchmark.

- **kim-2025 (qualifies, weak).** GPT-4 reached 96.93% on 391 lines that had first passed a keyword filter for position terms, with coder disagreements excluded. This does not transfer. Kim's text is pre-filtered explicit position language about a named bill, and CU press stance needs a target-valence step that Kim's task lacks.
- **b-2025 (qualifies, weak).** The headline 0.97 is not a usable target. The error-mode finding does transfer: residual errors cluster in implicit, indirect and sarcastic statements. That matches this reader's dominant error, implicit support read as neutral (n = 78).
- **ornstein-2024 (supports, moderate).** This bears on `claude_stance.py`, the pre-labeller and ensemble member, not on DEBATE. Supplying political context in the prompt is a sound design (GPT-3 got the sign right on 88.4% of 945 tweets with a preamble plus 6 few-shot examples).
- **ornstein-2024 (qualifies, moderate).** RLHF models are overconfident and drift toward Neutral. That is a GPT-3/4 result, not a Claude result. `claude_conf` is self-stated, not calibrated. ERR-generated neutrals (confidence 0.0) should be counted apart from model-chosen neutrals. An unrecognised label is mapped to neutral but keeps its stated confidence (line 92).
- **dai-2025 (weak).** Lead only: multi-LLM fusion beats a single LLM by about 5 points on VAST. The ablation confounds fusion with the number of models.
- **anon-nd strategic identity (UNVERIFIED lead).** Covert or coded support may land in the same zero bin as off-topic text, because DEBATE's neutral hypothesis is literally "neutral toward or not about credit unions".

**Weight:** weak. Only the implicit-stance failure mode is corroborated by the literature. The 0.571 on the adversarial goldeval frame, against 0.796 on the 760-row frame and 0.839 on n = 87 held out, is the system's own warning that accuracy is target-dependent.

**Contradictions.**
- Paul's position leans on Kim's 96.9% LLM-vs-human figure. The verified bearing says that figure does not transfer: it comes from pre-filtered explicit position text with disagreements excluded, so it is not a benchmark for CU press stance.
- The component says the reader is fine-tuned (552 rows, 3 epochs). Per the dai-2025 bearing, the cited `stage5_debate.py` runs a zero-shot-classification pipeline on the base model with hypothesis template 'This text is {}.' and contains no training code. The 552-label set and the 87.2% Claude agreement are not in the cited files. Where the fine-tuned weights come from is undocumented in those files.
- The DEBATE held-out accuracy (0.839, kappa 0.742) is numerically identical to the LinearSVC accuracy (83.9%), and both are on an n = 87 test. These may be two genuine measurements or one figure attributed twice. This must be resolved before either is quoted.
- The assumption that DEBATE neutral means "not about the bill" is already marked REFUTED in bill-sign-handling as a premise for overrides. In addition, the neutral bin pools off-topic text, possibly coded stance, and Claude ERR fallbacks.

**Gaps.**
- No evidence row draws on Burnham's own DEBATE / stance-detection guide (PSRM 2024.35), the method source the system cites. Read it for the NLI template and fine-tuning protocol.
- No study tests NLI stance on congressional press releases toward an interest group. Needed: a member-grouped, human-only gold test of a few hundred releases, stratified by party and target family (CFPB, interchange, tax).
- Distillation bias is unmeasured: how much of the agreement is with the Claude pre-labeller rather than with humans. Needed: a blind human re-label of a sample where the pre-labeller's output is hidden, then DEBATE accuracy against human-only and against Claude-seeded labels.
- There is no calibration study of DEBATE's entailment scores. It runs with multi_label=True, so its three scores are independent and do not sum to 1.

**Paul's position vs the evidence.** Paul endorses LLM and transformer position extraction and flags ambiguous statements as the weakness. The evidence agrees on the weakness: b-2025 and the anon-nd lead both put the residual error in implicit, indirect or coded statements, which matches the system's neutral-vs-support seam. It does not back the strength he cites. Kim's 96.9% does not transfer, and Birkenmaier's GPT-4o advantage is on a personality-cue task, not stance. Ornstein qualifies the LLM optimism: newer RLHF models can be overconfident and lean Neutral.

**Open questions.**
- Are the DEBATE 0.839 / kappa 0.742 and the LinearSVC 83.9% on n = 87 two separate measurements?
- Where in the code are the fine-tuned DEBATE weights produced and loaded, given that `stage5_debate.py` shows only the zero-shot base pipeline?
- What share of DEBATE neutral outputs on the 760-row gold are true off-topic text, and what share are implicit support?
- How many neutral rows in the Claude pre-labels are ERR fallbacks rather than model choices?

**Read first.** [[b-2025-comparing-fine-tuned]], [[ornstein-2024-train-your-stochastic]], [[kim-2025-measuring-interest-group]], [[vanderdoes-2022-strategic-identity-signaling]], [[dai-2025-large-language-model]]

## Supervised and stacked stance readers

**What it computes.** Stacked stance label per tweet or press item from classical and API readers.

**Method.** Best classical model: LinearSVC on word+char TF-IDF, 83.9% / macro-F1 0.816 on untouched test (n=87). Khan-style multi-view augmentation was tested and hurt classical models. Supervised tweet stance reached 0.737 CV on 399 golds. Stanceformer is planned, not built.

**Code refs.** `train_stance_domain.py; stance_clf_composed.pkl`, `fec/train_tweet_stance.py`

**Assumptions.**
- Target-specific labels are needed; sentiment is not stance
- The 456/87/217 split is never reshuffled; CV of about 0.90 is inflated and not quoted

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[birkenmaier-2025-measuring-politicians-public]] | 2025 | qualifies | On the paper's personality-cue task, few-shot LLM prompting (0.77) beats fine-tuned XLM-RoBERTa (0.70, strong balanced), which only slightly beats a bag-of-words SVM (0.63 strong balanced; 0.55 to 0.63 across variants). | Different task (CU press and social stance, not German agency and communion cues) and a different baseline: the paper's SVM is word-only with no C tuning. The system's ranking comes from its own held-out test, so this paper cannot rank the system's readers. The code's transformer defaults to distilroberta, not XLM-RoBERTa. | weak | VERIFIED |
| [[dai-2025-large-language-model]] | 2025 | qualifies | In-target (Table 3): FOLAR beats the best competitor by 1.7 on LA (72.7 vs COLA 71.0) and by 2.0 on DT_I (73.3 vs MPPT 71.3), but trails KEprompt on FM (70.6 vs 72.1). Zero-shot (Table 4): gains of +4.2 on VAST with 100 percent training data and +6.2 with 10 percent. The baselines include statistical, PLM, prompt and LLM methods. No TF-IDF or SVM baseline is reported. | The paper cannot settle SVM versus LLM for this corpus because it runs no bag-of-words baseline. Its pattern (small in-target gains, larger low-data or new-target gains) suggests an LLM-rule reader would help most on new or rare CU targets, which has not been tested for CU stance. The evidence is benchmark-level, rests on three runs and gives no confidence intervals. | weak | VERIFIED-SOFTENED |
| [[b-2025-comparing-fine-tuned]] | 2025 | method-source | Classical models on TF-IDF features rank RF 0.95 > SVM 0.94 > LR 0.93 > DT 0.92 > NB 0.89 on a 1,675-row test split. Fine-tuned RoBERTa on tweet plus comment paired input reports 0.97 accuracy on a support of 6,699 described as the augmented dataset. Augmentation and class balancing are credited with gains, but no with and without comparison is shown. | The mechanics (paired input, balanced loss, per-class P/R/F1 and macro-F1) transfer. The model ordering and the augmentation gain are not established by the paper: its classical rows are scored on 1,675 rows and its RoBERTa row on 6,699 rows. 6,699 is almost exactly 0.8 x 8,374 (6,699.2), the size of the 80 percent training share, so the RoBERTa figure may not be on a held-out set. The paper does not say which, so this is an inference and must be labelled as one. | strong | VERIFIED-SOFTENED |
| [[b-2025-comparing-fine-tuned]] | 2025 | qualifies | The transformer beats every classical model (0.97 vs at most 0.95). Dataset augmentation and class balancing are credited with improving performance, with no with and without comparison shown. | The system numbers are single-look on an 87-row test and were not re-verified here. They are not a refutation of the paper's own measurement; they show that its ordering and augmentation gain did not transfer to this small English press corpus. Relation changed from contradicts to qualifies, because the paper's claim is about its own data. | moderate | VERIFIED-SOFTENED |
| [[naz-2025-machine-deep-learning]] | 2025 | qualifies | Traditional ML (SVM and similar on TF-IDF) remains interpretable and competitive on small data. Deep and transformer gains appear with large labeled corpora. The text claims about a 25% LSTM gain over traditional ML, but no table supports the figure. | The paper studies personality, not stance, so this is a pattern about small labeled sets of a few hundred rows, not stance evidence. It fits the system's measured outcome. It does not show that transformers lose at larger n, and it says nothing about LLM zero-shot readers (Birkenmaier, Kim J.), which Paul's view rests on. | weak | UNVERIFIED |

**Assessment.**

**Evidence:** 5 rows (1 VERIFIED, 3 VERIFIED-SOFTENED, 1 UNVERIFIED). None ranks readers on this corpus.

- **birkenmaier-2025 (VERIFIED, qualifies, weak).** Few-shot LLM 0.77 beats fine-tuned XLM-R 0.70, which beats bag-of-words SVM 0.63. The task is German agency/communion personality cues, and the SVM is word-only and untuned. It cannot rank the system's tuned word+char LinearSVC.
- **b-2025 (method-source, strong).** The mechanics transfer: target-conditioned paired input, balanced loss, per-class P/R/F1. Its ordering does not. Classical models were scored on 1,675 rows and RoBERTa on a 6,699 "support", which is about 0.8 × 8,374, the size of the training share. The RoBERTa 0.97 may therefore not be held out. That is an inference.
- **b-2025 (qualifies, moderate).** On the CU press gold the ordering reversed: tuned SVM 0.816 macro-F1 vs augmented transformer 0.757 on the untouched n = 87. These are single-look figures, not re-verified. Augmentation was rejected 2026-07-03.
- **dai-2025 (weak).** The paper runs no bag-of-words baseline. Its pattern is small in-target gains and larger low-data or new-target gains. That suggests an LLM reader would help most on rare or new CU targets, which is untested.
- **naz-2025 (UNVERIFIED lead).** Classical models are competitive on small labeled sets, which fits the system's result.

**Weight:** weak on ranking, moderate on mechanics. The production choice (SVM) rests entirely on one 87-row look. With n = 87, a 0.06 macro-F1 gap is within about 1.5 standard errors and was not tested with a paired test.

**Contradictions.**
- Paul calls bag-of-words SVM outdated next to LLMs. The system's own held-out measurement put the tuned SVM (0.816 macro-F1) above the fine-tuned transformer (0.757). The literature he relies on (Birkenmaier) compares against a weaker, untuned SVM on a different task. Strictly, his claim is about LLM zero-shot readers, and no evidence row compares the Claude reader with the SVM on CU gold, so the claim is untested rather than refuted.
- Per the dai-2025 bearing, `train_tweet_stance.py` scores by 5-fold stratified CV over the golds and has no untouched test split. That sits awkwardly with the stated rule that CV is inflated and not quoted. The 0.737 tweet figure is a CV figure.

**Gaps.**
- No head-to-head of SVM vs fine-tuned DEBATE vs Claude zero-shot on the same untouched set with a paired test (McNemar). That is the comparison that would settle Paul's position.
- The Stanceformer / TSE sources the system cites have no evidence rows. The neutral-class target-aware-attention question is unaddressed.
- No learning-curve study (accuracy vs training n) to test the small-data explanation for the SVM's advantage.

**Paul's position vs the evidence.** Paul says SVM baselines are outdated and Peng's stacking reduces, but does not remove, long-distance dependency. The verified evidence does not support calling the SVM outdated for this corpus size. His supporting paper uses a different task and a weaker baseline, and the system's own look went the other way. Peng has no evidence row, and Paul's plan to re-read it stands. The fair reading: the SVM wins at about 456 training rows on this corpus, and LLM zero-shot readers remain untested against it.

**Open questions.**
- Is the 0.757 transformer figure macro-F1 or accuracy, and is it the same distilroberta reader that `train_stance_domain.py` defaults to?
- Does the Claude reader beat the SVM on the n = 87 untouched test, and on rare targets in particular?
- Is the 456/87/217 split still intact, given that the tweet trainer has no untouched split?

**Read first.** [[b-2025-comparing-fine-tuned]], [[birkenmaier-2025-measuring-politicians-public]], [[dai-2025-large-language-model]], [[naz-2025-machine-deep-learning]]

## Lexicoder LSD2015 tone intensity

**What it computes.** Per-release tone intensity = |net| / words from LSD2015 positive, negative and negated counts.

**Method.** stringi regex implementation that matches quanteda data_dictionary_LSD2015 counts; negation phrases are stripped first; 9.6 ms per document.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/parse_lsd.R`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_lsd.R`

**Assumptions.**
- Dictionary counts measure how heated a release is (intensity, not direction)

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[zhang-2022-data-assisted-social]] | 2022 | context | Scores phrase sentiment with the general-purpose Affin lexicon plus an ensemble classifier for praise vs complaint. Accuracy is described as 'highest' with no metric, and the Affin citation points to an unrelated photonics paper. | There is no evidence here that a general lexicon measures tone validly on congressional press releases. The open question (does LSD intensity agree with human-coded intensity?) is untouched. Rice & Zorn's case for domain dictionaries remains the relevant source. | weak | UNVERIFIED |
| [[pennebaker-2003-psychological-aspects-natural]] | 2003 | qualifies | Emotion words are under 5 percent of words in daily speech, emotional writing and affect-laden poetry, so emotion-word counts predict emotional state weakly. Word-count programs cannot consider context, irony or sarcasm. In at least two studies, judges' ratings of emotionality correlated more highly with language use than with self-reports. | The paper claim concerns personal emotional state, not how heated a policy release is. LSD2015 is a sentiment dictionary (Young and Soroka 2012 per file header), not LIWC. The sparsity point carries over: in short releases a few hits set the multiplier. The judge finding supports validating intensity against human coding. | weak | VERIFIED-SOFTENED |
| [[taherdoost-2023-artificial-intelligence-sentiment]] | 2023 | qualifies | Alantari et al., as summarised by the review, find a trade-off: neural networks predict well but diagnose poorly, while more transparent methods are poorer predictors. The review frames word-matching approaches as the baseline that AI improves on. The trade-off is qualitative only and no numbers are given. | The paper agrees with the system's split of roles: an opaque predictive reader for direction and a transparent dictionary for an interpretable intensity scalar. It gives no evidence that dictionary intensity is valid on political press releases. The open question of whether LSD intensity matches human-coded intensity on this corpus is untouched. The source evidence is consumer reviews. | weak | UNVERIFIED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | qualifies | Dictionary methods lose badly when meaning depends on context. The best dictionary the authors could build got 38.3 percent of tweet signs right, against 88.4 percent for GPT-3. Of 5332 speeches containing honor, GPT-3 labels only 145 as praising honor. | The paper dictionary is the authors own construction, not LSD2015, and the paper did not test intensity. Intensity inherits the context blindness the paper describes (ceremonial or quoted language reads as heated). Whether LSD intensity tracks human-coded intensity is untested. | weak | VERIFIED-SOFTENED |
| [[merkley-nd-party-cues-news]] |  | qualifies | The authors use Lexicoder with a custom dictionary of party names, office titles and party leaders to detect party cues, not sentiment or tone. They validate detection against hand coding of a random 700 articles (about 80% contain a cue by their definition). Stance is hand-coded on a separate random sample of 3000 articles (97% of articles with Democratic messages are wholly pro-consensus). Frames come from SVM classifiers (77-80% accuracy). | The paper is a method precedent only: a dictionary for detecting actors plus hand-coded validation. It never uses or validates sentiment or tone as an intensity measure, so it is no evidence that LSD tone intensity is valid on congressional press releases. | weak | VERIFIED-SOFTENED |
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | qualifies | Speech topic, word choice and length of remarks are highly controllable and driven by strategic concerns, while vocal pitch typically lies beyond the individual's control, making it a more honest indicator of intensity on a given issue (leakage hierarchy, pp. 942-944). The paper does not compare dictionary tone or LSD scores with pitch. | A theoretical argument only; the paper never tests LSD tone against pitch. The claim that press releases are staff-drafted and so more strategic than floor speech has no support in the paper or the code. LSD charge measures crafted rhetorical tone, not felt intensity, and should be labelled that way. | weak | VERIFIED-SOFTENED |
| [[rice-2018-corpus-based-dictionaries]] | 2018 | qualifies | General-purpose dictionaries carry valences that are wrong in specialized vocabularies (tennis 'love'). On the movie-review benchmark a corpus-derived dictionary reached 80.4% in-sample and 79.5% held-out, versus LIWC 69.4% and AFINN 71.3% in-sample and AFINN 70.6% held-out. | The paper's evidence covers polarity classification of movie reviews and court opinions, not intensity and not congressional press releases. The mechanism (domain nouns read as tone) is visible directly in the LSD entries. Size of the effect on releases remains UNVERIFIED in this pass. The paper does not test whether LSD intensity tracks human-judged heat. | moderate | VERIFIED-SOFTENED |
| [[park-2025-automatic-construction-direction]] | 2025 | qualifies | Direction plus noun carries polarity that a general or finance dictionary misses. LM alone labels 'Profit for the period was EUR 10.9 mn, down from EUR 14.3 mn in 2009' neutral (SentimentScore 0), while the (profit, down) pair gives refined score -1. On DS100, negative-class F1 is 0.4087 for LM alone and 0.7260 for LM+Senti-DD. | Weak. LSD sign is discarded here, so the paper's flips matter for intensity only when directional pairs cancel or inflate /net/, as in cost-cut or fee-reduce releases. The paper tests neither LSD nor intensity, and Paul's system has no human-coded intensity gold to test this against. | weak | VERIFIED-SOFTENED |
| [[haselmayer-2017-sentiment-analysis-political]] | 2017 | measurement-source | Validation on 200 sentences: aggregated lay crowd vs aggregated expert r = 0.82. Domain dictionary vs expert r = 0.65 at 84% coverage, about 0.6 when the 32 unmatched sentences are set to zero. Momtazi r = 0.13 (46% coverage) and SentiWS r = 0.19 (31% coverage). Authors state that dictionary analysis requires a customized dictionary and recommend caution with general-purpose dictionaries. | The validation protocol transfers: about 200 releases rated 0-4 by lay coders and experts, correlated with /lsd_net_norm/. The size of the generic-dictionary penalty does not transfer directly, because the paper's comparators are machine-translated German lexicons, not LSD2015. This paper does not establish that LSD2015 performs well on English news; the Young and Soroka citation is general sentiment work, not an LSD validation. The paper scores sentences for negativity only, while LSD here is bidirectional and scores whole release bodies. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence:** 9 rows (7 VERIFIED-SOFTENED, 2 UNVERIFIED). None validates LSD2015 intensity on congressional press releases. Every bearing either qualifies the instrument or supplies a protocol.

- **haselmayer-2017 (measurement-source, moderate).** This is the template to follow. On about 200 sentences, aggregated lay crowd vs experts gave r = 0.82, a domain dictionary r = 0.65, and generic lexicons r = 0.13 to 0.19. Caveat: the generic comparators are machine-translated German lexicons, not LSD2015, so the size of the penalty does not transfer.
- **rice-2018 (moderate).** Domain words carry wrong valence in general dictionaries. The bearing verified this in LSD itself: the 'credit' family (credit, credits, credited, ...) is positive, and debt*, charge*, risk*, oversight* and fraud* are negative, with no domain masking in `run_stage5.R:79`. In CU releases 'credit union' mentions therefore inject positive hits mechanically. Because intensity is |net|/words, those hits can inflate or cancel heat. The local masking-effect figures (rho 0.953, 4.1% sign flips) were not reproduced.
- **ornstein-2024.** Context blindness: the authors' own dictionary got 38.3% of tweet signs vs 88.4% for GPT-3. It was not LSD and not an intensity test.
- **pennebaker-2003.** Emotion words are under 5% of tokens, so in short releases a few hits set the multiplier.
- **dietrich-2019.** Theory only: crafted text is strategic, so LSD measures crafted rhetorical tone, not felt intensity. Label it that way.
- **park-2025.** Direction-noun pairs (e.g. cost-cut) can cancel or inflate |net|.
- **merkley-nd.** A dictionary-plus-hand-coding precedent for actor detection, not tone.
- **zhang-2022 and h-2023 (UNVERIFIED).** Consumer-review context only.

**Weight:** the evidence that a generic dictionary is risky here is moderate and convergent. Evidence that LSD intensity is valid on this corpus is absent. The brain note calls LSD "validated" on a three-case smoke test (pro +4 / negated 0 / neutral 0). By Haselmayer's standard that is a functional check, not a validity check.

**Contradictions.**
- Brain note `debate-stance-model.md:102` describes LSD as validated. The verified bearing shows the evidence is a three-case functional smoke test, not a validity study.
- An earlier bearing said charge per word is the intensity used in press_value. The corrected bearing shows live `run_stage5.R` defaults to |net_norm| (line 45, INTENSITY default net_norm), with charge only as an option.
- Paul's only literature anchor here (Merkley & Stecula) uses Lexicoder for party-cue detection, not tone intensity, so it does not support the component's construct.

**Gaps.**
- Human-coded intensity validation: about 200 CU releases rated 0 to 4 by lay and expert coders, correlated with |lsd_net_norm| and lsd_charge (Haselmayer protocol), with and without CU-domain masking of credit/charge/risk/oversight terms.
- Incremental validity: does stance sign × intensity predict subsequent CU votes or cosponsorship better than sign alone? No evidence row touches this.
- A sensitivity study of the domain-term effect on member-level press_value ranks, reproduced on a second path. The existing figures were not reproduced because of no R runtime and an rds-only text cache.

**Paul's position vs the evidence.** Paul has no annotation on tone. He highlighted Merkley & Stecula's use of Lexicoder with custom dictionaries to detect party cues. The point of that highlight that does transfer is custom dictionaries plus hand-coded validation, which is exactly what this component lacks: it runs stock LSD2015, uncustomized, with no human intensity gold.

**Open questions.**
- What share of LSD positive hits in CU releases come from the 'credit' family alone?
- Should intensity be relabelled 'rhetorical tone (LSD2015)' in all outputs, per the Dietrich bearing?
- Does the |net| default or the charge option better track human heat ratings?

**Read first.** [[haselmayer-2017-sentiment-analysis-political]], [[rice-2018-corpus-based-dictionaries]], [[ornstein-2024-train-your-stochastic]], [[pennebaker-2003-psychological-aspects-natural]], [[dietrich-2019-pitch-perfect-vocal]]

## press_value aggregate and human-label overlay

**What it computes.** press_value = stance_sign x LSD intensity per member x category. Human labels overwrite model stance where valid; 497 of 552 labels overlap the corpus.

**Method.** run_stage5.R block 3c is placed after the bill override and the Kennedy fix. Overlaid rows are excluded from model-accuracy measurement.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/make_standalone.py`

**Assumptions.**
- Human labels are authoritative
- Overlay rows cannot be used to measure DEBATE accuracy

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[ornstein-2024-train-your-stochastic]] | 2024 | suggests-extension | Continuous measures built from the label probability vector: per-sentence P(Conservative) minus P(Liberal), averaged per manifesto, gives rho 0.92 (economic) and 0.80 (social) across 18 UK manifestos with GPT-3. Tweet sentiment uses the first principal component of the positive/negative/neutral probability vector. | The paper evidence uses GPT-3 next-token probabilities on a few-shot prompt, not DEBATE NLI entailment scores. DEBATE runs with multi_label=True (stage5_debate.py:65), so its three scores are independent and do not sum to 1. A p_support minus p_oppose contrast is a different object and would need a pre-registered single look on the 760-row press gold. Remains UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED |
| [[rice-2018-corpus-based-dictionaries]] | 2018 | qualifies | Changing the dictionary reorders units. Justice-level rank correlations are 0.29 (corpus measure vs LIWC) and 0.16 (corpus measure vs AFINN), versus 0.64 between LIWC and AFINN. Standard deviation of justice means is 0.25 (corpus), 0.22 (LIWC) and 0.16 (AFINN). Justices with more than 50 majority opinions; comparison across 84 justices. | The divergence evidence comes from a 217-year court corpus with large semantic drift. Press releases from 2022 on are a narrow window, so the divergence is probably smaller there; that is the analyst's inference and is untested. The live blend press weight of 0.10 was not checked in this pass. | weak | VERIFIED-SOFTENED |

**Assessment.**

**Evidence:** 2 rows (2 VERIFIED-SOFTENED). Neither addresses the human-label overlay.

- **ornstein-2024 (suggests-extension, moderate).** A continuous score from the label probability vector reached rho 0.92 (economic) and 0.80 (social) across 18 UK manifestos with GPT-3. Here press_value uses the argmax sign × LSD intensity, while p_support / p_oppose / p_neutral are kept as columns but unused. DEBATE runs with multi_label=True (`stage5_debate.py:65`), so its three scores are independent entailment scores that do not sum to 1. A p_support − p_oppose contrast would be a different object from Ornstein's, and stays an UNVERIFIED proposal pending a pre-registered single look on the 760-row press gold.
- **rice-2018 (weak).** Changing the dictionary reorders units: justice-level rank correlations were 0.16 to 0.29 between the corpus measure and LIWC/AFINN. press_value uses one instrument with no alternative compared, so its sensitivity to the instrument is unknown. The analyst infers, untested, that divergence is smaller in a 2022-onward window.

**Weight:** weak. The aggregate inherits the unvalidated LSD intensity, and argmax over independent multi-label entailment scores is itself an unexamined design choice.

**Contradictions.**
- None of the evidence contradicts the component directly. One internal tension: picking the sign by argmax over three independent (multi_label=True) entailment scores treats them as if they were a distribution, which they are not.

**Gaps.**
- The empirical question (how much of a member's press score comes from human-overlaid rows) is an internal decomposition no paper can answer. Needed: per member × category, the share of press_value mass from overlay rows (497 of 552 labels overlap the corpus), reported with window and denominator.
- No instrument-sensitivity study: recompute member press_value ranks with a CU-masked LSD and with a probability-contrast sign, and report rank correlations.
- Bacik et al. 2026, which Paul cites as the frame for press as an augmentation to lobbying-pathway models, has no evidence row.

**Paul's position vs the evidence.** Paul treats sentiment-scored press as an augmentation to lobbying-pathway models and does not discuss the overlay. The evidence neither supports nor tests that use. It warns that the aggregate depends on one dictionary (Rice & Zorn), and it offers a continuous alternative that is untested here (Ornstein).

**Open questions.**
- What fraction of each member's press_value comes from human-overlaid rows?
- Are ACU-authored rows (816 of 2,388 in the press CSV) attributed to members and included in press_value?
- Does argmax over multi-label DEBATE scores ever pick a label with low absolute entailment, and how often?

**Read first.** [[ornstein-2024-train-your-stochastic]], [[rice-2018-corpus-based-dictionaries]], [[haselmayer-2017-sentiment-analysis-political]]

## Two-hop stance x valence decomposition with curated registry

**What it computes.** cu_alignment = stance(speaker->target) x valence(target->credit unions).

**Method.** Measured on 400 tweet golds: stance hop 0.915, valence hop 0.689 (the bottleneck). Composing model stance with oracle valence gives 0.872. Valence registry cu_valence_registry.json v2 (11 derived plus 14 seed entries) is derived from Paul's gold judgments; with 5-fold CV the composition reaches 0.782 accuracy / 0.754 F1 vs a flat baseline of 0.677. Auto-registry variants all fell below the flat baseline.

**Code refs.** `fec/apply_two_hop_stance.py (--eval, --uncovered)`, `derive_valence_registry.py`, `cu_valence_registry.json`, `fec/stance/build_media_blend.py`

**Assumptions.**
- Each target has a stable CU valence the owner can judge
- Per-target valence is not keyword-automatable (238 distinct targets in 400 tweets)

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[dai-2025-large-language-model]] | 2025 | method-source | FOL rules elicited from an LLM (Table 5 bike-lane rule links congestion to Against) are made differentiable with soft-logic operators: negation 1-U, conjunction U1*U2, implication 1-U1+U1*U2 (Eqs 2-5), and trained end to end with cross-entropy (Eq 12). Zero-shot VAST all-class F_avg: 81.0 vs 76.8 for KASD with 100 percent training data (+4.2); 80.2 vs 74.0 for MPPT with 10 percent (+6.2). Error analysis of 50 VAST errors (Sec 4.8) names LLM over-inference that links a positive clause to the target, and clause-level polarity conflicts. | Transfer is limited to the decomposition idea. FOLAR targets are benchmark targets, whereas the valence hop needs CU-specific, owner-judged knowledge that LLM priors do not supply. The paper's over-inference failure matches the automatic-registry failure in the system. LLM-elicited valence rules stay UNVERIFIED (DE-062) until scored against the 400 tweet golds under single-look rules. The paper gives no evidence that a soft operator beats an exact registry lookup where the registry has coverage. | moderate | VERIFIED-SOFTENED |
| [[tang-2015-negative-link-prediction]] | 2015 | supports | Signed triads are overwhelmingly balanced: 92.31 percent (Epinions) and 93.01 percent (Slashdot) of undirected triads (Sec. 3.2 p. 89). This is a structural prior that sign products usually hold in person-to-person trust networks. The paper does not test stance-times-valence composition. | Paper triads are person-to-person links. The system's third node is an interest reached through a curated valence registry. The paper supplies only a structural prior and does not validate the registry. The registry numbers are on the golds the design iterated against, so they are not an out-of-sample validation. | weak | VERIFIED-SOFTENED |
| [[park-2025-automatic-construction-direction]] | 2025 | method-source | Polarity attaches to a (direction, target) pair. Direction-dependent nouns are typed proportional or inversely proportional by PMI against direction-tagged sentences (Eq. 1, delta = 0). Of 719 tagged sentences, 691 are proportional and 28 inversely proportional, and the imbalance yields noisy words such as beer and day. | Paul's registry is a gold-derived analogue of the paper's direction-pair lexicon; the automatic PMI typing is the part that did not transfer. Transfer is limited by scale: 400 tweet golds and 238 distinct targets, against the paper's 4,835 sentences with a 1 percent noun-frequency floor. A PMI typing pass on this corpus needs a held-out look before any claim (DE-062). | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence:** 3 rows (3 VERIFIED-SOFTENED). They support the decomposition idea, not the registry's numbers.

- **dai-2025 (method-source, moderate).** LLM-elicited first-order-logic (FOL) rules over a stance and target relation, trained end to end, give +4.2 F_avg on zero-shot VAST. Its error analysis names LLM over-inference, which matches the system's finding that auto-registry variants fell below the flat baseline. LLM-elicited valence rules stay UNVERIFIED until scored against the 400 tweet golds. The paper does not show that soft operators beat an exact registry lookup where the registry has coverage.
- **park-2025 (method-source, moderate).** Polarity attaches to a (direction, target) pair. Paul's registry is a gold-derived analogue of that lexicon. The automatic PMI typing is the part that failed to transfer: five auto variants scored 0.464 to 0.604 against a flat 0.677. Scale is far below the paper's (400 golds and 238 targets, vs 4,835 sentences).
- **tang-2015 (supports, weak).** About 92 to 93% of signed triads in person-to-person trust networks are balanced. That is a structural prior that sign products usually hold, not a test of stance × valence toward an interest.

**Weight:** moderate for the design, weak for the measured gain. The hop decomposition itself (stance hop 0.915, valence hop 0.689, oracle-valence composition 0.872) is coherent and points to valence coverage as the bottleneck.

**Contradictions.**
- The evaluation protocol for 0.782 conflicts between bearings. The component and the park-2025 bearing call it honest 5-fold CV. The tang-2015 bearing reports that the 0.782 (`tweet_stance_stack.py` docstring) is measured on the 400 tweet golds the design iterated against, and that the 5-fold claim applies to the stacked model. Until resolved, 0.782 / F1 0.754 must not be quoted as out-of-sample.
- The registry is derived from Paul's gold judgments and scored on those same golds. That is a circularity risk independent of the CV question.
- Per the park-2025 bearing, the code keeps families at min_consist = 0.75 while the file header says 80%.

**Gaps.**
- TSE-style decoupling (ACL 2023) is cited in the system but has no evidence row. The empirical question of an explicit two-stage label schema vs end-to-end labels is untested.
- Valence stability across Congresses: no evidence. Needed: re-derive the registry on a later Congress's golds and measure per-target sign agreement.
- Coverage vs accuracy attribution: split residual errors into registry misses (fallback to flat), wrong valence and wrong stance.
- Fink et al. 2023 is cited in Paul's position but has no evidence row.

**Paul's position vs the evidence.** Paul asked how agreement is quantified when two actors both hold negative sentiment toward a third. Structural balance (tang-2015) is the literature's direct answer (negative × negative → aligned), and the registry implements it. The evidence supports it only as a structural prior on person-to-person networks, not as validation of an interest-mediated registry. His notes do not yet record the registry as the answer.

**Open questions.**
- Is the 0.782 composed score out-of-fold, or in-sample on the iterated-on golds?
- Is the registry's family consistency threshold meant to be 0.75 or 0.80?
- Would a fresh, never-seen tweet gold set reproduce the gain over the 0.677 flat baseline?

**Read first.** [[dai-2025-large-language-model]], [[park-2025-automatic-construction-direction]], [[tang-2015-negative-link-prediction]]

## Bill-sign valence (direction table, prose matcher; override rejected)

**What it computes.** Stance sign from a tracked bill's known CU direction when a release names the bill.

**Method.** cfpb_bill_directions.csv (Reduce +1 / Expand -1) plus the BILLS table in cu_press_release_scraper.R. The named-bill matcher had 93% precision on 187 releases but went 0 good / 3 bad as a blanket override. The DEBATE bill-sign override (12-member anti-CU prior list) dropped held-out accuracy from 0.839 / kappa 0.742 to 0.770 / 0.630 and was disabled 2026-06-30. The naive directional-verb lexicon (after Park, Lee & Cho 2025) gave 3 good / 43 bad sign changes and was rejected. The narrow Kennedy per-release fix is kept.

**Code refs.** `cu_press_release_scraper.R (BILLS ~1642; score_bill_signals ~2600)`, `cfpb_bill_directions.csv`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R (CU_S5_BILL_OVERRIDE)`

**Assumptions.**
- REFUTED: DEBATE neutral means a missed bill stance
- REFUTED: directional verb counts over whole text determine valence
- Member priors (anti-bank consumer advocates) need a member-level prior, not a lexicon

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2025-measuring-interest-group]] | 2025 | suggests-extension | The released dataset gives measured group positions on named S./H.R. bills for the 111th-117th Congresses (2009-2022). It is on Dataverse at doi:10.7910/DVN/D0QWM2. | Useful as an external audit of the hand-set direction table, but only for 111th-117th Congress bills that CU trade associations named in LD-2 lines. 118th and 119th Congress bills would need the Haiku lane. Use it to adjudicate the table, never to overwrite it. | moderate | VERIFIED-SOFTENED |
| [[park-2025-automatic-construction-direction]] | 2025 | qualifies | On four FPB subsets (DS50 4,835 / DS66 4,209 / DS75 3,447 / DS100 2,259 sentences) LM+Senti-DD weighted F1 is 0.7057, 0.7322, 0.7788, 0.8233 vs LM alone 0.5914, 0.6023, 0.6174, 0.5982. On SemEval-2017 (960 messages) and FiQA (436 sentences) it is 0.4789 and 0.4937, below several general lexicons. In the 20-case DS100 appendix sample 10 are correct and 10 wrong; cited failures include (profit, increase) and (company, increase). | Evidence transfers only where a directional word governs the target noun inside one short sentence, as in FPB headlines. It does not transfer to whole-release counting over multi-actor congressional press text, where speaker attitude toward the direction is a second hop the paper does not model. The paper's own shift to SemEval and FiQA drops lexicon F1 below 0.5. This supports restricting any rule to the sentence naming the bill; any such variant stays UNVERIFIED until a held-out gold look (DE-062). | strong | VERIFIED-SOFTENED |

**Assessment.**

**Evidence:** 2 rows (2 VERIFIED-SOFTENED). Both are consistent with the system's rejections.

- **park-2025 (qualifies, strong).** A direction-aware lexicon lifts weighted F1 on short financial headlines (0.71 to 0.82, vs 0.59 to 0.62 for LM alone). It drops below 0.5 on SemEval (960 messages) and FiQA (436 sentences), and its own appendix sample is 10 correct and 10 wrong. The evidence therefore transfers only where a directional word governs the target noun inside one short sentence. It does not transfer to whole-release counting over multi-actor congressional text. That explains the rejected directional-verb override (3 good / 43 bad sign changes) and supports restricting any future rule to the sentence that names the bill. Any such variant is UNVERIFIED until a held-out gold look.
- **kim-2025 (suggests-extension, moderate).** The Dataverse release (doi:10.7910/DVN/D0QWM2) gives measured group positions on named bills for the 111th to 117th Congresses. It can serve as an external audit of the 60-row `cfpb_bill_directions.csv`, only for bills that CU trade associations named in LD-2 lines. Use it to adjudicate the table, never to overwrite it. Under D-021, LDA-derived positions describe bills and organisations, never member edges or weights.

**Weight:** moderate, and it backs the existing decisions. Both rejected overrides (the DEBATE bill-sign override, 0.839 to 0.770 accuracy and kappa 0.742 to 0.630; the verb lexicon) are explained by the transfer conditions, not contradicted. The bill table now supplies sign only for text-less billstatus rows.

**Contradictions.**
- No contradiction with the system. A bookkeeping note: the bill-sign override rejection (`run_stage5.R` lines 105-108, 2026-06-30) and the directional-verb lexicon rejection are two separate experiments and should be cited separately.

**Gaps.**
- No evidence on an aboutness or prominence guard (the bill is the main topic). Needed: a held-out gold test of a bill-sign override gated on the bill being in the title or lead sentence, with a single pre-registered look.
- 118th and 119th Congress bills are not covered by Kim's dataset. The table's recent entries have no external audit source.
- No evidence on member-level priors (anti-bank consumer advocates) as a replacement for lexicons.

**Paul's position vs the evidence.** Paul takes Kim's Support / Oppose / Engage positions as the model for per-bill signing and criticises it as reactive, with no view of inter-industry conflict. The evidence supports using Kim's data as an audit of bill direction for 2009 to 2022. Nothing in the rows addresses his reactivity critique. Inter-industry conflict would need positions from opposing coalitions (e.g. banks vs CUs) on the same bills, which the released data might show for those Congresses. That is a lead, untested.

**Open questions.**
- How many of the 60 direction-table bills fall in the 111th to 117th Congresses and appear in Kim's data with a CU trade-association position?
- Does a sentence-restricted direction rule beat the current DEBATE sign on held-out gold?

**Read first.** [[park-2025-automatic-construction-direction]], [[kim-2025-measuring-interest-group]]

## CFPB axis isolation (registry families plus binding policy_force)

**What it computes.** CFPB stance split into restraint (+1), expansion (-1), institution (0) and general (0) families.

**Method.** policy_force is a binding post-meta override in both stacks, and the CATKW router was tightened. CFPB-negative tiers went D 123 -> 98, R 7 -> 0; no-evidence 174 -> 263.

**Code refs.** `fec/stance registry and CATKW router (exact file not named)`

**Assumptions.**
- CFPB restraint vs expansion is separable by registry families
- CFPB-negative tiers were confounded by party (123 D vs 7 R)

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | suggests-extension | Party overlap is under 1.5 percent in both chambers for the past 5 Congresses (p.15). The paper states, citing McCarty, Poole and Rosenthal (1999) rather than presenting evidence, that party per se does not account for much roll-call voting over and above the spatial model (p.17). | With near-zero overlap, dim1 and party are nearly collinear; the informative test is within-party. A pooled coefficient carries the UNSTRATIFIED label. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence:** 1 row (1 VERIFIED-SOFTENED).

- **poole-1999 (suggests-extension).** Party overlap was under 1.5% in both chambers over the five Congresses before 1999, so first-dimension ideology and party are nearly collinear. The informative test is within-party. The paper's statement that party adds little beyond the spatial model is a citation of McCarty, Poole & Rosenthal, not evidence presented in the paper.

**System facts from the bearing:**
- Before isolation, CFPB-negative tiers were D 123 vs R 7. After isolation they are D 98 vs R 0, and the no-evidence count rose from 174 to 263.
- `dw_control.py` provides a dim1 loader but is imported only by `hearing_antagonism_build.py`, not by any stance code.

Isolation therefore made the tier more purely Democratic, not less. The counts alone cannot tell a party proxy from a genuine, party-sorted policy position: CFPB expansion is substantively a Democratic position. Any pooled party-tier association must be labelled UNSTRATIFIED.

**Related:** a brain note quoted in the park-2025 bearing says CFPB is 64% inverse under sentence-level typing and 41% under release-level typing. That is a share of inverse tags, not accuracy. It shows the CFPB target is direction-sensitive at the sentence level.

**Weight:** weak. No paper tests the restraint / expansion / institution / general split.

**Contradictions.**
- The component assumes the CFPB-negative tiers were confounded by party. After isolation the split moved from 123 D / 7 R to 98 D / 0 R, a purer party split. Isolation removed a confound only if the 7 Republican rows were errors. That has not been shown. It may simply have sorted a genuinely partisan position.

**Gaps.**
- Within-party validation: among Democrats only, does the CFPB-negative tier predict votes on CFPB-related roll calls (for example CRA disapproval resolutions on CFPB rules), stratified by chamber, with window and denominator stated?
- No evidence that registry families separate restraint from expansion reliably. Needed: a human-coded sample of CFPB-mentioning releases labelled by family, and agreement of the policy_force router with it.
- No literature at all on CFPB stance measurement.

**Paul's position vs the evidence.** Paul has no literature here. The one bearing implies that, with near-zero party overlap, a pooled party association is uninformative. Only a within-party test against CFPB votes can say whether the axis measures more than party.

**Open questions.**
- Were the 7 pre-isolation Republican CFPB-negative rows misclassifications or genuine positions?
- Should `dw_control.py` dim1 residualization be applied to stance tiers? That needs a prereg and a single look, not adoption.
- What happened to the 89 rows that moved into no-evidence (174 to 263)?

**Read first.** [[poole-1999-nominate-after-years]], [[park-2025-automatic-construction-direction]], [[ringe-2013-pinpointing-powerful-voting]]

## Media blend (press + social) with recency decay

**What it computes.** raw cell = (1-0.5f) press/p80 + 0.5f social/p80, with f = min(1, nz/15) shrinkage for thin categories.

**Method.** Per post: sign x clip(margin, .25, 1.5) x (0.5 if retweet) x 0.5^(age/HL) / n_categories. Half-life is 2.0 years for press and 0.5 years for social. The press-side recency_weight in the R scraper also uses a 2-year half-life with a 2022-01-01 floor. 20 categories via CATKW regex; CROSSPOST_DEDUP is off by default.

**Code refs.** `fec/stance/build_media_blend.py:29-47`, `build_social_master.py`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R (recency_weight, wmean)`

**Assumptions.**
- Stance salience decays exponentially at the stated half-lives
- Social posts carry stance comparable to press after p80 scaling
- Thin categories (under 15 members with signal) are unreliable

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[steinertthrelkeld-2018-twitter-data]] | 2018 | context | Every US Senator and 430 House members had official Twitter accounts (2016). Tweet author metadata reflects when the API request was made, not when the tweet was created. | The coverage figure supports the assumption that almost all members have a social channel, but it counts Twitter accounts in 2016. It does not count the 119th Congress, it does not cover the platform mix the system reads (X, Facebook, Truth Social, Bluesky), and it says nothing about how many posts each member makes on CU topics. The metadata-timing caveat applies to handle-to-member mapping done at collection time. It does not apply to decay, which uses the post date. The paper offers nothing on ghost-writing, on the half-life, or on whether social posts add predictive validity over press releases. | weak | UNVERIFIED |
| [[sotoudeh-2024-network-based-measure]] | 2024 | qualifies | The 6-month half-life is one of three settings for decaying cosponsorship-influence capital, not a stance-salience estimate. Discrimination barely changes between 6 and 24 months (0.248 vs 0.203). | Not transferable. The paper's decay applies to accrued passage-rate ties and its outcome is bill passage. Its flatness across half-lives warns against treating any one value as calibrated. The paper does not justify 0.5 years for social posts or 2 years for press. | moderate | VERIFIED |
| [[hargittai-2015-bigger-always-better]] | 2015 | qualifies | Non-users are excluded from a site-based sampling frame by design. If selection is related to outcomes, results from users may reach the opposite conclusion about the wider population. Sampling should not be tied to the substantive question. The paper does not discuss how missing social-media activity should be coded. | The paper gives no evidence about missing-data coding. The concern bites only if posters differ from non-posters on stance in categories where f = 1; that is untested, and the analyst's own note says so. The stance-ladder rule on missing evidence was not checked. UNVERIFIED. | weak | VERIFIED-SOFTENED |

**Assessment.**

**Evidence:** 3 rows (1 VERIFIED, 1 VERIFIED-SOFTENED, 1 UNVERIFIED). None justifies the half-lives, the 0.5 social weight or the nz/15 shrinkage.

- **sotoudeh-2024 (VERIFIED, qualifies, moderate).** The 6-month half-life is one of three decay settings for cosponsorship influence capital, with bill passage as the outcome. It is not a stance-salience estimate. Discrimination barely changes between 6 and 24 months (0.248 vs 0.203). It does not transfer. If anything, it warns that no single half-life is calibrated.
- **hargittai-2015 (qualifies, weak).** Site-based frames exclude non-users by design. In `build_media_blend.py`, a member with no social value gets sv = 0. At f = 1 the press side is halved and silence scores as zero, not missing. This matters only if posters differ from non-posters on stance in categories at f = 1, which is untested.
- **steinertthrelkeld-2018 (UNVERIFIED lead).** Near-universal official Twitter accounts in 2016. Says nothing about the 119th Congress or the platform mix (X, Facebook, Truth Social, Bluesky). The API-timing caveat affects handle mapping, not decay, which uses the post date.

**Weight:** weak. The blend's parameters (2.0-year press half-life, 0.5-year social, SOCIAL_W 0.5, p80 scaling, nz/15) are design choices with no empirical calibration. The record states that the system's half-lives were not drawn from Paul's notes.

**FLAG:** Fink et al. 2023 is cited in Paul's position here. In the system's citations it supports author-volume normalized social and press edges, which is per-edge volume normalization, measured and REJECTED in this project. Do not use Fink as support for any volume normalization in the blend. The blend's per-post / n_categories split is a different operation.

**Contradictions.**
- Paul's sources give Sotoudeh's 6-month half-life as a decay timescale. The VERIFIED bearing shows it applies to cosponsorship influence capital with a passage outcome, not to stance salience, and that results are flat across 6 to 24 months. It cannot calibrate the 0.5-year social half-life.
- The design treats a member with no social posts as zero signal, halving the press contribution at f = 1. That conflicts with the component's own premise that thin data is unreliable, since missing is not the same as neutral.

**Gaps.**
- Half-life calibration: predict subsequent CU roll-call votes and cosponsorships from statements dated before them, across a grid of half-lives (e.g. 0.25 to 4 years), scored on a held-out Congress.
- Incremental validity of social over press, given ghost-writing: compare press-only and press+social blends on the same outcome.
- Shrinkage calibration: vary the nz threshold (e.g. 5 to 30) and check stability of category cells under resampling.
- Kim, Stuckatz & Wolters (effects fade within 2 to 3 years) has no evidence row.

**Paul's position vs the evidence.** Paul calls for temporal weighting and cites 6-month (Sotoudeh) and 2 to 3 year (Kim, Stuckatz & Wolters) timescales. The verified evidence supports temporal weighting in principle, but shows that the Sotoudeh value is about a different construct and is insensitive to the choice. The current half-lives remain uncalibrated choices, and the coincidence of the 0.5-year social half-life with Sotoudeh's 6 months is not evidence.

**Open questions.**
- Should silence in a category with f = 1 be coded missing rather than zero?
- Is CROSSPOST_DEDUP (off by default) inflating cells for members who post the same text on several platforms?
- What share of the press side of each cell comes from ACU-authored rows?

**Read first.** [[sotoudeh-2024-network-based-measure]], [[hargittai-2015-bigger-always-better]], [[steinertthrelkeld-2018-twitter-data]]

## Legacy CU press regex stance and graded tone scorer

**What it computes.** Per release: CU stance via a regex ladder (default support), graded tone via TONE_LEXICON, and member lean, stance and negativity aggregates.

**Method.** Ladder: oppose > support cues > neutral bill cues > default support. GRADED_CUTS c(1,2,3.5,5.5,8). Polarity (Wp-Wn)/(Wp+Wn) per Rice & Zorn. Over-flagging recorded: 21 of 32 neutrals were wrongly scored. Superseded for stance on 2026-06-21 and still computed as a comparison column.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/test_cu_scoring_offline.R`

**Assumptions.**
- CU-mention sentences carry the release's CU stance
- Default stance is support

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[rice-2018-corpus-based-dictionaries]] | 2018 | method-source | Polarity = (sum Wp - sum Wn)/(sum Wp + sum Wn), with Wp and Wn built from tf-idf and cosine-weighted counts over 500 positive and 500 negative words taken from 300-dimensional GloVe vectors (window 50, minimum count 90) trained on the domain corpus, seeded by 10 positive and 10 negative words. Gains over LIWC and AFINN already appear at 5,000 documents, rise steeply to about 15,000, and show marginal gains after 30,000. | The formula transfers as written. The paper's validity evidence applies only to corpus-derived, similarity-weighted dictionaries, which this code does not build. The paper's own accuracy gains start at 5,000 documents; the CU-relevant set is 1,563 text-bearing releases. Paper says document-level polarity hides subject (p.27), which supports the stance split. | strong | VERIFIED-SOFTENED |
| [[haselmayer-2017-sentiment-analysis-political]] | 2017 | contradicts | Sentence tonality is set by the single most negative dictionary word (max rule). A negator immediately before a word excludes that word, and an intensifier multiplies it by 1.25. Word scores are crowd-derived means on a 0-4 scale. Mean word scores with the max rule and booster/negation handling give r = 0.65 (n = 168 test sentences). Other aggregation combinations give r from 0.38 to 0.58. | This is a fidelity finding, not an empirical one. It matters for the display score_label magnitude and the comparison column only, because press_value and the press pillar do not consume the graded layer. No external claim that these graded labels follow Haselmayer and Jenny should be made. | strong | VERIFIED |

**Assessment.**

**Evidence:** 2 rows (1 VERIFIED, 1 VERIFIED-SOFTENED). Both are fidelity findings: the code does not do what its citations say.

- **haselmayer-2017 (VERIFIED, contradicts, strong).**
  - The graded layer header in `cu_press_release_scraper.R` cites Haselmayer & Jenny and says sentence tonality follows the strongest cue. The code (`weighted_tone`, about lines 2443-2475) sums every fired cue across CU sentences.
  - The paper's max rule with crowd-derived 0-4 word scores gave r = 0.65, while other aggregation rules gave 0.38 to 0.58. TONE_LEXICON is 44 hand-weighted entries (1/2/3), not crowd scores.
  - The code adds a 0.75 downtoner the paper lacks, and its negator window covers two preceding tokens.
- **rice-2018 (method-source, strong).**
  - The polarity formula (Wp − Wn)/(Wp + Wn) transfers as written (line 2799).
  - The paper's validity evidence is for corpus-derived, GloVe-similarity-weighted dictionaries. This code uses hand weights.
  - `expand_tone_lexicon()` departs from the paper on every parameter, and its output file is absent, consistent with never having run.
  - The paper's gains begin at about 5,000 documents. The CU set has 1,563 text-bearing releases.

**Impact:** limited. The graded layer feeds only the display score_label and a comparison column. press_value and the press pillar do not consume it. The 2026-06-21 supersede evidence (21 of 32 neutrals wrongly scored) is UNVERIFIED in the files checked.

**Contradictions.**
- The code header claims Haselmayer & Jenny's strongest-cue rule. The code sums all cues. No external text should say the graded labels follow Haselmayer & Jenny.
- The code cites Rice & Zorn, but uses hand weights rather than corpus-derived similarity weights, so the paper's validity evidence does not cover it.

**Gaps.**
- Party bias of default-support: score the legacy column against human gold stratified by party, and report the over-flagging rate by party with denominators.
- A verifiable record of the 21-of-32 neutral over-flagging that justified the supersede.

**Paul's position vs the evidence.** Paul has no annotated literature here. Rice & Zorn is on disk but unannotated. The evidence mainly says the component's own citations are misapplied. Since it is superseded for stance, the practical action is correcting the header text, not re-validating it.

**Open questions.**
- Is score_label shown to Paul or clients anywhere such that the misattributed method matters?
- Should the comparison column be retired, or kept with a corrected method description?

**Read first.** [[haselmayer-2017-sentiment-analysis-political]], [[rice-2018-corpus-based-dictionaries]], [[ornstein-2024-train-your-stochastic]]

## ACU speaker mode (planned)

**What it computes.** Treats ACU-authored press (816 rows, 243 members) as ACU-to-member friend or foe labels.

**Method.** The NLI hypothesis is branched by speaker_source; keyword extraction was rejected. Validation gate: held-out accuracy >= 0.851, and Marshall/Hawley ACU rows must net negative. Planned 2026-07-01 and blocked on labels.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/ACU_SPEAKER_MODE_PLAN.md`

**Assumptions.**
- ACU endorsing a member means pro-CU; ACU criticising a member means anti-CU

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) |  |  |  |  |  |  |

**Assessment.**

**GAP: no evidence rows.** The component is planned (2026-07-01) and blocked on labels. Its gate is held-out accuracy ≥ 0.851, with Marshall and Hawley ACU rows netting negative.

The papers Paul cites (Box-Steffensmeier et al. 2018, Furnas et al. 2022, Kim J. et al. 2025) have no bearing rows on this component. They also address a different construct. They concern whether group endorsements work as cues that shape receptiveness. They do not concern whether an advocacy group's statements about a member measure that member's stance.

**Cross-component evidence makes the component more urgent.** The frame-alignment bearing (joo-2022, reproduced on a second path) found that 816 of the 2,388 rows in `cu_press_stance_tone.csv` are americascreditunions.org articles. They carry 349 distinct titles, with up to 19 members on one title. ACU-authored text is therefore already attributed to members in the press corpus that feeds frame alignment, and that stance dynamics was re-run on. Whether press_value excludes it was not verified. Until speaker mode or a domain filter exists, ACU's voice is counted as the member's.

**Contradictions.**
- None from evidence. One construct note: Paul's cited cue literature supports treating ACU statements as a separate signal, but as a receptivity cue, not as a measure of the member's position. The component's assumption (ACU endorsing a member means the member is pro-CU) is about measurement, and that is untested.

**Gaps.**
- Predictive-validity study: do ACU statements about a member predict that member's subsequent CU votes and cosponsorships better than the member's own statements, on a held-out Congress with window and denominator stated?
- Label study: a human-coded set of ACU rows, labelled friend / foe / neutral toward the named member, to meet the 0.851 gate. This is the blocking item.
- Attribution audit: which downstream outputs (press_value, stance dynamics, media blend, frame alignment) currently include ACU-authored rows as member speech?

**Paul's position vs the evidence.** Paul's highlights say endorsements from well-connected groups are strong cues and that source identity shapes receptiveness. That supports separating ACU-authored statements from member speech, which the frame-alignment evidence shows is needed now. It does not show that ACU statements measure member stance; that needs the predictive-validity study.

**Open questions.**
- Should ACU-domain rows be excluded from member-level stance outputs before speaker mode is built?
- Who produces the ACU friend/foe labels, and on what sample size?

**Read first.** [[joo-2022-image-data-automated]], [[kim-2025-measuring-interest-group]], [[ornstein-2024-train-your-stochastic]]

## Stance dynamics and neighbour imputation

**What it computes.** Quarterly (91-day) stance trajectories in seven classes, plus imputation proposals for missing stances from network neighbours.

**Method.** Static band = median + MAD over observed-to-observed transitions (floor .05). Imputation needs at least 5 neighbours and |estimate| >= 0.75; 307 proposals; masking validation gives 77% sign agreement. Proposals only; the matrix is not changed. Co-movement was null on the cosponsor and HCoI layers.

**Code refs.** `fec/stance_dynamics.py`

**Assumptions.**
- Stance is locally similar among network neighbours (co-movement test was null)

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[zareie-2020-similarity-based-link]] | 2020 | suggests-extension | Many existing edges join nodes with no common neighbour: 53.7% in YST, falling to 3.1% in UAL. Correlation of second-order neighbourhood vectors still separates test from non-existing edges for such pairs, and DICN is best in 5 of 9 networks there. | The paper predicts edges, not node attributes, so at most it supports a second-order similarity weight for choosing which neighbours to average. Any second-order variant is a proposal that must beat a party-only baseline on masked cells. Weak. | weak | VERIFIED-SOFTENED |
| [[baptista-2022-universal-multilayer-network]] | 2022 | suggests-extension | RWR propagates proximity from seeds over several hops and across layers and is validated by LOOCV. Combining layers helped in the gene-disease task: gene multiplex plus disease monoplex beat PPI alone and the gene multiplex alone. The drug layer with low bipartite overlap added nothing, while the airport layer with high overlap helped. The paper does not state that layers help only when overlap is high in the gene-disease case. | A per-member RWR seeded on known stances could replace the one-hop mean, and the existing masking protocol already has the form of LOOCV. Neighbour co-movement was null, so a multi-hop walk may not help either. This is a candidate needing a prereg and a single look, not evidence. Stance labels are Paul's domain: proposals only, and the matrix is never edited. | weak | VERIFIED-SOFTENED |
| [[malaterre-2024-inferring-social-networks]] | 2024 | supports | HCoI ties are links from thematic similarity of texts, mediated by the texts rather than social ties. The paper does not discuss stance, agreement or change in agreement. | The paper's silence on stance supports keeping HCoI out of stance imputation only as an argument from the construct, not from the paper's evidence. The null result needs a labelled layer in the output before it is cited. | moderate | VERIFIED-SOFTENED |
| [[zhou-2026-measuring-stance-dynamics]] | 2026 | method-source | Stance is tracked as a trajectory over 28-day snapshots, with embedding persistence so silent users keep a stance. Movement is sorted into seven dynamics using a static band of median plus MAD. Mirror-image O-to-S and S-to-O flip curves suggest external events. Users with no direct stance are predicted through graph propagation over their neighbours. | The design vocabulary transfers. The paper says the mirror symmetry suggests external events; it does not establish an event detector the module can rely on. The paper's neighbour inference is GNN message passing over learned embeddings, not a copy rule, so the system's neighbour-based imputation is an analogue only. | strong | VERIFIED-SOFTENED |
| [[zhou-2026-measuring-stance-dynamics]] | 2026 | qualifies | The median plus MAD static band is applied to continuous stance values p in [-1,1] from the MLP, computed over all users' absolute changes. The paper gives no condition that users must post in nearly every period; its 10-tweet filter only implies about one interaction per snapshot on average. | The band means what the paper intends only where stance is continuous and densely observed. With pooled quarterly signs it mainly measures how coarse the labels are. The paper gives no support for the seven-class counts. The within-side classes are rare because a move must exceed about 0.71 on a [-1,1] scale; they are not empty by construction. | strong | VERIFIED-SOFTENED |
| [[zhou-2026-measuring-stance-dynamics]] | 2026 | qualifies | Neighbour co-movement: users shift in the same direction as their 1-hop and 2-hop neighbours (diagonal dominance in Fig. 6). The authors describe tight-knit communities moving together, call it a descriptive correlation, and report no null model and no identification. The text gives no numbers; values appear only in the heatmaps. | The paper's result is a descriptive correlation at 28-day resolution on Twitter, not evidence of influence. Do not carry the paper's potential social influences wording or its asymmetric social influence signals claim into system text. A null on one layer is a null on that layer only. | moderate | VERIFIED-SOFTENED |
| [[zhou-2026-measuring-stance-dynamics]] | 2026 | qualifies | Silent-user inference is validated with 5-fold cross-validation over annotated users. For validation users, all edges to referendum-labelled hashtags are removed before training and inference. Scoring is macro-F1 per period on that period's validation users, chosen over accuracy because of class imbalance. Reported TWLGCN levels are steady between 70 and 90 percent. | The paper's protocol retrains a model with masked own-edges and reports macro-F1 across folds. The system's check is a neighbour-only estimate scored by raw sign agreement on a confidence-selected subset. Call it an analogue. The 77.5 percent is not comparable to the paper's macro-F1 range and stays UNVERIFIED (DE-062) until a party or majority baseline is run. | strong | VERIFIED-SOFTENED |
| [[zhou-2026-measuring-stance-dynamics]] | 2026 | qualifies | Carrying learned embeddings and MLP weights forward between snapshots beats re-initialising each period, by 1.3 to 9.8 percent on Exit_DS. The 28-day window was chosen by a sensitivity analysis on giant-component fraction: shorter windows fragment the graph and longer ones mask rapid shifts. | The paper's persistence gain is for warm-started learned representations that keep updating from new edges. It does not support freezing a categorical label. The window rule can be borrowed, but it needs per-window graphs the press corpus does not supply. Fig. S2 is in the supplement and was not checked. | moderate | VERIFIED |
| [[zhan-2025-cost-minimized-stage]] | 2025 | suggests-extension | Missing preference entries are imputed from the K=4 most trusted neighbours with a known value. Each weight blends trust and preference similarity (mu = (t + sim)/(1 + sim), then normalized), and entries are filled in order of reliability r and iterated to convergence. In the worked case f10_45 = 0.4033 comes from 4 neighbours weighted 0.2549/0.2507/0.2503/0.2440, the highest-reliability gap (r = 3.8683) of 39 missing entries. Imputation MSE stays at 0.09-0.13 across p in {20..100}, n in {4..12} and missing rates 0.1/0.2/0.3, on synthetic data only. | Only the mechanics carry over: neighbour weights from tie strength plus profile similarity, and filling the best-supported gaps first. The paper's evidence does not transfer. Its MSE comes from random synthetic matrices; its trust network is elicited trust among decision-makers, not cosponsorship; its alternatives are 5 investment options, not legislators' CU stance categories. Its homophily premise is assumed, never tested, while this system measured co-movement as null on cosponsor ties. A weighted variant should be adopted only if a masked-cell test beats both the current unweighted mean and a party-only baseline on held-out members. Until a second path reproduces that result it stays UNVERIFIED (DE-062). Weights must be labelled heuristic tie strength, never trust or closeness (D-025). | weak | UNVERIFIED |
| [[cao-2025-social-network-group]] | 2025 | context | SNGDM consensus mechanisms model opinion updating along trust edges with DeGroot and Hegselmann-Krause dynamics (refs 9 and 71). The review treats network-driven opinion convergence as the core mechanism but reports no test of it. | DeGroot assumes that neighbours' opinions move a node's opinion. That holds by design in elicited SNGDM panels. The system's own null co-movement result means the assumption is not supported on congressional layers. This paper adds no evidence either way, so the imputation stays a proposal-only heuristic. | weak | UNVERIFIED |
| [[ma-2022-adversarial-attack-graph]] | 2022 | suggests-extension | Perturbing features of only 1 percent of nodes, chosen by an influence-maximization criterion, sharply lowers accuracy of GCN, GAT and JK-Net. Cora GAT falls from 87.7 (no attack) to 55.9 under InfMax-Unif at the 10 percent degree threshold. The paper's model (B = M^L) implies that propagation lets a few nodes affect many predictions; no propagation ablation is reported. | The system has no GNN and uses a one-hop mean with human adjudication, so propagation is shallow and an adversary is implausible. The transferable idea is a sensitivity test: flip or corrupt the labels of a few high-coverage members, chosen by walk coverage rather than at random, and count sign changes in the proposals. Any result stays UNVERIFIED until reproduced (DE-062). Weak. | weak | VERIFIED-SOFTENED |
| [[ringe-2013-pinpointing-powerful-voting]] |  | qualifies | The connectivity to agreement link depends on whether the pair are expected allies or adversaries (bloc). Same-bloc dyads co-vote in 574 of 594 cases (about 97 percent), so H2 is untestable. For opposed-bloc pairs more contact predicts less co-voting (p. 625; Table 3). | The paper outcome is votes, not stance labels, so transfer is conceptual. The next diagnostic is a party-only baseline and a split of same-party from cross-party neighbours. The 77 percent figure stays UNVERIFIED as a network effect until that baseline exists (DE-062). | moderate | VERIFIED |
| [[selfhout-2010-emerging-late-adolescent]] | 2010 | method-source | Stochastic actor-oriented models estimate tie selection conditional on the current network state, with ego, alter and similarity terms for each covariate, and pool groups by multilevel meta-analysis. The cited extensions (Steglich et al. 2006; Burk et al. 2007) model networks and behaviour together, which separates selection from influence. | Transfer is moderate in method only. The paper itself models selection and not influence, so it gives no evidence on stance contagion. A joint network-and-behaviour SIENA model is the standard way to ask whether ties drive stance change or stance similarity drives ties. That would need panel waves of cosponsor ties and quarterly stances. It would be new analysis, UNVERIFIED until reproduced, and its pooled estimates must be reported by stratum (party, chamber) or marked UNSTRATIFIED. | weak | UNVERIFIED |

**Assessment.**

**Evidence:** 13 rows (2 VERIFIED, 8 VERIFIED-SOFTENED, 3 UNVERIFIED). This is the best-evidenced component as a design transplant and the weakest as a validated measurement.

**Design vocabulary transfers (zhou-2026, method-source, strong).** Trajectories, a median+MAD static band, seven dynamics, flip curves and silent-user inference. The qualifying rows show where the port departs:

- **Band on coarse labels.** The band was built for continuous p in [−1, 1] from an MLP over densely observed users. Here it is applied to pooled quarterly signs, giving static_band 0.7076. Of 2,989 transitions, 2,737 (91.6%) are static. With coarse signs the band mainly measures label coarseness.
- **Window not tuned.** The 91-day window rests on an analogy comment. Zhou chose 28 days by sensitivity analysis (VERIFIED row).
- **Frozen labels counted as transitions.** Persistence carries a frozen raw label, not a warm-started embedding. Carried windows enter transition counts, which inflates static counts and credits flips to the window where posting resumes (VERIFIED row).
- **Imputation validation is not comparable.** Zhou retrains with masked edges and reports macro-F1 per fold (70 to 90%). The system scores raw sign agreement on confident cells: 77.5% on n = 1,087, with no party or majority baseline. True Neutral (score 0) is counted as a negative sign (line 143), and the final print says 3 neighbours while the threshold is 5 (line 152).

**Co-movement is null on both layers measured.** Cosponsor: 134 same / 122 opposite (52.3%). HCoI re-run: 302 / 296 (50.5%). The 49% HCoI figure is not stored in `stance_dynamics.json`. Zhou's own co-movement is descriptive, with no null model. Its "social influence" wording must not be carried into system text.

**ringe-2013-pinpointing-powerful-voting (VERIFIED).** Whether contact predicts agreement depends on bloc. Same-bloc dyads co-vote in about 97% of cases (574 of 594). So the next diagnostic is a party-only baseline and a same-party vs cross-party neighbour split. Cosponsor ties are largely within-party, so 77.5% may be party homophily, not a network effect.

**malaterre-2024.** HCoI ties are thematic text similarity, not social ties.

**Leads (UNVERIFIED or weak).**
- zhan-2025: trust- and similarity-weighted K-neighbour filling. Any weight must be labelled heuristic tie strength (D-025).
- cao-2025: DeGroot updating, an assumption the null contradicts.
- selfhout-2010: a SIENA network-behaviour model to separate selection from influence. It would need panel waves, and estimates must be stratified by party and chamber or marked UNSTRATIFIED.
- baptista-2022: RWR in place of the one-hop mean.
- zareie-2020: second-order similarity.
- ma-2022: a label-corruption sensitivity test.

**Contradictions.**
- The imputation premise (stance is locally similar among network neighbours) is contradicted by the system's own null co-movement on both cosponsor (52.3%) and HCoI (50.5% re-run) layers. The component's assumption text already notes this.
- The cited HCoI co-movement figure of 49% is not in the stored `stance_dynamics.json`. The re-run gives 50.5%. The stored co_movement block has no layer label.
- Masking code treats True Neutral as a negative sign (line 143), which biases the 77.5% sign-agreement figure in an unknown direction.
- Code prints "at least 3 informative neighbors" while SD_MINNB = 5 (line 152).
- Zhou's persistence gain is for warm-started learned embeddings. It does not support freezing a categorical label and counting carried windows as transitions.

**Gaps.**
- A party-only (and majority-class) baseline for the masked-cell test, with macro-F1. Until this exists the 77.5% is not evidence of a network effect.
- Window sensitivity (e.g. 28, 56, 91, 182 days) with transitions counted only between observed windows.
- A permutation or common-shock null for co-movement (shuffle neighbour identities within party and chamber).
- Selection vs influence: a SIENA-style joint model of cosponsor ties and quarterly stance, stratified by party and chamber. This is new analysis and needs a prereg.
- Bacik et al. 2026 and Fink et al. 2023 are cited in Paul's position without evidence rows.

**Paul's position vs the evidence.** Paul wants temporal dynamics and change-likelihood modeling. The verified evidence supports the trajectory design but not the network-based change or propagation idea: co-movement is null on both layers, and his notes predate that result. The imputation should remain proposals-only (as it is), described as a party-confounded heuristic until it beats a party baseline. Weighting or multi-hop variants (Zhan, Baptista) are leads, not recommendations. Their weights would be heuristic model outputs, never trust, closeness or influence.

**Open questions.**
- Does the 77.5% masked sign agreement exceed a party-only baseline on the same 1,087 cells?
- Are the 2,388 press rows in the stance-dynamics run filtered for ACU-authored items? 816 of them are ACU syndication.
- Which layer does the stored co_movement block (134 / 122) correspond to?
- Should True Neutral be excluded from sign agreement rather than counted as negative?

**Read first.** [[zhou-2026-measuring-stance-dynamics]], [[ringe-2013-pinpointing-powerful-voting]], [[malaterre-2024-inferring-social-networks]], [[selfhout-2010-emerging-late-adolescent]], [[baptista-2022-universal-multilayer-network]]

## Frame alignment (cross-member duplication rate)

**What it computes.** Per-category share of near-duplicate text across members, plus party framing chi-square.

**Method.** 3-gram shingle Jaccard >= 0.5. Reported duplication: CU Tax 65%, General CU 60%, Defense 12%. It feeds salience intensity (0.5 + alignment).

**Code refs.** `fec/frame_alignment.py`

**Assumptions.**
- High shingle overlap indicates coordinated messaging

**Evidence table.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[joo-2022-image-data-automated]] | 2022 | method-source | Framing = share of duplicate images within each event-label; two images are duplicates when the normalized distance between their 1,000-dim last fully connected layer vectors is below .2. Table 3: Venezuela state violence .308 vs Hong Kong contains-child .003 (paper: almost 103 times). The paper frames this as measuring which frames protesters choose to share. | Carries over only when the duplicated unit was chosen by the actor being scored, as Joo's sharers chose their images; here that means own-domain releases. Third-party ACU syndication measures the author's reach, not member coordination. Joo's frame labels come from a trained classifier; here they are press categories, and short titles under 3-gram Jaccard >= 0.5 are not comparable to a CNN-feature distance threshold. | strong | VERIFIED-SOFTENED |
| [[joo-2022-image-data-automated]] | 2022 | qualifies | The .2 duplicate threshold was chosen from inspecting the distance histogram, with no sensitivity analysis reported. The authors call the results provisional and suggest other operationalizations such as number of tweets containing a label instead of percentage, and expanding the frames considered. | Applies directly. Before any alignment figure is quoted, sweep the threshold (e.g. 0.3-0.7) on own-domain rows. The paper supplies no threshold validation the system could borrow. | moderate | VERIFIED |
| [[joo-2022-image-data-automated]] | 2022 | qualifies | Chi-squared tests of label against party are run separately on male and female images because the two parties have different gender ratios. Labels are sorted by chi-square with per-label significance stars; no multiple-comparison correction is described across the 1,730 unique labels. | Joo's design stratifies on a known compositional fault line. Here the fault lines are chamber and author domain. Until stratified, quote the chi-square figures as UNSTRATIFIED descriptive term lists, not party-framing findings. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence:** 3 rows (1 VERIFIED, 2 VERIFIED-SOFTENED). This is the most consequential correction in the system.

- **Method-source, strong, reproduced on a second path.** Joo's duplication rate measures frames that actors chose to share. The port scores 3-gram title Jaccard ≥ 0.5 across members over the full `cu_press_stance_tone.csv` (2,388 rows, file dated 2026-06-30), with no domain filter. 816 rows are ACU articles with 816 URLs but only 349 distinct titles. Restricting to own-domain (*.senate.gov / *.house.gov) items:
  - General Credit Union: 60.2% of 442 → **5.2% of 116**.
  - CU Tax Status: 64.7% of 190 → **38.1% of 42**.
  - Defense: 12.2% of 41 → 13.9% of 36.
  - ACU-only rows: 79.8% of 326 (General) and 72.3% of 148 (Tax).
  
  The reported 60% and 65% headline rates mainly measure ACU's reach, not member coordination. They feed salience intensity (0.5 + alignment), so CU-category salience is inflated.
- **VERIFIED, qualifies, moderate.** Joo's 0.2 threshold came from inspecting a histogram, with no sensitivity analysis, and the authors call the results provisional. The system's Jaccard 0.5 inherits an untested cut, and echo clusters use the same 0.5. Sweep 0.3 to 0.7 on own-domain rows before quoting any rate.
- **Qualifies, moderate.** Joo stratifies its party chi-square by image gender because of a compositional fault line. The system's party-framing chi-square pools chamber (1,231 Senate / 1,157 House) and author domain (34% ACU), with no correction. The brain's "D to CU-Bank Dynamics (chi-square = 128)" carries no stratum or window and must be quoted as an UNSTRATIFIED descriptive term list.

**Weight:** strong and negative for the current figures. The template is sound, and the port broke its transfer condition.

**Contradictions.**
- The reported duplication rates (CU Tax 65%, General CU 60%) are contradicted by the own-domain recompute (38.1% and 5.2%). Most of the measured alignment is one ACU headline attributed to many members.
- Party-framing chi-square figures are pooled across chamber and author domain. Joo's own design stratifies on a known fault line.

**Gaps.**
- Predictive validity: does own-domain coordinated messaging predict collective cosponsorship of the same bill? No evidence row addresses it.
- Threshold sensitivity on own-domain rows (Jaccard 0.3 to 0.7), with the rate reported at each cut.
- A stratified chi-square by chamber and author domain, with multiple-comparison control.

**Paul's position vs the evidence.** Paul takes Joo's duplication rate as the template, and the evidence supports the template. The verified recompute shows the implementation violates Joo's key condition, that the duplicated unit be chosen by the actor being scored. Own-domain releases meet it; ACU syndication does not. His margin note ('Monitoring tool?') concerns Dietrich's aisle-crossing video, not this metric.

**Open questions.**
- Should salience intensity (0.5 + alignment) be recomputed on own-domain alignment only?
- Is the 38.1% own-domain CU Tax rate itself robust to the Jaccard threshold?

**Read first.** [[joo-2022-image-data-automated]], [[steinertthrelkeld-2018-twitter-data]], [[malaterre-2024-inferring-social-networks]]

## Across components

## Cross-component patterns

**1. ACU syndication contaminates member speech.** 816 of the 2,388 rows (34%) in `cu_press_stance_tone.csv` are americascreditunions.org articles attributed to members: 349 distinct titles, with up to 19 members on one title. This is verified for frame alignment, where own-domain duplication falls to 5.2% for General CU and 38.1% for CU Tax. It feeds salience through 0.5 + alignment. Stance dynamics was re-run on the same 2,388 rows. Whether press_value, the media blend's press side and the party chi-square exclude these rows was not verified.

ACU speaker mode, the designed fix, is blocked on labels. A domain filter is a smaller interim step, and the ruling on it is Paul's.

**2. Accuracy rests on small single looks.** Every accuracy claim comes from the project's own single looks on small sets: n = 87, 400 tweet golds, 1,087 masked cells. The external papers supply no transferable benchmark: Kim's 96.9%, b-2025's 0.97 and Ornstein's 88.4% are all task-mismatched. Two figures need resolving before either is quoted:
- The DEBATE 0.839 and the SVM 83.9% on the same n = 87 may be one number attributed twice.
- Two bearings disagree on whether the two-hop 0.782 is out-of-fold.

**3. Gold labels are partly circular.**
- Claude pre-labels seed the human-overridden training set, and the reported 87.2% agreement is with Claude.
- The valence registry is derived from Paul's golds and scored on golds the design iterated against.
- Human-overlaid rows are correctly excluded from DEBATE accuracy. Their share of member scores is unmeasured.

**4. The neutral bin pools unlike things.** DEBATE neutral covers both off-topic and neutral. Coded or implicit support may land there (b-2025 error mode; anon-nd lead). Claude ERR fallbacks are neutral with confidence 0.0. Stance-dynamics masking counts True Neutral as a negative sign. Each component treats zero differently, and the media blend treats silence as zero.

**5. Party is an unstratified fault line.** CFPB tiers (98 D / 0 R after isolation), the party-framing chi-square (pooled across chamber and author domain) and neighbour imputation (cosponsor ties are largely within-party; no party baseline) all report pooled figures. Per ringe-2013-pinpointing-powerful-voting and poole-1999, the informative tests are within-party. Until stratified, every such figure carries the UNSTRATIFIED label.

**6. Generic instruments on domain text.** LSD2015 scores 'credit' as positive and risk/charge/oversight/fraud as negative on CU text (rice-2018, verified entries). There is no human intensity gold (haselmayer-2017 protocol), and the brain calls LSD validated on a three-case smoke test. The intensity scales every press_value cell.

**7. Uncalibrated time constants.** Press 2.0 years, social 0.5 years, the R scraper's 2-year half-life with a 2022 floor, and the 91-day dynamics window. Neither of Paul's decay sources supports these values. Sotoudeh's 6 months is about cosponsorship capital and is flat across 6 to 24 months. Zhou's 28 days came from a sensitivity analysis the system has not run.

**8. Citation-fidelity errors.**
- The legacy graded-tone header claims the Haselmayer & Jenny max rule, but the code sums cues.
- Rice & Zorn's validity evidence is for corpus-derived weights the code never built.
- The registry's min_consist is 0.75 in code vs 80% in the header.
- The imputation print message says 3 neighbours vs a threshold of 5.
- The cited `stage5_debate.py` shows only a zero-shot base pipeline.

**9. Flags.**
- Fink et al. 2023 appears in Paul's positions for two-hop, media blend and stance dynamics. In the system's citations it supports author-volume normalized social and press edges, the per-edge volume normalization measured and REJECTED in this project. FLAGGED, not recommended.
- Zhou's "social influence" language must not be imported.
- Any neighbour weighting (zhan-2025, baptista-2022) is a heuristic model output, never trust, closeness or access (D-025).
- Reciprocity is not validated (D-104..D-108), so no symmetric-tie assumption should enter imputation.
- LDA-derived positions (kim-2025) may audit bill directions but are never member edges or weights (D-021).

## Research agenda

1. Author-domain audit (no new model, Paul rules on any filter): recompute press_value, frame alignment, stance dynamics and the party chi-square with ACU-domain rows separated, reporting own-domain vs ACU figures with window (CSV dated 2026-06-30) and denominators. This answers the largest verified distortion first.
2. LSD intensity validation (Haselmayer protocol): about 200 CU releases rated 0 to 4 by lay and expert coders. Correlate with |lsd_net_norm| and lsd_charge, with and without masking the credit/charge/risk/oversight/fraud families. Prereg, single look. Then test whether sign × intensity predicts subsequent CU votes better than sign alone.
3. Party baseline for neighbour imputation: on the same 1,087 masked cells, compare the cosponsor-neighbour mean with party-only and majority-class baselines. Use macro-F1, split same-party from cross-party neighbours, and exclude True Neutral from sign agreement. Until this is done, the 77.5% stays UNVERIFIED as a network effect (DE-062).
4. Resolve the reader accuracy ledger: confirm whether the DEBATE 0.839 and the SVM 83.9% on n = 87 are distinct measurements. Run a paired McNemar test of SVM vs fine-tuned DEBATE vs Claude zero-shot on the untouched set. Run a blind human-only re-label sample to measure distillation bias toward the Claude pre-labeller.
5. Two-hop out-of-sample check: settle whether 0.782 / F1 0.754 is out-of-fold. Then score the gold-derived registry once on a fresh, never-iterated tweet gold set against the 0.677 flat baseline, and re-derive the registry on a later Congress to measure valence stability.
6. Half-life and blend calibration: grid press and social half-lives (about 0.25 to 4 years), score prediction of subsequent CU roll calls and cosponsorships on a held-out Congress, and test press+social vs press-only. Code social silence as missing vs zero, and vary the nz/15 shrinkage threshold.
7. CFPB within-party test: among Democrats only, stratified by chamber, check whether the CFPB-negative tier predicts CFPB-related roll-call votes. Hand-code a CFPB family sample (restraint, expansion, institution, general) to check the policy_force router. Report any pooled figure as UNSTRATIFIED.
8. Frame-alignment robustness: sweep Jaccard 0.3 to 0.7 on own-domain rows only. Recompute salience intensity (0.5 + alignment) on own-domain alignment. Stratify the party chi-square by chamber and author domain, with a multiple-comparison correction.
9. Stance-dynamics window sensitivity: run 28-, 56-, 91- and 182-day windows, count transitions only between observed windows, and add a within-party permutation null for co-movement. A SIENA-style selection vs influence model is a later, prereg-only option.
10. Bill-sign aboutness guard: run a single held-out gold look at a sentence-restricted direction rule and at a title or lead-sentence prominence gate. Audit the 60-row direction table against Kim's 111th to 117th Congress positions, to adjudicate only, never to overwrite.
11. ACU speaker-mode labels: build the friend/foe/neutral label set needed for the 0.851 gate. Then test whether ACU statements about members predict members' subsequent CU votes better than the members' own statements.
12. Documentation fidelity fixes (owner-approved edits only): correct the legacy graded-tone header (sum, not max rule), the brain's 'LSD validated' wording, the min_consist 0.75 vs 80% header mismatch, the imputation print message (5, not 3), the unlabeled co_movement block, and the provenance of the fine-tuned DEBATE weights.
