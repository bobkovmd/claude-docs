# Start building with Claude

Everything you need to integrate Claude into your applications. From first API call to production.

[Quickstart](/docs/en/get-started) [Get API key](/settings/keys) [API reference](/docs/en/api/overview)

```python
import anthropic

client = anthropic.Anthropic()

message = client.messages.create(
  model="claude-opus-4-8",
  max_tokens=1024,
  messages=[{
    "role": "user",
    "content": "Hello, Claude"
  }]
)
print(message.content[0].text)
```

## Choose how you build

Pick the developer surface that matches your approach, and the infrastructure that fits your stack.

### Messages

Direct model access. You construct every turn, manage conversation state, and write your own tool loop.

[Quickstart](/docs/en/get-started) [API reference](/docs/en/api/messages/create) [Client SDKs](/docs/en/api/client-sdks)

### Managed Agents

Fully managed agent infrastructure. Deploy and manage autonomous agents in stateful sessions with persistent event history.

[Quickstart](/docs/en/managed-agents/quickstart) [API reference](/docs/en/api/beta/sessions) [Define your agent](/docs/en/managed-agents/agent-setup) [Amazon Bedrock](/docs/en/build-with-claude/claude-in-amazon-bedrock) [Google Cloud](/docs/en/build-with-claude/claude-on-vertex-ai) [Microsoft Foundry](/docs/en/build-with-claude/claude-in-microsoft-foundry)

## From idea to production

Follow the lifecycle or jump to what you need.

### Get started

[Quickstart](/docs/en/get-started) [Get API key](/settings/keys) [Choose a model](/docs/en/about-claude/models/overview) [Install an SDK](/docs/en/api/client-sdks) [Try the Workbench](/workbench)

### Build

[Messages API](/docs/en/api/messages/create) [Extended thinking](/docs/en/build-with-claude/extended-thinking) [Vision](/docs/en/build-with-claude/vision) [Tool use](/docs/en/agents-and-tools/tool-use/overview) [Web search](/docs/en/agents-and-tools/tool-use/web-search-tool) [Code execution](/docs/en/agents-and-tools/tool-use/code-execution-tool) [Structured outputs](/docs/en/build-with-claude/structured-outputs) [Prompt caching](/docs/en/build-with-claude/prompt-caching) [Streaming](/docs/en/build-with-claude/streaming)

### Evaluate and ship

[Prompting best practices](/docs/en/build-with-claude/prompt-engineering/overview) [Run evals](/docs/en/test-and-evaluate/develop-tests) [Batch testing](/docs/en/build-with-claude/batch-processing) [Safety and guardrails](/docs/en/test-and-evaluate/strengthen-guardrails/increase-consistency) [Rate limits and errors](/docs/en/api/rate-limits) [Cost optimization](/docs/en/about-claude/pricing)

### Operate

[Workspaces and admin](/docs/en/build-with-claude/workspaces) [API key management](/settings/keys) [Usage monitoring](/docs/en/build-with-claude/usage-cost-api) [Model migration](/docs/en/about-claude/models/migration-guide)

## The Claude model family

Choose the right model for your use case.

**Most capable:** [Fable 5](/docs/en/about-claude/models/overview) - Highest capability for the most demanding reasoning and long-horizon agentic work.

**Advanced:** [Opus 4.8](/docs/en/about-claude/models/overview) - Excellent for complex analysis, coding, and creative tasks requiring deep reasoning.

**Best balance:** [Sonnet 4.6](/docs/en/about-claude/models/overview) - Ideal balance of intelligence and speed for most production workloads.

**Fastest:** [Haiku 4.5](/docs/en/about-claude/models/overview) - Lightning-fast responses for high-volume, latency-sensitive applications.

## Keep learning

[Courses](https://anthropic.skilljar.com/) Interactive courses to master Claude.
[Cookbook](https://platform.claude.com/cookbooks) Code samples and patterns.
[Quickstarts](https://github.com/anthropics/anthropic-quickstarts) Deployable starter apps.
[What's new](/docs/en/release-notes/overview) Latest features and updates.
[Claude Code](https://code.claude.com/docs/en/overview) An agentic coding assistant in your terminal.
