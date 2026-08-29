---
title: "AI Pipeline"
description: "AIVA's voice intelligence pipeline from wake word and ASR through LLM reasoning, tools and TTS."
weight: 30
toc: true
---

The central AIVA experience is a complete voice-processing loop:

```text
VOICE
  ↓
WAKE WORD
  ↓
ASR
  ↓
LANGUAGE
  ↓
CONTEXT
  ↓
LLM / TOOL
  ↓
TTS
  ↓
VOICE
```

## Step 1 — Wake Word

AIVA listens for the activation phrase:

> **Hey AIVA**

The wake-word module is designed to keep standby processing lightweight and activate the expensive pipeline only after detection.

The documented states are:

```text
STANDBY → DETECTED → LISTENING → PROCESSING → TIMEOUT
```

The documented benchmark achieved 96.4% true-positive rate with fewer than 0.5 false positives per hour. 

## Step 2 — Audio Capture

After activation, the system captures user speech and streams audio to the processing pipeline.

The production architecture specifies 16 kHz PCM audio with VAD active and WebSocket/WSS transport. 

## Step 3 — ASR

**ASR = Automatic Speech Recognition.**

AIVA uses faster-whisper to convert speech into text.

```text
Audio
  ↓
faster-whisper
  ↓
Transcript
```

The documented benchmark:

| Metric | Result |
|---|---:|
| English WER | 6.8% |
| Hindi WER | 14.2% |
| Transcription latency | 1043 ms |

All three reported targets passed. 

## Step 4 — Language Handling

AIVA supports multilingual interaction.

The documented production language matrix covers English (`en-IN`) and Hindi (`hi-IN`) with code-switching detection. The broader project description also identifies Marathi as supported.

The documented detection benchmark achieved 97.3% accuracy and code-switch latency of 143 ms. 

The processing concept is:

```text
Transcript
    ↓
Language Detection
    ↓
Language-specific configuration
    ↓
LLM / response generation
    ↓
TTS language / voice
```

## Step 5 — Context Assembly

The orchestrator retrieves relevant session history from Redis.

```text
Current Query
     +
Session History
     ↓
Context Assembly
```

The production design uses Redis for live session state and DynamoDB for persistent history. Sessions are designed around TTL-based lifecycle management. 

## Step 6 — Intent / LLM Routing

The documented architecture uses a dialogue manager before the general LLM path.

```text
                  Request
                     ↓
             Intent Evaluation
                /                    Known intent     Unknown
              ↓               ↓
           Tool /           Gemini
         structured          LLM
           handler
```

The production architecture describes a taxonomy of 50 intents and an LLM-as-fallback pattern. 

## Step 7 — Tool Execution

Tools provide external or deterministic information.

### Weather

```text
Weather Question
      ↓
Weather Tool
      ↓
Open-Meteo
      ↓
Current Data
      ↓
LLM Response
```

The reason for this architecture is simple: live weather information should come from a live data source rather than model memory.

### Wikipedia

```text
Knowledge Query
      ↓
Wikipedia Integration
      ↓
Retrieved Information
      ↓
Response
```

The project documentation also describes Wikipedia retrieval as part of the integrated system.

## Step 8 — LLM Response

Gemini 2.0 Flash generates conversational responses for open-domain queries and can participate in function/tool calling.

The documented pipeline includes streaming tokens and tool-result injection before final response generation. 

## Step 9 — TTS

The response is converted back into speech.

Repository/application description:

```text
Response Text
    ↓
gTTS
    ↓
Audio
    ↓
pygame
```

Production architecture:

```text
Response Text
    ↓
XTTS-v2 Service
    ↓
WAV Audio Chunks
```

## Step 10 — Audio Delivery

The final audio is returned to the client.

```text
TTS
 ↓
Audio Chunks
 ↓
WebSocket
 ↓
Client Playback
```

This bidirectional streaming design is one reason WebSockets were selected for the architecture.

## Complete Pipeline

```text
┌─────────┐
│   MIC   │
└────┬────┘
     ↓
┌────────────┐
│ Wake Word  │
└────┬───────┘
     ↓
┌────────────┐
│    ASR     │
│faster-whisper│
└────┬───────┘
     ↓
┌────────────┐
│  Language  │
└────┬───────┘
     ↓
┌────────────┐
│   Redis    │
│  Context   │
└────┬───────┘
     ↓
┌────────────┐
│  Dialogue  │
│   Router   │
└────┬───────┘
     ├──────────────→ Tools
     ↓
┌────────────┐
│   Gemini   │
└────┬───────┘
     ↓
┌────────────┐
│    TTS     │
└────┬───────┘
     ↓
┌────────────┐
│   Speaker  │
└────────────┘
```

## Latency

The documented observed breakdown is:

| Stage | Observed |
|---|---:|
| ASR | 1043 ms |
| LLM | 412 ms |
| TTS | 831 ms |
| Full pipeline | 2798 ms |

The observed full pipeline therefore remained below the documented 4-second target. 
