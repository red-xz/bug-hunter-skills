# HackerOne Report Template

Fill in every section. HackerOne uses CVSS-based severity — give an honest
severity and let triage adjust it.

## Title

[Vulnerability class] on [feature/endpoint] — [brief impact]

Example: `Stored XSS in profile bio executes in other users' browsers`

## Severity

[None / Low / Medium / High / Critical — with a one-line reason.
If you computed CVSS, include the vector and score.]

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
