# Tools Use — Complete Documentation

## Source
https://platform.claude.com/docs/en/agents-and-tools/tool-use/

## Overview

Tool use lets Claude call **functions you define** (client tools) or **functions Anthropic provides** (server tools).

- **Client tools**: Your app executes them. Claude returns `stop_reason: "tool_use"` + `tool_use` blocks.
- **Server tools**: Anthropic handles execution; results appear directly.

### Categories
1. **User-Defined Tools (Client-Executed)**: You write schema, execute code, return results
2. **Anthropic-Schema Tools (Client-Executed)**: bash, text_editor, computer, memory
3. **Server-Executed Tools**: web_search, web_fetch, code_execution, tool_search

### The Agentic Loop
1. Send request with `tools` array
2. Claude responds with `stop_reason: "tool_use"` + `tool_use` blocks
3. Execute each tool → format outputs as `tool_result`
4. Send new request with original messages + assistant response + `tool_result`
5. Repeat while `stop_reason == "tool_use"`

### System Prompt Token Counts

| Model | `auto` / `none` | `any` / `tool` |
|---|---|---|
| Claude Opus 4.8 | 290 | 410 |
| Claude Opus 4.7 | 675 | 804 |
| Claude Sonnet 4.6 | 497 | 589 |
| Claude Haiku 4.5 | 496 | 588 |

### Tool Definition Schema
| Parameter | Description |
|---|---|
| `name` | Must match `^[a-zA-Z0-9_-]{1,64}$` |
| `description` | Detailed plaintext: what, when, how |
| `input_schema` | JSON Schema for parameters |
| `input_examples` | Optional example input objects |

### Best Practices
- Provide **extremely detailed descriptions** (3–4+ sentences)
- Use `input_examples` for complex tools
- Consolidate related operations with `action` parameter
- Use meaningful namespacing
- Design tool responses to return only high-signal information

### Parallel Tool Use
- All parallel tool results must be in a **single user message**
- Use system prompt to encourage parallel calls

### Tool Runner (SDK)
Automates the agentic tool-call loop. Available in Python, TypeScript, C#, Go, Java, PHP, Ruby.

### Strict Tool Use
Setting `strict: true` guarantees JSON Schema compliance via grammar-constrained sampling.

### Server Tools
- `server_tool_use` blocks with `srvtoolu_` prefix
- `pause_turn` for long-running turns
- Domain filtering with `allowed_domains` / `blocked_domains`

### Web Search
Latest version: `web_search_20260318`. $10 per 1,000 searches.

### Code Execution
Sandboxed Python/Bash. Free when used with web search or web fetch.

### Tool Search
Scale to thousands of tools with regex-based dynamic discovery.

### Memory Tool
Client-side `/memories` directory with CRUD operations.

### Computer Use
Desktop interaction via screenshots, mouse, and keyboard.
