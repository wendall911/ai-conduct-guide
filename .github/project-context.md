# ai-conduct-guide — Project Context

## What This Is

The specification repository for `AI_CONDUCT.md` — a behavioral contract for
AI tools participating in open source projects. This is the CODE_OF_CONDUCT
equivalent for AI tools: established before the tool begins work, not after
failures occur.

This repository operates under its own `AI_CONDUCT.md`. The self-governing
property is not incidental — it is the demonstration that the contract is
operational, not theoretical.

## Migration Status

**Phase 1 active.** See `MIGRATION.md` for the full plan and checklist.

This project is dogfooding its own spec in real time. `.github/` contents
(including this file) are temporary scaffolding for the working environment —
not migration artifacts. When the migration completes, this file moves to
`.automation/context.md`.

## Target State

- `AI_CONDUCT.md` — pure contract only, no per-project metadata. Versionable,
  drop-in replaceable. Adopters copy this file as-is and upgrade by replacing it.
- `.automation/context.md` — per-project metadata, loaded by `/tape` after
  `AI_CONDUCT.md` if present. Each repo in a multi-repo setup has its own.
- `templates/` — removed. Root `AI_CONDUCT.md` is the adopter copy.
- `.github/guardrails-agent.md`, `.github/guardrails.md`, `.github/project-context.md`
  — scaffolding, removed when migration completes.

## Scope

Migration produces documentation only. Signal scripts (`/tape`, `/state`) are
documented as tool-generic script blocks in `tooling/agents/` — reference
implementation for a single-repo adopter. No actual script files are committed
to this repository as migration deliverables. Tool-specific exploration shapes
the tool-agnostic architecture; no specific tool's documentation is a migration
target.

## Branch Convention

- `main` is the only branch
- Releases are tagged following semver
- HEAD is always the current specification version

## Project Structure

- `AI_CONDUCT.md` — the contract; the primary artifact; the adopter copy
- `README.md` — human entry point
- `ADOPTING.md` — greenfield and migration guidance (tiered adoption rewrite pending)
- `INCIDENTS.md` — incident record and citation requirements
- `principles/` — reasoning behind each contract clause
- `tooling/` — tool-specific implementation notes; placeholders only for non-active tools
- `MIGRATION.md` — current working state tracking (temporary scaffolding)
- `.github/` — temporary scaffolding for dogfooding environment

## Contributing

See `INCIDENTS.md` for how to contribute incident reports.
Amendments to `AI_CONDUCT.md` require documented incidents as evidence.
Opinion-based amendments are not accepted.
