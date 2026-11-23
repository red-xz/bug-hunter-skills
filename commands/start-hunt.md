# /start-hunt — Begin a hunt on a new program

Use when starting work on a bug bounty program you have not hunted before.
Do not skip the scope steps — they are the difference between a bounty and a ban.

## 1. Scope check (do first, every time)

- [ ] Open the program page and read the full scope document.
- [ ] List in-scope domains / apps / assets. Write them down.
- [ ] List out-of-scope assets and forbidden test types (commonly: DoS,
      spam, social engineering, testing on other users' accounts, automated
      scanners at high rate).
- [ ] Check the reward table and the vulnerability rating taxonomy the
      program uses (Bugcrowd VRT / HackerOne severity).
- [ ] Confirm: the target I am about to touch is in scope, and my planned
      test types are allowed.

## 2. Recon (skill: `recon`)

- [ ] Enumerate subdomains of in-scope domains (passive sources first).
- [ ] Fingerprint the tech stack (framework, server, JS libraries).
- [ ] Collect endpoints: sitemap, robots.txt, JS files, API docs, error pages.
- [ ] Note login / signup / password-reset / profile / upload / search /
      payment flows — these are the high-value surfaces.

## 3. Map the app

- [ ] Create a test account (your own — never another user's).
- [ ] Click through every feature while proxying traffic (Burp Community /
      ZAP). Save the site map.
- [ ] Mark every place user input reaches the server: forms, URL params,
      headers, cookies, file uploads, API bodies.
- [ ] Mark every place one user's data is shown to another user
      (IDOR hunting ground).

## 4. Pick ONE bug class for this pass

Beginners: start with `hunt-idor` (broken access control). One class per
pass keeps testing focused and findings clean. Load that skill and follow it.

## 5. Test, note, validate

- [ ] For each candidate finding: record URL, request, response, and a
      screenshot immediately (see `evidence-hygiene`).
- [ ] Reproduce it from a clean state before moving on.
- [ ] When you have a candidate, switch to `/validate-finding`.
