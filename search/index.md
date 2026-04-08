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

Full-text content search requires search indexing to be installed in your repository. If indexing is not set up, title-only search still works. A setup prompt appears inside the palette when indexing is not active.
