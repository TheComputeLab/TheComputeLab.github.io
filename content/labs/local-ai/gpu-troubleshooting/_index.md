---
title: "GPU Container Troubleshooting"
description: "A practical troubleshooting guide for NVIDIA GPUs, Docker, CUDA, WSL2, LocalAI containers, VRAM, and GPU inference."
weight: 120
toc: true
---

> **When local AI fails, troubleshoot the stack from the hardware upward.**

Running AI workloads inside Docker adds several layers between the application and the physical GPU.

A typical stack is:

```text
AI Application
      |
      v
AI Runtime
      |
      v
Docker Container
      |
      v
NVIDIA Container Toolkit
      |
      v
CUDA Runtime
      |
      v
NVIDIA Driver
      |
      v
NVIDIA GPU
```

If any layer is broken, GPU inference can fail.

This page provides a practical troubleshooting workflow for local GPU containers.

---

# Lab Objective

The goal is to diagnose problems involving:

- NVIDIA GPU detection
- Docker GPU access
- NVIDIA Container Toolkit
- CUDA
- WSL2
- LocalAI GPU containers
- PyTorch CUDA detection
- VRAM
- CUDA out-of-memory
- CPU fallback
- model loading
- container configuration
- driver/runtime compatibility

The main principle is:

> **Do not troubleshoot the AI application first. Verify the infrastructure layer by layer.**

---

# The GPU Container Stack

A typical Windows + WSL2 + Docker environment can look like:

```text
Windows
   |
   v
NVIDIA Driver
   |
   v
WSL2
   |
   v
Linux Environment
   |
   v
Docker
   |
   v
NVIDIA Container Runtime
   |
   v
CUDA
   |
   v
AI Container
   |
   v
AI Application
```

For example:

```text
Windows
   |
   +-- NVIDIA Driver
          |
          v
        WSL2
          |
          v
        Docker
          |
          v
   LocalAI GPU Container
          |
          v
        CUDA
          |
          v
      NVIDIA GPU
```

---

# Troubleshooting Philosophy

Use a bottom-up approach:

```text
1. Hardware
      |
      v
2. NVIDIA Driver
      |
      v
3. WSL2
      |
      v
4. Docker
      |
      v
5. NVIDIA Container Toolkit
      |
      v
6. CUDA
      |
      v
7. Framework
      |
      v
8. AI Runtime
      |
      v
9. Model
      |
      v
10. Application
```

If a lower layer is broken, higher-level troubleshooting is usually premature.

---

# Step 1: Check the GPU on Windows

From Windows PowerShell:

```powershell
nvidia-smi
```

A working system should display information such as:

```text
NVIDIA-SMI
Driver Version
CUDA Version
GPU Name
Memory Usage
GPU Utilization
```

The exact output depends on the installed driver and GPU.

---

# What `nvidia-smi` Tells You

`nvidia-smi` is useful for checking:

```text
GPU Detection
Driver
VRAM
GPU Utilization
Running GPU Processes
```

For example:

```text
GPU
 |
 +-- Name
 +-- Memory
 +-- Utilization
 +-- Driver
```

If `nvidia-smi` fails on the host, do not start by debugging Docker.

Fix the host GPU environment first.

---

# Step 2: Check WSL2

From PowerShell:

```powershell
wsl --status
```

and:

```powershell
wsl -l -v
```

You want the Linux distribution to be running under:

```text
VERSION 2
```

Example:

```text
NAME      STATE     VERSION
Ubuntu    Running   2
```

---

# Enter WSL2

Start the Linux environment:

```powershell
wsl
```

Then check:

```bash
uname -a
```

and:

```bash
nvidia-smi
```

If the GPU is available inside WSL2, the Windows driver is generally being exposed correctly to the Linux environment.

---

# WSL2 GPU Path

The important relationship is:

```text
Windows NVIDIA Driver
        |
        v
       WSL2
        |
        v
   Linux Environment
        |
        v
      Docker
        |
        v
   GPU Container
```

