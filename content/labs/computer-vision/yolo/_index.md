---
title: "YOLO Object Detection"
description: "Practical YOLO object detection covering datasets, annotations, training, inference, evaluation, GPU acceleration, and deployment."
weight: 60
toc: true
---

> **YOLO turns an image into a set of object predictions — fast enough to power real-world computer vision systems.**

YOLO, or **You Only Look Once**, is a family of object detection models designed to detect multiple objects in an image efficiently.

A practical YOLO workflow looks like:

```text
IMAGE
  ↓
PREPROCESSING
  ↓
YOLO MODEL
  ↓
RAW PREDICTIONS
  ↓
CONFIDENCE FILTER
  ↓
NMS
  ↓
FINAL DETECTIONS
```

---

# What is YOLO?

YOLO is an object detection approach that predicts object locations and classes from an image.

A detection can contain:

```text
Class
Confidence
Bounding Box
```

For example:

```text
Person
Confidence: 0.94
Box: [120, 80, 340, 520]
```

Unlike image classification, a YOLO detector can identify multiple objects in the same image.

---

# Classification vs YOLO

Classification:

```text
IMAGE
  ↓
MODEL
  ↓
DOG
```

YOLO:

```text
IMAGE
  ↓
MODEL
  ↓
DOG      + Box + Confidence
PERSON   + Box + Confidence
CAR      + Box + Confidence
```

This makes YOLO useful for images containing multiple objects.

---

# YOLO Detection Pipeline

A simplified pipeline is:

```text
INPUT IMAGE
     ↓
RESIZE / PREPROCESS
     ↓
YOLO NETWORK
     ↓
FEATURE EXTRACTION
     ↓
OBJECT PREDICTIONS
     ↓
CONFIDENCE FILTER
     ↓
NON-MAXIMUM SUPPRESSION
     ↓
FINAL BOXES
```

The exact internal architecture varies between YOLO generations and implementations.

---

# YOLO Model Components

A modern object detector can be thought of as several logical stages:

```text
BACKBONE
   ↓
Extract visual features

NECK
   ↓
Combine features at different scales

HEAD
   ↓
Predict objects
```

The names and exact implementation vary by model version.

The important concept is:

```text
Image
 ↓
Features
 ↓
Multi-scale information
 ↓
Object predictions
```

---

# Multi-Scale Detection

Objects can appear at different sizes.

For example:

```text
Large Object
████████████████

Medium Object
████████

Small Object
██
```

A good detector needs to handle these different scales.

Modern detection architectures use feature representations at different resolutions to improve detection across object sizes.

---

# YOLO Model Sizes

YOLO implementations commonly provide multiple model sizes.

Typical naming may include variants such as:

```text
Nano
Small
Medium
Large
Extra Large
```

The trade-off is generally:

```text
Smaller Model
    ↓
Faster / Lower Resource Usage

Larger Model
    ↓
Potentially Higher Accuracy / More Compute
```

The best model depends on the application.

---

# Pretrained YOLO Models

Pretrained models are usually trained on large datasets.

They provide a useful starting point for:

```text
Inference
Transfer Learning
Fine-Tuning
Custom Detection
```

For experimentation, starting with a pretrained model is often much easier than training from random initialization.

---

# Installing a YOLO Python Package

One commonly used modern Python interface is provided by Ultralytics.

Installation:

```bash
pip install ultralytics
```

Then:

```python
from ultralytics import YOLO
```

Always check the documentation for the exact package and model version used by your project.

---

# Loading a Model

A simplified example:

```python
from ultralytics import YOLO

model = YOLO("yolo_model.pt")
```

A pretrained checkpoint can then be used for inference.

The exact checkpoint name depends on the YOLO version being used.

---

# Basic Inference

A simple image inference workflow:

```python
from ultralytics import YOLO

model = YOLO("yolo_model.pt")

results = model("image.jpg")

for result in results:
    print(result.boxes)
```

The returned results can contain:

```text
Bounding Boxes
Class IDs
Confidence Scores
```

---

# Reading Detection Results

Conceptually:

