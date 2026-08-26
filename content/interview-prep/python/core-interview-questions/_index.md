---
title: " 🎯 Python Core Interview Questions"
description: "Interview-focused Python questions and answers covering variables, data types, strings, collections, loops, functions, arguments, scope, modules, packages and file handling."
weight: 30
toc: true
---

This section is designed for quick interview preparation. Focus on understanding the concept, giving a simple example and explaining when you would use it.
## 1. Variables & Data Types
### What is a variable in Python?
A variable is a name that refers to an object in memory.
```python
name = "Python"
age = 30
```
Python is dynamically typed, so the type does not need to be declared explicitly.
### What are the common built-in data types?
```text
int
float
bool
str
list
tuple
set
dict
NoneType
```
### What is dynamic typing?
Python determines the type of an object at runtime.
```python
value = 10
value = "Python"
```
The same variable name can refer to objects of different types.
### Mutable vs immutable objects?
Mutable objects can be changed after creation:
```text
list
set
dict
```
Immutable objects cannot be changed after creation:
```text
int
float
bool
str
tuple
```
### How do you check the type of an object?
```python
value = 10
print(type(value))
```
### What is type casting?
Converting a value from one type to another.
```python
age = int("30")
price = float("10.5")
text = str(100)
```
### What is the difference between `None`, `False` and `0`?
They are different values. `None` represents the absence of a value.
## 2. Strings
### What is a string?
A string is an immutable sequence of characters.
```python
name = "Python"
```
### How do you access characters?
```python
text = "Python"
print(text[0])
print(text[-1])
```
### What is string slicing?
```python
text = "Python"
print(text[0:3])
print(text[::-1])
```
### Common string methods
```python
text.lower()
text.upper()
text.strip()
text.split()
text.replace("Python", "Java")
text.find("t")
text.count("o")
```
### How do you join strings?
```python
words = ["Python", "Interview", "Prep"]
result = " ".join(words)
```
### What is an f-string?
An f-string provides convenient string formatting.
```python
name = "Python"
version = 3
message = f"{name} version {version}"
```
### Why are strings immutable?
Once a string object is created, its contents cannot be changed. Operations that appear to modify a string create a new string.
## 3. Lists
### What is a list?
A list is an ordered, mutable collection.
```python
numbers = [1, 2, 3]
```
### Can a list contain different data types?
Yes.
```python
items = [10, "Python", True, 3.14]
```
### Common list methods
```python
items.append(value)
items.extend(values)
items.insert(index, value)
items.remove(value)
items.pop()
items.sort()
items.reverse()
```
### `append()` vs `extend()`?
`append()` adds one object.
```python
items = [1, 2]
items.append([3, 4])
# [1, 2, [3, 4]]
```
`extend()` adds elements from an iterable.
```python
items = [1, 2]
items.extend([3, 4])
# [1, 2, 3, 4]
```
### What is a list comprehension?
A compact way to create a list.
```python
squares = [x * x for x in range(5)]
```
## 4. Tuples
### What is a tuple?
A tuple is an ordered, immutable collection.
```python
point = (10, 20)
```
### Why use a tuple?
Tuples are useful when the collection should not be modified.
### How do you create a single-element tuple?
A trailing comma is required.
```python
value = (10,)
```
### Common tuple methods
```python
point.count(10)
point.index(20)
```
## 5. Sets
### What is a set?
A set is an unordered collection of unique elements.
```python
values = {1, 2, 3}
```
### Why use a set?
Sets are useful for removing duplicates and performing membership and mathematical set operations.
### `remove()` vs `discard()`?
`remove()` raises an error if the element does not exist. `discard()` does not.
```python
values.remove(2)
values.discard(10)
```
### Common set operations
```python
a.union(b)
a.intersection(b)
a.difference(b)
```
Operators:
```python
a | b
a & b
a - b
```
## 6. Dictionaries
### What is a dictionary?
A dictionary stores key-value pairs.
```python
user = {
    "name": "Python",
    "version": 3
}
```
### Are dictionary keys unique?
Yes. A dictionary cannot contain duplicate keys.
### Common dictionary methods
```python
user.keys()
user.values()
user.items()
user.get("name")
user.update({"version": 3})
user.pop("version")
```
### `get()` vs direct key access?
Direct access:
```python
user["name"]
```
raises `KeyError` if the key does not exist.
`get()` can safely return `None` or a default value:
```python
user.get("name")
user.get("missing", "default")
```
### How do you iterate over a dictionary?
```python
for key, value in user.items():
    print(key, value)
```
### What is a dictionary comprehension?
```python
squares = {x: x * x for x in range(5)}
```
## 7. Loops
### What is a `for` loop?
It iterates over an iterable.
```python
for item in items:
    print(item)
```
### What is a `while` loop?
It repeats while a condition is true.
```python
while condition:
    process()
```
### `break` vs `continue`?
`break` exits the loop. `continue` skips the current iteration.
```python
for number in numbers:
    if number == 5:
        break
```
### What does `range()` do?
It generates a sequence of numbers commonly used with loops.
```python
for i in range(5):
    print(i)
```
### What is `enumerate()`?
It provides both index and value while iterating.
```python
for index, value in enumerate(items):
    print(index, value)
```
### What is `zip()`?
It combines elements from multiple iterables.
```python
names = ["A", "B"]
ages = [20, 30]
for name, age in zip(names, ages):
    print(name, age)
```
## 8. Functions
### What is a function?
A reusable block of code defined using `def`.
```python
def add(a, b):
    return a + b
```
### Parameters vs arguments?
Parameters are variables defined by the function. Arguments are values passed to the function.
```python
def add(a, b):
    return a + b
add(10, 20)
```
### What does `return` do?
It sends a value back to the caller.
```python
def square(x):
    return x * x
```
### What happens if a function has no `return`?
It returns `None`.
### Can Python functions return multiple values?
Yes. Python returns them as a tuple.
```python
def get_values():
    return 10, 20
a, b = get_values()
```
### What is a lambda function?
A small anonymous function.
```python
square = lambda x: x * x
```
### What are docstrings?
Docstrings document functions, classes or modules.
```python
def add(a, b):
    "Return the sum of two numbers."
    return a + b
```
## 9. Arguments
### What are default arguments?
Arguments with predefined values.
```python
def greet(name="User"):
    return f"Hello {name}"
```
### What are keyword arguments?
Arguments passed using parameter names.
```python
greet(name="Python")
```
### What is `*args`?
It collects variable-length positional arguments.
```python
def add(*args):
    return sum(args)
```
### What is `**kwargs`?
It collects variable-length keyword arguments.
```python
def show(**kwargs):
    print(kwargs)
```
### Can `*args` and `**kwargs` be used together?
Yes.
```python
def function(*args, **kwargs):
    print(args)
    print(kwargs)
```
### What is argument unpacking?
```python
numbers = [10, 20]
add(*numbers)
```
Dictionary unpacking:
```python
data = {"name": "Python"}
function(**data)
```
## 10. Scope
### What is variable scope?
Scope determines where a variable can be accessed.
### What is LEGB?
Python searches for names in this order:
```text
L → Local
E → Enclosing
G → Global
B → Built-in
```
### What is a local variable?
```python
def test():
    value = 10
    print(value)
```
`value` exists inside the function's local scope.
### What does `global` do?
It allows a function to modify a global variable.
```python
value = 10
def update():
    global value
    value = 20
```
### What does `nonlocal` do?
It allows a nested function to modify a variable in an enclosing function scope.
```python
def outer():
    value = 10
    def inner():
        nonlocal value
        value = 20
    inner()
```
## 11. Modules & Packages
### What is a module?
A Python file containing reusable code.
```python
import os
```
### What is a package?
A package organizes related modules into a directory structure.
### Different import styles
```python
import os
from pathlib import Path
import json as json_module
```
### Why use modules?
Modules improve:
```text
reusability
organization
maintainability
separation of concerns
```
### What is `__name__ == "__main__"`?
It allows code to run when a file is executed directly but not when it is imported.
```python
def main():
    print("Application started")
if __name__ == "__main__":
    main()
```
### What is a virtual environment?
An isolated environment for project dependencies.
```bash
python -m venv .venv
```
## 12. File Handling
### How do you open a file?
```python
file = open("data.txt", "r")
```
### Common file modes
```text
r   read
w   write
a   append
x   create
b   binary
```
### How do you read a file?
```python
with open("data.txt", "r") as file:
    content = file.read()
```
### Why use `with`?
The context manager automatically handles resource cleanup.
```python
with open("data.txt") as file:
    content = file.read()
```
### Read line by line
```python
with open("data.txt") as file:
    for line in file:
        print(line.strip())
```
### Write to a file
```python
with open("output.txt", "w") as file:
    file.write("Hello Python")
```
### Append to a file
```python
with open("output.txt", "a") as file:
    file.write("New line")
```
### What is `pathlib`?
`pathlib` provides an object-oriented way to work with filesystem paths.
```python
from pathlib import Path
path = Path("data.txt")
if path.exists():
    print(path.read_text())
```
## Quick Core Interview Checklist
```text
Variables
↓
Data Types
↓
Mutable vs Immutable
↓
Strings
↓
Lists
↓
Tuples
↓
Sets
↓
Dictionaries
↓
Loops
↓
Functions
↓
Arguments
↓
Scope / LEGB
↓
Modules & Packages
↓
File Handling
```
## Interview Answer Pattern
For most Python questions, structure your answer as:
```text
1. Definition
2. How it works
3. Simple example
4. When to use it
5. Common pitfall
```
