# Home Lab

This is my home lab. I built it to understand how enterprise IT environments work. Not just read about them, but set them up, break them on purpose, figure out why they broke, and fix them.

The focus is **Active Directory, security monitoring, networking, Linux, and automation**. Everything gets documented as I go, including the things that go wrong.

---

## Environment

**Host machine:** ASUS ROG Zephyrus G16 — Core Ultra 9 185H, 32 GB RAM, RTX 4060, 1 TB NVMe, Windows 11  
**Hypervisor:** VMware Workstation Pro 17  
**Lab storage:** SanDisk Extreme 1 TB portable SSD (USB 3.2 Gen 2) — all VMs and ISOs live here, keeps the lab portable

### Virtual Machines

| VM | Role | RAM | vCPU | Disk |
|---|---|---|---|---|
| DC01 | Windows Server 2022 — Domain Controller | 4 GB | 2 | 60 GB |
| CLIENT01 | Windows 11 — domain-joined client | 4 GB | 2 | 60 GB |
| UbuntuServer | Ubuntu Server LTS — Linux + scripting | 2 GB | 2 | 30 GB |
| Wazuh | SIEM (Lab 3) | 6 GB | 2 | 60 GB |

All four run at the same time. The host keeps ~16 GB free for itself.

### Network

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

Lab VMs talk to each other on the NAT network. I manage them from the host over the host-only network — RDP to the Windows VMs, SSH to Ubuntu. Static IPs on all servers.

---

## Labs

### Lab 1 — Active Directory
Build a Windows domain from scratch. Domain controller, Organizational Units, users, groups, shared folder permissions, Group Policy. Then deliberately break things — corrupt DNS, lock accounts, misconfigure GPOs — and fix them.

### Lab 2 — Security Hardening
Enable advanced audit policy on the domain. Simulate failed logins and unauthorized access attempts. Watch Event IDs 4625 and 4740 show up in Event Viewer. Apply hardening: disable Guest account, restrict RDP, enforce password complexity.

### Lab 3 — SIEM + Detection
Set up Wazuh and forward Windows logs to it. Simulate brute-force login attempts. Find and analyze the events in the SIEM dashboard. Write a structured incident report for each simulated attack.

### Lab 4 — Network + Linux
Capture and read real network traffic with Wireshark — DNS queries, authentication handshakes, SMB file sharing. Set up an Ubuntu Server, harden it, and write bash scripts for common sysadmin tasks (log monitoring, health checks, automated backups).

---

## Troubleshooting Journal

Every problem I hit is logged in [`/troubleshooting-journal/`](./troubleshooting-journal/) — what broke, what the actual root cause was (not just the surface symptom), what fixed it, and what I took away from it. This is on purpose. Working through broken things is where most of the learning happens.

---

## Incident Reports

[`/incident-reports/`](./incident-reports/) has structured write-ups for the security events from Lab 3. Each one covers the timeline, root cause, impact, how it was detected, and how it was resolved.

---

## Status

| | Lab | Status |
|---|---|---|
| ✅ | Lab 0 — Environment setup | Complete |
| ⬜ | Lab 1 — Active Directory | Not started |
| ⬜ | Lab 2 — Security Hardening | Not started |
| ⬜ | Lab 3 — SIEM + Detection | Not started |
| ⬜ | Lab 4 — Network + Linux | Not started |

---

## References

- [EntrytoCyber AD Home Lab Guide](https://entrytocyber.com/article-active-directory-lab.html)
- [David Varghese's Virtual Security Home Lab series](https://blog.davidvarghese.net/posts/building-home-lab-part-6/)
- [Vidal Cyber Sec's AD + SIEM Home Lab](https://vidalcybersec.com/active-directory-home-lab/)
- [Pro Git book](https://git-scm.com/book/en/v2)
- Course material: Seneca Polytechnic CSNC program — MST100, MST200, OPS145, OPS245, CSn15, CSN205, SEC220
