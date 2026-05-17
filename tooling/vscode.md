# VS Code

## Tool Overview

VS Code hosts GitHub Copilot, Copilot Chat, and other AI extensions. AI
behavior is configured through extension settings and `.github/copilot-instructions.md`.

## Inline Autocomplete Considerations

Inline autocomplete (ghost text) suggestions are triggered automatically and
compete with native tab completion. Key tradeoffs:

- Automatic triggering interferes with object property completion (e.g., typing
  `object.` triggers Copilot rather than showing available properties)
- Manual trigger keybindings often conflict with existing shortcuts
- Suggestion quality is inconsistent and speed depends on API response time
- High-value use case: repetitive block patterns where context is unambiguous

## Configuration for Conduct Compliance

Add `AI_CONDUCT.md` reference to `.github/copilot-instructions.md`:

```markdown
Read AI_CONDUCT.md before any task. The behavioral contract in that file
governs how you operate in this project.
```

Note: Copilot instruction compliance is non-deterministic. See github-copilot.md.

## VsCodeVim

If using VsCodeVim, configure a keybinding for manual Copilot trigger that
does not conflict with vim normal mode or tab completion. Review existing
bindings before assigning. F-key bindings (check existing assignments first)
are less likely to conflict than modifier combinations.

## Evaluation

Placeholder — evaluation to be expanded from documented use.
