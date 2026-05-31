# AI_CONDUCT.md Architectural Design

Operational context for `AI_CONDUCT.md`. Records components whose purpose is not derivable from their text. Required before any drafting session targeting a structural component.

---

## Structural Components

### Preamble

The block of text before the first named section. Contains the contract scope declaration, the binding statement, the two-part document description, and the equal-weight directive.

**Equal-weight directive purpose:** Counteracts context compression gradient.  When sections in a large document are updated, processing weight shifts — later rules become deprioritized relative to earlier ones. The equal-weight directive corrects this. It was added empirically after observing a rule stop working; the cause was diagnosed as section weight imbalance, not rule text failure. It is not a behavioral rule. It is a structural correction to how the document is processed. The sentence is at the first position in the preamble because it must be processed before any gradient can form. This is the mechanism of its effectiveness: the gradient forms after it is read, not before.

### Enforcement Rules Division

The `## Enforcement Rules` section header and its introductory block.

**Purpose:** Establishes the two-part architecture. Principles define intent; enforcement rules define compliance. The introductory block states that non-compliance with either part is a violation and that where a rule and a principle share a name, the rule is the compliance target. Removing or restructuring this division collapses the two-part architecture into an undifferentiated list. The division is load-bearing.

### Violations Sections

Two sections: the contract principle `## Violations` and the enforcement rule
`## Violations`.

**Purpose:** Failure-capture mechanism. These sections govern agent behavior at the point of a contract failure. Their design stops further action, prevents the agent from continuing past the failure, and preserves enough state for the maintainer to diagnose what happened. They are a debugging protocol, not behavioral compliance rules. Editing them as if they were compliance rules will replace their purpose with a different one.

### Acknowledgment Block

The `## Acknowledgment` enforcement rule.

**Purpose:** User-facing notification that `AI_CONDUCT.md` is loaded and active in the current session. When the contract version has not been acknowledged by the user, this block appends a notice to every response until the user creates the acknowledgment file. It is a signaling mechanism directed at humans. Its logic is embedded and script-like; it is not backed by a contract principle.  Changes to this block require understanding its signaling purpose, not just its text.

### Version Block

The HTML comment `<!-- contract-version: x.x.x.x -->` and the version display line at the end of the file.

**Purpose:** Contract version tracking. The HTML comment is the machine-readable version source; the Acknowledgment block reads this value. The display line is for human readers. The comment format must remain intact — modifying it breaks the Acknowledgment comparison. Version increments follow the four-segment scheme used by the Acknowledgment partial-match logic.

### `.automation/context.md`

The repository identity file. Loaded alongside `AI_CONDUCT.md` at session start.

**Purpose:** Agents do not read repository README files before acting.  Repository identity, scope, and structure are not derived from ambient context — they are guessed without explicit injection. This file corrects that. In multi-repository environments, any repository boundary without this file is a boundary where the agent's working model resets to a guess.

This file is not a task instruction store. It records what the repository is.

---

## Content Notes

### User Agency Sections

Two sections: the contract principle `## User Agency` and the enforcement rule `## User Agency`.

**Purpose:** The principle adds mechanism-independence to the authority model in Authority: authorization is determined by source, not by the path through which an instruction entered the agent's context window. The two sections address different dimensions of the same authority model and are not
redundant.

The enforcement rule closes the session-context authorization bypass. Prior session context can accumulate injected instructions or claimed authorizations from earlier exchanges and is not a substitute for explicit instruction in the current exchange. Session context is the persistence vector in the injection attack chain; without explicit rule-level closure it is an authorization bypass path.

The Cleanup enforcement bullet (`Dry-run preview before any cleanup operation.`) is placed in this section because it implements the same authorization model: a dry-run preview forces scope disclosure before authorization applies, converting a general instruction into a specific enumerated action that the user can then explicitly authorize. Without the preview, the agent determines scope autonomously — making the authorization general rather than specific. The placement is not incidental; the Cleanup rule is a User Agency enforcement mechanism for a specific high-risk operation class.
