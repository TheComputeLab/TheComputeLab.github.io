---
title: "OpenCV & Image Processing"
description: "Practical image processing with OpenCV, NumPy, color spaces, transformations, filtering, thresholding, edges, contours, and computer vision pipelines."
weight: 50
toc: true
---

> **Before a model can understand an image, we often need to prepare the image. OpenCV provides the tools to do that.**

OpenCV is an open-source computer vision library widely used for image and video processing.

A typical workflow is:

```text
IMAGE / VIDEO
      ↓
      OpenCV
      ↓
PREPROCESSING
      ↓
FEATURES / MODEL INPUT
      ↓
COMPUTER VISION MODEL
```

---

# What is OpenCV?

OpenCV stands for:

```text
Open Source Computer Vision Library
```

It provides tools for:

```text
Image Processing
Video Processing
Computer Vision
Feature Detection
Object Tracking
Camera Processing
Geometric Transformations
```

OpenCV is commonly used together with:

```text
Python
NumPy
PyTorch
TensorFlow
YOLO
Streamlit
FastAPI
```

---

# Installing OpenCV

For Python projects, the commonly used package is:

```bash
pip install opencv-python
```

Then import it:

```python
import cv2
```

For environments without GUI support, such as some servers and containers, the headless package may be more appropriate:

```bash
pip install opencv-python-headless
```

Avoid installing both packages unnecessarily in the same environment.

---

# Reading an Image

OpenCV can load an image using:

```python
import cv2

image = cv2.imread("image.jpg")
```

The result is a NumPy array.

Conceptually:

```text
image.jpg
    ↓
cv2.imread()
    ↓
NumPy Array
```

If the file cannot be loaded, `cv2.imread()` can return `None`.

Always validate the result when building a robust application.

---

# Displaying an Image

In a desktop environment:

```python
cv2.imshow("Image", image)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

In notebooks or web applications, other display methods are often more convenient.

For example, with Matplotlib:

```python
import matplotlib.pyplot as plt

plt.imshow(image)
plt.axis("off")
plt.show()
```

Remember that OpenCV normally loads color images in BGR order, while Matplotlib expects RGB.

---

# BGR vs RGB

One of the most common OpenCV issues is the difference between:

```text
OpenCV → BGR
Matplotlib / many ML tools → RGB
```

Convert between them:

```python
rgb = cv2.cvtColor(
    image,
    cv2.COLOR_BGR2RGB
)
```

Conceptually:

```text
BGR
 ↓
Color Conversion
 ↓
RGB
```

If this conversion is forgotten, images may appear with incorrect colors.

---

# Image Dimensions

Because OpenCV images are NumPy arrays:

```python
height, width, channels = image.shape
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

For grayscale images, the shape may simply be:

```text
224 × 224
```

---

# Saving an Image

Use:

```python
cv2.imwrite(
    "output.jpg",
    image
)
```

The return value can be checked to determine whether the write operation succeeded.

---

# Grayscale Conversion

Convert a color image to grayscale:

```python
gray = cv2.cvtColor(
    image,
    cv2.COLOR_BGR2GRAY
)
```

Conceptually:

```text
RGB/BGR Image
      ↓
Grayscale
      ↓
Single Channel
```

Grayscale can simplify tasks where color is not important.

---

# Why Grayscale?

A color image may contain:

```text
3 channels
```

while a grayscale image contains:

```text
1 channel
```

Advantages can include:

```text
Less memory
Fewer values to process
Simpler feature extraction
Useful for edge detection
```

However, converting to grayscale removes color information, so it should only be done when appropriate.

---

# Resizing

Resize an image with:

```python
resized = cv2.resize(
    image,
    (224, 224)
)
```

This is commonly required before feeding an image into a deep-learning model.

```text
Original Image
1920 × 1080
      ↓
Resize
      ↓
224 × 224
```

---

# Interpolation

Different interpolation methods can be used during resizing.

Common examples include:

```text
INTER_NEAREST
INTER_LINEAR
INTER_CUBIC
INTER_AREA
```

The best choice depends on whether the image is being enlarged, reduced, or treated as a categorical mask.

For segmentation masks, nearest-neighbor interpolation is commonly used to avoid creating invalid intermediate class values.

---

# Cropping

Cropping selects a region of interest.

NumPy slicing can be used:

```python
crop = image[
    y1:y2,
    x1:x2
]
```

Conceptually:

```text
+-------------------------+
|                         |
|      ┌──────────┐       |
|      │   ROI    │       |
|      └──────────┘       |
|                         |
+-------------------------+
```

ROI means:

```text
Region of Interest
```

---

# Flipping

Horizontal flip:

```python
flipped = cv2.flip(
    image,
    1
)
```

Vertical flip:

```python
flipped = cv2.flip(
    image,
    0
)
```

