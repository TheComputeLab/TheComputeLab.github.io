---
title: " ⚡ Python Rapid Revision"
description: "A fast Python interview revision guide covering syntax, data types, operators, common methods, exceptions and frequently asked questions."
weight: 20
toc: true
---

A compact last-minute revision guide for common Python interview topics.

## 1. Syntax Cheat Sheet
### Variables
```python
name = "Python"
age = 30
active = True
```
Python variables do not require explicit type declarations.

### Conditions
```python
if age >= 18:
    print("Adult")
elif age > 0:
    print("Minor")
else:
    print("Invalid age")
```

### Loops
```python
for item in items:
    print(item)

while condition:
    do_something()
```

### Functions
```python
def add(a, b):
    return a + b
```

### List Comprehension
```python
squares = [x * x for x in range(10)]
```

### Dictionary Comprehension
```python
squares = {x: x * x for x in range(10)}
```

### Lambda
```python
square = lambda x: x * x
```

### Import
```python
import os
from pathlib import Path
```

### Exception Handling
```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
finally:
    print("Finished")
```

## 2. Python Data Types
| Type | Example | Mutable? |
|---|---|---|
| `int` | `10` | No |
| `float` | `10.5` | No |
| `bool` | `True` | No |
| `str` | `"Python"` | No |
| `list` | `[1, 2, 3]` | Yes |
| `tuple` | `(1, 2, 3)` | No |
| `set` | `{1, 2, 3}` | Yes |
| `dict` | `{"name": "Python"}` | Yes |
| `NoneType` | `None` | No |

### Important Interview Point
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

## 3. Operators
### Arithmetic
```python
+    # addition
-    # subtraction
*    # multiplication
/    # division
//   # floor division
%    # modulus
**   # exponent
```
Example:
```python
10 // 3    # 3
10 % 3     # 1
2 ** 3     # 8
```

### Comparison
```python
==
!=
>
<
>=
<=
```

### Logical
```python
and
or
not
```

### Identity
```python
is
is not
```

### Membership
```python
in
not in
```

## 4. Common String Methods
```python
text = " Python Interview "
text.lower()
text.upper()
text.strip()
text.replace("Python", "Java")
text.split()
text.startswith(" Python")
text.endswith(" ")
```

Other useful methods:
```python
"python".capitalize()
"python".title()
"python".find("t")
"python".count("o")
```

### Joining
```python
words = ["Python", "Interview", "Prep"]
result = " ".join(words)
```

## 5. Common List Methods
```python
numbers = [3, 1, 2]
numbers.append(4)
numbers.extend([5, 6])
numbers.insert(0, 10)
numbers.remove(3)
numbers.pop()
numbers.sort()
numbers.reverse()
```

Useful operations:
```python
len(numbers)
numbers.count(2)
numbers.index(2)
```

## 6. Common Dictionary Methods
```python
user = {
    "name": "Python",
    "version": 3
}
user.keys()
user.values()
user.items()
user.get("name")
user.update({"version": 3})
user.pop("version")
```

For safe access when a key may not exist:
```python
value = user.get("name")
```

## 7. Common Set Methods
```python
a = {1, 2, 3}
b = {3, 4, 5}
a.add(4)
a.remove(2)
a.discard(10)
```

Set operations:
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

## 8. Common Tuple Operations
```python
point = (10, 20, 10)
len(point)
point.count(10)
point.index(20)
```
Tuples are immutable.

## 9. Exceptions
### Common Exceptions
```text
ValueError
TypeError
NameError
IndexError
KeyError
AttributeError
FileNotFoundError
ZeroDivisionError
ImportError
ModuleNotFoundError
```

### Basic Pattern
```python
try:
    value = int(user_input)
except ValueError:
    print("Invalid number")
```

### Multiple Exceptions
```python
try:
    process()
except ValueError:
    handle_value_error()
except TypeError:
    handle_type_error()
```

### Finally
```python
try:
    file = open("data.txt")
except FileNotFoundError:
    print("File not found")
finally:
    print("Cleanup")
```

### Raise
```python
if age < 0:
    raise ValueError("Age cannot be negative")
```

## 10. Frequently Asked Questions
### What is the difference between `==` and `is`?
`==` compares values. `is` checks object identity.
```python
a = [1, 2]
b = [1, 2]
a == b    # True
a is b    # False
```

### List vs Tuple?
A list is mutable. A tuple is immutable.

### List vs Set?
A list maintains ordered elements and can contain duplicates. A set stores unique elements and is useful for membership testing and set operations.

### Dictionary vs Set?
A dictionary stores key-value pairs. A set stores unique values.

### What is `None`?
`None` represents the absence of a value.
```python
result = None
```

### What is a Python function?
A reusable block of code defined using `def`.
```python
def greet(name):
    return f"Hello {name}"
```

### What is a module?
A Python file containing reusable code such as functions, classes and variables.

### What is a package?
A package organizes related Python modules into a directory structure.

### What is a virtual environment?
An isolated Python environment used to keep project dependencies separate.
```bash
python -m venv .venv
```

### What is PEP 8?
PEP 8 is the Python style guide covering recommended formatting and coding conventions.

### What is indentation used for?
Python uses indentation to define code blocks.
```python
if active:
    print("Running")
```

### What is a list comprehension?
A compact way to create a list from an iterable.
```python
squares = [x * x for x in range(5)]
```

### What is `*args`?
It allows a function to accept a variable number of positional arguments.
```python
def add(*args):
    return sum(args)
```

### What is `**kwargs`?
It allows a function to accept a variable number of keyword arguments.
```python
def show(**kwargs):
    print(kwargs)
```

### `append()` vs `extend()`
`append()` adds one object as an element.
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

### What is slicing?
Slicing extracts part of a sequence.
```python
items[start:stop:step]
```
Example:
```python
numbers = [0, 1, 2, 3, 4]
numbers[1:4]
# [1, 2, 3]
numbers[::-1]
# reversed sequence
```

## 11. Interview Memory Map
```text
PYTHON
│
├── Syntax
│   ├── Variables
│   ├── Conditions
│   ├── Loops
│   └── Functions
│
├── Data Types
│   ├── String
│   ├── List
│   ├── Tuple
│   ├── Set
│   └── Dictionary
│
├── Operators
│   ├── Arithmetic
│   ├── Comparison
│   ├── Logical
│   ├── Identity
│   └── Membership
│
├── Methods
│   ├── String
│   ├── List
│   ├── Dictionary
│   └── Set
│
└── Exceptions
    ├── try
    ├── except
    ├── finally
    └── raise
```

## Quick Interview Rule
When answering a Python interview question, try to cover:
```text
WHAT IS IT?
    ↓
HOW DOES IT WORK?
    ↓
WHEN WOULD YOU USE IT?
    ↓
WHAT ARE THE COMMON PITFALLS?
    ↓
SHOW A SIMPLE EXAMPLE
```
