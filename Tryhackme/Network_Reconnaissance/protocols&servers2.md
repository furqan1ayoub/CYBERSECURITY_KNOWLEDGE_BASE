# Protocols & Servers 2
### Platform: TryHackMe
### Learning Path: Jr Penetration Tester
### Difficulty: ⭐⭐☆☆☆
### Date Completed: 07-07-2026

---

## Overview

This room builds on the first Protocols & Servers room by explaining how attackers exploit insecure protocols and how modern technologies protect network communication. It covers common attacks like sniffing, Man-in-the-Middle (MITM), and password attacks before introducing TLS, SSH, and modern authentication mechanisms.

---

## Key Concepts Learned

### Sniffing Attack
- Captures network traffic using packet capture tools.
- Can expose usernames, passwords, emails, and files when protocols use cleartext communication.
- Common tools:
  - Tcpdump
  - Wireshark
  - Tshark
- Primary mitigation: TLS and SSH.

---

### Man-in-the-Middle (MITM)
- Attacker secretly intercepts communication between two parties.
- Can read, modify, or forward messages.
- Common techniques:
  - ARP Spoofing
  - DNS Spoofing
  - Rogue Access Points
  - BGP Hijacking
- Modern protections:
  - HTTPS
  - HSTS
  - Certificate Transparency
  - Certificate Pinning

---

### Transport Layer Security (TLS)
- Encrypts network communication.
- Protects against sniffing and MITM attacks.
- Upgrades insecure protocols such as:
  - HTTP → HTTPS
  - FTP → FTPS
  - SMTP → SMTPS
  - POP3 → POP3S
  - IMAP → IMAPS
- Modern standard: TLS 1.3.

---

### Secure Shell (SSH)
- Secure replacement for Telnet.
- Provides:
  - Authentication
  - Confidentiality
  - Integrity
- Supports:
  - Password authentication
  - Public key authentication
  - Multi-factor authentication
- Also provides secure file transfer through SFTP.

---

### Password Attacks
Different attack techniques include:
- Password Guessing
- Dictionary Attack
- Brute Force
- Credential Stuffing
- Password Spraying
- Hybrid Attack

Hydra can automate password testing against many network services such as SSH, FTP, HTTP, SMTP, POP3, and IMAP.

---

## Important Tools

| Tool | Purpose |
|------|---------|
| Tcpdump | Packet capture (CLI) |
| Wireshark | Packet analysis (GUI) |
| Bettercap | MITM attacks |
| mitmproxy | HTTP/HTTPS interception |
| Hydra | Online password attacks |
| Hashcat | Offline hash cracking |
| John the Ripper | Offline hash cracking |

---

## Important Ports

| Protocol | Default Port | Secure Port |
|----------|-------------:|------------:|
| HTTP | 80 | 443 |
| FTP | 21 | 990 / 22 |
| SMTP | 25 | 465 / 587 |
| POP3 | 110 | 995 |
| IMAP | 143 | 993 |
| Telnet | 23 | 22 (SSH) |

---

## Key Takeaways

- Sniffing attacks rely on unencrypted communication.
- MITM attacks can intercept and modify traffic.
- TLS encrypts application-layer protocols and protects confidentiality and integrity.
- SSH securely replaces Telnet for remote administration.
- Public key authentication is preferred over password authentication.
- Hydra performs online password attacks against live services.
- Hashcat and John the Ripper crack password hashes offline.
- Multi-Factor Authentication is one of the strongest defenses against password attacks.

---

## Final Thoughts

This room shifted the focus from understanding how protocols work to understanding how attackers exploit insecure communication and how modern security mechanisms defend against those attacks. It reinforced the importance of encryption, secure authentication, and proper protocol configuration—fundamental concepts that every penetration tester should understand before moving on to more advanced topics.