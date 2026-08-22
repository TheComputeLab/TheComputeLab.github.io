---
title: "Local Inference"
description: "Understanding local AI inference, model execution, CPU and GPU inference, latency, throughput, memory, and practical benchmarking."
weight: 110
toc: true
---

> **Inference is the moment when a trained model becomes a working AI system.**

Training creates the model.

Inference uses that trained model to produce an output.

In the Local AI Lab, inference is the layer that connects:

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
CPU / GPU
    |
    v
Output
```

For GPU-based local AI:

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
CUDA
    |
    v
NVIDIA GPU
    |
    v
Output
```

This page documents how local inference works and how to measure, optimize, and troubleshoot it.

---

# What Is Inference?

Inference is the process of using a trained model to generate a prediction or output.

For a language model:

```text
Prompt
  |
  v
LLM
  |
  v
Generated Text
```

For an image model:

```text
Prompt / Image
      |
      v
Diffusion Model
      |
      v
Generated Image
```

The model is not being trained during inference.

The learned parameters are being used to calculate the output.

---

# Training vs Inference

### Training

```text
Data
 |
 v
Model
 |
 v
Loss
 |
 v
Parameter Updates
 |
 v
Trained Model
```

### Inference

```text
Input
 |
 v
Trained Model
 |
 v
Output
```

Training generally requires much more computation and data movement.

Inference can often be performed on smaller local hardware.

---

# Local Inference

Local inference means the model runs on infrastructure controlled by the user.

```text
User
 |
 v
Local Application
 |
 v
Local Runtime
 |
 v
Local Model
 |
 v
Local Hardware
```

No remote model API is required for the core inference operation.

---

# Cloud vs Local Inference

| Area | Cloud Inference | Local Inference |
|---|---|---|
| Model execution | Remote | Local |
| Network | Usually required | Not necessarily |
| Hardware | Provider | User |
| Latency | Network + compute | Local compute |
| Privacy control | Provider dependent | Greater local control |
| Cost | Usage based | Hardware/electricity |
| Maintenance | Lower | Higher |
| Scaling | Provider managed | User managed |

Local inference provides more infrastructure control, while cloud inference provides convenience and scalable managed hardware.

---

# Local Inference Architecture

A typical local AI architecture is:

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
             AI RUNTIME
                  |
                  v
                MODEL
                  |
          +-------+-------+
          |               |
          v               v
         CPU             GPU
                          |
                          v
                        CUDA
                          |
                          v
                  NVIDIA DRIVER
                          |
                          v
                         GPU
```

Docker can encapsulate the runtime:

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
       +-- Runtime
       +-- Model
       +-- CUDA
```

---

# Inference Pipeline

A simplified inference pipeline is:

```text
Input
  |
  v
Preprocessing
  |
  v
Tokenization / Encoding
  |
  v
Model
  |
  v
Computation
  |
  v
Decoding / Postprocessing
  |
  v
Output
```

The exact stages depend on the model.

---

# LLM Inference

For a local language model:

```text
User Prompt
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
Generated Text
```

The model generates output token by token.

---

# Image Model Inference

For a diffusion model:

```text
Prompt
  |
  v
Text Conditioning
  |
  v
Initial Noise
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

The process is computationally different from LLM inference but follows the same high-level idea:

```text
Input
  |
  v
Model
  |
  v
Output
```

---

# LocalAI Inference

The LocalAI experiment provides an API-based local inference architecture.

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

This makes the local model accessible through an application-friendly interface.

---

# API-Based Inference

A local application can communicate with an inference server:

```text
Application
     |
     | HTTP Request
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
Response
```

The API abstraction means the application does not necessarily need to know every detail of model execution.

---

# ComfyUI Inference

ComfyUI exposes image-generation inference through a node-based workflow.

```text
Workflow
   |
   v
Model
   |
   v
Conditioning
   |
   v
Sampler
   |
   v
VAE
   |
   v
Image
```

The workflow itself becomes part of the inference configuration.

---

# CPU Inference

Inference can run on the CPU.

```text
Application
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

CPU inference is useful when:

- no GPU is available
- the model is small
- testing is the priority
- GPU memory is insufficient
- the workload does not benefit significantly from GPU acceleration

---

# GPU Inference

GPU inference uses parallel computation.

