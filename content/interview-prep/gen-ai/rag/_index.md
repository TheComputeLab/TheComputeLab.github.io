---
title: ""
description: "Interview-focused RAG preparation covering ingestion, chunking, embeddings, retrieval, reranking, prompting, evaluation, hallucination mitigation, security, performance and production architecture."
weight: 60
toc: true
---
# 📚 Retrieval-Augmented Generation
RAG combines information retrieval with language generation. Instead of relying only on what the model learned during training, the application retrieves relevant external information and provides it to the model as context.
## 1. What Is RAG?
A simple RAG flow is:
```text
User Question
      ↓
Query Processing
      ↓
Retriever
      ↓
Relevant Documents
      ↓
Context
      ↓
LLM
      ↓
Generated Answer
```
### Interview Answer
> "RAG is an architecture that retrieves relevant external information at inference time and supplies it to an LLM as context. It is useful for domain-specific, private or frequently changing information."
## 2. Why Use RAG?
RAG is commonly used when an application needs:
```text
Private enterprise knowledge
Frequently changing information
Domain-specific documents
Knowledge not present in the base model
Citations / source grounding
```
Examples:
```text
Company policies
Technical documentation
Support knowledge bases
Product documentation
Incident records
Research papers
```
## 3. RAG Architecture
A production RAG system normally has two major paths.
### Ingestion Path
```text
Documents
 ↓
Load
 ↓
Clean
 ↓
Chunk
 ↓
Embed
 ↓
Index
```
### Query Path
```text
User Question
 ↓
Query Processing
 ↓
Retrieve
 ↓
Rerank / Filter
 ↓
Build Context
 ↓
LLM
 ↓
Answer
```
Separating ingestion from query-time retrieval is an important architectural concept.
## 4. Document Ingestion
Documents may come from:
```text
PDF
HTML
Markdown
Word documents
Databases
Cloud storage
APIs
Internal applications
```
The ingestion pipeline should preserve useful information such as:
```text
Document ID
Title
Source
Timestamp
Access permissions
Section
Metadata
```
## 5. Document Processing
Before indexing, documents may need:
```text
Text extraction
Cleaning
Normalization
Deduplication
Metadata extraction
Table handling
Image / OCR processing
```
Poor document processing can create poor retrieval results even when the embedding model is good.
## 6. Chunking
Chunking divides large documents into smaller pieces that can be retrieved.
Example:
```text
Large Document
      ↓
Section 1
Section 2
Section 3
      ↓
Smaller chunks
```
### Why Chunk?
Retrieving an entire large document can add irrelevant context and increase token usage.
### Chunking Strategies
```text
Fixed-size chunks
Recursive chunks
Sentence-based chunks
Paragraph-based chunks
Section-based chunks
Semantic chunks
```
## 7. Chunk Size
There is no universal perfect chunk size.
Consider:
```text
Document structure
Question complexity
Embedding model
Retrieval method
Context window
Expected answer length
```
Very small chunks may lose context. Very large chunks may contain too much irrelevant information.
## 8. Chunk Overlap
Overlap repeats a portion of content between adjacent chunks.
```text
Chunk A
[-----------]

Chunk B
       [-----------]
       ↑ overlap
```
Overlap can help preserve context across chunk boundaries.
Too much overlap increases:
```text
Storage
Index size
Retrieval redundancy
Token usage
```
## 9. Metadata
Metadata can improve retrieval and access control.
Examples:
```text
document_id
source
department
date
document_type
product
region
security_level
```
Metadata filtering can restrict search before or during vector retrieval.
## 10. Embeddings
An embedding model converts content into vectors.
```text
Text Chunk
 ↓
Embedding Model
 ↓
Vector
 ↓
Vector Index
```
Semantically related content should have useful proximity in vector space.
## 11. Vector Similarity
Common similarity approaches include:
```text
Cosine similarity
Dot product
Euclidean distance
```
The appropriate method depends on the embedding model and vector database.
### Cosine Similarity
Cosine similarity measures the angle between vectors and is commonly used for semantic similarity.
## 12. Vector Database
A vector database or vector-search system stores embeddings and supports similarity retrieval.
It may also support:
```text
Metadata filtering
Indexing
Hybrid search
Scoring
Namespaces / collections
Access controls
```
## 13. Retrieval
Retrieval selects the most relevant chunks for a query.
A simplified process:
```text
Question
 ↓
Query embedding
 ↓
Similarity search
 ↓
Top-K chunks
```
Retrieval quality is one of the most important factors in RAG quality.
## 14. Top-K
Top-K means retrieving the K highest-ranked candidates.
Example:
```text
Query
 ↓
Retrieve 20 candidates
 ↓
Select top 5
 ↓
Send relevant context to LLM
```
K should be tuned using evaluation rather than chosen arbitrarily.
## 15. Reranking
A reranker evaluates retrieved candidates more carefully and produces a better relevance ranking.
A common pattern:
```text
Query
 ↓
Fast initial retrieval
 ↓
Top-N candidates
 ↓
Reranker
 ↓
Top-K context
 ↓
LLM
```
This can improve relevance but adds latency and compute cost.
## 16. Hybrid Search
Hybrid retrieval combines multiple search strategies.
For example:
```text
Keyword / lexical search
        +
Vector / semantic search
        ↓
Combined ranking
```
Hybrid search can be useful when exact terminology, identifiers or semantic meaning all matter.
## 17. Query Rewriting
The original user question may not be the best retrieval query.
A query rewriting step can:
```text
Clarify intent
Expand terms
Resolve conversation context
Generate search-friendly wording
```
Example:
```text
User:
"What about the second failure?"

Rewritten:
"What caused the second backup job failure
in incident INC-1024?"
```
Query rewriting should be evaluated because an incorrect rewrite can hurt retrieval.
## 18. Context Construction
After retrieval, the application builds the context supplied to the LLM.
```text
System Instructions
+
User Question
+
Retrieved Chunks
+
Optional Metadata
```
The context should be:
```text
Relevant
Non-duplicated
Within token limits
Properly attributed
Security-appropriate
```
## 19. Grounded Generation
A grounded RAG prompt can instruct the model to use supplied context.
Example:
```text
Answer using only the supplied context.
If the context does not contain enough information,
state that the answer is unavailable.

Context:
{retrieved_context}

Question:
{question}
```
Prompting helps, but retrieval quality and application controls remain important.
## 20. Citations
RAG systems can return citations that identify the sources used for an answer.
Example:
```text
Answer
  ↓
Source 1
Source 2
Source 3
```
Useful citation metadata may include:
```text
Document title
Page
Section
URL
Document ID
Timestamp
```
## 21. RAG vs Fine-Tuning
| RAG | Fine-Tuning |
|---|---|
| Adds information during inference | Changes model behavior through additional training |
| Good for changing external knowledge | Useful for behavior or task adaptation |
| Sources can be updated independently | Training is required for updates |
| Retrieval quality matters | Training-data quality matters |
A system can also use both when appropriate.
## 22. RAG vs Long Context
A larger context window can allow more information to be provided directly to a model, but that does not eliminate the need for retrieval architecture.
Consider:
```text
Context size
Cost
Latency
Relevance
Information freshness
Access control
```
Retrieval can reduce irrelevant context and select the most useful information.
## 23. Hallucinations in RAG
RAG reduces some hallucination risks but does not guarantee factual correctness.
Possible causes:
```text
Wrong documents retrieved
Poor chunking
Insufficient context
Conflicting sources
Weak grounding instructions
Model generation errors
```
## 24. Retrieval Failure
If the answer is wrong, first determine whether the correct information was retrieved.
```text
Question
 ↓
Was the correct document found?
 ↓
Was the correct chunk found?
 ↓
Was it ranked highly enough?
 ↓
Was it passed to the model?
 ↓
Did the model use it correctly?
```
This separates retrieval failures from generation failures.
## 25. RAG Evaluation
Evaluate the system at multiple stages.
### Retrieval Metrics
Examples:
```text
Recall@K
Precision@K
MRR
NDCG
```
### Generation Metrics
Consider:
```text
Correctness
Relevance
Groundedness
Completeness
Citation accuracy
Safety
```
### System Metrics
```text
Latency
Cost
Error rate
Throughput
```
## 26. Recall vs Precision
### Recall
Measures how much of the relevant information was retrieved.
```text
High recall
→ Fewer relevant documents missed
```
### Precision
Measures how much of the retrieved information is relevant.
```text
High precision
→ Less irrelevant information retrieved
```
RAG systems often need to balance both.
## 27. Retrieval Evaluation
A useful test set contains:
```text
Question
Expected relevant document
Expected relevant chunk
Acceptable answer
```
This allows the retrieval pipeline to be evaluated independently of the LLM.
## 28. RAG Failure Modes
| Failure | Likely Area |
|---|---|
| Correct document not retrieved | Retrieval |
| Correct document but wrong chunk | Chunking |
| Relevant chunk ranked too low | Ranking |
| Good context but wrong answer | Generation |
| Outdated answer | Source freshness |
| Unauthorized information | Access control |
| Duplicate context | Retrieval / context construction |
| High latency | Retrieval / reranking / model |
| High cost | Context size / model / retrieval |
## 29. Access Control
Enterprise RAG must enforce document permissions.
A critical rule:
```text
User permissions
       ↓
Retrieval authorization
       ↓
Only authorized content
       ↓
LLM context
```
Do not rely on the LLM to decide whether a user is allowed to see a document.
## 30. Security Risks
Important RAG security concerns:
```text
Prompt injection
Malicious documents
Data leakage
Unauthorized retrieval
Sensitive metadata exposure
Tool abuse
Cross-tenant data access
```
Retrieved documents should be treated as untrusted data.
## 31. Freshness
RAG is useful for changing information because documents can be updated independently of model training.
A production system may need:
```text
Incremental ingestion
Document versioning
Deletion handling
Re-indexing
Freshness metadata
Cache invalidation
```
## 32. Caching
Caching can reduce latency and cost.
Possible layers:
```text
Embedding cache
Retrieval cache
Reranking cache
LLM response cache
```
Caching must respect:
```text
User permissions
Data freshness
Tenant boundaries
```
## 33. RAG Performance
If a RAG system is slow, isolate the latency:
```text
Document retrieval
 ↓
Embedding generation
 ↓
Vector search
 ↓
Reranking
 ↓
Prompt construction
 ↓
LLM time to first token
 ↓
Generation
```
Do not assume the LLM is always the bottleneck.
## 34. RAG Cost Optimization
Consider:
```text
Smaller embedding models
Efficient retrieval
Lower K
Reranking only when useful
Context compression
Caching
Appropriate LLM selection
Prompt optimization
```
Every retrieved token that reaches the model can affect cost and latency.
## 35. Context Compression
Context compression removes redundant or low-value information before sending context to the LLM.
Possible approaches:
```text
Filtering
Summarization
Deduplication
Reranking
Extractive selection
```
The goal is to maximize useful information per token.
## 36. Multi-Document Questions
Questions may require information from multiple sources.
A robust pipeline should:
```text
Retrieve multiple relevant sources
 ↓
Resolve conflicts
 ↓
Preserve source attribution
 ↓
Construct coherent context
 ↓
Generate answer
```
Conflicting sources should not be silently merged as if they were identical facts.
## 37. RAG for Structured Data
Not all enterprise questions should be solved through vector search.
For structured questions, consider:
```text
SQL
APIs
Databases
Search indexes
```
A production GenAI system can combine:
```text
RAG
+
SQL
+
APIs
+
Tools
```
## 38. RAG and Agents
An agent can use retrieval as one of its tools.
```text
Agent
 ├── Search
 ├── RAG
 ├── Database
 ├── API
 └── Calculator
```
RAG itself does not require an agent.
## 39. Common Interview Questions
### What is RAG?
RAG retrieves relevant external information and supplies it to an LLM as context before generating an answer.
### Why use RAG?
For private, domain-specific, changing or externally maintained knowledge.
### What is chunking?
Dividing documents into smaller retrievable units.
### What is an embedding?
A numerical representation used to capture semantic relationships.
### What is top-K retrieval?
Selecting the K highest-ranked retrieval results for further processing.
### Why use a reranker?
To improve the ordering and relevance of initially retrieved candidates.
### What is hybrid search?
Combining lexical and semantic retrieval approaches.
### Can RAG eliminate hallucinations?
No. It can reduce unsupported answers when retrieval and grounding work correctly, but generation can still fail.
### RAG or fine-tuning?
Use RAG when external knowledge needs to be supplied or updated; consider fine-tuning when model behavior or task adaptation is the main requirement.
### How do you troubleshoot a bad RAG answer?
First check whether the correct information was retrieved. Then inspect chunking, ranking, context construction, prompt behavior and generation.
## 40. Senior-Level Scenarios
### Your RAG system retrieves relevant documents but answers incorrectly. What do you check?
Check whether the relevant chunk was included, whether the context contains conflicting information, whether the prompt clearly requires grounding, whether the model can interpret the context and whether output evaluation identifies a generation failure.
### Retrieval is accurate but latency is too high. What do you investigate?
Measure embedding, vector search, reranking, context construction and LLM latency separately. Then optimize the actual bottleneck using caching, indexing, lower candidate counts, selective reranking or model changes.
### Users are seeing documents they should not access. What is wrong?
Authorization is being applied too late or not at all. Access control must be enforced before content reaches the model context.
### The knowledge base changes every hour. Would you fine-tune?
Usually not as the first solution. A retrieval-based architecture can ingest updated information without retraining the base model.
## 41. RAG Interview Mental Model
Remember:
```text
INGEST
 ↓
CHUNK
 ↓
EMBED
 ↓
INDEX
 ↓
RETRIEVE
 ↓
RERANK
 ↓
GROUND
 ↓
GENERATE
 ↓
EVALUATE
 ↓
MONITOR
```
For enterprise systems, add:
```text
AUTHORIZATION
SECURITY
FRESHNESS
COST
LATENCY
```
## 42. Continue the Preparation
Use the other GenAI landing-page tiles:
- [Quick Start](/interview-prep/gen-ai/quick-start/)
- [Rapid Revision](/interview-prep/gen-ai/rapid-revision/)
- [Core Concepts](/interview-prep/gen-ai/core-concepts/)
- [LLM](/interview-prep/gen-ai/llm/)
- [Prompt Engineering](/interview-prep/gen-ai/prompt-engineering/)
- [AI Agents](/interview-prep/gen-ai/ai-agents/)
- [Vector Databases](/interview-prep/gen-ai/vector-databases/)
- [Gen AI Architecture](/interview-prep/gen-ai/gen-ai-architecture/)
- [Troubleshooting](/interview-prep/gen-ai/troubleshooting/)
- [Senior-Level Scenarios](/interview-prep/gen-ai/senior-scenarios/)
- [Deep Dive](/interview-prep/gen-ai/deep-dive/)
