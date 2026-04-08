---
title: Databases
---

A database is a folder in your repository that contains a schema and a collection of records. Each record is a markdown document with structured properties defined by the schema.

## File structure

```
requirements/
├── _schema.json       # defines properties and views
├── req-001/
│   └── index.md       # a record
├── req-002/
│   └── index.md       # a record
```

The `_schema.json` file defines the property types, default views, and view configuration for the database. Records are sub-folders, each with their own `index.md` containing frontmatter properties and a markdown body.

## Property types

| Type | Description |
|---|---|
| `text` | Plain string value |
| `number` | Numeric value — supports number, currency, and percent formats |
| `select` | Single choice from a defined list of options |
| `multi-select` | Multiple choices with color coding |
| `status` | Select with built-in categories: to-do, in-progress, complete |
| `date` | ISO 8601 date, with optional time component |
| `checkbox` | Boolean true/false |
| `url` | Web or relative URL |
| `person` | Single GitHub collaborator |
| `people` | Multiple GitHub collaborators |
| `relation` | Link to a record in another database |
| `esign` | Electronic signature with login, name, and timestamp |

## Views

Each database supports multiple named views. Switch between them using the view switcher in the toolbar. Views are stored in the schema and persist across sessions.

| View | Description |
|---|---|
| Table | Columnar grid with sortable columns and inline filtering |
| List | Compact list with inline property display |
| Board | Kanban columns grouped by a status or select field |
| Gallery | Card grid — cover image from the first URL property |
| Feed | Chronological feed |

## Creating a record

Open a database and click **New** to create a record. A new folder is created with an `index.md` pre-populated with default property values from the schema.

## Sorting and filtering

In Table view, click a column header to sort ascending or descending. Use the filter input to search by any column value. Column order and visibility can be configured per view.

## Board grouping

Board view groups records by a `status` or `select` property. Lightworks auto-detects the best grouping field from the schema. Drag cards between columns to update that property — the change is committed to the record's frontmatter.