A GPU working in Windows does not automatically prove that the container can use it.

---

# Step 3: Check Docker

From PowerShell:

```powershell
docker version
```

and:

```powershell
docker info
```

Check that Docker Desktop or the Docker engine is running correctly.

---

# Check Running Containers

Use:

```powershell
docker ps
```

For all containers:

```powershell
docker ps -a
```

Useful information includes:

```text
Container Name
Image
Status
Ports
```

---

# Check Container Logs

For a LocalAI container:

```powershell
docker logs local-ai
```

If the container has a different name:

```powershell
docker ps -a
```

find the correct name first.

Follow logs live:

```powershell
docker logs -f local-ai
```

Logs are often the fastest way to identify:

- model loading problems
- CUDA errors
- missing libraries
- permission issues
- configuration errors
- runtime crashes

---

# Step 4: Test Docker GPU Access

The most important infrastructure test is whether a container can access the GPU.

A typical NVIDIA container test is:

```powershell
docker run --rm --gpus all nvidia/cuda:12.4.1-base-ubuntu22.04 nvidia-smi
```

The exact CUDA image version can be adjusted for the environment.

The important part is:

```text
--gpus all
```

This requests GPU access for the container.

---

# What a Successful Test Means

If the container prints:

```text
NVIDIA-SMI
GPU Name
Driver Version
CUDA Version
```

then:

```text
Docker
   |
   v
NVIDIA Runtime
   |
   v
GPU
```

is working.

This is a major diagnostic checkpoint.

---

# If Docker Cannot See the GPU

If the test fails:

```text
docker run --gpus all ...
```

do not immediately debug LocalAI.

The problem is probably below the AI application layer.

Check:

```text
Host GPU
    |
    v
WSL2
    |
    v
Docker
    |
    v
NVIDIA Container Toolkit
```

---

# `--gpus all`

The option:

```bash
--gpus all
```

requests access to all available GPUs.

For a system with one NVIDIA GPU:

```text
--gpus all
```

normally means the container can access that GPU.

For systems with multiple GPUs, device selection can be configured more specifically.

---

# Step 5: Check NVIDIA Container Toolkit

Docker needs the appropriate NVIDIA container integration to expose the GPU to containers.

The conceptual relationship is:

```text
Docker
   |
   v
NVIDIA Container Toolkit
   |
   v
CUDA Container
   |
   v
GPU
```

If ordinary Docker works but GPU-enabled containers fail, the NVIDIA container integration is an important place to investigate.

---

# Container Toolkit Diagnostic

Inside the Linux/WSL2 environment, inspect the NVIDIA container tooling according to the installed distribution and Docker setup.

A common diagnostic command is:

```bash
nvidia-ctk --version
```

If the command is unavailable, the toolkit may not be installed in that environment.

---

# Step 6: Check CUDA

CUDA exists at several conceptual layers.

```text
NVIDIA Driver
      |
      v
CUDA Support
      |
      v
Container CUDA Runtime
      |
      v
Framework
```

Do not confuse:

```text
Driver-supported CUDA version
```

with:

```text
CUDA toolkit/runtime installed inside a container
```

They are related but not identical.

---

# CUDA Version Checks

Host:

```powershell
nvidia-smi
```

Container:

```bash
nvidia-smi
```

Framework:

```python
import torch

print(torch.version.cuda)
print(torch.cuda.is_available())
```

These provide different pieces of information.

---

# Step 7: Check PyTorch

If the container uses PyTorch:

```python
import torch

print("PyTorch:", torch.__version__)
print("CUDA available:", torch.cuda.is_available())
print("CUDA version:", torch.version.cuda)

if torch.cuda.is_available():
    print("GPU:", torch.cuda.get_device_name(0))
```

Expected:

```text
CUDA available: True
```

If it returns:

```text
False
```

the application may fall back to CPU.

