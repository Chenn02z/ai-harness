# Milestone: Local Workflow Observability MVP

## Status

Accepted

## Goal

Give developers a local, one-shot view of completed harness workflow evidence
without changing the canonical trace or coupling trace producers to Phoenix.

## MVP Deliverable

After implementation, a normal harness workflow that creates `.agent-trace`
JSONL will be importable as one completed trace ending in `trace_end` into
locally running Phoenix OSS through a private JSONL-to-OTLP bridge. A local
development-only Docker Compose command may manage Phoenix for this milestone;
product deployment scaffolding is not part of it. OTLP will be a visualization
projection; `.agent-trace` will remain canonical evidence and the input to
`trace-doctor`.

## Developer Workflow

1. Run a normal harness workflow and produce `.agent-trace` JSONL.
2. After this milestone is implemented, run the one-shot local import for a
   completed trace.
3. After this milestone is implemented, inspect workflow and agent execution
   in Phoenix OSS locally.

The projection visualizes workflow and skill boundaries, agent delegation and
completion, document/source reads, tool and command calls, file edits,
tests/verification, reviews, and handoffs only when source records exist.
Only the current trace record format is supported. Legacy `{event: ...}`
records are not supported. Absent or unknown facts remain absent or unknown
and are never inferred.

## In Scope

- Every exported OTLP attribute has an explicit fixed key. Values may come
  only from child-spec static enumerations or these constrained patterns:
  safe relative paths match `^[A-Za-z0-9][A-Za-z0-9._-]{0,127}(?:/[A-Za-z0-9][A-Za-z0-9._-]{0,127})*$`;
  executable names match `^[A-Za-z0-9][A-Za-z0-9._-]{0,63}$`; and workflow,
  skill, and role names match `^[a-z0-9][a-z0-9._-]{0,63}$`.
- Test identifiers, parameterized or arbitrary names, and other content-bearing
  values are replaced with a static non-content-safe category or redacted.
  Raw line text is never exported. Malformed-input reports may export only a
  numeric line number and a fixed error category.
- The allowlist may include only constrained relative file paths, constrained
  executable names, static tool/event/outcome categories, constrained
  workflow/skill/role names, UTC timestamps matching RFC 3339, nonnegative
  finite durations bounded by seven days, and nonnegative local record indexes
  for parent relationships.
- Dropping every unlisted key, including task, note, detail, reason, command
  arguments/full command, result, prompts, model responses, file contents,
  diffs, tool/command output, environment values, artifact names, run IDs, and
  free-text labels.
- Full parse before export; nonzero exit with a line and error category for
  malformed input or unavailable Phoenix.
- No mutation of `.agent-trace`.
- Local Phoenix OSS and a local development-only Docker Compose command.

## Out Of Scope

- SDK, custom UI, hosted service, live tailing, provider-level LLM capture,
  and content opt-in.
- Queueing, retry, or deduplication; re-import may create duplicate Phoenix
  data.

## Architecture Seams

No existing code seam is claimed by this milestone. The approved work is a
private bridge and local Compose workflow described by the child specs; trace
producers remain independent of Phoenix.

## Specs

- `docs/specs/0001-trace-fact-and-privacy-contract.md` (planned)
- `docs/specs/0002-private-otlp-projection-bridge.md` (planned)
- `docs/specs/0003-local-phoenix-observability-workflow.md` (planned)

## Acceptance Criteria

- Every stated fact has a fixture/source map.
- A sentinel privacy test proves excluded values do not export.
- Malformed, unknown, incomplete, and out-of-order trace fixtures are covered.
- A local Phoenix smoke test proves the completed-trace import is inspectable.
- A source hash proves the bridge does not mutate `.agent-trace`.
- Review confirms no Phoenix dependency leaks into trace producers.

## Verification

- Run the child-spec verification commands and fixture suites.
- Run the local Docker Compose/Phoenix smoke test.
- Inspect exported OTLP fields against the explicit allowlist.
- Review the diff and dependency graph for producer isolation.

## Deferred

- Future candidate: parallel-writer coordination changes, to be separately
  proposed if needed; no milestone path exists.

## Open Questions

- None blocking acceptance; child specs define executable contracts.
