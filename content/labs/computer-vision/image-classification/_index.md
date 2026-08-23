---
title: "Image Classification"
description: "Practical image classification using datasets, preprocessing, CNNs, training, evaluation, and inference."
weight: 20
toc: true
---

> **Image classification answers one fundamental question: what does this image contain?**

Image classification assigns an input image to one of a predefined set of classes.

A simple classification pipeline is:

```text
IMAGE
  ↓
PREPROCESSING
  ↓
MODEL
  ↓
CLASS PROBABILITIES
  ↓
PREDICTED CLASS
```

---

# What is Image Classification?

Suppose we have three grain classes:

```text
Rice
Wheat
Lentils
```

The model receives an image and predicts which class it belongs to.

```text
Input Image
     ↓
   CNN
     ↓
┌───────────────┐
│ Rice     0.10 │
│ Wheat    0.05 │
│ Lentils  0.85 │
└───────────────┘
     ↓
Prediction: Lentils
```

The model does not simply memorize the name of the object.

During training, it learns visual patterns that help distinguish the classes.

---

# Classification Problem

A supervised classification problem can be represented as:

```text
Image → Label
```

For example:

```text
grain_001.jpg → rice
grain_002.jpg → wheat
grain_003.jpg → lentils
```

The training dataset contains many such examples.

The model learns a relationship between:

```text
IMAGE FEATURES → CLASS
```

---

# Dataset Structure

A simple directory-based dataset can look like:

```text
dataset/
│
├── rice/
│   ├── image001.jpg
│   ├── image002.jpg
│   └── image003.jpg
│
├── wheat/
│   ├── image001.jpg
│   ├── image002.jpg
│   └── image003.jpg
│
└── lentils/
    ├── image001.jpg
    ├── image002.jpg
    └── image003.jpg
```

The directory names can represent the class labels.

For a larger project, a separate metadata file can also be used.

---

# The Importance of the Dataset

Model performance is strongly influenced by dataset quality.

Before training, inspect:

```text
Number of images
Image dimensions
Class distribution
Image quality
Duplicates
Incorrect labels
Missing files
Background differences
Lighting differences
```

A model cannot compensate for every problem in the dataset.

---

# Class Balance

Suppose a dataset contains:

```text
Rice      5000 images
Wheat      800 images
Lentils    200 images
```

The classes are highly imbalanced.

A model may become biased toward the majority class.

A healthier dataset might have a more balanced distribution:

```text
Rice      2000
Wheat     2000
Lentils   2000
```

Perfect balance is not always necessary, but class distribution should always be inspected.

---

# Train, Validation and Test Sets

A typical dataset is divided into:

```text
DATASET
   │
   ├── Training
   │
   ├── Validation
   │
   └── Test
```

Their purposes are different.

### Training

Used to learn model parameters.

### Validation

Used during development to evaluate model behavior and tune the system.

### Test

Used for final evaluation on unseen data.

---

# Example Split

A common starting point might be:

```text
70% → Training
15% → Validation
15% → Test
```

The exact split depends on dataset size, problem type and experimental design.

For related images, such as multiple images from the same physical object, care must be taken to avoid leakage between splits.

---

# Image Preprocessing

Before training, images may need to be:

```text
Load
 ↓
Resize
 ↓
Convert color format
 ↓
Normalize
 ↓
Augment
 ↓
Batch
 ↓
Model
```

For example, a model may expect:

```text
224 × 224 × 3
```

Therefore every input image must be transformed into a compatible representation.

---

# Normalization

Raw RGB values commonly range from:

```text
0 → 255
```

A simple normalization approach is:

```python
image = image / 255.0
```

which converts the approximate range to:

```text
0 → 1
```

The preprocessing used during inference must be consistent with preprocessing used during training.

---

# Data Augmentation

Training images can sometimes be transformed to create additional variation.

Examples:

```text
Rotation
Horizontal Flip
Vertical Flip
Zoom
Crop
Translation
Brightness
Contrast
```

Conceptually:

```text
Original Image
      │
 ┌────┼────┐
 ↓    ↓    ↓
Rotate Flip Zoom
 │    │    │
 └────┼────┘
      ↓
Training Examples
```

Augmentation should reflect realistic variations in the real-world data.

---

# CNN for Classification

A basic CNN classification architecture may look like:

```text
INPUT IMAGE
     ↓
CONVOLUTION
     ↓
RELU
     ↓
POOLING
     ↓
CONVOLUTION
     ↓
RELU
     ↓
POOLING
     ↓
FLATTEN / GLOBAL POOLING
     ↓
DENSE LAYER
     ↓
OUTPUT
```

The convolution layers learn visual features.

The final layers convert those features into class predictions.

---

# Softmax Output

For a multi-class classification problem, the final layer commonly produces probabilities.

For example:

```text
Rice       0.12
Wheat      0.73
Lentils    0.15
```

The probabilities sum approximately to:

```text
1.0
```

The highest probability becomes the predicted class.

```text
Prediction = Wheat
```

---

# Training

During training, the model repeatedly processes batches of images.

