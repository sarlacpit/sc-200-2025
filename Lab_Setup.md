# A Step-by-Step Lab Environment

Here is how you put it all together to create your SC-200 lab:

* Sign up for the Microsoft 365 Developer Program to get your free M365 E5 tenant.

* Sign up for an Azure Free Account to get your Azure subscription with free credits.

In your Azure subscription: 

* Create a new Log Analytics Workspace.

* In that workspace: Enable Microsoft Sentinel (this will activate its 31-day free trial).

In Microsoft Sentinel: 
* Go to "Data connectors" and connect your M365 E5 tenant. The connector for "Microsoft 365 Defender" is crucial and ingesting its alerts is free.

In your Azure subscription: 
* Go to Microsoft Defender for Cloud and enable its features (you can use your free credits for this).

Once you complete these steps, you will have a fully functional environment where your M365 E5 tenant generates security alerts, and Microsoft Sentinel collects and displays them, allowing you to practice hunting, investigation, and remediation just as you would on the exam.
