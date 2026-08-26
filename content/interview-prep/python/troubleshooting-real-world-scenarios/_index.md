---
title: ""
description: "Interview-focused Python troubleshooting covering script failures, memory usage, performance, exception handling, API failures, file processing and production debugging."
weight: 80
toc: true
---
# 🛠️ Troubleshooting & Real-World Scenarios
This section focuses on how to reason about Python failures in real engineering environments. In interviews, explain what you would check, why you would check it and how you would safely fix the problem.
## 1. Script Fails
### Interview Question
**A Python script suddenly fails in production. What would you check first?**
### Recommended Answer
Start by identifying the exact failure rather than immediately changing the code.
```text
1. Check the error message and traceback
2. Check application logs
3. Identify when the failure started
4. Identify what changed
5. Reproduce the failure if possible
6. Check inputs and external dependencies
7. Validate configuration
8. Apply and test the fix
```
### Example
```python
try:
    process_data()
except Exception:
    logging.exception("Data processing failed")
```
`logging.exception()` records the traceback, which is useful during debugging.
### Senior-Level Point
Ask:
```text
Did the code change?
Did the input change?
Did the environment change?
Did a dependency change?
Did permissions change?
Did an external service change?
```
## 2. Memory Usage
### Interview Question
**A Python script consumes increasing amounts of memory. How would you investigate?**
### Recommended Answer
First determine whether the memory increase is expected or indicates a leak or inefficient data handling.
Check:
```text
Input size
Data structures
Large lists
Cached objects
Global references
Unreleased resources
Long-running processes
Repeated object creation
```
### Common Problem
Loading a very large file into memory:
```python
with open("large.log") as file:
    data = file.read()
```
A more memory-efficient approach is to process it line by line:
```python
with open("large.log") as file:
    for line in file:
        process(line)
```
### Interview Point
Generators can also help avoid materializing large datasets:
```python
def read_lines(path):
    with open(path) as file:
        for line in file:
            yield line
```
## 3. Slow Script
### Interview Question
**A Python script that normally completes in two minutes now takes twenty minutes. What would you check?**
### Recommended Answer
Do not optimize blindly. First identify where the time is being spent.
```text
1. Measure execution time
2. Identify the slow function
3. Check input size
4. Check loops and nested loops
5. Check file or network operations
6. Check external APIs
7. Check database operations
8. Profile the application
```
### Simple Timing
```python
import time

start = time.perf_counter()
process_data()
elapsed = time.perf_counter() - start

print(f"Elapsed: {elapsed:.2f}s")
```
### Interview Point
A common performance issue is repeatedly performing an O(n) lookup inside another loop.
A set or dictionary may reduce repeated lookups to average O(1).
## 4. Exception Handling
### Interview Question
**How should exceptions be handled in production Python code?**
### Recommended Answer
Catch exceptions at a level where you can actually handle or add useful context. Avoid hiding failures with a broad `except` that silently ignores errors.
### Good Example
```python
try:
    response = call_api()
except TimeoutError as error:
    logging.error("API timed out: %s", error)
    raise
```
### Avoid
```python
try:
    process()
except:
    pass
```
This hides the original failure and makes production debugging difficult.
### Multiple Exceptions
```python
try:
    value = int(text)
except ValueError:
    logging.error("Invalid numeric value")
except TypeError:
    logging.error("Unexpected input type")
```
### `finally`
Use `finally` for cleanup that must happen whether an operation succeeds or fails.
```python
resource = acquire()
try:
    use(resource)
finally:
    release(resource)
```
### Interview Point
Good exception handling should provide:
```text
Correct exception type
Useful context
Logging
Cleanup
Safe recovery or propagation
```
## 5. API Failure
### Interview Question
**Your Python automation script calls an API and the API starts failing. What would you check?**
### Recommended Answer
Separate client-side problems from server-side or network problems.
Check:
```text
HTTP status code
Response body
Authentication
Authorization
Endpoint URL
Request parameters
Request headers
Timeouts
DNS / network connectivity
Rate limits
API availability
```
### Example
```python
import requests

try:
    response = requests.get(
        "https://api.example.com/data",
        timeout=10
    )
    response.raise_for_status()
except requests.Timeout:
    logging.error("API request timed out")
except requests.HTTPError as error:
    logging.error("API returned an HTTP error: %s", error)
except requests.RequestException as error:
    logging.error("API request failed: %s", error)
```
### Retry Strategy
Retries can be appropriate for transient failures, but should not blindly retry every error.
Common transient cases may include:
```text
Temporary network failures
Timeouts
Some 5xx responses
Rate limiting when the API provides retry guidance
```
Use bounded retries and backoff rather than an infinite retry loop.
## 6. File Processing Failure
### Interview Question
**A Python script processes thousands of files and suddenly fails on one file. How would you troubleshoot it?**
### Recommended Answer
Identify the specific file and determine whether the problem is caused by the content, encoding, permissions, path, file type or application logic.
Check:
```text
File exists
File permissions
File size
File encoding
File format
Corrupt content
Unexpected characters
Path length or invalid path
Concurrent access
```
### Example
```python
from pathlib import Path

path = Path("input.txt")

try:
    text = path.read_text(encoding="utf-8")
except FileNotFoundError:
    logging.error("File does not exist: %s", path)
except PermissionError:
    logging.error("Permission denied: %s", path)
except UnicodeDecodeError:
    logging.error("Unable to decode file: %s", path)
```
### Production Pattern
When processing many independent files, decide whether one bad file should:
```text
Stop the entire job
or
Be logged and skipped while processing continues
```
The correct choice depends on business requirements and data integrity.
## 7. Production Debugging
### Interview Question
**How would you debug a Python application in production without making the problem worse?**
### Recommended Answer
Use a controlled, evidence-driven approach.
```text
1. Confirm the impact
2. Check logs and metrics
3. Identify the affected component
4. Compare healthy vs failing executions
5. Check recent changes
6. Check dependencies
7. Reproduce safely if possible
8. Apply the smallest safe remediation
9. Validate recovery
10. Document the root cause
```
### What would you look for in logs?
```text
Timestamp
Severity
Request or job identifier
Exception
Traceback
Input context
External service response
Execution duration
Affected resource
```
### Use correlation IDs
For distributed systems, a request or job identifier helps connect events across services.
```python
job_id = "JOB-12345"
logging.info("Starting job %s", job_id)
```
### Production Debugging Principle
Do not change multiple things at once if you are trying to identify the root cause. Make one controlled change, observe the result and document the outcome.
## Real-World Scenario Questions
### Scenario 1: Script works manually but fails from a scheduler
Check:
```text
Environment variables
Working directory
Python interpreter
Virtual environment
File permissions
PATH
User account
Configuration paths
Credentials
```
### Scenario 2: Script works on one server but fails on another
Compare:
```text
Python version
Installed packages
OS version
Environment variables
Configuration
Network access
Permissions
File paths
External dependencies
```
### Scenario 3: Script succeeds but produces incorrect results
Do not assume the code executed correctly just because it exited with code 0.
Check:
```text
Input data
Validation
Business logic
Edge cases
Parsing
Data types
Output assumptions
```
### Scenario 4: Automation sometimes succeeds and sometimes fails
Look for nondeterministic dependencies:
```text
Network
Timing
Race conditions
External APIs
Resource availability
Concurrency
Transient failures
Rate limits
```
## Senior-Level Troubleshooting Framework
When asked **"What would you check?"**, use this sequence:
```text
OBSERVE
↓
REPRODUCE
↓
ISOLATE
↓
IDENTIFY ROOT CAUSE
↓
REMEDIATE
↓
VALIDATE
↓
PREVENT RECURRENCE
```
### What does each step mean?
```text
Observe       → Gather logs, metrics and symptoms
Reproduce     → Confirm the behavior
Isolate       → Narrow down the failing component
Root Cause    → Identify why it failed
Remediate     → Apply a safe fix
Validate      → Confirm the system works
Prevent       → Add monitoring, tests or safeguards
```
## Quick Troubleshooting Checklist
```text
Script failure
↓
Memory usage
↓
Performance
↓
Exceptions
↓
API failures
↓
File processing
↓
Production debugging
```
## Interview Answer Pattern
For real-world troubleshooting questions, answer:
```text
1. What is the symptom?
2. What evidence would you collect?
3. What are the likely failure domains?
4. How would you isolate the problem?
5. What would you change?
6. How would you validate the fix?
7. How would you prevent recurrence?
```
