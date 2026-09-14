# Deployment release history

Keep one entry per release. Use UTC and never include credentials, tokens, recovery codes, raw database URLs containing secrets, or user data.

```text
## YYYY-MM-DD HH:MMZ — <short release name>

- Source commit: <full SHA>
- GitHub workflow: <run URL> — success/failure
- GitVerse workflow: <run URL> — success/failure
- Backend function version: <version ID> — ACTIVE/failed
- Static URLs: <GitHub Pages URL>, <GitVerse Pages URL>
- API health: pass/fail
- CORS preflight: pass/fail for each configured origin
- Smoke flow: describe only the non-sensitive operation and result
- Rollback target: <last known-good commit/version>
- Notes: <non-sensitive limitation or follow-up>
```

Minimum verification after every release:

1. Both Pages URLs return HTTP 200 and load their base-path assets.
2. API `/health` returns success.
3. OPTIONS preflight returns the exact requesting origin for each Pages host.
4. A fresh browser can bootstrap/read state without a console error.
5. The release commit and backend version are recorded together.
