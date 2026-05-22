---
title: Connections
---

Connections link Lightworks to external tools so you can reference their data inside your records. Each connection is authorized once per workspace via OAuth and then becomes available as both a property type on any database and a slash command inside the editor.

## Available connections

| Connection | Reference | Scopes |
|---|---|---|
| Linear | Issues across your Linear workspace | `read` |
| Jira | Issues across your Jira sites | `read` |

Both connections are read-only — Lightworks fetches issue metadata for display but never writes back to Linear or Jira.

## Connecting

Open **Settings → Connections** and click **Connect** next to the provider. You'll be redirected to the provider's OAuth consent screen, where you grant Lightworks the scopes listed above. After approving, you're returned to Lightworks and the connection is marked **Connected** with the authorizing user and date.

A connection is workspace-wide — once any member authorizes it, every member of the workspace can use the property type and slash command. The authorizing user's grant is what Lightworks uses to fetch data.

## As a property type

Add a Linear or Jira property to a database from the schema editor (see [Databases](/docs/databases) for the full list of property types). The property stores a reference to a single issue.

To set the value on a record, click the property and start typing — Lightworks searches the connected workspace and shows matching issues with their id, title, and status. Selecting one stores the issue reference in the record's frontmatter.

The rendered cell shows the issue id and current status. Hovering opens a preview card with the title, status, assignee, last-updated timestamp, and description. Clicking the external-link icon opens the issue in Linear or Jira.

Issue metadata is fetched live from the provider whenever the record is loaded — status changes in Linear or Jira appear in Lightworks without any sync step.

## As a slash command

Inside any document body, type `/` and choose the connection command to insert an inline reference:

- `/Linear ticket` — pick a Linear issue and insert a chip linking to it
- `/Jira issue` — pick a Jira issue and insert a chip linking to it

Inline chips behave the same as property cells — hover for a preview card, click the external-link icon to jump to the source.

## Disconnecting

From **Settings → Connections**, click **Disconnect** next to a connected provider. Lightworks revokes the stored OAuth token and the connection returns to its **Connect** state.

Existing property values and inline chips are preserved as plain references — the id and last-known title remain visible, but live metadata (status, assignee, description) no longer loads until the connection is restored. Reconnecting at any time resumes live data without re-entering values.

## Permissions

Only workspace admins can authorize or disconnect a connection. Any member can read property values and insert inline references once a connection exists.
