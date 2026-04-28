# Lab 4 — Network Analysis + Linux

Two parts: capture and read real network traffic with Wireshark, then set up an Ubuntu Server and write automation scripts.

---

## Environment

| Machine | OS | IP | Role |
|---|---|---|---|
| DC01 | Windows Server 2022 | 192.168.100.10 | Traffic source |
| CLIENT01 | Windows 11 | 192.168.100.20 | Traffic source |
| UbuntuServer | Ubuntu Server LTS | 192.168.100.30 | Linux + scripting |

Wireshark runs on the host machine, capturing traffic on the lab NAT network (`192.168.100.0/24`).

---

## Part A — Network Analysis (Wireshark)

**Captures**

| Traffic type | What to find |
|---|---|
| Domain login | Kerberos authentication exchange between CLIENT01 and DC01 |
| DNS query | CLIENT01 querying DC01 for `corp.local` records — request and response |
| File share | SMB traffic when CLIENT01 accesses `CompanyData\` on DC01 |

**Break / Capture / Fix**

Broke DNS on DC01 → captured CLIENT01 failing to resolve `corp.local` → fixed → captured successful resolution again. Side-by-side comparison of failure vs success traffic.

Each capture is saved as a `.pcapng` file and annotated with what's visible and why it matters.

---

## Part B — Linux + Automation

**Ubuntu Server Setup**

- Updated packages
- Enabled `ufw` firewall, allowed SSH only
- Enabled system logging
- SSH key authentication configured (password auth disabled)

**Automation Scripts**

Scripts live in [`/scripts/`](../scripts/).

| Script | What it does |
|---|---|
| `failed_logins.sh` | Searches `auth.log` for failed SSH attempts, prints a count |
| `suspicious_logins.sh` | Flags any user with more than 5 failed attempts in the log |
| `log_backup.sh` | Copies `auth.log` to a timestamped backup in `/var/log/backups/` |
| `health_check.sh` | Reports disk usage, memory usage, and status of running services |

Each script is commented to explain what it does and why.

---

## Notes

Wireshark captures, screenshots, and script output documented inline. Issues in [`/troubleshooting-journal/journal.md`](../troubleshooting-journal/journal.md).
