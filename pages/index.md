---
title: Pages
---

A page is a markdown document stored in your connected GitHub repository. Pages can contain rich content, structured properties, and nested sub-pages — all version-controlled as plain files.

## File structure

Pages are stored as folders containing an `index.md` file:

```
products/
├── overview/
│   └── index.md
├── api-reference/
│   └── index.md
│   └── authentication/
│       └── index.md
```

This folder-based structure lets pages contain sub-pages naturally through nesting.

## Nesting

Pages nest by folder hierarchy. The sidebar shows the full tree with expandable folders. A chevron indicates a folder has children — click it to expand or collapse.

You can nest pages as deeply as needed. Sub-pages can be created from within the editor using the `/Page` slash command, which inserts a link to a new page and opens it for editing.

Pages can also be moved between folders by dragging them in the sidebar, or via the **Move To** option in the page actions menu.

## Creating a page

Click the `+` button next to any folder in the sidebar, or navigate to **New page** from the command palette. You can also type `/Page` in any editor to create a sub-page inline.

When creating a page:

1. Enter a title — Lightworks generates a URL-safe slug from it
2. Optionally pick a destination folder
3. Optionally start from a template if the folder has record templates

The page is created as a folder bundle (`{slug}/index.md`) in your repository.

## Editing

Pages open in a WYSIWYG markdown editor. Everything you see is what gets stored as markdown — headings, lists, tables, code blocks, callouts, columns, task lists, math equations, and more.

Use the **slash command menu** (`/`) to insert any block type. A drag handle appears on the left of each block for reordering.

### Committing changes

Edits are not saved automatically. When you're ready, click **Commit** in the toolbar. Lightworks creates a pull request on a draft branch with your changes. You can continue editing and committing to the same PR until you're ready to merge.

If **Auto-commit on navigation** is enabled in your account settings, changes are committed automatically when you navigate away.

## Properties

Properties are structured metadata stored as YAML frontmatter at the top of the page file. They appear above the editor as a set of editable fields.

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
| `esign` | Electronic signature with MFA verification |

### Editing properties

Click any property value to edit it inline. To change a property's type, click its label and select a type from the menu. To add a new property, click the **+** button at the bottom of the properties section.

Property order can be changed by dragging rows. New properties added to a page are automatically reflected in the parent database's schema on the next commit.

### Schema

If a page lives inside a database (a folder with a `_schema.json`), its properties are defined by that schema. The schema controls which properties appear, their types, and their options. Standalone pages can also have ad-hoc properties without a schema.

## Page history

Every change to a page is tracked as a git commit in your repository. Open the `...` menu and click **View history** to see the full commit history for that page.

The history modal lists each commit with its message, author, and timestamp. Click any entry to open a read-only view of the page exactly as it was at that point in time — content, properties, and all. This lets you review what changed between versions or recover content from an earlier state.

History is per-page, not per-repository — only commits that touched the current page's file are shown.

## Page actions

Open the `...` menu in the toolbar or right-click a page in the sidebar to access:

| Action | Description |
|---|---|
| **View history** | Shows the git commit history for this page |
| **Duplicate** | Creates a copy of the page |
| **Export to PDF** | Downloads the page as a PDF |
| **Move to** | Moves the page to a different folder |
| **Archive** | Moves the page to the Archive folder (reversible) |
| **Delete** | Permanently deletes the page — only available for archived pages |
| **Favorite** | Pins the page for quick access |
