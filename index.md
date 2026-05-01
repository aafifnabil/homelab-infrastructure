---
title: "afn-lab's Homelab Infra"
---

# Architecting an Optimized Private Cloud for School/Campus Ecosystem

*Read this in other languages: [English](README.md), [Bahasa Indonesia](README-id.md).*

## 1. Executive Summary
Designed and currently managing an efficient Private Cloud infrastructure using Proxmox VE as the digital backbone for educational institutions. This system is optimized to handle intensive workloads, successfully serving **400+ concurrent users** on CBT platforms without performance degradation, focusing on service stability and hardware resource efficiency.

## 2. System Architecture & Resource Management

![Proxmox VE Dashboard](proxmox-dashboard.png)
*Proxmox VE Dashboard: Centralized management for all LXC containers and QEMU VMs.*

The infrastructure is designed with a service-isolation approach to maintain stability and maximize bare-metal resources:

* **Core Routing:** MikroTik CHR v7 (VM) acting as the network gateway, handling VLAN distribution and client bandwidth management.
* **Security & Access:** `cloudflared` for secure external tunneling and `adguard` for advanced DNS-level protection.
* **Production Services:** Application clusters for `moodle` & `garuda-cbt` running on LXC to achieve near-zero latency.
* **Digital Workspace:** `nextcloud` & `onlyoffice` as a self-hosted internal document collaboration suite.
* **Observability:** `uptimekuma` for real-time health monitoring of all services.

## 3. Resilience & Maintenance Strategy
Despite operating in a single-node environment, data security and service continuity remain top priorities through:
* **Automated Disaster Recovery:** Implementation of routine backup schedules to external storage to ensure data safety in case of system failure.
* **Traffic Optimization:** Custom `nginx-ncsibypass` engineering to maintain stable client connectivity within the local network.
* **Proactive Alerting:** Integrated monitoring with Telegram notifications for rapid response to service anomalies.

## 4. Engineering Highlights
* **Maximum Resource Efficiency:** Extensive use of LXC (Linux Containers) to run heavy applications, resulting in significantly lower CPU and RAM overhead compared to conventional VMs.
* **Network Precision:** Implementation of "Router-on-a-Stick" within the hypervisor for clean and secure traffic segmentation.
