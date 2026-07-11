# Walking An Application

### Platform: TryHackMe

### Learning Path: Jr Penetration Tester

### Difficulty: ⭐☆☆☆☆

### Date Completed: 11-07-2026

## Overview

This room introduced how to manually inspect a web application using only a web browser and its built-in Developer Tools. Instead of using automated scanners, the goal was to understand how a website works, find hidden information, and learn where security issues can exist.

## What I Learned

* Explored a website and identified its pages and features before testing.Enumeration comes before exploitation.
* Used **View Page Source** to find comments, hidden links, and framework information.
* Used the **Inspector** to view and edit HTML and CSS to reveal hidden content.
* Used the **Debugger** to understand JavaScript and pause it with breakpoints.
* Used the **Network** tab to view requests, responses, and AJAX traffic.
* Used the **Storage** tab to check Local Storage, Session Storage, and Cookies.
* Learned why cookie security flags like **HttpOnly**, **Secure**, and **SameSite** are important.

## Key Takeaways

* Always explore the website before testing it.
* Page Source may contain hidden information.
* Inspector can help reveal hidden content on a page.
* Debugger helps understand what JavaScript is doing.
* Network tab shows everything the browser sends and receives.
* Cookies are important for authentication and session management.
* Manual testing can find things that automated tools may miss.

## Developer Tools Covered

* View Page Source
* Inspector
* Debugger
* Network
* Storage
  * Local Storage
  * Session Storage
  * Cookies
  * Cache Storage

---

# A Simple Pentester Workflow

### Imagine you're testing a new website:

- 🌐 Browse every page (enumeration).
- 📄 Check View Source for comments and hidden links.
- 🔍 Use Inspector to look for hidden content.
- 🌐 Open Network and click around to watch requests.
- 🍪 Check Cookies in Storage and inspect security flags 
- 📜 If needed, inspect the JavaScript in Debugger.

### Room Link

https://tryhackme.com/room/walkinganapp

---

## Tags

`Walking An Application` `Developer Tools` `Page Source` `Inspector` `Debugger` `Network` `Cookies` `Web Application` `Manual Testing` `TryHackMe`