---

# Test GPU from Inside the Container

Enter the container:

```powershell
docker exec -it local-ai bash
```

If the image uses another shell:

```powershell
docker exec -it local-ai sh
```

Then test:

```bash
nvidia-smi
```

If available:

```bash
python
```

and test PyTorch.

---

# GPU Visibility Matrix

Use this simple matrix:

| Test | Result | Meaning |
|---|---|---|
| Windows `nvidia-smi` | ✓ | Host GPU works |
| WSL2 `nvidia-smi` | ✓ | WSL GPU access works |
| Docker CUDA test | ✓ | Container GPU access works |
| Container `nvidia-smi` | ✓ | GPU visible inside container |
| PyTorch CUDA | ✓ | Framework can use GPU |
| AI Runtime GPU | ✓ | Application can use GPU |

Troubleshoot the first failing layer.

---

# LocalAI GPU Container

A typical LocalAI GPU deployment may look like:

```text
Docker
  |
  v
LocalAI GPU Image
  |
  v
CUDA Runtime
  |
  v
NVIDIA GPU
```

The exact image tag and CUDA version depend on the deployment.

The important requirement is that the container has GPU access.

---

# Example Container Check

First identify the container:

```powershell
docker ps
```

Then inspect it:

```powershell
docker inspect local-ai
```

Look for GPU/device configuration and runtime-related settings.

---

# Container Restart

After configuration changes:

```powershell
docker restart local-ai
```

Then:

```powershell
docker logs -f local-ai
```

Restarting is often required after changing container environment variables or GPU configuration.

---

# CUDA Out of Memory

One of the most common AI errors is:

```text
CUDA out of memory
```

This means the current operation requires more available GPU memory than is currently available.

Possible causes:

```text
Large Model
+
Large Context
+
Large Resolution
+
Large Batch
+
Multiple Models
+
Other GPU Processes
```

---

# Check VRAM

Use:

```powershell
nvidia-smi
```

Look at:

```text
Memory-Usage
```

For example:

```text
VRAM:
10 GB / 12 GB
```

during inference indicates that most of the available memory is being used.

---

# Free GPU Memory

Check running processes:

```powershell
nvidia-smi
```

If another application is consuming VRAM:

```text
Browser
Game
ComfyUI
Another AI Container
Python Process
```

close it if appropriate.

Then run:

```powershell
nvidia-smi
```

again.

---

# Reduce Model Memory

If the model itself is too large:

```text
Use Smaller Model
       OR
Use Quantized Model
       OR
Use Lower Precision
       OR
Use Offloading
```

For LLMs, quantization can significantly reduce memory requirements.

For image generation, reducing resolution and batch size is often an effective first step.

---

# Reduce Context

For LLM inference, long context can increase memory consumption.

If you have:

```text
Very Long Prompt
+
Large Context Window
+
Large Model
```

VRAM usage can increase substantially.

Test with a shorter context first.

---

# Reduce Image Resolution

For diffusion workloads:

```text
1024 × 1024
```

can require substantially more memory than:

```text
512 × 512
```

Start with a smaller resolution to verify that the pipeline works.

---

# Reduce Batch Size

For image generation:

```text
Batch = 1
```

is a good troubleshooting baseline.

For LLM inference, reduce concurrency or batch-related settings when memory is constrained.

---

# CPU Fallback

A common failure pattern is:

```text
GPU exists
      |
      v
Container starts
      |
      v
AI application runs
      |
      v
CPU performs inference
```

Symptoms include:

```text
GPU Utilization ≈ 0%
CPU Utilization = High
Generation = Slow
```

---

# Diagnose CPU Fallback

Check:

```powershell
nvidia-smi
```

while inference is running.

Then check inside the application/framework.

For PyTorch:

```python
import torch

print(torch.cuda.is_available())
```

If:

```text
False
```

the framework is not using CUDA.

---

# Driver vs CUDA Mismatch

