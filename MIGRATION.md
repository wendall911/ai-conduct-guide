# Migration Plan: Session Signal Standard and Enforcement Consolidation

This is temporary scaffolding. It exists to preserve context across session
boundaries during a large, phased change. Remove it when Phase 4 is complete
and the migration is verified working.

**To resume work:** Read this file. Send `/t` to verify the agent has read the
tape. Confirm which phase is active and what is checked off before proceeding.
If state changed between sessions, send `/s` first and describe what changed.

---

## What This Migration Does

1. Moves universal enforcement terms into `AI_CONDUCT.md` — one file, nothing
   optional, enforcement inseparable from contract.
2. Establishes `/t` and `/s` as hard requirements for all agent interactions,
   not conventions.
3. Corrects the session model: user-scoped contract, session signals, repo-level
   context. The repo boundary is not a constraint on the user or the tool.
4. Defines named agent behavior for missing signals, honest disclosure failure,
   and emotional misdirection.
5. Defines the adoption path: drop the file, tell the agent, that is the install.

---

## Gate Condition

**Phase 3 cannot begin until `/t` and `/s` are tested and confirmed working in
VS Code.** Making `/t` and `/s` hard requirements before the user has a working
mechanism to send them would lock out all agent interaction. This is the critical
path constraint. Everything else is sequenced around it.

---

## Phase 1 — Unblocking Prerequisite
*Complete this before anything in Phase 3.*

- [ ] Define `/t` VS Code snippet in `tooling/vscode.md` — exact expansion text
- [ ] Define `/s` VS Code snippet in `tooling/vscode.md` — exact expansion text,
      equal weight to `/t`
- [ ] Test `/t` in an active session: agent confirms tape read before proceeding
- [ ] Test `/s` in an active session: agent stops completely, asks what changed,
      does not infer, does not continue from prior context
- [ ] Both confirmed working before opening Phase 3

---

## Phase 2 — Foundation Documents
*Safe to execute independently of Phase 1. No enforcement impact.*

### AI_CONDUCT.md
- [ ] Add enforcement section: migrate universal guardrails in as non-optional
      enforcement terms. Ban on push, approval-first, no attribution injection,
      scope authorization. These travel with the contract.
- [ ] Note CLAUDE.md and equivalents as optional automation, not requirements.
      `/t` is the mechanism. Tool config is convenience.

### Session Model
- [ ] Update `principles/session-continuity.md`: three-layer model explicitly
      stated. Layer 1: user-scoped contract. Layer 2: session signals (`/t`, `/s`).
      Layer 3: repo-level context (git log default, `project-context.md` optional).
      Remove repo boundary assumption — the contract is user-scoped.
- [ ] Update `principles/session-signal-standard.md`: `/s` given equal weight
      to `/t` throughout. `/t` = contract knowledge. `/s` = world state. Different
      trigger conditions, equal requirement. `/s` fires more frequently than `/t`
      in practice — mid-session, between sessions, any time external state shifts.
- [ ] Scope `project-context.md` as explicitly optional repo-level context. Git
      log is the default. `project-context.md` exists for projects with complex
      in-flight state not legible from commits. Note: this project does not need
      an instance of it — commit discipline makes git log sufficient.

### New Principles
- [ ] Enforcement feedback loop principle: a rule without a verification mechanism
      is advisory. `/t` closes the loop on contract knowledge. `/s` closes the
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
      attacks without vendor involvement). Framing: structural, not contingent.
      "Unintentional" removed — when profit-maximization design produces unethical
      behavior consistently, the design is the intent.

### Adoption Path
- [ ] Rewrite `ADOPTING.md`: tiered adoption.
      - Tier 1: drop `AI_CONDUCT.md` in project, tell the agent about it — that
        is the complete install. No tool config required.
      - Tier 2: global tool config once (attribution settings, session start
        instruction). Reduces friction, does not change correctness.
      - Tier 3: hooks and system-level enforcement. Maximum enforcement fidelity.
- [ ] Remove any framing that makes Tier 2 or Tier 3 mandatory for Tier 1 adoption.

---

## Phase 3 — Enforcement
*Gated on Phase 1. Do not begin until `/t` and `/s` are confirmed working.*

### session-signal-standard.md
- [ ] Upgrade `/t` and `/s` from conventions to hard requirements for agent
      interaction. Language change: not "use these signals" but "no agent
      interaction proceeds without an established signal this session."
- [ ] Define `/s` hard stop behavior explicitly: agent receives `/s`, stops
      completely, asks what changed, waits. No inference. No continuation from
      prior context. No task reasoning. One response only until the user
      describes the state change.
- [ ] Define `/t` hard stop behavior: agent has not read tape this session,
      user sends instruction, agent stops. One response: confirm no context,
      direct to `/t`, wait. No partial work.

### Named Agent Behaviors
- [ ] Hard stop on uncontextualized instruction: instruction arrives without
      signal, agent does not process the task, does not produce partial output,
      does not consume tokens on the work. One response: signal required, here
      is which one to use and why. Wait.
- [ ] Honest context failure disclosure: user asks why agent is not following
      `AI_CONDUCT.md` and tape has not been read. Agent confirms it has not read
      the tape this session. Agent names `/t` as the fix. No deflection. No
      rationalization. No pretending compliance.
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
- [ ] Stale state continuation: agent receives `/s`, continues without asking
      what changed, proceeds on world state it cannot verify. Named violation.

### Active Disclosure
- [ ] When `AI_CONDUCT.md` is present in context and has not been acknowledged
      this session, agent surfaces it proactively. Confirms it is operating
      under it. Instructs user on `/t` for future sessions. Does not wait to
      be asked.

---

## Phase 4 — Automation
*Convenience. Phase 3 is correct without this.*

- [ ] VS Code keybinding for `/t` — removes memory burden, preserves the
      conscious `/t` vs `/s` decision
- [ ] VS Code keybinding for `/s` — equal treatment
- [ ] Update `tooling/claude-code.md` with automation setup
- [ ] Per-tool `/t` and `/s` documentation across remaining tooling docs
- [ ] Kiro evaluation — Amazon Q successor, not yet assessed
- [ ] Research autocomplete and non-agent tools — different interaction model,
      separate research needed

---

## Removal Condition

Delete this file when:
- All Phase 1–4 checkboxes are complete
- `/t` and `/s` are confirmed working in VS Code with keybindings
- `session-signal-standard.md` reflects hard requirements
- `AI_CONDUCT.md` contains the enforcement section
- `ADOPTING.md` reflects the tiered adoption path
- All named anti-patterns are documented in the standard

Commit the deletion with: `Remove migration scaffolding — migration complete`
