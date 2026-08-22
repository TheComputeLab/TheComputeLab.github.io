---
title: "Local LLMs"
description: "Understanding local large language models, model formats, quantization, inference, VRAM, and local deployment."
weight: 60
toc: true
---

> **Run the model where you control the infrastructure.**

A Local LLM is a large language model that runs on infrastructure controlled by the user rather than being accessed exclusively through a remote cloud API.

A simplified architecture is:

```text
User
 |
 v
Application
 |
 v
Local LLM Runtime
 |
 v
Model
 |
 +---- CPU
 |
 +---- GPU
```

In the Local AI Lab, Local LLMs connect the application layer with the infrastructure we have already explored:

```text
Application
     |
     v
LocalAI
     |
     v
Local LLM
     |
     v
CUDA
     |
     v
NVIDIA GPU
```

---

# What Is a Local LLM?

An LLM is a machine-learning model trained to process and generate language.

A cloud-based architecture usually looks like:

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
Remote LLM
```

A local architecture looks like:

```text
Application
     |
     v
Local API
     |
     v
Local LLM
     |
     v
Local Hardware
```

The model and inference environment are hosted locally.

---

# Local vs Cloud LLMs

| Area | Cloud LLM | Local LLM |
|---|---|---|
| Model location | Remote | Local |
| Internet | Usually required | Can operate offline |
| Hardware | Provider managed | User managed |
| Cost | Usage based | Hardware/electricity |
| Privacy control | Provider dependent | Higher local control |
| Scaling | Provider managed | User managed |
| Maintenance | Lower | Higher |
| Model choice | Provider dependent | Broad local choice |
| Latency | Network dependent | Local |
| Customization | API dependent | High |

Neither approach is universally better.

The appropriate choice depends on the workload.

---

# Why Run an LLM Locally?

Local LLMs can be useful for:

- experimentation
- learning
- development
- offline applications
- privacy-sensitive workloads
- prototyping
- model comparison
- custom AI systems
- reducing repeated API usage

For a lab environment, local inference is especially valuable because the entire stack can be observed.

---

# The Local LLM Stack

A complete local LLM environment can be represented as:

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
            LLM RUNTIME
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

Docker can surround the runtime:

```text
HOST
 |
 +-- GPU
 |
 +-- Driver
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

# Model Weights

An LLM is represented by learned parameters, commonly called model weights.

During training, the model learns numerical values that encode patterns from its training data.

A simplified representation is:

```text
Training Data
      |
      v
Training Process
      |
      v
Model Parameters
      |
      v
Model Weights
```

During inference, these learned weights are used to generate responses.

The model is not "thinking" in the human sense.

It performs mathematical operations over its learned parameters and the input context.

---

# Parameters

Models are often described using parameter counts:

```text
1B
3B
7B
8B
13B
30B
70B
```

The `B` commonly represents billions of parameters.

A larger parameter count generally means a larger model, but:

> **More parameters does not automatically mean a better model for every task.**

Model architecture, training data, optimization, quantization, context length and task suitability also matter.

---

# Model Size and Hardware

A larger model generally requires more memory.

A simplified relationship is:

```text
More Parameters
       |
       v
Larger Model
       |
       v
More Memory
       |
       v
Higher Hardware Requirement
```

This is why local LLM selection should begin with available hardware.

---

# VRAM Requirements

For GPU inference, model weights may need to be loaded into VRAM.

Conceptually:

```text
Model Weights
      +
Runtime Memory
      +
KV Cache
      +
Temporary Tensors
      |
      v
Total VRAM Requirement
```

Therefore, available VRAM is one of the first constraints when selecting a local LLM.

---

# Quantization

Quantization reduces the numerical precision used to represent model parameters.

Common concepts include:

```text
FP32
FP16
BF16
INT8
4-bit
5-bit
6-bit
8-bit
```

A simplified idea is:

```text
Higher Precision
       |
       v
More Memory
       |
       v
Potentially Higher Resource Requirement
```

while:

```text
Lower Precision
       |
       v
Less Memory
       |
       v
Potentially Faster / More Accessible
```

The trade-off is that reducing precision can affect model quality depending on the method and model.

---

# Why Quantization Matters Locally

A model that is too large to fit into available VRAM may become usable after quantization.

Conceptually:

```text
Large Model
     |
     v
Quantization
     |
     v
Smaller Memory Footprint
     |
     v
Fits Hardware
```

This is one of the key techniques that makes local LLM experimentation practical on consumer hardware.

---

# GGUF

GGUF is a model file format commonly associated with local LLM inference ecosystems.

A simplified workflow is:

```text
Model
  |
  v
GGUF
  |
  v
Local Runtime
  |
  v
CPU / GPU
```

GGUF can package model information and weights in a format designed for efficient local inference.

The exact compatibility depends on the runtime and model architecture.

---

# Model Format Matters

A model file is not automatically compatible with every inference application.

For example:

```text
Model Format
     |
     v
Runtime Compatibility
     |
     v
Inference
```

Possible formats and ecosystems include:

```text
GGUF
Safetensors
Checkpoint formats
Diffusers structures
```

The correct choice depends on the model architecture and runtime.

This is why **Model Formats** will be treated as a separate topic in the Local AI Lab.

---

# Tokens

LLMs process text using tokens rather than raw characters or words.

A simplified pipeline is:

```text
Text
 |
 v
Tokenizer
 |
 v
Tokens
 |
 v
Model
 |
 v
Generated Tokens
 |
 v
Detokenizer
 |
 v
Text
```

For example:

```text
"Hello world"
```

is converted into a sequence of tokens.

The exact tokenization depends on the tokenizer used by the model.

---

# Tokenization

A tokenizer converts text into numerical token identifiers.

Conceptually:

```text
Human Text
     |
     v
Tokenizer
     |
     v
Token IDs
     |
     v
Model
```

The model works with these token representations rather than directly with the original text.

---

# Context Window

An LLM processes a limited amount of context at a time.

This is often called the context window.

Conceptually:

```text
Context Window
+--------------------------------+
| System Prompt                  |
| Conversation                   |
| User Input                     |
| Retrieved Information          |
+--------------------------------+
```

The maximum context depends on the model and runtime.

A larger context window can be useful for:

- long documents
- coding sessions
- conversations
- RAG
- structured analysis

But larger context can also increase memory requirements.

---

# Prompt Processing

A simplified inference process is:

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
Model
     |
     v
Next Token Prediction
     |
     v
Generated Token
     |
     v
Repeat
```

The model generates output token by token.

---

# Autoregressive Generation

For many language models, generation works iteratively.

Conceptually:

```text
Input
 |
 v
Predict Token 1
 |
 v
Predict Token 2
 |
 v
Predict Token 3
 |
 .
 .
 .
 |
 v
Final Response
```

Each newly generated token becomes part of the context used to generate subsequent tokens.

---

# Temperature

Temperature influences the randomness of token selection.

A simplified interpretation is:

```text
Lower Temperature
       |
       v
More deterministic
```

while:

```text
Higher Temperature
       |
       v
More varied
```

The useful value depends on the task.

For factual or structured tasks, lower randomness may be preferable.

For creative generation, greater variation may be useful.

---

# Sampling

The model typically produces probabilities for possible next tokens.

Conceptually:

```text
Prompt
  |
  v
Model
  |
  v
Token Probabilities
  |
  +---- Token A
  +---- Token B
  +---- Token C
  +---- ...
  |
  v
Sampling Strategy
  |
  v
Selected Token
```

Parameters such as temperature and other sampling controls influence how the next token is selected.

---

# CPU vs GPU Inference

A local LLM can potentially run on a CPU or GPU.

### CPU

```text
LLM
 |
 v
CPU
 |
 v
Output
```

### GPU

```text
LLM
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

GPU inference is often much faster for sufficiently large workloads.

However, CPU inference can still be useful for:

- small models
- testing
- systems without dedicated GPUs
- low-concurrency workloads

---

# GPU Offloading

Some local inference systems can divide the workload between CPU and GPU.

Conceptually:

```text
Model
 |
 +---- GPU
 |
 +---- CPU
```

This can allow models larger than available VRAM to run, although performance may be reduced.

The trade-off is:

```text
More GPU
   |
   v
Faster
```

versus:

```text
More CPU Offloading
   |
   v
Lower VRAM Requirement
   |
   v
Potentially Slower
```

---

# KV Cache

During autoregressive generation, inference runtimes often maintain a key-value cache, commonly called the KV cache.

Conceptually:

