# Lesson 4: Defender XDR Remediation Actions
In Defender, remediation is "surgical." You are usually interacting directly with the endpoint, the email, or the identity.

# The Big Three Actions:

**Isolate Device (MDE):**

* What it does: It cuts the network connection of the infected laptop. The device can only talk to the Defender portal. It cannot talk to the internet, the file server, or the attacker's C2 (Command & Control) server.

* When to use: Immediate containment of ransomware or active hackers.

* User Experience: The user sees a "Network Disconnected" message.

**Stop and Quarantine File:**

* What it does: Stops the process and moves the file to a secure, encrypted folder.
* Soft Delete (Email): In Defender for Office (MDO), this removes a phishing email from a user's inbox after it was delivered (ZAP - Zero-hour Auto Purge).

**Live Response:**

* What it does: This opens a remote shell (like a command line) to the device directly in your browser.
* Power: You can upload scripts, download suspicious files for analysis, and run forensic commands.

_exam tip (SC-200): Permissions To use Live Response, you often need specific permissions enabled in the Advanced Features settings._

**Standard Live Response:** Can run basic commands.

**Advanced Live Response:** Can upload scripts/files to the machine (Higher risk, requires distinct permission).

---
**Socratic Check**
Scenario: You have isolated an infected laptop ("Device X"). The ransomware is contained, but you need to get a specific malware sample off that laptop to analyze it in a sandbox.
The Problem: The device is isolated—it has no network access.
Question: How do you retrieve that file?

* Un-isolate the device briefly (risking the network).
* Use Live Response (which works even during isolation).
* Ask the user to put it on a USB stick.

(Pick the safest and correct method.)


Live Response is designed specifically to work through the secure tunnel established by Defender for Endpoint, even when the device is "Isolated" from the rest of the network. This allows you to perform forensics without exposing your organization to the threat.
