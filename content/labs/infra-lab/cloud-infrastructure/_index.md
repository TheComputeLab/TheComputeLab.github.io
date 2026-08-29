---
title: "Cloud Infrastructure"
description: "Cloud compute, storage, networking, availability, scalability, hybrid infrastructure and platform services."
weight: 40
---

Cloud infrastructure provides compute, storage, networking and platform capabilities through a service-oriented model.

## Core Architecture

```text
Users / Applications
        ↓
   Cloud Services
        ↓
Compute | Storage | Network
        ↓
Underlying Infrastructure
```

## Compute

Cloud platforms can provide virtual machines, containers, serverless workloads, specialized compute and GPU resources.

## Storage

Common cloud storage models include object, block and file storage. Each is suited to different access patterns.

## Networking

Cloud networks commonly include virtual networks, subnets, routing, security controls, load balancing and private connectivity.

```text
Virtual Network
├── Public Subnet
├── Private Subnet
├── Application Tier
└── Data Tier
```

## Availability

Availability can be improved through multiple instances, independent failure domains, load balancing, automated recovery and data replication.

## Scalability

Vertical scaling increases resources assigned to an instance. Horizontal scaling adds additional instances.

```text
             ┌── Server
Application ─┼── Server
             └── Server
```

## Hybrid Infrastructure

Many organizations operate on-premises and cloud environments together.

```text
On-Premises
    ↕
Connectivity
    ↕
Cloud
```

Hybrid environments require attention to networking, identity, security, data movement and monitoring.

## Infrastructure as Code

```text
Code
 ↓
Plan
 ↓
Provision
 ↓
Validate
 ↓
Operate
```

Infrastructure as Code improves consistency and repeatability.

## Cloud Security

Important areas include identity and access management, network segmentation, encryption, secrets management, logging and least privilege.

## Cost Awareness

Consider capacity, utilization, storage growth, data transfer, licensing and idle resources.

## Key Takeaways

- Cloud infrastructure provides compute, storage and networking as services.
- Availability depends on architecture.
- Scaling can be vertical or horizontal.
- Hybrid infrastructure connects on-premises and cloud platforms.
- Infrastructure as Code improves repeatability.
- Security and cost management remain infrastructure responsibilities.

> **Infrastructure principle:** Moving to cloud changes the operating model; it does not eliminate infrastructure engineering.

