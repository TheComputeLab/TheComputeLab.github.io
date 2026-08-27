---
title: "LLMs & Generative AI"
description: "A practical knowledge base covering the history, architecture, training, inference, setup and real-world use of Large Language Models and Generative AI."
weight: 20
toc: false
---

<section class="llm-lab-page">

<!-- HERO -->
<section class="llm-lab-hero">

<div class="llm-lab-status">
<span class="llm-lab-status-dot"></span>
LLM & GENERATIVE AI KNOWLEDGE BASE
</div>

<h1 class="llm-lab-title">
LLMs & <span>Generative AI</span>
</h1>

<p class="llm-lab-subtitle">
Understand where modern language models came from, how they work,
how Generative AI creates content, how to run models locally or through APIs,
and how these technologies become real applications.
</p>

<div class="llm-lab-terminal">
<span>$</span>
<strong>initialize_llm_knowledge_base()</strong>
<i></i>
</div>

</section>


<!-- LEARNING PATH -->
<section class="llm-lab-pipeline">

<div><span>01</span><strong>HISTORY</strong></div>
<div>→</div>
<div><span>02</span><strong>ARCHITECTURE</strong></div>
<div>→</div>
<div><span>03</span><strong>TRAINING</strong></div>
<div>→</div>
<div><span>04</span><strong>INFERENCE</strong></div>
<div>→</div>
<div><span>05</span><strong>APPLICATION</strong></div>

</section>


<!-- WHAT IS LLM -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>01 — FOUNDATION</span>
<h2>What is an <span>LLM?</span></h2>
</div>

<div class="llm-lab-two-column">

<div>
<p>
A Large Language Model is a machine-learning model trained on very large
amounts of text and other data so that it can learn statistical patterns
in language and perform tasks such as generation, summarization,
question answering, translation, classification and code generation.
</p>

<p>
Modern LLMs are commonly built using Transformer-based architectures.
A model does not simply store a database of answers. During inference,
it processes the supplied context and predicts what tokens should come next.
</p>
</div>

<div class="llm-lab-definition-card">
<span>MENTAL MODEL</span>
<strong>INPUT → TOKENS → MODEL → NEXT-TOKEN PROBABILITIES → OUTPUT</strong>
<small>
The generated response is produced token by token.
</small>
</div>

</div>

</section>


<!-- WHAT IS GENERATIVE AI -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>02 — BIGGER PICTURE</span>
<h2>What is <span>Generative AI?</span></h2>
</div>

<p class="llm-lab-wide-text">
Generative AI is the broader family of AI systems that can generate new
content from learned patterns. Depending on the model, the output can be
text, code, images, audio, video, structured data or combinations of these.
</p>

<div class="llm-lab-generation-grid">

<div>
<strong>TEXT</strong>
<small>Stories, answers, summaries, documents</small>
</div>

<div>
<strong>CODE</strong>
<small>Programs, tests, scripts and explanations</small>
</div>

<div>
<strong>IMAGE</strong>
<small>Generated or edited visual content</small>
</div>

<div>
<strong>AUDIO</strong>
<small>Speech, music and sound generation</small>
</div>

<div>
<strong>VIDEO</strong>
<small>Generated or transformed video content</small>
</div>

<div>
<strong>MULTIMODAL</strong>
<small>Systems combining text, image, audio and other inputs</small>
</div>

</div>

<p class="llm-lab-note">
<strong>Key distinction:</strong>
LLMs are one important class of Generative AI systems. Generative AI is
the larger category.
</p>

</section>


<!-- HISTORY -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>03 — HISTORY</span>
<h2>How did we get to <span>LLMs?</span></h2>
</div>

<div class="llm-lab-timeline">

<div>
<span>1950s–1980s</span>
<strong>Early AI & Language Processing</strong>
<p>
Rule-based systems, symbolic AI and early statistical approaches
established the foundations for machine language processing.
</p>
</div>

<div>
<span>1990s–2000s</span>
<strong>Statistical NLP</strong>
<p>
Probabilistic language models, n-grams and statistical machine translation
became important approaches for modelling language.
</p>
</div>

<div>
<span>2013</span>
<strong>Word Embeddings</strong>
<p>
Techniques such as Word2Vec helped represent words as vectors so that
semantic relationships could be learned from data.
</p>
</div>