```text
Application
    |
    v
Framework
    |
    v
CUDA
    |
    v
NVIDIA GPU
    |
    v
Output
```

GPU inference is often substantially faster for workloads that are well suited to parallel execution.

---

# CPU vs GPU

| Feature | CPU | GPU |
|---|---|---|
| Parallelism | Lower | Very high |
| General-purpose work | Excellent | Less general |
| AI acceleration | Limited | Excellent |
| VRAM | No dedicated VRAM | Dedicated VRAM |
| Setup | Usually simple | Driver/CUDA stack |
| Large AI workloads | Often slower | Often faster |

A GPU is not automatically better for every workload.

The workload must benefit from parallel computation.

---

# CUDA in Inference

For NVIDIA GPU inference:

```text
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

CUDA provides the software layer that allows AI frameworks to execute compatible operations on NVIDIA hardware.

---

# GPU Device Selection

An AI framework may select a device such as:

```text
CPU
```

or:

```text
CUDA
```

For example, in Python:

```python
import torch

device = "cuda" if torch.cuda.is_available() else "cpu"

print(device)
```

This provides a basic indication of which execution path is available.

---

# GPU Availability

A useful PyTorch diagnostic is:

```python
import torch

print("CUDA available:", torch.cuda.is_available())

if torch.cuda.is_available():
    print("GPU:", torch.cuda.get_device_name(0))
```

This tests the framework layer.

It should be combined with host-level checks such as:

```bash
nvidia-smi
```

---

# Model Loading

Inference begins with loading the model.

Conceptually:

```text
Model File
    |
    v
Loader
    |
    v
Model in Memory
    |
    v
Ready for Inference
```

Memory may be:

```text
System RAM
```

or:

```text
GPU VRAM
```

or a combination of both.

---

# Model Loading vs Inference

These are different stages.

### Model Loading

```text
Disk
 |
 v
RAM / VRAM
```

### Inference

```text
RAM / VRAM
 |
 v
Compute
 |
 v
Output
```

A model can load successfully and still fail during inference because inference may require additional memory.

---

# VRAM During Inference

GPU memory can be consumed by:

```text
Model Weights
+
Activations
+
KV Cache
+
Latents
+
VAE
+
Temporary Buffers
+
Other Processes
```

Therefore:

```text
Model Size
```

does not equal:

```text
Total VRAM Requirement
```

---

# Context and Memory

For LLM inference, longer context can increase memory requirements.

Conceptually:

```text
Short Context
    |
    v
Smaller Memory Requirement
```

while:

```text
Long Context
    |
    v
Larger KV Cache
    |
    v
Higher Memory Requirement
```

This is particularly important for long conversations and document processing.

---

# Batch Inference

Batching means processing multiple inputs together.

```text
Input 1 ─┐
Input 2 ─┼──> Model ──> Outputs
Input 3 ─┘
```

Batching can improve throughput.

However:

```text
Larger Batch
     |
     v
Higher Memory Requirement
```

Therefore, batching must be balanced against available hardware.

---

# Latency

Latency is the time required to produce a response.

For example:

```text
Request
  |
  |---- 1 second ----|
  |
  v
Response
```

For interactive AI applications, low latency is often important.

---

# Throughput

Throughput measures how much work can be processed over time.

For LLMs, a common metric is:

```text
tokens / second
```

For image generation, useful metrics can include:

```text
images / minute
```

or:

```text
seconds / image
```

---

# Latency vs Throughput

These are related but different.

```text
Latency
 |
 +-- How long until this response?
```

while:

```text
Throughput
 |
 +-- How much work can be processed over time?
```

An optimized system may prioritize one over the other depending on the workload.

---

# Tokens Per Second

For LLM inference, tokens per second is a useful performance metric.

Conceptually:

```text
Generated Tokens
----------------
Generation Time
=
Tokens / Second
```

For example:

```text
200 tokens
---------
10 seconds

= 20 tokens/sec
```

The exact calculation depends on what part of the generation process is being measured.

---

# Time to First Token

For interactive LLM applications, another useful metric is:

> **Time to First Token (TTFT)**

Conceptually:

```text
Request
   |
   |---- TTFT ----|
   |
   v
First Token
```

After the first token, the rest of the response can continue streaming.

---

# Streaming

Streaming sends output incrementally instead of waiting for the complete response.

Without streaming:

```text
Request
   |
   v
