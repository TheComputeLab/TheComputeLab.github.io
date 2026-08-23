---
title: "Computer Vision Model Evaluation & Metrics"
description: "A practical guide to evaluating computer vision models using classification, detection, segmentation, and medical-imaging metrics."
weight: 80
toc: true
---

> **A model is not good because it runs. It is good because we can measure how well it performs on data it has never seen.**

Model evaluation is the process of measuring how well a computer vision system performs.

The evaluation strategy depends on the task:

```text
CLASSIFICATION
      ↓
Accuracy / Precision / Recall / F1 / ROC-AUC

OBJECT DETECTION
      ↓
IoU / Precision / Recall / mAP

SEGMENTATION
      ↓
Dice / IoU / Pixel Accuracy

MEDICAL IMAGING
      ↓
Sensitivity / Specificity / Dice / IoU / Error Analysis
```

---

# Why Model Evaluation Matters

A model can appear successful during training but fail on unseen images.

For example:

```text
Training Accuracy     = 98%
Validation Accuracy   = 76%
```

This may indicate overfitting.

The objective is not:

```text
Memorize the training dataset
```

The objective is:

```text
Generalize to unseen data
```

---

# Train, Validation and Test Sets

A typical dataset is divided into:

```text
                 DATASET
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
      TRAIN      VALIDATION     TEST
        │           │           │
        ▼           ▼           ▼
     Training     Tuning      Final
     Weights      Decisions   Evaluation
```

### Training Set

Used to learn model parameters.

### Validation Set

Used to make development decisions such as:

```text
Hyperparameters
Thresholds
Model selection
Early stopping
```

### Test Set

Used for final evaluation after development decisions are complete.

---

# Data Leakage

Data leakage occurs when information from outside the training process improperly influences the model.

Examples include:

```text
Test images used during training
Duplicate images across splits
Patient data split incorrectly
Preprocessing using future information
```

Leakage can produce unrealistically high evaluation scores.

For medical imaging, splitting by patient rather than randomly by individual slices can be particularly important when multiple images come from the same patient.

---

# Classification Evaluation

For image classification, the model predicts one or more classes.

Example:

```text
IMAGE
  ↓
MODEL
  ↓
CAT
```

Common metrics include:

```text
Accuracy
Precision
Recall
F1-score
ROC-AUC
PR-AUC
```

---

# Confusion Matrix

A confusion matrix compares:

```text
Actual Class
     vs
Predicted Class
```

For binary classification:

```text
                  Predicted
                Positive Negative

Actual Positive    TP       FN

Actual Negative    FP       TN
```

Where:

```text
TP = True Positive
TN = True Negative
FP = False Positive
FN = False Negative
```

---

# True Positive

The model predicts positive and the actual class is positive.

```text
Actual:    Tumor
Predicted: Tumor

→ True Positive
```

---

# True Negative

The model predicts negative and the actual class is negative.

```text
Actual:    Normal
Predicted: Normal

→ True Negative
```

---

# False Positive

The model predicts positive when the actual class is negative.

```text
Actual:    Normal
Predicted: Tumor

→ False Positive
```

This is also called a:

```text
Type I Error
```

---

# False Negative

The model predicts negative when the actual class is positive.

```text
Actual:    Tumor
Predicted: Normal

→ False Negative
```

This is also called a:

```text
Type II Error
```

---

# Accuracy

Accuracy measures the proportion of correct predictions.

```text
Accuracy =
TP + TN
────────────────────
TP + TN + FP + FN
```

It can be useful when classes are reasonably balanced.

However, accuracy can be misleading for highly imbalanced datasets.

---

# Precision

Precision answers:

> When the model predicts positive, how often is it correct?

```text
Precision =
TP
────────────
TP + FP
```

High precision means relatively few false positives.

---

# Recall

Recall answers:

> Of all actual positive examples, how many did the model find?

```text
Recall =
TP
────────────
TP + FN
```

High recall means relatively few false negatives.

Recall is also commonly called:

```text
Sensitivity
True Positive Rate
```

in appropriate medical contexts.

---

# F1-Score

F1 combines precision and recall using the harmonic mean.

```text
F1 =
2 × Precision × Recall
───────────────────────
Precision + Recall
```

F1 is useful when both:

```text
False Positives
+
False Negatives
```

matter.

---

# Precision vs Recall

There is often a trade-off.

```text
Higher Threshold
      ↓
Fewer Predictions
      ↓
Potentially Higher Precision
      ↓
Potentially Lower Recall
```

