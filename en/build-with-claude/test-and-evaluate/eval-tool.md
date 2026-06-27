---
title: "Using the Evaluation tool"
url: "https://platform.claude.com/docs/en/test-and-evaluate/eval-tool"
lang: "en"
---

# Using the Evaluation tool

The Claude Console features an **Evaluation tool** that allows you to test your prompts under various scenarios.

## Accessing the Evaluate Feature

1. Open the Claude Console and navigate to the prompt editor.
2. After composing your prompt, look for the 'Evaluate' tab at the top of the screen.

> Ensure your prompt includes at least 1-2 dynamic variables using the double brace syntax: `{{variable}}`. This is required for creating eval test sets.

## Creating Test Cases

1. Click the '+ Add Row' button at the bottom left to manually add a case.
2. Use the 'Generate Test Case' feature to have Claude automatically generate test cases.
3. Import test cases from a CSV file.

## Tips for Effective Evaluation

- Structure your prompts with clear input and output formats
- Use the 'Generate a prompt' helper tool to create prompts with appropriate variable syntax
- Compare outputs of two or more prompts side-by-side
- Grade response quality on a 5-point scale
- Create new versions of your prompt and re-run the test suite
