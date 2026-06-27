---
title: "Mitigate jailbreaks and prompt injections"
url: "https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks"
lang: "en"
---

# Mitigate jailbreaks and prompt injections

Jailbreaking and prompt injection attempt to make Claude ignore its guidelines or your instructions. These attacks fall into two distinct threat models:

| Threat Model | Description |
|---|---|
| **Jailbreaks & Direct Prompt Injection** | The *user* is the adversary, crafting inputs to bypass guardrails |
| **Indirect Prompt Injection** | User is trusted, but Claude processes *third-party content* containing adversarial instructions |

## Jailbreaks & Direct Prompt Injection — Mitigations

1. **Harmlessness Screens** — Use a lightweight model (e.g., Claude Haiku 4.5) to pre-screen user input
2. **Input Validation** — Filter user input for known injection patterns
3. **Prompt Engineering** — Craft system prompts that emphasize ethical and legal boundaries
4. **Respond to Repeat Offenders** — Throttle or ban users who repeatedly attempt circumvention

## Indirect Prompt Injection — Mitigations

1. **Put Untrusted Content Only in Tool Results** — Deliver third-party content inside `tool_result` blocks
2. **Tell Claude What the Content Is** — Make nature and source explicit
3. **State the Policy in Your System Prompt** — Tell Claude that tool content is untrusted
4. **JSON-Encode Untrusted Content** — Wrap third-party strings in JSON object
5. **Don't Put Your Own Instructions in Tool Results**
6. **Limit Claude's Access (Principle of Least Privilege)**
7. **Screen Tool Outputs Before Claude Acts on Them**
8. **Red-Team Your Own Agent**
