---
title: ""
description: "Python string coding interview questions and common patterns."
weight: 10
toc: true
---
# 🔤 String Coding Questions
## Reverse a String
### Question
Reverse a string without using `reversed()`.
```python
text = "Python"
result = text[::-1]
print(result)
```
### Complexity
Time: O(n)
Space: O(n)
## Check Whether a String Contains Only Digits
```python
text = "12345"
print(text.isdigit())
```
## Count Characters
```python
text = "hello"
counts = {}
for char in text:
    counts[char] = counts.get(char, 0) + 1
print(counts)
```
## First Non-Repeating Character
```python
text = "swiss"
counts = {}
for char in text:
    counts[char] = counts.get(char, 0) + 1
for char in text:
    if counts[char] == 1:
        print(char)
        break
```
## Remove Duplicate Characters
```python
text = "programming"
result = ""
seen = set()
for char in text:
    if char not in seen:
        seen.add(char)
        result += char
print(result)
```
## Interview Tip
For string problems, think about:
```text
Hash map
Two pointers
Sliding window
Sorting
String slicing
```
