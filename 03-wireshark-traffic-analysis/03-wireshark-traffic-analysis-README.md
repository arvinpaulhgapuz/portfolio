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

- Identified 21 SYN packets from a single source (45.77.103.22) sweeping 21 distinct destination ports — common service ports plus database/remote-access ports (FTP, SSH, RDP, MSSQL, MySQL, VNC, Redis, MongoDB) — completed in under 350ms, each met with an immediate RST,ACK. Consistent with an automated TCP port scan against the victim host, not a legitimate connection attempt.
- Identified 6 short-lived TCP sessions from the victim to 185.220.101.47:4444, spaced almost exactly 60 seconds apart (±0.16s), each following the same pattern: SYN → SYN/ACK → ACK → small PSH,ACK (120 bytes out, ~63-67 bytes back) → FIN. This regular interval and fixed payload size is consistent with C2 beaconing/check-in traffic, not normal application behavior.
- Identified that the beacon channel (port 4444) and a large data transfer (port 443, ~64KB, same IP) are not independent events — the port-443 burst starts exactly 1 second after the 6th beacon completes. This timing is consistent with the beacon receiving a "go" instruction on its final check-in, followed immediately by payload download or data exfiltration over a TLS-looking port to blend in with normal traffic.

## Screenshots

![Capture screenshot](./screenshots/WiresharkHandsOnLabReport.png)
![Capture screenshot](./screenshots/WiresharkHandsOnLabReport2.png)
*This is a simulated incident called "Operation Midnight Crawl" — a training PCAP (operation_midnight_crawl.pcap) built around a fictional SOC alert: a host named DESKTOP-HR01 (192.168.10.45) triggered a high-priority alert for suspicious outbound connections. The lab teaches the standard SOC Tier 1 triage flow — orient first with stats, then isolate the host, then dig into DNS — to confirm a beaconing malware infection.*

## Skills Demonstrated

- Packet and protocol analysis
- Wireshark display filter syntax
- TCP stream reconstruction
- Traffic baselining and anomaly identification

## Reflection

What actually surprised me was the Task 1 conversations table flattening two different things into one row — port 443 and port 4444 are the same destination IP, so Wireshark's IP-pair grouping makes a single coordinated beacon→payload sequence look like two unrelated entries. If I'd only read the worksheet, I'd have walked away thinking "beaconing" and "large transfer" were separate findings. Only once I tracked it by timestamp did it become obvious they're one continuous event 1 second apart.
