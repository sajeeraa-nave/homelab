# Scripts

Bash scripts written during Lab 4 for automating common Linux sysadmin tasks on the Ubuntu Server (`192.168.100.30`).

---

| Script | Purpose |
|---|---|
| `failed_logins.sh` | Count failed SSH login attempts from `auth.log` |
| `suspicious_logins.sh` | Flag any user with more than 5 failed login attempts |
| `log_backup.sh` | Copy `auth.log` to a timestamped backup in `/var/log/backups/` |
| `health_check.sh` | Report disk usage, free memory, and status of key services |

Each script is commented inline.

---

Scripts are added during Lab 4. See [`/lab4-network-linux/`](../lab4-network-linux/) for context on how they're used.
