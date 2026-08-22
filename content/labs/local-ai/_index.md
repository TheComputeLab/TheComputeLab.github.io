---
title: "Local AI Lab"
description: "Experiments with local AI, GPU inference, containers, open-source models, image generation, and AI infrastructure."
weight: 40
toc: false
---

> **Run the model. Own the stack. Understand what happens underneath.**

The **Local AI Lab** is where I experiment with running artificial intelligence workloads on local hardware rather than relying entirely on cloud APIs.

This includes local language models, image-generation systems, GPU acceleration, CUDA, Docker containers, model formats, inference runtimes, and the infrastructure required to make everything work together.

---

## What is explored here?

```text
                     LOCAL AI
                         │
          ┌──────────────┼──────────────┐
          │              │              │
       MODELS          RUNTIME        HARDWARE
          │              │              │
      LLMs            LocalAI         NVIDIA GPU
      Diffusion       ComfyUI         CUDA
      DreamShaper     PyTorch         VRAM
          │              │              │
          └──────────────┼──────────────┘
                         │
                       Docker
                         │
                         ▼
                  LOCAL INFERENCE