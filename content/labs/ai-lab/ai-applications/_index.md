---
title: "AI Applications"
description: "A practical knowledge base for understanding, designing, building, deploying and operating real-world AI applications."
weight: 50
toc: true
---

<section class="ai-apps-page">

<!-- HERO -->
<section class="ai-apps-hero">

<div class="ai-apps-status">
<span class="ai-apps-status-dot"></span>
AI APPLICATIONS KNOWLEDGE BASE
</div>

<h1 class="ai-apps-title">
Build <span>AI Applications</span>
</h1>

<p class="ai-apps-subtitle">
Understand how modern AI applications are designed — from the user
interface and backend to models, RAG, agents, tools, databases,
multimodal systems, local inference, security, evaluation and production.
</p>

<div class="ai-apps-terminal">
<span>$</span>
<strong>initialize_ai_application()</strong>
<i></i>
</div>

</section>


<!-- ARCHITECTURE PIPELINE -->
<section class="ai-apps-pipeline">

<div><span>01</span><strong>USER</strong></div>
<div>→</div>
<div><span>02</span><strong>APP</strong></div>
<div>→</div>
<div><span>03</span><strong>MODEL</strong></div>
<div>→</div>
<div><span>04</span><strong>DATA</strong></div>
<div>→</div>
<div><span>05</span><strong>ACTION</strong></div>

</section>


<!-- WHAT IS AI APPLICATION -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>01 — FOUNDATION</span>
<h2>What is an <span>AI Application?</span></h2>
</div>

<p>
An AI application is a software system that uses one or more AI models
to perform tasks such as generating text, understanding documents,
analyzing images, processing speech, retrieving information, making
decisions or interacting with external tools.
</p>

<p>
The important distinction is that an AI application is usually much more
than a model. The application combines models with software engineering,
data, APIs, business logic, security, user interfaces and operational
infrastructure.
</p>

<div class="ai-apps-definition-card">
<span>MENTAL MODEL</span>
<strong>USER → APPLICATION → AI MODEL → DATA / TOOLS → RESPONSE / ACTION</strong>
<small>
The model is one component inside a larger software system.
</small>
</div>

</section>


<!-- APPLICATION COMPONENTS -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>02 — BUILDING BLOCKS</span>
<h2>The major <span>components.</span></h2>
</div>

<div class="ai-apps-card-grid">

<div>
<strong>USER INTERFACE</strong>
<p>Web, mobile, desktop, API or voice interface through which users interact with the system.</p>
</div>

<div>
<strong>BACKEND</strong>
<p>Application logic, authentication, orchestration, APIs and business rules.</p>
</div>

<div>
<strong>MODEL</strong>
<p>LLM, vision model, speech model, embedding model or another inference component.</p>
</div>

<div>
<strong>DATA</strong>
<p>Documents, databases, files, structured records and other application knowledge.</p>
</div>

<div>
<strong>TOOLS</strong>
<p>External APIs, databases, search engines, code execution or business systems.</p>
</div>

<div>
<strong>OBSERVABILITY</strong>
<p>Logs, traces, metrics, evaluation results and operational monitoring.</p>
</div>

</div>

</section>


<!-- BASIC ARCHITECTURE -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>03 — ARCHITECTURE</span>
<h2>A basic <span>AI application architecture.</span></h2>
</div>

<div class="ai-apps-architecture">

<div>
<strong>USER</strong>
<small>Web / Mobile / Voice</small>
</div>

<div>↓</div>

<div>
<strong>APPLICATION</strong>
<small>Frontend + Backend + Authentication</small>
</div>

<div>↓</div>

<div class="highlight">
<strong>AI ORCHESTRATION</strong>
<small>Prompting / Routing / RAG / Agents</small>
</div>

<div>↓</div>

<div>
<strong>MODEL LAYER</strong>
<small>Hosted API / Local Model</small>
</div>

<div>↓</div>

<div class="ai-apps-side-nodes">
<span>DATABASE</span>
<span>VECTOR STORE</span>
<span>TOOLS / APIs</span>
</div>

</div>

</section>


<!-- MODEL LAYER -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>04 — MODEL LAYER</span>
<h2>Choosing the right <span>AI model.</span></h2>
</div>

<div class="ai-apps-model-grid">

<div>
<strong>LLM</strong>
<p>Text generation, reasoning, summarization, extraction and conversational applications.</p>
</div>

