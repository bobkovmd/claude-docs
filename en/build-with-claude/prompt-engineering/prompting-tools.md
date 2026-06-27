---
title: "Console prompting tools"
url: "https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-tools"
lang: "en"
---

# Console prompting tools

The Claude Console provides three core tools for building and refining prompts, used in this order:

1. **Prompt generator** → Create a first draft
2. **Prompt templates & variables** → Add reusable structure
3. **Prompt improver** → Enhance existing prompts

## 1. Prompt Generator

- **Purpose:** Guides Claude to create high-quality prompt templates tailored to your task, following Anthropic's best practices.
- **Best for:** Solving the "blank page problem" — gives you a starting point for further testing.
- **Compatibility:** All Claude models, including extended thinking models.
- **Try it:** Directly on the Console dashboard.

## 2. Prompt Templates & Variables

Placeholders use **`{{double brackets}}`** syntax in the Console.

- **Fixed content** — Static instructions/context constant across interactions
- **Variable content** — Dynamic elements (user inputs, RAG content, conversation history, tool results)

## 3. Prompt Improver

Automated analysis and enhancement of prompts. Excels at making prompts more robust for complex, high-accuracy tasks.

### How It Works (4 Steps)
1. Example identification — Locates and extracts examples from your template
2. Initial draft — Creates a structured template with clear sections and XML tags
3. Chain of thought refinement — Adds/refines detailed reasoning instructions
4. Example enhancement — Updates examples to demonstrate the new reasoning process
