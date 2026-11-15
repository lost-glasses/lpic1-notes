# 📝 Text Processing & Filtering in Linux

This section covers essential Linux tools for searching, filtering, extracting, and modifying text from the command line.

## 1. grep — Searching Text

`grep` searches for lines that match a given pattern in files or command output.

**Syntax:**

```bash
grep [options] "pattern" file
```

### Common Options

| Option | Description |
|---|---|
| `-i` | Ignore uppercase and lowercase differences |
| `-v` | Show lines that do not match |
| `-w` | Match whole words only |
| `-n` | Show line numbers |
| `-c` | Count matching lines |
| `-r` | Search recursively in directories |
| `-E` | Use Extended Regular Expressions (ERE) |

### Examples

```bash
grep "error" logfile.txt
grep -in "error" logfile.txt
grep -v "^#" config.txt
grep -r "localhost" /etc/
```

**Useful combination:**

```bash
grep -E "error|fail" logfile.txt
```

Searches for lines containing either `error` or `fail`.

---

## 2. sed — Stream Editor

`sed` processes text line by line. It is commonly used for replacing text, deleting lines, and displaying selected lines.

**Syntax:**

```bash
sed [options] 'command' file
```

### Common Operations

| Operation | Command |
|---|---|
| Replace first occurrence on each line | `sed 's/old/new/' file.txt` |
| Replace all occurrences on each line | `sed 's/old/new/g' file.txt` |
| Delete lines containing a pattern | `sed '/pattern/d' file.txt` |
| Display specific lines | `sed -n '5,10p' file.txt` |
| Add text at the beginning of each line | `sed 's/^/Prefix: /' file.txt` |

### Modifying Files

By default, `sed` prints the modified text to standard output without changing the original file.

- `-i`: Modify the original file directly.
- `-i.bak`: Modify the original file and create a `.bak` backup.

```bash
sed -i 's/old/new/g' file.txt
sed -i.bak 's/old/new/g' file.txt
```

**Important:** Test your command without `-i` before modifying important files.

---

## 3. cut — Extracting Fields and Characters

`cut` extracts selected fields or character positions from each line of text.

**Syntax:**

```bash
cut [options] file
```

### Common Options

| Option | Description |
|---|---|
| `-d` | Specify the field delimiter |
| `-f` | Select fields |
| `-c` | Select character positions |

### Examples

**Extract fields using a delimiter:**

```bash
cut -d':' -f1 /etc/passwd
```

Displays the first field of each line, using `:` as the delimiter.

**Extract multiple fields:**

```bash
cut -d':' -f1,3 /etc/passwd
```

**Extract character positions:**

```bash
cut -c1-5 file.txt
```

Displays characters 1 through 5 from each line.

**Note:** `cut -d' '` uses a single space as the delimiter. It does not automatically handle multiple spaces as one separator. For variable whitespace, `awk` is often more suitable.

---

## 4. wc, sort & uniq — Counting and Sorting

These tools are frequently used together when analyzing text and command output.

### wc — Counting

`wc` counts lines, words, bytes, and characters.

| Option | Description |
|---|---|
| `-l` | Count lines |
| `-w` | Count words |
| `-c` | Count bytes |
| `-m` | Count characters |

```bash
wc -l file.txt
wc -w file.txt
```

### sort — Sorting Lines

`sort` arranges lines alphabetically or numerically.

| Option | Description |
|---|---|
| `-n` | Numeric sorting |
| `-r` | Reverse order |
| `-u` | Remove duplicate lines while sorting |
| `-k` | Sort by a specified field |

```bash
sort names.txt
sort -nr numbers.txt
sort -k2 -n data.txt
```

### uniq — Removing Consecutive Duplicates

`uniq` removes or counts adjacent duplicate lines.

| Option | Description |
|---|---|
| `-c` | Count occurrences |
| `-d` | Show only repeated lines |
| `-u` | Show only unique lines |

```bash
uniq file.txt
sort file.txt | uniq
sort file.txt | uniq -c
```

**Important:** `uniq` only detects consecutive duplicates. Use `sort` first when duplicates may appear in different parts of a file.

---

## 5. Regular Expressions: BRE

Basic Regular Expressions (BRE) are used by tools such as `grep` by default.

| Symbol | Meaning |
|---|---|
| `^` | Beginning of a line |
| `$` | End of a line |
| `.` | Any single character |
| `*` | Zero or more occurrences of the preceding character |
| `[abc]` | Any one of the listed characters |
| `[^abc]` | Any character except the listed characters |
| `[a-z]` | Any character in the specified range |

### Examples

```bash
grep "^root" file.txt
grep "end$" file.txt
grep "colou*r" file.txt
grep "[0-9]" file.txt
```

---

## 6. Extended Regular Expressions: ERE

Extended Regular Expressions provide additional operators without requiring the backslash escaping used by traditional BRE syntax.

Use `grep -E` to enable ERE.

| Symbol | Meaning |
|---|---|
| `+` | One or more occurrences |
| `?` | Zero or one occurrence |
| `|` | OR — match either pattern |
| `()` | Group expressions |
| `{n}` | Exactly n occurrences |
| `{n,m}` | Between n and m occurrences |

### Examples

```bash
grep -E "error|fail" logfile.txt
grep -E "colou?r" file.txt
grep -E "[0-9]{3}" file.txt
```

**BRE vs ERE:**

| Feature | BRE | ERE |
|---|---|---|
| Default `grep` mode | Yes | No |
| Enable with | `grep` | `grep -E` |
| OR operator | `\|` (GNU grep) | `|` |
| Grouping | `\(...\)` | `(...)` |
| One or more | `\+` (GNU grep) | `+` |

---

## 7. Practical One-Liners

A few useful combinations of text-processing commands:

**Find the 10 largest files under `/var/log`:**

```bash
find /var/log -type f -exec du -h {} + | sort -rh | head -n 10
```

**Count how many times each IP-shaped string appears in a log:**

```bash
grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' access.log | sort | uniq -c | sort -nr
```

This extracts IPv4-shaped strings and counts their occurrences. It does not validate whether each address is a valid IPv4 address.

**Remove empty lines:**

```bash
sed '/^$/d' file.txt
```

**Replace spaces with underscores:**

```bash
tr ' ' '_' < file.txt
```

---

## 8. Pro Tips

- Use `grep -n` when you need to locate a matching line quickly.
- Use `grep -E` when a search pattern needs operators such as `|`, `+`, or `?`.
- Use `sed` without `-i` first to preview changes.
- Combine `sort`, `uniq`, and `wc` to analyze repeated values and their frequency.
- Use `awk` when text processing requires more flexible field handling or calculations.
