---
title: ""
description: "Interview-focused Computer Vision preparation covering image processing, OpenCV, CNNs, classification, object detection, segmentation, deployment, troubleshooting and senior-level architecture."
weight: 50
toc: false
cascade:
  type: docs
---

<style>
/* =========================================================
   COMPUTER VISION INTERVIEW PREP
   Landing-page structure mirrors the working Python page.
   CV-specific animation classes are kept isolated.
   ========================================================= */

.cv-prep-page {
  width: 100%;
}

/* ---------- Hero ---------- */

.cv-prep-status {
  display: flex;
  align-items: center;
  gap: 8px;a
  margin-bottom: 14px;
  color: #7dd3fc;
  font: 700 9px/1 monospace;
  letter-spacing: .12em;
}

.cv-prep-status-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: #22c55e;
  box-shadow: 0 0 10px rgba(34,197,94,.65);
  animation: cv-status-blink 1.8s infinite;
}

@keyframes cv-status-blink {
  0%,80%,100% { opacity:.35; }
  85% { opacity:1; }
}

.cv-prep-title {
  margin: 0;
}

.cv-prep-title span {
  color: #60a5fa;
}

.cv-prep-subtitle {
  margin: 10px 0 0;
  color: #7c8da3;
  font: 700 9px/1.6 monospace;
  letter-spacing: .08em;
}

.cv-prep-terminal {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 18px;
  padding: 9px 12px;
  border: 1px solid rgba(96,165,250,.18);
  background: rgba(7,17,29,.7);
  color: #93c5fd;
  font: 10px/1 monospace;
}

.cv-terminal-prompt {
  color: #22c55e;
}

.cv-terminal-command {
  color: #c4b5fd;
}

.cv-terminal-cursor {
  width: 6px;
  height: 12px;
  background: #60a5fa;
  animation: cv-cursor 1s steps(1) infinite;
}

@keyframes cv-cursor {
  50% { opacity: 0; }
}

/* ---------- Pipeline ---------- */

.cv-prep-pipeline {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 12px;
  margin: 34px 0 38px;
}

.cv-prep-stage {
  display: flex;
  align-items: center;
  gap: 7px;
  color: #8fa0b6;
  font: 700 8px/1 monospace;
  letter-spacing: .06em;
}

.cv-prep-stage span {
  color: #60a5fa;
}

.cv-prep-arrow {
  color: #4b5563;
  font: 12px/1 monospace;
}

/* ---------- Grid ---------- */

.cv-prep-visual-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 18px;
}

/* ---------- Cards ---------- */

.cv-prep-card {
  display: block;
  min-width: 0;
  overflow: hidden;

  border: 1px solid rgba(96,165,250,.16);
  border-radius: 14px;

  background: rgba(7,17,29,.72);

  color: inherit;
  text-decoration: none !important;

  transition:
    transform .25s ease,
    border-color .25s ease,
    box-shadow .25s ease;
}

.cv-prep-card:hover,
.cv-prep-card:focus,
.cv-prep-card:visited {
  color: inherit;
  text-decoration: none !important;
}

.cv-prep-card:hover {
  transform: translateY(-4px);
  border-color: rgba(96,165,250,.38);
  box-shadow: 0 12px 30px rgba(0,0,0,.18);
}

.cv-prep-card * {
  text-decoration: none !important;
}

.cv-prep-card-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 11px 14px 0;

  color: #64748b;
  font: 9px/1 monospace;
}

.cv-prep-card-top small {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #64748b;
  font: 700 8px/1 monospace;
  letter-spacing: .05em;
}

.cv-prep-live-dot {
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: #22c55e;
  box-shadow: 0 0 8px rgba(34,197,94,.55);
}

/* ---------- CV visual area ---------- */

.cv-prep-tile {
  position: relative;
  width: 100%;
  height: 215px;
  margin-top: 10px;
  overflow: hidden;
  border-top: 1px solid rgba(96,165,250,.08);
  border-bottom: 1px solid rgba(96,165,250,.12);
  background: #07111d;
}

.cv-visual {
  position: relative;
  width: 100%;
  height: 215px;
  overflow: hidden;
  background: #07111d;
}

