---
name: static-pages-yandex-deploy
description: Deploy static web apps to GitHub Pages and GitVerse with a Yandex Cloud API backend, including repeatable checks, release history, and rollback guidance.
---

# Static Pages + Yandex Deploy

Use this skill when a project has a static browser frontend and a small serverless backend on Yandex Cloud, and it needs publication to GitHub Pages and GitVerse Pages. It covers planning and execution; it never invents credentials or silently changes DNS, databases, or production data.

## Required inputs and boundaries

Before mutating anything, identify:

- the source GitHub repository and default branch;
- the GitVerse repository and the Pages project URL;
- the Yandex Cloud folder, function ID/name, API Gateway URL, database, and the existing private package bucket (if used);
- build command, output directory, base path, API URL, and allowed browser origins;
- whether the user explicitly authorized pushes, cloud deployment, DNS, and certificate changes.

Never put tokens, HMAC keys, database credentials, or recovery secrets in source, workflow files, commits, logs, URLs, or the final response. Use the user's configured credential store or environment variables. Treat GitVerse and Yandex deployment as separate external mutations and verify each one independently.

Read the project's `AGENTS.md`, product/architecture docs, package scripts, and deployment configuration first. Preserve the existing data store; a frontend migration is not permission to reset or recreate a database.

## Recommended topology

`Browser → GitHub Pages or GitVerse Pages (static assets) → HTTPS API Gateway → Yandex Cloud Function → YDB/PostgreSQL`

The browser must call only the API Gateway. It must not connect directly to a database. Configure exact HTTPS CORS origins for every Pages host, and use the repository base path in the static build. Keep secrets server-side; public frontend configuration may contain only the API URL and non-sensitive public identifiers.

## Deployment sequence

1. **Preflight.** Check `git status`, current commit, branch, remotes, and whether the working tree contains unrelated changes. Run the repository's documented lint, typecheck, unit tests, build, and relevant E2E checks. Build locally and confirm the output directory has a root `index.html` plus the expected base-path asset URLs.

2. **Backend first.** Build a reproducible function package from the lockfile, excluding unrelated files and development artifacts. Upload it only to the already-approved private package bucket. Create a new immutable function version with the existing runtime, service account, memory/timeout, environment, and concurrency settings. Do not print the resulting configuration if it contains secrets. Check the version is `ACTIVE`, then call the public `/health` endpoint and one safe authenticated/read-only endpoint when possible.

3. **GitHub Pages.** Push the reviewed commit to the default branch only after explicit authorization. Wait for the Pages workflow to finish. Verify the published URL returns HTTP 200, the expected title, and assets under the configured base path. If the workflow fails, inspect its job log before changing code.

4. **GitVerse Pages.** Push the same commit to the GitVerse repository only after explicit authorization. Confirm Pages is enabled with source `Workflow`, not an incompatible branch source. The workflow must use GitVerse's published action repositories (`actions/upload-pages-artifact` and `actions/deploy-pages` where supported), upload a directory containing root `index.html`, and use `runs-on: ubuntu-latest`. Wait for both build and deploy jobs. Verify the GitVerse Pages URL and do not treat a build-only success as publication.

5. **Post-deploy smoke checks.** Check both static URLs, the API health endpoint, CORS preflight from each origin, and a fresh browser session. Exercise one non-destructive flow (bootstrap/session read) and confirm no console/network errors. For state-changing smoke tests, use a disposable test account only with explicit authorization; never overwrite a real user's state.

6. **Release record.** Record UTC time, source commit, backend function version, Pages workflow run IDs/URLs, published URLs, checks performed, and any known limitation. Keep this record in the project's deployment history file or release notes, not in secrets.

## Rollback

Rollback is provider-specific and must be deliberate. For the backend, switch the function alias/active version back to the last known-good immutable version; do not delete the database or package bucket. For Pages, revert the source commit and wait for both workflows, or select the last known-good artifact if the provider supports it. Re-run health, CORS, and smoke checks after rollback and record the incident.

## Common failure handling

- GitVerse `Set up job` or action clone failures: inspect job logs. A Pages action under `gitverse/<name>` may be unavailable; use the published `actions/<name>` repository and version.
- A published 404: check Pages source mode, root `index.html`, repository base path, and whether the deployment job actually ran.
- A 502/API failure: check function version status, Gateway integration, environment variables, and `/health`; do not regenerate or reset the database as a first response.
- CORS failures: compare the exact `Origin` (scheme, host, and port) with the allowlist. Never solve them with `*` in production.

## References

Read [references/release-history.md](references/release-history.md) when recording or reviewing a deployment. It provides a compact, non-secret release log format and verification checklist.
