---
title: GRAPH — interactive relationship visualization
date: '2026-05-20'
---

# GRAPH — interactive relationship visualization

The Console now supports a new `GRAPH` query type that renders your database relationships as an interactive 3D force-directed graph.

## Two modes

**`GRAPH *`** loads every database in your repository and displays all records — including those with no relationships. Use it to explore the full shape of your data at a glance.

**`GRAPH db → db → db`** restricts to the listed databases and shows only records that are connected along that path, filtering out isolated nodes.

```lql
-- Full repo overview
GRAPH *

-- Focused relationship path
GRAPH requirements → tests → results
AT BRANCH 'main'
```

## Interacting with the graph

- **Click a node** to select it — it pulses and enlarges to stand out
- **Click a collection** in the legend to toggle its nodes on or off without restarting the layout
- **Shift-click** a pinned node to release it

## Relation fields in table view

Relation fields in database tables now show the linked record's title with a page icon, rather than a raw ID or count. Hover over the table cell to see all linked records for that row.
