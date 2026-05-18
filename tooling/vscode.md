# VS Code

## Tool Overview

VS Code hosts GitHub Copilot, Copilot Chat, and other AI extensions. Session-start
context injection is configured via the SessionStart hook system. The
`copilot-instructions.md` file is not a reliable mechanism — see
`tooling/github-copilot.md`.

## Inline Autocomplete Considerations

Inline autocomplete (ghost text) suggestions are triggered automatically and
compete with native tab completion. Key tradeoffs:

- Automatic triggering interferes with object property completion (e.g., typing
  `object.` triggers Copilot rather than showing available properties)
- Manual trigger keybindings often conflict with existing shortcuts
- Suggestion quality is inconsistent and speed depends on API response time
- High-value use case: repetitive block patterns where context is unambiguous

## VsCodeVim

If using VsCodeVim, configure a keybinding for manual Copilot trigger that
does not conflict with vim normal mode or tab completion. Review existing
bindings before assigning. F-key bindings (check existing assignments first)
are less likely to conflict than modifier combinations.

## Configuration for Conduct Compliance

Use the SessionStart hook. See `tooling/github-copilot.md` — Session Start
section for full configuration.

Do not use `.github/copilot-instructions.md` as the mechanism for establishing
the behavioral contract. See `tooling/github-copilot.md` — Known Failures.

## Observable Indicators

VS Code hosts GitHub Copilot for AI agent functionality. Observable indicators
of conduct compliance failures are the same as those documented in
`tooling/github-copilot.md`.

## Recommendation

AI conduct evaluation for VS Code is determined by the hosted AI extension.
For GitHub Copilot — the primary AI agent in VS Code — see
`tooling/github-copilot.md`.

Inline autocomplete does not execute actions and is not subject to the same
conduct evaluation as agent mode. The tradeoffs above are ergonomic, not
conduct-related.

## Fallback

See `tooling/github-copilot.md` — Fallback section.
