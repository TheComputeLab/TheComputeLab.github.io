---
title: ""
description: "Python list coding interview questions and common patterns."
weight: 20
toc: true
---
# 📋 List Coding Questions
## Find the Largest Number
```python
numbers = [10, 4, 25, 8]
largest = numbers[0]
for number in numbers:
    if number > largest:
        largest = number
print(largest)
```
## Remove Duplicates
```python
numbers = [1, 2, 2, 3, 3, 4]
result = list(dict.fromkeys(numbers))
print(result)
```
## Find the Second Largest Number
```python
numbers = [10, 5, 20, 8, 20]
unique = sorted(set(numbers))
print(unique[-2])
```
## Move Zeros to the End
```python
numbers = [0, 1, 0, 3, 12]
non_zero = [x for x in numbers if x != 0]
result = non_zero + [0] * (len(numbers) - len(non_zero))
print(result)
```
## Two Sum
```python
numbers = [2, 7, 11, 15]
target = 9
seen = {}
for index, number in enumerate(numbers):
    needed = target - number
    if needed in seen:
        print(seen[needed], index)
        break
    seen[number] = index
```
### Complexity
The hash-map approach runs in O(n) time with O(n) extra space.
