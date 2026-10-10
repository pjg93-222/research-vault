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
paper_count: 36
tags: [system]
---

# Melange stance and tone measurement (press and social text)

Turns member press releases, tweets and other posts into signed CU stance and tone. Covers the Political DEBATE NLI stance reader, Lexicoder tone, two-hop stance x valence composition, bill-sign handling, CFPB axis isolation, human-label overlay, the media blend with recency decay, ACU speaker mode, stance dynamics and frame alignment. It includes the legacy Draft Systems R regex scorer (Stage 5).

Brain notes: [actors/debate-stance-model](obsidian://open?vault=brain&file=actors%2Fdebate-stance-model); [concepts/two-hop-stance-decomposition](obsidian://open?vault=brain&file=concepts%2Ftwo-hop-stance-decomposition); [topics/2026-06-feature-cu-stance-and-sentiment](obsidian://open?vault=brain&file=topics%2F2026-06-feature-cu-stance-and-sentiment); [topics/2026-06-feature-cu-stance-and-sentiment-stance-stack-design-and-accuracy](obsidian://open?vault=brain&file=topics%2F2026-06-feature-cu-stance-and-sentiment-stance-stack-design-and-accuracy); [topics/2026-07-feature-influence-engine-internals-salience-sna-wave](obsidian://open?vault=brain&file=topics%2F2026-07-feature-influence-engine-internals-salience-sna-wave)

## Melange stance and tone measurement: evidence assessment

**What was read.** The hub file holds 12 components, Paul's positions and 71 evidence rows: 12 VERIFIED, 50 VERIFIED-SOFTENED, 9 UNVERIFIED. The rows are unevenly spread. Most fall on the DEBATE reader (15), stance dynamics (13) and LSD tone (12). CFPB axis isolation and ACU speaker mode have one row each, and bill-sign handling has two. UNVERIFIED rows are treated below as leads, not findings (DE-062).

**Overall verdict.** The literature mainly *qualifies* this system. It rarely endorses it. Only a few rows support a design choice directly:
- Burnham 2024 is the method source for the DEBATE checkpoint and NLI framing.
- Joo 2022 is the source for duplication-rate framing.
- Zhou 2026 is the source for the stance-dynamics vocabulary.
- Burnham 2023/2024 support the two-hop decomposition pattern: bespoke, outside-knowledge concepts are where NLI is weak.

Several rows find **fidelity gaps** between the cited source and the code:
- The graded tone header cites Haselmayer & Jenny, but the code sums every cue where the source takes the strongest one.
- The Rice & Zorn lexicon expander uses different settings and has never run.
- Stance dynamics freezes a categorical label where Zhou carries forward learned embeddings.

Three rows find **measurement problems that change quoted numbers**:
1. **Frame alignment.** ACU syndication drives the headline duplication rates. General CU falls from 60.2% of 442 items to 5.2% of 116 own-domain items (whole cu_press_stance_tone.csv, dated 2026-06-30).
2. **DEBATE headline.** The 87.2% figure is agreement with the Claude pre-labeller (156-row test), not accuracy against humans.
3. **Two-hop 0.782.** Rows disagree on whether this was measured under honest 5-fold CV or on the golds the design iterated against.

**Gaps.** No row validates:
- LSD intensity against human-coded intensity;
- any half-life against later votes;
- the CFPB family split;
- advocacy-group statements as predictors of member votes;
- sign x intensity as better than sign alone.

**Paul's positions vs the evidence.** His strongest stated position is that LLM and transformer readers make bag-of-words SVMs outdated. His own data does not bear this out at this corpus size: on n=87 held out, the tuned SVM and fine-tuned DEBATE tie at about 0.839. The external sources he cites (Kim's 96.9% and Birkenmaier's GPT-4o) do not transfer to this task.

## Fine-tuned Political DEBATE NLI stance reader

**What it computes.** Per-release (and per-tweet) stance toward credit-union interests: support, oppose or neutral (sign -1/0/+1).

**Method.** mlburnham/Political_DEBATE_base_v1.0 fine-tuned on cu_stance_label_set.csv (552 rows; seed 42; 3 epochs). A Claude rater pre-labelled and human labels override. CPU, about 0.3 s per document. Reported results: 87.2% vs Claude (distillation); about 85% / kappa 0.76 on a member-grouped human-gold test; ft accuracy 0.839 / kappa 0.742 held out; 0.796 on the 760-row gold frame; 0.571 on the adversarial goldeval frame. It is one of five readers in the stacked press stack and the fallback reader in build_media_blend.

**Code refs.**
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_debate.py`
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/finetune_debate.py`
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R`
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/claude_stance.py`
- `press_stance_stack.py; tweet_stance_stack.py`

**Assumptions.**
- Policy stance toward CUs is recoverable from text by NLI entailment
- DEBATE neutral means the text is not about the bill, not a missed stance
- The held-out test sets (87 or 156 rows) are representative

**Evidence.**

15 rows (3 VERIFIED, 10 VERIFIED-SOFTENED, 2 UNVERIFIED); 9 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2025-measuring-interest-group]] | 2025 | qualifies | GPT-4 zero-shot labelling at temperature 0 reaches 96.93% accuracy on 391 coder-agreed lobbying lines. The lines pass a keyword filter for position terms first (e.g. endorse, defeat, advocate, watch). | Does not transfer. Kim's text is pre-filtered to explicit position terms about a named bill, and the validation set excludes coder disagreements. Press stance toward CUs needs a target-valence step that Kim's task lacks. The 96.9% is not a benchmark for this reader. | weak | VERIFIED-SOFTENED |
| [[dai-2025-large-language-model]] | 2025 | suggests-extension | Fusing decisions from three LLMs (GPT-3.5-turbo-0301, Claude-2-100k, LLaMA-2-70b-chat) by attention-based routing (Eqs 7-11) beats a single-GPT-3.5 variant: 81.0 vs 75.2 on zero-shot VAST with 100 percent training data (+5.8), and 80.2 vs 74.9 with 10 percent (+5.3). The authors say LLMs often exhibit bias in knowledge elicitation that varies with training data and methods. | The paper's ablation confounds fusion with the number of LLMs (one versus three), so it does not isolate a fusion effect, and no variance is reported. The benchmark inputs are tweets for SEM16 and ISD, while the paper does not describe VAST's source text and gives no collection dates, so a 2016-2020 framing is unsupported. The named model snapshots are the paper's choices; their retirement is not checked here. A second LLM rater would expose labeller-specific bias before human override, but the evidence is weak and new agent output stays UNVERIFIED (DE-062). | weak | VERIFIED-SOFTENED |
| [[b-2025-comparing-fine-tuned]] | 2025 | qualifies | A fine-tuned RoBERTa reaches 0.97 accuracy on political stance. Misclassification concentrates in sarcastic, ambiguous and indirect statements, and some disagree-labelled items are predicted unrelated when explicit negation is absent. | The error-mode finding (implicit and indirect stance is the residual failure) transfers and matches the system's neutral versus support seam. The 0.97 headline should not be a target or prior because it comes from a non-common evaluation set. The DEBATE reader is an NLI entailment model, not a RoBERTa classifier, so the architectures differ. Treat the paper as weak qualitative support only. | weak | VERIFIED-SOFTENED |
| [[taherdoost-2023-artificial-intelligence-sentiment]] | 2023 | context | The paper asserts that AI understands the tone of a statement rather than just recognising words. Citing Alantari et al. 2022, it says neural-network ML gives the most accurate predictions of the text-based methods compared. The review reports no accuracy figures of its own, and its corpus is 43 business and competitive-research papers from 2012-2022. | Weak. The cited evidence covers consumer reviews and business social media, not congressional press releases. It also concerns sentiment polarity, not target-specific stance toward CU policy interests. It neither supports nor tests the system's validated accuracy, which comes from a stricter protocol (held-out, member-grouped human gold). | weak | UNVERIFIED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | supports | Tweets whose meaning depends on outside political context are misread unless the prompt supplies that context. The Masterpiece sentiment prompt included a preamble on the ruling and six few-shot examples, and GPT-3 got the sign right on 88.4 percent of 945 tweets. | The paper task is sentiment toward a court ruling, not stance toward an interest, so the paper gives no accuracy figure for CU stance. The paper used six few-shot examples; claude_stance.py is zero-shot with a policy map. The supported point is the design choice of supplying context in the prompt. | moderate | VERIFIED-SOFTENED |
| [[ornstein-2024-train-your-stochastic]] | 2024 | qualifies | GPT-4, tuned with RLHF, is poorly calibrated and overconfident. For over 70 percent of its posts it gave one label more than 99 percent probability, and it chose Neutral more often than the authors. GPT-4 was slightly worse than GPT-3 on sentiment and little or no better on ideology. Section 4 says newer RLHF models frequently underperform on classification and scaling tasks, and the latest model is not always the best. | These are GPT-3 and GPT-4 results, not Claude results. The module requests no token log-probabilities (none found in the file), so the paper probability-vector method cannot be applied as written. claude_conf is a self-stated number, not a calibrated probability. Neutral rows created by ERR should be counted apart from model-chosen neutral rows. No DEBATE-vs-Claude agreement figure was located in the sources checked. | moderate | VERIFIED-SOFTENED |
| [[vanderdoes-2022-strategic-identity-signaling]] | 2022 | qualifies | Covert tweets were defined partly by moderates rating them neutral in orientation and affect while far copartisans saw political content; the authors argue automated political-speech classifiers will likely detect only overt speech and miss covert signals. | Holds only if members code CU positions for mixed constituencies. The paper's covertness concerns partisan identity on a left-right axis among ordinary Twitter users, and CU issues are comparatively bipartisan, so the size of any neutral-bin leakage in congressional press is unknown. The classifier warning is the authors' conjecture, not a tested result. | weak | UNVERIFIED |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | supports | Ambiguity is 34.0 percent when Trump is not mentioned versus 19.5 percent when he is. NLI classifiers degrade most in low-context documents, while supervised and in-context classifiers adapt better. | The paper measured Trump approval with generic DeBERTaV3 NLI, not Political DEBATE on CU press releases. Transfer to CU is by analogy. | strong | VERIFIED |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | qualifies | Zero-shot MCC for DeBERTaV3 Small is 0.15, Base 0.40 and Large 0.65 (83 percent accuracy). Performance comparable to supervised classifiers emerges only in larger models. | The paper gap is for generic zero-shot NLI. Political DEBATE is a politics-trained model not studied in this paper, so the size gap may differ. | moderate | VERIFIED |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | qualifies | In-context LLMs are useful for expanding training data and for validation, but not as the primary label source because proprietary models cannot be archived. The paper reports GPT-4 MCC 0.78 in Table 5 and recommends temperature zero in Section 4.3.4. | Applies to any training row whose human_label is empty. The share of Claude-only rows is not counted in the analysis. | moderate | VERIFIED-SOFTENED |
| [[burnham-2024-political-debate-zero]] | 2024 | method-source | Political DEBATE base and large are DeBERTa V3 NLI models (starting from Laurer et al. 2023 NLI checkpoints) further trained on PolNLI, 201,691 documents and 852 hypotheses. Each document is paired with a templated hypothesis and labeled entail or not entail. Full-training base hyperparameters: learning rate 2e-5 for 20 epochs (Appendix C.1). Few-shot runs used the default learning rate for 5 epochs with no tuning. | Direct provenance: same released checkpoint and same NLI framing. Epoch count differs: 3 in the script, 5 in the paper's few-shot runs, 20 in the paper's full training. Whether 3 epochs at lr 2e-5 suits a 552-row fine-tune is untested against the CU gold. | strong | VERIFIED |
| [[burnham-2024-political-debate-zero]] | 2024 | qualifies | The paper recommends the large model for zero-shot and most few-shot use, and the base model for simple few-shot or supervised work. On COVID threat minimization the base model largely fails zero-shot and does not match the supervised Electra classifier (MCC 0.66) in accuracy at 100 samples; the text gives no base MCC. The large model matches the classifier at 10 samples (MCC 0.67, accuracy 87 percent) and exceeds it at 25 (MCC 0.69, accuracy 88 percent). On Polistance quote tweets, Claude 3.5 scored MCC 0.29, base 0.62 and large 0.88. | Applies only if CU stance with sign-flip targets counts as bespoke in the paper's sense, which the oppose F1 of 0.08 suggests but does not prove. The paper reports MCC on binary tasks; the system task is 3-class. The base-versus-large gap on the adversarial frame has not been measured. | strong | VERIFIED-SOFTENED |
| [[burnham-2024-political-debate-zero]] | 2024 | qualifies | The paper recommends short, simple hypotheses close to the training templates, and says tasks needing long or multi-condition hypotheses call for few-shot training. During training, neutral and contradiction were merged into not-entail (footnote on p.6), so neutral is not a trained positive concept. | Fine-tuning on 552 rows partly offsets the template mismatch. Whether splitting neutral into a not-about-CUs hypothesis plus an explicit neutral rule beats the compound hypothesis is untested. Press releases are longer than the paper's short-document zero-shot condition. | moderate | VERIFIED-SOFTENED |
| [[burnham-2024-political-debate-zero]] | 2024 | measurement-source | The paper uses MCC as its primary metric, chosen for its relative robustness compared with F1 and accuracy on binary classification (p.9). It excludes GPT-4o from the benchmark because GPT-4o was used in label validation, and it discourages benchmarking OpenAI models on PolNLI for that reason (p.9-10). The paper does not say this exclusion would inflate agreement. | Agreement with the LLM that generated the labels is not accuracy against humans. Quote only human-gold figures and name the denominator (156, 87 or 760 rows). Do not merge the 156-row 87.2 percent figure, the n=87 0.839 figure and the about 85 percent member-grouped figure. | moderate | VERIFIED-SOFTENED |
| [[lemens-2025-positioning-political-texts]] | 2025 | supports | Asking an LLM directly for a 0-100 position score (JSON only, temperature 0, NA allowed) gives text-level positions. GPT-4o r=0.92 against crowd means, but on the 557 of 899 tweets that received a score (NA excluded). API cost about 1.5 USD for 900 tweets versus 1,626 GBP for human coding. Table A2 ENO for the LLMs in the table is 3 to 7 human raters. | The paper tests GPT-4, GPT-4o, MiXtral and Llama 3 (Table 1 lists no Claude model) on left-right and subsidy dimensions against independent crowd and expert benchmarks. The system rater is categorical (support/oppose/neutral), not a 0-100 scale, and its gold labels were pre-labelled by the same rater. The paper's accuracy figures cannot be quoted as properties of the Claude rater. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: moderate on provenance, weak on accuracy.**

