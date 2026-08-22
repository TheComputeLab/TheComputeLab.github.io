---
title: "Docker for AI"
description: "Using Docker to build reproducible, GPU-enabled environments for local AI workloads."
weight: 30
toc: true
---

> **Package the environment. Isolate the workload. Make the stack reproducible.**

Docker is one of the most useful pieces of infrastructure in a local AI environment.

AI applications often depend on a complicated combination of:

- Python
- system libraries
- CUDA
- GPU drivers
- inference runtimes
- model files
- environment variables
- application dependencies

Docker allows many of these software components to be packaged into a predictable environment.

---

# Why Docker for AI?

A traditional AI installation might look like:

```text
Host Operating System
        |
        +-- Python
        |
        +-- PyTorch
        |
        +-- CUDA libraries
        |
        +-- AI Application
        |
        +-- Model files
```

Over time, dependencies can conflict.

One application may require one version of a library while another application requires something different.

Docker changes the model:

```text
Host
 |
 +-------------------+
 | Docker            |
 |                   |
 | +---------------+ |
 | | AI Container  | |
 | |               | |
 | | Application   | |
 | | Runtime       | |
 | | Dependencies  | |
 | +---------------+ |
 +-------------------+
```

The application becomes isolated from much of the host software environment.

---

# Lab Objective

The objective of this experiment was to understand how Docker can be used to run local AI workloads.

The focus includes:

- Docker images
- Docker containers
- volumes
- port mapping
- environment variables
- GPU access
- NVIDIA Container Toolkit
- model persistence
- container lifecycle
- LocalAI containers
- GPU-enabled AI containers
- troubleshooting

The goal is not simply to learn Docker commands.

The goal is to understand:

> **What happens between the host system and the AI application inside the container?**

---

# Docker Architecture

A simplified AI container architecture looks like:

```text
┌─────────────────────────────────────────┐
│               HOST SYSTEM               │
│                                         │
│  Windows / Linux                        │
│                                         │
│  NVIDIA GPU                             │
│       │                                 │
│       ▼                                 │
│  NVIDIA Driver                          │
│       │                                 │
│       ▼                                 │
│  Docker                                 │
│       │                                 │
│       ▼                                 │
│ ┌─────────────────────────────────────┐ │
│ │          AI CONTAINER               │ │
│ │                                     │ │
│ │  Application                        │ │
│ │  Runtime                            │ │
│ │  Libraries                          │ │
│ │  Configuration                      │ │
│ │                                     │ │
│ └─────────────────────────────────────┘ │
│                                         │
└─────────────────────────────────────────┘
```

The container does not replace the host.

It provides an isolated execution environment on top of the host.

---

# Image vs Container

Two Docker concepts are particularly important.

## Image

An image is a packaged template containing the software required to create a container.

For example:

```text
LocalAI GPU Image
```

can contain:

```text
Application
Runtime
Libraries
Configuration
CUDA-related components
```

An image is not the running application.

---

## Container

A container is a running instance created from an image.

The relationship can be simplified as:

```text
Docker Image
      |
      | docker run
      v
Docker Container
      |
      v
Running Application
```

One image can be used to create multiple containers.

---

# The LocalAI Example

In the Local AI Lab, the GPU-enabled LocalAI image used in the experiment was:

```text
localai/localai:latest-gpu-nvidia-cuda-12
```

Conceptually:

```text
Docker Image
      |
      v
LocalAI Container
      |
      +---- API
      |
      +---- Model Storage
      |
      +---- GPU Access
      |
      +---- CUDA
```

This is a useful real-world example of Docker being used for AI infrastructure.

---

# Container Lifecycle

A container has a lifecycle.

A simplified flow is:

```text
Image
  |
  v
Create
  |
  v
Start
  |
  v
Running
  |
  +---- Stop
  |
  +---- Restart
  |
  v
Remove
```

This distinction matters because deleting a container does not necessarily mean deleting all the data associated with the application.

That depends on where the data is stored.

---

# Containers Should Be Disposable

One of the most useful Docker concepts for AI is:

> **Treat containers as replaceable.**

For example:

```text
Container A
    |
    X
Removed
```

can be replaced with:

```text
Container B
    |
    v
Same Application
    |
    v
Same Persistent Models
```

This works when important data is stored outside the container filesystem.

---

# Persistent Model Storage

AI models can be large.

A model might occupy hundreds of megabytes or several gigabytes.

It is therefore inefficient to treat the model as disposable container data.

A better architecture is:

```text
HOST
 |
 +-- models/
 |     |
 |     +-- model-1
 |     +-- model-2
 |     +-- model-3
 |
 +-- Docker Container
        |
        v
      LocalAI
```

The host directory can be mounted into the container.

---

# Docker Volumes

Docker provides persistent storage mechanisms such as volumes and bind mounts.

A simplified bind-mount architecture is:

```text
Host Directory
      |
      | mount
      v
Container Directory
      |
      v
AI Application
```

For AI applications this can be useful for:

- model files
- generated images
- configuration
- application data
- logs

The container can then be recreated without losing the important data.

---

# Bind Mounts vs Container Storage

Consider two approaches.

### Container-only storage

```text
Container
   |
   +-- Models
```

If the container is removed:

```text
Container
   X
Data may be lost
```

### Persistent storage

```text
Host
 |
 +-- Models
       |
       v
   Container
```

If the container is removed:

```text
Container
   X

Models
   ✓
```

For large AI models, persistent storage is generally the more practical design.

---

# Port Mapping

AI applications often expose HTTP APIs.

For example, the LocalAI experiment used:

```text
localhost:8080
```

Conceptually:

```text
Host
 |
 | Port 8080
 v
Docker
 |
 | Port mapping
 v
Container
 |
 v
LocalAI API
```

A port mapping connects a host port to a container port.

This allows applications outside the container to communicate with the service inside it.

---

# Application Communication

A local AI application can communicate with a containerized service like this:

```text
Python Application
       |
       | HTTP
       v
localhost:8080
       |
       v
Docker Container
       |
       v
LocalAI
       |
       v
Model
```

The application does not necessarily need to know that LocalAI is inside Docker.

It simply communicates with the API endpoint.

---

# Environment Variables

Containers often use environment variables for configuration.

Conceptually:

```text
Host / Docker
      |
      v
Environment Variables
      |
      v
Application
```

Common examples in AI environments include settings related to:

- model paths
- API configuration
- logging
- GPU behavior
- application options

Environment variables are useful because configuration can be changed without rebuilding the container image.

---

# Docker and NVIDIA GPUs

Running an AI container on a CPU is relatively straightforward.

GPU acceleration introduces another layer.

The architecture becomes:

```text
AI Application
      |
      v
Docker Container
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

The container needs a way to access the host GPU.

This is where NVIDIA's container tooling becomes important.

---

# NVIDIA Container Toolkit

The NVIDIA Container Toolkit provides the integration required for NVIDIA GPUs to be exposed to containers.

The conceptual path is:

```text
Container
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

This means GPU access is not simply a property of the Docker image.

The host environment and container runtime must also be configured correctly.

---

# GPU Visibility

A useful troubleshooting distinction is:

```text
GPU visible on host
        ≠
GPU visible inside container
```

For example:

```text
Host
 |
 +-- nvidia-smi
 |      |
 |      ✓
 |
 +-- Docker
        |
        +-- GPU Container
               |
               +-- GPU?
```

If the host sees the GPU but the container does not, investigate the Docker/NVIDIA integration.

---

# Checking the Host GPU

A first diagnostic command is:

```bash
nvidia-smi
```

This can show:

- GPU model
- driver version
- VRAM usage
- GPU utilization
- running processes

This establishes whether the host can communicate with the NVIDIA GPU.

It does not prove that Docker containers can access it.

---

# GPU Container Troubleshooting

A useful troubleshooting hierarchy is:

```text
GPU
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
Container
 |
 v
CUDA
 |
 v
PyTorch / Runtime
 |
 v
AI Application
 |
 v
Model
```

