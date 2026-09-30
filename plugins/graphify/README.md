# Graphify for Cursor

Search indexed code, trace dependencies, assess change impact, and retrieve
repository memory through Graphify's authenticated MCP server.

The package includes a remote MCP connection at `https://api.graphify.com/mcp`
(Streamable HTTP) and a rule for investigating code. It has no local executable,
install script, hook, bundled credential, or runtime dependency to install.

## Setup

1. Sign in to [Graphify](https://app.graphify.com), connect a repository you are
   authorized to use, and wait for indexing to complete.
2. Install Graphify from Cursor's marketplace once its listing is approved.
   Before approval, use the [local test guide](../../docs/cursor-submission.md#local-smoke-test)
   or the manual MCP configuration below.
3. Open Cursor's MCP settings and use the Graphify server's sign-in/connect
   action. Complete the Graphify OAuth flow in your browser and return to Cursor.
   No API key or client secret is included in this plugin; reauthentication may
   be required when a session expires or access changes.
4. Ask Cursor to list your Graphify workspaces and repositories, then identify
   the repository to investigate. If it needs to change workspace, confirm the
   choice: `set_workspace` also changes your account's default workspace.

For example: "Using Graphify, find where authentication is implemented in
`my-repository` and cite the relevant files." Other useful requests are "Trace
the callers of this symbol" and "Which linked tests should I inspect before
changing this function?" Use real repository and symbol names from your index.

## Main tools

The connected server supplies the authoritative schemas, descriptions, and
approval annotations. Availability can vary with server configuration and access.

| Tools | Purpose |
| --- | --- |
| `list_workspaces`, `list_repositories` | Discover accessible scope and repository IDs. |
| `set_workspace` | Select the active workspace and update the account's default workspace. |
| `query_graph`, `graphify_find`, `graphify_node` | Search indexed code and inspect symbols. |
| `graphify_callers`, `graphify_callees`, `graphify_trace` | Investigate directed call relationships. |
| `shortest_path` | Find a connecting graph path, which need not be a directed call chain. |
| `graphify_impact`, `impact_and_risk`, `graphify_tests_for` | Explore potential change impact and linked tests. These do not execute tests. |
| `recall`, `memories_about` | Retrieve saved repository context. |
| `remember` | Save durable repository memory; the result may require review. |

## Data handling and limitations

Tool arguments are sent to Graphify's hosted service under your authenticated
workspace permissions, and returned code context becomes available to the Cursor
agent. Do not put secrets or unrelated private information in queries.

The MCP tools do not edit source files. They are **not all read-only**:
`remember` persists memory and `set_workspace` changes account scope. When trail
capture is enabled for a workspace, `query_graph`, `graphify_trace`,
`graphify_find`, and `graphify_rank_files` may save query/search context as
repository memory; `recall` may record an unanswered memory query. Follow the
live tool annotations and Cursor's approval prompts.

Results describe an indexed snapshot, not necessarily the current branch or
uncommitted edits. Inspect local code before changing it. Static graph analysis
does not prove runtime behavior, security, or exhaustive test coverage. Saved
memories may contain historical or unverified statements.

Symbol lookup can use semantic fallback. If `graphify_node` cannot resolve an
exact match, it may return a different symbol with `resolved: semantic`, including
for a nonexistent name. Treat it as a suggestion and verify the returned symbol
and file before relying on it. An empty caller list means no matching indexed
callers were returned; it is not proof that a function is unused.

See Graphify's [privacy policy](https://graphify.com/privacy) for processing,
retention, and subprocessors, and its [terms](https://graphify.com/terms) for
service conditions. Plugin source licensing does not confer hosted service access.

## Manual MCP configuration

As an alternative to the plugin, merge this server entry into `~/.cursor/mcp.json`
(global) or `.cursor/mcp.json` (project). Preserve existing entries. This connects
the MCP server but does not install the plugin's rule. Avoid configuring the same
server both manually and through the plugin.

```json
{
  "mcpServers": {
    "graphify": {
      "url": "https://api.graphify.com/mcp"
    }
  }
}
```

## Troubleshooting

- **Needs login / unauthorized:** complete or renew the Graphify OAuth sign-in
  from Cursor's MCP settings. Do not paste tokens into the repository.
- **No repository or empty results:** verify the selected workspace, repository
  permissions, and completed indexing in Graphify. Use `list_repositories` to
  obtain the current ID instead of guessing one.
- **Stale answers:** compare the indexed snapshot with the branch and local files
  being edited; reindex through Graphify as needed.
- **Duplicate servers:** keep one Graphify connection. A local plugin, marketplace
  install, and manual MCP entry can otherwise overlap.
- **Local plugin missing:** copy the plugin directory with its hidden manifest,
  reload Cursor, and check Customize. Managed accounts may restrict local imports.

## Support

- [Graphify website](https://graphify.com) · [Documentation](https://docs.graphify.com)
- [Plugin issues](https://github.com/Graphify-Labs/graphify-cursor-plugin/issues)
- [Contact Graphify](https://graphify.com/contact) · [founders@graphify.com](mailto:founders@graphify.com)