**Provenance.** Burnham 2024 (VERIFIED, method-source) confirms the code uses the same released checkpoint (Political_DEBATE_base_v1.0) and the same entail/not-entail framing. The training recipe departs from the paper: 3 epochs at lr 2e-5, against the paper's 5 epochs for few-shot runs and 20 for full training.

**Accuracy.** Burnham 2023 (VERIFIED) shows two things:
- NLI degrades on low-context and ambiguous text (ambiguity 34.0% without the target named vs 19.5% with it).
- Supervised-level zero-shot performance appears only in large models (MCC 0.15 / 0.40 / 0.65 for small / base / large).

Burnham 2024 (VERIFIED-SOFTENED) recommends the large model for bespoke tasks. Production runs base, chosen for CPU reasons. On the adversarial goldeval frame the base model scores 0.571, and base vs large has never been compared on that frame.

**Label provenance.** Training labels are human_label when one exists, otherwise the Claude pre-label (Burnham 2023, SOFTENED). The share of Claude-only rows is uncounted. That means part of any held-out score may measure distillation of Claude rather than agreement with humans.

**Error mode.** B 2025 (SOFTENED, weak) corroborates the residual error: implicit or indirect stance gets read as neutral. Its 0.97 headline is not a usable benchmark. The neutral hypothesis is a compound ('neutral toward or not about credit unions'), yet the model was never trained on neutral as a positive concept (Burnham 2024 footnote). Coded or hedged support therefore lands in the same zero bin as off-topic text; van der Does 2022 is an UNVERIFIED lead on this.

**Claude rater (claude_stance.py).** Ornstein 2024 supports supplying policy context in the prompt. It also warns that RLHF models are overconfident and over-choose Neutral. The code's confidence field is self-stated, not calibrated. An API failure produces neutral with confidence 0.0, and an unrecognised label becomes neutral but keeps its stated confidence. Lemens 2025 supports LLM direct scoring, but it tested GPT and open models on continuous left-right positions, not Claude on 3-class stance.

**Contradictions.**

- The 87.2% figure (156-row test) is agreement with the Claude reference. The 0.839 / kappa 0.742-0.760 figure is on n=87 human-gold held out. The 'about 85% / kappa 0.76' figure is a member-grouped human gold. These are three different denominators and are sometimes quoted together (Burnham 2024 measurement-source row). The kappa for the n=87 test is reported as 0.742 in the component text but as 0.760 in the Burnham 2024 row.
- Kim 2025's 96.93% GPT-4 accuracy is on 391 coder-agreed, keyword-prefiltered lobbying lines. It does not transfer as a benchmark for this reader, yet Paul's position cites it as support.
- Dai 2025 row: stage5_debate.py as inspected is a zero-shot pipeline with no training code, while the component describes a fine-tuned reader. The fine-tuning lives in finetune_debate.py. Which checkpoint stage5 actually loads must be confirmed.
- claude_stance.py: an unrecognised label is reset to neutral, but its stated confidence is kept. Model-chosen neutrals and ERR neutrals are not separated.

**Gaps.**

- No base vs large fine-tune comparison on the same member-grouped human-gold test and adversarial frame. Fill with a single-look comparison, which needs Runpod or another non-local machine given the CPU constraint.
- No MCC reported for the 3-class task. Report MCC alongside accuracy on human gold only.
- The share of training rows labelled only by Claude is uncounted. Fill by counting it and re-scoring with Claude-only rows excluded from evaluation.
- No test of splitting the compound neutral hypothesis into a 'not about CUs' hypothesis plus an explicit neutral hypothesis.

**Paul's position vs the evidence.**

Paul endorses LLM and transformer position extraction, citing Kim's 96.9% and Birkenmaier's GPT-4o over SVM. The evidence does not let those figures transfer: Kim's lines were pre-filtered for explicit position terms, and Birkenmaier studied a different task. Paul flags ambiguous statements as the weakness. That is the best-supported point in the evidence: Burnham 2023's ambiguity gap and B 2025's error analysis both locate residual error in implicit or indirect stance, which matches the system's support-read-as-neutral seam. His cited sources say nothing on base vs large, yet Burnham 2024 bears directly on that choice.

**Open questions.**

- What share of the 552 training rows carry only a Claude pre-label, and does held-out accuracy on human gold change when those rows are excluded?
- Does fine-tuned large beat fine-tuned base on the 0.571 adversarial frame?
- How many neutral outputs come from API errors (ERR) as opposed to model judgment?

**Read first.** [[burnham-2024-political-debate-zero]], [[burnham-2023-stance-detection-practical-guide]], [[ornstein-2024-train-your-stochastic]], [[b-2025-comparing-fine-tuned]], [[lemens-2025-positioning-political-texts]]

## Supervised and stacked stance readers

**What it computes.** Stacked stance label per tweet or press item from classical and API readers.

**Method.** Best classical model: LinearSVC on word+char TF-IDF, 83.9% / macro-F1 0.816 on untouched test (n=87). Khan-style multi-view augmentation was tested and hurt classical models. Supervised tweet stance reached 0.737 CV on 399 golds. Stanceformer is planned, not built.

**Code refs.**
- `train_stance_domain.py; stance_clf_composed.pkl`
- `fec/train_tweet_stance.py`

**Assumptions.**
- Target-specific labels are needed; sentiment is not stance
- The 456/87/217 split is never reshuffled; CV of about 0.90 is inflated and not quoted

**Evidence.**

