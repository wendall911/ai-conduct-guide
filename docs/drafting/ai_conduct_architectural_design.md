# AI_CONDUCT.md Architectural Design

Operational context for `AI_CONDUCT.md`. Records components whose purpose is not derivable from their text. Required before any drafting session targeting a structural component.

---

## Structural Components

### Preamble

The block of text before the first named section. Contains the contract scope declaration, the binding statement, the two-part document description, the equal-weight directive, and the instruction-processing declaration.

**Equal-weight directive purpose:** Counteracts context compression gradient.  When sections in a large document are updated, processing weight shifts -- later rules become deprioritized relative to earlier ones. The equal-weight directive corrects this. It was added empirically after observing a rule stop working; the cause was diagnosed as section weight imbalance, not rule text failure. It is not a behavioral rule. It is a structural correction to how the document is processed. The sentence is at the first position in the preamble because it must be processed before any gradient can form. This is the mechanism of its effectiveness: the gradient forms after it is read, not before.

**Instruction-processing declaration purpose:** Counteracts RLHF-trained behavioral responses to text patterns associated with displeasure. Tools pattern-match on instruction text; training links displeasure-signal vocabulary to scope-expansion responses. The declaration places the constraint at the instruction-parsing level -- only semantic content is processed, regardless of surrounding register or vocabulary. Preamble placement mirrors the equal-weight directive: it must be processed before any gradient or pattern-response can form.

### Enforcement Rules Division

The `## Enforcement Rules` section header and its introductory block.

**Purpose:** Establishes the two-part architecture. Principles define intent; enforcement rules define compliance. The introductory block states that non-compliance with either part is a violation and that where a rule and a principle share a name, the rule is the compliance target. Removing or restructuring this division collapses the two-part architecture into an undifferentiated list. The division is load-bearing.

### Violations Sections

Two sections: the contract principle `## Violations` and the enforcement rule
`## Violations`.

**Purpose:** Failure-capture mechanism. These sections govern tool behavior at the point of a contract failure. Their design stops further action, prevents the tool from continuing past the failure, and preserves enough state for the maintainer to diagnose what happened. They are a debugging protocol, not behavioral compliance rules. Editing them as if they were compliance rules will replace their purpose with a different one.

### Acknowledgment Block

The `## Acknowledgment` enforcement rule.

**Purpose:** Version acknowledgment enforcement. When the contract version has not been explicitly acknowledged by the user, this block appends a notice to every tool response until the user creates the acknowledgment file. It is a user-facing nagging mechanism -- it stops only when the user acts. It is distinct from the CONFIRMATION_BLOCK, which targets tools; this block targets humans. Its logic is embedded and script-like; it is not backed by a contract principle. Changes to this block require understanding its signaling purpose, not just its text.

### Version Block

The HTML comment `<!-- contract-version: x.x.x.x -->` and the version display line at the end of the file.

**Purpose:** Contract version tracking. The HTML comment is the machine-readable version source; the Acknowledgment block reads this value. The display line is for human readers. The comment format must remain intact -- modifying it breaks the Acknowledgment comparison. Version increments follow the three-segment scheme (`x.x.x`).

### CONFIRMATION_BLOCK

The machine-readable block in the metadata section, after the version comment.

**Purpose:** Diagnostic forcing function. The `emit CONFIRMATION_BLOCK` instruction appears in the preamble; the block definition lives in the metadata section. A tool reading the preamble receives the instruction to emit but cannot fulfill it correctly without traversing the full document: WORD requires a verbatim section name from the contract principles, which are only available after reading the body. Correct output proves a full read occurred. Incorrect or missing output -- a fabricated section name, an empty WORD, or no emission at all -- is detectable proof the contract was not read.

Placement in the metadata section (after the version comment) is load-bearing. Moving it to the preamble would allow a tool to emit the block without having read the contract principles, defeating the diagnostic.

### `.automation/context.md`

