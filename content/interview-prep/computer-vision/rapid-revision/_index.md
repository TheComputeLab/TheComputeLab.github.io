---
title: ""
description: "Rapid revision guide for Computer Vision interviews covering image processing, OpenCV, CNNs, classification, detection, segmentation, metrics and deployment."
hideTitle: true
weight: 100
toc: true
cascade:
  type: docs
---
<style>
.cv-revision-page{width:100%;max-width:980px;margin:0 auto;padding:8px 0 60px}
.cv-revision-hero{position:relative;overflow:hidden;margin-bottom:28px;padding:30px 32px;border:1px solid rgba(96,165,250,.24);border-radius:16px;background:linear-gradient(rgba(96,165,250,.045) 1px,transparent 1px),linear-gradient(90deg,rgba(96,165,250,.045) 1px,transparent 1px),#07111d;background-size:24px 24px}
.cv-revision-hero>*{position:relative;z-index:1}
.cv-revision-status{display:flex;align-items:center;gap:8px;margin-bottom:16px;color:#7dd3fc;font:700 9px monospace;letter-spacing:.12em}
.cv-revision-dot{width:7px;height:7px;border-radius:50%;background:#22c55e;box-shadow:0 0 10px rgba(34,197,94,.65)}
.cv-revision-hero h1{margin:0 0 12px;font-size:clamp(38px,6vw,58px);line-height:1;letter-spacing:-.04em}
.cv-revision-hero h1 span{color:#60a5fa}
.cv-revision-hero p{max-width:760px;margin:0;color:#9aa9bb;font-size:13px;line-height:1.75}
.cv-revision-terminal{display:inline-flex;align-items:center;gap:8px;margin-top:20px;padding:10px 14px;border:1px solid rgba(96,165,250,.18);border-radius:6px;background:rgba(7,17,29,.75);font:10px monospace}
.cv-revision-terminal .prompt{color:#22c55e}
.cv-revision-terminal .command{color:#93c5fd}
.cv-revision-section{margin-top:34px}
.cv-revision-label{margin-bottom:7px;color:#60a5fa;font:700 9px monospace;letter-spacing:.12em}
.cv-revision-section h2{margin:0 0 10px;padding-bottom:9px;border-bottom:1px solid rgba(96,165,250,.15);font-size:24px}
.cv-revision-section>p{color:#9aa9bb;font-size:13px;line-height:1.75}
.cv-revision-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:14px;margin-top:18px}
.cv-revision-card{padding:18px;border:1px solid rgba(96,165,250,.14);border-radius:9px;background:rgba(7,17,29,.48)}
.cv-revision-card h3{margin:0 0 8px;font-size:15px}
.cv-revision-card p{margin:0;color:#9aa9bb;font-size:12px;line-height:1.7}
.cv-revision-card code{color:#93c5fd;font-family:monospace}
.cv-revision-table-wrap{margin-top:18px;overflow-x:auto;border:1px solid rgba(96,165,250,.14);border-radius:9px}
.cv-revision-table{width:100%;border-collapse:collapse;min-width:650px}
.cv-revision-table th,.cv-revision-table td{padding:11px 13px;border-bottom:1px solid rgba(96,165,250,.1);text-align:left;font-size:11px;line-height:1.55}
.cv-revision-table th{color:#7dd3fc;background:rgba(96,165,250,.045);font-family:monospace}
.cv-revision-table td{color:#9aa9bb}
.cv-revision-table tr:last-child td{border-bottom:0}
.cv-revision-pills{display:flex;flex-wrap:wrap;gap:7px;margin-top:16px}
.cv-revision-pill{padding:6px 9px;border:1px solid rgba(96,165,250,.17);border-radius:5px;color:#aebdcd;background:rgba(96,165,250,.035);font:9px monospace}
.cv-revision-callout{margin-top:18px;padding:15px 17px;border-left:2px solid #60a5fa;background:rgba(96,165,250,.045)}
.cv-revision-callout strong{color:#e2e8f0;font-size:12px}
.cv-revision-callout p{margin:6px 0 0;color:#9aa9bb;font-size:12px;line-height:1.7}
.cv-revision-checklist{display:grid;gap:8px;margin-top:16px}
.cv-revision-check{padding:10px 13px;border:1px solid rgba(96,165,250,.11);border-radius:6px;color:#aebdcd;background:rgba(7,17,29,.4);font:11px/1.5 monospace}
.cv-revision-check::before{content:"✓";margin-right:9px;color:#22c55e}
@media(max-width:700px){.cv-revision-hero{padding:24px 20px}.cv-revision-grid{grid-template-columns:1fr}.cv-revision-table{min-width:600px}}
</style>
<section class="cv-revision-page">
<section class="cv-revision-hero">
<div class="cv-revision-status"><span class="cv-revision-dot"></span>COMPUTER VISION • RAPID REVISION</div>
<h1>Computer Vision <span>Rapid Revision</span></h1>
<p>A compact interview revision sheet for the concepts you should be able to recall quickly: image processing, OpenCV, CNNs, classification, detection, segmentation, metrics and deployment.</p>
<div class="cv-revision-terminal"><span class="prompt">$</span><span class="command">run_rapid_revision()</span></div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">01 — IMAGE FUNDAMENTALS</div>

## Image Processing Essentials

<p>Remember what the basic image operations do and why they are used before a model sees the image.</p>
<div class="cv-revision-grid">
<div class="cv-revision-card">
### Pixel
<p>Smallest addressable image element containing an intensity or color value.</p></div>
<div class="cv-revision-card">
### Resolution
<p>Spatial dimensions of an image, commonly expressed as width × height.</p></div>
<div class="cv-revision-card">
### Channels
<p>Separate image components such as grayscale intensity or RGB color channels.</p></div>
<div class="cv-revision-card">
### Color Space
<p>Representation such as RGB, BGR, HSV or grayscale. Choose based on the task.</p></div>
<div class="cv-revision-card">
### Resize
<p>Changes spatial dimensions to meet model or processing requirements.</p></div>
<div class="cv-revision-card">
### Normalization
<p>Scales numerical pixel values into a range suitable for processing or model input.</p></div>
</div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">02 — FILTERING</div>

## Filtering & Transformations

<div class="cv-revision-table-wrap">
<table class="cv-revision-table">
<thead><tr><th>Concept</th><th>Quick recall</th><th>Interview angle</th></tr></thead>
<tbody>
<tr><td>Gaussian Blur</td><td>Reduces high-frequency noise using a Gaussian kernel.</td><td>Useful before edge detection or other noisy operations.</td></tr>
<tr><td>Median Filter</td><td>Replaces a pixel using a neighborhood median.</td><td>Useful for impulse/salt-and-pepper noise.</td></tr>
<tr><td>Sobel</td><td>Estimates image gradients.</td><td>Can highlight horizontal or vertical intensity changes.</td></tr>
<tr><td>Canny</td><td>Multi-stage edge detection algorithm.</td><td>Know why smoothing and thresholds matter.</td></tr>
<tr><td>Thresholding</td><td>Converts intensity information into regions based on a threshold.</td><td>Useful when foreground/background separation is simple.</td></tr>
<tr><td>Morphology</td><td>Operations such as erosion and dilation using a structuring element.</td><td>Useful for cleaning masks and connecting or removing regions.</td></tr>
</tbody>
</table>
</div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">03 — OPENCV</div>

## OpenCV Quick Recall

<div class="cv-revision-grid">
<div class="cv-revision-card">
### cv2.imread()
<p>Reads an image from disk.</p></div>
<div class="cv-revision-card">
### cv2.resize()
<p>Changes image dimensions.</p></div>
<div class="cv-revision-card">
### cv2.cvtColor()
<p>Converts between color representations such as BGR and grayscale.</p></div>
<div class="cv-revision-card">
### cv2.threshold()
<p>Applies a thresholding operation to an image.</p></div>
<div class="cv-revision-card">
### cv2.findContours()
<p>Extracts contours from suitable binary images.</p></div>
<div class="cv-revision-card">
### cv2.VideoCapture()
<p>Provides access to video files or camera streams.</p></div>
</div>
<div class="cv-revision-callout"><strong>Interview trap:</strong><p>OpenCV commonly loads color images in BGR channel order, while many other Python tools and model pipelines expect RGB. Always check the expected channel order.</p></div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">04 — CNN</div>

## CNN Concepts in One View

<div class="cv-revision-pills">
<span class="cv-revision-pill">Convolution</span><span class="cv-revision-pill">Kernel</span><span class="cv-revision-pill">Stride</span><span class="cv-revision-pill">Padding</span><span class="cv-revision-pill">Feature Map</span><span class="cv-revision-pill">Pooling</span><span class="cv-revision-pill">Activation</span><span class="cv-revision-pill">Batch Normalization</span><span class="cv-revision-pill">Dropout</span><span class="cv-revision-pill">Transfer Learning</span>
</div>
<div class="cv-revision-table-wrap">
<table class="cv-revision-table">
<thead><tr><th>Concept</th><th>Remember this</th></tr></thead>
<tbody>
<tr><td>Kernel / Filter</td><td>Learnable weights that slide across an input to detect local patterns.</td></tr>
<tr><td>Stride</td><td>Controls how far the kernel moves between positions.</td></tr>
<tr><td>Padding</td><td>Adds border values to control spatial dimensions and edge handling.</td></tr>
<tr><td>Feature Map</td><td>Activation output produced after applying learned filters.</td></tr>
<tr><td>Pooling</td><td>Reduces spatial resolution and can provide some translation robustness.</td></tr>
<tr><td>ReLU</td><td>Common activation that outputs zero for negative inputs and passes positive values.</td></tr>
</tbody>
</table>
</div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">05 — CLASSIFICATION</div>

## Classification Metrics

<div class="cv-revision-table-wrap">
<table class="cv-revision-table">
<thead><tr><th>Metric</th><th>Quick recall</th><th>Think about</th></tr></thead>
<tbody>
<tr><td>Accuracy</td><td>Correct predictions / total predictions.</td><td>Can be misleading with imbalanced classes.</td></tr>
<tr><td>Precision</td><td>TP / (TP + FP)</td><td>How trustworthy are positive predictions?</td></tr>
<tr><td>Recall</td><td>TP / (TP + FN)</td><td>How many actual positives were found?</td></tr>
<tr><td>F1</td><td>Harmonic mean of precision and recall.</td><td>Useful when both precision and recall matter.</td></tr>
<tr><td>Confusion Matrix</td><td>Table of TP, TN, FP and FN.</td><td>Useful for understanding class-specific failures.</td></tr>
</tbody>
</table>
</div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">06 — OBJECT DETECTION</div>

## Detection Quick Recall

<div class="cv-revision-grid">
<div class="cv-revision-card">
### Bounding Box
<p>Defines the spatial region containing a detected object.</p></div>
<div class="cv-revision-card">
### IoU
<p>Intersection over Union measures overlap between predicted and reference regions.</p></div>
<div class="cv-revision-card">
### NMS
<p>Non-Maximum Suppression removes highly overlapping duplicate detections.</p></div>
<div class="cv-revision-card">
### Confidence
<p>Model score associated with a detection. Threshold selection affects precision and recall.</p></div>
<div class="cv-revision-card">
### mAP
<p>Mean Average Precision summarizes detection precision-recall performance across classes and evaluation settings.</p></div>
<div class="cv-revision-card">
### YOLO
<p>A family of one-stage object detection approaches designed for efficient detection.</p></div>
</div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">07 — SEGMENTATION</div>

## Segmentation Quick Recall

<div class="cv-revision-grid">
<div class="cv-revision-card">
### Semantic
<p>Assigns a class label to each pixel.</p></div>
<div class="cv-revision-card">
### Instance
<p>Separates individual object instances, even when they share the same class.</p></div>
<div class="cv-revision-card">
### Panoptic
<p>Combines semantic and instance-style understanding across the image.</p></div>
<div class="cv-revision-card">
### U-Net
<p>Encoder-decoder architecture widely associated with biomedical and pixel-level segmentation.</p></div>
<div class="cv-revision-card">
### Dice
<p>Overlap-based metric commonly used for segmentation masks.</p></div>
<div class="cv-revision-card">
### Mask R-CNN
<p>Extends object detection with an instance segmentation branch.</p></div>
</div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">08 — TRAINING</div>

## Training & Generalization

<div class="cv-revision-grid">
<div class="cv-revision-card">
### Overfitting
<p>Training performance is strong but generalization to unseen data is poor.</p></div>
<div class="cv-revision-card">
### Data Augmentation
<p>Creates varied training examples through controlled transformations.</p></div>
<div class="cv-revision-card">
### Transfer Learning
<p>Starts from a pretrained model and adapts it to the target task.</p></div>
<div class="cv-revision-card">
### Learning Rate
<p>Controls the size of optimization updates during training.</p></div>
</div>
<div class="cv-revision-callout"><strong>Senior interview point:</strong><p>If validation performance is poor, do not immediately change the architecture. Check data quality, labels, preprocessing consistency, class balance, augmentation, leakage and train/validation distribution first.</p></div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">09 — DEPLOYMENT</div>

## Production Metrics

<div class="cv-revision-grid">
<div class="cv-revision-card">
### Latency
<p>Time required to produce an inference result.</p></div>
<div class="cv-revision-card">
### Throughput
<p>Number of images or frames processed in a given period.</p></div>
<div class="cv-revision-card">
### Memory
<p>Model and runtime memory requirements can constrain deployment targets.</p></div>
<div class="cv-revision-card">
### FPS
<p>Frames per second is especially relevant for real-time video applications.</p></div>
</div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">10 — TROUBLESHOOTING</div>

## When a Vision Model Performs Poorly

<div class="cv-revision-checklist">
<div class="cv-revision-check">Verify input dimensions, channel order and normalization.</div>
<div class="cv-revision-check">Inspect training and validation data quality.</div>
<div class="cv-revision-check">Check class imbalance and label correctness.</div>
<div class="cv-revision-check">Compare training and validation distributions.</div>
<div class="cv-revision-check">Inspect false positives and false negatives.</div>
<div class="cv-revision-check">Check whether the evaluation metric matches the real objective.</div>
<div class="cv-revision-check">Measure inference latency and resource usage separately from model accuracy.</div>
</div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">11 — INTERVIEW RAPID FIRE</div>

## Questions You Should Answer in 30 Seconds

<div class="cv-revision-checklist">
<div class="cv-revision-check">What is the difference between classification and object detection?</div>
<div class="cv-revision-check">What is IoU and why is it important?</div>
<div class="cv-revision-check">Why do CNNs use convolution?</div>
<div class="cv-revision-check">What is the difference between precision and recall?</div>
<div class="cv-revision-check">Why is data augmentation useful?</div>
<div class="cv-revision-check">What is transfer learning?</div>
<div class="cv-revision-check">What is Non-Maximum Suppression?</div>
<div class="cv-revision-check">What is the difference between semantic and instance segmentation?</div>
<div class="cv-revision-check">How would you debug a model that works in training but fails in production?</div>
<div class="cv-revision-check">How would you reduce inference latency?</div>
</div>
</section>

<section class="cv-revision-section">
<div class="cv-revision-label">12 — FINAL RECALL</div>

## The Complete Mental Model

<div class="cv-revision-callout">
<strong>Think in this order:</strong>
<p>Problem → Data → Preprocessing → Model → Prediction → Metric → Failure Cases → Deployment → Monitoring.</p>
</div>
</section>
</section>
