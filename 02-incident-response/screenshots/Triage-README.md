# SOC Analyst Training Lab — Incident Triage (BOTSv2 Dataset)

**Splunk · Cyber Kill Chain · MITRE ATT&CK**

## Activity Overview

| Field | Detail |
|---|---|
| **Activity Title** | Semi-Advanced Log Analysis & Incident Triage |
| **Dataset** | BOTSv2 (Boss of the SOC v2) — Splunk |
| **Dataset Time Period** | August 2017 (Splunk time picker: *All Time* or Aug 1 – Sep 1, 2017) |
| **Difficulty Level** | Intermediate – Semi-Advanced |
| **Number of Tickets** | 10 Incident Triage Tickets |
| **Frameworks Used** | Cyber Kill Chain (7 Phases) + MITRE ATT&CK |
| **Estimated Duration** | 60–90 minutes per ticket; full lab = 2–3 sessions |
| **Deliverable** | Completed Triage Report Form for each ticket |

## Learning Objectives

By the end of this activity, an analyst should be able to:

- Perform targeted Splunk SPL searches on real-world attack data
- Identify attacker activity and map it to the correct Cyber Kill Chain phase
- Apply MITRE ATT&CK technique IDs to observed behaviors
- Identify Indicators of Compromise (IOCs) from log evidence
- Write a structured, professional incident triage report

## Setting Up the Splunk Environment

| Step | What to Do |
|---|---|
| 1. Open Splunk | Launch Splunk Enterprise and log in (default: `http://localhost:8000`) |
| 2. Go to Search | Open the **Search & Reporting** app from the Splunk home screen |
| 3. Set Time Range | Set the time picker to *All Time*, or manually enter Aug 1, 2017 – Sep 1, 2017 |
| 4. Set the Index | All queries use `index=botsv2` — confirm this index exists in your instance |
| 5. Run Sanity Check | Run the verification query below before starting |

**Sanity check query:**
```spl
index=botsv2 | stats count by sourcetype | sort -count
```
Expected sourcetypes: `xmlwineventlog`, `stream:http`, `stream:dns`, `suricata`, `stream:smtp`

## Quick Reference — Cyber Kill Chain Phases

| Phase | What the Attacker is Doing |
|---|---|
| 1 · Reconnaissance | Gathers info about the target — port scans, OSINT, DNS enumeration |
| 2 · Weaponization | Creates the exploit/payload (usually not visible in logs) |
| 3 · Delivery | Payload is delivered — phishing email, drive-by download, USB drop |
| 4 · Exploitation | Vulnerability is triggered — SQLi, buffer overflow, macro execution |
| 5 · Installation | Malware is installed — persistence via registry, scheduled tasks, startup |
| 6 · Command & Control | C2 channel established — HTTP beaconing, DNS tunnelling |
| 7 · Actions on Objectives | Final goal — data exfiltration, ransomware, destruction, lateral movement |

## Student Instructions

**Step 1 — Read the Ticket**
Read the scenario carefully and note the IOCs (IP addresses, filenames, domains, event IDs) — these are the starting points for investigation.

**Step 2 — Run the Splunk Query**
Copy the provided SPL query into the Splunk search bar, set the time range to *All Time* or August 2017, and observe the results. Modify the query as needed (add filters, change fields, adjust time windows) to dig deeper.

**Step 3 — Answer the 5 Analyst Tasks**
Each ticket has 5 questions, answered using evidence from Splunk — no guessing. Every answer should reference a specific log field, value, or count. Map findings to the Kill Chain phase and the MITRE ATT&CK technique shown on the ticket.

**Step 4 — Complete the Triage Report Form**
Fill in every field of the Triage Report Form. The Summary of Findings should describe **what** happened, **how** it was found, and **what** the evidence shows. Recommended Response Actions should cover containment, eradication, and recovery.

---

## TKT-004 — Malicious PowerShell Execution and Registry Persistence

**Severity:** HIGH &nbsp;|&nbsp; **Kill Chain:** Installation &nbsp;|&nbsp; **MITRE:** T1059.003 · T1547.001

