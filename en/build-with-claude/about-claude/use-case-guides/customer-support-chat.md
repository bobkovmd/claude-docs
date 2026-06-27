---
title: "Customer support agent"
url: "https://platform.claude.com/docs/en/about-claude/use-case-guides/customer-support-chat"
lang: "en"
---

# Customer Support Agent Guide

Build a customer support chatbot with Claude that answers product questions, stays on topic, and generates quotes through tool use.

## When to Use Claude for Support

- High volume of repetitive queries
- Need for quick information synthesis across large knowledge bases
- 24/7 availability requirement
- Rapid scaling during peak periods
- Consistent brand voice at scale

## Pre-Build Planning

1. Define Your Ideal Chat Interaction — Map out every step of the customer journey
2. Break Into Unique Tasks — Example: greeting, product info, conversation management, quote generation
3. Establish Success Criteria:
   - Query comprehension accuracy ≥ 95%
   - Response relevance ≥ 90%
   - Response accuracy (intro info) 100%
   - Topic adherence 95% of responses
   - Escalation accuracy ≥ 95%
   - CSAT score ≥ 4/5

## Implementation

### Model Selection
- Claude Opus 4.8 — recommended balance of intelligence, latency, and cost
- Claude Haiku 4.5 — better for multi-step flows with RAG, tool use, or long-context prompts

### Building the Prompt
Claude works best with the bulk of prompt content written inside the first User turn (not the system prompt), with the only exception being role prompting.

### Tool Use
Tools signal intent to the application; they don't perform the actual calculation themselves. Example: `get_quote` tool for insurance premium calculation.

### ChatBot Class
Two core methods:
- `generate_message(messages, max_tokens)` — calls `anthropic.messages.create()`
- Tool execution and conversation management
