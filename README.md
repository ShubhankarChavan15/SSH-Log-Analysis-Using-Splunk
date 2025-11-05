# SSH-Log-Analysis-Using-Splunk
Analyze SSH authentication logs with Splunk to detect failed logins, brute-force attempts, and visualize attacker origins.
# 🔐 SSH Log Analysis using Splunk

A cybersecurity project focused on **SSH authentication analysis** using **Splunk dashboards**.  
This lab demonstrates how to visualize successful and failed logins, detect possible brute-force attacks, and identify unauthenticated connections.

---

## 🎯 Objective

The goal of this project is to analyze SSH authentication logs to detect:

- ✅ Successful logins (who connected, from where)  
- ❌ Failed login attempts (possible brute-force or password spraying)  
- 🚨 Multiple failed authentication attempts (indicators of brute-force)  
- ⚠️ Connections without authentication (potential scanning or incomplete sessions)

By completing this project, you’ll learn how to:

- Ingest SSH logs into Splunk  
- Run SPL queries for authentication activity  
- Create dashboards and alerts for SSH-related threats  

---

## 🧩 Lab Setup & Prerequisites

- 🖥️ **Splunk Enterprise or Free Edition**  
- 📄 **Dataset:** `ssh_logs.json`  
- ⚙️ **Sourcetype:** `_json`  
- 🗂️ **Index:** `ssh_logs`  

---

## 🧱 Preparation

1. Log in to your Splunk instance.  
2. Go to **Apps → Search & Reporting**.  
3. Click **Add Data → Upload**.  
4. Select the file `ssh_log.json`.  
5. Set **Sourcetype** to `_json`.  
6. Index it under a new index, e.g. `ssh_logs`.  
7. Click **Start Searching** to confirm the ingestion.  

---

## 🧮 Task 1: Ingest and Parse Logs

Upload and validate your SSH logs.

### ✅ Validation Query

```spl
index=ssh_logs 
| stats count by event_type
```

### 🚨 Task 2: Analyze Failed Login Attempts
```spl
index=ssh_logs event_type="Failed SSH Login"
| stats count by id.orig_h
```
Highlight the top 10 source IPs generating failed logins.
Create a bar chart visualization for failed login attempts per source IP.

### 🔐 Task 3: Detect Multiple Failed Authentication Attempts (Brute Force)

Search for multiple failed attempts in logs:

```spl
index=ssh_logs event_type="Multiple Failed Authentication Attempts"
| stats count by id.orig_h, id.resp_h
```

Detect repeated failures (e.g., more than 5 attempts).
Configure a Splunk alert:

Trigger when any IP attempts more than 5 logins within 10 minutes.

### ✅ Task 4: Track Successful Logins

Search for successful logins:

```spl
index=ssh_logs event_type="Successful SSH Login"
| stats count by id.orig_h, id.resp_h
```

Compare successful logins against prior failed attempts (to detect compromised accounts).
Create a dashboard panel showing top source IPs for successful logins.

### 🕵️‍♂️ Task 5: Spot Suspicious Connections Without Authentication

Search for unauthenticated SSH connections:

```spl
index=ssh_logs event_type="Connection Without Authentication"
| stats count by id.orig_h
```

Create a timechart visualization to monitor such events over time:

```spl
index=ssh_logs event_type="Connection Without Authentication"
| timechart count by id.orig_h
```
Identify repeated unauthenticated attempts — potential indicators of port scanning or SSH probing.

### 🧮 Dashboard Panels Summary

* Total SSH Events

* Successful Logins

* Failed Logins

* Invalid User Attempts (Unauthenticated Logins)

* Failed Logins by IP Adressess (Bar Chart)

* Possible Brute Force Attempts (Origin IP)

### 🖼 SSH Dashboard Overview

Here’s the complete dashboard showing all SSH threats:

![SSH Dashboard]("screenshots/ssh_dashboard.png")


