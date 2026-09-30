# Cursor marketplace submission

Prepared on 2026-09-30 for Graphify 0.1.1. This is submission preparation, not a
claim of approval or an existing public listing.

## Current status

- Public repository and MIT license: present.
- Native Cursor manifest, committed logo, remote MCP configuration, and rule:
  present; the nested package is listed in the root marketplace manifest.
- Plugin instructions now use the server's real tool names and disclose memory
  writes, optional trail capture, workspace changes, and indexed-data limitations.
- Metadata, local configuration, and component checks pass locally. CI runs
  `node scripts/validate-template.mjs` on pull requests and pushes to `main`.
- Live unauthenticated checks on 2026-09-30: `GET /mcp` returned 401 with a
  `WWW-Authenticate` link to protected-resource metadata; both discovery documents
  returned 200. Metadata advertises dynamic client registration, authorization
  code and refresh-token grants, and PKCE S256.
- Backend source already allows Cursor's desktop loopback, native URI scheme, and
  documented web callback. No backend patch was needed for these checks.
- **Still required:** local Cursor loading and an authenticated end-to-end smoke
  test. The desktop controller became unavailable before discovery could be
  verified; the public HTTP checks do not prove tool calls or OAuth completion.
- **Still required:** publisher sign-in and submission. The application page
  displayed "Sign in to apply"; account-specific fields, existing applications,
  publisher verification, and any additional requirements were not visible.

## Prepared listing copy

Use this copy in the corresponding fields the signed-in application presents.
Field names and additional requirements may differ.

| Item | Value |
| --- | --- |
| Plugin name / identifier | Graphify / `graphify` |
| Publisher | Graphify Labs |
| Public repository | https://github.com/Graphify-Labs/graphify-cursor-plugin |
| Default branch | `main` (merge the preparation PR before submission) |
| Package directory | `plugins/graphify` |
| Marketplace manifest | `.cursor-plugin/marketplace.json` |
| Plugin manifest | `plugins/graphify/.cursor-plugin/plugin.json` |
| Version | `0.1.1` |
| Website | https://graphify.com |
| Support | founders@graphify.com |
| Issue tracker | https://github.com/Graphify-Labs/graphify-cursor-plugin/issues |
| Privacy policy | https://graphify.com/privacy |
| Terms | https://graphify.com/terms |
| Source license | MIT |
| Logo | `plugins/graphify/assets/logo.png` |
| MCP endpoint | `https://api.graphify.com/mcp` |
| Authentication | Browser-based OAuth with dynamic registration and PKCE; no bundled API key |
| Suggested category | Developer tools / code intelligence, if offered |

**Short description**

Search indexed code, trace dependencies, assess change impact, and retrieve
repository memory through Graphify's authenticated MCP server.

**Long description**

Graphify connects Cursor to a knowledge graph of your indexed repositories.
Find symbols and callers, trace dependency paths, explore potential change impact,
locate linked tests, and retrieve saved repository decisions with source context.
The included rule guides repository selection, evidence-based answers, and checks
against local files before edits.

A Graphify account and an indexed repository are required. Sign in through OAuth
and choose an authorized workspace and repository. Results reflect the indexed
snapshot; graph analysis does not run tests or prove runtime behavior. Some tools
persist repository memory, optional query trails, or workspace preferences, as
described in their live schemas and the plugin README.

**Release notes**

Updated the plugin for the current Graphify MCP tools. Added workspace-selection
guidance, accurate persistence disclosures, installation and troubleshooting
instructions, publisher links, and automated package validation.

## Local smoke test

Use an account with a small, indexed repository containing no sensitive reviewer
data. Keep Cursor's normal approval prompts enabled. Run from this repository's
root on macOS/Linux:

```bash
node scripts/validate-template.mjs
plugin_dest="$HOME/.cursor/plugins/local/graphify"
if [ -e "$plugin_dest" ] || [ -L "$plugin_dest" ]; then
  echo "Existing local Graphify plugin found; inspect it before replacing it."
else
  mkdir -p "$HOME/.cursor/plugins/local"
  cp -R plugins/graphify "$plugin_dest"
fi
```

Copy the actual package, including its hidden `.cursor-plugin` directory. Do not
symlink to a directory outside the plugin folder. Reload Cursor, then inspect
Customize for the Graphify rule and MCP server. An installed marketplace version
with the same name may take precedence; managed accounts may restrict local imports.

Record the Cursor version, tested commit, workspace/repository used privately,
and pass/fail results:

| Check | Steps and expected result |
| --- | --- |
| Package loading | Reload Cursor; Graphify's rule and MCP connection appear without parse errors. |
| OAuth | Connect Graphify from MCP settings; complete sign-in and return to Cursor; tools become available. |
| Repository scope | Ask "List my Graphify workspaces and indexed repositories." Confirm only authorized scope is returned and choose the intended repository. |
| Code evidence | Ask "Using Graphify, locate a known symbol in this repository and cite its file." Check the answer against an actual indexed file. |
| Dependencies | Ask for callers or a path between two known connected symbols. Confirm the returned direction and evidence. |
| Limitations | Ask for a deliberately nonexistent symbol. The agent reports missing evidence without inventing a result. |
| Memory and approvals | Read existing repository memory. Only test `remember` if intentionally saving a test note; verify the returned save/review status. Do not change workspace unless intended. |

Do not claim these authenticated tests passed until they have been performed.
Keep private repository IDs, query results, tokens, and reviewer credentials out
of public issues, screenshots, and this repository.

## Final publisher steps

1. Merge the preparation PR after reviewing its changes and CI result.
2. Complete the local smoke test above, fixing any authentication or discovery
   issue before submitting.
3. Sign in at [Cursor's publisher application](https://cursor.com/marketplace/publish).
   Confirm whether Graphify already has an application before creating another.
   Submit this public repository URL and use the listing copy above where relevant.
   The root marketplace manifest identifies the nested plugin package.
4. Complete any publisher verification, terms, or reviewer access requests shown
   in the portal. If a demo or test account is requested, provide it through
   Cursor's private review channel; do not commit credentials. A short recording
   of connection, repository selection, and one cited answer is useful preparation,
   but was not confirmed as a mandatory form field.
5. Retain the submission reference and track review in the publisher account.
   Public availability depends on Cursor review; pushing or merging alone does
   not publish a listing.

## Sources

- [Cursor plugin reference](https://cursor.com/docs/reference/plugins): manifests,
  component paths, multi-plugin repositories, and submission checklist.
- [Cursor plugin setup](https://cursor.com/docs/plugins): local loading and installation.
- [Cursor MCP documentation](https://cursor.com/docs/mcp): remote HTTP and OAuth.
- [Cursor marketplace security](https://cursor.com/help/security-and-privacy/marketplace-security):
  public source, licensing, and review.
- [Graphify protected-resource metadata](https://api.graphify.com/.well-known/oauth-protected-resource)
  and [authorization-server metadata](https://api.graphify.com/.well-known/oauth-authorization-server):
  public discovery checks.
