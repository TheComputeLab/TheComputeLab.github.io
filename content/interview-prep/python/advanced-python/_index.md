---
title: " 🔧 Advanced Python"
description: "Interview-focused advanced Python preparation covering comprehensions, functional programming, iterators, generators, decorators, context managers, arguments and object copying."
weight: 50
toc: true
---

A practical interview guide to Python features that commonly appear in coding, automation and senior-level technical interviews.
## 1. List / Dict / Set Comprehensions
### What is a list comprehension?
A compact way to create a list from an iterable.
```python
squares = [x * x for x in range(5)]
```
Equivalent loop:
```python
squares = []
for x in range(5):
    squares.append(x * x)
```
### Can a list comprehension contain a condition?
Yes.
```python
even_numbers = [x for x in range(10) if x % 2 == 0]
```
### What is a dictionary comprehension?
It creates a dictionary using an expression.
```python
squares = {x: x * x for x in range(5)}
```
### What is a set comprehension?
It creates a set of unique results.
```python
unique_squares = {x * x for x in range(5)}
```
### Nested comprehension
```python
matrix = [[1, 2], [3, 4]]
values = [value for row in matrix for value in row]
```
### Interview pitfall
Comprehensions are concise, but overly complex nested comprehensions can reduce readability. Use a normal loop when the logic becomes difficult to understand.
## 2. Lambda
### What is a lambda function?
A lambda is a small anonymous function containing an expression.
```python
square = lambda x: x * x
print(square(5))
```
### Lambda with `sorted()`
```python
users = [
    {"name": "A", "age": 30},
    {"name": "B", "age": 20}
]
users.sort(key=lambda user: user["age"])
```
### Lambda limitations
A lambda is intended for simple expressions. For multi-step logic, a normal `def` function is generally clearer.
## 3. `map()`, `filter()` and `reduce()`
### What is `map()`?
`map()` applies a function to every item in an iterable and returns an iterator.
```python
numbers = [1, 2, 3, 4]
squares = map(lambda x: x * x, numbers)
print(list(squares))
```
### What is `filter()`?
`filter()` keeps items for which a function returns a truthy result.
```python
numbers = [1, 2, 3, 4, 5]
even = filter(lambda x: x % 2 == 0, numbers)
print(list(even))
```
### What is `reduce()`?
`reduce()` repeatedly applies a function to combine items into a single result. It is provided by `functools`.
```python
from functools import reduce

numbers = [1, 2, 3, 4]
total = reduce(lambda a, b: a + b, numbers)
print(total)
```
### `map()` vs list comprehension
Both can transform data.
```python
squares = [x * x for x in numbers]
```
A list comprehension is often easier to read for straightforward transformations.
### Interview point
`map()` and `filter()` return iterators in Python 3, so they are evaluated lazily when consumed.
## 4. Iterators
### What is an iterator?
An iterator is an object that produces values one at a time and implements the iterator protocol.
The key methods are:
```text
__iter__()
__next__()
```
### Example
```python
numbers = iter([10, 20, 30])

print(next(numbers))
print(next(numbers))
print(next(numbers))
```
When no values remain, `next()` raises `StopIteration`.
### Iterable vs iterator
An iterable can provide an iterator.
```python
items = [1, 2, 3]
iterator = iter(items)
```
The list is iterable. The object returned by `iter()` is an iterator.
### Why are iterators useful?
They allow data to be processed one item at a time instead of requiring all results to be held in memory.
## 5. Generators
### What is a generator?
A generator is a convenient way to create an iterator using `yield`.
```python
def numbers():
    yield 1
    yield 2
    yield 3
```
```python
for number in numbers():
    print(number)
```
### `yield` vs `return`
`return` ends a function and returns a result.
`yield` pauses a generator and preserves its state so execution can continue later.
### Why use generators?
Generators are useful for:
```text
Large files
Large datasets
Streaming data
Pipelines
Memory-efficient processing
```
### Generator expression
```python
squares = (x * x for x in range(1000000))
```
Unlike a list comprehension, this does not create the complete list immediately.
### Interview comparison
```text
List comprehension → creates the collection immediately
Generator          → produces values on demand
```
## 6. Decorators
### What is a decorator?
A decorator is a callable that modifies or extends the behavior of another function or class without changing its core implementation.
### Basic example
```python
def log_call(function):
    def wrapper():
        print("Calling function")
        result = function()
        print("Function complete")
        return result
    return wrapper

@log_call
def run():
    print("Running")

run()
```
### How does `@log_call` work?
This:
```python
@log_call
def run():
    pass
```
is equivalent to:
```python
def run():
    pass

run = log_call(run)
```
### Decorator with arguments
A general-purpose decorator can accept `*args` and `**kwargs`.
```python
from functools import wraps

def log_call(function):
    @wraps(function)
    def wrapper(*args, **kwargs):
        print("Calling", function.__name__)
        return function(*args, **kwargs)
    return wrapper
```
### Why use `functools.wraps`?
It preserves useful metadata such as the wrapped function's name and documentation.
### Common decorator use cases
```text
Logging
Timing
Authentication
Authorization
Caching
Validation
Retry logic
```
## 7. Context Managers
### What is a context manager?
A context manager controls setup and cleanup around a block of code.
The `with` statement is the most common way to use one.
```python
with open("data.txt") as file:
    content = file.read()
```
The file is automatically closed after the block.
### Why use context managers?
They are useful for reliable resource management:
```text
Files
Database connections
Locks
Network resources
Temporary resources
```
### How does a context manager work?
A class-based context manager commonly implements:
```text
__enter__()
__exit__()
```
Example:
```python
class Resource:
    def __enter__(self):
        print("Acquire resource")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("Release resource")
```
```python
with Resource():
    print("Using resource")
```
### Can context managers be created with a decorator?
Yes. `contextlib.contextmanager` can turn a generator function into a context manager.
```python
from contextlib import contextmanager

@contextmanager
def resource():
    print("Acquire")
    try:
        yield
    finally:
        print("Release")

with resource():
    print("Using resource")
```
## 8. `*args` / `**kwargs`
### What is `*args`?
It collects variable-length positional arguments into a tuple.
```python
def add(*args):
    return sum(args)

print(add(1, 2, 3))
```
### What is `**kwargs`?
It collects variable-length keyword arguments into a dictionary.
```python
def show(**kwargs):
    print(kwargs)

show(name="Python", version=3)
```
### Can they be combined?
Yes.
```python
def function(*args, **kwargs):
    print(args)
    print(kwargs)
```
### What is argument unpacking?
`*` unpacks an iterable.
```python
values = [10, 20, 30]
print(*values)
```
`**` unpacks a dictionary into keyword arguments.
```python
data = {"name": "Python"}
function(**data)
```
### Interview point
Remember:
```text
*args   → positional arguments → tuple
**kwargs → keyword arguments → dictionary
```
## 9. Shallow vs Deep Copy
### Why does copying matter?
With nested mutable objects, assigning or copying incorrectly can cause changes in one object to affect another.
### Assignment is not a copy
```python
a = [1, 2, 3]
b = a
```
Both names refer to the same list.
### What is a shallow copy?
A shallow copy creates a new outer object but keeps references to nested objects.
```python
import copy

original = [[1, 2], [3, 4]]
shallow = copy.copy(original)
```
The outer lists are different, but nested lists are shared.
### What is a deep copy?
A deep copy recursively copies nested objects.
```python
import copy

original = [[1, 2], [3, 4]]
deep = copy.deepcopy(original)
```
Changes to nested objects in `deep` do not affect the corresponding nested objects in `original`.
### Demonstration
```python
import copy

original = [[1, 2], [3, 4]]

shallow = copy.copy(original)
deep = copy.deepcopy(original)

original[0][0] = 99

print(shallow)
print(deep)
```
The shallow copy sees the nested change because the nested list is shared. The deep copy has its own nested objects.
### Shallow vs deep copy
```text
Assignment
    ↓
Same object

Shallow copy
    ↓
New outer object
    ↓
Nested objects may be shared

Deep copy
    ↓
New outer object
    ↓
Nested objects recursively copied
```
## Advanced Python Interview Comparison
| Topic | Key idea |
|---|---|
| List comprehension | Build lists concisely |
| Dict comprehension | Build dictionaries concisely |
| Set comprehension | Build unique-value sets |
| Lambda | Small anonymous function |
| `map()` | Transform iterable values |
| `filter()` | Select values based on a condition |
| `reduce()` | Combine values into one result |
| Iterator | Produces values one at a time |
| Generator | Convenient lazy iterator |
| Decorator | Extends or modifies callable behavior |
| Context manager | Controls setup and cleanup |
| `*args` | Variable positional arguments |
| `**kwargs` | Variable keyword arguments |
| Shallow copy | New outer object, nested references may remain shared |
| Deep copy | Recursively copies nested objects |
## Common Interview Questions
### Generator vs iterator?
A generator is a convenient way to create an iterator, typically using `yield`. An iterator is an object implementing the iterator protocol.
### Generator vs list?
A generator produces values lazily, while a list stores all its elements immediately.
### Decorator vs context manager?
A decorator modifies or wraps callable behavior. A context manager manages setup and cleanup around a block of code.
### `map()` vs `filter()`?
`map()` transforms each input item. `filter()` selects items based on a condition.
### Why use `reduce()`?
Use it when a sequence needs to be repeatedly combined into one result. In many cases, a built-in such as `sum()` or a loop can be clearer.
### Why use `deepcopy()`?
Use it when nested mutable objects must be independent rather than sharing references.
### What is the main performance benefit of generators?
They can reduce memory usage because values are produced on demand rather than materializing the entire result.
## Quick Advanced Interview Checklist
```text
Comprehensions
↓
Lambda
↓
map / filter / reduce
↓
Iterators
↓
Generators
↓
Decorators
↓
Context Managers
↓
*args / **kwargs
↓
Shallow vs Deep Copy
```
## Interview Answer Pattern
For advanced Python questions, explain:
```text
1. What is it?
2. How does it work?
3. Why is it useful?
4. Show a small example
5. Mention a practical use case
6. Mention a common pitfall
```
