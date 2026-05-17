# ai-conduct-guide — Project Context

## What This Is

The specification repository for `AI_CONDUCT.md` — a behavioral contract for
AI tools participating in open source projects. This is the CODE_OF_CONDUCT
equivalent for AI agents: established before the tool begins work, not after
failures occur.

## This Repository Governs Itself

This repository operates under its own `AI_CONDUCT.md`. The guardrails in
`.github/guardrails.md` implement the contract for agents working in this repo.
The self-governing property is not incidental — it is the demonstration that
the contract is operational, not theoretical.

## Branch Convention

- `main` is the only branch
- Releases are tagged following semver
- HEAD is always the current specification version

## Deployment Model

- Static documentation; no build step
- GitHub is the primary distribution point
- Future: ai-conduct.guide as the canonical website

## Contributing

See INCIDENTS.md for how to contribute incident reports.
Amendments to AI_CONDUCT.md require documented incidents as evidence.
Opinion-based amendments are not accepted.

## Project Structure

- `AI_CONDUCT.md` — the canonical template; the primary artifact
- `README.md` — human entry point
- `ADOPTING.md` — greenfield and migration guidance
- `INCIDENTS.md` — incident record and citation requirements
- `principles/` — reasoning behind each contract clause
- `tooling/` — tool-specific implementation notes
- `.github/` — self-governance files
