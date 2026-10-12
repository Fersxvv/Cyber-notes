# 🔢 IP Addresses and Subnetting

> `$ cat summary.txt`
> Every device on a network needs an address. Subnetting is how one big network gets split into smaller ones.

## IPv4 basics
- 32 bits, written as 4 numbers (octets) from 0 to 255: `192.168.1.10`
- Part of the address is the **network**, the rest is the **host** (the device)
- The **subnet mask** or **CIDR** number says where that split is

## Special ranges
| Range | What it is |
|---|---|
| `10.0.0.0/8` | Private |
| `172.16.0.0/12` (172.16.x.x – 172.31.x.x) | Private |
| `192.168.0.0/16` | Private |
| `127.0.0.0/8` | Loopback: the machine talking to itself (`127.0.0.1`) |
| `169.254.0.0/16` | APIPA: what a device gives itself when DHCP fails |

**Private** addresses don't work on the public internet. Your router uses **NAT** to share one public IP between all the private devices behind it.

## CIDR cheat sheet
The `/number` is how many bits are the network part. **Bigger number = smaller network.**

| CIDR | Subnet mask | Total addresses | Usable hosts |
|---|---|---|---|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /30 | 255.255.255.252 | 4 | 2 |

**Usable hosts = total − 2**, because every subnet loses two addresses:
- The **first** is the network address (the subnet's name)
- The **last** is the broadcast address (sends to everyone in the subnet)

## Worked example: `192.168.1.0/26`
A /26 has 64 addresses, so the /24 splits into 4 subnets:

| Subnet | Network address | Usable range | Broadcast |
|---|---|---|---|
| 1 | 192.168.1.0 | .1 – .62 | .63 |
| 2 | 192.168.1.64 | .65 – .126 | .127 |
| 3 | 192.168.1.128 | .129 – .190 | .191 |
| 4 | 192.168.1.192 | .193 – .254 | .255 |

## IPv6 (quick version)
- 128 bits, written in hex: `2001:0db8::1`
- So many addresses that NAT mostly isn't needed
- `::1` is the IPv6 loopback

## Commands to try
| Command | What it does |
|---|---|
| `ip a` | Shows your IP addresses (Linux) |
| `ifconfig` | Same, older command (also works on macOS) |
| `ping 8.8.8.8` | Checks you can reach an address |

## ❌ Mistakes I made
| What I thought | What's actually true |
|---|---|
| A /24 gives you 256 devices | 256 **addresses**, but only **254 usable**. Network and broadcast addresses can't be given to devices. |
| All 172.x.x.x addresses are private | Only **172.16 to 172.31** are private. 172.15.x.x or 172.32.x.x are public. |
| A bigger CIDR number means a bigger network | **The opposite.** /30 is tiny (2 hosts), /8 is huge. |
| The IP my laptop shows is the one websites see | My laptop has a **private** IP. Websites see my router's **public** IP, thanks to NAT. |

[⬅ Back to Networking](./README.md)
