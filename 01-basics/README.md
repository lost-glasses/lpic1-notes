# Linux Basics

A practical learning reference for essential Linux commands and concepts. This section covers filesystem navigation, file management, text editors, searching, links, redirection, pipes, and archiving.

Most examples were practiced in Kali Linux. AlmaLinux 9 was used for selected exercises. Refer to the relevant exercise notes when the specific environment matters.

---

## 1. Navigation & Listing

### Understanding the Linux Filesystem

Linux organizes files and directories in a hierarchical structure that starts from the root directory, represented by `/`.

Unlike Windows, which commonly uses drive letters such as `C:` and `D:`, Linux uses a single directory tree. Different disks and partitions can be attached to this tree at different mount points.

Some important directories include:

| Directory | Purpose |
| :--- | :--- |
| `/` | The root of the entire filesystem. |
| `/home` | Contains users' personal directories. |
| `/root` | The home directory of the root user. |
| `/etc` | Contains system-wide configuration files. |
| `/var` | Stores variable data, such as logs and caches. |
| `/tmp` | Stores temporary files. |
| `/bin` | Contains essential user commands. |
| `/usr` | Contains many user applications, libraries, and shared data. |
| `/dev` | Contains device files representing hardware and virtual devices. |

### Absolute vs. Relative Paths

A path describes the location of a file or directory.

- **Absolute path:** Starts from `/` and describes the complete location.
- **Relative path:** Describes a location relative to the current working directory.

For example, if the current directory is `/home/kali`:

```bash
cd /etc
```

This uses an absolute path.

```bash
cd Documents
```

This uses a relative path and looks for `Documents` inside `/home/kali`.

Two special path references are:

- `.` — The current directory.
- `..` — The parent directory.

### Common Navigation Commands

| Command | Description | Example |
| :--- | :--- | :--- |
| `pwd` | **Print Working Directory** — displays the current directory. | `pwd` |
| `ls` | Lists directory contents. | `ls` |
| `ls -l` | Displays files in long format, including permissions, ownership, size, and modification time. | `ls -l` |
| `ls -a` | Lists all entries, including hidden files. | `ls -a` |
| `ls -la` | Combines long format and hidden-file listing. | `ls -la` |
| `ls -lh` | Displays file sizes in a human-readable format. | `ls -lh` |
| `ls -li` | Displays inode numbers alongside file information. | `ls -li` |
| `ls -d */` | Lists directories in the current directory. | `ls -d */` |
| `cd` | Changes the current working directory. Without an argument, it goes to the user's home directory. | `cd` |
| `cd /etc` | Changes to a specific directory. | `cd /etc` |
| `cd ~` | Goes to the current user's home directory. | `cd ~` |
| `cd ..` | Moves one directory level up. | `cd ..` |
| `cd -` | Returns to the previous working directory. | `cd -` |

### What Are Hidden Files?

In Linux, filenames beginning with a dot (`.`) are treated as hidden files.

For example:

```bash
.bashrc
.profile
```

They are not displayed by a normal `ls` command. Use `ls -a` to display them.

**Why are they hidden?**

Many hidden files contain user or application configuration settings. Hiding them keeps ordinary directory listings less cluttered. It does not make them private or protected.

---

## 2. File & Directory Management

### File vs. Directory

A **file** stores data, such as text, configuration, or program content.

A **directory** organizes filesystem entries. It contains references to files and other directories, allowing the filesystem to maintain a hierarchical structure.

### Common Commands

| Command | Description | Example |
| :--- | :--- | :--- |
| `mkdir` | Creates a new directory. | `mkdir new_folder` |
| `mkdir -p` | Creates a directory along with any missing parent directories. | `mkdir -p project/src` |
| `touch` | Creates an empty file if it does not exist, or updates its timestamps if it already exists. | `touch file.txt` |
| `cp` | Copies a file. | `cp file.txt /tmp/` |
| `cp -r` | Copies a directory and its contents recursively. | `cp -r folder/ /tmp/` |
| `mv` | Moves or renames a file or directory. | `mv old.txt new.txt` |
| `rm` | Removes a file. | `rm file.txt` |
| `rm -r` | Removes a directory and its contents recursively. | `rm -r folder/` |
| `rm -rf` | Forces recursive removal without prompting. Use with extreme caution. | `rm -rf folder/` |
| `rmdir` | Removes an empty directory. | `rmdir empty_folder/` |

