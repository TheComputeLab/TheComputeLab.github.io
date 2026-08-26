---
title: ""
description: "Interview-focused Python automation preparation covering filesystem operations, subprocesses, JSON, CSV, APIs, logging, regular expressions, configuration and practical automation scripts."
weight: 70
toc: true
---
# 🔧 Python for Automation
Python is widely used for infrastructure, operations, DevOps and engineering automation. This guide focuses on practical automation concepts that commonly appear in technical interviews.
## 1. OS / pathlib
### How do you work with files and directories in Python?
For modern Python code, `pathlib` is usually preferred for filesystem paths.
```python
from pathlib import Path

path = Path("logs")

if path.exists():
    print("Directory exists")
```
### Create a directory
```python
from pathlib import Path

Path("output").mkdir(parents=True, exist_ok=True)
```
### Find files
```python
from pathlib import Path

for file in Path("logs").glob("*.log"):
    print(file)
```
### Read and write a file
```python
from pathlib import Path

path = Path("config.txt")
path.write_text("Python automation")
print(path.read_text())
```
### `os` vs `pathlib`
```text
os       → traditional operating-system interface
pathlib  → object-oriented filesystem path handling
```
## 2. subprocess
### Why use `subprocess`?
`subprocess` allows Python programs to execute external commands and interact with operating-system processes.
```python
import subprocess

result = subprocess.run(
    ["python", "--version"],
    capture_output=True,
    text=True
)

print(result.stdout)
```
### Check command success
```python
import subprocess

result = subprocess.run(
    ["python", "--version"],
    capture_output=True,
    text=True,
    check=True
)
```
With `check=True`, a non-zero exit status raises `CalledProcessError`.
### Capture standard output and errors
```python
import subprocess

result = subprocess.run(
    ["python", "--version"],
    capture_output=True,
    text=True
)

print(result.stdout)
print(result.stderr)
```
### Interview question
#### Why prefer `subprocess` over `os.system()`?
`subprocess` provides more control over arguments, output, errors, return codes and process execution.
## 3. JSON
### How do you read JSON?
```python
import json

with open("config.json") as file:
    data = json.load(file)

print(data)
```
### Convert JSON text to Python objects
```python
import json

data = json.loads('{"name": "Python", "version": 3}')
print(data["name"])
```
### Write JSON
```python
import json

data = {
    "name": "Python",
    "version": 3
}

with open("config.json", "w") as file:
    json.dump(data, file, indent=2)
```
### `load()` vs `loads()`
```text
load()   → reads JSON from a file-like object
loads()  → reads JSON from a string
dump()   → writes JSON to a file-like object
dumps()  → converts Python data to a JSON string
```
## 4. CSV
### How do you read a CSV file?
```python
import csv

with open("servers.csv", newline="") as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(row["hostname"])
```
### Write CSV
```python
import csv

rows = [
    {"name": "server01", "status": "OK"},
    {"name": "server02", "status": "FAILED"}
]

with open("report.csv", "w", newline="") as file:
    writer = csv.DictWriter(file, fieldnames=["name", "status"])
    writer.writeheader()
    writer.writerows(rows)
```
### Interview tip
Use the `csv` module for straightforward CSV processing. For large data-analysis workflows, tools such as pandas may be more appropriate depending on project requirements.
## 5. APIs
### How can Python interact with an API?
A Python program can send HTTP requests to an API and process the response.
A common approach is using the `requests` library.
```python
import requests

response = requests.get(
    "https://api.example.com/servers",
    timeout=10
)

response.raise_for_status()
data = response.json()
print(data)
```
### Why use a timeout?
A timeout prevents the program from waiting indefinitely for a response.
### How do you handle API failures?
```python
import requests

try:
    response = requests.get(
        "https://api.example.com/servers",
        timeout=10
    )
    response.raise_for_status()
except requests.RequestException as error:
    print(f"API request failed: {error}")
```
### Common API interview topics
```text
HTTP methods
Status codes
Authentication
Headers
Timeouts
Retries
Pagination
Rate limits
JSON responses
Error handling
```
## 6. Logging
### Why use logging instead of `print()`?
Logging provides levels, timestamps, formatting and configurable output destinations.
```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s"
)

logging.info("Automation started")
logging.warning("Disk space is low")
logging.error("Backup failed")
```
### Common logging levels
```text
DEBUG
INFO
WARNING
ERROR
CRITICAL
```
### Interview question
#### Why is logging important in automation?
Automation often runs without direct user interaction. Logs provide the evidence needed to understand what happened, diagnose failures and audit execution.
## 7. Regex
### What is regular expression matching?
Regular expressions provide patterns for searching, validating and extracting text.
```python
import re

text = "Server IP: 192.168.1.10"

match = re.search(r"\d+\.\d+\.\d+\.\d+", text)

if match:
    print(match.group())
```
### Common regex functions
```python
re.search()
re.match()
re.findall()
re.sub()
```
### Find all matches
```python
import re

text = "error 101, error 205, error 301"
errors = re.findall(r"error \d+", text)
print(errors)
```
### Replace text
```python
import re

text = "server-01 server-02"
result = re.sub(r"server-\d+", "server", text)
print(result)
```
### Interview tip
Regex is powerful for structured text extraction, but simple string methods are often clearer when the pattern is straightforward.
## 8. Configuration
### Why separate configuration from code?
Separating configuration makes automation easier to reuse across environments without changing the program logic.
Typical configuration values include:
```text
Server names
Paths
Ports
API endpoints
Feature flags
Timeouts
Environment-specific settings
```
### Environment variables
```python
import os

api_url = os.getenv("API_URL")
environment = os.getenv("ENVIRONMENT", "development")
```
### JSON configuration
```python
import json

with open("config.json") as file:
    config = json.load(file)

timeout = config.get("timeout", 30)
```
### Interview best practice
Do not hard-code secrets such as passwords or API tokens in source code. Use appropriate secret-management mechanisms or environment-based configuration.
## 9. Automation Scripts
### What makes a good automation script?
A production-oriented automation script should generally have:
```text
Clear inputs
Validation
Logging
Error handling
Useful exit codes
Configuration
Reusable functions
Safe execution
```
### Example: Check log files for errors
```python
from pathlib import Path
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s"
)

log_directory = Path("logs")

for file in log_directory.glob("*.log"):
    logging.info("Checking %s", file)
    text = file.read_text(errors="ignore")
    if "ERROR" in text:
        logging.warning("Errors found in %s", file)
```
### Example: Run a system command and log the result
```python
import logging
import subprocess

logging.basicConfig(level=logging.INFO)

result = subprocess.run(
    ["python", "--version"],
    capture_output=True,
    text=True
)

if result.returncode == 0:
    logging.info("Command succeeded: %s", result.stdout.strip())
else:
    logging.error("Command failed: %s", result.stderr.strip())
```
### Example: Simple automation structure
```python
import logging

def validate():
    return True

def collect_data():
    return {"status": "OK"}

def process(data):
    logging.info("Processing: %s", data)

def main():
    if not validate():
        logging.error("Validation failed")
        return 1

    data = collect_data()
    process(data)
    return 0

if __name__ == "__main__":
    raise SystemExit(main())
```
## Automation Interview Scenarios
### How would you automate a repetitive operational task?
A strong answer should cover:
```text
1. Understand the manual process
2. Identify inputs and outputs
3. Define failure conditions
4. Build the automation
5. Add logging and error handling
6. Test safely
7. Add configuration
8. Schedule or integrate the script
9. Monitor results
```
### How would you make an automation script production-ready?
Mention:
```text
Input validation
Exception handling
Logging
Configuration
Secrets management
Idempotency
Timeouts
Retries where appropriate
Exit codes
Testing
Monitoring
```
### What is idempotency?
An operation is idempotent when running it multiple times produces the same intended final state as running it once.
This is especially important in infrastructure automation because scripts may be retried after partial failures.
## Quick Automation Interview Checklist
```text
OS / pathlib
↓
subprocess
↓
JSON
↓
CSV
↓
APIs
↓
Logging
↓
Regex
↓
Configuration
↓
Automation Scripts
```
## Interview Answer Pattern
For an automation question, explain:
```text
1. What needs to be automated?
2. What inputs are required?
3. Which Python libraries would you use?
4. How will errors be handled?
5. How will execution be logged?
6. How will configuration and secrets be managed?
7. How will the automation be tested?
8. How will success or failure be monitored?
```
