---
title: "Stable Diffusion"
description: "Understanding diffusion models, latent space, text-to-image generation, checkpoints, sampling, VAE, and local GPU inference."
weight: 70
toc: true
---

> **Start with noise. Guide the denoising process. End with an image.**

Stable Diffusion is a family of generative AI models designed primarily for image generation and related image-to-image tasks.

It became particularly important for local AI experimentation because models can be run on consumer NVIDIA GPUs using tools such as PyTorch and ComfyUI.

A simplified generation pipeline is:

```text
Prompt
  |
  v
Text Encoder
  |
  v
Conditioning
  |
  v
Diffusion Model
  |
  v
Denoising
  |
  v
Latent Representation
  |
  v
VAE
  |
  v
Image
```

---

# What Is Stable Diffusion?

Stable Diffusion is based on diffusion-model techniques.

The core idea is to start with noise and progressively remove that noise while using conditioning information to guide the process toward a meaningful image.

Conceptually:

```text
Random Noise
     |
     v
Denoising
     |
     v
Denoising
     |
     v
Denoising
     |
     .
     .
     .
     |
     v
Image
```

The model learns how to perform this transformation during training.

---

# Lab Objective

The objective of this experiment was to understand:

- how diffusion models generate images
- how text prompts control generation
- what latent diffusion means
- how the text encoder participates
- how the diffusion model performs denoising
- how the VAE converts between image and latent representations
- how checkpoints package model components
- how samplers influence generation
- how CFG affects prompt guidance
- how seeds enable reproducibility
- how Stable Diffusion runs through ComfyUI
- how CUDA and NVIDIA GPUs accelerate generation
- how VRAM affects resolution and workflow complexity

The broader goal is:

> **Understand image generation as a computational pipeline rather than a black box.**

---

# The Diffusion Concept

Imagine an image being progressively corrupted with noise.

Conceptually:

```text
Original Image
      |
      v
Low Noise
      |
      v
More Noise
      |
      v
Heavy Noise
      |
      v
Random Noise
```

During training, the model learns how to reverse this process.

During generation, the process is effectively run in the opposite direction:

```text
Random Noise
      |
      v
Less Noise
      |
      v
Less Noise
      |
      v
Structured Image
```

The model is therefore learning a denoising process.

---

# Why Latent Diffusion?

Stable Diffusion operates primarily in a compressed latent representation rather than directly performing the complete diffusion process over raw pixels.

The simplified architecture is:

```text
Image
  |
  v
VAE Encoder
  |
  v
Latent Space
  |
  v
Diffusion Process
  |
  v
Latent Space
  |
  v
VAE Decoder
  |
  v
Image
```

This significantly reduces the computational workload compared with performing the complete process directly in pixel space.

---

# Latent Space

The latent representation is a compressed representation of visual information.

A simplified relationship is:

```text
Pixel Image
     |
     v
VAE Encoder
     |
     v
Latent
```

and:

```text
Latent
   |
   v
VAE Decoder
   |
   v
Pixel Image
```

The diffusion model works on the latent representation.

---

# Text-to-Image Pipeline

A simplified text-to-image workflow is:

```text
Text Prompt
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
     +------------------+
     |                  |
     v                  v
Diffusion Model     Negative
                    Conditioning
     |
     v
Denoising
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

ComfyUI exposes many of these stages as individual nodes.

---

# Text Encoder

The text encoder converts the prompt into a numerical representation.

Conceptually:

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
```

The diffusion model uses this conditioning to influence the generated image.

This is the bridge between human language and the visual generation process.

---

# CLIP and Text Conditioning

Many Stable Diffusion workflows use CLIP-related text encoding components.

CLIP is a model family designed to connect language and visual representations.

In a simplified Stable Diffusion workflow:

```text
Text
 |
 v
CLIP / Text Encoder
 |
 v
Conditioning
 |
 v
Diffusion Model
```

The exact architecture varies between Stable Diffusion generations and related model families.

---

# UNet

Many Stable Diffusion architectures use a UNet-based diffusion model.

The UNet participates in the denoising process.

Conceptually:

