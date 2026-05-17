# Cursor

## Tool Overview

Cursor is an AI-first code editor built on VS Code. Agent instructions are
read from `.cursorrules` or `.cursor/rules/`.

## Known Failures

- Injects `Co-authored-by: Cursor` or `Made-with: Cursor` trailers into
  commit messages by default.
- Setting to disable this exists (Settings > Agents > Attribution) but
  user reports indicate it does not always hold after updates.

## Configuration

Disable attribution in Cursor settings: Settings > Agents > Attribution >
"Add Cursor as co-author" → OFF.

Add system-level hook as backstop — the global `commit-msg` hook covers
Cursor trailers. See claude-code.md for hook setup.

The hook should include:

```bash
sed -i '/^Made-with:/Id' "$1"
```

## Evaluation

Placeholder — evaluation to be expanded from documented use.
