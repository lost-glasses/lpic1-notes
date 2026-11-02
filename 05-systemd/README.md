# ⚙️ Systemd & Service Management (Complete Guide)

Systemd is the modern init system used in most Linux distributions. It manages services, mounts, sockets, devices, and more. This guide covers everything you need to know for LPIC-1 and daily SysAdmin tasks.

## 🔍 1. What is Systemd?

- **Init System:** The first process (PID 1) started by the kernel. It initializes the system and manages services.
- **Units:** The core of systemd. Everything is a unit (services, mounts, devices, sockets, targets, timers).
- **Parallel Startup:** Starts services in parallel, making boot faster.
- **Dependency Management:** Automatically resolves service dependencies.
- **Logging:** Built-in logging with `journald`.

## 📂 2. Unit Types

| Unit Type | Description | Example |
| :--- | :--- | :--- |
| `.service` | A system service (daemon) | `ssh.service`, `nginx.service` |
| `.socket` | A network or IPC socket | `sshd.socket` |
| `.target` | A group of units (like runlevels) | `multi-user.target`, `graphical.target` |
| `.mount` | A mount point | `/mnt/data.mount` |
| `.device` | A hardware device | `dev-sda.device` |
| `.timer` | A scheduled task (like cron) | `logrotate.timer` |

## 📋 3. Listing Units & Services

| Command | Description |
| :--- | :--- |
| `systemctl list-units` | List all active units |
| `systemctl list-units --type=service` | List all active services |
| `systemctl list-units --type=service --state=running` | List only running services |
| `systemctl list-units --type=service --state=failed` | List failed services |
| `systemctl list-unit-files` | List all unit files (enabled/disabled) |
| `systemctl list-unit-files --type=service` | List all service files |
| `systemctl list-unit-files --state=enabled` | List all enabled unit files |
| `systemctl --failed` | List all failed units |

## 🔄 4. Managing Services (Start, Stop, Restart, Enable, Disable)

| Command | Description |
| :--- | :--- |
| `systemctl start SERVICE` | Start a service immediately |
| `systemctl stop SERVICE` | Stop a service immediately |
| `systemctl restart SERVICE` | Stop and then start a service |
| `systemctl reload SERVICE` | Reload configuration without stopping |
| `systemctl status SERVICE` | Show detailed status (loaded, active, logs, PID) |
| `systemctl enable SERVICE` | Enable service to start at boot |
| `systemctl disable SERVICE` | Disable service from starting at boot |
| `systemctl is-enabled SERVICE` | Check if a service is enabled (`enabled`/`disabled`) |
| `systemctl is-active SERVICE` | Check if a service is currently running (`active`/`inactive`) |
| `systemctl mask SERVICE` | Prevent a service from starting (even manually) |
| `systemctl unmask SERVICE` | Remove the mask |

## 📜 5. Viewing Logs with `journalctl`

`journalctl` is the log viewer for systemd. It reads logs from the `journald` service.

| Command | Description |
| :--- | :--- |
| `journalctl` | Show all logs |
| `journalctl -u SERVICE` | Show logs for a specific service |
| `journalctl -u ssh -n 20` | Show the last 20 lines for SSH |
| `journalctl -f` | Follow logs in real-time (like `tail -f`) |
| `journalctl -b` | Show logs from the current boot |
| `journalctl -b -1` | Show logs from the previous boot |
| `journalctl --since "1 hour ago"` | Show logs from the last hour |
| `journalctl --since "2026-11-01" --until "2026-11-02"` | Show logs between two dates |
| `journalctl -p err` | Show only error-level logs |
| `journalctl -p warning` | Show warning-level logs |
| `journalctl -k` | Show kernel logs |
| `journalctl -xe` | Show recent errors with explanations |

**Useful Options:**

- `-n N`: Show the last N lines.
- `-f`: Follow logs (real-time).
- `-p`: Filter by priority (`emerg`, `alert`, `crit`, `err`, `warning`, `notice`, `info`, `debug`).
- `--no-pager`: Print logs without a pager.

## 🎯 6. Runlevels & Targets (Comparison)

In systemd, **Runlevels** are replaced by **Targets**.

| Runlevel | Target | Description |
| :--- | :--- | :--- |
| 0 | `poweroff.target` | Shutdown |
| 1 | `rescue.target` | Single-user mode (rescue) |
| 2 | `multi-user.target` | Multi-user (no GUI) |
| 3 | `multi-user.target` | Multi-user (no GUI) |
| 4 | `multi-user.target` | Multi-user (no GUI) |
| 5 | `graphical.target` | Multi-user with GUI |
| 6 | `reboot.target` | Reboot |

| Command | Description |
| :--- | :--- |
| `systemctl get-default` | Show the current default target |
| `systemctl set-default TARGET` | Set a new default target |
| `systemctl isolate TARGET` | Switch to a target temporarily (immediately) |
| `systemctl list-units --type=target` | List all active targets |
| `systemctl list-dependencies TARGET` | Show dependencies of a target |

## 🛠️ 7. Power Management

| Command | Description |
| :--- | :--- |
| `systemctl reboot` | Reboot the system |
| `systemctl poweroff` | Shut down the system |
| `systemctl suspend` | Suspend (sleep) |
| `systemctl hibernate` | Hibernate (save to disk) |
| `systemctl hybrid-sleep` | Suspend + Hibernate |

## 📁 8. Unit Files & Locations

| Path | Description |
| :--- | :--- |
| `/lib/systemd/system/` | System-provided unit files (do not edit) |
| `/etc/systemd/system/` | Custom unit files (edit here) |
| `/run/systemd/system/` | Runtime unit files (temporary) |
| `/etc/systemd/system/multi-user.target.wants/` | Symlinks for enabled services |

| Command | Description |
| :--- | :--- |
| `systemctl cat SERVICE` | Show the unit file content |
| `systemctl edit SERVICE` | Create a custom override |
| `systemctl daemon-reload` | Reload systemd after editing unit files |

## 🔐 9. Creating a Custom Service

**Step 1: Create a script:**
```bash
#!/bin/bash
while true; do
    echo "Custom service is running at $(date)" >> /tmp/custom_service.log
    sleep 10
done
```bash
Save it as /usr/local/bin/custom_service.sh and make it executable:
```text
chmod +x /usr/local/bin/custom_service.sh
```
**Step 2: Create a unit file:**
```bash
sudo nano /etc/systemd/system/custom.service
```

**Step 3: Add the following:**
```ini
[Unit]
Description=My Custom Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/custom_service.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**Step 4: Enable and start:**
```bash
sudo systemctl daemon-reload
sudo systemctl enable custom.service
sudo systemctl start custom.service
systemctl status custom.service
```
## 📊 10. Comparison: SysV vs systemd

| Feature | SysV init (old) | systemd (modern) |
| :--- | :--- | :--- |
| Start service | service ssh start | systemctl start ssh |
| Stop service | service ssh stop | systemctl stop ssh |
| Enable at boot | chkconfig ssh on | systemctl enable ssh |
| Runlevel | runlevel | systemctl get-default |
| Change runlevel | init 3 | systemctl isolate multi-user.target |
| Logs | /var/log/syslog | journalctl |
