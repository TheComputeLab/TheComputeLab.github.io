---
title: "Implementation"
description: "How AIVA is structured and implemented as a modular Python application."
weight: 20
toc: true
---

AIVA is organised as a modular Python application so that speech, reasoning, tools, interfaces and infrastructure concerns remain independently testable.

## Repository Structure

The documented repository follows a Python `src` layout:

```text
aiva/
├── README.md
├── requirements.txt
├── requirements-dev.txt
├── pyproject.toml
├── src/
│   └── aiva/
│       ├── config/
│       ├── logger/
│       └── utils/
├── tests/
├── config/
├── docs/
└── .github/
    └── workflows/
```

The project documentation identifies the `src` layout as the packaging foundation. 

The broader application is organised around these functional areas:

```text
src/aiva/
├── asr/
├── llm/
├── tts/
├── wake/
├── multilingual/
├── tools/
├── cli/
├── web/
├── logging_system/
└── config/
```

## Configuration

Configuration is loaded from YAML and environment variables and validated with Pydantic.

```text
YAML + Environment
        ↓
Configuration Loader
        ↓
Pydantic Validation
        ↓
Application Settings
        ↓
Dependency Injection
```

The application launcher follows a lifecycle of configuration loading, validation, dependency injection, component initialisation, health checks and ready state. 

## Core Utility Layer

The documented foundational utilities include:

| Utility | Responsibility |
|---|---|
| `audio.py` | Audio format processing/conversion |
| `text.py` | Input sanitisation/normalisation |
| `language.py` | Language detection/code mapping |
| `retry.py` | Exponential-backoff retry logic |

Structured JSON logging and correlation IDs are designed for distributed tracing and CloudWatch compatibility. 

## Application Entry Points

AIVA supports two documented launcher modes:

| Mode | Entry point | Interface | Audio |
|---|---|---|---|
| CLI | `src/aiva/__main__.py` | Terminal | PyAudio |
| Web | `src/aiva/web/app.py` | Browser | WebRTC |

The application launcher handles startup, health verification and graceful shutdown. The documented startup target is under 3 seconds. 

## Speech Layer

The speech path is intentionally separated from language reasoning:

```text
Microphone
    ↓
Wake Detection
    ↓
Audio Capture
    ↓
faster-whisper
    ↓
Transcript
```

The documented ASR benchmark reports English WER of 6.8%, Hindi WER of 14.2% and transcription latency of 1043 ms. 

## Wake Word

The wake-word module uses openWakeWord for the documented production design.

States:

```text
STANDBY
   ↓
DETECTED
   ↓
LISTENING
   ↓
PROCESSING
   ↓
TIMEOUT → STANDBY
```

The documented test result reports 96.4% true-positive rate, false positives below 0.5/hour, detection latency below 300 ms and standby CPU usage below 2%. 

## LLM Layer

Gemini 2.0 Flash is used as the conversational intelligence layer.

The implementation separates:

```text
Known Request → Structured Handler / Tool

Open-Domain Request → Gemini
```

This makes live-data capabilities such as weather independent of model-generated factual guesses.

## Tools

Current project capabilities include:

- Weather
- Wikipedia

The interview documentation explicitly recommends using an external weather source because weather is dynamic, with the LLM responsible for phrasing the returned information. 

## TTS

The repository-oriented project description uses **gTTS + pygame** for speech output.

The production architecture documentation separately specifies **Coqui XTTS-v2** as the cloud TTS service.

That distinction is important:

```text
Repository / application description
        ↓
gTTS → pygame

Production architecture design
        ↓
XTTS-v2 service
```

## Interfaces

AIVA exposes three interaction approaches in the project description:

- CLI
- Streamlit
- FastAPI + WebSocket

The production architecture additionally documents a React web frontend. These should be treated as separate implementation/design stages rather than conflated into one claim.

## Development Pattern

The implementation follows:

```text
Define Module
     ↓
Implement Component
     ↓
Unit Test
     ↓
Integrate
     ↓
Integration Test
     ↓
Observe
     ↓
Optimise
```

This is the key engineering pattern behind AIVA.
