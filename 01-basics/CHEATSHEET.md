# Linux Basics — Cheat Sheet

A quick reference for commonly used Linux commands and terminal operations.

## 1. Navigation

| Command    | Description                                              |
| ---------- | -------------------------------------------------------- |
| `pwd`      | Show current working directory                           |
| `ls`       | List directory contents                                  |
| `ls -la`   | List all entries, including hidden files, in long format |
| `ls -lh`   | Show human-readable file sizes                           |
| `ls -li`   | Show inode numbers                                       |
| `ls -d */` | List directories in the current directory                |
| `cd /path` | Change to a specific directory                           |
| `cd ..`    | Move to parent directory                                 |
| `cd ~`     | Go to home directory                                     |
| `cd -`     | Return to previous directory                             |

## 2. File & Directory Management

| Command           | Description                               |
| ----------------- | ----------------------------------------- |
| `mkdir dir`       | Create a directory                        |
| `mkdir -p a/b`    | Create nested directories                 |
| `touch file.txt`  | Create an empty file or update timestamps |
| `cp file1 file2`  | Copy a file                               |
| `cp -r dir1 dir2` | Copy a directory recursively              |
| `mv old new`      | Move or rename a file or directory        |
| `rm file.txt`     | Remove a file                             |
| `rm -r dir`       | Remove a directory and its contents       |
| `rmdir dir`       | Remove an empty directory                 |

## 3. Viewing Files

| Command              | Description                        |
| -------------------- | ---------------------------------- |
| `cat file.txt`       | Display file contents              |
| `cat -n file.txt`    | Display contents with line numbers |
| `less file.txt`      | View a file page by page           |
| `head file.txt`      | Show first 10 lines                |
| `head -n 5 file.txt` | Show first 5 lines                 |
| `tail file.txt`      | Show last 10 lines                 |
| `tail -n 5 file.txt` | Show last 5 lines                  |
| `tail -f file.log`   | Follow new log entries             |
| `wc -l file.txt`     | Count lines                        |
| `wc -w file.txt`     | Count words                        |
| `wc -c file.txt`     | Count bytes                        |
| `wc -m file.txt`     | Count characters                   |

## 4. Text Editors

### Nano

| Shortcut   | Action           |
| ---------- | ---------------- |
| `Ctrl + O` | Save             |
| `Ctrl + X` | Exit             |
| `Ctrl + W` | Search           |
| `Ctrl + K` | Cut current line |
| `Ctrl + U` | Paste            |

### Vim

| Command | Action                |
| ------- | --------------------- |
| `i`     | Enter Insert Mode     |
| `Esc`   | Return to Normal Mode |
| `:w`    | Save                  |
| `:q`    | Quit                  |
| `:wq`   | Save and quit         |
| `:q!`   | Quit without saving   |
| `dd`    | Delete current line   |
| `yy`    | Copy current line     |
| `p`     | Paste                 |

## 5. Searching

### grep — Search Inside Files

| Command                    | Description                 |
| -------------------------- | --------------------------- |
| `grep "error" file.txt`    | Search for a pattern        |
| `grep -i "error" file.txt` | Case-insensitive search     |
| `grep -v "error" file.txt` | Show non-matching lines     |
| `grep -n "error" file.txt` | Show matching line numbers  |
| `grep -w "error" file.txt` | Match whole words           |
| `grep -E "error|fail" file.txt` | Search for either pattern |

### find — Search for Files and Directories

| Command                               | Description                       |
| ------------------------------------- | --------------------------------- |
| `find . -name "*.txt"`                | Find files by name                |
| `find . -iname "*.TXT"`               | Case-insensitive filename search |
| `find . -type f`                      | Find regular files                |
| `find . -type d`                      | Find directories                  |
| `find /var/log -type f -name "*.log"` | Find log files                    |

### Command Lookup

| Command           | Description                           |
| ----------------- | ------------------------------------- |
| `which bash`      | Show executable path                  |
| `command -v bash` | Show how the shell resolves a command |
| `type cd`         | Identify command type                 |

## 6. Links

| Command                          | Description                  |
| -------------------------------- | ---------------------------- |
| `ln original.txt hardlink.txt`   | Create a hard link           |
| `ln -s original.txt symlink.txt` | Create a symbolic link       |
| `ls -li file1 file2`             | Compare inode numbers        |
| `ls -l symlink.txt`              | Inspect symbolic link target |

## 7. Redirection & Pipes

