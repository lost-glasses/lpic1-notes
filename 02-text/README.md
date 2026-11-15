# 📝 Text Processing & Filtering — Cheat Sheet

A quick reference for searching, filtering, extracting, and manipulating text in Linux.

## 1. grep — Search Text

| Command | Description |
|---|---|
| `grep "error" file.txt` | Search for a pattern |
| `grep -i "error" file.txt` | Ignore case |
| `grep -v "error" file.txt` | Show non-matching lines |
| `grep -w "root" file.txt` | Match whole words |
| `grep -n "error" file.txt` | Show line numbers |
| `grep -c "error" file.txt` | Count matching lines |
| `grep -r "root" /etc/` | Search recursively |
| `grep -E "error|fail" file.txt` | Match either pattern |

## 2. sed — Stream Editor

| Command | Description |
|---|---|
| `sed 's/old/new/' file.txt` | Replace first occurrence per line |
| `sed 's/old/new/g' file.txt` | Replace all occurrences per line |
| `sed '/pattern/d' file.txt` | Delete matching lines |
| `sed -n '5,10p' file.txt` | Print lines 5–10 |
| `sed 's/^/Prefix: /' file.txt` | Add text at the beginning of each line |
| `sed -i 's/old/new/g' file.txt` | Modify the original file |
| `sed -i.bak 's/old/new/g' file.txt` | Modify file and create a backup |

**Remember:**
- `s` = substitute
- `g` = global replacement on each line
- `d` = delete
- `p` = print
- `-n` = suppress automatic output
- `-i` = edit file in place

## 3. cut — Extract Fields & Characters

| Command | Description |
|---|---|
| `cut -d':' -f1 file.txt` | Extract first field using `:` |
| `cut -d':' -f1,3 file.txt` | Extract fields 1 and 3 |
| `cut -d':' -f2-4 file.txt` | Extract fields 2–4 |
| `cut -c1-5 file.txt` | Extract characters 1–5 |
| `cut -c3- file.txt` | Extract from character 3 onward |

**Options:**
- `-d` = delimiter
- `-f` = field selection
- `-c` = character positions

## 4. tr — Translate & Delete Characters

| Command | Description |
|---|---|
| `tr 'a-z' 'A-Z'` | Convert lowercase to uppercase |
| `tr 'A-Z' 'a-z'` | Convert uppercase to lowercase |
| `tr -d '0-9'` | Delete digits |
| `tr ' ' '_'` | Replace spaces with underscores |
| `tr -s ' '` | Squeeze repeated spaces |

**Remember:**
- `-d` = delete characters
- `-s` = squeeze repeated characters
- `tr` reads from standard input (stdin), so use a pipe or input redirection.

Example:

```bash
echo "hello linux" | tr 'a-z' 'A-Z'
tr 'a-z' 'A-Z' < file.txt
```

## 5. wc, sort & uniq

### wc — Count

| Command | Description |
|---|---|
| `wc -l file.txt` | Count lines |
| `wc -w file.txt` | Count words |
| `wc -c file.txt` | Count bytes |
| `wc -m file.txt` | Count characters |

### sort — Sort Lines

| Command | Description |
|---|---|
| `sort file.txt` | Alphabetical sorting |
| `sort -n file.txt` | Numeric sorting |
| `sort -r file.txt` | Reverse sorting |
| `sort -nr file.txt` | Reverse numeric sorting |
| `sort -u file.txt` | Sort and remove duplicates |
| `sort -k2 -n file.txt` | Sort numerically by field 2 |

### uniq — Handle Duplicates

| Command | Description |
|---|---|
| `uniq file.txt` | Remove consecutive duplicates |
| `uniq -c file.txt` | Count consecutive occurrences |
| `uniq -d file.txt` | Show repeated lines |
| `uniq -u file.txt` | Show lines appearing once |
| `sort file.txt \| uniq -c` | Count all repeated lines after sorting |

**Important:** `uniq` only detects adjacent duplicates. Sort the input first when duplicates may be separated.

## 6. Regular Expressions

### BRE — Basic Regular Expressions

| Symbol | Meaning |
|---|---|
| `^` | Beginning of line |
| `$` | End of line |
| `.` | Any single character |
| `*` | Zero or more occurrences |
| `[abc]` | Any listed character |
| `[^abc]` | Any character except listed ones |
| `[a-z]` | Character range |

### ERE — Extended Regular Expressions

Use `grep -E` to enable ERE.

| Symbol | Meaning |
|---|---|
| `+` | One or more occurrences |
| `?` | Zero or one occurrence |
| `\|` | OR in GNU BRE |
| `|` | OR in ERE |
| `()` | Group expressions |
| `{n}` | Exactly n occurrences |
| `{n,m}` | Between n and m occurrences |

### Quick Examples

```bash
grep "^root" file.txt
grep "end$" file.txt
grep -E "error|fail" file.txt
grep -E "[0-9]{3}" file.txt
```

## 7. Useful One-Liners

| Task | Command |
|---|---|
| Find lines containing a word | `grep "word" file.txt` |
| Remove empty lines | `sed '/^$/d' file.txt` |
| Convert spaces to underscores | `tr ' ' '_' < file.txt` |
| Find 10 largest files under `/var/log` | `find /var/log -type f -exec du -h {} + \| sort -rh \| head -n 10` |
| Count occurrences of IP-shaped strings | `grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' access.log \| sort \| uniq -c \| sort -nr` |

---

## Quick Reminders

- `grep` → Search
- `sed` → Modify or filter lines
- `cut` → Extract fields or characters
- `tr` → Translate or delete characters
- `wc` → Count
- `sort` → Sort
- `uniq` → Handle consecutive duplicates

**Tip:** Combine commands with pipes (`|`) to build powerful text-processing workflows.
