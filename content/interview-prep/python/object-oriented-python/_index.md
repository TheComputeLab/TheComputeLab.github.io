---
title: " 🧠 Object-Oriented Python"
description: "Interview-focused Python OOP preparation covering classes, objects, inheritance, polymorphism, encapsulation, abstraction and class or static methods."
weight: 40
toc: true
---

A practical interview guide to the core object-oriented programming concepts used in Python.
## 1. Classes & Objects
### What is a class?
A class is a blueprint for creating objects. It defines attributes and behavior that objects created from it can use.
```python
class Engineer:
    def work(self):
        print("Engineering work")
```
### What is an object?
An object is an instance of a class.
```python
engineer = Engineer()
engineer.work()
```
### Class vs Object
```text
Class  → Blueprint
Object → Instance created from the blueprint
```
## 2. `__init__`
### What is `__init__`?
`__init__` is an initializer method that runs when an object is created. It is commonly used to initialize instance attributes.
```python
class Engineer:
    def __init__(self, name):
        self.name = name

engineer = Engineer("Python")
print(engineer.name)
```
### Is `__init__` a constructor?
In common Python terminology it is often called the constructor, but technically object creation and initialization are separate steps. `__new__` creates the instance and `__init__` initializes it.
### What is `self`?
`self` refers to the current instance of the class.
```python
class Engineer:
    def __init__(self, name):
        self.name = name
```
## 3. Instance vs Class Variables
### What is an instance variable?
An instance variable belongs to a particular object.
```python
class Engineer:
    def __init__(self, name):
        self.name = name
```
Each object can have a different `name`.
### What is a class variable?
A class variable is shared by instances of the class unless an instance overrides it.
```python
class Engineer:
    company = "Compute Lab"
```
### Example
```python
class Engineer:
    company = "Compute Lab"

    def __init__(self, name):
        self.name = name

a = Engineer("A")
b = Engineer("B")

print(a.company)
print(b.company)
```
### Interview comparison
```text
Instance variable → belongs to an object
Class variable     → belongs to the class and is shared
```
## 4. Inheritance
### What is inheritance?
Inheritance allows a child class to reuse or extend behavior from a parent class.
```python
class Engineer:
    def work(self):
        print("Engineering")

class PythonEngineer(Engineer):
    def code(self):
        print("Writing Python")
```
```python
engineer = PythonEngineer()
engineer.work()
engineer.code()
```
### Why use inheritance?
Common reasons include:
```text
Code reuse
Extension of behavior
Common interfaces
Modeling related classes
```
### What is method overriding?
A child class can provide its own implementation of a method inherited from the parent.
```python
class Engineer:
    def work(self):
        print("Engineering")

class PythonEngineer(Engineer):
    def work(self):
        print("Python engineering")
```
### What is `super()`?
`super()` is commonly used to access behavior from a parent class.
```python
class Engineer:
    def __init__(self, name):
        self.name = name

class PythonEngineer(Engineer):
    def __init__(self, name, version):
        super().__init__(name)
        self.version = version
```
## 5. Polymorphism
### What is polymorphism?
Polymorphism means the same interface or operation can work with objects of different types.
```python
class Veeam:
    def run(self):
        print("Backup")

class Python:
    def run(self):
        print("Automation")

for system in [Veeam(), Python()]:
    system.run()
```
The caller uses the same `run()` interface while each object provides its own behavior.
### Duck typing
Python commonly uses duck typing: if an object supports the required operation, its specific class may not matter.
```python
def execute(worker):
    worker.run()
```
Any object with a compatible `run()` method can be passed to `execute()`.
## 6. Encapsulation
### What is encapsulation?
Encapsulation means keeping data and behavior together and controlling how internal state is accessed or modified.
Python does not enforce private fields in the same way as some languages, but naming conventions and mechanisms such as name mangling are available.
### Single underscore
A leading underscore conventionally indicates internal use.
```python
class Engineer:
    def __init__(self):
        self._status = "active"
```
### Double underscore
A double leading underscore triggers name mangling.
```python
class Engineer:
    def __init__(self):
        self.__secret = "internal"
```
The attribute is name-mangled to reduce accidental access from outside the class.
### Property example
```python
class Engineer:
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name
```
## 7. Abstraction
### What is abstraction?
Abstraction exposes the required interface while hiding implementation details.
Python can implement formal abstraction using the `abc` module.
```python
from abc import ABC, abstractmethod

class BackupSystem(ABC):
    @abstractmethod
    def backup(self):
        pass
```
A concrete class implements the required behavior:
```python
class VeeamBackup(BackupSystem):
    def backup(self):
        print("Running backup")
```
### Why use abstraction?
```text
Define common interfaces
Hide implementation details
Enforce required methods
Improve maintainability
```
## 8. `@staticmethod`
### What is a static method?
A static method belongs to the class namespace but does not receive the instance or class automatically.
```python
class Utility:
    @staticmethod
    def add(a, b):
        return a + b

print(Utility.add(10, 20))
```
### When would you use it?
Use a static method when the operation logically belongs to the class but does not need instance or class state.
## 9. `@classmethod`
### What is a class method?
A class method receives the class as its first argument, conventionally named `cls`.
```python
class Engineer:
    company = "Compute Lab"

    @classmethod
    def get_company(cls):
        return cls.company
```
```python
print(Engineer.get_company())
```
### Common use case: alternative constructors
```python
class Engineer:
    def __init__(self, name):
        self.name = name

    @classmethod
    def from_default(cls):
        return cls("Unknown")
```
### Static method vs class method
```text
staticmethod → receives neither self nor cls automatically
classmethod  → receives cls automatically
instance method → receives self automatically
```
## Quick OOP Interview Comparison
| Concept | Key idea |
|---|---|
| Class | Blueprint for objects |
| Object | Instance of a class |
| `__init__` | Initializes an instance |
| Instance variable | Object-specific state |
| Class variable | Shared class-level state |
| Inheritance | Reuse or extend parent behavior |
| Polymorphism | Common interface, different behavior |
| Encapsulation | Control access to internal state |
| Abstraction | Expose interface, hide implementation |
| `@staticmethod` | Class-related operation without automatic `self` or `cls` |
| `@classmethod` | Method receiving `cls` |
## Common Interview Questions
### Inheritance vs composition?
Inheritance models an "is-a" relationship.
Composition models a "has-a" relationship and often provides more flexibility by combining objects rather than extending a class hierarchy.
### Can Python support multiple inheritance?
Yes.
```python
class A:
    pass

class B:
    pass

class C(A, B):
    pass
```
Python uses method resolution order (MRO) to determine how methods are searched.
### What is MRO?
MRO is the order in which Python searches classes when resolving attributes and methods.
```python
class A:
    pass

class B(A):
    pass

print(B.mro())
```
### What is method overriding?
A child class defines a method with the same name as a parent method and provides its own behavior.
### Does Python have method overloading?
Python does not support traditional compile-time method overloading by defining multiple methods with the same name and different signatures. Common alternatives include default arguments, `*args`, `**kwargs` and dispatch techniques.
## OOP Interview Answer Pattern
For an OOP question, answer in this order:
```text
1. Definition
2. Why it is used
3. Simple Python example
4. Real-world use case
5. Common interview comparison
```
