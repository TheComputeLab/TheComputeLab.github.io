---
title: "ComfyUI"
description: "Exploring node-based AI image generation, workflows, models, and GPU inference with ComfyUI."
weight: 20
toc: true
---

> **Build the workflow. Understand the pipeline. Generate locally.**

ComfyUI is a node-based interface for building and executing generative AI workflows.

Instead of hiding the image-generation process behind a single button, ComfyUI exposes the individual components of the pipeline.

That makes it particularly useful for experimentation and understanding how generative AI systems actually work.

---

# Why ComfyUI?

Many image-generation applications present the process as:

```text
Prompt
  |
  v
Generate
  |
  v
Image
```

ComfyUI exposes what happens between those steps.

A simplified workflow looks like:

```text
Prompt
   |
   v
Text Encoder
   |
   v
Conditioning
   |
   +------------------+
   |                  |
   v                  v
Checkpoint         Negative
  Model             Prompt
   |                  |
   +--------+---------+
            |
            v
         Sampler
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

This makes the system much easier to experiment with because each stage can be inspected and modified independently.

---

# Lab Objective

The objective of this experiment was to understand:

- how ComfyUI workflows are constructed
- how nodes communicate
- how models are loaded
- how checkpoints are used
- how prompts become conditioning
- how sampling produces latent representations
- how the VAE converts latent data into images
- how GPU acceleration affects generation
- how different components can be connected into reusable workflows

The broader goal is:

> **Understand the image-generation pipeline rather than simply pressing Generate.**

---

# ComfyUI Architecture

A useful way to think about ComfyUI is as a directed graph.

```text
                  ┌───────────────┐
                  │    PROMPT     │
                  └───────┬───────┘
                          |
                          v
                  ┌───────────────┐
                  │ TEXT ENCODER  │
                  └───────┬───────┘
                          |
                          v
                  ┌───────────────┐
                  │ CONDITIONING  │
                  └───────┬───────┘
                          |
                          v
┌───────────────┐   ┌───────────────┐
│ CHECKPOINT    │-->|    SAMPLER    │
│ MODEL         │   └───────┬───────┘
└───────────────┘           |
                            v
                    ┌───────────────┐
                    │ LATENT IMAGE  │
                    └───────┬───────┘
                            |
                            v
                    ┌───────────────┐
                    │      VAE      │
                    └───────┬───────┘
                            |
                            v
                    ┌───────────────┐
                    │     IMAGE     │
                    └───────────────┘
```

Each box represents a node or processing stage.

Connections between nodes represent data flowing through the workflow.

---

# Nodes

The fundamental building block of ComfyUI is the **node**.

A node performs a specific operation.

Examples include:

- loading a model
- encoding text
- creating conditioning
- sampling
- decoding a latent
- saving an image

A workflow is created by connecting these nodes.

Conceptually:

```text
Node A
   |
   v
Node B
   |
   v
Node C
   |
   v
Output
```

The workflow therefore becomes a visual representation of the inference pipeline.

---

# Workflow Graph

Unlike a traditional linear application, ComfyUI workflows can branch.

For example:

```text
                    Checkpoint
                         |
              +----------+----------+
              |                     |
              v                     v
       Positive Prompt       Negative Prompt
              |                     |
              +----------+----------+
                         |
                         v
                      Sampler
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

This graph-based structure makes complex workflows possible.

The same model can feed multiple operations.

Multiple conditioning paths can be combined.

Different processing stages can be inserted without rebuilding the entire application.

---

# The Core Components

## Checkpoint

The checkpoint contains the model weights required for image generation.

A simplified representation is:

```text
Checkpoint
     |
     +---- Model
     |
     +---- Text Encoder
     |
     +---- VAE
```

The exact contents depend on the model architecture and format.

A checkpoint is therefore much more than an ordinary image file.

It contains learned parameters that define how the model behaves.

---

# Text Prompt

The prompt describes what the model should generate.

For example:

```text
a futuristic laboratory,
high detail,
dramatic lighting,
cinematic composition
```

The text itself is not directly understood by the diffusion model.

It must first be converted into a numerical representation.

That happens through the text encoder.

---

# Text Encoding

The simplified process is:

```text
Text
 |
 v
Tokenizer
 |
 v
Text Encoder
 |
 v
Conditioning
```

The resulting conditioning information is passed to the diffusion process.

This is one of the important differences between:

```text
Human language
```

and:

```text
Model representation
```

The model operates on numerical representations rather than raw text.

---

# Positive and Negative Conditioning

A typical workflow contains two conditioning paths.

```text
Positive Prompt
      |
      v
Positive Conditioning
      |
      +--------+
               |
               v
            Sampler
               ^
               |
      +--------+
      |
      v
Negative Conditioning
      ^
      |
Negative Prompt
```

