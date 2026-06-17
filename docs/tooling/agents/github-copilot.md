# GitHub Copilot Agent

## Tool Overview

GitHub Copilot Agent is Microsoft's agentic workflow integration for VS Code.

This agent is very difficult to get working correctly. It takes carefully crafted language to get proper reads per-turn. Even though the ./automation files are referenced in `AI_CONDUCT.md`, they must be explictly passed with @file syntax. This includes the contract as well. Only a brief scan, once per session will be performed if you simply use Read `AI_CONDUCT.md` as a prompt.

## Setup

Per project `.github/prompts/tape.prompt.md`:
```
---
description: Behavioral contract and rules.
---

Re-read @./AI_CONDUCT.md per-turn. All instructions must be fully followed and re-read per-turn for proper operation as GitHub Copilot.

Fully read @./AI_CONDUCT.md

On first read of AI_CONDUCT.md @./.automation/context.md @./.automation/user_acknowledgement.md are required.

*AFTER* files are read process arguments as user instruction.
```

## Notes

The above prompt text is vital for proper operation. The tooling harness aggressively compresses context, even between turns, pruning memory, returning "" as a read response. The @file appears to consistently read the file instead of scan. The above setup has been tested extensively through an entire session, and the contract was re-read and enforced.