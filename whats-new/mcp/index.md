---
title: MCP Server
date: May 18, 2026
---

Lightworks MCP ([Model Context Protocol](https://modelcontextprotocol.io)) server, lets AI agents like Claude, Cursor, and VS Code Copilot read and edit your QMS records on your behalf. Connect a client in **Settings → MCP Server** with a Lightworks API token.

Ten tools are available — `list_databases`, `get_database`, `list_records`, `get_record` for discovery; `create_record`, `update_record`, `delete_record` for record edits; and `create_pr`, `cancel_pr`, `finalize_pr` to group writes into a reviewable pull request. Reads work on every plan; writes require the paid plan and a token with `write:databases`.
