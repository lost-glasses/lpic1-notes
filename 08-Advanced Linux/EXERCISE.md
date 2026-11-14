# 🧪 Advanced Linux - Exercises

This file contains practical exercises for Bash scripting, GPG, SSH, sudoers, cron, VNC, and X11 forwarding.

## 📝 Exercise 1: Script to Show File Types

**Task:** Write a script that receives a directory name as input and displays the file types inside it. The script should ask again if the user enters an invalid path.

**Solution:**
```bash
#!/bin/bash

while true; do
    if [ -z "$1" ]; then
        read -p "Enter a directory: " dir
    else
        dir="$1"
        set --
    fi

    if [ ! -e "$dir" ]; then
        echo "'$dir' does not exist! Try again:"
        continue
    fi

    if [ ! -d "$dir" ]; then
        echo "'$dir' is not a directory! Try again:"
        continue
    fi

    break
done

for file in "$dir"/*; do
    if [ -e "$file" ]; then
        type=$(file -b "$file")
        echo "$(basename "$file"): $type"
    fi
done
```

**Expected Output:**
```text
passwd: ASCII text
hosts: ASCII text
```

## 📝 Exercise 2: Script to Calculate Circle Area and Circumference

**Task:** Write a script that takes the radius as input and calculates both the area and circumference.

**Solution:**
```bash
#!/bin/bash

if [ -z "$1" ]; then
    read -p "Enter radius: " r
else
    r="$1"
fi

while true; do
    if [[ "$r" =~ ^[0-9]+([.][0-9]+)?$ ]]; then
        break
    else
        echo "'$r' is not a valid number! Try again:"
        read -p "Enter radius: " r
    fi
done

area=$(bc <<< "scale=2; 3.14 * $r * $r")
circumference=$(bc <<< "scale=2; 2 * 3.14 * $r")

echo "Radius: $r"
echo "Area: $area"
echo "Circumference: $circumference"
```

**Expected Output:**
```text
Radius: 5
Area: 78.50
Circumference: 31.40
```

## 📝 Exercise 3: Encrypt a File with GPG and Send It

**Task:** Encrypt a file using GPG, send it to another Linux system, then decrypt it and display the contents.

**Solution:**
```bash
# Create a test file
echo "This is a secret message." > secret.txt

# Encrypt the file (symmetric)
gpg -c secret.txt

# Send to remote system
scp secret.txt.gpg mini@192.168.157.128:/home/mini/
```

On the remote system, decrypt the file:
```bash
gpg -d secret.txt.gpg
```

**Expected Output:**
```text
This is a secret message.
```

## 📝 Exercise 4: Set Up SSH Server and Connect Remotely

**Task:** Set up an SSH server on your operating system and connect to it remotely.

**Solution:**
```bash
# On the server:
sudo apt install openssh-server -y
sudo systemctl enable --now ssh
ss -tlpn | grep :22

# On the client:
ssh mini@192.168.157.128
```

**Expected Output:**
```text
mini@mini:~$
```

## 📝 Exercise 5: Create a User and Allow Running `ss` with Root Privileges

**Task:** Create a new user and configure sudo so they can run the `ss` command with root privileges, but nothing else.

**Solution:**
```bash
sudo useradd -m testss
sudo passwd testss
which ss

sudo visudo -f /etc/sudoers.d/testss
```

Add this line inside the sudoers file:
```text
testss ALL=(ALL) /usr/bin/ss
```

Test the configuration:
```bash
su - testss
sudo ss -tlpn
sudo ls /root  # This should be denied
```

**Expected Output (for the denied command):**
```text
Sorry, user testss is not allowed to execute '/usr/bin/ls /root' as root...
```

## 📝 Exercise 6: SSH Passwordless Login with Public Key

**Task:** Configure SSH to connect to a Linux system without a password using public key authentication.

**Solution:**
```bash
# Generate key pair on client
ssh-keygen -t ed25519

# Copy public key to server
ssh-copy-id mini@192.168.157.128

# Connect without password
ssh mini@192.168.157.128
```

**Expected Output:**
```text
mini@mini:~$ (logged in without password)
```

## 📝 Exercise 7: Script to Calculate BMI

**Task:** Write a script that takes height and weight as arguments and displays the BMI, including the weight status.