Wait
   |
   v
Complete Response
```

With streaming:

```text
Request
   |
   v
Token 1
   |
   v
Token 2
   |
   v
Token 3
   |
   v
...
```

Streaming improves perceived responsiveness.

---

# Quantization

Quantization reduces the numerical precision used to represent model weights.

Conceptually:

```text
Full Precision
      |
      v
Quantization
      |
      v
Smaller Model Representation
      |
      v
Lower Memory Requirement
```

This is particularly useful for local LLM inference.

---

# Quantization Trade-off

Quantization can provide:

```text
Smaller Model
+
Lower Memory
+
Potentially Faster Inference
```

but may involve:

```text
Some Quality Loss
```

depending on the model and quantization method.

The appropriate quantization level depends on the hardware and task.

---

# CPU Offloading

When a model is too large to fit entirely into VRAM, some workloads can use CPU offloading.

Conceptually:

```text
Model
 |
 +---- GPU VRAM
 |
 +---- System RAM
```

This reduces GPU memory requirements but may increase data movement and latency.

---

# GPU Offloading

Some inference runtimes can control how much of a model is placed on the GPU.

Conceptually:

```text
Model Layers
 |
 +---- GPU
 |
 +---- CPU
```

More GPU placement can improve performance when sufficient VRAM is available.

More CPU placement can reduce VRAM pressure.

---

# Memory Bandwidth

AI inference is not only about compute cores.

Memory bandwidth also matters.

A simplified model is:

```text
Model
 |
 v
Memory
 |
 v
Compute
 |
 v
Output
```

If data cannot move efficiently between memory and compute units, the GPU may not be fully utilized.

---

# GPU Utilization

GPU utilization indicates how actively the GPU is being used.

A diagnostic workflow is:

```text
Start Inference
      |
      v
Run nvidia-smi
      |
      v
Observe GPU Utilization
      |
      v
Observe VRAM
```

For example:

```text
GPU Utilization: 80%
VRAM: 8 GB / 12 GB
```

can indicate active GPU usage.

But utilization alone does not prove that the system is optimally configured.

---

# CPU Fallback

A local AI application may accidentally run on the CPU.

Possible symptoms:

```text
GPU Utilization → Near 0%
VRAM → Low
CPU → High
Inference → Slow
```

The troubleshooting chain is:

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
Runtime
 |
 v
Application
```

---

# Inference Benchmark

A simple benchmark should record:

```text
Model
Hardware
Runtime
Precision
Quantization
Input Size
Output Size
Latency
Throughput
VRAM
CPU Usage
GPU Usage
```

For an LLM:

```text
Prompt:
Tokens:
TTFT:
Tokens/sec:
Total Time:
VRAM:
```

For image generation:

```text
Model:
Resolution:
Steps:
CFG:
Sampler:
Seed:
Generation Time:
VRAM:
```

---

# Controlled Benchmarking

A useful benchmark changes one variable at a time.

For example:

```text
Model: Same
Prompt: Same
Seed: Same
Hardware: Same
Runtime: Same

Variable:
Steps

20
30
40
```

Record:

```text
Time
VRAM
Quality
```

This produces more meaningful results than changing everything simultaneously.

---

# Local Inference Workflow

A practical workflow is:

```text
Select Model
     |
     v
Check Hardware
     |
     v
Select Runtime
     |
     v
Load Model
     |
     v
Verify Device
     |
     v
Run Baseline
     |
     v
Measure Performance
     |
     v
Optimize
     |
     v
Record Results
```

---

# Inference Optimization

Possible optimization areas include:

```text
Model Size
Quantization
Batch Size
Context Length
Resolution
GPU Offloading
CPU Offloading
Precision
Runtime
CUDA Configuration
```

Optimization should begin with measurement.

> **Measure first, optimize second.**

---

# Docker Inference

A containerized inference environment can look like:

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
       +-- Runtime
       +-- CUDA
       +-- Model
       |
       v
      GPU
```

This provides a reproducible software environment.

---

# Docker GPU Troubleshooting

When inference inside a container is slow or fails:

```text
1. Check Host GPU
       |
       v
2. Check nvidia-smi
       |
       v
3. Check Container GPU Access
       |
       v
4. Check CUDA
       |
       v
5. Check Framework
       |
       v
6. Check Runtime
       |
       v
