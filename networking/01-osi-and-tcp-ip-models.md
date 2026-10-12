# 🧱 OSI and TCP/IP Models

> `$ cat summary.txt`
> Two ways of splitting networking into layers, so you can work out where a problem (or an attack) is happening.

## The OSI model (7 layers)
Numbered from the **bottom** up.

| # | Layer | What it does | Example | Unit of data |
|---|---|---|---|---|
| 7 | Application | What the user's program talks to | HTTP, DNS, SSH | Data |
| 6 | Presentation | Formatting, encryption, compression | TLS, JPEG | Data |
| 5 | Session | Opens, manages and closes conversations | Sessions, sockets | Data |
| 4 | Transport | Gets data between apps, using ports | TCP, UDP | Segment / datagram |
| 3 | Network | Gets data between networks, using IP addresses | IP, ICMP, routers | Packet |
| 2 | Data Link | Gets data across one local network, using MAC addresses | Ethernet, switches | Frame |
| 1 | Physical | The actual cables and signals | Cables, Wi-Fi radio, hubs | Bits |

**Memory trick (bottom to top):** **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way

## The TCP/IP model (4 layers)
This is what the internet **actually runs on**. OSI is the teaching model.

| TCP/IP layer | Matches OSI layers |
|---|---|
| Application | 7, 6, 5 |
| Transport | 4 |
| Internet | 3 |
| Network Access (Link) | 2, 1 |

## Encapsulation
When you send data, each layer wraps it with its own header on the way **down**. The other end unwraps it on the way **up**.

`Data → + TCP header (segment) → + IP header (packet) → + Ethernet header (frame) → bits on the wire`

## Why it matters for security
Attacks happen at different layers:
- **Layer 2:** ARP spoofing, MAC flooding
- **Layer 3:** IP spoofing, ping floods
- **Layer 4:** SYN floods, port scanning
- **Layer 7:** phishing sites, SQL injection

Knowing the layer tells you what kind of defence you need.

## ❌ Mistakes I made
| What I thought | What's actually true |
|---|---|
| Layer 1 is Application because it's at the top of the list | Layers count from the **bottom**. Physical is 1, Application is 7. |
| A switch and a router are the same thing | A **switch** connects devices on one network (layer 2, MAC addresses). A **router** connects different networks (layer 3, IP addresses). |
| The internet runs on the OSI model | It runs on **TCP/IP**. OSI is a reference model for learning and troubleshooting. |

[⬅ Back to Networking](./README.md)
