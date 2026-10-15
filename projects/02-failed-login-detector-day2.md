# Password Failed Login Detector - Day 2

> **Day 2.** Recap of day 1, then starting `awk`. Mistakes left in on purpose, corrections underneath.

## Recap questions from yesterday (answered without looking at notes)

**1a. What is in the test file?**
- 2 failed logins for john123 (wrong password), 2 failed logins from invalid users, 1 where they did log in, and 1 line that isnt about logins.
- **Correction:** the last line isnt a SSH command. Its a message sshd logged when someone disconnected. Commands are what you type. Log lines are what the system writes down.

**1b. Why did I make my own?**
- So I can test it offline on the ships horrible wifi.
- The reason I missed (twice): because I wrote every line myself I ALREADY KNOW THE RIGHT ANSWER. I know there are 4 failed logins and which IPs they came from. So when the script prints something I can check if its right. On a real log with 40,000 lines I would have no idea. That is why test data exists.

**2. grep vs grep -c**
- `grep "..."` looks for a word/words in the file and prints the whole line.
- `grep -c "..."` counts the lines that match and just prints the number. (`--count` is the long version)

**3. Where is the IP?**
- Counting backwards the IP is always 4th from the end. The extra words "invalid user" come at the START of the line so the END of the line never changes.

---

## awk

Source: <https://www.geeksforgeeks.org/linux-unix/awk-command-unixlinux-examples/>

**What is awk and how does it help in looking through an auth.log file?**

awk is used in MAC and linux terminal. It is a pattern scanning and processing language. Used mainly for text processing, Data extraction and reporting.

It scans input files line by line.

grep finds whole LINES. awk can pull out a single word from a line by its position. In awk words are called fields. Field 1 is the first word, field 2 is the second and so on.

**Where is it commonly used?**

Mainly used in shell scripts and Data processing pipelines.

**Examples (from geeksforgeeks)**

```
awk '{print}' employee.txt
```

There is no pattern given to awk so the action is applied to all the lines. Without a pattern it is just gonna print all the lines of the file.

```
awk -F' ' '{print $1, $4}' employee.txt
```

- `-F` is for a custom field separator. (The default is spaces anyway so for my file I dont need it)
- `$1` is the first column, in this case that is the first name
- `$4` is the fourth column, in this case that is the salary

### Where I went wrong

- `{print $1, $4}` is NOT a pattern. The bit inside `{ }` is the ACTION (what to do). A pattern goes BEFORE the braces to choose which lines the action runs on. (still need to look up "awk pattern action")
- My notes app put in curly quotes. Terminal doesnt understand them. Straight quotes only.
- Use SINGLE quotes for awk. In double quotes the shell tries to read `$1` itself before awk even sees it.
- awk is lowercase. Not Awk.

---

## Step 6a - Print field 1 of every line

**My prediction:** the month of the log
- (Too vague. Should have said "Aug" six times. A vague prediction cant really be wrong so it doesnt test anything)

**What I ran (all my attempts)**

```
% awk {$1} test_auth.log
% awk "{$1}" test_auth.log
% awk -F' ' '{print , $1}' test_auth.loh
awk: syntax error at source line 1
 context is
{print >>>  , <<<
awk: illegal statement at source line 1
awk: illegal statement at source line 1
% awk -F' ' '{print , $1}' test_auth.log
awk: syntax error at source line 1
 context is
{print >>>  , <<<
awk: illegal statement at source line 1
awk: illegal statement at source line 1
% awk -F' ' '{print $1}' test_auth.log
Aug
Aug
Aug
Aug
Aug
Aug
```

The last one worked.

**Why did the others fail?**

1. `awk {$1} test_auth.log` - No print so it didnt do anything. ALSO no quotes. Without quotes the shell gets its hands on `{$1}` before awk sees it. Always wrap awk code in single quotes.
2. `awk "{$1}" test_auth.log` - No print. (Still need to test what double quotes do to `$1`)
3. `awk -F' ' '{print , $1}'` - The comma after print broke the syntax so it didnt run.
4. `test_auth.loh` - I misspelled .log.

---

## Still to do

- [ ] **Double quotes test.** Predict what `awk "{print $1}" test_auth.log` prints, run it, then compare with `awk '{print $1}' test_auth.log`. Explain why they are different.
- [ ] **Step 6b - print the LAST word of every line.**
  - Find the special built in name awk uses for "the number of fields on this line"
  - Use it to print the last field instead of a numbered one
  - Predict all 6 first. Lines 1-5 end the same. Line 6 doesnt.

[⬅ Back to projects](./README.md)
