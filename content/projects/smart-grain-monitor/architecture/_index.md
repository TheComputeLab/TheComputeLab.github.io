---
title: "Architecture"
description: "Hardware, software and data architecture of the Smart Grain Monitoring System."
weight: 10
toc: true
---

The system uses an **edge-computing IoT architecture**. Raspberry Pi 4 is the central controller, sensor gateway, local storage node and target ML inference device.

```text
                 GRAIN CONTAINER
┌─────────────────────────────────────────────┐
│ Camera       DHT22       MQ-135             │
│   │            │            │               │
│   │       temperature     gas               │
│   │       humidity                          │
│                                              │
│ Moisture     Load Cell      IR Level        │
└──────┬──────────┬──────────────┬────────────┘
       └──────────┴──────────────┘
                    ↓
             Raspberry Pi 4
        ┌───────────┼───────────┐
        ↓           ↓           ↓
     Sensors      ML Model    GPIO Control
        │           │           │
        └───────────┼───────────┘
                    ↓
             Grain Status
          /        |                SQLite   Dashboard   Alerts
```

## Hardware Layers

### Vision

- Pi Camera v3: 12 MP, autofocus, wide field of view.
- LED ring: controlled through a relay for consistent illumination.

### Environmental

- DHT22: temperature and relative humidity.
- MQ-135: CO₂/NH₃ and spoilage-related gas indication.
- MCP3008: ADC for analog sensors.

### Physical

- Load cell + HX711: grain weight.
- Capacitive moisture sensor: grain moisture.
- FC-51 IR: grain level.
- DS3231: offline timestamp.

The hardware document defines physical mounting positions for each sensor and specifies the camera at the centre of the lid pointing downward. 

## GPIO Architecture

| Interface | Device |
|---|---|
| GPIO2/3 I2C | DS3231 |
| GPIO4 | DHT22 |
| GPIO5/6 | HX711 |
| GPIO8–11 SPI | MCP3008 |
| GPIO17 | IR proximity |
| GPIO18 | LED relay |
| GPIO22 | Buzzer |
| GPIO23/24/25 | RGB LED |
| CSI-2 | Pi Camera v3 |

Analog sensors are routed through MCP3008 because Raspberry Pi GPIO is digital-only. 

## Software Architecture

```text
Client
  ↓
Flask Routes
  ↓
Services
 ├── Camera Service
 ├── Prediction Service
 ├── Sensor Service
 └── Control Service
  ↓
Hardware / ML / Storage
```

The prototype README explicitly separates API routes from reusable service logic. 

## Data Architecture

```text
Image + Sensor Readings
          ↓
     ML / Thresholds
          ↓
     Grain Status
          ↓
 ┌────────┼────────┐
 ↓        ↓        ↓
SQLite  Dashboard  Alert
```

The operational design logs timestamp, temperature, humidity, gas, weight, moisture, level, status and image path. 
