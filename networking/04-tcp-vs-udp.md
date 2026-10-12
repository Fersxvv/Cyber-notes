# 🤝 TCP vs UDP

> `$ cat summary.txt`
> The two main ways data travels at the transport layer (layer 4). TCP is careful. UDP is fast.

## Side by side
| | TCP | UDP |
|---|---|---|
| Connection | Sets up a connection first | Just sends |
| Reliable? | Yes: lost data is resent | No: lost data stays lost |
| Order | Arrives in order | Can arrive in any order |
| Speed | Slower (more overhead) | Faster (less overhead) |
| Used for | Websites, SSH, email, file transfers | DNS lookups, streaming, gaming, voice calls, DHCP |

**Simple way to think about it:** TCP is recorded delivery with a signature. UDP is shouting across the room and hoping they heard.

## The TCP three-way handshake
How TCP opens a connection:

1. **SYN:** client → server: "Can we talk?"
2. **SYN-ACK:** server → client: "Yes, can you hear me?"
3. **ACK:** client → server: "Yes. Let's go."

Then data flows. To close: **FIN** (polite goodbye) or **RST** (hang up immediately).

## Why it matters for security
- **SYN flood:** attacker sends loads of SYNs and never finishes the handshake, so the server fills up with half-open connections and stops responding.
- **Port scanning:** `nmap -sS` sends a SYN and watches the reply:
  - SYN-ACK back = port **open**
  - RST back = port **closed**
  - Nothing back = probably **filtered** by a firewall
- UDP scans are slower and less reliable, because a closed UDP port often just stays silent.

## ❌ Mistakes I made
| What I thought | What's actually true |
|---|---|
| The handshake goes SYN, ACK, SYN-ACK | It's **SYN → SYN-ACK → ACK**. The server's reply combines its SYN with an ACK of yours. |
| UDP is unreliable, so it's the bad one | It's chosen **on purpose** when speed matters more than perfection. A video call is better with a dropped frame than with a pause while it resends. |
| UDP has no handshake, so it can't be attacked | UDP is used in big **amplification attacks**: small spoofed request, huge reply sent to the victim. |

[⬅ Back to Networking](./README.md)
