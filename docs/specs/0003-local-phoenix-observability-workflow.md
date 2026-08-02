# Spec: Local Phoenix Observability Workflow

## Status

Accepted

## Goal

Make the private bridge observable in a one-shot, local Phoenix OSS workflow
without adding deployment infrastructure or a Phoenix integration to trace
producers.

## Scenario

A developer starts Phoenix locally, imports one completed `.agent-trace` with
the bridge, checks the local UI, and confirms the named project, synthetic
root, and fixed event children are inspectable.

## Architecture Reference

See [`docs/ARCHITECTURE.md`](../ARCHITECTURE.md). This spec implements the
development-only local Compose portion of the deferred observability seam and
uses spec 0002 for OTLP transport.

## In Scope

- One pinned Docker Compose Phoenix service.
- Localhost-only port publication.
- Start, smoke, import, and stop workflow for a completed trace.
- Manual UI verification of the projected span hierarchy.

## Out Of Scope

- Production deployment, persistence, database services, volumes, Phoenix
  SDKs, custom API clients, live ingestion, or hosted observability.

## Architecture Seams

- **Local service boundary:** Compose manages only Phoenix for development.
- **Projection boundary:** Phoenix receives only the OTLP projection from spec
  0002; it never becomes a trace producer or canonical evidence store.

## Contracts

### Compose definition

The Compose file has exactly one service using one pinned, tested image:

```yaml
services:
  phoenix:
    image: arizephoenix/phoenix:<VERIFIED_VERSION>
    ports:
      - "127.0.0.1:6006:6006"
```

`<VERIFIED_VERSION>` is a verification-selected placeholder until an image
version has been empirically tested; implementation must replace it with the
exact tested version. The Compose definition has no database, volume, 4317
port, second service, or non-localhost publication.

### Workflow

1. Start the single Phoenix service.
2. Assert the local UI responds `200` at `http://127.0.0.1:6006/`.
3. Run the bridge from spec 0002 against the completed current-format trace.
4. Assert the fixed OTLP endpoint accepts the binary request.
5. Manually inspect the UI for project `ai-harness-local`, root span
   `harness.workflow`, and its direct children with the fixed static names.
6. Stop the service after inspection.

The workflow must use the bridge's fixed endpoint and must not introduce a
custom API client. The local UI check and OTLP acceptance check are separate
smoke assertions.

### Observable output

The only resource project identifier is
`openinference.project.name=ai-harness-local`. The root and event-span names,
attributes, and parentage are exactly those defined by spec 0002. No source
hash is exported or printed.

## Failure Modes

- Phoenix image cannot be pulled or started: smoke fails with a nonzero result.
- UI is not `200`: smoke fails.
- OTLP request is not accepted: smoke fails.
- Bridge rejects the trace: no partial import is reported as success.
- Any non-localhost binding, extra service, volume, database, or 4317 mapping:
  workflow definition fails review.

## Acceptance Criteria

- Compose uses exactly one pinned tested Phoenix image and only
  `127.0.0.1:6006:6006`.
- A smoke command proves the local UI returns `200`.
- A smoke command proves the fixed OTLP endpoint accepts a valid bridge
  request.
- Manual UI verification finds project `ai-harness-local`,
  `harness.workflow`, and all event spans as direct fixed-name children.
- No database, volume, 4317 port, custom API client, or producer-side Phoenix
  dependency exists.

## Verification

- Run the Compose config validation and service startup check.
- Run the UI `200` smoke assertion.
- Run the completed-trace bridge import and OTLP acceptance assertion.
- Perform and record the named-project/root/fixed-children manual UI check.
- Review the Compose diff for image pinning, port scope, and excluded
  services/resources.

## Open Questions

- The exact Phoenix image version remains a verification-selected placeholder
  until empirically tested; no other blocking question remains.