While:

```text
Lower Threshold
      ↓
More Predictions
      ↓
Potentially Higher Recall
      ↓
Potentially Lower Precision
```

The best threshold depends on the application.

---

# Specificity

Specificity measures how well the model identifies negative examples.

```text
Specificity =
TN
────────────
TN + FP
```

In medical applications, sensitivity and specificity are often both important.

---

# ROC Curve

The ROC curve plots:

```text
True Positive Rate
        vs
False Positive Rate
```

across different classification thresholds.

```text
TPR
1.0 |              ______
    |           __/
    |        __/
    |     __/
    |  __/
0.0 +----------------------
     0.0                1.0
          False Positive Rate
```

---

# ROC-AUC

AUC means:

```text
Area Under the Curve
```

ROC-AUC summarizes the model's ability to rank positive examples above negative examples across thresholds.

A value closer to:

```text
1.0
```

generally indicates better ranking performance.

AUC should still be interpreted alongside class distribution and the actual operating point.

---

# Precision-Recall Curve

A Precision-Recall curve shows the trade-off between:

```text
Precision
Recall
```

across thresholds.

PR curves can be especially informative for highly imbalanced classification problems.

---

# PR-AUC

PR-AUC summarizes performance across the precision-recall trade-off.

For rare positive classes, PR-AUC can provide a more informative view than ROC-AUC alone.

---

# Multi-Class Classification

For multiple classes:

```text
Cat
Dog
Horse
Bird
```

a confusion matrix can show where errors occur.

Example:

```text
              Predicted
            Cat Dog Horse

Actual Cat   90  5   5
Actual Dog    7 88   5
Actual Horse  4  6  90
```

This immediately shows which classes are confused with each other.

---

# Macro vs Micro vs Weighted Metrics

For multi-class problems, metrics can be averaged in different ways.

### Macro

Calculate the metric independently for each class and average equally.

```text
All classes get equal importance
```

### Micro

Aggregate contributions across all classes before calculating the metric.

```text
Large classes influence the result more
```

### Weighted

Calculate each class metric and weight it according to class support.

These choices can produce different results, especially for imbalanced datasets.

---

# Per-Class Evaluation

Overall accuracy can hide weak classes.

Example:

```text
Class A → F1 = 0.95
Class B → F1 = 0.91
Class C → F1 = 0.61
```

The overall score may look good while Class C needs significant improvement.

Always inspect per-class performance when possible.

---

# Object Detection Evaluation

Object detection predicts:

```text
Class
+
Bounding Box
+
Confidence
```

Evaluation must therefore measure both:

```text
Classification correctness
+
Localization quality
```

---

# IoU

Intersection over Union measures bounding-box overlap.

```text
IoU =
Intersection Area
─────────────────
Union Area
```

Conceptually:

```text
Ground Truth
┌───────────────┐
│      ┌───────┼───────┐
│      │       │       │
└──────┼───────┘       │
       └───────────────┘
      Prediction
```

Higher IoU means greater overlap.

---

# Detection Matching

A predicted box can be considered a correct detection when it satisfies the evaluation rules for:

```text
Correct Class
+
Required IoU
```

Predictions that do not match ground-truth objects may become false positives.

Ground-truth objects that are not detected become false negatives.

---

# Detection Precision

For object detection:

```text
Precision =
Correct Detections
────────────────────────────
All Positive Predictions
```

False positive boxes reduce precision.

---

# Detection Recall

```text
Recall =
Detected Ground Truth Objects
──────────────────────────────
All Ground Truth Objects
```

Missed objects reduce recall.

---

# mAP

Mean Average Precision is one of the most widely used object detection metrics.

A simplified conceptual process is:

```text
Predictions
    ↓
Confidence Ranking
    ↓
Match with Ground Truth
    ↓
Precision / Recall
    ↓
Average Precision
    ↓
Average Across Classes
    ↓
mAP
```

The exact calculation depends on the evaluation protocol.

---

# mAP@0.5

This metric evaluates detections using an IoU threshold of:

```text
0.5
```

A predicted box with sufficient overlap and the correct class can count as a match under that criterion.

---

# mAP@0.5:0.95

This evaluates performance across multiple IoU thresholds.

Conceptually:

```text
0.50
0.55
0.60
...
0.95
```

The metric is therefore stricter than mAP at a single IoU threshold.

When comparing detectors, always confirm which mAP definition is being reported.

