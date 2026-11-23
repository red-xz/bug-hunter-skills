# /validate-finding — Confirm a candidate before reporting

Use when you think you have found something. A finding is only reportable
after it passes every check below (skill: `triage-validation`).

## 1. Reproduce

- [ ] Reproduce the issue from a clean state (fresh login / fresh session)
      at least twice, with the same steps.
- [ ] Write the steps down as you go — these become the report's
      reproduction section.
- [ ] Confirm the behavior is not caused by your own test data or a
      misread response.

## 2. Confirm real impact

- [ ] What can an attacker actually do with this? State it in one sentence.
- [ ] Could it affect another real user, or only your own test account?
      (Self-only impact is usually informational or N/A.)
- [ ] Is the impact in the program's reward table? Check the VRT / severity
      guidance before submitting.

## 3. Check for duplicates and known exclusions

- [ ] Search the program's disclosed reports (HackerOne Hacktivity /
      Bugcrowd disclosures) for the same bug on the same endpoint.
- [ ] Check the program's "known issues" / out-of-scope list — many common
      findings (missing headers, verbose errors without data, self-XSS) are
      explicitly excluded.

## 4. Check the scope boundary

- [ ] Did any step touch an out-of-scope asset or a real user's data?
      If yes: stop, do not include that data, and note the boundary in the
      report. Never go back to "get more proof" outside scope.

## 5. Evidence pack

- [ ] Screenshots or a short screen recording showing the issue
      (see `evidence-hygiene`).
- [ ] The exact request/response (redact session tokens, passwords, PII).
- [ ] A minimal proof-of-concept — the smallest steps that show the bug.

## 6. Decide

- Passes all checks → `/write-report`.
- Impact unclear → test one more controlled variation, then re-run this
  checklist. Do not submit "maybe" findings.
- Fails scope or impact → log it in your notes and move on.
