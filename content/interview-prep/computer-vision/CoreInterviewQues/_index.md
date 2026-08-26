---
title: ""
description: "Core Computer Vision interview questions with concise answers, explanations and practical interview angles."
weight: 20
toc: true
cascade:
  type: docs
---
<style>
.cv-core-page{width:100%;max-width:980px;margin:0 auto;padding:8px 0 60px}
.cv-core-hero{position:relative;overflow:hidden;margin-bottom:30px;padding:30px 32px;border:1px solid rgba(96,165,250,.24);border-radius:16px;background:linear-gradient(rgba(96,165,250,.045) 1px,transparent 1px),linear-gradient(90deg,rgba(96,165,250,.045) 1px,transparent 1px),#07111d;background-size:24px 24px}
.cv-core-hero>*{position:relative;z-index:1}
.cv-core-status{display:flex;align-items:center;gap:8px;margin-bottom:16px;color:#7dd3fc;font:700 9px monospace;letter-spacing:.12em}
.cv-core-dot{width:7px;height:7px;border-radius:50%;background:#22c55e;box-shadow:0 0 10px rgba(34,197,94,.65)}
.cv-core-hero h1{margin:0 0 14px;font-size:clamp(38px,6vw,58px);line-height:1;letter-spacing:-.04em}
.cv-core-hero h1 span{color:#60a5fa}
.cv-core-hero p{max-width:760px;margin:0;color:#9aa9bb;font-size:13px;line-height:1.75}
.cv-core-terminal{display:inline-flex;align-items:center;gap:8px;margin-top:20px;padding:10px 14px;border:1px solid rgba(96,165,250,.18);border-radius:6px;background:rgba(7,17,29,.75);font:10px monospace}
.cv-core-terminal .prompt{color:#22c55e}
.cv-core-terminal .command{color:#93c5fd}
.cv-core-section{margin-top:36px}
.cv-core-label{margin-bottom:7px;color:#60a5fa;font:700 9px monospace;letter-spacing:.12em}
.cv-core-section h2{margin:0 0 16px;padding-bottom:9px;border-bottom:1px solid rgba(96,165,250,.15);font-size:24px}
.cv-core-question{margin:16px 0;padding:18px 20px;border:1px solid rgba(96,165,250,.14);border-radius:9px;background:rgba(7,17,29,.48)}
.cv-core-question h3{margin:0 0 12px;color:#e2e8f0;font-size:15px;line-height:1.5}
.cv-core-answer{margin:0;color:#b8c5d4;font-size:12px;line-height:1.75}
.cv-core-answer strong{color:#7dd3fc}
.cv-core-explain{margin:11px 0 0;padding:11px 13px;border-left:2px solid rgba(96,165,250,.55);background:rgba(96,165,250,.035);color:#929faf;font-size:11px;line-height:1.7}
.cv-core-explain strong{color:#cbd5e1}
.cv-core-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:14px}
.cv-core-card{padding:17px;border:1px solid rgba(96,165,250,.13);border-radius:9px;background:rgba(7,17,29,.42)}
.cv-core-card h3{margin:0 0 8px;font-size:14px}
.cv-core-card p{margin:0;color:#9aa9bb;font-size:12px;line-height:1.7}
.cv-core-table-wrap{margin-top:16px;overflow-x:auto;border:1px solid rgba(96,165,250,.13);border-radius:9px}
.cv-core-table{width:100%;min-width:650px;border-collapse:collapse}
.cv-core-table th,.cv-core-table td{padding:11px 13px;border-bottom:1px solid rgba(96,165,250,.09);text-align:left;font-size:11px;line-height:1.55}
.cv-core-table th{color:#7dd3fc;background:rgba(96,165,250,.04);font-family:monospace}
.cv-core-table td{color:#9aa9bb}
.cv-core-table tr:last-child td{border-bottom:0}
.cv-core-callout{margin-top:16px;padding:15px 17px;border-left:2px solid #60a5fa;background:rgba(96,165,250,.045)}
.cv-core-callout strong{color:#e2e8f0;font-size:12px}
.cv-core-callout p{margin:6px 0 0;color:#9aa9bb;font-size:12px;line-height:1.7}
.cv-core-list{display:grid;gap:8px;margin:15px 0 0;padding:0;list-style:none}
.cv-core-list li{padding:10px 13px;border:1px solid rgba(96,165,250,.1);border-radius:6px;color:#aebdcd;background:rgba(7,17,29,.4);font:11px/1.6 monospace}
.cv-core-list li::before{content:"✓";margin-right:9px;color:#22c55e}
@media(max-width:700px){.cv-core-hero{padding:24px 20px}.cv-core-grid{grid-template-columns:1fr}.cv-core-table{min-width:600px}}
</style>
<section class="cv-core-page">
<section class="cv-core-hero">
<div class="cv-core-status"><span class="cv-core-dot"></span>COMPUTER VISION • CORE INTERVIEW QUESTIONS</div>
<h1>Computer Vision <span>Core Interview Questions</span></h1>
<p>Build strong answers to the questions that appear repeatedly in Computer Vision interviews. Focus on clear definitions, practical reasoning, trade-offs and the ability to connect algorithms to a real production pipeline.</p>
<div class="cv-core-terminal"><span class="prompt">$</span><span class="command">load_core_interview_questions()</span></div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">01 — FUNDAMENTALS</div>

## Computer Vision Basics

<div class="cv-core-question">

### 1. What is Computer Vision?

<p class="cv-core-answer"><strong>Answer:</strong> Computer Vision is the field of building systems that extract information from images or video and use that information for tasks such as classification, detection, segmentation, measurement or decision-making.</p>
<div class="cv-core-explain"><strong>Interview angle:</strong> Explain the problem first, then describe how pixels are transformed into useful information.</div>
</div>
<div class="cv-core-question">

### 2. What is the difference between an image and a feature?

<p class="cv-core-answer"><strong>Answer:</strong> An image is the raw visual input, while a feature is a representation or pattern extracted from that input that can help a model make predictions.</p>
<div class="cv-core-explain"><strong>Interview angle:</strong> Traditional systems often use engineered features; deep learning models learn representations automatically.</div>
</div>
<div class="cv-core-question">

### 3. What is a pixel?

<p class="cv-core-answer"><strong>Answer:</strong> A pixel is an individual spatial element of an image containing an intensity or color value.</p>
</div>
<div class="cv-core-question">

### 4. What do image height, width and channels represent?

<p class="cv-core-answer"><strong>Answer:</strong> Height and width describe spatial dimensions. Channels represent separate information components, such as one grayscale channel or three RGB color channels.</p>
<div class="cv-core-explain"><strong>Example:</strong> 224 × 224 × 3 represents height 224, width 224 and three color channels.</div>
</div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">02 — IMAGE PROCESSING</div>

## Image Processing Questions

<div class="cv-core-question">

### 5. Why do we resize images before inference?

<p class="cv-core-answer"><strong>Answer:</strong> Models are commonly trained for a fixed or constrained input size. Resizing creates a consistent input representation and controls computational cost.</p>
<div class="cv-core-explain"><strong>Trade-off:</strong> Aggressive resizing can remove important spatial information or distort object proportions.</div>
</div>
<div class="cv-core-question">

### 6. What is image normalization?

<p class="cv-core-answer"><strong>Answer:</strong> Normalization transforms pixel values into a numerical range or distribution suitable for a model, improving consistency and often helping optimization.</p>
</div>
<div class="cv-core-question">

### 7. What is image augmentation and why is it useful?

<p class="cv-core-answer"><strong>Answer:</strong> Augmentation creates varied training examples through transformations such as flips, crops, rotations or controlled appearance changes. It can improve generalization and reduce overfitting.</p>
<div class="cv-core-explain"><strong>Important:</strong> Augmentations must preserve the meaning of the task. A transformation that changes the label is not automatically valid.</div>
</div>
<div class="cv-core-question">

### 8. What is the difference between RGB and BGR?

<p class="cv-core-answer"><strong>Answer:</strong> Both represent the same three color components but use different channel ordering. OpenCV commonly represents loaded color images as BGR, while many other libraries and models use RGB.</p>
</div>
<div class="cv-core-question">

### 9. What is the difference between Gaussian blur and median filtering?

<p class="cv-core-answer"><strong>Answer:</strong> Gaussian blur uses a weighted neighborhood average and is useful for smoothing. Median filtering replaces a value using the neighborhood median and is particularly useful for impulse or salt-and-pepper noise.</p>
</div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">03 — OPENCV</div>

## OpenCV Interview Questions

<div class="cv-core-question">

### 10. What is OpenCV?

<p class="cv-core-answer"><strong>Answer:</strong> OpenCV is an open-source computer vision library providing tools for image and video processing, feature extraction, geometric operations, object detection and other vision tasks.</p>
</div>
<div class="cv-core-question">

### 11. How do you read an image using OpenCV?

<p class="cv-core-answer"><strong>Answer:</strong> Use <code>cv2.imread()</code> to load an image from disk and then verify that the returned image is valid before processing it.</p>
</div>
<div class="cv-core-question">

### 12. How do you convert a color image to grayscale?

<p class="cv-core-answer"><strong>Answer:</strong> Use <code>cv2.cvtColor()</code> with the appropriate color-conversion code, such as <code>cv2.COLOR_BGR2GRAY</code> for a typical OpenCV-loaded color image.</p>
</div>
<div class="cv-core-question">

### 13. What are contours?

<p class="cv-core-answer"><strong>Answer:</strong> Contours are curves representing boundaries of connected regions in an image. They can be useful for shape analysis, object measurement and geometric processing.</p>
</div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">04 — CNNs</div>

## Convolutional Neural Network Questions

<div class="cv-core-question">

### 14. Why are CNNs effective for images?

<p class="cv-core-answer"><strong>Answer:</strong> CNNs exploit local spatial structure through shared convolutional filters. Early layers can learn simple patterns while deeper layers combine them into increasingly complex representations.</p>
</div>
<div class="cv-core-question">

### 15. What is a convolution kernel?

<p class="cv-core-answer"><strong>Answer:</strong> A kernel is a set of learnable weights that slides across an input and computes responses that form a feature map.</p>
</div>
<div class="cv-core-question">

### 16. What is stride?

<p class="cv-core-answer"><strong>Answer:</strong> Stride is the number of spatial positions a convolution filter moves at each step. Larger stride generally reduces the spatial dimensions of the output.</p>
</div>
<div class="cv-core-question">

### 17. What is padding?

<p class="cv-core-answer"><strong>Answer:</strong> Padding adds values around the input boundary. It can help preserve spatial dimensions and allow edge information to participate in convolution.</p>
</div>
<div class="cv-core-question">

### 18. What is pooling?

<p class="cv-core-answer"><strong>Answer:</strong> Pooling reduces spatial dimensions by aggregating local values, such as with max pooling or average pooling.</p>
</div>
<div class="cv-core-question">

### 19. What is a feature map?

<p class="cv-core-answer"><strong>Answer:</strong> A feature map is an activation representation produced by applying learned filters to an input or previous feature map.</p>
</div>
<div class="cv-core-callout"><strong>Senior answer:</strong><p>When discussing CNNs, connect architecture choices to the problem: receptive field, spatial resolution, computational cost, robustness and the information required by the downstream task.</p></div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">05 — CLASSIFICATION</div>

## Image Classification Questions

<div class="cv-core-question">

### 20. What is image classification?

<p class="cv-core-answer"><strong>Answer:</strong> Image classification assigns one or more labels to an image based on its visual content.</p>
</div>
<div class="cv-core-question">

### 21. What is the difference between binary and multiclass classification?

<p class="cv-core-answer"><strong>Answer:</strong> Binary classification has two target classes. Multiclass classification has more than two mutually exclusive classes, where an input generally belongs to one class.</p>
</div>
<div class="cv-core-question">

### 22. What is multilabel classification?

<p class="cv-core-answer"><strong>Answer:</strong> Multilabel classification allows multiple labels to be true for the same image.</p>
</div>
<div class="cv-core-question">

### 23. Why can accuracy be misleading?

<p class="cv-core-answer"><strong>Answer:</strong> With imbalanced classes, a model can achieve high accuracy while performing poorly on the minority class. Precision, recall, F1 or class-specific metrics may provide a better view.</p>
</div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">06 — DETECTION</div>

## Object Detection Questions

<div class="cv-core-question">

### 24. What is object detection?

<p class="cv-core-answer"><strong>Answer:</strong> Object detection identifies objects in an image and estimates their locations, commonly using bounding boxes, together with class predictions and confidence scores.</p>
</div>
<div class="cv-core-question">

### 25. What is IoU?

<p class="cv-core-answer"><strong>Answer:</strong> Intersection over Union measures the overlap between a predicted region and a reference region: intersection area divided by union area.</p>
</div>
<div class="cv-core-question">

### 26. What is Non-Maximum Suppression?

<p class="cv-core-answer"><strong>Answer:</strong> NMS reduces duplicate detections by keeping strong candidates and suppressing highly overlapping lower-scoring boxes.</p>
</div>
<div class="cv-core-question">

### 27. What is the difference between one-stage and two-stage detectors?

<p class="cv-core-answer"><strong>Answer:</strong> One-stage detectors perform detection in a more direct pipeline and are often favored for speed. Two-stage detectors typically generate candidate regions and then refine/classify them, often trading additional computation for strong detection performance.</p>
</div>
<div class="cv-core-question">

### 28. What is mAP?

<p class="cv-core-answer"><strong>Answer:</strong> Mean Average Precision summarizes precision-recall performance for object detection across classes and specified evaluation conditions.</p>
</div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">07 — SEGMENTATION</div>

## Image Segmentation Questions

<div class="cv-core-question">

### 29. What is image segmentation?

<p class="cv-core-answer"><strong>Answer:</strong> Segmentation assigns labels or masks at the pixel level, providing more detailed spatial information than a simple image-level classification.</p>
</div>
<div class="cv-core-question">

### 30. What is semantic segmentation?

<p class="cv-core-answer"><strong>Answer:</strong> Semantic segmentation assigns a class to each pixel but does not necessarily distinguish separate instances of the same class.</p>
</div>
<div class="cv-core-question">

### 31. What is instance segmentation?

<p class="cv-core-answer"><strong>Answer:</strong> Instance segmentation identifies individual object instances and produces a separate mask for each instance.</p>
</div>
<div class="cv-core-question">

### 32. What is U-Net?

<p class="cv-core-answer"><strong>Answer:</strong> U-Net is an encoder-decoder architecture with skip connections designed to combine high-level context with spatial detail, widely used for segmentation.</p>
</div>
<div class="cv-core-question">

### 33. What is Dice score?

<p class="cv-core-answer"><strong>Answer:</strong> Dice measures overlap between predicted and reference regions and is commonly used to evaluate segmentation masks.</p>
</div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">08 — DATA & TRAINING</div>

## Training and Generalization Questions

<div class="cv-core-question">

### 34. What is overfitting?

<p class="cv-core-answer"><strong>Answer:</strong> Overfitting occurs when a model learns the training data too specifically and performs poorly on unseen data.</p>
<div class="cv-core-explain"><strong>Possible responses:</strong> Improve data quality, add appropriate augmentation, regularize, simplify the model, use transfer learning or adjust the training process.</div>
</div>
<div class="cv-core-question">

### 35. What is data leakage?

<p class="cv-core-answer"><strong>Answer:</strong> Data leakage occurs when information from outside the intended training data becomes available to the model during training or evaluation, producing overly optimistic results.</p>
</div>
<div class="cv-core-question">

### 36. Why split data into training, validation and test sets?

<p class="cv-core-answer"><strong>Answer:</strong> Training data is used to learn parameters, validation data supports model and hyperparameter decisions, and the test set provides a final estimate on unseen data.</p>
</div>
<div class="cv-core-question">

### 37. What is transfer learning?

<p class="cv-core-answer"><strong>Answer:</strong> Transfer learning starts with knowledge learned from a pretrained model and adapts it to a target task, often reducing training requirements when the target dataset is limited.</p>
</div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">09 — METRICS</div>

## Evaluation Questions

<div class="cv-core-table-wrap">
<table class="cv-core-table">
<thead><tr><th>Question</th><th>Core answer</th></tr></thead>
<tbody>
<tr><td>Precision?</td><td>Among predicted positives, how many are actually positive?</td></tr>
<tr><td>Recall?</td><td>Among actual positives, how many did the model find?</td></tr>
<tr><td>F1?</td><td>Harmonic mean of precision and recall.</td></tr>
<tr><td>IoU?</td><td>Overlap between predicted and reference regions divided by their union.</td></tr>
<tr><td>Accuracy?</td><td>Fraction of all predictions that are correct.</td></tr>
<tr><td>Confusion matrix?</td><td>Shows class-level counts such as true positives, false positives, true negatives and false negatives.</td></tr>
</tbody>
</table>
</div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">10 — TROUBLESHOOTING</div>

## Practical Interview Scenarios

<div class="cv-core-question">

### 38. Your model has high training accuracy but poor validation accuracy. What do you check?

<p class="cv-core-answer"><strong>Answer:</strong> Start with overfitting, then inspect data quality, labels, class balance, augmentation, preprocessing consistency and train/validation distribution. Also verify there is no leakage or accidental difference in the inference pipeline.</p>
</div>
<div class="cv-core-question">

### 39. Your model works in a notebook but fails in production. What do you investigate?

<p class="cv-core-answer"><strong>Answer:</strong> Compare the complete production input and preprocessing pipeline with training. Check image format, channel order, resize method, normalization, model version, dependency versions, device behavior and post-processing.</p>
</div>
<div class="cv-core-question">

### 40. Detection accuracy is good but inference is too slow. What can you do?

<p class="cv-core-answer"><strong>Answer:</strong> Profile the complete pipeline first. Then consider a smaller model, lower input resolution, optimized inference runtime, batching where appropriate, quantization, hardware acceleration or reducing unnecessary preprocessing and post-processing.</p>
</div>
<div class="cv-core-question">

### 41. False positives are too high. What would you change?

<p class="cv-core-answer"><strong>Answer:</strong> Inspect false-positive examples, confidence distributions and class-specific errors. Depending on the task, adjust decision thresholds, improve negative examples, improve labels, retrain with representative data or modify the model.</p>
</div>
<div class="cv-core-callout"><strong>Senior interview rule:</strong><p>Do not jump directly to changing the model. First establish whether the failure originates in data, preprocessing, model behavior, post-processing, infrastructure or the evaluation method.</p></div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">11 — SYSTEM DESIGN</div>

## Design a Real-Time Vision System

<div class="cv-core-question">

### 42. How would you design a real-time object detection system?

<p class="cv-core-answer"><strong>Answer:</strong> Define the latency and accuracy requirements first. Capture frames, preprocess consistently, run an appropriately sized detection model, apply post-processing, return results and monitor latency, throughput and model quality.</p>
<div class="cv-core-explain"><strong>Senior angle:</strong> Discuss hardware, frame sampling, batching, model optimization, failure handling, observability and how performance changes as the data distribution changes.</div>
</div>
<div class="cv-core-grid">
<div class="cv-core-card">
### Input
<p>Camera, video stream or uploaded images.</p></div>
<div class="cv-core-card">
### Preprocessing
<p>Resize, color conversion, normalization and task-specific preparation.</p></div>
<div class="cv-core-card">
### Inference
<p>Optimized model running on CPU, GPU or edge accelerator.</p></div>
<div class="cv-core-card">
### Post-processing
<p>Thresholding, NMS, masks or business-specific decision logic.</p></div>
</div>
</section>

<section class="cv-core-section">
<div class="cv-core-label">12 — RAPID FIRE</div>

## Questions to Practice Out Loud

<ul class="cv-core-list">
<li>Why are CNNs useful for image data?</li>
<li>What happens when you increase convolution stride?</li>
<li>Why is padding used?</li>
<li>What is the difference between RGB and BGR?</li>
<li>Why can accuracy be a bad metric?</li>
<li>What is IoU?</li>
<li>Why do we use NMS?</li>
<li>Classification vs detection vs segmentation?</li>
<li>Semantic vs instance segmentation?</li>
<li>What is transfer learning?</li>
<li>What causes overfitting?</li>
<li>How would you debug a production vision model?</li>
<li>How would you reduce inference latency?</li>
<li>Which metric would you choose for an imbalanced problem and why?</li>
<li>How would you design a real-time Computer Vision pipeline?</li>
</ul>
</section>

<section class="cv-core-section">
<div class="cv-core-label">13 — ANSWER FRAMEWORK</div>

## How to Give a Strong Interview Answer

<div class="cv-core-callout"><strong>Use this four-step pattern:</strong><p>Define the concept → explain how it works → give a practical example → mention a trade-off, limitation or failure case.</p></div>
<ul class="cv-core-list">
<li>Start with a precise one or two sentence definition.</li>
<li>Explain the mechanism only to the depth the interviewer needs.</li>
<li>Connect the concept to a real Computer Vision pipeline.</li>
<li>For senior roles, discuss trade-offs and operational consequences.</li>
</ul>
</section>
</section>
