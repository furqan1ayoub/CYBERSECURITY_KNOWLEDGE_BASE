# Content Discovery

### Platform: TryHackMe

### Learning Path: Jr Penetration Tester (PT1)

### Difficulty: ⭐⭐☆☆☆

### Date Completed: 11-07-2026

---

## Overview

This room focuses on **Content Discovery**, one of the most important phases of web application reconnaissance. The goal is to discover hidden pages, files, directories, subdomains, and virtual hosts that are not directly linked on the website. It covers manual techniques, OSINT, and automated enumeration using Gobuster.

---

## Key Concepts Learned

### Manual Content Discovery

* Check `robots.txt` for hidden directories and pages that search engines should avoid.
* Check `sitemap.xml` to discover indexed pages and hidden endpoints.
* Inspect **HTTP headers** to identify the web server, technologies, and custom headers.
* Identify the **framework stack** to learn default admin paths and search for known vulnerabilities.

### OSINT (Open-Source Intelligence)

* Use **Google Dorking** to find indexed files, login pages, and sensitive documents.
* Use **Wappalyzer** to identify technologies, frameworks, CMSs, and web servers.
* Use the **Wayback Machine** to discover old or removed pages.
* Search **GitHub** repositories and commit history for leaked credentials or configuration files.
* Check **Amazon S3 Buckets** for publicly accessible files and backups.

### Automated Content Discovery

* **Gobuster** automates the discovery of hidden content using wordlists.
* `dir` mode discovers directories and files.
* `dns` mode discovers subdomains.
* `vhost` mode discovers virtual hosts hosted on the same web server.

### Subdomains vs Virtual Hosts

* **Subdomains** are resolved through DNS (e.g., `blog.example.com`).
* **Virtual Hosts** are resolved by the web server using the HTTP `Host` header and may not exist in DNS.

---

## Important Commands

```bash
# Directory Enumeration
gobuster dir -u http://TARGET -w WORDLIST

# Directory Enumeration with Extensions
gobuster dir -u http://TARGET -w WORDLIST -x php,txt,js

# Follow Redirects
gobuster dir -u http://TARGET -w WORDLIST -r

# DNS Subdomain Enumeration
gobuster dns -d example.com -w WORDLIST

# Show IP Addresses
gobuster dns -d example.com -w WORDLIST -i

# Virtual Host Enumeration
gobuster vhost -u http://TARGET --domain example.com -w WORDLIST --append-domain

# View HTTP Headers
curl -v http://TARGET
```

---

## Important Tools

* **Gobuster** – Directory, file, subdomain, and virtual host enumeration.
* **curl** – View HTTP response headers.
* **Google Dorking** – Discover indexed content.
* **Wappalyzer** – Identify website technologies.
* **Wayback Machine** – View archived versions of websites.
* **GitHub** – Search for leaked source code and secrets.
* **Amazon S3** – Check for exposed cloud storage buckets.

---

## Important Files

* `robots.txt`
* `sitemap.xml`

---

## Key Takeaways

* Content discovery expands the attack surface before exploitation.
* Always combine **Manual**, **OSINT**, and **Automated** enumeration.
* Hidden directories, admin panels, backup files, and old pages often lead to valuable findings.
* HTTP headers and framework information help identify the technologies behind a website.
* Gobuster is one of the primary tools used for web content enumeration.
* Understanding the difference between **Subdomains** and **Virtual Hosts** is important during web reconnaissance.

---

## Pentester Mindset

Never rely only on what the homepage shows you. Many valuable findings exist in locations that are not linked anywhere on the website. Before attempting exploitation, spend time discovering hidden directories, files, subdomains, virtual hosts, and publicly available information. Better enumeration leads to better penetration testing.

---

## Final Thoughts

This room builds a strong foundation for web application enumeration. Learning how to combine manual inspection, OSINT, and Gobuster makes it much easier to map a target application and identify potential attack paths before moving into vulnerability testing and exploitation.