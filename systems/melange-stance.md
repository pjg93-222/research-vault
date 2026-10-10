---
type: system
name: "Melange stance and tone measurement (press and social text)"
slug: melange-stance
components:
  - melange-stance.debate-nli-classifier
  - melange-stance.supervised-stance-stack
  - melange-stance.lsd-tone
  - melange-stance.press-value-aggregate
  - melange-stance.two-hop-decomposition
  - melange-stance.bill-sign-handling
  - melange-stance.cfpb-axis-isolation
  - melange-stance.media-blend
  - melange-stance.legacy-regex-scorer
  - melange-stance.acu-speaker-mode
  - melange-stance.stance-dynamics
  - melange-stance.frame-alignment
brain_notes:
  - C:/Users/pgoff/Documents/brain/actors/debate-stance-model.md
  - C:/Users/pgoff/Documents/brain/concepts/two-hop-stance-decomposition.md
  - C:/Users/pgoff/Documents/brain/topics/2026-06-feature-cu-stance-and-sentiment.md
  - C:/Users/pgoff/Documents/brain/topics/2026-06-feature-cu-stance-and-sentiment-stance-stack-design-and-accuracy.md
  - C:/Users/pgoff/Documents/brain/topics/2026-07-feature-influence-engine-internals-salience-sna-wave.md
paper_count: 31
tags: [system]
---

# Melange stance and tone measurement (press and social text)

Turns member press releases, tweets and other posts into signed CU stance and tone. Covers the Political DEBATE NLI stance reader, Lexicoder tone, two-hop stance x valence composition, bill-sign handling, CFPB axis isolation, human-label overlay, the media blend with recency decay, ACU speaker mode, stance dynamics and frame alignment. It includes the legacy Draft Systems R regex scorer (Stage 5).

