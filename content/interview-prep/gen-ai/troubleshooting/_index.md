---
title: ""
description: "Interview-focused Generative AI troubleshooting covering hallucinations, RAG failures, agent failures, model errors, latency, cost, security, observability and production incidents."
weight: 100
toc: true
---
# 🛠️ Gen AI Troubleshooting
This page provides a practical framework for diagnosing failures in Generative AI applications. The key is to troubleshoot the complete system rather than assuming the LLM is always the problem.
## 1. Troubleshooting Mental Model
Start with the complete request path:
```text
User
 ↓
API
 ↓
Application
 ↓
Prompt / Context
 ↓
Retrieval / Tools
 ↓
LLM
 ↓
Validation
 ↓
Response
```
For every incident, identify:
```text
What failed?
Where did it fail?
What changed?
Can it be reproduced?
What evidence do we have?
```
## 2. First Response Checklist
When a GenAI application fails:
```text
☐ Capture request / correlation ID
☐ Check application logs
☐ Check model response
☐ Check model/provider status
☐ Check retrieval results
☐ Check tool calls
☐ Check latency
☐ Check token usage
☐ Check recent deployments
☐ Check configuration changes
☐ Check security / authorization events
```
Do not immediately change the prompt or model without identifying the failure layer.
## 3. Model Returns Wrong Answer
Possible causes:
```text
Incorrect prompt
Insufficient context
Bad retrieval
Model limitation
Conflicting context
Incorrect tool result
Generation configuration
```
Troubleshooting:
```text
Question
 ↓
Inspect prompt
 ↓
Inspect supplied context
 ↓
Inspect retrieved documents
 ↓
Inspect tool results
 ↓
Check model configuration
 ↓
Evaluate output
```
## 4. Hallucination
A hallucination is an unsupported or incorrect model response.
Possible causes:
```text
Missing information
Poor retrieval
Weak grounding
Ambiguous instructions
Conflicting sources
Model limitation
```
Mitigation:
```text
Improve retrieval
Improve context
Add grounding instructions
Use authoritative sources
Use structured output
Validate responses
Add evaluation
```
## 5. RAG Returns Irrelevant Documents
Check:
```text
Document extraction
Chunking
Chunk size
Chunk overlap
Embedding model
Query embedding
Similarity metric
Metadata filters
Top-K
Reranking
```
A useful troubleshooting sequence:
```text
User Question
 ↓
What query reached retrieval?
 ↓
What documents were retrieved?
 ↓
Were the correct documents indexed?
 ↓
Was the correct chunk retrieved?
 ↓
Was it passed to the LLM?
```
## 6. Correct Document Exists but Is Not Retrieved
Possible causes:
```text
Poor chunking
Weak embeddings
Wrong query representation
Incorrect filters
Low Top-K
Index configuration
Stale index
```
Test retrieval independently from generation.
If the correct chunk never reaches the LLM, changing the LLM prompt will not solve the retrieval problem.
## 7. RAG Retrieves Correct Data but Answer Is Wrong
This indicates a possible generation or context-use problem.
Check:
```text
Retrieved context
Context ordering
Duplicate information
Conflicting sources
Prompt instructions
Context limits
Model capability
Output validation
```
Compare:
```text
Retrieved evidence
vs
Generated answer
```
## 8. RAG Suddenly Returns Old Information
Possible causes:
```text
Stale index
Failed ingestion
Caching
Document version issue
Delete/update failure
Embedding pipeline failure
```
Check:
```text
Source document timestamp
Ingestion timestamp
Embedding timestamp
Index version
Cache
```
## 9. Documents Are Missing From Search
Check the ingestion pipeline:
```text
Source
 ↓
Extraction
 ↓
Cleaning
 ↓
Chunking
 ↓
Embedding
 ↓
Indexing
```
Identify the first stage where the document disappears.
## 10. Duplicate Results
Possible causes:
```text
Duplicate source documents
Overlapping chunks
Repeated ingestion
Multiple index versions
Poor deduplication
```
Mitigation:
```text
Document IDs
Chunk IDs
Content hashes
Versioning
Deduplication
```
## 11. Retrieval Is Too Broad
Symptoms:
```text
Many irrelevant chunks
Large prompts
High token usage
Poor answer quality
```
Investigate:
```text
Top-K
Metadata filtering
Query rewriting
Hybrid search
Reranking
Chunk size
```
## 12. Retrieval Is Too Narrow
Symptoms:
```text
Correct information is missing
Low recall
Model says "not found"
```
Investigate:
```text
Top-K
Embedding model
Query rewriting
Chunk size
Filters
Search strategy
```
## 13. Agent Selects the Wrong Tool
Possible causes:
```text
Ambiguous tool descriptions
Overlapping tools
Poor input context
Incorrect tool schema
Prompt problem
Model limitation
```
Improve:
```text
Tool names
Tool descriptions
Schemas
Examples
Routing logic
Evaluation
```
Do not depend only on natural-language instructions for critical tool selection.
## 14. Agent Uses the Same Tool Repeatedly
Possible causes:
```text
Missing state
Tool result unclear
No completion condition
Incorrect observation handling
Weak stopping logic
```
Check:
```text
Agent state
Tool result
Iteration count
Prompt
Completion criteria
```
Use explicit limits:
```text
Maximum iterations
Maximum tool calls
Time limit
Cost limit
```
## 15. Agent Performs an Unsafe Action
Treat this as an application security problem.
Required controls:
```text
Authentication
Authorization
Least privilege
Tool allow lists
Argument validation
Policy checks
Human approval
Audit logging
```
The prompt alone is not a security boundary.
## 16. Tool Call Fails
Classify the failure:
```text
Authentication
Authorization
Invalid input
Timeout
Rate limit
Network
Service outage
```
Then determine:
```text
Retryable?
Non-retryable?
Requires user action?
Requires fallback?
```
## 17. Tool Timeout
Possible causes:
```text
Slow external service
Network issue
Large request
Database bottleneck
API dependency
```
Check:
```text
Tool latency
Network latency
External service metrics
Timeout configuration
Request size
Concurrency
```
Use:
```text
Timeouts
Retries with backoff
Circuit breakers
Fallbacks
Async processing
```
## 18. API Rate Limit
Symptoms:
```text
HTTP 429
Throttling
Increasing retries
Latency spikes
```
Mitigation:
```text
Respect provider limits
Exponential backoff
Jitter
Request throttling
Queueing
Caching
Capacity planning
```
Do not create uncontrolled retry loops.
## 19. LLM API Failure
Possible causes:
```text
Provider outage
Rate limit
Authentication failure
Invalid request
Context limit
Model unavailable
Network failure
```
Check:
```text
Provider status
HTTP status
Request payload
Model name
Token count
Credentials
Recent configuration changes
```
## 20. Context Window Error
Symptoms:
```text
Request rejected
Truncated input
Unexpected output
High latency
```
Possible causes:
```text
Large conversation
Too many retrieved chunks
Large tool output
Oversized documents
Prompt growth
```
Mitigation:
```text
Reduce context
Summarize
Retrieve selectively
Deduplicate
Compress context
Use appropriate model context limits
```
## 21. Output Is Truncated
Possible causes:
```text
Output token limit
Context limit
Provider limit
Stop sequence
Timeout
```
Check:
```text
Input tokens
Output token limit
Total context
Stop conditions
Timeouts
```
## 22. Structured Output Is Invalid
Symptoms:
```text
Invalid JSON
Missing fields
Wrong data types
Unexpected values
```
Mitigation:
```text
Explicit schema
Structured-output capability
Schema validation
Retry / repair strategy
Application-side validation
```
Never assume model-generated JSON is automatically valid.
## 23. Prompt Works in Testing but Fails in Production
Compare:
```text
Model version
Prompt version
Input distribution
Context
Retrieved documents
Tool results
Generation parameters
Token limits
Application code
```
A demo dataset may not represent production traffic.
## 24. Recent Deployment Caused Failures
Use change correlation:
```text
Incident
 ↓
Recent changes
 ↓
Prompt
Model
Code
Retrieval
Infrastructure
Configuration
 ↓
Compare before / after
```
Version all important GenAI components.
## 25. Latency Suddenly Increases
Break the request into stages:
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
LLM Time to First Token
 ↓
