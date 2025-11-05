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