Brain notes: [actors/debate-stance-model](obsidian://open?vault=brain&file=actors%2Fdebate-stance-model); [concepts/two-hop-stance-decomposition](obsidian://open?vault=brain&file=concepts%2Ftwo-hop-stance-decomposition); [topics/2026-06-feature-cu-stance-and-sentiment](obsidian://open?vault=brain&file=topics%2F2026-06-feature-cu-stance-and-sentiment); [topics/2026-06-feature-cu-stance-and-sentiment-stance-stack-design-and-accuracy](obsidian://open?vault=brain&file=topics%2F2026-06-feature-cu-stance-and-sentiment-stance-stack-design-and-accuracy); [topics/2026-07-feature-influence-engine-internals-salience-sna-wave](obsidian://open?vault=brain&file=topics%2F2026-07-feature-influence-engine-internals-salience-sna-wave)

**Scope.** This covers 12 components, 56 evidence rows and 31 distinct papers. Of the rows, 8 are VERIFIED, 39 VERIFIED-SOFTENED and 9 UNVERIFIED. Source: `_work/state/hub/melange-stance.json`, read 2026-10-10. An online scan (2026-10-10) added unread leads, kept separate from the evidence rows. Two open-access copies were downloaded to `C:/Users/pgoff/Documents/research/_work/downloads/scan_2026-10-10/`:
- `burnham_2024_political_debate/2409.02078.pdf`: the Political DEBATE model paper itself.
- `lemens_gallego_2025_asking_averaging/2311.16639.pdf`: Le Mens & Gallego, asking-and-averaging LLM positions for 117th-Congress senators.

**Bottom line.** No paper validates a headline number this system reports. Every accuracy figure comes from the project's own single-look tests on small sets:
- DEBATE: 0.839 accuracy, kappa 0.742, n = 87 held out.
- SVM: 83.9% accuracy, macro-F1 0.816, same n = 87.
- Two-hop: 0.782 on 400 tweet golds.
- Masking sign agreement: 77.5% on 1,087 confidence-selected cells.

The literature does three things:
1. **It explains failure modes.** Implicit or indirect stance is read as neutral. Dictionaries are blind to context. LLMs over-infer outside knowledge.
2. **It supplies templates.** Haselmayer & Jenny's validation protocol, Joo's duplication rate, Zhou's trajectory design and Park's direction pairs.
3. **It shows several borrowed numbers do not transfer.**
   - Kim 2025's 96.9% comes from keyword-prefiltered lobbying lines with coder disagreements dropped.
   - Sotoudeh's 6-month half-life is a cosponsorship-capital decay, not a stance decay.
   - b-2025's transformer > SVM ordering is scored on different test sizes, 1,675 vs 6,699 rows.

**Strongest verified findings.** These are the system's own measurements, reproduced on a second path:
1. **Frame alignment is mostly American's Credit Unions (ACU) syndication.** Window: the whole `cu_press_stance_tone.csv` (2,388 rows, file dated 2026-06-30). General CU duplication is 60.2% of 442 items, but 5.2% of the 116 own-domain items. CU Tax Status is 64.7% of 190 but 38.1% of 42.
2. **Neighbour co-movement is null on both layers.** Cosponsor: 134 same vs 122 opposite (52.3%). HCoI re-run: 302 vs 296 (50.5%). Both are below the 0.55 threshold, with no permutation control.
3. **The legacy graded-tone header misstates its method.** It cites Haselmayer & Jenny's max rule, but the code sums every cue.

**Errors found.**
- The legacy header (point 3 above).
- `claude_stance.py` keeps the model's stated confidence on rows whose label was coerced to neutral (line 92).
- The stance_dynamics print says "at least 3 neighbours" where the threshold is 5 (line 152).
- The `derive_valence_registry.py` header says 80% consistency where the code uses 0.75.
- Persistence carries a frozen categorical label forward and counts the carried windows as static transitions. This inflates the static share (91.6% of 2,989 transitions).
- A scan lead, not checked against a fine-tune: Political DEBATE was trained on stance hypotheses of the form "The author of this document supports (stance)", and its authors recommend hypotheses modelled on the training templates. `stage5_debate.py` uses "This text is {}." This may partly explain the 53% zero-shot baseline (156-row test).

**Gap.** ACU speaker mode has no evidence rows. CFPB axis isolation has one row (Poole 1999), and it bears on the party confound, not on the method.

**Flags.**
- Fink et al. 2023 is cited in this system for author-volume-normalized social and press edges. That is the per-edge volume normalization this project measured and REJECTED. It is flagged and recommended nowhere.
- Neighbour and tie weights are heuristic model outputs, never closeness or access (D-025).
- Pooled party chi-squares and CFPB D/R tier counts carry the UNSTRATIFIED label.
- UNVERIFIED rows and scan leads are single-agent leads, not findings (DE-062).

## Fine-tuned Political DEBATE NLI stance reader

**What it computes.** Per-release (and per-tweet) stance toward credit-union interests: support, oppose or neutral (sign -1/0/+1).

**Method.** mlburnham/Political_DEBATE_base_v1.0 fine-tuned on cu_stance_label_set.csv (552 rows; seed 42; 3 epochs). A Claude rater pre-labelled and human labels override. CPU, about 0.3 s per document. Reported results: 87.2% vs Claude (distillation); about 85% / kappa 0.76 on a member-grouped human-gold test; ft accuracy 0.839 / kappa 0.742 held out; 0.796 on the 760-row gold frame; 0.571 on the adversarial goldeval frame. It is one of five readers in the stacked press stack and the fallback reader in build_media_blend.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_debate.py`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/finetune_debate.py`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/claude_stance.py`, `press_stance_stack.py; tweet_stance_stack.py`

**Assumptions.**
- Policy stance toward CUs is recoverable from text by NLI entailment
- DEBATE neutral means the text is not about the bill, not a missed stance
- The held-out test sets (87 or 156 rows) are representative

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2025-measuring-interest-group]] | 2025 | qualifies | GPT-4 zero-shot labelling at temperature 0 reaches 96.93% accuracy on 391 coder-agreed lobbying lines. The lines pass a keyword filter for position terms first (e.g. endorse, defeat, advocate, watch). | Does not transfer. Kim's text is pre-filtered to explicit position terms about a named bill, and the validation set excludes coder disagreements. Press stance toward CUs needs a target-valence step that Kim's task lacks. The 96.9% is not a benchmark for this reader. | weak | stage5_debate.py runs Political DEBATE zero-shot NLI to score each press release's stance toward credit unions (support, oppose and neutral hypotheses). Audit documents (not the code) report the fine-tuned reader at 0.839 accuracy on a held-out human-gold test (n=87) and 0.571 on the adversarial frame. |
| [[dai-2025-large-language-model]] | 2025 | suggests-extension | Fusing decisions from three LLMs (GPT-3.5-turbo-0301, Claude-2-100k, LLaMA-2-70b-chat) by attention-based routing (Eqs 7-11) beats a single-GPT-3.5 variant: 81.0 vs 75.2 on zero-shot VAST with 100 percent training data (+5.8), and 80.2 vs 74.9 with 10 percent (+5.3). The authors say LLMs often exhibit bias in knowledge elicitation that varies with training data and methods. | The paper's ablation confounds fusion with the number of LLMs (one versus three), so it does not isolate a fusion effect, and no variance is reported. The benchmark inputs are tweets for SEM16 and ISD, while the paper does not describe VAST's source text and gives no collection dates, so a 2016-2020 framing is unsupported. The named model snapshots are the paper's choices; their retirement is not checked here. A second LLM rater would expose labeller-specific bias before human override, but the evidence is weak and new agent output stays UNVERIFIED (DE-062). | weak | stage5_debate.py is zero-shot: the Political DEBATE base model (mlburnham/Political_DEBATE_base_v1.0) runs through a zero-shot-classification pipeline with hypothesis template 'This text is {}.' (line 31), and no training code appears in the file. claude_stance.py defaults to claude-sonnet-4-6 (line 32) and asks for a reason of at most 15 words (line 25). Its docstring calls the ensemble member a fine-tuned DEBATE and says the Claude pre-labels are corrected by a person. The 552-label training set and 87.2 percent agreement are not in the cited files. |
| [[b-2025-comparing-fine-tuned]] | 2025 | qualifies | A fine-tuned RoBERTa reaches 0.97 accuracy on political stance. Misclassification concentrates in sarcastic, ambiguous and indirect statements, and some disagree-labelled items are predicted unrelated when explicit negation is absent. | The error-mode finding (implicit and indirect stance is the residual failure) transfers and matches the system's neutral versus support seam. The 0.97 headline should not be a target or prior because it comes from a non-common evaluation set. The DEBATE reader is an NLI entailment model, not a RoBERTa classifier, so the architectures differ. Treat the paper as weak qualitative support only. | weak | The production transformer reader is Political DEBATE, an NLI-head model fine-tuned on about 552 labelled releases for 3 epochs. Per the analyst it scores 0.839 accuracy and kappa 0.742 held out, 0.571 on an adversarial frame, and the dominant error is implicit support read as neutral (n=78). |
| [[taherdoost-2023-artificial-intelligence-sentiment]] | 2023 | context | The paper asserts that AI understands the tone of a statement rather than just recognising words. Citing Alantari et al. 2022, it says neural-network ML gives the most accurate predictions of the text-based methods compared. The review reports no accuracy figures of its own, and its corpus is 43 business and competitive-research papers from 2012-2022. | Weak. The cited evidence covers consumer reviews and business social media, not congressional press releases. It also concerns sentiment polarity, not target-specific stance toward CU policy interests. It neither supports nor tests the system's validated accuracy, which comes from a stricter protocol (held-out, member-grouped human gold). | weak | A fine-tuned Political DEBATE NLI model (mlburnham/Political_DEBATE_base_v1.0) gives the CU stance direction. stance_sign is support, oppose or neutral, and the code states that this is independent of LSD tone. On held-out human gold it scored 0.839 accuracy / kappa 0.742 (n=87). It scored 0.796 on the 760-row frame and 0.571 on the adversarial goldeval frame. |
| [[ornstein-2024-train-your-stochastic]] | 2024 | supports | Tweets whose meaning depends on outside political context are misread unless the prompt supplies that context. The Masterpiece sentiment prompt included a preamble on the ruling and six few-shot examples, and GPT-3 got the sign right on 88.4 percent of 945 tweets. | The paper task is sentiment toward a court ruling, not stance toward an interest, so the paper gives no accuracy figure for CU stance. The paper used six few-shot examples; claude_stance.py is zero-shot with a policy map. The supported point is the design choice of supplying context in the prompt. | moderate | The system described is claude_stance.py, not DEBATE. Its SYSTEM prompt (lines 19-23) carries a credit-union policy map and tells the model to judge by the member position on the underlying issue, not by the words credit union. Its docstring (lines 5-8) names it the pre-labeller and the third member of the ensemble, with the Credit Card Competition Act example. |
| [[ornstein-2024-train-your-stochastic]] | 2024 | qualifies | GPT-4, tuned with RLHF, is poorly calibrated and overconfident. For over 70 percent of its posts it gave one label more than 99 percent probability, and it chose Neutral more often than the authors. GPT-4 was slightly worse than GPT-3 on sentiment and little or no better on ideology. Section 4 says newer RLHF models frequently underperform on classification and scaling tasks, and the latest model is not always the best. | These are GPT-3 and GPT-4 results, not Claude results. The module requests no token log-probabilities (none found in the file), so the paper probability-vector method cannot be applied as written. claude_conf is a self-stated number, not a calibrated probability. Neutral rows created by ERR should be counted apart from model-chosen neutral rows. No DEBATE-vs-Claude agreement figure was located in the sources checked. | moderate | claude_stance.py asks for a self-stated confidence from 0 to 1 in its JSON reply (line 25) and defaults to claude-sonnet-4-6 (line 32). A call that fails all 4 attempts stays neutral with confidence 0.0 and reason ERR (lines 80, 95-97). An unrecognised label is set to neutral (lines 89-91), but the stated confidence is still read at line 92, so it is not reset to 0.0. |
| [[vanderdoes-2022-strategic-identity-signaling]] | 2022 | qualifies | Covert tweets were defined partly by moderates rating them neutral in orientation and affect while far copartisans saw political content; the authors argue automated political-speech classifiers will likely detect only overt speech and miss covert signals. | Holds only if members code CU positions for mixed constituencies. The paper's covertness concerns partisan identity on a left-right axis among ordinary Twitter users, and CU issues are comparatively bipartisan, so the size of any neutral-bin leakage in congressional press is unknown. The classifier warning is the authors' conjecture, not a tested result. | weak | The DEBATE NLI reader picks the argmax over three hypotheses, and the neutral label is literally 'neutral toward or not about credit unions', mapped to sign 0. Coded or hedged support lands in the same zero bin as off-topic text, and no hedged or coded class exists. |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | supports | Ambiguity is 34.0 percent when Trump is not mentioned versus 19.5 percent when he is. NLI classifiers degrade most in low-context documents, while supervised and in-context classifiers adapt better. | The paper measured Trump approval with generic DeBERTaV3 NLI, not Political DEBATE on CU press releases. Transfer to CU is by analogy. | strong | Zero-shot DEBATE base read 53 percent accuracy and oppose F1 0.08 on the 156-row test. Fine-tuning on CU labels gave 87.2 percent accuracy and oppose F1 0.78 against the Claude reference. Figures trace to the vault note debate-stance-model and the 2026-06-22 topic note, not to code. |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | qualifies | Zero-shot MCC for DeBERTaV3 Small is 0.15, Base 0.40 and Large 0.65 (83 percent accuracy). Performance comparable to supervised classifiers emerges only in larger models. | The paper gap is for generic zero-shot NLI. Political DEBATE is a politics-trained model not studied in this paper, so the size gap may differ. | moderate | Production runs the Political DEBATE base model fine-tuned on 552 labeled rows for 3 epochs. The large model was never fine-tuned or scored on the same member-grouped test; the vault lists the large fine-tune as a next step. |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | qualifies | In-context LLMs are useful for expanding training data and for validation, but not as the primary label source because proprietary models cannot be archived. The paper reports GPT-4 MCC 0.78 in Table 5 and recommends temperature zero in Section 4.3.4. | Applies to any training row whose human_label is empty. The share of Claude-only rows is not counted in the analysis. | moderate | The fine-tune label is human_label when valid, otherwise the Claude pre_label. The Claude rater uses claude-sonnet-4-6 with no temperature argument. The 87.2 percent headline is agreement with Claude. |

**Assessment.** **Evidence weight: moderate on failure modes, none on accuracy.** There are 10 rows: 2 VERIFIED (Burnham 2023), 6 VERIFIED-SOFTENED and 2 UNVERIFIED.

**Burnham 2023 is the anchor (VERIFIED, strong).**
- NLI classifiers degrade most on low-context documents. Ambiguity is 34.0% when the target is not mentioned vs 19.5% when it is.
- Zero-shot quality scales sharply with model size (DeBERTaV3 MCC 0.15 / 0.40 / 0.65 for Small / Base / Large).
- The system tracks this. Zero-shot DEBATE base scored 53% accuracy and oppose F1 0.08 (156-row test). Fine-tuning on CU labels raised that to 87.2% and 0.78, but against Claude labels.
- Political DEBATE large has never been fine-tuned or scored on the same member-grouped test. That is the cheapest open experiment.

**Error mode (b-2025 and vanderDoes 2022).** Both converge on the system's observed residual error: implicit or indirect support read as neutral (n=78 such errors). b-2025's 0.97 headline is not a target, because its evaluation set is not common. vanderDoes is UNVERIFIED and its classifier warning is conjecture.

**Label provenance (Ornstein 2024 and Burnham 2023).**
- LLM raters are overconfident. Over 70% of GPT-4 posts got more than 99% probability on one label.
- Proprietary LLMs should not be the primary label source.
- The fine-tune's label is human_label when valid, otherwise the Claude pre_label. The share of Claude-only training rows has not been counted.
- The 87.2% headline is agreement with Claude, not with humans. Only the 0.839 / kappa 0.742 (n=87) figure is human-gold.

**Code defect (Ornstein row).** claude_stance.py coerces an unrecognised label to neutral but still reads the stated confidence at line 92. Its confidence is self-stated, not calibrated. ERR-neutral rows should be counted separately from model-chosen neutrals.

**Kim 2025 does not transfer.** It reports 96.93% on 391 lines that were keyword-prefiltered to position terms, with coder disagreements excluded.

**Scan leads (unread, UNVERIFIED).**
- *Political DEBATE paper* (Burnham, Kahn, Wang & Peng 2024; downloaded to `scan_2026-10-10/burnham_2024_political_debate/`). It has no evidence row although it is the model's own source. Its stance training pairs used "The author of this document supports (stance)", and the authors recommend short hypotheses modelled on training templates. The system uses "This text is {}." Its PolNLI training data includes stance-labelled tweets from senators and representatives and DC Inbox newsletter sentences, which strengthens the transfer argument for congressional text.
- *Laurer et al. 2024 (Political Analysis), BERT-NLI.* Fine-tuned on 500 texts, NLI models match classical models trained on about 5,000. That fits DEBATE tying the tuned SVM at about 0.84 with roughly 456-552 training rows.

**Contradictions.**
- Kim 2025's 96.9% GPT-4 accuracy (Paul's cited benchmark) comes from keyword-prefiltered, disagreement-excluded lobbying lines. It is not comparable to the system's 0.839 on 87 held-out CU press items.
- Burnham 2023 advises against proprietary LLMs as the primary label source, but the fine-tune falls back to Claude pre-labels wherever human_label is empty, and the 87.2% headline is agreement with Claude.

**Gaps.**
- No human-gold test larger than n=87 for the fine-tuned reader. A member-grouped test of 300 or more releases would put a real CI on 0.839.
- Political DEBATE large has not been fine-tuned or scored on the same member-grouped split.
- The hypothesis-template effect ("This text is {}" vs the training template "The author of this document supports...") has not been tested.
- No study of whether coded or hedged CU support exists in congressional press; that would need a hand-coded sample with a 'hedged support' class.

**Paul's position vs the evidence.** Paul's endorsement of transformer and LLM stance readers holds in direction: NLI and fine-tuned models beat zero-shot baselines here. But his cited benchmark (Kim's 96.9%) is an actual error as a reference point: it does not transfer to target-valence stance on press releases. His named weakness, ambiguous statements, is exactly the measured residual error (implicit support read as neutral).

**Open questions.**
- What share of the 552 fine-tune rows carry a Claude-only label, and does accuracy on the n=87 test differ between rows trained with human vs Claude labels?
- Does a training-template hypothesis improve the zero-shot baseline and the fine-tune?
- How many ERR-neutral rows (all 4 attempts failed) are in the label set?

**Read first.** [[burnham-2023-stance-detection-practical-guide]], [[ornstein-2024-train-your-stochastic]], [[b-2025-comparing-fine-tuned]], [[kim-2025-measuring-interest-group]], [[vanderdoes-2022-strategic-identity-signaling]]

## Supervised and stacked stance readers

**What it computes.** Stacked stance label per tweet or press item from classical and API readers.

**Method.** Best classical model: LinearSVC on word+char TF-IDF, 83.9% / macro-F1 0.816 on untouched test (n=87). Khan-style multi-view augmentation was tested and hurt classical models. Supervised tweet stance reached 0.737 CV on 399 golds. Stanceformer is planned, not built.

**Code refs.** `train_stance_domain.py; stance_clf_composed.pkl`, `fec/train_tweet_stance.py`

**Assumptions.**
- Target-specific labels are needed; sentiment is not stance
- The 456/87/217 split is never reshuffled; CV of about 0.90 is inflated and not quoted

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[birkenmaier-2025-measuring-politicians-public]] | 2025 | qualifies | On the paper's personality-cue task, few-shot LLM prompting (0.77) beats fine-tuned XLM-RoBERTa (0.70, strong balanced), which only slightly beats a bag-of-words SVM (0.63 strong balanced; 0.55 to 0.63 across variants). | Different task (CU press and social stance, not German agency and communion cues) and a different baseline: the paper's SVM is word-only with no C tuning. The system's ranking comes from its own held-out test, so this paper cannot rank the system's readers. The code's transformer defaults to distilroberta, not XLM-RoBERTa. | weak | train_stance_domain.py --tune builds a word plus char_wb TF-IDF union with a LinearSVC C grid chosen by CV on train only (lines 74-92). The brain reports the production SVM (word plus char, C=2.0) at 83.9 percent accuracy and 0.816 macro-F1 on the untouched n=87 test, and a transformer at 0.757. These brain figures were not rerun. |
| [[dai-2025-large-language-model]] | 2025 | qualifies | In-target (Table 3): FOLAR beats the best competitor by 1.7 on LA (72.7 vs COLA 71.0) and by 2.0 on DT_I (73.3 vs MPPT 71.3), but trails KEprompt on FM (70.6 vs 72.1). Zero-shot (Table 4): gains of +4.2 on VAST with 100 percent training data and +6.2 with 10 percent. The baselines include statistical, PLM, prompt and LLM methods. No TF-IDF or SVM baseline is reported. | The paper cannot settle SVM versus LLM for this corpus because it runs no bag-of-words baseline. Its pattern (small in-target gains, larger low-data or new-target gains) suggests an LLM-rule reader would help most on new or rare CU targets, which has not been tested for CU stance. The evidence is benchmark-level, rests on three runs and gives no confidence intervals. | weak | train_tweet_stance.py grids LinearSVC and LogReg over word plus char TF-IDF (lines 31-40) and scores by 5-fold stratified CV over the tweet golds (lines 42-54). The file docstring cites 400 golds. The file has no untouched test split. The figures 83.9 percent, macro-F1 0.816 on an untouched n=87 test, and 0.737 CV on 399 golds are not in the cited code and were not checked. |
| [[b-2025-comparing-fine-tuned]] | 2025 | method-source | Classical models on TF-IDF features rank RF 0.95 > SVM 0.94 > LR 0.93 > DT 0.92 > NB 0.89 on a 1,675-row test split. Fine-tuned RoBERTa on tweet plus comment paired input reports 0.97 accuracy on a support of 6,699 described as the augmented dataset. Augmentation and class balancing are credited with gains, but no with and without comparison is shown. | The mechanics (paired input, balanced loss, per-class P/R/F1 and macro-F1) transfer. The model ordering and the augmentation gain are not established by the paper: its classical rows are scored on 1,675 rows and its RoBERTa row on 6,699 rows. 6,699 is almost exactly 0.8 x 8,374 (6,699.2), the size of the 80 percent training share, so the RoBERTa figure may not be on a held-out set. The paper does not say which, so this is an inference and must be labelled as one. | strong | train_stance_domain.py is labelled as the Khan pipeline adapted to the CU domain. It builds a target-conditioned paired input, runs the same five classifiers with class_weight balanced, has an --augment flag that adds title and stance-span views to TRAIN only, and has a --transformer path defaulting to distilroberta-base with 4 epochs and inverse-frequency loss weights. |
| [[b-2025-comparing-fine-tuned]] | 2025 | qualifies | The transformer beats every classical model (0.97 vs at most 0.95). Dataset augmentation and class balancing are credited with improving performance, with no with and without comparison shown. | The system numbers are single-look on an 87-row test and were not re-verified here. They are not a refutation of the paper's own measurement; they show that its ordering and augmentation gain did not transfer to this small English press corpus. Relation changed from contradicts to qualifies, because the paper's claim is about its own data. | moderate | On the CU press gold set (per the analyst, 552 labelled rows, untouched test n=87), a transformer trained on augmented data reached 0.757 macro-F1, below a tuned word plus char LinearSVC at 0.816. The analyst reports the augmentation was rejected on 2026-07-03. Production uses the SVM. |
| [[naz-2025-machine-deep-learning]] | 2025 | qualifies | Traditional ML (SVM and similar on TF-IDF) remains interpretable and competitive on small data. Deep and transformer gains appear with large labeled corpora. The text claims about a 25% LSTM gain over traditional ML, but no table supports the figure. | The paper studies personality, not stance, so this is a pattern about small labeled sets of a few hundred rows, not stance evidence. It fits the system's measured outcome. It does not show that transformers lose at larger n, and it says nothing about LLM zero-shot readers (Birkenmaier, Kim J.), which Paul's view rests on. | weak | The best classical stance reader is LinearSVC on word (1-2 gram) plus char_wb (3-5 gram) TF-IDF: 83.9% accuracy and macro-F1 .816 on 87 untouched test rows, versus a transformer at 0.757 on the same comparison. Supervised tweet stance reached 0.737 CV on 399 golds. |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | qualifies | Bag-of-words LR, RF and SVM reach MCC 0.49-0.51 and RoBERTa, BERTweet and PoliBERTweet reach 0.60, 0.63 and 0.68 on Twitter. Laurer et al. 2024 found 500 training examples let language models outperform algorithms trained on several thousand. | With 456 training rows and an 87-row test all readers fall within each other's intervals. The paper's ordering is for larger training sets and for MCC, not macro-F1. | moderate | Production is a LinearSVC on word plus char TF-IDF. Vault notes give 83.9 percent accuracy, but the 0.816 macro-F1 is a leak-free figure paired with an 84.7 percent headline, and other notes give 0.792 and 0.835 for the same model. The NLI-fine-tuned DEBATE reached 0.839 accuracy and 0.835 macro-F1 on n=87, which matches the vault. A distilroberta macro-F1 of 0.757 was not located. |

**Assessment.** **Evidence weight: weak to moderate. It cannot rank the system's readers.** There are 6 rows: 1 VERIFIED (Birkenmaier 2025), 4 VERIFIED-SOFTENED and 1 UNVERIFIED.

**What the external results show, and why they do not transfer.**
- *Birkenmaier 2025:* few-shot LLM 0.77 > XLM-R 0.70 > bag-of-words SVM 0.63 (strong balanced). It is a different task (German personality cues), and the SVM is word-only and untuned.
- *Burnham 2023:* on Twitter, bag-of-words reaches MCC 0.49-0.51 and RoBERTa-family models 0.60-0.68. These orderings are for larger training sets and MCC, not macro-F1.
- *b-2025:* RoBERTa 0.97 vs at most 0.95 for classical models. The scoring sets differ: 1,675 rows for classical vs a 6,699-row support for RoBERTa. 6,699 is about 0.8 x 8,374, so the RoBERTa figure may be on training data; that is an inference.

**The system's own result (single-look, n=87 untouched test).**
- Tuned word+char LinearSVC: 83.9% accuracy, macro-F1 0.816.
- Fine-tuned DEBATE: 0.839 accuracy, macro-F1 0.835.
- Transformer: 0.757. That figure was not located in code on the Burnham pass.
- Vault notes disagree on the SVM's macro-F1: 0.816, 0.792 and 0.835 all appear. With about 456 training rows and 87 test rows, all readers fall within each other's intervals.

**Augmentation.** Khan-style augmentation helped in b-2025 with no ablation shown. It was rejected here (2026-07-03).

**Benchmark-level pattern.** Dai 2025 runs no SVM baseline. Its pattern is that LLM-rule gains are largest on new or low-data targets, which suggests where an LLM reader might help: rare CU targets. That is untested.

**Tweet stack scoring.** train_tweet_stance.py has no untouched split; it reports 5-fold CV (0.737 on 399 golds).

**Scan lead (unread).** Laurer 2024 BERT-NLI is the relevant prior for the data regime of a few hundred labels. It predicts NLI models beat classical ones at this n. Here they only tie.

**Contradictions.**
- b-2025 (transformer > SVM, augmentation helps) vs the system's own single-look result (tuned SVM 0.816 > transformer 0.757; augmentation rejected). This qualifies b-2025 rather than refuting it, because b-2025's claim is about its own data and its test sizes differ.
- Vault notes report three different macro-F1 values (0.816 / 0.792 / 0.835) for the production SVM, so the internal headline is itself inconsistent.

**Gaps.**
- No interval estimates. A bootstrap or repeated member-grouped CV over the 552 rows would show whether any reader ordering is real.
- No untouched test for the tweet stack (CV only on 399 golds).
- No few-shot LLM reader scored on the same n=87 split, which Birkenmaier's result implies is the comparison that matters.

**Paul's position vs the evidence.** Paul's highlight calls bag-of-words SVMs outdated next to LLMs. The system's own measurement contradicts that at this corpus size: the tuned SVM ties fine-tuned DEBATE and beats distilroberta. Birkenmaier's SVM was untuned and word-only, so it is a weak baseline, not evidence that tuned SVMs lose. No error otherwise; his planned re-read of Peng has no evidence row.

**Open questions.**
- Which macro-F1 (0.816, 0.792 or 0.835) is the current production SVM figure, and on which split?
- Does a few-shot LLM reader beat SVM and DEBATE on rare CU targets specifically?

**Read first.** [[burnham-2023-stance-detection-practical-guide]], [[birkenmaier-2025-measuring-politicians-public]], [[b-2025-comparing-fine-tuned]], [[dai-2025-large-language-model]]

## Lexicoder LSD2015 tone intensity

**What it computes.** Per-release tone intensity = |net| / words from LSD2015 positive, negative and negated counts.

**Method.** stringi regex implementation that matches quanteda data_dictionary_LSD2015 counts; negation phrases are stripped first; 9.6 ms per document.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/parse_lsd.R`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_lsd.R`

**Assumptions.**
- Dictionary counts measure how heated a release is (intensity, not direction)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[zhang-2022-data-assisted-social]] | 2022 | context | Scores phrase sentiment with the general-purpose Affin lexicon plus an ensemble classifier for praise vs complaint. Accuracy is described as 'highest' with no metric, and the Affin citation points to an unrelated photonics paper. | There is no evidence here that a general lexicon measures tone validly on congressional press releases. The open question (does LSD intensity agree with human-coded intensity?) is untouched. Rice & Zorn's case for domain dictionaries remains the relevant source. | weak | Tone intensity is the LSD2015 charge (pos+neg)/words with negation handling, used as intensity only. Direction comes from the stance readers. |
| [[pennebaker-2003-psychological-aspects-natural]] | 2003 | qualifies | Emotion words are under 5 percent of words in daily speech, emotional writing and affect-laden poetry, so emotion-word counts predict emotional state weakly. Word-count programs cannot consider context, irony or sarcasm. In at least two studies, judges' ratings of emotionality correlated more highly with language use than with self-reports. | The paper claim concerns personal emotional state, not how heated a policy release is. LSD2015 is a sentiment dictionary (Young and Soroka 2012 per file header), not LIWC. The sparsity point carries over: in short releases a few hits set the multiplier. The judge finding supports validating intensity against human coding. | weak | Per-release intensity is abs(lsd_net_norm) or lsd_charge = (pos + neg) / words, multiplied by the stance sign to give press_value, so intensity scales every press cell. |
| [[taherdoost-2023-artificial-intelligence-sentiment]] | 2023 | qualifies | Alantari et al., as summarised by the review, find a trade-off: neural networks predict well but diagnose poorly, while more transparent methods are poorer predictors. The review frames word-matching approaches as the baseline that AI improves on. The trade-off is qualitative only and no numbers are given. | The paper agrees with the system's split of roles: an opaque predictive reader for direction and a transparent dictionary for an interpretable intensity scalar. It gives no evidence that dictionary intensity is valid on political press releases. The open question of whether LSD intensity matches human-coded intensity on this corpus is untouched. The source evidence is consumer reviews. | weak | Lexicoder LSD2015 counts run with negation phrases removed first. They yield lsd_net_norm = net/words and lsd_charge = (pos+neg)/words. Only the magnitude (\|net_norm\| or charge) enters press_value = final_sign x intensity, so the dictionary sets intensity and never direction. |
| [[ornstein-2024-train-your-stochastic]] | 2024 | qualifies | Dictionary methods lose badly when meaning depends on context. The best dictionary the authors could build got 38.3 percent of tweet signs right, against 88.4 percent for GPT-3. Of 5332 speeches containing honor, GPT-3 labels only 145 as praising honor. | The paper dictionary is the authors own construction, not LSD2015, and the paper did not test intensity. Intensity inherits the context blindness the paper describes (ceremonial or quoted language reads as heated). Whether LSD intensity tracks human-coded intensity is untested. | weak | LSD2015 is used only for intensity. run_stage5.R:141 takes abs(lsd_net_norm) by default, or lsd_charge when CU_S5_INTENSITY=charge, and line 143 multiplies it by final_sign. stage5_lsd.R (header lines 10-15) matches and removes negation phrases before counting. Direction in run_stage5.R comes from the DEBATE stance with bill-sign and human-label overrides (lines 139-140), not from the Claude rater. |
| [[merkley-nd-party-cues-news]] |  | qualifies | The authors use Lexicoder with a custom dictionary of party names, office titles and party leaders to detect party cues, not sentiment or tone. They validate detection against hand coding of a random 700 articles (about 80% contain a cue by their definition). Stance is hand-coded on a separate random sample of 3000 articles (97% of articles with Democratic messages are wholly pro-consensus). Frames come from SVM classifiers (77-80% accuracy). | The paper is a method precedent only: a dictionary for detecting actors plus hand-coded validation. It never uses or validates sentiment or tone as an intensity measure, so it is no evidence that LSD tone intensity is valid on congressional press releases. | weak | stage5_lsd.R runs the LSD2015 dictionary in lsd_tone (lines 49-60), returning net tone and charge per word. Live run_stage5.R defaults the intensity to the absolute net tone per word (INTENSITY default net_norm, line 45), with charge only as an option, and press_value equals final_sign times that intensity at lines 141-143. The bearing's earlier statement that charge per word is the intensity used in press_value was wrong. |
| [[dietrich-2019-pitch-perfect-vocal]] | 2019 | qualifies | Speech topic, word choice and length of remarks are highly controllable and driven by strategic concerns, while vocal pitch typically lies beyond the individual's control, making it a more honest indicator of intensity on a given issue (leakage hierarchy, pp. 942-944). The paper does not compare dictionary tone or LSD scores with pitch. | A theoretical argument only; the paper never tests LSD tone against pitch. The claim that press releases are staff-drafted and so more strategic than floor speech has no support in the paper or the code. LSD charge measures crafted rhetorical tone, not felt intensity, and should be labelled that way. | weak | Press intensity is \|LSD net\| / words or (pos + neg) / words (lsd_charge) per member press release. The press pillar multiplies stance sign by this heat. |
| [[rice-2018-corpus-based-dictionaries]] | 2018 | qualifies | General-purpose dictionaries carry valences that are wrong in specialized vocabularies (tennis 'love'). On the movie-review benchmark a corpus-derived dictionary reached 80.4% in-sample and 79.5% held-out, versus LIWC 69.4% and AFINN 71.3% in-sample and AFINN 70.6% held-out. | The paper's evidence covers polarity classification of movie reviews and court opinions, not intensity and not congressional press releases. The mechanism (domain nouns read as tone) is visible directly in the LSD entries. Size of the effect on releases remains UNVERIFIED in this pass. The paper does not test whether LSD intensity tracks human-judged heat. | moderate | Stage 5b scores full release bodies with the stock LSD2015 dictionary (1,709 positive and 2,858 negative entries; counts verified against LSDaug2015.zip). Intensity = abs(pos-neg)/words. Verified entries: 'credit' family (credit, credits, credited, creditable, crediting) is positive; debt*, charge*, risk*, oversight* and fraud* are negative. No domain masking in run_stage5.R:79. The local masking figures (4.5% and 7.0% of mass, rho 0.953, 4.1% sign flips, press_value rho 0.976) were NOT reproduced here: no R runtime, and the text cache is R rds only. |
| [[park-2025-automatic-construction-direction]] | 2025 | qualifies | Direction plus noun carries polarity that a general or finance dictionary misses. LM alone labels 'Profit for the period was EUR 10.9 mn, down from EUR 14.3 mn in 2009' neutral (SentimentScore 0), while the (profit, down) pair gives refined score -1. On DS100, negative-class F1 is 0.4087 for LM alone and 0.7260 for LM+Senti-DD. | Weak. LSD sign is discarded here, so the paper's flips matter for intensity only when directional pairs cancel or inflate \|net\|, as in cost-cut or fee-reduce releases. The paper tests neither LSD nor intensity, and Paul's system has no human-coded intensity gold to test this against. | weak | LSD2015 tone in stage5_lsd.R is a direction-blind count of positive and negative words and negation phrases. Press intensity is \|net\|/words or charge, multiplied by the DEBATE or human sign, so LSD sets magnitude only and its sign is discarded. For text-less bill rows intensity is a fixed BILL_W (0.03). |
| [[haselmayer-2017-sentiment-analysis-political]] | 2017 | measurement-source | Validation on 200 sentences: aggregated lay crowd vs aggregated expert r = 0.82. Domain dictionary vs expert r = 0.65 at 84% coverage, about 0.6 when the 32 unmatched sentences are set to zero. Momtazi r = 0.13 (46% coverage) and SentiWS r = 0.19 (31% coverage). Authors state that dictionary analysis requires a customized dictionary and recommend caution with general-purpose dictionaries. | The validation protocol transfers: about 200 releases rated 0-4 by lay coders and experts, correlated with \|lsd_net_norm\|. The size of the generic-dictionary penalty does not transfer directly, because the paper's comparators are machine-translated German lexicons, not LSD2015. This paper does not establish that LSD2015 performs well on English news; the Young and Soroka citation is general sentiment work, not an LSD validation. The paper scores sentences for negativity only, while LSD here is bidirectional and scores whole release bodies. | moderate | press_value tone intensity is stock LSD2015, uncustomized. stage5_lsd.R lsd_tone (lines 48-61) removes negator-plus-word hits and flips them: negated negatives count as positive and negated positives as negative. lsd_net_norm = (pos - neg) / words. run_stage5.R line 141 takes abs(lsd_net_norm) as heat; line 143 sets press_value = final_sign x heat. The brain note at debate-stance-model.md line 102 records only a three-case smoke test (pro +4 / negated 0 / neutral 0) and calls LSD validated, (a functional check by the paper standard). |

**Assessment.** **Evidence weight: moderate that stock dictionaries are risky; none that LSD intensity is valid here.** There are 9 rows: 0 VERIFIED, 7 VERIFIED-SOFTENED and 2 UNVERIFIED.

**What LSD is used for.** It sets only magnitude: abs(lsd_net_norm) by default, or lsd_charge, multiplied by the stance sign (run_stage5.R:141-143). Its own sign is discarded.

**Dictionaries misfire out of domain.**
- *Haselmayer & Jenny 2017 (measurement source):* a domain dictionary reached r = 0.65 against experts (200 sentences), while general lexicons reached r = 0.13-0.19. The authors say dictionaries must be customized.
- *Rice & Zorn 2018:* general dictionaries carry wrong valences in specialized vocabularies.
- The mechanism is directly visible in LSD2015. The 'credit' family is positive. debt*, charge*, risk*, oversight* and fraud* are negative.
- The masking figures from a local re-run (4.5-7.0% of mass, rho 0.953, 4.1% sign flips) were NOT reproduced: there was no R runtime, and the text cache is .rds only.

**Context blindness (Ornstein 2024).** The authors' best dictionary got 38.3% of tweet signs right vs 88.4% for GPT-3. Intensity inherits this: ceremonial or quoted language reads as heated.

**Other qualifications.**
- *Pennebaker 2003:* emotion words are sparse (under 5% of words), so a few hits set the multiplier in short releases.
- *Dietrich 2019:* text tone is strategic and controllable, so label it crafted rhetorical tone, not felt intensity.

**Correction carried.** The production intensity default is net_norm, not charge.

**The brain overstates validation.** It calls LSD 'validated' on a three-case smoke test. By Haselmayer's standard that is a functional check, not a validation.

**Scan finding.** LSD's own validation (Young & Soroka 2012) is against human-coded news, not legislator press. No study validating LSD on congressional press releases was found. A Dutch economic-news study (van Atteveldt et al.) found off-the-shelf tools unreliable and recommends genre-specific manual validation.

**Contradictions.**
- The brain note (debate-stance-model.md line 102) calls LSD validated on a three-case smoke test (pro +4 / negated 0 / neutral 0). Haselmayer & Jenny's standard requires correlation against human-rated intensity on about 200 units.
- LSD2015 scores domain nouns ('credit' positive; debt, charge, risk, oversight, fraud negative) as tone, which is exactly the misfire Rice & Zorn and Haselmayer & Jenny warn against.

**Gaps.**
- No human-coded intensity gold. The study that fills it: about 200 releases rated 0-4 for heat by lay coders and an expert, correlated with |lsd_net_norm| and lsd_charge, following Haselmayer & Jenny.
- The effect of masking domain nouns has not been reproduced (R runtime needed).
- No comparison of LSD intensity against an LLM-rated intensity on the same releases.

**Paul's position vs the evidence.** Paul's highlight of Merkley & Stecula's custom Lexicoder dictionary is a fair precedent for customization. But that paper uses Lexicoder to detect party cues, not tone, so it is no evidence that stock LSD tone is valid here. Read correctly, his own source argues for a custom dictionary, which the system does not use.

**Open questions.**
- How much of press_value's magnitude is driven by domain nouns (credit, charge, risk) rather than evaluative language?
- Does net_norm or charge track human-rated heat better?

**Read first.** [[haselmayer-2017-sentiment-analysis-political]], [[rice-2018-corpus-based-dictionaries]], [[ornstein-2024-train-your-stochastic]], [[pennebaker-2003-psychological-aspects-natural]], [[dietrich-2019-pitch-perfect-vocal]]

## press_value aggregate and human-label overlay

**What it computes.** press_value = stance_sign x LSD intensity per member x category. Human labels overwrite model stance where valid; 497 of 552 labels overlap the corpus.

**Method.** run_stage5.R block 3c is placed after the bill override and the Kennedy fix. Overlaid rows are excluded from model-accuracy measurement.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/make_standalone.py`

**Assumptions.**
- Human labels are authoritative
- Overlay rows cannot be used to measure DEBATE accuracy

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[ornstein-2024-train-your-stochastic]] | 2024 | suggests-extension | Continuous measures built from the label probability vector: per-sentence P(Conservative) minus P(Liberal), averaged per manifesto, gives rho 0.92 (economic) and 0.80 (social) across 18 UK manifestos with GPT-3. Tweet sentiment uses the first principal component of the positive/negative/neutral probability vector. | The paper evidence uses GPT-3 next-token probabilities on a few-shot prompt, not DEBATE NLI entailment scores. DEBATE runs with multi_label=True (stage5_debate.py:65), so its three scores are independent and do not sum to 1. A p_support minus p_oppose contrast is a different object and would need a pre-registered single look on the 760-row press gold. Remains UNVERIFIED until a second path reproduces it (DE-062). | moderate | run_stage5.R:143 sets press_value = final_sign x intensity, where intensity is abs(lsd_net_norm) by default (line 141). final_sign is the bill sign or stance_sign, which is the argmax of DEBATE (run_stage5.R:140; stage5_debate.py:77-82). run_stage5.R:9 is a comment only. p_support, p_oppose and p_neutral are kept as columns (run_stage5.R:98,166) and do not enter press_value. |
| [[rice-2018-corpus-based-dictionaries]] | 2018 | qualifies | Changing the dictionary reorders units. Justice-level rank correlations are 0.29 (corpus measure vs LIWC) and 0.16 (corpus measure vs AFINN), versus 0.64 between LIWC and AFINN. Standard deviation of justice means is 0.25 (corpus), 0.22 (LIWC) and 0.16 (AFINN). Justices with more than 50 majority opinions; comparison across 84 justices. | The divergence evidence comes from a 217-year court corpus with large semantic drift. Press releases from 2022 on are a narrow window, so the divergence is probably smaller there; that is the analyst's inference and is untested. The live blend press weight of 0.10 was not checked in this pass. | weak | press_value = final_sign x abs(lsd_net_norm), or lsd_charge when INTENSITY is charge (run_stage5.R:141-143, verified). Member and category roll-up uses decay weights (cu_press_release_scraper.R:154-158 comment). The Press pillar normalization is signed-extrema in fec/moneyball_core.py:214-237 (verified as normalization, not as press_value roll-up). Only one instrument is used; no alternative is compared. |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | qualifies | Stance and sentiment are distinct dimensions to be measured independently. Sentiment may inform intensity but cannot stand in for stance. Integrating sentiment to scale stance intensity is named as a future direction needing more research. | This is a conceptual endorsement of separating the two dimensions only. The paper does not test whether multiplying sign by intensity improves measurement; it calls that open. | moderate | press_value = stance sign times LSD intensity (\|net\| per word). The sign comes from DEBATE or the human overlay, never from the dictionary. |

