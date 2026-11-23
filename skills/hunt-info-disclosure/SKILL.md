---
name: hunt-info-disclosure
description: Information disclosure hunting — error messages, exposed files, metadata, and debug output. Use when probing error handling, forgotten files, and responses that reveal more than they should.
---

# Information Disclosure

## Scope rule

Only test in-scope targets with explicit authorization, or lab targets you
own. If you find exposed data belonging to real users: stop, do not
download or browse it, redact it from your evidence, and report the
exposure itself. Never publish or share disclosed data.

## How it works

Apps leak information in the gaps: verbose error messages, stack traces,
debug endpoints, forgotten backup files, metadata in responses, and
sensitive data in client-side code. Each leak is small on its own; the
impact depends on what exactly is exposed.

## Where to look

- Error pages: trigger 404s, 500s, and invalid input — look for stack
  traces, SQL fragments, file paths, framework versions.
- Forgotten files: `.git/`, `.env`, `backup.zip`, `.bak`, `.old`,
  `config.php~`, `sitemap.xml`, `robots.txt`, `/.well-known/`.
- API responses: extra fields the UI does not show (internal IDs,
  emails, tokens, admin flags, password hashes).
- HTTP headers: version banners, internal hostnames, debug headers.
- JS bundles: API keys, internal endpoints, commented-out code.
- Metadata endpoints and public buckets referenced by the app.

## Detection steps

1. Cause errors deliberately (bad IDs, bad JSON, missing parameters) and
   read the full response — body and headers.
2. Request common sensitive paths at a polite rate (only where the
   program allows active probing): `/.git/HEAD`, `/.env`, `/backup.zip`,
   `/config.json`, `/debug`, `/actuator`, `/phpinfo.php`.
3. Compare API responses against what the UI displays — any field the UI
   hides but the API returns is a candidate.
4. Grep downloaded JS for `api_key`, `secret`, `token`, `password`,
   internal hostnames. A key is a finding only if it is valid and gives
   real access — verify with a single harmless call, then stop.

## Confirming real impact vs false positive

- Confirmed: the response exposes credentials, tokens, PII, internal
  network details, or source code that an attacker can use.
- Weak on its own: version numbers, server banners, generic stack traces
  with no data. Most programs exclude these — check before reporting.
- A `.git` directory that serves real objects is serious; a `.git/HEAD`
  that 404s is nothing.

## Severity guidance

- Exposed credentials, tokens, private keys, or real user PII: High to
  Critical (report the exposure; do not use the credentials).
- Source code or internal API exposure: Medium to High.
- Version disclosure / generic errors alone: Low / Informational —
  usually excluded.

## Remediation note

Generic error pages in production; never deploy `.git`, `.env`, backups,
or debug endpoints to public servers; strip sensitive fields from API
responses; rotate any exposed secret immediately.
