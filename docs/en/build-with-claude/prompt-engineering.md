# Prompt Engineering

## Overview
All prompting techniques — clarity, examples, XML structuring, role prompting, thinking, prompt chaining.

## Best Practices

### Be Clear and Direct
Think of Claude as a "brilliant but new employee" lacking context on your norms.

### Add Context/Motivation
Explaining *why* helps Claude generalize.

### Use Examples Effectively (Few-Shot)
- Include 3–5 examples
- Relevant, diverse, structured in `<example>` tags

### Structure Prompts with XML Tags
Use consistent, descriptive tag names with nesting.

### Give Claude a Role
A single sentence in system prompt focuses behavior and tone.

### Long Context Prompting (20k+ tokens)
- Put longform data at the TOP
- Structure documents with XML tags
- Ground responses in quotes

## Model-Specific Guides

### Claude Fable 5
- Longer turns by default
- Strong instruction following
- Ground progress claims during long runs
- Parallel subagents

### Claude Opus 4.8
- Calibrates response length based on task complexity
- Effort is more important than any prior Opus model
- Literal instruction following
- Direct, opinionated tone

## Console Prompting Tools
1. Prompt Generator — solves "blank page problem"
2. Templates & Variables — `{{variable_name}}` syntax
3. Prompt Improver — automated 4-step enhancement
