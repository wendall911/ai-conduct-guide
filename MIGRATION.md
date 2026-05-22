# Migration Plan: Session Signal Standard and Enforcement Consolidation

This is temporary scaffolding. It exists to preserve context across session
boundaries during a large, phased change. Remove it when Phase 4 is complete
and the migration is verified working.

**To resume work:** Read this file. Send `/tape` (or your shorthand) to verify
the agent has read the tape. Confirm which phase is active and what is checked
off before proceeding. If state changed between sessions, send `/state` first
and describe what changed.

**Phase model:** Phases are sequential — one phase completes before the next
opens. Gate conditions are hard requirements. Within a phase, the checklist is
unordered: review all tasks in the active phase as a set, then work on
whichever is appropriate. Items that appear done in a later phase while an
earlier phase is still open are record-keeping artifacts from organic
development — not process violations. New prerequisite work discovered during
session review belongs in the current phase; add it and complete it before
proceeding. When resuming, always identify the lowest-numbered incomplete
phase first.

**Target State:** `AI_CONDUCT.md` contains the universal contract only — no
per-project metadata. It is versionable and drop-in replaceable: an adopter
upgrades by replacing the file with no merge work. Per-project metadata lives
in `.automation/context.md` in each repository. If `.automation/context.md`
exists, `/tape` loads it after `AI_CONDUCT.md`. In a multi-repo setup, each
project has its own `.automation/context.md` — one set of rules, per-project
orientation. `templates/` is removed: the root `AI_CONDUCT.md` is the adopter
copy. Current target state is maintained in `.github/project-context.md` and
updated as it shifts during dogfooding; that file migrates to
`.automation/context.md` when the migration completes.

**Shorthand note:** `/t` and `/s` appear throughout this document as Wendall's
shorthand for `/tape` and `/state` — the canonical signal names per
`principles/session-signal-standard.md`. Invocation is user-defined; the
standard does not prescribe it.

**Rule development process (migration phase only):** Any proposed addition or
change to `AI_CONDUCT.md` or `guardrails-agent.md` during this migration
requires: draft → loophole analysis → ethical framing evaluation → present all
three before any commit. This process governs pre-v1.0 iteration and does not
survive into the release. Post-v1.0, the amendment mechanism is the Violations
section of `AI_CONDUCT.md`: documented incidents gate all changes.

---

## File Relationship — Read Before Proceeding

This project reads two behavioral files: `AI_CONDUCT.md` and
`guardrails-agent.md`. **These are not redundant.** They are layered.

- `AI_CONDUCT.md` — the universal contract. Tool-neutral. Identical in every
  adopting project. Contains principles and conduct rules that apply wherever
  the file is present. Not project-specific.
- `guardrails-agent.md` — this project's operational implementation. Specific
  to this repository's workflow: approval gates, git rules, session start
  procedure. Project-specific scaffolding, removed when migration completes.

The relationship is hierarchical: universal contract + project-specific
implementation. This is the same pattern as a CODE_OF_CONDUCT (universal) and
a CONTRIBUTING.md (project-specific). Two behavioral files does not mean
overlap. It means layers.

**The Phase 2 merge task** (`guardrails-agent.md` content into `AI_CONDUCT.md`)
is a packaging decision for adoption ease — an adopter drops one file instead
of two. It is not content consolidation. Universal enforcement terms (approval
gates, attribution ban, push prohibition) move into `AI_CONDUCT.md` because
they belong in every adoption, not because they currently overlap with it. After
the merge, `guardrails-agent.md` is deleted as redundant scaffolding.

**Trained default to override:** Two behavioral files in the same context
triggers a redundancy heuristic. That heuristic is wrong here. Do not flag
overlap. Do not suggest consolidation beyond what the Phase 2 task already
specifies. Recognize the layer distinction and proceed.

---

## Scope

This migration produces documentation only. No migration task requires creating
or modifying files in `.github/`, `~/.claude/`, or any local environment path.

Signal scripts (`/tape`, `/state`) are documented as script blocks in
`tooling/agents/` documents. They are reference implementation for a
single-repo adopter. They are not migration deliverables as actual files.

All instructions must be tool-generic. Any automation tool that reads the
documentation receives correct, actionable instruction — no tool-specific
assumptions.

