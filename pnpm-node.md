# pnpm, Node, vitest

## Add packages with `pnpm add`
- Rule: `pnpm add <pkg>` for runtime, `pnpm add -D <pkg>` for test and build. `pnpm install` with no arguments installs what package.json already lists.
- Rule: run tools with `pnpm exec <tool>`, not `npx`, so the project's own version runs.
- Seen: leg-template review, 2026-10 · verified

## The lockfile is pnpm-lock.yaml
- Rule: commit `pnpm-lock.yaml`. A `package-lock.json` in a pnpm project means somebody ran npm; delete it.
- Seen: leg-template review, 2026-10 · verified

## Declare the Node version, and match it everywhere
- Symptom: local builds and the image disagree (the Mac ran Node 26; the Dockerfile built on Node 20)
- Rule: pick the current LTS line and set it in one commit across the Dockerfile, CI, and `engines` in package.json (or `.nvmrc`).
- Why: Node 20 reached end of life on 2026-04-30
- Seen: application-pipeline, 2026-09 · verified

## vitest writes part of its output to stderr
- Symptom: a teed log file that's missing results
- Rule: `pnpm exec vitest run --reporter=verbose --no-color 2>&1 | tee <file>`. `verbose` gives the full `describe > it` tree, and `--no-color` keeps escape codes out of the file.
- Seen: Nicole's leg template · verified
