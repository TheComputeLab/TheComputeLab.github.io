---
title: "Deployment"
description: "AIVA cloud deployment, containers, reverse proxy, security and CI/CD."
weight: 40
toc: true
---

AIVA's documentation describes a production-oriented AWS architecture and an EC2 deployment model.

## Deployment Philosophy

The deployment design separates:

```text
Public Edge
     ↓
Reverse Proxy
     ↓
Application
     ↓
Internal Services
     ↓
Managed AWS Services
```

## AWS Architecture

The documented EC2 architecture uses:

- AWS EC2 t3.medium
- Ubuntu
- Docker Compose
- Nginx
- Elastic IP
- AWS Secrets Manager
- DynamoDB
- Redis
- CloudWatch
- Cognito
- Gemini API

The deployment documentation describes AWS region `ap-south-1` (Mumbai) and a disaster-recovery design involving `ap-southeast-1`. 

## Traffic Flow

```text
Browser / Mobile
      ↓
DNS
      ↓
Elastic IP
      ↓
Nginx
  HTTPS :443
      ↓
FastAPI
  :8080
      ↓
┌─────┼─────────────┐
↓     ↓             ↓
ASR   TTS         Redis
      ↓
AWS Managed Services
```

Internal application services are not directly internet-exposed. The documented internal ports are FastAPI 8080, ASR 50051, TTS 5002 and Redis 6379. 

## Docker Compose

The documented container stack contains four services:

| Container | Role |
|---|---|
| `aiva-orchestrator` | FastAPI API layer |
| `aiva-asr` | faster-whisper ASR |
| `aiva-tts` | XTTS-v2 TTS |
| `aiva-redis` | Session cache |

All containers are placed on a private Docker bridge network and health checks are enabled. 

## Nginx

Nginx provides:

- HTTPS termination
- HTTP → HTTPS redirect
- Reverse proxy
- HSTS
- Rate limiting

The documented rate limit is 60 requests/minute per user/IP.

## Secrets

The documented security model uses AWS Secrets Manager.

Secrets include:

- Gemini API key
- STT credentials
- Redis authentication
- Cognito client secret
- Deployment credentials

The design explicitly avoids hard-coded credentials and uses IAM-based access. 

## Network Security

The deployment design includes:

- Security groups
- Restricted internal ports
- TLS 1.3
- HTTPS enforcement
- VPC endpoints
- Restricted outbound traffic
- IAM roles
- Rate limiting

The documented hardening checklist also includes GuardDuty, Fail2Ban, OWASP mitigations and CIS AWS controls. 

## Persistence

```text
                 ┌───────────────┐
                 │   AIVA App    │
                 └───────┬───────┘
                         │
              ┌──────────┴──────────┐
              ↓                     ↓
           Redis                DynamoDB
        session state       persistent records
```

Redis handles fast ephemeral state. DynamoDB provides persistent storage for users, sessions, messages and API usage. 

## Observability

CloudWatch is used for:

- Application logs
- Infrastructure metrics
- Alerts

The documented monitoring stack also includes AWS X-Ray and GuardDuty. Key metrics include CPU, memory, API latency, LLM TTFT, errors and cost. 

## CI/CD

The documented workflow is:

```text
GitHub Push
    ↓
Automated Tests
    ↓
Docker Build
    ↓
Container Scan
    ↓
Push Image
    ↓
EC2 Deployment
    ↓
Health Verification
```

GitHub Actions uses OIDC rather than stored long-lived AWS credentials, and deployment is designed around rolling updates with tests as a deployment gate. 

## Deployment Runbook

The documented runbook is approximately:

1. Launch EC2.
2. Install Docker, Nginx and Git.
3. Clone the application.
4. Configure DNS.
5. Obtain SSL certificate.
6. Configure Nginx.
7. Start Docker Compose.
8. Run smoke tests.
9. Verify health.
10. Monitor the deployment.

The documented estimate is approximately 60–70 minutes for the deployment procedure. 

## Production Status in the Documentation

The final deployment report records four containers running, SSL Grade A, health checks passing and 99.8% uptime since deployment. These figures belong to the project's documented deployment report and should be treated as project-recorded results rather than independently verified platform telemetry. 
