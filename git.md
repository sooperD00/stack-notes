# git

## One line-ending convention, set before the first cross-machine commit
- Symptom: a file re-diffs whole the first time it's touched on another machine; a shell script fails with `python3\r: No such file or directory`
- Rule: `.gitattributes` with `* text=auto eol=lf`, then `git add --renormalize .` in its own commit. Mark binaries (`*.png binary`). A file kept byte-for-byte as evidence gets `-text`.
- Why: a CRLF on a shebang line names an interpreter with `\r` in it
- Seen: application-pipeline, 2026-09 · verified (29 CRLF files, 27 normalized)

## macOS `git grep -E` has no `\b`
- Symptom: a search comes back empty and looks like good news
- Rule: use `git grep -P` for word boundaries, or test the pattern on a line you know matches first.
- Seen: application-pipeline, 2026-09 · Apple git · verified

## `git grep` skips untracked files unless told
- Rule: pass `--untracked` when checking work nobody has added yet, like a cleanup-marker sweep at handoff.
- Seen: sprinter-kit, 2026-09 · verified
