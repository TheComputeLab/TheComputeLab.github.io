---
title: ""
description: "Core Generative AI concepts for technical interviews covering models, tokens, transformers, embeddings, inference, context, prompting and evaluation."
weight: 30
toc: true
---
# 🎯 Gen AI Core Concepts
This page builds the conceptual foundation needed before moving into LLMs, RAG, agents and production GenAI architecture.
## 1. What Is Generative AI?
Generative AI refers to AI systems that can create new content based on patterns learned from data.
Common output types include:
```text
Text
Code
Images
Audio
Video
Structured data
```
A useful interview distinction:
```text
Traditional AI
→ Predict / classify / detect

Generative AI
→ Generate new content
```
## 2. Foundation Models
A foundation model is a broadly trained model that can be adapted or used for many downstream tasks.
Examples of capabilities include:
```text
Language understanding
Text generation
Code generation
Summarization
Classification
Question answering
```
### Interview Question
**What is a foundation model?**
A strong answer:
> "A foundation model is trained on broad data at scale and can serve as a base for many downstream applications or specialized tasks."
## 3. Large Language Models
An LLM is a model designed to process and generate language.
A simplified view:
```text
Training Data
     ↓
Model Training
     ↓
Learned Parameters
     ↓
LLM
     ↓
Input Context
     ↓
Generated Output
```
Important topics:
```text
Tokens
Embeddings
Attention
Transformers
Context
Inference
Sampling
```
## 4. Tokens
Language models generally process tokenized input rather than raw characters or complete words.
A token may represent:
```text
A word
Part of a word
Punctuation
Whitespace / formatting
```
Example:
```text
"Generative AI"
        ↓
Tokenizer
        ↓
Token sequence
```
The exact tokenization depends on the model and tokenizer.
### Why Tokens Matter
Tokens affect:
```text
Context limits
Input cost
Output cost
Latency
Model capacity
```
### Interview Question
**Why do LLM APIs charge based on tokens?**
Because tokenized input and output are the units processed by the model and are useful for measuring computational usage.
## 5. Embeddings
An embedding converts information such as text into a numerical vector representation.
Conceptually:
```text
Text
 ↓
Embedding Model
 ↓
Vector
```
Semantically related content tends to have related representations in vector space.
Common applications:
```text
Semantic search
RAG
Recommendation
Clustering
Similarity detection
Classification
```
## 6. Context
Context is the information supplied to a model along with the current request.
It can include:
```text
System instructions
User request
Conversation history
Retrieved documents
Tool results
Structured application data
```
A useful mental model:
```text
Model capability
+
Context
+
Instructions
=
Application behavior
```
## 7. Context Window
A context window is the amount of tokenized information a model can process for a request.
It may include both input context and generated output depending on the model and API.
### Why Context Windows Matter
A larger context window can allow more information to be supplied, but large contexts can also affect:
```text
Latency
Cost
Retrieval strategy
Application complexity
```
More context does not automatically mean better answers.
## 8. Transformer Architecture
Modern LLMs commonly use Transformer-based architectures.
The Transformer introduced an architecture centered around attention mechanisms that allow the model to process relationships between tokens efficiently.
Simplified view:
```text
Input Tokens
     ↓
Embeddings
     ↓
Transformer Layers
     ↓
Attention + Neural Network Processing
     ↓
Output Representation
     ↓
Generated Tokens
```
For interviews, be prepared to explain attention at a high level before going into mathematical detail.
## 9. Attention
Attention allows a model to determine which parts of the available context are important when processing a token or sequence.
A simplified explanation:
```text
Token
 ↓
Look at relevant context
 ↓
Assign importance
 ↓
Build contextual representation
```
### Interview Question
**Why is attention important?**
It allows the model to capture relationships between different parts of the input sequence and build context-aware representations.
## 10. Training vs Inference
These are different phases.
### Training
```text
Data
 ↓
Forward computation
 ↓
Loss
 ↓
Parameter updates
 ↓
Repeated training
```
### Inference
```text
Input
 ↓
Trained model
 ↓
Prediction / generation
 ↓
Output
```
Training creates or adjusts model parameters. Inference uses the trained model to produce results.
## 11. Pretraining
Pretraining is the broad initial training phase in which a model learns general patterns from a large dataset.
The model learns statistical relationships that can later support many downstream capabilities.
## 12. Fine-Tuning
Fine-tuning continues training a pretrained model on additional data for a specific purpose.
Typical goals:
```text
Task adaptation
Behavior adaptation
Domain specialization
Output style
Instruction following
```
### Important Interview Distinction
Do not say:
> "Fine-tuning adds the latest company documents to the model."
Instead:
> "Fine-tuning adapts model behavior or capabilities using additional training. For frequently changing external knowledge, retrieval-based approaches are often more appropriate."
## 13. Inference
Inference is the process of using a trained model to produce an output from an input.
For an LLM:
```text
Prompt
 ↓
Tokenization
 ↓
Model computation
 ↓
Next-token probabilities
 ↓
Token selection
 ↓
Repeated generation
 ↓
Output
```
## 14. Sampling
An LLM can produce different outputs from the same prompt depending on the generation configuration and sampling strategy.
Important concepts include:
```text
Temperature
Top-k
Top-p
Greedy decoding
Sampling
```
### Temperature
In general:
```text
Lower temperature
→ more predictable / conservative generation

Higher temperature
→ more variation / randomness
```
The exact effect depends on the model and implementation.
## 15. Hallucinations
A hallucination is an unsupported, incorrect or fabricated model output presented as an answer.
Possible causes include:
```text
Missing information
Ambiguous instructions
Insufficient context
Poor retrieval
Model limitations
Probabilistic generation
```
Mitigation strategies:
```text
Grounding
RAG
Tool use
Structured outputs
Validation
Evaluation
Human review
```
## 16. Grounding
Grounding connects model responses to trusted information.
Examples:
```text
Enterprise documents
Databases
APIs
Search systems
Knowledge bases
Tool results
```
RAG is one common grounding architecture.
## 17. Prompt Engineering
Prompt engineering is the design and refinement of instructions and context supplied to a model.
A practical structure:
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
Useful techniques include:
```text
Zero-shot
Few-shot
Structured output
Examples
Explicit constraints
Prompt templates
```
## 18. Structured Output
Structured output constrains a model response to an expected format.
Example:
```json
{
  "priority": "high",
  "category": "backup",
  "action": "investigate_repository"
}
```
This is useful when model output is consumed by software rather than directly by a human.
## 19. Function Calling / Tool Calling
Tool calling allows an application to expose defined functions or external capabilities to a model-driven workflow.
Examples:
```text
Database query
Search
Calculator
REST API
Ticket creation
Cloud operation
Internal application
```
A simplified flow:
```text
User
 ↓
Model
 ↓
Tool selection
 ↓
Application executes tool
 ↓
Tool result
 ↓
Model
 ↓
Final response
```
The application should validate tool arguments and enforce authorization rather than blindly trusting model-generated requests.
## 20. AI Agents
An agent is a model-driven application pattern capable of selecting actions and using tools as part of completing a task.
Simplified loop:
```text
Goal
 ↓
Model
 ↓
Action
 ↓
Tool
 ↓
Observation
 ↓
Model
 ↓
Next action / answer
```
An agent is more than simply putting an LLM behind a chat interface.
## 21. RAG
Retrieval-Augmented Generation combines retrieval with generation.
Basic flow:
```text
Documents
 ↓
Chunking
 ↓
Embeddings
 ↓
Index
 ↓
Retrieve relevant content
 ↓
Add context
 ↓
LLM
 ↓
Answer
```
RAG is particularly useful when an application needs external, domain-specific or changing information.
## 22. Evaluation
GenAI applications need systematic evaluation because output quality is not always deterministic.
Evaluate dimensions such as:
```text
Correctness
Relevance
Grounding
Completeness
Safety
Latency
Cost
Consistency
```
Evaluation can use:
```text
Human review
Test datasets
Automated metrics
Model-based evaluation
Production feedback
```
## 23. Guardrails
Guardrails are controls that constrain or validate application behavior.
Examples:
```text
Input validation
Output validation
Content filtering
Tool authorization
Schema validation
Access control
Prompt-injection defenses
Rate limits
```
## 24. Security Concepts
Important GenAI security topics include:
```text
Prompt injection
Data leakage
Sensitive information exposure
Insecure tool use
Excessive permissions
Untrusted retrieved content
Model output validation
```
A strong production design treats model output as untrusted input when it can influence software actions.
## 25. Cost and Latency
GenAI systems need explicit cost and latency management.
Consider:
```text
Model choice
Token usage
Context size
Caching
Batching
Streaming
Retrieval latency
Tool latency
Request volume
```
A larger or more capable model is not automatically the best production choice.
## 26. Core Interview Questions
### What is the difference between AI and Generative AI?
AI is the broader field. Generative AI focuses specifically on systems that create new content.
### Why are tokens important?
Tokens are the units processed by language models and affect context limits, cost and latency.
### What is an embedding?
A numerical representation that captures useful semantic relationships for tasks such as similarity search and retrieval.
### What is attention?
An operation that allows the model to weigh relationships between elements of the input context when building representations.
### What is inference?
Using a trained model to produce an output for a given input.
### What is the difference between training and inference?
Training adjusts model parameters using data; inference uses the trained parameters to generate predictions or outputs.
### What is grounding?
Connecting model responses to trusted external information.
### Why does RAG help?
It provides relevant external context to the model without requiring the base model itself to contain all of that knowledge.
### What is the difference between RAG and fine-tuning?
RAG supplies information at inference time; fine-tuning changes model behavior through additional training.
### Why are guardrails important?
Because model outputs can be incorrect, unsafe or capable of influencing downstream systems. Guardrails provide validation and control.
## 27. Senior Interview Mental Model
When explaining a GenAI system, connect the concepts:
```text
BUSINESS PROBLEM
 ↓
DATA
 ↓
MODEL
 ↓
PROMPT / CONTEXT
 ↓
RAG / TOOLS IF REQUIRED
 ↓
OUTPUT
 ↓
EVALUATION
 ↓
SECURITY
 ↓
OBSERVABILITY
 ↓
COST + LATENCY
 ↓
PRODUCTION
```
Do not start with the model. Start with the problem the system needs to solve.
## 28. Continue the Preparation
Use the landing-page tiles for deeper preparation:
- [Quick Start](/interview-prep/gen-ai/quick-start/)
- [Rapid Revision](/interview-prep/gen-ai/rapid-revision/)
- [Large Language Models](/interview-prep/gen-ai/llm/)
- [Prompt Engineering](/interview-prep/gen-ai/prompt-engineering/)
- [RAG](/interview-prep/gen-ai/rag/)
- [AI Agents](/interview-prep/gen-ai/ai-agents/)
- [Vector Databases](/interview-prep/gen-ai/vector-databases/)
- [Gen AI Architecture](/interview-prep/gen-ai/gen-ai-architecture/)
- [Troubleshooting](/interview-prep/gen-ai/troubleshooting/)
- [Senior-Level Scenarios](/interview-prep/gen-ai/senior-scenarios/)
- [Deep Dive](/interview-prep/gen-ai/deep-dive/)
