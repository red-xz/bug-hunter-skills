---
name: hunt-ssrf
description: Server-side request forgery hunting. Use when testing URL fetchers, webhooks, image/PDF processors, or any feature where the server fetches a user-supplied URL.
---

# Server-Side Request Forgery (SSRF)

## Scope rule

Only test in-scope targets with explicit authorization, or lab targets you
own. Use a collaborator / request-bin URL you control, or a harmless
internal address (localhost) that only proves the server made the request.
Never probe internal services beyond proving the request happened, never
access cloud metadata endpoints on a live target beyond a non-destructive
read of a non-sensitive field — and stop at the first proof.

## How it works

The server fetches a URL the user supplies. If it does not restrict the
destination, an attacker points it at internal services — localhost admin
panels, internal APIs, or cloud metadata endpoints (e.g. 169.254.169.254)
that can leak credentials.

## Where to look

- URL preview / link unfurling, "fetch from URL" upload features.
- Webhooks and callback URLs.
- Image, PDF, or document processing that fetches remote files.
- Server-side PDF generation, screenshot services, RSS importers.
- Any parameter named url, link, fetch, src, image, callback, redirect.

## Detection steps

1. Supply a URL you control (a request-bin / Burp Collaborator address)
   and see if the server makes a request to it. An incoming hit proves SSRF.
2. Try `http://127.0.0.1/` and `http://localhost/` — a different response
   (or an error naming the internal service) suggests internal reach.
3. Try `http://169.254.169.254/` only to see if the request is attempted —
   do not extract credentials on a live target. The attempted request plus
   a distinguishable response is enough for the report.
4. Note filter behavior: blocked hosts, allowed schemes, redirects
   followed (open redirect + SSRF chains are common).

Standard test payloads (public knowledge):

- `http://127.0.0.1/` / `http://localhost/`
- `http://169.254.169.254/latest/meta-data/` (cloud metadata — proof only)
- `http://[::1]/` (IPv6 localhost)
- `http://0.0.0.0/`
- Your collaborator URL, e.g. `https://<your-id>.oastify.com/test`

## Confirming real impact vs false positive

- Confirmed: your collaborator receives a hit from the target server, or
  the server returns content from an internal address.
- Not a finding: the server fetches the URL but only returns a generic
  error with no internal reach, or it validates the URL against an
  allowlist and rejects yours.
- Blind SSRF (no response shown) still counts if the collaborator hit
  proves the request — document the hit with a timestamp.

## Severity guidance

- SSRF reaching cloud metadata or internal services: High to Critical.
- SSRF to arbitrary external URLs only (no internal reach proven):
  Medium — it can still be used for request smuggling or internal
  port scanning, but impact is harder to prove.
- URL fetch with a strict allowlist and no bypass: not a finding.

## Remediation note

Allowlist permitted hosts and schemes; resolve and validate the final IP
after redirects; block private/reserved IP ranges (including IPv6 and
encoded forms); disable redirects or validate each hop.