### Understanding Some Important Options

**What does recursive mean?**

Recursive operations apply an action to a directory and its contents, including subdirectories.

For example:

```bash
cp -r project/ backup/
```

The `-r` option tells `cp` to copy the directory and its contents, rather than treating it as a single regular file.

**What is the difference between `rm` and `rmdir`?**

- `rm` removes files.
- `rmdir` removes empty directories.
- `rm -r` can remove directories that contain files and other directories.

> **Important:** Be careful when using `rm`, especially with recursive or force options. Always check the target path before running a deletion command. Deleted files may be difficult or impossible to recover.

---

## 3. Viewing & Editing Files

Linux provides several commands for reading file contents. Some are suitable for displaying an entire file, while others are better for inspecting large files or monitoring logs.

### Viewing File Contents

| Command | Description | Example |
| :--- | :--- | :--- |
| `cat` | Displays file contents. It can also concatenate multiple files. | `cat file.txt` |
| `cat -n` | Displays file contents with line numbers. | `cat -n file.txt` |
| `less` | Views a file page by page. Press `q` to quit. | `less /var/log/syslog` |
| `head` | Displays the first 10 lines by default. | `head -n 20 file.txt` |
| `tail` | Displays the last 10 lines by default. | `tail -n 5 file.txt` |
| `tail -f` | Follows a file as new lines are added. Commonly used for monitoring logs. | `tail -f /var/log/syslog` |
| `wc -l` | Counts the number of lines. | `wc -l file.txt` |
| `wc -w` | Counts the number of words. | `wc -w file.txt` |
| `wc -c` | Counts the number of bytes. | `wc -c file.txt` |
| `wc -m` | Counts the number of characters according to the current locale. | `wc -m file.txt` |

### Why Do We Have Different Viewing Commands?

Each command is useful in a different situation:

- `cat` is convenient for displaying short files.
- `less` is useful for large files because it allows you to move through the content without printing everything at once.
- `head` helps inspect the beginning of a file.
- `tail` helps inspect the most recent entries, especially in log files.
- `tail -f` is useful when you want to watch new log entries as they appear.

### Bytes vs. Characters

A byte is a unit of data storage. A character is a text symbol.

These are not always the same.

For example, many English characters in UTF-8 use one byte, while some characters in other writing systems use multiple bytes.

Therefore:

- `wc -c` counts bytes.
- `wc -m` counts characters according to the current locale.

### Text Editors: Nano vs. Vim

A text editor allows you to create and modify text files directly from the terminal.

Two common terminal-based editors are Nano and Vim.

#### Nano

Nano is a simple, beginner-friendly terminal text editor.

Its shortcuts are displayed at the bottom of the screen, making it easier to learn.

Open a file:

```bash
nano file.txt
```

Useful shortcuts:

| Shortcut | Action |
| :--- | :--- |
| `Ctrl + O` | Write (save) the file. |
| `Ctrl + X` | Exit Nano. |
| `Ctrl + W` | Search for text. |
| `Ctrl + K` | Cut the current line. |
| `Ctrl + U` | Paste previously cut text. |
| `Ctrl + G` | Open the help screen. |

#### Vim

Vim is a powerful terminal-based text editor. Unlike Nano, it uses different modes to separate text entry from editing commands.

Open a file:

```bash
vim file.txt
```

The two most important modes are:

- **Normal Mode:** Used for navigation and executing editing commands. Vim starts in this mode.
- **Insert Mode:** Used to type and insert text into the file.

For example:

1. Open a file with `vim file.txt`.
2. Press `i` to enter Insert Mode.
3. Type your text.
4. Press `Esc` to return to Normal Mode.
5. Type `:wq` and press `Enter` to save and quit.

Useful Vim commands:

| Command | Action |
| :--- | :--- |
| `i` | Enter Insert Mode. |
| `Esc` | Return to Normal Mode. |
| `:w` | Save the file. |
| `:q` | Quit Vim. |
| `:wq` | Save and quit. |
| `:q!` | Quit without saving changes. |
| `dd` | Delete the current line. |
| `yy` | Copy the current line. |
| `p` | Paste after the current position. |

#### Nano vs. Vim: What's the Difference?