.cv-grid {
  position: absolute;
  inset: 0;
  opacity: .42;
  background-image:
    linear-gradient(rgba(96,165,250,.12) 1px, transparent 1px),
    linear-gradient(90deg, rgba(96,165,250,.12) 1px, transparent 1px);
  background-size: 25px 25px;
}

.cv-visual-label {
  position: absolute;
  left: 14px;
  bottom: 12px;
  color: #64748b;
  font: 700 7px/1 monospace;
  letter-spacing: .08em;
}

/* ---------- Camera ---------- */

.cv-camera {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 112px;
  height: 70px;
  transform: translate(-50%,-50%);
  border: 1px solid rgba(147,197,253,.45);
  border-radius: 8px;
  background: rgba(96,165,250,.035);
}

.cv-camera::before {
  content: "";
  position: absolute;
  left: 24px;
  top: 18px;
  width: 45px;
  height: 38px;
  border: 1px solid rgba(147,197,253,.6);
  border-radius: 50%;
  animation: cv-lens 2.5s ease-in-out infinite;
}

.cv-camera::after {
  content: "";
  position: absolute;
  right: 12px;
  top: 9px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #22c55e;
  box-shadow: 0 0 12px #22c55e;
  animation: cv-blink 1.8s infinite;
}

@keyframes cv-lens {
  0%,100% { transform: scale(.88); opacity: .45; }
  50% { transform: scale(1.08); opacity: 1; }
}

@keyframes cv-blink {
  0%,80%,100% { opacity: .35; }
  85% { opacity: 1; }
}

.cv-scan-line {
  position: absolute;
  left: 8%;
  right: 8%;
  top: 18%;
  height: 1px;
  background: #60a5fa;
  box-shadow: 0 0 10px rgba(96,165,250,.65);
  animation: cv-scan 2.8s linear infinite;
}

@keyframes cv-scan {
  0% { transform: translateY(0); opacity: .2; }
  50% { opacity: 1; }
  100% { transform: translateY(135px); opacity: .2; }
}

/* ---------- Pixel visual ---------- */

.cv-pixels {
  position: absolute;
  left: 50%;
  top: 50%;
  display: grid;
  grid-template-columns: repeat(7, 16px);
  gap: 5px;
  transform: translate(-50%,-50%);
}

.cv-pixel {
  width: 16px;
  height: 16px;
  border: 1px solid rgba(147,197,253,.24);
  background: rgba(96,165,250,.05);
  animation: cv-pixel 2.8s ease-in-out infinite;
}

.cv-pixel:nth-child(2n) { animation-delay: .2s; }
.cv-pixel:nth-child(3n) { animation-delay: .4s; }
.cv-pixel:nth-child(4n) { animation-delay: .6s; }

@keyframes cv-pixel {
  0%,100% { opacity:.35; transform: scale(.8); }
  50% {
    opacity:1;
    transform: scale(1);
    background: rgba(96,165,250,.28);
    box-shadow: 0 0 8px rgba(96,165,250,.16);
  }
}

/* ---------- Detection ---------- */

.cv-detection {
  position: absolute;
  inset: 0;
}

.cv-detection-box {
  position: absolute;
  width: 86px;
  height: 62px;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  border: 1px solid #60a5fa;
  animation: cv-detection-box 2.5s ease-in-out infinite;
}

.cv-detection-label {
  position: absolute;
  left: 50%;
  top: calc(50% - 48px);
  transform: translateX(-50%);
  padding: 4px 6px;
  color: #93c5fd;
  background: rgba(7,17,29,.82);
  border: 1px solid rgba(96,165,250,.35);
  font: 700 7px/1 monospace;
}

@keyframes cv-detection-box {
  0%,100% { transform: translate(-50%,-50%) scale(.92); opacity:.55; }
  50% { transform: translate(-50%,-50%) scale(1); opacity:1; }
}

/* ---------- Segmentation ---------- */

.cv-segmentation {
  position: absolute;
  inset: 0;
}

