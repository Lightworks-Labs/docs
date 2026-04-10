---
title: Developers
---

Lightworks provides a REST API for reading and writing records programmatically. It is designed for use in GitHub Actions workflows — particularly for premium templates like Traceability Matrix and SBOM that automate QMS record creation as part of your CI/CD pipeline.

## Credentials

You need two values from **Settings → API Tokens**:

- `LIGHTWORKS_ORG_ID` — your organization's unique identifier, shown at the bottom of the API Tokens tab
- `LIGHTWORKS_API_TOKEN` — a token you generate from the same tab

Store both as secrets in your GitHub repository and never commit them to source control.

## Generating a token

1. Open **Settings → API Tokens**
2. Enter a name (e.g. `CI Pipeline`) and select a permission level:
   - **Read** — `read:databases` only
   - **Write** — `read:databases` + `write:databases`
3. Click **Generate** and copy the token — it is shown only once

Tokens can be revoked or reactivated at any time from the same tab.

## Base URL

```
https://app.lightworks.io/api/v1
```

All requests require:

```
Authorization: Bearer <LIGHTWORKS_API_TOKEN>
```

## The `repo` parameter

All endpoints require a `repo` parameter identifying your connected repository. If your QMS content lives in a subfolder of the repository, append the path:

| Scenario | Value |
|---|---|
| QMS at repo root | `my-org/my-qms` |
| QMS in a `qms/` subfolder | `my-org/my-qms/qms` |
| QMS in a nested path | `my-org/my-qms/org/qms` |

---

## Object types

Every response object includes an `object` field identifying its type. This lets you safely discriminate between objects without inspecting their shape.

| Value | Description |
|---|---|
| `"database"` | A database (folder with a `_schema.json`) |
| `"record"` | A record (page bundle with an `index.md`) |
| `"pr"` | A submission PR |
| `"list"` | A paginated list of results |

### The `parent` field

Databases and records include a `parent` object describing what contains them. This mirrors the nesting model in Lightworks — databases and records can be nested like pages in Notion.

**Database parents:**

```json
// Top-level database
{ "type": "workspace" }

// Database nested inside a record
{ "type": "record", "path": "requirements/req-a3f7k2" }
```

**Record parents** (always a database):

```json
{ "type": "database", "database": "requirements" }
```

---

## Pagination

List endpoints (`GET /api/v1/databases`, `GET /api/v1/records`) return paginated results.

**Query parameters:**

| Parameter | Default | Max | Description |
|---|---|---|---|
| `page_size` | `50` | `100` | Number of results per page |
| `start_cursor` | — | — | Cursor from a previous response's `next_cursor` |

**Response envelope:**

```json
{
  "object": "list",
  "results": [ ... ],
  "next_cursor": "eyJvIjo1MH0",
  "has_more": true
}
```

When `has_more` is `false`, `next_cursor` will be `null` — you have reached the last page.

**Iterating all pages:**

```bash
cursor=""
while true; do
  PARAMS="db=requirements&orgId=$LW_ORG_ID&repo=$LW_REPO&page_size=100"
  [ -n "$cursor" ] && PARAMS="$PARAMS&start_cursor=$cursor"
  RESPONSE=$(curl -s "$LW_API_URL/api/v1/records?$PARAMS" \
    -H "Authorization: Bearer $LW_API_TOKEN")
  echo "$RESPONSE" | jq '.results[]'
  has_more=$(echo "$RESPONSE" | jq -r '.has_more')
  [ "$has_more" != "true" ] && break
  cursor=$(echo "$RESPONSE" | jq -r '.next_cursor')
done
```

Cursors are opaque — treat them as black boxes and do not attempt to parse or construct them.

---

## Databases

### List databases

Returns all databases in the connected repository, including nested ones. The `id` field is a path relative to your QMS root and is used as the `db` parameter on record endpoints.

```
GET /api/v1/databases?orgId=<orgId>&repo=<repo>
```

**Response**

```json
{
  "object": "list",
  "results": [
    {
      "object": "database",
      "id": "requirements",
      "name": "requirements",
      "parent": { "type": "workspace" }
    },
    {
      "object": "database",
      "id": "risks",
      "name": "risks",
      "parent": { "type": "workspace" }
    },
    {
      "object": "database",
      "id": "requirements/req-a3f7k2/tests",
      "name": "tests",
      "parent": { "type": "record", "path": "requirements/req-a3f7k2" }
    }
  ],
  "next_cursor": null,
  "has_more": false
}
```

---

## Records

### List records in a database

Returns direct child records of the specified database. Records in nested sub-databases are not included — query them separately using their own `db` path.

```
GET /api/v1/records?db=<db>&orgId=<orgId>&repo=<repo>
```

The `db` parameter comes from the `id` field in the databases list. For nested databases, it may contain slashes (e.g. `requirements/req-a3f7k2/tests`).

**Response**