When an AI workload fails, move through these layers rather than immediately assuming the model is broken.

---

# CUDA and Containers

CUDA introduces another important concept.

There may be differences between:

```text
Host Driver
```

and:

```text
CUDA Runtime inside the Container
```

The relationship can be simplified as:

```text
Host
 |
 +-- NVIDIA Driver
 |
 +-- GPU
 |
 +-- Docker
       |
       +-- Container
             |
             +-- CUDA Runtime
             |
             +-- AI Framework
```

Compatibility between these components is important.

---

# Why Container Images Matter

A Docker image can package a specific software environment.

For example:

```text
AI Image
 |
 +-- Application
 |
 +-- Python
 |
 +-- Libraries
 |
 +-- Runtime
 |
 +-- CUDA-related components
```

This makes it possible to distribute an environment that is much closer to a known working configuration.

That is particularly useful in machine learning.

---

# Reproducibility

One of Docker's biggest advantages is reproducibility.

Without containers:

```text
Machine A
 |
 +-- Python version X
 +-- Library version Y
 +-- CUDA version Z
```

Machine B may have:

```text
Machine B
 |
 +-- Python version X2
 +-- Library version Y2
 +-- CUDA version Z2
```

The application may behave differently.

With a container:

```text
Docker Image
     |
     +------------------+
     |                  |
     v                  v
Machine A           Machine B
     |                  |
     +--------+---------+
              |
              v
        Similar Runtime
```

Containers therefore help reduce environment differences.

---

# LocalAI + Docker

The LocalAI experiment demonstrates the complete concept:

```text
                 HOST
                  |
          ┌───────┴────────┐
          |                |
       NVIDIA GPU       Filesystem
          |                |
          v                v
       Docker         Model Directory
          |
          v
┌─────────────────────────────┐
│       LocalAI Container     │
│                             │
│       LocalAI API           │
│            |                │
│         Runtime             │
│            |                │
│          CUDA               │
└─────────────┬───────────────┘
              |
              v
             GPU
```

This is a practical example of containerized AI infrastructure.

---

# Docker + ComfyUI

The same architecture can be applied to image-generation workloads.

Conceptually:

```text
Host
 |
 +-- NVIDIA GPU
 |
 +-- Model Directory
 |
 +-- Docker
       |
       v
   ComfyUI Container
       |
       +-- Workflow
       |
       +-- Model
       |
       +-- PyTorch
       |
       +-- CUDA
       |
       v
      GPU
```

This makes Docker useful not only for LocalAI but also for other local AI applications.

---

# Why Separate Models from Images?

Docker images can be rebuilt or updated.

Models can be large and may remain unchanged for a long time.

Therefore:

```text
Application Environment
        |
        v
Docker Image
```

should be separated from:

```text
Model Data
        |
        v
Persistent Storage
```

This gives:

```text
Update Container
       |
       v
Keep Models
       |
       v
Continue Experiment
```

---

# Container Logs

When a containerized AI application fails, logs are often the first place to investigate.

The useful conceptual flow is:

```text
Application
     |
     v
Container
     |
     v
Logs
     |
     v
Error
     |
     v
Diagnosis
```

Logs can reveal:

- application startup failures
- model loading errors
- CUDA errors
- missing files
- configuration problems
- runtime failures

---

# Common Docker Problems

## Container exits immediately

Possible causes:

- application startup failure
- invalid configuration
- missing dependency
- incorrect command
- missing model
- permission problem

Start with the container logs.

---

## Port already in use

If another process is already using the host port:

```text
Host Port
    |
    X
Already in use
```

the container may fail to expose the service.

The solution is to either stop the conflicting service or choose another host port.

---

## Model not found

Possible causes:

```text
Wrong Host Path
      |
      v
Wrong Mount
      |
      v
Wrong Container Path
      |
      v
Application Cannot Find Model
```

When using mounted model directories, always verify both sides of the mapping.

---

## GPU not available

Possible causes include:

- NVIDIA driver issue
- NVIDIA Container Toolkit issue
- Docker configuration
- incorrect GPU-enabled image
- CUDA/runtime incompatibility
- application configuration