| Feature | Nano | Vim |
| :--- | :--- | :--- |
| Learning curve | Easier for beginners. | Requires learning its modes and commands. |
| Editing style | Shortcuts are displayed on screen. | Uses modes and keyboard commands. |
| Best suited for | Quick and straightforward editing. | Efficient editing and more advanced workflows. |
| Starting mode | Direct text editing. | Normal Mode. |

**Which one should you use?**

For quick configuration changes, Nano is often easier to use. Vim is worth learning because it provides powerful editing features and is commonly available on Linux systems.

---

## 4. Searching & Filtering

Linux provides different tools for searching files, directories, and text.

An important distinction:

- `find` searches for files and directories based on properties such as name, type, and size.
- `grep` searches inside text for matching patterns.

### grep — Searching Inside Text

`grep` searches for lines that match a given pattern.

Basic example:

```bash
grep "error" log.txt
```

This displays lines containing the word `error`.

| Command | Description | Example |
| :--- | :--- | :--- |
| `grep` | Searches for a pattern in a file. | `grep "error" log.txt` |
| `grep -i` | Searches without distinguishing uppercase and lowercase letters. | `grep -i "error" log.txt` |
| `grep -v` | Displays lines that do not match the pattern. | `grep -v "error" log.txt` |
| `grep -E` | Uses Extended Regular Expressions (ERE). | `grep -E "error\|fail" log.txt` |
| `grep -w` | Matches whole words rather than matching inside longer words. | `grep -w "error" log.txt` |

### What Are Regular Expressions?

A regular expression (regex) is a pattern used to describe text that you want to match.

For example:

```bash
grep -E "error|fail" log.txt
```

This searches for lines containing either `error` or `fail`.

Extended Regular Expressions support operators such as:

- `|` — OR: matches either pattern.
- `+` — One or more occurrences.
- `?` — Zero or one occurrence.
- `()` — Groups expressions together.

### find — Searching for Files and Directories

`find` searches through directory trees and can use different criteria to locate entries.

| Command | Description | Example |
| :--- | :--- | :--- |
| `find` | Searches for files and directories. | `find /home -name "*.txt"` |
| `find -type f` | Finds regular files. | `find . -type f` |
| `find -type d` | Finds directories. | `find . -type d` |
| `find -name` | Searches by filename. | `find /home -name "notes.txt"` |
| `find -iname` | Searches by filename without case sensitivity. | `find /home -iname "notes.txt"` |

The `.` in `find .` means the current directory. Therefore, the search starts from the current location.

### which vs. command -v

Both commands can help identify how a command is found by the shell, but they are not exactly the same.

| Command | Description | Example |
| :--- | :--- | :--- |
| `which` | Displays the path of an executable found through the command search path. | `which bash` |
| `command -v` | Asks the shell how it resolves a command. It can identify executables, aliases, and functions. | `command -v bash` |

`command -v` is generally more reliable for checking how the current shell resolves a command.

---

## 5. Links

Linux supports two main types of file links:

- Hard links
- Symbolic links (symlinks)

To understand their differences, we first need to understand an inode.

### What Is an Inode?

An inode is a filesystem data structure that stores information about a file, such as:

- File permissions
- Owner and group
- File size
- Timestamps
- Pointers to the file's data blocks

A directory stores names and references to their corresponding inodes.

**Important:** The filename itself is not stored inside the inode.

This is why multiple filenames can refer to the same inode.

### Hard Links

A hard link is another filename that refers to the same inode as an existing file.

Create one using:

```bash
ln original.txt hardlink.txt
```

Both filenames refer to the same underlying file data.

If you modify the contents through one name, the change is visible through the other name as well.

Deleting one filename does not remove the file's data while another hard link still refers to that inode.

### Symbolic Links

A symbolic link, also called a symlink, is a special file that stores a path pointing to another file or directory.

Create one using:

```bash
ln -s original.txt symlink.txt
```

Unlike a hard link, a symbolic link does not refer directly to the target's inode. It points to the target by its path.

If the target is moved or deleted, the symbolic link may become broken.

### Comparing Hard Links and Symbolic Links

| Feature | Hard Link | Symbolic Link |
| :--- | :--- | :--- |
| References | The same inode as the original file. | A target path. |
| Can cross filesystems? | Normally no. | Yes. |
| Can point to directories? | Normally not for ordinary users. | Yes. |
| If the original filename is deleted | The data remains accessible through another hard link. | The link may become broken if its target path no longer exists. |
| Inode number | Same as the original file. | Has its own inode. |

