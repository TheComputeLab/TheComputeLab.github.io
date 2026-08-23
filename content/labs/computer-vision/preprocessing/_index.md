---
title: "Image Preprocessing"
description: "A practical guide to preparing images for computer vision models using resizing, cropping, normalization, color conversion, denoising, enhancement, and reproducible preprocessing pipelines."
weight: 60
toc: true
---

> **A model can only learn from the data it receives. Good preprocessing makes that data consistent, useful, and suitable for the model.**

Image preprocessing is the process of transforming raw images into a form that a computer vision model can consume reliably.

A typical pipeline looks like:

```text
RAW IMAGE
   ↓
LOAD
   ↓
VALIDATE
   ↓
RESIZE / CROP
   ↓
COLOR CONVERSION
   ↓
NORMALIZATION
   ↓
TENSOR CONVERSION
   ↓
MODEL INPUT
```

---

# Why Image Preprocessing Matters

Images collected from the real world are rarely identical.

They may have different:

```text
Dimensions
Aspect Ratios
Color Spaces
Brightness
Contrast
Noise Levels
File Formats
Camera Conditions
```

A model expects a consistent input representation.

For example:

```text
Image A → 1920 × 1080
Image B → 1280 × 720
Image C → 640 × 480
```

A neural network may instead require:

```text
224 × 224 × 3
```

Preprocessing converts the different inputs into a consistent representation.

---

# Preprocessing Pipeline

A practical pipeline may be:

```text
              RAW IMAGE
                  │
                  ▼
             IMAGE LOAD
                  │
                  ▼
            FORMAT CHECK
                  │
                  ▼
             RESIZE/CROP
                  │
                  ▼
          COLOR CONVERSION
                  │
                  ▼
        NORMALIZATION / SCALE
                  │
                  ▼
          NUMPY / TENSOR
                  │
                  ▼
             MODEL INPUT
```

Not every project needs every step.

The correct pipeline depends on:

```text
Dataset
Model Architecture
Task
Image Modality
Training Strategy
Deployment Environment
```

---

# Loading an Image with OpenCV

OpenCV can load an image with:

```python
import cv2

image = cv2.imread(
    "image.jpg"
)

print(image.shape)
```

The shape is commonly:

```text
(height, width, channels)
```

For example:

```text
(480, 640, 3)
```

means:

```text
Height  = 480
Width   = 640
Channels = 3
```

---

# OpenCV Uses BGR

One common source of confusion is color channel order.

OpenCV normally loads color images as:

```text
BGR
```

while many deep learning workflows expect:

```text
RGB
```

Convert when necessary:

```python
rgb = cv2.cvtColor(
    image,
    cv2.COLOR_BGR2RGB
)
```

Always verify what your model and visualization library expect.

---

# PIL

Python Imaging Library implementations such as Pillow are also commonly used.

```python
from PIL import Image

image = Image.open(
    "image.jpg"
)

print(image.size)
```

Pillow commonly represents images in:

```text
RGB
```

This can make it convenient for many PyTorch data pipelines.

---

# Image Dimensions

Images may have arbitrary dimensions:

```text
1920 × 1080
1280 × 720
1024 × 768
640 × 480
```

Neural networks often require fixed dimensions.

For example:

```text
224 × 224
256 × 256
512 × 512
640 × 640
```

The target size depends on the model.

---

# Resizing

OpenCV:

```python
resized = cv2.resize(
    image,
    (224, 224)
)
```

This produces:

```text
Original
1920 × 1080
     ↓
224 × 224
```

Resizing is simple, but directly forcing an image into a different aspect ratio can distort objects.

---

# Aspect Ratio

Consider:

```text
Original:
16 : 9
```

and:

```text
Target:
1 : 1
```

Direct resizing:

```text
16:9
 ↓
1:1
```

can stretch or compress the image.

For some tasks this may be acceptable.

For others, geometric distortion can hurt model performance.

---

# Resize with Letterboxing

Instead of distorting the image, padding can preserve the aspect ratio.

Conceptually:

