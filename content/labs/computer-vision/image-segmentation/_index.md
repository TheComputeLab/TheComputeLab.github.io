---
title: "Image Segmentation"
description: "Understanding pixel-level image segmentation, masks, U-Net, Dice Score, IoU, medical imaging, training, inference, and deployment."
weight: 40
toc: true
---

> **Classification tells us what is present. Detection tells us where it is. Segmentation tells us which pixels belong to it.**

Image segmentation assigns a class or region to individual pixels in an image.

A simplified segmentation pipeline is:

```text
IMAGE
  ↓
PREPROCESSING
  ↓
SEGMENTATION MODEL
  ↓
PIXEL-LEVEL PREDICTION
  ↓
MASK
  ↓
POST-PROCESSING
  ↓
FINAL SEGMENTATION
```

---

# What is Image Segmentation?

Consider an image containing a tumor.

Classification might produce:

```text
Prediction: Tumor
```

Object detection might produce:

```text
Tumor
Bounding Box
```

Segmentation produces:

```text
Tumor
Pixel-level Mask
```

Conceptually:

```text
INPUT IMAGE
     │
     ▼
┌─────────────────────┐
│                     │
│       TUMOR         │
│      ███████        │
│     █████████       │
│      ███████        │
│                     │
└─────────────────────┘
            │
            ▼
      PIXEL MASK
```

This makes segmentation particularly useful when the exact shape and boundaries of an object matter.

---

# Classification vs Detection vs Segmentation

```text
CLASSIFICATION
Image → Class

DETECTION
Image → Class + Bounding Box

SEGMENTATION
Image → Class + Pixel-level Mask
```

The amount of spatial information increases as we move from classification to segmentation.

---

# What is a Segmentation Mask?

A segmentation mask is an image-like structure in which pixel values represent class membership.

For binary segmentation:

```text
0 → Background
1 → Object
```

Conceptually:

```text
IMAGE

. . . . . . . . .
. . . X X X . . .
. . X X X X X . .
. . . X X X . . .
. . . . . . . . .

MASK

0 0 0 0 0 0 0 0
0 0 0 1 1 1 0 0
0 0 1 1 1 1 1 0
0 0 0 1 1 1 0 0
0 0 0 0 0 0 0 0
```

The mask identifies the pixels belonging to the target object.

---

# Binary Segmentation

Binary segmentation contains two categories:

```text
Background
Target
```

For example:

```text
Background → 0
Tumor      → 1
```

A binary segmentation model can therefore produce a single-channel mask.

---

# Multi-Class Segmentation

Some problems contain multiple semantic classes.

For example:

```text
0 → Background
1 → Tumor
2 → Edema
3 → Organ
```

The output can contain multiple classes at each pixel.

---

# Semantic Segmentation

Semantic segmentation assigns a class to every pixel.

For example:

```text
Person → Person
Car    → Car
Road   → Road
Sky    → Sky
```

Objects belonging to the same class are treated as the same semantic category.

---

# Instance Segmentation

Instance segmentation separates individual objects.

For example:

```text
Person #1
Person #2
Person #3
```

Even though all three belong to the same class, they receive separate masks.

Conceptually:

```text
Semantic Segmentation:

Person Person Person

Instance Segmentation:

Person #1
Person #2
Person #3
```

---

# Panoptic Segmentation

Panoptic segmentation combines semantic and instance-level reasoning.

It attempts to represent:

```text
Stuff
+
Things
```

For example:

```text
Road       → semantic region
Sky        → semantic region
Car #1     → instance
Car #2     → instance
Person #1  → instance
```

---

# Segmentation Dataset

A segmentation dataset generally contains:

```text
dataset/
│
├── images/
│   ├── image001.png
│   ├── image002.png
│   └── image003.png
│
└── masks/
    ├── image001.png
    ├── image002.png
    └── image003.png
```

Each image should have a corresponding mask.

For example:

```text
images/image001.png
masks/image001.png
```

The pairing must remain correct throughout preprocessing and training.

---

# Image and Mask Alignment

One of the most important requirements is alignment.

```text
IMAGE
┌─────────────────┐
│                 │
│      OBJECT     │
│                 │
└─────────────────┘

MASK
┌─────────────────┐
│                 │
│      OBJECT     │
│                 │
└─────────────────┘
```

If the image and mask are shifted relative to each other, the model receives incorrect supervision.

---

# Mask Values

Mask encoding depends on the problem.

For binary segmentation:

```text
0 → Background
1 → Target
```

Some datasets use:

```text
0 → Background
255 → Target
```

Before training, the mask representation should be inspected and converted into the format expected by the loss function.

---

# Preprocessing

A typical segmentation preprocessing pipeline is:

