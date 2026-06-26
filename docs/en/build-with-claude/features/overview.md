# Claude Platform Features Overview

## API Surface Organization

Claude's API is organized into **five core areas**:

1. **Model capabilities** — Control reasoning and response formatting
2. **Tools** — Let Claude take actions on the web or in your environment
3. **Tool infrastructure** — Discovery and orchestration at scale
4. **Context management** — Keep long-running sessions efficient
5. **Files and assets** — Manage documents and data provided to Claude

## Feature Availability Classifications

| Classification | Key Details |
|---|---|
| **Beta** | Preview features; may change/discontinue |
| **Generally Available (GA)** | Stable, production-ready |
| **Deprecated** | Functional but no longer recommended |
| **Retired** | No longer available |

**Platforms:** Claude API, AWS Bedrock, Vertex AI (Google), Microsoft Foundry (Azure)

## Model Capabilities

| Feature | Description | Availability |
|---|---|---|
| **Context windows** | Up to 1M tokens | All platforms |
| **Adaptive thinking** | Dynamic thinking depth control | All platforms |
| **Batch processing** | Async bulk processing at 50% cost savings | Claude API, AWS |
| **Citations** | Ground responses in source documents | All platforms |
| **Effort** | Control token usage vs. response thoroughness | All platforms |
| **Extended thinking** | Step-by-step reasoning transparency | All platforms |
| **PDF support** | Process text and visual content from PDFs | All platforms |
| **Structured outputs** | Guaranteed schema conformance | All platforms |

## Tools

### Server-side Tools
| Feature | Description |
|---|---|
| **Code execution** | Sandboxed code execution |
| **Web fetch** | Retrieve full content from web pages and PDFs |
| **Web search** | Augment Claude with current real-world web data |

### Client-side Tools
| Feature | Description |
|---|---|
| **Bash** | Execute shell commands and scripts |
| **Computer use** | Control interfaces via screenshots, mouse, and keyboard |
| **Memory** | Store/retrieve information across conversations |
| **Text editor** | Create and edit text files |

## Tool Infrastructure

| Feature | Description |
|---|---|
| **Agent Skills** | Extend Claude with pre-built or custom Skills |
| **MCP connector** | Connect to remote MCP servers directly |
| **Tool search** | Scale to thousands of tools |
