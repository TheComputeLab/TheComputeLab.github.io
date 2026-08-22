---
title: "CUDA"
description: "Understanding CUDA, NVIDIA drivers, GPU compute, PyTorch, Docker, and local AI inference."
weight: 50
toc: true
---

> **CUDA is the software bridge between AI workloads and NVIDIA GPU compute.**

When running AI locally on an NVIDIA GPU, the GPU itself is only one part of the system.

The software stack must provide a way for applications and AI frameworks to communicate with the hardware.

CUDA is a major part of that stack.

A simplified architecture is:

```text
AI Application
      |
      v
AI Framework / Runtime
      |
      v
CUDA
      |
      v
NVIDIA Driver
      |
      v
NVIDIA GPU
```

This page explores that layer and how it connects to the Local AI Lab experiments.

---

# Why CUDA?

AI workloads perform enormous numbers of mathematical operations.

Many of these operations can be executed in parallel.

NVIDIA CUDA provides the programming and runtime ecosystem that allows software to use NVIDIA GPUs for general-purpose computation.

Conceptually:

```text
CPU
 |
 +-- Application Logic

GPU
 |
 +-- Parallel Computation
```

CUDA provides the software mechanisms that allow applications to submit computational work to the GPU.

---

# Lab Objective

The objective of this experiment was to understand:

- what CUDA is
- how CUDA relates to NVIDIA GPUs
- the difference between CUDA and the NVIDIA driver
- CUDA Toolkit vs CUDA Runtime
- CUDA cores and Tensor Cores
- how PyTorch uses CUDA
- how Docker exposes CUDA-enabled environments
- how LocalAI and ComfyUI use GPU acceleration
- how to check CUDA availability
- how to diagnose CUDA compatibility problems
- how to identify CPU fallback and out-of-memory conditions

The key idea is:

> **CUDA is part of the compute software stack, not the GPU itself.**

---

# CUDA Architecture

A simplified CUDA stack looks like:

```text
┌──────────────────────────────┐
│       AI Application         │
│                              │
│ LocalAI / ComfyUI / Python   │
└──────────────┬───────────────┘
               |
               v
┌──────────────────────────────┐
│       AI Framework           │
│                              │
│ PyTorch / Backend            │
└──────────────┬───────────────┘
               |
               v
┌──────────────────────────────┐
│            CUDA              │
│                              │
│ Runtime / Libraries / APIs   │
└──────────────┬───────────────┘
               |
               v
┌──────────────────────────────┐
│       NVIDIA Driver          │
└──────────────┬───────────────┘
               |
               v
┌──────────────────────────────┐
│        NVIDIA GPU            │
└──────────────────────────────┘
```

Each layer has a different responsibility.

---

# CUDA Is Not the GPU

This distinction is important.

```text
CUDA
  |
  +-- Software ecosystem
```

while:

```text
NVIDIA GPU
  |
  +-- Physical hardware
```

The relationship is:

```text
Software
   |
   v
CUDA
   |
   v
Driver
   |
   v
GPU Hardware
```

CUDA does not replace the GPU.

It provides the software infrastructure used to program and execute workloads on NVIDIA GPUs.

---

# CUDA vs NVIDIA Driver

These two are often confused.

### NVIDIA Driver

The driver provides the operating-system-level interface to the GPU.

```text
Operating System
       |
       v
NVIDIA Driver
       |
       v
GPU
```

### CUDA

CUDA provides APIs, libraries and runtime components used by applications to perform GPU computation.

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

A working AI environment usually needs both.

---

# CUDA Toolkit

The CUDA Toolkit is a development environment provided by NVIDIA.

It can include:

- CUDA compiler
- development libraries
- debugging tools
- profiling tools
- headers
- runtime components
- command-line utilities

It is mainly relevant when developing or compiling CUDA applications.

Not every end-user AI application requires the complete CUDA Toolkit to be installed on the host.

---

# CUDA Runtime

The CUDA Runtime provides the components required by applications to execute CUDA workloads.

In a containerized AI environment, the application may use CUDA runtime components supplied by the container image while the host provides the NVIDIA driver.

Conceptually:

```text
HOST
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
       +-- CUDA Runtime
       |
       +-- AI Framework
```

This is an important concept when working with Docker.

---

# CUDA Version

CUDA versions are commonly represented like:

```text
CUDA 11.x
CUDA 12.x
CUDA 13.x
```

Different AI frameworks and containers may be built against different CUDA versions.

This means the phrase:

> "My system has CUDA 12"

does not always tell the complete story.

There can be multiple relevant CUDA components:

```text
Host Driver
CUDA Toolkit
Container CUDA Runtime
Framework Build
```

---

# CUDA Compatibility

A simplified compatibility chain is:

