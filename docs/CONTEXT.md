# Workspace Context

Canonical terminology and workflow boundaries. Keep implementation details out.

## Canonical Terms

- `workspace`: this repo and its docs, skills, specs, and agent presets.
- `developer`: the human using this repo for specs-driven agentic work.
- `main agent`: the Codex thread owning judgment, delegation, reconciliation,
  user interaction, and final reporting.
- `subagent`: a delegated Codex development agent with a narrow role, model,
  reasoning, and permission profile.
- `skill`: a reusable workflow under `.agents/skills/`.
- `agent preset`: a `.codex/agents/*.toml` role/model/permission default.
- `requirements packet`: pre-spec output of `requirements` with resolved
  workflow, proposed spec path, scope, scenarios, criteria, and blockers.
- `Accepted milestone`: a `docs/milestones/` contract with all blocking
  questions resolved. Eligible for `spec`; does not authorize implementation
  without an Accepted child spec.
- `handoff artifact`: explicit producer-to-consumer artifact naming the
  producing skill, intended consumer, path, status, decisions, blockers, and
  agent routing log.
- `spec`: executable contract under `docs/specs/` with goal, scope, contracts,
  failure modes, acceptance criteria, verification, and open questions.
- `milestone`: larger deliverable slice under `docs/milestones/`.
- `requirement gathering`: vague intent to requirements packet or Accepted
  milestone.
- `spec grilling`: adversarial review before implementation.
- `dev loop`: Accepted spec to verified, reviewed diff.
- `context maintenance`: keeping docs, specs, skills, and agents aligned after
  decisions settle.
- `trace`: workflow evidence under `.agent-trace/<workflow-id>/`.

## Reference Documents

- `AGENTS.md`: Codex orientation and operating contract.
- `docs/PRODUCT.md`: product intent, scope, principles, and roadmap.
- `docs/WORKFLOWS.md`: handoff interface and status contract.
- `docs/AGENT_ROLES.md`: subagent roles, permissions, and routing principles.
- `docs/DOCS_POLICY.md`: documentation destinations and status rules.

## Workflow Boundaries

- Requirements: produces milestone contracts and requirements packets. Does
  not write specs or implement code.
- Specs: produces executable contracts under `docs/specs/` from Accepted
  milestones only. Does not modify milestones.
- Draft milestones must be reopened through `requirements`.
- Dev loop: implements from Accepted spec only. Reports changed files,
  verification, review findings, and follow-up handoffs.
- Review: compares the diff to the spec; it is not open-ended redesign.
- Context maintenance: runs when settled work changes terminology, boundaries,
  product direction, roles, or agent/skill usage.
- `grill-with-docs`: pressure-tests against docs; it does not replace the main
  workflows.
- Read-only subagents may run in parallel if independent. Write-capable
  subagents need disjoint file ownership.
- Bash scripts may run repeatable checks, not own agent routing.

## Idle Triage

When no explicit developer task is active and the workspace has Accepted
milestones without child specs, or Draft milestones that are unblocked, the
main agent should ask which milestone or spec should advance next and surface
the next-action candidates with their status and blockers.

Accepted milestones with no spec are the highest-priority idle candidates.
Draft milestones with resolved dependencies follow.

## Open Questions

- What trace retention policy is appropriate for this repo?

