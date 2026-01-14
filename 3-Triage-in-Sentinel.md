# Lesson 3: Triage Tactics in Microsoft Sentinel

We’ve covered Defender XDR (in L2 which is deep and specific). Now let’s move to Microsoft Sentinel, the cloud-native SIEM/SOAR.

**The Concept:** The Eagle-Eye View Defender sees Microsoft stuff really well. Sentinel sees everything (Firewalls, Linux, AWS, legacy apps).
When triaging in Sentinel, you rely heavily on two features to cut through the noise: **Entities and Watchlists.**

* **Entities:** The "Nouns" of the Crime
When data comes into Sentinel (e.g., a raw syslog message), it's just text. Sentinel needs to know what matters. _We map fields to Entities_:

  * Account: (Usernames, SIDs)

  * Host: (Server names)

  * IP: (Source/Dest Address)

  * Why? If you map Entities correctly in your Analytics Rules, Sentinel builds an Entity Page for that user. You can click "User Bob" and see a timeline of everything Bob has done across all data sources. This is instant context for triage.

* **Watchlists:** Your Secret Weapon
A Watchlist is a custom list of data you upload to Sentinel (CSV file).

  * High Value Examples: A list of VIP Users (CEO, CFO), Critical Servers, or Terminated Employees.

  * Noise Reduction Examples: A list of Trusted IP addresses (Scanners, VPN endpoints).

  * The Workflow: You write query logic (KQL) that says: "If an alert fires, check if the User is in the 'VIP_Users' Watchlist. If YES, bump Severity to HIGH."

**exam tip (SC-200): Watchlists are static or dynamic tables. You access them in KQL using the _GetWatchlist('Name') function.
**


Scenario: A question asks how to prioritize alerts for the C-Suite executives without writing a separate rule for each person.

Answer: Create a Watchlist of executives and modify the Analytics Rule to increase severity if the user matches the list.

Socratic Check
Let's test your ability to apply Watchlists to a Triage scenario.

Scenario: Your SOC is getting flooded with "Brute Force Login" alerts (Event ID 4625). 90% of them are coming from your company's authorized vulnerability scanner (IP: 192.168.1.50) which runs daily.

Question: How do you use a Watchlist to stop these specific alerts from cluttering your Incident Queue, while ensuring you don't miss a real brute force attack from a hacker?

(Tell me the strategy.)

---

By creating a "Trusted_Scanners" Watchlist and adding a line to your KQL query like 

```| where IPAddress !in (_GetWatchlist('Trusted_Scanners')), ```

You filter out the noise before it becomes an alert. This is a master-level move for reducing False Positives.
