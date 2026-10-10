---
type: theme
name: "Neural Networks"
paper_count: 1
tags: [theme, type/theme]
---

# Neural Networks

**Neural Networks: the theme holds one work and none of Paul's notes.**

Paul's annotation document for this category, `Neural Networks - Untitled document.md` (sha12 da706e80ef35), is four bare links. It has no takeaways, no commentary and no reviewer JSON. The archived copy from bib-20260705 is identical, so nothing was lost between versions. Only one of the four links became a full work record: [[zhou-2026-measuring-stance-dynamics]]. The other six slugs in the file come from that record's `related` list. They are links on the system side, not works annotated under this theme.

So this is not a survey of a literature. It is a reading of one paper, and of its role as the method source for `fec/stance/stance_dynamics.py`. [[zhou-2026-measuring-stance-dynamics]] builds TWLGCN, a temporal graph neural network. It infers user-level referendum stance (favour vs. against) from user-hashtag graphs rebuilt every 28 days, using Chile's 2020 and 2022 referendums (about 1.21M tweets; 12,237 and 4,934 users). Its second half is descriptive: trajectories of opinion movement, with no null baseline and no causal test.

The Melange port keeps the paper's design ideas and drops the GNN. Three things are not supported by the paper: carrying a member's last stance forward, the 91-day window, and neighbour-based imputation. All three are UNVERIFIED. The port's live output also shows three problems, set out in the systems section.

Any cross-work claim about neural-network methods would have to come from Paul. The suggested next step is to find his Neural Networks thinking (possibly in the Curated Annotations file or the Path-Finding / SNA bibliographies), or to annotate the remaining three links and re-run this synthesis.

## Strands

### Temporal graph neural networks for user-level stance trajectories

The only work here is [[zhou-2026-measuring-stance-dynamics]]. Its question is whether a temporal GNN can infer favour/against stance for every user in each 28-day period, including users who never post about the referendum. It also asks whether the resulting trajectories can describe opinion shifts at the population, individual and neighbourhood levels.

The design has four parts:
- a count-weighted bipartite user-hashtag graph for each snapshot;
- four LightGCN propagation layers with symmetric degree normalization, followed by attention over the layer outputs;
- focal loss for class imbalance, plus a supervised stance contrastive loss (SSCL);
- a warm start, in which each period inherits the previous period's embeddings and classifier weights.

Supervision is very thin: 80 of 171,615 hashtags (about 0.05%, Entry_DS) and 106 of 57,211 (about 0.19%, Exit_DS) are labelled by side. Each user's stance probability comes from the counts of labelled hashtags they used.

The paper's own findings, with the windows and denominators it reports:
- **Carry-forward helps.** On Exit_DS, the warm-started model beats a variant that re-initializes each period by 1.3% to 9.8% macro-F1.
- **Interaction edges help.** Active user-user edges (mention, quote, reply) add about 3% macro-F1 on both datasets. Passive follower/friend edges add little, and combining the two does not help.
- **SSCL helps.** It adds about 2% macro-F1 over focal loss alone.
- **Hashtag labels are a usable proxy.** Human coders agree with hashtag-derived labels at kappa 0.81 to 0.94, on 100 user snapshots per dataset. Human-human kappa is 0.836 and 0.842.
- **One claim cannot be checked.** The abstract says the model beats the baselines by more than 5% macro-F1 in every period, but the main text gives no baseline numbers; they are only in Fig. 2 and supplement Table S4.

The trajectory half of the paper is descriptive only. It defines seven classes of movement, with a "static" band set at the median + MAD of the change in a continuous probability. It shows mirrored flip curves and neighbours whose stance tends to move the same way (Fig. 6). There is no null model and no causal identification, so neighbour co-movement in this paper is association, not evidence that one user moves another.

Paul has written nothing about this strand. Everything above comes from the work record.

- [[zhou-2026-measuring-stance-dynamics]]

## Agreements and debates

**Agreements and disagreements: none can be stated across works.** There is one work and no notes from Paul, so this section can only record tensions inside the paper and between the paper and its port.

**Inside [[zhou-2026-measuring-stance-dynamics]]:**
- **Baseline gap.** The abstract claims more than 5% macro-F1 over baselines in every period. The text supports only "steady 70%-90%" for TWLGCN and says the baselines "fluctuate", with no numbers. Treat the >5% figure as unchecked.
- **Hashtags as stance.** The kappa of 0.81 to 0.94 rests on 100 snapshots per dataset. It supports hashtags as a proxy for the users who use labelled hashtags. It says nothing directly about the users the model infers who never post on the topic, which is the paper's headline capability.
- **Sample.** The authors concede a non-representative sample of Santiago news-followers. The work record links this to [[hargittai-2015-bigger-always-better]] as "relevant to", not "explains": Zhou does not cite Hargittai.

