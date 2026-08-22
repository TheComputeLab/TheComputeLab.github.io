---
title: "Local AI Architecture"
description: "A complete architecture overview of the Local AI Lab, connecting models, runtimes, Docker, CUDA, GPUs, APIs, storage, and inference."
weight: 130
toc: true
---

> **Local AI is not a single application. It is a complete computing stack.**

The Local AI Lab brings together everything explored in the previous experiments:

```text
                    LOCAL AI LAB
                         |
        +----------------+----------------+
        |                                 |
     LOCAL LLM                         IMAGE AI
        |                                 |
     LocalAI                           ComfyUI
        |                                 |
   Local LLMs                     Stable Diffusion
        |                                 |
     GGUF                           DreamShaper
        |                                 |
        +----------------+----------------+
                         |
                   Model Formats
                         |
                         v
                  Local Inference
                         |
              +----------+----------+
              |                     |
             CPU                   GPU
                                    |
                                  CUDA
                                    |
                             NVIDIA GPU
                                    |
                                 Docker
                                    |
                           GPU Container
                                    |
                           Troubleshooting
```

This page is the architectural overview of the entire Local AI Lab.

---

# Architecture Objective

The goal is to understand how the individual technologies fit together.

Instead of looking at:

```text
LocalAI
ComfyUI
CUDA
Docker
GPU
Models
```

as separate technologies, we can view them as layers of one system.

```text
Application
    |
    v
AI Runtime
    |
    v
Model
    |
    v
Framework
    |
    v
CUDA / CPU
    |
    v
Hardware
```

---

# The Complete Local AI Stack

A typical local AI system can be divided into several layers:

```text
┌─────────────────────────────────────┐
│           APPLICATION               │
├─────────────────────────────────────┤
│           AI RUNTIME                │
├─────────────────────────────────────┤
│             MODEL                  │
├─────────────────────────────────────┤
│        FRAMEWORK / ENGINE           │
├─────────────────────────────────────┤
│          CUDA / CPU                 │
├─────────────────────────────────────┤
│       DRIVER / CONTAINER            │
├─────────────────────────────────────┤
│          HARDWARE                   │
└─────────────────────────────────────┘
```

Each layer has a specific responsibility.

---

# Layer 1 — Application

The application is what the user interacts with.

Examples:

```text
Chat Interface
Web Application
API Client
ComfyUI
Python Application
Streamlit Application
```

The application sends an input to the AI system.

For example:

```text
User
 |
 v
Prompt
 |
 v
Application
```

---

# Layer 2 — AI Runtime

The runtime is responsible for executing the model.

Examples explored in this lab include:

```text
LocalAI
ComfyUI
PyTorch-based applications
Diffusion runtimes
LLM runtimes
```

The runtime manages the interaction between:

```text
Application
     |
     v
Model
     |
     v
Hardware
```

---

# Layer 3 — Model

The model contains the learned parameters.

For language AI:

```text
LLM
 |
 v
Generated Text
```

For image generation:

```text
Diffusion Model
 |
 v
Generated Image
```

Examples explored in the lab include:

```text
Local LLMs
Stable Diffusion
DreamShaper
```

---

# Layer 4 — Model Format

The model needs to be stored in a format that the runtime understands.

Examples:

```text
GGUF
SafeTensors
Checkpoint
Diffusers
```

The relationship is:

```text
Model
 |
 v
Format
 |
 v
Loader
 |
 v
Runtime
```

A file extension alone does not guarantee compatibility.

---

# Layer 5 — Framework / Engine

The AI runtime may use an underlying framework or inference engine.

Examples include:

```text
PyTorch
CUDA-enabled inference engines
LLM inference engines
Diffusion engines
```

The framework converts high-level model operations into computational operations that can run on the available hardware.

---

# Layer 6 — CUDA

For NVIDIA GPUs:

