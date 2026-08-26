---
title: "🧩 Common Easy / Medium Problems"
description: "A practical collection of common Python coding interview problems and patterns."
weight: 110
toc: true
---

## FizzBuzz
```python
for number in range(1, 16):
    if number % 15 == 0:
        print("FizzBuzz")
    elif number % 3 == 0:
        print("Fizz")
    elif number % 5 == 0:
        print("Buzz")
    else:
        print(number)
```
## Find Missing Number
```python
numbers = [0, 1, 3, 4]
n = len(numbers)
expected = n * (n + 1) // 2
print(expected - sum(numbers))
```
## Valid Parentheses
```python
def is_valid(text):
    stack = []
    pairs = {")": "(", "]": "[", "}": "{"}
    for char in text:
        if char in "([{":
            stack.append(char)
        elif not stack or stack.pop() != pairs[char]:
            return False
    return not stack

print(is_valid("({[]})"))
```
## Merge Two Sorted Lists
```python
def merge(a, b):
    result = []
    i = j = 0
    while i < len(a) and j < len(b):
        if a[i] <= b[j]:
            result.append(a[i])
            i += 1
        else:
            result.append(b[j])
            j += 1
    result.extend(a[i:])
    result.extend(b[j:])
    return result

print(merge([1, 3, 5], [2, 4, 6]))
```
## Find Maximum Subarray Sum
Kadane's algorithm solves the maximum subarray sum problem in O(n) time.
```python
def max_subarray(numbers):
    current = best = numbers[0]
    for number in numbers[1:]:
        current = max(number, current + number)
        best = max(best, current)
    return best

print(max_subarray([-2, 1, -3, 4, -1, 2, 1, -5, 4]))
```
## Reverse Words
```python
text = "Python interview preparation"
result = " ".join(text.split()[::-1])
print(result)
```
## Count Vowels
```python
text = "interview"
vowels = set("aeiou")
count = sum(1 for char in text.lower() if char in vowels)
print(count)
```
## Find Common Elements
```python
a = [1, 2, 3, 4]
b = [3, 4, 5, 6]
print(list(set(a) & set(b)))
```
## Interview Strategy
For easy and medium problems, practice recognizing the pattern rather than memorizing individual solutions:
```text
Hash map
Set
Two pointers
Sliding window
Stack
Binary search
Sorting
Recursion
Dynamic programming
```
