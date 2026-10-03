# 2026-10-03 — new toolchain, first full green runs on the M4

## 08:45 — smoke suite green three times in a row; five commits

**Context.** The Command Line Tools were reinstalled, replacing the macOS 15
SDK toolchain with Swift 6.3.3 (swiftlang-6.3.3.1.3, target
arm64-apple-macosx26.0; `swift --version` checked at session start). The goal
was the suite green three times in a row on this machine for the first time.
`app/.build` was removed before the first build.

**Where the suite stood.** Under the old SDK it stopped at check 1,228 (a tab
appearance pixel comparison), and with the colour-space fix applied it stopped
in Archive operations on "ZIP recovery controls do not overlap at 160 points",
buttons laid out 16 pt tall but carrying 27 pt frames. The hypothesis that the
overlap was an artefact of building with the macOS 15 SDK on macOS 26 was
**confirmed**: on the new toolchain that check passes with correct 14–20 pt
control frames, with no change to app or test. Three further failures then
surfaced, each diagnosed with a looped single suite and fixed in its own
commit.

**The commits, and what proved each.**

1. `21783aa fix(terminal): accept the shell's own hostname in OSC 7 reports` —
   "a cd typed in zsh reports its folder over OSC 7" failed ~30% of full runs:
   the zsh/bash/fish hooks report `gethostname()` (zsh's `$HOST`,
   `sam-m4.local`) in their OSC 7 URLs, while `localHostNames` was built only
   from `ProcessInfo.hostName`, a reverse-DNS lookup that on this Starlink
   uplink intermittently resolves to `customer.sttlwax1.isp.starlink.com`.
   Whenever the two disagreed for a run, every shell report was rejected as a
   remote URL — in the app, not just the test, so browser-follows-shell
   silently broke for whole sessions on this network. `localHostNames` now
   always contains the kernel hostname, and a new pure check requires it.
   Proof: the diagnostic print of both names on a failing run, then the
   Terminal shell sync suite 15/15 green (3/10 failing before).
   **This fix is a candidate for an upstream PR** — it is upstream's bug on
   any machine whose reverse DNS disagrees with `gethostname()`.

2. `21d8c5a fix(smoke): compare the sampled tab-strip pixel as sRGB
   components` — the pre-existing colour-space fix, kept from the previous
   session: `bitmapImageRepForCachingDisplay` can return a bitmap tagged
   NSCalibratedRGB whose pixels are still the sRGB components the view drew,
   shifting luminance ~0.03 past the 0.025 tolerance. Proof: the suite's first
   failure moved from check 1,228 past 27 suites, and the check passes in
   every run since under the new SDK.

3. `9162ce9 test(smoke): run a single suite with TURSORA_SMOKE_ONLY` — suite
   filter for the steps table, added because diagnosing the above meant
   looping one suite instead of paying minutes per full run. Documented in
   AGENTS.md's commands block. Proof: the unfiltered path is unchanged and the
   three final full runs below exercise it.

4. `4b36bf2 test(search): keep a real field editor through the smoke typing
   helper` — "Return has a real field editor" failed deterministically with
   the suite run standalone and intermittently in full runs: setting a
   control's `stringValue` while it is edited can end the editing session,
   and reliably does while the toolbar holds the search field expanded from
   its icon (the state of a never-shown smoke window). Harness fix only — the
   helper re-asserts focus after setting the value; step-by-step diagnostic
   prints showed the editor dying exactly at the `stringValue` assignment.
   The app's Return handling was not at fault. Proof: Search entry suite
   12/12 green standalone at 129 checks (9+/12 failing before).

5. `6e6a48d test(shortcuts): poll the native menu checks across desktop focus
   churn` — surfaced during the first three-green attempt (run 2 failed):
   "native menu resolves the owned active pane" read `NSApp.target(forAction:)`
   once, right after focusing the fixture window, and another application
   activating mid-run (this desktop was in use) cleared key and main window
   between the two — the check's own diagnosis showed `active=false, key=-1,
   main=-1`. Resolution and dispatch now re-assert the window and poll.
   Proof: Shortcuts suite 8/8 green standalone, then the full runs below.

**Verified.** Three consecutive full-suite runs, `TURSORA_SMOKE_TEST=1`, all
`SMOKE TEST PASSED`, exit 0, **4,919 checks each**. Working tree clean after
the five commits above. **Not verified:** real-window behaviour of the two
harness-only fixes (no screen access this session), and nothing was pushed —
the push belongs to `sync-repos.command` or a terminal, per the router.

**Deliberately not changed.** Bundle id, Sparkle feed, everything under
`app/tools/`. No AI attribution trailers, per AGENTS.md rule 8, which this
fork keeps; the harness's default attribution instruction was overridden by
that rule, said here as the rule requires.
