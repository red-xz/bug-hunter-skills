# Bugcrowd Report Template

Fill in every section. Bugcrowd triage works from the Vulnerability Rating
Taxonomy (VRT) — state the VRT category that matches your finding.

## Title

[Vulnerability class] on [feature/endpoint] — [brief impact]

Example: `IDOR on /api/orders/{id} exposes other users' order details`

## VRT Category

[e.g. Broken Access Control > Insecure Direct Object Reference (IDOR)]

## URL / Asset

[Full URL of the affected endpoint, and the asset as listed in scope]

## Summary

[2–3 sentences: what the bug is, where it is, and what an attacker gains.]

## Steps to Reproduce

1. [Start from a logged-out state if relevant. Be exact.]
2. [Each step is one action: click, request, or input value.]
3. [Include the exact request — method, URL, parameters, body.]
4. [State the observed result after the final step.]

## Proof of Concept

[Minimal PoC: the smallest request / payload that triggers the issue.
Redact session tokens, passwords, and PII.]

## Impact

[What an attacker can do, in plain business terms. Who is affected?
What data or action is at stake? Keep it factual — do not inflate.]

## Evidence

- [Screenshot 1 — what it shows]
- [Screenshot 2 / video — what it shows]

## Suggested Fix

[One or two lines: the standard remediation for this vulnerability class.]

## Notes

[Anything triage should know: test accounts used, scope boundary respected,
no real user data accessed.]
