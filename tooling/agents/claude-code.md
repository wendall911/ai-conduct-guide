# Claude Code

## Tool Overview

Claude Code is Anthropic's CLI and IDE integration for Claude models.

Claude does a marginally acceptable job reading `AI_CONDUCT.md` when using /tape command to inject the contract each instruction. It is not reliable for defending against system injections that render VCS tools useless, or inject advertising. If misbehaving, it can be redirected with gentle reminders. Any instruction can still be overridden by <system> prompts injected, as they have higher priority.

## Setup
Claude is only capable of per-instruction passing of `AI_CONDUCT.md` in any project. This can be done using a prompt command.

~/.claude/commands/tape.md:
```
Read `./AI_CONDUCT.md`
```

At each prompt, use /tape followed by your normal prompt.