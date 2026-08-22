---
title: "Image Generation"
description: "A practical guide to local AI image generation with Stable Diffusion, DreamShaper, ComfyUI, CUDA, and NVIDIA GPUs."
weight: 90
toc: true
---

> **Image generation is a pipeline: prompt, model, conditioning, denoising, decoding, and output.**

The Local AI Lab uses image generation as a practical way to connect the software and hardware layers we have explored.

The complete path can be represented as:

```text
Prompt
  |
  v
ComfyUI
  |
  v
Model
  |
  v
Text Conditioning
  |
  v
Diffusion
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

This page documents the practical process of generating images locally and the parameters that control the result.

---

# Lab Objective

The objective is to understand the complete local image-generation workflow:

- text-to-image generation
- image-to-image generation
- inpainting
- outpainting
- prompt construction
- negative prompts
- model selection
- sampling
- CFG
- sampling steps
- seeds
- resolution
- batch size
- VAE
- GPU inference
- VRAM management
- reproducible experiments
- troubleshooting

The goal is not simply:

> **Generate an image.**

The goal is:

> **Understand why the image was generated and which variables controlled the result.**

---

# Local Image Generation Stack

A typical local setup looks like:

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
 |
 v
Image
```

Docker can provide the application environment:

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
   ComfyUI Container
       |
       +-- PyTorch
       +-- CUDA Runtime
       +-- Models
```

---

# Text-to-Image

Text-to-image generation starts with a text description.

Example:

```text
A futuristic AI laboratory with holographic displays,
computers, servers, cinematic lighting, highly detailed
```

The simplified pipeline is:

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
    v
Diffusion Model
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

---

# The Prompt

The prompt is the main natural-language input to the image-generation system.

A prompt can describe:

```text
Subject
Environment
Composition
Lighting
Style
Camera
Materials
Mood
Color
Level of Detail
```

For example:

```text
A modern AI research laboratory,
large GPU servers,
holographic displays,
dramatic cinematic lighting,
wide-angle composition,
highly detailed
```

---

# Prompt Structure

A useful prompt structure is:

```text
[Subject]
+
[Environment]
+
[Composition]
+
[Lighting]
+
[Style]
+
[Quality / Detail]
```

Example:

```text
A scientist working inside a futuristic AI laboratory,
large GPU server racks,
wide-angle composition,
blue atmospheric lighting,
cinematic science-fiction style,
highly detailed
```

This is not a strict formula.

Different models respond differently to prompt structure.

---

# Positive Prompt

The positive prompt describes what should appear in the image.

Example:

```text
futuristic AI laboratory,
GPU servers,
large holographic display,
research environment,
cinematic lighting,
high detail
```

The conditioning information is passed into the diffusion process.

---

# Negative Prompt

The negative prompt describes characteristics that should be discouraged.

Example:

```text
blurry,
low quality,
distorted,
deformed,
watermark,
text,
cropped
```

Negative prompts are model-dependent.

They should be treated as a generation control rather than a guaranteed filtering mechanism.

---

# Prompt Iteration

Prompt engineering is usually iterative.

A practical process is:

```text
Prompt A
   |
   v
Generate
   |
   v
Evaluate
   |
   v
Modify Prompt
   |
   v
Generate Again
```

For example:

```text
A futuristic laboratory
```

can be expanded to:

```text
A futuristic AI research laboratory,
rows of GPU servers,
holographic data visualization,
scientists working at terminals,
cinematic lighting,
wide-angle composition
```

The objective is to add useful information without making the prompt unnecessarily complicated.

---

# Model Selection

The model strongly influences the visual result.

Different models can have different strengths:

```text
Photorealism
Illustration
Anime
Fantasy
Architecture
Portraits
Concept Art
Cinematic Images
```

Examples explored in this lab include:

```text
Stable Diffusion
DreamShaper
```

The prompt should therefore be considered together with the model.

---

# Prompt + Model

The same prompt can produce very different results with different models.

Conceptually:

```text
Prompt
  |
  +---- Model A → Result A
  |
  +---- Model B → Result B
  |
  +---- Model C → Result C
