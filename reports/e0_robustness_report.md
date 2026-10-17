# E0 ResNet50: Exploratory Image-Transformation Robustness Report

## 1. Purpose and scope

This exploratory experiment examines how the E0 IP102 pest classifier responds to controlled changes in a single external insect photograph. It is supplementary to the main E0–E4 **training-data-cleaning** study; it is **not** an evaluation of the IP102 official test split or an estimate of dataset-wide robustness.

## 2. Model and test setup

- **Model:** E0, ImageNet-pretrained ResNet50 fine-tuned for 102 IP102 classes.
- **Checkpoint:** `results/E0/best_model.pth` in the project's Google Drive storage (not committed to GitHub).
- **Inference preprocessing:** resize to 256 × 256, center crop to 224 × 224, convert to tensor, and normalize using ImageNet mean and standard deviation.
- **Input image:** `large (1).jpeg` (one external photograph).
- **Expected IP102 class:** ID 101, `Cicadellidae`, entered in the notebook as `EXPECTED_CLASS_ID = 101`. **Independent verification of the photograph's biological identification is not documented here.** Accordingly, “match” below means agreement with this user-specified reference class, not independently established ground-truth correctness.
- **Data source:** `results/robustness/transformation_results.csv` (13 rows, including the original image).
- **Reproduction:** `notebooks/e0_robustness_testing.ipynb`.

### Transformations

The notebook applies Gaussian blur with radii 2, 6, and 15; brightness factors 0.3 and 1.8; contrast factor 0.3; rotations of 90° and 180° with canvas expansion; pixelation by downsampling to approximately 3% of each original dimension and upsampling with nearest-neighbor interpolation; downsampling to 16 × 16 pixels and upsampling; and centered black rectangular occlusions. The occlusion parameters 0.4 and 0.7 refer to **each side length** of the rectangular mask, not 40% and 70% of total image area (approximately 16% and 49% of the image area, respectively, ignoring rounding).

## 3. Results

Values are rounded from the saved CSV. “Expected-class probability” is the model's softmax probability for IP102 class 101.

| Variation | Top-1 predicted class (ID) | Top-1 confidence | Expected-class probability | Matches reference? |
|---|---|---:|---:|:---:|
| Original | Cicadellidae (101) | 86.64% | 86.64% | Yes |
| Slight blur | Cicadellidae (101) | 40.37% | 40.37% | Yes |
| Strong blur | Erythroneura apicalis (72) | 44.07% | 0.84% | No |
| Dark image | Cicadellidae (101) | 76.88% | 76.88% | Yes |
| Bright image | Cicadellidae (101) | 55.01% | 55.01% | Yes |
| Low contrast | Cicadellidae (101) | 76.68% | 76.68% | Yes |
| Rotated 90 | grub (14) | **99.51%** | **0.29%** | No |
| Rotated 180 | Cicadellidae (101) | 95.94% | 95.94% | Yes |
| Extreme blur | Dasineura sp (93) | 42.56% | 0.04% | No |
| Pixelated | rice water weevil (10) | 25.84% | 0.30% | No |
| Low resolution | rice gall midge (5) | 34.00% | 0.52% | No |
| 40% occlusion | Phyllocoptes oleiverus ashmead (75) | 19.70% | 6.06% | No |
| 70% occlusion | Phyllocoptes oleiverus ashmead (75) | 8.31% | 1.53% | No |

The top-1 prediction matches the user-specified reference class in **6 of 13** tested conditions (including the unmodified image). These are correlated transformations of **one image**; 6/13 must **not** be interpreted as model accuracy or population-level robustness.

## 4. Observations

**Blur sensitivity.** The probability assigned to class 101 decreases from 86.64% on the original image to 40.37% with slight blur, 0.84% with strong blur, and 0.04% with extreme blur. The top-1 class changes under strong and extreme blur. `Erythroneura apicalis` is biologically related to the Cicadellidae family, but IP102 treats these as distinct labels.

**Rotation sensitivity and high-confidence disagreement.** The 90°-rotated variant is predicted as `grub` with 99.51% top-1 confidence, while the expected class receives 0.29%. In contrast, the 180° variant remains `Cicadellidae` at 95.94%. This is an example of high-confidence disagreement with the reference label, **not** proof of general miscalibration from a single image. Canvas expansion followed by fixed resize and center crop may change the apparent scale and composition; the 90° result cannot be attributed to orientation alone without a controlled follow-up.

**Illumination and contrast.** The tested dark, bright, and low-contrast variants retain the class-101 top-1 prediction, although confidence differs from the original. This is limited to the tested transformation strengths and photograph.

**Loss of detail and occlusion.** Pixelation, low-resolution reconstruction, and both central occlusion variants change the top-1 label. The two masks differ in their side-length fractions; their labels in the CSV should not be read as percentages of image area.

## 5. Limitations

1. Only one external image was used; results do not generalize to other insects, backgrounds, cameras, or IP102 classes.
2. The expected class was specified manually and is not independently verified in this report.
3. The transformation parameters and preprocessing pipeline jointly determine the observed predictions; effects are not necessarily attributable to one visual property in isolation.
4. Softmax confidence is not a calibrated probability of correctness. No calibration metric was estimated on this 13-variant set.
5. Some IP102 labels refer to taxonomically overlapping groups, so a class mismatch does not always imply an unrelated biological identification.
6. No statistical significance tests or comparison with cleaned-data models (E1–E4) were conducted.

## 6. Recommended follow-up

Repeat the same predefined transformations on a larger, independently labeled and appropriately licensed image set, covering both head and minority IP102 classes. Record transformation parameters, random seeds where applicable, image provenance, and per-image outcomes. To isolate rotation effects, compare preprocessing approaches that preserve the insect's framing and scale. Once E1–E4 are trained, apply the **same** evaluation protocol to each model and report class-stratified changes without conflating exploratory robustness with official IP102 test performance.

## 7. Reproducibility artifacts

- Notebook: `notebooks/e0_robustness_testing.ipynb`
- Raw results: `results/robustness/transformation_results.csv`
- E0 baseline context: `reports/week03_baseline_report.md`

**Status:** Exploratory, single-image analysis; not a confirmatory robustness benchmark.
