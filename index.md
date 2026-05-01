---
title: "afn-lab's Homelab Infra"
---

# Architecting an Advanced Private Cloud & Network Infrastructure for Campus Ecosystem

*Read this in other languages: [English](README.md), [Bahasa Indonesia](README-id.md).*

## 1. Executive Summary
Designed, deployed, and currently managing a comprehensive Private Cloud and Advanced Networking infrastructure using Proxmox VE. Built to serve educational institutions, this system seamlessly handles **400+ concurrent users** on CBT platforms. The architecture blends robust network engineering, hybrid access environments, and a dedicated DevOps sandbox to deliver high performance, security, and zero-downtime capabilities.

## 2. Core Architecture & Services

![Proxmox VE Dashboard](proxmox-dashboard.png)
*Proxmox VE Dashboard: Centralized management for LXC containers, QEMU VMs, and Docker environments.*

The infrastructure is segregated into distinct operational layers to ensure absolute security and resource efficiency:

* **Advanced Edge Routing & Gateway (MikroTik CHR v7):**
  * Engineered with **Load Balancing** and **Dual-WAN Failover** to guarantee uninterrupted connectivity.
  * Implemented **VRRP (Virtual Router Redundancy Protocol)** for gateway redundancy and high availability at the network layer.
* **Enterprise DNS & Security Filtering:**
  * Deployed `adguard` as a DNS sinkhole for network-wide ad-blocking, malicious website filtering, and automated safe-search enforcement for students.
* **Hybrid E-Learning & CBT Ecosystem (`moodle` & `garuda-cbt`):**
  * **Online Mode:** Secure remote access routed through Zero Trust Tunnels (`cloudflared`).
  * **Offline/Strict Exam Mode:** Local network lockdown to prevent cheating. Utilizing a custom **`nginx-ncsibypass`** to simulate internet connectivity for Windows clients (preventing captive portal errors) while physically dropping WAN access, ensuring students cannot browse the internet during exams.
* **Self-Hosted Digital Workspace:**
  * Deployed `nextcloud` to establish a sovereign, internal file server and document collaboration suite, ensuring full data privacy.
* **DevOps & App Development Sandbox:**
  * Provisioned a dedicated Docker environment for CI/CD testing and deploying in-house applications, specifically hosting the proprietary library management system, **ANSimpleLibrary**.

## 3. Resilience & Optimization Strategy
* **Automated Disaster Recovery:** Scheduled snapshot backups to external storage to ensure rapid Recovery Time Objective (RTO).
* **Resource Maximization:** Strategic utilization of LXC (Linux Containers) for production services and Docker for development, drastically reducing CPU/RAM overhead compared to traditional VMs.
* **Observability:** Centralized real-time health monitoring and proactive alerting via `uptimekuma` integrated with Telegram bots.
