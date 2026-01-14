## Lesson 1: The Architecture of an Incident

Before we can triage (prioritise) or remediate (fix), we must understand what we are looking at. In the past, you had a queue for endpoints (MDE), a queue for identity (MDI), and a queue for cloud apps (MDA).
Today, Microsoft uses the Unified Security Operations Platform.

**The Concept:** Alerts vs. Incidents Think of an Alert as a single piece of evidence—a witness stating, "I saw someone break a window." Think of an Incident as the entire case file.
Microsoft's correlation engine (often powered by the "fusion" rule engine in Sentinel or XDR correlation) looks at thousands of alerts and groups them together if they share the same Entities (Users, Devices, IPs, File Hashes).

**The Workflow:** If you treat every alert as a separate task, you will drown (Alert Fatigue). Your job as a Master Analyst is to work at the Incident level.
Here is how the flow works in the Microsoft ecosystem:


``` mermaid 
graph TD
    A[Signal/Telemetry] -->|Detection Rule| B(Alert)
    B -->|Correlation Engine| C{Related?}
    C -- Yes --> D[Existing Incident]
    C -- No --> E[New Incident]
    D --> F[Unified Incident Queue]
    E --> F
    F --> G[Analyst Triage]
```

## Key Triage Fields: When you open an incident in the Defender Portal (security.microsoft.com), you are immediately responsible for three fields:

* **Severity:** High, Medium, Low, Informational. (Driven by the most severe alert inside).
* **Status: New,** Active, Resolved (or Closed).
* **Classification:** True Positive (Real threat), False Positive (Not a threat), or Benign Positive (Real action, but authorized, e.g., a pen-test).

## Exam tip (SC-200): You might see a question asking where you should perform triage if you have both Defender XDR and Sentinel.

* Modern Answer: The Unified Experience in the Defender Portal (if configured) allows you to see Sentinel incidents inside the Defender portal.
* Classic Answer: Incidents sync bi-directionally. Closing an incident in Defender XDR closes it in Sentinel, and vice versa.


---

## Socratic Check
I want to test your intuition on "Correlation."
Imagine you see three alerts in your queue arriving within 5 minutes of each other:

* "Suspicious PowerShell Script" executed on Device A.

* "Anomalous Login" by User Bob.

* "Malware detected" on Device B.

If User Bob was the one logged into Device A and Device B at the time of these alerts, how would Microsoft Defender/Sentinel likely present this to you, and why does that matter for your triage process?


exam tip (SC-200): This is called Correlation. In Sentinel, this is often handled by **Fusion** rules or **Analytics Rules** that group by Entity. In Defender XDR, it is automated logic. Incidents reduce Triage Time.


---
