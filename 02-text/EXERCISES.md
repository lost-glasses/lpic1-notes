# 🧪 Text Processing - Exercises

This file contains practical exercises for grep, sed, awk, and other text processing tools.

## 📝 Exercise 1: Search with grep

Task: Create a file named log.txt with the following content, then use grep to find all lines containing the word "error".

```bash
echo -e "info: system started\nerror: disk full\nwarning: low memory\nerror: connection lost" > log.txt
grep "error" log.txt
```

Expected Output:
```text
error: disk full
error: connection lost
```

## 📝 Exercise 2: Replace with sed

Task: Replace all occurrences of "error" with "ERROR" in the log.txt file and display the result.

Solution:
```bash
sed 's/error/ERROR/g' log.txt
```

Expected Output:
```text
info: system started
ERROR: disk full
warning: low memory
ERROR: connection lost
```

## 📝 Exercise 3: Extract Columns with cut

Task: Create a file named users.txt with the following content, then use cut to display only the usernames (first column).

```bash
echo -e "alice:1001\ bob:1002\ carol:1003" > users.txt
cut -d: -f1 users.txt
```

Expected Output:
```text
alice
bob
carol
```

## 📝 Exercise 4: Sort and Count with sort and uniq

Task: Create a file named fruits.txt with the following content, sort it alphabetically, and count how many times each fruit appears.

```bash
echo -e "apple\nbanana\napple\ncherry\nbanana\napple" > fruits.txt
sort fruits.txt | uniq -c
```

Expected Output:
```text
3 apple
2 banana
1 cherry
```

## 📝 Exercise 5: Regex with grep -E

Task: Find all lines in fruits.txt that contain either "apple" or "cherry" using Extended Regex.

Solution:
```bash
grep -E "apple|cherry" fruits.txt
```

Expected Output:
```text
apple
apple
cherry
apple
```

## 📝 Exercise 6: Count Lines, Words, and Characters with wc

Task: Count the number of lines, words, and characters in log.txt.

Solution:
```bash
wc log.txt
```

Expected Output:
```text
4 12 85 log.txt
```

