#Lesson 5: Sentinel Remediation (Automation vs. Playbooks)
In Sentinel, remediation is broader. Since Sentinel connects to everything, it can fix things everywhere (e.g., reset an AD password, block an IP on a Cisco firewall, post a message in Teams).

We use two main tools: **Automation Rules and Playbooks.**

## 1. Automation Rules (The "Trigger" & Simple Actions)
Think of these as the "If This, Then That" logic inside Sentinel.

* Function: When an incident is created, do X.

* Capabilities:

  * Assign the incident to a specific owner.

  * Change severity.

  * Add tags.

  * Run a Playbook.

  * _New: Suppress alerts (close false positives automatically)._
 
  
## 2. Playbooks (The "Heavy Lifters" - Logic Apps)
Playbooks are powered by **Azure Logic Apps**. This is a visual designer where you build workflows.

* Function: Connect to external systems.

* Capabilities:

  *  "Block IP on Palo Alto Firewall."

  *  "Reset User Password in Entra ID."

  *  "Send an approval card to the Manager in Microsoft Teams."

  *  "Open a ticket in ServiceNow."

**The Relationship:** You rarely run a Playbook manually for every alert. instead: Incident triggers -> Automation Rule evaluates -> Playbook executes.

_exam tip (SC-200): Permissions To run a Playbook, Sentinel needs permissions (Managed Identity) to access the target resource._

---
## If the Playbook resets a password, the Logic App resource needs "User Administrator" or "Global Administrator" roles in Entra ID. It's not your permission that matters, it's the Playbook's permission.
---

## Socratic Check
Let's verify the distinction between Rules and Playbooks.
Scenario: You want to implement a process: "When a High Severity incident is created, immediately assign it to the 'Senior Analyst Team' and tag it as 'Urgent'."
Question: Do you need to build a Logic App Playbook for this, or can you do it purely with an Automation Rule? Why?

(Reply with your choice and reasoning.)

--- 
You only need an Automation Rule.
Changing the owner, severity, status, or tags are internal operations within the Sentinel database. Automation Rules are optimized for this—they are faster and cheaper than spinning up a Logic App.



---

## Mini-Review: Phase 2
Defender Remediation: "Isolate Device" is your panic button. "Live Response" works even when isolated.

Sentinel Remediation:

Automation Rules: Manage the incident (Tag, Assign, Close).

Playbooks (Logic Apps): Talk to the outside world (Firewalls, Service Desk, AD).

Permissions: Playbooks need their own identity (Managed Identity) with permissions to perform the action.

---