```text
Noisy Latent
     |
     v
   UNet
     |
     v
Noise Prediction
     |
     v
Denoising Step
```

The process repeats across multiple sampling steps.

---

# Denoising

A simplified denoising process is:

```text
Step 1
Random Noise
     |
     v
Noisy Latent

Step 2
Noisy Latent
     |
     v
Less Noise

Step 3
Less Noise
     |
     v
More Structure

...

Final
     |
     v
Clean Latent
```

The conditioning information influences the direction of the denoising process.

---

# Sampler

The sampler determines how the denoising process is executed.

Different samplers can produce different results even when:

```text
Model
Prompt
Seed
Steps
CFG
```

remain similar.

Examples of sampler families commonly encountered in diffusion workflows include:

```text
Euler
Euler a
DDIM
DPM++
```

The exact names and available options depend on the application and model.

---

# Sampling Steps

Sampling steps determine how many iterations are used during denoising.

Conceptually:

```text
Few Steps
   |
   v
Faster
```

while:

```text
More Steps
   |
   v
More Computation
```

Increasing the number of steps does not guarantee a better image.

The useful range depends on the model and sampler.

---

# CFG

CFG stands for **Classifier-Free Guidance**.

It influences how strongly the generation follows the conditioning.

Simplified:

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

Very high CFG values can produce unnatural or over-processed results.

---

# Negative Prompts

A negative prompt can specify characteristics that should be discouraged.

Example:

```text
Positive:
detailed futuristic laboratory,
cinematic lighting,
high detail
```

Negative:

```text
blurry,
low quality,
distorted,
watermark,
text
```

The effectiveness of negative prompting depends on the model architecture and training.

It should be treated as an experimental control rather than a guaranteed filter.

---

# Seeds

A seed controls the initial random state used during generation.

Conceptually:

```text
Seed
  |
  v
Initial Noise
  |
  v
Diffusion Process
  |
  v
Image
```

Using the same:

```text
Model
Prompt
Sampler
Steps
CFG
Resolution
Seed
```

can make a generation reproducible, assuming the same software environment and deterministic behavior.

---

# Why Seeds Matter

Without a controlled seed:

```text
Same Prompt
     |
     +---- Generation A
     |
     +---- Generation B
     |
     +---- Generation C
```

may produce different images.

With a controlled seed:

```text
Same Configuration
        +
Same Seed
        |
        v
More Reproducible Experiment
```

This is particularly useful when comparing model settings.

---

# Resolution

Resolution directly affects computational requirements.

For example:

```text
512 × 512
```

contains:

```text
262,144 pixels
```

while:

```text
1024 × 1024
```

contains:

```text
1,048,576 pixels
```

That is four times as many pixels.

Higher resolution can therefore increase memory and computation requirements substantially.

---

# VRAM

Stable Diffusion workloads use GPU memory for:

- model weights
- latent tensors
- activations
- VAE operations
- intermediate tensors
- additional models
- workflow components

A simplified relationship is:

```text
Model
  +
Resolution
  +
Batch Size
  +
Workflow Complexity
      |
      v
VRAM Requirement
```

This is why a workflow that works at one resolution may fail at another.

---

# Batch Size

Batch size controls how many images are processed together.

Conceptually:

```text
Batch = 1
   |
   v
One generation at a time
```

versus:

```text
Batch = 4
   |
   v
Multiple generations together
```

Increasing batch size can substantially increase memory requirements.

For local experimentation, batch size 1 is often a useful starting point.

---

# VAE

The Variational Autoencoder handles conversion between latent and image representations.

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

For image-to-image:

```text
Image
  |
  v
VAE Encode
  |
  v
Latent
```

The VAE is therefore an important part of the image-generation pipeline.

---

# Checkpoints

A Stable Diffusion checkpoint contains learned model parameters and, depending on the model format and packaging, may include several components required by the workflow.

A simplified representation is:

```text
Checkpoint
     |
     +---- Diffusion Model
     |
     +---- Text Encoder
     |
     +---- VAE
```

The exact contents vary between model families and file formats.

---

# Model Formats

Common model-related formats encountered in local image-generation workflows include:

```text
.safetensors
.ckpt
```