```text
GPU
 |
 v
NVIDIA Driver
 |
 v
CUDA Runtime
 |
 v
AI Framework
 |
 v
Application
```

The components need to be compatible.

A mismatch can result in:

- CUDA initialization failures
- unsupported operations
- runtime errors
- application startup failures
- CPU fallback
- model loading failures

Therefore, CUDA troubleshooting should always consider the entire stack.

---

# CUDA in Docker

Docker adds another layer.

A GPU-enabled container may look like:

```text
HOST
 |
 +-- GPU
 |
 +-- NVIDIA Driver
 |
 +-- Docker
       |
       +-- NVIDIA Container Toolkit
              |
              v
        AI Container
              |
              +-- CUDA Runtime
              |
              +-- PyTorch
              |
              +-- Application
```

The container does not normally replace the host NVIDIA driver.

The container provides the user-space software environment required by the application, while the host driver provides the interface to the physical GPU.

---

# NVIDIA Container Toolkit

The NVIDIA Container Toolkit is important when exposing NVIDIA GPUs to Docker containers.

The simplified path is:

```text
Docker
   |
   v
NVIDIA Container Toolkit
   |
   v
NVIDIA Driver
   |
   v
GPU
```

Without the appropriate GPU container integration, a container may start successfully while having no usable GPU.

This is a common source of confusion.

---

# GPU Detection vs CUDA Detection

These are different checks.

### Hardware/driver check

```bash
nvidia-smi
```

This asks:

> Can the NVIDIA driver communicate with the GPU?

### Framework check

```python
import torch

print(torch.cuda.is_available())
```

This asks:

> Can this PyTorch installation access CUDA?

A successful `nvidia-smi` result does not automatically guarantee that:

```python
torch.cuda.is_available()
```

will return `True`.

---

# Checking CUDA with `nvidia-smi`

A typical diagnostic command is:

```bash
nvidia-smi
```

It can provide:

```text
GPU
 |
 +-- Name
 +-- Driver Version
 +-- Memory Usage
 +-- GPU Utilization
 +-- Temperature
 +-- Processes
```

Some NVIDIA driver environments also display a CUDA-related version field.

However, this should not automatically be interpreted as:

> "This is the exact CUDA runtime used by my Python application."

The application may be using a different user-space CUDA runtime.

---

# Checking CUDA with PyTorch

For Python-based AI workloads:

```python
import torch

print("CUDA available:", torch.cuda.is_available())
print("PyTorch CUDA version:", torch.version.cuda)

if torch.cuda.is_available():
    print("GPU:", torch.cuda.get_device_name(0))
```

This gives visibility into the PyTorch side of the stack.

A useful diagnostic output might look like:

```text
CUDA available: True
PyTorch CUDA version: 12.x
GPU: NVIDIA ...
```

The exact values depend on the installed PyTorch build and hardware.

---

# CUDA Device

PyTorch can expose the CUDA device selected by the application.

For example:

```python
import torch

if torch.cuda.is_available():
    device = torch.device("cuda")
    print(device)
```

Conceptually:

```text
Python
  |
  v
PyTorch
  |
  v
CUDA
  |
  v
GPU
```

The application can then move tensors and models to the CUDA device.

---

# CPU vs CUDA Device

A simplified PyTorch example:

```python
device = "cuda" if torch.cuda.is_available() else "cpu"

print(device)
```

The result may be:

```text
cuda
```

or:

```text
cpu
```

This is useful for determining which execution path the application is using.

---

# Moving Data to the GPU

A simplified example:

```python
import torch

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

tensor = torch.randn(1000, 1000)
tensor = tensor.to(device)

print(tensor.device)
```

The important concept is:

```text
CPU Tensor
     |
     v
.to("cuda")
     |
     v
GPU Tensor
```

AI frameworks use similar mechanisms internally.

---

# GPU Kernels

A CUDA kernel is a function executed on the GPU.

Conceptually:

```text
CPU
 |
 | Launch Kernel
 v
GPU
 |
 +-- Thread
 +-- Thread
 +-- Thread
 +-- Thread
 +-- ...
```

Thousands of GPU threads can participate in parallel computation.

This execution model is one of the foundations of GPU acceleration.

---

# CUDA Cores

NVIDIA GPUs contain many processing units commonly referred to as CUDA cores.

A simplified idea is:

```text
GPU
 |
 +-- CUDA Core
 +-- CUDA Core
 +-- CUDA Core
 +-- CUDA Core
 +-- ...
```

The exact architecture varies between GPU generations.

CUDA cores are designed to perform general parallel computations.

More CUDA cores do not automatically mean proportionally better AI performance because other factors also matter.

---

# Tensor Cores

Modern NVIDIA GPUs may also contain specialized Tensor Cores.

Tensor Cores are designed to accelerate certain matrix and tensor operations that are heavily used in AI workloads.

