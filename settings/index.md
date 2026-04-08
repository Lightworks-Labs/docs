---
title: Settings
---

Settings is a modal with two sections — **Workspace** (org-level) and **Account** (user-level) — accessible from the gear icon at the bottom of the sidebar.

## Workspace

### Members

Manage access to your connected GitHub repositories. If your workspace has more than one connected repo, a repo selector appears at the top.

- **Add a member** — enter a GitHub username, choose a role (Read / Write / Admin), and click Add
- **Change a role** — click the role toggle on any collaborator row to update it immediately
- **Remove a member** — click the X icon on a collaborator row

Roles map to GitHub collaborator permission levels: Read (pull), Write (push), Admin.

### API Tokens

Generate tokens for programmatic access to the Lightworks API. All token operations require MFA verification.

- **Generate** — enter a token name and click Generate; the raw token is shown once in a copy prompt
- **Revoke** — disables a token; it can be reactivated later
- **Delete** — permanently removes a revoked token

The workspace's `LIGHTWORKS_ORG_ID` is shown at the bottom of this tab with a copy button.

### Billing

View your current plan and manage your subscription.

- **Plan** — shows your active plan (Free, Quality Manager, or Enterprise) with its included features
- **Change Plan** — opens the plan selection page
- **Billing Portal** — opens the Stripe portal for invoices, payment methods, and billing history
- **AI Token Balance** — shown if your plan includes an AI add-on; displays current balance, monthly deposit amount, and next deposit date

## Account

### Account

Your profile pulled from Clerk and GitHub: avatar, full name, primary email, GitHub handle, company, location, and website.

### Settings

User preferences stored locally:

- **Auto-commit on navigation** — automatically commits unsaved changes when you navigate away from a record
- **Appearance** — Light, Dark, or System theme

### Keyboard

View and remap keyboard shortcuts. Shortcuts are grouped by category and listed with their current key binding.

- **Remap** — click any shortcut row to enter a new key combination
- **Reset** — revert an individual shortcut or all overrides to defaults

### Language

Customize the terminology Lightworks uses throughout the interface. Changes apply only to you.

- **Preset** — toggle between Technical (commit, branch, pull request) and Non-Technical (revision, draft, change request) modes
- **Custom terms** — edit individual terms grouped by domain: Version Control, Workflow & Actions, Document & File

A blue dot marks any term you've customized. Click a term to edit it inline; click the reset icon to restore the default.
