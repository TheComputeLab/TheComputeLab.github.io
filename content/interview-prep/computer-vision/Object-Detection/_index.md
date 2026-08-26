---
title: ""
description: "Interview-focused Object Detection preparation covering detection pipelines, bounding boxes, IoU, NMS, YOLO, Faster R-CNN, training, metrics, troubleshooting and deployment."
hideTitle: true
weight: 50
toc: true
cascade:
  type: docs
---
<style>
.cv-det-page{width:100%;max-width:900px;margin:0 auto;padding:8px 0 60px}
.cv-det-hero{position:relative;overflow:hidden;margin-bottom:30px;padding:32px;border:1px solid rgba(96,165,250,.24);border-radius:16px;background:linear-gradient(rgba(96,165,250,.035) 1px,transparent 1px),linear-gradient(90deg,rgba(96,165,250,.035) 1px,transparent 1px),#0d0b0a;background-size:24px 24px}
.cv-det-hero>*{position:relative;z-index:1}
.cv-det-status{display:flex;align-items:center;gap:8px;margin-bottom:18px;color:#7dd3fc;font:700 9px monospace;letter-spacing:.12em}
.cv-det-dot{width:7px;height:7px;border-radius:50%;background:#22c55e;box-shadow:0 0 10px rgba(34,197,94,.7)}
.cv-det-hero h1{margin:0 0 15px;font-size:clamp(38px,6vw,58px);line-height:1;letter-spacing:-.04em}
.cv-det-hero h1 span{color:#60a5fa}
.cv-det-hero p{max-width:740px;margin:0;color:#aaa09a;font-size:13px;line-height:1.75}
.cv-det-terminal{display:inline-flex;align-items:center;gap:8px;margin-top:21px;padding:11px 14px;border:1px solid rgba(96,165,250,.2);border-radius:7px;background:rgba(13,11,10,.78);font:10px monospace}
.cv-det-terminal .prompt{color:#22c55e}
.cv-det-terminal .command{color:#7dd3fc}
.cv-det-section{margin-top:36px}
.cv-det-label{margin-bottom:7px;color:#60a5fa;font:700 9px monospace;letter-spacing:.12em}
.cv-det-section h2{margin:0 0 14px;padding-bottom:10px;border-bottom:1px solid rgba(96,165,250,.15);font-size:24px}
.cv-det-section>p{color:#aaa09a;font-size:13px;line-height:1.75}
.cv-det-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:14px;margin-top:18px}
.cv-det-card{padding:19px;border:1px solid rgba(96,165,250,.14);border-radius:9px;background:rgba(13,11,10,.52)}
.cv-det-card h3{margin:0 0 8px;font-size:15px}
.cv-det-card p{margin:0;color:#aaa09a;font-size:12px;line-height:1.7}
.cv-det-pills{display:flex;flex-wrap:wrap;gap:7px;margin-top:16px}
.cv-det-pill{padding:6px 9px;border:1px solid rgba(96,165,250,.17);border-radius:5px;color:#c8b7aa;background:rgba(96,165,250,.035);font:9px monospace}
.cv-det-table-wrap{margin-top:18px;overflow-x:auto;border:1px solid rgba(96,165,250,.13);border-radius:9px}
.cv-det-table{width:100%;min-width:650px;border-collapse:collapse}
.cv-det-table th,.cv-det-table td{padding:11px 13px;border-bottom:1px solid rgba(96,165,250,.09);text-align:left;font-size:11px;line-height:1.55}
.cv-det-table th{color:#7dd3fc;background:rgba(96,165,250,.045);font-family:monospace}
.cv-det-table td{color:#aaa09a}
.cv-det-table tr:last-child td{border-bottom:0}
.cv-det-callout{margin-top:18px;padding:16px 18px;border-left:2px solid #60a5fa;background:rgba(96,165,250,.045)}
.cv-det-callout strong{color:#f0e7e1;font-size:12px}
.cv-det-callout p{margin:6px 0 0;color:#aaa09a;font-size:12px;line-height:1.7}
.cv-det-code{margin:18px 0;padding:18px;overflow-x:auto;border:1px solid rgba(96,165,250,.14);border-radius:9px;background:#080706}
.cv-det-code pre{margin:0;color:#c8bdb5;font:11px/1.7 monospace}
.cv-det-checklist{display:grid;gap:8px;margin-top:16px}
.cv-det-check{padding:11px 14px;border:1px solid rgba(96,165,250,.11);border-radius:6px;color:#c0b4ac;background:rgba(13,11,10,.45);font:11px/1.55 monospace}
.cv-det-check::before{content:"✓";margin-right:9px;color:#22c55e}
@media(max-width:700px){.cv-det-hero{padding:24px 20px}.cv-det-grid{grid-template-columns:1fr}.cv-det-table{min-width:600px}}
</style>
<section class="cv-det-page">
<section class="cv-det-hero">
<div class="cv-det-status"><span class="cv-det-dot"></span>COMPUTER VISION • OBJECT DETECTION</div>
<h1>Object <span>Detection</span></h1>
<p>Interview-focused preparation for designing, training, evaluating and deploying object detection systems. Learn bounding boxes, IoU, confidence scores, NMS, YOLO, two-stage detectors, metrics, training strategy and production troubleshooting.</p>
<div class="cv-det-terminal"><span class="prompt">$</span><span class="command">build_detection_pipeline()</span></div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">01 — FUNDAMENTALS</div>

## What Is Object Detection?
<p>Object detection identifies objects in an image and predicts both what the objects are and where they are located. A typical detection result contains a class, confidence score and bounding box.</p>
<div class="cv-det-grid">
<div class="cv-det-card"><h3>Classification</h3><p>Answers what is present in the image or crop.</p></div>
<div class="cv-det-card"><h3>Localization</h3><p>Estimates where an object is, commonly with a bounding box.</p></div>
<div class="cv-det-card"><h3>Detection</h3><p>Combines object classification with localization for potentially multiple objects.</p></div>
<div class="cv-det-card"><h3>Confidence</h3><p>Represents the model's score associated with a predicted detection.</p></div>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">02 — PIPELINE</div>

## Object Detection Pipeline
<div class="cv-det-pills"><span class="cv-det-pill">Image</span><span class="cv-det-pill">Resize</span><span class="cv-det-pill">Normalize</span><span class="cv-det-pill">Feature Extraction</span><span class="cv-det-pill">Prediction</span><span class="cv-det-pill">Confidence Filter</span><span class="cv-det-pill">NMS</span><span class="cv-det-pill">Output</span></div>
<div class="cv-det-callout"><strong>Senior interview approach</strong><p>Define the target objects, required accuracy, acceptable false positives and false negatives, image resolution, latency target and deployment hardware before choosing a detector.</p></div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">03 — BOUNDING BOXES</div>

## Bounding Boxes
<p>A bounding box represents the approximate spatial extent of a detected object. It can be represented using coordinates such as x1, y1, x2, y2 or center coordinates plus width and height.</p>
<div class="cv-det-grid">
<div class="cv-det-card"><h3>XYXY</h3><p>Top-left and bottom-right coordinates: x1, y1, x2, y2.</p></div>
<div class="cv-det-card"><h3>XYWH</h3><p>Center or origin coordinates plus width and height, depending on the convention.</p></div>
<div class="cv-det-card"><h3>Normalized Boxes</h3><p>Coordinates represented relative to image dimensions, often between 0 and 1.</p></div>
<div class="cv-det-card"><h3>Clipping</h3><p>Predicted boxes should be constrained to valid image boundaries when required.</p></div>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">04 — IOU</div>
## Intersection over Union
<p>IoU measures the overlap between a predicted bounding box and a reference bounding box.</p>
<div class="cv-det-callout"><strong>Formula</strong><p>IoU = Area of Intersection ÷ Area of Union.</p></div>
<div class="cv-det-table-wrap">
<table class="cv-det-table">
<thead><tr><th>IoU behavior</th><th>Interpretation</th></tr></thead>
<tbody>
<tr><td>0</td><td>No overlap.</td></tr>
<tr><td>Between 0 and 1</td><td>Partial overlap.</td></tr>
<tr><td>1</td><td>Perfect overlap.</td></tr>
</tbody>
</table>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">05 — NMS</div>

## Non-Maximum Suppression
<p>Object detectors can produce multiple overlapping boxes for the same object. Non-Maximum Suppression keeps strong detections and suppresses highly overlapping lower-scoring detections.</p>
<div class="cv-det-grid">
<div class="cv-det-card"><h3>Step 1</h3><p>Sort candidate boxes by confidence score.</p></div>
<div class="cv-det-card"><h3>Step 2</h3><p>Select the highest-confidence box.</p></div>
<div class="cv-det-card"><h3>Step 3</h3><p>Compare remaining boxes using IoU.</p></div>
<div class="cv-det-card"><h3>Step 4</h3><p>Suppress boxes whose overlap exceeds the selected threshold and repeat.</p></div>
</div>
<div class="cv-det-callout"><strong>Interview point</strong><p>The NMS threshold is a post-processing decision. It can affect duplicate detections and how overlapping objects are retained.</p></div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">06 — DETECTOR TYPES</div>
## One-Stage vs Two-Stage Detectors
<div class="cv-det-table-wrap">
<table class="cv-det-table">
<thead><tr><th>Type</th><th>Approach</th><th>Typical trade-off</th></tr></thead>
<tbody>
<tr><td>One-Stage</td><td>Predict detections more directly from the feature representation.</td><td>Often attractive when inference speed is important.</td></tr>
<tr><td>Two-Stage</td><td>Generate candidate regions and then refine/classify them.</td><td>Can provide strong accuracy at the cost of additional computation.</td></tr>
</tbody>
</table>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">07 — YOLO</div>

## YOLO Family
<p>YOLO-style detectors are designed around efficient object detection and are widely used when real-time or near-real-time inference is important.</p>
<div class="cv-det-pills"><span class="cv-det-pill">Single-Stage</span><span class="cv-det-pill">Real-Time</span><span class="cv-det-pill">Bounding Boxes</span><span class="cv-det-pill">Confidence</span><span class="cv-det-pill">NMS / Post-Processing</span></div>
<div class="cv-det-callout"><strong>Interview answer</strong><p>When comparing YOLO-style systems, focus on the specific model version and implementation rather than treating the entire YOLO family as one fixed architecture.</p></div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">08 — TWO-STAGE DETECTION</div>
## Faster R-CNN
<p>Faster R-CNN is a two-stage detection architecture. A region proposal mechanism generates candidate regions and subsequent stages classify and refine those regions.</p>
<div class="cv-det-grid">
<div class="cv-det-card"><h3>Backbone</h3><p>Extracts visual feature representations from the input image.</p></div>
<div class="cv-det-card"><h3>Region Proposal</h3><p>Generates candidate object regions from learned feature maps.</p></div>
<div class="cv-det-card"><h3>ROI Features</h3><p>Extracts fixed or aligned representations for candidate regions.</p></div>
<div class="cv-det-card"><h3>Detection Head</h3><p>Predicts object classes and refines bounding-box coordinates.</p></div>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">09 — ANCHORS & BOXES</div>

## Anchors and Anchor-Free Detection
<div class="cv-det-grid">
<div class="cv-det-card"><h3>Anchor-Based</h3><p>Uses predefined reference boxes at different locations and scales and predicts adjustments to them.</p></div>
<div class="cv-det-card"><h3>Anchor-Free</h3><p>Predicts object properties without depending on a predefined anchor-box set.</p></div>
<div class="cv-det-card"><h3>Box Regression</h3><p>Learns how predicted coordinates should move toward the target object box.</p></div>
<div class="cv-det-card"><h3>Class Prediction</h3><p>Estimates which object category is associated with a detected region.</p></div>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">10 — TRAINING</div>

## Training an Object Detector
<div class="cv-det-pills"><span class="cv-det-pill">Images</span><span class="cv-det-pill">Annotations</span><span class="cv-det-pill">Augmentation</span><span class="cv-det-pill">Backbone</span><span class="cv-det-pill">Classification Loss</span><span class="cv-det-pill">Box Loss</span><span class="cv-det-pill">Validation</span></div>
<div class="cv-det-callout"><strong>Important</strong><p>Detection training usually involves multiple learning objectives, such as class prediction and localization. The exact loss design depends on the detector architecture.</p></div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">11 — DATASET</div>

## Detection Dataset Design
<div class="cv-det-grid">
<div class="cv-det-card"><h3>Bounding Box Quality</h3><p>Annotations should tightly and consistently represent the intended objects.</p></div>
<div class="cv-det-card"><h3>Class Coverage</h3><p>Include relevant object sizes, viewpoints, environments and difficult cases.</p></div>
<div class="cv-det-card"><h3>Small Objects</h3><p>Ensure the dataset contains enough examples at the sizes expected during deployment.</p></div>
<div class="cv-det-card"><h3>Negative Images</h3><p>Images without target objects can help the detector learn what should not trigger a detection.</p></div>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">12 — AUGMENTATION</div>

## Detection Augmentation
<div class="cv-det-table-wrap">
<table class="cv-det-table">
<thead><tr><th>Augmentation</th><th>Purpose</th><th>Important consideration</th></tr></thead>
<tbody>
<tr><td>Horizontal Flip</td><td>Adds viewpoint variation.</td><td>Only valid when object orientation and labels remain meaningful.</td></tr>
<tr><td>Scale</td><td>Improves robustness to object size variation.</td><td>Bounding boxes must be transformed consistently.</td></tr>
<tr><td>Crop</td><td>Creates different spatial contexts.</td><td>Do not accidentally remove or corrupt target annotations.</td></tr>
<tr><td>Color Changes</td><td>Improves robustness to appearance variation.</td><td>Should represent realistic production variation.</td></tr>
</tbody>
</table>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">13 — METRICS</div>

## Detection Evaluation
<div class="cv-det-grid">
<div class="cv-det-card"><h3>IoU</h3><p>Measures spatial overlap between predicted and reference boxes.</p></div>
<div class="cv-det-card"><h3>Precision</h3><p>Measures how many predicted detections are correct.</p></div>
<div class="cv-det-card"><h3>Recall</h3><p>Measures how many relevant objects are successfully detected.</p></div>
<div class="cv-det-card"><h3>mAP</h3><p>Summarizes average precision across classes under defined evaluation conditions.</p></div>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">14 — PRECISION & RECALL</div>

## Detection Error Analysis
<div class="cv-det-table-wrap">
<table class="cv-det-table">
<thead><tr><th>Problem</th><th>Possible direction</th></tr></thead>
<tbody>
<tr><td>Too many false positives</td><td>Inspect confidence threshold, negative examples, labels and class confusion.</td></tr>
<tr><td>Too many false negatives</td><td>Inspect confidence threshold, small objects, difficult examples and recall behavior.</td></tr>
<tr><td>Duplicate detections</td><td>Inspect NMS or other duplicate-removal logic.</td></tr>
<tr><td>Poor localization</td><td>Inspect annotation quality, box regression and object scale.</td></tr>
</tbody>
</table>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">15 — TROUBLESHOOTING</div>

## When Detection Performance Is Poor
<div class="cv-det-checklist">
<div class="cv-det-check">Inspect annotation quality and class definitions.</div>
<div class="cv-det-check">Check train and validation distribution differences.</div>
<div class="cv-det-check">Inspect performance by object size.</div>
<div class="cv-det-check">Review confidence thresholds and NMS settings.</div>
<div class="cv-det-check">Inspect false positives and false negatives visually.</div>
<div class="cv-det-check">Check image resizing and preprocessing consistency.</div>
<div class="cv-det-check">Verify that production camera conditions are represented in training data.</div>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">16 — PYTORCH CONCEPT</div>

## Detection Inference Flow
<div class="cv-det-code">
<pre><code>model.eval()
with torch.no_grad():
    predictions = model(images)
for prediction in predictions:
    boxes = prediction["boxes"]
    scores = prediction["scores"]
    labels = prediction["labels"]</code></pre>
</div>
<div class="cv-det-callout"><strong>Interview explanation</strong><p>Inference produces candidate boxes, scores and class labels. Thresholding and duplicate-removal logic may then be applied depending on the model and inference framework.</p></div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">17 — DEPLOYMENT</div>

## Production Object Detection
<div class="cv-det-pills"><span class="cv-det-pill">Camera</span><span class="cv-det-pill">Preprocessing</span><span class="cv-det-pill">Inference</span><span class="cv-det-pill">Post-Processing</span><span class="cv-det-pill">Latency</span><span class="cv-det-pill">Throughput</span><span class="cv-det-pill">GPU / CPU</span><span class="cv-det-pill">Monitoring</span></div>
<div class="cv-det-callout"><strong>Senior interview point</strong><p>Optimize the entire pipeline, not just model inference. Camera capture, decoding, preprocessing, model execution, NMS and result handling can all contribute to end-to-end latency.</p></div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">18 — REAL-TIME SYSTEM</div>

## Designing a Real-Time Detector
<div class="cv-det-grid">
<div class="cv-det-card"><h3>Requirement</h3><p>Define FPS, latency, accuracy and hardware constraints.</p></div>
<div class="cv-det-card"><h3>Model</h3><p>Select an architecture appropriate for the accuracy and speed target.</p></div>
<div class="cv-det-card"><h3>Optimization</h3><p>Consider input resolution, runtime optimization, batching strategy and quantization where appropriate.</p></div>
<div class="cv-det-card"><h3>Monitoring</h3><p>Track latency, throughput, confidence behavior and real-world detection quality.</p></div>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">19 — INTERVIEW RAPID FIRE</div>

## Object Detection Questions
<div class="cv-det-checklist">
<div class="cv-det-check">Classification vs object detection?</div>
<div class="cv-det-check">What is IoU?</div>
<div class="cv-det-check">What is NMS and why is it needed?</div>
<div class="cv-det-check">One-stage vs two-stage detectors?</div>
<div class="cv-det-check">How does YOLO differ conceptually from Faster R-CNN?</div>
<div class="cv-det-check">What is mAP?</div>
<div class="cv-det-check">How do you handle small objects?</div>
<div class="cv-det-check">How do you handle class imbalance?</div>
<div class="cv-det-check">Why might a detector have high precision but low recall?</div>
<div class="cv-det-check">How would you debug duplicate detections?</div>
<div class="cv-det-check">How would you reduce detector latency?</div>
<div class="cv-det-check">How would you design a production real-time detection system?</div>
</div>
</section>
<section class="cv-det-section">
<div class="cv-det-label">20 — FINAL RECALL</div>

## Detection Mental Model
<div class="cv-det-callout"><strong>Think in this order:</strong><p>Problem → Classes → Annotations → Preprocessing → Detector → Classification + Localization → Confidence → NMS → Metrics → Error Analysis → Optimization → Deployment → Monitoring.</p></div>
</section>
</section>