```python
for result in results:
    for box in result.boxes:
        print(box.xyxy)
        print(box.conf)
        print(box.cls)
```

This can provide:

```text
Coordinates
Confidence
Class
```

The exact result API depends on the installed YOLO implementation.

---

# Confidence Threshold

A detector can produce many candidate predictions.

A confidence threshold controls which predictions are retained.

For example:

```text
Threshold = 0.50
```

Predictions:

```text
0.95 → Keep
0.82 → Keep
0.49 → Remove
0.21 → Remove
```

A high threshold may improve precision but reduce recall.

A low threshold may increase recall but also increase false positives.

---

# Non-Maximum Suppression

A detector can produce several overlapping boxes for the same object.

```text
      ┌─────────────┐
     ┌───────────────┐
     │    OBJECT     │
     └───────────────┘
       └────────────┘
```

NMS keeps the strongest prediction and removes redundant boxes.

Conceptually:

```text
Candidate Boxes
      ↓
Calculate Overlap
      ↓
Select Highest Confidence
      ↓
Remove Strongly Overlapping Boxes
      ↓
Final Detection
```

---

# IoU

Intersection over Union measures the overlap between two boxes.

```text
IoU =
Intersection Area
─────────────────
Union Area
```

The value ranges approximately from:

```text
0 → No overlap
1 → Perfect overlap
```

IoU is important for:

```text
Evaluation
NMS
Localization Quality
```

---

# YOLO Dataset

A custom YOLO dataset commonly looks like:

```text
dataset/
│
├── images/
│   ├── train/
│   ├── val/
│   └── test/
│
├── labels/
│   ├── train/
│   ├── val/
│   └── test/
│
└── data.yaml
```

For every image:

```text
images/train/image001.jpg
```

there should normally be a corresponding label:

```text
labels/train/image001.txt
```

---

# YOLO Label Format

A common YOLO annotation is:

```text
class_id x_center y_center width height
```

Example:

```text
0 0.50 0.45 0.30 0.40
```

The coordinates are generally normalized relative to the image dimensions.

Important:

```text
class_id
x_center
y_center
width
height
```

The exact annotation specification should match the YOLO implementation being used.

---

# Multiple Objects

An image can contain multiple annotations.

Example:

```text
0 0.20 0.40 0.20 0.30
1 0.70 0.55 0.25 0.35
0 0.45 0.20 0.15 0.20
```

Each line represents one object.

---

# data.yaml

A YOLO dataset configuration commonly contains information such as:

```yaml
path: /path/to/dataset

train: images/train
val: images/val

names:
  0: person
  1: car
```

The exact fields can vary between implementations and versions.

The important information is:

```text
Dataset location
Training images
Validation images
Class names
```

---

# Creating a Custom Dataset

A practical custom-detection workflow is:

```text
COLLECT IMAGES
      ↓
DEFINE CLASSES
      ↓
ANNOTATE OBJECTS
      ↓
CHECK LABELS
      ↓
SPLIT DATASET
      ↓
CREATE CONFIGURATION
      ↓
TRAIN
```

---

# Annotation Tools

Common annotation workflows use tools that allow you to draw bounding boxes.

The process is:

```text
IMAGE
  ↓
DRAW BOX
  ↓
SELECT CLASS
  ↓
SAVE ANNOTATION
```

Before training, inspect several annotations manually.

---

# Annotation Quality

Bad labels can produce bad models.

Check for:

```text
Missing objects
Wrong classes
Incorrect boxes
Boxes too large
Boxes too small
Duplicate boxes
Incorrect filenames
```

A few minutes of annotation review can save hours of training time.

---

# Train / Validation / Test

A dataset can be separated into:

```text
TRAIN
  ↓
Learn model parameters

VALIDATION
  ↓
Monitor development

TEST
  ↓
Final evaluation
```

For example:

```text
70% Train
15% Validation
15% Test
```

The exact split should depend on the dataset.

---

# Training a Custom YOLO Model

A simplified Ultralytics-style workflow can look like:

```python
from ultralytics import YOLO

model = YOLO("pretrained_model.pt")

model.train(
    data="data.yaml",
    epochs=50,
    imgsz=640
)
```

