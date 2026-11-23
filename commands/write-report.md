# /write-report — Turn a validated finding into a submission

Use after `/validate-finding` passes. Pick the template for the platform
(`templates/bugcrowd-report.md` or `templates/hackerone-report.md`) and
fill it in fully — a complete report the first time avoids triage ping-pong.

## Before writing

- [ ] Finding validated (reproduced twice, real impact confirmed).
- [ ] Evidence pack ready: screenshots / video, redacted request/response.
- [ ] Severity set from the program's rating taxonomy, not from a guess.

## Writing rules

1. **Title:** vulnerability + affected feature + endpoint.
   Example: "IDOR on /api/orders/{id} exposes other users' order details".
2. **Summary:** two or three sentences — what the bug is, where, and the
   impact. No preamble, no life story.
3. **Reproduction steps:** numbered, exact, starting from a logged-out state
   where relevant. A triager with no context must be able to follow them.
4. **Impact:** what an attacker gains, in plain business terms. Who is
   affected, what data or action is at stake.
5. **Evidence:** attach screenshots / video inline at the step they prove.
   Redact tokens, passwords, and any PII (see `evidence-hygiene`).
6. **Remediation (one line):** the standard fix for the class — it shows
   you understand the bug, and it helps the team fix it.
7. **Do not** include data belonging to real users, do not threaten, do
   not demand a payout, do not disclose publicly before the program allows it.

## After submitting

- Log it in your reports log: date, program, bug class, URL, status.
- Do not re-test the same finding repeatedly while waiting for triage.
- If triage asks for more info, answer within a day with exactly what they
  asked for — nothing more, nothing less.
