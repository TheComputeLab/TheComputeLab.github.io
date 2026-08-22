---
title: "NVIDIA GPU"
description: "Understanding NVIDIA GPUs, VRAM, drivers, utilization, and GPU acceleration for local AI."
weight: 40
toc: true
---

> **The GPU is where the mathematics becomes compute.**

Modern AI workloads can perform enormous numbers of parallel mathematical operations.

A CPU can handle these operations, but GPUs are designed specifically for highly parallel workloads.

For local AI, an NVIDIA GPU also becomes the foundation for the CUDA software stack used by many AI frameworks and inference applications.

This page documents the role of the NVIDIA GPU in the Local AI Lab and how it connects to Docker, CUDA, PyTorch, LocalAI, and ComfyUI.

---

# Why a GPU for AI?

A simplified CPU architecture looks like:

```text
CPU
 |
 +-- General-purpose computation
 |
 +-- Few powerful cores
 |
 +-- Sequential / mixed workloads
```

A GPU is designed differently:

```text
GPU
 |
 +-- Many parallel compute resources
 |
 +-- Massive parallel workloads
 |
 +-- High memory bandwidth
 |
 +-- Matrix / tensor-oriented computation
```

AI workloads contain many operations that can be executed in parallel.

That makes GPUs particularly effective for deep-learning inference and training.

---

# The Local AI GPU Stack

The GPU is not an isolated component.

A typical local AI environment looks like:

```text
AI Application
      |
      v
Inference Runtime
      |
      v
PyTorch / Backend
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

When Docker is involved:

```text
AI Application
      |
      v
Docker Container
      |
      v
CUDA / Runtime
      |
      v
NVIDIA Container Toolkit
      |
      v
NVIDIA Driver
      |
      v
NVIDIA GPU
```

Every layer has a different responsibility.

---

# Lab Objective

The objective of this experiment was to understand:

- how the NVIDIA GPU participates in AI inference
- how GPU memory affects model loading
- how to inspect GPU state
- how NVIDIA drivers connect software to hardware
- how CUDA uses the GPU
- how Docker exposes GPUs to containers
- how to diagnose GPU-related problems
- why a GPU being visible on the host does not always mean that an AI application is using it

The key question is:

> **Can the entire software stack actually reach and use the GPU?**

---

# GPU Detection

The first diagnostic tool is:

```bash
nvidia-smi
```

This is one of the most useful commands in an NVIDIA-based AI environment.

It provides information about:

- GPU model
- driver version
- temperature
- power usage
- memory usage
- GPU utilization
- running processes

A simplified conceptual output looks like:

```text
+------------------------------------------------------+
| NVIDIA-SMI                                           |
+------------------------------------------------------+
| GPU  Name             Memory Usage    GPU-Util       |
|------------------------------------------------------|
| 0    NVIDIA GPU       4GB / 12GB      35%            |
+------------------------------------------------------+
```

The exact output depends on the installed GPU and driver.

---

# What `nvidia-smi` Tells You

`nvidia-smi` is useful because it provides visibility into the hardware and driver layer.

For example:

```text
GPU
 |
 +-- Name
 |
 +-- VRAM
 |
 +-- Utilization
 |
 +-- Temperature
 |
 +-- Power
 |
 +-- Processes
 |
 +-- Driver
```

However, there is an important limitation:

> **`nvidia-smi` showing a GPU does not prove that your AI application is using it.**

It only establishes that the NVIDIA driver can communicate with the GPU.

---

# GPU Utilization

GPU utilization indicates how actively the GPU is being used.

Conceptually:

```text
0%
 |
 | Idle
 |
50%
 |
 | Active workload
 |
100%
 |
 | Heavy utilization
```

During an AI generation workload, utilization may increase substantially.

However, utilization is not a simple measure of "AI performance."

A workload can have:

```text
High VRAM
Low GPU Utilization
```

or:

```text
Low VRAM
High GPU Utilization
```

depending on what the application is doing.

---

# VRAM

VRAM is the GPU's dedicated memory.

For local AI, VRAM is one of the most important hardware constraints.

A simplified model is:

```text
GPU
 |
 +-- Compute
 |
 +-- VRAM
