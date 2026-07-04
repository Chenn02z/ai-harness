# AI Harness

> A specs-driven agent workspace template for indie developers.

AI Harness is a reusable starter repo for building software with a strict
multi-agent development loop instead of one giant prompt. It gives you:

- a repo operating contract in `AGENTS.md`
- reusable workflow skills under `.agents/skills/`
- reusable agent presets under `.codex/agents/`
- docs and templates for milestones, specs, handoffs, and context alignment

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

1. Shape the request with `$requirements`.
2. Accept a milestone in `docs/milestones/` when the work is large enough.
3. Draft an implementation spec with `$spec`.
4. Pressure-test the spec before implementation.
5. Implement through `$dev-loop`.
6. Verify through `$test`.
7. Realign docs, skills, and agent rules through `$context` when decisions settle.

## Repository Layout

```text
.
├── AGENTS.md
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
2. Rewrite `docs/PRODUCT.md` for your actual product.
3. Adjust `docs/CONTEXT.md` only where your product introduces durable terms.
4. Keep the workflow docs and agent presets strict unless you intentionally
   change the harness rules.
5. Add your application code beside this harness structure.

## Non-Goals

- application runtime code
- framework-specific scaffolding
- domain-specific examples
- historical traces, accepted milestones, or completed specs from another repo
