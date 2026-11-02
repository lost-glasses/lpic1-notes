# ⏰ Time Management in Linux

This section covers how to view, set, and synchronize the system clock and hardware clock in Linux.

## 🕒 1. Viewing Date and Time

| Command | Description |
| :--- | :--- |
| `date` | Show current date and time |
| `date +%H:%M:%S` | Show only time (24-hour format) |
| `date +"%Y-%m-%d %H:%M:%S"` | Show full date and time |
| `date +"%I:%M:%S %p"` | Show time in 12-hour format with AM/PM |
| `hwclock --show` | Show hardware clock (BIOS/RTC) |
| `timedatectl` | Show comprehensive time and timezone info |

## ⚙️ 2. Setting Date and Time


| Command | Description |
| :--- | :--- |
| `date -s "2026-11-01 14:30:00"` | Set system date and time manually |
| `hwclock --systohc` | Sync hardware clock from system clock |
| `hwclock --hctosys` | Sync system clock from hardware clock |
| `timedatectl set-time "2026-11-01 14:30:00"` | Set system time (systemd) |

## 🌍 3. Timezone Management

| Command | Description |
| :--- | :--- |
| `timedatectl` | Show current timezone |
| `timedatectl list-timezones` | List all available timezones |
| `timedatectl set-timezone Asia/Tehran` | Set timezone to Tehran |
| `ls /usr/share/zoneinfo/` | List timezone data files |
| `TZ=Asia/Baghdad date` | Show time in Baghdad temporarily |
| `env TZ=UTC date` | Show time in UTC temporarily |

## 📦 4. NTP (Network Time Protocol) with chrony

| Command | Description |
| :--- | :--- |
| `chronyc tracking` | Show NTP sync status |
| `chronyc sources -v` | Show NTP servers and their status |
| `chronyc makestep` | Force a time jump (immediate sync) |
| `chronyc clients` | Show clients connected to this NTP server |
| `timedatectl set-ntp true` | Enable NTP synchronization |
| `systemctl status chrony` | Check chrony service status |
| `ntpdate -q 8.8.8.8` | Query NTP server (no change) |
| `ntpdate 8.8.8.8` | Sync time from NTP server |

## 🛠️ 5. Configuring chrony as an NTP Server

Step 1: Edit the chrony configuration file:

```bash
sudo nano /etc/chrony/chrony.conf
```

Step 2: Add the following line to allow clients:

```text
allow 192.168.157.0/24
```

Step 3: Restart the service:

```bash
sudo systemctl restart chrony
```

Step 4: Verify the server is listening:

```bash
sudo ss -tulpn | grep 514
```

# 💡 6. Pro Tips

| Shortcut / Command | Description |
| :--- | :--- |
| `timedatectl` | The best single command to check everything |
| `hwclock --systohc` | Always run after setting system time |
| `chronyc makestep` | Use when time difference is large (e.g., hours) |
| `TZ=Asia/Baghdad date` | Test timezones without changing system settings |
| `chronyc sources -v` | Check which NTP servers are being used |
| `timedatectl set-timezone` | Change the timezone permanently |
| `chronyc clients` | See who is using your NTP server |
