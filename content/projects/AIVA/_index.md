---
title: "AIVA — Personal AI Voice Assistant"
description: "A multilingual AI voice assistant combining speech recognition, LLM reasoning, intelligent tools, text-to-speech and real-time interaction."
weight: 50
toc: true
---

> **VOICE → ASR → LANGUAGE → LLM → TOOLS → TTS → RESPONSE**

AIVA is a multilingual AI-powered voice assistant designed as a modular conversational system. It combines speech recognition, large-language-model reasoning, tool integrations, text-to-speech, wake-word activation and real-time interfaces into one application.

The current implementation supports **English, Hindi and Marathi** and exposes three interaction modes:

- CLI conversation
- Streamlit web interface
- FastAPI + WebSocket API

## Project Status

| Area | Status |
|---|---|
| Core voice pipeline | 🟢 Implemented |
| Multilingual interaction | 🟢 Implemented |
| Gemini LLM integration | 🟢 Implemented |
| Weather / Wikipedia tools | 🟢 Implemented |
| Wake-word activation | 🟢 Implemented |
| Streamlit interface | 🟢 Implemented |
| FastAPI WebSocket API | 🟢 Implemented |
| Automated testing | 🟢 Implemented |
| Containerized deployment | 🟡 In Development |
| Cloud deployment | 🟡 In Development |
| CI/CD | 🟡 In Development |
| Persistent conversational memory | 🔵 Planned |

---

## Why AIVA?

AIVA explores a modular voice-AI architecture rather than coupling speech, reasoning, tools and output into one monolithic application.

```text
USER
  ↓
VOICE / TEXT
  ↓
WAKE WORD
  ↓
ASR
  ↓
LANGUAGE DETECTION
  ↓
TRANSLATION
  ↓
LLM REASONING
  ├──→ TOOLS
  ↓
RESPONSE
  ↓
TTS
  ↓
AUDIO OUTPUT
```

Each major capability can be developed, tested and replaced independently.

---

## Core Capabilities

### Speech Recognition

AIVA uses **faster-whisper** for automatic speech recognition.

```text
Microphone → Audio Capture → faster-whisper → Transcribed Text
```

### LLM Conversation

Google **Gemini 2.0 Flash** provides conversational reasoning.

Known capabilities can be handled through structured tools while general questions can be handled by the LLM.

### Text-to-Speech

AIVA uses **gTTS** for speech synthesis and **pygame** for audio playback.

```text
Response → gTTS → Audio → pygame → Speaker
```

### Wake Word

The assistant supports the wake phrase:

```text
"Hey AIVA"
```

Detection modes include energy-based detection and openWakeWord.

### Multilingual Interaction

Supported languages:

- English
- Hindi
- Marathi

The multilingual layer handles language detection and translation where required.

### Tools

Current external tools include:

- Weather via Open-Meteo
- Wikipedia knowledge lookup

The tools are intentionally separated from the LLM layer to support future agentic routing.

### Real-Time API

AIVA exposes a **FastAPI + WebSocket** interface for real-time interaction.

---

# System Architecture

```text
                    ┌───────────────┐
                    │     USER      │
                    └───────┬───────┘
                            │
                     Voice / Text
                            ↓
                    ┌───────────────┐
                    │ Wake Detector │
                    └───────┬───────┘
                            ↓
                    ┌───────────────┐
                    │ ASR / Whisper │
                    └───────┬───────┘
                            ↓
                    ┌───────────────┐
                    │   Language    │
                    │  Detection /  │
                    │  Translation  │
                    └───────┬───────┘
                            ↓
                    ┌───────────────┐
                    │ Gemini 2.0    │
                    │ Flash LLM     │
                    └───────┬───────┘
                       ┌────┴────┐
                       ↓         ↓
                  ┌────────┐ ┌──────────┐
                  │Weather │ │Wikipedia │
                  │ Tool   │ │   Tool   │
                  └────┬───┘ └────┬─────┘
                       └────┬──────┘
                            ↓
                    ┌───────────────┐
                    │  TTS / gTTS   │
                    └───────┬───────┘
                            ↓
                    ┌───────────────┐
                    │ Audio Output  │
                    └───────────────┘
```

---

# Application Modes

| Mode | Technology | Purpose |
|---|---|---|
| CLI | Python | Local conversation and testing |
| Web | Streamlit | Browser-based interaction |
| API | FastAPI + WebSocket | Real-time application integration |

This separation keeps the user interface independent from the core AI processing modules.

---

# Software Architecture

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

| Module | Responsibility |
|---|---|
| `asr` | Speech recognition |
| `llm` | Gemini integration |
| `tts` | Speech synthesis and playback |
| `wake` | Wake-word detection |
| `multilingual` | Language detection and translation |
| `tools` | External information/service tools |
| `cli` | Terminal interface |
| `web` | Streamlit and FastAPI interfaces |
| `logging_system` | Session, interaction and error logging |
| `config` | Application configuration |

---

# Technology Stack

| Layer | Technology |
|---|---|
| Language | Python 3.11+ |
| ASR | faster-whisper |
| LLM | Google Gemini 2.0 Flash |
| TTS | gTTS |
| Audio | pygame |
| Wake Word | openWakeWord / energy detection |
| Web UI | Streamlit |
| API | FastAPI + WebSockets |
| Runtime | Uvicorn |
| Weather | Open-Meteo |
| Knowledge | Wikipedia API |
| Configuration | python-dotenv, Pydantic, PyYAML |
| Cache | Redis |
| Testing | Pytest |

