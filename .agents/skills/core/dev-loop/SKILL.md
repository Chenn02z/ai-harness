---
name: dev-loop
description: Runs the specs-driven implementation loop for product work. Use when an Accepted spec should be implemented, verified, reviewed, and summarized with role-specific subagents.
---

# Dev Loop

Use this skill after a spec is Accepted, or when the user explicitly marks a
small task as a spec exception.

## Prerequisites

Read these before delegating any work:

1. The accepted spec under `docs/specs/`.
2. `docs/WORKFLOWS.md` (handoff interface and spec status contract only).

Do not pre-read `AGENTS.md`, `docs/PRODUCT.md`, or `docs/CONTEXT.md` unless
the task actually needs them.

## Workflow

1. Start `$codex-agent-tracer` before substantial work.
2. Read only the accepted spec and the prerequisites above.
3. Delegate exploration to `explorer`. Do not read source files yourself.
4. Confirm any spec gap with the user before implementation.
5. Delegate implementation to `implementer`. Do not edit files yourself.
   Batch reviewer findings into a single implementer pass.
6. Delegate verification to `$test` in dev-loop mode. Submit once per
   implementer pass. If failures occur, send them back to `implementer` as one batch.
7. Delegate review to `reviewer`. Collect findings, deduplicate, then send
   them as one batch to `implementer`.
8. If reviewer findings trigger another implementer pass, re-verify with
   `$test` before marking the spec Verified.
9. Update the spec status after the final review and verification pass.
10. Leave a follow-up handoff for `$context` or `$spec` when implementation
    settles terminology, changes scope, or exposes a spec gap.

## Guardrails

- Do not let parallel agents edit the same files.
- Write-capable agents must have disjoint file ownership.
- Do not expand scope without updating the spec first.
- Prefer targeted tests and checks over broad commands early.
- Delegate, do not DIY: explorer reads, implementer edits, test verifies,
  reviewer reviews. Main agent reconciles and reports.

## Output

Return a handoff artifact using the shared interface in `docs/WORKFLOWS.md`.

Include:

- spec path
- files changed
- verification commands and results
- reviewer findings
- trace path and duplicated-work findings
- remaining risks or follow-up specs

