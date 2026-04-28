# Lab 1 — Active Directory

Build a Windows domain from scratch. This is the foundation everything else runs on — identity, authentication, and access control all live here.

---

## Environment

| Machine | OS | IP | Role |
|---|---|---|---|
| DC01 | Windows Server 2022 | 192.168.100.10 | Domain Controller |
| CLIENT01 | Windows 11 | 192.168.100.20 | Domain-joined client |

Domain: `corp.local`

---

## What I built

**Domain Controller (DC01)**
- Installed AD DS role, promoted to domain controller
- Domain: `corp.local`
- Configured DNS on DC01 (required for AD to function)
- Set static IP: `192.168.100.10`

**Organizational Units**
```
corp.local/
├── IT/
├── HR/
├── Finance/
├── Users/
└── Computers/
```

**Users**
| Username | Department |
|---|---|
| j.smith | IT |
| a.khan | HR |
| m.patel | Finance |

**Groups**
| Group | Members |
|---|---|
| IT_Staff | j.smith |
| HR_Staff | a.khan |
| Finance_Staff | m.patel |

**Shared Folder Permissions**

Folder: `CompanyData\`

| Subfolder | Access |
|---|---|
| `CompanyData\HR\` | HR_Staff only |
| `CompanyData\Finance\` | Finance_Staff only |

Tested: logging in as a.khan (HR) and attempting to open Finance folder → Access Denied.

**Group Policy**
- Password must meet complexity requirements
- Screen locks after 10 minutes of inactivity
- Control Panel disabled for standard users

**Client (CLIENT01)**
- Joined to `corp.local`
- Tested domain login with j.smith, a.khan, m.patel
- Verified GPOs applied on login

---

## Break / Fix

The most important part of this lab. Each issue is logged in full in [`/troubleshooting-journal/journal.md`](../troubleshooting-journal/journal.md).

Scenarios intentionally broken and fixed:
- Broke DNS on DC01 → CLIENT01 couldn't find the domain → fixed by restoring DNS settings
- Locked out a.khan after repeated wrong passwords → unlocked via ADUC
- Misconfigured a GPO → unintended behavior on client → identified and corrected

---

## Notes

Screenshots and configuration details are documented inline as the lab progresses.
