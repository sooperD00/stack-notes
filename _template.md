# <Tool name>

<!-- HOW TO USE THIS FILE
Copy it to <tool>.md (lowercase, hyphens: uv.md, pnpm-node.md, sqlmodel-alembic.md) and add a
row for it to the README's Files table. Delete every comment as you fill it in.

THIS REPO IS PUBLIC. Write each gotcha about the tool, never about the project where it bit.
No project or client names, paths, hosts, URLs or data from a private repo. If a gotcha can't
be told without private detail, it stays in that project's docs.

One gotcha per ## heading. Order them by how often they bite, most often first.
When a gotcha stops being true, delete it. git keeps the history.
-->

## <What goes wrong, as a short title>
- Symptom: <what you see when it bites: the error text, the wrong number, the silent miss>
- Rule: <what to do instead, as an instruction someone can follow at the keyboard>
- Why: <the mechanism, in one line>
- Seen: <public project, or "private project">, YYYY-MM · <tool> <version> · verified | unverified
- Recheck: <the release or event that could make this stop being true, or "stable">
- Source: <a docs page, changelog entry, or issue that confirms it, or "none yet">

<!-- WHY EACH FIELD IS HERE

Symptom  is how a reader recognizes the gotcha when it happens. Write what they will actually
         see, because that's what they'll search for.

Rule     is the part an agent copies into a Watch. If it isn't an instruction, the Watch
         can't be checked.

Why      keeps the rule from being applied blindly, and tells you when it no longer applies.

Seen     is what makes a note reviewable. It records where it bit, when, on which version,
         and whether anyone has reproduced it. The version matters most: a gotcha tied to a
         version expires with it. A React 19 type deprecation is wrong advice on React 18, and
         may be gone in React 20. Without the version, nobody can tell a live gotcha from a
         stale one.
         - verified: reproduced, or it cost real time and the fix worked.
         - unverified: reported, but not yet reproduced. It's a lead. Check it before a Watch
           relies on it.

Recheck  says when to look at this note again: "uv 1.0", "React 20", "when Node 24 leaves
         LTS". "stable" means the cause is design, not a version (for example, make 3.81 on
         macOS, or how .dockerignore matches names). During a review, grep for Recheck lines
         whose release has shipped.

Source   lets the next reader confirm the note without reproducing it. A gotcha found the hard
         way often has none at first. Fill it in when you find one.
-->
