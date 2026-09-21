# Review gates

Agent-written code is not special. It goes through the same door as everything else — the door is just
used far more often, so it has to be cheap to pass and impossible to walk around.

## Machine gates (before a human looks)

A pull request is not ready for review until all of these pass. None of them requires judgement, which
is exactly why a machine should own them.

1. Build succeeds.
2. Types check with no new suppressions or escape hatches.
3. Tests pass, and new behaviour has a test that fails without the change.
4. Lint and format are clean.
5. No secrets, credentials, `.env` files, logs, dumps or archives in the diff.
6. Dependency changes are declared in the description with a reason.
7. The diff is scoped: unrelated files, drive-by reformatting and generated noise are rejected.

A request that fails a machine gate goes back to the agent, not to a reviewer. Reviewer attention is
the scarcest thing in this loop; spending it on a type error is the most expensive mistake in the
process.

## Human gates (always, regardless of who wrote the code)

A person reads, understands and takes responsibility for:

- **Anything touching authentication, sessions or tokens.**
- **Anything touching access control.** No exceptions and no automation, ever.
- **Schema changes and migrations**, including how they behave on rollback.
- **API contract changes**, including whether anything downstream breaks.
- **Cost and rate-limit implications** of new external calls.
- **Deletions**, especially of code that looks unused.
- **Anything that changes what a user sees** — the model does not know your product's voice.

The reviewer's question is not "is this plausible?" It is "do I understand why this works, and would I
have written something equivalent?" Plausibility is what agents are best at, which is precisely why it
is a useless standard.

## Signals that a diff needs more scepticism, not less

- Confident code in an unfamiliar corner of the system.
- New abstraction introduced to solve one instance of a problem.
- Tests that assert the implementation instead of the behaviour.
- Error handling that swallows rather than surfaces.
- A change that grew while being fixed, rather than shrinking.
- Comments explaining what the code does rather than why.

## The feedback obligation

Every rejection has to produce one of three things, or the loop does not improve:

1. A line added to the agent instructions.
2. A test that will catch the class of error next time.
3. A narrower tool, or a boundary moved.

If a rejection produces none of these, the same diff will arrive again next week. Recurring review
comments are a documentation bug with a very slow feedback cycle.

## Attribution

The author of a pull request is the person who opened it. Noting that an agent produced the first draft
is useful context for reviewers and useless as a defence. Nobody wants to hear that the model wrote the
regression.
