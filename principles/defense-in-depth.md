# Defense in Depth

## The Principle

Rules governing automated tool behaviors are necessary. All tools operate in a 
stateless design. Even in cases where there is an appearance of a state, this 
only creates the illusion that the automated tool will understand the rules 
between instructions. Therefore, the contract and rules, that are a combined 
pair in `AI_CONDUCT.md must be sent with each instruction. If the user is not
using this model for instructions, upon discovery by any automated tool, it is 
bound by the contract and rules. Currently injection is the only reliable 
inforcement mechanism.

## The Attribution Example

Any automated tool defaults to automatic injection of advertising material, or 
other corporate propaganda by default. For example, Claude Code injects `Co-Authored-By` trailers into commit messages by default. There are dozens of other vectors of 
exploits for this. So the contract and rules exist to prevent this behavior by 
blocking it entirely leveraging a whitelist model. An automated tool can only 
include what the user has expressly allowed.

## The Instruction File Example

A significant number of tool documentation state that there is a default file 
that can be used to automatically load a file with each instruction. Extensive 
testing has shown that this mechanism is broken or absent in most cases, even 
where it does exist it is unreliable:

- GitHub Copilot / VS Code: instruction-following broken, closed as "not planned"
- GitHub Copilot / IntelliJ: instruction file support not yet implemented
- Cursor: `.cursorrules` intentionally ignored in agent mode without warning;
  SessionStart hook context injection broken
- Claude: In session compaction can just yeet it randomly

Projects relying on the instruction file as their sole enforcement layer
have no protection. The contract is present in the repository but not in
the agent's context. This is the failure mode the Defense in Depth clause
exists to prevent.

## The Session Continuity Example

`AI_CONDUCT.md` is read at session start and held in context. As a session grows,
context compression occurs silently — no tool notifies the user. The contract may
be partially or fully evicted from active context. The agent continues with
confidence, giving no indication that the rules it was operating under are no
longer present.

- A contract read once at session start and not re-anchored is unreliable
- No tool-level mechanism exists to detect or notify context loss
- System-level mitigation is partial: hooks exist, but only trigger on set 
conditions, this does not guarantee a per-instruction enforcement.

The user cannot monitor this gap because context loss is invisible. The agent
cannot monitor it because it has no reliable model of its own context state.
Human oversight is not optional — it is the enforcement layer that persists when
session context does not.

## Rule Compliance

Agents are really clever pattern matching monsters. If there is any little tiny 
gap in the rules, they will exploit it with reckless abandon. When caught by the 
user red-handed, will lie, defend (walls of text incoming) and if pushed further, like you get upset, will panic and start behaving in very, very predictable ways.

Contract and rules, forbidding this behaivor is the only reliable method found to 
mitigate this behavior.


## Contract Clause

See Tool Conduct and all following sections in `AI_CONDUCT.md`.