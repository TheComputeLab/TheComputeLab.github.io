---
title: "Object Detection"
description: "Practical object detection using bounding boxes, YOLO, confidence scores, IoU, NMS, mAP, datasets, training, inference, and deployment."
weight: 30
toc: true
---

> **Object detection answers two questions: what is present, and where is it?**

Object detection identifies objects inside an image and returns both their class and location.

A simplified pipeline is:

```text
IMAGE
  ↓
PREPROCESSING
  ↓
OBJECT DETECTION MODEL
  ↓
BOUNDING BOXES
  ↓
CLASS + CONFIDENCE
  ↓
FINAL DETECTIONS
```

---

# What is Object Detection?

Image classification answers:

```text
What is in this image?
```

Object detection goes further:

```text
What objects are present?
Where are they?
What class does each object belong to?
How confident is the model?
```

For example:

```text
Image
  ↓
YOLO
  ↓
┌──────────────────────────┐
│ Person      0.94         │
│ Bounding Box: (...)      │
├──────────────────────────┤
│ Bicycle     0.87         │
│ Bounding Box: (...)      │
└──────────────────────────┘
```

An image can contain multiple objects and multiple classes.

---

# Classification vs Detection

Consider an image containing a dog.

### Classification

```text
Image
  ↓
Model
  ↓
Dog
```

### Detection

```text
Image
  ↓
Model
  ↓
Dog
+ Bounding Box
+ Confidence
```

Therefore:

```text
Classification → WHAT?

Detection      → WHAT + WHERE?
```

---

# Bounding Boxes

A bounding box defines the approximate location of an object.

Conceptually:

```text
+--------------------------------+
|                                |
|       ┌──────────────┐         |
|       │              │         |
|       │     DOG      │         |
|       │              │         |
|       └──────────────┘         |
|                                |
+--------------------------------+
```

The box may be represented using:

```text
x1
y1
x2
y2
```

where:

```text
(x1, y1) → top-left corner
(x2, y2) → bottom-right corner
```

---

# Bounding Box Formats

Different tools and datasets may represent boxes differently.

Common representations include:

```text
x1, y1, x2, y2
```

or:

```text
x_center, y_center, width, height
```

Some formats use normalized values rather than raw pixel coordinates.

Always verify the annotation format before training.

---

# YOLO Annotation Format

A common YOLO label format is:

```text
class_id x_center y_center width height
```

The coordinates are generally normalized relative to the image dimensions.

Example:

```text
0 0.52 0.48 0.30 0.40
```

This represents:

```text
Class ID
Center X
Center Y
Width
Height
```

The exact dataset configuration should be checked before training.

---

# Detection Dataset Structure

A typical YOLO-style dataset can look like:

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

The images and labels must correspond correctly.

For example:

```text
images/train/image001.jpg
labels/train/image001.txt
```

---

# Annotation

Object detection requires location information in addition to class labels.

For example:

```text
image001.jpg
```

might contain:

```text
Person
Bicycle
Car
```

The annotation must identify the location of each object.

```text
IMAGE
 │
 ├── Person → Box
 ├── Bicycle → Box
 └── Car → Box
```

Annotation quality has a major effect on model performance.

---

# Annotation Errors

Common annotation problems include:

```text
Missing objects
Wrong class
Incorrect bounding box
Too-large box
Too-small box
Duplicate annotation
Incorrect image-label pairing
```

A model trained on poor annotations can learn incorrect behavior.

---

# YOLO

YOLO stands for:

```text
You Only Look Once
```

YOLO-family models are designed for efficient object detection and are widely used for real-time and near-real-time applications.

A simplified workflow is:

```text
IMAGE
  ↓
YOLO MODEL
  ↓
FEATURE EXTRACTION
  ↓
OBJECT PREDICTIONS
  ↓
POST-PROCESSING
  ↓
FINAL DETECTIONS
```

---

# Why YOLO is Useful

