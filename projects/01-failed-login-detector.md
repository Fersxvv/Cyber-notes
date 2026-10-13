# Password Failed Login Detector

> **FIRST ATTEMPT** (contains errors, see corrections in [Update from Claude](#update-from-claude) below)

## What do I need to research?

I need to know where to find the name and locations of files where it records SHH login attempts on either Debian and/or Ubuntu (either could work, or both). (Could look into other OSs in the future.)

I also need to find out what a failed password line looks like. (This will show what I need to grab (greb), as it will help in the coding process.)

**The file path**
- The file path is mainly going to be `/var/log/auth.log`

**The file name**
- That is going to be the last name in the path (`auth.log`).

**What happens if it's empty or missing?**
- Well then that's going to be a `LogLevel VERBOSE` response, meaning something is wrong with the system, not with the code. (In my research it says, I quote: "setting LogLevel VERBOSE in the /etc/ssh/sshd_config file and restarting the rsyslog service.")

**What does a failed password line look like?**
- From my research it looks something like this:

```
<month> <day> <time> <Hostname> ssh[<PPID>]: Failed Password for <Username> from <Source IP> port <Port> ssh2
```

For example, if I was to fill this in it would be something like this:

```
January 12 12:43:32 localhost ssh[100000] Failed password for Fersx from 255.19.13.205 port 8080 ssh2
```

### A line from a real log

I am going to find a real SHH failed password line. I will link what I find and the source I found it from. I will also dissect it word by word to show I know what I am talking about (well, what I know).

```
Aug 23 03:47:15 centos7 sshd[3283]: Failed password for invalid user guest from 193.201.224.218 port 13181 ssh2
Aug 23 03:47:16 centos7 sshd[3283]: pam_unix(sshd:auth): check pass; user unknown
```

Source: https://bash-prompt.net/guides/using-logs-1/

**The start is the base you see every time you use terminal.**

```
Aug 23 03:47:15 centos7 sshd[3283]:
```

This is the month, date, time, the username (well, the hostname) and also the SSH code.

**The next is the failed login request.**

```
Failed password for invalid user guest
```

This is saying someone or yourself has entered the wrong password for the user guest and/or user.guest.

**The line that you could use for malicious and/or not.**

This is the IP that the intruder used, and/or the host is on, to log into the SSH login page.

```
193.201.224.218 port 13181 ssh2
```

This is showing the IP address and the port they are using to log in from. You could ping this to see if they are vulnerable.

---

All this above is my own research using google.com and other websites (they are linked in the notes if used).

This could be all bullshit I am trying here.

---

## Update from Claude

> Claude is my tutor in this, not my pal with the cheat sheet.

### Where I went wrong

**Ping them to see if they are vulnerable**

Claude has told me this is an awful message.
- `ping` only does something if the machine is on.
- Defenders never touch the attacker's machine.

**I misread a port**

Port `13181` is the attacker's own source port. It's pretty much a random number that the attacker's machine picked. The port they were attacking was `22`, as that is the main port for SSH.

**I missed the most important thing in my own example**

"Invalid user" means that the username doesn't exist on the system. I need to count the words before the IP address in each line.

**My missing file answer is wrong**

`LogLevel VERBOSE` controls how much detail gets logged. It has NOTHING to do with whether the file exists.

### 2 questions Claude gave me to research

#### 1. Why might newer Debian systems not have auth.log at all? (search for "journalctl")

Website used: https://man7.org/linux/man-pages/man1/journalctl.1.html

`journalctl` is used to print the log entries stored in the journal (`systemd-journald.service(8)` and `systemd-journal-remote.service(8)`).

If it is called without parameters, it will show the contents of the journal that are accessible to the user calling it. (It will start with the oldest entry that has been recorded.)

To answer the main question: in Debian 12 and newer (this is only a main feature if the installer is using a minimal and/or base installation), the traditional `rsyslog` system isn't installed by default anymore, so the `auth.log` file never gets made. Instead they changed to `systemd-journald` as the primary logging daemon. This is where it stores all the system and service logs, including auth events, in a central binary journal.

To access these you would need to use the `journalctl` command instead of reading the files in `/var/log/`.

Common `journalctl` commands (I will test these when I get a Debian VM installed):

| What | Command |
|---|---|
| View all the logs | `journalctl` |
| View SSH auth logs | `journalctl -u ssh.service` |
| View recent boot logs | `journalctl -b` |
| View logs in real time | `journalctl -f` |

If we really wanted the traditional `/var/log/auth.log` back, we would need to install `rsyslog`.

> `fail2ban` is NOT a log viewer. It reads the auth logs, finds IPs that keep failing and bans them. It's basically a pro version of this project.

#### 2. Why can't a normal user read the file?

`auth.log` is owned by `root` and its group is `adm`. If you ran `ls -l /var/log/auth.log` it would show something like:

```
-rw-r----- root adm
```

That means root can read and write it, the `adm` group can only read it, and everyone else gets nothing.

So a normal user who isn't root or in the `adm` group can't read it. You would need to use `sudo` or get added to the `adm` group.

---

### LogLevel VERBOSE (not part of the questions above, this is extra research)

Websites I used for research:
- https://sematext.com/blog/logging-levels/
- https://medium.com/israeli-tech-radar/do-you-know-the-log-verbosity-basics-7bd95265940b

From what I have read, verbose logging for a normal user is typically disabled by default, because it makes excessive data that increases the output of the user. For example, it increases storage, ingestion and operational costs. It also creates significant information noise.

#### What really is LogLevel?

This is a question I am asking myself to get a better understanding of it.

Verbose logging is a controlled troubleshooting state that is temporarily enabled for specific scopes (for example a single user and/or a service), rather than a default setting for basic and general consumption.

LogLevel (or log severity) is a label that is linked to a log entry. These can be categorised into 6 main levels:

| Level | What it means |
|---|---|
| **Trace** | Mainly used for deep debugging. Step-by-step execution detail. Not commonly used by a single user. |
| **Debug** | Detailed diagnostic information. Mainly used for dev projects and troubleshooting on your system or something else. (I don't know, couldn't find anything more on that.) |
| **Info** | Pretty much a thumbs up to say that the system is working and operating as expected. Records the basic events. |
| **Warn** | Alerts that some unexpected behaviour happened that did not prevent the system from continuing, but may indicate problems later down the line. |
| **Error** | A specific operation failed. This doesn't mean that it closes or disables the application. It could still be running. |
| **Fatal / Critical** | A MASSIVE BIG BOOM BOOM error happened that prevented the system or application from running or functioning. |

We can mitigate these by setting specific log level thresholds. We can filter out the less important logs (like Debug and Trace, or even Info) and keep Error and Fatal captured all the time, potentially triggering alerts.

> Note: these are general app log levels. sshd has its own list, check `man sshd_config`.

### What does `ls -l` do?

`ls -l` is a basic Linux command. (Yes I know, basic. I completely forgot about it and I used it every time in OverTheWire and TryHackMe.)

It displays files and directories in a long format, providing metadata for each entry.

### What is the adm group on Debian?

The `adm` group on Debian is a system group, mainly used for system monitoring. It allows users to read `/var/log` and use `xconsole`.

### What does `sudo` do?

(Again, I have used this countless times, but it's better to know than to guess.)

`sudo` is short for "superuser do". It allows authorised users to execute commands with extra security privileges.

Examples of `sudo` commands:

```
sudo apt update
sudo -u "user" "the command"
```

### Smaller errors I missed

- It is **SSH**, not SHH. I misspelled that countless times.
- The time, date and month isn't what's in the terminal, it's what's seen in the log file. The log's header.
- CentOS doesn't use `auth.log`, it uses `/var/log/secure`.

---

## What was I doing?

I needed to make a fake log file on my Mac so my script has something to read. 

### Making the project folder

Commands I used:

```
cd documents
mkdir Password-test2
ls -l
cd password-test2
pwd
```

`pwd` prints what folder you are currently in. This is your proof that you are in the right place.

### Where I went wrong

- I made `Password-test2` and then did `cd password-test2`. This only worked because the Mac ignores capital letters in folder names. **LINUX DOES NOT.** On Linux `Password-test` and `password-test` are 2 completely different folders. My script will run on Linux in the end, so I need to treat case like it matters every time.
- Commands are lowercase. `cd`, not `Cd`. (Watch out for auto capitals.)
- Paste straight from terminal. Don't type it out again from memory. I pasted the wrong commands once and it didn't match the output. A wrong record of what you typed is worse than no record.
- Show the output, not just the commands. The output is the proof.
- Folder name should be the project name (lowercase, hyphens, no spaces) because this will become the GitHub repo. I kept mine as it is for now.

### Making the file

```
nano test_auth.log
```
`nano` is a text editor in the terminal. The shortcuts to save and exit are at the bottom of the screen.

```
cat test_auth.log
```
Prints what's in the file, to prove the line is actually in there.

### My test file (final version)

```
Aug 23 03:47:32 Test sshd[5322]: Failed password for john123 from 193.201.224.218 port 8080 ssh2
Aug 23 03:54:31 Test sshd[5323]: Failed password for john123 from 193.201.224.218 port 9864 ssh2
Aug 23 04:01:12 Test sshd[5324]: Failed password for invalid user pig523 from 192.212.132.213 port 8767 ssh2
Aug 23 04:05:37 Test sshd[5325]: Failed password for invalid user Tom123 from 192.212.132.213 port 5334 ssh2
Aug 23 04:19:21 Test sshd[5326]: Accepted publickey for john123 from 193.201.224.218 port 9875 ssh2
Aug 23 06:47:52 Test sshd[5327]: Received disconnect from 203.0.113.42 port 44120:11: Bye Bye [preauth]
```

#### What is in it and why

- 2 failed logins for a real user (`john123`) from the SAME IP, so the script has a repeat offender to catch.
- 2 failed logins for invalid users (`pig523` and `Tom123`).
- 1 successful login (`Accepted publickey`).
- 1 line that has nothing to do with logins (`Received disconnect`), so I can check the script ignores it.

#### Mistakes I made building the lines (took me loads of goes)

- My first line had NO header. Every line needs the timestamp, hostname and `sshd[PID]:` at the start.
- I wrote "valid user". That doesn't exist. Only invalid users get extra words. A real user is just `Failed password for root from...`
- I put port 22. **WRONG.** The port in the log is the attacker's random source port. 22 is the port they are attacking. (I literally had this in my notes already.)
- I used `192.168.1.1`. That is a private address (normally a home router). An attacker from the internet won't have one of those.
- I used `194.312.332.213`. Not a real IP. Each of the 4 numbers in an IPv4 address can't go over 255.
- `Invalid User pig523 from...` was the wrong type of line AND the wrong case. The proper failed line is `Failed password for invalid user pig523 from...` (all lowercase for "invalid user").
- Then I overcorrected and made EVERYTHING lowercase. Real logs use capitals for `Failed`, `Accepted` and `Received`. Searching is case sensitive, so getting this wrong breaks the search.
- Same PID on every line. Each new SSH connection gets its own process, so it gets its own PID.
- Typos: `shh2`, `Recived`, `form`. Typos in test data matter because the script searches for EXACT text.
- Port 8080 is a well-known port (web servers). A source port is normally a random high number. (Still to look up: "ephemeral port range".)

#### Lesson

When I rushed and sent stuff back every 2 minutes, I made the same mistakes 3 times. When I slowed down and checked line by line against my notes, I got it right first time.

---

## Human error or attacker?

**Question:** `john123` failed twice from `.218` and then got in. If it was `Accepted password`, what does that mean?

It could be human error (someone fat-fingers their own password twice, then gets it right). A good analyst always thinks about the boring explanation.

**BUT** it could also mean an attacker guessed the password and the account is now compromised.

How to tell the difference:
- **Volume and speed.** 5 failures in 10 seconds = a script. 2 failures in 7 minutes = probably a person.
- **A single IP doesn't mean innocent.** Loads of attackers hammer away from one IP.
- **Is `.218` an IP the user normally logs in from?** (Home or work = leans human error. A server in another country = leans attacker.)
- **What did the account do after it got in?** (Checking emails = normal. Making new users or downloading files = compromised.)

---

## grep

`grep` finds LINES that match. It can't cut a piece out of a line.

```
grep "Failed password" test_auth.log
```
Printed lines 1-4. (I predicted this right.)

```
grep "failed password" test_auth.log
```
Printed nothing, because `grep` is case sensitive. (Predicted this right too.)

**Why the quotes?**
- Without them, `grep` searches for `Failed` and thinks `password` is a file name. So you get an error (`No such file or directory`) AND results. (Still need to run this and see it myself.)

**`grep -i`**
- Ignores case. Good for human-written stuff where capitals don't matter, but can cause false positives where capitals mean something different (example: `token` and `TOKEN`).

**`grep -c` (or `--count`)**
- Counts the matching lines instead of printing them. On a real server the search could return 40,000 lines, so counting matters.
- `grep -c "Failed password" test_auth.log` gave me `4` (predicted right).

**One dash vs two dashes**
- Two dashes = a whole word (`--count`).
- One dash = single letters (`-c`). So `-count` would be read as `-c -o -u -n -t` all mashed together. This rule works almost everywhere in Linux.

It's **grep**. NOT greb.

---

## The big problem: where is the IP?

If I split each line into words (wherever there is a space) and number them:
- Line 1: the IP is word 11
- Line 3: the IP is word 13

Why? Line 3 has 2 extra words (`invalid user`) before the username. That pushes everything after it 2 places to the right.

So if the script always grabbed word 11, it would grab `pig523` from line 3. It would report a USERNAME as if it was an attacker's IP. That is the bug.

### The fix (worked this out myself)

- The IP always comes right before the word `port`.
- Counting from the END of the line, the IP is always 4th from the end (`ssh2` = 1, port number = 2, `port` = 3, IP = 4). Same on line 1 and line 3.
- Counting from the end doesn't care how many extra words are at the start.

My first idea was `grep "port"`, but that doesn't work because `grep` finds lines, not words. Every line has `port` in it, so it would just print the whole file.
