---
title: "Labs"
description: "Hands-on experiments in artificial intelligence, computer vision, local inference, GPU computing, and intelligent systems."
weight: 40
toc: false
---

<style>

/* =========================================================
   MACHINE LEARNING LAB
   ========================================================= */

.lab-ml .ml-visual {
position: relative;
overflow: hidden;
background:
linear-gradient(rgba(96,165,250,.07) 1px, transparent 1px),
linear-gradient(90deg, rgba(96,165,250,.07) 1px, transparent 1px);
background-size: 24px 24px;
}

.ml-network {
position: absolute;
left: 50%;
top: 48%;
width: 230px;
height: 125px;
transform: translate(-50%, -50%);
}

.ml-node {
position: absolute;
width: 9px;
height: 9px;
border: 1px solid rgba(96,165,250,.9);
border-radius: 50%;
background: #07111d;
box-shadow: 0 0 8px rgba(96,165,250,.45);
animation: mlNodePulse 2.4s ease-in-out infinite;
}

.ml-node-one {
left: 10px;
top: 22px;
}

.ml-node-two {
left: 10px;
top: 62px;
animation-delay: .3s;
}

.ml-node-three {
left: 10px;
top: 102px;
animation-delay: .6s;
}

.ml-node-four {
right: 42px;
top: 42px;
animation-delay: .9s;
}

.ml-node-five {
right: 42px;
top: 82px;
animation-delay: 1.2s;
}

.ml-line {
position: absolute;
height: 1px;
background: rgba(96,165,250,.35);
transform-origin: left center;
animation: mlLinePulse 2.2s ease-in-out infinite;
}

.ml-line-one {
left: 19px;
top: 27px;
width: 155px;
transform: rotate(7deg);
}

.ml-line-two {
left: 19px;
top: 67px;
width: 155px;
}

.ml-line-three {
left: 19px;
top: 107px;
width: 155px;
transform: rotate(-7deg);
animation-delay: .4s;
}

.ml-line-four {
left: 165px;
top: 47px;
width: 30px;
transform: rotate(28deg);
animation-delay: .8s;
}

.ml-prediction {
position: absolute;
right: 0;
top: 50px;
display: flex;
align-items: center;
justify-content: center;
width: 32px;
height: 32px;
border: 1px solid rgba(96,165,250,.7);
border-radius: 5px;
color: #93c5fd;
background: rgba(96,165,250,.08);
font: 700 16px monospace;
box-shadow: 0 0 14px rgba(96,165,250,.15);
animation: mlPrediction 2.8s ease-in-out infinite;
}

.ml-data-label {
position: absolute;
left: 50%;
bottom: 18px;
transform: translateX(-50%);
color: #64748b;
font: 8px monospace;
letter-spacing: .4px;
white-space: nowrap;
}

@keyframes mlNodePulse {
0%,100% {
opacity: .4;
transform: scale(.8);
}
50% {
opacity: 1;
transform: scale(1.2);
box-shadow: 0 0 12px rgba(96,165,250,.7);
}
}

@keyframes mlLinePulse {
0%,100% {
opacity: .25;
}
50% {
opacity: .9;
box-shadow: 0 0 7px rgba(96,165,250,.25);
}
}

@keyframes mlPrediction {
0%,100% {
opacity: .55;
transform: scale(.92);
}
50% {
opacity: 1;
transform: scale(1.05);
box-shadow: 0 0 18px rgba(96,165,250,.3);
}
}

</style>

<section class="labs-page">

<!-- HERO -->
<div class="labs-hero">
<div class="labs-status">
<span class="labs-status-dot"></span>
EXPERIMENTAL ENVIRONMENT
</div>
<h1 class="labs-title">
The <span>Compute Lab</span>
</h1>
<p class="labs-subtitle">
Experiments, prototypes, benchmarks and engineering notes
across artificial intelligence, computer vision, edge computing,
local AI and infrastructure.
</p>
<div class="labs-terminal">
<span class="terminal-prompt">$</span>
<span class="terminal-command">initialize_labs()</span>
<span class="terminal-cursor"></span>
</div>
</div>

<!-- LAB GRID -->
<div class="labs-grid">

<!-- ================= AI LAB ================= -->

<a class="lab-card lab-ai" href="/labs/ai-lab/">

<div class="lab-card-top">
<span class="lab-number">01</span>
<span class="lab-tag">GENAI</span>
</div>

