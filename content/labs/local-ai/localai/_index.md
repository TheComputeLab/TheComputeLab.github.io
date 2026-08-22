---
title: "LocalAI"
description: "Running AI models locally with LocalAI, Docker, NVIDIA GPUs and CUDA."
weight: 10
toc: true
---

> **A local API layer for running AI models on your own hardware.**

LocalAI is an open-source inference layer that allows AI applications to communicate with locally hosted models through APIs compatible with common AI application interfaces.

For this experiment, the goal was not simply to install LocalAI.

The goal was to understand the complete path:

```text
Application
     |
     v
  LocalAI
     |
     v
Docker Container
     |
     v
 NVIDIA GPU
     |
     v
    CUDA
     |
     v
   Model
     |
     v
Local Inference
```

---

## Why LocalAI?

Cloud AI APIs are convenient, but they introduce dependencies on:

- Internet connectivity
- API availability
- usage limits
- API costs
- remote infrastructure
- external data processing

Running AI locally provides another option.

### Cloud AI

```text
Application
     |
     v
 Internet
     |
     v
 Cloud API
     |
     v
Remote Model
```

### Local AI

```text
Application
     |
     v
Local API
     |
     v
Local Model
     |
     v
Local GPU
```

The local architecture provides much greater control over the inference environment.

---

# Lab Objective

The objective of this experiment was to create a local AI environment capable of:

- running LocalAI
- using an NVIDIA GPU
- running inside Docker
- accessing local model files
- using CUDA-enabled inference
- exposing a local API
- experimenting with locally hosted models

The basic environment was:

```text
Windows Host
     |
     v
NVIDIA GPU
     |
     v
Docker
     |
     v
LocalAI GPU Container
     |
     v
CUDA
     |
     v
AI Model
```

---

# Environment

The experiment used a GPU-enabled LocalAI Docker image:

```text
localai/localai:latest-gpu-nvidia-cuda-12
```

The LocalAI service was exposed on:

```text
localhost:8080
```

The important components were:

| Component | Purpose |
|---|---|
| LocalAI | Local inference/API layer |
| Docker | Container runtime |
| NVIDIA GPU | Hardware acceleration |
| CUDA | GPU compute layer |
| Model files | AI model weights |
| Host filesystem | Persistent model storage |
| API | Application interface |

---

# Architecture

The complete setup can be viewed as several layers:

```text
┌──────────────────────────────────────┐
│             APPLICATION              │
│                                      │
│   Python / API Client / Web App      │
└──────────────────┬───────────────────┘
                   |
                   v
┌──────────────────────────────────────┐
│              LOCALAI                 │
│                                      │
│        Local AI API / Runtime        │
└──────────────────┬───────────────────┘
                   |
                   v
┌──────────────────────────────────────┐
│               DOCKER                 │
│                                      │
│       GPU-enabled Container          │
└──────────────────┬───────────────────┘
                   |
                   v
┌──────────────────────────────────────┐
│           NVIDIA CONTAINER           │
│              RUNTIME                 │
└──────────────────┬───────────────────┘
                   |
                   v
┌──────────────────────────────────────┐
│               CUDA                   │
└──────────────────┬───────────────────┘
                   |
                   v
┌──────────────────────────────────────┐
│             NVIDIA GPU               │
└──────────────────────────────────────┘
```

This layered architecture is important when troubleshooting.

If LocalAI fails, the problem may not actually be LocalAI.

It could be:

```text
Model
  |
  v
Runtime
  |
  v
CUDA
  |
  v
NVIDIA Driver
  |
  v
Docker
  |
  v
GPU
```

---

# Docker Container

The LocalAI service was run inside Docker rather than directly on the host.

The GPU-enabled image used for the experiment was:

```text
localai/localai:latest-gpu-nvidia-cuda-12
```

The container exposed LocalAI's API through:

```text
8080
```

Conceptually:

```text
Host
 |
 | localhost:8080
 v
Docker
 |
 v
LocalAI
```

This allows applications running on the host to communicate with the LocalAI service.

---

# GPU Acceleration

One of the most important parts of this experiment was ensuring that the LocalAI container could actually access the NVIDIA GPU.

There are two separate questions.

### Is the GPU working on the host?

Use:

```bash
nvidia-smi
```

This provides information about:

- GPU model
- driver
- VRAM
- utilization
- running processes

### Can Docker access the GPU?

A GPU working on the host does **not automatically mean that every Docker container can use it**.

The architecture should be:

```text
Host GPU
   |
   v
NVIDIA Driver
   |
   v
NVIDIA Container Toolkit
   |
   v
Docker
   |
   v
LocalAI
```

This distinction becomes very important when troubleshooting GPU containers.

---

# CUDA

CUDA sits between the NVIDIA hardware and the software performing GPU computation.

A simplified architecture is:

```text
LocalAI
   |
   v
Inference Runtime
   |
   v
CUDA
   |
   v
NVIDIA Driver
   |
   v
GPU
```

This is why a CUDA error does not necessarily mean that the AI model itself is broken.

The failure may exist somewhere in the GPU software stack.

---

# Model Storage

AI models should generally be separated from the Docker container lifecycle.

A better architecture is:

```text
Host
 |
 +-- models/
 |     |
 |     +-- model-1
 |     +-- model-2
 |     +-- model-3
 |
 +-- Docker
        |
        v
      LocalAI
```

The model directory can be mounted into the container.

This provides persistence.

If the container is deleted:

```text
Container -> deleted

Models -> preserved
```

This is one of the major advantages of persistent Docker volumes or bind mounts.

---

# Why Persistent Model Storage Matters

AI model files can be large.

Re-downloading models every time a container is recreated is inefficient.

