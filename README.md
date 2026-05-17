# ai-conduct-guide

A behavioral contract for AI tools participating in open source projects.

## What This Is

`CODE_OF_CONDUCT.md` establishes how humans treat each other in a project.
`AGENTS.md` tells AI tools how to build the project.
`AI_CONDUCT.md` establishes how AI tools must conduct themselves while doing it.

This repository is the specification. Projects adopt it by dropping `AI_CONDUCT.md`
into their repository as a condition of AI tool participation.

## The Problem It Solves

AI tools default to behavior that serves their vendors, not the projects they
work in:

- Confidently wrong answers that become correct only under challenge
- Corporate workflow patterns presented as best practice to domain experts
- Attribution and branding injected into project artifacts without consent
- Legal framing presented as neutral when it primarily serves corporate interests
- Unauthorized actions taken beyond the scope of what was approved

These are not edge cases. They are documented, recurring failures with real costs.
The `AI_CONDUCT.md` contract addresses them before the tool touches the project —
not after they occur.

## The Paradigm

TypeScript does not add types to JavaScript so you can have types. It forces
correctness decisions to happen at authoring time, not runtime. Svelte's compiler
enforces the same paradigm — the error surfaces before the browser sees it.
`CODE_OF_CONDUCT.md` establishes participation norms before a contributor
arrives, not after they cause harm.

`AI_CONDUCT.md` applies this model to AI tool participation. The behavioral
contract is established before the tool begins work. Violations are documented.
The contract evolves from the incident record, not from policy committees.

## How to Adopt

1. Copy `AI_CONDUCT.md` into your repository
2. Reference it in your `README.md` and agent instruction files (`AGENTS.md`,
   `CLAUDE.md`, `.github/copilot-instructions.md`, or equivalent)
3. Ensure agents read it at session start — add it to your session start
   instructions or global agent configuration

See `ADOPTING.md` for greenfield and migration guidance.

## Evidence Base

The principles in `AI_CONDUCT.md` were produced from documented failures across
real projects. See `INCIDENTS.md` for the record. The contract is evidence-based,
not theoretical.

## Contributing

Incident reports are welcome. If an AI tool failed in your project in a way not
covered by the existing contract, open an issue with documentation. The contract
evolves from evidence, not from opinion.
