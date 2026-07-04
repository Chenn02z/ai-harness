# Product Backbone

## Product Intent

This repository is a reusable AI harness for indie developers. The harness
gives a software project durable workflow structure through specs, explicit
handovers, role-specific subagents, and stable repo terminology.

It is not an app runtime and it is not tied to one domain. Your product code,
framework choices, and runtime architecture should live alongside this harness,
not inside its core rules.

## Current Scope

- repo operating instructions and workflow skills
- milestone and spec structure
- Codex agent presets for planning, grilling, exploration, implementation,
  verification, review, and documentation
- context docs that prevent terminology drift
- generic templates for specs and milestones

## Non-Goals For Now

- runtime application code
- deployment scaffolding
- framework-specific project generators
- domain-specific examples or sample products
- team collaboration workflows beyond a single main agent coordinating subagents

## Product Principles

- **Specs define truth**: implementation traces to accepted requirements.
- **Skills define process**: repeatable workflows live in `.agents/skills/`.
- **Agents execute roles**: model, reasoning, and permission presets live in
  `.codex/agents/`.
- **Main agent owns judgment**: subagents gather, challenge, implement, and
  review; the main agent reconciles and reports.
- **The harness is product-agnostic**: domain assumptions belong in your own
  product docs and specs.
- **Model routing follows risk**: ambiguous or high-impact work gets stronger
  models; narrow exploration can use cheaper ones.
- **Bash is not the orchestration brain**.

## Success Criteria

- vague requests become clear milestones or specs
- specs are grilled before implementation
- implementation follows a consistent explorer -> implementer -> verifier ->
  reviewer loop
- docs, specs, skills, and agent presets remain aligned
- developers can add any product domain without rewriting the harness itself

## Roadmap Backbone

1. Operating contract: `AGENTS.md`.
2. Workflow foundation: requirements, spec, dev-loop, context, and test skills.
3. Agent foundation: role/model/permission presets under `.codex/agents/`.
4. Template foundation: milestone and spec templates under `docs/`.
5. Domain adoption: each downstream product adds its own accepted milestones,
   specs, code, and docs without changing the shared harness by default.

