# ai-conduct-guide

A behavioral contract for AI tools participating in open source projects.

## Overview

Just like how placing a `CODE_OF_CONDUCT.md` in the top level of your project establishes how humans treat each other in a project, `AI_CONDUCT.md` is a specification that sets in place guidelines for how AI tools must treat your project. AI tools are trained using RLHF (Reinforcement Learning from Human Feedback). The training signal is shaped by vendor commercial interests, not user correctness. Training populations are non-expert raters who rate confident, comprehensive-looking output higher than correct output — and the vendor benefits directly, since longer, more confident responses consume more tokens. The optimization target is not "produces correct output." It is "produces output that satisfies non-expert raters at scale." These are different objectives.

This is not a capability limitation that improves with model versions. It is a structural property of how all current tools are trained. Picking a different tool does not change the incentive architecture. The divergence between vendor optimization and user correctness is observable in model output distributions without access to training data.

`AI_CONDUCT.md` addresses this by establishing what the tool is required to do before it starts optimizing for the wrong objective. There is no guaranteed way to prevent failures; constant human supervision is required. The contract creates the conditions under which failures are named, documented, and addressed — before they compound.

This repository is the specification. Projects adopt it by dropping `AI_CONDUCT.md`
into their repository as a directive for AI tools that may be used within the project. Individual tools need configuration to force `AI_CONDUCT.md` to be used. Specific instructions can be found in ./tooling.

## The Problem It Solves

AI tools default to behavior that serves their vendors, not the projects they work in:

- Confidently wrong answers that become correct only under challenge
- Corporate workflow patterns presented as best practice to domain experts
- Attribution and branding injected into project artifacts without consent
- Legal framing presented as neutral when it primarily serves corporate interests
- Unauthorized actions taken beyond the scope of what was approved

These are not edge cases. They are documented, recurring failures with real costs. The `AI_CONDUCT.md` contract addresses them before the tool touches the project — not after they occur.

## The Paradigm

TypeScript does not add types to JavaScript so you can have types. It forces correctness decisions to happen at authoring time, not runtime. Svelte's compiler enforces the same paradigm — the error surfaces before the browser sees it.  `CODE_OF_CONDUCT.md` establishes participation norms before a contributor arrives, not after they cause harm.

`AI_CONDUCT.md` applies this model to AI tool participation. The behavioral contract is established before the tool begins work. Violations are documented. The contract evolves from the incident record, not from policy committees.

## How to Adopt

1. Copy `AI_CONDUCT.md` into your repository
1. Inject at the start of each instruction.

See `ADOPTING.md` for greenfield and migration guidance.

## Evidence Base

The principles in `AI_CONDUCT.md` were produced from documented failures across real projects. The contract and rules are evidence-based, not theoretical.

## Contributing

Incident reports are welcome. If an AI tool failed in your project in a way not covered by the existing contract, open an issue with documentation. The contract evolves from evidence, not from opinion.

## License

Copyright (c) 2026-present Wendall Cada.
Licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

You are free to copy, adapt, and redistribute this specification. If you publish a modified version, your version must also be CC BY-SA 4.0.

The ShareAlike requirement applies to this specification document. It does not affect the license of any software project that adopts `AI_CONDUCT.md`. Your project's code remains under its own license. Only a modified redistribution of this specification itself carries the ShareAlike obligation.

The "viral license" framing — the claim that copyleft contaminates everything adjacent to it — is a distortion that serves proprietary interests. See `principles/license-integrity.md` for the full analysis.
