# Graphify plugin for Cursor and Antigravity

The official [Graphify](https://graphify.com) plugin source for Cursor and Google
Antigravity. It connects the agent to Graphify's authenticated MCP server and
supplies a rule for using indexed code and repository memory with appropriate
scope and citations.

| Plugin | Contents |
| --- | --- |
| [`graphify`](./plugins/graphify) | Remote MCP connection and a code investigation rule |

Both targets share one plugin folder. Each tool reads only its own files:

| File | Cursor | Antigravity |
| --- | --- | --- |
| Manifest | `.cursor-plugin/plugin.json` | `.antigravity-plugin/plugin.json` |
| MCP connection | `mcp.json` | `mcp_config.json` |
| Rule | `cursor-rules/graphify.mdc` | `rules/graphify.md` |
| Guide | `README.md` | `.antigravity-plugin/README.md` |

See the [Cursor guide](./plugins/graphify/README.md) and the
[Antigravity guide](./plugins/graphify/.antigravity-plugin/README.md) for setup,
available tools, data handling, limitations, and troubleshooting. A Graphify account with an
indexed repository is required. Marketplace availability depends on Cursor's
review; local testing and manual MCP configuration are available now.

## Validate and publish

Run from the repository root with Node.js 22 or later:

```bash
node scripts/validate-template.mjs
```

To check the Antigravity package, install the
[Antigravity CLI](https://antigravity.google/docs/getting-started/) and run
`agy plugin validate plugins/graphify`. The only expected warning is that
Antigravity ignores Cursor's `mcp.json`.

CI runs the same package checks. The [submission guide](./docs/cursor-submission.md)
contains the local smoke test, prepared listing copy, evidence, and the remaining
publisher steps. Submit the public repository through the
[Cursor publisher application](https://cursor.com/marketplace/publish).

The plugin package is [MIT licensed](./LICENSE). Use of the hosted Graphify service
is governed by its [terms](https://graphify.com/terms) and
[privacy policy](https://graphify.com/privacy).
