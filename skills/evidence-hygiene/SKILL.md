---
name: evidence-hygiene
description: Capture clean, safe evidence for bug bounty reports — screenshots, video, redacting PII and secrets, and writing proof-of-concepts that prove the bug without harming anyone.
---

# Evidence Hygiene

## Scope rule

Evidence proves the bug — it is never an excuse to collect data. Capture
the minimum that proves the finding, redact everything sensitive, and
never store real users' data.

## Screenshots and video

- Screenshot the moment of proof: the response showing the issue, the
  alert firing, the other test account's data visible.
- Include the URL bar and a timestamp where possible — it anchors the
  evidence to the target.
- Short screen recordings (30–60 seconds) beat long ones: start logged
  out, show the steps, end at the proof.
- Capture evidence the moment you find the bug. Reproducing later for a
  screenshot wastes time and sometimes the bug is already fixed.

## Redaction — always

Before attaching anything, redact:

- Session cookies, auth tokens, API keys, passwords — replace with
  `[REDACTED]`.
- Real users' names, emails, phone numbers, addresses — blur or remove.
  If the proof needs a record, use your own test account's record.
- Internal IPs and hostnames that are not part of the finding.
- Your own credentials too — test account passwords do not belong in a
  report either.

## Proof-of-concepts

- Minimal: the smallest request or payload that triggers the bug.
- Harmless: `alert(1)`, your own test data, a collaborator ping. Never
  a payload that deletes, modifies, or exfiltrates real data.
- Self-contained: a triager should be able to run it with only the
  report and a test account.
- Never include a weaponized exploit, a full data dump, or steps that
  would harm the target if run at scale.

## Storage

- Keep evidence in your own notes folder, not in a public repo.
  This bundle's `.gitignore` excludes `notes/` for that reason.
- Delete evidence containing sensitive data once the report is closed.
- Never post evidence publicly (social media, blog, GitHub) before the
  program authorizes disclosure — and even then, keep the redaction.
