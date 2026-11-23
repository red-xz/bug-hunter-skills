---
name: recon
description: Passive reconnaissance inside program scope — subdomain enumeration, technology fingerprinting, and endpoint discovery. Use at the start of a hunt on a new target.
---

# Recon

## Scope rule

Recon only against assets listed in the program's scope. Passive techniques
first (public data, DNS, certificates); active probing only where the
program allows it, at a polite rate, and never against out-of-scope assets.
Never touch real user data, never DoS, never pivot beyond scope.

## Subdomain enumeration

- Start passive: certificate transparency logs (crt.sh), DNS history,
  search engines, and public subdomain datasets. Passive sources leave no
  traffic on the target.
- Then active (if allowed): DNS brute-forcing with a small wordlist
  (subfinder, amass, gobuster dns). Keep the rate low.
- Resolve everything you find and keep only live hosts. Note which live
  hosts are in scope — a subdomain not listed in scope is out of scope
  until the program says otherwise.
- Check for forgotten or staging subdomains (dev, staging, test, old):
  they often run older code with weaker controls. Only test them if they
  are in scope.

## Technology fingerprinting

- Identify the stack: web server, framework, CMS, JS libraries, CDN, WAF.
  Tools: Wappalyzer, WhatWeb, or manual header / HTML inspection.
- Note version numbers where visible — old versions of a framework or
  library often map to known CVEs. Verify a version is actually vulnerable
  before reporting; version alone is rarely a valid finding.
- Note the auth mechanism (session cookie, JWT, OAuth), the API style
  (REST, GraphQL), and any third-party integrations (payment, upload, SSO).

## Endpoint discovery

- Read robots.txt and sitemap.xml — they list paths the owner cares about.
- Read the site's JavaScript files: API endpoints, hidden routes, and
  sometimes keys or internal hostnames leak there. (A leaked key is a
  finding only if it is valid and gives real access — test carefully and
  never use it beyond confirming validity.)
- Check for API documentation (Swagger/OpenAPI, GraphQL introspection if
  enabled) — in-scope only.
- Directory discovery (ffuf, dirsearch, gobuster) with a small wordlist at
  a polite rate, only where the program allows active scanning. Watch for
  admin panels, backups (.bak, .old, .zip), and config files.

## Output

End recon with a short target sheet: in-scope hosts, tech stack, the
interesting endpoints, and the features worth mapping first (login,
signup, password reset, profile, upload, search, payment). Hand it to the
mapping phase of `bb-methodology`.