<div class="lab-visual neural-visual">

<div class="neural-network">

<span class="node n1"></span>
<span class="node n2"></span>
<span class="node n3"></span>
<span class="node n4"></span>
<span class="node n5"></span>
<span class="node n6"></span>
<span class="node n7"></span>
<span class="node n8"></span>
<span class="node n9"></span>

<span class="connection c1"></span>
<span class="connection c2"></span>
<span class="connection c3"></span>
<span class="connection c4"></span>
<span class="connection c5"></span>
<span class="connection c6"></span>

<span class="signal s1"></span>
<span class="signal s2"></span>
<span class="signal s3"></span>

</div>

<div class="visual-label">
MODEL → AGENT → TOOL
</div>

</div>

<div class="lab-content">

<div class="lab-icon">🧠</div>

<h2>AI Lab</h2>

<p>
Exploring LLMs, Generative AI, agents, RAG,
speech AI and intelligent application architectures.
</p>

<div class="lab-topics">
<span>LLMs</span>
<span>Agents</span>
<span>RAG</span>
<span>ADK</span>
</div>

<div class="lab-explore">
Explore AI Lab
<span>→</span>
</div>

</div>

</a>

<!-- ================= COMPUTER VISION ================= -->

<a class="lab-card lab-cv" href="/labs/computer-vision/">

<div class="lab-card-top">
<span class="lab-number">02</span>
<span class="lab-tag">VISION</span>
</div>

<div class="lab-visual vision-visual">

<div class="vision-frame">

<div class="vision-object object-one">
<span>OBJECT</span>
</div>

<div class="vision-object object-two">
<span>YOLO</span>
</div>

<div class="scan-line"></div>

<div class="vision-crosshair"></div>

</div>

<div class="vision-coordinates">
x: 042 &nbsp; y: 118 &nbsp; conf: 0.94
</div>

</div>

<div class="lab-content">

<div class="lab-icon">👁️</div>

<h2>Computer Vision Lab</h2>

<p>
Experiments with object detection, segmentation,
classification, medical imaging and image understanding.
</p>

<div class="lab-topics">
<span>YOLO</span>
<span>OpenCV</span>
<span>U-Net</span>
<span>Imaging</span>
</div>

<div class="lab-explore">
Explore Computer Vision
<span>→</span>
</div>

</div>

</a>

<!-- ================= EDGE AI ================= -->

<a class="lab-card lab-edge" href="/labs/edge-ai/">

<div class="lab-card-top">
<span class="lab-number">03</span>
<span class="lab-tag">EDGE</span>
</div>

<div class="lab-visual edge-visual">

<div class="edge-camera">
<div class="camera-lens"></div>
<div class="camera-body"></div>
</div>

<div class="edge-line line-camera"></div>

<div class="raspberry-pi">
<div class="pi-chip">AI</div>
<div class="pi-pin pin-one"></div>
<div class="pi-pin pin-two"></div>
<div class="pi-pin pin-three"></div>
<div class="pi-pin pin-four"></div>
</div>

<div class="edge-line line-output"></div>

<div class="edge-output">
<span class="output-pulse"></span>
SENSOR
</div>

<div class="edge-data">
CAMERA → EDGE MODEL → SENSOR
</div>

</div>

<div class="lab-content">

<div class="lab-icon">⚡</div>

<h2>Edge AI Lab</h2>

<p>
Exploring AI inference on Raspberry Pi,
cameras, sensors and resource-constrained hardware.
</p>

<div class="lab-topics">
<span>Raspberry Pi</span>
<span>TFLite</span>
<span>GPIO</span>
<span>IoT</span>
</div>

<div class="lab-explore">
Explore Edge AI
<span>→</span>
</div>

</div>

</a>

<!-- ================= LOCAL AI ================= -->

<a class="lab-card lab-local" href="/labs/local-ai/">

<div class="lab-card-top">
<span class="lab-number">04</span>
<span class="lab-tag">LOCAL</span>
</div>

<div class="lab-visual gpu-visual">

<div class="gpu-model">

<div class="gpu-header">
LOCAL MODEL
</div>

<div class="gpu-bars">
<span></span>
<span></span>
<span></span>
<span></span>
<span></span>
<span></span>
</div>

<div class="gpu-chip">
GPU
</div>

</div>

