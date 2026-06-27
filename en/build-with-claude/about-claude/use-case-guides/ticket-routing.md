---
title: "Ticket routing"
url: "https://platform.claude.com/docs/en/about-claude/use-case-guides/ticket-routing"
lang: "en"
---

# Ticket routing with Claude

This guide explains how to use Claude's natural language understanding to classify customer support tickets at scale based on intent, urgency, prioritization, customer profile, and more.

## When to Use Claude vs. Traditional ML

Claude is preferable over traditional ML when:
- Limited labeled training data — Claude needs only a few dozen examples
- Evolving categories — Adapts to changing class definitions without extensive relabeling
- Complex, unstructured text — No extensive feature engineering required
- Semantic understanding needed
- Interpretable reasoning required
- Edge cases/ambiguity
- Multilingual support

## Building Your LLM Support Workflow

1. Understand current support approach
2. Define user intent categories
3. Establish success criteria (90-95% routing accuracy, <5 min time-to-assignment)
4. Choose the right model (Haiku 4.5 for cost-efficiency, Opus 4.8 for accuracy)
5. Build a strong classification prompt
6. Evaluate with precision/recall metrics