8 rows (1 VERIFIED, 6 VERIFIED-SOFTENED, 1 UNVERIFIED); 7 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[birkenmaier-2025-measuring-politicians-public]] | 2025 | qualifies | On the paper's personality-cue task, few-shot LLM prompting (0.77) beats fine-tuned XLM-RoBERTa (0.70, strong balanced), which only slightly beats a bag-of-words SVM (0.63 strong balanced; 0.55 to 0.63 across variants). | Different task (CU press and social stance, not German agency and communion cues) and a different baseline: the paper's SVM is word-only with no C tuning. The system's ranking comes from its own held-out test, so this paper cannot rank the system's readers. The code's transformer defaults to distilroberta, not XLM-RoBERTa. | weak | VERIFIED |
| [[dai-2025-large-language-model]] | 2025 | qualifies | In-target (Table 3): FOLAR beats the best competitor by 1.7 on LA (72.7 vs COLA 71.0) and by 2.0 on DT_I (73.3 vs MPPT 71.3), but trails KEprompt on FM (70.6 vs 72.1). Zero-shot (Table 4): gains of +4.2 on VAST with 100 percent training data and +6.2 with 10 percent. The baselines include statistical, PLM, prompt and LLM methods. No TF-IDF or SVM baseline is reported. | The paper cannot settle SVM versus LLM for this corpus because it runs no bag-of-words baseline. Its pattern (small in-target gains, larger low-data or new-target gains) suggests an LLM-rule reader would help most on new or rare CU targets, which has not been tested for CU stance. The evidence is benchmark-level, rests on three runs and gives no confidence intervals. | weak | VERIFIED-SOFTENED |
| [[b-2025-comparing-fine-tuned]] | 2025 | method-source | Classical models on TF-IDF features rank RF 0.95 > SVM 0.94 > LR 0.93 > DT 0.92 > NB 0.89 on a 1,675-row test split. Fine-tuned RoBERTa on tweet plus comment paired input reports 0.97 accuracy on a support of 6,699 described as the augmented dataset. Augmentation and class balancing are credited with gains, but no with and without comparison is shown. | The mechanics (paired input, balanced loss, per-class P/R/F1 and macro-F1) transfer. The model ordering and the augmentation gain are not established by the paper: its classical rows are scored on 1,675 rows and its RoBERTa row on 6,699 rows. 6,699 is almost exactly 0.8 x 8,374 (6,699.2), the size of the 80 percent training share, so the RoBERTa figure may not be on a held-out set. The paper does not say which, so this is an inference and must be labelled as one. | strong | VERIFIED-SOFTENED |
| [[b-2025-comparing-fine-tuned]] | 2025 | qualifies | The transformer beats every classical model (0.97 vs at most 0.95). Dataset augmentation and class balancing are credited with improving performance, with no with and without comparison shown. | The system numbers are single-look on an 87-row test and were not re-verified here. They are not a refutation of the paper's own measurement; they show that its ordering and augmentation gain did not transfer to this small English press corpus. Relation changed from contradicts to qualifies, because the paper's claim is about its own data. | moderate | VERIFIED-SOFTENED |
| [[naz-2025-machine-deep-learning]] | 2025 | qualifies | Traditional ML (SVM and similar on TF-IDF) remains interpretable and competitive on small data. Deep and transformer gains appear with large labeled corpora. The text claims about a 25% LSTM gain over traditional ML, but no table supports the figure. | The paper studies personality, not stance, so this is a pattern about small labeled sets of a few hundred rows, not stance evidence. It fits the system's measured outcome. It does not show that transformers lose at larger n, and it says nothing about LLM zero-shot readers (Birkenmaier, Kim J.), which Paul's view rests on. | weak | UNVERIFIED |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | qualifies | Bag-of-words LR, RF and SVM reach MCC 0.49-0.51 and RoBERTa, BERTweet and PoliBERTweet reach 0.60, 0.63 and 0.68 on Twitter. Laurer et al. 2024 found 500 training examples let language models outperform algorithms trained on several thousand. | With 456 training rows and an 87-row test all readers fall within each other's intervals. The paper's ordering is for larger training sets and for MCC, not macro-F1. | moderate | VERIFIED-SOFTENED |
| [[burnham-2024-political-debate-zero]] | 2024 | qualifies | On the COVID threat-minimization task only, the large model matches a supervised classifier at 10 samples (MCC 0.67 vs 0.66) and exceeds it at 25 (MCC 0.69). The base model, per the text, fails to match the supervised classifier's accuracy at 100 samples. The claim is confined to that bespoke task. | The paper's claim concerns the large model, not the base. A fine-tuned base losing to a stack on a bespoke CU target is consistent with the paper. The SVM-versus-transformer pair is not a DEBATE result and 0.757 is unverified. The 760-row stack is a meta-learner, not a supervised classifier alone. | weak | VERIFIED-SOFTENED |
| [[lemens-2025-positioning-political-texts]] | 2025 | qualifies | LLM direct query beats text classifiers on the paper's own benchmarks. Tweets: GPT-4o r=0.92 versus fine-tuned BERT 0.78, tf-idf 0.66, GloVe 0.63. Senators: GPT-4o 0.97 versus BERT 0.93, tf-idf 0.90. The paper says no classifier matches the best LLMs on within-party differences. Its classifiers are typicality readers (naive Bayes tf-idf, BERT, GloVe) trained on about 1 million party-labelled Congress tweets, not stance classifiers trained on human stance labels. No linear SVM is tested. | The paper compares continuous left-right positions, scored by correlation, against party-typicality readers. The system compares three-class target stance, scored by accuracy, against a stance-trained SVM. The paper does not test a stance-trained linear model, so it cannot contradict the system's SVM-over-transformer result. Treat it as a head-to-head test to run. | weak | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: weak to moderate. No external row can rank the system's readers on this corpus.**

**What the external papers compare.**
- Birkenmaier 2025 (VERIFIED): SVM < XLM-R < few-shot LLM, but its SVM is word-only and untuned, on a different task.
- B 2025 (method-source for the Khan-style pipeline): its RoBERTa row may not be on a held-out set. Support of 6,699 ~ 0.8 x 8,374. This is an inference and is labelled as one.
- Lemens 2025: compares party-typicality readers, not stance-trained SVMs.
- Dai 2025: runs no bag-of-words baseline.

**What carries over.** Burnham 2023 (SOFTENED) makes the one relevant point. With 456 training rows and an 87-row test, all readers fall within each other's intervals. Its ordering (LMs beat BoW) is for larger training sets and MCC. Naz 2025 (UNVERIFIED) is a lead pointing the same way: classical models stay competitive on small labelled sets.

**The system's own data.** On n=87, tuned word+char LinearSVC and fine-tuned DEBATE both sit at about 0.839 accuracy, which is effectively a tie. The augmentation that B 2025 credits hurt here. That is a non-transfer to a small English press corpus, not a refutation.

**Contradictions.**

- The SVM macro-F1 is reported as 0.816 in the component text. Burnham 2023 row found vault notes giving 0.792 and 0.835 for the same model, and 0.816 paired with an 84.7% headline rather than 83.9%.
- The 0.757 transformer figure was not located in code or vault by two separate rows (Burnham 2023, Burnham 2024). Fine-tuned DEBATE at 0.839 / macro-F1 0.835 on the same n=87 ties the SVM. The claim 'SVM 83.9% vs transformer 0.757' therefore compares the SVM against a distilroberta run, not against the production transformer.
- train_tweet_stance.py has no untouched test split; it reports 5-fold CV over the golds. The component says CV of about 0.90 is inflated and not quoted, yet the 0.737 tweet figure is also a CV number.

**Gaps.**

- No head-to-head on one fixed split with intervals: tuned SVM vs fine-tuned DEBATE base vs an LLM few-shot reader, scored by macro-F1 and MCC with bootstrap CIs. Single look, n=87 is underpowered, so the agenda should first state the detectable difference.
- Stanceformer is planned but not built, and no evidence row tests target-aware attention on the neutral class.

**Paul's position vs the evidence.**

Paul calls bag-of-words SVM baselines outdated next to LLMs. On this corpus at this size the evidence does not support that. The system's own held-out test ties the tuned SVM with fine-tuned DEBATE. The papers favouring LLMs either used untuned word-only SVMs (Birkenmaier) or compared different reader types (Lemens). Burnham 2023 states the size condition (about 500+ training examples, larger tests) under which the ordering holds. His note that Peng's stacking reduces long-distance dependency without removing it has no evidence row.

**Open questions.**

- Which macro-F1 (0.792, 0.816 or 0.835) is the leak-free figure for the production SVM on n=87, and which run produced it?
- Is the stacked meta-reader (0.837 on the 760-row frame, 0.765 adversarial per the build_media_blend comment) evaluated out-of-fold?

**Read first.** [[burnham-2023-stance-detection-practical-guide]], [[birkenmaier-2025-measuring-politicians-public]], [[b-2025-comparing-fine-tuned]], [[lemens-2025-positioning-political-texts]], [[dai-2025-large-language-model]]

## Lexicoder LSD2015 tone intensity

**What it computes.** Per-release tone intensity = |net| / words from LSD2015 positive, negative and negated counts.

**Method.** stringi regex implementation that matches quanteda data_dictionary_LSD2015 counts; negation phrases are stripped first; 9.6 ms per document.

**Code refs.**
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/parse_lsd.R`
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/stage5_lsd.R`

**Assumptions.**
- Dictionary counts measure how heated a release is (intensity, not direction)

**Evidence.**

12 rows (2 VERIFIED, 8 VERIFIED-SOFTENED, 2 UNVERIFIED); 12 distinct papers.

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
| [[aroyehun-2026-politics-of-feelings]] | 2026 | supports | In LES models (Table 9): valence (positive minus negative emotion) +0.020 (CI 0.011 to 0.028) and intensity -0.014 (CI -0.025 to -0.004, p < 0.01). The paper reads the rise in emotion since 1973 as intensification with both positive and negative emotions rising, not as a negative turn. In the paper, intensity is the sum of the eight z-scored emotion scores. | Supports keeping direction and intensity as separate fields rather than collapsing them. Weak transfer: floor speech vs press releases, dictionary vs transformer, and LES outcome vs CU stance or behavior. It does not show whether sign times intensity predicts behavior. Do not treat lsd_charge as the same construct as the paper's intensity; they are analogous only. | weak | VERIFIED-SOFTENED |
| [[osnabrugge-2021-playing-gallery-emotive]] | 2021 | method-source | Emotiveness is measured valence-agnostically: percent emotive words (ANEW, 851 emotive seeds) minus percent neutral words (970 neutral seeds). Skip-gram embeddings expand the dictionary to 2,015 emotive and 2,095 neutral UK words, following Rice and Zorn (2019). Scale runs from -100 to +100; mean 0.88, SD 8.95. | Applies if 'how heated' should mean emotional charge regardless of direction. The paper validates on parliamentary speech, not press releases. Switching intensity changes press_value and the Press pillar, a change-control decision for Paul, not a recommendation here. | moderate | VERIFIED |
| [[rheault-2016-measuring-emotion-parliamentary]] | 2016 | qualifies | The long-run trend reproduces with NRC, OpinionFinder and SentiWordNet (S1 Fig, text description only; figure not extracted). Polarity is a signed weighted sum over all lemmas. Film-review validation reaches up to 80 percent accuracy; no parliamentary validation is reported in the extracted text. Recession -0.198 (SE 0.062) and election +0.187 (SE 0.069) shift the annual polarity change, pooled over government and opposition, N=104 years, Table 2 Model 1 (UNSTRATIFIED). The yearly polarity series has a unit root. | Rheault measures direction; Paul uses tone only as an intensity multiplier. Under net_norm a release dense in both positive and negative words scores near zero heat, so the paper gives no support for reading abs(polarity) as intensity. The lsd_charge option is closer to an intensity construct. Period effects matter only if members' release windows differ in date coverage. Westminster debate text is not US press-release text. | moderate | VERIFIED |

**Assessment.**

**Evidence weight: no validation in hand. Twelve rows, none showing that LSD2015 intensity measures how heated a congressional press release is.**

