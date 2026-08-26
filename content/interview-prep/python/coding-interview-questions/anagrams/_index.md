---
title: "🔤 Anagrams"
description: "Python anagram interview questions using sorting and frequency counting."
weight: 90
toc: true
---

### What is an anagram?
Two strings are anagrams when they contain the same characters with the same frequencies, usually ignoring ordering.
## Using Sorting
```python
def are_anagrams(a, b):
    return sorted(a) == sorted(b)

print(are_anagrams("listen", "silent"))
```
### Complexity
Sorting makes the typical complexity O(n log n).
## Using `Counter`
```python
from collections import Counter

def are_anagrams(a, b):
    return Counter(a) == Counter(b)

print(are_anagrams("listen", "silent"))
```
### Complexity
The frequency-map approach is typically O(n) time with O(n) extra space.
## Group Anagrams
```python
from collections import defaultdict

words = ["eat", "tea", "tan", "ate", "nat", "bat"]
groups = defaultdict(list)
for word in words:
    key = tuple(sorted(word))
    groups[key].append(word)
print(list(groups.values()))
```
## Interview Tip
Ask whether spaces, punctuation and letter case should be ignored before writing the solution.
