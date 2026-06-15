# ai-conduct-guide - Project Context

## What This Is

The specification repository for `AI_CONDUCT.md` -- a behavioral contract for
AI tools participating in open source projects. This is the CODE_OF_CONDUCT
equivalent for AI tools: established before the tool begins work, not after
failures occur.

This repository operates under its own `AI_CONDUCT.md`. The self-governing
property is not incidental -- it is the demonstration that the contract is
operational, not theoretical.

## Project Structure

- `AI_CONDUCT.md` - the contract and enforcement rules; the primary artifact; the adopter copy
- `README.md` - human entry point for overall project
- `ADOPTING.md` - greenfield and migration guidance (tiered adoption rewrite pending)
- `principles/` - reasoning behind each contract clause for human consumption
- `principles/README.md` - human entry point for principles
- `docs/tooling/` - tool-specific implementation notes; placeholders only for non-active tools
- `docs/` - context injection helpers and reference documentation; some files serve both purposes.
- `.automation/context.md` - per-project metadata, loaded by `/tape` after `AI_CONDUCT.md`