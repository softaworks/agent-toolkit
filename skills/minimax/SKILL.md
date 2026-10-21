---
name: minimax
description: Configure MiniMax-M3.1-Flash and MiniMax-M3 text models, regional API endpoints, capabilities, thinking behavior, and token pricing. Use when users mention MiniMax-M3.1-Flash, MiniMax-M3, MiniMax API setup, or MiniMax regions.
---

# MiniMax Provider Guide

Use this skill when configuring a MiniMax text model or preserving an existing MiniMax configuration.

## Configuration Workflow

1. Preserve an explicitly saved, custom, or legacy model ID. Do not silently replace an existing `MiniMax-M2.7` selection or alter non-MiniMax settings.
2. For a new configuration without an explicit model, select `MiniMax-M3.1-Flash` as the default. Offer `MiniMax-M3` as the second active choice, including when disabled thinking is required.
3. Ask which region the user needs: `global_en` or `cn_zh`. Default to `global_en` only when no region is specified. Keep the region and endpoint family together.
4. Select the client's supported protocol. Keep an existing Chat Completions or Anthropic Messages integration working; native Responses is also available on the documented global endpoint.
5. Read the API key from `MINIMAX_API_KEY`. Never print, log, or commit it.
6. Present the chosen model, region, protocol, endpoint, and capability assumptions before making changes.

## Regional Endpoints

| Region | Chat Completions base URL | Anthropic Messages base URL | Documentation |
| --- | --- | --- | --- |
| `global_en` | `https://api.minimax.io/v1` | `https://api.minimax.io/anthropic` | `https://platform.minimax.io/docs` |
| `cn_zh` | `https://api.minimaxi.com/v1` | `https://api.minimaxi.com/anthropic` | `https://platform.minimaxi.com/docs` |

The [native Responses contract](https://platform.minimax.io/docs/api-reference/responses-create.md) documents `POST https://api.minimax.io/v1/responses`. Verify model availability and account entitlement for the selected region. Do not infer China Responses, third-party gateway, or subscription support from this international endpoint documentation.

## Active Model Selection

| Order | Model | Context window | Input modalities | Thinking behavior |
| ---: | --- | ---: | --- | --- |
| 1 | `MiniMax-M3.1-Flash` | 1,000,000 tokens | text, image, video | adaptive only; default effort `max` |
| 2 | `MiniMax-M3` | 1,000,000 tokens | text, image, video | adaptive or disabled; effort does not tune depth |

Flash supports up to 524,288 output tokens. Set an appropriate per-request output budget rather than automatically requesting the maximum; reasoning tokens consume that budget too.

### Protocol-specific thinking

For Flash, accepted effort values are `low`, `medium`, `high`, `xhigh`, and `max`; omission defaults to `max`. Disabling thinking or sending effort `none` returns HTTP 400. Do not send `service_tier` or `reasoning_split: false` for Flash.

| Protocol | Flash effort field | M3 thinking default and control |
| --- | --- | --- |
| Chat Completions | `reasoning_effort` | On by default; `thinking.type` accepts `adaptive` or `disabled`. Effort does not tune depth. |
| Anthropic Messages | `output_config.effort` | Off by default; enable with `thinking.type: adaptive`. Effort does not tune depth. |
| Responses | `reasoning.effort` | Off by default; a non-`none` effort enables reasoning without tuning depth. |

Preserve complete assistant, tool, reasoning, and multimodal content in subsequent turns. For Chat Completions this includes `tool_calls`, `<think>` content, and Flash's `reasoning_content`; for Messages, preserve all thinking, text, and tool-use blocks. For Responses, send full client-side history, including tool calls, results, and reasoning items: `store`, `previous_response_id`, `conversation`, and automatic truncation are unsupported or ignored.

See the [Chat Completions guide](https://platform.minimax.io/docs/api-reference/text-openai-api.md) and [Messages guide](https://platform.minimax.io/docs/api-reference/text-anthropic-api.md) for the current wire formats.

## Legacy Compatibility

Preserve existing saved, custom, or legacy model IDs, including `MiniMax-M2.7`, without silently rewriting them. Keep legacy models out of ordinary active recommendations; verify their availability on the existing route separately. M2.7's older text-only capability and always-on thinking behavior do not describe Flash or M3.

## Token Pricing

Prices are in USD per million tokens. These are direct API rates; gateway and regional pricing must be checked separately against the selected provider's contract.

| Model / standard input tier | Input | Output | Cache read | Cache write |
| --- | ---: | ---: | ---: | ---: |
| `MiniMax-M3.1-Flash` | $0.15 | $0.60 | $0.015 | Not specified |
| `MiniMax-M3`, <=512k input tokens | $0.30 | $1.20 | $0.06 | Not specified |
| `MiniMax-M3`, >512k input tokens | $0.60 | $2.40 | $0.12 | Not specified |

M3 priority admission costs 1.5 times the standard rate and uses `service_tier: priority`; this field is unsupported for Flash. Retain the provider's published `512k` notation without inventing an exact integer threshold. Confirm the [current pricing](https://platform.minimax.io/docs/pricing/overview.md), state assumptions, and calculate each token category separately. Do not invent an unspecified cache-write price.

## Validation Checklist

- Ordinary choices list `MiniMax-M3.1-Flash` first and `MiniMax-M3` second.
- Explicit saved, custom, and legacy model IDs are preserved, including `MiniMax-M2.7`.
- The endpoint belongs to the selected region and protocol.
- Flash does not send disabled thinking, effort `none`, `service_tier`, or `reasoning_split: false`.
- M3 thinking defaults are selected by protocol; multimodal inputs remain intact.
- Full tool and reasoning history is retained client-side.
- The API key remains in `MINIMAX_API_KEY` and is never included in output.