<div>
<span>2014–2016</span>
<strong>Sequence Models</strong>
<p>
RNNs, LSTMs, GRUs and encoder-decoder systems became important for
sequence modelling and machine translation.
</p>
</div>

<div>
<span>2017</span>
<strong>The Transformer</strong>
<p>
The paper "Attention Is All You Need" introduced the Transformer,
an architecture based on attention rather than recurrence or convolution.
</p>
</div>

<div>
<span>2018</span>
<strong>BERT & Modern Pretraining</strong>
<p>
BERT demonstrated the power of large-scale Transformer pretraining
using bidirectional representations for language understanding tasks.
</p>
</div>

<div>
<span>2018 onward</span>
<strong>GPT & Autoregressive Scaling</strong>
<p>
Decoder-based Transformer language models demonstrated that increasing
model scale, data and compute could produce increasingly capable
language generation systems.
</p>
</div>

<div>
<span>2020s</span>
<strong>Foundation Models & Generative AI</strong>
<p>
Large pretrained models evolved into general-purpose foundation models,
instruction-following assistants, multimodal systems and tool-using agents.
</p>
</div>

</div>

</section>


<!-- TRANSFORMER -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>04 — THE TRANSFORMER</span>
<h2>The architecture that changed <span>language AI.</span></h2>
</div>

<div class="llm-lab-architecture">

<div class="llm-architecture-node">
INPUT
<small>TEXT</small>
</div>

<div class="llm-architecture-arrow">↓</div>

<div class="llm-architecture-node">
TOKENIZER
<small>TOKENS + IDS</small>
</div>

<div class="llm-architecture-arrow">↓</div>

<div class="llm-architecture-node highlight">
TRANSFORMER
<small>ATTENTION + FEED FORWARD</small>
</div>

<div class="llm-architecture-arrow">↓</div>

<div class="llm-architecture-node">
OUTPUT
<small>NEXT TOKENS</small>
</div>

</div>

<p class="llm-lab-wide-text">
The Transformer introduced a highly parallelizable architecture based on
attention. Instead of processing a sequence strictly one recurrent step
at a time, attention allows the model to relate different positions in
the sequence when building representations.
</p>

</section>


<!-- ENCODER DECODER -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>05 — TRANSFORMER FAMILIES</span>
<h2>Encoder, Decoder and <span>Encoder-Decoder.</span></h2>
</div>

<div class="llm-lab-three-column">

<div class="llm-family-card">
<span>ENCODER-ONLY</span>
<h3>Understand</h3>
<p>
Designed primarily to build rich representations of input text.
Useful for classification, retrieval and language understanding tasks.
</p>
<div>Example: BERT</div>
</div>

<div class="llm-family-card">
<span>DECODER-ONLY</span>
<h3>Generate</h3>
<p>
Designed for autoregressive generation, predicting the next token
from previous context.
</p>
<div>Examples: GPT-style causal LMs, Llama-family models</div>
</div>

<div class="llm-family-card">
<span>ENCODER-DECODER</span>
<h3>Transform</h3>
<p>
Uses an encoder to represent input and a decoder to generate output.
Useful for sequence-to-sequence tasks.
</p>
<div>Examples: T5-style models</div>
</div>

</div>

</section>


<!-- ATTENTION -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>06 — ATTENTION</span>
<h2>How does <span>attention</span> work?</h2>
</div>

<p class="llm-lab-wide-text">
Attention allows a model to assign different importance to different
parts of the available context when processing a token.
</p>

<div class="llm-attention-flow">

<div>
<strong>QUERY</strong>
<small>What am I looking for?</small>
</div>

<div>×</div>

<div>
<strong>KEY</strong>
<small>What information is available?</small>
</div>

<div>+</div>

<div>
<strong>VALUE</strong>
<small>What information should be retrieved?</small>
</div>

<div>→</div>

<div class="highlight">
<strong>ATTENTION OUTPUT</strong>
<small>Context-aware representation</small>
</div>

</div>

<p class="llm-lab-note">
Self-attention allows tokens within a sequence to interact with other
tokens. Multi-head attention performs this process through multiple
learned attention projections.
</p>

</section>


<!-- TOKENIZATION -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>07 — TOKENIZATION</span>
<h2>Models do not read text as <span>humans do.</span></h2>
</div>

<div class="llm-token-flow">

<div>TEXT</div>
<div>→</div>
<div>TOKENS</div>
<div>→</div>
<div>TOKEN IDS</div>
<div>→</div>
<div>EMBEDDINGS</div>
<div>→</div>
<div>MODEL</div>

