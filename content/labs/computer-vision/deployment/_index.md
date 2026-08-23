---
title: "Computer Vision Deployment"
description: "A practical guide to deploying computer vision models with PyTorch, ONNX, FastAPI, Streamlit, Docker, GPU inference, optimization, testing, and monitoring."
weight: 90
toc: true
---

> **Training creates the model. Deployment turns that model into something people and applications can actually use.**

A computer vision project is not complete when training finishes.

A practical deployment pipeline looks like:

```text
DATA
 ↓
TRAINING
 ↓
EVALUATION
 ↓
MODEL CHECKPOINT
 ↓
OPTIMIZATION
 ↓
INFERENCE SERVICE
 ↓
APPLICATION / API
 ↓
DEPLOYMENT
 ↓
MONITORING
```

---

# What is Model Deployment?

Model deployment means making a trained computer vision model available for inference.

For example:

```text
USER
 ↓
UPLOAD IMAGE
 ↓
DEPLOYED MODEL
 ↓
PREDICTION
 ↓
RESULT
```

The deployed system may run on:

```text
Local Computer
Server
Docker Container
Cloud VM
GPU Server
Edge Device
```

---

# Training vs Inference

Training:

```text
Images
  ↓
Model
  ↓
Loss
  ↓
Backpropagation
  ↓
Updated Weights
```

Inference:

```text
New Image
    ↓
Trained Model
    ↓
Prediction
```

Training is computationally expensive.

Inference is the repeated process of using the trained model to make predictions.

---

# Deployment Architecture

A basic architecture is:

```text
                 COMPUTER VISION SYSTEM
                          │
                          ▼
                       CLIENT
                          │
                          ▼
                     HTTP REQUEST
                          │
                          ▼
                       API SERVER
                          │
                          ▼
                   PREPROCESSING
                          │
                          ▼
                    MODEL INFERENCE
                          │
                          ▼
                   POST-PROCESSING
                          │
                          ▼
                       RESPONSE
```

---

# Model Serialization

The trained model must be saved before deployment.

For PyTorch, a common approach is:

```python
torch.save(
    model.state_dict(),
    "model.pt"
)
```

The deployment environment then reconstructs the architecture and loads the weights.

```python
model.load_state_dict(
    torch.load(
        "model.pt",
        map_location=device
    )
)
```

For production systems, explicitly manage model versions and compatible code/configuration.

---

# PyTorch Inference Mode

During inference, gradients are usually unnecessary.

Use:

```python
model.eval()

with torch.no_grad():
    prediction = model(image)
```

This reduces unnecessary computation and prevents training behavior such as dropout updates.

For suitable workloads, `torch.inference_mode()` can also be considered.

---

# CPU vs GPU Deployment

A model can run on CPU or GPU.

```text
CPU
 ↓
Simple deployment
Lower hardware requirement
Potentially slower inference
```

or:

```text
NVIDIA GPU
 ↓
CUDA
 ↓
PyTorch
 ↓
Model
```

GPU deployment is particularly useful for high-throughput or computationally intensive models.

---

# Device Selection

A common PyTorch pattern is:

```python
import torch

device = (
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)

print(device)
```

Then:

```python
model.to(device)
image = image.to(device)
```

Both the model and input tensors must be on compatible devices.

---

# Batch Inference

Instead of processing one image:

```text
Image 1 → Model
Image 2 → Model
Image 3 → Model
```

batch inference processes multiple images together:

```text
┌─────────┐
│ Image 1 │
├─────────┤
│ Image 2 │
├─────────┤
│ Image 3 │
└─────────┘
     ↓
   MODEL
     ↓
Predictions
```

Batching can improve throughput, especially on GPUs.

---

# Real-Time Inference

Real-time systems prioritize latency.

Example:

```text
Camera
 ↓
Frame
 ↓
Model
 ↓
Detection
 ↓
Display
```

Important measurements include:

```text
Latency
FPS
GPU Utilization
Memory Usage
Throughput
```

---

# Batch vs Real-Time

| Mode | Primary Goal |
|---|---|
| Batch | High throughput |
| Real-time | Low latency |
| Offline | Large-scale processing |
| Interactive | Responsive user experience |

The deployment architecture should match the intended workload.

---

# Preprocessing

The deployment preprocessing must match training.

For example:

