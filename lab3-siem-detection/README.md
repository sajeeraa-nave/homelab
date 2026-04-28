# Lab 3 — SIEM + Detection

Set up a SIEM, forward logs from the domain, simulate attacks, detect them, and write proper incident reports.

---

## Environment

| Machine | OS | IP | Role |
|---|---|---|---|
| DC01 | Windows Server 2022 | 192.168.100.10 | Log source |
| CLIENT01 | Windows 11 | 192.168.100.20 | Log source / attack origin |
| Wazuh | Ubuntu (Wazuh OVA) | 192.168.100.40 | SIEM |

---

## SIEM Setup

Installed Wazuh on a dedicated VM. Deployed Wazuh agents on DC01 and CLIENT01 to forward Windows Security logs to the SIEM dashboard.

Confirmed logs flowing before running any simulations.

---

## Attack Scenarios

**Scenario 1 — Brute Force Login**

On CLIENT01, entered wrong password for `j.smith` 15 times consecutively.

What to detect:
- Repeated Event ID `4625` (failed logon) from the same source
- Event ID `4740` (account locked) shortly after
- Wazuh alert: authentication failure threshold exceeded

**Scenario 2 — Suspicious Login Behavior**

Logged in, logged out, and logged back in as `m.patel` repeatedly in a short window to generate anomalous session activity.

What to detect:
- Rapid succession of `4624` (logon) and `4634` (logoff) events
- Unusual session pattern for that account

**Scenario 3 — Unauthorized Access Attempt**

Logged into CLIENT01 as `a.khan` (HR). Repeatedly attempted to access `CompanyData\Finance\`.

What to detect:
- Repeated Event ID `4663` on the Finance folder
- Access denied pattern for a non-Finance account

---

## Detection Workflow

For each scenario:
1. Search events in Wazuh by Event ID and timeframe
2. Identify the affected user and source machine
3. Build a timeline of what happened
4. Determine why the behavior looks suspicious
5. Document findings in an incident report

---

## Incident Reports

Each simulated attack has a corresponding write-up in [`/incident-reports/`](../incident-reports/). Format:

- Incident summary
- Timeline
- Root cause
- Impact
- Detection method
- Resolution
- Prevention

---

## Notes

Logs, screenshots, and Wazuh alert details documented inline. Troubleshooting issues in [`/troubleshooting-journal/journal.md`](../troubleshooting-journal/journal.md).