</div>

<p class="llm-lab-wide-text">
Tokenization converts text into units that a model can process.
Depending on the tokenizer, these units may represent complete words,
subwords, characters or other pieces of text.
</p>

<div class="llm-token-example">
"The model is learning."
<br><br>
<span>→</span>
<br>
["The", " model", " is", " learning", "."]
<br><br>
<span>→</span>
<br>
[token IDs]
</div>

<p class="llm-lab-note">
Token counts influence context limits, latency and cost in many hosted
LLM systems.
</p>

</section>


<!-- EMBEDDINGS -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>08 — REPRESENTATIONS</span>
<h2>What are <span>embeddings?</span></h2>
</div>

<p class="llm-lab-wide-text">
An embedding represents information as a vector in a numerical space.
Semantically related items can occupy nearby regions of that space,
depending on how the embedding model was trained.
</p>

<div class="llm-embedding-visual">
<span class="point p1">cat</span>
<span class="point p2">dog</span>
<span class="point p3">car</span>
<span class="point p4">truck</span>
<span class="point p5">kitten</span>
</div>

<p class="llm-lab-note">
Embeddings are important in semantic search, retrieval-augmented generation,
recommendation systems, clustering and similarity search.
</p>

</section>


<!-- TRAINING -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>09 — TRAINING</span>
<h2>How is an LLM <span>trained?</span></h2>
</div>

<div class="llm-training-flow">

<div>
<strong>DATA</strong>
<small>Text / code / multimodal data</small>
</div>

<div>↓</div>

<div>
<strong>TOKENIZATION</strong>
<small>Convert data into model inputs</small>
</div>

<div>↓</div>

<div>
<strong>PRETRAINING</strong>
<small>Learn statistical patterns</small>
</div>

<div>↓</div>

<div>
<strong>POST-TRAINING</strong>
<small>Instruction following / alignment</small>
</div>

<div>↓</div>

<div>
<strong>EVALUATION</strong>
<small>Quality, safety and capability testing</small>
</div>

</div>

<h3>Pretraining</h3>

<p class="llm-lab-wide-text">
For causal language models, a common objective is next-token prediction:
given preceding context, predict the next token. Repeating this over
very large datasets allows the model to learn linguistic and other
statistical patterns.
</p>

<h3>Post-training</h3>

<p class="llm-lab-wide-text">
After pretraining, models can undergo additional training to improve
instruction following, task performance, safety and conversational behavior.
The exact methods vary by model and provider.
</p>

</section>


<!-- INFERENCE -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>10 — INFERENCE</span>
<h2>How does an LLM <span>answer you?</span></h2>
</div>

<div class="llm-inference-flow">

<div>USER PROMPT</div>
<div>↓</div>
<div>TOKENIZE</div>
<div>↓</div>
<div>MODEL FORWARD PASS</div>
<div>↓</div>
<div>NEXT-TOKEN SCORES</div>
<div>↓</div>
<div>DECODING / SAMPLING</div>
<div>↓</div>
<div>APPEND TOKEN</div>
<div>↻</div>
<div>FINAL RESPONSE</div>

</div>

<div class="llm-lab-three-column">

<div class="llm-mini-card">
<strong>GREEDY</strong>
<p>Select the highest-probability next token.</p>
</div>

<div class="llm-mini-card">
<strong>TEMPERATURE</strong>
<p>Changes how strongly the probability distribution is flattened or sharpened.</p>
</div>

<div class="llm-mini-card">
<strong>TOP-P / TOP-K</strong>
<p>Restrict candidate tokens considered during sampling.</p>
</div>

</div>

</section>


<!-- CONTEXT WINDOW -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>11 — CONTEXT</span>
<h2>The <span>context window</span> matters.</h2>
</div>

<p class="llm-lab-wide-text">
The context window is the amount of input and conversation state a model
can process for a particular request. Larger context windows can enable
longer documents and richer interactions, but context size also affects
resource usage and application design.
</p>

<div class="llm-context-stack">

<div>SYSTEM INSTRUCTIONS</div>
<div>CONVERSATION</div>
<div>USER INPUT</div>
<div>RETRIEVED INFORMATION</div>
<div>TOOL RESULTS</div>
<div class="highlight">MODEL CONTEXT WINDOW</div>

</div>

</section>


