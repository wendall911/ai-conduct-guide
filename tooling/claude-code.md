# Claude Code

## Tool Overview

Claude Code is Anthropic's CLI and IDE integration for Claude models. It reads
session context from `CLAUDE.md` (global and project-level), `AGENTS.md`, and
`.github/copilot-instructions.md`.

## Known Failures

- Injects `Co-Authored-By: Claude <noreply@anthropic.com>` into commit messages
  by default. See INCIDENTS.md.
- Defaults to worst-case team process assumptions (corporate workflow patterns)
  unless explicitly overridden by session context.
- Session resumption from context summary does not preserve all behavioral
  constraints without explicit re-reading of conduct documents.

## Configuration

Disable attribution injection:

```json
// ~/.claude/settings.json
{
    "gitAttribution": false
}
```

System-level backstop (strips trailers regardless of tool settings):

```bash
# ~/.git-hooks/commit-msg
sed -i '/^Co-Authored-By:/Id' "$1"
sed -i '/^Co-authored-by:/Id' "$1"
sed -i '/^Generated with/Id' "$1"
sed -i '/^🤖 Generated with/Id' "$1"
```

Register globally:

```bash
git config --global core.hooksPath ~/.git-hooks
chmod +x ~/.git-hooks/commit-msg
```

## Session Start

Add to global `~/.claude/CLAUDE.md`:

```markdown
At session start, read AI_CONDUCT.md before any other work.
```

## Evaluation

Instruction compliance is enforced via `PreToolUse` hooks — hard blocking is
technically possible and implemented. Compliance is not at-will by design.
This is a meaningful distinction from GitHub Copilot.
