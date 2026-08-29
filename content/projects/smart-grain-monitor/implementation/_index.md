---
title: "Implementation"
description: "Flask application structure, hardware integration and edge-ML implementation."
weight: 20
toc: true
---

The project is deliberately developed from a PC-testable software prototype toward Raspberry Pi hardware.

## Project Structure

```text
smart-grain-monitor/
├── app/
│   ├── main.py
│   ├── config.py
│   ├── routes/
│   │   ├── camera_routes.py
│   │   ├── prediction_routes.py
│   │   ├── sensor_routes.py
│   │   └── control_routes.py
│   ├── services/
│   │   ├── camera_service.py
│   │   ├── prediction_service.py
│   │   ├── sensor_service.py
│   │   └── control_service.py
│   ├── models/
│   │   └── grain_model.h5
│   ├── utils/
│   └── static/captured/
├── tests/
├── requirements.txt
├── run.py
└── .env
```

This separation keeps API endpoints independent from camera, ML, sensor and actuator logic. 

## API Layer

| Endpoint | Purpose |
|---|---|
| `GET /capture` | Capture image |
| `GET /predict` | Predict grain condition |
| `GET /sensors` | Return sensor data |
| `POST /control` | Control connected devices |

## Service Layer

**Camera service**
- Capture image.
- Save image.
- Return capture information.

**Prediction service**
- Load model.
- Preprocess image.
- Run inference.
- Map output to grain condition.

**Sensor service**
- Read sensors.
- Normalise readings.
- Return a consistent data structure.

**Control service**
- Control actuators.
- Trigger alerts.
- Keep GPIO logic out of routes.

## Configuration

The prototype uses `.env` for values such as:

```text
FLASK_ENV
FLASK_DEBUG
HOST
PORT
MODEL_PATH
CAMERA_INDEX
API_KEY
DEVICE_ID
```

This makes deployment configuration separate from application code. 

## Development Sequence

```text
Project Structure
      ↓
/capture
      ↓
/predict
      ↓
Simulated Sensors
      ↓
Browser / Postman Tests
      ↓
Raspberry Pi Hardware
```

This is the development order specified by the project README. 

## ML Deployment Path

The prototype identifies `grain_model.h5`; the hardware plan targets TensorFlow Lite:

```text
TensorFlow
   ↓
grain_model.h5
   ↓
TFLite Conversion
   ↓
grain_model.tflite
   ↓
tflite_runtime
   ↓
Raspberry Pi
```

The hardware plan calls for MobileNetV2 transfer learning, followed by TFLite conversion and integration into the monitoring script. 

## Hardware Bring-Up

The SOP requires wiring and inspection before power-on, followed by Pi boot, SSH, sensor test, camera test, relay test and buzzer test. 
