# Lab 2 — Security Hardening

Enable logging, simulate real authentication events, and apply security hardening to the domain built in Lab 1.

---

## Environment

Builds directly on Lab 1. Same domain (`corp.local`), same machines (DC01 at `192.168.100.10`, CLIENT01 at `192.168.100.20`).

---

## Audit Policy

Enabled advanced audit policy on DC01 via Group Policy:

| Policy | Setting |
|---|---|
| Logon Events | Success and Failure |
| Account Lockout | Success and Failure |
| Object Access | Success and Failure |

Logs visible in: **Event Viewer → Windows Logs → Security**

Key Event IDs:
| Event ID | Meaning |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4740 | Account locked out |
| 4663 | Object (file/folder) access attempt |

---

## Attack Simulations

**Failed Login / Account Lockout**

On CLIENT01, attempted login with wrong password for `a.khan` 10 times in a row. Observed:
- Event ID `4625` repeating in Security log on DC01
- Event ID `4740` appearing once account locked
- Account confirmed locked in Active Directory Users and Computers

**Unauthorized Folder Access**

Logged into CLIENT01 as `a.khan` (HR). Attempted to open `CompanyData\Finance\`. Observed:
- Access denied
- Event ID `4663` logged (access attempt on Finance folder)

---

## Hardening Applied

| Item | Before | After |
|---|---|---|
| Guest account | Enabled | Disabled |
| RDP access | Open to all | Restricted to IT_Staff group only |
| Password policy | Default | Min 12 chars, complexity required, 60-day expiry |
| Account lockout | None | 5 failed attempts → 30-min lockout |

---

## Notes

Observations and screenshots logged inline as the lab progresses. Issues are in [`/troubleshooting-journal/journal.md`](../troubleshooting-journal/journal.md).