```text
Resize
 ↓
Normalize
 ↓
Convert to Tensor
 ↓
Add Batch Dimension
 ↓
Model
```

A common deployment failure is using different preprocessing during inference.

```text
TRAINING
Resize → Normalize → Model

DEPLOYMENT
Resize → Different Normalize → Model
```

This can significantly degrade performance.

---

# Post-Processing

Model outputs often need additional processing.

Classification:

```text
Logits
 ↓
Softmax / Sigmoid
 ↓
Threshold / Argmax
 ↓
Class
```

Detection:

```text
Raw Predictions
 ↓
Confidence Filtering
 ↓
NMS
 ↓
Bounding Boxes
```

Segmentation:

```text
Probability Map
 ↓
Threshold / Argmax
 ↓
Mask
 ↓
Optional Post-processing
```

---

# REST API

A REST API allows another application to send an image to the model.

Example:

```text
POST /predict
```

Request:

```text
Image
```

Response:

```json
{
  "class": "cat",
  "confidence": 0.97
}
```

The exact response format should be designed around the client application's needs.

---

# FastAPI

FastAPI is a practical framework for building Python inference APIs.

Basic structure:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/health")
def health():
    return {
        "status": "ok"
    }
```

A prediction endpoint can then be added.

---

# FastAPI Prediction Architecture

```text
Client
  │
  ▼
POST /predict
  │
  ▼
Validate Input
  │
  ▼
Preprocess
  │
  ▼
Model
  │
  ▼
Postprocess
  │
  ▼
JSON Response
```

---

# Simplified FastAPI Example

```python
from fastapi import FastAPI, UploadFile, File

app = FastAPI()

@app.get("/health")
def health():
    return {"status": "ok"}

@app.post("/predict")
async def predict(
    file: UploadFile = File(...)
):
    # Load image
    # Preprocess image
    # Run model
    # Post-process prediction

    return {
        "status": "success"
    }
```

A production implementation should add validation, exception handling, logging, model loading, and resource management.

---

# Health Checks

A deployed service should expose a health endpoint.

```text
GET /health
```

Example response:

```json
{
  "status": "healthy"
}
```

Health checks help infrastructure determine whether the service is available.

---

# Readiness vs Liveness

Liveness asks:

```text
Is the process running?
```

Readiness asks:

```text
Is the service ready to accept inference requests?
```

For a model server, readiness may require:

```text
Application Started
+
Model Loaded
+
Required Resources Available
```

---

# Streamlit Deployment

Streamlit is useful for interactive computer vision applications.

Architecture:

```text
USER
 ↓
STREAMLIT UI
 ↓
IMAGE UPLOAD
 ↓
PREPROCESSING
 ↓
MODEL
 ↓
RESULT
```

A simple interface might be:

```python
import streamlit as st

st.title(
    "Computer Vision Demo"
)

uploaded_file = st.file_uploader(
    "Upload an image"
)

if uploaded_file:
    st.image(uploaded_file)
```

The model inference logic can then be integrated into the application.

---

# Streamlit vs FastAPI

| Feature | Streamlit | FastAPI |
|---|---|---|
| UI | Built-in | External |
| REST API | Not primary purpose | Yes |
| Prototyping | Excellent | Good |
| ML Demo | Excellent | Good |
| Backend Service | Limited | Excellent |
| Interactive Dashboard | Excellent | Requires frontend |

A common architecture is:

```text
Streamlit
   ↓
FastAPI
   ↓
Model
```

---

# Docker

Docker packages the application and its dependencies.

Conceptually:

```text
Docker Container
│
├── Python
├── PyTorch
├── OpenCV
├── FastAPI
├── Model
└── Application
```

This makes deployment environments more reproducible.

---

# Basic Dockerfile

A simplified example:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir     -r requirements.txt

COPY . .

CMD [
    "uvicorn",
    "app:app",
    "--host",
    "0.0.0.0",
    "--port",
    "8000"
]
```

For GPU workloads, the base image and CUDA-compatible dependencies must be selected carefully.

---

# Docker Build

Example:

```powershell
docker build     -t cv-inference .
```

Run:

```powershell
docker run     -p 8000:8000     cv-inference
```

The exact command may vary by operating system and deployment environment.

---

# GPU Containers

For NVIDIA GPU inference, the architecture becomes:

