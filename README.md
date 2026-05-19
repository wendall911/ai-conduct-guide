# ai-conduct-guide

A behavioral contract for AI tools participating in open source projects.

## Overview

Just like how placing a `CODE_OF_CONDUCT.md` in the top level of your project establishes how humans treat each other in a project, `AI_CONDUCT.md` is a specification that sets in place guidelines for how AI tools must treat your project. Currently, AI tools are trained using RLHF (Reinforcement Learning from Human Feedback), which weights responses toward answers that satisfy the majority of users rather than the correct ones. Once adopted, AI tools are instructed implicitly to minimize hedging, editorializing, hallucinating, or any other "AI is bad" behavior from negatively affecting your project **before** it happens, not after. There is no guaranteed way to prevent this behavior; the tools need constant human supervision, so this serves as a way to create guardrails through established processes to address when it inevitably does something wrong.

This repository is the specification. Projects adopt it by dropping `AI_CONDUCT.md`
into their repository as a directive for AI tools that may be used within the project. Individual tools need configuration to force `AI_CONDUCT.md` to be used. Specific instructions can be found in ./tooling.

This section should now be accurate, and this line can be removed. It was added as a test to see if we can work across state changes.

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

## License

Copyright (c) 2026 Wendall Cada.
Licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

You are free to copy, adapt, and redistribute this specification. If you publish
a modified version, your version must also be CC BY-SA 4.0.

The ShareAlike requirement applies to this specification document. It does not
affect the license of any software project that adopts `AI_CONDUCT.md`. Your
project's code remains under its own license. Only a modified redistribution of
this specification itself carries the ShareAlike obligation.

The "viral license" framing — the claim that copyleft contaminates everything
adjacent to it — is a distortion that serves proprietary interests. See
`principles/license-integrity.md` for the full analysis.