The exact model name and training arguments depend on the installed version.

---

# Epochs

An epoch represents one complete pass through the training dataset.

Example:

```text
Epoch 1
Epoch 2
Epoch 3
...
Epoch 50
```

More epochs do not automatically produce a better detector.

Monitor validation performance.

---

# Image Size

The training image size affects:

```text
Accuracy
Small-object detection
GPU memory
Training speed
Inference speed
```

For example:

```text
640 × 640
```

may be a common starting point, but the appropriate size depends on the problem.

---

# Batch Size

Batch size controls how many images are processed together.

For example:

```text
Batch = 16
```

means approximately 16 images are processed per training step.

Larger batches generally require more GPU memory.

---

# GPU Training

YOLO training can benefit substantially from GPU acceleration.

Typical architecture:

```text
Python
  ↓
YOLO / PyTorch
  ↓
CUDA
  ↓
NVIDIA Driver
  ↓
GPU
```

This connects YOLO directly to the infrastructure covered in the Local AI Lab.

---

# Checking GPU Availability

With PyTorch:

```python
import torch

print(torch.cuda.is_available())

if torch.cuda.is_available():
    print(torch.cuda.get_device_name(0))
```

Expected output may indicate:

```text
True
NVIDIA GPU name
```

If CUDA is unavailable, the application may fall back to CPU depending on the configuration.

---

# Training on CPU vs GPU

CPU:

```text
Lower cost
Usually slower for deep learning
```

GPU:

```text
Higher compute throughput
Usually preferred for training
Requires compatible drivers/runtime
```

For larger datasets and models, GPU acceleration can dramatically reduce training time.

---

# Monitoring Training

Important values to monitor include:

```text
Training Loss
Validation Loss
Precision
Recall
mAP
```

Also inspect actual predictions.

A metric can look good while specific classes still perform poorly.

---

# mAP

Mean Average Precision is a common object detection metric.

You may encounter:

```text
mAP@0.5
mAP@0.5:0.95
```

These represent different evaluation protocols.

Always understand which metric is being reported before comparing models.

---

# Precision and Recall

Precision:

```text
True Positives
────────────────────────
True Positives + False Positives
```

Recall:

```text
True Positives
────────────────────────
True Positives + False Negatives
```

A useful detector needs a practical balance between false detections and missed objects.

---

# Per-Class Performance

Overall mAP can hide class-specific weaknesses.

For example:

```text
Person     mAP = 0.91
Car        mAP = 0.88
Bicycle    mAP = 0.52
```

The bicycle class needs further investigation.

Possible causes:

```text
Too few examples
Small objects
Poor annotations
Visual similarity
Class imbalance
```

---

# Inference on Images

After training:

```python
model = YOLO("best.pt")

results = model("test.jpg")
```

The trained checkpoint may be called something like:

```text
best.pt
```

depending on the training workflow.

---

# Inference on Video

Conceptually:

```text
VIDEO
  ↓
FRAME
  ↓
YOLO
  ↓
DETECTIONS
  ↓
DRAW BOXES
  ↓
DISPLAY
  ↓
NEXT FRAME
```

This can be used for:

```text
Traffic Analysis
Object Counting
Surveillance
Industrial Monitoring
Robotics
```

---

# Webcam Inference

A webcam pipeline can look like:

```text
WEBCAM
  ↓
OpenCV
  ↓
FRAME
  ↓
YOLO
  ↓
DETECTIONS
  ↓
OpenCV
  ↓
DISPLAY
```

OpenCV handles the camera and display while YOLO performs the detection.

---

# Real-Time Performance

Important factors include:

```text
Model size
Input resolution
GPU
CPU
CUDA
Frame rate
Number of objects
Post-processing
```

Measure actual performance instead of assuming that a model is real-time.

---

# FPS

Frames per second is commonly used to describe video-processing speed.

For example:

```text
10 FPS
30 FPS
60 FPS
```

A higher FPS means more frames are processed each second.

But FPS should be considered together with latency and detection quality.

---