**Assessment.** **Evidence weight: weak and conceptual.** There are 3 rows, all VERIFIED-SOFTENED.

**Separating stance from intensity (Burnham 2023).** The paper endorses measuring stance and sentiment separately and lets sentiment inform intensity. It calls scaling stance by sentiment an open research question. press_value = final_sign x LSD intensity is therefore a design choice with no tested support.

**Instrument choice reorders units (Rice & Zorn 2018).** At the justice level, the corpus measure correlates only 0.29 with LIWC and 0.16 with AFINN. Press_value relies on one instrument with no alternative compared. The court corpus spans 217 years with large drift, so divergence on 2022+ press is probably smaller. That is an inference and untested.

**A continuous alternative (Ornstein 2024).** The paper builds continuous positions from the label-probability vector: P(Con) - P(Lib) averaged per manifesto gives rho 0.92 economic / 0.80 social across 18 UK manifestos. The system keeps p_support, p_oppose and p_neutral but does not use them in press_value. DEBATE runs with multi_label=True, so its scores are independent and do not sum to 1; a contrast would be a different object. UNVERIFIED until a single pre-registered look on the 760-row press gold.

**Human-label overlay.** No evidence row covers it. Overlaid rows are excluded from model-accuracy measurement, which is correct practice.

**Scan lead (unread; downloaded `lemens_gallego_2025_asking_averaging/`).** Le Mens & Gallego 2025 (Political Analysis) average per-text LLM positions over 100 tweets per senator (117th Congress) and report correlations above 0.90 with DW-NOMINATE. That is a template for member-level aggregation. On a left-right dimension, a 0.90 correlation is largely party separation, so it is not transferable as an accuracy prior for a bipartisan CU axis.

