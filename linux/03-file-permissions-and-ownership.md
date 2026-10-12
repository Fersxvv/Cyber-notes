# 🔐 File Permissions and Ownership

> `$ cat summary.txt`
> Who can read, change or run a file. This is core security: wrong permissions are one of the most common ways systems get broken into.

## Reading `ls -l`
```
-rwxr-xr-- 1 george devs 1024 Oct 12 12:00 script.sh
```
| Part | Meaning |
|---|---|
| `-` | Type: `-` file, `d` folder, `l` link |
| `rwx` | **Owner** permissions |
| `r-x` | **Group** permissions |
| `r--` | **Everyone else** permissions |
| `george` | The owner |
| `devs` | The group |

## What the letters mean
| Letter | On a file | On a folder |
|---|---|---|
| `r` read | Read the contents | List what's inside |
| `w` write | Change the contents | Create/delete files inside |
| `x` execute | Run it as a program | Enter it with `cd` |

## Number format
Each letter has a value: **r = 4, w = 2, x = 1**. Add them up for each group.

| Number | Letters | Meaning |
|---|---|---|
| 7 | `rwx` | Full access |
| 6 | `rw-` | Read and write |
| 5 | `r-x` | Read and run |
| 4 | `r--` | Read only |
| 0 | `---` | Nothing |

Common ones:
- `755` → owner full, everyone else read and run (scripts, folders)
- `644` → owner read/write, everyone else read only (normal files)
- `600` → only the owner can read/write (private keys, secrets)

## Changing permissions
| Command | What it does |
|---|---|
| `chmod 755 script.sh` | Sets permissions with numbers |
| `chmod +x script.sh` | Makes a file runnable |
| `chmod u+w file` | Adds write for the owner (`u` owner, `g` group, `o` others, `a` all) |
| `chmod o-r file` | Removes read from everyone else |
| `chmod -R 755 folder/` | Applies to a folder and everything inside |

## Changing ownership
| Command | What it does |
|---|---|
| `sudo chown george file` | Changes the owner |
| `sudo chown george:devs file` | Changes owner and group |
| `sudo chgrp devs file` | Changes just the group |

## Why it matters for security
- `777` means **anyone can change or run it**. Almost never the right answer, and a red flag when you find it on a system.
- SSH private keys must be `600`, or SSH refuses to use them.
- Attackers look for files they can write to that get run by root. That's a classic privilege escalation path.

[⬅ Back to Linux](./README.md)