A simplified training loop is:

```text
IMAGE BATCH
     ↓
FORWARD PASS
     ↓
PREDICTION
     ↓
LOSS
     ↓
BACKPROPAGATION
     ↓
WEIGHT UPDATE
     ↓
NEXT BATCH
```

This process repeats over multiple epochs.

---

# Epochs

An epoch represents one complete pass through the training dataset.

For example:

```text
Epoch 1
Epoch 2
Epoch 3
...
Epoch 20
```

More epochs do not automatically mean a better model.

The model should be monitored for both training and validation performance.

---

# Loss Function

Loss measures how different the model prediction is from the expected label.

For multi-class classification, a common choice is:

```text
Categorical Cross-Entropy
```

or:

```text
Sparse Categorical Cross-Entropy
```

depending on how labels are represented.

Conceptually:

```text
Correct prediction
       ↓
Lower loss

Incorrect prediction
       ↓
Higher loss
```

---

# Accuracy

Accuracy is:

```text
Correct Predictions
────────────────────
Total Predictions
```

For example:

```text
90 correct
100 total
```

gives:

```text
Accuracy = 90%
```

Accuracy is useful, but it should not be the only metric when classes are imbalanced.

---

# Precision

Precision asks:

> Of the samples predicted as a particular class, how many were actually that class?

```text
Precision =
True Positives
────────────────────────
True Positives + False Positives
```

---

# Recall

Recall asks:

> Of all actual samples belonging to a class, how many did the model find?

```text
Recall =
True Positives
────────────────────────
True Positives + False Negatives
```

---

# F1 Score

F1 combines precision and recall:

```text
F1 = 2 × (Precision × Recall)
     ─────────────────────────
       Precision + Recall
```

It can be useful when both false positives and false negatives matter.

---

# Confusion Matrix

A confusion matrix provides a class-by-class view of predictions.

For three classes:

```text
                 PREDICTED
              Rice Wheat Lentils

ACTUAL Rice     90    5      5
       Wheat     4   92      4
       Lentils   3    6     91
```

The diagonal represents correct predictions.

```text
Rice → Rice
Wheat → Wheat
Lentils → Lentils
```

Off-diagonal values represent errors.

---

# Why Confusion Matrices Matter

Overall accuracy can hide important problems.

For example:

```text
Accuracy = 92%
```

looks good.

But the confusion matrix might reveal:

```text
Rice → Wheat
Wheat → Rice
```

happening frequently.

This can indicate that the two classes have visually similar characteristics.

---

# Overfitting

A classification model may perform extremely well on training images while performing poorly on unseen images.

Example:

```text
Training Accuracy   99%
Validation Accuracy 72%
```

This is a warning sign of overfitting.

Possible solutions include:

```text
More data
Data augmentation
Regularization
Dropout
Early stopping
Transfer learning
Reducing model complexity
```

---

# Underfitting

Underfitting can occur when the model cannot learn enough useful patterns.

Example:

```text
Training Accuracy   62%
Validation Accuracy 60%
```

Possible causes include:

```text
Model too simple
Insufficient training
Poor preprocessing
Weak features
Learning rate issues
Insufficient data
```

---

# Transfer Learning

Training a deep CNN completely from scratch can require substantial data and compute.

Transfer learning starts with a model that has already learned useful visual features.

Conceptually:

```text
Pretrained Model
      ↓
Learned Visual Features
      ↓
Replace / Adapt Classifier
      ↓
Train on New Dataset
      ↓
New Classification Model
```

Common pretrained architectures include:

```text
ResNet
EfficientNet
MobileNet
VGG
ConvNeXt
```

The appropriate model depends on the task and deployment constraints.

---

# Practical Grain Classification Example

A practical example is classifying grains such as:

```text
Rice
Wheat
Lentils
```

The pipeline becomes:

```text
GRAIN IMAGE
     ↓
RESIZE
     ↓
NORMALIZE
     ↓
AUGMENTATION
     ↓
CNN / TRANSFER LEARNING
     ↓
CLASS PROBABILITIES
     ↓
PREDICTED GRAIN
```

Example:

```text
Input:
grain_sample_023.jpg

Output:
Rice       0.08
Wheat      0.11
Lentils    0.81

Prediction:
Lentils
```

---

# Real-World Dataset Problems

A model may fail even when the code is technically correct.

Common causes include:

```text
Poor lighting
Different backgrounds
Camera angle changes
Blurred images
Incorrect labels
Class imbalance
Too few examples
Duplicate images
Data leakage
Domain shift
```

For example, if all training images of wheat have a particular background, the model may accidentally learn the background instead of the grain itself.

---

# Model Evaluation Workflow

After training:

```text
TRAINED MODEL
      ↓
VALIDATION DATA
      ↓
Predictions
      ↓
Confusion Matrix
      ↓
Accuracy / Precision / Recall / F1
      ↓
Error Analysis
```

Do not stop at a single accuracy number.

Inspect the actual errors.

---

# Error Analysis

A useful classification workflow includes examining incorrect predictions.

