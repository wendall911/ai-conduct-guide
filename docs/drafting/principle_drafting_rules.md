# Principle Drafting Rules

These rules govern ALL output in this session without exception. Any output that does not comply with ALL rules simultaneously is a violation.

# Rules

- Empirical claims *MUST* include an inline link at first use. A claim without an inline link present in the document is unsourced.
- Specific figures and measurements *MUST* have a named source before being embedded. A precise number without a source *MUST* be classified as (c) and flagged — not embedded as (a) or (b).
- When a claim is flagged as needing a source, the action *MUST* be to find and add the citation. Rewriting or removing the claim is prohibited until a source search has been completed and returned no result.
- Claims *MUST* state their evidence tier: (a) peer-reviewed research, (b) documented real-world observations — named bug reports, community consensus, incident records — (c) project-level observation without external corroboration. Tier (b) is valid backing when (a) does not exist or does not reach the specific claim. Tier (c) *MUST* be stated as a project observation and *MUST NOT* be presented as independent validation.
- Research *MUST* be scoped to the claim it backs. A source documenting a mechanism does not automatically support claims about that mechanism's amplification in newer architectures. When a claim covers both a mechanism and a context-specific amplification (e.g., agentic workflows), each *MUST* be sourced separately. If no source exists for the amplification, name it as an unknown.
- Observed behavior and designed behavior *MUST* be stated separately. A rule designed to produce an outcome and a confirmed outcome are not the same claim and *MUST NOT* be presented as equivalent.
- The mechanism causing a failure *MUST* be documented separately from the symptom the failure produces. Both *MUST* be present when both are known.
- Unresolved questions *MUST* be stated explicitly as named unknowns. Omission and hedged language are not substitutes.
- Vendor patches and contract-layer approaches *MUST* be distinguished. They *MUST NOT* be presented as equivalent mitigations.
- Enforcement limits *MUST* be stated explicitly. Any rule whose enforcement depends on contract presence or tool compliance *MUST* say so.
- Before rewriting a claim, the source *MUST* be verified. A citation addition or targeted correction *MUST* be attempted before a rewrite is proposed.
- Changes *MUST* be the minimum necessary to satisfy the requirement. A full section rewrite when a source addition and sentence correction would suffice is a violation.
- Principles documents *MUST* document architectural reasoning. Restating a contract rule verbatim without adding reasoning is not compliant content.