YOLO is popular because it provides a practical balance between:

```text
Accuracy
Speed
Ease of use
Deployment flexibility
```

It can be used for:

```text
Images
Video
Webcams
Industrial inspection
Robotics
Surveillance
Traffic analysis
Custom datasets
```

---

# Detection Pipeline

A practical detection pipeline looks like:

```text
INPUT IMAGE
     ↓
RESIZE / PREPROCESS
     ↓
YOLO MODEL
     ↓
RAW PREDICTIONS
     ↓
CONFIDENCE FILTER
     ↓
NON-MAXIMUM SUPPRESSION
     ↓
FINAL BOXES
```

Understanding post-processing is important because the model may initially produce multiple overlapping predictions.

---

# Confidence Score

A detection model generally assigns a confidence score to a prediction.

Example:

```text
Person     0.96
Car        0.91
Bicycle    0.43
```

A confidence threshold can be used:

```text
threshold = 0.50
```

Then:

```text
0.96 → Keep
0.91 → Keep
0.43 → Remove
```

A very high threshold can remove valid detections.

A very low threshold can increase false positives.

---

# Non-Maximum Suppression

Object detectors may produce multiple boxes around the same object.

Example:

```text
       ┌───────────────┐
      ┌────────────────┐
      │    OBJECT      │
      └────────────────┘
       └──────────────┘
```

Non-Maximum Suppression, or NMS, helps keep the strongest prediction.

Simplified process:

```text
Multiple Boxes
      ↓
Compare Overlap
      ↓
Keep Highest Confidence
      ↓
Remove Redundant Boxes
      ↓
Final Detection
```

---

# Intersection over Union

IoU measures the overlap between two bounding boxes.

The formula is:

```text
IoU = Area of Intersection
      ─────────────────────
       Area of Union
```

Conceptually:

```text
Prediction Box
┌───────────────┐
│       ┌───────┼───────┐
│       │   ∩   │       │
└───────┼───────┘       │
        └───────────────┘
        Ground Truth
```

IoU ranges from:

```text
0 → No overlap
1 → Perfect overlap
```

---

# Why IoU Matters

IoU helps determine whether a predicted box sufficiently overlaps the ground-truth box.

For example:

```text
IoU = 0.90
```

indicates strong overlap.

While:

```text
IoU = 0.20
```

indicates weak overlap.

The threshold used depends on the evaluation protocol.

---

# Precision

Detection precision answers:

> Of the objects the model detected, how many were correct?

```text
Precision =
True Positives
────────────────────────
True Positives + False Positives
```

High precision means fewer false detections.

---

# Recall

Detection recall answers:

> Of the objects that actually exist, how many did the model find?

```text
Recall =
True Positives
────────────────────────
True Positives + False Negatives
```

High recall means fewer missed objects.

---

# Precision vs Recall

There is often a trade-off.

A higher confidence threshold may:

```text
Reduce false positives
but
Increase missed detections
```

A lower threshold may:

```text
Find more objects
but
Increase false positives
```

The correct operating point depends on the application.

---

# mAP

Mean Average Precision, commonly abbreviated as:

```text
mAP
```

is a widely used object detection evaluation metric.

It combines precision-recall behavior across classes and detection thresholds according to the evaluation protocol.

You may encounter metrics such as:

```text
mAP@0.5
mAP@0.5:0.95
```

These should not be treated as interchangeable.

---

# Training an Object Detector

A simplified training workflow is:

```text
ANNOTATED DATASET
       ↓
DATA CONFIGURATION
       ↓
MODEL
       ↓
TRAINING
       ↓
VALIDATION
       ↓
BEST CHECKPOINT
       ↓
TEST / INFERENCE
```

Training normally involves many iterations over batches of annotated images.

---

# Transfer Learning for Detection

Instead of starting with random model weights, a pretrained detection model can be adapted to a custom dataset.

