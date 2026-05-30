# Lab 2: Security Hardening & Compliance

**Goal:** Secure hospital servers to meet baseline requirements for handling medical data.

## Tasks
1. **Firewall:** Configure UFW to allow only SSH (22) and HTTP (80), deny everything else.
2. **SSH Hardening:** Disable root login and password authentication.
3. **Time Sync:** Install and start `chrony` for accurate log timestamps (required for audit trails).
4. **User Management:** Create a dedicated `med-admin` account on all servers.

## Playbook Structure
`site.yml` covers all four tasks with a handler that restarts SSH only when config changes.

## Running
From the project root:
```bash
ansible-playbook lab2_security/site.yml --ask-become-pass
```

Or targeting a single group:
```bash
ansible-playbook lab2_security/site.yml -l frontend --ask-become-pass
```

> **Note:** After running this playbook, password-based SSH is disabled. Make sure you have
> your SSH public key in `~/.ssh/authorized_keys` on each VM before running, or you will
> be locked out. Use `ssh-copy-id user@host` to set that up first.
