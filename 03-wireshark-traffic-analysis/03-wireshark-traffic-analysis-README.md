<!-- Replace bracketed placeholders with your real details before publishing. -->

# CASE-003 · Network Traffic & Packet Analysis

`Status: Documented` · `Category: Network Forensics` · `Tools: Wireshark`

## Overview

Most SOC tooling eventually points back to "go look at the packets." This case is about building that habit directly — capturing and reading raw network traffic in Wireshark instead of relying on summarized alerts.

## Lab Environment

| Component | Detail |
|---|---|
| Capture Tool | Wireshark [version] |
| Capture Source | [e.g., home lab network interface, sample PCAP set, honeypot segment from CASE-004] |
| Network Context | [brief description of what traffic was being captured and why] |

## Methodology

1. **Orient with protocol hierarchy** — opened each capture and reviewed *Statistics → Protocol Hierarchy* before drilling into individual frames, to understand the overall traffic mix first.
2. **Filter with intent** — used display filters to isolate specific conversations and patterns rather than scrolling raw captures.
3. **Follow the stream** — reconstructed full TCP/HTTP sessions using *Follow → TCP Stream* to see what actually happened end-to-end, not just that a connection occurred.
4. **Baseline vs. anomaly** — compared a capture of "normal" traffic against a capture with suspicious activity to build a feel for what should draw attention.

## Key Filters Used

> Replace with the filters you actually relied on and a one-line note on what each one isolates.

```
# Isolate traffic to/from a specific host
ip.addr == [x.x.x.x]

# Spot potential port-scan behavior (SYN without ACK)
tcp.flags.syn==1 && tcp.flags.ack==0

# Surface plaintext credentials in unencrypted protocols
http.request and (http contains "password")

# [Add your own filter here]
```

## Findings

- [Finding 1 — e.g., "Identified repeated SYN packets from a single source across a range of ports, consistent with a port scan."]
- [Finding 2]
- [Finding 3]

## Screenshots

![Capture screenshot](./screenshots/capture-overview.png)
*[One-line caption: what this capture shows and the filter used to find it.]*

## Skills Demonstrated

- Packet and protocol analysis
- Wireshark display filter syntax
- TCP stream reconstruction
- Traffic baselining and anomaly identification

## Reflection

[1–3 sentences: what surprised you when you actually looked at the packets versus what you expected, or what filter took the longest to get right.]
