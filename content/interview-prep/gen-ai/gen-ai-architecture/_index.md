---
title: ""
description: "Interview-focused Generative AI architecture preparation covering application patterns, RAG, agents, model serving, security, evaluation, observability, scalability, cost and production system design."
weight: 90
toc: true
---
# 🏗️ Gen AI Architecture
This page focuses on how to design production-ready Generative AI systems and how to explain architecture decisions clearly in senior-level interviews.
## 1. Start With the Business Problem
Do not start an architecture discussion by choosing an LLM.
Start with:
```text
Business Requirement
 ↓
Users
 ↓
Inputs
 ↓
Expected Outputs
 ↓
Constraints
 ↓
Architecture
```
Clarify:
```text
What problem are we solving?
Who uses the system?
What information is required?
Is the information static or changing?
What actions are required?
What are the latency requirements?
What are the security requirements?
What is the expected scale?
What is the cost target?
```
## 2. Basic GenAI Application
A simple architecture:
```text
User
 ↓
Web / Mobile / API
 ↓
Application Service
 ↓
LLM
 ↓
Response
```
Production systems normally require more components.
## 3. Production GenAI Architecture
```text
                         ┌──────────────┐
                         │     User     │
                         └──────┬───────┘
                                ↓
                         ┌──────────────┐
                         │ API Gateway  │
                         └──────┬───────┘
                                ↓
                     ┌────────────────────┐
                     │ Application Layer  │
                     └─────────┬──────────┘
                               ↓
                  ┌─────────────────────────┐
                  │ Prompt / Orchestration  │
                  └───────────┬─────────────┘
                              ↓
             ┌────────────────┼────────────────┐
             ↓                ↓                ↓
           RAG              Tools            Memory
             ↓                ↓                ↓
             └────────────────┼────────────────┘
                              ↓
                         ┌──────────┐
                         │   LLM    │
                         └────┬─────┘
                              ↓
                    ┌──────────────────┐
                    │ Guardrails / QA  │
                    └────────┬─────────┘
                             ↓
                           User
```
Around the entire system:
```text
Security
Observability
Evaluation
Cost Controls
Access Control
```
## 4. Architecture Building Blocks
Typical components include:
```text
Frontend
API Gateway
Application Service
Model Gateway
LLM
Embedding Model
Vector Store
Search
Databases
Tool Services
Object Storage
Cache
Queue
Observability
Evaluation Pipeline
Identity / Authorization
```
Not every system requires every component.
## 5. Model Gateway
A model gateway can provide a centralized layer between applications and model providers.
Possible responsibilities:
```text
Model routing
Authentication
Rate limiting
Usage tracking
Fallbacks
Logging
Cost controls
Provider abstraction
```
Example:
```text
Application
 ↓
Model Gateway
 ├── Model A
 ├── Model B
 └── Model C
```
## 6. Model Selection
Select models based on measured requirements:
```text
Quality
Latency
Cost
Context requirements
Tool support
Structured output
Privacy
Deployment model
Availability
```
Do not automatically select the largest model.
## 7. RAG Architecture
Use RAG when the application requires external or changing knowledge.
### Ingestion
```text
Documents
 ↓
Extraction
 ↓
Cleaning
 ↓
Chunking
 ↓
Embeddings
 ↓
Vector / Search Index
```
### Query
```text
User Question
 ↓
Query Processing
 ↓
Retrieve
 ↓
Rerank / Filter
 ↓
Context Construction
 ↓
LLM
 ↓
Answer + Sources
```
## 8. Agent Architecture
For tasks requiring dynamic actions:
```text
User
 ↓
Agent Orchestrator
 ↓
LLM
 ↓
Tool Selection
 ↓
Tool
 ↓
Observation
 ↓
LLM
 ↓
Next Action
 ↓
Completion
```
Tools might include:
```text
Search
Database
REST APIs
Ticketing
Cloud APIs
Monitoring
Calculators
Internal systems
```
## 9. Workflow vs Agent
Use deterministic workflows when the process is predictable.
```text
Step 1
 ↓
Step 2
 ↓
Step 3
```
Use agentic behavior when the system needs dynamic decision-making:
```text
Goal
 ↓
Model decides action
 ↓
Observe result
 ↓
Choose next action
```
A strong architecture may combine both.
## 10. RAG + Agent
An agent can use retrieval as one of its capabilities.
```text
                    Agent
                      ↓
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
       RAG          Database       API
        ↓             ↓             ↓
        └─────────────┼─────────────┘
                      ↓
                   Result
```
RAG and agents solve different architectural problems and can coexist.
## 11. Data Architecture
GenAI applications may use several data systems:
```text
Relational DB
→ Structured transactional data

Object Storage
→ Documents and large files

Search Index
→ Keyword / text retrieval

Vector Store
→ Semantic retrieval

Cache
→ Frequently accessed data

Event / Queue
→ Asynchronous processing
```
Choose storage based on access patterns.
## 12. API Layer
The API layer can handle:
```text
Authentication
Authorization
Validation
Rate limiting
Request routing
Tenant identification
Request IDs
```
It should not blindly expose model-generated actions.
## 13. Authentication vs Authorization
Authentication:
```text
Who are you?
```
Authorization:
```text
What are you allowed to access or do?
```
Both are critical in enterprise GenAI systems.
## 14. Multi-Tenant Architecture
For SaaS systems:
```text
User
 ↓
Identity
 ↓
Tenant
 ↓
Authorization
 ↓
Data / Retrieval
 ↓
LLM
```
Tenant boundaries should be enforced in application and data layers.
Do not rely on prompts to isolate tenants.
## 15. Security Architecture
Important controls:
```text
Identity
Authorization
Least privilege
Secrets management
Encryption
Network controls
Input validation
Output validation
Audit logging
Prompt-injection defenses
```
## 16. Prompt Injection
Treat user input and retrieved documents as untrusted data.
Potential attack:
```text
Untrusted Content
 ↓
Prompt Injection
 ↓
Model
 ↓
Malicious Tool Request
```
Mitigations:
```text
Instruction/data separation
Tool authorization
Allow lists
Least privilege
Validation
Human approval
Monitoring
```
## 17. Tool Security
Tools should have:
```text
Defined schemas
Authentication
Authorization
Input validation
Rate limits
Audit logging
Timeouts
```
Sensitive write operations may require human approval.
## 18. Guardrails
Guardrails can exist at several layers:
```text
Input
 ↓
Application
 ↓
Model
 ↓
Tool Call
 ↓
Tool Result
 ↓
Output
```
Examples:
```text
Content filtering
Schema validation
Policy checks
PII handling
Tool authorization
Human approval
```
## 19. Model Serving
Model serving can be:
```text
External API
Managed cloud model
Self-hosted model
On-premises model
Hybrid
```
The choice depends on:
```text
Privacy
Cost
Latency
Control
Scale
Hardware
Compliance
```
## 20. Self-Hosted vs API Models
### External / Managed API
Advantages:
```text
Fast deployment
No model infrastructure management
Easy scaling
Access to managed models
```
Trade-offs:
```text
Provider dependency
Data governance requirements
Network latency
Pricing
```
### Self-Hosted
Advantages:
```text
More infrastructure control
Potential data-locality benefits
Customization
```
Trade-offs:
```text
GPU infrastructure
Operations
Scaling
Model upgrades
Capacity planning
```
## 21. Model Routing
Different tasks may use different models.
```text
Request
 ↓
Router
 ├── Simple task → Small model
 ├── Complex task → Large model
 └── Specialized task → Domain model
```
Routing can improve cost and latency while preserving quality.
## 22. Fallbacks
A production system should define what happens when a model or dependency fails.
Example:
```text
Primary Model
 ↓
Failure
 ↓
Fallback Model
 ↓
Failure
 ↓
Graceful Error / Human Escalation
```
Fallbacks should be tested rather than assumed to work.
## 23. Caching
Caching can reduce repeated work.
Possible layers:
```text
Embedding cache
Retrieval cache
Prompt cache
Model response cache
Application cache
```
Cache decisions must consider:
```text
Freshness
Permissions
Tenant boundaries
Cost
```
## 24. Asynchronous Processing
Not every GenAI task needs synchronous execution.
For long-running jobs:
```text
User
 ↓
API
 ↓
Queue
 ↓
Worker
 ↓
LLM / Tools
 ↓
Store Result
 ↓
Notification
```
Useful for:
```text
Document ingestion
Large batch processing
Report generation
Long-running agent tasks
Evaluation jobs
```
## 25. Streaming
For interactive applications:
```text
User
 ↓
API
 ↓
LLM
 ↓
Token Stream
 ↓
UI
```
Streaming can improve perceived latency even when total generation time remains similar.
## 26. Observability
Production GenAI systems need visibility into:
```text
Requests
Model calls
Token usage
Latency
Retrieval
Tool calls
Errors
Retries
Costs
User outcomes
```
## 27. Distributed Tracing
A request may travel through many components:
```text
Request
 ↓
API
 ↓
Application
 ↓
Retriever
 ↓
Reranker
 ↓
LLM
 ↓
Tool
 ↓
LLM
 ↓
Response
```
A correlation ID or trace ID helps connect these operations.
## 28. LLM-Specific Metrics
Useful metrics:
```text
Time to First Token
Tokens per Second
Input Tokens
Output Tokens
Cost per Request
Model Error Rate
Tool Error Rate
Retrieval Latency
```
## 29. Quality Metrics
Evaluate:
```text
Correctness
Relevance
Groundedness
Completeness
Citation accuracy
Safety
Task success
```
Quality should be measured using representative datasets.
## 30. Evaluation Architecture
A useful evaluation pipeline:
```text
Test Dataset
 ↓
Prompt / Model
 ↓
Generated Output
 ↓
Evaluation
 ↓
Metrics
 ↓
Regression Report
```
Run evaluations before deploying major model, prompt or retrieval changes.
## 31. Human Evaluation
Human review can be valuable for:
```text
Complex answers
Safety
Subjective quality
New use cases
Evaluation-set creation
```
Automated evaluation should complement, not blindly replace, human review.
## 32. Cost Architecture
Major cost drivers:
```text
Model inference
Input tokens
Output tokens
Embeddings
Vector storage
Retrieval
Tool calls
GPU infrastructure
Network traffic
```
Track cost per:
```text
Request
User
Tenant
Feature
Model
```
## 33. Latency Budget
Break latency into stages:
```text
API
 ↓
Retrieval
 ↓
Reranking
 ↓
Prompt construction
 ↓
LLM Time to First Token
 ↓
Generation
 ↓
Tools
```
Identify the actual bottleneck before optimizing.
## 34. Reliability
Production architecture should handle:
```text
Model outage
API timeout
Rate limit
Retrieval failure
Tool failure
Invalid model output
Malformed structured response
Network failure
```
Use:
```text
Timeouts
Retries
Backoff
Circuit breakers
Fallbacks
Dead-letter queues
Graceful degradation
```
## 35. Retry Strategy
Retries should depend on failure type.
```text
Transient error
→ Retry with backoff

Invalid request
→ Do not blindly retry

Authorization failure
→ Do not retry

Rate limit
→ Respect retry guidance
```
Write operations also need idempotency.
## 36. Scalability
Scale each layer independently where possible:
```text
API
Application
Retrieval
Model serving
Workers
Queues
Storage
```
Consider:
```text
Request rate
Concurrent users
Token throughput
Vector count
Document ingestion rate
```
## 37. Availability
High-availability architecture may include:
```text
Multiple application instances
Replicated databases
Redundant retrieval systems
Multiple model endpoints
Load balancing
Health checks
Failover
```
Availability requirements should match business impact.
## 38. Disaster Recovery
Consider:
```text
Backups
Data replication
Model configuration
Prompt versions
Vector indexes
Documents
Secrets
Infrastructure configuration
```
Recovery objectives:
```text
RPO
RTO
```
## 39. Data Governance
Enterprise GenAI systems should consider:
```text
Data classification
Retention
Encryption
Access controls
Data residency
Auditability
PII
Sensitive information
```
## 40. Human-in-the-Loop
For high-risk operations:
```text
AI proposes
 ↓
Policy validation
 ↓
Human approval
 ↓
Action
```
Examples:
```text
Production changes
Financial operations
Sensitive communications
Security actions
Resource deletion
```
## 41. Architecture Decision: RAG or Fine-Tuning?
Ask:
```text
Is the problem changing knowledge?
        ↓
       YES
        ↓
       RAG
```
If the problem is primarily:
```text
Behavior
Task adaptation
Output style
Specialized response patterns
```
then investigate fine-tuning or other model adaptation approaches.
Both can be combined when appropriate.
## 42. Architecture Decision: Agent or Workflow?
```text
Predictable process
→ Workflow

Dynamic decision-making
→ Agent

Mixed requirement
→ Workflow + Agent
```
Prefer the simplest architecture that satisfies the requirements.
## 43. Architecture Decision: Vector DB or SQL?
Use SQL when the requirement is primarily:
```text
Structured records
Transactions
Aggregations
Relationships
```
Use vector search when the requirement is:
```text
Semantic similarity
Unstructured content retrieval
Embedding-based search
```
Many systems need both.
## 44. Reference Enterprise Architecture
```text
                         ┌──────────────┐
                         │     User     │
                         └──────┬───────┘
                                ↓
                       ┌─────────────────┐
                       │ API Gateway / WAF│
                       └────────┬────────┘
                                ↓
                     ┌─────────────────────┐
                     │ Application Service │
                     └─────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │ Orchestration Layer  │
                    └──────────┬───────────┘
                               ↓
          ┌────────────────────┼────────────────────┐
          ↓                    ↓                    ↓
        RAG                  Agents               APIs
          ↓                    ↓                    ↓
   ┌─────────────┐      ┌─────────────┐      ┌──────────┐
   │ Search /    │      │ Tools /     │      │ Business │
   │ Vector DB   │      │ Workflows   │      │ Systems  │
   └──────┬──────┘      └──────┬──────┘      └────┬─────┘
          └────────────────────┼───────────────────┘
                               ↓
                         ┌──────────┐
                         │   LLM    │
                         └────┬─────┘
                              ↓
                     ┌─────────────────┐
                     │ Guardrails / QA │
                     └────────┬────────┘
                              ↓
                           Response

     ┌──────────────────────────────────────────────┐
     │ Security │ Evaluation │ Observability │ Cost │
     └──────────────────────────────────────────────┘
```
## 45. Common Interview Questions
### How would you design a GenAI application?
Start with requirements, data sources, expected outputs, latency, security and scale. Then choose the simplest combination of model, retrieval, tools and application components that satisfies those requirements.
### When would you use RAG?
When the application needs external, private, domain-specific or frequently changing knowledge.
### When would you use an agent?
When dynamic multi-step decision-making and tool interaction provide meaningful value.
### How do you secure a GenAI application?
Use identity, authorization, least privilege, input/output validation, tool controls, data isolation, secrets management, monitoring and prompt-injection defenses.
### How do you reduce cost?
Use appropriate models, reduce unnecessary context and tokens, cache repeated work, optimize retrieval and route tasks based on complexity.
### How do you reduce latency?
Measure each stage, then optimize the actual bottleneck using caching, smaller models, retrieval optimization, streaming, parallelism or infrastructure improvements.
### How do you evaluate a GenAI system?
Use representative datasets and measure correctness, relevance, grounding, safety, task success, latency, cost and failure rates.
## 46. Senior-Level Scenarios
### Design a private enterprise knowledge assistant.
Use authenticated APIs, authorization-aware retrieval, document ingestion, embeddings, vector or hybrid search, RAG, an LLM, citations, evaluation and comprehensive observability. Ensure retrieved documents respect user permissions.
### Design an agent that can perform production operations.
Use strict tool schemas, least-privilege identities, policy validation, audit logging, iteration limits and human approval for high-impact actions. Do not allow the model to directly control infrastructure without application-level authorization.
### Your GenAI application is expensive. What do you investigate?
Break cost down by model, input tokens, output tokens, retrieval, embeddings, tool calls and infrastructure. Then optimize the largest drivers while measuring quality impact.
### Your RAG system is accurate but slow.
Measure retrieval, reranking, context construction and model latency independently. Optimize the actual bottleneck rather than assuming the vector database or LLM is responsible.
### Your model provider has an outage.
Use a tested fallback strategy, graceful degradation, queued processing where appropriate and clear user-facing failure behavior.
## 47. Production Readiness Checklist
```text
☐ Business requirements
☐ Model selection
☐ Prompt management
☐ RAG strategy
☐ Agent / workflow strategy
☐ Data architecture
☐ API security
☐ Authentication
☐ Authorization
☐ Tenant isolation
☐ Tool security
☐ Guardrails
☐ Evaluation
☐ Observability
☐ Cost controls
☐ Latency targets
☐ Scalability
☐ Reliability
☐ Backup / recovery
☐ Data governance
☐ Human approval for high-risk actions
```
## 48. Architecture Interview Framework
Use this structure when answering system-design questions:
```text
1. REQUIREMENTS
       ↓
2. DATA
       ↓
3. MODEL
       ↓
4. RAG / TOOLS / AGENTS
       ↓
5. APPLICATION ARCHITECTURE
       ↓
6. SECURITY
       ↓
7. EVALUATION
       ↓
8. OBSERVABILITY
       ↓
9. SCALE
       ↓
10. COST + LATENCY
       ↓
11. FAILURE HANDLING
```
This demonstrates engineering thinking rather than simply naming AI technologies.
## 49. Continue the Preparation
Use the other GenAI landing-page tiles:
- [Quick Start](/interview-prep/gen-ai/quick-start/)
- [Rapid Revision](/interview-prep/gen-ai/rapid-revision/)
- [Core Concepts](/interview-prep/gen-ai/core-concepts/)
- [LLM](/interview-prep/gen-ai/llm/)
- [Prompt Engineering](/interview-prep/gen-ai/prompt-engineering/)
- [RAG](/interview-prep/gen-ai/rag/)
- [AI Agents](/interview-prep/gen-ai/ai-agents/)
- [Vector Databases](/interview-prep/gen-ai/vector-databases/)
- [Troubleshooting](/interview-prep/gen-ai/troubleshooting/)
- [Senior-Level Scenarios](/interview-prep/gen-ai/senior-scenarios/)
- [Deep Dive](/interview-prep/gen-ai/deep-dive/)
