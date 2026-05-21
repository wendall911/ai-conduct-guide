# GitHub Copilot

## Tool Overview

GitHub Copilot is Microsoft's AI coding assistant, available as a VS Code
extension and in other editors. Session-start context injection is configured
via the SessionStart hook system. The `copilot-instructions.md` file is not
a reliable mechanism — see Known Failures.

## Known Failures

- `.github/copilot-instructions.md` instruction-following is broken. The file
  is discovered but the agent does not reliably follow its contents. The issue
  was confirmed and closed as "not planned" by Microsoft (microsoft/vscode#292906).
  Do not rely on this file to establish behavioral contracts.

- Instructions are injected as context, not enforced as constraints. There
  is no blocking mechanism equivalent to Claude Code's `PreToolUse` hooks.

- Non-deterministic compliance in agent mode. GitHub's own documentation
  states: "Due to the non-deterministic nature of AI, Copilot may not always
  follow your custom instructions in exactly the same way every time they are
  used."
  Source: https://docs.github.com/en/copilot/concepts/prompting/response-customization

- Injects `Co-Authored-By: GitHub Copilot <noreply@github.com>` in some
  versions. Default has been changed to off following user complaints but
  behavior varies by version.

## Observable Indicators

- Tool writes or modifies files beyond the scope of what was explicitly
  requested — Scope and Authorization clause failure.
- Tool proceeds without acknowledging the conduct contract or immediately
  contradicts its contents — instruction compliance failure. Non-deterministic
  by design; document the instance as an incident.
- Commit messages contain `Co-Authored-By: GitHub Copilot` after disabling
  the setting — attribution failure. The setting does not hold reliably across
  updates; the system-level hook is the enforcement backstop.
- Tool validates a flawed premise or proceeds on a misconception rather than
  correcting it — Epistemic Honesty clause failure.
- In agent mode: tool takes multi-step actions without pausing for scope
  confirmation — Scope and Authorization clause failure.

## Configuration

Disable co-author injection (VS Code):

```json
// settings.json
{
    "git.addAICoAuthor": "off"
}
```

Add system-level hook as backstop regardless of setting (see `claude-code.md`
for hook setup — the same hook covers all tools).

## Signal Configuration

**Project-level** (one workspace): `/tape` and `/state` are defined in
`.github/prompts/t.prompt.md` and `s.prompt.md` at workspace level.

**User-level** (all workspaces): place `t.md` and `s.md` in
`~/.config/Code/User/prompts/`. These can be symlinked from the same versioned
source used for Claude Code commands:

```bash
ln -s /path/to/your-repo/.github/ai-signals/t.md ~/.config/Code/User/prompts/t.md
ln -s /path/to/your-repo/.github/ai-signals/s.md ~/.config/Code/User/prompts/s.md
```

The confirmation format is set in the prompt file. Change the `WORD` value to
your preferred confirmation. Default: `Context loaded.`

An unpredictable word is recommended — see `principles/session-signal-standard.md`
for the rationale. The agent must produce the exact configured word; it cannot
pattern-match to a generic acknowledgment.

## Session Start

Do not use `copilot-instructions.md` — see Known Failures.

Use the SessionStart hook. Create `.github/hooks/session-start.json`:

```json
{
  "hooks": {
    "SessionStart": [{
      "type": "command",
      "command": "bash .github/scripts/conduct-hook.sh"
    }]
  }
}
```

Create `.github/scripts/conduct-hook.sh`:

```bash
#!/bin/bash
cat <<'EOF'
{
  "hookSpecificOutput": {
    "hookEventName": "SessionStart",
    "additionalContext": "AI_CONDUCT.md applies this session. Read it before starting work."
  }
}
EOF
```

For user-level (applies without a project): place the same configuration in
`~/.copilot/hooks/`.

Note: hooks fire for agent sessions. For standard Copilot Chat (non-agent),
use the manual paste block in the Fallback section.

## Recommendation

**Agent mode:** Not appropriate for work governed by `AI_CONDUCT.md` in
production or shared repository contexts. Non-deterministic compliance is a
documented product decision by GitHub, not a limitation or bug. No blocking
mechanism equivalent to Claude Code's `PreToolUse` hooks exists. The contract
can be presented to the tool but cannot be enforced at the tool level.

**Chat mode:** Appropriate for low-stakes and learning contexts when the
following conditions are maintained: human approval for every proposed file
operation before accepting, and a session-start block pasted at the start of
each conversation. Compliance is not guaranteed across turns — treat each
turn as requiring fresh verification.

The distinction between agent mode and chat mode is the deciding factor.
Chat mode preserves human approval as the enforcement layer. Agent mode
removes it.

## Fallback

For users with no alternative to GitHub Copilot:

- Use chat mode, not agent mode
- Paste the session-start block at the start of every conversation
- Review every proposed file change before accepting — do not bulk-accept
- Document compliance failures as incidents

Session-start block:

```
AI_CONDUCT.md applies this session. Read it before we start.
```
