# Home Lab

This is my home lab. I built it to actually understand how enterprise IT environments work — not just read about them, but set them up, break them on purpose, figure out why they broke, and fix them.

The focus is Active Directory, security monitoring, networking, Linux, and automation. Everything gets documented as I go, including the things that go wrong.

---

## Environment

This is a two-machine lab. The laptop is my workstation and mobile lab host. The OptiPlex is a dedicated 24/7 server running a bare-metal hypervisor. Together they let me build labs that mirror real enterprise networks — multiple machines on a real LAN, talking to each other across actual hardware.

### Machines

**Workstation host:** ASUS ROG Zephyrus G16 — Core Ultra 9 185H, 32 GB RAM, RTX 4060, 1 TB NVMe, Windows 11
**Hypervisor (workstation):** VMware Workstation Pro 17

**Server host:** Dell OptiPlex 5060 SFF — i5-8500 (6C), 16 GB DDR4, 512 GB NVMe + dedicated 2.5" SATA SSD for the lab
**Hypervisor (server):** Proxmox VE — bare-metal, runs 24/7

**Lab storage:** SanDisk Extreme 1 TB portable SSD (USB 3.2 Gen 2) — Phase 1 boot/storage drive for Proxmox, repurposed as backup target once the internal SSD is installed

### Virtual Machines

**On the laptop (VMware Workstation):**

| VM | Role | RAM | vCPU | Disk |
|---|---|---|---|---|
| DC01 | Windows Server 2022 — Domain Controller | 4 GB | 2 | 60 GB |
| CLIENT01 | Windows 11 — domain-joined client | 4 GB | 2 | 60 GB |
| UbuntuServer | Ubuntu Server LTS — Linux + scripting | 2 GB | 2 | 30 GB |
| Wazuh | SIEM (Lab 3) | 6 GB | 2 | 60 GB |

**On the OptiPlex (Proxmox):**

| VM | Role | RAM | vCPU | Disk |
|---|---|---|---|---|
| pfSense | Virtual firewall / router between subnets | 2 GB | 2 | 20 GB |
| AD-DC02 | Windows Server 2022 — secondary DC for two-machine domain labs | 4 GB | 2 | 60 GB |
| Kali | Attacker box for pen-test labs | 4 GB | 2 | 40 GB |
| Metasploitable / DVWA | Vulnerable target VMs for security labs | 1 GB | 1 | 20 GB |
| Pi-hole | Network-wide DNS ad-blocker for the household | 1 GB | 1 | 10 GB |

The laptop runs all four VMs simultaneously with ~16 GB free for the host. The OptiPlex runs its VMs around the clock.

### Network

```
                                  Internet
                                     │
                              [Home Router]
                                     │
                ┌────────────────────┼────────────────────┐
                │                                          │
        ┌───────────────┐                          ┌───────────────┐
        │   Laptop G16  │                          │  OptiPlex 5060 │
        │ (VMware Pro)  │◄────────LAN────────────►│   (Proxmox)    │
        └───────┬───────┘                          └───────┬───────┘
                │                                           │
       ┌────────┼────────┐                  ┌──────────────┼──────────────┐
       │        │        │                  │       │      │      │      │
     DC01   CLIENT01  Ubuntu  Wazuh      pfSense  DC02  Kali  Targets  Pi-hole
      .10     .20      .30    .40         (gw)   .10   .50    .60      .70
        NAT 192.168.100.0/24                  Server LAN 192.168.50.0/24
```

Laptop VMs talk to each other on the laptop's NAT network. OptiPlex VMs sit on their own subnet, routed through pfSense. The two machines reach each other across the home LAN — which is exactly the point. Two-machine labs (domain join, pen-test, traffic capture) need real network traffic flowing between two real hosts.

Static IPs on all servers. RDP/SSH for management. Eventually WireGuard on the OptiPlex so I can reach the lab from outside the house.

---

## Labs

### Lab 1 — Active Directory (laptop)
Build a Windows domain from scratch. Domain controller, Organizational Units, users, groups, shared folder permissions, Group Policy. Then deliberately break things — corrupt DNS, lock accounts, misconfigure GPOs — and fix them.

### Lab 2 — Security Hardening (laptop)
Enable advanced audit policy on the domain. Simulate failed logins and unauthorized access attempts. Watch Event IDs 4625 and 4740 show up in Event Viewer. Apply hardening: disable Guest account, restrict RDP, enforce password complexity.

### Lab 3 — SIEM + Detection (laptop)
Set up Wazuh and forward Windows logs to it. Simulate brute-force login attempts. Find and analyze the events in the SIEM dashboard. Write a structured incident report for each simulated attack.