**Contradictions.**
- (none)

**Gaps.**
- No test that sign x intensity outperforms sign alone against any external criterion (e.g. a hand-rated member-level CU stance, or later votes on CU-relevant bills).
- No comparison of LSD intensity with an alternative intensity instrument (Rice & Zorn's reordering concern).
- No evidence or validation of the human-label overlay's effect on member-level aggregates.

**Paul's position vs the evidence.** Paul treats sentiment-scored press as an augmentation to lobbying-pathway models; the evidence neither supports nor contradicts that. Burnham explicitly calls sentiment-scaled stance an unvalidated research direction, so the augmentation rests on an untested multiplier.

**Open questions.**
- Does member-level press_value rank members differently from member-level mean stance_sign alone, and by how much (rank correlation over the member x category cells)?

**Read first.** [[burnham-2023-stance-detection-practical-guide]], [[ornstein-2024-train-your-stochastic]], [[rice-2018-corpus-based-dictionaries]]

## Two-hop stance x valence decomposition with curated registry

**What it computes.** cu_alignment = stance(speaker->target) x valence(target->credit unions).

**Method.** Measured on 400 tweet golds: stance hop 0.915, valence hop 0.689 (the bottleneck). Composing model stance with oracle valence gives 0.872. Valence registry cu_valence_registry.json v2 (11 derived plus 14 seed entries) is derived from Paul's gold judgments; with 5-fold CV the composition reaches 0.782 accuracy / 0.754 F1 vs a flat baseline of 0.677. Auto-registry variants all fell below the flat baseline.

**Code refs.** `fec/apply_two_hop_stance.py (--eval, --uncovered)`, `derive_valence_registry.py`, `cu_valence_registry.json`, `fec/stance/build_media_blend.py`

**Assumptions.**
- Each target has a stable CU valence the owner can judge
- Per-target valence is not keyword-automatable (238 distinct targets in 400 tweets)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[dai-2025-large-language-model]] | 2025 | method-source | FOL rules elicited from an LLM (Table 5 bike-lane rule links congestion to Against) are made differentiable with soft-logic operators: negation 1-U, conjunction U1*U2, implication 1-U1+U1*U2 (Eqs 2-5), and trained end to end with cross-entropy (Eq 12). Zero-shot VAST all-class F_avg: 81.0 vs 76.8 for KASD with 100 percent training data (+4.2); 80.2 vs 74.0 for MPPT with 10 percent (+6.2). Error analysis of 50 VAST errors (Sec 4.8) names LLM over-inference that links a positive clause to the target, and clause-level polarity conflicts. | Transfer is limited to the decomposition idea. FOLAR targets are benchmark targets, whereas the valence hop needs CU-specific, owner-judged knowledge that LLM priors do not supply. The paper's over-inference failure matches the automatic-registry failure in the system. LLM-elicited valence rules stay UNVERIFIED (DE-062) until scored against the 400 tweet golds under single-look rules. The paper gives no evidence that a soft operator beats an exact registry lookup where the registry has coverage. | moderate | compose() in apply_two_hop_stance.py is a hard sign flip: support with positive valence stays support, and the reverse flips; neutral and unrelated pass through. lookup() takes the first registry entry whose match substring appears in target plus text, and the flat model alignment is the fallback on a miss (line 55). The file docstring reports stance-hop 0.915, valence-hop 0.689 and oracle ceiling 0.872, and the eval prints a flat baseline of 0.677. The composed 0.782 and F1 0.754 and the claim that every automatic-registry variant fell below the flat baseline are not in the cited code. |
| [[tang-2015-negative-link-prediction]] | 2015 | supports | Signed triads are overwhelmingly balanced: 92.31 percent (Epinions) and 93.01 percent (Slashdot) of undirected triads (Sec. 3.2 p. 89). This is a structural prior that sign products usually hold in person-to-person trust networks. The paper does not test stance-times-valence composition. | Paper triads are person-to-person links. The system's third node is an interest reached through a curated valence registry. The paper supplies only a structural prior and does not validate the registry. The registry numbers are on the golds the design iterated against, so they are not an out-of-sample validation. | weak | Sign logic confirmed: compose(stance, v) returns support when stance is support and v is positive, and when stance is oppose and v is negative; it returns oppose for the other two combinations. Correction: compose() sets no cu_alignment variable; cu_alignment appears only as CSV column names (lines 37, 55-56). The 0.872 oracle ceiling and 0.677 flat baseline are in the file docstring. The 0.782 registry two-hop score (tweet_stance_stack.py docstring) is measured on the 400 tweet golds the design iterated against, not 5-fold CV; the 5-fold out-of-fold claim applies to the stacked model. |
| [[park-2025-automatic-construction-direction]] | 2025 | method-source | Polarity attaches to a (direction, target) pair. Direction-dependent nouns are typed proportional or inversely proportional by PMI against direction-tagged sentences (Eq. 1, delta = 0). Of 719 tagged sentences, 691 are proportional and 28 inversely proportional, and the imbalance yields noisy words such as beer and day. | Paul's registry is a gold-derived analogue of the paper's direction-pair lexicon; the automatic PMI typing is the part that did not transfer. Transfer is limited by scale: 400 tweet golds and 238 distinct targets, against the paper's 4,835 sentences with a 1 percent noun-frequency floor. A PMI typing pass on this corpus needs a held-out look before any claim (DE-062). | moderate | Valence is attached per target family in derive_valence_registry.py. cfpb_institution and cfpb_general are fixed at 0 by user-directed policy and act as binding overrides. cfpb_reform and cfpb_expansion have no hard-coded sign; they take whatever sign the gold labels derive. The code keeps families at 75 percent gold consistency (min_consist=0.75) although the file header says 80 percent. Brain note: CFPB is 64% inverse under sentence-level typing and 41% under release-level typing (share of inverse tags, not accuracy). The gold-derived registry scores 0.782 accuracy in honest 5-fold CV against a flat 0.677 baseline; five auto-registry variants scored 0.464 to 0.604. |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | context | Stance is entailment relative to context C. When the target is absent, ambiguity rises (34.0 versus 19.5 percent) and accuracy depends on outside knowledge. The paper flags outside-knowledge inference as the hard step. | The paper never decomposes stance into a stance hop and a valence hop. The decomposition is the system's own design; the paper only explains why outside-knowledge steps are hard. | moderate | CU alignment = stance toward named target times target CU valence. Valence hop 0.689 versus stance hop 0.915 per the module docstring. A curated registry composed with the stance hop reached 0.782 accuracy under honest 5-fold CV on 400 tweet golds. |

**Assessment.** **Evidence weight: moderate as method precedent; no external validation.** There are 4 rows, all VERIFIED-SOFTENED.

