# Good First Issues

Small, well-scoped tasks for first-time contributors. Pick one, open a pull
request, and mention this file. No need to ask first — but if you want to
claim a task, open an issue and say you are working on it.

## New skills (follow the format in CONTRIBUTING.md)

1. **Add `hunt-xxe` skill** — XML External Entity injection: how it works,
   where to look, detection steps, standard payloads, impact vs false positive.
2. **Add `hunt-ssti` skill** — server-side template injection: detection with
   `{{7*7}}`-style probes, engine fingerprinting, impact guidance.
3. **Add `hunt-file-upload` skill** — unrestricted file upload: extension
   bypasses, content-type checks, where uploaded files land, impact guidance.
4. **Add `hunt-subdomain-takeover` skill** — dangling DNS records, how to
   check for takeover, safe proof without claiming the subdomain.

## Templates & docs

5. **Add an Intigriti report template** (`templates/intigriti-report.md`) —
   same structure as the Bugcrowd/HackerOne templates, adapted to Intigriti's
   submission fields.
6. **Translate the README into Hindi** (`README-hi.md`) — the bundle's
   audience includes many Hindi-speaking beginners.

## Quality

7. **Add a payload quick-reference** (`docs/payload-cheatsheet.md`) — one page
   collecting the most-used test payloads from all hunt skills, with a line
   each on what a "hit" looks like.
8. **Fix or clarify anything confusing** — if a sentence confused you as a
   beginner, it will confuse the next reader. Small clarity PRs are welcome.
