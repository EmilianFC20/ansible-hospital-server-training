# Lab 6: Hospital Server Decommissioning

**Goal:** Safely retire a hospital server by stopping services, revoking access, clearing sensitive data, and shutting down — all in a documented, auditable way.

## Why This Matters
In a hospital environment, decommissioning is a compliance event. Improper retirement risks:
- Sensitive patient data persisting on retired hardware
- Orphaned service accounts still holding access
- Audit gaps if the process isn't documented

## What the Playbook Does (in order)
1. **Snapshot** — records current state (hostname, services, users) to a local report file
2. **Stop services** — gracefully stops Nginx, Apache, and any hospital application services
3. **Revoke access** — locks user accounts, clears authorized SSH keys, removes sudo membership
4. **Clear sensitive data** — shreds bash history, flushes systemd journal
5. **Apply decommission MOTD** — marks the server as retired for anyone who logs in
6. **Shutdown** *(opt-in)* — halts the server if `confirm_shutdown=true` is also passed

## Safety Gates
The playbook requires explicit variables to run — it will fail fast if they're missing:

| Variable | Required | Description |
|---|---|---|
| `target_host` | Yes | Hostname from inventory (e.g. `ubuntu-01`) |
| `confirm_decommission` | Yes | Must be `true` to proceed past the safety check |
| `confirm_shutdown` | No | Pass `true` to also power off the server at the end |

## Running
```bash
# Dry run first — always
ansible-playbook lab6_decommission/decommission.yml \
  -e "target_host=ubuntu-01 confirm_decommission=true" \
  --check

# Actual decommission (no shutdown)
ansible-playbook lab6_decommission/decommission.yml \
  -e "target_host=ubuntu-01 confirm_decommission=true"

# Decommission + power off
ansible-playbook lab6_decommission/decommission.yml \
  -e "target_host=ubuntu-01 confirm_decommission=true confirm_shutdown=true"
```

## Output
A report file is saved locally to `./decommission_reports/` with the server name and date,
documenting the pre-decommission state for your records.

## Reverting (Lab Use Only)
Since this is a lab, you can restore a VM from a VMWare snapshot taken before running this playbook.
In production you would not do this — decommission is a one-way operation.
