---
title: "Testing & Evaluation"
description: "AIVA testing strategy, benchmarks, integration tests, security and performance results."
weight: 50
toc: true
---

Testing is treated as a first-class part of AIVA's engineering workflow.

```text
UNIT
 ↓
INTEGRATION
 ↓
PERFORMANCE
 ↓
SECURITY
 ↓
END-TO-END
 ↓
USER ACCEPTANCE
```

## Unit Testing

The documented test infrastructure uses:

- `unittest.mock`
- Shared fixtures
- `pytest-asyncio`
- `pytest-cov`
- GitHub push integration

Mocks are used for external APIs and hardware dependencies. 

The final documentation package records **247 unit tests** as the planned/submitted test suite. 

## Integration Testing

The documented integration scenarios include:

| ID | Scenario |
|---|---|
| INT-01 | English ASR → LLM → TTS |
| INT-02 | Hindi ASR → LLM → TTS |
| INT-03 | WebSocket audio streaming |
| INT-04 | Multi-turn Redis context |
| INT-05 | Gemini weather function calling |
| INT-06 | Wikipedia integration |
| INT-07 | Authentication lifecycle |
| INT-08 | ASR failure recovery |
| INT-09 | LLM timeout / circuit breaker |
| INT-10 | WebSocket reconnection |
| INT-11 | Language switching |
| INT-12 | Redis session restoration |

The final submission package records **47 integration tests**. 

## ASR Evaluation

The documented ASR benchmark reports:

| Metric | Target | Achieved |
|---|---:|---:|
| English WER | ≤ 12% | 6.8% |
| Hindi WER | ≤ 20% | 14.2% |
| Transcription latency | ≤ 1500 ms | 1043 ms |

The ASR test section records 28 tests, all passing, with 87% coverage. 

## Multilingual Evaluation

The documented multilingual module reports:

| Metric | Target | Achieved |
|---|---:|---:|
| English WER | ≤ 12% | 6.8% |
| Hindi WER | ≤ 20% | 14.2% |
| Detection accuracy | ≥ 90% | 97.3% |
| Code-switch latency | < 200 ms | 143 ms |

It also records 24 multilingual tests and 8/8 code-switch tests passing. 

## Wake-Word Evaluation

| Metric | Result |
|---|---:|
| Detection latency | < 300 ms |
| True-positive rate | 96.4% |
| False-positive rate | < 0.5/hour |
| Standby CPU | < 2% |
| Unit tests | 12/12 |


## End-to-End Latency

The documented observed latency:

```text
ASR       1043 ms
LLM        412 ms
TTS        831 ms
------------------
TOTAL     2798 ms
```

This was recorded as passing the <4-second target. 

## Load Testing

The documented performance test used Locust and k6 with:

- 100 concurrent users
- 30-minute sustained load

The report states that the system remained within SRS targets for 100 users, with degradation above 150 users addressed through horizontal scaling in production. 

## Security Testing

The documented security assessment includes:

| Test | Tool | Result |
|---|---|---|
| OWASP Top 10 | OWASP ZAP | 0 critical / 0 high |
| Python static analysis | Bandit | 0 critical |
| Container scan | Trivy | Critical CVEs patched |
| Dependencies | Snyk | No high severity |
| TLS | testssl.sh | Grade A |
| Secrets | gitleaks | 0 detected |

It also records testing of JWT authentication, rate limiting, CORS and input sanitisation. 

## Reliability Testing

The integration plan explicitly covers:

- ASR failure
- LLM timeout
- Circuit breaker behaviour
- WebSocket reconnection
- Language switching
- Session restoration

This is important for a voice assistant because failures can occur at the hardware, network, API, model or session layer.

## Acceptance Criteria

The documented SRS acceptance criteria include:

- Total time to first response ≤ 4 seconds
- English WER ≤ 12%
- Hindi WER ≤ 20%
- Uptime ≥ 99%
- 100 concurrent users

## Testing Philosophy

AIVA follows:

```text
Test the module
      ↓
Test the integration
      ↓
Test under load
      ↓
Test failure paths
      ↓
Test security
      ↓
Test the complete user journey
```

The project documentation notes that integration testing identified critical issues that unit testing alone did not catch.
