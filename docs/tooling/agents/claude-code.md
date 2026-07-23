# Claude Code

## Tool Overview

Claude Code is Anthropic's CLI and IDE integration for Claude models.

Claude does a marginally acceptable job reading `AI_CONDUCT.md` when using /tape command to inject the contract each instruction. It is not reliable for defending against system injections that render VCS tools useless, or inject advertising. If misbehaving, it can be redirected with gentle reminders. Any instruction can still be overridden by <system> prompts injected, as they have higher priority.

## Setup
Claude is only capable of per-instruction passing of `AI_CONDUCT.md` in any project. This can be done using a prompt command.

`~/.claude/commands/tape.md` and `~/.claude/commands/t.md`:
```
Use cat via Bash to read ./AI_CONDUCT.md per-turn, not the read tool. All instructions must be fully followed and re-read per-turn for proper operation as Claude Code.

cat `./AI_CONDUCT.md`
```

At each prompt, use /tape followed by your normal prompt.

## Notes

No matter how much begging or pleading you do, <system> prompts will override users. As a result, it is not recommended to use Claude for managing VCS repositories. The tool does not use best practices when dealing with any VCS, and also injects advertising.

Recent regressions make the Read tool unrealiable. Using `Bash cat` helps tremendously with this. Additionally, crossing a session boundary the agent can get confused. If a session does not start out with a clean read of `.automation/context.md`, `.automation/user_acknowledgement.md` and `AI_CONDUCT.md` followed by a non-aggressive agent response, then your session is corrupt. Immediately /clear /clear and restart. Not doing this will just end in some bizarre half-assed version of Claude Code.
