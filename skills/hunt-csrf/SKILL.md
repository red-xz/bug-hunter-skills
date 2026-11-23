---
name: hunt-csrf
description: Cross-site request forgery hunting. Use when testing state-changing actions — profile updates, password/email changes, purchases, deletions — for missing anti-CSRF protection.
---

# Cross-Site Request Forgery (CSRF)

## Scope rule

Only test in-scope targets with explicit authorization, or lab targets you
own. Test with your own accounts only. Build the PoC against your own
session — never trick a real user into submitting a forged request.

## How it works

The browser automatically sends cookies with cross-site requests. If a
state-changing request has no unpredictable token (or another check tying
it to the site), a malicious page can make the victim's browser perform
the action — change their email, transfer money, delete data — while they
are logged in.

## Where to look

- Profile updates: email change, password change, name, phone.
- Money actions: transfers, purchases, payout settings.
- Destructive actions: delete account, delete content, revoke access.
- Any POST/PUT/DELETE that changes state — especially ones that work
  with a simple form POST (no custom headers, no JSON-only body).

## Detection steps

1. Capture a state-changing request in the proxy. Look for an anti-CSRF
   token (hidden form field or custom header).
2. Replay the request with the token removed. If it still succeeds, the
   token is not enforced.
3. Replay with a wrong/empty token, and with a token from another session.
   Success in any of these cases = missing or broken CSRF protection.
4. Build a minimal HTML PoC: an auto-submitting form on a page you control
   that sends the same request. Open it while logged in as your test
   account. If the action happens, CSRF is confirmed.
5. Check the cookie flags: `SameSite=Lax` (the modern default) blocks
   most cross-site POST CSRF. If the session cookie has no SameSite
   attribute, the attack surface is wider — note it in the report.

## Confirming real impact vs false positive

- Confirmed: the forged request from another origin performs the action
  on your test account.
- Not a finding: the request requires a custom header or a JSON content
  type that a plain form cannot send (CORS preflight blocks it), or the
  token is properly validated.
- Impact depends on the action: email/password change and money movement
  are serious; changing a display name is not.

## Severity guidance

- CSRF on account takeover actions (email/password change): High.
- CSRF on money or destructive actions: Medium to High.
- CSRF on low-impact settings (theme, display name): Low — check the
  program's exclusions; often not accepted.

## Remediation note

Unpredictable per-session anti-CSRF tokens validated on every state-
changing request; `SameSite=Lax` (or Strict) on session cookies; verify
the Origin/Referer header as a secondary check.
