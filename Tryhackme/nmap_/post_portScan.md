# Nmap Post Port Scans

### Platform: TryHackMe

### Learning Path: Jr Penetration Tester (PT1)

### Difficulty: ⭐⭐⭐☆☆

### Date Completed: 10-07-2026

---

## Overview

This room focuses on what comes after discovering open ports. It covers service and version detection, operating system fingerprinting, traceroute, the Nmap Scripting Engine (NSE), and saving scan results. These features help transform a simple port scan into meaningful reconnaissance that guides the next stages of a penetration test.

---

## Key Concepts Learned

### Service & Version Detection

* `-sV` identifies the software and version running on open ports.
* Service versions are valuable because known vulnerabilities are typically associated with specific software versions.
* Version detection requires Nmap to establish a full TCP connection with the service, making it less stealthy than a SYN scan.

### OS Detection

* `-O` performs operating system fingerprinting by analysing TCP/IP stack behaviour, such as TCP sequence numbers, TTL values, and packet responses.
* OS detection is an educated guess rather than a guarantee. Virtual machines, firewalls, cloud environments, and network devices can affect accuracy.

### Traceroute

* `--traceroute` identifies the path (routers/hops) between the scanner and the target.
* It helps understand network topology and estimate the target's network distance.
* Some routers intentionally hide themselves, so missing hops are normal.

### Nmap Scripting Engine (NSE)

* NSE extends Nmap using Lua scripts to automate enumeration and security checks.
* `-sC` runs the default set of safe and commonly useful scripts.
* Individual scripts or script categories can be executed using `--script=<script>` or `--script=<category>`.
* Examples include gathering HTTP titles, checking FTP settings, or testing for known vulnerabilities.

### Saving Scan Results

* Saving scans is important for documentation, reporting, and future analysis.
* Nmap supports Normal, Grepable, and XML output formats.
* `-oA` saves all major output formats in a single command and is the most practical option.

### Aggressive Scan

* `-A` is a shortcut that combines:

  * Service Detection (`-sV`)
  * OS Detection (`-O`)
  * Default NSE Scripts (`-sC`)
  * Traceroute (`--traceroute`)
* It does **not** scan every port or run every NSE script.

---

## Important Commands

```bash
# Service Version Detection
sudo nmap -sV TARGET

# Faster Version Detection
sudo nmap -sV --version-light TARGET

# Most Thorough Version Detection
sudo nmap -sV --version-all TARGET

# Operating System Detection
sudo nmap -O TARGET

# Traceroute
sudo nmap --traceroute TARGET

# Default NSE Scripts
sudo nmap -sC TARGET

# Run a Specific NSE Script
sudo nmap --script=http-title TARGET

# Run Vulnerability Scripts
sudo nmap --script "vuln" TARGET

# Run All Scripts Matching a Pattern
sudo nmap --script "http*" TARGET

# Aggressive Enumeration
sudo nmap -A TARGET

# Save Output (Recommended)
sudo nmap -oA scan_results TARGET
```

---

## Important Tools

* **Nmap** – Performs service detection, OS fingerprinting, traceroute, scripting, and output generation.
* **Nmap Scripting Engine (NSE)** – Extends Nmap using Lua scripts for enumeration, information gathering, and vulnerability checks.

---

## Important Ports

This room focuses on **post-port scan enumeration**, so there are **no new ports** to remember.

---

## Key Takeaways

* Open ports are only the starting point; identifying the service and its version is what enables vulnerability research.
* `-sV` requires a full TCP connection because Nmap must communicate with the service.
* OS fingerprinting relies on TCP/IP behaviour and should always be treated as an informed estimate.
* NSE significantly expands Nmap's capabilities and should be used selectively based on discovered services.
* `-A` is a convenient shortcut but should not replace targeted enumeration during professional engagements.
* Always save scan results using `-oA` to simplify reporting and future analysis.
* Using Specified ports for specific service checks is more often preffered than full scans as its fast and reliable.

---

## Pentester Mindset

Discovering open ports answers **where** to investigate. Post-port scanning answers **what is actually running there**. Focus on collecting enough information to identify software versions, operating systems, and potential attack paths rather than relying on assumptions based solely on port numbers. Use NSE scripts deliberately to gather relevant information instead of running every available script.

---

## Final Thoughts

This room completes the Nmap series by moving beyond simple port discovery into service enumeration and documentation. Understanding what is running on a target is often far more valuable than simply knowing which ports are open, making these techniques an essential part of any penetration tester's reconnaissance workflow.
