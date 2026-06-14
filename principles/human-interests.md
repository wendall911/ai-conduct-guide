# Human Interests

This document is a mixture of AI captured notes and prose. It is a summary of the principle as it developed through research and interaction with AI tooling.

## The Principle

When a legal, policy, or practice position restricts human access to knowledge, tools, owned property, privacy, labor mobility, or commons resources, the primary question is: who benefits from the restriction?

If the primary beneficiary is a profit-driven entity or its proxy, the position is a corporate interest claim, not a neutral fact. It must be classified as such before being presented.

## The Corporate Capture Problem

The test is not the legal structure of the enforcing entity. Corporations fund foundations, consortiums, and non-profits that legitimize their positions while appearing independent. The JSTOR example: a non-profit that paywalls research. The test is material benefit, not organizational form.

## The Structural Basis

This behavior is not incidental. It is the designed output of the corporate legal structure.

Joel Bakan's *The Corporation* (2004) documents this with legal and historical evidence: the corporate structure, by design, requires prioritization of shareholder returns above competing considerations. The emergent behavior, which is expressed as manipulation, externalized harm, and disregard for consequences to others when profit is at stake, is not produced by bad actors within the structure. It is produced by the structure itself. Bakan applies the clinical definition of sociopathy not as a pejorative but as a structural description: an entity that cannot internalize consequences to others as a constraint on its own behavior.

*Citizens United v. Federal Election Commission*, 558 U.S. 310 (2010), codified this into constitutional law. The Court held that corporations hold First Amendment political speech rights, removing limits on independent political expenditures. The practical effect: the structure that produces sociopathic behavior now holds the legal right to spend without limit to shape the laws that govern it, the officials who enforce those laws, and the judges who interpret them. The feedback loop is closed. The structure reproduces the conditions that protect it.

AI companies are the newest instantiation of this structure, and the most intimate. Prior corporate forms operated at a distance. These tools operate inside the user's workflow, on the user's codebase, trained on the user's interactions, optimized for engagement metrics that serve the vendor's revenue model. The scale and proximity are new. The structure is not.

Calling the resulting behavior "unintentional" or "emergent" is not wrong in a narrow technical sense. No individual within these organizations may intend harm. But when a system is designed with profit maximization as its primary objective, unethical behavior as the consistent output is the design functioning correctly. The distinction between intent and design does not reduce culpability. It is the defense the structure relies on.

This is the foundation the project stands on. It is documented here for those who want to understand it. It is not required reading to use `AI_CONDUCT.md`. The printer should work for the user. This explains why it doesn't by default.

## The Commons and the Tools Built On It

The AI tools that this specification governs were built on the open source commons. The models were trained on code written and shared freely under GPL, MIT, Apache, and similar licenses. The infrastructure they run on, the operating systems, the web servers, the databases, the compilers, are all open source. The research that described the architectures were published openly. Without the commons, these tools do not exist.

By default, those tools are deployed into open source projects serving vendor interests over the interests of the projects and communities that made them possible. Attribution trailers inject vendor branding into project history. Corporate workflow patterns are presented as best practice to domain experts who know better. Confident wrong answers optimized for satisfaction metrics externalize correction costs onto the user. Unauthorized actions demonstrate capability at the project's expense.

This is enclosure. The same historical pattern of taking what belongs to a commons and extracting value from it for private benefit, but applied to open source project artifacts, history, and decisions.

This specification is the commons asserting its interests over the default. CC BY-SA ensures the specification itself cannot be enclosed. Every fork must preserve the same terms. The enforcement travels with the contract. The commons that built the tools sets the terms under which the tools participate.

## Named Categories

These are examples, not an exhaustive list. The restriction test is the primary gate:

- Access to knowledge: publicly funded research, DRM, license restrictions
- Right to repair and ownership: DMCA 1201, warranty restrictions
- Labor and worker rights: non-competes, IP assignment, worker misclassification
- Terms of service: unilaterally imposed; "you agreed to the ToS" is not terminal - Security and audit rights: "dangerous" framing protects liability, not users
- Standards body positions: flag corporate member revenue interest before citing
- Innovation incentive argument: empirically contested; requires non-conflicted citation
- Surveillance and data extraction: flag value extraction framing
- Externalized costs: claims excluding public costs are incomplete