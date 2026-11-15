# 📝 Text Processing & Filtering in Linux

Linux provides several command-line tools for searching, filtering, extracting, and modifying text. These tools are especially useful when working with configuration files, logs, and command output.

This section introduces some of the most common text-processing tools and explains how they can be combined to work with text efficiently.

## 1. grep — Searching Text

`grep` searches for lines that match a specific pattern. It is commonly used to find keywords, errors, or specific information inside files and command output.

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
| `-n` | Display line numbers |
| `-c` | Count matching lines |
| `-r` | Search recursively in directories |
| `-E` | Use Extended Regular Expressions (ERE) |

### Examples

```bash
grep "error" logfile.txt
```

Finds lines containing `error`.

```bash
grep -in "error" logfile.txt
```

Searches without case sensitivity and displays line numbers.

```bash
grep -v "^#" config.txt
```

Displays lines that do not start with `#`, useful for filtering out comments.

You can also search for multiple patterns using ERE:

```bash
grep -E "error|fail" logfile.txt
```

This matches lines containing either `error` or `fail`.

---

## 2. sed — Stream Editor

`sed` is a stream editor that processes text line by line. It is commonly used to replace text, delete lines, and display selected parts of a file.

**Syntax:**

```bash
sed [options] 'command' file
```

### Common Operations

| Operation | Command |
|---|---|
| Replace first occurrence per line | `sed 's/old/new/' file.txt` |
| Replace all occurrences per line | `sed 's/old/new/g' file.txt` |
| Delete matching lines | `sed '/pattern/d' file.txt` |
| Display selected lines | `sed -n '5,10p' file.txt` |
| Add a prefix to each line | `sed 's/^/Prefix: /' file.txt` |

### Editing Files

By default, `sed` prints the modified text to standard output and leaves the original file unchanged.

To modify the file directly, use `-i`:

```bash
sed -i 's/old/new/g' file.txt
```

To create a backup before modifying the file:

```bash
sed -i.bak 's/old/new/g' file.txt
```

**Important:** Test your command without `-i` first, especially when working with important files.

---

## 3. cut — Extracting Fields and Characters

`cut` extracts specific fields or character positions from each line of text.

It is useful when working with structured text, such as colon-separated entries in `/etc/passwd`.

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

Extract the first field using `:` as the delimiter:

```bash
cut -d':' -f1 /etc/passwd
```

Extract multiple fields:

```bash
cut -d':' -f1,3 /etc/passwd
```

Extract characters 1 through 5:

```bash
cut -c1-5 file.txt
```

**Note:** `cut` uses a specific delimiter. For example, `cut -d' '` treats each individual space as a separator. It does not automatically handle multiple spaces as one delimiter. For variable whitespace, `awk` is often more suitable.

---

## 4. tr — Translating and Deleting Characters

`tr` translates, replaces, or deletes individual characters from standard input (stdin).

Unlike `cut`, which extracts fields or character positions, `tr` works with characters themselves.

### Common Options

| Option | Description |
|---|---|
| `-d` | Delete specified characters |
| `-s` | Squeeze repeated characters into one |

### Examples

Convert lowercase letters to uppercase:

```bash
echo "hello linux" | tr 'a-z' 'A-Z'
```

Replace spaces with underscores:

```bash
echo "hello linux world" | tr ' ' '_'
```

Remove digits:

```bash
echo "Linux123" | tr -d '0-9'
```

**Important:** `tr` reads from standard input, so it is commonly used with pipes or input redirection.

```bash
tr 'a-z' 'A-Z' < file.txt
```

---

## 5. wc, sort & uniq — Counting and Sorting

These three tools are often used together to analyze text, organize data, and identify repeated values.

### wc — Counting Text

`wc` counts lines, words, bytes, and characters.

| Option | Description |
|---|---|
| `-l` | Count lines |
| `-w` | Count words |
| `-c` | Count bytes |
| `-m` | Count characters |

For example:

```bash
wc -l file.txt
```

Counts the number of lines in a file.

### sort — Sorting Lines

`sort` arranges lines alphabetically or numerically.

| Option | Description |
|---|---|
| `-n` | Sort numerically |
| `-r` | Reverse the sorting order |
| `-u` | Remove duplicate lines while sorting |
| `-k` | Sort by a specified field |

Examples:

```bash
sort names.txt
sort -nr numbers.txt
sort -k2 -n data.txt
```

### uniq — Handling Duplicate Lines

`uniq` detects and handles consecutive duplicate lines.

| Option | Description |
|---|---|
| `-c` | Count occurrences |
| `-d` | Display repeated lines |
| `-u` | Display lines that appear only once |

Example:

```bash
sort file.txt | uniq -c
```

Sorts the input first, then counts how many times each line appears.

**Important:** `uniq` only detects adjacent duplicates. Sorting the input first ensures that identical lines are placed together.

---

## 6. Regular Expressions

Regular Expressions (regex) are patterns used to match text. They are useful when searching for more complex patterns instead of exact words.

`grep` supports two commonly used regex types: Basic Regular Expressions (BRE) and Extended Regular Expressions (ERE).

### Basic Regular Expressions (BRE)

BRE is the default mode used by `grep`.

| Symbol | Meaning |
|---|---|
| `^` | Beginning of a line |
| `$` | End of a line |
| `.` | Any single character |
| `*` | Zero or more occurrences |
| `[abc]` | Any one of the listed characters |
| `[^abc]` | Any character except the listed ones |
| `[a-z]` | Any character within the range |

Examples:

```bash
grep "^root" file.txt
grep "end$" file.txt
grep "[0-9]" file.txt
```

### Extended Regular Expressions (ERE)

ERE provides additional operators with simpler syntax. Use `grep -E` to enable it.

| Symbol | Meaning |
|---|---|
| `+` | One or more occurrences |
| `?` | Zero or one occurrence |
| `|` | OR — match either pattern |
| `()` | Group expressions |
| `{n}` | Exactly n occurrences |
| `{n,m}` | Between n and m occurrences |

Examples:

```bash
grep -E "error|fail" file.txt
grep -E "colou?r" file.txt
grep -E "[0-9]{3}" file.txt
```

### BRE vs ERE

Some operators require escaping in BRE but not in ERE.

| Feature | BRE (GNU grep) | ERE |
|---|---|---|
| OR | `\|` | `|` |
| Grouping | `\(...\)` | `(...)` |
| One or more | `\+` | `+` |
| Zero or one | `\?` | `?` |

---

## 7. Practical Examples

Text-processing tools become especially useful when combined with pipes.

**Find the 10 largest files under `/var/log`:**

```bash
find /var/log -type f -exec du -h {} + | sort -rh | head -n 10
```

**Count occurrences of IPv4-shaped strings in a log:**

```bash
grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' access.log | sort | uniq -c | sort -nr
```

This extracts strings that look like IPv4 addresses and counts their occurrences. It does not validate whether each address is a valid IPv4 address.

**Remove empty lines from a file:**

```bash
sed '/^$/d' file.txt
```

---

## 8. Pro Tips

- Use `grep -n` to locate matching lines quickly.
- Use `sed` without `-i` to preview changes before editing a file.
- Combine `sort` and `uniq` to identify and count repeated values.
- Use `tr` for simple character translation or deletion.
- Use `awk` when you need more flexible field processing or calculations.
