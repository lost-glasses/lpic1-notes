# 📝 Text Processing & Filtering — Cheat Sheet

Quick command reference for common Linux text-processing tools.

## 1. grep — Search

**Syntax:**
```bash
grep [options] "pattern" file
```

| Option | Description |
|---|---|
| `-i` | Ignore case |
| `-v` | Invert match |
| `-w` | Match whole words |
| `-n` | Show line numbers |
| `-c` | Count matching lines |
| `-r` | Search recursively |
| `-E` | Enable extended regex |
| `-o` | Show only matched parts |

**Quick Examples:**
```bash
grep -in "error" logfile.txt
grep -v "^#" config.txt
grep -E "error|fail" logfile.txt
grep -r "root" /etc/
```

## 2. sed — Edit Text

**Syntax:**
```bash
sed [options] 'command' file
```

| Expression / Option | Description |
|---|---|
| `s/old/new/` | Replace first occurrence per line |
| `s/old/new/g` | Replace all occurrences per line |
| `/pattern/d` | Delete matching lines |
| `-n '5,10p'` | Print lines 5–10 |
| `-i` | Edit file in place |
| `-i.bak` | Edit and create backup |

**Quick Examples:**
```bash
sed 's/old/new/g' file.txt
sed '/^$/d' file.txt
sed -n '5,10p' file.txt
sed -i.bak 's/old/new/g' file.txt
```

## 3. cut — Extract Fields & Characters

**Syntax:**
```bash
cut [options] file
```

| Option | Description |
|---|---|
| `-d` | Specify delimiter |
| `-f` | Select fields |
| `-c` | Select character positions |

**Quick Examples:**
```bash
cut -d':' -f1 /etc/passwd
cut -d':' -f1,3 /etc/passwd
cut -d':' -f2-4 file.txt
cut -c1-5 file.txt
```

## 4. tr — Translate & Delete Characters

**Syntax:**
```bash
tr [options] SET1 [SET2]
```

| Option | Description |
|---|---|
| `-d` | Delete characters |
| `-s` | Squeeze repeated characters |

**Quick Examples:**
```bash
tr 'a-z' 'A-Z' < file.txt
tr 'A-Z' 'a-z' < file.txt
tr -d '0-9' < file.txt
tr ' ' '_' < file.txt
tr -s ' ' < file.txt
```

## 5. wc — Count

**Syntax:**
```bash
wc [options] file
```

| Option | Description |
|---|---|
| `-l` | Count lines |
| `-w` | Count words |
| `-c` | Count bytes |
| `-m` | Count characters |

```bash
wc -l file.txt
wc -w file.txt
wc -c file.txt
wc -m file.txt
```

## 6. sort — Sort Lines

**Syntax:**
```bash
sort [options] file
```

| Option | Description |
|---|---|
| `-n` | Numeric sorting |
| `-r` | Reverse order |
| `-u` | Remove duplicates |
| `-k` | Sort by field |

```bash
sort names.txt
sort -nr numbers.txt
sort -u file.txt
sort -k2 -n data.txt
```

## 7. uniq — Handle Duplicates

**Syntax:**
```bash
uniq [options] file
```

| Option | Description |
|---|---|
| `-c` | Count occurrences |
| `-d` | Show repeated lines |
| `-u` | Show unique lines |

```bash
uniq file.txt
uniq -c file.txt
uniq -d file.txt
uniq -u file.txt
sort file.txt | uniq -c
```

**Remember:** `uniq` only detects consecutive duplicates. Use `sort` first when necessary.

## 8. Regular Expressions

### BRE — Basic Regular Expressions

| Symbol | Meaning |
|---|---|
| `^` | Beginning of line |
| `$` | End of line |
| `.` | Any single character |
| `*` | Zero or more |
| `[abc]` | Any listed character |
| `[^abc]` | Any character except listed ones |
| `[a-z]` | Character range |

### ERE — Extended Regular Expressions

Enable with `grep -E`.

| Symbol | Meaning |
|---|---|
| `+` | One or more |
| `?` | Zero or one |
| `|` | OR |
| `()` | Grouping |
| `{n}` | Exactly n occurrences |
| `{n,m}` | Between n and m occurrences |

### BRE vs ERE

| Feature | BRE (GNU grep) | ERE |
|---|---|---|
| OR | `\|` | `|` |
| Grouping | `\(...\)` | `(...)` |
| One or more | `\+` | `+` |
| Zero or one | `\?` | `?` |

## 9. Useful Command Combinations

| Task | Command |
|---|---|
| Count matching lines | `grep -c "error" file.txt` |
| Remove empty lines | `sed '/^$/d' file.txt` |
| Find largest files | `find /var/log -type f -exec du -h {} + \| sort -rh \| head -n 10` |
| Count repeated lines | `sort file.txt \| uniq -c` |
| Count IP-shaped strings | `grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' access.log \| sort \| uniq -c \| sort -nr` |

---

**Quick Reminder:**

| Tool | Main Purpose |
|---|---|
| `grep` | Search |
| `sed` | Edit and filter lines |
| `cut` | Extract fields and characters |
| `tr` | Translate and delete characters |
| `wc` | Count |
| `sort` | Sort |
| `uniq` | Handle consecutive duplicates |