```text
IMAGE
  ↓
Resize
  ↓
Normalize
  ↓
Augmentation
  ↓
MODEL

MASK
  ↓
Resize using appropriate interpolation
  ↓
Convert labels
  ↓
Augmentation
  ↓
MODEL TARGET
```

A critical detail is that masks should not generally be resized using ordinary image interpolation because interpolation can create invalid class values.

Nearest-neighbor interpolation is commonly used for categorical masks.

---

# Data Augmentation

Segmentation augmentation must transform both:

```text
IMAGE
+
MASK
```

using the same spatial transformation.

For example:

```text
Original Image
      │
      ├──────────────┐
      │              │
      ▼              ▼
   Rotate           Mask
      │              │
      └──────┬───────┘
             ▼
       Augmented Pair
```

If the image is rotated but the mask is not, the training target becomes incorrect.

---

# U-Net

U-Net is one of the most widely recognized architectures for image segmentation.

It was originally designed for biomedical image segmentation.

Its architecture resembles:

```text
             ENCODER
                │
        ┌───────┴───────┐
        ▼               │
     Feature            │
     Extraction         │
        │               │
        ▼               │
      BOTTOM             │
        │                │
        ▼                │
             DECODER     │
        │                │
        ▼                │
      MASK               │
        ▲                │
        │                │
   SKIP CONNECTIONS ─────┘
```

---

# U-Net Architecture

A simplified U-Net looks like:

```text
INPUT
  │
  ▼
┌───────────┐
│ Encoder 1 │──────────────┐
└───────────┘              │
      │                    │
      ▼                    │
┌───────────┐              │
│ Encoder 2 │──────────┐   │
└───────────┘          │   │
      │                │   │
      ▼                │   │
┌───────────┐          │   │
│ Bottleneck│          │   │
└───────────┘          │   │
      │                │   │
      ▼                │   │
┌───────────┐          │   │
│ Decoder 2 │◄─────────┘   │
└───────────┘              │
      │                    │
      ▼                    │
┌───────────┐              │
│ Decoder 1 │◄─────────────┘
└───────────┘
      │
      ▼
   OUTPUT MASK
```

---

# Encoder

The encoder extracts increasingly abstract features.

Conceptually:

```text
Image
 ↓
Edges
 ↓
Textures
 ↓
Shapes
 ↓
High-level features
```

Spatial resolution generally decreases while feature depth increases.

---

# Bottleneck

The bottleneck is the deepest part of the network.

It contains a compressed representation of the input.

```text
High Resolution
      ↓
Lower Resolution
      ↓
Bottleneck
```

The decoder then reconstructs the segmentation map.

---

# Decoder

The decoder progressively increases spatial resolution.

```text
Bottleneck
    ↓
Upsampling
    ↓
Feature Fusion
    ↓
Upsampling
    ↓
Segmentation Mask
```

The objective is to recover detailed spatial information.

---

# Skip Connections

One of U-Net's important ideas is the use of skip connections.

Features from the encoder are passed directly to corresponding decoder stages.

```text
Encoder Feature
      │
      ├──────────────────► Decoder
      │
      ▼
Compressed Representation
      │
      ▼
Decoder
```

These connections help preserve fine-grained spatial information.

This is particularly useful when precise boundaries matter.

---

# Why Segmentation is Difficult

Segmentation requires the model to predict many pixels correctly.

A small boundary error can affect a large number of pixels.

Common challenges include:

```text
Small objects
Irregular shapes
Low contrast
Noise
Occlusion
Class imbalance
Poor annotations
Ambiguous boundaries
```

Medical images can introduce additional challenges.

---

# Medical Image Segmentation

Segmentation is widely used in medical imaging.

Examples include:

```text
Brain Tumor
Lung
Liver
Kidney
Blood Vessels
Organs
Lesions
Cells
```

A medical segmentation workflow may look like:

```text
MEDICAL IMAGE
      ↓
PREPROCESSING
      ↓
SEGMENTATION MODEL
      ↓
PREDICTED MASK
      ↓
POST-PROCESSING
      ↓
ANALYSIS
```

---

# Brain Tumor Segmentation

A practical example is brain tumor segmentation from MRI data.

Conceptually:

```text
MRI
 ↓
Preprocessing
 ↓
U-Net
 ↓
Tumor Segmentation
 ↓
Mask
```

The model learns to distinguish tumor-related regions from surrounding tissue.

For multi-region datasets, the output may contain multiple tumor-related classes rather than a single binary tumor mask.

---

# BraTS-Style Workflow

A typical brain tumor segmentation workflow may involve:

```text
MRI DATA
   ↓
MODALITY HANDLING
   ↓
PREPROCESSING
   ↓
LABEL / MASK PREPARATION
   ↓
TRAIN / VALIDATION SPLIT
   ↓
U-NET
   ↓
SEGMENTATION
   ↓
DICE / IoU
   ↓
ERROR ANALYSIS
```

