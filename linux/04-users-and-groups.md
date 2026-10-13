# 👥 Users and Groups

> `$ cat summary.txt`
> Every process and file belongs to a user. Groups let you give several users the same access at once.

## Key ideas
- **root** is the superuser (user ID 0) and can do anything.
- Normal users are limited, which is the point: a compromised normal account does less damage.
- **Groups** bundle users together so permissions can be given to a group instead of person by person.
- Every user has a **primary group** and can be in extra groups.

## Who am I?
| Command | What it does |
|---|---|
| `whoami` | Shows your username |
| `id` | Shows your user ID, group ID and all your groups |
| `groups` | Lists your groups |
| `who` | Shows who's logged in |

## The important files
| File | What's in it |
|---|---|
| `/etc/passwd` | Every user account (readable by everyone, no passwords in it) |
| `/etc/shadow` | Password hashes (root only) |
| `/etc/group` | Every group and its members |

A line in `/etc/passwd`:
```
user:x:1000:1000:User:/home/user:/bin/bash
```
`username : x (password is in shadow) : user ID : group ID : description : home folder : shell`

## Managing users (needs `sudo`)
| Command | What it does |
|---|---|
| `sudo adduser bob` | Creates a user with a home folder (Debian/Ubuntu, interactive) |
| `sudo useradd -m bob` | Creates a user (works on most distros) |
| `sudo passwd bob` | Sets or changes a password |
| `sudo userdel -r bob` | Deletes a user and their home folder |

## Managing groups
| Command | What it does |
|---|---|
| `sudo groupadd devs` | Creates a group |
| `sudo usermod -aG devs bob` | Adds bob to a group |
| `sudo gpasswd -d bob devs` | Removes bob from a group |

## `sudo` vs `su`
- `sudo command` runs **one command** as root, using **your** password.
- `su` switches to **another user** entirely (root by default), using **their** password.
- Who's allowed to use `sudo` is controlled by `/etc/sudoers`. Edit it only with `sudo visudo`.

## ⚠️ Gotchas I hit
- `usermod -G` **without** the `-a` replaces all of a user's groups instead of adding one. Always use `-aG`.
- Group changes only apply after logging out and back in.
- On macOS these commands mostly don't exist. Practise this in a Linux VM.

## Why it matters for security
- `sudo -l` shows what a user is allowed to run as root. It's one of the first things checked after getting into a system.
- Readable `/etc/shadow` = attackers can try to crack the password hashes.

[⬅ Back to Linux](./README.md)
