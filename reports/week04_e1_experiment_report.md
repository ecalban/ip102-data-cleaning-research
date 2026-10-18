# Week 04 — E1 Experiment Report

## 1. Objective

This experiment evaluates a ResNet50-based image classification
model on the IP102 agricultural pest dataset.

The main objective is to evaluate classification performance,
performance on underrepresented classes, and prediction calibration.

## 2. Dataset

The IP102 dataset contains 102 pest classes.

| Split | Number of images |
|---|---:|
| Training | 45,095 |
| Validation | 7,508 |
| Test | 22,619 |
| Total | 75,222 |

## 3. Model and Training Configuration

| Parameter | Value |
|---|---|
| Architecture | ResNet50 |
| Pretrained weights | ImageNet-1K V2 |
| Number of classes | 102 |
| Optimizer | AdamW |
| Learning rate | 0.0001 |
| Weight decay | 0.0001 |
| Batch size | 32 |
| Training epochs | 10 |
| Random seed | 42 |

The best checkpoint was selected according to validation Macro-F1.

Best epoch: **8**

Best validation Macro-F1: **0.6222**

## 4. E1 Test Results

The best model was evaluated on 22,619 test images.

| Metric | E1 |
|---|---:|
| Test Accuracy | 0.7111 |
| Macro Precision | 0.6624 |
| Macro Recall | 0.6175 |
| Macro-F1 | 0.6271 |
| Tail Recall | 0.5273 |
| ECE (15 bins) | 0.063392 |

Tail Recall was calculated using the same 21 tail classes
identified in the E0 experiment.

ECE was calculated using 15 confidence bins.

## 5. E0 vs E1 Comparison

| Metric | E0 | E1 |
|---|---:|---:|
| Test Accuracy | 0.708121 | 0.7111 |
| Test Macro-F1 | 0.633478 | 0.6271 |
| Tail Recall | 0.5303 | 0.5273 |
| ECE (15 bins) | 0.119675 | 0.063392 |

E1 achieved slightly higher overall test accuracy.

E0 achieved slightly higher Macro-F1 and Tail Recall,
suggesting a small advantage in balanced classification
performance and recognition of underrepresented classes.

E1 achieved a lower ECE, suggesting better agreement
between prediction confidence and observed accuracy.

The ECE comparison assumes that both experiments used
the same calibration evaluation procedure.

## 6. Visual Results

### E1 Training Loss

![E1 Training Loss](../results/E1/loss_curve.png)

### E1 Training Metrics

![E1 Training Metrics](../results/E1/metrics_curve.png)

### E1 Confusion Matrix

![E1 Confusion Matrix](../results/E1/test_results/confusion_matrix.png)

### E0 vs E1 Comparison

![E0 vs E1](../results/comparison/e0_e1_comparison.png)

## 7. Conclusion

The experiments demonstrate that model selection depends
on the evaluation criterion.

E1 provides slightly higher overall classification accuracy
and lower calibration error.

E0 provides slightly higher Macro-F1 and Tail Recall.

Future analysis should investigate class-level performance,
confusion patterns, and the impact of class imbalance.

