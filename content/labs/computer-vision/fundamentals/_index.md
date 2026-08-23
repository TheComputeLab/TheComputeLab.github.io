---
title: "Computer Vision Fundamentals"
description: "Understanding images as data, pixels, channels, features, convolution, CNNs, and the foundations of modern computer vision."
weight: 10
toc: true
---

> **Before a model can understand an image, we need to understand what an image actually is.**

Computer Vision is the field of Artificial Intelligence that enables computers to extract useful information from images and video.

## What is Computer Vision?

Typical computer vision tasks include:

```text
Image
 │
 ├── Classification
 │      └── What is in the image?
 │
 ├── Object Detection
 │      └── What objects are present and where?
 │
 ├── Segmentation
 │      └── Which pixels belong to which object?
 │
 ├── Image Generation
 │      └── Can we create a new image?
 │
 └── Image Analysis
        └── What useful information can we extract?
```

## An Image is Data

A computer receives an image as numerical values. A grayscale image can be represented as:

```text
[
  [  0,  20,  45,  80 ],
  [ 10,  35,  70, 120 ],
  [ 15,  50, 100, 180 ],
  [ 30,  70, 140, 220 ]
]
```

Therefore:

```text
IMAGE = MATRIX OF NUMBERS
```

## Pixels

A pixel is the smallest individual element of a digital image.

For an 8-bit grayscale image:

```text
0   → Black
255 → White
```

Values between them represent different shades of gray.

## Image Resolution

For example:

```text
1920 × 1080
```

means:

```text
Width  = 1920 pixels
Height = 1080 pixels
```

Total pixels:

```text
1920 × 1080 = 2,073,600 pixels
```

Higher resolution generally means more visual information, but also more computation and memory.

## Image Dimensions

A digital image can be described using:

```text
Height × Width × Channels
```

For example:

```text
224 × 224 × 3
```

means:

```text
Height   = 224
Width    = 224
Channels = 3
```

## Color Channels

A color image commonly uses three channels:

```text
R → Red
G → Green
B → Blue
```

Each channel contains its own matrix of pixel values.

## RGB Representation

A single RGB pixel can be represented as:

```text
(R, G, B)
```

For example:

```text
(255, 0, 0) → Red
(0, 255, 0) → Green
(0, 0, 255) → Blue
```

## Grayscale Images

A grayscale image contains a single channel:

```text
Height × Width × 1
```

For example:

```text
224 × 224 × 1
```

Advantages can include less memory, fewer input values, and simpler processing when color is not important.

## Image Tensors

Modern deep learning frameworks commonly represent images as tensors.

```text
Image
  ↓
Tensor
  ↓
[Height, Width, Channels]
```

A batch may become:

```text
[Batch, Height, Width, Channels]
```

For example:

```text
[32, 224, 224, 3]
```

means 32 images at 224 × 224 resolution with 3 color channels.

## Normalization

Raw pixel values may range from:

```text
0 → 255
```

A common transformation is:

```text
normalized_pixel = pixel / 255
```

So:

```text
0   → 0.0
128 → 0.502
255 → 1.0
```

The exact preprocessing should match the model and training pipeline.

## Image Preprocessing

A typical preprocessing pipeline may look like:

```text
RAW IMAGE
    ↓
Resize
    ↓
Color Conversion
    ↓
Noise Reduction
    ↓
Normalization
    ↓
Augmentation
    ↓
MODEL INPUT
```

Incorrect preprocessing can significantly reduce model performance.

## Resizing

Neural networks often expect a fixed input size:

```text
Original
1920 × 1080

       ↓

Resize

224 × 224
```

The resize operation must be selected carefully because aggressive resizing can remove important visual information.

## Image Filtering

Traditional computer vision uses filters such as:

```text
Blur
Sharpen
Edge Detection
Thresholding
Morphological Operations
```

These operations remain useful in modern deep-learning workflows.

## Features

A feature is a useful visual pattern that helps a model distinguish objects or structures.

Early visual features may include:

```text
Edges
Corners
Lines
Textures
```

Deeper neural networks can learn:

```text
Edges
  ↓
Textures
  ↓
Shapes
  ↓
Parts
  ↓
Objects
```

## Convolution

Convolution is one of the fundamental operations behind Convolutional Neural Networks.

A small matrix called a **kernel** or **filter** moves across the image:

```text
IMAGE + KERNEL
      ↓
FEATURE MAP
```

The kernel performs mathematical operations on local regions of the image.

## Why Convolution Works

Images contain spatial relationships. Nearby pixels are often related.

A convolution filter examines a local region rather than treating every pixel independently.

This allows a network to learn patterns such as:

```text
Edges
 ↓
Corners
 ↓
Textures
 ↓
Shapes
 ↓
Objects
```

## Convolutional Neural Networks

A CNN is a neural network architecture designed to work effectively with image-like data.

A simplified CNN:

```text
IMAGE
  ↓
CONVOLUTION
  ↓
ACTIVATION
  ↓
POOLING
  ↓
CONVOLUTION
  ↓
ACTIVATION
  ↓
POOLING
  ↓
FEATURES
  ↓
CLASSIFIER
  ↓
PREDICTION
```

## Pooling

Pooling reduces the spatial dimensions of feature maps.

A common example is:

```text
Max Pooling
```

For a 2 × 2 region, max pooling selects the largest value.

Pooling can reduce computation while retaining important information.

## Activation Functions

A commonly used activation function in CNNs is:

```text
ReLU
```

Conceptually:

```text
ReLU(x) = max(0, x)
```

Therefore:

```text
-5 → 0
-1 → 0
 0 → 0
 2 → 2
 8 → 8
```

## Classification

Classification answers:

> **What is this image?**

Example:

