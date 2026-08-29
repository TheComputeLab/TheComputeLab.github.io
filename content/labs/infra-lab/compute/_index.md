---
title: "Compute & Virtualization"
description: "Servers, compute resources, hypervisors, virtual machines, clusters and workload placement."
weight: 10
---

Compute provides the processing resources required to run operating systems, applications, databases and services. Virtualization adds an abstraction layer that allows physical resources to be shared efficiently.

## Basic Model

```text
Physical Server
      ↓
CPU + Memory + Network
      ↓
Operating System / Hypervisor
      ↓
Virtual Machines / Workloads
```

## Physical Servers

Important infrastructure characteristics include CPU capacity, memory, storage connectivity, network bandwidth, power, hardware redundancy and lifecycle management.

## Virtualization

Virtualization allows one physical server to host multiple isolated virtual machines.

```text
Physical Host
├── VM 01
├── VM 02
├── VM 03
└── VM 04
```

Each VM can have virtual CPUs, memory, disks, network interfaces and its own operating system.

## Hypervisors

A hypervisor creates and manages virtual machines.

| Type | Description |
|---|---|
| Type 1 | Runs directly on physical hardware |
| Type 2 | Runs on top of a host operating system |

## Resource Allocation

Key concepts include vCPU, memory allocation, CPU scheduling, overcommit, reservations, limits and resource pools.

The goal is to provide sufficient resources without unnecessary over-provisioning.

## Clusters

Multiple physical hosts can form a cluster.

```text
Cluster
├── Host 01
├── Host 02
├── Host 03
└── Host 04
```

Clusters can provide high availability, workload mobility, resource balancing and maintenance flexibility.

## Capacity Planning

Track CPU, memory, storage, network utilization and workload growth to determine whether infrastructure can support current and future demand.

## Performance

A workload may be constrained by CPU, memory, storage latency, storage throughput, network latency or network bandwidth. Always investigate the complete resource path.

## Key Takeaways

- Compute provides processing resources.
- Virtualization abstracts physical hardware into virtual resources.
- Hypervisors manage virtual machines.
- Clusters improve availability and flexibility.
- Capacity planning prevents resource shortages.

> **Infrastructure principle:** Balance performance, utilization, availability and operational simplicity.
