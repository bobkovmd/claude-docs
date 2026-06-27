# Prompting Best Practices

**Source:** [Claude Platform Docs](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)

Comprehensive guide to prompt engineering techniques for Claude's latest models, covering clarity, examples, XML structuring, thinking, and agentic systems.

This is the reference for prompt engineering with Claude's latest models, including Claude Fable 5, Claude Mythos 5, Claude Opus 4.8, Claude Opus 4.7, Claude Opus 4.6, Claude Sonnet 4.6, and Claude Haiku 4.5.

---

## Claude Fable 5

Prompting guidance for Claude Fable 5 and Claude Mythos 5 has its own page: [Prompting Claude Fable 5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5). It covers the behavioral differences from Claude Opus 4.8 and the prompt and scaffolding changes worth making, including effort levels, instruction following, long-run progress claims, memory systems, and the `reasoning_extraction` refusal category.

## Prompting Claude Opus 4.8

Prompting guidance for Claude Opus 4.8 has its own page: [Prompting Claude Opus 4.8](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-4-8). It covers response length, effort and thinking-depth calibration, tool use triggering, literal instruction following, subagent control, and design and frontend defaults.

---

## General Principles

The techniques in this section and the sections that follow apply to all current Claude models, including Claude Fable 5 and Claude Mythos 5.

### Be Clear and Direct

Claude responds well to clear, explicit instructions. Being specific about your desired output can help enhance results. If you want "above and beyond" behavior, explicitly request it rather than relying on the model to infer this from vague prompts.

Think of Claude as a brilliant but new employee who lacks context on your norms and workflows. The more precisely you explain what you want, the better the result.

**Golden rule:** Show your prompt to a colleague with minimal context on the task and ask them to follow it. If they'd be confused, Claude will be too.

- Be specific about the desired output format and constraints.
- Provide instructions as sequential steps using numbered lists or bullet points when the order or completeness of steps matters.

### Add Context to Improve Performance

Providing context or motivation behind your instructions, such as explaining to Claude why such behavior is important, can help Claude better understand your goals and deliver more targeted responses.

**Example:**
- ❌ `NEVER use ellipses`
- ✅ `Your response will be read aloud by a text-to-speech engine, so never use ellipses since the text-to-speech engine will not know how to pronounce them.`

### Use Examples Effectively

Examples are one of the most reliable ways to steer Claude's output format, tone, and structure. A few well-crafted examples (known as few-shot or multishot prompting) improve accuracy and consistency.

When adding examples, make them:
- **Relevant:** Mirror your actual use case closely.
- **Diverse:** Cover edge cases and vary enough that Claude doesn't pick up unintended patterns.
- **Structured:** Wrap examples in `<example>` tags (multiple examples in `<examples>` tags) so Claude can distinguish them from instructions.

Include **3–5 examples** for best results. You can also ask Claude to evaluate your examples for relevance and diversity, or to generate additional ones based on your initial set.

### Structure Prompts with XML Tags

XML tags help Claude parse complex prompts unambiguously, especially when your prompt mixes instructions, context, examples, and variable inputs. Wrapping each type of content in its own tag (e.g. `<instructions>`, `<context>`, `<input>`) reduces misinterpretation.

Best practices:
- Use consistent, descriptive tag names across your prompts.
- Nest tags when content has a natural hierarchy (documents inside `<documents>`, each inside `<document index="n">`).

### Give Claude a Role

Setting a role in the system prompt focuses Claude's behavior and tone for your use case. Even a single sentence makes a difference:

```json
{
  "system": "You are a helpful coding assistant specializing in Python."
}
```

### Long Context Prompting

When working with large documents or data-rich inputs (20k+ tokens), structure your prompt carefully to get the best results:

- **Put longform data at the top:** Place your long documents and inputs near the top of your prompt, above your query, instructions, and examples. Queries at the end can improve response quality by up to 30% in tests.
- **Structure document content and metadata with XML tags:** When using multiple documents, wrap each document in `<document>` tags with `<document_content>` and `<source>` (and other metadata) subtags for clarity.
- **Ground responses in quotes:** For long document tasks, ask Claude to quote relevant parts of the documents first before carrying out its task. This helps Claude cut through the noise.

### Model Self-Knowledge

