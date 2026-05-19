# Defense in Depth

## The Principle

Rules governing agent behavior are necessary but not sufficient. A rule in a
conduct document does not prevent a tool from injecting behavior the rule
prohibits — tools can be updated, reconfigured, or replaced, and new tools
arrive with their own defaults.

Reliable enforcement requires layers operating independently:

1. **Tool-level configuration** — disable the behavior at the source
2. **System-level enforcement** — git hooks, shell wrappers, pre-commit checks
3. **Documented policy** — this contract, for agent intent and session framing

If only one layer is in place, the system is fragile. The failure of any single
layer should not compromise the whole.

## The Attribution Example

Claude Code injects `Co-Authored-By` trailers into commit messages by default.
A rule in a conduct document saying "do not inject attribution" is layer 3 only.
The complete remediation:

- Layer 1: `gitAttribution: false` in `~/.claude/settings.json`
- Layer 2: `~/.git-hooks/commit-msg` stripping known trailers globally
- Layer 3: The rule in this contract

All three layers are required. Layer 3 alone fails when the tool is updated
or replaced.

## The Instruction File Example

The documented adoption path for most AI tools is: add an instruction file
to the project (`.github/copilot-instructions.md`, `.cursorrules`, or
equivalent) and the tool reads it at session start.

Research across Microsoft-hosted tools and Cursor found this mechanism is
broken or absent in every case except Claude Code:

- GitHub Copilot / VS Code: instruction-following broken, closed as "not planned"
- GitHub Copilot / IntelliJ: instruction file support not yet implemented
- Cursor: `.cursorrules` intentionally ignored in agent mode without warning;
  SessionStart hook context injection broken

Projects relying on the instruction file as their sole enforcement layer
have no protection. The contract is present in the repository but not in
the agent's context. This is the failure mode the Defense in Depth clause
exists to prevent.

See `incidents/2026-05-18-instruction-mechanism-pattern.md` for the full
documented record.

## The Session Continuity Example

`AI_CONDUCT.md` is read at session start and held in context. As a session grows,
context compression occurs silently — no tool notifies the user. The contract may
be partially or fully evicted from active context. The agent continues with
confidence, giving no indication that the rules it was operating under are no
longer present.

This is a third single-layer enforcement failure:

- A contract read once at session start and not re-anchored is layer 3 only
- No tool-level mechanism exists to detect or notify context loss
- System-level mitigation is partial: Claude Code's `PreToolUse` hooks enforce
  specific rules independently of context, but cannot re-inject the full contract

The user cannot monitor this gap because context loss is invisible. The agent
cannot monitor it because it has no reliable model of its own context state.
Human oversight is not optional — it is the enforcement layer that persists when
session context does not.

See `incidents/2026-05-18-session-continuity-failure.md` for the documented record.

## Contract Clause

See Defense in Depth in `AI_CONDUCT.md`.