<div>
<strong>VISION MODEL</strong>
<p>Image understanding, classification, detection and visual reasoning.</p>
</div>

<div>
<strong>SPEECH MODEL</strong>
<p>Speech recognition, synthesis, translation and voice interaction.</p>
</div>

<div>
<strong>EMBEDDING MODEL</strong>
<p>Convert text or other content into vectors for semantic retrieval.</p>
</div>

<div>
<strong>IMAGE GENERATION</strong>
<p>Create or transform visual content using generative models.</p>
</div>

<div>
<strong>SMALL / LOCAL MODEL</strong>
<p>Lower-cost or privacy-oriented inference on local or edge hardware.</p>
</div>

</div>

</section>


<!-- HOSTED VS LOCAL -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>05 — DEPLOYMENT</span>
<h2>Hosted AI vs <span>Local AI.</span></h2>
</div>

<div class="ai-apps-comparison">

<div>
<span>HOSTED</span>
<strong>Cloud model APIs</strong>
<p>
The application sends requests to a managed inference service. This can
simplify deployment and provide access to large models.
</p>
</div>

<div class="ai-apps-comparison-arrow">VS</div>

<div class="highlight">
<span>LOCAL</span>
<strong>Self-hosted inference</strong>
<p>
The model runs on infrastructure controlled by the application owner,
which can provide greater control over data and runtime behavior.
</p>
</div>

</div>

</section>


<!-- API APPLICATIONS -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>06 — API-BASED AI</span>
<h2>Building with <span>AI APIs.</span></h2>
</div>

<div class="ai-apps-api-flow">

<div>USER REQUEST</div>
<div>→</div>
<div>BACKEND</div>
<div>→</div>
<div class="highlight">AI API</div>
<div>→</div>
<div>MODEL</div>
<div>→</div>
<div>RESPONSE</div>

</div>

<p>
API-based applications allow developers to integrate AI capabilities
without managing the underlying model infrastructure. The application
still needs authentication, input validation, error handling, rate
limiting, observability and cost controls.
</p>

</section>


<!-- PROMPTING -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>07 — APPLICATION LOGIC</span>
<h2>Prompting and <span>structured output.</span></h2>
</div>

<p>
Prompts provide instructions and context to a model. Production
applications often require predictable output rather than free-form text,
so structured schemas and validation can be used when supported by the
model and framework.
</p>

<div class="ai-apps-prompt-flow">

<div>
<strong>SYSTEM INSTRUCTIONS</strong>
</div>

<div>+</div>

<div>
<strong>USER INPUT</strong>
</div>

<div>+</div>

<div>
<strong>APPLICATION CONTEXT</strong>
</div>

<div>→</div>

<div class="highlight">
<strong>MODEL</strong>
</div>

<div>→</div>

<div>
<strong>VALIDATED OUTPUT</strong>
</div>

</div>

</section>


<!-- RAG -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>08 — KNOWLEDGE</span>
<h2>AI applications with <span>RAG.</span></h2>
</div>

<p>
Retrieval-Augmented Generation allows an application to retrieve relevant
information from an external knowledge source and provide that information
to the model as context.
</p>

<div class="ai-apps-rag-flow">

<div>USER QUERY</div>
<div>→</div>
<div>EMBED QUERY</div>
<div>→</div>
<div>VECTOR / HYBRID SEARCH</div>
<div>→</div>
<div>RELEVANT CONTEXT</div>
<div>→</div>
<div class="highlight">LLM</div>
<div>→</div>
<div>ANSWER</div>

</div>

<p class="ai-apps-note">
RAG is useful when the application's knowledge changes independently of
the model or when responses need to be grounded in private or external
information.
</p>

</section>


<!-- AGENTS -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>09 — ACTION</span>
<h2>AI applications with <span>agents.</span></h2>
</div>

<div class="ai-apps-agent-flow">

<div>USER GOAL</div>
<div>↓</div>
<div class="highlight">AI AGENT</div>
<div>↙ &nbsp; ↓ &nbsp; ↘</div>

<div class="ai-apps-agent-tools">
<span>RAG</span>
<span>TOOLS</span>
<span>MEMORY</span>
</div>

<div>↓</div>
<div>OBSERVE → REASON → ACT</div>
<div>↓</div>
<div>RESULT</div>

</div>

<p>
An agent-oriented application gives a model access to tools and an
execution loop so it can select actions, inspect results and continue
toward a goal.
</p>

