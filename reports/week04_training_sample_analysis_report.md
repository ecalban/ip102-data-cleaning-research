# Week 04 — Training Sample Loss and Confidence Analysis

**Project:** Investigating the Impact of Targeted Training Data Cleaning on Pest Classification Performance and Minority Classes in the IP102 Dataset  
**Status:** Completed scoring and candidate-selection analysis; no cleaning experiment has been trained.  
**Dataset:** IP102 v1.1  
**Baseline:** E0, ImageNet-pretrained ResNet50

## 1. Objective

Score every **training** image with the trained E0 classifier, characterize class-wise difficulty, and analyze the prospective class-distribution effects of candidate 5% training-data removal rules. This is a **pre-training selection analysis**, not evidence that removing examples improves generalization.

## 2. Dataset and fixed minority-class definition

| Split | Images |
|---|---:|
| Train | 45,095 |
| Validation | 7,508 |
| Test | 22,619 |

The dataset has 102 classes. For consistency with Week 02 and E0 evaluation, **tail classes** are the 21 least frequent classes in the original training split:

`[72, 80, 61, 63, 98, 81, 36, 35, 65, 17, 85, 12, 75, 60, 53, 55, 89, 43, 96, 90, 79]`

They contain **1,864 training images**. An exploratory analysis used a different bottom-quartile definition (26 classes, 2,616 images); those numbers are **not** used for the primary comparison.

## 3. Scoring method

The trained E0 checkpoint was applied to all 45,095 training images using deterministic evaluation preprocessing: resize to 256 × 256, center crop to 224 × 224, convert to tensor, and ImageNet normalization. Recorded per-image quantities include cross-entropy loss, true-class confidence, top-1 confidence, best-other-class confidence, true-versus-best-other margin, predictive entropy, predicted class, and correctness.

Scores were generated **on the same training split used to fit E0**. Consequently, training accuracy and confidence are descriptive, potentially optimistic in-sample measurements; they are not held-out generalization estimates. The test split was not used to choose removal candidates.

## 4. Observed training-score results

| Statistic | Value |
|---|---:|
| Scored training images | 45,095 |
| Incorrect training predictions | 3,190 |
| Correct training predictions | 41,905 |
| Training top-1 accuracy | 92.93% |
| Mean cross-entropy loss | 0.214062 |
| Median cross-entropy loss | 0.013095 |
| 75th-percentile loss | 0.123155 |
| Maximum loss | 9.489944 |

The mean substantially exceeds the median, indicating a right-skewed loss distribution. High loss flags samples that E0 finds difficult or inconsistent with their provided labels; it **does not prove annotation errors**.

Examples of challenging classes include class 17 (88 images; mean loss 1.063433; training accuracy 60.23%) and class 98 (55 images; mean loss 0.977002; training accuracy 65.45%). Minority classes are not uniformly difficult: class 65 has 84 images and training accuracy 98.81%.

## 5. Candidate removal strategies

The nominal budget is `floor(0.05 × 45,095) = 2,254` training images. Candidate rules were analyzed without deleting images or training new models.

| Candidate rule | Selected images | Share of training set | Tail images selected | Share of original tail images |
|---|---:|---:|---:|---:|
| E1 — random 5% (seed 42) | 2,254 | 4.998% | 99 | 5.311% |
| E2 — highest cross-entropy loss | 2,254 | 4.998% | 146 | 7.833% |
| Original E3 — lowest true-class confidence | 2,254 | 4.998% | 146 | 7.833% |
| **Proposed** E3 — lowest true-vs-other margin | 2,254 | 4.998% | 132 | 7.082% |
| E4 — highest loss, capped at 5% per class | 2,207 | 4.894% | 85 | 4.560% |

The original E3 and E2 select **exactly the same 2,254 samples**, because per-sample cross-entropy is `−log(p_true)`; they must not be presented as independent treatments. The proposed margin-based E3 shares 1,917 of 2,254 samples with E2 (337 differ), but **requires a documented protocol amendment before training**.

E2 selects 146/1,864 (7.83%) of tail images, compared with E1's 99/1,864 (5.31%) and E4's 85/1,864 (4.56%). The class cap reduces concentrated selection from individual classes. However, E4 selects **47 fewer images overall** than E2; comparisons must explicitly acknowledge this budget difference. These are descriptive selection rates, not model-performance results.

## 6. Interpretation and limitations

- High-loss selection disproportionately targets some difficult minority classes, but the relationship is heterogeneous across classes.
- Removal candidates can include mislabeled, ambiguous, rare, or genuinely difficult but correctly labeled images; manual review would be needed to distinguish these mechanisms.
- The E0 model scored its own training examples; the analysis should not be interpreted as unbiased error detection.
- The fixed 21-class tail definition is necessary for comparison with the E0 tail-recall baseline.
- E4's per-class floor operation produces 2,207 rather than 2,254 removals. Either retain this design and report the difference or pre-register a revised matched-budget design; do not silently change the rule.
- No E1–E4 retraining or held-out evaluation has yet occurred. No claim of improved accuracy, macro F1, tail recall, or calibration is warranted.

## 7. Baseline reference and next steps

The existing E0 **test-set** metrics (distinct from the in-sample results above) are: accuracy **0.70812**, macro F1 **0.63348**, tail recall **0.53030**, and ECE-15 **0.11967**.

Before Week 05, freeze the selection rules and approve or reject the E3 amendment. Then retrain E1–E4 using controlled, comparable settings and evaluate on the same held-out test split, including overall and tail metrics. Preserve the original train/validation/test partitions and prevent test-informed selection.

## 8. Reproducibility artifacts

The following files were reported as successfully exported to Google Drive under `MyDrive/IP102_Proje/results/week04/`:

- `training_sample_scores.csv` — per-image scores
- `class_score_summary.csv` — per-class statistics with the fixed 21-class tail flag
- `removal_rule_summary.csv` — candidate-selection comparison
- `candidate_removal_flags.csv` — per-image candidate membership
- `scoring_metadata.json` — checkpoint and annotation hashes, preprocessing, seed, and selection rules

Additional exploratory files are present (`e0_training_sample_scores.csv`, `e0_class_statistics.csv`, `e2_global_high_loss_candidates.csv`, `e2_global_high_loss_class_impact.csv`), but the exploratory class-impact file uses the **different 26-class definition** and should not be used for the primary tail comparison.

**Provenance:** Results above are transcribed from the user's Colab outputs and the supplied Week 02–04 notebooks. The Google Drive CSV contents were not independently reloaded or audited in this report.
