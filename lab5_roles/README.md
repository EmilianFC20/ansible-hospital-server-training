# Lab 5: Modular Hospital Infrastructure (Roles)

**Goal:** Refactor the previous labs into reusable Ansible roles. Roles allow you to share and reuse automation across projects and teams.

## Role Structure
```
roles/
├── common/       ← baseline packages, timezone, NTP
├── security/     ← UFW, SSH hardening, admin user
└── webserver/    ← Nginx + templated config (frontend only)
```

## Tasks
1. Review each role in `roles/` — notice how `tasks/`, `handlers/`, `defaults/`, and `templates/` each have a single responsibility.
2. Understand how `defaults/main.yml` provides overridable defaults.
3. Run `site.yml` and observe how roles compose cleanly.
4. Try overriding a role variable at the play level (e.g., change `hospital_name`).

## Key Concepts
- **Role defaults:** Variables in `defaults/main.yml` have the lowest precedence — easily overridden.
- **Role handlers:** Scoped to the role — `Restart SSH` in security won't clash with other roles.
- **`include_role`:** Can be used inline in tasks for conditional role application.

## Running
```bash
# Apply all roles
ansible-playbook lab5_roles/site.yml --ask-become-pass

# Dry run (check mode)
ansible-playbook lab5_roles/site.yml --check --ask-become-pass

# Override a variable
ansible-playbook lab5_roles/site.yml -e "hospital_name='Metro Hospital'"
```