```text
┌───────────────────┐
│       IMAGE       │
│                   │
│                   │
└───────────────────┘
        ↓
┌───────────────────┐
│     │ IMAGE │     │
│     │       │     │
│     │       │     │
└───────────────────┘
```

The unused area is padded.

YOLO-style pipelines commonly use aspect-ratio-preserving resizing with padding.

---

# Cropping

Cropping removes part of the image.

```python
crop = image[
    100:400,
    150:450
]
```

Cropping can:

```text
Remove irrelevant regions
Focus on an object
Create fixed-size inputs
Reduce computation
```

However, careless cropping can remove important information.

---

# Center Crop

A center crop selects the central region.

Conceptually:

```text
┌─────────────────────┐
│                     │
│     ┌─────────┐     │
│     │  CROP   │     │
│     └─────────┘     │
│                     │
└─────────────────────┘
```

This is common in classification pipelines.

---

# Random Crop

During training, random crops can create different views of the same image.

Benefits may include:

```text
Data diversity
Translation robustness
Reduced overfitting
```

But the crop must remain appropriate for the task.

For example, random cropping can be dangerous if the target object is frequently removed.

---

# Normalization

Pixel values are commonly stored as:

```text
0 – 255
```

They may be scaled to:

```text
0 – 1
```

For example:

```python
image = image / 255.0
```

This can improve numerical behavior for many models.

---

# Mean and Standard Deviation Normalization

A common transformation is:

```text
normalized =
(image - mean) / std
```

In PyTorch:

```python
from torchvision import transforms

transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize(
        mean=[0.485, 0.456, 0.406],
        std=[0.229, 0.224, 0.225]
    )
])
```

The mean and standard deviation should match the model's training setup.

---

# Training and Inference Must Match

One of the most important rules:

```text
TRAINING PREPROCESSING
        =
INFERENCE PREPROCESSING
```

For example:

```text
Training:
RGB
→ Resize
→ Normalize

Inference:
BGR
→ Different Resize
→ No Normalize
```

This mismatch can significantly reduce model performance.

---

# Data Type

Images can be represented using different numeric types:

```text
uint8
float32
float16
```

A typical deep learning pipeline converts image data to floating point tensors.

Example:

```python
tensor = image.astype(
    "float32"
)
```

The exact dtype should be selected based on the framework and deployment requirements.

---

# NumPy to Tensor

PyTorch:

```python
import torch

tensor = torch.from_numpy(
    image
)
```

Many models expect channel-first tensors:

```text
C × H × W
```

while images are often represented as:

```text
H × W × C
```

This layout difference must be handled correctly.

---

# HWC vs CHW

Image:

```text
H × W × C
```

Model input:

```text
C × H × W
```

In PyTorch:

```python
tensor = torch.from_numpy(
    image
).permute(
    2, 0, 1
)
```

For a batch:

```text
N × C × H × W
```

where:

```text
N = Batch
C = Channels
H = Height
W = Width
```

---

# Batch Dimension

A single image may be:

```text
C × H × W
```

The model may expect:

```text
N × C × H × W
```

Add a batch dimension:

```python
tensor = tensor.unsqueeze(
    0
)
```

Result:

```text
1 × C × H × W
```

---

# Grayscale Images

A grayscale image has one channel:

```text
H × W × 1
```

instead of:

```text
H × W × 3
```

If a model expects three channels, the grayscale image may need to be converted or replicated appropriately.

Do not assume that every model can directly consume grayscale data.

---

# Color Space Conversion

Common color representations include:

```text
RGB
BGR
HSV
LAB
Grayscale
YCbCr
```

Different representations can be useful for different tasks.

For example, HSV separates:

```text
Hue
Saturation
Value
```

which can sometimes make color-based segmentation easier.

---

# Grayscale Conversion

OpenCV:

```python
gray = cv2.cvtColor(
    image,
    cv2.COLOR_BGR2GRAY
)
```

This reduces a three-channel color image to a single intensity channel.

Useful for tasks where color information is not required.

---

# Denoising

Images may contain noise from:

```text
Camera Sensors
Low Light
Compression
Medical Imaging Systems
Transmission
```