Generation
 ↓
Tools
```
Measure each stage independently.
## 26. LLM Is Slow
Possible causes:
```text
Large model
Large prompt
Large context
Provider load
High concurrency
Long output
Network latency
```
Possible optimizations:
```text
Smaller model
Prompt reduction
Context reduction
Streaming
Caching
Model routing
```
Measure quality impact before reducing model capability.
## 27. RAG Is Slow
Investigate:
```text
Embedding generation
Vector search
Hybrid search
Reranking
Metadata filters
Network latency
Index configuration
```
Do not automatically blame the LLM.
## 28. Agent Is Slow
Agent latency may be caused by sequential steps:
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
Possible improvements:
```text
Parallel independent tools
Reduce unnecessary iterations
Use smaller models for simple steps
Cache results
Avoid redundant retrieval
Set timeouts
```
## 29. Cost Suddenly Increases
Break cost into:
```text
Input tokens
Output tokens
Embedding calls
Reranking
Tool calls
Model selection
Retries
Agent iterations
```
Look for:
```text
Prompt growth
Context growth
Retry loops
Agent loops
Increased traffic
Model change
```
## 30. Token Usage Is Too High
Check:
```text
System prompt
Conversation history
Retrieved context
Tool outputs
Repeated instructions
Output length
```
Reduce:
```text
Duplicate context
Irrelevant documents
Unnecessary history
Verbose tool output
```
## 31. Agent Cost Is Too High
Common causes:
```text
Too many iterations
Expensive model
Repeated tool calls
Large context
Retries
Unnecessary planning
```
Controls:
```text
Iteration budget
Token budget
Cost budget
Model routing
Caching
Tool limits
```
## 32. Cache Causes Incorrect Results
Caching can introduce stale or unauthorized responses.
Check:
```text
Cache key
User identity
Tenant
Data version
TTL
Invalidation
Permissions
```
A cache key should reflect the information required to safely reuse the response.
## 33. Security Incident
If sensitive information is exposed:
```text
Stop / contain
 ↓
