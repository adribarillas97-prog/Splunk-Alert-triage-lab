## Hands-on Splunk investigation completed on TryHackMe.
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

<img width="697" height="416" alt="image" src="https://github.com/user-attachments/assets/8dd00ef6-cb00-491a-8510-d07e8ea98195" />

---

### 2. Attack Duration
**Question:** What was the duration of the brute force attack?
**Answer:** 5 minutes
- Started: 09:01:50 AM
- Ended: 09:06:35 AM

 <img width="1079" height="154" alt="image" src="https://github.com/user-attachments/assets/7ad0b5f8-74a9-4f35-9b17-0345c69d9c0f" />


---

### 3. Persistence — Malicious Account Creation
**Question:** What is the name of the user account created by the 
attacker for persistence?

**Answer:** system-utm

<img width="1143" height="68" alt="image" src="https://github.com/user-attachments/assets/526ab18d-8a3b-456a-9de0-a677aa6abbd9" />


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

<img width="640" height="127" alt="image" src="https://github.com/user-attachments/assets/ca337fce-302c-40ea-9a04-5913426f4f09" />


---

### 6. Lateral Movement — Workstation Identification
**Question:** What is the name of the parent process for the process that created this malicious task?
**Answer:** DEV-QA-SERVER

**Query used:**
index=win-alert host=WIN-H015 EventCode=4624 DEV-QA-SERVER

<img width="647" height="265" alt="image" src="https://github.com/user-attachments/assets/ce58d781-1dbe-458b-a7ae-dbf006c85040" />

---

### 7. Hydra Brute Force Timeline
**Question:** Which local group did the attacker enumerate during discovery? 

<img width="573" height="203" alt="image" src="https://github.com/user-attachments/assets/9049a054-bd40-45e5-bbbc-136647e030c9" />


---

### 8. What is the name of the workstation from which the Threat Actor logged into this host? 
We enter this query : index=win-alert host=WIN-H015 EventCode=4624  DEV-QA-SERVER 
Answer: DEV-QA-SERVER 

<img width="1019" height="532" alt="image" src="https://github.com/user-attachments/assets/c20d5e0b-f6b9-45ae-a769-91531b21b51d" />




## Key Takeaways
- Practiced full incident triage workflow in Splunk
- Correlated Linux and Windows logs to trace attacker activity
- Identified complete attack chain: brute force → persistence 
  → scheduled task → lateral movement
- Learned to identify attacker TTPs across multiple log sources
