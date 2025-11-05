# SSH-Log-Analysis-Using-Splunk
Analyze SSH authentication logs with Splunk to detect failed logins, brute-force attempts, and visualize attacker origins.
# 🧠 SSH Log Analysis using Splunk

A cybersecurity project focused on analyzing **SSH authentication logs** to detect suspicious login activity and brute-force attempts using **Splunk**.  
This hands-on lab demonstrates how to ingest logs, create visualizations, and configure alerts for real-world SOC monitoring.

---

## 🎯 Objective

The goal of this project is to analyze SSH authentication logs to detect:

- ✅ Successful logins (who connected, from where)  
- 🚫 Failed login attempts (possible brute-force or password spraying)  
- 🔁 Multiple failed authentication attempts (indicators of brute-force)  
- ⚠️ Connections without authentication (potential scanning or incomplete sessions)  

By completing this project, you’ll learn how to use Splunk to **ingest SSH logs**, **run SPL queries**, **create dashboards**, and **generate alerts** for suspicious activity.

---

## 🧩 Lab Setup and Prerequisites

**Requirements:**
- Splunk Enterprise (or Free) instance  
- SSH log file: `ssh_log.json`  
- Basic familiarity with SPL (Search Processing Language)

### ⚙️ Preparation

1. Log in to your Splunk instance → `Apps > Search & Reporting`  
2. Click **Add Data → Upload**  
3. Select the file: `ssh_log.json`  
4. Set **sourcetype:** `_json` (so Splunk automatically extracts fields)  
5. Index it under a new index → e.g., `ssh_logs`  
6. Review and click **Start Searching**

---

## 🧾 Step-by-Step Guide

### 🧱 Task 1: Ingest and Parse Logs

Upload `ssh_log.json` into Splunk and ensure the following fields are extracted:

- `event_type` → *(Successful SSH Login, Failed SSH Login, Multiple Failed Authentication Attempts, Connection Without Authentication)*  
- `auth_success` → *(true / false / null)*  
- `auth_attempts`  
- `id.orig_h` → *(source IP)*  
- `id.resp_h` → *(destination host)*  

✅ **Validation Search**
```spl
index=ssh_logs | stats count by event_type
🚫 Task 2: Analyze Failed Login Attempts
Identify all failed SSH login attempts:

spl
Copy code
index=ssh_logs event_type="Failed SSH Login"
| stats count by id.orig_h
Highlight the top 10 source IPs generating failed logins:

spl
Copy code
index=ssh_logs event_type="Failed SSH Login"
| top id.orig_h limit=10
🧮 Visualization:
Create a bar chart showing failed login attempts per source IP.

🔁 Task 3: Detect Multiple Failed Authentication Attempts (Brute Force)
Detect brute-force behavior:

spl
Copy code
index=ssh_logs event_type="Multiple Failed Authentication Attempts"
| stats count by id.orig_h, id.resp_h
Set up a Splunk alert:

Condition: Any IP with >5 failed logins in 10 minutes

Trigger: Once per result

Action: Send email or create incident in dashboard

✅ Task 4: Track Successful Logins
Search for successful logins:

spl
Copy code
index=ssh_logs event_type="Successful SSH Login"
| stats count by id.orig_h, id.resp_h
Compare successful logins against prior failed attempts to detect potentially compromised accounts.

📊 Visualization:
Bar chart showing Top Source IPs for successful logins.

⚠️ Task 5: Spot Suspicious Connections Without Authentication
Identify unauthenticated SSH connections:

spl
Copy code
index=ssh_logs event_type="Connection Without Authentication"
| stats count by id.orig_h
Monitor such events over time:

spl
Copy code
index=ssh_logs event_type="Connection Without Authentication"
| timechart count by id.orig_h


