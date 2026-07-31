---
name: Separate stems with Music AI
description: Upload an audio file and run a stem-separation (or any) workflow to completion, then retrieve the results.
api: openapi/moises-music-ai-openapi.yml
operations: [getUploadUrl, createJob, getJobStatus, getJob, deleteJob]
---

# Separate stems with Music AI

Use the Music AI API to run an audio file through one of your account's workflows
(for example a stem-separation workflow) and collect the output.

## Auth
Every request carries your API key verbatim in the `Authorization` header (no
`Bearer` prefix). Generate a key at https://music.ai/dash. Base URL:
`https://api.music.ai/v1`.

## Steps
1. **Stage the input.** Call `getUploadUrl` (`GET /upload`) to get a temporary
   `uploadUrl` and `downloadUrl`. `PUT` your local audio file to the `uploadUrl`
   (Google Cloud Storage). Skip this step if you already have a public audio URL.
2. **Create the job.** Call `createJob` (`POST /job`) with `name`, `workflow`
   (the slug of the workflow to run), and `params.inputUrl` set to the
   `downloadUrl` from step 1 (or your public URL). The response returns the job `id`.
3. **Poll for completion.** Call `getJobStatus` (`GET /job/{id}/status`) until
   `status` is `SUCCEEDED` or `FAILED`. Statuses progress `QUEUED → STARTED →
   SUCCEEDED|FAILED`. There are no webhooks — poll.
4. **Read results.** On `SUCCEEDED`, call `getJob` (`GET /job/{id}`) and read the
   `result` object for the output stem/asset URLs.
5. **Clean up (optional).** Call `deleteJob` (`DELETE /job/{id}`) when done.

## Error handling
Errors use a `{code, title, message}` envelope. Handle `BAD_INPUT` (fix the input
URL / workflow params), `TIMEOUT` (retry), and `INTERNAL_ERROR` (retry, then
contact support@music.ai). HTTP 401 means the API key is missing/invalid.
See `errors/moises-error-codes.yml`.
