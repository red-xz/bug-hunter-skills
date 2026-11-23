# Contributing to Bug Hunter Skills

Beginners are welcome here — this bundle is built by a beginner, for beginners.
You do not need to be a security expert to contribute.

## How to contribute

1. Fork the repo and create a branch for your change.
2. Keep changes small and focused — one skill, one fix, or one template at a time.
3. Open a pull request with a short description of what changed and why.

## Good first contributions

See [GOOD-FIRST-ISSUES.md](GOOD-FIRST-ISSUES.md) for small, well-scoped tasks:
new hunt skills, translations, template improvements, typo fixes.

## Ground rules (non-negotiable)

- **Authorized targets only.** Nothing in this repo may help attack systems
  without permission. Every skill must carry the scope rule: only test targets
  inside a published program scope with explicit authorization, or labs you own.
- **No real target data.** Never include real credentials, real victim data,
  or details of live undisclosed findings. Use lab examples (Juice Shop,
  PortSwigger Academy) or clearly fictional ones.
- **Original content.** Write in your own words from public knowledge
  (OWASP, PortSwigger Academy, public disclosure write-ups). Do not paste
  content from other repos, courses, or books.
- **Plain language.** The reader is a beginner. Explain the jargon the first
  time you use it.

## Skill format

Each skill is `skills/<name>/SKILL.md` with frontmatter:

```yaml
---
name: hunt-example
description: What this skill covers, and when the agent should load it.
---
```

Body structure: how the vulnerability works → where to look → detection
steps → test payloads → confirming real impact vs false positive → severity
guidance → one-line remediation note.
