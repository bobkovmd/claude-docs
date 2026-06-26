# Prompting Best Practices

## Overview

Comprehensive prompt engineering reference for Claude's latest models: **Claude Fable 5, Claude Mythos 5, Claude Opus 4.8, Claude Opus 4.7, Claude Opus 4.6, Claude Sonnet 4.6, and Claude Haiku 4.5**.

Organized in three parts:
1. **Model-specific guidance** (Fable 5, Opus 4.8)
2. **Techniques for all current models** (general principles, output/formatting, tool use, thinking, agentic systems)
3. **Migration considerations** from earlier model generations

---

## General Principles (All Models)

### Be Clear and Direct

Think of Claude as a brilliant but new employee who lacks context on your norms and workflows.

**Golden rule:** Show your prompt to a colleague with minimal context. If they'd be confused, Claude will be too.

- Specify desired output format and constraints
- Use numbered lists/bullet points when step order matters
- Explicitly request "above and beyond" behavior — don't rely on inference

### Add Context/Motivation

Explaining *why* helps Claude generalize.

### Use Examples Effectively (Few-Shot)

- **Relevant**: Mirror actual use case
- **Diverse**: Cover edge cases
- **Structured**: Wrap in `<example>` / `<examples>` tags
- **Include 3–5 examples** for best results

### Structure Prompts with XML Tags

Use consistent, descriptive tag names. Nest for hierarchy.

### Give Claude a Role

A single sentence in the system prompt focuses behavior and tone.

### Long Context Prompting (20k+ tokens)

- **Put longform data at the top** — above query, instructions, and examples
- **Structure documents with XML tags**
- **Ground responses in quotes**

---

## Output and Formatting

### Communication Style Changes

Latest models are more **direct, grounded, conversational, and less verbose**.

### Control Response Format

1. **Tell Claude what TO do, not what NOT to do**
2. **Use XML format indicators**
3. **Match prompt style to desired output**
4. **Detailed formatting instructions**

### Migrating Away from Prefilled Responses

Prefilled assistant messages on the last assistant turn are **no longer supported** starting Claude 4.6 (returns 400 error).

---

## Tool Use

### Explicit Action Directing
- ❌ "Can you suggest some changes?" (Claude will only suggest)
- ✅ "Change this function to improve its performance." (Claude will act)

### Tool Choice

Claude generally prefers to reason before using tools.

---

## Thinking

### Extended Thinking

Enable with `thinking: {type: "enabled", budget_tokens: N}`

### Adaptive Thinking (Claude Opus 4.8+)

- Thinking is OFF by default — set `thinking: {type: "adaptive"}`
- Control depth via `effort` parameter
- At `max`/`xhigh` effort, set large max output token budget (start at **64k tokens**)

---

## Agentic Systems

- Define clear goals and success criteria
- Specify available tools and constraints
- Use structured output formats
- Implement error handling and fallback strategies
- Test with diverse inputs