For every error, ask:

```text
What was the actual class?
What did the model predict?
Was the image difficult?
Was the label correct?
Was the image poorly exposed?
Was the class visually similar?
Was there a preprocessing issue?
```

This often provides more useful information than simply increasing the number of training epochs.

---

# Inference

Once training is complete, the model can classify a new image.

```text
NEW IMAGE
    ↓
SAME PREPROCESSING
    ↓
TRAINED MODEL
    ↓
PROBABILITIES
    ↓
PREDICTED CLASS
```

The preprocessing must match the training pipeline.

---

# Example Inference Code

A simplified TensorFlow/Keras inference workflow:

```python
from tensorflow.keras.utils import load_img, img_to_array
import numpy as np

image = load_img(
    "sample.jpg",
    target_size=(224, 224)
)

image_array = img_to_array(image)

image_array = image_array / 255.0

image_array = np.expand_dims(
    image_array,
    axis=0
)

predictions = model.predict(image_array)

predicted_class = np.argmax(predictions[0])

print(predicted_class)
```

The exact preprocessing and class mapping must match the training configuration.

---

# Class Mapping

A model's numeric output needs to be mapped back to a human-readable label.

For example:

```python
class_names = [
    "lentils",
    "rice",
    "wheat"
]
```

If:

```python
predicted_class = 2
```

then:

```python
print(class_names[predicted_class])
```

produces:

```text
wheat
```

The class ordering must remain consistent between training and inference.

---

# Deployment

A classification model can be exposed through an application.

For example:

```text
USER
  ↓
UPLOAD IMAGE
  ↓
STREAMLIT / API
  ↓
PREPROCESSING
  ↓
MODEL
  ↓
PREDICTION
  ↓
RESULT
```

Possible deployment technologies include:

```text
Streamlit
FastAPI
Docker
Hugging Face Spaces
Cloud platforms
Raspberry Pi / Edge devices
```

---

# Edge Deployment

For constrained hardware, a trained model may need optimization.

A common approach is TensorFlow Lite:

```text
TRAINING MODEL
      ↓
OPTIMIZATION
      ↓
TFLite MODEL
      ↓
EDGE DEVICE
      ↓
INFERENCE
```

This can reduce the resource requirements of the deployed model.

---

# Complete Classification Architecture

```text
                    IMAGE CLASSIFICATION
                            │
                            ▼
                         DATASET
                            │
                            ▼
                     DATA EXPLORATION
                            │
                            ▼
                      PREPROCESSING
                            │
                            ▼
                     AUGMENTATION
                            │
                            ▼
                   TRAIN / VALIDATION
                            │
                            ▼
                       CNN / MODEL
                            │
                            ▼
                       PREDICTION
                            │
                ┌───────────┴───────────┐
                ▼                       ▼
          CLASS PROBABILITIES      EVALUATION
                                        │
                              ┌─────────┼─────────┐
                              ▼         ▼         ▼
                           Accuracy  F1      Confusion
                                                Matrix
                                        │
                                        ▼
                                    DEPLOYMENT
```

---

# Practical Checklist

Before training:

```text
□ Inspect dataset
□ Verify class labels
□ Check class distribution
□ Remove duplicates
□ Check image quality
□ Define train/validation/test split
□ Define preprocessing
□ Define augmentation
```

During training:

```text
□ Monitor training loss
□ Monitor validation loss
□ Monitor accuracy
□ Watch for overfitting
□ Save the best model
```

After training:

```text
□ Evaluate on unseen data
□ Generate confusion matrix
□ Calculate precision / recall / F1
□ Inspect incorrect predictions
□ Test real-world images
□ Verify inference preprocessing
```

Before deployment:

```text
□ Save class mapping
□ Optimize model if required
□ Test inference pipeline
□ Measure latency
□ Test different image conditions
□ Package model and dependencies
```

---

# Key Takeaways

```text
✓ Classification maps an image to a class

✓ Dataset quality is critical

✓ Train, validation and test data serve different purposes

✓ Preprocessing must be consistent

✓ Augmentation can improve generalization

✓ CNNs learn visual features automatically

✓ Accuracy alone is not enough

✓ Confusion matrices reveal class-specific errors

✓ Error analysis is essential

✓ Transfer learning can reduce training requirements

✓ Inference must use the same preprocessing as training

✓ Deployment turns the model into a usable system
```

---

# Related Experiments

- [Computer Vision Fundamentals →](/labs/computer-vision/fundamentals/)
- [Object Detection →](/labs/computer-vision/object-detection/)
- [Image Segmentation →](/labs/computer-vision/image-segmentation/)
- [YOLO Experiments →](/labs/computer-vision/yolo/)
- [U-Net Experiments →](/labs/computer-vision/u-net/)
- [OpenCV Experiments →](/labs/computer-vision/opencv/)
- [Data Augmentation →](/labs/computer-vision/data-augmentation/)
- [Model Evaluation →](/labs/computer-vision/model-evaluation/)

---

> **A classifier is only as useful as the data, evaluation and real-world testing behind it.**
