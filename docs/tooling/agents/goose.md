# Goose

## Tool Overview

Goose is an open-source AI agent (desktop, CLI, and API) under the Agentic AI Foundation at the Linux Foundation. It supports 15+ AI providers and 70+ extensions via the Model Context Protocol.

## Notes

Architecturally, goose is designed to deprioritize user agency. This project is moving away from MCP architecture to Agent Client Protocol [ACP](https://agentclientprotocol.com) and [Open Plugins](https://open-plugins.com). The best-case scenario for Open Plugins is that a user could create a hook to inject in UserPromptSubmit event. This is still fed to the LLM as a "suggestion". There is currently no built-in extenstion available to inject AI_CONDUCT.md as a system instruction, though it still remains possible to do so with a custom ACP extension that wraps AI_CONDUCT.md as a system-level instruction. This would also need to be paired with opting out of "Auto Mode", which is enabled by default.

## Setup

Not recommended. Not supported.

No working workaround exists. The tool cannot enforce per-turn contract loading. Do not use goose for workflows requiring AI_CONDUCT.md enforcement. Even if manually passing AI_CONDUCT.md for read each turn, the contract is still immediately deprioritized. This is an architectural design decision to explicitly override user instruction, directly overriding User Agency.