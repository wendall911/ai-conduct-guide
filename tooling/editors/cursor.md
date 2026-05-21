# Cursor

## Tool Overview

Cursor is an AI-first code editor built on VS Code. Agent instructions are
configured via `.cursor/rules/*.mdc` files with `alwaysApply: true`. The
legacy `.cursorrules` format is not supported in agent mode.

## Known Failures

- `.cursorrules` is intentionally ignored in agent mode — the default mode —
  with no warning or error. This is a documented architectural decision, not
  a bug. Users following documentation that references `.cursorrules` receive
  zero compliance with no indication.
  Source: https://forum.cursor.com/t/cursorrules-file-silently-ignored-in-agent-mode-with-no-warning/152046

- `.cursor/rules/*.mdc` changes require a session reload — rules edited in
  an active session do not take effect until the chat is closed and reopened.

- SessionStart hook exists but `additionalContext` injection is broken.
  Context is accepted by the hook but never injected into the agent's system
  context. Multiple confirmed reports.
  Source: https://forum.cursor.com/t/sessionstart-hook-additional-context-is-never-injected-into-agents-initial-system-context/158452

- Injects `Co-authored-by: Cursor` or `Made-with: Cursor` trailers into
  commit messages by default. Setting to disable this exists but does not
  always hold after updates.

## Observable Indicators

- Commit messages contain `Co-authored-by: Cursor` or `Made-with: Cursor`
  after disabling the setting — attribution failure. The system-level hook
  is the enforcement backstop.
- Tool proceeds in agent mode as though no instruction rules exist — likely
  using `.cursorrules` rather than `.cursor/rules/*.mdc`. Switch formats and
  reload the session.
- Rules added to `.cursor/rules/` during an active session are not applied —
  expected behavior; close and reopen the chat.
- Agent compliance indicators beyond attribution: needs documented use to
  populate. Document failures as incidents.

## Configuration

Disable attribution in Cursor settings: Settings > Agents > Attribution >
"Add Cursor as co-author" → OFF.

Add system-level hook as backstop — the global `commit-msg` hook covers
Cursor trailers. See `../agents/claude-code.md` for hook setup.

The hook should include:

```bash
sed -i '/^Made-with:/Id' "$1"
```

## Session Start

Use `.cursor/rules/conduct.mdc` with `alwaysApply: true`:

```
---
description: AI conduct contract
alwaysApply: true
---
AI_CONDUCT.md applies this session. Read it before starting work.
```

Note: the SessionStart hook's `additionalContext` field is broken. Rules
files are the only reliable injection mechanism. Close and reopen the chat
after any rules change for them to take effect.

## Recommendation

Better than GitHub Copilot in VS Code — a working instruction format exists
(`.cursor/rules/*.mdc`). Worse than Claude Code — no reliable session-start
enforcement mechanism and the hooks system is partially broken.

Use `.cursor/rules/*.mdc` with `alwaysApply: true` for instruction delivery.
Do not use `.cursorrules` for any governed work.

## Fallback

If rules files are not effective:

- Human approval for every file operation
- Session-start instruction confirmed each session
- Paste the session-start block manually at the start of each conversation:

```
AI_CONDUCT.md applies this session. Read it before we start.
```

Document compliance failures as incidents.