One common source of confusion is:

```text
Driver Version
```

versus:

```text
CUDA Runtime Version
```

A container may contain a CUDA runtime that differs from the CUDA version shown by `nvidia-smi`.

Compatibility depends on the NVIDIA driver and CUDA runtime combination.

Therefore, do not assume:

```text
nvidia-smi CUDA Version
=
container CUDA toolkit version
```

They represent different things.

---

# Container Image Compatibility

An AI container may be built for a specific CUDA environment.

Conceptually:

```text
Container Image
      |
      v
CUDA Runtime
      |
      v
NVIDIA Driver
      |
      v
GPU
```

If the image expects functionality unsupported by the host driver, GPU execution can fail.

When changing container images, check the image's documented CUDA requirements.

---

# Inspect Container Environment

Useful commands:

```powershell
docker inspect local-ai
```

and:

```powershell
docker exec -it local-ai env
```

Inside the container, useful checks can include:

```bash
nvidia-smi
```

and, where applicable:

```bash
python
```

---

# Check Container Mounts

Models often live outside the container.

Inspect mounts:

```powershell
docker inspect local-ai
```

Look for:

```text
Mounts
```

A useful arrangement is:

```text
Host Models
     |
     v
Docker Volume / Bind Mount
     |
     v
Container
     |
     v
AI Runtime
```

---

# Model Not Found

If the container starts but the model is missing:

```text
1. Check Host File
       |
       v
2. Check Mount
       |
       v
3. Check Container Path
       |
       v
4. Check Runtime Model Directory
       |
       v
5. Check Model Format
```

Do not assume the host path and container path are identical.

---

# Windows Path vs WSL Path

In WSL2:

```text
C:\Projects\AI
```

may appear as:

```text
/mnt/c/Projects/AI
```

Similarly:

```text
G:\PROJECTS
```

is generally accessed through:

```text
/mnt/g/PROJECTS
```

Path translation problems can prevent Docker or applications from finding models.

---

# WSL Path Troubleshooting

Check:

```bash
ls /mnt/c
```

and:

```bash
ls /mnt/g
```

Then verify the actual project/model path.

Avoid assuming that a Windows path can be pasted directly into Linux commands.

---

# Docker Volume Example

A conceptual mapping is:

```text
Windows Host
G:\AI\Models
      |
      v
Docker Mount
      |
      v
/app/models
```

The application inside the container sees:

```text
/app/models
```

not necessarily:

```text
G:\AI\Models
```

---

# Container Cannot Start

If the container exits immediately:

```powershell
docker ps -a
```

Then:

```powershell
docker logs local-ai
```

Look for:

```text
CUDA errors
Missing libraries
Invalid arguments
Permission errors
Missing model
Configuration errors
Port conflicts
```

---

# Port Conflicts

If a container cannot start because a port is already in use:

```text
Port
 |
 +-- Existing Process
 |
 +-- Existing Container
```

Check:

```powershell
docker ps
```

and, on Windows:

```powershell
netstat -ano | findstr :8080
```

Replace `8080` with the port being used.

---

# Container Is Running but API Does Not Work

Check:

```powershell
docker ps
```

Then:

```powershell
docker logs local-ai
```

Verify the port mapping.

For example:

```text
0.0.0.0:8080 -> 8080
```

means the host port is mapped to the container port.

---

# Test Local API

If the application provides an HTTP endpoint, test it locally.

For example:

```powershell
curl http://localhost:8080/
```

The exact endpoint depends on the application.

If the container is running but the endpoint fails:

```text
Docker
  |
  v
Port Mapping
  |
  v
Application
```

needs to be checked.

---

# GPU Troubleshooting Decision Tree

Use this sequence:

