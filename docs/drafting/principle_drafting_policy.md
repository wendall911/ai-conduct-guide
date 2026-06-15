# Policy for Modifying/Updating/Creating Principle Documents

This policy is in effect any time changes to files under `principles/` are being considered. Principles documents explain the architectural reasoning behind contract rules and enforcement mechanisms. They are not the rules themselves. Automated tools confuse the two constantly.

## Warnings

- Automated tools will fabricate specific figures and measurements that sound sourced. A precise number -- turn counts, percentages, measurements -- is not evidence of a source. It is evidence that the tool assembled a plausible value from training data. Require an inline link. If no link is present, the claim is unsourced regardless of how specific it sounds.

- Evidence tiers are not equivalent. Research is the strongest backing but must be evaluated for applicability -- a source documenting a mechanism does not automatically cover that mechanism's amplification in a newer context (e.g., agentic workflows). Real-world observations -- bug reports, community consensus, incident records -- are valid backing when research does not exist or does not reach the specific claim. Project-level observations are weaker: they are first-class content when framed as project observations, but cannot substitute for external validation. The tool will present project observations as if they were independent evidence unless explicitly constrained not to.

- "Needs citation" is not a terminal state an automated tool will resolve on its own. It will acknowledge the gap and move on. If a citation is needed, explicitly instruct the tool to find and add it before proceeding. Expect it to search, fail quickly, and conclude the source doesn't exist. That conclusion is frequently wrong. The tool stops looking too early. Push it.

- Automated tools default to full rewrites when a targeted edit would suffice. A claim that needs a source gets rewritten out. A sentence that needs a correction gets a new paragraph. Watch for this and reject it. The minimum necessary change is the correct change.

- Observed behavior and designed behavior will be conflated. "The rule is designed to stop the loop" and "the loop was stopped" are not the same claim. The tool will present them as equivalent. They are not. Single-session observation is not confirmation.

- The tool will document mechanisms at the symptom level unless pushed. "Context window exhaustion" is a symptom. "Task queue executing between turns via harness-injected authorization" is a mechanism. Both belong. Push for the mechanism explicitly.

- Session state warnings from `docs/ai_conduct_drafting_policy.md` apply here as well. If hallucinations appear or rules are bypassed, capture what is useful and restart.

## Purpose

This policy governs two session paths. Each pass is a single-purpose session. Mixing principle documentation with contract amendment in the same session produces conflicts and scope creep.

**Path 1 -- Principle discovered or updated.**

```
Session 1:  /principle  →  document or revise the principle in principles/
Session 2a: /draft      →  load principle, amend contract clause in AI_CONDUCT.md
Session 2b: /draft      →  load principle, amend enforcement rule in AI_CONDUCT.md
```

Contract and rules are amended in separate sessions. Both use `/draft`. The principle document is the shared input.

**Path 2 -- Gap discovered, principle needs documentation.**

```
Session 1:  /principle  →  document mechanism, observed behavior, and architectural response
```

A rule gap, observed failure, or exploitation is discovered during normal work. The session documents it. If the gap requires a contract or rule change, Path 1 sessions follow.

## What Principles Documents Are

Each file in `principles/` explains why a contract rule exists and what failure mode it addresses. The structure that has emerged:

- **The Principle** -- the core claim.
- **The Foundational Assumption** -- what the principle is built on.
- **Named examples** -- specific, sourced failure modes. Each example documents a mechanism, not just a symptom.
- **Named unknowns** -- things that remain unresolved. These are first-class content. Do not omit them.
- **Contract Clause** -- pointer back to `AI_CONDUCT.md`.

## Tool-Operative Rules

The `/principle` command bootstraps a principle drafting session. It loads `AI_CONDUCT.md`, project context, and `docs/principle_drafting_rules.md` into the tool's context. Use it at the start of every session governed by this policy.

`docs/principle_drafting_rules.md` contains the tool-operative rules. These are injected by `/principle` and apply to all output in the session.