### Lab 4 — Network + Linux (laptop)
Capture and read real network traffic with Wireshark — DNS queries, authentication handshakes, SMB file sharing. Set up an Ubuntu Server, harden it, and write bash scripts for common sysadmin tasks (log monitoring, health checks, automated backups).

### Lab 5 — Proxmox + bare-metal hypervisor (OptiPlex)
Install Proxmox VE on the OptiPlex. Phase 1: boot from the SanDisk Extreme to test-drive the setup. Phase 2: dedicated internal SATA SSD, fresh permanent install. Configure storage, networking, snapshots, and automated backup jobs.

### Lab 6 — Two-machine Active Directory (laptop + OptiPlex)
Stand up a secondary domain controller on the OptiPlex. Join the laptop (and laptop VMs) to the domain over the real LAN. Replicate AD between sites, test failover, run cross-machine GPO scenarios. This is the version that mirrors actual enterprise AD.

### Lab 7 — pfSense firewall + segmentation (OptiPlex)
Build a virtual pfSense firewall as the gateway for the OptiPlex VM subnet. Configure firewall rules, NAT, and inter-VLAN routing. Practice the static-routing concepts from CSN115/CSN205 on real virtual hardware instead of Packet Tracer.

### Lab 8 — Pen-test lab (laptop attacker → OptiPlex target)
Kali Linux on the laptop. Metasploitable / DVWA on the OptiPlex. Real attacks crossing the LAN. Capture the traffic with Wireshark, document the kill chain, write up findings as if for a security report.

### Lab 9 — Remote access + household services (OptiPlex, 24/7)
WireGuard VPN on the OptiPlex so I can SSH into the lab from campus or anywhere else. Pi-hole as a network-wide DNS ad-blocker for everyone in the house. Lays groundwork for a future Jellyfin / Nextcloud build.

---

## Troubleshooting Journal

Every problem I hit is logged in [`/troubleshooting-journal/`](./troubleshooting-journal/) — what broke, what the actual root cause was (not just the surface symptom), what fixed it, and what I took away from it. This is on purpose. Working through broken things is where most of the learning happens.

---

## Incident Reports

[`/incident-reports/`](./incident-reports/) has structured write-ups for the security events from Lab 3 and Lab 8. Each one covers the timeline, root cause, impact, how it was detected, and how it was resolved.

---

## Status

| | Lab | Host | Status |
|---|---|---|---|
| 🔧 | Lab 0 — Environment setup (laptop) | Laptop | In progress |
| ⬜ | Lab 1 — Active Directory | Laptop | Not started |
| ⬜ | Lab 2 — Security Hardening | Laptop | Not started |
| ⬜ | Lab 3 — SIEM + Detection | Laptop | Not started |
| ⬜ | Lab 4 — Network + Linux | Laptop | Not started |
| ⬜ | Lab 5 — Proxmox setup | OptiPlex | Not started |
| ⬜ | Lab 6 — Two-machine AD | Laptop + OptiPlex | Not started |
| ⬜ | Lab 7 — pfSense firewall | OptiPlex | Not started |
| ⬜ | Lab 8 — Pen-test lab | Laptop + OptiPlex | Not started |
| ⬜ | Lab 9 — Remote access + services | OptiPlex | Not started |

---

## Course alignment

Labs reinforce upcoming Seneca CSNC coursework:

- **CSN205** — Static Networks → pfSense routing, subnetting (Lab 7)
- **MST200** — Microsoft Server Administration → AD, GPOs (Labs 1, 2, 6)
- **OPS245** — Open Systems Server → Linux server admin (Labs 4, 9)
- **SEC220** — Intro to System Security → SIEM, pen-test (Labs 2, 3, 8)

---

## References

- [EntrytoCyber AD Home Lab Guide](https://entrytocyber.com/article-active-directory-lab.html)
- [David Varghese's Virtual Security Home Lab series](https://blog.davidvarghese.net/posts/building-home-lab-part-6/)
- [Vidal Cyber Sec's AD + SIEM Home Lab](https://vidalcybersec.com/active-directory-home-lab/)
- [Proxmox VE Documentation](https://pve.proxmox.com/pve-docs/)
- [pfSense Documentation](https://docs.netgate.com/pfsense/en/latest/)
- [Pro Git book](https://git-scm.com/book/en/v2)
- Course material: Seneca Polytechnic CSNC program — MST200, OPS245, CSN205, SEC220

---

*Last updated: April 28, 2026*