```json
{
  "object": "list",
  "results": [
    {
      "object": "record",
      "id": "requ-a3f7k2",
      "path": "qms/requirements/requ-a3f7k2/index.md",
      "parent": { "type": "database", "database": "requirements" },
      "properties": {
        "id": "requ-a3f7k2",
        "title": "The system shall encrypt all data at rest",
        "status": "In Review",
        "priority": "High"
      }
    }
  ],
  "next_cursor": null,
  "has_more": false
}
```

### Get a record

```
GET /api/v1/records/<id>?db=<db>&orgId=<orgId>&repo=<repo>
```

Returns the full record including its markdown `body`. The `path` field is the file's location in the repository and can be used to construct a link back to the record in the Lightworks UI.

**Response**

```json
{
  "object": "record",
  "id": "requ-a3f7k2",
  "path": "qms/requirements/requ-a3f7k2/index.md",
  "parent": { "type": "database", "database": "requirements" },
  "properties": {
    "id": "requ-a3f7k2",
    "title": "The system shall encrypt all data at rest",
    "status": "Approved",
    "priority": "High"
  },
  "body": "## Rationale\n\nEncryption at rest protects sensitive data from unauthorized access..."
}
```

### Create a record

Mints a new record ID from the database prefix, writes a page bundle to the PR branch, and returns the new record.

```
POST /api/v1/records
```

**Body**

```json
{
  "db": "requirements",
  "orgId": "<orgId>",
  "repo": "<repo>",
  "prId": "<prId>",
  "properties": {
    "title": "The system shall encrypt all data at rest",
    "status": "Draft",
    "priority": "High"
  }
}
```

**Response** `201`

```json
{
  "object": "record",
  "id": "requ-a3f7k2",
  "path": "qms/requirements/requ-a3f7k2/index.md"
}
```

### Update a record

Merges the provided `properties` into the existing record frontmatter. Properties not included in the request are left unchanged.

```
PATCH /api/v1/records/<id>
```

**Body**

```json
{
  "db": "requirements",
  "orgId": "<orgId>",
  "repo": "<repo>",
  "prId": "<prId>",
  "properties": {
    "status": "Approved"
  }
}
```

**Response**

```json
{
  "object": "record",
  "id": "requ-a3f7k2",
  "properties": {
    "id": "requ-a3f7k2",
    "title": "The system shall encrypt all data at rest",
    "status": "Approved",
    "priority": "High"
  }
}
```

### Delete a record

Removes the record from the PR branch. For regulated QMS records, prefer updating `status` to `Archived` instead of deleting.

```
DELETE /api/v1/records/<id>
```

**Body**

```json
{
  "db": "requirements",
  "orgId": "<orgId>",
  "repo": "<repo>",
  "prId": "<prId>"
}
```

**Response**

```json
{ "id": "requ-a3f7k2", "deleted": true }
```

---

## Pull Requests

A PR groups one or more record writes into a single draft pull request for human review. All write operations require a `prId`.

The typical flow:

1. **Create** a PR → get a `prId` and a branch (`lw/api/<id>`)
2. **Write** records — each write commits to that branch
3. **Finalize** when the originator PR merges → opens a draft PR in your repo for review and signature
4. **Cancel** if the originator PR closes without merging → closes the branch

### Create a PR

Pass `originatorContext` to make it idempotent — calling this endpoint again with the same PR number returns the existing submission rather than creating a new one. This is safe to call on every push.

```
POST /api/v1/prs
```

**Body**

```json
{
  "orgId": "<orgId>",
  "repo": "<repo>",
  "originatorContext": {
    "pr_repo": "my-org/my-product",
    "pr_number": 42
  }
}
```

**Response** `201`

```json
{
  "object": "pr",
  "prId": "pr-42",
  "branch": "lw/api/pr-42",
  "created": true
}
```

If a PR already exists for this originator, `created` will be `false` and the existing `prId` is returned.

### Finalize a PR

Called when the originator PR is merged. Opens a draft pull request in the QMS repo for review and signature. Before merging, Lightworks automatically stamps a stable `id` into any records that were created through the UI rather than the API.

```
POST /api/v1/prs/<prId>/finalize
```

**Body**

```json
{
  "orgId": "<orgId>",
  "repo": "<repo>"
}
```

**Response**

```json
{
  "object": "pr",
  "prId": "pr-42",
  "prNumber": 7,
  "prUrl": "https://github.com/my-org/my-qms/pull/7"
}
```

### Cancel a PR

Called when the originator PR is closed without merging. Closes the draft PR (if any) and deletes the branch.

```
POST /api/v1/prs/<prId>/cancel
```

**Body**

```json
{
  "orgId": "<orgId>",
  "repo": "<repo>"
}
```

**Response**

```json
{ "object": "pr", "prId": "pr-42", "cancelled": true }
```

---

## Record IDs

Every record has a stable `id` written into its frontmatter. IDs follow a prefixed format derived from the database name:

**Format:** `{prefix}-{6 random alphanumeric chars}`

