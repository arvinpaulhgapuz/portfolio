<!--
  BEFORE PUBLISHING, replace every bracketed placeholder:
  [Your Name], [your.email@example.com], [linkedin.com/in/you], [github.com/you],
  [Honeypot tool], [Ticketing platform], [Year]
  Also swap the italic "e.g." notes inside each case for what you actually did,
  and adjust the 15-Day Timeline to match your real schedule.
-->

# 🛡️ [Your Name] — SOC Analyst Portfolio

**15-day self-directed training lab:** SIEM threat detection, network forensics, honeypot threat intelligence, and incident response.

![Status](https://img.shields.io/badge/status-open%20to%20work-2FBE9F?style=flat-square)
![Splunk](https://img.shields.io/badge/SIEM-Splunk-DD9A3D?style=flat-square)
![Wireshark](https://img.shields.io/badge/Network-Wireshark-1B9CFC?style=flat-square)
![Kali Linux](https://img.shields.io/badge/Lab-Kali%20Linux-557C94?style=flat-square)
![License](https://img.shields.io/badge/docs-MIT-8C96A5?style=flat-square)

> A home lab project built to practice the full lifecycle of a security incident — from raw packet capture to a written report — using the same categories of tooling found in real SOC environments. Built in 15 days as a self-directed program, not a job; treat this as evidence of hands-on initiative going into entry-level SOC / security operations roles.

🔗 **Live site:** `[add your GitHub Pages / hosted link here]`

---

## Table of Contents

- [About](#about)
- [Case Files](#case-files)
  - [CASE-001 · SIEM Threat Detection & Dashboards](#case-001--siem-threat-detection--dashboard-build)
  - [CASE-002 · Incident Triage, Ticketing & Reporting](#case-002--incident-triage-ticketing--reporting)
  - [CASE-003 · Network Traffic & Packet Analysis](#case-003--network-traffic--packet-analysis)
  - [CASE-004 · Honeypot Deployment & Threat Intelligence](#case-004--honeypot-deployment--threat-intelligence)
- [15-Day Program Timeline](#15-day-program-timeline)
- [Skill Set](#skill-set)
- [Repo Structure](#repo-structure)
- [Contact](#contact)

---

## About

I'm **[Your Name]**, an aspiring SOC Analyst building hands-on detection and response skills outside the classroom. [Add 2–3 sentences: your background before this lab, what drew you to security, and the type of role you're targeting.]

**Lab stack:** Splunk Enterprise · BOTSv2 Dataset · Wireshark · Kali Linux · [Honeypot tool] · [Ticketing platform]

---

## Case Files

Each case below mirrors a real SOC workflow stage — detection, response, network forensics, and adversary intelligence — and links to the supporting evidence folder in this repo.

### CASE-001 · SIEM Threat Detection & Dashboard Build
`Detection & Monitoring` · `Splunk Enterprise` · `BOTSv2 Dataset` · `SPL` · **Status: Documented**

**Objective:** Stand up a working SIEM environment and turn raw security logs into actionable visual monitoring, using "Boss of the SOC" v2 — an industry-standard dataset simulating a real breach against a fictional company.

**What I did:**
- Ingested and explored the BOTSv2 dataset in Splunk, learning its index and sourcetype structure before writing any searches.
- Wrote SPL queries to surface indicators of compromise — *[e.g., suspicious authentication activity, anomalous process execution, unusual DNS query patterns — swap in what you actually focused on]*.
- Designed and built Splunk dashboards from scratch: panels, visualizations, and time-range controls for a fast read on environment health.
- Rehearsed the dashboard as a live demo, practicing how to walk a non-technical audience through what each panel means.

**Skills demonstrated:** SPL query writing · log correlation · dashboard design · technical presenting

📁 Evidence: [`/01-splunk-siem-monitoring`](./01-splunk-siem-monitoring) — SPL queries, dashboard exports, screenshots

---

### CASE-002 · Incident Triage, Ticketing & Reporting
`Incident Response` · `[Ticketing platform]` · **Status: Documented**

**Objective:** Practice the part of SOC work that happens after detection — deciding what matters, tracking it, and writing it up the way a real analyst hands a case off to the next shift.

**What I did:**
- Logged alerts and findings from the Splunk dashboard work as tickets, rather than leaving them as one-off searches.
- Practiced triage: assigning priority and severity based on *[e.g., asset criticality, likelihood, potential business impact]* instead of treating every alert the same.
- Wrote structured incident reports for higher-priority cases — summary, timeline, indicators of compromise, and recommended remediation.
- Treated documentation as a deliverable in itself.

**Skills demonstrated:** alert triage · prioritization · ticketing workflows · incident report writing

📁 Evidence: [`/02-incident-response`](./02-incident-response) — report template and example tickets

---

### CASE-003 · Network Traffic & Packet Analysis
`Network Forensics` · `Wireshark` · **Status: Documented**

**Objective:** Build the habit of reading raw network traffic instead of trusting summaries.

**What I did:**
- Captured and analyzed live and saved traffic in Wireshark, reading the protocol hierarchy before drilling into individual frames.
- Used display filters to isolate specific conversations and suspicious patterns — *[e.g., unexpected ports, plaintext credentials, repeated connection attempts]*.
- Followed TCP streams end-to-end to reconstruct what happened in a session.
- Compared baseline "normal" traffic against anomalous captures.

**Skills demonstrated:** packet analysis · Wireshark filters · traffic baselining · anomaly identification

📁 Evidence: [`/03-wireshark-traffic-analysis`](./03-wireshark-traffic-analysis) — capture notes and filter examples

---

### CASE-004 · Honeypot Deployment & Threat Intelligence
`Adversary Intelligence` · `Kali Linux` · `[Honeypot tool]` · **Status: Documented**

**Objective:** Get a firsthand look at what automated attackers actually do, by giving them something to attack inside a safely isolated home lab.

**What I did:**
- Built and isolated a home lab network segment so the honeypot could be exposed without putting other devices at risk.
- Deployed a honeypot on Kali Linux to passively log connection attempts, credentials tried, and commands run.
- Reviewed the resulting logs for patterns — common usernames/passwords attempted, scanning behavior, follow-on actions.
- Cross-referenced honeypot activity with Wireshark captures of the same traffic.

**Skills demonstrated:** honeypot configuration · network segmentation · log analysis · attacker TTP identification

📁 Evidence: [`/04-kali-honeypot-homelab`](./04-kali-honeypot-homelab) — network diagram and log analysis notes

---

## 15-Day Program Timeline

*(General structure shown below — edit day ranges/phases to match your real schedule.)*

| Days | Phase | Focus |
|------|-------|-------|
| 1–2 | Foundations | Cybersecurity awareness, threat landscape, lab environment setup |
| 3–6 | SIEM Monitoring | Splunk setup, BOTSv2 ingestion, SPL practice, dashboard build, live demo |
| 7–9 | Network Forensics | Wireshark capture/filter practice, baseline vs. anomaly comparison |
| 10–12 | Adversary Lab | Kali Linux honeypot deployment, network isolation, log collection |
| 13–14 | Incident Response | Ticketing workflow, triage practice, incident report writing |
| 15 | Wrap-Up | Portfolio documentation, demo rehearsal, lessons learned |

## Skill Set

| Category | Skills |
|---|---|
| Detection & Monitoring | Splunk, SPL, dashboard design, log correlation |
| Network Forensics | Wireshark, protocol analysis, PCAP review, TCP stream reconstruction |
| Lab & Linux Ops | Kali Linux, honeypot deployment, network segmentation, Linux CLI |
| IR & Communication | Triage, ticketing, incident reports, stakeholder demos |

## Repo Structure

```
soc-analyst-portfolio/
├── README.md
├── 01-splunk-siem-monitoring/
│   ├── README.md              # write-up of approach + findings
│   ├── spl-queries.md         # your actual SPL queries, commented
│   └── screenshots/           # dashboard panels, search results
├── 02-incident-response/
│   ├── README.md
│   ├── incident-report-template.md
│   └── sample-tickets.md
├── 03-wireshark-traffic-analysis/
│   ├── README.md
│   ├── pcap-findings.md       # filters used + what they revealed
│   └── screenshots/
├── 04-kali-honeypot-homelab/
│   ├── README.md
│   ├── network-diagram.png    # how the lab was isolated
│   └── attack-log-analysis.md
└── assets/
    └── screenshots/
```

> Tip for recruiters skimming GitHub: keep real screenshots (dashboard panels, Wireshark captures, honeypot logs) inside each folder — visual proof of hands-on work matters more than extra prose.

## Contact

- **Email:** [your.email@example.com]
- **LinkedIn:** [linkedin.com/in/you]
- **GitHub:** [github.com/you]

---
*Built as part of a 15-day self-directed SOC training lab. © [Year] [Your Name].*