Use the troubleshooting hierarchy rather than changing multiple components at once.

---

# Docker Troubleshooting Method

A useful method is to isolate the layers.

### Step 1 — Host

```bash
nvidia-smi
```

Does the host see the GPU?

### Step 2 — Docker

Can Docker run containers normally?

### Step 3 — GPU Container

Can the container access the GPU?

### Step 4 — CUDA

Can the runtime communicate with CUDA?

### Step 5 — Application

Does LocalAI or ComfyUI start?

### Step 6 — Model

Can the application load the model?

This gives a structured diagnostic path:

```text
HOST
 ↓
DOCKER
 ↓
GPU
 ↓
CUDA
 ↓
APPLICATION
 ↓
MODEL
```

---

# Container Lifecycle and AI Experiments

Docker is particularly useful during experimentation.

A typical experiment might look like:

```text
Create Environment
       |
       v
Run Experiment
       |
       v
Observe Results
       |
       v
Modify Configuration
       |
       v
Recreate Container
       |
       v
Run Again
```

If the model files and configuration are persistent, the container itself can be recreated repeatedly.

This makes experimentation much less risky.

---

# What I Learned

The most important lesson from using Docker with local AI is:

> **The container is only one layer of the system.**

A working AI environment depends on:

```text
Host
 |
 v
GPU
 |
 v
Driver
 |
 v
Docker
 |
 v
NVIDIA Container Toolkit
 |
 v
Container
 |
 v
CUDA
 |
 v
Runtime
 |
 v
AI Application
 |
 v
Model
```

When something fails, knowing this hierarchy makes debugging much easier.

---

# Docker vs Native Installation

| Area | Native | Docker |
|---|---|---|
| Installation | Direct | Containerized |
| Isolation | Lower | Higher |
| Reproducibility | Moderate | High |
| Dependency conflicts | More likely | Reduced |
| GPU setup | Direct | Additional runtime layer |
| Model persistence | Filesystem | Volumes / mounts |
| Port management | Direct | Explicit mapping |
| Updates | Modify environment | Replace container/image |
| Learning curve | Lower initially | Higher initially |

Docker adds complexity at first.

But that complexity can pay off when managing multiple AI applications.

---

# Lab Workflow

The Docker layer can be viewed as:

```text
HOST
  |
  v
NVIDIA DRIVER
  |
  v
NVIDIA CONTAINER TOOLKIT
  |
  v
DOCKER
  |
  v
AI CONTAINER
  |
  +----------------+
  |                |
  v                v
LocalAI          ComfyUI
  |                |
  +-------+--------+
          |
          v
       CUDA / GPU
          |
          v
        MODEL
```

This architecture connects Docker directly to the other experiments in the Local AI Lab.

---

# Key Concepts

```text
IMAGE
  |
  v
CONTAINER
  |
  +---- PORT
  |
  +---- VOLUME
  |
  +---- ENVIRONMENT
  |
  +---- GPU
  |
  v
APPLICATION
```

Understanding these five concepts covers a large part of practical Docker usage for AI.

---

# Related Experiments

- [LocalAI →](/labs/local-ai/localai/)
- [ComfyUI →](/labs/local-ai/comfyui/)
- [NVIDIA GPU →](/labs/local-ai/nvidia-gpu/)
- [CUDA →](/labs/local-ai/cuda/)
- [Local LLMs →](/labs/local-ai/local-llms/)
- [Model Formats →](/labs/local-ai/model-formats/)
- [Local Inference →](/labs/local-ai/local-inference/)
- [GPU Container Troubleshooting →](/labs/local-ai/gpu-container-troubleshooting/)

---

# Lab Status

```text
DOCKER FOR AI
────────────────────────────

Containers          ✓
GPU Containers      ✓
Model Mounts        ✓
Port Mapping        ✓
LocalAI             ✓
GPU Experiments     ✓

STATUS: EXPERIMENTAL
```

> **Docker makes the AI environment reproducible.**
>
> **The GPU makes it fast.**
>
> **Understanding both makes it debuggable.**
