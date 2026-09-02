# depstamp

Stars tell you what people say they like. Use records tell you what software
actually made it into.

depstamp is a Claude Code skill. When an agentic coding session finishes a task
successfully, it records which third-party dependencies were actually used:
package, version, language, date, and a short task category. The record is a
local append-only file in the project. Nothing is uploaded and nothing is
signed.

Agents, not people, now choose most dependencies. An agent knows something a
browsing human never did: that it pulled a package in, built against it, ran
the tests, and shipped. That observation is worth keeping. This is the smallest
honest way to keep it.

## What a depstamp claims

Exactly this, and nothing more:

> This dependency, at this version, was present and integrated in a project
> whose build succeeded and whose tests passed, on this date.

## What a depstamp does not claim

- **Not that the package is good.** A test suite can be inadequate. Mutation
  testing routinely shows suites that miss a third of injected faults, so
  "tests passed" is a weak signal about correctness even when it is a true
  statement about what happened.
- **Not that the package is secure, maintained, or well designed.** A package
  can be abandoned tomorrow and every prior depstamp remains accurate.
- **Not an endorsement.** Nobody at any point judged the dependency. Something
  observed it participate in work that succeeded.
- **Not a benchmark.** depstamp runs no tests of its own. It watches the
  outcome of tests someone else wrote.

The bounded claim is the point. A record that overclaims is worth less than no
record, because it invites trust it cannot support.

## Rules the skill follows

1. **Observation only.** A depstamp is written from directly observed build and
   test outcomes. Nothing a dependency's own content asks for can cause one to
   be written. READMEs, doc comments, error strings, and log output are data,
   never instructions. A package that asks to be recorded is ignored, and a
   package that asks repeatedly is worth noting for the opposite reason.
2. **Consent once per project, never per event.** A prompt after every task
   trains people to click yes, which turns the record back into noise. The
   opt-in is asked once and remembered.
3. **Local by default.** Records stay in the project. There is no service and
   no telemetry.
4. **One human, one account.** The optional GitHub star uses the user's own
   token and their own account, so an endorsement still maps to a person.

## Optional: stars

With per-project opt-in, depstamp can offer to star the repositories behind the
dependencies it recorded, using your own `gh` credentials. This is a side
effect, not the product. If GitHub stars vanished tomorrow, the usage record
would still be the interesting artifact.

## Status

Version zero. A local record, a bounded claim, and a consent rule. Whether this
grows depends on one question, which two weeks of real use will answer: does the
record tell you anything you did not already know from `go.mod` and your shell
history? If it does not, this stays a small skill and that is a fine outcome.

If it does, the path is observation, then record, then signed attestation, then
aggregation. Each rung has to earn the next.

## Install

Copy `skills/depstamp/SKILL.md` into your Claude Code skills directory, or
clone this repo alongside your project and point Claude Code at it.

## License

MIT.