```text
Previous Context
       |
       v
KV Cache
       |
       v
Next Token Generation
```

The KV cache avoids recomputing certain information repeatedly.

However, a larger context can require more memory for the cache.

Therefore:

```text
Longer Context
      |
      v
Larger KV Cache
      |
      v
Higher Memory Requirement
```

---

# LocalAI + Local LLMs

The LocalAI experiment provides a convenient API layer for local inference.

A simplified architecture is:

```text
Application
     |
     | HTTP API
     v
LocalAI
     |
     v
Model Runtime
     |
     v
Local LLM
     |
     +---- CPU
     |
     +---- GPU
           |
           v
         CUDA
```

The application communicates with LocalAI instead of directly managing every model-runtime detail.

---

# API-Based Local Inference

A local application can communicate with the model through an API:

```text
Python / Web App
       |
       | HTTP
       v
localhost:8080
       |
       v
LocalAI
       |
       v
Local Model
```

This architecture makes it possible to build applications that behave similarly to cloud-API integrations while keeping the inference environment local.

---

# Local LLM Application Architecture

A larger application might look like:

```text
                 USER
                  |
                  v
             Web Interface
                  |
                  v
             Application
                  |
                  v
             Local API
                  |
                  v
                LocalAI
                  |
                  v
             Local LLM
                  |
          +-------+-------+
          |               |
          v               v
        CPU             GPU
                          |
                          v
                        CUDA
```

This architecture can be used as the foundation for local AI assistants and private AI applications.

---

# Model Selection

Choosing a local LLM should consider more than parameter count.

Useful factors include:

```text
Model Size
Architecture
Quantization
VRAM Requirement
Context Length
Task Performance
Inference Speed
License
Language Support
Runtime Compatibility
```

A practical selection process is:

```text
Define Task
    |
    v
Check Hardware
    |
    v
Estimate VRAM
    |
    v
Choose Model Size
    |
    v
Choose Quantization
    |
    v
Check Runtime Compatibility
    |
    v
Test
```

---

# Hardware-Aware Model Selection

A useful rule is:

> **Choose the model based on the hardware you actually have, not the model you wish you had.**

For example:

```text
Available VRAM
       |
       v
Model Options
       |
       v
Quantization Options
       |
       v
Performance Test
```

This makes local experimentation much more predictable.

---

# Local LLM Performance

Performance depends on several variables:

```text
Model
 |
 +-- Parameter Count
 +-- Quantization
 +-- Architecture
 +-- Context Length
 |
Hardware
 |
 +-- GPU
 +-- VRAM
 +-- Memory Bandwidth
 +-- CPU
 |
Runtime
 |
 +-- Backend
 +-- Optimization
 +-- GPU Offloading
```

Therefore, comparing models only by parameter count is insufficient.

---

# Measuring Inference

Useful metrics can include:

```text
Time to First Token
Tokens per Second
Total Generation Time
VRAM Usage
GPU Utilization
CPU Utilization
```

A simple conceptual benchmark is:

```text
Prompt
   |
   v
Start Timer
   |
   v
Generate
   |
   v
Stop Timer
   |
   v
Measure:
- latency
- tokens/sec
- memory
```

This allows different models or configurations to be compared.

---

# Privacy Considerations

A major reason to run local models is control over data flow.

Cloud:

```text
User Data
   |
   v
Internet
   |
   v
Remote Infrastructure
```

Local:

```text
User Data
   |
   v
Local Application
   |
   v
Local Model
```

Local inference can reduce exposure to external services, although the overall privacy of a system still depends on the application, logging, network configuration, operating system and storage practices.

---

# Offline AI

A local model can potentially operate without an Internet connection once the required software and model files are available.

Conceptually:

```text
Model
  +
Runtime
  +
Application
  |
  v
Offline AI
```

This can be useful for:

- isolated environments
- demonstrations
- experimentation
- privacy-sensitive workloads
- unreliable connectivity

---

# Common Local LLM Problems

## Model does not load

Possible causes:

- incompatible model format
- incorrect model path
- insufficient memory
- unsupported architecture
- runtime configuration
- corrupted model file

Diagnostic sequence:

```text
Check File
   |
   v
Check Format
   |
   v
Check Runtime
   |
   v
Check Memory
   |
   v
Check Logs
```

---

## Out of VRAM

