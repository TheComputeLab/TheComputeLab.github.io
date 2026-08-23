---
title: "Local AI Lab"
description: "Experiments with local AI, GPU inference, containers, open-source models, image generation, and AI infrastructure."
weight: 40
toc: false
---

<div class="local-ai-lab">

<div class="local-ai-hero">

<div class="local-ai-status">
<span></span> LOCAL COMPUTE ENVIRONMENT
</div>

<p>
Experiments with local models, GPU inference, containers,
image generation and the infrastructure underneath AI systems.
</p>

<div class="local-ai-terminal">
<span>$</span> local_ai.initialize()
</div>

</div>


## The Local AI Stack

<div class="local-ai-stack">

<div class="ai-stack-layer">
<strong>AI APPLICATIONS</strong>
<span>LocalAI · ComfyUI · AI Apps</span>
</div>

<div class="ai-stack-arrow">↓</div>

<div class="ai-stack-layer">
<strong>MODELS</strong>
<span>LLMs · Diffusion · DreamShaper</span>
</div>

<div class="ai-stack-arrow">↓</div>

<div class="ai-stack-layer">
<strong>INFERENCE</strong>
<span>PyTorch · Runtime · APIs</span>
</div>

<div class="ai-stack-arrow">↓</div>

<div class="ai-stack-layer">
<strong>CUDA</strong>
<span>GPU Compute</span>
</div>

<div class="ai-stack-arrow">↓</div>

<div class="ai-stack-layer">
<strong>CONTAINER</strong>
<span>Docker · NVIDIA Container Toolkit</span>
</div>

<div class="ai-stack-arrow">↓</div>

<div class="ai-stack-layer">
<strong>HARDWARE</strong>
<span>NVIDIA GPU · VRAM · Host System</span>
</div>

</div>


## Experiments

<div class="local-ai-grid">


<a class="local-ai-card" href="/labs/local-ai/localai/">

<div class="local-ai-card-number">01</div>

<div class="local-ai-card-icon">🤖</div>

<h3>LocalAI</h3>

<p>
Running AI models locally through an API-compatible
local inference environment.
</p>

<div class="local-ai-tags">
<span>API</span>
<span>GPU</span>
<span>Docker</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>


<a class="local-ai-card" href="/labs/local-ai/comfyui/">

<div class="local-ai-card-number">02</div>

<div class="local-ai-card-icon">🎨</div>

<h3>ComfyUI</h3>

<p>
Exploring node-based workflows for local image
generation and model experimentation.
</p>

<div class="local-ai-tags">
<span>Nodes</span>
<span>Diffusion</span>
<span>GPU</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>


<a class="local-ai-card" href="/labs/local-ai/docker/">

<div class="local-ai-card-number">03</div>

<div class="local-ai-card-icon">🐳</div>

<h3>Docker</h3>

<p>
Containerizing AI applications and creating
reproducible local inference environments.
</p>

<div class="local-ai-tags">
<span>Containers</span>
<span>Volumes</span>
<span>Runtime</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>


<a class="local-ai-card" href="/labs/local-ai/nvidia-gpu/">

<div class="local-ai-card-number">04</div>

<div class="local-ai-card-icon">⚡</div>

<h3>NVIDIA GPU</h3>

<p>
Understanding GPU visibility, VRAM, utilization
and accelerated AI workloads.
</p>

<div class="local-ai-tags">
<span>NVIDIA</span>
<span>VRAM</span>
<span>GPU</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>


<a class="local-ai-card" href="/labs/local-ai/cuda/">

<div class="local-ai-card-number">05</div>

<div class="local-ai-card-icon">▣</div>

<h3>CUDA</h3>

<p>
Understanding the relationship between NVIDIA
drivers, CUDA and AI frameworks.
</p>

<div class="local-ai-tags">
<span>CUDA</span>
<span>PyTorch</span>
<span>Compute</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>


<a class="local-ai-card" href="/labs/local-ai/local-llms/">

<div class="local-ai-card-number">06</div>

<div class="local-ai-card-icon">🧠</div>

<h3>Local LLMs</h3>

<p>
Exploring model size, quantization, VRAM,
context windows and local inference.
</p>

<div class="local-ai-tags">
<span>LLM</span>
<span>Quantization</span>
<span>VRAM</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>


<a class="local-ai-card" href="/labs/local-ai/stable-diffusion/">

<div class="local-ai-card-number">07</div>

<div class="local-ai-card-icon">🌌</div>

<h3>Stable Diffusion</h3>

<p>
Understanding diffusion-based image generation
and local model workflows.
</p>

<div class="local-ai-tags">
<span>Diffusion</span>
<span>Sampling</span>
<span>Images</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>


<a class="local-ai-card" href="/labs/local-ai/dreamshaper/">

<div class="local-ai-card-number">08</div>

<div class="local-ai-card-icon">🧪</div>

<h3>DreamShaper</h3>

<p>
A practical experiment with local image-generation
models and GPU inference.
</p>

<div class="local-ai-tags">
<span>Model</span>
<span>Checkpoint</span>
<span>GPU</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>


<a class="local-ai-card" href="/labs/local-ai/imagegeneration/">

<div class="local-ai-card-number">09</div>

<div class="local-ai-card-icon">🖼️</div>

<h3>Image Generation</h3>

<p>
Exploring prompts, sampling, resolution, VAE,
CFG and local generation pipelines.
</p>

<div class="local-ai-tags">
<span>Text-to-Image</span>
<span>VAE</span>
<span>Sampling</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>


<a class="local-ai-card" href="/labs/local-ai/modelformats/">

<div class="local-ai-card-number">10</div>

<div class="local-ai-card-icon">📦</div>

<h3>Model Formats</h3>

<p>
Understanding model files, formats, compatibility
and how runtimes load them.
</p>

<div class="local-ai-tags">
<span>Safetensors</span>
<span>GGUF</span>
<span>Diffusers</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>


<a class="local-ai-card" href="/labs/local-ai/localinference/">

<div class="local-ai-card-number">11</div>

<div class="local-ai-card-icon">🚀</div>

<h3>Local Inference</h3>

<p>
Understanding the complete path from model loading
to GPU-accelerated inference.
</p>

<div class="local-ai-tags">
<span>Inference</span>
<span>Latency</span>
<span>VRAM</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>


<a class="local-ai-card" href="/labs/local-ai/gpu-troubleshooting/">

<div class="local-ai-card-number">12</div>

<div class="local-ai-card-icon">🔧</div>

<h3>GPU Troubleshooting</h3>

<p>
Real-world troubleshooting of CUDA, Docker,
GPU visibility and containerized AI workloads.
</p>

<div class="local-ai-tags">
<span>CUDA</span>
<span>Docker</span>
<span>Debugging</span>
</div>

<div class="local-ai-link">
Explore →
</div>

</a>

</div>


## Lab Philosophy

<div class="local-ai-philosophy">

> **The interesting part isn't just getting the model to run.**

> It's understanding everything that makes it run.

</div>

<div class="local-ai-flow">

<span>MODEL</span>
<span>→</span>
<span>RUNTIME</span>
<span>→</span>
<span>CUDA</span>
<span>→</span>
<span>GPU</span>
<span>→</span>
<span>INFERENCE</span>

</div>

</div>


