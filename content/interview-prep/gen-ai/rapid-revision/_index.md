---
title: ""
description: "Rapid Generative AI interview revision covering essential terminology, comparisons, architecture patterns and frequently asked questions."
weight: 20
toc: true
---
# ⚡ Gen AI Rapid Revision
Use this page for the final few minutes before a Generative AI interview. Focus on definitions, comparisons and short explanations.
## 1. Core Cheat Sheet
| Term | Interview-ready definition |
|---|---|
| Generative AI | AI systems that generate new content such as text, code, images or audio. |
| LLM | Large Language Model trained to process and generate language. |
| Token | A unit of text processed by a language model. |
| Context Window | The amount of tokenized information a model can consider as context for a request. |
| Prompt | Instructions and context provided to a model. |
| Embedding | A numerical vector representation used to capture semantic relationships. |
| Inference | Using a trained model to generate an output for an input. |
| Hallucination | Unsupported, incorrect or fabricated model output presented as an answer. |
| Grounding | Connecting model output to reliable information or external sources. |
| RAG | Retrieval-Augmented Generation; retrieve relevant information and provide it to a model as context. |
| Agent | A model-driven application pattern capable of selecting actions and using tools toward a goal. |
| Tool Calling | Allowing a model-driven application to invoke defined functions or external services. |
| Vector Database | A system optimized for storing and searching vector representations. |
| Fine-Tuning | Additional training used to adapt a pretrained model to a task, behavior or domain. |
## 2. Essential Architecture
```text
User
 ↓
Application
 ↓
Prompt + Context
 ↓
Retrieval / Tools / Memory
 ↓
LLM
 ↓
Validation / Guardrails
 ↓
Response
```
Production systems also require:
```text
Security
Observability
Evaluation
Cost control
Latency management
Failure handling
```
## 3. RAG Cheat Sheet
```text
Documents
 ↓
Chunking
 ↓
Embeddings
 ↓
Index
 ↓
Retrieve
 ↓
Context
 ↓
LLM
 ↓
Answer
```
### RAG Interview Points
- Chunking affects retrieval quality.
- Embeddings represent semantic relationships.
- Retrieval quality directly affects grounded responses.
- Metadata filters can narrow retrieval.
- Reranking can improve the relevance of retrieved results.
- RAG is useful when external knowledge changes frequently.
## 4. LLM Cheat Sheet
Remember:
```text
Tokens
Context
Transformer
Attention
Training
Inference
Sampling
Temperature
```
### Strong Short Answer
> "An LLM processes tokenized input and uses learned representations to generate output tokens. The application controls the surrounding context, instructions, tools and retrieval."
## 5. Prompt Engineering
A useful prompt structure:
```text
ROLE
↓
TASK
↓
CONTEXT
↓
CONSTRAINTS
↓
OUTPUT FORMAT
```
Common techniques:
```text
Zero-shot
Few-shot
Structured output
Role prompting
Context injection
Prompt templates
Evaluation
```
## 6. Agents
Basic agent loop:
```text
Goal
 ↓
Model
 ↓
Choose action
 ↓
Tool
 ↓
Observe result
 ↓
Model
 ↓
Next action / final response
```
Key interview topics:
- Tool calling
- Planning
- Memory
- Orchestration
- Human approval
- Tool failures
- Security
- Observability
## 7. Important Comparisons
### RAG vs Fine-Tuning
| RAG | Fine-Tuning |
|---|---|
| Adds external context during inference | Changes model behavior through additional training |
| Good for changing knowledge | Useful for task or behavior adaptation |
| Knowledge can be updated without retraining the base model | Requires a training process |
| Retrieval quality matters | Training-data quality matters |
### LLM vs Agent
```text
LLM
→ Generates or transforms information.

Agent
→ Uses a model inside a workflow that can select actions,
  call tools, observe results and continue toward a goal.
```
### Semantic Search vs Keyword Search
```text
Keyword search
→ Matches terms.

Semantic search
→ Searches based on meaning represented in vectors or
  semantic representations.
```
## 8. Common Interview Questions
### What is Generative AI?
AI capable of generating new content based on learned patterns and input context.
### What is an LLM?
A large language model trained to process and generate language.
### What is RAG?
A pattern that retrieves relevant external information and supplies it to an LLM as context before generation.
### Why use RAG?
To provide external, current or domain-specific information without requiring the base model itself to contain that knowledge.
### What is an embedding?
A vector representation of data that can be used for semantic similarity and retrieval.
### What is hallucination?
When a model produces unsupported or incorrect information.
### How do you reduce hallucinations?
Use grounding, retrieval, tools, validation, evaluation and appropriate prompts.
### What is an AI agent?
A model-driven system that can select actions and use tools as part of completing a task.
### What is a vector database?
A system designed to efficiently store and retrieve vector representations.
### What is fine-tuning?
Additional training that adapts a pretrained model to specific behavior, tasks or domains.
### What is context-window limitation?
The model can only process a bounded amount of tokenized context for a request. Large context also has implications for latency and cost.
## 9. Production Checklist
When asked to design a GenAI application, quickly consider:
```text
☐ Business requirement
☐ Model selection
☐ Prompt strategy
☐ RAG requirement
☐ Tool requirement
☐ Data sources
☐ Security
☐ Privacy
☐ Evaluation
☐ Hallucination mitigation
☐ Latency
☐ Cost
☐ Observability
☐ Failure handling
☐ Scalability
```
## 10. Senior-Level Answer Formula
For design questions:
```text
REQUIREMENTS
 ↓
ARCHITECTURE
 ↓
MODEL / RAG / TOOLS
 ↓
TRADE-OFFS
 ↓
SECURITY
 ↓
EVALUATION
 ↓
OBSERVABILITY
 ↓
FAILURE MODES
 ↓
SCALE + COST
```
## 11. Where to Go Next
Use the corresponding landing-page tiles for deeper preparation:
- [Quick Start](/interview-prep/gen-ai/quick-start/) — fundamentals and terminology
- [Core Concepts](/interview-prep/gen-ai/core-concepts/) — foundations
- [LLM](/interview-prep/gen-ai/llm/) — LLM-specific preparation
- [Prompt Engineering](/interview-prep/gen-ai/prompt-engineering/) — prompting
- [RAG](/interview-prep/gen-ai/rag/) — retrieval and grounding
- [AI Agents](/interview-prep/gen-ai/ai-agents/) — agents and tools
- [Vector Databases](/interview-prep/gen-ai/vector-databases/) — vector search
- [Gen AI Architecture](/interview-prep/gen-ai/gen-ai-architecture/) — system design
- [Troubleshooting](/interview-prep/gen-ai/troubleshooting/) — production failures
- [Senior-Level Scenarios](/interview-prep/gen-ai/senior-scenarios/) — senior interview scenarios
- [Deep Dive](/interview-prep/gen-ai/deep-dive/) — advanced topics
## Final 60-Second Revision
Before the interview, remember:
```text
GEN AI
→ Generate content

LLM
→ Generate language

PROMPT
→ Instructions + context

EMBEDDING
→ Semantic vector representation

RAG
→ Retrieve + ground + generate

AGENT
→ Model + tools + actions

EVALUATION
→ Measure quality

PRODUCTION
→ Secure + observe + scale
```
