# Railway

## Every push to main deploys
- Symptom: a "stash" commit broke production
- Rule: park unfinished ideas in a quarantine folder, not on main. Use a short-lived branch only for a change that has to sit in its real place (a workflow, a root config). A done-when that says "must not touch prod" really means "must not change what prod runs".
- Seen: application-pipeline, 2026-09 · verified

## Set the health check and the builder by hand
- Rule: set the healthcheck path (e.g. `/health`), so Railway waits for the app before sending traffic. Set the builder to Dockerfile, so the setting says what runs instead of what Railway inferred.
- Why: without a health check, a build that succeeds while the app can't start looks the same as a good one
- Seen: application-pipeline, 2026-09 · verified

## Wait for CI needs a workflow on push to main
- Rule: gate deploys with a workflow on `push: branches: [main]`, then prove the gate. Push a deliberately red commit, confirm the deploy shows SKIPPED, then revert.
- Seen: application-pipeline, 2026-09 · planned, not yet proven
