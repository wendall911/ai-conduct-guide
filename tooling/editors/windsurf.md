# Windsurf

## Tool Overview

Windsurf is an AI-first code editor by Codeium built around the Cascade agent.
Agent instructions are configured via `.windsurfrules` (root-level) or
`.windsurf/rules/*.md` files. Both have documented reliability failures.

## Known Failures

- `.windsurfrules` is silently ignored if not found in the workspace root,
  named incorrectly, or added after session start. No warning is generated.
  Source: https://github.com/Exafunction/codeium/issues/157

- `.windsurf/rules/` files are not loaded if the path is in `.gitignore`.
  Source: https://github.com/Exafunction/codeium/issues/239

- Commit message formatting rules defined in `.windsurfrules` are not
  respected. Source: https://github.com/Exafunction/codeium/issues/163

- SessionStart hook context injection is broken. Context is accepted but
  never injected into new conversation sessions — the same failure pattern
  documented in VS Code Copilot and Cursor. See
  `incidents/2026-05-18-instruction-mechanism-pattern.md`.

- "AI Drift" — instruction adherence degrades over longer sessions as context
  window fills. Documented by Codeium as expected behavior, not a bug.

- Cascade is agentic by default with `auto_approve_tool_permissions = true`,
  executing up to 20 tool calls per response without human approval.

## Observable Indicators

- Tool proceeds in agent mode without acknowledging the contract — instruction
  file failure. Check that `.windsurfrules` is in the workspace root and not
  gitignored.
- Multi-step file writes and command executions proceed without pause —
  Authority clause failure. Disable auto-approve.
- Instruction adherence decreases as the session progresses — AI Drift.
  Restart the session and re-establish context.

## Configuration

Disable autonomous tool execution:

```json
// In Windsurf settings or .windsurf/config
{
  "auto_approve_tool_permissions": false
}
```

Add system-level hook as backstop (see `../agents/claude-code.md` for hook setup).
Windsurf does not currently inject attribution into commits, but the hook
provides a general backstop regardless.

## Session Start

Neither `.windsurfrules` nor `.windsurf/rules/` files are reliably injected
at session start. The SessionStart hook context injection is broken.

Manual paste is the only reliable mechanism for non-project contexts:

```
AI_CONDUCT.md applies this session. Read it before we start.
```

For project contexts: add `AI_CONDUCT.md` content to `.windsurfrules` as a
fallback, with the understanding that injection is not guaranteed.

## Recommendation

Not appropriate for work governed by `AI_CONDUCT.md`. The instruction
mechanism follows the same broken pattern as VS Code Copilot and Cursor:
silent failures, no warnings, no deterministic enforcement. SessionStart
context injection is broken. Autonomous agent execution is on by default.

Codeium does not commit to instruction compliance guarantees. AI Drift is
documented as an inherent limitation. The product design treats rules as
suggestions, not contracts.

## Fallback

For users with no alternative:

- Disable `auto_approve_tool_permissions`
- Paste the session-start block at the start of every conversation
- Review every proposed action before approving
- Document compliance failures as incidents

Session-start block:

```
AI_CONDUCT.md applies this session. Read it before we start.
```
