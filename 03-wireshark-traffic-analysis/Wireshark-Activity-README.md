# Operation Midnight Crawl

**Wireshark Network Forensics Lab | SOC Tier 1 Analyst Training**

## Ticket

| Field | Detail |
|---|---|
| **PCAP File** | `operation_midnight_crawl.pcap` |
| **Victim Host** | `192.168.10.45` (DESKTOP-HR01) |
| **Time** | 02:14 AM, 01 Jan 2024 |
| **Ticket** | SOC-2024-0089 |
| **Priority** | HIGH |
| **Alert** | Suspicious outbound connection detected |

---

## Task 1 — Open & Get Your Bearings

**Step 1: Open the PCAP in Wireshark**

`File → Open → operation_midnight_crawl.pcap`

**Step 2: Statistics → Protocol Hierarchy**

Reviewed the protocol breakdown before touching any filters — orienting to the full capture first, rather than jumping straight to a filtered view, is the correct analyst workflow and avoids tunnel vision on a single conversation.

**Step 3: Statistics → Conversations**

Opened the **TCP tab** and sorted by **Bytes (descending)** to identify which hosts were exchanging the most data. The top conversations all centered on the victim host talking to two distinct external IPs — one over a non-standard port, one over port 443.

> 💡 **Analyst Rule:** Never filter before orienting. Understanding the full picture first is the real analyst workflow.

---

## Task 2 — Isolate the Victim Host

Applied the filter:

```
ip.addr == 192.168.10.45
```

**Findings:**
- Identified unknown external IPs in the destination column, excluding known-safe infrastructure (Google, Microsoft, etc.)
- Checked for non-standard ports (normal = 80, 443, 53) — found traffic on a port well outside that baseline
- **Beaconing check:** the victim host contacted the same unknown external IP at a consistent, evenly-spaced interval, repeated across multiple cycles

> 🚨 **Red Flag:** Regular, evenly-spaced outbound connections to an unknown IP = malware C2 beaconing. This is the #1 sign of an active infection.

---

## Task 3 — Investigate DNS Activity

Cleared the previous filter and applied:

```
dns
```

**Findings:**
- Identified suspicious, randomized-looking domain names queried by the victim host that did not match any legitimate browsing pattern
- Found at least one query that returned an **NXDOMAIN** (domain not found) response — consistent with a secondary or backup C2 domain that wasn't active at capture time
- Reviewed **Statistics → DNS** to get the total count of unique domains queried during the session, separating legitimate lookups (search engines, OS update services) from attacker infrastructure

---

## Task 4 — Detect the Attack Behaviors

### 🕵️ Port Scan (Reconnaissance)

*One source IP sending SYN to many different ports in seconds.*

```
tcp.flags.syn == 1 and tcp.flags.ack == 0
```

Identified a scanning source IP distinct from the C2 server, sweeping a range of common ports against the victim host. A small subset of those ports responded with SYN-ACK, indicating they were open and reachable.

### 📦 Data Exfiltration (Large Outbound Transfers)

*Stolen data leaving the network — compare bytes sent vs. received in Conversations.*

```
ip.src == 192.168.10.45 and tcp.len > 1000
```

Found a strong asymmetry between outbound and inbound byte counts on the same destination IP used for C2 — far more data leaving the host than entering it, which is the core signature of exfiltration over what looks like ordinary HTTPS traffic.

### 🎮 C2 Port Check (Command & Control on Unusual Ports)

*Expand TCP in the Packet Details panel to inspect the payload.*

```
tcp.port == 4444
```

Traffic was present on this non-standard port. Inspecting the payload in the Packet Details panel revealed plaintext beacon-style markers rather than encrypted or legitimate application data — confirming this channel as the malware's command-and-control link.

---

## Task 5 — Indicators of Compromise (IOCs)

| IOC Type | What to Record | Finding |
|---|---|---|
| Suspicious C2 IP | Unknown external IP the victim beacons to | Identified — see ticket |
| Suspicious Port (C2) | Non-standard port used for C2 communication | Identified — see ticket |
| Malware Domain #1 | Unknown DNS domain queried by victim | Identified — resolves to C2 IP |
| Malware Domain #2 | Second suspicious domain (may be NXDOMAIN) | Identified — returned NXDOMAIN |
| Port Scanner IP | IP that conducted the port scan | Identified — distinct from C2 IP |
| Open Ports Found | Ports that responded to the scan (SYN-ACK) | Identified — small subset of scanned ports |
| Beacon Interval | Time in seconds between each C2 check-in | Identified — consistent fixed interval |
| Data Exfiltrated | Approx. bytes sent outbound to unknown IP | Identified — significant outbound/inbound asymmetry |

---

## Incident Report

**Attack Types Observed:** Port Scan · C2 Beaconing · Data Exfiltration · DNS Anomaly

**Severity:** High
**Confidence Level:** High
**Escalate to Tier 2:** Yes

### Summary of Findings

DESKTOP-HR01 (`192.168.10.45`) was first targeted by a network port scan from an external IP, which discovered a small number of open ports. Around the same time, the host began beaconing to a separate external IP on a non-standard port at a precise, fixed interval — the hallmark of malware command-and-control traffic rather than normal user activity. DNS logs confirmed the host resolving a suspicious, randomly-generated domain to that same C2 IP, with a second backup domain returning NXDOMAIN, suggesting a domain-resilience mechanism built into the malware.

Several beacon cycles in, the C2 channel triggered a large outbound data transfer to the same attacker infrastructure over port 443 — disguised as routine HTTPS traffic, but distinguished by a severe imbalance between bytes sent and bytes received. Combined, the evidence describes a full attack chain: reconnaissance (port scan), command-and-control (beaconing), and data exfiltration, against a single compromised endpoint.

### Recommended Actions

- Isolate `DESKTOP-HR01` from the network immediately to halt further beaconing and exfiltration
- Block the identified C2 and scanner IPs, and the malicious/NXDOMAIN domains, at the firewall and DNS layer
- Capture a forensic image of the host before remediation for further analysis
- Hunt across the environment for the same beacon signature and IOCs on other hosts
- Escalate to Tier 2 for full incident response and root-cause analysis (initial infection vector, e.g. phishing)

---

## Skills Demonstrated

`Wireshark` · `Protocol Hierarchy Analysis` · `Conversation/Bytes Analysis` · `DNS Investigation` · `Port Scan Detection` · `C2 Beacon Identification` · `Data Exfiltration Detection` · `IOC Collection` · `Incident Reporting`

---

*TESDA Cyber Threat Monitoring Level 1 • SOC Tier 1 Foundations*