.cv-mask {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 100px;
  height: 74px;
  transform: translate(-50%,-50%);
  border: 1px solid rgba(168,85,247,.55);
  background:
    radial-gradient(circle at 35% 45%, rgba(168,85,247,.34), transparent 32%),
    radial-gradient(circle at 68% 55%, rgba(96,165,250,.28), transparent 30%);
  animation: cv-mask 2.8s ease-in-out infinite;
}

@keyframes cv-mask {
  0%,100% { opacity:.45; transform: translate(-50%,-50%) scale(.92); }
  50% { opacity:1; transform: translate(-50%,-50%) scale(1); }
}

/* ---------- Architecture ---------- */

.cv-architecture {
  position: absolute;
  inset: 0;
}

.cv-arch-node {
  position: absolute;
  padding: 8px 10px;
  border: 1px solid rgba(249,115,22,.42);
  border-radius: 5px;
  color: #fdba74;
  background: rgba(249,115,22,.045);
  font: 700 8px monospace;
  animation: cv-arch 2.8s ease-in-out infinite;
}

.cv-arch-node.a1 { left: 12%; top: 45%; }
.cv-arch-node.a2 { left: 42%; top: 22%; animation-delay:.5s; }
.cv-arch-node.a3 { left: 42%; top: 67%; animation-delay:1s; }
.cv-arch-node.a4 { right: 11%; top: 45%; animation-delay:1.5s; }

.cv-arch-line {
  position: absolute;
  height: 1px;
  background: rgba(249,115,22,.35);
}

.cv-arch-line.a { left: 25%; top: 51%; width: 18%; transform: rotate(-16deg); }
.cv-arch-line.b { left: 25%; top: 51%; width: 18%; transform: rotate(16deg); }
.cv-arch-line.c { left: 56%; top: 35%; width: 18%; transform: rotate(16deg); }
.cv-arch-line.d { left: 56%; top: 70%; width: 18%; transform: rotate(-16deg); }

@keyframes cv-arch {
  0%,100% { opacity:.45; }
  50% { opacity:1; box-shadow: 0 0 13px rgba(249,115,22,.14); }
}

/* ---------- Feature maps ---------- */

.cv-feature-map {
  position: absolute;
  left: 50%;
  top: 50%;
  display: grid;
  grid-template-columns: repeat(5, 22px);
  gap: 5px;
  transform: translate(-50%,-50%);
}

.cv-feature {
  width: 22px;
  height: 22px;
  border: 1px solid rgba(168,85,247,.25);
  background: rgba(168,85,247,.05);
  animation: cv-feature 2.9s ease-in-out infinite;
}

.cv-feature:nth-child(2n) { animation-delay:.25s; }
.cv-feature:nth-child(3n) { animation-delay:.5s; }
.cv-feature:nth-child(4n) { animation-delay:.75s; }
.cv-feature:nth-child(5n) { animation-delay:1s; }

@keyframes cv-feature {
  0%,100% { opacity:.35; transform: scale(.86); }
  50% {
    opacity:1;
    transform: scale(1);
    background: rgba(168,85,247,.32);
    box-shadow: 0 0 8px rgba(168,85,247,.18);
  }
}

/* ---------- Card content ---------- */

.cv-prep-card-content {
  padding: 18px;
}

.cv-prep-card-icon {
  margin-bottom: 10px;
  font-size: 23px;
}

.cv-prep-card h2 {
  margin: 0 0 10px;
  padding-bottom: 9px;
  border-bottom: 1px solid rgba(96,165,250,.15);
  font-size: 19px;
  line-height: 1.2;
}

.cv-prep-card p {
  margin: 0 0 16px;
  color: #a8a2ae;
  font-size: 12px;
  line-height: 1.7;
}

.cv-prep-topics {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-bottom: 15px;
}

.cv-prep-topics span {
  padding: 5px 8px;
  border: 1px solid rgba(96,165,250,.17);
  border-radius: 4px;
  color: #8fa0b6;
  font: 8px/1 monospace;
}

.cv-prep-explore {
  display: flex;
  justify-content: space-between;
  padding-top: 11px;
  border-top: 1px solid rgba(96,165,250,.13);
  color: #7dd3fc;
  font: 10px/1 monospace;
}

.cv-prep-explore span {
  transition: transform .25s ease;
}

.cv-prep-card:hover .cv-prep-explore span {
  transform: translateX(5px);
}

