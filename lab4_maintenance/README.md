# Lab 4: Centralized Logging & Maintenance

**Goal:** Automate audit trails, log rotation, and scheduled health checks — critical for hospital compliance.

## Tasks
1. **Log Rotation:** Configure `logrotate` to rotate system logs daily and keep 14 days.
2. **Health Check Cron:** Schedule a daily disk/memory report to `/var/log/hospital_health.txt`.
3. **Audit Logging:** Install and start `auditd` to record privileged actions.
4. **Rsyslog:** Ensure the system logging daemon is running.

## Key Concepts
- **`copy` module with `content`:** Write inline file content without needing a template file.
- **`cron` module:** Manage crontab entries idempotently (re-running won't duplicate entries).
- **`handlers`:** React to configuration changes (e.g. restart auditd when rules change).
- **`auditd` rules:** Use `auditctl`-style rules to track privileged file access.

## Running
```bash
ansible-playbook lab4_maintenance/site.yml --ask-become-pass
```

Check the cron was created:
```bash
ansible all -b -a "crontab -l -u root"
```

Check audit log:
```bash
ansible all -b -a "ausearch -m USER_LOGIN --start today"
```
