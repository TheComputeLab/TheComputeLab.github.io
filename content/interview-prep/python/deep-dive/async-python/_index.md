---
title: ""
description: "Async Python interview preparation covering asyncio, coroutines, event loops and non-blocking I/O."
weight: 60
toc: true
---
# ⚡ Async Python
## What is asynchronous programming?
Asynchronous programming allows a program to start an operation and continue making progress while waiting for asynchronous I/O to complete.
## Basic Example
```python
import asyncio

async def task():
    await asyncio.sleep(1)
    return "done"

async def main():
    result = await task()
    print(result)

asyncio.run(main())
```
## What is a coroutine?
A function defined with `async def` produces a coroutine object when called. It can suspend at `await` points and allow other asynchronous tasks to run.
## Concurrent Tasks
```python
import asyncio

async def worker(name):
    await asyncio.sleep(1)
    return name

async def main():
    results = await asyncio.gather(
        worker("A"),
        worker("B"),
        worker("C")
    )
    print(results)

asyncio.run(main())
```
## What is the event loop?
The event loop coordinates asynchronous tasks and I/O operations.
A simplified model:
```text
Event loop
↓
Run coroutine
↓
Await I/O
↓
Task yields control
↓
Other task runs
↓
I/O completes
↓
Original task resumes
```
## Common Mistake
Calling blocking code inside an async application can block the event loop and reduce concurrency.
## Interview Question
### Asyncio vs threads?
Asyncio is especially useful for high numbers of asynchronous I/O operations when the libraries support async interfaces. Threads may be simpler when existing libraries are blocking or when the workload does not fit naturally into an async design.