### Scenario
A Sysmon alert fired on a Frothly Windows workstation. PowerShell was observed launching with an encoded command flag from an unusual parent process — Microsoft Word. Shortly after, registry changes were detected. This pattern is consistent with a malicious macro inside a phishing document dropping a payload and establishing persistence.

### IOCs / Artifacts
- Windows host (`10.0.1.x`)
- `winword.exe` → `powershell.exe`
- `-EncodedCommand` flag
- Registry Run key modified
- Sysmon Event ID 1

### Splunk Search Query
```spl
index=botsv2 sourcetype="xmlwineventlog:microsoft-windows-sysmon/operational" EventCode=1
| search ParentImage="*winword*" Image="*powershell*"
| table _time, Computer, CommandLine, ParentImage, User
```

**Alternative — check PowerShell script execution logs:**
```spl
index=botsv2 sourcetype=powershell:scriptexecutionsummary
| table _time, ComputerName, UserName, ScriptName, CommandLine
| sort -_time
```

### Analyst Tasks

| # | Question |
|---|---|
| Q1 | What is the full PowerShell command line in the log? Try to decode the Base64 encoded command string. |
| Q2 | Which parent process spawned PowerShell? Why is this parent-child relationship suspicious? |
| Q3 | What registry key was created and what value was set? How does this achieve persistence across reboots? |
| Q4 | Map to Kill Chain Installation phase and identify two MITRE ATT&CK techniques that apply to this ticket. |
| Q5 | What endpoint response steps must be documented in the ticket before escalating to senior analyst? |

### Triage Report Form

| Field | Details |
|---|---|
| **Analyst Name** | Arvin Paul H. Gapuz |
| **Date & Time** | June 11, 2026, 1:45 PM |
| **Ticket ID** | TKT-004 |
| **Severity** | HIGH — Malicious PowerShell Execution and Registry Persistence |
| **Kill Chain Phase** | Installation |
| **MITRE ATT&CK Technique(s)** | T1059.003 — Command and Scripting Interpreter: Windows PowerShell; T1547.001 — Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder |
| **Affected Host(s)** | Windows Host 10.0.1.x (Frothly Workstation) |
| **Source IP / Attacker IP** | `Identity="986bade3-97b2-44dc-9088-4df053329e42"` `InvocationLine="&\"$SplunkHome\etc\apps\Splunk_TA_microsoft_ad\bin\Invoke-MonitoredScript.ps1\" -Command \".\powershell\2012r2-siteinfo.ps1\""` `TerminatingError="False"` `ErrorCount="0"` `Elapsed="00:00:00.649"` |

**Summary of Findings**

A Frothly Windows workstation executed a PowerShell process spawned by Microsoft Word. The PowerShell command used the `-EncodedCommand` flag, indicating command obfuscation. Shortly afterward, Registry Run Key modifications were detected, establishing persistence on the endpoint. Sysmon Event ID 1 logs confirmed the suspicious parent-child relationship (`winword.exe` → `powershell.exe`). The activity is consistent with a phishing document containing a malicious macro that executed a payload and configured persistence mechanisms.

**IOCs Identified**

- Windows host: `10.0.1.x`
- Parent process: `winword.exe`
- Child process: `powershell.exe`
- `-EncodedCommand` flag
- Registry Run key modification
- Sysmon Event ID 1
- Potential malicious Word document
- Suspicious PowerShell script execution

**Recommended Response Actions**

*Containment*
- Isolate affected endpoint immediately
- Block associated indicators and payloads
- Disable affected user account if compromise is suspected

*Eradication*
- Remove malicious registry persistence
- Delete malicious documents and payloads
- Conduct malware scan and remediation
- Review PowerShell execution logs

*Recovery*
- Restore affected files if necessary
- Verify endpoint integrity
- Reconnect system after validation
- Continue monitoring for recurring persistence attempts

**Lessons Learned / Detection Gap**

Observed command pattern:
```
winword.exe -> powershell.exe -EncodedCommand <Base64_String>
```

The scenario indicates the Base64-encoded PowerShell command was used to download or execute a malicious payload, evade detection through command obfuscation, and establish persistence using Registry Run Keys.

**Finding:** Encoded PowerShell execution is a common malware technique used to hide malicious commands from security tools and analysts.

(Back To [`/02-incident-response`](./02-incident-response/02-incident-response-README.md)
