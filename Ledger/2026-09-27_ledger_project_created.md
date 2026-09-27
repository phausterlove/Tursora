# 2026-09-27 — the fork, and the project around it

## 07:07 — forked from zerolfx/Tursora at 0.5.0

**Context.** A research pass this morning, looking for a Finder window with a
terminal that follows the browsed folder, found zerolfx/Tursora. Sam installed
the 0.5.0 cask and it did, in his words, *"right away … what all the other
paid-for apps wouldn't."* He asked for it forked into `~/Obsidian` as a project
in the same shape as the others.

**How it got here.** Sam forked on GitHub with `gh repo fork zerolfx/Tursora
--clone=false` → `phausterlove/Tursora`, public. The clone into
`~/Obsidian/Tursora` was run through the Cowork device bridge — allowed as of
today, see the root Ledger entry of the same date for the rule change — with
`upstream` added as a second remote and its push URL set to `DISABLED`, so a
stray `git push upstream` fails rather than opening a pull request by accident.

**Added at the root of the repo, beside upstream's layout.**

1. `bootup.md` — the project's procedure: structure, what to read, build and
   test, the two remotes, what will bite.
2. `_context/Tursora_Context.md` — tier-2 memory: why the fork exists, what
   upstream is, what is decided.
3. `Threads/Backlog.md` — the queue. Opens with the Sparkle-feed / bundle-id
   question, because a build of this fork with upstream's defaults will offer
   to update itself into zerolfx's next release.
4. `Ledger/README.md` — the protocol, mirrored from the siblings; now seven
   copies.
5. `.gitignore` — the `~/Obsidian` conventions appended under upstream's lines:
   `_tmp/`, `_to_delete/`, `_staging/`, `*.bak`, `Claude outputs/`, editor noise.

**Deliberately not changed.** Nothing under `app/`, `docs/`, `site/` or
`Casks/`. Upstream's `AGENTS.md` and `CLAUDE.md` are left as they are; whether
the fork keeps their attribution rule is in the backlog for Sam to decide. The
name, bundle id and Sparkle feed are untouched — a rename is a migration, not a
setting.

**Verified.** `origin/main`, `upstream/main` and `HEAD` all at `1f4a484` after
the clone; `git remote -v` shows both remotes; no `.lock` or `tmp_*` left under
`.git/` after clone and fetch. `git status` lists exactly the five changes
above and nothing else. **Not verified:** the build. No `swift build` was run
this session — the bridge shell is a Linux VM and cannot; the first build on
the Mac is Sam's, and its smoke-run count belongs in the next entry. The push
to `origin` is also Sam's: the bridge has no credentials.
