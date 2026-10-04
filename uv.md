# uv

## uv picks the environment from the directory and never says which
- Symptom: tests, counts or imports that don't match what you expect; a package "missing" that you just installed
- Rule: run uv from the project folder that holds the venv (`cd backend && uv sync`), or pass `uv --directory backend run ...`. Check before trusting any number: `uv run python -c "import sys; print(sys.prefix)"` must end in the venv you mean.
- Why: a stale `.venv` at the repo root answered instead of `backend/.venv`
- Seen: application-pipeline, 2026-09 · uv 0.x · verified (cost an hour)

## One venv, in one place
- Rule: exactly one `.venv`, inside the project folder. A second `.venv` anywhere else is the bug. Delete it; don't work around it.
- Seen: application-pipeline, 2026-09 · verified

## Pin the interpreter before the first sync on a machine
- Symptom: the venv builds on a different Python than the image or CI (Homebrew Python was two minor versions ahead)
- Rule: `uv python pin <version>` writes `.python-version`. Commit it before anyone runs `uv sync` on a new machine. If a system Python (Homebrew's, say) has the same minor version as the pin, pinning isn't enough: set `[tool.uv] python-preference = "only-managed"` in `pyproject.toml` before the first `uv add` or `uv sync`, and uv downloads a managed build instead.
- Why: under the default preference, `managed`, uv uses a matching system interpreter when no managed one is installed yet, rather than downloading one.
- Seen: application-pipeline, 2026-09 · verified. The same-minor-version half: private project, 2026-10 · uv 0.11.8 · unverified (taken from uv's documented preference order; `only-managed` was set before the first `uv add`, so the fallback wasn't reproduced)
- Recheck: uv 1.0
- Source: https://docs.astral.sh/uv/concepts/python-versions/

## Declare and resolve in separate commits
- Rule: commit `pyproject.toml` (what the project asks for) apart from `uv.lock` (what resolved). Reading them together hides which one moved a version.
- Seen: application-pipeline, 2026-09 · verified

## Check the lock before it ships
- Rule: `uv lock --check` and `uv sync --check` both exit clean before a deploy or in CI.
- Seen: application-pipeline, 2026-09 · verified

## `--break-system-packages` is not a uv-project flag
- Rule: `uv add` / `uv add --dev` inside a project never needs it. It belongs to pip-style installs into a system Python.
- Seen: leg-template review, 2026-10 · verified
