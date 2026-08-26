---
title: ""
description: "Python sorting interview questions and common sorting patterns."
weight: 50
toc: true
---
# 🔃 Sorting
## Sort a List
```python
numbers = [5, 2, 8, 1, 3]
numbers.sort()
print(numbers)
```
## `sort()` vs `sorted()`
`sort()` modifies the existing list.
```python
numbers.sort()
```
`sorted()` returns a new sorted result.
```python
result = sorted(numbers)
```
## Sort by a Key
```python
users = [
    {"name": "A", "age": 30},
    {"name": "B", "age": 20}
]
result = sorted(users, key=lambda user: user["age"])
print(result)
```
## Find the Kth Largest Value
```python
numbers = [3, 1, 5, 2, 4]
k = 2
result = sorted(numbers, reverse=True)[k - 1]
print(result)
```
## Interview Tip
Before sorting, ask whether sorting is actually necessary. A hash map, heap, two-pointer approach or counting technique may provide better performance.