Identify affected data
 ↓
Identify affected users / tenants
 ↓
Check retrieval logs
 ↓
Check tool calls
 ↓
Check authorization
 ↓
Rotate credentials if required
 ↓
Preserve evidence
 ↓
Fix control
 ↓
Retest
```
Follow the organization's incident-response process.
## 34. Cross-Tenant Data Leakage
Check:
```text
Authentication
Tenant identification
Authorization
Metadata filters
Vector namespaces
Database queries
Cache keys
Logging
```
Tenant isolation must exist below the model layer.
## 35. Prompt Injection Incident
Investigate:
```text
Source of untrusted content
Retrieved documents
Prompt construction
Tool permissions
Agent decisions
Audit logs
```
Mitigate:
```text
Separate instructions from data
Restrict tools
Validate arguments
Use least privilege
Add approval gates
```
## 36. Observability Checklist
Capture useful telemetry:
```text
Request ID
User / tenant context
Model
Prompt version
Input token count
Output token count
Retrieval query
Retrieved document IDs
Tool names
Tool status
Latency
Errors
Retries
Cost
Final outcome
```
Protect or redact sensitive information in telemetry.
## 37. Troubleshooting With Correlation IDs
A correlation ID should follow the request:
```text
User Request
 ↓
API
 ↓
Application
 ↓
Retriever
 ↓
LLM
 ↓
Tool
 ↓
Response
```
This makes multi-component incidents much easier to investigate.
## 38. Logs vs Metrics vs Traces
### Logs
Detailed event information:
```text
Errors
Tool responses
State changes
Configuration events
```
### Metrics
Aggregated measurements:
```text
Latency
Error rate
Token usage
Cost
Throughput
```
### Traces
End-to-end request flow:
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
## 39. Production Debugging Workflow
Use:
```text
1. IDENTIFY
   What exactly failed?

2. REPRODUCE
   Can the failure be reproduced?

3. LOCALIZE
   Which component failed?