**The strongest rows bear on construct and method.**
- Haselmayer 2017 (measurement-source, SOFTENED) supplies a validation protocol: about 200 units rated 0-4 by lay and expert coders, then correlated with the dictionary score. Its finding is that general-purpose dictionaries underperform and a customized dictionary is required.
- Rice & Zorn 2018 (SOFTENED) gives the mechanism: domain nouns read as tone. It is visible directly in LSD2015, where 'credit' and its family are coded positive and debt*, charge*, risk*, oversight* and fraud* are coded negative. These are core vocabulary in CU press releases. The local masking figures (rho 0.953, 4.1% sign flips) were not reproduced (UNVERIFIED).
- Rheault 2016 (VERIFIED) and Osnabrugge 2021 (VERIFIED, method-source) bear on construct. The live default, abs(net/words), measures net direction, not intensity: a release dense in both positive and negative words scores near zero heat. lsd_charge ((pos+neg)/words) or a valence-agnostic emotiveness measure is closer to the stated construct.

**Weaker rows.**
- Ornstein 2024 and Pennebaker 2003: dictionaries are context-blind and sparse, so a few hits in a short release set the multiplier.
- Aroyehun 2026 weakly supports keeping direction and intensity separate.
- Dietrich 2019 argues text tone is strategic and crafted. That is theory only, and the 'staff-drafted' premise has no support.
- Merkley is a detection-dictionary precedent and says nothing about tone.
- Zhang 2022 and Taherdoost 2023 (both UNVERIFIED) are lead-only.

**Contradictions.**

- The component says the intensity is 'not direction', yet the live default (CU_S5_INTENSITY = net_norm) is abs(pos - neg)/words, a magnitude of net direction. Rheault and Osnabrugge rows both flag that this cancels mixed releases.
- The Merkley row records that an earlier bearing wrongly named charge per word as the press_value intensity. The default is net_norm.
- debate-stance-model.md calls LSD 'validated' on a three-case smoke test. By Haselmayer's standard that is a functional check, not a validation.

**Gaps.**

- No human-coded intensity gold. Fill with Haselmayer-style coding: about 200 own-domain releases rated 0-4 for heat by at least two coders, then correlate with abs(net_norm), with charge, and with a CU-masked LSD.
- No test of whether sign x intensity predicts behaviour better than sign alone. Fill with a pre-registered single look against later CU-relevant votes or cosponsorships.
- The effect of domain-noun masking (credit, charge, risk) has not been reproduced. Re-run in R and report rank changes in press_value.

**Paul's position vs the evidence.**

Paul has no annotation on this component. His highlighted source, Merkley & Stecula, uses Lexicoder with custom dictionaries to detect party cues. That supports customizing the dictionary, which the system has not done, and gives no support for stock-LSD tone as intensity. The evidence argues against uncustomized LSD on this vocabulary and against net_norm as the intensity construct. Switching to charge or masking is a change-control decision for Paul, not a recommendation made here.

**Open questions.**

- Should 'heat' mean emotional charge regardless of direction (charge / emotiveness) or net tone magnitude (net_norm)? This is Paul's construct call.
- How much does masking the CU-domain nouns credit, charge and risk move member press_value ranks?

**Read first.** [[haselmayer-2017-sentiment-analysis-political]], [[rice-2018-corpus-based-dictionaries]], [[rheault-2016-measuring-emotion-parliamentary]], [[osnabrugge-2021-playing-gallery-emotive]], [[ornstein-2024-train-your-stochastic]]

## press_value aggregate and human-label overlay

**What it computes.** press_value = stance_sign x LSD intensity per member x category. Human labels overwrite model stance where valid; 497 of 552 labels overlap the corpus.

**Method.** run_stage5.R block 3c is placed after the bill override and the Kennedy fix. Overlaid rows are excluded from model-accuracy measurement.

**Code refs.**
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R`
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/make_standalone.py`

**Assumptions.**
- Human labels are authoritative
- Overlay rows cannot be used to measure DEBATE accuracy

**Evidence.**

3 rows (3 VERIFIED-SOFTENED); 3 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[ornstein-2024-train-your-stochastic]] | 2024 | suggests-extension | Continuous measures built from the label probability vector: per-sentence P(Conservative) minus P(Liberal), averaged per manifesto, gives rho 0.92 (economic) and 0.80 (social) across 18 UK manifestos with GPT-3. Tweet sentiment uses the first principal component of the positive/negative/neutral probability vector. | The paper evidence uses GPT-3 next-token probabilities on a few-shot prompt, not DEBATE NLI entailment scores. DEBATE runs with multi_label=True (stage5_debate.py:65), so its three scores are independent and do not sum to 1. A p_support minus p_oppose contrast is a different object and would need a pre-registered single look on the 760-row press gold. Remains UNVERIFIED until a second path reproduces it (DE-062). | moderate | VERIFIED-SOFTENED |
| [[rice-2018-corpus-based-dictionaries]] | 2018 | qualifies | Changing the dictionary reorders units. Justice-level rank correlations are 0.29 (corpus measure vs LIWC) and 0.16 (corpus measure vs AFINN), versus 0.64 between LIWC and AFINN. Standard deviation of justice means is 0.25 (corpus), 0.22 (LIWC) and 0.16 (AFINN). Justices with more than 50 majority opinions; comparison across 84 justices. | The divergence evidence comes from a 217-year court corpus with large semantic drift. Press releases from 2022 on are a narrow window, so the divergence is probably smaller there; that is the analyst's inference and is untested. The live blend press weight of 0.10 was not checked in this pass. | weak | VERIFIED-SOFTENED |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | qualifies | Stance and sentiment are distinct dimensions to be measured independently. Sentiment may inform intensity but cannot stand in for stance. Integrating sentiment to scale stance intensity is named as a future direction needing more research. | This is a conceptual endorsement of separating the two dimensions only. The paper does not test whether multiplying sign by intensity improves measurement; it calls that open. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: weak. The aggregate formula is an untested design choice.**

**Sign x intensity.** Burnham 2023 (SOFTENED) endorses measuring stance and sentiment separately. It names 'sentiment scaling stance intensity' as open research. The multiplication press_value = sign x LSD intensity therefore has conceptual cover for keeping the dimensions separate, and no evidence for the product.

**Alternative.** Ornstein 2024 (SOFTENED) suggests building a continuous measure from label probabilities. DEBATE runs with multi_label=True, so its three scores are independent and do not sum to 1. A p_support - p_oppose contrast would be a different object that needs its own single look on the 760-row gold. It stays UNVERIFIED.

**Dictionary choice.** Rice & Zorn 2018 (SOFTENED, weak) shows that changing the dictionary reorders units (rank correlations 0.16-0.29 across dictionaries, 84 justices with more than 50 opinions each). The system uses one instrument and compares no alternative. The drift in that study comes from a 217-year court corpus, so the reordering in a 2022+ press window is probably smaller. That is the analyst's inference and is untested.

**Overlay share.** No row addresses the human-label overlay. The share of member-level press score driven by the 497 overlaid rows is unmeasured.

**Contradictions.**

None recorded.

**Gaps.**

- Overlay share: compute, per member x category, the fraction of press_value mass coming from human-overlaid rows (497 of 552 labels overlap the corpus), by party and chamber.
- Instrument sensitivity: rank correlation of member press_value under LSD net_norm vs charge vs a masked LSD. This is a descriptive diagnostic, not a model change.
- No test of sign x intensity against sign alone (see LSD tone).

**Paul's position vs the evidence.**

Paul treats sentiment-scored press as an augmentation to lobbying-pathway models, citing Bacik 2026. Bacik has no evidence row here, so his framing is neither supported nor contradicted. Burnham 2023 agrees with the separation of stance and sentiment but calls their multiplication unvalidated. His notes do not discuss the overlay, and no evidence addresses it.

**Open questions.**

- Does excluding overlaid rows materially change member press ranks? If yes, the press pillar partly reflects Paul's hand labels rather than the reader's output.
- Is the live press weight in the blend 0.10, as one row noted? It was not checked.

**Read first.** [[burnham-2023-stance-detection-practical-guide]], [[ornstein-2024-train-your-stochastic]], [[rice-2018-corpus-based-dictionaries]]

## Two-hop stance x valence decomposition with curated registry

**What it computes.** cu_alignment = stance(speaker->target) x valence(target->credit unions).

**Method.** Measured on 400 tweet golds: stance hop 0.915, valence hop 0.689 (the bottleneck). Composing model stance with oracle valence gives 0.872. Valence registry cu_valence_registry.json v2 (11 derived plus 14 seed entries) is derived from Paul's gold judgments; with 5-fold CV the composition reaches 0.782 accuracy / 0.754 F1 vs a flat baseline of 0.677. Auto-registry variants all fell below the flat baseline.

**Code refs.**
- `fec/apply_two_hop_stance.py (--eval, --uncovered)`
- `derive_valence_registry.py`
- `cu_valence_registry.json`
- `fec/stance/build_media_blend.py`

**Assumptions.**
- Each target has a stable CU valence the owner can judge
- Per-target valence is not keyword-automatable (238 distinct targets in 400 tweets)

**Evidence.**