The exact preprocessing depends on the dataset and model design.

---

# Loss Functions

Segmentation often requires loss functions designed for pixel-level predictions.

Common approaches include:

```text
Binary Cross-Entropy
Categorical Cross-Entropy
Dice Loss
Focal Loss
Combined Losses
```

A combined loss can sometimes help when foreground regions are small compared with the background.

---

# Dice Score

Dice Score measures overlap between the predicted mask and ground-truth mask.

The formula is:

```text
Dice =
2 × |Prediction ∩ Ground Truth|
────────────────────────────────
|Prediction| + |Ground Truth|
```

The value generally ranges from:

```text
0 → No overlap
1 → Perfect overlap
```

A higher Dice Score indicates better agreement between the predicted and target regions.

---

# Intersection over Union

IoU is another important segmentation metric.

```text
IoU =
Area of Intersection
────────────────────
Area of Union
```

For segmentation:

```text
Prediction Mask
       ∩
Ground Truth Mask
```

is compared with their union.

---

# Dice vs IoU

Both measure overlap, but they use different formulas.

```text
Dice:
2 × Intersection
──────────────────────
Prediction + Ground Truth

IoU:
Intersection
──────────────────────
Union
```

Both can be useful when evaluating segmentation quality.

---

# Pixel Accuracy

Pixel accuracy measures how many pixels are classified correctly.

```text
Correct Pixels
───────────────
Total Pixels
```

However, pixel accuracy can be misleading when the background dominates the image.

For example:

```text
Background = 95%
Tumor      = 5%
```

A model predicting mostly background can achieve high pixel accuracy while producing a poor tumor mask.

---

# Class Imbalance

Segmentation often has severe class imbalance.

For example:

```text
Background → 95%
Object     → 5%
```

This can cause the model to favor the background.

Possible approaches include:

```text
Dice Loss
Weighted Loss
Focal Loss
Balanced Sampling
Better Dataset Design
```

The appropriate strategy depends on the task.

---

# Training Pipeline

A typical segmentation training pipeline is:

```text
IMAGE + MASK
      ↓
PREPROCESSING
      ↓
AUGMENTATION
      ↓
BATCH
      ↓
U-NET
      ↓
PREDICTED MASK
      ↓
LOSS
      ↓
BACKPROPAGATION
      ↓
WEIGHT UPDATE
```

This repeats over many training iterations.

---

# Validation

During training, validation data can be used to monitor generalization.

Useful metrics may include:

```text
Validation Loss
Dice Score
IoU
Precision
Recall
```

A model should not be judged solely by training loss.

---

# Overfitting in Segmentation

Example:

```text
Training Dice     0.97
Validation Dice  0.74
```

This can indicate overfitting.

Possible strategies include:

```text
More training data
Augmentation
Regularization
Dropout
Early stopping
Transfer learning
Model simplification
```

---

# Segmentation Error Analysis

When a model produces a poor mask, inspect:

```text
Input Image
Ground Truth Mask
Predicted Mask
Difference
```

A useful visualization is:

```text
┌────────────┬────────────┬────────────┐
│ Input      │ Ground     │ Prediction │
│ Image      │ Truth      │            │
└────────────┴────────────┴────────────┘
```

This can reveal whether the model:

```text
Missed the object
Detected too much
Detected too little
Produced fragmented regions
Produced incorrect boundaries
```

---

# Common Segmentation Failure Modes

### Empty Prediction

The model predicts only background.

Possible causes:

```text
Class imbalance
Incorrect labels
Loss configuration
Threshold issue
Training instability
```

### Over-Segmentation

The model predicts too large a region.

Possible causes:

```text
Poor boundaries
Noisy training data
Weak localization
Threshold settings
```

### Under-Segmentation

The predicted region is too small.

Possible causes:

```text
Weak features
Insufficient training
Low contrast
Poor annotation
```

### Fragmented Mask

The target appears as multiple disconnected regions.

Possible causes:

```text
Noise
Insufficient context
Poor training examples
Post-processing issues
```

---

# Thresholding

For binary segmentation, a model may produce a probability map:

```text
0.01
0.12
0.73
0.91
```

A threshold can convert probabilities into a binary mask.

For example:

```text
threshold = 0.5
```

Then:

```text
0.01 → 0
0.12 → 0
0.73 → 1
0.91 → 1
```

The optimal threshold may depend on the dataset and evaluation objective.

---

# Post-Processing

Predicted masks can sometimes be refined after inference.

Possible operations include:

```text
Thresholding
Morphological Operations
Connected Components
Small Region Removal
Hole Filling
```

Post-processing should be validated carefully because it can improve or harm performance.

---

# Inference

A typical segmentation inference pipeline is:

