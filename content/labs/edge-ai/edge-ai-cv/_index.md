---
title: "Edge Computer Vision"
description: "Explore computer vision on edge devices including image classification, object detection, segmentation, tracking, real-time inference and vision pipelines."
weight: 40
toc: true
---

> **Edge Computer Vision brings visual intelligence closer to where images and video are produced, enabling real-time perception with lower latency, reduced bandwidth and greater autonomy.**

A traditional computer vision system may send images or video to a remote server for processing.

Edge Computer Vision moves some or all of that processing directly onto the device.

The basic architecture is:

```text
Camera / Sensor
       ↓
Image Capture
       ↓
Pre-processing
       ↓
AI Model
       ↓
Inference
       ↓
Post-processing
       ↓
Decision / Action
```

This approach is particularly useful when decisions need to happen quickly and continuously.

Why Computer Vision at the Edge?

Vision applications often generate large amounts of data.

A camera can continuously produce:

```text
Images
   +
Video Frames
   +
Metadata
```
Sending everything to the cloud can introduce:
```text
Network latency
Bandwidth requirements
Connectivity dependency
Cloud processing costs
Privacy concerns
```
Edge processing can reduce the amount of information that needs to leave the device.
```text
Camera
   ↓
Edge Device
   ↓
Visual Understanding
   ↓
Decision
```
Instead of:
```text
Camera
   ↓
Network
   ↓
Cloud
   ↓
AI Processing
   ↓
Network
   ↓
Device


                Edge Vision Pipeline
```
A practical vision system usually contains several stages.
```text
IMAGE / VIDEO
      ↓
CAPTURE
      ↓
PRE-PROCESSING
      ↓
MODEL INFERENCE
      ↓
POST-PROCESSING
      ↓
DECISION
      ↓
ACTION
```
Each stage can affect overall system performance.

Optimizing only the AI model is therefore not always enough.

## Image Capture
```text
The first stage is acquiring visual data.
```
Common sources include:
```text
Cameras
USB cameras
Industrial cameras
IP cameras
Mobile cameras
Embedded camera modules
Thermal cameras
Depth cameras
```
The captured data may be:
```text
RGB
Grayscale
Infrared
Thermal
Depth
Video
```
The sensor should be selected according to the application's requirements.

## Image Resolution

Higher resolution provides more visual information but also increases processing requirements.

For example:
```text
640 × 480
     ↓
Lower compute requirement
```
```text
1920 × 1080
     ↓
Higher compute requirement
```
```text
3840 × 2160
     ↓
Much higher compute and memory requirement
```

The highest resolution is not always the best choice.

The correct resolution depends on:
```text
Object size
Detection distance
Required accuracy
Processing capability
Memory
Latency requirements
Pre-processing
```
Before an image enters the neural network, it may need to be transformed.

Common operations include:
```text
Resizing
Normalization
Color conversion
Cropping
Padding
Noise reduction
Format conversion
```
Conceptually:
```text
Raw Image
    ↓
Resize
    ↓
Normalize
    ↓
Color Conversion
    ↓
Model Input
```
Pre-processing should be efficient because it happens repeatedly for every frame.

## Image Classification

Image classification answers:

#### What is in this image?

For example:
```text
Input Image
     ↓
AI Model
     ↓
┌──────────────────┐
│ Person     0.91  │
│ Vehicle    0.06  │
│ Animal     0.03  │
└──────────────────┘
```
Classification usually produces one or more class probabilities.

Applications include:
```text
Product inspection
Plant classification
Medical imaging
Wildlife monitoring
Industrial inspection
Device diagnostics
Object Detection
```

Object detection answers two questions:

#### What objects are present?
#### Where are they located?

A typical result contains:
```text
Object Class
Bounding Box
Confidence Score
```
Conceptually:
```text

        ┌───────────────┐
        │    PERSON     │
        │               │
        │               │
        └───────────────┘
             0.94
```
A detection model may return multiple objects in a single frame.
```text
Frame
 ├── Person      0.94
 ├── Car         0.91
 ├── Bicycle     0.87
 └── Dog         0.83

 ```

## Object Detection Pipeline

A simplified detection pipeline is:
```text
Camera
  ↓
Frame
  ↓
Pre-processing
  ↓
Object Detection Model
  ↓
Bounding Boxes
  ↓
Confidence Filtering
  ↓
Final Detections
 ```