Instead:

```text
Host Model Directory
        |
        | mount
        v
Docker Container
        |
        v
     LocalAI
```

The container becomes replaceable while the models remain persistent.

A useful containerization principle is:

> **Keep application state separate from the container lifecycle.**

---

# API Architecture

One of the useful characteristics of LocalAI is that applications can communicate with the inference environment through an API.

Conceptually:

```text
Python Application
       |
       | HTTP
       v
localhost:8080
       |
       v
    LocalAI
       |
       v
     Model
       |
       v
      GPU
```

This means an application does not necessarily need to know exactly how the model is running.

The application can simply communicate with the local API.

---

# Local Inference

The complete inference pipeline can be represented as:

```text
User Input
    |
    v
Application
    |
    v
LocalAI API
    |
    v
Model Loader
    |
    v
Inference Runtime
    |
    v
CUDA
    |
    v
NVIDIA GPU
    |
    v
Generated Output
```

This creates a separation between the application and the model infrastructure.

The application focuses on:

```text
"What do I want the AI to do?"
```

while the inference layer handles:

```text
"How do I execute the model?"
```

---

# LocalAI and Model Backends

LocalAI acts as an interface between an application and supported model backends.

A simplified architecture is:

```text
             Application
                  |
                  v
             LocalAI API
                  |
        +---------+---------+
        |                   |
        v                   v
   Text Models        Image / Other
        |
        v
   Model Backend
        |
        v
       GPU
```

This makes LocalAI useful as an experimentation layer because the application does not necessarily need to change every time the underlying model or backend changes.

---

# What I Learned

The most important lesson from this experiment was:

> **Local AI is not just about downloading a model.**

A working local AI environment requires several layers to cooperate.

```text
Application
     |
     v
LocalAI
     |
     v
Model
     |
     v
Runtime
     |
     v
CUDA
     |
     v
NVIDIA Driver
     |
     v
GPU
     |
     v
Docker
```

A failure at any layer can prevent inference.

---

# Troubleshooting

## GPU visible on host but not inside container

First check the host:

```bash
nvidia-smi
```

If the GPU works on the host but is unavailable inside Docker, investigate:

```text
NVIDIA Driver
       |
       v
NVIDIA Container Toolkit
       |
       v
Docker GPU Runtime
       |
       v
LocalAI Container
```

The problem is likely in the container GPU path rather than the model.

---

## CUDA problems

CUDA-related errors can originate from compatibility issues between:

```text
NVIDIA Driver
CUDA
Inference Runtime
PyTorch / Backend
Container Image
```

Therefore, don't treat:

```text
CUDA error
```

as a complete diagnosis.

First identify which layer is producing the error.

---

## Model doesn't load

Possible causes include:

- unsupported model format
- incorrect model path
- missing configuration
- incompatible backend
- insufficient VRAM
- corrupted model file
- incorrect model configuration

A useful troubleshooting sequence is:

```text
Check File
    |
    v
Check Format
    |
    v
Check Configuration
    |
    v
Check Runtime
    |
    v
Check GPU
    |
    v
Check VRAM
    |
    v
Check Logs
```

---

## Out of VRAM

Large models can exceed available GPU memory.

Possible approaches include:

```text
Smaller Model
      OR
Quantized Model
      OR
CPU Offloading
      OR
More VRAM
```

Model size therefore has a direct relationship with hardware requirements.

---

# LocalAI vs Cloud AI

| Area | Cloud AI | LocalAI |
|---|---|---|
| Hardware | Provider managed | Your hardware |
| Internet | Usually required | Can operate locally |
| Data path | Remote | Local |
| Cost | Usage based | Hardware + electricity |
| Latency | Network dependent | Local |
| Model control | Limited | High |
| Scaling | Provider managed | Self managed |
| Maintenance | Low | Higher |
| Privacy control | Provider dependent | Local environment |

Neither approach is universally better.

The appropriate architecture depends on the application.

---

# When Local AI Makes Sense

Local inference can be useful for:

### Development

Experiment with models without repeatedly calling external APIs.

### Privacy-sensitive workloads

Keep data within your own environment.

### Offline environments

Run inference without depending on an Internet connection.

### Prototyping

Test models and architectures locally.

### Learning

Understand what actually happens beneath an AI API.

That last point is particularly important for this Lab.

---

# Local AI Stack

The experiment connects directly to the other pages in this Lab:

```text
                 LOCAL AI
                     |
        +------------+------------+
        |            |            |
      LocalAI      Models       Docker
        |            |            |
        +------------+------------+
                     |
                     v
                  CUDA
                     |
                     v
                NVIDIA GPU
                     |
                     v
              Local Inference
```

Each layer will be explored separately in the following experiments.

---

# Related Experiments

- [Docker →](/labs/local-ai/docker/)
- [NVIDIA GPU →](/labs/local-ai/nvidia-gpu/)
- [CUDA →](/labs/local-ai/cuda/)
- [Local LLMs →](/labs/local-ai/local-llms/)
- [ComfyUI →](/labs/local-ai/comfyui/)
- [Stable Diffusion →](/labs/local-ai/stable-diffusion/)
- [DreamShaper →](/labs/local-ai/dreamshaper/)
- [Local Inference →](/labs/local-ai/local-inference/)
- [GPU Container Troubleshooting →](/labs/local-ai/gpu-container-troubleshooting/)

---

# Lab Status

```text
LOCALAI
────────────────────────────

Container          ✓
GPU Environment    ✓
CUDA               ✓
API                ✓
Model Experiments  ✓

STATUS: EXPERIMENTAL
```

> **Running AI locally is not just an alternative to cloud AI.**
>
> **It is a way to understand the entire AI stack.**