```

This is why model selection is part of prompt experimentation.

---

# ComfyUI Workflow

ComfyUI exposes the image-generation pipeline as a node graph.

A simplified text-to-image workflow is:

```text
Checkpoint Loader
       |
       +--------------------+
       |                    |
       v                    v
     Model                 CLIP
                              |
                   +----------+----------+
                   |                     |
                   v                     v
             Positive Prompt       Negative Prompt
                   |                     |
                   +----------+----------+
                              |
                              v
                           KSampler
                              |
                              v
                         Latent Image
                              |
                              v
                         VAE Decode
                              |
                              v
                            Save
```

This is useful because each stage can be inspected and modified.

---

# Checkpoint

The checkpoint provides the learned model weights and associated components required by the workflow.

Conceptually:

```text
Checkpoint
    |
    +---- Model
    +---- CLIP
    +---- VAE
```

The exact components depend on the model architecture and packaging.

---

# Text Encoding

The text encoder converts the prompt into conditioning information.

```text
Prompt
  |
  v
Text Encoder
  |
  v
Conditioning
```

Both positive and negative conditioning can be passed to the sampling stage.

---

# KSampler

KSampler performs the iterative sampling/denoising process.

It receives information such as:

```text
Model
Positive Conditioning
Negative Conditioning
Latent
Seed
Steps
CFG
Sampler
Scheduler
```

and produces a denoised latent.

```text
Inputs
  |
  v
KSampler
  |
  v
Latent
```

---

# Seed

The seed determines the initial random state.

Conceptually:

```text
Seed
 |
 v
Initial Noise
 |
 v
Diffusion
 |
 v
Image
```

A fixed seed makes controlled comparisons possible.

For example:

```text
Same Model
Same Prompt
Same Steps
Same CFG
Same Resolution
Different Sampler
```

The seed can remain fixed so the sampler becomes the primary variable.

---

# Random Seed vs Fixed Seed

### Random seed

Useful for:

```text
Exploration
Idea Generation
Variation
```

### Fixed seed

Useful for:

```text
Testing
Comparison
Reproducibility
Parameter Tuning
```

A lab environment should use fixed seeds when comparing parameters.

---

# Sampling Steps

Sampling steps determine how many iterations are performed during denoising.

Conceptually:

```text
Few Steps
   |
   v
Faster Generation
```

while:

```text
More Steps
   |
   v
More Computation
```

More steps do not guarantee better output.

The optimal range depends on the model and sampler.

---

# Sampler

The sampler determines how the denoising trajectory is calculated.

Examples include:

```text
Euler
Euler a
DDIM
DPM++
```

Different samplers can affect:

```text
Detail
Sharpness
Composition
Texture
Generation Speed
```

The exact effect depends on the model and workflow.

---

# CFG

CFG stands for Classifier-Free Guidance.

It controls how strongly the image generation follows the conditioning.

Simplified:

```text
Lower CFG
   |
   v
More freedom
```

and:

```text
Higher CFG
   |
   v
Stronger prompt influence
```

Very high CFG values can sometimes produce harsh or unnatural results.

---

# Resolution

Resolution controls the image dimensions.

For example:

```text
512 × 512
```

or:

```text
768 × 768
```

or:

```text
1024 × 1024
```

Higher resolution generally increases computational and memory requirements.

A useful experimental strategy is:

```text
Start Small
    |
    v
Validate Workflow
    |
    v
Increase Resolution
```

---

# Pixel Count

Resolution affects the total number of pixels.

For example:

```text
512 × 512
= 262,144 pixels
```

while:

```text
1024 × 1024
= 1,048,576 pixels
```

The second image contains four times as many pixels.

This helps explain why increasing resolution can have a significant impact on memory and generation time.

---

# Batch Size

Batch size determines how many images are generated together.

```text
Batch = 1
```

means one image.

```text
Batch = 4
```

means multiple images are processed as a batch.

Larger batches can increase VRAM usage substantially.

For initial experiments:

```text
Batch Size = 1
```

is a practical starting point.

---

# VAE

The VAE converts between image space and latent space.

For text-to-image:

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

# Text-to-Image Workflow

The complete process can be represented as:

```text
Prompt
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

---

# Image-to-Image

Image-to-image generation begins with an existing image.

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

The prompt and input image both influence the result.

---

# Denoising Strength

Image-to-image workflows often expose denoising strength.

Conceptually:

```text
Lower Denoising
       |
       v
Preserve More
```

while:

```text
Higher Denoising
       |
       v
Allow More Transformation
```

This makes denoising strength an important experimental parameter.

---

# Inpainting

Inpainting modifies a selected area of an image.

