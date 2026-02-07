# agentOS-Andriy

GTM research repository exploring Claude Code agent benefits for B2B agency operations, lead enrichment workflows, and agentic automation.

## Purpose

This repository serves as a research and development space to prototype solutions for prospect pain points identified through customer discovery interviews. The focus is on exploring how Claude Code agents can solve real GTM challenges in agency operations.

## Setup

### Virtual Environment

The repository includes a Python virtual environment for running prototypes:

```bash
source .venv/bin/activate
```

### Datagen Python SDK

The SDK is already installed. To verify:

```bash
python3 -c "from datagen_sdk import DatagenClient; print('SDK ready')"
```

### Environment Variables

Ensure `DATAGEN_API_KEY` is set:

```bash
python3 -c "import os; assert os.getenv('DATAGEN_API_KEY'), 'DATAGEN_API_KEY not set'; print('API key configured')"
```

## Structure

```
.
├── transcript/          # Customer discovery interviews
│   ├── andriy_transcript.json
│   ├── andriy_summary.json
│   └── andriy_meeting_summary.md
├── CLAUDE.md           # Guidance for Claude Code
├── agent.md            # Agent-specific rules for Datagen SDK
└── README.md           # This file
```

## Key Pain Points (from Andriy interview)

1. **Complex Multi-Step Enrichment**: Swiss company validation across websites, LinkedIn, and multi-geo HQs
2. **Intelligence Layer Need**: Beyond commodity outbound - context maintenance and personalized warmup
3. **Agency Technical Gap**: Strategy-focused agencies struggle with agentic workflow implementation
4. **Tool Consumption Efficiency**: Building agents that don't require excessive API calls
5. **Technical Skill Barriers**: Need for low-code approaches to complex AI automation

## Development Workflow

See `CLAUDE.md` for detailed guidance on:
- Working with prospect data and transcripts
- Prototyping solutions using DataGen platform
- Using the Datagen Python SDK
- Creating GTM content from research

## Related Resources

- Parent DataGen platform: `/Users/yu-shengkuo/projects/datagendev/`
- DataGen documentation: See `../Doc/`
- Marketing use cases: See `../marketing/`
