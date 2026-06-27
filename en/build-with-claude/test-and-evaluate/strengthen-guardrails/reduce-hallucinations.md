---
title: "Reduce hallucinations"
url: "https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations"
lang: "en"
---

# Reduce hallucinations

Even the most advanced language models, like Claude, can sometimes generate text that is factually incorrect or inconsistent with the given context.

## Basic hallucination minimization strategies

- **Allow Claude to say "I don't know"** — Explicitly give Claude permission to admit uncertainty.
- **Use direct quotes for factual grounding** — For tasks involving long documents (>20k tokens), ask Claude to extract word-for-word quotes first.
- **Verify with citations** — Make Claude's response auditable by having it cite quotes and sources for each claim.

## Advanced techniques

- **Chain-of-thought verification** — Ask Claude to explain its reasoning before giving a final answer
- **Best-of-N verification** — Run Claude through the same prompt multiple times and compare outputs
- **Iterative refinement** — Use Claude's outputs as inputs for follow-up prompts
- **External knowledge restriction** — Instruct Claude to only use information from provided documents
