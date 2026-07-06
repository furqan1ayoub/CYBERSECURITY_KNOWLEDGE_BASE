# Passive Reconnaissance

### Platform: TryHackMe

### Learning Path: Jr Penetration Tester

### Difficulty: ⭐☆☆☆

### Date Completed: 06-07-2026

## Overview

This room introduced **Passive Reconnaissance**, where information is gathered from public sources without directly interacting with the target. The focus was on understanding how to collect intelligence safely before moving on to active reconnaissance.

## What I Learned

* Difference between Passive and Active Reconnaissance.
* Used **WHOIS/RDAP** to gather domain registration information.
* Queried DNS records using **dig** and **nslookup**.
* Learned common DNS records such as **A, AAAA, MX, TXT, CNAME, and SOA**.
* Discovered subdomains using **DNSDumpster** and **Certificate Transparency (crt.sh)**.
* Used **Shodan** to identify publicly exposed internet-facing assets.
* Understood how passive reconnaissance helps expand the attack surface while remaining difficult to detect.

## Key Takeaways

* Passive Recon is the first step of a penetration test.
* WHOIS provides domain ownership and registration details.
* DNS records reveal infrastructure such as IP addresses and mail servers.
* Subdomain discovery can uncover forgotten or hidden assets.
* Shodan provides valuable information about exposed services without actively scanning the target.

## Tools Covered

* WHOIS / RDAP
* dig
* nslookup
* DNSDumpster
* crt.sh
* Shodan

---

### Room Link

https://tryhackme.com/room/passiverecon

---

## Tags

`Passive Recon` `OSINT` `WHOIS` `DNS` `dig` `Shodan` `Subdomain Enumeration` `TryHackMe`