---

# Detection Error Analysis

Inspect:

```text
False Positives
False Negatives
Wrong Classes
Poor Localization
Duplicate Detections
Small Object Failures
Occlusion Failures
```

Example:

```text
Prediction
┌───────────────┐
│               │
│     Object    │
│               │
└───────────────┘

Ground Truth
   ┌──────────┐
   │  Object  │
   └──────────┘
```

If the predicted box is poorly aligned, IoU may be low even when the class is correct.

---

# Segmentation Evaluation

Segmentation predicts a mask rather than a bounding box.

Therefore, evaluation compares:

```text
Predicted Mask
      vs
Ground Truth Mask
```

Common metrics include:

```text
Dice Score
IoU
Pixel Accuracy
Precision
Recall
```

---

# Dice Score

Dice measures overlap between two regions.

```text
Dice =
2 × Intersection
────────────────────────
Prediction + Ground Truth
```

The score generally ranges from:

```text
0 → No overlap
1 → Perfect overlap
```

Dice is especially common in medical segmentation.

---

# Segmentation IoU

```text
IoU =
Intersection
────────────────
Union
```

It is also called:

```text
Jaccard Index
```

Dice and IoU are related but should not be treated as identical metrics.

---

# Pixel Accuracy

Pixel accuracy measures:

```text
Correctly Classified Pixels
────────────────────────────
Total Pixels
```

It can be misleading when the background dominates.

Example:

```text
Background = 99%
Target     = 1%
```

A model predicting background everywhere could achieve approximately 99% pixel accuracy while completely failing the target segmentation.

---

# Medical Image Evaluation

Medical imaging often requires metrics beyond generic accuracy.

Depending on the task, useful metrics can include:

```text
Sensitivity
Specificity
Precision
Recall
Dice
IoU
Hausdorff Distance
Per-class Metrics
```

The appropriate metrics should reflect the clinical or research objective.

---

# Sensitivity vs Specificity

In a medical setting:

```text
Sensitivity
→ Ability to detect actual positive cases

Specificity
→ Ability to correctly identify negative cases
```

There may be a trade-off between them.

The acceptable trade-off depends on the intended use of the model.

---

# Hausdorff Distance

For segmentation boundaries, overlap metrics may not tell the whole story.

Hausdorff Distance measures the maximum distance between points on two sets, depending on the chosen definition.

Conceptually:

```text
Predicted Boundary
       ↕
Maximum Boundary Distance
       ↕
Ground Truth Boundary
```

Boundary-aware metrics can be useful when precise contours matter.

---

# Class Imbalance

Suppose:

```text
Background = 98%
Object     = 2%
```

Accuracy may look excellent even if the model misses the object.

Better analysis may include:

```text
Precision
Recall
F1
Dice
IoU
Per-class metrics
```

Always inspect the actual confusion or segmentation results.

---

# Threshold Selection

Many models produce probabilities rather than final labels.

For example:

```text
0.21
0.42
0.67
0.91
```

A threshold determines the final decision.

```text
Threshold = 0.50

0.21 → Negative
0.42 → Negative
0.67 → Positive
0.91 → Positive
```

The threshold should be selected using validation data rather than tuned on the final test set.

---

# Calibration

A model can be accurate but poorly calibrated.

Calibration asks whether predicted probabilities correspond reasonably well to actual frequencies.

For example:

```text
Predicted confidence ≈ 0.80
```

Ideally, examples receiving that confidence should be correct roughly 80% of the time under the relevant calibration definition.

Calibration can matter when confidence scores are used for decisions or downstream ranking.

---

# Overfitting

A model may perform extremely well on training data but poorly on unseen data.

Example:

```text
Train F1       = 0.98
Validation F1  = 0.74
```

Possible causes:

```text
Small dataset
Too much model capacity
Insufficient augmentation
Data leakage assumptions
Noisy validation distribution
```

---

# Underfitting

Underfitting occurs when the model performs poorly even on training data.

Example:

```text
Train F1       = 0.58
Validation F1  = 0.55
```

Possible causes include:

```text
Model too simple
Insufficient training
Poor features
Bad preprocessing
Learning rate problems
Noisy labels
```

---

# Cross-Validation

Cross-validation repeatedly splits the dataset to estimate performance.

For K-fold cross-validation:

```text
Fold 1 → Validation
Fold 2 → Validation
Fold 3 → Validation
...
Fold K → Validation
```

Each fold is used as validation once.

