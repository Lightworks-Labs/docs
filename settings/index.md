---
title: Settings
section: Platform
sectionOrder: 3
order: 3
---

Settings is a modal with two sections — **Account** (user-level) and **Workspace** (org-level) — accessible from the gear icon at the bottom of the sidebar.

## Account

### Account

Your profile pulled from Clerk and GitHub: avatar, full name, primary email, GitHub handle, company, location, and website.

### Preferences

User preferences stored locally:

- **Auto-commit on navigation** — automatically commits unsaved changes when you navigate away from a record
- **Appearance** — Light, Dark, or System theme

### Security

Manage the second factors used for MFA-gated actions — API token operations, eSignature capture, and other sensitive operations.

- **Authenticator apps** — enroll a TOTP authenticator (1Password, Authy, Google Authenticator, etc.); each enrolled app appears in the list with a delete action
- **Text message (SMS)** — register a phone number as a fallback factor; marked **Less secure** since SMS is more susceptible to interception and SIM-swap attacks
- **Backup codes** — generate a one-time set of recovery codes you can use if you lose access to your other factors; the refresh icon regenerates the set
- **Test** — verify your strongest configured factor end-to-end without performing a real action

### Notifications

Per-event email notification preferences. Each row toggles between **Off** and **Email**.

- **Review requests** — receive an email when someone requests your review on a pull request

### Language

Customize the terminology Lightworks uses throughout the interface. Changes apply only to you.

- **Preset** — toggle between Technical (commit, branch, pull request) and Non-Technical (revision, draft, change request) modes
- **Custom terms** — edit individual terms grouped by domain: Version Control, Workflow & Actions, Document & File

A blue dot marks any term you've customized. Click a term to edit it inline; click the reset icon to restore the default.

### Keyboard

View and remap keyboard shortcuts. Shortcuts are grouped by category (Global, Editor, …) and listed with their current key binding.

- **Remap** — click any shortcut row to enter a new key combination
- **Reset** — revert an individual shortcut or all overrides to defaults

## Workspace

### General

Workspace identity and the GitHub organization it's bound to.

- **Logo** — PNG or JPG up to 10MB; shown in the sidebar and on PDF exports
- **Organization name** — display name for the workspace; click Save to apply
- **GitHub organization** — read-only chip showing the connected GitHub org

### Members

Manage access to your connected GitHub repositories. All plans include unlimited seats — invite your entire team without worrying about per-seat costs. If your workspace has more than one connected repo, a repo selector appears at the top.

- **Add a member** — enter a GitHub username, choose a role (Read / Write / Admin), and click Add
- **Change a role** — click the role toggle on any collaborator row to update it immediately
- **Remove a member** — click the X icon on a collaborator row

Roles map to GitHub collaborator permission levels: Read (pull), Write (push), Admin.

### Signatures

Workspace-wide defaults for how [eSignatures](/docs/esignatures) are captured and rendered. Available on the Quality Manager plan and above.

- **Typeface** — cursive font used to render a signed name (default Satisfy); the preview updates as you change it
- **Attestation** — the legal statement a signer agrees to when signing; defaults to "I agree that this electronic signature is legally binding and represents my intent to sign this document."
- **Signature action** — how a signer commits a signature: **Code** requires a TOTP code from a configured authenticator, **Click** captures the signature on a single click after attestation

### API Tokens

Generate tokens for programmatic access to the Lightworks API. All token operations require MFA verification.

- **Generate** — enter a token name and click Generate; the raw token is shown once in a copy prompt
- **Revoke** — disables a token; it can be reactivated later
- **Delete** — permanently removes a revoked token

The workspace's `LIGHTWORKS_ORG_ID` is shown at the bottom of this tab with a copy button.

### Connections

Authorize and manage third-party integrations. See [Connections](/docs/connections) for what each one enables.

- **Connect** — start the provider's OAuth flow; on success the row flips to **Connected** with the authorizing user, the date, and the granted scopes
- **Disconnect** — revoke Lightworks' stored OAuth token; existing references in records are preserved but stop fetching live metadata until reconnected

Only workspace admins can connect or disconnect a provider. Linear and Jira are available today, both with `read` scope.

### MCP Server

Connect AI agents like Claude, Cursor, and VS Code Copilot to read and edit your QMS records. The tab shows the endpoint URL, authentication instructions, and a per-client config snippet (Claude Desktop, Cursor, Claude Code, VS Code). See [MCP Server](/docs/mcp) for the full reference.

### Billing

View your current plan and manage your subscription.

- **Plan** — shows your active plan (Free, Quality Manager, or Enterprise) with its included features
- **Change Plan** — opens the plan selection page
- **Billing Portal** — opens the Stripe portal for invoices, payment methods, and billing history
- **AI Token Balance** — shown if your plan includes an AI add-on; displays current balance, monthly deposit amount, and next deposit date

### Trust

Compliance and legal documents for the platform. Each row links to a downloadable artifact or live page; items still being completed are marked **In process**.

- **Compliance & legal documents** — Status (real-time system status and incident history), Privacy Policy, Terms of Service, SOC 2 Type II Report, GDPR Data Processing Addendum
- **Data subprocessors** — security and compliance documentation for services powering Lightworks