```

The model weights need memory.

But the model is not the only thing consuming VRAM.

Memory can also be used by:

- activations
- latent tensors
- KV cache
- VAE operations
- intermediate tensors
- multiple loaded models
- application overhead

---

# Model Size vs VRAM

A simplified relationship is:

```text
Larger Model
      |
      v
More Memory Required
      |
      v
Higher VRAM Requirement
```

For example, a small model may comfortably fit on a consumer GPU while a much larger model may require:

```text
Quantization
   OR
CPU Offloading
   OR
Multiple GPUs
   OR
More VRAM
```

This is why model selection should consider the target hardware.

---

# VRAM Is Not Just Model Size

A common misconception is:

> "If the model is 8 GB, I need exactly 8 GB of VRAM."

In practice, additional memory may be required.

Conceptually:

```text
Model Weights
      +
Runtime Memory
      +
Activations
      +
Buffers
      +
Other GPU Workloads
      |
      v
Total VRAM Requirement
```

Therefore, a model that appears to fit mathematically may still produce an out-of-memory error.

---

# GPU Memory During Image Generation

Image-generation workloads introduce additional memory requirements.

A simplified relationship is:

```text
Model
  +
Resolution
  +
Batch Size
  +
VAE
  +
Additional Models
      |
      v
GPU Memory Usage
```

Increasing image resolution can significantly increase memory requirements.

For experimentation, it is often useful to start with smaller resolutions and gradually increase them.

---

# NVIDIA Driver

The NVIDIA driver is the software layer that allows applications and GPU compute runtimes to communicate with the hardware.

A simplified architecture is:

```text
Application
     |
     v
CUDA / Runtime
     |
     v
NVIDIA Driver
     |
     v
GPU
```

The driver is therefore fundamental.

If the driver is missing or malfunctioning:

```text
Application
     |
     X
    GPU
```

The AI application cannot use the GPU correctly.

---

# CUDA

CUDA provides the programming and runtime ecosystem used to perform GPU computation on NVIDIA hardware.

The simplified relationship is:

```text
AI Framework
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

CUDA is not the same thing as the NVIDIA driver.

They are different layers.

This distinction becomes important when troubleshooting compatibility problems.

---

# CUDA Toolkit vs CUDA Runtime

The terms CUDA Toolkit and CUDA Runtime are often confused.

A simplified distinction is:

### CUDA Toolkit

A development environment containing tools and libraries used to build CUDA applications.

### CUDA Runtime

The components required by an application to execute CUDA workloads.

In containerized AI environments, the container may contain its own CUDA runtime components while still relying on the host NVIDIA driver.

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
       +-- AI Container
              |
              +-- CUDA Runtime
              |
              +-- AI Framework
```

This is one reason containerized CUDA environments can appear confusing initially.

---

# GPU Access from Docker

A Dockerized AI application requires GPU access to be configured correctly.

The conceptual path is:

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
GPU-enabled Container
    |
    v
AI Application
```

If any layer is broken, the application may fall back to CPU or fail.

---

# Host GPU vs Container GPU

This is one of the most important troubleshooting concepts.

Consider:

```text
Host
 |
 +-- nvidia-smi
       |
       ✓ GPU detected
```

This tells us:

```text
Host → GPU
```

But we also need:

```text
Container
 |
 +-- GPU
       |
       ✓
```

The two checks are different.

A healthy host GPU does not automatically guarantee healthy container GPU access.

---

# NVIDIA Container Toolkit

The NVIDIA Container Toolkit provides the integration between Docker and NVIDIA GPUs.

The architecture is:

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

This allows GPU-aware containers to access the hardware.

For local AI, this layer is especially important when using:

- LocalAI
- ComfyUI
- PyTorch containers
- CUDA-based inference services
- other GPU-enabled AI workloads

---

# GPU-enabled AI Container

A simplified container architecture is:

