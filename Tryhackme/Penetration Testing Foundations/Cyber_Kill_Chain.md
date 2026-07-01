# Cyber Kill Chain
### Platform: TryHackMe
### Learning Path: Jr Penetration Tester
### Difficulty: ⭐⭐⭐☆☆
### Date Completed: 01-07-2026

## Overview
This room explains how most cyber attacks are carried out from start to finish using the Cyber Kill Chain. It shows how attackers think during each phase of an attack and how defenders can interrupt the attack before the attacker reaches their final objective.

## What I Learned
-

1) Cyber Kill space is a fundamental framework which both pentesters as well as security analysts use to analyse security in the organizations.
2) For Security Analysts this framework helps them avoiding big attacks on the organization by interupting any stage in this chain.
3) This Chain is not compulsory or bit by bit followed instead this just provides a generanl way how attacks are carried out in stages and if they can stop the actions at any stage the attack fails
4)  The most damage occurs in the last phase; however, companies can protect themselves against such damage if they interrupt the chain in any of its earlier stages. 
5)  This framework almost cover every attack from small attacks like xss to large attacks like ransomwares ( though stages can be skipped/combined).
6) The reverse shell isn't the goal—it's just a way to continue the attack. The real objective is whatever the attacker originally wanted: steal data, deploy ransomware, move through the network, or disrupt systems. That's why this final stage is called Actions on Objectives: it's where the attacker actually accomplishes their mission.

## Key Concepts

### Reconnaissance
- Gather information about the target.
- Find possible attack surfaces and weaknesses.
- Can be Passive (OSINT) or Active (Nmap, scanning, etc.).

### Weaponisation
- Prepare the payload based on the information gathered.
- Could be malware, a malicious Office document, web shell, exploit, etc.

### Delivery
- Get the payload to the victim.
- Common methods include phishing emails, malicious links, USB devices and social engineering.

### Exploitation
- Successfully abuse a vulnerability or weakness.
- Examples include SQL Injection, XSS, weak passwords, misconfigurations and software vulnerabilities.

### Installation
- Maintain persistence on the compromised system.
- Examples include scheduled tasks, cron jobs, web shells, backdoors and startup scripts.

### Command & Control (C2)
- Establish a communication channel with the compromised machine.
- Common techniques include HTTPS, DNS Tunnelling, Reverse Shells, DGA and Fast Flux.

### Actions on Objectives
- Complete the original goal of the attack.
- Could be data theft, ransomware, financial theft, lateral movement or espionage.

---

## Attack Flow

Reconnaissance
➡️
Weaponisation
➡️
Delivery
➡️
Exploitation
➡️
Installation
➡️
Command & Control (C2)
➡️
Actions on Objectives

---

## Important Terms

- Payload
- Persistence
- Backdoor
- Reverse Shell
- Web Shell
- Data Exfiltration
- Lateral Movement
- LOLBins
- DGA
- Fast Flux
- DNS Tunnelling

---

## Session Takeaways

- Every attack usually follows a sequence instead of happening randomly.
- Reconnaissance provides the foundation for the rest of the attack.
- Installation is all about persistence.
- Command & Control allows attackers to communicate with compromised systems while trying to avoid detection.
- Different attackers have different end goals, but they all eventually reach the "Actions on Objectives" stage.


---

### Room Link  
https://tryhackme.com/room/cyberkillchain
---

## Tags

- Cyber Kill Chain
- Reconnaissance
- Exploitation
- Persistence
- Command & Control
- Data Exfiltration
- Threat Modelling
- TryHackMe