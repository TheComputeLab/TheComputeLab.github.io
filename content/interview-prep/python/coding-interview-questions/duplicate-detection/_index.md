---
title: ""
description: "Python coding interview questions for detecting duplicate values."
weight: 80
toc: true
---
# 🔁 Duplicate Detection
## Detect Any Duplicate
```python
numbers = [1, 2, 3, 2]
seen = set()
duplicate = None
for number in numbers:
    if number in seen:
        duplicate = number
        break
    seen.add(number)
print(duplicate)
```
### Complexity
Time: O(n)
Space: O(n)
## Return All Duplicates
```python
numbers = [1, 2, 3, 2, 4, 3, 5]
seen = set()
duplicates = set()
for number in numbers:
    if number in seen:
        duplicates.add(number)
    else:
        seen.add(number)
print(duplicates)
```
## Find Duplicates with `Counter`
```python
from collections import Counter

numbers = [1, 2, 2, 3, 3, 3]
counts = Counter(numbers)
duplicates = [number for number, count in counts.items() if count > 1]
print(duplicates)
```
## Interview Question
### Why use a set?
Set membership is average-case O(1), making it a common choice for duplicate detection.
