---
title: Repos
---

Lightworks reads and writes files directly in your GitHub repository. You need to connect at least one repository to get started.

## Prerequisites

Before connecting a repository, the Lightworks GitHub App must be installed on your GitHub organization or personal account and granted access to the repository you want to connect.

## Connecting your first repository

After completing onboarding, you'll be taken to the connect repository page automatically. You can also reach it from the sidebar by hovering over the **Repositories** section and clicking the menu.

### 1. Enter your GitHub organization or username

Type the name of the GitHub organization or personal account that owns the repository. Lightworks checks whether the GitHub App is installed on that account.

If the app is not yet installed, an **Install GitHub App** button appears. Clicking it takes you to GitHub to install the app and select which repositories to grant access to. After installation you'll be redirected back to complete the connection.

### 2. Select a repository

Choose a repository from the dropdown. Private repositories are marked with a lock icon. If you don't see a repository, click **Manage app repositories** to update which repos the GitHub App can access.

### 3. Choose a connection type

**Subfolder** is strongly recommended for most teams. When your QMS records live in a subfolder of the same repository as your source code, Premium templates can take full advantage of GitHub Actions automation — test results are automatically linked to the records that track them, and your SBOM is generated directly from your project's dependencies. The traceability matrix fills itself in because the code and the QMS are in the same repo.

- **Subfolder** (recommended) — Lightworks stores files in a specific folder within the repo (e.g. `docs/qms`). The rest of the repository remains untouched.
- **Full repository** — the entire repository is used as the workspace. Best suited for dedicated documentation repos with no source code.

### 4. Set the default branch

Lightworks defaults to the repository's default branch. Change this if your team works off a different branch.

### 5. Finish connecting

Lightworks checks that the required scaffold files exist in the repository. If any are missing, it opens a pull request to initialize them. Merge that PR on GitHub, and Lightworks will automatically complete the connection.

Once connected, you'll be redirected to your workspace.

## Connecting additional repositories

The steps are the same as above. Access the connect flow from the **Repositories** section menu in the sidebar.

The number of repositories you can connect depends on your plan:

| Plan | Repository limit |
|---|---|
| Free | 1 |
| Quality Manager | 5 |
| Enterprise | Unlimited |

If you've reached your plan's limit, upgrade from **Settings → Billing** to connect more repositories.

## Disconnecting a repository

1. Hover over the repository name in the sidebar
2. Click the `...` menu that appears
3. Click **Disconnect**
4. Confirm in the modal

Disconnecting removes the repository from Lightworks but does not delete any files from GitHub. You can reconnect it at any time — though any search indexes and repository-specific settings will need to be configured again.

If you disconnect your only connected repository, you'll be taken back to the connect repository page.