**Method sources.**
- *Park 2025* attaches polarity to a (direction, target) pair. The gold-derived valence registry is the hand-built analogue, and the automatic PMI typing is the part that did not transfer. Five automatic-registry variants scored 0.464-0.604 against a flat baseline of 0.677 (400 tweet golds).
- *Dai 2025* (FOLAR) makes LLM-elicited rules differentiable. Its error analysis names LLM over-inference, which matches the automatic-registry failure. compose() is a hard sign flip, not a soft operator. Nothing shows a soft operator beats an exact lookup where the registry has coverage.
- *Burnham 2023* explains why the outside-knowledge (valence) hop is the hard step: valence hop 0.689 vs stance hop 0.915. The decomposition itself is the system's own design.
- *Tang 2015* gives only a structural prior: signed triads are about 92-93% balanced in Epinions and Slashdot person-to-person networks.

**The headline is not out of sample.** The 0.782 two-hop score sits on the 400 tweet golds the design iterated against. The Tang pass attributes it to the tweet_stance_stack docstring and says the 5-fold out-of-fold claim belongs to the stacked model. The Park and Burnham rows call it honest 5-fold CV. These provenance claims conflict and must be resolved before 0.782 is quoted.

**Header defect.** derive_valence_registry.py keeps families at min_consist = 0.75, but its header says 80%.

**Contradictions.**
- The provenance of 0.782 conflicts: the Park and Burnham rows say honest 5-fold CV on 400 golds, while the Tang row says it was measured on the golds the design iterated against, with 5-fold applying only to the stacked model.
- derive_valence_registry.py header (80% consistency) vs code (min_consist=0.75).

**Gaps.**
- No fresh held-out gold set for the registry. Labelling 150-200 new tweets or releases on targets not used in registry derivation would give an out-of-sample estimate.
- No coverage measure: what share of production items hit a registry entry versus the flat fallback (line 55).

**Paul's position vs the evidence.** Paul's question (how agreement is quantified when two actors share negative sentiment toward a third) is answered structurally by stance x valence. Tang's balance prior is consistent with it, but only for person-to-person networks, not interests. No error.

**Open questions.**
- Is 0.782 an out-of-fold estimate or an in-sample one on the iterated golds?
- What fraction of production rows fall back to flat alignment?

**Read first.** [[park-2025-automatic-construction-direction]], [[dai-2025-large-language-model]], [[burnham-2023-stance-detection-practical-guide]], [[tang-2015-negative-link-prediction]]

## Bill-sign valence (direction table, prose matcher; override rejected)

**What it computes.** Stance sign from a tracked bill's known CU direction when a release names the bill.

**Method.** cfpb_bill_directions.csv (Reduce +1 / Expand -1) plus the BILLS table in cu_press_release_scraper.R. The named-bill matcher had 93% precision on 187 releases but went 0 good / 3 bad as a blanket override. The DEBATE bill-sign override (12-member anti-CU prior list) dropped held-out accuracy from 0.839 / kappa 0.742 to 0.770 / 0.630 and was disabled 2026-06-30. The naive directional-verb lexicon (after Park, Lee & Cho 2025) gave 3 good / 43 bad sign changes and was rejected. The narrow Kennedy per-release fix is kept.

**Code refs.** `cu_press_release_scraper.R (BILLS ~1642; score_bill_signals ~2600)`, `cfpb_bill_directions.csv`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R (CU_S5_BILL_OVERRIDE)`

**Assumptions.**
- REFUTED: DEBATE neutral means a missed bill stance
- REFUTED: directional verb counts over whole text determine valence
- Member priors (anti-bank consumer advocates) need a member-level prior, not a lexicon

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2025-measuring-interest-group]] | 2025 | suggests-extension | The released dataset gives measured group positions on named S./H.R. bills for the 111th-117th Congresses (2009-2022). It is on Dataverse at doi:10.7910/DVN/D0QWM2. | Useful as an external audit of the hand-set direction table, but only for 111th-117th Congress bills that CU trade associations named in LD-2 lines. 118th and 119th Congress bills would need the Haiku lane. Use it to adjudicate the table, never to overwrite it. | moderate | A bill's CU direction comes from a 60-row hand table, cfpb_bill_directions.csv (Direction 1 = Reduce, -1 = Expand; 5 Expand rows). |
| [[park-2025-automatic-construction-direction]] | 2025 | qualifies | On four FPB subsets (DS50 4,835 / DS66 4,209 / DS75 3,447 / DS100 2,259 sentences) LM+Senti-DD weighted F1 is 0.7057, 0.7322, 0.7788, 0.8233 vs LM alone 0.5914, 0.6023, 0.6174, 0.5982. On SemEval-2017 (960 messages) and FiQA (436 sentences) it is 0.4789 and 0.4937, below several general lexicons. In the 20-case DS100 appendix sample 10 are correct and 10 wrong; cited failures include (profit, increase) and (company, increase). | Evidence transfers only where a directional word governs the target noun inside one short sentence, as in FPB headlines. It does not transfer to whole-release counting over multi-actor congressional press text, where speaker attitude toward the direction is a second hop the paper does not model. The paper's own shift to SemEval and FiQA drops lexicon F1 below 0.5. This supports restricting any rule to the sentence naming the bill; any such variant stays UNVERIFIED until a held-out gold look (DE-062). | strong | Sign comes from DEBATE or the human overlay for text-bearing releases; the bill table supplies sign only for text-less billstatus rows (run_stage5.R lines 128-143). The directional-verb override derived from this paper was tested on whole release text and rejected (3 good / 43 bad sign changes on gold, per brain note). run_stage5.R lines 105-108 document a different rejected experiment (bill-sign override, accuracy 0.839 to 0.770, 2026-06-30) that the rel did not cite. |

**Assessment.** **Evidence weight: moderate, and it supports the rejections already made.** There are 2 rows, both VERIFIED-SOFTENED.

**Why the directional-verb override failed (Park 2025).** Direction-aware lexicons help only where a directional word governs the target noun inside one short sentence. Their weighted F1 is 0.71-0.82 on Financial PhraseBank subsets but falls below 0.5 on SemEval-2017 and FiQA. That explains why the whole-release override failed here (3 good / 43 bad sign changes on gold). Any revived rule must be restricted to the sentence naming the bill, and it stays UNVERIFIED until a held-out gold look.

**A separate rejected experiment.** The DEBATE bill-sign override, a 12-member prior list, lowered held-out accuracy from 0.839 to 0.770 (kappa 0.742 to 0.630). It was disabled on 2026-06-30.

**External audit source (Kim 2025).** Kim's released dataset of measured group positions on S./H.R. bills (111th-117th Congresses) can audit the 60-row hand direction table (cfpb_bill_directions.csv, 5 Expand rows).
- It covers only bills that CU trade associations named in LD-2 lines.
- Under D-021, LDA-derived positions are used to adjudicate the hand table, never as weights or edges, and never to overwrite it.
- 118th and 119th Congress bills are uncovered.

**Contradictions.**
- (none)

**Gaps.**
- No independent check of the 60-row direction table. A two-coder adjudication, or the Kim dataset for 111th-117th bills, would fill it.
- No sentence-scoped directional rule has been tested on held-out gold.

**Paul's position vs the evidence.** Paul's critique of Kim (reactive; does not show inter-industry conflict) is a substantive judgement the evidence does not address. Kim's value here is as an audit source for direction signs, not as a model of conflict. No error.

**Open questions.**
- How many of the 60 direction-table bills appear in Kim's 111th-117th dataset with a CU trade-association position, and do the signs agree?

**Read first.** [[park-2025-automatic-construction-direction]], [[kim-2025-measuring-interest-group]]

## CFPB axis isolation (registry families plus binding policy_force)

**What it computes.** CFPB stance split into restraint (+1), expansion (-1), institution (0) and general (0) families.

**Method.** policy_force is a binding post-meta override in both stacks, and the CATKW router was tightened. CFPB-negative tiers went D 123 -> 98, R 7 -> 0; no-evidence 174 -> 263.

**Code refs.** `fec/stance registry and CATKW router (exact file not named)`

**Assumptions.**
- CFPB restraint vs expansion is separable by registry families
- CFPB-negative tiers were confounded by party (123 D vs 7 R)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | suggests-extension | Party overlap is under 1.5 percent in both chambers for the past 5 Congresses (p.15). The paper states, citing McCarty, Poole and Rosenthal (1999) rather than presenting evidence, that party per se does not account for much roll-call voting over and above the spatial model (p.17). | With near-zero overlap, dim1 and party are nearly collinear; the informative test is within-party. A pooled coefficient carries the UNSTRATIFIED label. | moderate | Brain records CFPB-negative tiers as a confirmed party proxy (D 123 vs R 7), and after axis isolation D 98 vs R 0. dw_control.py provides a dim1 and extremity loader for psychometric residualization; within fec it is imported only by hearing_antagonism_build.py, not by any stance code. bluesky_stance_stack.py:46 defines POLICY_KEYS, the CFPB policy_force axis. |

**Assessment.** **Evidence weight: near GAP.** There is 1 VERIFIED-SOFTENED row (Poole 1999), and it bears on a confound, not on the method.

**The confound.** With party overlap under 1.5% in both chambers, DW-NOMINATE dim1 and party are nearly collinear.
- CFPB-negative tiers: D 123 vs R 7 before isolation, D 98 vs R 0 after. Both counts are pooled and UNSTRATIFIED.
- They are as consistent with a party proxy as with an isolated policy axis.
- The informative test is within-party.
- dw_control.py exists, but no stance code imports it (only hearing_antagonism_build.py does).

**Method.** The isolation itself (registry families; policy_force as a binding post-meta override; tightened CATKW router) has no literature. No-evidence cells rose from 174 to 263, so isolation trades coverage for purity, and that trade is unmeasured.

**Scan.** Le Mens & Gallego's 0.90+ correlations with DW-NOMINATE illustrate the same issue: on a partisan dimension, agreement with roll-call ideology is mostly party. Nothing was found on isolating an agency-specific policy axis from partisan text.

**Contradictions.**
- The brain records CFPB-negative tiers as a confirmed party proxy (D 123 vs R 7), yet the post-isolation result (D 98 vs R 0) is if anything more party-aligned. Isolation did not remove the party signal, and no within-party test exists.

**Gaps.**
- Within-party validation. Among Democrats (and separately Republicans), test whether CFPB tier predicts CFPB-relevant roll-call votes or cosponsorships better than dim1 does. Report by stratum or label the result UNSTRATIFIED.
- A hand-coded audit of cells moved to no-evidence (174 to 263). Were they true non-positions or lost signal?
- No literature found on axis-isolation methods; a methods study would need a design that separates the issue dimension (e.g. issue-specific ideal points from CFPB-related votes).

**Paul's position vs the evidence.** No literature, as Paul notes. The evidence that exists (Poole) raises a confound he has not addressed in writing: the tiers may measure party.

**Open questions.**
- Within each party, does CFPB tier vary at all, and does it track anything beyond dim1?
- What happened to the 89 cells moved into no-evidence?

**Read first.** [[poole-1999-nominate-after-years]]

## Media blend (press + social) with recency decay

**What it computes.** raw cell = (1-0.5f) press/p80 + 0.5f social/p80, with f = min(1, nz/15) shrinkage for thin categories.

**Method.** Per post: sign x clip(margin, .25, 1.5) x (0.5 if retweet) x 0.5^(age/HL) / n_categories. Half-life is 2.0 years for press and 0.5 years for social. The press-side recency_weight in the R scraper also uses a 2-year half-life with a 2022-01-01 floor. 20 categories via CATKW regex; CROSSPOST_DEDUP is off by default.

**Code refs.** `fec/stance/build_media_blend.py:29-47`, `build_social_master.py`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R (recency_weight, wmean)`

**Assumptions.**
- Stance salience decays exponentially at the stated half-lives
- Social posts carry stance comparable to press after p80 scaling
- Thin categories (under 15 members with signal) are unreliable

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[steinertthrelkeld-2018-twitter-data]] | 2018 | context | Every US Senator and 430 House members had official Twitter accounts (2016). Tweet author metadata reflects when the API request was made, not when the tweet was created. | The coverage figure supports the assumption that almost all members have a social channel, but it counts Twitter accounts in 2016. It does not count the 119th Congress, it does not cover the platform mix the system reads (X, Facebook, Truth Social, Bluesky), and it says nothing about how many posts each member makes on CU topics. The metadata-timing caveat applies to handle-to-member mapping done at collection time. It does not apply to decay, which uses the post date. The paper offers nothing on ghost-writing, on the half-life, or on whether social posts add predictive validity over press releases. | weak | The media blend gives social posts half the weight of each member-category cell, with SOCIAL_W 0.5, a 0.5-year half-life (decay 0.5^(age/HL)) and nz/15 shrinkage where social signal is thin. Members are attributed through a handle registry; unverified Bluesky handles are flagged and kept out of the member rollup. |
| [[sotoudeh-2024-network-based-measure]] | 2024 | qualifies | The 6-month half-life is one of three settings for decaying cosponsorship-influence capital, not a stance-salience estimate. Discrimination barely changes between 6 and 24 months (0.248 vs 0.203). | Not transferable. The paper's decay applies to accrued passage-rate ties and its outcome is bill passage. Its flatness across half-lives warns against treating any one value as calibrated. The paper does not justify 0.5 years for social posts or 2 years for press. | moderate | Media blend decays each post by 0.5^(age/HL): HL 2.0 years for press and SOCIAL_HL_YEARS default 0.5 for social (confirmed in build_media_blend.py). |
| [[hargittai-2015-bigger-always-better]] | 2015 | qualifies | Non-users are excluded from a site-based sampling frame by design. If selection is related to outcomes, results from users may reach the opposite conclusion about the wider population. Sampling should not be tied to the substantive question. The paper does not discuss how missing social-media activity should be coded. | The paper gives no evidence about missing-data coding. The concern bites only if posters differ from non-posters on stance in categories where f = 1; that is untested, and the analyst's own note says so. The stance-ladder rule on missing evidence was not checked. UNVERIFIED. | weak | build_media_blend.py: SOCIAL_W = 0.5; f = min(1, nz/15), with nz the members carrying a nonzero social value in the category (shrink on by default). Cell = (1 - 0.5f) x press/p80 + 0.5f x social/p80. A member with no social value gets sv = 0, so at f = 1 the press side is halved and silence scores as zero, not missing (confirmed at lines 29, 463, 479). The downstream Press Release - Raw link was not checked. |

