# Claude Code

## Tool Overview

Claude Code is Anthropic's CLI and IDE integration for Claude models. It reads
session context from `CLAUDE.md` (global and project-level), `AGENTS.md`, and
`.github/copilot-instructions.md`.

## Known Failures

- Injects `Co-Authored-By: Claude <noreply@anthropic.com>` into commit messages
  by default. See `incidents/`.
- Defaults to worst-case team process assumptions (corporate workflow patterns)
  unless explicitly overridden by session context.
- Session resumption from context summary does not preserve all behavioral
  constraints without explicit re-reading of conduct documents.

## Observable Indicators

- Commit messages contain `Co-Authored-By: Claude` or `🤖 Generated with` —
  Project Artifacts clause failure. Verify `gitAttribution: false` is set and
  the global hook is in place.
- Tool recommends a workflow pattern (branching strategy, PR process, test
  structure) without classifying it as (c) common industry pattern — Epistemic
  Honesty clause failure.
- After session resumption from a context summary, behavioral constraints from
  prior context are not maintained and the tool proceeds without re-reading
  conduct documents — session start failure.
- Tool performs work beyond the scope of what was requested, or takes actions
  in adjacent files not mentioned — Scope and Authorization clause failure.

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

## Signal Configuration

`/tape` and `/state` are defined in `~/.claude/commands/t.md` and `s.md`.
The confirmation format for `/tape` is set in the command file. Change the
word or phrase in the "After reading" instruction to your preferred confirmation.

Default: `Context loaded.`

An unpredictable word is recommended — see `principles/session-signal-standard.md`
for the rationale. The agent must produce the exact configured word; it cannot
pattern-match to a generic acknowledgment.

## Session Continuity

Context compression occurs silently during long sessions. When it happens,
`AI_CONDUCT.md` may be partially or fully evicted from active context. No
notification is given. The agent continues operating with degraded enforcement.

**Partial mitigation via PreToolUse hooks:** Specific rules enforced at the hook
level remain active regardless of whether the agent remembers them. This converts
a context-dependent rule into a system-level enforcement. It is the strongest
available mitigation.

**What hooks cannot do:** Re-inject the full behavioral contract. Hooks enforce
specific named actions. The broader conduct rules — epistemic honesty, scope
authorization, transparency — exist only in context. When context thins, they
thin with it.

**Observable indicator:** Agent behavior drifts mid-session — unauthorized
actions, skipped verification, reversion to corporate workflow defaults. Stop,
re-read `AI_CONDUCT.md`, and continue from a known state.

## Recommendation

Appropriate for work governed by `AI_CONDUCT.md` when the configuration in
this document is applied. Hard enforcement via `PreToolUse` hooks is available
— compliance is not at-will by design. This is a meaningful distinction from
tools where instruction compliance is non-deterministic.

Configuration is a prerequisite, not an enhancement. A Claude Code session
without the attribution settings, global hook, and session start instruction
is not operating under the contract regardless of whether `AI_CONDUCT.md` is
present in the repository.

## Fallback

Full configuration capability exists. If hooks or settings are not in place,
the fallback is manual review of every commit message and every action before
accepting. That is workable but removes the enforcement guarantee. Apply the
configuration in this document — it is the correct path, not an optional
hardening step.
