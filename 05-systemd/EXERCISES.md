# ⚙️ Systemd & Process Management - Exercises

This file contains practical exercises for runlevels, background jobs, signals, tmux, and systemd service management.

## 📝 Exercise 1: Check Current and Default Runlevel, Then Switch to Runlevel 3

Task: Check which runlevel your system is in and what its default runlevel is. Then switch the system to runlevel 3.

Solution:
```bash
systemctl get-default
systemctl isolate multi-user.target
systemctl get-default
```

Expected Output:
```text
graphical.target
```

## 📝 Exercise 2: Run a Program in Background, Bring to Foreground, Stop, and Resume in Background

Task: Run a desired program in the background. Then bring it to the foreground, stop it, and make it continue running in the background again.

Solution:
```bash
sleep 300 &
jobs
fg %1
# press Ctrl+Z to stop
bg %1
jobs
```

Expected Output:
```text
[1]+  Running                 sleep 300 &
[1]+  Stopped                 sleep 300
[1]+  sleep 300 &
```

## 📝 Exercise 3: Send TERM Signal to a Process

Task: Send the terminate signal to a desired process.

Solution:
```bash
sleep 500 &
ps -ef | grep sleep
kill -15 <PID>
```

Expected Output:
```text
[1]+  Terminated              sleep 500
```

## 📝 Exercise 4: Create New Console, List Its Processes, Then Kill Them All

Task: Create a new console (Ctrl+F1..F6), show all processes running in its terminal, then send the kill signal to all processes in that terminal.

Solution:
```bash
# switch to tty2 with Ctrl+Alt+F2
tty
ps -ef
pkill -9 -t tty2
```

Expected Output:
```text
/dev/tty2
root  1234  1  0 10:00 tty2  00:00:00 -bash
```

## 📝 Exercise 5: Keep a Program Running After Logout

Task: Make a desired program continue running even after logout.

Solution:
```bash
nohup sleep 1000 &
disown
exit
```

Expected Output:
```text
nohup: ignoring input and appending output to 'nohup.out'
```

## 📝 Exercise 6: Show Multiple Programs in One tmux Window

Task: Using tmux, display several desired programs in a single window.

Solution:
```bash
tmux
# Ctrl+b %  (split vertically)
# Ctrl+b "  (split horizontally)
# run different commands in each pane
# Ctrl+b d  (detach)
```

Expected Output:
```text
[0] 0:bash*  1:bash-  2:bash-
```

## 📝 Exercise 7: Identify the Init System, Install postfix and nginx, Check Auto-Start at Boot

Task: First determine what the system's init program is, then install postfix and nginx and check whether they start automatically at boot.

Solution:
```bash
ps -p 1 -o comm=
sudo apt install postfix nginx -y
systemctl is-enabled postfix nginx
```

Expected Output:
```text
systemd
enabled
enabled
```

## 📝 Exercise 8: Schedule System Shutdown After 3 Minutes

Task: Write a command that shuts down your system after three minutes.

Solution:
```bash
sudo shutdown -h +3
```

Expected Output:
```text
Shutdown scheduled for ... , use 'shutdown -c' to cancel.
```

## 📝 Exercise 9: Prevent sshd from Starting in Runlevel 2

Task: Make the SSH server not run in runlevel 2. (openssh-server may not be installed, so install it first.)

Solution:
```bash
sudo apt install openssh-server -y
sudo systemctl disable ssh
sudo systemctl stop ssh
systemctl is-enabled ssh
```

Expected Output:
```text
disabled
```

## 📝 Exercise 10: Verify Default systemd Target is graphical

Task: Verify that the default systemd target is set to graphical.

Solution:
```bash
systemctl get-default
```

Expected Output:
```text
graphical.target
```

## 📝 Exercise 11: Show Status of All Service Units

Task: Show the status of all service units.

Solution:
```bash
systemctl list-units --type=service
systemctl --failed
```

Expected Output:
```text
  UNIT                     LOAD   ACTIVE SUB     DESCRIPTION
  ssh.service              loaded active running OpenBSD Secure Shell server
  nginx.service            loaded active running A high performance web server
...
```

## 📝 Optional Exercise: C Program that Prints a Message on USR1 Signal

Task: Write a program in C (or any language) that prints "Received an usr1 signal" when it receives the USR1 signal.

Solution:
```bash
cat > usr1.c << 'EOF'
#include <stdio.h>
#include <signal.h>
void handler(int sig) { printf("Received an usr1 signal\n"); }
int main() {
    signal(SIGUSR1, handler);
    while(1) pause();
    return 0;
}
EOF
gcc usr1.c -o usr1
./usr1 &
kill -USR1 %1
```

Expected Output:
```text
Received an usr1 signal
```
