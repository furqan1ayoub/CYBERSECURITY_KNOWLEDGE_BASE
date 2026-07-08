# Guided Pentest – Nmap Host Discovery

### Platform: TryHackMe

### Learning Path: Jr Penetration Tester

### Difficulty: ⭐⭐⭐☆☆

### Date Completed: 08-07-2026

## Overview

This room introduces one of the most important phases of active reconnaissance: **host discovery**. Before scanning ports or enumerating services, a pentester should first identify which systems are actually online. The room covers how Nmap discovers live hosts using ARP, ICMP, TCP, and UDP, when each technique should be used, and how network topology influences the discovery process.

---

## Key Concepts Learned

### Host Discovery

* Host discovery is the process of identifying which systems are online before performing port scans.
* Scanning only live hosts reduces scan time, unnecessary network traffic, and noise.
* A host is considered online whenever it responds to a discovery probe, even if the response is an error (such as TCP RST or ICMP Port Unreachable).

### Subnets and Network Segments

* A **subnet** is a logical group of IP addresses.
* A **network segment** is the physical network where devices communicate directly.
* Routers separate subnets and do not forward ARP broadcasts.
* Before selecting a discovery method, determine whether the target is on the same subnet or a remote network.

### ARP Host Discovery

* ARP resolves an IP address to its MAC address.
* ARP works **only on the local subnet**.
* It is the fastest and most reliable host discovery technique on internal networks because devices must respond to ARP to communicate.

### ICMP Host Discovery

Nmap supports multiple ICMP discovery methods:

* Echo Request (`-PE`)
* Timestamp Request (`-PP`)
* Address Mask Request (`-PM`)

Since many firewalls block ICMP Echo Requests, alternative ICMP message types may still reveal live hosts.

### TCP Host Discovery

Nmap can send TCP probes instead of ICMP.

* **TCP SYN Ping (`-PS`)** sends a SYN packet.
* **TCP ACK Ping (`-PA`)** sends an ACK packet.

Whether the target replies with **SYN/ACK** or **RST**, any valid response confirms the host is online.

### UDP Host Discovery

UDP has no handshake.

If a UDP packet reaches a closed port, the host commonly returns an **ICMP Port Unreachable** message, confirming that the system is alive.

### Target Specification

Nmap supports multiple ways to define targets:

* Single IP address
* Hostnames
* Multiple hosts in a list
* IP ranges
* CIDR subnets
* Input files using `-iL`

This flexibility makes it easy to scan anything from a single machine to an entire network.

### DNS Resolution

* Nmap performs reverse DNS lookups for discovered hosts by default.
* Reverse DNS can reveal useful hostnames such as domain controllers, mail servers, or file servers.
* DNS information should be treated as supporting evidence, not proof.
* DNS lookups can be disabled to speed up scans.

---

## Important Commands

```bash
# Host discovery only (no port scan)
nmap -sn <target>

# ARP discovery (same subnet)
sudo nmap -PR -sn <target>

# ICMP Echo discovery
sudo nmap -PE -sn <target>

# ICMP Timestamp discovery
sudo nmap -PP -sn <target>

# ICMP Address Mask discovery
sudo nmap -PM -sn <target>

# TCP SYN discovery
sudo nmap -PS22,80,443 -sn <target>

# TCP ACK discovery
sudo nmap -PA22,80,443 -sn <target>

# UDP discovery
sudo nmap -PU53,161,162 -sn <target>

# Scan targets from a file
nmap -iL targets.txt

# Display targets without scanning
nmap -sL <target>

# Disable DNS resolution
nmap -n <target>

# Force reverse DNS lookups
nmap -R <target>

# Use a specific DNS server
nmap --dns-servers <DNS_IP> <target>
```

---

## Important Tools

* **Nmap** — Industry-standard network reconnaissance tool used for host discovery, port scanning, service enumeration, and much more.
* **Wireshark** — Packet analyzer used to observe ARP, ICMP, TCP, and UDP traffic generated during host discovery.

---
## Why It Matters

Host discovery reduces unnecessary traffic, speeds up reconnaissance, and helps focus enumeration on systems that are actually online. Choosing the correct discovery method also improves the chances of finding hosts hidden behind firewalls or different network configurations.

## Important Ports

| Port | Service   | Usage                                    |
| ---- | --------- | ---------------------------------------- |
| 22   | SSH       | Common TCP discovery target.             |
| 53   | DNS       | Common UDP discovery target.             |
| 80   | HTTP      | Default TCP discovery port used by Nmap. |
| 161  | SNMP      | Frequently used for UDP discovery.       |
| 162  | SNMP Trap | Additional UDP discovery target.         |
| 443  | HTTPS     | Common TCP discovery target.             |

---

## Key Takeaways

* Perform host discovery before any port scan.
* Use **ARP** when scanning hosts on the same subnet.
* ARP broadcasts never cross routers.
* If ICMP is filtered, switch to TCP or UDP discovery.
* Different environments require different discovery techniques.
* `-sn` performs host discovery without scanning ports.
* Use `-sL` to verify scan targets before launching a scan.
* Disable DNS lookups with `-n` when speed is more important than hostname resolution.

---

## Pentester Mindset

Always start by asking **"Which hosts are actually alive?"** before asking **"Which ports are open?"** Select the discovery technique that matches your position on the network. On internal assessments, ARP is usually the best option. On remote networks, rely on ICMP, TCP, or UDP depending on firewall behavior. If one discovery method fails, don't immediately assume the host is offline—adapt your approach and try another technique.

---
## Common Mistakes

- Assuming a host is offline after one failed discovery method.
- Using ARP against remote networks.
- Skipping host discovery and scanning every IP directly.
- Treating reverse DNS as proof of a system's identity.	

## Final Thoughts

This room builds the foundation for all future Nmap scanning. Understanding how Nmap discovers live hosts, how subnets affect reconnaissance, and when to use different discovery techniques makes reconnaissance faster, more accurate, and significantly more efficient during real penetration tests.