and model repositories based on structures such as:

```text
Diffusers
```

A model file must be compatible with the runtime and architecture.

Therefore:

> **A file extension alone does not guarantee compatibility.**

---

# Stable Diffusion + ComfyUI

ComfyUI makes the Stable Diffusion pipeline visible.

A simplified workflow is:

```text
Checkpoint Loader
       |
       +--------------------+
       |                    |
       v                    v
Model                  Text Encoder
       |                    |
       |             +------+------+
       |             |             |
       |             v             v
       |         Positive       Negative
       |         Conditioning  Conditioning
       |             |             |
       +-------------+-------------+
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

This is one of the main reasons ComfyUI is useful as a learning environment.

---

# Stable Diffusion + NVIDIA GPU

The local inference stack is:

```text
Stable Diffusion
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
NVIDIA GPU
```

The GPU accelerates the tensor operations involved in the diffusion process.

---

# Stable Diffusion + Docker

A containerized deployment can look like:

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
Stable Diffusion Environment
       |
       +-- ComfyUI
       +-- PyTorch
       +-- CUDA Runtime
       +-- Models
```

Persistent model storage can be mounted from the host.

---

# DreamShaper

DreamShaper is one of the image-generation models explored in the Local AI Lab.

The experiment can be represented as:

```text
DreamShaper
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
NVIDIA GPU
     |
     v
Generated Image
```

This experiment connects the model, runtime and infrastructure layers.

---

# Image-to-Image

Stable Diffusion workflows can also start from an existing image.

A simplified pipeline is:

```text
Input Image
     |
     v
VAE Encode
     |
     v
Latent
     |
     v
Denoising
     |
     v
Modified Latent
     |
     v
VAE Decode
     |
     v
Output Image
```

The input image therefore influences the resulting generation.

---

# Denoising Strength

Image-to-image workflows often expose a denoising-strength parameter.

Conceptually:

```text
Lower Denoising
      |
      v
Preserve more of the original
```

while:

```text
Higher Denoising
      |
      v
Allow greater transformation
```

The useful value depends on the task and model.

---

# Text-to-Image vs Image-to-Image

| Feature | Text-to-Image | Image-to-Image |
|---|---|---|
| Starting point | Noise | Existing image |
| Prompt | Important | Important |
| Input image | No | Yes |
| Control | Prompt-based | Prompt + image |
| Transformation | Full generation | Guided transformation |
| Typical use | New images | Modification / variation |

---

# Inpainting

Inpainting generates or modifies selected portions of an image.

Conceptually:

```text
Original Image
      |
      v
Mask
      |
      v
Masked Region
      |
      v
Diffusion
      |
      v
Completed Image
```

This makes diffusion models useful for image editing as well as generation.

---

# Outpainting

Outpainting extends an existing image beyond its original boundaries.

Conceptually:

```text
+----------------------+
|      Existing        |
|        Image          |
+----------------------+
|                      |
|     Generated        |
|      Extension       |
|                      |
+----------------------+
```

The model generates new content that attempts to remain consistent with the existing image.

---

# Control and Conditioning

Modern diffusion workflows can incorporate additional conditioning mechanisms.

Examples include:

```text
Text
Image
Mask
Pose
Edges
Depth
Reference Image
```

The general architecture becomes:

```text
Conditioning
      |
      +---- Text
      +---- Image
      +---- Control
      +---- Mask
      |
      v
Diffusion Process
```

This is one reason node-based systems such as ComfyUI can become extremely powerful.

---

# Stable Diffusion Experiment Workflow

A practical experiment can follow:

```text
Select Model
      |
      v
Load Checkpoint
      |
      v
Write Prompt
      |
      v
Set Negative Prompt
      |
      v
Choose Seed
      |
      v
Choose Sampler
      |
      v
Set Steps
      |
      v
Set CFG
      |
      v
Set Resolution
      |
      v
Generate
      |
      v
Evaluate
      |
      v
Adjust Parameters
```

This turns image generation into a repeatable experiment.

---

# Reproducibility

