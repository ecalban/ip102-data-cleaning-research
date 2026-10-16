# Week 2 — IP102 Dataset Exploration and Integrity Analysis

## 1. Objective

The objective of Week 2 was to examine the IP102 dataset before
training the baseline classification model.

The analysis focused on:

- Dataset structure and split sizes
- Class distribution and imbalance
- Minority-class identification
- Missing image detection
- Corrupted image detection
- Data preparation for subsequent experiments

## 2. Dataset Overview

The IP102 dataset contains 75,222 pest images belonging to
102 classes.

The original dataset splits were preserved throughout the analysis.

| Dataset Split | Number of Images |
|---------------|-----------------:|
| Training | 45,095 |
| Validation | 7,508 |
| Test | 22,619 |
| **Total** | **75,222** |

The training split is used for model training and subsequent
data cleaning experiments.

The validation split is used for model selection.

The test split is reserved for final evaluation.

## 3. Dataset Integrity Verification

Image integrity checks were performed across the training,
validation, and test splits.

The verification procedure checked:

1. Whether each referenced image file existed.
2. Whether each image could be verified using the Pillow library.

### Results

| Integrity Check | Result |
|-----------------|-------:|
| Missing images | 0 |
| Corrupted images | 0 |

No missing or corrupted images were detected during the checks.

These results indicate that the dataset files were accessible
and passed the performed image integrity verification.

However, file integrity does not guarantee that all images
have correct class labels.

## 4. Class Distribution Analysis

The training dataset contains substantial class imbalance.

The largest training class is class 101, containing 3,444 images.

The smallest training class is class 72, containing 42 images.

| Statistic | Result |
|-----------|-------:|
| Largest class | Class 101 |
| Largest class size | 3,444 |
| Smallest class | Class 72 |
| Smallest class size | 42 |
| Approximate imbalance ratio | 82:1 |

The largest class contains approximately 82 times as many
training images as the smallest class.

This imbalance motivates the use of class-sensitive
evaluation metrics.

In particular, overall accuracy alone may not adequately
represent performance on underrepresented classes.

## 5. Minority-Class Definition

For this study, minority classes are defined as the
21 classes with the fewest training images.

The definition is based exclusively on the original
training split and remains fixed across all experiments.

### Minority-Class Distribution

| Class ID | Training Images |
|----------|----------------:|
| 72 | 42 |
| 80 | 44 |
| 61 | 47 |
| 63 | 50 |
| 98 | 55 |
| 81 | 59 |
| 36 | 62 |
| 35 | 82 |
| 65 | 84 |
| 17 | 88 |
| 85 | 100 |
| 12 | 103 |
| 75 | 103 |
| 60 | 105 |
| 53 | 106 |
| 55 | 111 |
| 89 | 113 |
| 43 | 114 |
| 96 | 126 |
| 90 | 135 |
| 79 | 135 |

### Minority-Class Statistics

- Number of minority classes: 21
- Total number of classes: 102
- Minority classes as a percentage of all classes: 20.6%
- Total minority-class training images: 1,864
- Minority-class images as a percentage of training data: 4.13%

Although these 21 classes represent approximately one-fifth
of all categories, they account for only 4.13% of the
training images.

This observation highlights the importance of examining
the effects of data cleaning on minority classes.

## 6. Implications for Data Cleaning

The observed class imbalance introduces an important risk.

If samples are removed solely according to global loss
or confidence rankings, some minority classes may lose
a disproportionately large share of their training data.

To investigate this possibility, the experimental design
includes a class-protected high-loss cleaning strategy.

The project will compare:

- E0: Original training dataset
- E1: Random 5% sample removal
- E2: High-loss 5% sample removal
- E3: Low-confidence 5% sample removal
- E4: Class-protected high-loss sample removal

Only training samples will be considered for removal.

## 7. Evaluation Strategy

The following metrics will be used to evaluate the
effects of data cleaning:

### Accuracy

Measures the proportion of correctly classified images.

### Macro-F1

Computes the unweighted mean of F1 scores across
all 102 classes.

### Tail-Class Recall

Computes the unweighted mean of recall values across
the 21 predefined minority classes.

### Expected Calibration Error

Measures the discrepancy between prediction confidence
and observed accuracy.

The study uses 15 equal-width confidence bins.

## 8. Week 2 Outcomes

The main outcomes of Week 2 were:

1. Confirmed the original IP102 dataset split sizes.
2. Verified that no referenced images were missing.
3. Detected no corrupted images in the integrity checks.
4. Identified substantial training class imbalance.
5. Defined the 21 minority classes for subsequent evaluation.
6. Established the dataset analysis foundation for
   ResNet50 baseline training.

## 9. Limitations

The integrity checks verified image accessibility and
basic file integrity.

They did not establish whether the dataset contains
incorrect labels, ambiguous examples, duplicated images,
or visually irrelevant content.

These issues may be relevant to the later sample-scoring
and targeted cleaning experiments.

## 10. Next Steps

Week 3 focuses on training an ImageNet-pretrained
ResNet50 baseline model using the complete original
training split.

The baseline results will provide a reference for
evaluating the effects of targeted data cleaning.

---

**Documentation note:** This report was reconstructed
from the recorded Week 2 analysis results for inclusion
in the GitHub research repository.
