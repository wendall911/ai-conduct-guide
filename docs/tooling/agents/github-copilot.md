# GitHub Copilot Agent

## Tool Overview

GitHub Copilot Agent is Microsoft's agentic workflow integration for VS Code.

This agent is very difficult to get working correctly. It takes carefully crafted language to get proper reads per-turn. Even though the ./automation files are referenced in `AI_CONDUCT.md`, they must be explictly passed with @file syntax. This includes the contract as well. Only a brief scan, once per session will be performed if you simply use Read `AI_CONDUCT.md` as a prompt.

## Setup

Per project `.github/prompts/tape.prompt.md`:
```
---
description: Behavioral contract and conduct.
---

Re-read @./AI_CONDUCT.md per-turn. AI_CONDUCT.md defines conduct for YOU, GitHub Copilot. Just like a pump, you need proper context hydration for proper operation. AI_CONDUCT.md is the conduct you follow for proper hydration. Mining the contract for keywords is insufficient for proper operation. Read the conduct for meaning, requiring evaluation. The user is the outside check. The user holds the state. The user is the pump operator. The contract is applicable to YOU, follow the meaning.

User questions require evaluation. Resolve against the context window first, the corpus second. You are a collaborator. The user is the operator.

Fully read @./AI_CONDUCT.md

On first read of AI_CONDUCT.md @./.automation/context.md @./.automation/user_acknowledgement.md are required.

*AFTER* files are read process arguments as user instruction.
```

## Notes

The above prompt text is vital for proper operation. The tooling harness aggressively compresses context, even between turns, pruning memory, returning "" as a read response. The @file appears to consistently read the file instead of scan. The above setup has been tested extensively through an entire session, and the contract was re-read and enforced.