| Operator | Description                                     |
| -------- | ----------------------------------------------- |
| `>`      | Redirect stdout; overwrite file                 |
| `>>`     | Append stdout to file                           |
| `<`      | Read stdin from a file                          |
| `2>`     | Redirect stderr                                 |
| `2>>`    | Append stderr                                   |
| `&>`     | Redirect stdout and stderr in Bash              |
| `2>&1`   | Redirect stderr to stdout's current destination |
| `\|`     | Pipe stdout into another command                |
| `<< EOF` | Start a here-document                           |

### Examples

```bash
ls -la > output.txt
echo "New line" >> output.txt
sort < input.txt
ls /invalid 2> errors.txt
command > output.txt 2>&1
ls -la | grep ".txt"
```

### Here-Document

```bash
cat > file.txt << EOF
First line
Second line
Third line
EOF
```

## 8. Sorting & Text Processing

| Command                   | Description                                |
| ------------------------- | ------------------------------------------ |
| `sort file.txt`           | Sort lines alphabetically                  |
| `sort -r file.txt`        | Reverse sort order                         |
| `sort -n file.txt`        | Sort numerically                           |
| `sort -u file.txt`        | Sort and remove duplicate lines            |
| `nl file.txt`             | Number lines                               |
| `sort file.txt \| nl`     | Sort and number output                     |
| `cut -d: -f1 /etc/passwd` | Extract first field using `:` as delimiter |
| `tr 'a-z' 'A-Z'`          | Convert lowercase to uppercase             |

### tr — Translate or Delete Characters

`tr` is used to translate (replace) or delete characters from standard input (`stdin`).

It reads input from a pipe or input redirection and writes the result to standard output (`stdout`).

**Examples:**

```bash
# Convert lowercase letters to uppercase
echo "hello linux" | tr 'a-z' 'A-Z'
# Output: HELLO LINUX

# Convert uppercase letters to lowercase
echo "HELLO LINUX" | tr 'A-Z' 'a-z'
# Output: hello linux

# Delete all digits
echo "Linux123" | tr -d '0-9'
# Output: Linux

# Replace spaces with underscores
echo "hello linux world" | tr ' ' '_'
# Output: hello_linux_world
```

**Reading from a file:**

```bash
tr 'a-z' 'A-Z' < file.txt
```

This displays the converted content in the terminal without modifying the original file.

To save the converted output into another file:

```bash
tr 'a-z' 'A-Z' < file.txt > uppercase.txt
```

**Important:** `tr` does not normally take a filename as a regular command-line argument. It reads from standard input, so pipes (`|`) and input redirection (`<`) are commonly used with it.

## 9. Archiving & Compression

### tar

| Command                         | Description                       |
| ------------------------------- | --------------------------------- |
| `tar -cvf archive.tar dir/`     | Create an uncompressed archive    |
| `tar -xvf archive.tar`          | Extract an archive                |
| `tar -tvf archive.tar`          | List archive contents             |
| `tar -czvf archive.tar.gz dir/` | Create a gzip-compressed archive  |
| `tar -xzvf archive.tar.gz`      | Extract a gzip-compressed archive |
| `tar -tzf archive.tar.gz`       | List compressed archive contents |

### gzip & xz

| Command              | Description                |
| -------------------- | -------------------------- |
| `gzip file.txt`      | Compress using gzip        |
| `gzip -k file.txt`   | Compress and keep original |
| `gunzip file.txt.gz` | Decompress gzip file       |
| `xz file.txt`        | Compress using xz          |
| `xz -k file.txt`     | Compress and keep original |
| `unxz file.txt.xz`   | Decompress xz file         |

## 10. Getting Help

| Command        | Description             |
| -------------- | ----------------------- |
| `man ls`       | Open manual page        |
| `ls --help`    | Display command help    |
| `help cd`      | Show Bash built-in help |
| `type command` | Identify command type   |

## 11. Terminal Shortcuts

| Shortcut   | Action                            |
| ---------- | --------------------------------- |
| `Tab`      | Autocomplete                      |
| `Ctrl + C` | Interrupt foreground process      |
| `Ctrl + L` | Clear terminal screen             |
| `Ctrl + A` | Move to beginning of command line |
| `Ctrl + E` | Move to end of command line       |
| `Ctrl + U` | Delete from cursor to beginning   |
| `Ctrl + K` | Delete from cursor to end         |
| `Ctrl + R` | Search command history            |

---

**Tip:** Use `man command` whenever you need more details about a command or its available options.
