---
name: triage-validation
description: Validate a candidate finding before reporting — reproduce it, confirm real impact, check for duplicates and exclusions, and set an honest severity. Use after any hunt skill produces a candidate.
---

# Triage & Validation

## Scope rule

Validation never expands scope. If confirming a finding would require
touching an out-of-scope asset or a real user's data, do not do it —
report what you can prove within scope, and say so plainly.

## 1. Reproduce

- Reproduce from a clean state (fresh login / fresh session) at least
  twice with identical steps.
- Write the steps as you go — they become the report's reproduction
  section.
- Rule out self-inflicted causes: your own test data, a stale session,
  a proxy artifact, a misread response.

## 2. Confirm real impact

Answer in one sentence: what can an attacker actually do with this?
- Who is affected — only your test account, or other real users?
- What is gained — data, access, money, account control, or nothing?
- Self-only impact is usually Informational or N/A. A finding needs a
  plausible victim to be worth a report.

## 3. Check duplicates and exclusions

- Search the program's disclosed reports (HackerOne Hacktivity, Bugcrowd
  disclosures) for the same bug on the same endpoint.
- Read the program's out-of-scope / known-issues list. Common exclusions:
  missing security headers, verbose errors without data, self-XSS,
  clickjacking without impact, missing rate limiting alone, version
  disclosure, logout CSRF.
- If your finding matches an exclusion, do not submit it — log it and
  move on.

## 4. Set an honest severity

- Use the program's taxonomy (Bugcrowd VRT or HackerOne CVSS guidance),
  not a guess.
- Severity follows impact, not cleverness. A fancy bypass with no victim
  impact is Low; a boring IDOR exposing private data is High.
- When unsure between two levels, pick the lower one and let triage
  raise it. Overstating severity damages credibility.

## 5. Evidence check

- Screenshots or a short video showing the issue, attached to the step
  they prove (see `evidence-hygiene`).
- The exact request/response, redacted.
- A minimal PoC — the smallest steps that trigger the bug.

## 6. Decide

- Passes all checks → `report-writing` + `/write-report`.
- Impact unclear → one more controlled variation, then re-run this
  checklist. Never submit "maybe" findings.
- Fails scope, impact, or exclusion checks → log it in your notes and
  move on. Not every anomaly is a bug, and not every bug is a bounty.
