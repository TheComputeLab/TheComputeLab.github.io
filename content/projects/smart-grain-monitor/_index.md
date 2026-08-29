---
title: "Smart Grain Monitoring System 🌾"
description: "AI + IoT grain storage monitoring using Raspberry Pi, sensors, computer vision and alerts."
weight: 20
toc: true
---

An **AI + IoT edge-monitoring system** designed to detect grain-quality problems while continuously measuring storage conditions.

```text
Camera + Sensors
       ↓
 Raspberry Pi 4
       ↓
 ┌─────┴─────┐
 ML Inference  Sensor Analysis
 └─────┬─────┘
       ↓
Good / Warning / Critical
       ↓
SQLite + Dashboard + Alerts
```

## Goals

- Camera-based grain quality classification.
- Temperature and humidity monitoring.
- Gas/spoilage indication.
- Weight, moisture and grain-level measurement.
- Local SQLite logging with captured images.
- Audible and visual alerts.
- Wi-Fi/LAN dashboard access.

The hardware specification defines two phases: **hardware completion** followed by **ML model development**. It currently marks the hardware phase as **In Progress**. fileciteturn88file4

## Current Software Prototype

The attached README describes a Flask prototype with separate `routes`, `services`, `models`, `utils`, `tests` and configuration. It is intended to be developed on a PC first, using simulated sensors, before moving to Raspberry Pi hardware. fileciteturn88file5

## Technology Stack

| Area | Technology |
|---|---|
| Edge computer | Raspberry Pi 4 4 GB |
| Camera | Pi Camera v3 |
| Sensors | DHT22, MQ-135, load cell/HX711, moisture, IR, RTC |
| ADC | MCP3008 |
| ML | TensorFlow → TFLite target |
| Backend | Python / Flask |
| Storage | SQLite + MicroSD |
| Connectivity | Wi-Fi / LAN |
| Alerts | Buzzer + RGB LED |

## Documentation

- [Architecture](architecture/) — hardware, software and data architecture
- [Implementation](implementation/) — Flask application and hardware integration
- [IoT & AI Pipeline](ai-pipeline/) — sensing → vision → inference → decision
- [Deployment](deployment/) — Raspberry Pi edge deployment
- [Testing & Evaluation](testing/) — hardware, API, ML and system testing
- [Roadmap](roadmap/) — TFLite, dashboard, notifications and predictive monitoring

> **Project status note:** this portfolio documentation distinguishes the current prototype/design from planned production capabilities rather than presenting planned features as already deployed.
