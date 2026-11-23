---
name: hunt-sqli
description: SQL injection hunting — how it works, where to look, detection steps, confirmation, and severity guidance. Use when testing login forms, search, filters, or any parameter that may reach a database query.
---

# SQL Injection (SQLi)

## Scope rule

Only test in-scope targets with explicit authorization, or lab targets you
own (Juice Shop, PortSwigger labs). Never extract real user data — confirm
the injection with a boolean/time-based proof, then stop and report.
Never run destructive queries (DROP, DELETE, UPDATE) on any live target.

## How it works

User input is concatenated into a SQL query instead of being bound as a
parameter. A crafted input changes the query's meaning — bypassing a login,
returning rows the user should not see, or (in the worst case) reading or
modifying the database.

## Where to look

- Login and password-reset forms (the classic bypass).
- Search boxes, filters, sort/order parameters, category IDs.
- Any numeric ID in a URL or API body (`?id=`, `/users/123`).
- Hidden parameters found during recon (JS files, API docs).

## Detection steps

1. Break it first: send a single quote `'` in the parameter. A SQL error
   message, a 500, or a visibly different response suggests injection.
2. Boolean test: send `1 AND 1=1` vs `1 AND 1=2` (or `' AND '1'='1` vs
   `' AND '1'='2` for strings). Different responses = the input reaches
   the query.
3. Time test (blind): `1 AND SLEEP(5)` (MySQL) or `1; WAITFOR DELAY '0:0:5'`
   (MSSQL). A ~5 second delay confirms blind injection without extracting data.
4. Login bypass (lab / authorized only): `' OR 1=1 --` in the username
   field with any password. A successful login as another user confirms it.

Standard test payloads (public knowledge):

- `'` — break the query
- `' OR 1=1 --` — authentication bypass
- `1 AND 1=1` / `1 AND 1=2` — boolean confirmation
- `' UNION SELECT NULL,NULL --` — probe column count (add NULLs to match)
- `1 AND SLEEP(5)` — time-based confirmation

## Confirming real impact vs false positive

- A bare SQL error message is usually Low severity on its own — it is
  information disclosure, not yet exploitation. Real impact is proven by
  the boolean/time difference or by retrieving data you should not see
  (use your own test data as the proof, never another user's).
- If the app returns the same response for `1=1` and `1=2`, it is probably
  not injectable — move on.
- sqlmap is allowed only on lab targets or where the program explicitly
  permits automated tools. On live programs, manual confirmation is safer
  and usually enough for triage.

## Severity guidance

- Authentication bypass or data extraction of other users' records: High
  to Critical.
- Blind injection confirmed but no data shown: High (data access is
  implied by the confirmed injection).
- Error message only, no exploitable injection: Low / Informational —
  check the program's exclusions first; many programs exclude it.

## Remediation note

Parameterized queries (prepared statements) for all user input; least-
privilege database accounts; generic error messages that reveal no SQL detail.
