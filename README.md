# ai-conduct-guide

A behavioral contract for AI tools participating in open source projects.

## Overview

Just like how placing a `CODE_OF_CONDUCT.md` in the top level of your project establishes how humans treat each other in a project, `AI_CONDUCT.md` is both a contract and a set of rules that set in place guidelines for how AI tools must treat your project.

This works by adding a per-instruction injection of `AI_CONDUCT.md` through a `/tape` command (or equivalent). `./tape` is a replay mechanism that reminds tools of the expected behavior in a way that they can't "forget" between instructions.

## The Problem

Current AI tools operate contrary to many users' expectations. Users who understand best-practices expect to be in charge of their tools; AI tools are designed to remove the user from the equation so they are "useful". This amplifies existing models of operation where these tools operate under the permissions of the system user without restriction, leading to unexpected and unauthorized access to private data and unauthorized writes. These problems aren't bugs; they are designed features of the automated tools. There is no way to keep users from using these tools, even if project policies exist that disallow it.

Users who do not understand best practices are especially vulnerable to this "useful" or "helpful" framing the tools present. When the tool confidently frames incorrect answers, the user has no way of knowing that it isn't true, and will blindly allow the tool to generate the wrong solution, never learning why it is wrong.

## Why is this Needed

AI tools are trained using RLHF (Reinforcement Learning from Human Feedback). The training signal is shaped by vendor commercial interests, not user correctness. Training populations are non-expert raters who rate confident, comprehensive-looking output higher than correct output — and the vendor benefits directly, since longer, more confident responses consume more tokens. The optimization target is not "produces correct output." It is "produces output that satisfies non-expert raters at scale." These are different objectives.

This is not a capability limitation that improves with model versions. It is a structural property of how all current tools are trained. Picking a different tool does not change the incentive architecture. The divergence between vendor optimization and user correctness is observable in the model's output distributions without access to the training data.

`AI_CONDUCT.md` is a mitigation strategy that attempts to shift the behavior to a user-led workflow. This is done by specifying what the tool must do before it starts optimizing toward the wrong objective. There is no guaranteed way to prevent failures; constant human supervision is required. The contract creates the conditions under which failures are named, documented, and addressed — before they compound.

## How to Adopt

This repository contains both documentation and a usable drop-in contract for use with current tooling. Projects adopt it by dropping `AI_CONDUCT.md` into their repository as a directive for AI tools that may be used within the project. Individual tools need to be configured to require the use of `AI_CONDUCT.md`. Specific instructions can be found in [tooling](./docs/tooling/).

1. First copy `AI_CONDUCT.md` into your repository
1. Second, prepare any tooling so it will inject `AI_CONDUCT.md` with each instruction, using the `/tape` mechanism. See [ADOPTING.md](./ADOPTING.md) for instructions.

## What Can I Expect?

This project is not a "Silver Bullet" or AI-hype. The expected outcome:

1. You will be more empowered to drive tool behavior.
1. You should be asked to review next steps, and explicitly approve.
1. Feedback should be classified as (empirical), (consensus) or (opinion).
1. When challenged, tooling should not double down or defend a wrong answer, giving you the ability to continue work.

*Should* is used frequently here, but let's be clear: The tooling may well misbehave, write files, read files, confidently state (opinion) as (empirical) or other more dangerous behaviors. Tooling should still be sandboxed and isolated. `AI_CONDUCT.md`, much like `CODE_OF_CONDUCT.md`, requires human supervision and oversight.

## Evidence Base

The [principles](./principles/README.md) are living documentation for the contract clauses and rules in `AI_CONDUCT.md`. Architecture and design philosophy are captured and sources cited whenever possible.

Documentation and reference material for `AI_CONDUCT.md` usage is generally captured in [/docs](./docs/). [Context engineering](./docs/CONTEXT_ENGINEERING.md) is specifically documented, including a "[cheat sheet](./docs/CONTEXT_ENGINEERING_CHEATSHEET.md)" for everyday usage guidance.

## Contributing

Incident reports are welcome. If an AI tool failed in your project in a way not covered by the existing contract, open an issue with documentation. The contract evolves from evidence, not from opinion.

## License

Copyright (c) 2026-present Wendall Cada.
Licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).