**Between the paper and the Melange port** (`fec/stance/stance_dynamics.py`, checked against `data/stance/stance_dynamics.json`):
- **Static band.** The paper's median + MAD band assumes a continuous probability. Computed on pooled stance *signs*, it comes out at 0.708, so almost every move is classed as static. This figure is pooled over members and is UNSTRATIFIED by party or chamber.
- **Neighbour co-movement.** The paper shows it descriptively. The port's test is null: 134 same vs. 122 opposite (52%, n=256 neighbour moves in the live output).
- **Imputation scoring.** The paper scores with macro-F1 because its classes are imbalanced. The port reports 77.5% sign agreement (n=1,087 imputed member-windows) with no party or majority-class baseline. Without that baseline, 77.5% cannot be read as skill.

These are disagreements of method transfer, not of findings. The paper validates its own design on Twitter at a 28-day window. It does not validate the port's 91-day window on the CU press corpus, its carry-forward of last stance, or its imputation.

## Methods

**Dominant method in this theme:** a semi-supervised temporal GNN, as in [[zhou-2026-measuring-stance-dynamics]]. The approach has four parts: weak labels propagated through a user-hashtag bipartite graph; a warm start across fixed snapshots; a contrastive loss with focal loss for class imbalance; and optional user-user interaction layers. Evaluation is macro-F1 against other graph baselines (WLGCN, EvolveGCN, DynaDeno), plus human-coder agreement on the label proxy.

**Weaknesses, as recorded in the work and its record:**
1. **Weak-label circularity.** Training and evaluation labels both come from hashtags. Human validation covers only 100 snapshots per dataset, and only users who post labelled hashtags.
2. **Unreported baseline values.** The main claim of superiority points to a figure and the supplement, not to numbers in the text.
3. **Fixed snapshot window.** 28 days is chosen, not derived. The paper reports no sensitivity analysis on window length, so it gives no basis for any other window, including Melange's 91 days.
4. **Descriptive dynamics only.** The seven movement classes, the median + MAD static band and neighbour co-movement have no null model, permutation test or causal design. Co-movement could come from homophily, shared exposure or a common shock.
5. **Static band built for continuous outputs.** A MAD band on a continuous probability does not carry over to discrete signs. The port shows this: on pooled signs the band is 0.708, UNSTRATIFIED.
6. **Sample bias.** Santiago news-followers on Twitter are not a representative population; see [[hargittai-2015-bigger-always-better]] and [[steinertthrelkeld-2018-twitter-data]] for measurement caveats.

**Edge-weighting caution.** The paper uses count-weighted edges with node-level symmetric degree normalization inside the GCN. This is a neural-network propagation step. It is not evidence for per-edge volume normalization of Melange's SNA edges, which this project measured and REJECTED. Likewise, "follower edges add little" is a finding about Twitter follower graphs in one classifier. It must not be read as support for dropping a regular edge type, such as regular cosponsorship edges, which was also measured and REJECTED. FLAG: if either reading is proposed from this paper, it is not recommended.

## How the works relate

- [[zhou-2026-measuring-stance-dynamics]] — complements — [[malaterre-2024-inferring-social-networks]]: Linked from the system side only; Zhou does not cite it. The HCoI acronym matches Malaterre's hidden communities of interest method, so the link to the HCoI layer is plausible but not a citation chain.
- [[zhou-2026-measuring-stance-dynamics]] — complements — [[steinertthrelkeld-2018-twitter-data]]: Same data family (Twitter). Twitter As Data covers the measurement issues behind Zhou's hashtag-proxy labels and Twitter sampling. Not cited by Zhou. The July 2026 provenance claim for this link was not checked.
- [[zhou-2026-measuring-stance-dynamics]] — complements — [[b-2025-comparing-fine-tuned]]: An alternative stance-detection route: fine-tuned RoBERTa vs. traditional ML on political tweets, against Zhou's graph propagation from hashtag labels. The item's ext title (Roman Urdu) is a stale mismatch and needs correcting.
- [[zhou-2026-measuring-stance-dynamics]] — complements — [[dai-2025-large-language-model]]: Zhou names LLM-based user-level stance as future work (Sec. 6.1, p.17). Dai's LLM-enhanced logic tensor network is that direction.
- [[zhou-2026-measuring-stance-dynamics]] — relevant-to — [[hargittai-2015-bigger-always-better]]: Zhou concedes a non-representative Santiago news-follower sample. Hargittai covers such big-data sampling biases. Not cited by Zhou, so 'relevant to', not 'explains'.
- [[zhou-2026-measuring-stance-dynamics]] — shares-technique — [[kumar-2022-influence-maximization-social]]: Both apply graph embeddings or GNNs to social graphs. Kumar addresses seed selection and Zhou addresses stance classification, so 'same method' overstates the link.

