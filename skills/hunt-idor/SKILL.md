---
name: hunt-idor
description: Broken access control and IDOR hunting — the beginner's best first-win bug class. Use when testing any endpoint that takes an object ID, user ID, or resource reference.
---

# Broken Access Control / IDOR

## Scope rule

Only test in-scope targets with explicit authorization, or lab targets you
own. Test with TWO accounts you created yourself — never access a real
user's data. The moment a request returns another user's record, stop:
screenshot the proof (your own test data in the other account), do not
browse further, and report.

## How it works

The app checks that you are logged in, but not that the object you asked
for belongs to you. Changing an ID in a request returns or modifies
someone else's data. OWASP ranks broken access control #1 (A01, 2025)
because it is common, easy to test, and high impact.

## Where to look

- Numeric or UUID object IDs in URLs and API bodies: `/orders/123`,
  `/api/users/456/profile`, `/invoices/789.pdf`.
- Profile, settings, address book, order history, messages, files.
- "My" endpoints that still take an ID parameter.
- Bulk / list endpoints that accept a user ID filter.
- Predictable file paths: `/uploads/<userid>/document.pdf`.

## Detection steps

1. Create two test accounts, A and B. Give each a distinct record
   (order, note, profile field) with a recognizable value.
2. As A, note the request that fetches A's record (URL + parameters).
3. Replay the same request as B, changing only the ID to A's record.
4. Compare: if B sees A's data, it is IDOR. If B gets 403 / 404 / a
   redirect to their own data, access control works.
5. Test the write side too: try updating or deleting A's record as B
   (only with your own test records — never real data).
6. Test variations: sequential IDs (123 → 124), UUIDs from your own
   account, and IDs in POST bodies and headers — not just URLs.

Standard test patterns (no payloads needed — this is logic testing):

- Change `id=100` to `id=101` (your other test account's record).
- Remove the ID parameter entirely — does the app default to another
  user's record?
- Swap the ID in the body while keeping your own session cookie.
- Try the same request with no session at all (unauthenticated access).

## Confirming real impact vs false positive

- Confirmed: account B reads, modifies, or deletes account A's data.
  Screenshot both accounts' views.
- Not a finding: the app returns your own data regardless of the ID
  (it ignores the parameter), or returns a generic error.
- Be careful with shared/public resources (public profiles, public
  listings) — data meant to be public is not IDOR.

## Severity guidance

- Read access to another user's private data (PII, orders, messages,
  documents): High.
- Write/delete access to another user's data: High to Critical.
- Access to another user's non-sensitive data (e.g. public-ish settings):
  Medium. Check the program's VRT — some treat low-sensitivity IDOR as Low.

## Remediation note

Server-side authorization check on every object access: verify the
requesting user owns the object (or has an explicit role granting access)
before returning or modifying it. Never trust client-supplied IDs alone.