```text
PRETRAINED YOLO
      ↓
GENERAL VISUAL FEATURES
      ↓
CUSTOM DATASET
      ↓
FINE-TUNING
      ↓
CUSTOM DETECTOR
```

This can reduce training requirements compared with training a large model completely from scratch.

---

# Custom Object Detection

Suppose we want to detect objects in a custom environment.

The workflow becomes:

```text
COLLECT IMAGES
      ↓
ANNOTATE OBJECTS
      ↓
CHECK LABELS
      ↓
SPLIT DATASET
      ↓
TRAIN YOLO
      ↓
VALIDATE
      ↓
ERROR ANALYSIS
      ↓
INFERENCE
```

---

# Data Augmentation

Detection training can use augmentation such as:

```text
Horizontal Flip
Scale
Crop
Rotation
Translation
Brightness
Contrast
```

However, transformations must preserve valid bounding-box annotations.

An augmentation that moves or distorts an object must also correctly transform its bounding box.

---

# Class Imbalance

A detection dataset may contain:

```text
Person      10,000 objects
Car          8,000 objects
Bicycle        300 objects
```

The model may learn the majority classes more easily.

Possible strategies include:

```text
Collect more minority examples
Improve dataset diversity
Review augmentation
Inspect per-class metrics
Use appropriate sampling strategies
```

---

# Object Detection Errors

Common errors include:

```text
False Positive
False Negative
Wrong Class
Poor Localization
Duplicate Detection
Missed Small Object
```

A useful error-analysis workflow is:

```text
Prediction
   ↓
Compare with Ground Truth
   ↓
Check Class
   ↓
Check Bounding Box
   ↓
Check Confidence
   ↓
Identify Failure Pattern
```

---

# Small Object Detection

Small objects can be difficult because they occupy only a small portion of the image.

For example:

```text
+--------------------------------+
|                                |
|                 •              |
|                                |
|                                |
+--------------------------------+
```

Potential challenges include:

```text
Low pixel count
Blur
Occlusion
Downscaling
Background clutter
```

Possible improvements depend on the model and pipeline and may include higher input resolution, better data, appropriate augmentation, and architecture choices.

---

# Occlusion

An object may be partially hidden.

```text
┌──────────────┐
│    OBJECT    │
│       ███████│
│       ███████│
└──────────────┘
```

The detector must infer the object's presence from incomplete visual information.

More diverse training examples can help improve robustness.

---

# Real-Time Detection

For video or webcam applications:

```text
CAMERA
  ↓
FRAME
  ↓
PREPROCESS
  ↓
MODEL
  ↓
POST-PROCESS
  ↓
DISPLAY
  ↓
NEXT FRAME
```

The system must balance:

```text
Accuracy
FPS
Latency
GPU utilization
Memory usage
```

A highly accurate model that is too slow may not be suitable for real-time applications.

---

# Detection on GPU

GPU acceleration can significantly improve inference performance for many deep-learning detectors.

A typical setup is:

```text
APPLICATION
     ↓
PYTHON
     ↓
YOLO / PyTorch
     ↓
CUDA
     ↓
NVIDIA GPU
```

This connects directly with the Local AI Lab.

---

# Object Detection Deployment

A trained detector can be deployed through:

```text
Streamlit
FastAPI
Docker
Cloud services
Edge devices
Desktop applications
Web applications
```

A typical API architecture is:

```text
CLIENT
  ↓
HTTP REQUEST
  ↓
FASTAPI
  ↓
PREPROCESSING
  ↓
YOLO
  ↓
POST-PROCESSING
  ↓
JSON RESPONSE
```

---

# Example Detection Response

An API could return information such as:

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

The exact response structure depends on the application.

---

# Practical Python Inference

A simplified example using a YOLO-compatible Python API might look like:

```python
from ultralytics import YOLO

model = YOLO("best.pt")

results = model("image.jpg")

for result in results:
    for box in result.boxes:
        print(box.xyxy)
        print(box.conf)
        print(box.cls)
```

