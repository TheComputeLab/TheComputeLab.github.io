---
title: ""
description: "Python searching interview problems including linear and binary search."
weight: 40
toc: true
---
# 🔎 Searching
## Linear Search
```python
numbers = [4, 8, 2, 10, 6]
target = 10
found = False
for number in numbers:
    if number == target:
        found = True
        break
print(found)
```
### Complexity
Time: O(n)
Space: O(1)
## Binary Search
Binary search requires sorted data.
```python
def binary_search(numbers, target):
    left = 0
    right = len(numbers) - 1
    while left <= right:
        mid = (left + right) // 2
        if numbers[mid] == target:
            return mid
        if numbers[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

print(binary_search([1, 3, 5, 7, 9], 7))
```
### Complexity
Time: O(log n)
Space: O(1)
## Interview Question
### When would you use binary search?
When the search space is ordered and the problem allows repeatedly eliminating half of the remaining search space.
