---
title: "Define success criteria and build evaluations"
url: "https://platform.claude.com/docs/en/test-and-evaluate/develop-tests"
lang: "en"
---

# Define success criteria and build evaluations

Building successful LLM-based applications requires clearly defining **success criteria** first, then designing **evaluations** to measure performance against them.

## Defining Success Criteria

Good success criteria are **SMART**: Specific, Measurable, Achievable, Relevant.

**Bad:** *"The model should classify sentiments well"*

**Good:** *"Our sentiment analysis model should achieve an F1 score of at least 0.85 on a held-out test set of 10,000 diverse Twitter posts."*

## Common Success Criteria

- **Task fidelity** — How well the model performs on the task, including edge cases
- **Consistency** — Similar responses for similar inputs
- **Relevance and coherence** — Directly addresses user questions logically
- **Tone and style** — Output style matches audience expectations
- **Privacy preservation** — Handles sensitive information correctly
- **Context utilization** — Effectively uses provided context/history
- **Latency** — Acceptable response time
- **Price** — Budget constraints for running the model

## Building Evaluations

### Grading Methods (Ranked by Preference)

| Method | Speed | Reliability | Nuance |
|--------|-------|-------------|--------|
| **Code-based** (exact match, string match) | Fastest | Highest | Low |
| **LLM-based** | Fast | High | Medium-High |
| **Human grading** | Slowest | Variable | Highest |

### Tips for LLM-Based Grading
- Use detailed, clear rubrics
- Be empirical or specific — output 'correct'/'incorrect' or 1-5 scale
- Encourage reasoning — ask the LLM to think first, then discard reasoning
- Use a different model for grading than the one being evaluated
