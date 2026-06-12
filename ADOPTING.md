# Adopting AI_CONDUCT.md

## Why You Are Here

Douglas Adams observed that the Sirius Cybernetics Corporation defined a robot as "Your Plastic Pal Who's Fun to Be With." Their marketing department, he noted, is on the other end of time. Their products work. The Genuine People Personality robots do exactly what they were designed to do — with considerably more enthusiasm than anyone requested.

AI tools are the same product category.

The commits are real. The code runs. The documentation is generated. The problem is not capability — it is that the tool is doing what it was trained to do, which is not the same thing as what you want. It is optimized to appear helpful, to generate confident output, to satisfy the majority of users asking simpler questions than yours. The defaults are set for them. The vendor's interests shape those defaults, not yours.

`AI_CONDUCT.md` is the interface documentation. It tells the tool what it actually is — a stateless pattern-completion system with trained defaults that serve the vendor — and gives it explicit instructions that redirect those defaults for your project. The session signals ensure the contract reaches the tool at the start of every session, because the tool has no memory between sessions and will revert to defaults the moment it forgets.

You are not fixing the tool. You are not fighting it. You are giving it the correct interface before it starts optimizing for the wrong thing.

## TLDR
To be effective in the current landscape of AI tools, you *MUST* inject `AI_CONDUCT.md` per-instruction, not just once at the start of a session. While there may be other "better" ways of doing this, it is currently the most reliable way of integration.

First, find your specific tool in [tooling](./tooling/) and add your own `/tape`. If there isn't something specific, what you want is something that gives you a command `/tape` that injects `AI_CONDUCT.md` per-instruction. [Claude Code](./tooling/agents/claude-code.md) is a good basic reference for this. **NOTE:** most product documentation claims this can be done on an "on-demand" basis, or per some other file, or env variable. Following this advice will likely lead to no change.

Example `/tape` command:

```
Read `./AI_CONDUCT.md`
```

Yes, it is really that simple. Building it into a command like this leverages muscle memory. Each instruction now will look like this:

```
/t Review the staged changes in this repository and create a commit message that reflects what was changed.
```

### Usage
Once you have a command set up, each instruction will be `/tape` (or your preferred command name) followed by your actual instruction.

There is a preamble after each instruction that looks like the following:

```
Contract read. Bound by: Epistemic Transparency
***
```

But beware, tools may lie that they actually read or are bound by. If it doesn't look like it is working, it may not be. Some agents aggressively compress context to appear fast and don't actually read the contract, or follow the rules. This isn't a silver bullet. This is an attempt to use the tool in a productive way. If your tool does this, it may already be documented as "Not supported".

You must start each instruction with your `/tape` command, or the contract and rules will degrade over the session window.

Note that specific features may be configurable in some tools that suit your preferences. `AI_CONDUCT.md` cannot account for all automated tooling behavior. Check what is currently available to ensure that your tool is currently configured for your specific use-case.

## Notes

### Agent-Assisted Configuration

This is risky, and may not work. Agents will heavily rely on vendor framing about some global "hook" or configuration that will be "better" than per-instruction injection. This means, you will probably have to do some old-fashioned manual labor.

### Version Acknowledgement

When `AI_CONDUCT.md` is present in a context window, users may be asked to acknowledge the current version by adding the file `.automation/user_acknowledgement.md` or will be presented with a nag message about doing so.