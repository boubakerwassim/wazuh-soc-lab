# 05 – Failed Windows logons (Event ID 4625) — in progress

## Goal

Detect repeated failed logons on the Windows 11 endpoint, a typical first indicator of password guessing or brute force (MITRE ATT&CK **T1110 – Brute Force**).

## Step 1 – Generate and verify on Windows ✅

Failed logons were generated on my own Windows 11 laptop (wrong password). Event Viewer → Windows Logs → Security, filtered on Event ID 4625:

- 5 events "An account failed to log on" (Event ID 4625, task category *Logon*, keyword *Audit Failure*)
- Logon type **2** = interactive logon (local sign-in)

![Event ID 4625 in Event Viewer](../screenshots/07-failed-logons-4625-event-viewer.png)

## Step 2 – Find the alerts in Wazuh ⏳

- Dashboard → Threat Hunting: `data.win.system.eventID: 4625`
- Server: `sudo grep '"eventID":"4625"' /var/ossec/logs/alerts/alerts.json | tail -3`

To document: rule ID, level, description, target account, logon type, failure reason.

## Step 3 – Incident report ⏳

Write a short report with [incident-reports/TEMPLATE.md](../incident-reports/TEMPLATE.md): timeline, true/false positive, recommendation.
