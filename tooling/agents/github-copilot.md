# GitHub Copilot Agent

## Tool Overview

GitHub Copilot Agent is Microsoft's agentic workflow integration for VS Code.

## Setup

Not recommended. Not supported.

The agent cannot reliably read `AI_CONDUCT.md` by any currently available method. The failure is at the tool layer: file read operations can return empty strings to the model context, and can happen in unpredictable ways in a session.