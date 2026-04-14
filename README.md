# 🛡️ SOC Security Dashboard — Splunk

> A SOC-style dashboard built in Splunk to monitor authentication events, detect brute-force attempts, analyze system errors, and visualize suspicious activity patterns using indexed log data.

---

## 📌 Project Overview

This project simulates a real-world Security Operations Center (SOC) workflow using Splunk Enterprise. Log data was ingested, parsed, and queried using SPL (Search Processing Language) to identify attack patterns, failed logins, and brute-force attempts — then visualized across a multi-panel custom Splunk dashboard.

*Environment:* Kali Linux | Splunk Enterprise 10.2.2  
*Index:* main  
*Log Sources:* sample_splunk_logs.log, sample.log (custom_log sourcetype)  
*Total Events Indexed:* 31  
*Dashboard Export Date:* 2026-04-10 23:37:02 IST

---

## 🎯 Objectives

- Ingest and index authentication log data into Splunk
- Write SPL queries to detect failed logins, brute-force patterns, and suspicious activity
- Build a multi-panel SOC dashboard covering event volume, authentication failures, IP analysis, and brute-force detection
- Simulate real SOC analyst workflow: event triage, severity classification, IP-based correlation

---

## 📊 Dashboard Panels

### 1. System Event Volume
Displays all indexed events. Captures the full authentication timeline including INFO, ERROR, and WARNING entries across both log sources.

### 2. Authentication Failures — Failed Logins
Targeted view of all failed login events. Identified *13 failed login attempts* across users admin, root, and test from three distinct source IPs.

### 3. Suspicious Activity Signals — Errors & Warnings
Timeline visualization of ERROR and WARNING events showing spike patterns. Field metadata: _time, host, index, linecount, source, sourcetype.

### 4. Source IP Analysis — Activity by IP
Bar chart showing total event count per host. The kali host recorded *~31 events* across the monitored period.

### 5. Brute Force Detection — Attack Counts
Dedicated brute-force panel. The kali host recorded *~12 brute-force-related events* — triggered by Possible brute force detected and Multiple failed login attempts WARNING entries.

### 6. Login Failure Severity
Aggregated severity classification:

| Host | Failed Login Count | Severity |
|------|-------------------|----------|
| kali | 13 | *HIGH* |

---

## 🚨 Key Events Detected

| Timestamp | Level | Event | Source IP |
|-----------|-------|-------|-----------|
| 10:35:59 | ERROR | Failed login — user admin | 192.168.1.30 |
| 10:33:21 | WARNING | Possible brute force detected | 192.168.1.50 |
| 10:32:44 | ERROR | Failed login — user root | 192.168.1.50 |
| 10:29:55 | WARNING | Repeated login failures | 192.168.1.15 |
| 10:27:05 | ERROR | Failed login — user test | 192.168.1.15 |
| 10:26:20 | WARNING | Suspicious activity detected | 192.168.1.30 |
| 10:24:10 | ERROR | Failed login — user admin | 192.168.1.30 |
| 10:22:33 | WARNING | Multiple failed login attempts | 192.168.1.15 |
| 10:21:15 | ERROR | Failed login — user root | 192.168.1.15 |
| 10:31:10 | INFO | User admin logged in (legitimate) | 192.168.1.10 |

*Attacker IPs:* 192.168.1.15, 192.168.1.50, 192.168.1.30  
*Targeted Accounts:* admin, root, test, guest, support  
*Legitimate Source:* 192.168.1.10 (admin successful logins)

---

## 🔍 SPL Queries

### All Events
spl
index=main


### Source-Specific Query
spl
source="sample_splunk_logs.log" host="kali" index="main" sourcetype="Splunk sample logs"


### Failed Login Detection
spl
index=main "Failed login"


### Failed Login Count by Host
spl
index=main "Failed login" | stats count by host


### ERROR Events Only
spl
index=main ERROR


### WARNING Events Only
spl
index=main WARNING


### Combined Threat Events
spl
index=main (ERROR OR WARNING)


### Broad Multi-Keyword Sweep
spl
index=main ("Failed" OR "ERROR" OR "WARNING")


---

## 🗂️ Log Sources

| File | Sourcetype | Notes |
|------|------------|-------|
| sample_splunk_logs.log | Splunk sample logs | 15 structured auth events |
| sample.log | custom_log | Multi-line events; captures login sequences |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| Splunk Enterprise 10.2.2 | Log ingestion, SPL querying, dashboard building |
| SPL | Detection queries and aggregation |
| Kali Linux | Host environment |
| Custom log files | Simulated authentication event data |

---

## 🧠 Skills Demonstrated

- Log ingestion and index configuration in Splunk
- SPL query writing — field filtering, boolean operators, stats aggregation
- Brute-force and failed login detection from raw log data
- Multi-panel dashboard creation with tables, bar charts, and timeline visualizations
- Severity classification (HIGH) based on event correlation
- SOC analyst workflow: triage → filter → correlate → visualize

---

## 📁 Repository Structure


splunk-soc-dashboard/
├── README.md
├── logs/
│   ├── sample_splunk_logs.log
│   └── sample.log
├── queries/
│   └── spl_queries.md
├── dashboard/
│   └── soc_security_dashboard-2026-04-10.pdf
└── screenshots/
    ├── dashboard_overview.png
    ├── authentication_failures.png
    ├── brute_force_detection.png
    └── login_severity.png