# Stop Reasons and Fallback

Claude API responses include `stop_reason` field indicating why the model stopped generating.

## Stop Reasons
| Value | Meaning |
|---|---|
| `end_turn` | Natural completion |
| `max_tokens` | Hit token limit |
| `stop_sequence` | Hit stop sequence |
| `tool_use` | Tool call requested |
| `refusal` | Safety classifier declined |
| `pause_turn` | Server tool iteration limit hit |

# Fallback Credit

Avoid double prompt-cache costs when retrying refused requests.

## How It Works
1. Opt in with `anthropic-beta: fallback-credit-2026-06-01`
2. Read `fallback_credit_token` from refusal's `stop_details`
3. Echo token on retry
4. Retry billed as if conversation was always on new model
