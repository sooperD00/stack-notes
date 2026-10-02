# stack-notes

# stack-notes

> **This repo is public. Every note is public the moment it's pushed.**
> - Write each gotcha about the tool, never about the project where it bit. No
>   project or client names, file paths, hosts, URLs, or data from a private repo.
> - The Seen line names a public project, or says `private project`.
> - If a gotcha can't be told without private detail, it stays in that project's docs.

Gotchas for the tools I build with, one file per tool. Each one cost somebody an hour once.
Planning and coding sessions read only the files for the tools a leg touches, and copy what
applies into the leg's Watches.

Keep this clone beside your projects, never inside one. Give a session access with
`claude --add-dir ~/repos/stack-notes`.

## Files

| File | Covers |
|------|--------|
| [uv.md](uv.md) | Python environments, pinning, locks |
| [pnpm-node.md](pnpm-node.md) | pnpm, Node versions, vitest |
| [sqlmodel-alembic.md](sqlmodel-alembic.md) | SQLModel sessions, Alembic autogenerate, test databases |
| [react.md](react.md) | React and its types |
| [git.md](git.md) | Line endings, grep on macOS |
| [macos.md](macos.md) | The shell and tools Apple ships |
| [docker.md](docker.md) | Build context and ignore files |
| [railway.md](railway.md) | Deploys, health checks, CI gating |

## One gotcha

```
## Short title: what goes wrong
- Symptom: what you see when it bites
- Rule: what to do instead, as an instruction
- Why: the mechanism, in one line
- Seen: <public project, or "private project">, YYYY-MM · <versions> · verified | unverified
```

- **Seen** is what makes a note reviewable. A gotcha tied to a version expires with it.
- **unverified** means nobody has reproduced it yet. Check it before a Watch leans on it.
- One gotcha per heading. When one stops being true, delete it; git keeps the history.

## Adding one

1. During a leg, the agent lists a new gotcha in the leg file's Issues as
   `stack: <tool> — <gotcha>`.
2. At handoff, it goes in the project's housekeeping.md as `[h-<id>] stack: <tool> — <gotcha>`.
3. At a review, collect them with `grep -n '] stack:' ~/repos/*/docs/sprints/housekeeping.md`,
   write each one up here in the format above, and clear the line in the project.
