---
name: hunt-open-redirect
description: Open redirect hunting. Use when testing login/logout flows, password resets, or any parameter that controls where the user is sent next.
---

# Open Redirect

## Scope rule

Only test in-scope targets with explicit authorization, or lab targets you
own. Use a harmless destination you control (e.g. example.com) as the
proof — never redirect real users, never chain into a phishing page.

## How it works

The app takes a destination from the URL (often `?next=`, `?redirect=`,
`?returnUrl=`) and redirects the user there without validating it. An
attacker crafts a link on the trusted domain that lands the victim on a
malicious site — a classic phishing setup.

## Where to look

- Login / logout redirects (`?next=/dashboard`).
- Password reset and email-verification links.
- OAuth / SSO callbacks.
- "Continue" links, language switchers, and link shorteners.
- Any parameter named next, redirect, return, returnUrl, url, goto, dest.

## Detection steps

1. Find a redirect parameter and replace its value with
   `https://example.com`. If the app redirects there, it is open.
2. Try protocol-relative `//example.com` and path tricks
   (`/\\example.com`, `https://trusted.com@evil.com`) to test filters.
3. Note whether the redirect is a 302/301 from the server or a
   client-side `window.location` assignment — both count, but server-side
   is cleaner to report.

Standard test payloads (public knowledge):

- `https://example.com`
- `//example.com`
- `/\example.com`
- `https://trusted-domain.com@evil.com` (replace with the real domains)

## Confirming real impact vs false positive

- Confirmed: the app redirects to an external domain you supplied.
- Not a finding: the app only redirects to paths on its own domain, or
  validates against an allowlist.
- Impact is usually Low to Medium on its own. It becomes interesting when
  chained — e.g. an open redirect that also leaks a token in the URL, or
  one on an OAuth callback. Document the chain if you find one.

## Severity guidance

- Plain open redirect: Low (many programs list it as accepted but low
  value; some exclude it — check the program's exclusions first).
- Open redirect that leaks a token or enables account takeover via OAuth:
  High — report the chain, not just the redirect.

## Remediation note

Allowlist redirect destinations (relative paths only, or a fixed list of
trusted hosts); never trust a raw URL parameter for redirection.