Confidence thresholds are often used to remove weak predictions.

For example:
```text
Confidence > 0.50
        ↓
Keep Detection

 ```
## Semantic Segmentation

Semantic segmentation assigns a class to individual pixels.

Instead of only detecting an object:
```text
Person → Bounding Box
 ```
segmentation can determine:
```text
Person → Pixel-level region
 ```
Conceptually:
```text
Input Image
     ↓
Segmentation Model
     ↓
Pixel Classification
     ↓
Segmentation Mask
 ```
This can be useful for:
```text
Road scenes
Agriculture
Industrial inspection
Robotics
Medical imaging
Autonomous systems
 ```
## Instance Segmentation

Instance segmentation goes one step further.

It identifies individual objects and provides a mask for each instance.

For example:
```text
Person A → Mask A
Person B → Mask B
Person C → Mask C
 ```
This allows the system to distinguish between separate objects belonging to the same class.

## Object Tracking

Detection tells us what is present in a frame.

Tracking attempts to maintain object identity across multiple frames.
```text
Frame 1
   ↓
Person #1
   ↓
Frame 2
   ↓
Person #1
   ↓
Frame 3
   ↓
Person #1
 ```
A tracker may assign an ID:
```text
Person → ID 17
Vehicle → ID 42
 ```
This enables applications such as:
```text
People counting
Vehicle tracking
Traffic monitoring
Retail analytics
Robotics
Security systems
Detection + Tracking
 ```

## A common architecture combines detection and tracking.
```text
Camera
   ↓
Frame
   ↓
Object Detector
   ↓
Detected Objects
   ↓
Tracker
   ↓
Object IDs
   ↓
Application Logic
 ```

The detector identifies objects.

The tracker maintains their identities over time.

## Real-Time Vision

Many edge vision applications require real-time processing.

For example:
```text
Camera
   ↓
30 FPS
   ↓
30 Frames / Second
 ```
The system must process frames quickly enough to keep up with the incoming stream.

If processing is too slow:
```text
Camera:       30 FPS
Processing:   10 FPS
 ```
frames may accumulate or be dropped.

## FPS vs Latency

FPS and latency are related but different.

## FPS

Frames per second describes throughput.
```text
30 FPS

means the system processes approximately 30 frames per second.
 ```
## Latency

Latency describes how long one frame takes to travel through the pipeline.
```text
Capture
  ↓
Pre-processing
  ↓
Inference
  ↓
Post-processing
  ↓
Result
 ```
For example:
```text
Capture:           4 ms
Pre-processing:    5 ms
Inference:        18 ms
Post-processing:   3 ms
-----------------------
Total:            30 ms
 ```
The complete pipeline should be benchmarked.

## Edge Vision Performance

Important metrics include:
```text
Metric	Meaning
FPS	Frames processed per second
Latency	Time required for one inference pipeline
Accuracy	Quality of predictions
Memory	RAM required by the system
Model Size	Storage required by the model
Power	Energy consumed during operation
Temperature	Thermal behavior during sustained workloads
 ```
These metrics should be measured on the actual target hardware.

## Model Selection

The largest model is not necessarily the best model for edge deployment.

Consider:
```text
Accuracy
   +
Latency
   +
Memory
   +
Power
   +
Hardware Support
 ```
For an edge application, a slightly less accurate model may be preferable if it is significantly faster and more efficient.

## Lightweight Vision Models

Edge devices often benefit from efficient model architectures.

The goal is:
```text
Smaller Model
      ↓
Less Compute
      ↓
Lower Memory
      ↓
Lower Latency
      ↓
Lower Power
 ```
The exact architecture should be selected according to the application and hardware.

## Quantized Vision Models

Computer vision models can also use lower-precision representations.

For example:
```text
FP32
  ↓
FP16
  ↓
INT8
 ```
Lower precision may reduce memory requirements and improve inference performance on supported hardware.

However:
```text
Accuracy must always be validated after quantization.
 ```

## Hardware Acceleration

Edge vision systems may use:
However:
```text
CPU
GPU
NPU
AI Accelerator
 ```
A typical architecture is:
```text
Camera
   ↓
Application
   ↓
Inference Runtime
   ↓
CPU / GPU / NPU
   ↓
Vision Result
 ```
The best processor depends on:
```text
Model architecture
Runtime support
Operator support
Power budget
Required throughput
Memory Bandwidth
 ```
