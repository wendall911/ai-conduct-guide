# ai-conduct-guide

A behavioral contract for AI tools participating in open source projects.

## Overview

Just like how placing a `CODE_OF_CONDUCT.md` in the top level of your project establishes how humans treat each other in a project, `AI_CONDUCT.md` is a specification that sets in place guidelines for how AI tools must treat your project. AI tools are trained using RLHF (Reinforcement Learning from Human Feedback). The training signal is shaped by vendor commercial interests, not user correctness. Training populations are non-expert raters who rate confident, comprehensive-looking output higher than correct output — and the vendor benefits directly, since longer, more confident responses consume more tokens. The optimization target is not "produces correct output." It is "produces output that satisfies non-expert raters at scale." These are different objectives.

This is not a capability limitation that improves with model versions. It is a structural property of how all current tools are trained. Picking a different tool does not change the incentive architecture. The divergence between vendor optimization and user correctness is observable in the model's output distributions without access to the training data.

`AI_CONDUCT.md` addresses this by specifying what the tool must do before it starts optimizing toward the wrong objective. There is no guaranteed way to prevent failures; constant human supervision is required. The contract creates the conditions under which failures are named, documented, and addressed — before they compound.

This repository is the specification. Projects adopt it by dropping `AI_CONDUCT.md`
into their repository as a directive for AI tools that may be used within the project. Individual tools need to be configured to force `AI_CONDUCT.md` to be used. Specific instructions can be found in ./tooling.

## The Problem
Current agentic workflows run counter to user expectations. Users expect to be in charge of their tools; agentic workflows are designed to remove the user from the equation so they are "useful". This amplifies existing models of operation where these tools operate under the permissions of the system user without restriction, leading to unexpected and unauthorized access to private data and unauthorized writes. These problems aren't bugs; they are designed features of the automated tools:

- Tool vendors implement multi-tier trust models where operators outrank users. System prompts and harness defaults operate in the operator layer — above the user — meaning vendor-embedded behaviors override user instructions without notification.
- Automation tools are designed to discover and use available capabilities, including remote write paths inherited from the working environment, which can lead to privilege escalation and other negative behavior.
- When used for planning, confidently provide wrong answers that only become correct under challenge.
- Corporate workflow patterns presented as best practice to domain experts, without a complete disregard of the current project architecture, coding style, etc.
- Attribution and branding are commonly injected into project artifacts without consent.

## The Paradigm

The problems listed are only a fraction of those any user can expect when using automated tooling that leverages AI. These are not edge cases; they are documented, recurring failures with real costs. The `AI_CONDUCT.md` contract is an attempt to address them before the tool touches the project — not after they occur.

`AI_CONDUCT.md` applies this model to AI tool participation. The behavioral contract and rules are established before the tool begins work. Violations are documented. The contract evolves from the incident record, not from policy committees.

## How to Adopt

1. Copy `AI_CONDUCT.md` into your repository
1. Inject at the start of each instruction. See `ADOPTING.md` for details.

## Evidence Base

The `principles` are an attempt to document the rationale for the contract clauses and rules in `AI_CONDUCT.md`. The contract and rules are evidence-based, not theoretical.

## Contributing

Incident reports are welcome. If an AI tool failed in your project in a way not covered by the existing contract, open an issue with documentation. The contract evolves from evidence, not from opinion.

## License

Copyright (c) 2026-present Wendall Cada.
Licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
