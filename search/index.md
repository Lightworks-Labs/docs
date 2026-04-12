---
title: Search
---

Open search with **Cmd+K** (Mac) or **Ctrl+K** (Windows/Linux). Search covers documents, database records, and workspace commands at once.

## Basic search

Type any word or phrase to search across all page titles and content in your connected repository. Results are grouped into Documents, Records, and Commands.

## Advanced syntax

Append operators to narrow results:

| Operator | Example | Description |
|---|---|---|
| `type:` | `type:page` | Filter to pages only |
| `type:` | `type:record` | Filter to database records only |
| `in:` | `in:title` | Search page titles only |
| `in:` | `in:content` | Search page content only |
| `property:` | `property:status=open` | Filter records by a property value |
| `repo:` | `repo:my-repo` | Limit to a specific connected repository |

Operators can be combined. For example:

```
type:record property:status=open in:title review
```

When advanced syntax is detected, an **Advanced** badge appears in the search bar.

## Recent searches

The five most recent searches are shown when the palette opens with no query typed.

## Search indexing

Full-text content search requires a search index to be installed in your repository. If indexing is not set up, title-only search still works. A setup prompt appears inside the palette when indexing is not active.

### Installing the indexer

Click **Set up indexing** in the search palette. Lightworks opens a pull request that adds a GitHub Actions workflow to your repository. Merge the PR — the workflow runs immediately and builds the initial index. From that point on, indexing runs automatically on every push to your main branch.

### How indexing works

The indexer scans all markdown files in your repository (or subfolder, if configured) and writes the index back to `.lightworks/search/` as a set of JSON files committed by the automation bot. The index is stored in your repository — Lightworks never copies your content to an external service.

Each file is indexed with its title, full content, and frontmatter property values. Titles are weighted higher than content in search results, and property values are weighted higher than body text, so structured records surface accurately.

Large files are split into chunks of 100 lines for efficient snippet generation — only the relevant chunk is fetched when you open a result.

### Index freshness

An index health badge in the search palette shows the current status:

- **Healthy** — the index reflects the latest commits on main
- **Stale** — new commits have landed since the last index run; results may be behind
- **Not installed** — the workflow has not been set up yet

If the index is stale, click **Trigger reindex** to kick off the workflow manually without waiting for the next push.

The index is considered stale when a user commit on main is newer than the last successful indexer run. Automation commits (the `chore: update search index` commits from the bot itself) are excluded from this check so they don't count as new content.

### What gets indexed

- All `.md` files up to 1 MB
- Page title, body content, and frontmatter property values
- Both pages and database records

Files in `node_modules/`, `.git/`, and `vendor/` are excluded automatically.

---

## Structured queries

Search is designed for quick lookup. For filtering records by property values, joining databases, or querying your data at a point in time, use the [Console](/docs/console) — it supports full [LQL](/docs/console) queries with WHERE clauses, JOINs, and temporal snapshots.