Computer vision can become memory-intensive.

A high-resolution frame may contain a large amount of data.

For example:
```text
High Resolution
      ↓
Large Frame
      ↓
More Memory
      ↓
More Data Movement
 ```
Repeated memory transfers can become a performance bottleneck.

Efficient tensor layouts and minimizing unnecessary copies can therefore improve performance.

## Video Processing

Real-time video introduces another challenge.

Instead of processing one image:
```text
Image
  ↓
Inference
 ```
the system continuously processes:
```text
Frame 1
Frame 2
Frame 3
Frame 4
Frame 5
 ```

The pipeline therefore needs to remain stable over long periods.

## Frame Skipping

Not every application needs to run inference on every frame.

For example:
```text
Camera
30 FPS
  ↓
Process every 2nd frame
  ↓
15 AI inferences / second
 ```
This can reduce:
```text
Compute
Power
Heat
 ```
while still providing acceptable application performance.

Whether frame skipping is appropriate depends on how quickly the objects or events can change.

## Region of Interest

Instead of processing the entire image, a system may process only a relevant region.
```text
Full Frame
┌─────────────────────────┐
│                         │
│      ┌───────────┐      │
│      │    ROI    │      │
│      │           │      │
│      └───────────┘      │
│                         │
└─────────────────────────┘
 ```
This can reduce the amount of data processed by the model.

Examples include:
```text
Road lanes
Factory conveyor areas
Door entrances
Vehicle regions
Restricted zones
Multi-Stage Vision Systems
 ```
Some applications use multiple models.

For example:
```text
Camera
   ↓
Fast Detector
   ↓
Candidate Objects
   ↓
Detailed Classifier
   ↓
Final Decision
 ```
A lightweight first-stage model can reduce the amount of work performed by a more expensive model.

## Edge Vision + Sensors

Computer vision can also be combined with other sensors.
```text
Camera ───────┐
              │
Depth Sensor ─┼──→ Edge AI
              │
IMU ──────────┤
              │
Temperature ──┘
 ```
This is called sensor fusion.

Combining multiple sources can provide a richer representation of the environment.

## Computer Vision in Robotics

Robots can use Edge Computer Vision for perception.

A simplified architecture is:
```text
Camera
   ↓
Vision Model
   ↓
Object Detection
   ↓
Scene Understanding
   ↓
Planning
   ↓
Robot Action
 ```
For robotics, latency can be particularly important because perception feeds downstream decisions.

## Industrial Computer Vision

Factories can use edge vision for real-time inspection.

For example:
```text
Production Line
      ↓
Camera
      ↓
Edge AI
      ↓
Defect Detection
      ↓
Reject / Accept
 ```
Advantages include:
```text
Real-time decisions
Local processing
Reduced bandwidth
Continuous operation
Integration with industrial systems
Smart Cameras
 ```
A smart camera combines imaging and processing.

Instead of:
```text
Camera
  ↓
Network
  ↓
Server
  ↓
AI
 ```
a smart camera can perform:
```text
Camera
  ↓
Local AI
  ↓
Result
 ```
Only the important result may need to be transmitted.

For example:
```text
Object detected
Confidence: 94%
Location: Zone 3
Privacy at the Edge
 ```
Local processing can reduce the need to transmit raw video.

Instead of sending:
```text
Continuous Video
       ↓
Cloud
 ```
the device can send:
```text
Event
Metadata
Detection
Alert
 ```
This can be useful for privacy-sensitive applications.

However, edge processing does not automatically guarantee privacy.

The complete system still requires appropriate security controls.

## Edge Vision Optimization Workflow

A practical optimization workflow is:
```text
Define Vision Task
       ↓
Select Camera
       ↓
Choose Model
       ↓
Establish Baseline
       ↓
Optimize Model
       ↓
Deploy to Hardware
       ↓
Measure FPS / Latency
       ↓
Validate Accuracy
       ↓
Optimize Pipeline
       ↓
Deploy
 ```
Optimization should be iterative.

Example: Real-Time Person Detection

Consider a camera-based people detection system.

Requirements
```text
Input:
1080p Camera

Target:
Edge Device

Requirement:
Real-time detection

Target latency:
< 40 ms

Minimum accuracy:
Application-defined
 ```

