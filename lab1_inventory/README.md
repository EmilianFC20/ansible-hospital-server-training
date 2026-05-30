# Lab 1: Hospital Network Inventory & Ad-hoc Commands

**Goal:** Organize hospital servers into logical groups and verify connectivity.

## Tasks
1. Review `inventory/hosts.ini` at the project root — this is the shared inventory.
2. Note how `group_vars/` separates connection variables from host definitions.
3. Verify SSH connectivity using the `ping` module.
4. Run ad-hoc commands to check server state.

## Environment Tips (VMWare NAT)
- With NAT, your host can reach the VMs but VMs may not resolve each other by name — use IPs directly.
- Ensure your VM user has passwordless sudo, or use `--ask-become-pass`.
- Prefer SSH key auth over passwords (`ssh-copy-id user@host`).

## Ad-hoc Commands
Run from the project root (where `ansible.cfg` lives):

```bash
# Ping all hospital servers
ansible all -m ping

# Check memory on backend servers
ansible backend -a "free -m"

# Check uptime on all servers
ansible all -a "uptime"

# Check disk space on frontend
ansible frontend -a "df -h"

# List running services
ansible all -b -a "systemctl list-units --type=service --state=running"
```

## Inventory Structure
```
inventory/
├── hosts.ini            ← host definitions with IPs
└── group_vars/
    ├── all.yml          ← applies to every host (python interpreter)
    ├── frontend.yml     ← ansible_user for ubuntu-01
    └── backend.yml      ← ansible_user for ubuntu-02
```
