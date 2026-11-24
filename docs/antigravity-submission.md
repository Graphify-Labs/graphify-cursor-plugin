# Antigravity Marketplace submission

Prepared on 2026-11-25 for Graphify 0.1.2, following
[Build plugins](https://antigravity.google/docs/build-plugins/). This is
submission preparation, not a claim of an existing listing.

## Package

The Antigravity plugin lives in the same folder as the Cursor plugin,
`plugins/graphify`, using the multi-provider layout the Antigravity docs
describe. Antigravity reads `.antigravity-plugin/plugin.json`, which takes
precedence over other manifests and is promoted to the root `plugin.json` when the
Marketplace bundle is built.

| Item | Value |
| --- | --- |
| Plugin name / display name | `graphify` / Graphify |
| Publisher | Graphify Labs |
| Repository | https://github.com/Graphify-Labs/graphify-cursor-plugin |
| Plugin folder | https://github.com/Graphify-Labs/graphify-cursor-plugin/tree/main/plugins/graphify |
| Manifest | `plugins/graphify/.antigravity-plugin/plugin.json` |
| MCP config | `plugins/graphify/mcp_config.json` (remote Streamable HTTP, `https://api.graphify.com/mcp`) |
| Rule | `plugins/graphify/rules/graphify.md` (`trigger: model_decision`) |
| Guide | `plugins/graphify/.antigravity-plugin/README.md` |
| Logo | `plugins/graphify/assets/logo-antigravity.png` (512x512, transparent padding) |
| Version | `0.1.2` |
| Source license | MIT |
| Website / docs | https://graphify.com / https://docs.graphify.com |
| Support | founders@graphify.com |
| Privacy / terms | https://graphify.com/privacy / https://graphify.com/terms |

## Checks

- `agy plugin validate plugins/graphify` (Antigravity CLI 1.5.16) passes with
  one rule, one MCP server, zero errors, and one expected warning: Antigravity
  ignores Cursor's `mcp.json`. The docs list `mcp.json` as a known multi-tool
  file, and the CI filter from the docs passes.
- `agy plugin install ./plugins/graphify` installs the plugin enabled, with one
  rule (`graphify.md`) and one MCP server (`graphify`).
- `node scripts/validate-template.mjs` checks both packages.
- Sign-in: `https://api.graphify.com/mcp` returns 401 with protected resource
  metadata whose `resource` is the MCP URL, and the authorization server
  advertises dynamic client registration and PKCE S256, which Antigravity uses
  when no client ID is configured.

## Not yet tested

- A live agent session in Antigravity, the browser sign-in, and tool calls.
  These need an Antigravity Google sign-in and a Graphify account.
- Installing from the Marketplace, which needs the listing.