Possible causes:

```text
Large Model
+
Long Context
+
Large KV Cache
+
Other GPU Workloads
```

Possible solutions:

```text
Smaller Model
      OR
Quantization
      OR
Shorter Context
      OR
GPU Offloading
      OR
Free VRAM
```

---

## Inference is too slow

Possible causes:

- CPU inference
- insufficient GPU
- excessive CPU offloading
- large model
- large context
- inefficient runtime
- competing workloads

Check:

```text
GPU Utilization
VRAM Usage
CPU Usage
Tokens/sec
```

---

# Local LLM Troubleshooting

A structured troubleshooting sequence is:

```text
1. Model File
       |
       v
2. Model Format
       |
       v
3. Runtime Compatibility
       |
       v
4. Available Memory
       |
       v
5. CPU / GPU Device
       |
       v
6. CUDA
       |
       v
7. Application Logs
       |
       v
8. Performance
```

This is much more effective than changing multiple settings randomly.

---

# Model Storage

Large model files should be stored outside disposable containers when using Docker.

A practical architecture is:

```text
Host
 |
 +-- models/
 |     |
 |     +-- LLM-1
 |     +-- LLM-2
 |     +-- LLM-3
 |
 +-- Docker
       |
       v
    LocalAI
       |
       v
    Model
```

This allows the runtime container to be recreated without downloading every model again.

---

# Reproducibility

A local LLM experiment should record:

```text
Model
Model Format
Quantization
Runtime
GPU
VRAM
CUDA
Driver
Context Length
Sampling Settings
```

This allows the experiment to be reproduced later.

A useful experiment record might look like:

```text
MODEL:
FORMAT:
QUANTIZATION:
RUNTIME:
GPU:
VRAM:
CUDA:
DRIVER:
CONTEXT:
TEMPERATURE:
TOKENS/SEC:
```

---

# Local LLM Experiment Workflow

A practical workflow is:

```text
Select Task
     |
     v
Select Model
     |
     v
Check Model Format
     |
     v
Check Hardware
     |
     v
Load Model
     |
     v
Run Prompt
     |
     v
Measure Performance
     |
     v
Adjust Parameters
     |
     v
Record Results
```

This turns casual experimentation into a reproducible lab experiment.

---

# What I Learned

The most important lesson from local LLM experimentation is:

> **Running an LLM locally is a hardware, software and model-selection problem at the same time.**

The complete chain is:

```text
Application
     |
     v
API
     |
     v
Runtime
     |
     v
Model
     |
     v
Quantization
     |
     v
CPU / GPU
     |
     v
CUDA
     |
     v
NVIDIA Driver
     |
     v
Hardware
```

A model that looks attractive on paper may not be practical on a particular machine.

The best local model is usually the one that provides a useful balance between:

```text
Quality
Speed
Memory
Cost
Privacy
Compatibility
```

---

# Local LLM Checklist

```text
LOCAL LLM
────────────────────────────

[ ] Task defined
[ ] Model selected
[ ] Model format verified
[ ] Quantization understood
[ ] VRAM checked
[ ] Context length checked
[ ] Runtime selected
[ ] CUDA available
[ ] GPU detected
[ ] Model loaded
[ ] Inference tested
[ ] Tokens/sec measured
[ ] VRAM monitored
[ ] Results recorded
```

---

# Related Experiments

- [LocalAI →](/labs/local-ai/localai/)
- [ComfyUI →](/labs/local-ai/comfyui/)
- [Docker →](/labs/local-ai/docker/)
- [NVIDIA GPU →](/labs/local-ai/nvidia-gpu/)
- [CUDA →](/labs/local-ai/cuda/)
- [Model Formats →](/labs/local-ai/model-formats/)
- [Local Inference →](/labs/local-ai/local-inference/)
- [GPU Container Troubleshooting →](/labs/local-ai/gpu-container-troubleshooting/)

---

# Lab Status

```text
LOCAL LLMs
────────────────────────────

Local Inference       ✓
Model Loading         ✓
GPU Inference         ✓
Quantization          ✓
VRAM Investigation    ✓
LocalAI Integration   ✓
Performance Testing   ✓

STATUS: EXPERIMENTAL
```

> **A local LLM is more than a model file.**
>
> **It is the combination of model, runtime, hardware and inference configuration.**
