# bootup.md — Tursora

You arrived here from `~/Obsidian/bootup.md`. If you did not, go do its four
universal steps first — date from the system, verify the skills, read tier-1
memory, and stay inside this project.

**Scope: this directory only.**

---

## What this is

Sam's fork of [zerolfx/Tursora](https://github.com/zerolfx/Tursora), a native
macOS file manager (Swift + AppKit, MIT) whose integrated terminal follows the
folder being browsed, in both directions, without an explicit click. That one
behaviour is the reason the fork exists: Path Finder and the paid alternatives
all needed a click, and this did not. Forked 2026-09-27 at upstream 0.5.0.

There is **no skill** for this project. This file is the procedure. Upstream's
`AGENTS.md` is the other half of it — its coding rules still hold here (the
smoke test verifies every change; views never touch the filesystem; no modals
on a headless run), and it names the documents to read for any given task.

## The structure

Upstream's layout, with the Obsidian project files beside it at the root:

```
app/                 the Swift package — Sources/Tursora/{Model,UI}, the in-app smoke tests, tools/
docs/                upstream's ARCHITECTURE, SPEC, DECISIONS, ROADMAP, DEVELOPMENT, gaps/, research/
site/                upstream's website
Casks/               upstream's Homebrew cask
AGENTS.md            upstream's rules for coding agents — read it
Threads/             running work in this fork: the backlog, and one file per open thread
_context/            tier-2 memory
Ledger/              system work
_tmp/                session scratch, gitignored, cleared whenever
```

**Model/ is headless, UI/ is AppKit.** That split is what makes the smoke suite
possible without a screen, and it is the rule to keep: nothing in `Model/`
imports a view.

## Read before you write

| Read | Why |
|---|---|
| `_context/Tursora_Context.md` | why the fork exists, what Sam wants from it, what is decided |
| `Threads/Backlog.md` | what is queued, so a session does not re-derive it |
| `AGENTS.md` | upstream's working rules; the smoke-test discipline |
| `docs/ARCHITECTURE.md` | the object graph — five minutes that saves an hour |
| `docs/DEVELOPMENT.md` | toolchain, build, packaging, known AppKit traps |
| `app/Sources/Tursora/Model/TerminalShellIntegration.swift` and `TerminalDirectorySync.swift` | **before touching anything about the terminal following the folder** — the FIFO and OSC 7 design is the feature |
| `Ledger/` (last entry or two) | what the previous session changed and verified |

## Build and test

Command Line Tools with Swift 6.2+ and the macOS 26 SDK. No Xcode project.

```bash
cd ~/Obsidian/Tursora/app
swift build                                   # debug build
TURSORA_SMOKE_TEST=1 .build/debug/Tursora     # the test suite — in-app, headless; green 3× before committing
tools/make-app.sh && open build/Tursora.app   # release bundle
```

The smoke test is the application itself in a self-check mode, not XCTest. A
modal dialog hangs it, which is why error paths check `SmokeTest.isRequested`.
After an interrupted run: `find "$TMPDIR" -maxdepth 1 -name "tursora-*" -exec rm -rf {} +`.

## Two remotes

```
origin     https://github.com/phausterlove/Tursora.git    the fork — sync-repos pushes here
upstream   https://github.com/zerolfx/Tursora.git         zerolfx — pull from, never push to
```

Taking upstream's changes is `git fetch upstream && git merge upstream/main`,
from a terminal on the Mac. The project files at the root (`bootup.md`,
`_context/`, `Ledger/`, `Threads/`) are ours alone and never conflict; the
merge is only ever about `app/` and `docs/`.

**Never run git writes through the Cowork device bridge** — the router's rule,
and it applies here with more force than usual because a fresh `swift build`
plus a stale `index.lock` is a confusing afternoon.

## What will bite you

**The built app will try to update itself into upstream's build.** `app/tools/make-app.sh`
bakes in the bundle id `com.tursora.Tursora` and a Sparkle feed pointing at
zerolfx's releases. A build of this fork, left with its defaults, will offer
0.6.0 from upstream and, if automatic installation is on, take it. Open thread
in `Threads/Backlog.md`; until it is settled, keep automatic updates off in
Settings → Updates.

**Ad-hoc signed.** Local builds normally launch; a downloaded copy needs
`xattr -dr com.apple.quarantine`. Do not add `sudo`, do not change system
protections.

**Auto-follow is zsh only.** bash and fish apply the folder at their next prompt
draw; any other shell needs *Restart in Current Folder*. Nothing is ever typed
into the running session — that is the design, not a limitation to fix.

**Upstream is one author moving fast**, with a harness. Two weeks, 64 commits,
52k lines. Expect `upstream/main` to move a lot and to occasionally restructure.
Merge often rather than rarely.

## Finishing

Write the Ledger entry — `Ledger/README.md` has the protocol. Say what was
verified and by which check; the smoke run's assertion count is a fine thing
to name. Then commit and push from a terminal or via `~/Obsidian/sync-repos.command`.
