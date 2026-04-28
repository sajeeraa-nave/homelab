# Lab 0 — Environment Setup

Foundation for the entire lab. Documents the host machine, hypervisor, storage strategy, and network design that every subsequent lab builds on.

---

## Goal

A stable, portable virtualization environment capable of running a small enterprise network — domain controller, client, Linux server, and SIEM — simultaneously, with clean baselines that can be rebuilt quickly.

---

## Host Machine

| Component | Spec |
|---|---|
| Model | ASUS ROG Zephyrus G16 OLED |
| CPU | Intel Core Ultra 9 185H |
| RAM | 32 GB |
| Internal Storage | 1 TB NVMe SSD |
| GPU | NVIDIA GeForce RTX 4060 |
| OS | Windows 11 |

The Core Ultra 9 supports Intel VT-x and VT-d — required for running 64-bit guest VMs efficiently. Both must be enabled in BIOS before VMware will work.

---

## External Lab Storage

| Component | Spec |
|---|---|
| Model | SanDisk Extreme Portable SSD |
| Capacity | 1 TB |
| Interface | USB 3.2 Gen 2 (~1050 MB/s) |
| Format | exFAT |

All VMs, ISOs, and baseline clones live on the external SSD. This keeps the host's internal drive free and makes the entire lab portable.

### Storage Layout

```
SanDisk-1TB/
├── VMs/                    # Active VMs
│   ├── DC01/
│   ├── CLIENT01/
│   ├── UbuntuServer/
│   └── Wazuh/
├── ISOs/                   # Installation media
│   ├── WindowsServer2022.iso
│   ├── Windows11.iso
│   └── UbuntuServer-LTS.iso
├── BaselineClones/         # Clean post-install snapshots, never touched
└── Snapshots/              # Working snapshots per lab milestone
```

**Rule:** always safely eject the SSD before unplugging. Pulling the cable while VMware has the disk open will corrupt VM files.

---

## Hypervisor

**VMware Workstation Pro 17** — free for personal use. Snapshot management is reliable for Windows Server work and aligns with what MST200 at Seneca uses.

---

## VM Resource Allocation

Total host RAM: 32 GB. Allocated across lab VMs when running concurrently:

| VM | Role | RAM | vCPU | Disk |
|---|---|---|---|---|
| DC01 | Windows Server 2022 — Domain Controller | 4 GB | 2 | 60 GB |
| CLIENT01 | Windows 11 — Domain-joined client | 4 GB | 2 | 60 GB |
| UbuntuServer | Ubuntu Server LTS — Linux + automation | 2 GB | 2 | 30 GB |
| Wazuh | SIEM (Lab 3 only) | 6 GB | 2 | 60 GB |
| **Total** | | **16 GB** | | |

Host retains ~16 GB for Windows, browser, and VMware itself. All four VMs can run concurrently — required for Lab 3 (SIEM observing live AD traffic).

---

## Network Design

Two virtual networks:

**Lab network (NAT)** — `192.168.100.0/24`
All lab VMs. Internal traffic between DC01, CLIENT01, UbuntuServer, and Wazuh. Internet access through host NAT for updates.

**Host-only management** — `192.168.200.0/24`
Host-to-VM management traffic only (RDP from host, SSH to UbuntuServer).

```
                    ┌─────────────────────────────┐
                    │      Host: G16 (Win 11)     │
                    └──────────────┬──────────────┘
                                   │
              ┌────────────────────┼────────────────────┐
              │                    │                    │
        NAT (lab)             Host-only            Internet
       192.168.100.0/24    192.168.200.0/24
              │
   ┌──────────┼──────────┬──────────┐
   │          │          │          │
 DC01     CLIENT01   UbuntuSrv    Wazuh
 .10        .20         .30        .40
```

Static IPs for all servers. CLIENT01 gets its address via DHCP from DC01 once Lab 1 is configured.

---

## Baseline Clone Strategy

Each freshly-installed VM is cloned once before any configuration and stored in `BaselineClones/`. Lab rebuilds take ~2 minutes from a clone vs ~45 minutes for a fresh install.

Workflow:
1. Install OS
2. Patch fully, install VMware Tools
3. Power down cleanly
4. Clone → `BaselineClones/<OS>-clean/`
5. Lab work happens on the original VM, with snapshots at each milestone

If a lab goes off the rails beyond what's instructive to troubleshoot, discard the VM and pull a fresh copy from the baseline.

---

## Setup Checklist

- [ ] BIOS: Intel VT-x and VT-d enabled
- [ ] VMware Workstation Pro 17 installed
- [ ] External SSD formatted exFAT, mounts reliably
- [ ] Windows Server 2022 Evaluation ISO downloaded
- [ ] Windows 11 ISO downloaded
- [ ] Ubuntu Server LTS ISO downloaded
- [ ] Lab NAT network (`192.168.100.0/24`) created in VMware
- [ ] Host-only network (`192.168.200.0/24`) created in VMware
- [ ] DC01 created, Windows Server installed, baseline cloned

---

## Next

Once the checklist is complete → [Lab 1: Active Directory](../lab1-active-directory/README.md)
