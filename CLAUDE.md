# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repository explores the benefits of Claude Code agents for GTM (Go-To-Market) use cases, specifically focusing on B2B agency operations, lead enrichment workflows, and agentic automation. The repo serves as a research and development space to prototype solutions for prospect pain points identified through customer discovery interviews.

## Repository Structure

- **transcript/**: Customer discovery meeting transcripts and summaries
  - Raw transcripts in JSON format
  - Processed summaries with pain points, quotes, and prospect profiles
  - Meeting metadata and context

## Context: DataGen Ecosystem

This repository is part of the broader DataGen platform ecosystem located at `/Users/yu-shengkuo/projects/datagendev/`. The parent directory contains:

- **DataGen/**: Core SignalGen AI agent platform (FastAPI + Wasp)
- **datagen-mcp-server/**: Cloudflare Workers MCP server
- **marketing/**: GTM content and use cases
- **Doc/**: User-facing documentation

Reference the parent `CLAUDE.md` at `/Users/yu-shengkuo/projects/datagendev/CLAUDE.md` for:
- DataGen platform architecture and development commands
- MCP server development and deployment
- Tool integration patterns and flow system usage

## Research Focus Areas

Based on prospect interviews, key GTM challenges being explored:

### 1. Complex Multi-Step Enrichment Operations
Building agentic workflows that handle company enrichment across multiple validation steps:
- Website validation for entity-specific domains
- LinkedIn account verification and matching
- Multi-geography headquarters identification
- Sequential tool orchestration without excessive API consumption

### 2. Intelligence Layer for Modern Outbound
Beyond basic outbound, agencies need:
- Context maintenance for prospects not yet ready to buy
- Personalized content generation for warmup sequences
- Buyer signal detection and tracking over time
- Strategic overlay on commodity outbound operations

### 3. Agency Technical Capability Gap
Solutions must bridge the gap between:
- Strategic/advisory expertise (common in agencies)
- Technical implementation of AI agents and automation (less common)
- Need for low-code or agent-assisted approaches to complex workflows

## Working with Prospect Data

### Transcript Analysis
When analyzing meeting transcripts:
- Extract pain points with supporting quotes from the transcript
- Identify specific workflow patterns and operational complexity
- Note prospect's role, company context, and industry vertical
- Look for technical vs. strategic skill gaps

### Pain Point Documentation Format
Structure pain points as:
- **Description**: Clear summary of the challenge
- **Evidence**: Direct quotes from transcript showing the pain
- **Context**: Prospect's role, company stage, and use case

## Connection to DataGen Platform

When prototyping solutions for identified pain points:

### Using SignalGen Flows
1. Map multi-step manual processes to Flow + Step architecture
2. Use `@variable` syntax for inputs and `#result` for step outputs
3. Reference tools with `/tool_name` pattern
4. Chain steps sequentially for operations requiring context

### Tool Integration Strategy
For complex enrichment workflows:
- Start with DataGen's existing tool providers (LinkedIn, Perplexity, Firecrawl)
- Identify gaps requiring new MCP-based tools
- Consider tool consumption costs and optimization patterns
- Use Python execution (`executeCode`) for custom logic between tool calls

### Validation with MCP Server
Test agentic workflows through:
- MCP server at `http://localhost:8788/sse` for debugging
- Claude Desktop integration for end-to-end testing
- Tool search functionality to discover available operations

## GTM Positioning

When creating content or examples from this research:

### Value Props to Emphasize
- "From CSV to CRM-ready signals in one session"
- "Leads routed in seconds, not shifts"
- "Clean CRM, confident forecasts—without another tool to learn"

### Target Use Cases
- Prospecting at Scale: Account + contact intelligence with personalization
- Real-Time Lead Routing: Dedup, enrich, and assign with fuzzy matching
- CRM Hygiene: Automated standardization and completeness checks

### Messaging Framework
Position DataGen as solving the "agentic workflow complexity" problem:
- Strategy-first agencies shouldn't need deep technical skills
- Complex multi-step operations packaged into simple flows
- Intelligence layer that goes beyond commodity outbound

## Development Workflow

### Adding New Prospect Research
1. Place raw meeting transcript JSON in `transcript/`
2. Generate summary with pain points and evidence
3. Create meeting summary markdown with structured format
4. Update this CLAUDE.md if new patterns emerge

### Prototyping Solutions
1. Identify pain point from transcript analysis
2. Design flow architecture using DataGen patterns
3. Test with DataGen local environment (see parent CLAUDE.md)
4. Document as use case template for marketing/

### Creating GTM Content
1. Extract anonymized pain points and quotes
2. Map to DataGen platform capabilities
3. Create before/after workflow examples
4. Package as use case or demo script

## Analysis Guidelines

When asked to analyze transcripts or develop solutions:
- Focus on operational complexity and workflow patterns
- Identify specific tool chains and sequential dependencies
- Consider agency vs. end-customer use cases
- Map pain points to DataGen Flow + Step + Tool architecture
- Think about low-code/agent-assisted approaches for non-technical users

## Datagen Python SDK (how to use)

### Purpose

Use the Datagen Python SDK (`datagen-python-sdk`) when you need to run DataGen-connected tools from a local Python codebase (apps, scripts, cron jobs). Use Datagen MCP for interactive discovery/debugging of tool names and schemas.

### Prerequisites

- Install: `pip install datagen-python-sdk`
- Auth: set `DATAGEN_API_KEY` in the environment
- Virtual environment located at `.venv/` (already configured in this repo)

### Mental model (critical)

- You execute tools by alias name: `client.execute_tool("<tool_alias>", params)`
- Tool aliases are commonly:
  - `mcp_<Provider>_<tool_name>` for connected MCP servers (Gmail/Linear/Neon/etc.)
  - First-party DataGen tools like `listTools`, `searchTools`, `getToolDetails`
- Always be schema-first: confirm params via `getToolDetails` before calling a tool from code.

### Recommended workflow (always follow)

1) Verify `DATAGEN_API_KEY` exists (if missing, ask user to set it)
2) Import and create the SDK client:
   - `from datagen_sdk import DatagenClient`
   - `client = DatagenClient()`
3) Discover tool alias with `searchTools` (don't guess)
4) Confirm tool schema with `getToolDetails`
5) Execute with `client.execute_tool(tool_alias, params)`
6) Handle errors:
   - 401/403: missing/invalid API key OR the target MCP server isn't connected/authenticated in DataGen dashboard
   - 400/422: wrong params → re-check `getToolDetails` and retry

### Minimal example

```python
import os
from datagen_sdk import DatagenClient

if not os.getenv("DATAGEN_API_KEY"):
    raise RuntimeError("DATAGEN_API_KEY not set")

client = DatagenClient()
result = client.execute_tool(
    "mcp_Gmail_gmail_send_email",
    {
        "to": "user@example.com",
        "subject": "Hello",
        "body": "Hi from DataGen!",
    },
)
print(result)
```

### Discovery examples (don't skip)

```python
from datagen_sdk import DatagenClient

client = DatagenClient()

# List all tools
tools = client.execute_tool("listTools")

# Search by intent
matches = client.execute_tool("searchTools", {"query": "send email"})

# Get schema for a tool alias
details = client.execute_tool("getToolDetails", {"tool_name": "mcp_Gmail_gmail_send_email"})
```
