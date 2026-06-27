---
title: "Reduce prompt leak"
url: "https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-prompt-leak"
lang: "en"
---

# Reduce prompt leak

Prompt leaks can expose sensitive information that you expect to be "hidden" in your prompt.

## Strategies to reduce prompt leak

- **Separate context from queries** — Use system prompts to isolate key information. Emphasize key instructions in the `User` turn, then reemphasize by prefilling the `Assistant` turn.
- **Use post-processing** — Filter Claude's outputs for keywords that might indicate a leak.
- **Avoid unnecessary proprietary details** — If Claude doesn't need it to perform the task, don't include it.
- **Regular audits** — Periodically review prompts and outputs for potential leaks.