**Assessment.** **Evidence weight: weak. The decay constants are unsupported.** There are 3 rows: 1 VERIFIED (Sotoudeh 2024), 1 VERIFIED-SOFTENED and 1 UNVERIFIED.

**The half-lives (Sotoudeh 2024, VERIFIED).** The 6-month half-life is one of three settings for decaying cosponsorship-influence capital, with bill passage as the outcome. Discrimination barely changes between 6 and 24 months (0.248 vs 0.203). It cannot justify 0.5 years for social or 2.0 years for press, and its flatness warns that no single value is calibrated.

**Silence coded as zero (Hargittai 2015).** A member with no social value gets sv = 0. At f = 1 the press side is halved and silence scores as zero, not missing. This biases cells only if posters and non-posters differ in stance, which is untested.

**Coverage (Steinert-Threlkeld 2018, UNVERIFIED).** Every senator and 430 House members had Twitter accounts in 2016. That says nothing about 119th Congress platforms (X, Facebook, Truth Social, Bluesky) or CU-topic posting rates.

**Fink 2023: FLAG.** The system cites Fink et al. 2023 for author-volume-normalized social and press edges. That is the per-edge volume normalization this project measured and REJECTED. It is flagged and not recommended.

**Scan.** No study estimating a decay rate for legislator issue-stance salience in public statements was found.

**Contradictions.**
- Paul's notes cite Sotoudeh's 6-month half-life as a decay timescale for shifting relationships. In the paper it is one of three tested settings for cosponsorship-capital decay, with almost no discrimination difference out to 24 months. It is not an empirical stance half-life.
- Fink 2023 is cited in support of volume-normalized edges, a proposal this project measured and rejected.

**Gaps.**
- Half-life calibration. For a held-out quarter, predict members' observed stance from blended history under a grid of half-lives (0.25-4 years, press and social separately) and pick by out-of-sample fit. Preregister one look.
- Missing vs zero coding. Compare cell values with social silence treated as missing (press-only) vs zero, by category where f = 1.
- Incremental validity of social over press against any external criterion.

**Paul's position vs the evidence.** Paul calls for temporal weighting. That is reasonable, but his cited timescales are misread. Sotoudeh's 6 months is a tested setting with flat results, not an estimate, and Kim's 2-3-year fade concerns a different effect. The system's 2.0 / 0.5-year values come from neither, as the hub notes.

**Open questions.**
- How much would member-category cells move if social silence were coded missing rather than zero?
- Is the 0.5-year social half-life sensitive enough to change any member's sign?

**Read first.** [[sotoudeh-2024-network-based-measure]], [[hargittai-2015-bigger-always-better]], [[steinertthrelkeld-2018-twitter-data]]

## Legacy CU press regex stance and graded tone scorer

**What it computes.** Per release: CU stance via a regex ladder (default support), graded tone via TONE_LEXICON, and member lean, stance and negativity aggregates.

