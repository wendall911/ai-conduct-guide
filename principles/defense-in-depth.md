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

## Contract Clause

See Defense in Depth in `AI_CONDUCT.md`.
