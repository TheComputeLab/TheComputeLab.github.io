---
title: "GIL"
description: "Python Global Interpreter Lock interview preparation and practical concurrency implications."
weight: 30
toc: true
---
# 🔒 Global Interpreter Lock
## What is the GIL?
In CPython, the Global Interpreter Lock (GIL) is a mechanism that allows only one thread at a time to execute Python bytecode within a process.
## Why does the GIL matter?
For CPU-bound Python code, multiple threads do not generally provide parallel execution of Python bytecode in standard CPython.
For I/O-bound workloads, threads can still be useful because execution can wait on I/O and the interpreter can allow other work to proceed.
## CPU-Bound Example
```python
def cpu_work():
    total = 0
    for number in range(10_000_000):
        total += number
    return total
```
For CPU-heavy work, multiprocessing or other approaches may be more appropriate depending on the workload.
## I/O-Bound Example
```text
API calls
File operations
Network communication
Waiting for external systems
```
Threads can be useful when the workload spends significant time waiting.
## Interview Question
### Does the GIL make Python unable to do concurrency?
No. Concurrency is still possible with threads, processes and asynchronous programming. The GIL specifically affects execution of Python bytecode in CPython threads.
## Senior-Level Point
Always distinguish:
```text
Concurrency
Parallelism
CPU-bound work
I/O-bound work
CPython-specific behavior
```
