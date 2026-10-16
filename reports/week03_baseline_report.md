# Week 3 — ResNet50 Baseline Training and Evaluation

## 1. Objective

The objective of Week 3 was to establish a reproducible
baseline for pest image classification using the IP102 dataset.

An ImageNet-pretrained ResNet50 model was fine-tuned on the
complete original training split without removing any images.

This experiment is designated E0 and serves as the reference
for subsequent data cleaning experiments.

## 2. Dataset

The original IP102 dataset splits were preserved.

| Split | Number of Images |
|-------|-----------------:|
| Training | 45,095 |
| Validation | 7,508 |
| Test | 22,619 |
| Total | 75,222 |

The dataset contains 102 pest classes.

## 3. Computing Environment

- Platform: Google Colab
- GPU: NVIDIA Tesla T4
- Framework: PyTorch
- Architecture: ResNet50
- Initialization: ImageNet pretrained weights

## 4. Training Configuration

| Parameter | Value |
|-----------|-------|
| Random seed | 42 |
| Epochs | 10 |
| Batch size | 32 |
| Optimizer | AdamW |
| Learning rate | 0.0001 |
| Weight decay | 0.0001 |
| Loss function | CrossEntropyLoss |
| Training strategy | Full fine-tuning |
| Checkpoint selection | Best validation Macro-F1 |

### Training Image Transformations

- Resize to 256 × 256
- Random crop to 224 × 224
- Random horizontal flip
- Convert to tensor
- ImageNet normalization

### Validation and Test Transformations

- Resize to 256 × 256
- Center crop to 224 × 224
- Convert to tensor
- ImageNet normalization

## 5. Training Results

The baseline model was trained for 10 epochs.

| Epoch | Train Loss | Val Loss | Val Accuracy | Val Macro-F1 |
|------:|-----------:|---------:|-------------:|-------------:|
| 1 | 1.9845 | 1.3534 | 0.6275 | 0.4762 |
| 2 | 1.1547 | 1.1655 | 0.6665 | 0.5530 |
| 3 | 0.8989 | 1.1171 | 0.6893 | 0.5840 |
| 4 | 0.7442 | 1.0894 | 0.6998 | 0.6114 |
| 5 | 0.6198 | 1.1111 | 0.6973 | 0.6103 |
| 6 | 0.5299 | 1.1362 | 0.7036 | 0.6206 |
| 7 | 0.4556 | 1.1604 | 0.7036 | 0.6255 |
| 8 | 0.3985 | 1.1942 | 0.7126 | 0.6327 |
| 9 | 0.3519 | 1.2366 | 0.7108 | 0.6343 |
| 10 | 0.3164 | 1.2622 | 0.7076 | 0.6252 |

## 6. Best Checkpoint Selection

The best checkpoint was selected using validation Macro-F1.

The highest validation Macro-F1 was achieved at epoch 9:

- Best epoch: 9
- Validation Accuracy: 0.7108
- Validation Macro-F1: 0.6343

The epoch 9 checkpoint was saved as `best_model.pth`.

Although validation accuracy was slightly higher at epoch 8,
epoch 9 was selected according to the predefined Macro-F1
criterion.

## 7. Test Evaluation

The best checkpoint was evaluated on the original test split
containing 22,619 images.

| Metric | Result |
|--------|-------:|
| Test Accuracy | 0.7081 |
| Test Macro-F1 | 0.6335 |
| Tail-Class Recall | 0.5303 |
| ECE (15 bins) | 0.1197 |

### Accuracy

The model correctly classified approximately 70.81% of the
test images.

### Macro-F1

The Macro-F1 score was 0.6335, reflecting performance
across all 102 classes with equal class weighting.

### Tail-Class Recall

The mean recall across the 21 predefined minority classes
was 0.5303.

This result provides an important reference for evaluating
whether data cleaning benefits or harms underrepresented
classes.

### Expected Calibration Error

The model achieved an ECE of 0.1197 using 15 equal-width
confidence bins.

Lower ECE values indicate better calibration.

## 8. Training Behavior

Training loss decreased throughout the 10 epochs.

Validation loss reached its minimum at epoch 4 and
increased during later epochs.

This pattern suggests possible overfitting.

However, validation Macro-F1 continued improving until
epoch 9, indicating that different evaluation metrics
captured different aspects of model performance.

The model was selected using validation Macro-F1,
as specified in the experimental protocol.

## 9. Interpretation

The baseline achieved similar validation and test
Macro-F1 scores:

- Validation Macro-F1: 0.6343
- Test Macro-F1: 0.6335

This indicates similar performance on these two splits.

The lower minority-class recall highlights the importance
of evaluating class-sensitive outcomes in later experiments.

The baseline results alone do not establish whether
targeted data cleaning will improve performance.

## 10. Limitations

The baseline was trained using a single random seed.

Therefore, the results do not quantify variability across
independent training runs.

The observed validation loss trend suggests possible
overfitting, but additional analysis would be required
to characterize its causes.

The ECE value depends on the chosen calibration
estimation procedure.

## 11. Saved Artifacts

The original experiment artifacts were saved to Google Drive:

`MyDrive/IP102_Proje/results/E0/`

The saved files include:

- `best_model.pth`
- `training_history.csv`
- `test_results.csv`
- `class_recalls.csv`

The model checkpoint is not included in the GitHub
repository because of its size.

## 12. Week 3 Outcome

The E0 baseline experiment was successfully completed.

The resulting model and evaluation metrics establish
a reference for the planned data cleaning experiments.

## 13. Next Steps

Week 4 will focus on calculating training-sample
difficulty scores using loss and true-class confidence.

These scores will support the targeted cleaning
strategies planned for Week 5.

---

**Documentation note:** This report was reconstructed
from the recorded Week 3 training logs and test results
for inclusion in the research repository.
