# Graphify for Antigravity

Search indexed code, trace callers and dependencies, check change impact, and
recall repository memory through Graphify's authenticated MCP server.

The plugin adds a remote MCP connection to `https://api.graphify.com/mcp`
(Streamable HTTP) and a rule that tells the agent when and how to use it. It has
no local executable, install script, hook, bundled credential, or runtime
dependency.

## Setup

1. Sign in to [Graphify](https://app.graphify.com), connect a repository you are
   authorized to use, and wait for indexing to finish.
2. Install Graphify from the Antigravity Marketplace, or from the CLI with
   `agy plugin install https://github.com/Graphify-Labs/graphify-cursor-plugin/tree/main/plugins/graphify`.
3. When the Graphify MCP server first connects, Antigravity opens the Graphify
   sign-in in your browser. Approve access and return to Antigravity. No API key
   or client secret is included in this plugin.
4. Ask the agent to list your Graphify workspaces and repositories, then name
   the repository to investigate. If it needs to change workspace, confirm the
   choice: `set_workspace` also changes your account's default workspace.

For example: "Using Graphify, find where authentication is implemented in
`my-repository` and cite the relevant files." Other useful requests are "Trace
the callers of this symbol" and "Which linked tests should I check before
changing this function?"

## Main tools

The connected server supplies the authoritative schemas, descriptions, and
approval annotations. Availability can vary with server configuration and access.

| Tools | Purpose |
| --- | --- |
| `list_workspaces`, `list_repositories` | Discover accessible scope and repository IDs. |
| `set_workspace` | Select the active workspace and update the account's default workspace. |
| `query_graph`, `graphify_find`, `graphify_node` | Search indexed code and inspect symbols. |
| `graphify_callers`, `graphify_trace` | Investigate callers and directed call paths. |
| `graphify_file_neighbors`, `graphify_imports_exports` | Map a file's dependencies and dependents. |
| `graphify_rank_files` | Rank the files most relevant to a question. |
| `graphify_impact`, `graphify_tests_for` | Explore potential change impact and linked tests. These do not run tests. |
| `recall` | Retrieve saved repository context. |
| `remember` | Save durable repository memory; the result may need review. |

## Data handling and limitations

Tool arguments are sent to Graphify's hosted service under your authenticated
workspace permissions, and returned code context becomes available to the
Antigravity agent. Do not put secrets or unrelated private information in queries.

The MCP tools do not edit source files, but they are not all read only:
`remember` saves memory and `set_workspace` changes account scope. When trail
capture is enabled for a workspace, some search tools may save query context as
repository memory. Follow the live tool annotations and Antigravity's approval
prompts.

Results describe an indexed snapshot, not necessarily the current branch or
uncommitted edits. Static graph analysis does not prove runtime behavior or
complete test coverage. If `graphify_node` cannot resolve an exact match it may
return a different symbol marked `resolved: semantic`; verify it before relying
on it.

See Graphify's [privacy policy](https://graphify.com/privacy) and
[terms](https://graphify.com/terms).

## Troubleshooting

- **Needs sign in or unauthorized:** reconnect the Graphify MCP server from the
  plugin's MCP tab and complete the browser sign-in again.
- **No repository or empty results:** check the selected workspace, repository
  permissions, and finished indexing in Graphify. Use `list_repositories` to get
  the current ID instead of guessing one.
- **Duplicate servers:** keep one Graphify connection. A plugin install and a
  manual entry in `mcp_config.json` can otherwise overlap.

## Support

- [Graphify](https://graphify.com) · [Documentation](https://docs.graphify.com)
- [Plugin issues](https://github.com/Graphify-Labs/graphify-cursor-plugin/issues)
- [founders@graphify.com](mailto:founders@graphify.com)