```text
HOST
 │
 ├── NVIDIA DRIVER
 │
 ▼
CONTAINER RUNTIME
 │
 ▼
CUDA USER-SPACE
 │
 ▼
PyTorch
 │
 ▼
MODEL
 │
 ▼
GPU
```

The host driver and container software stack must be compatible.

---

# NVIDIA Container Toolkit

GPU-enabled Docker environments commonly use NVIDIA Container Toolkit.

A GPU-enabled container can then expose the GPU to the application.

Conceptually:

```text
docker
  ↓
NVIDIA Runtime
  ↓
CUDA
  ↓
GPU
```

Always verify the actual runtime configuration on the target machine.

---

# Testing GPU Visibility

Inside a suitable CUDA-enabled container:

```bash
nvidia-smi
```

Then from Python:

```python
import torch

print(torch.cuda.is_available())

if torch.cuda.is_available():
    print(
        torch.cuda.get_device_name(0)
    )
```

Both checks are useful when diagnosing deployment issues.

---

# ONNX

ONNX provides an interoperable model representation.

A simplified workflow:

```text
PyTorch Model
      ↓
Export
      ↓
ONNX Model
      ↓
ONNX Runtime
      ↓
Inference
```

ONNX can be useful when the deployment environment differs from the original training framework.

---

# ONNX Runtime

ONNX Runtime can execute ONNX models.

Conceptually:

```text
ONNX Model
    ↓
ONNX Runtime
    ↓
CPU / GPU
    ↓
Prediction
```

It can be useful for deployment-focused inference pipelines.

---

# Model Export

A simplified PyTorch export concept is:

```python
torch.onnx.export(
    model,
    example_input,
    "model.onnx"
)
```

The exact export API and recommended options depend on the installed PyTorch version and model architecture.

Always validate exported model outputs against the original model.

---

# Model Validation After Export

Never assume:

```text
PyTorch Output
=
ONNX Output
```

Validate them.

```text
Same Input
   │
   ├─────────────► PyTorch
   │
   └─────────────► ONNX
                     │
                     ▼
              Compare Outputs
```

Small numerical differences can occur, but predictions should remain acceptable for the intended application.

---

# Model Optimization

Deployment may require optimization.

Possible approaches include:

```text
Smaller Architecture
Quantization
ONNX
TensorRT
Mixed Precision
Batching
Caching
Image Resizing
Efficient Preprocessing
```

Optimization should be measured rather than assumed to improve the system.

---

# Quantization

Quantization reduces numerical precision.

For example:

```text
FP32
 ↓
FP16
 ↓
INT8
```

Potential benefits:

```text
Lower memory usage
Faster inference
Smaller model
```

Potential trade-offs:

```text
Accuracy degradation
Hardware compatibility
Implementation complexity
```

---

# FP16

Half precision can reduce memory requirements.

Conceptually:

```text
FP32
 ↓
FP16
 ↓
Lower Memory / Potentially Faster Inference
```

GPU support and model operations determine whether FP16 provides a useful improvement.

---

# TensorRT

For NVIDIA-focused deployments, TensorRT can be used to optimize supported inference workloads.

Conceptually:

```text
Trained Model
      ↓
Export
      ↓
TensorRT Engine
      ↓
NVIDIA GPU
      ↓
Optimized Inference
```

The conversion process depends on the model architecture and supported operators.

---

# Latency

Latency measures how long one inference request takes.

Example:

```text
Request
   ↓
Preprocess = 10 ms
Inference  = 35 ms
Postprocess = 5 ms
   ↓
Total = 50 ms
```

Reducing model inference time alone may not minimize total request latency.

---

# Throughput

Throughput measures how much work the system processes over time.

For example:

```text
100 images / second
```

A system can have:

```text
Higher throughput
but
Higher individual latency
```

This is why latency and throughput should be measured separately.

---

# FPS

For video applications:

```text
FPS =
Processed Frames
───────────────
Time
```

Example:

```text
30 FPS
```

means approximately 30 frames are processed per second.

Real-time requirements depend on the application.

---

# Memory Monitoring

Track:

```text
System RAM
GPU VRAM
CPU Utilization
GPU Utilization
Disk Usage
```

For NVIDIA GPUs:

```bash
nvidia-smi
```

can help inspect GPU utilization and memory.

---

# Logging

A production inference service should log useful information.

