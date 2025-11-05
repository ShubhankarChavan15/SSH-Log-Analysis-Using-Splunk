# SSH-Log-Analysis-Using-Splunk
Analyze SSH authentication logs with Splunk to detect failed logins, brute-force attempts, and visualize attacker origins.
# 🧠 Splunk SSH Log Analyser

A cybersecurity project focused on **SSH authentication analysis** using **Splunk dashboards**.  
This lab demonstrates how to visualize successful and failed logins, detect possible brute-force attacks, and map attacker origins geographically.

---

## 🎯 Objective

To create an **interactive Splunk dashboard** that analyzes SSH logs and provides visibility into authentication activity and potential brute-force attempts.

---

## 🧩 Lab Setup

**Tool:** Splunk Enterprise  
**Dataset:** `ssh_logs.json`, `ssh_logs_new.json`  
**Host:** kali  
**Sourcetype:** `_json`  

---

## ⚙️ Task 0: Setting up Time Range

### 🕒 Add Time Range Input
1. Click **Add Input**  
2. Select **Time** → click the pencil ✏️  
3. Set **Label:** `Time Range` and **Token:** `time_range`  
4. Add another input → select **Submit**  

💡 *Note:* For all future panels, set the time to `time_range` for consistency.

---

## 📊 Task 1: Authentication Overview Panels

**Goal:** Give a quick summary of SSH activity.

```spl
source="ssh_logs.json" host="kali" sourcetype="_json"
| stats count AS "Total SSH Events"