6 rows (6 VERIFIED-SOFTENED); 6 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[dai-2025-large-language-model]] | 2025 | method-source | FOL rules elicited from an LLM (Table 5 bike-lane rule links congestion to Against) are made differentiable with soft-logic operators: negation 1-U, conjunction U1*U2, implication 1-U1+U1*U2 (Eqs 2-5), and trained end to end with cross-entropy (Eq 12). Zero-shot VAST all-class F_avg: 81.0 vs 76.8 for KASD with 100 percent training data (+4.2); 80.2 vs 74.0 for MPPT with 10 percent (+6.2). Error analysis of 50 VAST errors (Sec 4.8) names LLM over-inference that links a positive clause to the target, and clause-level polarity conflicts. | Transfer is limited to the decomposition idea. FOLAR targets are benchmark targets, whereas the valence hop needs CU-specific, owner-judged knowledge that LLM priors do not supply. The paper's over-inference failure matches the automatic-registry failure in the system. LLM-elicited valence rules stay UNVERIFIED (DE-062) until scored against the 400 tweet golds under single-look rules. The paper gives no evidence that a soft operator beats an exact registry lookup where the registry has coverage. | moderate | VERIFIED-SOFTENED |
| [[tang-2015-negative-link-prediction]] | 2015 | supports | Signed triads are overwhelmingly balanced: 92.31 percent (Epinions) and 93.01 percent (Slashdot) of undirected triads (Sec. 3.2 p. 89). This is a structural prior that sign products usually hold in person-to-person trust networks. The paper does not test stance-times-valence composition. | Paper triads are person-to-person links. The system's third node is an interest reached through a curated valence registry. The paper supplies only a structural prior and does not validate the registry. The registry numbers are on the golds the design iterated against, so they are not an out-of-sample validation. | weak | VERIFIED-SOFTENED |
| [[park-2025-automatic-construction-direction]] | 2025 | method-source | Polarity attaches to a (direction, target) pair. Direction-dependent nouns are typed proportional or inversely proportional by PMI against direction-tagged sentences (Eq. 1, delta = 0). Of 719 tagged sentences, 691 are proportional and 28 inversely proportional, and the imbalance yields noisy words such as beer and day. | Paul's registry is a gold-derived analogue of the paper's direction-pair lexicon; the automatic PMI typing is the part that did not transfer. Transfer is limited by scale: 400 tweet golds and 238 distinct targets, against the paper's 4,835 sentences with a 1 percent noun-frequency floor. A PMI typing pass on this corpus needs a held-out look before any claim (DE-062). | moderate | VERIFIED-SOFTENED |
| [[burnham-2023-stance-detection-practical-guide]] | 2023 | context | Stance is entailment relative to context C. When the target is absent, ambiguity rises (34.0 versus 19.5 percent) and accuracy depends on outside knowledge. The paper flags outside-knowledge inference as the hard step. | The paper never decomposes stance into a stance hop and a valence hop. The decomposition is the system's own design; the paper only explains why outside-knowledge steps are hard. | moderate | VERIFIED-SOFTENED |
| [[burnham-2024-political-debate-zero]] | 2024 | supports | The paper says NLI classifiers work best zero-shot on commonly understood concepts such as support for a person or policy. They are weaker on bespoke concepts needing specialized domain knowledge, where few-shot or supervised training is advised. This is qualitative guidance; the paper does not test a decomposition. | The concept note does not name DEBATE as the 0.915 stance-hop model; it refers to the stance stack. The paper supports the pattern, not the mechanism. Valence is a bespoke concept, which is the paper's stated condition for weakness. | moderate | VERIFIED-SOFTENED |
| [[lemens-2025-positioning-political-texts]] | 2025 | qualifies | Sec 3.4 and Fig 5 (36 coal-subsidy speeches, 10 languages, six-language crowd mean): GPT-4o overall r=0.93, with Against 0.91 and For 0.80. Against-group r is 0.07 for GPT-4 Turbo, 0.04 for MiXtral 8X22B and 0.05 for Llama 3 70B, but GPT-4o is strong within Against. Tweets within party for GPT-4o: 0.68 Democrats and 0.66 Republicans (Fig 1). | The paper tests one policy dimension (coal subsidy, 36 speeches) against crowd benchmarks. The weak within-Against cases belong to models other than GPT-4o, which is strong in both groups. The paper does not discuss within-camp weakness, so any link to the valence hop is the analyst's inference and suggestive only. | weak | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: moderate support for the pattern, none for the specific numbers.**

**Pattern support.**
- Burnham 2024 (SOFTENED): NLI is weak on bespoke concepts needing domain knowledge.
- Burnham 2023 (SOFTENED): the outside-knowledge inference is the hard step.

Together these explain why the valence hop (0.689) is the bottleneck and the stance hop (0.915) is not. Neither paper decomposes stance into two hops; the decomposition is the system's own design.

**Analogues.**
- Park 2025 (method-source, SOFTENED): polarity attaches to a (direction, target) pair. Paul's gold-derived registry is the analogue. Automatic PMI typing is the part that failed to transfer, with five auto-registry variants scoring 0.464-0.604 against a flat 0.677.
- Dai 2025 (method-source): offers soft-logic FOL composition. Its documented failure mode, LLM over-inference, matches the auto-registry failure, and it gives no evidence that soft operators beat an exact registry lookup where the registry has coverage.
- Tang 2015 (weak): only a structural prior (about 92-93% balanced triads in person-to-person trust networks).
- Lemens 2025: within-camp weakness. Its link to the valence hop is the analyst's inference.

**Contradictions.**

- Is the 0.782 / F1 0.754 composition score out-of-sample? Park 2025 and Burnham 2023 rows record 'honest 5-fold CV on 400 tweet golds'. The Tang 2015 row says it was measured on the golds the design iterated against, not 5-fold CV, and that the 5-fold claim applies to the stacked model. Until resolved, 0.782 should not be quoted as held-out.
- Registry size and threshold differ by source. The component says 11 derived + 14 seed. The Burnham 2024 row says the 14-entry curated path is an unbuilt option. The code keeps families at 0.75 gold consistency while the file header says 80%.
- The concept note does not name the model behind the 0.915 stance hop; it refers to 'the stance stack', not DEBATE.

**Gaps.**

- No held-out test of the registry on targets outside the 400 golds. Fill with a fresh gold batch labelled after the registry was frozen, scored once.
- No measure of valence stability across Congresses. Fill by re-judging a sample of registry targets on 117th vs 119th Congress text.
- No test of whether coverage, rather than stance accuracy, explains residual error. Split errors into registry-miss vs registry-hit-but-wrong.

**Paul's position vs the evidence.**

Paul asked how agreement between two actors is quantified when both hold negative sentiment toward a third party. The registry is the system's answer, and the evidence supports its premise that target valence is bespoke knowledge an owner must supply. His cited Fink 2023 and Birkenmaier rows do not appear as evidence for this component. The evidence agrees with his implicit stance against automatic valence derivation.

**Open questions.**

- Was 0.782 computed out-of-fold or in-sample on the iterated golds?
- Which registry is live: derived-only, or derived plus seed?

**Read first.** [[burnham-2024-political-debate-zero]], [[park-2025-automatic-construction-direction]], [[dai-2025-large-language-model]], [[burnham-2023-stance-detection-practical-guide]], [[tang-2015-negative-link-prediction]]

## Bill-sign valence (direction table, prose matcher; override rejected)

**What it computes.** Stance sign from a tracked bill's known CU direction when a release names the bill.

**Method.** cfpb_bill_directions.csv (Reduce +1 / Expand -1) plus the BILLS table in cu_press_release_scraper.R. The named-bill matcher had 93% precision on 187 releases but went 0 good / 3 bad as a blanket override. The DEBATE bill-sign override (12-member anti-CU prior list) dropped held-out accuracy from 0.839 / kappa 0.742 to 0.770 / 0.630 and was disabled 2026-06-30. The naive directional-verb lexicon (after Park, Lee & Cho 2025) gave 3 good / 43 bad sign changes and was rejected. The narrow Kennedy per-release fix is kept.

**Code refs.**
- `cu_press_release_scraper.R (BILLS ~1642; score_bill_signals ~2600)`
- `cfpb_bill_directions.csv`
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/run_stage5.R (CU_S5_BILL_OVERRIDE)`

**Assumptions.**
- REFUTED: DEBATE neutral means a missed bill stance
- REFUTED: directional verb counts over whole text determine valence
- Member priors (anti-bank consumer advocates) need a member-level prior, not a lexicon

**Evidence.**

2 rows (2 VERIFIED-SOFTENED); 2 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[kim-2025-measuring-interest-group]] | 2025 | suggests-extension | The released dataset gives measured group positions on named S./H.R. bills for the 111th-117th Congresses (2009-2022). It is on Dataverse at doi:10.7910/DVN/D0QWM2. | Useful as an external audit of the hand-set direction table, but only for 111th-117th Congress bills that CU trade associations named in LD-2 lines. 118th and 119th Congress bills would need the Haiku lane. Use it to adjudicate the table, never to overwrite it. | moderate | VERIFIED-SOFTENED |
| [[park-2025-automatic-construction-direction]] | 2025 | qualifies | On four FPB subsets (DS50 4,835 / DS66 4,209 / DS75 3,447 / DS100 2,259 sentences) LM+Senti-DD weighted F1 is 0.7057, 0.7322, 0.7788, 0.8233 vs LM alone 0.5914, 0.6023, 0.6174, 0.5982. On SemEval-2017 (960 messages) and FiQA (436 sentences) it is 0.4789 and 0.4937, below several general lexicons. In the 20-case DS100 appendix sample 10 are correct and 10 wrong; cited failures include (profit, increase) and (company, increase). | Evidence transfers only where a directional word governs the target noun inside one short sentence, as in FPB headlines. It does not transfer to whole-release counting over multi-actor congressional press text, where speaker attitude toward the direction is a second hop the paper does not model. The paper's own shift to SemEval and FiQA drops lexicon F1 below 0.5. This supports restricting any rule to the sentence naming the bill; any such variant stays UNVERIFIED until a held-out gold look (DE-062). | strong | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: thin (2 rows), but both are useful and agree with the system's rejections.**

**Park 2025 (SOFTENED, strong).** The direction-aware lexicon works where a directional word governs the target noun inside one short sentence: FPB headlines, weighted F1 0.71-0.82. On SemEval and FiQA its F1 falls below 0.5. That explains why the whole-release directional-verb override failed (3 good / 43 bad on gold). It supports the open question of restricting any rule to the sentence that names the bill. Such a variant is UNVERIFIED until a held-out gold look.

**Kim 2025 (SOFTENED).** Measured group positions on named bills in the 111th-117th Congresses (doi:10.7910/DVN/D0QWM2). This can audit the 60-row hand direction table for bills CU trade associations named, and should be used to adjudicate it, never to overwrite it. 118th-119th Congress bills are not covered.

The evidence agrees with the system's decision to disable the bill-sign override, which dropped accuracy from 0.839 to 0.770 and kappa from 0.742 to 0.630 on held out (n=87).

**Contradictions.**

- The Park 2025 row notes that the rejected experiment cited from run_stage5.R lines 105-108 is the bill-sign override (0.839 to 0.770), while the brain note cites the directional-verb dry run (3/43). These are two different rejected experiments and should not be cited for each other.

**Gaps.**

- No test of an aboutness or prominence guard (the bill is the release's main topic) on a bill-sign override. Fill with a single look on the n=87 held-out gold restricted to bill-naming releases.
- No external audit of cfpb_bill_directions.csv yet. Fill by joining it to the LobbyView positions dataset for overlapping 111th-117th bills and reporting agreement and disagreement counts.

**Paul's position vs the evidence.**

Paul takes Kim's Support / Oppose / Engage bill positions as the model for per-bill signing, and critiques the method as reactive and blind to inter-industry conflict. The evidence supports using Kim as an audit source. It does not address his inter-industry critique; no row studies conflicting industry positions on the same bill.

**Open questions.**

- How many of the 60 direction-table bills fall in the 111th-117th Congresses and appear in the LobbyView positions data with CU trade-association positions?

**Read first.** [[park-2025-automatic-construction-direction]], [[kim-2025-measuring-interest-group]]

## CFPB axis isolation (registry families plus binding policy_force)

**What it computes.** CFPB stance split into restraint (+1), expansion (-1), institution (0) and general (0) families.

**Method.** policy_force is a binding post-meta override in both stacks, and the CATKW router was tightened. CFPB-negative tiers went D 123 -> 98, R 7 -> 0; no-evidence 174 -> 263.

**Code refs.**
- `fec/stance registry and CATKW router (exact file not named)`

**Assumptions.**
- CFPB restraint vs expansion is separable by registry families
- CFPB-negative tiers were confounded by party (123 D vs 7 R)

**Evidence.**

1 rows (1 VERIFIED-SOFTENED); 1 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[poole-1999-nominate-after-years]] | 1999 | suggests-extension | Party overlap is under 1.5 percent in both chambers for the past 5 Congresses (p.15). The paper states, citing McCarty, Poole and Rosenthal (1999) rather than presenting evidence, that party per se does not account for much roll-call voting over and above the spatial model (p.17). | With near-zero overlap, dim1 and party are nearly collinear; the informative test is within-party. A pooled coefficient carries the UNSTRATIFIED label. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: essentially a GAP.**

One row bears on this component: Poole 1999 (SOFTENED). Party overlap is under 1.5% in recent Congresses, so party and NOMINATE dim1 are nearly collinear. The informative test of whether CFPB-negative tiers are a party proxy is therefore **within-party**. Any pooled coefficient carries the UNSTRATIFIED label.

The component's own numbers show the problem the isolation was meant to fix: CFPB-negative tiers went from D 123 / R 7 to D 98 / R 0. A tier with zero Republicans after isolation cannot be tested against party by a pooled model.

dw_control.py exists but is not imported by any stance code.

**Contradictions.**

- The isolation was meant to remove a party confound. After it, CFPB-negative tiers are D 98 / R 0, more one-sided than before (D 123 / R 7).

**Gaps.**

- No literature on separating CFPB restraint from expansion stances. Fill with a within-Democrat study: do members in the CFPB-negative tier differ from other Democrats on recorded CFPB-relevant roll calls (e.g. CRA disapproval votes), controlling for dim1? Report per chamber.
- No validation of the four family assignments (restraint, expansion, institution, general) against human coding. Fill with a coded sample of policy_force-forced rows.

**Paul's position vs the evidence.**

Paul cites no literature here. The single evidence row neither supports nor contradicts the design. It sets the test condition: stratify by party, or label the result UNSTRATIFIED.

**Open questions.**

- Within Democrats only, does CFPB-negative tier membership track CFPB-relevant votes after controlling for dim1?
- Why did no-evidence rows rise from 174 to 263 after isolation? Is the binding policy_force override zeroing real signal?

**Read first.** [[poole-1999-nominate-after-years]]

## Media blend (press + social) with recency decay

**What it computes.** raw cell = (1-0.5f) press/p80 + 0.5f social/p80, with f = min(1, nz/15) shrinkage for thin categories.

**Method.** Per post: sign x clip(margin, .25, 1.5) x (0.5 if retweet) x 0.5^(age/HL) / n_categories. Half-life is 2.0 years for press and 0.5 years for social. The press-side recency_weight in the R scraper also uses a 2-year half-life with a 2022-01-01 floor. 20 categories via CATKW regex; CROSSPOST_DEDUP is off by default.

**Code refs.**
- `fec/stance/build_media_blend.py:29-47`
- `build_social_master.py`
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R (recency_weight, wmean)`