```text
AI Runtime
    |
    v
Framework
    |
    v
CUDA
    |
    v
NVIDIA GPU
```

CUDA provides the software layer used by compatible frameworks to execute operations on NVIDIA hardware.

---

# Layer 7 — NVIDIA Driver

The NVIDIA driver connects the operating environment to the GPU.

Conceptually:

```text
Application
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

The driver is therefore a foundational component of the GPU inference stack.

---

# Layer 8 — Docker

Docker provides an isolated and reproducible software environment.

A container can package:

```text
Application
Runtime
Libraries
CUDA Runtime
Configuration
```

while the host provides:

```text
GPU
Driver
Storage
Operating System
```

---

# Docker GPU Architecture

A typical architecture is:

```text
HOST
 |
 +-- Windows / Linux
 |
 +-- NVIDIA Driver
 |
 +-- GPU
 |
 +-- Docker
       |
       v
   AI Container
       |
       +-- Application
       +-- Runtime
       +-- CUDA Runtime
       +-- Model
```

The NVIDIA container integration provides the bridge between the container and the host GPU.

---

# WSL2 Architecture

For Windows-based local AI experimentation, WSL2 can provide the Linux environment used by Docker.

```text
Windows
   |
   v
NVIDIA Driver
   |
   v
WSL2
   |
   v
Linux
   |
   v
Docker
   |
   v
AI Container
   |
   v
GPU
```

This creates a powerful local development environment.

---

# LocalAI Architecture

LocalAI can provide an API layer over local models.

A simplified architecture is:

```text
Application
     |
     | HTTP
     v
LocalAI
     |
     v
Model Runtime
     |
     v
Local Model
     |
     +---- CPU
     |
     +---- GPU
           |
           v
         CUDA
```

This allows applications to communicate with local models through an API.

---

# Local LLM Architecture

The LLM path can be represented as:

```text
User
 |
 v
Application
 |
 v
LocalAI / LLM Runtime
 |
 v
GGUF / Compatible Model
 |
 v
Inference Engine
 |
 +---- CPU
 |
 +---- GPU
       |
       v
     CUDA
       |
       v
      GPU
 |
 v
Generated Text
```

---

# LLM Inference Flow

A simplified LLM request is:

```text
Prompt
  |
  v
Tokenizer
  |
  v
Token IDs
  |
  v
LLM
  |
  v
Next Token
  |
  v
Next Token
  |
  v
...
  |
  v
Generated Response
```

The runtime repeatedly executes the model to generate tokens.

---

# Image AI Architecture

The image-generation path is different:

```text
User
 |
 v
Prompt
 |
 v
ComfyUI
 |
 v
Stable Diffusion / DreamShaper
 |
 v
Text Conditioning
 |
 v
Denoising
 |
 v
Latent
 |
 v
VAE
 |
 v
Image
```

---

# ComfyUI Architecture

ComfyUI represents the image-generation pipeline as a node graph.

A simplified workflow is:

```text
Checkpoint
    |
    +-------- Model
    |
    +-------- CLIP
    |
    +-------- VAE
              |
Prompt ------> CLIP
              |
              v
        Conditioning
              |
              v
           KSampler
              |
              v
            Latent
              |
              v
          VAE Decode
              |
              v
            Image
```

The node-based design makes the individual stages visible.

---

# DreamShaper Architecture

The DreamShaper workflow fits into the image-generation stack:

```text
DreamShaper
     |
     v
SafeTensors
     |
     v
ComfyUI
     |
     v
Stable Diffusion Workflow
     |
     v
PyTorch / Runtime
     |
     v
CUDA
     |
     v
NVIDIA GPU
     |
     v
Generated Image
```

This is a practical example of the complete local AI stack.

---

# CPU Architecture

Local AI does not require a GPU.

A CPU-only architecture can be:

```text
Application
    |
    v
Runtime
    |
    v
Model
    |
    v
CPU
    |
    v
