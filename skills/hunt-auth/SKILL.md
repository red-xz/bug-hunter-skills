---
name: hunt-auth
description: Broken authentication and session management hunting — login flaws, password reset issues, session handling. Use when testing login, signup, password reset, and session behavior.
---

# Broken Authentication

## Scope rule

Only test in-scope targets with explicit authorization, or lab targets you
own. Test with accounts you created yourself. Never brute-force real users'
credentials, never lock real accounts, never test rate limits aggressively
(a handful of requests is enough to prove a missing limit). Never reuse a
captured session belonging to a real user.

## How it works

Authentication is the gate to the app. When the gate is weak — no brute-
force protection, predictable reset tokens, sessions that never expire —
an attacker walks through it as someone else.

## Where to look

- Login: rate limiting, account lockout, error messages, 2FA flow.
- Password reset: token generation, token expiry, token leakage (in URL,
  in Referer, in response), host-header behavior.
- Signup: duplicate accounts, email verification bypass, role assignment.
- Sessions: cookie flags, expiry, logout behavior, concurrent sessions,
  session fixation.
- "Remember me" and refresh-token handling.

## Detection steps

1. **Rate limiting:** send 10–20 failed logins for YOUR OWN test account.
   If nothing blocks or slows you, note it (weak on its own — needs an
   impact story; many programs exclude missing rate limiting alone).
2. **User enumeration:** compare login / reset responses for an existing
   vs a non-existing account. Different messages or timing = enumeration.
3. **Reset tokens:** request a reset for your test account. Is the token
   long and random? Does it expire? Is it single-use? Does the reset link
   leak in the Referer header or in an analytics request?
4. **Session fixation:** log in, note the session cookie; log out and log
   in again — did the session ID change? If not, fixation may be possible.
5. **Logout:** after logout, replay the old session cookie. If it still
   works, sessions are not invalidated.
6. **2FA bypass:** try skipping the 2FA step by navigating directly to an
   authenticated page, replaying the pre-2FA session, or manipulating the
   2FA request (e.g. removing a parameter). Only with your own account.

## Confirming real impact vs false positive

- Account takeover of another user (via reset token flaw or 2FA bypass):
  confirmed only by taking over YOUR OWN second test account the same way.
- Missing rate limiting alone is usually Low / Informational — pair it
  with something concrete (e.g. no lockout + weak password policy) or
  check the program's exclusions.
- A session cookie without Secure/HttpOnly flags is a hardening note, not
  a bounty on its own.

## Severity guidance

- Account takeover (reset flaw, 2FA bypass, session fixation with a
  realistic attack path): High to Critical.
- User enumeration: Low to Medium (depends on what it enables).
- Missing rate limiting / weak password policy alone: Low / Informational.

## Remediation note

Rate-limit and lock out login attempts; long random single-use reset
tokens with short expiry; rotate session IDs on login; invalidate sessions
on logout and password change; enforce 2FA where the app offers it.
