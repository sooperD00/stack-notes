# macOS

## Apple's make is 3.81, and `.ONESHELL:` does nothing
- Symptom: `cd backend` on one recipe line doesn't carry to the next, so the next command runs in the wrong folder
- Rule: put `cd backend && uv sync` on one line, or install a newer make with Homebrew (it lands as `gmake` unless you add the gnubin path).
- Why: Apple stopped at the last GPLv2 make, from 2006. make arrives with the Xcode command line tools.
- Seen: application-pipeline, 2026-09 · verified

## Homebrew Python is not your project's Python
- Rule: see uv.md, "Pin the interpreter before the first sync on a machine".