**Assumptions.**
- Stance salience decays exponentially at the stated half-lives
- Social posts carry stance comparable to press after p80 scaling
- Thin categories (under 15 members with signal) are unreliable

**Evidence.**

4 rows (2 VERIFIED, 1 VERIFIED-SOFTENED, 1 UNVERIFIED); 4 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[steinertthrelkeld-2018-twitter-data]] | 2018 | context | Every US Senator and 430 House members had official Twitter accounts (2016). Tweet author metadata reflects when the API request was made, not when the tweet was created. | The coverage figure supports the assumption that almost all members have a social channel, but it counts Twitter accounts in 2016. It does not count the 119th Congress, it does not cover the platform mix the system reads (X, Facebook, Truth Social, Bluesky), and it says nothing about how many posts each member makes on CU topics. The metadata-timing caveat applies to handle-to-member mapping done at collection time. It does not apply to decay, which uses the post date. The paper offers nothing on ghost-writing, on the half-life, or on whether social posts add predictive validity over press releases. | weak | UNVERIFIED |
| [[sotoudeh-2024-network-based-measure]] | 2024 | qualifies | The 6-month half-life is one of three settings for decaying cosponsorship-influence capital, not a stance-salience estimate. Discrimination barely changes between 6 and 24 months (0.248 vs 0.203). | Not transferable. The paper's decay applies to accrued passage-rate ties and its outcome is bill passage. Its flatness across half-lives warns against treating any one value as calibrated. The paper does not justify 0.5 years for social posts or 2 years for press. | moderate | VERIFIED |
| [[hargittai-2015-bigger-always-better]] | 2015 | qualifies | Non-users are excluded from a site-based sampling frame by design. If selection is related to outcomes, results from users may reach the opposite conclusion about the wider population. Sampling should not be tied to the substantive question. The paper does not discuss how missing social-media activity should be coded. | The paper gives no evidence about missing-data coding. The concern bites only if posters differ from non-posters on stance in categories where f = 1; that is untested, and the analyst's own note says so. The stance-ladder rule on missing evidence was not checked. UNVERIFIED. | weak | VERIFIED-SOFTENED |
| [[lemens-2025-positioning-political-texts]] | 2025 | suggests-extension | Sec 2.1: a document's position is the average of the sentence or tweet scores for which the LLM returned a numeric score, so NA units are dropped from the average. App E, Table A1 (GPT-4o row): tweets that received a score had 18.7 human ratings on average versus 11.6 for unscored tweets, which the paper reads as NA marking low political content. | The paper validates averaging for left-right placement, not for CU advocacy intensity. Averaging measures position while the blend sums volume. Whether volume should count is Paul's policy call, not a recommendation to normalise. Any change needs Paul's ruling and zero-rank-delta or look discipline. | moderate | VERIFIED |

**Assessment.**

**Evidence weight: weak. The half-lives and the 0.5 social weight have no empirical grounding in the evidence.**

**Half-lives.** Sotoudeh 2024 (VERIFIED) is Paul's cited source for a 6-month half-life. In that paper the half-life decays cosponsorship influence capital for predicting bill passage, not stance salience. Its discrimination barely changes between 6 and 24 months (0.248 vs 0.203), which warns against treating any single value as calibrated.

**Neutral vs failed.** Lemens 2025 (VERIFIED) averages over scored units and drops NA. The blend instead sums sign x intensity x decay per post. Neutral posts contribute zero, and API-failure neutrals (confidence 0.0) are indistinguishable from real neutrals. Whether volume should count is Paul's policy call; this is noted, not recommended.

**Silence coded as zero.** A member with no social value gets sv = 0, so at f = 1 their press side is halved. Hargittai 2015 (SOFTENED) raises selection concerns but gives no evidence on missing-data coding. Steinert-Threlkeld 2018 (UNVERIFIED) supports near-universal account coverage, but only for Twitter in 2016.

**Contradictions.**

- Paul's position cites Sotoudeh's 6-month half-life as a decay timescale for shifting relationships. The VERIFIED row says that half-life is for a different construct (cosponsorship capital to passage) and is not identified: the paper's results are flat between 6 and 24 months.
- Under f = 1, silence on social media halves a member's press contribution rather than being treated as missing (build_media_blend.py lines 29, 463, 479).

**Gaps.**

- No study of which half-life best predicts later votes from past statements. Fill with a pre-registered look: fit stance-to-vote agreement on CU-relevant roll calls at half-lives of 0.25-4 years, separately for press and social, stratified by chamber.
- No test of whether social posts add predictive validity over press. Fill with a nested comparison (press-only vs press+social) on the same outcome.
- No calibration of the nz/15 shrinkage threshold.

**Paul's position vs the evidence.**

Paul calls for temporal weighting and draws timescales from Sotoudeh (6 months) and Kim, Stuckatz & Wolters (effects fading in 2-3 years). The system's 2.0-year press and 0.5-year social half-lives were not drawn from his notes. Sotoudeh does not identify a half-life for stance, and Kim/Stuckatz/Wolters has no evidence row here. His general call for decay is reasonable; the specific values are unsupported either way.

**Open questions.**

- Should a member with no social posts in a category be treated as missing rather than zero?
- Should ERR neutrals from claude_stance.py be excluded from the blend instead of counted as neutral?

**Read first.** [[sotoudeh-2024-network-based-measure]], [[lemens-2025-positioning-political-texts]], [[hargittai-2015-bigger-always-better]], [[steinertthrelkeld-2018-twitter-data]]

## Legacy CU press regex stance and graded tone scorer

**What it computes.** Per release: CU stance via a regex ladder (default support), graded tone via TONE_LEXICON, and member lean, stance and negativity aggregates.

**Method.** Ladder: oppose > support cues > neutral bill cues > default support. GRADED_CUTS c(1,2,3.5,5.5,8). Polarity (Wp-Wn)/(Wp+Wn) per Rice & Zorn. Over-flagging recorded: 21 of 32 neutrals were wrongly scored. Superseded for stance on 2026-06-21 and still computed as a comparison column.

**Code refs.**
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/cu_press_release_scraper.R`
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/test_cu_scoring_offline.R`

**Assumptions.**
- CU-mention sentences carry the release's CU stance
- Default stance is support

**Evidence.**

3 rows (1 VERIFIED, 2 VERIFIED-SOFTENED); 3 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[rice-2018-corpus-based-dictionaries]] | 2018 | method-source | Polarity = (sum Wp - sum Wn)/(sum Wp + sum Wn), with Wp and Wn built from tf-idf and cosine-weighted counts over 500 positive and 500 negative words taken from 300-dimensional GloVe vectors (window 50, minimum count 90) trained on the domain corpus, seeded by 10 positive and 10 negative words. Gains over LIWC and AFINN already appear at 5,000 documents, rise steeply to about 15,000, and show marginal gains after 30,000. | The formula transfers as written. The paper's validity evidence applies only to corpus-derived, similarity-weighted dictionaries, which this code does not build. The paper's own accuracy gains start at 5,000 documents; the CU-relevant set is 1,563 text-bearing releases. Paper says document-level polarity hides subject (p.27), which supports the stance split. | strong | VERIFIED-SOFTENED |
| [[haselmayer-2017-sentiment-analysis-political]] | 2017 | contradicts | Sentence tonality is set by the single most negative dictionary word (max rule). A negator immediately before a word excludes that word, and an intensifier multiplies it by 1.25. Word scores are crowd-derived means on a 0-4 scale. Mean word scores with the max rule and booster/negation handling give r = 0.65 (n = 168 test sentences). Other aggregation combinations give r from 0.38 to 0.58. | This is a fidelity finding, not an empirical one. It matters for the display score_label magnitude and the comparison column only, because press_value and the press pillar do not consume the graded layer. No external claim that these graded labels follow Haselmayer and Jenny should be made. | strong | VERIFIED |
| [[rheault-2016-measuring-emotion-parliamentary]] | 2016 | context | Domain lexicon from 200 emotion seeds (100 positive, 100 negative lemmas) using GloVe vectors of 300 dimensions per lemma-PoS pair, symmetric window 15, 108,506 lemma types at frequency 10 or more. Each lemma score is its summed cosine to positive seeds minus summed cosine to negative seeds, scaled to [-1,1]. The top and bottom 2,000 are kept (4,200 words). Table 1 lists compound (verb) at -1.00. | Parallel method, not the source: the helper header cites Rice and Zorn 2018 for the seed-to-vocabulary logic. Its GloVe settings differ from Rheault's (rank 50, skip-gram window 5, minimum count 5, at least 50 documents). Its seeds mix stance words (support, champion, protect) with affect words. The press corpus is far smaller than Hansard, so vectors will be noisier. Rheault's own odd top entries show the risk. The analyst review step is the safeguard; the top and bottom cut has no in-domain validation. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: strong on fidelity, and it shows the scorer does not implement what it cites.**