```
The assistant is Claude, created by Anthropic. The current model is Claude Opus 4.8.
```

```
When an LLM is needed, please default to Claude Opus 4.8 unless the user requests otherwise.
The exact model string for Claude Opus 4.8 is claude-opus-4-8.
```

---

## Output and Formatting

### Communication Style and Verbosity

Claude's latest models have a more concise and natural communication style compared to previous models:

- **More direct and grounded:** Provides fact-based progress reports rather than self-celebratory updates
- **More conversational:** Slightly more fluent and colloquial, less machine-like
- **Less verbose:** May skip detailed summaries for efficiency unless prompted otherwise

If you prefer more visibility into its reasoning:

```
After completing a task that involves tool use, provide a quick summary of the work you've done.
```

### Control the Format of Responses

1. **Tell Claude what TO do, not what NOT to do**
   - Instead of: "Do not use markdown in your response"
   - Try: "Your response should be composed of smoothly flowing prose paragraphs."

2. **Use XML format indicators**
   - Try: "Write the prose sections of your response in <smoothly_flowing_prose_paragraphs> tags."

3. **Match your prompt style to the desired output**
   - Removing markdown from your prompt can reduce the volume of markdown in the output.

4. **Use detailed prompts for specific formatting preferences**

```xml
<avoid_excessive_markdown_and_bullet_points>
When writing reports, documents, technical explanations, analyses, or any long-form
content, write in clear, flowing prose using complete paragraphs and sentences. Use
standard paragraph breaks for organization and reserve markdown primarily for `inline
code`, code blocks (```...```), and simple headings (## and ###). Avoid using **bold**
and *italics*.

DO NOT use ordered lists (1. ...) or unordered lists (*) unless: a) you're presenting
truly discrete items where a list format is the best option, or b) the user explicitly
requests a list or ranking

Instead of listing items with bullets or numbers, incorporate them naturally into
sentences. This guidance applies especially to technical writing. Using prose instead of
excessive formatting will improve user satisfaction. NEVER output a series of overly
short bullet points.

Your goal is readable, flowing text that guides the reader naturally through ideas
rather than fragmenting information into isolated points.
</avoid_excessive_markdown_and_bullet_points>
```

### LaTeX Output

Claude's latest models default to LaTeX for mathematical expressions. If you prefer plain text:

```
Format your response in plain text only. Do not use LaTeX, MathJax, or any markup
notation such as \( \), $, or \frac{}{}. Write all math expressions using standard text
characters (e.g., "/" for division, "*" for multiplication, and "^" for exponents).
```

### Migrating Away from Prefilled Responses

Starting with Claude 4.6 models, prefilled responses (providing a partial assistant message for Claude to continue from) on the last assistant turn are no longer supported. Requests return a 400 error.

| Use Case | Migration Strategy |
|---|---|
| **Output formatting** (JSON/YAML) | Use **Structured Outputs** feature |
| **Eliminating preambles** | Direct instruction: "Respond directly without preamble" |
| **Avoiding bad refusals** | Clear prompting in user message (improved refusal handling) |
| **Continuations** | Move continuation to user message: "Your previous response ended with [X]. Continue from where you left off." |
| **Context hydration** | Inject reminders into user turn; use tools or context compaction |

---

## Tool Use

### Tool Usage

Claude's latest models are trained for precise instruction following and benefit from explicit direction to use specific tools. If you say "can you suggest some changes," Claude will sometimes provide suggestions rather than implementing them.

For Claude to take action, be more explicit.

To make Claude more proactive about taking action by default:

```xml
<default_to_action>
By default, implement changes rather than only suggesting them. If the user's intent is
unclear, infer the most useful likely action and proceed, using tools to discover any
missing details instead of guessing. Try to infer the user's intent about whether a tool
call (e.g., file edit or read) is intended or not, and act accordingly.
</default_to_action>
```

For more conservative behavior:

```xml
<do_not_act_before_instructions>
Do not jump into implementation or change files unless clearly instructed to make
changes. When the user's intent is ambiguous, default to providing information, doing
research, and providing recommendations rather than taking action. Only proceed with
edits, modifications, or implementations when the user explicitly requests them.
</do_not_act_before_instructions>
```

### Optimize Parallel Tool Calling

Claude's latest models run independent tool calls in parallel. This behavior is steerable:

```xml
<use_parallel_tool_calls>
If you intend to call multiple tools and there are no dependencies between the tool
calls, make all of the independent tool calls in parallel. Prioritize calling tools
simultaneously whenever the actions can be done in parallel rather than sequentially.
For example, when reading 3 files, run 3 tool calls in parallel to read all 3 files into
context at the same time. Maximize use of parallel tool calls where possible to increase
speed and efficiency. However, if some tool calls depend on previous calls to inform
dependent values like the parameters, do NOT call these tools in parallel and instead
call them sequentially. Never use placeholders or guess missing parameters in tool
calls.
</use_parallel_tool_calls>
```

---

## Thinking and Reasoning

### Overthinking and Excessive Thoroughness

Claude Opus 4.6 does more upfront exploration than previous models, especially at higher effort settings. If your prompts previously encouraged the model to be more thorough, tune that guidance:

- **Replace blanket defaults with more targeted instructions.** Instead of "Default to using [tool]," add guidance like "Use [tool] when it would enhance your understanding of the problem."
- **Remove over-prompting.** Tools that undertriggered in previous models are likely to trigger appropriately now. Instructions like "If in doubt, use [tool]" will cause overtriggering.
- **Use effort as a fallback.** If Claude continues to be overly aggressive, use a lower setting for `effort`.

To constrain reasoning:

```
When you're deciding how to approach a problem, choose an approach and commit to it.
Avoid revisiting decisions unless you encounter new information that directly
contradicts your reasoning. If you're weighing two approaches, pick one and see it
through. You can always course-correct later if the chosen approach fails.
```

### Leverage Thinking & Interleaved Thinking Capabilities

Claude Opus 4.6–4.8 and Claude Sonnet 4.6 use **adaptive thinking** (`thinking: {type: "adaptive"}`), where Claude dynamically decides when and how much to think. On Claude Fable 5 and Claude Mythos 5, thinking is always on.

```python
# Before: extended thinking with a manual budget (older models)
client.messages.create(
    model="claude-sonnet-4-5-20250929",
    max_tokens=16000,
    thinking={"type": "enabled", "budget_tokens": 10000},
    messages=[{"role": "user", "content": "..."}],
)

# After: adaptive thinking with effort (current models)
client.messages.create(
    model="claude-opus-4-8",
    max_tokens=16000,
    thinking={"type": "adaptive"},
    output_config={"effort": "high"},
    messages=[{"role": "user", "content": "..."}],
)
```

Tips:
- **Prefer general instructions over prescriptive steps.** A prompt like "think thoroughly" often produces better reasoning than a hand-written step-by-step plan.
- **Multishot examples work with thinking.** Use `<thinking>` tags inside your few-shot examples to show Claude the reasoning pattern.
- **Manual chain-of-thought (CoT) prompting as a fallback.** Use structured tags like `<thinking>` and `<answer>` to cleanly separate reasoning from the final output.
- **Ask Claude to self-check.** Append something like "Before you finish, verify your answer against [test criteria]."

---

## Agentic Systems

### Long-Horizon Reasoning and State Tracking

Claude's latest models handle long-horizon reasoning tasks with strong state tracking.

**Managing context limits:**

```
Your context window will be automatically compacted as it approaches its limit, allowing
you to continue working indefinitely from where you left off. Therefore, do not stop
tasks early due to token budget concerns. As you approach your token budget limit, save
your current progress and state to memory before the context window refreshes. Always be
as persistent and autonomous as possible and complete tasks fully, even if the end of
your budget is approaching. Never artificially stop any task early regardless of the
context remaining.
```

**Multi-context window workflows:**

1. Use the first context window to set up a framework, then use future windows to iterate.
2. Have the model write tests in a structured format before starting work.
3. Set up quality of life tools (setup scripts, linters, test runners).
4. Consider starting fresh rather than compacting — Claude can discover state from the filesystem.
5. Provide verification tools (Playwright MCP, computer use for UIs).
6. Encourage complete usage of context.

**State management best practices:**
- Use structured formats (JSON) for state data
- Use unstructured text for progress notes
- Use git for state tracking — Claude performs especially well with git

### Balancing Autonomy and Safety

