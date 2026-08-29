---
title: "Testing & Evaluation"
description: "Hardware, sensor, API, ML and end-to-end validation strategy."
weight: 50
toc: true
---

The project requires both software testing and physical-system validation.

## Test Layers

```text
Hardware Bring-up
      ↓
Sensor Tests
      ↓
API / Service Tests
      ↓
ML Evaluation
      ↓
End-to-End Cycle
```

## API Testing

The prototype contains:

```text
tests/test_api.py
```

The README recommends browser/Postman testing during development. 

Tests should cover:

- `/capture`
- `/predict`
- `/sensors`
- `/control`
- invalid inputs
- service failures

## Hardware Validation

The SOP defines checks for:

| Component | Validation |
|---|---|
| Raspberry Pi | Successful boot |
| SSH | Login works |
| Sensors | No `None` / error readings |
| Camera | Image saved |
| LED relay | Ring illuminates |
| Buzzer | Audible alert |
| Enclosure | Connections secure |

The prescribed hardware sequence includes a dedicated `sensor_test.py` step. 

## GPIO Testing

Each device should be checked against the wiring map:

- I2C: RTC
- SPI: MCP3008
- GPIO: DHT22, HX711, IR
- GPIO18: relay
- GPIO22: buzzer
- GPIO23–25: RGB LED
- CSI-2: camera

A continuity check is required before powering the system. 

## Calibration

### Load Cell

```text
Empty Tray
   ↓
Zero
   ↓
Known Weight
   ↓
Scale Factor
   ↓
Verify
```

### MQ-135

The hardware document recommends a 24-hour 5V burn-in followed by recording a clean-air baseline. 

## ML Evaluation

Evaluate:

- Accuracy
- Precision
- Recall
- F1-score
- Confusion matrix
- Inference latency
- False-critical rate
- False-safe rate

For this application, the false-safe rate is particularly important because failing to flag a real deterioration event can be more consequential than an extra warning.

## End-to-End Test

A complete cycle should verify:

```text
Scheduler
 ↓
LED
 ↓
Camera
 ↓
Sensors
 ↓
Validation
 ↓
ML
 ↓
Thresholds
 ↓
Alert
 ↓
SQLite
 ↓
Dashboard
```

The operational SOP defines this sequence and its ten-minute default interval. 

## Failure Tests

| Failure | Expected response |
|---|---|
| Sensor unavailable | Retry / invalid reading |
| Camera failure | Log capture failure |
| Invalid reading | Reject / retry |
| ML unavailable | Safe error path |
| Network unavailable | Continue local logging |
| Dashboard unavailable | Monitoring continues |
| Critical condition | Local alert |

## Hardware Acceptance

Before moving to ML deployment:

- All sensors return valid readings.
- Camera captures correctly.
- LED relay works.
- Buzzer works.
- RGB LED works.
- Load cell is calibrated.
- MQ-135 baseline is recorded.
- SQLite logging works.
- Flask API responds.
- Enclosures are secured.

The goal is to validate the **whole monitoring system**, not merely the classifier.
