---
name: report-writing
description: Write bug bounty reports that triage accepts — Bugcrowd and HackerOne structure, clear reproduction steps, honest impact, and severity guidance. Use after a finding is validated.
---

# Report Writing

## Scope rule

Reports contain only what was proven within scope. Never include real
users' data, credentials, or session tokens. Redact everything sensitive
(see `evidence-hygiene`).

## Structure (both platforms)

1. **Title:** vulnerability + feature + endpoint.
   `IDOR on /api/orders/{id} exposes other users' order details`
2. **Summary:** 2–3 sentences — what, where, impact. No preamble.
3. **Steps to reproduce:** numbered, exact, from a logged-out state where
   relevant. A triager with no context must be able to follow them.
4. **Proof of concept:** the minimal request or payload that triggers it.
5. **Impact:** what an attacker gains, in plain business terms.
6. **Evidence:** screenshots / video attached at the step they prove.
7. **Suggested fix:** one or two lines, the standard remediation.
8. **Notes:** test accounts used, scope respected, no real user data touched.

Use the matching template: `templates/bugcrowd-report.md` or
`templates/hackerone-report.md`.

## Platform notes

- **Bugcrowd:** state the VRT (Vulnerability Rating Taxonomy) category
  that matches the finding. Managed triage gives clearer feedback —
  read it and apply it.
- **HackerOne:** give an honest severity (CVSS if you can compute it).
  Check Hacktivity for duplicates before submitting.

## Writing rules

- One finding per report. Never bundle two bugs into one submission.
- Write for a tired triager: short sentences, exact steps, no filler.
- State impact factually — never inflate ("could lead to full takeover"
  only if you can show the chain).
- Attach evidence inline at the relevant step, not dumped at the end.
- Never threaten, never demand a payout, never disclose publicly before
  the program allows it.
- If triage asks for more information, answer within a day with exactly
  what they asked for.

## Common reasons reports get rejected

- Steps that cannot be reproduced (missing a login step, wrong parameter).
- No demonstrated impact ("I found an error message").
- Duplicate of an already-reported issue.
- Out of scope or on the program's exclusion list.
- Self-only impact presented as a user-facing bug.

## After submitting

Log it: date, program, bug class, URL, status. Do not re-test the same
finding repeatedly while waiting. A "not applicable" verdict is feedback —
read the reason, find the gap, and carry it into the next hunt.
