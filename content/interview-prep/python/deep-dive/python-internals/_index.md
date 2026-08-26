---
title: ""
description: "Python internals interview preparation covering bytecode, execution, objects and the interpreter."
weight: 10
toc: true
---
# 🧠 Python Internals
## What happens when Python executes a program?
At a high level:
```text
Python source code
↓
Parsing / compilation
↓
Bytecode
↓
Python virtual machine / interpreter
↓
Execution
```
Python implementation details vary by implementation. In standard CPython discussions, source code is compiled to bytecode that the interpreter executes.
## What is bytecode?
Bytecode is an intermediate instruction representation used by CPython.
You can inspect it with the `dis` module:
```python
import dis

def add(a, b):
    return a + b

dis.dis(add)
```
## What is an object in Python?
Python uses objects to represent values. Objects have identity, type and value.
```python
x = 10
print(id(x))
print(type(x))
```
## What is dynamic typing?
The variable name does not have a permanently fixed type. Objects have types, and names can refer to objects of different types at different times.
```python
value = 10
value = "Python"
```
## Interview Question
### Is Python interpreted or compiled?
A strong answer is that Python source is compiled to an intermediate bytecode representation in CPython and then executed by the Python interpreter/virtual machine. Calling Python simply "interpreted" is an oversimplification.
