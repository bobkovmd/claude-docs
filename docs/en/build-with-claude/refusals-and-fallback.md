# Refusals and Fallback

## Overview
Claude Fable 5 includes safety classifiers that decline certain requests. Refusals return as HTTP 200 with `stop_reason: "refusal"`.

## Refusal Response
```json
{
  "stop_reason": "refusal",
  "stop_details": {
    "type": "refusal",
    "category": "cyber",
    "explanation": "This request was declined because it could enable cyber harm."
  }
}
```

### Refusal Categories
| `category` | Meaning |
|---|---|
| `"cyber"` | Could enable cyber harm |
| `"bio"` | Could enable biological harm |
| `"frontier_llm"` | Could assist development of competing AI models |
| `"reasoning_extraction"` | Asks model to reproduce internal reasoning |

## Fallback Approaches
1. **Server-side fallback** — One request, API handles retry
2. **SDK middleware** — Configure once on client
3. **Manual retry with fallback credit** — Full control

## Server-Side Fallback
- Beta header: `anthropic-beta: server-side-fallback-2026-06-01`
- Up to 3 fallback models
- Only safety classifier declines trigger fallback

## Fallback Credit
Avoid paying prompt-cache costs twice when retrying a refused request on another model.
