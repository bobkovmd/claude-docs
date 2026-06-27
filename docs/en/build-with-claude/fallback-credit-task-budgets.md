# Fallback Credit

Avoid paying prompt-cache costs twice when retrying refused requests on another model.

## Flow
1. Opt in with `anthropic-beta: fallback-credit-2026-06-01`
2. Read `fallback_credit_token` and `fallback_has_prefill_claim` from refusal
3. Build retry with token
4. Send retry with same beta header

## Constraint
Token cannot be redeemed from Message Batches results — only direct Messages API.

# Task Budgets (Beta)

Advisory token budget for the full agentic loop.

## Usage
```json
"output_config": {
    "effort": "high",
    "task_budget": {"type": "tokens", "total": 64000}
}
```

## Key Points
- Advisory, not enforced (soft hint)
- Minimum: 20,000 tokens
- Complements effort parameter
- Visible only to model (not in API response)

# Fast Mode (Research Preview)

Up to 2.5x higher output tokens per second at premium pricing.

## Pricing
- Input: $10/MTok
- Output: $50/MTop

## Requirements
- Research preview access
- Claude Opus 4.8 only
- Beta header: `fast-mode-2026-02-01`
