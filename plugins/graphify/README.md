# Graphify for Cursor

Ground your coding agent in your codebase's knowledge graph.

Graphify indexes your repositories into a graph of entities and relationships. This
plugin points Cursor at the Graphify MCP server so the agent can query that graph
directly: find how things connect, trace grounded paths, and read the conventions a
module actually follows, instead of inferring from whatever files happen to be open.

## What it adds

- **MCP server** at `https://api.graphify.com/mcp` (Streamable HTTP).
- **A rule** that tells the agent when to reach for the graph.

## Tools (all read-only)

| Tool | What it does |
| --- | --- |
| `query_graph` | Search entities and relationships across the indexed graph. |
| `get_node` | Fetch one node with its edges, file span, and confidence tags. |
| `path` | Trace the grounded path between two entities. |
| `explain_style` | Summarize the conventions a module actually follows. |

Nothing on this list can modify code, the graph, or your account.

## Setup

1. Install the plugin from the Cursor Marketplace.
2. On first use, Cursor runs a one-time sign-in (OAuth 2.1 via Auth0). There is no
   API key to paste and nothing to configure.
3. Ask the agent about your codebase. It will call the Graphify tools when a question
   is about structure, dependencies, or conventions.

You need a Graphify account with at least one indexed repository. Sign up and connect
a repo at [graphify.com](https://graphify.com).

## Manual configuration

If you prefer to wire the server yourself instead of installing the plugin, add this
to `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` (per repo):

```json
{
  "mcpServers": {
    "graphify": {
      "url": "https://api.graphify.com/mcp"
    }
  }
}
```

Cursor shows "Needs login" until you finish the one-time sign-in.

## Links

- Website: https://graphify.com
- Connect other agents (Claude Code, Codex, VS Code, and more): your Graphify dashboard, Integrations tab
