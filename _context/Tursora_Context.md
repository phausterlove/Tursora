---
type: long-term-memory
tier: project
project: Tursora
created: 2026-09-27
updated: 2026-09-27
---

# Tursora — Project Context

**Tier 2.** Facts about this fork and what Sam wants from it. Global facts about
Sam — his people, his principles, his voice — live in
`~/Obsidian/_context/Sam_Context.md` and are read first, every session.

**The test for what goes here:** *would this still be true if Tursora were
deleted?* That Sam thinks in systems and lands on concrete things — global.
That the fork keeps upstream's name until a rename is decided — here.

Corrections follow the archive convention: original claim stated and marked
withdrawn, never silently revised.

---

## Why the fork exists

Sam wanted a Finder window with a side-by-side terminal whose working directory
simply follows the folder shown. Path Finder does it with an explicit click; the
paid alternatives were no better. On 2026-09-27 a research pass found
zerolfx/Tursora, he installed the 0.5.0 cask, and his words were: *"Right away I
was able to intuitively do what all the other paid-for apps wouldn't."* He
already had enhancement ideas, so it became a fork rather than a dependency.

The terminal-follow design is the feature to protect: shell to browser rides on
OSC 7 from a `precmd` hook; browser to shell goes through a private request
file plus a FIFO that wakes zsh's line editor, so a `cd` only lands at a safe
prompt and never touches half-typed input. Shell integration is injected via a
temporary `ZDOTDIR` (or `--rcfile` / `--init-command`), so the user's own rc
files are untouched.

## What upstream is

- One author (zerol), agent-driven development: `AGENTS.md` is the contributor
  guide, and a commit explicitly forbids AI attribution trailers in upstream's
  history. 64 commits from 2026-09-12 to 09-26; the first landed 15.8k lines.
- Native AppKit in code, no SwiftUI, no nibs. `Model/` headless, `UI/` AppKit.
  Dependencies pinned exactly: SwiftTerm 1.15.0, Sparkle 2.9.6.
- Tests are an in-app smoke mode (`TURSORA_SMOKE_TEST=1`), not XCTest.
- Ghostty embedding was investigated upstream and rejected — the VT library
  ships without a renderer. SwiftTerm is the terminal for the foreseeable future.
- Declared out of scope upstream: Finder tags, Import from iPhone.

## Decided

- **Name kept.** Folder `Tursora`, repo `phausterlove/Tursora` (a GitHub fork,
  public), routing word "tursora". Renaming the app — bundle id, icon, Sparkle
  feed — is a separate migration, not a setting, and is not yet decided.
- **Two remotes**, `origin` (the fork) and `upstream` (zerolfx). Upstream is
  pulled from, never pushed to.
- **Obsidian project files live at the repo root** beside upstream's layout.
  They are the fork's own and never conflict on merge.

## Standing constraints

- The router's rule: no git writes through the Cowork device bridge.
- Upstream's `AGENTS.md` rules hold for code changes here.
- Automatic update installation stays off until the Sparkle feed question in
  `Threads/Backlog.md` is settled.

## Related

`finder-follow` — the no-app alternative from the same research day: a zsh
plugin and a polling daemon that keep the real Finder and a real terminal in
lockstep. Delivered in chat, not adopted into `~/Obsidian`. If Tursora ever
disappoints, that is the fallback.
