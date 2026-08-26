---
title: ""
description: "Python multiprocessing interview preparation covering processes, pools, IPC and CPU-bound workloads."
weight: 50
toc: true
---
# 🧩 Multiprocessing
## What is multiprocessing?
Multiprocessing runs work in separate processes. Each process has its own memory space.
## Basic Example
```python
from multiprocessing import Process

def worker():
    print("Worker running")

process = Process(target=worker)
process.start()
process.join()
```
## Process Pool
For repeated independent work, a process pool can simplify parallel execution:
```python
from multiprocessing import Pool

def square(number):
    return number * number

with Pool() as pool:
    results = pool.map(square, [1, 2, 3, 4])

print(results)
```
## Why Use Multiprocessing?
It can be appropriate for CPU-bound workloads where multiple processes can execute on multiple CPU cores.
## Trade-offs
```text
Process startup overhead
Memory overhead
Data serialization
Inter-process communication
More complex debugging
Process lifecycle management
```
## Interview Question
### How do processes communicate?
Possible mechanisms include:
```text
Queues
Pipes
Shared memory
Managers
Files
External systems
```
The right mechanism depends on performance, consistency and architecture requirements.
## Important Consideration
When using multiprocessing, protect process-starting code appropriately, especially in applications where the multiprocessing start method requires it:
```python
if __name__ == "__main__":
    ...
```