The results can then be aggregated.

---

# Stratified Cross-Validation

For classification, stratified splitting attempts to preserve class proportions across folds.

Example:

```text
Dataset
70% Class A
30% Class B
```

Each fold attempts to maintain a similar distribution.

This is particularly useful for imbalanced classification datasets.

---

# Patient-Level Splitting

For medical imaging, random image-level splitting can cause leakage when multiple images belong to the same patient.

Prefer:

```text
PATIENT 001 → TRAIN
PATIENT 002 → TRAIN
PATIENT 003 → VALIDATION
PATIENT 004 → TEST
```

rather than:

```text
Patient 001 Slice 1 → TRAIN
Patient 001 Slice 2 → TEST
```

when the task requires independent patient-level evaluation.

---

# Model Comparison

When comparing models, use the same:

```text
Dataset
Test Set
Preprocessing
Evaluation Protocol
Metrics
```

Example:

```text
Model A
Accuracy = 0.91

Model B
Accuracy = 0.93
```

This comparison is meaningful only if both were evaluated under the same conditions.

---

# Confidence Intervals

A single metric is only an estimate of performance.

Where appropriate, uncertainty can be reported using:

```text
Confidence Intervals
Bootstrap Estimates
Repeated Cross-Validation
```

For research-oriented work, reporting uncertainty can make comparisons more informative.

---

# Statistical Significance

A small difference between models may not necessarily represent a meaningful improvement.

For example:

```text
Model A Dice = 0.842
Model B Dice = 0.847
```

The difference may be small relative to evaluation uncertainty.

Statistical testing should match the experimental design and data structure.

---

# Evaluation Workflow

A robust evaluation workflow is:

```text
TRAINED MODEL
      ↓
UNSEEN TEST DATA
      ↓
GENERATE PREDICTIONS
      ↓
CALCULATE METRICS
      ↓
PER-CLASS ANALYSIS
      ↓
CONFUSION / ERROR ANALYSIS
      ↓
VISUAL INSPECTION
      ↓
COMPARE WITH BASELINE
      ↓
REPORT RESULTS
```

---

# Classification Evaluation Example

A simple scikit-learn example:

```python
from sklearn.metrics import (
    accuracy_score,
    precision_score,
    recall_score,
    f1_score
)

accuracy = accuracy_score(
    y_true,
    y_pred
)

precision = precision_score(
    y_true,
    y_pred
)

recall = recall_score(
    y_true,
    y_pred
)

f1 = f1_score(
    y_true,
    y_pred
)

print("Accuracy:", accuracy)
print("Precision:", precision)
print("Recall:", recall)
print("F1:", f1)
```

For multi-class problems, configure the appropriate averaging strategy.

---

# Confusion Matrix Example

```python
from sklearn.metrics import confusion_matrix

cm = confusion_matrix(
    y_true,
    y_pred
)

print(cm)
```

A visualization can then be created to inspect class-level errors.

---

# Classification Report

A convenient summary is:

```python
from sklearn.metrics import classification_report

print(
    classification_report(
        y_true,
        y_pred
    )
)
```

This can report metrics such as:

```text
Precision
Recall
F1-score
Support
```

---

# Segmentation Evaluation Example

A simplified Dice implementation:

```python
def dice_score(pred, target, smooth=1e-6):

    intersection = (
        pred * target
    ).sum()

    return (
        2.0 * intersection + smooth
    ) / (
        pred.sum() +
        target.sum() +
        smooth
    )
```

The exact implementation should account for tensor dimensions, thresholding, and multi-class behavior.

---

# IoU Example

```python
def iou_score(pred, target, smooth=1e-6):

    intersection = (
        pred * target
    ).sum()

    union = (
        pred + target - intersection
    ).sum()

    return (
        intersection + smooth
    ) / (
        union + smooth
    )
```

Again, production implementations should handle batch and class dimensions carefully.

---

# Object Detection Evaluation

For YOLO-style detectors, evaluation is typically performed using the framework's validation tools.

The output may include:

```text
Precision
Recall
mAP50
mAP50-95
Per-class metrics
```

Always verify the exact metric definitions used by the installed version.

---

# Error Analysis Matrix

A useful evaluation table is:

| Error Type | Possible Cause | Action |
|---|---|---|
| False Positive | Background confusion | Add representative negatives |
| False Negative | Weak features | Add examples / tune threshold |
| Wrong Class | Similar classes | Improve labels and data |
| Poor Box | Localization issue | Inspect annotations |
| Poor Mask | Boundary issue | Improve masks / loss |
| Small Object Failure | Low resolution | Increase input resolution |
| Domain Failure | Distribution shift | Add representative data |

