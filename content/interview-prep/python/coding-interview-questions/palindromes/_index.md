---
title: ""
description: "Python palindrome interview questions and two-pointer techniques."
weight: 100
toc: true
---
# 🔄 Palindromes
### What is a palindrome?
A palindrome reads the same forward and backward.
Examples:
```text
level
radar
madam
```
## Simple Solution
```python
def is_palindrome(text):
    return text == text[::-1]

print(is_palindrome("level"))
```
## Two-Pointer Solution
```python
def is_palindrome(text):
    left = 0
    right = len(text) - 1
    while left < right:
        if text[left] != text[right]:
            return False
        left += 1
        right -= 1
    return True

print(is_palindrome("level"))
```
### Complexity
Time: O(n)
Space: O(1) for the two-pointer approach.
## Interview Variant
Check a phrase while ignoring spaces and case.
```python
def is_palindrome(text):
    cleaned = "".join(char.lower() for char in text if char.isalnum())
    return cleaned == cleaned[::-1]
```
## Interview Pattern
For palindrome problems, consider:
```text
Reverse comparison
Two pointers
Normalization
Case sensitivity
Special characters
```
