# Prompting Claude Opus 4.8

## Overview

Claude Opus 4.8 excels at long-horizon agentic work, knowledge work, vision, and memory tasks. It performs well out-of-the-box on existing Claude Opus 4.7 prompts, but several behavioral patterns may require tuning.

## Response Length & Verbosity

- **Calibrates response length based on perceived task complexity** — shorter answers for simple lookups, longer for open-ended analysis
- If your product requires specific verbosity, tune prompts explicitly

## Calibrating Effort & Thinking Depth

The `effort` parameter trades intelligence for speed/cost. **Effort is more important for Opus 4.8 than any prior Opus model.**

| Effort Level | Best For |
|--------------|----------|
| **`max`** | Intelligence-demanding tasks; may show diminishing returns or overthinking |
| **`xhigh`** | **Best for most coding and agentic use cases** |
| **`high`** | Minimum for intelligence-sensitive use cases; balances tokens and intelligence |
| **`medium`** | Cost-sensitive use cases; trades off intelligence |
| **`low`** | Short, scoped, latency-sensitive tasks not requiring deep reasoning |

### Key Behaviors
- **Respects effort levels strictly** — at `low`/`medium`, scopes work to exactly what was asked
- Risk of **under-thinking** on moderately complex tasks at `low` effort
- If shallow reasoning observed, **raise effort to `high`/`xhigh`** rather than prompting around it

## Adaptive Thinking

- **Thinking is OFF by default** — must explicitly set `thinking: {type: "adaptive"}`
- At `max` or `xhigh` effort, set a large max output token budget (start at **64k tokens**)

## Tool Use Triggering

- **Favors reasoning over tool calls** — produces better results in most cases
- **Increasing effort increases tool usage** — `high`/`xhigh` show substantially more tool use

## Literal Instruction Following

- **Interprets prompts literally and explicitly**, especially at lower effort levels
- Does **not** silently generalize instructions or infer unstated requests
- **Upside:** Precision and less thrash; better for API use cases, structured extraction

## Tone & Writing Style

- Tends toward **direct, opinionated style** with minimal validation-forward phrasing
- If your product voice differs, re-evaluate style prompts

## Controlling Subagent Spawning

- **Spawns fewer subagents by default** — behavior is steerable through prompting

## Design & Frontend Defaults

### Default House Style
- **Backgrounds:** Warm cream/off-white (~`#F4F1EA`)
- **Display type:** Serif (Georgia, Fraunces, Playfair)
- **Accents:** Italic word-accents, terracotta/amber

> ⚠️ This reads well for editorial/hospitality/portfolio but feels off for dashboards, dev tools, fintech, healthcare, or enterprise apps.
