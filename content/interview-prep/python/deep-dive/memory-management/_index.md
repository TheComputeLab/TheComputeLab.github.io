---
title: ""
description: "Python memory management interview preparation covering references, garbage collection and object allocation."
weight: 20
toc: true
---
# 🧠 Memory Management
## How does Python manage memory?
Python manages objects automatically. In CPython, reference counting is a major mechanism, with cyclic garbage collection handling reference cycles.
## Reference Counting
A simplified example:
```python
a = []
b = a
```
Both names reference the same list object.
When references to an object are removed and its reference count reaches zero in CPython, the object can generally be deallocated.
## Garbage Collection
Reference counting alone cannot handle cycles such as:
```python
a = []
a.append(a)
```
Python's cyclic garbage collector can detect and collect certain unreachable reference cycles.
## `del` Does Not Always Delete the Object
```python
items = [1, 2, 3]
other = items

del items

print(other)
```
`del items` removes the name `items`; the object remains reachable through `other`.
## Memory-Efficient Processing
Avoid loading unnecessarily large datasets into memory:
```python
with open("large.log") as file:
    for line in file:
        process(line)
```
Generators are also useful for lazy processing.
## Interview Question
### How would you investigate high memory usage?
Check:
```text
Input size
Object lifetime
Large containers
Caches
Reference retention
Reference cycles
Repeated allocations
External resources
```
Then profile memory rather than guessing.