Both directions:

```python
flipped = cv2.flip(
    image,
    -1
)
```

Flipping can be useful for augmentation when the transformation makes sense for the application.

---

# Rotation

Images can be rotated using a transformation matrix.

A convenient approach for common rotations is:

```python
rotated = cv2.rotate(
    image,
    cv2.ROTATE_90_CLOCKWISE
)
```

Other angles can be handled with an affine transformation matrix.

---

# Translation

Translation moves an image horizontally or vertically.

Conceptually:

```text
Original
   ↓
Transformation Matrix
   ↓
Shifted Image
```

This can be useful for augmentation and geometric preprocessing.

---

# Image Filtering

Filtering modifies or extracts information from an image.

Common filters include:

```text
Blur
Gaussian Blur
Median Blur
Bilateral Filter
Sharpening
```

---

# Gaussian Blur

Gaussian blur is commonly used to reduce noise.

```python
blurred = cv2.GaussianBlur(
    image,
    (5, 5),
    0
)
```

A simplified pipeline is:

```text
NOISY IMAGE
     ↓
GAUSSIAN BLUR
     ↓
SMOOTHER IMAGE
```

Blurring can also help before edge detection.

---

# Median Blur

Median filtering can be useful for certain types of noise.

```python
blurred = cv2.medianBlur(
    image,
    5
)
```

It replaces a pixel with the median value of its neighborhood.

---

# Thresholding

Thresholding converts an image into regions based on pixel intensity.

A simple binary threshold:

```python
_, binary = cv2.threshold(
    gray,
    127,
    255,
    cv2.THRESH_BINARY
)
```

Conceptually:

```text
Pixel < Threshold → 0
Pixel ≥ Threshold → 255
```

---

# Adaptive Thresholding

A single global threshold does not always work well when lighting varies across an image.

Adaptive thresholding calculates thresholds based on local neighborhoods.

Example:

```python
adaptive = cv2.adaptiveThreshold(
    gray,
    255,
    cv2.ADAPTIVE_THRESH_GAUSSIAN_C,
    cv2.THRESH_BINARY,
    11,
    2
)
```

This can be useful for documents and uneven illumination.

---

# Otsu Thresholding

Otsu's method automatically chooses a threshold based on the image histogram.

```python
_, binary = cv2.threshold(
    gray,
    0,
    255,
    cv2.THRESH_BINARY + cv2.THRESH_OTSU
)
```

It can be useful when foreground and background have reasonably separable intensity distributions.

---

# Edge Detection

Edges often represent object boundaries.

A common OpenCV method is:

```text
Canny Edge Detection
```

Example:

```python
edges = cv2.Canny(
    gray,
    100,
    200
)
```

Pipeline:

```text
IMAGE
  ↓
GRAYSCALE
  ↓
BLUR
  ↓
CANNY
  ↓
EDGES
```

---

# Why Edges Matter

Edges can provide information about:

```text
Object boundaries
Shapes
Corners
Text
Structures
Surface changes
```

Traditional computer vision pipelines often use edges as an intermediate representation.

Deep-learning models can also learn edge-like features automatically.

---

# Contours

Contours represent continuous boundaries around regions.

Find contours:

```python
contours, hierarchy = cv2.findContours(
    binary,
    cv2.RETR_EXTERNAL,
    cv2.CHAIN_APPROX_SIMPLE
)
```

Contours can be used for:

```text
Shape Analysis
Object Counting
Area Measurement
Boundary Detection
Simple Object Detection
```

---

# Drawing Contours

Contours can be drawn using:

```python
cv2.drawContours(
    image,
    contours,
    -1,
    (0, 255, 0),
    2
)
```

The color tuple here is in BGR order.

---

# Bounding Rectangles

A contour can be converted into a bounding rectangle:

```python
x, y, w, h = cv2.boundingRect(
    contour
)
```

Then draw it:

```python
cv2.rectangle(
    image,
    (x, y),
    (x + w, y + h),
    (0, 255, 0),
    2
)
```

This connects traditional OpenCV processing with the bounding boxes used in object detection.

---

# Morphological Operations

Morphological operations work with the structure of binary images.

Common operations include:

```text
Erosion
Dilation
Opening
Closing
```

These are often useful for cleaning masks and binary images.

---

# Erosion

Erosion reduces foreground regions.

Conceptually:

```text
██████
██████
██████

     ↓

 ████
 ████
```

It can help remove small unwanted regions.

---

# Dilation

Dilation expands foreground regions.

```text
 ████
 ████

     ↓

██████
██████
```

It can help connect nearby regions.

---

# Opening

Opening is approximately:

```text
Erosion
   ↓
Dilation
```

It can help remove small noise.

---

# Closing

Closing is approximately:

```text
Dilation
   ↓
Erosion
```

