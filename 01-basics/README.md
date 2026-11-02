# 🐧 Linux Basics - The Complete Foundation

This document covers all the essential commands and concepts I've learned for navigating, managing, and manipulating files in a Linux environment. Tested on Kali Linux (Debian-based) and AlmaLinux 9 (RHEL-based).

## 📂 1. Navigation & Listing

| Command | Description | Example |
| :--- | :--- | :--- |
| `pwd` | **Print Working Directory** (show current path) | `pwd` |
| `ls` | **List** directory contents | `ls` |
| `ls -l` | List with **long format** (permissions, size, date) | `ls -l` |
| `ls -a` | List **all** files (including hidden files starting with `.`) | `ls -a` |
| `ls -la` | Combine long format + hidden files | `ls -la` |
| `ls -lh` | Long format + **human-readable** sizes (KB, MB, GB) | `ls -lh` |
| `ls -li` | Long format + **inode** number | `ls -li` |
| `ls -d */` | List only **directories**, not files | `ls -d */` |
| `cd` | **Change Directory** | `cd /etc` |
| `cd ~` | Go to **home** directory | `cd ~` |
| `cd ..` | Go **up one level** | `cd ..` |
| `cd -` | Go back to **previous** directory | `cd -` |

## 📁 2. File & Directory Management

| Command | Description | Example |
| :--- | :--- | :--- |
| `mkdir` | **Make** a new directory | `mkdir new_folder` |
| `mkdir -p` | Make directory and **parent directories** if needed | `mkdir -p project/src` |
| `touch` | Create an **empty file** or update timestamp | `touch file.txt` |
| `cp` | **Copy** a file | `cp file.txt /tmp/` |
| `cp -r` | Copy **recursively** (for directories) | `cp -r folder/ /tmp/` |
| `mv` | **Move** or **Rename** a file/directory | `mv file.txt /tmp/` or `mv old.txt new.txt` |
| `rm` | **Remove** a file | `rm file.txt` |
| `rm -r` | Remove **recursively** (for directories) | `rm -r folder/` |
| `rm -rf` | **Force** remove recursively (no prompts - use with extreme caution!) | `rm -rf folder/` |
| `rmdir` | Remove an **empty** directory | `rmdir empty_folder/` |

## 📄 3. Viewing & Editing Files

| Command | Description | Example |
| :--- | :--- | :--- |
| `cat` | **Concatenate** and display file content | `cat /etc/passwd` |
| `cat -n` | Display with **line numbers** | `cat -n file.txt` |
| `less` | View file **page by page** (scrolling) | `less /var/log/syslog` |
| `head` | Display the **first** lines (default: 10) | `head -n 20 file.txt` |
| `tail` | Display the **last** lines (default: 10) | `tail -n 5 file.txt` |
| `tail -f` | **Follow** the file in real-time (live logs) | `tail -f /var/log/syslog` |
| `wc -l` | **Count lines** in a file | `wc -l file.txt` |
| `wc -w` | Count **words** in a file | `wc -w file.txt` |
| `wc -c` | Count **characters** in a file | `wc -c file.txt` |
| `nano` | Simple text editor | `nano file.txt` |
| `vi` / `vim` | Advanced text editor | `vim file.txt` |

## 🔍 4. Searching & Filtering

| Command | Description | Example |
| :--- | :--- | :--- |
| `grep` | Search for a **pattern** in a file | `grep "error" log.txt` |
| `grep -i` | Search **case-insensitive** | `grep -i "error" log.txt` |
| `grep -v` | **Invert** match (show lines that do NOT contain the pattern) | `grep -v "error" log.txt` |
| `grep -E` | Use **Extended Regex** (allows `|`, `+`, `?`) | `grep -E "error|fail" log.txt` |
| `grep -w` | Match **whole words** only | `grep -w "error" log.txt` |
| `find` | **Find** files by name, size, or type | `find /home -name "*.txt"` |
| `find -type f` | Find only **files** | `find . -type f` |
| `find -type d` | Find only **directories** | `find . -type d` |
| `which` | Locate the **path** of a command | `which bash` |

## 🔗 5. Links

| Command | Description | Example |
| :--- | :--- | :--- |
| `ln` | Create a **Hard Link** (same inode, survives original deletion) | `ln original.txt hardlink.txt` |
| `ln -s` | Create a **Symbolic Link** (shortcut, breaks if original is deleted) | `ln -s /tmp /home/kali/tmp_link` |
| `ls -l` | Check if a file is a link (look for `l` at the beginning and `->`) | `ls -l mylink` |

## 📦 6. Redirection & Pipes

| Symbol | Description | Example |
| :--- | :--- | :--- |
| `>` | **Redirect** output to a file (overwrites) | `ls > filelist.txt` |
| `>>` | **Append** output to a file | `echo "new line" >> file.txt` |
| `<` | **Redirect** input from a file | `sort < unsorted.txt` |
| `\|` (pipe) | Send output of one command to another as input | `ls -la \| grep ".txt"` |
| `2>` | Redirect **errors** to a file | `ls /xyz 2> error.log` |
| `&>` | Redirect **both** output and errors to a file | `ls /xyz &> output.log` |
| `2>&1` | Redirect **errors to where standard output goes** | `ls /xyz > output.txt 2>&1` |
| `<<` | **Here Document** (multi-line input) | `cat > file.txt << EOF ... EOF` |

## 📦 7. Archiving & Compression

| Command | Description | Example |
| :--- | :--- | :--- |
| `tar` | **Archive** files (tape archive) | `tar -cvf archive.tar folder/` |
| `tar -czvf` | Archive + **gzip** compression (creates `.tar.gz`) | `tar -czvf archive.tar.gz folder/` |
| `tar -xzvf` | **Extract** a `.tar.gz` archive | `tar -xzvf archive.tar.gz` |
| `tar -tzvf` | **List** contents of a `.tar.gz` archive (without extracting) | `tar -tzvf archive.tar.gz` |
| `gzip` | Compress a file (removes original) | `gzip file.txt` |
| `gzip -k` | Compress a file (keep original) | `gzip -k file.txt` |
| `gunzip` | Decompress a `.gz` file | `gunzip file.txt.gz` |
| `xz` | High compression (removes original) | `xz file.txt` |
| `xz -k` | High compression (keep original) | `xz -k file.txt` |
| `unxz` | Decompress a `.xz` file | `unxz file.txt.xz` |

## 8. Pro Tips & Shortcuts

| Shortcut | Description |
| :--- | :--- |
| `# This is a comment` | **Comment** in Bash (everything after `#` is ignored) 
| `Tab` | **Auto-complete** commands and file names |
| `Ctrl + C` | **Stop** a running command |
| `Ctrl + L` | **Clear** the terminal screen |
| `Ctrl + A` | Move to the **beginning** of the line |
| `Ctrl + E` | Move to the **end** of the line |
| `Ctrl + U` | **Delete** from cursor to the beginning of the line |
| `Ctrl + K` | **Delete** from cursor to the end of the line |
| `Ctrl + R` | **Search** backward through command history |
| `man <command>` | Read the **manual** for any command |
| `--help` | Quick **help** for most commands |

---
