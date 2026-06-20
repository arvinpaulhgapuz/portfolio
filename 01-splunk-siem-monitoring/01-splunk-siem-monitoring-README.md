<!-- Replace bracketed placeholders with your real details before publishing. -->

# CASE-001 · SIEM Threat Detection & Dashboard Build

`Status: Documented` · `Category: Detection & Monitoring` · `Tools: Splunk Enterprise, BOTSv2 Dataset, SPL`

## Overview

This case covers standing up a working SIEM environment and learning to turn raw security logs into actionable monitoring. The lab uses **BOTSv2 (Boss of the SOC v2)** — a publicly available dataset built by Splunk for SOC training, containing logs from a simulated breach against a fictional brewing company ("Frothly"). It includes a mix of Windows event logs, network/wire data, and endpoint telemetry, which makes it realistic enough to practice real detection workflows on.

## Lab Environment

| Component | Detail |
|---|---|
| SIEM Platform | Splunk Enterprise [version] |
| Dataset | BOTSv2 |
| Indexes Used | `[index names, e.g., botsv2]` |
| Key Sourcetypes | `[e.g., WinEventLog:Security, stream:http, suricata]` |
| Deployment | [Single instance on local VM / Docker / etc.] |

## Methodology

1. **Ingest & orient** — loaded the BOTSv2 dataset into Splunk and ran broad searches (`index=botsv2`) to understand what sourcetypes and fields were available before writing any targeted queries.
2. **Hunt with SPL** — wrote searches to surface indicators of compromise across the simulated environment.
3. **Build dashboards** — translated the most useful searches into permanent dashboard panels: time charts for activity volume, tables for top talkers/users, and single-value panels for at-a-glance health checks.
4. **Rehearse the demo** — practiced presenting the dashboard live, narrating what each panel shows and why it matters to someone without a security background.

## Key SPL Queries

> Replace these with your actual queries and a one-line note on what each one is hunting for.

```spl
# [Describe what this search detects]
index=botsv2 sourcetype=[sourcetype]
| stats count by [field]
| sort -count

# [Describe what this search detects]
index=botsv2 [search terms]
| timechart span=1h count by [field]
```

## Findings

- Finding 1 — Identifies which source IP addresses are generating the highest volume of HTTP traffic — useful for spotting a single host or external IP responsible for an unusually large share of activity, a common indicator of scanning, beaconing, or compromised hosts.
- Finding 2 - Plots total event volume over time in hourly buckets, making sudden spikes or drops visible at a glance. A sharp, isolated spike — like the one in your screenshot around Aug 7 — often indicates a burst of automated activity (a scan, an attack, or a bulk data transfer) rather than normal user behavior, and is usually the first thing worth investigating further.


## Dashboard
![SPLUNK screenshot](./screenshots/SPLUNK_Enterprise_Dashboard.png)

![Dashboard screenshot](./screenshots/BotsV2-Security-Overview-Dashboard.png)
Dashboard overview: top source IPs, HTTP status code distribution, traffic volume over time, and top Windows login accounts — used to spot anomalies (unusual spikes, dominant accounts, unexpected status codes) at a glance.

## Skills Demonstrated

- SPL query writing (search, stats, timechart, eval)
- Log correlation across multiple sourcetypes
- Dashboard and visualization design
- Translating technical findings for a non-technical audience

## Reflection

The hardest part was field discovery — figuring out which raw field actually mapped to "source IP" or "account name" across different sourcetypes, since BOTSv2 has inconsistent naming and some fields only appear after expanding specific event types. With more time, I'd build a quick field-mapping reference up front before touching any panels, instead of discovering inconsistencies mid-build and having to backtrack on queries I'd already written.
