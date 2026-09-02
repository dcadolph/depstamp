# Record format

Version zero. One JSON object per line in `.depstamp/records.jsonl`, appended
and never rewritten. An append-only file is the whole storage design: it merges
cleanly, it is readable without tooling, and a record that was true when written
stays true.

## Fields

| Field | Meaning |
| ----------- | ------- |
| `schema` | Format version, currently `depstamp/v0`.&nbsp; |
| `package` | The dependency's canonical name in its ecosystem.&nbsp; |
| `version` | The exact resolved version in use, not a range.&nbsp; |
| `ecosystem` | `go`, `npm`, `pypi`, `cargo`, and so on.&nbsp; |
| `project` | Local project name. Not a path, and never an absolute one.&nbsp; |
| `task` | Coarse category: `testing`, `http`, `cli`, `parsing`, `data`, `build`, `other`.&nbsp; |
| `observed` | What was actually seen. Currently always `build+tests`.&nbsp; |
| `date` | Calendar date, `YYYY-MM-DD`. Deliberately not a timestamp.&nbsp; |

## What is deliberately absent

- **No identity.** No user, machine, org, or repository URL. A record says work
  happened, not who did it.
- **No paths.** An absolute path leaks a name and a directory layout.
- **No timestamps.** A date is enough to answer whether use is recent. A
  timestamp is enough to reconstruct someone's working hours.
- **No signature.** Version zero is a local note. Signing a record nobody
  consumes is infrastructure ahead of validation. If aggregation ever earns
  its place, the signed form becomes `depstamp/v1` and this format stays
  readable.
- **No score, rank, or verdict.** There is no field for how good the dependency
  is, because nothing here measures that.

## Why `observed` exists

It names the evidence behind the claim rather than leaving it implied. Today it
has one value, `build+tests`. If a future version records a weaker observation,
such as a build with no test run, the field distinguishes the two instead of
silently widening what a record means.

## Reading

A record is one observation, not a vote. Counting records tells you how often a
dependency participated in successful work in one person's projects. It does not
tell you the dependency is good, and any aggregate view built on this format
should say so where a reader can see it.