OpenCV provides filtering operations such as:

```python
blur = cv2.GaussianBlur(
    image,
    (5, 5),
    0
)
```

Other filters include:

```text
Median Filter
Bilateral Filter
Gaussian Filter
```

Denoising should be used carefully because excessive smoothing can remove useful features.

---

# Gaussian Blur

Gaussian blur smooths local variations.

Conceptually:

```text
NOISY IMAGE
     ↓
GAUSSIAN FILTER
     ↓
SMOOTHER IMAGE
```

It can be useful before:

```text
Edge Detection
Thresholding
Contour Detection
```

but may reduce fine details.

---

# Median Filtering

Median filtering is often useful for reducing certain types of impulse noise.

```python
filtered = cv2.medianBlur(
    image,
    5
)
```

It replaces a pixel with the median value in its neighborhood.

---

# Contrast Enhancement

Low-contrast images may contain limited intensity separation.

Contrast enhancement can make structures easier to distinguish.

Possible techniques include:

```text
Histogram Equalization
CLAHE
Contrast Stretching
```

---

# Histogram Equalization

Histogram equalization redistributes intensity values.

Conceptually:

```text
Input Histogram
      ↓
Redistribution
      ↓
Enhanced Contrast
```

It can improve global contrast but may also amplify noise.

---

# CLAHE

CLAHE stands for:

```text
Contrast Limited Adaptive Histogram Equalization
```

It works locally while limiting excessive contrast amplification.

This can be useful for certain:

```text
Medical Images
Low-Contrast Images
Texture Analysis
```

---

# Thresholding

Thresholding converts an image into regions based on intensity.

Simple threshold:

```python
_, binary = cv2.threshold(
    gray,
    127,
    255,
    cv2.THRESH_BINARY
)
```

Result:

```text
Pixel < Threshold → 0
Pixel ≥ Threshold → 255
```

Thresholding is useful for certain segmentation and preprocessing tasks.

---

# Adaptive Thresholding

Global thresholding may fail when illumination varies.

Adaptive thresholding calculates thresholds based on local regions.

Conceptually:

```text
IMAGE
 ↓
LOCAL REGIONS
 ↓
LOCAL THRESHOLDS
 ↓
BINARY IMAGE
```

This can work better for uneven lighting.

---

# Edge Detection

Edges represent strong intensity changes.

Canny edge detection:

```python
edges = cv2.Canny(
    gray,
    100,
    200
)
```

Edges can help identify:

```text
Object Boundaries
Shapes
Contours
Structures
```

---

# Morphological Operations

Morphological operations work on shapes and binary images.

Common operations:

```text
Erosion
Dilation
Opening
Closing
```

Erosion generally reduces foreground regions.

Dilation generally expands them.

---

# Opening and Closing

Opening:

```text
Erosion
   ↓
Dilation
```

can help remove small noise.

Closing:

```text
Dilation
   ↓
Erosion
```

can help fill small gaps.

---

# Image Quality Checks

Before sending an image to a model, validate:

```text
File Exists
Image Can Be Decoded
Correct Dimensions
Expected Channels
Valid Pixel Values
Expected Data Type
```

Example:

```python
if image is None:
    raise ValueError(
        "Unable to load image"
    )
```

---

# Handling Corrupt Images

A robust pipeline should not assume every file is valid.

Possible problems:

```text
Corrupt JPEG
Empty File
Unsupported Format
Incomplete Download
Invalid Header
```

Handle failures gracefully instead of allowing the entire inference pipeline to crash.

---

# Preprocessing for Classification

A common classification pipeline:

```text
IMAGE
 ↓
Resize
 ↓
Center Crop
 ↓
RGB
 ↓
Tensor
 ↓
Normalize
 ↓
MODEL
```

Example:

```python
from torchvision import transforms

transform = transforms.Compose([
    transforms.Resize(256),
    transforms.CenterCrop(224),
    transforms.ToTensor(),
    transforms.Normalize(
        mean=[0.485, 0.456, 0.406],
        std=[0.229, 0.224, 0.225]
    )
])
```