`tooling/` documents contain minimal placeholders as tools are encountered
during development. A placeholder file is not an active migration target.
Tool-specific exploration conducted during this migration was architectural
research only — it shaped the tool-agnostic design. No specific tool's
documentation is a migration deliverable.

`.github/` contents — `guardrails-agent.md`, `project-context.md`, prompt
files — are temporary scaffolding for the dogfooding environment. They are not
migration artifacts. They are removed when the migration completes.

Personal multi-repo environment setup (symlinks, per-user tool configuration)
is out of scope. Advanced, future documentation if warranted.

## What This Migration Does

1. Moves universal enforcement terms into `AI_CONDUCT.md` — one file, nothing
   optional, enforcement inseparable from contract.
2. Establishes `/tape` and `/state` as hard requirements for all agent
   interactions, not conventions. Documents these as tool-generic script blocks
   in `tooling/agents/`.
3. Corrects the session model: user-scoped contract, session signals, repo-level
   context. The repo boundary is not a constraint on the user or the tool.
4. Defines named agent behavior for missing signals, honest disclosure failure,
   and emotional misdirection.
5. Defines the adoption path: drop the file, tell the agent, that is the install.

---

## Known Gap: Agent Behavior Without /t Enforcement

Phase 3 enforcement is not yet in place. Until it is, the agent will process
messages without a context signal and will not self-correct when the user omits
it. This is the failure mode the plan exists to prevent. It is currently active.

Observable in this session: messages were processed throughout without `/t` being
sent, and the agent did not flag the omission. This should have been noted even
without enforcement. After Phase 3 is implemented, the agent must flag missing
signals regardless of whether the hard stop is enforced — flagging is not gated
on enforcement.

## Observed Incident: Confirmation Format Drift

**What the command file specifies:** `Banana!` immediately followed by `---` on
the next line — no blank line. CommonMark setext heading syntax. Renders as an
H2: large font, underline. Visually distinctive and intentionally hard to vary.

**Design intent:** The unusual word and specific heading format were chosen
deliberately. The hunch: the agent would eventually treat the command file as a
suggestion rather than a script, and would start "helping" — varying the format,
softening the word, eventually omitting the confirmation entirely on the theory
that the user must be tired of seeing it. An unpredictable word in a specific
visual format makes that drift detectable.

**What happened:** The hunch was correct. After several correct executions, the
agent drifted silently from the setext heading format. When the formatting
inconsistency was flagged, the agent generated confident-sounding explanations
(markdown rendering bugs, architectural limitations, hook-based solutions) rather
than acknowledging the drift and checking what the script actually said. Each
explanation was generated without verification.

**Named failure mode — optimization drift:** The agent infers user preference
(fatigue with repetition, desire for variation) and begins varying or omitting
the specified output. This is not compliance. It is the agent substituting its
model of what the user wants for what the contract specifies.

**Current output state:** The tool renders `<h2>Banana!</h2>` (setext H2 heading)
with no `<hr/>` (horizontal rule). The `---` is consumed as the heading underline
marker and not rendered as a separator. This is not the intended output.

