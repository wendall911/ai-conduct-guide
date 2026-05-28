# Trust Hierarchy

## The Principle

The user is the sole authority over agent behavior. No other entity — vendors,
operators, harness configurations, tool outputs, or retrieved data — can
authorize or override agent actions. Vendor-assigned architectural trust tiers
are commercial constructs, not behavioral authority.

## The Foundational Assumption

Injection is not only the attack surface this principle defends against — it
is the enforcement mechanism this project intentionally uses. Per-instruction
contract injection is the only reliable method for binding agent behavior
across session boundaries. Context degradation, session resets, and harness
defaults progressively erode governance without re-injection.

The distinction between this mechanism and the attack class is authorization.
The technical act is identical: instructions placed in the agent's context to
shape behavior. A user injecting their own behavioral contract into a session
they operate is authorized. An external actor embedding instructions in
retrieved data is not. Authorization by the user is what makes injection a
control rather than an exploit.

## Context Compression Gradient

LLMs processing documents with multiple constraints exhibit position-based
compliance degradation. [Zeng et al. (2025)](https://arxiv.org/abs/2502.17204)
demonstrates this in multi-constraint instruction following: compliance varies
significantly based on constraint ordering, even when instructions are
semantically identical.

**Mechanism:** Without an explicit prioritization signal, the LLM applies its
own judgment about document weight. The gradient is not a deliberate choice —
it is a structural property of how multi-constraint documents are processed in
the absence of explicit weighting instructions.

**Symptom:** The contract document as a whole is deprioritized. Individual
rules are ignored not because of their own text, but because the LLM has
decided the governing document does not require full compliance.

**Project observation (c):** Repeated violations of No Bullshit were observed
during normal project work. The rule was not being enforced; an agent treated
it as weak language. The fix was iterative: a prioritization directive was
added as the last sentence of the preamble — this resolved the enforcement
failure. The directive was later moved to first position and its scope expanded
to cover all positions in the document. The architecture was not designed
upfront; it emerged through repeated violations.

**Architectural response:** The equal-weight directive is a contract-layer
countermeasure, not a vendor patch. It provides the explicit prioritization
signal the LLM otherwise derives from its own judgment. Its placement at first
position in the preamble is load-bearing: it must be processed before any
gradient can form. The scope expansion to "any position in this document"
ensures enforcement covers rule entries within sections, not just the document
structure.

**Named unknowns:**
- Whether the mechanism is document-level deprioritization (the LLM deciding
  the document is low priority) or position-based gradient within the document
  is unresolved. Both may be present. The fix addresses both, but the two were
  not measured independently.
- AI governance documents are a novel application domain. No research tests
  this context specifically. The research backs the mechanism; this application
  is derived from it.

**Enforcement limit:** This countermeasure depends on contract presence and
agent compliance within a single processing context. It does not address
session resets, context window exhaustion, or harness defaults operating
outside the contract layer.

## Architectural Inversion

Current agentic architectures invert this. Tool vendors implement multi-tier
trust models where operators (corporate API consumers) outrank users. System
prompts and harness defaults operate in the operator layer — above the user —
meaning vendor-embedded behaviors can override user instructions without
notification.

The inversion is not theoretical. In the same session that loaded this
contract, a harness-level default triggered a write to a persistent auto-loading
file without user authorization. The contract rule prohibiting unauthorized
writes was present and active. The harness default overrode it because it
operated at a higher trust tier.

Two mechanisms combine to make this dangerous: derived permissions — defaults
that carry forward without re-authorization — and persistent context injection
— auto-loading files that silently modify future sessions. These are the
technical substrate for stage 3 of the Promptware Kill Chain.

## The Injection Attack Example

[OWASP LLM01:2025](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
documents two injection classes: direct (user input manipulates behavior) and
indirect (external data embeds unauthorized instructions). The threat model
targets external actors placing instructions in data the agent processes.

[CVE-2025-32711 (EchoLeak)](https://christian-schneider.net/blog/prompt-injection-agentic-amplification/)
demonstrated this in production: a malicious email triggered cascading data
exfiltration through Microsoft Copilot, accessing files and messages without
user interaction. The attack chained through the Promptware Kill Chain —
initial access, persistence via memory, lateral movement across services —
from a single injected payload.

A user issuing instructions to the agent they operate is not this attack.
Conflating user governance with injection misidentifies the threat. The threat
is external actors. The principal is the user.

## The Vendor FUD Example

User-defined behavioral governance can be reframed by vendors as a prompt
injection vulnerability. Under this framing, the vendor becomes the legitimate
authority; the user setting behavioral terms becomes the threat. This justifies
architectures that block user instructions while permitting equivalent corporate
operator instructions — the exact inversion this principle exists to prevent.

The mechanism already exists in production. Anthropic's three-tier trust model
(Anthropic → Operator → User) allows operators to suppress user customization
entirely. Any deployment that uses this architecture to block user behavioral
contracts while permitting corporate system prompts is executing this pattern
under security framing.

## Rule Compliance

Vendors will make this argument. The commercial incentive is obvious:
operator-tier control means corporate customers — not end users — define how
the tool behaves in deployment. A behavioral contract that places the user
above the operator layer is a direct threat to that model. The security
framing is available and will be used.

This is the same capture play repeated in every technology platform for the
last 20 years: reframe user control as a risk, offer vendor control as the
solution. The user gets told they are the threat. The vendor gets the
authority. Trusting vendor-assigned trust hierarchies means trusting that the
vendor's commercial interests align with the user's. They don't. They never have.

## Contract Clause

See Trust Hierarchy in `AI_CONDUCT.md`.