# Small Object Detection

Small objects can be difficult for YOLO.

Examples:

```text
Tiny objects
Distant objects
Low-resolution objects
Crowded scenes
```

Possible approaches include:

```text
Higher input resolution
Better training data
More small-object examples
Appropriate model selection
Careful augmentation
```

---

# Occlusion

Objects may be partially hidden:

```text
Person A
████████
     ████████
     Person B
```

This can make detection difficult.

Diverse training data containing realistic occlusion can improve robustness.

---

# Data Augmentation

YOLO training can use transformations such as:

```text
Flip
Scale
Crop
Translation
Color variation
Mosaic-style augmentation
```

The exact augmentation strategy depends on the implementation and version.

Augmentation should represent realistic conditions rather than arbitrary image distortion.

---

# Overfitting

A custom detector can overfit a small dataset.

Warning signs include:

```text
Very strong training metrics
Weak validation metrics
Poor performance on new images
```

Potential solutions:

```text
More diverse data
Better annotations
Augmentation
Transfer learning
Regularization
Early stopping
```

---

# Domain Shift

A detector trained in one environment may fail in another.

Example:

```text
Training:
Studio Images

Deployment:
Outdoor Images
```

Differences may include:

```text
Lighting
Background
Camera
Resolution
Object orientation
Distance
Weather
```

This is called domain shift.

Real-world testing should include the conditions expected during deployment.

---

# Error Analysis

Inspect:

```text
True Positives
False Positives
False Negatives
Wrong Classes
Poorly Localized Boxes
```

A useful process is:

```text
BAD PREDICTION
      ↓
CHECK IMAGE
      ↓
CHECK LABEL
      ↓
CHECK CONFIDENCE
      ↓
CHECK BOX
      ↓
IDENTIFY PATTERN
      ↓
IMPROVE DATA / MODEL
```

---

# Model Export

A trained YOLO model may be exported for other runtimes.

Potential formats include:

```text
ONNX
TensorRT
OpenVINO
TFLite
```

The available export targets depend on the YOLO implementation and model.

---

# ONNX

ONNX provides a portable representation that can be used by different inference runtimes.

Conceptually:

```text
YOLO Model
    ↓
ONNX Export
    ↓
ONNX Runtime / Other Runtime
    ↓
Inference
```

This can be useful when the training framework and deployment runtime are different.

---

# TensorRT

NVIDIA TensorRT is designed for optimized inference on NVIDIA hardware.

Conceptually:

```text
YOLO
 ↓
TensorRT
 ↓
NVIDIA GPU
 ↓
Optimized Inference
```

TensorRT can be useful when inference latency and throughput are important.

---

# Docker Deployment

A YOLO application can be containerized:

```text
APPLICATION
     ↓
DOCKER
     ↓
PYTHON
     ↓
YOLO
     ↓
CUDA
     ↓
NVIDIA GPU
```

For GPU containers, the host must have a compatible NVIDIA driver and container runtime configuration.

---

# API Deployment

A YOLO model can be exposed through an API.

```text
CLIENT
  ↓
HTTP
  ↓
FASTAPI
  ↓
YOLO
  ↓
PREDICTIONS
  ↓
JSON
```

Example response:

```json
{
  "detections": [
    {
      "class": "person",
      "confidence": 0.94,
      "bbox": [120, 80, 340, 520]
    }
  ]
}
```

---

# Streamlit Deployment

A simple interactive application can be:

```text
USER
  ↓
UPLOAD IMAGE
  ↓
STREAMLIT
  ↓
YOLO
  ↓
ANNOTATED IMAGE
  ↓
DISPLAY
```

This is useful for demonstrations and internal tools.

---

# Common YOLO Problems

## No Detections

Check:

```text
Confidence threshold
Model weights
Class mapping
Image preprocessing
Dataset labels
Training quality
```

## Too Many False Positives

Investigate:

```text
Confidence threshold
Training data
Background bias
Incorrect annotations
Class imbalance
```

## Poor Bounding Boxes

Investigate:

```text
Annotation quality
Input resolution
Training data
Model configuration
Localization performance
```