Example:

```text
Timestamp
Request ID
Model Version
Input Dimensions
Inference Time
Device
Prediction
Error
```

Avoid logging sensitive image data or personally identifiable information unless the system is specifically designed and authorized to handle it.

---

# Error Handling

Possible API failures include:

```text
Invalid File
Unsupported Format
Corrupted Image
Model Loading Failure
GPU Unavailable
Out of Memory
Inference Exception
Timeout
```

A production service should return meaningful HTTP status codes and safe error messages.

---

# Input Validation

Validate:

```text
File Type
File Size
Image Dimensions
Color Channels
Image Integrity
```

For example:

```text
JPEG
PNG
WEBP
```

may be accepted while unsupported formats are rejected.

---

# Model Versioning

Never treat a production model as an anonymous file.

Use explicit versions:

```text
model-v1
model-v2
model-v3
```

A stronger naming approach can include:

```text
Model
Version
Dataset Version
Training Configuration
Date
```

Example:

```text
unet-brain-v3
```

---

# Model Registry Concept

A larger system can maintain:

```text
MODEL REGISTRY
      │
 ┌────┼────┐
 ▼    ▼    ▼
 v1   v2   v3
```

Each model can be associated with:

```text
Metrics
Training Data
Configuration
Artifacts
Deployment Status
```

---

# Reproducibility

A deployment should be reproducible.

Record:

```text
Python Version
PyTorch Version
CUDA Version
OS
Dependencies
Model Version
Configuration
Environment Variables
```

A `requirements.txt` file can capture Python dependencies.

---

# Environment Variables

Do not hard-code deployment secrets.

Bad:

```python
API_KEY = "secret-value"
```

Prefer environment variables:

```python
import os

API_KEY = os.getenv(
    "API_KEY"
)
```

This is particularly important when deploying public services.

---

# Secrets

Sensitive values may include:

```text
API Keys
Database Credentials
Cloud Credentials
Private Tokens
Service Passwords
```

Do not commit them to Git.

Use:

```text
Environment Variables
Secret Managers
Deployment Platform Secrets
```

---

# API Security

A public inference API may need:

```text
Authentication
Rate Limiting
Request Validation
HTTPS
File Size Limits
Timeouts
Logging
```

Security requirements depend on the application and threat model.

---

# Testing the API

Example request:

```bash
curl   -X POST   http://localhost:8000/predict
```

For file upload APIs, the exact command depends on the endpoint contract.

Test:

```text
Valid Image
Invalid Image
Large Image
Unsupported Format
Empty Request
Multiple Requests
GPU Failure
Model Failure
```

---

# Load Testing

A service may work correctly for one request and fail under concurrent traffic.

Test:

```text
1 request
10 requests
50 requests
100 requests
```

Measure:

```text
Latency
Throughput
CPU
RAM
VRAM
Errors
Timeouts
```

Do not load-test a production endpoint without authorization.

---

# Monitoring

Production monitoring should track:

```text
Availability
Latency
Throughput
Error Rate
Resource Usage
Model Version
```

For ML systems, also consider:

```text
Input Distribution
Prediction Distribution
Data Drift
Performance Drift
```

---

# Data Drift

The data seen after deployment may differ from training data.

Example:

```text
TRAINING
High-quality daylight images

PRODUCTION
Night-time images
```

The model may perform worse even though the software is functioning correctly.

---

# Model Drift

Model performance can degrade when the relationship between inputs and expected outputs changes.

Monitoring may identify:

```text
Increasing errors
Changing class distribution
New image conditions
New devices
New environments
```

Retraining may eventually be required.

---

# Computer Vision Deployment Pipeline

```text
                    TRAINED MODEL
                          │
                          ▼
                    MODEL VALIDATION
                          │
                          ▼
                    MODEL VERSIONING
                          │
                          ▼
                    OPTIMIZATION
                 ┌────────┼────────┐
                 ▼        ▼        ▼
               ONNX      FP16    TensorRT
                 │        │        │
                 └────────┼────────┘
                          ▼
                     INFERENCE
                          │
                 ┌────────┴────────┐
                 ▼                 ▼
              FastAPI           Streamlit
                 │                 │
                 └────────┬────────┘
                          ▼
                        Docker
                          │
                 ┌────────┴────────┐
                 ▼                 ▼
                CPU               GPU
                 │                 │
                 └────────┬────────┘
                          ▼
                     PRODUCTION
                          │
                          ▼
                    MONITORING
```

