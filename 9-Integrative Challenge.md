# Lesson 9: Integrative Challenge - "The Midnight Oil"

* The Scenario: It is Tuesday, 11:42 PM. You are the on-call SOC Analyst. Incident #3041 appears in your Defender Unified Queue.

* Severity: High.

* Title: "Multi-stage incident involving Lateral Movement and Ransomware behavior."

* Entities: User jdoe, Device Finance-Laptop-01, Device FileServer-HR.

The Alert Timeline:

* 11:30 PM: jdoe clicks a link in an email (Phishing).

* 11:35 PM: Suspicious PowerShell script runs on Finance-Laptop-01 (Download Cradle).

* 11:38 PM: jdoe account performs "Overpass-the-Hash" attack against FileServer-HR.

* 11:40 PM: Mass file modification detected on FileServer-HR (Ransomware encryption start).

Your Mission: You must stop the bleeding and document the event.

* The Process Flow (Your Moves):

Code snippet
``` mermaid
graph TD
    A[Incident #3041 Detected] --> B{Step 1: Triage}
    B -->|Check Severity| C[High Severity Confirmed]
    B -->|Check Copilot Summary| D[Identify Lateral Movement Path]
    D --> E{Step 2: Containment}
    E -->|Action 1| F[Isolate Finance-Laptop-01]
    E -->|Action 2| G[Isolate FileServer-HR]
    E -->|Action 3| H[Suspend User jdoe in Entra ID]
    G --> I{Step 3: Remediation}
    I -->|Live Response| J[Connect to FileServer-HR]
    J -->|Identify Process| K[Kill Ransomware Process]
    K --> L[Collect Forensic Sample]
    L --> M{Step 4: Closure}
    M -->|Classify| N[True Positive]
    M -->|Report| O[Generate Copilot Report]
```

The Challenge Question: You have isolated both devices. The attack is paused. You are now in Live Response on the FileServer-HR. You see a strange file encryptor.exe in the C:\Temp folder.
You want to know if this specific file hash has been seen in your organization before today (perhaps lying dormant).

* Which tool do you use to find this instantly?
* A) Ask Security Copilot to "Analyze the file".
* B) Run a KQL query in Advanced Hunting for the FileHash.
* C) Check the "Action Center" history.

---

The best answer for historical search is B) Run a KQL query in Advanced Hunting.

Here is the distinction:

* Copilot "Analyze" (Option A): This explains the capability of the file (e.g., "This file encrypts data"). It analyzes the code itself.

* Advanced Hunting (Option B): This queries the database of events.
  If you want to know "Has this file hash been seen on any machine in the last 30 days?", you need to ask the database (DeviceFileEvents | where SHA256 == "...").
