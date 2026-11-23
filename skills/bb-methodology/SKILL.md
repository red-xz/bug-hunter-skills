---
name: bb-methodology
description: The 5-phase bug bounty hunt workflow — recon, mapping, testing, validation, reporting — plus the scope and authorization hard rules. Load this first when starting any hunt or when unsure what to do next.
---

# Bug Bounty Methodology

## Scope and authorization — hard rules (non-negotiable)

- Only test targets inside a published program's scope with explicit
  authorization. A vulnerability disclosure policy is NOT authorization.
- Never test random sites, and never test the hunter's own apps outside a
  bounty program without a written go-ahead for that specific test.
- Never touch real user data. Never exfiltrate. Never run denial-of-service
  tests. Never pivot beyond scope.
- If a finding gives access to real data: STOP, report, do not explore.
- Practice only on: Juice Shop (local), PortSwigger labs, TryHackMe, HTB,
  or in-scope bounty targets.

## The 5-phase workflow

### Phase 1 — Recon

Understand the target before touching it. Passive first: subdomains, tech
stack, endpoints, public information. See `recon`.

### Phase 2 — Mapping

Walk the app as a normal user with a proxy running (Burp Community / ZAP).
Record every feature, every form, every API call. Mark where user input
reaches the server and where one user's data is shown to another user.
A good map is what separates a hunt from random clicking.

### Phase 3 — Testing

One bug class per pass. Load the matching `hunt-*` skill and follow it.
Beginners start with `hunt-idor` — the highest hit rate per hour of effort.
Record every candidate immediately (URL, request, response, screenshot).

### Phase 4 — Validation

Every candidate goes through `triage-validation` before it is reported:
reproduce twice from a clean state, confirm real impact, check for
duplicates and program exclusions, confirm the scope boundary was respected.

### Phase 5 — Reporting

One finding per report. Follow `report-writing` and the platform template
(`templates/bugcrowd-report.md` or `templates/hackerone-report.md`).
Clean reproduction steps + real impact + redacted evidence.

## Working rules

- One program at a time. Depth beats breadth — a hunter who knows one app
  deeply finds more than a hunter who skimmed ten.
- Log every report: date, program, bug class, URL, status, payout.
- A rejected report is feedback. Read the triage comment, find the gap
  (usually impact or reproducibility), and apply it next time.
- Stop conditions: if you are unsure whether something is in scope, it is
  out of scope until the program confirms otherwise. Ask in the program's
  channel rather than guessing.
