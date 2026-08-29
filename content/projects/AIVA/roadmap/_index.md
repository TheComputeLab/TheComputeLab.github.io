---
title: "Roadmap"
description: "AIVA's future evolution toward a more agentic, multilingual and resilient personal AI system."
weight: 60
toc: true
---

AIVA is designed as an evolving system. The next stage is not simply adding more features; it is making the assistant more capable while preserving modularity, safety, observability and predictable behaviour.

## Current Foundation

```text
Voice
 ↓
Wake Word
 ↓
ASR
 ↓
Language
 ↓
Context
 ↓
LLM
 ↓
Tools
 ↓
TTS
```

This provides the foundation for the next generation of AIVA.

## Priority Roadmap

The final project report identifies these future enhancements:

| Enhancement | Priority |
|---|---|
| Hindi ASR fine-tuning on Indian-accent corpus | High |
| Additional Indian languages — Tamil, Bengali | High |
| On-device LLM inference / offline mode | Medium |
| Voice personalisation / cloning | Medium |
| Migration to ECS Fargate for auto-scaling | Low |
| RAG with custom document upload | Low |


## 1. Better Indian-Language ASR

Hindi noise robustness is identified as a remaining challenge.

The next step is a focused evaluation and fine-tuning effort using representative Indian-accent speech data.

```text
Indian Speech Corpus
       ↓
Data Cleaning
       ↓
Fine-tuning / Evaluation
       ↓
WER Benchmark
       ↓
Production Model
```

## 2. More Indian Languages

The roadmap identifies Tamil and Bengali as additional high-priority languages.

The language architecture should make this an incremental addition:

```text
Language Registry
      ↓
ASR Configuration
      ↓
Prompt Configuration
      ↓
TTS Configuration
      ↓
Evaluation Set
```

## 3. Agentic Tool Routing

AIVA currently has a strong foundation for agentic behaviour because tools are already separated from the conversational layer.

Future routing could become:

```text
User Request
     ↓
Intent / Planner
     ↓
Tool Selection
 ┌───┼────┬───────┐
 ↓   ↓    ↓       ↓
Web Weather Wiki Calendar
     ↓
Tool Result
     ↓
LLM Response
```

The project interview documentation proposes an intent/router mechanism, typed tool schemas, validation and state/memory as the next step toward a more agentic system. 

## 4. RAG & Personal Knowledge

The architecture already defines persistent conversation storage.

The next evolution is to allow controlled retrieval from user-provided documents:

```text
Documents
   ↓
Chunking
   ↓
Embeddings
   ↓
Vector Store
   ↓
Retriever
   ↓
LLM Context
```

This should remain separate from general conversation so that retrieval can be tested and audited independently.

## 5. Persistent Memory

AIVA can evolve from session context to longer-lived memory:

```text
Short-term
   ↓
Redis Session

Long-term
   ↓
Persistent Memory
```

Memory should be selective, explicit and observable rather than blindly storing every conversation.

## 6. Offline Mode

An on-device LLM is listed as a medium-priority roadmap item.

The conceptual architecture would become:

```text
                User
                 ↓
             AIVA Router
              /                    ↓         ↓
       Local Models   Cloud Models
             \         /
              ↓       ↓
               Response
```

The router could select local inference when privacy, connectivity or latency requirements justify it.

## 7. Voice Personalisation

Voice personalisation is also identified as a medium-priority enhancement.

The design should preserve explicit user control and avoid coupling voice identity to the core dialogue engine.

## 8. Cloud Scaling

The documented architecture currently emphasises EC2 for cost, control and learning value. ECS Fargate is identified as a later scaling option.

The migration path is intentionally straightforward because the application is already containerised:

```text
Docker Compose / EC2
        ↓
Container Registry
        ↓
ECS Fargate
        ↓
Auto Scaling
```

## 9. Better Latency

The project documentation identifies several optimisation strategies:

- Stream audio where practical
- Stream transcription
- Use efficient models
- Cache repeated tool results
- Keep prompts compact
- Use asynchronous I/O

These optimisations target the complete pipeline rather than optimising only one component.

## 10. Resilience

A production assistant needs graceful failure.

Future resilience improvements include:

```text
Timeout
  ↓
Retry if safe
  ↓
Fallback
  ↓
Clear User Message
  ↓
Structured Error Log
```

The architecture already includes failure scenarios for ASR, LLM and WebSocket communication.

## Long-Term Vision

The direction is:

```text
VOICE ASSISTANT
      ↓
CONTEXT-AWARE ASSISTANT
      ↓
TOOL-USING ASSISTANT
      ↓
AGENTIC ASSISTANT
      ↓
PERSONAL AI PLATFORM
```

The goal is not to replace the modular architecture with a single autonomous agent.

The goal is to make the existing architecture progressively more capable while retaining:

- Modular components
- Typed interfaces
- Tool validation
- Human control
- Security
- Observability
- Measurable evaluation

## AIVA Engineering Principle

> **More intelligence should not mean less control.**

Every future capability should have a clear interface, measurable behaviour and a safe failure mode.
