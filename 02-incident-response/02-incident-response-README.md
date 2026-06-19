<!-- Replace bracketed placeholders with your real details before publishing. -->

# CASE-002 · Incident Triage, Ticketing & Reporting

`Status: Documented` · `Category: Incident Response` · `Tools: [Ticketing platform], Incident Report Writing`

## Overview

This case covers the part of SOC work that happens *after* detection: deciding what matters, tracking it through to resolution, and writing it up clearly enough that another analyst could pick up the case cold. Findings from the [CASE-001 Splunk dashboard work](../01-splunk-siem-monitoring) fed directly into this workflow.

## Workflow

```
Alert / Finding  →  Ticket Logged  →  Triage  →  Investigation  →  Report  →  Close
```

1. **Log it** — every alert or finding worth tracking was entered into [ticketing platform] as a ticket, rather than left as a one-off search result.
2. **Triage it** — each ticket was scored for priority before deep investigation, using the criteria below.
3. **Investigate** — pulled supporting evidence (Splunk searches, packet captures, honeypot logs) to confirm or rule out the ticket.
4. **Report it** — higher-priority tickets were written up using the [incident report template](./incident-report-template.md).
5. **Close it** — ticket updated with final status and a link to the report.

## Triage / Priority Criteria

| Priority | Criteria | Example |
|---|---|---|
| **P1 – Critical** | Active compromise, confirmed data exposure | [example] |
| **P2 – High** | Strong indicator of compromise, no confirmed impact yet | [example] |
| **P3 – Medium** | Suspicious but plausible benign explanation | [example] |
| **P4 – Low** | Informational / noise, logged for trend tracking | [example] |

## Sample Tickets

See [`sample-tickets.md`](./sample-tickets.md) for [X] example tickets logged during this lab, showing the fields used and how priority was assigned.

## Incident Report

See [`incident-report-template.md`](./incident-report-template.md) for the reusable template, and [add filenames] for completed example reports written during this lab.

## Skills Demonstrated

- Alert triage and prioritization under a defined framework
- Ticketing/case management workflow
- Incident report writing (summary, timeline, IOCs, recommendations)
- Clear, audience-aware technical communication

## Reflection

[1–3 sentences: what made triage harder than expected, or what you learned about writing reports for someone who wasn't there when the incident happened.]