The positive prompt describes what we want.

The negative prompt can describe characteristics we want to discourage.

For example:

```text
Positive:

detailed architectural visualization,
modern laboratory,
high quality
```

and:

```text
Negative:

blurry,
low quality,
distorted,
text,
watermark
```

---

# Latent Space

A diffusion model does not normally generate the final image directly at every step.

Instead, the generation process operates in a latent representation.

Simplified:

```text
Prompt
  |
  v
Conditioning
  |
  v
Latent Representation
  |
  v
Denoising
  |
  v
Final Latent
  |
  v
VAE
  |
  v
Image
```

The latent representation is smaller and more efficient to process than the full-resolution image representation.

This is one reason diffusion models can perform their computations efficiently compared with operating directly on raw pixels throughout the entire process.

---

# Sampler

The sampler controls the iterative denoising process.

A simplified representation is:

```text
Random Noise
     |
     v
Denoising Step 1
     |
     v
Denoising Step 2
     |
     v
Denoising Step 3
     |
     .
     .
     .
     |
     v
Final Latent
```

The sampler uses the model and conditioning information to progressively transform noise into a meaningful latent representation.

---

# Sampling Steps

The number of sampling steps influences the generation process.

Conceptually:

```text
Few Steps
   |
   v
Faster generation
```

while:

```text
More Steps
   |
   v
More computation
```

More steps do not automatically guarantee a better image.

The useful range depends on the model, sampler and workflow.

This is something that is best understood experimentally.

---

# CFG

CFG stands for **Classifier-Free Guidance**.

It controls how strongly the generation process follows the conditioning.

A simplified interpretation is:

```text
Lower CFG
     |
     v
More freedom
```

versus:

```text
Higher CFG
     |
     v
Stronger prompt guidance
```

Very high values are not automatically better.

Excessive guidance can produce unnatural or over-processed results.

---

# VAE

VAE stands for **Variational Autoencoder**.

In a typical image-generation workflow, the VAE is responsible for converting between:

```text
Image Space
     ↕
Latent Space
```

For generation:

```text
Latent
  |
  v
VAE Decode
  |
  v
Image
```

For image-to-image workflows, the reverse direction can also be used:

```text
Image
  |
  v
VAE Encode
  |
  v
Latent
```

This makes the VAE an important part of the overall pipeline.

---

# GPU Acceleration

Image generation is computationally intensive.

A simplified architecture is:

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

The GPU performs the large number of parallel mathematical operations required by the model.

This is why GPU memory and GPU compute capability are important when running ComfyUI locally.

---

# VRAM

VRAM is one of the practical limitations when running image-generation models locally.

GPU memory is consumed by things such as:

- model weights
- activations
- latent tensors
- VAE operations
- image resolution
- batch size
- additional models
- workflow components

A simplified relationship is:

```text
Larger Model
      +
Higher Resolution
      +
Larger Batch
      +
More Components
      |
      v
Higher VRAM Requirement
```

This is why a workflow that works at one resolution may fail at another.

---

# Resolution

Image resolution has a major impact on memory usage.

For example:

```text
512 × 512
```

requires significantly less memory than:

```text
1024 × 1024
```

because the number of pixels increases substantially.

For experimentation, it is often useful to start with a smaller resolution.

Then increase it once the workflow is stable.

---

# Model Loading

A typical ComfyUI workflow begins by loading the required model.

Conceptually:

```text
Model File
    |
    v
Model Loader
    |
    +---- Model
    |
    +---- CLIP / Text Encoder
    |
    +---- VAE
```

The exact outputs depend on the model and node being used.

Correct model placement is therefore important.

---

# Model Directories

ComfyUI expects models to be available in appropriate model directories.

A simplified structure may look like:

```text
ComfyUI/
│
├── models/
│   │
│   ├── checkpoints/
│   ├── clip/
│   ├── vae/
│   ├── loras/
│   ├── controlnet/
│   └── upscale_models/
│
├── custom_nodes/
│
└── workflow/
```

The exact directories depend on the model type and ComfyUI configuration.

This separation helps keep different model components organized.

---

# DreamShaper Experiment

One of the models explored in the Local AI Lab is **DreamShaper**.

The experiment provides a useful example of the relationship between:

```text
Model
  |
  v
Model Format
  |
  v
ComfyUI
  |
  v
Workflow
  |
  v
CUDA
  |
  v
GPU
  |
  v
Generated Image
```

This is particularly useful because the same model file is not automatically compatible with every AI application.

The runtime must understand the model format and architecture.

---

# Model Formats

