# 🚪 Common Ports and Protocols

> `$ cat summary.txt`
> An IP address gets data to the right machine. A port gets it to the right program on that machine.

## The basics
- Ports run from **0 to 65535**
- **0–1023** are "well-known" ports, reserved for standard services
- A service "listens" on a port and waits for connections

## Ports to know
| Port | Protocol | What it's for | Secure? |
|---|---|---|---|
| 20, 21 | FTP | File transfer | ❌ Plaintext |
| 22 | SSH | Secure remote login | ✅ |
| 23 | Telnet | Old remote login | ❌ Plaintext |
| 25 | SMTP | Sending email | ⚠️ Depends on setup |
| 53 | DNS | Turning names into IPs | ⚠️ Usually unencrypted |
| 67, 68 | DHCP | Handing out IP addresses | — |
| 80 | HTTP | Websites | ❌ Plaintext |
| 110 | POP3 | Downloading email | ❌ Plaintext |
| 143 | IMAP | Reading email on the server | ❌ Plaintext |
| 443 | HTTPS | Websites, encrypted | ✅ |
| 445 | SMB | Windows file sharing | ⚠️ Big target |
| 3306 | MySQL | Database | ⚠️ Shouldn't face the internet |
| 3389 | RDP | Windows remote desktop | ⚠️ Big target |

**Plaintext** means anyone watching the network can read it, passwords included.

## Commands to try
| Command | What it does |
|---|---|
| `ss -tuln` | Shows which ports are listening on your machine (Linux) |
| `netstat -an` | Older version, works on macOS too |
| `nmap localhost` | Scans your own machine for open ports |

> ⚠️ Only scan machines you own or have permission to scan.

## Why it matters for security
- An open port is a door. Attackers scan for open doors first.
- Old plaintext protocols (Telnet, FTP, HTTP) leak credentials.
- SMB (445) and RDP (3389) are behind loads of real-world attacks.

## ❌ Mistakes I made
| What I thought | What's actually true |
|---|---|
| DNS only uses UDP | Mostly UDP, but it uses **TCP** for big responses and zone transfers. |
| An open port means the machine is hackable | An open port just means something is listening. It's only a problem if that service is vulnerable or badly set up. |
| Moving SSH from port 22 to another port makes it secure | It cuts down on automated noise, but it's **not real security**. A port scan finds it in seconds. Strong keys and no password login are what secure it. |

[⬅ Back to Networking](./README.md)