<!-- PROMPTING -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>12 — USING LLMs</span>
<h2>Prompting: communicate the <span>task clearly.</span></h2>
</div>

<div class="llm-prompt-grid">

<div>
<strong>ROLE</strong>
<small>Who should the model act as?</small>
</div>

<div>
<strong>CONTEXT</strong>
<small>What information does it need?</small>
</div>

<div>
<strong>TASK</strong>
<small>What exactly should it do?</small>
</div>

<div>
<strong>CONSTRAINTS</strong>
<small>What should it avoid or enforce?</small>
</div>

<div>
<strong>FORMAT</strong>
<small>What should the output look like?</small>
</div>

<div>
<strong>EXAMPLES</strong>
<small>Can examples clarify the desired behavior?</small>
</div>

</div>

<p class="llm-lab-note">
Good prompting is useful, but production systems should not rely on prompting
alone. Retrieval, tools, validation, structured outputs, evaluation and
application logic often provide stronger reliability.
</p>

</section>


<!-- RAG -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>13 — KNOWLEDGE</span>
<h2>LLM + RAG = <span>grounded generation.</span></h2>
</div>

<div class="llm-rag-flow">

<div>USER QUESTION</div>
<div>↓</div>
<div>EMBED QUERY</div>
<div>↓</div>
<div>VECTOR SEARCH</div>
<div>↓</div>
<div>RETRIEVED CONTEXT</div>
<div>↓</div>
<div>LLM</div>
<div>↓</div>
<div>GROUNDED ANSWER</div>

</div>

<p class="llm-lab-wide-text">
Retrieval-Augmented Generation adds external information to the model's
context at request time. This is useful when an application needs to
answer from private, current or domain-specific information.
</p>

<a class="llm-lab-related-link" href="/labs/ai/rag/">
Explore RAG Systems →
</a>

</section>


<!-- FINE TUNING -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>14 — CUSTOMIZATION</span>
<h2>Prompting vs RAG vs <span>Fine-tuning.</span></h2>
</div>

<div class="llm-three-way">

<div>
<strong>PROMPTING</strong>
<p>Change instructions and context without changing model weights.</p>
</div>

<div>
<strong>RAG</strong>
<p>Provide external knowledge at inference time.</p>
</div>

<div>
<strong>FINE-TUNING</strong>
<p>Train a pretrained model further for a targeted behavior or domain.</p>
</div>

</div>

<p class="llm-lab-note">
Choose the simplest technique that satisfies the requirement.
Fine-tuning is not automatically the best way to add new knowledge.
</p>

</section>


<!-- GENERATIVE AI WORKFLOW -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>15 — APPLICATION ARCHITECTURE</span>
<h2>From model to <span>real application.</span></h2>
</div>

<div class="llm-application-flow">

<div>
<strong>USER</strong>
</div>

<div>↓</div>

<div>
<strong>APPLICATION</strong>
<small>UI / API / AUTH</small>
</div>

<div>↓</div>

<div class="highlight">
<strong>LLM</strong>
<small>GENERATION / REASONING</small>
</div>

<div>↙ &nbsp; ↓ &nbsp; ↘</div>

<div class="llm-application-branches">
<span>RAG</span>
<span>TOOLS</span>
<span>MEMORY</span>
</div>

<div>↓</div>

<div>
<strong>VALIDATION</strong>
<small>SAFETY / FORMAT / BUSINESS LOGIC</small>
</div>

<div>↓</div>

<div>
<strong>USER RESPONSE</strong>
</div>

</div>

</section>


<!-- SETUP -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>16 — SETUP</span>
<h2>How can you <span>use an LLM?</span></h2>
</div>

<div class="llm-setup-grid">

<div class="llm-setup-card">
<span>PATH 01</span>
<h3>Hosted API</h3>
<p>
Use a cloud provider's model through an API. This is usually the fastest
way to build a prototype because infrastructure and model serving are
managed for you.
</p>
<code>APPLICATION → API → HOSTED MODEL</code>
</div>

<div class="llm-setup-card">
<span>PATH 02</span>
<h3>Local Model</h3>
<p>
Download compatible model weights and run inference on your own hardware.
This provides more control over data and infrastructure, but requires
suitable compute, memory and model-serving software.
</p>
<code>APPLICATION → LOCAL SERVER → MODEL</code>
</div>