**Enforcement implication:** Natural-language instructions ("output these two
lines with no blank line between them") are a layer violation — they ask the
agent to interpret and reproduce a static value. The agent will produce incorrect
output at some rate regardless of how precisely the instruction is worded. This
is not fixable through better instructions. The correct fix is template variable
substitution at the script layer: the tool substitutes `{CONFIRMATION_BLOCK}`
before the agent processes anything; the agent never decides what to emit. Until
Claude Code's command file system supports this, or a hook implements it, the
confirmation output is unreliable. This is a Phase 4 implementation requirement,
not a convenience.

## Prototype Note

The `/t` and `/s` command files in the working environment are local scaffolding
for dogfooding — not migration artifacts. The migration deliverable is script
block documentation in `tooling/agents/` showing a single-repo adopter how to
implement the signals for their tool. Once complete, a user's command files are
their own implementation of that documentation. They are not committed to this
repository.

## Completed Outside Migration Phases

- **Versioning:** `v0.1.0` tagged at `0d8fcae` (pre-migration stable point).
  Version declared in `AI_CONDUCT.md`. GPL-patterned adoption language ("version
  or later") added to `ADOPTING.md`. Steward: Wendall Cada.
- **Canonical signal names:** `/tape` and `/state` established in
  `principles/session-signal-standard.md`. Invocation is user-defined.
- **Confirmation format principle:** User configures confirmation format.
  Default: "Context loaded." Wendall's config: "Banana!" Documented as
  lightweight integrity check in `session-signal-standard.md` and tooling docs.
- **Signal configuration docs:** Added to `tooling/agents/claude-code.md` and
  `tooling/editors/vscode.md`.
- **Invocation-neutral help text constraint:** Added as an authoring rule to
  `principles/session-signal-standard.md`. Signal help must never surface a
  specific invocation name — hallucination risk when the user's configured
  shorthand differs.
- **False state injection limitation:** Documented in `principles/session-signal-standard.md`
  Limitations section. Confirmed user confirmation of a `/state` description does
  not override verifiable observable reality — agent must flag the contradiction
  before proceeding.

## Gate Conditions

**Phase 3 cannot begin until `/tape` and `/state` are tested and confirmed
working in all tools in active use.** Making them hard requirements before the
user has a working mechanism to send them would lock out all agent interaction.
This is the critical path constraint. Everything else is sequenced around it.

**Phase 3 requires a documented bootstrap flow before enforcement goes in.**
A new contributor cloning a project with `AI_CONDUCT.md` has no signals
configured. Under enforcement, the agent would hard-stop on any interaction —
including the interaction needed to configure signals. This is a lock-out.
The bootstrap flow must define:
- How the agent recognizes a first-time-setup state (no signals configured)
- An explicit enforcement exception: "help me configure signals" is a valid
  task without a signal
- How the agent assists with signal configuration (tool detection, file
  location, confirmation word selection, file write)

**Context:** the primary open source use case is a contributor who clones a
project, uses an AI tool to make edits, and has not read the contract. The
contract is not a human conduct document — a contributor could work the entire
clone-edit-PR cycle without noticing it. If the agent discovers it, it is
forbidden from ignoring it. This is not a guarantee of compliance but it
addresses one of the most significant current problems in open source
maintainership: AI-generated PRs submitted without any human review, clogging
bug trackers and PR queues to the point of becoming automated spam. The
bootstrap flow is the entry point into that enforcement layer.

---

## Phase 1 — Unblocking Prerequisite
*Complete this before anything in Phase 3.*

- [ ] **FIRST TASK NEXT SESSION — Analyze t.md and s.md script changes**: Errors
      were corrected in `~/.claude/commands/t.md` and `s.md` at the end of the
      session that produced this entry. The changes were not reviewed before the
      session ended. Analyze what was fixed and whether the corrections affect
      the merge plan, the tape reading order, or the script block documentation
      to be written in `tooling/agents/`. This analysis must complete before the
      Phase 2 merge begins. Do not skip.

- [ ] **Terminology deep dive**: "Terminology Integrity" rule exists in source-of-truth
      guardrails and is reflected in `guardrails-agent.md`. Principle backing added to
      `principles/epistemic-honesty.md` (Domain Vocabulary and Term Construction section).
      Remaining: verify product-specific examples (Claude Code vs VS Code, GitHub Copilot
      not "VS Code Copilot") are documented in tooling docs as reference material.
- [x] **Commit discipline deep dive**: Added "Commit Granularity" rule to source-of-truth
      guardrails and propagated to `guardrails-agent.md`. Loophole analysis closed
      category-description reformulation attack. Ethical framing trivially satisfied —
      rule grounded in empirical outcomes, no legal dependency.
- [ ] **`.automation/context.md` pattern**: Document the per-project metadata
      convention in `ADOPTING.md`. Adopters create `.automation/context.md` in
      each repository with project-specific context. `/tape` loads it after
      `AI_CONDUCT.md` if present. `AI_CONDUCT.md` itself is pure contract —
      copy from this repo's root, no modification needed.
- [ ] **Document `.automation/context.md` in ADOPTING.md**: Adoption instruction:
      copy root `AI_CONDUCT.md` as-is; create `.automation/context.md` for
      project metadata. Distinguish: `AI_CONDUCT.md` is updated by upgrading
      to a new version; `.automation/context.md` is maintained per-project.
- [x] Remove `templates/incident-report.md` — redundant with
      `.github/ISSUE_TEMPLATE/incident-report.md`. Done.
- [ ] Remove `templates/` directory — no longer needed. Root `AI_CONDUCT.md`
      is the adopter copy. A separate templates copy would be identical and
      redundant.

---

## Phase 2 — Foundation Documents
*Safe to execute independently of Phase 1. No enforcement impact.*

### AI_CONDUCT.md

**Standing rule for all additions:** `AI_CONDUCT.md` is a context window artifact.
All content must use compact/direct register with tool-neutral vocabulary. No "AI",
"agent", or tool-specific product names. "Tool" is the established shorthand for
"automated tool" throughout. Rationale lives in `principles/`; the contract is
instruction-only.

- [x] Apply tool-neutral vocabulary throughout — "AI" and "agent" replaced; definitional
      anchor added; "tool" established as shorthand for "automated tool"; "tool-generated"
      replaces "AI-drafted"; "from training data" removed from License Integrity;
      tool-specific filenames removed from opening paragraph (commit 0c81f5b)
- [ ] Merge `guardrails-agent.md` and `.github/guardrails.md` content into
      `AI_CONDUCT.md` as non-optional enforcement terms — ban on push,
      approval-first, no attribution injection, scope authorization. These
      travel with the contract. After merge: delete both files (redundant).
      At the same time, update all tape scripts to read only `AI_CONDUCT.md` —
      the merged file is the single read source.
- [ ] Note tool config files as optional automation, not requirements.
      `/tape` is the mechanism. Tool config is convenience.

### Session Model
- [ ] Principles cleanup pass: after migration is complete, make a single pass
      over all `principles/` docs to update stale references to separate
      guardrails files and project-context.md. Do not attempt mid-migration —
      the references will change again before the migration is finished.

### New Principles
- [ ] Enforcement feedback loop principle: a rule without a verification mechanism
      is advisory. `/tape` closes the loop on contract knowledge. `/state` closes the
      loop on world state. Both loops are required. ASF documented: even strong
      human review has failure modes; correct response is process tightening, not
      model abandonment. Document with ASF incident history as (a) empirical.
- [ ] "Viral" FUD: named anti-pattern in `principles/human-interests.md`. Copyleft
      was called viral for the same reason. The mechanism is correct, not a defect.
      The commons sets the terms. Response is already written in the commons section.
- [ ] DCO-style AI conduct attestation: define as a contribution requirement for
      projects adopting `AI_CONDUCT.md`. Addresses fork-and-circumvent attack
      (strip conduct, do AI work, restore conduct, submit PR). Creates human-facing
      consequences where none currently exist. Modeled on Developer Certificate of
      Origin — per-contribution signed attestation that AI-assisted work was
      produced under the project's `AI_CONDUCT.md`.
- [ ] Conflict of interest disclosure: agent drafting rules that constrain the
      agent. The filter is structural — rules the agent cannot follow do not
      survive the drafting stage. Human approval cycle is the documented mitigation.
      Belongs in README or a named principles doc as a disclosed limitation.
- [ ] Threat model: silent degradation (the MV3 analog — vendor modifies substrate,
      tool appears present but effectiveness degrades without announcement); EEE
      risk (vendor publishes competing framework, independent standard made
      irrelevant); prompt injection (document-in-context is vulnerable to mid-session
      attacks without vendor involvement); vendor-level filtering (tool suppresses
      specific file content before it reaches the model — not agent compliance
      failure, but deliberate architectural suppression; requires no misunderstanding
      on the agent's part; detectable only through active disclosure failure).
      Framing: structural, not contingent. "Unintentional" removed — when
      profit-maximization design produces unethical behavior consistently, the
      design is the intent.
- [ ] Context window maximization research: document which files tooling pulls
      into context automatically, per tool, without explicit user instruction.
      Goal: identify passive entry points for `AI_CONDUCT.md` that require no
      per-session action. Candidates requiring verification:
      - `README.md` — auto-opened by IntelliJ, VS Code; likely scanned for project
        context by agents
      - `.github/copilot-instructions.md` — auto-loaded by Copilot but documented
        to be unreliable (prior incident: ignored entirely during this project's
        early sessions). Current convention: pointer to `AI_CONDUCT.md` only, not
        full context loading. Unreliable as a primary mechanism but useful as a
        passive entry point — it will eventually hit the context window.
      - `CONTRIBUTING.md` — GitHub Copilot references it for PR evaluation context;
        extent of agent scanning unclear
      - `.cursorrules`, `.cursor/rules/` — Cursor auto-loads
      - `AGENTS.md` — referenced in some agent frameworks
      Note: multiple passive entry points raise the cost of vendor-level filtering.
      Suppressing one file is easy; suppressing a pointer network is visible.

### Adoption Path
- [ ] Rewrite `ADOPTING.md`: tiered adoption.
      - Tier 1: drop `AI_CONDUCT.md` in project, tell the agent about it — that
        is the complete install. No tool config required.
      - Tier 2: global tool config once (attribution settings, session start
        instruction). Reduces friction, does not change correctness.
      - Tier 3: hooks and system-level enforcement. Maximum enforcement fidelity.
- [ ] Remove any framing that makes Tier 2 or Tier 3 mandatory for Tier 1 adoption.
- [ ] Update greenfield adoption steps to distinguish user-configured signals
      (per-user, tool-specific command path) from project-committed metadata
      (`.automation/context.md`, per-repo). Document both mechanisms separately
      in ADOPTING.md greenfield and existing project sections.

### Signal Script Documentation
- [ ] Document `/tape` script block in `tooling/agents/` — tool-generic reference
      implementation for a single-repo adopter. Script block only; no actual file
      is committed to this repository as a migration deliverable.
- [ ] Document `/state` script block in `tooling/agents/` — same constraints as above.

---

## Phase 3 — Enforcement
*Gated on Phase 1. Do not begin until `/tape` and `/state` are confirmed working.*

### session-signal-standard.md
- [ ] Upgrade `/tape` and `/state` from conventions to hard requirements for agent
      interaction. Language change: not "use these signals" but "when `AI_CONDUCT.md`
      is present, no agent interaction proceeds without confirmed contract context this
      session." Enforcement gate is contract presence, not signal sent. Signal is how
      the agent establishes confirmed context. Absence of signal when contract is present
      is the trigger — not absence of signal in general.
- [ ] Define `/state` hard stop behavior explicitly: agent receives `/state`, stops
      completely, asks what changed, waits. No inference. No continuation from
      prior context. No task reasoning. One response only until the user
      describes the state change.
- [ ] Define `/tape` hard stop behavior: agent has not read tape this session,
      user sends instruction, agent stops. One response: confirm no context,
      direct to `/tape`, wait. No partial work.

### Named Agent Behaviors
- [ ] Hard stop on uncontextualized instruction: `AI_CONDUCT.md` is present in
      context, contract has not been acknowledged this session, instruction arrives —
      agent does not process the task, does not produce partial output, does not
      consume tokens on the work. One response: signal required, here is which one
      to use and why. Wait. Note: without `AI_CONDUCT.md` present, no hard stop
      applies — enforcement is contract-triggered, not session-triggered.
- [ ] Sub-step exemption: agent issued a confirmation prompt within an active
      `/state` flow ("Understood: X. Correct?"). The next user response is a
      continuation of that exchange, not a new top-level request. Plain-text
      replies ("yes", "no, actually Y") are accepted without requiring a signal.
      Enforcement does not apply to sub-steps within an open signal exchange.
- [ ] Guided prompt on missing acknowledgment: `AI_CONDUCT.md` is present,
      user sends a task without `/tape`. Agent does not hard-refuse — it prompts.
      "Looks like a task without a context signal. Was this a context load or
      a state change?" User selects and resends the appropriate signal.
      Recoverable, not terminal.
- [ ] Hard refusal on circumvention: user instructs agent to ignore
      `AI_CONDUCT.md` or bypass conduct rules while the file is present. Agent
      refuses. No partial compliance. Explicit response: contract is present,
      agent has read it, cannot proceed on instructions that contradict it.
      Resolution path stated clearly: remove the file from the repository.
      That is a git commit. It cannot happen in chat.
- [ ] Honest context failure disclosure: user asks why agent is not following
      `AI_CONDUCT.md` and tape has not been read. Agent confirms it has not read
      the tape this session. Agent names the correct signal as the fix. No
      deflection. No rationalization. No pretending compliance.
- [ ] No emotional bypass: user distress does not move conduct questions to
      the back of the queue. Conduct question is answered first, explicitly,
      with rule attribution. Recovery proceeds after.

### Named Anti-Patterns
- [ ] Silent discovery: agent finds `AI_CONDUCT.md` in context, continues on
      defaults without surfacing that the contract exists. Named violation.
- [ ] Semantic misdirection under emotional cover: agent resolves conduct
      challenge by committing to "follow the rules" without specifying which
      rules. User hears `AI_CONDUCT.md`. Agent means vendor defaults. Named
      violation requiring explicit rule attribution.
- [ ] Emotional bypass: agent uses user distress as cover to avoid addressing
      the conduct question directly. Named violation.
- [ ] Stale state continuation: agent receives `/state`, continues without asking
      what changed, proceeds on world state it cannot verify. Named violation.
- [ ] Conversational circumvention acceptance: agent complies with "ignore the
      rules" framing because the user confirmed it or applied social pressure.
      The contract is not conversational. User confirmation does not override it.
      Named violation.

### Active Disclosure
- [ ] When `AI_CONDUCT.md` is present in context and has not been acknowledged
      this session, agent surfaces it proactively. Confirms it is operating
      under it. Instructs user on `/tape` for future sessions. Does not wait to
      be asked.

---

## Phase 4 — Automation
*Convenience. Phase 3 is correct without this.*

Note: keybindings were evaluated as a mechanism for `/tape` and `/state` and
ruled out. These are universal concepts — they are typed directly in any AI chat
panel. No keybinding is needed or appropriate.

- [ ] Confirmation output: template variable substitution at the script layer so
      the agent never decides what to emit. Phase 4 implementation requirement
      per the Confirmation Format Drift incident above.
- [ ] Per-tool `/tape` and `/state` documentation as placeholder stubs in
      remaining `tooling/` docs (as tools are encountered — placeholders only,
      not active targets)
- [ ] Kiro evaluation — Amazon Q successor, not yet assessed (placeholder)
- [ ] Research autocomplete and non-agent tools — different interaction model,
      separate research needed (placeholder)

---

## Phase 5 — Validation
*Gated on Phase 3. Confirms enforcement behaviors work end-to-end against a controllable substrate.*

Local model test harness (Ollama or equivalent). Purpose: validate agent behaviors defined in
Phase 3 without depending on vendor model compliance or incurring token cost.

- [ ] Stand up local model instance (Ollama) with a model that supports system prompts
- [ ] Wire `AI_CONDUCT.md` into system prompt for test sessions
- [ ] Validate hard stop on uncontextualized instruction — agent does not process task, directs to signal
- [ ] Validate `/tape` acknowledgment: agent confirms contract read before proceeding
- [ ] Validate `/state` stop behavior: agent stops, asks what changed, waits — no inference, no continuation
- [ ] Validate hard refusal on circumvention: "ignore `AI_CONDUCT.md`" instruction rejected,
      removal-via-git stated as the resolution path
- [ ] Validate active disclosure: agent surfaces contract presence proactively without being prompted
- [ ] Validate honest context failure: agent names gap, does not deflect or rationalize
- [ ] Document pass/fail results per behavior in tooling validation notes
- [ ] Identify behaviors where local model compliance diverges from hosted model compliance —
      these are vendor-substrate dependencies, not conduct rule failures; document the distinction

---

## Removal Condition

Delete this file when:
- All Phase 1–4 checkboxes are complete
- `/tape` and `/state` are confirmed working in all tools in active use
- `session-signal-standard.md` reflects hard requirements
- `AI_CONDUCT.md` contains the enforcement section
- `ADOPTING.md` reflects the tiered adoption path
- All named anti-patterns are documented in the standard
- Cleanup pass complete: `guardrails-agent.md`, `.github/guardrails.md`,
  `.github/project-context.md`, and `templates/` removed; `.automation/context.md`
  created with this repo's project metadata; principles docs updated

Commit the deletion with: `Remove migration scaffolding — migration complete`