```text
GPU Problem?
     |
     v
Does Windows nvidia-smi work?
     |
   No +----> Fix NVIDIA Driver / Host
     |
    Yes
     |
     v
Does WSL2 nvidia-smi work?
     |
   No +----> Fix WSL2 GPU Access
     |
    Yes
     |
     v
Does Docker CUDA test work?
     |
   No +----> Fix NVIDIA Container Integration
     |
    Yes
     |
     v
Does container nvidia-smi work?
     |
   No +----> Check Container GPU Config
     |
    Yes
     |
     v
Does PyTorch detect CUDA?
     |
   No +----> Check Framework / CUDA
     |
    Yes
     |
     v
Does AI Runtime use GPU?
     |
   No +----> Check Runtime Configuration
     |
    Yes
     |
     v
Check Model / VRAM / Workflow
```

---

# Golden Diagnostic Sequence

When something fails, run these checks in order:

```text
1. nvidia-smi
2. wsl --status
3. wsl -l -v
4. WSL nvidia-smi
5. docker version
6. docker ps
7. Docker CUDA test
8. Container nvidia-smi
9. PyTorch CUDA test
10. AI runtime logs
11. Model loading
12. Inference
```

This prevents random troubleshooting.

---

# Minimal Diagnostic Commands

### Windows

```powershell
nvidia-smi
wsl --status
wsl -l -v
docker version
docker ps
```

### WSL2

```bash
nvidia-smi
```

### Docker

```powershell
docker ps
docker ps -a
docker logs <container>
docker inspect <container>
```

### GPU Container

```powershell
docker run --rm --gpus all nvidia/cuda:12.4.1-base-ubuntu22.04 nvidia-smi
```

### PyTorch

```python
import torch

print(torch.cuda.is_available())

if torch.cuda.is_available():
    print(torch.cuda.get_device_name(0))
```

---

# What I Learned

The biggest lesson from GPU container troubleshooting is:

> **A GPU problem is usually a stack problem, not an AI-model problem.**

The complete chain is:

```text
Hardware
   |
   v
Driver
   |
   v
WSL2
   |
   v
Docker
   |
   v
NVIDIA Container Toolkit
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
Model
   |
   v
Inference
```

Troubleshoot from the bottom upward.

---

# GPU Container Troubleshooting Checklist

```text
GPU CONTAINER
────────────────────────────

[ ] Windows nvidia-smi works
[ ] WSL2 enabled
[ ] WSL2 GPU works
[ ] Docker running
[ ] NVIDIA Container Toolkit available
[ ] Docker GPU test works
[ ] Container sees GPU
[ ] CUDA available
[ ] PyTorch sees GPU
[ ] AI runtime sees GPU
[ ] Model accessible
[ ] Model compatible
[ ] VRAM sufficient
[ ] Container logs checked
[ ] Port mapping verified
[ ] Inference tested
```

---

# Related Experiments

- [LocalAI →](/labs/local-ai/localai/)
- [Docker →](/labs/local-ai/docker/)
- [NVIDIA GPU →](/labs/local-ai/nvidia-gpu/)
- [CUDA →](/labs/local-ai/cuda/)
- [Local Inference →](/labs/local-ai/localinference/)
- [Model Formats →](/labs/local-ai/modelformats/)
- [Local LLMs →](/labs/local-ai/local-llms/)
- [ComfyUI →](/labs/local-ai/comfyui/)

---

# Lab Status

```text
GPU CONTAINER TROUBLESHOOTING
────────────────────────────

Host GPU Detection       ✓
WSL2 GPU Detection       ✓
Docker GPU Access        ✓
CUDA Diagnostics         ✓
PyTorch Diagnostics      ✓
VRAM Troubleshooting     ✓
CPU Fallback             ✓
Container Diagnostics    ✓
Model Mounts             ✓
Port Troubleshooting     ✓
Decision Tree            ✓

STATUS: EXPERIMENTAL
```

> **When debugging local AI, start with the GPU and work upward.**
>
> **If `nvidia-smi` works on the host, inside WSL2, and inside the container, most of the infrastructure path has already been validated.**
