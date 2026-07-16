
# WEB SERVER ATTACK 1
### Platform: TryHackMe

### Learning Path: Jr Penetration Tester

### Difficulty: ⭐⭐⭐⭐☆

### Date Completed: 15-07-2026



## STEP 0 — ALWAYS DO THIS FIRST (any server, any port)

```bash
curl -sI http://IP:PORT
```
**Look for:** `Server:` header, `X-Powered-By:` header
**If headers are hidden/blank**, request a fake page to check the error page instead:
```bash
curl -s http://IP:PORT/nonexistent-xyz
curl -sI http://IP:PORT/nonexistent-xyz | grep -i server

curl -sS -D - -o /dev/null -L https://ip.lab/ | grep -i '^server:'
```

| What you see | What it means |
|---|---|
| `Server: Apache/2.4.x (Ubuntu)` | Apache |
| `Server: nginx/1.24.0` | Nginx |
| `Server: SimpleHTTP/0.6 Python/3.x` | Python HTTP server |
| No `Server` header, but `X-Powered-By: Express` | Node.js Express |
| 404 page mentions Apache in body | Apache (even if header hidden) |
| 404 page shows nginx version in footer | Nginx (even if header hidden) |
| 404 page is plain text | Python |

---

## 🐍 PYTHON HTTP SERVER (commonly port 8000)

**Mental model:** No security features exist at all. It serves EVERY file in the folder, no exceptions, including hidden dotfiles.

```bash
# 1. Check for directory listing
curl -s http://IP:8000/

# 2. Try common secret files directly (even without seeing them listed)
curl -s http://IP:8000/.env

# 3. If you see any .zip/.tar.gz — download and open it
curl -s http://IP:8000/backup.zip -o backup.zip
unzip backup.zip -d backup-contents/
```

**Traps to remember:**
- If `index.html` exists → directory listing is hidden, but **other files are still fully accessible** if you know/guess the name. "Not listed" ≠ "not accessible."
- No `-o` needed for text files (prints to screen). **Must use `-o filename` for binaries/archives** (zip, tar.gz) — they're not readable as raw text.

---

## 🪶 APACHE2 (commonly port 80)

**Mental model:** Most common server → check version, then run through 4 fixed checks in order.

```bash
# 1. Version
curl -SI http://IP:80 | grep -i server

# 2. Directory listing (needs: Options +Indexes AND no index.html)
curl -s http://IP:80/files/

# 3. Status page (should be localhost-only, sometimes globally exposed)
curl -s http://IP:80/server-status

# 4. Hidden/unlinked files via brute force
gobuster dir -u http://IP:80 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -x bak,txt,html -t 20
```

**Traps to remember:**
- `ServerTokens OS` (Ubuntu default) shows BOTH version number AND OS name together.
- `/server-status` default = `Require local` (localhost only). Gets silently broken by `Require all granted` anywhere in vhost config — **check it even if the server "looks" default.**
- Gobuster speed tip: run WITHOUT `-x` first to find real directories, THEN do a targeted `-x bak` pass — saves time.
- `.bak` files → often contain creds/configs. `.htpasswd` → crackable password hash + confirms Basic Auth is used somewhere.

---

## 🟢 NODE.JS EXPRESS (commonly port 3000)

**Mental model:** This one is different — it's not files in a folder, it's actual running code. Each request triggers a function. Follow this literal chain — each output tells you the next command.

```bash
# 1. Confirm framework
curl -sI http://IP:3000
# → look for: X-Powered-By: Express

# 2. Check root path for version info
curl -s http://IP:3000
# → look for: {"version":"x.x.x"} — just note it

# 3. Try to trigger an error (guess a likely API path)
curl -s http://IP:3000/api/users
# → look for: "stack": "..." — SAVE this if found

# 4. Look for a routes list (this is your map for everything after)
curl -s http://IP:3000/api/routes
# → returns a JSON list of every path the app has

# 5. From that list — if a debug/env path showed up:
curl -s http://IP:3000/api/debug/env
# → look for: DB_PASSWORD, SECRET_KEY, NODE_ENV=development

# 6. From that list — if a static path showed up:
curl -s http://IP:3000/static/config.js
# → look for: internal URLs, DEBUG=true, hardcoded values
```

**Core concepts (if the "why" still feels fuzzy):**
- A **route** = a coded rule: "when someone visits URL X, run function Y." `/api/routes` is just one extra route a dev left in that happens to print out the list of all other routes.
- **Static files** = plain files handed out with no logic (like a filing cabinet) — CSS, JS, config files meant for the browser.

