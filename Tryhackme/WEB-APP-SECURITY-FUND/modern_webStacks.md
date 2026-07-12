# Modern Web Stacks

### Platform: TryHackMe

### Learning Path: Jr Penetration Tester (PT1)

### Difficulty: ⭐⭐⭐⭐☆

### Date Completed: 12-07-2026

---

## Overview

This room focuses on **web stack fingerprinting**. Before attempting exploitation, a penetration tester should first identify the technology running behind a web application using passive information such as HTTP headers, cookies, error pages, HTML source, and URL patterns.

Once the framework and version are identified, the next step is to research known vulnerabilities (CVEs) and perform targeted testing instead of blindly running exploits.


The room demonstrates this process using four common web stacks:

- MERN (Express)
- Next.js
- Django
- LAMP (Apache)

### A web stack is simply the technologies used to build a website

---

## Pentesting Workflow

```text
Fingerprint Technology
        ↓
Identify Version
        ↓
Research CVEs
        ↓
Understand the Root Cause
        ↓
Test Manually
        ↓
Exploit (If Vulnerable)
```

This workflow is far more effective than randomly testing payloads.
### The goal is:

Figure out what technology you're attacking.

---

# MERN (Express)
MongoDB
Express.js
React
Node.js

Express is the backend framework that processes requests.

## Fingerprinting

Look for:

- `X-Powered-By: Express`
- `connect.sid` cookie
- `Cannot GET /nonexistent` error page

These strongly indicate an Express backend.

### Example

```http
X-Powered-By: Express
Set-Cookie: connect.sid=...
```

or

```
Cannot GET /nonexistent
```

---

## What To Check Next

After identifying Express, enumerate:

- REST APIs
- JSON endpoints
- Authentication
- Session handling
- User profile/update endpoints
- Input validation

### Most Express applications expose APIs that become the main attack surface.

---

## Vulnerability Demonstrated

**Prototype Pollution**

A vulnerable merge function accepted every JSON key.

Example payload:

```json
{
 "__proto__":{
   "isAdmin":true
 }
}
```

Instead of modifying one user, it modified JavaScript's shared `Object.prototype`, causing every object to inherit:

```
isAdmin = true
```

The application trusted:

```javascript
currentUser.isAdmin
```

which resulted in an authentication bypass.

---

## Pentester Mindset

Don't memorize Prototype Pollution.

Remember:

```
Express
      ↓
Find APIs
      ↓
Check how JSON is processed
      ↓
Research Express/Node.js CVEs
```

---

# Next.js 
Next.js is built on top of React and runs on Node.js & Many modern websites use it.

## Fingerprinting

Look for:

- `X-Powered-By: Next.js`
- `window.__next_f`
- `/_next/static/`
- `x-nextjs-cache`
- `x-nextjs-prerender`

These indicate a production Next.js application.

---

## What To Check Next

Focus on:

- Authentication
- Middleware
- API Routes
- React Server Components

---

## Vulnerability Demonstrated

**CVE-2025-29927**

Authentication was implemented inside middleware.

Normally:

```
Request
    ↓
Middleware
    ↓
Authentication
    ↓
Dashboard
```

Next.js trusted an internal header:

```
x-middleware-subrequest
```

An attacker could manually send this header, causing middleware to be skipped completely.

Result:

```
Authentication Bypass
```

---

## Pentester Mindset

Don't remember one header.

Remember:

```
Next.js
      ↓
Research Middleware
      ↓
Authentication
      ↓
Known Next.js CVEs
```

---

# Django 
Python's most popular web framework.

## Fingerprinting

Look for:

Headers

```
Server: WSGIServer
```

Cookie

```
csrftoken
```

Hidden Form Field

```
csrfmiddlewaretoken
```

Security Headers

```
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: same-origin
```

These together strongly indicate Django.

---

## What To Check Next

Look for:

