---
name: minimax
description: Configure MiniMax-M3.1-Flash and MiniMax-M3 text models, regional API endpoints, capabilities, reasoning behavior, and token pricing. Use when users mention MiniMax-M3.1-Flash, MiniMax-M3, MiniMax API setup, or MiniMax regions.
---

# MiniMax Provider Guide

Use this skill when configuring an active MiniMax text model or preserving an existing MiniMax configuration.

## Configuration Workflow

1. Select `MiniMax-M3.1-Flash` as the ordinary default, followed by `MiniMax-M3` when the request needs the broader M3 capability profile.
2. Preserve an explicitly saved, custom, or legacy model ID. Do not silently replace an existing `MiniMax-M2.7` selection.
3. Select the user's region and protocol. Keep the region and endpoint family together.
4. Use the native Responses API when the client supports it, or keep an existing Chat Completions or Anthropic Messages integration working. Responses stateful fields are not a substitute for client-side history.
5. Read the API key from `MINIMAX_API_KEY`. Never print, log, or commit it.
6. Present the chosen model, region, protocol, endpoint, and capability assumptions before making changes.

## Regional Endpoints

| Region | OpenAI-compatible base URL | Responses path | Anthropic Messages base URL | Documentation |
| --- | --- | --- | --- | --- |
| `global_en` | `https://api.minimax.io/v1` | `/responses` | `https://api.minimax.io/anthropic` | `https://platform.minimax.io/docs` |
| `cn_zh` | `https://api.minimaxi.com/v1` | `/responses` | `https://api.minimaxi.com/anthropic` | `https://platform.minimaxi.com/docs` |

The current official Flash Responses contract documents `POST https://api.minimax.io/v1/responses`. Regional endpoint availability and account entitlement must be verified for the selected region; do not infer China or third-party gateway Flash support from the international documentation.

## Active Model Selection

| Order | Model | Context window | Max output | Input modalities | Reasoning behavior |
| ---: | --- | ---: | ---: | --- | --- |
| 1 | `MiniMax-M3.1-Flash` | 1,000,000 tokens | 524,288 tokens | text, image, video | adaptive only; default effort `max` |
| 2 | `MiniMax-M3` | 1,000,000 tokens | provider-defined | text, image, video | adaptive or disabled; effort is ignored |

For Flash, accepted effort values are `low`, `medium`, `high`, `xhigh`, and `max`; `none` is rejected. OpenAI-compatible clients send `reasoning_effort`; Anthropic clients send `output_config.effort`. Do not send unsupported `service_tier` or `reasoning_split` fields.

For M3, omitted thinking is off in the Responses contract. A non-`none` effort turns reasoning on without tuning its depth. Keep the client-side tool and reasoning history because `store`, `previous_response_id`, `conversation`, and truncation state are unsupported or ignored by the Responses contract.

## Legacy Compatibility

`MiniMax-M2.7` is retained for existing saved, custom, or legacy configurations. Do not present it ahead of the active Flash or M3 choices, and do not reset a stored model ID merely because it is absent from the active defaults. Its older text-only capability and always-on reasoning behavior do not describe Flash or M3.

## Token Pricing

Prices are in USD per million tokens and are estimates only. Confirm the provider's current pricing before quoting a bill.

| Model | Input | Output | Cache read | Cache write |
| --- | ---: | ---: | ---: | ---: |
| `MiniMax-M3.1-Flash` | $0.15 | $0.60 | $0.015 | Not specified |
| `MiniMax-M3` standard requests | $0.30 | $1.20 | $0.06 | Not specified |
| `MiniMax-M3` larger requests | $0.60 | $2.40 | $0.12 | Not specified |

Apply the M3 rate supplied by the provider for the request size; do not invent a boundary from an application-side token count. The current Flash cache-write price is not specified in the verified contract.

## Validation Checklist

- Ordinary choices list `MiniMax-M3.1-Flash` first and `MiniMax-M3` second.
- Explicit saved, custom, and legacy model IDs remain usable, including `MiniMax-M2.7`.
- The endpoint belongs to the selected region and protocol.
- Flash reasoning uses only adaptive effort values; `none`, `service_tier`, and `reasoning_split` are not sent.
- M3 multimodal inputs remain available and its effort behavior is preserved.
- Responses tool and reasoning history is retained client-side.
- The API key remains in `MINIMAX_API_KEY` and is never included in output.
