---
title: ""
description: "Interview-focused vector database preparation covering embeddings, similarity search, indexing, metadata filtering, hybrid search, retrieval, performance, scaling, security and production design."
weight: 80
toc: true
---
# 🗄️ Vector Databases
Vector databases and vector-search systems are commonly used in GenAI applications for semantic retrieval, especially in RAG architectures.
## 1. What Is a Vector Database?
A vector database is a system designed to store, index and search numerical vector representations efficiently.
A typical GenAI flow is:
```text
Document
 ↓
Embedding Model
 ↓
Vector
 ↓
Vector Database
 ↓
Similarity Search
 ↓
Relevant Documents
```
### Interview Answer
> "A vector database stores vector representations of data and supports efficient similarity search. In GenAI applications it is commonly used to retrieve semantically relevant information for RAG."
## 2. What Is a Vector?
A vector is a numerical representation of information.
Example:
```text
Text
 ↓
Embedding Model
 ↓
[0.12, -0.34, 0.81, ...]
```
The dimensions and values depend on the embedding model.
## 3. What Is an Embedding?
An embedding model converts data into vectors that capture useful semantic or other learned relationships.
Common inputs:
```text
Text
Images
Audio
Code
```
For RAG, text embeddings are especially common.
## 4. Why Use Embeddings?
Embeddings enable operations such as:
```text
Semantic search
Similarity search
Recommendation
Clustering
Duplicate detection
Retrieval
```
Unlike simple keyword matching, embeddings can represent relationships based on meaning.
## 5. Vector Similarity
Common similarity or distance measures include:
```text
Cosine similarity
Dot product
Euclidean distance
```
The correct choice depends on the embedding model and indexing implementation.
## 6. Cosine Similarity
Cosine similarity compares the orientation of two vectors.
Conceptually:
```text
Vector A
   ↘
    similarity
   ↗
Vector B
```
A higher cosine similarity generally indicates greater directional similarity.
The exact score interpretation depends on the vector representation and normalization.
## 7. Dot Product
The dot product is another common similarity measure.
For vectors:
```text
A = [a1, a2, ...]
B = [b1, b2, ...]
```
The dot product is:
```text
A · B = a1b1 + a2b2 + ...
```
Some systems use normalized vectors so dot-product search behaves similarly to cosine similarity.
## 8. Euclidean Distance
Euclidean distance measures the straight-line distance between vectors.
Conceptually:
```text
Smaller distance
→ More similar
```
The preferred metric depends on the embedding model and application.
## 9. Vector Index
A vector index makes similarity search efficient over potentially large numbers of vectors.
Common approximate nearest-neighbor approaches include:
```text
HNSW
IVF
PQ
```
The exact index choice depends on workload and database capabilities.
## 10. Exact vs Approximate Search
### Exact Search
Compares the query against all relevant vectors.
Advantages:
```text
High accuracy
Simple concept
```
Trade-off:
```text
Can become expensive at large scale
```
### Approximate Nearest Neighbor
Uses specialized indexes to find highly similar vectors efficiently.
Advantages:
```text
Lower search latency
Scales to larger datasets
```
Trade-off:
```text
May trade a small amount of recall for speed
```
## 11. HNSW
HNSW stands for Hierarchical Navigable Small World.
It creates a graph-based structure that supports efficient approximate nearest-neighbor search.
Conceptually:
```text
Top Layer
  ↓
Middle Layer
  ↓
Detailed Graph
  ↓
Nearest Vectors
```
Important trade-offs include:
```text
Search speed
Recall
Memory usage
Index build time
```
## 12. IVF
IVF, or Inverted File Index, partitions vectors into groups or clusters.
A simplified flow:
```text
All vectors
 ↓
Clusters
 ↓
Search relevant clusters
 ↓
Candidate vectors
 ↓
Nearest results
```
Searching fewer clusters can improve speed but may reduce recall if the correct cluster is missed.
## 13. Product Quantization
Product Quantization reduces vector storage requirements by representing vectors using compressed representations.
Potential benefits:
```text
Lower memory usage
Smaller indexes
Potentially faster search
```
Trade-offs can include reduced accuracy depending on compression settings.
## 14. Top-K Search
A query can request the K most similar vectors.
```text
Query
 ↓
Vector search
 ↓
Top-K results
```
Example:
```text
K = 5
→ Return five highest-ranked candidates
```
K should be selected using evaluation and downstream requirements.
## 15. Metadata
Vector records often contain metadata alongside embeddings.
Example:
```json
{
  "document_id": "DOC-1004",
  "department": "engineering",
  "product": "backup",
  "year": 2026
}
```
Metadata enables filtering and improves retrieval control.
## 16. Metadata Filtering
A query can combine semantic similarity with metadata conditions.
Example:
```text
Semantic query:
"How do I restore a failed backup?"

Filter:
product = "backup"
department = "engineering"
```
This can reduce irrelevant results and support access control.
## 17. Pre-Filtering vs Post-Filtering
### Pre-Filtering
Apply filters before similarity search or as part of candidate selection.
Potential advantage:
```text
Search only allowed / relevant records
```
### Post-Filtering
Perform similarity search and filter results afterward.
Potential problem:
```text
Top-K candidates may disappear after filtering
```
The exact behavior depends on the database.
## 18. Hybrid Search
Hybrid search combines lexical and semantic retrieval.
```text
Keyword Search
      +
Vector Search
      ↓
Combined Results
```
This is useful when both exact terms and semantic meaning matter.
Examples:
```text
Product names
Error codes
Ticket IDs
Technical terminology
Natural-language questions
```
## 19. Sparse vs Dense Retrieval
### Sparse Retrieval
Represents text using sparse term-based signals.
Examples:
```text
Keyword matching
BM25-style retrieval
```
### Dense Retrieval
Uses dense vectors from embedding models.
```text
Text
 ↓
Embedding
 ↓
Dense vector
```
Hybrid systems can combine both.
## 20. Vector Database vs Traditional Database
A traditional relational database is optimized for structured data and operations such as:
```text
Transactions
Joins
Constraints
Structured queries
```
A vector database is optimized for operations such as:
```text
Vector similarity
Nearest-neighbor search
Embedding storage
Semantic retrieval
```
Many production systems use both rather than replacing one with the other.
## 21. Vector Database vs Search Engine
Search engines commonly excel at:
```text
Keyword search
Filtering
Text ranking
Faceting
```
Vector databases or vector-search engines excel at:
```text
Semantic similarity
Nearest-neighbor retrieval
Embedding-based search
```
Modern systems can support both approaches.
## 22. RAG Architecture with Vector Search
```text
Documents
 ↓
Chunking
 ↓
Embedding Model
 ↓
Vector Index
 ↓
                         User Question
                              ↓
                         Query Embedding
                              ↓
                         Vector Search
                              ↓
                         Top-K Chunks
                              ↓
                         Reranking
                              ↓
                         Context
                              ↓
                            LLM
                              ↓
                           Answer
```
## 23. Query Embeddings
For semantic retrieval, the user query is usually converted into an embedding using a compatible embedding model.
```text
User Query
 ↓
Embedding Model
 ↓
Query Vector
 ↓
Similarity Search
```
The query and stored content need compatible representations.
## 24. Embedding Model Selection
Consider:
```text
Retrieval quality
Vector dimensions
Latency
Cost
Language support
Domain performance
Deployment requirements
```
Do not choose an embedding model only because it has more dimensions.
## 25. Dimension Size
An embedding dimension is the number of numerical components in the vector.
Example:
```text
[0.12, -0.34, 0.81, ...]
        ↑
      dimensions
```
Higher dimensionality can increase storage and compute requirements. More dimensions do not automatically mean better retrieval.
## 26. Indexing Trade-Offs
Vector indexes balance:
```text
Recall
Latency
Memory
Build time
Update cost
```
A production system should tune these based on measured workload requirements.
## 27. Update Patterns
Vector data can be:
```text
Static
Batch updated
Incrementally updated
Near-real-time
```
For frequently changing data, the ingestion pipeline may need:
```text
Change detection
Re-embedding
Upserts
Deletes
Index updates
Versioning
```
## 28. Deletes and Stale Data
A production retrieval system needs a strategy for deleted or replaced documents.
Potential mechanisms:
```text
Soft delete
Hard delete
Tombstones
Version metadata
Index refresh
```
A deleted document should not remain retrievable because of a stale index.
## 29. Multi-Tenant Vector Search
Enterprise systems may store data for multiple customers or organizational units.
A common pattern is:
```text
Tenant ID
 ↓
Authorization
 ↓
Metadata filter / namespace
 ↓
Vector search
```
Never rely only on a model prompt to isolate tenants.
## 30. Security
Important security concerns include:
```text
Unauthorized retrieval
Cross-tenant data leakage
Sensitive metadata
Malicious documents
Prompt injection
Embedding data exposure
Weak access controls
```
Authorization should be enforced at the application and retrieval layers.
## 31. Vector Search and Prompt Injection
Retrieved documents may contain malicious instructions.
```text
Vector Search
 ↓
Malicious Document
 ↓
LLM Context
 ↓
Potential Prompt Injection
```
Retrieved content should be treated as untrusted data, not trusted instructions.
## 32. Retrieval Quality
Important factors include:
```text
Chunking
Embedding model
Similarity metric
Index configuration
Metadata filters
Query formulation
Top-K
Reranking
Source quality
```
A poor retrieval pipeline can produce poor RAG answers even with a highly capable LLM.
## 33. Retrieval Metrics
Useful metrics include:
```text
Recall@K
Precision@K
MRR
NDCG
```
### Recall@K
Measures whether relevant items appear within the top K retrieved results.
### Precision@K
Measures how many of the top K retrieved results are relevant.
### MRR
Mean Reciprocal Rank emphasizes the position of the first relevant result.
### NDCG
Normalized Discounted Cumulative Gain evaluates ranked relevance while giving greater importance to higher-ranked results.
## 34. Reranking
A vector search system may retrieve a broader candidate set and then use a reranker.
```text
Query
 ↓
Vector / Hybrid Search
 ↓
Top-N Candidates
 ↓
Reranker
 ↓
Top-K Context
```
This can improve relevance at the cost of additional processing.
## 35. Filtering and Search Quality
Filters can improve precision:
```text
product = "veeam"
document_type = "manual"
version = "latest"
```
But overly restrictive filters can reduce recall.
The filter strategy should therefore be evaluated with realistic queries.
## 36. Scaling
At larger scale, consider:
```text
Sharding
Replication
Partitioning
Index memory
Query concurrency
Storage
Caching
Load balancing
```
Scaling strategy depends on:
```text
Vector count
Dimension
Query rate
Update rate
Latency target
Availability requirement
```
## 37. High Availability
Production systems may require:
```text
Replication
Multiple nodes
Failover
Backups
Disaster recovery
Monitoring
```
The exact architecture depends on the selected database or search platform.
## 38. Latency Optimization
If vector search is slow, investigate:
```text
Index configuration
Number of candidates
Metadata filtering
Vector dimensions
Hardware
Network latency
Query concurrency
Reranking
```
Measure each stage before optimizing.
## 39. Cost Optimization
Potential approaches:
```text
Efficient embedding models
Appropriate vector dimensions
Compression
Index tuning
Caching
Batch ingestion
Selective reranking
Storage lifecycle policies
```
Optimize based on workload rather than reducing quality blindly.
## 40. Common Vector Database Failure Modes
| Failure | Likely Cause |
|---|---|
| Irrelevant results | Poor embeddings or chunking |
| Correct document missing | Low recall |
| Slow search | Index or hardware bottleneck |
| Too many irrelevant results | Poor precision |
| Results disappear after filtering | Overly restrictive filter |
| Old content returned | Stale index |
| Cross-tenant data | Broken authorization |
| High storage usage | Large vectors / duplication |
| High ingestion cost | Excessive re-embedding |
## 41. Common Interview Questions
### What is a vector database?
A system optimized for storing vector representations and performing efficient similarity search.
### Why use a vector database in RAG?
It provides a mechanism for finding semantically relevant document chunks that can be supplied to an LLM.
### What is an embedding?
A numerical representation of data that captures useful relationships for tasks such as similarity search.
### What is cosine similarity?
A similarity measure based on the angle between two vectors.
### What is HNSW?
A graph-based approximate nearest-neighbor indexing approach designed for efficient similarity search.
### What is hybrid search?
Combining lexical and semantic retrieval signals.
### Why use metadata?
To filter results, improve precision and support access-control requirements.
### What is Top-K?
The number of highest-ranked candidates returned from a retrieval operation.
### Why use a reranker?
To improve the relevance ordering of an initial candidate set.
### Can a vector database replace a relational database?
Usually not. They solve different problems and are often used together.
## 42. Senior-Level Scenarios
### Retrieval quality is poor. What do you check first?
Start with the evaluation dataset. Then inspect document extraction, chunking, embedding quality, similarity metric, index configuration, filters, query formulation and reranking.
### Search is fast but RAG answers are wrong. What does that tell you?
Fast retrieval does not imply relevant retrieval. Check recall, precision and whether the correct chunk reaches the LLM.
### The vector index is growing rapidly. What do you investigate?
Check document duplication, chunk count, embedding dimensions, metadata overhead, update patterns, stale records and compression options.
### A customer can retrieve another customer's documents. What should you do?
Treat it as a critical authorization failure. Enforce tenant-aware access controls before retrieval and verify isolation at the database or index layer.
### Would you always use a vector database for RAG?
No. The retrieval technology should match the data and query requirements. Traditional search, SQL, APIs or hybrid systems may be more appropriate for some workloads.
## 43. Production Checklist
```text
☐ Document ingestion
☐ Text extraction
☐ Chunking strategy
☐ Metadata
☐ Embedding model
☐ Similarity metric
☐ Vector index
☐ Top-K tuning
☐ Reranking if needed
☐ Hybrid search if needed
☐ Access control
☐ Tenant isolation
☐ Freshness
☐ Deletes / updates
☐ Evaluation dataset
☐ Retrieval metrics
☐ Latency monitoring
☐ Cost monitoring
☐ Backup / recovery
☐ High availability
```
## 44. Vector Database Interview Mental Model
Remember:
```text
DATA
 ↓
CHUNK
 ↓
EMBED
 ↓
INDEX
 ↓
QUERY
 ↓
RETRIEVE
 ↓
FILTER
 ↓
RERANK
 ↓
CONTEXT
 ↓
LLM
```
For production:
```text
SECURITY
FRESHNESS
SCALABILITY
LATENCY
COST
OBSERVABILITY
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
- [Gen AI Architecture](/interview-prep/gen-ai/gen-ai-architecture/)
- [Troubleshooting](/interview-prep/gen-ai/troubleshooting/)
- [Senior-Level Scenarios](/interview-prep/gen-ai/senior-scenarios/)
- [Deep Dive](/interview-prep/gen-ai/deep-dive/)
