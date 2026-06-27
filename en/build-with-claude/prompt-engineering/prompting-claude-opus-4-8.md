---
title: "Prompting Claude Opus 4.8"
url: "https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-4-8"
lang: "en"
---

# Prompting Claude Opus 4.8

Claude Opus 4.8 excels at long-horizon agentic work, knowledge work, vision, and memory tasks. It works well out of the box with existing Claude Opus 4.7 prompts. This guide covers behaviors that most often require tuning.

> For API parameter changes when migrating from Opus 4.7 (sampling parameters, effort default, 1M context window, mid-conversation system messages, refusal stop details), see the migration guide.

## 1. Response Length & Verbosity

- Opus 4.8 calibrates response length to perceived task complexity — shorter on simple lookups, longer on open-ended analysis.
- To decrease verbosity: "Provide concise, focused responses. Skip non-essential context, and keep examples minimal."
- Positive examples (showing desired concision) are more effective than negative instructions ("don't do X").

## 2. Effort Calibration & Thinking Depth

The effort parameter trades intelligence for speed/cost. Opus 4.8 respects effort levels strictly, especially at the low end.

| Effort Level | Use Case |
|---|---|
| max | Intelligence-demanding tasks; diminishing returns possible; risk of overthinking |
| xhigh | Best for most coding and agentic use cases (recommended starting point) |
| high | Minimum for most intelligence-sensitive use cases |
| medium | Cost-sensitive; trades off intelligence |
| low | Short, scoped, latency-sensitive tasks; risk of under-thinking on moderate tasks |

### Key Guidance
- Under-thinking on complex problems? Raise effort to high/xhigh rather than prompting around it.
- Need to keep effort at low for latency? Add: "This task involves multi-step reasoning. Think carefully through the problem before responding."
- Thinking is OFF by default unless you set thinking: {type: "adaptive"}. To reduce excessive thinking: "Thinking adds latency and should only be used when it will meaningfully improve answer quality — typically for problems that require multi-step reasoning. When in doubt, respond directly."
- At max or xhigh effort, set large max output tokens (start at 64k tokens)
- Effort is more important for Opus 4.8 than any prior Opus model — experiment actively when upgrading.

## 3. Tool Use Triggering

- Opus 4.8 favors reasoning over tool calls by default (usually better results).
- To increase tool usage: raise effort to high/xhigh, or explicitly instruct the model about when/how to use tools.

## 4. User-Facing Progress Updates

- Opus 4.8 provides more regular, higher-quality updates during long agentic traces.
- Try removing scaffolding that forces interim status messages.
- If update length/content is miscalibrated, explicitly describe desired format and provide examples.

## 5. Literal Instruction Following

- Opus 4.8 interprets prompts literally and explicitly, especially at lower effort levels.
- It does not silently generalize instructions or infer unrequested actions.
- Upside: Precision, less thrash, better for structured extraction and pipelines.
- If broad application is needed, state scope explicitly: "Apply this formatting to every section, not just the first one"

## 6. Tone & Writing Style

- Opus 4.8 tends toward a direct, opinionated style with minimal validation-forward phrasing and sparing emoji use.
- If a warmer/conversational voice is needed: "Use a warm, collaborative tone. Acknowledge the user's framing before answering."
- Re-evaluate style prompts against the new baseline if migrating.

## 7. Controlling Subagent Spawning

- Opus 4.8 spawns fewer subagents by default.
- Behavior is steerable via prompting:
  - "Do not spawn a subagent for work you can complete directly in a single response (e.g. refactoring a function you can already see)."
  - "Spawn multiple subagents in the same turn when fanning out across items or reading multiple files."

## 8. Design & Frontend Defaults

- Opus 4.8 has a persistent default house style: warm cream/off-white backgrounds (~#F4F1EA), serif display type (Georgia, Fraunces, Playfair), italic word-accents, terracotta/amber accent.
- This reads well for editorial/hospitality/portfolio but feels wrong for dashboards, dev tools, fintech, healthcare, or enterprise apps.
- Generic instructions ("don't use cream") just shift to a different fixed palette.

### Two Reliable Approaches

**Approach 1: Specify a concrete alternative** — the model follows explicit specs precisely (specific hex colors, typography, layout, and component specs).

**Approach 2: Have the model propose options before building** — broad creative instructions work well when the model proposes first.