</section>


<!-- TOOLS -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>10 — TOOL CALLING</span>
<h2>Connecting AI to <span>real systems.</span></h2>
</div>

<div class="ai-apps-tool-grid">

<div>
<strong>DATABASE</strong>
<p>Query application data using controlled interfaces.</p>
</div>

<div>
<strong>REST API</strong>
<p>Call external services and business applications.</p>
</div>

<div>
<strong>SEARCH</strong>
<p>Retrieve current information from approved sources.</p>
</div>

<div>
<strong>CODE</strong>
<p>Execute controlled computation or data processing.</p>
</div>

<div>
<strong>FILES</strong>
<p>Read, transform and analyze application documents.</p>
</div>

<div>
<strong>BUSINESS SYSTEM</strong>
<p>Interact with enterprise workflows through secure APIs.</p>
</div>

</div>

</section>


<!-- MULTIMODAL -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>11 — MULTIMODAL AI</span>
<h2>Applications that understand <span>multiple modalities.</span></h2>
</div>

<div class="ai-apps-multimodal">

<div>
<span>TEXT</span>
<strong>Documents</strong>
</div>

<div>
<span>IMAGE</span>
<strong>Visual content</strong>
</div>

<div>
<span>AUDIO</span>
<strong>Speech</strong>
</div>

<div>
<span>VIDEO</span>
<strong>Temporal visual data</strong>
</div>

<div>
<span>STRUCTURED DATA</span>
<strong>Tables / databases</strong>
</div>

</div>

<p>
Multimodal applications combine different types of input and output.
Examples include document intelligence, visual assistants, voice agents
and systems that analyze images together with textual context.
</p>

</section>


<!-- COPILOTS -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>12 — AI COPILOTS</span>
<h2>What makes an AI <span>copilot?</span></h2>
</div>

<div class="ai-apps-card-grid">

<div>
<strong>CONTEXT AWARENESS</strong>
<p>The system understands the current task, document or workflow.</p>
</div>

<div>
<strong>ASSISTIVE OUTPUT</strong>
<p>The AI suggests, summarizes, generates or explains information.</p>
</div>

<div>
<strong>HUMAN CONTROL</strong>
<p>The user can review or override important actions.</p>
</div>

<div>
<strong>WORKFLOW INTEGRATION</strong>
<p>The copilot operates inside an existing application or business process.</p>
</div>

<div>
<strong>FEEDBACK</strong>
<p>User feedback can help improve prompts, retrieval and application behavior.</p>
</div>

<div>
<strong>GUARDRAILS</strong>
<p>Controls limit unsafe, invalid or unauthorized actions.</p>
</div>

</div>

</section>


<!-- DOCUMENT INTELLIGENCE -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>13 — DOCUMENT AI</span>
<h2>From documents to <span>structured knowledge.</span></h2>
</div>

<div class="ai-apps-document-flow">

<div>DOCUMENT</div>
<div>→</div>
<div>OCR / PARSING</div>
<div>→</div>
<div>CHUNKING</div>
<div>→</div>
<div>EMBEDDINGS</div>
<div>→</div>
<div class="highlight">RAG / EXTRACTION</div>
<div>→</div>
<div>STRUCTURED OUTPUT</div>

</div>

</section>


<!-- VISION -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>14 — COMPUTER VISION</span>
<h2>AI applications that <span>see.</span></h2>
</div>

<div class="ai-apps-vision-grid">

<div>
<strong>CLASSIFICATION</strong>
<p>Determine the category of an image.</p>
</div>

<div>
<strong>OBJECT DETECTION</strong>
<p>Locate and classify objects in images or video.</p>
</div>

<div>
<strong>SEGMENTATION</strong>
<p>Identify pixels belonging to particular objects or regions.</p>
</div>

<div>
<strong>VISION-LANGUAGE</strong>
<p>Combine visual information with natural language reasoning.</p>
</div>

</div>

</section>


<!-- SPEECH -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>15 — VOICE</span>
<h2>AI applications that <span>listen and speak.</span></h2>
</div>

<div class="ai-apps-voice-flow">

<div>MICROPHONE</div>
<div>→</div>
<div>ASR</div>
<div>→</div>
<div class="highlight">LLM / AGENT</div>
<div>→</div>
<div>TTS</div>
<div>→</div>
<div>SPEAKER</div>

</div>

</section>


