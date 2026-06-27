# Claude Platform Documentation

Full clone of Claude Platform documentation from https://docs.claude.com/

## Status

| Language | Pages | Status |
|----------|-------|--------|
| English (EN) | 69 | ✅ Complete (all available pages) |
| Russian (RU) | 18 | ⚠️ Partial (RU migration incomplete on docs.claude.com) |

**Total: 87 pages**

## Structure

```
en/build-with-claude/
├── overview.md                    # Features overview
├── working-with-messages.md       # Using the Messages API
├── stop-reasons-and-fallback.md   # Stop reasons and fallback
├── refusals-and-fallback.md       # Refusals and fallback
├── fallback-credit.md             # Fallback credit
├── extended-thinking.md           # Extended thinking
├── adaptive-thinking.md           # Adaptive thinking
├── effort.md                      # Effort parameter
├── task-budgets.md                # Task budgets
├── fast-mode.md                   # Fast mode
├── structured-outputs.md          # Structured outputs
├── citations.md                   # Citations
├── streaming-messages.md          # Streaming messages
├── batch-processing.md            # Batch processing
├── search-results.md              # Search results
├── streaming-refusals.md          # Streaming refusals
├── multilingual-support.md        # Multilingual support
├── embeddings.md                  # Embeddings
├── vision.md                      # Vision
├── pdf-support.md                 # PDF support
├── working-with-messages.md       # Working with Messages API
├── context-management/
│   ├── context-windows.md         # Context windows
│   ├── compaction.md              # Compaction
│   ├── context-editing.md         # Context editing
│   ├── prompt-caching.md          # Prompt caching
│   ├── mid-conversation-system-messages.md
│   ├── orchestration-mode.md      # Build an orchestration mode
│   ├── cache-diagnostics.md       # Cache diagnostics
│   └── token-counting.md          # Token counting
├── tools/
│   ├── overview.md                # Tools overview
│   ├── how-tool-use-works.md      # How tool use works
│   ├── tutorial.md                # Tutorial: Build a tool-using agent
│   ├── define-tools.md            # Define tools
│   ├── handle-tool-calls.md       # Handle tool calls
│   ├── parallel-tool-use.md       # Parallel tool use
│   ├── tool-runner.md             # Tool Runner (SDK)
│   ├── strict-tool-use.md         # Strict tool use
│   ├── server-tools.md            # Server tools
│   ├── web-search.md              # Web search tool
│   ├── web-fetch.md               # Web fetch tool
│   ├── code-execution.md          # Code execution tool
│   ├── advisor.md                 # Advisor tool
│   ├── tool-search.md             # Tool search tool
│   ├── memory.md                  # Memory tool
│   ├── bash.md                    # Bash tool
│   ├── text-editor.md             # Text editor tool
│   ├── computer-use.md            # Computer use tool
│   ├── troubleshooting.md         # Troubleshooting
│   ├── tool-reference.md          # Tool reference
│   ├── manage-tool-context.md     # Manage tool context
│   ├── tool-combinations.md       # Tool combinations
│   ├── tool-use-with-prompt-caching.md
│   ├── programmatic-tool-calling.md
│   └── fine-grained-tool-streaming.md
├── working-with-files/
│   ├── files-api.md               # Files API
│   ├── pdf-support.md             # PDF support
│   └── images-and-vision.md       # Images and vision
├── skills/
│   ├── overview.md                # Skills overview
│   ├── quickstart.md              # Skills quickstart
│   ├── best-practices.md          # Skills best practices
│   ├── enterprise.md              # Skills for enterprise
│   └── api.md                     # Skills in the API
├── mcp/
│   ├── remote-servers.md          # Remote MCP servers
│   ├── connector.md               # MCP connector
│   └── tunnels.md                 # MCP tunnels
└── cloud-platforms/
    ├── amazon-bedrock.md          # Amazon Bedrock
    ├── amazon-bedrock-legacy.md   # Amazon Bedrock (legacy)
    ├── claude-on-aws.md           # Claude Platform on AWS
    ├── google-cloud.md            # Google Cloud
    └── microsoft-foundry.md       # Microsoft Foundry
```

## Source

- **URL**: https://docs.claude.com/en/build-with-claude/
- **Format**: Full markdown (not summaries)
- **Last updated**: 2026-06-13

## Notes

- All pages are saved as full literal content — not summaries
- Some pages return 404 because docs.claude.com restructured URLs
- RU documentation is partially available (migration in progress)
