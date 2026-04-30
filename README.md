# AAP-Windows

Ansible playbooks for **Ansible Automation Platform** jobs targeting **Windows** hosts over **WinRM**. This repo mirrors the *categories* of automation commonly demonstrated on Linux (connectivity, facts, packages/features, services, firewall visibility, scheduling, logs, patches, reboots) using **`ansible.windows`**.

## Inventory and Controller usage

- Each playbook uses **`hosts: l2-windows`**. Your Controller inventory (or `inventory/l2-windows.ini` for CLI runs) must define that group.
- Point job templates at an inventory that contains your Windows host(s) in **`l2-windows`**.
- You may still set **`limit`** on the job template or at launch to narrow hosts within that group.
- Attach a **Machine** credential with **WinRM** (HTTP/HTTPS) for the Windows host.
- Ensure the execution environment includes the **`ansible.windows`** collection (see `requirements.yml`).

## Playbook index

| Playbook | Linux-style parallel | Risk |
|----------|----------------------|------|
| `playbooks/win_connectivity.yml` | ping / SSH smoke test | Read-only |
| `playbooks/win_gather_facts.yml` | setup / gather_facts | Read-only |
| `playbooks/win_disk_facts.yml` | filesystem / disk facts | Read-only |
| `playbooks/win_package_inventory.yml` | `rpm -qa` / package facts | Read-only |
| `playbooks/win_service_facts.yml` | `systemctl status` / service facts | Read-only |
| `playbooks/win_service.yml` | start/stop/restart services | **Mutating** |
| `playbooks/win_firewall_inspect.yml` | firewalld / iptables visibility | Read-only |
| `playbooks/win_scheduled_task_inspect.yml` | cron / timers visibility | Read-only |
| `playbooks/win_eventlog_recent.yml` | journalctl-style recent events | Read-only |
| `playbooks/win_optional_feature.yml` | optional RPM/feature install | **Mutating** |
| `playbooks/win_updates.yml` | dnf/yum update | **Mutating**, long-running |
| `playbooks/win_reboot.yml` | reboot | **Disruptive** |

To target a different group name, fork the repo or change **`hosts:`** in each play.

## Extra variables (examples)

- **`win_service.yml`**: `service_name` (e.g. `Spooler`), `service_state` (`started` / `stopped` / `restarted`).
- **`win_optional_feature.yml`**: `feature_name` (optional component name, e.g. `TelnetClient`), `feature_state` (`present` / `absent`).
- **`win_updates.yml`**: `category_names` (list, default security/critical), `reboot_allowed` (bool).
- **`win_reboot.yml`**: `pre_reboot_delay_sec`, `reboot_timeout_sec`.

## SCM project on AAP

Create a **Project** from this repo (`main`), sync, then add **Job templates** per playbook path. Prefer read-only jobs first in demos.
