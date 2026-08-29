---
title: "Networking"
description: "Network architecture, switching, routing, IP addressing, DNS, connectivity, security and resilient communication."
weight: 20
---

Networking connects infrastructure components and allows users, applications, servers, storage systems and cloud platforms to communicate.

## Basic Network Model

```text
Client
  ↓
Switch
  ↓
Router / Gateway
  ↓
Firewall
  ↓
Server / Application
```

## IP Addressing

IP addresses identify devices and interfaces. Important concepts include IPv4, IPv6, subnetting, default gateways, private addresses and public addresses.

## Switching

Switches connect devices within a local network. VLANs can logically separate traffic.

```text
Server A ──┐
Server B ──┼── Switch ── Uplink
Server C ──┘
```

## Routing

Routers forward traffic between different networks.

```text
Network A
    ↓
 Router
    ↓
Network B
```

Routing concepts include routing tables, static routes, dynamic routing, default routes and next-hop gateways.

## DNS

DNS translates names into network addresses.

```text
application.example
        ↓
       DNS
        ↓
    IP Address
```

DNS problems can appear as application or connectivity failures.

## Latency and Bandwidth

Latency is the time required for traffic to travel between endpoints. Bandwidth is the available transfer capacity.

> Bandwidth and latency are different performance characteristics.

## Network Segmentation

Common logical zones include management, server, storage, backup, user and guest networks. Segmentation improves organization and can strengthen security.

## Troubleshooting

1. Check physical connectivity.
2. Verify interface status.
3. Check IP configuration.
4. Test the gateway.
5. Test DNS resolution.
6. Test the destination.
7. Check routing.
8. Check firewall rules.
9. Review logs and monitoring.

Common tools:

```text
ping
tracert / traceroute
ipconfig / ifconfig
nslookup / dig
netstat / ss
arp
```

## Resilient Networking

Redundant links, switches, routers, gateways, link aggregation and dynamic routing can reduce single points of failure.

## Key Takeaways

- Switching connects devices within networks.
- Routing connects different networks.
- DNS maps names to addresses.
- Latency and bandwidth measure different characteristics.
- Segmentation improves organization and security.
- Redundancy improves availability.

> **Infrastructure principle:** Reliable applications require reliable paths between their dependencies.
