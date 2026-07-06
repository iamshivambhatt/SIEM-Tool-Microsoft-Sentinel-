# Section 1: SIEM Implementation — Microsoft Sentinel

**Course:** CYBR3090
**Author:** Shivam Bhatt
**Date:** April 19, 2026

## Overview

This project implements a cloud-native SIEM solution using **Microsoft Sentinel** to collect, aggregate, and analyze security events from a sandboxed lab environment. The goal was to gain hands-on experience with real-time monitoring, log analytics, and automated threat detection using an industry-standard SIEM/SOAR platform.

## Tool Selection

| Attribute | Detail |
|---|---|
| **Tool** | Microsoft Sentinel |
| **Vendor site** | https://azure.microsoft.com/en-ca/products/microsoft-sentinel |
| **License type** | Free trial (personal Azure account) |
| **Trial terms** | New Azure accounts receive $200 USD in credits for 30 days. Sentinel adds a separate 31-day trial waiving data ingestion charges up to 10 GB/day |
| **Trial window** | Activated April 18, 2026 → expires May 19, 2026 |
| **Pricing after trial** | Consumption-based, billed per GB ingested |
| **Deployment model** | Fully cloud-native — no local software installation required; managed entirely from a browser |

## Architecture

- **Log Analytics Workspace** (`CYBR3090-Workspace`) — central data store for all ingested logs, queried via KQL (Kusto Query Language)
- **Microsoft Monitoring Agent (MMA)** — installed on the Windows 10 sandbox VM (`DESKTOP-MM1IHBE`) to forward Windows event logs and heartbeat signals to the workspace in real time
- **Analytics Rules Engine** — scheduled detection rules that automatically raise incidents when defined thresholds are exceeded

> **Note:** The Log Analytics Workspace and the Sentinel resource are separate Azure resources that must be created independently and then linked together — Sentinel does not create its own workspace automatically.

## Key Features Demonstrated

- **Cloud-native monitoring** — accessible from any browser, no on-prem infrastructure
- **KQL query editor** — used to query device uptime, event counts, and establish baseline behavior for the sandbox VM
- **Heartbeat monitoring** — continuous liveness signal from connected devices; the Win10 VM sent 82 heartbeats over 10 hours of uptime during the demo
- **Custom Analytics Rule** — a **VM Heartbeat Monitor** rule was built to automatically generate an incident if the workstation stops sending heartbeats (e.g., due to outage or compromise)
- **Incident management dashboard** — correlates related alerts into a single incident with severity scoring (High / Medium / Low)
- **Content Hub** — one-click installation of pre-built connectors, analytics rule templates, and workbooks

## Deliverables

1. **Working sandbox demonstration** — Sentinel connected to the Windows 10 VM via MMA, ingesting heartbeat and event log data
2. **Demo video (5–10 min)** — walkthrough of the deployment, live KQL queries, and the custom analytics rule triggering an incident
3. **Instructor briefing sheet** (`Briefing_Sheet_-_SIEM_(Microsoft_Sentinel).pdf`) — one-page summary covering tool selection, licensing, features, production use case, and lessons learned

## Use in a Production Environment

At enterprise scale, Sentinel would serve as the central security monitoring hub:

- All endpoints (Windows/Linux workstations, servers), network devices (firewalls, routers, switches), identity providers (Azure AD), and cloud workloads connected via data connectors
- The **Incidents queue** used as the SOC analyst's daily triage dashboard
- Scheduled **KQL hunting queries** proactively searching for indicators of compromise
- **Analytics rules** automating Tier-1 detection — e.g., alerting on heartbeat loss, brute-force login attempts, or anomalous outbound traffic
- **Playbooks** (SOAR) automating response actions such as disabling compromised accounts or blocking malicious IPs
- **Workbooks** generating compliance reporting aligned with NIST CSF and ISO 27001

## Lessons Learned

- The Log Analytics Workspace must exist before Sentinel is deployed — they are linked, not bundled
- MMA is the legacy (but simplest) connection method for non-Azure/local Hyper-V VMs that lack Azure Arc
- Sentinel's Content Hub and Data Connectors have migrated to the **Microsoft Defender portal** (security.microsoft.com); the classic Azure portal auto-redirects there
- KQL has a learning curve, but built-in query templates and IntelliSense make it approachable — `filter`, `summarize`, and `project` cover most day-to-day analysis needs

## Files

- `Briefing_Sheet_-_SIEM_(Microsoft_Sentinel).pdf` — one-page instructor briefing sheet
- Demo video (submitted separately, 5–10 min) — not included in this repo
