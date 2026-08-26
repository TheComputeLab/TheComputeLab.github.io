---
title: ""
description: "Interview-focused preparation for LLM fundamentals, Transformers, attention, tokenization, training, inference, decoding, context windows, model selection and production considerations."
weight: 40
toc: true
---
# 🧠 Large Language Models
This guide focuses on the LLM topics most commonly discussed in technical interviews, from the basic architecture to production engineering decisions.
## 1. What Is an LLM?
A Large Language Model is a machine-learning model trained on large amounts of data to process and generate language.
A simplified flow is:
```text
Text
 ↓
Tokenization
 ↓
Token Embeddings
 ↓
Transformer Layers
 ↓
Output Probabilities
 ↓
Generated Tokens
```
### Interview Answer
> "An LLM processes tokenized input using learned representations, commonly through Transformer-based architecture, and generates output tokens based on the context and learned probability distributions."
## 2. Why Are LLMs Called "Large"?
"Large" can refer to factors such as:
```text
Number of parameters
Training data
Training compute
Model architecture
Model capability
```
Parameter count alone does not completely determine model quality.
## 3. Tokens
LLMs generally operate on tokens rather than raw words.
A token can represent:
```text
A complete word
Part of a word
Punctuation
Whitespace or formatting
```
Example:
```text
"unbelievable"
       ↓
possible token pieces
       ↓
"un" + "believ" + "able"
```
Exact tokenization depends on the tokenizer.
### Why Tokens Matter
Tokens affect:
- Context-window usage
- Input and output cost
- Latency
- Throughput
- Maximum prompt size
## 4. Tokenization
Tokenization converts text into a sequence of tokens that the model can process.
```text
Raw text
 ↓
Tokenizer
 ↓
Token IDs
 ↓
Model
```
The tokenizer and model are generally designed to work together.
## 5. Embeddings
Token IDs are mapped to numerical vectors called embeddings.
```text
Token ID
 ↓
Embedding lookup
 ↓
Vector representation
```
Embeddings allow the neural network to work with numerical representations rather than raw text.
## 6. Transformer Architecture
Modern LLMs commonly use Transformer-based architectures.
A simplified Transformer flow is:
```text
Input Tokens
 ↓
Token Embeddings
 ↓
Positional Information
 ↓
Transformer Blocks
 ↓
Output Representation
 ↓
Vocabulary Probabilities
```
A Transformer block commonly contains:
```text
Self-Attention
+
Feed-Forward Network
+
Normalization
+
Residual Connections
```
Exact implementation varies between model architectures.
## 7. Self-Attention
Self-attention allows tokens to incorporate information from other relevant tokens in the sequence.
Conceptually:
```text
Input tokens
     ↓
Determine relationships
     ↓
Assign attention weights
     ↓
Build contextual representations
```
### Interview Answer
> "Self-attention allows each token representation to incorporate information from other tokens, helping the model understand relationships and context within the sequence."
## 8. Query, Key and Value
Attention is commonly described using three representations:
```text
Query
Key
Value
```
A simplified conceptual flow:
```text
Query
  ↓
Compare with Keys
  ↓
Attention Scores
  ↓
Weighted Values
  ↓
Contextual Representation
```
The standard attention operation is often expressed as:
```text
Attention(Q, K, V) = softmax(QKᵀ / √dₖ)V
```
For interviews, understand what each component represents rather than memorizing the equation without context.
## 9. Multi-Head Attention
Instead of performing one attention operation, Transformer architectures can use multiple attention heads.
```text
Input
 ↓
Head 1
Head 2
Head 3
...
Head N
 ↓
Combine representations
```
Different heads can learn different relationships or patterns.
## 10. Positional Information
Self-attention by itself does not inherently provide the model with sequence order in the same way a sequential model does.
Transformer architectures therefore use mechanisms that provide positional information.
Depending on the model, this may involve:
```text
Positional embeddings
Rotary positional embeddings
Other positional encoding techniques
```
### Interview Question
**Why does a Transformer need positional information?**
Because token order matters in language, and the model needs information about where tokens occur in the sequence.
## 11. Causal Language Modeling
Many autoregressive LLMs generate text one token at a time.
```text
"The system is"
        ↓
predict next token
        ↓
"working"
        ↓
predict next token
        ↓
"correctly"
```
During generation, the model should not use future tokens that have not yet been generated.
This is commonly implemented using causal masking.
## 12. Next-Token Prediction
A simplified generation process:
```text
Input tokens
 ↓
Model
 ↓
Probability distribution
 ↓
Select next token
 ↓
Append token
 ↓
Run model again
 ↓
Repeat
```
This repeated process is called autoregressive generation.
## 13. Logits
The model produces numerical scores called logits before probabilities are calculated.
Conceptually:
```text
Hidden representation
 ↓
Output layer
 ↓
Logits
 ↓
Softmax / decoding process
 ↓
Token probabilities
```
A logit is not itself a probability.
## 14. Softmax
Softmax converts a set of scores into a probability distribution.
Conceptually:
```text
Logits
 ↓
Softmax
 ↓
Probabilities
```
The probabilities represent the model's relative preference for possible next tokens.
## 15. Decoding
Decoding determines how the next token is selected from the model's output distribution.
Common approaches:
```text
Greedy decoding
Sampling
Temperature
Top-k
Top-p
```
## 16. Temperature
Temperature changes the shape of the probability distribution used during sampling.
Generally:
```text
Lower temperature
→ more deterministic / conservative

Higher temperature
→ more varied / diverse
```
Temperature does not make a model "more intelligent."
## 17. Top-k
Top-k sampling limits candidate selection to the k most likely tokens before sampling.
Conceptually:
```text
All possible tokens
 ↓
Select top K
 ↓
Sample
```
## 18. Top-p
Top-p, also called nucleus sampling, selects from the smallest set of tokens whose cumulative probability reaches a specified threshold.
Conceptually:
```text
All tokens
 ↓
Build probability distribution
 ↓
Select probability mass up to P
 ↓
Sample
```
## 19. Context Window
The context window is the amount of tokenized information a model can process for a request.
It may include:
```text
System instructions
User prompt
Conversation history
Retrieved documents
Tool results
Structured context
```
### Important Point
A larger context window does not automatically mean better answers.
Large contexts can affect:
```text
Cost
Latency
Retrieval strategy
Attention / processing requirements
```
## 20. Inference
Inference is the process of running a trained model to produce an output.
```text
Prompt
 ↓
Tokenization
 ↓
Model computation
 ↓
Logits
 ↓
Decoding
 ↓
Generated tokens
```
## 21. Training vs Inference
### Training
```text
Training data
 ↓
Model
 ↓
Loss
 ↓
Parameter updates
 ↓
Repeat
```
### Inference
```text
Input
 ↓
Trained model
 ↓
Output
```
Training changes model parameters. Inference normally does not.
## 22. Pretraining
Pretraining is the broad initial training phase where a model learns general patterns from large datasets.
It can establish capabilities such as:
```text
Language patterns
Syntax
Semantics
General knowledge patterns
Code patterns
Reasoning-related behavior
```
The exact capabilities depend on training data, architecture and training methods.
## 23. Fine-Tuning
Fine-tuning continues training a pretrained model using additional data.
It can be used for:
```text
Task adaptation
Instruction following
Behavior adaptation
Domain specialization
Output style
```
### Interview Tip
If the problem is frequently changing external knowledge, investigate RAG before assuming fine-tuning is the right solution.
## 24. Instruction Tuning
Instruction tuning trains a model on examples of instructions and desired responses so it becomes better at following user instructions.
A simplified idea:
```text
Instruction
+
Expected response
 ↓
Additional training
 ↓
Better instruction following
```
## 25. Alignment
Alignment broadly refers to methods intended to make model behavior better match desired human or application objectives.
Interview discussions may include:
```text
Human feedback
Preference data
Safety training
Instruction tuning
Reward modeling
Policy constraints
```
Avoid treating alignment as one single algorithm.
## 26. Hallucination
LLMs can generate plausible but unsupported information.
Possible causes include:
```text
Missing context
Ambiguous prompts
Insufficient grounding
Retrieval failure
Model limitations
Probabilistic generation
```
Mitigation:
```text
RAG
Tool use
Grounding
Structured outputs
Validation
Evaluation
Human review
```
## 27. LLM vs Search Engine
```text
Search engine
→ Finds and ranks external information.

LLM
→ Generates responses from learned model representations
  and supplied context.
```
Production applications can combine both:
```text
Search / Retrieval
 ↓
Relevant information
 ↓
LLM
 ↓
Grounded response
```
## 28. LLM vs RAG
An LLM by itself:
```text
Prompt
 ↓
Model
 ↓
Response
```
An LLM with RAG:
```text
Prompt
 ↓
Retrieve relevant information
 ↓
Add context
 ↓
Model
 ↓
Grounded response
```
## 29. LLM vs Agent
```text
LLM
→ Generates or transforms information.

Agent
→ Uses an LLM within a workflow that can select actions,
  call tools, observe results and continue toward a goal.
```
## 30. Model Selection
When selecting an LLM for production, consider:
```text
Task quality
Latency
Cost
Context requirements
Tool support
Structured output support
Security / privacy requirements
Deployment options
Reliability
Evaluation results
```
Do not choose solely by parameter count or benchmark score.
## 31. Smaller vs Larger Models
### Smaller Model
Potential advantages:
```text
Lower cost
Lower latency
Higher throughput
Easier deployment
```
### Larger Model
Potential advantages:
```text
Higher capability for difficult tasks
More complex reasoning or generation
Potentially better performance on selected workloads
```
The correct choice depends on measured requirements.
## 32. Quantization
Quantization reduces numerical precision used to represent model parameters or computations.
Potential benefits:
```text
Lower memory usage
Lower hardware requirements
Potentially faster inference
Lower deployment cost
```
Trade-offs can include reduced model quality depending on the quantization method and workload.
## 33. Batch vs Streaming Inference
### Batch
Process multiple requests or inputs together.
Useful for:
```text
Offline workloads
High throughput
Large processing jobs
```
### Streaming
Return generated output incrementally.
Useful for:
```text
Interactive applications
Chat interfaces
Improved perceived latency
```
## 34. LLM Production Metrics
Important metrics include:
```text
Latency
Time to first token
Tokens per second
Throughput
Token usage
Cost per request
Error rate
Quality
Grounding
Safety
User satisfaction
```
## 35. Common LLM Failure Modes
| Failure | Possible Cause |
|---|---|
| Hallucination | Missing or unreliable context |
| Wrong answer | Poor reasoning or insufficient information |
| Truncated response | Context/output limits |
| High latency | Large model, long context or infrastructure bottleneck |
| High cost | Excessive token usage or expensive model |
| Inconsistent output | Sampling or prompt variability |
| Poor retrieval | Weak embedding/retrieval pipeline |
| Tool failure | Invalid arguments, timeout or authorization issue |
## 36. Common Interview Questions
### Why do LLMs use tokens?
Tokens provide a numerical representation that the model can process and are also useful for measuring context, compute usage and API cost.
### What is self-attention?
Self-attention lets token representations incorporate information from other tokens in the sequence according to learned attention weights.
### What are Query, Key and Value?
They are representations used by the attention mechanism to compare a query with keys and use the resulting weights to combine values.
### Why is positional information needed?
Because language depends on token order and the model needs information about sequence position.
### What is causal masking?
A mechanism that prevents an autoregressive model from using future tokens when predicting the next token.
### What is a logit?
A raw numerical score produced before converting candidate outputs into probabilities.
### What is temperature?
A generation parameter that changes the distribution used during sampling and therefore influences output variability.
### What is the context window?
The bounded amount of tokenized context a model can process for a request.
### What is fine-tuning?
Additional training of a pretrained model to adapt it to particular tasks, behaviors or domains.
### What is quantization?
Reducing numerical precision to decrease memory requirements and potentially improve inference efficiency.
## 37. Senior-Level Interview Questions
### How would you choose an LLM for production?
Start with requirements and evaluation criteria, then compare candidate models using representative workloads. Consider quality, latency, cost, context, tool support, security and deployment constraints.
### How would you reduce LLM latency?
Investigate model size, prompt length, context size, retrieval latency, tool latency, batching, caching, streaming and infrastructure before changing components.
### How would you reduce LLM cost?
Reduce unnecessary tokens, optimize prompts and retrieved context, use appropriate model sizes, cache repeated work and route workloads to models based on task complexity.
### How would you troubleshoot poor LLM output?
Separate the problem into prompt quality, context quality, retrieval quality, model capability, generation configuration and application logic. Use representative test cases and evaluation data rather than relying on one example.
## 38. LLM Interview Mental Model
When asked an LLM question, think:
```text
TOKENS
 ↓
EMBEDDINGS
 ↓
TRANSFORMER
 ↓
ATTENTION
 ↓
CONTEXT
 ↓
LOGITS
 ↓
DECODING
 ↓
OUTPUT
```
For production questions, extend it:
```text
MODEL
 ↓
APPLICATION
 ↓
RAG / TOOLS
 ↓
EVALUATION
 ↓
SECURITY
 ↓
OBSERVABILITY
 ↓
COST + LATENCY
```
## 39. Continue the Preparation
Use the other landing-page tiles for the surrounding GenAI topics:
- [Quick Start](/interview-prep/gen-ai/quick-start/)
- [Rapid Revision](/interview-prep/gen-ai/rapid-revision/)
- [Core Concepts](/interview-prep/gen-ai/core-concepts/)
- [Prompt Engineering](/interview-prep/gen-ai/prompt-engineering/)
- [RAG](/interview-prep/gen-ai/rag/)
- [AI Agents](/interview-prep/gen-ai/ai-agents/)
- [Vector Databases](/interview-prep/gen-ai/vector-databases/)
- [Gen AI Architecture](/interview-prep/gen-ai/gen-ai-architecture/)
- [Troubleshooting](/interview-prep/gen-ai/troubleshooting/)
- [Senior-Level Scenarios](/interview-prep/gen-ai/senior-scenarios/)
- [Deep Dive](/interview-prep/gen-ai/deep-dive/)
