---
title: ""
description: "Senior-level Generative AI interview scenarios covering architecture, RAG, agents, security, reliability, performance, cost, governance and production decision-making."
weight: 110
toc: true
---
# 🎯 Senior-Level Gen AI Scenarios

This page focuses on the kind of questions where the interviewer is testing **engineering judgment**, not just knowledge of GenAI terminology.

A strong senior-level answer should explain:

```text
Requirement
   ↓
Architecture
   ↓
Trade-offs
   ↓
Security
   ↓
Reliability
   ↓
Evaluation
   ↓
Cost + Performance
   ↓
Production Operations
```

## 1. How Would You Design an Enterprise GenAI Platform?

Start with the business requirements:

```text
Users
 ↓
Authentication
 ↓
API Gateway
 ↓
Application / Orchestration
 ↓
 ┌──────────────┬──────────────┬──────────────┐
 ↓              ↓              ↓
RAG           Agents          APIs / Tools
 ↓              ↓              ↓
Search        Tools          Enterprise Systems
 ↓              ↓              ↓
 └──────────────┴──────────────┘
                ↓
               LLM
                ↓
        Guardrails / Validation
                ↓
             Response
```

Across all components:

```text
Security
Observability
Evaluation
Cost Controls
Governance
```

### Senior-Level Thinking

Do not begin by saying:

> "We will use an LLM and a vector database."

Instead explain:

```text
Business requirement
→ Data requirements
→ Model requirements
→ Retrieval requirements
→ Tool requirements
→ Security requirements
→ Scale requirements
→ Operational requirements
```

---

## 2. RAG or Fine-Tuning?

### Use RAG when:

```text
Knowledge changes frequently
Private enterprise data is required
Documents need citations
Knowledge should be updated independently
```

### Consider Fine-Tuning when:

```text
Behavior needs adaptation
Output style needs specialization
Task-specific patterns are important
```

### Strong Interview Answer

> "I would first determine whether the problem is knowledge or behavior. If the main requirement is access to changing or private knowledge, I would start with RAG. If the requirement is primarily model behavior or task adaptation, I would evaluate fine-tuning."

---

## 3. RAG Is Giving Poor Answers. What Do You Check?

Do not immediately blame the LLM.

Use:

```text
Question
 ↓
Retrieval Query
 ↓
Retrieved Documents
 ↓
Correct Chunk?
 ↓
Ranking
 ↓
Context Construction
 ↓
Prompt
 ↓
LLM
 ↓
Answer
```

Check:

```text
Document extraction
Chunking
Embedding model
Similarity metric
Top-K
Metadata filtering
Reranking
Context limits
Prompt
Model behavior
```

### Key Principle

> If the correct evidence never reaches the model, changing the model prompt will not fix the retrieval problem.

---

## 4. RAG Retrieves the Correct Document but the Answer Is Wrong

Now investigate the generation layer.

Check:

```text
Retrieved context
 ↓
Context ordering
 ↓
Conflicting information
 ↓
Prompt instructions
 ↓
Context length
 ↓
Model behavior
 ↓
Output validation
```

Compare the answer against the retrieved evidence.

Ask:

```text
Was the answer supported?
Did the model ignore evidence?
Was the evidence ambiguous?
Were multiple sources conflicting?
```

---

## 5. How Would You Reduce RAG Hallucinations?

Use multiple controls:

```text
High-quality sources
        ↓
Better chunking
        ↓
Better retrieval
        ↓
Reranking
        ↓
Grounded prompt
        ↓
Structured response
        ↓
Citation / evidence validation
        ↓
Evaluation
```

Important:

> RAG reduces hallucination risk but does not guarantee factual correctness.

---

## 6. How Would You Design an AI Agent for Production?

Start with the goal.

```text
Goal
 ↓
Agent State
 ↓
LLM
 ↓
Tool Selection
 ↓
Tool Execution
 ↓
Observation
 ↓
Next Action
 ↓
Completion
```