### Useful Commands for Inspecting Links

| Command | Description | Example |
| :--- | :--- | :--- |
| `ln` | Creates a hard link. | `ln original.txt hardlink.txt` |
| `ln -s` | Creates a symbolic link. | `ln -s original.txt symlink.txt` |
| `ls -li` | Displays inode numbers, useful for comparing hard links. | `ls -li original.txt hardlink.txt` |
| `ls -l` | Displays symbolic link information, including its target after `->`. | `ls -l symlink.txt` |

---

## 6. Redirection & Pipes

Normally, commands receive input from the terminal and display their output in the terminal.

Redirection and pipes allow us to control where that input and output go.

They are especially useful when working with logs, command results, scripts, and automation.

### Standard Input, Output, and Error

Linux processes commonly use three standard data streams:

| Stream | File Descriptor | Purpose |
| :--- | :--- | :--- |
| Standard Input (stdin) | `0` | Receives input. |
| Standard Output (stdout) | `1` | Sends normal command output. |
| Standard Error (stderr) | `2` | Sends error messages. |

For example, when you run a command, its normal output usually appears on the terminal. If the command encounters an error, the error message is normally sent to stderr.

Keeping these streams separate allows us to save normal output and errors in different places.

### Redirection Operators

| Symbol | Description | Example |
| :--- | :--- | :--- |
| `>` | Redirects standard output to a file, overwriting its previous contents. | `ls > filelist.txt` |
| `>>` | Appends standard output to a file without overwriting existing contents. | `echo "new line" >> file.txt` |
| `<` | Uses a file as standard input. | `sort < unsorted.txt` |
| `2>` | Redirects standard error to a file. | `ls /nonexistent 2> error.log` |
| `&>` | In Bash, redirects both standard output and standard error to a file. | `ls /nonexistent &> output.log` |
| `2>&1` | Redirects standard error to the current destination of standard output. | `ls /nonexistent > output.txt 2>&1` |
| `<<` | Starts a here-document, providing multiple lines of input to a command. | `cat << EOF` |

### What's the Difference Between `>` and `>>`?

Both operators send output to a file, but they handle existing content differently.

**Using `>`:**

```bash
echo "First line" > file.txt
echo "Second line" > file.txt
```

The second command overwrites the first line.

**Using `>>`:**

```bash
echo "First line" > file.txt
echo "Second line" >> file.txt
```

The second command appends a new line without removing the previous content.

### Understanding `2>&1`

This operator can look confusing at first.

Consider:

```bash
command > output.txt 2>&1
```

Read it in this order:

1. `> output.txt` sends standard output (file descriptor `1`) to `output.txt`.
2. `2>&1` sends standard error (file descriptor `2`) to the same destination as standard output.

As a result, both normal output and error messages go into `output.txt`.

**Why does order matter?**

Redirections are processed from left to right.

For example:

```bash
command > output.txt 2>&1
```

Both streams go to the file.

But:

```bash
command 2>&1 > output.txt
```

Standard error is first connected to the original standard output destination. Then standard output is redirected to the file.

Therefore, the two commands do not behave the same way.

### Pipes (`|`)

A pipe connects the standard output of one command to the standard input of another command.

In simple terms, it allows commands to pass data to each other.

Example:

```bash
ls -la | grep ".txt"
```

How it works:

1. `ls -la` lists directory contents.
2. The pipe (`|`) passes its output to `grep`.
3. `grep ".txt"` filters the received lines.

This is useful because you can combine small commands to perform more complex tasks.

### Here-Documents (`<<`)

A here-document provides multiple lines of input directly to a command.

Example:

```bash
cat > file.txt << EOF
First line
Second line
Third line
EOF
```

Everything between the opening `EOF` and the closing `EOF` is provided as input to `cat`.

Here-documents are especially useful in shell scripts when you need to provide blocks of text to commands.

---

## 7. Archiving & Compression

Archiving and compression are related, but they are not the same operation.

### What Is Archiving?

Archiving combines multiple files and directories into a single archive file.

For example, imagine you have a project directory containing several files and subdirectories. Instead of handling each item separately, you can combine them into one archive.

The `tar` command is commonly used for this purpose.

### What Is Compression?

Compression reduces the amount of storage space needed to represent data.

Tools such as `gzip` and `xz` compress files using different compression algorithms.

### Archiving vs. Compression