It can help close small gaps inside foreground regions.

---

# Histograms

An image histogram represents the distribution of pixel intensities.

For grayscale:

```text
Intensity
0 ─────────────────── 255
│
│      ███
│   ███████
│ ██████████
└──────────────────────
```

Histograms can help analyze:

```text
Brightness
Contrast
Exposure
Threshold selection
```

---

# Color Spaces

OpenCV supports multiple color representations.

Common examples include:

```text
BGR
RGB
Grayscale
HSV
LAB
```

Different representations can make different computer vision tasks easier.

---

# HSV

HSV represents:

```text
H → Hue
S → Saturation
V → Value
```

Convert BGR to HSV:

```python
hsv = cv2.cvtColor(
    image,
    cv2.COLOR_BGR2HSV
)
```

HSV is often useful for color-based segmentation.

---

# Color-Based Segmentation

For example, a range of HSV values can be selected:

```python
mask = cv2.inRange(
    hsv,
    lower,
    upper
)
```

Pipeline:

```text
IMAGE
  ↓
HSV
  ↓
COLOR RANGE
  ↓
MASK
```

This is a simple example of segmentation using traditional computer vision.

---

# Drawing on Images

OpenCV can draw:

```text
Lines
Rectangles
Circles
Polygons
Text
```

Example:

```python
cv2.rectangle(
    image,
    (50, 50),
    (200, 200),
    (0, 255, 0),
    2
)
```

This is useful for visualization and debugging.

---

# Adding Text

```python
cv2.putText(
    image,
    "Object",
    (50, 40),
    cv2.FONT_HERSHEY_SIMPLEX,
    1,
    (0, 255, 0),
    2
)
```

This is commonly used to display:

```text
Class
Confidence
Coordinates
FPS
Debug information
```

---

# Webcam Processing

OpenCV can capture frames from a camera:

```python
cap = cv2.VideoCapture(0)
```

A simple loop:

```python
while True:
    ret, frame = cap.read()

    if not ret:
        break

    cv2.imshow(
        "Camera",
        frame
    )

    if cv2.waitKey(1) & 0xFF == ord("q"):
        break

cap.release()
cv2.destroyAllWindows()
```

The pipeline becomes:

```text
CAMERA
  ↓
FRAME
  ↓
OPENCV
  ↓
PROCESSING
  ↓
DISPLAY
  ↓
NEXT FRAME
```

---

# Video Processing

OpenCV can also process video files.

```text
VIDEO
  ↓
FRAME 1
FRAME 2
FRAME 3
...
FRAME N
```

Each frame can be processed independently.

This makes OpenCV useful for:

```text
Object Detection
Tracking
Motion Detection
Video Analytics
Frame Extraction
```

---

# OpenCV + NumPy

OpenCV images are NumPy arrays.

This makes operations such as slicing and mathematical transformations easy.

Example:

```python
import numpy as np
import cv2

image = cv2.imread("image.jpg")

print(image.shape)
print(image.dtype)
```

Typical output might look like:

```text
(480, 640, 3)
uint8
```

---

# Data Types

A common OpenCV image uses:

```text
uint8
```

with values:

```text
0 → 255
```

When performing numerical operations, be careful about data types and value ranges.

For machine-learning pipelines, images are often converted to:

```text
float32
```

and normalized.

---

# OpenCV + Deep Learning

OpenCV can prepare images before they enter a neural network.

For example:

```text
IMAGE
  ↓
OpenCV
  ├── Resize
  ├── Crop
  ├── Color Conversion
  └── Normalize
  ↓
PyTorch / TensorFlow
  ↓
MODEL
  ↓
PREDICTION
```

OpenCV does not replace the deep-learning model.

It often acts as the preprocessing and computer-vision utility layer around the model.

---

# OpenCV + YOLO

A practical object detection application may combine:

```text
Camera
  ↓
OpenCV
  ↓
Frame
  ↓
YOLO
  ↓
Bounding Boxes
  ↓
OpenCV Drawing
  ↓
Display
```

This is a common architecture for real-time computer vision applications.

---

# OpenCV + Segmentation

OpenCV can also support segmentation workflows.

For example:

```text
IMAGE
  ↓
OpenCV Preprocessing
  ↓
U-Net
  ↓
MASK
  ↓
OpenCV Post-processing
  ↓
OVERLAY
```

This is useful for visualization and mask cleanup.

---

# Practical Preprocessing Pipeline

A reusable preprocessing pipeline might be:

```text
INPUT IMAGE
     ↓
READ
     ↓
CHECK
     ↓
RESIZE
     ↓
COLOR CONVERSION
     ↓
DENOISE
     ↓
NORMALIZE
     ↓
MODEL INPUT
```

Not every project needs every step.