A simplified architecture is:

```text
NVIDIA GPU
 |
 +-- CUDA Cores
 |
 +-- Tensor Cores
 |
 +-- Memory System
 |
 +-- Other GPU Hardware
```

Tensor Cores can be particularly useful for deep-learning workloads using supported numerical formats.

---

# Precision

AI workloads can use different numerical precisions.

Examples include:

```text
FP32
FP16
BF16
INT8
```

The appropriate precision depends on:

- model
- framework
- hardware
- performance requirements
- accuracy requirements

Lower-precision computation can reduce memory requirements and improve performance on compatible hardware.

---

# CUDA and AI Frameworks

AI frameworks such as PyTorch use CUDA to access NVIDIA GPUs.

The simplified stack is:

```text
Python
  |
  v
PyTorch
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

The developer normally interacts with PyTorch rather than writing CUDA kernels directly.

This is one reason modern AI development can use GPU acceleration without requiring every developer to become a CUDA programmer.

---

# CUDA and LocalAI

The LocalAI experiment uses the GPU-enabled container environment:

```text
localai/localai:latest-gpu-nvidia-cuda-12
```

The conceptual stack is:

```text
Application
     |
     v
LocalAI
     |
     v
Inference Backend
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

The exact backend and runtime can vary depending on the model being used.

---

# CUDA and ComfyUI

ComfyUI commonly relies on PyTorch for model execution.

The architecture is:

```text
ComfyUI
   |
   v
PyTorch
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

During image generation, CUDA-enabled operations can execute on the GPU.

This is particularly important for diffusion models because their inference process involves many tensor operations.

---

# CUDA and Diffusion

A simplified diffusion pipeline is:

```text
Prompt
  |
  v
Conditioning
  |
  v
Model
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

Many of the mathematical operations involved can be accelerated using CUDA.

The result is significantly faster inference than many CPU-only configurations.

---

# CUDA Memory

CUDA workloads use GPU memory.

For AI:

```text
GPU VRAM
 |
 +-- Model
 |
 +-- Tensors
 |
 +-- Activations
 |
 +-- Latents
 |
 +-- Runtime Buffers
 |
 +-- Other Processes
```

When the total requirement exceeds available memory, an out-of-memory error can occur.

---

# CUDA Out of Memory

A typical error is conceptually:

```text
CUDA out of memory
```

This does not necessarily mean:

> "CUDA is broken."

It often means:

> **The requested workload cannot fit into the currently available GPU memory.**

Possible causes include:

- large model
- high image resolution
- large batch
- multiple models
- VAE memory usage
- other GPU processes

---

# Handling CUDA OOM

Possible solutions include:

```text
Reduce Resolution
        |
        v
Reduce Batch Size
        |
        v
Use Smaller Model
        |
        v
Use Quantization
        |
        v
Use Offloading
        |
        v
Free GPU Memory
```

The appropriate solution depends on the application.

---

# CPU Fallback

Sometimes an application silently or explicitly falls back to CPU execution.

Conceptually:

```text
Application
    |
    v
CUDA Available?
    |
 +--+--+
 |     |
Yes    No
 |     |
 v     v
GPU   CPU
```

CPU fallback can make an application appear to work while being dramatically slower.

Therefore, successful application startup does not prove that CUDA acceleration is active.

---

# Detecting CPU Fallback

Useful indicators include:

```text
GPU Utilization → near 0%
VRAM Usage      → unchanged
CPU Usage       → very high
Generation      → unexpectedly slow
```

For Python applications:

```python
import torch

print(torch.cuda.is_available())
```

For containerized applications, inspect the application logs and GPU utilization on the host.

---

# CUDA Troubleshooting

A structured diagnostic path is:

```text
1. Check GPU
       |
       v
2. Check NVIDIA Driver
       |
       v
3. Check Docker GPU Access
       |
       v
4. Check CUDA Runtime
       |
       v
5. Check Framework
       |
       v
6. Check Application
       |
       v
7. Check Model
```

Avoid changing multiple components simultaneously.

Changing several layers at once makes it difficult to determine what actually fixed the problem.

---

# CUDA Compatibility Checklist

```text
[ ] GPU supported
[ ] NVIDIA driver installed
[ ] nvidia-smi works
[ ] Docker working
[ ] NVIDIA Container Toolkit available
[ ] GPU visible inside container
[ ] CUDA runtime available
[ ] PyTorch / backend supports CUDA
[ ] Application selects CUDA
[ ] Model supported
[ ] Enough VRAM available
```

This checklist is useful when bringing a new AI application into the Local AI Lab.

---

# Common CUDA Problems

## `torch.cuda.is_available()` returns `False`

Possible causes:

- CPU-only PyTorch installation
- CUDA runtime problem
- driver problem
- incompatible environment
- container GPU access problem