4. INSPECT
   What evidence exists?

5. HYPOTHESIZE
   What changed or caused it?

6. TEST
   Validate the hypothesis.

7. MITIGATE
   Restore service safely.

8. FIX
   Correct the underlying issue.

9. VALIDATE
   Run representative tests.

10. MONITOR
   Confirm the issue stays resolved.
```
## 40. Common Failure Matrix
| Symptom | First Area to Check |
|---|---|
| Wrong factual answer | Retrieval / grounding |
| Irrelevant RAG results | Chunking / embeddings / retrieval |
| Missing document | Ingestion / indexing |
| Old information | Freshness / cache / index |
| Wrong tool | Tool schema / routing |
| Repeated tool calls | State / stopping logic |
| Unsafe tool action | Authorization / guardrails |
| High latency | Stage-by-stage tracing |
| High cost | Tokens / model / retries |
| Invalid JSON | Structured output / validation |
| 429 errors | Rate limits / backoff |
| Context error | Prompt + retrieved context |
| Data leakage | Authorization / tenant isolation |
## 41. Common Interview Questions
### How do you troubleshoot hallucinations?
Determine whether the answer is unsupported because of missing context, retrieval failure, conflicting sources or model behavior. Inspect the evidence supplied to the model before changing the prompt.
### How do you troubleshoot RAG?
Separate ingestion, retrieval and generation. Verify that the correct document was indexed, the correct chunk was retrieved and the retrieved context was actually supplied to the model.
### How do you troubleshoot an agent loop?
Inspect state transitions, tool results, completion criteria and iteration limits. Check whether the agent understands that the previous action succeeded.
### How do you troubleshoot high latency?
Trace the request across API, retrieval, reranking, model and tool calls. Optimize the measured bottleneck.
### How do you troubleshoot high GenAI cost?
Break down usage by tokens, models, retrieval, tools, retries and agent iterations. Identify the largest cost driver before optimizing.
### How do you handle a model provider outage?
Use a tested fallback or graceful degradation strategy, protect downstream systems and communicate failure clearly to users.
## 42. Senior-Level Scenarios
### Production RAG answers are suddenly wrong after a deployment. What do you do?
First compare the deployment against the last known-good version. Check prompt, model, retrieval configuration, embedding model, index, context construction and application changes. Use traces and representative test cases to isolate the regression.
### Users report that the AI is exposing private documents. What is your priority?
Treat it as a security incident. Contain the issue, verify authorization and tenant isolation, inspect retrieval and cache behavior, identify affected data and restore service only after the access-control path is corrected and tested.
### Agent actions are creating duplicate tickets. What do you investigate?
Check retry behavior, idempotency, tool state, agent loops and API semantics. Add idempotency keys or state checks where appropriate.
### The LLM is healthy but the application is slow. What could be wrong?
Retrieval, reranking, network calls, database access, tool execution, application code or queueing may be the bottleneck. Trace the complete request instead of focusing only on the model.
## 43. Production Troubleshooting Checklist
```text
☐ Correlation ID
☐ Logs
☐ Metrics
☐ Distributed trace
☐ Model status
☐ Prompt version
☐ Model version
☐ Retrieval results
☐ Tool calls
☐ Token usage
☐ Latency breakdown
☐ Recent changes
☐ Authorization
☐ Tenant isolation
☐ Cache behavior
☐ Retry behavior
☐ Evaluation test
☐ Mitigation
☐ Root-cause fix
☐ Post-fix monitoring
```
## 44. Troubleshooting Mental Model
Remember:
```text
INPUT
 ↓
PROMPT
 ↓
CONTEXT
 ↓
RETRIEVAL
 ↓
TOOLS
 ↓
MODEL
 ↓
OUTPUT
 ↓
VALIDATION
 ↓
APPLICATION
```
Then investigate:
```text
QUALITY
SECURITY
LATENCY
COST
RELIABILITY
```
## 45. Continue the Preparation
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
- [Senior-Level Scenarios](/interview-prep/gen-ai/senior-scenarios/)
- [Deep Dive](/interview-prep/gen-ai/deep-dive/)
