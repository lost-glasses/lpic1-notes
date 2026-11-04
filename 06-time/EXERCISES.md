# ⏰ Time & Log Management - Exercises

This file contains practical exercises for hardware clock, NTP with chrony, rsyslog rules, journalctl, and locale/timezone.

## 📝 Exercise 1: Show Hardware Clock and Sync It with System Clock

Task: Show the system's hardware clock and then synchronize it with the operating system clock.

Solution:
```bash
sudo hwclock --show
sudo hwclock --systohc
sudo hwclock --show
```

Expected Output:
```text
2026-11-04 10:30:00.123456+03:30
```

## 📝 Exercise 2: Show Today's Date with Time Format

Task: Using the date command, show today's date in hour-minute-second format.

Solution:
```bash
date +%H:%M:%S
```

Expected Output:
```text
10:30:45
```

## 📝 Exercise 3: Set Up an NTP Server with chrony and Query It with ntpdate

Task: Using chrony, set up an NTP server on your system, then query it from another Linux system using ntpdate.

Solution:
```bash
# On the server:
sudo nano /etc/chrony/chrony.conf
# add: allow 192.168.1.0/24
sudo systemctl restart chrony
sudo ss -tulpn | grep 123

# On the client:
sudo ntpdate -q 192.168.1.100
```

Expected Output:
```text
server 192.168.1.100, stratum 3, offset 0.000123, delay 0.02567
```

## 📝 Exercise 4: Check Last Authentication Logs (auth and authpriv)

Task: Check the latest authentication logs of the system, auth and authpriv.

Solution:
```bash
sudo journalctl -u ssh -n 20
sudo tail -n 20 /var/log/auth.log
sudo grep -E "auth|authpriv" /var/log/syslog | tail -n 20
```

Expected Output:
```text
Nov  4 10:30:01 kali sshd[1234]: Accepted password for kali from 192.168.1.5
```

## 📝 Exercise 5: Write a New rsyslog Rule for Mail Warn Logs

Task: Write a new rsyslog rule that stores mail-related logs with severity warning and above in the file mail.warn.

Solution:
```bash
sudo nano /etc/rsyslog.d/10-mail-warn.conf
# add this line:
mail.warn    /var/log/mail.warn
sudo systemctl restart rsyslog
sudo logger -p mail.warn "test mail warning"
sudo cat /var/log/mail.warn
```

Expected Output:
```text
test mail warning
```

## 📝 Exercise 6: Check Latest Logs of a Desired Unit with journalctl

Task: Using journalctl, check the latest logs of a desired unit.

Solution:
```bash
sudo journalctl -u nginx -n 30
sudo journalctl -u nginx -f
```

Expected Output:
```text
Nov 04 10:30:00 kali systemd[1]: Started A high performance web server.
```

## 📝 Exercise 7: Send an Email from Your User to a New User and Show It

Task: Send an email from your user to a new user, then show the received email.

Solution:
```bash
sudo useradd -m testuser
sudo passwd testuser
echo "Hello testuser, this is a test email!" | mail -s "Test Subject" testuser
sudo cat /var/mail/testuser
```

Expected Output:
```text
From kali@kali  Mon Nov 04 10:30:00 2026
Subject: Test Subject
To: testuser@kali
Hello testuser, this is a test email!
```

## 📝 Exercise 8: Show date Output Once in Persian and Once in English Using env

Task: Make the date command output the date once in Persian and once in English using env.

Solution:
```bash
env LANG=fa_IR.UTF-8 date
env LANG=en_US.UTF-8 date
```

Expected Output:
```text
سه‌شنبه ۰۴ نوامبر ۲۰۲۶ ...
Tue Nov  4 10:30:00 +0330 2026
```

## 📝 Exercise 9: Show Available Locale Status

Task: Show the status of available locales on the system.

Solution:
```bash
locale
locale -a
```

Expected Output:
```text
LANG=en_US.UTF-8
LC_CTYPE="en_US.UTF-8"
...
C
C.utf8
en_US.utf8
```

## 📝 Exercise 10: Show Baghdad Local Time Using TZ Variable

Task: Using the TZ environment variable, show the local time of Baghdad.

Solution:
```bash
TZ=Asia/Baghdad date
```

Expected Output:
```text
Tue Nov  4 10:00:00 +03 2026
```

## 📝 Optional Exercise 1: Turn the System into a Log Server

Task: Turn your system into a log server and receive logs from other points in the network.

Solution:
```bash
# On the server:
sudo nano /etc/rsyslog.conf
# uncomment:
# module(load="imudp")
# input(type="imudp" port="514")
sudo systemctl restart rsyslog
sudo ss -tulpn | grep 514

# On the client:
sudo nano /etc/rsyslog.d/50-remote.conf
# add: *.* @@192.168.1.100:514
sudo systemctl restart rsyslog
```

Expected Output:
```text
udp   UNCONN 0  0  0.0.0.0:514  0.0.0.0:*
```

## 📝 Optional Exercise 2: Build a Desired Locale with locale-gen

Task: Using locale-gen, build your desired locale file.

Solution:
```bash
sudo nano /etc/locale.gen
# uncomment: en_US.UTF-8 UTF-8
sudo locale-gen
locale -a | grep en_US
```

Expected Output:
```text
Generating locales (this might take a while)...
  en_US.UTF-8... done
Generation complete.
en_US.utf8
```
