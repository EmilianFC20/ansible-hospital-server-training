# Hospital Ansible Labs

Structured Ansible training path simulating a hospital server environment.

## Stack
- **Automation:** Ansible
- **Target OS:** Ubuntu/Debian (2x VMs via VMWare Workstation NAT)
- **Control Node:** Linux host running Ansible

## Project Structure
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

## Running Labs
Each lab directory has its own `README.md`. The global inventory is at `inventory/hosts.ini`.

Labs 1–5 use:
```bash
ansible-playbook -i inventory/hosts.ini lab<N>_*/site.yml
```

Lab 6 (destructive — read README first):
```bash
ansible-playbook -i inventory/hosts.ini lab6_decommission/decommission.yml \
  -e "target_host=ubuntu-01 confirm_decommission=true"
```

## Conventions
- `frontend` group: patient-facing servers (ubuntu-01)
- `backend` group: internal/data servers (ubuntu-02)
- `hospital` group: parent group containing both
- Never store passwords in inventory — use ansible-vault or SSH keys
- Each lab has a `README.md` with tasks, goals, and run commands