**Solution:**
```bash
#!/bin/bash

if [ -z "$1" ] || [ -z "$2" ]; then
    read -p "Enter height (meters): " h
    read -p "Enter weight (kg): " w
else
    h="$1"
    w="$2"
fi

bmi=$(bc <<< "scale=2; $w / ($h * $h)")
echo "Your BMI is: $bmi"

if (( $(bc <<< "$bmi < 18.5") )); then
    echo "Status: Underweight"
elif (( $(bc <<< "$bmi < 25") )); then
    echo "Status: Normal"
elif (( $(bc <<< "$bmi < 30") )); then
    echo "Status: Overweight"
else
    echo "Status: Obese"
fi
```

**Expected Output:**
```text
Your BMI is: 22.85
Status: Normal
```

## 📝 Exercise 8: Install a Virtual PDF Printer

**Task:** Install a virtual PDF printer and print a desired file using it.

**Solution:**
```bash
sudo apt install cups printer-driver-cups-pdf -y
sudo systemctl restart cups

sudo lpadmin -p printer2 -v cups-pdf:/ -P /usr/share/ppd/cups-pdf/CUPS-PDF_opt.ppd -E

echo "This is a test for PDF printer." > test-print.txt
lp -d printer2 test-print.txt

ls -l ~/PDF/
```

**Expected Output:**
```text
-rw------- 1 kali kali 7864 Nov 12 12:12 test-print.txt__able.pdf
```

## 📝 Exercise 9: Schedule a Monthly Backup with cron

**Task:** Configure a cron job to back up your home directory using `tar` on the 30th day of every month at 8 PM.

**Solution:**
```bash
nano ~/backup-home.sh
```

Script content:
```bash
#!/bin/bash
backup_date=$(date +%Y-%m-%d)
backup_dir="$HOME/backups"
mkdir -p "$backup_dir"
tar -czvf "$backup_dir/home-backup-$backup_date.tar.gz" \
    --exclude="$backup_dir" \
    "$HOME"
```

Make executable and add to cron:
```bash
chmod +x ~/backup-home.sh
crontab -e
```

Add this line:
```text
0 20 30 * * /home/kali/backup-home.sh
```

**Expected Output (for `crontab -l`):**
```text
0 20 30 * * /home/kali/backup-home.sh
```

## 📝 Exercise 10: Run a Program at 4 PM with `at`

**Task:** Using `at`, schedule a desired program (e.g., creating a file) to run at 4 PM today.

**Solution:**
```bash
sudo apt install at -y
sudo systemctl start atd
sudo systemctl enable atd

at 16:00
```

Inside the `at` prompt, type the command and press `Ctrl+D`:
```bash
echo "This file was created by at at $(date)" > ~/at-test.txt
```

Verify the job is scheduled:
```bash
atq
```

**Expected Output:**
```text
1	Thu Nov 12 16:00:00 2026 a kali
```

## 📝 Exercise 11: X11 Forwarding to Display `xclock`

**Task:** Enable X11 forwarding and display the `xclock` program from another system.

**Solution:**
```bash
# On the server (enable X11Forwarding in /etc/ssh/sshd_config)
sudo systemctl restart ssh
sudo apt install xauth x11-apps -y

# On the client
ssh -X mini@192.168.157.128
xclock
```

**Expected Output:**
```text
A graphical clock appears on the client screen.
```

## 📝 Exercise 12: Connect to a Linux Desktop with VNC

**Task:** Connect to a Linux desktop using the VNC protocol.

**Solution:**
```bash
# On the server: install XFCE and VNC
sudo apt install xfce4 xfce4-goodies tigervnc-standalone-server -y
vncpasswd

nano ~/.vnc/xstartup
```

`xstartup` content:
```bash
#!/bin/sh
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
exec startxfce4
```

Start VNC and connect from the client:
```bash
chmod +x ~/.vnc/xstartup
vncserver :1 -geometry 1280x720 -depth 24 -localhost no

# On the client:
vncviewer 192.168.157.128:1
```

**Expected Output:**
```text
XFCE desktop appears on the client screen.
```

## 📝 Exercise 13: X11 Forwarding for `gparted` with Root Access

**Task:** Using X11 forwarding, display the `gparted` program from another system. The `.Xauthority` file must be copied to root's home directory because `gparted` runs with root privileges.

**Solution:**
```bash
# On the server: install gparted
sudo apt install gparted -y

# On the client: connect with trusted forwarding
ssh -Y mini@192.168.157.128

# On the server: copy .Xauthority to root
sudo cp ~/.Xauthority /root/

# Run gparted with sudo
sudo gparted
```

**Expected Output:**
```text
GParted graphical window appears on the client screen.
```