A generation should ideally record:

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
Software Version
GPU
```

This allows the result to be recreated or compared later.

A useful experiment record is:

```text
MODEL:
PROMPT:
NEGATIVE:
SEED:
SAMPLER:
STEPS:
CFG:
RESOLUTION:
VAE:
GPU:
```

---

# Common Problems

## Black Image

Possible causes include:

- VAE issue
- model incompatibility
- incorrect workflow
- unsupported precision
- corrupted model
- runtime problem

Check the workflow and model components individually.

---

## CUDA Out of Memory

Possible causes:

```text
Large Resolution
+
Large Model
+
Large Batch
+
Additional Models
```

Try:

```text
Lower Resolution
      OR
Batch Size = 1
      OR
Smaller Model
      OR
Memory Optimization
```

---

## Model Not Appearing

Check:

```text
Model File
     |
     v
Correct Directory
     |
     v
Correct Format
     |
     v
Compatible Architecture
     |
     v
Restart / Refresh
```

---

## Image Quality Is Poor

Potential factors include:

- prompt quality
- model choice
- resolution
- sampler
- sampling steps
- CFG
- seed
- VAE
- model limitations

Avoid changing every parameter simultaneously.

Change one variable at a time.

---

# Scientific Experimentation

Stable Diffusion is useful for learning experimental methodology.

For example:

```text
Experiment A
Prompt + Seed + Sampler A
        |
        v
Result A

Experiment B
Prompt + Seed + Sampler B
        |
        v
Result B
```

Only changing the sampler makes the comparison more meaningful.

This is the same principle used in machine-learning experimentation:

> **Control variables whenever possible.**

---

# Parameter Sensitivity

Different parameters affect different parts of the generation process.

```text
Prompt
  |
  v
Semantic Direction

Seed
  |
  v
Random Initialization

Sampler
  |
  v
Denoising Strategy

Steps
  |
  v
Number of Iterations

CFG
  |
  v
Conditioning Strength

Resolution
  |
  v
Image Size / Compute
```

Understanding these relationships makes troubleshooting much easier.

---

# What I Learned

The biggest lesson from Stable Diffusion is:

> **Image generation is a controlled denoising process operating in a learned latent space.**

The complete simplified pipeline is:

```text
PROMPT
   |
   v
TEXT ENCODER
   |
   v
CONDITIONING
   |
   v
NOISE
   |
   v
DIFFUSION / DENOISING
   |
   v
LATENT
   |
   v
VAE
   |
   v
IMAGE
```

The quality and performance of the result depend on the interaction between:

```text
Model
Prompt
Sampler
Steps
CFG
Seed
Resolution
VAE
Hardware
```

---

# Stable Diffusion Checklist

```text
STABLE DIFFUSION
────────────────────────────

[ ] Model selected
[ ] Model format verified
[ ] Checkpoint loaded
[ ] Text encoder available
[ ] VAE available
[ ] Prompt prepared
[ ] Negative prompt prepared
[ ] Seed selected
[ ] Sampler selected
[ ] Steps selected
[ ] CFG selected
[ ] Resolution selected
[ ] GPU available
[ ] VRAM sufficient
[ ] Result recorded
```

---

# Related Experiments

- [ComfyUI →](/labs/local-ai/comfyui/)
- [DreamShaper →](/labs/local-ai/dreamshaper/)
- [Image Generation →](/labs/local-ai/image-generation/)
- [Model Formats →](/labs/local-ai/model-formats/)
- [NVIDIA GPU →](/labs/local-ai/nvidia-gpu/)
- [CUDA →](/labs/local-ai/cuda/)
- [Docker →](/labs/local-ai/docker/)
- [Local Inference →](/labs/local-ai/local-inference/)

---

# Lab Status

```text
STABLE DIFFUSION
────────────────────────────

Diffusion Concepts     ✓
Latent Space           ✓
ComfyUI Integration    ✓
GPU Inference          ✓
Sampling               ✓
VAE                    ✓
DreamShaper            ✓
Reproducibility        ✓

STATUS: EXPERIMENTAL
```

> **Stable Diffusion turns learned visual patterns into an iterative denoising process.**
>
> **ComfyUI makes that process visible.**
>
> **The GPU makes it practical to run locally.**
