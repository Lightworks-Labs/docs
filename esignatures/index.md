---
title: eSignatures
---

eSignatures let you capture legally binding electronic signatures directly within Lightworks records. Each signature is verified with multi-factor authentication and permanently stored in the document's frontmatter.

## How it works

An `esign` property captures who signed, when, and under what session context. When a collaborator signs, Lightworks:

1. Prompts for TOTP verification (6-digit authenticator code)
2. Records the signature with full metadata
3. Commits the updated frontmatter to the repository

The signature is stored as a structured value in the record's `index.md`:

```yaml
approval:
  login: jsmith
  name: Jane Smith
  serverTimestamp: "2025-11-04T18:32:00Z"
  hash: "sha256:a3f9..."
  prNumber: 42
  userId: user_abc123
  sessionId: sess_xyz789
  mfaMethod: totp
  ipAddress: "203.0.113.5"
  userAgent: "Mozilla/5.0 ..."
```

## Signing contexts

### PR merge signing

When a pull request is configured to require signatures before merge, reviewers see a **Sign & Merge** button. Clicking it opens the MFA modal — enter your 6-digit authenticator code to confirm. On success, the signature is written to the record and the PR is merged in one atomic operation.

### Independent record signing

Any `esign` property in a record can also be signed independently, outside of a PR. Open the record, click the unsigned signature chip, and complete MFA verification. The change is committed directly to the branch.

## Rendering

- **Unsigned** — shown as a faint placeholder chip with a pen icon
- **Signed** — displays the signer's name in Dancing Script (cursive) alongside their GitHub avatar, with a colored background indicating completion
- **Hover card** — shows full metadata: signer login, timestamp, SHA-256 hash, MFA method, and associated PR number

## Adding an eSign property

In the schema editor, add a property with type `esign`. You can also add multiple `esign` properties to a single record to capture sequential approvals (e.g. author → reviewer → quality manager).

When a record is signed via PR merge for the first time, Lightworks automatically adds the `esign` property to the database schema if it isn't already present.

## Compliance

eSignatures in Lightworks are designed to meet the requirements of:

- **ESIGN Act** (U.S. Electronic Signatures in Global and National Commerce Act)
- **UETA** (Uniform Electronic Transactions Act)

Each signature record includes the signer's intent, identity verification via MFA, timestamp, and tamper-evident hash — satisfying the core requirements for electronic signature validity in regulated industries.

## Plan requirement

eSignatures require the **Quality Manager** plan or higher. The `esign` property type is not available on free or standard plans.