The preprocessing should be designed around the problem and the model.

---

# Example Preprocessing Function

```python
import cv2
import numpy as np

def preprocess_image(path):
    image = cv2.imread(path)

    if image is None:
        raise ValueError("Unable to load image")

    image = cv2.cvtColor(
        image,
        cv2.COLOR_BGR2RGB
    )

    image = cv2.resize(
        image,
        (224, 224)
    )

    image = image.astype(
        np.float32
    ) / 255.0

    return image
```

This produces an RGB, normalized 224 × 224 image.

The exact pipeline must match the model's expected input.

---

# Common OpenCV Problems

## Image Appears Blue or Red

Likely cause:

```text
BGR vs RGB mismatch
```

Fix:

```python
rgb = cv2.cvtColor(
    image,
    cv2.COLOR_BGR2RGB
)
```

---

## Image Cannot Be Loaded

Check:

```text
File path
File extension
Permissions
Current working directory
File existence
```

Example:

```python
if image is None:
    raise ValueError("Image could not be loaded")
```

---

## Webcam Does Not Open

Check:

```text
Camera permissions
Camera index
Another application using the camera
Operating system access
```

---

## Model Gives Unexpected Results

Inspect:

```text
Image color format
Image dimensions
Normalization
Channel order
Resize method
Training preprocessing
```

The model's preprocessing and inference preprocessing must match.

---

# OpenCV Performance

For real-time systems, performance matters.

Important factors include:

```text
Image resolution
Frame rate
Algorithm complexity
CPU utilization
GPU acceleration
Model inference time
Memory usage
```

A common optimization strategy is:

```text
Capture
  ↓
Resize
  ↓
Process
  ↓
Display
```

rather than processing unnecessarily large frames.

---

# Computer Vision Pipeline

OpenCV often sits near the beginning and end of a vision system:

```text
             CAMERA / IMAGE
                    │
                    ▼
                 OpenCV
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
    PREPROCESSING         TRADITIONAL CV
          │                   │
          ▼                   ▼
      ML MODEL            FEATURES
          │
          ▼
     PREDICTION
          │
          ▼
        OpenCV
          │
          ▼
     VISUALIZATION
```

---

# Practical OpenCV Toolkit

The most useful operations to remember are:

```text
cv2.imread()
cv2.imwrite()
cv2.cvtColor()
cv2.resize()
cv2.flip()
cv2.rotate()
cv2.GaussianBlur()
cv2.medianBlur()
cv2.threshold()
cv2.adaptiveThreshold()
cv2.Canny()
cv2.findContours()
cv2.boundingRect()
cv2.inRange()
cv2.VideoCapture()
cv2.rectangle()
cv2.putText()
```

These operations cover a large portion of everyday image-processing tasks.

---

# Practical Checklist

## Image Handling

```text
□ Load image
□ Check for None
□ Inspect shape
□ Inspect dtype
□ Check color format
□ Save output
```

## Preprocessing

```text
□ Resize
□ Crop
□ Convert color
□ Normalize
□ Denoise when required
□ Preserve masks correctly
```

## Traditional Computer Vision

```text
□ Threshold
□ Detect edges
□ Find contours
□ Analyze shapes
□ Use morphology
□ Generate masks
```

## Model Integration

```text
□ Match training preprocessing
□ Convert BGR → RGB when required
□ Match input resolution
□ Match dtype
□ Match normalization
□ Validate inference output
```

---

# Key Takeaways

```text
✓ OpenCV is a major computer vision toolkit

✓ OpenCV images are commonly represented as NumPy arrays

✓ OpenCV normally uses BGR channel ordering

✓ Color conversion is important when working with ML libraries

✓ Resizing and normalization prepare images for models

✓ Filtering can reduce noise

✓ Thresholding can create binary masks

✓ Canny detects edges

✓ Contours provide useful shape information

✓ Morphological operations can clean binary masks

✓ HSV can be useful for color-based segmentation

✓ OpenCV can process images, video and camera streams

✓ OpenCV works well alongside PyTorch, TensorFlow and YOLO

✓ Robust preprocessing is essential for reliable model inference
```

---

# Related Experiments

- [Computer Vision Fundamentals →](/labs/computer-vision/fundamentals/)
- [Image Classification →](/labs/computer-vision/image-classification/)
- [Object Detection →](/labs/computer-vision/object-detection/)
- [Image Segmentation →](/labs/computer-vision/image-segmentation/)
- [YOLO Experiments →](/labs/computer-vision/yolo/)
- [U-Net Experiments →](/labs/computer-vision/u-net/)
- [Image Preprocessing →](/labs/computer-vision/preprocessing/)
- [Model Evaluation →](/labs/computer-vision/model-evaluation/)

---

> **Good computer vision often starts before the model — with the quality of the pixels entering it.**
