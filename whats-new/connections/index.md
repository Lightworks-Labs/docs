---
title: Linear and Jira Connections
date: May 22, 2026
---

Reference [Linear](https://linear.app) and [Jira](https://www.atlassian.com/software/jira) issues directly inside your records. Authorize either provider from **Settings → Connections** with a single OAuth click — connections are workspace-wide and read-only.

Once connected, two new entry points appear:

- A **property type** on any database — store a single issue reference per record, with status and assignee fetched live whenever the record loads
- A **slash command** in the editor — `/Linear ticket` or `/Jira issue` inserts an inline chip that links to the issue, with a hover preview showing title, status, and description

Disconnect at any time from the same Settings tab; existing references remain as static ids until the connection is restored.