---

# Example Project Structure

A practical computer vision deployment project might use:

```text
cv-deployment/
│
├── app/
│   ├── main.py
│   ├── inference.py
│   ├── preprocessing.py
│   └── postprocessing.py
│
├── models/
│   └── model.pt
│
├── tests/
│   ├── test_api.py
│   └── test_inference.py
│
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── .gitignore
└── README.md
```

Separating inference logic from the API layer makes the system easier to test and maintain.

---

# Deployment Checklist

## Model

```text
□ Best checkpoint selected
□ Model loads correctly
□ Model version recorded
□ Input/output shapes documented
```

## Preprocessing

```text
□ Resize verified
□ Normalization verified
□ Channel order verified
□ Training and inference preprocessing match
```

## Inference

```text
□ CPU tested
□ GPU tested where applicable
□ Batch size tested
□ Latency measured
□ Memory measured
```

## API

```text
□ Health endpoint
□ Prediction endpoint
□ Input validation
□ Error handling
□ Logging
□ Authentication if required
```

## Container

```text
□ Docker image builds
□ Container starts
□ Model loads
□ CPU inference works
□ GPU inference works if required
```

## Production

```text
□ HTTPS
□ Secrets managed safely
□ Resource limits
□ Monitoring
□ Model versioning
□ Rollback strategy
□ Backup / recovery plan
```

---

# Practical Deployment Workflow

```text
1. Train Model
      ↓
2. Evaluate Model
      ↓
3. Save Best Checkpoint
      ↓
4. Build Inference Pipeline
      ↓
5. Test Locally
      ↓
6. Export / Optimize
      ↓
7. Build API or UI
      ↓
8. Containerize
      ↓
9. Test Container
      ↓
10. Deploy
      ↓
11. Monitor
      ↓
12. Improve
```

---

# Key Takeaways

```text
✓ Deployment is the bridge between a trained model and a usable application

✓ Training and inference have different requirements

✓ Preprocessing must remain consistent

✓ FastAPI is useful for inference APIs

✓ Streamlit is useful for interactive ML applications

✓ Docker improves deployment reproducibility

✓ NVIDIA GPU deployment requires compatible driver and CUDA/runtime configuration

✓ ONNX can provide an interoperable deployment format

✓ Quantization and optimized runtimes can reduce inference cost

✓ Latency and throughput should be measured separately

✓ Production systems need logging and error handling

✓ Model versions should be tracked

✓ Secrets should never be hard-coded into source code

✓ Input validation is important for public inference services

✓ Monitoring should cover both infrastructure and model behavior

✓ Data drift can reduce real-world model performance

✓ Deployment is an iterative engineering process
```

---

# Computer Vision Lab — End-to-End Journey

You have now covered the major stages:

```text
01  Computer Vision Fundamentals
          ↓
02  Image Classification
          ↓
03  Object Detection
          ↓
04  Image Segmentation
          ↓
05  OpenCV & Image Processing
          ↓
06  YOLO Object Detection
          ↓
07  U-Net & Medical Image Segmentation
          ↓
08  Model Evaluation & Metrics
          ↓
09  Computer Vision Deployment
```

The overall journey is:

```text
IMAGE
  ↓
UNDERSTAND
  ↓
CLASSIFY
  ↓
DETECT
  ↓
SEGMENT
  ↓
EVALUATE
  ↓
DEPLOY
  ↓
MONITOR
```

---

# Related Experiments

- [Computer Vision Fundamentals →](/labs/computer-vision/fundamentals/)
- [Image Classification →](/labs/computer-vision/image-classification/)
- [Object Detection →](/labs/computer-vision/object-detection/)
- [Image Segmentation →](/labs/computer-vision/image-segmentation/)
- [OpenCV & Image Processing →](/labs/computer-vision/opencv/)
- [YOLO Object Detection →](/labs/computer-vision/yolo/)
- [U-Net & Medical Image Segmentation →](/labs/computer-vision/u-net/)
- [Model Evaluation & Metrics →](/labs/computer-vision/model-evaluation/)

---

> **A successful computer vision project does not end with a trained model. It ends when the model can reliably solve a real problem in a real environment.**
