# Prompting Claude Fable 5

## Overview

This guide covers prompting and scaffolding patterns specific to **Claude Fable 5** and **Claude Mythos 5**. Claude Fable 5 targets problems previously too complex, long-running, or ambiguous for prior models — especially effective at end-to-end work taking hours, days, or weeks.

## Key Behavioral Differences from Claude Opus 4.8

### Capability Improvements
- **Long-horizon autonomy** — sustains productive output over multi-day, goal-directed runs
- **First-shot correctness** — single-pass implementations of systems that previously took days of iteration
- **Vision** — interprets dense technical images, web apps, and detailed screenshots with higher accuracy
- **Enterprise workflows** — professional-grade output on financial analysis, spreadsheets, slides, documents
- **Code review & debugging** — noticeably higher bug-finding recall
- **Navigating ambiguity** — performs well on complex, multi-threaded requests
- **Delegation & collaboration** — significantly more dependable at dispatching parallel subagents

### Safety Classifiers

Claude Fable 5 runs safety classifiers targeting:
- Offensive cybersecurity techniques (exploits, malware, attack tooling)
- Biology and life sciences content (lab methods, molecular mechanisms)
- Extraction of the model's summarized thinking

## Prompting Patterns

### 1. Longer Turns by Default
Individual requests on hard tasks can run **many minutes** at higher effort settings; autonomous runs can extend **hours**.

### 2. Consider All Effort Levels
- Use **`high`** as default for most tasks
- Use **`xhigh`** for most capability-sensitive workloads
- Use **`medium`** or **`low`** for routine work

### 3. Strong Instruction Following
Brief instructions now steer most behaviors effectively — no need to enumerate each behavior by name.

### 4. Ground Progress Claims During Long Runs
Instruct Claude Fable 5 to audit progress against actual tool results. In Anthropic's testing, this **nearly eliminated fabricated status reports**.
