# GitHub Copilot Agent

## Tool Overview

GitHub Copilot Agent is Microsoft's agentic workflow integration for VS Code.

## Setup

Not recommended. Not supported.

This tool cannot reliably read `AI_CONDUCT.md` by any currently available method. The failure is at the architecture layer: any file or tool operation can return empty string to the model context, and can happen in unpredictable ways in a session, or even a single instruction.