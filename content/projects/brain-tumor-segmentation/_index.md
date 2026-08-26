---
title: "🧠 Brain Tumor Segmentation"
description: "An end-to-end medical imaging application for brain tumor segmentation from MRI scans using U-Net and Attention U-Net."
weight: 20
toc: false
---

> **A deep learning project for automated brain tumor segmentation from MRI scans.**

Brain Tumor Segmentation is an end-to-end medical AI application that explores how deep learning can be used to identify and segment tumor regions in brain MRI scans.

The project combines **medical image processing, semantic segmentation, deep learning, backend APIs and a web-based interface** into a complete machine-learning system.

The implementation uses **U-Net and Attention U-Net** models and provides a workflow for uploading MRI data, preprocessing the image, running model inference, generating a tumor mask and visualizing the result.

---

## Project Status

**Status: 🟢 Implemented / Research Project**

The core application architecture, model inference workflow, frontend, backend and visualization pipeline are implemented.

The project also contains experimental notebooks, trained model weights and Docker configuration.

Several advanced capabilities remain planned, including multi-modal MRI fusion, 3D U-Net and additional explainability and deployment features.

---

## Problem Statement

Manual brain tumor segmentation is a time-consuming medical imaging task.

Tumor regions can vary significantly in:

- Shape
- Size
- Location
- Intensity
- Appearance across MRI modalities

The objective of this project is to explore an automated segmentation workflow using convolutional neural networks.

The system takes MRI data as input and produces a predicted tumor segmentation mask that can be visualized against the original MRI slice.

This project is intended for **research and educational purposes**, not clinical diagnosis.

---

## What the System Does

The application follows this workflow:

```text
MRI Upload
    ↓
MRI Preprocessing
    ↓
Normalization
    ↓
Resize
    ↓
U-Net / Attention U-Net
    ↓
Probability Mask
    ↓
Thresholding
    ↓
Binary Tumor Mask
    ↓
MRI Overlay
    ↓
Visualization
```

The backend extracts a representative slice from the MRI volume, performs Z-score normalization and resizing, then passes the processed image to the segmentation model.

The model produces a probability mask which is subsequently thresholded into a binary tumor mask and overlaid on the original MRI slice.

---

## Dataset

The project is designed around the **BraTS — Brain Tumor Segmentation Dataset**.

The dataset contains multi-modal MRI scans including:

- T1
- T1ce
- T2
- FLAIR

Ground-truth annotations include:

- Whole Tumor — WT
- Tumor Core — TC
- Enhancing Tumor — ET

The dataset uses the **NIfTI `.nii.gz` format**.

The complete BraTS dataset is not included in the repository. Sample MRI files can be placed in the project's sample-data directory.

---

## System Architecture

The application follows a frontend → API → inference architecture:

```text
┌──────────────────────┐
│       User           │
│     MRI Upload       │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Streamlit Frontend   │
└──────────┬───────────┘
           │
           │ REST API
           ▼
┌──────────────────────┐
│   FastAPI Backend    │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Preprocessing        │
│ • MRI slice          │
│ • Normalization      │
│ • Resize             │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Segmentation Model   │
│ U-Net                │
│ Attention U-Net      │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Post-processing      │
│ Probability → Mask   │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Visualization        │
│ Mask + MRI Overlay   │
└──────────────────────┘
```

The repository separates the frontend, FastAPI backend, model weights, sample data, notebooks and Docker configuration.

---

## Model Architecture

### U-Net

The primary segmentation architecture is **U-Net**, a convolutional neural network architecture designed for image segmentation.

The general concept is:

```text
Input Image
     ↓
Encoder
     ↓
Feature Extraction
     ↓
Bottleneck
     ↓
Decoder
     ↓
Segmentation Mask
```

The encoder extracts increasingly abstract image features while the decoder reconstructs the spatial representation required for pixel-level segmentation.

Skip connections help preserve spatial information between corresponding encoder and decoder stages.

### Attention U-Net

The project also includes an **Attention U-Net** model.

The attention mechanism is intended to help the network focus on more relevant spatial regions during segmentation.

Both model weights are included in the project's model structure:

```text
models/
├── unet.pth
└── attention_unet.pth
```

The application can perform inference using either U-Net or Attention U-Net.

---

## MRI Processing Pipeline

Before inference, the MRI data passes through preprocessing.

### 1. MRI Input

The application accepts:

```text
.nii
.nii.gz
```

MRI volumes.

### 2. Slice Extraction

A representative slice is extracted from the MRI volume for the current inference workflow.

### 3. Normalization

The image undergoes **Z-score normalization**.

This standardizes the intensity distribution before model inference.

### 4. Resizing

The processed image is resized to dimensions compatible with the segmentation model.

### 5. Model Inference

The processed image is passed to:

```text
U-Net
    OR
Attention U-Net
```

### 6. Probability Mask

The model produces a probability map representing the predicted tumor region.

### 7. Thresholding

The probability map is thresholded to produce a binary segmentation mask.

### 8. Visualization

The resulting mask is overlaid on the original MRI slice.

---

## Example Workflow

```text
UPLOAD MRI
    ↓
READ NIFTI
    ↓
EXTRACT SLICE
    ↓
NORMALIZE
    ↓
RESIZE
    ↓
MODEL INFERENCE
    ↓
GENERATE PROBABILITY MAP
    ↓
THRESHOLD
    ↓
GENERATE TUMOR MASK
    ↓
OVERLAY MASK
    ↓
DISPLAY RESULT
```

