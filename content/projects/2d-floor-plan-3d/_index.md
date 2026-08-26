---
title: " 🏠 2D Floor Plan Generator"
description: "A Streamlit-based application for generating and analyzing 2D floor plans using machine learning, OpenCV and image processing."
weight: 30
toc: true
cascade:
  type: docs
---

> **A machine-learning and computer-vision application for generating and analyzing 2D floor plans.**

The 2D Floor Plan Generator is an interactive web application built with **Python and Streamlit** that explores how machine learning and image-processing techniques can be applied to floor-plan generation and analysis.

The application allows users to work with floor-plan images or structural input data and visualize the resulting layouts.

The project focuses on combining:

```text
Input Data
    ↓
Image Processing
    ↓
Machine Learning
    ↓
Floor Plan Generation
    ↓
Visualization
```

---

## Project Status

**Status: 🟢 Implemented / Experimental**

The current project provides a Streamlit-based application with:

- 2D floor-plan generation
- Image processing
- OpenCV integration
- Machine-learning model integration
- Interactive visualization
- Matplotlib-based rendering
- Deployment support for Hugging Face Spaces and Streamlit Cloud

Advanced capabilities such as 3D floor-plan generation, AI-based room detection, real-time camera input and CAD export are currently listed as **future improvements**.

---

## Problem Statement

Creating and analyzing floor plans involves understanding spatial structure, rooms, boundaries and layout information.

The goal of this project is to explore how computer vision and machine-learning techniques can assist with the generation and visualization of 2D floor-plan layouts.

Instead of creating a static drawing application, the project investigates a workflow where input data is processed computationally and transformed into a visual floor-plan representation.

---

## What the System Does

The application provides an interactive interface where users can provide input data or images and generate and visualize 2D floor-plan layouts.

The high-level workflow is:

```text
INPUT
  ↓
DATA / IMAGE
  ↓
OPENCV PROCESSING
  ↓
ML MODEL
  ↓
FLOOR PLAN GENERATION
  ↓
MATPLOTLIB VISUALIZATION
  ↓
STREAMLIT UI
```

The application is designed to provide fast visual feedback through an interactive Streamlit interface.

---

## Core Features

### 🏠 2D Floor Plan Generation

The application can generate 2D floor plans from input data.

The generated layout can then be visualized through the Streamlit interface.

### 📷 Image Processing

**OpenCV** is used as part of the image-processing workflow.

This provides the foundation for processing floor-plan imagery and extracting useful information for downstream processing.

### 🤖 Machine Learning

The project includes machine-learning model integration.

The repository provides a dedicated model directory:

```text
model/
```

This allows the application to load and use trained model components as part of the processing workflow.

### 📊 Visualization

The generated floor-plan information is visualized using **Matplotlib**.

This provides a graphical representation of the processed layout and allows users to inspect the generated result.

### ⚡ Interactive Application

The application is built using **Streamlit**.

```text
User Input
    ↓
Processing
    ↓
Visualization
    ↓
Interactive Result
```

---

## System Architecture

The current application can be represented as:

```text
┌─────────────────────────┐
│          USER           │
│                         │
│ Image / Structural Data │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│      STREAMLIT UI       │
│                         │
│ Input + Visualization   │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│    IMAGE PROCESSING     │
│                         │
│        OpenCV           │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│    MACHINE LEARNING     │
│                         │
│      Model Layer        │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│    FLOOR PLAN OUTPUT    │
│                         │
│      2D Layout          │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│     VISUALIZATION       │
│                         │
│       Matplotlib        │
└─────────────────────────┘
```

---

## Processing Pipeline

The project combines image processing and machine learning into a simple processing pipeline.

```text
RAW INPUT
   ↓
IMAGE / STRUCTURAL DATA
   ↓
PREPROCESSING
   ↓
OPENCV
   ↓
MODEL PROCESSING
   ↓
2D FLOOR PLAN
   ↓
VISUALIZATION
```

The exact model architecture and training methodology are intentionally not overstated because the current project README does not specify a particular trained model architecture.

---

## Technology Stack

### Programming

```text
Python
```

### Application Framework

```text
Streamlit
```

### Computer Vision

```text
OpenCV
```

### Numerical Processing

```text
NumPy
Pandas
```

### Visualization

```text
Matplotlib
```

### Machine Learning

```text
Scikit-learn
TensorFlow
```

The README identifies Scikit-learn / TensorFlow as model-support technologies depending on the implementation.

---

## Project Structure

The repository follows a simple application structure:

```text
2D-Floor-Plan-3D/

├── app.py
├── requirements.txt
├── model/
│   └── trained ML model
├── data/
│   └── input / sample data
├── utils/
│   └── helper functions
└── README.md
```

The main application entry point is:

```text
app.py
```

---

## Application Workflow

From the user's perspective, the application follows:

```text
OPEN APPLICATION
       ↓
PROVIDE INPUT
       ↓
PROCESS FLOOR PLAN
       ↓
RUN ML / IMAGE PROCESSING
       ↓
GENERATE 2D LAYOUT
       ↓
VISUALIZE RESULT
```

The application is designed to make this workflow accessible through a browser-based Streamlit interface.

---

## Local Development

The application can be run locally using Python.

### Create Environment

```bash
python -m venv .venv
```

Windows:

```powershell
.venv\Scripts\activate
```

