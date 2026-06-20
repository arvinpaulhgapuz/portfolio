# CISCO SOC Essentials – Investigating with Splunk

## 📖 Overview

This project contains practical exercises, investigations, and search techniques performed using Splunk as part of the Cisco SOC Essentials course. The objective is to develop foundational Security Operations Center (SOC) skills by analyzing security events, identifying suspicious activities, and conducting incident investigations using the Splunk Search Processing Language (SPL).

---

## 🎯 Learning Objectives

Upon completing this project, you will be able to:

- Navigate the Splunk interface efficiently.
- Perform searches using SPL (Search Processing Language).
- Investigate security incidents using log data.
- Correlate events from multiple sources.
- Identify indicators of compromise (IOCs).
- Analyze network and endpoint activity.
- Create reports and visualizations for security monitoring.
- Apply SOC investigation methodologies in real-world scenarios.

---

## 🛠 Technologies Used

- Splunk Enterprise
- Search Processing Language (SPL)
- Cisco SOC Essentials Labs
- Windows Event Logs
- Linux System Logs
- Network Security Logs

---

## 📂 Project Contents

### Splunk Fundamentals
- Understanding Splunk architecture
- Data ingestion and indexing
- Searching and filtering events
- Field extraction and analysis

### Security Investigations
- Authentication failures
- Brute-force attack detection
- Privilege escalation attempts
- Malware activity investigation
- Network reconnaissance detection
- Threat hunting exercises

### Data Analysis
- Event correlation
- Statistical analysis
- Timeline reconstruction
- IOC identification

### Visualization and Reporting
- Dashboards
- Charts and graphs
- Security reports
- Alert creation

---

## 🔍 Sample SPL Queries

### Failed Login Attempts

```spl
index=security EventCode=4625
| stats count by Account_Name
| sort - count
```

### Top Source IP Addresses

```spl
index=firewall action=blocked
| stats count by src_ip
| sort - count
```

### Network Traffic Over Time

```spl
index=network
| timechart count by protocol
```

---

## 🧩 Investigation Workflow

1. Review alerts and suspicious events.
2. Collect relevant logs and contextual information.
3. Correlate related activities across data sources.
4. Identify indicators of compromise (IOCs).
5. Determine the scope and impact of the incident.
6. Document findings and recommended actions.

---

## 📚 Skills Developed

- Security Monitoring
- Log Analysis
- Threat Detection
- Incident Response
- Threat Hunting
- SIEM Operations
- SPL Query Development
- SOC Analyst Methodologies

---

## 🚀 Key Takeaways

This project demonstrates how Splunk can be leveraged within a Security Operations Center (SOC) to detect, investigate, and respond to cybersecurity threats. Through hands-on analysis and SPL-based investigations, learners gain practical experience in security monitoring and incident investigation workflows.

---

## 👨‍💻 Author

**Cisco SOC Essentials – Investigating with Splunk**

A hands-on cybersecurity project focused on developing SOC analyst skills through security investigations and log analysis using Splunk.
