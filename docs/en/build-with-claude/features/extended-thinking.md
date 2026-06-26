# Extended Thinking

## Overview

Extended thinking gives Claude enhanced reasoning capabilities by generating internal `thinking` content blocks before delivering a final response. Available on all current Claude models, though the method of enabling it varies.

## Model Support Matrix

| Model | Manual (`budget_tokens`) | Recommended Approach |
|---|---|---|
| Claude Fable 5 / Mythos 5 | Not supported | Adaptive thinking (always on) |
| Claude Opus 4.8 / 4.7 | Not supported | Adaptive thinking with `effort` |
| Claude Opus 4.6 / Sonnet 4.6 | Deprecated | Adaptive thinking with `effort` |
| Claude Opus 4.5 / Haiku 4.5 | Supported | N/A |

## How It Works

```json
{
  "content": [
    {"type": "thinking", "thinking": "Let me analyze...", "signature": "..."},
    {"type": "text", "text": "Based on my analysis..."}
  ]
}
```

## Enabling Extended Thinking

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=16000,
    thinking={"type": "enabled", "budget_tokens": 10000},
    messages=[{"role": "user", "content": "..."}],
)
```

### Key Parameters
- **`type`**: `"enabled"` or `"adaptive"`
- **`budget_tokens`**: Maximum tokens for internal reasoning (minimum 1,024)
- **`display`**: `"summarized"` or `"omitted"`

## Adaptive Thinking (Claude Opus 4.8+)

- Thinking is OFF by default
- Control depth via `effort` parameter
- At `max`/`xhigh` effort, set large max output (start at 64k tokens)

## Streaming Thinking

```python
with client.messages.stream(
    model="claude-sonnet-4-6",
    max_tokens=16000,
    thinking={"type": "enabled", "budget_tokens": 10000},
    messages=[...],
) as stream:
    for event in stream:
        if event.type == "thinking_delta":
            print(event.thinking_delta, end="")
```