Production controls:

```text
Tool schemas
Authorization
Least privilege
Timeouts
Retries
Iteration limits
Cost limits
Human approval
Audit logs
Tracing
```

---

## 7. When Should You NOT Use an Agent?

This is an important senior-level question.

If the process is predictable:

```text
Step 1
 ↓
Step 2
 ↓
Step 3
```

prefer a deterministic workflow.

Use an agent when:

```text
The next action is dynamic
Multiple tools may be required
The task requires decision-making
The path cannot easily be predefined
```

### Strong Answer

> "I would not introduce an agent simply because the application uses an LLM. If a deterministic workflow solves the problem reliably, it is usually easier to test, operate and secure."

---

## 8. An Agent Is Stuck in a Loop. How Do You Fix It?

Check:

```text
Agent state
Tool result
Completion criteria
Prompt
Iteration count
Tool schema
```

Add:

```text
Maximum iterations
Maximum tool calls
Timeout
Token budget
Cost budget
Explicit completion condition
```

Also investigate why the model did not recognize the previous action as successful.

---

## 9. An Agent Selects the Wrong Tool

Possible causes:

```text
Ambiguous tool descriptions
Overlapping tools
Poor schemas
Insufficient context
Weak routing
Model limitation
```

Improve:

```text
Clear tool names
Precise descriptions
Strict input schemas
Examples
Explicit routing rules
Application-side validation
```

For critical actions, use deterministic routing or policy checks rather than trusting model selection alone.

---

## 10. The Agent Wants to Delete a Production Resource

Never allow the model to directly bypass application controls.

Use:

```text
Agent
 ↓
Proposed Action
 ↓
Authorization
 ↓
Policy Check
 ↓
Human Approval
 ↓
Tool Execution
 ↓
Audit Log
```

Use least-privilege credentials.

---

## 11. How Would You Secure an Agent?

Security layers:

```text
Identity
 ↓
Authorization
 ↓
Tool Permissions
 ↓
Input Validation
 ↓
Policy Checks
 ↓
Human Approval
 ↓
Execution
 ↓
Audit
```

Important threats:

```text
Prompt injection
Data leakage
Unauthorized tools
Credential exposure
Cross-tenant access
Malicious documents
Tool abuse
```

---

## 12. Prompt Injection Attack in a RAG System

Example:

```text
Malicious Document
 ↓
Retrieved
 ↓
Added to Context
 ↓
LLM
 ↓
Malicious Instruction
```

Mitigation:

```text
Treat retrieved content as untrusted
Separate data from instructions
Limit tool permissions
Validate tool arguments
Use allow lists
Use human approval for sensitive actions
Monitor suspicious behavior
```

The retrieved document must not automatically become a trusted instruction source.

---

## 13. Cross-Tenant Data Leakage

Treat this as a critical security issue.

Check:

```text
Authentication
Tenant identification
Authorization
Metadata filters
Vector namespaces
Database queries
Cache keys
Tool permissions
```

Correct architecture:

```text
User
 ↓
Identity
 ↓
Tenant
 ↓
Authorization
 ↓
Allowed Data
 ↓
Retrieval
 ↓
LLM
```

Never rely on a prompt such as:

```text
"Only answer using this customer's data."
```

as the actual security boundary.

---

## 14. How Would You Design Multi-Tenant RAG?

Possible isolation mechanisms:

```text
Tenant-aware indexes
Namespaces
Metadata filters
Separate collections
Separate databases
```

The choice depends on:

```text
Security requirements
Scale
Cost
Operational complexity
Compliance
```

Authorization should be enforced before unauthorized data reaches model context.

---

## 15. Users Are Getting Stale Information

Investigate:

```text
Source document
 ↓
Ingestion
 ↓
Chunking
 ↓
Embedding
 ↓
Index
 ↓
Cache
 ↓
Retrieval
```