```text
┌──────────────────────────────────┐
│         AI CONTAINER             │
│                                  │
│  Application                     │
│      |                           │
│      v                           │
│  Runtime                         │
│      |                           │
│      v                           │
│  CUDA                            │
└───────────────┬──────────────────┘
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

This is the architecture behind many local AI GPU workloads.

---

# LocalAI + NVIDIA GPU

The LocalAI experiment in this Lab used a GPU-enabled Docker image:

```text
localai/localai:latest-gpu-nvidia-cuda-12
```

The conceptual architecture was:

```text
Application
     |
     v
LocalAI API
     |
     v
Docker Container
     |
     v
CUDA
     |
     v
NVIDIA GPU
```

This provides a practical example of how the GPU becomes part of the local inference stack.

---

# ComfyUI + NVIDIA GPU

ComfyUI follows a similar architecture:

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
NVIDIA GPU
```

During image generation, the GPU performs the computationally expensive operations required by the model.

GPU memory is used for:

```text
Model
Latents
Activations
VAE
Intermediate Tensors
```

---

# CPU vs GPU Inference

A simplified comparison:

```text
CPU Inference

Application
    |
    v
CPU
    |
    v
Output
```

versus:

```text
GPU Inference

Application
    |
    v
CUDA
    |
    v
GPU
    |
    v
Output
```

GPU inference is often substantially faster for workloads that can take advantage of parallel computation.

However, GPU acceleration is not guaranteed simply because a GPU exists.

The application and framework must actually use it.

---

# Detecting CPU Fallback

A common local AI problem is:

> The GPU exists, but the application is running on the CPU.

Possible indicators include:

- GPU utilization remains near zero
- CPU utilization becomes very high
- generation is unexpectedly slow
- application logs mention CPU
- framework reports a CPU device
- VRAM usage remains unchanged

The diagnostic path is:

```text
Application
     |
     v
Framework
     |
     v
Device Selection
     |
     +---- CPU
     |
     +---- GPU
```

The goal is to verify which path is actually being selected.

---

# PyTorch and GPU Detection

For Python-based AI applications, PyTorch can be used to check CUDA availability.

A common diagnostic is:

```python
import torch

print(torch.cuda.is_available())

if torch.cuda.is_available():
    print(torch.cuda.get_device_name(0))
```

Conceptually:

```text
PyTorch
   |
   +-- CUDA available?
          |
          +-- True  → GPU available
          |
          +-- False → investigate
```

This checks the framework layer rather than just the operating system.

---

# A Layered GPU Test

A good diagnostic sequence is:

### Layer 1 — Hardware

```bash
nvidia-smi
```

### Layer 2 — Docker

Confirm Docker is functioning.

### Layer 3 — NVIDIA Container Toolkit

Confirm GPU-enabled containers can access the GPU.

### Layer 4 — Framework

For example:

```python
torch.cuda.is_available()
```

### Layer 5 — Application

Check whether LocalAI, ComfyUI, or another application is actually using the GPU.

The complete path is:

```text
Hardware
   |
   v
Driver
   |
   v
Container Runtime
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

---

# Common GPU Problems

## GPU not detected

Possible causes:

- NVIDIA driver problem
- hardware issue
- virtualization configuration
- unsupported environment
- container configuration

Start with:

```bash
nvidia-smi
```

---

## CUDA unavailable

Possible causes:

- incorrect PyTorch build
- incompatible CUDA runtime
- driver issue
- environment problem
- CPU-only framework installation

Check the framework:

```python
import torch

print(torch.cuda.is_available())
print(torch.version.cuda)
```

---

## GPU works but application is slow

Possible causes include:

- CPU fallback
- insufficient VRAM
- model offloading
- memory pressure
- incorrect runtime configuration
- other GPU workloads

Check:

```text
GPU Utilization
VRAM Usage
CPU Usage
Application Logs
```

---

# Out of Memory

A CUDA out-of-memory error means the workload requires more GPU memory than is currently available.

Possible causes:

```text
Large Model
+
High Resolution
+
Large Batch
+
Multiple Models
+
Other GPU Processes
```

Possible solutions:

```text
Lower Resolution
       OR