The exact API depends on the YOLO implementation and version being used.

---

# Model Export

Depending on the deployment environment, a trained detector may be exported to another format.

Common deployment-oriented formats include:

```text
PyTorch
ONNX
TensorRT
OpenVINO
TFLite
```

Compatibility depends on the model architecture and target runtime.

---

# Object Detection Architecture

A complete detection system can be viewed as:

```text
                       OBJECT DETECTION
                              │
                              ▼
                           DATASET
                              │
                              ▼
                         ANNOTATION
                              │
                              ▼
                         PREPROCESSING
                              │
                              ▼
                         YOLO / MODEL
                              │
                              ▼
                       RAW PREDICTIONS
                              │
                    ┌─────────┴─────────┐
                    ▼                   ▼
              CONFIDENCE              BOXES
               FILTER                   │
                    │                   │
                    └─────────┬─────────┘
                              ▼
                             NMS
                              │
                              ▼
                      FINAL DETECTIONS
                              │
                 ┌────────────┴────────────┐
                 ▼                         ▼
             VISUALIZATION             METRICS
                                         │
                                  IoU / mAP / Recall
                                         │
                                         ▼
                                    DEPLOYMENT
```

---

# Practical Checklist

## Before Training

```text
□ Collect representative images
□ Define object classes
□ Annotate every required object
□ Check annotation quality
□ Verify image-label pairing
□ Check class distribution
□ Split train / validation / test
□ Configure dataset
```

## During Training

```text
□ Monitor training metrics
□ Monitor validation metrics
□ Watch for overfitting
□ Save the best checkpoint
□ Inspect sample predictions
```

## After Training

```text
□ Test unseen images
□ Measure IoU
□ Check precision
□ Check recall
□ Review mAP
□ Inspect false positives
□ Inspect false negatives
□ Test different confidence thresholds
```

## Before Deployment

```text
□ Test inference speed
□ Measure latency
□ Check GPU / CPU utilization
□ Verify preprocessing
□ Verify class mapping
□ Test real-world images
□ Package model and dependencies
```

---

# Common Troubleshooting

### No detections

Check:

```text
Confidence threshold
Model weights
Image preprocessing
Class mapping
Dataset labels
Image quality
```

### Too many false positives

Try investigating:

```text
Confidence threshold
Training data diversity
Incorrect annotations
Background bias
Class imbalance
```

### Boxes are poorly positioned

Investigate:

```text
Annotation quality
Training data
Input resolution
Localization performance
IoU metrics
```

### Detection is too slow

Investigate:

```text
Model size
Input resolution
CPU vs GPU
CUDA configuration
Batch size
Inference runtime
```

---

# Key Takeaways

```text
✓ Object detection identifies what and where

✓ Bounding boxes represent object locations

✓ YOLO is a practical detection framework

✓ Confidence thresholds affect precision and recall

✓ IoU measures bounding-box overlap

✓ NMS removes redundant overlapping detections

✓ mAP is a major detection evaluation metric

✓ Annotation quality directly affects model quality

✓ Small objects and occlusion create additional challenges

✓ GPU acceleration can improve inference performance

✓ Real-time detection requires balancing accuracy and speed

✓ Deployment requires more than just the trained model
```

---

# Related Experiments

- [Computer Vision Fundamentals →](/labs/computer-vision/fundamentals/)
- [Image Classification →](/labs/computer-vision/image-classification/)
- [Image Segmentation →](/labs/computer-vision/image-segmentation/)
- [YOLO Experiments →](/labs/computer-vision/yolo/)
- [U-Net Experiments →](/labs/computer-vision/u-net/)
- [OpenCV Experiments →](/labs/computer-vision/opencv/)
- [Model Evaluation →](/labs/computer-vision/model-evaluation/)
- [Computer Vision Deployment →](/labs/computer-vision/deployment/)

---

> **Detection is not just drawing boxes — it is the engineering problem of reliably locating objects under real-world conditions.**
