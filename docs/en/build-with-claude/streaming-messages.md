# Streaming Messages

Set `stream: true` to receive incremental responses via SSE.

## Event Types
- `message_start` — Contains Message object with empty content
- `content_block_start` — New content block begins
- `content_block_delta` — Content delta (text_delta, input_json_delta, thinking_delta, signature_delta)
- `content_block_stop` — Content block ends
- `message_delta` — Top-level changes (stop_reason)
- `message_stop` — Stream terminator

## Delta Types
- `text_delta` — Text content
- `input_json_delta` — Partial JSON for tool use
- `thinking_delta` — Extended thinking content
- `signature_delta` — Integrity verification for thinking blocks

## Streaming Refusals
Starting Claude 4, streaming responses return `stop_reason: "refusal"` when classifiers detect violations. Reset context after refusal.
