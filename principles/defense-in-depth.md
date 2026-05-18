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

## Contract Clause

See Defense in Depth in `AI_CONDUCT.md`.
