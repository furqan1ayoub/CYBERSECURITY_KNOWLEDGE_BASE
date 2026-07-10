# Nmap Advanced Port Scans

### Platform: TryHackMe

### Learning Path: Jr Penetration Tester (PT1)

### Difficulty: ⭐⭐⭐⭐☆

### Date Completed: 10-07-2026

---

## Overview

This room explores advanced Nmap scanning techniques that go beyond standard SYN and Connect scans. It introduces scans that manipulate TCP flags to infer port states, map firewall rules, and perform stealthier reconnaissance. It also covers evasion techniques such as spoofing, decoy scans, packet fragmentation, and methods for obtaining more detailed scan output.

---

## Key Concepts Learned

### Advanced TCP Flag Scans

* **Null Scan (`-sN`)** – Sends a TCP packet with no flags set. A closed port responds with **RST**, while no response is reported as **open|filtered**.
* **FIN Scan (`-sF`)** – Sends only the **FIN** flag. Behaves similarly to a Null Scan.
* **Xmas Scan (`-sX`)** – Sends **FIN + PSH + URG** flags simultaneously. Like Null and FIN scans, it relies on the absence of a response.
* **Maimon Scan (`-sM`)** – Sends **FIN + ACK**. Designed for certain older BSD-derived systems and rarely useful on modern networks.

### Firewall Discovery

* **ACK Scan (`-sA`)** is used to determine whether a firewall allows traffic to reach a port. It does **not** identify open services.
* **Window Scan (`-sW`)** is similar to an ACK scan but also examines the TCP Window field in RST responses. It is effective only on certain operating systems.

### Spoofing & Evasion

* **IP Spoofing** changes the source IP address of scan packets. Responses are sent to the spoofed IP, making this technique useful only in limited scenarios where responses can still be monitored.
* **MAC Spoofing** changes the attacker's MAC address on the local network while keeping the IP address unchanged.
* **Decoy Scans** mix the attacker's IP with multiple fake source IPs, making attribution more difficult.
* **Idle (Zombie) Scan** uses an idle third-party host to indirectly determine whether target ports are open by observing changes in the zombie's IP ID.

### Packet Evasion

* Packet fragmentation (`-f`, `-ff`) splits packets into smaller fragments in an attempt to bypass older firewalls or IDS implementations.
* `--data-length` appends random padding bytes to packets to make traffic appear less predictable.

### Scan Output

* `--reason` explains **why** Nmap classified a host or port in a particular state.
* `-v` and `-vv` increase scan verbosity.
* `-d` and `-dd` provide debugging output.

---

## Important Commands

```bash
# Null Scan
sudo nmap -sN TARGET

# FIN Scan
sudo nmap -sF TARGET

# Xmas Scan
sudo nmap -sX TARGET

# Maimon Scan
sudo nmap -sM TARGET

# ACK Scan
sudo nmap -sA TARGET

# Window Scan
sudo nmap -sW TARGET

# Custom TCP Flags
sudo nmap --scanflags URGACKPSHRSTSYNFIN TARGET

# IP Spoofing
sudo nmap -e eth0 -Pn -S SPOOFED_IP TARGET

# MAC Spoofing (LAN only)
sudo nmap --spoof-mac SPOOFED_MAC TARGET

# Decoy Scan
sudo nmap -D RND,RND,ME TARGET

# Idle (Zombie) Scan
sudo nmap -sI ZOMBIE_IP TARGET

# Fragment Packets
sudo nmap -f TARGET
sudo nmap -ff TARGET

# Append Random Data
sudo nmap --data-length 50 TARGET

# Explain Scan Results
sudo nmap --reason TARGET

# Verbose Output
sudo nmap -v TARGET
sudo nmap -vv TARGET
```

---

## Important Tools

* **Nmap** – Advanced network scanner for host discovery, port scanning, firewall analysis, and network reconnaissance.
* **Wireshark** – Packet analyzer used to observe how different Nmap scan types behave on the network.

---

## Important Ports

This room focuses on **scan techniques rather than specific ports**. The examples commonly use:

| Port | Service |
| ---: | ------- |
|   22 | SSH     |
|   25 | SMTP    |
|   80 | HTTP    |
|  443 | HTTPS   |

---

## Key Takeaways

* Null, FIN, and Xmas scans infer port states based on the **absence or presence of RST responses**.
* ACK and Window scans are designed to **analyze firewall behavior**, not discover services.
* Maimon scans are primarily of historical interest and are rarely effective today.
* IP spoofing is generally impractical unless scan responses can also be captured.
* MAC spoofing only affects the local network and does not hide the attacker's IP.
* Decoy scans make it harder to identify the real scanning host by mixing legitimate and fake source IPs.
* Idle scans demonstrate how protocol behavior can be exploited to gather information indirectly.
* Modern firewalls and IDS are generally resistant to simple fragmentation-based evasion techniques.
* `--reason` is valuable for understanding why Nmap reached its conclusions during a scan.

---

## Pentester Mindset

Choose scan types based on the objective. Use SYN scans for general port discovery, ACK or Window scans to understand firewall filtering, and advanced scans only when a target's behavior suggests they may provide additional information. Understanding the reasoning behind each scan is far more valuable than memorizing every TCP flag combination.

---

## Final Thoughts

This room builds on the fundamentals of port scanning by introducing specialized techniques for firewall analysis, stealth, and protocol manipulation. While many of these scans are less effective against modern systems, they provide valuable insight into how TCP works and how creative scanning methods have evolved over time.
