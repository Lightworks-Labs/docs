---
title: Pull Requests
---

Every change in Lightworks is committed to a branch and proposed as a pull request before it reaches your main branch. Pull requests are visible in the sidebar under the PR icon.

## How changes become pull requests

When you save changes to a document, Lightworks commits them to a branch named after you and opens a pull request automatically. The PR accumulates further edits until it is merged or closed.

## Reviewing a pull request

Open a pull request from the sidebar to see:

- **Diff view** — a side-by-side or unified view of every file changed, with additions in green and removals in red
- **Timeline** — a chronological log of comments, commits, and status events
- **File list** — all files touched by the PR

## Commenting

Type a comment in the text box at the bottom of the timeline and submit. Comments are posted directly to the GitHub pull request and visible to anyone with repository access.

## Merging

Click **Merge** to merge the pull request into the main branch. Two merge paths are available depending on your plan:

- **Standard merge** — merges immediately with no additional confirmation
- **eSignature merge** — prompts for MFA verification before merging, then writes an eSignature record into every markdown file touched by the PR (requires the eSignatures plan)

Once merged, the sidebar file tree refreshes automatically.

## Closing and reopening

Use the **···** menu on a pull request to close it without merging, or to reopen a previously closed PR.

## Conflict resolution

If a pull request has merge conflicts, a **Resolve conflicts** prompt appears. Lightworks opens a side-by-side conflict editor where you can choose between incoming and current changes for each conflicting file, then commit the resolution directly from the app.
