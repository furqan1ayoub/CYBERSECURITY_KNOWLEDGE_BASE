# Guided Pentest – Nmap Basic Port Scans

### Platform: TryHackMe

### Learning Path: Jr Penetration Tester

### Difficulty: ⭐⭐⭐☆☆

### Date Completed: 09-07-2026

## Overview

This room builds on host discovery by introducing Nmap's core port scanning techniques. It explains how TCP Connect, TCP SYN, and UDP scans work, how Nmap interprets different port states, and how to control scan scope and performance using common command-line options. These concepts form the foundation for service enumeration during a penetration test.

---

## Key Concepts Learned

### Ports and Services

* An **IP address** identifies a host, while a **port** identifies a specific network service running on that host.
* Multiple services can run on the same host, but only one application can listen on a specific TCP or UDP port on the same IP address.
* Common examples:

  * HTTP → TCP 80
  * HTTPS → TCP 443
  * SSH → TCP 22
  * DNS → UDP/TCP 53

### Port States

Nmap reports ports in different states depending on the responses received.

* **Open** – A service is actively listening.
* **Closed** – No service is listening, but the host is reachable.
* **Filtered** – A firewall or filtering device prevents Nmap from determining the port state.
* **Unfiltered** – The port is reachable, but Nmap cannot determine whether it is open or closed (typically seen with ACK scans).
* **Open|Filtered** – Unable to distinguish between an open port and packet filtering.
* **Closed|Filtered** – Unable to distinguish between a closed port and packet filtering.

For most engagements, **Open**, **Closed**, and **Filtered** are the states encountered most frequently.

### TCP Flags

Understanding a few TCP flags makes it easier to understand how Nmap scans work.

* **SYN** – Starts a TCP connection.
* **ACK** – Acknowledges received packets.
* **RST** – Immediately resets or rejects a connection.
* **FIN** – Gracefully closes a connection.
* **PSH** – Requests immediate delivery of data to the application.
* **URG** – Marks urgent data (rarely encountered in practice).

### TCP Connect Scan (-sT)

* Completes the full TCP three-way handshake.
* Determines whether a port is open before immediately resetting the connection.
* Used automatically when raw packet privileges are unavailable.
* Default choice for unprivileged users.

### TCP SYN Scan (-sS)

* Sends a SYN packet and stops before completing the TCP handshake.
* If the port replies with SYN/ACK, Nmap immediately sends RST instead of ACK.
* Faster and less likely to appear in application logs than a full TCP connection.
* Requires root/sudo privileges because raw packets are used.
* Preferred TCP scan during penetration testing.

### UDP Scan (-sU)

* UDP is connectionless and has no handshake.
* Closed UDP ports typically return **ICMP Destination Unreachable (Type 3, Code 3)**.
* Open UDP ports often return no response, making UDP scans slower and sometimes inconclusive.
* Important for discovering services such as DNS and SNMP.

### Port Selection

Nmap allows precise control over which ports are scanned.

* Scan individual ports.
* Scan ranges of ports.
* Scan all 65,535 TCP ports.
* Scan only the most common ports for faster reconnaissance.

### Scan Performance

Nmap provides multiple ways to balance speed, accuracy, and stealth.

* Timing templates (`-T0` to `-T5`) control overall scan aggressiveness.
* Packet rate options (`--min-rate`, `--max-rate`) control packets sent per second.
* Parallelism options control how many probes Nmap performs simultaneously.
#### Choose your scan based on the engagement. During labs or CTFs, speed is often the priority. During real assessments, balance speed, accuracy, and stealth. Scan only the ports you need, and adjust timing to suit the environment. 

---

## Important Commands

```bash
# TCP Connect Scan (non-root)
nmap -sT <target>

# TCP SYN Scan (preferred)
sudo nmap -sS <target>

# UDP Scan
sudo nmap -sU <target>

# Scan specific ports
nmap -p22,80,443 <target>

# Scan a range of ports
nmap -p20-25 <target>

# Scan all TCP ports
nmap -p- <target>

# Scan the 100 most common ports
nmap -F <target>

# Scan the top 10 most common ports
nmap --top-ports 10 <target>

# Scan ports sequentially
nmap -r <target>

# Faster scan (commonly used in labs)
nmap -T4 <target>

# Slower, quieter scan
nmap -T1 <target>

# Limit packets per second
nmap --max-rate 100 <target>

# Increase concurrent probes
nmap --min-parallelism 100 <target>

# Both TCP & UDP SCANS 
sudo nmap -sS -sU -p T:22,23,25,80,443,U:53,69,123 TARGET

Because -p applies to both scan types when you combine -sS and -sU.

T: = TCP ports
U: = UDP ports

So Nmap knows exactly which ports belong to which protocol.
```

---

## Important Tools

* **Nmap** — Industry-standard network scanner used for host discovery, port scanning, service detection, and network enumeration.
* **Wireshark** — Packet analyzer useful for observing TCP handshakes, TCP flags, ICMP responses, and UDP scan behavior.

---

## Important Ports

| Port | Service               | Protocol  |
| ---- | --------------------- | --------- |
| 21   | FTP                   | TCP       |
| 22   | SSH                   | TCP       |
| 53   | DNS                   | TCP / UDP |
| 67   | DHCP Server           | UDP       |
| 69   | TFTP                  | UDP       |
| 80   | HTTP                  | TCP       |
| 123  | NTP                   | UDP       |
| 137  | NetBIOS Name Service  | UDP       |
| 138  | NetBIOS Datagram      | UDP       |
| 161  | SNMP                  | UDP       |
| 443  | HTTPS                 | TCP       |
| 445  | SMB                   | TCP       |
| 631  | IPP (Printing)        | TCP / UDP |
| 1434 | Microsoft SQL Monitor | UDP       |

> These ports appeared during the room or are commonly associated with the demonstrated scans.

---

## Key Takeaways

* Port scanning begins after confirming the target is online.
* TCP Connect Scan establishes a complete TCP connection.
* TCP SYN Scan identifies open ports without completing the handshake and is the preferred TCP scan when root privileges are available.
* UDP scanning is slower because many UDP services do not respond unless necessary.
* Different port states provide different information about the target and possible firewall behavior.
* Scan only the ports you need whenever possible.
* Timing templates provide an easy way to balance speed and stealth without manually tuning packet rates.

---

## Pentester Mindset

Port scanning is not about scanning everything as quickly as possible—it's about collecting reliable information efficiently. Begin with common ports, expand only when necessary, and always consider the environment you're assessing. Use SYN scans whenever possible, remember that UDP requires its own scan, and interpret filtered results as indicators of security controls rather than assuming ports are closed.

---

## Final Thoughts

This room establishes the core techniques used in almost every penetration test. Understanding how Nmap performs TCP and UDP scans, how to interpret port states, and how to adjust scan scope and performance provides a solid foundation for service enumeration and the more advanced scanning techniques covered in later Nmap modules.
