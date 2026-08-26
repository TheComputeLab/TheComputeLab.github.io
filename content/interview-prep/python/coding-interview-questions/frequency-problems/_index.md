---
title: ""
description: "Python interview problems based on counting and frequency maps."
weight: 70
toc: true
---
# 🔢 Frequency Problems
Frequency questions are often solved efficiently using a dictionary or `collections.Counter`.
## Count Characters
```python
from collections import Counter

text = "banana"
counts = Counter(text)
print(counts)
```
## Find the Most Common Character
```python
from collections import Counter

text = "banana"
result = Counter(text).most_common(1)[0]
print(result)
```
## Check Whether Two Lists Have the Same Frequencies
```python
from collections import Counter

a = [1, 2, 2, 3]
b = [2, 3, 2, 1]
print(Counter(a) == Counter(b))
```
## Interview Pattern
When you see:
```text
frequency
count
occurrences
duplicates
most common
same elements
```
Think about a frequency map.
