---
title: "Deployment"
description: "Raspberry Pi edge deployment, local networking, storage and operational setup."
weight: 40
toc: true
---

The target deployment is an **edge device installed beside the grain container**.

```text
Grain Container
      ↓
 Raspberry Pi 4
 ├── Sensors
 ├── Camera
 ├── TFLite ML
 ├── SQLite
 └── Flask API
      ↓
   Wi-Fi / LAN
      ↓
Dashboard / Remote Client
```

## Physical Deployment

The documented container design recommends approximately:

| Dimension | Value |
|---|---:|
| Width | 450 mm |
| Depth | 450 mm |
| Height | 350 mm |
| Wall thickness | 12–18 mm |
| Lid thickness | 10–12 mm |
| Camera clearance | ≥150 mm |

The Pi electronics are housed in a separate weatherproof enclosure with cable glands for protected cable routing. 

## Raspberry Pi Bring-Up

The SOP sequence is:

```text
Power On
  ↓
Boot
  ↓
Wi-Fi / LAN
  ↓
SSH
  ↓
sensor_test.py
  ↓
Camera Test
  ↓
LED Test
  ↓
Buzzer Test
  ↓
Seal Enclosure
```

Power is intentionally connected only after the wiring and continuity checks are complete. 

## Software

The Flask prototype runs with:

```bash
python run.py
```

The README also specifies a Python virtual environment and `requirements.txt` installation workflow. 

## Local Storage

```text
MicroSD
 ├── Raspberry Pi OS
 ├── grain_monitor.db
 └── /data/images/
```

Local storage allows the monitoring loop to retain data even if network connectivity is temporarily unavailable.

## Connectivity

The system can expose data through:

- Wi-Fi
- LAN
- Flask REST API
- MQTT in the planned dashboard architecture

## Security

Before wider network deployment, the API should use:

- authentication
- environment-based secrets
- restricted network access
- firewall rules
- input validation
- secure remote administration
- regular OS/package updates

The prototype already places API configuration and a future API key in `.env`. 

## Why Edge?

Edge processing is appropriate because:

1. Sensors are local.
2. Alerts do not require a cloud round trip.
3. Images can be processed locally.
4. Data can survive network outages.
5. Only selected data needs to leave the device.

Cloud connectivity can therefore be added later without making the core monitoring loop dependent on the internet.