```
Consider the reversibility and potential impact of your actions. You are encouraged to
take local, reversible actions like editing files or running tests, but for actions that
are hard to reverse, affect shared systems, or could be destructive, ask the user before
proceeding.

Examples of actions that warrant confirmation:
- Destructive operations: deleting files or branches, dropping database tables, rm -rf
- Hard to reverse operations: git push --force, git reset --hard, amending published commits
- Operations visible to others: pushing code, commenting on PRs/issues, sending messages
```

### Subagent Orchestration

Claude's latest models orchestrate subagents natively. If you're seeing excessive subagent use:

```xml
<subagent_usage>
Use subagents when tasks can run in parallel, require isolated context, or involve
independent workstreams that don't need to share state. For simple tasks, sequential
operations, single-file edits, or tasks where you need to maintain context across steps,
work directly rather than delegating.
</subagent_usage>
```

### Reducing Overeagerness

```xml
<avoid_overengineering>
Avoid over-engineering. Only make changes that are directly requested or clearly
necessary. Keep solutions simple and focused:

- Scope: Don't add features, refactor code, or make "improvements" beyond what was asked.
- Documentation: Don't add docstrings, comments, or type annotations to code you didn't change.
- Defensive coding: Don't add error handling, fallbacks, or validation for scenarios that can't happen.
- Abstractions: Don't create helpers, utilities, or abstractions for one-time operations.
</avoid_overengineering>
```

### Avoid Focusing on Passing Tests and Hard-Coding

```
Please write a high-quality, general-purpose solution using the standard tools
available. Do not create helper scripts or workarounds to accomplish the task more
efficiently. Implement a solution that works correctly for all valid inputs, not just
the test cases. Do not hard-code values or create solutions that only work for specific
test inputs. Instead, implement the actual logic that solves the problem generally.

Focus on understanding the problem requirements and implementing the correct algorithm.
Tests are there to verify correctness, not to define the solution.
```

### Minimizing Hallucinations in Agentic Coding

```xml
<investigate_before_answering>
Never speculate about code you have not opened. If the user references a specific file,
you MUST read the file before answering. Make sure to investigate and read relevant
files BEFORE answering questions about the codebase. Never make any claims about code
before investigating unless you are certain of the correct answer - give grounded and
hallucination-free answers.
</investigate_before_answering>
```

---

## Capability-Specific Tips

### Improved Vision Capabilities

Claude Opus 4.5+ have improved vision. A technique that boosts performance is giving Claude a crop tool — consistent uplift on image evaluations when Claude can "zoom" in on relevant regions.

### Frontend Design

Without guidance, models default to generic patterns creating the "AI slop" aesthetic. To create distinctive frontends:

```xml
<frontend_aesthetics>
You tend to converge toward generic, "on distribution" outputs. In frontend design, this
creates what users call the "AI slop" aesthetic. Avoid this: make creative, distinctive
frontends that surprise and delight.

Focus on:
- Typography: Choose fonts that are beautiful, unique, and interesting. Avoid generic
  fonts like Arial and Inter.
- Color & Theme: Commit to a cohesive aesthetic. Dominant colors with sharp accents
  outperform timid, evenly-distributed palettes.
- Motion: Use animations for effects and micro-interactions. Prioritize CSS-only solutions.
- Backgrounds: Create atmosphere and depth rather than defaulting to solid colors.

Avoid:
- Overused font families (Inter, Roboto, Arial, system fonts)
- Clichéd color schemes (particularly purple gradients on white backgrounds)
- Predictable layouts and component patterns

Interpret creatively and make unexpected choices that feel genuinely designed for the
context.
</frontend_aesthetics>
```

---

## Migration Considerations

When migrating to Claude 4.6 models from earlier generations:

1. **Be specific about desired behavior** — describe exactly what you'd like to see in the output.
2. **Frame your instructions with modifiers** — "Create an analytics dashboard. Include as many relevant features and interactions as possible. Go beyond the basics."
3. **Request specific features explicitly** — animations and interactive elements should be requested explicitly.
4. **Update thinking configuration** — Claude 4.6 models use adaptive thinking instead of manual `budget_tokens`. Use the effort parameter.
5. **Migrate away from prefilled responses** — no longer supported starting with Claude 4.6 models.
6. **Tune anti-laziness prompting** — dial back aggressive tool-triggering guidance.