**Method.** Ladder: oppose > support cues > neutral bill cues > default support. GRADED_CUTS c(1,2,3.5,5.5,8). Polarity (Wp-Wn)/(Wp+Wn) per Rice & Zorn. Over-flagging recorded: 21 of 32 neutrals were wrongly scored. Superseded for stance on 2026-06-21 and still computed as a comparison column.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R`, `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/test_cu_scoring_offline.R`

**Assumptions.**
- CU-mention sentences carry the release's CU stance
- Default stance is support

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[rice-2018-corpus-based-dictionaries]] | 2018 | method-source | Polarity = (sum Wp - sum Wn)/(sum Wp + sum Wn), with Wp and Wn built from tf-idf and cosine-weighted counts over 500 positive and 500 negative words taken from 300-dimensional GloVe vectors (window 50, minimum count 90) trained on the domain corpus, seeded by 10 positive and 10 negative words. Gains over LIWC and AFINN already appear at 5,000 documents, rise steeply to about 15,000, and show marginal gains after 30,000. | The formula transfers as written. The paper's validity evidence applies only to corpus-derived, similarity-weighted dictionaries, which this code does not build. The paper's own accuracy gains start at 5,000 documents; the CU-relevant set is 1,563 text-bearing releases. Paper says document-level polarity hides subject (p.27), which supports the stance split. | strong | Formula at cu_press_release_scraper.R:2799 matches (Wp-Wn)/(Wp+Wn). Wp and Wn come from the hand-curated TONE_LEXICON, whose weights are 1, 2 and 3 (confirmed), not corpus-derived cosine or tf-idf weights. expand_tone_lexicon() (line 2490 onward) departs from the paper: GloVe rank 50 (paper 300), window 5 (paper 50), term_count_min 5 (paper 90), a bipolar axis, 200 terms per side (paper 500), different seeds, 50-document floor. lexicon_candidates.csv is absent, consistent with never having run. The 2026-06-21 supersede claim (21 of 32 neutral releases) is unsupported in files checked (UNVERIFIED). |
| [[haselmayer-2017-sentiment-analysis-political]] | 2017 | contradicts | Sentence tonality is set by the single most negative dictionary word (max rule). A negator immediately before a word excludes that word, and an intensifier multiplies it by 1.25. Word scores are crowd-derived means on a 0-4 scale. Mean word scores with the max rule and booster/negation handling give r = 0.65 (n = 168 test sentences). Other aggregation combinations give r from 0.38 to 0.58. | This is a fidelity finding, not an empirical one. It matters for the display score_label magnitude and the comparison column only, because press_value and the press pillar do not consume the graded layer. No external claim that these graded labels follow Haselmayer and Jenny should be made. | strong | The graded layer header in cu_press_release_scraper.R cites Haselmayer and Jenny and says sentence tonality follows the strongest cue. The code does not: weighted_tone (about lines 2443-2475) sums every fired cue across all CU sentences into Wp and Wn, and graded_from_signed buckets \|Wp - Wn\| by GRADED_CUTS. The negator window covers two preceding tokens; a 0.75 downtoner is added (line 139), not in the paper. TONE_LEXICON has 44 hand-weighted entries (weights 1, 2, 3), not crowd-derived scores. run_stage5.R lines 146-160 reuse the magnitude only for display score_label; press_value does not use it. |

**Assessment.** **Evidence weight: strong, as a fidelity finding against the code.** There are 2 rows: 1 VERIFIED (Haselmayer & Jenny 2017, contradicts) and 1 VERIFIED-SOFTENED (Rice & Zorn 2018).

**Actual error: the header misstates the aggregation rule.** The graded-tone header in cu_press_release_scraper.R cites Haselmayer & Jenny and says sentence tonality follows the strongest cue (the max rule, r = 0.65 on 168 test sentences). The code does not do this.
- weighted_tone (about lines 2443-2475) sums every cue across all CU sentences.
- It uses a two-token negator window and adds a 0.75 downtoner that is not in the paper.
- TONE_LEXICON has 44 hand-weighted entries (weights 1/2/3), not crowd-derived 0-4 scores.
- In Haselmayer & Jenny, the alternative aggregations score r = 0.38-0.58, so the rule matters.
- Impact is limited: only the display score_label and the comparison column consume this layer, not press_value.

**Rice & Zorn 2018: formula only.** The polarity formula (Wp-Wn)/(Wp+Wn) is implemented as written. The paper's validity evidence is for corpus-derived, similarity-weighted dictionaries, which this code does not build.
- expand_tone_lexicon() departs on every parameter: GloVe rank 50 vs the paper's 300, window 5 vs 50, minimum count 5 vs 90, and 200 vs 500 terms per side.
- lexicon_candidates.csv is absent, so it has likely never run.
- The paper's gains start at 5,000 documents; the CU corpus has 1,563 text-bearing releases.

**Supersession claim.** The 2026-06-21 supersede claim (21 of 32 neutral releases over-flagged) is UNVERIFIED in the files checked.

**Contradictions.**
- The code header claims Haselmayer & Jenny's strongest-cue rule, but weighted_tone sums all cues. The VERIFIED row shows this is an implementation contradiction.
- The Rice & Zorn citation implies a corpus-derived dictionary, but the lexicon is hand-curated and the expansion routine has apparently never run.

**Gaps.**
- None needed for production, since press_value does not consume this layer. If the comparison column is kept, the header should state the real rule, or the code should implement max-rule aggregation.

**Paul's position vs the evidence.** Paul has no position (Rice & Zorn is on disk, unannotated). The error is in the code's self-description, not in Paul's view.

**Open questions.**
- Should the graded layer be retired, given that it is superseded for stance and only feeds a display label?

**Read first.** [[haselmayer-2017-sentiment-analysis-political]], [[rice-2018-corpus-based-dictionaries]]

## ACU speaker mode (planned)

**What it computes.** Treats ACU-authored press (816 rows, 243 members) as ACU-to-member friend or foe labels.

**Method.** The NLI hypothesis is branched by speaker_source; keyword extraction was rejected. Validation gate: held-out accuracy >= 0.851, and Marshall/Hawley ACU rows must net negative. Planned 2026-07-01 and blocked on labels.

**Code refs.** `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/ACU_SPEAKER_MODE_PLAN.md`

**Assumptions.**
- ACU endorsing a member means pro-CU; ACU criticising a member means anti-CU

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| (none: GAP) |  |  |  |  |  |  |

**Assessment.** **GAP: no evidence rows.**

**The plan.** Branch the NLI hypothesis by speaker_source so that ACU-authored press (816 rows, 243 members) is read as ACU-to-member friend or foe. The validation gate: held-out accuracy of at least 0.851, and Marshall and Hawley ACU rows must net negative. It is blocked on labels.

**What the existing literature does and does not show.**
- Paul's cited sources (Box-Steffensmeier et al. 2018; Furnas et al. 2022) concern endorsements as cues to legislators and source identity in persuasion.
- They support the idea that an interest group's statement about a member is a distinct signal.
- None measures an interest group's stance toward a member from its own text.

**Bearing from other components.** The frame-alignment finding shows ACU syndication dominates the cross-member duplicate signal. ACU rows are distinct in kind, which supports separating them. They are 816 rows carrying only 349 distinct titles, so the effective sample is closer to 349 than 816.

**Scan leads (unread, UNVERIFIED).**
- Political DEBATE's training data includes DC Inbox newsletter sentences and quote tweets labelled for stance toward 20+ members of Congress. Hypotheses of the form 'The author of this document supports (member)' are native to the model.
- The paper reports that quote-tweet stance attribution (two speakers in one text) is its hardest dataset.
- ACU articles quoting members pose the same attribution problem.

**Contradictions.**
- (none)

**Gaps.**
- Needed study: a labelled set of distinct ACU titles (not 816 syndicated rows) coded for ACU's stance toward each named member, split by title, not row, so syndicated copies do not leak across folds. Score member-targeted hypotheses ('ACU supports [member]') against the 0.851 gate.
- No literature on interest-group-to-legislator stance measured from group-authored text; Kim 2025 measures group positions on bills, not on members.

**Paul's position vs the evidence.** Paul's highlights (well-connected endorsers are strong cues; source identity shapes receptiveness) support treating ACU text as a separate signal. They are about effects on recipients, not about measuring ACU's stance, so they motivate the component but do not validate it.

**Open questions.**
- Should validation be on the 349 distinct titles rather than the 816 rows?
- How are ACU articles that quote a member separated from ACU's own stance toward that member?

**Read first.** [[kim-2025-measuring-interest-group]], [[burnham-2023-stance-detection-practical-guide]], [[joo-2022-image-data-automated]]

## Stance dynamics and neighbour imputation

**What it computes.** Quarterly (91-day) stance trajectories in seven classes, plus imputation proposals for missing stances from network neighbours.

**Method.** Static band = median + MAD over observed-to-observed transitions (floor .05). Imputation needs at least 5 neighbours and |estimate| >= 0.75; 307 proposals; masking validation gives 77% sign agreement. Proposals only; the matrix is not changed. Co-movement was null on the cosponsor and HCoI layers.

**Code refs.** `fec/stance_dynamics.py`

**Assumptions.**
- Stance is locally similar among network neighbours (co-movement test was null)

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[zareie-2020-similarity-based-link]] | 2020 | suggests-extension | Many existing edges join nodes with no common neighbour: 53.7% in YST, falling to 3.1% in UAL. Correlation of second-order neighbourhood vectors still separates test from non-existing edges for such pairs, and DICN is best in 5 of 9 networks there. | The paper predicts edges, not node attributes, so at most it supports a second-order similarity weight for choosing which neighbours to average. Any second-order variant is a proposal that must beat a party-only baseline on masked cells. Weak. | weak | The silent-member imputation proposal averages stance-tier scores over direct cosponsor-graph neighbours only. It needs at least SD_MINNB = 5 informative neighbours and \|estimate\| >= SD_CONF = 0.75. Members below that get no proposal. The masking check counts sign agreement only on confident estimates. |
| [[baptista-2022-universal-multilayer-network]] | 2022 | suggests-extension | RWR propagates proximity from seeds over several hops and across layers and is validated by LOOCV. Combining layers helped in the gene-disease task: gene multiplex plus disease monoplex beat PPI alone and the gene multiplex alone. The drug layer with low bipartite overlap added nothing, while the airport layer with high overlap helped. The paper does not state that layers help only when overlap is high in the gene-disease case. | A per-member RWR seeded on known stances could replace the one-hop mean, and the existing masking protocol already has the form of LOOCV. Neighbour co-movement was null, so a multi-hop walk may not help either. This is a candidate needing a prereg and a single look, not evidence. Stance labels are Paul's domain: proposals only, and the matrix is never edited. | weak | Confirmed: imputation is the mean tier score of a member's direct neighbours on the cosponsor graph only (adjacency built at fec/stance/stance_dynamics.py:90), with at least 5 neighbours (MINNB) and absolute estimate at least 0.75 (CONF). HCoI is used only for the co-movement test. The proposal file data/stance/stance_imputation_proposals.csv has 308 lines including the header, so 307 proposals is reproduced. The 77% masking sign agreement was not reproduced. |
| [[malaterre-2024-inferring-social-networks]] | 2024 | supports | HCoI ties are links from thematic similarity of texts, mediated by the texts rather than social ties. The paper does not discuss stance, agreement or change in agreement. | The paper's silence on stance supports keeping HCoI out of stance imputation only as an argument from the construct, not from the paper's evidence. The null result needs a labelled layer in the output before it is cited. | moderate | stance_dynamics.py runs a neighbour co-movement test on HCoI same-community pairs (lines 111-121). Imputation proposals use only cosponsor adjacency; hadj is used only in the co-movement test. Thresholds: at least 5 neighbours (SD_MINNB) and confidence at least 0.75 (SD_CONF). The stored stance_dynamics.json co_movement block reads same 134, opposite 122, which is 52 percent with no layer label, so the 49 percent null is not reproduced. |
| [[zhou-2026-measuring-stance-dynamics]] | 2026 | method-source | Stance is tracked as a trajectory over 28-day snapshots, with embedding persistence so silent users keep a stance. Movement is sorted into seven dynamics using a static band of median plus MAD. Mirror-image O-to-S and S-to-O flip curves suggest external events. Users with no direct stance are predicted through graph propagation over their neighbours. | The design vocabulary transfers. The paper says the mirror symmetry suggests external events; it does not establish an event detector the module can rely on. The paper's neighbour inference is GNN message passing over learned embeddings, not a copy rule, so the system's neighbour-based imputation is an analogue only. | strong | Module docstring (lines 3-21) states the transplant. Code confirms 91-day default windows (line 26), last observed stance carried through silent windows (lines 48-55), the seven-dynamics classifier (lines 64-67), flip curves (lines 72-84), cosponsor and HCoI co-movement (lines 94-120), and CSV-only imputation proposals that leave the matrix untouched (lines 132-151). Re-run on the press CSV in scratch: 2,388 rows, 373 members, 18 windows. |
| [[zhou-2026-measuring-stance-dynamics]] | 2026 | qualifies | The median plus MAD static band is applied to continuous stance values p in [-1,1] from the MLP, computed over all users' absolute changes. The paper gives no condition that users must post in nearly every period; its 10-tweet filter only implies about one interaction per snapshot on average. | The band means what the paper intends only where stance is continuous and densely observed. With pooled quarterly signs it mainly measures how coarse the labels are. The paper gives no support for the seven-class counts. The within-side classes are rare because a move must exceed about 0.71 on a [-1,1] scale; they are not empty by construction. | strong | Band is computed on observed-to-observed transitions only, from confidence-weighted means of stance_sign in {-1,0,+1}, floored at 0.05 (code lines 56-63). Live static_band 0.7076 is reproduced. Of 2,989 transitions, 2,737 (91.6 percent) are static and 252 move: S-to-O 111, O-to-S 43, S-to-MS 88 (a move with no sign change), S-to-LS 7, O-to-LO 2, O-to-MO 1 (flip_curves in stance_dynamics.json, matching the scratch re-run). |
| [[zhou-2026-measuring-stance-dynamics]] | 2026 | qualifies | Neighbour co-movement: users shift in the same direction as their 1-hop and 2-hop neighbours (diagonal dominance in Fig. 6). The authors describe tight-knit communities moving together, call it a descriptive correlation, and report no null model and no identification. The text gives no numbers; values appear only in the heatmaps. | The paper's result is a descriptive correlation at 28-day resolution on Twitter, not evidence of influence. Do not carry the paper's potential social influences wording or its asymmetric social influence signals claim into system text. A null on one layer is a null on that layer only. | moderate | Co-movement counts same versus opposite direction for neighbours who both move in a window. Diagonal dominance is declared above 0.55 (code line 106). Cosponsor result as stored: same 134, opposite 122, 52.3 percent, below threshold, so null. The HCoI figure of 49 percent cited in the rel is not stored in stance_dynamics.json. Scratch re-run on the current press_hcoi.json gives same 302, opposite 296, 50.5 percent, also below threshold. No permutation or common-shock control exists. |
| [[zhou-2026-measuring-stance-dynamics]] | 2026 | qualifies | Silent-user inference is validated with 5-fold cross-validation over annotated users. For validation users, all edges to referendum-labelled hashtags are removed before training and inference. Scoring is macro-F1 per period on that period's validation users, chosen over accuracy because of class imbalance. Reported TWLGCN levels are steady between 70 and 90 percent. | The paper's protocol retrains a model with masked own-edges and reports macro-F1 across folds. The system's check is a neighbour-only estimate scored by raw sign agreement on a confidence-selected subset. Call it an analogue. The 77.5 percent is not comparable to the paper's macro-F1 range and stays UNVERIFIED (DE-062) until a party or majority baseline is run. | strong | Imputation takes the mean tier score of at least 5 cosponsor neighbours (MINNB=5, line 132) and keeps blank member-issue cells with \|est\| at least 0.75 (CONF, line 132). Masking check compares the neighbour estimate sign with the known label sign on confident cells only: 77.5 percent on n=1,087 (stance_dynamics.json). Proposals: 307 (CSV has 308 lines including header). True Neutral (score 0) counts as a negative sign (line 143). The final print says at least 3 informative neighbors while the threshold is 5 (line 152). No party-only or majority-class baseline, no macro-F1, no retraining, no 5-fold split. |
| [[zhou-2026-measuring-stance-dynamics]] | 2026 | qualifies | Carrying learned embeddings and MLP weights forward between snapshots beats re-initialising each period, by 1.3 to 9.8 percent on Exit_DS. The 28-day window was chosen by a sensitivity analysis on giant-component fraction: shorter windows fragment the graph and longer ones mask rapid shifts. | The paper's persistence gain is for warm-started learned representations that keep updating from new edges. It does not support freezing a categorical label. The window rule can be borrowed, but it needs per-window graphs the press corpus does not supply. Fig. S2 is in the supplement and was not checked. | moderate | Persistence carries the last observed raw stance into silent windows (lines 48-55), not learned embeddings. Carried windows also enter transition counts (lines 69-73), which inflates static counts and credits flips to the window where posting resumes. The 91-day default (line 26) is justified by an analogy comment about quarters, with no sensitivity run. |
| [[zhan-2025-cost-minimized-stage]] | 2025 | suggests-extension | Missing preference entries are imputed from the K=4 most trusted neighbours with a known value. Each weight blends trust and preference similarity (mu = (t + sim)/(1 + sim), then normalized), and entries are filled in order of reliability r and iterated to convergence. In the worked case f10_45 = 0.4033 comes from 4 neighbours weighted 0.2549/0.2507/0.2503/0.2440, the highest-reliability gap (r = 3.8683) of 39 missing entries. Imputation MSE stays at 0.09-0.13 across p in {20..100}, n in {4..12} and missing rates 0.1/0.2/0.3, on synthetic data only. | Only the mechanics carry over: neighbour weights from tie strength plus profile similarity, and filling the best-supported gaps first. The paper's evidence does not transfer. Its MSE comes from random synthetic matrices; its trust network is elicited trust among decision-makers, not cosponsorship; its alternatives are 5 investment options, not legislators' CU stance categories. Its homophily premise is assumed, never tested, while this system measured co-movement as null on cosponsor ties. A weighted variant should be adopted only if a masked-cell test beats both the current unweighted mean and a party-only baseline on held-out members. Until a second path reproduces that result it stays UNVERIFIED (DE-062). Weights must be labelled heuristic tie strength, never trust or closeness (D-025). | weak | The silent-member proposer takes each member's cosponsor neighbours as an UNWEIGHTED binary adjacency set (edge weights are dropped when the Edges sheet is read). It averages the neighbours' tier scores per category, with no similarity weighting, no reliability ordering and no iteration. It proposes a cell only when there are at least 5 neighbours (SD_MINNB) and \|est\| >= 0.75 (SD_CONF). It checks sign agreement on cells whose stance is already known, with no party-only baseline, and outputs proposals only. The same script reports co-movement as null on cosponsor ties. |
| [[cao-2025-social-network-group]] | 2025 | context | SNGDM consensus mechanisms model opinion updating along trust edges with DeGroot and Hegselmann-Krause dynamics (refs 9 and 71). The review treats network-driven opinion convergence as the core mechanism but reports no test of it. | DeGroot assumes that neighbours' opinions move a node's opinion. That holds by design in elicited SNGDM panels. The system's own null co-movement result means the assumption is not supported on congressional layers. This paper adds no evidence either way, so the imputation stays a proposal-only heuristic. | weak | Neighbour imputation takes an unweighted mean of neighbours' tier scores, which is one DeGroot-style averaging step. It requires at least 5 neighbours (MINNB) and \|est\| >= 0.75 (CONF), and it only proposes values without changing the matrix. Measured co-movement on the cosponsor and HCoI layers was null. |
| [[ma-2022-adversarial-attack-graph]] | 2022 | suggests-extension | Perturbing features of only 1 percent of nodes, chosen by an influence-maximization criterion, sharply lowers accuracy of GCN, GAT and JK-Net. Cora GAT falls from 87.7 (no attack) to 55.9 under InfMax-Unif at the 10 percent degree threshold. The paper's model (B = M^L) implies that propagation lets a few nodes affect many predictions; no propagation ablation is reported. | The system has no GNN and uses a one-hop mean with human adjudication, so propagation is shallow and an adversary is implausible. The transferable idea is a sensitivity test: flip or corrupt the labels of a few high-coverage members, chosen by walk coverage rather than at random, and count sign changes in the proposals. Any result stays UNVERIFIED until reproduced (DE-062). Weak. | weak | Neighbour imputation takes the one-hop mean TSCORE of at least 5 labelled cosponsor neighbours and keeps proposals with \|est\| at least 0.75. The masking validation sign agreement is stored in data/stance/stance_dynamics.json as 0.775 over n = 1087. Proposals go to a CSV only; the matrix is unchanged. |
| [[ringe-2013-pinpointing-powerful-voting]] | 2013 | qualifies | The connectivity to agreement link depends on whether the pair are expected allies or adversaries (bloc). Same-bloc dyads co-vote in 574 of 594 cases (about 97 percent), so H2 is untestable. For opposed-bloc pairs more contact predicts less co-voting (p. 625; Table 3). | The paper outcome is votes, not stance labels, so transfer is conceptual. The next diagnostic is a party-only baseline and a split of same-party from cross-party neighbours. The 77 percent figure stays UNVERIFIED as a network effect until that baseline exists (DE-062). | moderate | Neighbour imputation takes the unweighted mean of TSCORE over every cosponsor neighbour, with at least 5 neighbours (SD_MINNB) and absolute estimate at least 0.75 (SD_CONF). It does not condition on party. Masking validation reports sign agreement but has no party-only baseline. The co-movement test on cosponsor and HCoI ties is reported as null. |
| [[selfhout-2010-emerging-late-adolescent]] | 2010 | method-source | Stochastic actor-oriented models estimate tie selection conditional on the current network state, with ego, alter and similarity terms for each covariate, and pool groups by multilevel meta-analysis. The cited extensions (Steglich et al. 2006; Burk et al. 2007) model networks and behaviour together, which separates selection from influence. | Transfer is moderate in method only. The paper itself models selection and not influence, so it gives no evidence on stance contagion. A joint network-and-behaviour SIENA model is the standard way to ask whether ties drive stance change or stance similarity drives ties. That would need panel waves of cosponsor ties and quarterly stances. It would be new analysis, UNVERIFIED until reproduced, and its pooled estimates must be reported by stratum (party, chamber) or marked UNSTRATIFIED. | weak | Neighbour co-movement counts members who move in the same versus the opposite direction as their cosponsor or HCoI neighbours, quarter to quarter. The result was null on both layers. Neighbour imputation proposals are built on this (77% sign agreement under masking). |

**Assessment.** **Evidence weight: strong as design precedent, strong against overclaiming.** There are 13 rows: 2 VERIFIED, 8 VERIFIED-SOFTENED and 3 UNVERIFIED. This is the most-evidenced component.

**Design template (Zhou & Elejalde 2026).** The module transplants Zhou's 28-day trajectories, seven dynamics classes and silent-user inference to 91-day press windows.

**Departures from Zhou that matter.**
1. *The static band.* Zhou applies the median + MAD band to continuous, densely observed stance. Here it runs on pooled quarterly signs, where it mostly measures label coarseness. The band is 0.7076, and 2,737 of 2,989 transitions (91.6%) are static.
2. *Persistence.* Zhou's gain (1.3-9.8%) is from warm-starting learned embeddings. Here a frozen categorical label is carried forward, and the carried windows enter transition counts. That inflates the static count and credits flips to the window where posting resumes. This is a code-level error.
3. *Window length.* Zhou chose 28 days by sensitivity analysis. The 91-day window here rests on a quarters analogy with no sensitivity run.
4. *Silent-member inference.* Zhou retrains with masked edges and reports macro-F1 over 5 folds (70-90%). The system's check is raw sign agreement on confidence-selected cells (77.5% on n = 1,087), with no party-only or majority baseline. It is not comparable and stays UNVERIFIED.

**Co-movement: null, and the wording must stay out.** Zhou's diagonal dominance is a descriptive correlation with no null model. The system found co-movement null on both layers. Cosponsor: 134 same vs 122 opposite (52.3%). HCoI re-run: 302 vs 296 (50.5%). Do not carry Zhou's 'social influence' wording into system text.

**Party (Ringe 2013, VERIFIED).** The link between connectivity and agreement flips by bloc. Same-bloc dyads co-vote 574 of 594 times. That makes a party-only baseline, and a same-party vs cross-party neighbour split, the required diagnostic before 77.5% means anything.

**Other rows.**
- *Extension ideas, all proposals that must beat the unweighted mean and a party baseline on masked cells:* Zareie (second-order similarity), Baptista (random walk with restart), Zhan (weighted neighbours, UNVERIFIED) and Ma (corrupt a few high-coverage members' labels as a sensitivity test).
- *Assumptions the system's own null undercuts:* Cao (UNVERIFIED) and the homophily premise.
- *Selfhout (UNVERIFIED):* names SIENA as the standard way to separate selection from influence.

**Malaterre 2024.** HCoI ties are textual similarity, not social ties. That supports keeping HCoI out of imputation, by construct.

**Smaller code defects.** True Neutral (score 0) counts as a negative sign (line 143). The final print says at least 3 neighbours while the threshold is 5 (line 152).

**Fink 2023 flag.** It is cited here as context; its volume-normalized edges are the rejected proposal (see media-blend).

**Contradictions.**
- Zhou reports neighbour co-movement (diagonal dominance). The system measured co-movement null on cosponsor (52.3% of 256) and HCoI (50.5% of 598) ties. The imputation proposer nonetheless averages cosponsor neighbours, a DeGroot-style step whose premise the system's own test does not support.
- The persistence implementation (frozen labels counted as static transitions) contradicts the mechanism behind Zhou's persistence gain (warm-started embeddings).

**Gaps.**
- A party-only (and majority-class) baseline on the same 1,087 masked cells, plus a same-party vs cross-party neighbour split (Ringe). Until then, 77.5% is not a network effect.
- A permutation or common-shock null for co-movement (shuffle neighbour identities within party x chamber).
- A window-length sensitivity run (e.g. 30 / 60 / 91 / 182 days).
- Transition counts excluding carried windows.
- A selection vs influence model (SIENA-style), which would need panel waves of cosponsor ties and quarterly stances, reported by party and chamber stratum.

**Paul's position vs the evidence.** Paul wants temporal dynamics and change-likelihood modelling. The design follows that, but his notes predate the null co-movement result. That null, plus Ringe's bloc dependence, means neighbour-based change prediction is not supported on these layers yet.

**Open questions.**
- What is the party-only baseline's sign agreement on the 1,087 masked cells?
- How many of the 2,737 static transitions involve a carried (silent) window?

**Read first.** [[zhou-2026-measuring-stance-dynamics]], [[ringe-2013-pinpointing-powerful-voting]], [[malaterre-2024-inferring-social-networks]], [[zareie-2020-similarity-based-link]], [[baptista-2022-universal-multilayer-network]]

## Frame alignment (cross-member duplication rate)

**What it computes.** Per-category share of near-duplicate text across members, plus party framing chi-square.

**Method.** 3-gram shingle Jaccard >= 0.5. Reported duplication: CU Tax 65%, General CU 60%, Defense 12%. It feeds salience intensity (0.5 + alignment).

**Code refs.** `fec/frame_alignment.py`

**Assumptions.**
- High shingle overlap indicates coordinated messaging

**Evidence.**

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[joo-2022-image-data-automated]] | 2022 | method-source | Framing = share of duplicate images within each event-label; two images are duplicates when the normalized distance between their 1,000-dim last fully connected layer vectors is below .2. Table 3: Venezuela state violence .308 vs Hong Kong contains-child .003 (paper: almost 103 times). The paper frames this as measuring which frames protesters choose to share. | Carries over only when the duplicated unit was chosen by the actor being scored, as Joo's sharers chose their images; here that means own-domain releases. Third-party ACU syndication measures the author's reach, not member coordination. Joo's frame labels come from a trained classifier; here they are press categories, and short titles under 3-gram Jaccard >= 0.5 are not comparable to a CNN-feature distance threshold. | strong | Ports the duplication rate to press titles: an item counts as aligned if another member's title in the same category has 3-word-shingle Jaccard >= 0.5. SRC is the full cu_press_stance_tone.csv (2,388 rows) with no domain filter. 816 rows are americascreditunions.org articles; they carry 816 distinct URLs but only 349 distinct titles, with up to 19 members on one title, so one ACU headline attributed to many members scores as cross-member duplication. Recompute (reproduced on a second path with identical numbers; window = whole CSV, file dated 2026-06-30): General Credit Union 60.2% of 442 items -> 5.2% of 116 own-domain (*.senate.gov/*.house.gov) items; Credit Union Tax Status 64.7% of 190 -> 38.1% of 42; Defense 12.2% of 41 -> 13.9% of 36; ACU-only rows 79.8% of 326 (General) and 72.3% of 148 (Tax). |
| [[joo-2022-image-data-automated]] | 2022 | qualifies | The .2 duplicate threshold was chosen from inspecting the distance histogram, with no sensitivity analysis reported. The authors call the results provisional and suggest other operationalizations such as number of tweets containing a label instead of percentage, and expanding the frames considered. | Applies directly. Before any alignment figure is quoted, sweep the threshold (e.g. 0.3-0.7) on own-domain rows. The paper supplies no threshold validation the system could borrow. | moderate | The docstring calls Jaccard >= 0.5 the analog of Joo's .2 distance and inherits the same untested choice. There is no threshold sweep; the only audit check is that alignment rates fall in [0,1]. Echo clusters use the same 0.5 cut with a minimum group of 3 items. |
| [[joo-2022-image-data-automated]] | 2022 | qualifies | Chi-squared tests of label against party are run separately on male and female images because the two parties have different gender ratios. Labels are sorted by chi-square with per-label significance stars; no multiple-comparison correction is described across the 1,730 unique labels. | Joo's design stratifies on a known compositional fault line. Here the fault lines are chamber and author domain. Until stratified, quote the chi-square figures as UNSTRATIFIED descriptive term lists, not party-framing findings. | moderate | The party-framing block computes chi-squared of press category and title terms against party (D vs R), pooled across chamber and source domain, with no strata and no correction. The pooled input is 1,231 Senate and 1,157 House rows, 816 (34%) of them ACU-authored. The brain note quotes 'D to CU-Bank Dynamics (chi-square = 128)' with no stratum or window. |

**Assessment.** **Evidence weight: strong, and it overturns the current headline.** There are 3 rows: 1 VERIFIED and 2 VERIFIED-SOFTENED, all from Joo & Steinert-Threlkeld 2022.

**When the measure transfers.** Joo's duplication rate measures frames chosen by the sharers themselves. Here that means own-domain releases (*.senate.gov / *.house.gov) only.

**What the current figures actually measure.** The source CSV includes 816 americascreditunions.org rows carrying only 349 distinct titles, with up to 19 members on one title. Window: whole cu_press_stance_tone.csv, 2,388 rows, file dated 2026-06-30. Restricting to own-domain items:
- General CU duplication falls from 60.2% (of 442 items) to 5.2% (of 116).
- CU Tax Status falls from 64.7% (of 190) to 38.1% (of 42).
- Defense, with little ACU content, stays about the same: 12.2% of 41 vs 13.9% of 36.

The reported 60-65% figures measure ACU syndication reach, not member coordination. They feed salience intensity (0.5 + alignment), so the error propagates.

**Threshold (VERIFIED).** Joo's 0.2 threshold came from inspecting a histogram with no sensitivity analysis. The system's Jaccard >= 0.5 inherits an untested choice. Sweep 0.3-0.7 on own-domain rows before quoting any rate.

**Party framing.** Joo stratifies chi-squares by a known compositional fault line (gender). The system pools Senate and House (1,231 / 1,157) and ACU vs own-domain (816 ACU rows, 34%), with no correction. 'D to CU-Bank Dynamics (chi-square = 128)' is therefore an UNSTRATIFIED descriptive figure, not a party-framing finding.

**Scan.**
- Text-reuse work on Congress (e.g. Wilkerson, Smith & Stramp 2015, AJPS) treats shared n-grams as evidence of a common source.
- A 2019 press-copy study warns that newswire, PR and common-source overlaps are legitimate and must be separated before inferring coordination. That supports the ACU exclusion.

**Contradictions.**
- The reported duplication (General CU 60%, CU Tax 65%) vs own-domain duplication (5.2% of 116; 38.1% of 42). The headline measures ACU syndication, not cross-member framing alignment.
- The brain quotes a party-framing chi-square of 128 with no stratum or window, while Joo's own design stratifies on the compositional fault line.

**Gaps.**
- Recompute alignment and the salience-intensity input on own-domain rows only.
- A threshold sweep (Jaccard 0.3-0.7) with rates reported per setting.
- Chi-squares stratified by chamber and author domain, with a multiple-comparison correction.
- A hand-checked sample of high-overlap own-domain pairs to separate boilerplate and quoted bill text from shared framing.

**Paul's position vs the evidence.** Paul takes Joo's duplicate-rate framing as the template, which is sound. The implementation breaks Joo's precondition that the actor chooses the duplicated unit. That is an actual error in the current figures, not in Paul's choice of template.

**Open questions.**
- After restricting to own-domain rows, do any categories other than CU Tax show meaningful cross-member duplication?
- How much does salience intensity change when alignment is recomputed on own-domain rows?

**Read first.** [[joo-2022-image-data-automated]]

## Across components

**1. Every headline is a project-internal single look on a small set; no paper validates one.**
- DEBATE 0.839 (n = 87), SVM 0.816 (n = 87), two-hop 0.782 (400 golds), masking 77.5% (1,087 confidence-selected cells), LSD 'validated' (3-case smoke test).
- None has a reported interval.
- The provenance of two (0.782; SVM macro-F1) conflicts across vault notes.
- The common fix is bootstrap intervals and member-grouped splits, plus one fresh held-out gold set for each layer.

**2. Labels lean on Claude.** The DEBATE fine-tune falls back to Claude pre-labels, the 87.2% headline is agreement with Claude, and claude_stance confidence is self-stated. Ornstein (LLM overconfidence) and Burnham (no proprietary primary labels) both bear on this. Count the Claude-only rows.

**3. The neutral bin absorbs several failures.**
- Implicit support read as neutral, which is the dominant DEBATE error.
- ERR-coerced neutrals.
- True Neutral counted as negative in stance_dynamics masking.
- Social silence coded as zero in the media blend.

Each is a different object sharing a zero.

**4. Party is the unaddressed confound.** It runs through CFPB tiers (D 98 vs R 0, UNSTRATIFIED), frame-alignment chi-squares (pooled over chamber and ACU), and neighbour imputation (no party baseline; Ringe's bloc dependence). The diagnostic for all three is the same: within-party strata or a party-only baseline.

**5. ACU syndication contaminates two components.** It inflates frame alignment (60% falls to 5% own-domain in General CU), and it is the planned input to ACU speaker mode. Split ACU rows out everywhere and validate on the 349 distinct titles, not the 816 rows.

**6. Borrowed constants do not transfer.** Kim's 96.9%, Sotoudeh's half-life, b-2025's model ordering, Joo's 0.2 threshold, Zhou's band and persistence, and Haselmayer's max rule (claimed but not implemented) all fail to carry over.

**Wording and flags.**
- Fink 2023's author-volume-normalized edges = the per-edge volume normalization this project REJECTED. Flagged, not recommended.
- Neighbour weights are heuristic, never closeness (D-025).
- LDA-derived Kim positions are for adjudicating the bill table only (D-021).
- Do not import Zhou's 'social influence' wording.

## Research agenda

1. Frame alignment: recompute on own-domain rows only, sweep Jaccard 0.3-0.7, and stratify the party chi-squares by chamber and author domain. Then re-derive the salience-intensity input. This is the cheapest fix with the largest effect.
2. Stance dynamics: run a party-only and a majority-class baseline on the same 1,087 masked cells, split neighbours into same-party and cross-party, add a permutation null for co-movement, and drop carried windows from transition counts.
3. LSD intensity validation: have lay coders and an expert rate about 200 own-domain releases 0-4 for heat, then correlate with |lsd_net_norm| and lsd_charge, with and without domain-noun masking (Haselmayer & Jenny protocol). Needs the R runtime to reproduce the masking figures.
4. Stance readers: count the Claude-only rows in the 552-row fine-tune set, then bootstrap intervals for DEBATE vs SVM on n=87. Fine-tune Political DEBATE large, and test the training-template hypothesis ('The author of this document supports...'), on the same member-grouped split, with one preregistered look.
5. CFPB axis: test within-party whether CFPB tier tracks CFPB-relevant votes or cosponsorship beyond DW-NOMINATE dim1, and hand-audit the 89 cells moved to no-evidence.
6. Media blend: run a half-life grid (0.25-4 years, press and social separately) predicting held-out-quarter stance, compare zero vs missing coding of social silence, and preregister one look.
7. ACU speaker mode: label stance toward named members on the 349 distinct ACU titles, split by title, and score member-targeted NLI hypotheses against the 0.851 gate.
8. Two-hop: settle whether 0.782 is out-of-fold, then label 150-200 fresh items on targets unused in registry derivation, and report registry coverage vs flat fallback.
9. Bill directions: cross-check the 60-row hand table against Kim 2025 positions for 111th-117th bills (audit only, D-021).
10. Code-header and code fixes to put to Paul (not applied): the legacy scorer header claims the max rule; the derive_valence_registry header says 80% vs code 0.75; the stance_dynamics print says 3 vs threshold 5; claude_stance keeps the confidence on coerced-neutral rows.
11. Read the scan leads (unread, UNVERIFIED): the Political DEBATE paper (downloaded), Le Mens & Gallego 2025 (downloaded), Laurer et al. 2024 BERT-NLI (Political Analysis), and Wilkerson, Smith & Stramp 2015 text reuse.
