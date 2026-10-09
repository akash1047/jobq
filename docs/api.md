# API Reference

> Status: API implementation and request schemas are not yet finalized.

The planned API supports submitting jobs, retrieving job status, and listing jobs. Cancellation semantics will be documented when defined.

## Generated documentation

When the FastAPI application is running with its default documentation settings:

- Swagger UI: http://localhost:8000/docs
- OpenAPI schema: http://localhost:8000/openapi.json

The generated OpenAPI schema is the authoritative reference for implemented endpoints and request/response formats.

## Planned submission fields

| Field | Purpose |
|---|---|
| Job type | Selects a registered handler |
| Payload | Handler-specific JSON input |
| Priority | Controls selection preference |
| Scheduled time | Earliest execution time |
| Maximum attempts | Limits total execution attempts |

Unknown job types and invalid payloads should be rejected before queue insertion.

## Execution semantics

Acceptance means the job has been persisted, not completed.

Jobs may execute more than once. Clients and handlers must account for duplicate execution.

## Access control

Authentication and authorization are not yet specified. Deployment must restrict access until appropriate controls are implemented.