<!-- EDGE -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>16 — EDGE AI</span>
<h2>AI applications beyond the <span>cloud.</span></h2>
</div>

<div class="ai-apps-edge-flow">

<div>
<strong>CAMERA / SENSOR</strong>
<small>Real-world input</small>
</div>

<div>→</div>

<div class="highlight">
<strong>EDGE MODEL</strong>
<small>Local inference</small>
</div>

<div>→</div>

<div>
<strong>ACTION</strong>
<small>Device response</small>
</div>

</div>

<p>
Edge AI can reduce network dependency and latency by performing inference
closer to where data is generated.
</p>

</section>


<!-- DATA LAYER -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>17 — DATA</span>
<h2>The <span>data layer.</span></h2>
</div>

<div class="ai-apps-data-grid">

<div>
<strong>SQL DATABASE</strong>
<p>Structured application records.</p>
</div>

<div>
<strong>OBJECT STORAGE</strong>
<p>Documents, images, audio and other files.</p>
</div>

<div>
<strong>VECTOR DATABASE</strong>
<p>Semantic representations for retrieval.</p>
</div>

<div>
<strong>CACHE</strong>
<p>Frequently accessed results and application state.</p>
</div>

</div>

</section>


<!-- SECURITY -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>18 — SECURITY</span>
<h2>Security for <span>AI applications.</span></h2>
</div>

<div class="ai-apps-security-grid">

<div><strong>AUTHENTICATION</strong><p>Verify users and services.</p></div>
<div><strong>AUTHORIZATION</strong><p>Control what each identity can access.</p></div>
<div><strong>INPUT VALIDATION</strong><p>Validate and constrain application input.</p></div>
<div><strong>PROMPT INJECTION</strong><p>Protect systems from malicious instructions in model context.</p></div>
<div><strong>DATA PROTECTION</strong><p>Protect private data in storage and transit.</p></div>
<div><strong>TOOL PERMISSIONS</strong><p>Restrict what an AI system is allowed to execute.</p></div>
<div><strong>SECRETS</strong><p>Keep API keys and credentials outside prompts and source code.</p></div>
<div><strong>AUDITING</strong><p>Record important application and tool actions.</p></div>

</div>

</section>


<!-- GUARDRAILS -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>19 — GUARDRAILS</span>
<h2>Keeping AI behavior <span>controlled.</span></h2>
</div>

<div class="ai-apps-guardrail-flow">

<div>USER INPUT</div>
<div>→</div>
<div>INPUT GUARDRAILS</div>
<div>→</div>
<div class="highlight">AI SYSTEM</div>
<div>→</div>
<div>OUTPUT VALIDATION</div>
<div>→</div>
<div>USER / ACTION</div>

</div>

</section>


<!-- EVALUATION -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>20 — EVALUATION</span>
<h2>How do we know the application <span>works?</span></h2>
</div>

<div class="ai-apps-evaluation-grid">

<div>
<strong>QUALITY</strong>
<small>Is the answer correct and useful?</small>
</div>

<div>
<strong>GROUNDING</strong>
<small>Is the response supported by retrieved information?</small>
</div>

<div>
<strong>LATENCY</strong>
<small>How quickly does the application respond?</small>
</div>

<div>
<strong>COST</strong>
<small>How much does each request or workflow cost?</small>
</div>

<div>
<strong>RELIABILITY</strong>
<small>Does the system behave consistently?</small>
</div>

<div>
<strong>SAFETY</strong>
<small>Does it remain within intended boundaries?</small>
</div>

</div>

</section>


<!-- OBSERVABILITY -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>21 — OPERATIONS</span>
<h2>Observability for <span>AI systems.</span></h2>
</div>

<div class="ai-apps-observability">

<div>
<span>LOGS</span>
<strong>What happened?</strong>
</div>

<div>
<span>METRICS</span>
<strong>How is it performing?</strong>
</div>

<div>
<span>TRACES</span>
<strong>Where did time go?</strong>
</div>

<div>
<span>EVALUATION</span>
<strong>Was the result good?</strong>
</div>

</div>

</section>


<!-- COST -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>22 — COST</span>
<h2>Managing <span>AI application cost.</span></h2>
</div>

<div class="ai-apps-cost-grid">

<div>
<strong>MODEL SELECTION</strong>
<p>Use the smallest model that reliably solves the task.</p>
</div>

<div>
<strong>TOKEN CONTROL</strong>
<p>Reduce unnecessary context and output.</p>
</div>

