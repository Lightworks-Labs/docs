---
title: Console
---

The Console is an interactive query environment for your Lightworks databases. It uses **LQL (Lightworks Query Language)** — a SQL-like language designed for querying structured markdown records and tracing relationships across databases.

Open the Console from the sidebar under **Console**.

---

## Running a query

Type a query in the editor and press **⌘↵** (Mac) or **Ctrl↵** (Windows) to run it, or click the **Run** button.

Results appear below the editor with a metadata banner showing the branch, resolved commit, row count, and execution time.

---

## LQL syntax

### SELECT

```lql
SELECT *
FROM requirements

SELECT id, title, status, assignee
FROM requirements
```

Use `*` to return all fields, or name specific fields to narrow the result.

### WHERE

```lql
SELECT *
FROM requirements
WHERE status = 'active'

SELECT *
FROM requirements
WHERE status IN ('active', 'in-review')
AND assignee IS NOT NULL

SELECT *
FROM tests
WHERE title CONTAINS 'authentication'
```

**Operators**

| Operator | Description |
|---|---|
| `=` `!=` `>` `<` `>=` `<=` | Comparison |
| `IN (...)` | Match any value in a list |
| `IS NULL` / `IS NOT NULL` | Presence check |
| `CONTAINS 'text'` | Full-text search across field values and record content |
| `AND` / `OR` / `NOT` | Logical grouping |

### ORDER BY and LIMIT

```lql
SELECT *
FROM requirements
ORDER BY updated_at DESC
LIMIT 25
```

### JOIN

Link two databases on a shared relation field. If both schemas have a matching `relation` or `relation[]` field pointing to each other, the `ON` clause can be omitted — LQL will resolve it automatically from the schema.

```lql
-- Explicit ON clause
SELECT r.id, r.title, t.id AS test_id, t.status
FROM requirements r
LEFT JOIN tests t ON r.linked_tests CONTAINS t.id

-- Schema-derived (ON omitted)
SELECT r.title, t.title, t.status
FROM requirements r
LEFT JOIN tests t
WHERE t.status = 'passed'
```

Use `LEFT JOIN` to include requirements even when no matching test exists — those rows show a **GAP** indicator.

### TRACE

Trace a relationship chain across multiple databases in order:

```lql
TRACE requirements → tests → results
```

LQL walks the relation graph defined in your `_schema.json` files and returns every linked record across the chain. Useful for end-to-end traceability audits.

---

## Temporal queries

Query your data at a point in time using `AT`, or compare two snapshots with `BETWEEN`.

### AT

```lql
-- By date/time
SELECT * FROM requirements
AT '2025-01-15'

-- By commit SHA
SELECT * FROM requirements
AT COMMIT 'abc1234'

-- By branch tip
SELECT * FROM requirements
AT BRANCH 'release/v2.0'

-- By tag
SELECT * FROM requirements
AT TAG 'v1.0.0'
```

LQL resolves the expression to the nearest commit at or before the specified point, then runs the query against the index at that SHA.

### BETWEEN

Compare two snapshots and return only records that changed (added, removed, or modified):

```lql
SELECT id, title, status
FROM requirements
BETWEEN '2025-01-01' AND '2025-03-01'

SELECT *
FROM requirements
BETWEEN TAG 'v1.0.0' AND TAG 'v2.0.0'
```

Each row includes a `change` column: `added`, `removed`, or `modified`.

---

## Schema browser

The **Schema** tab in the left sidebar shows all databases detected in your connected repository. Each database lists its fields and types. Click a field or database name to insert it into the editor at the cursor position.

**Field types**

| Type | Description |
|---|---|
| `text` | Free-form text |
| `number` | Numeric value |
| `select` | Single-choice option |
| `status` | Status with todo / in-progress / complete categories |
| `multi-select` | Multiple-choice options |
| `date` | Date or datetime |
| `url` | URL string |
| `checkbox` | Boolean true/false |
| `person` | Single GitHub collaborator login |
| `people` | Multiple GitHub collaborator logins |
| `esign` | Electronic signature |
| `relation` / `relation[]` | Link to one or many records in another database |

---

## Saved queries

Click **Commit** (or **Save** depending on your role settings) to save the current query. You can store it as a **draft** (saved to your account, not in the repo) or **commit it to the repository** at `.lightworks/queries/<name>.lql`.

Committed queries are versioned alongside your data and visible to everyone with repo access. Drafts are private to your account.

The **Saved** tab in the left sidebar lists all queries. Click any query to load it into the editor.

---

## Exporting results

Use the **⋯** menu in the top-right to export results:

| Format | Description |
|---|---|
| **Download CSV** | Comma-separated file with query metadata header |
| **Download JSON** | JSON object with `meta` and `results` arrays |
| **Copy as Markdown** | GitHub-flavored markdown table, ready to paste into a doc or PR |

---

## Examples

```lql
-- All open requirements assigned to a specific person
SELECT id, title, status, priority
FROM requirements
WHERE status != 'closed'
AND assignee = 'octocat'
ORDER BY priority DESC

-- Tests that haven't been linked to any requirement
SELECT t.id, t.title, t.status
FROM tests t
LEFT JOIN requirements r
WHERE r.id IS NULL

-- Requirements added since the last release
SELECT id, title, status, change
FROM requirements
BETWEEN TAG 'v1.2.0' AND BRANCH 'main'

-- Full traceability chain at a point in time
TRACE requirements → tests → results
AT TAG 'v2.0.0'

-- Text search across all test records
SELECT *
FROM tests
WHERE CONTAINS 'login flow'
LIMIT 20
```
