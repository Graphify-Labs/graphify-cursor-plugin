# Graphify Cursor plugin

The official [Graphify](https://graphify.com) plugin for the Cursor Marketplace.

Graphify indexes your repositories into a knowledge graph of entities and relationships.
This plugin points your coding agent at the Graphify MCP server so it can query that
graph directly instead of inferring structure from whatever files are open.

## Plugins in this repo

| Plugin | Description |
| --- | --- |
| [`graphify`](./plugins/graphify) | Query your codebase's knowledge graph over MCP: entities, relationships, grounded paths, and module conventions. |

## Develop

Validate before submitting:

```bash
node scripts/validate-template.mjs
```

See each plugin's own README for setup and the tool surface. Publishing is handled
through the [Cursor Marketplace](https://cursor.com) publisher application.
