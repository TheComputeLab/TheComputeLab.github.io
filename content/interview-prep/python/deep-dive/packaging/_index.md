---
title: ""
description: "Python packaging interview preparation covering virtual environments, dependencies, pyproject.toml and distribution."
weight: 80
toc: true
---
# 📦 Python Packaging
## Why is packaging important?
Packaging provides a controlled way to manage dependencies, project metadata, builds and distribution.
## Virtual Environments
A virtual environment isolates project dependencies from the system Python installation.
```bash
python -m venv .venv
```
Activate it according to the operating system and shell being used.
## Dependencies
A project should define its dependencies rather than relying on whatever happens to be installed globally.
Common approaches include:
```text
requirements.txt
pyproject.toml
Dependency management tools
```
## `pyproject.toml`
Modern Python projects commonly use `pyproject.toml` for project metadata and build configuration.
A simplified example:
```toml
[project]
name = "example-app"
version = "1.0.0"
dependencies = [
    "requests"
]
```
## Why Pin Dependencies?
Uncontrolled dependency updates can introduce unexpected behavior or security and compatibility problems.
Depending on the project, teams may use pinned or constrained dependency versions and lock files.
## Build and Distribution
Python packages can be built into distribution artifacts and installed using standard Python packaging tooling.
## Interview Question
### How do you make Python deployments reproducible?
Use:
```text
Defined dependencies
Controlled Python version
Virtual environments or containers
Repeatable build process
Version-controlled configuration
Automated tests
CI/CD
```
## Senior-Level Point
Packaging is part of software delivery, not simply a way to install a Python module.
