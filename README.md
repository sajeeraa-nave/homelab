# Home Lab — Enterprise Systems & Cybersecurity

Portfolio project focused on enterprise built to develop and demonstrate skills in identity management, security monitoring, detection and response, networking, Linux administration, and automation. 

Every lab is broken intentionally, debugged systematically, and documented thoroughly.

---

## What This Project Demonstrates:

- Building and managing an enterprise-style domain environment
- Detecting and responding to simulated attacks
- Analyzing logs across Windows and Linux systems
- Automating monitoring and system tasks

### Skills Demonstrated

Active Directory · DNS · Group Policy · Windows event logging · Security hardening · SIEM (Wazuh) · Attack simulation · Incident response · Wireshark · Packet analysis · Ubuntu Server · Bash scripting · PowerShell · Network design · Virtualization (VMware) · Technical documentation

---

## Environment

| Component | Spec |
|---|---|
| **Host machine** | ASUS ROG Zephyrus G16 — Core Ultra 9 185H, 32 GB RAM, RTX 4060, 1 TB NVMe |
| **Hypervisor** | VMware Workstation Pro 17 |
| **Lab storage** | SanDisk Extreme Portable SSD 1 TB (USB 3.2 Gen 2) |
| **Host OS** | Windows 11 |

### Virtual Machines

| VM | Role | RAM | vCPU | Disk |
|---|---|---|---|---|
| DC01 | Windows Server 2022 — Domain Controller | 4 GB | 2 | 60 GB |
| CLIENT01 | Windows 11 — Domain-joined client | 4 GB | 2 | 60 GB |
| UbuntuServer | Ubuntu Server LTS — Linux + automation | 2 GB | 2 | 30 GB |
| Wazuh | SIEM (Lab 3) | 6 GB | 2 | 60 GB |

All four run concurrently. Host retains ~16 GB for OS and tooling.

### Network Design

```
                    ┌──────────────────────────┐
                    │    Host: G16 (Win 11)    │
                    └─────────────┬────────────┘
                                  │
             ┌────────────────────┼────────────────────┐
             │                    │                    │
       NAT (lab)             Host-only            Internet
    192.168.100.0/24      192.168.200.0/24
             │
  ┌──────────┼──────────┬──────────┐
  │          │          │          │
DC01     CLIENT01   UbuntuSrv    Wazuh
.10        .20         .30        .40
```

Lab network handles inter-VM traffic. Host-only handles management (RDP from host, SSH to Ubuntu). Static IPs on all servers.

---

## Repository Structure

```
homelab/
├── README.md                     ← you are here
├── lab1-active-directory/        ← domain controller, OUs, GPOs, break/fix
├── lab2-security-hardening/      ← audit policy, event logs, lockdown
├── lab3-siem-detection/          ← Wazuh, attack simulation, incident reports
├── lab4-network-linux/           ← Wireshark captures, Ubuntu Server, automation scripts
├── incident-reports/             ← structured reports: summary, timeline, root cause, fix
├── scripts/                      ← bash and PowerShell automation
└── troubleshooting-journal/      ← every problem encountered, root cause, fix, lesson
```

---

## Labs

### Lab 1 — Active Directory
Build a working Windows domain from scratch. Create the domain controller (`DC01`), join a client machine, configure Organizational Units, users, groups, shared folder permissions, and Group Policy. Then break things intentionally — misconfigure DNS, lock accounts, corrupt GPOs — and fix them, documenting every step.

**Skills:** AD DS, DNS, DHCP, GPO, NTFS permissions, account management, domain troubleshooting

#### Outcome:


---

### Lab 2 — Security Hardening
Enable advanced audit policy on the domain. Simulate authentication events (failed logins, account lockouts, unauthorized folder access). Observe Event IDs 4625 and 4740 in Event Viewer. Apply baseline hardening: disable Guest, restrict RDP, enforce password policy.

**Skills:** Windows audit policy, Event Viewer, security baselines, authentication event analysis

#### Outcome:

---

### Lab 3 — SIEM + Attack → Detect → Respond
Install Wazuh SIEM. Forward Windows logs. Simulate brute-force login attempts and unauthorized access. Detect events in the SIEM dashboard. Write a structured incident report covering timeline, root cause, impact, detection method, resolution, and prevention.

**Skills:** Wazuh, log forwarding, alert correlation, SOC workflow, incident documentation

#### Outcome:

---

### Lab 4 — Network Analysis + Linux + Automation
Capture and analyze real domain traffic with Wireshark (DNS queries, TCP handshakes, SMB file share traffic, authentication flows). Build an Ubuntu Server — harden it, enable logging, write automation scripts for failed login detection, log backup, and system health checks.

**Skills:** Wireshark, packet analysis, Ubuntu Server, ufw, systemctl, bash scripting

#### Outcome:

---

## Troubleshooting Journal

Every problem encountered across all labs is logged in [`/troubleshooting-journal/`](./troubleshooting-journal/) using a consistent format:

- **Problem** — what went wrong
- **Symptoms** — observable behavior, verbatim error messages
- **Root cause** — one level deeper than the surface symptom
- **Fix** — exact steps that resolved it
- **What I learned** — the concept this reinforced
- **Prevention** — how to stop it happening again

This is deliberate. Breaking things and fixing them with understanding is the point.

---

## Incident Reports

Structured reports in [`/incident-reports/`](./incident-reports/) for simulated security events from Lab 3. Format mirrors real SOC documentation: incident summary, timeline, root cause, impact assessment, detection method, resolution steps, and prevention recommendations.

---

## Scripts

Automation scripts in [`/scripts/`](./scripts/):

| Script | Purpose |
|---|---|
| `failed_logins.sh` | Count failed SSH login attempts from auth.log |
| `suspicious_logins.sh` | Flag repeated failures for the same user |
| `log_backup.sh` | Copy auth.log to timestamped backup |
| `health_check.sh` | Report disk usage, memory, and running services |

---

## Status

| Lab | Focus | Status |
|---|---|---|
| Lab 0 — Setup | Environment, VMs, network design | In progress |
| Lab 1 — Active Directory | Domain controller, OUs, GPOs, break/fix | Queued |
| Lab 2 — Security Hardening | Audit policy, lockdown, event analysis | Queued |
| Lab 3 — SIEM + Detection | Wazuh, attack simulation, incident reports | Queued |
| Lab 4 — Network + Linux | Wireshark, Ubuntu Server, automation | Queued |

Target: first complete version pushed by end of **Summer 2026**. 
Mature portfolio version ready **November 2026**.

---

## References

- Active Directory setup: [EntrytoCyber AD Home Lab Guide](https://entrytocyber.com/article-active-directory-lab.html), [David Varghese's Virtual Security Home Lab series](https://blog.davidvarghese.net/posts/building-home-lab-part-6/)
- Documentation practices: [Make a README](https://www.makeareadme.com/), [freeCodeCamp GitHub README guide](https://www.freecodecamp.org/news/how-to-write-a-good-readme-file/)
- Git/GitHub workflow: [freeCodeCamp Git handbook](https://www.freecodecamp.org/news/learn-how-to-use-git-and-github-a-beginner-friendly-handbook), [Pro Git book](https://git-scm.com/book/en/v2)
- Course material: Seneca Polytechnic CSNC program — MST200, OPS245, CSN205, SEC220
- AI assistance: Claude (Anthropic) for scaffolding, brainstorming, and documentation review