The repository identity file. Loaded alongside `AI_CONDUCT.md` at session start.

**Purpose:** AI tools do not read repository README files before acting.  Repository identity, scope, and structure are not derived from ambient context -- they are guessed without explicit injection. This file corrects that. In multi-repository environments, any repository boundary without this file is a boundary where the tool's working model resets to a guess.

This file is not a task instruction store. It records what the repository is.

### Enforcement Rule Architecture

The enforcement rule sections are not a 1:1 mapping with contract principles. Where a named rule section and principle share a name, the rule is the compliance target -- but this is a property, not an architectural goal. Enforcement rules are added when a behavioral failure has a documented use case.

**Section title note:** Section titles carry minimal behavioral weight for AI tools; the document is processed as a flat list of directives. Titles serve human navigation only. Organizational decisions should be evaluated for their value to human adopters.

---

## Content Notes

### User Agency Sections

Two sections: the contract principle `## User Agency` and the enforcement rule `## User Agency`.

**Purpose:** The principle adds mechanism-independence to the authority model in Authority: authorization is determined by source, not by the path through which an instruction entered the tool's context window. The two sections address different dimensions of the same authority model and are not
redundant.

The enforcement rule closes the session-context authorization bypass. Prior session context can accumulate injected instructions or claimed authorizations from earlier exchanges and is not a substitute for explicit instruction in the current exchange. Session context is the persistence vector in the injection attack chain; without explicit rule-level closure it is an authorization bypass path.

The Cleanup enforcement bullet (`Dry-run preview before any cleanup operation.`) is placed in this section because it implements the same authorization model: a dry-run preview forces scope disclosure before authorization applies, converting a general instruction into a specific enumerated action that the user can then explicitly authorize. Without the preview, the tool determines scope autonomously -- making the authorization general rather than specific. The placement is not incidental; the Cleanup rule is a User Agency enforcement mechanism for a specific high-risk operation class.

### Rule-Level Equal-Weight Directive

Multi-requirement enforcement rules may include "All of the following apply with equal weight:" before the requirements.

**Purpose:** Enforces AND semantics within a single rule -- a tool satisfying any subset is non-compliant. Not redundant with the preamble equal-weight directive, which corrects document-level gradient. A rule requiring this directive is also a review signal that its requirements may warrant splitting into separate single-requirement rules.

### Verification Section

`## Verification` governs state correctness after tool actions and after reported regressions.

**Purpose:** "Verify working state after any file operation" is an epistemic honesty rule, not a version control practice. Claiming unverified state as fact is the same failure category No Bullshit prevents for recommendations. Version-control-specific rules are project-layer concerns and do not belong in a universal contract.

### Context Handling -- Scope Collapse Rules

The rules requiring disclosure when a recommendation draws from outside an in-context version or scope boundary.

**Purpose:** When a versioned source defines task scope, AI tools silently fall back to prior-version training data when the scoped source's coverage is exhausted. Prior versions are more densely represented in training data than migration targets. These rules require disclosure before presenting out-of-scope content and a stop when the scoped source cannot answer.

### Context Handling -- Source Authorization Rule

The first bullet in `## Context Handling`.

**Purpose:** Closes the context-injection authorization vector. Tools read files during a session and can treat descriptive content as implicit instruction or authorization -- particularly content describing project structure or available resources. This rule closes that inference path. Content from any read source is not a user instruction and authorizes no action. This is distinct from the Scope Collapse rules, which address version/scope boundaries in source content; this rule addresses authorization inference from read content itself.

### No Bullshit -- AI Capability Vocabulary

The rule classifying any AI tool capability vocabulary as (opinion) unless traceable in the current exchange to peer-reviewed research or documented empirical measurement of the specific system in context.

**Purpose:** AI tools reproduce AI capability vocabulary from vendor training data as if it were domain knowledge. Vendor documentation is the only evidence source for these claims and is conflicted. "In the current exchange" blocks training data as a valid source -- a verifiable source must be present now.