Check timestamps and versions.

Potential causes:

```text
Failed ingestion
Stale index
Cache
Document versioning
Delete failure
Embedding pipeline failure
```

---

## 16. GenAI Application Suddenly Becomes Slow

Break latency into components:

```text
API
 ↓
Application
 ↓
Embedding
 ↓
Retrieval
 ↓
Reranking
 ↓
LLM TTFT
 ↓
Generation
 ↓
Tools
```

Measure each stage.

Do not assume the LLM is the bottleneck.

---

## 17. Agent Is Too Slow

A common pattern is:

```text
LLM
 ↓
Tool
 ↓
LLM
 ↓
Tool
 ↓
LLM
```

Optimization options:

```text
Parallel independent operations
Reduce unnecessary iterations
Use smaller models for simple tasks
Cache results
Avoid duplicate retrieval
Set timeouts
```

---

## 18. GenAI Application Is Too Expensive

Break the cost down:

```text
Input tokens
Output tokens
Embedding calls
Reranking
LLM model
Tool calls
Retries
Agent iterations
Infrastructure
```

Then optimize the largest contributor.

Possible actions:

```text
Model routing
Prompt reduction
Context reduction
Caching
Smaller models
Lower Top-K
Fewer agent iterations
```

Always measure quality impact.

---

## 19. Token Usage Is Growing

Possible causes:

```text
Conversation history
Large system prompt
Large retrieved context
Large tool responses
Repeated context
Agent state growth
```

Mitigation:

```text
Summarization
Context compression
Deduplication
Selective retrieval
Tool output filtering
Conversation truncation
```

---

## 20. Model Provider Has an Outage

A production architecture should not depend on a single untested path.

Possible design:

```text
Primary Model
 ↓
Failure
 ↓
Fallback Model
 ↓
Failure
 ↓
Graceful Degradation
 ↓
Human / Support Escalation
```

Also consider:

```text
Provider status monitoring
Timeouts
Circuit breakers
Retry policies
Capacity planning
```

Fallbacks should be tested regularly.

---

## 21. API Rate Limits Are Being Hit

Symptoms:

```text
429 errors
Retries
Latency spikes
Failed requests
```

Use:

```text
Rate limiting
Exponential backoff
Jitter
Queueing
Caching
Request batching
Capacity planning
```

Do not create an uncontrolled retry loop.

---

## 22. How Would You Design GenAI for High Availability?

Consider:

```text
Multiple application instances
Load balancing
Replicated databases
Redundant retrieval
Multiple model endpoints
Health checks
Failover
Monitoring
```

Define:

```text
Availability target
RPO
RTO
```

Architecture should match business criticality.

---

## 23. How Would You Design Disaster Recovery?

Back up or reproduce:

```text
Documents
Indexes
Databases
Prompts
Configurations
Model settings
Infrastructure
Secrets
Evaluation datasets
```

Define:

```text
RPO
→ How much data can be lost?

RTO
→ How quickly must service recover?
```

---

## 24. How Would You Evaluate a GenAI System?

Evaluate multiple layers:

### Retrieval

```text
Recall@K
Precision@K
MRR
NDCG
```

### Generation

```text
Correctness
Relevance
Groundedness
Completeness
Citation accuracy
```

### System

```text
Latency
Cost
Error rate
Throughput
```

### Agent

```text
Task success
Tool selection
Tool argument accuracy
Iterations
Safety
```

---

## 25. How Do You Prevent Prompt Changes From Breaking Production?

Version:

```text
Prompt
Model
Embedding model
Retrieval configuration
Tool schemas
Application code
```

Use:

```text
Evaluation dataset
 ↓
Baseline
 ↓
New version
 ↓
Regression evaluation
 ↓
Deploy
```

Do not rely only on manual testing.

---

## 26. Model Upgrade Causes Quality Regression

Compare:

```text
Old model
vs
New model
```