Pipeline
 ```text
Camera
   ↓
Resize Frame
   ↓
Normalize
   ↓
Object Detection
   ↓
Confidence Filtering
   ↓
Tracking
   ↓
Person Count
 ```
The final system should be benchmarked end-to-end.

### Example Performance Benchmark

A simplified benchmark could look like:
 ```text
Configuration	FPS	Latency	Memory
FP32 CPU	12	83 ms	620 MB
FP16 GPU	28	36 ms	410 MB
INT8 NPU	42	24 ms	280 MB
 ```
These numbers are illustrative.

Actual performance depends on:
 ```text
Model
Hardware
Runtime
Input resolution
Pre-processing
Post-processing
Common Edge Vision Bottlenecks
 ```
Typical bottlenecks include:
 ```text
Camera capture
Image decoding
Resizing
Color conversion
Memory copies
Model inference
Post-processing
Tracking
Network communication
 ```
A useful diagnostic approach is:
 ```text
Measure Every Stage
        ↓
Find Slowest Stage
        ↓
Optimize Bottleneck
        ↓
Benchmark Again
 ```
## Common Mistakes

Using Excessive Resolution
 ```text
Higher resolution can dramatically increase compute requirements.
 ```
Using an Oversized Model
 ```text
A large model may provide unnecessary accuracy for the application.
 ```
Ignoring Pre-processing
 ```text
Image preparation can consume significant processing time.
 ```
Ignoring Post-processing
 ```text
Detection and tracking logic can also become bottlenecks.
 ```
Benchmarking Only on a Desktop
 ```text
Desktop performance does not represent edge-device performance.
 ```
Ignoring Thermal Behavior
 ```text
A device may perform well initially and 
slow down during sustained workloads.
 ```
Measuring Only FPS
 ```text
FPS alone does not describe complete system performance.
 ```

## End-to-End Vision Architecture

A complete Edge Computer Vision system can be viewed as:
 ```text
                CAMERA
                   ↓
             FRAME CAPTURE
                   ↓
            PRE-PROCESSING
                   ↓
            AI INFERENCE
                   ↓
          POST-PROCESSING
                   ↓
             TRACKING
                   ↓
          SCENE UNDERSTANDING
                   ↓
              DECISION
                   ↓
               ACTION
 ```
Every stage contributes to the final system.

## Edge Computer Vision Stack

The complete stack can be viewed as:
 ```text
Application
     ↓
Vision Pipeline
     ↓
AI Model
     ↓
Inference Runtime
     ↓
Hardware Accelerator
     ↓
Edge Operating System
     ↓
Camera / Sensors
     ↓
Physical Environment
 ```
This is why Edge Computer Vision is both an AI problem and a systems-engineering problem.

## Key Takeaways

Remember these ideas:

1. **Edge Computer Vision processes visual data close to where it is generated.**
2. **The complete vision pipeline includes capture, pre-processing, inference and post-processing.**
3. **Image classification identifies what is present in an image.**
4. **Object detection identifies objects and their locations.**
5. **Segmentation provides pixel-level understanding.**
6. **Tracking maintains object identity across frames.**
7. **FPS measures throughput while latency measures processing time.**
8. **Quantization and efficient models can improve edge inference efficiency.**
9. **Hardware acceleration can significantly improve vision performance.**
10. **The complete pipeline must be benchmarked on the target device.**


## What's Next?

Edge Computer Vision becomes even more powerful when connected to sensors and physical systems.

## Edge AI & IoT

We will explore:
 ```text
Sensors
   ↓
Edge Device
   ↓
AI Inference
   ↓
Decision
   ↓
Actuator
 ```

#### → Next: Edge AI & IoT

 Edge AI Deployment

**Then we will look at deploying optimized AI systems into real-world devices.**

#### → Next: Edge AI Deployment

Edge AI Optimization

**Review the techniques used to make models smaller, faster and more efficient.**

#### → Back to Edge AI Optimization

## Interview Preparation

The concepts covered here are useful for:
 ```text
Edge AI interviews
Computer Vision interviews
Machine Learning interviews
AI Engineer interviews
Embedded AI roles
Robotics interviews
 ```
Important topics to understand include:
 ```text
Classification
Detection
Segmentation
Tracking
FPS
Latency
Quantization
Hardware Acceleration
Camera Pipelines
Edge Deployment
  ```

→ Explore the AI & ML Interview Preparation Guide

