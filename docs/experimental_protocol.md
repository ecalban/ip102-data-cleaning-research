# Experimental Protocol

## 1. Study Objective

This study investigates the effects of targeted training data
cleaning on pest image classification performance using the
IP102 dataset.

The main objective is to compare targeted sample removal
strategies against both the original training dataset and
random sample removal.

Special attention is given to minority-class performance
and prediction calibration.

## 2. Dataset

The IP102 dataset contains 75,222 images belonging to
102 pest categories.

The original dataset splits are preserved:

| Split | Number of Images |
|-------|-----------------:|
| Training | 45,095 |
| Validation | 7,508 |
| Test | 22,619 |
| Total | 75,222 |

Only the training split is modified during data cleaning.

Validation and test images are never removed.

## 3. Minority-Class Definition

Minority classes are defined as the 21 classes with the
smallest number of samples in the original training split.

The class IDs are:

72, 80, 61, 63, 98, 81, 36, 35, 65, 17, 85,
12, 75, 60, 53, 55, 89, 43, 96, 90, 79

These classes contain 1,864 training images in total.

The minority-class definition remains fixed across all
experiments.

## 4. Baseline Architecture

All experiments use the same model architecture:

- Architecture: ResNet50
- Initialization: ImageNet-pretrained weights
- Output classes: 102
- Training strategy: Full fine-tuning

## 5. Training Configuration

The baseline training configuration is:

| Parameter | Value |
|-----------|-------|
| Random seed | 42 |
| Epochs | 10 |
| Batch size | 32 |
| Optimizer | AdamW |
| Learning rate | 0.0001 |
| Weight decay | 0.0001 |
| Loss function | CrossEntropyLoss |
| Model selection | Best validation Macro-F1 |

The same configuration is planned for the cleaning
experiments to support consistent comparisons.

## 6. Image Preprocessing

### Training

1. Resize images to 256 x 256.
2. Apply a random 224 x 224 crop.
3. Apply random horizontal flipping.
4. Convert images to tensors.
5. Normalize using ImageNet mean and standard deviation.

### Validation and Testing

1. Resize images to 256 x 256.
2. Apply a center crop of 224 x 224.
3. Convert images to tensors.
4. Normalize using ImageNet mean and standard deviation.

## 7. Experimental Conditions

### E0 — Baseline

Train ResNet50 using the complete original training split.

No samples are removed.

### E1 — Random Removal

Randomly remove approximately 5% of training samples.

The selection uses a fixed random seed.

This experiment serves as a control for the effect of
reducing training set size.

### E2 — High-Loss Removal

Rank training samples using a loss-based difficulty score.

Remove approximately 5% of samples with the highest scores.

The sample-scoring procedure must be fixed and documented
before running the experiment.

### E3 — Low-Confidence Removal

Rank training samples using the model's predicted
probability for their ground-truth class.

Remove approximately 5% of samples with the lowest
true-class confidence scores.

The scoring procedure must be consistent and reproducible.

### E4 — Class-Protected High-Loss Removal

Use loss-based sample ranking while limiting the number
of removed samples within each class.

The planned per-class removal limit is:

floor(0.05 * number_of_training_samples_in_class)

This strategy is designed to protect minority classes
from disproportionate sample removal.

Because of the per-class limits, the total number of
removed samples may be lower than in E1, E2, and E3.

This difference must be reported when comparing results.

## 8. Evaluation Metrics

### Accuracy

The proportion of correctly classified test samples.

Higher values indicate better performance.

### Macro-F1

The unweighted mean of the F1 scores across all 102 classes.

This metric gives equal importance to each class.

### Tail-Class Recall

The unweighted mean of per-class recall values for the
21 predefined minority classes.

### Expected Calibration Error

ECE measures the difference between prediction confidence
and observed accuracy.

The evaluation uses 15 equal-width confidence bins.

Lower ECE values indicate better calibration.

## 9. Model Selection and Test Evaluation

The validation split is used to select the best checkpoint
based on Macro-F1.

The test split is reserved for final evaluation.

Test results must not be used to choose sample-removal
thresholds or tune model hyperparameters.

## 10. Sample Scoring and Data Leakage Prevention

Sample removal decisions must be based exclusively on
training images and their labels.

Validation and test images must not contribute to
sample-level cleaning scores.

Training samples should be scored using deterministic
evaluation preprocessing rather than random augmentation.

For persistent-loss scoring, the scoring checkpoints
and aggregation rule must be documented.

## 11. Reproducibility

The following information should be recorded for each
experiment:

- Random seed
- Training configuration
- Dataset split
- Removed sample IDs
- Number of removed samples per class
- Model checkpoint
- Validation history
- Final evaluation metrics

## 12. Completed Baseline Results

The E0 baseline experiment has been completed.

The best checkpoint was selected at epoch 9.

| Metric | E0 Result |
|--------|----------:|
| Test Accuracy | 0.7081 |
| Test Macro-F1 | 0.6335 |
| Tail-Class Recall | 0.5303 |
| ECE (15 bins) | 0.1197 |

These results provide the reference point for subsequent
cleaning experiments.

## 13. Methodological Limitations

The current baseline results come from a single training run.

Small differences between experiments may reflect
training variability rather than the cleaning strategy alone.

Additional random seeds would strengthen the conclusions
if computational resources permit.

This protocol documents the intended experimental design.
Any later methodological changes should be recorded
explicitly in the repository.
