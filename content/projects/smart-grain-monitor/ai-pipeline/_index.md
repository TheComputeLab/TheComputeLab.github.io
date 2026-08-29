---
title: "IoT & AI Pipeline"
description: "Monitoring cycle from image capture and sensor acquisition to ML inference, alerts and logging."
weight: 30
toc: true
---

The system combines **visual grain analysis** with environmental and physical measurements.

## Complete Monitoring Cycle

```text
Scheduler
   ↓
LED ON
   ↓
500 ms warm-up
   ↓
Capture Image
   ↓
LED OFF
   ↓
Read Sensors
   ↓
Validate
   ↓
ML Inference
   ↓
Threshold Evaluation
   ↓
Alert if required
   ↓
SQLite + Dashboard
   ↓
Wait 10 minutes
```

The hardware SOP specifies a default **10-minute monitoring interval**, configurable using cron/systemd. 

## Image Capture

GPIO18 activates the LED ring through a relay. After approximately 500 ms, the camera captures a 1280×960 JPEG.

```text
GPIO18 HIGH → Relay → LED ON
                    ↓
                 500 ms
                    ↓
              Camera Capture
                    ↓
              Save JPEG
                    ↓
GPIO18 LOW → LED OFF
```

Consistent illumination is important for reliable computer-vision inference.

## Sensor Acquisition

The cycle reads:

- Temperature
- Humidity
- Gas
- Weight
- Moisture
- Grain level
- Timestamp

The documented implementation uses parallel sensor reads for the monitoring cycle. 

## Data Validation

The documented baseline checks include:

```text
Temperature: 10–50°C
Humidity:    10–90%
Weight:      > 0
```

Invalid data can be marked for retry instead of being blindly passed into inference.

## ML Inference

The target edge inference path is:

```text
Captured Image
      +
Sensor Array
      ↓
TFLite Interpreter
      ↓
grain_model.tflite
      ↓
Status + Confidence
```

The system uses the conceptual status classes **Good / Warning / Critical** for operational decisions. 

## Hybrid Decision Logic

The design does not depend only on the camera model.

An alert can be triggered when:

```text
Humidity > 70%
OR CO₂ > 800 ppm
OR Temperature > 35°C
OR ML status = Critical
```

This creates a hybrid system:

```text
Vision Model ─────┐
                  ├──→ Decision ──→ Alert
Sensor Thresholds ┘
```

## Alerts

- Buzzer on GPIO22.
- RGB LED for visual state.
- Green = good.
- Yellow = warning.
- Red = critical.


## Logging

Each cycle stores:

```text
timestamp
temperature
humidity
gas
weight
moisture
level
status
image_path
```

This historical dataset can later support trend analysis and model improvement.

## Dashboard

The prototype exposes REST endpoints for camera, prediction, sensor and control operations. The hardware plan additionally describes MQTT or Flask API publication for dashboard updates. 

## Dataset Development

The planned ML dataset covers:

- Good grain
- Mouldy grain
- Wet grain
- Infested grain

Target: **500+ images per class**.

Planned workflow:

```text
Collect → Label → MobileNetV2
       → Evaluate → TFLite
       → Raspberry Pi → E2E Test
```