The Streamlit interface provides the user-facing application while the FastAPI service handles backend processing and inference.

---

## Evaluation

The project includes support for commonly used segmentation metrics.

### Dice Score

Dice Score measures the overlap between the predicted segmentation and the ground-truth segmentation.

```text
Dice = 2 × |Prediction ∩ Ground Truth|
       ───────────────────────────────
       |Prediction| + |Ground Truth|
```

### Intersection over Union

IoU measures the ratio between the intersection and union of the predicted and ground-truth regions.

```text
IoU = Prediction ∩ Ground Truth
      ─────────────────────────
      Prediction ∪ Ground Truth
```

The repository contains metric implementation that can be enabled when ground-truth masks are available.

---

## Application Architecture

The project is structured as a full-stack machine-learning application rather than a single training notebook.

```text
brain-tumor-segmentation-app/

├── frontend/
│   ├── app.py
│   └── ui_utils.py
│
├── backend/
│   ├── main.py
│   ├── inference.py
│   ├── preprocessing.py
│   └── metrics.py
│
├── models/
│   ├── unet.pth
│   └── attention_unet.pth
│
├── data/
│   └── sample_mri/
│
├── notebooks/
│   └── exploration.ipynb
│
├── docker/
│   ├── Dockerfile.backend
│   └── Dockerfile.frontend
│
├── training/
│
├── requirements.txt
└── README.md
```

---

## Technology Stack

### Programming

```text
Python 3.9+
```

### Deep Learning

```text
PyTorch
U-Net
Attention U-Net
```

### Medical Imaging

```text
NiBabel
OpenCV
NumPy
```

### Backend

```text
FastAPI
Uvicorn
```

### Frontend

```text
Streamlit
Pillow
Matplotlib
```

### Deployment

```text
Docker
GitHub
```

---

## Why This Project Matters

This project connects several parts of an AI system:

```text
Medical Imaging
      ↓
Data Processing
      ↓
Deep Learning
      ↓
Segmentation
      ↓
Model Inference
      ↓
REST API
      ↓
Web Application
      ↓
Visualization
```

Instead of treating the model as an isolated experiment, the project explores how a trained segmentation model can be integrated into an actual application.

---

## Engineering Challenges

### Medical Image Formats

MRI data is commonly stored in specialized formats such as NIfTI rather than ordinary image formats.

### Model Input Compatibility

The MRI data must be transformed into a representation compatible with the trained neural network:

```text
MRI Volume
    ↓
Slice
    ↓
Normalization
    ↓
Resize
    ↓
Tensor
```

### Segmentation Output

The model produces a spatial prediction representing the tumor region rather than simply returning a classification such as `Tumor / No Tumor`.

### Frontend / Backend Integration

The project separates the Streamlit user interface from the FastAPI backend, creating a more realistic application architecture.

---

## Deployment

The repository includes Docker configuration for frontend and backend components.

```text
Docker
   │
   ├── Frontend Container
   │       ↓
   │    Streamlit
   │
   └── Backend Container
           ↓
        FastAPI
           ↓
      Model Inference
```

---

## Current Implementation

### 🟢 Implemented

- MRI upload workflow
- NIfTI input handling
- MRI slice processing
- Z-score normalization
- Image resizing
- U-Net inference
- Attention U-Net inference
- Probability mask generation
- Binary tumor mask generation
- MRI overlay visualization
- Streamlit frontend
- FastAPI backend
- Segmentation metrics implementation
- Docker configuration
- Project experimentation notebooks

---

## Future Development

### Multi-modal MRI Fusion

Combining:

```text
T1
+
T2
+
FLAIR
```

to provide the model with richer MRI information.

### 3D U-Net

Moving from slice-based processing toward volumetric 3D segmentation.

### Real-time Metrics

Displaying Dice and IoU during or after inference when ground-truth masks are available.

### Explainability

Exploring Grad-CAM and related techniques to improve model interpretability.

### Cloud Deployment

Potential deployment on AWS or GCP.

### PACS Integration

Potential integration with medical imaging infrastructure such as PACS systems.

These are **planned enhancements**, not capabilities presented as currently implemented.

---

## Research & Ethical Boundary

This project is intended for:

- Research
- Education
- Machine-learning experimentation
- Medical imaging engineering exploration

It is **not intended for clinical diagnosis**.

Any real-world clinical deployment would require substantially more validation, regulatory consideration, dataset governance and clinical evaluation than this project provides.

---

## Project Philosophy

> **The goal is not simply to segment a tumor.**
>
> It is to understand the complete system required to transform medical imaging data into a useful machine-learning application.

```text
DATA
 ↓
PREPROCESSING
 ↓
MODEL
 ↓
INFERENCE
 ↓
POST-PROCESSING
 ↓
API
 ↓
APPLICATION
 ↓
VISUALIZATION
 ↓
EVALUATION
```

The project sits at the intersection of **computer vision, deep learning, medical imaging and AI application engineering**.

---

## Repository

[View the Brain Tumor Segmentation repository](https://github.com/maheshkol/brain-tumor-segmentation-app)

---

## Project Classification

| Area | Details |
|---|---|
| Domain | Medical AI |
| Problem | Brain tumor segmentation |
| Input | MRI / NIfTI |
| Primary Model | U-Net |
| Additional Model | Attention U-Net |
| Framework | PyTorch |
| Backend | FastAPI |
| Frontend | Streamlit |
| Imaging | NiBabel, OpenCV, NumPy |
| Deployment | Docker |
| Evaluation | Dice, IoU |
| Status | 🟢 Implemented / Research |
