---
title: ""
description: "Interview-focused AI agent preparation covering agent architecture, tool calling, planning, memory, workflows, orchestration, multi-agent systems, security, evaluation, reliability and production design."
weight: 70
toc: true
---
# 🤖 AI Agents
An AI agent is a model-driven application pattern where a model can select actions, use tools, observe results and continue working toward a goal.
## 1. What Is an AI Agent?
A simple agent loop is:
```text
Goal
 ↓
Model
 ↓
Choose Action
 ↓
Tool
 ↓
Observe Result
 ↓
Model
 ↓
Next Action / Final Answer
```
The important difference from a basic chatbot is that an agent can participate in a multi-step workflow and interact with external capabilities.
### Interview Answer
> "An AI agent is an application pattern that uses a model to decide or select actions, interact with tools or external systems, observe results and continue toward a goal."
## 2. LLM vs Agent
```text
LLM
→ Generates or transforms information.

Agent
→ Uses an LLM inside a workflow that can select actions,
  call tools, observe results and continue toward a goal.
```
An LLM can be a component of an agent, but an LLM by itself is not necessarily an agent.
## 3. Agent Architecture
A simplified architecture:
```text
                    ┌──────────────┐
                    │     User     │
                    └──────┬───────┘
                           ↓
                    ┌──────────────┐
                    │ Application  │
                    └──────┬───────┘
                           ↓
                    ┌──────────────┐
                    │     Agent    │
                    └──────┬───────┘
                           ↓
                    ┌──────────────┐
                    │     LLM      │
                    └──────┬───────┘
                           ↓
             ┌─────────────┼─────────────┐
             ↓             ↓             ↓
          Search        Database        API
             ↓             ↓             ↓
             └─────────────┼─────────────┘
                           ↓
                       Tool Result
                           ↓
                          Agent
                           ↓
                      Final Answer
```
## 4. Agent Components
A production agent can contain:
```text
Model
Instructions
Tools
Memory
State
Planning / workflow logic
Guardrails
Evaluation
Observability
Authorization
```
Not every agent requires every component.
## 5. Tools
Tools allow an agent to interact with external systems.
Examples:
```text
Search
Database
REST API
Calculator
File system
Ticketing system
Cloud platform
Monitoring system
Internal application
```
A tool should have a clearly defined interface:
```text
Tool name
Purpose
Input schema
Output schema
Permissions
Failure behavior
```
## 6. Tool Calling
A simplified tool-calling flow:
```text
User Request
 ↓
Agent / Model
 ↓
Select Tool
 ↓
Generate Arguments
 ↓
Application Validates
 ↓
Tool Executes
 ↓
Tool Result
 ↓
Model
 ↓
Answer / Next Action
```
The application should validate arguments and authorization before executing the tool.
## 7. Tool Selection
An agent may choose among multiple tools based on the task.
Example:
```text
User asks:
"What is the status of ticket INC-1004?"

Possible tools:
Search
Ticket API
Database
Calculator
```
The agent should select the capability appropriate for the task.
## 8. Tool Permissions
Never rely only on the model to enforce authorization.
Use:
```text
Identity
 ↓
Authorization
 ↓
Tool permissions
 ↓
Execution
```
For sensitive actions, consider:
```text
Least privilege
Allow lists
Human approval
Audit logging
Rate limits
```
## 9. Read vs Write Tools
This is an important production distinction.
### Read Tool
```text
Search ticket
Get database record
Get monitoring status
```
### Write Tool
```text
Create ticket
Delete resource
Restart service
Modify database
```
Write operations should generally have stronger authorization and validation.
## 10. Agent Loop
A basic agent loop can be represented as:
```text
Observe Goal
 ↓
Decide
 ↓
Act
 ↓
Observe Result
 ↓
Decide
 ↓
Act
 ↓
Complete
```
The loop should have explicit stopping conditions.
## 11. Stopping Conditions
An agent should not run indefinitely.
Possible limits:
```text
Maximum iterations
Maximum tool calls
Time limit
Token budget
Cost budget
Successful completion
Human cancellation
```
## 12. Planning
Planning breaks a complex objective into smaller actions.
Example:
```text
Goal:
Prepare an incident report.

Plan:
1. Retrieve incident details
2. Retrieve related logs
3. Analyze timeline
4. Identify likely cause
5. Generate report
```
Planning can be explicit or handled dynamically by the model depending on the architecture.
## 13. Planning vs Workflow
A deterministic workflow:
```text
Step 1
 ↓
Step 2
 ↓
Step 3
```
An agentic workflow:
```text
Goal
 ↓
Model decides next action
 ↓
Tool
 ↓
Observation
 ↓
Model decides next action
```
Use deterministic workflows when the sequence is known and predictable. Use agentic behavior when dynamic decision-making provides meaningful value.
## 14. Agent vs Workflow
| Workflow | Agent |
|---|---|
| Usually predefined steps | Can dynamically choose actions |
| More predictable | More flexible |
| Easier to test | More complex to evaluate |
| Easier to control | Requires stronger guardrails |
| Often lower variance | Potentially higher variance |
Neither approach is automatically better.
## 15. Memory
Agent applications may maintain information across steps or interactions.
Types can include:
```text
Short-term state
Conversation history
Task state
Long-term user information
External knowledge
```
Memory should be designed deliberately rather than storing everything.
## 16. Short-Term Memory
Short-term memory contains information needed within the current task.
Example:
```text
User request
 ↓
Tool result
 ↓
Previous tool result
 ↓
Next action
```
This is often represented as workflow state or conversation context.
## 17. Long-Term Memory
Long-term memory can persist information beyond one interaction.
Possible storage:
```text
Database
Vector store
Profile store
Knowledge base
```
Examples:
```text
User preferences
Past tasks
Known entities
Historical interactions
```
Access controls and retention policies are important.
## 18. Agent State
State represents information needed to continue the workflow.
Example:
```json
{
  "task": "investigate_incident",
  "incident_id": "INC-1004",
  "status": "collecting_logs",
  "attempts": 2
}
```
State should be explicit when reliability and recovery matter.
## 19. Agent Memory vs RAG
```text
Memory
→ Information about previous interactions,
  task state or persistent user context.

RAG
→ Retrieval of external knowledge relevant to
  the current task.
```
An application can use both.
## 20. Multi-Agent Systems
A multi-agent architecture uses multiple specialized agents.
Example:
```text
                    Supervisor
                        ↓
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
      Research       Coding       Validation
       Agent          Agent          Agent
          ↓             ↓             ↓
          └─────────────┼─────────────┘
                        ↓
                     Result
```
Use multiple agents only when specialization or parallelism provides a real benefit.
## 21. Supervisor Pattern
A supervisor coordinates specialized agents.
```text
User
 ↓
Supervisor
 ↓
Select Specialist
 ↓
Specialist Agent
 ↓
Result
 ↓
Supervisor
 ↓
Final Response
```
## 22. Handoff Pattern
One agent can transfer a task to another specialized agent.
```text
Triage Agent
 ↓
Determine category
 ↓
Handoff
 ↓
Specialist Agent
 ↓
Resolution
```
This can be useful when responsibilities are clearly separated.
## 23. Parallel Agents
Independent subtasks can sometimes run concurrently.
```text
                Supervisor
                    ↓
        ┌───────────┼───────────┐
        ↓           ↓           ↓
     Agent A     Agent B     Agent C
        ↓           ↓           ↓
        └───────────┼───────────┘
                    ↓
                  Merge
```
Parallelism can reduce latency but introduces coordination complexity.
## 24. Agent Orchestration
Orchestration controls:
```text
Which agent runs
Which tool is used
What state is passed
When to retry
When to stop
How results are combined
```
Orchestration can be implemented using application code, workflow engines or agent frameworks.
## 25. Agent Frameworks
Frameworks can provide abstractions for:
```text
Tools
State
Workflows
Memory
Agent loops
Tracing
Human approval
```
Do not choose a framework before understanding the required architecture.
For interviews, explain the underlying pattern first and the framework second.
## 26. Human-in-the-Loop
Sensitive or high-impact operations may require human approval.
```text
Agent
 ↓
Proposed Action
 ↓
Human Approval
 ↓
Tool Execution
```
Examples:
```text
Delete cloud resource
Modify production configuration
Approve financial transaction
Send external communication
Restart critical infrastructure
```
## 27. Agent Reliability
Agents can fail in ways traditional deterministic software may not.
Examples:
```text
Wrong tool selection
Invalid arguments
Repeated tool calls
Infinite loops
Incorrect reasoning
Bad retrieval
Tool timeout
Conflicting results
```
Mitigations:
```text
Validation
Retries
Timeouts
Iteration limits
Tool schemas
Fallbacks
Human approval
Evaluation
```
## 28. Tool Failure Handling
A tool may fail because of:
```text
Timeout
Authentication failure
Invalid input
Rate limit
Service outage
Network error
```
A robust agent should distinguish:
```text
Retryable failure
Non-retryable failure
Authorization failure
Unknown failure
```
## 29. Retries
Retries should not blindly repeat every operation.
Consider:
```text
Retry count
Backoff
Idempotency
Error type
Time limit
Cost
```
Write operations require special care because repeating an action can cause duplicate side effects.
## 30. Idempotency
An operation is idempotent when repeating it produces the same intended final state.
This matters for agents because the model may retry actions.
Example:
```text
Set configuration = X
```
is generally safer to retry than:
```text
Charge customer $100
```
which could create duplicate side effects if not protected.
## 31. Guardrails
Agent guardrails can operate at multiple points:
```text
Input
 ↓
Planning
 ↓
Tool selection
 ↓
Tool arguments
 ↓
Tool execution
 ↓
Output
```
Examples:
```text
Schema validation
Policy checks
Allow lists
Content filtering
Permission checks
Human approval
```
## 32. Prompt Injection in Agents
Agents are especially sensitive to prompt injection because model output can influence tool calls.
Potential attack:
```text
Untrusted document
 ↓
Malicious instruction
 ↓
Agent follows instruction
 ↓
Sensitive tool call
```
Mitigation requires:
```text
Untrusted-data isolation
Tool authorization
Least privilege
Input validation
Output validation
Human approval
Monitoring
```
## 33. Agent Security
Important security areas:
```text
Identity
Authorization
Secrets
Tool permissions
Data isolation
Prompt injection
Audit logging
Network controls
Rate limiting
```
Never expose credentials directly to model context when a safer application-controlled mechanism exists.
## 34. Observability
Agent systems should record enough information to understand failures.
Useful telemetry:
```text
Request ID
Agent state
Model calls
Tool calls
Tool arguments
Tool results
Latency
Token usage
Errors
Retries
Final outcome
```
Sensitive data should be protected or redacted in logs.
## 35. Agent Tracing
Tracing helps visualize a multi-step execution:
```text
Request
 ↓
Agent
 ├── LLM call
 ├── Search
 ├── Database
 ├── LLM call
 └── Final response
```
Tracing is particularly useful for debugging latency and incorrect tool behavior.
## 36. Agent Evaluation
Evaluate more than the final answer.
Possible dimensions:
```text
Task success
Tool selection
Tool argument accuracy
Number of steps
Latency
Cost
Safety
Grounding
Recovery from failure
```
Representative scenarios are essential.
## 37. Agent Cost
Agent workflows can become expensive because one user request may produce multiple model and tool calls.
Consider:
```text
Number of iterations
Model selection
Prompt size
Tool calls
Retries
Context growth
Parallel execution
Caching
```
Set budgets where appropriate.
## 38. Agent Latency
Latency can come from:
```text
LLM calls
Tool calls
Retrieval
Sequential steps
Retries
External APIs
```
A useful optimization is to parallelize independent tasks when safe.
## 39. Agent vs RAG
```text
RAG
→ Retrieval architecture.

Agent
→ Dynamic decision-making and action architecture.
```
An agent can use RAG as a tool:
```text
Agent
 ├── RAG
 ├── Search
 ├── Database
 └── API
```
## 40. Agent vs Chatbot
```text
Chatbot
→ Primarily conversational interaction.

Agent
→ Can dynamically perform actions and use external capabilities.
```
A chatbot can contain agent-like capabilities, so the distinction depends on the actual architecture rather than the UI.
## 41. Common Interview Questions
### What is an AI agent?
A model-driven application that can select actions, use tools, observe results and continue toward a goal.
### What is tool calling?
A mechanism that allows a model-driven application to request execution of defined functions or external services.
### What is agent memory?
Information maintained across steps or interactions to support task execution or personalization.
### What is the difference between an agent and a workflow?
A workflow generally follows predefined steps, while an agent can dynamically select actions based on the current state and goal.
### When should you use an agent?
When the task benefits from dynamic decision-making, tool use or multi-step execution. Avoid agentic complexity when a deterministic workflow is sufficient.
### What is a multi-agent system?
A system where multiple specialized agents collaborate or coordinate to complete a larger task.
### Why are guardrails important?
Because model decisions can influence external systems and create real-world side effects.
### How do you prevent an agent from running forever?
Use iteration limits, timeouts, token or cost budgets, explicit completion criteria and cancellation mechanisms.
### How do you secure tool calls?
Use application-level authentication, authorization, least privilege, input validation, allow lists and auditing.
## 42. Senior-Level Scenarios
### Would you use an agent or a deterministic workflow?
Start with the business requirements. If the sequence is predictable, prefer a deterministic workflow for control and testability. Introduce agentic decision-making where dynamic behavior creates meaningful value.
### An agent repeatedly calls the same tool. What do you investigate?
Check the state being returned, tool results, stopping conditions, prompt instructions, iteration limits and whether the model can recognize successful completion.
### An agent wants to delete a production resource. What should happen?
The application should enforce authorization and policy checks. For high-impact actions, require explicit human approval before execution.
### How would you debug a failed agent task?
Trace the complete execution: model calls, selected tools, arguments, results, state transitions, retries and final output. Determine whether the failure originated in planning, tool execution, state management or generation.
### When would you choose multiple agents?
Only when specialization, isolation or parallelism provides a measurable benefit that justifies additional orchestration and evaluation complexity.
## 43. Production Agent Checklist
```text
☐ Clear business goal
☐ Defined tools
☐ Tool schemas
☐ Authentication
☐ Authorization
☐ Least privilege
☐ Input validation
☐ Output validation
☐ State management
☐ Memory strategy
☐ Iteration limits
☐ Timeouts
☐ Retry policy
☐ Idempotency
☐ Human approval where required
☐ Prompt-injection defenses
☐ Tracing
☐ Evaluation
☐ Cost controls
☐ Latency monitoring
```
## 44. Agent Interview Mental Model
Remember:
```text
GOAL
 ↓
STATE
 ↓
MODEL
 ↓
DECISION
 ↓
TOOL
 ↓
OBSERVATION
 ↓
NEXT DECISION
 ↓
VALIDATION
 ↓
COMPLETION
```
For production systems, extend it with:
```text
SECURITY
OBSERVABILITY
EVALUATION
COST
FAILURE HANDLING
```
## 45. Continue the Preparation
Use the other GenAI landing-page tiles:
- [Quick Start](/interview-prep/gen-ai/quick-start/)
- [Rapid Revision](/interview-prep/gen-ai/rapid-revision/)
- [Core Concepts](/interview-prep/gen-ai/core-concepts/)
- [LLM](/interview-prep/gen-ai/llm/)
- [Prompt Engineering](/interview-prep/gen-ai/prompt-engineering/)
- [RAG](/interview-prep/gen-ai/rag/)
- [Vector Databases](/interview-prep/gen-ai/vector-databases/)
- [Gen AI Architecture](/interview-prep/gen-ai/gen-ai-architecture/)
- [Troubleshooting](/interview-prep/gen-ai/troubleshooting/)
- [Senior-Level Scenarios](/interview-prep/gen-ai/senior-scenarios/)
- [Deep Dive](/interview-prep/gen-ai/deep-dive/)
