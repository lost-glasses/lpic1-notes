# 📝 Text Processing & Filtering in Linux

This section covers the essential tools for processing, filtering, and manipulating text directly from the command line. These are the daily bread and butter of any Linux SysAdmin.

## 🔍 1. Searching with `grep` (Global Regular Expression Print)

| Command | Description | Example |
| :--- | :--- | :--- |
| `grep "pattern" file` | Search for a **pattern** in a file | `grep "error" log.txt` |
| `grep -i "pattern" file` | **Case-insensitive** search | `grep -i "error" log.txt` |
| `grep -v "pattern" file` | **Invert match** (show lines without the pattern) | `grep -v "error" log.txt` |
| `grep -w "pattern" file` | Match **whole words** only | `grep -w "error" log.txt` |
| `grep -r "pattern" dir` | **Recursive** search in directories | `grep -r "error" /var/log/` |
| `grep -E "pat1|pat2" file` | Use **Extended Regex** (OR operator) | `grep -E "error|fail" log.txt` |
| `grep -c "pattern" file` | **Count** matching lines | `grep -c "error" log.txt` |
| `grep -n "pattern" file` | Show **line numbers** | `grep -n "error" log.txt` |

## ✂️ 2. Stream Editing with `sed` (Stream Editor)

| Command | Description | Example |
| :--- | :--- | :--- |
| `sed 's/old/new/' file` | Replace the **first** occurrence in each line | `sed 's/apple/orange/' fruits.txt` |
| `sed 's/old/new/g' file` | Replace **all** occurrences (Global) | `sed 's/apple/orange/g' fruits.txt` |
| `sed -i 's/old/new/g' file` | Edit the file **in-place** (no backup) | `sed -i 's/apple/orange/g' fruits.txt` |
| `sed -i.bak 's/old/new/g' file` | Edit in-place with a **backup** (file.bak) | `sed -i.bak 's/apple/orange/g' fruits.txt` |
| `sed '/pattern/d' file` | **Delete** lines matching the pattern | `sed '/error/d' log.txt` |
| `sed -n '5,10p' file` | Print only lines **5 to 10** | `sed -n '5,10p' file.txt` |
| `sed 's|/|\\|g' file` | Replace **slashes** with backslashes | `sed 's|/|\\|g' paths.txt` |
| `sed 's/^/prefix /' file` | Add a prefix to the **beginning** of each line | `sed 's/^/LOG: /' file.txt` |

## 📊 3. Data Extraction with `cut`

| Command | Description | Example |
| :--- | :--- | :--- |
| `cut -d: -f1 /etc/passwd` | Cut by **delimiter** (`:`) and select field 1 | `cut -d: -f1 /etc/passwd` |
| `cut -d' ' -f2 file` | Cut by **space** delimiter | `cut -d' ' -f2 data.txt` |
| `cut -c1-5 file` | Cut by **character** position | `cut -c1-5 file.txt` |

## 📈 4. Text Statistics & Sorting

| Command | Description | Example |
| :--- | :--- | :--- |
| `wc -l file` | Count **lines** | `wc -l file.txt` |
| `wc -w file` | Count **words** | `wc -w file.txt` |
| `sort file` | Sort lines **alphabetically** | `sort names.txt` |
| `sort -n file` | Sort **numerically** | `sort -n numbers.txt` |
| `sort -r file` | Sort in **reverse** order | `sort -r names.txt` |
| `sort -k2 file` | Sort by the **second column** | `sort -k2 data.txt` |
| `uniq file` | Remove **consecutive** duplicate lines | `sort file.txt \| uniq` |
| `uniq -c file` | Count **occurrences** of each line | `sort file.txt \| uniq -c` |

## 🔤 5. Basic Regular Expressions (BRE)

| Pattern | Description | Example |
| :--- | :--- | :--- |
| `^` | **Start** of line | `grep "^hello" file.txt` |
| `$` | **End** of line | `grep "world$" file.txt` |
| `.` | Any **single character** | `grep "h.llo" file.txt` |
| `*` | **Zero or more** of the previous character | `grep "ab*c" file.txt` |
| `[abc]` | Any **one** of the characters inside brackets | `grep "[aeiou]" file.txt` |
| `[^abc]` | Any character **except** those inside brackets | `grep "[^0-9]" file.txt` |
| `[a-z]` | Any character in the **range** | `grep "[a-z]" file.txt` |
| `\` | **Escape** a special character | `grep "\." file.txt` (literal dot) |

## 🚀 6. Extended Regular Expressions (ERE) - used with `grep -E`

| Pattern | Description | Example |
| :--- | :--- | :--- |
| `+` | **One or more** of the previous character | `grep -E "ab+c" file.txt` |
| `?` | **Zero or one** of the previous character | `grep -E "ab?c" file.txt` |
| `\|` | **OR** operator | `grep -E "cat\|dog" file.txt` |
| `()` | **Group** patterns | `grep -E "(ab)+" file.txt` |
| `{n}` | Exactly **n** repetitions | `grep -E "a{3}" file.txt` |
| `{n,m}` | Between **n and m** repetitions | `grep -E "a{2,4}" file.txt` |

## 🧠 7. Practical One-Liners

| Task | Command |
| :--- | :--- |
| Find the **top 10 largest files** | `du -ah /var/log \| sort -rh \| head -n 10` |
| Find the **most frequent IP** in a log | `grep -oE "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" access.log \| sort \| uniq -c \| sort -rn \| head` |
| **Remove empty lines** from a file | `sed '/^$/d' file.txt` or `grep -v "^$" file.txt` |
| **Replace all spaces** with underscores | `sed 's/ /_/g' file.txt` |
| **Count unique users** in a log | `cut -d' ' -f1 access.log \| sort \| uniq \| wc -l` |

**Step-by-step Breakdown:**

1.  **`du -ah /var/log`**
    *   `du`: Disk Usage (calculates the size of files and directories).
    *   `-a`: All (shows files as well as directories).
    *   `-h`: Human-readable (shows sizes in KB, MB, GB instead of bytes).
    *   `/var/log`: The directory we want to analyze.
    *   *What it does:* Lists every file and folder in `/var/log` along with its size.

2.  **`|` (Pipe)**
    *   Takes the output from the previous command and passes it as input to the next command.

3.  **`sort -rh`**
    *   `sort`: Sorts the lines.
    *   `-r`: Reverse (sorts from largest to smallest, instead of A-Z).
    *   `-h`: Human-readable (understands `1K`, `1M`, `1G` so that `15M` comes before `2.5M`).
    *   *What it does:* Sorts the list of files from the biggest to the smallest.

4.  **`head -n 10`**
    *   `head`: Shows the first lines of the output.
    *   `-n 10`: Shows only the first 10 lines.
    *   *What it does:* Grabs only the top 10 largest files.

**Final Result:** It shows the 10 largest files/directories in `/var/log`, sorted from biggest to smallest.

## 💡 Pro Tips
- **`grep`** is best for **searching**.
- **`sed`** is best for **replacing** or **deleting** lines.
- **`awk`** is best for **processing columns** and **calculations** (covered in the next level).
- Always test complex `sed` commands without `-i` first to see the output before modifying the file.

---