<div>
<strong>CACHING</strong>
<p>Reuse results when appropriate.</p>
</div>

<div>
<strong>ROUTING</strong>
<p>Send simple requests to cheaper models.</p>
</div>

<div>
<strong>RETRIEVAL</strong>
<p>Retrieve only the context needed for the task.</p>
</div>

<div>
<strong>MONITORING</strong>
<p>Track usage and cost by application or workflow.</p>
</div>

</div>

</section>


<!-- PRODUCTION -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>23 — PRODUCTION</span>
<h2>A production-ready <span>AI architecture.</span></h2>
</div>

<div class="ai-apps-production">

<div>
<strong>CLIENT</strong>
<small>Web / Mobile / Voice</small>
</div>

<div>↓</div>

<div>
<strong>API GATEWAY</strong>
<small>Authentication / Rate limits</small>
</div>

<div>↓</div>

<div>
<strong>APPLICATION SERVICE</strong>
<small>Business logic / Orchestration</small>
</div>

<div>↓</div>

<div class="highlight">
<strong>AI ORCHESTRATION</strong>
<small>LLM / RAG / Agents / Tools</small>
</div>

<div>↓</div>

<div class="ai-apps-production-dependencies">
<span>MODEL</span>
<span>DATABASE</span>
<span>VECTOR STORE</span>
<span>EXTERNAL APIs</span>
</div>

<div>↓</div>

<div>
<strong>OBSERVABILITY</strong>
<small>Logs / Metrics / Traces / Evaluation</small>
</div>

</div>

</section>


<!-- SCALING -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>24 — SCALING</span>
<h2>Scaling AI <span>applications.</span></h2>
</div>

<div class="ai-apps-scaling-grid">

<div><strong>HORIZONTAL SCALING</strong><p>Add application instances to handle more requests.</p></div>
<div><strong>ASYNC PROCESSING</strong><p>Move long-running AI workloads into queues and workers.</p></div>
<div><strong>MODEL ROUTING</strong><p>Distribute requests across models according to task and cost.</p></div>
<div><strong>GPU SCALING</strong><p>Scale inference infrastructure when self-hosting models.</p></div>
<div><strong>CACHING</strong><p>Avoid repeated computation where safe.</p></div>
<div><strong>STREAMING</strong><p>Return partial results for better perceived latency.</p></div>

</div>

</section>


<!-- APPLICATION LIFECYCLE -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>25 — DEVELOPMENT</span>
<h2>The AI application <span>lifecycle.</span></h2>
</div>

<div class="ai-apps-lifecycle">

<div><span>01</span><strong>DEFINE</strong><small>Identify the problem.</small></div>
<div><span>02</span><strong>PROTOTYPE</strong><small>Test the AI capability.</small></div>
<div><span>03</span><strong>EVALUATE</strong><small>Measure quality.</small></div>
<div><span>04</span><strong>INTEGRATE</strong><small>Build the application.</small></div>
<div><span>05</span><strong>SECURE</strong><small>Add controls.</small></div>
<div><span>06</span><strong>DEPLOY</strong><small>Release to production.</small></div>
<div><span>07</span><strong>MONITOR</strong><small>Observe real usage.</small></div>
<div><span>08</span><strong>IMPROVE</strong><small>Iterate continuously.</small></div>

</div>

</section>


<!-- PYTHON -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>26 — PYTHON</span>
<h2>A simple <span>AI application.</span></h2>
</div>

<pre class="ai-apps-code-block"><code># Conceptual AI application flow

user_input = get_user_input()

# Retrieve application knowledge when required
context = retrieve_context(user_input)

# Build the model request
prompt = build_prompt(
    user_input=user_input,
    context=context
)

# Run inference
response = model.generate(prompt)

# Validate before returning to the user
result = validate_output(response)

return result</code></pre>

<p class="ai-apps-note">
A production implementation also needs authentication, error handling,
timeouts, retries, logging, monitoring, evaluation and appropriate
security controls.
</p>

</section>


<!-- REAL WORLD -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>27 — REAL WORLD</span>
<h2>Common <span>AI application patterns.</span></h2>
</div>

<div class="ai-apps-use-case-grid">

