# Ansible Training: Hospital Infrastructure Automation

Hands-on labs simulating a hospital server environment — security, reliability, and lifecycle automation.

## Environment Prerequisites
- 2x Ubuntu/Debian VMs (VMWare Workstation, NAT or Bridged)
- SSH access with sudo privileges on each VM
- Ansible installed on your control node
- IPs configured in `inventory/hosts.ini`

---

## Lab Roadmap

### [ ] Lab 1: Hospital Network Inventory & Ad-hoc Commands
**Directory:** `lab1_inventory/`
- **Goal:** Establish connectivity and manage server groups.
- **Topics:** `hosts.ini`, `group_vars`, `ansible.cfg`, `ping`, `shell` modules.
- **Run:** `ansible all -m ping`

### [ ] Lab 2: Security Hardening & Compliance
**Directory:** `lab2_security/`
- **Goal:** Secure servers for medical data handling.
- **Topics:** `ufw`, `sshd_config`, `chrony`, user management, `lineinfile`.
- **Run:** `ansible-playbook lab2_security/site.yml --ask-become-pass`

### [ ] Lab 3: Healthcare Web Portal (Nginx & Templates)
**Directory:** `lab3_webportal/`
- **Goal:** Deploy a templated web server configuration.
- **Topics:** `nginx`, `template` (Jinja2), `vars`, `handlers`, `file` (symlinks).
- **Run:** `ansible-playbook lab3_webportal/site.yml --ask-become-pass`

### [ ] Lab 4: Centralized Logging & Maintenance
**Directory:** `lab4_maintenance/`
- **Goal:** Ensure audit trails and system health monitoring.
- **Topics:** `logrotate`, `cron`, `auditd`, audit rules, `service` management.
- **Run:** `ansible-playbook lab4_maintenance/site.yml --ask-become-pass`

### [ ] Lab 5: Modular Hospital Infrastructure (Roles)
**Directory:** `lab5_roles/`
- **Goal:** Refactor labs 2–3 into reusable Ansible roles.
- **Topics:** role structure (`tasks/`, `handlers/`, `defaults/`, `templates/`), `ansible-galaxy`.
- **Run:** `ansible-playbook lab5_roles/site.yml --ask-become-pass`

### [ ] Lab 6: Server Decommissioning
**Directory:** `lab6_decommission/`
- **Goal:** Safely retire a hospital server — revoke access, clear data, document, shut down.
- **Topics:** `user` (lock), `shell`, `copy`, `cron` (remove), `service` (stop), `debug`, `delegate_to`.
- **Run:** `ansible-playbook lab6_decommission/decommission.yml -e "target_host=ubuntu-01 confirm_decommission=true"`
- **WARNING:** Destructive — read `lab6_decommission/README.md` first and take a VM snapshot.

---

## Quick Reference

| Lab | Hosts | Become | Key Modules |
|-----|-------|--------|-------------|
| 1   | all   | no     | ping, shell |
| 2   | all   | yes    | ufw, lineinfile, user, service |
| 3   | frontend | yes | template, file, copy, service |
| 4   | all   | yes    | copy, cron, service |
| 5   | all / frontend | yes | roles |
| 6   | target_host | yes | user, shell, copy, service, debug |
