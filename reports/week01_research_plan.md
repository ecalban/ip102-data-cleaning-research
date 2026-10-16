# Week 1 — Research Planning and Experimental Design

## 1. Research Title

Investigating the Impact of Targeted Training Data Cleaning on Pest
Classification Performance and Minority Classes in the IP102 Dataset

## 2. Research Objective

This project investigates whether targeted removal of potentially
problematic training images can improve pest image classification
performance compared with random data removal.

Special attention is given to minority classes, which contain
relatively few training samples.

## 3. Research Questions

1. Does targeted training data cleaning improve classification
   performance compared with the original training dataset?

2. Does removing high-loss samples produce better results than
   removing randomly selected samples?

3. How does confidence-based sample removal affect classification
   performance?

4. Can class-protected cleaning reduce potential negative effects
   on minority classes?

5. How do different cleaning strategies affect model calibration?

## 4. Dataset

The project uses the IP102 pest image classification dataset.

- Number of classes: 102
- Total images: 75,222
- Training images: 45,095
- Validation images: 7,508
- Test images: 22,619

## 5. Baseline Model

The baseline model is ResNet50 initialized with ImageNet-pretrained
weights.

The same model architecture and training configuration will be used
across the planned experiments to support meaningful comparisons.

## 6. Experimental Design

| Experiment | Description |
|------------|-------------|
| E0 | Baseline training without data removal |
| E1 | Random removal of 5% of training samples |
| E2 | Removal of 5% of samples with the highest persistent loss |
| E3 | Removal of 5% of samples with the lowest true-class confidence |
| E4 | Class-protected high-loss sample removal |

Only training samples will be removed.

The validation set will be used for model selection, while the test
set will be reserved for final evaluation.

## 7. Evaluation Metrics

The experiments will be evaluated using:

- Classification Accuracy
- Macro-F1 Score
- Tail-Class Recall
- Expected Calibration Error (ECE)

Tail classes are defined using the 21 least-represented classes
in the original training split.

## 8. Computing Environment

- Google Colab
- NVIDIA Tesla T4 GPU
- Python
- PyTorch
- Torchvision

## 9. Six-Week Research Plan

| Week | Planned Activity |
|------|------------------|
| 1 | Research planning and experimental design |
| 2 | Dataset exploration and integrity verification |
| 3 | ResNet50 baseline training and evaluation |
| 4 | Training sample loss and confidence analysis |
| 5 | Data cleaning experiments |
| 6 | Comparative analysis and final reporting |

## 10. Reproducibility Considerations

The experiments will use a fixed random seed and consistent
training settings.

The original training, validation, and test splits will be
preserved.

Data cleaning decisions will be based on training samples only.

## 11. Week 1 Outcome

The research objective, experimental comparisons, evaluation
metrics, and six-week project plan were established.

This document is a reconstructed record of the Week 1 research
plan, prepared for project documentation.
