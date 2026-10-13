# 📁 Navigating the File System

> `$ cat summary.txt`
> How to move around a Linux system and know where you are.

## The layout
Linux has one tree that starts at `/` (called "root"). Everything lives under it: no `C:` drive like Windows.

| Path | What's there |
|---|---|
| `/` | The top of everything |
| `/home` | Users' personal folders (`/home/user`) |
| `/root` | The root user's home folder (not the same as `/`) |
| `/etc` | System config files |
| `/bin`, `/usr/bin` | Programs and commands |
| `/var/log` | Log files |
| `/tmp` | Temporary files, often wiped on reboot |

## Core commands
| Command | What it does |
|---|---|
| `pwd` | **P**rint **w**orking **d**irectory: shows where you are |
| `ls` | Lists what's in the current folder |
| `ls -l` | Long list: permissions, owner, size, date |
| `ls -a` | Shows hidden files (names starting with `.`) |
| `ls -la` | Both of the above |
| `cd folder` | Moves into a folder |
| `cd ..` | Moves up one level |
| `cd ~` or just `cd` | Goes to your home folder |
| `cd -` | Goes back to the last folder you were in |
| `cd /` | Goes to the top of the tree |

## Absolute vs relative paths
- **Absolute** starts with `/` and works from anywhere: `cd /var/log`
- **Relative** starts from where you are now: `cd log` only works if `log` is inside your current folder

## Shortcuts
- `~` = your home folder
- `.` = the folder you're in
- `..` = the folder above
- **Tab** autocompletes file and folder names. Use it constantly; it stops typos.

## Gotchas I hit
- Linux is **case-sensitive**: `Documents` and `documents` are different folders. (macOS lets you get away with it, Linux won't.)
- Folder names with spaces need quotes or a backslash: `cd "cyber notes"` or `cd cyber\ notes`

[⬅ Back to Linux](./README.md)
