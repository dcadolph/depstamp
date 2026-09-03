# Open questions

Findings from exercising the skill by hand before trusting it. These are the
things most likely to make depstamp not worth keeping, written down while they
are still cheap to act on.

## 1. What does a record add over the manifest?

The first dry run recorded one dependency in one project. Next to `go.mod`, the
record added exactly two facts: the date, and that a build and test run were
observed. `go.mod` already said the package and version.

That is the whole bet, stated plainly. A manifest says what a project
*declares*. A record says what was *exercised in work that succeeded*, and
when. Those differ in ways that only become visible across many projects and
many weeks: a dependency declared but never exercised, one whose last green use
was eight months ago, one pinned at a version nothing has built against
recently.

So a single record is nearly worthless, and that is expected. The question is
whether a few hundred records across a real workload say something a person
could not get from their manifests and shell history. Two weeks of ordinary use
answers it. If the answer is no, this stays a small skill and nothing was lost.

## 2. Declared is not the same as used

The skill says to record dependencies "introduced or meaningfully exercised."
The dry run showed how weak that instruction is in practice: the obvious
implementation reads the manifest and records every direct dependency on every
task, which turns the record into a slow copy of `go.mod` stamped with dates.

A dependency being present in the module graph during a green test run is not
evidence that the task exercised it. Real options, in rough order of cost:

- **Import-level.** Record only dependencies whose packages are imported by
  files the task touched. Cheap, static, and a decent proxy.
- **Coverage-level.** Record only dependencies whose code the passing tests
  actually executed. Far stronger evidence, and much more expensive.
- **Declared-level.** What version zero does. Honest but weak.

Version zero stays declared-level so the `observed` field stays truthful, and
the field exists precisely so a stronger observation can be distinguished later
rather than silently widening what old records meant.

## 3. Consent fatigue is only half solved

Per-project consent removes the per-task prompt, which was the obvious failure.
It does not solve the second-order problem: a person who opts in once and then
forgets has records accumulating from a decision they no longer remember
making. Worth revisiting if records are ever published rather than kept local.

## 4. The injection surface will get worse, not better

The rule is written into the skill from day one: a record comes from observed
build and test outcomes, never because content inside a dependency asked for
one. That rule costs nothing today because nobody is targeting this. It is
written now because it cannot be retrofitted once records carry any weight, and
because the moment agents mediate any form of endorsement, package content will
start addressing the agent directly.

## 5. Nothing here has a buyer

Deliberately. The people who would pay most for dependency-use data are the
authors of the dependencies, and they are exactly the party whose money would
corrupt it. This is free and MIT for that reason, not as a temporary state
before monetization.