<div class="llm-setup-card">
<span>PATH 03</span>
<h3>Model Framework</h3>
<p>
Use libraries such as Transformers to load pretrained models, tokenize
inputs and run inference or training workflows.
</p>
<code>PYTHON → TRANSFORMERS → MODEL</code>
</div>

</div>

</section>


<!-- LOCAL SETUP -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>17 — LOCAL AI</span>
<h2>A simple <span>local workflow.</span></h2>
</div>

<div class="llm-code-flow">

<div>
<strong>1. CHOOSE MODEL</strong>
<small>Select a model that fits your task, license and hardware.</small>
</div>

<div>
<strong>2. DOWNLOAD / LOAD</strong>
<small>Obtain the model weights and tokenizer.</small>
</div>

<div>
<strong>3. SERVE</strong>
<small>Run an inference engine or local model application.</small>
</div>

<div>
<strong>4. CONNECT</strong>
<small>Call the local endpoint from your application.</small>
</div>

<div>
<strong>5. EVALUATE</strong>
<small>Measure quality, latency, memory and cost.</small>
</div>

</div>

<pre class="llm-code-block"><code># Example using Hugging Face Transformers

from transformers import pipeline

generator = pipeline(
    "text-generation",
    model="YOUR_MODEL"
)

result = generator(
    "Explain transformers simply.",
    max_new_tokens=100
)

print(result[0]["generated_text"])</code></pre>

<p class="llm-lab-note">
The exact model, hardware and serving method determine the installation
requirements. Always check the model's own documentation and license.
</p>

</section>


<!-- MODEL SERVING -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>18 — MODEL SERVING</span>
<h2>Inference engines turn models into <span>services.</span></h2>
</div>

<div class="llm-serving-grid">

<div>
<strong>TRANSFORMERS</strong>
<p>Model framework for loading, training and running many pretrained models.</p>
</div>

<div>
<strong>vLLM</strong>
<p>High-performance serving and inference with an OpenAI-compatible server.</p>
</div>

<div>
<strong>OLLAMA</strong>
<p>Convenient local model runtime focused on running open models locally.</p>
</div>

<div>
<strong>CUSTOM SERVER</strong>
<p>Build your own service layer around a model for application-specific control.</p>
</div>

</div>

</section>


<!-- HOW GENERATIVE AI WORKS -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>19 — GENERATIVE AI</span>
<h2>How does Generative AI <span>create?</span></h2>
</div>

<p class="llm-lab-wide-text">
Different generative models use different architectures and objectives.
A language model generates token sequences. Image-generation systems can
generate visual data through architectures and iterative denoising
processes. Audio and video systems use their own representations and
generation pipelines.
</p>

<div class="llm-gen-model-grid">

<div>
<strong>LANGUAGE</strong>
<span>Tokens → Transformer → Text</span>
</div>

<div>
<strong>IMAGE</strong>
<span>Prompt / Conditions → Generative Model → Image</span>
</div>

<div>
<strong>AUDIO</strong>
<span>Text / Audio Conditions → Generative Model → Audio</span>
</div>

<div>
<strong>VIDEO</strong>
<span>Text / Visual Conditions → Generative Model → Video</span>
</div>

</div>

<p class="llm-lab-note">
There is no single "Generative AI algorithm." The field includes multiple
model families, training objectives and generation techniques.
</p>

</section>


<!-- LIMITATIONS -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>20 — LIMITATIONS</span>
<h2>What can <span>go wrong?</span></h2>
</div>

<div class="llm-risk-grid">

<div><strong>HALLUCINATION</strong><p>Generated content can be plausible but incorrect.</p></div>
<div><strong>BIAS</strong><p>Models can reproduce undesirable patterns present in data.</p></div>
<div><strong>CONTEXT LIMITS</strong><p>The model can only process the context available to it.</p></div>
<div><strong>STALE KNOWLEDGE</strong><p>A pretrained model does not automatically know new information.</p></div>
<div><strong>PROMPT INJECTION</strong><p>Untrusted content can attempt to influence application instructions.</p></div>
<div><strong>COST / LATENCY</strong><p>Large models can require significant compute and network resources.</p></div>
<div><strong>DATA PRIVACY</strong><p>Applications must control sensitive information sent to models.</p></div>
<div><strong>NON-DETERMINISM</strong><p>Sampling can produce different outputs for the same input.</p></div>

</div>

</section>


<!-- EVALUATION -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>21 — EVALUATION</span>
<h2>Don't just ask if it <span>works.</span></h2>
</div>