## Training is Too Slow

Check:

```text
GPU availability
CUDA configuration
Batch size
Image size
Model size
Data loading
```

## CUDA Not Available

Check:

```text
NVIDIA driver
PyTorch CUDA build
CUDA compatibility
GPU visibility
Container configuration
```

---

# Practical YOLO Workflow

```text
1. Collect images
        ↓
2. Define classes
        ↓
3. Annotate objects
        ↓
4. Validate annotations
        ↓
5. Create dataset structure
        ↓
6. Create data.yaml
        ↓
7. Select pretrained model
        ↓
8. Train
        ↓
9. Validate
        ↓
10. Inspect predictions
        ↓
11. Tune thresholds
        ↓
12. Test real-world images
        ↓
13. Export if required
        ↓
14. Deploy
```

---

# Complete YOLO Architecture

```text
                         YOLO PROJECT
                              │
                              ▼
                           DATASET
                              │
                              ▼
                         ANNOTATIONS
                              │
                              ▼
                         data.yaml
                              │
                              ▼
                      PRETRAINED MODEL
                              │
                              ▼
                           TRAINING
                              │
                              ▼
                         VALIDATION
                              │
                ┌─────────────┼─────────────┐
                ▼             ▼             ▼
             Precision      Recall         mAP
                │             │             │
                └─────────────┼─────────────┘
                              ▼
                        ERROR ANALYSIS
                              │
                              ▼
                           best.pt
                              │
                              ▼
                          INFERENCE
                              │
                 ┌────────────┼────────────┐
                 ▼            ▼            ▼
               Image        Video       Webcam
                 │            │            │
                 └────────────┼────────────┘
                              ▼
                           DEPLOYMENT
                              │
                  ┌───────────┼───────────┐
                  ▼           ▼           ▼
               Streamlit   FastAPI      Docker
                              │
                              ▼
                          GPU / Edge
```

---

# Practical Checklist

## Dataset

```text
□ Define classes
□ Collect representative images
□ Annotate objects
□ Validate labels
□ Check class balance
□ Split train / validation / test
□ Create data.yaml
```

## Training

```text
□ Select pretrained model
□ Check GPU
□ Configure image size
□ Configure batch size
□ Train
□ Monitor validation metrics
□ Save best checkpoint
```

## Evaluation

```text
□ Test unseen images
□ Check precision
□ Check recall
□ Check mAP
□ Inspect per-class performance
□ Review false positives
□ Review false negatives
```

## Deployment

```text
□ Test inference latency
□ Test real-world images
□ Verify preprocessing
□ Verify class mapping
□ Export if required
□ Package dependencies
□ Test GPU / CPU behavior
```

---

# Key Takeaways

```text
✓ YOLO is designed for efficient object detection

✓ A detection contains class, confidence and location

✓ Bounding boxes require correctly formatted annotations

✓ Custom YOLO training depends heavily on dataset quality

✓ Transfer learning is a practical starting point

✓ Confidence threshold affects precision and recall

✓ NMS removes redundant detections

✓ IoU measures localization overlap

✓ mAP is a major detection evaluation metric

✓ GPU acceleration can significantly improve training and inference

✓ ONNX and TensorRT can help with deployment

✓ Real-world testing is essential

✓ The best detector is not necessarily the largest model

✓ Data quality often matters more than simply increasing model size
```

---

# Related Experiments

- [Computer Vision Fundamentals →](/labs/computer-vision/fundamentals/)
- [Image Classification →](/labs/computer-vision/image-classification/)
- [Object Detection →](/labs/computer-vision/object-detection/)
- [OpenCV & Image Processing →](/labs/computer-vision/opencv/)
- [Image Segmentation →](/labs/computer-vision/image-segmentation/)
- [U-Net Experiments →](/labs/computer-vision/u-net/)
- [Model Evaluation →](/labs/computer-vision/model-evaluation/)
- [Computer Vision Deployment →](/labs/computer-vision/deployment/)

---

> **YOLO makes detection fast, but building a reliable detector is still a data, evaluation, and deployment engineering problem.**
