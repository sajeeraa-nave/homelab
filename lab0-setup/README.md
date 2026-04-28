Host machine: ASUS ROG Zephyrus G16 OLED, Core Ultra 9 185H, 32GB RAM, 1TB internal SSD, RTX 4060
	•	External lab storage: SanDisk Extreme 1TB portable SSD (USB 3.2 Gen 2)
	•	Hypervisor: VMware Workstation Pro 17
	•	Storage strategy: VMs and ISOs on external SSD; baseline clones preserved for fast rebuilds



ssd setup
SanDisk 1TB External SSD/
├── VMs/
│   ├── DC01-Windows-Server-2022/
│   ├── CLIENT01-Windows-11/
│   ├── UbuntuServer-Lab4/
│   └── Wazuh-SIEM/
├── ISOs/
│   ├── WindowsServer2022.iso
│   ├── Windows11.iso
│   └── UbuntuServer.iso
├── Snapshots/
└── BaselineClones/