```text
Image
  ↓
CNN
  ↓
Probabilities
  ↓
Rice    0.05
Wheat   0.90
Lentil  0.05
  ↓
Prediction: Wheat
```

## Object Detection

Detection answers:

```text
What is it?
Where is it?
```

The output may contain:

```text
Object
Bounding Box
Confidence
Class
```

Modern detectors such as YOLO perform this task efficiently.

## Image Segmentation

Segmentation works at the pixel level.

```text
Image
  ↓
Pixel-level Mask
  ↓
Object Region
```

For example:

```text
MRI
 ↓
U-Net
 ↓
Tumor Mask
```

This is especially important in medical imaging.

## Classification vs Detection vs Segmentation

```text
CLASSIFICATION
Image → Class

DETECTION
Image → Class + Bounding Box

SEGMENTATION
Image → Pixel-level Mask
```

## Computer Vision Pipeline

A typical computer vision project looks like:

```text
DATASET
   ↓
DATA EXPLORATION
   ↓
PREPROCESSING
   ↓
AUGMENTATION
   ↓
TRAIN / VALIDATION SPLIT
   ↓
MODEL TRAINING
   ↓
VALIDATION
   ↓
EVALUATION
   ↓
INFERENCE
   ↓
DEPLOYMENT
```

## Dataset

Important considerations include:

```text
Number of images
Class distribution
Image quality
Resolution
Labels
Duplicates
Missing data
Data leakage
```

A large dataset is not automatically a good dataset.

## Training, Validation and Test Data

```text
TRAIN
  ↓
Learn model parameters

VALIDATION
  ↓
Tune the system

TEST
  ↓
Estimate final performance
```

Keeping these datasets properly separated is essential for trustworthy evaluation.

## Overfitting

A model can memorize the training dataset rather than learning general patterns.

The model may perform well on training data but poorly on unseen data.

## Underfitting

Underfitting occurs when the model is unable to capture the patterns in the data.

Possible causes include:

- Insufficient model capacity
- Poor features
- Insufficient training
- Excessive regularization

## Computer Vision Metrics

### Classification

```text
Accuracy
Precision
Recall
F1 Score
Confusion Matrix
```

### Object Detection

```text
IoU
Precision
Recall
mAP
```

### Segmentation

```text
IoU
Dice Score
Precision
Recall
```

## From Notebook to Application

A trained model is only one component of a real application.

```text
USER
  ↓
APPLICATION
  ↓
API
  ↓
VISION MODEL
  ↓
GPU / CPU
  ↓
PREDICTION
  ↓
APPLICATION
```

Technologies used in this lab may include:

```text
Python
Streamlit
FastAPI
Docker
Hugging Face Spaces
Cloud deployment
Edge devices
```

## Computer Vision and Edge AI

A typical edge vision system can look like:

```text
Camera
  ↓
Edge Device
  ↓
Computer Vision Model
  ↓
Prediction
  ↓
Action
```

## Practical Computer Vision Stack

```text
Python
│
├── NumPy
├── OpenCV
├── Pillow
│
├── TensorFlow / Keras
├── PyTorch
│
├── YOLO
├── U-Net
│
├── Scikit-learn
│
├── Streamlit
├── FastAPI
│
└── Docker
```

## Key Mental Model

When approaching a computer vision problem:

```text
1. What is the input?
        ↓
2. What do I want to predict?
        ↓
3. Which vision task is appropriate?
        ↓
4. What data do I have?
        ↓
5. How should the data be processed?
        ↓
6. Which model is appropriate?
        ↓
7. How will I evaluate it?
        ↓
8. How will I deploy it?
```

## The Computer Vision Journey

```text
                 COMPUTER VISION
                        │
                        ▼
                 IMAGE FUNDAMENTALS
                        │
                        ▼
                  PREPROCESSING
                        │
                        ▼
              ┌─────────┼─────────┐
              │         │         │
              ▼         ▼         ▼
        CLASSIFICATION DETECTION SEGMENTATION
              │         │         │
             CNN       YOLO      U-NET
              │         │         │
              └─────────┼─────────┘
                        │
                        ▼
                   EVALUATION
                        │
                        ▼
                   DEPLOYMENT
                        │
                        ▼
                  REAL SYSTEM
```

## What This Lab Will Build

```text
Fundamentals
     ↓
Image Processing
     ↓
Classification
     ↓
Object Detection
     ↓
Segmentation
     ↓
Medical Imaging
     ↓
Evaluation
     ↓
Deployment
     ↓
Complete Architecture
```

The goal is not simply to understand computer vision terminology.

The goal is to understand how to build, evaluate, troubleshoot and deploy a complete computer vision system.

## Key Takeaways

```text
✓ Images are numerical data
✓ Pixels form image matrices
✓ RGB images contain multiple channels
✓ Preprocessing prepares images for models
✓ CNNs learn hierarchical visual features
✓ Classification predicts what an image contains
✓ Detection predicts what and where
✓ Segmentation predicts pixel-level regions
✓ Different tasks require different metrics
✓ Dataset quality strongly affects model performance
✓ A trained model is only one part of a real system
✓ Deployment connects the model to an application
```

## Related Experiments

- [Image Classification →](/labs/computer-vision/image-classification/)
- [Object Detection →](/labs/computer-vision/object-detection/)
- [Image Segmentation →](/labs/computer-vision/image-segmentation/)
- [OpenCV →](/labs/computer-vision/opencv/)
- [Image Preprocessing →](/labs/computer-vision/preprocessing/)
- [YOLO Experiments →](/labs/computer-vision/yolo/)
- [U-Net Experiments →](/labs/computer-vision/u-net/)
- [Medical Image Analysis →](/labs/computer-vision/medical-imaging/)

---

> **Computer Vision starts with pixels — but the real engineering begins when those pixels become reliable decisions.**
