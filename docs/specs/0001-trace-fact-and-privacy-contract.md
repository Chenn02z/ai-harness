# Spec: Trace Fact and Privacy Contract

## Status

Accepted

## Goal

Define the only facts that a local OTLP projection may read from the current
`.agent-trace` JSONL format and the fixed, content-safe fields it may emit.
`.agent-trace` remains canonical evidence.

## Scenario

A completed current-format trace is read once and converted into facts for the
bridge. The resulting facts describe workflow and execution boundaries without
exporting prompts, responses, command text, file contents, diffs, or other
content-bearing values.

## Architecture Reference

See [`docs/ARCHITECTURE.md`](../ARCHITECTURE.md). This spec implements the
deferred private JSONL-to-OTLP projection boundary without coupling trace
producers to Phoenix.

## In Scope

- Current kind-only records and a complete-trace rule ending in `trace_end`.
- Fixed allowlisted facts and constrained values.
- Privacy rejection and omission rules before any HTTP request.
- Fact fixtures for valid, malformed, legacy, incomplete, unknown, and
  out-of-order input.

## Out Of Scope

- OTLP encoding, HTTP, Phoenix, SDKs, exporters, or Compose.
- Reading or interpreting executable command text.
- Inference of missing facts or parent relationships.

## Architecture Seams

- **Trace producer boundary:** the bridge consumes `.agent-trace` without
  changing its format or requiring producer dependencies.
- **Projection boundary:** facts are the only input available to the OTLP
  bridge; source records are not passed through as arbitrary attributes.

## Contracts

### Input

Each line must be a JSON object in the current kind-only format with a required
string `kind`. A legacy event record containing `event` instead of `kind` is
rejected. The whole input is parsed and validated before any fact is exposed to
HTTP.

Supported kinds are the current workflow evidence kinds:
`trace_start`, `trace_end`, `agent_delegation`, `delegate_result`,
`skill_usage`, `context_read`, `file_edit`, `verification`, `command`,
`handoff`, `duplicated_work`, and `missing_event`. Unknown kinds are retained
only as the static category `unknown`; they never cause arbitrary fields to be
exported.

The input is valid only when it contains one `trace_start`, one final
`trace_end`, and no record after `trace_end`. Required-record failures,
malformed JSON, non-object records, missing `kind`, invalid required values,
and legacy records are rejected before HTTP.

### Read and privacy rules

- Only fields explicitly mapped in the fixed table below may be read.
- Never read or derive from `task`, `note`, `reason`, `detail`, `command`,
  `result`, `artifact`, or `summary`.
- No executable parsing, command tokenization, text extraction, or semantic
  inference is permitted.
- Optional values outside their allowed set are omitted, not coerced or
  redacted into a new value.
- Raw line text is never exported. Malformed-input diagnostics may contain
  only a numeric line number and fixed error category.

### Fixed fact fields

| Fact | Source shape | Allowed output |
| --- | --- | --- |
| `kind` | record `kind` | one of the static kinds above, or `unknown` |
| `timestamp` | RFC 3339 UTC timestamp | valid UTC RFC 3339 string |
| `duration_ms` | numeric duration | nonnegative finite number, at most seven days |
| `path` | path field | safe relative path matching `^[A-Za-z0-9.]`-prefixed segments with each segment `[A-Za-z0-9._-]{1,128}`, no `.` or `..` segment |
| `executable` | executable field | `[A-Za-z0-9][A-Za-z0-9._-]{0,63}` |
| `workflow` | workflow field | `[a-z0-9][a-z0-9._-]{0,63}` |
| `skill` | skill field | `[a-z0-9][a-z0-9._-]{0,63}` |
| `role` | role field | `[a-z0-9][a-z0-9._-]{0,63}` |
| `tool_category` | classified tool field | fixed static category only |
| `event_category` | classified event field | fixed static category only |
| `outcome` | outcome field | fixed static category only |
| `record_index` | local record position | nonnegative integer, used only as a local fact |

The path constraint permits dot-prefixed segments, but rejects `.` and `..`
segments and absolute paths. Test identifiers, parameterized names, arbitrary
names, run IDs, environment values, and all other content-bearing values are
omitted or represented by a fixed category. No source hash is exported or
printed.

### Relationship rule

The fact contract contains no inferred parent relation. Record order and any
source-local index do not establish ancestry; the bridge attaches all event
spans directly under its synthetic fixed root.

## Failure Modes

- Invalid or incomplete input: fail nonzero with line number and fixed error
  category; make no HTTP request.
- Legacy `{event: ...}` input: reject before HTTP.
- Unknown optional values: omit them.
- Unknown kinds: use `unknown` only where the record remains structurally
  valid; never inspect unallowlisted fields.
- Any forbidden value appearing in source: it is neither read nor exported.

## Acceptance Criteria

- Fixtures prove every exported fact has an explicit source map.
- A sentinel fixture proves every forbidden field is absent from output.
- Legacy, malformed, incomplete, unknown, and out-of-order fixtures are
  covered.
- Path, executable, name, timestamp, duration, and index constraints are
  enforced exactly.
- No source hash is exported or printed, and no source record is mutated.

## Verification

- Run the fact parser and privacy fixture suite.
- Inspect serialized facts against the fixed table.
- Hash `.agent-trace` before and after the bridge run in the verification
  harness; the bridge itself must not compute, export, or print that hash.

## Open Questions

- None.