- **Haselmayer 2017 (VERIFIED, contradicts).** The paper sets sentence tone by the strongest cue (max rule; r = 0.65 vs 0.38-0.58 for other aggregations, n = 168 sentences). The R code instead sums every fired cue into Wp and Wn. It also uses 44 hand-weighted entries (weights 1/2/3) where the paper uses crowd-derived scores, and adds a 0.75 downtoner that the paper does not have.
- **Rice & Zorn 2018 (method-source).** The polarity formula matches. The weights are not corpus-derived. expand_tone_lexicon() departs from the paper's settings everywhere and appears never to have run (lexicon_candidates.csv is absent). The paper's gains begin at about 5,000 documents; the CU corpus has 1,563 text-bearing releases.
- **Rheault 2016 (SOFTENED, context).** A parallel method. It adds a caution that the seeds mix stance words with affect words.

The impact is limited: press_value does not consume the graded layer, which feeds only the display score_label and a comparison column.

**Contradictions.**

- The file header claims sentence tonality follows the strongest cue, per Haselmayer & Jenny. The code sums all cues (weighted_tone, about lines 2443-2475).
- The 'superseded 2026-06-21, 21 of 32 neutrals wrongly scored' claim is unsupported in the files checked (Rice & Zorn row, UNVERIFIED).

**Gaps.**

- No party-stratified bias measure for the default-support ladder against human gold. Fill by scoring the n=87 or 760-row gold with the regex column and tabulating error by party and chamber.

**Paul's position vs the evidence.**

Paul has no annotated literature on this component; Rice & Zorn is on disk but unannotated. The evidence says no external claim should present these graded labels as following Haselmayer & Jenny or Rice & Zorn.

**Open questions.**

- Should the comparison column be dropped, or the header citation corrected, since the code does not implement the cited method?

**Read first.** [[haselmayer-2017-sentiment-analysis-political]], [[rice-2018-corpus-based-dictionaries]], [[rheault-2016-measuring-emotion-parliamentary]]

## ACU speaker mode (planned)

**What it computes.** Treats ACU-authored press (816 rows, 243 members) as ACU-to-member friend or foe labels.

**Method.** The NLI hypothesis is branched by speaker_source; keyword extraction was rejected. Validation gate: held-out accuracy >= 0.851, and Marshall/Hawley ACU rows must net negative. Planned 2026-07-01 and blocked on labels.

**Code refs.**
- `C:/Users/pgoff/OneDrive/Desktop/Draft Systems/ACU_SPEAKER_MODE_PLAN.md`

**Assumptions.**
- ACU endorsing a member means pro-CU; ACU criticising a member means anti-CU

**Evidence.**

1 rows (1 VERIFIED-SOFTENED); 1 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[burnham-2024-political-debate-zero]] | 2024 | qualifies | NLI zero-shot works best when labels can be derived from the text alone, without meta-knowledge about who wrote it (p.16). On quote tweets, which carry two opinions from two people, the hardest set: Claude 0.29, base 0.62, large 0.88 MCC. DeBERTa Large NLI had negative correlation with the correct labels on that set. | The meta-knowledge condition applies directly, since the paper names authorship. ACU texts about a member resemble two-voice quote tweets only by analogy; the paper gives no result for advocacy-group-about-member texts. Base-versus-large on such texts is untested. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: near GAP.**

The one row, Burnham 2024 (SOFTENED), cuts against an easy build. NLI zero-shot works best when labels follow from the text alone, without knowing who wrote it. On two-voice quote tweets the base model reached MCC 0.62 and Claude 0.29, and DeBERTa Large NLI correlated negatively with the true labels.

ACU-authored texts about a member are a similar authorship-dependent task, though only by analogy. Branching the hypothesis by speaker_source is the right direction, but it is untested. The 0.851 held-out gate and the requirement that Marshall/Hawley rows net negative are sensible pre-commitments, and the plan is blocked on labels.

**Contradictions.**

None recorded.

**Gaps.**

- No evidence that advocacy-group statements about a member predict that member's later CU votes. Fill with an ACU-friend/foe label set on the 816 rows (243 members), then a single look comparing ACU-about-member signal vs the member's own statements against later CU-relevant roll calls.
- No base vs large comparison on authorship-dependent texts.

**Paul's position vs the evidence.**

Paul highlights that endorsements from well-connected groups are strong cues and that source identity shapes receptiveness (Box-Steffensmeier 2018, Furnas 2022). Neither has an evidence row here, so the cue-strength premise is unexamined in this pass. Burnham 2024 supports treating source identity as a distinct modelling problem, consistent with a separate speaker mode. It also warns that NLI handles source-dependent labels poorly.

**Open questions.**

- How many of the 816 ACU rows name a member as subject, as opposed to merely tagging them?

**Read first.** [[burnham-2024-political-debate-zero]]

## Stance dynamics and neighbour imputation

**What it computes.** Quarterly (91-day) stance trajectories in seven classes, plus imputation proposals for missing stances from network neighbours.

**Method.** Static band = median + MAD over observed-to-observed transitions (floor .05). Imputation needs at least 5 neighbours and |estimate| >= 0.75; 307 proposals; masking validation gives 77% sign agreement. Proposals only; the matrix is not changed. Co-movement was null on the cosponsor and HCoI layers.

**Code refs.**
- `fec/stance_dynamics.py`

**Assumptions.**
- Stance is locally similar among network neighbours (co-movement test was null)

**Evidence.**

13 rows (2 VERIFIED, 8 VERIFIED-SOFTENED, 3 UNVERIFIED); 9 distinct papers.

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
| [[ringe-2013-pinpointing-powerful-voting]] | 2013 | qualifies | The connectivity to agreement link depends on whether the pair are expected allies or adversaries (bloc). Same-bloc dyads co-vote in 574 of 594 cases (about 97 percent), so H2 is untestable. For opposed-bloc pairs more contact predicts less co-voting (p. 625; Table 3). | The paper outcome is votes, not stance labels, so transfer is conceptual. The next diagnostic is a party-only baseline and a split of same-party from cross-party neighbours. The 77 percent figure stays UNVERIFIED as a network effect until that baseline exists (DE-062). | moderate | VERIFIED |
| [[selfhout-2010-emerging-late-adolescent]] | 2010 | method-source | Stochastic actor-oriented models estimate tie selection conditional on the current network state, with ego, alter and similarity terms for each covariate, and pool groups by multilevel meta-analysis. The cited extensions (Steglich et al. 2006; Burk et al. 2007) model networks and behaviour together, which separates selection from influence. | Transfer is moderate in method only. The paper itself models selection and not influence, so it gives no evidence on stance contagion. A joint network-and-behaviour SIENA model is the standard way to ask whether ties drive stance change or stance similarity drives ties. That would need panel waves of cosponsor ties and quarterly stances. It would be new analysis, UNVERIFIED until reproduced, and its pooled estimates must be reported by stratum (party, chamber) or marked UNSTRATIFIED. | weak | UNVERIFIED |

**Assessment.**

**Evidence weight: strong on provenance and on critique; nothing supports the imputation as a network effect.**

**Provenance.** Zhou 2026 (method-source, SOFTENED) is the transplant source, and the vocabulary transfers: 7 dynamics, a static band of median + MAD, flip curves.

**Where the transplant departs from Zhou.**
- The band was designed for continuous, densely observed stance. Applied to pooled quarterly signs, it mainly measures label coarseness: the live static band is 0.7076, and 91.6% of 2,989 transitions are static.
- Zhou carries learned embeddings forward between snapshots. The code freezes the last categorical label instead, and carried windows enter transition counts. That inflates static counts and credits flips to the window where posting resumes.
- The 91-day window has no sensitivity run. Zhou chose its 28-day window by sensitivity analysis.
- Zhou validates by retraining with own edges masked and scoring macro-F1 over 5 folds. The system's masking check is raw sign agreement on a confidence-selected subset (77.5%, n = 1,087) with no party-only or majority baseline. It stays UNVERIFIED as a network signal.

**Co-movement.** Zhou's co-movement result is a descriptive correlation with no null model. The system's stored cosponsor co-movement is 52.3% (134 same vs 122 opposite), below the 0.55 threshold, so null. An HCoI re-run gives 50.5% (302 vs 296). The 49% HCoI figure cited elsewhere is not stored. These nulls are descriptive co-movement results only, and no influence reading attaches to them.

**Party conditioning.** Ringe 2013 (VERIFIED) shows that the link between connectivity and agreement depends on bloc. The imputation does not condition on party, so a party-only baseline is the required next diagnostic.

**Leads only.** Zareie 2020 and Baptista 2022 (second-order similarity, RWR) are weak. Zhan 2025, Cao 2025 and Selfhout 2010 are UNVERIFIED leads. Cao assumes DeGroot neighbour influence, which the system's null contradicts. Selfhout points to SIENA joint selection/influence models as the standard method. Ma 2022 suggests a robustness test: corrupt the labels of a few high-coverage members and count how many proposals flip.

**Contradictions.**

- The HCoI co-movement null is cited as 49%, but no labelled HCoI figure is stored. stance_dynamics.json holds an unlabelled 134/122 (52%), and the scratch re-run gives 50.5%.
- The final print says 'at least 3 informative neighbors' while the threshold SD_MINNB is 5 (line 152).
- True Neutral (score 0) counts as a negative sign in the masking check (line 143), which biases sign agreement.
- The imputation assumes stance is locally similar among neighbours, yet the system's own co-movement test is null on both layers.

**Gaps.**

- No party-only or majority-class baseline for the 77.5% masking agreement. Fill by re-scoring the same 1,087 masked cells with the member's party majority sign, split into same-party and cross-party neighbours (per Ringe).
- No window sensitivity analysis (e.g. 28/56/91/182 days).
- No permutation null or common-shock control for co-movement.
- No joint network-and-behaviour (SIENA) model; stance contagion along cosponsor ties is unexamined. Any pooled estimate must be reported by party and chamber or marked UNSTRATIFIED.

**Paul's position vs the evidence.**

Paul wants temporal dynamics and change-likelihood modelling, citing Fink 2023, Bacik 2026 and Zhou & Elejalde 2026. Zhou supports the trajectory vocabulary. Paul's notes predate the null co-movement result, which undercuts any change-likelihood model that relies on neighbour propagation. Fink and Bacik have no evidence rows for this component.

