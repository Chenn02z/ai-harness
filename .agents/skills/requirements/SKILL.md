---
name: requirements
description: Produces requirements packets and Accepted milestone contracts for product work. Use when gathering requirements, clarifying product direction, deciding scope, or accepting milestone direction before spec creates implementation contracts.
---

# Requirements Workflow

Use before writing implementation specs when the request is still ambiguous or
when milestone direction must be accepted before spec work starts.

## Prerequisites

1. `AGENTS.md` (one-time orientation)
2. `docs/WORKFLOWS.md` (handoff interface)

## Workflow

1. Start `$codex-agent-tracer` before substantial work.
2. Resolve the request: milestone contract and acceptance, requirements packet,
   context decision, or implementation request needing a spec first.
3. Use `$grill-with-docs` to pressure-test direction.
4. Use `spec-planner` for scope, scenarios, and acceptance criteria candidates.
5. Use `spec-griller` to challenge ambiguity, failure modes, and scope creep.
6. The main agent settles decisions with the user.
7. For milestone work: use `doc-curator` to write or update the milestone under
   `docs/milestones/`. Only mark `Accepted` after all blocking questions resolve.
8. If scope, terms, or boundaries change, hand off to `$context`.
9. Hand Accepted milestones to `$spec` for spec creation.

## Blocking Questions

Blocking questions must be answered by the repo or the user before milestone
acceptance. If unanswered, return Draft or blocked, not Accepted. Deferred
questions may remain only when explicitly listed as non-blocking and assigned
to child specs or later milestones.

## Output

Return a handoff artifact using the shared interface in `docs/WORKFLOWS.md`.
Include the resolved workflow, proposed spec path, milestone path and status,
scope boundary, scenarios, acceptance criteria candidates, settled decisions,
trace path, and blocking questions.