---

# Evaluation by Task

| Task | Primary Metrics |
|---|---|
| Classification | Accuracy, Precision, Recall, F1 |
| Imbalanced Classification | F1, PR-AUC, Recall |
| Object Detection | Precision, Recall, IoU, mAP |
| Semantic Segmentation | Dice, IoU, Pixel Accuracy |
| Medical Segmentation | Dice, IoU, Sensitivity, Specificity |
| Boundary-sensitive Segmentation | Dice, IoU, Boundary Metrics |

The metric selection should always follow the actual problem.

---

# What a Good Evaluation Report Contains

A useful computer vision evaluation report should include:

```text
Dataset
Model
Training Configuration
Evaluation Split
Preprocessing
Metrics
Overall Results
Per-Class Results
Confusion Matrix
Qualitative Examples
Failure Cases
Limitations
```

For research projects, also document:

```text
Random Seeds
Hardware
Software Versions
Model Checkpoint
Hyperparameters
```

---

# Complete Evaluation Architecture

```text
                        COMPUTER VISION MODEL
                                  │
                                  ▼
                             TEST DATA
                                  │
                                  ▼
                             PREDICTIONS
                                  │
                ┌─────────────────┼─────────────────┐
                ▼                 ▼                 ▼
          CLASSIFICATION       DETECTION       SEGMENTATION
                │                 │                 │
                ▼                 ▼                 ▼
          Accuracy/F1        IoU / mAP        Dice / IoU
                │                 │                 │
                └─────────────────┼─────────────────┘
                                  ▼
                           ERROR ANALYSIS
                                  │
                       ┌──────────┼──────────┐
                       ▼          ▼          ▼
                    Per-Class  Visual      Failure
                    Metrics    Review       Cases
                       │          │          │
                       └──────────┼──────────┘
                                  ▼
                           FINAL REPORT
```

---

# Practical Evaluation Checklist

## Dataset

```text
□ Test set is separated
□ No data leakage
□ Class distribution checked
□ Patient-level split where appropriate
□ Test set not used for tuning
```

## Classification

```text
□ Accuracy
□ Precision
□ Recall
□ F1
□ Confusion Matrix
□ ROC-AUC where appropriate
□ PR-AUC for imbalanced problems
```

## Detection

```text
□ IoU
□ Precision
□ Recall
□ mAP@0.5
□ mAP@0.5:0.95
□ Per-class performance
□ False positive analysis
□ False negative analysis
```

## Segmentation

```text
□ Dice
□ IoU
□ Pixel accuracy where appropriate
□ Precision
□ Recall
□ Boundary analysis where relevant
□ Visual mask inspection
```

## Final Analysis

```text
□ Compare against baseline
□ Inspect failure cases
□ Report limitations
□ Report uncertainty where appropriate
□ Document evaluation protocol
```

---

# Key Takeaways

```text
✓ Evaluation must use unseen data

✓ Accuracy alone is not enough

✓ Precision measures false-positive control

✓ Recall measures how many positives are found

✓ F1 balances precision and recall

✓ Confusion matrices reveal class-level errors

✓ IoU measures spatial overlap

✓ mAP is widely used for object detection

✓ Dice and IoU are important for segmentation

✓ Class imbalance can make accuracy misleading

✓ Thresholds should be selected using validation data

✓ Patient-level splitting can be essential for medical imaging

✓ Visual error analysis is as important as numerical metrics

✓ Model comparisons require consistent evaluation protocols

✓ A strong evaluation report includes both metrics and failure analysis
```

---

# Related Experiments

- [Computer Vision Fundamentals →](/labs/computer-vision/fundamentals/)
- [Image Classification →](/labs/computer-vision/image-classification/)
- [Object Detection →](/labs/computer-vision/object-detection/)
- [OpenCV & Image Processing →](/labs/computer-vision/opencv/)
- [Image Segmentation →](/labs/computer-vision/image-segmentation/)
- [YOLO Object Detection →](/labs/computer-vision/yolo/)
- [U-Net & Medical Image Segmentation →](/labs/computer-vision/u-net/)
- [Computer Vision Deployment →](/labs/computer-vision/deployment/)

---

> **The metric tells you what happened. Error analysis helps you understand why.**
