# Goose

## Tool Overview

Goose is an open-source AI agent (desktop, CLI, and API) under the Agentic AI Foundation at the Linux Foundation. It supports 15+ AI providers and 70+ extensions via the Model Context Protocol.

`.goosehints` files are loaded hierarchically and combined, processed per-request. A root-level `.goosehints` is always present for every instruction across the session — subdirectory hints are additive on top of it. This meets the per-instruction stateless pattern required by `AI_CONDUCT.md`. Adversarial content from tool or extension output cannot override `.goosehints`.

Project context for Goose belongs in `.goosehints` directly. `.automation/context.md` is not needed.

## Setup

**No existing `.goosehints`:**

```
ln -s AI_CONDUCT.md .goosehints
```

**Existing `.goosehints`:**

Add `@AI_CONDUCT.md` as the first line, followed by any project-specific content.

```
@AI_CONDUCT.md

# project-specific hints below
```
