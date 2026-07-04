---
name: bootstrap
description: Bootstraps a fresh fork of the harness into a real product workspace through an adversarial founder interview and durable doc creation. Use when a developer has just forked the repo, the product docs still reflect the harness template, or the project direction is not yet coherent enough for normal requirements work.
---

# Bootstrap Workflow

Use this skill as the first serious action after forking the harness for a new
product. The goal is to turn template scaffolding into a product-specific
backbone before normal milestone and spec work begins.

## Prerequisites

1. `AGENTS.md` (one-time orientation)
2. `README.md` (workspace overview and first-run framing)
3. `docs/WORKFLOWS.md` (handoff interface and status language)
4. `docs/DOCS_POLICY.md` (durable destinations)
5. Current `docs/PRODUCT.md` and `docs/CONTEXT.md` (template state to replace)

## Workflow

1. Start `$codex-agent-tracer` before substantial work.
2. Inspect whether the repo is still in harness-template state or already has
   downstream product direction.
3. Use `$grill-with-docs` to pressure-test vague product language and force
   concrete boundary decisions before docs are written.
4. Delegate repo-state reading to `explorer`.
5. Delegate ambiguity and failure-mode pressure-testing to `spec-griller`.
6. Keep interviewing the developer until the blocking fields are resolved or
   explicitly deferred as non-blocking:
   - target user
   - primary pain
   - promised outcome
   - first platform or surface
   - monetization or business model assumption
   - launch slice
   - explicit non-goals
   - hard constraints
   - success metric
   - command and environment expectations known so far
7. If the answers are still fuzzy or contradictory, stop with a Draft onboarding
   packet rather than pretending the product is ready.
8. Delegate durable doc updates to `doc-curator`:
   - rewrite `docs/PRODUCT.md` for the real product
   - update `docs/CONTEXT.md` with durable terminology only
   - add project-local commands and constraints to `AGENTS.md` when known
   - create or update the first milestone under `docs/milestones/` if the
     launch slice is large enough
9. Hand the resulting workspace to `$requirements` when additional milestone
   shaping is needed, or directly to `$spec` only when an Accepted milestone
   already exists.

## Blocking Questions

Bootstrap is blocked until the repo can answer, from docs or the user, who the
product serves, what pain it solves, what the first useful slice is, and which
durable constraints the rest of the workflows must respect.

Unknown setup or verification commands are not always blockers, but they must
be written down in `AGENTS.md` as maturity gaps instead of silently ignored.

## Output

Return a handoff artifact using the shared interface in `docs/WORKFLOWS.md`.
Include:

- onboarding status (`Draft` or `Accepted`)
- changed doc paths
- settled product decisions
- unresolved blockers
- initial milestone path if one was created
- trace path
- agent routing log, including any manual fallbacks