The actual transformation values must match the model's training configuration.

---

# Preprocessing for Object Detection

Detection pipelines need to preserve the relationship between:

```text
Image
+
Bounding Boxes
```

If an image is resized:

```text
Image Coordinates
        ↓
Bounding Box Coordinates
```

must be transformed consistently.

If an image is cropped:

```text
Image Crop
+
Box Coordinates
```

must remain aligned.

---

# Preprocessing for Segmentation

Segmentation has:

```text
IMAGE
+
MASK
```

Geometric transformations must normally be applied consistently to both.

```text
Image Resize ──────┐
                   ├──→ Aligned
Mask Resize ───────┘
```

However, image interpolation and mask interpolation should not be treated identically.

For categorical masks, nearest-neighbor interpolation is commonly used to avoid creating artificial class values.

---

# Medical Image Preprocessing

Medical imaging may require additional operations:

```text
DICOM Loading
Orientation
Resampling
Windowing
Intensity Normalization
Registration
Cropping
Spacing Normalization
```

The correct pipeline depends heavily on the modality and research protocol.

---

# Augmentation vs Preprocessing

These concepts are related but different.

## Preprocessing

Usually creates the standardized input representation:

```text
Resize
Normalize
Convert
Crop
```

## Augmentation

Intentionally introduces variation during training:

```text
Rotate
Flip
Zoom
Translate
Color Jitter
```

A useful distinction is:

```text
PREPROCESSING
→ Make data consistent

AUGMENTATION
→ Make training data more diverse
```

---

# Avoiding Data Leakage

Preprocessing operations that learn statistics from data should be handled carefully.

For example:

```text
TRAINING DATA
      ↓
Calculate Statistics
      ↓
Apply to Training / Validation / Test
```

Do not calculate dataset-wide statistics using the test set if that would leak test information into model development.

---

# Reproducible Preprocessing

Keep preprocessing code centralized.

Example:

```text
project/
│
├── preprocessing.py
├── dataset.py
├── train.py
├── evaluate.py
└── inference.py
```

This reduces the chance that training and inference use different transformations.

---

# Example Reusable Function

```python
def preprocess_image(
    image
):
    image = cv2.cvtColor(
        image,
        cv2.COLOR_BGR2RGB
    )

    image = cv2.resize(
        image,
        (224, 224)
    )

    image = image.astype(
        "float32"
    ) / 255.0

    return image
```

A production implementation should document the exact expected input and output format.

---

# Visualization Before Training

Always inspect the transformed image.

```text
RAW IMAGE
   ↓
PREPROCESSING
   ↓
VISUALIZE
   ↓
MODEL
```

Ask:

```text
Is the image distorted?
Are colors correct?
Is the target still visible?
Is the image too dark?
Is information lost?
Is the mask still aligned?
```

This simple step can catch many pipeline bugs.

---

# Common Preprocessing Mistakes

## Mistake 1 — Wrong Color Order

```text
Expected: RGB
Received: BGR
```

---

## Mistake 2 — Wrong Image Size

```text
Training: 224 × 224
Inference: 512 × 512
```

when the model expects the training configuration.

---

## Mistake 3 — Missing Normalization

```text
Training → Normalized
Inference → Raw 0–255
```

---

## Mistake 4 — Wrong Tensor Layout

```text
HWC
```

passed where:

```text
CHW
```

is expected.

---

## Mistake 5 — Mask Interpolation

Using ordinary linear interpolation for categorical segmentation masks can introduce invalid class values.

---

## Mistake 6 — Excessive Processing

Every transformation changes the data.

More preprocessing is not automatically better.

---

# Debugging Preprocessing

When predictions look wrong, inspect the pipeline step-by-step:

```text
RAW IMAGE
   ↓
CHECK
   ↓
RESIZE
   ↓
CHECK
   ↓
COLOR CONVERSION
   ↓
CHECK
   ↓
NORMALIZATION
   ↓
CHECK
   ↓
TENSOR
   ↓
CHECK
   ↓
MODEL
```

Do not immediately blame the model.