7. Check Model
```

A working host GPU does not automatically guarantee a working container GPU.

---

# Common Inference Problems

## Model loads but inference fails

Possible causes:

- insufficient memory
- unsupported operation
- incompatible runtime
- CUDA issue
- model architecture mismatch

Check the logs and memory usage.

---

## Inference is extremely slow

Check:

```text
CPU vs GPU
GPU Utilization
VRAM
Model Size
Quantization
Offloading
Context
Resolution
```

---

## CUDA out of memory

Try:

```text
Reduce Batch
Reduce Resolution
Use Smaller Model
Use Quantization
Reduce Context
Free VRAM
Use Offloading
```

---

## GPU is visible but not used

Check:

```text
nvidia-smi
torch.cuda.is_available()
Application Logs
Runtime Configuration
```

---

# Local Inference Security

A local inference server may expose an HTTP API.

For example:

```text
localhost:8080
```

This should not automatically be exposed to the public Internet.

A safer architecture is:

```text
Local Application
      |
      v
localhost
      |
      v
Inference Server
```

If remote access is required, authentication, network controls and access restrictions should be considered.

---

# Model Persistence

Models should ideally be stored outside disposable containers.

```text
Host
 |
 +-- models/
 |
 +-- Docker
       |
       v
   Inference Container
       |
       v
   Mounted Model
```

This makes container replacement easier.

---

# Reproducibility

A good local inference record contains:

```text
Model:
Model Version:
Format:
Quantization:
Runtime:
Hardware:
GPU:
VRAM:
CUDA:
Driver:
Input:
Output:
Latency:
Throughput:
```

This makes experiments repeatable.

---

# Local Inference Experiment Template

```text
Experiment ID:
Date:

MODEL:
MODEL VERSION:
FORMAT:
QUANTIZATION:

RUNTIME:

GPU:
VRAM:
CUDA:
DRIVER:

INPUT:

OUTPUT:

LATENCY:
TTFT:
THROUGHPUT:

GPU UTILIZATION:
CPU UTILIZATION:

OBSERVATIONS:

RESULT:
```

---

# What I Learned

The most important lesson is:

> **Inference is the point where every layer of the local AI stack has to work together.**

The complete chain is:

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
Memory
     |
     v
CPU / GPU
     |
     v
CUDA
     |
     v
Driver
     |
     v
Hardware
```

A model can be valid.

The GPU can be valid.

CUDA can be installed.

Docker can be running.

And inference can still fail if the layers are not correctly connected.

---

# Local Inference Checklist

```text
LOCAL INFERENCE
────────────────────────────

[ ] Model selected
[ ] Model format verified
[ ] Runtime selected
[ ] Model loaded
[ ] GPU / CPU device verified
[ ] CUDA checked
[ ] VRAM checked
[ ] Input validated
[ ] Baseline inference completed
[ ] Latency measured
[ ] Throughput measured
[ ] GPU utilization checked
[ ] CPU utilization checked
[ ] Optimization tested
[ ] Results recorded
```

---

# Related Experiments

- [LocalAI →](/labs/local-ai/localai/)
- [Local LLMs →](/labs/local-ai/local-llms/)
- [Model Formats →](/labs/local-ai/modelformats/)
- [ComfyUI →](/labs/local-ai/comfyui/)
- [Stable Diffusion →](/labs/local-ai/stable-diffusion/)
- [DreamShaper →](/labs/local-ai/dreamshaper/)
- [Image Generation →](/labs/local-ai/imagegeneration/)
- [NVIDIA GPU →](/labs/local-ai/nvidia-gpu/)
- [CUDA →](/labs/local-ai/cuda/)
- [Docker →](/labs/local-ai/docker/)

---

# Lab Status

```text
LOCAL INFERENCE
────────────────────────────

CPU Inference          ✓
GPU Inference          ✓
CUDA                   ✓
LocalAI                ✓
ComfyUI                ✓
Latency                ✓
Throughput             ✓
VRAM Monitoring        ✓
Quantization           ✓
Offloading             ✓
Benchmarking           ✓
Troubleshooting        ✓

STATUS: EXPERIMENTAL
```

> **A local model becomes useful when inference is reliable, measurable, and reproducible.**
>
> **The goal of the Local AI Lab is not only to run models, but to understand the infrastructure that makes them run.**