- Forms
- URL parameters
- Search functionality
- Sorting parameters
- Database queries

---

## Vulnerability Demonstrated

**CVE-2021-35042**

The application accepted:

```
?order=
```

and directly inserted it into an SQL query.

This allowed SQL Injection.

The room used:

```
updatexml()
```

which intentionally generated a database error.

Because Django was running with:

```
DEBUG = True
```

the error revealed sensitive database information such as:

- MySQL Version
- Database Name

---

## Pentester Mindset

Don't memorize `updatexml()`.

Remember:

```
Django
      ↓
Find user-controlled parameters
      ↓
Research Django CVEs
      ↓
Look for SQL Injection
```

---

# LAMP (Apache) - 
One of the oldest and most common web stacks.

## Components

- Linux
- Apache
- MySQL
- PHP

---

## Fingerprinting

Look for:

```
Server: Apache
```

or

```
Server: Apache/2.4.49
```

Also check:

```
/cgi-bin/
```

Response:

```
403 Forbidden
```

means CGI exists but directory listing is disabled.

---

## What To Check Next

Research:

- Apache Version
- PHP
- CGI
- Apache Modules
- Known Apache CVEs

---

## Vulnerability Demonstrated

**CVE-2021-41773**

Apache 2.4.49 incorrectly handled encoded directory traversal.

Instead of:

```
../
```

the attacker used:

```
.%2e/
```

Apache decoded it later, allowing directory traversal.

When `mod_cgi` was enabled:

```
Traversal
      ↓
Reach /bin/sh
      ↓
Execute Commands
      ↓
Remote Code Execution
```

---

## Why `--path-as-is`

Normally, curl cleans the URL before sending it.

Using:

```bash
--path-as-is
```

forces curl to send the payload exactly as written so Apache receives the encoded traversal.

---

# Nikto

Nikto automates initial fingerprinting.

It quickly identifies:

- Frameworks
- Server versions
- Missing security headers
- Common misconfigurations

Examples:

Express

```
X-Powered-By: Express
```

Next.js

```
X-Powered-By: Next.js
```

Django

```
WSGIServer
```

Apache

```
Apache/2.4.49
```

Nikto is useful for reconnaissance, but manual verification is still required.

---

# Tools Used

- curl
- Nikto
- Browser Developer Tools
- Nmap
- SearchSploit
- CVE Databases

---

# CVEs Covered

| Stack | CVE | Impact |
|--------|------|--------|
| Express | CVE-2020-8203 | Prototype Pollution → Authentication Bypass |
| Next.js | CVE-2025-29927 | Middleware Authentication Bypass |
| Django | CVE-2021-35042 | SQL Injection |
| Apache | CVE-2021-41773 | Path Traversal → Remote Code Execution |

---

# Key Takeaways

- Every web application leaks information through headers, cookies, HTML source, URLs, and error pages.
- Fingerprinting the framework should always come before exploitation.
- Identifying the exact version helps map directly to known CVEs.
- Manual fingerprinting is often faster and more accurate than relying only on automated scanners.
- After identifying a framework, research **common attack surfaces** and **known vulnerabilities** instead of trying random payloads.
- Nikto is useful for quickly identifying technologies and server information but should always be combined with manual testing.

---

## Pentester Mindset ⭐

Don't think:

```
I found Express.
I know one exploit.
```

Think:

```
Identify Framework ( manual & automated )
        ↓
Identify Version
        ↓
Research CVEs
        ↓
Understand Common Attack Surface 
        ↓
Test Manually
        ↓
Exploit if Vulnerable
```

 As a penetration tester, the focus should always be on understanding why a vulnerability exists before reaching for an exploit. Every signal you read, every header you inspect, and every version you confirm brings you closer to a targeted, evidence-driven attack chain rather than a noisy scanner run.

The CVEs shown in this room are examples. The real lesson is learning a repeatable workflow that can be applied to **any** modern web framework.