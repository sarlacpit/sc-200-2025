# Standard Operating Procedure (SOP): Ransomware Triage & Remediation
**Version:** 1.0  
**Tools:** Microsoft Defender XDR, Microsoft Sentinel, Security Copilot  
**Author:** SOC Team

---

## 1. Purpose
This document defines the standardized workflow for detecting, containing, and eradicating ransomware threats using the Microsoft Unified Security Operations Platform. It prioritizes speed of containment followed by AI-assisted investigation.

## 2. Process Overview (Workflow)

```mermaid
graph TD
    A[Alert/Incident Triggered] -->|High Severity| B[Phase 1: Triage & Classification]
    B --> C{Ransomware Confirmed?}
    C -->|No| D[Reclassify & Close]
    C -->|Yes / Suspicious| E[Phase 2: Immediate Containment]
    E --> F[Isolate Device via MDE]
    E --> G[Suspend User via Entra ID]
    F --> H[Phase 3: AI Investigation]
    G --> H
    H -->|Copilot| I[Analyze Scripts & Summarize]
    H -->|Live Response| J[Forensics & Eradication]
    J --> K[Phase 4: Recovery & Closure]
    K --> L[Generate Incident Report]
```

---

## 3. Phase 1: Triage & Identification

**Objective:** Rapidly confirm if the incident is a true positive ransomware attack.

1.  **Navigate to the Unified Incident Queue** in `security.microsoft.com`.
2.  **Filter** for `Severity: High` and `Category: Ransomware`.
3.  **Review the Incident Graph:**
    * Check for linked entities (Multiple devices often indicate lateral movement).
4.  **Use Security Copilot:**
    * Click **"Summarize Incident"** at the top of the incident page.
    * *Key Indicators to look for:* Volume Shadow Copy deletion (`vssadmin`), High frequency file modifications, Encoded PowerShell commands.

> **Decision Point:** If the activity resembles encryption or data exfiltration, proceed immediately to **Phase 2**. Do not wait for full forensic confirmation.

---

## 4. Phase 2: Immediate Containment

**Objective:** Stop the spread. Time is critical.

### 4.1 Device Isolation (Defender for Endpoint)
1.  Go to the **Assets** tab within the Incident.
2.  Select the infected device(s).
3.  Click **Isolate Device**.
    * *Note:* Ensure "Allow Outlook/Teams" is **unchecked** for ransomware scenarios to prevent C2 communication via legitimate channels.
    * *Result:* Device is cut off from the network but remains accessible to the SOC team via Defender.

### 4.2 Identity Suspension (Entra ID / Sentinel)
1.  Identify the compromised user in the **Users** tab.
2.  **Option A (Manual):** Navigate to the user page -> Click **Suspend User**.
3.  **Option B (Sentinel Playbook):** If working in Sentinel, run the `Block-AADUser` playbook against the incident.

---

## 5. Phase 3: Investigation & Analysis

**Objective:** Understand the scope and identifying the entry point (Patient Zero).

### 5.1 Script Analysis with Copilot
If the timeline shows PowerShell or Command Line activity:
1.  Select the alert in the timeline.
2.  Highlight the script block.
3.  Click **"Analyze with Copilot"**.
4.  *Action:* If Copilot identifies the script as a downloader or C2 beacon, capture the external IP/Domain.

### 5.2 Advanced Hunting (Scope Check)
Use KQL to check if the file hash or IP has been seen elsewhere.

```kusto
// Search for the malicious file hash across all devices
DeviceFileEvents
| where SHA256 == "INSERT_MALICIOUS_HASH_HERE"
| where Timestamp > ago(7d)
| project Timestamp, DeviceName, FileName, FolderPath, ActionType
```

---

## 6. Phase 4: Eradication (Live Response)

**Objective:** Remove malicious artifacts and ensure the machine is clean.

1.  Initiate a **Live Response Session** on the isolated device.
2.  **Upload Tools:** Upload a forensic scanner or removal script if necessary.
3.  **Kill Processes:**
    * Command: `remediate process [ProcessID]` (if AIR hasn't already killed it).
4.  **Collect Evidence:**
    * Command: `getfile "C:\Temp\ransomware_note.txt"` (for analysis).
5.  **Review Action Center:**
    * Check `Action Center` -> `History` to ensure Defender Antivirus has successfully quarantined the primary threat files.

---

## 7. Phase 5: Recovery & Closure

**Objective:** Restore service and document the lesson.

1.  **Verification:** Reboot the machine (via Live Response `shutdown /r`) and verify no malicious processes restart.
2.  **Unisolation:**
    * Only perform this after the machine is confirmed clean or reimaged.
    * Select Device -> **Release from Isolation**.
3.  **Reporting:**
    * Use **Security Copilot** to "Generate Incident Report".
    * Export to PDF and attach to the ITSM ticket.
4.  **Post-Incident Review:**
    * Update **Watchlists** in Sentinel with any new IOCs (Indicators of Compromise).
    * Close Incident in Defender:
        * **Classification:** True Positive.
        * **Determination:** Ransomware.
