Hands-on Splunk investigation completed on TryHackMe.
Analyzed a real attack scenario involving brute force,
persistence, lateral movement, and malicious process execution.

## Tools Used
- Splunk Enterprise (SIEM)
- Linux authentication logs
- Windows event logs
- TryHackMe AttackBox

## Investigation Walkthrough



### 1. Brute Force Detection — Linux
**Question:** How many failed login attempts were made on user john.smith?
**Answer:** 500

**Query used:**
index="linux-alert" Failed password "John.smith"
| stats count as Failed_Attempts
![image alt] (https://github.com/adribarillas97-prog/Splunk-Alert-triage-lab/blob/875b926ccf553897a8141a7954f15c80f0749db0/GetImage.png)
---

### 2. Attack Duration
**Question:** What was the duration of the brute force attack?
**Answer:** 5 minutes
- Started: 09:01:50 AM
- Ended: 09:06:35 AM

---

### 3. Persistence — Malicious Account Creation
**Question:** What is the name of the user account created by the 
attacker for persistence?
**Answer:** system-utm

---

### 4. Malicious Scheduled Task
**Question:** What is the ProcessId of the process that created 
this malicious task?
**Answer:** 5816

**Query used:**
index="win-alert" EventCode=4698 AssessmentTaskOne
| table _time EventCode user_name host Task_Name Message

---

### 5. Parent Process Identification
**Question:** What is the name of the parent process for the 
process that created this malicious task?
**Answer:** cmd.exe
**Parent host:** WIN-H015 net.exe

---

### 6. Lateral Movement — Workstation Identification
**Question:** What is the name of the workstation from which the 
Threat Actor logged into this host?
**Answer:** DEV-QA-SERVER

**Query used:**
index=win-alert host=WIN-H015 EventCode=4624 DEV-QA-SERVER

---

### 7. Hydra Brute Force Timeline
**Question:** What time did the brute force activity using 
Hydra begin?
**Answer:** 2025-09-14 21:20:27

---

## Key Takeaways
- Practiced full incident triage workflow in Splunk
- Correlated Linux and Windows logs to trace attacker activity
- Identified complete attack chain: brute force → persistence 
  → scheduled task → lateral movement
- Learned to identify attacker TTPs across multiple log sources