**Open questions.**

- Does the 77.5% sign agreement exceed a party-only baseline on the same masked cells?
- Should carried-forward silent windows be excluded from transition counts?

**Read first.** [[zhou-2026-measuring-stance-dynamics]], [[ringe-2013-pinpointing-powerful-voting]], [[selfhout-2010-emerging-late-adolescent]], [[malaterre-2024-inferring-social-networks]], [[zareie-2020-similarity-based-link]]

## Frame alignment (cross-member duplication rate)

**What it computes.** Per-category share of near-duplicate text across members, plus party framing chi-square.

**Method.** 3-gram shingle Jaccard >= 0.5. Reported duplication: CU Tax 65%, General CU 60%, Defense 12%. It feeds salience intensity (0.5 + alignment).

**Code refs.**
- `fec/frame_alignment.py`

**Assumptions.**
- High shingle overlap indicates coordinated messaging

**Evidence.**

3 rows (1 VERIFIED, 2 VERIFIED-SOFTENED); 1 distinct papers.

| Paper | Year | Relation | Paper shows | Transfers when | Strength | Check |
|---|---|---|---|---|---|---|
| [[joo-2022-image-data-automated]] | 2022 | method-source | Framing = share of duplicate images within each event-label; two images are duplicates when the normalized distance between their 1,000-dim last fully connected layer vectors is below .2. Table 3: Venezuela state violence .308 vs Hong Kong contains-child .003 (paper: almost 103 times). The paper frames this as measuring which frames protesters choose to share. | Carries over only when the duplicated unit was chosen by the actor being scored, as Joo's sharers chose their images; here that means own-domain releases. Third-party ACU syndication measures the author's reach, not member coordination. Joo's frame labels come from a trained classifier; here they are press categories, and short titles under 3-gram Jaccard >= 0.5 are not comparable to a CNN-feature distance threshold. | strong | VERIFIED-SOFTENED |
| [[joo-2022-image-data-automated]] | 2022 | qualifies | The .2 duplicate threshold was chosen from inspecting the distance histogram, with no sensitivity analysis reported. The authors call the results provisional and suggest other operationalizations such as number of tweets containing a label instead of percentage, and expanding the frames considered. | Applies directly. Before any alignment figure is quoted, sweep the threshold (e.g. 0.3-0.7) on own-domain rows. The paper supplies no threshold validation the system could borrow. | moderate | VERIFIED |
| [[joo-2022-image-data-automated]] | 2022 | qualifies | Chi-squared tests of label against party are run separately on male and female images because the two parties have different gender ratios. Labels are sorted by chi-square with per-label significance stars; no multiple-comparison correction is described across the 1,730 unique labels. | Joo's design stratifies on a known compositional fault line. Here the fault lines are chamber and author domain. Until stratified, quote the chi-square figures as UNSTRATIFIED descriptive term lists, not party-framing findings. | moderate | VERIFIED-SOFTENED |

**Assessment.**

**Evidence weight: strong, and it overturns the reported figures.**

**Syndication inflates the headline rates.** Joo 2022 (method-source, SOFTENED) defines framing as duplication chosen by the actor being scored. The system computes duplication over the whole cu_press_stance_tone.csv: 2,388 rows (file dated 2026-06-30), of which 816 are americascreditunions.org articles. Those 816 carry only 349 distinct titles, with up to 19 members sharing one title. Recomputed on own-domain (*.senate.gov / *.house.gov) rows, reproduced on a second path:

| Category | Whole CSV | Own-domain only |
|---|---|---|
| General Credit Union | 60.2% of 442 | 5.2% of 116 |
| Credit Union Tax Status | 64.7% of 190 | 38.1% of 42 |
| Defense | 12.2% of 41 | 13.9% of 36 |

The headline 60% / 65% therefore mostly measures ACU's reach, not member coordination.

**Untested threshold.** Joo 2022 (VERIFIED) chose its .2 distance threshold by inspecting a histogram, with no sensitivity analysis, and calls its results provisional. The system's Jaccard >= 0.5 cut inherits the same untested choice.

**Party framing.** Joo stratifies its chi-square on a known compositional fault line. The system's party-framing chi-square is pooled over chamber and source domain (1,231 Senate / 1,157 House rows; 34% ACU-authored), with no correction. Figures such as 'D to CU-Bank Dynamics chi-square = 128' are UNSTRATIFIED descriptive term lists.

**Contradictions.**

- The reported General CU 60% and CU Tax 65% duplication rates fall to 5.2% and 38.1% on own-domain rows. Third-party ACU syndication drives the headline rates.
- These alignment rates feed salience intensity (0.5 + alignment), so ACU syndication currently inflates salience for CU categories.

**Gaps.**

- No threshold sweep. Fill by recomputing over Jaccard 0.3-0.7 on own-domain rows only.
- No test that duplicated messaging predicts collective cosponsorship of the same bill. Fill with a dyad-level look: same-title-cluster membership vs co-cosponsorship within a window, stratified by party and chamber.
- Party framing chi-square needs stratification by chamber and author domain, plus a multiple-comparison correction.

**Paul's position vs the evidence.**

Paul takes Joo's duplicate-rate framing as the template; that is a sound method source. The evidence shows the port violates Joo's core condition, that the actor being scored chooses the duplicated unit. His 'Monitoring tool?' margin note concerns Dietrich's aisle-crossing video, not this component.

**Open questions.**

- Should ACU-authored rows be excluded from frame alignment, and from the salience intensity it feeds? This is a change-control call for Paul.

**Read first.** [[joo-2022-image-data-automated]], [[malaterre-2024-inferring-social-networks]]

## Across components

**1. Denominators get conflated across the stack.** Several accuracy figures are quoted together that sit on different bases:

| Figure | What it is | Denominator |
|---|---|---|
| 87.2% | Agreement with the Claude labeller | 156 rows |
| 0.839 / kappa 0.742-0.760 | Accuracy on human gold | n=87 |
| about 85% | Member-grouped human gold | not stated |
| 0.796 / 0.571 | Gold frame / adversarial frame | 760 rows / adversarial goldeval frame |
| 0.782 | Two-hop composition; CV status disputed | 400 tweet golds |
| 0.757 | Transformer; not located in code or vault | n=87 |
| 0.816 vs 0.792 vs 0.835 | SVM macro-F1, reported three ways | n=87 |

Every quoted figure should carry its denominator and window. Figures measured against the LLM labeller should never be called accuracy.

**2. The neutral bin is overloaded.** Zero sign currently means four different things:
- not about CUs;
- hedged or coded support (van der Does lead; B 2025 error mode);
- a claude_stance API failure (ERR);
- an unrecognised label.

All four flow into the media blend as a zero contribution and into the stance-dynamics masking check, where True Neutral counts as a negative sign. Burnham 2024 notes that DEBATE never trained neutral as a positive concept.

**3. ACU syndication contaminates the press corpus.** 816 of 2,388 rows (34%) in cu_press_stance_tone.csv are americascreditunions.org articles carrying only 349 distinct titles. The frame-alignment numbers are already shown to be driven by this. The same rows also enter:
- press_value;
- stance dynamics windows;
- the party-framing chi-square.

ACU speaker mode is the planned remedy, and it is blocked on labels.

**4. Party is the unaddressed fault line.** Four components need party strata (and chamber where relevant), or the UNSTRATIFIED label, before any result is quoted:
- CFPB tiers: D 98 / R 0 after isolation.
- Frame chi-square: pooled.
- Imputation: no party-only baseline; Ringe shows the connectivity-agreement link depends on bloc.
- Poole: party and dim1 are collinear.

**5. Citations that the code does not implement.**
- The graded tone header cites Haselmayer & Jenny; the code sums cues instead of taking the strongest.
- The Rice & Zorn expander uses different settings and never ran.
- Zhou's persistence is learned embeddings; the code freezes a categorical label.
- Joo's actor-chosen duplication condition is not met.

These are fidelity issues, cheap to fix in documentation and distinct from any empirical claim.

**6. No validity evidence links measured stance to behaviour.** No component's evidence connects stance, tone intensity, half-life or ACU signal to later votes or cosponsorship. The whole stack is validated, where at all, against text labels.

**Wording guards observed.** Co-movement results are descriptive, and no influence language is used. Neighbour weights, if ever added, are heuristic tie strength, not closeness (D-025). Lemens' averaging point is noted as Paul's policy call, not a volume-normalization recommendation. None of the 71 rows argues for the two rejected SNA proposals.

## Research agenda

1. Reconcile the stance accuracy ledger. Resolve the CV status of 0.782, which SVM macro-F1 (0.792 / 0.816 / 0.835) is leak-free on n=87, and the provenance of the 0.757 figure. Then publish one table that gives the denominator, rater type (human vs Claude) and window for every figure.
2. Count the training rows labelled only by Claude (of 552) and re-score held-out accuracy on human-gold-only rows. Report MCC alongside accuracy.
3. LSD intensity validation in Haselmayer style: about 200 own-domain releases rated 0-4 for heat by two or more coders. Correlate the ratings with abs(net_norm), with charge, and with a CU-domain-masked LSD (credit / charge / risk). Report by chamber.
4. Recompute frame alignment on own-domain rows only, with a Jaccard sweep from 0.3 to 0.7. Re-run the party-framing chi-square stratified by chamber and author domain with a multiple-comparison correction. Quantify how ACU rows move salience intensity.
5. Add a party-only and a majority-class baseline to the stance-imputation masking check on the same 1,087 cells. Split results by same-party vs cross-party neighbours. Fix the treatment of True Neutral as a negative sign and the 3-vs-5 neighbours print mismatch.
6. Single-look comparison of fine-tuned DEBATE base vs large on the n=87 member-grouped human gold and the adversarial frame, run on Runpod with de-identified text.
7. Pre-register a half-life study: agreement between stance and CU-relevant votes at half-lives of 0.25-4 years, run separately for press and social and stratified by chamber, with a nested press-only vs press+social comparison.
8. Within-Democrat CFPB test: does CFPB-negative tier membership track CFPB-relevant roll calls after controlling for NOMINATE dim1? Report per chamber.
9. Audit cfpb_bill_directions.csv against the LobbyView positions dataset (doi:10.7910/DVN/D0QWM2) for overlapping 111th-117th Congress bills. Use the result to adjudicate the table, never to overwrite it.
10. Hold a two-hop registry check: a fresh tweet-gold batch labelled after the registry was frozen, scored once, with errors split into registry miss vs registry hit-but-wrong.
11. Split the neutral bin: separate ERR and unrecognised-label rows from model-chosen neutrals in claude_stance and build_media_blend, and count each.
12. Correct the documentation for fidelity: the graded-tone header claim (Haselmayer max rule), the status of the Rice & Zorn expander, and stance_dynamics persistence vs Zhou.
