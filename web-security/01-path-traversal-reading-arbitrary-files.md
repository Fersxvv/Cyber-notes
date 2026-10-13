# 📂 Path Traversal: Reading Arbitrary Files

> `$ cat summary.txt`
> The app takes a filename from the user, sticks it onto a folder path and reads it. With no checks, you can walk out of that folder with `../` and read **any file the server can read**.

## The setup (normal behaviour)
A shop site shows product images like this:
```html
<img src="/loadImage?filename=218.png">
```
- `filename` is a **user-controlled parameter**. That's the weak point.
- Images live in `/var/www/images/`
- The app does **base directory + filename**, then reads the file:
```
/var/www/images/218.png
```

## The attack
Request:
```
https://insecure-website.com/loadImage?filename=../../../etc/passwd
```
The app builds:
```
/var/www/images/../../../etc/passwd
```
Each `../` means **go up one folder**:

| Step | Where you are |
|---|---|
| Start | `/var/www/images/` |
| `../` | `/var/www/` |
| `../../` | `/var/` |
| `../../../` | `/` (root) |
| + `etc/passwd` | **`/etc/passwd`** |

The file actually read is `/etc/passwd`.

## Why `/etc/passwd`?
- It's a standard file on **Unix/Linux**
- It lists the **users registered on the server**
- It's always there, so it's perfect proof the attack works
- The same trick reads **any other file** the app has permission to read

## Windows
- Both `../` **and** `..\` work as traversal sequences
- Example:
```
https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini
```
- `win.ini` is the Windows "always exists" test file

## Key points
| # | Point |
|---|---|
| 1 | Root cause: **user input used directly in a file path** with no validation |
| 2 | `../` = up one directory. Stack them to reach root `/` |
| 3 | Extra `../` doesn't break it. You can't go higher than root, so extras are ignored. Handy when you don't know how deep the base folder is |
| 4 | Linux target: `/etc/passwd`. Windows target: `windows\win.ini` |
| 5 | Windows accepts **both** slash directions |

## Quick-fire revision
| Question | Answer |
|---|---|
| What does `../` do? | Moves up one directory |
| What's the vulnerable part of `/loadImage?filename=218.png`? | The `filename` parameter |
| Base dir `/var/www/images/`: how many `../` to reach root? | 3 |
| Classic Linux file to test with? | `/etc/passwd` |
| Classic Windows file to test with? | `windows\win.ini` |
| Which traversal sequences work on Windows? | `../` and `..\` |
| What's in `/etc/passwd`? | Details of users registered on the server |

[⬅ Back to Web Security](./README.md)
