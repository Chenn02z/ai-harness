# AI Harness

> A specs-driven agent workspace template for indie developers.

AI Harness is a reusable starter repo for building software with a strict
multi-agent development loop instead of one giant prompt. It gives you:

- a repo operating contract in `AGENTS.md`
- reusable workflow skills under `.agents/skills/`
- reusable agent presets under `.codex/agents/`
- docs and templates for milestones, specs, handoffs, and context alignment
- a first-run bootstrap path for turning the template into a real product repo

This repo is the harness, not your product runtime. Bring your own app, domain,
and codebase. The harness exists to keep planning, implementation, review, and
documentation aligned as your project grows.

## Who It Is For

Solo and indie developers who want:

- explicit milestone and spec gates
- repeatable multi-agent workflows
- stable repo terminology
- traceable handoffs between planning, implementation, testing, and docs

## Workflow Backbone

1. Run `$bootstrap` immediately after forking to interrogate product direction
   and rewrite the template docs into a real project backbone.
2. Shape the request with `$requirements`.
3. Accept a milestone in `docs/milestones/` when the work is large enough.
4. Draft an implementation spec with `$spec`.
5. Pressure-test the spec before implementation.
6. Implement through `$dev-loop`, which includes verification by delegating to
   `$test`.
7. Invoke `$test` directly for standalone or ad hoc verification when you do
   not need the full dev loop.
8. Realign docs, skills, and agent rules through `$context` when decisions settle.

## Repository Layout

```text
.
├── AGENTS.md
├── user-journeys.html
├── .agents/skills/
├── .codex/agents/
└── docs/
    ├── adr/
    ├── PRODUCT.md
    ├── CONTEXT.md
    ├── WORKFLOWS.md
    ├── AGENT_ROLES.md
    ├── DOCS_POLICY.md
    ├── milestones/
    ├── specs/
    └── techdebt/
```

## How To Use

1. Start from this repo as a template.
2. Run `$bootstrap` as the first real task after the fork.
3. Answer the grilling session until `docs/PRODUCT.md`, `docs/CONTEXT.md`,
   project-local `AGENTS.md` details, and the first milestone are coherent.
4. Use `user-journeys.html` at the repo root as the visual map of what the
   harness supports and what the intended developer paths are.
5. Keep the workflow docs and agent presets strict unless you intentionally
   change the harness rules.
6. Add your application code beside this harness structure.

## Non-Goals

- application runtime code
- framework-specific scaffolding
- domain-specific examples
- historical traces, accepted milestones, or completed specs from another repo
