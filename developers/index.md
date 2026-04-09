---
title: Developers
---

Lightworks provides API tokens for programmatic access to your workspace. Tokens are used by GitHub Actions workflows in Premium templates and will power the record API coming soon.

## Credentials

You need two values from **Settings → API Tokens**:

- `LIGHTWORKS_ORG_ID` — your organization's unique identifier, shown at the bottom of the API Tokens tab
- `LIGHTWORKS_API_TOKEN` — a token you generate from the same tab

Store both as secrets in your CI environment (e.g. GitHub Actions secrets) and never commit them to source control.

## Generating a token

1. Open **Settings → API Tokens**
2. Enter a name for the token and click **Generate**
3. Copy the token from the one-time reveal prompt — it is shown only once
4. Store it securely

Tokens can be revoked at any time. A revoked token can be reactivated or permanently deleted.

## Premium template workflows

Premium templates include pre-built GitHub Actions workflows that use your credentials to write data directly into your databases:

- **Traceability Matrix** — syncs test run results to the `tests` database and updates coverage status on linked requirements after every CI run
- **SBOM** — scans dependency manifests (npm, Python, Go, Maven, Rust) and upserts records to the `dependencies` and `vulnerabilities` databases on every push

## Record API

A REST API for reading and writing records programmatically is coming soon.