| Database | Prefix | Example ID |
|---|---|---|
| `requirements` | `requ` | `requ-a3f7k2` |
| `risks` | `risk` | `risk-9m2pkx` |
| `tasks` | `task` | `task-7b1qzr` |
| `sbom` | `sbom` | `sbom-k4np8w` |
| `traceability-matrix` | `trac` | `trac-2xv5md` |
| `change-control` | `chan` | `chan-r8qt0j` |

**API-created records** receive their ID at creation time. The record's folder name matches its ID, so lookups are fast.

**Records created in the Lightworks UI** receive a stable minted ID automatically when their PR is merged — written as a commit to the PR branch immediately before the merge is finalized.

IDs are not reused after deletion.

---

## GitHub Actions workflow

Drop this file into `.github/workflows/lightworks-sync.yml` in any repository. Set `LIGHTWORKS_API_TOKEN` and `LIGHTWORKS_ORG_ID` as repository secrets.

Replace `<your-org>/<your-qms-repo>` with your connected QMS repository (append a path suffix if your QMS content lives in a subfolder, e.g. `<your-org>/<your-qms-repo>/qms`).

```yaml
name: Lightworks QMS Sync

on:
  pull_request:
    types: [opened, synchronize, closed]

env:
  LW_API_URL: https://app.lightworks.io
  LW_ORG_ID: ${{ secrets.LIGHTWORKS_ORG_ID }}
  LW_REPO: <your-org>/<your-qms-repo>

jobs:
  stage-records:
    if: github.event.action != 'closed'
    runs-on: ubuntu-latest
    outputs:
      submission_id: ${{ steps.submission.outputs.submission_id }}
    steps:
      - uses: actions/checkout@v4

      - name: Create or resume submission
        id: submission
        run: |
          RESPONSE=$(curl -sf -X POST "$LW_API_URL/api/v1/prs" \
            -H "Authorization: Bearer ${{ secrets.LIGHTWORKS_API_TOKEN }}" \
            -H "Content-Type: application/json" \
            -d '{
              "orgId": "${{ env.LW_ORG_ID }}",
              "repo": "${{ env.LW_REPO }}",
              "originatorContext": {
                "pr_repo": "${{ github.repository }}",
                "pr_number": ${{ github.event.pull_request.number }}
              }
            }')
          echo "submission_id=$(echo $RESPONSE | jq -r '.prId')" >> $GITHUB_OUTPUT

      # Add your record writes here. Example:
      - name: Sync SBOM record
        run: |
          curl -sf -X POST "$LW_API_URL/api/v1/records" \
            -H "Authorization: Bearer ${{ secrets.LIGHTWORKS_API_TOKEN }}" \
            -H "Content-Type: application/json" \
            -d "{
              \"db\": \"sbom\",
              \"orgId\": \"${{ env.LW_ORG_ID }}\",
              \"repo\": \"${{ env.LW_REPO }}\",
              \"prId\": \"${{ steps.submission.outputs.submission_id }}\",
              \"properties\": {
                \"title\": \"SBOM ${{ github.sha }}\",
                \"status\": \"Draft\",
                \"commit\": \"${{ github.sha }}\",
                \"pr\": ${{ github.event.pull_request.number }}
              }
            }"

  finalize:
    if: github.event.action == 'closed' && github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    steps:
      - name: Finalize submission
        run: |
          curl -sf -X POST "$LW_API_URL/api/v1/prs/pr-${{ github.event.pull_request.number }}/finalize" \
            -H "Authorization: Bearer ${{ secrets.LIGHTWORKS_API_TOKEN }}" \
            -H "Content-Type: application/json" \
            -d '{
              "orgId": "${{ env.LW_ORG_ID }}",
              "repo": "${{ env.LW_REPO }}"
            }'

  cancel:
    if: github.event.action == 'closed' && github.event.pull_request.merged != true
    runs-on: ubuntu-latest
    steps:
      - name: Cancel submission
        run: |
          curl -sf -X POST "$LW_API_URL/api/v1/prs/pr-${{ github.event.pull_request.number }}/cancel" \
            -H "Authorization: Bearer ${{ secrets.LIGHTWORKS_API_TOKEN }}" \
            -H "Content-Type: application/json" \
            -d '{
              "orgId": "${{ env.LW_ORG_ID }}",
              "repo": "${{ env.LW_REPO }}"
            }'
```

> **No webhook registration required.** The workflow handles the full PR lifecycle using GitHub Actions native triggers.

---

## Premium template workflows

Premium templates ship with pre-built versions of this workflow, pre-configured for their specific databases:

- **Traceability Matrix** — syncs test run results to the `tests` database and links coverage status to requirements after every CI run
- **SBOM** — scans dependency manifests (npm, Python, Go, Maven, Rust) and upserts records to the `sbom` and `vulnerabilities` databases on every push

---

## Error reference

| Status | Meaning |
|---|---|
| `400` | Bad request — missing or invalid parameter |
| `401` | Invalid or expired API token |
| `403` | Token lacks the required scope, or repo is not connected to this org |
| `404` | Record or PR not found |
| `409` | PR is no longer in staging state |
| `500` | GitHub API or internal error — message included in response |
