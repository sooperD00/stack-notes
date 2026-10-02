# Docker

## A bare name in .dockerignore matches only at the root
- Symptom: a `.venv`, `.env` or `node_modules` deep in the tree ships in the build context
- Rule: write `**/name` to match at any depth. Check with a probe that makes a fake file for every ignore rule and asks Docker what it would send (application-pipeline's `scripts/check_docker_context.py --probe`).
- Why: .gitignore and .dockerignore look alike and match differently
- Seen: application-pipeline, 2026-09 · verified (398 leaks → 0)

## Dockerfile.dockerignore replaces .dockerignore
- Rule: if a `<Dockerfile name>.dockerignore` exists, the plain `.dockerignore` is ignored for that build. Keep one.
- Seen: application-pipeline, 2026-09 · verified
