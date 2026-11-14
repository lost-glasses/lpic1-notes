# 🛡️ Advanced Linux: Scripting, Security & Remote Access

This section covers the essential tools for Bash scripting, GPG encryption, SSH configuration, sudoers, cron jobs, and X11 forwarding.

## 📜 1. Bash Scripting Basics

### Structure & Variables
| Command / Syntax | Description | Example |
| :--- | :--- | :--- |
| `#!/bin/bash` | **Shebang** (tells the system to use bash) | `#!/bin/bash` |
| `var="value"` | **Define** a variable (no spaces around `=`) | `name="Ali"` |
| `$var` or `${var}` | **Use** a variable | `echo "$name"` |
| `read -p "prompt" var` | **Read** input from the user | `read -p "Enter name: " name` |
| `$1`, `$2`, ... | **Positional arguments** (command line) | `echo "$1"` |
| `$#` | **Number** of arguments | `echo "$#"` |
| `set --` | **Clear** all positional arguments | `set --` |

** Note: ** Variable names cannot contain hyphens (`-`). Use underscores (`_`) instead.
** Note: ** Without quotes, `[ -n $1 ]` becomes `[ -n ]` when `$1` is empty, which always returns true. **Always use quotes:** `[ -n "$1" ]`.

### Conditionals & Loops
| Command / Syntax | Description | Example |
| :--- | :--- | :--- |
| `if [ condition ]; then ... fi` | **If** statement | `if [ -d "$dir" ]; then echo "yes"; fi` |
| `elif [ condition ]; then ...` | **Else if** | `elif [ -f "$dir" ]; then ...` |
| `else ...` | **Else** | `else echo "no"; fi` |
| `while true; do ... done` | **Infinite loop** (use `break` to exit) | `while true; do read -p "Enter: " x; [[ "$x" =~ ^[0-9]+$ ]] && break; done` |
| `for var in list; do ... done` | **For loop** | `for f in *.txt; do echo "$f"; done` |
| `continue` | **Skip** to next iteration | `if [ ! -e "$file" ]; then continue; fi` |
| `break` | **Exit** the loop | `if [ -d "$dir" ]; then break; fi` |

### Comparison Operators
| Operator | Description | Example |
| :--- | :--- | :--- |
| `-z "$var"` | String is **empty** | `[ -z "$1" ]` |
| `-n "$var"` | String is **not empty** | `[ -n "$1" ]` |
| `"$a" = "$b"` | Strings are **equal** | `[ "$a" = "hello" ]` |
| `"$a" != "$b"` | Strings are **not equal** | `[ "$a" != "hello" ]` |
| `$a -eq $b` | Integers are **equal** | `[ "$a" -eq 5 ]` |
| `$a -gt $b` | Integer **greater than** | `[ "$a" -gt 5 ]` |
| `-d "$path"` | **Directory** exists | `[ -d "/etc" ]` |
| `-f "$path"` | **File** exists | `[ -f "/etc/passwd" ]` |
| `-e "$path"` | **Exists** (file or directory) | `[ -e "$dir" ]` |
| `[[ "$var" =~ regex ]]` | **Regex** match | `[[ "$r" =~ ^[0-9]+$ ]]` |

### Arithmetic & Math
| Command | Description | Example |
| :--- | :--- | :--- |
| `$((...))` | **Integer** math | `x=$((5 + 3))` |
| `bc <<< "..."` | **Decimal** math | `y=$(bc <<< "scale=2; 5/3")` |
| `scale=2` | Number of **decimal places** in `bc` | `bc <<< "scale=2; 3.14 * 2"` |

### Debugging
| Command | Description | Example |
| :--- | :--- | :--- |
| `set -x` | **Enable** debug mode (prints commands) | `set -x` |
| `set +x` | **Disable** debug mode | `set +x` |
| `bash -n script.sh` | **Check** syntax without running | `bash -n script.sh` |
| `bash -x script.sh` | **Run** with debug output | `bash -x script.sh` |

## 🔐 2. GPG (GNU Privacy Guard)

