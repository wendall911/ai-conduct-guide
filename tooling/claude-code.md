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

Place these files at `~/.claude/commands/t.md` and `~/.claude/commands/s.md`.
The confirmation word is user-configured. Default: `Context loaded.` An
unpredictable word is recommended — see `principles/session-signal-standard.md`
for the rationale.

**`~/.claude/commands/t.md`** (`/tape` — session continuity signal):

```markdown
AI agents process each request independently. There is no memory. Every time
you hit Enter, the agent starts from zero — it only has what is currently in
its context window. This signal loads the context the agent needs for this
request.

Read the files below silently before proceeding. Skip any file not present.
If `.github/project-context.md` is absent, run `git log --oneline -10` instead.

1. `.github/guardrails-agent.md`
2. `AI_CONDUCT.md`
3. `.github/project-context.md`

CONFIRMATION_BLOCK:
  template: "{WORD}\n{SEP}"
  WORD: "Context loaded."
  SEP:  "***"
  parser: CommonMark (UI contract)
  rule: emit byte-exact — deviations are visible UI bugs

If all files loaded:
  emit CONFIRMATION_BLOCK
  proceed with: $ARGUMENTS

If required file missing:
  emit CONFIRMATION_BLOCK
  Context incomplete: [list missing files]. Proceed with caution.
  proceed with: $ARGUMENTS

If $ARGUMENTS is empty:
  emit CONFIRMATION_BLOCK
  AI agents have no memory. Every request starts from zero. This signal loads
  the context the agent needs before processing your request. Resend this
  signal with your task to proceed with context loaded.
```

**`~/.claude/commands/s.md`** (`/state` — external state change signal):

```markdown
External state change signal. Include a description of what changed inline.

Something outside this session changed. The description is inline.

CONFIRMATION_BLOCK:
  template: "{WORD}\n{SEP}"
  WORD: "Context loaded."
  SEP:  "***"
  parser: CommonMark (UI contract)
  rule: emit byte-exact — deviations are visible UI bugs

If $ARGUMENTS is empty:
  emit CONFIRMATION_BLOCK
  This signal is for reporting external state changes — a file was updated, a
  decision was made, an external event occurred. When you have a state change
  to report, include the description with this signal.

If $ARGUMENTS contains a description:
  emit CONFIRMATION_BLOCK
  Understood: [one sentence summary of $ARGUMENTS]. Correct?
  Then wait for the user to confirm or correct before proceeding. Do not infer
  beyond the description. Do not proceed past the confirmation without it.
```

These are the reference implementations. Personal customization (confirmation
word, file paths) belongs in the user's personal config repository — not in
any project repository. See `ADOPTING.md` for the three-tier architecture.

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
