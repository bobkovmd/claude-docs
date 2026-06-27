---
title: "Glossary"
url: "https://platform.claude.com/docs/en/about-claude/glossary"
lang: "en"
---

# Glossary

These concepts are not unique to Anthropic's language models, but we present a brief summary of key terms below.

## Context Window
The model's "working memory" — amount of text it can reference when generating new text. Distinct from training data corpus. Larger windows → better handling of complex/lengthy prompts and extended conversations.

## Fine-tuning
Further training a pretrained model on additional data to mimic new patterns/characteristics. Claude is already fine-tuned to be a helpful assistant. API does not currently offer fine-tuning; contact Anthropic to explore.

## HHH Framework
Anthropic's three core goals for Claude:
- **Helpful** — Performs tasks/answers questions to best of ability with relevant, useful info
- **Honest** — Gives accurate info, no hallucination/confabulation, acknowledges limitations
- **Harmless** — Not offensive/discriminatory; politely refuses dangerous/unethical requests with explanation

## Latency
Time between submitting a prompt and receiving generated output. Lower latency = faster response times. Critical for real-time apps, chatbots, interactive experiences.

## LLM (Large Language Model)
AI language models with many parameters trained on vast text data. Capable of: text generation, Q&A, summarization, and more. Claude = conversational LLM fine-tuned with RLHF to be helpful, honest, and harmless.

## MCP (Model Context Protocol)
Open protocol standardizing how applications provide context to LLMs. "Like a USB-C port for AI applications" — unified connection to data sources and tools.

## MCP Connector
Allows API users to connect to MCP servers directly from Messages API without building an MCP client. Supports tool calling. Currently in beta.

## Pretraining
Initial training on large unlabeled text corpus. Claude uses autoregressive language models — trained to predict next word given prior context.

## RAG (Retrieval Augmented Generation)
Combines information retrieval with language model generation. Augments model with external knowledge base passed into context window. Data retrieved at runtime when query is sent.

## RLHF (Reinforcement Learning from Human Feedback)
Trains pretrained models to align with human preferences. Humans rank example texts; RL encourages model to prefer higher-ranked output styles.

## Temperature
Controls randomness of model predictions during text generation. Higher → more creative, diverse outputs. Lower → more conservative, deterministic.

## TTFT (Time to First Token)
Measures time to generate the first token after receiving a prompt. Key indicator of model responsiveness. Critical for interactive apps, chatbots, real-time systems.

## Tokens
Smallest individual units of a language model (words, subwords, characters, or bytes). Claude: ~1 token ≈ 3.5 English characters.