Many apparent model failures originate in preprocessing.

---

# Performance Considerations

Preprocessing can become a bottleneck.

Example:

```text
GPU Inference
      ↓
Very Fast

CPU Image Processing
      ↓
Slow

Overall Pipeline
      ↓
Limited by CPU
```

Measure:

```text
Load Time
Decode Time
Resize Time
Normalization Time
Inference Time
Post-processing Time
```

Optimize only after identifying the actual bottleneck.

---

# Production Preprocessing Architecture

```text
                 REQUEST
                    │
                    ▼
              INPUT VALIDATION
                    │
                    ▼
               IMAGE DECODE
                    │
                    ▼
               PREPROCESSING
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
       RESIZE    COLOR     NORMALIZE
          │         │         │
          └─────────┼─────────┘
                    ▼
                 TENSOR
                    │
                    ▼
                  MODEL
                    │
                    ▼
                PREDICTION
```

---

# Practical Checklist

## Input

```text
□ File exists
□ File can be decoded
□ Format is supported
□ Dimensions are valid
□ Channels are correct
```

## Transformation

```text
□ Resize verified
□ Aspect ratio considered
□ Crop verified
□ Color order verified
□ Normalization verified
□ Data type verified
```

## Tensor

```text
□ HWC / CHW verified
□ Batch dimension verified
□ Device verified
□ dtype verified
```

## Detection / Segmentation

```text
□ Bounding boxes transformed correctly
□ Masks remain aligned
□ Mask interpolation is appropriate
□ Labels remain valid
```

## Deployment

```text
□ Training and inference pipelines match
□ Preprocessing is version-controlled
□ Pipeline is tested
□ Performance is measured
□ Failures are handled safely
```

---

# End-to-End Example

```text
               RAW JPEG
                  │
                  ▼
             OpenCV / PIL
                  │
                  ▼
             RGB CONVERSION
                  │
                  ▼
              RESIZE
                  │
                  ▼
             NORMALIZE
                  │
                  ▼
             NUMPY ARRAY
                  │
                  ▼
             PYTORCH TENSOR
                  │
                  ▼
              CHW FORMAT
                  │
                  ▼
            BATCH DIMENSION
                  │
                  ▼
                MODEL
                  │
                  ▼
             PREDICTION
```

---

# Key Takeaways

```text
✓ Preprocessing converts raw images into model-ready inputs

✓ Image dimensions should match the model's expected input

✓ OpenCV commonly uses BGR while many ML workflows use RGB

✓ Aspect ratio should be considered when resizing

✓ Normalization must match the training configuration

✓ PyTorch commonly uses C × H × W tensor layout

✓ Models often expect a batch dimension

✓ Detection boxes must remain aligned with image transformations

✓ Segmentation masks require special handling during resizing

✓ Preprocessing and augmentation are different concepts

✓ Dataset statistics must be handled carefully to avoid leakage

✓ Always visualize transformed images

✓ Preprocessing can become an inference bottleneck

✓ Centralizing preprocessing improves reproducibility

✓ More preprocessing does not automatically mean better performance
```

---

# Related Experiments

- [Computer Vision Fundamentals →](/labs/computer-vision/fundamentals/)
- [Image Classification →](/labs/computer-vision/image-classification/)
- [Object Detection →](/labs/computer-vision/object-detection/)
- [Image Segmentation →](/labs/computer-vision/image-segmentation/)
- [Medical Image Analysis →](/labs/computer-vision/medical-imaging/)
- [YOLO Experiments →](/labs/computer-vision/yolo/)
- [U-Net Experiments →](/labs/computer-vision/u-net/)
- [OpenCV Experiments →](/labs/computer-vision/opencv/)
- [Data Augmentation →](/labs/computer-vision/data-augmentation/)
- [Model Evaluation →](/labs/computer-vision/model-evaluation/)
- [Computer Vision Deployment →](/labs/computer-vision/deployment/)
- [Computer Vision Architecture →](/labs/computer-vision/architecture/)

---

> **A reliable computer vision pipeline starts before the model sees the image.**
