# Ansible Hospital Server Training

A hands-on Ansible training path built around a fictional hospital IT environment. Six progressive labs walk you through real-world automation tasks — inventory management, security hardening, web portal deployment, compliance logging, role-based design, and safe server decommissioning.

> **Educational use only.** This repository is a training lab. IPs and usernames in `inventory/` are specific to the author's lab VMs — update them to match your own environment before running any playbooks (see [Before you run](#before-you-run)).

---

## Prerequisites

- **Control node:** Linux/macOS with Ansible ≥ 2.12 installed
- **Managed nodes:** 2× Ubuntu/Debian VMs (VMware Workstation, NAT or Bridged networking)
  - `ubuntu-01` — patient-facing frontend server
  - `ubuntu-02` — internal/data backend server
- SSH access to both VMs with sudo privileges
- Recommended: SSH key auth (`ssh-copy-id user@host`) instead of passwords

---

## Repository Structure

```
Ansible_Test/
├── ansible.cfg                  # Global config (points to inventory/)
├── inventory/
│   ├── hosts.ini                # All managed hosts
│   └── group_vars/
│       ├── all.yml              # Variables for all hosts
│       ├── frontend.yml         # Frontend connection vars
│       └── backend.yml          # Backend connection vars
├── lab1_inventory/              # Lab 1: Inventory & ad-hoc commands
├── lab2_security/               # Lab 2: Firewall, SSH hardening, NTP
├── lab3_webportal/              # Lab 3: Nginx + Jinja2 templates
├── lab4_maintenance/            # Lab 4: Logrotate, cron, auditd
├── lab5_roles/                  # Lab 5: Reusable Ansible roles
└── lab6_decommission/           # Lab 6: Safe server decommissioning
```

---

## Before You Run

1. **Set your VM IPs** in `inventory/hosts.ini`:
   ```ini
   ubuntu-01 ansible_host=<your-frontend-ip>
   ubuntu-02 ansible_host=<your-backend-ip>
   ```

2. **Set your usernames** in `inventory/group_vars/`:
   ```yaml
   # frontend.yml
   ansible_user: your_frontend_user

   # backend.yml
   ansible_user: your_backend_user
   ```

3. **Use SSH keys or ansible-vault for credentials** — never commit plaintext passwords.
   ```bash
   # Copy your SSH key to each VM
   ssh-copy-id user@<vm-ip>

   # Or encrypt a password with vault
   ansible-vault encrypt_string 'MyPassword' --name 'ansible_ssh_pass'
   ```

---

## Labs

| Lab | Goal | Hosts | Become | Run Command |
|-----|------|-------|--------|-------------|
| [Lab 1](lab1_inventory/README.md) | Inventory & ad-hoc commands | all | no | `ansible all -m ping` |
| [Lab 2](lab2_security/README.md) | Security hardening & compliance | all | yes | `ansible-playbook lab2_security/site.yml --ask-become-pass` |
| [Lab 3](lab3_webportal/README.md) | Healthcare web portal (Nginx + Jinja2) | frontend | yes | `ansible-playbook lab3_webportal/site.yml --ask-become-pass` |
| [Lab 4](lab4_maintenance/README.md) | Centralized logging & maintenance | all | yes | `ansible-playbook lab4_maintenance/site.yml --ask-become-pass` |
| [Lab 5](lab5_roles/README.md) | Modular infrastructure with roles | all / frontend | yes | `ansible-playbook lab5_roles/site.yml --ask-become-pass` |
| [Lab 6](lab6_decommission/README.md) | Safe server decommissioning ⚠️ | target_host | yes | See below |

> **Lab 6 is destructive.** Read [`lab6_decommission/README.md`](lab6_decommission/README.md) first and take a VM snapshot before running:
> ```bash
> ansible-playbook lab6_decommission/decommission.yml \
>   -e "target_host=ubuntu-01 confirm_decommission=true"
> ```

Full lab descriptions and checkpoint lists are in [LABS.md](LABS.md).

---

## Conventions

| Group | Role | Host |
|-------|------|------|
| `frontend` | Patient-facing servers | `ubuntu-01` |
| `backend` | Internal / data servers | `ubuntu-02` |
| `hospital` | Parent group (frontend + backend) | both |

- **Secrets:** Never store passwords in inventory. Use `ansible-vault` or SSH key auth.
- **Idempotency:** Every playbook is designed to be safe to re-run.
- **Become:** Labs that need `sudo` require `--ask-become-pass` (or passwordless sudo configured on the VMs).

---

## Stack

| Component | Details |
|-----------|---------|
| Automation | Ansible ≥ 2.12 |
| Target OS | Ubuntu / Debian |
| Virtualisation | VMware Workstation (NAT) |
| Connection | SSH + sudo |
