---
title: MCP Server
---

Lightworks MCP ([Model Context Protocol](https://modelcontextprotocol.io)) server lets AI agents — Claude, ChatGPT, Cursor, Copilot, and any other MCP-compatible client — read and edit your QMS records on your behalf. The server speaks the same record, database, and PR model as the [REST API](/docs/developers), so writes still flow through a pull request for human review and signature.

## Endpoint

```
https://app.lightworks.md/api/mcp
```

## Authentication

Authenticate with a Lightworks API token sent as `Authorization: Bearer <token>`. Generate one from **Settings → API Tokens**:

- **Read** tokens (`read:databases`) work on every plan
- **Write** tokens (`read:databases` + `write:databases`) require the paid plan

Tokens can be revoked or reactivated at any time. The workspace's `LIGHTWORKS_ORG_ID` is shown at the bottom of the API Tokens tab.

## Connecting a client

Open **Settings → MCP Server** to copy a ready-made config for your client.

### Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "lightworks": {
      "url": "https://app.lightworks.md/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN"
      }
    }
  }
}
```

### Cursor, Claude Code, VS Code

The Settings → MCP Server panel includes a tab for each client with the exact file path and snippet to paste.

## Available tools

### Discovery (read)

| Tool | Description |
|---|---|
| `list_databases` | List QMS databases visible in the connected repo. Returns `id`, `name`, and `parent` for each. Paginated. |
| `get_database` | Fetch a single database by its path. Returns `name`, `parent`, and the full property schema. Example id: `"reqs"` or `"parent-db/child-db"`. |
| `list_records` | List records directly under a database. Sub-database records are not included; query their database separately. Paginated. |
| `get_record` | Fetch a single record by id. Returns its properties and markdown body. Example: `{ db: "reqs", id: "REQ-12" }`. |

### Records (write)

| Tool | Description |
|---|---|
| `create_record` | Create a new record on a PR branch. **Not idempotent** — every call mints a new record id. Requires an open PR from `create_pr` and a properties object that satisfies the database schema. |
| `update_record` | Update an existing record on a PR branch by merging the provided properties into its frontmatter. Idempotent — repeating the same call results in the same final state. |
| `delete_record` | Delete a record on a PR branch. Idempotent — deleting a record that no longer exists returns `404` without side effects. |

### Pull requests (write)

| Tool | Description |
|---|---|
| `create_pr` | Create a Lightworks API PR branch. Returns a `prId` you pass to `create_record` / `update_record` / `delete_record`, then `finalize_pr`. Idempotent only when `originatorContext.pr_number` is provided (deterministic prId `pr-{prNumber}`); without it each call mints a fresh random prId. |
| `cancel_pr` | Cancel an API PR: close any open GitHub PR and delete the branch. Idempotent — calling on an already-cancelled PR succeeds silently. |
| `finalize_pr` | Open a GitHub pull request for the API PR branch. Idempotent — returns the existing PR if one is already open. |

## Typical flow

Writes are grouped into a PR so a human can review and sign before anything lands on the default branch.

1. **`create_pr`** → get a `prId` and branch
2. **`create_record`** / **`update_record`** / **`delete_record`** — each call commits to the branch
3. **`finalize_pr`** → opens a GitHub pull request in your QMS repo for review and signature
4. **`cancel_pr`** if you change your mind → closes the branch without merging

For one-off reads, you can call the discovery tools directly — no PR required.

## Related

- [Developers](/docs/developers) — the underlying REST API and GitHub Actions workflow
- [Settings](/docs/settings) — generate API tokens and find your `LIGHTWORKS_ORG_ID`
- [Pull Requests](/docs/pull-requests) — review, comment, and merge changes opened by an agent
