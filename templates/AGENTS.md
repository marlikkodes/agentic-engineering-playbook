# AGENTS.md — annotated template

Copy this into the root of a repository and delete what does not apply. Every section exists because
its absence caused a specific class of bad output. The comments explain which one.

---

## Project

One paragraph: what this system does, who uses it, and what breaking it would cost.

> Why: without stakes, an agent optimises for the change that closes the ticket. With stakes, it asks
> before touching the parts that matter.

## Architecture in five lines

- Runtime and framework, with the version that actually matters.
- Where server code ends and client code begins.
- Where data lives and who is allowed to talk to it.
- How authentication works, in one sentence.
- What is deployed where.

> Why: agents infer architecture from whatever file they opened first. Five lines here prevents an
> afternoon of plausible code written against an imagined structure.

## Ground rules

- Never commit secrets, tokens, `.env` files, logs, database dumps or archives.
- Never widen a permission, scope, role or access rule; propose it in the diff description instead.
- Never add a dependency without noting why an existing one does not do the job.
- Never rewrite history, force-push, or touch release branches.
- Prefer editing an existing module over creating a parallel one.

> Why: these are the mistakes that are expensive to undo rather than annoying to review.

## Boundaries

**Free to change:** feature code, tests, docs, styles.

**Ask first:** database schema and migrations, authentication and session handling, API contracts,
infrastructure and deployment configuration, anything under a security-review path.

**Never:** access control rules, credentials, third-party account settings, CI secrets.

> Why: capability is not the constraint; consequence is. This section is where the real safety lives.

## Conventions

- Language, formatter and linter settings are already configured — run them, do not argue with them.
- Naming and file-layout patterns, with one example of each.
- Commit style, including the prefixes this repository uses.
- Test expectations: what must exist before a change is considered done.

> Why: half of all review comments on agent output are convention comments. Convention comments are
> a documentation failure, not a model failure.

## Definition of done

A change is finished when it builds, type-checks, passes tests and lint, includes tests for new
behaviour, updates any documentation it invalidates, and arrives as a diff a reviewer can read in
one sitting.

> Why: "done" is the single most useful thing to define precisely. Everything else is negotiable.

## Verification commands

```bash
# install
# build
# type-check
# test
# lint
```

> Why: an agent that can verify its own work produces far fewer review cycles. Give it the exact
> commands rather than making it guess from the package manifest.

## Known traps

A short list of the mistakes this repository invites — the caching quirk, the hydration mismatch, the
environment variable that must be used for redirects, the module that looks dead but is not.

> Why: this is the highest-value section in the file and the only one that improves by itself. Add a
> line every time a review catches the same thing twice.

## Out of scope

What this repository deliberately does not do, and where that work belongs instead.

> Why: prevents helpful expansion into other people's systems.