Output
```

This is useful for:

```text
Testing
Development
Small Models
Fallback
Systems Without GPUs
```

---

# GPU Architecture

For GPU inference:

```text
Application
    |
    v
Runtime
    |
    v
Model
    |
    v
Framework
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

This generally provides significantly more parallel compute capability for suitable AI workloads.

---

# CPU vs GPU Decision

A simplified decision process:

```text
Model / Workload
      |
      v
Can CPU handle it?
      |
   Yes|        No
      |         |
      v         v
    CPU       GPU
                |
                v
              CUDA
```

For larger models and diffusion workloads, GPU acceleration can become particularly important.

---

# Memory Architecture

Local AI uses several types of memory:

```text
Storage
   |
   v
System RAM
   |
   v
GPU VRAM
```

The model may be stored on disk, loaded into system memory, and then partially or fully placed in GPU memory depending on the runtime.

---

# Storage Layer

Models can occupy significant disk space.

A useful organization is:

```text
AI-MODELS/
│
├── LLM/
│
├── DIFFUSION/
│
├── CHECKPOINTS/
│
├── VAE/
│
├── LORA/
│
└── CONTROLNET/
```

Separating model types simplifies management.

---

# Persistent Docker Storage

Models should generally survive container recreation.

A useful architecture is:

```text
HOST
 |
 +-- models/
 |
 +-- Docker Container
       |
       +-- Mounted /models
```

The container can then be recreated without downloading the models again.

---

# Network Architecture

A local AI server may expose an API:

```text
Application
     |
     | HTTP
     v
localhost:8080
     |
     v
AI Runtime
     |
     v
Model
```

For example:

```text
Web App
   |
   v
Local API
   |
   v
LocalAI
   |
   v
LLM
```

Keeping inference endpoints local can reduce unnecessary network exposure.

---

# Security Boundary

A local AI service should not automatically be exposed to the public Internet.

Safer default:

```text
Application
    |
    v
localhost
    |
    v
AI Runtime
```

If remote access is required, consider:

```text
Authentication
HTTPS
Firewall Rules
Network Segmentation
Access Control
```

---

# Monitoring Architecture

A useful local AI monitoring stack can observe:

```text
Application
     |
     +-- Logs
     |
     +-- Latency
     |
     +-- Errors

GPU
 |
 +-- Utilization
 +-- VRAM
 +-- Temperature
 +-- Processes
```

For NVIDIA systems:

```bash
nvidia-smi
```

is a useful first diagnostic tool.

---

# Performance Metrics

For LLMs:

```text
Time to First Token
Tokens / Second
Total Generation Time
VRAM
```

For image generation:

```text
Seconds / Image
Images / Minute
Resolution
Steps
VRAM
GPU Utilization
```

These measurements make local AI performance quantifiable.

---

# Observability Flow

A practical diagnostic flow is:

```text
Request
  |
  v
Application Logs
  |
  v
Runtime Logs
  |
  v
Framework
  |
  v
GPU Metrics
  |
  v
System Metrics
```

When something goes wrong, move down the stack until the first failure is found.

---

# Troubleshooting Architecture

The Local AI troubleshooting hierarchy is:

```text
Hardware
   |
   v
Driver
   |
   v
WSL2
   |
   v
Docker
   |
   v
NVIDIA Container Runtime
   |
   v
CUDA
   |
   v
Framework
   |
   v
AI Runtime
   |
   v
Model
   |
   v
Workflow
   |
   v
Application
```

This hierarchy is one of the most important concepts in the lab.

---

# Golden Diagnostic Test

For NVIDIA GPU containers, a useful baseline test is:

```bash
docker run --rm --gpus all nvidia/cuda:12.4.1-base-ubuntu22.04 nvidia-smi
```

If this works:

```text
Docker
   |
   v
NVIDIA Container Integration
   |
   v
CUDA Container
   |
   v
GPU
```

