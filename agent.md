# agent.md

This file provides agent-specific rules for working with Datagen SDK and MCP tools in this repository.

## Datagen SDK + Datagen MCP (agent rules)

### When to use what

- Use **Datagen MCP** for interactive discovery/debugging:
  - `searchTools` to find the right tool alias
  - `getToolDetails` to confirm exact input schema
- Use **Datagen Python SDK** for execution in real code:
  - `DatagenClient.execute_tool(tool_alias, params)`

### Non-negotiable workflow

1) If you don't know the tool name: call `searchTools`.
2) In code, always create the SDK client as `client`:
   - `from datagen_sdk import DatagenClient`
   - `client = DatagenClient()`
3) Before you call a tool from code: call `getToolDetails` and match the schema exactly.
4) Execute via SDK using the exact alias name you discovered:
   - `client.execute_tool(tool_alias, params)`
5) If you hit auth errors: tell the user to set `DATAGEN_API_KEY` and/or connect/auth the relevant MCP server in DataGen.

### SDK quickstart

```python
import os
from datagen_sdk import DatagenClient

assert os.getenv("DATAGEN_API_KEY"), "Set DATAGEN_API_KEY first"

client = DatagenClient()
print(client.execute_tool("listTools"))
```

## GTM Research Context

When building prototypes for prospect pain points:

1. **Start with tool discovery**: Use `searchTools` to find relevant tools for the workflow
2. **Verify tool schemas**: Always call `getToolDetails` before writing code that calls the tool
3. **Build incrementally**: Test each step of a multi-step workflow independently
4. **Document complexity**: Track how many tools/steps are required for each pain point solution
5. **Consider agency users**: Solutions should be explainable to non-technical strategists

## Code Organization

When creating prototype scripts:
- Place them in a `prototypes/` directory
- Name files descriptively (e.g., `swiss_company_enrichment.py`)
- Include inline comments explaining the workflow steps
- Document tool consumption and cost implications
- Add error handling for auth and API failures

## Testing Approach

For each prototype:
1. Test with mock/example data first
2. Verify tool availability via `listTools`
3. Validate all required MCP servers are connected
4. Document any manual setup steps required
5. Create a simple README for each prototype explaining its purpose and usage
