# Backlog — Tursora fork

What is queued for this fork, roughly in order. A thread that grows past a few
lines gets its own file in this folder. Done items move to the Ledger, not to
the bottom of this list.

## Decide first

**Sparkle feed and bundle id.** `app/tools/make-app.sh` bakes in
`com.tursora.Tursora` and `SUFeedURL` → zerolfx's releases. Options: point the
feed at nothing and rely on `git merge upstream/main` for updates; or change
the bundle id and feed to this fork's own, which is the start of a rename.
Until decided: automatic update installation off in Settings → Updates.

**Commit attribution.** Upstream's `AGENTS.md` forbids AI attribution trailers
in upstream's history. Whether the fork keeps that rule is Sam's call; the
Obsidian convention is that a commit message explains the decision, not the
diff, whoever wrote it.

## Build

**Reveal in Finder.** Nothing in the source calls
`NSWorkspace.shared.activateFileViewerSelecting` — there is no way to hand the
current selection to Finder for the things Tursora does not do yet (tags,
Quick Actions, Put Back). A menu item, a shortcut, and a context-menu entry.
Small, and a good first change to learn the shape of `BrowserEverydayCommands`
and the smoke-test discipline.

**Sam's enhancement ideas.** Stated 2026-09-27 to exist; not yet written down.
Add them here before they evaporate.

## Watch

**Upstream velocity.** 64 commits in two weeks, restructures included. Merge
`upstream/main` often. If a merge touches `Model/TerminalShellIntegration.swift`
or `Model/TerminalDirectorySync.swift`, read the diff — that is the feature.
