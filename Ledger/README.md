# Ledger — Tursora

A dated, chronological record of **system work done at this level**, written
at the end of the session that did it.

## What earns an entry here

**System work.** The shape of this fork: what was changed in `app/` and why,
what came in from upstream and what it broke, decisions about naming, updates,
signing, the build. Changes to how the project *works*.

**Not the routine.** A `swift build` that passed, a merge that fast-forwarded
cleanly, a session that read the code and changed nothing — that is the work
this project exists to hold and needs no record beyond git's own.

The test: *would a thread six months from now need this to understand why the
fork is shaped the way it is?*

## The protocol

### 1. Check the date and time before writing anything

Not from memory, and not from the conversation — from the system:

```bash
date "+%A %Y-%m-%d %H:%M %Z"
```

Everything after it depends on the answer, and the common failure is a thread
that assumes the date from context and files a session under the wrong day. A
session that began in the evening and ran past midnight is the case that
catches you.

### 2. Look for today's file

```bash
ls Ledger/
```

- **`<today>_ledger_*.md` exists** → append a section to it. Do not create a
  second file for the same day.
- **No such file** → create `YYYY-MM-DD_ledger_<theme>.md`.

The date is the date the session *began*, so work running past midnight stays
in one file.

### 3. Write at the end, not the start

A ledger written before the work describes intentions. Write it once the work
has landed, so it describes what is true. If a session is interrupted, write
what did land and say plainly what was left mid-flight.

### 4. Append; never quietly revise

New sessions add sections. Earlier entries are not edited to match what was
learned later — a wrong entry is corrected by a **new** entry that states the
original claim and marks it withdrawn. That is the standing convention in the
Retrospectives Archive and it applies to every ledger here.

### 5. Say what was verified

An entry claiming something works is worth much less than one naming the check
that proved it and the result. **If nothing was verified, say that too** — it
is useful to know which changes are still standing on assumption.

---

## A note on the copies

Everything above except *What earns an entry here* is the same in
`~/Obsidian/Ledger/`, `Garden Tracker/Ledger/`, `HealthData/Ledger/`,
`Retrospectives Archive/Ledger/`, `Live Looping/Ledger/` and
`Connexus Tracker/Ledger/`. That is seven copies of one protocol, which is
exactly the mirror this vault keeps getting bitten by.

It is deliberate here, for one reason: the repos can be cloned apart, and a
Ledger whose instructions live in a sibling repo is a Ledger with no
instructions the moment someone clones just one. The duplication buys
independence, and it is stable text rather than a list that grows.

It is still a mirror. If the protocol changes, it changes in every copy.
