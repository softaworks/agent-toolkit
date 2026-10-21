# MiniMax Skill

Configure MiniMax-M3.1-Flash and MiniMax-M3 with the correct protocol, regional API base, capability, reasoning, and token-pricing guidance.

## When to Use

Use this skill when a request mentions:

- MiniMax API configuration
- `MiniMax-M3.1-Flash` or `MiniMax-M3`
- Global or China-region MiniMax endpoints
- MiniMax model capabilities, reasoning behavior, or cost estimates
- Preserving an existing `MiniMax-M2.7` configuration

## What It Covers

- Flash-first active model selection with M3 as the second active choice
- Preservation of explicit saved, custom, and legacy model IDs
- Global and China-region endpoint families
- Chat Completions, native Responses, and Anthropic Messages protocol guidance
- Context windows, multimodal inputs, and reasoning behavior
- Input, output, and cache-token pricing
- API-key handling through `MINIMAX_API_KEY`

## Installation

Install the plugin from the Agent Toolkit marketplace:

```text
/plugin install minimax@agent-toolkit
```

Then ask the agent to configure MiniMax, select a region or model, preserve an existing model ID, or estimate token costs. See `SKILL.md` for the current workflow and configuration tables.
