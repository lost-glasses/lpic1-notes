# 🌐 Network Configuration in Linux

This section covers how to view, configure, and troubleshoot network interfaces, IP addresses, routing, DNS, and SSH in a Linux environment.

## 🔍 1. Viewing Network Interfaces & IPs

| Command | Description | Example |
| :--- | :--- | :--- |
| `ip addr` | Show all network interfaces and IP addresses (modern) | `ip a` |
| `ip addr show eth0` | Show IP info for a specific interface | `ip addr show eth0` |
| `ifconfig` | Show all interfaces (legacy tool) | `ifconfig` |
| `ip link` | Show link-layer info (MAC address, status) | `ip link show` |
| `ip -br addr` | Brief format (one line per interface) | `ip -br addr` |

## 🛣️ 2. Routing (Default Gateway)

| Command | Description | Example |
| :--- | :--- | :--- |
| `ip route` | Show the routing table | `ip route` |
| `ip route show` | Same as above | `ip route show` |
| `ip route get 8.8.8.8` | Show the route to a specific IP | `ip route get 8.8.8.8` |
| `route -n` | Show routing table (legacy tool) | `route -n` |

## 🌐 3. ARP / Neighbor Table (MAC Addresses)

| Command | Description | Example |
| :--- | :--- | :--- |
| `ip neigh` | Show the ARP table (MAC addresses) | `ip neigh` |
| `arp -a` | Show ARP table (legacy tool) | `arp -a` |
| `ip neigh flush all` | Clear the ARP cache | `sudo ip neigh flush all` |

## 📡 4. DNS Configuration & Resolution

| Command | Description | Example |
| :--- | :--- | :--- |
| `cat /etc/resolv.conf` | Show current DNS servers | `cat /etc/resolv.conf` |
| `resolvectl status` | Show DNS status (systemd-resolved) | `resolvectl status` |
| `host domain.com` | Resolve a domain name to IP | `host google.com` |
| `dig domain.com` | Advanced DNS query | `dig google.com +short` |
| `nslookup domain.com` | Query DNS (legacy) | `nslookup google.com` |
| `getent hosts domain.com` | Resolve using `/etc/hosts` first | `getent hosts google.com` |

## 📝 5. `/etc/hosts` (Local DNS Override)

| Command / File | Description | Example |
| :--- | :--- | :--- |
| `/etc/hosts` | Local static DNS entries | `sudo nano /etc/hosts` |
| `getent hosts domain.com` | Check if domain is in `/etc/hosts` | `getent hosts google.com` |

**Format of `/etc/hosts`:**
```text
<IP Address> <Hostname> [Aliases...]
```
**Example:**
```142.250.72.14 google.com
```


## 🔗 6. NetworkManager (nmcli & nmtui)

| Command | Description | Example |
| :--- | :--- | :--- |
| `nmcli device status` | Show device status | `nmcli device status` |
| `nmcli connection show` | Show all saved connections | `nmcli connection show` |
| `nmcli connection up "Wired connection 1"` | Activate a connection | `sudo nmcli connection up "Wired connection 1"` |
| `nmcli connection modify "Wired connection 1" ipv4.dns "8.8.8.8"` | Change DNS | `sudo nmcli connection modify "Wired connection 1" ipv4.dns "8.8.8.8"` |
| `nmtui` | Text-based UI for NetworkManager | `sudo nmtui` |

## 🖥️ 7. Hostname & Machine ID

| Command | Description | Example |
| :--- | :--- | :--- |
| `hostnamectl` | Show current hostname and machine ID | `hostnamectl` |
| `hostnamectl set-hostname mykali` | Change the hostname | `sudo hostnamectl set-hostname mykali` |
| `cat /etc/machine-id` | Show the machine ID | `cat /etc/machine-id` |
| `systemd-machine-id-setup` | Generate a new machine ID | `sudo systemd-machine-id-setup` |

## 🔑 8. SSH (Secure Shell)

| Command | Description | Example |
| :--- | :--- | :--- |
| `ssh user@host` | Connect to a remote host | `ssh kali@192.168.1.100` |
| `ssh-keygen -t ed25519` | Generate SSH key pair | `ssh-keygen -t ed25519` |
| `ssh-copy-id user@host` | Copy public key to remote host | `ssh-copy-id kali@192.168.1.100` |
| `scp file.txt user@host:/path/` | Securely copy a file | `scp file.txt kali@192.168.1.100:/tmp/` |
| `systemctl status ssh` | Check SSH service status | `sudo systemctl status ssh` |

## 🕒 9. Time Synchronization (NTP)

| Command | Description | Example |
| :--- | :--- | :--- |
| `timedatectl` | Show current time, timezone, and NTP status | `timedatectl` |
| `timedatectl set-ntp true` | Enable NTP synchronization | `sudo timedatectl set-ntp true` |
| `chronyc tracking` | Show NTP sync status (chrony) | `chronyc tracking` |
| `chronyc sources -v` | Show NTP servers | `chronyc sources -v` |
| `chronyc makestep` | Force a time step (jump) | `sudo chronyc makestep` |
| `ntpdate -q <server>` | Query NTP server (no change) | `sudo ntpdate -q 192.168.157.140` |

## 📡 10. Network Diagnostics & Testing

| Command | Description | Example |
| :--- | :--- | :--- |
| `ping -c 4 google.com` | Ping a host (4 times) | `ping -c 4 google.com` |
| `traceroute google.com` | Trace the route to a host | `traceroute google.com` |
| `mtr -r -c 10 8.8.8.8` | Advanced traceroute (report mode) | `sudo mtr -r -c 10 8.8.8.8` |
| `tracepath 8.8.8.8` | Trace path with MTU discovery | `tracepath 8.8.8.8` |
| `ss -tulpn` | Show open ports and listening services | `sudo ss -tulpn` |
| `nc -zv host 80` | Test if a port is open (netcat) | `nc -zv google.com 80` |

## 💡 11. Pro Tips

| Shortcut / Command | Description |
| :--- | :--- |
| `ip -br addr` | Quickest way to see all IPs |
| `ip route get 8.8.8.8` | Check which gateway is used for internet |
| `nmcli connection modify ...` | Best way to change network settings permanently |
| `getent hosts domain` | Check `/etc/hosts` before DNS |
| `traceroute` or `mtr` | Diagnose network latency and path |
| `ss -tulpn` | See what's listening on your server |

---
