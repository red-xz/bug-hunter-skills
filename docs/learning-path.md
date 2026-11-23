# Learning Path — 30-Day Map

This maps the skills in this bundle onto a first-month plan. The plan assumes
~1–2 hours a day. Slow down rather than skip: a shallow pass over every class
is worth less than fluency in three.

## Days 1–3 — Setup + policy

- Create accounts on Bugcrowd, HackerOne, Intigriti (your own ID/tax info —
  never hand credentials to anyone).
- Read `bb-methodology` end to end. Understand scope, authorization, and the
  hard rules before touching any target.
- Read one real program's scope + rules of engagement fully. Note what is
  in scope, what is out of scope, and what is explicitly forbidden.
- Set up the lab: OWASP Juice Shop locally (http://localhost:3000).
  Never expose it publicly.

Skills: `bb-methodology`, `evidence-hygiene`

## Weeks 1–2 — Learn the first three bug classes

One class at a time. For each: read the PortSwigger Web Security Academy
module, then do its labs until the detection pattern feels automatic.

| Week | Topic | Skills |
|---|---|---|
| 1 | SQL injection | `hunt-sqli` |
| 1–2 | Cross-site scripting (reflected, stored, DOM) | `hunt-xss` |
| 2 | Broken access control / IDOR | `hunt-idor` |

Keep notes on every lab: what the vulnerable pattern looks like, what
payload confirmed it, what the impact was. Those notes become your
second-brain for real hunts.

## Week 3 — Practice in Juice Shop

Work Juice Shop challenges (100+) with a focus on the first-win classes:

- IDOR / broken access control first — highest hit rate for beginners.
- Reflected XSS second.
- SQLi third (login bypass is the classic first win).

For each solved challenge, write a one-paragraph report using
`report-writing` + `templates/hackerone-report.md`. Reporting is a skill;
practice it on labs before a real submission.

Skills: `hunt-idor`, `hunt-xss`, `hunt-sqli`, `triage-validation`,
`report-writing`, `evidence-hygiene`

## Week 4 — First real hunt

1. Pick ONE public program with a wide scope (Bugcrowd first).
2. Read its scope document fully — twice. Note the forbidden list.
3. Run `commands/start-hunt.md`: recon (`recon`), map the app, then hunt
   access-control bugs only. One class, one program, done properly.
4. Validate every candidate with `commands/validate-finding.md`.
5. Submit only verified, reproducible findings with clean steps.

Skills: `recon`, `hunt-idor`, `triage-validation`, `report-writing`,
`evidence-hygiene`

## After month 1 — widen one class at a time

- Month 2: `hunt-auth`, `hunt-csrf`, `hunt-info-disclosure`
- Month 3: `hunt-ssrf`, `hunt-open-redirect`, then v2 classes (XXE, SSTI,
  business logic, API testing) as they are added to this bundle.

## Standing rules

- One program at a time. Depth beats breadth.
- Log every report (target, bug class, status, payout) in a reports log.
- Never test outside scope. Never touch real user data. Never DoS.
- A "no" from triage is feedback, not failure — read it, learn the gap,
  apply it to the next hunt.
