# Lesson 2: Triage Tactics in Defender XDR & Automated Investigation
Now that you've selected that High Severity incident, you need to triage it efficiently. In the old days, you would manually dig through logs. In Defender XDR, we use AIR (Automated Investigation and Response).

**The Concept:** The Self-Healing SOC When an alert triggers in Defender for Endpoint (MDE) or Defender for Office 365 (MDO), the system doesn't just notify you; it starts an investigation immediately.
It spins up a virtual analyst that:

* Queries the endpoint.

* Checks file hashes, running processes, and registry keys.

* Correlates this with global threat intelligence.

* Decides on a Verdict: Malicious, Suspicious, or No Threats Found.

* Proposes an Action: (e.g., Quarantine file, Stop process, Soft delete email).

The Workflow (AIR):

``` mermaid
graph TD
    A[Alert Triggered] --> B[Automated Investigation Starts]
    B --> C{Evidence Analysis}
    C -->|Malicious| D[Remediation Action Proposed]
    C -->|Clean| E[Investigation Closed]
    D --> F{Automation Level?}
    F -->|Full Automation| G[Action Executed Automatically]
    F -->|Semi - Require Approval| H[Action Pending Approval]
    H --> I[Analyst Approves/Rejects in Action Center]
    G --> J[Alert Resolved]
    I --> J

```

**Crucial** Triage Tool:** The Action Center If your automation settings are set to "Semi-Automated" (Require approval for folders/devices), the robot does the work but waits for you to push the button.

* You go to the Action Center.

* You see "Pending Actions."

* You click Approve (to fix it) or Reject (if it's a false positive).

## exam tip (SC-200): Pay close attention to Device Groups. You can configure different levels of automation for different servers.

* Full - Remediate threats automatically: The action happens instantly. You just get a report.

* Semi - Require approval for all folders: The investigation runs, but you must click approve in the Action Center.

**Note:** You cannot turn off automation entirely for MDE, but you can control the remediation level.

---

**Socratic Check**
Let's test your understanding of "Full" vs. "Semi" automation in a high-pressure scenario.

Scenario: It is 3:00 AM on a Saturday.

Server Group A (Production Database) is set to Semi - Require Approval.

Server Group B (Employee Laptops) is set to Full - Remediate threats automatically.

Ransomware hits both groups simultaneously.

Question: What happens to the ransomware on the Employee Laptops vs. the Production Database? As the analyst waking up at 3:15 AM, where is your immediate attention needed and why?