---

# End-to-End Request Flow

```text
01  User says "Hey AIVA"
        ↓
02  Wake-word detector activates
        ↓
03  Microphone captures speech
        ↓
04  faster-whisper transcribes audio
        ↓
05  Language is detected
        ↓
06  Text is normalised / translated
        ↓
07  Request is evaluated
        ↓
08  Tool selected when required
        ↓
09  Gemini generates response
        ↓
10  Response is converted to speech
        ↓
11  Audio is played
```

---

# Tool-Oriented Design

AIVA separates deterministic capabilities from open-ended LLM reasoning.

```text
"What is the weather?"
        ↓
   Weather Tool
        ↓
   Open-Meteo
        ↓
   Structured Result
```

Whereas:

```text
"Explain how transformers work."
        ↓
      Gemini
        ↓
 Natural Language Response
```

This architecture provides a foundation for future tools such as:

```text
Calendar
Email
Web Search
Documents
RAG
System Automation
Productivity
Personal Knowledge
```

---

# Logging & Observability

AIVA maintains structured runtime logs:

```text
logs/
├── sessions/
├── interactions/
└── errors/
```

Interaction records can include:

- User input
- Detected language
- Translated text
- Intent
- Tool selected
- Response
- Latency

Error logs capture error information and tracebacks.

---

# Testing

AIVA uses **Pytest** for automated testing.

Important areas include:

- Configuration
- Audio processing
- Language handling
- LLM integration
- Tool execution
- Wake-word detection
- API behaviour
- Error handling

The architecture is designed so individual modules can be tested independently before complete pipeline integration.

---

# Security

API credentials should be treated as secrets rather than source-code configuration.

```text
.env / Secret Store
       ↓
Environment
       ↓
Application Configuration
```

> Never commit API keys, passwords or other secrets to GitHub.

The production architecture documented for AIVA uses AWS Secrets Manager and IAM-based access rather than hard-coded credentials.

---

# Cloud & Deployment Architecture

The documented production architecture uses:

```text
Internet
   ↓
DNS / Edge
   ↓
Nginx Reverse Proxy
   ↓
AWS EC2
   ↓
Docker Compose
   ├── AIVA Orchestrator
   ├── ASR Service
   ├── TTS Service
   └── Redis
   ↓
AWS Managed Services
```

The documented deployment uses an **AWS EC2 t3.medium** host with Ubuntu, Docker Compose and Nginx.

---

# Data & Persistence

The broader production architecture separates persistent and ephemeral state:

```text
Persistent Data
      ↓
   DynamoDB

Fast / Ephemeral State
      ↓
     Redis

Observability
      ↓
   CloudWatch
```

The documented design includes persistence for users, sessions, messages and API usage, while Redis supports fast session and transient state.

---

# CI/CD Direction

The project documentation defines a GitHub Actions workflow:

```text
Git Push
   ↓
Code Quality
   ↓
Security Scans
   ↓
Unit Tests
   ↓
Integration Tests
   ↓
Docker Build
   ↓
Container Scan
   ↓
Deploy
   ↓
Health Verification
```

This turns the project into a repeatable software delivery workflow rather than a manually deployed application.

---

# Development Evolution

```text
Research
   ↓
Requirements
   ↓
Architecture
   ↓
Core Modules
   ↓
Integration
   ↓
Testing
   ↓
Web / API
   ↓
Containerization
   ↓
Cloud Deployment
   ↓
CI/CD
   ↓
Future Agentic Capabilities
```

The accompanying project documentation covers this progression from research and requirements through architecture, implementation, testing and deployment.

---

# Future Direction

Planned expansion areas include:

- Agentic tool routing
- RAG and document knowledge
- Persistent conversational memory
- React frontend
- Mobile client
- Containerized deployment
- AWS scaling
- CI/CD automation
- Additional productivity tools
- More advanced streaming voice interaction

The long-term direction is to evolve AIVA from a voice-enabled LLM application into a modular personal AI platform.

---

# Project Metrics

| Metric | Target / Design |
|---|---|
| Languages | English, Hindi, Marathi |
| Response latency target | < 4 seconds |
| Availability target | 99%+ |
| LLM | Gemini |
| ASR | faster-whisper |
| API | FastAPI + WebSocket |
| Cache | Redis |
| Cloud | AWS |
| Deployment | EC2 + Docker Compose |
| Testing | Pytest |
| CI/CD | GitHub Actions |

---

# Repository

The implementation is publicly available on GitHub:

**Personal AI Voice Assistant System — AIVA**

https://github.com/maheshkol/Personal-AI-Voice-Assistant-System---AIVA

The repository contains the modular `src/aiva` implementation, tests, configuration and application entry points.

---

# What This Project Demonstrates

AIVA demonstrates the engineering path from an AI idea to an engineered system:

```text
AI MODEL
   +
SPEECH
   +
TOOLS
   +
API
   +
UI
   +
LOGGING
   +
TESTING
   +
CONTAINERS
   +
CLOUD
   +
CI/CD
   =
ENGINEERED AI SYSTEM
```

The key learning is not one model or library. It is the integration of multiple engineering layers into a system that can be tested, observed, deployed and extended.

---

## Project Philosophy

> **Build. Test. Break. Understand. Improve.**

AIVA is an evolving system. New capabilities should be added as modular components, validated independently, integrated into the complete pipeline and measured before becoming part of the production architecture.
