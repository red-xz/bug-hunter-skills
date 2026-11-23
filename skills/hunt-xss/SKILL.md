---
name: hunt-xss
description: Cross-site scripting hunting — reflected, stored, and DOM XSS. Use when testing search boxes, comments, profiles, or any place user input is displayed back to users.
---

# Cross-Site Scripting (XSS)

## Scope rule

Only test in-scope targets with explicit authorization, or lab targets you
own. Use harmless proof payloads (`alert(1)`, `console.log`) — never steal
real session tokens, never run payloads against real users, never exfiltrate
data. Confirm the execution, screenshot it, stop.

## How it works

User input is reflected into a page without proper encoding, so the browser
treats it as code. The attacker's script then runs in a victim's browser
session — it can read page content, perform actions as the victim, or
redirect them.

Three types:

- **Reflected:** the payload comes back in the immediate response (search
  results, error messages). Needs a victim to click a crafted link.
- **Stored:** the payload is saved (comment, profile, ticket) and executes
  for everyone who views it. Higher impact.
- **DOM:** JavaScript on the page unsafely writes user-controlled data into
  the DOM (e.g. `innerHTML` from a URL fragment). No server round-trip.

## Where to look

- Search boxes, filters, and any reflected parameter.
- Comments, reviews, profile fields (name, bio), support tickets, file
  names shown to other users.
- URL fragments and parameters consumed by client-side JS.
- Anywhere the app renders user content as HTML (rich-text editors,
  markdown renderers, SVG uploads).

## Detection steps

1. Probe with a unique marker first, e.g. `xss123test`, and find where it
   appears in the response / DOM. This tells you the reflection context
   (HTML body, attribute, script block, JS string).
2. Match the payload to the context:
   - HTML body: `<script>alert(1)</script>`
   - Inside an attribute: `" onmouseover="alert(1)`
   - Inside a JS string: `'-alert(1)-'`
   - Event handler: `<img src=x onerror=alert(1)>`
   - SVG / HTML injection: `<svg onload=alert(1)>`
3. Stored: submit the payload in a field, then view it as a second test
   account. Execution in the second account confirms stored XSS.
4. DOM: trace where `location.hash`, URL params, or `postMessage` data
   flow into sinks (`innerHTML`, `document.write`, `eval`, jQuery `.html()`).

Standard test payloads (public knowledge):

- `<script>alert(1)</script>`
- `<img src=x onerror=alert(1)>`
- `<svg onload=alert(1)>`
- `"><script>alert(1)</script>`
- `javascript:alert(1)` (for href contexts)

## Confirming real impact vs false positive

- The payload must actually execute (alert fires / console output) — a
  payload merely visible in the page source as encoded text is not XSS.
- Self-XSS (only executes for the attacker themself, e.g. in their own
  profile preview) is almost always excluded by programs. Real impact
  needs another user (or an admin) to trigger it.
- If a WAF blocks the payload, that is not a finding — try a different
  context, do not spend hours on filter bypass as a beginner.

## Severity guidance

- Stored XSS affecting other users or admins: High (Critical if it can
  reach an admin session and take over accounts).
- Reflected XSS with a plausible victim flow: Medium.
- Self-XSS or XSS requiring heavy victim interaction: Low / N/A —
  check program exclusions first.

## Remediation note

Context-aware output encoding on render; Content Security Policy as
defense-in-depth; never insert untrusted data with `innerHTML` or
equivalent sinks — use `textContent` and framework auto-escaping.