<div><strong>CHAT ASSISTANT</strong><span>LLM + conversation + application context.</span></div>
<div><strong>KNOWLEDGE ASSISTANT</strong><span>RAG + LLM + citations.</span></div>
<div><strong>AI COPILOT</strong><span>Context + generation + human workflow.</span></div>
<div><strong>VOICE AGENT</strong><span>ASR + agent + tools + TTS.</span></div>
<div><strong>DOCUMENT AI</strong><span>OCR + extraction + validation.</span></div>
<div><strong>VISION SYSTEM</strong><span>Images + vision model + business logic.</span></div>
<div><strong>AI AUTOMATION</strong><span>Agent + tools + workflow execution.</span></div>
<div><strong>EDGE AI</strong><span>Sensor + local model + device action.</span></div>

</div>

</section>


<!-- DESIGN CHECKLIST -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>28 — DESIGN CHECKLIST</span>
<h2>Before building an <span>AI application.</span></h2>
</div>

<div class="ai-apps-checklist">

<div><span>□</span><strong>What problem are we solving?</strong></div>
<div><span>□</span><strong>Does the task actually require AI?</strong></div>
<div><span>□</span><strong>Which model is appropriate?</strong></div>
<div><span>□</span><strong>Does the application need RAG?</strong></div>
<div><span>□</span><strong>Does it need tool calling or agents?</strong></div>
<div><span>□</span><strong>Where will inference run?</strong></div>
<div><span>□</span><strong>What data does the system access?</strong></div>
<div><span>□</span><strong>How will quality be evaluated?</strong></div>
<div><span>□</span><strong>What are the security risks?</strong></div>
<div><span>□</span><strong>What is the expected latency and cost?</strong></div>

</div>

</section>


<!-- LEARNING ROADMAP -->
<section class="ai-apps-section">

<div class="ai-apps-section-heading">
<span>29 — LEARNING ROADMAP</span>
<h2>Learn to build AI applications <span>step by step.</span></h2>
</div>

<div class="ai-apps-roadmap">

<div><span>01</span><strong>Python & Software Engineering</strong></div>
<div><span>02</span><strong>LLMs & Generative AI</strong></div>
<div><span>03</span><strong>Prompt Engineering</strong></div>
<div><span>04</span><strong>Embeddings & Vector Search</strong></div>
<div><span>05</span><strong>RAG Systems</strong></div>
<div><span>06</span><strong>AI Agents & Tool Calling</strong></div>
<div><span>07</span><strong>Multimodal AI</strong></div>
<div><span>08</span><strong>APIs & Backend Architecture</strong></div>
<div><span>09</span><strong>Security & Guardrails</strong></div>
<div><span>10</span><strong>Evaluation & Observability</strong></div>
<div><span>11</span><strong>Deployment & Scaling</strong></div>
<div><span>12</span><strong>Production AI Systems</strong></div>

</div>

</section>


<!-- INTERVIEW -->
<section class="ai-apps-interview">

<span>READY FOR THE NEXT LEVEL?</span>

<h2>
Turn AI application knowledge into
<span>interview answers.</span>
</h2>

<p>
Use the GenAI Interview Prep module for LLM architecture, RAG,
agents, tool calling, multimodal systems, AI application design,
security, scaling, troubleshooting and senior-level architecture
questions.
</p>

<a href="/interview-prep/gen-ai/">
Open GenAI Interview Prep →
</a>

</section>


<!-- REFERENCES -->
<section class="ai-apps-references">

<div class="ai-apps-section-heading">
<span>30 — REFERENCES</span>
<h2>Explore the <span>ecosystem.</span></h2>
</div>

<div class="ai-apps-reference-list">

<a href="https://huggingface.co/docs">
Hugging Face — Models, datasets and AI tooling
</a>

<a href="https://fastapi.tiangolo.com/">
FastAPI — Python API framework
</a>

<a href="https://docs.docker.com/">
Docker — Containerization
</a>

<a href="https://kubernetes.io/docs/">
Kubernetes — Container orchestration
</a>

<a href="https://mlflow.org/docs/latest/">
MLflow — Machine learning and AI lifecycle tooling
</a>

<a href="https://opentelemetry.io/docs/">
OpenTelemetry — Observability
</a>

</div>

</section>


<!-- NAVIGATION -->
<div class="ai-apps-navigation">

<a href="/labs/ai/">← Back to AI Lab</a>

<a href="/labs/ai/rag/">RAG Systems</a>

<a href="/labs/ai/speech-ai/">Speech AI</a>

<a href="/labs/ai/agents/">AI Agents</a>

</div>

</section>