| Command | Description | Example |
| :--- | :--- | :--- |
| `gpg -c file` | **Encrypt** with a passphrase (symmetric) | `gpg -c secret.txt` |
| `gpg -d file.gpg` | **Decrypt** a file | `gpg -d secret.txt.gpg` |
| `gpg -d file.gpg > out.txt` | Decrypt and **save** to a file | `gpg -d secret.txt.gpg > plain.txt` |
| `gpg --full-generate-key` | Generate a **key pair** (asymmetric) | `gpg --full-generate-key` |
| `gpg --armor --export` | **Export** public key (ASCII) | `gpg --armor --export user@email.com` |
| `gpg --import` | **Import** a public key | `gpg --import pubkey.asc` |
| `gpg --encrypt --recipient` | Encrypt with a **public key** | `gpg --encrypt --recipient user@email.com file.txt` |

** Note: ** Symmetric encryption (`-c`) is simpler for quick tests. Asymmetric encryption requires exchanging public keys first.

## 🔑 3. SSH (Secure Shell)

### SSH Server Configuration (`/etc/ssh/sshd_config`)
| Directive | Description | Example |
| :--- | :--- | :--- |
| `X11Forwarding yes` | Enable **X11 forwarding** | `X11Forwarding yes` |
| `PasswordAuthentication yes` | Allow **password** login | `PasswordAuthentication yes` |
| `PubkeyAuthentication yes` | Allow **public key** login (default) | `PubkeyAuthentication yes` |
| `AllowUsers user1 user2` | **Restrict** login to specific users | `AllowUsers kali mini` |
| `PermitRootLogin no` | **Prevent** root login | `PermitRootLogin no` |

After editing, restart the service: `sudo systemctl restart ssh`

### Key Management
| Command | Description | Example |
| :--- | :--- | :--- |
| `ssh-keygen -t ed25519` | **Generate** a key pair (recommended) | `ssh-keygen -t ed25519` |
| `ssh-keygen -t rsa -b 4096` | Generate an RSA key (fallback) | `ssh-keygen -t rsa -b 4096` |
| `ssh-copy-id user@host` | **Copy** public key to server | `ssh-copy-id mini@192.168.1.100` |
| `ssh-copy-id -i key.pub user@host` | Copy a **specific** key | `ssh-copy-id -i ~/.ssh/kali.pub mini@192.168.1.100` |
| `ssh-add ~/.ssh/id_ed25519` | Add key to **ssh-agent** | `eval $(ssh-agent) && ssh-add ~/.ssh/id_ed25519` |

### Important Files & Permissions
| Path | Description | Permission |
| :--- | :--- | :--- |
| `~/.ssh/` | SSH **directory** | `700` (drwx------) |
| `~/.ssh/authorized_keys` | Keys allowed to **log in** | `600` (-rw-------) |
| `~/.ssh/known_hosts` | **Server fingerprints** | `600` (-rw-------) |
| `~/.ssh/config` | SSH **client configuration** | `600` (-rw-------) |

** Note: ** SSH will **reject** keys if permissions are wrong. Always check with `ls -ld ~/.ssh` and `ls -l ~/.ssh/authorized_keys`.

### X11 Forwarding
| Command | Description | Example |
| :--- | :--- | :--- |
| `ssh -X user@host` | Enable **X11 forwarding** (secure) | `ssh -X mini@192.168.1.100` |
| `ssh -Y user@host` | **Trusted** X11 forwarding (if `-X` fails) | `ssh -Y mini@192.168.1.100` |
| `echo $DISPLAY` | Check if **DISPLAY** is set | `echo $DISPLAY` (should be `localhost:10.0`) |
| `sudo cp ~/.Xauthority /root/` | Required for **sudo GUI apps** | `sudo cp ~/.Xauthority /root/` |
| `sudo XAUTHORITY=/root/.Xauthority gparted` | Run GUI app with **explicit** Xauthority | `sudo XAUTHORITY=/root/.Xauthority gparted` |

## ⚙️ 4. sudo and sudoers

| Command | Description | Example |
| :--- | :--- | :--- |
| `sudo visudo` | Edit **main** sudoers file safely | `sudo visudo` |
| `sudo visudo -f /etc/sudoers.d/user` | Edit a **separate** file (better) | `sudo visudo -f /etc/sudoers.d/testss` |
| `user ALL=(ALL) /path/cmd` | Allow running **only** a specific command | `testss ALL=(ALL) /usr/bin/ss` |
| `user ALL=(ALL) NOPASSWD: /path/cmd` | Allow running **without password** | `testss ALL=(ALL) NOPASSWD: /usr/bin/ss` |
| `sudo -l` | **List** allowed sudo commands for current user | `sudo -l` |

