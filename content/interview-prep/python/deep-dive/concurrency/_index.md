---
title: ""
description: "Python concurrency interview preparation covering threads, processes and asynchronous execution."
weight: 40
toc: true
---
# ⚙️ Concurrency
## What is concurrency?
Concurrency means multiple tasks can make progress during overlapping periods. It does not necessarily mean tasks execute simultaneously on multiple CPU cores.
## Concurrency vs Parallelism
```text
Concurrency → tasks overlap in progress
Parallelism  → tasks execute at the same time
```
## Python Concurrency Options
```text
threading
multiprocessing
asyncio
concurrent.futures
```
## When to Use Threads
Threads can be useful for I/O-bound work:
```text
Network calls
File I/O
Waiting on services
```
## When to Use Processes
Processes are useful when CPU-bound work needs parallel execution and the workload justifies process overhead.
## When to Use Asyncio
Asyncio is useful when many operations spend time waiting on asynchronous I/O and the libraries involved support async execution.
## Interview Question
### How would you choose a concurrency model?
Consider:
```text
CPU-bound or I/O-bound?
Number of tasks?
Blocking libraries?
Latency requirements?
Shared state?
Memory overhead?
Failure handling?
Complexity?
```
## Senior-Level Point
Do not choose concurrency simply because it sounds faster. Measure the bottleneck and choose the model that matches the workload.
