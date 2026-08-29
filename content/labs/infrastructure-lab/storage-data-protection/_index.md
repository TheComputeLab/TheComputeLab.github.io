---
title: "Storage & Data Protection"
description: "Storage architecture, SAN, NAS, volumes, protocols, snapshots, replication, backup and disaster recovery."
weight: 30
---

Storage provides persistent capacity to applications, virtual machines and users. A strong storage platform must provide capacity, performance, availability and recoverability.

## Storage Path

```text
Application
    ↓
Operating System
    ↓
Volume / File System
    ↓
Storage Protocol
    ↓
Storage System
    ↓
Physical Media
```

## SAN

Storage Area Networks provide block-level storage to hosts. Common technologies include Fibre Channel and iSCSI.

## NAS

Network Attached Storage provides file-level access through protocols such as NFS and SMB.

## Volumes

A volume is a logical storage resource presented to a host or application. Consider capacity, performance, availability, permissions and protection requirements.

## Storage Performance

| Metric | Meaning |
|---|---|
| IOPS | Input/output operations per second |
| Throughput | Amount of data transferred over time |
| Latency | Time required to complete an I/O |

## Snapshots

Snapshots provide point-in-time representations of data and can support recovery, testing and application protection.

Snapshots should not automatically be treated as a replacement for independent backups.

## Replication

```text
Primary Storage
      ↓
   Replication
      ↓
Secondary Storage
```

Replication can support disaster recovery and business continuity.

## Backup and Recovery

A backup strategy should define frequency, retention, location, recovery objectives and restore testing.

### RPO

Recovery Point Objective defines how much data loss is acceptable.

### RTO

Recovery Time Objective defines how quickly a service should be restored.

## Storage Troubleshooting

Check host connectivity, protocols, paths, volume status, capacity, latency, throughput, IOPS and replication status.

## Key Takeaways

- SAN provides block-level storage.
- NAS provides file-level storage.
- IOPS, throughput and latency describe different performance characteristics.
- Snapshots provide point-in-time protection.
- Replication supports disaster recovery.
- Backups provide recoverable copies.
- RPO and RTO define protection requirements.

> **Infrastructure principle:** Data protection is effective only when recovery has been designed and tested.
