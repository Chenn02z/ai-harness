# Spec: Private OTLP Projection Bridge

## Status

Accepted

## Goal

Provide a one-shot local bridge that converts the facts in
[`0001-trace-fact-and-privacy-contract.md`](0001-trace-fact-and-privacy-contract.md)
into a binary OTLP trace request for local Phoenix.

## Scenario

A developer supplies one completed current-format `.agent-trace` JSONL file.
The bridge fully validates and projects it, then sends one OTLP
`ExportTraceServiceRequest` to the fixed local endpoint. Invalid input is
reported without making an HTTP request.

## Architecture Reference

See [`docs/ARCHITECTURE.md`](../ARCHITECTURE.md). This is the private bridge
seam in the deferred Local Workflow Observability MVP; trace producers remain
independent of the bridge and Phoenix.

## In Scope

- Python 3.11+ bridge runtime.
- Pinned generated-protobuf dependencies: `protobuf` and
  `opentelemetry-proto`.
- Full validation, fact projection, OTLP serialization, and one HTTP POST.
- Fixed resource, root span, child-span, and attribute contracts.

## Out Of Scope

- OpenTelemetry SDK, OpenTelemetry exporter packages, Phoenix SDK, custom API
  clients, queues, retries, deduplication, live tailing, or hosted endpoints.
- Configurable endpoints, content opt-in, source hashes, or mutation of input.

## Architecture Seams

- **Fact-to-OTLP seam:** only the fixed facts from spec 0001 can reach the
  generated OTLP messages.
- **Transport seam:** generated protobuf serialization is sent with a small
  standard-library HTTP request; no provider client is introduced.

## Contracts

### Runtime and dependencies

The bridge runs on Python 3.11 or newer. `protobuf` and `opentelemetry-proto`
are pinned to exact versions selected and verified during implementation.
The dependency set explicitly excludes the OTel SDK, OTel exporters, and the
Phoenix SDK.

### Input and request

The bridge consumes exactly one path to a JSONL trace and applies spec 0001.
It must fully parse and validate the entire trace before opening an HTTP
connection. It sends exactly one request on success:

- method: `POST`
- URL: `http://127.0.0.1:6006/v1/traces`
- `Content-Type`: `application/x-protobuf`
- body: binary `ExportTraceServiceRequest` serialized by generated protobuf
  classes

There is no endpoint override, environment override, alternate content type,
or custom API client. Phoenix response handling is limited to success/failure
of this request; retries are out of scope.

### OTLP projection

Resource attribute:

| Key | Value |
| --- | --- |
| `openinference.project.name` | `ai-harness-local` |

Create one synthetic root span named `harness.workflow`. Every event span is
a direct child of that root. Event spans use only the fixed static names:
`harness.trace_start`, `harness.trace_end`, `harness.agent_delegation`,
`harness.delegate_result`, `harness.skill_usage`, `harness.context_read`,
`harness.file_edit`, `harness.verification`, `harness.command`,
`harness.handoff`, `harness.duplicated_work`, and
`harness.missing_event`; unknown kinds use `harness.unknown`.

No parent is inferred from record order, record indexes, delegation fields, or
any other source value. Span attributes are exactly the fixed allowlist from
spec 0001. No raw record, forbidden field, source hash, prompt, response,
command text, file content, diff, or arbitrary name is serialized.

### Exit behavior

Success exits zero after the single POST. Malformed, legacy, incomplete,
forbidden-required, or otherwise invalid input exits nonzero before HTTP and
reports only a line number plus fixed error category where applicable.

## Failure Modes

- Missing or unavailable input: nonzero exit and no request.
- Any parse or required-record failure: nonzero exit and no request.
- Phoenix unavailable or non-success response: nonzero exit.
- Serialization failure: nonzero exit and no partial request.
- Re-import: allowed to create duplicate Phoenix data; no deduplication is
  attempted.

## Acceptance Criteria

- A valid completed fixture produces one binary OTLP POST at the fixed URL.
- The request contains the fixed resource attribute, one root, and direct
  child event spans with static names.
- Invalid and legacy fixtures make zero HTTP requests.
- Dependency inspection shows only pinned `protobuf` and
  `opentelemetry-proto` for protobuf/OTLP generation, with no OTel SDK,
  exporter, or Phoenix SDK.
- The input file is byte-for-byte unchanged; no source hash is printed or
  exported.

## Verification

- Run the bridge fixture suite with a local HTTP test server at the fixed
  endpoint.
- Decode the received body using generated protobuf classes and inspect the
  fixed fields, root, children, and resource attribute.
- Run malformed/legacy tests and assert zero requests.
- Run dependency and source-mutation checks.

## Open Questions

- None.