```text
NEW IMAGE
    ↓
SAME PREPROCESSING
    ↓
TRAINED MODEL
    ↓
PROBABILITY MAP
    ↓
THRESHOLD / CLASS SELECTION
    ↓
SEGMENTATION MASK
    ↓
VISUALIZATION
```

The inference preprocessing must remain consistent with the training pipeline.

---

# Example Conceptual Inference

```python
prediction = model.predict(image)

mask = prediction[0]

binary_mask = (mask > 0.5).astype("uint8")
```

The exact implementation depends on whether the model performs binary or multi-class segmentation.

---

# Visualization

A useful segmentation visualization overlays the predicted mask on the original image.

Conceptually:

```text
Original Image
      +
Predicted Mask
      ↓
Overlay
```

This makes it easier to inspect:

```text
Boundaries
False Positives
False Negatives
Missing Regions
```

---

# Segmentation Deployment

A segmentation model can be deployed through:

```text
Streamlit
FastAPI
Docker
Cloud
Desktop Application
Edge Device
```

A simple application pipeline is:

```text
USER
 ↓
UPLOAD IMAGE
 ↓
API / APPLICATION
 ↓
PREPROCESS
 ↓
U-NET
 ↓
MASK
 ↓
OVERLAY
 ↓
RESULT
```

---

# Complete Segmentation Architecture

```text
                      IMAGE SEGMENTATION
                              │
                              ▼
                           DATASET
                              │
                       ┌──────┴──────┐
                       ▼             ▼
                     IMAGE          MASK
                       │             │
                       └──────┬──────┘
                              ▼
                         PREPROCESSING
                              │
                              ▼
                         AUGMENTATION
                              │
                              ▼
                            U-NET
                              │
                              ▼
                       PREDICTED MASK
                              │
                  ┌───────────┴───────────┐
                  ▼                       ▼
             POST-PROCESSING          METRICS
                  │                 Dice / IoU
                  │                     │
                  └──────────┬──────────┘
                             ▼
                         DEPLOYMENT
```

---

# Practical Checklist

## Before Training

```text
□ Inspect images
□ Inspect masks
□ Verify image-mask pairing
□ Check mask values
□ Check image dimensions
□ Check class distribution
□ Split train / validation / test
□ Define preprocessing
□ Define augmentation
```

## During Training

```text
□ Monitor training loss
□ Monitor validation loss
□ Monitor Dice / IoU
□ Watch for overfitting
□ Save best checkpoint
□ Visualize sample predictions
```

## After Training

```text
□ Evaluate on unseen data
□ Calculate Dice
□ Calculate IoU
□ Inspect predicted masks
□ Inspect false positives
□ Inspect false negatives
□ Test different thresholds
```

## Before Deployment

```text
□ Verify preprocessing
□ Verify mask encoding
□ Measure inference latency
□ Test real-world inputs
□ Test memory requirements
□ Package model and dependencies
```

---

# Practical Project Connection

The segmentation concepts here connect directly to practical projects involving:

```text
Medical Imaging
Brain Tumor Segmentation
MRI Analysis
U-Net
Pixel-level Masks
Dice Score
IoU
```

The important engineering lesson is:

```text
MODEL
  ≠
COMPLETE SYSTEM
```

A successful segmentation application also requires:

```text
Dataset
+
Correct Masks
+
Preprocessing
+
Training
+
Evaluation
+
Inference
+
Visualization
+
Deployment
```

---

# Key Takeaways

```text
✓ Segmentation predicts pixel-level regions

✓ Binary segmentation separates foreground and background

✓ Multi-class segmentation predicts multiple classes

✓ Semantic segmentation labels pixels by class

✓ Instance segmentation separates individual objects

✓ U-Net is a widely used segmentation architecture

✓ Skip connections help preserve spatial information

✓ Dice Score and IoU measure mask overlap

✓ Pixel accuracy can be misleading with class imbalance

✓ Image and mask transformations must remain aligned

✓ Medical imaging is an important segmentation application

✓ Error analysis is essential for understanding mask failures

✓ Deployment requires consistent preprocessing and inference
```

---

# Related Experiments

- [Computer Vision Fundamentals →](/labs/computer-vision/fundamentals/)
- [Image Classification →](/labs/computer-vision/image-classification/)
- [Object Detection →](/labs/computer-vision/object-detection/)
- [U-Net Experiments →](/labs/computer-vision/u-net/)
- [Brain Tumor Segmentation →](/labs/computer-vision/brain-tumor-segmentation/)
- [Medical Image Analysis →](/labs/computer-vision/medical-imaging/)
- [Model Evaluation →](/labs/computer-vision/model-evaluation/)
- [Computer Vision Deployment →](/labs/computer-vision/deployment/)

---

> **Segmentation turns an image into a map — showing exactly which pixels matter.**