<div class="llm-evaluation-grid">

<div>
<strong>QUALITY</strong>
<small>Is the answer correct and useful?</small>
</div>

<div>
<strong>GROUNDING</strong>
<small>Does the answer follow trusted information?</small>
</div>

<div>
<strong>LATENCY</strong>
<small>How long does the system take?</small>
</div>

<div>
<strong>COST</strong>
<small>How much compute or API usage is required?</small>
</div>

<div>
<strong>SAFETY</strong>
<small>Does the system behave within required boundaries?</small>
</div>

<div>
<strong>RELIABILITY</strong>
<small>Does it behave consistently across representative cases?</small>
</div>

</div>

</section>


<!-- REAL WORLD -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>22 — APPLICATIONS</span>
<h2>Where are LLMs and Generative AI <span>used?</span></h2>
</div>

<div class="llm-use-case-grid">

<div><strong>AI ASSISTANTS</strong><span>Conversation and task assistance</span></div>
<div><strong>CODING</strong><span>Code generation, review and debugging</span></div>
<div><strong>SEARCH</strong><span>Semantic retrieval and answer generation</span></div>
<div><strong>DOCUMENT AI</strong><span>Extraction, summarization and question answering</span></div>
<div><strong>CUSTOMER SUPPORT</strong><span>Conversational support and automation</span></div>
<div><strong>CONTENT</strong><span>Writing, editing and transformation</span></div>
<div><strong>ANALYTICS</strong><span>Natural-language interfaces for data</span></div>
<div><strong>AGENTS</strong><span>Multi-step tasks using tools and external systems</span></div>

</div>

</section>


<!-- LEARNING ROADMAP -->
<section class="llm-lab-section">

<div class="llm-lab-section-heading">
<span>23 — LEARNING ROADMAP</span>
<h2>How to learn LLMs <span>properly.</span></h2>
</div>

<div class="llm-roadmap">

<div><span>01</span><strong>Python + ML Basics</strong></div>
<div><span>02</span><strong>NLP Foundations</strong></div>
<div><span>03</span><strong>Tokenization + Embeddings</strong></div>
<div><span>04</span><strong>Attention + Transformers</strong></div>
<div><span>05</span><strong>LLM Training + Inference</strong></div>
<div><span>06</span><strong>Prompt Engineering</strong></div>
<div><span>07</span><strong>RAG + Vector Search</strong></div>
<div><span>08</span><strong>Agents + Tools</strong></div>
<div><span>09</span><strong>Evaluation + Safety</strong></div>
<div><span>10</span><strong>Production Architecture</strong></div>

</div>

</section>


<!-- INTERVIEW PREP -->
<section class="llm-lab-interview">

<span>READY FOR THE NEXT LEVEL?</span>

<h2>
Turn the knowledge into
<span>interview answers.</span>
</h2>

<p>
Use the dedicated GenAI Interview Prep module for structured questions,
architecture explanations, terminology, project discussions and
interview-focused preparation.
</p>

<a href="/interview-prep/gen-ai/">
Open GenAI Interview Prep →
</a>

</section>


<!-- REFERENCES -->
<section class="llm-lab-references">

<div class="llm-lab-section-heading">
<span>24 — REFERENCES</span>
<h2>Learn from the <span>original sources.</span></h2>
</div>

<div class="llm-reference-list">

<a href="https://arxiv.org/abs/1706.03762">
Attention Is All You Need — Transformer architecture
</a>

<a href="https://arxiv.org/abs/1810.04805">
BERT — Bidirectional Transformer pretraining
</a>

<a href="https://huggingface.co/learn/llm-course/en/chapter1/1">
Hugging Face LLM Course
</a>

<a href="https://huggingface.co/docs/transformers/quicktour">
Hugging Face Transformers Quickstart
</a>

<a href="https://docs.vllm.ai/en/latest/getting_started/quickstart/">
vLLM Quickstart
</a>

<a href="https://ollama.com/">
Ollama — Run open models locally
</a>

<a href="https://ai.google.dev/gemini-api/docs/get-started">
Gemini API — Getting Started
</a>

</div>

</section>


<!-- NAVIGATION -->
<div class="llm-lab-navigation">

<a href="/labs/ai/">← Back to AI Lab</a>

<a href="/labs/ai/agents/">AI Agents →</a>

</div>

</section>