Across:

```text
Accuracy
Grounding
Tool calling
Structured output
Latency
Cost
Safety
```

Use a representative evaluation dataset before production rollout.

A staged rollout can reduce risk:

```text
Test
 ↓
Canary
 ↓
Monitor
 ↓
Gradual rollout
 ↓
Full deployment
```

---

## 27. How Would You Design Observability?

Capture:

```text
Request ID
User / tenant
Model
Prompt version
Input tokens
Output tokens
Retrieval query
Retrieved document IDs
Tool calls
Tool results
Latency
Errors
Retries
Cost
Outcome
```

Use:

```text
Logs
Metrics
Traces
```

Protect sensitive information in telemetry.

---

## 28. Logs vs Metrics vs Traces

### Logs

Detailed events:

```text
Errors
State changes
Tool failures
Configuration changes
```

### Metrics

Aggregated values:

```text
Latency
Error rate
Token usage
Cost
Throughput
```

### Traces

End-to-end execution:

```text
API
 ↓
RAG
 ↓
LLM
 ↓
Tool
 ↓
LLM
```

Use all three together.

---

## 29. How Would You Reduce GenAI Latency?

First measure.

Then consider:

```text
Smaller model
Prompt reduction
Context reduction
Caching
Streaming
Parallel tool calls
Faster retrieval
Selective reranking
Model routing
```

Important:

> Optimize the measured bottleneck, not the component you assume is slow.

---

## 30. How Would You Reduce GenAI Cost?

Use:

```text
Model routing
Caching
Prompt optimization
Context compression
Smaller models
Batch processing
Lower unnecessary Top-K
Fewer agent iterations
Efficient embeddings
```

Monitor:

```text
Cost per request
Cost per user
Cost per tenant
Cost per feature
```

---

## 31. When Would You Use a Vector Database?

Use vector search when semantic retrieval is required.

Examples:

```text
Technical documentation
Enterprise knowledge
Support articles
Research content
Unstructured documents
```

Do not use a vector database automatically.

For structured data, SQL or APIs may be better.

---

## 32. When Would You Use Hybrid Search?

Use hybrid retrieval when both semantic meaning and exact terms matter.

Example:

```text
Semantic Search
      +
Keyword Search
      ↓
Combined Ranking
```

Useful for:

```text
Error codes
Product names
Ticket IDs
Technical terminology
Natural-language questions
```

---

## 33. How Would You Choose an Embedding Model?

Consider:

```text
Retrieval quality
Language support
Domain performance
Latency
Cost
Dimensions
Deployment requirements
```

Evaluate it on your own retrieval dataset.

Do not assume that a larger embedding dimension automatically means better results.

---

## 34. How Would You Handle Structured Data?

Do not force every question through RAG.

For structured information:

```text
User
 ↓
Intent
 ↓
SQL / API / Database
 ↓
Result
 ↓
LLM
```

A production GenAI application may combine:

```text
RAG
+
SQL
+
APIs
+
Tools
+
LLM
```

---

## 35. Agent or Workflow?

Use:

```text
Predictable process
→ Deterministic workflow

Dynamic decision-making
→ Agent

Mixed process
→ Workflow + Agent
```

Senior-level principle:

> Prefer the simplest architecture that satisfies the requirements.

---

## 36. How Would You Design a Human-in-the-Loop System?

For high-risk actions:

```text
AI
 ↓
Proposed Action
 ↓
Validation
 ↓
Human Approval
 ↓
Execution
 ↓
Audit
```

Examples:

```text
Production changes
Financial transactions
Security actions
Resource deletion
External communications
```

---

## 37. How Would You Design a GenAI Platform for Multiple Models?

Use a model gateway:

```text
Applications
     ↓
Model Gateway
     ↓
 ┌────────┬────────┬────────┐
 ↓        ↓        ↓
Model A  Model B  Model C
```

Gateway responsibilities:

```text
Routing
Authentication
Rate limiting
Usage tracking
Cost tracking
Fallback
Provider abstraction
```

---

## 38. How Would You Design for Model Provider Independence?

Avoid coupling the application tightly to one provider.

Use:

```text
Application
 ↓
Model Abstraction / Gateway
 ↓
Provider A
Provider B
Provider C
```

Standardize where practical:

```text
Request interface
Response interface
Telemetry
Error handling
Fallback behavior
```

But avoid hiding provider-specific capabilities when those capabilities provide important value.

---

## 39. How Would You Handle Sensitive Data?

Consider:

```text
Data classification
 ↓
Access control
 ↓
PII handling
 ↓
Encryption
 ↓
Model policy
 ↓
Logging policy
 ↓
Retention
```

Possible controls:

```text
Redaction
Tokenization
Encryption
Private networking
Data residency
Restricted model providers
```

---

## 40. Production GenAI Architecture Checklist

```text
☐ Business requirement
☐ User requirements
☐ Data sources
☐ Model selection
☐ Prompt strategy
☐ RAG strategy
☐ Agent / workflow strategy
☐ Tool architecture
☐ API architecture
☐ Authentication
☐ Authorization
☐ Tenant isolation
☐ Security
☐ Guardrails
☐ Evaluation
☐ Observability
☐ Cost
☐ Latency
☐ Scalability
☐ Reliability
☐ Disaster recovery
☐ Governance
```

---

## 41. Senior Interview Answer Framework

When asked:

> "Design a GenAI system for X."

Use this sequence:

```text
1. Clarify Requirements
        ↓
2. Identify Data Sources
        ↓
3. Choose Model Strategy
        ↓
4. Decide RAG / Tools / Agents
        ↓
5. Design Application Architecture
        ↓
6. Design Security
        ↓
7. Design Evaluation
        ↓
8. Design Observability
        ↓
9. Discuss Scale
        ↓
10. Discuss Cost + Latency
        ↓
11. Discuss Failure Handling
```

This demonstrates architecture thinking rather than simply listing technologies.

---

## 42. Final Senior-Level Mental Model

Remember:

```text
                BUSINESS GOAL
                     ↓
                REQUIREMENTS
                     ↓
              DATA + KNOWLEDGE
                     ↓
              MODEL STRATEGY
                     ↓
        ┌────────────┼────────────┐
        ↓            ↓            ↓
       RAG         AGENTS       TOOLS
        ↓            ↓            ↓
        └────────────┼────────────┘
                     ↓
                APPLICATION
                     ↓
          SECURITY + GUARDRAILS
                     ↓
               EVALUATION
                     ↓
              OBSERVABILITY
                     ↓
          SCALE + COST + LATENCY
                     ↓
                PRODUCTION
```

The strongest senior answer is rarely:

> "Use the latest model."

It is:

> "Choose the simplest architecture that satisfies the business requirements, then design the security, evaluation, reliability, observability, scale and cost controls around it."

---

## 43. Continue the Preparation

Use the other GenAI landing-page tiles:

- [Quick Start](/interview-prep/gen-ai/quick-start/)
- [Rapid Revision](/interview-prep/gen-ai/rapid-revision/)
- [Core Concepts](/interview-prep/gen-ai/core-concepts/)
- [LLM](/interview-prep/gen-ai/llm/)
- [Prompt Engineering](/interview-prep/gen-ai/prompt-engineering/)
- [RAG](/interview-prep/gen-ai/rag/)
- [AI Agents](/interview-prep/gen-ai/ai-agents/)
- [Vector Databases](/interview-prep/gen-ai/vector-databases/)
- [Gen AI Architecture](/interview-prep/gen-ai/gen-ai-architecture/)
- [Troubleshooting](/interview-prep/gen-ai/troubleshooting/)
- [Deep Dive](/interview-prep/gen-ai/deep-dive/)