/* ---------- Philosophy ---------- */

.cv-prep-philosophy {
  margin-top: 42px;
  padding: 24px;
  border: 1px solid rgba(96,165,250,.14);
  border-radius: 12px;
  background: rgba(7,17,29,.5);
}

.cv-prep-philosophy-label {
  color: #64748b;
  font: 700 8px/1 monospace;
  letter-spacing: .1em;
}

.cv-prep-philosophy h2 {
  margin: 10px 0;
  font-size: 25px;
}

.cv-prep-philosophy h2 span {
  color: #60a5fa;
}

.cv-prep-philosophy p {
  max-width: 700px;
  margin: 0;
  color: #8fa0b6;
  font-size: 12px;
  line-height: 1.7;
}

.cv-prep-philosophy-flow {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 8px;
  margin-top: 18px;
  color: #7dd3fc;
  font: 8px/1 monospace;
}

.cv-prep-philosophy-flow i {
  color: #475569;
  font-style: normal;
}

/* ---------- Responsive ---------- */

@media (max-width: 760px) {
  .cv-prep-visual-grid {
    grid-template-columns: 1fr;
  }

  .cv-prep-pipeline {
    justify-content: flex-start;
  }

  .cv-prep-arrow {
    display: none;
  }
}
</style>

<section class="cv-prep-page">

  <section class="cv-prep-hero">
    <div class="cv-prep-status">
      <span class="cv-prep-status-dot" href="/computer-vision/gen-ai/"></span>
      COMPUTER VISION INTERVIEW WORKBENCH
    </div>
    <h1 class="cv-prep-title">
      Computer Vision <span>Interview Prep</span>
    </h1>
    <p class="cv-prep-subtitle">
      IMAGE PROCESSING · DEEP LEARNING · DETECTION · SEGMENTATION · DEPLOYMENT
    </p>
    <div class="cv-prep-terminal">
      <span class="cv-terminal-prompt">$</span>
      <span class="cv-terminal-command">initialize_cv_prep()</span>
      <span class="cv-terminal-cursor"></span>
    </div>

  </section>

  <section class="cv-prep-pipeline">
    <div class="cv-prep-stage">
      <span>01</span>
      <strong>LEARN</strong>
    </div>
    <div class="cv-prep-arrow">→</div>
    <div class="cv-prep-stage">
      <span>02</span>
      <strong>PROCESS</strong>
    </div>
    <div class="cv-prep-arrow">→</div>
    <div class="cv-prep-stage">
      <span>03</span>
      <strong>DETECT</strong>
    </div>
    <div class="cv-prep-arrow">→</div>
    <div class="cv-prep-stage">
      <span>04</span>
      <strong>EVALUATE</strong>
    </div>
    <div class="cv-prep-arrow">→</div>
    <div class="cv-prep-stage">
      <span>05</span>
      <strong>DEPLOY</strong>
    </div>

  </section>

  <section class="cv-prep-visual-grid">
    <!-- 01 QUICK START -->
    <a class="cv-prep-card cv-prep-card-quick"
       href="/interview-prep/computer-vision/quick-start/">
      <div class="cv-prep-card-top">
        <span>01</span>
        <small><i class="cv-prep-live-dot"></i>START HERE</small>
      </div>
      <div class="cv-prep-tile cv-visual">
        <div class="cv-grid"></div>
        <div class="cv-camera"></div>
        <div class="cv-scan-line"></div>
        <div class="cv-visual-label">IMAGE → PIXELS → PIPELINE</div>
      </div>
      <div class="cv-prep-card-content">
        <div class="cv-prep-card-icon">🚀</div>
        <h2>Quick Start</h2>
        <p>
          Start with the Computer Vision pipeline, core terminology,
          image representations and the concepts most frequently used
          in interviews.
        </p>
        <div class="cv-prep-topics">
          <span>Fundamentals</span>
          <span>Pipeline</span>
        </div>
        <div class="cv-prep-explore">
          Start Quick Start <span>→</span>
        </div>
      </div>
    </a>
    <!-- 02 RAPID REVISION -->
    <a class="cv-prep-card cv-prep-card-revision"
       href="/interview-prep/computer-vision/rapid-revision/">
      <div class="cv-prep-card-top">
        <span>02</span>
        <small>RAPID REVISION</small>
      </div>
      <div class="cv-prep-tile cv-visual">
        <div class="cv-revision-stack">
          <div class="cv-revision-row"></div>
          <div class="cv-revision-row"></div>
          <div class="cv-revision-row"></div>
          <div class="cv-revision-row"></div>
          <div class="cv-revision-row"></div>
        </div>
        <div class="cv-visual-label">FAST REVIEW MODE</div>
      </div>
      <div class="cv-prep-card-content">
        <div class="cv-prep-card-icon">⚡</div>
        <h2>Rapid Revision</h2>
        <p>
          Fast revision of image representations, OpenCV operations,
          CNN terminology, common metrics and interview-ready definitions.
        </p>
        <div class="cv-prep-topics">
          <span>Revision</span>
          <span>Metrics</span>
        </div>
        <div class="cv-prep-explore">
          Start Rapid Revision <span>→</span>
        </div>
      </div>
    </a>
    <!-- 03 CORE CONCEPTS -->
    <a class="cv-prep-card cv-prep-card-core"
       href="/interview-prep/computer-vision/core-concepts/">
      <div class="cv-prep-card-top">
        <span>03</span>
        <small>CORE CONCEPTS</small>
      </div>
      <div class="cv-prep-tile cv-visual">
        <div class="cv-pixels">
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span>
        </div>
        <div class="cv-visual-label">PIXELS → FEATURES</div>
      </div>
      <div class="cv-prep-card-content">
        <div class="cv-prep-card-icon">🧠</div>
        <h2>Core Concepts</h2>
        <p>
          Images, pixels, color spaces, filtering, thresholding,
          contours, transformations and essential OpenCV concepts.
        </p>
        <div class="cv-prep-topics">
          <span>OpenCV</span>
          <span>Image Processing</span>
        </div>
        <div class="cv-prep-explore">
          Explore Core Concepts <span>→</span>
        </div>
      </div>
    </a>
    <!-- 04 DEEP LEARNING -->
    <a class="cv-prep-card cv-prep-card-deep-learning"
       href="/interview-prep/computer-vision/deep-learning/">
      <div class="cv-prep-card-top">
        <span>04</span>
        <small>DEEP LEARNING</small>
      </div>
      <div class="cv-prep-tile cv-visual">
        <div class="cv-pixels">
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
          <span class="cv-pixel"></span><span class="cv-pixel"></span>
        </div>
        <div class="cv-visual-label">FEATURES → CNN → PREDICTION</div>
      </div>
      <div class="cv-prep-card-content">
        <div class="cv-prep-card-icon">🧬</div>
        <h2>Deep Learning</h2>
        <p>
          CNNs, convolution, pooling, padding, stride, activations,
          normalization, dropout and transfer learning.
        </p>
        <div class="cv-prep-topics">
          <span>CNN</span>
          <span>Transfer Learning</span>
        </div>
        <div class="cv-prep-explore">
          Explore Deep Learning <span>→</span>
        </div>
      </div>
    </a>
    <!-- 05 CLASSIFICATION -->
    <a class="cv-prep-card"
       href="/interview-prep/computer-vision/classification/">
      <div class="cv-prep-card-top">
        <span>05</span>
        <small>CLASSIFICATION</small>
      </div>
      <div class="cv-prep-tile cv-visual">
        <div class="cv-detection">
          <div class="cv-detection-box"></div>
          <div class="cv-detection-label">CLASS 0.96</div>
        </div>
        <div class="cv-visual-label">IMAGE → CLASS</div>
      </div>
      <div class="cv-prep-card-content">
        <div class="cv-prep-card-icon">🎯</div>
        <h2>Image Classification</h2>
        <p>
          Classification pipelines, CNN architectures, transfer learning,
          confusion matrices, precision, recall, F1 and accuracy.
        </p>
        <div class="cv-prep-topics">
          <span>CNN</span>
          <span>Metrics</span>
        </div>
        <div class="cv-prep-explore">
          Explore Classification <span>→</span>
        </div>
      </div>
    </a>
    <!-- 06 DETECTION -->
    <a class="cv-prep-card cv-prep-card-detection"
       href="/interview-prep/computer-vision/object-detection/">
      <div class="cv-prep-card-top">
        <span>06</span>
        <small>DETECTION</small>
      </div>
      <div class="cv-prep-tile cv-visual">
        <div class="cv-detection">
          <div class="cv-detection-box"></div>
          <div class="cv-detection-label">OBJECT 0.94</div>
        </div>
        <div class="cv-visual-label">LOCALIZE → CLASSIFY</div>
      </div>
      <div class="cv-prep-card-content">
        <div class="cv-prep-card-icon">🔲</div>
        <h2>Object Detection</h2>
        <p>
          Bounding boxes, IoU, NMS, YOLO, SSD, Faster R-CNN and detection
          metrics including mAP.
        </p>
        <div class="cv-prep-topics">
          <span>YOLO</span>
          <span>IoU</span>
          <span>mAP</span>
        </div>
        <div class="cv-prep-explore">
          Explore Object Detection <span>→</span>
        </div>
      </div>
    </a>
    <!-- 07 SEGMENTATION -->
    <a class="cv-prep-card cv-prep-card-segmentation"
       href="/interview-prep/computer-vision/image-segmentation/">
      <div class="cv-prep-card-top">
        <span>07</span>
        <small>SEGMENTATION</small>
      </div>
      <div class="cv-prep-tile cv-visual">
        <div class="cv-segmentation">
          <div class="cv-mask"></div>
        </div>
        <div class="cv-visual-label">PIXEL → MASK</div>
      </div>
      <div class="cv-prep-card-content">
        <div class="cv-prep-card-icon">🧩</div>
        <h2>Image Segmentation</h2>
        <p>
          Semantic, instance and panoptic segmentation, U-Net,
          Mask R-CNN, Dice and IoU.
        </p>
        <div class="cv-prep-topics">
          <span>U-Net</span>
          <span>Mask R-CNN</span>
        </div>
        <div class="cv-prep-explore">
          Explore Segmentation <span>→</span>
        </div>
      </div>
    </a>
    <!-- 08 COMPUTER VISION WITH PYTHON -->
    <a class="cv-prep-card"
       href="/interview-prep/computer-vision/python/">
      <div class="cv-prep-card-top">
        <span>08</span>
        <small>IMPLEMENTATION</small>
      </div>
      <div class="cv-prep-tile cv-visual">
        <div class="cv-camera"></div>
        <div class="cv-grid"></div>
        <div class="cv-scan-line"></div>
        <div class="cv-visual-label">PYTHON → OPENCV → INFERENCE</div>
      </div>
      <div class="cv-prep-card-content">
        <div class="cv-prep-card-icon">🐍</div>
        <h2>Computer Vision with Python</h2>
        <p>
          OpenCV, NumPy, Pillow, PyTorch, TensorFlow, preprocessing
          and inference workflows.
        </p>
        <div class="cv-prep-topics">
          <span>Python</span>
          <span>OpenCV</span>
        </div>
        <div class="cv-prep-explore">
          Explore Python CV <span>→</span>
        </div>
      </div>
    </a>
    <!-- 09 TROUBLESHOOTING -->
    <a class="cv-prep-card"
       href="/interview-prep/computer-vision/troubleshooting/">
      <div class="cv-prep-card-top">
        <span>09</span>
        <small>TROUBLESHOOTING</small>
      </div>
      <div class="cv-prep-tile cv-visual">
        <div class="cv-architecture">
          <div class="cv-arch-node a1">INPUT</div>
          <div class="cv-arch-node a2">MODEL</div>
          <div class="cv-arch-node a3">DATA</div>
          <div class="cv-arch-node a4">OUTPUT</div>
          <div class="cv-arch-line a"></div>
          <div class="cv-arch-line b"></div>
          <div class="cv-arch-line c"></div>
          <div class="cv-arch-line d"></div>
        </div>
        <div class="cv-visual-label">OBSERVE → DIAGNOSE → FIX</div>
      </div>
      <div class="cv-prep-card-content">
        <div class="cv-prep-card-icon">🛠️</div>
        <h2>Troubleshooting</h2>
        <p>
          Debugging preprocessing, data quality, model behavior,
          inference failures, performance problems and production issues.
        </p>
        <div class="cv-prep-topics">
          <span>Debugging</span>
          <span>Production</span>
        </div>
        <div class="cv-prep-explore">
          Explore Troubleshooting <span>→</span>
        </div>
      </div>
    </a>
    <!-- 10 SENIOR SCENARIOS -->
    <a class="cv-prep-card"
       href="/interview-prep/computer-vision/senior-scenarios/">
      <div class="cv-prep-card-top">
        <span>10</span>
        <small>SENIOR SCENARIOS</small>
      </div>
      <div class="cv-prep-tile cv-visual">
        <div class="cv-architecture">
          <div class="cv-arch-node a1">DATA</div>
          <div class="cv-arch-node a2">TRAIN</div>
          <div class="cv-arch-node a3">SERVE</div>
          <div class="cv-arch-node a4">MONITOR</div>
          <div class="cv-arch-line a"></div>
          <div class="cv-arch-line b"></div>
          <div class="cv-arch-line c"></div>
          <div class="cv-arch-line d"></div>
        </div>
        <div class="cv-visual-label">SYSTEM → SCALE → TRADEOFFS</div>
     </div>
      <div class="cv-prep-card-content">
        <div class="cv-prep-card-icon">🏗️</div>
        <h2>Senior Scenarios</h2>
        <p>
          System design, model selection, scalability, latency,
          deployment trade-offs and production Computer Vision architecture.
        </p>
        <div class="cv-prep-topics">
          <span>Architecture</span>
          <span>Scale</span>
        </div>
        <div class="cv-prep-explore">
          Explore Senior Scenarios <span>→</span>
        </div>
      </div>
    </a>
    <!-- 11 DEEP DIVE -->
    <a class="cv-prep-card"
       href="/interview-prep/computer-vision/deep-dive/">
      <div class="cv-prep-card-top">
        <span>11</span>
        <small>DEEP DIVE</small>
      </div>
      <div class="cv-prep-tile cv-visual">
        <div class="cv-feature-map">
          <span class="cv-feature"></span><span class="cv-feature"></span>
          <span class="cv-feature"></span><span class="cv-feature"></span>
          <span class="cv-feature"></span><span class="cv-feature"></span>
          <span class="cv-feature"></span><span class="cv-feature"></span>
          <span class="cv-feature"></span><span class="cv-feature"></span>
          <span class="cv-feature"></span><span class="cv-feature"></span>
          <span class="cv-feature"></span><span class="cv-feature"></span>
          <span class="cv-feature"></span><span class="cv-feature"></span>
          <span class="cv-feature"></span><span class="cv-feature"></span>
          <span class="cv-feature"></span><span class="cv-feature"></span>
        </div>
        <div class="cv-visual-label">FEATURE MAPS → REPRESENTATIONS</div>
      </div>
      <div class="cv-prep-card-content">
        <div class="cv-prep-card-icon">🔬</div>
        <h2>Deep Dive</h2>
        <p>
          CNN internals, feature maps, Vision Transformers,
          optimization, quantization and deployment.
        </p>
        <div class="cv-prep-topics">
          <span>CNN Internals</span>
          <span>ViT</span>
        </div>
        <div class="cv-prep-explore">
          Explore Deep Dive <span>→</span>
        </div>
      </div>
    </a>
  </section>
  <section class="cv-prep-philosophy">
    <span class="cv-prep-philosophy-label">
      COMPUTER VISION INTERVIEW PHILOSOPHY
    </span>
    <h2>
      Understand the image.
      <span>Explain the system.</span>
    </h2>
    <p>
      Strong Computer Vision interview answers combine fundamentals,
      model knowledge, evaluation, debugging and deployment judgment.
    </p>
    <div class="cv-prep-philosophy-flow">
      <span>IMAGE</span>
      <i>→</i>
      <span>MODEL</span>
      <i>→</i>
      <span>EVALUATE</span>
      <i>→</i>
      <span>DEBUG</span>
      <i>→</i>
      <span>DEPLOY</span>
    </div>

  </section>

</section>
