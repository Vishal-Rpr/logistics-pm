# Connectors

This plugin works standalone with manual input. Connecting tools improves the experience by giving Claude direct access to your project data.

## Included Connectors

| Category | Tool | What it enables |
|----------|------|----------------|
| Project tracker | **Linear** | Create tickets from PRDs, read backlog context, check sprint status |

## Recommended Additional Connectors

Add these to `.mcp.json` based on your stack:

| Category | Options | What it enables |
|----------|---------|----------------|
| Knowledge base | Notion, Google Docs | Read existing specs, meeting notes, research docs |
| Chat | Slack | Pull stakeholder context, share updates |
| Design | Figma | Reference designs when writing specs |
| Analytics | Amplitude, Mixpanel | Pull product usage data for metrics review |
| Accounting | Accounting-system MCP (custom) | Read financial data for credit management features |
| Rate intelligence | Rate-intelligence MCP (custom) | Query market rates for RFQ analysis |

## How to Add a Connector

Edit `.mcp.json` and add the server configuration. Example for adding Notion:

```json
{
  "mcpServers": {
    "linear": {
      "type": "url",
      "url": "https://mcp.linear.app/sse"
    },
    "notion": {
      "type": "url",
      "url": "https://mcp.notion.so/sse"
    }
  }
}
```

## Custom MCP Servers

For systems without official MCP servers (accounting systems, rate intelligence platforms, carrier APIs), you can build lightweight custom MCP servers. See the rate benchmarking PRD in `reference-docs/` for an example of how to evaluate and scope an MCP wrapper for a rate intelligence API.