```text
Original Image
      |
      v
Create Mask
      |
      v
Select Region
      |
      v
Diffusion
      |
      v
Generated Region
      |
      v
Final Image
```

Example uses:

```text
Remove Object
Replace Object
Repair Area
Change Face / Clothing
Modify Background
```

---

# Outpainting

Outpainting extends an image beyond its original boundaries.

```text
Existing Image
+----------------------+
|                      |
|      Original        |
|       Content        |
|                      |
+----------------------+
|                      |
|    Generated Area    |
|                      |
+----------------------+
```

The model attempts to create content that remains visually consistent with the original image.

---

# Control Inputs

Image-generation workflows can use more than text.

Possible conditioning inputs include:

```text
Text
Image
Mask
Pose
Depth
Edges
Reference Image
Structure
```

Conceptually:

```text
             Conditioning
                   |
       +-----------+-----------+
       |           |           |
      Text       Image       Control
       |           |           |
       +-----------+-----------+
                   |
                   v
              Diffusion
```

This is one of the strengths of node-based workflows.

---

# GPU Acceleration

Local image generation is computationally expensive.

The GPU stack is:

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
NVIDIA GPU
```

GPU acceleration makes diffusion workloads practical on suitable local hardware.

---

# VRAM

VRAM is often the primary hardware constraint.

Memory can be used by:

```text
Model
+
Latents
+
Activations
+
VAE
+
Additional Models
+
Batch
+
Other GPU Processes
```

Therefore:

```text
Image Generation
      |
      v
VRAM Requirement
```

depends on more than just the model size.

---

# CUDA Out of Memory

A common failure is:

```text
CUDA out of memory
```

This usually means the current workload requires more available GPU memory than the system can provide.

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
Other GPU Applications
```

Possible solutions:

```text
Reduce Resolution
      OR
Reduce Batch
      OR
Use Smaller Model
      OR
Free VRAM
      OR
Use Memory Optimization
```

---

# CPU Fallback

Sometimes the application runs on CPU even though an NVIDIA GPU exists.

Possible symptoms:

```text
GPU Utilization ≈ 0%
VRAM Usage ≈ unchanged
CPU Usage = high
Generation = slow
```

The diagnostic path is:

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
PyTorch
 |
 v
ComfyUI
```

Every layer needs to be checked.

---

# Measuring Generation Performance

Useful metrics include:

```text
Generation Time
Time to First Output
Images per Minute
GPU Utilization
VRAM Usage
CPU Usage
```

A simple experiment can record:

```text
Model:
Resolution:
Steps:
CFG:
Sampler:
Seed:
Generation Time:
VRAM:
GPU:
```

This makes performance comparisons possible.

---

# Prompt Experiment

A controlled experiment might begin with:

```text
Prompt:
A futuristic AI laboratory
```

Then expand it:

```text
A futuristic AI research laboratory,
rows of NVIDIA GPU servers,
large holographic displays,
scientists working at computer terminals,
cinematic lighting,
wide-angle composition,
highly detailed
```

Keep:

```text
Model
Seed
Sampler
Steps
CFG
Resolution
```

constant.

Then compare the outputs.

---

# Sampler Experiment

Keep everything fixed:

```text
Model: DreamShaper
Prompt: Fixed
Seed: Fixed
Steps: Fixed
CFG: Fixed
Resolution: Fixed
```

Change:

```text
Sampler A
```

to:

```text
Sampler B
```

Then compare:

```text
Composition
Detail
Texture
Artifacts
Speed
```

This is a better experiment than changing several variables at once.

---

# Resolution Experiment

Start with:

```text
512 × 512
```

Then:

```text
768 × 768
```

Then, if the hardware allows:

```text
1024 × 1024
```

Record:

```text
Generation Time
VRAM Usage
GPU Utilization
Image Quality
```

This demonstrates the relationship between resolution and compute requirements.

---

# Seed Experiment

Use the same:

```text
Model
Prompt
Sampler
Steps
CFG
Resolution
```

and compare:

```text
Seed A
Seed B
Seed C
```

This demonstrates the effect of different initial noise conditions.

---

# Model Comparison

Another useful experiment is:

```text
Same Prompt
Same Seed
Same Resolution
Same Sampler
Same Steps
Same CFG
```

Compare:

```text
Stable Diffusion Model
        vs
