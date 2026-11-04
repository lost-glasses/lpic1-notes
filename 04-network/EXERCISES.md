# 🌐 Network - Exercises

This file contains practical exercises for viewing and configuring network interfaces, DNS, routing, and troubleshooting.

## 📝 Exercise 1: Check Domain Service Port and Protocol with getent

Task: Using getent, check which port and protocol the domain service of your operating system uses.

Solution:
```bash
getent services domain
```

Expected Output:
```text
domain              53/tcp
```

## 📝 Exercise 2: Add a Temporary IP to the NIC with ip

Task: Using the ip command, temporarily add an IP address to the system's NIC. Then show the result with ip a.

Solution:
```bash
sudo ip addr add 192.168.1.50/24 dev eth0
ip a show eth0
```

Expected Output:
```text
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
    inet 192.168.1.50/24 brd 192.168.1.255 scope global eth0
```

## 📝 Exercise 3: Show the ARP / Neighbor Table

Task: Show the MAC addresses that have been detected and cached by the operating system on your network.

Solution:
```bash
ip neigh
arp -a
```

Expected Output:
```text
192.168.1.1 dev eth0 lladdr 00:11:22:33:44:55 REACHABLE
```

## 📝 Exercise 4: Change DNS to 8.8.8.8 in CLI

Task: Using your system's network manager tools, change the DNS to 8.8.8.8 from the CLI.

Solution (NetworkManager):
```bash
nmcli connection modify "Wired connection 1" ipv4.dns "8.8.8.8"
nmcli connection up "Wired connection 1"
resolvectl status
```

Alternative (manual):
```bash
sudo nano /etc/resolv.conf
# add: nameserver 8.8.8.8
```

Expected Output:
```text
Link 2 (eth0)
    Current DNS Server: 8.8.8.8
```

## 📝 Exercise 5: Ping yahoo.com Four Times

Task: Using ping, ping yahoo.com four times. Note that when you ping a domain name, the first step is to resolve it via DNS.

Solution:
```bash
ping -c 4 yahoo.com
```

Expected Output:
```text
PING yahoo.com (74.6.231.21) 56(84) bytes of data.
64 bytes from 74.6.231.21: icmp_seq=1 ttl=52 time=120 ms
...
4 packets transmitted, 4 received, 0% packet loss
```

## 📝 Exercise 6: Show Routing Table and Trace Route to google.com

Task: Using ip or route, show the system's routes to other networks. Also trace the routing path to google.com.

Solution:
```bash
ip route
ip route get 8.8.8.8
traceroute google.com
```

Expected Output:
```text
default via 192.168.1.1 dev eth0 proto dhcp metric 100
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.100
8.8.8.8 via 192.168.1.1 dev eth0 src 192.168.1.100
```

## 📝 Exercise 7: Resolve google.com from /etc/hosts Instead of DNS

Task: Make the google.com address be read from /etc/hosts instead of being queried from the DNS server.

Solution:
```bash
sudo nano /etc/hosts
# add this line:
142.250.72.14 google.com
getent hosts google.com
```

Expected Output:
```text
142.250.72.14   google.com
```

## 📝 Exercise 8: Restart NetworkManager and Show Status

Task: Restart the system's network manager and show its status.

Solution:
```bash
sudo systemctl restart NetworkManager
systemctl status NetworkManager
```

Expected Output:
```text
● NetworkManager.service - Network Manager
     Active: active (running) since ...
```

## 📝 Exercise 9: Show Hostname and Machine ID, Then Change Hostname

Task: Show the system hostname and machine ID, then change the hostname to a desired name.

Solution:
```bash
hostnamectl
cat /etc/machine-id
sudo hostnamectl set-hostname mykali
hostnamectl
```

Expected Output:
```text
Static hostname: mykali
Machine ID: 9f2c1a...
```

## 📝 Exercise 10: Resolve lpi.org with host

Task: Using the host tool, resolve the address of lpi.org.

Solution:
```bash
host lpi.org
```

Expected Output:
```text
lpi.org has address 198.51.100.10
```

## 📝 Optional Exercise: IPv6 Between Two VMs

Task: Configure two test operating systems with IPv6, then ping them using ping6. Note: all switches/routers in the path must support IPv6.

Solution:
```bash
# On VM1:
sudo ip -6 addr add 2001:db8::1/64 dev eth0
# On VM2:
sudo ip -6 addr add 2001:db8::2/64 dev eth0
# From VM1:
ping6 2001:db8::2
```

Expected Output:
```text
PING 2001:db8::2(2001:db8::2) 56 data bytes
64 bytes from 2001:db8::2: icmp_seq=1 ttl=64 time=0.5 ms
```
