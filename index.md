---
title: "afn-lab's Homelab Infra"
---

# Architecting an Advanced Hybrid Private Cloud for Educational Ecosystem

*Read this in other languages: [English](README.md), [Bahasa Indonesia](README-id.md).*

## 1. Executive Summary
Designed, deployed, and currently managing a highly optimized Private Cloud infrastructure using Proxmox VE. This project serves as the digital backbone for an educational institution, engineered to deliver hybrid capabilities (Online & Local network). The system seamlessly handles **400+ concurrent users** during massive CBT exams while providing advanced network management, content filtering, and an integrated development environment for custom applications.

## 2. System Architecture & Core Services

![Proxmox VE Dashboard](proxmox-dashboard.png)
*Proxmox VE Dashboard: Centralized management for LXC containers, QEMU VMs, and Docker environments.*

The infrastructure is heavily segmented to ensure performance, security, and resource efficiency:

* **Advanced Core Routing (MikroTik CHR):** The network brain configured with enterprise-grade features including **Load Balancing**, **Multi-WAN Failover**, and **VRRP** (Virtual Router Redundancy Protocol) to ensure zero downtime on network gateways.
* **Hybrid CBT Engine (Online & Offline):** An adaptive examination system running on lightweight LXC. It provides worldwide access via `cloudflared` Zero Trust Tunnels, while simultaneously supporting isolated local network exams utilizing a custom **`nginx-ncsibypass`** configuration to maintain stable Windows client connections without internet access.
* **DNS Management & Content Filtering:** Centralized `adguard` deployment acting as a DNS sinkhole to aggressively block ads, malware, and filter inappropriate websites, ensuring a safe digital environment for students.
* **Self-Hosted Digital Workspace:** `nextcloud` deployment serving as a secure, private file server and collaboration suite for school administration.
* **Custom App Development & Containerization:** A dedicated **Docker** environment utilized to develop, test, and host **ANSimpleLibrary**—a proprietary, custom-built library management application, showcasing full-cycle Dev/Ops capabilities.

## 3. Resilience & Maintenance Strategy
To maximize uptime and data integrity on a single-node hypervisor, I implemented a strict resilience framework:
* **Automated Disaster Recovery:** Scheduled, off-site data backups to external storage to guarantee a fast Recovery Time Objective (RTO).
* **Traffic Optimization:** Fine-tuned bandwidth queues and routing rules within MikroTik to prevent bottlenecking during massive concurrent exam logins.
* **Proactive Alerting:** Integrated `uptimekuma` for real-time observability, pushing instant anomaly notifications directly to Telegram.

## 4. Engineering Highlights
* **Full-Stack Competency:** Successfully bridging Infrastructure operations (Proxmox, MikroTik, Networking) with Software deployment (Dockerizing custom applications).
* **Maximum Resource Efficiency:** Extensive use of Linux Containers (LXC) and Docker over traditional VMs, slashing CPU and RAM overhead while maximizing throughput.