** Note: ** Always use `visudo` to edit sudoers. A syntax error can **lock you out** of sudo.

## ⏰ 5. Scheduling Tasks (cron & at)

### cron (Recurring Tasks)
| Command / Syntax | Description | Example |
| :--- | :--- | :--- |
| `crontab -e` | **Edit** cron jobs | `crontab -e` |
| `crontab -l` | **List** current cron jobs | `crontab -l` |
| `* * * * * command` | Format: **min hour day month weekday** | `0 20 30 * * /home/kali/backup.sh` |
| `0 20 30 * *` | 8 PM on the **30th** of every month | `0 20 30 * * /home/kali/backup.sh` |
| `* * * * *` | Every **minute** (for testing) | `* * * * * /home/kali/backup.sh` |

** Note: ** Always use **full paths** in cron (e.g., `/home/kali/script.sh`), not `~`.

### at (One-Time Tasks)
| Command | Description | Example |
| :--- | :--- | :--- |
| `at 16:00` | Schedule for **4 PM** today | `at 16:00` (then type command, Ctrl+D) |
| `at now + 1 minute` | Schedule for **1 minute** from now | `at now + 1 minute` |
| `atq` | **List** pending jobs | `atq` |
| `at -c 1` | **Show** details of job number 1 | `at -c 1` |
| `atrm 1` | **Remove** job number 1 | `atrm 1` |

## 🖥️ 6. Remote Desktop (VNC)

| Command | Description | Example |
| :--- | :--- | :--- |
| `vncpasswd` | Set **VNC password** | `vncpasswd` |
| `vncserver :1 -geometry 1280x720 -depth 24` | Start VNC on **display :1** | `vncserver :1 -geometry 1280x720 -depth 24` |
| `vncserver :1 ... -localhost no` | Allow connections from **other machines** | `vncserver :1 -geometry 1280x720 -localhost no` |
| `vncviewer host:1` | **Connect** to VNC server | `vncviewer 192.168.1.100:1` |
| `vncserver -kill :1` | **Stop** VNC server | `vncserver -kill :1` |
| `ss -tlpn \| grep 5901` | Check if VNC port is **open** | `ss -tlpn \| grep 5901` |

** Note: ** The `-localhost no` flag is required to allow remote connections. Without it, only localhost can connect.
** Note: ** The `~/.vnc/xstartup` file must start a desktop environment (e.g., `exec startxfce4`).

## 🖨️ 7. Virtual PDF Printer

| Command | Description | Example |
| :--- | :--- | :--- |
| `sudo apt install cups printer-driver-cups-pdf` | **Install** CUPS and PDF printer | `sudo apt install cups printer-driver-cups-pdf -y` |
| `sudo systemctl restart cups` | **Restart** CUPS service | `sudo systemctl restart cups` |
| `sudo lpadmin -p name -v cups-pdf:/ -P ppd -E` | **Create** PDF printer | `sudo lpadmin -p printer1 -v cups-pdf:/ -P /usr/share/ppd/cups-pdf/CUPS-PDF_opt.ppd -E` |
| `lp -d printer1 file.txt` | **Print** a file to PDF | `lp -d printer1 test.txt` |
| `ls -l ~/PDF/` | **Find** output PDFs | `ls -l ~/PDF/` |
| `sudo cupsenable printer1` | **Enable** a disabled printer | `sudo cupsenable printer1` |
| `sudo cupsaccept printer1` | **Accept** jobs for a printer | `sudo cupsaccept printer1` |

** Note: ** The PPD path on Kali is usually `/usr/share/ppd/cups-pdf/CUPS-PDF_opt.ppd`.
** Note: ** If `lpadmin` says `cups-driverd failed`, use `-P` with the full PPD path instead of `-m`.

## 💡 Pro Tips

| Shortcut / Command | Description |
| :--- | :--- |
| `Tab` | Auto-complete commands and paths |
| `Ctrl + R` | Search command history |
| `bash -x script.sh` | Run a script in debug mode |
| `man command` | Read the manual for any command |
| `which command` | Find the full path of a command |
| `history` | Show command history |
| `set -x` / `set +x` | Enable/disable debug mode in a script |