One of the practical challenges when experimenting with local AI is understanding model formats.

Examples include:

```text
.safetensors
.ckpt
```

and model repositories organized using structures such as:

```text
Diffusers/
```

The important question is not simply:

> "Do I have the model?"

but:

> **"Can this runtime load this model in this format?"**

This is why model format becomes its own topic in the Local AI Lab.

---

# Workflow Files

ComfyUI workflows can be saved and reused.

This makes it possible to create:

```text
Workflow A
   |
   +-- Basic Text-to-Image

Workflow B
   |
   +-- High Resolution

Workflow C
   |
   +-- Image-to-Image

Workflow D
   |
   +-- Advanced Generation
```

A workflow can therefore become a reusable experiment.

Instead of remembering every node and connection, the workflow itself records the configuration.

---

# Reproducibility

One of the strongest advantages of a node-based workflow is reproducibility.

A generation can be described using parameters such as:

```text
Model
Prompt
Negative Prompt
Seed
Sampler
Steps
CFG
Resolution
VAE
```

If these parameters are preserved, the experiment can be reproduced much more easily.

This is particularly useful when comparing different models or generation settings.

---

# Troubleshooting

## Model not appearing

Possible causes:

- model placed in the wrong directory
- unsupported format
- incorrect model configuration
- ComfyUI needs to be restarted
- model scanner did not detect the file

A useful sequence is:

```text
Check File
    |
    v
Check Directory
    |
    v
Check Format
    |
    v
Restart ComfyUI
    |
    v
Check Model List
```

---

## CUDA / GPU problems

If generation falls back to CPU or fails during execution, check:

```text
NVIDIA Driver
      |
      v
CUDA
      |
      v
PyTorch
      |
      v
ComfyUI
      |
      v
Model
```

The GPU being visible to Windows does not automatically guarantee that PyTorch and ComfyUI are using it correctly.

---

## Out of memory

If ComfyUI reports a CUDA out-of-memory error, investigate:

```text
Model Size
Resolution
Batch Size
VAE
Additional Nodes
Other GPU Processes
```

Possible approaches include:

```text
Lower Resolution
       OR
Smaller Batch
       OR
Smaller Model
       OR
Memory Optimization
       OR
Close Other GPU Applications
```

---

# What I Learned

The biggest lesson from ComfyUI is that **generative AI is a pipeline**.

Instead of:

```text
Prompt → Image
```

the actual process is closer to:

```text
Prompt
  |
  v
Tokenizer
  |
  v
Text Encoder
  |
  v
Conditioning
  |
  +----------------+
  |                |
  v                v
Positive         Negative
  |                |
  +-------+--------+
          |
          v
       Sampler
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

ComfyUI makes this pipeline visible.

That is what makes it particularly valuable as a learning and experimentation environment.

---

# ComfyUI vs Traditional Interfaces

| Feature | Traditional UI | ComfyUI |
|---|---|---|
| Workflow visibility | Low | High |
| Node-based | Usually no | Yes |
| Customization | Limited | High |
| Reproducibility | Moderate | High |
| Advanced workflows | Limited | Excellent |
| Learning curve | Lower | Higher |
| Experimentation | Moderate | Excellent |

ComfyUI trades simplicity for control.

For experimentation, that trade-off is often worthwhile.

---

# Lab Workflow

The experiments in this section can be viewed as:

```text
MODEL
  |
  v
MODEL FORMAT
  |
  v
COMFYUI
  |
  v
WORKFLOW
  |
  v
CUDA
  |
  v
GPU
  |
  v
INFERENCE
  |
  v
IMAGE
```

Each layer can be investigated independently.

---

# Related Experiments

- [LocalAI →](/labs/local-ai/localai/)
- [Docker →](/labs/local-ai/docker/)
- [NVIDIA GPU →](/labs/local-ai/nvidia-gpu/)
- [CUDA →](/labs/local-ai/cuda/)
- [Stable Diffusion →](/labs/local-ai/stable-diffusion/)
- [DreamShaper →](/labs/local-ai/dreamshaper/)
- [Image Generation →](/labs/local-ai/image-generation/)
- [Model Formats →](/labs/local-ai/model-formats/)
- [Local Inference →](/labs/local-ai/local-inference/)
- [GPU Container Troubleshooting →](/labs/local-ai/gpu-container-troubleshooting/)

---

# Lab Status

```text
COMFYUI
────────────────────────────

Node Workflows      ✓
Model Loading       ✓
Image Generation    ✓
GPU Experiments     ✓
DreamShaper         ✓

STATUS: EXPERIMENTAL
```

> **ComfyUI turns the black box of image generation into a visible graph.**
>
> **Once you can see the pipeline, you can experiment with it.**
