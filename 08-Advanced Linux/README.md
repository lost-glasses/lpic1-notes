# 🛡️ Advanced Linux: Scripting, Security & Remote Access

This section covers Bash scripting, GPG encryption, SSH configuration, sudoers, cron jobs, and X11 forwarding.

## 📜 1. Bash Scripting Basics

### Structure & Variables

Every script starts with a shebang (`#!/bin/bash`) to tell the system which interpreter to use.

```bash
#!/bin/bash

name="Ali"
echo "Hello, $name"
```

**Important Notes:**
*   Variable names **cannot contain hyphens** (`-`). Use underscores (`_`) instead (e.g., `backup_dir` not `backup-dir`).
*   Always use quotes around variables to prevent empty-string errors. Without quotes, `[ -n $1 ]` becomes `[ -n ]` when `$1` is empty, which always returns true. **Always write:** `[ -n "$1" ]`.

### Conditionals

```bash
if [ -z "$1" ]; then
    echo "No argument provided"
elif [ -d "$1" ]; then
    echo "It is a directory"
else
    echo "It is something else"
fi
```

### Loops

```bash
for file in "$dir"/*; do
    echo "$file"
done

while true; do
    read -p "Enter a number: " n
    [[ "$n" =~ ^[0-9]+$ ]] && break
done
```

### Arithmetic & Math

*   `$((...))` is used for **integer** math.
*   `bc` is used for **decimal** math (use `scale=2` for two decimal places).

```bash
x=$((5 + 3))
y=$(bc <<< "scale=2; 5/3")
```

### Debugging

*   `set -x`: Enables debug mode (prints each command before executing it).
*   `set +x`: Disables debug mode.

## 🔐 2. GPG (GNU Privacy Guard)

GPG is used for encrypting and signing data.

### Symmetric Encryption (with a passphrase)

```bash
gpg -c secret.txt          # Encrypts to secret.txt.gpg
gpg -d secret.txt.gpg      # Decrypts and prints to terminal
```

### Asymmetric Encryption (Public/Private Keys)

```bash
gpg --full-generate-key
gpg --armor --export user@email.com > pubkey.asc
gpg --import pubkey.asc
gpg --encrypt --recipient user@email.com file.txt
```

## 🔑 3. SSH (Secure Shell)

### SSH Server Configuration (`/etc/ssh/sshd_config`)

*   `X11Forwarding yes`: Enables X11 forwarding.
*   `PasswordAuthentication yes`: Allows password login.
*   `PubkeyAuthentication yes`: Allows public key login (default).
*   `AllowUsers user1 user2`: Restricts login to specific users.

After editing, restart the service: `sudo systemctl restart ssh`

### Key Management

```bash
ssh-keygen -t ed25519                 # Generate a key pair (recommended)
ssh-copy-id user@host                 # Copy public key to server
eval $(ssh-agent) && ssh-add ~/.ssh/id_ed25519  # Add key to ssh-agent
```

**Important Files & Permissions:**
*   `~/.ssh/` must have permission `700`.
*   `~/.ssh/authorized_keys` must have permission `600`.
*   If permissions are wrong, SSH will **reject** the keys.

### X11 Forwarding

*   `ssh -X user@host`: Enables X11 forwarding (secure).
*   `ssh -Y user@host`: Trusted X11 forwarding (use if `-X` fails).
*   `echo $DISPLAY`: Checks if DISPLAY is set (should be `localhost:10.0`).

** Important: ** To run GUI apps with `sudo` (like `gparted`), you must copy the `.Xauthority` file to root's home directory:
```bash
sudo cp ~/.Xauthority /root/
sudo gparted
```

## ⚙️ 4. sudo and sudoers

The `sudoers` file controls who can run what as root. **Always edit it with `visudo`**.

```bash
sudo visudo -f /etc/sudoers.d/username
```

To allow a user to run **only** a specific command:
```text
username ALL=(ALL) /usr/bin/ss
```

## ⏰ 5. Scheduling Tasks (cron & at)

### cron (Recurring Tasks)

```bash
crontab -e    # Edit cron jobs
crontab -l    # List current cron jobs
```

Format: `minute hour day month weekday command`
Example (8 PM on the 30th of every month):
```text
0 20 30 * * /home/kali/backup-home.sh
```

*   **Note:** Always use **full paths** in cron (e.g., `/home/kali/script.sh`), not `~`.

### at (One-Time Tasks)

```bash
at 16:00        # Schedule for 4 PM today (Ctrl+D to finish)
at now + 1 minute
atq             # List pending jobs
atrm 1          # Remove job number 1
```

## 🖥️ 6. Remote Desktop (VNC)

```bash
vncpasswd                          # Set VNC password
vncserver :1 -geometry 1280x720 -depth 24 -localhost no
vncviewer 192.168.157.128:1        # Connect from client
vncserver -kill :1                 # Stop VNC server
```

*   **Note:** The `-localhost no` flag is required to allow connections from other machines.
*   **Note:** The `~/.vnc/xstartup` file must be configured to start a desktop environment (e.g., `exec startxfce4`).

## 🖨️ 7. Virtual PDF Printer

```bash
sudo apt install cups printer-driver-cups-pdf -y
sudo systemctl restart cups
sudo lpadmin -p printer1 -v cups-pdf:/ -P /usr/share/ppd/cups-pdf/CUPS-PDF_opt.ppd -E
lp -d printer1 file.txt
```

*   **Note:** Output PDFs are saved in `~/PDF/` by default.
*   **Note:** If the printer is disabled, run: `sudo cupsenable printer1 && sudo cupsaccept printer1`
