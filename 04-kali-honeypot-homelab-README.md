<!-- Replace bracketed placeholders with your real details before publishing. -->

# CASE-004 · Honeypot Deployment & Threat Intelligence

`Status: Documented` · `Category: Adversary Intelligence` · `Tools: Kali Linux, [Honeypot tool], Home Lab`

## Overview

A honeypot gives a firsthand, unfiltered look at what automated attackers actually do — without putting real systems at risk. This case covers deploying one inside an isolated home lab segment and analyzing what it caught.

## Lab Environment

| Component | Detail |
|---|---|
| Host OS | Kali Linux [version] |
| Honeypot Software | [e.g., Cowrie / Dionaea / T-Pot] |
| Virtualization | [e.g., VirtualBox / VMware / Proxmox] |
| Network Isolation | [e.g., host-only adapter / isolated VLAN / separate virtual switch with no route to personal devices] |
| Exposure | [e.g., exposed only within the lab network / port-forwarded from router for internet-facing test] |

## Network Diagram

![Network diagram](./network-diagram.png)
*[Caption describing how the honeypot was segmented from the rest of the home network.]*

## Methodology

1. **Isolate first** — built a dedicated network segment so the honeypot could be exposed without any path back to personal devices or data.
2. **Deploy & configure** — installed and configured [honeypot tool] on Kali Linux to passively log connection attempts, credentials tried, and any commands executed by a connecting party.
3. **Let it run** — left the honeypot live for [duration] to collect a realistic sample of automated scanning/attack traffic.
4. **Analyze the logs** — reviewed honeypot logs for patterns: common usernames/passwords attempted, source IP behavior, scanning cadence, and any follow-on actions after a simulated "successful" login.
5. **Cross-reference with packets** — correlated honeypot application-layer logs against the matching Wireshark capture (see [CASE-003](../03-wireshark-traffic-analysis)) to connect what was logged at the service level with what was visible on the wire.

## Findings

- [Finding 1 — e.g., "Most login attempts used a small, repeated set of default credentials, consistent with automated credential-stuffing tools rather than targeted human attackers."]
- [Finding 2 — e.g., observed scanning cadence/timing pattern]
- [Finding 3 — e.g., notable source IP ranges or ASN patterns, if explored]

## Skills Demonstrated

- Honeypot deployment and configuration
- Network segmentation and isolation design
- Log analysis and pattern recognition
- Correlating application-layer and network-layer evidence

## Reflection

[1–3 sentences: what did seeing live attack attempts teach you that reading about threats didn't, and what would you add to the lab next — e.g., a second honeypot type, longer collection window, or basic geolocation/ASN enrichment of source IPs.]
