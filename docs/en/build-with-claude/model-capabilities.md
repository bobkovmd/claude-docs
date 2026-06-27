# Model Capabilities

## Extended Thinking
Enhanced reasoning with transparent step-by-step thought process.

### How It Works
API response includes `thinking` content blocks followed by `text` content blocks.

### Key Parameters
- `budget_tokens`: Maximum tokens for internal reasoning (min 1,024)
- `display`: "summarized" or "omitted"

### Streaming Thinking
Stream via SSE. Thinking content arrives via `thinking_delta` events.

## Adaptive Thinking (Claude Opus 4.8+)
- Thinking is OFF by default
- Control depth via `effort` parameter
- At `max`/`xhigh` effort, set large max output (start at 64k tokens)

### Effort Levels
| Level | Best For |
|---|---|
| `max` | Intelligence-demanding tasks |
| `xhigh` | Best for coding and agentic use cases |
| `high` | Minimum for intelligence-sensitive use cases |
| `medium` | Cost-sensitive |
| `low` | Latency-sensitive |

## Task Budgets (Beta)
Advisory token budget for the full agentic loop. Self-regulate token spend.

## Fast Mode (Research Preview)
Up to 2.5x higher output tokens per second. $50/MTok output for Opus 4.8.

## Structured Outputs
Guaranteed schema conformance via JSON outputs or strict tool use.

## Citations
Ground responses in source documents with exact sentence/passage references.

## Streaming Messages
Incremental responses via SSE. Supports text, tool use, and thinking deltas.

## Batch Processing
Async processing at 50% cost reduction. Up to 100K requests per batch.

## Search Results
Natural citations for RAG with source attribution.

## Multilingual Support
Strong cross-lingual performance relative to English (100% baseline).

## Embeddings
Anthropic does not offer its own embedding model. Voyage AI is recommended.
