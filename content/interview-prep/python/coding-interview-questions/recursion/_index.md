---
title: ""
description: "Python recursion interview questions including factorial, Fibonacci and recursive traversal."
weight: 60
toc: true
---
# 🔁 Recursion
## What is recursion?
Recursion occurs when a function calls itself to solve smaller versions of the same problem.
Every recursive solution needs:
```text
Base case
Recursive case
```
## Factorial
```python
def factorial(n):
    if n == 0:
        return 1
    return n * factorial(n - 1)

print(factorial(5))
```
## Fibonacci
```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(6))
```
The simple Fibonacci implementation has exponential time complexity and is mainly useful for understanding recursion.
## Sum of a List
```python
def list_sum(numbers):
    if not numbers:
        return 0
    return numbers[0] + list_sum(numbers[1:])

print(list_sum([1, 2, 3, 4]))
```
## Interview Pitfall
Always identify the base case first. Without a terminating condition, recursion can continue until Python raises a recursion-related error.