Smaller Batch
       OR
Smaller Model
       OR
Quantization
       OR
Offloading
       OR
Free GPU Memory
```

---

# GPU Monitoring

During an experiment, it can be useful to monitor:

```text
GPU Utilization
VRAM Usage
Temperature
Power
Processes
```

A useful mental model is:

```text
             GPU
              |
      +-------+-------+
      |       |       |
     VRAM    Util   Temp
      |       |       |
      +-------+-------+
              |
          Workload
```

This helps determine whether the workload is actually reaching the GPU.

---

# Multiple AI Applications

A local machine may run several GPU-enabled applications.

For example:

```text
                NVIDIA GPU
                     |
        +------------+------------+
        |                         |
     LocalAI                    ComfyUI
        |                         |
      LLM                     Diffusion
```

Both applications compete for:

- VRAM
- compute
- memory bandwidth
- power
- system resources

This is why closing unused GPU applications can sometimes resolve out-of-memory problems.

---

# GPU as a Shared Resource

The GPU should be treated like a shared infrastructure resource.

```text
                 GPU
                  |
       +----------+----------+
       |          |          |
    LocalAI    ComfyUI    Other Apps
       |          |          |
       +----------+----------+
                  |
                VRAM
```

When multiple workloads are running, the total memory demand matters.

---

# GPU Troubleshooting Method

When an AI workload fails, move through the stack systematically.

```text
1. Is the GPU detected?
          |
          v
2. Is the NVIDIA driver working?
          |
          v
3. Can Docker access the GPU?
          |
          v
4. Is CUDA available?
          |
          v
5. Does PyTorch see the GPU?
          |
          v
6. Does the application select the GPU?
          |
          v
7. Does the model fit in VRAM?
```

This approach avoids changing several components at the same time.

---

# What I Learned

The biggest lesson from working with local AI GPUs is:

> **Having a GPU is only the beginning.**

The complete chain must work:

```text
GPU
 |
 v
NVIDIA Driver
 |
 v
CUDA
 |
 v
Framework
 |
 v
AI Application
 |
 v
Model
```

For containers:

```text
GPU
 |
 v
Driver
 |
 v
NVIDIA Container Toolkit
 |
 v
Docker
 |
 v
CUDA
 |
 v
Framework
 |
 v
AI Application
```

A failure anywhere in this chain can look like an AI problem.

Often it is actually an infrastructure problem.

---

# GPU Investigation Checklist

```text
[ ] GPU physically available
[ ] NVIDIA driver installed
[ ] nvidia-smi works
[ ] GPU temperature normal
[ ] GPU memory available
[ ] Docker working
[ ] NVIDIA Container Toolkit configured
[ ] GPU visible inside container
[ ] CUDA available
[ ] PyTorch detects GPU
[ ] Application selects GPU
[ ] Model fits VRAM
[ ] No competing GPU workloads
```

This checklist is useful whenever a new local AI application is introduced.

---

# Related Experiments

- [LocalAI →](/labs/local-ai/localai/)
- [ComfyUI →](/labs/local-ai/comfyui/)
- [Docker →](/labs/local-ai/docker/)
- [CUDA →](/labs/local-ai/cuda/)
- [Local LLMs →](/labs/local-ai/local-llms/)
- [Local Inference →](/labs/local-ai/local-inference/)
- [GPU Container Troubleshooting →](/labs/local-ai/gpu-container-troubleshooting/)

---

# Lab Status

```text
NVIDIA GPU
────────────────────────────

GPU Detection        ✓
nvidia-smi           ✓
VRAM Investigation   ✓
CUDA Concepts        ✓
Docker GPU Access    ✓
LocalAI GPU          ✓
ComfyUI GPU          ✓

STATUS: EXPERIMENTAL
```

> **The model is only one part of the system.**
>
> **The GPU, driver, runtime and software stack determine whether that model can actually run.**