| Feature | Archiving | Compression |
| :--- | :--- | :--- |
| Purpose | Combines multiple files into one archive. | Reduces data size. |
| Common tool | `tar` | `gzip`, `xz` |
| Does it necessarily reduce size? | No. | Yes, when compression is effective. |
| Example | `archive.tar` | `file.txt.gz` |

A `.tar` file is an archive, but it is not necessarily compressed.

A `.tar.gz` file is a tar archive that has also been compressed using gzip.

### The tar Command

`tar` stands for Tape Archive. Although it was originally designed for tape archives, it is now commonly used to package files and directories.

General structure:

```bash
tar [options] archive_name files
```

Common commands:

| Command | Description | Example |
| :--- | :--- | :--- |
| `tar -cvf` | Creates an uncompressed tar archive. | `tar -cvf archive.tar folder/` |
| `tar -czvf` | Creates a tar archive compressed with gzip. | `tar -czvf archive.tar.gz folder/` |
| `tar -xzvf` | Extracts a gzip-compressed tar archive. | `tar -xzvf archive.tar.gz` |
| `tar -tzvf` | Lists the contents of a gzip-compressed tar archive without extracting it. | `tar -tzvf archive.tar.gz` |

### Understanding tar Options

| Option | Meaning |
| :--- | :--- |
| `-c` | Create an archive. |
| `-x` | Extract an archive. |
| `-t` | List archive contents. |
| `-v` | Verbose output: show processed files. |
| `-f` | Specify the archive filename. |
| `-z` | Use gzip compression. |

For example:

```bash
tar -czvf backup.tar.gz project/
```

This command:

1. Creates an archive (`-c`).
2. Uses gzip compression (`-z`).
3. Displays processed files (`-v`).
4. Writes the result to the specified archive file (`-f`).

### gzip and xz

These tools compress individual files.

| Command | Description | Example |
| :--- | :--- | :--- |
| `gzip` | Compresses a file using gzip. By default, the original file is replaced by the compressed file. | `gzip file.txt` |
| `gzip -k` | Compresses a file while keeping the original. | `gzip -k file.txt` |
| `gunzip` | Decompresses a `.gz` file. | `gunzip file.txt.gz` |
| `xz` | Compresses a file using xz. By default, the original file is replaced by the compressed file. | `xz file.txt` |
| `xz -k` | Compresses a file while keeping the original. | `xz -k file.txt` |
| `unxz` | Decompresses an `.xz` file. | `unxz file.txt.xz` |

**Why use different compression tools?**

Different compression algorithms have different characteristics, such as compression ratio, speed, and resource usage.

The important thing to remember for LPIC-1 is how to recognize and work with common formats such as `.gz`, `.xz`, `.tar`, and `.tar.gz`.

---

## 8. Pro Tips & Shortcuts

### Bash Comments

A comment is text in a shell script that is ignored by Bash.

Comments are useful for explaining what a command or section of a script does.

```bash
# This is a comment
echo "Hello Linux"
```

### Terminal Shortcuts

| Shortcut / Command | Description |
| :--- | :--- |
| `Tab` | Autocompletes commands and file or directory names. |
| `Ctrl + C` | Sends an interrupt signal to the foreground process. |
| `Ctrl + L` | Clears the terminal screen. |
| `Ctrl + A` | Moves the cursor to the beginning of the command line. |
| `Ctrl + E` | Moves the cursor to the end of the command line. |
| `Ctrl + U` | Deletes text from the cursor to the beginning of the line. |
| `Ctrl + K` | Deletes text from the cursor to the end of the line. |
| `Ctrl + R` | Searches backward through command history. |

### Getting Help in Linux

Linux provides built-in documentation and help options for many commands.

| Command | Description | Example |
| :--- | :--- | :--- |
| `man` | Opens the manual page for a command. | `man ls` |
| `--help` | Displays a command's help information, when supported. | `ls --help` |
| `help` | Displays help for Bash built-in commands. | `help cd` |
| `type` | Identifies how the shell interprets a command. | `type cd` |

**Why are these useful?**

You do not need to memorize every option of every Linux command.

Knowing how to access documentation is an important Linux skill. When you encounter an unfamiliar command, its manual page or help output can explain its purpose, options, and usage.

---

[Exercises](https://github.com/lost-glasses/lpic1-notes/blob/main/01-basics/EXERCISES.md)