has been validated.

The exact CUDA image tag should match the requirements of the environment being tested.

---

# AI Application Diagnostic

Once the container GPU works:

```text
Container GPU
      |
      v
Framework CUDA
      |
      v
AI Runtime
      |
      v
Model
      |
      v
Inference
```

Each layer can then be validated independently.

---

# Complete LLM Architecture

```text
                     USER
                       |
                       v
                APPLICATION
                       |
                       v
                  LOCAL API
                       |
                       v
                    LocalAI
                       |
                       v
                 LLM Runtime
                       |
                       v
                    GGUF
                       |
                       v
               Inference Engine
                       |
              +--------+--------+
              |                 |
             CPU               GPU
                                |
                               CUDA
                                |
                         NVIDIA Driver
                                |
                              GPU
                                |
                                v
                         Generated Text
```

---

# Complete Image Architecture

```text
                     USER
                       |
                       v
                    PROMPT
                       |
                       v
                    ComfyUI
                       |
                       v
              Stable Diffusion
                       |
                       v
                  DreamShaper
                       |
              +--------+--------+
              |                 |
            CLIP              VAE
              |                 |
              v                 |
         Conditioning           |
              |                 |
              +-------+---------+
                      |
                      v
                   KSampler
                      |
                      v
                    Latent
                      |
                      v
                 VAE Decode
                      |
                      v
                    IMAGE
                      |
                      v
                 NVIDIA GPU
                      |
                     CUDA
```

---

# Complete Infrastructure Architecture

```text
                         HOST
                          |
             +------------+------------+
             |                         |
          Storage                    GPU
             |                         |
             |                    NVIDIA Driver
             |                         |
             |                        WSL2
             |                         |
             +-----------+-------------+
                         |
                       Docker
                         |
                  NVIDIA Container
                     Integration
                         |
                  +------+------+
                  |             |
               LocalAI       ComfyUI
                  |             |
               Local LLM    Image Models
                  |             |
                GGUF      Stable Diffusion
                                |
                           DreamShaper
                  |             |
                  +------+------+
                         |
                       CUDA
                         |
                        GPU
```

---

# End-to-End Data Flow

The complete system can be summarized as:

```text
USER INPUT
    |
    v
APPLICATION
    |
    v
API / WORKFLOW
    |
    v
AI RUNTIME
    |
    v
MODEL
    |
    v
FRAMEWORK
    |
    v
CPU / CUDA
    |
    v
HARDWARE
    |
    v
INFERENCE
    |
    v
OUTPUT
```

The output then returns through the stack:

```text
GPU
 |
 v
Runtime
 |
 v
API
 |
 v
Application
 |
 v
User
```

---

# Why Local AI Architecture Matters

Understanding the architecture changes how troubleshooting is approached.

Instead of asking:

> "Why isn't my AI working?"

ask:

```text
Which layer failed?
```

For example:

```text
GPU not detected
     |
     v
Hardware / Driver

GPU works on host but not container
     |
     v
Docker / NVIDIA Container Integration

Container sees GPU but AI uses CPU
     |
     v
Framework / Runtime

Model loads but inference fails
     |
     v
Model / VRAM / Compatibility

Inference works but is slow
     |
     v
Performance / Hardware / Configuration
```

This turns troubleshooting into a systematic process.

---

# Local AI Design Principles

The lab follows several principles.

### 1. Separate Models From Applications

```text
Models
  ≠
Containers
  ≠
Applications
```

This makes systems easier to maintain.

### 2. Measure Before Optimizing

Record:

```text
Latency
VRAM
GPU Utilization
Throughput
```

before changing the configuration.

### 3. Validate Bottom-Up

Start with:

```text
GPU
```

and work upward.

### 4. Keep Experiments Reproducible

Record:

```text
Model
Version
Prompt
Seed
Settings
Hardware
Runtime
```

### 5. Prefer Modular Systems

