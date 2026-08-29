---
title: "Roadmap"
description: "Evolution from hardware prototype to edge AI, dashboard, notifications and predictive grain monitoring."
weight: 60
toc: true
---

The project evolves in controlled stages:

```text
Software Prototype
       ↓
Hardware Build
       ↓
Calibration
       ↓
Dataset
       ↓
ML Training
       ↓
TFLite Edge AI
       ↓
Dashboard
       ↓
Notifications
       ↓
Predictive Monitoring
```

The hardware document currently identifies the hardware phase as in progress. 

## Phase 1 — Hardware Completion

- Complete container.
- Install camera and LED ring.
- Install sensors.
- Complete GPIO wiring.
- Run `sensor_test.py`.
- Validate camera and outputs.
- Seal the enclosure.

The SOP provides a 26-step assembly and verification sequence. 

## Phase 2 — Calibration

Calibrate the load cell using known weights and establish an MQ-135 clean-air baseline after burn-in.

## Phase 3 — ML Dataset

Collect **500+ images per class** covering:

- Good
- Mouldy
- Wet
- Infested

The dataset should include realistic variations in grain arrangement and lighting. 

## Phase 4 — Train Model

The documented plan is MobileNetV2 transfer learning with TensorFlow:

```text
Dataset
 ↓
Train / Validation Split
 ↓
MobileNetV2
 ↓
Evaluation
 ↓
Model Selection
```

## Phase 5 — Edge AI

```text
TensorFlow Model
      ↓
TFLite
      ↓
tflite_runtime
      ↓
Raspberry Pi
```

Integrate inference into `monitor.py` and test the complete cycle. 

## Phase 6 — Dashboard

Build a dashboard showing:

- Current status
- Temperature
- Humidity
- Gas
- Moisture
- Weight
- Grain level
- Trends
- Images
- Alert history

The hardware plan identifies Flask/Dash and MQTT as possible integration paths. 

## Phase 7 — Notifications

The documented plan includes:

- Telegram Bot API
- Email via SMTP
- Daily grain-status reports

A useful alert should include the status, sensor evidence, timestamp and captured image reference.

## Phase 8 — Additional Sensors

Potential upgrades include:

| Sensor | Purpose | Priority |
|---|---|---|
| BMP280 | Pressure / seal changes | High |
| MLX90614 | Grain hot-spot temperature | High |
| MQ-7 | CO / fermentation indication | High |
| SHT31-D | Higher-accuracy temp/RH | Medium |
| TSL2561 / BH1750 | Ambient light | Medium |
| Dust/PM | Particulates | Medium |
| Soil EC | Grain electrical conductivity | Low |

## Phase 9 — Cloud / Fleet Monitoring

Once the local device is reliable:

```text
Multiple Grain Monitors
        ↓
Secure Gateway
        ↓
Cloud API / MQTT
        ↓
Central Database
        ↓
Fleet Dashboard
```

Cloud should extend the system rather than become a dependency for local alerts.

## Phase 10 — Predictive Grain Health

The long-term evolution is from threshold monitoring to trend-based prediction:

```text
Humidity Trend
+ Temperature Trend
+ Gas Trend
+ Moisture Trend
+ Weight Trend
+ Image History
          ↓
      Risk Score
          ↓
Early Spoilage Prediction
```

That turns the project from a sensor dashboard into a **predictive grain-storage intelligence platform**.

> **Engineering principle:** Detect early, measure continuously, alert locally, and preserve the evidence.
