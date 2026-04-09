---
title: Developers
---

The Lightworks API lets you read and write records programmatically — useful for integrating CI/CD pipelines, automating traceability updates, and syncing dependency or test data into your workspace.

## Authentication

All API requests require two values from **Settings → API Tokens**:

- `LIGHTWORKS_ORG_ID` — your organization's unique identifier, shown at the bottom of the API Tokens tab
- `LIGHTWORKS_API_TOKEN` — a token you generate from the same tab

Pass the token as a Bearer header on every request:

```
Authorization: Bearer <LIGHTWORKS_API_TOKEN>
```

Tokens are org-scoped — a token has access to all databases in your workspace. Store both values as secrets in your CI environment (e.g. GitHub Actions secrets) and never commit them to source control.

## Generating a token

1. Open **Settings → API Tokens**
2. Enter a name for the token and click **Generate**
3. Copy the token from the one-time reveal prompt — it is shown only once
4. Store it securely (e.g. as a GitHub Actions secret named `LIGHTWORKS_API_TOKEN`)

Tokens can be revoked at any time. A revoked token can be reactivated or permanently deleted.

## Base URL

```
https://app.lightworks.md/api/v1
```

## Endpoints

### List records

```
GET /orgs/{ORG_ID}/databases/{database_id}/records
```

Returns all records in a database.

```json
{
  "records": [
    {
      "id": "rec_abc123",
      "fields": {
        "name": "Authentication Module",
        "status": "approved",
        "linked_requirements": ["rec_def456"]
      }
    }
  ]
}
```

### Create a record

```
POST /orgs/{ORG_ID}/databases/{database_id}/records
Content-Type: application/json
```

```json
{
  "fields": {
    "name": "lodash",
    "version": "4.17.21",
    "ecosystem": "npm",
    "purl": "pkg:npm/lodash@4.17.21"
  }
}
```

### Update a record

```
PATCH /orgs/{ORG_ID}/databases/{database_id}/records/{record_id}
Content-Type: application/json
```

```json
{
  "fields": {
    "status": "passed",
    "last_run_at": "2026-04-08T15:30:00Z"
  }
}
```

Only the fields included in the request body are updated. Omitted fields are left unchanged.

## Field values

Field values follow the property types defined in the database schema:

| Type | Format |
|---|---|
| `text` | String |
| `number` | Number |
| `select` / `status` | String matching an option value |
| `multi-select` | Array of strings |
| `date` | ISO 8601 string (`"2026-04-08T15:30:00Z"`) |
| `checkbox` | Boolean |
| `url` | String |
| `person` / `people` | GitHub login string or array |
| `relation` | Record ID string or array of record ID strings |

## GitHub Actions example

```yaml
- name: Update test result
  run: |
    curl -s -X PATCH \
      "https://app.lightworks.md/api/v1/orgs/${{ secrets.LIGHTWORKS_ORG_ID }}/databases/tests/records/$RECORD_ID" \
      -H "Authorization: Bearer ${{ secrets.LIGHTWORKS_API_TOKEN }}" \
      -H "Content-Type: application/json" \
      -d '{"fields": {"status": "passed", "last_run_at": "'"$(date -u +%Y-%m-%dT%H:%M:%SZ)"'"}}'
```

## Premium template workflows

Premium templates include pre-built GitHub Actions workflows that use the API automatically:

- **Traceability Matrix** — syncs test run results to the `tests` database and updates coverage status on linked requirements after every CI run
- **SBOM** — scans dependency manifests (npm, Python, Go, Maven, Rust) and upserts records to the `dependencies` and `vulnerabilities` databases on every push
