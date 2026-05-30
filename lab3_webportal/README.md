# Lab 3: Healthcare Web Portal (Nginx & Jinja2 Templates)

**Goal:** Deploy a templated Nginx configuration to frontend servers using Ansible variables and Jinja2.

## Tasks
1. Install Nginx on `frontend` servers.
2. Use the `template` module to deploy a site config from `templates/nginx.conf.j2`.
3. Enable the site and disable the default Nginx site.
4. Use a `handler` to reload Nginx only when the config changes.
5. Verify the portal is accessible on port 80.

## Key Concepts
- **`template` module:** Renders Jinja2 files server-side, injecting Ansible variables.
- **`handlers`:** Run tasks at the end of a play only if notified — avoids unnecessary restarts.
- **`vars`:** Define `hospital_name` and `web_port` to customize the config without touching the template.

## Running
```bash
ansible-playbook lab3_webportal/site.yml --ask-become-pass
```

Verify after running:
```bash
ansible frontend -a "curl -s http://localhost/health"
ansible frontend -a "nginx -t"
```