Start by checking:

```bash
nvidia-smi
```

Then inspect the PyTorch installation.

---

## `nvidia-smi` works but PyTorch does not

This usually means the hardware and driver layer is working, but the framework layer has a problem.

Investigate:

```text
NVIDIA Driver
       |
       v
CUDA Runtime
       |
       v
PyTorch Build
```

The GPU can be healthy while the Python environment is misconfigured.

---

## Docker sees the container but not the GPU

Investigate:

```text
Docker
  |
  v
NVIDIA Container Toolkit
  |
  v
NVIDIA Driver
  |
  v
GPU
```

A GPU-enabled image alone is not sufficient.

---

## CUDA error after changing models

If one model works and another fails, investigate the model and workload before assuming that CUDA itself is broken.

For example:

```text
Model A
   |
   v
Works

Model B
   |
   v
CUDA OOM
```

This may simply indicate that Model B requires more VRAM.

---

# CUDA and Reproducibility

CUDA is one reason AI environments can be difficult to reproduce.

A workload may depend on:

```text
GPU Architecture
Driver
CUDA Runtime
Framework Version
Model
Application
```

Therefore, recording the environment is useful.

A practical experiment record might include:

```text
GPU:
Driver:
CUDA:
PyTorch:
Application:
Model:
VRAM:
```

This makes future troubleshooting much easier.

---

# Recording the Environment

For a local AI experiment, useful diagnostic information includes:

```bash
nvidia-smi
```

and in Python:

```python
import torch

print("PyTorch:", torch.__version__)
print("CUDA available:", torch.cuda.is_available())
print("PyTorch CUDA:", torch.version.cuda)

if torch.cuda.is_available():
    print("GPU:", torch.cuda.get_device_name(0))
```

This creates a useful snapshot of the software stack.

---

# CUDA in the Local AI Stack

The complete Local AI Lab architecture can now be represented as:

```text
                    APPLICATION
                         |
                         v
                LocalAI / ComfyUI
                         |
                         v
                  AI Framework
                         |
                         v
                       CUDA
                         |
                         v
              NVIDIA Container Layer
                         |
                         v
                  NVIDIA Driver
                         |
                         v
                    NVIDIA GPU
```

Docker sits around the application environment:

```text
HOST
 |
 +-- NVIDIA GPU
 |
 +-- NVIDIA Driver
 |
 +-- Docker
       |
       v
   AI Container
       |
       +-- CUDA Runtime
       |
       +-- Framework
       |
       +-- Application
```

---

# What I Learned

The most important lesson from the CUDA layer is:

> **GPU acceleration is a complete software and hardware chain.**

It is not enough to have:

```text
NVIDIA GPU
```

The entire path must work:

```text
GPU
 |
 v
Driver
 |
 v
CUDA
 |
 v
Framework
 |
 v
Application
 |
 v
Model
```

When Docker is involved:

```text
GPU
 |
 v
Driver
 |
 v
Container Toolkit
 |
 v
Docker
 |
 v
Container
 |
 v
CUDA
 |
 v
Framework
 |
 v
Application
```

Understanding this hierarchy makes local AI troubleshooting much more systematic.

---

# CUDA Investigation Checklist

```text
CUDA
────────────────────────────

[ ] NVIDIA GPU detected
[ ] nvidia-smi works
[ ] Driver identified
[ ] CUDA runtime understood
[ ] PyTorch CUDA available
[ ] GPU device detected
[ ] Docker GPU access working
[ ] LocalAI GPU path working
[ ] ComfyUI GPU path working
[ ] VRAM sufficient
[ ] CPU fallback ruled out
[ ] CUDA OOM understood
```

---

# Related Experiments

- [NVIDIA GPU →](/labs/local-ai/nvidia-gpu/)
- [Docker →](/labs/local-ai/docker/)
- [LocalAI →](/labs/local-ai/localai/)
- [ComfyUI →](/labs/local-ai/comfyui/)
- [Local LLMs →](/labs/local-ai/local-llms/)
- [Model Formats →](/labs/local-ai/model-formats/)
- [Local Inference →](/labs/local-ai/local-inference/)
- [GPU Container Troubleshooting →](/labs/local-ai/gpu-container-troubleshooting/)

---

# Lab Status

```text
CUDA
────────────────────────────

CUDA Concepts        ✓
GPU Runtime          ✓
PyTorch CUDA         ✓
Docker CUDA          ✓
LocalAI CUDA         ✓
ComfyUI CUDA         ✓
GPU Troubleshooting  ✓

STATUS: EXPERIMENTAL
```

> **CUDA is the bridge between the AI software stack and NVIDIA GPU compute.**
>
> **Understanding that bridge makes the entire local AI stack easier to debug.**
