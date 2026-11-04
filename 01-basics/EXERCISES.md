# 🧪 Linux Basics - Exercises

This file contains practical exercises and solutions for the Linux basics commands.

## 📝 Exercise 1: Navigation & Listing

Task: Navigate to /var/log, list all files with details, and find the largest file.

Solution:

```bash
cd /var/log
ls -lah
du -ah /var/log | sort -rh | head -n 5
```

## 📝 Exercise 2: File Management

Task: Create a directory called project with a subdirectory src, then create an empty file main.c inside src.

Solution:

```bash
mkdir -p project/src
touch project/src/main.c
ls -R project/
```

## 📝 Exercise 3: Links

Task: Create a hard link and a symbolic link for /etc/passwd in your home directory.

Solution:

```bash
ln /etc/passwd ~/passwd_hard
ln -s /etc/passwd ~/passwd_sym
ls -li ~/passwd*
```

## 📝 Exercise 4: Redirection

Task: Save the output of ls -la / to a file named root_list.txt, and append the output of date to it.

Solution:

```bash
ls -la / > root_list.txt
date >> root_list.txt
cat root_list.txt
```