A modular architecture allows components to be replaced independently.

---

# Local AI Lab Architecture

The complete lab can now be represented as:

```text
                         THE COMPUTE LAB
                               |
                        LOCAL AI LAB
                               |
          +--------------------+--------------------+
          |                                         |
      LANGUAGE AI                               IMAGE AI
          |                                         |
       LocalAI                                   ComfyUI
          |                                         |
     Local LLMs                             Stable Diffusion
          |                                         |
         GGUF                                   DreamShaper
          |                                         |
          +--------------------+--------------------+
                               |
                         Model Formats
                               |
                         Local Inference
                               |
                    +----------+----------+
                    |                     |
                   CPU                   GPU
                                          |
                                        CUDA
                                          |
                                   NVIDIA Driver
                                          |
                                         WSL2
                                          |
                                       Docker
                                          |
                              GPU Container Runtime
                                          |
                                  Troubleshooting
```

This is the architecture that ties the entire section together.

---

# What I Learned

The biggest lesson from the Local AI Lab is:

> **Local AI is an infrastructure problem as much as it is a model problem.**

A successful local AI system requires several layers to work together:

```text
Hardware
   +
Operating System
   +
Driver
   +
Container Runtime
   +
CUDA
   +
Framework
   +
Model
   +
AI Runtime
   +
Application
```

If one layer fails, the complete system can fail.

---

# Local AI Lab Checklist

```text
LOCAL AI ARCHITECTURE
────────────────────────────

[ ] Hardware
[ ] NVIDIA GPU
[ ] NVIDIA Driver
[ ] WSL2 / Linux
[ ] Docker
[ ] NVIDIA Container Integration
[ ] CUDA
[ ] Framework
[ ] Model
[ ] Model Format
[ ] AI Runtime
[ ] Application
[ ] Storage
[ ] API
[ ] Monitoring
[ ] Logging
[ ] Security
[ ] Performance Measurement
[ ] Troubleshooting Process
```

---

# Final Architecture Summary

```text
MODEL
  |
  v
RUNTIME
  |
  v
FRAMEWORK
  |
  v
CUDA / CPU
  |
  v
DRIVER
  |
  v
HARDWARE
```

And around this core:

```text
Docker
Storage
APIs
Monitoring
Security
Workflows
Troubleshooting
```

form the infrastructure required to turn a model into a usable local AI system.

---

# Related Experiments

- [LocalAI →](/labs/local-ai/localai/)
- [ComfyUI →](/labs/local-ai/comfyui/)
- [Docker →](/labs/local-ai/docker/)
- [NVIDIA GPU →](/labs/local-ai/nvidia-gpu/)
- [CUDA →](/labs/local-ai/cuda/)
- [Local LLMs →](/labs/local-ai/local-llms/)
- [Stable Diffusion →](/labs/local-ai/stable-diffusion/)
- [DreamShaper →](/labs/local-ai/dreamshaper/)
- [Image Generation →](/labs/local-ai/imagegeneration/)
- [Model Formats →](/labs/local-ai/modelformats/)
- [Local Inference →](/labs/local-ai/localinference/)
- [GPU Container Troubleshooting →](/labs/local-ai/gpu-troubleshooting/)

---

# Lab Status

```text
LOCAL AI LAB
────────────────────────────

LocalAI                    ✓
ComfyUI                    ✓
Docker for AI              ✓
NVIDIA GPU                ✓
CUDA                       ✓
Local LLMs                 ✓
Stable Diffusion           ✓
DreamShaper                ✓
Image Generation           ✓
Model Formats              ✓
Local Inference            ✓
GPU Troubleshooting        ✓
Architecture               ✓

STATUS: COMPLETE
```

> **The Local AI Lab is complete.**
>
> **From model files to GPU hardware, from Docker containers to CUDA, and from local LLMs to image generation — every layer is connected into one practical local AI architecture.**
