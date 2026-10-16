# Targeted Training Data Cleaning for Pest Classification on IP102

## Project Overview

This research investigates the impact of targeted training data
cleaning on pest image classification performance, with a
particular focus on minority classes.

We use the IP102 dataset and an ImageNet-pretrained ResNet50
model to compare different training data cleaning strategies.

## Dataset

- Dataset: IP102
- Number of classes: 102
- Total images: 75,222
- Training images: 45,095
- Validation images: 7,508
- Test images: 22,619

## Experimental Design

| Experiment | Strategy |
|------------|----------|
| E0 | Baseline (no data removal) |
| E1 | Random 5% training data removal |
| E2 | Highest-loss 5% removal |
| E3 | Lowest-confidence 5% removal |
| E4 | Class-protected high-loss removal |

## Baseline Results (E0)

| Metric | Result |
|--------|--------|
| Test Accuracy | 70.81% |
| Test Macro-F1 | 63.35% |
| Tail-Class Recall | 53.03% |
| Expected Calibration Error (ECE) | 0.1197 |

Model: ResNet50 (ImageNet pretrained)

Best checkpoint: Epoch 9

## Project Progress

- [x] Week 1: Literature review and experimental planning
- [x] Week 2: Dataset exploration and integrity checks
- [x] Week 3: Baseline training and evaluation
- [ ] Week 4: Training sample loss and confidence analysis
- [ ] Week 5: Data cleaning experiments
- [ ] Week 6: Comparative analysis and final report

## Evaluation Metrics

- Classification Accuracy
- Macro-F1 Score
- Tail-Class Recall
- Expected Calibration Error (ECE)

## Computing Environment

- Google Colab
- NVIDIA Tesla T4 GPU
- PyTorch
- ResNet50

## Project Status

The baseline experiment (E0) has been completed.
The next stage focuses on identifying potentially
problematic training samples using loss and confidence scores.
