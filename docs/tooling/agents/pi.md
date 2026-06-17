# PI Agent

## Tool Overview

PI Agent is a minimal agent harness that ships with powerful defaults.

Skips features like sub-agents and plan mode. Correctly enforces per-turn contract reads without context compression.

## Setup

Global `~/.pi/agent/prompts/tape.md` or per project .pi/prompts/tape.md:
```
---
description: Behavioral contract and rules.
---

Re-read AI_CONDUCT.md per-turn. All instructions must be fully followed and re-read per-turn for proper operation as a coding agent harness.

Read `./AI_CONDUCT.md` *THEN:* $@
```

Use `/tape` followed by your instruction at each prompt.

## Notes

No destructive reads, writes or advertising injection observed in initial testing.