Linux / macOS:

```bash
source .venv/bin/activate
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Run Application

```bash
streamlit run app.py
```

The application runs locally at:

```text
http://localhost:8501
```

---

## Deployment

The project is designed to be deployable on cloud-based Streamlit environments.

### Hugging Face Spaces

The project can be deployed using a Streamlit Space.

The repository README provides the following configuration:

```yaml
---
title: 2D Floor Plan Generator
emoji: 🏠
colorFrom: blue
colorTo: green
sdk: streamlit
app_file: app.py
---
```

### Streamlit Cloud

The project can also be deployed by connecting the GitHub repository to Streamlit Cloud and selecting:

```text
app.py
```

as the application entry point.

---

## Deployment Considerations

The project README highlights several practical deployment considerations.

### OpenCV

For deployment environments, the README recommends:

```text
opencv-python-headless
```

instead of:

```text
opencv-python
```

### Relative Paths

Absolute paths such as:

```text
C:\Users\...
```

should be avoided.

Instead, the application should use relative paths such as:

```text
./model/model.h5
```

This makes the application more portable across development and deployment environments.

### Large Models

For large model files, the project recommends external storage such as:

```text
Google Drive
Hugging Face
```

with the model loaded dynamically.

---

## Engineering Considerations

### Reproducibility

The application uses:

```text
requirements.txt
```

to define its Python dependencies.

This provides a reproducible environment for local and cloud deployment.

### Portable Application Paths

Using relative paths instead of machine-specific absolute paths makes the application easier to deploy across:

```text
Windows
Linux
Cloud environments
Hugging Face Spaces
Streamlit Cloud
```

### Interactive Visualization

Using Streamlit and Matplotlib allows the project to expose the generated floor plan directly through an interactive web interface.

This reduces the gap between:

```text
Machine Learning Model
        ↓
Application
        ↓
Human Visualization
```

---

## Current Implementation

### 🟢 Implemented

- 2D floor-plan generation
- Input-data processing
- Image processing using OpenCV
- Machine-learning model integration
- NumPy / Pandas processing
- Matplotlib visualization
- Streamlit application
- Local execution
- Hugging Face Spaces deployment support
- Streamlit Cloud deployment support

---

## Future Development

The project README identifies several future improvements.

### 🧊 3D Floor Plan Generation

Extend the current 2D workflow into a three-dimensional representation.

```text
2D FLOOR PLAN
      ↓
STRUCTURAL INTERPRETATION
      ↓
3D FLOOR PLAN
```

This would extend the project toward interactive architectural visualization.

### 📷 Real-Time Camera Input

Add support for real-time camera input.

```text
CAMERA
   ↓
IMAGE CAPTURE
   ↓
PROCESSING
   ↓
FLOOR PLAN ANALYSIS
```

### 🤖 AI-Based Room Detection

Introduce automated detection of rooms and structural elements using AI.

```text
FLOOR PLAN IMAGE
       ↓
AI DETECTION
       ↓
ROOM IDENTIFICATION
       ↓
STRUCTURAL MAP
```

### 📐 CAD Export

Provide the ability to export generated floor plans into CAD-compatible formats.

These capabilities are **planned**, not presented as currently implemented.

---

## Current vs Future Scope

| Capability | Status |
|---|---|
| 2D Floor Plan Generation | 🟢 Implemented |
| OpenCV Processing | 🟢 Implemented |
| ML Integration | 🟢 Implemented |
| Matplotlib Visualization | 🟢 Implemented |
| Streamlit Interface | 🟢 Implemented |
| Hugging Face Deployment | 🟢 Supported |
| Streamlit Cloud Deployment | 🟢 Supported |
| 3D Floor Plan Generation | 🔵 Planned |
| Real-Time Camera Input | 🔵 Planned |
| AI Room Detection | 🔵 Planned |
| CAD Export | 🔵 Planned |

---

## Project Philosophy

> **The goal is not simply to draw a floor plan.**
>
> It is to explore how data, computer vision and machine learning can be combined to transform structural information into a useful visual representation.

```text
INPUT
  ↓
UNDERSTAND
  ↓
PROCESS
  ↓
GENERATE
  ↓
VISUALIZE
```

---

## Engineering Journey

This project demonstrates several practical engineering concepts:

```text
Problem Definition
       ↓
Application Design
       ↓
Image Processing
       ↓
Machine Learning
       ↓
Visualization
       ↓
Interactive UI
       ↓
Deployment
       ↓
Future 3D / AI Extensions
```

The emphasis is on building a working application around machine-learning and computer-vision components rather than keeping the work isolated inside a notebook.

---

## Repository

[View the 2D Floor Plan Generator repository](https://github.com/maheshkol/2D-Floor-Plan-3D/)

---

## Project Classification

| Area | Details |
|---|---|
| Domain | Computer Vision + Machine Learning |
| Problem | 2D floor-plan generation and analysis |
| Input | Images / structural data |
| Processing | OpenCV |
| ML | Scikit-learn / TensorFlow |
| Visualization | Matplotlib |
| Application | Streamlit |
| Deployment | Hugging Face Spaces / Streamlit Cloud |
| Current Scope | 2D floor plans |
| Future Scope | 3D, room detection, camera input, CAD |
| Status | 🟢 Implemented / Experimental |

---

## Author

**Mahesh Kolekar**
