---
title: Releases
---

Releases create a versioned GitHub release from your repository content, with optional PDF artifacts attached.

## Creating a release

Open **Releases** from the sidebar and click **Create Release**. Fill in the release title, version, and release notes, then publish or save as a draft.

## Versioning

Lightworks suggests the next semver version based on your most recent published release. Use the slider to pick between:

- **Patch** — internal milestone or minor clerical update
- **Minor** — design review checkpoint or release candidate
- **Major** — regulatory submission (510(k), CE Mark, etc.)

You can also type a version manually in the version field.

## Release types

| Option | Description |
|---|---|
| Save as draft | Release is saved to GitHub but not published — publish manually from GitHub when ready |
| Pre-release | Marks the release as not production-ready |

## Artifacts

When creating a DHF release, Lightworks generates PDF artifacts and attaches them to the GitHub release:

- **Design History File** — a compiled PDF of your document scope, always included
- **Traceability Matrix** — requires the Traceability Matrix bundle to be installed
- **Software Bill of Materials** — CycloneDX JSON format, requires the SBOM bundle to be installed

## Document setup

The DHF PDF cover page is populated from the document setup fields: title, subtitle, product, company, document number, and author. Lightworks pre-fills company, product, and author from your connected repository and Clerk profile.

## Table of contents scope

By default the DHF includes all documents in the repository. Use the **Table of Contents** accordion to select a specific subset of pages to include.
