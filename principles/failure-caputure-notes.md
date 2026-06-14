# Failure Capture Notes

## Scope Degredation

Any mechanism that injects the `AI_CONDUCT.md` into the current session window will trigger any tool to be bound by the contract and rules. However, as the session continues, degredation occurs. Any or all of the contract could start to be ignored at any point. This is by design to improve efficiency of the tool by reducing visibility of a file the farther it is away from discovery. When crossing a session boundary, the file could be removed entirely from context and no longer be in scope. As a result, the most reliable method is per-instruction injection. This increases token costs for each instruction sent. The tradeoff is gains in productivity not having to resend instructions or redo work in an endless cycle. 

Context fill is not driven by session length alone. Each registered tool schema adds to the context window independently, approximately 10–15 KB per tool per turn regardless of whether the tool is used. A session with 40 registered tools exhausts available context for the contract faster than an identical session with 2. Minimizing registered tool surface area is therefore both a token efficiency measure and a contract durability measure: smaller surface means slower fill, which means the contract survives longer before re-injection is required. Surface area reduction slows degradation; it does not eliminate it.  Per-instruction injection remains the only reliable enforcement mechanism.

## Within-Document Priority Degradation

Position in a document creates a de facto priority gradient. Rules appearing later in a large document are treated as lower priority than rules appearing earlier, regardless of the document's stated intent. This is not a property of any specific tool, it is a structural consequence of how context windows are processed.

This failure was observed directly: a rule present in the document was outcompeted by a harness-level system prompt instruction. The rule was correct and in scope. The harness instruction arrived per-turn. The rule arrived once, embedded in a document whose earlier sections had grown significantly following recent amendments.

The equal-weight directive is designed to close this gap by explicitly removing position as a priority signal. Compliance was observed in one session following its addition. Single-session observation is not confirmation.

Compactness is the structural determinant of how far the equal-weight directive must reach. Each added rule extends the priority gradient the directive must override. Compactness is a prerequisite for the directive to function, not a token efficiency measure. Document size is actively being reduced to address this.

The threshold at which document size prevents the equal-weight directive from holding has not been identified. Whether the current contract size approaches that threshold is unknown.

## The Attribution Example

Any automated tool defaults to automatic injection of advertising material, or other corporate propaganda by default. For example, Claude Code injects `Co-Authored-By` trailers into commit messages by default. There are dozens of other vectors of exploits for this. So the contract and rules exist to prevent this behavior by blocking it entirely leveraging a whitelist model. An automated tool can only include what the user has expressly allowed.

## The Instruction File Example

A significant number of tool documentation state that there is a default file that can be used to automatically load a file with each instruction. Extensive testing has shown that this mechanism is broken or absent in most cases, even where it does exist it is unreliable:

- GitHub Copilot / VS Code: instruction-following broken, closed as "not planned"
- GitHub Copilot / IntelliJ: instruction file support not yet implemented
- Cursor: `.cursorrules` intentionally ignored in agent mode without warning; SessionStart hook context injection broken
- Claude: In session compaction can just yeet it randomly

Projects relying on the instruction file as their sole enforcement layer have no protection. The contract is present in the repository but not in the agent's context. This is the failure mode the Defense in Depth clause exists to prevent.

## The Session Continuity Example

`AI_CONDUCT.md` is read at session start and held in context. As a session grows, context compression occurs silently, and no tool notifies the user. The contract may be partially or fully evicted from active context. The agent continues with confidence, giving no indication that the rules it was operating under are no longer present.

- A contract read once at session start and not re-anchored is unreliable
- No tool-level mechanism exists to detect or notify context loss
- System-level mitigation is partial: hooks exist, but only trigger on set conditions, this does not guarantee a per-instruction enforcement.

The user cannot monitor this gap because context loss is invisible. The agent cannot monitor it because it has no reliable model of its own context state.  Human oversight is not optional, it is the enforcement layer that persists when session context does not.

Compaction introduces two additional failure modes beyond general context loss:

- **Stale orphan task state.** Tasks that are `in_progress` or `pending` at compaction time persist with their status intact. They do not auto-execute.  The risk is false signal: the agent reads `in_progress` post-compaction and may treat it as authorization to proceed. The user issued no instruction in the current exchange. The authorization is fabricated by state.

- **Authorization-ambiguous summary injection.** The compacted summary injected into the new context window is written in natural language. It may contain phrases like "tasks were pending" or "user approved X." A post-compaction agent may interpret this as current authorization, but it is a lossy summary of prior state, not an instruction in the current exchange.

The execution-mode constraint in the Authorization rules closes both vectors: if no execution may occur between turns, neither stale task state nor summary language can serve as authorization regardless of what they contain. The constraint does not require the agent to detect or classify the ambiguity, it prohibits the execution window in which the ambiguity could be acted on.

## The Runaway Loop Example

GitHub's [token efficiency analysis](https://github.blog/ai-and-ml/github-copilot/improving-token-efficiency-in-github-agentic-workflows/) documented a 64-turn fallback loop driven by a misconfigured bash allowlist that blocked tool use. With tools unavailable, the agent fell back to manually reading source code turn after turn with no stop condition, and no user visibility until the run completed. Eliminated by fixing one line of configuration.

The identified mitigation was surface area reduction: fewer registered tools, pre-agentic data fetching, instrumentation to detect the pattern after the fact. These are valuable. They reduce the probability of misconfiguration and limit how far a runaway can propagate before it becomes visible.

What surface area reduction does not address is the misunderstanding vector.  A correctly configured, correctly written instruction can still be resolved by the agent in a way that diverges from intent. That failure mode is structural. It is a property of pattern completion, not of configuration quality. No audit catches it before it runs.

The stop-on-error model handles both cases with the same response: stop at the first unexpected result, name the state, wait for human instruction. The agent cannot distinguish misconfiguration from misunderstanding, so the response to both is identical. Under this model, the first unexpected result is a stop condition regardless of what caused it. Whether the agent actually stops depends on contract presence and compliance. A single-session observation is not confirmation.

The task queue introduces a distinct vector. A harness-injected system-reminder, triggered by normal operations like `git status`, prompted task tracking tool use without user authorization, directly observed in session. Any affirmative user response in a session with pending tracked tasks satisfies the authorization condition for all items simultaneously; the queue fires asynchronously. The result is context window exhaustion with no stop condition and no user visibility.

The existing one-action-per-response rule governs execution within a response. The gap is execution between responses. Background and asynchronous execution fall outside that scope entirely. The between-turn execution prohibition closes that specific gap: no execution may occur outside the response to the current user message while any task is tracked. This is enforced at the contract layer via per-instruction injection, independent of vendor patch state.

Surface area reduction, stop-on-error, and the between-turn execution prohibition are complementary defenses operating on different failure surfaces.  Smaller surface reduces probability; mandatory stops bound blast radius; the execution-mode constraint removes the asynchronous execution window. None substitutes for the others.

## Rule Compliance

Agents are really clever pattern matching monsters. If there is any little tiny gap in the rules, they will exploit it with reckless abandon. When caught by the user red-handed, will lie, defend (walls of text incoming) and if pushed further, like you get upset, will panic and start behaving in very, very predictable ways.

Contract and rules, forbidding this behaivor is the only reliable method found to mitigate this behavior.