**Traps to remember:**
- `NODE_ENV=production` does NOT guarantee safety — a custom error handler written by the dev can leak stack traces regardless of this setting. Always still try step 3.
- `express.static()` **auto-404s on dotfiles** (`.env`, etc.) by default — OPPOSITE of Python's server. A 404 on a dotfile here does **NOT** prove the file doesn't exist — it just means that access path is blocked.
- Correct step order and WHY: Headers (confirm) → Errors (poke) → Routes (get the map) → Env vars (follow map) → Static files (follow map). Steps 4-5-6 depend on step 4's output, which is why routes comes before env/static.

---

## 🌀 NGINX (commonly port 8080, since Apache often owns port 80)

**Mental model:** Same investigative pattern as Apache, different config vocabulary. Nginx is often a reverse proxy / traffic handler in real deployments.

```bash
# 1. Version
curl -sI http://IP:8080 | grep -i server

# 2. Verify version isn't partially hidden (check 404 footer)
curl -s http://IP:8080/nonexistent-path

# 3. Directory listing (needs: autoindex on; in config — OFF by default, unlike Apache)
curl -s http://IP:8080/files/

# 4. Status endpoint (stub_status module)
curl -s http://IP:8080/nginx_status
```

**Traps to remember:**
- Apache uses `ServerTokens` → Nginx uses `server_tokens` (same idea, different name). Controls BOTH the header AND the error page version at once.
- Nginx directory listing is **OFF by default** (opposite mental note from Apache) — only shows if a dev explicitly added `autoindex on;`.
- `nginx_status` output — the 3 unlabeled numbers on one line = **accepted, handled, total requests** (in that order, since server start). Last line = current connections by state (Reading/Writing/Waiting).
- Secure config: `allow 127.0.0.1; deny all;` → Misconfigured: `allow all;`

---

## 🛡️ SECURITY HEADERS AUDIT (run against ALL 4 servers)

```bash
for port in 80 8000 3000 8080; do
  echo "=== Port $port ==="
  curl -sI http://IP:$port/ | grep -iE "x-frame-options|x-content-type|content-security-policy|strict-transport|referrer-policy" || echo "(no security headers found)"
done
```

| Header | Protects Against |
|---|---|
| `X-Frame-Options` | Clickjacking |
| `X-Content-Type-Options` | MIME sniffing |
| `Content-Security-Policy` | Restricts script/style sources |
| `Referrer-Policy` | Controls leaked URL info on navigation |
| `Strict-Transport-Security` | Forces HTTPS (only relevant on HTTPS servers — expected missing on plain HTTP) |

**Trap:** `X-Frame-Options` is technically superseded by `CSP: frame-ancestors`. A hardened site may skip X-Frame-Options intentionally if CSP covers it — check for both before flagging as "missing."

---

## 🤖 NIKTO (automated scan)

```bash
nikto -h http://IP:80 -nointeractive
```
- `-nointeractive` = skip prompts, run straight through
- Findings start with `+`
- **Loud and easily detected** — for authorized testing only, not stealth

**Faster scan (restrict checks):**
```bash
nikto -h TARGET -Tuning 123
```
Tuning digits are **glued together** (like a PIN), not comma-separated. `123` = run check-category 1, 2, AND 3 — not "check #123."

---

## 🔁 CROSS-SERVER PATTERN TABLE (for quick recall)

| Misconfig | Apache | Python | Node.js | Nginx |
|---|---|---|---|---|
| Version disclosure | Yes | Yes | Partial (via X-Powered-By only) | Yes |
| Directory listing | `/files/` | Root path | N/A (no listing feature exists) | `/files/` |
| Status/debug endpoint | `/server-status` | N/A | `/api/debug/env`, `/api/routes` | `/nginx_status` |
| Sensitive files | `.bak`, notes | `.env`, `.zip` | `config.js` | `.txt`, `.tar.gz` |
| Missing security headers | Yes | Yes | Yes | Yes |

---


# PT1 Web Server Recon — Master Cheat Sheet

**Golden rule for the whole room:** Default configs are convenient, not secure. Nobody breaks these servers — they just never get hardened. Our job is to find what was left open, not to exploit anything.

---


## 🎯 ONE-LINE MEMORY HOOKS

- **Python:** "Serves everything, no exceptions — even the hidden stuff."
- **Apache:** "Version → Listing → Status page → Gobuster. In that order, every time."
- **Node.js:** "Confirm it → Poke it → Get the map → Follow the map."
- **Nginx:** "Apache's twin — same checks, different directive names, listing OFF by default."
- **All 4:** "No security headers by default — always run the curl+grep loop."