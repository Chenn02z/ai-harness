---
name: spec
description: Converts Accepted milestone contracts into implementation specs under docs/specs. Reject Draft or non-Accepted milestones; do not update milestone contracts.
---

# Spec Workflow

Use when an Accepted milestone needs one or more executable implementation
specs under `docs/specs/`. Consume Accepted milestone contracts from
`$requirements`. Reject Draft, blocked, or non-Accepted milestones.

## Prerequisites

1. `AGENTS.md` (one-time orientation)
2. `docs/WORKFLOWS.md` (handoff interface and spec status contract)
3. The Accepted milestone under `docs/milestones/`

## Spec Template

```md
# Spec: Title

## Status
Draft

## Goal

## Scenario

## In Scope

## Out Of Scope

## Contracts

## Failure Modes

## Acceptance Criteria

## Verification

## Open Questions
```

## Workflow

1. Start `$codex-agent-tracer` before substantial work.
2. Use `explorer` if repo state is unclear. Confirm the milestone is `Accepted`.
3. Use `spec-planner` to draft or revise specs from the Accepted milestone.
4. For multi-spec milestones: name child spec paths and Draft or Accepted targets.
5. Use `spec-griller` before marking any spec Accepted.
6. Use `$grill-with-docs` when spec changes terminology, boundaries, or
   milestone direction.
7. If code reveals the milestone is wrong, hand back to `$requirements`.
8. If the spec settles context changes, hand off to `$context`.
9. Leave an explicit handoff artifact for `$dev-loop` when ready.

## Required Agent Gates

1. `spec-planner` must draft or revise the spec.
2. `spec-griller` must review before Accepted.

## Output

Return a handoff artifact. Include the source milestone path, created or
revised spec path, key contracts and acceptance criteria, settled decisions,
open questions, trace path, and verification expectations.