<div class="gpu-particles">
<span></span>
<span></span>
<span></span>
<span></span>
<span></span>
</div>

<div class="gpu-output">
<span>INFERENCE</span>
<strong>READY</strong>
</div>

<div class="gpu-label">
LOCAL INFERENCE
</div>

</div>

<div class="lab-content">

<div class="lab-icon">🖥️</div>

<h2>Local AI Lab</h2>

<p>
Running AI locally with GPUs, Docker,
LocalAI, ComfyUI and open-source models.
</p>

<div class="lab-topics">
<span>LocalAI</span>
<span>ComfyUI</span>
<span>CUDA</span>
<span>Docker</span>
</div>

<div class="lab-explore">
Explore Local AI
<span>→</span>
</div>

</div>

</a>

<!-- ================= INFRASTRUCTURE ================= -->

<a class="lab-card lab-infra" href="/labs/infrastructure/">

<div class="lab-card-top">
<span class="lab-number">05</span>
<span class="lab-tag">SYSTEMS</span>
</div>

<div class="lab-visual infra-visual">

<div class="infra-node server-node">
<span class="node-symbol">▣</span>
SERVER
<small>ONLINE</small>
</div>

<div class="infra-flow flow-one">
<span></span>
</div>

<div class="infra-node backup-node">
<span class="node-symbol">◈</span>
BACKUP
<small>SYNCING</small>
</div>

<div class="infra-flow flow-two">
<span></span>
</div>

<div class="infra-node recovery-node">
<span class="node-symbol">↻</span>
RECOVERY
<small>READY</small>
</div>

<div class="infra-data-label">
RPO / RTO / AUTOMATION
</div>

</div>

<div class="lab-content">

<div class="lab-icon">⚙️</div>

<h2>Infrastructure Lab</h2>

<p>
Infrastructure engineering, backup,
automation, Linux, containers and AI-powered operations.
</p>

<div class="lab-topics">
<span>Veeam</span>
<span>Linux</span>
<span>Docker</span>
<span>AIOps</span>
</div>

<div class="lab-explore">
Explore Infrastructure
<span>→</span>
</div>

</div>

</a>

<!-- ================= MACHINE LEARNING ================= -->

<a class="lab-card lab-ml" href="/labs/machine-learning/">

<div class="lab-card-top">
<span class="lab-number">06</span>
<span class="lab-tag">ML</span>
</div>

<div class="lab-visual ml-visual">

<div class="ml-network">

<span class="ml-node ml-node-one"></span>
<span class="ml-node ml-node-two"></span>
<span class="ml-node ml-node-three"></span>
<span class="ml-node ml-node-four"></span>
<span class="ml-node ml-node-five"></span>

<span class="ml-line ml-line-one"></span>
<span class="ml-line ml-line-two"></span>
<span class="ml-line ml-line-three"></span>
<span class="ml-line ml-line-four"></span>

<span class="ml-prediction">ŷ</span>

</div>

<div class="ml-data-label">
DATA → FEATURES → MODEL → PREDICTION
</div>

</div>

<div class="lab-content">

<div class="lab-icon">🧠</div>

<h2>Machine Learning Lab</h2>

<p>
Exploring supervised learning, unsupervised learning,
feature engineering, model evaluation and practical
machine-learning systems.
</p>

<div class="lab-topics">
<span>Python</span>
<span>Scikit-learn</span>
<span>Models</span>
<span>MLOps</span>
</div>

<div class="lab-explore">
Explore Machine Learning
<span>→</span>
</div>

</div>

</a>

</div>


<!-- ================= LAB PHILOSOPHY ================= -->

<section class="labs-philosophy">

<div class="philosophy-line"></div>

<div class="philosophy-content">

<span class="philosophy-label">LAB PHILOSOPHY</span>

<h2>
Build. Experiment. Break.
<span>Learn. Improve.</span>
</h2>

<p>
The Labs are where ideas are tested before they become projects.
Experiments, prototypes, benchmarks, failures and lessons learned
are documented here.
</p>

</div>

<div class="philosophy-terminal">

<div>
<span class="terminal-green">●</span>
SYSTEM
<strong>READY</strong>
</div>

<div>
EXPERIMENTS
<strong>ACTIVE</strong>
</div>

<div>
PROJECTS
<strong>EVOLVING</strong>
</div>

</div>

</section>

</section>