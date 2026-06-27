---
title: "Reducing latency"
url: "https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-latency"
lang: "en"
---

# Reducing latency

**Latency**: The time taken for a model to process a prompt and generate output, influenced by model size, prompt complexity, and infrastructure.

> "It's always better to first engineer a prompt that works well without model or prompt constraints, and then try latency reduction strategies afterward."

## How to Measure Latency

| Metric | Description |
|--------|-------------|
| **Baseline latency** | Time to process prompt and generate response |
| **Time to First Token (TTFT)** | Time from prompt sent to first token generated |

## Strategies to Reduce Latency

### 1. Choose the Right Model
For speed-critical applications: **Claude Haiku 4.5** (fastest response times with high intelligence)

### 2. Optimize Prompt and Output Length
- Be clear but concise
- Ask for shorter responses
- Use `max_tokens` parameter as a hard cap
- Experiment with temperature

### 3. Leverage Streaming
Model begins sending response before completion — significantly improves perceived responsiveness.
