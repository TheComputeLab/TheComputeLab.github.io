---
title: "Architecture"
description: "AIVA system architecture, component boundaries, data flow and architectural decisions."
weight: 10
toc: true
---

AIVA is documented as a **Hybrid Client–Cloud Microservice Architecture** with a dual-module boundary and an event-driven voice pipeline. The architecture separates client-side audio/UI responsibilities from cloud-side speech processing, LLM inference and persistence.

> **Client → WebSocket → Orchestrator → ASR → Dialogue → LLM / Tools → TTS → Client**

## Architectural Principles

### 1. Dual-module separation

**Module 1 — Client Side**

- Audio capture
- Voice activity detection
- Wake-word interaction
- Audio playback
- User interface

**Module 2 — Cloud Side**

- ASR
- Dialogue management
- LLM inference
- TTS
- Session/context management
- Persistent data

The WebSocket contract forms the boundary between the two modules.

### 2. Event-driven processing

A user interaction moves through an ordered pipeline:

```text
Audio Capture
     ↓
ASR Transcription
     ↓
Dialogue Management
     ↓
LLM / Tool Execution
     ↓
TTS Synthesis
     ↓
Audio Playback
```

### 3. LLM-as-fallback dialogue

Known intents can be processed deterministically. The LLM is then used for unmatched or open-domain requests.

This reduces unnecessary model calls and provides a clearer path for reliable tool execution.

## Component View

```text
┌─────────────────── CLIENT ───────────────────┐
│                                               │
│  Wake Word → Audio Capture → WebSocket       │
│                         ↑                     │
│                  Audio Playback               │
└────────────────────────┬──────────────────────┘
                         │ WSS
                         ↓
┌────────────────── CLOUD / BACKEND ────────────┐
│                                               │
│  FastAPI Orchestrator                         │
│       │                                       │
│       ├── ASR Engine                          │
│       ├── Dialogue Manager                    │
│       ├── LLM Proxy → Gemini                  │
│       ├── Tool Executor                       │
│       └── TTS Engine                           │
│                                               │
│  Redis ← Session State                        │
│  DynamoDB ← Persistent History / Usage        │
└───────────────────────────────────────────────┘
```

## Eight-Step Data Flow

The architecture document defines this nominal flow:

1. Wake word detected and capture begins.
2. Audio chunks are streamed to the backend.
3. faster-whisper performs ASR.
4. Redis context is assembled and intent is evaluated.
5. Gemini generates a response or invokes a tool.
6. TTS generates audio.
7. Audio is streamed back to the client.
8. Session state is updated in Redis and DynamoDB.

The documented target is a **full pipeline under 4 seconds**, with component targets of ASR ≤ 1500 ms, LLM TTFT ≤ 800 ms and TTS ≤ 900 ms. 

## Integration Interfaces

Important interfaces include:

| Flow | Interface |
|---|---|
| Audio → ASR | WebSocket binary |
| Transcript → UI | WebSocket JSON |
| Query → LLM | HTTPS / REST |
| Tokens → UI | WebSocket JSON |
| Response → TTS | Internal service interface |
| Audio → Client | WebSocket binary |

## Architecture Decisions

The documented ADRs include:

- WebSocket for bidirectional real-time communication
- ASR and TTS as sidecar services
- Whisper/faster-whisper for the ASR strategy
- Redis for low-latency session management
- LLM fallback for resilience and cost control

The documented architecture also targets 100 concurrent users and 99% availability. 

## Data Architecture

```text
                    ┌───────────────┐
                    │   Client UI   │
                    └───────┬───────┘
                            │
                       WebSocket
                            │
                            ↓
                    ┌───────────────┐
                    │  Orchestrator │
                    └───┬────┬────┬──┘
                        │    │    │
                       ASR  LLM   TTS
                        │    │    │
                        └────┴────┘
                             │
                    ┌────────┴────────┐
                    │                 │
                  Redis            DynamoDB
                ephemeral         persistent
```

The data design uses append-only message records, TTL-based deletion, correlation IDs and no PII in logs. 

## Important Implementation vs Design Note

The source material describes both the **current/repository implementation** and a more advanced **production architecture**. They should not be presented as identical.

For example, the project summary describes gTTS/pygame and Streamlit, while the production architecture document describes Coqui XTTS-v2, a React frontend and containerised cloud services. We will keep these distinctions explicit rather than claiming every production-design component is already part of the public repository.
