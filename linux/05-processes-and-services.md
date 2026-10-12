# ⚙️ Processes and Services

> `$ cat summary.txt`
> Every running program is a process. Services are processes that run in the background, usually from boot.

## Seeing what's running
| Command | What it does |
|---|---|
| `ps` | Processes in your current terminal |
| `ps aux` | Every process on the system |
| `ps aux \| grep ssh` | Finds processes with "ssh" in the name |
| `top` | Live view, sorted by CPU (`q` to quit) |
| `htop` | Nicer version of `top` (may need installing) |
| `pgrep firefox` | Gives the PID of a process by name |

## Reading `ps aux`
| Column | Meaning |
|---|---|
| `USER` | Who's running it |
| `PID` | Process ID: the number used to control it |
| `%CPU` / `%MEM` | How much it's using |
| `STAT` | State (`R` running, `S` sleeping, `Z` zombie) |
| `COMMAND` | What actually started it |

## Stopping processes
| Command | What it does |
|---|---|
| `kill 1234` | Asks process 1234 to stop politely (SIGTERM, 15) |
| `kill -9 1234` | Forces it to stop now (SIGKILL, 9) |
| `pkill firefox` | Kills by name |
| `Ctrl+C` | Stops whatever's running in the terminal |

Try a normal `kill` first. `-9` gives the program no chance to clean up.

## Background and foreground
| Command | What it does |
|---|---|
| `command &` | Runs it in the background |
| `Ctrl+Z` | Pauses the current job |
| `jobs` | Lists your background jobs |
| `bg` / `fg` | Resumes a job in the background / brings it to the front |

## Services (systemd)
| Command | What it does |
|---|---|
| `systemctl status ssh` | Is it running? |
| `sudo systemctl start ssh` | Start it now |
| `sudo systemctl stop ssh` | Stop it now |
| `sudo systemctl enable ssh` | Start it automatically at boot |
| `sudo systemctl disable ssh` | Don't start it at boot |
| `systemctl list-units --type=service` | Lists all services |

## Why it matters for security
- An unknown process running as root is one of the first signs a machine has been compromised.
- Every service you don't need is extra attack surface. If it isn't used, disable it.
- `ps aux` can show passwords that were typed straight into a command, which is why you shouldn't do that.

## ❌ Mistakes I made
| What I thought | What's actually true |
|---|---|
| `kill` always kills the process | By default it **asks** it to stop. The program can ignore it. `kill -9` can't be ignored. |
| `start` means it'll run after a reboot too | `start` is only for now. You need `enable` for it to come back after reboot. |

[⬅ Back to Linux](./README.md)
