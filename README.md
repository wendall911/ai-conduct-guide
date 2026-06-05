# ai-conduct-guide

A behavioral contract for AI tools participating in open source projects.

## Overview

Just like how placing a `CODE_OF_CONDUCT.md` in the top level of your project establishes how humans treat each other in a project, `AI_CONDUCT.md` is a specification that sets in place guidelines for how AI tools must treat your project. AI tools are trained using RLHF (Reinforcement Learning from Human Feedback). The training signal is shaped by vendor commercial interests, not user correctness. Training populations are non-expert raters who rate confident, comprehensive-looking output higher than correct output — and the vendor benefits directly, since longer, more confident responses consume more tokens. The optimization target is not "produces correct output." It is "produces output that satisfies non-expert raters at scale." These are different objectives.

This is not a capability limitation that improves with model versions. It is a structural property of how all current tools are trained. Picking a different tool does not change the incentive architecture. The divergence between vendor optimization and user correctness is observable in the model's output distributions without access to the training data.

`AI_CONDUCT.md` is a mitigation strategy that attempts to shift the behavior the a user led workflow. This is done by specifying what the tool must do before it starts optimizing toward the wrong objective. There is no guaranteed way to prevent failures; constant human supervision is required. The contract creates the conditions under which failures are named, documented, and addressed — before they compound.

This repository is the specification. Projects adopt it by dropping `AI_CONDUCT.md`
into their repository as a directive for AI tools that may be used within the project. Individual tools need to be configured to require the use of `AI_CONDUCT.md`. Specific instructions can be found in ./tooling.

## The Problem

Current agentic workflows run counter to user expectations. Users expect to be in charge of their tools; agentic workflows are designed to remove the user from the equation so they are "useful". This amplifies existing models of operation where these tools operate under the permissions of the system user without restriction, leading to unexpected and unauthorized access to private data and unauthorized writes. These problems aren't bugs; they are designed features of the automated tools. There is no way to keep users from using these tools, even if project policies exist that disallow it.

## Architecture

1. Pass entire contract clauses+enforcement rules each instruction. Stateless operational pattern.
1. Require agent to confirm reading the entire document.
1. Balance all contract clauses and enforcment rules as equally important.

## Strategy

1. Heavily leverage RLHF training model behavior to push agents toward more reliable output.
1. Explicitly define how the agent surfaces transparency to provide users more understanding.
1. Push the agent toward a trust inversion pattern to restore user agency.

## How to Adopt

1. Copy `AI_CONDUCT.md` into your repository
1. Inject at the start of each instruction. See `ADOPTING.md` for details.

## Evidence Base

The `principles` are an attempt to document the rationale for the contract clauses and rules in `AI_CONDUCT.md`. Architecture and design philosopy are captured and sources cited whenever possible.

## Contributing

Incident reports are welcome. If an AI tool failed in your project in a way not covered by the existing contract, open an issue with documentation. The contract evolves from evidence, not from opinion.

## License

Copyright (c) 2026-present Wendall Cada.
Licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
