---
name: Discover workflows and manage jobs
description: List the workflows available in your account and browse, filter, and clean up processing jobs.
api: openapi/moises-music-ai-openapi.yml
operations: [listWorkflows, listJobs, getJob, deleteJob, getApplication]
---

# Discover workflows and manage jobs

Use the Music AI API to inspect the workflows configured in your account and to
manage the jobs you have submitted.

## Auth
Send your API key in the `Authorization` header. Base URL `https://api.music.ai/v1`.

## Steps
1. **Confirm the account.** Call `getApplication` (`GET /application`) to verify
   which application/account your key belongs to.
2. **List workflows.** Call `listWorkflows` (`GET /workflow`) with `page`/`size`
   to page through the workflows configured in the dashboard. Use a workflow's
   slug as the `workflow` value when creating jobs.
3. **List jobs.** Call `listJobs` (`GET /job`) with `page` (default 0) and `size`
   (default 100). Filter with `status` (`QUEUED|STARTED|SUCCEEDED|FAILED`),
   `workflow`, and/or `batchName`.
4. **Inspect a job.** Call `getJob` (`GET /job/{id}`) for full detail including
   `result`, timestamps, and `metadata`.
5. **Delete a job.** Call `deleteJob` (`DELETE /job/{id}`) to remove a job.

## Notes
Pagination is page-number based (`page`/`size`). Errors use the
`{code, title, message}` envelope (see `errors/moises-error-codes.yml`).