## What it means for the systems

**What this theme means for Melange.** Every bearing here runs through [[zhou-2026-measuring-stance-dynamics]] and the stance-dynamics port. The bearings marked VERIFIED-SOFTENED are verified links whose wording has been softened. None of them validates the port.

**1. Method source for `melange-stance.stance-dynamics` ([[systems/melange-stance]]; VERIFIED-SOFTENED).** `fec/stance/stance_dynamics.py` is an explicit port without the GNN. It uses:
- 91-day windows on the CU press corpus;
- carry-forward of a member's last stance through silent windows;
- the seven-dynamics taxonomy;
- a test of neighbour co-movement;
- neighbour-based imputation of stance for silent members.

The paper supports the design ideas only. It does not validate carry-forward of a discrete label (its carry-forward is of learned embeddings), the 91-day window, or the imputation. All three remain **UNVERIFIED**.

**2. Qualifications on the port (VERIFIED and VERIFIED-SOFTENED).**
- **Static band.** It is ill-posed on signs: 0.708 on pooled stance signs, UNSTRATIFIED, so nearly every move is static. Either compute the band on a continuous stance score, as the paper does, or replace it with a rule for discrete transitions. Report it stratified by party.
- **Neighbour co-movement.** It is null in the live output: 134 same / 122 opposite, 52%, n=256. Neither the paper nor the port supports language saying neighbours move one another. Describe it as co-movement only, and as not detected.
- **Imputation accuracy.** The 77.5% sign agreement (n=1,087 imputed member-windows, 91-day windows) needs a party baseline and a majority-class baseline, and should be scored with macro-F1 to match the paper. Until then it is not a skill figure.
- **Graph-derived stance.** Any neighbour or graph-derived stance is a heuristic model output, never closeness, access or an introduction probability (D-025).

**3. Possible extension to `melange-sna.multilayer-social-press-edges` ([[systems/melange-sna]]; VERIFIED-SOFTENED).** The paper finds that active interaction edges (mention, quote, reply) carry more stance signal than passive follower edges, at about +3% macro-F1. This suggests an interaction layer may beat a static follow layer for the social/press layers. It is a lead to measure, not a finding for Melange.

FLAG: this must not become an argument to drop regular cosponsorship edges, or to adopt per-edge volume normalization because of the paper's degree normalization. Both were measured and REJECTED here. LDA stays an identity source, not an edge or weight source (D-021).

**4. Bearings that do not apply.** Nothing in this theme touches reciprocity (which is not validated, D-104..D-108), revolving-door edges, or money models.

## Gaps

**Gaps in the theme itself**
- Paul's notes are missing: the annotation document is four bare links. Three of the four links never became work records, so the theme rests on one paper. Before this theme supports any claim about neural-network methods, Paul needs to locate his Neural Networks thinking (Curated Annotations, Path-Finding or SNA bibliographies) or annotate the three remaining links.
- The six `related` slugs are system-side links, not annotated works in this theme. Only the Dai link (LLM stance as Zhou's stated future work) has textual support in the paper. The rest are not citation chains.

**Gaps in the evidence base for Melange's use**
- **No validation of the port's choices.** Nothing tests the 91-day window, carry-forward of a discrete stance through silent windows, or neighbour imputation on legislator press data. These need a window-sensitivity check and a baselined imputation score: macro-F1 against party and majority-class baselines, stratified by party and chamber.
- **No null model for dynamics.** Neither the paper nor the port has a permutation or shuffled-network null for neighbour co-movement or flip curves, and neither attempts to separate homophily from common shocks.
- **No transfer evidence.** All results are from Twitter users in Chile's 2020/2022 referendums at 28-day snapshots. There is nothing on elite or legislator corpora, on press releases, or on US settings.
- **Unreported baseline numbers.** Fig. 2 and supplement Table S4 were not read, so the >5% claim stays unchecked.
- **Label-proxy coverage.** Human validation covers 100 snapshots per dataset, and only users who post labelled hashtags. Accuracy on silent or inferred users is unmeasured, and those are the users that matter for imputing stance for silent members.
- **Missing comparison.** There is no work comparing GNN stance propagation against the text-only stance classifiers Melange already runs, or against LLM stance ([[dai-2025-large-language-model]], [[b-2025-comparing-fine-tuned]]), on the same data.

## All works

- [[zhou-2026-measuring-stance-dynamics]] (2026) — Measuring stance dynamics in political debate using temporal graph neural networks