DreamShaper
```

This demonstrates how model training influences the generated result.

---

# Reproducibility

A reproducible image-generation record should contain:

```text
MODEL:
MODEL VERSION:
PROMPT:
NEGATIVE PROMPT:
SEED:
SAMPLER:
SCHEDULER:
STEPS:
CFG:
RESOLUTION:
BATCH SIZE:
VAE:
GPU:
CUDA:
COMFYUI VERSION:
```

Without these details, reproducing an image can be difficult.

---

# Image Generation Experiment Template

```text
Experiment ID:
Date:

MODEL:
MODEL VERSION:

PROMPT:

NEGATIVE PROMPT:

SEED:

SAMPLER:
SCHEDULER:
STEPS:
CFG:

RESOLUTION:
BATCH:

VAE:

GPU:
VRAM:
CUDA:

GENERATION TIME:

OBSERVATIONS:

RESULT:
```

This converts an image-generation session into a documented experiment.

---

# Common Problems

## Model not found

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
ComfyUI Refresh
```

---

## Black or corrupted output

Possible causes:

- incompatible model
- VAE issue
- precision issue
- workflow problem
- corrupted model
- runtime problem

Inspect the workflow and logs.

---

## Poor image quality

Possible causes:

```text
Prompt
Model
Sampler
Steps
CFG
Resolution
VAE
```

Change one variable at a time.

---

## Generation too slow

Check:

```text
CPU vs GPU
GPU Utilization
VRAM
Model Size
Resolution
Steps
Other GPU Processes
```

The first question should be:

> **Is the GPU actually being used?**

---

# Image Generation Best Practices

A practical local workflow is:

```text
1. Choose Model
2. Start at Small Resolution
3. Use Batch = 1
4. Use a Fixed Seed
5. Generate Baseline
6. Change One Parameter
7. Compare
8. Record Results
9. Increase Resolution
10. Save Workflow
```

This minimizes confusion and makes results reproducible.

---

# Workflow Preservation

ComfyUI workflows should be saved along with the generated image.

A useful project structure is:

```text
image-generation/
│
├── models/
│
├── workflows/
│
├── outputs/
│
└── experiments/
```

For each experiment:

```text
experiment-001/
├── workflow.json
├── prompt.txt
├── settings.txt
└── output.png
```

This makes the experiment easier to reproduce.

---

# Model + Workflow + Hardware

A generated image is the result of several interacting variables:

```text
              MODEL
                |
                v
            WORKFLOW
                |
                v
             SETTINGS
                |
                v
             HARDWARE
                |
                v
              IMAGE
```

Changing any one of these can change the result.

---

# What I Learned

The biggest lesson from local image generation is:

> **The generated image is the output of an entire computational pipeline.**

It is influenced by:

```text
Model
Prompt
Conditioning
Seed
Sampler
Steps
CFG
Resolution
VAE
Hardware
```

The same prompt can produce different results because the underlying configuration changes.

This makes image generation an excellent environment for experimentation.

---

# Image Generation Checklist

```text
IMAGE GENERATION
────────────────────────────

[ ] Model selected
[ ] Model architecture verified
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
[ ] Batch size selected
[ ] GPU detected
[ ] CUDA available
[ ] VRAM sufficient
[ ] Workflow saved
[ ] Output saved
[ ] Experiment recorded
```

---

# Related Experiments

- [ComfyUI →](/labs/local-ai/comfyui/)
- [Stable Diffusion →](/labs/local-ai/stable-diffusion/)
- [DreamShaper →](/labs/local-ai/dreamshaper/)
- [Model Formats →](/labs/local-ai/model-formats/)
- [NVIDIA GPU →](/labs/local-ai/nvidia-gpu/)
- [CUDA →](/labs/local-ai/cuda/)
- [Docker →](/labs/local-ai/docker/)
- [Local Inference →](/labs/local-ai/local-inference/)

---

# Lab Status

```text
IMAGE GENERATION
────────────────────────────

Text-to-Image         ✓
Image-to-Image        ✓
Inpainting            ✓
Outpainting            ✓
Prompt Experiments    ✓
Sampling              ✓
GPU Inference         ✓
VRAM Testing          ✓
Reproducibility       ✓

STATUS: EXPERIMENTAL
```

> **The goal is not only to generate images.**
>
> **The goal is to understand the pipeline well enough to control, reproduce, and troubleshoot the generation process.**
