# Tool scoping

What an agent is allowed to touch matters more than how good the agent is. This note describes how we
decide, and how we keep the decision small enough to hold in your head.

## The test we apply

Before exposing a capability to an agent, answer three questions:

1. **What is the worst plausible outcome of this being called wrongly?** Not the worst imaginable — the
   worst plausible. If the answer involves permissions, money, customer data or anything irreversible,
   the tool does not get exposed; a human-approved path does.
2. **Is the outcome visible?** A tool whose effect appears in a diff, a log or a queue is safe to iterate
   on. A tool whose effect is silent is not.
3. **Is it reversible in one step?** If undoing it requires archaeology, it belongs behind a human.

## Three tiers

**Tier 1 — read and propose.** Reading code, searching, running tests, drafting diffs. Exposed freely,
because the output is reviewable and nothing changes without a merge.

**Tier 2 — write with a paper trail.** Creating branches, opening pull requests, writing files, running
migrations against a local database. Exposed, but every action lands somewhere a human will see it.

**Tier 3 — consequential.** Anything that touches production data, access control, third-party accounts,
spending, publishing or deletion. Never exposed. If an agent needs the outcome, it prepares the change
and a human executes it.

The tier boundaries are not about trust in the model. They are about whether a mistake costs a review
comment or a phone call.

## Narrow beats general

A tool called `complete_task(id)` is better than `call_api(method, path, body)` even though the second
is strictly more capable. Reasons, in order of how often they matter:

- Errors are meaningful. "Task not found" is actionable; "400 Bad Request" is a puzzle.
- The model spends its attention on the problem rather than on the API surface.
- The blast radius is written in the signature, so scoping decisions survive refactors.
- Rate limits, retries and pagination live in one place instead of in a prompt.

The corollary: when a routine operation becomes load-bearing for a feature, promote it from a
general-purpose protocol tool to an explicit typed call. That decision is recorded as
[ADR 0003](https://github.com/kishoresahas/engineering-notes/blob/main/adr/0003-rest-over-mcp-for-core-integrations.md)
in a related repository.

## Protocol tools versus written clients

Protocol-based tooling (MCP and similar) is excellent for breadth: exploring an unfamiliar system,
one-off analysis, work where you do not yet know which calls you need. Written clients are for the
operations a feature promises to a user. Mixing them is fine; leaving the boundary undocumented is not,
because the next person cannot tell which failures they own.

## Anti-patterns

- **Shell access as a convenience.** It collapses every tier into one.
- **Credentials in the agent's context** so it can "handle auth itself".
- **A tool that can widen its own permissions.** Access control is never a tool.
- **Adding capability to fix a bad result.** Usually the instructions were unclear, not the toolset
  incomplete.
- **One tool per API endpoint.** Model the task, not the vendor's routing table.
